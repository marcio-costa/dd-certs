# Domain Drill #4 — Incident Response, Dashboarding, and Automation (15 Q)

> Week 3 consolidation. ~20 minutes timed.

**1.** A Case in Datadog is best used to:
A. Aggregate related signals, evidence, comments, and assignees
B. Replace detection rules
C. Replace dashboards
D. Hold archives

**2.** Notification Rules differ from per-rule notifications because they:
A. Apply across many rules and signals matching a query
B. Always require Bits AI
C. Replace the rule's query
D. Send only to email

**3.** A workflow can be triggered by:
A. Security Signal, Manual, Scheduled, Monitor, API, Incident, Agent
B. Only API
C. Only manual
D. Only schedule

**4.** Required for a workflow to be runnable from a security signal:
A. A Security trigger configured on the workflow
B. A Workflow Premium license
C. A Case attached
D. An AWS account-level policy

**5.** Which is NOT a typical SOAR blueprint action?
A. Block IP at AWS WAF
B. Disable IAM user
C. Drop indexed logs from the security index
D. Lookup IOC at threat intel provider

**6.** When SOC notifications are sent to Slack, Bits AI:
A. Replies on the same thread with its conclusion and a link
B. Deletes the original
C. Sends a new email
D. Opens a dashboard

**7.** A Case is appropriately created when:
A. The signal needs ongoing investigation across analysts
B. The signal is clearly Benign and can be archived in one click
C. Severity is Info
D. The signal is a duplicate

**8.** Cases sync bidirectionally with:
A. Jira / ServiceNow
B. CloudWatch
C. Lambda
D. Synthetics

**9.** A standard SOC KPI is:
A. MTTR for security signals
B. CPU saturation
C. RUM session length
D. Synthetic test count

**10.** Logs-to-Metrics is useful for:
A. Building trend metrics from log activity (e.g., `failed_logins.count`)
B. Replacing dashboards
C. Disabling Cloud SIEM
D. Encrypting logs

**11.** A workflow listing API supports filtering by:
A. Name, tags, owner, handle, trigger type
B. Only ID
C. Only severity
D. Only signal status

**12.** The recommended notification pattern when one signal must alert multiple channels:
A. A single Notification Rule with multiple channels
B. Two duplicate detection rules
C. Manual emailing
D. Custom workflow each time

**13.** A workflow execution returns:
A. `execution_id`
B. `signal_uuid`
C. `dashboard_id`
D. `tag_id`

**14.** A workflow with an Agent trigger is typically used by:
A. Bits AI / Datadog MCP server
B. The Datadog billing system
C. Database Monitoring
D. RUM

**15.** "Severity:(HIGH OR CRITICAL)" Notification Rule routing pattern is best for:
A. Routing the most important signals to the 24/7 channel
B. Disabling rules
C. Creating dashboards
D. Lowering severity

---

## Answer key
1-A 2-A 3-A 4-A 5-C 6-A 7-A 8-A 9-A 10-A 11-A 12-A 13-A 14-A 15-A
