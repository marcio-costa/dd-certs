# Drill 07 — Log Troubleshooting (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A team reports "no logs from this Linux host". You SSH in and run `datadog-agent status`. The output shows the **Logs Agent** block missing entirely. Which TWO statements are TRUE? (Select TWO)

A) `logs_enabled: true` is most likely missing from `datadog.yaml`.
B) The Agent is too old.
C) Restart the Agent after enabling logs.
D) The host has no log files.
E) The status output always omits Logs Agent if no integrations are configured.

**Q2.** Output: `LogsProcessed: 1,000,000  LogsSent: 250,000`. Most likely:

A) An `exclude_at_match` rule is dropping 75% at the edge.
B) Network drops 75%.
C) Datadog throttles the org.
D) Wrong API key.

**Q3.** A team's logs land in the wrong region's Datadog org. Likely cause:

A) Wrong `site:` (e.g., `datadoghq.com` vs `datadoghq.eu`).
B) Wrong API key.
C) Misspelled service.
D) Wrong tag.

**Q4.** A K8s Agent DaemonSet has no `/var/log/pods` mount. Result:

A) Pod logs aren't readable; container log collection silently fails.
B) The DaemonSet pods crash-loop.
C) Logs go to a fallback path.
D) Datadog auto-mounts the path.

**Q5.** A team's Java stack traces are split into many separate logs. The fix:

A) Add a `multi_line` rule whose pattern matches the start of a new log line (e.g., `^\d{4}-\d{2}-\d{2}`).
B) Increase Agent memory.
C) Disable Grok parsing.
D) Use `mask_sequences` for newlines.

**Q6.** Logs arrive in Live Tail but never appear in the Log Explorer. Most likely:

A) Index exclusion filters or wrong inclusion routing.
B) Live Tail is broken.
C) Patterns hid them.
D) The Agent's API key changed.

**Q7.** A team's logs have `source: "unknown"` and unparsed `message`. Two TRUE statements: (Select TWO)

A) `source` was probably not set on the integration config.
B) Setting `source: nginx` (lowercase, exact) triggers the OOTB pipeline.
C) The Agent must be reinstalled.
D) The log is unparseable.
E) Custom pipelines are required for every source.

**Q8.** HTTP intake calls return `403 Forbidden`. Most likely:

A) Wrong API key.
B) Payload too large.
C) Site is down.
D) Wrong content type.

**Q9.** HTTP intake calls return `413 Payload Too Large`. Most likely:

A) Payload exceeded 5 MB.
B) Wrong API key.
C) Wrong site.
D) Missing headers.

**Q10.** A team's `mask_sequences` regex isn't masking. Two TRUE root causes: (Select TWO)

A) YAML escaping of backslashes is wrong.
B) The regex requires PCRE features the Go regex engine (RE2) doesn't support (lookahead/lookbehind).
C) `mask_sequences` runs server-side, not at the Agent.
D) The regex must be lowercase.
E) Datadog requires a placeholder >= 16 characters.

**Q11.** Logs are delayed by minutes after a network blip. Most likely:

A) The Agent buffered logs on disk during disconnect; flushing takes time.
B) Datadog ingestion is broken.
C) The host clock drifted.
D) Pipeline backlog at Datadog.

**Q12.** A team's Agent log shows `permission denied` reading `/var/log/myapp/app.log`. Two TRUE fixes: (Select TWO)

A) Add `dd-agent` to the file's group.
B) Use `setfacl -m u:dd-agent:rx` on the file and parent dir.
C) `chmod 777` (NEVER — security).
D) Run the Agent as root.
E) Disable SELinux.

**Q13.** Logs are tagged `host: i-0123abcd` but the team wants the friendly hostname. The fix:

A) Set `hostname:` in `datadog.yaml` or `DD_HOSTNAME` env var.
B) Add a Status remapper.
C) Edit the EC2 metadata service.
D) Use `mask_sequences` for the host.

**Q14.** A team's Datadog Forwarder Lambda fails to ship CloudWatch Logs. Two TRUE causes: (Select TWO)

A) IAM role lacks `logs:GetLogEvents` and `logs:DescribeLogGroups`.
B) Lambda timeout too short for the volume.
C) The Datadog Agent is offline (irrelevant — Forwarder is serverless).
D) The Forwarder needs a VPC endpoint to reach Datadog (in private VPCs).
E) CloudWatch Logs requires JSON-only.

**Q15.** A team's logs show in the Datadog UI with the wrong timestamps (always 5 hours behind). Most likely:

A) Date remapper is using a format that doesn't include timezone, and the host is in UTC+5.
B) Datadog ingest is delayed.
C) The Explorer renders in the user's local time.
D) The log monitor cache is stale.

---

## Answer Key

1. **A, C** — Master switch missing + restart needed.
2. **A** — Edge filter dropping.
3. **A** — Wrong site.
4. **A** — Silent failure to read pod log files.
5. **A** — Multi-line rule.
6. **A** — Indexing routing/exclusion.
7. **A, B** — Source mismatch is the cause; correct match triggers OOTB pipeline.
8. **A** — 403 = unauthorized.
9. **A** — 413 = too large (>5 MB).
10. **A, B** — YAML escaping + RE2 limitations.
11. **A** — Agent disk-buffer flush after reconnect.
12. **A, B** — Group membership + ACLs without weakening permissions.
13. **A** — `hostname:` or `DD_HOSTNAME`.
14. **A, B** — IAM permissions + Lambda timeout.
15. **A** — Date remapper missing timezone, treats local time as UTC.
