# Quiz 02 — Log Collection (Domain 2)

**Coverage:** enabling log collection · log filtering & obfuscation
**Length:** 12 questions

---

### 1. What is the **single** setting that enables log collection in the Datadog Agent?

A) `log_level: info` in `datadog.yaml`.
B) `logs_enabled: true` in `datadog.yaml`.
C) `logs.enabled: true` in `agent.yaml`.
D) Setting `DD_LOGS_INJECTION=true`.

### 2. A platform engineer adds a `logs:` block in `conf.d/myapp.d/conf.yaml` and restarts the Agent. No logs appear in Datadog. Which is the **most likely** missing step?

A) The system clock is not in sync.
B) `logs_enabled: true` is missing from `datadog.yaml`.
C) The integration is not in the Datadog Marketplace.
D) The Agent doesn't support custom file types.

### 3. Which Agent log-processing rule **drops** matching log lines before they leave the host?

A) `mask_sequences`
B) `multi_line`
C) `exclude_at_match`
D) `include_at_match`

### 4. A `multi_line` rule's `pattern` should match…

A) The end of every log line.
B) The start of a new log line (timestamp, level, etc.).
C) Every line of a stack trace.
D) Only blank lines.

### 5. Which two of the following are valid `type:` values in an Agent `logs:` configuration entry? (Select TWO)

A) `file`
B) `tcp`
C) `journal`
D) `http`
E) `windows_event`

### 6. In Kubernetes, a team uses pod annotations to set `source: java` on a specific container's logs. Which annotation key is correct?

A) `datadog.com/log.source`
B) `ad.datadoghq.com/<container-name>.logs`
C) `dd-agent/log/source`
D) `kubernetes.io/datadog-logs`

### 7. A regulator requires that credit-card numbers in logs **never reach Datadog**. Which is the most appropriate scrubbing approach?

A) Sensitive Data Scanner with redaction.
B) Index exclusion filter on logs containing 16-digit numbers.
C) Agent `log_processing_rules` of type `mask_sequences` with a credit-card regex.
D) Pipeline Grok parser that drops matched fields.

### 8. Which log path is **not** supported by the Agent file tailer?

A) An absolute path: `/var/log/myapp/app.log`.
B) A simple glob: `/var/log/myapp/*.log`.
C) A recursive double-star glob: `/var/log/**/*.log`.
D) A list of multiple `logs:` entries each with their own path.

### 9. In Docker with `container_collect_all: true`, the Agent collects logs from…

A) Files inside the container's filesystem.
B) The container's STDOUT/STDERR streams (via the runtime).
C) Each container's `/var/log/messages`.
D) The host journald only.

### 10. Which TWO settings are equivalent ways to enable container log collection? (Select TWO)

A) `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true` env var on the Agent container.
B) `container_collect_all: true` under `logs_config:` in `datadog.yaml`.
C) Adding `--collect-logs` to the `docker run` command of every workload.
D) Setting `service: container_logs` in every integration `conf.yaml`.

### 11. The HTTP Logs Intake API endpoint is:

A) `https://api.datadoghq.com/api/v2/logs`
B) `https://logs.datadoghq.com/v1/intake`
C) `https://http-intake.logs.<site>/api/v2/logs`
D) `https://intake.datadoghq.com/v1/logs`

### 12. A team forgot to set `logs_enabled: true` but did add `tags:` and `service:` in their integration config. What happens?

A) The tags are applied to metrics from that integration; logs do not flow.
B) The Agent automatically enables logs because tags are present.
C) The Agent emits a warning and crashes.
D) Logs flow but without the configured tags.

---

## Answer Key

1. **B** — `logs_enabled: true` in `datadog.yaml` is the master switch. `DD_LOGS_INJECTION` (D) is for *trace–log injection* in the tracer, not Agent collection.

2. **B** — The single most common cause: forgetting the master switch.

3. **C** — `exclude_at_match` drops the log when its `pattern` matches. `mask_sequences` (A) replaces a substring; `multi_line` (B) aggregates; `include_at_match` (D) keeps matches and drops non-matches.

4. **B** — `multi_line` says: "lines that don't match the pattern are appended to the previous line that did". So the pattern must identify the *start* of a new log entry (typical timestamp prefix).

5. **A, B, E** — `file`, `tcp`, `udp`, `journald`, `windows_event`, `docker` are valid. `journal` (C) is misspelled; `http` (D) isn't an Agent listener type — direct HTTP submission is via the intake API, not the Agent.

6. **B** — `ad.datadoghq.com/<container-name>.logs` is the autodiscovery annotation key; the value is JSON config.

7. **C** — `mask_sequences` runs **at the Agent edge**, scrubbing the value before it leaves the host. Sensitive Data Scanner (A) is server-side — the data has already reached Datadog when it runs.

8. **C** — Recursive `**` globs are not supported. Use multiple `logs:` entries instead.

9. **B** — `container_collect_all: true` reads STDOUT/STDERR via the container runtime; it does not read files inside the container.

10. **A, B** — The env var and the YAML setting are equivalent, used interchangeably depending on deployment.

11. **C** — Site-specific path is `https://http-intake.logs.<site>/api/v2/logs`. This is **not** `api.<site>` (common distractor).

12. **A** — Without `logs_enabled: true`, the Agent ignores all `logs:` configuration. Other (metrics) data may still flow with the tags.

---

**Score yourself:** Below 9/12 → re-read `knowledge-base/02-log-collection.md` and check `progress-tracking/weak-areas.md`.
