# Quiz — Domain 5: Troubleshooting the Agent (15 Qs)

> Time: ~15 min.

---

**1.** Which command gives a self-report of the Agent's runtime state?
A. `datadog-agent runtime`
B. `datadog-agent status`
C. `datadog-agent diagnose info`
D. `dd report`

**2.** Where are the Datadog Agent logs on Linux by default?
A. `/etc/datadog-agent/logs/`
B. `/var/log/datadog/`
C. `/opt/datadog/logs/`
D. `/usr/local/datadog/`

**3.** Which command bundles diagnostic data and uploads it to Datadog Support?
A. `datadog-agent dump`
B. `datadog-agent flare <CASE_ID>`
C. `datadog-agent support`
D. `datadog-agent send-logs`

**4.** A specific integration check is missing data. Which command shows the per-check execution result, last error, and metrics emitted?
A. `agent diagnose`
B. `agent configcheck`
C. `agent status` (per-check section)
D. `agent integrations show`

**5.** A user wants to test connectivity from the Agent to all Datadog endpoints for their site. Which command is best?
A. `agent connectivity-test`
B. `agent diagnose datadog-connectivity`
C. `ping api.datadoghq.com`
D. `agent flare`

**6.** The Agent reports the wrong hostname on the Infrastructure list. Which command shows what hostname the Agent will report?
A. `hostname`
B. `agent hostname`
C. `agent status` only
D. `dd-host`

**7.** Which monitor query reliably alerts when an Agent has stopped reporting?
A. `avg(last_5m): system.uptime{*} == 0`
B. `avg(last_5m): datadog.agent.running{*} by {host} < 1`
C. `last(last_5m): system.cpu.user{*} == 0`
D. `events("source:agent down").rollup("count")`

**8.** A custom check is in the right folder but `agent status` shows no entry for it. Which is the most likely fix?
A. Create the matching `conf.yaml` under `conf.d/<name>.d/`
B. Restart the Cluster Agent
C. Change the Agent log level
D. Mount `/var/run/docker.sock`

**9.** Which env var or YAML key changes the Agent log level?
A. `verbose: true`
B. `log_level: debug`
C. `LOG_DEBUG=1`
D. `agent_loglevel=DEBUG`

**10.** The Agent runs but `agent status` shows the Forwarder dropping payloads. Which is the most useful next step?
A. Re-install the Agent
B. Inspect proxy/firewall and run `agent diagnose datadog-connectivity`
C. Reboot the host
D. Disable the Trace Agent

**11.** Logs aren't being collected from a Docker container even though `logs_enabled: true`. The most likely missing setting is:
A. `DD_PROCESS_AGENT_ENABLED=true`
B. `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true`
C. `DD_KUBERNETES=true`
D. `DD_APM_NON_LOCAL_TRAFFIC=true`

**12.** Which file path is used for Agent logs on Windows?
A. `C:\ProgramData\Datadog\logs\`
B. `C:\Datadog\logs\`
C. `C:\Windows\datadog\logs\`
D. `%APPDATA%\datadog\logs\`

**13.** Which subprocess inside the Agent handles APM trace ingestion?
A. process-agent
B. trace-agent
C. system-probe
D. security-agent

**14.** A host shows up twice (duplicate entries) in the Infrastructure list. The most common cause is:
A. The cloud integration was set up twice
B. Two Agents are running on the same host
C. The hostname was set explicitly and conflicts with the cloud-detected hostname (or instance was rebuilt)
D. A network outage caused a re-registration

**15.** A user runs `agent flare` without specifying a case ID. What happens?
A. The command fails
B. The command prompts for an email and creates a new support case
C. It uploads the bundle to a public URL
D. It writes to disk only and never uploads

---

## Answer Key

1. **B** — `datadog-agent status`.
2. **B** — `/var/log/datadog/`.
3. **B** — `flare <CASE_ID>`.
4. **C** — `agent status` shows per-check status.
5. **B** — `agent diagnose datadog-connectivity`.
6. **B** — `agent hostname`.
7. **B** — `datadog.agent.running` is the canonical heartbeat.
8. **A** — Custom checks need a matching `conf.yaml` to be scheduled.
9. **B** — `log_level: debug`.
10. **B** — Forwarder issues are typically network/proxy/firewall related.
11. **B** — `DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true`.
12. **A** — `C:\ProgramData\Datadog\logs\`.
13. **B** — `trace-agent`.
14. **C** — Hostname conflict / explicit override is the typical cause.
15. **B** — Prompts for email and creates a case.

> **Score: ___ / 15**
