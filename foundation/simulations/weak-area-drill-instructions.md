# Adaptive Weak-Area Drill System

This is a small system for converting your mock-exam mistakes into targeted re-drills. It's "adaptive" in the sense that what you study next is driven by *your* score breakdown, not a one-size-fits-all schedule.

## How it works (the loop)

```
   ┌──────────────────────────┐
   │ 1. Take a quiz / mock    │
   └──────────┬───────────────┘
              ▼
   ┌──────────────────────────┐
   │ 2. Score per domain      │
   │    (see scoring sheet)   │
   └──────────┬───────────────┘
              ▼
   ┌──────────────────────────┐
   │ 3. Pick weakest 1–2      │
   │    domains by % correct  │
   └──────────┬───────────────┘
              ▼
   ┌──────────────────────────┐
   │ 4. Re-read RAG file(s)   │
   │    + redo missed Qs      │
   └──────────┬───────────────┘
              ▼
   ┌──────────────────────────┐
   │ 5. Re-quiz, then loop    │
   └──────────────────────────┘
```

## Step 1 — After any quiz/mock, fill in the diagnostic

Open `progress-tracking/results.md`. For every quiz you take, log:
- date
- quiz name
- per-domain score (% correct)
- list of question numbers you got wrong
- 1-line "what concept did I miss" for each wrong answer

Example row:
```
| 2026-05-22 | Mock #1 | D1 88% / D2 50% / D3 75% / D4 61% / D5 80% / D6 73% | D2: Q14, Q16 (App key vs API key); D4: Q39, Q43, Q44, Q45, Q47 |
```

## Step 2 — Decide what to drill

Apply this rule of thumb:

- **Domains < 70%** → mandatory drill, top priority.
- **Domains 70–80%** → optional drill, do if time allows.
- **Domains ≥ 80%** → skim and move on.

If two domains tie, pick the one with the **higher exam weight** first. Datadog Fundamentals weights data collection (Domain 4) and visualization/monitors (Domain 6) heaviest, so prioritize those when in doubt.

## Step 3 — Drill protocol per weak domain (45 min)

For each weak domain, do this 45-minute block:

| Min | Activity |
|---|---|
| 0–10 | Re-read the RAG file(s) for that domain. Don't skim — read aloud the headings you've forgotten. |
| 10–25 | For each missed question, write a one-paragraph explanation in your own words of why the right answer was right and the others were wrong. Save as `progress-tracking/drill-notes/<domain>-<date>.md`. |
| 25–40 | Re-take that domain's quiz from `quizzes/<NN>-<domain>-quiz.md`. New attempt; don't peek at the previous answers. |
| 40–45 | Compare the two attempts. If the same question is wrong twice, mark it `★` in the drill notes. ★ questions go on a flashcard list you review every morning until exam day. |

## Step 4 — Generate a custom mini-quiz from your wrongs

Once you have ≥ 10 ★ questions, ask the cert-prep skill to generate a custom mini-quiz:

```
"Quiz me on these 10 ★ topics: <list them>"
```

The skill will produce 10 fresh questions (different scenarios, same concepts) so you can verify the concept stuck and you didn't just memorize the wording.

## Step 5 — Repeat until next mock exam

Continue the loop. Take Mock #2 only when **every domain is ≥ 70%** in the latest drill round.

---

## Per-domain "where to read" cheat sheet

| If weakest domain is… | Re-read these files | Then re-take this quiz |
|---|---|---|
| 1 — Computer Fundamentals | `rag-knowledge-base/01-computer-fundamentals/01-computer-fundamentals.md` | `quizzes/01-computer-fundamentals-quiz.md` |
| 2 — Infrastructure Deployment | `02-infrastructure-agent/{01-agent-architecture,02-installation-platforms,03-keys-sites-org}.md` | `quizzes/02-infrastructure-agent-quiz.md` |
| 3 — Networking & Agent Config | `03-networking-config/{01-networking-firewall,02-integrations-checks,03-dogstatsd}.md` | `quizzes/03-networking-config-quiz.md` |
| 4 — Data Collection | `04-data-collection/{01-metrics-types,02-tagging-best-practices,03-events-service-checks-host-map}.md` | `quizzes/04-data-collection-quiz.md` + `quiz-tagging-focus.md` |
| 5 — Troubleshooting | `05-agent-troubleshooting/01-troubleshooting-toolkit.md` | `quizzes/05-troubleshooting-quiz.md` |
| 6 — Visualization & Monitors | `06-visualization-monitors/{01-dashboards,02-monitors}.md` | `quizzes/06-visualization-monitors-quiz.md` |

## Common patterns of mistakes (and what they mean)

| Pattern in your wrong answers | What it usually signals |
|---|---|
| Confusing API key vs. App key | You're memorizing names without semantics — re-read keys section §2.C.1. |
| Picking `histogram` when answer is `distribution` | You haven't internalized the per-host vs. global aggregation distinction — re-read §4.A.1. |
| Falling for cardinality traps | You haven't internalized "tag = context multiplier" — re-read §4.A.4 and §4.B.5. |
| Wrong proxy/site config | You're not mentally simulating the request path — re-read §3.A.1. |
| Missing "the Agent only needs API key" | Same as #1; review §2.C.1. |
| Confusing monitor types | Re-read §6.B.1 table and write each row from memory. |

## When you've drilled enough

You're ready for the real exam when:
- Every quiz scores ≥ 85% on a fresh attempt.
- Mock #1 ≥ 75%, Mock #2 ≥ 80%.
- You can recite the USTagging tags, the four service-check states, and the six site URLs from memory.
- You can explain the difference between **histogram** and **distribution** in one sentence without notes.
- You can name the right command for a given symptom from §5.5 of the troubleshooting file.
