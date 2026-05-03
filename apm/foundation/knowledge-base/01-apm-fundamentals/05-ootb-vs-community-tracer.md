# OOTB Tracer vs Community Tracer & Language Differences

> **Domain 1 — APM Fundamentals**

## Datadog tracing libraries (OOTB / "Officially supported")

Datadog publishes and maintains tracing libraries for major languages. These are the **out-of-the-box (OOTB) tracers**:

| Language         | Library           | Repository                  |
| ---------------- | ----------------- | --------------------------- |
| Java             | `dd-trace-java`   | `DataDog/dd-trace-java`     |
| Python           | `dd-trace-py`     | `DataDog/dd-trace-py`       |
| Node.js          | `dd-trace-js`     | `DataDog/dd-trace-js`       |
| Ruby             | `dd-trace-rb` / `datadog`     | `DataDog/dd-trace-rb` |
| Go               | `dd-trace-go`     | `DataDog/dd-trace-go`       |
| .NET             | `dd-trace-dotnet` | `DataDog/dd-trace-dotnet`   |
| PHP              | `dd-trace-php`    | `DataDog/dd-trace-php`      |
| C++              | `dd-trace-cpp`    | `DataDog/dd-trace-cpp`      |

**OOTB benefits**

- Officially supported by Datadog (SLA, security, version compatibility).
- Auto-instruments **dozens of frameworks** out of the box (HTTP, gRPC, SQL, ORMs, queues, caches).
- Integrates seamlessly with the Agent, Continuous Profiler, ASM, and DBM.
- Receives Datadog-specific features first (Watchdog metadata, ASM payload capture, DBM linkage).

## Community tracers

A "community tracer" is one **not authored or maintained by Datadog** — typically:

- An OpenTelemetry SDK with the Datadog exporter.
- A community OSS project that emits Datadog-compatible spans.
- A custom in-house tracer using the `dd-trace` agent endpoint API directly.

**Trade-offs**

| Aspect                       | OOTB Tracer (`dd-trace-*`) | Community / OTel SDK    |
| ---------------------------- | -------------------------- | ----------------------- |
| Datadog support              | Full                       | Best-effort             |
| Out-of-the-box framework coverage | Very broad             | Narrower                |
| Continuous Profiler          | Built-in                   | Separate setup needed   |
| Single-step install support  | Yes                        | Limited                 |
| Vendor neutrality            | Datadog-only               | OpenTelemetry standard  |
| Feature parity with new Datadog releases | Immediate     | May lag                 |

## Language-level differences in Automatic Instrumentation

Not all languages support the same auto-instrumentation mechanism:

| Language      | Automatic mechanism                                                    |
| ------------- | ---------------------------------------------------------------------- |
| **Java**      | `-javaagent:dd-java-agent.jar` bytecode instrumentation at JVM startup. |
| **.NET**      | CLR profiler via `CORECLR_*` env vars (Linux/Windows).                 |
| **Python**    | `ddtrace-run` wrapper or `import ddtrace.auto`.                        |
| **Node.js**   | `--require dd-trace/init` or `import 'dd-trace/init'`.                 |
| **Ruby**      | `require 'datadog/auto_instrument'`.                                   |
| **PHP**       | INI extension (`ddtrace.so`) loaded by PHP runtime.                    |
| **Go**        | **No automatic bytecode injection**: manual aliasing of contrib packages (`httptrace`, `sqltrace`, `chitrace`). Optional Single-Step Orchestrion-based auto-instrument is in beta. |

## Things to remember for the exam

- Datadog libraries are sometimes called the **"officially supported" tracers**.
- Community tracers usually = OpenTelemetry path.
- **Go** is the language that **cannot** be fully auto-instrumented via bytecode rewriting — you must import contribution packages.
- All tracing libraries send spans to the local **Agent on port 8126** by default.
