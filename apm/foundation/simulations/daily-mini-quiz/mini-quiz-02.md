# Daily Mini-Quiz #02 (10 Qs)

> Mixed. ~10 minutes.

---

**Q1.** Which architecture sends application traces through an OpenTelemetry Collector before they reach Datadog?

A) `dd-trace` library → Datadog Agent → Datadog
B) OpenTelemetry SDK → OTel Collector with Datadog exporter → Datadog
C) Datadog Lambda Extension → Datadog
D) Application → DogStatsD → Datadog

---

**Q2.** Which language requires importing contrib packages instead of bytecode-level auto-instrumentation?

A) Java   B) Python   C) Go   D) .NET

---

**Q3.** Trace metrics are computed from:

A) 100% of ingested spans
B) Only retained spans
C) Only error spans
D) Spans matching the rare filter

---

**Q4.** A custom retention filter keeps spans for ___; the intelligent retention filter keeps spans for ___:

A) 7 days / 15 days
B) 15 days / 30 days
C) 24 hours / 7 days
D) 30 days / 90 days

---

**Q5.** Single-Step Instrumentation in Kubernetes relies on:

A) The Datadog Agent only
B) The Cluster Agent + Admission Controller
C) DogStatsD on every pod
D) An OTel Collector

---

**Q6.** A user wants to drop healthcheck traces at the Agent. The right knob is:

A) `DD_APM_REPLACE_TAGS`
B) `DD_APM_IGNORE_RESOURCES='GET /healthz'`
C) `DD_TRACE_DROP=healthz`
D) `DD_APM_NON_LOCAL_TRAFFIC=false`

---

**Q7.** Which monitor type detects deviation from a learned seasonal pattern?

A) Threshold   B) Anomaly   C) Forecast   D) Outlier

---

**Q8.** The Service Page Deployments tab is used to:

A) Manage incidents
B) Compare metrics between two software versions
C) Configure retention filters
D) Edit dashboards

---

**Q9.** A team needs to keep 100% of `POST /payments` traces for 15 days. They should:

A) Set `DD_TRACE_SAMPLE_RATE=1` only
B) Add a custom retention filter for `service:payments resource_name:"POST /payments"`
C) Disable the intelligent retention filter
D) Use Live Search only

---

**Q10.** Which Kubernetes-recommended pattern points an app at the local node's Agent?

A) Hardcode `127.0.0.1:8126`
B) `DD_AGENT_HOST=status.hostIP` via downward API, or UDS hostPath
C) `DD_AGENT_HOST=0.0.0.0`
D) Use the API key directly from the app

---

## Answer Key

| # | Answer | KB § |
|---|--------|------|
| 1 | B | 1.2 |
| 2 | C | 1.5 |
| 3 | A | 1.4 / 3.8 |
| 4 | B | 1.4 |
| 5 | B | 2.2 |
| 6 | B | 2.5 |
| 7 | B | 4.2 |
| 8 | B | 3.5 |
| 9 | B | 1.4 / 2.4 |
| 10 | B | 2.1 / 2.3 |
