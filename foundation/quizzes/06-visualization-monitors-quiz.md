# Quiz — Domain 6: Visualization & Monitors (20 Qs — heavily weighted domain)

> Time: ~20 min.

---

**1.** A team needs a dashboard where some widgets show the last 1h and others the last 24h, and where they can drag widgets freely. Which layout fits best?
A. Ordered (Timeboard-style)
B. Free (Screenboard-style)
C. List
D. Stack

**2.** Which widget would you use to display a single big "current value" number?
A. Top List
B. Query Value
C. Heatmap
D. Note

**3.** Template variables are used to:
A. Permanently rename a metric
B. Parameterize widget queries so users can switch values like `env`, `service`, `region`
C. Create a backup of the dashboard
D. Schedule the dashboard PDF report

**4.** Which monitor type alerts when a metric **deviates from its learned baseline**?
A. Forecast
B. Outlier
C. Anomaly
D. Composite

**5.** Which monitor type predicts a future value crossing a threshold?
A. Forecast
B. Outlier
C. Anomaly
D. Change

**6.** Which monitor type combines multiple monitors with boolean logic?
A. Composite
B. Multi-alert
C. Forecast
D. Time-slice

**7.** A monitor query `avg(last_5m): system.cpu.user{env:prod} by {host} > 80` will alert:
A. Once when ANY host exceeds 80
B. Per host that exceeds 80 (multi-alert)
C. Only when ALL hosts exceed 80
D. Only when the cluster average exceeds 80

**8.** When a `count`-typed metric query is used in a monitor and you want the threshold to be in **absolute occurrences over 5 minutes**, you should append:
A. `.as_rate()`
B. `.as_count()`
C. `.rollup(sum, 300)`
D. `.fill(zero)`

**9.** Which notification handle pages a Slack channel called `#alerts` in workspace `acme`?
A. `@slack:acme:alerts`
B. `@acme.alerts`
C. `@slack-acme-alerts`
D. `@page-acme:alerts`

**10.** Which template variable in a monitor message renders the **current metric value** that triggered the alert?
A. `{{value}}`
B. `{{current.value}}`
C. `{{val}}`
D. `{{metric.value}}`

**11.** Which two are characteristics of a Notebook? (select TWO)
A. Live cells that re-run on open
B. Markdown cells with optional Mermaid diagrams
C. Auto-creates monitors for every metric used
D. Replaces the Service Catalog

**12.** A team wants to mute monitors **every weekend** for `env:staging`. Which mechanism implements this?
A. A `downtime` with an RRULE like `FREQ=WEEKLY;BYDAY=SA,SU` and a 48h duration
B. Deleting the monitor each Friday
C. Lowering the threshold every weekend
D. Switching the notification channel to email

**13.** Which best describes a **metric-based SLO**?
A. Two metric queries: a "good events" numerator and a "total events" denominator, evaluated over a window
B. A monitor that fires when the SLO is met
C. A single threshold on a metric
D. A composite of two existing monitors

**14.** An error-budget alert query for SLO `abc123` over the last 30 days, alerting when more than 75% is consumed, looks like:
A. `error_budget("abc123").over("30d") > 75`
B. `slo:abc123 > 0.25`
C. `consumption(abc123 30d) > .75`
D. `slo("abc123").budget(30) > 75`

**15.** Which widget is best for visualizing the distribution of values within a window (e.g., latency buckets)?
A. Top List
B. Heat Map / Distribution
C. Note
D. Hostmap

**16.** A monitor in **No Data** state will alert by default:
A. Yes, always
B. No, you must enable "Notify if no data" in the monitor configuration
C. Only on the recovery
D. Only if the metric is system metric

**17.** Which monitor type would you use to alert when a single Kafka broker has higher consumer lag than its peers?
A. Anomaly
B. Outlier
C. Forecast
D. Change

**18.** Which dashboard widget shows a hex-grid of hosts colored by metric value?
A. Heatmap
B. Hostmap
C. Top List
D. Service Map

**19.** Which of the following is the correct query operator that returns the largest value in the time window for use in monitor evaluation?
A. `max()`
B. `top()`
C. `peak()`
D. `last()`

**20.** Which is TRUE about the difference between a Time-slice SLO and a Metric-based SLO?
A. Time-slice SLOs require two queries; Metric-based requires one
B. Time-slice SLOs measure the fraction of evaluation intervals meeting a condition; Metric-based use numerator/denominator queries
C. They are identical, just renamed
D. Time-slice only works for synthetics

---

## Answer Key

1. **B** — Free layout (Screenboard-style).
2. **B** — Query Value.
3. **B**
4. **C** — Anomaly.
5. **A** — Forecast.
6. **A** — Composite.
7. **B** — Multi-alert per host because of `by {host}`.
8. **B** — `.as_count()` for absolute counts.
9. **C** — `@slack-<workspace>-<channel>`.
10. **A** — `{{value}}`.
11. **A and B** — Live cells; Markdown + Mermaid.
12. **A**
13. **A**
14. **A**
15. **B** — Heat Map or Distribution widget.
16. **B**
17. **B** — Outlier.
18. **B** — Hostmap.
19. **A** — `max()` for "at any point in window".
20. **B**

> **Score: ___ / 20**  → if < 14 right, re-read all of Domain 6.
