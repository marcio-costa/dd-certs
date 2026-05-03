# Tagging & Unified Service Tagging

> **Domain 1 — APM Fundamentals**
> The single most-tested topic. Master this.

## Why tags matter

Tags are how Datadog **slices, filters and correlates** every piece of telemetry. APM, logs, infra metrics and RUM all share the same tag namespace, so a well-tagged service can be jumped between products in one click.

## Tag rules

- Tags must start with a letter and only use `[a-z0-9_:./-]`.
- Up to **200 characters**.
- Tags are case-sensitive on values; keys are forced to lowercase.
- `key:value` is the canonical form; bare values (without `:`) are valid but discouraged.

## The three reserved tags — Unified Service Tagging (USM)

USM unifies APM, logs, processes, and infrastructure metrics through three tags:

| Tag        | Purpose                                          | Example value     |
| ---------- | ------------------------------------------------ | ----------------- |
| `env`      | Environment (production, staging, dev)           | `prod`            |
| `service`  | Logical service name                             | `payment-api`     |
| `version`  | Build / release version                          | `2025.10.01-abc`  |

### How to set them

Per environment variable on the application:

```bash
DD_ENV=prod
DD_SERVICE=payment-api
DD_VERSION=2025.10.01-abc
```

In a Dockerfile:

```dockerfile
ENV DD_SERVICE="payment-api"
ENV DD_ENV="prod"
ENV DD_VERSION="2025.10.01-abc"
LABEL com.datadoghq.tags.service="payment-api"
LABEL com.datadoghq.tags.env="prod"
LABEL com.datadoghq.tags.version="2025.10.01-abc"
```

In Kubernetes pod metadata (recommended via `tags.datadoghq.com/*` labels):

```yaml
metadata:
  labels:
    tags.datadoghq.com/service: "payment-api"
    tags.datadoghq.com/env:     "prod"
    tags.datadoghq.com/version: "2025.10.01-abc"
```

### Automatic version tagging

Agent **7.52.0+** can derive the `version` tag automatically using a priority hierarchy:

1. Manually set `DD_VERSION`.
2. Container image tag (when not `latest`).
3. Git commit SHA (if available via `OCI` labels / Source Code Integration).

If image tags are sufficient, no further config is required.

## Span tags vs trace tags

- A **span tag** (custom attribute) is set on a single span: `span.set_tag('order.id', order_id)`.
- A **root-span tag** propagates to its descendants in some cases but not all — most analytics use the span where the tag was set.
- Custom tags appear in queries with the `@` prefix: `@order.id:12345`.

## Things to remember for the exam

- USM = `env`, `service`, `version`. **Memorize these three.**
- USM unlocks: deployment tracking, service-page version selector, log/trace correlation, version-comparison views.
- Custom span tag query syntax uses `@key:value`.
- Reserved tag values like `service:web-store* ` support wildcards.
