# Datadog Fundamentals — 5-Day Sprint Plan (Mon–Fri, 2h/day)

**Target exam date:** Saturday, immediately after Friday's final review
**Daily budget:** 2 hours (120 minutes)
**Total study time:** 10 hours + Saturday's 1-hour pre-exam warm-up
**Calibration:** Senior PSA at Datadog with strong baseline + extra weight on **Agent Troubleshooting** and **Monitors / SLOs** (your self-identified weak spots)

> **Why this plan can work in 5 days:** you already have the full study system in this folder — RAG knowledge base, six per-domain quizzes, two 75-question mock exams, a quick-reference card and a common-pitfalls list. This plan is a *scheduling overlay* that compresses the 28-day plan into a 5-day sprint, double-weighting your weak areas. The "guarantee" is not magic — it is hands-on practice + Mock Exam #1 + the official Datadog practice exam, all done before exam day. If you score ≥ 80% on Mock #1 and ≥ 85% on the official practice, you will almost certainly pass.

---

## Pre-flight checklist (do this BEFORE Monday — 10 minutes)

- [ ] Confirm exam booking on [Kryterion Webassessor](https://www.webassessor.com/datadog) for Saturday (or book it now)
- [ ] Sign up for a [Datadog 14-day free trial](https://www.datadoghq.com/free-datadog-trial/) — needed for hands-on labs
- [ ] Have ready: Ubuntu VM (or local Linux), Docker Desktop, `kind` or `minikube` (only Day 3 lab needs the cluster)
- [ ] Print or pin: `foundation/rag-knowledge-base/reference/quick-reference-card.md` (for exam morning)
- [ ] Open `foundation/progress-tracking/tracker.md` and add a 5-day section

---

## Daily ritual (every day, same shape)

| Block | Time | Activity |
|---|---|---|
| **A** | 0–60 min | Read the day's RAG file(s) — active reading: re-read any sentence you can't paraphrase out loud |
| **B** | 60–95 min | Hands-on lab in your Datadog trial org |
| **C** | 95–115 min | Take the day's quiz (closed-book, timer on) |
| **D** | 115–120 min | Log results in `progress-tracking/results.md` + note confusing topics in `weak-areas.md` |

Stick to the 2-hour box. If a block runs long, cut the next block — *don't bleed into Day 2*.

---

## Day 1 — Monday — Foundations + Infrastructure & Agent Deployment

**Domain coverage:** Domain 1 (Computer Fundamentals) — skim, you have this from AWS ProServe + Domain 2 (Infrastructure Deployment) — primary focus

### Block A — Read (60 min)
- [ ] `rag-knowledge-base/01-computer-fundamentals/01-computer-fundamentals.md` — **skim** (10 min) — you know most of this from your ProServe background; focus only on Datadog-specific concepts (e.g., how containers map to Agent autodiscovery)
- [ ] `rag-knowledge-base/02-infrastructure-agent/01-agent-architecture.md` (20 min)
- [ ] `rag-knowledge-base/02-infrastructure-agent/02-installation-platforms.md` (15 min)
- [ ] `rag-knowledge-base/02-infrastructure-agent/03-keys-sites-org.md` (15 min) — **memorize:** API key vs Application key, all Datadog sites (US1, US3, US5, EU1, US1-FED, AP1)

### Block B — Hands-on lab (35 min)
- [ ] Install the Agent on a Linux VM using the one-liner from your Datadog org
- [ ] Open `/etc/datadog-agent/datadog.yaml` and identify: `api_key`, `site`, `tags`, `hostname`, `logs_enabled`
- [ ] Run `sudo datadog-agent status` — verify the host appears in **Infrastructure → Host map** within 2 minutes
- [ ] Bonus: install the Agent in a Docker container with `-e DD_API_KEY -e DD_SITE -e DD_TAGS="env:lab"`

### Block C — Quiz (20 min)
- [ ] `quizzes/01-computer-fundamentals-quiz.md` (15 questions) — warm-up; target ≥ 85%
- [ ] Log score in `progress-tracking/results.md`

### Block D — Log (5 min)
- [ ] Note anything you got wrong → `progress-tracking/weak-areas.md`

**End-of-day acceptance criterion:** You can explain — without looking — the Agent's three main processes (core/agent, trace-agent, process-agent) and where they send data.

---

## Day 2 — Tuesday — Networking, Agent Config & Data Collection

**Domain coverage:** Domain 3 (Networking & Agent Config) + Domain 4 (Data Collection — the broadest domain)

### Block A — Read (65 min)
- [ ] `rag-knowledge-base/03-networking-config/01-networking-firewall.md` (15 min) — outbound ports, proxy config
- [ ] `rag-knowledge-base/03-networking-config/02-integrations-checks.md` (15 min) — autodiscovery, check templates
- [ ] `rag-knowledge-base/03-networking-config/03-dogstatsd.md` (10 min) — protocol, port 8125, metric submission
- [ ] `rag-knowledge-base/04-data-collection/01-metrics-types.md` (10 min) — **memorize:** COUNT, RATE, GAUGE, HISTOGRAM, DISTRIBUTION
- [ ] `rag-knowledge-base/04-data-collection/02-tagging-best-practices.md` (10 min) — **unified service tagging:** `env`, `service`, `version`
- [ ] `rag-knowledge-base/04-data-collection/03-events-service-checks-host-map.md` (5 min)

### Block B — Hands-on lab (30 min)
- [ ] Send a custom metric via DogStatsD from a Python or bash script: `echo "lab.test:1|c|#env:lab" > /dev/udp/127.0.0.1/8125` — find it in **Metrics Explorer** within 1 min
- [ ] Enable Redis autodiscovery: run `docker run -d --name redis -p 6379:6379 -l "com.datadoghq.ad.checks={\"redisdb\":{\"init_config\":{},\"instances\":[{\"host\":\"%%host%%\",\"port\":\"6379\"}]}}" redis`
- [ ] Verify Redis integration appears in **Integrations → Tile** view

### Block C — Quiz (20 min)
- [ ] `quizzes/quiz-tagging-focus.md` (15 questions) — tagging is the highest-yield topic; mastery here is worth ≥ 5 questions on the real exam

### Block D — Log (5 min)

**End-of-day acceptance criterion:** Without looking, write out the five metric types and which one to use for "number of HTTP 500s per minute" (answer: COUNT or RATE; the Agent sends RATE downsampled per second).

---

## Day 3 — Wednesday — Agent Troubleshooting (DOUBLE WEIGHT — your weak area)

**Domain coverage:** Domain 5 (Troubleshooting the Datadog Agent) — deep dive

### Block A — Read (30 min)
- [ ] `rag-knowledge-base/05-agent-troubleshooting/01-troubleshooting-toolkit.md` (full read, take notes)
- [ ] `rag-knowledge-base/reference/common-pitfalls.md` (skim sections 1–10 — agent-related ones)
- [ ] **Commit to memory** these 5 commands:
  - `sudo datadog-agent status` — full status output
  - `sudo datadog-agent health` — readiness check
  - `sudo datadog-agent configcheck` — see all loaded check configs
  - `sudo datadog-agent flare` — bundle logs + config for Support
  - `sudo datadog-agent check <check_name>` — run a single check on demand

### Block B — Hands-on lab (60 min) — **the most important hour of the week**

Deliberately break the Agent **three times** and recover each one. After each break/fix, write a 2-sentence note on which command revealed the issue.

- [ ] **Break #1 — Bad API key:** edit `datadog.yaml`, set `api_key: BOGUSKEY`, restart Agent, run `agent status` and find the auth error. Fix it.
- [ ] **Break #2 — Wrong site:** set `site: datadoghq.eu` while your org is on US1. Restart. Confirm the host stops reporting in the UI. Fix.
- [ ] **Break #3 — Log permissions:** create `/tmp/lab.log`, configure log collection on it, then `chmod 600 /tmp/lab.log; chown root:root /tmp/lab.log`. Tail Agent logs (`/var/log/datadog/agent.log`) to see the permission-denied error. Fix by adding `dd-agent` to a group that can read the file.
- [ ] Run `agent flare` and inspect the zip contents — know what's in there (configs, status, logs, network info, system info)

### Block C — Quiz (20 min)
- [ ] `quizzes/05-troubleshooting-quiz.md` (15 questions) — target ≥ 90%; if below, immediately re-read the troubleshooting RAG file and retake

### Block D — Log (10 min)
- [ ] Document each of the 3 breaks in `weak-areas.md` with the recovery procedure

**End-of-day acceptance criterion:** You can list, from memory, what `agent flare` collects and where Agent logs live on Linux (`/var/log/datadog/`) and Windows (`C:\ProgramData\Datadog\logs\`).

---

## Day 4 — Thursday — Monitors, SLOs & Dashboards (DOUBLE WEIGHT — your weak area)

**Domain coverage:** Domain 6 (Data Visualization & Utilization) — deep dive

### Block A — Read (55 min)
- [ ] `rag-knowledge-base/06-visualization-monitors/01-dashboards.md` (25 min) — **memorize:** Timeboards vs Screenboards (now unified under "Dashboards"), widget types, template variables, scope vs filter
- [ ] `rag-knowledge-base/06-visualization-monitors/02-monitors.md` (30 min) — **memorize:**
  - Monitor types: **Metric, Anomaly, Outlier, Forecast, Composite, Log, APM, Event, Process, Network, Custom Check, Watchdog, SLO, Audit, Live Process, Database Monitoring**
  - States: **OK, Warn, Alert, No Data**
  - Notification options: `@user`, `@team`, `@slack-`, `@webhook-`, `@pagerduty-`, downtimes, recovery thresholds, no-data timeout, renotification

### Block B — Hands-on lab (45 min)

Build all three in your trial org:

- [ ] **Lab 1 — Composite Monitor (15 min):** create a metric monitor on `system.cpu.user > 80%` AND a log monitor on `status:error` from one service, then combine them into a composite that alerts only when **both** fire. Test by tailing logs and running `stress -c 4`.
- [ ] **Lab 2 — Metric-based SLO (15 min):** define an SLO of 99.9% availability over 7 days based on the success/total ratio of a synthetic HTTP test. Verify the error budget calculation.
- [ ] **Lab 3 — Dashboard with template variable (15 min):** create a dashboard with widgets filtered by a `$env` template variable; flip the value between `prod` / `lab` and watch the widgets update.

### Block C — Quiz (15 min)
- [ ] `quizzes/06-visualization-monitors-quiz.md` (20 questions) — target ≥ 85%

### Block D — Log (5 min)

**End-of-day acceptance criterion:** Without looking, explain the difference between a **metric-based SLO** and a **monitor-based SLO** and when you'd choose each.

---

## Day 5 — Friday — Full Simulation + Targeted Review + Test-Day Prep

This is your dress rehearsal. Treat it like the real exam.

### Block A — Mock Exam #1 (90 min) — closed book, timer ON

- [ ] Open `simulations/mock-exam-1.md` — 75 questions, 90-minute timer (the real exam is 90 questions / 120 min, so your pace target is **~75 sec/question**)
- [ ] No pausing, no lookups, no breaks. Flag questions you're unsure about and only revisit at the end.

### Block B — Score + targeted review (20 min)

- [ ] Score the exam using the answer key at the bottom
- [ ] Compute per-domain breakdown — any domain below **80%** is a "Friday-night priority"
- [ ] Re-read the RAG file for each weak domain (skim only — focus on the topics tied to questions you missed)

### Block C — Pre-exam essentials (10 min)

- [ ] Read `rag-knowledge-base/reference/quick-reference-card.md` end-to-end
- [ ] Read `rag-knowledge-base/reference/common-pitfalls.md` — the 20 traps the exam loves

### Block D — Test-day logistics (rest of session, ~0–5 min if you're efficient)

- [ ] Confirm Kryterion appointment time + check time zone
- [ ] **Remote proctor:** quiet room, clean desk, second monitor disconnected, phone out of reach. Test webcam + ID ready.
- [ ] Plan a 7-hour sleep window tonight — *do not cram past 9 pm*

### Optional evening bonus (off the 2h budget, only if score on Mock #1 was < 80%)

- [ ] Take Datadog's official 25-question free practice exam: [learn.datadoghq.com/courses/datadog-fundamentals-practice-exam](https://learn.datadoghq.com/courses/datadog-fundamentals-practice-exam) — closest stylistic match to the real exam

**Pass/no-pass gate:** If Mock Exam #1 score ≥ 80% AND official practice ≥ 85%, you are exam-ready. If either is below, do Mock Exam #2 Saturday morning (only if your exam slot is afternoon) — see "Contingency" below.

---

## Saturday — Exam Day

**Do not study new material.** Cramming on exam morning hurts more than it helps.

### Morning routine (60 min before exam)

- [ ] 15 min: Re-read `quick-reference-card.md` only
- [ ] 15 min: Re-read `common-pitfalls.md` only
- [ ] 10 min: Eat a real breakfast — protein + slow carbs, not just coffee
- [ ] 10 min: Hydrate, restroom, set up test environment, run Kryterion's system check
- [ ] 10 min: Sit still, deep breaths — your brain works better on calm than caffeine

### During the exam

- **Pace:** 90 questions in 120 min = **80 seconds per question**. If you're stuck after 90s, **flag and move on**.
- **Review at end:** budget 15 min at the end for flagged questions. Trust your first instinct unless you find concrete evidence to change.
- **Multi-select questions** (rare but they happen): the prompt will say "select TWO" or "select all that apply" — count your boxes before submitting.
- **Read the question fully**: the most common trap is missing a qualifier like "EXCEPT", "NOT", "MOST efficient", "FIRST step".

---

## Why this plan can guarantee a pass (and the honest fine print)

The "guarantee" is the result of stacking high-probability signals before exam day:

1. **All 6 domains covered** with the official RAG knowledge base
2. **Hands-on practice on every domain** — most exam questions test scenario application, not pure recall
3. **Mock Exam #1 (75 Q) under timed conditions** — the strongest predictor of real-exam performance
4. **Datadog's official 25-Q practice exam** — written by the same team that writes the real exam
5. **Double weighting on your two weak areas** (Agent Troubleshooting + Monitors/SLOs) — collapses the most likely failure modes
6. **Quick-reference + common-pitfalls** on exam morning — patches any last-minute leaks

**The honest part:** no plan can guarantee a pass without you executing it. The pass-rate predictor is the score on Mock Exam #1 + the official practice. If you hit ≥ 80% on Mock #1 by Friday, your odds of passing Saturday are very high (Datadog does not publish the cut score, but community reports place it around 70–75%).

---

## Contingency — if Friday's mock score is below 80%

- **Saturday morning (if exam is afternoon):** do `simulations/mock-exam-2.md` cold, then read the RAG files matching your two weakest domains. Otherwise: reschedule the exam by 2–3 days and add a Saturday + Sunday review using `simulations/weak-area-drill-instructions.md`. Kryterion allows rescheduling up to 24 hours before the exam at no fee.
- **If you score < 70% on Mock #1:** reschedule. Don't burn an attempt.

---

## Tracker — check off each block as you finish

| Day | Block A (Read) | Block B (Lab) | Block C (Quiz) | Score | Done |
|---|---|---|---|---|---|
| Mon — Foundations & Infra | ☐ | ☐ | Quiz 01 ☐ | ____% | ☐ |
| Tue — Networking & Data Collection | ☐ | ☐ | Quiz tagging-focus ☐ | ____% | ☐ |
| Wed — Agent Troubleshooting | ☐ | ☐ | Quiz 05 ☐ | ____% | ☐ |
| Thu — Monitors / SLOs / Dashboards | ☐ | ☐ | Quiz 06 ☐ | ____% | ☐ |
| Fri — Mock Exam #1 + Review | Mock ☐ | ☐ | Official 25-Q ☐ | ____% / ____% | ☐ |
| Sat — Exam Day | Reference card ☐ | — | **EXAM** ☐ | PASS/FAIL | ☐ |

---

## Quick map — what to open when

| If you need... | Open this |
|---|---|
| Today's plan | `study-plan/study-plan-5day.md` (this file) |
| Read a topic | `rag-knowledge-base/INDEX.md` (jump table) |
| Drill a domain | `quizzes/0X-*.md` |
| Tagging deep dive | `quizzes/quiz-tagging-focus.md` |
| Practice the full exam | `simulations/mock-exam-1.md` then `mock-exam-2.md` |
| Exam-morning cheat sheet | `rag-knowledge-base/reference/quick-reference-card.md` |
| Traps to avoid | `rag-knowledge-base/reference/common-pitfalls.md` |
| Log results | `progress-tracking/results.md` |

---

> **Source guide:** Datadog Fundamentals Exam Guide (January 2026) — 6 domains, 90 questions, 120 min, $100, Kryterion-proctored.
> **Calibration:** Senior PSA at Datadog • weak areas: Agent Troubleshooting + Monitors/SLOs • exam: Saturday after this plan.
> **Plan version:** 5-day sprint v1 — May 2026.
