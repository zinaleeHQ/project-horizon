# Data: Constraint Matrix & Guardrails

**Purpose:** Defines the non-negotiable boundaries within which all prioritization decisions must operate. These guardrails are loaded into the WSJF scoring prompt as structured input — not as narrative context, but as logic gates that filter decisions before scoring begins.

---

### 1. Resource & Team Capacity

| Parameter | Value |
|---|---|
| **Structure** | 1 PM, 1 Scrum Master, 1 Tech Lead, 2 Data/Integration Engineers, 2 Full-Stack Developers, 2 QA Engineers |
| **Cadence** | 2-week Sprints inside a larger SAFe enterprise Release Train |
| **Velocity** | Stable at 35–40 Story Points per sprint |
| **Horizon** | 3-Sprint delivery window (6 weeks total) prior to next PI Planning cycle |

---

### 2. Technical & Compliance Boundaries

| Guardrail | Rule |
|---|---|
| **Data Security** | Strict Zero-Trust architecture. 100% HIPAA and SOC 2 Type II compliant. No public API data leaks. All deployment within secure AWS cloud partition. |
| **Core Interoperability** | No modifications permitted to core HL7 v2/v3 schemas or the HL7 interface engine data engine mappings live across 200+ partner locations. |
| **Vendor Constraints** | Integration must operate strictly within existing third-party AI platform parameters (RCM platform APIs). Core ML model tuning is out of scope for this team. |

---

### 3. Operational & Business Guardrails

| Guardrail | Rule |
|---|---|
| **The 24-Hour Deployment Rule** | All production cutovers must execute within a Friday–Saturday maintenance window. Maximum allowable system downtime is 24 hours. |
| **Revenue Preservation** | Zero tolerance for pipeline interruptions that increase Days Sales Outstanding (DSO) or delay claim filings. Any code triggering DSO impact will be automatically rolled back. |
| **Clinician Friction Cap** | Frontend UI modifications cannot add more than two actions/clicks to the physician charting workflow across distributed sites. |

---

### 4. Pre-Scoring Filter Rules

Before WSJF scoring begins, the following item types are automatically flagged and removed from the active scoring pool:

| Filter Rule | Rationale |
|---|---|
| Items with no defined acceptance criteria | Cannot be scored for Job Size without measurable completion state |
| Items requiring core ML model changes (RCM platform) | Outside vendor contract scope — not actionable within this horizon |
| Items with hard dependency on incomplete upstream work | Blocked items are scored separately after their blockers are resolved |
| Items requiring schema changes to live HL7 mappings | Violates core interoperability guardrail |
| Duplicate submissions | Consolidated into primary item before scoring |

---

*This document is a structured data input for the WSJF scoring prompt. It is not narrative context — it is a logic gate applied before evaluation begins.*

*See [wsjf-scoring-prompt.md](../prompts/wsjf-scoring-prompt.md) for how these constraints are applied in the prompt architecture.*