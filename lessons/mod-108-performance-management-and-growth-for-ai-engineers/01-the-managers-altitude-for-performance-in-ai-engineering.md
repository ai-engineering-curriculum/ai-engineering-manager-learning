# The Manager's Altitude for Performance Management in AI Engineering

Performance management is one of the oldest disciplines engineering management teaches. 1:1s, written feedback, calibration meetings, performance-review cycles, promotion packets, performance-improvement plans — Camille Fournier, Andy Grove, Will Larson, Michael Lopp, Lara Hogan, Julie Zhuo, Kim Scott, and a generation of handbooks have produced the canonical practice, and `ai-augmented-team-manager` (level 40) is where this track assumes a reader learned it. This module does not re-teach that canon. It adapts it to the AI engineering surface, where the ladder has six distinguishable IC shapes, the toolkit is non-deterministic, the frontier model that defined last quarter's bar was deprecated this quarter, and the person being reviewed may have co-written half their shipped code with a frontier coding assistant.

This first chapter sets the altitude. What the AI Engineering Manager owns on the performance surface, what defers to other roles, which artefacts make up the module, and what this module deliberately does not teach.

## Motivation — four failure shapes when performance management has no AI-specific adaptation

The canonical practice is necessary and insufficient. Four failure shapes repeat in teams that run performance management against a generic engineering rubric on an AI engineering team.

- **The flat ladder.** The team's internal ladder says "Senior Engineer," "Staff Engineer," and nothing else. Everyone reviewed against that rubric receives the same narrative shape: owned scope, influenced direction, mentored juniors. But the person running the retrieval pipeline and the person running the eval harness are doing structurally different work, against different artefact classes, with different failure modes and different signals of excellence. The generic review tells the eval engineer "your scope is narrow" because their work does not look like applied feature-shipping, when in fact they are at scope-of-measurement for the entire team — a scope the applied engineers' artefacts quietly depend on. The eval engineer's work is systematically under-counted. Within a year they either disengage or leave.
- **The frontier-assistant halo.** A senior engineer is shipping four times what they shipped eighteen months ago. The pull-request throughput is unmistakable. Their 1:1s sound confident. The review draft writes itself: "top-of-level, exceeds expectations." The quieter reading: nearly every PR was authored end-to-end with a coding assistant; the engineer has not written an unassisted design document, debugged a trace by hand, or made a model-choice call with their own judgement in nine months. They are shipping someone else's pattern-matching at scale. The review that rates them on throughput misses the atrophy, and when the next model regression arrives the team will discover that the engineer who shipped the retrieval layer cannot explain why it was structured that way.
- **The counterfactual grading of a replaced model.** An engineer spent the quarter fine-tuning a specialist model that would have shaved serving cost by 40%. Three weeks before the release gate, the provider shipped a new base model that closed most of the quality gap for free. The fine-tune became unnecessary; the deliverable was shelved. The review draft reads: "did not ship." But the counterfactual the engineer was asked to deliver against *was shipping a 40% cost-saving fine-tune*, a judgement call the manager and the engineer made together at planning time against the then-current model landscape. Grading the engineer against a world they did not live in is unfair, demotivating, and (over time) selects for engineers who refuse to invest in anything that can be made obsolete by a vendor release — which is most interesting AI engineering work.
- **The silent credential drift.** Two engineers at the same level are evaluated for a staff-plus promotion. One spent the year shipping, mentoring, and writing internal architecture memos nobody outside the team has read. The other spent the year at four AI conferences, published two "state of the stack" blog posts, and collected a vendor certification in an area they do not work in. The second engineer's external profile is louder; the first engineer's contribution to the team's operating system is larger. If the manager cannot distinguish useful growth from credential theatre, the promotion goes to the louder profile and the team learns that external performance is the path.

All four failures look, at first glance, like calibration failures. They are. But the root is deeper: the generic-engineering review *has no AI-specific vocabulary* for what the work actually is, what growth in it looks like, or what the current technology drift makes fair. This module provides that vocabulary.

## Core concept 1 — what this module does not re-teach

Everything on the shortlist below is assumed, not taught, here. If the reader has not internalised it, the referral is `ai-augmented-team-manager` (level 40), where general engineering-management canon is this track's prerequisite.

- **The 1:1 practice** — cadence, agenda ownership, note-taking, the shape of a good 1:1 (Fournier's *The Manager's Path*, Hogan's *Resilient Management*).
- **Feedback mechanics** — timely, specific, situation–behaviour–impact (SBI), the difference between feedback and coaching, the radical-candor two-axis frame (Kim Scott), the hard-conversation rehearsal (Fournier, Lopp).
- **The performance-review cycle** — half-year or annual cadence, self-review, peer feedback, calibration meeting, delivered packet, follow-through (Grove's *High Output Management* for the OKR substrate; GitLab handbook for the open written rubric).
- **Promotion packet mechanics** — how to document scope and impact, how to pre-wire with peers, how calibration meetings run (Larson's *An Elegant Puzzle* and *Staff Engineer*).
- **The hard end** — performance-improvement plans, exits, and the separation of performance from personhood (Lopp's *Managing Humans*; the Dekker "just culture" reading that mod-107 chapter 05 grounds in).

If any of the above are unfamiliar, read them before reading this module. Everything here assumes the mechanics are in your hands and asks: *given those mechanics, how do you specialise them to the AI engineering surface?*

## Core concept 2 — the four manager-owned artefacts

The rest of this module builds four manager-owned artefacts. They map one-to-one to the learning objectives and to the chapters.

1. **The AI-engineering-specific level rubric (chapter 02).** What "good" looks like at L20, L30, L40, and L48 *per shape* — applied, agentic, eval, RAG, fine-tune, and the staff-plus altitudes (`agentic-systems-architect`, `principal-ml-engineer`). Not a free-standing document authored by this manager alone: it specialises the public leveling framework the hiring loop in mod-102 anchors on (Dropbox / Google / GitLab) with AI-shape specialisations. The manager owns the specialisation, not the base framework.
2. **The 1:1 / feedback / review cadence specialised to AI craft (chapter 03).** The 1:1 agenda template that surfaces the AI-specific artefact classes (traces, evals, registered prompts/judges, incidents, fixture additions, on-call command), the SBI-shape feedback library for AI-specific behaviours, and the review-packet template that an AI engineer's work actually reads out on. Includes the *honest-cadence split* between a senior who is leaning on a frontier assistant without leading and a junior who is using one responsibly to learn — two different situations, two different feedback shapes, both common.
3. **The growth-plan surface across the sub-ladders and the IC↔manager pivot (chapter 04).** The manager-owned artefact that lets an engineer move legibly between applied / agentic / eval / RAG / fine-tune, between IC and tech-lead altitude, between IC and manager track. Includes the anti-atrophy rotation plan (chapter 05) wired into the growth-plan template.
4. **The drift-fair calibration process (chapter 06).** The calibration ritual — borrowed from mod-102 chapter 04's hiring calibration — adapted to performance review, with the specific adaptations that handle rapid technology drift fairly: the counterfactual-at-planning-time principle, the shipped-against-replaced-model note, the model-generation-change re-read of the rubric, the published correction when drift is caught.

A reading discipline threads through all four: the **credential-honesty read** (chapter 07) — how to weight conferences, publications, open-source contribution, and vendor certifications in a review without rewarding credential theatre.

A team has a **manager-owned performance surface** when: the level rubric is published and specialised per AI-engineering shape; every IC's 1:1 surfaces the five AI-specific artefact classes on a recurring cadence; every IC has a growth plan that references cross-shape rotation targets; the last review cycle's calibration meeting produced dated corrections to the published rubric; and every promotion packet from the last twelve months reads against the AI-specific rubric, not a generic engineering template.

## Core concept 3 — what the IC owns, what the manager owns

The pattern from the rest of the track repeats here. The IC owns craft; the manager owns the system in which craft is made legible.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Doing the work that produces the artefacts — the shipped feature, the eval suite that holds the line, the retrieval pipeline that meets its precision target, the fixture added after the incident. Across `applied-ai-engineer`, `agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`, `senior-agentic-ai-engineer`. | Specifying **what "good at each level looks like"** per shape; keeping that rubric current against model drift (chapter 02); running the calibration meeting that places ICs against it fairly (chapter 06). |
| Showing up to 1:1s with the agenda, the questions, the blockers; accepting feedback; practicing the craft the feedback points at. | Running the 1:1 and feedback cadence; writing the review packet; delivering the message; following through on commitments. The AI-specific adaptation is on the manager's side (chapter 03). |
| Driving their own growth: naming the sub-ladder they want to move toward, taking the rotation, doing the reading, practising without the assistant when the growth plan calls for it. | Co-authoring the growth plan with the IC, sequencing the opportunities, protecting the time for deliberate practice, partnering with the staff-plus IC or peer manager for cross-team rotations. Chapter 04. |
| Investing in external signal (conference talks, OSS contribution, publication) when the growth plan or the IC's own interest calls for it; being honest about which activities move craft versus merely performing it. | Reading external signal honestly in reviews; distinguishing useful growth from credential theatre; calibrating the weight that external artefacts carry in promotion (chapter 07). |
| Owning the shape of their own assistant usage on the day — when to accept the completion, when to rewrite by hand, when to turn the assistant off and think. | Designing the anti-atrophy rotation plan; requiring deliberate "learn without the assistant" time; providing the reading time against vendor cookbooks and release notes; making the pattern visible on the team (chapter 05). |

The pattern: *the IC produces the artefact; the manager builds the rubric and the ritual in which the artefact is legible, fair, and compounds.*

## Core concept 4 — the four deferrals

Four roles the AI Engineering Manager explicitly defers to on this module. Each has standing authority the manager does not.

### `ai-augmented-team-manager` (level 40) — the general management canon and the IC↔manager-track transition

The sibling-below in the AI Workforce family. Owns the general engineering-management operating system this module assumes: 1:1 practice, feedback mechanics, the review cycle, promotion-packet discipline, PIP mechanics. This module is built on top of that. The one surface where the two manager roles explicitly partner: the IC-to-manager-track transition (chapter 04). When an L40 AI engineer is considering a management move, the 40-level sibling is the right partner for the *management* side of the growth plan (what the role is, what the first ninety days look like, how the craft of management is learned), while this track's manager owns the *AI-engineering* side (what scope they are stepping away from, what specialisation they are keeping or dropping, what pattern they will leave with the team). Chapter 04 is specific about the handoff.

### Human Resources / People Partners — PIP mechanics, exits, compensation, legal binding

Any performance-improvement plan, any exit, any compensation decision carries legal and process obligations the manager does not own alone. HR owns the PIP template, the documentation requirements, the exit paperwork, and the compensation-band policy. The manager owns the craft of delivering the message, writing the specific behaviours, and following through. On an AI team the HR partner may be unfamiliar with the shape of the roles (what does "an eval engineer exceeds expectations" look like?); the manager's job is to translate the AI-specific artefact language into the behaviour-level language the HR template needs. Partner with HR early on any hard-end case; do not freelance.

### The staff-plus ICs (`agentic-systems-architect`, `principal-ml-engineer`) — technical judgment on an IC's work at depth

The staff-plus IC partners of this manager (mod-101 chapter 01) are the right technical readers for a promotion packet involving deep craft. When an L30 is being considered for L40, or an L40 for L48, the staff-plus IC's reading of their architecture decisions, their technical direction, their eval methodology, or their fine-tune methodology is the signal the manager's review alone cannot carry. The staff-plus IC does not run the review (the manager does); they *testify* to the technical level of the work. The manager commissions the testimony early, in writing, and reads it against the rubric.

### The peer manager (`ai-infra-team-lead`) and cross-functional partners — the growth-plan rotations

Growth plans that cross sub-ladders (chapter 04) often cross teams. A promising L30 applied engineer may need a quarter rotation into the infra team to learn the serving substrate; an L40 agentic engineer may need to shadow the security operations on-call to learn adversarial thinking at depth. Those rotations are co-owned by the two managers (host and sending), with the receiving staff-plus IC as the craft sponsor. The sending manager proposes the rotation; the receiving manager commits capacity; neither manager holds the whole relationship alone. Chapter 04 walks the mechanics.

A pattern to internalise: *the manager is the connective tissue* for an IC's growth. Nobody else is tracking across all four deferrals at once — HR for the hard-end mechanics, the general-management peer for the IC-to-manager pivot, the staff-plus IC for technical testimony, the peer manager for cross-team rotation. The manager runs the single-point view that makes the IC's career legible to the system.

## Core concept 5 — what this module does not teach

Three deliberate exclusions, each a referral.

- **The craft of the IC role itself.** This module does not teach how to write a judge, how to design a retrieval pipeline, how to run an SFT loop, or how to build a tool-contract. Each IC track teaches its own craft (`applied-ai-engineer-learning`, `ai-eval-engineer-learning`, `rag-engineer-learning`, `fine-tuning-engineer-learning`, `agentic-ai-engineer-learning`, `senior-agentic-ai-engineer-learning`). The manager does not need to be a specialist; the manager needs to *recognise what good looks like* at each level of each specialty, which is the chapter 02 work.
- **The HR-process substrate.** This module does not teach PIP paperwork, exit interviews, compensation band structure, or the legal side of a performance case. Partner with HR for the substrate; the manager's job is to feed it the AI-specific behavioural content.
- **The management craft at base.** This module does not re-teach how to have a hard conversation, how to write a review, or how to run a calibration meeting at the general-engineering altitude. The `ai-augmented-team-manager` (level 40) track is the prerequisite; the Fournier / Grove / Larson / Lopp / Hogan canon is the base reading.

What the manager must know at depth from this module: the **vocabulary** of AI engineering levels and shapes (L20 through L48 across six IC shapes); the **artefact classes** that make AI engineering work legible (traces, evals, registered prompts/judges, incidents led, fixtures added, on-call command); the **drift patterns** that affect fair calibration on this surface (model-generation shift, specialty crystallisation, assistant throughput inflation, counterfactual-at-planning-time); and the **habit** of specialising a published leveling framework rather than inventing an opaque internal language.

## The shape of the chapters ahead

- **Chapter 02 — What good looks like at each level and shape.** L20 → L48 anchors per shape (applied, agentic, eval, RAG, fine-tune, senior agentic, staff-plus); the artefact-class reads per level; the shape-specialisation note; how the rubric is published.
- **Chapter 03 — The 1:1, feedback, and performance-review cadence specialised to AI craft.** The AI-specific 1:1 agenda; the feedback library specialised to AI-engineering behaviours; the review-packet template; the honest-cadence split for frontier-assistant-leaning seniors vs. assistant-leveraging juniors.
- **Chapter 04 — Growth plans across the sub-ladders and the IC↔manager pivot.** Cross-shape growth plans; the rotation mechanics; the tech-lead track; the IC-to-manager pivot (co-owned with the `ai-augmented-team-manager`); the manager-to-IC pivot (common, under-named).
- **Chapter 05 — Anti-atrophy against frontier-assistant over-reliance.** The atrophy failure mode; the rotation surface across model + framework shifts; mandatory read-time against vendor cookbooks and release notes; the deliberate "learn without the assistant" loop; how to make the pattern visible on the team.
- **Chapter 06 — Fair calibration under rapid technology drift.** The counterfactual-at-planning-time principle; the shipped-against-replaced-model note; the model-generation change re-read of the rubric; the calibration ritual's AI-specific adaptations; published corrections; the drift-patterns to catch.
- **Chapter 07 — Credential honesty.** What external signal (conferences, publication, open source, vendor certifications) actually tells you about an AI engineer; what is useful growth; what is credential theatre; how to weight external artefacts in review and promotion.

## What good looks like

A team has a manager-owned performance surface when:

- The AI-engineering level rubric is published and specialised per shape; every IC can read their current level against it without a secret-language translation.
- Every IC's 1:1 surfaces the five AI-specific artefact classes on a recurring cadence (chapter 03); feedback in the last six months is specific to AI-engineering behaviours, not to generic engineering shorthand.
- Every IC has a growth plan with a named target (next level, or a cross-shape rotation, or a tech-lead / manager pivot); the plan was co-authored in the last two cycles and references concrete rotations or practice loops.
- Every IC's growth plan contains an anti-atrophy element — a specific "learn without the assistant" commitment and a reading loop against vendor cookbooks / release notes.
- The last review cycle's calibration meeting produced a dated correction to the published rubric when drift was caught; the correction is visible to future managers and candidates.
- Every promotion packet from the last twelve months reads against the AI-specific rubric; the staff-plus IC testimony is wired in for L30→L40 and L40→L48 promotions.
- External credentials (conferences attended, publications, OSS) are weighted in reviews against a published read (chapter 07) that distinguishes useful growth from theatre.

## What bad looks like

- The team uses a generic engineering ladder; "Senior Engineer" means the same thing to the retrieval specialist and the applied-feature generalist, and both quietly know the review language does not describe their work.
- 1:1 notes from the last six months are generic (project status, blockers, vacation) and never reference traces, evals, registered artefacts, incidents, or fixture additions.
- Nobody on the team has a written growth plan; growth is conversational and resets every cycle.
- The senior engineer shipping four times what they shipped eighteen months ago is rated on throughput; nobody has looked at whether the craft underneath is growing or atrophying.
- The engineer whose fine-tune was shelved by a provider release is rated "did not ship" without a note about the counterfactual the plan was made against.
- The promotion goes to the engineer with the louder external profile; nobody can articulate how it compares to the quieter engineer's actual contribution to the team's operating system.
- The team has had a model-generation shift in the last twelve months; the level rubric has not been re-read; the quiet rubric drift means L30 now describes work that would have been L20 two years ago, and promotion velocity falls without an explanation.

## Summary

- Performance management for AI engineers is the canonical management discipline specialised to a surface with six distinguishable IC shapes, non-deterministic artefacts, rapid technology drift, and a frontier assistant that can confuse throughput with craft.
- The manager owns four artefacts: the AI-engineering level rubric, the 1:1 / feedback / review cadence specialised to AI craft, the growth-plan surface across sub-ladders and the IC↔manager pivot, and the drift-fair calibration process. A reading discipline (credential honesty) threads through all four.
- Four deferrals: the `ai-augmented-team-manager` for the general management canon and the IC↔manager pivot; HR for PIP mechanics and exits; the staff-plus ICs for technical testimony on promotion packets; the peer manager for cross-team rotations. The manager is the connective tissue.
- What this module does not teach: the IC craft (owned by each IC track), the HR substrate (owned by People), the base management canon (owned by `ai-augmented-team-manager`).

The next chapter opens the rubric: what "good" looks like at L20, L30, L40, and L48, specialised for each of the six AI-engineering shapes.
