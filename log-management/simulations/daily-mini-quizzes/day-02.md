# Daily Mini-Quiz #2 — Mixed (post-Domains 3+4 study)

10 questions · ~12 minutes · Skews toward Parsing + Search/Filtering.

---

**Q1.** Pipelines run:

A) Bottom-to-top · B) Top-to-bottom; logs pass through every match · C) In random order · D) In parallel

**Q2.** Within a pipeline, processors run:

A) In declared order · B) Alphabetically · C) By weight · D) Randomly

**Q3.** The seven reserved attributes (top-level, no `@`) include all of the following EXCEPT:

A) `host` · B) `service` · C) `@http.method` · D) `status`

**Q4.** A team's logs are JSON with `severity: "WARN"`. Without any custom pipeline, the official `status` is:

A) `info` · B) `warn` · C) empty · D) `error`

**Q5.** Live Tail's approximate window is:

A) 60 sec · B) 15 min · C) 1 hr · D) Index retention

**Q6.** Inclusive range query for HTTP 5xx:

A) `@http.status_code:[500 TO 599]` · B) `@http.status_code:{500 TO 599}` · C) `@http.status_code:500-599` · D) `@http.status_code BETWEEN 500 AND 599`

**Q7.** The query `-@usr.id:*` matches logs where:

A) `@usr.id` exists · B) `@usr.id` is null · C) `@usr.id` does not exist · D) `@usr.id` is uppercase

**Q8.** Top List supports how many group-by dimensions?

A) 1 · B) 2 · C) 4 · D) 8

**Q9.** A custom JSON key isn't appearing in the facet sidebar. Fix:

A) Reduce log retention · B) Declare it as a facet via the log side panel · C) Restart the Agent · D) Add to a saved view

**Q10.** Patterns clusters logs by:

A) `dd.trace_id` · B) Similar text shape (variable parts → `*`) · C) Five-min buckets · D) Host

---

## Answer Key

1. B 2. A 3. C 4. B 5. B 6. A 7. C 8. A 9. B 10. B
