# Quiz — Domain 3: Networking & Agent Configuration (15 Qs)

> Time: ~15 min.

---

**1.** Which port does the trace-agent listen on by default?
A. UDP 8125
B. TCP 8126
C. TCP 443
D. UDP 5000

**2.** A host has no direct internet but has access to a corporate proxy at `proxy.example.com:3128`. Which `datadog.yaml` block configures the Agent to use it?

A.
```yaml
proxy_host: proxy.example.com
proxy_port: 3128
```

B.
```yaml
proxy:
  http: http://proxy.example.com:3128
  https: http://proxy.example.com:3128
```

C.
```yaml
http_proxy: proxy.example.com:3128
```

D.
```yaml
proxy.url: http://proxy.example.com:3128
```

**3.** Which of the following is the recommended Autodiscovery format for Agent v7.36+ in Kubernetes?
A. Container labels v1
B. Pod annotations v1
C. Pod annotations v2 (`ad.datadoghq.com/<container>.checks`)
D. Service annotations

**4.** The `%%host%%` template variable in an Autodiscovery config resolves to:
A. The Datadog host machine
B. The discovered container's IP
C. The hostname of the K8s node
D. `127.0.0.1`

**5.** Which command lists every Autodiscovery and file-based check configuration the Agent has loaded, including its source?
A. `agent status`
B. `agent flare`
C. `agent configcheck`
D. `agent diagnose`

**6.** A team's metrics are arriving but logs are not. Metrics endpoint passes connectivity tests. Which is the MOST likely root cause?
A. The Logs Agent is disabled or `logs_enabled: false`
B. Wrong API key
C. DogStatsD UDP loss
D. The host is on the wrong site

**7.** Which `datadog.yaml` setting must be true for the Agent to collect application logs?
A. `logs_collection: enabled`
B. `logs_enabled: true`
C. `enable_logs: true`
D. `dogstatsd_logs: true`

**8.** Which DogStatsD datagram correctly submits a counter "page.views" with tag `env:prod`?
A. `page.views|c|1|env=prod`
B. `page.views:1|c|#env:prod`
C. `count page.views 1 env:prod`
D. `page.views=1c?env=prod`

**9.** Which DogStatsD type code submits a **distribution** sample?
A. `g`
B. `c`
C. `h`
D. `d`

**10.** The Agent runs in a Docker container; you want application metrics from another container on the same host to flow through DogStatsD. What is the simplest correct setup?
A. Put both containers on the same Docker network and use the Agent container's name as the StatsD host
B. Expose port 8125 UDP on the Agent container and have apps target host:8125
C. Either A or B — both can work
D. None — DogStatsD only works on the host network

**11.** A custom Python check located at `/etc/datadog-agent/checks.d/myapp.py` runs successfully but doesn't show up in `agent status`. The most common cause is:
A. The check class is missing
B. There is no matching `conf.yaml` under `/etc/datadog-agent/conf.d/myapp.d/`
C. Python 2 is required for custom checks
D. The Agent must be running in debug mode

**12.** Which is true about the **Cluster Agent's** outbound traffic?
A. It only talks to the Node Agents and not Datadog
B. It needs outbound HTTPS 443 to Datadog like the Node Agents
C. It uses port 8125 to ship its data
D. It listens for inbound 443 from Datadog

**13.** Which of the following is the Auto-Conf source path for default-shipped integration templates?
A. `/etc/datadog-agent/auto.d/`
B. `/etc/datadog-agent/conf.d/auto_conf.yaml`
C. `/var/lib/datadog/auto/`
D. `/etc/datadog/auto-discovery/`

**14.** Where should `169.254.169.254` typically be added when configuring an HTTPS proxy on a cloud VM?
A. `proxy.https`
B. `proxy.http`
C. `no_proxy` (so cloud metadata isn't routed through the proxy)
D. `additional_endpoints`

**15.** Which statement about DogStatsD UDP and reliability is TRUE?
A. It is a TCP-based, lossless protocol
B. UDP datagrams can be silently dropped under heavy load; switch to a Unix Domain Socket for higher reliability
C. Lost datagrams trigger an Agent restart
D. DogStatsD always retries

---

## Answer Key

1. **B** — TCP 8126.
2. **B** — `proxy:` block with `http`/`https` URLs.
3. **C** — Annotations v2 from Agent v7.36+.
4. **B** — Container's IP.
5. **C** — `agent configcheck`.
6. **A** — Logs aren't enabled.
7. **B** — `logs_enabled: true` (or `DD_LOGS_ENABLED=true`).
8. **B** — `name:value|type|#tag:value`.
9. **D** — `d` for distribution.
10. **C** — Both routing approaches work; common in production.
11. **B** — A check needs a matching `conf.yaml` to be scheduled.
12. **B** — Cluster Agent needs the same outbound 443.
13. **B** — `auto_conf.yaml` in `conf.d/`.
14. **C** — Cloud metadata IP belongs in `no_proxy`.
15. **B** — UDP is lossy; UDS gives reliability + origin detection.

> **Score: ___ / 15**
