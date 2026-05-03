# Drill 06 — Log Utilization (Hard / Scenario)

**15 questions · ~25 minutes**

---

**Q1.** A team configures a log monitor: `logs("status:error service:checkout").index("main").rollup("count").last("5m") > 100`. Which TWO statements are TRUE? (Select TWO)

A) The monitor evaluates every minute by default.
B) The monitor only fires if 5xx is sustained over the entire 5-minute window.
C) `index("main")` limits the query to the `main` index.
D) Adding `by service` would create per-service alerts when **Multi-Alert** is enabled.
E) The monitor runs against archives.

**Q2.** A team needs an alert on **error rate ratio** (errors/total > 5%) per service. Which configuration?

A) Two threshold monitors and a composite monitor combining them.
B) Multi-Alert grouped by service with two computes (count of `status:error` and total count) and a formula.
C) Anomaly monitor only.
D) Outlier monitor only.

**Q3.** Generated metrics from logs work on logs that are excluded from indexing.

A) True — that's the headline value of Logging Without Limits™.
B) False — only indexed logs feed metric generation.
C) True only if the exclusion filter is below 100%.
D) False — generated metrics only run from archived logs.

**Q4.** Which TWO Logs-to-Metrics aggregation types are supported? (Select TWO)

A) `count`
B) `distribution` over a numeric attribute
C) `unique`
D) `linear_regression`
E) `histogram`

**Q5.** The maximum number of group-by tag dimensions on a generated metric is:

A) 4
B) 8
C) 10
D) Unlimited

**Q6.** A team wants `nginx.requests` count grouped by `service`, `@http.method`, and `@http.status_code`. Which TWO statements are TRUE? (Select TWO)

A) The metric will be tagged with all three on every data point.
B) Custom metric cardinality is the product of unique tag combinations — keep an eye on cost.
C) Datadog deduplicates tag combinations across data points.
D) The metric updates retroactively for last week's logs.
E) The metric is searchable in the Log Explorer.

**Q7.** A team archives logs to S3. Which TWO statements are TRUE? (Select TWO)

A) Datadog requires an IAM role (or access key) with PutObject permission on the bucket.
B) Files are stored as compressed JSON (`.json.gz`).
C) Archives are encrypted only via customer KMS keys.
D) Archives include `_dd` internal metadata fields.
E) Archives only retain indexed logs.

**Q8.** Rehydration:

A) Re-indexes archived logs into a temporary rehydrated index for a chosen retention period.
B) Re-encrypts archives.
C) Restores deleted indexes.
D) Replays logs through the pipelines once more.

**Q9.** A team wants to forward only `service:web` logs to Splunk via HEC. The setup:

A) Forwarding configuration with a filter query `service:web` pointing at Splunk HEC URL/token.
B) An Agent rule on every host.
C) A custom Lambda Forwarder.
D) A pipeline processor.

**Q10.** Which TWO statements about Log Forwarding are TRUE? (Select TWO)

A) Destinations include Splunk HEC, Elasticsearch/OpenSearch, generic HTTP, Microsoft Sentinel.
B) Forwarding sends a copy of logs in real time.
C) Forwarding requires the Datadog Agent in the destination network.
D) Forwarding replaces archives.
E) Forwarding does not affect indexing decisions.

**Q11.** A team excludes 100% of healthcheck logs from the `main` index. They still want to count healthchecks per service per minute for SLO calculations. Which approach is best?

A) Disable the exclusion filter.
B) Create a Logs to Metrics rule (count, group by `service`) on the same query.
C) Move healthchecks to a separate index.
D) Forward healthcheck logs to a self-hosted system.

**Q12.** A team monitors a query against `index:*`. They notice some indexed logs are missing from the count. Likely cause:

A) Some indexes have restriction queries that hide logs from the user role running the monitor.
B) Live Tail interfered.
C) The monitor's threshold rejected them.
D) `index:*` is invalid syntax.

**Q13.** Which TWO are valid actions for a log monitor's notification body? (Select TWO)

A) `@email` recipients in the message.
B) `@slack-channel-name` for Slack.
C) Inline `{{value}}` template variable.
D) Nested SQL queries.
E) `@here` in arbitrary destinations.

**Q14.** A team configures a *renotify if not resolved every 30m*. The result:

A) The monitor continues to send a reminder every 30 minutes while still in alert.
B) The monitor auto-resolves after 30 minutes.
C) The monitor downgrades severity every 30 minutes.
D) The monitor stops alerting after 30 minutes.

**Q15.** A team uses Generate Metrics with a custom metric name. Which is the right format?

A) `nginx.requests.count` (dot-separated, lowercase, max 200 chars).
B) `nginx-requests-count` (kebab case).
C) `Nginx Requests Count` (spaces).
D) `nginx_requests_count` (snake case is invalid).

---

## Answer Key

1. **C, D** — `index("main")` scopes; `by service` + Multi-Alert yields per-service alerts. (A) eval frequency is configurable; (B) wrong (rollup count over window — not "sustained over entire 5min"); (E) monitors run against indexed logs.
2. **B** — Multi-Alert with two computes and formula.
3. **A** — Logging Without Limits™.
4. **A, B** — `count` and `distribution`.
5. **C** — 10.
6. **A, B** — Tagging applied + cardinality drives cost.
7. **A, B** — IAM PutObject + JSON.gz format.
8. **A** — Temporary rehydrated index.
9. **A** — Forwarding config with filter.
10. **A, B, E** — Multiple destinations + real-time + doesn't affect indexing — pick any TWO.
11. **B** — Logs to Metrics works on excluded logs.
12. **A** — Restriction queries are RBAC.
13. **A, C** — Both supported in monitor templates.
14. **A** — Reminder every 30m while unresolved.
15. **A** — Datadog metric naming: dot-separated lowercase.
