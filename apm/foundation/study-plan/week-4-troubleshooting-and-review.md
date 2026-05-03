# Week 4 — Troubleshooting + Review + Full Mock Exam

> Goal: bring the whole picture together, drill weak areas, and pass the full 75-question mock under timed conditions.

| Day | Block 1 — Read | Block 2 — Lab | Block 3 — Quiz / Sim | Block 4 — Reflect |
|-----|------------------|---------------|----------------------|--------------------|
| **Mon** | KB §4.1 Trace Search During Incidents + KB §4.3 Trace ↔ Log Correlation | Inject a synthetic latency spike in the sample app (`time.sleep(3)` on one endpoint). Use the incident playbook end-to-end. Enable `DD_LOGS_INJECTION=true` and verify trace IDs appear in log lines. | Quiz: `quizzes/domain-4-troubleshooting.md` (Q1–Q8) | Write the incident playbook from memory in 10 steps. |
| **Tue** | KB §4.2 Monitors & Alerting | Create one of each monitor: APM Metric (threshold), APM Trace Analytics, Anomaly Monitor, Error Tracking monitor. Schedule a downtime. | Quiz: Q9–Q15 | Compare anomaly vs Watchdog in writing. |
| **Wed** | Re-read weakest 3 KB files from previous weeks. | Re-do hands-on for any concept that's still fuzzy. | **Domain 4 mini-mock** (12 questions) — `simulations/domain-specific-drills/drill-domain-4.md` + **Daily mini-quiz**. | Update weakness map. |
| **Thu** | Skim every KB file. Focus on numbers (ports, retention, defaults). | Take the **Datadog official 25-question practice exam** (free, on learn.datadoghq.com). | Review all explanations from official practice exam. | Note any new gaps. |
| **Fri** | Light review only. | None — rest the brain. | **Full Mock Exam (75 Qs / 2h)** — `simulations/full-mock-exam/mock-exam-1-questions.md`. Time yourself strictly. | Score per domain. Run **adaptive weak-area drills** on any domain <80%. |
| **Sat** | Targeted review of weak topics. | Re-do the Datadog official practice exam if available. | One last **daily mini-quiz**. | Sleep early. |
| **Sun** | **Exam day** if scheduled — see `exam-day-checklist.md`. | — | — | Celebrate. |

## Success criteria before sitting the exam

- [ ] ≥80% on the full 75-question mock exam.
- [ ] ≥80% on each individual domain mini-mock.
- [ ] You can answer any "what is X" question for every line of the official content outline.
- [ ] You can pick the right APM feature for any scenario.
- [ ] You feel calm: you've slept 8h, eaten, and arrived at the test 15 min early.

## After the exam

- Update `progress/exam-results.md` with your actual score.
- If you passed, share the cert link from Datadog Credly.
- If you didn't, you have up to **3 retakes within 180 days** — focus the redo plan on the lowest-scoring domains.
