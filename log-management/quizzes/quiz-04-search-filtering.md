# Quiz 04 — Log Searching & Filtering (Domain 4)

**Coverage:** Live vs. Explorer · search syntax · Explorer functionality
**Length:** 12 questions

---

### 1. A team has just deployed log collection from a new service. They want to verify logs are arriving **right now** even though the service hasn't been added to any index. Which view do they use?

A) Log Explorer with a 1-hour time range.
B) Live Tail.
C) Notebook query.
D) Saved view.

### 2. What is the approximate time window for Live Tail?

A) Last 60 seconds.
B) Last 15 minutes.
C) Last 1 hour.
D) Equal to the index retention.

### 3. Which TWO statements about Live Tail are correct? (Select TWO)

A) It shows all ingested logs, including those excluded from indexing.
B) It supports timeseries aggregations.
C) It uses the same query syntax as the Log Explorer.
D) It is the default tab in the Log Explorer.
E) It can be saved as a Saved View.

### 4. To search logs where the HTTP status is 500-599 inclusive, which query is correct?

A) `@http.status_code:500-599`
B) `@http.status_code:[500 TO 599]`
C) `@http.status_code:{500 TO 599}`
D) `@http.status_code BETWEEN 500 AND 599`

### 5. Which TWO of the following queries find logs from the `checkout` service that are NOT 200-OK? (Select TWO)

A) `service:checkout NOT @http.status_code:200`
B) `service:checkout -@http.status_code:200`
C) `service:checkout != 200`
D) `service:checkout AND status:error`
E) `service:checkout AND -200`

### 6. Which prefix is required for **attribute** filters (vs. tags or reserved attributes)?

A) `#`
B) `:`
C) `@`
D) `&`

### 7. The query `@usr.id:*` matches logs where:

A) `@usr.id` does not exist.
B) `@usr.id` is `null`.
C) `@usr.id` exists with any value.
D) `@usr.id` equals the literal string `*`.

### 8. A custom JSON field `request_path` is showing up correctly inside individual logs but **cannot be searched as a facet from the sidebar**. The most likely cause is:

A) The field has too many distinct values.
B) The field has not been declared as a facet.
C) The Explorer is in Live Tail mode.
D) The field is over the 1 MB log limit.

### 9. The **Patterns** tab in the Log Explorer is most useful for:

A) Detecting trace-log correlation breakage.
B) Clustering logs that share a similar text shape and identifying noise to drop.
C) Building a heat map of CPU usage.
D) Generating Grok patterns automatically.

### 10. A Saved View persists which of the following? (Select ALL that apply)

A) The query string.
B) The selected time range (relative ranges are preserved as live; fixed ranges convert to absolute on save).
C) The current visualization (List, Timeseries, Patterns, etc.).
D) The selected facets in the sidebar.
E) The user's saved Datadog API key.

### 11. The **Top List** visualization is limited to how many group-by dimensions?

A) 1
B) 2
C) 4
D) Unlimited

### 12. **Transactions** in the Log Explorer group logs by:

A) Their `dd.trace_id`, only.
B) A correlation attribute you specify (e.g., `request_id`), within a max duration window.
C) Five-minute time buckets.
D) The same `host` and `service`.

---

## Answer Key

1. **B** — Live Tail streams all ingested logs in real time, regardless of indexing status. New services often haven't reached an index yet.

2. **B** — ~15 minutes.

3. **A, C** — Live Tail shows all ingested logs (incl. excluded) and uses the same query syntax. It does not support aggregations (B), is not the default tab (D), and cannot be saved as a Saved View (E).

4. **B** — `[a TO b]` is **inclusive** range. `{a TO b}` is exclusive. There is no `BETWEEN` syntax.

5. **A, B** — `NOT` and `-` (minus prefix) both negate. (C) is invalid syntax; (D) is too narrow (only error status); (E) is invalid.

6. **C** — `@` for attributes; tags and reserved attributes use no prefix.

7. **C** — `@usr.id:*` means "exists with any value". `-@usr.id:*` would mean "does not exist".

8. **B** — Custom JSON keys must be **explicitly declared as facets** before they appear in the sidebar.

9. **B** — Patterns clusters logs by similar text shape (variable parts shown as `*`). Great for finding high-volume noise to add to exclusion filters.

10. **A, B, C, D** — Saved Views persist query, time range, visualization, and facet selection. They do **not** include API keys (E).

11. **A** — Top List is limited to **1 dimension**. Other visualizations support up to 4.

12. **B** — Transactions group logs by a correlation attribute within a max window you specify.

---

**Score:** below 9/12 → re-read `knowledge-base/04-search-filtering.md`.
