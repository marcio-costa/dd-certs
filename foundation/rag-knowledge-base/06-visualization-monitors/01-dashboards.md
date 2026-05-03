# Domain 6.A — Dashboards, Widgets, and Notebooks

> **Source docs:** `/dashboards/`, `/dashboards/widgets/`, `/notebooks/`

## 6.A.1 Dashboard Types

| Type | Layout | Best for | Replaces |
|---|---|---|---|
| **Timeboard** | Time-aligned graphs across the same time window. All widgets share a global time picker. | Troubleshooting; correlating multiple metrics over the same window. | (legacy term) |
| **Screenboard** | Free-form layout with widgets that can each have their own time scope. Drag widgets anywhere. | Status pages, executive views, mixed-time visualizations. | (legacy term) |
| **Dashboard (modern)** | Unified UI: you choose `Ordered` (timeboard-style grid) or `Free` (screenboard-style) layout when you create the dashboard. Both share template variables, group widgets, copy/paste, etc. | Everything since 2020. |

The current UI just calls them all **Dashboards**, with a layout toggle. The terms "timeboard" and "screenboard" survive in older docs and the legacy API.

## 6.A.2 Common Widget Types

| Widget | Use |
|---|---|
| **Timeseries** | Line/area/bar of one or more metrics over time. Default for most graphs. |
| **Top List** | Ordered table — top N hosts, services, etc., by a metric. |
| **Query Value** | Single big number (e.g., current 5xx rate). |
| **Heat Map** | Distribution over time (bucketed rows, time on X). |
| **Distribution** | Histogram of values within a time window. |
| **Hostmap** | Hex grid of hosts colored/sized by metrics (Domain 4). |
| **Toplist / Treemap** | Hierarchical breakdown by tag(s). |
| **SLO Summary / SLO List** | Status of SLOs. |
| **Alert Graph / Alert Value** | Visualize a monitor's underlying query and current state. |
| **Check Status** | Aggregated service-check state. |
| **Note**, **Image**, **Iframe**, **Markdown** | Annotations and embedded content. |
| **Log Stream** | Live logs matching a query. |
| **Trace List** | APM traces matching a query. |
| **Group** | Container that collapses other widgets. |
| **Powerpack** | Shareable group of widgets with template-variable wiring. |

## 6.A.3 Template Variables

Template variables let you parameterize every widget on a dashboard. Examples:

- `$env` — choose `prod`, `staging`, `dev`.
- `$service` — choose a service from APM.
- `$cluster`, `$region`, `$availability-zone`.

Defined at the top of the dashboard with:
- A **name** (`env`).
- A **prefix** (the tag key — `env`).
- An **available values** source (a tag value list, a metric, or APM service list).
- A **default**.

In a query, reference them with `$<name>`:
```
avg:system.cpu.user{env:$env, service:$service}
```

You can also use **template variable presets** to bookmark combinations of values.

## 6.A.4 Querying in a Widget

Datadog's metric query syntax has four parts:
```
<aggregator>:<metric_name>{<filter>}[by {<group>}].rollup(<method>, <interval>).<modifier>
```

Examples:
```
avg:system.cpu.user{env:prod} by {host}
sum:nginx.net.requests{service:web}.as_rate()
p95:trace.flask.request{service:checkout}.rollup(avg, 60)
top(avg:system.load.norm.1{env:prod} by {host}, 10, 'mean', 'desc')
```

Aggregators: `avg`, `sum`, `min`, `max`, `count_nonzero`, `count_not_null`, `percentile`. APM and distributions add `p50`, `p75`, `p90`, `p95`, `p99`.

Modifiers: `.as_rate()`, `.as_count()`, `.rollup(method, seconds)`, `.fill(linear|zero|null|last|previous)`, `.anomalies()`, `.forecast()`, `.outliers()`, `.derivative()`, `.diff()`, `.moving_rollup(...)`.

## 6.A.5 Sharing Dashboards

- **Share read-only link** — public URL with no login required (turn off when not needed; data is public).
- **Embed widget** — get an iframe snippet for a single widget.
- **Scheduled report** — email a PDF snapshot to a list of users.
- **Permissions** — restrict edit access to a role.

## 6.A.6 Notebooks

A **Notebook** is a long-form, runbook-style document mixing markdown, graphs, log queries, and trace queries. Use it for:
- **Investigations**: capture what you were looking at and why.
- **Postmortems**: stitch together graphs that show timeline of an incident.
- **Runbooks**: link from monitor messages.
- **Reports**: weekly/monthly performance write-ups with live charts.

Cells you can add: Markdown (with Mermaid for diagrams), Timeseries, Query Value, Top List, Distribution, Heat Map, Log Stream, Trace List, Funnel, Saved View.

You can **import a graph from a dashboard** straight into a notebook cell, then iterate without affecting the original dashboard.
