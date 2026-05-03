# Quiz — Domain 4: Data Collection (20 Qs — heavily weighted domain)

> Time: ~20 min.

---

**1.** What is the difference between the **count** and **rate** metric submission types?
A. They are the same
B. `count` reports occurrences in the interval; `rate` is normalized per second
C. `count` is for integers; `rate` is for floats
D. `rate` is only used by Agent integrations; `count` only by DogStatsD

**2.** Which metric type aggregates **per host** before submission, and which aggregates **globally on the Datadog backend**?
A. Histogram per host; Distribution global
B. Distribution per host; Histogram global
C. Both are per host
D. Both are global

**3.** A team wants accurate p95 latency across all 200 instances of a service. Which metric type is best?
A. Gauge
B. Count
C. Histogram
D. Distribution

**4.** The Unified Service Tagging tags are:
A. `team`, `service`, `release`
B. `env`, `service`, `version`
C. `cluster`, `pod`, `container`
D. `host`, `app`, `tier`

**5.** Which of the following is a CARDINALITY-DANGEROUS tag value?
A. `env:prod`
B. `service:checkout`
C. `request_id:abcdef-12345`
D. `region:us-east-1`

**6.** Which is the correct DogStatsD datagram for a **gauge** of value 42 with tag `queue:billing`?
A. `g:queue.size=42|billing`
B. `queue.size:42|g|#queue:billing`
C. `42 queue.size g queue:billing`
D. `queue.size:42|c|@queue:billing`

**7.** A new Agent integration adds a **service check** that reports OK or CRITICAL. Which numeric status code represents CRITICAL?
A. 0
B. 1
C. 2
D. 3

**8.** Where can you set tags so that **every metric** from a host is tagged the same way?
A. Only via DogStatsD `#` block
B. Only in integration `conf.yaml`
C. The `tags:` block in `datadog.yaml`
D. Only via Cloud Provider tags

**9.** Which of the following is NOT a valid place tags can come from?
A. Datadog UI manual host tagging
B. Container labels / pod annotations
C. Default DNS reverse lookups
D. AWS resource tags via the AWS integration

**10.** Which of the following is a **system metric** (free with the Agent) and not a custom metric?
A. `myapp.requests`
B. `system.cpu.user`
C. `checkout.duration`
D. `payment.errors`

**11.** A team submits `myapp.users{environment:prod, account_id:<unique-per-customer>}` for 50,000 customers. What is the issue?
A. None — Datadog handles unlimited cardinality
B. The high-cardinality `account_id` tag will inflate custom-metric billing
C. Tags can't have underscores
D. The metric name is too long

**12.** Which statement about events in Datadog is TRUE?
A. Events count toward custom-metrics billing
B. Events appear in the Events Explorer and can be overlaid on dashboards
C. Events can only be submitted via the API, never DogStatsD
D. Events are only for Datadog-internal use

**13.** Which of the following correctly applies USTagging to a Java app running in Docker?
A. `ENV DD_SERVICE="checkout"; ENV DD_ENV="prod"; ENV DD_VERSION="1.2.3"`
B. `ENV SERVICE="checkout"; ENV ENV="prod"; ENV VERSION="1.2.3"`
C. `ENV TAGS="env:prod,service:checkout,version:1.2.3"`
D. None — USTagging is set in the Datadog UI

**14.** Which of these statements about the Host Map is TRUE?
A. It only shows Kubernetes nodes
B. It is a hexagon-grid view; size and color can be driven by metrics, group/filter by tags
C. It auto-detects threats and security signals
D. It is read-only and can't be filtered

**15.** Which of the following is the correct in-app metric type produced by submitting via DogStatsD `histogram`?
A. A single GAUGE
B. A single DISTRIBUTION
C. Multiple metrics: `.avg`, `.median`, `.95percentile`, `.max`, `.count`, etc.
D. A single COUNT

**16.** What does `monotonic_count` ensure that a regular `count` does not?
A. Counts are emitted exactly once
B. The submitted value is treated as ever-increasing; resets to zero are ignored
C. Tags are de-duplicated
D. The metric is automatically distributed

**17.** Which two are true about events submitted via DogStatsD? (select TWO)
A. They use the format `_e{<title_len>,<text_len>}:<title>|<text>|...`
B. They are sent over TCP 8126
C. They support tags
D. They require app keys

**18.** Which metric query correctly returns the **per-host average CPU user** across only `env:prod`?
A. `system.cpu.user{env:prod}`
B. `avg:system.cpu.user{env:prod}`
C. `avg:system.cpu.user{env:prod} by {host}`
D. `system.cpu.user{env:prod} group_by host`

**19.** A team wants to attribute custom DogStatsD metrics to the **specific pod** they came from in a Kubernetes cluster. Which feature must be enabled?
A. Cluster Agent
B. DogStatsD origin detection (via UDS or specific config)
C. APM
D. Process Agent

**20.** Which of the following statements about the Infrastructure List is TRUE?
A. Hosts are billed when they're listed, regardless of activity
B. A host is "active" if it has reported metrics within the last 2 hours
C. The Infrastructure list shows only Linux hosts
D. The Infrastructure list does not show containers

---

## Answer Key

1. **B**
2. **A** — Histogram is per-host; Distribution aggregates globally on the backend.
3. **D** — Distribution for accurate cross-host percentiles.
4. **B** — `env`, `service`, `version`.
5. **C** — `request_id` is unbounded → cardinality blow-up.
6. **B**
7. **C** — 0=OK, 1=WARN, 2=CRIT, 3=UNKNOWN.
8. **C** — `tags:` block in `datadog.yaml`.
9. **C** — DNS reverse lookups are NOT a tag source.
10. **B** — System metrics are bundled.
11. **B** — Cardinality blow-up.
12. **B**
13. **A**
14. **B**
15. **C** — Multiple aggregated metrics per histogram.
16. **B** — Treated as monotonically increasing; resets ignored.
17. **A and C** — Format is correct; events support tags. Sent over UDP, not TCP.
18. **C** — `avg:` aggregator with `by {host}` returns per-host averages.
19. **B** — Origin detection (best with UDS).
20. **B** — Active = reported within ~2 hours.

> **Score: ___ / 20**  → if < 14 right, re-read **all of Domain 4** before moving on.
