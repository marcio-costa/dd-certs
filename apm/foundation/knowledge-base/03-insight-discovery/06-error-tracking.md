# Error Tracking (APM)

> **Domain 3 — Insight Discovery**

## What it is

Error Tracking groups individual error spans into **issues** by computing a **fingerprint** for each error and clustering similar errors together. It surfaces:

- The unique number of issues affecting your service.
- Which deploy/version introduced an issue.
- Affected users, endpoints, and traces.
- The most relevant stack trace per issue.

## How fingerprints work

By default, the fingerprint is computed from:

1. **error.type** (e.g., `RuntimeException`, `KeyError`, `nil dereference`)
2. **error.message** (cleaned of variable parts)
3. **The frames composing the stack trace**

Errors sharing the same fingerprint belong to the same issue.

## Custom fingerprinting

Override the auto-grouping by tagging the error span:

```python
with tracer.trace("throws.an.error") as span:
    span.set_tag('error.fingerprint', 'my-custom-grouping-material')
    raise Exception("Something went wrong")
```

Use this when noisy stack-trace differences over-fragment a single bug into many issues.

## Issue states

| State        | Meaning                                                          |
| ------------ | ---------------------------------------------------------------- |
| **For Review** | New or recurring; needs triage.                                |
| **Reviewed** | Acknowledged but not yet fixed.                                  |
| **Resolved** | Fix deployed / accepted.                                         |
| **Ignored**  | Won't fix / known noise.                                         |

Resolved issues that **reappear after a new deployment** automatically reopen.

## Useful attributes per issue

- Distribution of impacted spans over time
- First seen / last seen
- Latest stack trace, span attributes, host & container tags
- Affected versions
- Linked traces and logs
- Suspected commit (when Source Code Integration is configured)

## Sources of errors

Error Tracking aggregates errors from:

- **APM** (error spans)
- **Logs** (logs with `status:error`)
- **RUM** (front-end errors)

You can scope the explorer to one source or all.

## Comparison with Watchdog

| Feature       | Detects                                          | Action                                |
| ------------- | ------------------------------------------------ | ------------------------------------- |
| Watchdog      | **Anomalies** in error rate / latency / hits     | Surfaces "Story" with context         |
| Error Tracking | **Individual error issues** grouped by fingerprint | Triage and assign issue ownership |

They complement each other: Watchdog says "rate spiked"; Error Tracking shows "what new exception caused it".

## Things to remember for the exam

- An **issue** = group of errors sharing a **fingerprint**.
- Fingerprint = `error.type` + `error.message` + stack frames.
- Override grouping with `error.fingerprint` span tag.
- Resolved issues auto-reopen on regression after a new deploy.
- Error Tracking handles APM, Logs, and RUM error sources.
