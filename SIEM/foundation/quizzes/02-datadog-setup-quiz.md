# Quiz 02 — Datadog Setup (40 Q)

> Four sub-quizzes of 10 questions each. Use them across Days 5, 6, 8, and 9.

---

## Section A — Indexing & retention (10 Q)

**1.** Which retention tier is intended for logs needing **long-term retention with occasional urgent queries** (e.g., security and network logs)?
A. Standard Indexing
B. Flex Logs
C. Log Archives
D. Live Tail only

**2.** Which retention tier is the cheapest for logs that are rarely queried?
A. Standard Indexing
B. Flex Logs
C. Log Archives
D. Hot Indexing

**3.** Cloud SIEM detection runs on logs that match the Security Filter:
A. Only if those logs are in a Standard Index
B. Only if those logs are in Flex Logs
C. Regardless of the storage tier
D. Only after the logs are rehydrated from archive

**4.** Which two filter mechanisms exist on a Datadog index?
A. Index Filter (include) and Exclusion Filter
B. Inbound Filter and Outbound Filter
C. Live Filter and Archive Filter
D. Schema Filter and Tag Filter

**5.** An Exclusion Filter takes effect:
A. Before logs are processed by the pipeline
B. Before logs are indexed (so they don't count against the index volume)
C. Only when querying logs
D. Only on archived logs

**6.** When you enable Cloud SIEM, the new dedicated security index must be:
A. Disabled by default
B. Prioritized correctly so security logs route into it instead of generic indexes
C. Stored only in S3
D. Encrypted with a customer key (required)

**7.** Which scanner can redact or hash PII patterns *before* logs are indexed?
A. Cloud Security Posture Management
B. Sensitive Data Scanner
C. Threat Intel Processor
D. Audit Trail

**8.** A pattern-based pre-indexing redaction is best applied via:
A. A custom monitor
B. The Sensitive Data Scanner with predefined rules library or custom regex
C. A Notebook
D. A Reference Table lookup

**9.** Index priority controls:
A. The order of matching when a log is routed to indexes
B. The user permission level
C. Archive frequency
D. Pipeline execution order

**10.** Which is true of the relationship between detection and indexing?
A. Detection requires indexing
B. Detection is independent of indexing once the Security Filter matches
C. Indexing is always cheaper than detection
D. Indexing disables detection

---

## Section B — AWS log collection (10 Q)

**1.** Which AWS service is a **built-in destination** for Datadog logs?
A. AWS Step Functions
B. Amazon Data Firehose (Kinesis Data Firehose)
C. AWS Glue
D. AWS App Runner

**2.** Recommended buffer size when Datadog is the Firehose destination?
A. 256 KiB
B. 1 MiB
C. 2 MiB
D. 5 MiB

**3.** Recommended content encoding for Firehose → Datadog?
A. None
B. ZSTD
C. GZIP
D. Snappy

**4.** Which API key is required when configuring Datadog as a Firehose destination?
A. AWS access key
B. Datadog API key (directly or via AWS Secrets Manager)
C. Anthropic API key
D. Datadog Application Key

**5.** Which delivery method is best when you want a **single AWS account, fastest setup**?
A. Datadog Lambda Forwarder
B. AWS App Mesh
C. Custom EC2 forwarder
D. Manual upload via UI

**6.** Which delivery method best supports fanning out the same log stream to Datadog **and** another consumer?
A. Lambda Forwarder
B. Direct SNS to Datadog
C. Kinesis Data Streams → Firehose → Datadog
D. CloudWatch Subscriptions to email

**7.** Which AWS CLI command subscribes a CloudWatch Log Group to a Firehose / Kinesis destination?
A. `aws cloudwatch put-metric-alarm`
B. `aws logs put-subscription-filter`
C. `aws firehose create-delivery-stream`
D. `aws iam attach-role-policy`

**8.** What is the AWS account ID Datadog assumes role *from* for the standard AWS partition?
A. `123456789012`
B. `464622532012`
C. `065115117704`
D. `987654321000`

**9.** Which AWS-managed policy is attached to the Datadog AWS integration role in addition to a Datadog-generated policy?
A. `AdministratorAccess`
B. `SecurityAudit`
C. `ReadOnlyAccess`
D. `AWSConfigUserAccess`

**10.** Which Terraform resource configures the Datadog side of the AWS integration?
A. `aws_iam_role`
B. `datadog_integration_aws`
C. `datadog_integration_aws_account`
D. `datadog_aws_lambda`

---

## Section C — Pipelines & processors (10 Q)

**1.** Which processor converts unstructured `message` text into structured attributes?
A. Attribute Remapper
B. Grok Parser
C. Date Remapper
D. Status Remapper

**2.** Which reserved attribute requires its own dedicated remapper?
A. `host`
B. `service`
C. `status`
D. All of the above

**3.** Which processor enriches a log with threat intelligence based on an IP attribute matching a Reference Table?
A. Lookup Processor
B. Threat Intel Processor
C. OCSF Processor
D. Trace ID Remapper

**4.** Which processor remaps a log to OCSF event classes?
A. OCSF Processor
B. Reference Table Lookup
C. URL Parser
D. Geo IP Parser

**5.** Where in the pipeline should the OCSF Processor be placed?
A. First
B. Middle
C. **Last**, after all other processors
D. In a separate, parallel pipeline

**6.** The Attribute Remapper's `target_format` value `auto`:
A. Converts to integer always
B. Casts to string always
C. Applies no cast (preserves type)
D. Converts to base64

**7.** A general (non-reserved) Attribute Remapper supports cast types:
A. Only String
B. String, Integer, Double
C. Boolean only
D. Date only

**8.** A Date Remapper sets the value of which reserved attribute?
A. `service`
B. `status`
C. `date`
D. `host`

**9.** When updating an existing integration log pipeline by adding a remapper, you should typically:
A. Enable Preserve Source Attribute
B. Disable Preserve Source Attribute (to avoid duplicate values)
C. Disable the rest of the pipeline
D. Move the new processor to the top

**10.** Which is NOT a typical pipeline processor?
A. Grok Parser
B. Sensitive Data Scanner
C. Anomaly Detection Processor
D. Category Processor

---

## Section D — Cloud SIEM configuration (10 Q)

**1.** What is the API method to create a Cloud SIEM Security Filter?
A. `GET /api/v2/security_monitoring/configuration/security_filters`
B. `POST /api/v2/security_monitoring/configuration/security_filters`
C. `PUT /api/v1/security_filters`
D. `DELETE /api/v2/security_filters`

**2.** Which two header values are required when calling the Security Filter API?
A. `Authorization: Bearer ...`
B. `DD-API-KEY` and `DD-APPLICATION-KEY`
C. `X-Datadog-Service`
D. `DD-Site`

**3.** A Security Filter's `filtered_data_type` for log-based detection is:
A. `metrics`
B. `traces`
C. `logs`
D. `signals`

**4.** Activating Cloud SIEM creates a dedicated:
A. Custom dashboard
B. Detection ruleset only
C. Log index for security data
D. Workflow Automation workspace

**5.** To verify Cloud SIEM ingestion is working, the simplest check is:
A. Open the Security Signals Explorer for incoming signals after a known trigger
B. Check the AWS CloudWatch dashboard
C. Inspect `/var/log/datadog`
D. Run a synthetic test

**6.** When you activate Cloud SIEM, OOTB rules:
A. Must be cloned before they fire
B. Are automatically applied to processed logs
C. Are disabled by default and must be enabled manually
D. Only run after a Bits AI subscription is purchased

**7.** Which of the following is required to use the Datadog Workflow Automation actions in your AWS account from a SOAR blueprint?
A. AWS root credentials in Datadog
B. An IAM role with the right permissions for the action (commonly via Datadog AWS integration role)
C. A Lambda forwarder
D. AWS CodeBuild

**8.** When initial onboarding, the Cloud SIEM "Phase 1: Setup" recommends:
A. Building all custom rules first
B. Configuring log ingestion before activating Content Packs
C. Buying a Workflow Automation seat first
D. Disabling all OOTB rules

**9.** When the Cloud SIEM index is created but security logs still appear in a generic index, the most likely cause is:
A. The pipeline is broken
B. Index priority is incorrect
C. The Security Filter is too broad
D. The retention is set to 0

**10.** A best practice when first enabling Cloud SIEM is to:
A. Enable all custom rules immediately
B. Activate the Content Packs that match log sources you actually ingest
C. Disable Bits AI to start clean
D. Move all logs to Archive

---

## Answer keys

### Section A — Indexing & retention
1-B, 2-C, 3-C, 4-A, 5-B, 6-B, 7-B, 8-B, 9-A, 10-B

### Section B — AWS log collection
1-B, 2-C, 3-C, 4-B, 5-A, 6-C, 7-B, 8-B, 9-B, 10-C

### Section C — Pipelines & processors
1-B, 2-D, 3-B, 4-A, 5-C, 6-C, 7-B, 8-C, 9-B, 10-C

### Section D — Cloud SIEM configuration
1-B, 2-B, 3-C, 4-C, 5-A, 6-B, 7-B, 8-B, 9-B, 10-B

### Highlights / explanations
- *A-Q3* — detection is **not** tied to indexing; it depends on the Security Filter only. This is a heavily tested concept.
- *A-Q5* — Exclusion filters drop logs before they're indexed (saving cost); they don't drop logs from detection unless you set the Security Filter as well.
- *B-Q2/B-Q3* — Memorize 2 MiB + GZIP for Firehose to Datadog. Frequent exam item.
- *B-Q8* — Datadog AWS account: 464622532012 (commercial) / 065115117704 (GovCloud). Memorize at least the commercial one.
- *C-Q5* — The OCSF Processor must be **LAST** so all parsing/enrichment runs before the canonical OCSF mapping is computed.
- *D-Q1/Q3* — POST to `/api/v2/security_monitoring/configuration/security_filters` with `filtered_data_type: logs`.
