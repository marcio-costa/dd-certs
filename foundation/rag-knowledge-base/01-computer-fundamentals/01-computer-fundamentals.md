# Domain 1 — Computer Fundamentals

> **Why this is on the exam:** Datadog runs on top of an operating system, sends data over the network, and is most often deployed in cloud and containerized environments. The exam expects you to recognize the foundational concepts so the rest of the material makes sense.

---

## 1.1 Operating System Concepts

### Processes
- A **process** is a running instance of a program. Each process has a PID, memory space, file descriptors, and one or more threads.
- The kernel scheduler allocates CPU time among processes; you read CPU pressure on Linux with `top`, `htop`, `ps`.
- **Daemons** (Linux) and **services** (Windows) are long-running background processes — the Datadog Agent is one of them.
- Process states: running, sleeping, stopped, zombie. Datadog's `system.processes.*` metrics expose these states.

### File systems
- A file system organizes data on a block device into files and directories. On Linux, the root is `/`; on Windows, drives like `C:\`.
- Datadog Agent reads/writes its config in `/etc/datadog-agent/` (Linux) or `C:\ProgramData\Datadog\` (Windows).
- Disk usage metrics (`system.disk.used`, `system.disk.in_use`) come from polling the file system.

### Networking basics
- **TCP/IP** is the four-layer model: link → internet (IP) → transport (TCP/UDP) → application (HTTP, DNS).
- **Ports** are 16-bit identifiers (0–65535). Well-known: 80 (HTTP), 443 (HTTPS), 53 (DNS), **8125 (DogStatsD UDP)**, **8126 (APM trace ingest)**.
- **DNS** resolves hostnames to IPs. The Agent uses DNS to find Datadog endpoints (`api.datadoghq.com`, `app.datadoghq.com`, `metrics.agent.datadoghq.com`, etc.).
- **HTTP** is request/response over TCP. Datadog Agent talks to the backend over **HTTPS (443 outbound)**.

### Permissions
- Linux processes run as a user; the Agent typically runs as the `dd-agent` user.
- For some integrations (e.g., reading Docker socket, journald, host files), the `dd-agent` user must be added to a group (e.g., `docker`, `systemd-journal`) or granted file ACLs.

---

## 1.2 Cloud Computing Fundamentals

| Model | Examples | Datadog relevance |
|---|---|---|
| **IaaS** (Infrastructure as a Service) | AWS EC2, Azure VM, GCP Compute Engine | You install the Agent on the VM yourself. |
| **PaaS** (Platform as a Service) | Elastic Beanstalk, App Engine, Heroku | Use a Datadog buildpack or extension; no host access. |
| **SaaS** (Software as a Service) | Datadog itself, Slack, Salesforce | The vendor runs everything; you consume an API/UI. |
| **FaaS / Serverless** | AWS Lambda, Azure Functions, Cloud Run | Use Datadog Lambda Extension or Forwarder; no Agent on host. |

- **Region/site selection** is critical: Datadog's SaaS backend is regional (US1, US3, US5, EU1, US1-FED, AP1). All telemetry must be sent to **the site that matches your account** — see Domain 2.
- **Cloud integrations** (AWS, Azure, GCP) pull metrics/events through the cloud provider's APIs; they do **not** require the Agent to be running on each VM. Set up via crawler / API role.

---

## 1.3 Containerization Basics

### Docker
- A **container** packages an app with its dependencies as an immutable image. Containers share the host kernel (unlike VMs).
- Container engine = **runtime** (Docker Engine, containerd, CRI-O). The Agent reads container metadata from the runtime via the socket (`/var/run/docker.sock`) or CRI API.
- **Images** are pulled from registries (Docker Hub, ECR, GCR). `gcr.io/datadoghq/agent:7` is the official Agent image.
- One container per process is the convention; Datadog Agent typically runs in its own container alongside your app containers.

### Why this matters for Datadog
- The Agent uses container labels and pod annotations for **Autodiscovery** (Domain 3).
- It tags every metric/log/trace with container metadata (`container_name`, `image_name`, `kube_namespace`, etc.).
- Logs from containers are collected via the runtime's log driver or by tailing `/var/log/pods/*` (Kubernetes).

---

## 1.4 Orchestration: Kubernetes Basics

| Term | What it is |
|---|---|
| **Cluster** | A set of nodes managed by a control plane. |
| **Node** | A worker machine (VM/bare-metal) running kubelet. |
| **Pod** | The smallest deployable unit — one or more containers sharing network/storage. |
| **DaemonSet** | Runs **one pod per node** — how the Datadog Agent is deployed. |
| **Deployment** | Declares N replicas of a pod, managed by a ReplicaSet. |
| **Service** | A stable network endpoint that load-balances to a set of pods. |
| **Namespace** | Logical partition for objects within a cluster. |
| **ConfigMap / Secret** | Mechanisms to inject config / credentials into pods. |
| **Cluster Agent** | A separate Datadog component that aggregates cluster-level data, exposes external metrics for HPA, and reduces API server load. |

- Standard Datadog deployment: **Node Agent (DaemonSet)** + **Cluster Agent (Deployment)** + optional **Cluster Checks Runner**.
- Install via **Helm chart** (`datadog/datadog`) or the **Datadog Operator**.

---

## 1.5 Networking Concepts You Need

### Outbound connectivity
- All Datadog Agent traffic is **outbound HTTPS to port 443** to the backend endpoints for your site.
- If the host can't reach the internet directly, configure a **proxy** (Domain 3).
- The Agent does **not** require any inbound port from Datadog's side.

### Local listeners
| Port | Protocol | Used by |
|---|---|---|
| 8125 | UDP | DogStatsD (custom metrics, events, service checks) |
| 8126 | TCP | APM/trace receiver |
| 5000 | TCP | Agent local API (`agent status`, GUI) |
| 5001–5003 | TCP | Cluster Agent and Cluster Check Runner |
| 5555 | TCP | Cluster Agent admission controller (TLS) |

### DNS endpoints (per site)
- US1 (default): `*.datadoghq.com`
- US3: `*.us3.datadoghq.com`
- US5: `*.us5.datadoghq.com`
- EU1: `*.datadoghq.eu`
- US1-FED: `*.ddog-gov.com`
- AP1: `*.ap1.datadoghq.com`

---

## 1.6 Quick Self-Check

1. What is the difference between a container and a VM?
2. Which AWS service model places the responsibility for OS patching on you — IaaS, PaaS, or SaaS?
3. What Kubernetes object would you use to deploy the Datadog Agent on every node?
4. What outbound port does the Agent need open to send data to Datadog SaaS?
5. Why does the Datadog site you choose at signup matter?

> Answers in `quizzes/01-computer-fundamentals-quiz.md`.
