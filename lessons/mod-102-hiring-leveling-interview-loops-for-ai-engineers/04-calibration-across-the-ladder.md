# Calibration Across the AI Engineering Ladder

A level is a shared contract: "we all mean the same thing when we say L30 for an applied-AI engineer." Calibration is the ongoing work of making that contract real — placing candidates on the ladder consistently, keeping the ladder honest as the market shifts, and refusing to let a secret internal language replace the public rubric. This chapter sets the calibration mechanics for L20 → L40 across the AI-engineering IC ladder, extends them up to the staff-plus altitude, and names the drift patterns that eat calibration if you let them.

## Motivation

Without calibration, four things go wrong at once:

- **Inconsistent offers.** Two candidates with similar artefact posture are given different levels because they interviewed with different loops at different weeks.
- **Secret leveling language.** The company starts talking about "L4" and "L5" in internal shorthand that is never published anywhere. New managers cannot onboard; candidates cannot self-assess; external hiring becomes a lottery.
- **Title drift.** Two years in, half the team is "Senior AI Engineer" because the title is used for recruiting but never defended against a rubric. Promotions stop meaning anything; new senior hires discover the bar is not what they thought.
- **Model-driven level inflation.** The industry gets better at LLM features; the team's L30 bar from eighteen months ago now describes an L20. Nobody re-levels the rubric; the team quietly stops promoting and attrition rises.

Calibration is the mechanism that prevents all four. It is boring, it is procedural, and it is load-bearing.

## What calibration is, mechanically

Calibration is three distinct practices, run on different cadences:

1. **Per-candidate calibration.** At the end of every loop, the hiring manager plus the interview panel place the candidate at a level against the published rubric. Not "pass/fail" — a level and a confidence.
2. **Rolling pipeline calibration.** Every two to four weeks, the hiring managers across the AI engineering function meet, pull the last N offers, and ask: did we level consistently? The output is a correction (we rated two L30s too high; next loop we tighten) or a rubric update.
3. **Annual ladder calibration.** Once a year (or on major market shifts — frontier model generation change, a new specialty crystallising into a role), the entire AI-engineering ladder rubric is re-read against what the roles now actually do, and updated with a dated version history.

The rest of this chapter walks each practice.

## Per-candidate calibration: the artefact-and-behaviour debrief

The debrief that happens at the end of a loop is where leveling either works or fails. The habit that makes it work is simple: **every level claim is defended by an artefact or a behaviour from the loop, not by vibe.**

### The debrief script

For each round, the round owner says:

- "The rubric for this round at this level was [read the published rubric for the level we are hiring at]."
- "What I saw was [specific thing the candidate did or produced]."
- "I would place them at [level] because [what the artefact/behaviour implies against the rubric]."

The hiring manager's single most important question is **"what evidence is that?"** Every time someone says a level without an artefact behind it, ask. Three cycles of that question in a debrief is enough to make the whole room do it unprompted.

### The leveling grid

The final placement is a grid, not a number. For each of the five artefact classes from chapter 02 — traces reviewed, eval suites, prompts/judges registered, incidents led, mentorship scope — the candidate lands at one of {below, at, above} for the target level. A candidate who is "at" or "above" on four of five is at level with confidence; a candidate who is "below" on two or more is below level; a candidate who is "above" on all five is above level and the conversation is about offering up.

| Artefact class | Target level | Below | At | Above |
|---|---|---|---|---|
| Traces reviewed | L30 | | ✓ | |
| Eval suites | L30 | | ✓ | |
| Prompts/judges registered | L30 | | | ✓ |
| Incidents led | L30 | ✓ | | |
| Mentorship scope | L30 | | ✓ | |

The example above shows an at-level L30 candidate with one gap (incident experience) and one strength (registry hygiene). The offer is at level; the ramp plan explicitly puts incident shadowing in the first 90 days.

### The role-matrix placement

Level is only half the call. The other half is **shape**: which row in chapter 01's matrix fits. A candidate who is at L30 on the five artefact classes could legitimately be offered as applied, agentic, eval, or RAG — but one shape will fit their artefact portfolio better than the others. The debrief places shape *and* level. An offer that gets this wrong miscasts the candidate into a job they will underdeliver on.

## Rolling pipeline calibration: catching drift between loops

Per-candidate calibration keeps a single offer honest. Rolling calibration keeps a pipeline of offers honest against each other.

### The cadence

Every two to four weeks, the AI-engineering hiring managers meet for 60 minutes with the last N (typically 5–10) completed loops on the table. For each:

- What level was the offer at?
- What were the artefact-class ratings from the debrief?
- What did the candidate actually produce in the loop that justified the level?

The question the meeting answers: **when we said L30 this quarter, did we mean the same thing each time?**

### Common drifts you will catch

Over a year of running this, four drift patterns recur. Each one has a specific correction.

- **Shape drift.** You offered three "L30 applied-AI engineer" slots in a quarter and two of them are actually running as eval engineers because the team's work shifted. The correction: tighten the role-matrix placement in the debrief, or (better) acknowledge the shape shift and re-publish the JD mix.
- **Level inflation under tight market.** In a quarter where candidates are scarce, the bar quietly moves down. The correction: revisit the "below / at / above" ratings on recent loops and ask whether you would still extend the same offer with a slower clock; if no, the hiring bar needs a defended re-write, not a quiet slide.
- **Round over-weighting.** One round (often §5 system design, sometimes §4 debugging) ends up dominating offers — candidates who did well there got offered over candidates who did better on §1 and §3. The correction: weight the rubric explicitly, publish the weights.
- **Secret-language drift.** The debrief starts using internal shorthand ("that was a classic L3.5") that is not in any published rubric. The correction: pause the debrief, re-read the public rubric aloud, strip the shorthand. If the shorthand captures a real concept not in the public rubric, add it to the public rubric with a version note.

### The correction is publication, not punishment

When calibration finds a problem, the fix is to **publish the correction** — updated rubric line, updated role-matrix note, published offer-letter template change. Internal corrections that never get written down do not stick. The GitLab-handbook pattern (mod-102 chapter 02) is the default: calibration outputs land in a public document that candidates and future managers can read.

## Annual ladder calibration: re-reading the rubric

Once a year, pull the full AI-engineering ladder and ask one question: **do the levels still describe the work the roles are actually doing?**

Triggers that force an annual review earlier:

- A frontier-model generation shift (GPT-5-class, Claude 5-class, Gemini 3-class). The baseline capability moved; the baseline for "what L30 builds" probably moved with it.
- A new specialty crystallising. Two years ago "eval engineer" was unusual; now it has its own IC track. If your ladder has not absorbed it, the role gets squished into "applied engineer" and your calibration fails.
- A recurring complaint from candidates or recent hires that the ladder language does not match the work. That is market-level signal.
- A pattern of staff-plus candidates who do not fit either `agentic-systems-architect` or `principal-ml-engineer` cleanly. That means the architect-vs-MLE line needs re-drawing or a third staff-plus shape is forming.

The output of an annual review is a dated, versioned update to the rubric. Keeping the version history visible is calibration's most under-appreciated move — it is the audit trail that lets you say "we moved the L30 bar on this date, for this reason."

## Rubric anchors: L20 → L48 across the ladder

The public rubric specialises three dimensions at each level: **scope, autonomy, and influence**. The table below gives the anchor at each level across the AI-engineering IC family. Specialisation by shape (chapter 01) layers on top — a `rag-engineer` at L30 reads "scope" through the retrieval-quality lens, an `ai-eval-engineer` at L30 reads "scope" through the measurement lens.

### L20 — `llm-application-developer`

- **Scope:** a well-scoped slice of a larger feature; ships under senior guidance.
- **Autonomy:** executes against a specified plan; can identify when to ask.
- **Influence:** none expected beyond their own work.
- **Artefact posture:** small feature PRs, one prompt in the registry, trace-reading muscle for their own bugs.

### L30 — applied / agentic / eval / RAG / fine-tune (in-scope independent)

- **Scope:** an entire feature end-to-end, or a specialist slice (an eval suite, a retrieval pipeline, a fine-tune run) owned soup-to-nuts.
- **Autonomy:** operates independently within scope; reaches for help on scope-crossing decisions; defends technical calls in review.
- **Influence:** mentors L20 engineers in their shape; influences one team.
- **Artefact posture:** hundreds of traces reviewed weekly, eval cases on every PR, one or more registered artefacts per sprint, one or more incidents led, meaningful L20 pairing.

### L40 — `senior-agentic-ai-engineer` (tech-lead altitude)

- **Scope:** a sub-system that spans multiple features or multiple ICs' work; the agent's architecture; the registry conventions; the incident surface.
- **Autonomy:** full technical autonomy on their sub-system; collaborates with the AI Engineering Manager on roadmap and headcount; collaborates with the staff-plus architect on cross-team direction.
- **Influence:** tech-lead for the team's agentic surface; mentors L20s and L30s across adjacent shapes; is the on-call escalation.
- **Artefact posture:** failure-mode maps, kill-switch surfaces, registry conventions, mentorship record, incident command record.

### L48 — `agentic-systems-architect` / `principal-ml-engineer` (staff-plus IC)

- **Scope:** cross-team technical direction; reusable primitives; standards; multi-quarter technical investment.
- **Autonomy:** sets technical direction in partnership with the AI Engineering Manager and the CAIO / head of engineering; escalation point for staff-level technical disputes.
- **Influence:** shapes multiple teams' work; grows the next generation of L40s; represents the technical direction externally when needed.
- **Artefact posture:** architecture decision records, technical direction documents, standards owned, staff-plus mentorship record. (Depth belongs to the IC tracks [`agentic-systems-architect`](https://github.com/ai-engineering-curriculum/agentic-systems-architect-learning) and [`principal-ml-engineer`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning); this track places those roles rather than teaches them.)

## Anchoring to public exemplars

The three public leveling frameworks named in chapter 01 remain the anchors; this chapter uses them differently.

- **Dropbox dimensions.** The "scope / autonomy / influence" rubric above maps directly to Dropbox's dimensions (specifically project management + technical skills + people). If your rubric drifts far from a dimensional structure, you are heading toward opaque leveling. The [Dropbox framework](https://dropbox.github.io/dbx-career-framework/) stays linkable in the public rubric as the anchor.
- **Google L3 / L4 / L5 shape.** The [levels.fyi Google explainer](https://www.levels.fyi/blog/google-swe-levels-explained.html) describes a well-known shape of "needs guidance → independent in scope → cross-scope tech-lead" progression. The L20 / L30 / L40 anchors in this track trace that shape.
- **GitLab handbook culture.** The [GitLab engineering handbook](https://handbook.gitlab.com/handbook/engineering/) is the anchor for *how* the rubric is published and maintained: linkable, versioned, behaviourally-worded, visible to candidates.

Linking to these in your own public rubric makes it legible to candidates arriving from elsewhere and prevents the secret-leveling drift.

## No secret leveling language

The single most important calibration rule: **the only language the team uses to talk about level is the published rubric.** Three concrete prohibitions:

1. **No "half-levels" that are not published.** "L3.5" and "senior-but-not-staff" belong in the published rubric or not at all.
2. **No invisible gating criteria.** "You need to have led an incident to be promoted to L40" is a real criterion; it goes in the published rubric.
3. **No oral promotion rules.** Promotion criteria that live only in the manager's head grade inconsistently and legally expose the company. Write them down.

If calibration discovers that the team *does* mean something specific when it uses internal shorthand, the response is to either formalise the concept in the public rubric or stop using the shorthand. One of the two. Not both, not neither.

## A concrete read: a disputed debrief

A debrief is at an impasse. Two interviewers say "strong L30 applied." One says "L30 but shape is agentic, not applied." One says "L40 ready — their system-design round was obviously senior."

The hiring manager's moves, in order:

1. **Re-read the rubric aloud** for L30 applied vs. L30 agentic vs. L40. The room hears the actual behavioural anchors.
2. **For each gap,** ask the L40 advocate: *what did the candidate do that an L30 agentic would not have done?* The answer must reference the round. If the answer is "they just seemed senior," that is a vibe, not a level.
3. **For the shape dispute,** ask the agentic advocate: *which rounds did the candidate's artefact portfolio lean agentic?* If §5 system design went agent-heavy because the candidate took it there, that is signal; if the loop was applied-shaped and the dispute is résumé-driven, grade on the loop.
4. **Place with artefacts.** The grid gets filled in; shape and level land. The offer goes out with a documented debrief; the next calibration meeting (two to four weeks) reads it and either confirms or corrects.

The failure mode the manager prevents is the "strong intuition" offer. An L40 offer that cannot be defended in the grid is an offer the team cannot defend in the first quarter either.

## Summary

- Calibration is three practices on three cadences: per-candidate debrief (every loop), rolling pipeline review (every 2–4 weeks), annual ladder re-read.
- Per-candidate calibration turns level into a defended grid across five artefact classes and the role matrix.
- Rolling calibration catches four drift patterns: shape drift, level inflation, round over-weighting, secret-language drift. The correction is publication, not punishment.
- Annual calibration re-reads the whole rubric against the work the roles actually do; frontier-model generation shifts and new specialties are triggers.
- Anchors stay public: Dropbox-style dimensions, Google-style scope-autonomy-influence progression, GitLab-style publication discipline.
- The only language the team uses to talk about level is the published rubric. Secret shorthand is a calibration bug.

The next chapter turns from the loop-and-calibration machinery to the top of the funnel: where candidates come from, how to build a pipeline that does not collapse to one source, and how to avoid the trap of hiring one "AI-first-ever" superstar who refuses to work the eval and incident surface.
