# Domain 4.A — Metrics: Types, Submission, and Behavior

> **Source docs:** `/metrics/types/`, `/metrics/custom_metrics/`, `/metrics/distributions/`

## 4.A.1 Submission Types vs. In-App Types

When you submit a metric, you choose a **submission type** (what your code calls). Datadog stores it as one of a few **in-app types** that determine how it behaves in queries.

| Submission type | Submission method | Resulting in-app type | What it represents |
|---|---|---|---|
| **COUNT** | `statsd.increment` / `self.count()` / `c` datagram / API `count` | COUNT (per-interval) | Total occurrences during the flush interval. Aggregated by sum. |
| **RATE** | `self.rate()` (Agent check) | GAUGE (rate/sec) | Time-normalized delta between submissions. |
| **GAUGE** | `statsd.gauge` / `self.gauge()` / `g` datagram / API `gauge` | GAUGE | Last reported value. Snapshot of "current state". |
| **HISTOGRAM** | `statsd.histogram` / `self.histogram()` / `h` datagram | Multiple GAUGE/RATE metrics: `.count`, `.avg`, `.median`, `.95percentile`, `.max`, `.min` | Statistical sample. Aggregated **per Agent (host)**. |
| **DISTRIBUTION** | `statsd.distribution` / `d` datagram / API `distribution` | DISTRIBUTION | Like histogram, but aggregated **globally on Datadog backend** — accurate global percentiles. |
| **SET** | `statsd.set` / `s` datagram | GAUGE (count of unique values) | Count of unique values seen during the interval. |
| **MONOTONIC_COUNT** | `self.monotonic_count()` (Agent check) | COUNT | Difference between submissions; ignores resets to zero. |
| **TIMER** | `ms` datagram | Same as HISTOGRAM | StatsD compatibility alias. |

### When to use which
- **GAUGE** for instantaneous values: queue length, temperature, connections open.
- **COUNT** for things you increment: requests served, errors raised, jobs processed.
- **RATE** when the source is itself a per-second rate.
- **HISTOGRAM** for per-host distributions: response latency on one server.
- **DISTRIBUTION** when you need global percentiles across many hosts (preferred for SLOs and APIs).
- **SET** for counts of unique strings (unique users, unique error codes) over an interval.

## 4.A.2 Submission Sources

Three ways data reaches the Datadog metrics intake:
1. **DogStatsD** — your app sends datagrams to local UDP 8125. Aggregated for ~10s, then flushed.
2. **Agent integration check** — runs on the schedule (default every 15s), uses `self.<type>()` calls.
3. **HTTP API** — `POST https://api.<site>/api/v2/series` with the metric type and points.

Direct API submission is useful for batch jobs, serverless, and ETL where running an Agent isn't practical.

## 4.A.3 Anatomy of a Metric Point

Every metric carries:
- **Name** — dot-namespaced, e.g., `myapp.checkout.duration`.
- **Value** — float.
- **Timestamp** — Unix epoch seconds.
- **Tags** — key:value strings (or just keys). Most query power comes from tags.
- **Host** — usually inferred from the Agent submitting it.
- **Type** — see above.
- **Unit** (optional) — declared in metadata; lets the UI show "ms", "bytes", "%", etc.

## 4.A.4 Custom Metrics — Cardinality and Billing

A **custom metric** = a unique combination of (metric name, host, tag values).

Each unique combination is one **metric context**, billed by your plan (commonly 100 custom metrics per host included; overage billed per 100 contexts).

### Example
```
myapp.requests{env:prod, region:us-east-1, endpoint:/api/orders} = 1 context
myapp.requests{env:prod, region:us-east-1, endpoint:/api/users}  = 1 context
myapp.requests{env:prod, region:us-west-2,  endpoint:/api/orders} = 1 context
```

If you add a high-cardinality tag like `user_id:42`, every unique user becomes its own context — costs explode quickly.

### Mitigations
- **Don't tag with unbounded values** (user IDs, request IDs, timestamps).
- Use **distributions** instead of per-host histograms to avoid host-as-cardinality-multiplier.
- Use **logs-to-metrics** for low-frequency aggregations from log data (Domain 6).

## 4.A.5 Standard / System Metrics

The Agent ships dozens of built-in **system** metrics from the OS:
- `system.cpu.user`, `system.cpu.system`, `system.cpu.idle`, `system.cpu.iowait`
- `system.mem.used`, `system.mem.free`, `system.mem.usable`
- `system.load.1`, `system.load.5`, `system.load.15`
- `system.disk.in_use`, `system.disk.read`, `system.disk.write`
- `system.net.bytes_rcvd`, `system.net.bytes_sent`
- `system.processes.number` and `system.processes.run`

These are **not** counted as custom metrics — they're free with any Agent install.

## 4.A.6 Events and Service Checks (Companion Data Types)

- **Events** are discrete, timestamped messages (deploys, scaling actions, alerts, comments). Show up on the Events Explorer and as overlays on dashboards. Sources: integrations, DogStatsD events, API.
- **Service checks** are one of `OK / WARN / CRIT / UNKNOWN`, scoped to a check name and tags. They power "X integration is up" monitors (e.g., `postgres.can_connect`). Sources: Agent checks, DogStatsD, API.

You can alert on metrics, events, and service checks (Domain 6).
