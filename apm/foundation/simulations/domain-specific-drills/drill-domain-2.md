# Domain-Specific Drill — Domain 2: Application Instrumentation

> 15 questions, ~20 minutes. Take at the end of Week 2.

---

**Q1.** Which env var enables the Continuous Profiler in most languages?

A) `DD_PROFILER_ON`
B) `DD_PROFILING_ENABLED`
C) `DD_TRACE_PROFILE`
D) `DD_PROFILE_ALWAYS`

---

**Q2.** Which sampling rule grammar is **current** (replacing the deprecated `DD_TRACE_SAMPLE_RATE`)?

A) `DD_TRACE_SAMPLER_RATE`
B) `DD_TRACE_SAMPLING_RULES`
C) `DD_APM_SAMPLE_PCT`
D) `DD_TRACE_RULES`

---

**Q3.** A team needs to **scrub a sensitive header value** from every span before it leaves the Agent. The cleanest mechanism is:

A) `DD_APM_REPLACE_TAGS`
B) `DD_TAG_OBFUSCATE`
C) `DD_APM_OBFUSCATE_HEADERS`
D) `DD_TRACE_HEADER_DROP`

---

**Q4.** Which is **true** about Single-Step Instrumentation (SSI) in Kubernetes?

A) It requires manual code changes in every microservice.
B) It requires the Cluster Agent + Admission Controller to inject the tracer at pod creation.
C) It bypasses the Datadog Agent entirely.
D) It only works on Java applications.

---

**Q5.** Where does **SQL obfuscation** for APM spans run?

A) In the database driver.
B) In the application tracer.
C) In the Datadog trace-agent.
D) In the Datadog backend storage tier.

---

**Q6.** What does `DD_APM_IGNORE_RESOURCES='GET /healthz'` achieve?

A) Drops health-check traces at the Agent.
B) Disables APM for healthchecks at the application.
C) Stops generating runtime metrics for health endpoints.
D) Forces healthcheck spans into the rare retention bucket.

---

**Q7.** A Java app uses automatic instrumentation. Which JVM flag is required to enable it?

A) `-Xtrace=datadog`
B) `-Ddd.trace.enabled=true`
C) `-javaagent:dd-java-agent.jar`
D) `-Ddatadog.tracer=on`

---

**Q8.** Which statement about runtime metrics is correct?

A) They are always enabled and require no configuration.
B) They flow over DogStatsD UDP/8125 when enabled and appear on the Service Page.
C) They are extracted from spans by the Datadog backend.
D) They replace trace metrics.

---

**Q9.** A team needs to **add new spans to a running production app without redeploying**. The right tool is:

A) Manual instrumentation
B) Single-Step Instrumentation
C) Dynamic Instrumentation
D) DogStatsD events

---

**Q10.** Which is a recommended way for a Kubernetes pod to point at the local node's Agent?

A) Hardcode `127.0.0.1:8126`.
B) Use `DD_AGENT_HOST=status.hostIP` from the Downward API, or use a UDS hostPath.
C) Set `DD_AGENT_HOST=0.0.0.0`.
D) Always use the OTel Collector.

---

**Q11.** A retention filter is **independent** of:

A) Sampling decisions.
B) Whether trace metrics are computed.
C) The 15-day indexed retention period.
D) Both A and B.

---

**Q12.** A team wants every error trace from a critical service to be searchable for 15 days. The minimum required configuration is:

A) Set `DD_TRACE_SAMPLE_RATE=1.0` only.
B) Add a custom retention filter matching `service:critical-service status:error`.
C) Disable the intelligent retention filter.
D) Increase `DD_APM_TARGET_TPS` to 1000.

---

**Q13.** Which env var auto-injects trace and span IDs into log lines from supported tracers?

A) `DD_TRACE_LOG_INJECT`
B) `DD_LOGS_INJECTION`
C) `DD_LOG_TRACE_LINK`
D) `DD_TRACE_CORRELATE_LOGS`

---

**Q14.** Which statement is **true** about the Continuous Profiler?

A) It runs only during local development.
B) It samples in production at a target overhead of about 1% CPU.
C) It must be installed as a separate binary outside the tracer.
D) It replaces APM tracing.

---

**Q15.** A team needs to drop traces that lack an `env:prod` tag. The right knob is:

A) `DD_APM_FILTER_TAGS_REQUIRE='env:prod'`
B) `DD_APM_REJECT_TAGS='env:prod'`
C) `DD_TAG_REQUIRE_ENV=prod`
D) `DD_APM_DROP_NON_PROD=true`

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | B | 2.6 |
| 2 | B | 2.4 |
| 3 | A | 2.5 |
| 4 | B | 2.2 |
| 5 | C | 2.3 / 2.5 |
| 6 | A | 2.5 |
| 7 | C | 1.5 / 2.2 |
| 8 | B | 2.1 |
| 9 | C | 2.2 |
| 10 | B | 2.1 / 2.3 |
| 11 | D | 2.4 |
| 12 | B | 1.4 / 2.4 |
| 13 | B | 2.1 / 4.3 |
| 14 | B | 2.6 |
| 15 | A | 2.3 / 2.5 |
