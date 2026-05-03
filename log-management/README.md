# Datadog Log Management Fundamentals — Study System

Complete prep system for the **Datadog Log Management Fundamentals** certification, built from the official January-2026 exam guide and Datadog's documentation (sourced via Context7).

> Exam at a glance: 90 questions (75 scored + 15 pretest), 2 hours seat time, multiple-choice only, 3-year credential validity, $100 USD via Kryterion Webassessor.

**Built for:** 2 hours/day · 4-week timeline · English (US) · comprehensive coverage of all 7 exam domains.

---

## Folder map

```
foundation/log-management/
├── README.md                                   ← you are here
│
├── knowledge-base/                             ← curated study notes (RAG-ready)
│   ├── INDEX.md                                ← navigation + topic-to-file lookup
│   ├── 01-logging-fundamentals.md              ← Domain 1
│   ├── 02-log-collection.md                    ← Domain 2
│   ├── 03-log-parsing.md                       ← Domain 3
│   ├── 04-search-filtering.md                  ← Domain 4
│   ├── 05-log-analysis.md                      ← Domain 5
│   ├── 06-log-utilization.md                   ← Domain 6
│   ├── 07-log-troubleshooting.md               ← Domain 7
│   ├── glossary.md                             ← A–Z term reference
│   └── quick-reference.md                      ← exam-day cheat sheet
│
├── study-plan/
│   └── study-plan.md                           ← day-by-day, 28 days × 2h
│
├── quizzes/                                    ← reinforcement (12 Qs each)
│   ├── quiz-01-logging-fundamentals.md
│   ├── quiz-02-log-collection.md
│   ├── quiz-03-log-parsing.md
│   ├── quiz-04-search-filtering.md
│   ├── quiz-05-log-analysis.md
│   ├── quiz-06-log-utilization.md
│   └── quiz-07-log-troubleshooting.md
│
├── simulations/
│   ├── README.md                               ← which simulation when
│   ├── adaptive-weak-area-drill.md             ← recipe to assemble a 25-Q targeted drill
│   ├── daily-mini-quizzes/                     ← 10 Qs each, 7 days
│   │   ├── day-01.md … day-07.md
│   ├── domain-drills/                          ← 15 hard/scenario Qs per domain
│   │   ├── drill-01-logging-fundamentals.md
│   │   └── … drill-07-log-troubleshooting.md
│   └── full-mock-exams/                        ← 75 Qs each, 2-hour exam-realistic
│       ├── mock-exam-1.md
│       └── mock-exam-2.md
│
└── progress-tracking/
    ├── tracker.md                              ← daily fill-in template (tied to the plan)
    └── weak-areas.md                           ← running log of concepts to revisit
```

**38 markdown files · ~6,300 lines · 4 simulation formats.**

---

## Quick start (read this and go)

1. **Read** `knowledge-base/01-logging-fundamentals.md` and `02-log-collection.md` (Day 1–5 of the plan).
2. **Open** `study-plan/study-plan.md` and copy the **Daily template** at the bottom into `progress-tracking/tracker.md` for each day.
3. **Take quizzes** as you go — every domain has one in `quizzes/`. Add wrong answers to `progress-tracking/weak-areas.md`.
4. **Run a daily mini-quiz** every Saturday and Sunday in Weeks 1–2.
5. **Take Mock Exam #1** on Day 14 (under exam conditions). Grade Day 15.
6. **Switch to drills** in Week 4: domain-specific drills + adaptive weak-area drills.
7. **Take Mock Exam #2** on Day 25. If you score ≥ 65/75, you're ready.

---

## The four simulation formats

| Format | Scope | Length | When |
|---|---|---|---|
| **Daily mini-quiz** | Mixed-domain · 10 Qs · ~12 min | 7 days available | Every Sat/Sun + key consolidation days |
| **Domain-specific drill** | Single domain · 15 hard Qs · ~25 min | One per domain | After studying that domain end-to-end |
| **Adaptive weak-area drill** | Custom 25-Q assembled from your weakest 2 domains | ~30 min | Whenever weak-areas log has 5+ items |
| **Full mock exam** | Exam-realistic · 75 Qs · 2 hours | Two distinct exams | Day 14 (mid) + Day 25 (final) |

---

## Knowledge-base (RAG) coverage

Each domain file is self-contained markdown ready for ingestion into any RAG retriever (Cursor, Continue, Claude Projects, Notion AI, custom ChromaDB, etc.). Headings are exam-aligned; bullets and tables are dense; code/config snippets are real Datadog YAML & query syntax.

**Source documentation referenced:**

- `docs.datadoghq.com/logs/` — Log Management overview
- `docs.datadoghq.com/agent/logs/` — Host Agent Log Collection
- `docs.datadoghq.com/agent/configuration/agent-configuration-files/` — `datadog.yaml` reference
- `docs.datadoghq.com/getting_started/tagging/` — Tags & Unified Service Tagging
- `docs.datadoghq.com/logs/log_configuration/` — pipelines, processors, indexes, archives
- `docs.datadoghq.com/logs/explorer/` — Explorer, search syntax, facets, saved views, analytics, live tail
- `docs.datadoghq.com/monitors/types/log/` — log monitors
- `docs.datadoghq.com/logs/log_configuration/logs_to_metrics/` — generate metrics from logs
- `docs.datadoghq.com/sensitive_data_scanner/` — server-side scrubbing
- `docs.datadoghq.com/logs/troubleshooting/` — troubleshooting

> **Note on data sourcing:** The Datadog documentation site itself is on the Cowork environment's egress block list, so the knowledge base was assembled by querying Context7's mirror of `/datadog/documentation` (the official Datadog GitHub doc repo) and merging with the official exam-guide PDF you uploaded. If you want to refresh from the live docs in a different environment, the section URLs above are the authoritative source.

---

## Tracking your progress

Two files in `progress-tracking/` keep you honest:

- **`tracker.md`** — A pre-built day-by-day checklist mirroring the study plan. Fill it in each day; it includes scoring boxes for every quiz/mock and a mid-/end-of-week reflection space.
- **`weak-areas.md`** — A running log of every concept you've gotten wrong. Each entry has Open → Reviewed → Mastered status. Drives the **Adaptive Weak-Area Drill**.

If you skip these, you'll re-learn the same things three times. Use them.

---

## Quick command reference

| Need | Where to look |
|---|---|
| What does *Logging Without Limits* mean? | `knowledge-base/01-logging-fundamentals.md` §1.1 |
| How do I enable log collection? | `knowledge-base/02-log-collection.md` §2.1 |
| Difference between `mask_sequences` and Sensitive Data Scanner | `knowledge-base/02-log-collection.md` §2.5 + §2.6 |
| Recommended pipeline processor order | `knowledge-base/03-log-parsing.md` §3.2 |
| Standard attribute namespaces (`@http.*`, `@network.*`, etc.) | `knowledge-base/03-log-parsing.md` §3.4 |
| Live Tail vs. Log Explorer | `knowledge-base/04-search-filtering.md` §4.1 |
| Range query syntax (`[ ]` vs. `{ }`) | `knowledge-base/04-search-filtering.md` §4.2 |
| Index inclusion vs. exclusion | `knowledge-base/05-log-analysis.md` §5.1 |
| Logs to Metrics setup | `knowledge-base/06-log-utilization.md` §6.2 |
| Archive → Rehydration flow | `knowledge-base/06-log-utilization.md` §6.3 |
| First troubleshooting steps | `knowledge-base/07-log-troubleshooting.md` §7.1–7.2 |
| Exam-day cheat sheet | `knowledge-base/quick-reference.md` |

---

## Domain weighting (used across simulations)

The exam content outline doesn't publish exact percentages. We've estimated based on subtopic counts:

| Domain | Approx. weight | Mock-exam Qs |
|---|---|---|
| Logging Fundamentals | ~15% | 11/75 |
| Log Collection | ~12% | 9/75 |
| Log Parsing | ~19% | 14/75 |
| Log Searching & Filtering | ~17% | 13/75 |
| Log Analysis | ~13% | 10/75 |
| Log Utilization | ~15% | 11/75 |
| Log Troubleshooting | ~9% | 7/75 |

If your real exam differs, adjust your post-mock review accordingly (and update `weak-areas.md` for new patterns).

---

## How to refresh this material

When the exam guide changes (Datadog updates them periodically) or you revisit prep in a year:

1. Re-read the latest official exam guide PDF and compare to the **Exam content outline** at the start of each knowledge-base file.
2. If subtopics changed: update `knowledge-base/INDEX.md` and the affected domain files.
3. If only emphasis changed: re-balance the per-domain question counts in the mock exams.
4. If a Datadog feature behavior changed (e.g., new processors, new visualizations): re-query the relevant Datadog doc page (or Context7 `/datadog/documentation`) and patch the knowledge-base file.
5. Bump the date stamp at the top of `knowledge-base/INDEX.md`.

---

## Good luck

You have ~52 hours of structured work in front of you. Most candidates pass with focused 4-week prep at 2h/day. The single biggest predictor is **using the tracker and weak-areas log religiously** — the materials don't help if you don't measure where they aren't sticking.

When you sit the exam, scan `quick-reference.md` once that morning, drink water, sit early afternoon if you can choose, and trust the prep.
