# Quiz 05 — Content Pack Implementation and Usage (10 Q)

> Use on Day 19.

**1.** A Content Pack typically includes (pick the most complete):
A. Detection rules, dashboard, Investigator preset, SOAR blueprints, parsers, configuration guide
B. Detection rules only
C. Detection rules and dashboards only
D. Just configuration guide

**2.** Activating a Content Pack:
A. Automatically configures the upstream AWS service
B. Enables Datadog-side parser, OOTB rules, and dashboards but you still must ship the logs to Datadog
C. Replaces all custom rules
D. Disables the Security Filter

**3.** Which is NOT a typical AWS Content Pack?
A. AWS CloudTrail
B. AWS GuardDuty
C. AWS Database Migration Service
D. AWS WAF

**4.** Which OCSF library mappings does Datadog support OOTB? (pick best)
A. Only AWS
B. AWS, GCP, M365 Defender, Okta, Palo Alto, Zscaler ZPA, GitHub, Workspace Admin, Infoblox
C. Only Microsoft 365
D. Only on-prem firewalls

**5.** When a Content Pack is activated but signals never appear, the FIRST thing to check is:
A. The Datadog Agent version
B. Whether logs are actually arriving with the right `source:` tag
C. AWS Lambda concurrency
D. Dashboard refresh interval

**6.** Logs arrive but the OCSF-based detection never matches. The likely cause is:
A. The OCSF Processor is misconfigured or not the **last** processor
B. The Security Filter is too narrow
C. The detection rule was deleted
D. AWS WAF is offline

**7.** A Content Pack's Investigator preset:
A. Replaces the Investigator
B. Provides entity-pivot views tailored to that source's attribute schema
C. Disables Bits AI
D. Adds new metrics

**8.** The recommended way to customize a Content Pack dashboard is:
A. Modify the OOTB dashboard directly
B. Clone it and edit the clone
C. Disable the Content Pack
D. Use the API to delete the original

**9.** A "new value" rule that just keeps firing on a freshly-activated Content Pack is most likely:
A. Broken
B. Still building its baseline (24h roll-up); volume should subside
C. A signal correlation issue
D. Caused by the Datadog Agent

**10.** When the integration parser is broken (reserved attributes empty), the right action is:
A. Re-enable / re-import the integration pipeline; ensure custom pipelines aren't overriding it
B. Delete the index
C. Disable Cloud SIEM
D. Recreate the AWS account

---

## Answer key
1-A, 2-B, 3-C, 4-B, 5-B, 6-A, 7-B, 8-B, 9-B, 10-A

### Highlights
- *Q3* — DMS isn't a Cloud SIEM Content Pack target.
- *Q9* — New-value rules need 24h to build a baseline; expect noise initially.
- *Q10* — Pipeline ordering and OOTB pipeline preservation are common operational pitfalls.
