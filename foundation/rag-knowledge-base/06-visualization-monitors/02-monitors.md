# Domain 6.B — Monitors and Alerting

> **Source docs:** `/monitors/`, `/monitors/types/`, `/monitors/configuration/`, `/monitors/notify/`

## 6.B.1 Monitor Types (Highly Tested)

| Monitor type | What it watches | Typical example |
|---|---|---|
| **Metric** | Threshold or change in a metric query. | Alert when `avg(last_5m): system.cpu.user{env:prod} > 90`. |
| **Anomaly** | Metric deviating from learned baseline. | "Logins are abnormally low for 9am Tuesday." |
| **Forecast** | Predicted future value crosses a threshold. | "Disk will hit 90% in the next 4 hours." |
| **Outlier** | One member of a group diverging from peers. | "One Kafka broker has higher consumer lag than the others." |
| **Change** | Absolute or relative change vs. a prior window. | "Latency 50% higher than 1 week ago." |
| **Composite** | Boolean combo of other monitors. | `(monitor A) && (monitor B)`. |
| **Service Check** | State of a service check. | `postgres.can_connect` is CRITICAL. |
| **Process** | Number of running processes matching a pattern. | "nginx process count below 1." |
| **Network** | TCP/HTTP reachability via the Agent. | "Port 443 unreachable from edge nodes." |
| **Custom Check** | Result of a custom Agent check. | Application heartbeat. |
| **Event / Event V2** | Event matching a query fires. | "AWS Auto Scaling event in prod." |
| **Logs** | Log query crosses a threshold. | "More than 100 ERROR logs in 5 min." |
| **APM Trace Analytics** | Span/trace query crosses a threshold. | "p95 latency > 500ms for `/checkout`." |
| **Error Tracking** | New issue or spike in an error issue. | "New issue in payment-service." |
| **RUM** | Real-User Monitoring query. | "JS errors per session > 5." |
| **Synthetic** | Outcome of a Synthetic test. | "Browser test failed in 2 of 3 locations." |
| **CI Pipelines** | CI metrics. | "Build failure rate > 20%." |
| **Watchdog** | Watchdog (anomaly) story fires. | (auto-detected). |
| **Audit Trail** | Datadog audit events. | "API key created outside of business hours." |
| **SLO** | SLO error budget burn. | "30-day error budget consumed > 75%." |
| **Database Monitoring** | Query metrics. | "p95 query latency on orders DB > 1s." |
| **CSPM / CWS / Cloud SIEM** | Security signals. | "New CIS benchmark violation." |

## 6.B.2 Monitor Anatomy

A monitor has these parts:

1. **Detect** — query and threshold (or anomaly/forecast/outlier).
2. **Notify** — message body (markdown, with template variables) sent to handles.
3. **Restrictions** — which roles can edit, mute, etc.

### Thresholds
- **Critical** — alert state. Required.
- **Warning** — optional softer state.
- **Critical recovery / Warning recovery** — optional, lets you recover at a different threshold (hysteresis to prevent flapping).
- **No Data** — fires when data hasn't been received. Configurable.

### Evaluation window
- `avg(last_5m):` — the **aggregation** function applied to data **over the last 5 minutes**.
- Other windows: `last_1m`, `last_5m`, `last_10m`, `last_15m`, `last_30m`, `last_1h`, `last_2h`, `last_4h`, `last_1d` (varies by monitor type).
- `min()` (sustained — alert only if the value crossed the threshold for the entire window).
- `max()` (peaks — alert if it crossed at any point).
- `change()`, `pct_change()` for change monitors.

### `as_count()` vs. `as_rate()`
For `count`-typed metrics, choose how the monitor evaluates:
- `.as_count()` — total count over each minute bucket (use for "more than 100 errors in 5 min").
- `.as_rate()` — per-second rate (use for "more than 5 errors/sec").

## 6.B.3 Notifications

Monitor messages support **markdown** and **template variables**.

Common template variables:
- `{{value}}` — current value of the metric.
- `{{threshold}}` / `{{warn_threshold}}` — configured threshold.
- `{{host.name}}`, `{{kube_namespace.name}}`, `{{service.name}}` — group context (depends on `by` clause).
- `{{#is_alert}} ... {{/is_alert}}` — conditional block fired on alert.
- `{{#is_warning}} ... {{/is_warning}}`, `{{#is_recovery}} ... {{/is_recovery}}`, `{{#is_no_data}} ... {{/is_no_data}}`.

### Notification handles
- `@username` — Datadog user.
- `@team-name` — Datadog team.
- `@email@example.com` — direct email.
- `@slack-<workspace>-<channel>` — Slack integration.
- `@pagerduty`, `@pagerduty-<service>` — PagerDuty.
- `@opsgenie`, `@victorops`, `@webhook-<name>` — others.
- `@jira-<integration>` — auto-create Jira ticket.

### Renotification & escalation
- `notify_audit` — also notify on changes to the monitor itself.
- `renotify_interval` — re-page every N minutes if still alerting.
- `renotify_occurrences` — limit how many re-pages.
- `escalation_message` — separate message used for renotifications.

## 6.B.4 Multi-Alert (Group-Aware) Monitors

When you add a `by {tag}` clause, the monitor evaluates **per group** and can alert per host/service/etc.

```
avg(last_5m): avg:system.cpu.user{env:prod} by {host} > 90
```

This becomes one **multi-alert** that creates one notification per `host` group that crosses the threshold. Recoveries are also per group.

## 6.B.5 Downtime

A **downtime** mutes monitors during a planned window. Reasons: maintenance, deployments, batch jobs.

You define:
- **Scope** (e.g., `env:staging` or specific monitor IDs).
- **Schedule** — start, duration, optional **RRULE** for recurrence (`FREQ=WEEKLY;BYDAY=SA,SU`).
- **Notify-on-end** behavior.
- Optional **mute first recovery** to suppress the noise that occurs when downtime ends.

Downtimes can be created in the UI (*Monitors → Manage Downtimes*) or via API:
```bash
curl -X POST "https://api.datadoghq.com/api/v2/downtime" \
  -H "DD-API-KEY: $DD_API_KEY" \
  -H "DD-APPLICATION-KEY: $DD_APP_KEY" \
  -H "Content-type: application/json" \
  -d '{
    "data": {"type": "downtime",
      "attributes": {
        "monitor_identifier": {"monitor_tags": ["env:staging"]},
        "scope": "env:staging",
        "schedule": {
          "timezone": "UTC",
          "recurrences": [{
            "start": "2026-05-04T22:00",
            "duration": "8h",
            "rrule": "FREQ=DAILY;INTERVAL=1"
          }]
        }
      }
    }
  }'
```

## 6.B.6 SLOs

A **Service Level Objective** is a target for a service's reliability over a time window.

| SLO type | Numerator / denominator | When to use |
|---|---|---|
| **Metric-based** | `sum:requests.success{...}.as_count()` / `sum:requests.total{...}.as_count()` | Custom or APM-derived ratios. |
| **Monitor-based** | Time-based fraction of time a monitor was OK. | Fewer signals; you already have a "good" monitor. |
| **Time-slice** (newer) | Fraction of minutes meeting a condition (e.g., latency < threshold). | Pure SLI from a metric without needing two queries. |

Each SLO has:
- A **target** (e.g., 99.9%).
- A **time window** — `7d`, `30d`, `90d`, or rolling/calendar.
- A **threshold** for an associated monitor (often).

### Error budget
The complement of the SLO — for a 99.9% target over 30d, the **budget** is 0.1% of 30d ≈ 43 minutes of downtime.

You can create an **error-budget alert** monitor:
```
error_budget("<slo_id>").over("30d") > 75
```
This fires when more than 75% of the budget is consumed.

## 6.B.7 Monitor Status Page

`/monitors/manage` lists all monitors with current state, last triggered, mutes, edit shortcuts. Use saved views to filter (e.g., "All monitors in `env:prod` not muted, alerting").

The `/monitors/<id>` page shows the metric query overlay, a timeline of state changes, audit history (who changed what), notifications sent, and a list of triggered groups.
