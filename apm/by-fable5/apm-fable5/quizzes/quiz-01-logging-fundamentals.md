# Quiz 01 — Logging Fundamentals (Domain 1)

**Coverage:** logging systems rationale · log sources · logging formats · log emission
**Length:** 12 questions
**Answer key at the bottom — fold the page or scroll down only after you've answered.**

---

### 1. Which of the following best describes the role of logs in observability?

A) Logs measure the performance of a system over time using numeric values.
B) Logs are immutable, timestamped event records that provide context about *why* something happened.
C) Logs aggregate request flows across services to show *where* latency originates.
D) Logs are the only signal needed to diagnose distributed systems.

### 2. A team is designing a new microservice. They want minimal pipeline configuration in Datadog. Which log format is recommended?

A) Plaintext free-form messages.
B) `key=value` (logfmt) lines.
C) Structured JSON.
D) XML documents.

### 3. What does *Logging Without Limits™* mean?

A) Datadog imposes no maximum on the size of an individual log.
B) Datadog decouples log ingestion cost from log indexing/search cost.
C) Datadog removes its 1 MB log line limit on premium plans.
D) Datadog allows unlimited log retention by default.

### 4. Which two correlation IDs must be present in a log line for it to appear in the **Trace side panel** of an APM trace? (Select TWO)

A) `dd.trace_id`
B) `dd.host_id`
C) `dd.span_id`
D) `dd.run_id`
E) `dd.org_id`

### 5. A network appliance can only forward logs via syslog. Which of the following is a valid Datadog collection path?

A) The appliance writes to a local file the Datadog Agent tails.
B) The appliance forwards UDP/TCP syslog to a port the Datadog Agent listens on.
C) The appliance sends a Datadog StatsD metric per log line.
D) The appliance pushes logs over RDP to the Agent.

### 6. Which Datadog log severity (status) value is reserved for the most critical events?

A) `critical`
B) `alert`
C) `emergency`
D) `error`

### 7. A serverless application running on AWS Lambda needs to send logs to Datadog with no Agent. Which is the most common path?

A) Install the Datadog Agent inside each Lambda invocation.
B) Use the Datadog Forwarder Lambda or the Datadog Lambda Extension.
C) POST every log to `api.datadoghq.com/v1/series`.
D) Have Lambda write logs to S3, then mount S3 on the Agent.

### 8. Which statement about ingestion vs. indexing is correct?

A) All ingested logs are automatically indexed.
B) Logs that are excluded from an index are also excluded from archives and metric generation.
C) A log can be ingested and archived without ever being indexed.
D) Indexing happens before pipeline processing.

### 9. Place the following log lifecycle stages in the correct order:

1. Index
2. Pipelines
3. Ingestion
4. Source

A) 4 → 3 → 2 → 1
B) 4 → 2 → 3 → 1
C) 3 → 4 → 1 → 2
D) 1 → 2 → 3 → 4

### 10. Which TWO of the following are valid Datadog log sources? (Select TWO)

A) `/var/log/syslog` tailed by the Agent
B) Browser SDK posting to HTTP intake
C) DogStatsD metric submissions
D) AWS X-Ray spans
E) Direct posts to `api.datadoghq.com/api/v1/check_run`

### 11. Which feature decouples ingestion from indexing so you can ingest a high-volume access log cheaply but only index a 10% sample?

A) Sensitive Data Scanner.
B) Index exclusion filters.
C) Pipelines.
D) Live Tail.

### 12. A team logs every transaction with a `level=ERR` field. They notice the Datadog Explorer shows `status:error` automatically without any custom pipeline. Which of the following best explains this?

A) Datadog ignores the `level` attribute and infers status from log volume.
B) Datadog's auto-JSON parsing recognizes JSON keys named `level`/`severity` and maps them to the official `status`.
C) The Agent's `level_to_status` rule does the mapping.
D) Datadog generates a Status remapper at random for each new source.

---

## Answer Key

1. **B** — Logs answer the *why* (full context). Metrics answer the *what* (counts/rates), traces answer the *where* (which span). A is metrics; C is traces; D is wrong because logs are one of three pillars.

2. **C** — Datadog auto-parses JSON: each top-level key becomes an attribute, and recognized keys (`level`, `severity`, `timestamp`) are auto-mapped to reserved attributes. No Grok required.

3. **B** — *Logging Without Limits* is the architectural choice to charge ingestion cheaply and indexing more selectively, so log *volume* and log *cost* are no longer linked.

4. **A, C** — Trace–log correlation needs `dd.trace_id` and `dd.span_id`. (`service` and `env` must also match, but they're tags, not the IDs the question asks about.)

5. **B** — The Agent supports `type: tcp` and `type: udp` listeners on a configured port for syslog and similar. (A) is also possible if the device wrote to a file, but the prompt says "only via syslog".

6. **C** — Datadog's reserved status hierarchy goes `emergency, alert, critical, error, warning, notice, info, debug`. `emergency` is the most severe.

7. **B** — The Datadog Forwarder Lambda (subscribed to a CloudWatch Log Group) and the Datadog Lambda Extension are the standard paths. The Agent doesn't run inside Lambda invocations (A); the metrics API (C) is wrong.

8. **C** — Archives capture all ingested logs regardless of indexing; logs to metrics also work on ingested-but-excluded logs. (A) is false (you choose what to index); (B) is false (excluded logs still hit archives + metrics); (D) is false (pipelines run before indexing).

9. **A** — Source → Ingestion → Pipelines → Index. Pipelines run *after* ingestion (parsing/enriching) and *before* indexing.

10. **A, B** — File tailing and the Browser SDK are valid log sources. (C) is metrics; (D) is APM traces; (E) is service checks.

11. **B** — Index exclusion filters can sample down a high-volume source by percentage. Sensitive Data Scanner (A) scrubs PII. Pipelines (C) parse but don't drop indexed events.

12. **B** — Datadog's auto-JSON parser detects JSON fields `level`/`severity` and remaps to the reserved `status`. (`timestamp` is similarly auto-detected for the date.)

---

**Score yourself:** Count correct answers (multi-select counts only if both are right). Anything below 9/12 → re-read `knowledge-base/01-logging-fundamentals.md` and add the missed concepts to `progress-tracking/weak-areas.md`.
