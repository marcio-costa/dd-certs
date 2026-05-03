# Quiz 03 — Log Parsing (Domain 3)

**Coverage:** processors & pipelines · standard attributes · log composition
**Length:** 12 questions

---

### 1. Pipelines in Datadog Logs are evaluated:

A) In random order, picked at runtime.
B) In top-to-bottom order, with logs passing through every pipeline whose filter matches.
C) In top-to-bottom order, but a log stops at the first matching pipeline.
D) Bottom-to-top, then re-evaluated until no changes occur.

### 2. Within a single pipeline, processors execute:

A) In parallel.
B) In the order they appear in the pipeline definition.
C) Alphabetically by processor name.
D) By processor priority weight.

### 3. A team writes a Grok parser that extracts a `timestamp` attribute. The log appears with a timestamp matching the **Agent ingest time**, not the parsed time. What is the most likely cause?

A) The Grok parser failed silently.
B) There is no Date remapper consuming the parsed `timestamp`.
C) Datadog requires an Arithmetic processor to set timestamps.
D) The status remapper overwrote the timestamp.

### 4. Which TWO of the following are reserved attributes (top-level, no `@` prefix)? (Select TWO)

A) `host`
B) `@http.method`
C) `service`
D) `@usr.id`
E) `@duration`

### 5. A log arrives as JSON with a top-level field `severity: "ERROR"`. Without any custom pipeline, what does Datadog do?

A) Nothing — `severity` is treated as a regular attribute.
B) Auto-maps `severity` to the official `status` field.
C) Drops the log because `severity` is reserved.
D) Quarantines the log for manual review.

### 6. Which built-in Grok matcher captures a numeric value as a number (not a string)?

A) `%{data:value}`
B) `%{notSpace:value}`
C) `%{integer:value}`
D) `%{regex(.*)::value}`

### 7. Which standard attribute namespace stores HTTP request/response metadata?

A) `@network.*`
B) `@http.*`
C) `@usr.*`
D) `@error.*`

### 8. A team needs to add `country` as a tag based on the client IP `@network.client.ip`. The most efficient processor:

A) Lookup processor (with a country-table CSV).
B) GeoIP parser.
C) Category processor.
D) String builder.

### 9. After processing, a log line shows `status: "warning"` and `tags: ["env:prod", "team:web"]`. Which TWO of the following queries match this log? (Select TWO)

A) `status:warn`
B) `status:warning`
C) `team:web`
D) `@team:web`
E) `status:Warning` (uppercase)

### 10. Which best describes the difference between **match rules** and **support rules** in a Grok parser?

A) Match rules are case-sensitive; support rules are not.
B) Match rules are tried against the source attribute; support rules are reusable named patterns referenced by match rules.
C) Match rules run first; support rules run only on logs that match nothing.
D) They are aliases — Datadog uses one or the other depending on plan tier.

### 11. A custom JSON log has a field `usr_id`. To make it appear under the standard `@usr.id` namespace and feed the User entity dashboards, you should add a:

A) Status remapper.
B) Standard Attribute Remapper.
C) GeoIP parser.
D) Multi-line rule in the Agent.

### 12. Which TWO statements about JSON auto-parsing are correct? (Select TWO)

A) Datadog auto-parses JSON logs and promotes top-level keys to attributes.
B) JSON `level` or `severity` keys are automatically mapped to the official log `status`.
C) JSON auto-parsing requires enabling the *Auto JSON Pipeline* manually.
D) JSON fields with names matching reserved attributes (`host`, `service`) are ignored.
E) JSON auto-parsing only works for logs sent via the HTTP intake API.

---

## Answer Key

1. **B** — Pipelines run top-to-bottom. A log passes through *every* pipeline whose filter matches; it does not stop at the first match. (C) is the common wrong-answer trap.

2. **B** — Within a pipeline, processors execute in declared order. Reordering is significant — e.g., the Date remapper must come *after* the grok that produced the timestamp.

3. **B** — Without a Date remapper consuming the parsed `timestamp`, the official log timestamp falls back to ingest time.

4. **A, C** — Reserved attributes are the seven core fields: `host`, `service`, `source`, `status`, `message`, `timestamp`, `tags`. (B), (D), (E) are standard attributes (with `@`).

5. **B** — Auto-JSON parsing recognizes `level` and `severity` and remaps to `status`. Same is true for `timestamp` (date) and reserved fields like `host`, `service`, `source`, `message`.

6. **C** — `%{integer:value}` produces a JSON number. `%{data:...}` and `%{notSpace:...}` produce strings; (D) is invalid syntax.

7. **B** — `@http.*` is the HTTP namespace (`@http.method`, `@http.status_code`, `@http.url`, `@http.useragent`). `@network.*` is connection-level; `@usr.*` is user; `@error.*` is errors.

8. **B** — GeoIP parser is purpose-built for IP → country/city/coordinates. Lookup processor (A) would work but requires you to maintain a CSV.

9. **B, C** — `status:warning` matches the official status. `team:web` matches a tag. Note: tags don't take the `@` prefix; (D) would query an *attribute*, not a tag. (A) `warn` is *not* the same as `warning` (case-sensitive value).

10. **B** — Match rules are tried; support rules are reusable definitions referenced from match rules.

11. **B** — A Standard Attribute Remapper aligns custom names to standard attribute names (e.g., `usr_id` → `@usr.id`).

12. **A, B** — Auto-JSON: top-level keys become attributes, `level`/`severity`/`timestamp` keys auto-remap, `host`/`service`/`source`/`message` JSON keys are also recognized as reserved. (C) and (E) are false; (D) is false (those fields *are* recognized).

---

**Score:** below 9/12 → re-read `knowledge-base/03-log-parsing.md`.
