# Quiz 07 — Log Troubleshooting (Domain 7)

**Coverage:** Agent issues · ingestion issues
**Length:** 12 questions

---

### 1. The first command to run when investigating "no logs are arriving" on a host is:

A) `datadog-agent restart`
B) `datadog-agent status`
C) `systemctl reload datadog-agent`
D) `tail -f /var/log/syslog`

### 2. Which command produces a support bundle (logs, configs, status) for Datadog Support?

A) `datadog-agent diagnose`
B) `datadog-agent status --output json`
C) `datadog-agent flare <ticket-id>`
D) `datadog-agent dump`

### 3. Live Tail shows logs but the Log Explorer does not. The most likely cause is:

A) Live Tail is broken.
B) Index exclusion filters or wrong inclusion routing.
C) The Agent's API key changed.
D) Patterns clustering hid the logs.

### 4. The Datadog Agent's main log file on Linux is at:

A) `/etc/datadog-agent/agent.log`
B) `/var/log/datadog/agent.log`
C) `/usr/share/datadog/agent.log`
D) `/var/run/datadog/agent.log`

### 5. A `permission denied` error in the Agent's logs while tailing `/var/log/myapp/app.log` is best fixed with:

A) `chmod 777` on the log file.
B) Running the Agent as `root`.
C) `setfacl -m u:dd-agent:rx` on the file and its parent directory.
D) Disabling SELinux.

### 6. A team's logs land in the wrong Datadog organization (different region). The most likely misconfiguration is:

A) The Agent's `api_key` is wrong.
B) The Agent's `site:` value is wrong (e.g., `datadoghq.eu` vs `datadoghq.com`).
C) The integration name is misspelled.
D) The host's DNS resolver is failing.

### 7. A multi-line Java stack trace is being broken into many separate logs. The fix is to add an Agent rule of type:

A) `mask_sequences`
B) `multi_line` whose `pattern` matches the start of a new log line.
C) `exclude_at_match`
D) `include_at_match`

### 8. In Kubernetes, Agent pods run as a DaemonSet but no container logs appear. The most common cause is:

A) Missing volume mounts for `/var/log/pods` and `/var/log/containers`.
B) Missing trace_id injection.
C) Pod CPU limit too low.
D) Datadog backend rejected the API key (would also break Live Tail).

### 9. Logs arrive but appear with `source: "unknown"` and no parsing. The fix is:

A) Increase the Agent log level.
B) Set the correct `source:` in the integration `conf.yaml` matching an integration pipeline name (e.g., `nginx`, `apache`, `python`).
C) Increase the index daily quota.
D) Restart the host.

### 10. Which TWO of the following indicate the Agent's log-collection block is healthy in `datadog-agent status`? (Select TWO)

A) `Logs Agent` section is present and shows `LogsSent` increasing.
B) `Per-source Status: OK` for the configured paths.
C) `LogsSent: 0` while `LogsProcessed` keeps growing.
D) `Type: file, Path: ..., Status: error: file not found`.
E) The host appears in the Datadog Infrastructure list.

### 11. A new log file isn't being picked up although other files in the same directory are. Which TWO are likely causes? (Select TWO)

A) The `path:` glob doesn't match the new filename.
B) Datadog requires a restart whenever a new file is created (it doesn't).
C) The `dd-agent` user lacks read on the file.
D) The file exceeds 1 MB.
E) The Agent's API key needs rotating.

### 12. Logs sent via the HTTP Intake API are sometimes rejected with `413` errors. The likely cause:

A) Wrong `Content-Type`.
B) Payload exceeds 5 MB.
C) Missing `DD-API-KEY` header.
D) The Datadog site is down.

---

## Answer Key

1. **B** — `datadog-agent status` is the canonical diagnostic — it tells you whether logs are enabled, files are being tailed, and the send rate.

2. **C** — `datadog-agent flare <ticket>` builds and uploads a support bundle.

3. **B** — Logs reach Datadog (since Live Tail is real-time over ingestion) but no index is admitting them — usually exclusion filters or inclusion routing.

4. **B** — `/var/log/datadog/agent.log`. Beware of (A) which mixes config and log paths.

5. **C** — ACLs are the right tool. Avoid `chmod 777` (security risk) and don't run the Agent as `root` for log tailing.

6. **B** — Wrong `site` routes to a different region. The `api_key` would generally not match in the wrong region either, but the *configuration* root cause is `site`.

7. **B** — `multi_line` rule with a pattern matching the start of a new log line.

8. **A** — Without `/var/log/pods` (and `/var/log/containers`) mounted into the Agent DaemonSet, the Agent can't read pod log files.

9. **B** — Setting `source:` in the integration config to match a known integration triggers the OOTB pipeline. Without a matching `source`, no parsing.

10. **A, B** — `Logs Agent` block present with growing `LogsSent` and per-source Status: OK indicate health. (C) suggests something is dropping logs (investigate `log_processing_rules`); (D) is an error; (E) is unrelated to logs.

11. **A, C** — Glob mismatch and permissions are the two everyday causes. (B) is false (the Agent watches for new files matching the glob); (D) is wrong (1 MB is per single log line, not per file); (E) wouldn't cause selective failure.

12. **B** — HTTP intake max payload is 5 MB. Each log line max 1 MB.

---

**Score:** below 9/12 → re-read `knowledge-base/07-log-troubleshooting.md`.
