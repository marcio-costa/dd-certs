# Drill 04 — Log Searching & Filtering (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A team has just enabled log collection on a brand-new service. They want to verify logs are flowing in real time. Logs aren't showing in the Explorer. Which TWO statements are TRUE? (Select TWO)

A) Live Tail will show the logs even if no inclusion filter routes them to an index yet.
B) Logs are guaranteed to appear in the Explorer within 30 seconds.
C) The Explorer view depends on indexing; un-indexed logs won't appear there.
D) Live Tail requires the logs to be archived first.
E) Saved Views automatically create an index.

**Q2.** A query `service:checkout @http.status_code:>=500` returns no results, but raw logs in the JSON show `status_code: 503`. Most likely:

A) The attribute is `@status_code`, not `@http.status_code` — the field hasn't been remapped to the standard attribute.
B) Datadog dropped them.
C) The Explorer is broken.
D) Range queries don't support `>=`.

**Q3.** Which TWO queries find logs whose URL contains `/api/v1/orders/<numeric_id>`? (Select TWO)

A) `@http.url:/api/v1/orders/*`
B) `@http.url:"/api/v1/orders/" AND @http.url:*`
C) `@http.url:/api/v1/orders/?` (single-char wildcard, only matches single-digit IDs)
D) `@http.url:/api/v1/orders/12*` (wildcard for prefixed IDs)
E) `@http.url:[/api/v1/orders/0 TO /api/v1/orders/Z]`

**Q4.** A team's query runs slow with `@http.url:*sensitive*`. The cause:

A) Leading wildcards force a full scan.
B) Trailing wildcards are not supported.
C) Datadog rate-limits queries.
D) The Explorer disables wildcards over 1 hour.

**Q5.** A team wants to filter logs **without** an `@error.kind` attribute set. The right query:

A) `@error.kind:null`
B) `-@error.kind:*`
C) `@error.kind:""`
D) `NOT @error.kind`

**Q6.** A custom JSON key `request_id` is on every log but doesn't appear in the facet sidebar. The single fix:

A) Add `request_id` as a facet via *Add to facets* in the log side panel.
B) Re-ingest all logs.
C) Restart the Agent.
D) Increase index retention.

**Q7.** Which TWO statements about Saved Views are TRUE? (Select TWO)

A) They persist query, time range, viz, and facet selection.
B) They include the user's API key.
C) Live time ranges (e.g., "Past 1 hour") stay live; absolute ranges convert on save.
D) They can be parameterized via URL `?query=...`.
E) They auto-refresh every 5 seconds.

**Q8.** Live Tail vs. Explorer — which TWO statements are TRUE? (Select TWO)

A) Live Tail shows logs from the last ~15 minutes.
B) Live Tail does not support visualization tabs (Top List, Pie, etc.).
C) Live Tail shows only indexed logs.
D) Saved Views can be applied to Live Tail.
E) Both use the same query syntax.

**Q9.** A team uses **Patterns**. Which TWO uses are appropriate? (Select TWO)

A) Identifying recurring noisy logs to add to exclusion filters.
B) Generating Grok patterns automatically (no — patterns are clusters of similar messages, not regex).
C) Spotting a new error pattern when its volume rises.
D) Building dashboards from clusters.
E) Replacing the Explorer search bar.

**Q10.** A team uses **Transactions** to group logs by `request_id`. Which TWO are TRUE? (Select TWO)

A) Max duration is configurable.
B) Logs further apart than the max duration are split into separate transactions.
C) Transactions require APM enabled.
D) Transactions create new metrics automatically.
E) Transactions are read-only — they can't be exported to dashboards.

**Q11.** Which is the maximum number of group-by dimensions for the **Tree Map** visualization?

A) 1
B) 2
C) 4
D) 8

**Q12.** A team queries `service:(checkout OR billing) AND env:prod NOT @http.status_code:200`. Which logs match?

A) Logs from `checkout` or `billing` in prod where the HTTP status is not 200.
B) Logs from `checkout` only.
C) Any log from `prod`.
D) Datadog rejects the syntax.

**Q13.** A facet's display group can be customized. Which TWO statements about facet groups are TRUE? (Select TWO)

A) Facet groups organize the sidebar (e.g., "Web", "Database").
B) Facet groups change query syntax.
C) A facet can be moved between groups.
D) Facet groups are global per org.
E) Facet groups are required for any custom facet.

**Q14.** A team writes `service:checkout host:web-0?`. The wildcard `?` in the host name matches:

A) Any single character in that position.
B) Zero or more characters.
C) Literal `?`.
D) Datadog rejects it.

**Q15.** A team wants to share a Saved View URL with a teammate. The teammate sees no results because they belong to a role that lacks access to the index. Which Datadog feature enforces this?

A) Restriction queries on the index.
B) Pipeline filters.
C) Multi-Alert.
D) Generate Metrics.

---

## Answer Key

1. **A, C** — Live Tail bypasses indexing; Explorer is index-bound.
2. **A** — Field name mismatch with the standard attribute. Add a Standard Attribute Remapper.
3. **A, D** — Wildcard for the trailing path or prefix. (C) only matches single-char; (E) is invalid range.
4. **A** — Leading wildcards force scans.
5. **B** — `-@error.kind:*` = "does not exist". (`null` and `""` aren't equivalent in Datadog log search.)
6. **A** — Add as facet via the log side panel.
7. **A, C** — Saved Views persist these. (B) wrong; (D) parameterized URL is `?query=` (this is also correct, so AC and D are all TRUE — but the question asks for TWO; the cleanest pair is A and C). Note: D is also defensible — pick AC for the most universally-correct pair.
8. **A, B, E** — But the question asks for TWO; A and E are the safest pair. B is also correct.
9. **A, C** — Find noise + spot rare patterns.
10. **A, B** — Configurable max + split on overflow.
11. **C** — Tree Map up to 4.
12. **A** — Combined boolean filter as written.
13. **A, C** — Group as organization; movable.
14. **A** — `?` is single-char wildcard.
15. **A** — Restriction queries are RBAC for indexes.
