# Week 3 — Insight Discovery (Domain 3)

> Goal: navigate the APM UI like an expert. Software Catalog, Trace Explorer, search syntax, profilers, deployments, error tracking, span summary, dashboards.

| Day | Block 1 — Read | Block 2 — Lab | Block 3 — Quiz | Block 4 — Reflect |
|-----|------------------|---------------|----------------|--------------------|
| **Mon** | KB §3.1 Software Catalog + KB §3.8 Service Performance Dashboards | In the Datadog UI, open Software Catalog. Add an `entity.datadog.yaml` for one of your services. Then open the Service Page and explore every tab. | Quiz: `quizzes/domain-3-insight-discovery.md` (Q1–Q5) | List every Service Page tab from memory. |
| **Tue** | KB §3.2 Search Syntax + KB §3.3 Live vs Retained Search | Build 10 different queries in the Trace Explorer. Toggle between Live (15 min) and Indexed (1 day) and observe which tags are searchable. | Quiz: Q6–Q12 | Write each operator (AND, OR, NOT, wildcard, range, exists) with an example. |
| **Wed** | KB §3.4 Profilers (UI usage) + KB §3.5 Deployment Tracking | Generate two distinct `DD_VERSION` deploys with different latency profiles. Use the Service Page Deployments tab and compare versions side-by-side, including profile diff. | Quiz: Q13–Q18 | Note which auto-version source kicked in. |
| **Thu** | KB §3.6 Error Tracking | Throw three different exceptions in the sample app, then one duplicate of the first. Open Error Tracking and confirm grouping. Add `error.fingerprint` to override grouping. Resolve an issue, redeploy with the bug, watch it auto-reopen. | Quiz: Q19–Q23 | Write the fingerprint formula from memory. |
| **Fri** | KB §3.7 Span Summary | Open Span Summary on the busiest service. Identify the span with the highest "% execution time". Drill into traces from that span row. | **Domain 3 mini-mock** (20 questions) — `simulations/domain-specific-drills/drill-domain-3.md` | Score; flag any topic <80%. |
| **Sat** | Optional — re-read weak areas. | Hands-on capstone: build a custom dashboard with: (a) Service Summary widget, (b) Top Resources by error rate, (c) Profiling Flame Graph widget, (d) Deployment-version overlay. | Optional **daily mini-quiz**. | None. |
| **Sun** | Rest. | — | — | Plan Week 4. |

## Success criteria for Week 3

- [ ] You can write a working trace search query for any scenario.
- [ ] You know exactly when to use Live vs Indexed Search.
- [ ] You can read and explain a flame graph and a span summary table.
- [ ] You can configure deployment tracking and run a version comparison.
- [ ] You can explain how Error Tracking groups errors and how to override grouping.
- [ ] You scored ≥80% on the Domain 3 mini-mock.
