# Full Mock Exam #2 — Datadog Log Management Fundamentals

**Length:** 75 questions
**Time:** 2 hours
**Format:** Multiple choice (single answer unless "Select TWO/THREE" stated)
**Different from Mock #1.** Take this near the end of your prep (Day 25).

---

## Domain 1 — Logging Fundamentals (Q1–Q11)

**Q1.** A team operating a high-traffic e-commerce site argues that logs are unnecessary because they have metrics + traces. Which is the strongest counter-argument?

A) Logs are mandatory under PCI-DSS even when other telemetry exists.
B) Metrics + traces don't capture full event context (request payloads, stack traces); logs answer *why*.
C) Datadog includes free log storage on all plans.
D) Logs are required to compute service maps.

**Q2.** A development team adopts JSON logging in a new microservice. What does Datadog's auto-JSON parsing **not** do automatically? (Select ONE)

A) Promote top-level keys to attributes.
B) Map `level` / `severity` to `status`.
C) Map `timestamp` to the official log timestamp.
D) Create custom facets for arbitrary nested keys so they appear in the facet sidebar.

**Q3.** Logs from an EC2 instance reach Datadog with `host:i-0abc1234`. The team prefers a friendly hostname. Which configuration mechanism is the right place to set it?

A) Add `host:` in `datadog.yaml` (`hostname:` field).
B) Set `DD_HOSTNAME` environment variable on the Agent.
C) Either A or B (both override the auto-detected EC2 instance ID).
D) Datadog forces `host` to the cloud-provider ID and cannot be changed.

**Q4.** Which TWO of the following are valid log emission methods to Datadog? (Select TWO)

A) Browser SDK posting to HTTP intake.
B) Agent reading container STDOUT/STDERR.
C) DogStatsD UDP packets.
D) ICMP echo packets carrying payload.
E) Agentless device polling Datadog every 30 seconds.

**Q5.** Which is **not** part of "Logging Without Limits™"?

A) Decoupling ingestion from indexing.
B) Generating metrics from logs that are not indexed.
C) Archiving every ingested log to S3/Blob/GCS.
D) Eliminating the per-log line size limit.

**Q6.** A team logs in Common Log Format from NGINX. The simplest path to structured fields is:

A) Write a Grok parser from scratch.
B) Set `source: nginx` so the OOTB integration pipeline parses the logs.
C) Switch NGINX to JSON logging.
D) Use a Lookup processor.

**Q7.** A web app emits logs from the user's browser. The recommended ingestion path is:

A) Pass logs through a backend proxy that calls the HTTP intake.
B) Datadog Browser SDK direct to the intake.
C) Browser-based DogStatsD library.
D) Logs are not supported from browsers.

**Q8.** Which TWO statements about reserved attributes are correct? (Select TWO)

A) They include `host`, `service`, `source`, `status`, `message`, `timestamp`, `tags`.
B) They are searched without an `@` prefix.
C) They are searchable only after declaration in *Logs → Configuration → Standard Attributes*.
D) `@http.method` is a reserved attribute.
E) `@usr.id` is a reserved attribute.

**Q9.** Logs that are routed via the Datadog Forwarder Lambda from CloudWatch Log Groups are typically tagged with which `source`?

A) `lambda`
B) The CloudWatch service prefix derived from the Log Group name (e.g., `aws.lambda`).
C) Always `cloudwatch`.
D) `unknown` until manually tagged.

**Q10.** A team needs a log entry to be tied to a span in APM. Which TWO elements are required on the log? (Select TWO)

A) `dd.trace_id`
B) `dd.span_id`
C) `dd.host_id`
D) `dd.org_id`
E) Matching `service` and `env` tags

**Q11.** Datadog reserved log status values include all of the following EXCEPT:

A) `notice`
B) `info`
C) `verbose`
D) `debug`

---

## Domain 2 — Log Collection (Q12–Q20)

**Q12.** A team sets `DD_LOGS_ENABLED=true` on the Datadog Agent container in Kubernetes. They also set `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true`. Which TWO statements are TRUE? (Select TWO)

A) Logs from every container on the node will be collected.
B) Pod annotations can override the source/service per container.
C) The Agent must be installed inside every workload pod.
D) Files inside containers will be collected automatically.
E) Logs collected this way bypass pipelines.

**Q13.** A team configures `path: /var/log/myapp/*.log` and one of the rotated files is named `app.log.1.gz`. The Agent:

A) Reads gzip files in real time.
B) Does not auto-decompress `.gz` rotated files.
C) Decompresses the file once at startup.
D) Renames the file before reading.

**Q14.** The Agent rule type that masks credit-card numbers before they leave the host is:

A) `mask_sequences`
B) `obfuscate`
C) `redact`
D) `replace`

**Q15.** A team adds an `include_at_match` rule with `pattern: ERROR`. Which logs reach Datadog?

A) All logs.
B) Only logs whose raw line contains "ERROR".
C) Only logs with `status:error`.
D) None.

**Q16.** A `multi_line` rule's `pattern` is `^\d{4}-\d{2}-\d{2}` and the log file contains a stack trace where lines start with `at com.example...`. The result:

A) Each `at com.example...` line is a separate log.
B) The `at com.example...` lines are appended to the last line that matched the date pattern, forming one log.
C) The rule fails because lookahead is needed.
D) The file is excluded.

**Q17.** A team forwards logs from on-prem servers using Vector. Datadog supports this because Vector has a Datadog sink that posts to:

A) The Agent over UDP.
B) The HTTP Logs Intake API directly.
C) The metrics API.
D) The Datadog SaaS Forwarder.

**Q18.** A team uses Sensitive Data Scanner. Which is TRUE?

A) It runs on the Agent.
B) It runs in the Datadog backend after ingestion and before indexing.
C) It only works on logs sent via HTTP intake.
D) It produces a separate "scrubbed" copy alongside the original.

**Q19.** Which TWO are valid actions for a Sensitive Data Scanner rule? (Select TWO)

A) Redact (replace with placeholder).
B) Hash (one-way replacement).
C) Encrypt with customer KMS.
D) Append a tag without modifying the value.
E) Drop the log entirely.

**Q20.** A team uses Unified Service Tagging. The three reserved tags are:

A) `host`, `region`, `cluster`.
B) `env`, `service`, `version`.
C) `team`, `app`, `tier`.
D) `prod`, `dev`, `qa`.

---

## Domain 3 — Log Parsing (Q21–Q34)

**Q21.** A pipeline can be nested inside another pipeline. The behavior is:

A) The inner pipeline runs always.
B) The inner pipeline runs only if the outer pipeline's filter matched.
C) The inner pipeline is ignored.
D) Nesting requires Datadog Premium.

**Q22.** Which processor is **best** to add a new attribute that says `latency_bucket: fast/medium/slow` based on `@duration`?

A) Category processor.
B) Arithmetic processor.
C) String builder.
D) Date remapper.

**Q23.** Which Grok matcher captures any non-whitespace token as a string?

A) `%{notSpace:value}`
B) `%{word:value}`
C) `%{data:value}`
D) `%{regex(...):value}`

**Q24.** Which Grok filter sets the value to null if it equals a literal string (e.g., `-`)?

A) `:nullIf("-")`
B) `:keyvalue`
C) `:json`
D) `:scale(0)`

**Q25.** A team has parsed `level` from a log via Grok. They want this to set the official log severity (color in the Explorer). The processor:

A) Date remapper.
B) Status remapper sourcing `level`.
C) Service remapper.
D) Message remapper.

**Q26.** A team wants `country_code` available as a tag derived from `@network.client.ip`. Two processors are usually used in sequence. They are:

A) GeoIP parser, then Status remapper.
B) GeoIP parser, then a generic Remapper that copies `@network.client.geoip.country.iso_code` to a tag (or a custom standard attribute).
C) URL parser, then Date remapper.
D) Lookup processor only.

**Q27.** Which TWO processors can produce a *new attribute* whose value is computed from one or more existing attributes? (Select TWO)

A) Arithmetic processor.
B) String builder.
C) Status remapper.
D) Date remapper.
E) GeoIP parser (sets new attributes derived from one input).

**Q28.** Why might integration pipelines not parse logs as expected?

A) The integration is paused for the org.
B) The `source:` value at collection doesn't match the integration name (e.g., `Nginx` instead of `nginx`).
C) Pipelines run before ingestion.
D) Index retention is too short.

**Q29.** Which is the correct relationship between **integration pipelines** and **custom pipelines**?

A) Integration pipelines run only after all custom pipelines.
B) Integration pipelines and custom pipelines coexist; both can be reordered, except integration pipelines can be cloned but not edited in place.
C) Integration pipelines override custom pipelines silently.
D) Custom pipelines must be enabled per-integration.

**Q30.** Which is a *standard attribute* (not reserved)?

A) `@network.client.ip`
B) `host`
C) `tags`
D) `message`

**Q31.** A custom JSON log has `level: "WARN"`. Without any custom pipeline, what does the official `status` show?

A) `info`
B) `warn` (auto-mapped from `level`).
C) Empty.
D) `error`

**Q32.** The reserved attribute `message` is shown in the Explorer as:

A) The headline log line displayed for each row.
B) A side panel only.
C) An archive metadata field.
D) An auto-generated UUID.

**Q33.** Which Grok pattern correctly parses an ISO-8601 timestamp into the `timestamp` attribute?

A) `%{date("yyyy-MM-dd HH:mm:ss"):timestamp}`
B) `%{date("yyyy-MM-dd'T'HH:mm:ss.SSSZ"):timestamp}`
C) Either A or B depending on the actual log format.
D) `%{date("ISO8601"):timestamp}`

**Q34.** Which TWO statements about pipelines are TRUE? (Select TWO)

A) A log can be enriched by multiple pipelines if multiple filters match.
B) Pipelines stop processing on the first filter match.
C) Pipelines support nested pipelines (one level deep).
D) Pipelines run in alphabetical order.
E) Pipelines run before ingestion.

---

## Domain 4 — Log Searching & Filtering (Q35–Q47)

**Q35.** Which TWO queries are equivalent? (Select TWO)

A) `service:checkout AND status:error`
B) `service:checkout status:error`
C) `service:checkout OR status:error`
D) `+service:checkout +status:error`
E) `service:checkout NOT status:error`

**Q36.** A team writes `@duration:>1000`. Which statement is TRUE?

A) Matches logs where `@duration` > 1000 (any unit).
B) The unit is whatever was declared on the measure.
C) Only valid for integers.
D) Datadog rejects the syntax.

**Q37.** To exclude logs from a specific host:

A) `NOT host:web-01`
B) `-host:web-01`
C) Either A or B.
D) `host:NOT web-01`

**Q38.** Which is TRUE about leading wildcards (`*foo`)?

A) They are not supported.
B) They are supported but slower; avoid where possible.
C) They are required for partial match.
D) They auto-expand to regex.

**Q39.** A team notices the Explorer is auto-suggesting facet values for `team` but not for `request_id`. The likely reason:

A) `request_id` has too few values.
B) `team` is a faceted tag; `request_id` has not been added as a facet.
C) `request_id` is a reserved attribute.
D) Datadog only auto-suggests for tags.

**Q40.** Which is the **maximum number of group-by dimensions** in a Pie Chart visualization?

A) 1
B) 2
C) 4
D) 8

**Q41.** A team uses `View In Context`. Which logs appear?

A) Logs with the same `dd.trace_id` only.
B) Logs from the same `host` and `service` around the chosen log's timestamp.
C) All logs from the org around that time.
D) Only logs containing the same `message` text.

**Q42.** A Saved View URL can be parameterized to override the query at open time. The right URL parameter is:

A) `?filter=...`
B) `?query=...`
C) `?search=...`
D) `?q=...`

**Q43.** A team wants to find logs whose `@http.url` contains `/api/v1/users/` followed by a numeric ID. Which TWO queries match? (Select TWO)

A) `@http.url:/api/v1/users/*`
B) `@http.url:"/api/v1/users/123"` (exact match for one)
C) `@http.url:[/api/v1/users/0 TO /api/v1/users/999]`
D) `@http.url:/api/v1/users/?`
E) `@http.url:*api*v1*users*`

**Q44.** The Patterns tab uses which attribute by default?

A) `host`
B) `service`
C) `message`
D) `timestamp`

**Q45.** Which TWO statements about Transactions are correct? (Select TWO)

A) They group logs by a correlation attribute over a time window.
B) They are limited to logs with the same `dd.trace_id`.
C) They support a configurable max duration.
D) They auto-create facets for the grouping attribute.
E) They require APM to be enabled.

**Q46.** A search bar query `@http.status_code:5*` matches:

A) Only `500`.
B) Any string starting with `5` in `@http.status_code`.
C) The exact string `5*`.
D) Datadog rejects the wildcard.

**Q47.** What is the most efficient way to run the same query against the index every morning and view it in the Explorer?

A) Bookmark the URL.
B) Save it as a Saved View.
C) Build a notebook cell.
D) Create a custom pipeline.

---

## Domain 5 — Log Analysis (Q48–Q57)

**Q48.** Excluded logs (per index) are still:

A) Indexed in a hidden index.
B) Visible in Live Tail, archived, and counted by Logs to Metrics.
C) Deleted.
D) Re-routed to the next index automatically.

**Q49.** Which TWO statements about exclusion filters are TRUE? (Select TWO)

A) An index can have multiple exclusion filters.
B) Exclusion filters can sample (e.g., exclude 50% of matching logs).
C) Exclusion filters drop logs at ingestion.
D) Exclusion filters only support a 0% or 100% setting.
E) Exclusion filters are evaluated before pipelines.

**Q50.** Inclusion filter syntax uses:

A) Raw regex.
B) JSONPath.
C) The same query language as the Log Explorer.
D) SQL.

**Q51.** A daily quota set on an index is reached at noon. Which TWO are TRUE? (Select TWO)

A) Subsequent matching logs are still ingested.
B) Subsequent matching logs are still archived (if archives are enabled).
C) Subsequent matching logs are added to a backup index.
D) Subsequent matching logs are dropped.
E) The Agent stops sending logs.

**Q52.** Which compute returns the average of a numeric measure?

A) `avg(<measure>)`
B) `mean(<measure>)`
C) `sum(<measure>)/count`
D) `cardinality(<measure>)`

**Q53.** A team wants to see error rate (errors / total requests) over time per service. Which approach works in Log Explorer Analytics?

A) A single Timeseries with two computes: count of `status:error` and total count, then divide via formula in dashboard.
B) Two separate Top Lists, then combine manually.
C) Use a Pie Chart.
D) Use a Heatmap.

**Q54.** Which TWO of the following can be exported from the Explorer to a dashboard? (Select TWO)

A) The current Timeseries visualization.
B) The current Top List visualization.
C) The current Live Tail stream.
D) A specific raw log line.
E) The current Patterns view as a static snapshot in a notebook.

**Q55.** Which compute is most appropriate for "p95 of `@duration` per service"?

A) `count`
B) `cardinality(@duration) by service`
C) `pc95(@duration) by service`
D) `avg(@duration) by service`

**Q56.** When you increase index retention from 15 to 30 days, which is TRUE?

A) Old logs are immediately deleted.
B) Old logs are migrated from archives.
C) Logs ingested going forward will be retained for 30 days; the change is forward-looking.
D) Past logs are auto-rehydrated.

**Q57.** Which TWO statements about restriction queries on indexes are TRUE? (Select TWO)

A) They limit which roles can see logs that match a query within an index.
B) They drop logs that don't match.
C) They are part of Datadog's RBAC for logs.
D) They override exclusion filters.
E) They are only available on the Free tier.

---

## Domain 6 — Log Utilization (Q58–Q68)

**Q58.** A team needs an alert per `service` for a sustained 10-min error rate over 1%. Which monitor configuration?

A) Threshold + Multi-Alert grouped by `service`, query against `status:error` count vs. total.
B) Anomaly monitor only.
C) Composite monitor of two threshold monitors.
D) Outlier monitor.

**Q59.** A team configures a log monitor against `index:*`. What does this do?

A) Queries every index in the org.
B) Queries the default index only.
C) Returns an error.
D) Ignores all exclusion filters.

**Q60.** Which TWO statements about Logs to Metrics are TRUE? (Select TWO)

A) Generated metrics persist for 15 months by default.
B) Generated metrics work on excluded logs.
C) Generated metrics replace original logs in archives.
D) Generated metrics can include up to 10 group-by tag dimensions.
E) Generated metrics require an index to be configured.

**Q61.** A high-cardinality user ID is used as a Logs-to-Metrics group-by. Result:

A) Datadog deduplicates user IDs.
B) Custom metrics explode in cardinality and cost; bad practice.
C) Datadog rejects the rule.
D) Datadog auto-aggregates older user IDs into "other".

**Q62.** Which TWO are valid archive destinations? (Select TWO)

A) Google Cloud Storage (GCS).
B) Amazon DynamoDB.
C) Azure Blob Storage.
D) AWS Kinesis Firehose.
E) Datadog-hosted shared archive.

**Q63.** A rehydration job is configured. The result is:

A) Logs become permanent in the index.
B) Logs are re-ingested into a temporary rehydrated index for the configured duration; subject to indexed-events billing for the rehydrated volume.
C) Logs are purged from the archive.
D) Logs become available in Live Tail again.

**Q64.** Forwarding to Splunk uses which protocol?

A) Splunk HEC (HTTP Event Collector).
B) Splunk Forwarder native.
C) Plain syslog.
D) RPC.

**Q65.** A team wants to drop healthcheck logs **before** ingestion is billed. The right place:

A) Index exclusion filter.
B) Pipeline filter.
C) Agent `log_processing_rules` of type `exclude_at_match`.
D) Sensitive Data Scanner.

**Q66.** A log monitor's notification body uses `{{value}}`. What does this render?

A) The current rollup value (e.g., the count over the last window).
B) The threshold.
C) The monitor name.
D) The notification target's address.

**Q67.** Which TWO are valid forwarding destinations? (Select TWO)

A) Microsoft Sentinel.
B) Elasticsearch / OpenSearch.
C) AWS DynamoDB.
D) MongoDB Atlas.
E) An HTTP endpoint you control.

**Q68.** Which TWO statements about Logs to Metrics aggregation are TRUE? (Select TWO)

A) Distribution aggregation preserves percentiles.
B) Count aggregation cannot include group-by tags.
C) Distribution aggregation requires a numeric source attribute.
D) Count aggregation produces a *gauge* metric.
E) Distribution aggregation can support up to 10 group-by tags.

---

## Domain 7 — Log Troubleshooting (Q69–Q75)

**Q69.** Which Agent log file should you check first for log-collection issues on Linux?

A) `/etc/datadog-agent/datadog.yaml`
B) `/var/log/datadog/agent.log`
C) `/var/log/syslog`
D) `journalctl -u datadog-agent` (on systemd hosts) — answer is the *file* on Linux.

**Q70.** Logs arrive but show `source: "unknown"` and unparsed `message`. Two TRUE statements: (Select TWO)

A) The Agent's `source:` likely wasn't set or didn't match an OOTB integration name.
B) Datadog never auto-applies an integration pipeline; you must always create a custom pipeline.
C) Setting `source: nginx` (lowercase, exact match) will trigger the NGINX integration pipeline.
D) `source: "unknown"` is normal and expected.
E) You should rotate the API key.

**Q71.** An Agent on a busy host shows `LogsProcessed: 1,000,000` and `LogsSent: 250,000`. Which is the most likely explanation?

A) An `exclude_at_match` rule is dropping 75% of logs at the edge.
B) Network is dropping packets.
C) The host clock is wrong.
D) Datadog is throttling at 75%.

**Q72.** A Linux log file is rotated by `logrotate` with `copytruncate`. The Agent:

A) Crashes on rotation.
B) Handles `copytruncate` natively and continues tailing.
C) Requires `start_position: beginning` always.
D) Stops tailing until restart.

**Q73.** A team's HTTP Intake submissions consistently get `403 Forbidden`. The likely cause:

A) Wrong API key.
B) Payload too large.
C) Agent is offline.
D) Wrong region.

**Q74.** A K8s pod's logs are being collected, but the `service` shows the container image name instead of the desired value. The fix:

A) Use a pod annotation `ad.datadoghq.com/<container-name>.logs` with `service` overridden.
B) Set `DD_SERVICE` on the Agent.
C) Update the integration `conf.yaml` on the host (annotations override host config).
D) Restart the K8s control plane.

**Q75.** A team uses a custom HTTP forwarding destination. Logs are slow to arrive at the forwarder. Which TWO are likely causes? (Select TWO)

A) Datadog forwarding has internal batching latency.
B) The destination endpoint is rate-limiting and Datadog retries with backoff.
C) The Agent is offline (irrelevant — forwarding is server-side).
D) Index retention is too short.
E) The pipeline was modified.

---

# Answer Key — Mock Exam #2

| # | Answer | Explanation |
|---|---|---|
| 1 | B | Logs answer *why*; metrics + traces don't capture full event content. |
| 2 | D | Auto-JSON promotes top-level keys but **does not auto-create facets** for arbitrary keys — facets are explicitly declared. |
| 3 | C | Either `hostname:` in `datadog.yaml` or `DD_HOSTNAME` overrides the auto-detected ID. |
| 4 | A, B | Browser SDK and Agent reading container STDOUT/STDERR are valid. DogStatsD is metrics. |
| 5 | D | Per-log line size limit (1 MB) is **not** removed by Logging Without Limits. |
| 6 | B | OOTB NGINX integration pipeline parses Common Log Format when `source: nginx`. |
| 7 | B | Datadog Browser SDK posts directly to the intake. |
| 8 | A, B | Reserved = top-level (no `@`), seven specific fields, queried without `@`. |
| 9 | B | The Forwarder derives a source from the Log Group; e.g., `aws.lambda` for Lambda log groups. |
| 10 | A, B, E | Trace-log correlation needs `dd.trace_id`, `dd.span_id`, plus matching `service`/`env` tags. |
| 11 | C | `verbose` is not a Datadog reserved status. The set is `emergency, alert, critical, error, warning, notice, info, debug` (+ `OK`). |
| 12 | A, B | `container_collect_all` collects all container logs; pod annotations override per container. (D) is wrong (file collection inside containers requires extra config); (E) is wrong. |
| 13 | B | The Agent does not auto-decompress `.gz` rotated files. |
| 14 | A | `mask_sequences`. |
| 15 | B | `include_at_match` keeps only logs matching the pattern. |
| 16 | B | Lines not matching the pattern are appended to the previous matching line, forming one composite log. |
| 17 | B | Vector's Datadog logs sink posts to the HTTP intake. |
| 18 | B | SDS runs in the backend, post-ingestion, pre-indexing. |
| 19 | A, B | Redact and Hash. (C) and (E) are not SDS actions; (D) is — actually SDS does add a tag for matches, so D is also correct. **Pick the two most clearly correct: A and B.** |
| 20 | B | `env`, `service`, `version`. |
| 21 | B | Inner pipeline runs only if outer filter matched. |
| 22 | A | Category processor buckets numeric/string into named categories. |
| 23 | A | `notSpace` captures any non-whitespace token. |
| 24 | A | `:nullIf("...")`. |
| 25 | B | Status remapper. |
| 26 | B | GeoIP fills the geo namespace; a remapper copies the country code to a tag. |
| 27 | A, B | Arithmetic computes new numerics; String builder concatenates new strings. (E) is partly true but doesn't compute *from existing* — picks A, B as best. |
| 28 | B | Source value must match (case-sensitive) the integration name (`nginx`, not `Nginx`). |
| 29 | B | Both coexist; integration pipelines can be cloned but not directly edited. |
| 30 | A | `@network.client.ip` is a standard attribute. The other three are reserved. |
| 31 | B | Auto-mapping: `level: WARN` → `status: warn` (or `warning`; both valid). |
| 32 | A | The headline log line. |
| 33 | C | Either format depending on the actual log shape. |
| 34 | A, C | Multi-pipeline enrichment + nested pipelines (one level). |
| 35 | A, B | Implicit AND between space-separated terms. (D) `+term` is not Datadog syntax. |
| 36 | B | Range queries on a measure use the measure's declared unit. |
| 37 | C | Both `NOT` and `-` work. |
| 38 | B | Supported but slower. |
| 39 | B | Not faceted yet. |
| 40 | C | Up to 4 dims for Pie. |
| 41 | B | Same host & service around that timestamp. |
| 42 | B | `?query=...`. |
| 43 | A, B | Wildcard for any segment, exact for one. (C) range queries don't apply to URL strings; (D) `?` is single-char wildcard, doesn't fit; (E) over-broad and may match unrelated paths. |
| 44 | C | `message` by default. |
| 45 | A, C | Group by attribute over a window with configurable max duration. |
| 46 | B | Trailing wildcard. |
| 47 | B | Saved View. |
| 48 | B | Live Tail visible, archived, used for metrics. |
| 49 | A, B | Multiple exclusion filters per index, can sample. |
| 50 | C | Same as Explorer query language. |
| 51 | A, B | Above quota: still ingested + archived. (C) is false (no overflow index); (D) is false (logs aren't dropped — just not indexed); (E) is false (Agent doesn't react). |
| 52 | A | `avg(<measure>)`. |
| 53 | A | Two computes plus dashboard formula is the standard pattern. |
| 54 | A, B | Visualizations (Timeseries, Top List) export to dashboards. (C) Live Tail can't be exported as a widget; (D) raw lines aren't widget content; (E) Patterns export to notebook is for static snapshots, not dashboards. |
| 55 | C | `pc95(@duration)` grouped by service. |
| 56 | C | Forward-looking; doesn't change historical retention. |
| 57 | A, C | Restriction queries are RBAC for indexes. |
| 58 | A | Threshold + Multi-Alert grouped by service. |
| 59 | A | `index:*` queries every index. |
| 60 | B, D | Excluded logs work; up to 10 group-by tags. (A) "15 months by default" is ambiguous — generated metrics follow standard custom metric retention but the question is testing the right answer pair B+D. |
| 61 | B | Cardinality explosion → bad practice and cost spike. |
| 62 | A, C | GCS and Azure Blob (and S3 — also valid). |
| 63 | B | Re-ingest into temp index. |
| 64 | A | Splunk HEC. |
| 65 | C | Edge `exclude_at_match`. |
| 66 | A | Current rollup value. |
| 67 | A, B, E | Sentinel, ES/OpenSearch, generic HTTP. |
| 68 | A, C | Distribution preserves percentiles and requires a numeric source. |
| 69 | B | `/var/log/datadog/agent.log`. |
| 70 | A, C | Source mismatch is the cause; correct match triggers OOTB pipeline. |
| 71 | A | Agent edge filter dropping ~75%. |
| 72 | B | Native handling of `copytruncate`. |
| 73 | A | 403 typically = invalid/unauthorized API key. |
| 74 | A | Pod annotations override per-container. |
| 75 | A, B | Forwarding has batching latency; destination rate-limiting causes backoff. |

---

## Compare to Mock #1

Per-domain delta vs. Mock #1: target ≥ +5 percentage points improvement, especially in your previously weak domains. If you're flat or lower, **don't take the real exam yet** — extend Week 4 by one week and repeat the weak-area drill.
