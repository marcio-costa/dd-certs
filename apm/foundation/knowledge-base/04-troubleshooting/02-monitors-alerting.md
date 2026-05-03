# Monitors & Alerting (APM)

> **Domain 4 — Troubleshooting Application using APM**

## APM monitor types

| Monitor type           | Detects                                                | Underlying data                     |
| ---------------------- | ------------------------------------------------------ | ----------------------------------- |
| **APM Metric (trace)** | Threshold/anomaly/forecast/outlier on trace metrics    | `trace.*` metrics (always-on, 15 mo) |
| **APM Trace Analytics**| Query on individual spans (any tag)                    | Indexed spans                        |
| **APM Error Tracking** | New issue / regressed issue / threshold of issues      | Error Tracking issues                |
| **Watchdog**           | AI-detected anomalies (auto-generated stories)         | All ingested telemetry               |

## Automatic APM monitors

Datadog can create monitors for you for every service entry point:

- **Error rate threshold monitor** — alerts when error rate spikes beyond a default 10% (configurable per service).
- **Latency threshold monitor** — alerts on p99 latency spikes.

Generated automatically with sensible defaults; can be customized per service.

## Alert conditions on APM metrics

When you build a custom APM Metric monitor you choose:

| Condition         | Use it for                                                 |
| ----------------- | ---------------------------------------------------------- |
| **Threshold**     | "p95 latency > 800 ms".                                    |
| **Change**        | "Error rate increased >50% vs an hour ago".                |
| **Anomaly**       | Deviates from learned seasonal pattern.                    |
| **Outliers**      | One service/host/version behaves differently from peers.   |
| **Forecast**      | Predict crossing a threshold soon.                         |
| **Composite**     | Combine multiple monitors with boolean logic.              |

### Anomaly Monitor

Detects when the metric deviates by N standard deviations from prediction (above, below, or both) for a configurable duration. Robust against seasonality.

### Watchdog Alerts

Watchdog scans all services for anomalies on **error rate, latency, and hits**. It only alerts on services with at least **0.5 req/s** to suppress noise. Anomalies on hits without latency/error impact are ignored. The output is a "Story" summarizing root-cause clues (anomalous tags, related deploys, etc.).

## Notifications

- Notifications are markdown messages with template variables (`{{service.name}}`, `{{value}}`).
- Use `@` mentions to route (`@slack-ops`, `@pagerduty-payments`, `@team-payments`).
- Conditional message blocks: `{{#is_alert}}…{{/is_alert}}`.
- Set escalation paths via no-data, recovery, and renotification rules.

## SLO-based alerting

Define an **SLO** (e.g., "99.9% of /checkout requests under 500ms") and alert on:

- Error budget burn rate (fast and slow burn windows).
- Forecasted SLO miss.

Useful for pure customer-impact alerting that's resilient to noise.

## Monitor downtimes

Schedule a **downtime** when you know an alert will fire (deploys, maintenance) so it doesn't notify. Downtimes can be one-off or recurring (cron-style).

## Things to remember for the exam

- **Trace metrics** drive APM Metric monitors and are 100% of ingested data.
- **APM Trace Analytics monitors** query individual indexed spans (good for low-volume specific queries).
- **Watchdog** is automatic anomaly detection; thresholds are managed by Datadog.
- **Anomaly monitor** is a user-defined statistical alert with seasonality awareness.
- Default automatic error-rate alert threshold is **10%**.
- Use **downtimes** to silence monitors during planned maintenance.
