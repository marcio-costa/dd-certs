# Mock Exam #1 — Datadog Fundamentals (75 Qs / 90 min)

> Take this in one sitting. Use a timer. Do not consult notes.
> When done, score with the answer key at the bottom and update `progress-tracking/results.md`.

Target: ≥ 70%. If < 70%, do not take Mock #2 yet — drill weak areas first.

Scoring: 1 point per correct answer. Multi-select: all correct + no incorrect = 1 point.

---

### Domain 1 — Computer Fundamentals (8 Qs)

**1.** A pod and a container are: A) the same thing  B) a pod runs containers  C) a container runs pods  D) unrelated.

**2.** Which AWS service is FaaS? A) EC2  B) RDS  C) Lambda  D) S3.

**3.** All Datadog Agent → SaaS traffic goes over: A) UDP 8125  B) TCP 8126  C) HTTPS 443 outbound  D) inbound 443.

**4.** A K8s DaemonSet runs: A) one replica per cluster  B) one pod per node  C) one job per build  D) one container per pod.

**5.** TLS termination at a corporate proxy can break the Agent unless: A) you switch to plain HTTP  B) the corporate CA is trusted by the OS  C) Agent is run as root  D) APM is disabled.

**6.** Which is NOT a Datadog site? A) US1  B) US2  C) US5  D) AP1.

**7.** Which port does APM/trace ingest use locally? A) 8125 UDP  B) 8126 TCP  C) 9090 TCP  D) 5044 TCP.

**8.** A SaaS app's user does not need to: A) write code  B) install the OS  C) provide credentials  D) trust the vendor.

---

### Domain 2 — Infrastructure Deployment (12 Qs)

**9.** The Agent needs which credential? A) API key  B) App key  C) Both  D) SSO.

**10.** Default Linux config path: A) `/etc/datadog/agent.conf`  B) `/etc/datadog-agent/datadog.yaml`  C) `/var/lib/datadog/conf.yaml`  D) `~/datadog.yaml`.

**11.** EU site value: A) `eu1.datadog.com`  B) `datadoghq.eu`  C) `datadog.eu`  D) `eu.datadoghq.com`.

**12.** Helm chart name: A) `datadog/datadog`  B) `datadog/agent`  C) `dd/agent`  D) `datadog-helm/agent`.

**13.** "403 Forbidden" in `agent status` Forwarder section commonly means: A) bad API key or wrong site  B) DogStatsD overflow  C) Trace agent down  D) DNS misconfig.

**14.** App keys are tied to: A) the org  B) a user or service account  C) the Cluster Agent  D) integrations.

**15.** The Datadog Operator is: A) a person at Datadog  B) a CRD-based K8s installer  C) a CLI-only tool  D) the Cluster Agent.

**16.** Which install method is most common on a managed K8s cluster? A) Helm chart or Operator  B) Direct kubectl apply  C) Source build  D) Lambda extension.

**17.** When installing the Agent on a Linux VM via the install script, you must set: A) `DD_API_KEY` (always) and `DD_SITE` (if not US1)  B) only `DD_USERNAME`  C) `DD_API_KEY` and `DD_APP_KEY`  D) `DD_AGENT_HOST`.

**18.** AWS integration via IAM role pulls in: A) per-host process metrics  B) AWS service-level metrics + tags  C) Lambda extension data  D) Application traces.

**19.** Two components typically deployed by the Helm chart in K8s (select TWO): A) Node Agent  B) Cluster Agent  C) Datadog UI  D) Sidecar per pod.

**20.** Read-only access for a new auditor is fastest given by: A) Adding to the Read-Only role  B) Sharing API key  C) Inviting as Admin  D) Creating a custom role from scratch.

---

### Domain 3 — Networking & Agent Configuration (12 Qs)

**21.** DogStatsD port/protocol: A) UDP 8125  B) TCP 8125  C) UDP 8126  D) TCP 8126.

**22.** Which `datadog.yaml` block configures a forward proxy? A) `proxy_url:`  B) `proxy:` with `http`/`https`  C) `forward_proxy:`  D) `network:`.

**23.** Cloud metadata IP `169.254.169.254` should typically go in: A) `proxy.https`  B) `no_proxy`  C) `additional_endpoints`  D) `dd_url`.

**24.** Annotations v2 is recommended from Agent version: A) v6.4+  B) v7.0+  C) v7.36+  D) v8+.

**25.** `%%host%%` in Autodiscovery means: A) the K8s node IP  B) the discovered container IP  C) Datadog backend  D) localhost.

**26.** To list all check configurations and their source: A) `agent status`  B) `agent configcheck`  C) `agent diagnose`  D) `agent fetch`.

**27.** Logs work fine but APM doesn't. Most likely root cause: A) `apm_config.enabled` not true or app not pointing to 8126  B) Wrong API key  C) Wrong site  D) Cluster Agent missing.

**28.** A custom check needs: A) Python file in `checks.d` only  B) Python file in `checks.d` AND matching `conf.yaml` in `conf.d/<name>.d/`  C) Just a YAML in `conf.d`  D) A K8s annotation.

**29.** Default check interval: A) 5s  B) 15s  C) 30s  D) 60s.

**30.** A DogStatsD datagram with type and tags is: A) `metric:1|c|#tag:val`  B) `metric=1|count|tag=val`  C) `metric|1|c|tag:val`  D) `metric:1:c:tag=val`.

**31.** `service_check` numeric for CRITICAL: A) 0  B) 1  C) 2  D) 3.

**32.** Which Datadog Agent product needs `force_use_http: true` for proxy environments? A) Process  B) Logs  C) APM  D) Synthetics.

---

### Domain 4 — Data Collection (18 Qs)

**33.** USTagging tags: A) env, service, version  B) env, app, build  C) cluster, pod, namespace  D) team, region, env.

**34.** `gauge` reports: A) increments  B) last value  C) per-second rate  D) percentile.

**35.** `count` vs `rate`: A) count = per-interval; rate = per-second normalized  B) identical  C) count for ints, rate for floats  D) reverse of A.

**36.** Which is per-host vs. global aggregated? A) histogram per-host, distribution global  B) reverse  C) both global  D) both per-host.

**37.** High-cardinality tag value: A) `env:prod`  B) `service:cart`  C) `request_id:abc-123`  D) `region:eu`.

**38.** USTagging in Docker `ENV`: A) `DD_ENV / DD_SERVICE / DD_VERSION`  B) `ENV / SERVICE / VERSION`  C) `DATADOG_ENV / SERVICE / VERSION`  D) `TAGS=env:..,service:..`.

**39.** Free system metric example: A) `myapp.requests`  B) `system.cpu.user`  C) `payment.errors`  D) `checkout.duration`.

**40.** Service check states: A) OK/WARN/CRIT/UNKNOWN  B) UP/DOWN  C) OK/FAIL  D) GREEN/YELLOW/RED.

**41.** `monotonic_count` ignores: A) tags  B) resets to zero  C) the host  D) the time window.

**42.** Custom metric context = A) one metric per host per tag combo  B) per check  C) per Agent  D) per dashboard.

**43.** Best for global p95 across many hosts: A) histogram  B) distribution  C) gauge  D) set.

**44.** Where can tags be assigned (select FOUR)? A) `datadog.yaml` `tags:`  B) integration `conf.yaml`  C) container labels / pod annotations  D) DogStatsD `#`  E) DNS records.

**45.** Reserved key prefix: A) `host`  B) `weather`  C) `mood`  D) `gpu`.

**46.** Tag length limit: A) 50  B) 100  C) 200  D) 500.

**47.** ECS USTagging via: A) env vars only  B) Docker labels only  C) both env vars and Docker labels  D) IAM roles.

**48.** Which is NOT a way to ship metrics? A) DogStatsD  B) Agent integration check  C) HTTP API  D) SMTP.

**49.** Active host (billing) means reported within: A) last 5 min  B) last 30 min  C) last 2 hours  D) last 24 hours.

**50.** A K8s pod metric with `kube_namespace` tag is: A) auto-applied by the Agent  B) manually added in DogStatsD  C) sourced from CloudWatch  D) only on the Cluster Agent.

---

### Domain 5 — Troubleshooting (10 Qs)

**51.** First diagnostic command: A) `agent diagnose`  B) `agent status`  C) `agent flare`  D) `agent reload`.

**52.** Send to support: A) `agent flare <CASE_ID>`  B) `agent dump`  C) `agent send`  D) email logs.

**53.** Logs path on Linux: A) `/etc/datadog/logs/`  B) `/var/log/datadog/`  C) `/opt/datadog/logs/`  D) `/usr/local/datadog/`.

**54.** Connectivity test to Datadog: A) `agent diagnose datadog-connectivity`  B) `ping datadoghq.com`  C) `agent test`  D) `agent ack`.

**55.** Best monitor for "Agent down": A) `system.uptime`  B) `datadog.agent.running < 1`  C) `system.cpu.idle == 100`  D) `process.up`.

**56.** A custom check's Python is correct but it doesn't show in `agent status`: A) wrong site  B) missing `conf.yaml`  C) wrong API key  D) cluster agent not running.

**57.** Logs on Windows: A) `C:\Windows\datadog\logs\`  B) `C:\ProgramData\Datadog\logs\`  C) `C:\Datadog\`  D) `%APPDATA%\Datadog\`.

**58.** To see effective hostname: A) `agent hostname`  B) `hostname -s`  C) `agent which`  D) `dd-host`.

**59.** Forwarder dropping payloads suggests: A) network/proxy issue  B) bad metric type  C) wrong K8s namespace  D) Agent too old.

**60.** Container logs not collected: A) `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true` missing  B) `apm_enabled` false  C) `process_agent_enabled` false  D) wrong cluster role.

---

### Domain 6 — Visualization & Monitors (15 Qs)

**61.** Single big number widget: A) Top List  B) Query Value  C) Heatmap  D) Note.

**62.** Per-host hex grid: A) Hostmap  B) Heatmap  C) Service Map  D) Top List.

**63.** Forecast monitor predicts: A) future threshold crossing  B) past anomalies  C) outliers in a peer group  D) baseline deviation.

**64.** Anomaly monitor uses: A) baseline learned from history  B) absolute threshold  C) per-group comparison  D) future projection.

**65.** Outlier monitor: A) one member differing from peers  B) global threshold  C) baseline learned  D) future prediction.

**66.** Multi-alert is created by: A) `by {tag}` clause  B) `composite_query`  C) `forecast()`  D) downtime.

**67.** `as_count()` is needed when: A) the metric is a count and threshold is in absolute occurrences  B) for gauges  C) to lower noise  D) for distributions only.

**68.** Slack handle format: A) `@slack-acme-alerts`  B) `@acme:alerts`  C) `@slack:#alerts`  D) `@page-acme`.

**69.** Composite monitor combines via: A) `&&`/`||`/`!`  B) `+`/`-`  C) `AND`/`OR` (uppercase only)  D) regex.

**70.** Downtime recurrence uses: A) RRULE  B) crontab  C) SQL  D) JSON Schedule.

**71.** SLO time windows include: A) 7d, 30d, 90d  B) 1d, 7d, 28d  C) 24h, 7d, 60d  D) 1h, 1d, 365d.

**72.** Error budget alert syntax: A) `error_budget("<id>").over("<win>") > <pct>`  B) `slo("<id>") > <pct>`  C) `consumption("<id>", "<win>")`  D) `budget(<id>) burned`.

**73.** Time-slice SLO measures: A) fraction of evaluation intervals meeting a condition  B) numerator/denominator counts  C) time the monitor was OK  D) number of users affected.

**74.** A monitor with `min(last_5m): metric{*} > 90` alerts when: A) the metric was sustained > 90 the entire 5 min  B) it crossed 90 once  C) average > 90  D) max > 90.

**75.** Dashboard sharing options include (select TWO): A) public read-only link  B) embed widget iframe  C) email login credentials  D) sync to local file.

---

# Mock Exam #1 — Answer Key

| Q | Ans | | Q | Ans | | Q | Ans |
|---|-----|---|---|-----|---|---|-----|
| 1 | B | | 26 | B | | 51 | B |
| 2 | C | | 27 | A | | 52 | A |
| 3 | C | | 28 | B | | 53 | B |
| 4 | B | | 29 | B | | 54 | A |
| 5 | B | | 30 | A | | 55 | B |
| 6 | B (no US2) | | 31 | C | | 56 | B |
| 7 | B | | 32 | B | | 57 | B |
| 8 | B | | 33 | A | | 58 | A |
| 9 | A | | 34 | B | | 59 | A |
| 10 | B | | 35 | A | | 60 | A |
| 11 | B | | 36 | A | | 61 | B |
| 12 | A | | 37 | C | | 62 | A |
| 13 | A | | 38 | A | | 63 | A |
| 14 | B | | 39 | B | | 64 | A |
| 15 | B | | 40 | A | | 65 | A |
| 16 | A | | 41 | B | | 66 | A |
| 17 | A | | 42 | A | | 67 | A |
| 18 | B | | 43 | B | | 68 | A |
| 19 | A,B | | 44 | A,B,C,D | | 69 | A |
| 20 | A | | 45 | A | | 70 | A |
| 21 | A | | 46 | C | | 71 | A |
| 22 | B | | 47 | C | | 72 | A |
| 23 | B | | 48 | D | | 73 | A |
| 24 | C | | 49 | C | | 74 | A |
| 25 | B | | 50 | A | | 75 | A,B |

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

> Pass benchmark: 70% (≈53/75). Strong: 80% (60/75). Update `progress-tracking/results.md` and feed your weakest domain into the **Weak-Area Drill** workflow (see `simulations/weak-area-drill-instructions.md`).
