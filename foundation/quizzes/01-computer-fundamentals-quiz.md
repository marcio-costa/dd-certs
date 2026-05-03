# Quiz — Domain 1: Computer Fundamentals (15 Qs)

> Time: ~15 min. Cover the answers below. Mark your score in `progress-tracking/tracker.md`.

---

**1.** Which of the following is NOT a property of a container compared to a virtual machine?
A. Shares the host kernel
B. Packaged with its dependencies as an image
C. Includes its own kernel and OS
D. Started in seconds rather than minutes

**2.** A team has many web servers behind an Application Load Balancer in AWS. Which model is responsible for keeping the underlying VM operating system patched?
A. The cloud provider, because it's IaaS
B. The cloud provider, because it's PaaS
C. The customer, because it's IaaS
D. The customer, because it's SaaS

**3.** Which Kubernetes object would you use to deploy the Datadog Agent on every worker node?
A. Deployment
B. StatefulSet
C. DaemonSet
D. Job

**4.** Which protocol/port does the Datadog Agent's DogStatsD server listen on by default?
A. TCP 8126
B. UDP 8125
C. TCP 443
D. UDP 53

**5.** What outbound port must be open from the Datadog Agent host to send all telemetry to Datadog SaaS?
A. 80
B. 443
C. 8125
D. 8126

**6.** A team running on AWS EU-West-1 wants their data to stay in Europe. Which Datadog site should they choose?
A. US1
B. EU1
C. US3
D. AP1

**7.** Which of the following is NOT a typical Kubernetes object?
A. Pod
B. Service
C. Namespace
D. Container Engine

**8.** Why are container labels and Kubernetes pod annotations relevant to the Datadog Agent? (select TWO)
A. They drive Autodiscovery configuration
B. They determine the host's CPU quota
C. They surface as tags on collected telemetry
D. They override the API key

**9.** Which protocol does the Datadog APM trace receiver use?
A. UDP on 8125
B. TCP on 8126
C. HTTPS on 443 (only outbound)
D. gRPC on 50051

**10.** Of these AWS services, which is the best example of a **PaaS**?
A. EC2
B. S3
C. AWS Elastic Beanstalk
D. CloudWatch

**11.** Which of the following is true about the Datadog Cluster Agent in Kubernetes?
A. It replaces the Node Agent
B. It reduces load on the Kubernetes API server and provides external metrics for HPA
C. It runs as a DaemonSet
D. It is mandatory for any K8s install

**12.** Where does the Datadog Agent typically read host-level CPU and memory metrics from on Linux?
A. The container runtime via /var/run/docker.sock
B. The /proc and /sys file systems
C. The kubelet API
D. CloudWatch

**13.** Which of the following sites does NOT exist as a Datadog region?
A. US1
B. EU2
C. US5
D. AP1

**14.** Which port on the Datadog Agent does the local web GUI use on Linux?
A. 5000
B. 5002
C. 8125
D. 8500

**15.** Which AWS service model removes the need to install the Agent on a host but still requires you to instrument code with Datadog libraries?
A. EC2 (IaaS)
B. ECS on EC2
C. Lambda (FaaS / Serverless)
D. RDS (Managed DB)

---

## Answer Key

1. **C** — A container shares the host kernel; it does NOT have its own kernel.
2. **C** — In IaaS the cloud provider gives you the VM; OS patching is the customer's responsibility.
3. **C** — DaemonSet runs one pod per node, the standard pattern for Agents.
4. **B** — DogStatsD = UDP 8125.
5. **B** — All Agent telemetry flows over outbound HTTPS 443 to the regional endpoints.
6. **B** — EU1 (`datadoghq.eu`) is the EU site.
7. **D** — "Container engine" is a runtime (Docker, containerd), not a Kubernetes object.
8. **A and C** — Autodiscovery is driven by container labels / pod annotations; they also become tags on telemetry.
9. **B** — APM trace receiver listens on TCP 8126.
10. **C** — Elastic Beanstalk abstracts the OS/runtime — classic PaaS.
11. **B** — The Cluster Agent reduces API server load and supplies external metrics for HPA.
12. **B** — `/proc` and `/sys` (mounted into the Agent container in containerized installs).
13. **B** — There is no EU2; sites are US1, US3, US5, EU1, US1-FED, AP1.
14. **B** — `http://127.0.0.1:5002` (Linux). Port 5000 is the IPC API.
15. **C** — Lambda runs without an Agent host; you add the Datadog Lambda Extension/layer and instrument code.

> **Score: ___ / 15**  → if < 11 right, re-read `01-computer-fundamentals/01-computer-fundamentals.md`.
