# Adaptive Weak-Area Drills

> Generated **on-demand** by the `datadog-cert-prep` skill. Each drill is targeted at one or two narrow topics where you missed questions in a recent quiz or simulation.

## How to invoke

In Cowork, just say:

```
adaptive drill on retention periods
adaptive drill on sampling rules
adaptive drill on Error Tracking fingerprints
adaptive drill on the Service Page tabs
```

The skill will:

1. Pull the matching KB file(s) from `knowledge-base/`.
2. Generate a fresh 10-question drill at the difficulty you specify (default: medium).
3. Score you, give explanations, and link back to the relevant KB section.

## Example — sample drill output

```
TOPIC: Retention Periods (Domain 1, KB §1.4)
DIFFICULTY: medium
LENGTH: 10 questions

Q1. Ingested spans (Live Search window) live for ___ minutes.
A) 1   B) 5   C) 15   D) 60                Correct: C

Q2. Custom retention filters keep matching spans for ___ days.
...
```

## When to run an adaptive drill

| Trigger | Recommended response |
|---------|----------------------|
| You scored <80% on a domain-specific drill | One adaptive drill per missed topic |
| You scored <70% on the full mock | Two-three adaptive drills before retaking the mock |
| A specific concept feels fuzzy after reading the KB | One adaptive drill on that concept |
| You haven't studied in 3+ days | An adaptive "refresh" drill mixing 4 random topics |

## Tracking

Log every adaptive drill in `progress/daily-tracker.md` so you can see whether the gap actually closed.

## Why these aren't pre-written

The whole point of adaptive drills is that they are **fresh and targeted** for what you missed. The skill generates them dynamically from the same RAG knowledge base used for everything else, so the questions stay aligned with your latest documentation refresh.

If you want **pre-written, fixed quizzes** instead, use:

- `quizzes/` — domain-by-domain, repeatable
- `simulations/domain-specific-drills/` — slightly larger, exam-style
- `simulations/full-mock-exam/` — full 75-question simulation
