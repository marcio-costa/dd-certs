# Trace Search Syntax

> **Domain 3 — Insight Discovery**

## Where to query

- **Trace Explorer** (Live or Indexed)
- **APM Monitors** (alerts on a query)
- **Notebooks**, **Dashboards**, **Span-based metrics**

All use the same **event-based search syntax**.

## Reserved attributes (no `@` prefix)

| Token             | Meaning                                            |
| ----------------- | -------------------------------------------------- |
| `service:`        | Service name (`service:checkout`).                 |
| `resource_name:`  | Resource (`resource_name:"GET /checkout"`).        |
| `operation_name:` | Span operation (`operation_name:redis.command`).   |
| `env:`            | Environment tag (`env:prod`).                      |
| `version:`        | Version tag (`version:2025.10.01`).                |
| `status:`         | `ok` / `error`.                                    |
| `host:`           | Host that emitted the span.                        |
| `span.type:`      | Span type (`web`, `db`, `cache`).                  |
| `trace_id:`       | Specific trace ID.                                 |
| `span_id:`        | Specific span ID.                                  |

## Custom span tags (with `@` prefix)

```text
@http.method:POST
@http.status_code:>=500
@db.statement:"SELECT * FROM orders"
@order.id:12345
```

## Numeric and duration filters

```text
@duration:>1s          # spans longer than 1 second
@duration:[500ms TO 2s]
@http.status_code:>=400
```

## Operators

| Operator   | Example                                         |
| ---------- | ----------------------------------------------- |
| AND (default) | `service:cart status:error`                  |
| OR         | `service:cart OR service:checkout`              |
| NOT / `-`  | `service:cart -resource_name:"GET /healthz"`    |
| Wildcard   | `service:web-*`                                 |
| Range      | `@duration:[100ms TO 1s]`                       |
| Quoted     | `resource_name:"POST /api/v1/orders"`           |
| Existence  | `_exists_:@order.id` / `_missing_:@order.id`    |

## Cross-span queries

You can target *trace* attributes (e.g. inspect ancestors):

```text
@span.parent.service:frontend service:backend
```

Find traces where a backend span has a frontend parent.

## Examples (memorize patterns)

```text
# All errors in payment-api last 15 min
service:payment-api status:error

# Slow checkouts
service:checkout resource_name:"POST /checkout" @duration:>1s

# Custom-tag query: find a specific user's traces
@user.id:42 service:web-store

# Multiple services with prefix wildcard
service:web-* status:error

# Exclude noisy resource
service:checkout -resource_name:"GET /healthz"
```

## Things to remember for the exam

- Reserved attributes have **no `@`**; custom span tags **always** start with `@`.
- Wildcards use `*`. Phrases need **double quotes**.
- Range syntax: `[A TO B]`.
- Same syntax in Live Search, Indexed Search, monitors, dashboards — learn once, use everywhere.
