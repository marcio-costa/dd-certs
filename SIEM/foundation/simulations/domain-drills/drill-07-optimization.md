# Domain Drill #7 — Optimization and Best Practices (15 Q)

> Week 4 consolidation. ~20 minutes timed.

**1.** OCSF stands for:
A. Open Cybersecurity Schema Framework
B. Optional Cloud Security Forum
C. Outbound Cybersecurity Standard Format
D. Operational Cyber Security Foundation

**2.** OCSF Processor placement in pipeline:
A. First · B. Last · C. Anywhere · D. Outside the pipeline

**3.** Which is NOT a typical OCSF event class?
A. Authentication · B. API Activity · C. Network Activity · D. Container Restart

**4.** Multiple OCSF mappings in a single OCSF Processor:
A. Are not supported
B. Are supported (for different event classes)
C. Available only via API
D. Always required

**5.** Threat Intel Processor's required input is:
A. The IoC key (the log attribute to match, e.g., `@network.client.ip`)
B. A Slack channel
C. An AWS region
D. A database string

**6.** BYOTI uses which Datadog object to store IOCs?
A. Reference Table
B. Custom Metric
C. Notebook
D. Synthetics test

**7.** OEM threat-intel persistence:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**8.** BYOTI persistence:
A. 24 hours · B. As long as the reference table exists · C. 30 days · D. Forever fixed

**9.** Cost dial for **detection** volume:
A. Security Filter (narrowing reduces logs eligible for detection)
B. Index Filter
C. Exclusion Filter
D. Live Tail

**10.** Cost dial for **indexing** volume:
A. Security Filter
B. Exclusion Filter (drops logs before indexing)
C. Live Tail
D. Notification rule

**11.** Pipeline-hygiene best practice:
A. Don't fork OOTB pipelines; clone if changes needed; one source = one pipeline
B. Use one giant pipeline
C. Disable OOTB pipelines
D. Skip parsing

**12.** Quarterly detection-hygiene work should:
A. Enable any new OOTB rules; tune/deprecate the bottom decile by noise
B. Disable everything
C. Delete dashboards
D. Stop ingest

**13.** A "suppression registry" is:
A. A document/system tracking all suppressions and rationale
B. A dashboard for billing
C. A SOAR blueprint
D. A type of detection rule

**14.** A symptom that the Security Filter is too narrow:
A. Detections that should fire never do
B. Too many low-value signals
C. Index full
D. Workflows fail

**15.** A symptom that the Security Filter is too broad:
A. High signal volume + high cost
B. No detections fire
C. RBAC errors
D. AWS region count

---

## Answer key
1-A 2-B 3-D 4-B 5-A 6-A 7-A 8-B 9-A 10-B 11-A 12-A 13-A 14-A 15-A
