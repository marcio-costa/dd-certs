# Study Plan — Datadog Log Management Fundamentals

**Daily commitment:** 2 hours
**Total length:** 4 weeks (28 calendar days, 26 study days + 2 buffer/exam days)
**Total study time:** ~52 hours
**Target outcome:** Pass the 90-question Log Management Fundamentals exam (75 scored + 15 pretest, 2 h)

---

## How to use this plan

Each day has a fixed structure to maximize retention without burnout:

```
0:00 – 0:50    Read & take notes      (50 min)  ← absorb the knowledge-base file
0:50 – 1:00    Break                  (10 min)
1:00 – 1:30    Hands-on / docs deep   (30 min)  ← Datadog UI or extra docs
1:30 – 1:50    Quiz / drill           (20 min)
1:50 – 2:00    Reflection notes       (10 min)  ← 3 bullets in your tracker
```

**Tracker file:** `../progress-tracking/tracker.md` (created in this plan).

**Weak-areas file:** `../progress-tracking/weak-areas.md` — log every concept you got wrong; it drives Week-4 review.

**Hands-on environment:** sign up for the Datadog 14-day free trial early in Week 1 so it's still active for hands-on labs through Week 3.

---

## Week 1 — Foundations & Collection

> **Focus:** the *what* and *how it gets in*. By end of week, you can explain Logging Without Limits and configure the Agent.

### Day 1 (Mon) — Domain 1, part 1: Why logs, sources, formats

- **Read (50m):** `knowledge-base/01-logging-fundamentals.md` §1.1 – §1.3
- **Hands-on (30m):** Sign up for Datadog 14-day trial · install Agent on a local VM/container · open the Logs landing page
- **Quiz (20m):** First 5 Qs of `quizzes/quiz-01-logging-fundamentals.md`
- **Reflect:** 3 bullets — what surprised you, what was already familiar, what to ask Day 2

### Day 2 (Tue) — Domain 1, part 2: Emission & lifecycle

- **Read (50m):** `01-logging-fundamentals.md` §1.4 – §1.8
- **Hands-on (30m):** Emit a sample JSON log from a script · POST one log to the HTTP Logs Intake API with `curl`
- **Quiz (20m):** Remaining Qs of `quiz-01`; review missed items
- **Reflect:** 3 bullets — what you can already explain to a colleague

### Day 3 (Wed) — Domain 2, part 1: Enabling collection

- **Read (50m):** `02-log-collection.md` §2.1 – §2.3
- **Hands-on (30m):** Enable `logs_enabled: true` · tail `/var/log/syslog` · verify in Live Tail
- **Quiz (20m):** First half of `quiz-02-log-collection.md`
- **Reflect:** Find one config option in `datadog.yaml` you didn't know existed

### Day 4 (Thu) — Domain 2, part 2: Cloud & containers

- **Read (50m):** `02-log-collection.md` §2.4 (cloud) and read official Datadog docs on Kubernetes log collection (annotations)
- **Hands-on (30m):** Run a Docker container with a log source label · or read pod-annotation examples and write one yourself
- **Quiz (20m):** Second half of `quiz-02`
- **Reflect:** Note differences between Docker + K8s collection

### Day 5 (Fri) — Domain 2, part 3: Filtering & obfuscation

- **Read (50m):** `02-log-collection.md` §2.5 – §2.8
- **Hands-on (30m):** Add an `exclude_at_match` rule on a healthcheck path · add a `mask_sequences` rule for credit cards · trigger and verify
- **Quiz (20m):** Re-do any wrong answers from `quiz-02`; full re-attempt of weakest 5 Qs
- **Reflect:** Add anything still fuzzy to `weak-areas.md`

### Day 6 (Sat) — Domain 1+2 consolidation

- **Read (40m):** Re-skim `01-logging-fundamentals.md` and `02-log-collection.md` highlights only
- **Hands-on (40m):** Configure log collection for one new source (your favorite app or `nginx` running locally)
- **Quiz (30m):** **Daily mini-quiz #1** (`simulations/daily-mini-quizzes/day-01.md`)
- **Reflect:** 10 min

### Day 7 (Sun) — Rest / catch-up

- 2 h reserved as **buffer** if you skipped a day, or rest if on track. Light reading: skim the official exam guide PDF again.

---

## Week 2 — Parsing & Search

> **Focus:** transforming raw logs into structured data and querying them. The heaviest content week.

### Day 8 (Mon) — Domain 3, part 1: Pipelines & processors

- **Read (50m):** `03-log-parsing.md` §3.1 – §3.2
- **Hands-on (30m):** In the Datadog UI, navigate *Logs → Configuration → Pipelines* · clone an integration pipeline (e.g., NGINX) · examine its processors
- **Quiz (20m):** First third of `quiz-03-log-parsing.md`
- **Reflect:** 3 bullets

### Day 9 (Tue) — Domain 3, part 2: Grok parser deep dive

- **Read (50m):** `03-log-parsing.md` §3.3
- **Hands-on (30m):** Build a Grok parser in the UI for a sample log line · use match rules + a support rule · save and verify with sample logs
- **Quiz (20m):** Second third of `quiz-03`
- **Reflect:** Save 3 working Grok patterns into your notes

### Day 10 (Wed) — Domain 3, part 3: Standard attributes & composition

- **Read (50m):** `03-log-parsing.md` §3.4 – §3.8
- **Hands-on (30m):** Open *Logs → Configuration → Standard Attributes* · add a custom standard attribute · write a remapper that aligns one of your custom JSON keys to `@http.status_code`
- **Quiz (20m):** Last third of `quiz-03`
- **Reflect:** Note three standard attribute namespaces you could use immediately

### Day 11 (Thu) — Domain 4, part 1: Live Tail vs. Explorer & search syntax

- **Read (50m):** `04-search-filtering.md` §4.1 – §4.2
- **Hands-on (30m):** Open Live Tail · run 5 different queries · open Log Explorer · re-run them; compare result counts
- **Quiz (20m):** First half of `quiz-04-search-filtering.md`
- **Reflect:** 3 query patterns you'll reuse

### Day 12 (Fri) — Domain 4, part 2: Facets, saved views, patterns, transactions

- **Read (50m):** `04-search-filtering.md` §4.3 – §4.7
- **Hands-on (30m):** Create a custom facet · save a view · explore the Patterns tab · build a Transaction grouped by an attribute
- **Quiz (20m):** Second half of `quiz-04`
- **Reflect:** Note which features you'd use in real work

### Day 13 (Sat) — Domain 3+4 consolidation

- **Read (40m):** Skim `03` and `04` highlights
- **Hands-on (50m):** End-to-end mini-project — ingest a custom JSON log · write a pipeline that extracts fields · search with facets in Explorer
- **Quiz (20m):** **Daily mini-quiz #2** (`simulations/daily-mini-quizzes/day-02.md`)
- **Reflect:** 10 min

### Day 14 (Sun) — **Mid-point milestone: Mock Exam #1**

- **2 h:** Take **Full Mock Exam #1** (75 Qs, 2-hour timer) — `simulations/full-mock-exams/mock-exam-1.md`
- After: **Don't grade it yet.** Walk away. Grade the next morning fresh.

---

## Week 3 — Analysis, Utilization & Troubleshooting

> **Focus:** what you do *with* logs, and how to fix it when things break.

### Day 15 (Mon) — Grade Mock #1, weak-area planning

- **Grade (40m):** `simulations/full-mock-exams/mock-exam-1.md` answer key · score per domain
- **Plan (30m):** Identify your bottom-2 domains · add objectives to `weak-areas.md` · re-prioritize Days 22–25 of this plan accordingly
- **Read (30m):** Re-read whichever knowledge-base file scored lowest
- **Quiz (20m):** Re-take the matching domain quiz
- **Reflect:** 3 bullets on what surprised you in your score

### Day 16 (Tue) — Domain 5, part 1: Filtering & excluding

- **Read (50m):** `05-log-analysis.md` §5.1 – §5.2
- **Hands-on (30m):** Open *Logs → Configuration → Indexes* · examine inclusion/exclusion filters · create a test exclusion filter at 50% sampling
- **Quiz (20m):** First half of `quiz-05-log-analysis.md`
- **Reflect:** 3 bullets

### Day 17 (Wed) — Domain 5, part 2: Aggregations & visualizing

- **Read (50m):** `05-log-analysis.md` §5.3 – §5.6
- **Hands-on (30m):** Build a Top List, Timeseries, Table, and Tree Map of the same query in Explorer · note group-by limits
- **Quiz (20m):** Second half of `quiz-05`
- **Reflect:** Build one log-based dashboard widget

### Day 18 (Thu) — Domain 6, part 1: Monitors

- **Read (50m):** `06-log-utilization.md` §6.1
- **Hands-on (30m):** Create a log monitor (threshold + multi-alert by `service`) · test with a noisy query · note the templating
- **Quiz (20m):** First third of `quiz-06-log-utilization.md`
- **Reflect:** 3 bullets

### Day 19 (Fri) — Domain 6, part 2: Generate Metrics & Logs Without Limits

- **Read (50m):** `06-log-utilization.md` §6.2 – §6.4
- **Hands-on (30m):** *Logs → Generate Metrics* · create a metric from logs (count + group by service); query it in Metric Explorer
- **Quiz (20m):** Second third of `quiz-06`
- **Reflect:** Confirm you understand the difference between exclusion and edge filtering

### Day 20 (Sat) — Domain 6, part 3: Exporting (archives, rehydration, forwarding)

- **Read (50m):** `06-log-utilization.md` §6.3 – §6.7
- **Hands-on (30m):** Walk the *Archives* configuration screen · note the IAM/role steps for S3 · view rehydration history if any
- **Quiz (20m):** Last third of `quiz-06`
- **Reflect:** Daily mini-quiz #3 if time

### Day 21 (Sun) — Domain 7: Troubleshooting

- **Read (50m):** `07-log-troubleshooting.md` (all of it)
- **Hands-on (30m):** Run `datadog-agent status` · interpret the Logs Agent block · run `agent flare` (don't upload — just produce the bundle and look at its contents)
- **Quiz (20m):** `quiz-07-log-troubleshooting.md`
- **Reflect:** 3 bullets

---

## Week 4 — Integration, Drilling, & Exam Day

> **Focus:** speed, recall, weak-area cleanup. Read less, drill more.

### Day 22 (Mon) — Adaptive weak-area drill #1

- **Drill (60m):** Open `simulations/adaptive-weak-area-drill.md` · use it to build a custom 25-Q drill targeting your two lowest domains from Mock #1
- **Re-read (40m):** Knowledge-base section for the lowest-scoring subdomain
- **Quiz (20m):** Daily mini-quiz #4

### Day 23 (Tue) — Domain-specific drills

- **Drill (90m):** Run **two** domain-specific drills back-to-back from `simulations/domain-drills/` — pick the two heaviest weight domains for you (typically Parsing + Search-Analysis)
- **Reflect (30m):** Detailed review of each wrong answer; update `weak-areas.md`

### Day 24 (Wed) — Glossary & quick-reference review

- **Drill (45m):** Open `knowledge-base/glossary.md` · cover the right column · self-quiz the term ↔ definition for every entry
- **Drill (45m):** Open `knowledge-base/quick-reference.md` · re-create from memory in a blank doc · check what you missed
- **Quiz (30m):** Daily mini-quiz #5 + #6

### Day 25 (Thu) — **Full Mock Exam #2**

- **2 h:** Take `simulations/full-mock-exams/mock-exam-2.md` under exam conditions (timer, no notes)
- **30m:** Grade and analyze. Compare to Mock #1.

### Day 26 (Fri) — Final cleanup

- **Re-drill (60m):** All remaining bullets in `weak-areas.md`. Anything you can't immediately explain → re-read that knowledge-base section.
- **Read (30m):** `quick-reference.md` end to end.
- **Quiz (30m):** Daily mini-quiz #7

### Day 27 (Sat) — **Light review day**

- **Read (45m):** Re-skim only the knowledge-base intros + section summaries (the "Exam-style takeaways" boxes).
- **Drill (45m):** One last adaptive weak-area drill.
- **Stop studying by mid-afternoon.** Sleep early.

### Day 28 (Sun) — **Exam day**

- Morning: light breakfast, water, no caffeine if it makes you jittery. Re-read `quick-reference.md` once.
- Sit the exam in the early afternoon if possible (peak cognitive performance window for most people).
- After: log the result; if pass, schedule a celebratory action; if not, the same materials work for the retake (180-day window allows up to 3 attempts).

---

## What you should be able to do by the end of each week

| End of week | You can… |
|---|---|
| **1** | Explain Logging Without Limits in 30 seconds; configure Agent log collection for files, containers, and a TCP socket; write a `mask_sequences` rule. |
| **2** | Build a Grok parser from scratch; explain processor ordering; write the right query in the Explorer for any common scenario; create a custom facet. |
| **3** | Configure index inclusion + exclusion filters and explain the cost effect; create a log monitor with multi-alert; create a metric from logs; describe the archive → rehydrate flow. |
| **4** | Hit ≥80% on Mock Exam #2; confidently explain every term in the glossary. |

---

## Daily template — copy into your tracker

Copy this block to `progress-tracking/tracker.md` and fill it in each day.

```
### Day [N] — [date] — [topic]

- [ ] Read           (50m)
- [ ] Hands-on       (30m)
- [ ] Quiz           (20m)
- [ ] Reflect notes  (10m)

Quiz score: __ / __

Three insights:
1.
2.
3.

Add to weak-areas:
-
```

---

## If you fall behind

- Skip the **hands-on** block first (re-do it on Day 7 / Day 14 / Day 21 buffers). Reading + quizzing is the highest exam-yield work.
- Never skip a quiz; the feedback loop is what turns reading into recall.
- If you miss two consecutive days, push the schedule by 2 days rather than try to "catch up" — exhaustion compounds errors on the exam.

---

## Estimated time investment summary

| Activity | Hours |
|---|---|
| Reading knowledge base | ~14 h |
| Hands-on practice | ~10 h |
| Domain quizzes | ~5 h |
| Daily mini-quizzes (×7) | ~2 h |
| Mock exams (×2) + grading | ~5 h |
| Weak-area drills | ~4 h |
| Glossary / quick-ref review | ~2 h |
| Reflection / tracker | ~3 h |
| Buffer / catch-up | ~7 h |
| **Total** | **~52 h** |
