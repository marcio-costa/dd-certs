# Adaptive Drill Question Bank — 100 questions tagged by domain & sub-topic

This bank is the source you draw from when running an Adaptive Weak-Area Drill (see `adaptive-protocol.md`). Each question is tagged `[D<domain>:<subtopic>]`. Pick questions whose tags match your weak areas.

Tags used:
- `D1:arch` — architecture / workflow
- `D1:aws-sources` — AWS log sources
- `D1:data-sec` — data security & RBAC
- `D2:index` — indexing/retention strategy
- `D2:collect` — AWS log collection
- `D2:pipelines` — pipelines & processors
- `D2:config` — Cloud SIEM configuration
- `D3:components` — detection rule components
- `D3:types` — detection rule types
- `D3:custom` — OOTB & custom rule mgmt
- `D3:triage` — signal triage / Bits AI / MITRE
- `D4:design` — IR workflow design
- `D4:dash` — dashboards & metrics
- `D4:soar` — workflow automation / SOAR
- `D4:cases` — cases & notifications
- `D5:packs` — content packs
- `D6:frameworks` — compliance frameworks
- `D6:retention` — retention / PCI / RBAC
- `D6:metrics` — audit & metrics
- `D7:ocsf` — OCSF normalization
- `D7:threat-intel` — threat intelligence
- `D7:opt` — operational optimization

## Question bank

**A01 [D1:arch]** What does the Cloud SIEM Security Filter define?
A. The list of indexed events · B. The subset of logs that detection rules can analyze · C. Retention · D. RBAC

**A02 [D1:arch]** Detection runs on logs in which storage tier?
A. Standard only · B. Flex only · C. Archive only · D. Any tier as long as the Security Filter matches

**A03 [D1:arch]** Severity of a Security Signal:
A. P1–P5 · B. Info/Low/Medium/High/Critical · C. Yellow/Orange/Red · D. Tier 0–2

**A04 [D1:arch]** Bits AI labels a signal as:
A. Investigating → Benign or Suspicious · B. Open / Closed · C. Pass / Fail · D. Approved / Rejected

**A05 [D1:arch]** Signal Correlation rules:
A. Combine existing signals into a higher-fidelity signal · B. Replace OOTB · C. Disable detection · D. Delete logs

**A06 [D1:aws-sources]** Best AWS source for IAM API misuse:
A. CloudTrail · B. WAF · C. ELB · D. CloudFront

**A07 [D1:aws-sources]** Best AWS source for east-west outbound flows:
A. CloudTrail · B. WAF · C. VPC Flow Logs · D. EKS audit

**A08 [D1:aws-sources]** Best source for DNS-based C2:
A. VPC Flow · B. Route 53 / DNS query · C. ALB · D. S3

**A09 [D1:aws-sources]** GuardDuty's `source:` value:
A. `aws-guardduty` · B. `guardduty` · C. `gd` · D. `findings`

**A10 [D1:aws-sources]** EKS audit logs `source:`:
A. `eks` · B. `kubernetes.audit` · C. `k8s.audit` · D. `kubernetes`

**A11 [D1:data-sec]** Sensitive Data Scanner runs:
A. After indexing · B. Before indexing for redaction/hashing · C. Only on archive · D. Never

**A12 [D1:data-sec]** PCI logs intake:
A. `agent-http-intake-pci.logs.datadoghq.com:443` · B. Default · C. EU · D. AP1

**A13 [D1:data-sec]** PCI Datadog site:
A. EU1 · B. US1 · C. US3 · D. AP1

**A14 [D1:data-sec]** Audit Trail searchable retention default:
A. 7 days · B. 30 · C. 90 · D. 1 year

**A15 [D1:data-sec]** Default Datadog roles:
A. Owner/Editor/Viewer · B. Administrator/Standard/Read-Only · C. Tier 1/2/3 · D. Power/Auditor/Guest

**A16 [D2:index]** Tier between Standard Indexing and Archive:
A. Live Tail · B. Flex Logs · C. Hot Tier · D. Cold Tier

**A17 [D2:index]** Exclusion filters drop logs:
A. After indexing · B. Before indexing (cost saving) · C. Only on archive · D. Only on detection

**A18 [D2:index]** Detection vs indexing:
A. Detection requires indexing · B. Detection is independent of indexing · C. Indexing disables detection · D. They are the same

**A19 [D2:index]** When Cloud SIEM is enabled, the security index must be:
A. Disabled · B. Prioritized correctly · C. In S3 · D. Encrypted with customer key

**A20 [D2:index]** Cheapest tier for forensic-only logs:
A. Standard · B. Flex · C. Archive · D. Live Tail

**A21 [D2:collect]** Recommended Firehose buffer size to Datadog:
A. 256 KiB · B. 2 MiB · C. 5 MiB · D. 16 MiB

**A22 [D2:collect]** Recommended Firehose encoding to Datadog:
A. None · B. GZIP · C. ZSTD · D. Snappy

**A23 [D2:collect]** Datadog AWS account ID (commercial):
A. 464622532012 · B. 065115117704 · C. 123456789012 · D. 987654321000

**A24 [D2:collect]** AWS-managed policy attached to Datadog integration role:
A. AdministratorAccess · B. SecurityAudit · C. ReadOnlyAccess · D. AWSConfigUserAccess

**A25 [D2:collect]** Best AWS log collection for fan-out to Datadog + another consumer:
A. Lambda Forwarder · B. SNS · C. KDS → Firehose → Datadog · D. CodeDeploy

**A26 [D2:collect]** CLI to subscribe a CloudWatch Log Group to a destination:
A. `aws cloudwatch put-metric-alarm` · B. `aws logs put-subscription-filter` · C. `aws iam attach-role-policy` · D. `aws ssm send-command`

**A27 [D2:pipelines]** OCSF Processor placement:
A. First · B. Last · C. Anywhere · D. Outside the pipeline

**A28 [D2:pipelines]** Reserved attributes that need a dedicated remapper include:
A. host, message, service, status, date, trace_id, span_id · B. user, ip, port · C. severity, tactic · D. None

**A29 [D2:pipelines]** Processor that converts unstructured `message` to attributes:
A. Attribute Remapper · B. Grok Parser · C. Date Remapper · D. Status Remapper

**A30 [D2:pipelines]** Processor for IOC matching against a reference table:
A. Lookup · B. Threat Intel · C. OCSF · D. URL Parser

**A31 [D2:pipelines]** Cast types supported by the generic Attribute Remapper:
A. Only String · B. String/Integer/Double · C. Boolean only · D. Date only

**A32 [D2:config]** API endpoint to create a Security Filter:
A. POST `/api/v2/security_monitoring/configuration/security_filters` · B. GET `/api/v1/security_filters` · C. PUT `/api/v1/log_filters` · D. DELETE `/api/v2/security_filters`

**A33 [D2:config]** Headers required for the Security Filter API:
A. Authorization Bearer · B. DD-API-KEY + DD-APPLICATION-KEY · C. X-Datadog-Service · D. DD-Site only

**A34 [D2:config]** `filtered_data_type` for log-based detection:
A. metrics · B. traces · C. logs · D. signals

**A35 [D2:config]** When SIEM index is created but logs still go to a generic index, fix:
A. Re-prioritize indexes · B. Restart Agent · C. Recreate AWS account · D. Change region

**A36 [D3:components]** A "rule case" lets one rule:
A. Output different severities depending on counts · B. Be disabled · C. Skip indexing · D. Run once a day

**A37 [D3:components]** Suppression conditions:
A. Disable the rule · B. Prevent the rule from firing for specific entities · C. Increase severity · D. Change playbook only

**A38 [D3:components]** A notification template variable for the user is:
A. `{{user_id}}` · B. `{{@usr.id}}` · C. `${USER_ID}` · D. `[user]`

**A39 [D3:types]** New Value rule fires when:
A. A previously-unseen value appears for an attribute
B. A pre-existing value reappears
C. The rule is cloned
D. Bits AI is offline

**A40 [D3:types]** Impossible Travel rule:
A. Same user logs in from too-distant geographies in too-short window · B. Long traceroutes · C. TZ mismatch · D. Static IPs

**A41 [D3:types]** Anomaly rule:
A. Statistical baseline deviation · B. Static count threshold · C. New value · D. Correlation

**A42 [D3:types]** Third Party rule "trigger for each new" roll-up:
A. 1 hour · B. 24 hours · C. 7 days · D. 30 days

**A43 [D3:types]** Cloud Configuration rules belong to:
A. Cloud SIEM · B. CSPM · C. APM · D. RUM

**A44 [D3:custom]** Cloning an OOTB rule:
A. Creates an editable copy preserving structure · B. Deletes the original · C. Raises severity · D. Disables the rule

**A45 [D3:custom]** A noisy rule's first remediation:
A. Suppress / tighten thresholds · B. Disable Cloud SIEM · C. Promote to Critical · D. Notify CFO

**A46 [D3:custom]** A custom rule that produces 0 signals in 30 days is most likely:
A. Properly tuned · B. Mis-scoped or query-wrong · C. Working as intended for a rare event · D. Either B or C — investigate

**A47 [D3:triage]** Investigator is:
A. A graphical pivot from a signal to related entities · B. CSPM dashboard · C. Billing console · D. Detection editor

**A48 [D3:triage]** Bits AI side-panel actions include all EXCEPT:
A. Create Case · B. Run SOAR blueprint · C. Add suppression · D. Drop indexed logs

**A49 [D3:triage]** MITRE ATT&CK Map is best for:
A. Strategic gap analysis of detection coverage · B. Cost optimization · C. AWS account mgmt · D. RBAC

**A50 [D3:triage]** Permission to read SIEM signals:
A. `dashboards_read` · B. `security_monitoring_signals_read` · C. `apm_read` · D. `metrics_read`

**A51 [D4:design]** A Case in Datadog:
A. Aggregates signals, evidence, comments, assignees · B. Replaces detection rules · C. Replaces dashboards · D. Holds archives

**A52 [D4:design]** A typical end-to-end SOC flow:
A. Logs → Index → Notebook · B. Signal → Notification → Investigator/Bits AI → Case → SOAR → Detection improvement · C. Audit Trail → SOAR · D. Dashboard → Customer Support

**A53 [D4:design]** Bits AI replies in Slack/Jira notifications by:
A. In-thread reply with conclusion + link · B. Deletes the message · C. Sends a new email · D. Opens a dashboard

**A54 [D4:dash]** A standard SOC KPI:
A. MTTR for security signals · B. CPU saturation · C. RUM session length · D. Synthetic test count

**A55 [D4:dash]** Logs-to-Metrics use case:
A. Build trend metrics from log activity · B. Replace dashboards · C. Disable Cloud SIEM · D. Encrypt logs

**A56 [D4:dash]** "Top users by signal count" widget:
A. Time-series · B. Top List · C. Heatmap · D. Geomap

**A57 [D4:dash]** SLOs for SIEM ops include:
A. % HIGH+ closed within X hours · B. Number of dashboards · C. Number of integrations · D. Number of pipelines

**A58 [D4:soar]** A workflow trigger that lets it run from a security signal is:
A. Security · B. API · C. Schedule · D. Agent

**A59 [D4:soar]** A workflow execution returns:
A. `execution_id` · B. `signal_uuid` · C. `dashboard_id` · D. `tag_id`

**A60 [D4:soar]** Which is NOT a typical SOAR blueprint?
A. Block IP at WAF · B. Disable IAM user · C. Drop indexed logs from index · D. Lookup IOC

**A61 [D4:soar]** Workflow listing API filters by:
A. Name, tags, owner, handle, trigger type · B. Only ID · C. Only severity · D. None

**A62 [D4:cases]** Case statuses:
A. To Do/In Progress/Closed · B. New/Acknowledged · C. Active/Inactive · D. Pass/Fail

**A63 [D4:cases]** Case priorities:
A. P1-P5 with P1 highest · B. P1-P5 with P5 highest · C. Fixed · D. None

**A64 [D4:cases]** Cases bidirectionally sync with:
A. Jira / ServiceNow · B. CloudWatch · C. Lambda · D. Synthetics

**A65 [D4:cases]** Which channel is NOT a typical notification destination?
A. Slack · B. PagerDuty · C. Jira · D. AWS Backup

**A66 [D5:packs]** A Content Pack typically includes:
A. Detection rules + dashboard + Investigator preset + SOAR blueprints + parser + config guide · B. Detection rules only · C. Dashboards only · D. CloudFormation only

**A67 [D5:packs]** Activating a Content Pack does NOT automatically:
A. Configure the upstream AWS service · B. Enable the parser · C. Enable OOTB rules · D. Enable the dashboard

**A68 [D5:packs]** New value rule on freshly-activated pack appears noisy because:
A. Building 24h baseline · B. Pipeline broken · C. AWS WAF offline · D. Misconfig

**A69 [D5:packs]** Best practice for customizing the Content Pack dashboard:
A. Edit OOTB directly · B. Clone and edit clone · C. Disable pack · D. Delete OOTB

**A70 [D5:packs]** Symptom: pack activated, no signals; first check:
A. Logs actually arriving with right `source:` · B. Datadog Agent version · C. Lambda concurrency · D. Browser cache

**A71 [D6:frameworks]** Cloud SIEM compliance role:
A. Generates runtime/log-based detection evidence · B. Replaces CSPM · C. Configures AWS · D. Replaces AWS Config

**A72 [D6:frameworks]** PCI/HIPAA/SOC2/CIS configuration mapping comes from:
A. Cloud SIEM · B. CSPM · C. Workload Protection · D. RUM

**A73 [D6:frameworks]** Detection rule tag for framework grouping:
A. `compliance:pci-dss` · B. `aws:iam` · C. `severity:high` · D. `team:soc`

**A74 [D6:frameworks]** Audit Trail captures:
A. Datadog admin actions · B. AWS CloudTrail · C. APM errors · D. Synthetics results

**A75 [D6:frameworks]** Audit Trail long-term retention is via:
A. Long-term retention/archive · B. Standard Indexing · C. RUM Sessions · D. APM traces

**A76 [D6:retention]** PCI Datadog site:
A. EU1 · B. US1 · C. AP1 · D. US3

**A77 [D6:retention]** PCI APM endpoint:
A. `https://trace-pci.agent.datadoghq.com` · B. `app.datadoghq.com` · C. `api.datadoghq.com` · D. Default

**A78 [D6:retention]** Audit Trail in PCI orgs:
A. Auto-enabled and must remain enabled · B. Optional · C. Disabled · D. One-time

**A79 [D6:retention]** OEM threat-intel persistence:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**A80 [D6:retention]** BYOTI persistence:
A. 24 hours · B. 30 days · C. As long as the reference table exists · D. Forever

**A81 [D6:retention]** Permission to read log data from a specific index:
A. `logs_read_index_data` · B. `dashboards_read` · C. `apm_read` · D. `metrics_read`

**A82 [D6:retention]** RBAC best practice:
A. Roles for permissions; map users to roles · B. Per-user permissions · C. No RBAC · D. Admin to all

**A83 [D6:metrics]** Operational metric for SIEM team:
A. MTTR · B. CPU usage · C. Disk IOPS · D. RUM session length

**A84 [D6:metrics]** Noise rate per rule:
A. False positives / total · B. MTTA / MTTR · C. CPU / RAM · D. Logs / signals

**A85 [D6:metrics]** Sudden drop in detection volume usually means:
A. Broken pipeline · B. Improved security · C. Holiday quiet · D. Bits AI upgrade

**A86 [D6:metrics]** Cost dial for SIEM ingest:
A. Daily log GB into security index · B. Number of dashboards · C. Number of users · D. Number of monitors

**A87 [D7:ocsf]** OCSF stands for:
A. Open Cybersecurity Schema Framework · B. Optional Cloud Security Forum · C. Outbound Cyber Standard Format · D. Operational Cyber Security Foundation

**A88 [D7:ocsf]** OCSF Processor must be placed:
A. First · B. Last · C. Anywhere · D. Outside

**A89 [D7:ocsf]** Which is NOT a typical OCSF event class?
A. Authentication · B. API Activity · C. Network Activity · D. Container Restart

**A90 [D7:ocsf]** Multiple OCSF mappings in one processor:
A. Not supported · B. Supported · C. API only · D. Always required

**A91 [D7:ocsf]** Library mappings cover (best):
A. AWS, GCP, M365 Defender, Okta, Palo Alto, Zscaler ZPA, GitHub, Workspace, Infoblox · B. Only AWS · C. Only Microsoft · D. Only firewalls

**A92 [D7:threat-intel]** Threat Intel Processor required input:
A. IoC key · B. Slack channel · C. AWS region · D. DB string

**A93 [D7:threat-intel]** BYOTI uses which Datadog object?
A. Reference Table · B. Custom Metric · C. Notebook · D. Synthetics

**A94 [D7:threat-intel]** OEM threat-intel persists:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**A95 [D7:threat-intel]** BYOTI persists:
A. 24 hours · B. As long as the reference table exists · C. 30 days · D. Forever

**A96 [D7:opt]** Cost dial for **detection** volume:
A. Security Filter · B. Index Filter · C. Exclusion Filter · D. Live Tail

**A97 [D7:opt]** Cost dial for **indexing** volume:
A. Security Filter · B. Exclusion Filter · C. Live Tail · D. Notification rule

**A98 [D7:opt]** Pipeline-hygiene best practice:
A. One source = one pipeline; clone OOTB if changes needed · B. One giant pipeline · C. Disable OOTB · D. Skip parsing

**A99 [D7:opt]** Quarterly hygiene work:
A. Enable new OOTB rules; tune/deprecate the bottom decile by noise · B. Disable everything · C. Delete dashboards · D. Stop ingest

**A100 [D7:opt]** Suppression registry:
A. Tracks all suppressions and rationale · B. Bills customers · C. Implements OCSF · D. Manages dashboards

---

## Answer key

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 21 | B | 41 | A | 61 | A | 81 | A |
| 2 | D | 22 | B | 42 | B | 62 | A | 82 | A |
| 3 | B | 23 | A | 43 | B | 63 | A | 83 | A |
| 4 | A | 24 | B | 44 | A | 64 | A | 84 | A |
| 5 | A | 25 | C | 45 | A | 65 | D | 85 | A |
| 6 | A | 26 | B | 46 | D | 66 | A | 86 | A |
| 7 | C | 27 | B | 47 | A | 67 | A | 87 | A |
| 8 | B | 28 | A | 48 | D | 68 | A | 88 | B |
| 9 | B | 29 | B | 49 | A | 69 | B | 89 | D |
| 10 | B | 30 | B | 50 | B | 70 | A | 90 | B |
| 11 | B | 31 | B | 51 | A | 71 | A | 91 | A |
| 12 | A | 32 | A | 52 | B | 72 | B | 92 | A |
| 13 | B | 33 | B | 53 | A | 73 | A | 93 | A |
| 14 | C | 34 | C | 54 | A | 74 | A | 94 | A |
| 15 | B | 35 | A | 55 | A | 75 | A | 95 | B |
| 16 | B | 36 | A | 56 | B | 76 | B | 96 | A |
| 17 | B | 37 | B | 57 | A | 77 | A | 97 | B |
| 18 | B | 38 | B | 58 | A | 78 | A | 98 | A |
| 19 | B | 39 | A | 59 | A | 79 | A | 99 | A |
| 20 | C | 40 | A | 60 | C | 80 | C | 100 | A |
