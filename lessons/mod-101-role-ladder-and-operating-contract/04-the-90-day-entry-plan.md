# The 90-Day Entry Plan

The first 90 days as an AI Engineering Manager are different from the first 90 days as a backend engineering manager, and different from the first 90 days as an IC AI engineer. You are inheriting a team whose work is probabilistic, whose dependencies include third-party frontier labs, and whose release gate is evidence-based rather than calendar-based. This chapter lays out a concrete, week-by-week plan to take over an existing team without breaking it.

## Motivation

A good entry plan does three things at once:

- **Protects the team** — you listen more than you intervene; you don't rewrite the operating system in week two.
- **Protects the business** — you have a defensible read on risk by the end of the quarter: what could break, what the plan is if it does, what your team can promise.
- **Protects you** — you have evidence, by day 90, to defend the hard calls you will start making in day 91.

The common anti-patterns:

- The new manager who rewrites standups in week one.
- The new manager who has no written read on the team's eval coverage by the 30-day mark and discovers at day 45 that there is almost none.
- The new manager who hasn't talked to their nearest counterparts in governance, infra, product, and security by the 45-day mark, then lands in an incident and doesn't know whom to call.

Avoiding each of those is a check on the plan below.

## The plan at a glance

| Phase | Duration | Primary mode | What you produce |
|---|---|---|---|
| 1. Listen | Weeks 1–2 (days 1–14) | Observe, read, 1:1 | Named stakeholder tour, first-pass team read |
| 2. Inventory | Weeks 3–6 (days 15–45) | Audit | Four written inventories + first small commitments |
| 3. Commit | Weeks 7–13 (days 46–90) | Decide, align, start | One operating-cadence change, one release-gate change, one roadmap ask |

Each phase has concrete deliverables. Each deliverable is something you can hand to your manager, your staff IC, and (where relevant) your governance and infra peers.

## Phase 1 — Weeks 1–2: Listen

The temptation to intervene is highest right now and you have the least evidence to justify it. Hold.

### Objectives

- First 1:1 with every direct report.
- First read of the last three postmortems (if they exist).
- Named stakeholder tour: at least one 30-minute conversation with each of the following roles (where they exist in your org) — product lead, staff-plus IC partner, infra-team-lead peer, governance / risk peer, CAIO or whoever holds that altitude, your direct manager.
- Read-only access to the prompt registry, eval reports, incident log, model-provider contracts, and the current roadmap.

### What to listen for

In every 1:1, ask the same three questions and record the answers:

1. "What is the one thing you wish the previous manager had held the line on?"
2. "What is the one thing you're most worried about on this team in the next 90 days?"
3. "What should I not change in the first quarter?"

The pattern of answers becomes your month-one prioritisation.

### Trap to avoid

Don't promise anything in week one. "Let me understand before I commit" is both honest and defensible. The exception is operational safety: if someone says, in a 1:1, that there is an active jailbreak path or a compliance issue, you escalate it that day — the escalation itself is not a "change," it's your job.

## Phase 2 — Weeks 3–6: Inventory

Four written inventories, each one page. Each one answers a specific question and names the gap you will address later.

### Inventory 1 — Team shape

The question: *does the team I have match the work on the plate?*

- Role mix: how many applied-ai-engineers, agentic engineers, eval engineers, RAG engineers, fine-tuning engineers, LLM developers, staff-plus / senior ICs. Pulled from HR/level data, not guessed.
- Open roles and their time-to-fill history.
- Attrition risk read, pulled from your 1:1 notes.
- Span of control: are you managing more than your altitude supports? Under 7 or over 10 directs, specifically, are flags (per Larson, *An Elegant Puzzle*).
- Gap statement: one sentence per gap, naming whether you fill it by hiring, re-shaping, or coaching.

### Inventory 2 — Model and vendor inventory

The question: *what does the team depend on that is outside its control?*

- Every model in production, by provider, version, task, and estimated cost per month.
- Every provider the team has an account with, and the contract status (standard terms, signed DPA, enterprise agreement).
- Known deprecations and their deadlines. Scan each provider's release notes and deprecation pages — Anthropic, OpenAI, Google, Mistral — for every model the team depends on.
- Fallback posture: for each production model, is there a defined fallback if it rate-limits, deprecates, or regresses? Yes / no / partial.
- Gap statement: one row per model naming the highest-severity risk.

### Inventory 3 — Eval and incident inventory

The question: *what do we actually know about whether the product is working?*

- Every registered eval suite: what it covers, how often it runs, what gates (PR / canary / release / production).
- Every golden dataset: where it lives, how it's refreshed, who owns it.
- Every judge in use: LLM-as-judge prompts, rubric judges, human-review workflows.
- Every incident from the last two quarters, categorised: jailbreak, prompt injection, hallucination, post-upgrade regression, judge drift, tool-use failure, retrieval degradation, outage, moderation false-positive storm, other.
- For each incident: was a permanent eval fixture added? (Yes / no.)
- Gap statement: name the three evals that should exist and do not.

### Inventory 4 — Prompt and judge registry audit

The question: *are our prompts and judges first-class artefacts, or ambient knowledge?*

- Where every production prompt lives (registry, repo, Google Doc, hard-coded).
- Prompt versioning posture: can you reproduce the prompt that shipped with a given release? Yes / no.
- Judge registry posture: is there a canonical list of judges with change history?
- PR hygiene: does a prompt-change PR require a trace sample and a judge delta, or is it a one-line text change?
- Gap statement: one sentence on where the registry needs to be in 90 days.

### Stakeholder tour, second pass

By end of week 6, close the loop on each stakeholder conversation from phase 1 with a one-paragraph read of what you've found. Not a plan yet — a *read*. The read is the ground-truth input to any later commitment.

### Deliverable set

Four inventories (one page each), four gap statements, four one-sentence first-move proposals. You are not yet executing the moves. You are preparing to defend them.

## Phase 3 — Weeks 7–13: Commit

Three commitments. No more. Pick them from your inventories, not from the eval-of-the-month press release.

### Commitment 1 — One operating-cadence change

Change exactly one meeting, ritual, or review in the team's operating cadence. Candidates, grounded in modules 108 and 109:

- Add a weekly 20-minute model-watch — scan provider release notes, triage what matters.
- Add a biweekly eval review — walk the last 10 PRs and their eval-delta reports.
- Collapse a ritual that is not earning its place.

Pick one. Announce it with a reason. Review it after four weeks.

### Commitment 2 — One release-gate change

Change exactly one thing about how the team releases. Candidates:

- Make prompt + trace + judge review a code-review requirement for PRs touching the prompt registry.
- Add an eval-delta threshold that gates canary → GA (not just logs it).
- Require that every incident of the last quarter has an eval fixture before the next major release.

Pick one. Document it in the release-gate policy. Enforce it from a specific date.

### Commitment 3 — One roadmap ask

Take one defensible ask to your manager and (if applicable) the CAIO. Candidates:

- Headcount: a role the team is missing (eval engineer, senior agentic engineer, staff-plus partner).
- Budget: observability tooling, labelling program, red-team engagement.
- Scope: deferring a feature your team should not be building.

Each ask is defended with the evidence from your inventories.

### The 90-day review

At day 90, write a one-page document with the following sections, and share it with your manager and staff IC partner:

- What I heard in Phase 1. (Patterns, not individuals.)
- What I found in Phase 2. (The four inventories, summarised.)
- What I committed to in Phase 3. (The three changes and the asks.)
- What I'm watching in days 91–180. (Risks and leading indicators.)
- What I need from this leadership group to be successful. (One or two items, specific.)

This document becomes the artefact your manager uses to evaluate you at the six-month mark, and the artefact you use to hold yourself accountable.

## What this plan deliberately does not do

- No reorg in the first 90 days unless you inherit one already in flight, or you discover a safety-level issue. Reorgs started on thin evidence don't stabilise.
- No firing in the first 90 days unless an active performance process is already in motion. You do not have the context yet to make the call.
- No wholesale tooling change. One cadence change, one release-gate change, one roadmap ask — that is the budget.
- No new strategy published externally. The CAIO owns strategy; your job in the first 90 days is to feed them the ground truth that shapes it.

## A concrete read: one 30-day artefact

Here's what Inventory 2 (model and vendor inventory) might look like at 30 days, in schematic form (filled with placeholders — do *not* fabricate real numbers in your own version until you have them):

```
MODEL & VENDOR INVENTORY — 2026-12-05
Team: Agents Team · Manager: [you]

Provider A — enterprise agreement signed, DPA current
  - model-A-flagship, used by: Feature 1, Feature 2
      est. monthly spend: [measure before quoting]
      deprecation: none announced
      fallback: Provider B flagship, tested, documented
  - model-A-mini, used by: internal tools
      deprecation: announced, sunset 2027-02-28  ← HIGH-RISK
      fallback: none defined  ← GAP #1

Provider B — standard terms, no signed DPA
  - model-B-flagship, used by: Feature 1 fallback path
      status: tested quarterly, last tested 2026-10
      DPA gap  ← GAP #2 (route to legal + procurement)

Open-weights — self-hosted
  - [name], used by: classifier / guardrail
      serving owner: ai-infra-team-lead
      shared SLO: p95 latency <= Xms (confirm)

GAP STATEMENT
  G1: model-A-mini has no fallback and 85 days to deprecation. Action: scope fallback by day 45.
  G2: Provider B carries traffic without a signed DPA. Action: route to legal by day 35.
```

That's one page. Produced in week four. Enough to walk into a cross-functional with evidence rather than impressions.

## Summary

- The 90-day plan has three phases: Listen (weeks 1–2), Inventory (weeks 3–6), Commit (weeks 7–13).
- Listen phase produces a stakeholder tour and first-pass team read — no commitments.
- Inventory phase produces four one-page documents: team shape, model + vendor, eval + incident, prompt + judge registry.
- Commit phase produces three changes: one cadence change, one release-gate change, one roadmap ask.
- At day 90 you ship one page summarising what you heard, found, committed to, are watching, and need.
- Everything you avoid doing — reorg, firing on thin evidence, wholesale tooling change, externally-visible strategy — protects both the team and your own credibility in the second quarter.

This chapter closes the module. The exercises walk you through producing the three artefacts this module teaches: the ladder diagram and deferral contract (chapter 01 + 03), the canon-crosswalk (chapter 02), and the 90-day plan for one real team (chapter 04).
