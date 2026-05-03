# Daily Mini-Quiz #1 — Mixed (post-Domains 1+2 study)

10 questions · ~12 minutes · Coverage skewed toward Domains 1–2.

---

**Q1.** Logs answer the **___** question in the three pillars of observability.

A) what · B) where · C) why · D) how much

**Q2.** Which setting enables log collection in the Agent?

A) `log_level: info` · B) `logs_enabled: true` · C) `enable_logs: 1` · D) `DD_LOGS_INJECTION=true`

**Q3.** Datadog's auto-JSON parsing maps which JSON field to the official log severity?

A) `loglevel` · B) `level` or `severity` · C) `priority` · D) `criticality`

**Q4.** Which `log_processing_rules` type drops a log at the Agent edge?

A) `mask_sequences` · B) `multi_line` · C) `exclude_at_match` · D) `include_at_match`

**Q5.** The HTTP Logs Intake endpoint host is:

A) `api.datadoghq.com` · B) `http-intake.logs.<site>` · C) `intake.datadoghq.com` · D) `logs.api.datadoghq.com`

**Q6.** A `multi_line` rule's `pattern` should match…

A) the end of a log line · B) the start of a new log line · C) all blank lines · D) only timestamps in UTC

**Q7.** In Kubernetes, the autodiscovery annotation key for log config on container `web` is:

A) `ad.datadoghq.com/web.logs` · B) `dd-agent/log/web` · C) `datadog.io/web/logs` · D) `kubernetes.io/datadog/web`

**Q8.** Which TWO are valid `type:` values in a `logs:` config? (Select TWO)

A) `file` · B) `tcp` · C) `journal` · D) `http` · E) `windows_event`

**Q9.** A team posts directly to the HTTP intake. The max single-payload size is:

A) 1 MB · B) 5 MB · C) 10 MB · D) 100 MB

**Q10.** A team uses Sensitive Data Scanner. Where does it run?

A) On the Agent · B) In the Datadog backend post-ingestion / pre-indexing · C) On the user's browser · D) On AWS S3

---

## Answer Key

1. C — Logs answer *why*.
2. B — `logs_enabled: true`.
3. B — `level` or `severity` are auto-mapped.
4. C — `exclude_at_match`.
5. B — `http-intake.logs.<site>`.
6. B — Start of a new log line.
7. A — `ad.datadoghq.com/<container-name>.logs`.
8. A, E — `file` and `windows_event`. Also `tcp`, `udp`, `journald`, `docker` are valid.
9. B — 5 MB payload max (and 1 MB per single log line).
10. B — Backend, after ingestion, before indexing.
