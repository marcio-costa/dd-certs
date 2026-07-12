# Log Management Fundamentals — Knowledge Base (RAG)

Curated study notes for the **Datadog Log Management Fundamentals** certification, organized by exam domain.
Backed by Datadog official docs (sourced via Context7 + the official exam guide PDF, January 2026 edition).

> **Exam at a glance:** 90 questions (75 scored + 15 pretest), 2-hour seat time, multiple-choice only, 3-year credential validity.
> Source: `log-management-fundamentals-exam-guide-jan2026.pdf` (uploaded).

---

## How to use this knowledge base

1. **Linear study** — Read the seven domain files in order; each maps to the exam content outline.
2. **As RAG retrieval** — Each file is self-contained markdown that any RAG retriever (Cursor, Continue, Notion AI, etc.) can ingest. Headings and bullets are kept exam-friendly.
3. **Quick lookups** — Use `glossary.md` for term definitions and `quick-reference.md` for the night-before cheat sheet.
4. **Spaced repetition** — After each domain file, run the matching quiz in `../quizzes/`.

---

## File map (mirrors the exam content outline)

| # | File | Exam domain | Subtopics |
|---|------|-------------|-----------|
| 1 | `01-logging-fundamentals.md` | Logging Fundamentals | Logging systems rationale, log sources, log formats, log emission |
| 2 | `02-log-collection.md` | Log Collection | Enabling log collection, log filtering & obfuscation |
| 3 | `03-log-parsing.md` | Log Parsing | Processors & pipelines, standard attributes, log composition |
| 4 | `04-search-filtering.md` | Log Searching & Filtering | Live vs. Explorer, search syntax, Explorer functionality |
| 5 | `05-log-analysis.md` | Log Analysis | Filtering & excluding, aggregations, visualizing |
| 6 | `06-log-utilization.md` | Log Utilization | Monitors & alerting, metric generation, exporting |
| 7 | `07-log-troubleshooting.md` | Log Troubleshooting | Agent issues, ingestion issues |
| — | `glossary.md` | All | A–Z term reference |
| — | `quick-reference.md` | All | One-page cheat sheet for exam day |

---

## Source documentation referenced

Primary docs cited across this knowledge base:

- Log Management overview — `docs.datadoghq.com/logs/`
- Host Agent Log Collection — `docs.datadoghq.com/agent/logs/`
- Agent Configuration Files — `docs.datadoghq.com/agent/configuration/agent-configuration-files/`
- Tags — `docs.datadoghq.com/getting_started/tagging/`
- Log Configuration (pipelines, processors, indexes, archives) — `docs.datadoghq.com/logs/log_configuration/`
- Log Explorer (search, facets, saved views, analytics, live tail) — `docs.datadoghq.com/logs/explorer/`
- Log Monitors — `docs.datadoghq.com/monitors/types/log/`
- Generate Metrics from Logs — `docs.datadoghq.com/logs/log_configuration/logs_to_metrics/`
- Sensitive Data Scanner — `docs.datadoghq.com/sensitive_data_scanner/`
- Log Management troubleshooting — `docs.datadoghq.com/logs/troubleshooting/`

---

## Topic-to-file quick lookup

| Topic | Find it in |
|---|---|
| `logs_enabled: true` | `02-log-collection.md` |
| Grok parser syntax | `03-log-parsing.md` |
| `@http.status_code` | `04-search-filtering.md` |
| Status remapper / Date remapper | `03-log-parsing.md` |
| Standard attributes (host, service, source, etc.) | `03-log-parsing.md` |
| Live Tail | `04-search-filtering.md` + `06-log-utilization.md` |
| Exclusion filters / index management | `06-log-utilization.md` |
| Generate metrics from logs | `06-log-utilization.md` |
| Archives (S3, Azure Blob, GCS) | `06-log-utilization.md` |
| Log monitors | `06-log-utilization.md` |
| Sensitive Data Scanner / scrubbing | `02-log-collection.md` |
| Agent flare / log collection troubleshooting | `07-log-troubleshooting.md` |
| Logging Without Limits™ | `01-logging-fundamentals.md` + `06-log-utilization.md` |
| Reserved attributes vs. standard attributes | `03-log-parsing.md` |
| Patterns / Transactions | `05-log-analysis.md` |
| Tags (host, env, service, version) | `02-log-collection.md` + `glossary.md` |
