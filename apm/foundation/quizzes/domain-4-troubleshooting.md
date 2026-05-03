# Quiz — Domain 4: Troubleshooting Application using APM (15 Qs)

> Single-answer multiple choice. Answers at the bottom.

---

**Q1.** During a live incident, the **first** APM view to open is typically:

A) Watchdog Stories list
B) Service Page for the affected service, scoped to the incident time window
C) Software Catalog map
D) Notebook editor

---

**Q2.** The "Critical Path" feature on a trace highlights:

A) The first error span only.
B) The longest causal path from root to leaf.
C) The path with the most child spans.
D) The frontend → backend hop.

---

**Q3.** A latency monitor should be configured to fire when p95 exceeds 800 ms. Which alert condition fits best?

A) Threshold
B) Anomaly
C) Forecast
D) Outliers

---

**Q4.** Watchdog automatically scans services for anomalies on:

A) Disk I/O and CPU usage
B) Error rate, latency, and hits
C) Memory leaks
D) Container restarts

---

**Q5.** Watchdog suppresses anomaly detection for low-traffic endpoints below approximately:

A) 0.05 req/s
B) 0.5 req/s
C) 50 req/s
D) 5000 req/s

---

**Q6.** Which monitor type detects when a metric deviates from its learned seasonal pattern?

A) Threshold
B) Anomaly
C) Outliers
D) Forecast

---

**Q7.** The default error-rate threshold for an automatic APM error monitor is:

A) 1%
B) 5%
C) 10%
D) 50%

---

**Q8.** To silence a monitor during a planned maintenance window, you should:

A) Delete the monitor.
B) Schedule a downtime.
C) Decrease the threshold.
D) Mute the host.

---

**Q9.** The simplest way to enable trace-log correlation for supported tracers is:

A) Use the Datadog Forwarder.
B) Set `DD_LOGS_INJECTION=true` so trace/span IDs are injected into log lines.
C) Manually add `dd.trace_id` to every log call.
D) Use a custom log pipeline.

---

**Q10.** Which header is part of the W3C trace context propagation standard?

A) `x-trace-id`
B) `traceparent`
C) `dd-trace-id`
D) `x-correlation-id`

---

**Q11.** A user opens a slow trace and clicks the Code Hotspots tab. They see:

A) The error stack trace.
B) Profiler samples taken during that specific trace.
C) Recent log lines from any service.
D) Network packet capture.

---

**Q12.** Which monitor type alerts on individual indexed spans matching a specific tag query?

A) APM Metric Monitor
B) APM Trace Analytics Monitor
C) Composite Monitor
D) Live Process Monitor

---

**Q13.** A service emits structured JSON logs but trace IDs don't appear in Datadog logs. The MOST likely fix is:

A) Set `DD_TRACE_ENABLED=true`.
B) Enable `DD_LOGS_INJECTION=true` (or include the IDs in your logging pattern) and ensure logs are ingested by Datadog.
C) Increase log retention.
D) Add a retention filter for traces.

---

**Q14.** Which feature shows AI-generated narratives explaining anomalies and likely causes?

A) Notebooks
B) Watchdog Stories
C) Error Tracking issues
D) Live Tail

---

**Q15.** During an incident at 2:00 AM, you need to check what was happening 12 hours ago in the Trace Explorer. Which mode are you in?

A) Live Search.
B) Indexed (Retained) Search; you need traces to have been kept by a retention filter.
C) Trace metrics view only.
D) Notebooks.

---

## Answer Key

| # | Answer | Explanation |
|---|--------|-------------|
| 1 | **B** | The Service Page is purpose-built for fast triage during an incident. |
| 2 | **B** | Critical Path = longest causal path from root to leaf — the most useful one-click drill-down. |
| 3 | **A** | A simple "fire when p95 > 800ms" is a classic threshold alert. |
| 4 | **B** | Watchdog watches error rate, latency, and hits (request rate). |
| 5 | **B** | Watchdog requires roughly 0.5 req/s before monitoring an endpoint to suppress noise. |
| 6 | **B** | Anomaly monitors are specifically built for seasonal-aware deviation detection. |
| 7 | **C** | Default automatic APM error-rate threshold is 10%, configurable per service. |
| 8 | **B** | Downtimes silence monitors for a defined window without deleting them. |
| 9 | **B** | `DD_LOGS_INJECTION=true` is the canonical one-line solution. |
| 10 | **B** | `traceparent` is the W3C standard. Datadog's native header is `x-datadog-trace-id`. |
| 11 | **B** | Code Hotspots correlates trace spans to profile samples taken during that trace. |
| 12 | **B** | APM Trace Analytics monitors evaluate queries against indexed spans. |
| 13 | **B** | `DD_LOGS_INJECTION=true` (or a logging pattern that includes the IDs) is the standard fix. |
| 14 | **B** | Watchdog Stories are the AI-generated incident narratives. |
| 15 | **B** | Anything beyond 15 min flips to Indexed Search; the trace must have been retained. |
