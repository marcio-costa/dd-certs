# Datadog Tracing Libraries

> **Domain 2 — Application Instrumentation**

## What a tracing library does

A Datadog tracing library, embedded in your app, performs four jobs:

1. **Creates spans** when instrumented code runs.
2. **Propagates trace context** across async boundaries and across services.
3. **Submits spans** to the local Datadog Agent (default `localhost:8126`).
4. **Collects runtime metrics** (heap, GC, thread counts) when enabled.

## Configuration knobs every candidate must know

| Env var                           | Purpose                                              |
| --------------------------------- | ---------------------------------------------------- |
| `DD_AGENT_HOST`                   | Hostname/IP of the Datadog Agent.                    |
| `DD_TRACE_AGENT_PORT`             | Port of the trace receiver (default `8126`).         |
| `DD_TRACE_AGENT_URL`              | Combined URL or UDS path (`unix:///var/run/datadog/apm.socket`). |
| `DD_TRACE_ENABLED`                | Master switch (default `true`).                      |
| `DD_SERVICE` / `DD_ENV` / `DD_VERSION` | Unified Service Tagging (USM).                  |
| `DD_TAGS`                         | Comma-separated additional global tags.              |
| `DD_TRACE_SAMPLE_RATE`            | Legacy single global sample rate (deprecated).       |
| `DD_TRACE_SAMPLING_RULES`         | Modern JSON array of per-service/resource rules.     |
| `DD_LOGS_INJECTION`               | Auto-injects `dd.trace_id`/`dd.span_id` into log records. |
| `DD_RUNTIME_METRICS_ENABLED`      | Enables runtime metrics (port `8125/UDP` to DogStatsD). |
| `DD_PROFILING_ENABLED`            | Enables Continuous Profiler.                         |
| `DD_TRACE_DEBUG`                  | Verbose tracer logs.                                 |

## How an app talks to the Agent

- **TCP**: `http://localhost:8126` — works in containers when the Agent is reachable on the host's network.
- **Unix Domain Socket (UDS)**: `unix:///var/run/datadog/apm.socket` — preferred in Kubernetes for performance/security.
- **In Kubernetes (DaemonSet Agent)**, set `DD_AGENT_HOST` to the **node IP** via the downward API:

  ```yaml
  env:
    - name: DD_AGENT_HOST
      valueFrom:
        fieldRef:
          fieldPath: status.hostIP
  ```

## Runtime metrics

When enabled, the tracer emits language-specific runtime metrics over **DogStatsD UDP/8125**:

- JVM: `jvm.heap_memory`, `jvm.gc.*`
- Python: `runtime.python.gc.*`, `runtime.python.thread_count`
- Node.js: `runtime.node.event_loop.*`
- .NET: `runtime.dotnet.*`

These are visible on the Service Page **Runtime Metrics** tab — useful for correlating slow traces with GC pauses, etc.

## Things to remember for the exam

- Most tracer config is set with `DD_*` environment variables.
- The application sends to the **Agent**, not directly to Datadog backend.
- **UDS** is the preferred socket type in Kubernetes.
- `DD_LOGS_INJECTION=true` is the canonical way to enable trace-log correlation in supported languages.
