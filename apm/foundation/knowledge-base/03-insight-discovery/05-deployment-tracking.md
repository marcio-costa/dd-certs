# Deployment Tracking

> **Domain 3 — Insight Discovery**

## What it is

Deployment Tracking links each trace to a **specific software version**, enabling Datadog to compare performance across versions, surface regressions immediately, and feed DORA Metrics.

## Prerequisites

1. Service uses **Unified Service Tagging** (`env`, `service`, **`version`**).
2. Service has a **Software Catalog** entry (recommended).
3. Tracer is updated and forwarding the `version` tag on every span.

## Setting the version

| Method                              | Notes                                                |
| ----------------------------------- | ---------------------------------------------------- |
| `DD_VERSION` env var                | Most common. Restart container/process to apply.     |
| `tags.datadoghq.com/version` label  | Kubernetes-recommended (works with Admission Controller). |
| Container image tag                 | Auto-derived (Agent 7.52+) when `DD_VERSION` not set. |
| Git commit SHA                      | Auto-derived as last fallback (Agent 7.52+).         |

## Where deployment tracking shows up

- **Service Page → Deployments tab**: timeline of versions seen, with their request rate, errors, latency.
- **Version comparison**: side-by-side metrics between any two versions.
- **Auto-detection of regressions**: Watchdog highlights versions that introduced anomalies.
- **Trace Explorer / Monitors**: filter `version:2025-10-01` to scope queries.
- **Change Tracking**: code deployment events overlaid on dashboards.
- **DORA Metrics**: APM Deployment Tracking can be used as a deployment data source for measuring deployment frequency, lead time, and change failure rate.

## Comparing deployments

The Service Page's Deployments tab lets you select **two versions** and visualizes:

- p50/p95/p99 latency delta
- Error rate delta
- Hits per second delta
- Resource-level changes (which endpoints regressed)
- Profile comparison link

## Best practices

- Use a stable, sortable version scheme (e.g. semver, build timestamp, or git short SHA).
- Avoid `latest` for the version tag — Datadog won't know when the deploy actually happened.
- Tag every artifact (container image + DD_VERSION + label) consistently to avoid mismatches.
- Couple with Continuous Profiler comparison for the deepest analysis.

## Things to remember for the exam

- Deployment Tracking depends on the **`version` tag** of USM.
- Auto-version (Agent **7.52+**) priority: explicit `DD_VERSION` > image tag > Git SHA.
- The Service Page **Deployments** tab is the entry point.
- Ties into **DORA Metrics** when configured as a deployment data source.
