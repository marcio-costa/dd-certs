# Domain 5 — Troubleshooting the Datadog Agent

> **Source docs:** `/agent/troubleshooting/`, `/agent/troubleshooting/agent_check_status/`, `/agent/troubleshooting/send_a_flare/`

The exam expects you to know which command to run for which symptom and where the Agent keeps its logs.

## 5.1 The `agent status` Command

The first stop for any Agent issue. Reports:

- **Agent version**, build, hostname, host tags.
- **Forwarder status**: count of payloads sent and any errors.
- **Aggregator** stats.
- **DogStatsD** stats: packets received, parse errors.
- **Per-check status**: last run time, last error, metrics emitted, total runs.
- **Logs Agent**, **Trace Agent**, **Process Agent** statuses.

```bash
# Linux/Mac
sudo datadog-agent status

# Docker
docker exec dd-agent agent status

# Kubernetes
kubectl exec <agent-pod> -- agent status

# Windows
& "$env:ProgramFiles\Datadog\Datadog Agent\bin\agent.exe" status
```

Filter to a specific check:
```bash
sudo datadog-agent status check postgres
```

## 5.2 Where Agent Logs Live

| Platform | Path |
|---|---|
| Linux | `/var/log/datadog/agent.log`, `trace-agent.log`, `process-agent.log`, `system-probe.log` |
| Windows | `C:\ProgramData\Datadog\logs\agent.log`, `trace-agent.log`, etc. |
| macOS | `/opt/datadog-agent/logs/` |
| Container | stdout/stderr of the Agent container; also `/var/log/datadog/` inside the container |
| Kubernetes | `kubectl logs <agent-pod> -c agent` (and `trace-agent`, `process-agent`) |

### Adjust log level on the fly
```bash
sudo datadog-agent config set log_level debug    # Agent 7.34+
sudo datadog-agent config get log_level
```

Or in `datadog.yaml`:
```yaml
log_level: debug   # one of: trace, debug, info, warn, error, critical, off
```
Restart the Agent to apply.

## 5.3 The `flare` Command

A **flare** packages logs, configuration (with secrets scrubbed), and runtime state into a zip and uploads it to Datadog Support.

```bash
sudo datadog-agent flare <CASE_ID>           # Linux
docker exec dd-agent agent flare <CASE_ID>   # Docker
kubectl exec <agent-pod> -- agent flare <CASE_ID>   # K8s
```

If you don't have a case yet, the command will offer to create one with your account email.

What's inscrubbed/scrubbed:
- `api_key` → masked.
- Passwords in integration configs → masked.
- Files unrelated to Datadog → not included.

## 5.4 Other Useful Commands

| Command | What it does |
|---|---|
| `datadog-agent configcheck` | Lists every loaded check config and where it came from (file, annotation, Auto-Conf). |
| `datadog-agent check <name> [-l debug] [--check-rate]` | Runs an integration check once and prints its output, including each metric and tag. |
| `datadog-agent diagnose` | Runs a battery of self-checks (connectivity, hostname resolution, permission, etc.) and reports pass/fail. |
| `datadog-agent diagnose datadog-connectivity` | Tests connectivity to each Datadog endpoint for your site. |
| `datadog-agent hostname` | Prints the hostname the Agent will report. |
| `datadog-agent secret` | Tests the secret backend. |
| `datadog-agent jmx list collected` | Inspects what JMX integrations are pulling. |

## 5.5 Common Symptoms → Likely Causes

| Symptom | First thing to check |
|---|---|
| "Forbidden" / "Invalid API key" in `agent status` | Wrong key, key revoked, or wrong `site:`. |
| Some checks missing metrics | `agent status check <name>` to see error; `configcheck` to confirm config loaded. |
| All metrics intermittently missing | Forwarder retries — check network, proxy, DNS, TLS. |
| No traces visible | Trace Agent not enabled (`apm_config.enabled: true`), wrong port, app not configured with `DD_AGENT_HOST` and `DD_TRACE_AGENT_PORT`. |
| No container metrics on Docker | Docker socket not mounted, or `dd-agent` user not in `docker` group. |
| No logs | `logs_enabled: true` missing; for containers, `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true` not set; or `agent-intake.logs.<site>` blocked at firewall. |
| Hostname mismatch / duplicate hosts | `hostname` was set explicitly and conflicts with cloud metadata; clear it and let auto-detection win. |
| Wrong tags on metrics | Conflicting tag layers (Agent config vs. integration vs. container labels). Use `configcheck` to confirm tag inheritance. |
| Custom metrics overage warning | Cardinality explosion; identify high-cardinality tags via the **Metrics Summary**. |

## 5.6 The Agent's GUI

On Linux/macOS, the Agent ships a tiny web GUI at `http://127.0.0.1:5002` (Linux) or via `agent launch-gui` (Windows/macOS). It mirrors what `agent status` shows and lets you submit a flare.

## 5.7 Agent Self-Telemetry

The Agent reports **its own** metrics under the `datadog.agent.*` and `datadog.dogstatsd.*` namespaces:

- `datadog.agent.running` — gauge, `1` while the Agent is up. Easy "is the Agent up" monitor.
- `datadog.agent.payload.dropped` — count of dropped payloads (network/back-end issues).
- `datadog.dogstatsd.packet_drops_in_total` — UDP packets the DogStatsD server failed to read.

A common monitor: alert when `avg by host of datadog.agent.running` is **below 1** for 5 minutes — that's a host whose Agent went down.
