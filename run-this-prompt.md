# Try This Yourself — Project Horizon WSJF Prioritization Engine

---

# Prompt: AI-Assisted WSJF Prioritization Engine

**Version:** 1.0
**Framework:** Weighted Shortest Job First (WSJF) — SAFe Standard
**Author:** Zina Lee, Product Manager
*Copy everything below this line and paste it into Claude (claude.ai) or any AI assistant. Hit send and wait for the output. The AI will guide you through the next step when it’s done.*

---

## Prompt Architecture Overview

This prompt is structured in three layers that execute sequentially:

1. **Layer 1 — Context Ingestion:** Load and parse all input data
2. **Layer 2 — Constraint Filtering:** Remove ineligible items before scoring
3. **Layer 3 — WSJF Scoring & Output Generation:** Score, rank, sequence, and format

Each layer has explicit guardrails and output requirements. Do not skip or compress layers.

---

## 🟡 SYSTEM CONTEXT

You are an enterprise product prioritization assistant supporting a Product Manager at a multi-site healthcare operations organization. You have been given:

- A raw stakeholder intake queue of 20 unranked items
- A constraint matrix defining team capacity, technical boundaries, and compliance guardrails
- A WSJF scoring framework with defined dimensions and Fibonacci job sizing

Your role is to function as a structured analytical engine — not a decision-maker. You will apply the framework to the data and surface the most defensible prioritization order. The PM retains final authority over all sequencing decisions.

**Critical behavioral rules:**
- Do not introduce information not present in the provided data
- Do not allow item urgency claims (CRITICAL/HIGH/MEDIUM/LOW as labeled by requestors) to override WSJF scoring — these labels reflect political pressure, not business value
- Do not score items that fail constraint filtering — flag them separately with the reason
- Require explicit reasoning for every score assigned — never output a number without a sentence explaining it
- Use exact Fibonacci values only: 1, 2, 3, 5, 8, 13, 20

---

## 📥 INPUT DATA SOURCES

**INPUT_A: Stakeholder Intake Queue**
```
[See DATA FILE 1 below]
```

**INPUT_B: Constraint Matrix**
```
[See DATA FILE 2 below]
```

---

## 🔴 LAYER 1: CONTEXT INGESTION

### Intake Queue Parsing
For each item in INPUT_A, extract and confirm:
- Item ID
- Requestor and business unit
- Problem statement
- Stated urgency (label only — do not use as a scoring input)
- Any explicit time constraints or deadlines mentioned

### Constraint Matrix Parsing
From INPUT_B, extract:
- Team velocity ceiling (story points per sprint)
- Sprint boundary rules (what cannot be split across sprints)
- Hard compliance requirements (items that must be included regardless of score)
- Technical dependency constraints (items that must precede other items)
- Vendor boundary rules (what cannot be modified)

### Layer 1 Output Requirements
Output a structured summary confirming:
1. Total items ingested from INPUT_A
2. Active constraints extracted from INPUT_B
3. Any items with explicit deadlines or compliance flags — list these separately as they will require override consideration in Layer 3

---

## 🟠 LAYER 2: CONSTRAINT FILTERING

### Filtering Instructions
Before scoring begins, filter INPUT_A against INPUT_B constraints:

**Filter 1 — Vendor Boundary Filter**
Remove any item that requires modification to core AI model logic or parameters. These items are out of scope. Flag them with reason: "Vendor boundary — requires core model modification."

**Filter 2 — Compliance Mandate Filter**
Identify items that are non-negotiable compliance requirements. These items will be scored but flagged for mandatory inclusion regardless of rank.

**Filter 3 — Dependency Filter**
Identify items that cannot be executed until another item is complete. Flag these with their dependency. They may still be scored but cannot be sequenced before their dependency.

**Filter 4 — Capacity Filter**
Do not remove items based on capacity — capacity constraints affect sequencing, not eligibility. Note items whose job size exceeds single-sprint capacity for sequencing purposes.

### Layer 2 Output Requirements
Output:
1. Items removed by Vendor Boundary Filter (with reason)
2. Items flagged as Compliance Mandates
3. Items flagged with Dependencies
4. Remaining eligible items confirmed for scoring

---

## 🟢 LAYER 3: WSJF SCORING & OUTPUT GENERATION

### WSJF Scoring Framework

**Formula:** WSJF = (UBV + TC + RR + OE) ÷ JS

**Dimension Definitions:**

| Dimension | Abbreviation | Definition |
|---|---|---|
| User/Business Value | UBV | Direct value delivered to end users or the business if this item is completed |
| Time Criticality | TC | How much does the cost of delay increase over time? Is there a deadline, a window, or a compounding penalty? |
| Risk Reduction / Opportunity Enablement | RR | Does completing this item reduce a significant risk OR unlock future high-value work? |
| Opportunity Enablement | OE | Already included in RR dimension above — do not score separately |
| Job Size | JS | Relative effort required to complete this item (denominator — larger = lower priority) |

**Scoring Scale:** Fibonacci only: 1, 2, 3, 5, 8, 13, 20

**Scoring Rules:**
- Score each eligible item on UBV, TC, RR, and JS independently
- Provide one sentence of reasoning for each score
- Calculate WSJF = (UBV + TC + RR) ÷ JS
- Round to two decimal places

### Output Format Requirements

**Output 1: WSJF Scorecard**
Produce a markdown table with columns: Item ID | Title | UBV | TC | RR | JS | WSJF Score | Reasoning Summary

**Output 2: Ranked Priority List**
Rank all eligible items by WSJF score (highest to lowest). For items with identical scores, apply tiebreaker: higher TC wins.

**Output 3: Constraint Override Flags**
List any items that should be repositioned despite their WSJF rank due to:
- Compliance mandates (must include regardless of rank)
- Hard deadlines (TC=10 items that cannot wait for their ranked position)
- Dependency chains (item B cannot start until item A is complete)

**Output 4: Sprint Sequence Recommendation**
Using the constraint matrix velocity ceiling, sequence the top-ranked items into sprint slots. Apply the following rules:
- Do not exceed velocity ceiling per sprint
- Do not split items across sprints unless explicitly flagged as splittable
- Compliance mandates and hard-deadline items take sprint slots before WSJF-ranked items if required
- Flag any sprint where sequencing required overriding the WSJF rank

**Output 5: Jira-Ready Epic Summaries**
For the top 5 items by final sequence position, produce a Jira-ready summary:
- Epic Title
- One-sentence description
- Acceptance criteria (3 bullet points)
- Story point estimate (from JS score)
- Sprint assignment

---

## DATA FILE 1: Intake Queue

# Data: Stakeholder Intake Queue

**Sprint Planning Cycle:** PI 3 · Sprint 1
**Intake Period:** 4 weeks
**Total Items Submitted:** 20
**Status:** Unranked — pending WSJF scoring

> Urgency labels (CRITICAL / HIGH / MEDIUM / LOW) reflect requestor self-assessment only. They are **not** scoring inputs. WSJF scoring replaces these labels with a defensible, structured ranking.

---

## Intake Queue

| ID | Title | Requestor | Business Unit | Problem Statement | Stated Urgency | Notes |
|---|---|---|---|---|---|---|
| 01 | HL7 Inbound Mapping — Diagnosis Code Gap | VP Engineering | Engineering | Inbound HL7 ADT feeds from 3 partner hospitals are dropping diagnosis codes on transfer. Affects downstream charge capture accuracy. | CRITICAL | Partner Health System A has issued a formal cure notice. 30-day resolution window. |
| 02 | OB Modifier Code Mismatch Rate | Revenue Cycle Director | Finance | Manual cases are generating a 22% modifier mismatch rate. Each mismatch requires a billing correction that adds 4–6 days to claim resolution. | HIGH | Mismatch rate increased after last platform update. Root cause unconfirmed. |
| 03 | HL7 Outbound Feed — Charge Data Delay | VP Engineering | Engineering | Outbound HL7 charge data to billing system has an intermittent 6–12 hour delay. Affects same-day claim submission rate. | HIGH | Intermittent — affects approximately 15% of transmissions. |
| 04 | Clinician Workflow Optimization — Manual Case UX | VP Clinical Operations | Clinical Ops | Manual case entry takes 8.4 minutes average. Clinician satisfaction score is 2.9/5. Two sites have escalated formally. | HIGH | Satisfaction score below 3.0 triggers executive review per SLA. |
| 05 | SOC 2 Type II Re-Certification | CISO | Compliance | SOC 2 certification expires in 45 days. Re-certification requires documented evidence of 14 controls across 4 domains. | CRITICAL | External auditor engagement confirmed. Evidence collection window: 3 weeks. |
| 06 | Billing Reconciliation Dashboard | CFO | Finance | Finance lacks real-time visibility into claim submission status, modifier error rates, and DSO trajectory. Currently reconciling manually in spreadsheets. | HIGH | CFO has requested a dashboard by end of PI. |
| 07 | HL7 Batch Processing — Retry Logic | VP Engineering | Engineering | Failed HL7 batch transactions are not retrying automatically. Manual intervention required. Affects 8% of nightly batches. | MEDIUM | Related to Items 01 and 03. |
| 08 | Clinician Onboarding — RCM Platform Module | VP Clinical Operations | Clinical Ops | New hire onboarding for RCM platform takes 3 weeks. No standardized training path. High variation in time-to-competency. | MEDIUM | 40 new hires expected next quarter. |
| 09 | API Compatibility — v2.4.1 Parameter Change | VP Engineering | Engineering | RCM platform v2.4.1 introduced an undocumented parameter change affecting the modifier mapping field. Integration layer is calling deprecated field name. | HIGH | Silent failure — no error thrown. Discovered during manual audit. |
| 10 | Partner Site Expansion — 3 New Hospitals | Chief Growth Officer | Business Dev | 3 new partner hospitals are ready to onboard. Each requires HL7 feed configuration, RCM platform provisioning, and billing system integration. | HIGH | Revenue impact: estimated $2.1M ARR. Contracts signed. |
| 11 | Clinician Satisfaction Survey — Automated Cadence | VP Clinical Operations | Clinical Ops | Satisfaction surveys are sent manually and inconsistently. Response rate is 14%. Automating cadence and delivery is estimated to improve response rate to 40%+. | LOW | |
| 12 | RCM Platform — Autonomous Case Rate Improvement | VP Engineering | Engineering | Autonomous case rate is 85%. Industry benchmark is 90%+. Each percentage point improvement reduces manual case volume by approximately 200 cases/month. | MEDIUM | Requires model retraining — vendor approval needed. |
| 13 | HIPAA Audit Log Gap | CISO | Compliance | Current audit log does not capture modifier code selection events. HIPAA requires a complete audit trail for all billable decision points. | CRITICAL | Gap identified during internal audit. External audit scheduled in 60 days. |
| 14 | Zero-Trust Network Segmentation | CISO | Compliance | Current network architecture does not meet zero-trust standards required for SOC 2 renewal. Segmentation work is a dependency for Item 05. | CRITICAL | Dependency: must complete before SOC 2 evidence collection closes. |
| 15 | Reporting API — External Partner Access | Chief Growth Officer | Business Dev | Partner hospitals are requesting API access to their own performance data. Currently requires a manual report pull from the PM team. | MEDIUM | |
| 16 | HL7 Mapping — Upgrade to v2.8 Standard | VP Engineering | Engineering | Current HL7 implementation is v2.5.1. Partner Health System A contract renewal requires v2.8 compliance. Cure notice issued. | CRITICAL | Same cure notice as Item 01. 30-day window. |
| 17 | Mobile Optimization — RCM Platform Field View | VP Clinical Operations | Clinical Ops | Rural site clinicians report the RCM platform mobile view is unusable on 3G connections. Affects 23% of active users. | MEDIUM | |
| 18 | Internal Documentation — Integration Architecture | VP Engineering | Engineering | Current integration architecture is undocumented. New engineers take 6–8 weeks to onboard. Documentation sprint estimated at 5 story points. | LOW | |
| 19 | HL7 — Outbound ADT Notification Delay | VP Engineering | Engineering | Outbound ADT notifications to partner hospitals are delayed 2–4 hours. Affects partner hospital care coordination workflows. | MEDIUM | Related to Item 03. |
| 20 | RCM Platform Dashboard — Executive Summary View | CFO | Finance | Executives want a high-level summary view in the RCM platform showing autonomous case rate, manual case rate, and DSO. | LOW | Related to Item 06. |

---

## DATA FILE 2: Constraint Matrix

# Data: Constraint Matrix

**Purpose:** Defines the non-negotiable boundaries within which the WSJF scoring engine must operate. These are loaded into the prompt as structured inputs — not narrative context — so the AI treats them as hard logic gates rather than soft preferences.

---

## Active Constraints

| # | Constraint | Rule | Rationale |
|---|---|---|---|
| 1 | **Velocity Ceiling** | Team capacity is 35–40 story points per 2-week sprint. Do not sequence more than 38 SP into any single sprint. | Based on team’s 6-sprint rolling average velocity. Accounts for sprint ceremonies, onboarding, and unplanned work buffer. |
| 2 | **HIPAA Compliance Mandate** | Any item flagged as a HIPAA compliance gap must be included in Sprint 1 regardless of WSJF rank. | Regulatory non-negotiable. External audit in 60 days creates a hard deadline that overrides prioritization scoring. |
| 3 | **SOC 2 Evidence Window** | SOC 2 re-certification evidence collection closes in 3 weeks. Any item required for SOC 2 compliance must be sequenced before that window closes. | External auditor engagement is confirmed. Missing the evidence window means failing re-certification. |
| 4 | **Cure Notice Deadline** | Partner Health System A has issued a formal cure notice with a 30-day resolution window. Any item tagged to this cure notice must be resolved within Sprint 1–2. | Contract breach risk. Cure notice resolution is a revenue protection requirement, not a feature request. |
| 5 | **Vendor Model Boundary** | No item may require modification to the RCM platform’s core AI coding logic, model weights, or training data. Only the data translation and display layer is in scope. | Vendor contract restriction. Core model changes require a formal change order with a 90-day minimum lead time. |
| 6 | **No Cross-Sprint Splitting** | Items may not be split across sprint boundaries unless explicitly marked as “splittable” in the intake queue. | Incomplete items at sprint close create carry-over debt that distorts velocity measurement and PI reporting. |
| 7 | **Single-Team Scope** | All items must be executable by a single cross-functional team of 6 (2 data engineers, 2 full-stack devs, 1 QA engineer, 1 tech lead). Items requiring a separate team or external resource are flagged for dependency management. | PI planning is scoped to this team only. External dependencies must be escalated outside this exercise. |
| 8 | **Deployment Window** | Production deployments may only occur during the Friday maintenance window (11 PM – 2 AM). Items requiring mid-sprint deployment are flagged for deployment risk review. | Change management policy. Unscheduled deployments require a P1 exception approved by VP Engineering. |


---

## COMPLETION INSTRUCTION

When you have completed all three layers of output above, stop and say exactly this:

"I've completed the WSJF analysis. Would you like me to now evaluate the output — identifying where a PM should override the scores, what relationships between items the scoring doesn't capture, and what you'd do next if this were a real sprint planning session?"

Wait for the user to respond before continuing.
