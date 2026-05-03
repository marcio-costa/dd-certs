# Domain-Specific Drill — Domain 3: Insight Discovery

> 20 questions, ~25 minutes. Take at the end of Week 3.

---

**Q1.** A service appears in the Software Catalog when:

A) An `entity.datadog.yaml` file is committed to a repo connected via integration.
B) APM traces with a recognized `service` tag arrive in Datadog.
C) The OpenTelemetry resource attribute `service.name` is set.
D) Any of the above.

---

**Q2.** The Software Catalog map's two main layouts are:

A) Tree and Hierarchy
B) Cluster and Flow
C) Heatmap and Sankey
D) Service and Resource

---

**Q3.** Which Trace Explorer query finds spans where the duration was between 200 ms and 1 s?

A) `@duration:[200ms TO 1s]`
B) `@duration:>200ms AND @duration:<1s`
C) Both A and B work.
D) `duration:200..1000`

---

**Q4.** Live Search vs Indexed Search — which is **false**?

A) Live Search covers the last 15 minutes of ingested data.
B) Live Search can query any span tag, even non-indexed ones.
C) Indexed Search can query spans up to 15 days back if they were retained.
D) Indexed Search bypasses retention filters.

---

**Q5.** Which Trace Explorer query expresses "errors in the `cart` service in the new version `2025-10-01`"?

A) `cart status:error version:2025-10-01`
B) `service:cart status:error version:2025-10-01`
C) `service:cart status:error @version:2025-10-01`
D) `cart-2025-10-01-errors`

---

**Q6.** The **default visualization** in the Continuous Profiler is the:

A) Pie chart of frame counts.
B) Flame graph.
C) Sankey diagram of CPU.
D) Sparkline.

---

**Q7.** Which feature compares CPU profiles from two **time periods** or **two versions** side-by-side?

A) Live Tail
B) Profile Comparison view
C) Watchdog
D) Service Map

---

**Q8.** Auto-version (Agent 7.52+) priority order is:

A) Image tag > `DD_VERSION` > Git SHA
B) `DD_VERSION` > image tag > Git SHA
C) Git SHA > `DD_VERSION` > image tag
D) Hostname > `DD_VERSION` > image tag

---

**Q9.** Where do you compare metrics between two deployments side-by-side?

A) Trace Explorer.
B) Service Page → Deployments tab.
C) Software Catalog → Cost.
D) Notebooks only.

---

**Q10.** Error Tracking groups errors into **issues** by computing a fingerprint based on:

A) `error.type` + `error.message` + stack frames.
B) The user IP and timestamp.
C) The `service` and `env` tags only.
D) The HTTP status code only.

---

**Q11.** A team wants to override the default Error Tracking grouping for one specific exception. The recommended approach is:

A) Set the `error.fingerprint` span tag.
B) Use a `DD_ERROR_GROUPING` env var (doesn't exist).
C) Manually merge issues every time.
D) Disable Error Tracking for that service.

---

**Q12.** A previously-resolved Error Tracking issue reappears after a new deploy. Datadog will:

A) Create a duplicate issue.
B) Auto-reopen the existing issue.
C) Suppress the new occurrence.
D) Send a Watchdog alert only.

---

**Q13.** The Span Summary table reports for each span:

A) Only the average duration.
B) Avg occurrences/trace, % traces with the span, avg duration, avg % execution time.
C) Stack traces of every span instance.
D) Per-user count of spans.

---

**Q14.** Which statement about trace metrics is **true**?

A) They are sampled at 10%.
B) They are computed from 100% of ingested spans and retained 15 months.
C) They follow the format `apm.<service>.<metric>`.
D) They are deleted when retention filters change.

---

**Q15.** Which OOTB Service Page chart receives Watchdog anomaly overlays?

A) CPU usage.
B) Requests/sec, latency, error rate.
C) Disk I/O.
D) None — Watchdog appears only in Stories.

---

**Q16.** A custom span-based metric is billed as:

A) Free, like trace metrics.
B) A standard custom metric (charged per unique tag combination).
C) An indexed span.
D) An additional log line.

---

**Q17.** Which feature is the canonical visualization of service-to-service dependencies?

A) Notebook
B) Flame graph
C) Service Map
D) Hostmap

---

**Q18.** A user opens Trace Explorer with a **3-day window**. They are now in:

A) Live Search.
B) Indexed (Retained) Search; only spans kept by retention filters are visible.
C) Trace metrics view.
D) Logs Explorer.

---

**Q19.** Which of these can a Service Definition v2.2 specify? *(Pick the most complete answer)*

A) Service name only.
B) Service name, team, tier, languages, contacts, links, integrations, tags.
C) Just the source repository URL.
D) Only PagerDuty integration.

---

**Q20.** A team wants to keep all `POST /checkout` traces searchable for **15 days**. They should:

A) Add a custom retention filter matching `service:checkout resource_name:"POST /checkout"`.
B) Set `DD_TRACE_SAMPLE_RATE=1`.
C) Disable the intelligent retention filter.
D) Use Live Search only.

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | D | 3.1 |
| 2 | B | 3.1 |
| 3 | C | 3.2 |
| 4 | D | 3.3 |
| 5 | B | 3.2 |
| 6 | B | 3.4 / 2.6 |
| 7 | B | 3.4 |
| 8 | B | 3.5 |
| 9 | B | 3.5 |
| 10 | A | 3.6 |
| 11 | A | 3.6 |
| 12 | B | 3.6 |
| 13 | B | 3.7 |
| 14 | B | 3.8 / 1.4 |
| 15 | B | 3.8 |
| 16 | B | 3.8 |
| 17 | C | 3.1 |
| 18 | B | 3.3 |
| 19 | B | 3.1 |
| 20 | A | 1.4 / 2.4 |
