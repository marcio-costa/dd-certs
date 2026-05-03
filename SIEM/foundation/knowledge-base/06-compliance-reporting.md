# Domain 6 – Compliance and Reporting

> Sources: Cloud SIEM Data Security, Datadog PCI Compliance docs, Audit Trail, Compliance Frameworks docs, Log Archives.

## 6.1 Threat Tracking and Risk Reporting

Cloud SIEM gives you a few first-class artifacts to track risk:
- **Security Signals Explorer** — the source of truth for what's actively detected.
- **MITRE ATT&CK Map** — the strategic view of detection coverage by tactic/technique.
- **Cloud SIEM dashboards** — operational and executive views (signals over time, top detected tactics, top targeted users/services).
- **Custom reports** — Notebooks, scheduled dashboard exports (PDF/email), and Saved Views.

For exec-level reporting, the recommended pattern is:
1. Define KPIs (signals/day by severity, % HIGH+ closed within SLA, mean time to resolve, % MITRE coverage).
2. Build a dashboard combining Security Signal widgets + Logs-to-Metrics widgets.
3. Schedule weekly/monthly delivery via dashboard scheduled reports.

## 6.2 Compliance Framework Mapping

Datadog supports mapping detection rules and posture findings to common compliance frameworks. Cloud SIEM specifically focuses on **runtime / log-based detection** as compliance evidence; **Cloud Security Misconfigurations (CSPM)** is the product that maps **configuration** to frameworks (PCI DSS, HIPAA, SOC 2, ISO 27001, NIST 800-53, CIS).

For SIEM:
- Detection rules can be tagged by framework (e.g., `compliance:pci-dss`).
- The MITRE ATT&CK Map shows technique coverage; combine with framework tags to argue evidence-of-control.
- Audit Trail provides evidence of *who did what in Datadog* (rule changes, user logins, dashboard edits).

## 6.3 Data Retention and Security Policies

### Retention tiers — recap
- **Standard Indexes** — short-term (3, 7, 15, 30, 60, 90 days) for hot search.
- **Flex Logs** — months to years; queryable on demand without rehydrate.
- **Log Archives** — multi-year storage in your S3/Azure/GCS; rehydrate to query.

### Policy patterns for compliance
- **PCI DSS** requires specific endpoints and audit-trail evidence:
  - Logs go to PCI intake: `agent-http-intake-pci.logs.datadoghq.com:443`
  - APM traces go to PCI APM endpoint: `https://trace-pci.agent.datadoghq.com`
  - Audit Trail must be enabled and remain enabled (auto-enabled when org is PCI-configured)
  - Org must be in **US1** site
  - All transport HTTPS only
- **HIPAA / SOC2 / GDPR** patterns commonly apply Sensitive Data Scanner before indexing and rely on Archive + Flex tiering for retention requirements.
- **Threat intelligence persistence:** OEM- and integration-based feeds last **24 hours**; bring-your-own threat intelligence persists for **as long as configured** in the reference table.

### RBAC for security data
- `security_monitoring_signals_read`, `security_monitoring_rules_read/write`, `security_monitoring_filters_read/write` permissions.
- `logs_read_index_data` — reads logs from a given index.
- Use **roles** (not individual users) for permissions; default Datadog roles are Administrator, Standard, Read-only.
- Sensitive index data should be guarded at the **index** level (per-index `read` permission).

## 6.4 Audit Reporting and Evidence Generation

**Audit Trail** records administrative actions in Datadog (user logins, role changes, rule edits, dashboard changes, API key usage). It supports search and analytics for up to **90 days** of retention by default, with **long-term retention and archiving** for compliance.

For evidence packages an auditor would ask for:
- A copy of the active **detection rules** (export via API)
- A list of **Security Signals** for the period with severity, status, assignee, time-to-resolve
- **Audit Trail** events showing who modified rules / suppression
- **Sensitive Data Scanner** redaction rule list and run history
- Archive bucket inventory + retention policy

## 6.5 Security Metrics and Performance Monitoring

Track the health of your SIEM operation with metrics:
- **Detection volume** — signals/day by severity. A sudden drop = a broken pipeline.
- **MTTR** — time from signal open to signal closed.
- **MTTA** — time from signal open to assignee acknowledged.
- **% of HIGH/CRITICAL closed within SLA**
- **% MITRE ATT&CK coverage** — count of tactics with at least one rule active
- **Noise rate** — signals archived as false positive / total signals
- **Ingest volume** — daily log GB into the security index (cost driver)

These can be implemented as Logs-to-Metrics from the security signals event source, then plotted on a dashboard with SLOs.
