# Domain 2 — Log Collection

**Exam subtopics:** Enabling Log Collection · Log Filtering & Obfuscation

---

## 2.1 Enabling log collection — the master switch

Log collection is **disabled by default** in the Datadog Agent. To turn it on, edit `datadog.yaml` (the main Agent config file) and set:

```yaml
logs_enabled: true
```

After this change, restart the Agent. Without `logs_enabled: true`, no other log configuration in the Agent has any effect — this is a *very* common exam trap.

### Where `datadog.yaml` lives by platform

| Platform | Path |
|---|---|
| Linux | `/etc/datadog-agent/datadog.yaml` |
| Windows | `C:\ProgramData\Datadog\datadog.yaml` |
| macOS | `~/.datadog-agent/datadog.yaml` |
| Docker container | Set via env var `DD_LOGS_ENABLED=true` |
| Kubernetes (Helm) | `datadog.logs.enabled: true` in values.yaml |

In **containerized environments**, you almost always use environment variables (`DD_LOGS_ENABLED=true`) instead of editing the file.

---

## 2.2 Per-source log configuration

Once logs are enabled globally, you tell the Agent *what* to collect via per-integration `conf.d` files. Each integration that supports logs has a `conf.d/<integration>.d/conf.yaml` file with a `logs:` section.

### Tail a custom log file

`/etc/datadog-agent/conf.d/myapp.d/conf.yaml`:

```yaml
logs:
  - type: file
    path: "/var/log/myapp/app.log"
    service: "checkout-service"
    source: "java"
    sourcecategory: "sourcecode"
    tags:
      - env:prod
      - team:checkout
```

Mandatory fields per entry:

- **`type`** — `file`, `tcp`, `udp`, `journald`, `windows_event`, or `docker`.
- **`path`** — file path (supports glob wildcards like `/var/log/*.log`).
- **`service`** — logical service name (joins logs to APM/RUM via Unified Service Tagging).
- **`source`** — the integration / technology emitting the log (this is what triggers Datadog's **out-of-the-box pipeline**).

Optional but powerful:

- **`tags`** — extra tags attached to every log from this entry.
- **`log_processing_rules`** — pre-ingestion filtering, scrubbing, multi-line aggregation (see §2.5).
- **`encoding`** — for non-UTF-8 files (e.g., `utf-16-le` on Windows).
- **`exclude_paths`** — exclude specific files when path uses a wildcard.

### Listen on a TCP/UDP port (syslog, network gear)

```yaml
logs:
  - type: tcp
    port: 10514
    service: "firewall"
    source: "syslog"
```

### Tail journald

```yaml
logs:
  - type: journald
    path: /var/log/journal/
    include_units:
      - sshd.service
      - kubelet.service
```

### Tail Windows Event Log

```yaml
logs:
  - type: windows_event
    channel_path: "Security"
    source: "windows.security"
    service: "windows"
```

---

## 2.3 Container log collection

### Docker (host-installed Agent)

Set in `datadog.yaml`:

```yaml
logs_enabled: true
logs_config:
  container_collect_all: true
```

Or via env vars on the Agent container:

```
DD_LOGS_ENABLED=true
DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true
```

`container_collect_all: true` tells the Agent to grab logs from **every** container on the host. Per-container customization is done via **Docker labels** or **Pod annotations**.

### Kubernetes (Agent as DaemonSet)

Two collection modes:

1. **Read from log files on the node** (`/var/log/pods/`) — recommended, no socket access needed. Set `containerCollectAll: true` and mount `/var/log/pods/` into the Agent.
2. **Read from the Docker socket** — older, requires socket mount.

To customize per pod, add **annotations**:

```yaml
metadata:
  annotations:
    ad.datadoghq.com/<container-name>.logs: |
      [{
        "source": "java",
        "service": "checkout",
        "log_processing_rules": [
          { "type": "multi_line", "name": "log_start_with_date",
            "pattern": "\\d{4}-\\d{2}-\\d{2}" }
        ]
      }]
```

The annotation key uses **autodiscovery** prefix `ad.datadoghq.com/<container-name>.logs`.

### Container-image-based discovery

Datadog ships **default integration mappings** so common images (e.g., `nginx`, `redis`, `mongo`) get the right `source` automatically. You can override with `DD_CONTAINER_IMAGE_AS_SOURCE` and similar env vars.

---

## 2.4 Cloud-managed log sources (no Agent)

| Cloud service | Common collection method |
|---|---|
| AWS CloudWatch Logs | **Datadog Forwarder** Lambda subscribed to a Log Group |
| AWS CloudTrail | Forwarder reads from S3 trail bucket |
| AWS VPC Flow Logs | Forwarder reads from S3 |
| AWS Lambda function logs | Datadog Lambda Extension (preferred) or Forwarder |
| Azure activity logs | Event Hub → Datadog Azure Integration |
| GCP services | Pub/Sub → Datadog Dataflow template |
| Browser apps | Datadog Browser SDK → HTTP Intake |
| Custom apps (no Agent allowed) | Direct POST to `https://http-intake.logs.<site>/api/v2/logs` |

### HTTP Logs Intake essentials

- Endpoint: `https://http-intake.logs.<site>/api/v2/logs` (where `<site>` is `datadoghq.com`, `datadoghq.eu`, `us3.datadoghq.com`, etc.).
- Headers: `DD-API-KEY: <api_key>` and `Content-Type: application/json`.
- Body: array of JSON objects; each must include `message`, optionally `service`, `host`, `ddsource`, `ddtags`.
- **Max single-payload size:** 5 MB; each individual log up to 1 MB.

---

## 2.5 Log filtering & obfuscation — `log_processing_rules`

Filtering and scrubbing happen at three points; **the exam often asks you to pick the right one**.

| Stage | Where it runs | Use it for | Trade-off |
|---|---|---|---|
| **`log_processing_rules` in the Agent** | Edge (before sending) | Drop logs you never want to ingest; mask PII before it leaves the host | Reduces ingestion bill; lost forever |
| **Pipeline processors** | Datadog backend, after ingestion | Parsing, enrichment, restructure | Logs already ingested; charged for ingestion |
| **Index exclusion filters** | Datadog backend, after pipelines | Stop low-value logs from being indexed (still archived/metrics) | Logs ingested + parsed but not searchable in Explorer |

### `log_processing_rules` types

Defined in the Agent's `conf.yaml`. Four `type` values:

#### 1. `exclude_at_match` — drop the log

```yaml
logs:
  - type: file
    path: /var/log/app.log
    service: web
    source: nginx
    log_processing_rules:
      - type: exclude_at_match
        name: drop_healthchecks
        pattern: GET\s+/health
```

If the regex matches the raw log line, the log is dropped at the Agent — never sent to Datadog, never charged.

#### 2. `include_at_match` — only forward matches

```yaml
log_processing_rules:
  - type: include_at_match
    name: only_errors
    pattern: \s+(ERROR|FATAL)\s+
```

Inverse of exclude — keeps only matching lines. Multiple include rules **must all match**.

#### 3. `mask_sequences` — redact / scrub

```yaml
log_processing_rules:
  - type: mask_sequences
    name: mask_credit_cards
    pattern: \d{4}-\d{4}-\d{4}-\d{4}
    replace_placeholder: "[CREDIT_CARD]"
```

Replaces matched substrings with a placeholder before the log leaves the host. The single best way to handle PII when the log volume is high enough that backend scrubbing would still be a leak risk.

#### 4. `multi_line` — aggregate stack traces

```yaml
log_processing_rules:
  - type: multi_line
    name: stack_trace
    pattern: \d{4}-\d{2}-\d{2}
```

Lines that **don't match** the pattern are appended to the previous line that did. Pattern should match the **start of a new log line** (timestamp prefix, log level, etc.). This is critical for Java/Python/.NET stack traces that span many lines.

### Order of rules

`log_processing_rules` execute **in the order listed** within the same source. Use this to first drop noise, then mask the survivors, then aggregate multi-line.

---

## 2.6 Sensitive Data Scanner (server-side scrubbing)

When you can't do edge masking (e.g., logs come via Lambda Forwarder or HTTP intake), use the **Sensitive Data Scanner** in the Datadog UI:

- Library of out-of-the-box rules: credit cards, SSNs, AWS keys, JWT, IPv4/v6, email, phone numbers (multiple regions).
- Custom rules using regex.
- Apply per **scanning group** (target a service / env / log subset).
- Actions: **Redact**, **Hash**, **Partial redact**, **No action (just tag the match)**.
- Adds a `sensitive_data` tag to scanned logs so you can search for which logs had matches.

This runs *after* the Agent and *before* indexing — it costs ingestion but is searchable post-redaction.

---

## 2.7 Tagging logs

Tags are how you slice and filter every log in Datadog. They flow into logs from multiple sources, **merged in this order** (later overrides earlier):

1. Agent host tags (from `tags:` in `datadog.yaml`)
2. Tags from the integration config (`tags:` in the `conf.yaml`)
3. Tags from environment / metadata (cloud provider, Kubernetes labels)
4. Inline tags in the log itself (`ddtags` field for HTTP intake)

### Reserved (a.k.a. unified service) tags

The trio you must memorize: **`env`, `service`, `version`**. They link logs to APM, RUM, infrastructure, and everything else through **Unified Service Tagging**. Always set them via Agent config or the application environment.

### Cardinality warning

Tags become facets in the Log Explorer. Avoid extremely high-cardinality values (raw user IDs, full URLs with query strings) as tags — put those in attributes instead. Tags ≠ attributes:

- **Tag** — `key:value`, set during collection, indexed for fast filtering.
- **Attribute** — `@field.name`, extracted from log content during processing.

---

## 2.8 Common gotchas (Domain 2)

- **`logs_enabled: true` is required** — without it, every other log setting is ignored.
- The Agent only reads files it has **permission to read**. On Linux, the `dd-agent` user must be able to `read` the log file. Use ACLs (`setfacl`) or add `dd-agent` to a group.
- **Glob paths** in `path:` are supported (`/var/log/myapp/*.log`), but **recursive globs** (`**`) are not — use multiple entries.
- A single integration `conf.yaml` can define **multiple** entries under `logs:` (it's a YAML list).
- For containers, **labels and pod annotations override** Agent defaults — use them for per-workload customization.
- The HTTP Logs Intake API uses the **logs intake** endpoint (`http-intake.logs.<site>`), **not** `api.<site>` — common exam distractor.
- Docker `container_collect_all: true` collects **STDOUT/STDERR**, not files inside the container.
