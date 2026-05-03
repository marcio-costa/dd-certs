# Mock Exam #2 — Datadog Fundamentals (75 Qs / 90 min)

> Take this in one sitting at the end of Week 4. This exam is intentionally a bit harder than Mock #1.
> Target: ≥ 80%. If you score that, you are exam-ready.

---

### Domain 1 — Computer Fundamentals (8 Qs)

**1.** Which AWS service most closely matches IaaS? A) Lambda  B) EC2  C) Elastic Beanstalk  D) RDS.

**2.** A pod typically: A) holds exactly one container  B) holds one or more containers sharing network/storage  C) replaces a node  D) holds an entire cluster.

**3.** Which is NOT a Datadog regional site? A) US1  B) US3  C) AP1  D) APAC2.

**4.** A `kubectl get daemonset -A` command on a fresh Datadog Helm install will show: A) zero DaemonSets  B) the Datadog Agent DaemonSet  C) only the Cluster Agent  D) only kube-proxy.

**5.** Cloud metadata service IP for AWS: A) `169.254.169.254`  B) `127.0.0.1`  C) `10.0.0.1`  D) `100.64.0.1`.

**6.** Outbound port the Agent uses: A) 80  B) 443  C) 22  D) 53.

**7.** Which AWS log destination is consumed by Datadog Forwarder Lambda? A) S3 + CloudWatch Logs  B) DynamoDB  C) Kinesis Streams only  D) RDS audit logs.

**8.** Containers share what with the host? A) IP address  B) the OS kernel  C) memory pages by default  D) nothing.

---

### Domain 2 — Infrastructure Deployment (12 Qs)

**9.** A team uses GCP and wants data in a GCP-hosted Datadog region. They should pick: A) US1  B) US5  C) EU1  D) US1-FED.

**10.** A team needs to run the Agent on Linux but with a non-default site `datadoghq.eu`. They should set: A) `DD_SITE=datadoghq.eu`  B) `DD_REGION=eu`  C) `DATADOG_SITE=eu1`  D) `DD_LOCATION=eu`.

**11.** The Datadog Agent v7 supports: A) Python 2 only  B) Python 3 only  C) Python 2 and 3  D) No Python support.

**12.** The minimum K8s privileges the Datadog Operator needs are typically delivered by: A) ClusterRole + ClusterRoleBinding  B) PodSecurityPolicy only  C) Anonymous auth  D) NodePort.

**13.** When using the Datadog Operator, the user creates a: A) `DatadogAgent` CR  B) `Helm` CR  C) `Kustomization` CR  D) `Secret` only.

**14.** A team in the air-gapped US Federal cloud should pick: A) US1  B) US3  C) US1-FED  D) AP1.

**15.** App keys can be created by: A) the Datadog Admin only  B) any user (and assigned to the user); also can be associated with service accounts  C) only via API  D) only via Terraform.

**16.** When you rotate an API key, the Agent will fail intake when: A) the new key is created  B) the old key is deleted  C) on Agent restart only  D) never — both keys remain valid.

**17.** Two install patterns for the Agent on K8s (select TWO): A) Helm chart  B) Datadog Operator  C) Bash script per node manually  D) BOSH release.

**18.** Which command exposes the Helm chart's default values? A) `helm show values datadog/datadog`  B) `helm dump datadog/datadog`  C) `helm cat datadog/datadog`  D) `helm read datadog/datadog`.

**19.** Lambda Extension is deployed as: A) a Lambda layer added to the function  B) a sidecar container  C) a separate Lambda  D) a CloudWatch destination.

**20.** Which is true about hostname assignment? A) Hostname is auto-detected from cloud metadata; explicit `hostname:` overrides it  B) Hostname must be set manually always  C) Hostname is randomized per restart  D) Hostname comes only from `/etc/hostname`.

---

### Domain 3 — Networking & Agent Configuration (12 Qs)

**21.** Trace agent local port: A) 8125 UDP  B) 8126 TCP  C) 9090  D) 5044.

**22.** A common reason logs work but APM doesn't: A) `apm_config.enabled` not true OR the app isn't pointing at port 8126  B) wrong API key  C) wrong site  D) Cluster Agent missing.

**23.** Autodiscovery template variable for env var inside container: A) `%%env_<NAME>%%`  B) `${VAR}`  C) `<<env:VAR>>`  D) `%%var_NAME%%`.

**24.** Pod annotation v2 key prefix: A) `ad.datadoghq.com/<container_name>.checks`  B) `dd.tags/<container>.checks`  C) `datadog.io/<container>.checks`  D) `monitoring.datadoghq.com`.

**25.** The Auto-Conf yaml file path on Linux: A) `/etc/datadog-agent/conf.d/auto_conf.yaml`  B) `/etc/datadog/auto.yml`  C) `/var/lib/dd/autoconfig.yaml`  D) `/usr/share/datadog/auto_conf.yaml`.

**26.** A correct DogStatsD distribution datagram: A) `latency:235|d|#endpoint:/api`  B) `latency:235|h|#endpoint:/api`  C) `latency:235|c|@endpoint=/api`  D) `latency=235|d|tags=endpoint:/api`.

**27.** Which custom check method submits a histogram sample? A) `self.histogram(name, value, tags=[...])`  B) `self.h(name, value)`  C) `self.gauge(name, value, type='hist')`  D) `self.set(name, value)`.

**28.** Which is the first thing to check when a metric integration shows "Errors" in `agent status`? A) `agent configcheck` for that integration's config  B) Network ports  C) Agent version  D) Restart the Cluster Agent.

**29.** A check that runs every 60 seconds is configured with: A) `min_collection_interval: 60`  B) `frequency: 60`  C) `interval_seconds: 60`  D) `cron: "* * * * *"`.

**30.** Network monitoring (NPM) requires: A) System Probe  B) Cluster Agent  C) Extra Helm chart  D) None.

**31.** A Custom Resource named `DatadogAgent` is provided by: A) Datadog Operator  B) Helm chart  C) Cluster Agent  D) APM.

**32.** Which proxy env var prefix is the Agent's specific one (vs. POSIX standard)? A) `DD_PROXY_*`  B) `DATADOG_PROXY_*`  C) `AGENT_PROXY_*`  D) `DDOG_PROXY_*`.

---

### Domain 4 — Data Collection (18 Qs)

**33.** Which is the correct full set of system check states? A) OK, WARNING, CRITICAL, UNKNOWN  B) OK, FAIL  C) UP, DOWN  D) HEALTHY, UNHEALTHY.

**34.** Tagging in `datadog.yaml` `tags:` applies to: A) all metrics from that Agent  B) only system checks  C) only Logs  D) only DogStatsD metrics.

**35.** A `set` metric reports: A) the count of unique values in the interval  B) the cardinality of all tags  C) all values in a list  D) sum of values.

**36.** USTagging tags appear in (select THREE): A) APM traces  B) Logs (when properly configured)  C) Infrastructure metrics  D) AWS billing.

**37.** Custom metrics overage is billed per: A) 100 metric contexts  B) 1,000 hosts  C) 10,000 events  D) 1 metric per dollar.

**38.** Which is best for low-cardinality, low-frequency aggregations from log data? A) Custom metric  B) Logs-to-metrics  C) DogStatsD distribution  D) Synthetic check.

**39.** Distinct value of unique users in a window — best metric type: A) gauge  B) histogram  C) distribution  D) set.

**40.** A team adds the tag `customer_email:user@example.com` to a metric. Result: A) cardinality blow-up; high custom-metric bill  B) Datadog auto-aggregates  C) Datadog ignores the tag  D) Tag rejected as invalid.

**41.** A monotonic_count submission with values [10, 5, 12]: A) emits diffs ignoring resets to zero (so contributions: 10, then -5 reset ignored, then 12-5 = 7? actually 5 to 12 is 7) — practically: handles counters that reset  B) errors  C) sums all values  D) treats each as gauge.

**42.** Which submission-time call corresponds to a Datadog **rate** metric? A) `self.rate(name, value, ...)`  B) `self.gauge`  C) `self.count`  D) `self.histogram`.

**43.** A high-frequency app sends 50,000 DogStatsD packets/sec; some are missing. The likely fix (select TWO): A) increase `dogstatsd_buffer_size`  B) switch to UDS (Unix Domain Socket)  C) restart the host  D) lower DogStatsD port.

**44.** Which is NOT auto-applied as a tag by the Agent in K8s? A) `kube_namespace`  B) `pod_name`  C) `kube_deployment`  D) `customer_id`.

**45.** Which API endpoint accepts metric series submissions? A) `/api/v2/series`  B) `/api/v1/metrics`  C) `/api/v3/dogstatsd`  D) `/api/v2/host`.

**46.** Which integration includes a service check `postgres.can_connect`? A) Postgres  B) MySQL  C) MongoDB  D) Redis.

**47.** A `count`-typed metric query without `.as_count()` is interpreted as: A) per-second rate  B) absolute count  C) cumulative since Agent start  D) histogram.

**48.** AWS resource tags become Datadog tags when: A) the AWS integration is set up and tag collection is enabled  B) you manually paste them  C) Lambda Extension is installed  D) you set `DD_AWS=true`.

**49.** Origin detection in DogStatsD requires (select TWO): A) UDS or specific config flags  B) `apm_config.enabled`  C) Agent in K8s with proper mounts  D) Cluster Agent.

**50.** Tags case-handling: A) keys lowercase, values case-sensitive but case-insensitive in queries  B) all uppercase  C) all case-sensitive  D) random.

---

### Domain 5 — Troubleshooting (10 Qs)

**51.** Best command for "is the Agent talking to Datadog correctly?": A) `agent diagnose datadog-connectivity`  B) `agent ping`  C) `agent verify`  D) `dd-host check`.

**52.** Where do APM-trace agent logs go on Linux? A) `/var/log/datadog/trace-agent.log`  B) `/var/log/datadog/agent.log`  C) `/var/log/dd-trace.log`  D) `/var/log/datadog/process-agent.log`.

**53.** Lowering the log level temporarily (Agent ≥ 7.34): A) `datadog-agent config set log_level debug`  B) Edit YAML and restart only  C) Send SIGUSR1  D) Not possible.

**54.** A flare's contents are: A) Agent logs and config (with secrets scrubbed) and runtime state  B) Datadog backend logs  C) System wide logs  D) The host's filesystem.

**55.** A duplicate host in Infrastructure list often points to: A) hostname conflict (explicit override vs cloud detection, or rebuilt instance)  B) Datadog bug  C) DogStatsD overflow  D) Cluster Agent issue.

**56.** Which integration tag wouldn't typically be auto-set on a Postgres metric? A) `db`  B) `host`  C) `team` (custom)  D) `port`.

**57.** A symptom of `dogstatsd_packet_drops_in_total` increasing: A) UDP buffer overflow  B) too many APM traces  C) wrong TLS cert  D) Cluster Agent down.

**58.** `agent check <name> --check-rate` is useful to: A) See the rate-derived value of a count metric (run two collections back-to-back)  B) Run faster  C) Rate-limit the check  D) Show the integration's frequency.

**59.** A Windows Agent is not appearing in Infrastructure. Likely first check: A) Service `DatadogAgent` running and `agent status` output  B) Reinstall  C) Check Active Directory  D) Reboot.

**60.** The local Agent GUI is only available on: A) the loopback interface (127.0.0.1)  B) any interface  C) port 80  D) the Cluster Agent.

---

### Domain 6 — Visualization & Monitors (15 Qs)

**61.** The widget that lists top N items by a metric value: A) Top List  B) Heat Map  C) Service Map  D) Note.

**62.** Which two are layout options for modern dashboards? A) Ordered + Free  B) Tile + List  C) Vertical + Horizontal  D) Card + Stack.

**63.** Anomaly monitor algorithm options include (select TWO): A) `basic`  B) `agile`  C) `robust`  D) `extreme`.

**64.** Outlier monitor algorithm `DBSCAN` is for: A) clustering peer groups to identify outliers  B) DNS-based scanning  C) database schema analysis  D) data backup.

**65.** Multi-alert on `by {host, env}` will: A) alert per (host, env) combo  B) alert only when both differ  C) alert globally  D) error.

**66.** A composite monitor with `(101 && 102)`: A) alerts when monitor IDs 101 and 102 are both alerting  B) sums their values  C) requires monitor 101 to escalate to 102  D) creates two new monitors.

**67.** `mute_first_recovery_notification` purpose: A) suppress noisy first recovery after downtime ends  B) silence all recoveries  C) silence the first alert  D) silence email only.

**68.** Time-slice SLO measures: A) fraction of intervals meeting a condition  B) total seconds of downtime  C) number of users affected  D) cost.

**69.** An error budget over `30d` of 99.9% target equals approximately: A) 43 minutes  B) 4.3 hours  C) 7 days  D) 1 hour.

**70.** A `change()` monitor compares: A) current value vs. a value from a prior window  B) two metrics  C) two services  D) two hosts.

**71.** Which is the right way to alert when a value drops below threshold for the entire 5 minutes? A) `min(last_5m): metric < 10`  B) `max(last_5m): metric < 10`  C) `last(last_5m): metric < 10`  D) `change(): metric < 10`.

**72.** To page PagerDuty service "infra": A) `@pagerduty-infra`  B) `@pd:infra`  C) `@pagerduty/infra`  D) `@pager:infra`.

**73.** Notebook supports Mermaid in: A) Markdown cell  B) Timeseries cell  C) Trace cell  D) Heatmap cell.

**74.** A scheduled report from a dashboard sends: A) PDF snapshots over email at intervals  B) live link  C) Slack DMs  D) raw JSON.

**75.** Best widget for a single SLO's status: A) SLO Summary  B) Top List  C) Hostmap  D) Note.

---

# Mock Exam #2 — Answer Key

| Q | Ans | | Q | Ans | | Q | Ans |
|---|-----|---|---|-----|---|---|-----|
| 1 | B | | 26 | A | | 51 | A |
| 2 | B | | 27 | A | | 52 | A |
| 3 | D | | 28 | A | | 53 | A |
| 4 | B | | 29 | A | | 54 | A |
| 5 | A | | 30 | A | | 55 | A |
| 6 | B | | 31 | A | | 56 | C |
| 7 | A | | 32 | A | | 57 | A |
| 8 | B | | 33 | A | | 58 | A |
| 9 | B (US5 = GCP) | | 34 | A | | 59 | A |
| 10 | A | | 35 | A | | 60 | A |
| 11 | B | | 36 | A,B,C | | 61 | A |
| 12 | A | | 37 | A | | 62 | A |
| 13 | A | | 38 | B | | 63 | A,C |
| 14 | C | | 39 | D | | 64 | A |
| 15 | B | | 40 | A | | 65 | A |
| 16 | B | | 41 | A | | 66 | A |
| 17 | A,B | | 42 | A | | 67 | A |
| 18 | A | | 43 | A,B | | 68 | A |
| 19 | A | | 44 | D | | 69 | A |
| 20 | A | | 45 | A | | 70 | A |
| 21 | B | | 46 | A | | 71 | A |
| 22 | A | | 47 | A | | 72 | A |
| 23 | A | | 48 | A | | 73 | A |
| 24 | A | | 49 | A,C | | 74 | A |
| 25 | A | | 50 | A | | 75 | A |

## Per-domain score sheet

| Domain | Question range | Your score | / Total |
|---|---|---|---|
| 1. Computer Fundamentals | 1–8 | __ | / 8 |
| 2. Infrastructure Deployment | 9–20 | __ | / 12 |
| 3. Networking & Agent Config | 21–32 | __ | / 12 |
| 4. Data Collection | 33–50 | __ | / 18 |
| 5. Troubleshooting | 51–60 | __ | / 10 |
| 6. Visualization & Monitors | 61–75 | __ | / 15 |
| **TOTAL** | | **__** | **/ 75** |

> Pass benchmark: 70%. Strong: 80%+. If you scored ≥ 80%, you are exam-ready. If 70–79%, do one more pass of `reference/common-pitfalls.md` and your weakest domain. If < 70%, postpone the exam by a week and run the Weak-Area Drill twice.
