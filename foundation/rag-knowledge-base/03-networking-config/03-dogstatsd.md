# Domain 3.C — DogStatsD in Depth

> **Source docs:** `/developers/dogstatsd/`, `/metrics/custom_metrics/dogstatsd_metrics_submission/`

## 3.C.1 What DogStatsD Is

DogStatsD is a **server bundled with every Datadog Agent** that:
- Listens on **UDP port 8125** (or a Unix domain socket — `/var/run/datadog/dsd.socket`).
- Speaks an extended StatsD protocol called the **DogStatsD datagram format**, adding **tags** and Datadog-specific types.
- Aggregates incoming samples in memory and flushes batched metrics to the Datadog backend (default flush: every 10 seconds).

Apps send metrics by writing tiny UDP datagrams to the local Agent — no HTTPS, no auth, no library required (although libraries make life easier).

## 3.C.2 Datagram Format

```
<METRIC_NAME>:<VALUE>|<TYPE>|@<SAMPLE_RATE>|#<TAG_KEY_1>:<TAG_VALUE_1>,<TAG_2>
```

Type codes:
| Code | Type |
|---|---|
| `g` | gauge |
| `c` | count |
| `h` | histogram |
| `d` | distribution |
| `s` | set |
| `ms` | timer (treated as histogram) |

Examples:
```
page.views:1|c|#env:prod,page:home
request.duration:235|ms|#env:prod,endpoint:/api
queue.size:42|g|#queue:billing
unique.users:user_42|s
checkout.amount:13.99|d|@0.1|#env:prod
```

### Sending without a library
```bash
# Bash
echo -n "page.views:1|c|#env:prod" >/dev/udp/localhost/8125
# netcat
echo -n "page.views:1|c|#env:prod" | nc -4u -w0 127.0.0.1 8125
```

### Python (using socket)
```python
import socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.sendto(b"page.views:1|c|#env:prod", ("localhost", 8125))
```

### With the official `datadog` library
```python
from datadog import initialize, statsd
initialize(statsd_host="localhost", statsd_port=8125)
statsd.increment("page.views", tags=["env:prod", "page:home"])
statsd.gauge("queue.size", 42, tags=["queue:billing"])
statsd.histogram("request.duration", 0.235, tags=["endpoint:/api"])
```

## 3.C.3 Events and Service Checks via DogStatsD

Events:
```
_e{<TITLE_LEN>,<TEXT_LEN>}:<TITLE>|<TEXT>|d:<TIMESTAMP>|p:<PRIORITY>|t:<ALERT_TYPE>|#<TAGS>
```
Example:
```
_e{21,36}:An exception occurred|Cannot parse CSV file from 10.0.0.17|t:warning|#err_type:bad_request
```

Service checks:
```
_sc|<NAME>|<STATUS>|d:<TIMESTAMP>|h:<HOSTNAME>|#<TAGS>|m:<MESSAGE>
```
Status codes: `0=OK`, `1=WARNING`, `2=CRITICAL`, `3=UNKNOWN`.

## 3.C.4 Configuration

In `datadog.yaml`:
```yaml
use_dogstatsd: true
dogstatsd_port: 8125
dogstatsd_non_local_traffic: false   # set true to accept from other hosts
dogstatsd_socket: ""                 # path to UDS, e.g. /var/run/datadog/dsd.socket
dogstatsd_buffer_size: 8192
dogstatsd_origin_detection: true     # tag with container origin in K8s
```

In Kubernetes (Helm), `agents.useHostPort: true` exposes 8125 on the node, and you can also enable a UDS volume mount for in-pod traffic with origin detection.

## 3.C.5 Origin Detection

When apps send DogStatsD metrics from inside containers via the Unix socket, the Agent uses **origin detection** to automatically tag the metric with the **source container/pod** (`container_id`, `kube_pod_name`, etc.). Required if you want per-pod attribution of custom metrics.

## 3.C.6 Tradeoffs

- **UDP is lossy** — under heavy load, the Agent's receive buffer can drop datagrams. Tune `dogstatsd_buffer_size` and `dogstatsd_queue_size` if you see `dogstatsd_packet_drops_in_total` increasing.
- **Tag cardinality matters** — every unique combination of tag values creates a new metric "context", which counts toward your **custom metrics** billing. Avoid putting unbounded values (user IDs, request IDs, timestamps) in tags.
- **Histograms are aggregated locally per Agent**; **distributions** are aggregated globally on the Datadog backend, so distributions give you accurate global percentiles across all hosts.
