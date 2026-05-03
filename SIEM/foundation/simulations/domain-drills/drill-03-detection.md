# Domain Drill #3 — Security Monitoring & Threat Detection (20 Q)

> Week 2 consolidation. ~25 minutes timed.

**1.** Which is NOT a standard component of a detection rule?
A. Query · B. Group by · C. Trace ID · D. Severity

**2.** A detection rule's "rule cases" are used to:
A. Output different severities depending on aggregated counts
B. Disable other rules
C. Skip indexing
D. Trigger only once a day

**3.** A `New Value` rule fires when:
A. A pre-existing value reappears
B. A previously-unseen value appears for a chosen attribute
C. The rule is cloned
D. Audit Trail is enabled

**4.** An `Impossible Travel` rule looks for:
A. Logins from too-distant geographies in too-short windows
B. Long traceroutes
C. Time zone mismatches only
D. Static IP allowlists

**5.** A `Signal Correlation` rule:
A. Combines existing signals into a single, higher-fidelity signal
B. Replaces all OOTB rules
C. Creates a dashboard
D. Disables Bits AI

**6.** Which rule type is best for "this IAM role assumed for the first time in a new region"?
A. Threshold · B. New Value · C. Anomaly · D. Correlation

**7.** Which rule type is best for "more than 10 failed logins from one IP in 5 minutes"?
A. Threshold · B. Anomaly · C. New Value · D. Correlation

**8.** Cloud Configuration / Infrastructure Configuration rules belong to:
A. Cloud SIEM · B. CSPM · C. APM · D. RUM

**9.** A "Third Party" rule's "Trigger for each new" attribute uses what window?
A. 1 hour · B. 24 hours · C. 7 days · D. 30 days

**10.** Cloning an OOTB rule:
A. Creates an editable copy preserving structure & MITRE annotations
B. Deletes the original
C. Moves it to Archive
D. Changes severity to Critical

**11.** Which template variable renders the matching client IP in a notification?
A. `{{ip}}` · B. `{{@network.client.ip}}` · C. `${client_ip}` · D. `{{aws.ip}}`

**12.** Suppression conditions should be used to:
A. Disable the rule entirely
B. Prevent false positives for known service/automation accounts without disabling the rule
C. Increase severity
D. Change the playbook text

**13.** When tuning a noisy rule, the FIRST step should be:
A. Disable it
B. Confirm the noise (signals/day, distribution) and add suppression or tighten thresholds
C. Promote to Critical
D. Notify the CFO

**14.** Bits AI Security Analyst statuses are:
A. Investigating → Benign or Suspicious
B. Open → Closed
C. Pending → Approved
D. Pass → Fail

**15.** Bits AI side-panel actions include all EXCEPT:
A. Create Case
B. Run SOAR blueprint
C. Add suppression
D. Drop indexed logs

**16.** Investigator pivots from a signal to:
A. Related entities and their activity over time
B. Synthetic test results
C. Billing data
D. RUM session replay

**17.** The MITRE ATT&CK Map is best used for:
A. Strategic gap analysis of detection coverage
B. Cost optimization
C. AWS account management
D. RBAC

**18.** Threat hunting is best operationalized by:
A. Saving successful hunts as detection rules
B. Disabling Cloud SIEM
C. Running once and forgetting
D. Storing notes locally

**19.** A custom rule that produces zero signals in 30 days is most likely:
A. Properly tuned
B. Mis-scoped or query-wrong
C. Working as intended for a real-world rare event
D. Either B or C — investigate

**20.** Notification messages support:
A. Markdown + template variables like `{{@usr.id}}`
B. Plain text only
C. JSON only
D. Hex strings

---

## Answer key
1-C 2-A 3-B 4-A 5-A 6-B 7-A 8-B 9-B 10-A 11-B 12-B 13-B 14-A 15-D 16-A 17-A 18-A 19-D 20-A
