# Quiz — Domain 2: Application Instrumentation (20 Qs)

> Single-answer multiple choice. Answers at the bottom.

---

**Q1.** Which env var enables the Datadog Continuous Profiler in most languages?

A) `DD_CPU_PROFILING`
B) `DD_PROFILING_ENABLED`
C) `DD_PROFILE_RUN`
D) `DD_TRACE_PROFILER`

---

**Q2.** A team wants to roll APM out across many Kubernetes pods without touching application code. What's the best mechanism?

A) Manual instrumentation in each service.
B) DogStatsD client in each pod.
C) Single-Step Instrumentation via the Cluster Agent and Admission Controller.
D) Run a separate Datadog Agent inside every pod.

---

**Q3.** Where does SQL obfuscation for APM spans take place?

A) In the application tracer.
B) In the Datadog backend.
C) In the trace-agent (Datadog Agent).
D) In the database driver.

---

**Q4.** Which env var instructs the trace-agent to accept connections from non-localhost addresses?

A) `DD_APM_REMOTE_ENABLED`
B) `DD_APM_NON_LOCAL_TRAFFIC`
C) `DD_APM_REMOTE_ACCEPT`
D) `DD_BIND_ALL`

---

**Q5.** Which is the modern, JSON-based env var to define per-service sampling rules?

A) `DD_TRACE_SAMPLE_RATE`
B) `DD_SAMPLER_RULES`
C) `DD_TRACE_SAMPLING_RULES`
D) `DD_APM_SAMPLE_PCT`

---

**Q6.** The default `DD_APM_TARGET_TPS` value is:

A) 1
B) 10
C) 100
D) 1000

---

**Q7.** Which env var lets you scrub sensitive content from arbitrary span tags?

A) `DD_APM_REPLACE_TAGS`
B) `DD_TAG_SCRUBBER`
C) `DD_APM_SECURE_MODE`
D) `DD_OBFUSCATE_TAGS`

---

**Q8.** A pod sends `application/x-protobuf` traces to the Agent in Kubernetes (DaemonSet). The recommended way to point the app at the local Agent is:

A) Hardcode `127.0.0.1:8126`.
B) Use the Kubernetes Service IP for the Agent.
C) Set `DD_AGENT_HOST` to the node IP via `status.hostIP` downward API, or use a UDS hostPath.
D) Send traces via DogStatsD on 8125.

---

**Q9.** Which statement about head-based sampling is true?

A) Each service decides independently whether to keep a trace.
B) The decision is made when the trace ends.
C) The decision is made at the root span and propagated downstream.
D) Sampling is performed in the Datadog backend.

---

**Q10.** A retention filter keeps a trace for 15 days. If you also need to keep the trace for **30 days**, which mechanism is responsible?

A) A second custom retention filter.
B) The intelligent retention filter (Datadog-managed).
C) The trace metrics retention.
D) The error-rare filter.

---

**Q11.** Which environment variable enables auto-injection of trace IDs into log lines from the tracer?

A) `DD_LOGS_TRACE_ID`
B) `DD_LOGS_INJECTION`
C) `DD_TRACE_LOG_LINK`
D) `DD_LOGGER_INJECT`

---

**Q12.** A retention filter is intended to:

A) Drop traces with PII at ingest time.
B) Decide which sampled spans are kept for long-term searchable storage.
C) Reduce CPU on the trace-agent.
D) Disable sampling for a specific service.

---

**Q13.** Which is **true** about the Continuous Profiler?

A) It samples production code at a target overhead of about 1% CPU.
B) It only runs during development, never in production.
C) It replaces APM instrumentation.
D) It must be installed as a separate binary.

---

**Q14.** Which feature lets you add new spans, log lines, or metrics to a running production app without redeploying?

A) Single-Step Instrumentation
B) Manual Instrumentation
C) Dynamic Instrumentation
D) DogStatsD

---

**Q15.** Which of the following is the canonical way to drop entire traces by resource regex?

A) `DD_APM_FILTER_TAGS_REJECT`
B) `DD_APM_REPLACE_TAGS`
C) `DD_APM_IGNORE_RESOURCES`
D) `DD_TRACE_DROP_RESOURCE`

---

**Q16.** Which of these statements about Single-Step Instrumentation is correct?

A) It only works on Linux hosts.
B) It only supports Java.
C) It can be enabled in host, Docker, and Kubernetes modes via `DD_APM_INSTRUMENTATION_ENABLED`.
D) It bypasses the Datadog Agent and sends traces directly to the backend.

---

**Q17.** Trace metrics are computed from:

A) 100% of ingested spans, regardless of sampling/retention decisions.
B) Only sampled-and-retained spans.
C) Only error spans.
D) The output of custom span-based metrics.

---

**Q18.** Which UDS path is commonly used for the Agent's APM receiver in Kubernetes?

A) `/var/run/datadog/dsd.socket`
B) `/var/run/datadog/apm.socket`
C) `/var/run/agent/trace.sock`
D) `/var/lib/datadog/apm.sock`

---

**Q19.** A Java application uses automatic instrumentation. Which startup flag is required?

A) `-Ddd.trace.enabled=true`
B) `-javaagent:dd-java-agent.jar`
C) `-Xtrace=datadog`
D) `-Ddd.classloader=true`

---

**Q20.** Which of the following best describes how runtime metrics (JVM, Python, Node) reach Datadog?

A) Sent via 8126/TCP with traces.
B) Sent via DogStatsD on 8125/UDP, when enabled.
C) Sent over HTTPS directly to Datadog.
D) They are not collected by Datadog.

---

## Answer Key

| # | Answer | Explanation |
|---|--------|-------------|
| 1 | **B** | `DD_PROFILING_ENABLED=true` is the canonical knob across nearly every language. |
| 2 | **C** | The Cluster Agent + Admission Controller path is the K8s-native SSI mechanism. |
| 3 | **C** | Obfuscation runs in the trace-agent; the application sends raw SQL, the Agent normalizes it. |
| 4 | **B** | `DD_APM_NON_LOCAL_TRAFFIC=true` (default `true` since Agent 7.18) accepts non-local traffic. |
| 5 | **C** | `DD_TRACE_SAMPLING_RULES` (JSON array) is the modern replacement for `DD_TRACE_SAMPLE_RATE`. |
| 6 | **B** | Default target is 10 TPS per service. |
| 7 | **A** | `DD_APM_REPLACE_TAGS` accepts patterns to redact span tag values. |
| 8 | **C** | Either downward-API host IP or a UDS hostPath are the recommended K8s patterns. Hardcoding 127.0.0.1 won't work because the Agent runs on the node, not in the pod. |
| 9 | **C** | Head-based sampling: decide at the root span and propagate via trace context. |
| 10 | **B** | The intelligent retention filter keeps spans for 30 days. |
| 11 | **B** | `DD_LOGS_INJECTION=true` is the canonical knob. |
| 12 | **B** | Retention filters decide which ingested spans are kept long-term in the indexed store. |
| 13 | **A** | The Continuous Profiler is **always-on in production** at a target overhead of ~1% CPU. |
| 14 | **C** | Dynamic Instrumentation adds telemetry to a running app without redeploy. |
| 15 | **C** | `DD_APM_IGNORE_RESOURCES` accepts a comma-separated regex list. |
| 16 | **C** | SSI supports `host`, `docker`, and Kubernetes via Cluster Agent admission control. |
| 17 | **A** | Trace metrics use 100% of ingested data — independent of sampling/retention. |
| 18 | **B** | The standard UDS path is `/var/run/datadog/apm.socket`. |
| 19 | **B** | `-javaagent:dd-java-agent.jar` is required to enable bytecode instrumentation. |
| 20 | **B** | Runtime metrics flow over DogStatsD UDP/8125 when enabled. |
