# Log Management Fundamentals — Glossary

A–Z definitions of terms that show up on the exam. Keep this open while you take quizzes.

---

**Agent (Datadog Agent)** — Lightweight collector daemon that runs on hosts/containers. For logs, it tails files, listens on TCP/UDP, or reads container streams, then forwards to Datadog. Master switch: `logs_enabled: true` in `datadog.yaml`.

**Annotation (Kubernetes)** — Pod-level metadata key starting with `ad.datadoghq.com/<container-name>.logs` used to customize log collection per pod (source, service, parsing rules).

**Archive** — Long-term cold storage for ingested logs in S3, Azure Blob, or GCS. Stores all ingested logs that match the archive filter, regardless of indexing. Cheap; rehydrate when needed.

**Attribute** — A field inside a log, queried with `@key:value`. Either auto-extracted from JSON or produced by a Grok parser / processor. Distinct from **tags** (which sit on the log envelope).

**Attribute remapper** — Pipeline processor that copies/renames an attribute or moves it to a tag.

**Autodiscovery** — Mechanism by which the Agent discovers containers and applies log/metric configs from labels or annotations.

**Category processor** — Pipeline processor that buckets a numeric or string attribute into named categories (e.g., `latency_bucket: fast/medium/slow`).

**`container_collect_all`** — Agent setting that enables log collection from all containers' STDOUT/STDERR.

**Daily quota** — Per-index hard cap on indexed events/day. Above the quota, logs are still ingested but not indexed.

**Date remapper** — Pipeline processor that sets the official log timestamp from a parsed attribute.

**`datadog.yaml`** — Main Agent config file. Path: `/etc/datadog-agent/datadog.yaml` (Linux) or `C:\ProgramData\Datadog\datadog.yaml` (Windows).

**`dd.trace_id` / `dd.span_id`** — Reserved IDs that link a log to its APM trace/span. Auto-injected when log injection is enabled in the tracer.

**`ddsource`** — Log envelope field carrying the source/integration name (alias of `source` for HTTP intake).

**`ddtags`** — Comma-separated tags attached to a log via the HTTP intake API.

**Distribution metric (generated)** — Metric type produced by Logs to Metrics that preserves percentiles/sum/avg from a numeric attribute.

**Edge filter** — Informal name for an Agent-side `log_processing_rules` of type `exclude_at_match` that drops a log before it leaves the host.

**`env`** — Reserved tag (part of Unified Service Tagging) identifying the deployment environment (`prod`, `staging`, etc.).

**Exclusion filter** — Per-index rule that drops a percentage of matching logs from being indexed (still ingested, archived, used for metrics).

**Facet** — Indexed attribute or tag in the Log Explorer sidebar, usable for click-to-filter and aggregation.

**Flare** — Support bundle produced by `datadog-agent flare`; uploads logs/configs/runtime info to Datadog Support.

**Forwarder (Datadog Forwarder)** — AWS Lambda that ships CloudWatch / S3 / Kinesis logs to Datadog when no Agent is on the source.

**Forwarding (Log Forwarding)** — Datadog feature to push selected logs to external destinations (Splunk, Elastic, HTTP, Sentinel).

**Generate Metrics (Logs to Metrics)** — Feature that creates custom metrics from log volumes/attributes; works on ingested logs even if excluded from indexing.

**GeoIP parser** — Pipeline processor that resolves an IP attribute to country/region/city/lat-long.

**Glob (path)** — Wildcard pattern in `path:` for tailing multiple files (`/var/log/myapp/*.log`). Recursive `**` is not supported.

**Grok parser** — Pipeline processor that extracts attributes from a string using named regex patterns. Match rules vs. support rules.

**Group-by** — Aggregation dimension(s) in the Explorer Analytics view; up to 4 dimensions (1 for Top List).

**HTTP Logs Intake** — REST endpoint `https://http-intake.logs.<site>/api/v2/logs` for Agent-less log submission.

**`include_at_match`** — Edge log_processing_rules type that keeps only matching logs.

**Index** — Searchable retention bucket in Datadog Logs. Has inclusion filter, exclusion filters, retention, daily quota.

**Inclusion filter** — Query that defines which logs enter an index.

**Indexed event** — A log line that has been written to an index and is searchable; the unit of indexing billing.

**Ingestion** — Phase where logs are accepted by Datadog and pass through pipelines. Billed per-GB. Independent of indexing.

**Integration pipeline** — Datadog-built pipeline auto-applied when a log's `source` matches an integration name (`nginx`, `redis`, …). Can be cloned for custom edits.

**Live Tail** — Real-time streaming view of all ingested logs over the last ~15 minutes. Doesn't depend on indexing.

**`log_processing_rules`** — Per-source Agent-side rules: `exclude_at_match`, `include_at_match`, `mask_sequences`, `multi_line`.

**Logging Without Limits™** — Datadog's model that decouples ingestion from indexing so you can ingest all logs cheaply and choose what to index.

**Logs to Metrics** — See *Generate Metrics*.

**Lookup processor** — Pipeline processor that maps an attribute via a small in-pipeline lookup table.

**`mask_sequences`** — Edge log_processing_rules type that redacts substrings via regex with a placeholder.

**Match rule (Grok)** — A named pattern in a Grok parser that produces extracted attributes; first match wins.

**Measure** — Numeric facet used in aggregations (sum, avg, percentiles). Has a unit (ns, ms, bytes, etc.).

**Message** — Reserved attribute holding the headline log text. The `Message remapper` can promote a parsed attribute into the message.

**Multi-Alert** — Monitor option that creates one alert per group (per service, per host, etc.) from a single monitor.

**Multi-line rule** — Edge `log_processing_rules` of type `multi_line` that aggregates lines that don't match the pattern with the previous line that did.

**OOTB pipeline** — Out-of-the-box (integration) pipeline.

**Pattern** — Auto-clustered group of similar logs in the Explorer Patterns tab (variable parts replaced by `*`).

**Pipeline** — Ordered set of processors that run on logs matching the pipeline's filter query. Pipelines run top-to-bottom.

**Processor** — Atomic transformation step inside a pipeline (Grok parser, remapper, lookup, etc.).

**Reference Table** — Managed CSV uploaded to Datadog and queried by reference-table-lookup processors for enrichment.

**Rehydration** — Re-ingesting archived logs into a temporary index so they become searchable again.

**Remapper** — Pipeline processor that promotes/copies/renames attributes (Date, Status, Service, Message, Trace ID, generic).

**Reserved attribute** — One of the seven top-level fields (`host`, `service`, `source`, `status`, `message`, `timestamp`, `tags`) that Datadog promotes for every log.

**Restriction query** — RBAC query on an index that limits which roles can see which logs.

**Retention** — Configurable per-index search window (3, 7, 15, 30, 60, 90 days).

**Saved View** — A stored Explorer state (query + time range + visualization + facet selection).

**Sensitive Data Scanner** — Server-side scrubbing tool with library of PII regexes; redacts/hashes matched substrings post-ingestion.

**`service`** — Reserved tag/attribute identifying the logical service emitting the log. One of Unified Service Tags.

**Site** — Datadog region (`datadoghq.com`, `datadoghq.eu`, `us3.datadoghq.com`, `us5.datadoghq.com`, `ap1.datadoghq.com`, etc.). Wrong site → logs go to wrong region.

**`source`** — Reserved attribute identifying the integration/technology of the log (`nginx`, `python`, `java`). Triggers OOTB pipelines.

**Standard attribute** — Datadog-blessed attribute name in a known namespace (`@http.*`, `@network.*`, `@error.*`, `@usr.*`, `@db.*`).

**Status** — Reserved attribute carrying log severity (`emergency`…`debug`). Set by the Status remapper.

**Status remapper** — Pipeline processor that maps a parsed attribute (typically `level`) onto the official `status`.

**Support rule (Grok)** — Reusable named pattern referenced by match rules.

**Tag** — `key:value` pair attached to logs at collection time. Used for slicing/filtering. Distinct from attributes.

**Timestamp** — Reserved attribute representing when the event occurred. Set by Date remapper or auto-detected JSON `timestamp`.

**Top List** — Single-dimension aggregation visualization (the only viz limited to 1 group-by).

**Transactions** — Explorer feature that groups logs by a correlation attribute (e.g., `request_id`) within a time window.

**Tree Map** — Multi-dimension proportional visualization.

**Unified Service Tagging** — Convention to tag all telemetry with `env`, `service`, `version` for cross-product correlation.

**`version`** — Reserved tag (Unified Service Tagging) identifying the deployment version.

**View In Context** — Explorer button that opens a stream of all logs from the same host/service around a chosen log's timestamp.

**Watchdog** — Datadog's automatic anomaly detection. For logs, surfaces unusual error/log-volume patterns without setup.
