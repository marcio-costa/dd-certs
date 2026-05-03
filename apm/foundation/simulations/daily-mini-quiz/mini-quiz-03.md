# Daily Mini-Quiz #03 (10 Qs)

> Mixed. ~10 minutes.

---

**Q1.** A span is best described as:

A) A unit of work (function call, DB query, HTTP request) with start/end times.
B) A complete distributed trace.
C) A single host metric.
D) A log line.

---

**Q2.** Auto-version (Agent 7.52+) priority is:

A) Image tag > `DD_VERSION` > Git SHA
B) `DD_VERSION` > image tag > Git SHA
C) Git SHA > `DD_VERSION` > image tag
D) Image tag > Git SHA > `DD_VERSION`

---

**Q3.** Which env var enables the Continuous Profiler in most languages?

A) `DD_PROFILE_ON`
B) `DD_PROFILING_ENABLED`
C) `DD_TRACE_PROFILE`
D) `DD_PROFILER`

---

**Q4.** Where does SQL obfuscation for APM spans run?

A) In the application driver
B) In the trace-agent
C) In the Datadog backend storage
D) In DogStatsD

---

**Q5.** A team needs to redact a sensitive value across many span tags. The right knob is:

A) `DD_APM_REPLACE_TAGS`
B) `DD_TRACE_SCRUB`
C) `DD_TAG_OBFUSCATE`
D) `DD_SECURE_TAGS`

---

**Q6.** Which Trace Explorer query finds errors in `cart` for version `2025-10-01`?

A) `cart status:error version:2025-10-01`
B) `service:cart status:error version:2025-10-01`
C) `service:cart @status:err @version:2025-10-01`
D) `cart-2025-10-01-errors`

---

**Q7.** Watchdog Stories provide:

A) AI-generated narratives summarizing detected anomalies and clues.
B) A live tail of all production logs.
C) A list of all dashboards.
D) The trace explorer time picker.

---

**Q8.** The W3C standard trace context header is:

A) `x-datadog-trace-id`
B) `traceparent`
C) `b3`
D) `x-trace-id`

---

**Q9.** Which Service Page tab summarizes per-span occurrences and execution share?

A) Errors
B) Span Summary
C) Profile
D) Runtime Metrics

---

**Q10.** During an incident at 4:00 AM you need to look at traces from 6 hours ago. Which Trace Explorer mode are you in?

A) Live Search
B) Indexed (Retained) Search
C) Trace metrics view
D) Notebooks

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | A | 1.1 |
| 2 | B | 3.5 |
| 3 | B | 2.6 |
| 4 | B | 2.5 |
| 5 | A | 2.5 |
| 6 | B | 3.2 |
| 7 | A | 4.2 |
| 8 | B | 4.3 |
| 9 | B | 3.7 |
| 10 | B | 3.3 |
