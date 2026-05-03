# Full Mock Exam #1 — 75 questions / 120 minutes

> Use on **Day 21** of the study plan. Take it under exam-like conditions: no notes, single sitting, timer on. Aim for ≥ 70% to feel ready; iterate on weak areas.
>
> **Question distribution** (mirrors approximate domain weight on the real exam):
> - Cloud SIEM Fundamentals: 12 Q
> - Datadog Setup: 16 Q
> - Detection & Threat Detection: 16 Q
> - IR / SOAR / Dashboards: 12 Q
> - Content Packs: 7 Q
> - Compliance & Reporting: 7 Q
> - Optimization: 5 Q

---

## Section 1 — Cloud SIEM Fundamentals (12 Q)

**1.** Which best describes Datadog Cloud SIEM?
A. A standalone product running on its own cluster
B. A log-based threat detection product running on Datadog's Log Management platform
C. An AWS-only SaaS managed by AWS
D. A free add-on to Datadog APM

**2.** A Security Signal is generated:
A. For every log line ingested
B. When a detection rule's conditions are met against the matching log stream
C. Only when Bits AI investigates
D. Only when Workflow Automation runs

**3.** The Cloud SIEM Security Filter primarily controls:
A. Notification routing
B. Which logs detection rules can analyze
C. Retention period
D. RBAC scope

**4.** Detection runs against logs that match the Security Filter:
A. Only when those logs are in Standard Indexing
B. Only when those logs are in Flex Logs
C. Regardless of storage tier
D. Only after rehydrate from Archive

**5.** A Security Signal carries which framework annotation by default?
A. NIST 800-53
B. MITRE ATT&CK (tactic + technique)
C. CIS AWS Benchmarks
D. PCI DSS controls

**6.** Which signal status doesn't exist?
A. Open · B. Under Review · C. Archived · D. Quarantined

**7.** GuardDuty findings appear in Datadog with `source:` tag:
A. `aws-guardduty` · B. `guardduty` · C. `gd` · D. `findings`

**8.** Which AWS source is best for L7 web attack detection?
A. CloudTrail · B. WAF · C. VPC Flow · D. ELB

**9.** Which Datadog product analyzes runtime activity on hosts using the Datadog Agent (NOT Cloud SIEM)?
A. CSPM · B. AAP · C. Workload Protection · D. Sensitive Data Scanner

**10.** Bits AI Security Analyst statuses are:
A. Investigating → Benign or Suspicious
B. Open → Closed
C. Pending → Approved → Rejected
D. Pass → Fail

**11.** The minimum recommended hands-on experience for ideal candidates is:
A. 1 month · B. 3 months · C. 6 months · D. 12 months

**12.** Which is OUT of scope per the exam guide?
A. Cloud SIEM Architecture · B. AWS Integration · C. Datadog APM · D. OCSF

---

## Section 2 — Datadog Setup (16 Q)

**13.** Tier between Standard Indexing and Archive (long-term + occasional urgent query):
A. Live Tail · B. Flex Logs · C. Hot Tier · D. Cold Tier

**14.** Cheapest tier for forensic-only logs rarely queried:
A. Standard · B. Flex · C. Archive · D. Live Tail

**15.** Exclusion filters drop logs:
A. After indexing · B. Before indexing (cost saving) · C. Only on archive · D. Only on detection

**16.** Cloud SIEM detection vs indexing:
A. Detection requires indexing
B. Detection is independent of indexing once Security Filter matches
C. Indexing is always cheaper than detection
D. They're the same

**17.** When Cloud SIEM is enabled, the new dedicated security index must be:
A. Disabled by default
B. Prioritized correctly above generic indexes
C. Stored only in S3
D. Encrypted with a customer key (required)

**18.** Recommended Firehose buffer size for Datadog destination:
A. 1 MiB · B. 2 MiB · C. 5 MiB · D. 16 MiB

**19.** Recommended Firehose content encoding to Datadog:
A. None · B. GZIP · C. ZSTD · D. Snappy

**20.** Datadog AWS account ID for the standard partition:
A. 464622532012 · B. 065115117704 · C. 123456789012 · D. 987654321000

**21.** AWS-managed policy attached to the Datadog AWS integration role:
A. AdministratorAccess · B. SecurityAudit · C. ReadOnlyAccess · D. AWSConfigUserAccess

**22.** Best AWS log delivery for fan-out to Datadog AND another consumer:
A. Lambda Forwarder · B. SNS · C. Kinesis Data Streams → Firehose → Datadog · D. CodeDeploy

**23.** Which CLI subscribes a CloudWatch Log Group to Firehose/KDS?
A. `aws cloudwatch put-metric-alarm`
B. `aws logs put-subscription-filter`
C. `aws iam attach-role-policy`
D. `aws ssm send-command`

**24.** OCSF Processor placement:
A. First · B. Last · C. Anywhere · D. Outside the pipeline

**25.** Reserved attributes that need a dedicated remapper include:
A. `host`, `message`, `service`, `status`, `date`, `trace_id`, `span_id`
B. `usr`, `network`, `evt`
C. `tags` only
D. None

**26.** Processor that converts unstructured `message` to attributes:
A. Attribute Remapper · B. Grok Parser · C. Date Remapper · D. Status Remapper

**27.** API endpoint to create a Cloud SIEM Security Filter:
A. `POST /api/v2/security_monitoring/configuration/security_filters`
B. `GET /api/v1/security_filters`
C. `PUT /api/v1/log_filters`
D. `DELETE /api/v2/security_filters`

**28.** Headers required for the Security Filter API:
A. Authorization Bearer
B. DD-API-KEY + DD-APPLICATION-KEY
C. X-Datadog-Service
D. DD-Site only

---

## Section 3 — Detection & Threat Detection (16 Q)

**29.** Which is NOT a standard component of a detection rule?
A. Query · B. Group by · C. Trace ID · D. Severity

**30.** Rule cases let one rule:
A. Output different severities depending on aggregated counts
B. Run only once a day
C. Skip indexing
D. Disable other rules

**31.** A New Value rule fires when:
A. A previously-unseen value appears for an attribute
B. A static threshold is breached
C. Two signals correlate
D. Audit Trail is enabled

**32.** An Impossible Travel rule:
A. Same user logs in from too-distant geos in too-short window
B. Long traceroutes
C. Time zone mismatches
D. Static IP allowlists

**33.** A Signal Correlation rule:
A. Combines existing signals into a higher-fidelity signal
B. Replaces all OOTB rules
C. Creates a dashboard
D. Disables Bits AI

**34.** Cloud Configuration / Infrastructure Configuration rules belong to:
A. Cloud SIEM · B. CSPM · C. APM · D. RUM

**35.** Third Party rule "trigger for each new" attribute uses what window?
A. 1 hour · B. 24 hours · C. 7 days · D. 30 days

**36.** Cloning an OOTB rule:
A. Creates an editable copy preserving structure & MITRE annotations
B. Deletes the original
C. Always raises severity to Critical
D. Disables Cloud SIEM

**37.** Suppression conditions are used to:
A. Disable the rule altogether
B. Prevent the rule from firing for known-good entities
C. Force notifications to one channel
D. Replace MITRE annotations

**38.** Best template variable for the matching client IP:
A. `{{ip}}` · B. `{{@network.client.ip}}` · C. `${client_ip}` · D. `{{aws.ip}}`

**39.** When a rule produces 500 signals/day with no human action taken, the right move is:
A. Promote to Critical
B. Tighten thresholds / suppression / re-scope
C. Notify the CFO
D. Archive Cloud SIEM

**40.** Investigator is best described as:
A. A graphical pivot from a signal to all related entities
B. CSPM dashboard
C. Billing console
D. Detection editor

**41.** Bits AI side-panel actions include all EXCEPT:
A. Create Case · B. Run SOAR blueprint · C. Add suppression · D. Drop indexed logs

**42.** MITRE ATT&CK Map is best used for:
A. Strategic gap analysis of detection coverage
B. Cost optimization
C. AWS account management
D. RBAC

**43.** A custom rule producing 0 signals in 30 days is most likely:
A. Properly tuned
B. Mis-scoped or query-wrong
C. Working as intended for a real-world rare event
D. Either B or C — investigate

**44.** Which permission grants read access to security signals?
A. `dashboards_read` · B. `security_monitoring_signals_read` · C. `apm_read` · D. `metrics_read`

---

## Section 4 — IR / SOAR / Dashboards (12 Q)

**45.** A Case in Datadog is best used to:
A. Aggregate related signals, evidence, comments, and assignees
B. Replace detection rules
C. Replace dashboards
D. Hold archives

**46.** Notification Rules differ from per-rule notifications because they:
A. Apply across many rules and signals matching a query
B. Replace the rule's query
C. Disable the rule
D. Require Bits AI

**47.** A workflow trigger that lets it run from a security signal is:
A. Security · B. API · C. Schedule · D. Agent

**48.** Which is NOT a typical SOAR blueprint?
A. Block IP at AWS WAF · B. Disable IAM user · C. Drop indexed logs from index · D. Lookup IOC

**49.** When SOC notifications go to Slack, Bits AI:
A. Replies on the same thread with its conclusion + link
B. Deletes the original
C. Sends a new email
D. Opens a dashboard

**50.** Cases sync bidirectionally with:
A. Jira / ServiceNow · B. CloudWatch · C. Lambda · D. Synthetics

**51.** A workflow execution returns:
A. `execution_id` · B. `signal_uuid` · C. `dashboard_id` · D. `tag_id`

**52.** Workflow listing API filters by:
A. Name, tags, owner, handle, trigger type · B. Only ID · C. Only severity · D. None

**53.** A standard SOC KPI is:
A. MTTR for security signals · B. CPU saturation · C. RUM session length · D. Synthetic test count

**54.** Logs-to-Metrics is useful for:
A. Building trend metrics from log activity
B. Replacing dashboards
C. Disabling Cloud SIEM
D. Encrypting logs

**55.** Case statuses include:
A. To Do / In Progress / Closed · B. Active / Inactive · C. Open / Half-Open / Closed · D. Pass / Fail

**56.** When the same signal must alert two channels, the cleanest pattern is:
A. A single Notification Rule with multiple channels
B. Two duplicate detection rules
C. Manual emailing
D. Custom workflow

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

**59.** OCSF library mappings cover (best):
A. AWS, GCP, M365 Defender, Okta, Palo Alto, Zscaler ZPA, GitHub, Workspace, Infoblox
B. Only AWS
C. Only Microsoft 365
D. Only on-prem

**60.** Symptom: Content Pack activated, no signals; first check:
A. Logs actually arriving with the right `source:`
B. Datadog Agent version
C. Lambda concurrency
D. Browser cache

**61.** Symptom: logs arrive, OCSF-based detection doesn't match. Likely cause:
A. OCSF Processor misconfigured or not last in pipeline
B. Detection rule deleted
C. Bits AI disabled
D. Index full

**62.** Best practice for customizing the Content Pack dashboard:
A. Edit OOTB directly · B. Clone and edit clone · C. Disable pack · D. Delete OOTB

**63.** A new value rule on a freshly-activated pack often appears noisy because:
A. It's still building its 24h baseline
B. Pipeline broken
C. AWS WAF offline
D. Misconfig

---

## Section 6 — Compliance & Reporting (7 Q)

**64.** Cloud SIEM's role in compliance is primarily:
A. Generating runtime/log-based detection evidence
B. Replacing CSPM
C. Configuring AWS
D. Replacing AWS Config

**65.** PCI-compliant Datadog org must use which site?
A. EU1 · B. US1 · C. AP1 · D. US3

**66.** Audit Trail's default searchable retention is approximately:
A. 7 days · B. 30 days · C. 90 days · D. 1 year

**67.** OEM threat-intel persistence:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**68.** BYOTI persistence:
A. 24 hours · B. 30 days · C. As long as the reference table exists · D. Forever

**69.** Permission to read log data from a specific index:
A. `logs_read_index_data` · B. `dashboards_read` · C. `apm_read` · D. `metrics_read`

**70.** Sensitive Data Scanner scanning cloud storage sends back to Datadog:
A. The matched secret · B. Only the location of the match · C. The full file · D. Nothing

---

## Section 7 — Optimization (5 Q)

**71.** OCSF stands for:
A. Open Cybersecurity Schema Framework
B. Optional Cloud Security Forum
C. Outbound Cybersecurity Standard Format
D. Operational Cyber Security Foundation

**72.** Which is NOT a typical OCSF event class?
A. Authentication · B. API Activity · C. Network Activity · D. Container Restart

**73.** BYOTI uses which Datadog object?
A. Reference Table · B. Custom Metric · C. Notebook · D. Synthetics

**74.** Cost dial for **detection** volume:
A. Security Filter · B. Index Filter · C. Exclusion Filter · D. Live Tail

**75.** Pipeline-hygiene best practice:
A. One source = one pipeline; clone OOTB if changes needed
B. One giant pipeline
C. Disable OOTB pipelines
D. Skip parsing

---

## Answer key (cover before taking, reveal after)

| Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 16 | B | 31 | A | 46 | A | 61 | A |
| 2 | B | 17 | B | 32 | A | 47 | A | 62 | B |
| 3 | B | 18 | B | 33 | A | 48 | C | 63 | A |
| 4 | C | 19 | B | 34 | B | 49 | A | 64 | A |
| 5 | B | 20 | A | 35 | B | 50 | A | 65 | B |
| 6 | D | 21 | B | 36 | A | 51 | A | 66 | C |
| 7 | B | 22 | C | 37 | B | 52 | A | 67 | A |
| 8 | B | 23 | B | 38 | B | 53 | A | 68 | C |
| 9 | C | 24 | B | 39 | B | 54 | A | 69 | A |
| 10 | A | 25 | A | 40 | A | 55 | A | 70 | B |
| 11 | C | 26 | B | 41 | D | 56 | A | 71 | A |
| 12 | C | 27 | A | 42 | A | 57 | A | 72 | D |
| 13 | B | 28 | B | 43 | D | 58 | A | 73 | A |
| 14 | C | 29 | C | 44 | B | 59 | A | 74 | A |
| 15 | B | 30 | A | 45 | A | 60 | A | 75 | A |

## Score band

| Score | Interpretation | Next action |
|---|---|---|
| 65–75 (≥ 87%) | Exam-ready. Light review only. | Move to Day 22+ exec; sit the real exam. |
| 53–64 (70–85%) | Likely to pass; tune weak areas. | Identify your weakest 2 domains and run an Adaptive Drill. |
| 38–52 (50–69%) | Fail risk. Re-study the worst 2 domains for 3–5 days. | Use Domain Drills + KB re-read; repeat Mock #2 after recovery. |
| < 38 (< 50%) | Don't sit yet. Restart at Week 2. | Full re-do of Days 8–14; rebuild fundamentals. |

## Per-domain breakdown (fill in)

| Domain | Qs | Correct | % |
|---|---|---|---|
| Cloud SIEM Fundamentals (1–12) | 12 | | |
| Datadog Setup (13–28) | 16 | | |
| Detection (29–44) | 16 | | |
| IR/SOAR/Dash (45–56) | 12 | | |
| Content Packs (57–63) | 7 | | |
| Compliance (64–70) | 7 | | |
| Optimization (71–75) | 5 | | |
| **Total** | **75** | | |
