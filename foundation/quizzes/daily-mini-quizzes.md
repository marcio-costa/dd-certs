# Daily Mini-Quizzes (28 × 10 Qs)

> Each day in the study plan has a 10-question mini-quiz. Answers at the end of each quiz, separated by `<details>`.

---

## Day 1 — Orientation
1. Which port does the trace-agent use? **B. TCP 8126**
2. Datadog Agent's all-data-out outbound port? **B. 443**
3. The default Datadog site is: **A. US1 (`datadoghq.com`)**
4. A Pod is the smallest deployable unit in: **C. Kubernetes**
5. AWS Lambda is best classified as: **D. FaaS / Serverless**
6. Which command starts the Agent on systemd? **A. `systemctl start datadog-agent`**
7. Which subprocess handles custom metrics from your apps? **B. DogStatsD**
8. The Agent **opens** which inbound ports for Datadog? **A. None**
9. Which file ends in `.yaml` and holds the main Agent config? **C. `datadog.yaml`**
10. The "active" host count drives Datadog's: **A. Infrastructure billing**

---

## Day 2 — Agent Architecture
1. Which Agent component sends payloads to the backend? **C. Forwarder**
2. Which Agent runs Python integration checks? **A. Collector**
3. Trace-agent listens on port: **B. TCP 8126**
4. DogStatsD listens on port: **A. UDP 8125**
5. Agent 5 used what supervisor? **B. supervisord**
6. Agent 7 supports which Python? **B. Python 3 only**
7. Which file overrides Datadog config via env vars? **D. None — env vars override directly, no separate file**
8. The forwarder retries with: **A. Exponential back-off**
9. The Logs Agent ships logs over: **B. Compressed HTTPS or TCP**
10. Process-agent collects: **C. Live processes/containers (and optionally network)**

---

## Day 3 — Installation Patterns
1. The K8s install pattern is: **B. Helm chart or Datadog Operator**
2. The DaemonSet runs the Agent: **A. Once per node**
3. Docker Agent needs which mounts? **D. docker.sock + /proc + /sys/fs/cgroup**
4. The Lambda Extension is installed as a: **B. Lambda layer**
5. The one-line Linux install needs at minimum: **A. `DD_API_KEY` (and `DD_SITE` if not US1)**
6. AWS integration replaces the host Agent for: **C. AWS service-level metrics only**
7. The Cluster Agent is deployed via: **B. A separate Deployment**
8. ECS sidecar deployment uses: **A. A Datadog ECS task definition**
9. Helm chart name: **B. `datadog/datadog`**
10. Operator works via: **C. CRDs (`DatadogAgent` etc.)**

---

## Day 4 — Sites, Keys, Org Setup
1. The Agent needs which key? **A. API key**
2. Terraform automation needs which key? **B. App key**
3. Wrong site set on Agent → **B. Intake fails**
4. EU site URL: **A. `app.datadoghq.eu`**
5. Service accounts hold: **B. App keys for automation**
6. Default RBAC roles: **A. Admin, Standard, Read-only**
7. Site is set via: **C. `site:` in datadog.yaml or `DD_SITE` env**
8. US1-FED is for: **D. US Federal customers**
9. SCIM is for: **A. User provisioning from IdP**
10. Multi-org accounts are useful for: **B. Parent companies with isolated subsidiaries**

---

## Day 5 — Networking & Firewall
1. All Agent traffic is: **A. Outbound over HTTPS 443**
2. Logs intake hostname: **B. `agent-intake.logs.<site>`**
3. APM intake hostname: **A. `trace.agent.<site>`**
4. To bypass cloud metadata in proxy: add to **C. `no_proxy`**
5. `force_use_http: true` belongs in: **C. `logs_config:`**
6. Cluster Agent needs Datadog access? **A. Yes**
7. IP ranges are at: **B. `ip-ranges.datadoghq.com`**
8. MITM proxy breaks Agent unless: **B. Corporate CA is trusted by OS**
9. TLS port: **C. 443**
10. Inbound from Datadog needed? **A. No**

---

## Day 6 — Integrations & Custom Checks
1. Postgres integration config goes in: **B. `/etc/datadog-agent/conf.d/postgres.d/conf.yaml`**
2. Annotations v2 prefix: **A. `ad.datadoghq.com/<container>`**
3. `%%port%%` resolves to: **C. Discovered container port**
4. To run a check once: **A. `agent check <name>`**
5. Auto-Conf file: **B. `auto_conf.yaml` in `conf.d/`**
6. Custom check class inherits: **A. `AgentCheck`**
7. A custom check missing from `agent status` usually means: **B. Missing `conf.yaml`**
8. Default check interval: **A. 15 seconds**
9. Service check states: **C. OK / WARN / CRIT / UNKNOWN**
10. To verify Autodiscovery loaded: **A. `agent configcheck`**

---

## Day 8 — Metric Types
1. `gauge` reports: **A. Last value**
2. `count` reports: **B. Increments per interval**
3. `histogram` aggregates: **A. Per host**
4. `distribution` aggregates: **B. Globally on Datadog backend**
5. `monotonic_count` ignores: **A. Resets to zero**
6. Best for global p95 across many hosts: **B. Distribution**
7. `set` returns: **A. Count of unique values**
8. `as_count()` modifier returns: **A. Absolute totals over each interval**
9. `as_rate()` modifier returns: **B. Per-second rate**
10. Which is NOT a metric type? **D. `cluster`**

---

## Day 9 — Submission Methods
1. DogStatsD protocol: **A. UDP**
2. DogStatsD port: **B. 8125**
3. Agent check submits via: **C. AgentCheck instance methods**
4. API metric submission endpoint: **C. `/api/v2/series`**
5. Which is most reliable? **B. Direct API or Agent check (TCP)**
6. UDS for DogStatsD enables: **A. Origin detection and reliability**
7. Custom Python checks live in: **B. `/etc/datadog-agent/checks.d/`**
8. Per-check interval is configurable via: **A. `min_collection_interval`**
9. DogStatsD aggregation interval (default): **B. 10 seconds**
10. Direct API consumes: **A. The org's API rate limit**

---

## Day 10 — Tagging Foundations
1. Tag format: **A. key:value** (or just key)
2. Tag keys are: **A. Lowercased**
3. Maximum tag length: **B. 200 characters**
4. Tags are case-sensitive in storage but: **B. Case-insensitive in queries by default**
5. Reserved tag keys include: **D. host, device, source, service, version, env**
6. Tags assigned via cloud integration come from: **B. AWS/Azure/GCP resource tags**
7. Tags in DogStatsD use prefix: **A. `#`**
8. To tag every metric from a host: **A. `tags:` in `datadog.yaml`**
9. Conflicts resolved by: **A. Tags compose — all layers contribute**
10. Tag dot-namespacing applies to: **B. Metric names, not tags**

---

## Day 11 — USTagging
1. The three USTagging tags: **B. env, service, version**
2. K8s labels prefix: **B. `tags.datadoghq.com/`**
3. Java env vars: **A. DD_ENV, DD_SERVICE, DD_VERSION**
4. ECS supports USTagging via: **C. Both env vars and Docker labels**
5. USTagging enables: **A. Cross-product correlation (metrics, traces, logs, RUM)**
6. Setting only DD_SERVICE will: **B. Partially correlate**
7. The `version` tag is useful for: **A. Deploy markers and version comparison**
8. The `env` tag is useful for: **B. Filtering across prod/staging/dev**
9. The `service` tag is the unit of: **A. Service Catalog**
10. USTagging is enforced by: **B. Convention; nothing rejects missing tags**

---

## Day 12 — Cardinality & Cost
1. Custom metric context = **A. Unique metric+host+tag-values combination**
2. Free with Agent: **A. System metrics** (`system.*`)
3. High-cardinality tag candidate: **C. user_id**
4. Best alternative for high-cardinality dimensions: **B. Logs and APM trace tags**
5. Distributions help cardinality because: **A. They aggregate globally, not per-host**
6. Metrics summary helps to find: **B. High-cardinality contexts**
7. Custom-metric overage is billed per: **B. 100 metric contexts**
8. `system.cpu.user` is: **B. Free system metric**
9. Tagging with timestamps creates: **A. Unbounded cardinality**
10. Best practice: **A. Standardize a small, finite tag taxonomy**

---

## Day 13 — Events, Service Checks, Host Map
1. Service check OK code: **A. 0**
2. Service check WARN code: **B. 1**
3. Service check CRITICAL code: **C. 2**
4. Service check UNKNOWN code: **D. 3**
5. Events Explorer shows: **A. Discrete timestamped messages**
6. Events can be overlaid on: **A. Dashboard graphs**
7. Host map widget displays: **B. Hex grid of hosts colored by metric**
8. Active hosts are those reporting in: **A. ~Last 2 hours**
9. `_e{...}` is the syntax for: **A. DogStatsD event**
10. `_sc|name|status|...` is: **A. DogStatsD service check**

---

## Day 15 — Dashboards
1. Single-number widget: **B. Query Value**
2. Top N: **A. Top List**
3. Per-host hex grid: **C. Hostmap**
4. Live logs in dashboard: **D. Log Stream**
5. Template variable prefix in queries: **B. `$`**
6. Modern dashboards have which two layouts? **A. Ordered and Free**
7. To embed a single widget: **A. Get an iframe URL**
8. Public link is: **A. Read-only and bypasses auth**
9. Notebook cells include: **C. Markdown, Timeseries, Log Stream, Trace List**
10. Mermaid diagrams render in: **B. Markdown cells of notebooks**

---

## Day 16 — Notebooks
1. Notebook best for: **A. Investigations and runbooks**
2. Mermaid is supported in: **B. Markdown cells**
3. Notebook cells re-run: **A. On reload**
4. You can import a graph from: **A. A dashboard**
5. Notebooks support: **A. Logs, traces, metrics**
6. Notebook output: **A. Persistent and shareable**
7. Live tail in notebooks: **A. Yes via Log Stream**
8. Editing a notebook affects the source dashboard? **A. No**
9. Notebook permissions: **A. Inherit role-based access**
10. Comments on cells: **A. Supported**

---

## Day 17 — Monitors I
1. Threshold for "alert" state: **A. Critical threshold**
2. Optional softer state: **B. Warning**
3. Default eval window: **A. Configurable per monitor (e.g., last_5m)**
4. `min()` aggregator means: **A. Threshold sustained throughout window**
5. `max()` aggregator means: **B. Threshold crossed at any point in window**
6. Recovery threshold creates: **A. Hysteresis to prevent flapping**
7. To send to PagerDuty: **A. `@pagerduty` handle**
8. To send to a Datadog user: **A. `@username`**
9. To create per-host alerts: **B. Add `by {host}`**
10. Composite syntax: **A. References monitor IDs with `&&`/`||`/`!`**

---

## Day 18 — Monitors II
1. Multi-alert is created by: **A. `by {tag}` clause**
2. New groups: **A. Get one full window before evaluation**
3. `new_group_delay` purpose: **A. Avoid spam from new groups**
4. Composite monitors: **A. Combine other monitors via boolean ops**
5. `notify_audit`: **A. Alert on monitor edits**
6. Renotify interval: **A. Page again after N min if still alerting**
7. Downtime scope: **A. Filters which groups are muted**
8. Downtime monitor_identifier: **A. Selects which monitors are muted**
9. Mute first recovery: **A. Suppresses post-downtime recovery noise**
10. Downtimes can recur via: **A. RRULE**

---

## Day 19 — SLOs & Error Budgets
1. Metric-based SLO needs: **A. Numerator and denominator queries**
2. Monitor-based SLO uses: **B. Time the monitor was OK**
3. Time-slice SLO uses: **A. Fraction of intervals meeting a condition**
4. SLO target example: **A. 99.9%**
5. Error budget: **A. Complement of the SLO target**
6. 99.9% over 30d budget ≈ **A. 43 minutes**
7. Error budget alert: **A. `error_budget("<id>").over("<window>") > <pct>`**
8. SLO time windows include: **A. 7d, 30d, 90d**
9. SLO summary widget: **A. Yes**
10. SLOs reside under: **A. Service Mgmt → SLOs**

---

## Day 20 — Troubleshooting
1. First diagnostic command: **A. `agent status`**
2. Send to support: **B. `agent flare <CASE_ID>`**
3. List loaded check configs: **A. `agent configcheck`**
4. Run one check now: **A. `agent check <name>`**
5. Connectivity test: **A. `agent diagnose datadog-connectivity`**
6. Linux logs at: **B. `/var/log/datadog/`**
7. Windows logs at: **A. `C:\ProgramData\Datadog\logs\`**
8. To watch the Agent itself: **A. Monitor `datadog.agent.running` `< 1`**
9. To raise log verbosity: **A. `log_level: debug` in `datadog.yaml`**
10. Web GUI on Linux at: **A. `http://127.0.0.1:5002`**

---

## Day 24 — Cross-domain Day
1. End-to-end signal pipeline minimum: **A. Agent → DogStatsD → Forwarder → Datadog backend**
2. Tag at the most specific layer to: **A. Override broader-layer values**
3. To get APM in containers: **A. Set `apm_config.enabled: true` and `DD_AGENT_HOST` for app**
4. SLO + monitor + downtime + dashboard combined creates: **A. A complete production observability footprint**
5. Best widget to compare two services side-by-side: **A. Timeseries with two queries**
6. To send a Slack message on an alert: **C. `@slack-<workspace>-<channel>` handle**
7. To restrict who edits a monitor: **A. Use restrictions / RBAC**
8. To know if your Agent is sending: **A. Check `agent status` Forwarder section**
9. To verify USTagging: **A. Open Service Catalog and look for service entry**
10. To diagnose log gaps: **A. Check `logs_enabled`, container collect-all, and intake hostname**

---

> When you're done with Week 4, all of these become rapid-fire warm-ups. Aim for 9/10 on each before exam day.
