# Week 2 — Application Instrumentation (Domain 2)

> Goal: own the practical "how do I get telemetry into Datadog" toolkit — every config knob, every instrumentation style, sampling, security, and profiling.

| Day | Block 1 — Read | Block 2 — Lab | Block 3 — Quiz | Block 4 — Reflect |
|-----|------------------|---------------|----------------|--------------------|
| **Mon** | KB §2.1 Tracing Libraries + KB §2.3 Datadog Agent Architecture | Open `agent status`. Identify trace-agent section. Set `DD_AGENT_HOST` via env var, restart, observe behavior. | Quiz: `quizzes/domain-2-instrumentation.md` (Q1–Q5) | List the 5 most important `DD_*` env vars from memory. |
| **Tue** | KB §2.2 Instrumentation Types | Try **Single-Step Instrumentation** on a Docker app: `DD_APM_INSTRUMENTATION_ENABLED=docker DD_APM_INSTRUMENTATION_LIBRARIES=python:4 bash install_script.sh`. | Quiz: Q6–Q10 | Compare in writing: SSI vs Auto vs Manual vs Dynamic. |
| **Wed** | KB §2.4 Sampling vs Retention | Set `DD_TRACE_SAMPLING_RULES='[{"service":"my-app","sample_rate":0.5}]'`. Generate traffic; verify sampling. Add a custom **retention filter** in the UI and confirm spans appear in Indexed Search. | Quiz: Q11–Q15 | Write a sentence: "Sampling = ___, retention = ___". |
| **Thu** | KB §2.5 APM Data Security | Add `DD_APM_REPLACE_TAGS` to scrub a tag. Add `DD_APM_IGNORE_RESOURCES` for `/healthz`. Verify in the UI. | Quiz: Q16–Q20 | Document where SQL obfuscation happens in the pipeline. |
| **Fri** | KB §2.6 Profile Collection | Enable Continuous Profiler (`DD_PROFILING_ENABLED=true`) on the sample app. Open a flame graph in the UI. Use the comparison view between two recent timeranges. | **Domain 2 mini-mock** (15 questions) — `simulations/domain-specific-drills/drill-domain-2.md` | Score; identify weak topics. |
| **Sat** | Optional — re-read weak areas. | Hands-on capstone: deploy the Agent as a **Kubernetes DaemonSet**, configure pods with USM labels, and verify traces flow with UDS instead of TCP. | Optional **daily mini-quiz**. | None. |
| **Sun** | Rest, or finish the Datadog Learning Center "Instrumentation" course. | — | — | Plan Week 3. |

## Success criteria for Week 2

- [ ] You can pick the right instrumentation type for a given scenario.
- [ ] You can list at least 8 `DD_APM_*` env vars and explain them.
- [ ] You can describe the sampling pipeline from app → Agent → backend.
- [ ] You can configure a retention filter using search syntax.
- [ ] You scored ≥80% on the Domain 2 mini-mock.
