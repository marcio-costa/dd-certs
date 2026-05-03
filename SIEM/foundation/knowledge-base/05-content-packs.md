# Domain 5 – Content Pack Implementation and Usage

> Sources: Cloud SIEM Content Packs docs, Cloud SIEM Ingest and Enrich, Getting Started with Cloud SIEM.

## 5.1 What is a Content Pack?

A **Content Pack** is a curated bundle of Datadog SIEM content built around a specific log integration. Activating a Content Pack gives you, per integration:
- **Detection Rules** tuned for that log source (OOTB, MITRE-mapped).
- **Out-of-the-box Dashboards** with security-relevant widgets.
- **Investigator presets** — entity-pivot views tailored to that log's attribute schema.
- **SOAR Workflow Automation blueprints** — ready-to-run remediation workflows.
- **Configuration guides** — step-by-step setup of the integration.
- **Log pipeline parser** — normalizes the source into structured attributes.
- Optionally, **OCSF Pipeline support** — the integration is mapped to the OCSF schema OOTB.

The full Content Pack catalog is in Security → Cloud SIEM → Content Packs.

## 5.2 Content Pack Discovery and Activation

Discovery flow:
1. Browse Content Packs by category (AWS, Azure, GCP, identity, network, endpoint, SaaS).
2. Each Content Pack page shows: required setup, included rules, included dashboards, OCSF support, sample logs.
3. Click **Activate** — this enables the parser, OOTB rules, and dashboards.
4. Follow the linked configuration guide to actually start ingesting the source.

Important: activating a Content Pack does NOT automatically configure the upstream AWS service to send logs. You still need to:
- Enable the AWS service (CloudTrail, GuardDuty, VPC Flow Logs, …)
- Send those logs to Datadog (Lambda Forwarder or Firehose destination)
- Confirm logs land with the right `source:` tag

For AWS, the most commonly-activated Content Packs in scope for the exam:
- **AWS CloudTrail**
- **AWS GuardDuty**
- **AWS VPC Flow Logs**
- **AWS WAF**
- **AWS Route 53 Resolver / DNS Query Logs**
- **AWS S3 access logs**
- **AWS ELB / ALB**
- **Amazon EKS audit logs**

## 5.3 Content Pack Configuration and Setup

After activation:
1. **Verify the parser** — open Logs → Pipelines, find the integration's pipeline. It should be enabled.
2. **Verify reserved attributes** are populated by the parser (`@usr.id`, `@network.client.ip`, `@evt.name`, `@evt.outcome`, etc.).
3. **Confirm OOTB rules** are enabled — Security → Detection Rules → filter by `source:<integration>`.
4. **Pin the dashboard** — Dashboards → search for the Content Pack name → favorite/pin.
5. **Map the SOAR workflow blueprints** — go to Workflow Automation, find templates labeled with the integration, customize action targets (e.g., the AWS account/role for IP block).
6. **Set notification rules** — route HIGH/CRITICAL signals from this source to the right channel.

## 5.4 Operational Use of Content Pack Components

Day-to-day:
- **Triage** signals via the OOTB rules — they appear in Security Signals Explorer with the proper MITRE tags.
- Open the **Content Pack dashboard** for situational awareness.
- Use the **Investigator preset** to pivot from a user/IP to all related activity for that source.
- Run **SOAR blueprints** from the signal panel for containment.
- Periodically review which OOTB rules are noisy / silent — silent rules may indicate the integration isn't actually sending data.

## 5.5 Content Pack Health Monitoring and Troubleshooting

Common issues and how to diagnose:

| Symptom | Likely cause | Fix |
|---|---|---|
| Content Pack is activated but no signals | Logs not actually arriving (`source:<x>` returns 0 hits in Logs Explorer) | Reconfigure Lambda Forwarder / Firehose subscription; check IAM and IAM trust policy |
| Logs arrive but reserved attributes are empty | Pipeline disabled or out of order | Re-enable the integration pipeline; ensure custom pipelines aren't overwriting it |
| OCSF-based detections don't match | OCSF Processor not the **last** processor, or wrong event class mapping | Reorder pipeline; verify OCSF mapping; remember OCSF Processor must be last |
| Bits AI not investigating signals | Permissions or feature not enabled | Verify Bits AI Security Analyst is enabled; check `security_signals_read` permission |
| Detection rule fires on intended source but volume is too low | Security Filter is too narrow | Update Security Filter query to include the integration's `source:` |
| New value rule keeps firing | Baseline rebuild required (24h roll-up) | Wait 24h or temporarily increase threshold |

### OCSF Pipeline support
Some Content Packs ship an **OCSF library mapping** — pre-configured transforms in the Observability Pipelines OCSF processor. The supported library mappings include AWS services (CloudTrail, GuardDuty, WAF), GCP, M365 Defender, Okta, Palo Alto Networks, Zscaler ZPA, GitHub, Google Workspace Admin, and Infoblox. Each mapping declares the log type, OCSF category, and supported OCSF versions.
