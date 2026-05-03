# Domain 7 — Log Troubleshooting

**Exam subtopics:** Agent Issues · Ingestion Issues

---

## 7.1 The diagnostic flow

When logs are missing or wrong, walk down this funnel and stop at the first place where you find the problem.

```
1. Is the Agent running and healthy on the host?
2. Is log collection enabled (logs_enabled: true)?
3. Is the log file readable by the dd-agent user?
4. Does the integration config (conf.yaml) match the file path?
5. Are log_processing_rules silently dropping or scrubbing the log?
6. Are logs reaching Datadog (Live Tail)?
7. Are pipelines parsing as expected?
8. Are exclusion filters dropping the log from the index?
9. Are facets / standard attributes set up so search works?
```

The next sections cover each step.

---

## 7.2 Agent health — `agent status`

Run on the host:

```bash
sudo datadog-agent status
```

Look at the **Logs Agent** section:

```
==========
Logs Agent
==========
    Sending compressed logs in HTTPS to agent-http-intake.logs.datadoghq.com on port 443
    BytesSent: 12.3MB
    EncodedBytesSent: 4.5MB
    LogsProcessed: 1234
    LogsSent: 1230

  custom-app
  ----------
    - Type: file
      Path: /var/log/myapp/app.log
      Status: OK  (2026-05-03 14:21:55 UTC)
      Inputs:
         /var/log/myapp/app.log
```

Things to check:

- **`Status: OK`** — file is being tailed.
- **`LogsProcessed`** vs. **`LogsSent`** — if Sent ≪ Processed, an `exclude_at_match` rule may be dropping more than expected.
- **Errors** — if you see `permission denied`, the Agent user can't read the file.

### `agent flare` — bundled logs for support

```bash
sudo datadog-agent flare <ticket-id>
```

Bundles the Agent logs, config files, status, and runtime metrics into a `.zip` and uploads to Datadog Support. The exam often asks: *"What command produces a support bundle?"* → `agent flare`.

---

## 7.3 Common Agent-side problems

### Problem: "I enabled logs but nothing arrives"

Checklist:

1. `logs_enabled: true` in `datadog.yaml`?
2. Agent restarted after the change?
3. `conf.d/<integration>.d/conf.yaml` has a `logs:` block with valid `path`?
4. The path **actually contains files** matching the glob?
5. The `dd-agent` user can read the file? (`sudo -u dd-agent cat <path>`)
6. Outbound HTTPS (port 443) to `agent-http-intake.logs.<site>` is allowed by firewall?

### Problem: "Logs arrive but with the wrong timestamp"

- Agent ingest time is used **until** a Date remapper sets the official timestamp.
- Either the JSON `timestamp` field isn't being auto-detected (wrong format), or your Grok parser produced a `timestamp` attribute but no Date remapper consumes it.
- Add a Date remapper in the relevant pipeline, or rename your field to `timestamp` for auto-detection.

### Problem: "Logs arrive but `service` / `source` are wrong"

- These are set by the Agent at collection time (`service:`, `source:` in `conf.yaml`).
- For containers, image-based defaults apply unless you override via labels/annotations.
- A **Service remapper** in a custom pipeline can override after the fact, but fixing at the Agent is cleaner.

### Problem: "Multi-line stack traces are split into many logs"

- Add a `multi_line` `log_processing_rules` whose `pattern` matches the **start of a new log line** (timestamp / level prefix).
- Test the pattern in a regex tool — Agent uses Go's `regexp` (RE2 syntax), no lookahead/lookbehind.

### Problem: "Permission denied" reading log files (Linux)

- Common with files owned by `root:adm` or app-specific groups.
- Fix with ACLs:
  ```bash
  sudo setfacl -m u:dd-agent:rx /var/log/myapp/app.log
  sudo setfacl -m u:dd-agent:rx /var/log/myapp
  ```
  Or add `dd-agent` to the file's group.

### Problem: rotated logs aren't picked up

- Agent uses a **tail-from-end** model and supports `copytruncate` and `create` rotation modes.
- If logrotate uses a custom mode, ensure it isn't moving files out of the watched glob faster than the Agent's rotation tracker reacts.
- The setting `start_position: beginning` (in the Agent's logs config) reads from the start of new files — useful when files are short-lived.

---

## 7.4 Containerized environments

### Docker

```bash
docker logs <agent-container>
docker exec -it <agent-container> agent status
```

If `container_collect_all: true` is set but no container logs appear:

- Verify the Agent container has access to `/var/run/docker.sock` (or to `/var/lib/docker/containers/`).
- Verify the Agent has the Docker integration enabled (`DD_LOGS_CONFIG_DOCKER_CONTAINER_USE_FILE=true` for file-based collection).

### Kubernetes

```bash
kubectl exec -it <agent-pod> -n datadog -- agent status
kubectl exec -it <agent-pod> -n datadog -- agent flare
```

Common K8s issues:

- **DaemonSet missing volume mounts** for `/var/log/pods` and `/var/log/containers`. The Helm chart does this by default; custom manifests often forget.
- **Annotations on the wrong object**. Pod annotations on a Deployment template work; annotations on a Service do not.
- **Container name mismatch.** `ad.datadoghq.com/<container-name>.logs` must match the actual `containers[*].name` in the pod spec.

---

## 7.5 Ingestion-side issues

### Live Tail says no logs

- Confirm the Agent is sending: `agent status` shows `LogsSent` increasing.
- Confirm correct **API key**. The Agent's `api_key` and the Datadog org must match.
- Confirm correct **site**. `site: datadoghq.com` for US1, `datadoghq.eu` for EU, `us3.datadoghq.com`, etc. Wrong site → logs go to a different region's intake → don't appear in your org.

### Live Tail shows logs, Explorer doesn't

- The log is **not in any index** (excluded or no inclusion match).
- Check *Logs → Configuration → Indexes* and verify the inclusion filters.
- Check exclusion filters — a 100% exclude on a query like `*` would silently swallow everything in that index.

### Logs appear with `unknown` source / no parsing

- The `source` set at collection didn't match any integration pipeline.
- Pre-built pipeline names are case-sensitive (`source:nginx` not `Nginx`).
- Either set the right `source` at the Agent or build a custom pipeline targeting the actual `source` value.

### Logs are heavily delayed

- Agent buffers and batches; under normal conditions logs appear in seconds, but if the Agent has been disconnected, it can take minutes to flush its on-disk buffer (`runpath`).
- Check Agent logs for `502 Bad Gateway` or DNS errors → network connectivity to intake.

### Logs are dropped due to size limits

- A single log line is capped at **1 MB**; longer lines are truncated.
- HTTP intake payload max **5 MB**.
- Multi-line rules can produce very large composite logs — be careful with massive Java stack traces.

---

## 7.6 Reading the Agent's own log files

The Agent log files themselves are diagnostic gold:

| File | What it tells you |
|---|---|
| `/var/log/datadog/agent.log` | Core Agent events, integration loading, config errors |
| `/var/log/datadog/process-agent.log` | Process collection |
| `/var/log/datadog/trace-agent.log` | APM trace ingest |
| `/var/log/datadog/system-probe.log` | NPM / system probe |

Increase verbosity by setting `log_level: debug` in `datadog.yaml`. Don't leave it at debug in production.

---

## 7.7 The Log Hub (UI-based diagnostics)

In the Datadog UI, the **Logs Hub** (or *Logs → Configuration → Pipelines* → status panel) shows:

- Logs received per source over time.
- Logs *parsed* vs. *unparsed* (a high unparsed rate signals a missing pipeline).
- Logs *excluded* from indexes (sanity check on exclusion filters).

The UI also has a **diagnostics tool** under each integration tile that simulates the pipeline against sample logs.

---

## 7.8 Quick-fix matrix

| Symptom | First check | Likely cause |
|---|---|---|
| No logs anywhere | `agent status` | `logs_enabled: false`, Agent stopped, network blocked |
| Logs in Live Tail but not Explorer | Index inclusion / exclusion filters | 100% exclusion, wrong index routing |
| Wrong timestamp | Pipelines view → Date remapper | Date remapper missing or after a faulty grok |
| Wrong status | Status remapper | Mapping doesn't include your `level` value |
| `unknown` source | Agent `source:` field | Mismatched name vs. integration pipeline |
| Stack traces split | Agent multi_line rule | Pattern doesn't match log start |
| `permission denied` | Linux ACLs | `dd-agent` lacks read on file/dir |
| Logs delayed by minutes | Agent network | Buffer flush after reconnect |
| K8s logs missing | DaemonSet volumes / annotations | Missing `/var/log/pods` mount |
| Specific attribute not searchable | Facets | Custom JSON key not added as facet |

---

## 7.9 Exam-style takeaways for Domain 7

- `datadog-agent status` — your first stop. Look at the Logs Agent block.
- `datadog-agent flare <ticket>` — bundles everything for support.
- `logs_enabled: true` is the master switch; without it, nothing else works.
- "Live Tail sees them, Explorer doesn't" → exclusion filters or wrong index.
- Wrong site (`datadoghq.eu` vs. `datadoghq.com`) silently routes logs to the wrong region.
- File ACLs: `dd-agent` must read the file *and* its parent directory.
- Multi-line patterns must match the **start** of a new log line — typically timestamp or level.
- Container logs need `/var/log/pods` mounts and matching container-name annotations.
- 1 MB per log, 5 MB per HTTP payload — exceeded → truncation/rejection.
