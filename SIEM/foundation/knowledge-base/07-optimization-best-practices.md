# Domain 7 – Optimization and Best Practices

> Sources: OCSF Processor docs, Bring Your Own Threat Intelligence, Workflow Automation, Cloud SIEM ingest_and_enrich docs.

## 7.1 OCSF Data Normalization

### Why OCSF
The **Open Cybersecurity Schema Framework (OCSF)** is a vendor-neutral schema for security telemetry. Normalizing to OCSF means:
- Detection rules can be written once against OCSF fields and apply across vendors.
- Cross-tool correlation (CloudTrail vs. Okta vs. WAF) becomes straightforward.
- Data exports and threat-intel matching are easier.

### How to implement OCSF in Datadog
Datadog provides OCSF support in two places:
1. **Cloud SIEM Log Pipelines (built-in)** — many Content Packs ship with OCSF mappings out of the box (CloudTrail, GuardDuty, WAF, Okta, GitHub, Workspace Admin, Infoblox, M365 Defender, Palo Alto, Zscaler ZPA, …).
2. **Observability Pipelines OCSF Processor** — for custom or unsupported sources, you add an OCSF Processor that remaps logs to OCSF event classes.

### OCSF Processor — placement and configuration
- **Placement:** Datadog recommends the OCSF Processor be the **last** processor in your pipeline. Do all parsing/enrichment first, then remap to OCSF so the canonical fields are populated.
- **Configuration:** specify the source log type, the target OCSF event class (e.g., `Authentication`, `API Activity`, `File Activity`, `Network Activity`), the OCSF version, and any custom field mappings (including ENUM attributes with numeric values).
- **Multiple mappings per processor** are supported — useful when a single pipeline emits several OCSF event classes.

### OCSF event class examples
- **Authentication** (1002) — login events from Okta, AWS IAM ConsoleLogin, Google Workspace.
- **API Activity** (6003) — most CloudTrail management events.
- **Network Activity** (4001) — VPC Flow Logs, Zscaler ZPA, ALB.
- **Detection Finding** (2004) — GuardDuty findings, third-party detections.

## 7.2 Threat Intelligence and Security Research

### Built-in threat intelligence
Datadog provides built-in threat intelligence for Cloud SIEM logs out of the box. Indicators from OEM and integration-based feeds **persist for 24 hours** and are automatically applied to enrich logs.

### Bring Your Own Threat Intelligence (BYOTI)
- You upload IOC lists (IPs, domains, hashes, URLs) as **Reference Tables** in Datadog.
- Configure a **Threat Intel Processor** in your log pipeline. The processor accepts an **IoC key** (the attribute on the log to match — typically `@network.client.ip`, `@dns.question.name`, or a hash field) and the reference table to compare against.
- Matched logs are enriched with `threat_intel.*` attributes (provider, indicator type, confidence, etc.).
- Reference Tables can be combined with arbitrary metadata to enrich logs beyond IOC matching.
- Persistence: **as long as you configure** in the reference table — unlike OEM feeds, no 24-hour expiry.

Use cases:
- Match outbound IPs against an IOC list to fire detections on contact with known malicious infrastructure.
- Enrich CloudTrail and VPC Flow Log records with the threat-intel feed your SOC subscribes to (e.g., from MISP, ThreatStream, custom STIX feed).

## 7.3 Automation and Operational Improvement

Continuous improvement of your SIEM operation:
- **Auto-suppress** noisy rules with explicit suppression conditions instead of disabling them.
- **Move repetitive triage** into SOAR workflows; let analysts focus on novel signals.
- **Promote successful threat hunts** into custom detection rules so the next event auto-triggers.
- Use **Bits AI Security Analyst** as a first-pass triage layer; capture its accuracy with feedback to improve.
- Schedule **weekly reviews** of the MITRE coverage map to spot drift.
- Track **noise rate per rule** as an operating metric and deactivate / tighten the worst offenders.

## 7.4 Team Enablement and Training

- Onboard analysts using the Datadog Cloud Security Engineer learning path + Attacks & Threat Detection learning path.
- Build internal **runbooks** (use the playbook field on each rule).
- Create **shared Saved Views** in Logs Explorer and Security Signals Explorer for common hunts.
- Use **Notebooks** to document investigations and turn them into post-mortems and shared evidence.
- Standardize **case templates** so investigations follow the same shape (impact, scope, root cause, remediation).

## 7.5 Infrastructure Management and Continuous Optimization

### Cost / volume optimization
- **Tighten the Security Filter** — only log sources that actually need detection should be analyzed. Logs you only need for forensics can stay in Flex/Archive.
- Move high-volume / low-value logs (debug logs, healthchecks) to **Flex Logs** rather than Standard Indexes.
- Use **Exclusion Filters** to drop predictable noise *before* indexing — but never before detection (use the Security Filter for that).
- Leverage **Sensitive Data Scanner** at ingest to pre-compute redactions instead of re-processing later.

### Pipeline hygiene
- One log source = one pipeline. Don't fork OOTB pipelines; clone them if you need changes.
- Order processors carefully: parse → enrich → categorize → OCSF (last).
- Re-test pipelines whenever you upgrade an integration; new fields may be added that require parser updates.

### Detection hygiene
- Quarterly review of OOTB rules: enable any new ones Datadog has shipped; review your custom rules for staleness.
- For each rule, track: signals/week, true-positive rate, MTTR. Deprecate or tighten the bottom decile.
- Maintain a **suppression registry** so you know exactly which entities are excluded from which rules and why.
