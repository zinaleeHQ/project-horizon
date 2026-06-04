# Output: Jira-Ready Epic & User Story Structure

**Generated from:** WSJF Scorecard Top 3 Epics
**Format:** Jira-compatible Epic → Story hierarchy
**Story Point Scale:** Fibonacci (1, 2, 3, 5, 8, 13)

---

## 🟦 EPIC 1: HL7 Data Mapping Upgrade — InterSystems HealthConnect

**Epic Goal:** Resolve active HL7 data integrity failures at Mercy Health and upgrade the HealthConnect translation layer to prevent recurrence across the partner network.

**Business Justification:** 30-day cure notice from Mercy Health. Contract revenue at risk. Root cause for duplicate patient records across multiple partner sites.

**Acceptance Criteria (Epic Level):**
- [ ] Mercy Health HL7 data drop rate reduced to 0% over a 72-hour post-deployment monitoring window
- [ ] Cure notice response delivered with documented resolution evidence
- [ ] Regression test suite confirms no schema changes introduced to live HL7 v2/v3 mappings
- [ ] All 200+ partner connection schemas validated against new translation layer
- [ ] Rollback procedure documented and tested in staging environment

---

### Story 1.1: HL7 Translation Layer Audit
**As an** EMIS Data Engineer
**I need** a complete audit of current HealthConnect translation mappings for Mercy Health’s HL7 feed
**So that** I can identify exactly which field mappings are causing data drops and duplicate record creation

**Story Points:** 3
**Acceptance Criteria:**
- [ ] Given access to Mercy Health’s HL7 feed logs, when I run the audit tool, then all mapping exceptions are exported to a structured report
- [ ] Given the audit report, the root-cause mapping failure(s) are identified with specific HL7 segment and field references
- [ ] Given the audit findings, a remediation plan is documented and reviewed by the Tech Lead before development begins

---

### Story 1.2: Translation Layer Patch — Mercy Health Feed
**As an** EMIS Data Engineer
**I need** to update the HealthConnect translation layer configuration for Mercy Health’s HL7 ADT and ORU message feeds
**So that** patient admission records are correctly mapped without creating duplicate entries in the enterprise system

**Story Points:** 5
**Acceptance Criteria:**
- [ ] Given the patched translation layer, when a Mercy Health HL7 ADT A01 message is processed, then no duplicate patient record is created
- [ ] Given the patch, when tested against a 500-message sample from Mercy Health’s production feed, then the data drop rate is 0%
- [ ] Given the patch deployment, no changes are introduced to the core HL7 v2.3 schema structure used by other partner connections

---

### Story 1.3: Integration Regression Test Suite
**As a** QA Engineer
**I need** to run a full regression test across all active partner HL7 connections against the updated translation layer
**So that** the Mercy Health fix does not introduce message processing failures at other partner sites

**Story Points:** 8
**Acceptance Criteria:**
- [ ] Given the updated translation layer, when tested against HL7 message samples from the top 20 partner connections, then all messages process without error
- [ ] Given the regression suite results, any new exceptions are documented with severity classification before go-live approval
- [ ] Given a clean regression run, a go-live approval sign-off is obtained from the Tech Lead and PM

---

## 🟧 EPIC 2: Clinician Workflow Optimization — Commure Manual Intervention Cases

**Epic Goal:** Redesign the manual charge entry workflow for non-autonomous Commure cases to reduce per-encounter time by ≥40% without adding more than 2 actions to the current physician charting baseline.

**Business Justification:** Clinician burnout at 12+ sites. Program renewal risk. Manual workflow variation driving billing error patterns that feed Epic 3.

**Acceptance Criteria (Epic Level):**
- [ ] Manual charge entry time reduced from 8+ minutes to ≤4.5 minutes per encounter at pilot sites
- [ ] Workflow redesign introduces no more than 2 additional clicks vs. current baseline (compliance with Clinician Friction Cap guardrail)
- [ ] Redesign functions identically on Epic and Cerner EMR environments (EMR neutrality requirement)
- [ ] Pilot UAT completed at 10 sites with ≥80% hospitalist satisfaction score
- [ ] Commure manual exception rate shows measurable reduction at pilot sites within 2 weeks of deployment

---

### Story 2.1: Current-State Workflow Mapping
**As a** Product Manager
**I need** a documented step-by-step map of the current manual charge entry workflow across 5 representative pilot sites
**So that** I can identify exactly where the 8+ minutes are being spent and which steps are candidates for elimination or simplification

**Story Points:** 3
**Acceptance Criteria:**
- [ ] Given interviews with 2 hospitalists per site archetype, then a current-state swimlane diagram is produced covering all steps from charge trigger to submission
- [ ] Given the workflow map, each step is tagged as Value-Added, Non-Value-Added, or Required Non-Value-Added (per Lean classification)
- [ ] Given the map, a friction inventory is produced identifying the top 5 time-consuming or error-prone steps

---

### Story 2.2: Redesigned Workflow UI — Manual Intervention Screen
**As a** hospitalist managing a non-autonomous Commure charge case
**I need** a simplified charge review and submission screen that surfaces the most likely correct modifier codes in a single view
**So that** I can review, confirm, and submit a charge in under 4 minutes without navigating between multiple system screens

**Story Points:** 8
**Acceptance Criteria:**
- [ ] Given a non-autonomous Commure case, when I open the charge review screen, then the top 3 AI-suggested modifier codes are displayed with confidence scores in a single view
- [ ] Given the redesigned screen, the complete review-confirm-submit workflow requires no more than current baseline + 2 clicks
- [ ] Given Epic and Cerner EMR environments, the redesigned screen renders identically with no EMR-specific code branching
- [ ] Given a usability test with 5 hospitalists, then ≥80% rate the new workflow as faster than the current process

---

### Story 2.3: Pilot Deployment & UAT — 10 Sites
**As a** Clinical Operations Field Lead
**I need** the redesigned workflow deployed to 10 pilot sites with a structured UAT period
**So that** I can confirm the improvement is real before committing to enterprise-wide rollout

**Story Points:** 5
**Acceptance Criteria:**
- [ ] Given the pilot deployment, 10 sites are live on the redesigned workflow by end of Sprint 2
- [ ] Given a 2-week UAT period, hospitalist satisfaction survey results are collected from ≥30 respondents
- [ ] Given UAT results, manual charge entry time measurements are collected and compared against pre-deployment baseline
- [ ] Given any UAT-identified critical defects, a hotfix process is documented and actioned within 48 hours

---

## 🟥 EPIC 3: Billing Reconciliation Dashboard — Commure Modifier Error Tracking

**Epic Goal:** Build a real-time reconciliation dashboard giving Revenue Cycle visibility into Commure modifier mismatch rates, stuck claims, and DSO trend — enabling proactive insurer management and data-driven vendor discussions.

**Business Justification:** $420K in claims currently stuck. DSO exposure growing weekly. CFO escalation path active. Dashboard enables both immediate triage and long-term pattern analysis.

**Acceptance Criteria (Epic Level):**
- [ ] Dashboard shows real-time count and value of claims in each status: Auto-Coded, Pending Review, Submitted, Rejected, Resubmitted
- [ ] Modifier mismatch rate is displayed as a trend line (rolling 30-day) with drill-down by CPT code category
- [ ] DSO impact is calculated and displayed at the portfolio level and by hospital partner
- [ ] Dashboard is accessible to Revenue Cycle team without engineering involvement for ongoing use
- [ ] Data refreshes at minimum every 4 hours from Commure and billing system feeds

---

### Story 3.1: Data Model & Feed Integration Design
**As a** Data Engineer
**I need** a documented data model connecting Commure charge output, the modifier review queue, and the billing system claims status feed
**So that** the dashboard has a reliable, well-structured data foundation that doesn’t require manual reconciliation

**Story Points:** 5
**Acceptance Criteria:**
- [ ] Given Commure’s existing API parameters, a data model is designed that maps charge output fields to billing system claim status fields
- [ ] Given the data model, a field-level mapping document is reviewed and approved by the Tech Lead
- [ ] Given the mapping, a test data feed is established in the staging environment with ≥30 days of historical data

---

### Story 3.2: Dashboard Build — Modifier Error Tracking View
**As a** Revenue Cycle Manager
**I need** a dashboard that shows me at a glance which modifier codes are generating the most rejections and how much revenue is at risk
**So that** I can prioritize my team’s manual review work and bring data to my next call with Commure

**Story Points:** 8
**Acceptance Criteria:**
- [ ] Given the live data feed, when I open the dashboard, then I see current claim counts by status updated within the last 4 hours
- [ ] Given the modifier error view, when I click on a modifier code, then I see a breakdown of rejection reasons and affected claim values
- [ ] Given the DSO trend view, when I select a date range, then I see DSO impact over that period by hospital partner
- [ ] Given the dashboard, a Revenue Cycle team member with no engineering background can navigate all views without training

---

### Story 3.3: UAT with Revenue Cycle & Go-Live
**As a** Revenue Cycle Director
**I need** to validate the dashboard against known claim data before it becomes the team’s official reporting tool
**So that** I’m confident the numbers are accurate before I present them to the CFO

**Story Points:** 5
**Acceptance Criteria:**
- [ ] Given 3 Revenue Cycle team members in a UAT session, when they validate dashboard figures against their manual reports, then ≥95% of values match within acceptable rounding tolerance
- [ ] Given UAT sign-off, the dashboard is promoted to production and communicated to the Revenue Cycle team
- [ ] Given go-live, a 30-day hypercare period is established with a named PM and engineering contact for issue escalation

---

*Output generated from WSJF scorecard · See [wsjf-scorecard.md](./wsjf-scorecard.md) and [sprint-roadmap.md](./sprint-roadmap.md)*
*User story format: Standard Agile · Acceptance criteria format: Given-When-Then (BDD)*