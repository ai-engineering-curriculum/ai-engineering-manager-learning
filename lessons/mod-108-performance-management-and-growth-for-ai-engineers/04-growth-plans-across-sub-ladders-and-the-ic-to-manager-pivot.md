# Growth Plans Across the Sub-Ladders and the IC ↔ Manager Pivot

The rubric in chapter 02 reads an IC against where they are. The cadence in chapter 03 keeps the read current. This chapter is where the manager and the IC co-author *where the IC is going* — the written growth plan that names a next-level target, a cross-shape rotation, a tech-lead stretch, or a track pivot, with the sequencing and the specific commitments that get them there. The AI-engineering specialisation is that the growth plan commonly moves an IC *across sub-ladders* (applied → eval, agentic → RAG, RAG → fine-tune) in a way general-engineering growth plans rarely do, and because the AI-engineering sub-ladders are young, those moves are often designed, not inherited. This chapter builds the artefact and names the four growth trajectories the manager will see most.

## Motivation

Three failure patterns the written growth plan prevents.

- **Growth as conversation only.** The IC has growth aspirations — a next level, a shape they are curious about, an interest in tech lead or management. The aspirations surface in a 1:1, the manager agrees they are good aspirations, nothing is written, the cycle ends, next cycle starts with the same conversation. A year passes; the IC has grown in the direction of whichever project landed in their inbox. The plan was never a plan; it was a mood.
- **The invisible sub-ladder trap.** An L30 applied-AI engineer is interested in moving toward eval work. Their manager says "sure, take some eval cases." Six months later the engineer has written thirty eval cases and is still placed as applied in the rubric; nobody has formally sponsored the shape transition; the eval specialist who was supposed to be the craft partner was never looped in; the engineer is now half-applied, half-eval, under-placed in both. The growth plan was there in spirit; the *sponsorship* and *placement* mechanics were not.
- **The misdiagnosed management ambition.** A strong L40 engineer says "I think I want to be a manager." The track-switch conversation happens informally in 1:1. The manager, enthusiastic to grow a leader, makes space on the team's management surface; the engineer takes it, discovers six months in that the parts of the work they actually loved (the deep trace reading, the architecture call) are gone, and asks to switch back. The role swap caused a quarter of productivity loss on the team and a dented engineer who thinks they "failed" at management when actually nobody designed the pivot and the honest conversation about what the role was did not happen. The `ai-augmented-team-manager` (level 40) partnership and the honest trajectory walk-through in chapter 05 of *The Manager's Path* were the missing pieces.

The written growth plan plus explicit trajectory design is the antidote to all three.

## The growth-plan template

The growth plan is a short written artefact co-authored by the IC and the manager, updated at review cycles and whenever a trajectory decision is made. The template fits on one to two pages.

### Sections

1. **Current placement.** Shape (from chapter 02), level, artefact-class grid read at the last review. One paragraph.
2. **Trajectory target for the next 12 months.** Pick one of the four named trajectories below (level-up in shape, cross-shape rotation, tech-lead stretch, track pivot) or an explicit combination. Not "whatever opportunities arise" — a specific trajectory name the plan is designed against.
3. **Specific commitments, with sequencing and timebox.** The three to five things the IC will do in the next cycle that build evidence toward the trajectory. Each commitment carries a dated check-in and a named sponsor if someone outside the manager relationship (a staff-plus IC, a peer manager, an eval specialist) is involved.
4. **Anti-atrophy element.** The specific "learn without the assistant" commitment and the reading-loop commitment from chapter 05. Not optional; growth plans without an anti-atrophy element in a frontier-tooling era are plans the manager will regret next cycle.
5. **Credential-honesty element.** If the plan includes external artefacts (conferences, publications, OSS contributions, certifications), each one is named with the inside-the-team read-out it is expected to produce (chapter 07). External activity without an inside read-out is theatre and is flagged as such on the plan.
6. **Drift notes.** Any technology-drift considerations that affect the plan — a vendor roadmap that may obviate the shape of a commitment, a model-generation shift that may re-read the rubric (chapter 06). One or two lines; sets up the drift-fair review conversation at cycle-end.

### A one-page example

```
IC: [name]        Shape: applied-ai-engineer       Level: L30
Last-review grid: traces at, evals above, registered at, incidents at, mentorship at

Trajectory target for next 12 months:
  Cross-shape rotation: applied → eval (4 months) → return with eval depth; 
  level target for cycle-end 2026Q4 remains L30 applied, with eval-shape endorsement 
  from the eval specialist (chapter 02: dual-shape read).

Commitments, next cycle:
  1. Pair with ai-eval-engineer S. on judge-methodology work, 2 days / week, 
     weeks 1–16. Sponsor: S. Check-in: monthly in joint 1:1.
  2. Author the methodology memo for the new release-gate judge. Draft 
     due week 8. Reviewed by S. and the release-triad chair.
  3. Shadow one on-call rotation on the eval surface in weeks 10–14.
  4. Co-author the eval-registry convention update with S.; publish week 20.

Anti-atrophy element:
  - One deliberate-practice loop per week: hand-score 20 judge outputs without 
    the assistant; compare against the assistant's scores; notes in the 1:1 doc.
  - Reading loop: provider release notes and vendor cookbook sections on 
    eval / structured output, one hour per week; summarise one takeaway per week.

Credential-honesty element:
  - Attending one eval-focused conference talk this cycle. Expected inside 
    read-out: one takeaway brought back to the eval registry; shared in a team 
    show-and-tell.

Drift notes:
  - Provider X's judge-model roadmap includes a structured-output upgrade in Q2;
    the methodology memo should account for the change; may require threshold 
    recalibration mid-cycle.
```

The plan is short, dated, sponsored, and specific. The cycle-end review packet (chapter 03) reads directly off it: what moved, what didn't, what is sequenced for the next one.

## Trajectory 1 — level-up within shape

The simplest growth trajectory. The IC is staying in their shape (`applied-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`, `agentic-ai-engineer`) and growing toward the next level anchor (L30 → L40, or on the L40 → L48 arc which the staff-plus IC tracks teach).

### What the plan looks like

The plan reads directly off the artefact-class grid in chapter 02. The gap rows — where the IC is "below" or "at" and the next level anchor expects "above" — are the specific commitments. For an L30 → L40 trajectory, mentorship across adjacent shapes and incident-command craft are the most common gaps the plan will name. For an L40 → L48 trajectory, cross-team influence, standards authorship (ADRs, methodology handbooks), and multi-quarter technical direction are the most common gaps.

### What the manager sequences

- A scope expansion in the IC's current shape that creates the occasion for the missing artefacts. If the gap is a cross-team ADR, the manager works with the staff-plus IC and the peer manager to sponsor an ADR with real cross-team scope (not a makework ADR).
- A mentorship assignment that produces the missing mentorship record; often a specific L20 or L30 engineer on an adjacent shape who could benefit.
- An on-call shadowing or primary-rotation assignment where incident-command experience is the gap.

### Sponsorship

The sponsor pattern at L30 → L40 is usually the manager plus the staff-plus IC (`agentic-systems-architect` for agentic-shape candidates; `principal-ml-engineer` for ML-methodology candidates). The sponsor is not just a reviewer; they commit to reading the IC's work at depth over the trajectory window.

## Trajectory 2 — the cross-shape rotation

The most AI-engineering-specific growth move: an IC moves laterally across sub-ladders. The common patterns the manager will see:

- **Applied → eval.** The applied engineer who has grown an interest in measurement and wants to own the measurement contract instead of consuming it.
- **Applied → agentic.** The applied engineer whose shipping scope has drifted toward multi-step flows and tool use, and whose depth needs the agent-specific craft.
- **Agentic → senior agentic.** The L30 agentic engineer stepping into tech-lead on the agentic surface; a level-up *and* a shape sharpening.
- **RAG → applied.** The specialist RAG engineer who wants to broaden; often a path for a senior IC who has fully owned a retrieval substrate and is ready to carry integration scope.
- **Fine-tune → applied or eval.** When the fine-tuning case on the team becomes smaller (vendor APIs closed the gap) or shifts to a different shape of work, the fine-tuning specialist often wants to re-anchor.
- **Infra → applied or agentic.** The AI-infra engineer (peer-manager's team) who wants to move to the application surface; often involves a cross-team move the two managers co-sponsor (chapter 01 deferral 4).

### What the plan looks like

The cross-shape rotation has three phases, usually spanning two to four months.

**Phase 1 — pairing (weeks 1–8).** The rotating IC pairs with a specialist of the target shape for a significant portion of their time (one or two days a week minimum). The pairing produces specific co-authored artefacts — not shadow work, but artefacts the specialist signs off on. The pairing is sponsored by the specialist (the "receiving sponsor") and co-ordinated by the sending manager.

**Phase 2 — scoped ownership (weeks 9–16).** The rotating IC owns a small scope in the target shape — one eval suite, one fixture class, one retrieval sub-problem, one fine-tune experiment — that they ship end-to-end with the specialist as reviewer. The ownership is real; the artefact is theirs.

**Phase 3 — placement decision (week 17–20).** The sending manager, the receiving sponsor, and the IC decide together: does the IC return to their original shape with a cross-shape competence (the common outcome), or does the IC transition fully to the target shape (occasional but valid)? The decision reads against the artefact-class grid in both shapes.

### Sponsorship

The cross-shape rotation has two sponsors: the IC's manager (sending) and the specialist IC in the target shape (receiving). Neither owns the rotation alone. If the target shape is on another team, the peer manager (chapter 01 deferral 4) is a third party in the sponsorship.

### Risk notes

- **The half-rotation trap.** An IC who pairs for Phase 1, then gets pulled back onto their original shape's work by a staffing emergency, ends up half-placed in both shapes and under-counted in the next review. The sending manager's job is to protect the rotation window from exactly this interruption; the cycle planning (mod-103, mod-109) should have slack for it or the rotation should not be committed to.
- **The reverse-rotation ask.** An IC who has done a 4-month rotation and wants to go back to their original shape is making a legitimate call; the growth plan should treat "returned with cross-shape competence" as the primary outcome, not "transitioned permanently," so the IC is not graded as having "failed" the rotation if they return.

## Trajectory 3 — the tech-lead stretch

Distinct from a straight level-up because it is not primarily about the level; it is about the *tech-lead role on the sub-system*. An L30 who is the acting tech-lead on an agent surface for two quarters is on a tech-lead stretch; they may or may not be promoted to L40 at cycle-end, depending on the artefact-class grid, but the role experience is the growth even if the promotion lands a cycle later.

### What the plan looks like

- **Named sub-system.** The tech-lead stretch targets a specific sub-system — the agent's planning loop, the eval registry's convention layer, the retrieval substrate's corpus contract, the fine-tune methodology. Not "tech-leading the team"; a specific scope.
- **Tech-lead-specific commitments.** Author a design document for the sub-system; chair an architecture review; commit to one cross-team artefact (an ADR, a tool-contract spec, a methodology memo) that other teams read.
- **Partner staff-plus IC.** The tech-lead stretch at L30 → L40 nearly always requires the staff-plus IC to sponsor the design work; the manager alone cannot read the architectural depth.
- **Incident-command rotation.** Senior tech-lead craft includes being the Incident Commander on their sub-system's rotation (mod-107 chapter 04). The stretch includes commanding at least one in-production incident in the window.

### Risk notes

- **The "senior-IC" trap.** A tech-lead stretch that never names cross-team scope collapses into "senior IC doing their own work well." The specific thing that makes a tech-lead role is reach beyond their own output; chapter 02's influence dimension at L40 is the test.
- **The quiet promotion deferral.** When a tech-lead stretch produces the artefacts and the IC does not get promoted at cycle-end, the manager must explain specifically which artefact-class row is the gap, not say "you need more time." Opaque deferral kills trust; chapter 06 is specific about the correction.

## Trajectory 4 — the IC-to-manager-track pivot (and the manager-to-IC return)

The track-switch trajectory. The most consequential growth plan the manager and IC will co-author together, and the one that benefits most from an explicit partnership with the sibling-below role, `ai-augmented-team-manager` (level 40, AI Workforce family).

### The two directions

- **IC → manager.** An L40 senior IC is considering the management track. The usual reasons: they have been de-facto leading the team's people side informally and like it; they see a scaling bottleneck they want to help solve; a role has opened up and the IC is being considered. The pivot is a shape change, not just a level change. The management ladder at level 45 (this role) often sits on top of the general management canon at level 40 (`ai-augmented-team-manager`); the pivot's first stop is usually a team-manager role at the 40-level, with this level-45 role as the eventual target.
- **Manager → IC.** The under-named but common return trajectory. A manager who tried the role, found the craft parts they missed outweighed the people-side draw, or simply tested the role and preferred IC, returns to the IC ladder. The staff-plus IC altitude (L48, `agentic-systems-architect` / `principal-ml-engineer`) is often the destination. The return is not a failure; it is a legitimate career move and should be treated as one on the growth plan.

### The IC → manager growth plan

A pivot that works has three phases over six to twelve months.

**Phase 0 — the honest trajectory walk-through (before any commitment).** The IC and both managers (the current manager and the `ai-augmented-team-manager` partner) sit down and walk through what the role actually is. Fournier's chapter on the management transition in *The Manager's Path* is the canonical reading for this conversation; the AI-engineering adaptation is that the IC is giving up specific scope the AI-engineering ladder uniquely values (the trace reading at depth, the architecture call on an agent surface, the fine-tune methodology at its own altitude) in exchange for the management surface. Make the trade explicit. The question the walk-through asks: "which parts of your current work would you *miss* if they left your week next quarter?" The parts the IC answers specifically are the parts they should test the trade against before committing. Many pivots fail Phase 0 — the IC decides, honestly, that the trade is not what they want, and stays IC. That is not a failed pivot; it is a successful Phase 0.

**Phase 1 — stretch assignment as tech-lead or interim manager (months 1–4).** If Phase 0 lands "yes, let's try," the IC takes on a visible people-facing scope that does not formally re-level them. Common shapes: lead a two-person pod inside the team; take over the 1:1 cadence for two L20s under the manager's observation; co-chair the hiring loop for a specific role; co-run the retrospective and the planning rituals. The commitments are written on the growth plan; the `ai-augmented-team-manager` is a reading partner on the general management craft.

**Phase 2 — first management role (months 5–12).** A formal manager-of-a-small-team role, often at level 40 (`ai-augmented-team-manager`-adjacent) as the first stop, with the level-45 AI Engineering Manager role as the possible next step. The pivot at this phase is formal: the IC is now a manager. The growth plan from here is on the `ai-augmented-team-manager` track; this manager hands the growth sponsorship over to the general-management peer.

### The manager → IC return

A return to the IC ladder is a legitimate move the manager should design *before* the IC commits to the pivot, not after, so the IC knows the return is available. On the growth plan:

- **Phase 0 of the pivot includes the return clause.** "If, at the end of Phase 1, you prefer to return to IC, the staff-plus IC track is available; the technical scope you are continuing to maintain in Phase 1 is sufficient to re-anchor." The manager who wires this in advance is one whose ICs take the pivot seriously because the alternative is explicit.
- **The reverse direction has its own three phases.** A manager returning to IC rebuilds their technical anchor in a shape they left; the growth plan treats the first six months like a shape transition (Trajectory 2) applied to their original shape, with a staff-plus IC as the sponsor.

### Risk notes

- **The de-facto-manager-without-the-title trap.** An L40 IC who is de-facto managing without the title (running the hiring loop, mentoring across shapes, handling conflict) is often someone who "wants to be a manager" without having tested it. Phase 0 is the test; the growth plan names the stretch assignment as Phase 1 before the role change is committed.
- **The emergency pivot.** When a manager role opens suddenly and the team needs coverage, the pivot is often compressed. The growth plan should not do this if avoidable; if unavoidable, the compressed pivot should explicitly name what Phase 0 content is being skipped and build in a two-month check-in where the return clause is on the table.

## The cross-team rotation mechanics

Several of the trajectories above involve cross-team work: a cross-shape rotation into the eval team, a tech-lead stretch that includes the infra team's substrate, a manager-track pivot into a different sub-team.

### The two-manager contract

Any rotation that crosses teams requires a written contract between the sending and receiving managers. The contract is short, usually three paragraphs in a shared doc.

- **What the IC will do.** Specific scope, specific sponsor on the receiving side, dated start and end.
- **What the sending team gives up.** The IC's capacity on the sending team, named explicitly, with the sending manager's commitment to not reach for them mid-rotation.
- **What the receiving team provides.** Workstation, review time from the sponsor, inclusion in the receiving team's rituals, a specific stake of work the IC will own end-to-end.

### Reading-out cadence

A monthly joint 1:1 (sending manager + receiving sponsor + IC) is the minimum. The joint 1:1 reads the IC's trajectory from both sides; it prevents the drift where the IC is de-facto owned by one side and nominally by the other.

### The lateral move out

Occasionally a rotation produces the honest finding that the IC wants to transition fully to the receiving team. The growth plan should not pretend this is not possible. The two managers negotiate the transition as a headcount move; neither side pretends the question is still "will they rotate back." Being up-front about this early makes it easier, not harder, for the IC to make the right call.

## A concrete read: four ICs, four plans

Four ICs on the team, four written growth plans, four different trajectories.

- **IC-A — L30 applied, trajectory 1 (level-up within shape).** Twelve-month plan to L40 senior applied with the artefact-class gaps (mentorship across adjacent shapes, cross-team ADR, incident-command experience) specifically commissioned. Staff-plus sponsor: the `agentic-systems-architect`. Anti-atrophy: one unassisted design memo per month on a decision from the cycle. External signal: none this cycle; growth is inside-focused.
- **IC-B — L30 agentic, trajectory 2 (cross-shape rotation into eval).** Four-month rotation paired with the senior eval specialist, with Phase 1/2/3 commitments; the placement decision in week 17 will likely be "return to agentic with eval competence," which chapter 02's dual-shape read endorses. Anti-atrophy: deliberate practice on hand-scoring judge outputs weekly.
- **IC-C — L30 RAG, trajectory 3 (tech-lead stretch on the retrieval substrate).** Eight-month stretch targeting the retrieval substrate convention; cross-team scope includes the search team's corpus feed. Staff-plus sponsor: the architect. Specific tech-lead commitments include an ADR, chairing one cross-team retrieval review, and one incident-command rotation. Promotion to L40 at cycle-end 2026-Q4 is the target; the artefact-class grid will decide.
- **IC-D — L40 agentic, trajectory 4 (IC-to-manager pivot, Phase 0).** The honest trajectory walk-through is scheduled with the `ai-augmented-team-manager` peer; Phase 0 outcome will be "yes/no" by end of cycle. If yes, Phase 1 begins next cycle with a two-person pod lead role; the return-clause is written in advance. The growth plan is deliberately slow here — the walk-through is the whole first cycle's content.

Four plans, four specific trajectories, four named sponsors, four written documents. Every one is readable at cycle-end against what was committed; chapter 06 is specific about how calibration handles each fairly.

## Summary

- The growth plan is a short written artefact co-authored by the IC and manager, updated at review cycles. Six sections; the two AI-specific adaptations are the anti-atrophy element (chapter 05) and the credential-honesty element (chapter 07).
- Four named trajectories: level-up within shape; cross-shape rotation; tech-lead stretch; IC ↔ manager pivot. Each has a specific sequencing pattern and sponsorship model.
- The cross-shape rotation (trajectory 2) has a three-phase sequence over four months with two sponsors (sending manager, receiving specialist); the manager's job is to protect the rotation window from being interrupted by staffing emergencies.
- The IC ↔ manager pivot (trajectory 4) is co-owned with the `ai-augmented-team-manager` (level 40) peer and starts with Phase 0 — the honest trajectory walk-through — before any commitment. The return clause is written in advance so the IC takes the pivot seriously and the return seriously too.
- Cross-team rotations carry a written contract between the two managers; the joint 1:1 is the read-out cadence that prevents drift.

The next chapter turns from the trajectories to the anti-atrophy loop every growth plan in a frontier-tooling era carries: the deliberate-practice discipline against assistant-driven craft decay, the mandatory read-time against vendor cookbooks and release notes, and the rotations across model + framework shifts that keep an AI engineer's judgment current without hiding behind the assistant.
