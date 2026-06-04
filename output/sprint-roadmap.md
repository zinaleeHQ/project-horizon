# Output: 3-Sprint Delivery Roadmap

**PI Horizon:** 3 Sprints · 6 Weeks · Pre-PI Planning
**Team Velocity:** 35–40 Story Points per sprint
**Sequencing Logic:** WSJF-ranked, dependency-adjusted, capacity-constrained

---

## Sprint 1 — “Stabilize the Foundation”
**Capacity:** 38 SP · Dates: Weeks 1–2
**Sprint Goal:** Resolve the active hospital partner contract risk and close the open compliance gaps before any feature development begins.

| Epic / Item | Work Package | SP | Owner Role | Dependency |
|---|---|---|---|---|
| **Epic 1** — HL7 Mapping Upgrade | Translation layer update for Mercy Health HL7 feeds | 13 | Data Engineer (x2) | None |
| **Epic 1** — HL7 Mapping Upgrade | Integration regression testing across 200+ partner schemas | 8 | QA Engineer (x2) | Above |
| Item 13 — HIPAA Audit Log Gap | Audit log coverage patch for HealthConnect–EMR pipeline | 5 | Data Engineer | Epic 1 mapping work |
| Item 05C — SOC 2 Re-Certification | AWS VPC partition policy update + endpoint re-certification | 5 | Tech Lead + Data Eng | None |
| Item 09 — Commure API Compatibility | Review Commure v2.4.1 parameter documentation + compatibility matrix | 3 | Data Engineer | None |
| Item 19 — Staged Rollout Plan | Define partner cohort staging for HL7 cutover — document in runbook | 2 | PM + Scrum Master | Epic 1 |
| **Sprint Buffer** | QA overflow + risk mitigation | 2 | QA | — |

**Total: 38 SP**

**Sprint 1 Definition of Done:**
- [ ] Mercy Health HL7 data drops resolved and verified — cure notice response delivered
- [ ] HIPAA audit log gaps closed and documented for Q3 audit
- [ ] SOC 2 endpoint re-certification complete
- [ ] Commure v2.4.1 compatibility assessment delivered to vendor
- [ ] Staged rollout runbook approved by Operations Director

---

## Sprint 2 — “Optimize the Workflow”
**Capacity:** 38 SP · Dates: Weeks 3–4
**Sprint Goal:** Reduce clinician manual intervention time and begin addressing the active revenue exposure in the billing pipeline.

| Epic / Item | Work Package | SP | Owner Role | Dependency |
|---|---|---|---|---|
| **Epic 2** — Clinician Workflow Optimization | Current-state charting flow audit + friction mapping | 3 | PM + QA | Sprint 1 complete |
| **Epic 2** — Clinician Workflow Optimization | Redesign manual charge entry UI (2-click cap compliance) | 8 | Full-Stack Dev (x2) | Audit above |
| **Epic 2** — Clinician Workflow Optimization | Pilot deployment to 10 sites + UAT | 5 | QA + Field Ops | UI above |
| Item 06+16 — Claims Reconciliation Triage | Emergency reconciliation of 340 stuck claims ($420K) | 8 | Data Engineer + Finance | Commure API cert (Sprint 1) |
| Item 03 — HealthConnect Schema Upgrade | Begin partner cohort onboarding upgrade (Cohort A: 50 sites) | 8 | Data Engineer (x2) | Epic 1 |
| Item 07 — St. Francis Investigation | Root cause analysis on schema mismatches + partner update | 3 | Data Engineer | Epic 1 |
| **Sprint Buffer** | Rollback contingency | 3 | QA | — |

**Total: 38 SP**

**Sprint 2 Definition of Done:**
- [ ] Clinician workflow redesign piloted at 10 sites — manual entry time reduced ≥ 40%
- [ ] 340 stuck claims reconciled and resubmitted — DSO exposure report delivered to Finance
- [ ] HealthConnect Cohort A upgrade complete (50 sites)
- [ ] St. Francis mismatch root cause identified and partner briefed

---

## Sprint 3 — “Build Visibility”
**Capacity:** 37 SP · Dates: Weeks 5–6
**Sprint Goal:** Deliver the billing reconciliation dashboard, complete the HealthConnect upgrade cohort, and close the remaining operational gaps.

| Epic / Item | Work Package | SP | Owner Role | Dependency |
|---|---|---|---|---|
| **Epic 3** — Billing Reconciliation Dashboard | Data model design: Commure + billing feeds | 5 | Tech Lead + Data Eng | Epic 2 workflow stable |
| **Epic 3** — Billing Reconciliation Dashboard | Dashboard build: modifier error tracking, DSO trend, rejection rate | 8 | Full-Stack Dev (x2) | Above |
| **Epic 3** — Billing Reconciliation Dashboard | QA, UAT with Revenue Cycle team, and go-live | 5 | QA + Revenue Cycle | Above |
| Item 17 — New Partner Onboarding Unblock | HealthConnect Cohort B upgrade (remaining 150 sites) | 8 | Data Engineer (x2) | Epic 1 + Cohort A |
| Item 12 — Orphaned Charge Reconciliation | Batch reconciliation process for rural site timeout charges | 5 | Data Engineer | Commure API cert |
| Item 18 — Onboarding Documentation | Update hospitalist onboarding guide — Commure manual workflow SOP | 2 | PM | Epic 2 complete |
| **Sprint Buffer** | QA overflow | 4 | QA | — |

**Total: 37 SP**

**Sprint 3 Definition of Done:**
- [ ] Billing reconciliation dashboard live and adopted by Revenue Cycle team
- [ ] All 200+ partner sites upgraded to new HealthConnect schema mappings
- [ ] Orphaned charge records reconciled — zero backlog entering PI Planning
- [ ] Updated hospitalist onboarding documentation published

---

## Deferred to Post-PI Backlog

| Item | Reason for Deferral | Recommended PI Action |
|---|---|---|
| Item 08 — JCAHO Dashboard | JS:13, data dependency on stable Commure+EMR pipeline (available post-Sprint 3) | Schedule for PI+1 Sprint 1 |
| Item 11 — Board AI ROI Dashboard | JS:8, lower WSJF, no hard deadline in this horizon | Refine requirements in PI Planning |
| Item 10 — Automated UAT Harness | High value for next PI but not Sprint-Ready without capacity allocation | Bring to PI Planning as a PI Objective |
| Item 15 — Performance Analytics | Blocked by data pipeline stability (available Sprint 3 close) | Unblocked at PI+1 start |
| Item 20 — CEO Technology Story | Requires Discovery sprint to define scope and output | Schedule Discovery in PI+1 |

---

*Output generated from WSJF scorecard · See [wsjf-scorecard.md](./wsjf-scorecard.md) for full scoring rationale.*
*See [PROCESS.md](../PROCESS.md) for the sequencing decision on Epics 2 and 3.*