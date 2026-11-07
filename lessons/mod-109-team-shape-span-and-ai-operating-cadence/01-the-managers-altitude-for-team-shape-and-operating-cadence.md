# The Manager's Altitude for Team Shape and Operating Cadence

Team shape, operating rhythm, and span of control are three of the oldest problems in engineering management. Fred Brooks wrote about team structure in 1975; Andrew Grove taught the weekly staff meeting in 1983; Manuel Pais and Matthew Skelton published the modern team-topology taxonomy in 2019; and Will Larson has been writing about engineering-team sizing and the IC-to-manager pivot for a decade. The canon is mature, and `ai-augmented-team-manager` (level 40) is where this track assumes a reader has internalised it. This module does not re-teach that canon. It specialises it to the AI engineering surface, where the workload has a measurement discipline the generic engineering team does not carry (an eval harness, a prompt + judge + fixture registry, a trace corpus), where the external surface changes under the team weekly (frontier-model releases, provider deprecations, framework-level API shifts), and where the IC toolkit itself (Cursor, Claude Code, Copilot, Windsurf, Cody, Sweep) is the subject of its own commissioning decision.

This first chapter sets the altitude. What the AI Engineering Manager owns on the team-shape and operating-cadence surface, what defers to other roles, which artefacts make up the module, and what the module deliberately does not teach.

## Motivation — four failure shapes when the operating-cadence layer has no AI-specific adaptation

A team imported from a generic engineering-team template reads, at first glance, like it is working. The standups happen; the backlog is groomed; the sprint ships. Under the surface, four failures repeat.

- **The team shape that leaves measurement stateless.** The team is shaped as four applied-AI engineers plus a product manager plus a half-time infra rotation, because that is what a generic product-engineering pod looks like. There is no eval engineer and no owned measurement contract. The team ships features that look right and quietly regress against the judges nobody is running. When a regression lands in production, the team discovers it has no baseline to compare against and no judge whose verdict is trusted; the incident is chased by vibes, closed on a prompt-tweak that is never tested against the old corpus, and recurs two weeks later. The shape — not the engineers — produced the failure.
- **The ritual set that never looked at the external surface.** The team runs a textbook Scrum cadence: standup, sprint planning, retrospective, demo. None of the four rituals reads on the frontier-model release notes, the provider pricing changes, the framework-API deprecations, or the eval-score trace from the week. The team is three releases behind on provider changes and finds out when a request-format deprecation starts returning 400s in production. The rituals were honest work; they did not read on the surface this team actually operates on.
- **The triage queue that does not know *which kind* of regression it is looking at.** A bug comes in: "the summariser is worse this week." Classical triage routes it as a bug to the applied engineer who owns the summariser. Four days of investigation later, the finding is: the provider shipped a new default model version mid-week and the old prompt prompts out of it differently. No code the team owns changed. The classical triage categories (frontend / backend / infra / p1 / p2) had no language for "the vendor shipped a version." The team spends the first six months on the surface quietly re-learning the taxonomy an AI-specialised triage would have given them day one.
- **The assistant stack that nobody commissioned honestly.** Each IC installed the assistant they heard about on a podcast. Half the team is on Cursor, a quarter on Claude Code, a quarter on nothing in particular. Code is leaving the company in prompts to assistants whose data-handling nobody has read. Promotion velocity reads differently between the two halves of the team, and nobody can tell whether the difference is craft, assistant leverage, or self-reported throughput. Six months on, the team has an unspoken productivity story no one can audit — and a data-leakage posture no one can defend.

All four failures look, at first glance, like execution failures. They are not; they are *shape and cadence* failures. The team was composed and operated against a template that pre-dates the surface it is working on. This module gives the manager the AI-specific template.

## Core concept 1 — what this module does not re-teach

Everything on the shortlist below is assumed. If the reader has not internalised it, the referral is `ai-augmented-team-manager` (level 40), where general engineering-management canon is this track's prerequisite.

- **Team-topology canon.** Stream-aligned, platform, enabling, and complicated-subsystem teams; the three interaction modes (collaboration, X-as-a-service, facilitating); the cognitive-load discipline (Skelton & Pais, *Team Topologies*). This module uses the vocabulary but does not teach it.
- **Agile-ritual mechanics at base.** Standup discipline, sprint planning, retrospective facilitation, estimation, Kanban WIP limits, the Scrum master's role when a team has one. If the reader has not run these before, read the Scrum Guide and a practitioner reference (*Essential Scrum*, Rubin, or the Kanban canon, Anderson) first.
- **Classical span-of-control reading.** Numerical span, direct-report count, the heuristics on where to split a group (five to eight is the common range; Grove, Larson, Fournier, Lopp). This module adapts the read; it does not re-derive it.
- **The 1:1 practice and the review cycle.** Owned by mod-108 and by `ai-augmented-team-manager` (level 40). The ritual spine in chapter 03 schedules 1:1s and reviews as fixed points on the team cadence; it does not teach the conversation craft.
- **Budgeting and headcount mechanics.** Owned by mod-110 and by the general-management track. Team-shape decisions have cost sides, which this module names; the FinOps and build-vs-buy reading lives next door.

If any of the above are unfamiliar, read them before reading this module. Everything here assumes the mechanics are in your hands and asks: *given those mechanics, how do you specialise them to the AI engineering surface?*

## Core concept 2 — the four manager-owned artefacts

The rest of this module builds four manager-owned artefacts. They map one-to-one to the learning objectives and to the chapters.

1. **The team-shape decision (chapter 02).** The written, dated decision covering mix (how many applied, agentic, eval, RAG, fine-tune ICs, with a staff-plus IC partner or not), alignment (vertical product-aligned pod vs. horizontal capability team), and the embedded-PM question. Specialised from Larson's team-sizing framework and from *Team Topologies*; the specialisation is around AI engineering's distinctive workloads — eval and prompt and trace and judge work at scale, incident surface shaped by non-determinism, and a model-provider release cadence the team has to absorb.
2. **The ritual set (chapter 03).** The four AI-specific rituals a working team runs — a weekly model-watch meeting, a prompt + eval + trace review, an incident replay, a vendor-provider release review — sequenced against the 1:1 / planning / retro backbone the team already runs. Includes the discipline of killing a ritual that is not earning its place.
3. **The triage cadence (chapter 04).** The backlog, bug, and experiment triage for a non-deterministic system: the regression-by-cause taxonomy (prompt vs. judge vs. retrieval vs. tool vs. model-vendor), the triage meeting's shape, and the experiment-work sizing contract that keeps research-flavoured work from colonising the delivery capacity or vice versa.
4. **The IC AI-assist stack commission (chapter 05).** The written choice of assistant stack for the team (R14 in the role matrix), the honest productivity-measurement plan, and the data-flow policy. The IC-level craft of using an assistant well is explicitly deferred down to `ai-assisted-software-engineer` (level 20); the manager commissions and audits, the IC uses.

A span read (chapter 06) threads through all four: *the team you can shape, the rituals you can run, the triage you can hold, and the assistant stack you can commission honestly are all bounded by the span-of-control you actually have.* Chapter 06 is specific about reading the span honestly at a level-45 altitude.

A team has a **manager-owned shape-and-cadence surface** when: the team shape is a written, dated decision linked from the team handbook; the ritual set is published with named owners and a kill-criterion for each ritual; the triage cadence has a working regression-by-cause taxonomy and a meeting that reads on it; the assistant stack is commissioned, with a measurement plan and a data-flow policy on file; and the span read is current — the manager can name without hesitation which direction the team is next (partner-needed, split-needed, or manager-to-grow).

## Core concept 3 — what the IC owns, what the manager owns

The pattern from the rest of the track repeats here. The IC owns craft; the manager owns the system in which craft is organised and run.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Doing the AI engineering work the team ships — the feature, the eval, the retrieval pipeline, the fine-tune, the incident fix, the fixture addition. Owned by the role tracks (`applied-ai-engineer`, `agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`, `senior-agentic-ai-engineer`). | Composing the team that ships the above (chapter 02); naming the mix, the alignment, and the embedded-PM choice; making the shape change when the workload shifts. |
| Showing up to the rituals prepared — the trace read, the eval-score note, the model-watch summary, the release-review input. | Running the ritual set (chapter 03); publishing the owned, dated ritual list; killing the rituals that are not earning. |
| Classifying their own in-progress bug or regression against the cause taxonomy; naming the vendor-version in play; writing the experiment brief and defending the sizing. | Owning the triage cadence (chapter 04); publishing the taxonomy; running the triage meeting; holding the experiment-work capacity line. |
| Using the assistant the team has commissioned, in the manner the team's data policy allows, with the escape-hatch discipline the level-20 role sibling teaches. | Commissioning the stack (chapter 05); writing the data-flow policy; running the honest productivity-measurement discipline; adjudicating the "my favourite assistant is not on the list" conversation. |
| Working at the altitude the current span supports; flagging to the manager when the team is exceeding its span (incident backlog growing, PR latency rising, 1:1 cadence slipping). | Reading the span honestly (chapter 06); deciding to add a partner, split the pod, or grow a manager; executing the decision with the deferrals this chapter names. |

The pattern: *the IC ships the artefact; the manager composes the team, the ritual, and the span in which the artefact ships predictably.*

## Core concept 4 — the four deferrals

Four roles the AI Engineering Manager explicitly defers to on this module. Each has standing authority the manager does not.

### `ai-augmented-team-manager` (level 40) — the general management canon and the first-time-manager coaching

The sibling-below in the AI Workforce family. Owns the general engineering-management operating system this module assumes: team-topology canon, agile-ritual mechanics at base, classical span-of-control reading, the 1:1 / review cycle, promotion-packet discipline. This module is built on top of that. The one surface where the two manager roles explicitly partner: the IC-to-manager pivot (chapter 06). When a senior IC is being coached into a first-time-manager role, the 40-level sibling is the right partner for the *management* side of the coaching (what the role is, what the first ninety days look like, how the craft of management is learned), while this track's manager owns the *AI-engineering-management-specific* side (the AI-specific ritual set the new manager will run, the triage cadence they will hold, the assistant stack they will commission).

### `ai-assisted-software-engineer` (level 20) — IC-level assistant craft

The role sibling that teaches the IC how to use a coding assistant well — how to prompt it, how to read its output critically, how to ignore it when it is wrong, how to write the escape hatches that keep the IC in charge of the judgment. Chapter 05 is explicit: the manager commissions and audits the stack at the team level; the IC craft is level-20 territory. The two sides of the handoff are symmetric: the manager tells the IC *what* is commissioned; the IC role teaches the IC *how* to wield it.

### People / Human Resources — headcount approval, policy, and legal binding

Any team-shape change that requires a new hire, a transfer, or a departure routes through People / HR. The manager owns the written decision, the justification against workload, and the hiring-plan sequencing (mod-102 chapter 03); People own the headcount envelope, the compensation bands, the policy-of-record on assistant data-flow (the legal side of chapter 05), and the exit substrate. Partner early on any shape change that has a headcount implication; the manager's standing is to propose and defend, not to approve headcount unilaterally.

### The staff-plus ICs (`agentic-systems-architect`, `principal-ml-engineer`) — technical judgment on the work the team's shape is designed around

The staff-plus IC partners of this manager (mod-101 chapter 01) are the right readers for whether the team's shape can support the technical direction. When the team is being asked to take on an agentic subsystem it has not touched before, the `agentic-systems-architect` is the signal on whether the current mix can hold it or whether a senior-agentic hire is the honest path. When the team is being asked to run a specialist fine-tune programme, the `principal-ml-engineer` is the signal on whether the ritual set and the triage cadence are built to carry the work. The manager composes the team; the staff-plus ICs testify to whether the composition can carry the technical ambition.

A pattern to internalise: *the manager is the connective tissue between the technical direction (staff-plus ICs), the people substrate (People / HR), the sibling management canon (the level-40 peer), and the IC-level craft (the level-20 role).* Nobody else is sitting at the intersection of all four.

## Core concept 5 — what this module does not teach

Three deliberate exclusions, each a referral.

- **The IC craft itself.** This module does not teach how to write a judge, how to tune a retrieval pipeline, how to run a fine-tune, how to debug a trace, or how to use Cursor well. Each IC track teaches its own craft; `ai-assisted-software-engineer` (level 20) teaches the assistant craft. The manager recognises what good looks like at each craft, which is mod-108 chapter 02 work; this module uses the recognition to compose the team.
- **The general team-topology / agile-ritual / span-of-control canon.** Skelton & Pais's four team types, the three interaction modes, the Scrum / Kanban / XP ritual-at-base, the classical five-to-eight span heuristic — all level-40 territory, all prerequisite reading. This module uses the vocabulary and specialises it; it does not re-derive it.
- **The budget and FinOps side of team shape.** A shape decision has a cost side (chapter 02 names it, chapter 05 names the assistant-stack cost); mod-110 teaches the budget and FinOps craft. The manager who runs this module and ignores mod-110 ships a team shape that cannot be funded.

What the manager must know at depth from this module: the **vocabulary** of team shape specialised to AI engineering workloads (mix, alignment, embedded PM), the **AI-specific ritual set** that reads on the frontier-model surface, the **regression-by-cause taxonomy** that classical triage does not carry, the **honest productivity-measurement discipline** against the current empirical literature on frontier assistants, and the **three directions the span read goes** (partner-needed, split-needed, manager-to-grow) at a level-45 altitude.

## The shape of the chapters ahead

- **Chapter 02 — Choosing team shape.** Mix (applied + eval + fine-tune split, with or without a staff-plus IC partner); alignment (vertical product-aligned pod vs. horizontal capability team); the embedded-PM question; two worked team-shape decisions (a vertical product-aligned pod with embedded PM; a horizontal eval + platform capability team without one).
- **Chapter 03 — The AI-specific ritual set, and the rituals that must die.** The four rituals that earn their place; the ritual-cost ledger; the kill-criteria; the quarterly audit; the sequencing against the general-management ritual backbone the level-40 sibling owns.
- **Chapter 04 — Backlog, bug, and experiment triage for non-deterministic systems.** The regression-by-cause taxonomy (prompt / judge / retrieval / tool / model-vendor); the triage meeting's shape; the experiment-work sizing contract; the three failure shapes classical triage produces on this surface and the AI-specific fixes.
- **Chapter 05 — Commissioning the IC AI-assist stack (R14).** The current field; the honest measurement discipline; the data-flow policy; the rollout plan; the deferral down to level 20 for IC-level craft.
- **Chapter 06 — Span of control at level 45: when to add a partner, when to split, when to grow a manager.** Reading the span honestly; the three signal patterns; the staff-plus IC partner vs. tech-lead distinction; the first-time-manager coaching contract; the anti-patterns.

## What good looks like

A team has a manager-owned shape-and-cadence surface when:

- A written, dated team-shape decision is on file in the team handbook. The decision names the mix, the alignment, and the embedded-PM choice, and references the staff-plus IC partner that read it.
- A ritual set is published with named owners and a last-audit date; each ritual has a kill-criterion; the last quarterly audit retired at least one ritual that was not earning its place or named why every ritual is still needed.
- The triage cadence has a working regression-by-cause taxonomy visible on every open bug; the triage meeting runs on a cadence the team can predict; experiment work is sized and bounded against capacity.
- The assistant stack is commissioned in writing, with a data-flow policy of record and a productivity-measurement plan that the manager reads at calibration time (mod-108 chapter 06).
- The span read is current: the manager can name the next direction (partner-needed, split-needed, or manager-to-grow) and has a written plan against the direction they are in.
- The 1:1 spine, the review cycle, and the performance-management artefacts (mod-108) sit inside the cadence this module designs; nobody has to ask when the next retro, next model-watch meeting, or next calibration meeting is.

## What bad looks like

- The team shape is a historical artefact nobody has re-opened; the mix is what it was two hires ago; nobody can point to the decision in writing.
- The ritual set is "whatever Scrum we inherited"; the standup happens, the retro happens; nothing on the calendar reads on the frontier-model surface, the eval scores, the trace corpus, or the vendor release notes.
- Bugs are triaged with a generic engineering taxonomy; the regression-by-cause distinction (prompt vs. judge vs. retrieval vs. tool vs. model-vendor) exists only in individual engineers' heads; nobody classifies.
- The assistant stack is whatever each IC installed; the data-flow policy is a Slack thread from eight months ago; productivity claims are self-reported; the manager cannot defend the current posture under audit.
- The manager cannot say honestly which direction the span is heading; the backlog grows, the PR latency slips, the 1:1 cadence falls behind, and the diagnosis keeps being "we need to work harder."
- The ritual set and the triage meeting have been the same for a year; no ritual has been killed, no taxonomy has been updated, no shape decision has been re-opened.

## Summary

- Team shape, operating cadence, and span of control are the manager-owned layer that organises AI engineering craft into a team that ships predictably against a non-deterministic surface. The general canon (Skelton & Pais, the Scrum / Kanban / XP ritual-at-base, Larson on span) is the prerequisite; this module specialises it.
- The manager owns four artefacts: the team-shape decision, the AI-specific ritual set, the triage cadence, and the IC AI-assist stack commission. A span read threads through all four.
- Four deferrals: `ai-augmented-team-manager` (level 40) for the general canon and the first-time-manager coaching; `ai-assisted-software-engineer` (level 20) for IC-level assistant craft; People / HR for headcount, policy, and legal binding; the staff-plus ICs for technical testimony on whether the team's shape can carry the direction.
- What this module does not teach: the IC craft (owned by each role track), the general topology / agile-ritual / span canon (owned by the level-40 sibling), the budget and FinOps side (owned by mod-110).

The next chapter opens the first manager-owned artefact: the written, dated team-shape decision, with the mix, the alignment, and the embedded-PM question specialised to AI engineering workloads.
