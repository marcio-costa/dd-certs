# Drill 08 — Corner Cases & Exam Traps (Mixed, All Domains)

**20 questions · ~35 minutes · The traps that separate a first-attempt pass from a retake.**

Every question here targets a documented behavior that beginners commonly get wrong. Take this drill in Week 4, after both mock exams.

---

**Q1.** A team batches logs into a single HTTP intake request as a JSON array. The request is rejected even though the payload is under 5 MB. The most likely cause:

A) The array contains more than 1,000 log entries (the per-request array limit).
B) Arrays are not supported — one log per request.
C) The `Content-Type` must be `text/plain` for arrays.
D) Arrays require an application key in addition to the API key.

**Q2.** In Kubernetes, a service's JSON logs contain a `host` field set by the application. After ingestion, the logs stop inheriting host-level tags (like `availability-zone`). Why?

A) Kubernetes strips host tags from JSON logs.
B) Preprocessing for JSON logs maps the JSON `host` field onto the reserved `host` attribute, overriding the Agent hostname — so the log no longer matches the real host.
C) The DaemonSet lost its volume mounts.
D) Host tags only apply to metrics.

**Q3.** A JSON log arrives with no `level`, `severity`, or `status` field at all. Without any custom pipeline, its official `status` is:

A) `error`
B) `info` (the default when no status attribute is recognized)
C) `debug`
D) Empty — the log fails to index

**Q4.** A team searches `@usr.email:JOHN@ACME.COM` but the logs store `john@acme.com`. The result:

A) The logs match — searches are case-insensitive.
B) The logs do NOT match — attribute searches are case sensitive.
C) Datadog throws a syntax error.
D) Only the domain part is matched.

**Q5.** Which TWO options can be configured on an index's **daily quota**? (Select TWO)

A) A custom reset time (instead of midnight UTC).
B) A warning threshold (get notified before the quota is hit).
C) An automatic retention extension.
D) A failover index for overflow logs.
E) Per-user quotas.

**Q6.** A team excluded 100% of `status:debug` logs from all indexes but must still alert when debug volume exceeds a threshold. The supported pattern:

A) A log monitor on the excluded logs.
B) A Live Tail monitor.
C) A Logs to Metrics rule on the debug query, then a **metric monitor** on the generated metric.
D) A Watchdog alert on the archive.

**Q7.** A role already has restriction query "team:frontend". An admin assigns it a new restriction query "team:backend". The result:

A) The role now sees logs matching either query.
B) The role sees only logs matching both queries (intersection).
C) The new query **replaces** the old one — a role has exactly one restriction query.
D) Datadog rejects the second assignment.

**Q8.** During an incident, a Live Tail query matching a huge log volume shows fewer logs than expected. The documented explanation:

A) Live Tail displays only indexed logs.
B) Live Tail output is **uniformly sampled** when too many logs match, to stay readable while remaining statistically representative.
C) Live Tail caps at 100 logs total.
D) The Agent throttles at the edge.

**Q9.** How is **rehydration** billed?

A) It's free — the logs were already paid for at ingestion.
B) Rehydrated logs are billed as **indexed events** for the rehydrated volume.
C) Billed per GB scanned in the archive, like Athena.
D) Flat monthly fee.

**Q10.** Log-based metrics (Logs to Metrics) are retained:

A) As long as the source index retention.
B) 15 months, at 10-second granularity.
C) 30 days.
D) Forever, at 1-hour granularity.

**Q11.** A team's `multi_line` pattern uses a regex lookahead `(?=...)`. The Agent fails to apply it. Why?

A) Lookaheads need double escaping in YAML.
B) The Agent's regex engine is Go RE2, which does not support lookahead/lookbehind.
C) `multi_line` only accepts literal strings.
D) Lookaheads require Agent v5.

**Q12.** A team tails `/var/log/myapp/*.log` but wants to skip `/var/log/myapp/noisy.log`. The right Agent config key:

A) `exclude_paths` in the same `logs:` entry.
B) `skip_files` in `datadog.yaml`.
C) An index exclusion filter.
D) `ignore: true` on a second entry.

**Q13.** An Agent `logs:` entry has TWO `include_at_match` rules. A log line is forwarded when:

A) It matches at least one rule (OR).
B) It matches **all** include rules (AND).
C) The first rule wins; the second is ignored.
D) Multiple include rules are invalid.

**Q14.** A team wants to tweak one processor inside the out-of-the-box NGINX integration pipeline. Which statements is TRUE?

A) Integration pipelines can be edited in place.
B) Integration pipelines can be **cloned**; you edit the clone (and can disable the original).
C) Integration pipelines are immutable and cannot be cloned.
D) You must contact Datadog Support to edit them.

**Q15.** The fastest way to see exactly **which pipelines and processors touched a specific log** is:

A) Reading the `_dd` metadata block manually.
B) The **Pipeline Scanner** (from the log side panel or the Pipelines page).
C) Running `datadog-agent stream-logs`.
D) Exporting the log to a notebook.

**Q16.** A team wants to be alerted inside Datadog when any index hits its daily quota. A documented approach is:

A) Monitor the Datadog-generated events — e.g., search `source:datadog "daily quota reached"` and alert on it.
B) Poll the index API every minute from a Lambda.
C) Quota alerts are not possible.
D) Watchdog automatically pages you.

**Q17.** With `container_collect_all: true`, the Agent also collects its **own** container logs, creating noise. The cleanest fix:

A) `DD_CONTAINER_EXCLUDE` (e.g., `name:datadog-agent`) so the Agent skips those containers.
B) An index exclusion filter on `service:agent`.
C) Disable `container_collect_all`.
D) Run the Agent outside Docker.

**Q18.** A team has huge log volumes they rarely query but must keep searchable for ~6–15 months without paying full indexing cost or running rehydrations. The Datadog storage tier designed for this:

A) A second standard index with 90-day retention.
B) **Flex Logs** (flexible storage tier between standard indexing and archives).
C) Live Tail.
D) Metrics without Limits.

**Q19.** Rules under `logs_config.processing_rules` in `datadog.yaml` (global scope) differ from `log_processing_rules` inside an integration's `conf.yaml` because global rules:

A) Apply to **all** logs collected by the Agent, not just one source.
B) Only support `multi_line`.
C) Run in the Datadog backend.
D) Require a restart per rule change.

**Q20.** A JSON log contains `"msg": "user login ok"` and no `message` field. In the Explorer, the log line displays "user login ok" because:

A) The Explorer always shows the raw JSON.
B) `msg` is in the default **message** attribute list of JSON preprocessing (`message`, `msg`, `log`), so it's mapped to the reserved `message`.
C) A hidden Grok parser ran.
D) `msg` was declared as a facet.

---

## Answer Key

1. **A** — HTTP intake limits: **5 MB uncompressed payload**, **1 MB per single log**, **max 1,000 logs per array**. All three show up as distractors for each other on the exam.
2. **B** — Documented Kubernetes gotcha: JSON `host`/`hostname`/`syslog.hostname` overrides the Agent hostname during preprocessing; Datadog recommends clearing those attributes. The log then fails to inherit host-level tags.
3. **B** — When no status attribute is recognized, logs default to **INFO**. Fix by adding the real severity attribute to the preprocessing status-attribute list (put it early — order gives precedence).
4. **B** — Attribute searches are **case sensitive**. Normalize case in a pipeline (Grok `lowercase` filter / attribute remapper) if your data is inconsistent.
5. **A, B** — Daily quotas support a custom reset time and a warning threshold. There is no overflow index (over-quota logs are still ingested, archived, and metric-eligible — just not indexed).
6. **C** — Log monitors evaluate **indexed** logs only, and there is no monitor on Live Tail. Excluded logs can only drive alerting through generated metrics.
7. **C** — One restriction query per role; assigning a new one removes the previous assignment. Users needing multiple scopes get multiple roles.
8. **B** — Live Tail samples uniformly at random under very high match volume; refine the query to see everything.
9. **B** — Rehydrated logs are billed as indexed events (`logs_rehydrated_indexed_events` usage type). Scope rehydrations with a query + narrow time range.
10. **B** — 15 months at 10-second granularity — much longer than any index retention, which is why Logs to Metrics is the long-term trend answer.
11. **B** — Agent regexes are Go **RE2**: no lookahead/lookbehind/backreferences. Rewrite the pattern to match the start of a new log entry directly.
12. **A** — `exclude_paths` complements a wildcard `path:` within the same entry.
13. **B** — Multiple `include_at_match` rules must **all** match (logical AND). To OR conditions, use one rule with an alternation regex (`ERROR|FATAL`).
14. **B** — Clone, edit the clone, optionally disable the original. Originals stay Datadog-managed so they can be updated.
15. **B** — Pipeline Scanner traces a live log through every matching pipeline/processor — the canonical "why isn't this parsed right?" tool.
16. **A** — Datadog emits `datadog_index` events (e.g., "daily quota reached") searchable via `source:datadog`; alert on them like any event/log.
17. **A** — `DD_CONTAINER_EXCLUDE` (or `container_exclude`) filters containers out of Autodiscovery/collection at the Agent — no ingestion cost at all. An index exclusion filter (B) would still pay ingestion.
18. **B** — Flex Logs: keep logs queryable for long windows at lower cost than standard indexing, without the rehydration workflow. Know it exists and where it sits in the tiers.
19. **A** — Global `logs_config.processing_rules` apply Agent-wide (all sources); per-integration `log_processing_rules` scope to that source. Same rule types in both.
20. **B** — Default message attributes for JSON preprocessing: `message`, `msg`, `log`. Preprocessing maps them to the reserved `message` shown as the log's headline.

---

**Score:** below 16/20 → re-read the "corner case" callouts in the knowledge base (03 §3.6 preprocessing, 05 §5.1, 06 §6.1/§6.4, 07 §7.7) and retake in 3 days.
