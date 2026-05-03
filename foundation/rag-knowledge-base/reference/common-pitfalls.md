# Common Exam Pitfalls and "Gotcha" Topics

This is the list of recurring traps that show up on the Datadog Fundamentals exam. Treat these as flashcards.

## Pitfall 1: Mixing up API key and Application key
- The **Agent** only needs the **API key**. App keys are for the management/read API.
- "What does the Agent need to send data to Datadog?" → API key + correct `site:`.

## Pitfall 2: Site ≠ default
- US1 is the default. If your account is on EU1, US3, US5, US1-FED, or AP1, set `site:` accordingly **or** the Agent will try the wrong endpoint and fail.

## Pitfall 3: Histogram vs. Distribution
- **Histogram** = aggregated **per host**. Combining percentiles across hosts is mathematically not meaningful.
- **Distribution** = aggregated **globally on Datadog backend**. Use this for SLOs and per-tag percentiles spanning many hosts.

## Pitfall 4: Cardinality blow-up from tags
- Tagging with `user_id`, `request_id`, etc. creates a new context per unique value → custom metric overage.
- Put high-cardinality dimensions in **logs** or **APM trace tags** instead.

## Pitfall 5: USTagging set incompletely
- Setting only `DD_SERVICE` without `DD_ENV` and `DD_VERSION` partially breaks correlation. You need **all three**.

## Pitfall 6: Autodiscovery — file vs. annotation precedence
- Pod annotations (Kubernetes) > Container labels (Docker) > Files in `conf.d/` > Auto-Conf.
- Annotations v2 (Agent ≥ 7.36) is the recommended format.

## Pitfall 7: DogStatsD is UDP — lossy
- High-volume custom metrics can drop without warning. Tune `dogstatsd_buffer_size`, watch `datadog.dogstatsd.packet_drops_in_total`.
- For higher reliability, use the **UDS** (Unix Domain Socket) transport.

## Pitfall 8: Logs need their own switch and endpoint
- `logs_enabled: true` (or `DD_LOGS_ENABLED=true`) is required.
- For container logs: `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true`.
- The Logs intake hostname (`agent-intake.logs.<site>`) must be reachable through the firewall — the metrics endpoint working doesn't guarantee logs work.

## Pitfall 9: APM enable + ports
- `apm_config.enabled: true` (or `DD_APM_ENABLED=true`).
- App must reach `127.0.0.1:8126`. In K8s, use `DD_AGENT_HOST` from the downward API to point at the host node.

## Pitfall 10: Agent vs. Cluster Agent
- The **Cluster Agent** (Kubernetes) reduces API server load and provides cluster-level data + external metrics for HPA.
- It still needs internet access to Datadog. Both Node Agent **and** Cluster Agent need it.

## Pitfall 11: Monitor "no data" vs. recovery
- A monitor in **No Data** state can be configured to alert (default: false).
- A monitor recovers when the value moves below the recovery threshold *and* the recovery window passes.

## Pitfall 12: Multi-alert monitor evaluation
- `by {host}` creates per-host groups; the monitor alerts/recovers **per group**.
- New groups only get evaluated after one full evaluation window — set `new_group_delay` so newly born groups don't spam.

## Pitfall 13: Composite monitors
- Use parentheses and operators `&&` `||` `!`: `composite_query: !((1234) && (5678))`.
- Composite monitors don't have a query of their own — they reference other monitors by ID.

## Pitfall 14: Downtime scope vs. monitor identifier
- `scope` filters which **groups** of a multi-alert monitor are muted.
- `monitor_identifier` selects which **monitors** are muted (by ID or by tags).
- Use both for surgical mutes.

## Pitfall 15: `as_count()` vs. `as_rate()` in monitor queries
- A `count`-type metric query without a modifier returns **per-second rate** by default.
- Use `.as_count()` to get the absolute count over each interval — required when your threshold is in absolute occurrences ("more than 100 errors in 5 min").

## Pitfall 16: Agent versions
- Agent 7 (Python 3, current default).
- Agent 6 (maintenance, Python 2 + 3 hybrid).
- Agent 5 is end-of-life.
- New deployments should use Agent 7 unless a specific integration requires otherwise.

## Pitfall 17: Cloud integrations are NOT a substitute for the Agent
- AWS/Azure/GCP integrations bring in **provider** metrics (CloudWatch, etc.).
- They do **not** give you per-host metrics, logs, or APM. You still need an Agent for that.

## Pitfall 18: Trace propagation requires consistent libraries
- Distributed tracing across services requires either consistent Datadog tracers or OpenTelemetry compatibility. Mismatched headers break the trace.

## Pitfall 19: Downtime ends with notification?
- By default, downtime end notifications can be disabled (`notify_end_types: []`). The first recovery after a downtime is also commonly muted with `mute_first_recovery_notification: true`.

## Pitfall 20: Reading dashboards — `avg by host` vs. `avg`
- `avg:metric{*}` — average across all hosts (one line).
- `avg:metric{*} by {host}` — one line per host (could be hundreds).
- This affects both readability and monitor cardinality.
