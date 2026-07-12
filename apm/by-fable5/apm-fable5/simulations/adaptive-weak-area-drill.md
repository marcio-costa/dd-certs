# Adaptive Weak-Area Drill

Use this when your `progress-tracking/weak-areas.md` has 5+ open items, after Mock #1, and again in Week 4.

The drill **adapts** because *you* assemble it from the existing question pool, weighted toward your weakest domains. It's not a fixed quiz — it's a recipe.

---

## Step 1 — Identify your two weakest domains

Open `../progress-tracking/tracker.md` and `../progress-tracking/weak-areas.md`. Pick the two domains where:

- Mock-exam score is lowest, OR
- The most `open` items in `weak-areas.md` accumulate.

Call them **WEAK-A** and **WEAK-B**.

---

## Step 2 — Build a 25-Q drill

Pull questions from the question pool with this distribution:

| Source | Count | Notes |
|---|---|---|
| `../domain-drills/drill-XX-{WEAK-A}.md` | 8 Qs | Hardest pool for that domain |
| `../domain-drills/drill-XX-{WEAK-B}.md` | 8 Qs | Hardest pool for the second domain |
| `../../quizzes/quiz-XX-{WEAK-A}.md` | 3 Qs | Reinforcement-level on WEAK-A |
| `../../quizzes/quiz-XX-{WEAK-B}.md` | 3 Qs | Reinforcement-level on WEAK-B |
| `../full-mock-exams/mock-exam-1.md` | 3 Qs | Pick 3 you got wrong |
| **Total** | **25** | |

**Pick questions you got wrong before, plus any from drills you haven't taken yet.**

Avoid copy-paste — instead, list the question numbers in this drill log:

```
WEAK-A: drill-04 Q3, Q5, Q7, Q9, Q11, Q12, Q13, Q14
WEAK-B: drill-06 Q1, Q4, Q5, Q6, Q9, Q10, Q11, Q15
WEAK-A reinforcement: quiz-04 Q5, Q8, Q11
WEAK-B reinforcement: quiz-06 Q3, Q9, Q12
Mock #1 retries: Q23, Q49, Q66
```

---

## Step 3 — Time-box and take it cold

- 30 minutes timer.
- No notes; no Datadog UI.
- Answer in a separate file (`drill-attempt-DATE.md`) so you don't see your previous attempt's answers.

---

## Step 4 — Grade and reflect

| Score | Meaning | Action |
|---|---|---|
| **24–25/25** | These weak areas have moved into mastery. | Mark items in `weak-areas.md` as **mastered**; pick the next-weakest two domains for the next drill. |
| **20–23/25** | Strong, but a few stubborn items. | Re-read the relevant knowledge-base sections; rerun in 3 days. |
| **15–19/25** | Real gaps remain. | Re-read the entire knowledge-base file for both domains; redo their domain quiz; redrill in 5–7 days. |
| **< 15/25** | Likely studying too fast. | Pause new content; spend 2 days re-reading the knowledge base for both domains; redo their quizzes; redrill. |

For every wrong answer, write in `weak-areas.md`:

```
- [Date] WEAK-A · "missed concept verbatim" · open
```

---

## Step 5 — Pattern recognition

After 2–3 adaptive drills, look for **patterns** in your wrong answers:

- Are you missing **terminology** (reserved vs. standard attributes, Live Tail vs. Explorer)? → Re-drill the **glossary**.
- Are you missing **scenarios** (multi-line, edge vs. backend scrubbing, exclusion vs. inclusion)? → Practice scenario reasoning out loud; explain the mechanism to a colleague or rubber duck.
- Are you missing **ordering** (pipeline order, processor order, log lifecycle)? → Re-do the *ordering* questions from Mock #1 + Mock #2.
- Are you missing **syntax** (search syntax, range queries, wildcards)? → Re-read `04-search-filtering.md` §4.2 + `quick-reference.md` syntax block daily for 3 days.

---

## Step 6 — Stop drilling, start recalling

In the **last 3 days before the exam**, switch from drills to **active recall**:

- Cover the right column of the glossary; recite definitions for each term.
- Re-create the `quick-reference.md` cheat sheet from memory; check what you missed.
- Explain each of the 6 exam-style takeaways at the end of every domain knowledge-base file out loud, without looking.

Drills are practice; recall is performance. Switch modes deliberately.

---

## Quick-pick: Which two domains to focus on first?

Common weak-area hot spots based on prior students:

1. **Log Parsing — pipeline / processor ordering, Grok filters.** Easy points if you read 03-log-parsing.md twice.
2. **Log Analysis — exclusion vs. ingestion vs. archive interplay.** The exam *loves* this distinction.
3. **Log Utilization — Logs to Metrics + Forwarding + Archive details.** Lots of small facts.
4. **Search syntax — range queries `[ ]` vs. `{ }`, wildcards, `@` prefix.** Pure memorization.

If unsure, start with **Log Parsing + Log Analysis** for your first adaptive drill.
