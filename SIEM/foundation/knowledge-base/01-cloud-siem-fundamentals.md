# Domain 1 – Cloud SIEM Fundamentals for AWS

> Sources: Datadog Cloud SIEM Overview, Getting Started with Cloud SIEM, AWS Configuration Guide for Cloud SIEM, Cloud SIEM Data Security, Datadog Security product page.

## 1.1 What is Datadog Cloud SIEM?

Datadog Cloud SIEM is a log-based threat detection product that runs entirely on the same Datadog Log Management platform as everything else in Datadog. It analyzes ingested logs in **real time** against detection rules and produces **Security Signals** that engineers triage in the **Security Signals Explorer**.

Key product properties:
- **No separate cluster to deploy** — Cloud SIEM runs on the same SaaS platform as Datadog Logs, APM and Infrastructure.
- **Same log pipelines feed the SIEM** as feed the rest of the platform; you only need to make sure log sources are ingested and parsed.
- Detection runs against **all logs that pass the security filter**, regardless of whether they are indexed for search.
- Signals carry **MITRE ATT&CK tactic/technique** annotations and severity (Info, Low, Medium, High, Critical).
- Datadog AI ("Bits AI Security Analyst") can pre-investigate signals and mark them Benign or Suspicious.

## 1.2 Cloud SIEM architecture and workflow

The end-to-end flow:

```
AWS account ─► (CloudTrail / GuardDuty / VPC FL / WAF / Route 53 / ALB / S3) 
            │
            ├── Lambda Forwarder ─────────────┐
            │                                 │
            └── Amazon Data Firehose ─────────┤
                                              ▼
                              Datadog log intake (region-specific)
                                              │
                                              ▼
                                Log Pipelines (parsing, enrichment, OCSF remap)
                                              │
                                              ├──► Index (Standard / Flex / Archive)
                                              │
                                              ▼
                                Security Filter (logs eligible for SIEM analysis)
                                              │
                                              ▼
                                Detection Rules (OOTB + custom)
                                              │
                                              ▼
                                Security Signals → Investigator / Cases / Workflows / Bits AI
```

**Key concepts:**
- **Security Filter:** a query that defines which subset of all ingested logs is analyzed by Cloud SIEM. By default, logs that match `source:<aws-service>` are included.
- **Cloud SIEM Index:** when you enable Cloud SIEM, Datadog creates a dedicated security index that must be **prioritized correctly** in the index list; otherwise security logs get indexed into a generic index and sampling/exclusion may apply.
- **Detection Rule:** rule that runs continuously over the security filter's stream and fires a Signal when matched.
- **Security Signal:** the alert object. Has severity, status (Open/Under Review/Archived), MITRE mapping, evidence logs, and a playbook field.

## 1.3 Log Ingestion and Management

Cloud SIEM consumes the same log pipelines as Datadog Log Management. You can collect AWS logs by:

| Method | Best when | Notes |
|---|---|---|
| **Datadog Lambda Forwarder** | You want a fast, single-account setup or need transformation in Lambda. | Push-based; the Lambda subscribes to S3, CloudWatch Logs, or SNS and forwards to Datadog. |
| **Amazon Data Firehose with Datadog destination** | High-volume, multi-account, prefer AWS-native delivery, want to also fan out logs to other consumers. | Datadog is a built-in Firehose destination. Recommended buffer size: **2 MiB**, content encoding **GZIP**. Authentication uses Datadog API key (direct or via AWS Secrets Manager). |
| **Amazon Kinesis Data Streams → Firehose → Datadog** | You want to multi-cast the same logs to Datadog and another destination (S3, third-party SIEM). | Kinesis Data Stream as Firehose source. |
| **Datadog Agent log collection** | Logs from EC2/ECS/EKS hosts. | Agent ships logs over HTTPS. |

For CloudWatch Logs subscriptions to Firehose:
```bash
aws logs put-subscription-filter \
  --log-group-name "<MYLOGGROUPNAME>" \
  --filter-name "<MyFilterName>" \
  --filter-pattern "" \
  --destination-arn "<DESTINATIONARN>" \
  --role-arn "<MYROLEARN>"
```

## 1.4 AWS Security Foundations – the log sources you must know

The exam expects you to understand **what each AWS log source is good for** in a SIEM context:

| AWS source | Detection use cases | `source:` value in Datadog |
|---|---|---|
| **CloudTrail** (management + data events) | IAM abuse, console logins from new IPs, role assume chains, credential exfiltration, S3 policy changes | `source:cloudtrail` |
| **GuardDuty findings** | AWS-native threat findings (port scans, crypto-mining, exfiltration heuristics) | `source:guardduty` |
| **VPC Flow Logs** | East-west traffic, suspicious outbound flows, exfiltration to known-bad IPs | `source:vpc` |
| **AWS WAF** | Web attacks at the edge, bot abuse, OWASP Top 10 hits | `source:waf` |
| **Route 53 Resolver / Query logs** | DNS-based command-and-control, DGA detection | `source:route53` |
| **ALB / ELB / CloudFront** | App-layer recon, brute force, abnormal user agents | `source:alb` / `source:elb` / `source:cloudfront` |
| **S3 access logs** | Sensitive bucket reads, anonymous access | `source:s3` |
| **Amazon EKS / Kubernetes audit logs** | Kube API misuse, suspicious exec, RBAC changes | `source:kubernetes.audit` |

Default security filter pattern that Cloud SIEM uses for AWS:
```
source:(cloudtrail OR guardduty OR route53)
source:(vpc OR waf OR elb OR alb)
```

## 1.5 Where Cloud SIEM stops and other Datadog products begin

Out-of-scope for SIEM (handled by other products, but candidates often confuse them):
- **Cloud Security Misconfigurations / CSPM** — scans cloud *configuration*, not logs.
- **Cloud Security Identity Risks (CIEM)** — IAM-based identity risk.
- **Workload Protection** — Datadog Agent runtime detections on hosts.
- **App and API Protection (AAP)** — runtime protection of apps, uses APM traces.

The exam guide explicitly excludes APM/INFRA from scope.

## 1.6 Data Security in Cloud SIEM

- All log data is encrypted in transit (TLS 1.2+) and at rest.
- **Sensitive Data Scanner** can be applied to logs *before* indexing or detection, masking/hashing PII/PCI.
- **Security Filter** acts as a hard cut-off: only logs matching the filter are ever inspected by detection rules. This is also the cost-control surface — narrowing it reduces SIEM ingest volume.
- Cloud SIEM honors **RBAC** through `security_monitoring_*` permissions (read signals, manage rules, manage filters).
- For PCI-compliant orgs, log endpoints must use the dedicated PCI intake (`agent-http-intake-pci.logs.datadoghq.com:443`) and Audit Trail must be enabled and remain enabled.
