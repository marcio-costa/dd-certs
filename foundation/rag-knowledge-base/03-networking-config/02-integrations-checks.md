# Domain 3.B — Integrations, Agent Checks, and Custom Checks

> **Source docs:** `/integrations/`, `/developers/integrations/agent_check`, `/developers/integrations/python`

## 3.B.1 What an "Integration" Is

A Datadog **integration** is a packaged way to collect data from a specific technology (PostgreSQL, Redis, NGINX, Kafka, AWS, Snowflake, etc.). It bundles:

- An **Agent check** (Python script or YAML config) that polls the technology and submits metrics, events, and service checks.
- A **manifest** (assets, OOTB dashboard, monitor templates, log pipeline).
- Documentation in the Datadog UI and docs site.

There are 700+ official integrations. The exam doesn't require you to memorize them, but expects you to recognize:
- How to **enable** one (config file or autodiscovery).
- The difference between **Agent integrations**, **API/crawler integrations** (AWS, Azure, GCP), and **community integrations**.

## 3.B.2 Configuring an Agent Check by File

Each Agent integration ships with a folder under:
```
/etc/datadog-agent/conf.d/<integration_name>.d/
    conf.yaml.example
    conf.yaml          (you create this; not present by default)
```

Example — Postgres:
```yaml
init_config:

instances:
  - host: db.internal.example.com
    port: 5432
    username: datadog
    password: <PASSWORD>
    tags:
      - "env:prod"
      - "service:orders"
```

After editing:
```bash
sudo systemctl restart datadog-agent
sudo datadog-agent status     # verify the check ran
sudo datadog-agent check postgres   # run once, show output
```

## 3.B.3 Autodiscovery (Containers)

In container environments you don't typically write `conf.yaml` files; instead the Agent discovers running services and applies templates.

### Sources of Autodiscovery configuration (in priority order)
1. **Pod annotations** (Kubernetes) — `ad.datadoghq.com/<container_name>.checks`
2. **Container labels** (Docker) — `com.datadoghq.ad.checks`
3. **Files** under `/etc/datadog-agent/conf.d/<integration>.d/`
4. **Auto-Conf** — bundled defaults shipped with the Agent for common images (Redis, NGINX, etc.). Lives at `/etc/datadog-agent/conf.d/auto_conf.yaml`.

### Annotations v2 (Agent v7.36+)
```yaml
metadata:
  annotations:
    ad.datadoghq.com/redis.checks: |
      {
        "redisdb": {
          "instances": [
            { "host": "%%host%%", "port": "6379" }
          ]
        }
      }
    ad.datadoghq.com/redis.logs: '[{"source":"redis","service":"cache"}]'
```

### Template variables you can use inside Autodiscovery configs
| Variable | Resolves to |
|---|---|
| `%%host%%` | Container's IP |
| `%%port%%` / `%%port_<index>%%` | Exposed port |
| `%%env_<NAME>%%` | Value of env var inside the container |
| `%%kube_namespace%%`, `%%kube_pod_name%%` | Kubernetes metadata |
| `%%container_id%%`, `%%container_name%%` | Runtime metadata |
| `%%hostname%%` | Host the container runs on |

### Docker labels equivalent
```dockerfile
LABEL "com.datadoghq.ad.checks"='{"redisdb": {"instances": [{"host": "%%host%%", "port": "6379"}]}}'
LABEL "com.datadoghq.ad.logs"='[{"source": "redis", "service": "cache"}]'
```

### Verifying Autodiscovery
```bash
sudo datadog-agent configcheck      # all loaded integration configs and where they came from
sudo datadog-agent status           # per-check run status, errors, last collection time
```

## 3.B.4 Custom Agent Checks

If no integration covers your need, write a **custom check** in Python.

`/etc/datadog-agent/checks.d/my_check.py`:
```python
from datadog_checks.base import AgentCheck

class MyCheck(AgentCheck):
    def check(self, instance):
        url = instance.get("url")
        # ... do something
        self.gauge("myapp.requests", 42, tags=["env:prod", f"url:{url}"])
        self.service_check("myapp.can_connect", AgentCheck.OK)
```

`/etc/datadog-agent/conf.d/my_check.d/conf.yaml`:
```yaml
init_config:

instances:
  - url: https://my-app.example.com
```

The check class exposes:
- `self.gauge`, `self.count`, `self.rate`, `self.histogram`, `self.distribution`, `self.monotonic_count`, `self.set` — metric submission
- `self.event(...)` — submit an event
- `self.service_check(name, status, tags=...)` — submit a service check (`OK`, `WARNING`, `CRITICAL`, `UNKNOWN`)

By default, the Collector runs each instance every **15 seconds** (`min_collection_interval: 15`). You can change it per instance.

## 3.B.5 DogStatsD vs. Agent Check vs. API

| Submission method | When you use it | Latency / batching |
|---|---|---|
| **DogStatsD** (UDP 8125) | Application code wants to emit **custom metrics**, **events**, or **service checks** without polling. | Aggregated locally (10-second flush by default), low overhead. UDP = lossy. |
| **Agent check** | A long-lived integration polling some endpoint or running a system command. Configured via YAML. | Runs on a schedule (default 15s). |
| **REST API** (`POST /api/v2/series`) | Submitting metrics directly without an Agent (e.g., from a Lambda without the extension). | Synchronous HTTPS; consumes the org's API rate limit. |

> **For the exam:** `gauge` reports the current value, `count` reports an increment over the flush interval, `rate` is normalized per-second, `histogram` and `distribution` capture statistical samples (more in Domain 4).
