# Domain 2 – Datadog Setup (Log Management & AWS Integration)

> Sources: AWS Configuration Guide for Cloud SIEM, Send AWS Services Logs with the Datadog Amazon Data Firehose Destination, Amazon Kinesis Data Firehose, Log Pipelines docs, Flex Logs / Standard Indexing / Archives docs.

## 2.1 Log Management and Indexing Strategy

### Index, Flex, Archive – three retention tiers

| Tier | Use case | Hot/warm | Cost driver |
|---|---|---|---|
| **Standard Indexing** | Frequent, short-term querying. The default. | Hot | Indexed events |
| **Flex Logs** | Long-term retention with **occasional urgent** queries (security/transaction/network). Sits between Standard and Archive. | Warm | Stored events |
| **Log Archives** | Infrequent, long-term retention to S3/Azure Blob/GCS. Rehydrate to query. | Cold | Storage + rehydrate |

**Important for the exam:** Cloud SIEM detection runs against logs that match the **Security Filter** *regardless* of whether they are indexed. You can keep raw logs in Flex/Archive but still detect on them. Indexing matters for **search**, not for **detection**.

### Index filters and exclusion filters
- **Index Filter (`include`):** narrows which logs go into a given index.
- **Exclusion Filter:** drops or samples a percentage of logs **before indexing** (saves cost). 
- For sensitive data, audit both filter sets — exclusion filters may unintentionally hide compliance-relevant logs from Standard indexes.
- For Cloud SIEM, ensure the dedicated security index is **prioritized above** generic catch-all indexes so security logs are routed correctly.

### Sensitive data
- **Sensitive Data Scanner** scans logs and can redact/hash PII, PCI, custom regex patterns *before* indexing.
- For cloud storage scanning, only the **location** of a match is sent to Datadog by the scanner — not the secret itself.

## 2.2 AWS Integration and Log Collection

### Two officially supported delivery methods

#### A. Datadog Lambda Forwarder
- One Lambda function in your AWS account.
- Subscribes to S3 buckets, CloudWatch Log Groups, and SNS topics.
- Pushes logs to the Datadog HTTP intake.
- Easy to deploy (CloudFormation template). Common for smaller / single-account environments.

#### B. Amazon Data Firehose with Datadog destination (recommended for scale)
Datadog is a **built-in destination** in Amazon Data Firehose. To use it:

1. Create a Firehose delivery stream.
2. **Source:** "Direct PUT" (logs straight from a CloudWatch subscription) **or** "Amazon Kinesis Data Streams" (when you want to fan out the same stream to multiple consumers).
3. **Destination:** Datadog. Provide the Datadog API key directly **or via AWS Secrets Manager**.
4. **Buffer hints (recommended):** **2 MiB**, **GZIP** content encoding for single-line records.
5. Subscribe CloudWatch Log Groups to the Firehose:
   ```bash
   aws logs put-subscription-filter \
     --log-group-name "<MYLOGGROUPNAME>" \
     --filter-name "<MyFilterName>" \
     --filter-pattern "" \
     --destination-arn "<FIREHOSE_OR_KDS_ARN>" \
     --role-arn "<MYROLEARN>"
   ```
6. Verify logs arrive in the Datadog Logs Explorer with the appropriate `source:` and `service:` tags.

### When to use which
| Scenario | Recommended |
|---|---|
| Single AWS account, low volume, want fastest setup | Lambda Forwarder |
| Multi-account, high volume, want AWS-native delivery | Firehose with Datadog destination |
| You need to send the same log stream to Datadog **and** another destination (S3, Splunk, etc.) | Kinesis Data Stream → Firehose → Datadog (KDS allows multiple consumers) |
| Logs are S3 events (e.g., CloudTrail, S3 access logs) | Lambda Forwarder triggered on S3 |

### AWS IAM integration
The Datadog AWS integration uses an **IAM role** with `sts:AssumeRole` from Datadog's account ID (`464622532012` for AWS standard, `065115117704` for GovCloud), an **External ID** for trust, the AWS-managed `SecurityAudit` policy, and a custom Datadog policy generated from the IAM-permissions data source. The Terraform `datadog_integration_aws_account` resource models the full setup.

## 2.3 Log Processing and Normalization

Datadog log pipelines apply ordered processors to every log:

| Processor | Purpose |
|---|---|
| **Grok Parser** | Convert unstructured `message` text into structured attributes. Supports `match_rules` and `support_rules`. |
| **Date Remapper** | Map an attribute to the reserved `date` field (timestamp). |
| **Status Remapper** | Map a field to the reserved `status` (severity) attribute. |
| **Service Remapper** | Map a field to the reserved `service` attribute. |
| **Attribute Remapper** | Generic source-to-target attribute or tag remap. Can cast type (`String`, `Integer`, `Double`). Cannot remap reserved attributes (use the dedicated remappers). |
| **Category Processor** | Bucket logs by attribute pattern. |
| **Lookup Processor** | Enrich with a static table. |
| **Reference Table Lookup** | Enrich with a managed reference table (e.g., asset inventory, threat intel IOC list). |
| **Pipeline Lookup / Branch** | Conditional logic. |
| **Sensitive Data Scanner** | Redact/hash sensitive content. |
| **OCSF Processor** | Remap to OCSF schema event classes. **Place last** in the pipeline so other processors run first. |
| **Threat Intel Processor** | Match a log attribute (e.g., IP) against a reference table of IOCs and enrich the log with `threat_intel.*` attributes. |

### Reserved (special) attributes
`host`, `message`, `service`, `status`, `date`, `trace_id`, `span_id` — cannot be remapped via the generic Attribute Remapper. Use the dedicated remapper for each.

## 2.4 Cloud SIEM Configuration and Verification

To enable Cloud SIEM:
1. Open Security → Cloud SIEM → Get Started.
2. Select log sources to analyze (this builds the **Security Filter**).
3. Activate Cloud SIEM. Datadog creates a dedicated security log index.
4. Verify index priority – the new SIEM index must be above generic indexes that would catch the same logs.
5. Activate **Content Packs** for the AWS log sources you ingest (CloudTrail, GuardDuty, VPC Flow Logs, etc.). Content Packs include detection rules, dashboards, Investigator presets and SOAR workflows.
6. Verify in **Security Signals Explorer** that signals start to appear (use a known-trigger event such as a console login from a new geography to test).

### Setting up Security Filters via API
```bash
curl -L -X POST 'https://api.datadoghq.com/api/v2/security_monitoring/configuration/security_filters' \
  --header 'Content-Type: application/json' \
  --header 'DD-API-KEY: <DD_API_KEY>' \
  --header 'DD-APPLICATION-KEY: <DD_APP_KEY>' \
  --data-raw '{
    "data": {
      "type": "security_filters",
      "attributes": {
        "is_enabled": true,
        "name": "cloudtrail",
        "exclusion_filters": [],
        "filtered_data_type": "logs",
        "query": "source:cloudtrail"
      }
    }
  }'
```

### Verification checklist
- Logs arrive in Logs Explorer with the right `source:` tag.
- Reserved attributes (`@usr.id`, `@network.client.ip`, `@evt.name`) are populated by the integration pipeline.
- Cloud SIEM dedicated index is enabled and prioritized.
- OOTB detection rules show `Enabled: true`.
- Security Signals Explorer is generating new signals from a representative test action.
