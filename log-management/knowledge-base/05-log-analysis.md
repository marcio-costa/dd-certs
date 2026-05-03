# Domain 5 — Log Analysis

**Exam subtopics:** Filtering and Excluding · Aggregations · Visualizing

---

## 5.1 Filtering vs. excluding — different things in different places

The exam loves the difference between these two, and where each lives.

| Concept | Where it lives | Effect |
|---|---|---|
| **Search filter** | Log Explorer query bar | Limits what *you* see in the current view; doesn't change what's stored |
| **Saved-view filter** | Inside a Saved View | A pre-configured query that opens with that view |
| **Index filter** (inclusion) | *Logs → Configuration → Indexes* | Routes only matching logs to that index |
| **Exclusion filter** | *Logs → Configuration → Indexes* (per index) | Drops a *percentage* of matching logs from the index after they pass the inclusion filter |
| **Restriction query** (RBAC) | *Logs → Configuration → Indexes* (per index) | Limits which roles can see which logs in that index |
| **Edge filter** (`exclude_at_match`) | Agent `log_processing_rules` | Drops logs at the host before sending to Datadog |

### Indexes & exclusion in detail

An **index** in Datadog Logs has three levers:

1. **Inclusion filter** — the query that defines which logs *enter* the index. Indexes are evaluated **in order**; a log goes into the **first index** whose inclusion filter matches.
2. **Exclusion filters** — multiple per index. Each has a query and a **percentage to exclude (0–100%)**. You can exclude 100% of healthcheck logs, sample 50% of debug logs, etc. **Excluded logs are still ingested, parsed, archived, and counted toward metrics generation** — they just don't go into the searchable index.
3. **Retention** — how long indexed logs are searchable (3, 7, 15, 30, 60, 90 days; configurable on your plan).

> Memorize: **exclusion filters drop logs from the index, NOT from ingestion.** Logs you want to drop *before* paying for ingestion need an Agent edge rule (`log_processing_rules` with `exclude_at_match`).

### Daily quota per index

Each index can also have a **daily quota** — a hard cap on indexed log count per day. Logs above the quota stop being indexed (still ingested).

---

## 5.2 Aggregations — turning logs into numbers

The Explorer's **Analytics** tab (and the visualization tabs that aren't List) all rely on aggregations.

### Group-by dimensions

Pick **1 to 4 facet dimensions** to group by. Group-by is hierarchical: top-N for the first dimension, then top-N within each first-dim bucket, and so on.

Visualization caps:
- **Top List**: 1 dimension only.
- **Timeseries**, **Table**, **Tree Map**, **Pie**, **Geomap**: up to 4 dimensions.

### Compute (the numeric measure)

Choose **what to compute** for each group:

| Compute | Applies to | Description |
|---|---|---|
| **count** | Anything | Number of matching logs |
| **cardinality** | Any facet | Distinct values of the facet (e.g., distinct `@usr.id`) |
| **sum** | Numeric measure | Sum of the measure |
| **avg** | Numeric measure | Average of the measure |
| **min** / **max** | Numeric measure | Extremes |
| **median (p50)** | Numeric measure | 50th percentile |
| **pcXX** | Numeric measure | Configurable percentile (p75, p95, p99) |

Multiple computes per query are supported (e.g., count *and* avg(@duration) on the same group-by).

### Time bucketing (timeseries)

For **Timeseries** visualizations, choose an interval (10s → 1d). Datadog auto-picks a sensible default if you don't.

### Examples

- "Errors per service over time": `status:error` → group by `service` → compute `count` → Timeseries.
- "Slowest endpoints": `*` → group by `@http.url_details.path` → compute `p95(@duration)` → Top List.
- "Distinct users seeing errors": `status:error` → group by `service` → compute `cardinality(@usr.id)` → Table.

---

## 5.3 Visualizations

| Visualization | Best for | Limits |
|---|---|---|
| **List** | Reading individual log lines | No aggregation |
| **Timeseries** | Trends over time | Up to 4 group-by dimensions |
| **Top List** | "Top N by Y" | 1 group-by dimension only |
| **Table** | Multi-column comparison | Up to 4 dimensions, multiple computes |
| **Tree Map** | Proportional contribution by category | 1–4 dimensions |
| **Pie Chart** | Share of total | 1–4 dimensions |
| **Geomap** | Geographic distribution | Group by `@network.client.geoip.country.iso_code` |
| **Patterns** | Surface common log shapes | Auto, no group-by |
| **Transactions** | Group by correlation attribute | 1 attribute, time window |

### Saving a visualization to a dashboard

From the Explorer: **Export → Add to Dashboard**. The widget reproduces the query, group-by, compute, and visualization. Future dashboard loads re-run the query against the live index.

### Notebooks and incident docs

The same export menu offers **Send to Notebook** — useful for incident write-ups. The notebook captures the query, snapshot, and your commentary.

---

## 5.4 Drilldown patterns the exam expects

### From a metric alert to logs

1. Metric monitor fires with a service tag.
2. Click *Related Logs* → opens Log Explorer with `service:<name>` and the time window pre-applied.

### From an APM trace to logs

1. Open the trace.
2. **Logs** tab in the trace side panel shows logs with the same `dd.trace_id`.
3. Requires log–trace correlation (`dd.trace_id`/`dd.span_id` in the log).

### From an error log to its trace

The right panel of any log with `dd.trace_id` shows a **View Trace** button.

### Watchdog Insights for Logs

Datadog automatically detects unusual patterns (sudden error spikes, new error types, latency anomalies) and surfaces them in **Watchdog**. No setup required; visible from the Logs landing page.

---

## 5.5 The "filter early, aggregate cheaply" rule

Aggregations run against the **indexed** logs only. Two implications:

- If a log isn't in an index (excluded, archived only), it **won't show up in Explorer aggregations**. To analyze excluded logs, **rehydrate** the relevant archive into a temporary index first.
- Pre-filter your query as tightly as possible before grouping. Querying `*` and grouping by `@usr.id` is much slower than `service:checkout status:error` then grouping.

---

## 5.6 Exam-style takeaways for Domain 5

- **Exclusion filter ≠ ingestion drop.** Excluded logs still pay for ingestion and still flow to archives / metrics.
- An index has **one inclusion filter**, **multiple exclusion filters** (each with a 0–100% drop rate), and a **retention** setting.
- A log goes into the **first index** whose inclusion filter matches it.
- **Top List supports 1 group-by dimension; everything else supports up to 4.**
- For percentile computes (p95, p99), the source attribute must be declared as a **measure**, not just a facet.
- To see logs that were **excluded from indexing**, use **Archives + Rehydrate** or **Live Tail** (within 15 min).
- Aggregations only see indexed logs — pre-filter tightly before grouping.
- Use **View In Context** to pivot from one log to all logs from the same host/service around that time.
