# Exercise 04: IC AI-Assist Stack Rollout Plan

**Estimated effort:** 2 hours · **Output:** a written IC AI-assist stack commission document for one team covering the workload read, the mix, the data-flow policy, the measurement plan, and the rollout plan.

## Objective

Produce the fourth manager-owned artefact of this module: the commissioned IC AI-assist stack specified in chapter 05 (R14 in the role matrix) — the written choice of which assistant(s) the team uses, under what data-flow policy, measured against what productivity read, rolled out on what plan, and audited on what cadence.

A peer manager reading your commission document should be able to tell which assistants are on the stack, what code-sensitivity classification applies to each part of the codebase, what the vendor commitments are, how productivity is being measured, and when the next audit is — without asking you.

## Prerequisites

- Chapter 05 of this module for the full commission structure (workload read, mix, data-flow policy, measurement plan, rollout).
- The outputs of exercises 01 (shape decision) and 03 (triage cadence) — the stack commission reads against the shape and will produce signals into the triage cadence.
- Mod-106 chapter 06 (or equivalent) for the broader AI data-handling and release-triad substrate the data-flow policy lives inside.
- Mod-108 chapter 05 (anti-atrophy against frontier-assistant over-reliance) for the honest-cadence reading the commission's measurement plan integrates with.
- The current empirical literature cited in chapter 05 and in resources.md — Peng et al. 2023, Dell'Acqua et al. 2023, Microsoft Research 2024, METR 2025 — read in enough depth to defend a measurement plan against audit.
- Your organisation's People / HR and Legal partners, named — the data-flow policy is a joint document, not an engineering-only artefact.
- Your organisation's current policy of record on AI tool use (if one exists), named.

## Problem statement

A published IC AI-assist stack commission, in markdown, in your team's engineering handbook (with the data-flow policy co-signed by People / Legal). The document has six parts.

### Part A — the header and sign-off block (5 minutes)

- Document version, date, author (you), the staff-plus IC partner who reviewed the technical side, the People / Legal partner who co-signed the data-flow policy, and the next scheduled re-read date.
- A one-paragraph preface stating the commission's purpose, the team it applies to, and the deferral to `ai-assisted-software-engineer` (level 20) for IC-level craft.

### Part B — the workload read (20 minutes)

A section stating which assistant shapes fit the team's dominant work.

- A short enumeration of the team's work types (short-lived frontend components, backend refactoring, large legacy navigation, workflow automation / ticket closing, research prototyping, agentic subsystem development — adapt to your team).
- For each work type, which of the chapter-05 assistant shapes (completion, chat, edit, agent) is a natural fit, and which specific product in the current field is a candidate (Cursor, Claude Code, Copilot / Workspace, Windsurf, Cody, Sweep).
- A short defence of why the workload read drives the mix in Part C rather than vendor preference or marketing resonance.

### Part C — the mix (15 minutes)

- **The primary.** The assistant the team is expected to use by default. One, named, with the enterprise tier pinned.
- **The secondaries.** One or two allowed and supported alternatives. For each, the use case it covers (e.g., "Claude Code as secondary for terminal-native and agentic workflow work").
- **The explicitly-not-supported.** Any product the team is explicitly not to use (beta assistants, personal-tier subscriptions, self-hosted assistants that have not been vetted). Name why each is excluded.
- **The refresh cadence.** How often the stack choice is re-opened. Annual is a common default; quarterly is appropriate in a fast-moving period.

### Part D — the data-flow policy (30 minutes)

The joint People / Legal + Engineering document. The four layers chapter 05 names.

1. **Code sensitivity classification.** The classifications relevant to your codebase (open-source / public, internal product, security-critical, customer-data-touching, regulated). Name which parts of your codebase fall into which; where possible, name the directory or repo boundaries.
2. **Assistant allow-list per classification.** Positive list: which assistants are allowed on which classifications. Anything not on the list is off.
3. **Vendor commitments per allowed assistant.** For each allowed assistant on the enterprise tier: whether the vendor trains on the team's code (no, by commitment), retention period, audit log availability, SOC 2 / ISO 27001 status, data-residency commitments if required. Pinned as of the commission date; the quarterly audit re-verifies.
4. **Audit cadence.** The schedule for the quarterly audit of the policy (terms-of-service re-read against each vendor, PR-sampling for assistant-involvement signals, immediate review on any new-assistant proposal from an IC).

If your organisation already has a broader AI-tool policy of record (common in regulated industries or at larger scale), name it, cite it, and specify how this team-level policy inherits from and complements it.

### Part E — the measurement plan (20 minutes)

The three honest signals chapter 05 names.

- **Specifics self-report.** The short, quarterly, named-task-level survey the team will run. Three to five questions; written out. The survey is not Net-Promoter or thumbs-up/down; it is task-level specific ("What specific tasks did the assistant make you faster on this quarter? Slower on? Where did it produce a bug you had to catch?").
- **Code-review signal.** The sampling discipline. How many PRs per quarter, from which ICs, read by whom, against what rubric. The rubric reads qualitatively against the honest-cadence split (mod-108 chapter 03): is the craft growing, or is the IC relying on the assistant without leading?
- **Narrow bake-off (if any).** If there is a specific question the team is answering about the stack (should we switch the primary? should we add a secondary? is the agent mode worth the context switch?), the two-week narrow bake-off design: participants, task class, acceptance criteria, time-box.

Also name one dishonest-measurement pattern chapter 05 warns against that the team is actively refusing to rely on (the vendor-sourced top-line "developer productivity" number; raw PR-throughput without a quality read; averaged self-report without triangulation).

### Part F — the rollout plan (20 minutes)

The thirty-/ninety-day beats chapter 05 names, adapted to your team.

- **Week 0.** What is published, by whom, cleared with whom (People, Legal, the next-line-manager, the staff-plus IC partner).
- **Week 1.** Installation, enterprise-tier enrolment, the kickoff meeting (30-minute policy walk-through, not a tool tutorial — the IC-level craft is level-20 territory).
- **Weeks 2–4.** Settle. The weekly 5-minute "how is the stack" check as part of the existing retrospective.
- **Day 30.** The thirty-day check-in. Agenda, attendees, output.
- **Weeks 5–12.** Operate. The triage and incident-replay signals that feed back into the commission (did the stack produce a bug we had to catch? did a data-flow violation occur?).
- **Day 90.** The ninety-day read. The first honest productivity read: survey, code-review sample, bake-off result if any. Decision point: continue, adjust, tighten, loosen.
- **Quarterly thereafter.** The re-read cadence becomes quarterly.

## Requirements

- The document is a single markdown file in your team's engineering handbook, co-signed by the People / Legal partner.
- The document has a version, date, author, technical reviewer, policy co-signer, and next-re-read date.
- Part B names the workload read with specific work types and assistant-shape fits.
- Part C names a primary, secondaries, explicitly-not-supported list, and refresh cadence.
- Part D has all four layers of the data-flow policy filled with specifics relevant to your codebase and your organisation.
- Part E has three honest-measurement signals named with specifics (not placeholders), and one dishonest-pattern the plan refuses.
- Part F has specific calendar dates on the thirty-day and ninety-day beats on the manager's calendar at the time of publication.
- The deferral to `ai-assisted-software-engineer` (level 20) is explicit in the preface; the document does not teach IC-level assistant craft inline.
- Length: 2,000–3,500 words.

## Starter guidance

- Write Part D (data-flow policy) first and co-author it with the People / Legal partner from the beginning. Writing the policy in engineering and then "sending it to Legal for review" is slower and produces a weaker artefact than co-authoring. The co-author version lands in days; the review version takes weeks and often returns with surprises.
- The vendor commitments in Part D sub-layer 3 require reading the current enterprise-tier terms of service for each vendor on your allow-list. The terms change on a near-quarterly cadence. Pin the terms as-of the commission date and schedule the quarterly audit explicitly.
- For Part E, the hardest work is the self-report survey design. A good survey takes the IC under five minutes and produces specifics the manager can read; a bad survey takes fifteen minutes and produces aggregate sentiment the manager cannot act on. Three to five task-level questions is the shape; test it on one IC before publishing.
- If your organisation does not yet have a broader AI-tool policy of record and you are being asked to write one as a side-effect of this exercise, flag it to People / Legal explicitly. A team-level commission is appropriate; writing company-wide policy is out of scope for one engineering manager's ninety-minute document and should be escalated.
- Resist the urge to include an assistant-use training in Part F. The IC-level craft is level-20 territory; the manager-side onboarding is a policy walk-through, not a tool tutorial. If the team genuinely needs IC-side training on prompting, agent modes, or escape hatches, point to the level-20 role sibling material or to the vendor's own training (which some ICs will naturally find more useful than others).
- For the measurement plan, read at least one of the primary empirical sources in resources.md before writing (Peng et al. 2023 is the shortest; Dell'Acqua et al. 2023 is the most memorable; METR 2025 is the most recent). The plan has to survive an audit against a senior IC who has read the literature; the plan that reads "the assistant makes us faster" without triangulation does not.
- Ask the staff-plus IC partner to read Part B (workload) and Part C (mix). Their read on whether the stack fits the actual work is the technical acceptance test; a stack chosen for a workload the team does not have is a commission waste.

## Acceptance criteria

- The document is in your team's engineering handbook, linked from your submission; the data-flow policy is co-signed by People / Legal.
- Parts A–F are complete with the specifics the chapter-05 structure names.
- The vendor commitments in Part D are pinned as-of the commission date and link to the current enterprise-tier terms.
- The measurement plan refuses at least one dishonest pattern explicitly and triangulates across self-report + code-review + (optionally) bake-off.
- The rollout plan has specific thirty-day and ninety-day dates on the manager's calendar.
- The deferral to `ai-assisted-software-engineer` (level 20) is explicit in the preface.
- A peer manager can read the commission and tell what is on the stack, what the policy is, and when the next audit is within five minutes.
- Length is 2,000–3,500 words.

## Stretch goals

- **The retroactive audit.** Before publishing the new commission, audit the current informal state: which assistants are actually in use on the team, under which tier (enterprise or personal), against which code classifications. The delta between the current state and the new commission is the rollout's actual work.
- **The thirty-day check-in script.** Write the agenda and the question-set for the thirty-day check-in meeting. The script forces specificity: a check-in without a script tends to collapse into "how is it going?" and produces nothing.
- **The ninety-day read template.** Write the structured note the ninety-day read will produce — a one-page template covering the survey results, the code-review read, the bake-off result if any, and the decision (continue / adjust / tighten / loosen). The template is the forcing function; writing it now means the ninety-day meeting produces a decision rather than a discussion.
- **The data-flow incident playbook.** If an IC pastes sensitive code into a non-allow-listed assistant, or uses a personal-tier account on company code, what is the response? Write the incident-response short playbook — who is notified, what is documented, what remediation is required, what the follow-up with the IC looks like. The playbook is the artefact the manager reaches for when the quarterly audit surfaces a violation.
- **The cross-team commission comparison.** If your organisation has multiple AI engineering teams, ask each manager for their commission (or write a proposed shared one). The cross-team comparison surfaces whether the data-flow policy is consistent (it probably should be, at the organisation level); whether the stack choices diverge for workload reasons (they probably should, per team); and whether the measurement discipline is comparable (it probably should be, to allow cross-team calibration).
- **The level-20 role-sibling handoff.** Write the one-page onboarding document a new IC joining the team receives, covering (a) what is on the stack, (b) where the data-flow policy lives, (c) where to find the level-20 role sibling material on IC-level craft. The document is the artefact that makes the deferral operational rather than theoretical.
