# Week 1 — APM Fundamentals (Domain 1)

> Goal: build a rock-solid mental model for what APM is, the architecture, tagging, and retention. Everything else builds on this week.

| Day | Block 1 (30 min) — Read | Block 2 (45 min) — Lab | Block 3 (30 min) — Quiz | Block 4 (15 min) — Reflect |
|-----|--------------------------|------------------------|--------------------------|----------------------------|
| **Mon** | KB §1.1 APM Rationale + KB §1.2 Tracing Architectures | Sign up for Datadog 14-day free trial. Install Datadog Agent on a Linux VM or Docker. Confirm `agent status` shows trace-agent listening on `8126`. | Quiz: `quizzes/domain-1-apm-fundamentals.md` (Q1–Q5) | Note unfamiliar terms in `progress/daily-tracker.md`. |
| **Tue** | KB §1.3 Tagging & Unified Service Tagging | Set `DD_ENV`, `DD_SERVICE`, `DD_VERSION` on a sample app and confirm tags appear in the Service Page. Try changing `DD_VERSION` and watching the Deployments tab. | Quiz: `quizzes/domain-1-apm-fundamentals.md` (Q6–Q10) | Write USM env-var triple from memory. |
| **Wed** | KB §1.4 Retention Periods for APM Data | In the Datadog UI, open Trace Explorer → Live (15 min). Switch to a 1-day window to see Indexed mode. Open `Setup APM → Retention Filters` to inspect default + intelligent filters. | Quiz: Q11–Q15 | Memorize the four numbers: 15 min, 15 days, 30 days, 15 months. |
| **Thu** | KB §1.5 OOTB vs Community Tracer + Language Differences | Pick your strongest language. Install the Datadog tracer (`pip install ddtrace` / `npm install dd-trace` / `dd-java-agent.jar`) and run the official "Hello, traces" example. | Quiz: Q16–Q20 | Identify which language you'd default to, and why Go is "different". |
| **Fri** | Re-read all five KB files (skim) | Lab consolidation: a) inject custom span tag (`order.id`); b) confirm it appears in the Trace Explorer using `@order.id`. | **Domain 1 mini-mock** (15 questions) — `simulations/domain-specific-drills/drill-domain-1.md` | Score yourself; flag anything <80% for the weekend. |
| **Sat** | Optional — re-read weak areas. | Hands-on capstone: run the **OpenTelemetry demo** (`opentelemetry-demo`) instrumented with the Datadog exporter; explore the Service Map. | Optional **daily mini-quiz** (10 Qs). | None. |
| **Sun** | Rest, or do the Datadog Learning Center "APM Fundamentals" course. | — | — | Plan Week 2. |

## Success criteria for Week 1

- [ ] You can draw the path: App → Tracer → Agent (port 8126) → Datadog backend.
- [ ] You can recite USM (`env`, `service`, `version`) and explain why each matters.
- [ ] You can state the four retention numbers without lookup.
- [ ] You can list 3 differences between OOTB and community tracers.
- [ ] You scored ≥80% on the Domain 1 mini-mock.

## If you scored below 80%

Run the **adaptive weak-area drill** (`simulations/adaptive-weak-area-drills/`) targeting Domain 1 before starting Week 2.
