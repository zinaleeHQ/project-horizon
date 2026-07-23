# Project Horizon: The Enterprise Prioritization Blueprint

**An AI scoring engine ranked a 20-item backlog and put a documentation update in first place — ahead of a compliance deadline and a live billing error. Here's why that happened, and what I did about it.**

That's the short version of what this project demonstrates: not that AI can prioritize a backlog (it can), but that someone still has to know when the ranking is wrong, and why, before it becomes a sprint plan.

The scenario: a multi-site healthcare enterprise, 200+ hospital partners, an HL7 interface engine and an AI-assisted billing platform both running at roughly 85% reliability, and a 20-item intake queue where every stakeholder believes their item is Priority One. Three sprints on the clock before the next PI Planning cycle. No extra headcount.

I ran a WSJF (Weighted Shortest Job First) scoring engine against the full queue, then had to catch three places where the math and the reality didn't match.

[→ See the rest of the judgment calls in PROCESS.md](https://github.com) — same kind of thinking, more of it, including the two calls I got wrong before I got them right.

[![Methodology](https://img.shields.io/badge/Methodology-SAFe%20%7C%20WSJF-0052CC?style=flat-square)](https://scaledagileframework.com/wsjf/)
[![Domain](https://img.shields.io/badge/Domain-Healthcare%20IT%20%7C%20Revenue%20Cycle-teal?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Portfolio%20Simulation-orange?style=flat-square)]()

---

## ⬡ Why This Matters Outside of Tech

Every organization eventually runs into the same wall: too many things to do, not enough time or people to do them, and a room full of stakeholders each convinced their thing matters most. A hospital system calls that an intake queue. A law firm calls it docket triage. A case management agency calls it a caseload. Different vocabulary, same math problem.

What's underneath this project is a discipline built specifically for that problem — the same prioritization logic that large engineering organizations formalized once "just go with your gut" stopped scaling. It has a name (WSJF, if you're curious, nested inside a broader framework called SAFe), but the name matters less than what it does: it forces every competing priority to answer the same question — *what does it cost us every day we don't do this?* — instead of rewarding whoever argued loudest in the last meeting.

That's the part that travels. The healthcare scenario below is specific, but the underlying skill isn't industry-locked. It's the same judgment a law firm partner uses deciding which case gets attention this week, or a case manager uses deciding which client gets the next available appointment slot. I just documented mine with receipts.

---

## ⌗ What the Engine Got Wrong (And Why That's the Point)

I built a WSJF scoring prompt, fed it 20 real-shaped intake items — HL7 data failures, billing mismatches, compliance deadlines, executive demands, clinician complaints — and let it run against a fixed constraint matrix (team capacity, HIPAA, deployment windows, the usual). Full inputs are in `/data` if you want to see the mess before it got sorted.

Three things happened that a clean demo never would have shown me:

1. A **documentation item scored #1**, WSJF 10.0 — ahead of a compliance item and a live billing error — because it had a Job Size of 1 and WSJF has no way to tell "small" from "trivial." A PM who ships that ranking without reading it has let a spreadsheet make a decision it wasn't built to make.
2. The **highest-priority item by contract deadline ranked #8** in the live run, not #1, because a Job Size of 8 dragged its score down. The override held regardless — a hospital partner cure notice doesn't get negotiated against a story-point estimate — but overriding a framework that's *wrong* is a stronger proof of judgment than confirming one that happens to agree with you.
3. And the engine **flagged a connection I hadn't built into the data**: two intake items I'd scoped as separate epics were probably the same root cause wearing two names. That one wasn't a correction on my part. That was the AI surfacing something worth checking, and me confirming it was right.

None of this makes WSJF a bad framework. It makes it a framework that needs a person reading the output, which is a different claim — and the one this whole project is actually making.

---

## ☍ The Sequencing Call

Raw scoring ranked a billing dashboard epic above a clinician workflow epic. I reversed that for sprint sequencing, because the dashboard measures the exact manual-intervention rate the workflow fix is designed to reduce. 

Build the dashboard first, and the baseline it establishes is obsolete the moment the workflow fix ships in the following sprint — every future improvement gets measured against a number that already includes waste that's about to disappear.

| Rank | Epic | WSJF Score | Sprint |
| :---: | :--- | :---: | :---: |
| **1** | HL7 Data Mapping Upgrade | 6.33* | **Sprint 1** |
| **2** | Clinician Workflow Optimization | 4.67 | **Sprint 2** |
| **3** | Billing Reconciliation Dashboard | 3.63 | **Sprint 3** |

_\*Mock-data score shown. Live run scored this item 3.50 and ranked it #8 — see PROCESS.md for what that divergence meant._

---

## ⚙&#xFE0E; How the Engine Actually Works

The prompt (`/prompts/wsjf-scoring-prompt.md`) does four things in sequence: ingests all 20 items plus the constraint matrix, filters out anything that violates a hard compliance or vendor guardrail *before* scoring starts, scores what's left across the WSJF dimensions with reasoning attached to every number, then sequences the results into a 3-sprint plan. Output comes back as Jira-ready epics, not just a ranked list — the full scorecard is in `/output/wsjf-scorecard.md`.

The constraint matrix lives as its own document rather than buried in the prompt text, mainly because that changes how the AI treats it. Constraints written into prose get weighed like everything else. Constraints structured as a matrix get treated as gates — filtering decisions out before scoring, not softening them during scoring. Worth knowing if you're building something similar.

| Constraint | Value |
| :--- | :--- |
| **Team** | 1 PM · 1 Scrum Master · 1 Tech Lead · 2 Data Engineers · 2 Full-Stack Devs · 2 QA |
| **Sprint Velocity** | 35–40 Story Points per 2-week sprint |
| **Delivery Horizon** | 3 Sprints (6 weeks) before next PI Planning |
| **Data Security** | HIPAA · SOC 2 Type II · Zero-Trust · No public API exposure |
| **Interoperability Lock** | No modifications to live HL7 v2/v3 schemas or HealthConnect core mappings |
| **Vendor Boundary** | RCM platform API data translation layer only — ML model tuning out of scope |
| **Deployment Rule** | Friday–Saturday maintenance window · 24-hour maximum downtime |
| **Revenue Protection** | Zero tolerance for DSO-impacting pipeline interruptions |
| **Clinician Friction Cap** | Maximum 2 additional workflow actions vs. current baseline |

---

## ⌬ Repository Contents

```text
project-horizon/
├── README.md               ← This document
├── PROCESS.md              ← PM decision log and AI methodology
├── /data/
│   ├── intake-queue.md     ← 20 raw stakeholder requests (unranked)
│   └── constraint-matrix.md ← Team capacity and compliance guardrails
├── /prompts/
│   └── wsjf-scoring-prompt.md ← The AI prioritization engine
└── /output/
    ├── wsjf-scorecard.md   ← Full WSJF scoring matrix (all 20 items)
    ├── sprint-roadmap.md   ← 3-sprint delivery plan
    └── jira-backlog.md     ← Jira-ready Epics and User Stories
```

---

## ⧉ Try the Prompt Yourself

There's a live page with a one-click **Copy Prompt** button — grabs the full prompt plus data, ready to paste into Claude, GPT-4, or Gemini.

👉 [Open Project Horizon Prompt Copy page](https://github.io)

It pauses at a judgment checkpoint before the final phase. That pause isn't a bug — it's where the PM is supposed to be reading, not clicking through.

---

## ↳ Portfolio Navigation

This is **Prompt 1 of 4** in a connected PM portfolio. The three projects tell a single end-to-end story:

| Project | Question Answered | Methodology |
| :--- | :--- | :--- |
| ↳ **Project Horizon** _(this repo)_ | What do we build and when? | SAFe · WSJF |
| ↳ [Project Clarity](https://github.com) | How do we change how people work? | Lean · DMAIC |
| ↳ [Project Signal](https://github.com) | How do we keep every stakeholder aligned? | Stakeholder Intelligence · Audience Mapping |
| ↳ [Project Vista](https://github.com) | How do we give every stakeholder self-service visibility? | KPI Governance · Data Architecture |

[**← Back to Portfolio Overview**](https://github.com)

---
_*Portfolio case study · Built from publicly available information · No proprietary data used · June 2026*_
