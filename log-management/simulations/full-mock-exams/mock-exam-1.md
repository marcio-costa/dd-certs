# Full Mock Exam #1 — Datadog Log Management Fundamentals

**Length:** 75 questions
**Time:** 2 hours
**Format:** Multiple choice (single answer unless "Select TWO/THREE" is stated)
**Scoring:** 1 point per fully-correct answer (multi-select counts only if all correct picks are selected and no extras)

> Take this under exam conditions. No notes, no Datadog UI. Don't grade until at least 12 hours after finishing.

---

## Domain 1 — Logging Fundamentals (Q1–Q11)

**Q1.** Which of the following best describes the role of logs vs. metrics vs. traces?

A) Metrics tell you *why*; logs tell you *what*; traces tell you *where*.
B) Logs tell you *why*; metrics tell you *what*; traces tell you *where*.
C) All three answer the same questions; the difference is only retention.
D) Traces are sampled; logs and metrics never are.

**Q2.** Which TWO statements about *Logging Without Limits™* are correct? (Select TWO)

A) It decouples ingestion from indexing.
B) It enables retroactive indexing of any archived log without rehydration.
C) It allows generating metrics from logs that are not indexed.
D) It removes the per-log size limit.
E) It pre-creates an exclusion filter on every index.

**Q3.** A new microservice will emit ~500 fields per log. The team wants minimum pipeline overhead. The best emission format is:

A) Plaintext with delimiters.
B) JSON.
C) XML.
D) `key=value` (logfmt).

**Q4.** Which of the following are *reserved* (top-level, no `@` prefix) attributes? (Select THREE)

A) `service`
B) `@http.method`
C) `host`
D) `status`
E) `@usr.id`
F) `@duration`

**Q5.** A log line carries `dd.trace_id`, `dd.span_id`, `service`, and `env` matching the APM service. The user navigates to the trace in the APM UI; the log appears in the **Logs** tab of the trace side panel because:

A) Datadog auto-injects matching logs into every trace based on host alone.
B) The log was correlated via the IDs and matching service/env tags.
C) Live Tail merges the log into the trace.
D) Trace–log correlation requires opening a notebook.

**Q6.** Which Datadog reserved status value is the most severe?

A) `error`
B) `critical`
C) `alert`
D) `emergency`

**Q7.** A team must ingest logs from a network switch that only emits RFC 3164 syslog over UDP. The recommended path is:

A) Install the Agent on the switch.
B) Have the Agent on a host listen on a UDP port and forward.
C) POST every line via `curl` to the HTTP intake.
D) Convert syslog to OpenTelemetry first.

**Q8.** A team logs every event to STDOUT in JSON. They run on Kubernetes. Which TWO statements are correct? (Select TWO)

A) The Datadog Agent (DaemonSet) reads the pod log files Kubernetes writes to `/var/log/pods/`.
B) The application must write to a file inside the container; STDOUT is not collected.
C) Pod annotations can override `service` and `source` per container.
D) The Agent reads STDOUT directly from the container's process.
E) Logs from a stopped pod are deleted immediately.

**Q9.** Which of the following is **not** a typical log emission method to Datadog?

A) Agent tails a file.
B) Agent listens on TCP/UDP.
C) Direct POST to HTTP Logs Intake.
D) Datadog scrapes log files via SSH.

**Q10.** A team wants to retain logs cheaply for 5 years for an annual audit. The recommended approach is:

A) Set index retention to 5 years.
B) Use Log Archives (S3) and Rehydrate at audit time.
C) Forward all logs to a self-hosted Elasticsearch.
D) Increase the daily index quota by 100×.

**Q11.** Datadog's auto-JSON parser auto-maps which JSON fields to reserved attributes? (Select THREE)

A) `host`
B) `severity` → `status`
C) `version`
D) `timestamp`
E) `level` → `status`

---

## Domain 2 — Log Collection (Q12–Q20)

**Q12.** What is the **single** Agent setting that enables log collection?

A) `log_level: info` in `datadog.yaml`.
B) `logs_enabled: true` in `datadog.yaml`.
C) Setting `DD_LOGS_INJECTION=true`.
D) Adding `logs:` blocks under `conf.d/`.

**Q13.** A team has `logs_enabled: true` and a `conf.d/myapp.d/conf.yaml` with a valid `path:`. Logs do not arrive. The Agent log shows `permission denied`. The fix is:

A) `chmod 777` the log file.
B) Run the Agent as root.
C) Apply ACLs (`setfacl`) to grant `dd-agent` read access to the file and its parent directory.
D) Disable AppArmor / SELinux.

**Q14.** Which `log_processing_rules` type drops a log at the Agent edge before sending it to Datadog?

A) `mask_sequences`
B) `multi_line`
C) `exclude_at_match`
D) `include_at_match`

**Q15.** A `multi_line` rule's `pattern` should match:

A) The end of a log line.
B) The start of a new log entry (timestamp/level prefix).
C) Any blank line.
D) The application's process ID.

**Q16.** Which TWO are valid `type:` values inside a `logs:` entry? (Select TWO)

A) `file`
B) `tcp`
C) `journal`
D) `http`
E) `windows_event`

**Q17.** Which is the correct annotation to configure log collection for a specific Kubernetes container called `web`?

A) `datadog.com/log.source.web`
B) `ad.datadoghq.com/web.logs`
C) `dd-agent/log/source/web`
D) `kubernetes.io/datadog/log-web`

**Q18.** A regulator requires that PII never reaches Datadog. The most appropriate scrubbing approach:

A) Sensitive Data Scanner with a redaction rule.
B) Index exclusion filter.
C) Agent `log_processing_rules` of type `mask_sequences`.
D) Pipeline Grok parser.

**Q19.** The HTTP Logs Intake endpoint is:

A) `https://api.datadoghq.com/api/v2/logs`
B) `https://http-intake.logs.<site>/api/v2/logs`
C) `https://logs.datadoghq.com/v1/intake`
D) `https://intake.datadoghq.com/v2/logs`

**Q20.** Inside an Agent integration `conf.yaml`, you can list…

A) Only one `logs:` entry per file.
B) Multiple `logs:` entries (a YAML list) covering different paths/sources.
C) Up to two `logs:` entries (Free tier limit).
D) Only `logs:` entries that share the same `service:`.

---

## Domain 3 — Log Parsing (Q21–Q34)

**Q21.** Pipelines in the Datadog backend run:

A) Bottom-to-top.
B) In random order.
C) Top-to-bottom; logs pass through every pipeline whose filter matches.
D) Top-to-bottom; logs stop at the first matching pipeline.

**Q22.** Within a pipeline, processors execute:

A) In parallel.
B) In declared order.
C) Alphabetically.
D) By processor weight.

**Q23.** A team's Grok parser extracts `timestamp` but logs still show ingest time. The fix is:

A) Increase Agent log level.
B) Add a Date remapper sourcing `timestamp`.
C) Change index retention.
D) Add a Status remapper.

**Q24.** Which is the **canonical processor order** within a custom pipeline?

A) Enrich → restructure → parse → normalize.
B) Parse → normalize (remappers) → enrich → restructure.
C) Restructure → parse → normalize → enrich.
D) Normalize → parse → restructure → enrich.

**Q25.** The Grok matcher `%{integer:user_id}` produces:

A) A string with leading zeros preserved.
B) A JSON number `user_id`.
C) An ISO 8601 timestamp.
D) Always nil.

**Q26.** Which Grok filter parses a substring as JSON into nested attributes?

A) `:json`
B) `:keyvalue`
C) `:array`
D) `:nullIf("-")`

**Q27.** "Match rules" vs. "Support rules" in a Grok parser:

A) Match rules run on logs; support rules are a Datadog Premium tier feature.
B) Match rules are reusable patterns; support rules are tried against the source.
C) Match rules are tried against the source; support rules are reusable named patterns referenced by match rules.
D) They are aliases of the same thing.

**Q28.** A standard attribute remapper is best used to:

A) Re-encrypt the log payload.
B) Align custom field names (e.g., `usr_id`) to Datadog standard attributes (`@usr.id`).
C) Drop logs not matching a pattern.
D) Convert seconds to milliseconds.

**Q29.** A team's logs include latency in seconds. The dashboard expects nanoseconds (`@duration`). The simplest processor:

A) String builder.
B) Arithmetic processor with multiplication by 1e9, OR a Grok filter like `:scale(1000000000)`.
C) Lookup processor.
D) Status remapper.

**Q30.** Which TWO are reserved attributes? (Select TWO)

A) `host`
B) `@http.url`
C) `service`
D) `@network.client.ip`
E) `@usr.email`

**Q31.** Datadog auto-JSON parsing **does not** auto-detect:

A) `level` → `status`
B) `severity` → `status`
C) `request_path` → `@http.url`
D) `timestamp` → official log timestamp

**Q32.** A team wants a tag `country` derived from `@network.client.ip`. The single best processor:

A) GeoIP parser.
B) URL parser.
C) String builder.
D) Lookup processor.

**Q33.** A pipeline filter uses the same query syntax as the Log Explorer.

A) True.
B) False — pipeline filters use raw regex.
C) False — pipeline filters use a JSON-only filter language.
D) False — pipelines do not support filtering.

**Q34.** The reserved attribute `source` is used by Datadog to:

A) Set the log's severity.
B) Trigger out-of-the-box integration pipelines (e.g., `source:nginx` triggers the NGINX pipeline).
C) Identify the host.
D) Generate metrics automatically.

---

## Domain 4 — Log Searching & Filtering (Q35–Q47)

**Q35.** A team has just enabled log collection on a brand-new service. They want to immediately verify logs are flowing — even though no inclusion filter routes them to an index yet. Which view do they use?

A) Saved View.
B) Log Explorer with `*` query and 1-day range.
C) Live Tail.
D) Notebook.

**Q36.** The approximate Live Tail window is:

A) 60 seconds.
B) 15 minutes.
C) 1 hour.
D) Index retention.

**Q37.** Which TWO statements about Live Tail are TRUE? (Select TWO)

A) It shows logs even if they are excluded from indexes.
B) It supports timeseries aggregations.
C) It uses the same query syntax as the Log Explorer.
D) It can be saved as a Saved View.
E) It is rate-limited to 10 logs/sec.

**Q38.** To filter logs where `@http.status_code` is 400 to 499 inclusive:

A) `@http.status_code:[400 TO 499]`
B) `@http.status_code:{400 TO 499}`
C) `@http.status_code:400-499`
D) `@http.status_code BETWEEN 400 AND 499`

**Q39.** Which TWO queries find logs from the `checkout` service that are **not** HTTP 200? (Select TWO)

A) `service:checkout NOT @http.status_code:200`
B) `service:checkout -@http.status_code:200`
C) `service:checkout AND status:!=200`
D) `service:checkout AND -200`
E) `service:checkout NOT 200`

**Q40.** The required prefix for **attribute** searches (vs. tags / reserved attributes):

A) `:`
B) `#`
C) `@`
D) `&`

**Q41.** The query `-@usr.id:*` matches logs where:

A) `@usr.id` exists with any value.
B) `@usr.id` does not exist.
C) `@usr.id` equals `null`.
D) `@usr.id` is uppercase.

**Q42.** A custom JSON key `request_path` is present on every log but cannot be filtered from the facet sidebar. The most likely cause is:

A) The field has too many distinct values.
B) The field has not been declared as a facet.
C) The Explorer is in Live Tail.
D) The log payload is over 1 MB.

**Q43.** **Patterns** in the Log Explorer is most useful for:

A) Detecting trace-log correlation breakage.
B) Generating Grok rules automatically.
C) Clustering logs that share a similar text shape and identifying noise to drop.
D) Showing logs grouped by host.

**Q44.** A Saved View persists which of the following? (Select ALL that apply)

A) The query string.
B) The selected time range.
C) The chosen visualization (List, Patterns, Analytics, …).
D) The selected facets in the sidebar.
E) The user's API key.

**Q45.** Top List visualization is limited to how many group-by dimensions?

A) 1
B) 2
C) 4
D) Unlimited

**Q46.** Transactions in the Log Explorer group logs by:

A) Their `dd.trace_id` only.
B) The same host and service.
C) A correlation attribute the user specifies (e.g., `request_id`) within a max-duration window.
D) Five-minute time buckets.

**Q47.** Which is **NOT** a valid Log Explorer visualization tab?

A) List
B) Timeseries
C) Top List
D) Heatmap
E) Pie Chart

---

## Domain 5 — Log Analysis (Q48–Q57)

**Q48.** An exclusion filter on an index drops a log from being…

A) Ingested.
B) Indexed (searchable in Explorer).
C) Stored in archives.
D) Counted by Logs to Metrics.

**Q49.** A team applies a 100% exclusion filter on `source:nginx @http.url:/health*`. Which TWO are TRUE? (Select TWO)

A) Healthcheck logs disappear from the Explorer for that index.
B) Healthcheck logs are still visible in Live Tail.
C) Datadog stops charging ingestion for healthchecks.
D) Healthcheck logs no longer feed Logs to Metrics.
E) Healthcheck logs are deleted from archives.

**Q50.** A log can match…

A) Multiple indexes simultaneously.
B) Only the first index whose inclusion filter matches.
C) An unlimited number of pipelines but at most one index.
D) No index at all only if explicitly tagged with `noindex:true`.

**Q51.** To compute a 95th-percentile of `@duration` in Analytics, the attribute must be a:

A) Tag.
B) Standard attribute.
C) Measure with a unit.
D) Pattern.

**Q52.** Which compute returns the number of **distinct values** of an attribute?

A) `count`
B) `cardinality(<attribute>)`
C) `unique(<attribute>)`
D) `distribution`

**Q53.** Index retention controls:

A) Bytes per day ingested.
B) How long indexed logs remain searchable in the Explorer.
C) How long archives are kept in S3.
D) Maximum facets per log.

**Q54.** The fastest way to investigate logs from the same host/service around the timestamp of a specific log is:

A) Manually copy the host and timestamp into a query.
B) Click *View In Context*.
C) Live Tail.
D) Export to CSV.

**Q55.** A team wants the cheapest way to keep five years of logs available for occasional historical investigation:

A) Set index retention to 5 years.
B) Increase daily quota indefinitely.
C) Use Log Archives (S3 / Blob / GCS), Rehydrate when needed.
D) Forward all logs to self-hosted Elasticsearch.

**Q56.** Which TWO visualizations support up to 4 group-by dimensions? (Select TWO)

A) Top List
B) Timeseries
C) Tree Map
D) List
E) Patterns

**Q57.** A daily quota of 1,000,000 events/day on an index is reached at 18:00. Subsequent matching logs:

A) Are dropped at ingestion.
B) Are still ingested + archived but not added to the index.
C) Trigger an Agent crash.
D) Are queued and indexed at midnight.

---

## Domain 6 — Log Utilization (Q58–Q68)

**Q58.** A standard log monitor query has the form `logs("<query>").index("<idx>").rollup("count").last("5m") > N`. `last("5m")` defines:

A) Recovery threshold.
B) Evaluation window.
C) Notification cooldown.
D) Min datapoints before evaluation.

**Q59.** A team wants one alert per service for "errors > 50/min". The right monitor option is:

A) Composite monitor.
B) Multi-Alert (group by `service`).
C) Outlier monitor.
D) Anomaly monitor.

**Q60.** Logs to Metrics generated metrics work on:

A) Only indexed logs.
B) Only archived logs.
C) All ingested logs (including those excluded from indexes).
D) Only HTTP-intake logs.

**Q61.** The maximum number of group-by tag dimensions for a generated metric is:

A) 4
B) 8
C) 10
D) Unlimited

**Q62.** Which TWO Logs-to-Metrics aggregations are supported? (Select TWO)

A) `count`
B) `distribution` over a numeric attribute
C) `unique`
D) `linear_regression`
E) `histogram`

**Q63.** Which TWO are valid Log Archive destinations? (Select TWO)

A) AWS S3
B) AWS RDS
C) Azure Blob Storage
D) Datadog-hosted archive (any region)
E) AWS DynamoDB

**Q64.** Rehydration:

A) Re-encrypts archives with a new KMS key.
B) Re-ingests archived logs into a temporary index for searching.
C) Re-runs pipelines on already-indexed logs.
D) Re-issues the Agent's API key.

**Q65.** Log Forwarding can push logs in real time to which destinations? (Select THREE)

A) Splunk HEC
B) Elasticsearch / OpenSearch
C) Generic HTTP webhook
D) AWS DynamoDB
E) Microsoft Sentinel
F) MongoDB Atlas

**Q66.** A team excludes 100% of `level:debug` logs from `main` index. They still need to count debug volumes per service over time. The right approach:

A) Disable the exclusion filter.
B) Send debug logs only via HTTP intake.
C) Create a Logs to Metrics rule (count, group by `service`).
D) Move debug logs to a separate index with full retention.

**Q67.** Which TWO statements about log monitor groups are correct? (Select TWO)

A) Multi-Alert creates one alert per unique tag value.
B) Group retention controls how long a monitor remembers a group before dropping it.
C) Group-by is only available for anomaly monitors.
D) A monitor can group by at most 1 tag.
E) Multi-Alert merges all groups into a single combined alert.

**Q68.** The biggest cost benefit of *Logging Without Limits™* is:

A) Disabling ingestion entirely.
B) Free indefinite index retention.
C) Cheap ingestion of all logs while choosing precisely which to index.
D) Avoiding S3 storage charges.

---

## Domain 7 — Log Troubleshooting (Q69–Q75)

**Q69.** The first command to run when investigating "no logs are arriving" on a host is:

A) `datadog-agent restart`
B) `datadog-agent status`
C) `systemctl reload datadog-agent`
D) `tail -f /var/log/datadog/agent.log`

**Q70.** Which command builds a support bundle for Datadog Support?

A) `datadog-agent diagnose`
B) `datadog-agent flare <ticket-id>`
C) `datadog-agent dump`
D) `datadog-agent status --output json`

**Q71.** Live Tail shows logs but the Log Explorer does not. The most likely cause:

A) Live Tail is broken.
B) Index exclusion filters or wrong inclusion routing.
C) The Agent's API key is wrong.
D) Patterns clustering hid the logs.

**Q72.** A team's logs land in the wrong region's Datadog org. The likely misconfiguration:

A) Wrong `api_key`.
B) Wrong `site:` value (e.g., `datadoghq.eu` vs. `datadoghq.com`).
C) Misspelled integration name.
D) DNS failure on the host.

**Q73.** A multi-line Java stack trace is being broken into many separate logs. The fix:

A) `mask_sequences`
B) `multi_line` rule whose pattern matches the start of a new log line
C) `exclude_at_match`
D) Increase log_level to debug

**Q74.** In Kubernetes, Agent pods (DaemonSet) are running but no container logs appear. The most common cause:

A) Missing `/var/log/pods` (and `/var/log/containers`) volume mounts.
B) Trace IDs aren't injected.
C) Pod CPU limit too low.
D) The cluster is on a Datadog-incompatible Kubernetes version.

**Q75.** Logs sent via the HTTP Intake API are rejected with `413 Payload Too Large`. The likely cause:

A) Wrong `Content-Type`.
B) Missing `DD-API-KEY` header.
C) Payload exceeds 5 MB.
D) The Datadog site is down.

---

# Answer Key — Mock Exam #1

**Don't peek until you've finished all 75.**

| # | Answer | Brief explanation |
|---|---|---|
| 1 | B | Logs = why · metrics = what · traces = where. |
| 2 | A, C | Decouples ingestion/indexing; metrics work on un-indexed logs. (B) is wrong (rehydration required). |
| 3 | B | Auto-parsed JSON minimizes pipeline work. |
| 4 | A, C, D | Reserved attrs are top-level (no `@`): `host`, `service`, `status` (also `source`, `message`, `timestamp`, `tags`). |
| 5 | B | IDs + matching service/env link the log to the trace. |
| 6 | D | Severity order: emergency, alert, critical, error, warning, notice, info, debug. |
| 7 | B | Agent listens on TCP/UDP socket for syslog. |
| 8 | A, C | DaemonSet reads `/var/log/pods/`; pod annotations override. STDOUT *is* collected (B), via the runtime files. |
| 9 | D | Datadog never SSHes into hosts. |
| 10 | B | Archives + Rehydrate is the cheap-storage answer. |
| 11 | A, B/E, D | Auto-JSON detects host, severity/level → status, timestamp. (Both B and E are correct mappings; either selected with A and D scores correct.) |
| 12 | B | `logs_enabled: true` is the master switch. |
| 13 | C | ACLs grant `dd-agent` read access without insecure 777. |
| 14 | C | `exclude_at_match` drops at the edge. |
| 15 | B | Pattern matches start of new log; non-matching lines join previous. |
| 16 | A, B / E | `file`, `tcp`, `udp`, `journald`, `windows_event`, `docker` are valid. (`journal` and `http` are not.) |
| 17 | B | `ad.datadoghq.com/<container-name>.logs` autodiscovery key. |
| 18 | C | Edge `mask_sequences` keeps PII off the wire. SDS (A) only scrubs after ingestion. |
| 19 | B | `https://http-intake.logs.<site>/api/v2/logs`. |
| 20 | B | YAML list — multiple `logs:` entries per file are supported. |
| 21 | C | Top-to-bottom; logs pass through every matching pipeline. |
| 22 | B | Declared order. |
| 23 | B | A Date remapper consuming the parsed `timestamp` is required. |
| 24 | B | Parse → normalize → enrich → restructure. |
| 25 | B | `%{integer:...}` produces a JSON number. |
| 26 | A | `:json` filter parses JSON substrings. |
| 27 | C | Match rules: tried against source. Support rules: reusable named patterns referenced by match rules. |
| 28 | B | Aligns custom names to standard attributes. |
| 29 | B | Arithmetic processor or grok `:scale(...)` filter. |
| 30 | A, C | `host` and `service` are reserved (no `@`). |
| 31 | C | Custom field name like `request_path` is NOT auto-mapped to `@http.url`. The other three are auto-mapped. |
| 32 | A | GeoIP parser is purpose-built for IP enrichment. |
| 33 | A | True — same query language. |
| 34 | B | `source:` triggers the matching OOTB integration pipeline. |
| 35 | C | Live Tail bypasses indexes. |
| 36 | B | ~15 minutes. |
| 37 | A, C | All ingested logs visible; same query syntax. |
| 38 | A | `[a TO b]` inclusive range. |
| 39 | A, B | `NOT` and `-` both negate. |
| 40 | C | `@` for attributes. |
| 41 | B | `-@usr.id:*` = does not exist. |
| 42 | B | Must be declared as a facet to appear in sidebar. |
| 43 | C | Clusters similar logs; useful for finding noise to exclude. |
| 44 | A, B, C, D | Saved View persists query, time range, viz, and facets. Not API key. |
| 45 | A | Top List supports 1 dimension. |
| 46 | C | Group by correlation attribute within a max window. |
| 47 | D | Heatmap is not a Log Explorer viz tab. (List, Timeseries, Top List, Table, Tree Map, Pie, Geomap, Patterns, Transactions are.) |
| 48 | B | Excluded from indexing only. |
| 49 | A, B | Removed from index, still visible in Live Tail. (C) wrong (still ingested and charged); (D) wrong (still feeds metrics); (E) wrong (still archived). |
| 50 | B | First matching index wins. |
| 51 | C | Percentiles require a measure. |
| 52 | B | `cardinality` returns distinct count. |
| 53 | B | Search retention. |
| 54 | B | View In Context. |
| 55 | C | Archives + Rehydrate. |
| 56 | B, C | Timeseries and Tree Map both up to 4 dims. (A) is 1; (D) and (E) don't use group-bys. |
| 57 | B | Above quota: ingested + archived, not indexed. |
| 58 | B | Evaluation window. |
| 59 | B | Multi-Alert grouped by service. |
| 60 | C | All ingested logs (incl. excluded). |
| 61 | C | Up to 10 group-by tags. |
| 62 | A, B | `count` and `distribution`. |
| 63 | A, C | S3, Azure Blob, GCS (D distractor since "any region" implies hosted; archives are customer-cloud, not Datadog-hosted). |
| 64 | B | Re-ingest archived logs into a temporary index. |
| 65 | A, B, C, E | Splunk HEC, ES/OpenSearch, HTTP webhook, Microsoft Sentinel are valid. (Pick any 3 of those four for full credit; D and F are wrong.) |
| 66 | C | Logs to Metrics works on excluded logs. |
| 67 | A, B | Multi-Alert = one per group; group retention controls memory. |
| 68 | C | Decouple ingestion from indexing. |
| 69 | B | `datadog-agent status` first. |
| 70 | B | `datadog-agent flare <ticket>`. |
| 71 | B | Exclusion filter / inclusion routing. |
| 72 | B | Wrong `site` routes to wrong region. |
| 73 | B | `multi_line` rule. |
| 74 | A | Volume mounts are required for the DaemonSet to read pod logs. |
| 75 | C | Payload exceeds 5 MB intake limit. |

---

## Scoring rubric

- **≥ 65/75 (87%)** — On track. Schedule the real exam.
- **55–64/75 (73–86%)** — Solid; identify your bottom 1–2 domains and drill them.
- **45–54/75 (60–72%)** — Weak. Re-read the bottom-2 knowledge-base files and repeat their domain quizzes; retake Mock #2 in 5–7 days.
- **< 45/75** — Re-do Week 1+2 of the study plan; you're moving too fast.

After grading, copy any concept you missed into `progress-tracking/weak-areas.md` and run the **adaptive weak-area drill** before continuing.
