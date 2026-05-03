# Log Management Fundamentals — Quick Reference (Exam Day Cheat Sheet)

Print this. One page of the things that show up on every practice run.

---

## The seven reserved attributes (no `@`)

`host` · `service` · `source` · `status` · `message` · `timestamp` · `tags`

## Status (severity) values

`emergency` · `alert` · `critical` · `error` · `warning` · `notice` · `info` · `debug` · `OK`

## Standard attribute namespaces

`@http.*` · `@network.*` · `@error.*` · `@usr.*` · `@db.*` · `@duration` · `@trace_id` / `@span_id`

---

## Master switches

| Setting | Purpose |
|---|---|
| `logs_enabled: true` | Enable log collection in `datadog.yaml` (master switch) |
| `DD_LOGS_ENABLED=true` | Same, via env var (containers) |
| `container_collect_all: true` | Collect logs from every container on the host |
| `logs_config.container_collect_all: true` | YAML form of above |

---

## Agent log entry skeleton

```yaml
logs:
  - type: file        # or tcp / udp / journald / windows_event / docker
    path: "/var/log/myapp/app.log"
    service: "checkout"
    source: "java"
    tags: ["env:prod", "team:checkout"]
    log_processing_rules:
      - type: exclude_at_match
        name: drop_health
        pattern: "GET /health"
      - type: mask_sequences
        name: mask_cc
        pattern: '\d{4}-\d{4}-\d{4}-\d{4}'
        replace_placeholder: "[CC]"
      - type: multi_line
        name: stack_trace
        pattern: '\d{4}-\d{2}-\d{2}'
```

---

## `log_processing_rules` types (Agent edge)

| Type | Effect |
|---|---|
| `exclude_at_match` | Drop log if pattern matches |
| `include_at_match` | Keep only logs that match |
| `mask_sequences` | Replace matched substring with placeholder |
| `multi_line` | Lines NOT matching pattern join previous line |

---

## Pipeline processors — order convention

1. **Grok parser** (extract from text)
2. **Date remapper** (set timestamp)
3. **Status remapper** (set severity)
4. **Service / Message remapper** (set reserved fields)
5. **Standard attribute remappers** (rename to `@http.*`, etc.)
6. **Enrichment** (GeoIP, User-Agent, lookup, category, arithmetic)
7. **Restructure** (attribute remapper, string builder)

---

## Search syntax cheat sheet

```
# Tags / reserved (no @)
service:checkout env:prod host:web-01 status:error source:nginx

# Attributes (with @)
@http.status_code:500
@http.url:/api/v1/orders
@duration:>1000000000

# Ranges  [inclusive]  {exclusive}
@http.status_code:[500 TO 599]
@duration:{1000 TO *]

# Booleans (uppercase)
service:checkout AND status:error
status:(error OR warn)
service:web NOT @http.status_code:200

# Wildcards
service:payment-*
@http.url:/api/v1/users/*
host:web-?

# Existence
@usr.id:*       # exists
-@usr.id:*      # missing

# Free text (matches message)
"connection refused"
```

---

## Live Tail vs. Explorer (memorize the differences)

| | **Live Tail** | **Explorer** |
|---|---|---|
| Window | Last ~15 min | Index retention |
| Sees | All ingested logs | Indexed only |
| Aggregations | No | Yes |
| Saved views | No | Yes |
| Use for | Real-time validation | Investigation, dashboards, monitors |

---

## Index, exclusion, archive — what hits which

| Stage | Affects | Cost lever |
|---|---|---|
| Agent `exclude_at_match` | Drop **before** Datadog sees it | ↓ ingestion bill |
| Pipeline processors | Parse / enrich (everything still ingested) | none |
| Index inclusion | Routes to that index | none |
| Index exclusion (% drop) | Drop **after** ingestion, **before** indexing | ↓ index bill |
| Archive | All ingested logs to S3/Blob/GCS | $$ to your cloud, not Datadog |
| Generate Metrics | Works on **all ingested** logs (incl. excluded) | adds custom metrics |
| Forwarding | Push to Splunk/Elastic/HTTP/Sentinel | n/a |

---

## Visualizations & group-by limits

| Viz | Max group-bys |
|---|---|
| Top List | **1** |
| Timeseries | 4 |
| Table | 4 |
| Tree Map | 4 |
| Pie Chart | 4 |
| Geomap | 4 |

---

## Compute aggregations

`count` · `cardinality(<facet>)` · `sum(measure)` · `avg(measure)` · `min/max(measure)` · `pcXX(measure)` (e.g., `pc95`)

---

## Log monitor query template

```
logs("service:checkout status:error")
  .index("main")
  .rollup("count")
  .last("5m") > 50
```

---

## Sizing limits

| Limit | Value |
|---|---|
| Single log line | 1 MB |
| HTTP intake payload | 5 MB |
| Logs to Metrics group-by tags | 10 |
| Index retention options | 3, 7, 15, 30, 60, 90 days |
| Live Tail window | ~15 min |
| Pipeline filter syntax | Same as Explorer |

---

## Top troubleshooting commands

```bash
sudo datadog-agent status                  # all components incl. Logs Agent
sudo datadog-agent flare <ticket-id>       # support bundle
sudo datadog-agent stream-logs              # see logs as the Agent sees them
sudo -u dd-agent cat <path>                # check read permission
```

Increase verbosity: `log_level: debug` in `datadog.yaml` (revert in prod).

---

## Symptom → first place to look

| Symptom | First check |
|---|---|
| No logs at all | `agent status` → is logs collection enabled & sending? |
| In Live Tail, not Explorer | Index exclusion filters / inclusion routing |
| Wrong timestamp | Date remapper after grok |
| Wrong status | Status remapper coverage |
| `unknown` source | Source name vs. OOTB pipeline expectations |
| Stack trace split | `multi_line` rule pattern |
| Permission denied | `dd-agent` ACLs on file & parent dir |
| K8s logs missing | `/var/log/pods` mount; annotation container name |
| Logs in another region | `site:` setting in `datadog.yaml` |

---

## Sites

| Region | Site value |
|---|---|
| US1 | `datadoghq.com` |
| US3 | `us3.datadoghq.com` |
| US5 | `us5.datadoghq.com` |
| EU1 | `datadoghq.eu` |
| AP1 | `ap1.datadoghq.com` |
| US1-FED | `ddog-gov.com` |
