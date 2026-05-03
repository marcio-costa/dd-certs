# Span Summary

> **Domain 3 — Insight Discovery**

## What it is

The **Span Summary** is a tabular breakdown of **how often** and **how long** every type of span occurs within a chosen scope (a service, a resource, an endpoint). It is the answer to "what work happens inside this request, and how much does it cost?"

## Where you find it

- **Service Page → Span Summary tab**
- **Resource Page → Span Summary tab** (per-resource view)
- Span tabs in the trace details panel

## Metrics shown

For each span name (operation), the summary reports:

| Column                       | Meaning                                                              |
| ---------------------------- | -------------------------------------------------------------------- |
| **Avg occurrences per trace** | How many times a span of this type appears in an average trace.     |
| **% of traces with this span** | Share of traces where this span occurs at least once.              |
| **Avg duration**             | Mean wall-clock time of the span.                                    |
| **Avg % execution time**     | The share of the parent request's time consumed by this span.        |

These numbers are computed only on traces where the span appears at least once.

## Use cases

- Spot N+1 query patterns (`db.query` shows up 200 times per request).
- Identify the dominant cost in a trace ("`http.request to upstream-x` consumes 70% of execution time").
- Find regression candidates after a deploy (a span suddenly appears more often or takes longer).
- Drive optimization decisions ("which span deserves manual instrumentation/caching?").

## Combining with other features

- **Filter** by environment, version, custom tags to compare scopes.
- **Compare Deployments** uses the Span Summary diff to highlight regressions.
- Click a row to drill into traces where that span appears.

## Things to remember for the exam

- Span Summary metrics are **per-span-type** (grouped by operation name) within a scope.
- Key metrics: avg occurrences/trace, % traces with span, avg duration, avg % execution time.
- Used to find latency hot-spots and N+1 problems.
- Calculated only on traces containing the span (not on all traces).
