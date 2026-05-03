# Software Catalog (formerly Service Catalog)

> **Domain 3 — Insight Discovery**

## What it is

The **Software Catalog** is the central inventory of every service Datadog knows about — APM-instrumented or not. For each service it shows ownership, dependencies, performance summary, deployment info, security posture, SLOs, runbooks, and code links.

It is the foundation of the **Internal Developer Portal** experience.

## Where services come from

| Source                           | Notes                                                               |
| -------------------------------- | ------------------------------------------------------------------- |
| **APM instrumentation**          | Any service emitting traces appears automatically.                  |
| **Service Definitions (YAML)**   | Manually defined `entity.datadog.yaml` files (committed to repos).  |
| **OpenTelemetry resource attributes** | `service.name`, `service.namespace`, etc.                      |
| **GitHub / GitLab integration**  | Auto-detect repos and link source code.                             |

## Service Definition v2.2 example

```yaml
schema-version: v2.2
dd-service: shopping-cart
team: e-commerce
application: shopping-app
tier: "1"
type: web
languages: [go, python]
contacts:
  - {type: slack, contact: https://yourorg.slack.com/archives/e-commerce}
  - {type: email, contact: ecommerce@example.com}
links:
  - {name: Runbook,  type: runbook, url: http://runbook/shopping-cart}
  - {name: Source,   type: repo,    provider: github, url: https://github.com/shopping-cart}
  - {name: Docs,     type: doc,     url: https://wiki/ecommerce/shopping-cart}
tags:
  - business-unit:retail
  - cost-center:engineering
integrations:
  pagerduty:
    service-url: https://www.pagerduty.com/service-directory/PSHOPPINGCART
  opsgenie:
    service-url: https://www.opsgenie.com/service/uuid
    region: US
```

## Catalog views & layouts

- **List view** — searchable table of all services with team/owner/tier filters.
- **Map view** — graph of service dependencies in **Cluster** or **Flow** layout.
- **Performance** — request rate, latency, error rate per service (sourced from trace metrics).
- **Security** — ASM and CSPM signals per service.
- **Reliability** — SLO status.
- **Cost** — Cloud Cost Management linkage when enabled.
- **Quality / Scorecards** — score services against custom rules (e.g. "has runbook", "uses USM").

## Key relationships

- A service's **dependencies** (upstream/downstream) come from APM trace data.
- The **Service Map** is a visualization layer over the same dependency graph.
- Software Catalog ↔ APM Service Page is one click — they share `dd-service`.

## Things to remember for the exam

- Services appear automatically when APM is instrumented (no YAML required).
- Service Definition is **v2.2** (current schema).
- The map view supports **Cluster** and **Flow** layouts.
- `dd-service` in the YAML must match the `service` tag in APM data for linkage.
- Tier (`tier: "1"`) is a critical-tier indicator used for SLO and Watchdog prioritization.
