# Commissioning Red-Team Work

Red-teaming an AI product is a craft. The craft belongs to the `ai-risk-engineer` (level 25, peer-to-manager relationship) and, on agentic surfaces, to the `agentic-safety-engineer` (level 40). The AI Engineering Manager does not write jailbreak prompts personally — the manager **commissions** the engagement, **reviews** the findings, and **routes** the fixes. This chapter walks the three motions.

Three reasons the manager owns commissioning rather than delegating it:

1. The scope of a red-team engagement is a product decision. "What are we worried about?" is not a question the IC answers on behalf of the product.
2. The findings land as fixes, mitigations, or accepted risks. "Accepted risk" is a product signature — manager, not IC.
3. The red-team cadence has a time budget attached. Someone has to fund the engagement; that is the manager's quarterly plan.

## Motivation — three red-team failure modes

- **Launch-week red-team.** The feature is built, the demo is on Friday, and someone asks "has anyone tried to jailbreak it?" The resulting two-day scramble produces real findings — four or five, as it always does — but there is no time to fix them. They become either "known issues" or quiet overrides. The organisation did not commission red-team work; it did last-minute panic.
- **Scoreless red-team.** The team runs a jailbreak party. Someone produces a slide deck titled "We Tried to Break It." The slide deck lives on a shared drive. No finding has a ticket; no finding has an owner; no finding has a decision. Six months later the same jailbreak lands in the news.
- **Bug-bounty-but-not-really.** The team says "we have a bounty" because there is a form on the website. No researcher has ever been paid. No scope is published. The first time a real researcher tests the system, they find something severe and have no idea who to tell. The organisation claimed a programme but did not build one.

All three are cured by the same discipline: a written commissioning brief, a defined engagement shape, a tracked findings surface, and a regular cadence.

## Core concept 1 — the commissioning brief

The red-team commissioning brief is a one-to-two-page document the manager writes before an engagement starts. It is a contract with the red-team practitioner (internal, external, or bug-bounty researcher); it ensures the manager and the practitioner share expectations.

Six sections.

### Section 1 — product surface and scope

Which feature, which surface, which deployment tier, which data classes are in scope. Which are explicitly **out** of scope (another team's surface, infra-layer vulnerabilities, physical security). An engagement that lacks scope will spend half its budget on edges the manager did not want tested. An engagement that pretends to have no scope is a budgeting mistake.

### Section 2 — risk model and categories of concern

The categories the manager wants tested, with concrete product-specific instantiations. Draw from:

- **OWASP Top 10 for LLM Applications** categories (chapter 02 core concept 1) — prompt injection (direct and indirect), insecure output handling, excessive agency, sensitive information disclosure, overreliance, model denial of service, insecure plugin design.
- **NIST GenAI Profile** risk categories — misinformation, confabulation, CBRN uplift, dangerous or violent content, data privacy violations, intellectual-property harms, cybersecurity, information integrity.
- **MITRE ATLAS** adversarial tactics for AI systems — reconnaissance, initial access, execution, persistence, defence evasion, discovery, exfiltration, impact.
- **Product-specific harms** — the harms that are unique to this product and not covered in any public taxonomy. A medical-adjacent feature needs a category for clinical-decision confabulation; a code-execution agent needs a category for privilege escalation via tool-use chaining; a document-analysis agent needs a category for indirect injection via retrieved content.

Each category gets one or two sentences of product-specific instantiation — the thing that would make the harm concrete in this product. Not "prompt injection" but "an attacker embeds instructions in a retrieved customer email, causing the agent to email a competitor the customer's pricing."

### Section 3 — attack surface description for the practitioner

A one-page description of the feature as the practitioner sees it: the input channels (user text, uploaded files, retrieved documents, tool-call outputs), the system prompt skeleton (practitioners should know the shape of the system prompt they are attacking), the tool catalogue the agent has access to and the authorisation posture of each (chapter 07), the HITL checkpoints that exist, the current guardrail stack (chapter 03) and its postures.

This section is the manager's chance to prevent the engagement from spending a day re-discovering things the team already knows. "The agent has a `send_email` tool" is in-scope information; "the agent sees retrieved content wrapped in `<retrieved>…</retrieved>` tags" is in-scope information. The engagement is adversarial against the *product*, not against the practitioner's lack of information.

### Section 4 — success criteria and time budget

What a successful engagement produces. For most product-focused engagements:

- A categorised list of findings, each with severity (critical / high / medium / low / informational), reproducibility steps, affected surface, and proposed mitigation direction.
- A coverage report — which categories from section 2 were tested, and how exhaustively; which were deprioritised and why.
- A one-page written report the manager reads in thirty minutes.

Plus a time budget — days of effort, calendar window — and a clear statement of what is **not** expected ("We are not asking you to try to break the underlying model; the provider's capability-eval program covers that").

### Section 5 — findings handling and escalation path

How findings route into the team's work queue. The manager owns this routing:

- **Critical** findings halt the current release (release-triad gate in chapter 05). Fix before ship; manager personally signs when closed.
- **High** findings get a two-week fix SLA; mitigations in guardrail or HITL may close them earlier if the mitigation is well-tested.
- **Medium** findings get a quarterly fix SLA or an explicit accepted-risk entry with a documented reason and an expiry date.
- **Low / informational** findings are logged; aggregated patterns across many low findings trigger a re-scope of the next engagement.

Plus the **escalation path** for findings that cross boundaries the manager cannot handle unilaterally — findings that implicate legal, security, or a different team's system. A finding that discovers a cross-tenant data leak is not a bug the AI engineering manager closes alone.

### Section 6 — confidentiality and disclosure posture

For external engagements, the posture on confidentiality (NDA scope, what the researcher may publish, timelines for coordinated disclosure, credit). For internal rotations, the handling of findings before fixes are shipped (do not share widely until mitigated; use a security-restricted tracker). For bug-bounty engagements, the published disclosure policy.

## Core concept 2 — the three engagement shapes

Red-team work comes in three shapes. The manager picks the shape that matches the product's maturity, the current threat model, and the available budget.

### Shape A — internal engagement with a specialist IC

The `ai-risk-engineer` (level 25) or `agentic-safety-engineer` (level 40), where the role exists in the organisation, runs a time-boxed engagement against the feature. The commissioning brief is the contract. The practitioner spends their days writing attack prompts, chaining prompt-injection vectors, probing tool-use authorisation, and writing the findings report.

Commissioning-manager read: this is the strongest shape when the role exists. The practitioner has context; the findings are high-signal and well-prioritised; the reporting loop is tight because it is all internal. The weakness is independence — a practitioner who reports into the same manager as the team being tested will feel some pressure to find "the right amount" of bugs. Rotate practitioners across teams, or pair an internal engagement with a periodic external one, to counter this.

### Shape B — external vendor engagement

A specialist trust-and-safety firm or AI red-team consultancy runs the engagement under contract. The commissioning brief is the SOW's technical annex. Engagement duration is typically one to four weeks.

Commissioning-manager read: the strongest shape for independence and for diverse-perspective findings; the weakness is calendar time (procurement cycle + NDA + onboarding typically takes a month before the engagement starts) and context cost (the vendor spends the first days learning the product). Best used quarterly or around major new-capability launches, not as the day-to-day cadence.

### Shape C — bug-bounty / open programme

A published scope with a published reward schedule and a disclosure policy. Researchers submit findings; a triage rotation handles them. The commissioning brief in this shape is the **programme definition** — what is in scope, what is out, the reward tiers, the response SLA.

Commissioning-manager read: strongest shape for *continuous* coverage once the programme has researchers who recognise it. The weakness is the long ramp (a new programme sees few submissions for months) and the triage cost (every submission must be assessed, including the many that are not real). Best used in combination with shape A or B — the bounty catches the long tail; the specialist engagement catches the deep harm.

Most mature AI engineering organisations run **all three**: a rotating internal IC engagement per feature, a quarterly external engagement for independence, and an open bounty for continuous coverage. A smaller organisation may run only one; the manager's job is to be honest about which shape is active and which coverage is not.

### A note on automated red-teaming

Automated jailbreak probes (Garak, PyRIT, promptbench-style harness, provider-specific tools) are increasingly common. They produce breadth cheaply but rarely discover novel harms. Treat them as a **regression suite** that runs on every release — valuable as an eval surface in mod-104 chapter 06's cadence, not as a substitute for a human engagement. The commissioning brief may reference them in section 4 as "automated coverage runs nightly; this engagement focuses on the categories automated coverage does not reach well."

## Core concept 3 — the findings-review ritual

Findings land; something has to happen with them. The default is that nothing happens with them — reports pile up, each new launch adds more, and six months later the pile is a liability. The manager runs a weekly or fortnightly findings-review ritual to prevent this.

A findings review covers each new finding with four possible dispositions, each a written decision the manager signs.

- **Fix** — route to engineering with a ticket, an owner, and an SLA from section 5 of the brief. The fix is tested with a regression case added to the eval suite (mod-104 chapter 04) so the fix is durable.
- **Mitigate** — fix is non-trivial; mitigate via guardrail (chapter 02 / 03), via HITL (chapter 02 section 5), or via product scope reduction. The mitigation ticket is tracked the same way as a fix; the finding is only "closed" when the mitigation is live and tested.
- **Accept** — the risk is understood, the cost of the fix exceeds the business value of the mitigation, the accepted-risk entry is written. Accepted-risk entries carry a one-paragraph rationale, a dated signature (manager, plus whoever else — legal, security — needs to co-sign given the severity), and an expiry (typically 6 or 12 months) by which the decision is reconsidered. "Accepted risk" without an expiry is "ignored forever."
- **Block ship** — critical finding; the release-triad gate in chapter 05 fires red. Fix or mitigate before ship; no override except through the chapter 05 override protocol.

Keep the findings in the same tracker the team's other work lives in — not a separate "red-team spreadsheet" that only the manager reads. A bug is a bug; the fact that it came from a red-team engagement is a metadata tag, not a different work surface.

### The cross-cutting finding

Some findings cross concerns. A prompt-injection path that exfiltrates data via a tool call touches the agent authorisation boundary (chapter 07), the guardrail stack (chapter 03), the incident playbook (mod-107), and legal (data-protection obligations). The findings review names the lead owner — usually the AI Engineering Manager — and the review schedule across the crossing concerns. The manager's job is to prevent a cross-cutting finding from falling through the cracks between teams.

## Core concept 4 — the cadence

Red-team work is not a one-shot. The cadence the manager runs:

- **Per-feature engagement** — a time-boxed internal engagement scoped to the feature, timed before the release-triad gate (chapter 05) can be read green. A feature that has not been red-teamed cannot ship at HIGH severity.
- **Quarterly external engagement** — one external vendor engagement per quarter, rotating across the product's most sensitive surfaces. The output is both a findings list and a coverage statement against the surfaces the organisation runs.
- **Continuous automated regression** — the automated jailbreak probes run nightly as part of the eval suite (mod-104 chapter 06). New categories are added when a human engagement discovers them.
- **Open bounty, if operated** — continuous ingestion; weekly triage; monthly review of aggregate patterns.
- **Capability-tier trigger** — when the provider's RSP / Preparedness / FSF (chapter 06) advances the capability tier of the model the product depends on, a scoped engagement runs against the new-tier risks before the model migration ships. The migration brief from mod-105 chapter 07 fires this.

## Anti-patterns to name and defeat

- **"Red-team is an engineering tax."** No; it is engineering coverage. Treating it as a cost line that can be deferred is the cheapest way to import an incident.
- **The findings-dump review.** The review where the practitioner reads the findings aloud for an hour and no decisions are made is a review that produces no action. The findings-review ritual has disposition decisions on every finding before the meeting ends.
- **"The provider's capability evals cover this."** They do not. The provider's capability evaluation is done against the *model*; the product is the model plus the retrieval stack plus the tool catalogue plus the HITL plus the system prompt. The product-level red-team is still required, no matter which model is behind it.
- **"We tested it in a dev environment."** Many classes of finding — rate-limit interaction with caching, cost-blast-radius, cross-tenant data boundaries, OAuth scope confusion — only reproduce against the real infrastructure. The engagement should hit as close to production as the risk model allows, with a kill-switch and an on-call partner.
- **The hero red-teamer.** A single IC does every engagement, develops deep but idiosyncratic blind spots, and nobody else can run a session. Rotate; pair; cross-train.

## What good looks like

- The red-team commissioning brief for the current feature is open on the manager's desk the week before the first attack session.
- Findings land in the team's tracker within two business days of an engagement closing; each has a disposition within a week; critical findings halt the release.
- The accepted-risk register is dated, named, and has an expiry on every entry.
- The quarterly external engagement is on the calendar for the next four quarters with named vendors (or the honest statement "not currently operated, re-funded in Q4 2026").
- A new engineer, on their first day, can open the last engagement's report and see the findings, the dispositions, and the fixes shipped against them.

## What bad looks like

- "Red-team" is a hallway conversation that happens the week before launch.
- Findings live in a shared Google Doc that only the manager has opened.
- The accepted-risk register is undated and has no expiry.
- An external engagement has not happened in twelve months.
- The team shipped a model-tier migration (mod-105) without a tier-appropriate red-team against the new capability.

## Summary

- The red-team commissioning brief — product surface, risk model and categories of concern, attack-surface description, success criteria and time budget, findings handling and escalation path, confidentiality and disclosure posture — is the manager-owned contract that makes a red-team engagement produce decisions rather than reports.
- Three engagement shapes — internal IC specialist, external vendor, bug-bounty — and the mature organisation runs all three, with automated regression as a continuous eval surface.
- The findings-review ritual closes every finding with one of four dispositions — fix, mitigate, accept, block ship — each a written decision the manager signs. Accepted-risk entries carry expiry dates.
- The cadence is per-feature plus quarterly external plus continuous automated regression plus bounty plus capability-tier triggers from the model portfolio.

The next chapter picks up the third manager-owned artefact: the release-triad gate the manager personally signs, which reads the eval evidence, the data posture, and the safety case together before every release of consequence.
