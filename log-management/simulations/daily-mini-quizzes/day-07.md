# Daily Mini-Quiz #7 — Final (pre-Mock #2)

10 questions · ~12 minutes · The trickiest mix.

---

**Q1.** Three reserved Unified Service Tags (USTs):

A) `env`, `service`, `version` · B) `host`, `region`, `cluster` · C) `team`, `app`, `tier` · D) `prod`, `dev`, `qa`

**Q2.** A team's pipeline has Date remapper *before* the Grok parser that produces `timestamp`. Result:

A) Parsed timestamp wins · B) Ingest timestamp wins (remapper had nothing to consume) · C) Pipeline fails · D) Auto-reordered

**Q3.** A team uses Saved View URL `?query=...` to override. Which parameter name?

A) `?filter=` · B) `?query=` · C) `?search=` · D) `?q=`

**Q4.** Index retention options are typically (pick ones DD lists):

A) 1, 3, 5, 7 days · B) 3, 7, 15, 30, 60, 90 days · C) Continuous · D) 1 year minimum

**Q5.** Which TWO are TRUE about generated metrics? (Select TWO)

A) Up to 10 group-by tags · B) Work on excluded logs · C) Replace the original logs in archives · D) Auto-rehydrate · E) Require an index

**Q6.** A log monitor query against `index:*`:

A) Queries every index · B) Queries default only · C) Errors out · D) Bypasses exclusion filters

**Q7.** A team's Browser SDK posts logs. Which TWO are TRUE? (Select TWO)

A) Direct posts to HTTP intake · B) Requires Agent on user's machine · C) Can correlate with RUM · D) Stores logs locally first · E) Chrome only

**Q8.** A team forwards logs from on-prem hosts via Vector. The Datadog logs sink in Vector posts to:

A) The Agent over UDP · B) The HTTP Logs Intake API · C) Metrics API · D) Datadog SaaS Forwarder

**Q9.** A regulator needs PII to never reach Datadog. Best approach:

A) Sensitive Data Scanner with redaction · B) Index exclusion filter · C) Agent `mask_sequences` rule · D) Generate Metrics

**Q10.** Which TWO trace IDs link a log to its APM span? (Select TWO)

A) `dd.trace_id` · B) `dd.host_id` · C) `dd.span_id` · D) `dd.org_id` · E) `dd.run_id`

---

## Answer Key

1. A 2. B 3. B 4. B 5. A, B 6. A 7. A, C 8. B 9. C 10. A, C
