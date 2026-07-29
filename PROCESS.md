# The PROCESS — How I Built This Project

*Zina Lee, Product Manager*

---

## ⬡&#xFE0E; Why I Built This

I've spent years as a PM in environments where AI tools were restricted or unavailable — including federal contracting work where the security posture simply didn't permit it. That means my AI fluency has lived entirely in theory: frameworks, certifications, strategic understanding — but no portfolio artifacts to show for it. 

This project exists to close that gap honestly: by building something grounded in a real company's actual technology challenges, using AI as a transparent and accountable tool, and documenting every decision along the way.

---

## ∴&#xFE0E; The Strategic Decisions I Made

### ⌗&#xFE0E; Why WSJF Over a Value vs. Effort Matrix

I considered both seriously. A Value vs. Effort matrix is more visually intuitive and immediately readable by non-technical stakeholders. But for this specific scenario (a SAFe enterprise team with a defined PI Planning horizon), WSJF was the correct choice for three reasons:

* ›&#xFE0E; **Time Criticality is the dominant variable here.** One item in the intake queue (the HL7 mapping upgrade) has a hard 30-day hospital partner contract deadline. A standard Value/Effort matrix treats urgency as a narrative modifier rather than a scored dimension, which would systematically underweight the most time-sensitive item in the queue.
* ›&#xFE0E; **WSJF is the SAFe organizational standard.** Using it demonstrates methodology alignment with how enterprise Release Trains actually operate at scale. Choosing a simpler framework would have been easier to explain, but harder to defend in a panel interview with a technical audience.
* ›&#xFE0E; **Cost of Delay framing produces better thinking.** The WSJF question ("what does it cost us every day we don't do this?") produces materially different answers than "how valuable is this?". The first question captures urgency, risk, and compounding impact. The second captures only static value.

### ⎔&#xFE0E; Why I Built the Constraint Matrix as a Separate Document

Most people using AI for analysis embed constraints directly in the prompt narrative: *"Remember we only have three developers..."* I didn't, and that distinction matters. 

When constraints live in prose, the AI treats them as soft context. When they're structured as a matrix with explicit guardrails, the AI treats them as logic gates that filter decisions before scoring begins. The difference in output quality is significant and reproducible, across different AI platforms.

Separating the constraint matrix also makes it **reusable and version-controllable**:
* ›&#xFE0E; If the team's velocity changes next quarter, I update one document.
* ›&#xFE0E; If a compliance requirement changes, I update one document.
* ›&#xFE0E; The prompt doesn't need to change at all.

### ☍&#xFE0E; Why I Overrode the Raw WSJF Ranking for Sprint Sequencing

The raw WSJF score ranked Epic 3 (Billing Reconciliation Dashboard) above Epic 2 (Clinician Workflow Optimization). I overrode this for sequencing purposes based on a technical dependency the scoring matrix doesn't capture:

The billing dashboard measures the rate of manual RCM platform interventions. If we build the dashboard before optimizing the workflow that drives those interventions, we establish a baseline metric that is immediately obsolete the moment Sprint 2 delivers. Measuring before optimizing wastes an entire sprint of baseline data.

This is the kind of judgment call that a scoring engine surfaces but cannot make. The AI produced the ranking. I made the sequencing decision. That distinction is the whole point of this portfolio.

---

### ⬡&#xFE0E; A Note on SAFe Terminology

The work items in this project are called "Epics" in the Jira sense — large, team-deliverable items scoped within a single PI. In strict SAFe, these would be Features: PI-bound items that feed into Portfolio-level Epics, WSJF’d at the Program Backlog level rather than the Portfolio level. 

SAFe Epics can cross PI boundaries; the items here cannot and do not. The distinction is deliberate — this project applies WSJF at the Feature/team level, which is the more common real-world application for a single-team context.

---

## ⚙&#xFE0E; How I Directed the AI

### ⌸&#xFE0E; What the AI Did
* ↳&#xFE0E; Drafted the 20-item intake queue based on scenario parameters and known technology architecture
* ↳&#xFE0E; Applied WSJF scoring criteria consistently across all items once dimensions were defined
* ↳&#xFE0E; Generated the formatted output documents (scorecard tables, Jira story structure, roadmap layout)
* ↳&#xFE0E; Provided a research synthesis of publicly available technology partnerships and infrastructure

### ∴&#xFE0E; What I Decided
* ↳&#xFE0E; The choice of WSJF as the scoring framework
* ↳&#xFE0E; The specific guardrail values in the constraint matrix
* ↳&#xFE0E; The dimensional weights and scoring rationale for each intake item
* ↳&#xFE0E; The sprint sequencing override for Epics 2 and 3
* ↳&#xFE0E; The framing and narrative of every public-facing document
* ↳&#xFE0E; The determination that this scenario is realistic and defensible based on public information

### ⎔&#xFE0E; The Prompt Engineering Approach

The WSJF scoring prompt `/prompts/wsjf-scoring-prompt.md` is structured in three layers: context ingestion, audience variable logic, and output enforcement. The key design decisions were:

* ›&#xFE0E; **Separating constraint filtering from scoring** so that hard guardrails (HIPAA, vendor boundaries, deployment rules) eliminate ineligible items *before* WSJF math begins
* ›&#xFE0E; **Requiring explicit reasoning per item** rather than just a score, to surface the logic behind each number and make it auditable
* ›&#xFE0E; **Enforcing structured output format** (JSON-compatible markdown tables) so the output feeds directly into downstream Jira tooling without manual reformatting
* ›&#xFE0E; **Including a sequencing instruction** that asks the AI to flag dependency relationships it identifies, which I then validated against my own technical judgment

---

## ⟹&#xFE0E; What Happened When I Actually Ran It

The output files in this repository reflect mock data, designed to produce a clean and readable demonstration. When I ran the prompt live against the actual data files, the results diverged from the mock in three instructive ways.

### ⟹&#xFE0E; The HL7 Mapping Upgrade ranked #8, not #1

In the mock, the HL7 upgrade lands near the top of the priority list, which is consistent with its TC=10 cure notice. In the live run, it ranked #8 (WSJF 3.50), penalized by a Job Size of 8. Large, necessary work scores poorly under WSJF because the framework is structurally biased toward small, high-value items.

The PM override stands regardless: a hard external contract deadline is not a dimension to be weighed against job size. HL7 is Sprint 1. But the override is a stronger demonstration of judgment when it's correcting a low score than when it's confirming a high one. Validating the obvious is not the same as knowing when the framework is wrong.

### ⟹&#xFE0E; Items 02 and 09 are probably the same root cause

The intake queue treats Item 02 (elevated OB modifier mismatch rate) and Item 09 (v2.4.1 API compatibility) as separate work items. The live run flagged them as likely the same root cause: the undocumented parameter change in v2.4.1 is the probable source of the mismatch rate increase.

Treating them as two epics means fixing the symptom in one sprint and the cause in another — with the mismatch rate potentially rising again in between. A real implementation would consolidate these as a single root-cause epic before sprint planning begins. This relationship was not documented in the data files; the prompt surfaced it from context.

### ⟹&#xFE0E; Item 18 (documentation) scored #1 at WSJF 10.0

Documentation updates carry a Job Size of 1. That makes them mathematically guaranteed to score near the top of any WSJF ranking, regardless of actual priority. Item 18 outscored every clinical, financial, and compliance item in the queue — not because documentation is the most important work, but because the framework has no way to distinguish between "small" and "trivial."

A PM who schedules a documentation sprint over a compliance item or a billing error fix has let the framework make a decision it is not qualified to make. Background tasks with JS=1 are a known WSJF artifact. Catching them is not optional.

---

These three divergences are a more useful artifact than a clean mock would have been. The prompt works, but "works" means it produces a ranking that requires PM judgment to interpret, not a ranking that can be executed without review.

---

## ↳&#xFE0E; What I'd Do Differently With Real Data

* ›&#xFE0E; **1. Stakeholder interviews before scoring.** The WSJF dimension values I assigned were estimated from scenario logic. Real User Business Value and Time Criticality scores require structured stakeholder input — typically a 30-minute facilitated scoring session with domain leads from each affected team.
* ›&#xFE0E; **2. Velocity calibration from sprint history.** I used 35–40 story points as team velocity. A real team's velocity is drawn from their actual sprint history — typically a 6-sprint rolling average that accounts for holidays, onboarding, and technical debt cycles.
* ›&#xFE0E; **3. Full dependency mapping before sequencing.** The output documents note the Epic 2/3 dependency, but a real implementation would include a complete dependency map across all 20 items before any sequencing decisions are made. Non-obvious dependencies (a compliance item that blocks a data pipeline item, for example) frequently change the optimal sequence in ways that raw WSJF scoring cannot detect.

---

*This document reflects my actual decision-making process in building this project. It is intended to give reviewers (technical and non-technical) an honest view of where the PM thinking ends and the AI tooling begins — and vice-versa.*

*[Back to README](./README.md)*
