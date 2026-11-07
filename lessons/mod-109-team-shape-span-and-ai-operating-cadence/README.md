# mod-109: Team Shape, Span, and the AI Engineering Operating Cadence

> Ninth module of the AI Engineering Manager track. Builds the manager-owned team-design and operating-rhythm layer for an AI engineering team: how to choose the team's shape (applied + eval + fine-tune split; vertical product-aligned pod vs. horizontal capability team; with or without an embedded PM); the AI-specific ritual set a working team runs on (weekly model-watch, prompt + eval + trace review, incident replay, vendor-provider release review) and the discipline of eliminating rituals that do not earn their place; the backlog + bug + experiment triage cadence a non-deterministic system demands (regression classification by cause, experiment-work sizing); the commissioned, honestly-measured IC AI-assist stack (R14 responsibility — Cursor, Claude Code, GitHub Copilot / Workspace, Windsurf, Cody, Sweep) with the team's data-flow policy; and the honest span-of-control read at a level-45 engineering-management altitude — when to add a tech-lead or staff-plus IC partner, when to split the pod, when to coach a senior IC into the first-time-manager transition. General team-topology, agile-ritual-mechanics, and basic span-of-control canon defer to `ai-augmented-team-manager` (level 40) and are this module's prerequisite, not its content.

**Estimated effort:** 12 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Choose team shape — applied + eval + fine-tune split, vertical product-aligned pod vs. horizontal capability team, with or without an embedded PM — using Larson's team-sizing framework specialised for AI engineering workloads (eval + prompt + trace work, incident surface, model-provider volatility).
- Design the AI-specific ritual set — a weekly model-watch meeting, a prompt + eval + trace review, an incident replay, a vendor-provider release review — and run the discipline that kills the rituals that do not earn their place.
- Set the backlog, bug, and experiment triage cadence appropriate for non-deterministic systems — regression classified by cause (prompt vs. judge vs. retrieval vs. tool vs. model-vendor), experiment work sized and bounded against capacity.
- Commission the IC-level AI-assist stack for the team (R14): choose the mix from the current field (Cursor, Claude Code, GitHub Copilot / Workspace, Windsurf, Cody, Sweep), roll it out, measure productivity honestly against the current empirical literature (Peng et al. 2023, Dell'Acqua et al. 2023, Microsoft Research 2024, METR 2025), set the data-flow policy (what code and data may go to which assistant), and defer IC-level craft down to `ai-assisted-software-engineer` (level 20).
- Operate span-of-control honestly at a level-45 engineering-management altitude — when to add a tech-lead or staff-plus IC partner, when to split into two pods, when to coach a senior IC into the first-time-manager transition.

## Chapters

1. [The Manager's Altitude for Team Shape and Operating Cadence](01-the-managers-altitude-for-team-shape-and-operating-cadence.md) — what the manager owns (the four manager-owned artefacts) vs. what defers to the IC, to People / HR, to the staff-plus ICs, and to the general-management peer; what this module does not re-teach.
2. [Choosing Team Shape: Mix, Alignment, and the Embedded PM Question](02-choosing-team-shape-mix-alignment-and-the-embedded-pm-question.md) — team-sizing specialised for AI engineering workloads; the applied / eval / fine-tune split; vertical product-aligned pod vs. horizontal capability team; with or without an embedded PM; two worked team-shape decisions.
3. [The AI-Specific Ritual Set, and the Rituals That Must Die](03-the-ai-specific-ritual-set-and-the-rituals-that-must-die.md) — the four rituals that earn their place on an AI engineering team (model-watch, prompt + eval + trace review, incident replay, vendor-provider release review); the ritual-cost ledger; the kill-criteria for a ritual that does not earn; a worked quarterly ritual audit.
4. [Backlog, Bug, and Experiment Triage for Non-Deterministic Systems](04-backlog-bug-and-experiment-triage-for-non-deterministic-systems.md) — the regression-by-cause taxonomy (prompt / judge / retrieval / tool / model-vendor); the triage cadence; the experiment-work sizing contract; the three failure shapes a classical triage produces on this surface and the AI-specific fixes.
5. [Commissioning the IC AI-Assist Stack (R14)](05-commissioning-the-ic-ai-assist-stack-r14.md) — the current assistant field (Cursor, Claude Code, Copilot / Workspace, Windsurf, Cody, Sweep); the honest productivity-measurement discipline against Peng et al. / Dell'Acqua et al. / Microsoft Research / METR; the data-flow policy the team actually needs; the rollout plan; the deferral down to `ai-assisted-software-engineer` (level 20) for IC craft.
6. [Span of Control at Level 45: When to Add a Partner, When to Split, When to Grow a Manager](06-span-of-control-at-level-45.md) — reading span honestly at the L45 altitude; the three signal patterns (partner-needed, split-needed, manager-to-grow); the staff-plus IC partner vs. tech-lead distinction; the first-time-manager coaching contract; the anti-patterns.

## Exercises

- [Exercise 01 — Team Shape and Span Decision for One Team](exercises/exercise-01-team-shape-and-span-decision-for-one-team.md) *(2 h)* — produce a written team-shape decision for one real (or realistic composite) team covering mix, alignment, embedded PM, and the span read.
- [Exercise 02 — AI-Specific Ritual Set Design](exercises/exercise-02-ai-specific-ritual-set-design.md) *(2 h)* — design the ritual set for one team, with a ritual-cost ledger, kill-criteria, and a first-quarter audit plan.
- [Exercise 03 — Backlog, Bug, and Experiment Triage Cadence](exercises/exercise-03-backlog-bug-experiment-triage-cadence.md) *(2 h)* — specify the triage cadence for one team, including the regression-by-cause taxonomy, experiment sizing, and the triage-meeting shape.
- [Exercise 04 — IC AI-Assist Stack Rollout Plan](exercises/exercise-04-ic-ai-assist-stack-rollout-plan.md) *(2 h)* — commission the assistant stack for one team with an honest measurement plan and a data-flow policy.

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Depends on `ai-augmented-team-manager` (level 40).** The general management canon on team topology (Skelton & Pais), agile-ritual mechanics (Scrum, Kanban, the standup / retro / planning triad), classical span-of-control reading, and the first-time-manager coaching playbook is this module's prerequisite, not its content. This module specialises it to the AI engineering surface.
- **Builds on mod-101 (role ladder and operating contract).** The six AI-engineering IC shapes and the two staff-plus IC shapes mod-101 names are the roster this module composes into teams; the handoff contracts are the substrate the rituals and triage cadence are tuned against.
- **Pairs with mod-102 (hiring, leveling, interview loops).** The mix decision (chapter 02) drives the open-role list mod-102's hiring loop runs against; the span read (chapter 06) is one of the inputs to the hiring-plan sequencing mod-102 chapter 03 produces.
- **Pairs with mod-103 (roadmapping non-deterministic work).** The experiment-work sizing contract (chapter 04) reads against the confidence-banded planning mod-103 produces; the backlog triage cadence is the operational layer of mod-103's roadmap discipline.
- **Pairs with mod-104 (eval-driven SDLC and release gates).** The prompt + eval + trace review ritual (chapter 03) reads directly against the registry and release gates mod-104 builds; the regression-by-cause triage (chapter 04) depends on the trace surface mod-104 produces.
- **Pairs with mod-105 (vendor and model portfolio).** The weekly model-watch ritual (chapter 03) and the vendor-provider release review are the operating-cadence beats of the portfolio discipline mod-105 teaches; the model-vendor regression class (chapter 04) reads against the vendor-landscape surface mod-105 names.
- **Pairs with mod-106 (safety and the release triad).** The incident-replay ritual (chapter 03) and the on-call surface implied by the span read (chapter 06) are grown on the release-triad discipline mod-106 builds.
- **Pairs with mod-107 (AI-specific incident response and postmortems).** The incident-replay ritual is the operating-cadence beat of mod-107's blameless postmortem practice; the incident-to-fixture loop is the feedback mechanism this module's rituals sequence.
- **Pairs with mod-108 (performance management and growth for AI engineers).** The deliberate-practice slot, the reading loop, the rotation loop, the on-call rotation, the 1:1 spine, and the calibration meeting mod-108 names are beats the cadence this module designs sequences into a whole-team rhythm; mod-108 names the beats, mod-109 schedules them.
- **Pairs with mod-110 (budget, FinOps, build vs. buy).** The team-shape mix (chapter 02) and the IC AI-assist stack commission (chapter 05) are budget line items; the team-shape decision has a cost side mod-110 teaches the manager to read.
- **Pairs with mod-111 (cross-functional partnership).** The embedded-PM decision (chapter 02) and the cross-functional surface the ritual set touches (chapter 03) read against the partnership shapes mod-111 teaches.
- **Pairs with mod-112 (executive communication).** The ritual outputs (model-watch note, release-review minutes, incident replay) and the span read feed the executive-level read-outs mod-112 teaches the manager to compose.
- **Defers down to `ai-assisted-software-engineer` (level 20).** Chapter 05 commissions the assistant stack and sets the team policy; the IC-level craft of using the assistant well — prompting it, ignoring it when it is wrong, writing the escape hatches that keep the engineer in charge of the judgment — is taught in the level-20 role sibling, not here.
