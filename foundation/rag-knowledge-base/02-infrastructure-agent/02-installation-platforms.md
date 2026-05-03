# Domain 2.B — Installing the Agent on Different Platforms

> **Source docs:** `/agent/basic_agent_usage`, `/containers/docker`, `/containers/kubernetes`

The exam will ask you to recognize the right install method for each environment.

## 2.B.1 Linux

One-line script (RPM, DEB, SUSE):
```bash
DD_API_KEY=<YOUR_API_KEY> \
DD_SITE="datadoghq.com" \
DD_AGENT_MAJOR_VERSION=7 \
bash -c "$(curl -L https://install.datadoghq.com/scripts/install_script_agent7.sh)"
```

What it does:
1. Detects the distribution.
2. Adds the Datadog package repo.
3. Installs the `datadog-agent` package.
4. Writes `/etc/datadog-agent/datadog.yaml` with your `api_key` and `site`.
5. Starts the Agent via systemd.

Manually with the package manager:
```bash
# Debian/Ubuntu
sudo apt-get install datadog-agent
# RHEL/CentOS/Amazon Linux
sudo yum install datadog-agent
```

## 2.B.2 Windows

PowerShell (run elevated):
```powershell
$env:DD_API_KEY="<YOUR_API_KEY>"
$env:DD_SITE="datadoghq.com"
. { iwr -useb https://install.datadoghq.com/scripts/install_script_agent7.ps1 } | iex
```

Or download the **MSI installer** from the Datadog UI under *Integrations → Agent → Windows*.

Service name: `DatadogAgent`. Logs at `C:\ProgramData\Datadog\logs\`.

## 2.B.3 macOS

```bash
DD_API_KEY=<YOUR_API_KEY> bash -c "$(curl -L https://install.datadoghq.com/scripts/install_mac_os.sh)"
```

Mac is supported for development/laptop use; production hosts are typically Linux or Windows.

## 2.B.4 Docker

Run the official image with the host's docker socket and proc/cgroup mounted in:
```bash
docker run -d --name dd-agent \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /proc/:/host/proc/:ro \
  -v /sys/fs/cgroup/:/host/sys/fs/cgroup:ro \
  -e DD_API_KEY=<YOUR_API_KEY> \
  -e DD_SITE=datadoghq.com \
  -e DD_LOGS_ENABLED=true \
  -e DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL=true \
  -e DD_APM_ENABLED=true \
  -p 8125:8125/udp \
  -p 8126:8126/tcp \
  gcr.io/datadoghq/agent:7
```

Why the mounts:
- **Docker socket** → discover containers + read labels for Autodiscovery.
- **/proc** → host-level process/CPU/memory metrics.
- **/sys/fs/cgroup** → per-container resource usage.

## 2.B.5 Kubernetes

Two officially supported install methods:

### Helm chart (most common)
```bash
helm repo add datadog https://helm.datadoghq.com
helm install datadog-agent \
  --set datadog.apiKey=<YOUR_API_KEY> \
  --set datadog.site=datadoghq.com \
  -f datadog-values.yaml \
  datadog/datadog
```

A typical `datadog-values.yaml` will:
- Enable the Cluster Agent.
- Turn on `logs.enabled`, `apm.portEnabled`, `processAgent.enabled`.
- Configure `kubelet.tlsVerify` for managed K8s flavors.

### Datadog Operator (CRD-based)
You apply a `DatadogAgent` custom resource. Best for teams that prefer GitOps and want lifecycle management for both the Node Agent and Cluster Agent.

### What gets deployed
- **Node Agent**: a `DaemonSet` — one pod per node.
- **Cluster Agent**: a `Deployment` — usually 1–2 replicas; aggregates cluster-level data (events, KSM, HPA external metrics).
- Optional **Cluster Checks Runner**: a separate `Deployment` for offloading non-host checks (DBs, HTTP endpoints).

## 2.B.6 Cloud Integrations vs. Agent

For some sources, you don't run an Agent at all — you connect at the **cloud account** level:

- **AWS**: Datadog assumes an IAM role in your account. CloudWatch metrics, CloudTrail events, and tags pull through. You **still** install the Agent on EC2 instances if you want host-level metrics, logs, APM, etc.
- **Azure** and **GCP**: similar pattern using app registration / service account.

Cloud integrations and the Agent **complement** each other: the integration is for AWS/Azure/GCP-managed services; the Agent is for what runs inside your VM/container.

## 2.B.7 Serverless

- **AWS Lambda**: install the **Datadog Lambda Extension** as a layer (sidecar in the function's execution environment) **and** the Datadog tracing layer for your runtime. Or use the **Datadog Forwarder** Lambda to forward CloudWatch logs.
- **Azure Functions** and **Google Cloud Functions/Run** have analogous extensions.

## 2.B.8 Picking the Right Method (Cheat Sheet)

| Environment | Install method |
|---|---|
| Single Linux/Windows VM | One-line install script |
| Fleet of VMs | Config management (Ansible/Chef/Puppet) calling the install script |
| Standalone Docker host | Run the Agent container with socket + proc + cgroup mounts |
| ECS / Fargate | Datadog ECS task definition (sidecar) |
| Kubernetes | Helm chart **or** Datadog Operator |
| Lambda | Lambda Extension layer + tracer layer |
| AWS managed services (RDS, ELB, etc.) | AWS integration via IAM role — no Agent |
