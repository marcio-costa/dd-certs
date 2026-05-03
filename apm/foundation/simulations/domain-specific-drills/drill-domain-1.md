# Domain-Specific Drill — Domain 1: APM Fundamentals

> 15 questions, ~20 minutes. Take this at the end of Week 1.

---

**Q1.** Which of the following best describes the role of a **service** in Datadog APM?

A) A single host that runs an application.
B) A logical group of processes performing the same function (e.g. `payment-api`).
C) A unique transaction ID for one request.
D) A collection of dashboards for one team.

---

**Q2.** Which of these is a **resource** in Datadog APM terminology?

A) The Datadog Agent process.
B) `GET /checkout` (a specific endpoint within a service).
C) A custom metric like `app.signups`.
D) A Kubernetes pod.

---

**Q3.** Which is **true** about distributed tracing context propagation?

A) Each service generates a fresh trace ID for every request.
B) Trace IDs are passed between services (typically via HTTP headers) so spans can be stitched together.
C) Datadog backend correlates spans by host name only.
D) Trace context is only required for synchronous calls.

---

**Q4.** A team uses Agent 7.20 in Docker. By default, will the trace-agent accept traces from app containers?

A) No — `DD_APM_NON_LOCAL_TRAFFIC` defaults to `false`.
B) Yes — `DD_APM_NON_LOCAL_TRAFFIC` defaults to `true` since Agent 7.18.
C) Only if `DD_APM_RECEIVER_PORT` is changed.
D) Only with the OTel Collector path.

---

**Q5.** Which scenario best matches the **OpenTelemetry SDK + Collector + Datadog exporter** architecture?

A) An organization standardized on OpenTelemetry and wants vendor-neutral instrumentation.
B) A team wants the lowest setup time for Datadog APM.
C) A team uses the Datadog Lambda Extension.
D) A team only collects custom metrics.

---

**Q6.** Which set of tags is the foundation of correlation across APM, logs, processes, and infrastructure metrics in Datadog?

A) `cluster`, `pod`, `node`
B) `env`, `service`, `version`
C) `app`, `team`, `cost-center`
D) `region`, `zone`, `cell`

---

**Q7.** A Kubernetes Pod manifest sets `tags.datadoghq.com/version: "2025-10"`. What is the effect on APM traces?

A) The Agent rejects spans without an explicit `DD_VERSION`.
B) The version tag is automatically attached to APM data when USM is configured.
C) The label is ignored unless the Cluster Agent has `DD_TAG_LABELS` set.
D) The version tag only applies to logs.

---

**Q8.** **True or false**: Trace metrics like `trace.web.request.duration` are computed only after retention filters are applied.

A) True — only retained spans contribute to trace metrics.
B) False — trace metrics are computed from 100% of ingested spans, before retention.
C) True — only error spans contribute to trace metrics.
D) False — trace metrics are sampled from 10% of spans.

---

**Q9.** A user expects to find a specific trace from yesterday in the Trace Explorer but can't. Which is the MOST LIKELY cause?

A) The trace was indexed but Live Search is enabled.
B) The trace was ingested but not retained by any retention filter.
C) The Datadog backend is down.
D) The trace ID was malformed.

---

**Q10.** Which language requires importing contrib packages (e.g., `httptrace`, `chitrace`) instead of bytecode-level auto-instrumentation?

A) Java
B) Python
C) Go
D) PHP

---

**Q11.** The **intelligent retention filter** is best described as:

A) A user-defined filter that keeps spans for 15 days.
B) A Datadog-managed filter that keeps representative spans for 30 days to support Watchdog and Error Tracking.
C) A filter that drops PII tags automatically.
D) A filter that prioritizes high-cardinality tags.

---

**Q12.** Which is the **default port** the trace-agent listens on for app traces, and which protocol?

A) 8125 / UDP
B) 8126 / TCP
C) 4317 / gRPC
D) 443 / TCP

---

**Q13.** A community tracer in the Datadog ecosystem most commonly refers to:

A) A Datadog `dd-trace-*` library.
B) An OpenTelemetry SDK (or third-party tracer) that emits Datadog-compatible spans.
C) A Datadog Cluster Agent component.
D) The DogStatsD client.

---

**Q14.** A trace in Datadog APM is best described as:

A) A single function call with metadata.
B) A directed graph of spans representing one end-to-end request, possibly across many services.
C) A continuous metric stream from the application.
D) A log line with a unique ID.

---

**Q15.** Which is **true** about a span?

A) A span always has children.
B) A span represents a unit of work (e.g., HTTP call, DB query, function call) with start/end times.
C) A span is always 1 ms or longer.
D) Spans cannot have custom tags.

---

## Answer Key

| # | Answer | KB §  |
|---|--------|-------|
| 1 | B | 1.1 |
| 2 | B | 1.1 |
| 3 | B | 1.1 |
| 4 | B | 2.3 |
| 5 | A | 1.2 |
| 6 | B | 1.3 |
| 7 | B | 1.3 |
| 8 | B | 1.4 / 2.4 |
| 9 | B | 1.4 / 3.3 |
| 10 | C | 1.5 |
| 11 | B | 1.4 |
| 12 | B | 1.2 / 2.3 |
| 13 | B | 1.5 |
| 14 | B | 1.1 |
| 15 | B | 1.1 |
