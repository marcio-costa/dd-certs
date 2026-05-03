# Retention Periods for APM Data

> **Domain 1 — APM Fundamentals**
> A frequent exam trap — make sure you know each number.

## Two-tier model

Datadog separates **ingestion** (collecting data from the Agent) from **retention/indexing** (keeping it searchable in the backend).

| Data                                                 | Retention                          |
| ---------------------------------------------------- | ---------------------------------- |
| **Ingested spans** (Live Search window)              | **15 minutes**                     |
| **Indexed spans** kept by *custom* retention filters | **15 days**                        |
| **Indexed spans** kept by the *intelligent* retention filter | **30 days**                |
| **Trace metrics** (computed from 100% of ingested traces) | **15 months**                |
| **App Analytics / span-based custom metrics**        | **15 months**                      |

### Trace metrics

Even when spans are dropped by sampling/retention, **trace metrics** (`trace.<operation>.duration`, `trace.<operation>.hits`, `trace.<operation>.errors`, etc.) are computed from **100%** of ingested data and retained for 15 months. This is why dashboards still show accurate aggregates after spans roll off.

## Live Search vs Indexed Data Search

- **Live Search** queries the last **15 minutes** of *all ingested* data, across any span and any tag (including non-indexed ones).
- **Indexed Data Search** (formerly Retained Search) queries spans kept by **retention filters** for **15 days** (or 30 days via the intelligent filter).
- The Trace Explorer UI shows a visual indicator for which mode you are in.

## Retention filters

- **Custom retention filters**: rules you define (service, resource, tag patterns) that decide which spans to keep for 15 days.
- **Intelligent retention filter**: Datadog-managed; auto-keeps a sample of spans needed to drive Watchdog, Error Tracking, and exemplar selection — kept for 30 days.
- **Error and rare-trace filters**: enabled by default (errors and unusual traces are over-sampled).

## Things to remember for the exam

- "Ingested" = 15 minutes.
- "Indexed by custom retention filter" = 15 days.
- "Intelligent retention filter" = 30 days.
- "Trace metrics" = 15 months.
- Billing is based on **indexed** spans (not ingested).
