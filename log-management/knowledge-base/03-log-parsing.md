# Domain 3 — Log Parsing

**Exam subtopics:** Processors & Pipelines · Standard Attributes · Log Composition

This is one of the heaviest exam domains — be ready for ordering questions and "which processor would you add" scenarios.

---

## 3.1 The pipeline model

A **log pipeline** is an ordered set of **processors** that runs against logs matching a **filter query**. Pipelines are evaluated **top to bottom** in the order shown in *Logs → Configuration → Pipelines*.

```
incoming log
   │
   ▼
┌─────────────────────────┐
│ Pipeline 1 (filter X)   │ ─── if filter matches, processors 1..n run
└─────────────────────────┘
   │
   ▼
┌─────────────────────────┐
│ Pipeline 2 (filter Y)   │
└─────────────────────────┘
   │
   ▼
... output → indexes / archives / metrics
```

Key behaviors:

- A log **passes through every pipeline whose filter matches**, in order.
- A log **does NOT stop** at the first matching pipeline — multiple pipelines can enrich the same log.
- **Integration pipelines** (e.g., `Nginx`, `Kafka`, `Redis`) are pre-built by Datadog and triggered by `source:<integration_name>`. You can clone or disable them but not edit the originals directly — clone, then customize.
- **Custom pipelines** sit alongside integration pipelines and can be reordered.

### Nested pipelines

A pipeline can contain other pipelines (one level of nesting). The inner pipeline only runs if the outer pipeline's filter matched. Useful for grouping environment-specific transformations.

---

## 3.2 Processors — the toolbox

| Processor | What it does | Typical use |
|---|---|---|
| **Grok parser** | Extracts attributes from a string using a Grok pattern | Parse unstructured / regex-style logs |
| **Date remapper** | Sets the official log timestamp from an attribute | Use `timestamp` field instead of ingest time |
| **Status remapper** | Sets the official log `status` (severity) | Map `level: ERROR` → `status: error` |
| **Service remapper** | Sets the official `service` from an attribute | When `service` was logged in the message |
| **Message remapper** | Sets the official `message` (the headline shown in Explorer) | Replace raw log with a cleaner field |
| **Remapper (generic)** | Copies one attribute to another (or to a tag) | Promote `@user.id` to a tag |
| **Attribute remapper** | Generic attribute rename / move / copy | Restructure JSON paths |
| **URL parser** | Splits URLs into host, path, query params | API gateway / web logs |
| **User-Agent parser** | Splits UA string into browser, OS, device | Web access logs |
| **Category processor** | Buckets logs by attribute value into a new category attribute | `latency_bucket: fast/medium/slow` |
| **Arithmetic processor** | Computes new numeric attribute from others | `latency_ms = end_time - start_time` |
| **String builder** | Concatenates strings into a new attribute | Compose `request_path` from parts |
| **GeoIP parser** | Resolves IP → country / city / coordinates | Tag a `network.client.ip` |
| **Lookup processor** | Maps a value via a lookup table you upload | `user_id` → `user_tier` |
| **Reference Table lookup** | Maps via a managed reference table (CSV) | Larger lookup tables |
| **Trace ID remapper** | Promotes an attribute to `dd.trace_id` for correlation | Custom log–trace correlation |

### Required processor ordering

Processors execute **in declaration order** within their pipeline. The canonical recommended order:

1. **Parse** raw logs (Grok parser) → extract attributes from `message`.
2. **Normalize** with remappers (date, status, service, message) → align with standard attributes.
3. **Enrich** (GeoIP, User-Agent, category, lookup, arithmetic).
4. **Restructure** (attribute remappers, string builder).

A reorder bug is the #1 cause of "my log is parsed but the timestamp is wrong / status is wrong" — the date remapper must come *after* the grok parser that extracts the date.

---

## 3.3 The Grok parser in depth

Grok patterns are named regexes. Datadog's Grok flavor uses this syntax:

```
%{<MATCHER>:<EXTRACT>}
%{<MATCHER>:<EXTRACT>:<FILTER>}
```

- `<MATCHER>` — built-in pattern: `notSpace`, `data`, `word`, `integer`, `number`, `date(format)`, `boolean`, `ipv4`, `ipv6`, `uuid`, `regex(...)`.
- `<EXTRACT>` — the attribute name to assign (omit to discard).
- `<FILTER>` — optional post-processing: `lowercase`, `uppercase`, `nullIf("...")`, `keyvalue(...)`, `json`, `array(...)`, `scale(<n>)`.

### Match rules vs. support rules

A grok parser has two text areas:

- **Match rules** — the patterns to attempt against the source attribute. The first match wins.
- **Support rules** — reusable named patterns that match rules can reference. Like helper functions.

### Worked example

Input log:

```
2026-05-03 14:22:01 INFO [user=42] Order 4521 created
```

Match rule:

```
my_rule %{date("yyyy-MM-dd HH:mm:ss"):timestamp} %{word:level} \[user=%{integer:user_id}\] Order %{integer:order_id} %{data:action}
```

Result:

```json
{
  "timestamp": "2026-05-03T14:22:01.000Z",
  "level": "INFO",
  "user_id": 42,
  "order_id": 4521,
  "action": "created"
}
```

Then a **Date remapper** on `timestamp` sets the official log timestamp, and a **Status remapper** on `level` sets the official severity.

### Useful Grok filters

- `:keyvalue` → parse `k1=v1 k2=v2` substrings.
- `:json` → parse a JSON substring into nested attributes.
- `:nullIf("-")` → set null instead of literal `"-"` (common for missing fields in web logs).
- `:scale(0.001)` → multiply a number (e.g., µs → ms).

---

## 3.4 Standard attributes & naming convention

Datadog defines a **standard attributes** set so logs from different sources share a common vocabulary. Memorize the key namespaces:

| Namespace | Examples | Meaning |
|---|---|---|
| Reserved (no `@` prefix) | `host`, `service`, `source`, `status`, `message`, `timestamp`, `tags` | Top-level fields Datadog promotes for every log |
| `@http.*` | `@http.method`, `@http.status_code`, `@http.url`, `@http.useragent` | HTTP request/response metadata |
| `@network.*` | `@network.client.ip`, `@network.bytes_read`, `@network.client.geoip.country.iso_code` | Connection / IP / GeoIP |
| `@error.*` | `@error.kind`, `@error.message`, `@error.stack` | Application errors |
| `@usr.*` | `@usr.id`, `@usr.email`, `@usr.name` | Authenticated user info |
| `@db.*` | `@db.statement`, `@db.system`, `@db.user` | Database operations |
| `@duration` | numeric, in nanoseconds | Operation duration |
| `@trace_id`, `@span_id` | Datadog APM correlation | Same as `dd.trace_id`, `dd.span_id` |

Why this matters:

- **Out-of-the-box dashboards** and analytics rely on these names. If your logs use `request_status` instead of `@http.status_code`, the HTTP dashboard won't populate.
- **Standard attributes are searchable as facets** without manual creation.
- A **standard attribute remapper** in your custom pipeline maps your fields into the standard names.

### Reserved vs. standard

- **Reserved attributes** are top-level (no `@`): the seven core fields (`host`, `service`, `source`, `status`, `message`, `timestamp`, `tags`). They are special in the Explorer (always visible columns) and are set by remappers.
- **Standard attributes** are everything `@*` that Datadog has standardized. The list is editable in *Logs → Configuration → Standard Attributes*.

---

## 3.5 Log composition

How a log **looks** in Datadog after processing — the structured "envelope" that you query against:

```json
{
  "host": "web-01",
  "service": "checkout",
  "source": "java",
  "status": "error",
  "message": "Database timeout after 5000ms",
  "timestamp": "2026-05-03T14:22:01.000Z",
  "tags": ["env:prod", "team:checkout", "version:1.4.2"],
  "@http.method": "POST",
  "@http.url": "/api/v1/orders",
  "@http.status_code": 500,
  "@error.kind": "TimeoutException",
  "@error.stack": "...",
  "@duration": 5012000000,
  "@usr.id": "u-99",
  "dd.trace_id": "5894812345...",
  "dd.span_id": "...",
  "_dd": { ... }   // Datadog internal metadata
}
```

Components:

- **Reserved attributes** (`host`, `service`, …) — top-level, no `@`.
- **Tags** — `key:value`, exposed under the `tags` array.
- **Attributes** — anything `@*`, organized into namespaces.
- **Internal metadata** — `_dd` block with ingestion / pipeline metadata; usually hidden in the UI.

Querying:

- Tags: `service:checkout`, `env:prod`
- Attributes: `@http.status_code:500`, `@duration:>1000000000`
- Reserved: `status:error`, `host:web-01`

Note the `@` is required for attribute search but **not** for tag/reserved-attribute search.

---

## 3.6 Auto-JSON parsing

If a log arrives at Datadog as **valid JSON**, Datadog automatically:

- Promotes JSON fields to top-level attributes (no Grok required).
- Detects and uses these JSON fields as reserved attributes if their names match: `host`, `service`, `source`, `status`, `message`, `timestamp`, `level`, `severity`.
- For `level`/`severity` → `status` is automatically remapped (no status-remapper needed).
- For `timestamp` (RFC 3339 / ISO 8601) → date is auto-remapped to the official timestamp.

This is why "emit JSON logs" is repeatedly the right exam answer — most parsing work disappears.

---

## 3.7 Pipeline filters & best practices

Filter syntax inside *Logs → Configuration → Pipelines* uses the same query syntax as the Log Explorer:

```
service:checkout AND source:java
service:(billing OR orders) env:prod
```

Best practices:

- **Filter narrowly.** A pipeline with filter `*` runs for every log — expensive and slow.
- **Use integration pipelines** when the source has one. Don't reinvent NGINX parsing.
- **Use `category` processors** to bucket numeric values (e.g., latency) for analytics.
- **Test sample logs** in the pipeline editor before saving — Datadog's UI shows the resulting attributes immediately.

---

## 3.8 Exam-style takeaways for Domain 3

- Pipelines run **top to bottom**; a log passes through every pipeline that matches.
- Inside a pipeline, processors run **in declaration order**. Reordering matters — the date remapper must come *after* the grok parser that produced the timestamp field.
- The **Grok parser**, **status remapper**, and **date remapper** are the three processors you must know cold.
- **Reserved attributes** = no `@`, the seven core fields. **Standard attributes** = `@*` with a Datadog-blessed name.
- JSON logs are auto-parsed; `level`/`severity` and `timestamp` JSON fields are auto-remapped.
- Use **standard attribute remappers** to align custom field names to `@http.*`, `@network.*`, etc., so OOTB dashboards work.
- Integration pipelines are triggered by **`source:<name>`** — set `source` in the Agent config and let Datadog do the work.
