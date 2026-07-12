# Daily Mini-Quiz #4 — Mixed (post-Mock #1)

10 questions · ~12 minutes · Reinforces weak-area patterns common after Mock #1.

---

**Q1.** A team needs an alert *per service* on errors. Best monitor option:

A) Anomaly · B) Multi-Alert grouped by service · C) Composite · D) Outlier

**Q2.** Generated metrics from logs work on:

A) Indexed logs only · B) Archived only · C) All ingested logs (incl. excluded) · D) HTTP-intake only

**Q3.** Maximum group-by tags on a generated metric:

A) 4 · B) 8 · C) 10 · D) Unlimited

**Q4.** Valid Logs-to-Metrics aggregations include:

A) `count` and `unique` · B) `count` and `distribution` · C) `histogram` only · D) `regression`

**Q5.** Valid log archive destinations include all of these EXCEPT:

A) AWS S3 · B) Azure Blob · C) GCS · D) AWS DynamoDB

**Q6.** Rehydration:

A) Re-encrypts archives · B) Re-ingests archived logs into a temporary index · C) Restores deleted indexes · D) Replays pipelines

**Q7.** Forwarding destinations include all EXCEPT:

A) Splunk HEC · B) Elasticsearch · C) Microsoft Sentinel · D) MongoDB Atlas

**Q8.** A team wants to drop healthchecks **before** ingestion is billed:

A) Exclusion filter (index) · B) Pipeline filter · C) Agent `exclude_at_match` · D) SDS

**Q9.** First troubleshooting command on a Linux host with no logs:

A) `datadog-agent restart` · B) `datadog-agent status` · C) `tail /var/log/syslog` · D) `journalctl -u datadog-agent`

**Q10.** Live Tail shows logs but Explorer does not. Likely:

A) Live Tail bug · B) Index exclusion / wrong inclusion · C) Wrong API key · D) Agent crash

---

## Answer Key

1. B 2. C 3. C 4. B 5. D 6. B 7. D 8. C 9. B 10. B
