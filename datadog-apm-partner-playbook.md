# Datadog APM — Partner Playbook

**Author:** Marcio Costa, Sr. Partner Solution Architect — GSI, Datadog Brazil
**Date:** 2026-05-21
**Audience:** Partner Solution Architects, GSI Practice Leads, Channel Sellers, Alliance Managers

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What Is Datadog APM?](#2-what-is-datadog-apm)
3. [Key Elements of Datadog APM](#3-key-elements-of-datadog-apm)
4. [Value Proposition by Element](#4-value-proposition-by-element)
5. [Competitive Differentiation](#5-competitive-differentiation)
6. [How to Position Datadog APM for Partners (GSI Focus)](#6-how-to-position-datadog-apm-for-partners-gsi-focus)
7. [Partner Service Catalog](#7-partner-service-catalog)
8. [Joint Go-to-Market Plays](#8-joint-go-to-market-plays)
9. [Discovery Questions for Partner-Led Deals](#9-discovery-questions-for-partner-led-deals)
10. [Common Objections and Responses](#10-common-objections-and-responses)
11. [Reference Architectures](#11-reference-architectures)
12. [Appendix — Quick Facts and Resources](#12-appendix--quick-facts-and-resources)

---

## 1. Executive Summary

Datadog APM (Application Performance Monitoring) is the **distributed tracing and code-level performance** capability of the Datadog platform. It is not a stand-alone tool — it is part of a **unified observability platform** that natively correlates traces with logs, infrastructure metrics, real-user monitoring (RUM), database telemetry, security signals, and CI test data.

For GSI partners (Accenture, Deloitte, Wipro, TCS, Infosys, Cognizant, Capgemini, etc.), Datadog APM represents a **multi-year services opportunity** anchored in three durable trends:

- **Cloud migration and modernization** — every monolith-to-microservices journey needs distributed tracing.
- **AI-native applications** — LLM observability (extension of APM) is now a board-level requirement.
- **FinOps and engineering efficiency** — APM data fuels cost-per-request and developer productivity narratives that CIOs are buying in 2026.

The partner value capture sits in **assessment, implementation, modernization, managed services, and platform engineering enablement** — not in reselling licenses alone.

---

## 2. What Is Datadog APM?

Datadog APM provides end-to-end visibility into application behavior by:

- Capturing **distributed traces** (every request as it flows across services, brokers, databases, and external APIs).
- Collecting **code-level telemetry** through auto-instrumentation libraries (Java, .NET, Go, Python, Node.js, Ruby, PHP, C++, Rust).
- Continuously profiling **CPU, memory, lock contention, I/O, and exceptions** in production with negligible overhead.
- Correlating application performance with **infrastructure, logs, network, real-user sessions, synthetic checks, database queries, and security findings** in a single pane of glass.

Crucially, Datadog APM is **OpenTelemetry-compatible**: it ingests OTLP traces and metrics natively, so customers can adopt the open standard without giving up the integrated experience.

---

## 3. Key Elements of Datadog APM

Below are the eleven elements every partner architect should be able to articulate cold.

### 3.1 Distributed Tracing (100% Ingestion + Intelligent Retention)

Every request can be traced end-to-end across distributed systems. Datadog ingests **100% of traces at the agent** and applies **Intelligent Retention** to keep all errors, all high-latency outliers, and a representative sample of normal traffic indefinitely searchable for 15 days (or longer with extended retention SKUs).

### 3.2 Service Catalog and Service Map

A dynamically generated topology of every service, its dependencies, ownership, deployment metadata, SLOs, and recent changes. The Service Catalog turns APM into the **single source of truth for "who owns what"** in the engineering organization.

### 3.3 Continuous Profiler

Always-on production profiling at the code-line level (~1% overhead). Surfaces the exact method, line, and frame consuming CPU, allocating memory, holding locks, or blocking on I/O. Available for Java, Python, Go, Node.js, Ruby, .NET, PHP.

### 3.4 Error Tracking

Automatic grouping of exceptions and errors by fingerprint, with stack-trace deduplication, regression detection, and Jira/PagerDuty/ServiceNow integration. Distinguishes new errors, regressions, and recurring issues.

### 3.5 Deployment Tracking

Every deployment is captured as a first-class event correlated with version tags. APM shows the exact deploy that introduced a regression and supports automatic rollback workflows.

### 3.6 Trace ↔ Log Correlation

Logs are auto-injected with `trace_id` and `span_id`. One click jumps from a slow span to the exact log lines emitted by that request — eliminating the "swivel-chair" debugging between tools.

### 3.7 Database Monitoring (DBM) Correlation

APM spans tied to actual SQL execution plans, lock waits, top normalized queries, and host-level database telemetry. Supports Postgres, MySQL, SQL Server, Oracle, MongoDB, Cassandra, Elasticsearch.

### 3.8 Real User Monitoring (RUM) ↔ APM Correlation

Browser and mobile user sessions linked to the exact backend traces, so partners can show "this user action triggered this slow database query" in one view. Mobile RUM covers iOS, Android, Flutter, React Native.

### 3.9 CI Visibility and Test Optimization

Test executions, flaky-test detection, and pipeline performance correlated with the APM data of the services being tested. Closes the loop between SDLC quality and production behavior.

### 3.10 Service Level Objectives (SLOs)

Native SLO objects built on APM metrics: latency, error rate, throughput. Burn-rate alerts, error-budget tracking, and historical SLO compliance reporting — usable directly by SRE and platform teams.

### 3.11 LLM Observability (APM for AI Applications)

Trace LLM calls, agentic workflows, RAG pipelines, prompt/response payloads, token usage, latency, and quality evaluations. Integrates with OpenAI, Anthropic, Bedrock, Vertex, Azure OpenAI, and custom models. **This is the fastest-growing element of APM in 2026** and the most strategic for GSIs building GenAI practices.

### 3.12 Watchdog (AI-Powered Anomaly Detection)

Proactive, unsupervised ML detection of anomalies in services, endpoints, queries, and deployments. Watchdog Insights and Watchdog RCA reduce manual investigation time and feed directly into Bits AI for autonomous root-cause analysis.

### 3.13 OpenTelemetry (OTel) Compatibility

Native ingestion of OTLP traces, metrics, and logs. Datadog Agent can act as an OTel Collector. Customers preserve their open-standard investment while consuming Datadog's correlated experience.

### 3.14 Live Search and Trace Analytics

Sub-second search across billions of spans with custom tag dimensions. Build analytics dashboards directly from trace data — e.g., "p99 checkout latency by tenant, region, and feature flag."

---

## 4. Value Proposition by Element

The table below maps each element to the buyer outcome partners should lead with.

| Key Element | Technical Value | Business Value | Buyer Persona |
|---|---|---|---|
| Distributed Tracing + Intelligent Retention | 100% ingest, no blind spots, kept errors and outliers forever | Faster MTTR, no "I wish we had captured that trace" moments | VP Eng, SRE Lead |
| Service Catalog + Map | Living architecture diagram, ownership clarity | Onboarding, reorg planning, compliance audits | CTO, Platform Eng |
| Continuous Profiler | Code-line-level cost attribution | 10–40% infra cost reduction, dev productivity | FinOps, VP Eng |
| Error Tracking | Auto-grouped exceptions, regression detection | Lower bug-to-production time, better customer experience | QA Lead, SRE |
| Deployment Tracking | Pinpoints which release broke things | Faster rollback, safer release velocity | Release Manager, DevOps |
| Trace ↔ Log Correlation | One-click pivot, unified context | Eliminates tool-switching, cuts investigation time by ~50% | Application Support |
| DBM Correlation | SQL plans tied to slow spans | Removes "is it the app or the DB?" finger-pointing | DBA, Backend Lead |
| RUM ↔ APM | Real user impact tied to backend | Quantify revenue impact of perf issues | Product, CMO |
| CI Visibility | Flaky test detection, pipeline analytics | Faster, more reliable releases | DevEx, Platform |
| SLOs | Error budgets, burn-rate alerting | Aligns engineering with business reliability targets | SRE, CIO |
| LLM Observability | Token, latency, prompt, eval tracing | De-risks GenAI in production | Head of AI, CIO |
| Watchdog | Unsupervised anomaly detection | Catches incidents pre-customer-impact | SRE, NOC |
| OTel compatibility | Open standard, no vendor lock-in at the data layer | Future-proof investment, board-friendly story | Architect, CIO |
| Live Search + Analytics | Sub-second trace analytics | Business-relevant KPIs from telemetry | Product, Eng Leadership |

### 4.1 Strategic Themes (How to Bundle the Value)

Partners rarely sell a feature — they sell an outcome. Bundle the elements above into four narratives:

**Theme 1 — "Modernize with confidence"**
Distributed Tracing + Service Map + Deployment Tracking + DBM. Sells into application modernization, monolith decomposition, and cloud migration programs. Anchors GSI modernization offerings.

**Theme 2 — "Run AI in production responsibly"**
LLM Observability + Watchdog + SLOs + RUM. Sells into every GenAI and agentic AI initiative. Highest growth area for 2026; GSIs already have AI practices that need this layer.

**Theme 3 — "Cut cloud spend without cutting performance"**
Continuous Profiler + APM + DBM + Cloud Cost Management integration. FinOps narrative — pairs beautifully with CIO cost-rationalization mandates.

**Theme 4 — "Platform engineering at scale"**
Service Catalog + SLOs + CI Visibility + Error Tracking + OTel. Sells into Internal Developer Platform (IDP) initiatives. GSIs delivering Backstage-style platforms layer Datadog as the observability backbone.

---

## 5. Competitive Differentiation

When a partner walks into a deal, expect to face at least one of these competitors. Memorize these one-liners.

| Competitor | Their Pitch | Datadog Counter |
|---|---|---|
| **Dynatrace** | "We have AI (Davis)." | Datadog has Watchdog *and* Bits AI *and* native LLM Observability. Datadog is one platform, not OneAgent + Cluster Management + Grail bolt-ons. Faster deployment, modern UX. |
| **New Relic** | "Per-user pricing is cheaper." | Per-user pricing punishes scale-out organizations and platform teams. Datadog's host/ingest model aligns with workload, and the integrated suite avoids the patchwork experience NR delivers. |
| **Splunk Observability (Cisco)** | "We unify with Splunk Enterprise." | Splunk is two acquisitions stitched together (SignalFx + Plumbr). Datadog was built unified from day one. Splunk's APM also lacks the depth of profiling, DBM, and LLM Observability. |
| **Grafana / OSS stack** | "It's open source and free." | TCO of a self-hosted Prometheus + Tempo + Loki + Grafana stack at enterprise scale is 3–5× higher than Datadog when you account for SRE headcount, retention storage, and the lack of correlation across signals. Datadog ingests OTel — customers keep the standard, lose the operational burden. |
| **AWS-native (X-Ray, CloudWatch)** | "It's included with AWS." | Single-cloud only. No correlation with on-prem, Azure, GCP, SaaS. No profiler. No DBM. No RUM. Fine for AWS-only POCs, breaks down in real enterprises. |
| **Azure Monitor / Application Insights** | "It's included with Azure." | Same single-cloud limitation. Most enterprises are multi-cloud + on-prem + SaaS — Datadog is the only consistent layer across them. |

---

## 6. How to Position Datadog APM for Partners (GSI Focus)

### 6.1 The Partner Value Equation

GSIs don't buy Datadog. Their **clients** buy Datadog. The GSI's job — and your job as a PSA — is to ensure Datadog shows up as the **enabling layer** inside the GSI's own offerings.

The math is simple:

> **For every $1 of Datadog ARR a GSI influences, the GSI captures $3–$7 in services revenue** (assessment + implementation + managed services + modernization).

This ratio is the single most important slide in any GSI conversation. It is why APM matters more than any other Datadog product: APM is the **anchor** that brings logs, infrastructure, RUM, DBM, and security along with it.

### 6.2 Three Plays GSIs Run Best

**Play 1 — Embedded in Cloud Migration / Modernization Programs**
- GSI is already running a 2-year modernization (e.g., Accenture's myWizard, Deloitte's Ascend, Wipro's FullStride).
- Position Datadog APM as the **landing-zone observability standard** before the first microservice ships.
- Partner sells: discovery → instrumentation → SLO definition → runbook automation → managed run.

**Play 2 — Platform Engineering / Internal Developer Platform (IDP)**
- GSI is building a Backstage-based or custom IDP for the client.
- Datadog APM + Service Catalog + SLOs become the **observability paved path** every developer onboards onto.
- Partner sells: platform design, golden-path tooling, developer enablement, ongoing operations.

**Play 3 — GenAI Production Enablement**
- GSI has a GenAI practice but the client's first three pilots hit walls around hallucination, cost, and latency.
- Datadog LLM Observability is the **only major APM-class product with native LLM tracing**.
- Partner sells: GenAI readiness assessment, observability instrumentation, eval framework setup, prod operations.

### 6.3 Where APM Lives in the GSI Value Chain

```
        ┌─────────────────────────────────────────────────────────┐
        │              GSI Engagement Lifecycle                    │
        ├─────────────────────────────────────────────────────────┤
        │ Advise → Design → Build → Migrate → Run → Optimize       │
        │   ▲        ▲       ▲       ▲       ▲       ▲             │
        │   │        │       │       │       │       │             │
        │   │        │       │       │       │       └─ Profiler,  │
        │   │        │       │       │       │          FinOps     │
        │   │        │       │       │       └─ SLOs, Watchdog,    │
        │   │        │       │       │          Managed Ops        │
        │   │        │       │       └─ Trace correlation,         │
        │   │        │       │          DBM, RUM                   │
        │   │        │       └─ Auto-instrumentation, OTel         │
        │   │        └─ Service Catalog, SLO design                │
        │   └─ Assessment, baseline, business case                 │
        └─────────────────────────────────────────────────────────┘
```

Every stage has an APM hook. Map your GSI's existing methodology onto this lifecycle to show where Datadog "plugs in" — never sell against the GSI's framework, sell **inside** it.

### 6.4 The "Land and Expand" APM Motion

Datadog land-and-expand inside a GSI account typically follows this arc:

1. **Land**: One business unit, one workload. APM on 3–5 critical microservices. Often paired with Infra monitoring on the same hosts.
2. **Expand horizontally**: APM rolled to adjacent services in the same BU. Logs follow within 90 days.
3. **Expand vertically**: DBM, RUM, and CI Visibility added to the same workload — full-stack coverage.
4. **Expand laterally**: Other BUs adopt the pattern. The GSI's playbook becomes the client's enterprise standard.
5. **Enterprise platform deal**: Multi-year, multi-product MSA. GSI managed-services contract attached.

The PSA's role at each stage is different — but in stages 1–3, your job is to **make the partner architect look like a hero in front of the client**. Bring reference architectures, instrument-with-them, and never run a workshop without a partner SA in the room.

---

## 7. Partner Service Catalog

The services below are what GSI practices wrap around Datadog APM. Use this list when building joint solution briefs.

### 7.1 Assessment & Strategy
- Observability maturity assessment (using Datadog's OMM framework)
- Application portfolio rationalization tied to APM coverage
- TCO and ROI modeling vs. incumbent tooling
- FinOps assessment using Continuous Profiler data
- AI/GenAI observability readiness assessment

### 7.2 Implementation
- Auto-instrumentation rollout (per language / per platform)
- OpenTelemetry collector design and deployment
- Service Catalog population and ownership mapping
- SLO design workshops and burn-rate alert configuration
- DBM rollout (per database engine)
- RUM instrumentation (web + mobile)
- LLM Observability instrumentation for GenAI workloads

### 7.3 Modernization
- Monolith decomposition guided by APM dependency graphs
- Database modernization using DBM baseline data
- Cloud migration validation (before/after performance evidence)
- Legacy app retirement scoring using APM usage data

### 7.4 Managed Services / Run
- 24×7 NOC built on Datadog dashboards, monitors, and Watchdog
- Incident response playbooks integrated with Bits AI
- SRE-as-a-service with error-budget governance
- Observability platform operations (multi-tenant, RBAC, governance)
- Custom integrations and bespoke instrumentation libraries

### 7.5 Platform Engineering Enablement
- Backstage + Datadog Service Catalog integration
- Golden-path templates with embedded instrumentation
- Developer enablement and training programs
- Internal observability champions program

---

## 8. Joint Go-to-Market Plays

### 8.1 The Five GTM Motions GSIs Will Co-Sell

1. **Cloud Migration Co-Sell** — bundled with AWS / Azure / GCP migration deals.
2. **SAP Observability** — APM for SAP RISE, BTP, and S/4HANA workloads.
3. **AI/GenAI Production Enablement** — LLM Observability + agent practice.
4. **Platform Engineering / IDP** — observability layer of a Backstage practice.
5. **Cost Optimization / FinOps** — Profiler-led infra cost reduction.

### 8.2 Co-Marketing Assets to Request

Every GSI relationship needs at least one of each:

- **Joint solution brief** (2-pager) per play above.
- **Reference architecture diagram** co-branded.
- **Customer reference** (one logo per region, one per industry).
- **Lighthouse webinar** (60 min, Datadog + GSI + customer).
- **Datadog Partner Network (DPN) status** — Premier or Select tier matters for marketing dollars.

### 8.3 QBR Anchors

In every QBR with a GSI, you must show:

- Pipeline influenced (in dollars and stage).
- Pipeline sourced (deals the GSI brought to Datadog).
- Joint wins (with named clients, ARR, and partner services revenue).
- Enablement metrics (certified architects, completed deliverables, joint POCs).
- Roadmap intersections (what Datadog is shipping that the GSI should preview).

---

## 9. Discovery Questions for Partner-Led Deals

Use these in joint sessions with the GSI's client. They are designed to surface APM-relevant pain *and* expand into adjacent Datadog products.

**Strategic / Business**
- What does a "good month" of application reliability look like? How do you measure it today?
- If your top-3 customer-facing services were 30% faster, what would change for the business?
- What percentage of your engineering hours go to debugging vs. building?
- How is GenAI in production going? What's blocking scale?

**Technical**
- How many services are in production? How many are instrumented today?
- When a customer reports a slow page, how long from ticket to root cause?
- How do you correlate a database slowdown with the application calls that caused it?
- Are you running OpenTelemetry today? At what layer?
- What is your incident review process? What inputs feed it?

**Commercial / Partner-friendly**
- Who owns the observability budget? Is it centralized or per-BU?
- What are your existing contracts with Dynatrace / New Relic / Splunk and when do they renew?
- Is there an active modernization or migration program where observability is in scope?

---

## 10. Common Objections and Responses

| Objection | Response |
|---|---|
| "We already have New Relic / Dynatrace." | Run a side-by-side POC on one critical workload. Datadog wins on correlation, deployment speed, and breadth. We've never lost a head-to-head technical POC when the customer engages with us properly. |
| "OpenTelemetry means we don't need a vendor." | OTel is a data standard, not a product. You still need ingest, storage, analytics, alerting, RBAC, retention, AI, and integrations. Datadog ingests OTLP natively — you get the standard *and* the platform. |
| "It's too expensive." | Compare TCO, not list price. Include the cost of incumbent tool + storage + SRE headcount + outage minutes saved. We routinely show 20–40% lower 3-year TCO. Profiler alone often pays for the platform via infra cost reduction. |
| "We're worried about lock-in." | All ingest is OTel-compatible. All data is exportable. No proprietary agent format required. The lock-in argument is reversed against legacy APM vendors. |
| "Our data can't leave our region / country." | Datadog has regional data residency: US1, US3, US5, EU1, AP1, AP2, and dedicated government regions. Brazil-resident workloads can be served via US regions with documented data-flow controls; check the latest Trust Center for current regional offerings. |
| "We don't want yet another agent." | The Datadog Agent replaces multiple agents (infra, APM, logs, network, security). Most customers reduce agent footprint after Datadog. |
| "Profiler will impact production." | Continuous Profiler overhead is benchmarked at ~1% CPU. It runs in production at the world's largest e-commerce, banking, and SaaS companies. |

---

## 11. Reference Architectures

### 11.1 Greenfield Microservices on Kubernetes (AWS / Azure / GCP)

```
   ┌──────────────────────────────────────────────────────────────┐
   │                     Kubernetes Cluster                        │
   │                                                                │
   │  ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐            │
   │  │ Svc A  │──▶│ Svc B  │──▶│ Svc C  │──▶│  DB    │            │
   │  └───┬────┘   └───┬────┘   └───┬────┘   └───┬────┘            │
   │      │            │            │             │                 │
   │      ▼            ▼            ▼             ▼                 │
   │   ┌──────────────────────────────────────────────┐            │
   │   │      Datadog Agent (DaemonSet) + Cluster      │            │
   │   │      Agent (Deployment) + Admission Ctrl      │            │
   │   └────────────────────┬─────────────────────────┘            │
   └────────────────────────┼─────────────────────────────────────┘
                            │ TLS
                            ▼
              ┌──────────────────────────┐
              │  Datadog SaaS (regional)  │
              │  APM • Logs • Metrics •   │
              │  RUM • DBM • LLM Obs •    │
              │  Watchdog • Bits AI       │
              └──────────────────────────┘
```

Key design notes: enable Admission Controller for automatic library injection (no app code changes), use Unified Service Tagging (`env`, `service`, `version`), and configure Cluster Agent for cost-efficient metric collection.

### 11.2 OpenTelemetry-First Architecture

```
   Applications ──(OTLP)──▶ OTel Collector ──(OTLP)──▶ Datadog Agent ──▶ Datadog
                                  │
                                  ├──(OTLP)──▶ Secondary backend (optional)
                                  │
                                  └── Sampling, batching, redaction, attribute mapping
```

Use when the customer mandates OTel. Datadog Agent can also act as the OTel Collector, simplifying the architecture to a single binary.

### 11.3 Hybrid / On-Premises with Egress Constraints

```
   On-Prem DC                           Datadog SaaS
   ┌─────────────────┐                 ┌───────────────┐
   │  Apps + DBs     │                 │               │
   │  Agents ────────┼──── HTTPS ─────▶│   Backend     │
   │  Cluster Agent  │   (proxy/PrivateLink optional)  │
   └─────────────────┘                 └───────────────┘

   Optional: deploy a forwarding proxy or use AWS PrivateLink
   for compliance-bound traffic.
```

### 11.4 GenAI / LLM Workload

```
   User ──▶ App ──▶ LLM Gateway ──▶ Provider (OpenAI/Anthropic/Bedrock)
                       │
                       ├── Prompt + Response (sampled)
                       ├── Tokens, latency, cost
                       ├── Eval scores (quality, hallucination)
                       └── Trace IDs ───────▶ Datadog LLM Observability
                                                          │
                                                          ▼
                                              Same UI as APM traces
                                              Linked to RUM session
                                              Linked to underlying DB calls
```

This is the architecture every GSI GenAI practice should standardize on for production-bound LLM workloads.

---

## 12. Appendix — Quick Facts and Resources

### 12.1 Talk-Track Cheat Sheet

- **"One platform, not a stitched suite."** APM, infra, logs, RUM, DBM, security, CI, LLM — all built on the same data model.
- **"100% trace ingestion + intelligent retention."** No sampling-at-source blind spots.
- **"OTel-native."** Keep the open standard, drop the operational burden.
- **"AI runs through APM."** Watchdog detects, Bits AI investigates, LLM Observability traces.
- **"Designed for the multi-cloud, polyglot reality."** Eight first-class language agents, every major cloud and on-prem.

### 12.2 Certifications Worth Pushing on Partner Architects

- Datadog Fundamentals (entry — every architect)
- Datadog APM & Distributed Tracing (core for any APM-led deal)
- Datadog Log Management (the natural attach)
- Datadog Database Monitoring (high-value, low-supply skill)
- Datadog Cloud SIEM (security crossover)

### 12.3 Resources

- Datadog Partner Network portal — partner.datadoghq.com
- Datadog documentation — docs.datadoghq.com
- Datadog Learning Center — learn.datadoghq.com
- Trust Center / Compliance — trust.datadoghq.com

---

*This document is a living playbook. Update at minimum quarterly to reflect product GA announcements (typically at DASH in summer and at AWS re:Invent in winter), partner program changes, and competitive shifts.*
