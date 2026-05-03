# Datadog Fundamentals — Study System

This folder contains everything you need to prepare for the **Datadog Fundamentals** certification:
a curated knowledge base, a 4-week × 2h-daily study plan, six per-domain quizzes, daily mini-quizzes, two full mock exams, and a weak-area drill workflow.

> Built for: **beginner**, **English**, **4-week target**, **comprehensive RAG depth**.

---

## Folder map

```
foundation/
├── README.md                            ← you are here
│
├── rag-knowledge-base/                  ← curated study notes (also usable as RAG)
│   ├── INDEX.md                         ← table of contents + topic-to-file mapping
│   ├── 01-computer-fundamentals/
│   │   └── 01-computer-fundamentals.md
│   ├── 02-infrastructure-agent/
│   │   ├── 01-agent-architecture.md
│   │   ├── 02-installation-platforms.md
│   │   └── 03-keys-sites-org.md
│   ├── 03-networking-config/
│   │   ├── 01-networking-firewall.md
│   │   ├── 02-integrations-checks.md
│   │   └── 03-dogstatsd.md
│   ├── 04-data-collection/
│   │   ├── 01-metrics-types.md
│   │   ├── 02-tagging-best-practices.md
│   │   └── 03-events-service-checks-host-map.md
│   ├── 05-agent-troubleshooting/
│   │   └── 01-troubleshooting-toolkit.md
│   ├── 06-visualization-monitors/
│   │   ├── 01-dashboards.md
│   │   └── 02-monitors.md
│   └── reference/
│       ├── quick-reference-card.md      ← print this for exam day
│       └── common-pitfalls.md           ← 20 traps the exam loves
│
├── study-plan/
│   └── study-plan.md                    ← day-by-day, 28 days × 2 h
│
├── quizzes/
│   ├── 01-computer-fundamentals-quiz.md (15 Q)
│   ├── 02-infrastructure-agent-quiz.md  (15 Q)
│   ├── 03-networking-config-quiz.md     (15 Q)
│   ├── 04-data-collection-quiz.md       (20 Q)  ← heavy domain
│   ├── 05-troubleshooting-quiz.md       (15 Q)
│   ├── 06-visualization-monitors-quiz.md (20 Q) ← heavy domain
│   ├── quiz-tagging-focus.md            (15 Q)  ← deep-focus drill
│   └── daily-mini-quizzes.md            (28 × 10 Q daily warm-ups)
│
├── simulations/
│   ├── mock-exam-1.md                   (75 Q / 90 min)  ← Day 21 / end of Week 3
│   ├── mock-exam-2.md                   (75 Q / 90 min)  ← Day 26 / end of Week 4
│   └── weak-area-drill-instructions.md  ← adaptive loop based on your scores
│
└── progress-tracking/
    ├── tracker.md                       ← daily check-in
    ├── results.md                       ← log every quiz/mock
    └── weak-areas.md                    ← running list of topics to revisit
```

---

## How to use this in a typical day

1. Open `study-plan/study-plan.md` and find today's section.
2. Read the listed RAG file(s) (~70 min).
3. Do the hands-on lab in your trial Datadog org (~30 min).
4. Take the day's mini-quiz from `quizzes/daily-mini-quizzes.md` (~20 min).
5. Mark the day in `progress-tracking/tracker.md`.
6. If anything was confusing, jot a note in `progress-tracking/weak-areas.md`.

## Milestones

| Day | Milestone |
|---|---|
| 7 | First domain drill (Domain 2 + 3 quizzes) |
| 14 | Domain 4 + tagging drill |
| 21 | **Mock Exam #1** (target ≥ 70%) |
| 26 | **Mock Exam #2** (target ≥ 80%) |
| 28 | Pre-exam light review |
| 29 | **EXAM DAY** |

## When something goes wrong

- **Schedule slipped:** see "Adjustments" at the bottom of `study-plan/study-plan.md`.
- **Score too low:** follow `simulations/weak-area-drill-instructions.md`.
- **Confused about a topic:** open `rag-knowledge-base/INDEX.md` and jump to the right file.
- **Concept feels missing:** the `reference/` folder is the quickest way to refresh foundational concepts.

## Hands-on environment

Sign up for the [14-day Datadog free trial](https://www.datadoghq.com/free-datadog-trial/) on Day 1. The trial covers everything in the exam scope. You can also:

- Run a single Linux VM (cloud or local Vagrant) for Agent installation.
- Use Docker Desktop for the container scenarios.
- Use `kind` or `minikube` for the Kubernetes scenarios.

## Beyond Fundamentals

Once you pass, the same study system pattern works for:
- APM & Distributed Tracing Fundamentals
- Log Management Fundamentals
- Database Monitoring Fundamentals
- Cloud SIEM for AWS Fundamentals

Each of those exams is 90 questions / 2 hours / $100. Your hands-on Datadog trial setup is reusable.

---

> **Source guide referenced:** Datadog Fundamentals Exam Guide (January 2026).
> **Documentation source:** Official Datadog documentation via Context7 (`/datadog/documentation`), synthesized into the RAG files.
> **Last refreshed:** May 2026.
