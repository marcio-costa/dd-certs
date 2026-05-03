# Daily Mini-Quiz #01 (10 Qs)

> Mixed across all four domains. ~10 minutes.

---

**Q1.** The trace-agent's default TCP port for receiving traces is:

A) 8125   B) 8126   C) 4317   D) 9092

---

**Q2.** Unified Service Tagging tags are:

A) `app`, `tier`, `region`
B) `env`, `service`, `version`
C) `team`, `cluster`, `pod`
D) `host`, `container`, `namespace`

---

**Q3.** Live Search covers the last:

A) 5 minutes   B) 15 minutes   C) 1 hour   D) 24 hours

---

**Q4.** Custom retention filters keep matching indexed spans for:

A) 1 hour   B) 24 hours   C) 15 days   D) 90 days

---

**Q5.** The default Continuous Profiler visualization is the:

A) Heat map   B) Pie chart   C) Flame graph   D) Sankey

---

**Q6.** Which env var enables trace-log correlation?

A) `DD_TRACE_LOG_INJECT`
B) `DD_LOGS_INJECTION`
C) `DD_LOG_CORRELATE`
D) `DD_TRACE_LINK_LOGS`

---

**Q7.** A custom span tag query in the Trace Explorer uses:

A) `#tag:value`
B) `$tag:value`
C) `@tag:value`
D) `tag=value`

---

**Q8.** Error Tracking groups errors by:

A) IP address
B) `error.type` + `error.message` + stack frames (fingerprint)
C) `service` and `host` only
D) HTTP status code only

---

**Q9.** The default automatic APM error-rate alert threshold is:

A) 1%   B) 5%   C) 10%   D) 25%

---

**Q10.** Which is the canonical W3C trace context header?

A) `x-trace-id`
B) `traceparent`
C) `x-datadog-trace-id`
D) `b3-trace`

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | B | 1.2 |
| 2 | B | 1.3 |
| 3 | B | 1.4 |
| 4 | C | 1.4 |
| 5 | C | 3.4 |
| 6 | B | 4.3 |
| 7 | C | 3.2 |
| 8 | B | 3.6 |
| 9 | C | 4.2 |
| 10 | B | 4.3 |
