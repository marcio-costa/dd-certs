# Quick-Reference Card

A single-page cheat sheet — print this and review it the morning of the exam.

## Sites
| Site | App URL | API host |
|---|---|---|
| US1 | `app.datadoghq.com` | `api.datadoghq.com` |
| US3 | `us3.datadoghq.com` | `api.us3.datadoghq.com` |
| US5 | `us5.datadoghq.com` | `api.us5.datadoghq.com` |
| EU1 | `app.datadoghq.eu` | `api.datadoghq.eu` |
| US1-FED | `app.ddog-gov.com` | `api.ddog-gov.com` |
| AP1 | `ap1.datadoghq.com` | `api.ap1.datadoghq.com` |

## Ports
| Port | Use |
|---|---|
| 443 (out) | All Agent → Datadog traffic |
| 8125 UDP | DogStatsD (custom metrics, events, service checks) |
| 8126 TCP | APM trace receiver |
| 5000 TCP | Local Agent IPC (`agent status`) |
| 5002 TCP | Agent GUI (Linux loopback only) |
| 5005 TCP | Agent expvar |

## API Key vs. App Key
- **API key** = org-level, used by Agent + submission API. **One per org** typically.
- **App key** = user-level (or service-account-scoped), used by management/read API.

## Metric Submission Types
| Type | Use |
|---|---|
| GAUGE | Current value |
| COUNT | Increments per interval |
| RATE | Pre-normalized per-second |
| HISTOGRAM | Per-host distribution |
| DISTRIBUTION | Global percentiles |
| SET | Count of unique values |

## Unified Service Tagging — must remember
- `env`
- `service`
- `version`

## Key Agent Commands
| Command | Purpose |
|---|---|
| `agent status` | Self-report |
| `agent flare <CASE_ID>` | Send diagnostic bundle |
| `agent configcheck` | Show loaded check configs |
| `agent check <name>` | Run one check, show output |
| `agent diagnose` | Connectivity + setup checks |
| `agent hostname` | Show effective hostname |

## Config File Locations
| Platform | datadog.yaml |
|---|---|
| Linux | `/etc/datadog-agent/datadog.yaml` |
| Windows | `C:\ProgramData\Datadog\datadog.yaml` |
| macOS | `/opt/datadog-agent/etc/datadog.yaml` |
| Container | `/etc/datadog-agent/datadog.yaml` (env vars usually preferred) |

## Logs Locations
| Platform | Path |
|---|---|
| Linux | `/var/log/datadog/` |
| Windows | `C:\ProgramData\Datadog\logs\` |
| macOS | `/opt/datadog-agent/logs/` |

## Monitor Window Aggregations
- `avg()` — average over the window
- `min()` — required to be over threshold the whole window (sustained)
- `max()` — at any point in the window (peaks)
- `change()` — absolute change vs. prior window
- `pct_change()` — percent change vs. prior window

## Monitor Types — fast recognition
- Metric, Anomaly, Forecast, Outlier, Change, Composite — all on metrics
- Service Check — `OK/WARN/CRIT/UNKNOWN`
- Logs, APM Trace Analytics, Error Tracking, RUM, Synthetic
- Process, Network, Custom Check, Event, Audit Trail
- SLO, DBM, CSPM/CWS/Cloud SIEM

## Dashboard Widget Quick Picks
- Trends → Timeseries
- Single number → Query Value
- Top N → Top List or Toplist
- Per-host comparison → Hostmap
- Distribution view → Distribution / Heat Map
- SLO health → SLO Summary

## "Where do tags come from?" — every layer
1. Agent (`datadog.yaml` `tags:`)
2. Integration `conf.yaml` `tags:`
3. Container labels / pod annotations / cloud resource tags
4. DogStatsD per-datagram `#tag:value`
5. Manual UI tagging on host
6. Tracer libraries (`DD_TAGS`, `DD_ENV`, `DD_SERVICE`, `DD_VERSION`)

## Custom-Metrics Cardinality — DO NOT tag with
- `user_id`, `request_id`, `session_id`, `email`, `trace_id`, timestamps, anything per-request
