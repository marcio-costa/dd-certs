# Datadog Fundamentals — 4-Week Study Plan (2 hours/day)

> **Calibration:** Beginner level, English language, target exam date 4 weeks from start.
> **Daily commitment:** 120 minutes split into ~70 min reading + 30 min hands-on + 20 min quiz/recall.
> Plan total: **28 days × 2 h = 56 hours of focused study + 2 full mock exams.**

---

## How to use this plan

1. **Each morning** (or your study slot), open the day's section below.
2. Read the listed RAG knowledge base file(s) — these are the curated study notes.
3. Complete the **hands-on lab** (or skim it if you have no Datadog account yet — see "Lab setup").
4. Take the **daily mini-quiz** (file: `quizzes/daily-mini-quizzes.md`).
5. Mark the day complete in `progress-tracking/tracker.md`.

### Lab setup (one-time)
- Sign up for the **14-day free trial** at `datadoghq.com/free-datadog-trial/`.
- Install the Agent on a Linux VM (or Docker on your laptop).
- The trial covers the entire exam scope.

> If you finish the trial before the exam, that's okay — read-only exposure to the docs is enough to pass; hands-on just makes it stick faster.

---

## Week 1 — Foundations & the Agent

Goal: build the mental model. By Sunday night you should be able to explain "what the Agent is and what it does" without notes.

### Day 1 (Mon) — Orientation & Computer Fundamentals
- **70 min reading:** `rag-knowledge-base/01-computer-fundamentals/01-computer-fundamentals.md` and the top-level `README.md`.
- **30 min hands-on:** Sign up for the Datadog trial. Skim the UI: Dashboards → Host Map → Infrastructure → Monitors → Logs → APM. Don't configure anything yet.
- **20 min recall:** Mini-quiz Day 1 (10 Qs).

### Day 2 (Tue) — Agent Architecture
- **70 min reading:** `02-infrastructure-agent/01-agent-architecture.md`.
- **30 min hands-on:** Install the Agent on a Linux VM (one-line script). Run `datadog-agent status` and read every section. Identify each subprocess (trace-agent, process-agent, system-probe).
- **20 min recall:** Mini-quiz Day 2.

### Day 3 (Wed) — Installation Patterns
- **70 min reading:** `02-infrastructure-agent/02-installation-platforms.md`.
- **30 min hands-on:** Run the Agent in a Docker container. Verify it shows up in *Infrastructure list* alongside your VM. (Optional: install the Helm chart on a tiny `kind` or Minikube cluster.)
- **20 min recall:** Mini-quiz Day 3.

### Day 4 (Thu) — Sites, Keys, Org Setup
- **70 min reading:** `02-infrastructure-agent/03-keys-sites-org.md`.
- **30 min hands-on:** Create a new App Key for yourself. Create a Service Account and assign it the read-only role. Note where the API key lives in *Org Settings → API Keys*.
- **20 min recall:** Mini-quiz Day 4.

### Day 5 (Fri) — Networking & Firewall
- **70 min reading:** `03-networking-config/01-networking-firewall.md`.
- **30 min hands-on:** From your Agent host, run `curl -v https://api.datadoghq.com` (or your site's host) to confirm outbound 443 works. Check `datadog-agent diagnose datadog-connectivity`.
- **20 min recall:** Mini-quiz Day 5.

### Day 6 (Sat) — Integrations & Custom Checks
- **70 min reading:** `03-networking-config/02-integrations-checks.md`.
- **30 min hands-on:** Enable the **NGINX** integration (or Postgres, Redis — whatever you have). Edit `/etc/datadog-agent/conf.d/nginx.d/conf.yaml`. Run `agent check nginx` to see metrics flow.
- **20 min recall:** Mini-quiz Day 6.

### Day 7 (Sun) — DogStatsD + Week 1 Review
- **70 min reading:** `03-networking-config/03-dogstatsd.md` + skim `reference/quick-reference-card.md`.
- **30 min hands-on:** Send a custom metric: `echo -n "test.metric:42|g|#env:dev" | nc -4u -w0 127.0.0.1 8125`. Find it in *Metrics → Explorer*.
- **20 min recall:** **Domain Drill 1**: re-take `quizzes/02-infrastructure-agent-quiz.md` and the first half of `quizzes/03-networking-config-quiz.md`.

---

## Week 2 — Data Collection & Tagging

Goal: deeply understand metrics and tagging — these are heavily weighted.

### Day 8 (Mon) — Metric Types
- **70 min reading:** `04-data-collection/01-metrics-types.md`.
- **30 min hands-on:** Open *Metrics Explorer*; query `system.cpu.user{*} by {host}`, `system.load.norm.1`, `datadog.agent.running`. Try `.as_count()` vs `.as_rate()` on a count-typed metric.
- **20 min recall:** Mini-quiz Day 8.

### Day 9 (Tue) — Submission Methods Deep-Dive
- **70 min reading:** Re-read `04-data-collection/01-metrics-types.md` §4.A.2–4.A.4 and `03-networking-config/02-integrations-checks.md` §3.B.5.
- **30 min hands-on:** Write a tiny custom check (`/etc/datadog-agent/checks.d/hello.py`) that emits a gauge. Reload Agent. Verify it appears.
- **20 min recall:** Mini-quiz Day 9.

### Day 10 (Wed) — Tagging Foundations
- **70 min reading:** `04-data-collection/02-tagging-best-practices.md` §4.B.1–4.B.2.
- **30 min hands-on:** In `datadog.yaml` add `tags: [team:platform, env:dev]`. Restart Agent. Confirm new tags on `system.*` metrics. Tag the host manually in *Infrastructure*.
- **20 min recall:** Mini-quiz Day 10.

### Day 11 (Thu) — Unified Service Tagging
- **70 min reading:** `04-data-collection/02-tagging-best-practices.md` §4.B.3–4.B.4.
- **30 min hands-on:** Run any container app with `-e DD_ENV=dev -e DD_SERVICE=demo -e DD_VERSION=1.0` and verify the *Service Catalog* picks it up.
- **20 min recall:** Mini-quiz Day 11.

### Day 12 (Fri) — Cardinality & Custom Metrics Cost
- **70 min reading:** `04-data-collection/02-tagging-best-practices.md` §4.B.5 + re-read §4.A.4 of metric types.
- **30 min hands-on:** Open *Metrics → Summary*; sort by "distinct metrics" to see which are highest cardinality. Imagine the bill.
- **20 min recall:** Mini-quiz Day 12.

### Day 13 (Sat) — Events, Service Checks, Host Map
- **70 min reading:** `04-data-collection/03-events-service-checks-host-map.md`.
- **30 min hands-on:** Submit a custom event via `datadog-agent` API or DogStatsD. View it in *Events Explorer* and overlay it on a dashboard. Open *Host Map*; group by `availability-zone`.
- **20 min recall:** Mini-quiz Day 13.

### Day 14 (Sun) — Week 2 Review + Domain Drill
- **120 min:** Re-read `02-tagging-best-practices.md` end-to-end. Take **Domain Drill 2 + 3**: `quizzes/04-data-collection-quiz.md` (20 Qs) **and** `quizzes/quiz-tagging-focus.md` (15 Qs). Review wrong answers against the RAG.

---

## Week 3 — Visualization, Monitors, and Troubleshooting

Goal: be conversational about dashboards and monitors and recognize every troubleshooting symptom.

### Day 15 (Mon) — Dashboards
- **70 min reading:** `06-visualization-monitors/01-dashboards.md`.
- **30 min hands-on:** Build your first dashboard with 4 widgets: timeseries (CPU), top list (load by host), query value (Agent count), hostmap. Add a `$env` template variable.
- **20 min recall:** Mini-quiz Day 15.

### Day 16 (Tue) — Notebooks
- **70 min reading:** `06-visualization-monitors/01-dashboards.md` §6.A.6 + Datadog UI exploration.
- **30 min hands-on:** Create a notebook titled "First incident sim". Add a Markdown cell, a Timeseries cell, a Log Stream cell.
- **20 min recall:** Mini-quiz Day 16.

### Day 17 (Wed) — Monitors I (Metric, Threshold, Recovery)
- **70 min reading:** `06-visualization-monitors/02-monitors.md` §6.B.1–6.B.3.
- **30 min hands-on:** Create a metric monitor on `system.cpu.user{host:<your-host>} > 1` (low threshold so it fires). Watch it alert; mute it.
- **20 min recall:** Mini-quiz Day 17.

### Day 18 (Thu) — Monitors II (Multi-alert, Composite, Downtime)
- **70 min reading:** `06-visualization-monitors/02-monitors.md` §6.B.4–6.B.5.
- **30 min hands-on:** Convert your monitor to a multi-alert with `by {host}`. Then schedule a 1-hour downtime on it.
- **20 min recall:** Mini-quiz Day 18.

### Day 19 (Fri) — SLOs and Error Budgets
- **70 min reading:** `06-visualization-monitors/02-monitors.md` §6.B.6–6.B.7.
- **30 min hands-on:** Create a metric-based SLO: numerator = `sum:system.cpu.user{*}.as_count()`, denominator = `sum:system.cpu.user{*}.as_count()` (just to see the UI). Set target 99%.
- **20 min recall:** Mini-quiz Day 19.

### Day 20 (Sat) — Troubleshooting Toolkit
- **70 min reading:** `05-agent-troubleshooting/01-troubleshooting-toolkit.md`.
- **30 min hands-on:** Run every command listed: status, configcheck, diagnose, hostname, check, secret. Generate a flare locally (don't upload).
- **20 min recall:** Mini-quiz Day 20.

### Day 21 (Sun) — Week 3 Review + Mock Exam #1
- **30 min:** Re-read `reference/common-pitfalls.md`.
- **90 min:** Take **Mock Exam #1** (`simulations/mock-exam-1.md`, 75 Qs / 90 min). Score against the answer key.
- **End of day:** Identify the 2 weakest domains; copy their objectives into `progress-tracking/weak-areas.md`.

---

## Week 4 — Reinforcement, Weak-Area Drills, Final Mock

Goal: shore up gaps, get your confidence to "I'd be surprised if I failed."

### Day 22 (Mon) — Weak-Area Day 1
- **120 min:** Re-read the RAG files for your **two weakest domains** from Mock #1. Take the corresponding domain drills again. See `simulations/weak-area-drill-instructions.md`.

### Day 23 (Tue) — Weak-Area Day 2
- **120 min:** Same as Day 22 but for the next two weakest domains, *or* deepen Day 22's domains with hands-on labs.

### Day 24 (Wed) — Cross-domain integration day
- **70 min reading:** Re-read `reference/quick-reference-card.md` and `reference/common-pitfalls.md`.
- **30 min hands-on:** Set up a fully end-to-end demo: install Agent, enable an integration, send DogStatsD metrics, build a dashboard with template variables, create a monitor, schedule a downtime, create an SLO. Document every step.
- **20 min recall:** Mini-quiz Day 24.

### Day 25 (Thu) — Tags + USTagging Deep Pass
- **120 min:** Drill on tags. Read `04-data-collection/02-tagging-best-practices.md` again, take `quizzes/quiz-tagging-focus.md`, and check that you can list every layer where a tag can be applied.

### Day 26 (Fri) — Mock Exam #2 (Final)
- **120 min:** Take **Mock Exam #2** (`simulations/mock-exam-2.md`, 75 Qs / 90 min). Aim for ≥80%. If under 70%, push the exam date.

### Day 27 (Sat) — Light Day, Cheat Sheet, Lab
- **60 min:** Read every line of `reference/quick-reference-card.md` aloud.
- **30 min hands-on:** Walk through the Datadog UI for 30 min just clicking around — no goal, just absorb.
- **30 min:** Re-do any drill where you scored under 80%.

### Day 28 (Sun) — Pre-Exam Day
- **60 min:** Skim `reference/common-pitfalls.md` and your `progress-tracking/weak-areas.md`.
- **0 min hands-on:** Rest your brain.
- **30 min:** Light review only.
- **No new material.** Sleep early.

### Day 29 (Mon) — **EXAM DAY**
Show up rested. Use process of elimination. Flag uncertain questions and revisit. 75 scored + 15 pretest = 90 questions in 120 minutes (~80 sec/question average).

---

## Adjustments

- **If you fall behind by 1 day:** double up on a Saturday.
- **If you fall behind by 3+ days:** push the exam date by a week and continue from where you left off.
- **If Mock #1 < 60%:** spend Week 4 entirely re-reading the RAG and doing drills, push the exam to Week 5.
- **If Mock #1 ≥ 85%:** you're ahead — use Week 4 to deepen one specialty domain (e.g., Logs or APM) for your next certification.
