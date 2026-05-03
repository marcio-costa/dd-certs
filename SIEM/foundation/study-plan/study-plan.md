# 4-Week Study Plan — Datadog Cloud SIEM for AWS Fundamentals

**Daily commitment:** 2 hours
**Total commitment:** ~56 hours across 28 days
**Target outcome:** Pass the Datadog Cloud SIEM for AWS Fundamentals certification on first attempt.

> Tip: Before you start each session, open the relevant knowledge base file under `foundation/knowledge-base/` and the matching quiz under `foundation/quizzes/`. End each day by running the **Daily Mini-Quiz** (10 questions / ~10 minutes).

---

## How the 2 hours are structured every day

| Block | Length | Activity |
|---|---|---|
| **Block 1 — Read & note** | 50 min | Read the day's KB section. Take 1-page notes in your own words. |
| **Block 2 — Hands-on or Datadog Learning Center** | 50 min | If you have a Datadog trial: do the matching lab. If not: watch the matching Cloud Security Engineer Learning Path video and follow along. |
| **Daily Mini-Quiz** | 10 min | 10 questions from `foundation/simulations/daily-mini/`. Score honestly. |
| **Reflection & gap log** | 10 min | Write 3 things you learned + 1 thing you still don't understand. Add the gap to your weak-areas list. |

---

## Week 1 — Foundations & Setup (Domains 1 & 2)

### Day 1 — Exam orientation
- Read `knowledge-base/00-exam-overview.md`
- Skim the actual exam guide PDF you have
- Watch: *Cloud SIEM Learning Path — "Introduction to Datadog Cloud SIEM"*
- **Quiz 00 — Exam mechanics & scope** (10 Q)

### Day 2 — Cloud SIEM architecture & workflow
- Read `01-cloud-siem-fundamentals.md` §1.1–1.2
- Trace the end-to-end log flow on a whiteboard from your memory
- Watch: *Cloud SIEM Architecture* video
- **Quiz 01-A — Architecture** (10 Q)

### Day 3 — AWS log sources you must memorize
- Read `01-cloud-siem-fundamentals.md` §1.3–1.5
- Hands-on (if trial): enable CloudTrail in a sandbox AWS account, ship to Datadog via Lambda Forwarder
- Make a flashcard set: AWS source → use case → Datadog `source:` tag
- **Quiz 01-B — AWS log sources** (10 Q)

### Day 4 — Cloud SIEM data security
- Read `01-cloud-siem-fundamentals.md` §1.6
- Read Cloud SIEM Data Security doc
- Map out RBAC permissions on paper for SIEM (read signals, manage rules, manage filters)
- **Quiz 01-C — Data security & RBAC** (10 Q)

### Day 5 — Log management & indexing strategy
- Read `02-datadog-setup.md` §2.1
- Hands-on: in Datadog, create an Index, then add an Index filter and an Exclusion filter; observe the difference
- Build a comparison table (you draw it from memory): Standard Indexing vs Flex Logs vs Archives
- **Quiz 02-A — Indexing & retention** (10 Q)

### Day 6 — AWS integration & log collection
- Read `02-datadog-setup.md` §2.2
- Watch: *AWS Configuration Guide — Lambda Forwarder*
- Watch: *AWS Configuration Guide — Amazon Data Firehose*
- Memorize: when to use which delivery method
- **Quiz 02-B — AWS log collection** (10 Q)

### Day 7 — Week-1 review + Domain Drill
- Re-read your notes and any items in your weak-areas log
- Run **Domain Drill #1** (15 questions covering Domain 1) and **Domain Drill #2** (15 questions covering Domain 2)
- Review every wrong answer against the KB

---

## Week 2 — Detection & SOAR (Domains 3 & 4)

### Day 8 — Log processing & normalization
- Read `02-datadog-setup.md` §2.3
- Hands-on: Logs → Pipelines → clone an OOTB integration pipeline, add an Attribute Remapper
- Memorize the **reserved attributes** that need their own remapper
- **Quiz 02-C — Pipelines & processors** (10 Q)

### Day 9 — Cloud SIEM configuration & verification
- Read `02-datadog-setup.md` §2.4
- Hands-on: enable Cloud SIEM in Datadog. Inspect the auto-created index. Verify priority order.
- Try the API call for creating a Security Filter (use an Application Key in your sandbox)
- **Quiz 02-D — SIEM configuration** (10 Q)

### Day 10 — Detection rule components
- Read `03-security-monitoring-threat-detection.md` §3.1
- Make flashcards for: query, group by, trigger, rule cases, severity, MITRE tactic
- Hands-on: open an OOTB rule and read every field
- **Quiz 03-A — Rule components** (10 Q)

### Day 11 — Detection rule types
- Read `03-security-monitoring-threat-detection.md` §3.2
- Memorize the rule-type table; especially **New Value**, **Impossible Travel**, **Anomaly**, **Signal Correlation**, **Third Party**
- Watch: *Detection Rule Types in Cloud SIEM*
- **Quiz 03-B — Rule types** (10 Q)

### Day 12 — OOTB rule management & custom rule development
- Read `03-security-monitoring-threat-detection.md` §3.3–3.4
- Hands-on: clone an OOTB rule and modify the threshold; save as Draft
- Practice the custom-rule workflow on paper end-to-end
- **Quiz 03-C — OOTB & custom rules** (10 Q)

### Day 13 — Signal investigation & triage + Bits AI
- Read `03-security-monitoring-threat-detection.md` §3.5
- Open Security Signals Explorer and walk through any signal — Investigator graph, Bits AI tab
- Memorize Bits AI workflow: Investigating → Benign / Suspicious + side-panel actions
- **Quiz 03-D — Signal triage & Bits AI** (10 Q)

### Day 14 — Rule testing + threat hunting
- Read `03-security-monitoring-threat-detection.md` §3.6–3.7
- Hands-on: open the **MITRE ATT&CK Map** and identify gaps in your sandbox
- Run **Domain Drill #3** (20 questions covering Domain 3)

---

## Week 3 — SOAR, Content Packs, Compliance, Optimization

### Day 15 — Incident response design
- Read `04-incident-response-soar.md` §4.1
- Diagram the signal → notification → case → SOAR loop on paper
- **Quiz 04-A — IR workflow design** (10 Q)

### Day 16 — Security dashboards & metrics
- Read `04-incident-response-soar.md` §4.2
- Hands-on: pin a Cloud SIEM dashboard, then clone and add a Security Signals widget
- Add a Logs-to-Metrics metric for `failed_logins.count`
- **Quiz 04-B — Dashboards & metrics** (10 Q)

### Day 17 — Workflow Automation (SOAR)
- Read `04-incident-response-soar.md` §4.3
- Hands-on: open Workflow Automation, look at the template library, identify Security triggers
- Walk through one SOAR blueprint (e.g., *Block IP at AWS WAF*) on paper
- **Quiz 04-C — Workflow Automation / SOAR** (10 Q)

### Day 18 — Case Management & notifications
- Read `04-incident-response-soar.md` §4.4 + §4.6
- Hands-on: from a signal, click *Create Case*; explore status, priority, comments
- Memorize Notification Rules vs per-rule notifications
- **Quiz 04-D — Cases & notifications** (10 Q)

### Day 19 — Content Pack discovery, activation, configuration
- Read `05-content-packs.md` §5.1–5.3
- Hands-on: in Datadog, browse Content Packs and activate the AWS CloudTrail pack
- Verify parser, OOTB rules, dashboard, blueprint
- **Quiz 05-A — Content Packs** (10 Q)

### Day 20 — Operational use & troubleshooting
- Read `05-content-packs.md` §5.4–5.5
- Memorize the troubleshooting symptom→fix table
- Run **Domain Drill #4** (15 Q on D4) and **Domain Drill #5** (15 Q on D5)

### Day 21 — First **Full Mock Exam**
- Run **Full Mock Exam** (75 Q / 2h) under exam-like conditions:
  - No notes, single sitting, timer on, single attempt
- Score and identify your weakest 2 domains; log them in your weak-areas list

---

## Week 4 — Compliance, Optimization, Drills, Exam day

### Day 22 — Threat tracking & framework mapping
- Read `06-compliance-reporting.md` §6.1–6.2
- Watch: *Datadog Compliance Frameworks* video
- **Quiz 06-A — Threat tracking & frameworks** (10 Q)

### Day 23 — Data retention & RBAC
- Read `06-compliance-reporting.md` §6.3
- Hands-on: review the retention setting on each of your indexes and archives
- Memorize the PCI specifics (US1 site, PCI endpoint, Audit Trail required)
- **Quiz 06-B — Retention, PCI, RBAC** (10 Q)

### Day 24 — Audit reporting & security metrics
- Read `06-compliance-reporting.md` §6.4–6.5
- Hands-on: open Audit Trail, filter for rule-edit events
- Decide your top 5 SIEM SLO metrics
- **Quiz 06-C — Audit & metrics** (10 Q)

### Day 25 — OCSF normalization & threat intelligence
- Read `07-optimization-best-practices.md` §7.1–7.2
- Memorize: OCSF processor must be **last**; OEM threat intel persists 24h, BYOTI persists as configured
- Hands-on: upload a small Reference Table of test IOCs; add a Threat Intel Processor
- **Quiz 07-A — OCSF & threat intel** (10 Q)

### Day 26 — Automation, enablement, infrastructure optimization
- Read `07-optimization-best-practices.md` §7.3–7.5
- Build a list of cost-optimization levers from memory
- **Quiz 07-B — Optimization** (10 Q)

### Day 27 — **Adaptive weak-area drill** + **Full Mock Exam #2**
- Run the **Adaptive Weak-Area Drill** generator targeting your bottom 2 domains from Day 21
- Then run **Full Mock Exam #2** (75 Q / 2h)
- You should now be scoring ≥80%; if not, schedule one more review day before sitting the real exam

### Day 28 — Exam day prep
- Light review only — re-read your weak-areas notes
- 1× **Daily Mini-Quiz** to warm up
- Re-read the playbook field, MITRE map and Bits AI lifecycle (high-frequency exam topics)
- Sleep well. Take the exam.

---

## Tracking sheet (fill in as you go)

| Day | Date | Hours done | Score (Daily Mini) | Weak-areas added |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| ... | | | | |
| 28 | | | | |

(Replicate this in a notebook or spreadsheet — staying honest with the log is what closes gaps.)
