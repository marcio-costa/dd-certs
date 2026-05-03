# Profile Collection (Continuous Profiler)

> **Domain 2 — Application Instrumentation**

## What it is

The **Continuous Profiler** (CP) gathers **always-on** code-level performance data — CPU, memory, lock, exception and wall-clock samples — from your services in production at low overhead (target ~1% CPU).

## Enable per language

Common pattern: enable in the same library you already use for tracing.

| Language    | How to enable                                                                            |
| ----------- | ---------------------------------------------------------------------------------------- |
| Java        | `-javaagent:dd-java-agent.jar` (auto) + `DD_PROFILING_ENABLED=true`                     |
| Python      | `import ddtrace.profiling.auto` *or* `DD_PROFILING_ENABLED=true` with `ddtrace-run`     |
| Node.js     | `DD_PROFILING_ENABLED=true` with `--require dd-trace/init`                              |
| Ruby        | `DD_PROFILING_ENABLED=true`; profiler activates automatically                           |
| .NET        | `DD_PROFILING_ENABLED=1` + `LD_PRELOAD=/opt/datadog/continuousprofiler/Datadog.Linux.ApiWrapper.x64.so` |
| Go          | `import "gopkg.in/DataDog/dd-trace-go.v1/profiler"` + `profiler.Start()`                |
| PHP         | Set `datadog.profiling.enabled = 1`                                                     |

The library uploads profiles every **60 seconds by default**.

## Profiler visualizations

| Visualization     | What it shows                                                                       |
| ----------------- | ----------------------------------------------------------------------------------- |
| **Flame graph**   | Default view: stack frames as horizontal blocks; width = resource consumption.      |
| **Hotspots**      | Top N most expensive functions across all stacks.                                   |
| **Source code**   | Inline source view with hot lines highlighted.                                      |
| **Comparison**    | Diff two profiles side-by-side (e.g., before/after deploy).                         |
| **Timeline**      | Per-thread stack samples over time.                                                 |
| **Profiler dashboard widgets** | Embed flame graphs in dashboards.                                       |

## Available profile types

- **CPU** — sampled CPU time per stack frame.
- **Wall** (Wall-clock) — wall-clock time including I/O waits.
- **Allocations** — memory allocated per stack.
- **Heap / live objects** — currently retained memory.
- **Lock contention** — time threads block waiting for locks.
- **Exceptions** — count and stacks of thrown exceptions.

(Available types vary by language.)

## Correlation with traces

A profile sample carries the trace and span ID active at sampling time, enabling:

- "Show profiles for this trace" from a flame graph in the trace view.
- "Show traces hitting this code path" from the profiler.

This is one of the strongest features: connect *which line of code* caused *which slow request*.

## Things to remember for the exam

- Continuous Profiler is **separate from tracing** but shipped in the same library.
- Default visualization is the **flame graph**.
- It runs **always-on** in production at low overhead.
- Enable with `DD_PROFILING_ENABLED=true` (env var name is the same across most languages).
- Profile data is correlated to spans via the active trace context.
