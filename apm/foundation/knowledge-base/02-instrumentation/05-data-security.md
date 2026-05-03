# APM Data Security

> **Domain 2 — Application Instrumentation**

Datadog APM offers multiple layers to keep sensitive data out of your traces.

## Built-in obfuscators (Agent-side)

The trace-agent ships with **default obfuscators** for common data types. They normalize values (e.g., turn literal `WHERE id = 42` into `WHERE id = ?`) and strip secrets.

Configurable in `datadog.yaml`:

```yaml
apm_config:
  enabled: true
  obfuscation:
    elasticsearch:
      enabled: true
      keep_values: [client_id, product_id]
      obfuscate_sql_values: [val1]
    mongodb:
      enabled: true
    http:
      remove_query_string: true
      remove_paths_with_digits: true
    redis:
      enabled: true
    memcached:
      enabled: true
    sql_exec_plan:
      enabled: true
```

| Obfuscator     | What it does                                                       |
| -------------- | ------------------------------------------------------------------ |
| SQL            | Replaces literals with `?`, strips comments, normalizes whitespace. |
| ElasticSearch  | Removes literal values from queries.                               |
| MongoDB        | Strips literal values from BSON queries.                           |
| Redis          | Strips arguments from sensitive commands (e.g., `AUTH`).           |
| HTTP           | Optionally strips query strings and digit-containing path segments. |

## DBM extra mode (Agent 7.63+)

```yaml
apm_config:
  sql_obfuscation_mode: "obfuscate_and_normalize"
```

Used to align span SQL with Database Monitoring's normalized fingerprints.

## Tag scrubbing (`DD_APM_REPLACE_TAGS`)

Apply regex-based redactions to any span tag.

YAML form:

```yaml
apm_config:
  replace_tags:
    - name: "*"                       # any tag
      pattern: "foobar"
      repl:    "REDACTED"
    - name: "function.request.headers.auth"
      pattern: "(?s).*"
      repl:    ""
    - name: "function.response.apiToken"
      pattern: "(?s).*"
      repl:    "****"
```

Env-var form (JSON array):

```bash
DD_APM_REPLACE_TAGS='[{"name":"*","pattern":"foobar","repl":"REDACTED"}]'
```

## Drop entire resources

```bash
DD_APM_IGNORE_RESOURCES='GET /healthz,(GET|POST) /metrics'
```

Useful for noisy probes and out-of-scope endpoints.

## Filter by required / rejected tags

```bash
DD_APM_FILTER_TAGS_REQUIRE='env:prod'
DD_APM_FILTER_TAGS_REJECT='secret:true'
```

## App-side scrubbing

You can also scrub before tags ever reach the Agent: the tracer's API lets you set, modify or drop tags inside `set_tag` / `on_span_start` hooks.

## Application Security & PII

For ASM, Datadog automatically scans payloads for PII patterns. For APM, **PII is the user's responsibility** — use the obfuscators and tag scrubbers above.

## Things to remember for the exam

- Default SQL obfuscation is **on** — span SQL appears with `?` placeholders out of the box.
- `DD_APM_REPLACE_TAGS` is the canonical knob for redacting arbitrary tag values.
- `DD_APM_IGNORE_RESOURCES` drops entire traces by resource regex.
- Obfuscation runs **at the Agent**, not in the application.
