# Quiz 05 — Log Analysis (Domain 5)

**Coverage:** filtering & excluding · aggregations · visualizing
**Length:** 12 questions

---

### 1. An exclusion filter on an index drops a log from being…

A) Ingested into Datadog.
B) Stored in archives.
C) Indexed (i.e., searchable in the Explorer).
D) Used to generate metrics.

### 2. A team configures an exclusion filter at 100% on `source:nginx @http.url:/health*`. Which TWO statements are TRUE? (Select TWO)

A) Healthcheck logs no longer appear in the Log Explorer for that index.
B) Healthcheck logs still appear in Live Tail.
C) Datadog stops charging ingestion for those logs.
D) Healthcheck logs can still feed Logs to Metrics.
E) Healthcheck logs are deleted from archives.

### 3. A log can be routed to multiple indexes simultaneously.

A) True — every matching index ingests the log.
B) False — a log is routed to the first index whose inclusion filter matches.
C) True, but only if `multi_index: true` is set.
D) False — there is only one global index.

### 4. The maximum number of group-by dimensions for a **Timeseries** visualization is:

A) 1
B) 2
C) 4
D) 8

### 5. To compute a 95th-percentile of `@duration`, the attribute must first be declared as a:

A) Facet.
B) Measure (with a unit).
C) Tag.
D) Log Pattern.

### 6. Which compute returns the number of distinct values of an attribute (e.g., distinct users seeing errors)?

A) `count`
B) `distribution`
C) `cardinality(<attribute>)`
D) `unique(<attribute>)`

### 7. A retention setting on an index controls:

A) How many bytes per day are ingested.
B) How long indexed logs remain searchable in the Explorer.
C) How long archived logs are retained in S3.
D) The maximum number of facets per log.

### 8. The fastest way to investigate logs from a single host around the timestamp of a specific log line is:

A) Manually copy the host tag and timestamp into a new query.
B) Click **View In Context** on the log.
C) Open Live Tail and wait.
D) Export to CSV and grep.

### 9. A team wants the cheapest way to keep five years of logs available for occasional historical investigation. The recommended approach:

A) Set index retention to 5 years.
B) Increase daily quota indefinitely.
C) Use Log Archives (S3 / Blob / GCS), and Rehydrate when needed.
D) Forward logs to a self-hosted Elasticsearch.

### 10. Which TWO visualizations can use up to 4 group-by dimensions? (Select TWO)

A) Top List
B) Timeseries
C) Tree Map
D) Patterns
E) List

### 11. A daily quota on an index of 1,000,000 events/day is reached at 18:00 UTC. What happens to subsequent logs that match the index's inclusion filter?

A) The Datadog Agent stops sending those logs.
B) The logs are archived but not added to the index.
C) The logs are dropped at ingestion.
D) The Agent throws errors and crashes.

### 12. Which TWO statements about index inclusion vs. exclusion filters are correct? (Select TWO)

A) An index can have multiple exclusion filters, each with a percentage to exclude.
B) An index can have multiple inclusion filters; logs match if any one passes.
C) Excluded logs still go to archives if archives are configured.
D) Excluded logs are not counted toward Logs to Metrics.
E) Inclusion filter syntax is the same query language as the Log Explorer.

---

## Answer Key

1. **C** — Exclusion filter excludes from indexing only. The log is still ingested, archived, and usable for metric generation.

2. **A, D** — Healthcheck logs disappear from the Explorer (A) and still feed Logs to Metrics (D). Live Tail (B) shows them; (C) is wrong (still charged ingestion); (E) is wrong (still archived).

3. **B** — A log enters the **first** index whose inclusion filter matches. Order matters in the Indexes list.

4. **C** — Up to 4 group-by dimensions for Timeseries (and Table, Tree Map, Pie, Geomap). Top List is the exception with 1.

5. **B** — Aggregations like sum/avg/percentiles require a **measure**, which is a numeric facet with a unit.

6. **C** — `cardinality(<attribute>)` returns distinct count.

7. **B** — Retention is the in-index search window. Archive retention is separate (managed by your cloud).

8. **B** — **View In Context** opens a stream of logs from the same host/service around that time.

9. **C** — Archives are designed for cheap long-term storage; Rehydration brings selected ranges back when you need to search them.

10. **B, C** — Timeseries and Tree Map both support up to 4 group-bys. Top List (A) is limited to 1; Patterns (D) and List (E) don't use group-bys.

11. **B** — Above the daily quota, additional matches are still ingested + archived but **not added to the index**. The Agent doesn't react to indexing decisions.

12. **A, C, E** — Multiple exclusion filters per index (A); excluded logs still archive (C); inclusion uses Explorer query syntax (E). (B) is wrong (one inclusion filter per index); (D) is wrong (Logs to Metrics works on excluded logs).

---

**Score:** below 9/12 → re-read `knowledge-base/05-log-analysis.md`.
