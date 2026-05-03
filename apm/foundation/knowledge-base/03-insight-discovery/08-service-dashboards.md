# Service Performance Dashboards & Trace Metrics

> **Domain 3 — Insight Discovery**

## Out-of-the-box (OOTB) Service Dashboard

Every APM-instrumented service automatically gets a **Service Page** with default visualizations:

| Section                | Charts                                                                   |
| ---------------------- | ------------------------------------------------------------------------ |
| **Overview**           | Requests/sec, error rate, latency (p50/p75/p90/p95/p99/max).             |
| **Top Resources**      | Highest-traffic endpoints, sortable by hits/errors/latency.              |
| **Errors**             | Error rate by resource and error type.                                   |
| **Apdex / Latency**    | Latency distribution and Apdex score over time.                          |
| **Runtime Metrics**    | JVM/Python/Node runtime metrics (when enabled).                          |
| **Profile**            | Embedded profiler view.                                                  |
| **Deployments**        | Version timeline + comparisons.                                          |
| **Span Summary**       | Per-span breakdown (see Span Summary doc).                               |

Watchdog overlays anomaly bands on the requests, latency and error charts automatically.

## Trace Metrics

For every operation name, Datadog computes these standard metrics from **100% of ingested traces** and retains them **15 months**:

| Metric                                | Description                                |
| ------------------------------------- | ------------------------------------------ |
| `trace.<operation>.hits`              | Count of spans (by service/resource/etc.). |
| `trace.<operation>.errors`            | Count of error spans.                      |
| `trace.<operation>.duration`          | Distribution of span durations.            |
| `trace.<operation>.duration.by.service` | Same, split by service.                  |

Trace metrics are queried like normal metrics in dashboards, monitors, and notebooks.

```text
trace.http.request.errors{service:checkout,env:prod}.as_rate()
```

## Custom span-based metrics

Beyond OOTB trace metrics, you can create **custom metrics from spans**:

- Define a query (e.g. `service:checkout @order.value:>0`).
- Choose grouping tags (`env`, `version`, `country`).
- Choose aggregation (count, p95 of `@duration`, sum of `@order.value`).

Custom span metrics:

- Are computed at ingest time (work for any tag, indexed or not).
- Retained **15 months** like standard metrics.
- **Billable** as custom metrics (count distinct combinations of tag values).

## Custom dashboards from APM

- The **Service Summary** widget shows hits/errors/latency for any service.
- The **Profiling Flame Graph** widget pins flame graphs.
- The **Trace Search** widget shows live trace results.
- Standard timeseries widgets accept any `trace.*` metric.

## Things to remember for the exam

- The Service Page is generated **automatically** when APM is on.
- `trace.<op>.hits / .errors / .duration` are the canonical trace metrics — retained **15 months**, computed from **100%** of ingested data.
- Custom span-based metrics let you create metrics from any span attribute.
- Watchdog anomaly overlays appear on Requests, Latency, and Error rate charts by default.
