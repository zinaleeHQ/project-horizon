# ð· Project Horizon: The Enterprise Prioritization Blueprint

> **Scaling AI Revenue Integration & HL7 Interoperability in Multi-Site Healthcare Operations**

[![Methodology](https://img.shields.io/badge/Methodology-SAFe%20%7C%20WSJF-0052CC?style=flat-square)](https://scaledagileframework.com/wsjf/)
[![Domain](https://img.shields.io/badge/Domain-Healthcare%20IT%20%7C%20Revenue%20Cycle-teal?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Portfolio%20Simulation-orange?style=flat-square)]()

---

## ð The Operational Challenge

A fast-growing multi-site healthcare enterprise operates across 200+ hospital partner locations with 2,000+ distributed clinicians. The technology stack includes an enterprise HL7 interface engine managing HL7 v2/v3 data pipelines across hundreds of unique EMR integrations, and a recently deployed AI-assisted RCM platform handling revenue cycle coding for the majority of clinical encounters.

The system works â but only 85% of the time.

The product teamâs intake queue has become a battlefield. Twenty-plus competing requests are flooding in from clinical operations, revenue cycle, compliance, IT infrastructure, hospital partners, and executive leadership simultaneously. Every stakeholder believes their issue is Priority One.

The engineering team has a fixed 3-sprint horizon before the next PI Planning cycle. There are no additional resources. Every prioritization decision is also a deferral decision.

**This is the enterprise prioritization bottleneck.** Itâs precisely the scenario where human intuition â shaped by whoever spoke loudest in the last meeting â produces the worst possible outcomes.

This project documents an AI-assisted resolution of that bottleneck using a structured WSJF (Weighted Shortest Job First) scoring framework, enforced by a prompt-engineered evaluation engine designed to remove emotional and political bias from the backlog.

---

## ð¥ The Data Inputs

Three structured inputs feed the prioritization engine. See the `/data` folder for full source files.

### The Intake Queue
20 raw stakeholder requests representing the full spectrum of competing enterprise priorities â HL7 data integrity failures, RCM platform billing mismatches, compliance audit deadlines, clinical burnout complaints, executive visibility demands, and active vendor escalations. The queue is unranked, unfiltered, and deliberately messy â exactly as it would arrive in a real enterprise environment.

### The Constraint Matrix
A fixed set of resource and compliance guardrails that govern every prioritization decision:

| Constraint | Value |
|---|---|
| Team | 1 PM Â· 1 Scrum Master Â· 1 Tech Lead Â· 2 Data Engineers Â· 2 Full-Stack Devs Â· 2 QA |
| Sprint Velocity | 35â40 Story Points per 2-week sprint |
| Delivery Horizon | 3 Sprints (6 weeks) before next PI Planning |
| Data Security | HIPAA Â· SOC 2 Type II Â· Zero-Trust Â· No public API exposure |
| Interoperability Lock | No modifications to live HL7 v2/v3 schemas or HealthConnect core mappings |
| Vendor Boundary | RCM platform API data translation layer only â ML model tuning out of scope |
| Deployment Rule | FridayâSaturday maintenance window Â· 24-hour maximum downtime |
| Revenue Protection | Zero tolerance for DSO-impacting pipeline interruptions |
| Clinician Friction Cap | Maximum 2 additional workflow actions vs. current baseline |

### The Scoring Framework: WSJF
Weighted Shortest Job First â the SAFe standard for value-based prioritization:

> **WSJF Score = (User Business Value + Time Criticality + Risk Reduction) Ã· Job Size**

Each value dimension scored 1â10. Job Size scored on Fibonacci scale (1, 2, 3, 5, 8, 13).

---

## ð¤ The AI-Assisted Prioritization Engine

The prompt engineering block in `/prompts/wsjf-scoring-prompt.md` performs five operations:

1. **Context ingestion** â Processes all 20 intake items and the constraint matrix simultaneously
2. **Constraint filtering** â Removes items that violate hard compliance, vendor, or safety guardrails *before* scoring begins
3. **WSJF scoring** â Evaluates each item across all four dimensions with explicit reasoning per item
4. **Sequencing** â Arranges top-priority items into a 3-sprint delivery plan that maximizes value realization within capacity
5. **Output generation** â Produces the scored matrix and downstream Jira-ready Epic structure

The prompt is engineered to eliminate three common enterprise prioritization failure modes:
- **Recency bias** â latest request wins
- **Authority bias** â loudest executive wins
- **Effort anchoring** â easiest item wins

---

## ð The Output: Prioritized Roadmap

After applying the WSJF scoring engine against all 20 intake items within the constraint guardrails, three epics emerged as the sprint-ready priorities. Full scorecard in `/output/wsjf-scorecard.md`.

| Rank | Epic | WSJF Score | Sprint |
|---|---|---|---|
| **1** | HL7 Data Mapping Upgrade â HL7 interface engine | **6.33** | Sprint 1 |
| **2** | Clinician Workflow Optimization â RCM platform Manual Cases | **4.67** | Sprint 2 |
| **3** | Billing Reconciliation Dashboard â RCM platform Modifier Errors | **3.63** | Sprint 3 |

> **Why this sequence?** The HL7 mapping upgrade carries the highest time criticality due to a 30-day hospital partner contract deadline and the highest risk reduction value from preventing downstream cascading failures. Clinician workflow optimization leads Sprint 2 because it reduces the manual exception rate that the Sprint 3 billing dashboard will measure â optimizing before measuring prevents an immediately obsolete baseline. This sequencing decision overrode the raw WSJF ranking of Epics 2 and 3 based on a technical dependency not captured in the scoring matrix alone.

---

## ð Repository Contents

```
project-horizon/
âââ README.md                    â This document
âââ PROCESS.md                   â PM decision log and AI methodology
âââ /data/
â   âââ intake-queue.md          â 20 raw stakeholder requests (unranked)
â   âââ constraint-matrix.md    â Team capacity and compliance guardrails
âââ /prompts/
â   âââ wsjf-scoring-prompt.md  â The AI prioritization engine
âââ /output/
    âââ wsjf-scorecard.md        â Full WSJF scoring matrix (all 20 items)
    âââ sprint-roadmap.md        â 3-sprint delivery plan
    âââ jira-backlog.md          â Jira-ready Epics and User Stories
```

---

## â Project Manager Requirements

| Requirement | How This Project Demonstrates It |
|---|---|
| *âLead prioritization based on business impact and resource constraintsâ* | WSJF matrix with explicit constraint enforcement applied before scoring |
| *âUtilize data insights to support product prioritization decisionsâ* | AI-driven scoring replaces intuition with structured, reproducible evaluation |
| *âServe as primary liaison between business stakeholders, tech teams, and vendorsâ* | Intake queue includes direct clinical vs. IT vs. vendor conflicts resolved through the framework |
| *âDevelop and maintain product roadmaps aligned with organizational goalsâ* | 3-sprint roadmap with PI Planning horizon alignment and explicit dependency mapping |
| *âEnsure data quality and integrity across systemsâ* | HL7 integrity and RCM platform reconciliation epics address root-cause data quality failures |

---

## â Project Manager Methodology Intervention

WSJF, or any other methodology, is an input to the decision, not the decision itself. In this example, using these data points, the WSJF scores actually kept Epics 2 and 3 in the same order â Clinician Workflow came before the Billing Dashboard either way. The Billing Dashboard measures modifier error rates. But if you deploy that dashboard before you've fixed the workflow that's generating those errors, you're measuring a broken state you're already committed to changing; the baseline is already obsolete. I am documenting this explicitly because the more important skill to demonstrate is knowing when the framework gets you most of the way there, and knowing when a human intervention has to override it and say why.

---

## ð Portfolio Navigation

This is **Agent 1 of 3** in a connected PM portfolio. The three projects tell a single end-to-end story:

| Project | Question Answered | Methodology |
|---|---|---|
| **Project Horizon** (this repo) | What do we build and when? | SAFe Â· WSJF |
| [Project Clarity](https://github.com/zinaleeHQ/project-clarity) | How do we change how people work? | Lean Â· DMAIC |
| [Project Signal](https://github.com/zinaleeHQ/project-signal) | How do we keep every stakeholder aligned? | Stakeholder Intelligence Â· Audience Mapping |

[**â Back to Portfolio Overview**](https://github.com/zinaleeHQ)

---

*Portfolio simulation Â· All scenario details constructed from publicly available information Â· No proprietary data from any organization has been used Â· Built June 2026*
