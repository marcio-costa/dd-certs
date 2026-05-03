# Tracing Architectures

> **Domain 1 — APM Fundamentals**

## Three common architectures

### 1. Application → Datadog Agent → Datadog backend (recommended)

```
[Instrumented App] --(localhost:8126)--> [Datadog Agent / trace-agent] --(HTTPS)--> [Datadog]
```

- Tracing libraries (`dd-trace-java`, `dd-trace-py`, etc.) live **inside the app**.
- The Agent runs **on the same host or node** (DaemonSet in Kubernetes, sidecar in some cases).
- Communication uses HTTP on **port 8126** by default, or a Unix Domain Socket.
- The Agent buffers, samples, computes trace metrics, and ships to Datadog.

### 2. Application → OpenTelemetry Collector → Datadog backend

```
[OTel-instrumented App] --(OTLP)--> [OTel Collector with Datadog exporter] --> [Datadog]
```

- Application uses OpenTelemetry SDKs.
- An **OpenTelemetry Collector** with the **Datadog exporter** translates OTLP to Datadog's format.
- Useful for orgs already invested in OTel, or for multi-vendor tracing.
- Collector listens on OTLP gRPC port `4317` and HTTP `4318`.
- Use the `datadog/connector` to compute APM trace metrics.

### 3. Datadog Distribution of OpenTelemetry Collector (DDOT)

- A Datadog-built distribution of the OTel Collector that ships with the Agent.
- Combines OTel data flexibility with Datadog Agent capabilities (e.g. infraattributes processor).
- Can be enabled in the DatadogAgent CR: `features.otelCollector.enabled: true`.

### 4. Serverless (Lambda)

- Datadog Lambda Extension runs as a Lambda layer.
- Tracing library inside the function generates spans, which are forwarded by the extension.
- No standalone Agent host needed.

## When to choose which

| Scenario                                       | Recommended architecture                                 |
| ---------------------------------------------- | -------------------------------------------------------- |
| Greenfield, want fastest setup                 | dd-trace library + Datadog Agent (Single-Step Install)   |
| Already standardized on OpenTelemetry          | OTel SDK + Collector + Datadog exporter                  |
| Want both OTel flexibility and Agent features  | DDOT collector                                           |
| AWS Lambda                                     | Datadog Lambda Extension                                 |

## Things to remember for the exam

- The Agent **always** sits between the app (or collector) and Datadog backend in non-serverless setups.
- Default trace port: **8126/TCP**. DogStatsD: **8125/UDP**.
- `DD_APM_NON_LOCAL_TRAFFIC=true` is needed to accept traces from other containers/pods.
- OTel and dd-trace can coexist; Datadog supports both protocols.
