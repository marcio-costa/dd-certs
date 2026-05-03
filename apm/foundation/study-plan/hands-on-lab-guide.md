# Hands-On Lab Guide

> Hands-on practice is the single best lever to actually pass. The exam tests *applied* understanding.

## Step 1 — Get a Datadog environment

Pick **one** of:

1. **Datadog Free Trial (14 days)** — sign up at datadoghq.com.
2. **Existing org** at your company (ask your admin for an APM-enabled org with a non-prod env).
3. **Datadog Learn Sandbox** (offered inside Learning Center courses).

## Step 2 — Install the Agent

Pick a host or container runtime you already have:

```bash
# Linux (one-line install)
DD_API_KEY=<your-key> DD_SITE="datadoghq.com" \
  bash -c "$(curl -L https://install.datadoghq.com/scripts/install_script_agent7.sh)"

# Verify
sudo datadog-agent status | grep -A2 "trace-agent"
```

## Step 3 — Pick a sample app

Three solid options, all open source:

1. **OpenTelemetry Demo** — multi-service microservice, great for Service Map, OTel architecture.
2. **`dd-trace-py` `notes` sample** — small Python app, used in Datadog tutorials.
3. **Your own toy Flask/Express/Spring service** — fastest if you're a developer.

## Step 4 — Instrument with Datadog

Example for Python:

```bash
pip install ddtrace
DD_SERVICE=notes DD_ENV=lab DD_VERSION=0.1.0 \
DD_LOGS_INJECTION=true DD_PROFILING_ENABLED=true \
ddtrace-run python app.py
```

## Step 5 — Run the lab milestones

Map them to study days:

| Milestone                                                              | Map to study day |
| ---------------------------------------------------------------------- | ---------------- |
| Confirm trace-agent listening on 8126                                  | Week 1, Mon      |
| See env, service, version on Service Page                              | Week 1, Tue      |
| Toggle Live vs Indexed Search in Trace Explorer                        | Week 1, Wed      |
| Use auto-instrumentation in your favourite language                    | Week 1, Thu      |
| Single-Step Instrumentation on Docker                                  | Week 2, Tue      |
| Apply `DD_TRACE_SAMPLING_RULES` and a retention filter                 | Week 2, Wed      |
| Tag scrubbing with `DD_APM_REPLACE_TAGS`                               | Week 2, Thu      |
| Continuous Profiler flame graph and comparison                         | Week 2, Fri      |
| Software Catalog + Service Page tabs                                   | Week 3, Mon      |
| 10 trace search queries (Live + Indexed)                               | Week 3, Tue      |
| Deployment Tracking version comparison                                 | Week 3, Wed      |
| Error Tracking grouping + custom fingerprint                           | Week 3, Thu      |
| Span Summary drill into highest-cost span                              | Week 3, Fri      |
| Trace ↔ Log correlation working end-to-end                             | Week 4, Mon      |
| Create one of every APM monitor type                                   | Week 4, Tue      |

## Tips

- Generate steady synthetic traffic (`siege`, `wrk`, or `hey`) so charts have data.
- Tag your test runs with `DD_TAGS=lab_run:<date>` so you can filter them out later.
- Keep cost low — Datadog free trial covers all features for 14 days.

## Cleanup

When done with the trial, uninstall the Agent (`sudo apt remove datadog-agent`) and delete the test org or expire the trial.
