# Commissioning the IC AI-Assist Stack (R14)

The fourth manager-owned artefact of this module is the team's IC AI-assist stack: the written, intentional choice of which coding assistant(s) the team uses, under what data-flow policy, measured against what productivity read, and audited on what cadence. The AI Engineering Manager's responsibility here is R14 in the track's role matrix — commission the stack at the team level, set the policy, measure honestly, defer the IC-level craft of using the assistant well to `ai-assisted-software-engineer` (level 20). The manager chooses the instruments; the IC plays them.

This chapter walks the commission. The field of assistants is moving; the chapter names the current field at the time of writing (late 2026) but focuses on the *decision discipline*, which will outlast the specific products.

## Motivation — three stack failures that read as engineering-productivity failures

Three failures the honest commission prevents.

- **The "everyone picks their own" stack with no measurement and no policy.** Each IC installed the assistant they heard about: half the team on Cursor, a quarter on Claude Code, a quarter on nothing in particular, one engineer on a self-hosted Ollama + Cody setup. Six months in, promotion velocity reads differently between the two halves of the team; nobody can tell whether the difference is craft, assistant leverage, or self-report bias; and the team's codebase is leaving in prompts to four different vendors whose data-handling policies nobody has read. The problem is not the engineers; the manager never commissioned.
- **The vendor-mandated stack with no honest measurement.** The parent organisation mandated a single assistant (Copilot, say). The executive deck claims "the team is 30% more productive." The manager reads the report; the methodology is self-report, taken three weeks after the rollout, with the vendor providing the survey. The claim is probably wrong — Peng et al. 2023 and Dell'Acqua et al. 2023 both surfaced that frontier-assistant effects are uneven across task type, experience level, and population — and the local reality is that two senior ICs are visibly slower on complex refactoring work (consistent with the METR 2025 finding on experienced-OSS-developers-on-complex-tasks) while three juniors are visibly faster on boilerplate. The honest read is not "the team is 30% more productive"; it is "the stack is helping some engineers on some tasks and hurting others on others, and we should know which." The failure is not the mandate; the failure is the measurement being imported without local verification.
- **The policy-of-default where sensitive code leaks.** The team is on Cursor with enterprise settings, but one of the ICs has a personal Pro subscription they use to edit PRs on a company laptop at home. Another ran a quick refactor on Replit Agent using a code snippet pasted in. A third tried an evaluation with a beta assistant whose terms of service nobody read. The team's codebase is leaving the company in ways nobody has consented to. Nothing is on fire today; the regulatory read-out next quarter will find the breadcrumbs.

All three failures are policy, measurement, or commissioning failures — not engineer failures. The IC-level craft is level-20 territory; the manager's job is to make the commission honest.

## Core concept 1 — the current assistant field (late 2026)

A snapshot, not an endorsement. The decision discipline below outlasts the snapshot; the specific products evolve on a near-quarterly cadence.

- **Cursor** ([cursor.com](https://cursor.com/)) — an IDE fork of VS Code with tight assistant integration, multi-model support, agentic edit modes, repo-level context. Adoption is wide on frontend and full-stack teams; Windows / Mac / Linux. Enterprise tier offers data-flow controls and audit.
- **Claude Code** ([claude.com/claude-code](https://www.claude.com/claude-code)) — Anthropic's terminal-native coding assistant and SDK; multi-agent harness, persistent sessions, headless and interactive modes. Adoption stronger on back-end and infrastructure work; also serves as a CLI-native agentic harness for workflow automation.
- **GitHub Copilot** ([github.com/features/copilot](https://github.com/features/copilot)) — the longest-established of the current field; now split into Copilot (completion + chat) and Copilot Workspace (task-level agent). Ships with enterprise governance, SOC 2 certification, and audit logging.
- **Windsurf** ([codeium.com/windsurf](https://codeium.com/windsurf)) — originally Codeium's IDE; multi-model, agentic workflows, deep context. Comparable in shape to Cursor; different pricing and integration model.
- **Cody** ([sourcegraph.com/cody](https://sourcegraph.com/cody)) — Sourcegraph's assistant, with the signature advantage of code-graph context across large repos; strong in enterprise settings with large codebases.
- **Sweep** ([sweep.dev](https://sweep.dev/)) — issue-to-PR automation, heavier agentic workflow at the ticket-closing altitude.
- **Adjacent and specialist tools** — Replit Agent, Devin (Cognition), Aider, Continue, Tabby (self-hosted), Lovable, Bolt.new, v0. These shape-shift quickly; audit the current field on commission date.

Four axes separate the field on the current snapshot:

1. **Where the assistant lives.** IDE plugin (Copilot in VS Code / JetBrains), IDE fork (Cursor, Windsurf), terminal-native (Claude Code, Aider), browser (Replit Agent, Lovable), ticket-level / issue-level (Sweep, Copilot Workspace).
2. **Which models it uses.** Single-vendor (Copilot historically OpenAI-leaning; Claude Code Anthropic-leaning), multi-model (Cursor, Windsurf, Cody support multiple back-end models), self-hosted (Tabby, Continue, Cody local-model modes).
3. **The agentic altitude.** Completion (fills the next lines), chat (answers a question), edit (multi-file change), agent (completes a ticket or task with planning and tool use).
4. **The data-flow posture.** What leaves the laptop, where it goes, whether it is retained, whether it is used for training, what audit trail exists.

A commission picks against the four axes against the team's workload, the data policy, and the budget.

## Core concept 2 — the commissioning decision

A written commission has five sections.

### 1. The workload read

What the team is being asked to do on a day. Short-lived frontend components (completion + chat win); long-lived backend refactors (edit-level agent, repo-level context win); large legacy codebase navigation (Cody's code-graph advantage); workflow-automation / ticket-closing (Sweep or Copilot Workspace); specialist research-flavoured prototyping (Claude Code or Aider in terminal, or Cursor's agent mode). The commission picks the stack that matches the dominant workload; it does not pick the stack that reads loudest on LinkedIn.

### 2. The mix

Rarely one assistant; typically a primary and a secondary. A reasonable vertical-pod commission might be: Cursor as the primary IDE assistant, Claude Code as the secondary terminal/agentic tool, GitHub Copilot available on the IDE side for engineers who prefer it. The commission names the primary (what the team is expected to use by default), the secondaries (allowed and supported), and the explicitly-not-supported (nobody runs Replit Agent against this codebase from a personal account; nobody pastes code into a beta assistant whose ToS has not been read).

### 3. The data-flow policy

The policy of record. Four questions it answers.

- **Which code can leave the laptop, to which assistant?** Not all code is equal; some teams work on regulated data pipelines or sensitive security-critical code where even the vendor-reviewed enterprise tier is off-limits. The policy names the categories (open source, product code, security-critical, customer-data-touching, regulated) and the assistant allowed per category.
- **Which data can the assistant see in context?** The IDE-side assistants ingest the open files; if a customer-data CSV is open, it may be sent. The policy names the file-system boundaries; the IC's job (level-20 craft) is to maintain them.
- **Where does the assistant send the data and what is the retention?** Enterprise tiers on Cursor, Copilot, Claude Code, Cody, Windsurf offer varying combinations of no-training, short retention, audit logging. The policy pins the tier and the vendor commitment in writing; the commission is signed by a manager on the policy side (People / Legal) and the manager on the engineering side.
- **What is the audit trail and the review cadence?** Who checks that the policy is being followed. How often. What signal would flag a violation. A policy with no audit cadence is a policy of hope.

Chapter 06 of mod-106 (safety and the release triad) names the broader AI data-handling discipline; this chapter's data-flow policy is the IC-side instantiation.

### 4. The measurement plan

Honest productivity measurement is the hardest part of the commission. The current empirical literature (resources.md names the primary sources) is divergent. Peng et al. 2023 (GitHub Copilot RCT) found a 55% task-completion speedup on a specific JS task on a specific population. Dell'Acqua et al. 2023 (Harvard / BCG) found a "jagged frontier" — assistant-aided consultants outperformed on tasks inside the frontier, under-performed on tasks outside it. Microsoft Research 2024 (three field experiments) found task-level and experience-level heterogeneity. METR 2025 (experienced open-source developers on complex OSS tasks) found that developers *felt* faster with AI assistance but were measurably slower on the specific task class studied. The honest read from the literature: the effect varies by task, by experience level, by codebase, by task complexity, and by the specific assistant.

The manager's measurement plan has three honest readings and avoids one dishonest one.

- **Self-report with specifics.** A quarterly, short, named-task-level survey: "On what specific tasks did the assistant make you faster this quarter? Slower? Where did it produce a bug you had to catch?" Not a Net-Promoter number; not a "did the assistant help?" yes/no. Specifics the manager can read.
- **Code-review signal.** The manager and the senior IC read a sample of the team's PRs each quarter with the assistant's involvement visible (in Cursor and Claude Code, assistant edits can be tagged; in Copilot, less easily). The read is qualitative: is the craft growing? Are the PRs getting better-reasoned, or are they getting longer and more plausible-but-wrong? The honest-cadence split from mod-108 chapter 03 is the companion reading.
- **Narrow controlled bake-off.** For a specific question (does Cursor's agent mode help more than Claude Code's on our retrieval-pipeline refactoring work?), the team can run a two-week small-sample A/B — a handful of engineers on each, specific task class, written acceptance criteria. Limited by sample size; useful for a narrow answer.

The dishonest reading to avoid: a top-line "developer productivity" number from the vendor's own survey, extrapolated to a dollar savings. The literature is too divergent; the local effects too heterogeneous; the number will almost always be unfaithful to what is actually happening on the team.

### 5. The rollout and the deferral

The rollout is written: who is on the stack by when, what training is on offer (not much is needed for IDE-side; more for Claude Code and agentic modes), what the thirty-day check-in and ninety-day read look like. The deferral is the explicit note: *the IC-level craft of using the assistant well — prompting it, reading its output critically, knowing when to ignore it, the escape hatches that keep the IC in charge of the judgment — is taught by `ai-assisted-software-engineer` (level 20), not here.* The manager commissions; the IC role teaches the craft.

## Core concept 3 — the honest productivity-measurement discipline

The literature is the manager's grounding; the local data is the manager's responsibility. Three honest-measurement principles.

### Principle 1 — effects are jagged, not uniform

Dell'Acqua et al. 2023's "jagged frontier" is the shortest way to carry the finding. Inside the frontier (tasks the model is strong on), assistant users outperformed. Outside the frontier, they under-performed — partly because they could not tell they were outside. For the AI engineering manager: the stack will help on some tasks and hurt on others, and the ICs may not reliably know which is which. The measurement has to be task-level specific, not team-level aggregate.

### Principle 2 — experience moderates the effect, and not simply

Peng et al. 2023 found junior developers gained more than seniors on the completion task they studied. Microsoft Research 2024 and METR 2025 complicated the picture — seniors on complex, unfamiliar, or reasoning-heavy tasks sometimes feel faster but are measurably slower; juniors on simple, pattern-heavy tasks are reliably faster. The AI engineering manager's read: do not assume seniors are getting the biggest lift; do not assume juniors are the only beneficiaries; do assume the pattern on your team may not match any single study.

### Principle 3 — self-report drifts positive

Several studies in the current literature surface a self-report / measurement gap. The engineer *feels* faster; the task takes longer. Without a controlled or empirical read, self-report is systematically rosy. The measurement plan has to include at least one non-self-report signal (code-review read, PR latency time-series, bug-rate comparison across periods the stack changed) alongside the self-report.

A warning. Three dishonest-measurement patterns to actively avoid.

- **"Our engineers report feeling 30% more productive on average."** Average self-report, no task specificity, no triangulation. Not an audit-defensible claim.
- **"PR throughput is up 2x since adopting the assistant."** PR throughput is easy to game (smaller PRs, draft PRs, bot PRs); without a quality read alongside, higher throughput can be the stack writing worse code faster.
- **"The vendor's published study says our class of team sees 55% speedup."** The vendor's study is not your team. Local measurement is the only honest claim.

<!-- needs-research: the field is moving monthly in late 2026; verify the current state of Peng et al. follow-ups, METR's full publication, and any 2026 IC-productivity-with-agents research before relying on specific findings at commission time. -->

## Core concept 4 — the data-flow policy in detail

A model data-flow policy has four layers. Each is a decision the manager and the People / Legal partner publish together.

### Layer 1 — code sensitivity classification

Classify the team's codebase. Open-source and public-facing code (freely usable). Internal product code (usable under enterprise tiers). Security-critical or secret-handling code (restricted; often local-only assistants or none). Regulated or customer-data-touching code (restricted; the policy may require no assistant at all, or only a self-hosted assistant).

### Layer 2 — assistant allow-list by classification

For each classification, the assistants allowed. A reasonable default on a vertical product-aligned pod with enterprise tiers in place: Cursor and Claude Code on the first two classifications; Copilot available; nothing on the third and fourth without explicit review. The allow-list is a positive list; anything not on it is off.

### Layer 3 — the data-handling commitments from the vendor

The specific enterprise-tier commitments pinned in writing per assistant: whether the assistant trains on the team's code (should be no), retention period (should be short and bounded), audit log availability, SOC 2 / ISO 27001 status, and the data-residency commitments where regulation requires. This is a People / Legal + Engineering joint document, signed.

### Layer 4 — the audit cadence

How the policy is checked. A reasonable minimum: a quarterly review of the vendor's commitments against the latest terms of service (vendors change these), a semi-annual sampling audit of PRs (looking for assistant-involvement signals the team's tooling can surface), and an immediate review on any new assistant proposed by an IC. The audit cadence is the thing that keeps the policy from being a pdf nobody opens.

## Core concept 5 — the rollout plan

A worked rollout for a seven-IC vertical pod moving onto a commissioned stack (Cursor primary, Claude Code secondary, Copilot supplementary).

- **Week 0 — publish.** The commission document, the data-flow policy, the measurement plan, the thirty-day and ninety-day check-in dates. Signed by the engineering manager, read by the staff-plus IC partner, cleared by People / Legal.
- **Week 1 — install.** Every IC is on the primary assistant on the enterprise tier. The manager runs a 30-minute kickoff — not an assistant-use tutorial (that is level-20 territory), but a policy walk-through: what classifications the codebase has, what assistants are allowed where, how the audit will work.
- **Weeks 2–4 — settle.** ICs work. The manager and the senior IC run a weekly five-minute "how is the stack" check as part of the existing retro. No major decisions until week 4.
- **Day 30 — the thirty-day check-in.** A thirty-minute review. What is working. What is friction. Any data-flow concerns observed. Any assistant the team is asking to add. The first quarterly productivity survey runs.
- **Weeks 5–12 — operate.** Normal operating cadence. The incident replay and the bug triage carry any assistant-related signals back to the commission (did the stack produce a bug we had to catch? did the data-flow policy get violated?).
- **Day 90 — the ninety-day read.** The first honest productivity read. The code-review sample, the self-report survey, the narrow bake-off results (if any ran). Decision point: continue, adjust the stack (swap the primary, add or drop a secondary), tighten or loosen the policy, re-size the measurement.
- **Quarterly thereafter.** The ninety-day shape becomes the quarterly shape. The commission is re-opened annually.

A team that moves through the thirty-day and ninety-day beats produces a defensible, written record of its stack posture.

## Core concept 6 — the deferral to level 20

A specific, explicit note.

The IC-level craft of using an assistant well — knowing when to accept the completion and when to rewrite it; prompting the assistant with just enough context to make the next edit useful; reading its output critically rather than accepting it as the answer; knowing what the current model is strong and weak on; the escape hatches that keep the engineer in charge of the judgment; the deliberate-practice loop that keeps craft from atrophying under assistant throughput (mod-108 chapter 05) — is taught by `ai-assisted-software-engineer` (level 20), not by this chapter.

The manager commissions the stack; the manager sets the policy; the manager runs the honest measurement. The manager does not teach the IC how to prompt Cursor well. The two sides of the handoff are: the manager says what is on the stack; the IC's role sibling teaches how to wield it. If this chapter drifted into teaching IC-level craft, it would overlap with level-20 territory and dilute both.

Two implications for the manager.

- **When a new IC joins, the commission is onboarded (they are told what is on the stack and the policy) and the IC-level craft is signposted (they are pointed to the level-20 role sibling material, or the team's internal equivalent).** The manager does not teach the craft in the onboarding; the IC does the level-20 work.
- **When an IC is struggling with the assistant — producing plausible-but-wrong PRs, over-relying on the completion, not writing a design document without one — the diagnosis is at mod-108 chapter 05 (anti-atrophy) and the growth plan is level-20 material. The manager does not debug the IC's craft inline; the manager sets up the growth plan.**

## What good looks like

- The commission is written, dated, signed, linked from the team handbook.
- The data-flow policy is a joint People / Legal + Engineering document, with the vendor commitments pinned per assistant, and the quarterly audit cadence named.
- The measurement plan has three honest signals (specifics self-report, code-review read, narrow bake-off where relevant); no vendor-sourced top-line number is in use.
- The rollout plan hit its thirty-day and ninety-day beats; the quarterly read is on cadence.
- Every IC can name which assistant(s) are on the stack, which code sensitivity classification applies to which parts of the codebase, and where to find the policy.
- The IC-level craft is explicitly deferred to the level-20 role sibling; the manager does not teach it inline.

## What bad looks like

- Each IC chose their own assistant; the stack is sedimentary; no commission exists.
- A vendor-mandated stack is in place; the productivity claim is a vendor-sourced top-line number the manager cannot defend under scrutiny.
- The data-flow policy is a Slack thread from months ago; nobody has audited against it; the quarterly audit cadence is not scheduled.
- Sensitive code is leaving the laptop to an assistant whose ToS nobody has read; a personal account is used on a company codebase; an IC pastes code into a beta assistant to try it out.
- The honest productivity reading is "we feel faster"; no code-review signal, no triangulation, no narrow bake-off has ever run.
- The manager is teaching IC-level assistant craft in 1:1s; the level-20 deferral has collapsed; both sides of the handoff are confused.

## Summary

- The IC AI-assist stack is the manager's responsibility to commission (R14): the written choice of which assistant(s) the team uses, under what data-flow policy, measured against what productivity read, audited on what cadence.
- The field has four distinguishing axes (where the assistant lives, which models, agentic altitude, data-flow posture); the commission picks the stack against the team's workload, policy, and budget.
- The data-flow policy is a layered, joint People / Legal + Engineering document — code classification, assistant allow-list per classification, vendor commitments, audit cadence.
- Honest productivity measurement is specific, task-level, and triangulated against the current empirical literature (Peng et al. 2023, Dell'Acqua et al. 2023, Microsoft Research 2024, METR 2025); self-report drifts positive without triangulation; vendor top-lines do not survive local audit.
- The rollout walks a thirty-day and ninety-day beat and becomes a quarterly discipline.
- The IC-level craft is explicitly deferred to `ai-assisted-software-engineer` (level 20); the manager commissions, the IC role teaches the craft.

The next chapter opens the span-of-control read: how to tell when the team is partner-needed, when it is split-needed, and when a senior IC should be coached into the first-time-manager pivot.
