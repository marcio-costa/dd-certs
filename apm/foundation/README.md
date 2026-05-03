# Foundation — Datadog APM & Distributed Tracing Certification Prep

> Everything you need to pass the **Datadog APM & Distributed Tracing Fundamentals** exam in 4 weeks at 2 hours per day.

## What's in this folder

```
foundation/
├── README.md                           ← you are here
├── exam-guide-summary.md               ← official content outline distilled
│
├── knowledge-base/                     ← RAG: official Datadog docs as markdown
│   ├── README.md                       ← index by domain
│   ├── 01-apm-fundamentals/            (5 files)
│   ├── 02-instrumentation/             (6 files)
│   ├── 03-insight-discovery/           (8 files)
│   └── 04-troubleshooting/             (3 files)
│
├── study-plan/                         ← 2h/day, 4-week plan with daily blocks
│   ├── README.md
│   ├── week-1-apm-fundamentals.md
│   ├── week-2-instrumentation.md
│   ├── week-3-insight-discovery.md
│   ├── week-4-troubleshooting-and-review.md
│   ├── hands-on-lab-guide.md           ← Datadog free trial setup
│   └── exam-day-checklist.md
│
├── quizzes/                            ← reinforcement (paired with study plan)
│   ├── README.md
│   ├── domain-1-apm-fundamentals.md    (20 Qs)
│   ├── domain-2-instrumentation.md     (20 Qs)
│   ├── domain-3-insight-discovery.md   (23 Qs)
│   └── domain-4-troubleshooting.md     (15 Qs)
│
├── simulations/                        ← all 4 simulation formats
│   ├── README.md
│   ├── adaptive-weak-area-drills/      ← on-demand from cert-prep skill
│   ├── domain-specific-drills/         (4 drills, 12–20 Qs each)
│   ├── full-mock-exam/                 (75 Qs / 2 hours, full simulation)
│   └── daily-mini-quiz/                (10-Q warmups; 3 ready, more on demand)
│
└── progress/                           ← your tracking workspace
    ├── daily-tracker.md
    ├── scoring-template.md
    └── exam-results.md
```

## How to use this in order

1. **Read** `exam-guide-summary.md` to internalize the official content outline.
2. **Open** `study-plan/README.md` and start Week 1.
3. Each day, follow the 4-block 2-hour structure:
   - **30 min** — Read the assigned `knowledge-base/` file.
   - **45 min** — Hands-on lab (use `study-plan/hands-on-lab-guide.md`).
   - **30 min** — Answer the corresponding `quizzes/` block.
   - **15 min** — Update `progress/daily-tracker.md`.
4. **End of each week** — Take the matching `simulations/domain-specific-drills/`.
5. **End of Week 4** — Take `simulations/full-mock-exam/mock-exam-1-questions.md` under timed conditions.
6. **Anytime** weak — Run `simulations/daily-mini-quiz/` or ask the cert-prep skill for an **adaptive weak-area drill**.

## Quick commands (via the cert-prep skill)

The `datadog-cert-prep` skill is loaded — invoke any of these any time:

- `quiz me on tagging` — generate a fresh focused quiz.
- `adaptive drill on retention periods` — targeted weak-area practice.
- `daily mini-quiz` — fresh 10-Q warmup.
- `full mock exam` — generate another 75-Q full simulation when you've burned through the existing one.
- `explain DD_TRACE_SAMPLING_RULES` — KB-backed Q&A.
- `refresh docs` — re-pull from official Datadog docs (if any KB file feels outdated).

## What you'll know after 4 weeks

- The four exam domains end-to-end (APM Fundamentals, Instrumentation, Insight Discovery, Troubleshooting).
- The full Datadog APM tagging, sampling, and retention model.
- Every instrumentation type (SSI, Automatic, Manual, Dynamic) and when to use which.
- The Trace Explorer, Software Catalog, Service Page, Profiler, Error Tracking, Span Summary.
- How to pick the right monitor type for any APM scenario.

## Source attribution

- Knowledge base content sourced from `/datadog/documentation` via Context7 (May 2026 snapshot).
- Exam structure follows the **APM & Distributed Tracing Certification Exam Guide 2025** (uploaded PDF).
- Quiz/simulation content is original, calibrated to match the official exam format and difficulty.

## Owner

Marcio Acosta — building toward the cert in 4 weeks.

## Last updated

May 3, 2026.
