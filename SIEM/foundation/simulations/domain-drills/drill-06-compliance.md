# Domain Drill #6 — Compliance and Reporting (15 Q)

> Week 4 consolidation. ~20 minutes timed.

**1.** Cloud SIEM's role in compliance is primarily:
A. Generating runtime / log-based detection evidence
B. Replacing CSPM
C. Configuring AWS
D. Replacing AWS Config

**2.** Which Datadog product maps cloud configuration state to PCI/HIPAA/SOC 2/CIS?
A. Cloud SIEM
B. Cloud Security Misconfigurations / CSPM
C. Workload Protection
D. RUM

**3.** PCI-compliant Datadog org must use which site?
A. EU1 · B. US1 · C. AP1 · D. US3

**4.** PCI logs intake endpoint:
A. `agent-http-intake-pci.logs.datadoghq.com:443`
B. `app.datadoghq.com`
C. `api.datadoghq.com`
D. Default intake

**5.** PCI APM endpoint:
A. `https://trace-pci.agent.datadoghq.com`
B. `https://app.datadoghq.com`
C. `https://api.datadoghq.com`
D. Default trace endpoint

**6.** Audit Trail in PCI-configured orgs:
A. Is automatically enabled and must remain enabled
B. Is optional
C. Is disabled
D. Runs once

**7.** Audit Trail's default searchable retention is approximately:
A. 7 days · B. 30 days · C. 90 days · D. 1 year

**8.** Which is appropriate compliance evidence for "we have detection coverage"?
A. MITRE ATT&CK Map view + active detection rules export
B. CPU usage graph
C. APM service map
D. RUM session replay

**9.** OEM/integration threat-intel feeds persist for:
A. 24 hours · B. 7 days · C. 30 days · D. Indefinite

**10.** Custom (BYOTI) threat-intel persists:
A. 24 hours
B. 30 days
C. As long as the reference table exists
D. Forever

**11.** Permission to read log data from a specific index:
A. `logs_read_index_data` · B. `dashboards_read` · C. `apm_read` · D. `metrics_read`

**12.** Default Datadog roles:
A. Administrator / Standard / Read-Only
B. Owner / Editor / Viewer
C. Tier 1 / 2 / 3
D. Power / Auditor / Guest

**13.** Sensitive Data Scanner scanning cloud storage sends back to Datadog:
A. The matched secret
B. Only the location of the match
C. The full file
D. Nothing

**14.** A useful KPI for the SIEM team is:
A. % HIGH/CRITICAL signals closed within SLA
B. CPU saturation
C. RUM session length
D. Synthetic test count

**15.** A sudden drop in detection volume usually indicates:
A. A broken pipeline / upstream log source change
B. Improved security posture
C. Holiday quiet
D. Bits AI just upgraded

---

## Answer key
1-A 2-B 3-B 4-A 5-A 6-A 7-C 8-A 9-A 10-C 11-A 12-A 13-B 14-A 15-A
