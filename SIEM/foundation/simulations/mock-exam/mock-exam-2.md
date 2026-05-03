# Full Mock Exam #2 — 75 questions / 120 minutes

> Use on **Day 27** of the study plan, after the Adaptive Weak-Area Drill. This Mock is harder and more scenario-based than Mock #1.
>
> Same domain distribution as Mock #1.

---

## Section 1 — Cloud SIEM Fundamentals (12 Q)

**1.** A SOC analyst opens a high-severity signal that says "AWS root user used in 3 separate API calls within 10 minutes". The most likely AWS source feeding this rule is:
A. VPC Flow Logs · B. CloudTrail · C. WAF · D. EKS audit

**2.** Which Cloud SIEM concept is the *cost-control surface for detection*?
A. Index Filter · B. Exclusion Filter · C. Security Filter · D. Notification Rule

**3.** A team raised the concern: "we want to detect on these logs but only retain them in Archive for cost." The right answer is:
A. Detection cannot run on Archive — you must index
B. Detection runs on logs that match the Security Filter regardless of storage tier
C. Move them to Standard Indexing
D. Disable detection

**4.** A new Cloud SIEM index has been created but security logs continue landing in a generic catch-all index. The most likely cause is:
A. The Datadog Agent is out of date
B. Index priority has the catch-all matching first
C. AWS Lambda concurrency
D. Browser cache

**5.** Which AWS log source most directly surfaces anonymous reads of an S3 bucket?
A. CloudTrail data events / S3 access logs
B. VPC Flow Logs
C. WAF logs
D. EKS audit

**6.** EKS / Kubernetes audit log `source:` value:
A. `kubernetes.audit` · B. `eks` · C. `k8s.audit` · D. `kubernetes`

**7.** Which Datadog product family uses the Datadog Agent at host runtime to detect threats (not Cloud SIEM)?
A. CSPM · B. AAP · C. Workload Protection · D. Sensitive Data Scanner

**8.** A signal's MITRE annotation is:
A. Tactic + Technique · B. CVE · C. Severity only · D. Customer name

**9.** A signal with `severity:CRITICAL` should typically:
A. Trigger a Notification Rule that pages the on-call team
B. Be archived automatically
C. Be ignored
D. Be turned into a dashboard

**10.** Bits AI Security Analyst displays which intermediate label?
A. Investigating · B. Pending · C. Open · D. Triage

**11.** Which Datadog-side step happens automatically when Cloud SIEM is enabled?
A. A dedicated security log index is created
B. AWS CloudTrail is configured for you
C. A free Datadog Agent is shipped
D. PCI is enabled

**12.** Which AWS source family is usually first in a default Cloud SIEM Security Filter for AWS?
A. cloudtrail / guardduty / route53
B. s3 / dynamodb / lambda
C. ec2 / ebs / efs
D. cloudfront / api-gateway

---

## Section 2 — Datadog Setup (16 Q)

**13.** A team has 2 TB/day of low-value debug logs they only need for forensic queries. Best tier:
A. Standard Indexing · B. Flex Logs · C. Log Archives · D. Live Tail

**14.** Which Datadog feature redacts PII pre-indexing?
A. Audit Trail · B. Sensitive Data Scanner · C. Workflow Automation · D. Bits AI

**15.** When Datadog is the Firehose destination, the recommended buffer / encoding is:
A. 256 KiB / Snappy · B. 2 MiB / GZIP · C. 5 MiB / ZSTD · D. 16 MiB / None

**16.** Datadog's commercial AWS account ID for `sts:AssumeRole` is:
A. 464622532012 · B. 065115117704 · C. 123456789012 · D. 987654321000

**17.** AWS-managed policy attached to the Datadog AWS integration role:
A. AdministratorAccess · B. SecurityAudit · C. ReadOnlyAccess · D. AWSConfigUserAccess

**18.** Best AWS log delivery method for a single account, fastest setup:
A. Lambda Forwarder
B. Amazon Data Firehose with Datadog destination
C. Manual upload
D. SNS

**19.** Best AWS log delivery for fan-out to Datadog AND another consumer:
A. Lambda Forwarder
B. Direct SNS to Datadog
C. KDS → Firehose → Datadog
D. CodeDeploy

**20.** AWS CLI to subscribe a CloudWatch Log Group to Firehose/KDS:
A. `aws cloudwatch put-metric-alarm`
B. `aws logs put-subscription-filter`
C. `aws iam attach-role-policy`
D. `aws ssm send-command`

**21.** The OCSF Processor should be placed:
A. First · B. Last · C. Anywhere · D. In a separate pipeline

**22.** Generic Attribute Remapper supported `target_format` values:
A. only `string` · B. `auto`, `string`, `integer` · C. only `boolean` · D. only `date`

**23.** A Grok parser config requires:
A. `match_rules` and optional `support_rules`
B. Just regex
C. Just `samples`
D. Nothing

**24.** Reserved attributes that need a dedicated remapper include:
A. host, message, service, status, date, trace_id, span_id
B. user, ip, port
C. severity, tactic
D. None

**25.** When updating an existing integration pipeline by adding an Attribute Remapper, you typically:
A. Enable Preserve Source Attribute
B. Disable Preserve Source Attribute (avoid duplicate values)
C. Disable other processors
D. Move to position 0

**26.** API endpoint to create a Cloud SIEM Security Filter:
A. POST `/api/v2/security_monitoring/configuration/security_filters`
B. GET `/api/v1/security_filters`
C. PUT `/api/v1/log_filters`
D. DELETE `/api/v2/security_filters`

**27.** A Security Filter `filtered_data_type` for log-based detection is:
A. metrics · B. traces · C. logs · D. signals

**28.** Headers required for the Security Filter API:
A. Authorization Bearer
B. DD-API-KEY + DD-APPLICATION-KEY
C. X-Datadog-Service
D. DD-Site only

---

## Section 3 — Detection & Threat Detection (16 Q)

**29.** Which is NOT a standard component of a detection rule?
A. Query · B. Group by · C. Trace ID · D. Severity

**30.** Rule cases:
A. Output different severities depending on aggregated counts
B. Replace OOTB rules
C. Disable indexing
D. Trigger only once a day

**31.** A New Value rule:
A. Fires when a previously-unseen value appears for an attribute
B. Fires on a static threshold breach
C. Combines two signals
D. Replaces OOTB

**32.** Impossible Travel:
A. Same user logs in from too-distant geos in too-short window
B. Long traceroutes
C. Time-zone mismatch only
D. Static IP allowlists

**33.** Anomaly:
A. Statistical baseline deviation
B. Static threshold count
C. New value
D. Correlation

**34.** Cloud Configuration / Infrastructure Configuration rules belong to:
A. Cloud SIEM · B. CSPM · C. APM · D. RUM

**35.** Third Party rule "trigger for each new" attribute uses what window?
A. 1 hour · B. 24 hours · C. 7 days · D. 30 days

**36.** Cloning an OOTB rule:
A. Creates an editable copy preserving structure & MITRE annotations
B. Deletes the original
C. Always promotes severity
D. Disables Cloud SIEM

**37.** Suppression conditions:
A. Disable rule
B. Prevent firing for known-good entities
C. Force notifications to one channel
D. Replace MITRE annotations

**38.** A noisy custom rule that produces 500 signals/day:
A. Disable it
B. Tighten thresholds, add suppression, re-scope query
C. Promote to Critical
D. Notify CFO

**39.** A custom rule with 0 signals in 30 days:
A. Properly tuned
B. Mis-scoped or query-wrong
C. Working as intended for a rare event
D. Either B or C — investigate

**40.** Investigator:
A. Graphical pivot from a signal to all related entities
B. CSPM dashboard
C. Billing console
D. Detection editor

**41.** Bits AI side-panel actions include all EXCEPT:
A. Create Case · B. Run SOAR blueprint · C. Add suppression · D. Drop indexed logs

**42.** MITRE ATT&CK Map use:
A. Strategic gap analysis of detection coverage
B. Cost optimization
C. AWS account management
D. RBAC

**43.** Best template variable for the matching user:
A. `{{user_id}}` · B. `{{@usr.id}}` · C. `${USER_ID}` · D. `[user]`

**44.** A signal status of "Under Review" indicates:
A. The signal is actively being investigated
B. The signal is closed
C. The signal is suppressed
D. The signal is archived

---

## Section 4 — IR / SOAR / Dashboards (12 Q)

**45.** A Case in Datadog:
A. Aggregates related signals, evidence, comments, assignees
B. Replaces detection rules
C. Replaces dashboards
D. Holds archives

**46.** Notification Rules:
A. Apply across many rules and signals matching a query (cross-rule alerting policy)
B. Replace the rule's query
C. Disable the rule
D. Require Bits AI

**47.** Workflow trigger required for a signal-panel Run is:
A. Security · B. API · C. Schedule · D. Agent

**48.** Which is NOT a typical SOAR blueprint?
A. Block IP at AWS WAF · B. Disable IAM user · C. Drop indexed logs · D. Lookup IOC

**49.** When Cloud SIEM notifies Slack/Jira, Bits AI:
A. Replies in-thread with conclusion + link
B. Deletes the message
C. Sends a new email
D. Opens a new dashboard

**50.** Cases sync bidirectionally with:
A. Jira / ServiceNow · B. CloudWatch · C. Lambda · D. Synthetics

**51.** A workflow execution returns:
A. `execution_id` · B. `signal_uuid` · C. `dashboard_id` · D. `tag_id`

**52.** The recommended pattern when one signal must alert two channels (#soc Slack and PagerDuty):
A. A single Notification Rule with multiple channels
B. Two duplicate detection rules
C. Manual emailing
D. Custom workflow

**53.** Which is a useful operational SLO for SIEM?
A. ">95% of HIGH/CRITICAL signals closed within 4 hours"
B. "0% disk usage"
C. ">99% CPU"
D. "<1ms ping"

**54.** "Top users by signal count" widget type:
A. Top List · B. Time-series · C. Heatmap · D. Geomap

**55.** A workflow listing API filters by:
A. Name, tags, owner, handle, trigger type · B. Only ID · C. Only severity · D. None

**56.** Case statuses include:
A. To Do / In Progress / Closed · B. Active / Inactive · C. Open / Half-Open / Closed · D. Pass / Fail

---

## Section 5 — Content Packs (7 Q)

**57.** A Content Pack typically includes:
A. Detection rules + dashboard + Investigator preset + SOAR blueprints + parser + config guide
B. Detection rules only
C. Dashboards only
D. CloudFormation only

**58.** Activating a Content Pack does NOT automatically:
A. Configure the upstream AWS service to send logs
B. Enable the parser
C. Enable OOTB rules
D. Enable the OOTB dashboard

**59.** OCSF library mappings cover:
A. AWS, GCP, M365 Defender, Okta, Palo Alto, Zscaler ZPA, GitHub, Workspace, Infoblox
B. Only AWS
C. Only Microsoft 365
D. Only on-prem

**60.** Symptom: Pack activated, no signals; first check:
A. Logs actually arriving with the right `source:`
B. Datadog Agent version
C. Lambda concurrency
D. Browser cache

**61.** Symptom: logs arrive, OCSF detection doesn't match. Likely cause:
A. OCSF Processor misconfigured or not last in pipeline
B. Detection rule deleted
C. Bits AI disabled
D. Index full

**62.** A Content Pack's Investigator preset:
A. Provides entity-pivot views tailored to the source's schema
B. Replaces the Investigator
C. Adds a dashboard widget only
D. Disables Bits AI

**63.** Best practice for customizing the Content Pack dashboard:
A. Edit OOTB directly · B. Clone and edit clone · C. Disable pack · D. Delete OOTB

---

## Section 6 — Compliance & Reporting (7 Q)

**64.** PCI-compliant Datadog org must use site:
A. EU1 · B. US1 · C. AP1 · D. US3

**65.** PCI logs intake endpoint:
A. `agent-http-intake-pci.logs.datadoghq.com:443`
B. `app.datadoghq.com`
C. `api.datadoghq.com`
D. Default intake

**66.** Audit Trail in PCI-configured orgs:
A. Auto-enabled and must remain enabled
B. Optional
C. Disabled
D. One-time

**67.** Audit Trail searchable retention default is approximately:
A. 7 days · B. 30 days · C. 90 days · D. 1 year

**68.** OEM threat-intel persistence:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**69.** BYOTI persistence:
A. 24 hours · B. 30 days · C. As long as the reference table exists · D. Forever

**70.** Permission to read log data from a specific index:
A. `logs_read_index_data` · B. `dashboards_read` · C. `apm_read` · D. `metrics_read`

---

## Section 7 — Optimization (5 Q)

**71.** OCSF stands for:
A. Open Cybersecurity Schema Framework
B. Optional Cloud Security Forum
C. Outbound Cybersecurity Standard Format
D. Operational Cyber Security Foundation

**72.** OCSF Processor placement:
A. First · B. Last · C. Anywhere · D. Outside the pipeline

**73.** BYOTI uses which Datadog object?
A. Reference Table · B. Custom Metric · C. Notebook · D. Synthetics

**74.** Cost dial for **detection** volume:
A. Security Filter · B. Index Filter · C. Exclusion Filter · D. Live Tail

**75.** A symptom that the Security Filter is too narrow:
A. Detections that should fire never do
B. Too many low-value signals
C. Index full
D. Workflows fail

---

## Answer key

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 16 | A | 31 | A | 46 | A | 61 | A |
| 2 | C | 17 | B | 32 | A | 47 | A | 62 | A |
| 3 | B | 18 | A | 33 | A | 48 | C | 63 | B |
| 4 | B | 19 | C | 34 | B | 49 | A | 64 | B |
| 5 | A | 20 | B | 35 | B | 50 | A | 65 | A |
| 6 | A | 21 | B | 36 | A | 51 | A | 66 | A |
| 7 | C | 22 | B | 37 | B | 52 | A | 67 | C |
| 8 | A | 23 | A | 38 | B | 53 | A | 68 | A |
| 9 | A | 24 | A | 39 | D | 54 | A | 69 | C |
| 10 | A | 25 | B | 40 | A | 55 | A | 70 | A |
| 11 | A | 26 | A | 41 | D | 56 | A | 71 | A |
| 12 | A | 27 | C | 42 | A | 57 | A | 72 | B |
| 13 | C | 28 | B | 43 | B | 58 | A | 73 | A |
| 14 | B | 29 | C | 44 | A | 59 | A | 74 | A |
| 15 | B | 30 | A | 45 | A | 60 | A | 75 | A |

## Per-domain breakdown (fill in)

| Domain | Qs | Correct | % | Δ vs Mock #1 |
|---|---|---|---|---|
| Cloud SIEM Fundamentals (1–12) | 12 | | | |
| Datadog Setup (13–28) | 16 | | | |
| Detection (29–44) | 16 | | | |
| IR/SOAR/Dash (45–56) | 12 | | | |
| Content Packs (57–63) | 7 | | | |
| Compliance (64–70) | 7 | | | |
| Optimization (71–75) | 5 | | | |
| **Total** | **75** | | | |

If your Mock #2 score is **higher than Mock #1**, you're trending the right way. If equal or lower, do one more cycle of Adaptive Drill on your weakest domain before sitting the exam.
