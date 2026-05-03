# Drill 05 — Log Analysis (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A team applies `service:checkout` as the inclusion filter on `index_a` and `*` on `index_b` (which is listed below `index_a`). A log with `service:checkout` arrives. Where does it land?

A) `index_a` only — first match wins.
B) Both `index_a` and `index_b` — every matching index ingests.
C) `index_b` only.
D) Neither — Datadog rejects ambiguous routing.

**Q2.** Which TWO statements about exclusion filters are TRUE? (Select TWO)

A) An index can have multiple exclusion filters, each with a 0–100% sample rate.
B) Excluded logs are still archived (if archives are configured).
C) Excluded logs are dropped at ingestion.
D) Excluded logs cannot be used for Logs to Metrics.
E) Exclusion filters use a JSON-only query syntax.

**Q3.** A team configures an exclusion filter at 50% on `source:nginx`. After the filter, the index contains:

A) All `source:nginx` logs.
B) Approximately half of `source:nginx` logs (sampled at 50%).
C) Zero `source:nginx` logs.
D) Only `source:nginx` logs from the past hour.

**Q4.** A team analyzes the slowest endpoints by p99 latency. The query: group by `@http.url` → compute `pc99(@duration)` → Top List. Which TWO must be configured? (Select TWO)

A) `@duration` declared as a measure with a unit.
B) `@http.url` declared as a string facet.
C) `@duration` declared as a string facet.
D) An Agent rule of type `mask_sequences`.
E) A Status remapper.

**Q5.** A team uses Logs to Metrics to count `service:checkout status:error` per service. They later realize older logs are missing from the metric. Why?

A) Generated metrics are forward-looking only — they aren't backfilled from historical logs.
B) The metric resolution rolled them up.
C) The metric quota was exceeded.
D) The Status remapper was retired.

**Q6.** Which TWO visualizations support up to 4 group-by dimensions? (Select TWO)

A) Top List
B) Timeseries
C) Pie Chart
D) Patterns
E) List

**Q7.** A team runs a Top List `service:checkout @http.status_code:5* | top by @http.url limit 10`. Which is the maximum number of group-by dimensions used?

A) 1
B) 2
C) 4
D) Top List's limit is 1.

**Q8.** A daily quota of 1M events/day is reached. Subsequent logs that match the index's inclusion filter:

A) Are dropped at ingestion.
B) Are still ingested + archived but not added to the index.
C) Trigger an Agent crash.
D) Are queued and indexed at midnight.

**Q9.** A team needs a 1-month searchable retention for security-critical logs and 7-day for everything else. The recommended setup:

A) One index with mixed retentions (not supported).
B) Two indexes — security-critical with 30-day retention; default with 7-day. Inclusion filters route per service.
C) Forward security logs to an external system.
D) Use Sensitive Data Scanner to extend retention.

**Q10.** A team wants to view logs that were excluded from the index three weeks ago. The right approach:

A) Increase exclusion filter sample to 0% retroactively.
B) Use Live Tail.
C) Rehydrate the matching archive into a temporary index.
D) Use the View In Context feature.

**Q11.** A custom dashboard widget shows log analytics from a specific index. When the index is renamed, the widget:

A) Continues working (Datadog tracks indexes by ID).
B) Breaks until the widget is updated to point to the new name.
C) Auto-creates a new widget.
D) Cannot exist — log widgets aren't supported on dashboards.

**Q12.** A team uses View In Context on a log line. Which other logs appear?

A) Only logs with the same `dd.trace_id`.
B) Logs from the same `host` and `service` around the chosen log's timestamp.
C) Logs across the entire org for that minute.
D) Only logs from the same archive.

**Q13.** Which compute returns the **distinct** count of users who saw an error, in a query grouped by `service`?

A) `count by service`
B) `cardinality(@usr.id) by service`
C) `unique(@usr.id) by service`
D) `count(@usr.id) by service`

**Q14.** A team adjusts an index's retention from 30 to 7 days. Which is TRUE?

A) Logs older than 7 days are deleted from the index immediately at the change time.
B) Going forward, retention is 7 days; previously-retained logs continue to be searchable until their 30-day window naturally expires.
C) Logs are auto-archived to compensate.
D) The Explorer becomes unavailable for that index.

**Q15.** A team's index is filling fast. Which TWO levers reduce indexing cost without losing audit trail? (Select TWO)

A) Add an exclusion filter for high-volume noise (sample 100%); ensure archives capture the dropped logs.
B) Reduce retention to 3 days.
C) Disable ingestion.
D) Add a daily quota.
E) Switch all logs to plaintext.

---

## Answer Key

1. **A** — First matching index wins (top-to-bottom).
2. **A, B** — Multi-filter + archives still receive. (C) wrong (it's about indexing, not ingestion); (D) wrong; (E) wrong.
3. **B** — Sampled — roughly half.
4. **A, B** — Measure for percentile + facet for grouping.
5. **A** — Forward-looking only.
6. **B, C** — Timeseries and Pie support up to 4.
7. **D** — Top List limit is 1.
8. **B** — Above quota: ingested + archived, not indexed.
9. **B** — Two indexes with appropriate retentions and inclusion routing.
10. **C** — Rehydrate from archive.
11. **A** — Indexes are tracked by ID; renaming doesn't break widgets.
12. **B** — Same host & service, around that timestamp.
13. **B** — `cardinality` for distinct count.
14. **B** — Forward-looking; existing logs keep their original retention.
15. **A, D** — Exclusion (with archives) + daily quota.
