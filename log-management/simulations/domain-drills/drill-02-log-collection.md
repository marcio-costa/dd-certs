# Drill 02 — Log Collection (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A new Linux host has the Agent installed; `datadog-agent status` shows green for metrics but no Logs Agent block at all. The most likely cause:

A) The Agent version is too old.
B) `logs_enabled: true` is not set in `datadog.yaml`.
C) The host is missing `tail` command.
D) The API key is wrong.

**Q2.** A team uses a host where logs are owned by `root:adm` (mode `640`). Which TWO actions allow the Agent to read them without weakening file permissions globally? (Select TWO)

A) `setfacl -m u:dd-agent:rx /var/log/myapp/app.log`
B) `chmod 777 /var/log/myapp/app.log`
C) Add `dd-agent` user to the `adm` group.
D) Run the Agent as root.
E) Disable SELinux.

**Q3.** A team uses Kubernetes with an old containerd version. Pod log paths under `/var/log/pods/` are read-only for the Agent. The DaemonSet can't read them. The most reliable fix:

A) Change the Agent container's UID/GID and add the appropriate volume mount with read access.
B) Disable Kubernetes logging.
C) Switch to Docker.
D) Use `mask_sequences` to bypass permissions.

**Q4.** A team adds three rules in the Agent in this order:
1. `mask_sequences` for SSNs
2. `exclude_at_match` for healthchecks
3. `multi_line` for stack traces

Will this work as expected?

A) Yes — order doesn't matter.
B) No — `exclude_at_match` should be first to drop noise before masking spends CPU.
C) No — `multi_line` must be first.
D) No — `mask_sequences` must come last.

**Q5.** A team writes the following Agent config:
```yaml
logs:
  - type: file
    path: /var/log/myapp/*.log
    service: web
    source: nginx
    log_processing_rules:
      - type: include_at_match
        name: errors_only
        pattern: 'ERROR|FATAL'
```
Which logs are sent to Datadog?

A) Every log.
B) Only logs whose raw line contains "ERROR" or "FATAL".
C) Only logs with `status:error`.
D) None.

**Q6.** Which TWO Agent configurations enable container log collection? (Select TWO)

A) `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true` env var on Agent container.
B) Setting `service: container_logs` in every workload's deployment.
C) `logs_config.container_collect_all: true` in `datadog.yaml`.
D) Enabling `DD_LOGS_INJECTION=true` in workloads.
E) Mounting the Docker socket on every workload pod.

**Q7.** A team uses a pod annotation to override log config. The container's name in the pod spec is `web-server`. The correct annotation key is:

A) `ad.datadoghq.com/web-server.logs`
B) `ad.datadoghq.com/web-server/logs`
C) `dd-agent.io/log-config/web-server`
D) `ad.datadoghq.com/logs.web-server`

**Q8.** A team wants to ingest VPC Flow Logs from AWS. Which approach is most common?

A) Run the Datadog Agent in each VPC.
B) Use the Datadog Forwarder Lambda subscribed to the S3 bucket holding the flow logs.
C) Stream Flow Logs over UDP to the Agent.
D) Manually export Flow Logs daily.

**Q9.** A team posts logs via HTTP intake and includes `ddtags: "env:prod,team:web"`. What does Datadog do with it?

A) Adds `env:prod` and `team:web` tags to each log in the request.
B) Treats it as the `message`.
C) Rejects the request.
D) Saves the string as a single tag.

**Q10.** A team's PII-scrub regex matches credit-card numbers. Which TWO statements about choosing **Agent edge masking vs. Sensitive Data Scanner** are correct? (Select TWO)

A) Agent-edge masking keeps the data off Datadog entirely (best for strict compliance).
B) SDS lets the data into Datadog and then redacts before indexing — the data was visible to Datadog backend systems briefly.
C) SDS is mandatory for PCI-DSS compliance.
D) Agent-edge masking has access to a larger built-in rule library.
E) Both can produce a tag indicating a match was found.

**Q11.** A team forgot to set `service` in the integration `conf.yaml`. The default `service` for log entries from that file is:

A) The host's hostname.
B) `unknown`.
C) The integration source name (e.g., `nginx`).
D) Empty (Datadog rejects the log).

**Q12.** Two `logs:` entries in the same file have overlapping path globs (one is more specific). Which entry processes a log?

A) The first one in file order.
B) The most specific glob.
C) Both, producing duplicate logs.
D) Only the second.

**Q13.** A team uses `journald` as a log source. Which TWO statements are TRUE? (Select TWO)

A) The Agent supports `type: journald`.
B) `include_units` filters which systemd units to collect.
C) Journald collection requires a mounted Docker socket.
D) Journald supports `start_position: beginning`.
E) Journald collection bypasses pipelines.

**Q14.** A team uses log_processing_rules with `type: mask_sequences` and a placeholder `[CC]`. The Agent fails to start with a YAML parsing error. Likely cause:

A) `replace_placeholder` requires double quotes.
B) Backslashes in the regex were not escaped properly in YAML.
C) `mask_sequences` is not a valid type.
D) Datadog requires the regex to be in PCRE format.

**Q15.** A team's Vector instance forwards logs to Datadog. They want each log to have a tag `forwarded_via:vector`. Which configuration achieves this?

A) Add the tag in Vector before posting (Vector's Datadog logs sink supports tags).
B) Add it in Datadog as an exclusion filter.
C) Add it via Sensitive Data Scanner.
D) Datadog auto-tags logs forwarded by Vector.

---

## Answer Key

1. **B** — `logs_enabled: true` is the master switch; without it, no Logs Agent block appears in `status`.
2. **A, C** — ACLs or group membership without weakening permissions.
3. **A** — Adjust the Agent's runtime user/permissions and volume mounts.
4. **B** — Best practice: drop noise first, then mask survivors, then aggregate. (Order matters because each rule consumes CPU; drop early.)
5. **B** — `include_at_match` keeps only matching raw lines.
6. **A, C** — Env var or YAML setting on the Agent. (B), (D), (E) wrong.
7. **A** — `ad.datadoghq.com/<container-name>.logs`.
8. **B** — Forwarder Lambda subscribed to the S3 bucket.
9. **A** — `ddtags` is parsed as comma-separated tags applied to logs.
10. **A, B** — Edge keeps data off; SDS is post-ingestion.
11. **B** — Default `service` becomes `unknown` — exam common gotcha.
12. **C** — Both entries match → duplicate logs. Use `exclude_paths` or refine globs.
13. **A, B** — journald support and `include_units`. (C) wrong; (D) wrong (journald uses cursor-based reading); (E) wrong.
14. **B** — YAML escaping is the #1 issue with regex patterns.
15. **A** — Vector's Datadog sink supports a `tags` key in its config.
