# Quiz 06 — Log Utilization (Domain 6)

**Coverage:** monitors & alerting · metric generation · exporting (archives, rehydration, forwarding)
**Length:** 12 questions

---

### 1. A standard log monitor query has the form `logs("<query>").index("<idx>").rollup("count").last("5m") > N`. What does `last("5m")` define?

A) The recovery threshold.
B) The evaluation window.
C) The renotification interval.
D) The minimum data points before the monitor starts evaluating.

### 2. A team wants one alert per service for a "errors > 50/min" condition. The right monitor option is:

A) Anomaly monitor.
B) Multi-Alert (group by `service`).
C) Composite monitor.
D) Outlier monitor.

### 3. Generated metrics from logs (Logs to Metrics) work on…

A) Only indexed logs.
B) Only archived logs.
C) All ingested logs, including those excluded from indexes.
D) Only logs sent through the HTTP intake.

### 4. The maximum number of group-by tag dimensions on a metric generated from logs is:

A) 4
B) 8
C) 10
D) Unlimited

### 5. Which TWO Logs-to-Metrics aggregations are supported? (Select TWO)

A) `count`
B) `distribution` over a numeric attribute
C) `unique` of a string attribute
D) `histogram_bucket`
E) `linear_regression`

### 6. Log Archives can be sent to which TWO cloud destinations? (Select TWO)

A) AWS S3.
B) AWS RDS.
C) Azure Blob Storage.
D) AWS Kinesis Firehose.
E) Datadog's hosted archive (US1 region only).

### 7. **Rehydration** in Datadog Logs means:

A) Re-encrypting archives with a new KMS key.
B) Re-ingesting archived logs into a temporary index for searching.
C) Re-running pipelines on already-indexed logs.
D) Re-issuing the Agent's API key.

### 8. The Log Forwarding feature can push logs in real time to which destinations? (Select THREE)

A) Splunk HEC
B) Elasticsearch / OpenSearch
C) Generic HTTP webhook
D) AWS DynamoDB
E) MongoDB Atlas
F) Microsoft Sentinel

### 9. A team excludes 100% of `level:debug` logs from the `main` index. They still want to count debug logs per service over time. The right approach is:

A) Reduce the exclusion filter to 0%.
B) Create a Logs to Metrics rule from the same query (count, group by `service`).
C) Move `level:debug` logs to a separate index.
D) Send debug logs only via HTTP intake.

### 10. Which TWO statements about log monitor groups are correct? (Select TWO)

A) Multi-Alert creates one alert per unique tag value of the group-by.
B) Multi-Alert creates a single alert that lists all groups in the body.
C) The "group retention time" controls how long the monitor remembers a group before dropping it.
D) Group-by is only available for anomaly monitors.
E) A monitor can group by at most 1 tag.

### 11. Which best describes the difference between `mask_sequences` (Agent rule) and Sensitive Data Scanner (server-side)?

A) They are aliases of the same feature.
B) `mask_sequences` runs at the Agent edge; Sensitive Data Scanner runs in the Datadog backend after ingestion.
C) `mask_sequences` only handles credit cards; Sensitive Data Scanner handles all PII.
D) Sensitive Data Scanner runs on the Agent; `mask_sequences` runs on archives.

### 12. The biggest cost benefit of *Logging Without Limits™* is that you can:

A) Disable ingestion entirely.
B) Retain logs forever in indexes for free.
C) Ingest all logs at low cost while choosing precisely which to index.
D) Avoid AWS S3 storage charges.

---

## Answer Key

1. **B** — `last("5m")` is the time window evaluated. The threshold (`> N`) and recovery threshold are configured separately.

2. **B** — Multi-Alert grouped by a tag yields one alert per unique tag value.

3. **C** — Generated metrics work on all ingested logs (the headline benefit of Logging Without Limits™).

4. **C** — Up to 10 tag dimensions on a generated metric.

5. **A, B** — `count` and `distribution` are supported. (C–E) are not Logs-to-Metrics aggregations.

6. **A, C** — S3 and Azure Blob (and GCS, not in the choices) are valid archive destinations. RDS and Kinesis Firehose are not archive targets; (E) is fictional.

7. **B** — Rehydration re-ingests archived logs into a temporary index for re-search.

8. **A, B, C, F** — Splunk HEC, Elasticsearch/OpenSearch, HTTP webhook, and Microsoft Sentinel. The question asked for THREE — pick any three of the four valid ones; the wrong distractors are DynamoDB (D) and MongoDB Atlas (E).

9. **B** — Generated metrics work on excluded logs, so a count grouped by `service` solves the analytics need without indexing.

10. **A, C** — Multi-Alert = one alert per group; group retention controls how long groups live. (B), (D), (E) are wrong.

11. **B** — Edge vs. backend. Edge masking happens on the host before transmission; SDS runs in the backend after ingestion (so the data was visible to Datadog before scrubbing).

12. **C** — Decoupling ingestion (low cost) from indexing (selective) is the headline value.

---

**Score:** below 9/12 → re-read `knowledge-base/06-log-utilization.md`.
