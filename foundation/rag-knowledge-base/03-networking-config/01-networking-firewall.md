# Domain 3.A — Networking, Firewalls, and Proxies

> **Source docs:** `/agent/configuration/network/`, `/agent/configuration/proxy/`

## 3.A.1 What the Agent Needs on the Network

All Agent traffic is **outbound only**, over **HTTPS port 443**, to a small set of regional endpoints.

For US1, the relevant hostnames include (this list is not exhaustive but is what the Agent uses):

| Hostname | Purpose |
|---|---|
| `agent-intake.logs.datadoghq.com` | Logs intake |
| `metrics.agent.datadoghq.com` | Metrics intake (versioned: `<version>-app.agent.datadoghq.com`) |
| `trace.agent.datadoghq.com` | APM trace intake |
| `process.datadoghq.com` | Live Processes / Live Containers |
| `orchestrator.datadoghq.com` | Kubernetes orchestrator data |
| `instrumentation-telemetry-intake.datadoghq.com` | Tracer telemetry |
| `api.datadoghq.com` | REST API (also used by `agent flare`) |
| `flare.datadoghq.com` | Send flare bundles |
| `config.datadoghq.com` | Remote Configuration |
| `dbm-metrics-intake.datadoghq.com` | Database Monitoring |
| `ndmflow-intake.datadoghq.com` | Network Device Monitoring (NetFlow) |

> Replace the suffix `datadoghq.com` with your **site's domain** (`datadoghq.eu`, `us3.datadoghq.com`, `us5.datadoghq.com`, `ddog-gov.com`, `ap1.datadoghq.com`).

## 3.A.2 Local Listening Ports

| Port | Protocol | Component | Purpose |
|---|---|---|---|
| 8125 | UDP | DogStatsD | Receive custom metrics, events, and service checks from your apps |
| 8126 | TCP | trace-agent | Receive APM spans from tracer libraries |
| 5000 | TCP | core Agent IPC | Used by `agent status`, GUI |
| 5001–5003 | TCP | Cluster Agent | Cluster Agent API and DCA <-> Node Agent |
| 5555 | TCP | Cluster Agent | Admission controller (TLS) |
| 6062 | TCP | trace-agent expvar | Internal metrics |

Inbound from Datadog: **none required.** Datadog SaaS never connects in to your environment.

## 3.A.3 Proxies

If outbound internet is restricted, the Agent supports proxies via three mechanisms:

### A. Proxy block in `datadog.yaml`
```yaml
proxy:
  http:  http://USER:PASS@proxy.example.com:3128
  https: http://USER:PASS@proxy.example.com:3128
  no_proxy:
    - 127.0.0.1
    - localhost
    - 169.254.169.254       # cloud metadata service
    - .internal.example.com
no_proxy_nonexact_match: true
logs_config:
  force_use_http: true       # ensure logs use HTTP+proxy, not raw TCP
```

### B. Environment variables
```
DD_PROXY_HTTP=http://proxy.example.com:3128
DD_PROXY_HTTPS=http://proxy.example.com:3128
DD_PROXY_NO_PROXY=127.0.0.1,localhost
```
or the standard `HTTPS_PROXY` / `HTTP_PROXY` / `NO_PROXY` env vars.

### C. A relay/proxy host
Run a small proxy (Squid, NGINX, HAProxy, or another **Datadog Agent** in proxy mode) inside your network, then point fleet Agents at it. Logs and traces also support being routed through such a relay.

> **Precedence:** Per-feature settings > `datadog.yaml` proxy block > `DD_PROXY_*` env vars > `HTTPS_PROXY` env vars.

## 3.A.4 Common Networking Pitfalls

- **Logs use a separate endpoint** (`agent-intake.logs.<site>`). Even if metrics work, logs may fail if firewall/proxy doesn't allow that hostname.
- **APM** uses `trace.agent.<site>`; if you're seeing metrics but no traces, suspect this hostname.
- **TLS interception** by an MITM proxy will break the Agent unless the corporate CA is trusted by the OS cert store.
- **Cloud metadata service** (`169.254.169.254` on AWS, similar on GCP/Azure) must be in `no_proxy`, otherwise the Agent can't enrich data with cloud tags.
- The **Cluster Agent** also needs outbound access to Datadog; not just the Node Agents.

## 3.A.5 IP Ranges

Datadog publishes its IP ranges per site at `https://ip-ranges.datadoghq.com/` (and per-site equivalents). Use them for firewall allow-lists when DNS-based rules aren't an option.

The categories include:
- `webhooks` — used by Datadog when calling your endpoints
- `api` — `api.<site>` and related
- `agents` — Agent intake endpoints
- `synthetics` — Synthetic test workers
- `synthetics-private-locations` — IPs used by managed PLs
- `logs`, `process`, `orchestrator`, `remote-configuration`, `apm`, `global` — per-product

These ranges change; treat the JSON file as the source of truth.
