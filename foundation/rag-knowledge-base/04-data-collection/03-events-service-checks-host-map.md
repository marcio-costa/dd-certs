# Domain 4.C — Events, Service Checks, Host Map & Infrastructure List

> **Source docs:** `/events/`, `/monitors/types/service_check/`, `/infrastructure/hostmap/`, `/infrastructure/`

## 4.C.1 Events

An **event** is a discrete, timestamped record of something that happened in your environment.

### Sources
- Built-in integrations (e.g., AWS CloudTrail, Kubernetes, Chef, Puppet, Docker, GitHub).
- DogStatsD (`_e{...}` datagram).
- Agent custom checks (`self.event(...)`).
- REST API (`POST /api/v1/events`).
- Datadog itself (monitor alerts, deployment markers).

### Anatomy
```python
self.event({
    "timestamp": int(time.time()),
    "event_type": "deploy",
    "msg_title": "Deployed checkout v1.2.3",
    "msg_text":  "Rolled out via Argo CD",
    "alert_type": "info",          # info | warning | error | success
    "priority":   "normal",        # normal | low
    "tags":       ["service:checkout", "env:prod"],
    "aggregation_key": "checkout-deploys",
    "source_type_name": "my-deploy-tool"
})
```

### Where you see events
- **Events Explorer** (`/event/explorer`).
- As **overlays** on dashboard graphs (toggle "Events" in the widget).
- As triggers for **Event monitors** (alert when an event matching a query fires).

## 4.C.2 Service Checks

A **service check** is a periodic up/down/degraded indicator for a specific check, with these states:

| Code | State | Meaning |
|---|---|---|
| 0 | OK | Healthy. |
| 1 | WARNING | Degraded but not down. |
| 2 | CRITICAL | Down / failing. |
| 3 | UNKNOWN | Could not determine. |

Examples shipped by integrations:
- `postgres.can_connect`
- `nginx.can_connect`
- `kubernetes.kubelet.check`
- `datadog.agent.up`

You can alert on a service check using a **Service Check monitor** that triggers on transitions between states.

## 4.C.3 Host Map

The **Host Map** (`/infrastructure/map`) is a hexagonal heatmap visualization where every hex is a host, sized and colored by metrics, grouped/filtered by tags. Useful for:
- Spotting outliers (one slow host in a fleet).
- Comparing fleets across regions/clusters.
- Drilling into a single host's dashboard.

Common configurations:
- **Group by:** `availability-zone`, `cluster`, `service`.
- **Color by:** `system.cpu.user`, `system.mem.used`, `system.load.norm.1`.
- **Size by:** number of containers, request rate, etc.

## 4.C.4 Infrastructure List

The **Infrastructure List** (`/infrastructure`) is a table view of every host/container/service known to Datadog with:
- Hostname, OS, cloud provider, status (up / no Agent / Agent only / muted).
- Apps detected on the host (Postgres, NGINX, …).
- Latest tag values.
- Direct link to the host dashboard.

### "Active" vs. "Up"
A host counts as **active** if it has reported metrics in the last 2 hours. The active count drives **infrastructure billing**.

### Container view
The same area exposes **Containers** and **Processes** views (Live Containers and Live Processes), provided `process_config` is enabled in the Agent.

## 4.C.5 Tying It Together — A Mental Model

For the exam, picture how these data types differ:

| Data type | Frequency | Cardinality | Best for |
|---|---|---|---|
| **Metric** | High (every flush interval) | Bounded by tag combos | Trends, dashboards, monitors |
| **Event** | Low | Open-ended | Discrete things to remember (deploys, alarms) |
| **Service check** | Periodic (per check interval) | Bounded by integration | "Is X up?" alerting |
| **Log** | Very high | Unbounded | Per-event detail, debugging |
| **Trace/span** | High (per request) | Unbounded | Per-request latency, dependencies |

The Agent transports all of them; the UI exposes them in dedicated explorers, but they share **tags** so they can all be correlated.
