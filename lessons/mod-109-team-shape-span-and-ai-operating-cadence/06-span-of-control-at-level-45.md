# Span of Control at Level 45: When to Add a Partner, When to Split, When to Grow a Manager

Span of control is the oldest management-arithmetic problem. Five to eight direct reports is the conventional range Grove, Larson, Fournier, Lopp, and the broader canon converge on. The range is wide for a reason: span is not a count, it is a *bandwidth reading* against the surface the manager is actually running. Two direct reports on a quietly-executing platform team with no incident load can be a smaller span than seven direct reports on a product-shipping vertical pod running biweekly releases against a volatile external surface. This final chapter is the honest span reading at a level-45 AI-engineering-management altitude — the three directions the span goes, the signal patterns that tell you which direction the team is in, the decisions each direction asks of the manager, and the anti-patterns.

The classical span-of-control canon is prerequisite (level-40 sibling territory). This chapter specialises it to the AI engineering surface: the AI-specific rituals (chapter 03), the triage cadence (chapter 04), the commissioned assistant stack (chapter 05), and the shape decision (chapter 02) all land weight on the manager's time budget, and the honest read is against those weights.

## Motivation — three span failures a classical span-reading misses

Before the framework, three failures where the headcount-only read of span does not catch the bandwidth truth.

- **The "comfortable six direct reports" that are quietly drowning the manager.** The team is six engineers; on paper, mid-range span. The manager is eighteen months in. The AI-specific ritual set is running (model-watch, prompt + eval + trace review, incident replay, vendor release review), the triage cadence is weekly, 1:1s are weekly, calibration is twice a year, the on-call rotation is live, and the assistant stack has a ninety-day read next month. The manager's week is 55 hours; three of the engineers have growth plans in development; one is in a hard conversation about underperformance; the staff-plus IC partner is a dotted-line; the PM is a peer. On the surface, span looks comfortable; in practice, the manager cannot read traces at the altitude mod-108 chapter 02 requires because they do not have time — and the quiet consequence is calibration has drifted on three of the ICs. Span is not six; span is six-plus-the-full-AI-specific-operating-cadence, and six on that cadence is tight.
- **The ten-person team that should have been split a year ago.** The team is ten AI engineers. The manager inherited them; the shape was two merged pods from a reorganisation. 1:1s are every other week. Calibration is two days of back-to-back mock reviews. The incident replay has been skipped three cycles running because the manager cannot find the time to chair it. The triage meeting has twelve attendees and runs long. The manager knows something has to change; the organisation has been unwilling to open a second engineering-manager role. The failure is not that the team is bad; it is that the span was broken a year ago and nobody has executed the split.
- **The senior IC who has been informally managing two others for two quarters.** The L40 senior agentic engineer has been "helping" two L30 applied engineers — reviewing their PRs, giving them feedback in 1:1s that the manager does not know about, writing their growth-plan notes for the manager to co-sign. The L40's craft is deteriorating because they have no time for it. The two L30s report to the manager on paper but report to the L40 in practice. The L40 has not been coached into a first-time-manager role; they have slid into it; they are failing at both their IC craft and the management they did not sign up for. The manager had two quarters to notice and either coach them into the manager pivot formally or extract them back to IC craft; they did neither.

All three failures look like the manager is under-performing. They are span-reading failures: the headcount said one thing, the bandwidth said another, and the direction was not named in time.

## Core concept 1 — the span bandwidth read

Span of control at the level-45 altitude is a function of four weights the manager carries. Read each.

### Weight 1 — direct reports and the IC cadence

Number of direct reports is the obvious weight. For each, the manager carries: the weekly 1:1, the quarterly or biannual review, the growth-plan co-authorship, the calibration-meeting defence, the feedback-conversation cadence (mod-108 chapter 03), the promotion-packet work when the IC is up, and the hard-conversation rehearsal when it is needed. At a steady state, each direct report is roughly one to three manager-hours a week across all the cadence beats — more during promotion cycles and hard-conversation periods, less during steady execution.

### Weight 2 — the AI-specific ritual set

The four rituals from chapter 03 land weight. The model-watch (30 min / week), the prompt + eval + trace review (60 min / week), the incident replay (60 min / biweekly), the vendor-release review (90 min / month). In owner-mode the manager runs some; in attendance-mode they attend; either way the preparation work is non-trivial — the vendor-release review is a 90-minute meeting with roughly 90 minutes of preparation reading. Call it six manager-hours a month, more in active-release periods.

### Weight 3 — the triage cadence and the incident surface

The triage meeting (chapter 04) at weekly cadence with ~45 minutes of meeting plus 30 minutes of preparation — roughly four manager-hours a month. The incident surface is the volatile one: an active production regression can cost the manager a day in a week; a sustained incident class can cost a quarter's focused-work time. The honest read against the incident surface is per-quarter, not per-week.

### Weight 4 — the connective-tissue work

The weights the chapter 01 deferrals imply: partnering with the staff-plus IC, the peer manager, People / HR, and (in the first-time-manager coaching context) the level-40 sibling. The cross-functional surface (PM, security, legal) on vertical pods. The executive read-outs (mod-112). The hiring loop work when the team is growing (mod-102). These are collectively a quarter to a third of the manager's week when the team is in a growth phase; less in steady state.

### The read

The honest span read is: what is the manager's bandwidth after carrying weights two, three, and four, and does the remaining bandwidth suffice to serve the IC cadence (weight 1) on the number of ICs the manager has?

A worked arithmetic for a comfortable mid-range on a vertical product-aligned pod:

- 40-hour week available.
- 10 hours (25%) on connective tissue, cross-functional, executive, hiring.
- 6 hours (15%) on the AI-specific rituals + triage + preparation.
- 4 hours (10%) on incident surface at steady state (buffer for volatility).
- That leaves 20 hours for direct-report cadence and focused-work. At 2 hours per direct report per week on the IC cadence, that is 10 direct reports theoretically — but the "hours per direct report" is non-uniform (a direct report in promotion season or in a hard-conversation needs 4–6; a steady L30 needs 1.5; an L40 driving themselves may need 1), and the manager needs focused-work time for their own reading, their own growth, and the manager's own thinking.

In practice, five to seven direct reports on an AI engineering team with the four weights running is the sustainable range. Fewer leaves manager-of-nothing dynamics; more drifts into the "quietly drowning" failure (motivation 1).

## Core concept 2 — the three directions the span read goes

A span read is not a number. It is a *direction* the manager is about to move in. Three directions.

### Direction 1 — partner-needed

The team is at or near the top of the manager's span, and the right move is to add a technical partner at the staff-plus IC altitude or at the tech-lead altitude. The manager is not short on people; they are short on *technical altitude* — somebody to co-read the architecture, co-chair the vendor-release review, testify on L30→L40 promotions, carry the technical judgment that the manager alone cannot cover.

The partner may be a staff-plus IC (`agentic-systems-architect`, `principal-ml-engineer`) joining the team (as a report or as a dotted-line partnership), or it may be a tech-lead role grown from the current senior IC (L40 moving into a tech-lead / staff-track role, with partial technical-leadership responsibilities named explicitly and not implied).

The telling signals.

- The manager cannot read the team's architectural judgment honestly at the altitude the work demands; the staff-plus IC partner is absent from the chapter 02 shape decision.
- Vendor-release review decisions keep deferring because the manager alone is not the right altitude to make them.
- Senior ICs are frustrated that technical direction is single-point from the manager; they want a staff-plus altitude to appeal to.
- L30→L40 promotions are stuck because the testimony needed is beyond the manager's ability to carry alone.

The decision.

- Open a staff-plus IC slot on the team (or formalise the partnership with an existing staff-plus IC); write it into the shape decision (chapter 02).
- Or: name the current L40 as a tech-lead, formally, in writing, with named technical-leadership responsibilities (architecture review, vendor-release review co-chair, L30 mentorship ownership) and explicit time allocation (25–40% of their week). Not an implicit lift; an explicit, written, visible role. The L40's management work, if any, stays with the manager.

### Direction 2 — split-needed

The team is beyond the manager's bandwidth — the span is broken. The right move is to split into two teams, each with a manager.

The telling signals.

- 1:1 cadence has slipped from weekly to biweekly without an intentional decision.
- Incident-replay cycles are being skipped because the manager cannot chair them.
- Calibration is drifting: the manager cannot hold all the ICs' artefact-class reads in their head; several ICs are quietly mis-calibrated at the last review and nobody caught it in time.
- Triage meeting attendance is exceeding six or seven; the meeting runs long and produces less decision-per-minute.
- The manager's focused-work time has disappeared; they are working 55+ hour weeks sustainedly and still feel behind.

The decision.

- Open a second engineering-manager role. The split is almost always along a product-surface boundary or a capability boundary; the chapter 02 team-shape decision is re-opened to split the team cleanly.
- The split names the two new teams, the two managers (one may be the current manager; the other is hired externally or promoted internally), the mix in each team, and the relationship (peer managers reporting to the same next-line-manager).
- Internal promotion into the new engineering-manager role is often the right path — see direction 3.

A specific rule the AI-engineering canon adds: a split decision should not be delayed through more than two consecutive quarterly span reads. If the signals above have shown up in two quarterly span reads and the split has not been committed to, the manager is quietly running a broken team and the ICs know it. Act on direction 2 within one quarter of recognising it, or own the consequence in writing.

### Direction 3 — manager-to-grow

The team is at or near the top of the manager's span, there is an L40 or a senior-L30 on the team who is ready for the first-time-manager pivot (mod-108 chapter 04 names the honest-trajectory walk-through), and the organisation is in a position to split the team and promote them into the second engineering-manager role. The right move is to coach them into the pivot, formally.

The telling signals.

- The L40 or senior-L30 has been doing informal management-adjacent work for some time (motivation 3's failure shape, but caught in time); they want it formal or want it to stop.
- The IC's own growth plan and mod-108 chapter 04's honest trajectory walk-through both conclude that management is the right next move for them.
- The team is in a shape and surface the first-time-manager can reasonably hold with the manager's active coaching (not the hardest possible team: a stable surface, a predictable workload, a cohesive IC roster).
- The organisation supports a formal manager promotion (the headcount, the band, the role signoff from People and the next-line-manager).

The decision.

- Formalise the pivot. The level-40 sibling (`ai-augmented-team-manager`) owns the management-side coaching curriculum. The AI Engineering Manager owns the AI-engineering-management-specific coaching — the chapter 03 rituals they will run, the chapter 04 triage they will hold, the chapter 05 assistant-stack commission they will own. The coaching is written as a 90-day plan with specific transfers (I will run the model-watch for the first three sessions and hand the chair to you; I will co-chair the triage meeting for six weeks and then you chair it; I will shadow the first vendor-release review you run).
- The split of the team is concurrent: the new manager is given a sub-team they can hold. The IC's remaining IC work is explicitly reduced or dropped to make room for the new management work (mod-108 chapter 04 names the "manager-to-IC" pivot as common and under-named; the inverse, IC-to-manager, has the same drop-the-old-work discipline).
- The first six months are heavily coached. The new manager's 1:1 with the AI Engineering Manager is weekly and the agenda carries the AI-specific operating-cadence coaching alongside the general-management coaching the level-40 sibling runs.

## Core concept 3 — the staff-plus IC partner vs. the tech-lead distinction

Direction 1 (partner-needed) has two shapes that look similar from the outside. They are not interchangeable.

### The staff-plus IC partner

A full staff-plus IC (`agentic-systems-architect`, `principal-ml-engineer`) — typically L48+ in the mod-108 chapter 02 rubric, often reporting to a next-line engineering leader or to an architecture function, dotted-line into the team. Their work is depth technical leadership — architecture, methodology, the hardest judgment calls, promotion testimony on the L40→L48 altitude. They are not managing anyone. Mod-101 chapter 01 names them as the depth partners this manager defers to.

A team with a staff-plus IC partner benefits from technical depth beyond any L40's altitude; it loses the "always available" quality that a tech-lead inside the team would have (the staff-plus IC is partnering across multiple teams and cannot be the daily go-to).

### The tech-lead

A senior L40 inside the team, named as a tech-lead with written technical-leadership responsibilities — architecture review for the team's work, co-chairing the vendor-release review, mentoring L30s, co-owning the triage meeting. They are not managing (no direct reports; the manager retains all personnel responsibility); they are leading technically with a protected time allocation (typically 25–40% of their week on leadership work).

A team with a tech-lead gains everyday technical altitude inside the team; it does not gain the depth a staff-plus IC brings; the tech-lead's IC craft is explicitly reduced to make room for the leadership work (mod-108 chapter 04's cross-shape and cross-altitude discipline).

The decision between the two.

- **Depth beyond L40 is the need: staff-plus IC partner.** Architecture of an agentic subsystem the team has never built, methodology on a fine-tune programme that needs principal-ML altitude, promotion testimony at the L40→L48 altitude.
- **Everyday leadership presence is the need: tech-lead.** Co-chairing the rituals, mentoring the L30s, co-owning the triage, being the first appeal on technical direction. A current senior IC growing into it.
- **Both are the need: both.** Many well-shaped teams have an internal tech-lead *and* a dotted-line staff-plus IC partner; the two roles are complementary, not redundant.

## Core concept 4 — the first-time-manager coaching contract

Direction 3 (manager-to-grow) produces a specific manager-owned artefact: the first-time-manager coaching contract. It is a written document, co-authored with the IC pivoting, read by the level-40 sibling (`ai-augmented-team-manager`), covering the 90-day transition.

The contract has five sections.

1. **The new role and the sub-team.** The team the new manager will hold, its shape (chapter 02 vocabulary), its scope, its initial direct reports, and the explicit reduction of the IC's prior IC work.
2. **The co-owned coaching plan.** Which management craft is being learned, from whom. The level-40 sibling owns the general-management craft (1:1s, feedback, review cycle, promotion packets, calibration). This manager owns the AI-engineering-management craft (the rituals, the triage, the stack commission, the span read itself). The split is written, not implied.
3. **The 90-day coaching cadence.** A concrete calendar: the shadowing on existing rituals for the first three weeks, the co-chairing from week four, the independent chairing from week eight, the honest-read check-in at day 90. Weekly 1:1 with the AI Engineering Manager, carrying the AI-specific operating-cadence coaching.
4. **The honest-trajectory check-ins.** Mod-108 chapter 04's honest-trajectory walk-through adapted to the pivot: at day 30 and day 90, the new manager and the AI Engineering Manager (and separately with the level-40 sibling) read whether the pivot is going well, whether the new manager is enjoying the work, whether the IC craft they are leaving is one they want to re-enter. The pivot can be reversed cleanly at day 30 or day 90 if it is not the right move; reversing it is not a failure, it is a feature of an honest contract.
5. **The success criteria at day 90.** Specific, behaviour-worded. "The new manager has independently chaired the last two prompt + eval + trace reviews with written notes." "The new manager has run three 1:1s per IC without the AI Engineering Manager reading the prep." "The new manager has co-authored one growth plan with their IC." Not "feels confident"; specific artefacts.

The contract is a forcing function. Without it, the first-time-manager pivot is often informal and undefined, and the new manager does not know whether they are succeeding; the manager promoting them does not know whether to double-down on coaching or to reverse; the ICs under the new manager are confused by the half-defined role.

## Core concept 5 — the quarterly span read

The span read is a scheduled artefact — a 30-minute written read the manager does each quarter, timed against the quarterly ritual audit (chapter 03) and the half-year review cycle (mod-108 chapter 03). The read answers four questions.

1. **What is my current direct-report count, and what weight is each report carrying on my cadence?** A short line per IC: current level, current growth-plan status, promotion-cycle status, hard-conversation status, time-cost estimate.
2. **What is the ritual + triage + incident weight this quarter?** The ritual-cost ledger from chapter 03 updated for the quarter; the incident surface read.
3. **Where is my focused-work time going, and is the manager's own growth being served?** Are the manager's own 1:1s with their next-line-manager happening? Is the reading loop (mod-108 chapter 05) being honoured? Has the manager said "no" to anything this quarter they should have said "no" to?
4. **Which of the three directions is the team heading?** Partner-needed, split-needed, manager-to-grow, or steady state. If a direction has shown up in two consecutive quarterly reads, what is the plan to act on it?

The quarterly span read is the forcing function against drift. Without it, the manager either runs to crisis or runs quietly over-capacity for a year without a visible trigger.

## Core concept 6 — the anti-patterns

Four span anti-patterns the AI Engineering Manager should recognise and refuse.

- **The "I can hold it a little longer" anti-pattern.** The span-read signals have been present for two quarters. The manager keeps telling themselves they can hold it until the next planning cycle, the next hire, the next review period. The team's cadence has already quietly slipped (motivation 1); the signals the manager is ignoring are the signals the team is reading on them. Act on the direction in one quarter, or write down the reason the direction is being deferred and what the deferral is costing.
- **The informal tech-lead anti-pattern.** The senior IC is doing management work without the formal role, the time allocation, or the written responsibilities (motivation 3). Their craft is deteriorating and nobody has decided whether the role is management or is leadership-adjacent-IC. Formalise or extract — within one quarter of recognising it.
- **The "reorganise to avoid a hard conversation" anti-pattern.** The manager has a performance conversation they do not want to have. They propose splitting the team, or adding a tech-lead, or re-aligning a report's scope, to avoid the conversation. The reorganisation is a dodge. The hard conversation remains. People / HR partnering, the level-40 sibling partnering, and the SBI feedback library (mod-108 chapter 03) are the honest paths; a reorganisation on top of an unresolved performance case is a worse outcome than the conversation.
- **The staff-plus-IC-as-manager anti-pattern.** The team's "staff-plus IC" is informally managing people — chairing 1:1s, writing growth plans, delivering feedback in the manager's absence — without the manager role, the compensation, or the People-partnership responsibilities. The staff-plus IC is a depth role (`agentic-systems-architect`, `principal-ml-engineer`); it is not a manager role (mod-101 chapter 01). If the team needs a manager, hire or promote one; if the team needs a staff-plus IC, hire or promote one; do not blur the two.

## What good looks like

- The manager runs a quarterly span read and the artefact is in the handbook.
- The current direction (partner-needed, split-needed, manager-to-grow, or steady state) is explicit; if a direction is open, the plan to act on it has a date.
- If a tech-lead is on the team, the role is formalised in writing with time allocation and named responsibilities; if a staff-plus IC is partnered, the partnership is written with scope and expected altitude of contribution.
- If a first-time-manager pivot is in progress, the coaching contract is written, co-owned with the level-40 sibling, and the 30/90-day check-ins are scheduled.
- The four weights (direct reports, rituals, triage + incident, connective tissue) are read against the manager's actual week; the manager can name the shape of their own week out loud.
- The manager has said "no" to something in the last quarter they should have said "no" to; focused-work time and the manager's own reading loop are being honoured.

## What bad looks like

- The manager cannot name the current span-read direction; the last quarterly read did not happen or was not written down.
- Signals of a direction (missed replay cycles, drifted 1:1s, drifted calibration, long triage meetings) have been present for two quarters without a visible plan.
- A senior IC is informally managing other ICs with no formal role, no written time allocation, and no coaching contract; their IC craft has visibly deteriorated.
- A tech-lead role is implied but not written; nobody can state the responsibilities or the time allocation.
- The manager's response to a performance case is a reorganisation; the hard conversation has not happened.
- The manager is working sustained 55+ hour weeks and reads the solution as "work harder" rather than "the span is broken."

## Summary

- Span of control at the level-45 AI-engineering-management altitude is a bandwidth reading against four weights: direct reports and the IC cadence, the AI-specific ritual set and triage, the incident surface, and the connective-tissue work. Headcount alone is a thin reading.
- The three directions the span read goes: partner-needed (add a staff-plus IC partnership or formalise a tech-lead), split-needed (open a second engineering-manager role and split the team), manager-to-grow (coach a senior IC into the first-time-manager pivot).
- The staff-plus IC partner and the internal tech-lead are not interchangeable; depth beyond L40 is the staff-plus IC; everyday leadership presence is the tech-lead; both is a common combination.
- The first-time-manager coaching contract is a written, co-authored artefact with the level-40 sibling; it has five sections and runs on a 30/90-day cadence; reversing the pivot is a feature of the contract, not a failure.
- Four anti-patterns to refuse: "I can hold it a little longer," the informal tech-lead, the reorganise-to-avoid-the-conversation, the staff-plus-IC-as-manager.
- The quarterly span read is the scheduled artefact against drift.

This closes the module. The chapters built the four manager-owned artefacts — the team-shape decision, the AI-specific ritual set, the triage cadence, the IC AI-assist stack commission — and the span read that keeps all four honest. The exercises put them in your hands against a team you know. The paired `ai-engineering-manager-solutions` repository publishes reference solutions. `resources.md` points at the primary sources this module reads on.
