# Domain 2.A — Datadog Agent Architecture

> **Source docs:** `/agent/architecture`, `/agent/basic_agent_usage`, `/agent/configuration`

## 2.A.1 What the Agent Does

The Datadog Agent is the lightweight software that runs on your hosts (VMs, containers, serverless extensions) to:

1. **Collect** system, application, and integration data (metrics, logs, traces, events, service checks, profiles, network, security signals).
2. **Aggregate** data locally (especially DogStatsD metrics) and apply tags.
3. **Forward** payloads over HTTPS to the Datadog backend that matches your site.

The current major version is **Agent 7** (Python 3 only). Agent 6 is in maintenance. Agent 5 is end-of-life and should not be deployed for new installs.

## 2.A.2 Internal Components (Agent 6/7)

The Agent runs as a single binary with several internal sub-components:

| Component | Role |
|---|---|
| **Collector** | Runs the configured **integration checks** at their interval (default: every 15s) and produces metrics, events, service checks. |
| **Forwarder** | Buffers, batches, and ships payloads to the Datadog backend. Handles retries with exponential back-off. |
| **DogStatsD server** | Listens on **UDP 8125** (or a Unix socket) for custom metrics from your application code. |
| **Trace Agent** (`trace-agent`) | Listens on **TCP 8126** for spans from APM tracer libraries. Performs sampling and forwards traces. |
| **Logs Agent** | Tails files / containers / sockets and ships logs over compressed HTTPS or TCP. |
| **Process Agent** | Collects live processes, containers, and (with Cloud Network Monitoring enabled) network connections. |
| **System Probe** | Optional eBPF-based component for Cloud Network Monitoring, USM, CWS. |
| **Security Agent** | Runs CSM/CWS detection rules. |

In Agent **5** (legacy), each was a separate Python process under `supervisord`; in Agent 6/7 they are sub-processes/goroutines of one Go binary, with the exception of `trace-agent`, `process-agent`, and `system-probe`, which are still separate processes managed by the main Agent.

## 2.A.3 Configuration Surfaces

| Surface | Where | Used for |
|---|---|---|
| `datadog.yaml` | `/etc/datadog-agent/datadog.yaml` (Linux), `C:\ProgramData\Datadog\datadog.yaml` (Windows) | Main Agent settings: API key, site, tags, proxy, log/APM/process toggles. |
| `conf.d/<integration>.d/conf.yaml` | `/etc/datadog-agent/conf.d/` | Per-integration check configuration (one file per integration). |
| Environment variables | The container, systemd unit, or shell | Override anything in `datadog.yaml`. Format: `DD_<UPPERCASE_KEY>` (nested keys joined by `_`). |
| Container labels / pod annotations | Docker labels, K8s pod annotations | **Autodiscovery** (Domain 3). |
| Helm `values.yaml` / Operator CR | K8s | Wraps the above to render config into the cluster. |

### Minimal `datadog.yaml`
```yaml
api_key: <YOUR_API_KEY>
site: datadoghq.com           # Change for non-US1 sites
logs_enabled: true
apm_config:
  enabled: true
process_config:
  process_collection:
    enabled: true
tags:
  - team:platform
  - env:prod
```

## 2.A.4 Lifecycle Commands

Linux (systemd):
```bash
sudo systemctl start  datadog-agent
sudo systemctl stop   datadog-agent
sudo systemctl status datadog-agent
sudo datadog-agent status            # Agent self-report
sudo datadog-agent flare <CASE_ID>   # Send diagnostic bundle to Support
sudo datadog-agent configcheck       # Show all loaded integration configs
sudo datadog-agent check <name>      # Run a single integration check once
```

Windows:
```powershell
& "$env:ProgramFiles\Datadog\Datadog Agent\bin\agent.exe" status
& "$env:ProgramFiles\Datadog\Datadog Agent\bin\agent.exe" launch-gui
& "$env:ProgramFiles\Datadog\Datadog Agent\bin\agent.exe" flare
```

Docker:
```bash
docker exec -it dd-agent agent status
docker exec -it dd-agent agent flare <CASE_ID>
```

Kubernetes:
```bash
kubectl exec -it <agent-pod> -- agent status
kubectl exec -it <agent-pod> -- agent flare <CASE_ID>
```

## 2.A.5 Where Data Goes

```
+---------------+       UDP 8125       +-----------+
|  Your app /   |--------------------->| DogStatsD |
|  custom code  |   metrics, events    |  server   |
+---------------+                      +-----+-----+
                                             |
+---------------+   integration checks       v
|   System,     |   (HTTP, JMX, SNMP, ...)   |
|   integrations|--------------------------->|
+---------------+                            |
                                       +-----+-----+        HTTPS 443
                                       | Forwarder |---------------------->  Datadog SaaS
                                       +-----------+   (compressed JSON)        backend
+---------------+   TCP 8126                                                        |
|  APM tracers  |--------------------->  trace-agent  --(HTTPS 443)--------> trace.<site>
+---------------+   spans
```

## 2.A.6 Common Misconceptions to Avoid on the Exam

- The Agent does **not** open inbound ports for Datadog to call into. All traffic is **Agent → Datadog over outbound 443**.
- Custom metrics submitted via DogStatsD are sent to **port 8125 UDP on localhost**, not directly to Datadog.
- Setting `site:` is required for any region other than US1; if you forget it, data goes to the wrong region.
- Agent 7 only supports Python 3 for custom checks; Agent 5 used Python 2.7 and is deprecated.
