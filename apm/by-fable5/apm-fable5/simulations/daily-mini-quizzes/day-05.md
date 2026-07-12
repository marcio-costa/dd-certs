# Daily Mini-Quiz #5 — Mixed (post-Domain 7 study)

10 questions · ~12 minutes · Heavier on Troubleshooting + cross-domain.

---

**Q1.** `datadog-agent flare <ticket-id>` does what?

A) Restarts the Agent · B) Builds a support bundle and uploads to Datadog Support · C) Drops a ticket · D) Toggles debug logging

**Q2.** A team's logs land in the wrong region's Datadog org. Likely cause:

A) Wrong `site:` · B) Wrong API key · C) Misspelled service · D) DNS failure

**Q3.** A K8s DaemonSet is missing volume mounts for `/var/log/pods`. Result:

A) Pods crash-loop · B) Container logs aren't readable; collection silently fails · C) Auto-mounted by Datadog · D) Logs go to syslog

**Q4.** `LogsProcessed: 1M  LogsSent: 250k` suggests:

A) Wrong API key · B) Edge `exclude_at_match` dropping ~75% · C) Agent crashed · D) Datadog throttling org

**Q5.** Reading `app.log` fails with `permission denied`. Best fix:

A) `chmod 777` · B) Run as root · C) `setfacl -m u:dd-agent:rx` on file + parent dir · D) Disable SELinux

**Q6.** HTTP intake returns `403 Forbidden`. Most likely:

A) Payload too large · B) Wrong API key · C) Site is down · D) Wrong `Content-Type`

**Q7.** HTTP intake returns `413 Payload Too Large`. Most likely:

A) Payload exceeded 5 MB · B) Wrong API key · C) Wrong site · D) Missing headers

**Q8.** Multi-line stack traces split into many logs. Fix:

A) `multi_line` rule whose pattern matches start of new log · B) `mask_sequences` · C) Increase Agent memory · D) Disable parsing

**Q9.** Logs show `source: "unknown"` and unparsed `message`. Fix:

A) Set `source:` to a value matching an integration name · B) Restart the Agent · C) Increase index retention · D) Add a Multi-Alert

**Q10.** A team uses `mask_sequences` but PII still leaks through. Two TRUE causes? (Pick the BEST one if forced to one)

A) YAML escaping wrong; OR regex needs lookahead the RE2 engine doesn't support · B) Datadog disabled the rule · C) Dependency missing on host · D) The Agent runs in privileged mode

---

## Answer Key

1. B 2. A 3. B 4. B 5. C 6. B 7. A 8. A 9. A 10. A
