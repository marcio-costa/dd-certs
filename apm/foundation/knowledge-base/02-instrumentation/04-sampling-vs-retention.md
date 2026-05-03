# Sampling vs Retention

> **Domain 2 — Application Instrumentation**
> Conceptually distinct steps that often get confused on exams.

## Big picture: Ingestion control + Retention control

```
   App spans
      │
      ▼
 ┌────────────┐    Sampling decision (head-based)
 │  Tracer    │ ──────────────────────────────────► drop or send to Agent
 └────────────┘
      │
      ▼
 ┌────────────┐    Ingestion sampling (Agent / backend)
 │  Backend   │ ──────────────────────────────────► trace metrics computed for ALL ingested
 └────────────┘
      │
      ▼
 ┌─────────────────┐ Retention filters (custom + intelligent + error/rare)
 │  Indexed store  │ Indexed spans kept 15 days (custom) / 30 days (intelligent)
 └─────────────────┘
```

## Sampling (Ingestion)

Sampling decides **which traces to send from the app** (and from the Agent to the backend).

### Head-based sampling (default)

The decision to keep/drop a trace is made **at the start of the root span** and propagated to all downstream services via trace context. So you must configure sampling rates **on the root service** for them to take effect.

### Sampling mechanisms

| Mechanism                         | Where set                                       | Notes                                                    |
| --------------------------------- | ----------------------------------------------- | -------------------------------------------------------- |
| **Default Datadog sampler**       | Agent — based on `DD_APM_TARGET_TPS` (default 10) | Auto-targets a TPS goal per service.                    |
| **DD_TRACE_SAMPLING_RULES**       | Tracer env var (JSON array)                     | Per-service/resource explicit rates.                     |
| **Manual priority**               | Code: `tracer.set_priority(USER_KEEP)`          | Force keep / force drop on important traces.             |
| **Single Sampling Rate (legacy)** | `DD_TRACE_SAMPLE_RATE`                          | Deprecated; superseded by `DD_TRACE_SAMPLING_RULES`.     |
| **Error & Rare rules**            | Tracer/Agent built-in                           | Always over-sample errors and rare traces.               |

### Sampling rules example

```bash
DD_TRACE_SAMPLING_RULES='[
  {"service":"checkout","resource":"GET /checkout","sample_rate":1},
  {"service":"checkout","sample_rate":0.2}
]'
```

Java equivalent:

```bash
java -Ddd.trace.sampling.rules='[{"service":"checkout","sample_rate":0.5}]' -javaagent:dd-java-agent.jar
```

### Tail-based sampling (ingestion control beta)

Datadog supports tail-based sampling via the Agent's **Ingestion Controls** feature when configured at the Agent level — decisions are made after the trace completes, allowing rule-based "keep all errors", "keep all >1s", etc.

## Retention (Indexing)

Retention decides **which ingested spans to keep searchable**. Sampling and retention are independent — you can ingest 100% and retain 1%, or ingest 5% and retain 100% of those 5%.

### Filter types

1. **Custom retention filters** — user-defined, kept **15 days**.
2. **Intelligent retention filter** — Datadog-managed, kept **30 days**.
3. **Error & rare filters** — enabled by default, kept 15 days.

Filters use the same **search syntax** as the Trace Explorer.

```text
service:checkout @http.status_code:>=500
```

## Trace metrics — always 100%

Trace metrics (`trace.<operation>.hits`, `.errors`, `.duration`) are computed from every ingested trace **before** sampling/retention removes spans. They are stored for **15 months** and unaffected by retention filter changes.

## Decision rules

- "I need long-term analytics" → don't worry about retention; trace metrics carry it.
- "I need to keep the actual spans of all checkouts" → add a **retention filter** matching that resource.
- "My costs are too high" → adjust **sampling rates** (ingestion) and/or **retention filters** (indexed spans).

## Things to remember for the exam

- Sampling = **ingestion** decision; Retention = **indexing/storage** decision.
- Default sampling is **head-based** — set rates on the **root service**.
- Trace metrics are always computed from **100% of ingested** traces and retained **15 months**.
- Indexed spans (custom filter) live for **15 days**.
- Intelligent retention filter keeps spans for **30 days**.
