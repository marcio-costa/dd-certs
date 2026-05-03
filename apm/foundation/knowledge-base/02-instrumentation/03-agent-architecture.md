# Datadog Agent Architecture (APM perspective)

> **Domain 2 — Application Instrumentation**

## Agent processes relevant to APM

The Datadog Agent is a multi-process binary. The most relevant ones for APM:

| Process            | Role                                                   |
| ------------------ | ------------------------------------------------------ |
| `agent`            | Main process — metrics, integrations, orchestration.   |
| `trace-agent`      | Receives spans from the application on **port 8126**. Computes trace metrics, applies sampling and obfuscation, then forwards. |
| `process-agent`    | Live process discovery, container info.                |
| `system-probe`     | eBPF-based network and security data.                  |
| `dogstatsd`        | UDP `8125` for custom metrics, runtime metrics.        |

## Trace-agent pipeline

```
   App tracer
      │ HTTP /v0.x/traces  (port 8126)
      ▼
   ┌─────────────────────────────────────────────┐
   │             Datadog trace-agent             │
   │  ┌─ Receiver ── Normalizer ── Obfuscator    │
   │  │      └─ rejects malformed spans          │
   │  ├─ Sampler (priority + rules)              │
   │  ├─ Trace metrics calculator                │
   │  └─ Writers (traces + stats) → backend      │
   └─────────────────────────────────────────────┘
      │ HTTPS
      ▼
   trace.agent.<dd_site>
```

Key points:

- All ingested spans contribute to **trace metrics**, even if the span itself is dropped.
- Obfuscator handles SQL/MongoDB/ElasticSearch normalization.
- Configurable via `apm_config:` in `datadog.yaml` or `DD_APM_*` env vars.

## Important env vars

| Variable                        | Default                                | Purpose                                      |
| ------------------------------- | -------------------------------------- | -------------------------------------------- |
| `DD_APM_ENABLED`                | `true` (Agent 7.18+)                   | Master switch for trace-agent.               |
| `DD_APM_RECEIVER_PORT`          | `8126`                                 | TCP listen port.                             |
| `DD_APM_RECEIVER_SOCKET`        | (unset)                                | UDS path (e.g. `/var/run/datadog/apm.socket`). |
| `DD_APM_NON_LOCAL_TRAFFIC`      | `true` (Agent 7.18+)                   | Accept traces from non-localhost.            |
| `DD_APM_DD_URL`                 | `https://trace.agent.<site>`           | Override backend endpoint.                   |
| `DD_APM_TARGET_TPS`             | `10`                                   | Default per-service traces-per-second target. |
| `DD_APM_ERROR_TPS`              | `10`                                   | Error trace chunks per second per service.   |
| `DD_APM_MAX_EPS`                | `200`                                  | Max APM events/sec.                          |
| `DD_APM_FILTER_TAGS_REQUIRE`    |                                        | Drop traces missing these tags.              |
| `DD_APM_FILTER_TAGS_REJECT`     |                                        | Drop traces matching these tags.             |
| `DD_APM_REPLACE_TAGS`           |                                        | Scrub tag values (data security).            |
| `DD_APM_IGNORE_RESOURCES`       |                                        | Regex list of resources to drop entirely.    |
| `DD_APM_MAX_MEMORY`             | `500000000` bytes                      | Soft memory cap; rate-limits when exceeded.  |
| `DD_APM_MAX_CPU_PERCENT`        | `50`                                   | Soft CPU cap.                                |

## Deployment patterns

| Platform        | Agent deployment                                                            |
| --------------- | --------------------------------------------------------------------------- |
| Linux/Windows host | Package install or installer script. App talks to `localhost:8126`.       |
| Docker          | Agent container with `8126` exposed; app containers point to host or shared network. |
| Kubernetes      | Agent **DaemonSet** (one per node). Pods set `DD_AGENT_HOST=status.hostIP` or use UDS via `hostPath`. |
| ECS Fargate     | Agent as a sidecar in the task definition.                                  |
| Lambda          | **Datadog Lambda Extension** layer; no host Agent.                          |

## Things to remember for the exam

- The trace-agent is **not** a separate binary you install; it ships with the standard Datadog Agent.
- Default port is **8126/TCP**; UDS is preferred in K8s.
- `DD_APM_NON_LOCAL_TRAFFIC=true` is required for cross-container traces.
- Sampling and obfuscation happen **at the Agent**, not in the app.
