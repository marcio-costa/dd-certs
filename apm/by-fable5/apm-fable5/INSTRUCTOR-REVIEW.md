# Instructor Review — Log Management Fundamentals Prep Kit

**Reviewer perspective:** Datadog training-enablement instructor, 7+ years with the product, contributor to certification content.
**Review date:** July 3, 2026 · **Scope:** all 40 files · **Method:** line-by-line technical audit with every contested fact verified against current Datadog documentation.

---

## Verdict

**This kit is now first-attempt-pass ready** for a motivated beginner who follows the 4-week plan and hits the score gates (≥ 65/75 on Mock #2, ≥ 16/20 on Drill 08). The structure was already strong — correct 7-domain alignment, sound domain weighting, spaced repetition, two full mocks, and a weak-area feedback loop. What was holding it back were a handful of factual errors that would have caused wrong answers on the real exam, and ~14 structurally defective questions. All are fixed.

No prep kit can literally *guarantee* a pass — but the failure modes I see most in first-time candidates (edge vs. backend filtering confusion, exclusion vs. ingestion, Live Tail vs. Explorer, processor ordering, JSON preprocessing surprises) are now each covered by teaching text **and** at least two questions.

---

## Factual errors found and corrected (verified against Datadog docs)

1. **Live Tail "15-minute window" — removed everywhere.** Docs describe Live Tail as a *real-time stream* of all ingested logs (indexed or not), uniformly sampled at very high volume, with **no historical lookback**. The kit asserted a "last ~15 min" window in 9 places, including as a quiz/mock answer. If the real exam offers "real-time stream" as an option, the old material would have cost you the point.
2. **JSON preprocessing precedence — answer flipped in 2 questions.** Preprocessing for JSON logs maps reserved keys (`service`, `host`, `level`/`severity`→`status`, `timestamp`, `message`/`msg`/`log`) **over** Agent-set values. Drill-03 Q14 and Day-06 Q1 taught the opposite (Agent wins). Docs are explicit — including the documented Kubernetes gotcha where a JSON `host` field breaks host-tag inheritance. New teaching section added at `03-log-parsing.md` §3.6.
3. **Restriction queries mislocated.** They are role-level RBAC (Data Access page / Roles API, one query per role, scoping *Logs Read Data*) — **not** a per-index setting under Logs → Configuration → Indexes, as the KB and three questions implied. Corrected in KB 01/05, glossary, Mock-2 Q57, Drill-04 Q15, Drill-06 Q12.
4. **Fictional "Live Tail monitor" removed.** KB-06 described a monitor type that watches the Live Tail stream. No such feature exists. Replaced with the supported pattern (excluded logs → Logs to Metrics → metric monitor), which is itself a classic exam scenario.
5. **"Default service is `unknown`" — unsupported claim.** Drill-02 Q11 rewritten to the documented behavior: container autodiscovery falls back to the **short image name** for `source`/`service`.
6. **Index retention list incomplete.** 45-day retention exists (usage types confirm 3/7/15/30/45/60/90/180/365/custom). Lists updated in KB, glossary, quick-reference, Day-07 Q4.
7. **"Logs Hub" (KB-07 §7.7) — not a real UI surface.** Replaced with the real diagnostics: **Pipeline Scanner**, per-processor sample testing, index volume graphs, and the documented `source:datadog "daily quota reached"` events.
8. **Log-based metric retention.** Mock-2 Q60 dismissed "15 months" as ambiguous; docs state log-based metrics keep **15 months at 10-second granularity**. Question restructured (now Select THREE: A, B, D).
9. **Minor:** "nine reserved statuses (emergency…debug)" → eight severities + `OK` (KB-01).

## Defective question mechanics fixed

Fourteen questions had select-count mismatches or answer keys that admitted "pick any of the valid ones" — a defect the real exam never has, and which trains bad test-taking instincts:

- Select-count corrections (TWO→THREE or distractor swapped so the count is exact): Quiz-02 Q5 · Quiz-06 Q8 · Mock-1 Q11, Q16, Q65 · Mock-2 Q19, Q60, Q67 · Drill-04 Q7, Q8 · Drill-06 Q10, Q13 · Day-01 Q8.
- Wording cleanups: Mock-2 Q69 (self-referential option), SDS actions aligned to documented set (Redact / Partially Redact / Hash).

Every answer key now has exactly the advertised number of correct options, and explanations state *why* each distractor is wrong.

## What was added

**Drill 08 — Corner Cases & Exam Traps** (`simulations/domain-drills/drill-08-corner-cases-mixed.md`): 20 new doc-verified questions covering traps none of the existing material tested — the 1,000-log HTTP array limit, default INFO status fallback, case-sensitive attribute search, daily-quota custom reset + warning threshold, one-restriction-query-per-role replacement behavior, Live Tail sampling, rehydration billed as indexed events, RE2 (no lookahead) in Agent rules, `exclude_paths`, AND semantics of multiple `include_at_match` rules, integration-pipeline cloning, Pipeline Scanner, `DD_CONTAINER_EXCLUDE`, global `logs_config.processing_rules`, JSON `msg`→`message` mapping, and Flex Logs awareness. Scheduled into Day 26 of the study plan.

Also added to the KB: preprocessing-precedence section (03), Flex Logs awareness note (06), log-based-metric retention + HTTP array limit in the quick-reference sizing table.

## What I deliberately did NOT change

The 7-domain structure, domain weighting estimates, question volumes, study-plan cadence, and tracker workflow — all sound. Quiz difficulty calibration (reinforcement quizzes easier than drills, drills easier than corner cases) is pedagogically right. The "grade the next morning" mock-exam discipline is a genuinely good practice I'd teach myself.

---

## Exam-readiness gates (follow these and you walk in prepared)

Sit the exam only when **all** of these are true: Mock #2 ≥ 65/75 with no domain below 60%; Drill 08 ≥ 16/20; you can recite from memory the log lifecycle (source → ingest → pipelines → [Live Tail / archives / metrics] → indexes → Explorer/monitors), the three filtering locations (Agent edge / index exclusion / restriction query) with their cost effects, and the JSON preprocessing default attribute lists; and `weak-areas.md` has zero items still marked *open*.

The five distinctions the exam weaponizes — keep them cold: ingested ≠ indexed; exclusion filter ≠ edge filter; Live Tail ≠ Explorer; tags ≠ attributes (`@`); reserved ≠ standard attributes.

Boa prova — trust the prep.
