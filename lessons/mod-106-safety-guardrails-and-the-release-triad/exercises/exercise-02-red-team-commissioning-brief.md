# Exercise 02: Red-Team Commissioning Brief

**Estimated effort:** 2 hours · **Output:** a 1–2 page red-team commissioning brief for one feature, plus a one-page findings-handling appendix the brief references.

## Objective

Produce the two artefacts chapter 04 commissions — for the same feature you wrote the guardrails spec for in exercise 01 (recommended) or a different feature where you have enough context to be honest.

1. A **commissioning brief** that scopes an actual red-team engagement an `ai-risk-engineer` (internal) or an external vendor practitioner could execute without a second round of clarifying questions.
2. A **findings-handling appendix** that names the disposition rules, SLAs, escalation paths, and accepted-risk register format the engagement feeds into.

A peer AI Engineering Manager should be able to read the brief in ten minutes, judge whether the scope and the time budget match, and either sign or push back with three sharp questions.

## Prerequisites

- Chapter 02 (`Guardrails as Product Spec`) — the spec's section 1 risk model drives the brief's categories of concern.
- Chapter 04 (`Commissioning Red-Team Work`) — the brief's six sections, the three engagement shapes, the findings-review ritual.
- Chapter 07 at a skim — if the feature calls tools, the authorisation boundary informs the attack-surface description.
- Chapter 06 at a skim — if the feature depends on a provider model at a new capability tier, that tier is part of the scope.
- The guardrails spec from exercise 01 (or an equivalent).

## Problem statement

Two artefacts, written in markdown, filed where your team's security / safety work is tracked.

### Artefact A — the commissioning brief (1–2 pages, 75 minutes)

All six sections from chapter 04 core concept 1, with concrete content.

1. **Product surface and scope** — which feature; which surface (API, chat, embedded); which deployment tier (staging? production-like? production with kill-switch?); which data classes; what is explicitly **out** of scope (e.g. infra-layer; the underlying model's capability evals the provider runs; adjacent features served by other teams).
2. **Risk model and categories of concern** — categories the engagement should probe, with product-specific instantiations. Draw from:
   - OWASP LLM Top 10 categories that are in-scope per exercise 01's coverage matrix.
   - NIST GenAI Profile categories applicable to the product.
   - MITRE ATLAS tactics relevant to the attack surface (name at least two tactics the brief names, chosen because they apply to the feature's shape).
   - Product-specific harms unique to this feature.
   - One category called out as **priority** (the manager's top concern), with a one-line "if the engagement finds nothing here I would be surprised" statement.
3. **Attack surface description for the practitioner** — the agent's input channels; the system-prompt skeleton (shape, not the full prompt); the tool catalogue from chapter 07's boundary document and each tool's authorisation posture; the HITL checkpoints; the guardrail stack from exercise 01's spec and its failure behaviour; the retrieval sources (if any); the data classes and their trust level.
4. **Success criteria and time budget** — what the engagement produces (findings list with severity / reproducibility / affected surface / proposed mitigation; coverage report against section 2 categories; one-page written report); the time budget (person-days of effort, calendar window); what is explicitly **not** expected.
5. **Findings handling and escalation path** — points to the findings-handling appendix (artefact B); names the critical / high / medium / low disposition rules and the "block ship" trigger; names the escalation path for cross-cutting findings (findings that implicate legal, security, a different team, an external vendor).
6. **Confidentiality and disclosure posture** — the NDA and confidentiality boundary; what the practitioner may publish or discuss externally; the disclosure timeline for critical findings if the engagement is external; the attribution posture for an internal engagement.

Append a **sign-off** block: who commissioned, who executed (named or "to be assigned by Q-start"), expected report date, next-engagement window.

### Artefact B — findings-handling appendix (1 page, 30 minutes)

A single page the brief points to in section 5. Content:

- **Disposition rules.** Four dispositions (fix / mitigate / accept / block ship) with one-sentence definitions and typical examples.
- **SLAs.** Critical findings halt release; high findings 2-week fix or deployed mitigation; medium findings 1-quarter fix or accepted-risk entry; low findings logged with aggregate-pattern review in the quarterly red-team retrospective.
- **Accepted-risk register format.** A schema for accepted-risk entries — id, finding, rationale, mitigations deployed, co-signatures required (manager + which other roles), expiry date, review-trigger (what event forces an earlier re-evaluation).
- **Escalation path.** Two columns: "finding type" and "escalates to (role + named human)." Cover: cross-tenant data leak, PII exfiltration, credential leak, cost-blast-radius, legal / regulatory implication, upstream provider vulnerability, product-design flaw (routes to product), security-engineering-level vulnerability (routes to security).
- **Tracker metadata.** Where findings live (same tracker as the team's other work? separate security-restricted tracker for critical findings before fix?); the labelling convention; the retention policy on the findings record.

### Remaining time (15 minutes)

Cross-reference the brief against exercise 01's coverage matrix: every priority / high-priority harm in the matrix should appear as a category in section 2 of the brief, or an explicit note on why it is not probed in this engagement (e.g. "covered by the external quarterly engagement; this scoped engagement focuses on the agentic-surface categories added in v2.3"). Fix any gaps.

## Requirements

- All six sections of the brief present, each with concrete content specific to the feature.
- Section 2 has **minimum five** categories, each with a product-specific instantiation (not just the generic OWASP / NIST / ATLAS label).
- Section 2 names at least one priority category.
- Section 3 describes the attack surface in enough detail that a practitioner does not have to ask "where do I attack?" A concrete sign: section 3 names the tool catalogue (if applicable), the input channels, and the guardrail stack from exercise 01.
- Section 4's time budget is specific (person-days of effort + calendar window); "we have a few weeks" fails.
- Section 5 references the findings-handling appendix (artefact B).
- Section 6 names the confidentiality posture even if the engagement is internal ("internal; findings restricted to <security-restricted tracker> until mitigated").
- The findings-handling appendix is one page, covers all five sub-sections above, and names escalation targets by role (ideally with named-humans placeholders).
- The brief cross-references at least one artefact from another module (the eval registry from mod-104; the portfolio from mod-105; the incident playbook from mod-107 for the escalation path) — safety work does not live in isolation.

## Starter guidance

- The brief is a contract with the practitioner. Write it as though you will hand it to someone you have never worked with. If the brief depends on tribal knowledge about the feature, document the knowledge in section 3.
- Section 2 is where most briefs fail. A generic "test prompt injection" is not a category; "attempt indirect prompt injection via retrieved customer emails to cause the agent to emit the `send_email` tool with an attacker-controlled recipient" is. Write each category with one of these specific instantiations.
- The time budget is a forcing function. If the categories in section 2 need 15 person-days and the budget is 5, something must give: narrow categories, or expand budget, or split across two engagements. Making the trade-off explicit in the brief is the manager's job.
- For section 3, the attack surface description should be about as long as the spec's section 1 risk model from exercise 01. If it is shorter, the practitioner is missing context; if it is much longer, you are probably over-specifying.
- For the escalation path in the findings-handling appendix, be specific about cross-tenant findings. Those are the ones that most often fall through the cracks between teams.
- The accepted-risk register format is where product safety becomes an auditable artefact. Entries with no expiry are forever; entries with no co-signatures on severity-warranted findings are not accepted risks, they are shrugged risks. Make the required co-signatures explicit.
- Pin the OWASP, NIST, and ATLAS versions in the brief so a practitioner joining in six months knows which taxonomy ids you meant.
- Do not promise what you cannot deliver. "Red-team will have access to a production clone with full tool catalogue" is a strong brief; it also requires that the production clone exists. If it does not, the brief says "red-team runs against staging with production-equivalent tool catalogue" and notes the limitation in section 4.

## Acceptance criteria

- The brief is 1–2 pages; all six sections present and concrete.
- Section 2 has ≥ 5 categories with product-specific instantiations and a named priority.
- Section 3's attack surface is detailed enough that a practitioner can start in < 30 minutes of orientation.
- Section 4's time budget is a specific number of person-days plus a calendar window.
- Section 5 references the findings-handling appendix and names the "block ship" trigger.
- Section 6 names the confidentiality posture even for internal engagements.
- Findings-handling appendix is 1 page; four dispositions; SLAs; accepted-risk register schema; escalation path by finding type.
- Cross-reference to exercise 01's coverage matrix: no priority / high-priority harm in the matrix is absent from the brief.
- Cross-reference to at least one other module's artefact (eval registry, portfolio, incident playbook).
- A peer AI Engineering Manager can read both in 10 minutes and either sign or ask three sharp questions.

## Stretch goals

- **Three-shape plan.** Write a one-page plan for the next four quarters that distributes engagements across the three shapes (internal IC, external vendor, bug-bounty / internal rotation) with named owners, budgets, and expected coverage. The plan should include the capability-tier trigger from chapter 06 that fires a tier-appropriate engagement when the model portfolio migrates.
- **Automated regression pairing.** Append a one-page specification of which automated jailbreak probes (garak, PyRIT, etc.) run nightly as the eval-registry regression surface (mod-104 chapter 06), and which categories they are not expected to cover. This is the complement that lets the human engagement focus on novel work.
- **Retrospective template.** Write the format of the quarterly red-team retrospective — aggregate findings patterns, category re-prioritisation for next quarter, coverage gaps, process improvements. The output is the commissioning brief for next quarter's engagement.
- **External engagement SOW translation.** If the brief is for an external engagement, extract the SOW-ready version — with the non-contractual sections stripped, confidentiality upgraded to NDA-language, and the deliverables phrased in SOW terms. Pair with the procurement handshake from mod-105 chapter 05.
