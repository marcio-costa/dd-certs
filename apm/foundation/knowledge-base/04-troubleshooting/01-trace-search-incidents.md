# Using Trace Search During Incidents

> **Domain 4 — Troubleshooting Application using APM**

## The incident playbook with APM

When an alert fires (latency spike, error budget burn, etc.):

1. **Open the Service Page** for the affected service.
2. **Switch to the time window** of the incident.
3. **Triage with OOTB charts**: which resource(s) and which versions are responsible?
4. **Drill into traces** via the Trace Explorer (Live for last 15 min, Indexed for older).
5. **Filter** by `status:error`, the affected resource, and a custom tag if you have one (e.g. `@user.id`, `@order.id`).
6. **Open a representative trace** to inspect the **flame graph / waterfall**.
7. **Check Error Tracking** to see whether this is a known issue or new.
8. **Pivot to logs** with the trace ID baked into log lines (trace-log correlation).
9. **Pivot to profiles** for the slow span via the Code Hotspots tab.
10. **Compare versions** on the Deployments tab.

## Reading the trace waterfall

- Each row is a span; horizontal length = duration.
- Indentation = parent/child relationship.
- Red bars = error spans.
- Click a span to see its tags, logs, and metrics in the side panel.
- "Critical Path" highlights the longest causal path from root to leaf (most useful single drill-down).
- The "Errors" tab inside a trace lists every error span and their stack traces.

## Reading flame graphs

- Width = time spent.
- Color groups by service or operation.
- Stack frames flow top-down (root span at top).

## Service Map during an incident

- Open **APM → Service Map** to see which dependency is failing red.
- Click an edge to see request rate and error rate between two services.
- Useful to confirm "the slowness is downstream of `payment-api`".

## Common queries to memorize

```text
# All errors for this service in last 15 min
service:checkout status:error

# Slow checkout calls right now
service:checkout resource_name:"POST /checkout" @duration:>1s

# Specific user's recent journeys
@user.id:42

# A specific trace ID from a customer ticket
trace_id:abc123def456

# Errors only in the new version
service:checkout status:error version:2025-10-01

# Compare two versions
service:checkout version:(2025-09-15 OR 2025-10-01)
```

## Things to remember for the exam

- Trace Explorer ↔ Service Page ↔ Logs Explorer all share the time picker and tags.
- **Live Search (15 min)** is the default during a real-time incident.
- Always include `service:` and a time range; add `status:error` for triage.
- Trace ↔ Log correlation depends on the trace ID being injected into logs (`DD_LOGS_INJECTION=true`).
