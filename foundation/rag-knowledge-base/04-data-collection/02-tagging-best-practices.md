# Domain 4.B — Tagging: The Most Tested Topic

> **Source docs:** `/getting_started/tagging/`, `/getting_started/tagging/unified_service_tagging/`, `/getting_started/tagging/assigning_tags/`

Tags are the single most important concept across the entire Datadog platform. Expect **multiple questions** about tagging on the exam.

## 4.B.1 What a Tag Is

A **tag** is a `key:value` pair (or a bare key) attached to a piece of telemetry. Tags let you slice, filter, and aggregate data across hosts, services, environments, regions, customers, deploys — anything you care about.

### Tag rules
- Lowercase ASCII letters, numbers, underscores, minuses, dots, slashes, colons.
- Must start with a letter; no leading colon or number.
- Tag **keys** are lowercased. Tag **values** are case-sensitive (but case-insensitive in queries by default).
- **Length limit:** 200 characters total. **Per-host tag limit** is large but finite.
- Reserved key prefixes: `host`, `device`, `source`, `service`, `version`, `env`.

## 4.B.2 Where Tags Come From

Tags can be applied at several layers — and they all add up on the metric:

| Layer | How |
|---|---|
| **Datadog UI / API** | Apply tags to a host directly (Infrastructure list → host → "Apply tags"). |
| **Cloud integration** | AWS/Azure/GCP tags on resources are imported as Datadog tags. |
| **`datadog.yaml`** | `tags:` block applies to **all** data from that Agent. |
| **`hostname:` and `tags:` in integration `conf.yaml`** | Per-integration tags. |
| **Container labels / pod annotations** | Datadog reads labels like `tags.datadoghq.com/env` automatically. |
| **DogStatsD `#` block in datagram** | Per-metric tags. |
| **Tracer libraries** | Per-trace tags via `DD_TAGS`, `DD_ENV`, `DD_SERVICE`, `DD_VERSION`. |
| **Logs pipelines / processors** | Tags added during log enrichment. |

A single metric will end up with the union of all tags from these layers.

## 4.B.3 Unified Service Tagging (USTagging)

The single most asked-about tagging topic. The three "reserved" tags that **must** be applied across metrics, traces, and logs to correlate them in Datadog:

| Tag | What it represents |
|---|---|
| **`env`** | Environment: `prod`, `staging`, `dev`. |
| **`service`** | Logical service name: `checkout`, `web-store`, `payment-api`. |
| **`version`** | Version of the service that emitted the data: `1.2.3`, git SHA, build number. |

When all three are set consistently, Datadog can:
- Group APM traces, logs, infrastructure metrics, and RUM under a single service in the **Service Catalog**.
- Show **deployment markers** on dashboards.
- Compare performance across versions in the APM UI.
- Drive correlated investigations from a single click.

### Setting USTagging — common methods

**Linux host (env vars before service start):**
```bash
DD_ENV=prod DD_SERVICE=checkout DD_VERSION=1.2.3 \
  java -javaagent:dd-java-agent.jar -jar app.jar
```

**Docker (in your app container):**
```dockerfile
ENV DD_SERVICE="checkout"
ENV DD_ENV="prod"
ENV DD_VERSION="1.2.3"
LABEL com.datadoghq.tags.env="prod"
LABEL com.datadoghq.tags.service="checkout"
LABEL com.datadoghq.tags.version="1.2.3"
```

**Kubernetes (pod template):**
```yaml
template:
  metadata:
    labels:
      tags.datadoghq.com/env: "prod"
      tags.datadoghq.com/service: "checkout"
      tags.datadoghq.com/version: "1.2.3"
  spec:
    containers:
      - name: app
        env:
          - name: DD_ENV
            valueFrom: { fieldRef: { fieldPath: metadata.labels['tags.datadoghq.com/env'] } }
          - name: DD_SERVICE
            valueFrom: { fieldRef: { fieldPath: metadata.labels['tags.datadoghq.com/service'] } }
          - name: DD_VERSION
            valueFrom: { fieldRef: { fieldPath: metadata.labels['tags.datadoghq.com/version'] } }
```

**ECS task definition (already comprehensive):**
```json
"environment": [
  {"name": "DD_ENV",     "value": "prod"},
  {"name": "DD_SERVICE", "value": "checkout"},
  {"name": "DD_VERSION", "value": "1.2.3"}
],
"dockerLabels": {
  "com.datadoghq.tags.env":     "prod",
  "com.datadoghq.tags.service": "checkout",
  "com.datadoghq.tags.version": "1.2.3"
}
```

## 4.B.4 Best-Practice Tag Set Beyond USTagging

Recommended additional standardized keys (consistent naming pays off):

| Key | Example | Use |
|---|---|---|
| `team` | `team:platform` | Routing notifications, ownership. |
| `region` | `region:us-east-1` | Filtering by AWS/GCP/Azure region. |
| `availability-zone` / `zone` | `zone:us-east-1a` | Failure-domain analysis. |
| `cluster` | `cluster:prod-eks-1` | Multi-cluster Kubernetes. |
| `namespace` | `namespace:checkout` | K8s namespace. |
| `pod_name` | (auto-applied) | K8s pod identity. |
| `cost_center` / `business_unit` | `cost_center:retail` | FinOps reporting. |

## 4.B.5 Tag Cardinality Pitfalls

Avoid tagging with values that explode in cardinality:
- `user_id`, `request_id`, `session_id`, `email`, `trace_id`
- Timestamps (`hour:14`)
- Anything generated per request

If you need to slice on these, put them in **logs** or **APM trace tags** — both are cheaper and built for high-cardinality dimensions.

## 4.B.6 Common Exam Questions

- "Which three tags make up Unified Service Tagging?" → `env`, `service`, `version`.
- "Why use Unified Service Tagging?" → Correlate metrics, traces, logs, RUM for the same service across deploys.
- "What happens if you tag a metric with `user_id`?" → Cardinality explosion = custom metrics overage.
- "Where can tags be assigned?" → Agent config, integration config, container labels/pod annotations, cloud integration import, manually in UI/API, DogStatsD per-datagram.
- "Difference between a tag key and a tag value?" → Keys are lowercased and indexed for filtering; values can be case-sensitive but are matched case-insensitively in queries.
