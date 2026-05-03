# Instrumentation Types

> **Domain 2 — Application Instrumentation**

Datadog supports four instrumentation styles. The exam tests when to use each.

## 1. Single-Step Instrumentation (SSI)

The newest, fastest path — installs and instruments without code changes.

- **Linux hosts:** the install script (`install_script_agent7.sh`) with `DD_APM_INSTRUMENTATION_ENABLED=host` injects the tracer into apps managed by `systemd`.
- **Docker:** `DD_APM_INSTRUMENTATION_ENABLED=docker` patches running containers.
- **Kubernetes:** the **Cluster Agent** + **Admission Controller** mutates pods on creation to inject the tracer (Agent ≥ 7.39).
- Configurable per-language via `DD_APM_INSTRUMENTATION_LIBRARIES` (e.g. `java:1,python:4,js:5`).

Advantages: zero code changes, central control via cluster operators, opt-in via labels.

```yaml
metadata:
  labels:
    datadoghq.com/apm-instrumentation: "enabled"
```

## 2. Automatic Instrumentation (per app, library-level)

The tracer auto-detects supported frameworks and creates spans without code changes — but you still install the library and start it explicitly.

| Language    | How to start                                                    |
| ----------- | --------------------------------------------------------------- |
| Java        | `java -javaagent:/dd-java-agent.jar -jar app.jar`               |
| Python      | `ddtrace-run python app.py` *or* `import ddtrace.auto`          |
| Node.js     | `node --require dd-trace/init server.js`                        |
| Ruby        | `require 'datadog/auto_instrument'`                             |
| .NET        | `CORECLR_ENABLE_PROFILING=1` + `CORECLR_PROFILER_PATH=...`      |
| PHP         | Load `ddtrace.so` extension                                     |
| Go          | Import contrib packages (e.g. `chitrace`, `httptrace`) — no bytecode injection |

## 3. Manual / Custom Instrumentation

You explicitly create spans and attach attributes for code paths the auto-instrumenter doesn't cover.

```python
from ddtrace import tracer

@tracer.wrap(service="orders", resource="checkout")
def checkout(order):
    with tracer.trace("orders.validate") as span:
        span.set_tag("order.value", order.value)
        validate(order)
```

```ruby
Datadog::Tracing.trace('web.request',
                       service: 'my-web-service',
                       resource: 'Article#submit') do |span, trace|
  span.set_tag('custom.tag', 'value')
end
```

Use it for: business logic spans, custom metrics from spans, fine-grained latency breakdowns.

## 4. Dynamic Instrumentation

Datadog can add new spans, log lines, or metrics to a **running production app** **without redeploying**.

- Configured from the Datadog UI (no code change).
- Captures local variables, function args, and stack traces at chosen lines.
- Available for Java, Python, Node.js, .NET, and Ruby (varying maturity).
- Powered by tracer features that hot-load new probes.

Use it for: ad-hoc debugging, missing telemetry on a hot path, capturing rare-bug context.

## Decision matrix

| Need                                              | Best fit                          |
| ------------------------------------------------- | --------------------------------- |
| Roll APM out across many K8s services quickly     | Single-Step Instrumentation       |
| Standard web app with supported framework         | Automatic Instrumentation         |
| Track custom business operations                  | Manual Instrumentation            |
| Add telemetry to running prod with no redeploy    | Dynamic Instrumentation           |

## Things to remember for the exam

- Single-Step ≠ Automatic. SSI **installs the tracer for you**; automatic refers to *what the tracer does once running*.
- Dynamic Instrumentation requires **no code change and no restart**.
- Manual instrumentation is the way to add **custom span tags** like `@order.id`.
