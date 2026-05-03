# RAG Knowledge Base — Index

This is the searchable index for everything in the `rag-knowledge-base/` folder. Use it as the table of contents and as the lookup map when you (or an LLM) want to retrieve a specific topic.

> **How to use as RAG:** point your retriever at the `.md` files below. Each file is self-contained (~5–10 KB) and uses semantic headings (`##`, `###`) so you can chunk on heading boundaries without losing context. Use the `Topics covered` and `Related exam questions` sections below for keyword-style retrieval.

---

## Domain map → folders

| Exam Domain | Folder | Files |
|---|---|---|
| 1. Computer Fundamentals | `01-computer-fundamentals/` | OS, cloud models, containers, K8s, networking |
| 2. Infrastructure Deployment with Datadog | `02-infrastructure-agent/` | Agent architecture, install, sites & keys |
| 3. Networking and Agent Configuration | `03-networking-config/` | Firewall/proxies, integrations, DogStatsD |
| 4. Data Collection | `04-data-collection/` | Metric types, tagging, events/checks/host map |
| 5. Troubleshooting the Datadog Agent | `05-agent-troubleshooting/` | Status, flare, logs, common symptoms |
| 6. Data Visualization and Utilization | `06-visualization-monitors/` | Dashboards, widgets, monitors, SLOs, notebooks |
| Reference (cross-domain) | `reference/` | Quick reference card, common pitfalls |

---

## File-level index

### `01-computer-fundamentals/01-computer-fundamentals.md`
- **Topics covered:** processes, file systems, TCP/IP, ports, DNS, IaaS/PaaS/SaaS/FaaS, Docker basics, Kubernetes objects (pod, node, daemonset, deployment, namespace), outbound port 443, DogStatsD/APM ports, regional sites, cluster agent.
- **Related exam questions:** "Which K8s object deploys the Agent on every node?", "Which model is responsible for OS patching?", "Which port is DogStatsD on?", "Why does the site choice matter?"

### `02-infrastructure-agent/01-agent-architecture.md`
- **Topics covered:** Agent 7 vs 6 vs 5, Collector, Forwarder, DogStatsD server, Trace Agent, Logs Agent, Process Agent, System Probe, datadog.yaml, env-var overrides, autodiscovery surfaces, lifecycle commands (start/status/flare/configcheck), data flow diagram.
- **Related exam questions:** "What component receives metrics from your apps?", "Which file is the main Agent config?", "What format are env-var overrides?", "Does Datadog open inbound ports?"

### `02-infrastructure-agent/02-installation-platforms.md`
- **Topics covered:** install scripts (Linux/Windows/macOS), Docker run with socket+proc+cgroup mounts, K8s Helm chart, Datadog Operator, ECS sidecar, Lambda extension, AWS/Azure/GCP cloud integrations vs. Agent, picking the right install method.
- **Related exam questions:** "How do you deploy on K8s?", "What mounts does the Docker Agent need?", "Do you still need an Agent if you have the AWS integration?", "How do you instrument Lambda?"

### `02-infrastructure-agent/03-keys-sites-org.md`
- **Topics covered:** API key vs. App key (purpose, scope, rotation), six Datadog sites with URLs, RBAC roles (Admin/Standard/Read-only), custom roles, service accounts, SSO/SCIM.
- **Related exam questions:** "What does the Agent need to send data?", "Difference between API key and App key?", "Where do you set the site?", "What is a service account used for?"

### `03-networking-config/01-networking-firewall.md`
- **Topics covered:** outbound endpoints by feature, regional hostname patterns, local listening ports, proxy block in datadog.yaml, env vars (`DD_PROXY_*`, `HTTPS_PROXY`), no_proxy, MITM/TLS interception, IP-ranges JSON, Cluster Agent connectivity.
- **Related exam questions:** "Which port does the trace agent listen on?", "How do you configure a proxy?", "Why might logs fail when metrics work?"

### `03-networking-config/02-integrations-checks.md`
- **Topics covered:** integration concept, conf.d structure, autodiscovery (annotations v2, Docker labels, files, Auto-Conf), template variables, configcheck, custom Python checks, AgentCheck submission methods (gauge/count/rate/histogram/distribution/monotonic_count/set/event/service_check), comparison: DogStatsD vs. Agent check vs. API.
- **Related exam questions:** "How do you enable Postgres integration on a host?", "Where do K8s annotations go?", "Where does Auto-Conf live?", "Difference between gauge and count?"

### `03-networking-config/03-dogstatsd.md`
- **Topics covered:** datagram format, type codes (g/c/h/d/s/ms), shell-only submission, Python socket example, official `datadog` library, events and service checks via DogStatsD, configuration (`use_dogstatsd`, port, UDS), origin detection in K8s, UDP loss tradeoffs, histogram vs. distribution.
- **Related exam questions:** "What protocol/port is DogStatsD?", "How do you tag a DogStatsD metric?", "What does origin detection do?", "How do you fix dropped DogStatsD packets?"

### `04-data-collection/01-metrics-types.md`
- **Topics covered:** all submission types and their in-app type, when to use each, three submission paths (DogStatsD/Agent check/API), metric anatomy (name, value, ts, tags, host, type, unit), custom-metric cardinality and billing, system metrics list (cpu/mem/load/disk/net/processes), events vs. service checks vs. metrics summary.
- **Related exam questions:** "Difference between count and rate?", "When to use distribution over histogram?", "Why is `user_id` a bad tag?", "Are system metrics counted as custom?"

### `04-data-collection/02-tagging-best-practices.md`
- **Topics covered:** tag rules (chars, length, reserved keys), all places tags can be set, Unified Service Tagging (env/service/version) for hosts/Docker/K8s/ECS, recommended additional keys (team, region, AZ, cluster, namespace, cost_center), cardinality pitfalls, exam Q&A.
- **Related exam questions:** "Three tags in USTagging?", "Why use USTagging?", "Where can tags be assigned?", "Difference between tag keys and values?"

### `04-data-collection/03-events-service-checks-host-map.md`
- **Topics covered:** events (sources, anatomy, where they appear), service checks (states OK/WARN/CRIT/UNKNOWN, examples), Host Map widget, Infrastructure List, active vs. up hosts, Live Containers, mental model comparing data types.
- **Related exam questions:** "What's the value of state CRITICAL?", "What is a service check vs. a metric?", "What does the host map visualize?"

### `05-agent-troubleshooting/01-troubleshooting-toolkit.md`
- **Topics covered:** `agent status` deep dive, log file locations per platform, `log_level` toggling, `flare`, configcheck, check, diagnose, hostname, secret, GUI on 5002, Agent self-telemetry (`datadog.agent.running`).
- **Related exam questions:** "How do you send logs to support?", "Where are Agent logs on Linux?", "How do you run a check once?", "How do you alert on Agent down?"

### `06-visualization-monitors/01-dashboards.md`
- **Topics covered:** Timeboard vs. Screenboard vs. modern Dashboard with layout toggle, widget catalog, template variables, query syntax (`avg`, `by`, `rollup`, modifiers), sharing (link, embed, scheduled report), Notebooks (cells, Mermaid, importing graphs).
- **Related exam questions:** "Best widget for a single number?", "How do you parameterize a dashboard?", "What is the difference between timeboard and screenboard?"

### `06-visualization-monitors/02-monitors.md`
- **Topics covered:** every monitor type, monitor anatomy (detect/notify/restrict), thresholds and recovery, evaluation windows and aggregations, `as_count()` vs. `as_rate()`, notification syntax and template variables, multi-alert monitors, downtimes (recurrence, scope, mute first recovery), SLOs (metric/monitor/time-slice), error budgets.
- **Related exam questions:** "Which monitor type for a baseline deviation?", "How do you alert per host?", "What is a composite monitor?", "How do error-budget alerts work?"

### `reference/quick-reference-card.md`
- **Topics covered:** all sites, ports, key types, metric types, USTagging, Agent commands, file paths, monitor aggregations, monitor type rapid recall, dashboard widget rapid recall.
- **Related exam questions:** every "spot identification" question.

### `reference/common-pitfalls.md`
- **Topics covered:** 20 traps including API/App-key confusion, default site, histogram/distribution, cardinality, USTagging completeness, autodiscovery precedence, DogStatsD UDP loss, logs intake hostname, APM port, Cluster Agent connectivity, multi-alert behavior, composite syntax, downtime scope vs. identifier, Agent version policy, AWS integration vs. Agent.

---

## Suggested study traversal

| Day-of-week | Files to focus on |
|---|---|
| Mon | Domain 1 (whole folder) — set the foundation |
| Tue | Domain 2.A architecture + 2.C keys/sites |
| Wed | Domain 2.B install platforms (with hands-on) |
| Thu | Domain 3.A networking + 3.B integrations |
| Fri | Domain 3.C DogStatsD + Domain 4.A metric types |
| Sat | Domain 4.B tagging + 4.C events/checks/host map |
| Sun | Domain 5 troubleshooting + `reference/` cards |
| Following week | Domain 6 dashboards & monitors, daily mini-quizzes, then practice exams |

See `study-plan/study-plan.md` for the full 4-week schedule.
