# Simulations — Four formats

You asked for **all four** simulation formats. Here's how each one is structured and when to use it.

| Format | When to use | File location | Length | Difficulty mix |
|--------|-------------|---------------|--------|----------------|
| **Daily Mini-Quiz (10 Qs)** | Daily warm-up at the start of a study block | [`daily-mini-quiz/`](daily-mini-quiz/) | 10 Qs, ~10 min | Mostly easy + medium |
| **Domain-Specific Drill** | At the end of each weekly study block | [`domain-specific-drills/`](domain-specific-drills/) | 12–20 Qs per domain | Calibrated to that domain |
| **Adaptive Weak-Area Drill** | After any quiz/sim where a domain scored <80% | [`adaptive-weak-area-drills/`](adaptive-weak-area-drills/) | Variable, on-demand | Targeted at gaps |
| **Full Mock Exam (75 Qs / 2h)** | Once near end of Week 4, then optionally repeat | [`full-mock-exam/`](full-mock-exam/) | 75 scored Qs, 2 hours | ~30% easy / 50% medium / 20% hard |

## How they differ

- **Daily Mini-Quiz** keeps your retrieval muscle warm. It mixes domains so you don't lose facts from earlier weeks. ~10 min, no clock pressure.
- **Domain-Specific Drill** validates whether a week's study block stuck. Targeted feedback per domain; if you score <80%, do an adaptive drill before moving on.
- **Adaptive Weak-Area Drill** is **on-demand**: tell the cert-prep skill *"adaptive drill on retention periods"* (or any topic) and it generates a focused 10-question quiz from the knowledge base. The starter file in this folder explains how to invoke it.
- **Full Mock Exam** is the closest possible simulation of the real thing: 75 questions, 2 hours, single-answer multiple choice only, weighted across all four domains in the right proportions.

## Question weighting (matches official content outline)

| Domain | Weight | Mock Qs |
|--------|--------|---------|
| Domain 1 — APM Fundamentals | ~25% | 19 |
| Domain 2 — Application Instrumentation | ~30% | 22 |
| Domain 3 — Insight Discovery | ~30% | 22 |
| Domain 4 — Troubleshooting Application using APM | ~15% | 12 |
| **Total** | **100%** | **75** |

## Tracking your progress

After every simulation, log:

- Overall score
- Per-domain score
- The specific question numbers you got wrong
- The KB section to re-read

Use `progress/scoring-template.md` as the template.
