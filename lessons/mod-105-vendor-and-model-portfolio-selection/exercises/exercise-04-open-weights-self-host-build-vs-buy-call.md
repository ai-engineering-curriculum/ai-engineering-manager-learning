# Exercise 04: Open-Weights / Self-Host Build-vs-Buy Call

**Estimated effort:** 3 hours · **Output:** a two-page memo that lands on *prefer API* or *self-host*, with a fully-loaded cost model, a licence read, and the Infra-partnership surface named.

## Objective

Produce the honest memo chapter 06 commissions — the artefact that commits one feature (or one subset of a product's traffic) to either an API-based path or a self-hosted path on open-weights (or a managed-open-weights intermediate step). The memo must include the fully-loaded cost model, the licence read, the operational commitment, and the exit criteria.

The exercise is specifically about *landing the memo honestly*. Many memos written in the wild recommend self-host because the author wants self-host; the discipline here is to write a memo that would recommend *prefer API* if the honest analysis lands there.

## Prerequisites

- Chapter 06 (`Open-Weights and Self-Host, Honestly`) — the four narrow cases, the hidden costs, the Infra partnership, the serving-stack landscape, the licence reading.
- Exercise 01's inventory and selection memo — the memo references the feature and the current primary.
- Some access to real pricing — your organisation's cloud bill for GPU capacity, if you have one, or a public pricing page for an on-demand GPU provider. The cost model needs honest numbers, not round-numbered approximations.
- One peer who could play the role of the AI Infra manager (`ai-infra-team-lead`) for the exercise. If you have a real counterpart, run this with them for 20 minutes to get real numbers. If you do not, mark the Infra-partnership specifics with `[placeholder: co-signed by peer Infra manager at Y-date]` and continue solo.

## Problem statement

Pick **one specific feature** from the product you used in exercise 01. It may be the same feature or a different one. For that feature, write the two-page memo in the shape chapter 06 core concept 6 commissions. Six sections.

### Section 1 — Feature profile and product rationale (one paragraph)

Which feature, what shape, what traffic. Name explicitly which of the four narrow cases (chapter 06 core concept 1) the self-host justification would fall under — data posture, latency floor, cost crossover, specialist fine-tune — or state that **none** applies. If none applies, the memo is a *prefer API* memo; write it as such.

### Section 2 — The candidate stack (half page)

If self-host is on the table, name:

- Model identifier and licence (chapter 06 core concept 5).
- Serving framework (vLLM, SGLang, TGI, TensorRT-LLM, Triton, TEI, or managed-open-weights alternative).
- Hardware (specific SKU, or `[placeholder: specified by Infra at Y-date]`).
- Region.
- Reserved-vs-on-demand split.

If self-host is not on the table, this section is "n/a — self-host not recommended" with a sentence pointing to section 3's analysis.

For completeness, list the **managed-open-weights alternative** you considered (Together, Fireworks, Groq, Cerebras, Databricks, Anyscale, Modal, AWS Bedrock-hosted Llama, Azure-hosted Llama) even if the recommendation is *prefer managed-API*. Many teams skip this step and jump from closed-API to true self-host; chapter 06 is explicit that managed-open-weights is the most-underweighted intermediate option.

### Section 3 — The full cost model (half page)

The eight line items from chapter 06 core concept 2, each with a number and a basis. Compare against the current API alternative at the feature's traffic.

The eight lines:

1. Engineering cost — build (eng-months × fully-loaded compensation).
2. Engineering cost — operate (ongoing eng-months per year).
3. Engineering cost — keep up (periodic re-eval and migration).
4. Hardware cost (reserved-or-on-demand, divided by honest utilisation, not nameplate).
5. Observability and operational cost.
6. Compliance evidence cost.
7. Model-upgrade risk (write a dollar figure or state it as probability-weighted).
8. Disaster recovery.

The API alternative column has **one** line: total cost per month at current traffic + projected traffic growth.

A memo whose cost model omits three or more of these lines fails the exercise; the discipline is showing the honest surface, not an answer.

### Section 4 — Quality evidence (two paragraphs)

The open-weights (or managed-open-weights) candidate's eval score against the feature's thresholds (from exercise 01's memo). The regression (or improvement) vs. the API alternative. The fallback plan if the chosen path underperforms in production (reference chapter 04 / exercise 03).

If eval evidence does not yet exist, state the plan to commission it: who runs the eval, by when, against which dataset version. A memo without eval evidence is a memo without a decision — but it can commit to commissioning the eval as its first action.

### Section 5 — Operational commitment (one paragraph)

If self-host is recommended: on-call rota, SLO targets, pager threshold, the named humans who carry the pager on the Infra side, the escalation path. Co-signed by the peer AI Infra manager (or `[placeholder: co-signed at Y-date]` with a deadline).

If self-host is *not* recommended but managed-open-weights is: the operational commitment is lighter — the manager still names the escalation path and the on-call touchpoints.

If API-only is recommended: this section notes the existing API-side on-call surface (reference exercise 03's runbook) and is one sentence.

### Section 6 — Ownership and exit criteria

- IC owners on both AI Engineering and AI Infra sides (named humans).
- AI Engineering Manager (you) and peer AI Infra manager (named, if self-host is recommended).
- Exit criteria — under what conditions would we revisit this decision? Specific numbers:
  - On-call load exceeds X pages / month → revisit.
  - Eval score regresses by Y on category Z → revisit.
  - Cost overruns by W % for Q consecutive months → revisit.
  - Managed-open-weights provider enters market at price < current cost by Z % → revisit.
  - Licence or acceptable-use clause changes → revisit.
- Next quarterly review date.

## Requirements

- Two pages. A six-page memo is a design doc; a one-paragraph memo is a Slack message.
- The four-narrow-cases justification (section 1) is named by name, or the memo lands on *prefer API* / *prefer managed-open-weights*.
- The cost model (section 3) carries all 8 line items with numbers; if a number is a placeholder, it is `[placeholder: source X by date Y]` and the owner is named.
- The managed-open-weights alternative is considered in section 2 even if the recommendation is something else.
- Section 5's operational commitment names humans (or placeholders with dates); "the Infra team" is not an owner.
- Section 6's exit criteria are specific numbers or specific events; "if things change" fails.
- The memo cites licences (section 2) — if open-weights is in scope, the licence is named and the acceptable-use clause has been reviewed.
- Peer-review-ready: a peer AI Engineering Manager reading the memo should be able to say (a) "I agree with the recommendation" with reasons, or (b) "I would push back on these specific numbers" with reasons. "This feels right" from a reviewer is a fail.

## Starter guidance

- Start by naming which of the four cases applies, in one sentence. If none applies, write the *prefer API* memo; most teams have more features that belong in this bucket than in the self-host bucket. Writing a clean *prefer API* memo is harder than it sounds — it must honestly surface the alternative rather than dismiss it.
- The cost model (section 3) is where most memos fail the exercise. Spend at least 45 minutes on this section. If the numbers come out cleanly in favour of self-host, you are either genuinely past crossover or (more likely) you have omitted one of the eight lines. Re-read chapter 06 core concept 2 and check.
- For the managed-open-weights option (Together, Fireworks, Groq, Cerebras, etc.), check public pricing pages for the specific model you are considering. The pricing often looks surprisingly close to true self-host once self-host's engineering and operational costs are loaded, and the DPA posture of these providers is in the inventory alongside the closed APIs.
- Licence reading is a 15-minute task per model. For Llama specifically, read the current Llama Community Licence (versions differ per release); for Mistral, check whether the specific model is Apache 2.0 or the Mistral Research Licence (the research licence does not permit commercial use); for Qwen / DeepSeek, licences vary per release. If the memo cites "Llama" or "Qwen" without naming the licence version, re-read chapter 06 core concept 5.
- The peer AI Infra manager's role is non-negotiable if self-host is recommended. If you do not have a peer Infra manager in your organisation, section 5 should flag that as the blocking issue — the self-host decision cannot be made solo, and the first action is hiring or reassigning that role, not building the stack.
- For an honest *prefer API* memo, the exit criteria are about when to re-open the decision in the future. "Traffic grows past X calls/day for 12 consecutive months" is a specific exit criterion; "if our scale changes" is not.
- If your organisation already self-hosts, treat this memo as a *re-justification* of the current state, not as an onboarding. Many teams discover during re-justification that they are past the four narrow cases and should migrate back to API or managed-open-weights.

## Acceptance criteria

- Two-page memo with all six sections present.
- Section 1 names which narrow case applies or honestly lands on *prefer API* / *prefer managed-open-weights*.
- Section 2 names a candidate stack (with licence) if self-host is on the table, or an honest "n/a" with reasoning; managed-open-weights considered.
- Section 3's cost model has all 8 lines with numbers (or placeholders with owners and dates); API alternative compared.
- Section 4 cites eval evidence or commits to commissioning it.
- Section 5 names on-call humans (or placeholders with dates); peer Infra manager co-signs if self-host is recommended.
- Section 6 exit criteria are specific numbers or specific events.
- Licences named and reviewed where applicable.
- Peer-reviewable: a peer AI Engineering Manager can read it and either sign off with reasoning or push back on specific numbers with reasoning.

## Stretch goals

- **The honest re-justification.** If your team already self-hosts a model, write the re-justification memo for the current state. This is often an uncomfortable exercise — many self-host decisions made in 2023 or 2024 have had their cost crossover eroded by API price reductions and by managed-open-weights entrants. Writing the honest memo is the first step to migrating back if the analysis lands there.
- **Managed-open-weights deep dive.** Pick one managed-open-weights provider (Together, Fireworks, Groq, Cerebras, or a cloud hyperscaler's hosted-open-weights offering) and run a full inventory-row-style evaluation of it against a specific model: DPA posture, pricing, latency, SLA, enterprise features. This is the row that would go into exercise 01's inventory if your team adopts this path.
- **Infra-partnership interview.** If you have a peer AI Infra manager, run a 30-minute conversation with them seeded by the memo draft. Take notes on where their numbers differ from yours (they usually differ on engineering time — Infra managers quote twice what AI Engineering managers estimate) and revise the memo with their input. The revised memo is substantially more defensible.
- **Portfolio-level build-vs-buy summary.** Across all the features in your product (using exercise 01's inventory), write a one-page memo that classifies each feature into API-first, managed-open-weights-candidate, or self-host-candidate. This is the artefact that goes to mod-110's budget review as the forward-looking signal for AI engineering spend.
- **Second-opinion request.** Write a one-page "second-opinion request" that would go to another AI Engineering Manager at a peer organisation (or to an advisor). State the feature, the recommendation, and the three specific numbers in your cost model you are least confident about. The exercise is to know what you don't know well enough that an outside read would catch it.
