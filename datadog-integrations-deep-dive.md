# Datadog Integrations — Comprehensive Study Note

> **Source-grounded learning note** | Generated 2026-05-25 | Reading time: ~25 min
> Tailored for: Sr Partner Solution Architect (Datadog, GSI Brazil) studying for the Datadog certification — with extra depth on JMX, custom checks, Auto-Discovery, and Secrets Management.

---

## TL;DR — Pareto ao cubo (the 1% that gives 99%)

- **An integration is just YAML living under `conf.d/<name>.d/conf.yaml` plus optional Python under `checks.d/`**. The Agent's Collector scans those folders on start (and on reload), instantiates one runner per entry in `instances:`, and runs `check()` every `min_collection_interval` seconds (default **15s**). Everything else — Auto-Discovery, JMX, custom checks — is a variant of this same pattern.

- **The config has exactly three load-bearing top-level keys**: `init_config` (shared once, usually empty), `instances` (a list — one per target), and optionally `logs`. In containerized environments you also see `ad_identifiers` (Auto-Discovery container image matcher) and `is_jmx: true` (turns the check into a JMXFetch check instead of a Python check).

- **For Kubernetes, you do NOT edit conf.yaml per pod**. You annotate the pod with `ad.datadoghq.com/<container_name>.checks` (modern v2 format) or `.check_names` / `.init_configs` / `.instances` (legacy v1). The Agent watches the Kube API, sees the annotation, expands `%%host%%`, `%%port%%`, `%%env_XXX%%` template variables at runtime, and applies the check — no restart, no manual reload.

- **Custom checks subclass `AgentCheck` and submit signals via four primary methods**: `self.gauge()`, `self.count()`, `self.rate()`, `self.histogram()` for metrics, plus `self.service_check(name, status, ...)` for OK/WARNING/CRITICAL/UNKNOWN status. The Collector catches unhandled exceptions, marks that run as failed, and retries on the next interval — the Agent does NOT crash and the check is NOT permanently disabled.

- **JMX troubleshooting follows a strict order**: (1) network reachability to the JMX port FIRST, (2) `is_jmx: true` in the check's conf.yaml, (3) the `datadog-agent jmx list ...` family to see what MBeans exist vs. what's matched vs. what's collected. There is NO global `jmx_enabled` flag in `datadog.yaml`.

- **Secrets never go plaintext in conf.yaml**. The Datadog-native pattern is `ENC[<secret_id>]` resolved at runtime by a `secret_backend_command` executable (or, since Agent 7.70+, the bundled `secret_backend_type` / `secret_backend_config`). Environment variable substitution with `$VAR` is NOT supported inside integration conf.yaml.

---

## Why this matters for you

As a Sr PSA at Datadog working with GSIs, integrations are the most concrete piece of the platform you'll demo, troubleshoot, and architect inside partner-led customer engagements. Every observability story — APM, logs, metrics, security — starts with an integration enabled and emitting data correctly. Knowing the anatomy of `conf.yaml`, Auto-Discovery in Kubernetes, and the JMX/secrets edge cases is the difference between a confident architect call and one where the GSI partner's engineer outpaces you. This is also the densest area of the Datadog certification exam: a few subtle distinctions (`is_jmx` vs. a nonexistent `jmx_enabled`, `service_check()` vs. invented method names, `ENC[]` vs. env vars) reliably show up as multi-select traps.

---

## Deep dive

### 1. What an integration actually is

A Datadog "integration" is a packaged way for the Agent to collect telemetry from something — a database, a JVM, a Kubernetes API, an AWS service, a custom internal app. Under the hood it is almost always one of two shapes:

```
┌─────────────────────────────────────────────────────────────────┐
│  Agent-side integration (the one this note focuses on)          │
│                                                                 │
│   conf.d/<integration>.d/conf.yaml   ←  declares targets        │
│   checks.d/<integration>.py          ←  Python that polls them  │
│                                          (optional — built-in   │
│                                           checks ship with the  │
│                                           Agent)                │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  Cloud / SaaS crawler integration                               │
│                                                                 │
│   Configured in the Datadog UI (Integrations tile) — Datadog    │
│   crawls AWS/Azure/GCP/SaaS APIs server-side. No Agent involved │
│   for metrics collection.                                       │
└─────────────────────────────────────────────────────────────────┘
```

This note is about Agent-side integrations because that's what the certification tests and what you'll touch most in GSI work.

The value proposition lives at three layers. First, **out-of-the-box coverage**: 750+ integrations means the partner's customer almost never has to build collection code. Second, **standardization**: every integration produces metrics that conform to Datadog's tagging and naming conventions, so a dashboard built for one customer's PostgreSQL works for the next. Third, **extensibility**: when a partner-built or in-house app isn't covered, the same Agent runs custom Python checks with the same lifecycle, same submission API, and same delivery path as the official ones.

### 2. Where files live (Linux paths the exam tests)

| Purpose | Path |
| --- | --- |
| Main Agent config | `/etc/datadog-agent/datadog.yaml` |
| Integration **configs** (YAML) | `/etc/datadog-agent/conf.d/<integration>.d/conf.yaml` |
| **Custom check** Python code | `/etc/datadog-agent/checks.d/<name>.py` |
| Example/default configs | shipped as `conf.yaml.example` in the same `.d/` folder |
| Agent embedded interpreter | `/opt/datadog-agent/embedded/` (NOT for user files) |

The pattern is intentional: configs in `conf.d/`, custom code in `checks.d/`. The custom check's Python filename and its conf.yaml folder name must match for the Collector to wire them together (e.g., `checks.d/my_app.py` ↔ `conf.d/my_app.d/conf.yaml`).

### 3. Anatomy of `conf.yaml` — the keys that actually matter

A canonical `conf.yaml` looks like this:

```yaml
# Optional — used by Auto-Discovery on containers (covered later)
ad_identifiers:
  - postgres

# Optional — flips this from a Python check to a JMXFetch check
is_jmx: false

# Configuration shared across ALL instances. Usually empty for built-ins.
init_config:
  global_custom_queries:
    - metric_prefix: app.db
      query: SELECT 1
      columns:
        - name: app.db.up
          type: gauge

# REQUIRED — one entry per thing you are monitoring.
instances:
  - host: db-primary.internal
    port: 5432
    username: datadog
    password: ENC[postgres_password]      # secrets backend, not plaintext
    tags:
      - env:prod
      - role:primary
    min_collection_interval: 30           # override the default 15s here

  - host: db-replica.internal
    port: 5432
    username: datadog
    password: ENC[postgres_password]
    tags:
      - env:prod
      - role:replica

# Optional — log collection bundled with the integration
logs:
  - type: file
    path: /var/log/postgresql/postgresql.log
    source: postgresql
    service: orders-db
```

Key takeaways the exam loves:

- **`init_config`** is shared across instances. Useful for default credentials or common tags. Most built-in integrations leave it empty.
- **`instances`** is a list. Each item becomes one independent check run scheduled by the Collector. If you have ten databases, you get ten entries.
- **`min_collection_interval`** lives **inside an instance**, not at the top level, not in `datadog.yaml`. Default is **15 seconds**. Setting it to 30 means "try every 30s" — the Collector may skip if the previous run hasn't finished.
- **`tags`** at the instance level attach to every metric and service check that instance emits. This is how you map metrics to environments, teams, services.
- **`logs`** lets one integration declare both metrics and log collection in the same file (you also need `logs_enabled: true` in `datadog.yaml`).

### 4. The Collector — what actually runs the check

Inside the Agent there are several subprocesses; the **Collector** is the one that schedules and runs checks. The lifecycle of any check is:

```
       Agent start / reload
              │
              ▼
   Scan conf.d/  ──▶  build one Runner per `instances` entry
              │
              ▼
   For each runner, every min_collection_interval seconds:
              │
              ▼
        runner.check(instance)
              │
       ┌──────┴──────┐
       │             │
   success         exception (any)
       │             │
       ▼             ▼
   metrics      Collector logs the error,
   forwarded    marks THIS RUN as failed,
                schedules the NEXT run normally.
                Agent does NOT crash.
                Check is NOT permanently disabled.
```

This is exactly the answer to "what happens to a custom check that raises an unhandled exception?" — the run fails, the next run is attempted. Permanent disabling never happens automatically.

After the check runs, metrics flow through the **Aggregator** (in-memory rollups: gauge → last value, count → sum, rate → per-second derivative) and then the **Forwarder** ships them over HTTPS to Datadog's intake endpoint. If `datadog-agent check mysql` shows local metrics but nothing appears in the UI, the failure is downstream of the check itself — almost always either the `api_key` in `datadog.yaml` is wrong, or the Forwarder cannot reach the intake (firewall, proxy, DNS).

### 5. Custom checks — the `AgentCheck` API you must memorize

Every custom check is a Python class that inherits from `AgentCheck`:

```python
# /etc/datadog-agent/checks.d/my_app.py
from datadog_checks.base import AgentCheck

__version__ = "1.0.0"

class MyAppCheck(AgentCheck):

    def check(self, instance):
        endpoint = instance["endpoint"]
        tags = instance.get("tags", [])

        try:
            response = self.http.get(endpoint)
            latency_ms = response.elapsed.total_seconds() * 1000

            # --- METRIC SUBMISSION ---
            self.gauge("my_app.latency.ms", latency_ms, tags=tags)
            self.count("my_app.requests", 1, tags=tags)
            self.rate("my_app.bytes_per_sec", response.content_size, tags=tags)
            self.histogram("my_app.payload_bytes", len(response.content), tags=tags)

            # --- SERVICE CHECK SUBMISSION ---
            self.service_check(
                "my_app.can_connect",
                AgentCheck.OK,
                tags=tags,
            )

            # --- EVENT SUBMISSION (optional) ---
            self.event({
                "timestamp": int(time.time()),
                "event_type": "my_app.deploy",
                "msg_title": "App responded",
                "msg_text": f"latency={latency_ms}ms",
                "tags": tags,
            })

        except Exception as e:
            self.service_check(
                "my_app.can_connect",
                AgentCheck.CRITICAL,
                tags=tags,
                message=str(e),
            )
            # Re-raising is fine — the Collector handles it
            raise
```

The four status constants on `AgentCheck` are the only valid arguments to `service_check`:

| Constant | Value | Meaning |
| --- | --- | --- |
| `AgentCheck.OK` | `0` | Healthy / green |
| `AgentCheck.WARNING` | `1` | Degraded / yellow |
| `AgentCheck.CRITICAL` | `2` | Failing / red |
| `AgentCheck.UNKNOWN` | `3` | Indeterminate / grey |

There is **no** `self.health_check()`, no `self.status()`, no `self.emit()`. The exam reliably uses these invented names as distractors. Anchor to the real API.

### 6. Auto-Discovery — how the Agent monitors moving containers

In Kubernetes (and Docker), pod IPs and ports change constantly. Hard-coding `host: 10.0.42.7` in conf.yaml is hopeless. Auto-Discovery solves this with three pieces:

**(a) An identifier** that says "this config template applies to this kind of container." The most common is the container image short name (`redis`, `nginx`, `postgres`).

**(b) A template** with placeholder variables that get resolved at runtime:

| Template variable | Resolves to |
| --- | --- |
| `%%host%%` | The IP of the container being monitored |
| `%%port%%` | The container's first exposed port |
| `%%port_<N>%%` | The N-th exposed port |
| `%%env_VAR_NAME%%` | The value of an env var on the container |
| `%%kube_pod_name%%` | The Kubernetes pod name |
| `%%kube_namespace%%` | The Kubernetes namespace |

**(c) A source** the Agent reads to find the template. Valid sources include:

- **Kubernetes pod annotations** (the most common — see below)
- **ConfigMaps** labeled for Datadog discovery (used with the Cluster Agent)
- **Local config files** placed in `conf.d/<integration>.d/auto_conf.yaml` shipping with the Agent

Datadog UI tiles, env vars in `datadog.yaml`, and Kubernetes Secrets are NOT Auto-Discovery sources.

#### Annotating a pod

Modern (v2) format — single key per container, JSON body:

```yaml
metadata:
  annotations:
    ad.datadoghq.com/redis.checks: |
      {
        "redisdb": {
          "init_config": {},
          "instances": [
            {
              "host": "%%host%%",
              "port": "6379",
              "password": "%%env_REDIS_PASSWORD%%"
            }
          ]
        }
      }
```

Legacy (v1) format — three separate keys:

```yaml
metadata:
  annotations:
    ad.datadoghq.com/redis.check_names: '["redisdb"]'
    ad.datadoghq.com/redis.init_configs: '[{}]'
    ad.datadoghq.com/redis.instances: '[{"host":"%%host%%","port":"6379"}]'
```

In both, the segment after `ad.datadoghq.com/` (here, `redis`) is the **container name inside the pod spec** — not the image. Mismatches between `containers[].name` and the annotation key are a frequent silent failure.

#### What triggers the Agent to apply the change

The Agent (running as a DaemonSet, one pod per node) continuously watches the Kubernetes API. When a pod appears with annotations matching containers on its own node, the Agent generates the check config in memory and starts running it on the next scheduling tick. **No Agent restart, no pod restart, no manual `datadog-agent reload`** is required for annotation-driven discovery.

### 7. JMX integrations — the corner with the most exam traps

JMX (Java Management Extensions) is how JVM-based apps expose metrics. Datadog uses a sidecar process called **JMXFetch** — bundled with the Agent — that connects to a target JVM's JMX endpoint over RMI/TCP and reads MBeans.

#### Turning on JMX for a check

There is **no `jmx_enabled: true` flag in `datadog.yaml`**. JMX is enabled per-check by setting `is_jmx: true` in the check's `conf.yaml`:

```yaml
# /etc/datadog-agent/conf.d/kafka.d/conf.yaml
is_jmx: true

init_config:
  is_jmx: true
  collect_default_metrics: true

instances:
  - host: kafka-broker-1
    port: 9999          # the JMX port on the broker
    name: kafka-broker-1
    tags:
      - env:prod
```

The MBeans to collect are described under `init_config.conf` (a list of MBean filters), each with `include` and optional `exclude`:

```yaml
init_config:
  is_jmx: true
  conf:
    - include:
        domain: kafka.server
        bean: kafka.server:type=BrokerTopicMetrics,name=MessagesInPerSec
        attribute:
          OneMinuteRate:
            alias: kafka.messages_in.rate
            metric_type: rate
```

#### Troubleshooting JMX — the right order

When a JMX check fails with `Cannot connect to JMX server`, work through this checklist top-down. Stop at the first thing that fails — don't jump to deeper config debugging.

```
1. NETWORK first.  Can the Agent host reach the JMX port?
   $ telnet kafka-broker-1 9999
   $ nc -zv kafka-broker-1 9999

   Common gotcha: the JVM bound JMX to 127.0.0.1 only. You must set:
     -Djava.rmi.server.hostname=<reachable_ip>
     -Dcom.sun.management.jmxremote.port=9999
     -Dcom.sun.management.jmxremote.rmi.port=9999

2. AUTHENTICATION.  Does the JVM require user/password or SSL on JMX?

3. is_jmx: true present in conf.yaml?

4. MBean filters.  Are include/exclude rules actually matching anything?
```

#### The `datadog-agent jmx` toolkit (Q15)

When the check connects but you're not getting the metrics you expect, you need to know **which MBeans exist on the JVM** and **which ones your filters are matching**. That's exactly what `datadog-agent jmx list ...` is for:

| Command | What it shows |
| --- | --- |
| `datadog-agent jmx list collected` | MBeans actually being collected right now |
| `datadog-agent jmx list matching` | MBeans that match your `include` rules |
| `datadog-agent jmx list not-matching` | MBeans on the JVM that do NOT match your rules |
| `datadog-agent jmx list everything` | Every MBean the JVM exposes |
| `datadog-agent jmx list limited` | MBeans matched but excluded because the 350-metric/instance cap was hit |
| `datadog-agent jmx collect` | Run collection once and print metrics to console |

The Agent 5/6-era forms (`list_collected_attributes`, `list_matching_attributes`, etc.) still appear in older docs but the v7 subcommand form (`list collected`, `list matching`) is what the modern exam expects.

> ⚠️ On Linux, prepend `sudo -u dd-agent` so the command runs as the same user as the Agent. Otherwise it can't read the JMXFetch config files and you'll see misleading "no MBeans" output.

`datadog-agent check <name> --log-level debug` gives you verbose check output but is **not** the right tool for MBean discovery — that's what `jmx list` is for.

### 8. Secrets management — `ENC[]`, not env vars

You cannot put secrets in plaintext in `conf.yaml` because anyone with read access to `/etc/datadog-agent/` can read them, and they often end up in Git or backups. Datadog's solution is the **secrets backend**:

```yaml
# /etc/datadog-agent/datadog.yaml
secret_backend_command: /etc/datadog-agent/secret-helper.sh
secret_backend_timeout: 5
# Only the dd-agent user should be able to read the executable
```

```yaml
# /etc/datadog-agent/conf.d/mysql.d/conf.yaml
instances:
  - host: mysql.internal
    port: 3306
    username: datadog
    password: ENC[mysql_datadog_password]   # ← resolved at runtime
```

At startup (and on reload), the Agent scans every loaded config for `ENC[<id>]` tokens, calls `secret_backend_command` over stdin with a JSON list of those IDs, and the executable returns a JSON map of `{id: plaintext}`. The plaintext lives only in Agent memory — it is never written to disk and never sent to Datadog.

Since Agent v7.70+ Datadog also ships a **native bundled backend**, configured via `secret_backend_type` (e.g., `aws.secrets`, `gcp.secrets`, `azure.keyvault`, `file.json`) and `secret_backend_config` — no external executable needed.

What the exam wants you to know definitively:

- **`ENC[<id>]`** is the syntax inside `conf.yaml`.
- **Environment variable substitution like `$VAR_NAME` is NOT supported** inside integration conf.yaml. Env vars work for top-level Agent settings like `DD_API_KEY`, not for arbitrary check fields.
- **Base64 encoding** is not a secret mechanism — it's plaintext to anyone who can read the file.
- You **cannot use `ENC[]` inside `secret_*` keys themselves** (no chicken-and-egg).

### 9. Troubleshooting playbook

When something is wrong, work outward from the check:

```
   ┌───────────────────────────────────────────────────────────────┐
   │ Step 1: Does the Agent know about the check?                  │
   │   $ sudo datadog-agent status                                 │
   │   Look under "Running Checks" — is your check listed?         │
   │   If NOT: conf.yaml is missing, mis-named, or has a YAML      │
   │   syntax error. Check Agent logs.                             │
   └───────────────────────────────────────────────────────────────┘
                          │
                          ▼
   ┌───────────────────────────────────────────────────────────────┐
   │ Step 2: Run the check manually.                               │
   │   $ sudo -u dd-agent datadog-agent check <name>               │
   │   For JMX: $ sudo -u dd-agent datadog-agent jmx collect       │
   │   Do metrics appear locally?                                  │
   │   If NOT: the check itself is failing — read the error.       │
   └───────────────────────────────────────────────────────────────┘
                          │
                          ▼
   ┌───────────────────────────────────────────────────────────────┐
   │ Step 3: Metrics local but not in Datadog UI?                  │
   │   - api_key in datadog.yaml correct?                          │
   │   - Forwarder healthy? `agent status` → "Forwarder" section   │
   │   - Outbound HTTPS to *.datadoghq.com / *.datadoghq.eu open?  │
   │   - Proxy needed? `proxy:` block in datadog.yaml              │
   │   - DD_SITE matches your org's site (datadoghq.com vs .eu)?   │
   └───────────────────────────────────────────────────────────────┘
                          │
                          ▼
   ┌───────────────────────────────────────────────────────────────┐
   │ Step 4: Auto-Discovery not picking up a container?            │
   │   $ sudo datadog-agent configcheck                            │
   │   Shows all loaded check configs and which AD source created  │
   │   them. If your pod isn't there:                              │
   │   - annotation key matches container NAME (not image)?        │
   │   - Agent running on the same node as the pod?                │
   │   - For ConfigMap source: Cluster Agent enabled?              │
   └───────────────────────────────────────────────────────────────┘
                          │
                          ▼
   ┌───────────────────────────────────────────────────────────────┐
   │ Step 5: Need a flare for Datadog Support                      │
   │   $ sudo datadog-agent flare <case_id>                        │
   │   Bundles configs (with secrets scrubbed), logs, status, and  │
   │   recent metrics into a tarball uploaded to Support.          │
   └───────────────────────────────────────────────────────────────┘
```

Useful one-liners:

```bash
# Show all loaded check configs and where they came from
sudo datadog-agent configcheck

# Validate datadog.yaml YAML syntax without restarting
sudo datadog-agent config check

# Reload integration configs without an Agent restart (v7+)
sudo systemctl restart datadog-agent      # full restart
sudo datadog-agent stream-logs            # tail logs the Agent is shipping

# JMX-specific
sudo -u dd-agent datadog-agent jmx list collected
sudo -u dd-agent datadog-agent jmx list not-matching
sudo -u dd-agent datadog-agent jmx list limited

# Verify secret resolution
sudo datadog-agent secret
```

---

## Practical examples

**Example: GSI customer with a fleet of PostgreSQL instances on EC2**

- *Context*: An Accenture-led customer runs 40 PostgreSQL instances across EC2 ASGs. They want the `postgres` integration on each, with per-instance tags for `cluster`, `role`, and `env`.
- *How the concept applies*: Ship a single `conf.d/postgres.d/conf.yaml` via configuration management (Ansible/Chef/Terraform user-data) with 40 entries in `instances`, each with `host`, `port`, and tags. The password lives once in AWS Secrets Manager, referenced as `password: ENC[pg_datadog_password]`, and `secret_backend_command` calls the AWS CLI to resolve it. Rotating the password means updating Secrets Manager — no Agent config change.
- *Outcome / what to notice*: The whole fleet uses one config artifact. Adding a new DB is one Terraform PR. Secrets never touch Git.

**Example: Kafka brokers on Kubernetes monitored via Auto-Discovery + JMX**

- *Context*: A Wipro DevOps team runs Kafka on EKS via the Strimzi operator. Broker pod IPs change on each rollout.
- *How the concept applies*: Annotate each broker pod with `ad.datadoghq.com/kafka.checks` containing a `kafka` JMX template (`is_jmx: true`, `host: "%%host%%"`, `port: "9999"`). The Datadog Agent DaemonSet sees new broker pods on its node, expands `%%host%%` to the current pod IP, and JMXFetch connects automatically. When a broker rolls, the old check is torn down and a new one stands up — no human action.
- *Outcome / what to notice*: Zero human reconfiguration during deploys. If a broker pod's metrics suddenly drop, the diagnostic path is `kubectl exec` into the Agent pod and run `datadog-agent jmx list collected` to confirm JMXFetch sees the new broker.

**Example: Custom check for a partner-built internal API**

- *Context*: An in-house "license-server" microservice that no integration exists for. The team wants three signals: request rate, latency p99, and a service-check that the `/health` endpoint returns 200.
- *How the concept applies*: A `checks.d/license_server.py` subclassing `AgentCheck`. The `check()` method hits `/health` and emits `self.gauge("license_server.latency_ms", ...)`, `self.count("license_server.requests", ...)`, and `self.service_check("license_server.health", AgentCheck.OK)` on success, or `AgentCheck.CRITICAL` on exception. A matching `conf.d/license_server.d/conf.yaml` lists endpoints in `instances:`.
- *Outcome / what to notice*: Same dashboards, same monitors, same SLO pages as any official integration. The custom code is ~40 lines.

**Example: GSI demoing Datadog for compliance — credentials must never be plaintext**

- *Context*: Deloitte's regulated-industry team is evaluating Datadog and the security review will fail if they find a DB password in a YAML file on disk.
- *How the concept applies*: `ENC[<id>]` in every integration conf.yaml, `secret_backend_command` pointing at a script that calls HashiCorp Vault, executable permissions `0500` owned by `dd-agent:dd-agent`. Show the security team that `cat conf.yaml` reveals only `ENC[mysql_password]`, never the plaintext.
- *Outcome / what to notice*: The demo turns a potential blocker into a selling point. Bring this in any GSI compliance conversation.

---

## Vocabulary boost (C1/C2 English)

| Term | Definition | Example sentence |
| --- | --- | --- |
| **anatomy of** | the underlying structure of something, broken into parts | "Let's walk through the anatomy of a `conf.yaml` before troubleshooting." |
| **load-bearing** | essential — if removed, the whole structure fails | "`instances` is the load-bearing key — without it the check never runs." |
| **canonical** | the standard, authoritative form of something | "Here's the canonical PostgreSQL conf.yaml — every other one is a variation of it." |
| **ephemeral** | short-lived, transient | "Container IPs are ephemeral, which is why Auto-Discovery exists." |
| **chicken-and-egg** | a circular dependency where each side requires the other to exist first | "You can't put `ENC[]` inside `secret_backend_command` — it's a chicken-and-egg problem." |
| **silently fail** | to break without raising any visible error | "A mismatched container name in the annotation will silently fail Auto-Discovery." |
| **distractor** | (in exam writing) a wrong answer designed to look plausible | "`self.health_check()` is a classic distractor — the real method is `service_check()`." |
| **anchor to** | to ground a decision in a reliable reference point | "When the exam invents method names, anchor to the actual AgentCheck API." |
| **outpace** | to move faster or progress more quickly than someone | "Without knowing JMX internals, you risk being outpaced by the partner's engineer on the call." |

---

## Quiz — Validate your understanding

**Part 1: Multiple choice (10 questions)**

**Q1.** Inside a check's `conf.yaml`, where do you set `min_collection_interval` to make a specific instance run every 30 seconds?

A) At the top level of `conf.yaml`
B) Inside `init_config`
C) Inside the relevant entry under `instances`
D) In `datadog.yaml` under `check_runners`

---

**Q2.** Which method on `AgentCheck` is used to signal that a monitored target is healthy?

A) `self.health_check("my.check", status="ok")`
B) `self.status("my.check", 0)`
C) `self.service_check("my.check", AgentCheck.OK)`
D) `self.gauge("my.check.status", 0)`

---

**Q3.** The Datadog Agent runs as a DaemonSet in Kubernetes. An engineer adds an `ad.datadoghq.com/redis.checks` annotation to a Redis pod. What must happen for the new check to take effect?

A) `datadog-agent reload` must be run on the node
B) The pod must be restarted
C) Nothing manual — the Agent watches the Kube API and applies the change on the next tick
D) A `datadog-checks` ConfigMap must be updated

---

**Q4.** A JMX check fails with `Cannot connect to JMX server`. The Kafka broker is running. What should you verify FIRST?

A) That the Agent host has network access to the JMX port on the broker
B) That `jmx_enabled: true` is set in `datadog.yaml`
C) That JMXFetch is installed as a separate package
D) That the broker has its own Datadog Agent installed

---

**Q5.** Which command shows you which MBeans currently exist on the JVM but are NOT matching any of your `include` filters?

A) `datadog-agent check kafka --log-level debug`
B) `datadog-agent jmx list not-matching`
C) `datadog-agent status --jmx`
D) `jmxfetch --list-beans`

---

**Q6.** What is the correct way to keep a database password out of plaintext in `conf.yaml`?

A) Base64-encode the password
B) Reference it as `$DB_PASSWORD` and set the env var on the Agent process
C) Use `password: ENC[db_password]` with a configured `secret_backend_command`
D) Store it in a Kubernetes Secret and mount it — the Agent reads files automatically

---

**Q7.** A custom check raises an unhandled `ConnectionError` during one of its runs. What does the Collector do?

A) Crashes the Agent and restarts it
B) Permanently disables the check until the next Agent restart
C) Marks that one run as failed and runs the check again at the next scheduled interval
D) Silently swallows the exception and reports `OK`

---

**Q8.** Which of these is the default check collection interval?

A) 5 seconds
B) 10 seconds
C) 15 seconds
D) 30 seconds

---

**Q9.** A custom check Python file should be placed in which directory?

A) `/etc/datadog-agent/conf.d/<name>.d/`
B) `/opt/datadog-agent/embedded/lib/`
C) `/etc/datadog-agent/checks.d/`
D) `/var/log/datadog/checks/`

---

**Q10.** Which Auto-Discovery template variable resolves to the IP of the container being monitored?

A) `%%kube_pod_name%%`
B) `%%host%%`
C) `%%node_ip%%`
D) `%%agent_host%%`

---

**Part 2: Scenario-based (7 questions)**

**Q11.** You're on a call with an Accenture engineer who insists that to monitor JMX in Kubernetes you need a dedicated `jmx-agent` sidecar in every Java pod. How do you respond?

**Q12.** A Deloitte customer's security team rejects Datadog because they see `password: hunter2` in a `mysql.d/conf.yaml` file checked into Git. Walk through the change you'd recommend, and what guarantee the customer's CISO gets.

**Q13.** A Wipro DevOps engineer reports: "I ran `datadog-agent check postgres` on the host and I see all my metrics in the output, but nothing shows up in the Datadog UI 30 minutes later." Give three independent things you'd ask them to check, in priority order.

**Q14.** A TCS team is rolling Kafka on EKS. After a deploy, three of nine brokers stop reporting metrics. The other six are fine. What do you check first, and what does `datadog-agent jmx list collected` from inside the Agent pod tell you?

**Q15.** You're designing a custom check for a partner-built API. The partner wants the check to emit one metric: the number of active users. Should you use `self.gauge()`, `self.count()`, or `self.rate()` — and why?

**Q16.** A Cognizant architect asks: "If I have a hundred PostgreSQL instances, do I need a hundred `postgres.d/` folders?" What's the right answer and why?

**Q17.** A customer's `conf.yaml` declares `is_jmx: true` and references `host: "%%host%%"` in a Kubernetes annotation. The Agent generates the config but JMXFetch errors with `Connection refused`. What are the two most likely root causes, in order?

---

## Answer key & explanations

**Q1 — C.** `min_collection_interval` is always at the instance level. Top-level placement is silently ignored; `init_config` is for shared parameters not scheduling; `datadog.yaml` has no `check_runners` field for per-check intervals.

**Q2 — C.** `self.service_check(name, status, tags=None, hostname=None, message=None)` is the real method. `AgentCheck.OK = 0`, `WARNING = 1`, `CRITICAL = 2`, `UNKNOWN = 3`. The other three options are invented distractors.

**Q3 — C.** The Agent (as a DaemonSet) continuously watches the Kubernetes API. New annotations on pods scheduled on its node are picked up automatically — no reload, no restart. ConfigMaps are a separate AD source, not required here.

**Q4 — A.** Always check network first when the error is "cannot connect." There is no `jmx_enabled` flag in `datadog.yaml` — JMX is enabled per-check with `is_jmx: true` in the integration's `conf.yaml`. JMXFetch is bundled with the Agent, not a separate package.

**Q5 — B.** `datadog-agent jmx list not-matching` is the dedicated tool. `--log-level debug` gives verbose output but is not designed for MBean discovery. `datadog-agent status --jmx` is not a real subcommand, and `jmxfetch --list-beans` is the wrong binary path.

**Q6 — C.** The Datadog-native pattern is `ENC[<id>]` with a `secret_backend_command` (or, in Agent 7.70+, `secret_backend_type`). Base64 is reversible plaintext. `$VAR` env var substitution is not supported in integration conf.yaml. Mounting a K8s Secret as a file does NOT make Datadog automatically read it.

**Q7 — C.** Unhandled exceptions are caught by the Collector. The current run is marked failed; the check stays registered and the next scheduled run proceeds normally. The Agent never crashes due to a failing user check.

**Q8 — C.** The default is 15 seconds. Setting `min_collection_interval` to a higher number means "try at least this often" — the Collector may skip if a previous run hasn't returned.

**Q9 — C.** Python code lives in `/etc/datadog-agent/checks.d/`. The matching config goes in `/etc/datadog-agent/conf.d/<name>.d/conf.yaml`. The `embedded/` path is the Agent's Python runtime — never put user code there.

**Q10 — B.** `%%host%%` → the container's IP. `%%kube_pod_name%%` is the pod name (text, not IP). `%%node_ip%%` and `%%agent_host%%` are not standard AD template variables.

---

**Q11 — Scenario.** Correct him politely with facts: JMXFetch is bundled with the Datadog Agent. The Agent DaemonSet's pod on the same node opens a TCP/RMI connection to the JVM's JMX port — no sidecar needed inside the Java pod. The Java pod only has to expose its JMX port (typically with `-Dcom.sun.management.jmxremote.port` and `-Djava.rmi.server.hostname`). The right artifact to share is the Kafka or Cassandra integration docs page showing the annotation-driven pattern. Trade-off worth flagging: if the JVM pod must remain network-isolated, then yes, a sidecar pattern (or push-based StatsD instead of pull-based JMX) is an option — but it is not the default.

**Q12 — Scenario.** The fix is `password: ENC[mysql_datadog_password]` in `conf.yaml`, plus `secret_backend_command: /etc/datadog-agent/vault-helper.sh` in `datadog.yaml`, plus a small executable that calls Vault (or AWS Secrets Manager / Azure Key Vault). Permissions: the helper executable is `0500`, owned by `dd-agent:dd-agent`. Guarantee to the CISO: the plaintext is only resolved into Agent memory at startup, never written to disk, never sent to Datadog. Git holds only `ENC[<id>]` references, which are useless without access to the customer's own vault. Mention Agent 7.70+'s bundled `secret_backend_type` if they want fewer moving parts.

**Q13 — Scenario.** Priority order: (1) `datadog-agent status` — is the Forwarder healthy? Look for "Transactions Errors" / "Transactions Dropped." (2) Is `api_key` in `datadog.yaml` correct and does `DD_SITE` match (datadoghq.com vs. datadoghq.eu)? A wrong site silently accepts the connection but data goes to the wrong org. (3) Outbound HTTPS to `*.datadoghq.com` — proxy needed? Corporate firewall blocking? The fact that the manual `check` shows local metrics means the check itself is fine — the issue is downstream.

**Q14 — Scenario.** First check: are the three failing brokers on the same Kubernetes node? If yes, the Datadog Agent pod on that node is the problem (maybe crashlooping, maybe out of resources). If they're spread across nodes, the issue is per-broker — likely JMX endpoint changed (new port, new hostname binding after the deploy). Running `datadog-agent jmx list collected` from inside the Agent pod tells you which JMX targets the Agent successfully connects to. If the three failing brokers aren't in the output, the Agent never connected — chase network and `-Djava.rmi.server.hostname`. If they ARE in the output but with fewer MBeans than the healthy six, chase `list matching` to compare include filters against actual MBeans.

**Q15 — Scenario.** `self.gauge()`. "Active users" is a point-in-time snapshot — at the moment the check runs, there are N active users. That's exactly what a gauge represents. `count` is for things that monotonically accumulate between runs (requests handled, errors raised). `rate` derives a per-second value from a count and would tell you "users joining per second," not "users active right now." A common mistake is to reach for `count` here because "users" feels countable — but the semantic is "current state," which is the definition of a gauge.

**Q16 — Scenario.** One folder, one `conf.yaml`, a hundred entries in `instances`. Each entry becomes one runner managed by the Collector. The split is per-target, not per-config-file. If they want per-DB customization (different tags, intervals, queries), that goes inside the corresponding instance entry. The only time you'd want multiple `.d` folders is if you're running fundamentally different check shapes for the same software — and even then, Auto-Discovery usually expresses it better than file-splitting.

**Q17 — Scenario.** Most likely (a): the JMX port isn't actually open on the broker container — the JVM was started without `-Dcom.sun.management.jmxremote.port`, or it was started with the port bound only to `127.0.0.1` (the loopback inside the container) instead of `0.0.0.0`. Second most likely (b): a Kubernetes NetworkPolicy is blocking the Agent pod from reaching the broker pod on that port. Test from inside the Agent pod with `kubectl exec`: `nc -zv <broker_pod_ip> <jmx_port>`. If TCP succeeds but JMX still refuses, you're back to the JVM-side `java.rmi.server.hostname` issue — RMI may hand the Agent a callback hostname that isn't reachable.

---

## Three things to lock in before exam day

1. **AgentCheck submission API** — `gauge`, `count`, `rate`, `histogram`, `service_check`, `event`, `log`. Plus the four status constants on `AgentCheck` (`OK`, `WARNING`, `CRITICAL`, `UNKNOWN`). Invented method names (`health_check`, `emit`, `status`) are always distractors.
2. **JMX diagnostic ladder** — network first → `is_jmx: true` second → `datadog-agent jmx list collected/matching/not-matching/everything/limited` third. No global `jmx_enabled`.
3. **Secrets** — `ENC[<id>]` in conf.yaml + `secret_backend_command` (or `secret_backend_type` in v7.70+). Not env vars, not base64, not silent file-mount magic.

---

## Sources

- [Datadog — Agent Configuration Files](https://docs.datadoghq.com/agent/configuration/agent-configuration-files/) (primary)
- [Datadog — Writing a Custom Agent Check](https://docs.datadoghq.com/developers/custom_checks/write_agent_check/) (primary)
- [Datadog — Service Check Submission: Agent Check](https://docs.datadoghq.com/extend/service_checks/agent_service_checks_submission/) (primary)
- [Datadog — Metric Submission: Custom Agent Check](https://docs.datadoghq.com/metrics/custom_metrics/agent_metrics_submission/) (primary)
- [Datadog — Basic Agent Autodiscovery](https://docs.datadoghq.com/getting_started/containers/autodiscovery/) (primary)
- [Datadog — Autodiscovery Template Variables](https://docs.datadoghq.com/containers/guide/template_variables/) (primary)
- [Datadog — Kubernetes and Integrations](https://docs.datadoghq.com/containers/kubernetes/integrations/) (primary)
- [Datadog — Autodiscovery with JMX](https://docs.datadoghq.com/agent/guide/autodiscovery-with-jmx/) (primary)
- [Datadog — Secrets Management](https://docs.datadoghq.com/agent/configuration/secrets-management/) (primary)
- [Datadog — Agent Commands](https://docs.datadoghq.com/agent/configuration/agent-commands/) (primary)
- [Datadog — Agent Troubleshooting](https://docs.datadoghq.com/agent/troubleshooting/) (primary)
- [Datadog Blog — Easy JMX discovery & browsing with the open source Agent](https://www.datadoghq.com/blog/easy-jmx-discovery-browsing-open-source-agent/)
- [GitHub — DataDog/integrations-core (conf.yaml.example references)](https://github.com/DataDog/integrations-core)
