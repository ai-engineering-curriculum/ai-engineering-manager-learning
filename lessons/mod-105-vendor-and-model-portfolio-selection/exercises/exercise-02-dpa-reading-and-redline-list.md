# Exercise 02: DPA Reading and Redline List

**Estimated effort:** 2 hours · **Output:** (a) a clause-by-clause read of one real, publicly-available vendor DPA, and (b) a one-page redline list the organisation would defend.

## Objective

Produce the two artefacts chapter 05 commissions:

1. A **clause-by-clause read** of one real, publicly-available vendor Data Processing Addendum, mapping each clause to its escalation target (counsel / procurement / AI Engineering Manager) and noting the organisation's position.
2. A **one-page redline list** generalised from the read — the list of positions the organisation would defend on *any* AI-vendor DPA going forward.

This exercise is specifically scoped so you do not need legal advice to complete it. You are learning to *recognise* the clauses, *flag* the escalations, and *hold* the redline list. You are not learning to write binding legal language — chapter 05's third deferral.

## Prerequisites

- Chapter 05 (`Reading a DPA and Partnering with Procurement`) — the clause families and the escalation model.
- Chapter 02 (`The Frontier Portfolio Landscape`) at a skim — the DPA posture is one of the ten dimensions of an inventory row.
- Ability to open a public DPA online. For the exercise, pick one of:
  - OpenAI's DPA — `openai.com/policies/data-processing-addendum/`
  - Anthropic's DPA — `anthropic.com/legal/dpa`
  - Google Cloud's data-processing terms — `cloud.google.com/terms/data-processing-terms`
  - AWS's GDPR DPA — `aws.amazon.com/compliance/gdpr-center/` (via the current terms link)
  - Microsoft's Product Terms / DPA — `microsoft.com/licensing/docs` or the specific Azure OpenAI terms page
  - Cohere's DPA — published on their legal page
- Access to the vendor's current sub-processor list and SOC 2 statement where relevant (public-facing pages only — the exercise is based on publicly-readable material).

If your organisation has a customer-facing DPA you can read, open it alongside; the point of this exercise is that your *customer DPA* constrains what you accept in your *vendor DPA*. If your organisation does not have a customer DPA you can access, use a representative public-facing DPA from a company of comparable posture as the proxy constraint.

## Problem statement

### Part A — clause-by-clause read (75 minutes)

Pick one vendor DPA from the list above. Read it end to end. For each of the clause families below, record:

1. **Clause family** (from chapter 05 core concept 1).
2. **Vendor's position** (two or three sentences, quoting the DPA's actual language where helpful).
3. **Your organisation's position** (what you would ask for).
4. **Delta** (acceptable / needs negotiation / non-starter).
5. **Escalation target** (counsel / procurement / AI Engineering Manager).

The ten clause families from chapter 05:

- Training on customer data
- Data retention
- Data residency (processing and storage)
- Sub-processors (list, flow-down, notification period)
- Deletion rights and data-subject requests
- SLA, credits, and remedies
- Indemnity (IP on outputs, data breach, confidentiality)
- Confidentiality and restrictions on vendor's use
- Audit rights
- Term, termination, deprecation and change notice

Output format: a markdown table or a one-section-per-clause document, three to six pages.

### Part B — the one-page redline list (30 minutes)

Generalise from the specific read into a **portable** one-page redline list — the positions the organisation would defend on *any* AI-vendor DPA, not just this one. For each clause family above:

- **The organisation's position** (one sentence).
- **The minimum acceptable fallback** (if the vendor refuses the primary).
- **The escalation target.**

The list should be short enough to fit on one page and specific enough that a procurement partner reading it can take it into a negotiation without re-asking you for the details.

### Part C — the unknowns (15 minutes)

At the bottom of part B, write a **three-to-five item list** of questions you cannot answer without your organisation's counsel or without organisation-specific context. Examples: "Is the IP-indemnity cap of X sufficient for our outputs' volume?" "Does our customer DPA language require 15-day or 30-day sub-processor notice?" "Are there regulatory frameworks (HIPAA, GLBA) that pin specific DPA language we must insist on?"

This list is part of the exercise's output — knowing what you cannot answer solo is as important as writing the positions you can.

## Requirements

- Part A: all ten clause families from chapter 05 covered; no clause family silently skipped. A clause family genuinely not present in the DPA gets a "not addressed in this DPA; see position in redline list" entry.
- Part A: at least three clauses flagged to counsel, at least three flagged to procurement, and at least two flagged to the AI Engineering Manager. If your DPA read ends with "escalate everything to counsel", re-read chapter 05 — the whole point of the escalation model is parallel, scoped routing.
- Part B: one page, fits on one printed page. More than one page means the list has stopped being a portable artefact.
- Part B: every clause family in part A has a corresponding line in part B's redline list.
- Part C: 3–5 specific questions, each pinned to a specific escalation (counsel / specific internal stakeholder). "What should we do?" fails.

## Starter guidance

- Read the whole DPA once, straight through, before taking notes. The clauses reference each other and often sit in unexpected sections (indemnity in the master agreement, residency in an addendum, sub-processors in a linked page). A first-pass read orients the second-pass annotation.
- When you flag to counsel, be specific about *what the opinion is*. "Review for sufficiency of IP indemnity given our outputs volume" is actionable; "ask legal" is not. Chapter 05's escalation model depends on the manager routing a well-formed question.
- The training-on-data clause is sometimes buried. Look for the words "improve", "enhance", "develop", and "service" — training language often hides in a "service improvement" clause rather than under an explicit "training" heading.
- The residency clause is often in a separate schedule or annex, not in the main DPA. Follow the cross-references.
- The sub-processor list is usually on a separate URL. Open the sub-processor list as part of the read; a DPA whose sub-processor list is "inaccessible to me as a non-customer" is itself a redline (sub-processor transparency).
- For part B, your organisation's position should state a *concrete ask*, not a philosophy. "Prefer short retention" fails; "Zero retention (ZDR) for inputs and outputs; abuse-monitoring exception acceptable if scoped and SLA-ed" passes.
- For part C, every question should name the person or role whose opinion resolves it. "Counsel to opine on …" or "DPO to confirm …" or "Head of Procurement to decide …" — the question is incomplete without the owner.

## Acceptance criteria

- Part A covers all 10 clause families against the chosen DPA; each has vendor position, organisation position, delta, and escalation target.
- Part A's escalation is spread across counsel / procurement / manager, not concentrated on one.
- Part B fits on one page, carries all 10 clause families, and has a position / fallback / escalation per family.
- Part C lists 3–5 specific questions, each pinned to a named escalation.
- The chosen DPA is one of the real, publicly-readable vendor DPAs listed in the prerequisites.
- A peer AI Engineering Manager can read part B and hand it to a procurement partner without further explanation.

## Stretch goals

- **Second-vendor comparison.** Do part A against a second vendor's DPA and produce a **crosswalk** — a two-column table showing how the two vendors' positions differ on each clause family. This is the artefact that justifies "vendor X is the primary because their DPA sits closer to our customer DPA than vendor Y's."
- **Customer-DPA back-reference.** Read your organisation's current customer-facing DPA and annotate the redline list with which of your organisation's commitments each line supports. A DPA clause the organisation promised its own customers but isn't defending in vendor contracts is a gap. Flag it to your DPO or counsel.
- **Sub-processor subscription log.** For each vendor in your inventory (exercise 01), find the URL for the current sub-processor list and the subscription mechanism (RSS, email list, webhook). File a one-page subscription log with named subscribers on your team.
- **Change-of-posture history.** For your chosen vendor, dig up the DPA change history — the vendor often publishes dated version notes. Write a half-page memo on what changed in the last 12 months and whether any change would have warranted a re-opened contract conversation at the time.
