# Domain 4 — Log Searching & Filtering

**Exam subtopics:** Live vs. Explorer · Search Syntax · Explorer Functionality

---

## 4.1 Live Tail vs. Log Explorer — the headline distinction

Two different views, two different windows of time.

| | **Live Tail** | **Log Explorer** |
|---|---|---|
| Time range | **Last ~15 minutes**, streaming as logs arrive | **Index retention** (typically 15 / 30 days+) |
| What you see | **All ingested logs**, even those *not* indexed | **Only indexed logs** |
| Search syntax | Same query language, applied as a **filter on the live stream** | Full faceted search, dashboards, monitors |
| Aggregations | None — list view only | Timeseries, top list, table, tree map, pie, geomap |
| Saved views | No (it's live) | Yes |
| Cost | No search cost — it's all from the ingestion stream | Search cost via index retention pricing |

When to use which:

- **Live Tail** — verifying a new pipeline / collection is working, watching errors during a deploy, debugging right now.
- **Log Explorer** — historical investigation, dashboards, monitors, post-mortems.

> Common exam trap: a question describing "I want to confirm logs are arriving from a brand-new service" — the answer is **Live Tail**, because the logs may not be in any index yet (or might be excluded from indexing).

---

## 4.2 Search syntax

The same syntax works in Live Tail, Log Explorer, log monitors, and dashboard log widgets.

### Free-text search

```
"connection refused"
timeout
```

Free text matches against the **`message`** attribute by default. Quote multi-word phrases.

### Tag filters (`key:value`)

```
service:checkout
env:prod
host:web-01
```

No `@` prefix for **tags** or **reserved attributes** (`host`, `service`, `source`, `status`).

### Attribute filters (`@key:value`)

```
@http.status_code:500
@http.url:/api/v1/orders
@usr.id:u-99
@duration:>1000000000
```

**Always prefix attributes with `@`**.

### Range queries

Brackets `[...]` are inclusive; braces `{...}` are exclusive.

```
@http.status_code:[500 TO 599]
@duration:{1000 TO *]
@http.response_time:[* TO 100}
```

### Boolean operators

`AND`, `OR`, `NOT` (uppercase). Implicit `AND` between space-separated terms.

```
service:checkout AND status:error
status:(error OR warn)
service:web NOT @http.status_code:200
```

Use parentheses for grouping:

```
(service:billing OR service:orders) AND env:prod AND status:error
```

### Wildcards

`*` for any sequence, `?` for a single character. Wildcards can be **mid-token or at the end**, but **leading wildcards** (`*error`) are slow and should be avoided.

```
service:payment-*
@http.url:/api/v1/users/*
host:web-?
```

### Escaping

Special characters (`+ - = & | > < ! ( ) { } [ ] ^ " ~ * ? : \ /`) inside values must be escaped with `\`, or wrap the value in quotes:

```
@http.url:"/path/with:colons"
@error.message:"OutOfMemoryError\: Java heap"
```

### Existence / nullity

```
@usr.id:*           ← attribute exists (any value)
-@usr.id:*          ← attribute does NOT exist
@usr.id:""          ← empty string (different from missing)
```

### Common shortcut: `source` and `host`

`source` is a **reserved attribute** (the technology name set during collection — `nginx`, `python`, `java`). `host` is also reserved (the hostname). Both are tags-style:

```
source:nginx host:web-01
```

---

## 4.3 The Log Explorer interface

Anatomy from left → right:

1. **Time range selector** (top right) — pick a relative window (15m, 1h, 4h, 1d, custom) or **Live Tail**.
2. **Search bar** — free text + facet syntax.
3. **Facets sidebar** (left) — clickable list of facets and measures. Numeric measures show histograms; string facets show top-N lists.
4. **Visualization tabs** (top center) — **List**, **Timeseries**, **Top List**, **Table**, **Tree Map**, **Pie Chart**, **Transactions**, **Patterns**.
5. **Result area** — the chosen visualization.
6. **Right panel** — opens when you click a log; shows full attributes, related traces/metrics, **View in context** button.

### Saved Views

A **Saved View** stores:

- The query
- The time range (relative time ranges like "Past 1 hour" are preserved as live; absolute ranges are converted)
- The chosen visualization (List, Timeseries, Patterns, Analytics, etc.)
- The selected facets in the sidebar

Save with the **Save** button → name + folder. Share the URL or pin to a team folder. Saved views can be referenced in dashboards and monitors.

URL format example:
```
https://app.datadoghq.com/logs?saved_view=305130
https://app.datadoghq.com/logs?saved_view=305130&query=@source:nginx @network.client.ip:1.2.3.4
```

### View In Context

When viewing a single log, **View In Context** opens a stream of *all* logs from the same `host` and `service` around that timestamp — invaluable for stack-trace tracing.

---

## 4.4 Facets and measures

Facets are searchable, filterable indexes of attributes/tags.

| Type | Use | Example |
|---|---|---|
| **Tag facet** | Filter by tag values | `env`, `service`, `team` |
| **String facet** | Filter by attribute string values | `@usr.id`, `@http.url_details.path` |
| **Numeric measure** | Filter and aggregate (sum, avg, percentile) | `@duration`, `@http.response_size` |

### Auto-created vs. custom facets

- **Reserved attributes** + **standard attributes** become facets automatically.
- Custom JSON keys do **not** become facets until you explicitly create them (*Add as facet*).
- **You only need to facet attributes you search/filter on.** Faceting too many attributes adds load with no benefit.

### Measures vs. facets

A **measure** is a numeric facet you want to aggregate on (sum, avg, p99). To use `@duration` in an aggregation, declare it as a **measure** with a unit (nanoseconds, milliseconds, bytes, etc.).

### Facet definition workflow

1. Open a log in the Explorer that contains the attribute.
2. Click the attribute → **Create facet** or **Create measure**.
3. Choose display name, group, description.
4. Once created, the facet appears in the sidebar and can be used in `@key:value` searches **even on logs ingested before the facet was created** (Datadog re-indexes on the fly).

---

## 4.5 Patterns

The **Patterns** tab automatically clusters logs that share a similar text structure, hiding variable parts (IDs, timestamps).

```
[INFO] User * logged in from *
[ERROR] Connection refused on host * port *
```

Use it to:

- Spot dominant log "shapes" you can ignore (drop in pipeline).
- Find rare patterns that indicate new errors.
- Build exclusion filters (one-click → *Add to exclusion filter*).

Patterns work on the **`message`** attribute by default; you can repoint them to another attribute.

---

## 4.6 Transactions

A **Transaction** groups related logs by a shared attribute (e.g., `request_id`, `session_id`) into a single timeline.

- Define the **grouping attribute** (e.g., `@request_id`).
- Set the **maximum duration** (e.g., 30 s) — logs further apart are split into separate transactions.
- Optionally a **start condition** and **end condition**.

Output: a list of transactions you can sort by total duration, count, or status, then drill into the constituent log lines.

Use cases: stitching together a multi-service request flow when you don't have full APM tracing.

---

## 4.7 Exam-style takeaways for Domain 4

- **Live Tail = last 15 min of every ingested log.** Explorer = retained / indexed logs.
- Use **Live Tail** to verify ingestion before logs are indexed.
- Tags / reserved attributes are queried **without** `@`. Custom attributes need `@`.
- Range queries use `[a TO b]` (inclusive) or `{a TO b}` (exclusive); `*` is the open bound.
- Wildcards work mid/end-token; **leading wildcards** are slow.
- A log only becomes searchable by an attribute when it's a **facet** — non-faceted JSON keys won't filter from the sidebar (but `@key:value` text search still works).
- **Saved Views** preserve query + time range + visualization + facets.
- **Patterns** group similar logs; great for finding noise to drop.
- **Transactions** stitch logs by a correlation attribute over a time window.
