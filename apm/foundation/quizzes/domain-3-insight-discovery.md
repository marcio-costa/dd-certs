# Quiz — Domain 3: Insight Discovery (23 Qs)

> Single-answer multiple choice. Answers at the bottom.

---

**Q1.** A service appears in the Software Catalog automatically when:

A) A `service.yaml` is committed to the source repo.
B) The service emits APM traces with a recognized `service` tag.
C) An admin manually adds it via the API.
D) The service is registered in PagerDuty.

---

**Q2.** Which Software Catalog map layout shows services grouped by team or tier?

A) Hierarchy
B) Cluster
C) Tree
D) Heatmap

---

**Q3.** What does the `tier: "1"` field in a Service Definition represent?

A) The version of the schema.
B) A criticality tier used for SLO and Watchdog prioritization.
C) The number of replicas in Kubernetes.
D) The Datadog billing plan tier.

---

**Q4.** Which query finds errors for service `payment-api` in the Trace Explorer?

A) `service=payment-api error=true`
B) `service:payment-api status:error`
C) `@service:payment-api @error:1`
D) `payment-api -ok`

---

**Q5.** Which operator finds traces with span duration over 1 second?

A) `@duration:>1s`
B) `duration>1s`
C) `@span.duration:gt:1`
D) `latency:>1s`

---

**Q6.** Live Search queries:

A) The last 15 minutes of ingested data, across all spans and tags.
B) Only error spans from the last hour.
C) The last 15 days of indexed spans.
D) Trace metrics for the chosen window.

---

**Q7.** Which is **true** about Indexed (Retained) Search compared to Live Search?

A) Indexed Search can query any tag, not just indexed ones.
B) Indexed Search has no time-window limit.
C) Indexed Search is limited to spans kept by retention filters.
D) Indexed Search bypasses retention filters.

---

**Q8.** The default visualization in the Continuous Profiler is the:

A) Heat map
B) Pie chart
C) Flame graph
D) Sankey diagram

---

**Q9.** Which feature lets you compare CPU profiles from two different deployment versions?

A) Live Tail
B) Profile Comparison view
C) Watchdog
D) Service Map

---

**Q10.** Auto-version (Agent 7.52+) priority order is:

A) Image tag > `DD_VERSION` > Git SHA
B) `DD_VERSION` > image tag > Git SHA
C) Git SHA > `DD_VERSION` > image tag
D) `DD_VERSION` > Git SHA > image tag

---

**Q11.** Where on the UI do you see metric deltas between two deployments?

A) Trace Explorer
B) Service Page → Deployments tab
C) Software Catalog → Cost
D) Monitor management page

---

**Q12.** Error Tracking groups errors by computing a **fingerprint** based on:

A) The host name and timestamp.
B) `error.type` + `error.message` + stack frames.
C) The `service` and `version` tags.
D) The HTTP status code only.

---

**Q13.** A team wants to override Error Tracking's default grouping for a specific exception. The best approach is:

A) Disable Error Tracking for that service.
B) Set the `error.fingerprint` span tag with a custom value.
C) Use a `DD_ERROR_GROUPING` env var.
D) Suppress the issue manually each time.

---

**Q14.** When a deployment introduces a previously **resolved** error, Error Tracking will:

A) Ignore it because it was resolved.
B) Re-open the issue automatically.
C) Create a duplicate issue.
D) Send a Watchdog alert only.

---

**Q15.** The Span Summary table shows, for each span:

A) The total CPU consumed by the entire trace.
B) Avg occurrences per trace, % of traces with the span, avg duration, avg % execution time.
C) The list of distinct error messages.
D) The number of users impacted.

---

**Q16.** A custom span tag `@order.value:>100` cannot be queried in **Indexed Search** unless:

A) The Agent obfuscator is disabled.
B) The tag is included in a retention filter (and the spans are indexed).
C) Live Search is disabled.
D) `DD_TRACE_SAMPLING_RULES` is set.

---

**Q17.** Which OOTB chart on the Service Page does Watchdog overlay anomaly bands on?

A) Just CPU usage
B) Requests, latency, and error rate
C) Memory usage and disk I/O
D) Apdex score only

---

**Q18.** Trace metric naming convention is:

A) `apm.<service>.<metric>`
B) `trace.<operation_name>.<metric>` (e.g., `trace.http.request.duration`)
C) `dd.<env>.<service>.<metric>`
D) `service.<service>.<metric>`

---

**Q19.** A custom span-based metric is billed as:

A) Free, like trace metrics.
B) A custom metric (counted per unique tag combination).
C) An indexed span.
D) A monitor.

---

**Q20.** The Service Map shows:

A) Hosts grouped by region.
B) Service-to-service dependencies discovered from APM traces.
C) Network packets between processes.
D) Agent installation status across the fleet.

---

**Q21.** A user opens the Trace Explorer and selects a 7-day time window. They are now in:

A) Live Search.
B) Indexed (Retained) Search.
C) Trace metrics view.
D) Software Catalog view.

---

**Q22.** Which query returns errors that occurred only in version `2025-10-01`?

A) `service:checkout error version=2025-10-01`
B) `service:checkout status:error version:2025-10-01`
C) `service:checkout @status:err @version:2025-10-01`
D) `checkout:2025-10-01:error`

---

**Q23.** A service has `dd-service: shopping-cart` in its Service Definition but emits APM traces with `service:shoppingcart`. What happens?

A) Datadog automatically reconciles the names.
B) The Software Catalog will create two distinct entries that won't link.
C) Only the YAML version is used.
D) Only the APM version is used.

---

## Answer Key

| # | Answer | Explanation |
|---|--------|-------------|
| 1 | **B** | Any service emitting APM traces is auto-discovered. YAML is optional metadata. |
| 2 | **B** | Cluster layout groups services by ownership/tier. Flow shows traffic-weighted dependencies. |
| 3 | **B** | `tier` is a critical-tier indicator used for SLO/Watchdog prioritization. |
| 4 | **B** | `service:` and `status:` are reserved attributes (no `@`). `status:error` is the canonical query. |
| 5 | **A** | `@duration:` is the standard span attribute query. Range form: `[A TO B]`. |
| 6 | **A** | Live Search hits all ingested data of the last 15 min, including non-indexed tags. |
| 7 | **C** | Indexed Search can only see spans retained by filters. Live Search has the broader tag access. |
| 8 | **C** | Flame graph is the default visualization in the Profiler. |
| 9 | **B** | Profile Comparison diff two profiles side-by-side, often by version. |
| 10 | **B** | Priority is `DD_VERSION` first, then container image tag, then Git SHA. |
| 11 | **B** | The Service Page Deployments tab is the canonical view for version comparison. |
| 12 | **B** | Fingerprint formula: `error.type` + `error.message` + stack frames. |
| 13 | **B** | Set `error.fingerprint` on the span to override grouping. |
| 14 | **B** | Resolved issues that recur after deploy auto-reopen. |
| 15 | **B** | These are the four span-summary metrics. |
| 16 | **B** | Indexed Search needs the tag to be on indexed spans (kept by a retention filter). |
| 17 | **B** | Watchdog anomaly bands appear on Requests, Latency, and Error rate by default. |
| 18 | **B** | Trace metrics follow `trace.<operation>.<metric>` convention. |
| 19 | **B** | Custom span-based metrics are billed as standard custom metrics by tag-combination cardinality. |
| 20 | **B** | Service Map visualizes service dependencies derived from APM traces. |
| 21 | **B** | A 7-day window forces Indexed Search; Live is bound to ≤15 minutes. |
| 22 | **B** | Reserved attributes use no `@`; `version:` is reserved. |
| 23 | **B** | The Catalog matches by `dd-service` name; mismatched names create separate entries. Always align them. |
