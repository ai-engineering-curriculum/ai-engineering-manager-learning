# Exercise 01: Team Shape and Span Decision For One Team

**Estimated effort:** 2 hours · **Output:** a written, dated team-shape decision document covering mix, alignment, embedded PM, and the current span read, for one real (or realistic composite) team.

## Objective

Produce the first manager-owned artefact of this module: the written, dated team-shape decision specified in chapter 02, anchored by a quarterly span read specified in chapter 06. The document is the artefact your team's next hiring decision, ritual audit, and quarterly operating review will read against.

A peer AI Engineering Manager, reading your decision, should be able to describe your team back to you in two paragraphs — the mix, the alignment, the embedded-PM choice, the direction the span is heading — without asking you to translate any secret-language shorthand.

## Prerequisites

- Chapter 01 of this module for the four manager-owned artefacts and the four deferrals.
- Chapter 02 for the full shape-decision structure (three axes, workload specialisation, alignment vocabulary, PM question).
- Chapter 06 for the span-read structure (four weights, three directions, quarterly read).
- Mod-101 chapter 01 (the AI-engineering role matrix and the six neighbouring roles the manager defers to).
- Skelton & Pais, *Team Topologies*, for the alignment vocabulary (stream-aligned, platform, enabling, complicated-subsystem; collaboration, X-as-a-service, facilitating).
- Larson, *An Elegant Puzzle*, for the team-sizing framework this chapter specialises.
- Your team's (or a realistic composite team's) current state: the workload it carries, the surface it ships to, the current roster, the current adjacent dependencies.

## Problem statement

A written team-shape and span decision, in markdown, filed where your team's engineering handbook lives (team wiki, Notion, GitLab-style handbook). The document has five parts.

### Part A — the context (15 minutes)

A header block and context paragraph:

- Document version, date, author (you), the staff-plus IC partner who reviewed it (or a note on why none was consulted), and the next scheduled re-read date.
- A one-paragraph context write-up: what product surface(s) or capability the team is being asked to carry; which product KPIs or capability KPIs it reads on; which adjacent teams and dependencies it interacts with; the time horizon the decision is being made against.

### Part B — the mix (30 minutes)

A section stating the mix numerically and justifying it against the AI engineering workload breakdown.

- **The numeric mix.** A line per IC shape and partnership role: `3 x applied-ai-engineer`, `1 x ai-eval-engineer`, `1 x rag-engineer`, `0.2 FTE agentic-systems-architect partnership (dotted-line)`, `1 x product manager`, `1 x engineering manager`. Include half-time rotations (infra, security, on-call) explicitly.
- **The workload justification.** A short table or set of paragraphs mapping each of the AI engineering workloads from chapter 02 (new-feature delivery, measurement, external-surface absorption, non-deterministic incident command, retrieval depth, specialist fine-tune, architectural direction) to the shapes in the mix that carry it. Each workload has to have a named owner or an explicit deferral to an adjacent team; a workload with neither is a gap the exercise surfaces.
- **The common-mistakes check.** A short paragraph self-auditing against the three common mix mistakes chapter 02 names (measurement as unfilled seat, fine-tune hire before fine-tune workload, no staff-plus partner anywhere).

### Part C — the alignment (20 minutes)

- **The alignment choice** in *Team Topologies* vocabulary: stream-aligned, platform, enabling, or complicated-subsystem. State it plainly.
- **The justification** against the three readings chapter 02 names (workload one-surface-deep → vertical; workload two-or-more-pods-would-duplicate → horizontal; depth-beyond-one-pod → complicated-subsystem).
- **If horizontal or complicated-subsystem**: the interaction mode (collaboration, X-as-a-service, facilitating) per dependent team, written down, with a note on whether the receiving managers have agreed to the mode. If the agreement is pending, name the date you will have it.
- **The time horizon**: this alignment is valid for what period? What workload shift would re-open the decision?

### Part D — the embedded-PM choice (15 minutes)

- **The choice**: yes (dedicated embedded PM), no (platform / internal-customer-only, shared with manager + staff-plus), or shared (one PM across multiple teams).
- **The justification** against the four things chapter 02 names an embedded PM covers (quality floor, roadmap balance, cross-functional surface, release discipline). If the choice is "no," name explicitly who is covering each of the four and whether that cover is sustainable.
- **The AI-PM note**: if the PM is an AI-specialist PM or a general PM, name it. If a general PM, name the on-ramp they are going through to read evals and traces as part of their craft.

### Part E — the span read (40 minutes)

The honest span read specified in chapter 06. Four sub-sections.

1. **The four weights.** For each of the four weights (direct reports and IC cadence; AI-specific rituals; triage + incident; connective-tissue), a short paragraph estimating the current cost on your week. The ritual-cost ledger (chapter 03 core concept 4) is an input if you have one; if you do not, produce a first-draft ledger here.
2. **Your per-IC time estimate.** A short line per direct report: current level, current growth-plan status, current promotion-cycle status, current hard-conversation status, time-cost estimate (hours per week on 1:1 + growth + review + promotion-packet + hard-conversation work).
3. **The direction.** Which of the three directions is the team currently in (partner-needed, split-needed, manager-to-grow), or is it steady state? A short defence of the direction read against the telling signals chapter 06 names.
4. **The planned action.** If the direction is not steady state, what are you doing about it, by when, with whom? If the direction is steady state, what would move it to not-steady-state, and when is the next span read scheduled?

## Requirements

- The document is a single markdown file in your team's engineering handbook; the exercise submission links to it.
- The document has a version, a date, an author, a reviewer (or a note), and a next-re-read date.
- The mix is stated numerically and justified against the AI engineering workload breakdown; every workload has a named owner or a written deferral.
- The alignment is in *Team Topologies* vocabulary; interaction modes per dependent team are named where relevant.
- The embedded-PM choice is explicit and justified; if "no," the four PM-covered responsibilities are redistributed with named owners.
- The span read covers the four weights, a per-IC cost estimate, the current direction, and the planned action.
- Length: 1,500–3,000 words. Longer than that is typically a sign the decision is unclear; shorter is typically a sign the span read is missing.
- The document is behaviour-worded on the span read: do not write "the team is tired" when you can write "1:1 cadence has slipped from weekly to biweekly in the last two months without an intentional decision."
- The document is honest about what you do not know: if the staff-plus IC partner has not reviewed it, say so and date the review; if the ritual-cost ledger is a first-draft estimate, label it.

## Starter guidance

- Write Part B (the mix) first. The mix is the hardest part to make honest — every working manager has a drift between the mix on paper and the workload the team actually carries. The exercise's leverage is the gap it surfaces.
- When mapping workloads to shapes, test the mapping against the three common-mix mistakes chapter 02 names. If measurement is tagged to "every engineer," you have motivation failure 1 and the fix is to name the eval ownership or the eval-team deferral explicitly.
- Part C (alignment) is usually quick for vertical pods and slow for horizontal teams. A horizontal team's alignment is only honest if the interaction mode is written down per dependent team; if you are writing a horizontal-team shape decision without the interaction modes, go get the receiving managers in the room before publishing.
- Part D (embedded PM) is the section most managers skip. If your team does not have an embedded PM today and you write "no," defend it with the four responsibilities mapped elsewhere. Half of the "no" answers written honestly turn out to be "we have been quietly running without one and it is costing us" — which is a shape failure the exercise surfaces.
- For Part E (the span read), resist the temptation to write the comfortable number. The honest arithmetic in chapter 06 (five to seven direct reports on a full AI-specific cadence) is a range; your team may sit under or over it for honest reasons. Write the honest number and the signals it rests on.
- Do not write Part E after the mix and alignment are "right." Write it against what is true today, not against what the mix-and-alignment decision implies ought to be true.
- Ask the staff-plus IC partner to read the document before publishing. If no staff-plus IC partner is available, say so and name the next-line-manager or peer manager you asked instead. A shape-and-span decision read only by its author is a decision that has not been challenged.
- Re-read the decision in 90 days (set a calendar reminder when you publish). The quarterly span read (chapter 06 core concept 5) is a scheduled artefact; schedule it now.

## Acceptance criteria

- The document is in your team's engineering handbook, linked from your exercise submission.
- The document has a version, date, author, reviewer, and next-re-read date.
- The mix is stated numerically with workload justification; every AI-engineering workload has a named owner or a written deferral.
- The alignment is in *Team Topologies* vocabulary with interaction modes written where relevant.
- The embedded-PM choice is explicit; if "no," the four responsibilities are redistributed with named owners.
- The span read covers the four weights, the per-IC estimate, the current direction, and the planned action.
- Length is 1,500–3,000 words.
- A peer manager reads the document and can describe the team back in two paragraphs within five minutes.
- The next-re-read date is on your calendar.

## Stretch goals

- **The counter-shape.** For the same team and the same workload, write an alternative shape decision that trades off differently (e.g., swap the RAG engineer for a second applied engineer plus a horizontal retrieval-capability team dependency). Describe what the counter-shape gains and loses. Chapter 02 is specific that the decision is a trade-off, not a recipe.
- **The quarterly re-read dry-run.** Your current shape decision is version 1.0. Write version 1.1 anticipating the frontier-model generation shift that may land in the next quarter, or the product-surface expansion your team is being asked to carry next cycle. The v1.0 → v1.1 delta is the forcing function for which decisions were made against drift-sensitive inputs.
- **The hiring-plan cross-reference.** Take your shape decision's "next planned hire" implication and draft the open-role JD against mod-102 chapter 02. The mix justification should flow into the JD's expectations section.
- **The first-time-manager pivot overlay.** If your span read's direction is `manager-to-grow`, draft the 90-day coaching contract chapter 06 core concept 4 names, in addition to the shape decision. Submit both.
- **The horizontal-team service contract.** If your team is horizontal (platform / complicated-subsystem), draft the X-as-a-service contract with one of your dependent teams: capacity line, intake mechanism, SLA, roadmap input cadence, escalation path. The service contract is the artefact that prevents the request-queue failure (chapter 02 motivation 2).
