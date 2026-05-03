# Datadog Cloud SIEM for AWS Fundamentals – Exam Overview

## Exam mechanics
- **Duration:** 120 minutes (max seat time)
- **Total questions:** 60 multiple choice (50 scored + 10 unscored pretest, not identified to candidate)
- **Question type:** Multiple choice with one correct answer (distractors are designed to look plausible)
- **Pass mark:** Not officially published, but historically Datadog Fundamentals exams target ~70%
- **Validity:** 3 years
- **Retakes:** Up to 3 per 180-day window
- **Delivery:** Proctored via Kryterion / online

## What is in scope
The exam validates knowledge across seven domains:

1. **Cloud SIEM Fundamentals for AWS** – log ingestion, architecture/workflow, AWS security foundations
2. **Datadog Setup** – log mgmt indexing strategy, AWS integration & log collection, log processing & normalization, Cloud SIEM configuration & verification
3. **Security Monitoring and Threat Detection** – detection rule components, OOTB rule mgmt, custom rule development, signal investigation/triage, rule testing/validation, threat hunting
4. **Incident Response, Dashboarding, and Automation (SOAR)** – workflow design, security dashboards & metrics, workflow automation, case management, signal triage with Bits AI
5. **Content Pack Implementation and Usage** – discovery & activation, configuration & setup, operational use, health monitoring & troubleshooting
6. **Compliance and Reporting** – threat tracking & risk reporting, framework mapping, data retention & security policies, audit reporting & evidence, security metrics
7. **Optimization and Best Practices** – OCSF normalization, threat intelligence, automation/operational improvement, team enablement, infrastructure mgmt

## What is OUT of scope
- General SIEM theory and history
- Generic security theory
- Datadog Infrastructure (INFRA) and APM (these are different exams)
- Cloud computing fundamentals (you are expected to already know AWS basics)

## Target candidate (per Datadog)
- Minimum **6 months** of hands-on Cloud SIEM for AWS in production
- Roles: Cloud Security Engineer, AppSec/NetSec, DevSecOps, SOC Analyst, Cloud Solutions Architect, SRE/Observability Engineer, Compliance Auditor with cloud focus, AWS Cloud Engineer

## Recommended study material (official)
- **Cloud Security Engineer – Cloud SIEM Learning Path** (11 courses)
- **Attacks & Threat Detection Learning Path**
- **Practice Exam** (10 questions on Datadog Learning Center)
- **Documentation** the exam guide explicitly highlights:
  - Datadog Cloud SIEM Overview
  - Datadog Security
  - Cloud SIEM Data Security
  - AWS Configuration Guide for Cloud SIEM
  - Getting Started with Cloud SIEM
  - What is OCSF and How Do You Implement It?
  - Bring Your Own Threat Intelligence
  - Amazon Kinesis Data Firehose
  - Send AWS Services Logs with the Datadog Amazon Data Firehose Destination

## Strategy tips
- Heaviest domains by exam-question volume are typically Detection Rules + Datadog Setup + SOAR. Allocate study hours proportionally.
- Memorize the **AWS log source → use case → detection** triplet (CloudTrail/GuardDuty/VPC Flow Logs/WAF/Route53 DNS/S3/ELB).
- Know the two main AWS log shipping methods: **Lambda Forwarder** vs **Amazon Data Firehose destination**, and when to choose each.
- Be fluent in the **OCSF schema** terms (event class, category, type) and the OCSF Processor location in the pipeline.
- Be able to identify each **OOTB detection rule type**: log detection (threshold, new value, anomaly, impossible travel), signal correlation, third-party.
- Know the difference between **Standard Indexes**, **Flex Logs**, and **Log Archives** for retention/cost.
