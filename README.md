# Project Horizon: The Enterprise Prioritization Blueprint

> **What this project is about, in plain language:**
>
> This project tackles a problem every technology team faces: too many things to work on and no defensible way to decide what goes first. An AI tool scored and ranked a backlog of 20 competing work items. The PM's job is to catch the three places where the scores were wrong — and know why they were wrong — before those scores become a sprint plan that ships work in the wrong order and measures results against a broken baseline.


> **Scaling AI Revenue Integration & HL7 Interoperability in Multi-Site Healthcare Operations**

[![Methodology](https://img.shields.io/badge/Methodology-SAFe%20%7C%20WSJF-0052CC?style=flat-square)](https://scaledagileframework.com/wsjf/)
[![Domain](https://img.shields.io/badge/Domain-Healthcare%20IT%20%7C%20Revenue%20Cycle-teal?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Portfolio%20Simulation-orange?style=flat-square)]()

---

## 📋 The Operational Challenge

A fast-growing multi-site healthcare enterprise operates across 200+ hospital partner locations with 2,000+ distributed clinicians. The technology stack includes an enterprise HL7 interface engine managing HL7 v2/v3 data pipelines across hundreds of unique EMR integrations, and a recently deployed AI-assisted RCM platform handling revenue cycle coding for the majority of clinical encounters.

The system works — but only 85% of the time.

The product team’s intake queue has become a battlefield. Twenty-plus competing requests are flooding in from clinical operations, revenue cycle, compliance, IT infrastructure, hospital partners, and executive leadership simultaneously. Every stakeholder believes their issue is Priority One.

The engineering team has a fixed 3-sprint horizon before the next PI Planning cycle. There are no additional resources. Every prioritization decision is also a deferral decision.

**This is the enterprise prioritization bottleneck.** It’s precisely the scenario where human intuition — shaped by whoever spoke loudest in the last meeting — produces the worst possible outcomes.

This project documents an AI-assisted resolution of that bottleneck using a structured WSJF (Weighted Shortest Job First) scoring framework, enforced by a prompt-engineered evaluation engine designed to remove emotional and political bias from the backlog.

---

## 📥 The Data Inputs

Three structured inputs feed the prioritization engine. See the `/data` folder for full source files.

### The Intake Queue
20 raw stakeholder requests representing the full spectrum of competing enterprise priorities — HL7 data integrity failures, RCM platform billing mismatches, compliance audit deadlines, clinical burnout complaints, executive visibility demands, and active vendor exportations. The queue is unranked, unfiltered, and deliberately messy — exactly as it would arrive in a real enterprise environment.

### The Constraint Matrix
A fixed set of resource and compliance guardrails that govern every prioritization decision:

| Constraint | Value |
|---|---|
| Team | 1 PM · 1 Scrum Master · 1 Tech Lead · 2 Data Engineers · 2 Full-Stack Devs · 2 QA |
| Sprint Velocity | 35–40 Story Points per 2-week sprint |
| Delivery Horizon | 3 Sprints (6 weeks) before next PI Ptanning |
| Data Security | HIPAA · SOC 2 Type II · Zero-Trust · No public API exposure |
| Interoperability Lock | No modifications to live HL7 v2/v3 schemas or HealthConnect core mappings |
| Vendor Boundary | RCM ylatform API data translation layer only — ML model tuning out of scope |
| Deployment Rule | Friday–Saturday maintenance window · 24-hour maximum downtime |
| Revenue Protection | Zero tolerance for DSO-impacting pipeline interruptions |
| Clinician Friction Cap | Maximum 2 additional workflow actions vs. current baseline |


### The Scoring Framework: WSJF
Weighted Shortest Job First — the SAFe standard for value-based prioritization:

> **WSJF Score = (User Business Value + Time Criticality + Risk Reduction) ÷ Job Size**

Each value dimension scored 1-10. Job Size scored on Fibonacci scale (1, 2, 3, 5, 8, 13).

---

## 🤖 The AI-Assisted Prioritization Engine

The prompt engineering block in `/prompts/wsjf-scoring-prompt.md` performs five operations:

1. **Context ingestion** — Processes all 20 intake items and the constraint matrix simultaneously
2. **Constraint filtering** — Removes items that violate hard compliance, vendor, or safety guardrails *before* scoring begins
3. **WSJF scoring** — Evaluates each item across all four dimensions with explicit reasoning per item
4. **Sequencing** — Arranges top-priority items into a 3-sprint delivery plan that maximizes value realization within capacity
5. **Output generation** — Produces the scored matrix and downstream Jira-ready Epic structure

The prompt is engineered to eliminate three common enterprise prioritization failure modes:
- **Recency bias** — latest request wins
- **Authority bias** — loudest executive wins
- **Effort anchoring** — easiest item wins

---

## 📊 The Output: Prioritized Roadmap

After applying the WSJF scoring engine against all 20 intake items within the constraint guardrails, three epics emerged as the sprint-ready priorities. Full scorecard in `/output/wsjf-scorecard.md`.

| Rank | Epic | WSJF Score | Sprint |
|---|---|---|---|
| **1** | HL7 Data Mapping Upgrade — HL7 interface engine | **6.33** | Sprint 1 |
| **2** | Clinician Workflow Optimization — RCM platform Manual Cases | **4.67** | Sprint 2 |
| **3** | Billing Reconciliation Dashboard — RCM platform Modifier Errors | **3.63** | Sprint 3 |

> **Why this sequence?** The HL7 mapping upgrade carries the highest time criticality due to a 30-day hospital partner contract deadline and the highest risk reduction value from preventing downstream cascading failures. Clinician workflow optimization leads Sprint 2 because it reduces the manual exception rate that the Sprint 3 billing dashboard will measure — optimizing before measuring prevents an immediately obsolete baseline. This sequencing decision overrode the raw WSJF ranking of Epics 2 and 3 based on a technical dependency not captured in the scoring matrix alone.

---

## 📁 Repository Contents

```
project-horizon/
├── README.md                    ← This document
├── PROCESS.md                   ← PM decision log and AI methodology
├── /data/
│   ├── intake-queue.md          ← 20 raw stakeholder requests (unranked)
│   └── constraint-matrix.md    ← Team capacity and compliance guardrails
├── /prompts/
│   └── wsjf-scoring-prompt.md  ← The AI prioritization engine
└── /output/
    ├── wsjf-scorecard.md        ← Full WSJF scoring matrix (all 20 items)
    ├── sprint-roadmap.md        ← 3-sprint delivery plan
    └── jira-backlog.md          ← Jira-ready Epics and User Stories
```

---

## ✅ Project Manager Requirements

| Requirement | How This Project Demonstrates It |
|---|---|
| *“Lead prioritization based on business impact and resource constraints”* | WSJF matrix with explicit constraint enforcement applied before scoring |
| *“Utilize data insights to support product prioritization decisions”* | AI-driven scoring replaces intuition with structured, reproducible evaluation |
| *“Serve as primary liaison between business stakeholders, tech teams, and vendors”* | Intake queue includes direct clinical vs. IT vs. vendor conflicts resolved through the framework |
| *“Develop and maintain product roadmaps aligned with organizational goals”* | 3-sprint roadmap with PI Planning horizon alignment and explicit dependency mapping |
| *“Ensure data quality and integrity across systems”* | HL7 integrity and RCM platform reconciliation epics address root-cause data quality failures |

---

## ✅ Project Manager Methodology Intervention

With the mock data used here, the WSJF framework scores produce the right sequence of work — but the PM must catch the dependencies the scores alone don't document. If the billing dashboard is deployed before the clinician workflow is fixed, it measures a broken baseline: every future improvement gets compared to a number that includes waste you've already committed to removing. WSJF, or any other methodology, is an input to the decision, not the decision itself.


---


## 🚀 Want to Try This Yourself?

This project has a live HTML page with a one-click **Copy Prompt** button that copies the complete prompt for you, including data. Paste/Ctrl-V into Claude, GPT-4, or Gemini — no setup required.

👉 [Open Project Horizon Prompt Copy page](https://zinaleeHQ.github.io/project-horizon/)

Each prompt pauses at a PM judgment checkpoint before the final phase. Answer "yes" when you are ready to move forward. (That pause is the point.)


---


## 🔗 Portfolio Navigation

This is **Agent 1 of 4** in a connected PM portfolio. The three projects tell a single end-to-end story:

| Project | Question Answered | Methodology |
|---|---|---|
| **Project Horizon** (this repo) | What do we build and when? | SAFe · WSJF |
| [Project Clarity](https://github.com/zinaleeHQ/project-clarity) | How do we change how people work? | Lean · DMAIC |
| [Project Signal](https://github.com/zinaleeHQ/project-signal) | How do we keep every stakeholder aligned? | Stakeholder Intelligence · Audience Mapping |
| [Project Vista](https://github.com/zinaleeHQ/project-vista) | How do we give every stakeholder self-service visibility? | KPI Governance · Data Architecture |

[**← Back to Portfolio Overview**](https://github.com/zinaleeHQ)

---

*Portfolio case studies · All scenario details constructed from publicly available information · No proprietary data from any organization has been used · Built June 2026*
