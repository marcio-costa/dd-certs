# Full Mock Exam #1 — Answer Key

> Use this **only after** you've finished all 75 questions and stopped the clock.

## Per-domain answer table

### Section A — Domain 1: APM Fundamentals (Q1–Q19)

| # | Answer | KB §  | Note |
|---|--------|-------|------|
| 1 | B | 1.1 | A trace is a tree of spans for an end-to-end request. |
| 2 | B | 1.2 | trace-agent default is 8126/TCP; OTLP gRPC is 4317. |
| 3 | B | 1.3 | USM = `env`, `service`, `version`. |
| 4 | B | 1.4 | Live Search window = 15 minutes. |
| 5 | C | 1.4 | Custom retention filter = 15 days. |
| 6 | C | 1.4 | Intelligent retention filter = 30 days. |
| 7 | D | 1.4 / 3.8 | Trace metrics retained 15 months. |
| 8 | B | 1.2 / 2.4 | Head-based: decide at root span, propagate via context. |
| 9 | C | 1.5 | Go uses contrib packages, not bytecode injection. |
| 10 | B | 1.5 | Community = OpenTelemetry SDK / third-party. |
| 11 | B | 3.5 | Priority: explicit `DD_VERSION` > image tag > Git SHA. |
| 12 | B | 1.2 | Datadog Lambda Extension is the recommended Lambda path. |
| 13 | B | 1.5 | Broader OOTB framework coverage and Datadog feature parity. |
| 14 | B | 1.1 | Resource = specific action within a service. |
| 15 | A | 1.3 | USM correlates APM, logs, processes, infra metrics. |
| 16 | B | 1.1 / 4.3 | Trace context propagates via headers (`traceparent` etc.). |
| 17 | A | 1.2 | DogStatsD = 8125/UDP. |
| 18 | B | 2.3 | `DD_APM_NON_LOCAL_TRAFFIC` defaults to `true` since 7.18. |
| 19 | B | 1.4 / 2.4 | Trace metrics from 100% ingested, retained 15 months. |

### Section B — Domain 2: Application Instrumentation (Q20–Q41)

| # | Answer | KB §  | Note |
|---|--------|-------|------|
| 20 | B | 2.6 | `DD_PROFILING_ENABLED=true` is canonical. |
| 21 | C | 2.4 | `DD_TRACE_SAMPLING_RULES` is the modern form. |
| 22 | B | 2.5 | Obfuscation runs in the trace-agent. |
| 23 | B | 2.5 | `DD_APM_IGNORE_RESOURCES` accepts regex list. |
| 24 | B | 2.3 | `DD_APM_NON_LOCAL_TRAFFIC=true`. |
| 25 | A | 2.5 | `DD_APM_REPLACE_TAGS` with regex patterns. |
| 26 | B | 2.2 | `-javaagent:dd-java-agent.jar` enables JVM auto-instrumentation. |
| 27 | B | 2.3 | `DD_APM_TARGET_TPS` defaults to 10. |
| 28 | C | 2.2 | Dynamic Instrumentation = no redeploy. |
| 29 | B | 2.1 / 2.3 | Downward API host IP or UDS hostPath. |
| 30 | B | 2.1 / 2.3 | Standard UDS path: `/var/run/datadog/apm.socket`. |
| 31 | B | 2.2 | SSI in Kubernetes uses the Cluster Agent + Admission Controller. |
| 32 | B | 2.1 | Runtime metrics flow over DogStatsD UDP/8125. |
| 33 | B | 2.4 | Retention filters decide long-term indexed-span storage. |
| 34 | B | 2.1 / 4.3 | `DD_LOGS_INJECTION=true`. |
| 35 | B | 2.4 | Custom retention filter for that query. |
| 36 | B | 2.6 | ~1% CPU target, always-on in production. |
| 37 | A | 2.3 / 2.5 | `DD_APM_FILTER_TAGS_REQUIRE='env:prod'`. |
| 38 | B | 2.4 | Sampling = ingest decision; retention = storage decision. |
| 39 | B | 2.2 | SSI requires no application code changes. |
| 40 | B | 2.2 | `ddtrace-run` is automatic library-level instrumentation. |
| 41 | A | 2.3 | `DD_APM_ENABLED=false` disables the trace-agent. |

### Section C — Domain 3: Insight Discovery (Q42–Q63)

| # | Answer | KB §  | Note |
|---|--------|-------|------|
| 42 | B | 3.1 | APM traces alone discover the service; YAML/OTel can supplement. |
| 43 | B | 3.1 | Cluster and Flow are the supported map layouts. |
| 44 | C | 3.2 | `@` precedes custom span tags. |
| 45 | B | 3.2 | Reserved attributes (no `@`); `version:` is reserved. |
| 46 | D | 3.3 | Indexed Search **respects** retention filters; it doesn't bypass them. |
| 47 | C | 3.4 | Flame graph is the default. |
| 48 | B | 3.4 / 3.5 | Profile Comparison view diffs two profiles. |
| 49 | B | 3.5 | `DD_VERSION` > image tag > Git SHA. |
| 50 | B | 3.5 | Service Page Deployments tab. |
| 51 | A | 3.6 | Fingerprint = `error.type` + `error.message` + stack frames. |
| 52 | B | 3.6 | `error.fingerprint` overrides grouping. |
| 53 | B | 3.6 | Resolved → recurring on deploy = auto-reopen. |
| 54 | B | 3.7 | Span Summary's four columns. |
| 55 | B | 3.8 | `trace.<operation>.<metric>` convention. |
| 56 | B | 3.8 | Custom span-based metric is billed as a custom metric. |
| 57 | B | 3.8 | Watchdog overlays Requests, Latency, Error rate. |
| 58 | B | 3.3 | Anything beyond 15 min flips to Indexed Search. |
| 59 | C | 3.1 | Service Map is the dependency visualization. |
| 60 | B | 3.1 | Service Definition v2.2 supports rich metadata. |
| 61 | A | 1.4 / 2.4 | Custom retention filter on `service:checkout resource_name:"POST /checkout"`. |
| 62 | C | 3.2 | Both range and AND-of-comparisons are valid. |
| 63 | B | 3.1 | Mismatched service names create distinct catalog entries. |

### Section D — Domain 4: Troubleshooting Application using APM (Q64–Q75)

| # | Answer | KB §  | Note |
|---|--------|-------|------|
| 64 | B | 4.1 | Service Page is the canonical incident triage entry. |
| 65 | B | 4.1 | Critical Path = longest causal path from root to leaf. |
| 66 | A | 4.2 | Threshold alert on a trace metric (p95 evaluator). |
| 67 | B | 4.2 | Watchdog scans error rate, latency, hits. |
| 68 | B | 4.2 | Roughly 0.5 req/s threshold to suppress noise. |
| 69 | B | 4.2 | Anomaly monitor handles seasonal-aware deviation. |
| 70 | C | 4.2 | Default automatic APM error-rate threshold = 10%. |
| 71 | B | 4.2 | Schedule a downtime to silence during planned events. |
| 72 | B | 4.3 | `DD_LOGS_INJECTION=true`. |
| 73 | B | 4.3 | `traceparent` is the W3C trace context header. |
| 74 | B | 4.2 | APM Trace Analytics Monitor queries individual indexed spans. |
| 75 | B | 3.3 / 4.1 | Beyond 15 min = Indexed Search; trace must be retained. |

---

## Scoring template

| Domain | Correct | Total | % | Pass (≥70%)? |
|--------|---------|-------|---|---------------|
| Domain 1 | __ | 19 | __ | __ |
| Domain 2 | __ | 22 | __ | __ |
| Domain 3 | __ | 22 | __ | __ |
| Domain 4 | __ | 12 | __ | __ |
| **Total** | __ | **75** | __ | **≥80%?** |

## Action plan based on score

| Result | What to do next |
|--------|-----------------|
| ≥80% overall, ≥70% per domain | You're ready. Run a daily mini-quiz the day before the exam. Review the cheat sheet in `study-plan/exam-day-checklist.md`. |
| 70–79% overall, all domains ≥60% | Run **adaptive weak-area drills** on your two lowest domains. Retake mock in 3–5 days. |
| <70% overall, or any domain <60% | Re-read the relevant KB sections, redo the domain-specific drill, then run two adaptive drills before another mock attempt. |

## Logging

Copy the scoring table into `progress/exam-results.md` with the date you took the mock.
