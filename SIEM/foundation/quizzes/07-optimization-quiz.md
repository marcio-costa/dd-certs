# Quiz 07 — Optimization and Best Practices (20 Q)

> Two sub-quizzes of 10 questions each. Use them across Days 25–26.

---

## Section A — OCSF & threat intelligence (10 Q)

**1.** What does OCSF stand for?
A. Open Cybersecurity Schema Framework
B. Optional Cloud Security Forum
C. Outbound Cybersecurity Standard Format
D. Operational Cyber Security Foundation

**2.** Which Datadog processor remaps logs to OCSF events?
A. Date Remapper
B. OCSF Processor
C. Status Remapper
D. URL Parser

**3.** Where in a pipeline should the OCSF Processor be?
A. First
B. Middle
C. Last
D. In a separate pipeline

**4.** Which is NOT an OCSF event class?
A. Authentication
B. API Activity
C. Network Activity
D. Container Restart

**5.** Datadog's BYOTI uses which underlying object to store IOCs?
A. Custom Metric
B. Reference Table
C. Notebook
D. Synthetics test

**6.** Which Datadog processor matches logs against an IOC reference table and enriches them?
A. Threat Intel Processor
B. OCSF Processor
C. Lookup Processor
D. URL Parser

**7.** The Threat Intel Processor's required input is:
A. The IoC key (the log attribute to match, e.g., `@network.client.ip`)
B. A Slack channel
C. An AWS region
D. A database connection string

**8.** OEM threat intel persistence is:
A. 24 hours
B. 7 days
C. 30 days
D. 1 year

**9.** Custom threat intel (BYOTI) persistence is:
A. 24 hours fixed
B. As long as the reference table exists
C. 90 days
D. Never

**10.** Multi-mapping in a single OCSF Processor is:
A. Not supported
B. Supported — one processor can host multiple mappings (e.g., for different OCSF event classes)
C. Available only via API
D. Always required

---

## Section B — Optimization (10 Q)

**1.** A primary cost-control surface for Cloud SIEM is:
A. The Security Filter — narrowing it reduces logs eligible for detection
B. The dashboard count
C. The number of users
D. The number of pipelines

**2.** To save on indexing without losing forensic capability, the recommendation is:
A. Move high-volume, low-value logs to Flex Logs
B. Store everything in Standard Indexing
C. Stop ingesting logs
D. Disable detection

**3.** Which is true about exclusion filters?
A. They drop logs *before* indexing to save cost — they do NOT remove logs from detection
B. They drop logs from detection
C. They are equivalent to security filters
D. They only run once a day

**4.** The recommended pattern when a custom rule is consistently noisy is:
A. Disable the rule
B. Add suppression conditions or tighten thresholds; review noise rate
C. Increase severity to Critical
D. Send all signals to Slack

**5.** Pipeline hygiene best practice:
A. Fork OOTB integration pipelines
B. Don't fork OOTB pipelines; clone if you need changes; one source = one pipeline
C. Use one giant pipeline for everything
D. Disable OOTB pipelines and rebuild

**6.** Quarterly detection-hygiene work should:
A. Enable any new OOTB rules; deprecate / tune the bottom decile by noise
B. Ignore signal trends
C. Disable Cloud SIEM
D. Delete dashboards

**7.** A "suppression registry" is:
A. A document or system that records all rule suppressions and the rationale for each
B. A dashboard for billing
C. A SOAR blueprint
D. A type of detection rule

**8.** A symptom that the Security Filter is too narrow is:
A. Detections that should have fired never do
B. Too many signals
C. Too many dashboards
D. CPU saturation

**9.** A symptom that the Security Filter is too broad is:
A. Too many low-value signals; high cost
B. Detections never fire
C. AWS VPC Flow Logs not arriving
D. Workflow failures

**10.** Best practice for runbooks is to:
A. Keep them in someone's local notes
B. Use the Playbook field of each detection rule and/or Notebooks
C. Email them once a year
D. Embed them in CloudWatch alarms

---

## Answer keys

### Section A — OCSF & threat intelligence
1-A, 2-B, 3-C, 4-D, 5-B, 6-A, 7-A, 8-A, 9-B, 10-B

### Section B — Optimization
1-A, 2-A, 3-A, 4-B, 5-B, 6-A, 7-A, 8-A, 9-A, 10-B

### Highlights / explanations
- *A-Q3* — OCSF Processor placement is one of the most repeated facts on the exam: **last in pipeline**.
- *A-Q8 / A-Q9* — Persistence delta: 24h vs configurable. Flashcard this.
- *B-Q1 / B-Q3* — Security Filter is the cost dial for *detection*. Exclusion Filters are the cost dial for *indexing*. They are different surfaces.
