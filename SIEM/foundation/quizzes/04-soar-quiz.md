# Quiz 04 — Incident Response, Dashboarding, and Automation (40 Q)

> Four sub-quizzes of 10 questions each. Use them across Days 15–18.

---

## Section A — IR workflow design (10 Q)

**1.** Which Datadog object aggregates related signals, evidence, comments and assignees?
A. Notebook
B. Case
C. Dashboard
D. Pipeline

**2.** Notification Rules are best described as:
A. Per-rule notification configuration only
B. Cross-rule alerting policies that route signals matching a query to channels
C. Permissions controls
D. Detection rule clones

**3.** A typical end-to-end SOC flow in Cloud SIEM is:
A. Logs → Index → Notebook → Forget
B. Signal → Notification Rule → Investigator/Bits AI → Case → SOAR → Improve detection
C. Audit Trail → SOAR → API key
D. Dashboard → Alert → Customer Support

**4.** Which is FIRST in the response loop?
A. Run a SOAR workflow
B. A signal is triggered by a detection rule
C. Close the case
D. Edit the OOTB rule

**5.** Cases support which collaboration features?
A. Assignees, status (To Do/In Progress/Closed), comments, priority
B. Slack only
C. Jira only
D. None

**6.** Which Datadog product provides the SOAR engine?
A. APM
B. Workflow Automation
C. Database Monitoring
D. Real User Monitoring

**7.** A notification rule that fires for `severity:(HIGH OR CRITICAL)` is best used to:
A. Block IP addresses
B. Route the most important signals to a 24/7 channel
C. Disable noisy rules
D. Lower severity

**8.** "Promote to Case" workflow exists to:
A. Increase signal severity
B. Track an investigation with structured fields and history
C. Send to email only
D. Mute notifications

**9.** Which is a Bits AI side-panel action?
A. Create a Case with pre-populated context
B. Drop indexed logs
C. Delete the rule
D. Restart the Datadog Agent

**10.** Improving detection after each incident usually involves:
A. Disabling the rule entirely
B. Tuning thresholds, adding suppression, or creating new rules to capture missed indicators
C. Increasing severity to Critical for everything
D. Disabling Cloud SIEM

---

## Section B — Dashboards & metrics (10 Q)

**1.** A common security KPI to track on a Cloud SIEM dashboard is:
A. CPU utilization
B. Mean time to resolve (MTTR) signals
C. Network latency to a CDN
D. Database query QPS

**2.** Which feature converts log activity into metrics?
A. Logs to Metrics
B. Custom Metrics Explorer
C. Trace Metrics
D. RUM Sessions

**3.** The MITRE ATT&CK Map can be added to a dashboard as a:
A. Widget
B. Detection rule
C. Forwarder
D. Tag

**4.** Cloud SIEM dashboards are:
A. Read-only
B. Cloneable and customizable
C. Only available in the EU region
D. Hidden behind support requests

**5.** Which widget type best displays a "top users by signal count" view?
A. Time-series
B. Top List / Toplist
C. Heatmap
D. Geomap

**6.** When tracking MTTA, you measure time from:
A. Signal open → Assignee acknowledged
B. Detection rule created → Detection rule disabled
C. Log ingest → Log archived
D. Incident declared → Incident resolved

**7.** SLOs for Cloud SIEM operations would track:
A. % HIGH/CRITICAL signals closed within X hours
B. Number of dashboards
C. Number of integrations
D. Number of pipelines

**8.** Security widget for "signals over time" pulls from data source:
A. APM Trace
B. Security Signals
C. RUM events
D. Synthetics tests

**9.** A Logs-to-Metrics rule for `failed_logins.count` would query something like:
A. `source:cloudtrail @evt.name:ConsoleLogin @evt.outcome:failure`
B. `service:datadog-agent`
C. `host:* error:true`
D. `metric.name:cpu.usage`

**10.** The exec-level pattern for reporting is:
A. Dashboard with KPIs + scheduled email/PDF delivery
B. Manually copy-paste to Excel
C. Disable Cloud SIEM during exec reviews
D. Use only Notebooks

---

## Section C — Workflow Automation / SOAR (10 Q)

**1.** A Workflow Automation **trigger** can be:
A. Security Signal, Manual, Scheduled, Monitor, API, Incident, Agent
B. Only API
C. Only Schedule
D. Only Monitor

**2.** For a workflow to be runnable from a security signal, it must have:
A. A Security trigger configured
B. A custom AWS account
C. A Workflow Premium license
D. A Case attached

**3.** Which is NOT a typical SOAR blueprint action?
A. Block IP at AWS WAF
B. Disable IAM user / rotate access key
C. Create Jira ticket
D. Drop indexed logs from the security index

**4.** What identifies a workflow execution programmatically?
A. `execution_id`
B. `signal_uuid`
C. `dashboard_id`
D. `tag_id`

**5.** A workflow with an **Agent trigger** is often used by:
A. Bits AI / Datadog MCP server to execute remediation
B. The Datadog billing system
C. Database Monitoring
D. RUM

**6.** Which API endpoint executes a published workflow with an agent trigger?
A. `POST /execute_datadog_workflow`
B. `POST /v1/run_workflow`
C. `GET /workflow/run`
D. `POST /v2/workflows/start`

**7.** Workflow listing API can filter by:
A. Name, tags, owner, handle, trigger type
B. Only workflow ID
C. Only severity
D. Random

**8.** A Workflow Automation **action** typically:
A. Runs an SQL query
B. Calls a connector to a third-party API or AWS service
C. Disables Cloud SIEM
D. Moves a log to archive

**9.** The recommended pattern for repeatable containment is:
A. Manual click-ops
B. Codified as a SOAR workflow with consistent inputs/outputs
C. A Slack DM to the SOC manager
D. Email to the user

**10.** A workflow can be triggered manually by:
A. Anyone with read-only access
B. Users with permission to run workflows, from the signal panel or the Workflow UI
C. Only the workflow owner
D. Only Bits AI

---

## Section D — Cases & notifications (10 Q)

**1.** Case statuses include:
A. To Do, In Progress, Closed
B. Active, Inactive
C. Open, Half-Open, Closed
D. New, Acknowledged, Resolved, Reopened only

**2.** Cases can sync bidirectionally with:
A. AWS Lambda
B. Jira / ServiceNow
C. CloudWatch
D. PagerDuty only

**3.** A Notification Rule's filter is typically:
A. A Datadog query (e.g., severity, source, env)
B. A regex on log message text
C. A Terraform plan
D. An IAM ARN

**4.** Cloud SIEM notifications can be sent to (pick the most complete):
A. Email, Slack, MS Teams, PagerDuty, Jira, ServiceNow, webhook
B. Email only
C. Slack only
D. Webhook only

**5.** Bits AI updates Slack/Jira notifications by:
A. Replying in-thread with its investigation conclusion and a link
B. Deleting the original message
C. Sending a new email
D. Opening a new dashboard

**6.** Per-rule notification messages support:
A. Markdown + template variables like `{{@usr.id}}`
B. Plain text only
C. JSON only
D. None

**7.** When the same signal must alert two channels (e.g., #soc and PagerDuty for HIGH), the cleanest approach is:
A. Two duplicate detection rules
B. A single Notification Rule with multiple channels
C. Manual emailing
D. A custom workflow

**8.** Cases store evidence such as:
A. Linked signals, logs, metrics, APM traces, comments
B. Only photographs
C. Only Slack threads
D. Only Jira tickets

**9.** The signal-to-case pattern is recommended when:
A. The signal needs ongoing investigation across multiple analysts
B. The signal can be archived in 1 minute
C. Bits AI labels it Benign
D. The signal severity is Info

**10.** Which is true about case priority?
A. P1 is least critical
B. Priorities support P1–P5 with P1 being highest
C. Priority is fixed
D. Cases don't have priority

---

## Answer keys

### Section A — IR workflow design
1-B, 2-B, 3-B, 4-B, 5-A, 6-B, 7-B, 8-B, 9-A, 10-B

### Section B — Dashboards & metrics
1-B, 2-A, 3-A, 4-B, 5-B, 6-A, 7-A, 8-B, 9-A, 10-A

### Section C — Workflow Automation / SOAR
1-A, 2-A, 3-D, 4-A, 5-A, 6-A, 7-A, 8-B, 9-B, 10-B

### Section D — Cases & notifications
1-A, 2-B, 3-A, 4-A, 5-A, 6-A, 7-B, 8-A, 9-A, 10-B

### Highlights / explanations
- *C-Q3* — SOAR workflows do NOT drop logs from the index. Containment actions are network-, identity-, or ITSM-oriented.
- *D-Q1* — Cases use **To Do / In Progress / Closed**. Don't confuse with monitor/incident statuses.
- *D-Q5* — Bits AI augments existing Slack/Jira notifications via in-thread replies, not by replacing them.
