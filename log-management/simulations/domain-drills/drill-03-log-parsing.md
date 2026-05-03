# Drill 03 — Log Parsing (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A team's Java app logs in this format:
```
2026-05-03 14:22:01.123 ERROR [thread-1] com.example.OrderService - Order 4521 failed: timeout
```
Which Grok rule extracts `timestamp`, `level`, `thread`, `class`, `message`?

A) `%{date("yyyy-MM-dd HH:mm:ss.SSS"):timestamp} %{word:level} \[%{notSpace:thread}\] %{notSpace:class} - %{data:message}`
B) `%{notSpace:timestamp} %{notSpace:level} %{notSpace:thread} %{notSpace:class} %{notSpace:message}`
C) `%{date:timestamp} %{level} %{thread} %{class} %{message}`
D) `%{ISO8601:timestamp} %{LOGLEVEL:level} %{DATA:msg}`

**Q2.** A team writes a Grok parser with two match rules. Match-rule A matches but the wanted attribute is empty; match-rule B would match too. Which is TRUE?

A) Both rules apply; attributes are merged.
B) The first matching rule wins; B is never tried.
C) The Grok parser fails open.
D) Match rules are tried in random order.

**Q3.** A team needs to extract structured key-value pairs from a free-form log:
```
foo=1 bar="hello world" baz=3.14
```
Which Grok filter is most appropriate?

A) `:keyvalue`
B) `:json`
C) `:array`
D) `:regex(...)`

**Q4.** A team has a custom JSON log with a numeric field `request_duration_ms`. They want to do percentile aggregations on it. Which TWO are required? (Select TWO)

A) Declare it as a measure (numeric facet) with the unit set.
B) Declare it as a string facet.
C) Have the field present as a number, not a string, in the log.
D) Add a Status remapper.
E) Run a Date remapper.

**Q5.** Which TWO statements about pipeline ordering are TRUE? (Select TWO)

A) A log can be processed by multiple pipelines if multiple filters match.
B) Pipelines stop processing on the first match.
C) Within a pipeline, processors run in the order they appear.
D) Pipeline order is irrelevant — processors run by priority weight.
E) A nested pipeline runs only if the outer pipeline's filter matched.

**Q6.** A team uses an integration pipeline for `source:nginx`. They need a small custom enrichment (add a tag based on Host header). They should:

A) Edit the integration pipeline directly.
B) Clone the integration pipeline and edit the clone, OR add a custom pipeline after it.
C) Disable the integration pipeline and write everything from scratch.
D) Forward to a custom service for enrichment.

**Q7.** A team has a JSON field `severity_text: "Warning"` they want mapped to `status`. The most direct approach:

A) Add a Status remapper sourcing `severity_text`.
B) Rename to `level` so auto-JSON parsing handles it.
C) Use a Lookup processor with a 1:1 table.
D) Either A or B works.

**Q8.** A team wants to parse `request.user.id` from a nested JSON path so it appears at the top of the log envelope as `@usr.id`. The right tool:

A) Standard Attribute Remapper.
B) Status remapper.
C) Date remapper.
D) Lookup processor.

**Q9.** Which Grok matcher handles ISO-8601 timestamps with milliseconds and timezone (e.g., `2026-05-03T14:22:01.123+0000`)?

A) `%{date("yyyy-MM-dd HH:mm:ss"):timestamp}`
B) `%{date("yyyy-MM-dd'T'HH:mm:ss.SSSZ"):timestamp}`
C) `%{ISO8601:timestamp}`
D) `%{datetime:timestamp}`

**Q10.** A team's logs include latency in microseconds. The standard `@duration` is in **nanoseconds**. The fix:

A) Multiply by 1000 with an Arithmetic processor (or use `:scale(1000)` in Grok).
B) Divide by 1000.
C) Set unit on the measure to microseconds; `@duration` stays raw.
D) Convert to milliseconds.

**Q11.** A team accidentally sets a Date remapper before the Grok parser that produces the timestamp. The result:

A) Logs use the parsed timestamp normally.
B) Logs use the ingest timestamp (the remapper had nothing to consume).
C) The pipeline fails.
D) The Grok parser is auto-reordered.

**Q12.** A team needs to add `country` to logs based on `@network.client.ip`. The most efficient single processor:

A) GeoIP parser.
B) URL parser.
C) Lookup processor.
D) Reference Table lookup.

**Q13.** Which is the canonical recommended order of processors in a custom pipeline?

A) Restructure → enrich → parse → normalize.
B) Parse → normalize (remappers) → enrich → restructure.
C) Enrich → restructure → normalize → parse.
D) Normalize → enrich → parse → restructure.

**Q14.** A team's logs end up with multiple `service` values per host because some come in as JSON `service` and the Agent's config also sets a `service`. The behavior:

A) The Agent's collection-time `service` wins.
B) The JSON's `service` overrides because auto-JSON parsing sees it as reserved.
C) Multiple services are produced.
D) The log is rejected.

**Q15.** A team adds a String Builder processor with template `${@usr.first_name} ${@usr.last_name}`. The result `@usr.full_name` shows `null null` for many logs. Likely cause:

A) The String Builder requires both source attributes to be non-null on every log.
B) It's a known Datadog bug.
C) The String Builder must come before the parser.
D) Switch to a Lookup processor.

---

## Answer Key

1. **A** — Correct grok with date format and bracketed thread.
2. **B** — First matching rule wins.
3. **A** — `:keyvalue` parses key-value pairs (with quoted values handled).
4. **A, C** — Measure declaration + numeric type in source data.
5. **A, C, E** — Multi-pipeline + declared order + nested pipeline conditional.
6. **B** — Clone or compose with a custom pipeline. Don't edit integration pipelines directly (you can't, anyway).
7. **D** — Either approach works; Status remapper is explicit and reliable, renaming to `level` triggers auto-mapping.
8. **A** — Standard Attribute Remapper aligns nested JSON paths to standard attributes.
9. **B** — Use a custom date format string. (C) `ISO8601` is not a Datadog Grok matcher token — must use `date("...")` with explicit format.
10. **A** — Convert µs → ns by multiplying by 1000 (1 µs = 1000 ns). Either Arithmetic or `:scale(1000)`.
11. **B** — Without the source attribute being present at remapper run-time, ingest timestamp wins.
12. **A** — GeoIP parser is purpose-built for this.
13. **B** — Parse → normalize → enrich → restructure.
14. **A** — Collection-time `service` is set on the log envelope before pipelines; pipelines can re-map it. The Agent's value wins by default unless a remapper overrides.
15. **A** — String Builder produces `null null` strings if any source attribute is missing.
