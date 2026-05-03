# Trace ↔ Log ↔ Other Telemetry Correlation

> **Domain 4 — Troubleshooting Application using APM**

## What it gives you

- Open a slow trace → see exactly the log lines emitted during it.
- Open a log line → jump to the trace it belongs to.
- Open a trace → see infrastructure metrics for the host/container that ran it.
- Open a slow span → see the profile sample taken during it.

## Trace ↔ Log correlation

Mechanism: the tracer **injects** the active trace and span IDs into log lines (as MDC/structured attributes), and Datadog Log Management indexes those attributes.

| Setting                      | Effect                                                            |
| ---------------------------- | ----------------------------------------------------------------- |
| `DD_LOGS_INJECTION=true`     | Tracer auto-injects `dd.trace_id`, `dd.span_id` into logs.        |
| Logger pattern includes the IDs | Required if you build the log line yourself.                   |
| Logs ingested by Datadog     | Required so Datadog can match the IDs.                            |

For **JSON logs**, fields appear as:

```json
{"timestamp":"...","level":"ERROR","message":"...","dd":{"trace_id":"...", "span_id":"..."}}
```

Java/Logback example pattern:

```
%d{ISO8601} %-5p [%X{dd.trace_id} %X{dd.span_id}] %logger - %msg%n
```

## Trace ↔ Infrastructure metrics

Spans automatically carry the host name, container ID, and Kubernetes metadata. Click a span → "View related host/container metrics".

Required:

- Agent collecting infra metrics on the same host.
- Container/Kubernetes integration enabled.

## Trace ↔ Profile

Already covered in the Profiler doc — the tracer tags every profile sample with the active trace/span IDs.

## Trace ↔ RUM

RUM SDK (browser/mobile) injects the trace ID on outgoing fetch/XHR requests via the `traceparent`/`x-datadog-trace-id` headers. The backend tracer continues the trace, so a single trace spans front-end + back-end.

| Setting                              | Effect                                                  |
| ------------------------------------ | ------------------------------------------------------- |
| `allowedTracingUrls` in RUM init     | Whitelist of backend hostnames to attach trace headers. |
| Server-side `dd-trace` library       | Continues the trace from RUM.                           |

## Trace ↔ Network performance

The Network Performance Monitoring product can correlate flow-level metrics with traces using the host/container linkage.

## Things to remember for the exam

- `DD_LOGS_INJECTION=true` is the simplest way to enable trace-log correlation in supported languages.
- IDs propagate as `dd.trace_id` / `dd.span_id` (or as a `dd` object in JSON logs).
- Trace context propagates over HTTP via headers like `traceparent` (W3C) and `x-datadog-trace-id` (Datadog format).
- All correlation flows depend on **shared identifiers** (trace IDs, host names, container IDs).
