# Quiz 06 — Compliance and Reporting (30 Q)

> Three sub-quizzes of 10 questions each. Use them across Days 22–24.

---

## Section A — Threat tracking & frameworks (10 Q)

**1.** Which Datadog product maps cloud configuration state to compliance frameworks (PCI/HIPAA/SOC2/CIS)?
A. Cloud SIEM
B. Cloud Security Misconfigurations / CSPM
C. Workload Protection
D. RUM

**2.** Cloud SIEM's role in compliance is primarily:
A. Generating runtime / log-based detection evidence
B. Replacing CSPM
C. Monitoring CPU
D. Sending email reports

**3.** A common KPI for an exec security report is:
A. Signals/day by severity, % HIGH+ closed within SLA, MITRE coverage
B. Average disk write IOPS
C. EC2 boot time
D. Dashboard count

**4.** A detection rule can be tagged with framework labels (e.g., `compliance:pci-dss`) to:
A. Disable the rule
B. Group rules for evidence-of-control reports
C. Hide the rule
D. Enforce IAM

**5.** The MITRE ATT&CK Map is most useful for:
A. Cost optimization
B. Strategic detection coverage view by tactic and technique
C. Configuring Lambda
D. Loading test data

**6.** Audit Trail captures:
A. Network packet captures
B. Administrative actions in Datadog (rule edits, dashboard changes, user logins, API key usage)
C. AWS CloudTrail records
D. APM service errors

**7.** Cloud SIEM dashboards are appropriate for:
A. Operational + executive views with signal counts, top tactics, top users
B. Replacing detection rules
C. Replacing AWS Config
D. Hosting Lambda functions

**8.** Reports for an auditor typically include:
A. List of active detection rules, signals for the period, audit trail evidence, retention policies
B. CPU graphs only
C. Network latency graphs only
D. Free-form essays

**9.** Which is appropriate evidence that detection coverage is operational?
A. The MITRE ATT&CK Map showing rule activity per tactic
B. CPU saturation alerts
C. APM service map
D. RUM session replay

**10.** Notebooks are useful for:
A. Documenting investigations and post-mortems
B. Storing logs
C. Compiling code
D. Recording meetings

---

## Section B — Retention, PCI, RBAC (10 Q)

**1.** PCI-compliant Log Management requires:
A. EU1 site
B. US1 site, PCI intake endpoint, Audit Trail enabled, HTTPS only
C. AP1 site
D. Custom region

**2.** PCI APM endpoint is:
A. `agent-http-intake.logs.datadoghq.com`
B. `https://trace-pci.agent.datadoghq.com`
C. `app.datadoghq.com`
D. `api.datadoghq.com`

**3.** Audit Trail searchable retention default is approximately:
A. 7 days
B. 30 days
C. 90 days
D. 1 year

**4.** Which is true about Audit Trail in PCI-configured orgs?
A. It is optional
B. It is automatically enabled and must remain enabled
C. It is disabled
D. It only runs once

**5.** For long-term retention with occasional urgent queries, use:
A. Standard Indexing
B. Flex Logs
C. Log Archives
D. Live Tail

**6.** OEM/integration-based threat-intel feeds in Datadog persist for:
A. 24 hours
B. 7 days
C. 30 days
D. Indefinitely

**7.** Bring-Your-Own threat-intel persists:
A. 24 hours
B. As long as you keep the reference table
C. 30 days
D. 90 days only

**8.** Which permission grants reading log data from a specific index?
A. `logs_read_index_data`
B. `logs_admin`
C. `metrics_read`
D. `dashboards_read`

**9.** Default Datadog roles are:
A. Administrator, Standard, Read-Only
B. Owner, Editor, Viewer
C. Tier 1, Tier 2, Tier 3
D. Power User, Auditor, Guest

**10.** RBAC best practice is:
A. Assign permissions to individual users
B. Use roles for permissions; map users to roles
C. Disable RBAC for SOC operators
D. Give Admin to everyone

---

## Section C — Audit & metrics (10 Q)

**1.** Which is a useful operational metric for the SIEM team?
A. MTTR for security signals
B. Average HTTP 200 rate
C. EC2 instance count
D. RUM session length

**2.** "Noise rate per rule" is calculated as:
A. Time to resolve / Time to detect
B. Signals archived as false positive / total signals
C. Logs ingested / Logs indexed
D. Rules active / Rules enabled

**3.** A sudden drop in detection volume usually indicates:
A. Improved security posture
B. A broken pipeline or upstream log source
C. The user took a vacation
D. The exam was passed

**4.** A target SLO might look like:
A. ">95% of HIGH/CRITICAL signals closed within 4 hours"
B. "0% disk usage"
C. ">99% CPU"
D. "<1ms ping"

**5.** Audit Trail data can be retained long term via:
A. Long-term retention/archival
B. Standard Indexing only
C. RUM Sessions
D. APM traces

**6.** A metric to flag SIEM ingest cost is:
A. Daily log GB into the security index
B. Number of dashboards
C. Number of users
D. Number of monitors

**7.** A dashboard widget for "signals open by severity over time" should be sourced from:
A. Security Signals data source
B. APM data source
C. RUM data source
D. Synthetic test data

**8.** Which can be extracted from signals to build trend metrics?
A. Logs to Metrics
B. Custom span metrics
C. RUM metrics
D. Database query metrics

**9.** The point of measuring MTTA is to:
A. Confirm an analyst saw the signal in time
B. Track CPU usage
C. Measure ingest volume
D. Charge customers

**10.** Periodic review of detection rules is best done:
A. Daily for every rule
B. Quarterly, focusing on noisy/silent rules
C. Yearly after the audit
D. Never

---

## Answer keys

### Section A — Threat tracking & frameworks
1-B, 2-A, 3-A, 4-B, 5-B, 6-B, 7-A, 8-A, 9-A, 10-A

### Section B — Retention, PCI, RBAC
1-B, 2-B, 3-C, 4-B, 5-B, 6-A, 7-B, 8-A, 9-A, 10-B

### Section C — Audit & metrics
1-A, 2-B, 3-B, 4-A, 5-A, 6-A, 7-A, 8-A, 9-A, 10-B

### Highlights / explanations
- *B-Q6 / B-Q7* — Memorize: OEM/integration threat-intel persistence is **24 hours**; BYOTI persists as long as configured.
- *B-Q1/Q2* — PCI requires US1 + PCI endpoints (`agent-http-intake-pci.logs.datadoghq.com` for logs, `trace-pci.agent.datadoghq.com` for APM) + Audit Trail. Common exam targets.
