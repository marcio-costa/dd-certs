# Domain Drill #2 — Datadog Setup (20 Q)

> Week 1 / Week 2 consolidation. ~25 minutes timed.

**1.** A team needs long-term retention with the ability to query *some* of the data urgently if there's an incident. Best fit:
A. Standard Indexing · B. Flex Logs · C. Log Archives only · D. Live Tail

**2.** Cheapest tier for logs only needed for forensics later:
A. Standard Indexing · B. Flex Logs · C. Log Archives · D. Detection bus

**3.** Which configuration prevents detection on a log source even if it is being ingested?
A. Tagging the log with `env:prod`
B. Excluding it from the Security Filter
C. Adding a custom field to it
D. Putting it in a custom dashboard

**4.** An exclusion filter primarily affects:
A. Detection
B. Indexing volume / cost
C. Archives
D. RBAC

**5.** When configuring Amazon Data Firehose with Datadog as destination, the recommended buffer / encoding is:
A. 256 KiB / none
B. 2 MiB / GZIP
C. 5 MiB / Snappy
D. 16 MiB / ZSTD

**6.** Which AWS service ID is Datadog's account for `sts:AssumeRole` (commercial AWS)?
A. 464622532012
B. 065115117704
C. 123456789012
D. 987654321000

**7.** The AWS-managed policy attached to the Datadog AWS integration role is:
A. AdministratorAccess · B. SecurityAudit · C. ReadOnlyAccess · D. AWSConfigUserAccess

**8.** Best AWS log delivery method for **multi-account, high-volume**, AWS-native:
A. Lambda Forwarder
B. Amazon Data Firehose with Datadog destination
C. Manual upload via UI
D. SNS

**9.** Best AWS log delivery method for fan-out to Datadog and another consumer:
A. Lambda Forwarder
B. Direct SNS to Datadog
C. KDS → Firehose → Datadog
D. CodeDeploy

**10.** The CLI command to subscribe a CloudWatch Log Group to a destination:
A. `aws cloudwatch put-metric-alarm`
B. `aws logs put-subscription-filter`
C. `aws iam attach-role-policy`
D. `aws ssm send-command`

**11.** The OCSF Processor should be placed:
A. First in the pipeline
B. After parsing/enrichment, last in the pipeline
C. Anywhere
D. In a separate pipeline

**12.** Reserved attributes that cannot be set by the generic Attribute Remapper include:
A. `host`, `message`, `service`, `status`, `date`, `trace_id`, `span_id`
B. `usr`, `network`, `evt`
C. `tags` only
D. None

**13.** Grok Parser configuration requires:
A. `match_rules` (and optionally `support_rules`)
B. Just a regex
C. Just `samples`
D. Nothing

**14.** When updating an existing integration pipeline by adding an Attribute Remapper, you should typically:
A. Enable Preserve Source Attribute
B. Disable Preserve Source Attribute (avoid duplicate values)
C. Disable other processors
D. Move it to position 0

**15.** The API endpoint for creating a Cloud SIEM Security Filter is:
A. `POST /api/v2/security_monitoring/configuration/security_filters`
B. `GET /api/v1/security_filters`
C. `PUT /api/v1/log_filters`
D. `DELETE /api/v2/security_filters`

**16.** Required headers for the Security Filter API:
A. `Authorization: Bearer …`
B. `DD-API-KEY` and `DD-APPLICATION-KEY`
C. `X-Datadog-Service`
D. `DD-Site` only

**17.** A Security Filter's `filtered_data_type` for log-based detection is:
A. `metrics` · B. `traces` · C. `logs` · D. `signals`

**18.** When Cloud SIEM is enabled but detection rules don't fire, a likely cause is:
A. The Security Filter excludes the relevant log source
B. The Datadog Agent version is old
C. AWS Lambda concurrency is low
D. Browser cache

**19.** A best practice when first enabling Cloud SIEM is to:
A. Enable all custom rules immediately
B. Activate Content Packs that match the log sources you actually ingest
C. Disable Bits AI
D. Move all logs to Archive

**20.** Sensitive Data Scanner runs against logs:
A. After indexing
B. Before indexing for redaction/hashing
C. Only on archives
D. Never

---

## Answer key
1-B 2-C 3-B 4-B 5-B 6-A 7-B 8-B 9-C 10-B 11-B 12-A 13-A 14-B 15-A 16-B 17-C 18-A 19-B 20-B
