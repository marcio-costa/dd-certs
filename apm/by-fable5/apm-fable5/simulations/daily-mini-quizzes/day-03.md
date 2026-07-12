# Daily Mini-Quiz #3 — Mixed (post-Domain 5 study)

10 questions · ~12 minutes · Skews toward Analysis + cross-domain.

---

**Q1.** An exclusion filter on an index drops a log from being:

A) Ingested · B) Indexed · C) Archived · D) Used for metric generation

**Q2.** A 100% exclusion filter on `source:nginx @http.url:/health*` means:

A) Healthchecks aren't ingested · B) Healthchecks aren't indexed (Explorer); still archived + metrics-eligible · C) Healthchecks are deleted from archives · D) The Agent stops sending healthchecks

**Q3.** Index inclusion filters use:

A) Raw regex · B) The Explorer query language · C) JSONPath · D) SQL

**Q4.** A daily quota is reached at noon. Subsequent logs that match the index:

A) Are dropped at ingestion · B) Still ingested + archived but not indexed · C) Trigger Agent crash · D) Queued and indexed at midnight

**Q5.** To compute `pc99(@duration)`, the attribute must be a:

A) String facet · B) Measure with a unit · C) Tag · D) Pattern

**Q6.** Maximum group-by dimensions for **Tree Map** visualization:

A) 1 · B) 2 · C) 4 · D) 8

**Q7.** View In Context shows:

A) All logs in the org during that minute · B) Logs from same `host` and `service` around that timestamp · C) Logs with same `dd.trace_id` only · D) The pipeline's processors

**Q8.** Cheapest way to retain logs 5 years for occasional historical investigation:

A) Index retention 5 years · B) Increase daily quota · C) Archives + Rehydrate · D) Self-hosted Elasticsearch

**Q9.** A team excludes `level:debug` 100% from `main`. They still need debug count per service. The right approach:

A) Reduce exclusion to 0% · B) Logs to Metrics rule (count, group by service) · C) Move debug to a new index · D) Self-host

**Q10.** A log can match how many indexes?

A) Multiple — every matching index ingests · B) Exactly one — first match wins · C) Up to 3 · D) None unless tagged

---

## Answer Key

1. B 2. B 3. B 4. B 5. B 6. C 7. B 8. C 9. B 10. B
