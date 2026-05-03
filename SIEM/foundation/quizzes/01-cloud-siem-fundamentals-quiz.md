# Quiz 01 — Cloud SIEM Fundamentals for AWS (30 Q)

> Three sub-quizzes of 10 questions each. Use them across Days 2–4.

---

## Section A — Architecture & workflow (10 Q)

**1.** Which Datadog object stores the result of a detection rule firing?
A. Log
B. Security Signal
C. Metric Event
D. Trace Span

**2.** What does the Cloud SIEM Security Filter define?
A. The list of signals shown in the Security Signals Explorer
B. The subset of ingested logs that detection rules are allowed to analyze
C. The retention policy for the security log index
D. The list of users that receive notifications

**3.** When you enable Cloud SIEM, Datadog creates a dedicated:
A. Log archive
B. Detection rule set only
C. Log index for security data that must be prioritized correctly
D. AWS account

**4.** A Security Signal carries which framework annotation by default?
A. NIST 800-53
B. MITRE ATT&CK tactic & technique
C. CIS AWS Benchmarks
D. PCI DSS controls

**5.** Cloud SIEM detection runs on:
A. Only logs that are indexed in Standard indexes
B. Only logs that are archived
C. Logs that match the Security Filter, regardless of index status
D. Only logs that match a custom detection rule

**6.** Which Datadog product analyzes runtime activity on hosts using the Datadog Agent (NOT Cloud SIEM)?
A. Cloud Security Misconfigurations
B. App and API Protection
C. Workload Protection
D. Sensitive Data Scanner

**7.** Severity values for a Security Signal are:
A. P1, P2, P3, P4, P5
B. Info, Low, Medium, High, Critical
C. Green, Yellow, Red
D. Tier 0, Tier 1, Tier 2

**8.** Which AI feature pre-investigates new signals and labels them Benign or Suspicious?
A. Watchdog
B. Bits AI Security Analyst
C. AI Pipelines
D. Anomaly Detection

**9.** After signal correlation produces a higher-fidelity signal, the underlying signals are:
A. Deleted
B. Still present and linked as parent/child
C. Archived automatically
D. Promoted to incidents

**10.** Which of the following is NOT a status a signal can have?
A. Open
B. Under Review
C. Archived
D. Quarantined

---

## Section B — AWS log sources (10 Q)

**1.** Which AWS log source is most relevant for detecting **IAM credential abuse and unusual API calls**?
A. VPC Flow Logs
B. CloudTrail
C. WAF logs
D. ELB logs

**2.** Which `source:` tag identifies AWS GuardDuty findings in Datadog?
A. `source:aws-guardduty`
B. `source:guardduty`
C. `source:gd`
D. `source:findings`

**3.** Which AWS log source primarily detects **east-west / outbound network traffic to suspicious IPs**?
A. CloudTrail
B. WAF
C. VPC Flow Logs
D. Route 53 Resolver query logs

**4.** Which log source is most useful for detecting **DNS-based C2**?
A. VPC Flow Logs
B. Route 53 Resolver / DNS query logs
C. ALB logs
D. S3 access logs

**5.** Which log source surfaces **anonymous reads of an S3 bucket**?
A. CloudTrail data events / S3 access logs
B. VPC Flow Logs
C. WAF logs
D. EKS audit logs

**6.** Which AWS log source maps to Kubernetes API audit records?
A. `source:eks`
B. `source:kubernetes`
C. `source:kubernetes.audit`
D. `source:k8s.audit`

**7.** Which TWO sources are the most common for the AWS Cloud SIEM out-of-the-box detections that ship with Cloud SIEM enable? (pick the best single answer that lists both)
A. CloudTrail and GuardDuty
B. APM traces and CloudWatch metrics
C. EC2 system logs and EBS snapshots
D. CodePipeline and CodeBuild

**8.** Which AWS source captures L7 web attacks at the edge?
A. AWS WAF
B. ELB access logs
C. CloudTrail data events
D. Route 53 query logs

**9.** A typical AWS-focused default Security Filter starts with which of the following queries?
A. `source:(cloudtrail OR guardduty OR route53)`
B. `service:aws`
C. `host:aws-*`
D. `env:prod`

**10.** Which AWS-related Datadog product (NOT Cloud SIEM) scans cloud configuration state for misconfigurations?
A. Application Security
B. Cloud Security Misconfigurations / CSPM
C. Workload Protection
D. Sensitive Data Scanner

---

## Section C — Data security & RBAC (10 Q)

**1.** Which feature can redact or hash PII before logs are indexed?
A. Audit Trail
B. Sensitive Data Scanner
C. Workflow Automation
D. Bits AI

**2.** For PCI-compliant Log Management, all logs must be sent to:
A. The default global intake
B. Any intake the customer prefers
C. The dedicated PCI-compliant intake (`agent-http-intake-pci.logs.datadoghq.com:443`)
D. The European intake (`http-intake.logs.datadoghq.eu`)

**3.** PCI-compliant Datadog organizations must be on which Datadog site?
A. EU1
B. US3
C. US1
D. AP1

**4.** Audit Trail provides searchable history for:
A. Detection signals
B. AWS CloudTrail records
C. Administrative actions in Datadog (rule edits, user logins, dashboard changes)
D. APM service errors

**5.** Default retention of Audit Trail searchable history (without long-term retention/archives) is approximately:
A. 7 days
B. 30 days
C. 90 days
D. 365 days

**6.** Which legacy permission allows reading log data from a specific index?
A. `logs_read_index_data`
B. `logs_index_admin`
C. `security_signals_read`
D. `dashboards_read`

**7.** Which is NOT a default Datadog role?
A. Administrator
B. Standard
C. Read-Only
D. Auditor

**8.** What does the **Security Filter** primarily protect from a *cost* perspective?
A. The number of dashboards you can create
B. The amount of data ingested by APM
C. The volume of logs eligible for detection processing
D. The number of users in the org

**9.** Which permission is required for a tool/agent to search Cloud SIEM signals?
A. `logs_read_data`
B. `security_monitoring_signals_read`
C. `dashboards_read`
D. `incidents_read`

**10.** When Sensitive Data Scanner scans a cloud storage bucket, what is sent back to Datadog?
A. The matched secret
B. Only the location of the match
C. The full file contents
D. Nothing; it runs locally only

---

## Answer keys

### Section A — Architecture
1-B, 2-B, 3-C, 4-B, 5-C, 6-C, 7-B, 8-B, 9-B, 10-D

**Explanations (highlights):**
- *Q5* — detection runs against logs matching the Security Filter even if those logs are only in Flex Logs or Archive.
- *Q6* — Workload Protection uses the Datadog Agent + detection rules at the host runtime layer.
- *Q9* — signal correlation combines without destroying the parent signals; they remain visible.
- *Q10* — signal statuses are Open / Under Review / Archived; "Quarantined" is not a status.

### Section B — AWS log sources
1-B, 2-B, 3-C, 4-B, 5-A, 6-C, 7-A, 8-A, 9-A, 10-B

**Explanations:**
- *Q5* — CloudTrail data events and S3 access logs both record bucket access; either is the right answer family.
- *Q9* — the default Cloud SIEM Security Filter for AWS starts with the cloudtrail/guardduty/route53 source list, with vpc/waf/elb/alb a close second.

### Section C — Data security & RBAC
1-B, 2-C, 3-C, 4-C, 5-C, 6-A, 7-D, 8-C, 9-B, 10-B

**Explanations:**
- *Q3* — PCI-compliant org requirement is US1.
- *Q5* — Audit Trail searchable retention is up to 90 days, with long-term retention available via archive.
- *Q10* — to keep secrets out of Datadog, the scanner only sends back the *location* of a match.
