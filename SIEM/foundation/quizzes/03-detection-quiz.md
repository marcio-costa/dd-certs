# Quiz 03 — Security Monitoring & Threat Detection (40 Q)

> Four sub-quizzes of 10 questions each. Use them across Days 10–13.

---

## Section A — Rule components (10 Q)

**1.** Which is NOT a component of a Cloud SIEM detection rule?
A. Query
B. Group by
C. Trace span ID
D. Severity

**2.** Rule **cases** allow a single rule to:
A. Run only once per day
B. Trigger different signals at different severities depending on aggregated counts
C. Skip indexing
D. Disable other rules

**3.** Which severities can a rule case set?
A. P1–P5
B. Info, Low, Medium, High, Critical
C. Yellow, Orange, Red
D. None — severity is fixed per rule

**4.** Suppression conditions are used to:
A. Disable the rule altogether
B. Prevent the rule from firing for specific entities (e.g., known service accounts)
C. Increase severity by one level
D. Force notifications to only one channel

**5.** Which framework is annotated on detection rules?
A. STRIDE
B. MITRE ATT&CK (tactic + technique)
C. NIST CSF
D. CIS

**6.** A notification template variable for the matching user is:
A. `{{user_id}}`
B. `{{@usr.id}}`
C. `${USER_ID}`
D. `[user]`

**7.** A custom detection rule's "Group by" controls:
A. The order of fields in notifications
B. Which attribute combinations are tracked separately when aggregating events
C. The rule's severity
D. The rule's output format

**8.** The "Playbook" field on a rule contains:
A. SQL queries
B. Free-text remediation steps shown next to the signal
C. The Terraform of the rule
D. The MITRE technique IDs

**9.** Which is true about editing OOTB rules?
A. They cannot be edited at all
B. They can be cloned, or edited in place to extend the query, change severity, or modify the playbook
C. Editing them resets them at midnight
D. Editing requires support to enable

**10.** Which of these is a valid root query attribute for an AWS console-login rule?
A. `source:cloudtrail @evt.name:ConsoleLogin`
B. `source:waf @http.method:GET`
C. `service:ec2 @env:prod`
D. `host:i-0123*`

---

## Section B — Rule types (10 Q)

**1.** Which detection rule type fires when a previously-unseen value appears for a chosen attribute?
A. Threshold
B. New Value
C. Anomaly
D. Signal Correlation

**2.** Which rule type fires when the same user logs in from two geographies in a window too short to physically travel?
A. New Value
B. Threshold
C. Impossible Travel
D. Third Party

**3.** Which rule type combines existing signals into a higher-fidelity signal?
A. Anomaly
B. Signal Correlation
C. Threshold
D. New Value

**4.** A Threshold rule fires when:
A. A static count is exceeded over a rolling window
B. A new value is observed
C. Two signals are correlated
D. An OCSF event is detected

**5.** Which rule type uses a statistical baseline to detect deviations?
A. Threshold
B. Anomaly
C. Impossible Travel
D. Third Party

**6.** Which rule category does NOT belong to Cloud SIEM (it belongs to Cloud Security Misconfigurations)?
A. Log Detection
B. Signal Correlation
C. Cloud Configuration / Infrastructure Configuration
D. Third Party

**7.** A "Third Party" rule type's "Trigger for each new" dropdown attribute generates a signal:
A. Per new attribute value over a 24-hour roll-up
B. Per matching log
C. Per minute
D. Once per day total

**8.** Which rule type is best for "first time this IAM role assumed a role in a new region"?
A. Anomaly
B. New Value
C. Threshold
D. Signal Correlation

**9.** Which is a sensible threshold for a brute-force login rule (illustrative)?
A. 5 failures in 5 minutes from one IP
B. 1 success per day
C. 1 failure per year
D. 0 failures

**10.** Which Datadog product family uses **detection rules** but is NOT Cloud SIEM?
A. Cloud Security Misconfigurations, Cloud Security Identity Risks, Workload Protection, AAP
B. Datadog APM
C. Database Monitoring
D. Real User Monitoring

---

## Section C — OOTB & custom rules (10 Q)

**1.** A best practice when an OOTB rule is too noisy is to:
A. Delete it
B. Add a suppression condition for known-good entities, or tighten thresholds
C. Disable Cloud SIEM
D. Move the index to Archive

**2.** When creating a custom rule, the recommended FIRST step is to:
A. Set MITRE annotations
B. Write the root query in Logs Explorer and verify expected matches
C. Write the playbook
D. Configure Slack notifications

**3.** Which structure contains MULTIPLE severity outcomes from the same rule?
A. Pipelines
B. Rule Cases
C. Tags
D. Saved Views

**4.** When tuning thresholds for a Threshold rule, the goal is to:
A. Maximize signal volume
B. Match real-world attacker behavior while minimizing noise
C. Match every individual log
D. Output zero signals

**5.** The MITRE ATT&CK Map view in Cloud SIEM helps you:
A. Disable rules
B. Identify gaps in detection coverage by tactic & technique
C. Pay for new rules
D. Forward signals to Slack

**6.** Cloning an OOTB rule and editing it:
A. Breaks the original
B. Creates a new editable rule that preserves MITRE annotations and structure
C. Deletes the OOTB rule
D. Requires Bits AI to be enabled

**7.** Which template variable would render the matching client IP in a notification?
A. `{{ip}}`
B. `{{@network.client.ip}}`
C. `${client_ip}`
D. `{{aws.ip}}`

**8.** Which is a strong signal that a custom rule is too broad?
A. Zero signals after 7 days
B. A few well-correlated signals per day
C. Hundreds of signals per day with no human action taken
D. The rule has 1 case

**9.** A new value rule typically requires what amount of baseline time?
A. 1 minute
B. 24 hours roll-up
C. 7 days
D. 30 days

**10.** When a rule should not fire for a known automation account, the right tool is:
A. Disabling the rule
B. Suppression list with that user's ID
C. Lowering severity to Info
D. Manually archiving every signal

---

## Section D — Signal triage & Bits AI (10 Q)

**1.** What is the Investigator in Cloud SIEM?
A. A graph-based interface for pivoting from a signal to related entities
B. A dashboard for billing
C. A workflow editor
D. A log archive viewer

**2.** A signal's status can be:
A. Open / Under Review / Archived
B. New / Snoozed / Closed
C. Active / Inactive
D. Pass / Fail

**3.** Bits AI Security Analyst statuses are:
A. Investigating → Benign or Suspicious
B. Open → Closed
C. Pending → Approved → Rejected
D. Done

**4.** When Cloud SIEM notifications are sent to Slack/Jira, Bits AI:
A. Cannot interact
B. Replies on the same thread with its conclusion and a link to the full investigation
C. Deletes the message
D. Always escalates

**5.** From the Bits AI Investigation side panel, which action is NOT available?
A. Create a Case with pre-populated context
B. Run a SOAR blueprint workflow
C. Declare an incident
D. Delete the underlying logs

**6.** Cases provide:
A. Long-term log retention
B. A centralized workspace for triage, tracking, comments, and remediation of issues
C. Replacement of detection rules
D. Replacement of dashboards

**7.** Notification Rules are best for:
A. Ad-hoc alerting per rule
B. Setting cross-rule alerting policies (e.g., "all HIGH+ to Slack #soc")
C. Managing user accounts
D. Configuring AWS

**8.** A Security Signal includes evidence in the form of:
A. A list of matching logs and aggregated values
B. A binary file attachment only
C. AWS CloudWatch alarms
D. CSV reports only

**9.** Which permission grants read access to security signals?
A. `dashboards_read`
B. `security_monitoring_signals_read`
C. `apm_read`
D. `metrics_read`

**10.** Which is the PRIMARY benefit of Bits AI in SOC operations?
A. Replaces all human analysts
B. Pre-investigates signals to reduce mean time to triage and prioritize human attention
C. Automatically closes all signals
D. Creates new detection rules

---

## Answer keys

### Section A — Rule components
1-C, 2-B, 3-B, 4-B, 5-B, 6-B, 7-B, 8-B, 9-B, 10-A

### Section B — Rule types
1-B, 2-C, 3-B, 4-A, 5-B, 6-C, 7-A, 8-B, 9-A, 10-A

### Section C — OOTB & custom rules
1-B, 2-B, 3-B, 4-B, 5-B, 6-B, 7-B, 8-C, 9-B, 10-B

### Section D — Signal triage & Bits AI
1-A, 2-A, 3-A, 4-B, 5-D, 6-B, 7-B, 8-A, 9-B, 10-B

### Highlights / explanations
- *B-Q6* — Cloud Configuration / Infrastructure Configuration belong to Cloud Security Misconfigurations / CSPM, **not** Cloud SIEM. Common distractor.
- *B-Q7* — Third Party rules use a 24-hour roll-up window for the "Trigger for each new" attribute, generating one signal per new value.
- *D-Q5* — Bits AI does not delete underlying logs. It can archive a signal, declare an incident, run a workflow, suppress a rule, etc., but never deletes evidence.
