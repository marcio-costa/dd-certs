# Domain 1 — Logging Fundamentals

**Exam subtopics:** Logging Systems (rationale & justification) · Log Sources · Logging Formats · Log Emission

---

## 1.1 Why logging matters (Logging Systems — rationale)

Logs are immutable, timestamped records of events that occur inside a system. In observability, they are one of the **three pillars** alongside metrics and traces:

| Pillar | Question it answers | Cardinality | Cost driver |
|---|---|---|---|
| **Metrics** | *What* is happening? (rates, errors, latency) | Low | Number of unique tag combinations |
| **Traces** | *Where* is it happening? (which service / span) | Medium | Number of indexed spans |
| **Logs** | *Why* is it happening? (full context, message text) | High | Volume ingested + volume indexed |

Logs are the highest-fidelity signal: they capture full event context (stack traces, request payloads, user IDs). The trade-off is volume — modern systems can produce TB/day of logs.

**Key reasons organizations centralize logs in Datadog:**

- **Troubleshooting** — pivot from a metric alert or trace to the underlying log line in one click.
- **Auditing & compliance** — retain logs to meet PCI-DSS, HIPAA, SOC 2, GDPR retention requirements (use **Archives** for long-term cold storage).
- **Security** — feed Cloud SIEM detection rules off log streams.
- **Business analytics** — derive metrics from logs (e.g., orders per minute) without modifying app code.
- **Cost control via Logging Without Limits™** — ingest *all* logs cheaply, then choose which to index for high-cost search.

### Logging Without Limits™

Datadog's defining concept: **decouple ingestion from indexing**. You pay a low rate to ingest every log, then use **indexes** + **exclusion filters** + **retention** policies to choose which subset is searchable in the Log Explorer. Everything else can still be:

- Live-tailed in real time (15-min window)
- Used to generate custom metrics
- Sent to long-term **Archives**
- Forwarded to third-party destinations

This means log *volume* and log *cost* are no longer linked.

---

## 1.2 Log sources

A **log source** is anywhere a log line originates. Datadog supports collection from a very broad set of sources; the exam expects you to recognize the categories.

### Application logs
Logs your code emits. Two emission patterns:

1. **Write to a file** (most common) — the Datadog Agent tails the file and forwards lines.
2. **Write to STDOUT/STDERR** (12-factor / containers) — the container runtime captures the stream; the Agent reads from the container engine.

### Host / system logs
- **Linux:** `/var/log/syslog`, `/var/log/messages`, `/var/log/auth.log`, journald.
- **Windows:** Windows Event Log channels (`System`, `Application`, `Security`).
- **macOS:** Unified Logging via `/var/log/system.log`.

### Container logs
- **Docker** — Agent collects from the Docker socket or via the `/var/lib/docker/containers/` JSON files when **Docker log collection** is enabled.
- **Kubernetes** — Agent runs as a DaemonSet and reads pod log files under `/var/log/pods/`. Annotations on pods customize parsing.
- **Containerd / CRI-O** — same model, files under `/var/log/pods/`.

### Cloud / managed services
- **AWS** — CloudTrail, VPC Flow Logs, ELB, S3 access logs, RDS, Lambda — typically ingested via the **Datadog Forwarder** Lambda or a Kinesis Firehose.
- **Azure** — via Activity Logs / Event Hub / Diagnostic Settings + the Datadog integration.
- **GCP** — via Pub/Sub + Datadog Dataflow template.

### Network appliances & proxies
Devices that emit **syslog** (RFC 3164 / RFC 5424). The Agent listens on a TCP/UDP port and forwards.

### Custom sources via API
Apps can POST directly to the **HTTP Logs Intake API** (`https://http-intake.logs.datadoghq.com/api/v2/logs`). No Agent required — useful for serverless / browser RUM logs.

### Browser & mobile logs
The **Datadog Browser SDK** and **Mobile RUM SDKs** ship logs directly to Datadog over HTTP.

---

## 1.3 Log formats

The exam expects you to recognize the trade-offs between formats and how Datadog handles them.

### Unstructured (raw text)

```
2026-05-03 14:22:01 INFO Order 4521 created for user 99
```

- **Pro:** Easiest to write; what most legacy apps emit.
- **Con:** Requires a **Grok parser** in a Datadog pipeline to extract fields. Fragile when log format changes.

### Semi-structured (key=value, logfmt)

```
ts=2026-05-03T14:22:01Z level=info msg="order created" order_id=4521 user_id=99
```

- **Pro:** Easy to read, easy to parse with built-in **key/value (logfmt) parser** processor.
- **Con:** Field types are still strings; numeric fields may need a remapper.

### Structured (JSON)

```json
{"ts":"2026-05-03T14:22:01Z","level":"info","msg":"order created","order_id":4521,"user_id":99}
```

- **Pro:** **Datadog automatically parses JSON** at ingestion — every top-level key becomes an attribute. No pipeline required for basic extraction.
- **Pro:** Keeps numeric and boolean types intact.
- **Con:** Slightly larger payload than logfmt.
- **Recommendation:** For new applications, **always emit JSON**. This is repeatedly the right answer on exam scenario questions.

### Common-Log / Combined-Log (web servers)

```
127.0.0.1 - frank [03/May/2026:14:22:01 +0000] "GET /api/v1/users HTTP/1.1" 200 2326
```

Standard format for Apache / NGINX. Datadog ships **out-of-the-box integration pipelines** that parse these for you when `source: nginx` or `source: apache` is set.

### Syslog (RFC 3164 / RFC 5424)

Used by network gear and many *nix services. The Agent can listen on a TCP/UDP socket; the integration pipeline handles the format.

---

## 1.4 Log emission

How an application or system *gets the log out*. Patterns the exam tests:

| Emission method | Description | When to use |
|---|---|---|
| **File** | App writes to a log file; Agent tails it. | Default for VMs, traditional apps. |
| **STDOUT/STDERR** | App prints; container runtime captures. | Containers (12-factor). |
| **Direct to Agent (TCP/UDP)** | App sends over a socket. | Apps that can't write files; appliances. |
| **HTTP Intake API** | App POSTs directly to `http-intake.logs.datadoghq.com`. | Serverless, browser, edge. |
| **Lambda Forwarder** | AWS-only Lambda that ships CloudWatch / S3 logs. | AWS-managed services. |
| **Vector / Fluent Bit / Fluentd** | Third-party agent forwards to Datadog. | Existing log pipelines you don't want to replace. |
| **Cloud-native (Azure Event Hub, GCP Pub/Sub)** | Cloud streams to Datadog integration. | Azure / GCP managed-service logs. |

### Logging libraries to know

- **Python:** `logging` module + `python-json-logger`. Datadog APM auto-injects `dd.trace_id`/`dd.span_id` for log–trace correlation.
- **Java:** SLF4J + Logback / Log4j2. Use `MDC` for context. Datadog tracer auto-injects trace IDs.
- **Node.js:** `winston`, `pino`, `bunyan`. The `dd-trace` module auto-injects trace IDs when log injection is enabled.
- **Go:** `slog`, `zap`, `logrus`. Set `DD_LOGS_INJECTION=true` to auto-correlate.
- **.NET:** `Microsoft.Extensions.Logging`, `Serilog`, `NLog`.

### Trace–log correlation (memorize)

For a log line to appear linked to its trace inside the **Trace Side Panel**, it must contain:

- `dd.trace_id`
- `dd.span_id`
- `service` and `env` matching the APM service

These are typically auto-injected by enabling **log injection** in the tracer (`DD_LOGS_INJECTION=true` for most languages).

---

## 1.5 Log levels (severity)

Datadog's reserved severity levels and their numeric weights (used by the Status remapper and search):

| Status | Aliases the remapper accepts | Typical numeric |
|---|---|---|
| `emergency` | `emerg`, `0` | 0 |
| `alert` | `1` | 1 |
| `critical` | `crit`, `2` | 2 |
| `error` | `err`, `3` | 3 |
| `warning` | `warn`, `4` | 4 |
| `notice` | `5` | 5 |
| `info` | `6` | 6 |
| `debug` | `7` | 7 |
| `OK` | (Datadog-specific synonym for OK status) | — |

`@status` is the **standard attribute** used after the status-remapper has run.

---

## 1.6 The log lifecycle in Datadog

Memorize this pipeline; many exam questions hinge on it:

```
[Source]
   │
   │ (Agent / API / Forwarder)
   ▼
[Ingestion]  ◀─── all logs pay ingestion fees
   │
   ▼
[Processing pipelines]   ◀─── grok, remappers, lookup, category
   │
   ├─► Live Tail (15-min window — sees all ingested logs)
   ├─► Generate Metrics (custom metrics from log volume/content)
   ├─► Archives (long-term cold storage in S3/Azure/GCS)
   ├─► Forwarding (push to external destinations)
   │
   ▼
[Indexes]   ◀─── only indexed logs cost search/retention $$
   │
   ▼
[Log Explorer]  ◀─── search, dashboards, monitors run here
```

Two things to remember:

1. **All ingested logs flow through processing pipelines**, even if you ultimately exclude them from the index.
2. **Exclusion filters live on indexes**, *after* processing — so excluded logs are still parsed, can still generate metrics, can still go to archives.

---

## 1.7 Compliance & retention essentials

| Need | Datadog feature |
|---|---|
| Searchable retention | **Index retention** (3, 7, 15, 30, 60, 90 days configurable) |
| Long-term cheap storage | **Log Archives** (S3 / Azure Blob / GCS) |
| Search archived logs again | **Rehydration** from archives into a temporary index |
| Mask PII in logs | **Sensitive Data Scanner** (or processor scrubbing rules) |
| Restrict who can see what | **RBAC** + **restriction queries** on indexes |

---

## 1.8 Exam-style takeaways for Domain 1

- Logs answer **why**; metrics and traces answer *what* and *where*.
- For new apps, **emit JSON** — Datadog parses it automatically with no pipeline work.
- **Logging Without Limits™ = decouple ingestion from indexing.**
- **All logs flow through pipelines first, then indexes.** Exclusion filters live on indexes.
- A log line is correlated to a trace only if it carries `dd.trace_id` + `dd.span_id` + matching `service`/`env`.
- For containers, the Agent reads STDOUT/STDERR via the runtime; for VMs, it tails files.
- Datadog supports nine reserved log statuses (`emergency`…`debug`) plus `OK`.
