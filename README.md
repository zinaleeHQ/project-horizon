# The PROCESS — How I Built This Project

*Zina Lee, Product Manager*

---

## ⬡&#xFE0E; Why I Built This

Most of my PM career has happened in environments where AI tools were restricted — mostly federal contracting work where the security posture didn't leave room for experimentation. My AI fluency existed almost entirely on paper: frameworks, certifications, a decent theoretical grounding. Nothing I could point to and say, "here's what I did with it."

I built this project to close that gap — to solve a real company's real technology problems, using AI the way I'd expect a competent PM to use it: a tool I stay accountable for, not a black box I defer to. And I wrote down every decision as I made it. If you read one page of this portfolio, [read the README first](./README.md). This PROCESS page is where you get more of the same thinking, applied further.

---

## ⌗&#xFE0E; The Strategic Decisions I Made

### ◆&#xFE0E; Why WSJF Over a Value vs. Effort Matrix

I went back and forth on this one. A Value/Effort matrix is more intuitive, easier to explain to a room full of non-technical stakeholders, and honestly would have made for a cleaner-looking deliverable.

But the scenario is a SAFe enterprise team working inside a fixed PI Planning horizon, and that changes the math. One item in the queue — the HL7 mapping upgrade — carries a hard 30-day contract deadline. A Value/Effort matrix treats urgency as something you mention in the room, not something you score. WSJF asks a sharper question instead: *what does it cost us every day we don't do this?* That's a different question than "how valuable is this," and it produces different, better answers. There's also a plain credibility reason: WSJF is the SAFe standard, and a simpler framework would have been easier to explain but harder to defend to a technical panel asking why I skipped it.

### ◆&#xFE0E; Why the Constraint Matrix Lives in Its Own Document

Most people embed constraints directly in the prompt narrative — something like *"remember, we only have three developers..."* I didn't, and the distinction matters more than it looks like it should. Constraints written into prose get treated as soft context, something the AI weighs in with everything else. Constraints structured as a matrix get treated as gates — they filter out ineligible items before scoring starts, not during it. I tested this across a couple of AI platforms and it held up consistently.

It's also just more reusable. If team velocity shifts next quarter, I update one document. The scoring prompt itself never has to change.

<!-- Left this section almost entirely alone — the dashes here were doing real work and the section reads clean. Dropped "Also," as its own sentence opener since it was a little throat-clearing; "It's also just more reusable" gets there faster. -->

### ◆&#xFE0E; A Note on SAFe Terminology

I'm using "Epics" here in the Jira sense — large, team-deliverable items scoped within a single PI. In strict SAFe usage, these would technically be Features, since SAFe Epics can span multiple PIs and nothing here does. I applied WSJF at the Feature/team level deliberately, because that's the more common real-world context for a single-team implementation, and I'd rather be precise about that than let a technical reviewer catch it first.

---

## ⚙&#xFE0E; How I Directed the AI

The AI drafted the 20-item intake queue from scenario parameters and known technology architecture, applied WSJF scoring consistently once I'd defined the dimensions, and generated the formatted outputs: scorecard tables, Jira story structure, the roadmap layout. It also pulled together a research synthesis of publicly available technology partnerships and infrastructure patterns.

I made the calls that actually carry risk if they're wrong: choosing WSJF in the first place, setting the guardrail values in the constraint matrix, weighting each dimension and writing the scoring rationale, overriding the sprint sequencing where the framework missed a dependency, and shaping the narrative of every document a reviewer would actually read. I also made the call that this scenario is realistic and defensible against public information — a judgment call in its own right, given that it's a portfolio piece and not a client engagement.

The scoring prompt (`/prompts/wsjf-scoring-prompt.md`) runs in three layers: context ingestion, audience variable logic, output enforcement. Constraint filtering happens before scoring, not during it. I required explicit reasoning per item rather than a bare number, so I could audit the logic later without reconstructing it from memory. Output had to come back as JSON-compatible markdown tables, since anything else meant manual reformatting before it could feed into Jira. And I built in a sequencing instruction asking the AI to flag any dependencies it noticed on its own — which I then checked against my own read of the technical picture.

---

## ⌬&#xFE0E; More of What the Live Run Surfaced

The README covers the documentation-item artifact and the contract-deadline override — the two clearest examples of catching the framework being wrong. Two more patterns showed up in the same live run, worth documenting because they're a different kind of catch.

◆&#xFE0E;**Items 02 and 09 were probably one root cause wearing two names.** The intake queue treats elevated OB modifier mismatches and a v2.4.1 API compatibility issue as separate work. The live run flagged them as connected: an undocumented parameter change in v2.4.1 is the likely source of the mismatch increase. This wasn't in the source data; the prompt pulled it from context. Left as two epics, you fix the symptom in one sprint and the cause in another, with the mismatch rate free to climb again in between. A real implementation would consolidate these before sprint planning starts.

◆&#xFE0E;**The sequencing override needed a dependency the scoring matrix couldn't see.** Raw WSJF ranked the billing dashboard epic above the clinician workflow epic — a known WSJF artifact, and not a subtle one. I reversed the sequence because the dashboard measures the exact manual-intervention rate the workflow fix is designed to reduce. Build the dashboard first, and the baseline goes stale the moment the workflow fix ships. A scoring engine can hand you a ranking. It can't tell you that measuring before fixing wastes a sprint of data. That catch is on the PM, every time.

---

## ↳&#xFE0E; What I'd Do Differently With Real Data

The dimension values here were estimated from scenario logic, which is fine for a portfolio piece and not fine for a real backlog. Real User Business Value and Time Criticality numbers need structured input from actual domain leads — typically a 30-minute facilitated scoring session per affected team, before any of this gets trusted.

Velocity is the same story. I used a flat 35–40 story points as a reasonable placeholder, but a real team's velocity comes from sprint history: typically a six-sprint rolling average, adjusted for holidays, onboarding, and whatever technical debt cycle they're mid-way through.

And I'd want a full dependency map across all 20 items before making a single sequencing call, not just the one dependency that happened to surface on its own. Non-obvious dependencies — a compliance item quietly blocking a data pipeline item, for instance — tend to change the optimal sequence in ways raw WSJF scoring can't see on its own.

---

*This document reflects my actual decision-making process in building this project — where the PM thinking ends and the AI tooling begins, and where it doesn't.*

*[Back to README](./README.md)*

