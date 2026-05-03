# Quiz — Tagging Deep-Focus (15 Qs)

> Tagging shows up on most Datadog Fundamentals exams. This drill is intentionally repetitive.

---

**1.** The three Unified Service Tagging tags are: ___, ___, ___.
A. host, service, region
B. env, service, version
C. team, service, build
D. cluster, service, deploy

**2.** Which env vars set USTagging in a containerized application?
A. `ENV`, `SERVICE`, `VERSION`
B. `DD_ENV`, `DD_SERVICE`, `DD_VERSION`
C. `DATADOG_ENV`, `DATADOG_SERVICE`, `DATADOG_VERSION`
D. `TAGS`

**3.** Tags can be applied at all of the following layers EXCEPT:
A. `datadog.yaml` `tags:` block
B. Integration `conf.yaml`
C. DogStatsD per-datagram `#`
D. The TLS handshake to api.datadoghq.com

**4.** Which of these tag values is the LEAST safe from a cardinality perspective?
A. `env:prod`
B. `region:us-east-1`
C. `customer_email:user@example.com`
D. `service:checkout`

**5.** Which Kubernetes labels enable USTagging via the Datadog Admission Controller?
A. `app.kubernetes.io/env|service|version`
B. `tags.datadoghq.com/env|service|version`
C. `dd.tags/env|service|version`
D. `datadog/env|service|version`

**6.** A metric reported by the Agent inherits tags from (select THREE that always apply):
A. `tags:` in `datadog.yaml`
B. The integration's `conf.yaml`
C. Container labels (when running in a container)
D. The user's local shell variables

**7.** True or False: Tag keys are normalized to lowercase by Datadog.
A. True
B. False

**8.** True or False: The maximum length of a single tag (key+value) is 200 characters.
A. True
B. False

**9.** Which is the right way to tag with USTagging in a Linux systemd unit?
A. Set `Environment=DD_ENV=prod`, `Environment=DD_SERVICE=checkout`, `Environment=DD_VERSION=1.2.3`
B. Add a `tag.yaml` file in `/etc`
C. Pass `--ust env=prod,service=checkout` on the binary
D. Set them only in the Datadog UI

**10.** A team has set `DD_SERVICE=checkout` but **not** `DD_ENV` or `DD_VERSION`. The expected impact is:
A. APM, log, and metric correlation will be partial; Service Catalog deploy comparisons will be missing
B. Nothing — only `DD_SERVICE` is required
C. Datadog will refuse to ingest the metrics
D. Tags will be applied as `env:unknown`

**11.** Which view in the Datadog UI helps you identify high-cardinality custom metrics?
A. Service Catalog
B. Metrics → Summary
C. Notebooks
D. Watchdog

**12.** Which of the following is NOT a recommended tag key?
A. `team`
B. `cost_center`
C. `email`
D. `cluster`

**13.** Which is TRUE about reserved tag keys?
A. There are no reserved keys
B. Keys like `host`, `device`, `source`, `service`, `version`, `env` have special semantics in Datadog and shouldn't be redefined
C. You can shadow `host` to anything you like with no side effects
D. Reserved keys can only be set by the Cluster Agent

**14.** Which tag is automatically applied by the Agent when running in Kubernetes?
A. `kube_namespace`
B. `mood`
C. `gpu_arch`
D. `provider:datadog`

**15.** Which is the correct DogStatsD datagram with two tags?
A. `latency:235|ms|env:prod|service:api`
B. `latency:235|ms|#env:prod,service:api`
C. `latency:235|ms|tags=env:prod,service:api`
D. `latency:235|ms|tags:env=prod,service=api`

---

## Answer Key

1. **B** — env, service, version.
2. **B** — `DD_ENV`, `DD_SERVICE`, `DD_VERSION`.
3. **D**
4. **C** — Email is highly unbounded.
5. **B** — `tags.datadoghq.com/...`.
6. **A, B, C**
7. **A** — Lowercased.
8. **A** — 200 char total.
9. **A**
10. **A** — Partial correlation.
11. **B** — Metrics Summary shows distinct context counts.
12. **C** — Don't tag with `email`.
13. **B**
14. **A** — `kube_namespace` is auto-applied.
15. **B**

> **Score: ___ / 15**
