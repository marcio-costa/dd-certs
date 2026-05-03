# Domain 2.C — API Keys, Application Keys, Sites & Org Settings

> **Source docs:** `/account_management/api-app-keys/`, `/getting_started/site/`

## 2.C.1 API Keys vs. Application Keys

| | API Key | Application (App) Key |
|---|---|---|
| **Purpose** | Identifies the **organization** sending data. Used by the **Agent** and the **submission API** (metrics, logs, traces, events, service checks). | Identifies a **user** and their permissions. Used by the **read/configuration API** (dashboards, monitors, SLOs, etc.). |
| **Required by Agent?** | **Yes** | No |
| **Created where** | *Organization Settings → API Keys* | *Organization Settings → Application Keys* (also created automatically per user) |
| **Scope** | Org-wide; can be revoked centrally. | Inherits the user's RBAC permissions. App keys can be **scoped** to specific roles or service accounts. |
| **Format** | 32-character hex string. | 40-character hex string. |
| **Rotation** | Rotate by creating a new key, switching the Agent to it, then deleting the old one. | Same. App keys can also be deleted by the owning user. |

**On the exam:** if a question is about the Agent submitting data, the Agent only needs the **API key**. App keys come into play when calling the management API (e.g., to create monitors via Terraform).

## 2.C.2 Datadog Sites (Regions)

The site you choose at signup determines:
- The web UI URL you log in to.
- The API/intake endpoints the Agent must reach.
- Where your data is stored (data residency).

| Site (UI) | Subdomain pattern | API hostname | When to pick |
|---|---|---|---|
| US1 (default) | `app.datadoghq.com` | `api.datadoghq.com` | Default for most US/global orgs |
| US3 | `us3.datadoghq.com` | `api.us3.datadoghq.com` | Azure-hosted region |
| US5 | `us5.datadoghq.com` | `api.us5.datadoghq.com` | GCP-hosted region |
| EU1 | `app.datadoghq.eu` | `api.datadoghq.eu` | EU data residency |
| US1-FED | `app.ddog-gov.com` | `api.ddog-gov.com` | US Federal (FedRAMP-authorized) |
| AP1 | `ap1.datadoghq.com` | `api.ap1.datadoghq.com` | Asia-Pacific (Tokyo) |

### Configuring the site

In `datadog.yaml`:
```yaml
site: datadoghq.eu
```

As an env var:
```bash
DD_SITE=datadoghq.eu
```

Helm:
```yaml
datadog:
  site: datadoghq.eu
```

> **You cannot move data between sites.** If you signed up on the wrong site, you must create a new org on the correct site and reinstall Agents.

## 2.C.3 Organizations and Sub-Organizations

- One **organization** = one Datadog tenant = one set of API keys + one billing entity.
- **Multi-org accounts** are available for parent companies who want isolated child orgs (each with its own users, dashboards, monitors). The parent has a console to switch between them.

## 2.C.4 Users, Roles & RBAC

Datadog has three default roles:

| Role | Capabilities |
|---|---|
| **Datadog Admin Role** | Full read/write across all assets, plus org settings, billing, users, and API keys. |
| **Datadog Standard Role** | Read/write on most assets (dashboards, monitors, etc.), but no org-level changes. |
| **Datadog Read-Only Role** | View-only access. |

**Custom roles** are built from fine-grained **permissions** (e.g., `dashboards_write`, `monitors_downtime`, `logs_read_data`). Use these for least-privilege access.

**Service Accounts** are non-human accounts to which you can attach **App Keys** for automation (Terraform, CI/CD).

## 2.C.5 SSO and User Provisioning

- SAML SSO (Okta, OneLogin, Azure AD, Google Workspace).
- SCIM provisioning to auto-create/disable users from your IdP.
- Configured in *Organization Settings → SAML*.

## 2.C.6 Common Exam Pitfalls

- A team can have many App keys but should typically share **one** API key per environment to keep ingest centralized.
- If the Agent reports "Forbidden" or "Invalid API key" in `agent status` after changing keys, you forgot to restart the Agent.
- US1-FED, US3, US5, EU1, AP1 each have separate `api.*` and `app.*` hostnames — proxy/firewall rules must allow them.
- Rotating an API key does **not** invalidate already-ingested data, but the Agent will start failing intake the moment the key is deleted.
