# Domain 4 – Incident Response, Dashboarding, and Automation (SOAR)

> Sources: Cloud SIEM Respond and Report docs, Workflow Automation, Case Management, Bits AI Security Analyst, Datadog Dashboards.

## 4.1 Incident Response Workflow Design

Incident response in Cloud SIEM is built on three pillars:
1. **Notification rules** — choose where alerts are delivered (email, Slack, PagerDuty, Jira, ServiceNow, webhook). Different from per-rule notification: **Notification Rules** apply across many detection rules with conditions like "if severity is HIGH or CRITICAL".
2. **Case Management** — each significant signal can be promoted to a **Case** that aggregates related signals, evidence, comments, assignees, and status (To Do, In Progress, Closed).
3. **Workflow Automation (SOAR)** — runs automated or semi-automated response actions (block IP at WAF, disable user, snapshot EBS, lookup IOC, etc.).

Typical end-to-end flow:
```
Signal fires → Notification Rule routes to channel → on-call analyst opens
            → Investigator + Bits AI quickly pivots
            → Promote to Case (or Bits AI auto-creates one)
            → Run SOAR Workflow blueprint to contain
            → Close Case with evidence + comments
            → Improve detection (tune thresholds / add suppression)
```

## 4.2 Security Dashboards and Metrics

Cloud SIEM ships pre-built dashboards for each Content Pack (CloudTrail, GuardDuty, etc.). They show:
- Signal volume by severity over time
- Top users / IPs / resources triggering signals
- Detection rule coverage (which rules are firing)
- MITRE coverage heatmap

You can clone any OOTB dashboard and customize it. For metrics dashboards:
- Use **Logs to Metrics** to extract numerical metrics from logs (e.g., `failed_logins.count`).
- Use **Security Signals**-source widgets to show counts/groupings of signals.
- Add **MITRE ATT&CK Map** as a widget.
- Add **SLO** widgets to track MTTA / MTTR.

## 4.3 Workflow Automation and SOAR

Datadog **Workflow Automation** is the SOAR engine. A workflow is a directed graph of:
- **Triggers** — Security Signal, Manual (run from a signal), Scheduled, Monitor, API, Incident, Agent.
- **Actions** — pre-built connectors to AWS (e.g., disable IAM user, revoke key, block IP at WAF, snapshot EBS), Slack, Jira, ServiceNow, PagerDuty, Okta, GitHub, custom HTTP, custom Python.
- **Conditions / Branches** — if/else based on inputs.

### Triggering a workflow from a signal
- The workflow must have a **Security trigger** configured to be available from the signal panel.
- Analysts can **manually run** the workflow from the signal, supplying input parameters.
- Bits AI can also propose / run workflows during investigation.

### SOAR blueprints
Pre-built workflow templates published by Datadog for common containment actions:
- **Block IP at AWS WAF**
- **Disable IAM user / rotate access key**
- **Lookup IOC at threat intel provider** (VirusTotal, AbuseIPDB, etc.)
- **Send Slack message + assign signal**
- **Create Jira ticket / ServiceNow incident**
- **Quarantine EC2 (apply isolation security group)**

## 4.4 Case Management and Collaboration

Cases provide a centralized workspace to triage, track, and remediate. Key fields:
- **Title, Description, Status (To Do / In Progress / Closed)**
- **Priority** (P1–P5)
- **Assignee, Team**
- **Related Signals / Logs / Metrics / APM traces**
- **Comments and timeline**
- **Linked Jira / ServiceNow ticket** (bidirectional sync available via integration)

Bits AI and the SRE assistant can also push their findings into Case Management automatically (Jira / ServiceNow via Case Management).

## 4.5 Signal Triage and AI Assistance (Bits AI Security Analyst)

- **Coverage:** every new signal is investigated by Bits AI before a human picks it up.
- **Severity column status:** *Investigating* → *Benign* or *Suspicious*.
- **Output:** an investigation summary, evidence trail of which logs/entities were checked, and a recommendation.
- **Slack/Jira integration:** if Cloud SIEM notifies these channels, Bits AI replies in-thread with its conclusion and a link to the full investigation.
- **Side-panel actions:** create a Case (pre-populated), run a SOAR workflow (blueprint), declare an incident, add a rule suppression, archive the signal, or open the standard Cloud SIEM view.
- Always allow analysts to **provide feedback** so the model improves on your data.

## 4.6 Notifications

Two notification surfaces:
- **Per-rule notifications** — set inside the rule's "Say what's happening" section. Use template variables like `{{@usr.id}}`, `{{@network.client.ip}}`, `{{@evt.name}}`.
- **Notification Rules** — global rules that can route any signal matching a query (e.g., `severity:(HIGH OR CRITICAL) source:cloudtrail`) to a channel. Easier to manage at scale than configuring each rule individually.

Supported notification channels include Email, Slack, Microsoft Teams, PagerDuty, Jira, ServiceNow, and arbitrary webhooks.
