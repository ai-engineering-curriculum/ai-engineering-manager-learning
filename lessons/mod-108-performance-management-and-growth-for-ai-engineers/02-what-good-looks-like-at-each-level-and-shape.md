# What "Good" Looks Like At Each Level and Shape

A level rubric that says "senior engineer" and nothing else is not a rubric; it is a mood. The first manager-owned artefact of this module is a level rubric specialised to the AI engineering surface — specific enough that an eval engineer at L30 and an applied-AI engineer at L30 both read it and recognise themselves, and specific enough that a candidate for promotion can produce the evidence in the artefact classes the rubric names. This chapter builds that rubric at each level anchor (L20, L30, L40, L48), then specialises the row-by-row read for each of the six AI-engineering IC shapes. It also pins the rubric to the public exemplars mod-102 chapter 04 anchors on so the language stays legible, not secret.

## Motivation

Three failure patterns the specialised rubric prevents.

- **Shape-blind level flattening.** The team's one-size-fits-all rubric reads "L40 means tech-lead for one team with mentorship and architectural judgment." That fits a `senior-agentic-ai-engineer` reasonably well. It does not fit a senior eval engineer who is leading the team's measurement contract across every shipping surface but has no architectural decisions that look like architecture decisions to the rubric-writer. The senior eval engineer reads the review and concludes the ladder has no room for them. They either shift to a shape the ladder sees (and the measurement discipline weakens on the team) or leave.
- **Hidden level-drift under model improvement.** The rubric was written eighteen months ago, when L30 applied-AI meant "shipped an LLM feature end-to-end with meaningful trace work." The frontier model generation that year made trace work half as hard — the models hold their context better, hallucinate less on the shape of feature this team ships, and route tool calls more reliably. L30 as written in the old rubric now describes work a new-hire L20 can do with a weekend. If nobody re-reads the rubric, promotions silently inflate: a quiet L30 takes two cycles to reach L40, not because the engineer grew, but because the rubric did not notice the baseline moved. Chapter 06 is specific about how calibration catches this; chapter 02 is specific about how the rubric is written to be re-readable.
- **Secret leveling language.** Internal shorthand ("that's clearly a stretch-L4," "this reads as an upper-L30 but not yet L40-ready") grows up next to the published rubric and replaces it. Candidates and new managers hear the shorthand but cannot find it in any document. Promotion velocity varies by manager; attrition rises among the engineers the shorthand under-counts. Mod-102 chapter 04 names this calibration bug on the hiring side; it is the same bug on the performance side.

The specialised rubric below is the antidote: dimensional, behaviour-worded, shape-specialised, publicly linkable, and re-read on a cadence.

## The rubric shape — three dimensions, five artefact classes

The published rubric reads on **three dimensions** (specialised from the Dropbox career framework and the Google L3/L4/L5 explainer) and **five artefact classes** (specialised to AI engineering).

The three dimensions:

- **Scope** — the area of the system the IC owns and is accountable for.
- **Autonomy** — the altitude of decision an IC is trusted to make independently.
- **Influence** — the span of people and work the IC changes beyond their own output.

These are the Dropbox / Google shape. If the rubric drifts far from a dimensional structure, the leveling language is heading toward opaque calibration.

The five AI-engineering artefact classes (carried from mod-102 chapter 04):

- **Traces reviewed** — reading the system's production behaviour; closing the eval → trace → fix loop.
- **Eval suites owned or contributed to** — the measurement contract.
- **Prompts / judges / fixtures registered** — the registry hygiene that makes the operating system compound.
- **Incidents led or responded to** — on-call command and the incident-to-fixture loop (mod-107 chapter 06).
- **Mentorship / cross-shape pairing** — the pull-up of less-experienced engineers, often across shapes.

The rubric at each level reads each dimension *through* the five artefact classes. "Senior at L40" is not "a title you earned"; it is "scope spans multiple ICs' work, autonomy is full on their sub-system, influence is tech-lead across adjacent shapes, traces-and-evals ownership is at the registry-conventions altitude, and the mentorship record is dated and demonstrable."

## L20 — on-ramp (`llm-application-developer`)

The on-ramp level. One IC shape — the on-ramp developer; other shapes have their first level at L30, with L20 being where someone *entering* the specialty from an adjacent background is placed while they ramp.

### The dimensional read

- **Scope** — a well-scoped slice of a larger feature; ships under senior guidance; the sub-feature, not the system.
- **Autonomy** — executes against a specified plan; identifies when to ask; does not yet judge trade-offs independently.
- **Influence** — none expected beyond their own work.

### The artefact-class read at L20

- **Traces reviewed** — reads traces for their own bugs; can walk through a two-turn trace and point at the model-choice step; does not yet pattern-match across the corpus of traces the team has seen.
- **Eval suites** — writes a basic eval case when their PR calls for one; does not yet design a judge or own the eval methodology for a feature.
- **Registered artefacts** — has one or two prompts in the registry; hygiene is adequate, not exemplary.
- **Incidents** — has shadowed one incident call; has not led; may have authored one small runbook update.
- **Mentorship** — none expected.

### The honest cadence note

For L20 engineers specifically, the "learn-without-the-assistant" loop (chapter 05) is a growth requirement, not a nice-to-have. L20 is the level where the craft is being built. If the engineer's throughput is entirely assistant-driven, the manager is looking at the atrophy of a craft that was never grown in the first place. Chapter 05 is specific.

## L30 — in-scope independent (five IC shapes)

Five IC shapes anchor at L30: `applied-ai-engineer`, `agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`. The dimensional and artefact-class anchors below are the shared baseline; the shape specialisations follow.

### The shared L30 dimensional read

- **Scope** — an entire feature end-to-end *or* a specialist slice (an eval suite, a retrieval pipeline, a fine-tune run) owned soup-to-nuts; the thing that ships because of them.
- **Autonomy** — operates independently in-scope; reaches for help on scope-crossing decisions; defends technical calls in review; makes model-choice and architecture calls for their scope with a documented rationale.
- **Influence** — mentors L20 engineers in their shape; influences one team; registry conventions improve because of them.

### The shared L30 artefact-class read

- **Traces reviewed** — reads hundreds of traces per cycle in-scope; pattern-matches across the corpus; closes the trace → eval → fix loop without being asked.
- **Eval suites** — contributes eval cases on every PR; owns one or more eval suites or sub-suites.
- **Registered artefacts** — registers one or more prompts, judges, or fixtures per sprint; registry hygiene is exemplary in their scope.
- **Incidents** — led at least one incident on their scope; produced a dated postmortem and the incident-to-fixture loop was closed (mod-107 chapter 06).
- **Mentorship** — meaningful L20 pairing; at least one L20 engineer has pulled up because of them.

### Shape specialisations at L30

The shared anchors above read differently through each shape's lens. The specialisation below names, for each shape, the one *unmistakable artefact* an L30 of this shape produces that an L30 of another shape does not.

- **`applied-ai-engineer` L30.** The unmistakable artefact is a *shipped feature with an eval delta* — a PR that moved the production surface, with before/after eval numbers, trace samples, a cost-per-call read, and a release-triad readiness note. Several per cycle. The integration craft at scope.
- **`agentic-ai-engineer` L30.** The unmistakable artefact is a *working multi-step agent with a failure-mode map* — a tool-invocation trace through a multi-turn interaction, a documented authorisation boundary, a HITL checkpoint discipline, and a kill-switch surface they own. One or two agent systems at scope.
- **`ai-eval-engineer` L30.** The unmistakable artefact is a *judge or rubric suite with a defended methodology memo* — the measurement contract for one or more product surfaces, with change history, label-provenance, judge-choice defence, and a documented confidence band on the passing threshold. The measurement contract at scope.
- **`rag-engineer` L30.** The unmistakable artefact is a *retrieval pipeline with recall/precision numbers on the golden set* — the chunking choice, the index decision, the re-ranker rationale, the corpus contract, the freshness SLO. The retrieval quality at scope.
- **`fine-tuning-engineer` L30.** The unmistakable artefact is a *checkpoint + model card + eval against baselines* — a trained model with the Mitchell-et-al. model-card documentation, eval results against baselines and the vendor API alternative, training-data contract, cost / latency / serving memo. The post-training craft at scope.

A promotion packet for L30 includes the unmistakable artefact for the shape. A candidate without one, or with one in a different shape, is either being placed in the wrong shape or is not yet at L30.

### The honest cadence note at L30

The L30 engineer is where the frontier-assistant-halo failure (chapter 01, chapter 05) is most common and most costly. They ship a lot of features; a lot of the mechanical work is assistant-authored; the review conversation is about whether the *judgment* behind the work is theirs. Chapter 03 walks the question set. The rubric at L30 should ask: when the model provider shifts next, which calls in their scope will still be good calls? If the answer is "none without the assistant re-doing the work," the engineer is at L30-throughput but L20-judgment; chapter 05 is the plan.

## L40 — tech-lead altitude (`senior-agentic-ai-engineer` and senior sub-ladder specialists)

L40 is the tech-lead level. The primary IC shape at L40 in the public ladder is `senior-agentic-ai-engineer`, where the published IC track teaches the craft at depth. Senior specialists in the other shapes exist and are valid; the convention in this track is that L40 depth for a non-agentic shape (a senior eval engineer, a senior RAG engineer, a senior fine-tuning engineer) is specialised from the `senior-agentic-ai-engineer` rubric with the shape-specific artefacts substituted. The manager owns the specialisation; the IC track is the depth-anchor.

### The dimensional read

- **Scope** — a sub-system that spans multiple features or multiple ICs' work; the agent's architecture, the eval registry's conventions, the retrieval substrate, the fine-tune methodology for the team; the incident surface for their sub-system.
- **Autonomy** — full technical autonomy on their sub-system; collaborates with the AI Engineering Manager on roadmap and headcount; collaborates with the staff-plus architect (`agentic-systems-architect`, `principal-ml-engineer`) on cross-team direction.
- **Influence** — tech-lead for the team's primary surface in their shape; mentors L20s and L30s *across adjacent shapes* (this is the point about influence at L40 that is specialised for AI engineering — a senior agentic engineer who has not pulled up anyone outside agent-specific scope is at L40-ceiling); is the on-call escalation for their sub-system.

### The artefact-class read at L40

- **Traces reviewed** — reads trace samples across the whole team's scope in their sub-system, not only their own; the on-call escalation point for trace-level forensics.
- **Eval suites** — owns the registry conventions in their sub-system; the methodology memos for cross-feature evals read off their work; the anti-drift calibration (mod-104 chapter 05) in their area is theirs to run.
- **Registered artefacts** — owns a *conventions document* for the registry in their sub-system (naming, versioning, change-review discipline) that the rest of the team reads from.
- **Incidents** — has led at least two incidents in the last twelve months; is an Incident Commander on the AI-side rotation (mod-107 chapter 04); the postmortems they ran produced permanent fixtures in the registry.
- **Mentorship** — dated and demonstrable mentorship record: at least two engineers (ideally across adjacent shapes) who pulled up with their direct involvement; written mentorship notes or a 1:1 record that reads out on it.

### The L40 specialisations by shape

- **`senior-agentic-ai-engineer` L40.** The unmistakable artefact is a *cross-team tool-contract standard or failure-taxonomy document* that other teams read from — not just the team's agent, but the shape of agent engineering across the organisation.
- **Senior eval specialist (L40, often titled "staff eval engineer" at larger companies).** The unmistakable artefact is a *registry convention + judge-methodology handbook* that the whole engineering organisation reads from; eval-delta artefacts for every major release gate pass through them.
- **Senior RAG specialist (L40).** The unmistakable artefact is a *retrieval substrate and a corpus-contract discipline* shared across the organisation; freshness SLOs and corpus-governance patterns the whole team follows.
- **Senior fine-tuning specialist (L40).** The unmistakable artefact is a *post-training playbook and a model-card convention* adopted across the team; the go/no-go memo for when to fine-tune vs. buy inference reads off their framework.

### The honest cadence note at L40

The L40 engineer is where the *assistant-halo* and the *scope-reach* failure modes diverge in diagnostic signal. A senior who ships prolifically with an assistant but has no dated mentorship record, no written conventions document, no cross-team artefact that another team reads from, is at L40-title and L30-reach. The gap is conversation-worthy in a 1:1 and conversation-worthy at calibration. The manager's job is not to hold back the promotion because "there's always more to do"; it is to name the specific missing artefacts and sequence the quarter that produces them.

## L48 — staff-plus IC (`agentic-systems-architect`, `principal-ml-engineer`)

L48 is the staff-plus altitude. Two public shapes anchor here: the agent-systems architect and the principal ML engineer. The IC tracks (`agentic-systems-architect-learning`, `principal-ml-engineer-learning`) are the depth anchors; this track places the roles rather than teaches them.

### The dimensional read

- **Scope** — cross-team technical direction; reusable primitives; standards; multi-quarter technical investment; the technical shape of the AI engineering function at the org level.
- **Autonomy** — sets technical direction in partnership with the AI Engineering Manager, the peer manager group, and the CAIO / head of engineering; is the escalation point for staff-level technical disputes.
- **Influence** — shapes multiple teams' work; grows the next generation of L40s; represents the technical direction externally when needed (chapter 07 is specific about what this reads like honestly).

### The artefact-class read at L48

- **Traces reviewed** — reads trace samples across the organisation on their technical surface; is the technical escalation for cross-team forensics.
- **Eval suites** — the methodology for cross-organisational eval standards reads off them; they commission methodology reviews (mod-104 chapter 05).
- **Registered artefacts** — architecture decision records (ADRs) are their primary written medium; technical direction documents the org reads from.
- **Incidents** — technical review on the most consequential incidents; the staff-plus write-up on an incident that the exec team reads often reads off them (mod-107 chapter 07).
- **Mentorship** — grows L40s into staff-plus; the cross-team mentorship record is multi-quarter.

### The honest cadence note at L48

Credential theatre (chapter 07) is a specific risk at L48. The staff-plus profile often includes external artefacts — conference talks, publications, open-source contributions — because the role's influence surface legitimately extends beyond the company. The honest read in review is: did the external artefact *also* move the inside? A principal MLE who publishes two papers but has produced no ADR the team references and mentored no L40 to L48 is performing the role externally without doing the work internally. Chapter 07 walks the specific test.

### The two L48 shape specialisations

- **`agentic-systems-architect` L48.** Owns the architecture of the agent systems the team builds; the cross-team tool-contract standard; the multi-quarter technical direction for agentic surfaces. The IC track is [`agentic-systems-architect-learning`](https://github.com/ai-engineering-curriculum/agentic-systems-architect-learning).
- **`principal-ml-engineer` L48.** Owns ML-methodology direction, deep eval-design judgement, and (for teams doing fine-tuning, preference data, distillation) the staff-plus technical partnership. The IC track is [`principal-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning).

A third shape sometimes crystallises — a "principal eval engineer" or "principal retrieval architect" — in teams whose eval or retrieval surface is unusually consequential. Mod-102 chapter 04 names the pattern-recognition for when a new staff-plus shape is forming; this chapter inherits it. If the staff-plus candidates in your pipeline do not fit the architect or principal MLE rubric cleanly, the ladder may need a third row.

## Reading the rubric: a grid, not a number

The published rubric is readable as a dimension × shape × level grid. The compact form for a single IC in a review looks like this:

| Artefact class | Current level (L30 agentic) | Below | At | Above |
|---|---|---|---|---|
| Traces reviewed | L30 agentic | | ✓ | |
| Eval suites | L30 agentic | | | ✓ |
| Registered artefacts | L30 agentic | | ✓ | |
| Incidents led | L30 agentic | | ✓ | |
| Mentorship | L30 agentic | ✓ | | |

The grid above shows a solid at-level L30 agentic engineer with one strength (registry hygiene) and one gap (mentorship). The review packet names the gap and the specific plan: in the next cycle, pair with one L20 on agent-side bug work, with a dated check-in at month three. This is the read mod-102 chapter 04 uses on the hiring side; it is the same read on the performance side.

The grid form is also what the calibration meeting (chapter 06) argues against. A promotion recommendation should land with the grid filled in; a dispute at calibration argues row by row.

## Publishing the rubric

The leveling rubric is a public document. The three concrete requirements:

- **Linkable.** The rubric lives at a URL (team handbook, internal wiki page) that is referenced in JDs (mod-102 chapter 02), in promotion packets, in 1:1 growth-plan docs. If engineers cannot find the current level anchors without asking their manager, the rubric is not published.
- **Versioned.** The rubric carries a visible version number and a change log. When calibration catches drift (chapter 06), the correction is dated and lives in the change log; "we tightened the L30 applied bar on 2026-04-15 because model regression X made the previous bar describe L20 work" is the audit trail that lets the team defend promotion decisions.
- **Behaviour-worded.** The rubric talks about *what the IC does*, not *how many years they have done it for* and not *what their title is*. "Owns the measurement contract for one or more product surfaces" is behaviour-worded. "Senior engineer with 5+ years" is not and should not appear anywhere in the rubric.

GitLab-handbook culture (mod-102 chapter 01) is the model.

## Anchoring to the public exemplars

The three public leveling frameworks named in mod-102 chapter 04 are the anchors; this chapter re-reads them against performance (not hiring).

- **Dropbox Engineering Career Framework.** The "scope / autonomy / influence" rubric above traces the Dropbox dimensional structure. The specialisation is that technical skills at L30 reads differently for applied vs. eval vs. RAG vs. fine-tune (the row-by-row read in this chapter), and that influence at L40 explicitly includes *mentorship across adjacent shapes*, not just within shape. Link: [dropbox.github.io/dbx-career-framework/](https://dropbox.github.io/dbx-career-framework/).
- **Google L3 / L4 / L5 / L6 / L7 shape.** The levels.fyi explainer describes the industry-standard shape of "needs guidance → independent in-scope → cross-scope tech-lead → cross-team architect → multi-org influence." L20 / L30 / L40 / L48 in this track map to L3 / L4 / L5 / L6–L7. The Google read for performance is that *the scope-you-own-alone statement* at each level is explicit; the per-shape "unmistakable artefact" in this chapter is the specialisation. Link: [levels.fyi/blog/google-swe-levels-explained](https://www.levels.fyi/blog/google-swe-levels-explained.html).
- **GitLab engineering handbook.** The GitLab pattern is used not for its levels (company-specific) but for its publication discipline. The rubric this chapter writes is maintained with the same handbook habit: linkable, behaviour-worded, versioned, open to engineers and candidates. Link: [handbook.gitlab.com/handbook/engineering/](https://handbook.gitlab.com/handbook/engineering/).

If the manager's rubric cannot be linked to publicly available anchors and specialised against them, the rubric is heading toward opaque calibration.

## A concrete read: four ICs, four grids

A team of eight runs a review cycle. The manager produces the compact grid per IC and reads it against the rubric. Four of the eight read like this (names omitted; shapes and levels real):

- **IC-A, L30 applied-AI, up for cycle review.** Grid: traces at, evals at, registered artefacts at, incidents at, mentorship above. Packet: at-level L30 with a strength in mentorship; the explicit growth target next cycle is to broaden into a second shape — pair into eval work with the eval specialist for a quarter. Chapter 04 is the rotation plan.
- **IC-B, L30 eval, up for promotion to L40 senior eval.** Grid: traces above, evals above, registered artefacts above, incidents at, mentorship *at* (one L20 across adjacent shapes). Packet: promotion-ready on four of five artefact classes; the gap is mentorship *breadth* — the mentorship is within shape, not across. The L40 anchor is specific about cross-shape mentorship at L40 influence. The recommendation is one more cycle with a documented cross-shape mentorship target; the promotion lands next cycle with the gap closed.
- **IC-C, L40 senior agentic, up for staff-plus consideration.** Grid: traces at, evals at, registered artefacts above (owns the conventions document), incidents at, mentorship at. Packet: solid at-level L40, no staff-plus gap evident but no staff-plus reach either — no cross-team ADR, no multi-team influence artefact, no mentorship of an L40 toward staff-plus. The packet is not "not ready for L48"; it is "L48 requires evidence of *beyond-team* influence; sequence a quarter that produces it." Chapter 04 and chapter 07 together design the quarter.
- **IC-D, L30 applied-AI, hired 18 months ago.** Grid: traces above, evals *below* (eval muscle never grew — they ship features but every PR is light on eval work), registered artefacts at, incidents below (has not led any), mentorship below. Packet: at-level L30 by title but not by artefact posture. The review is honest about the gap; the growth plan for next cycle names the specific eval-loop target and the incident-shadowing expectation, with monthly check-ins. The feedback shape (chapter 03) and the anti-atrophy loop (chapter 05) are both relevant.

Four ICs, four grids, four review conversations grounded in the specialised rubric. None of them required a secret internal shorthand; all of them could be defended at calibration row by row.

## Summary

- The specialised level rubric is the first manager-owned artefact of this module. It reads on three dimensions (scope, autonomy, influence) and five AI-engineering artefact classes (traces, evals, registered artefacts, incidents, mentorship).
- Four level anchors: L20 (on-ramp, one shape), L30 (in-scope independent, five shapes), L40 (tech-lead, specialised across shapes), L48 (staff-plus, two canonical shapes with a third occasionally crystallising).
- Each shape at each level has an **unmistakable artefact** — the one thing an IC of this shape at this level produces that an IC of another shape does not. A promotion packet without the unmistakable artefact is either in the wrong shape or not yet at level.
- The rubric is a grid, not a number; the dispute at calibration argues row by row.
- The rubric is **published, versioned, behaviour-worded**, and anchored to public exemplars (Dropbox, Google, GitLab). Secret leveling language is a calibration bug chapter 06 is specific about catching.

The next chapter turns from the rubric to the ritual: how the 1:1, feedback, and performance-review cadence is specialised to surface the AI-specific artefact classes and tell apart two common and easily-confused situations — a senior leaning on a frontier assistant without leading, and a junior leveraging the assistant responsibly to learn.
