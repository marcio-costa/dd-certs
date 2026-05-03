# Drill 01 — Logging Fundamentals (Hard / Scenario)

**15 questions · ~25 minutes · Harder than the reinforcement quiz.**

---

**Q1.** A platform team is choosing between three logging strategies for a new fleet of 200 microservices. Which approach minimizes long-term operational toil in Datadog?

A) Free-form text + a centralized Grok parser team to maintain regexes per service.
B) Structured JSON emitted by every service, with `env`, `service`, `version` always present.
C) Mixed text + JSON depending on language preferences.
D) XML to allow nested schemas.

**Q2.** A senior SRE argues that with traces and metrics, logs are no longer needed. Which TWO observability gaps would still motivate keeping logs? (Select TWO)

A) Capturing the exact request body that triggered an error.
B) Computing percentile latency.
C) Service map dependency visualization.
D) Capturing application-emitted business events that aren't traced (e.g., "user changed password").
E) Detecting CPU saturation.

**Q3.** A regulator demands that logs be retained for 7 years. Which Datadog feature is the cost-optimized answer?

A) Index retention set to 7 years.
B) Log Archives in customer-owned cloud storage (S3/Blob/GCS).
C) Daily forwarding to a self-managed Elasticsearch.
D) Generate metrics from logs and rely on metric retention.

**Q4.** Which of the following is **not** generally considered a benefit of *Logging Without Limits™*?

A) Ingesting all logs cheaply.
B) Choosing precisely which logs go into expensive search indexes.
C) Generating metrics from any ingested log, regardless of indexing.
D) Eliminating the per-log size limit.

**Q5.** A team uses two log severity values: `INFO` and `ERR`. Datadog auto-maps `ERR` to:

A) `error`
B) `info`
C) `warning`
D) `unknown`

**Q6.** A team logs in JSON but the timestamp field is named `event_time` (RFC 3339 string). Datadog shows ingest time, not event time. The correct fix:

A) Rename `event_time` to `timestamp` in the application, OR add a Date remapper sourcing `event_time`.
B) Increase index retention.
C) Configure a multi_line rule.
D) Switch to plaintext logging.

**Q7.** A serverless team posts logs from AWS Lambda. They prefer a Datadog-supplied solution that batches calls. The best option:

A) Datadog Lambda Extension.
B) Datadog Agent inside the Lambda.
C) Manual `curl` calls per invocation.
D) Browser SDK in Lambda.

**Q8.** A team uses Datadog Browser SDK in a single-page web app. Which TWO statements are TRUE? (Select TWO)

A) The SDK posts logs to the HTTP intake from the user's browser.
B) The SDK requires the Datadog Agent to be installed on the user's machine.
C) The SDK can correlate logs with RUM sessions.
D) The SDK requires logs to be stored locally first.
E) The SDK only supports Chrome.

**Q9.** A team operates in EU1 and US1 Datadog organizations. A host's Agent is configured with `site: datadoghq.com` but the team meant EU. Result:

A) Logs split between regions.
B) Logs land only in the US1 org; the EU1 org never sees them.
C) Datadog auto-detects and routes to the correct region.
D) The Agent refuses to start.

**Q10.** Which TWO trace IDs are required on a log for it to be linked to its APM span in the trace side panel? (Select TWO)

A) `dd.trace_id`
B) `dd.span_id`
C) `dd.host_id`
D) `dd.org_id`
E) `dd.service_id`

**Q11.** A team's Java app emits both file logs (file system) and STDOUT. They run on Kubernetes. The best collection strategy:

A) Disable STDOUT logging and only tail files.
B) Disable file logging and rely on STDOUT only — Datadog DaemonSet collects pod log files written by the kubelet.
C) Collect both for redundancy.
D) Forward STDOUT to a sidecar that writes to a file the Agent tails.

**Q12.** Which is the most accurate description of Datadog's seven reserved attributes?

A) `host`, `service`, `source`, `status`, `message`, `timestamp`, `tags` — top-level fields with no `@` prefix.
B) `env`, `service`, `version` — Unified Service Tags only.
C) `@http.method`, `@http.status_code`, `@network.client.ip`, `@error.kind`, `@usr.id`, `@duration`, `@db.statement`.
D) The first seven JSON fields in any log.

**Q13.** Which TWO statements about HTTP Logs Intake are TRUE? (Select TWO)

A) Endpoint format: `https://http-intake.logs.<site>/api/v2/logs`.
B) Endpoint format: `https://api.<site>/api/v1/logs`.
C) Max payload: 5 MB; max single log: 1 MB.
D) Requires the Agent to be running locally.
E) Requires `Authorization: Bearer <key>` header.

**Q14.** A team plans to migrate from a self-hosted ELK to Datadog. Which migration step has the **highest payoff** for getting clean Datadog dashboards quickly?

A) Replicating ELK index settings.
B) Standardizing on Datadog standard attributes (`@http.*`, `@network.*`, etc.) so OOTB dashboards populate automatically.
C) Replicating Kibana visualizations 1:1.
D) Setting up a separate Forwarder per service.

**Q15.** A team observes high log volume from a chatty debug source. Which TWO options reduce *ingestion cost* (not just indexing)? (Select TWO)

A) Add an Agent `exclude_at_match` rule.
B) Configure an index exclusion filter at 100% for that source.
C) Disable debug logging in the application.
D) Increase index retention.
E) Add a daily quota on the index.

---

## Answer Key

1. **B** — JSON + USTs minimize parsing work and align with OOTB dashboards.
2. **A, D** — Logs capture what metrics/traces don't: full event content + business events.
3. **B** — Archives in cheap object storage; rehydrate when needed.
4. **D** — Per-log size limit (1 MB) is unchanged.
5. **A** — `ERR` aliases to `error` per the Status remapper's recognized values.
6. **A** — Rename to the auto-detected name OR add a Date remapper.
7. **A** — Datadog Lambda Extension is the modern, preferred path.
8. **A, C** — Direct posts to intake; correlation with RUM sessions.
9. **B** — Wrong site routes to the wrong region.
10. **A, B** — `dd.trace_id` + `dd.span_id`.
11. **B** — In K8s, STDOUT-only is cleanest; the kubelet writes files the Agent reads.
12. **A** — Reserved = top-level no `@`, those seven fields.
13. **A, C** — Correct endpoint and size limits.
14. **B** — Aligning to standard attributes is the single biggest dashboard quick-win.
15. **A, C** — Edge filter and source-side suppression both reduce **ingestion**. Exclusion filters reduce indexing only.
