# The AI-Specific Ritual Set, and the Rituals That Must Die

A ritual is a meeting that recurs. Rituals accumulate. Every manager inherits more than they add, and every team ends up with a weekly calendar whose shape is sedimentary — this standup from Scrum, that retro from a book somebody read in 2018, this planning meeting from a transformation that happened two years ago, this product sync that used to be twice a week and nobody shrank to once. The second manager-owned artefact of this module is a written ritual set — the meetings an AI engineering team runs, with named owners, written purposes, outputs, and kill-criteria — plus the discipline that retires the meetings that are not earning their place.

This chapter names the four rituals that are AI-specific — the ones this module adds on top of the general-management ritual backbone the level-40 sibling teaches — and the discipline that keeps the full calendar honest. The general ritual mechanics (standup, sprint planning, retrospective, demo, 1:1, review cycle) are prerequisite; this chapter sequences the AI-specific rituals against them and names the unique ritual work that is AI-specific.

## Motivation — three ritual-set failures that look like calendar fatigue

Before the framework, three ritual failures that read as the team being "over-meetinged" when the real problem is deeper.

- **The ritual that reads on no AI-specific surface.** The team runs twelve hours of meetings a week — standup, sprint planning, grooming, retro, 1:1s, demo, product sync, cross-team sync, on-call handoff. None of them reads on the frontier-model surface, the week's eval scores, the trace corpus the team has accumulated, or the provider release notes. Three weeks after a provider version-change, a production regression is traced back to a prompt-behaviour drift the team's rituals would not have caught — because none of them was designed to catch it. The team is meeting enough; the meetings are not reading on the right surface.
- **The ritual that quietly turned into a status report.** The team has a Thursday "AI update" meeting, inherited from a previous manager. It has been the same meeting for eighteen months: engineers report what they shipped, the manager nods, nobody makes a decision, nobody surfaces a disagreement, and the output is a slack message of bullet points. The ritual is expensive — six engineers, 45 minutes, every week — and it produces no outcome the team or the business needs. It has not been killed because killing it would require someone to notice it is not earning its place.
- **The ritual set whose outputs are not read.** The team runs a model-watch meeting weekly. The output is a note. Nobody outside the meeting reads the note. The decisions made in the meeting (we are OK staying on GPT-4o-2024-08-06 for another month; we are moving the summariser pipeline to the new Claude snapshot after we re-run the registered judges on the current prompts) live only in the heads of the attendees; the on-call engineer who was not there does not know, and runs an investigation six weeks later against the old model version because the trace she inherited did not record which model it ran on. The ritual happens; its outputs do not compound.

All three failures look like calendar fatigue. They are ritual-design failures. The AI engineering team's ritual set has to be chosen, published, and audited on the same discipline the eval harness is — what is it measuring, and is the measurement earning its place?

## Core concept 1 — the ritual backbone the level-40 sibling owns

The general-management ritual backbone this module assumes.

- **Daily standup** (if the team uses one; some teams defer to async standups in writing). Mechanics: Scrum Guide.
- **Sprint planning** (or equivalent iteration planning). Mechanics: *Essential Scrum*, Rubin; the GitLab handbook.
- **Retrospective** (iteration retrospective, delivered on the cadence the team chose). Mechanics: Derby & Larsen, *Agile Retrospectives*.
- **Demo / showcase** (customer-facing or stakeholder-facing demonstration of what shipped).
- **1:1s** (manager ↔ IC; manager ↔ staff-plus IC; manager ↔ peer; manager ↔ next-line-manager). Mechanics: mod-108 chapter 03, plus the Fournier / Hogan canon.
- **Review and calibration cycle** (half-year or annual, with the mid-cycle and calibration beats). Mechanics: mod-108 chapters 03 and 06.
- **On-call handoff** (weekly or at whatever on-call-rotation length the team uses). Mechanics: mod-107 and mod-106.
- **Cross-team and cross-functional syncs** (product, security, legal, exec). Mechanics: mod-111 and mod-112.

Everything in the list above is level-40 territory. This module does not re-teach it. This module adds four AI-specific rituals on top and names how to sequence them.

## Core concept 2 — the four AI-specific rituals that earn their place

The four rituals this chapter defends. Each has a purpose, a cadence, an owner, an output, and a kill-criterion.

### Ritual 1 — the weekly model-watch meeting

**Purpose.** Read the external model-provider surface the team depends on. Catch provider releases, deprecations, pricing changes, version rollouts, and framework-API shifts before they land in production. Decide whether the team moves (migrates to a new default model snapshot, re-runs evals against a new version, updates a prompt against a new vendor's behaviour) or holds (we are staying on the current version until the next release window).

**Cadence.** Weekly, 30 minutes. Timed against the vendor cadence the team's dominant providers publish — most frontier providers release and deprecate on near-weekly rhythms; a monthly meeting is too slow.

**Owner.** The senior IC covering the model-portfolio reading (or the manager, if no senior IC owns it). Chair rotates quarterly; rotation is a deliberate-practice slot for the IC growth plan (mod-108 chapter 05).

**Input.** The last week's release notes from the team's dominant providers (Anthropic, OpenAI, Google DeepMind, Meta, Hugging Face, and the framework vendors in use — LangChain, LlamaIndex, Pydantic AI, Instructor, etc.). The last week's eval-score trace. The last week's incident log. Any customer-side feedback flagged by the PM.

**Output.** A dated note, written in the team handbook. The note has three sections: (1) what changed on the external surface this week; (2) what the team is doing about it (migration planned, eval re-run queued, prompt update in review, or explicit "no-op, we are holding"); (3) the next meeting's watch-list (what we are watching for in the coming week).

**Kill-criterion.** If three consecutive weeks of notes end "no-op, we are holding" and no observed drift on the eval harness, downgrade to bi-weekly. If three further weeks bi-weekly produce no decisions, downgrade to monthly and merge with the vendor-release-review ritual. Do not kill entirely — the external surface changes even when you are not reading it; downgrade, do not blind.

**Why this ritual earns its place.** Mod-105 (vendor and model portfolio) is the strategic reading; this ritual is its operating cadence. Without the model-watch, external-surface absorption is ad-hoc — the senior IC catches the release in a podcast or a slack channel on a Tuesday and the team is one person deep on provider awareness.

### Ritual 2 — the prompt + eval + trace review

**Purpose.** Read the team's own system. Walk a sample of the week's traces — the longest, the ugliest, the slowest, and a random draw — against the registered judges and the week's eval scores. Catch regressions the summary metrics mask, raise fixture candidates, close the trace → eval → fix loop (mod-104).

**Cadence.** Weekly, 60 minutes. Longer than the model-watch because it is working session, not read-out. In some teams the ritual is "weekly trace Fridays" with a single trace walked slowly; in others it is a rotation across subsystems. The shape matters less than the discipline: at least one working session a week against the trace corpus.

**Owner.** The eval engineer (in a vertical pod that has one; in a horizontal eval capability team, one of the eval engineers). In a pod without an eval engineer, the senior-most IC owns the ritual explicitly as part of their L40 artefact-class contract (mod-108 chapter 02).

**Input.** The week's traces (a sample, not all), the registered judges' verdicts, the eval harness's week-over-week delta, any fixture candidates raised by the on-call rotation, any customer complaints the PM has flagged.

**Output.** A short written note — what was walked, what was found, which fixtures were added to the registry, which prompts were updated or re-reviewed. The output is pinned to the registry, not just to a meeting-notes doc.

**Kill-criterion.** The ritual is in the kill queue if (a) the registry has not had a new fixture added from the ritual in four consecutive weeks (either the registry has caught up, in which case shrink to monthly; or the ritual has drifted into read-out, in which case rebuild it) and (b) the trace corpus has no observed uninvestigated anomalies.

**Why this ritual earns its place.** The eval + trace + fixture loop is the compounding mechanism of the team's measurement discipline. If the team is not working the loop weekly, the registry rots and the trace corpus becomes a write-only buffer. The ritual's existence is what keeps the measurement discipline alive.

### Ritual 3 — the incident replay

**Purpose.** Walk one closed incident (from the current cycle or a recent one) through the trace, the escalation, the fix, and the fixture-that-should-have-caught-it. Blameless. Inherited from the SRE / mod-107 practice but specialised: the blame-free surface is the AI-specific causal chain (prompt / judge / retrieval / tool / model-vendor), and the output is almost always a new fixture or a new judge.

**Cadence.** Every two weeks, 60 minutes, or weekly if the incident surface is hotter. In quiet periods, monthly with two incidents walked back-to-back.

**Owner.** The engineering manager or a rotating senior IC (mod-108 chapter 02's L40 incident-command artefact class). The ritual is deliberately exposed to the team's L30 ICs so the on-call-command craft grows.

**Input.** One closed incident, the full trace, the registered prompt and judge involved, the retrieval log, the tool-call log, the postmortem (mod-107 chapter 05 shape — blameless, five-whys-adjacent, classified by cause).

**Output.** Either (a) a new fixture or a judge update in the registry; or (b) a documented decision that the incident's root cause was out-of-scope for the current fixture surface, with a written note of what is being deferred. Both outputs feed the eval harness.

**Kill-criterion.** If the team has had no production incidents in six weeks and the backlog of unreplayed incidents is empty, downgrade to monthly. If the team is small enough that incidents are rare, the ritual can be merged with the prompt + eval + trace review as a quarterly deep-walk. Do not kill entirely — the incident-to-fixture loop is how the team's operating-system memory compounds.

**Why this ritual earns its place.** Mod-107 is the strategic reading on blameless postmortems; this ritual is the cadence that keeps the practice alive on a team scale. Without a scheduled replay, incidents produce postmortems that are written and then shelved; with one, the registry grows after every incident and the measurement discipline compounds.

### Ritual 4 — the vendor-provider release review

**Purpose.** The decision-making companion to the model-watch. Where the model-watch is "what happened on the external surface this week," the release review is "which provider releases are we going to act on this month." The output is not notes; it is decisions — which version migrations are scheduled, which fine-tunes are being paused or re-scoped against the new baseline, which prompt rewrites are commissioned.

**Cadence.** Monthly, 90 minutes. The monthly rhythm is tuned against the typical provider release cadence (major releases on near-monthly rhythms; the ritual catches the ones that matter).

**Owner.** The engineering manager plus the staff-plus IC partner (`agentic-systems-architect` or `principal-ml-engineer`), with the eval engineer and the senior applied/agentic/RAG ICs attending. This is the only ritual on the AI-specific list where the staff-plus IC partner is inside-the-room by default.

**Input.** The last month's model-watch notes, the eval-score surface over the month, the vendor roadmaps the team has access to, the portfolio reading from mod-105 (current provider mix, concentration risk, upcoming deprecation deadlines), the cost reading from mod-110 (this quarter's provider spend vs. budget).

**Output.** A dated decision log. Decisions name the version migrations, the fine-tune work being re-sized, the prompt work being commissioned, the cost moves being made, and the next re-read date.

**Kill-criterion.** The ritual earns its place as long as the team's dominant vendor spend is non-trivial and the provider landscape is moving. If the team's portfolio reading (mod-105 chapter 06) shows a stable provider mix for two quarters, downgrade to quarterly. The ritual becomes unnecessary only if the team is explicitly deferring the whole vendor-portfolio decision to a central function (a central AI platform team, a central model-governance committee), in which case the decision belongs on the central function's calendar, not the team's.

**Why this ritual earns its place.** Mod-105 is the portfolio discipline; this ritual is where the portfolio decisions actually get made. Without it, migrations drift by six to twelve weeks past their natural window, cost moves do not land, and the executive read-outs (mod-112) report vendor spend the team is not acting on.

## Core concept 3 — sequencing against the general ritual backbone

The four AI-specific rituals have to sit inside a weekly calendar that already carries the general backbone. A worked sequencing for a vertical product-aligned pod of seven (chapter 02 vignette 1):

| Day | Morning | Afternoon |
|---|---|---|
| Monday | Standup (15 min). Sprint planning or grooming, biweekly (90 min). | Model-watch meeting (30 min). 1:1s (2–3). |
| Tuesday | Standup (15 min). | Prompt + eval + trace review (60 min). PR reviews / focused work. |
| Wednesday | Standup (15 min). Cross-team sync (30 min, biweekly). | Focused work. |
| Thursday | Standup (15 min). | Incident replay (60 min, biweekly). 1:1s (1–2). |
| Friday | Standup (15 min). Demo or retrospective, alternating (60 min). | Focused work / on-call handoff. |

Monthly: the vendor-provider release review (90 min). Quarterly: the ritual-set audit (90 min), run as a retrospective on the ritual set itself (see core concept 5 below).

Three sequencing notes.

- **The model-watch sits Monday morning for a reason.** The team enters the week with a current read on the external surface, which makes the week's decisions downstream (sprint planning on Monday afternoon or Tuesday, 1:1s through the week) read on current information. A Friday model-watch strands a week's worth of decisions on last week's information.
- **The prompt + eval + trace review sits Tuesday.** After the model-watch (so the team knows if a vendor release is shaping this week's traces) and before the week's shipping pace peaks. A late-week slot risks the ritual being bumped by shipping pressure; a Monday slot stacks too much decision-making on one day.
- **The incident replay is biweekly in healthy periods.** The ritual is more expensive than the weekly ones because it involves a deep walk through one artefact; running it weekly in quiet periods produces read-out dynamics. The right frequency is "whatever cadence keeps the backlog of unreplayed incidents close to zero" — biweekly in healthy periods, weekly in hot periods, monthly in quiet.

## Core concept 4 — the ritual-cost ledger

Every ritual has a cost. A team of seven running a 60-minute meeting costs seven person-hours. The ritual-cost ledger is a written reading, revisited quarterly, of what the team's ritual calendar actually costs — in time, in cognitive switching, in the focused-work blocks it fragments.

A minimal ledger has three columns: the ritual, the weekly cost (person-hours = headcount × duration × frequency), and the last date the ritual produced a decision or an artefact that outlived the meeting. The last column is the earning column; a ritual that has not produced a decision or artefact in six to eight weeks is a candidate for the kill queue.

A worked ledger for the sequencing above:

| Ritual | Cadence | Duration | Headcount | Weekly person-hours | Last decision/artefact produced |
|---|---|---|---|---|---|
| Daily standup | 5x/wk | 15 min | 7 | 8.75 | Yesterday — dependency surfaced |
| Sprint planning | Biweekly | 90 min | 7 | 5.25 | This sprint's commitments |
| Retrospective | Biweekly | 60 min | 7 | 3.5 | Last retro's three action items |
| Demo | Biweekly | 60 min | 7 | 3.5 | Last demo's customer-side feedback |
| Model-watch | Weekly | 30 min | 4 | 2.0 | This week's migration-hold note |
| Prompt + eval + trace review | Weekly | 60 min | 5 | 5.0 | Two fixtures added to registry |
| Incident replay | Biweekly | 60 min | 5 | 2.5 | Last replay's judge update |
| Vendor release review | Monthly | 90 min | 5 | 1.9 | Last month's decision log |
| 1:1s | Weekly | 30 min each | — | 3.5 (manager side) | Each 1:1's agenda delivered |
| Cross-team sync | Biweekly | 30 min | 3 | 0.75 | — |
| **Total** | | | | **~36.6 p-h/wk** | |

A team of seven loses roughly 37 person-hours a week to recurring meetings against a theoretical capacity of 280 person-hours (40 × 7). That is 13%. Is that too high? It depends on the earning column — if every ritual is producing a decision or an artefact, 13% is low; if two or three rituals have not produced anything in six weeks, the ledger has rot and 13% is wasted.

The ledger is the artefact the quarterly ritual audit reads on. Write it once, keep it current.

## Core concept 5 — kill-criteria and the quarterly ritual audit

The quarterly ritual audit is a 90-minute meeting the manager runs once a quarter. It is a retrospective on the ritual set, not on the work. The input is the ritual-cost ledger; the output is a set of changes — rituals retired, rituals downgraded in frequency, rituals re-chartered with new owners or outputs, rituals merged.

A ritual is a candidate for the kill queue when any of the following hold.

- **No decision or artefact in six to eight weeks.** The ritual is happening; it is not producing anything that outlives the meeting. Either re-charter (name the output) or kill.
- **Attendance drift.** The invite says five; three or four show up each week; nobody can explain who should be there. The ritual has drifted and nobody agrees on its purpose. Re-charter or kill.
- **Output is not read.** The ritual produces a note; the note is not linked from the team handbook, not referenced in any downstream decision, not read by the on-call engineer who was not there. The ritual exists; its output does not compound. Fix the output-publication step or kill.
- **Merge opportunity.** Two rituals cover overlapping surface (a weekly model-watch and a weekly AI-news-of-the-week sync run by a different person). Merge them; the merged ritual takes one owner, one slot, one output.
- **Context shift.** The team's workload has moved and the ritual was designed against the old workload. The classic case: a weekly sprint-planning ritual on a team that has moved to continuous flow (Kanban). The ritual does not match the mechanics any more; retire it.

A ritual that has earned its place has, specifically: a named owner, a written purpose, a cadence matched to the surface it reads on, an output that is published somewhere the team reads, and a kill-criterion the manager will actually act on.

The audit's output is a written changelog: what was killed, what was downgraded, what was re-chartered, what was added. The changelog is the forcing function; without it, the audit decays into a meeting about meetings.

## Core concept 6 — rituals that must die

Three specific rituals the manager should default to killing on first inspection.

- **The weekly "AI status" meeting that is a verbal read-out of the sprint board.** The sprint board is already visible; reading it aloud costs seven person-hours a week and produces no decision. Replace with a written update (slack, handbook, or a daily-standup note) and reclaim the time.
- **The "research" meeting with no concrete deliverable.** A weekly 60-minute block where "we discuss recent papers." Research is important; a ritual without a deliverable is not how the team does it. Replace with a rotating reading-group with a one-page written summary per session published to the handbook (mod-108 chapter 05's reading-loop deliverable), or with a 1:1 agenda item in each IC's growth plan.
- **The "AI office hours" with no attendees.** The team opened a weekly office-hours slot for stakeholders. For the first four weeks it had attendees; for the last fifteen weeks it has had none. Kill it. Replace with an asynchronous intake form or a monthly slot that fires only when an agenda exists.

Killing a ritual is a leadership act. It signals that the manager is reading the calendar, is willing to retire their own meetings, and is modelling the audit discipline. A manager who has killed zero rituals in the last year is almost certainly running a calendar with waste; a manager who has killed two or three has usually tightened the week visibly for the team.

## What good looks like

- The AI-specific ritual set is published in the team handbook with name, purpose, cadence, owner, output, kill-criterion for each.
- The ritual-cost ledger is current (updated within the last quarter) and visible to the team.
- Every AI-specific ritual's last-decision-or-artefact column has an entry from the last six to eight weeks.
- The quarterly ritual audit has run on time and produced a written changelog; the last audit retired or re-chartered at least one ritual.
- The sequencing is such that the model-watch precedes sprint planning and the prompt + eval + trace review precedes the week's shipping peak.
- Outputs of each AI-specific ritual are linked from the team handbook; an on-call engineer who missed the week's rituals can reconstruct what was decided from the notes alone.

## What bad looks like

- The team's calendar is sedimentary; the manager cannot say in one sentence what each ritual is for.
- No ritual-cost ledger exists; the team has never tallied how many person-hours a week recurring meetings consume.
- Rituals run on autopilot; the quarterly audit has not happened (or happens on an annual cadence and produces no changes).
- The model-watch and the vendor-provider release review do not exist, or exist but have not produced a decision in the last quarter.
- The prompt + eval + trace review has quietly become a status read-out; the registry has not had a fixture added from it in two months.
- Incident replays are skipped when the on-call rotation is quiet; the backlog of unreplayed incidents is three or more; the incident-to-fixture loop is broken.
- Rituals are added over time; none are ever killed; the calendar is one-way ratcheted toward full.

## Summary

- The AI engineering team's ritual set is the general-management ritual backbone (standup, planning, retro, demo, 1:1, review cycle, on-call handoff) plus four AI-specific rituals: the weekly model-watch, the weekly prompt + eval + trace review, the (bi)weekly incident replay, and the monthly vendor-provider release review.
- Each ritual has a purpose, a cadence, an owner, an output, and a kill-criterion; the four AI-specific rituals each read on a specific AI engineering surface the general backbone does not reach.
- The ritual-cost ledger is a written, quarterly-revisited reading of what the calendar actually costs; the quarterly ritual audit reads the ledger and retires, downgrades, re-charters, or merges rituals.
- Killing a ritual is an explicit leadership act; a manager who has killed zero in the last year is almost certainly running a calendar with waste.

The next chapter opens the third manager-owned artefact: the backlog + bug + experiment triage cadence for a non-deterministic system, with the regression-by-cause taxonomy that distinguishes it from classical triage.
