# Quiz — Domain 2: Infrastructure Deployment with Datadog (15 Qs)

> Time: ~15 min.

---

**1.** Which credential does the Datadog Agent require to send data to Datadog?
A. API key only
B. App key only
C. Both API key and App key
D. SSO token

**2.** Where is the main Agent configuration file on Linux?
A. `/var/log/datadog/datadog.yaml`
B. `/etc/datadog-agent/datadog.yaml`
C. `/opt/datadog/conf.yaml`
D. `~/.datadog/agent.yaml`

**3.** Which environment variable would override the `site` value in `datadog.yaml`?
A. `DATADOG_SITE`
B. `DD_AGENT_SITE`
C. `DD_SITE`
D. `DATADOG_REGION`

**4.** A user installs the Datadog Agent on a Linux VM but data never appears. The Agent logs say "403 Forbidden". Which is the MOST likely cause?
A. The Agent host firewall blocks 8125 inbound
B. Wrong API key, or wrong `site` setting
C. DogStatsD is not enabled
D. The Agent is too new for the backend

**5.** Which of the following is the correct Helm command to install the Datadog Agent into a Kubernetes cluster?
A. `kubectl apply datadog/agent`
B. `helm install datadog-agent --set datadog.apiKey=$KEY datadog/datadog`
C. `dd-agent install --kubernetes`
D. `kubectl create daemonset datadog`

**6.** A Datadog org is on the US3 site. The Agent should be configured with which `site` value?
A. `datadoghq.com`
B. `us3.datadoghq.com`
C. `us3.datadog.com`
D. `app.us3.datadoghq.com`

**7.** Which TWO components are typically deployed when using the Datadog Helm chart in Kubernetes? (select TWO)
A. Node Agent (DaemonSet)
B. Cluster Agent (Deployment)
C. Database Monitoring Sidecar (per pod)
D. Datadog UI (Service)

**8.** Which of the following does an **Application key** primarily authenticate?
A. The Agent submitting metrics
B. A user calling the management API
C. The Datadog UI itself
D. SSO sign-on

**9.** Which mounts are commonly required when running the Datadog Agent in a Docker container? (select THREE)
A. `/var/run/docker.sock`
B. `/proc/`
C. `/sys/fs/cgroup`
D. `/etc/datadog-secret`

**10.** A team needs an automation tool (Terraform) to create monitors. Which credential should they put in the CI environment?
A. The Agent's API key
B. An App key tied to a service account with the right roles
C. Their personal user password
D. The Cluster Agent token

**11.** The `datadog.yaml` file shows `site: datadoghq.com` but the org is on EU1. Which symptom will appear?
A. Metrics will arrive but logs won't
B. The Agent will fail intake with 403/404 errors
C. Data will appear under a generic "global" account
D. Hosts will appear with the wrong hostname

**12.** Which of the following best describes the Datadog Operator?
A. A Datadog employee role inside your org
B. A Kubernetes CRD-based installer for the Datadog Agents
C. The component that handles UDP DogStatsD packets
D. A CLI tool to bootstrap an Agent on Windows

**13.** A new team member needs read-only access to all dashboards. The fastest correct option is:
A. Share your API key with them
B. Add them to the Datadog Read-Only Role
C. Add them to the Datadog Admin Role to be safe
D. Create a custom role with all permissions

**14.** When installing on a Linux VM with the one-line script, which env vars are required at minimum?
A. `DD_API_KEY` and (if not US1) `DD_SITE`
B. `DD_USERNAME` and `DD_PASSWORD`
C. `DD_API_KEY` and `DD_APP_KEY`
D. `DD_AGENT_HOST` and `DD_AGENT_PORT`

**15.** Which statement is TRUE about Datadog AWS integration?
A. It requires an Agent on every EC2 to function
B. It pulls CloudWatch metrics and AWS resource tags into Datadog without needing an Agent
C. It captures process-level metrics on EC2 hosts
D. It replaces the need for the Lambda Extension

---

## Answer Key

1. **A** — API key.
2. **B** — `/etc/datadog-agent/datadog.yaml`.
3. **C** — `DD_SITE`.
4. **B** — 403 = bad credentials or wrong region (site).
5. **B** — Helm install with the official chart.
6. **B** — `us3.datadoghq.com`.
7. **A and B** — Node Agent + Cluster Agent.
8. **B** — App keys are for the management/read API and authorize as a user/service account.
9. **A, B, C** — Docker socket, proc, cgroup. (`/etc/datadog-secret` is invented.)
10. **B** — App key on a service account with limited roles.
11. **B** — Wrong site → intake errors.
12. **B** — A CRD-based K8s installer.
13. **B** — Add to the built-in Read-Only Role.
14. **A** — `DD_API_KEY` always; `DD_SITE` if not the default US1.
15. **B** — AWS integration is API-based; doesn't replace Agent for in-VM telemetry.

> **Score: ___ / 15**
