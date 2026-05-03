# Domain 6 — Log Utilization

**Exam subtopics:** Monitors & Alerting · Metric Generation · Exporting

---

## 6.1 Log Monitors

A **log monitor** triggers when log activity matches a query and crosses a threshold. Three monitor "shapes":

| Monitor type | Triggers when | Use case |
|---|---|---|
| **Threshold** | A count over a window crosses a number | "More than 50 errors per 5 min on checkout" |
| **Anomaly** | Volume deviates from historical baseline | "Unusual spike in 5xx" |
| **Change alert** | Volume changes vs. a prior window | "Errors doubled compared to last hour" |

> Log monitors run against **indexed** logs (with one exception — see Live monitor below). If your logs are excluded from the index they won't drive a monitor.

### Anatomy of a log monitor query

```
logs("status:error service:checkout").index("main").rollup("count").last("5m") > 50
```

Components:

- **Query** — same syntax as the Log Explorer (`status:error service:checkout`).
- **Index** — which index to query (or `*` for all).
- **Rollup** — `count` (the most common), or `cardinality(<facet>)`, or aggregation on a measure (`avg`, `sum`, `min`, `max`, percentile).
- **Time window** — `last("5m")`, `last("15m")`, etc.
- **Threshold** — the comparator (`>`, `>=`, `<`, `<=`).

### Group-by alerting (multi-alert)

Toggle **Multi Alert** to alert *per group* (e.g., per `service`, per `host`). One monitor → many alerts, one per unique tag value.

### Notifications

`@email`, `@slack-channel`, `@pagerduty-…`, `@webhook-…`, `@dd-myteam` — embed the handle in the message body. Use template variables `{{service.name}}`, `{{value}}`, `{{threshold}}`. Add `@here`/`@channel` for Slack escalations.

### Recovery and re-notify

Monitor configuration has:
- **Renotify if not resolved every X minutes** — keeps an unresolved alert visible.
- **Recovery threshold** — separate value, lower than the alert threshold, to avoid flapping.
- **Group retention time** — how long the monitor remembers a group before dropping it.

### Live (a.k.a. on-Live-Tail) monitors

A specialized monitor type that watches the **Live Tail stream** for the appearance of any log matching a query. Triggers near-instantly, **doesn't depend on the log being indexed**. Less commonly tested but worth knowing.

---

## 6.2 Generate metrics from logs (Logs to Metrics)

Datadog can derive **custom metrics** from log volumes/values, on-ingest, **independent of indexing**. This is the headline benefit of *Logging Without Limits™*.

### Configuration: *Logs → Generate Metrics*

Define:

1. **Query** — which logs the metric counts.
2. **Metric name** — must follow Datadog metric naming (`my_app.errors.count`).
3. **Aggregation type:**
   - **`count`** — number of matching logs (becomes a `count` metric type).
   - **`distribution`** of a numeric attribute — sum/avg/percentiles preserved.
4. **Group by** — up to **10 tag dimensions** that become tags on the metric (e.g., `service`, `env`, `endpoint`).

### Why use Logs to Metrics

- **Cheap long-term retention.** Metrics retain 15 months by default; logs typically 30 days.
- **Dashboards / monitors don't need the log indexed.**
- **Cardinality control.** Tag carefully — every unique tag-value combination is a custom metric instance.

### What it does NOT do

- It does **not** prevent the log from being ingested or indexed — those are separate decisions (see exclusion filters).
- It does **not** retroactively create metrics from past logs (only forward-looking).

### Example

Requirement: track requests-per-minute by endpoint without paying to index access logs.

1. Drop access logs from the `main` index via an **exclusion filter** (`source:nginx @http.url:* → 100% exclude`).
2. Add a Logs to Metrics rule: query `source:nginx`, metric `nginx.requests`, count, group by `service`, `@http.method`, `@http.status_code`.
3. Build dashboards / monitors off the metric. Logs go to **archives** for forensics.

---

## 6.3 Exporting logs

Datadog supports several export paths.

### Log Archives — long-term cold storage

| Destination | Notes |
|---|---|
| **AWS S3** | Most common; assume-role IAM auth. |
| **Azure Blob Storage** | App registration with storage RBAC. |
| **GCS (Google Cloud Storage)** | Service account JSON. |

What gets archived:

- **All ingested logs** that match the archive's filter (independent of indexing).
- Compressed JSON files (`.json.gz`) with one log per line.
- Files are organized in a `dt=YYYY-MM-DD/hour=HH/...` Hive-style layout.

Archive **encryption**:

- Server-side encryption: AWS S3 SSE-S3 / SSE-KMS, Azure / GCS native.
- Client-side encryption is supported via configuration (less common).

### Rehydration — re-indexing archived logs

When you need to investigate logs older than the index retention:

1. *Logs → Configuration → Rehydrate from Archive*.
2. Pick the archive, time range, and an optional query filter.
3. Datadog re-ingests matching logs into a **temporary rehydrated index** for a chosen retention.
4. Counts toward your indexed-events bill for that month, but is bounded.

### Log Forwarding — push logs to third-party destinations

Datadog can forward selected logs in real time to:

- **Splunk** (HEC)
- **Elasticsearch** / **OpenSearch**
- **HTTP** endpoints (generic webhooks)
- **Microsoft Sentinel**

Configure under *Logs → Configuration → Forwarding*. Use a **filter query** to forward only the relevant subset. Useful when teams need logs in another SIEM but you want Datadog as the central pipeline.

### Reading logs out via API

`GET /api/v2/logs/events/search` lets you pull logs programmatically. There is also a CSV/JSON export from the Explorer (small ad-hoc exports).

---

## 6.4 Logging Without Limits™ — putting it all together

The exam loves this concept; here it is in one diagram:

```
                        [ AGENT / API / FORWARDER ]
                                    │
                                    ▼
                          ┌────────────────┐
                          │   INGESTION    │  ← billed per-GB ingested
                          └────────────────┘
                                    │
                                    ▼
                          ┌────────────────┐
                          │   PIPELINES    │  ← parsing, enrichment
                          └────────────────┘
                                    │
                ┌───────────────────┼─────────────────────────┐
                ▼                   ▼                         ▼
        ┌──────────────┐   ┌──────────────┐         ┌──────────────────┐
        │  LIVE TAIL   │   │   ARCHIVES   │         │   GEN METRICS    │
        │  (15-min)    │   │ (cheap, S3)  │         │  (volume/value)  │
        └──────────────┘   └──────────────┘         └──────────────────┘
                                    │
                                    ▼
                       ┌────────────────────────┐
                       │   INDEXES (search)     │  ← billed per-event indexed
                       │   - Inclusion filters  │
                       │   - Exclusion filters  │
                       │   - Retention          │
                       └────────────────────────┘
                                    │
                ┌───────────────────┼────────────────────┐
                ▼                   ▼                    ▼
       ┌──────────────┐   ┌──────────────────┐   ┌──────────────────┐
       │   EXPLORER   │   │ LOG MONITORS     │   │   FORWARDING     │
       └──────────────┘   └──────────────────┘   └──────────────────┘
```

Key billing principles:

- **Ingestion** is metered in GB. Reduce with edge `exclude_at_match` rules.
- **Indexing** is metered in millions of events × retention. Reduce with **exclusion filters** and **daily quotas**.
- **Archives** are billed by your cloud provider (S3 / Blob / GCS), not Datadog.
- **Generated metrics** count toward your custom metrics quota.

---

## 6.5 Sensitive data scanning recap

Already covered under Domain 2 (collection-level masking) and Domain 3 (server-side scanning). Two complementary places:

- **Agent-side `mask_sequences`** — for high-confidence regexes, before the data leaves the host.
- **Sensitive Data Scanner (server-side)** — broader rule library, applied centrally, after ingestion, before indexing.

---

## 6.6 Cost-control checklist (from low-impact to high-impact)

1. **Drop healthchecks at the Agent edge** (`exclude_at_match` on common probes).
2. **Aggregate stack traces** with `multi_line` rules so each error is one log, not 50.
3. **Generate metrics** from high-volume but predictable logs (access logs, queue depth).
4. **Exclude high-volume, low-search-value logs** from indexes (debug logs from prod, healthchecks).
5. **Use daily quotas** on indexes to bound the worst case.
6. **Right-size retention** per index — 30 days for general, 7 days for noisy debug, 90 days for security.
7. **Archive everything** — cheap at S3 prices, rehydrate when you need it.

---

## 6.7 Exam-style takeaways for Domain 6

- A log monitor query: `logs("<query>").index("<idx>").rollup("count").last("5m") > N`.
- **Log monitors run on indexed logs** (except the rare *live* monitor).
- **Generated metrics work even on excluded logs** — that's the point of Logging Without Limits™.
- Generated metrics support **count** and **distribution** aggregations and **up to 10 group-by tags**.
- **Archives** are cheap, indexed search is expensive — use exclusion filters to push noise to archives.
- **Rehydration** brings archived logs back into a temporary index when you need historical search.
- **Forwarding** sends logs to Splunk / Elasticsearch / HTTP / Sentinel — useful for SIEM hand-offs.
