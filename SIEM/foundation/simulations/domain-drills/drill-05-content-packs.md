# Domain Drill #5 — Content Pack Implementation and Usage (15 Q)

> Week 3 consolidation. ~20 minutes timed.

**1.** A Content Pack typically includes:
A. Detection rules + dashboard + Investigator preset + SOAR blueprints + parser + config guide
B. Detection rules only
C. Dashboards only
D. Just a CloudFormation template

**2.** Activating a Content Pack does NOT automatically:
A. Configure the upstream AWS service to send logs
B. Enable the parser
C. Enable OOTB rules
D. Enable the OOTB dashboard

**3.** Common AWS Content Packs include:
A. CloudTrail, GuardDuty, VPC Flow Logs, WAF, Route 53, S3, ELB/ALB, EKS audit
B. AWS IoT only
C. AWS DMS
D. AWS Cost Explorer

**4.** OCSF Pipeline support is available for many integrations including:
A. AWS CloudTrail / GuardDuty / WAF, GCP, M365 Defender, Okta, Palo Alto, Zscaler ZPA, GitHub, Workspace Admin, Infoblox
B. Only AWS
C. Only Microsoft 365
D. Only on-prem firewalls

**5.** Symptom: Content Pack activated but no signals. Most likely cause:
A. Logs aren't actually arriving with the right `source:`
B. AWS Lambda concurrency
C. Browser cache
D. RBAC

**6.** Symptom: logs arrive, OCSF detection doesn't match. Likely cause:
A. OCSF Processor misconfigured or not last in pipeline
B. Detection rule is disabled
C. Bits AI is off
D. Index full

**7.** Best practice for customizing the Content Pack dashboard:
A. Edit the OOTB directly · B. Clone and edit the clone · C. Disable the pack · D. Delete OOTB

**8.** A Content Pack's Investigator preset:
A. Provides entity-pivot views tailored to that source's attribute schema
B. Replaces the Investigator
C. Is a dashboard widget only
D. Disables Bits AI

**9.** A "new value" rule from a freshly-activated pack often appears noisy because:
A. It is still building its 24h baseline
B. The pipeline is broken
C. AWS WAF is offline
D. The rule is misconfigured

**10.** When a pipeline parser is broken (reserved attributes empty), a remediation is:
A. Re-enable / re-import the integration pipeline; ensure custom pipelines aren't overwriting it
B. Delete the index
C. Disable Cloud SIEM
D. Recreate the AWS account

**11.** A Content Pack's SOAR blueprint:
A. Is a pre-built workflow template you customize for your environment
B. Replaces the SOAR engine
C. Disables detection rules
D. Is a Lambda function

**12.** Notification Rules for Content Pack signals:
A. Should typically route HIGH/CRITICAL severity signals to a SOC channel
B. Are required to be configured per Content Pack
C. Cannot be set per source
D. Must include AWS region

**13.** A Content Pack with no OCSF mapping:
A. Will not work
B. Still works for native Cloud SIEM detection; OCSF mapping is optional
C. Cannot be activated
D. Disables Bits AI

**14.** When you operationalize a Content Pack, the FIRST verification action is:
A. Check that logs actually arrive with the right tags
B. Run a SOAR blueprint
C. Send to Slack
D. Disable OOTB rules

**15.** Which is NOT typical periodic maintenance for a Content Pack?
A. Reviewing which OOTB rules fire vs. are silent
B. Updating the dashboard with new widgets
C. Deleting the integration pipeline
D. Re-validating the parser after upgrades

---

## Answer key
1-A 2-A 3-A 4-A 5-A 6-A 7-B 8-A 9-A 10-A 11-A 12-A 13-B 14-A 15-C
