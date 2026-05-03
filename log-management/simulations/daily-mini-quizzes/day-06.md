# Daily Mini-Quiz #6 — Mixed (Week 4 review)

10 questions · ~12 minutes · Hard mix of all domains.

---

**Q1.** A log has both a JSON `service` field and an Agent-set `service:` in `conf.yaml`. Which value typically wins on the log envelope?

A) The Agent-set value (collection-time) · B) The JSON value (auto-mapped) · C) Random · D) They are concatenated

**Q2.** Pipelines support nested pipelines up to:

A) 1 level · B) 3 levels · C) Unlimited · D) Nesting not supported

**Q3.** Which TWO are valid forwarding destinations? (Select TWO)

A) Splunk HEC · B) DynamoDB · C) Elasticsearch · D) MongoDB Atlas · E) AWS RDS

**Q4.** A team needs `country` tag from `@network.client.ip`. Best processor sequence:

A) GeoIP parser → Standard Attribute Remapper (or Remapper to tag) · B) URL parser · C) Lookup · D) Reference Table only

**Q5.** A measure's unit is set to nanoseconds. The query `@duration:>1000`:

A) Matches logs where `@duration` > 1000 nanoseconds · B) Matches `@duration` > 1000 ms · C) Datadog auto-converts · D) Invalid

**Q6.** Standard attribute remapper aligns custom field names to:

A) Tags · B) Standard Datadog attributes (e.g., `@usr.id`) · C) Reserved attributes · D) Dashboards

**Q7.** A team wants to find logs **without** `@error.kind` set:

A) `@error.kind:null` · B) `-@error.kind:*` · C) `@error.kind:""` · D) `NOT @error.kind`

**Q8.** A team's `multi_line` rule pattern is `^\d{4}-\d{2}-\d{2}` and a stack-trace line `at com.example.Foo.bar(...)` arrives:

A) Becomes its own log · B) Is appended to the previous matching line as one composite log · C) Causes Agent crash · D) Is dropped

**Q9.** Logs to Metrics aggregations include:

A) count + distribution · B) count + cardinality · C) histogram + percentile · D) avg + sum

**Q10.** A team adds an exclusion filter on `level:debug` at 100%. They still want a count of debug logs over time. The path:

A) Disable exclusion · B) Logs to Metrics (count by service) · C) Move debug to new index · D) Self-host

---

## Answer Key

1. A 2. A 3. A, C 4. A 5. A 6. B 7. B 8. B 9. A 10. B
