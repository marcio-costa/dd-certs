# Live Search vs Retained (Indexed) Search

> **Domain 3 — Insight Discovery**
> Common exam question: which one to use, and why.

## At a glance

| Aspect                     | **Live Search**                                  | **Indexed (Retained) Search**                         |
| -------------------------- | ------------------------------------------------ | ----------------------------------------------------- |
| Data source                | All ingested spans                               | Spans kept by retention filters                       |
| Time window                | **Last 15 minutes**                              | Up to **15 days** (custom) / **30 days** (intelligent) |
| Tags searchable            | **All tags** (no need to be indexed)             | Indexed tags only (defined in retention filters)      |
| Use case                   | Real-time triage during an incident              | Historical investigation, post-mortem, audit         |
| UI indicator               | "Live" badge                                     | Time selector beyond 15 min auto-switches mode       |
| Cost impact                | Free (within ingestion)                          | Drives **billable indexed spans**                     |

## Live Search (default in Trace Explorer)

When you open **Traces → Trace Explorer** with the default 15-min window, you're in Live mode. This view runs your query across **every ingested span** of the last 15 min — useful for:

- "Is this trace happening right now?"
- "What custom tag values exist in last 15 min?"
- Production incident debugging.

## Indexed/Retained Search

Selecting a time window beyond 15 minutes (or explicitly switching to Indexed Data Search) queries the **indexed spans** kept by your retention filters. You can:

- Look up the trace from yesterday's incident.
- Inspect throughput/latency over a long period (although trace metrics handle aggregates better).
- Investigate which spans your retention filters captured.

## How retention filters affect it

A trace appears in Indexed Search **only if** it matches at least one retention filter (custom, intelligent, or error/rare). If you need a particular pattern to be searchable beyond 15 minutes, **add a custom retention filter**.

## Trace metrics vs Live/Indexed

Both Live and Indexed search return individual spans/traces. **Trace metrics** are computed from 100% of ingested data and stored 15 months — they are independent of which spans were indexed and used for charts and dashboards.

## Decision flow

1. Need data older than 15 minutes? → **Indexed Search** (and the trace must be retained).
2. Need to query an arbitrary custom tag right now? → **Live Search**.
3. Need long-term aggregate trends? → **Trace metrics** in dashboards/notebooks.

## Things to remember for the exam

- Live Search window = **15 minutes**.
- Custom retention filters keep spans **15 days**; intelligent filter keeps them **30 days**.
- Live Search can query **any tag**; Indexed Search is limited to indexed tags.
- Switching the time picker beyond 15 minutes flips you into Indexed Search.
