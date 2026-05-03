# Domain-Specific Drill — Domain 4: Troubleshooting Application using APM

> 12 questions, ~15 minutes. Take in Week 4.

---

**Q1.** During an incident, the FIRST APM page to open is typically:

A) Watchdog Stories list.
B) Service Page for the affected service, scoped to the incident time window.
C) Notebooks editor.
D) Logs Explorer with no filter.

---

**Q2.** "Critical Path" in the trace waterfall view highlights:

A) The first error span.
B) The longest causal path from root to leaf.
C) The path with the most child spans.
D) The slowest database query.

---

**Q3.** Which monitor type alerts when p95 latency exceeds 800 ms?

A) Threshold alert on the trace metric `trace.<op>.duration` (p95 evaluator).
B) Anomaly alert.
C) Forecast alert.
D) Outlier alert.

---

**Q4.** Watchdog automatically scans services for anomalies on:

A) Disk and memory only.
B) Error rate, latency, and hits (request rate).
C) Container restarts.
D) Network latency between hosts.

---

**Q5.** Watchdog suppresses anomaly detection for endpoints below approximately:

A) 0.05 req/s
B) 0.5 req/s
C) 5 req/s
D) 50 req/s

---

**Q6.** Which monitor type is best for "the metric deviates from its learned weekly seasonal pattern"?

A) Threshold
B) Anomaly
C) Forecast
D) Outlier

---

**Q7.** Default automatic APM error-rate threshold is:

A) 1%
B) 5%
C) 10%
D) 25%

---

**Q8.** A team wants to **silence** an alert during a planned database migration. The right tool is:

A) Delete the monitor.
B) Schedule a downtime.
C) Decrease the threshold.
D) Mute the host.

---

**Q9.** Which env var is the simplest way to enable trace-log correlation?

A) `DD_TRACE_LOG_INJECT`
B) `DD_LOGS_INJECTION`
C) `DD_LOG_CORRELATE`
D) `DD_LOGS_TRACE_ID`

---

**Q10.** Which header is the W3C standard for trace context propagation?

A) `x-datadog-trace-id`
B) `traceparent`
C) `x-correlation-id`
D) `b3`

---

**Q11.** A user opens a slow trace and clicks "Code Hotspots". They see:

A) Recent log lines from the entire app.
B) Profiler samples taken during this specific trace.
C) The error stack trace only.
D) Network packet details.

---

**Q12.** Which monitor type queries individual indexed spans (rather than aggregated trace metrics)?

A) APM Metric Monitor.
B) APM Trace Analytics Monitor.
C) Composite Monitor.
D) Service-level Monitor.

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | B | 4.1 |
| 2 | B | 4.1 |
| 3 | A | 4.2 |
| 4 | B | 4.2 |
| 5 | B | 4.2 |
| 6 | B | 4.2 |
| 7 | C | 4.2 |
| 8 | B | 4.2 |
| 9 | B | 4.3 |
| 10 | B | 4.3 |
| 11 | B | 3.4 / 4.1 |
| 12 | B | 4.2 |
