# Datadog Cloud SIEM for AWS Fundamentals — Certification Prep Package

Everything you need to pass the **Datadog Cloud SIEM for AWS Fundamentals** certification, organized for a **2-hour-per-day, 28-day** study plan.

> Source of truth for exam objectives is the official Datadog exam guide PDF (uploaded as `datadog-cloud-siem-for-aws-fundamentals-exam-guide.pdf`). This package was built directly from that guide and from the official Datadog documentation it references.

---

## Folder map

```
foundation/
├── README.md                         ← you are here
│
├── knowledge-base/                   ← RAG / your study source of truth
│   ├── README.md
│   ├── 00-exam-overview.md
│   ├── 01-cloud-siem-fundamentals.md
│   ├── 02-datadog-setup.md
│   ├── 03-security-monitoring-threat-detection.md
│   ├── 04-incident-response-soar.md
│   ├── 05-content-packs.md
│   ├── 06-compliance-reporting.md
│   └── 07-optimization-best-practices.md
│
├── study-plan/                       ← 2h-per-day, 28-day plan
│   ├── study-plan.md
│   └── daily-checklist.md
│
├── quizzes/                          ← reinforcement (220 Q)
│   ├── README.md
│   ├── 00-exam-mechanics-quiz.md
│   ├── 01-cloud-siem-fundamentals-quiz.md
│   ├── 02-datadog-setup-quiz.md
│   ├── 03-detection-quiz.md
│   ├── 04-soar-quiz.md
│   ├── 05-content-packs-quiz.md
│   ├── 06-compliance-quiz.md
│   └── 07-optimization-quiz.md
│
└── simulations/                      ← all four simulation formats
    ├── README.md
    ├── adaptive-drills/
    │   ├── adaptive-protocol.md
    │   └── adaptive-bank.md (100 Q tagged by domain & sub-topic)
    ├── domain-drills/
    │   ├── README.md
    │   ├── drill-01-fundamentals.md (15 Q)
    │   ├── drill-02-datadog-setup.md (20 Q)
    │   ├── drill-03-detection.md (20 Q)
    │   ├── drill-04-soar.md (15 Q)
    │   ├── drill-05-content-packs.md (15 Q)
    │   ├── drill-06-compliance.md (15 Q)
    │   └── drill-07-optimization.md (15 Q)
    ├── mock-exam/
    │   ├── README.md
    │   ├── mock-exam-1.md (75 Q / 2h)
    │   └── mock-exam-2.md (75 Q / 2h)
    └── daily-mini/
        ├── README.md
        └── daily-mini-bank.md (70 Q)
```

## Total practice volume

| Source | Questions |
|---|---|
| Reinforcement quizzes | 220 |
| Daily-mini bank | 70 |
| Adaptive bank | 100 |
| Domain drills | 115 |
| Mock #1 + Mock #2 | 150 |
| **Total** | **655** |

## How to use this package

**Today, do this in order:**
1. Open `study-plan/study-plan.md` and read the whole plan once.
2. Open `knowledge-base/00-exam-overview.md` and `knowledge-base/README.md` — get oriented.
3. Tomorrow morning, start **Day 1** of the study plan. Use `study-plan/daily-checklist.md` as a printable template you fill in each day.
4. End of every day: take that day's **Daily Mini-Quiz** (random 10 from `simulations/daily-mini/daily-mini-bank.md`).
5. End of every week: run the matching **Domain Drill** in `simulations/domain-drills/`.
6. **Day 21:** sit `simulations/mock-exam/mock-exam-1.md` under exam-like conditions.
7. **Day 27:** run an **Adaptive Weak-Area Drill** (per `simulations/adaptive-drills/adaptive-protocol.md`) then sit `mock-exam-2.md`.
8. **Day 28:** light review — re-read your weak-areas log.
9. **Exam day:** sit the real exam.

## Knowledge-base file → exam domain mapping

| KB file | Exam domain |
|---|---|
| `01-cloud-siem-fundamentals.md` | Cloud SIEM Fundamentals for AWS |
| `02-datadog-setup.md` | Datadog Setup |
| `03-security-monitoring-threat-detection.md` | Security Monitoring and Threat Detection |
| `04-incident-response-soar.md` | Incident Response, Dashboarding, and Automation (SOAR) |
| `05-content-packs.md` | Content Pack Implementation and Usage |
| `06-compliance-reporting.md` | Compliance and Reporting |
| `07-optimization-best-practices.md` | Optimization and Best Practices |

## Source documentation

The knowledge base is derived directly from the documentation pages the exam guide explicitly references, accessed via Context7's `/datadog/documentation` library:

- Datadog Cloud SIEM Overview
- Datadog Security
- Cloud SIEM Data Security
- AWS Configuration Guide for Cloud SIEM
- Getting Started with Cloud SIEM
- What is OCSF and How Do You Implement It?
- Bring Your Own Threat Intelligence
- Amazon Kinesis Data Firehose
- Send AWS Services Logs with the Datadog Amazon Data Firehose Destination

Plus supporting Datadog documentation areas covering detection rules, MITRE ATT&CK Map, Cloud SIEM Investigator, Bits AI Security Analyst, Workflow Automation, Case Management, Sensitive Data Scanner, Audit Trail, Flex Logs, log pipelines/processors, Reference Tables, and the OCSF processor.
