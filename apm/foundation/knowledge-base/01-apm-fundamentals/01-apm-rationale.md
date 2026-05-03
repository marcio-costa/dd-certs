# APM Rationale & Datadog Approach

> **Domain 1 — APM Fundamentals**
> Source: docs.datadoghq.com (Context7 `/datadog/documentation`)

## Why APM exists

Modern apps are distributed: a single user request can fan out across dozens of services, queues, caches and databases. When something is slow or broken, traditional logs and host metrics alone can't tell you **which hop** caused it.

**Application Performance Monitoring (APM)** answers three questions:

1. **Where** is time spent in a request? (latency breakdown)
2. **What** failed and where did the error originate? (error attribution)
3. **How** is performance changing over time / across deployments? (trends and regressions)

## The three pillars of observability

| Pillar  | What it tells you                                  | Datadog product       |
| ------- | -------------------------------------------------- | --------------------- |
| Metrics | Aggregated, low-cardinality numbers (rate, count)  | Infrastructure / APM trace metrics |
| Traces  | The detailed lifecycle of a single request         | APM & Distributed Tracing |
| Logs    | Discrete events with rich context                  | Log Management        |

APM correlates all three using **unified service tags** (`env`, `service`, `version`).

## Datadog's approach to APM

- **One Agent, many telemetry types.** The Datadog Agent collects metrics, logs, and traces in a single binary (the trace-agent component listens on port `8126` by default).
- **Tracing libraries (`dd-trace-*`) inside the application** instrument code automatically and send spans to the local Agent.
- **The Agent forwards to Datadog backend**, computing trace metrics, applying sampling, and shipping spans.
- **Datadog UI** stitches spans into traces, builds the Service Map, populates the Software Catalog, generates Service Performance dashboards, runs Watchdog anomaly detection, and powers Error Tracking.

## Key terminology cheat sheet

| Term          | Definition                                                                 |
| ------------- | -------------------------------------------------------------------------- |
| **Span**      | A single unit of work (a function call, DB query, HTTP request).           |
| **Trace**     | A tree of spans representing one end-to-end request across services.       |
| **Service**   | A logical group of processes doing the same job (e.g. `payment-api`).      |
| **Resource**  | A particular action within a service (e.g. `GET /checkout`).               |
| **Operation** | The generic name of the work (e.g. `web.request`, `mysql.query`).          |
| **Trace context propagation** | Passing trace IDs between services (HTTP headers, message metadata) so spans can be stitched back together. |

## Things to remember for the exam

- The exam is non-programming, non-architecture, non-testing — it focuses on **using** Datadog APM.
- "Tracing" and "APM" are used interchangeably.
- Datadog's APM is built on top of distributed tracing — every conceptual question ultimately maps to traces, spans, services, resources, and tags.
