# PROCESS.md — How I Built This Project

*By Zina Lee, Enterprise Product Manager*

---

## Why I Built This

I’ve spent years as a PM in environments where AI tools were restricted or unavailable — including federal contracting work where the security posture simply didn’t permit it. That means my AI fluency has lived entirely in theory: frameworks, certifications, strategic understanding — but no portfolio artifacts to show for it.

This project exists to close that gap honestly: by building something grounded in a real company’s actual technology challenges, using AI as a transparent and accountable tool, and documenting every decision along the way.

---

## The Strategic Decisions I Made

### Why WSJF Over a Value vs. Effort Matrix

I considered both seriously. A Value vs. Effort matrix is more visually intuitive and immediately readable by non-technical stakeholders. But for this specific scenario — a SAFe enterprise team with a defined PI Planning horizon — WSJF was the correct choice for three reasons:

**1. Time Criticality is the dominant variable here.** One item in the intake queue (the HL7 mapping upgrade) has a hard 30-day hospital partner contract deadline. A standard Value/Effort matrix treats urgency as a narrative modifier rather than a scored dimension — which would systematically underweight the most time-sensitive item in the queue.

**2. WSJF is the SAFe organizational standard.** Using it demonstrates methodology alignment with how enterprise Release Trains actually operate at scale. Choosing a simpler framework would have been easier to explain — but harder to defend in a panel interview with a technical audience.

**3. Cost of Delay framing produces better thinking.** The WSJF question — “what does it cost us every day we don’t do this?” — produces materially different answers than “how valuable is this?”. The first question captures urgency, risk, and compounding impact. The second captures only static value.

### Why I Built the Constraint Matrix as a Separate Document

Most people using AI for analysis embed constraints directly in the prompt narrative: *“remember we only have three developers...”* I didn’t — and the distinction matters.

When constraints live in prose, the AI treats them as soft context. When they’re structured as a matrix with explicit guardrails, the AI treats them as logic gates that filter decisions before scoring begins. The difference in output quality is significant and reproducible.

Separating the constraint matrix also makes it **reusable and version-controllable**. If the team’s velocity changes next quarter, I update one document. If a compliance requirement changes, I update one document. The prompt doesn’t need to change at all.

### Why I Overrode the Raw WSJF Ranking for Sprint Sequencing

The raw WSJF score ranked Epic 3 (Billing Reconciliation Dashboard) above Epic 2 (Clinician Workflow Optimization). I overrode this for sequencing purposes based on a technical dependency the scoring matrix doesn’t capture:

The billing dashboard measures the rate of manual Commure interventions. If we build the dashboard before optimizing the workflow that drives those interventions, we establish a baseline metric that is immediately obsolete the moment Sprint 2 delivers. Measuring before optimizing wastes an entire sprint of baseline data.

This is the kind of judgment call that a scoring engine surfaces but cannot make. The AI produced the ranking. I made the sequencing decision. That distinction is the whole point of this portfolio.

---

## How I Directed the AI

### What the AI Did
- Drafted the 20-item intake queue based on scenario parameters and known OBHG technology architecture
- Applied WSJF scoring criteria consistently across all items once dimensions were defined
- Generated the formatted output documents (scorecard tables, Jira story structure, roadmap layout)
- Provided a research synthesis of OBHG’s publicly available technology partnerships and infrastructure

### What I Decided
- The choice of WSJF as the scoring framework
- The specific guardrail values in the constraint matrix
- The dimensional weights and scoring rationale for each intake item
- The sprint sequencing override for Epics 2 and 3
- The framing and narrative of every public-facing document
- The determination that this scenario is realistic and defensible based on public information

### The Prompt Engineering Approach

The WSJF scoring prompt (`/prompts/wsjf-scoring-prompt.md`) is structured in three layers: context ingestion, audience variable logic, and output enforcement. The key design decisions were:

- **Separating constraint filtering from scoring** so that hard guardrails (HIPAA, vendor boundaries, deployment rules) eliminate ineligible items *before* WSJF math begins
- **Requiring explicit reasoning per item** rather than just a score, to surface the logic behind each number and make it auditable
- **Enforcing structured output format** (JSON-compatible markdown tables) so the output feeds directly into downstream Jira tooling without manual reformatting
- **Including a sequencing instruction** that asks the AI to flag dependency relationships it identifies, which I then validated against my own technical judgment

---

## What I’d Do Differently With Real Data

**1. Stakeholder interviews before scoring.** The WSJF dimension values I assigned were estimated from scenario logic. Real User Business Value and Time Criticality scores require structured stakeholder input — typically a 30-minute facilitated scoring session with domain leads from each affected team.

**2. Velocity calibration from sprint history.** I used 35–40 story points as team velocity. A real team’s velocity is drawn from their actual sprint history — typically a 6-sprint rolling average that accounts for holidays, onboarding, and technical debt cycles.

**3. Full dependency mapping before sequencing.** The output documents note the Epic 2/3 dependency, but a real implementation would include a complete dependency map across all 20 items before any sequencing decisions are made. Non-obvious dependencies (a compliance item that blocks a data pipeline item, for example) frequently change the optimal sequence in ways that raw WSJF scoring cannot detect.

---

*This document reflects my actual decision-making process in building this project. It is intended to give reviewers — technical and non-technical — an honest view of where the PM thinking ends and the AI tooling begins.*

*[Back to README](./README.md)*