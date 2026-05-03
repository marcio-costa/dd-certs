# Exam Guide Summary — APM & Distributed Tracing Fundamentals (2025)

> Distilled from the official Datadog exam guide PDF. This is your one-page reference for what's actually on the test.

## Vital stats

| Item | Value |
|------|-------|
| Vendor | Kryterion (Webassessor) |
| Format | **Single-answer multiple choice only** (no select-N, no ordering) |
| Total questions | 90 (75 scored + 15 unscored pretest) |
| Scored questions | **75** |
| Duration | **2 hours** |
| Cost | $100 USD (some pilot exams $50) |
| Passing score | Not publicly disclosed |
| Validity | 3 years |
| Retakes | Up to 3 within a 180-day window |
| Recommended experience | Minimum 6 months hands-on with Datadog APM |

## Out-of-scope (NOT tested)

- Programming
- Systems architecture (high-level design)
- Software testing

The exam tests **using** Datadog APM, not building applications.

## Content outline (4 domains)

### 1. APM Fundamentals
- APM Rationale
- Datadog Approach to APM
- Tracing Architectures
- Language Level Differences — Automatic Instrumentation
- OOTB Tracer vs Community Tracer
- Tagging
- Retention Periods for APM Data

### 2. Application Instrumentation
- Datadog Tracing Libraries
- Instrumentation Types
- Datadog Agent Architecture
- Sampling vs Retention
- APM Data Security
- Profile Collection

### 3. Insight Discovery
- Software Catalog
- Search Syntax
- Trace Live vs Retained Search
- Profilers
- Deployment Tracking
- Error Tracking
- Span Summary
- Service Performance Dashboards

### 4. Troubleshooting Application using APM
- Using Trace Search (during incident)
- Monitors & Alerting

## Recommended Datadog knowledge (from the official guide)

- General APM concepts
- Datadog APM setup and use
- Datadog tracing libraries
- Instrumentation types
- Trace/Log Correlation
- Datadog APM features: Services, Search Syntax, Live vs Retained Search, Trace Search, Flame Graphs, Profile Collection, Deployment Tracking, Error Tracking, Monitors and Alerting, Span Summary

## Recommended docs to skim once (from the official guide)

- Getting Started with Tags
- APM & Continuous Profiler client libraries
- Profiler Visualizations
- Correlate APM Data with Other Telemetry
- Anomaly Monitor
- SLOs
- Datadog Lambda Extension
- Set Up the OpenTelemetry Collector
- Advanced Usage — `ddtrace` documentation

## Approximate domain weighting (used for the mock exam)

| Domain | Weight | Mock Qs |
|--------|--------|---------|
| Domain 1 | ~25% | 19 |
| Domain 2 | ~30% | 22 |
| Domain 3 | ~30% | 22 |
| Domain 4 | ~15% | 12 |
| **Total** | **100%** | **75** |
