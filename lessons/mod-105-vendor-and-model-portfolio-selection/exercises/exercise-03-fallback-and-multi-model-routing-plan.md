# Exercise 03: Fallback and Multi-Model Routing Plan

**Estimated effort:** 2 hours · **Output:** a one-to-two page fallback and routing plan for one production feature, plus the tabletop and restore-drill calendar entries.

## Objective

Produce the artefact chapter 04 commissions — a written fallback and multi-model routing plan for **one specific production feature** of the product you used in exercise 01. The plan must name the fallback shape(s), the routing policy, the abstraction layer, the triggers and restore conditions, the data-posture parity check, and the tabletop and restore-drill schedule.

By the end of the exercise, on-call should be able to read the plan at 2 a.m. during a vendor outage and know what to do. If it fails that test, it is not a plan.

## Prerequisites

- Chapter 04 (`Fallback and Multi-Model Routing`) — the four fallback shapes, four routing policies, abstraction layer, triggers and restore conditions, data-posture parity.
- Exercise 01's inventory and selection memo — the plan refers to the feature and the primary/fallback pair committed there.
- Enough context on your feature's production environment to answer: where does it run, what observability it has, who is on-call for it. If this is a planned feature not yet in production, mark operational specifics as `[placeholder: specify at launch]` with a date.

## Problem statement

Write the plan in markdown, one to two pages, with the following sections.

### Section 1 — Feature and primary/fallback pair (quarter page)

One-paragraph recap of the feature (reference exercise 01's memo, don't re-write it). The primary model + provider + hosting path (from the memo). The fallback model + provider + hosting path (from the memo).

### Section 2 — Fallback shape(s) (half page)

For this feature, name which of the four fallback shapes from chapter 04 apply — same-family downgrade, cross-provider swap, cached-response degradation, hard-offline refusal — or a combination. For each shape named:

- Why it is appropriate for this feature (the feature's shape and risk posture justify it).
- The quality delta on the fallback (eval evidence; cite from exercise 01).
- The user-visible behaviour change (if any — the UX spec sits here).

Shapes you *considered and rejected* get a one-sentence rejection. A feature with only one fallback shape should explain why (and may fail the exercise if no good explanation exists — few production features have only one appropriate shape).

### Section 3 — Routing policy (quarter page)

Which of the four routing policies (quality-first, cost-first, latency-first, capability-first) applies to this feature? If a hybrid (quality-first default with cost-first on a specific sub-call, for example), describe both parts.

If cost-first or capability-first routing is used, name the **routing-quality eval** that measures the classifier's decisions independently — chapter 04 is specific about this obligation.

### Section 4 — Abstraction layer (quarter page)

Name the layer every production call goes through. Pick one and justify in two sentences:

- LiteLLM (library or on-prem proxy)
- LangChain / LangGraph
- Portkey AI, OpenRouter, or similar managed gateway
- Vercel AI SDK, Mastra, or similar framework
- A thin internal wrapper

State the **current** state (is this already in place? partially? not at all?) and the **plan to get there** if it is not yet in place.

### Section 5 — Triggers and restore conditions (half page)

The signals that engage fallback, with specific thresholds, and the signals that restore to primary. Chapter 04's four trigger shapes:

- **Hard failure** (5xx, timeout): threshold and cooldown.
- **Rate-limited** (429): threshold duration.
- **Quality degradation** (online-eval triggered): threshold and whether auto-swap or manager-sign-off.
- **Content-policy block**: per-request detection and handling.

Each trigger has a **restore condition** — the signal that returns traffic to primary. A trigger without a restore is a permanent swap; chapter 04 is specific.

### Section 6 — Data-posture parity check (quarter page)

For each (primary, fallback) pair, confirm:

- Training-on-data posture matches (or fallback is stricter).
- Residency commitment holds on fallback.
- Sub-processor lists have been disclosed upstream (to your customers via your own DPA).
- Enterprise features required by the feature (HIPAA BAA, SOC 2, etc.) are matched on the fallback.

If any of these do not match, the fallback is not usable for this feature under its current posture — note the gap and the plan to close it.

### Section 7 — Tabletop and restore drill (quarter page)

The two rituals chapter 04 names:

- **Quarterly tabletop.** Date of next tabletop, attendees, scenario. One-sentence output expected.
- **Semiannual restore drill.** Date of next restore drill, traffic percentage, duration, rollback procedure, observability pre-wired.

Both are calendar entries. If the dates are TBD, name the owner who will schedule them and the deadline for scheduling.

## Requirements

- Plan is 1–2 pages; longer plans are less likely to be read at 2 a.m.
- Section 2 names ≥ 2 fallback shapes or justifies why only one applies.
- Section 3 names a routing policy; if cost-first or capability-first is used, the routing-quality eval is named.
- Section 4 picks one abstraction layer; raw-SDK application code is not an acceptable answer.
- Section 5 has specific threshold numbers (e.g. "2 consecutive 5xx within 60s") and specific restore conditions (not blank).
- Section 6 confirms parity on all four axes; a gap is a flag, not a silent omission.
- Section 7 has either dates or a named owner and a deadline for scheduling.
- The fallback cited in sections 1-6 matches the fallback in exercise 01's selection memo.

## Starter guidance

- Section 2 is the section where "same-family downgrade only" is the common failure. Same-family downgrade mitigates nothing when the provider is down; most production features need a cross-provider swap *plus* a degraded-mode or hard-offline for the catastrophic case. Default to two shapes unless a specific reason argues for one.
- Section 5's triggers are commonly either too twitchy (fallback engages on a single transient error, thrashes) or too loose (fallback engages only when the whole provider is already down for hours). Chapter 04's example numbers — 2 consecutive 5xx within 60s, 10-minute cooldown — are a reasonable starting point for most features; adjust to the feature's error-tolerance profile.
- Section 6 is the section procurement and security reviewers care about. If any parity axis fails, the fallback is a *theoretical* SPOF-shield that fails the moment it engages — this is worse than no fallback (it is a latent compliance issue dressed as a reliability feature).
- Section 7 should pre-date the entries, not just say "quarterly". A tabletop on a date 90 days from now is a calendar entry; "we'll do a tabletop at some point" is not.
- If your feature has no production traffic yet (pre-launch), write the plan as it would engage at launch; the first tabletop is scheduled for launch week. Explicitly state the pre-launch posture; don't fudge it.
- If no abstraction layer exists today, the plan's immediate action is to introduce one. The fallback plan cannot work without it. Write the introducing-the-abstraction as the first step in section 4's "plan to get there".

## Acceptance criteria

- One-to-two page plan with all seven sections present.
- Fallback shape(s) named with justification and quality-delta evidence.
- Routing policy named; routing-quality eval named where cost-first or capability-first routing is used.
- Abstraction layer picked, not "we'll figure it out".
- Triggers have specific numbers; restore conditions are explicit.
- Data-posture parity confirmed on all four axes (training-on-data, residency, sub-processors, enterprise features).
- Tabletop and restore drill have dates or owner-and-deadline.
- Primary/fallback pair matches exercise 01's selection memo.
- A peer AI Engineering Manager can read the plan and tell you (a) one thing they would push back on, or (b) nothing, this is signable.

## Stretch goals

- **Incident runbook draft.** Write the one-page incident runbook mod-107 will build on: observable signals (which dashboard to check), decision criteria (when to engage, when to escalate, when to hard-offline), rollback procedure, communication plan (who to tell: eng leadership, PM, support, customer success). The fallback plan is the system; the runbook is on-call's cheat sheet.
- **Second-feature contrast.** Write the fallback and routing plan for a second feature on the same product with a different shape (one interactive, one batch). The two plans should differ on routing policy, trigger thresholds, and tabletop frequency. Write a half-page memo on which parts of the plan generalise across features and which must be feature-specific.
- **Tabletop script.** Write a one-page tabletop script: the scenario narration, the questions to ask attendees at each step, the artefacts to open live (inventory, selection memo, runbook, status pages). The output of running this tabletop *is* a draft of mod-107's incident-response playbook for the feature.
- **Cost-of-fallback model.** For the cross-provider fallback specifically, write a half-page cost model: what the fallback costs per call relative to the primary, and the breakeven point at which continuous use of the fallback would prompt renegotiation of the primary contract. This is the artefact that makes "the fallback was engaged for 6 hours last Tuesday" into a quarterly review item instead of a one-off.
