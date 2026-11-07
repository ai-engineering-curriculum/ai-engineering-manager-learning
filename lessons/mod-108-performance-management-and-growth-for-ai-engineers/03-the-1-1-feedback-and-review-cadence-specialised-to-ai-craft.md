# The 1:1, Feedback, and Performance-Review Cadence Specialised to AI Craft

The level rubric in chapter 02 is the artefact an IC is read against. This chapter is the ritual that produces the read. 1:1s, feedback, and the performance-review cycle are not re-taught here — the Fournier / Grove / Larson / Lopp / Hogan / Scott canon is this module's prerequisite (chapter 01). What this chapter adds is the AI-engineering specialisation: an agenda template that surfaces the artefact classes the rubric reads on; a feedback library in SBI (situation–behaviour–impact) shape wording the behaviours specific to AI engineering; a performance-review packet template an AI engineer's work actually reads out on; and the **honest-cadence split** between two situations that look similar on throughput charts but require different feedback shapes — a senior who is leaning on a frontier assistant without leading, and a junior who is leveraging the assistant responsibly to learn.

## Motivation

Three failure patterns the specialised cadence prevents.

- **Generic 1:1 drift.** The team has a 1:1 cadence; the notes from the last six months read "sprint status, blocker on infra, vacation, roadmap question." Nothing in the notes references a trace, an eval delta, a registered prompt, a fixture addition, or an incident. The 1:1 is a project-status meeting. The review conversation at cycle-end discovers that neither the manager nor the IC has been reading the IC's work at the altitude the rubric reads on; the level discussion is a surprise.
- **SBI feedback without AI-engineering vocabulary.** The manager is giving feedback in good SBI shape ("in yesterday's review, I noticed you shipped the PR without the eval delta attached; the impact was the release gate chair had to ask for the number mid-meeting"). But because the feedback library the manager is drawing from is generic-engineering, the AI-specific behaviours — judge choice without a methodology memo, prompt registry hygiene, trace sampling cadence, on-call command craft — never surface as feedback. The IC grows on generic-engineering dimensions and plateaus on the AI dimensions. The quarterly review surprises everyone.
- **The misdiagnosed assistant pattern.** Two engineers ship a comparable PR volume. One is a senior who has quietly stopped reading a trace without the assistant, stopped reasoning through a judge-choice without the assistant, and ships what the assistant drafts with light editing. The other is a junior who is pairing with the assistant on every PR, but can walk through the trace unassisted when asked, articulates their judge choice when questioned, and corrects the assistant when the suggestion is off. On raw throughput they look identical; on 1:1 questioning they are opposite. If the manager cannot tell them apart, the senior gets a promotion they are not growing into and the junior gets feedback that assumes atrophy where there is none.

The ritual in this chapter is the vocabulary and the diagnostic the manager runs weekly.

## The AI-specific 1:1 agenda template

1:1s inherit their base shape from the Fournier / Hogan / Grove reading: short cadence (weekly or fortnightly), owned by the IC on agenda but structured by the manager, protected on calendar, written up in a shared doc the IC has read access to. Mod-101 chapter 02 connects to the operating-contract substrate.

What this module adds is a **rolling agenda spine** that surfaces the five AI-specific artefact classes on a predictable cadence. The spine does not take over every 1:1 — a 1:1 that becomes entirely a rubric check is a bad 1:1 — but it guarantees that over six-to-eight 1:1s a cycle, every artefact class has been read.

### The spine (one rolling item per 1:1)

Each 1:1 dedicates one agenda slot — ten to fifteen minutes — to a rolling **artefact-class check**. The rotation:

- **Week 1 — traces.** "Walk me through a trace from this week that surprised you." Not "what did you ship"; what did the production surface do that you didn't expect, and what did the trace tell you? The question is diagnostic for the trace-reading muscle (chapter 02 artefact class 1). An IC who has not read a surprising trace this week is either not reading enough traces or is not surprised by traces anymore — both are flags.
- **Week 2 — eval work.** "Which eval case did you add or change this cycle, and what did it catch?" Generic engineering frames "eval" as "test"; the AI-specific frame is that the eval is the measurement contract. The question tests whether the IC is extending the contract.
- **Week 3 — registry hygiene.** "Any prompts / judges / fixtures you registered or changed this week? Walk me through the versioning and the change note." The question tests the registry discipline that makes the operating system compound.
- **Week 4 — incidents and on-call.** "Anything paged? Anything you escalated? Any incident-to-fixture loop you closed?" If nothing paged on their surface, the question is "when did you last dry-run a runbook you own?" (mod-107 chapter 03). The question tests the on-call command craft.
- **Week 5 — mentorship / cross-shape pairing.** "Who did you pair with across shapes this cycle, and what did you learn or teach?" The question tests the influence dimension at L30-plus.
- **Week 6 — the "learn without the assistant" loop.** Reserved for the anti-atrophy check from chapter 05. "This cycle's unassisted practice: what did you choose, how long did it take, what did you notice about your own craft?" The question tests whether the deliberate-practice loop is real.
- **Week 7 — growth-plan progress.** "On your written growth plan, what moved this cycle? What didn't? What are we sequencing for the next one?" Chapter 04 is the growth plan.
- **Week 8 — credential honesty.** "Anything external you did this cycle — a conference, a blog, an open-source contribution? Honest read: did it also move the inside?" Chapter 07 is the frame.

Eight items on an eight-week rotation. The spine takes ten to fifteen minutes; the rest of the 1:1 is the IC's agenda — project status, blockers, career questions, feedback both directions, personal items. The spine is a *coverage guarantee*, not a replacement for the IC-owned conversation.

### What the spine produces

A 1:1 run with the spine produces a dated written record that reads like the artefact-class grid in chapter 02 at weekly resolution. Six months of 1:1 notes should be scannable into the review packet without re-interviewing the IC. If the manager's cycle-end review draft requires a fresh round of interviews with peers to produce the evidence, the 1:1 cadence has not been doing its job.

### One concrete anti-pattern

A 1:1 agenda where every slot is "project status, roadmap, blockers" and the AI-specific spine is never run is a 1:1 that will produce a surprise review. The fix is boring and procedural: add the rotating spine item to the shared doc, run it the first week, keep running it. By cycle-end the evidence is there.

## The feedback library specialised to AI-engineering behaviours

Feedback inherits its base shape from Scott (radical candor) and the SBI structure canonicalised in the Center for Creative Leadership literature: *situation — behaviour — impact*. What this module adds is the behaviour library specific to AI engineering. Below are fifteen specific behaviours worth building SBI-shape feedback around. Each is a *specific thing an IC did or did not do* that a manager can point at in a review.

### Trace-reading craft

- **Reading traces on their own PRs** — "In your eval walk-through last Thursday, you referenced the three failing traces by their IDs and walked us through the first-turn tool call; that's the altitude we want trace reading at." (SBI shape; the impact is the other engineers in the room now have a shared read of the failure.)
- **Reading traces on others' surface** — "You spent an hour this week reading traces on the retrieval team's surface after the regression they flagged; the fixture you proposed is now in their registry. That's the cross-shape mentorship L40 asks for."

### Eval work

- **Methodology memo** — "The judge memo you attached to the PR explained why a rubric judge was the right choice over an LLM-judge for this product; that's methodology defence I can read at calibration."
- **Threshold defence** — "The release-gate meeting asked why the threshold was 0.78 and you cited the confidence band on the golden set; that's the level of threshold defence the gate needs from an L30-plus."
- **The anti-pattern** — "You moved the threshold down last week without the methodology note; that's the behaviour that looks like eval-rot from the outside. The behaviour we want is: moved threshold, note why, dated."

### Registry hygiene

- **Clean registration** — "The prompt you registered on Monday has the diff note, the eval delta, and the approval; the pattern there is what L30 registry discipline looks like."
- **The anti-pattern** — "The judge version from two weeks ago was renamed without a version bump; a downstream run can't reproduce. The behaviour we want is: version bumps on any semantic change, always."

### Incident command and on-call

- **First-response discipline** — "On Tuesday's page, you ran the runbook, preserved the trace evidence before pulling the kill-switch, and opened the comms channel in the first five minutes. That's the on-call command L30 expects."
- **Postmortem facilitation** — "You facilitated the retrospective at L30 without taking the blame for an incident you were not the root cause of; the discipline there is the chapter-05-of-mod-107 habit worth naming."

### Cross-shape mentorship

- **Pairing across shapes** — "You paired with the eval engineer for six hours on Tuesday morning and co-authored the fixture; that's the kind of cross-shape pairing the L40 influence dimension reads on."
- **The anti-pattern at L40** — "Your mentorship record this cycle is all within shape. The behaviour we want is one or more pairings with someone on an adjacent shape (RAG, eval, applied); the growth plan names this."

### Assistant usage (chapter 05 ties to this)

- **Judgment-owned use** — "In the retrieval-chunker PR, you accepted the assistant's draft but rewrote the re-ranker decision because you had evidence from the previous golden set that the assistant's choice was wrong. That's the judgement-owned use of the assistant that the ladder reads at L30."
- **The halo risk** — "The three PRs this week all accepted the assistant's draft substantially; the eval cases attached were the assistant's default set; when I asked you about the retrieval-chunking choice on Wednesday you couldn't walk me through it. The behaviour I'm flagging is: assistant-authored ships without owned judgment behind them. Not a line-crossing today; a trajectory I want us to redirect before it becomes one."

### External signal (chapter 07 ties to this)

- **Useful external signal** — "The talk you gave at the engineering meetup walked through the fixture discipline our team actually uses; you came back with two cross-company connections who have improved our jailbreak-fixture coverage. That's external signal that also moved the inside."
- **The theatre risk** — "The vendor certification you completed this cycle is in an area you don't currently work in and you haven't referenced it in any decision since. The behaviour I'm noting is: external artefact without an internal read-out. The time it took is time the growth plan was counting on; let's sequence."

### The library as a living artefact

The fifteen behaviours above are starter content. The manager's version is a longer doc that grows as new behaviours become worth naming — a judge-corpus-governance behaviour, a model-vendor-portfolio read behaviour, an authorisation-boundary behaviour as agentic work matures. The library is a manager-owned document the manager reads before writing a review; it is not a public artefact (SBI feedback is personal), but its *categories* are legible to the team through the spine (the 1:1 agenda) and the rubric (chapter 02).

## The honest-cadence split

Two situations show up repeatedly in a frontier-tooling era and are easily confused. The ritual cost of getting them wrong is a senior promoted past their craft and a junior over-corrected into stalling. The ritual difference is a diagnostic in the 1:1, a feedback shape, and a growth-plan shape.

### Situation A — the frontier-era senior leaning on the assistant without leading

**The shape.** A mid-to-senior engineer whose shipped throughput has risen sharply in the last six to twelve months. The PRs are plausible; the reviews pass; the eval cases attached are reasonable. 1:1 feels confident. When asked to walk through a trace, the engineer can — up to a point. When asked why the retrieval chunker is 400 tokens instead of 800, the answer is "that's what we had." When asked what model regression would most bite the current design, the answer drifts. When asked to debug a trace with the assistant turned off, the engineer either says "let me pull it up" (and opens the assistant) or produces a diagnosis thirty minutes slower than their usual pace.

**The diagnostic in the 1:1.** Three questions, asked over three successive 1:1s, that surface the situation without accusing the engineer of anything:

1. "Walk me through the design decision behind the retrieval chunker. What would make you change it?"
2. "If the model provider shipped a 4x cheaper, 20% dumber variant tomorrow, which of your current features would be affected first?"
3. "Pick a PR from last week. Can you narrate the key file change without opening the assistant window?"

If the engineer answers all three confidently with specific references, the halo risk is not the shape. If two or three of the three draw blanks, the shape is a senior shipping assisted throughput without owned judgment underneath.

**The feedback shape.** SBI, specific, trajectory-not-verdict. The sentence worth having in the kit: *"I want us to catch this now because what I'm seeing is a trajectory, not a line crossed. Your output is high. The question that sits under my concern is: when the model provider shifts next quarter, do the calls in your scope hold? I think we should redirect the next cycle around rebuilding the owned judgment under the throughput."*

**The growth-plan shape.** Chapter 05's anti-atrophy loop, specific to this engineer: one deliberate-practice loop per week, assistant off, on a representative problem shape in their scope; an unassisted design document per cycle; mandatory read-time against the vendor cookbooks / release notes their scope depends on; a paired re-review of three PRs from last quarter, asking "what would we do differently if we knew what we know now." The review conversation at cycle-end acknowledges the throughput *and* names the gap. The promotion conversation waits a cycle.

### Situation B — the junior leveraging the assistant responsibly to learn

**The shape.** An L20 or early-L30 engineer who pairs with the assistant on most PRs. Throughput is high for their level. The PRs include the assistant's suggested eval cases but also one or two of their own. When asked to walk through a trace, they do, including the part where the assistant's suggestion was wrong and they corrected it. When asked why the chunker is 400 tokens, they explain the trade-off they saw. When asked to pair on a trace with the assistant off, they do, slower, visibly thinking, and usually get there.

**The diagnostic in the 1:1.** Same three questions as Situation A. The signal is in the *shape* of the answer, not the speed. The junior who articulates "I don't know why it's 400 — the assistant suggested it and I didn't push back, I should go look" is in Situation B: honest about the gap, has the metacognition to see it, is learning. The senior who answers "that's what we had" is in Situation A: the gap is there and is not being seen.

**The feedback shape.** SBI, specific, affirming the pattern worth repeating. *"The way you narrated that trace on Wednesday — including the part where the assistant's eval suggestion was wrong and you added your own — is what we want responsible assistant use to look like at L20. The trajectory you're on, if sustained, puts you well inside L30 next cycle."*

**The growth-plan shape.** Chapter 04's cross-shape rotation; chapter 05's deliberate-practice loop (right-sized for L20 — perhaps one unassisted problem a cycle, not weekly). The mentorship pairing at L30 (chapter 02) is the growth target. The review conversation at cycle-end celebrates the trajectory *and* names the pattern they are reinforcing so they see it.

### The diagnostic as a team ritual

Running the three-question diagnostic across every IC is a ritual, not a trap. The point is not to catch anyone; the point is to make the behaviour pattern legible on a cadence so the two situations are told apart. The manager who runs the diagnostic quarterly and writes it up in the 1:1 notes has the evidence at review time; the manager who diagnoses only after a cycle's review has gone sideways has already shipped the misread.

## The review packet template

A performance-review packet is the written artefact an IC and the calibration meeting read at cycle-end. The base shape is Fournier / Larson / GitLab-handbook; the AI-specific adaptations are in what each section includes.

A packet that holds up at calibration (chapter 06) has the following sections, each roughly a page.

1. **Context and shape.** Which IC shape they are placed in on the current rubric (chapter 02); any shape transitions in the last cycle; the current level and the artefact-class grid.
2. **Artefact-class evidence.** For each of the five artefact classes — traces reviewed, eval work, registered artefacts, incidents led, mentorship — three to five specific examples from the cycle, dated, with links. This section is the review's evidence base; the spine in the 1:1 notes should make it short to compile.
3. **Growth-plan read.** What the growth plan from last cycle said; what moved; what didn't; the honest read on the "learn without the assistant" commitment (chapter 05) and the rotation targets (chapter 04).
4. **Drift notes.** Any **counterfactual-at-planning-time** adjustments (chapter 06) — work that was deprioritised because a vendor release obviated it, work that was re-scoped because a model regression made the previous plan invalid. A line per adjustment; not a defence of under-delivery, a read of the environment.
5. **External signal.** Any external artefacts from the cycle — talks, publications, OSS contributions, certifications. For each, one line on whether it also moved the inside (chapter 07). Credential theatre is named in this section, not hidden.
6. **Peer-feedback synthesis.** Three to five lines of synthesis from peer feedback, in the IC's and the manager's framing — not raw quotes, interpreted signal.
7. **Rating and rationale.** The level placement at cycle-end; the rating (meets / exceeds / etc. per the company's cycle) with a two-sentence rationale that references the rubric and the artefact-class grid.
8. **Next-cycle growth plan.** The IC-manager co-authored plan for next cycle (chapter 04); named commitments; dated check-in points.

A packet with blanks in sections 2 or 4 is a packet the manager and the IC need to re-work before calibration. A packet with every section filled and the artefact-class grid defended is a packet that holds up row by row.

## A concrete read: a mid-cycle feedback conversation

An L30 applied engineer has been shipping prolifically with the assistant for two quarters. The 1:1 spine has surfaced three concerning signals: on three successive "walk me through the trace" weeks, the engineer opened the assistant to answer; the unassisted-practice slot on the growth plan has not run in six weeks; the "design decision behind the retrieval chunker" question drew a blank. The manager writes the conversation out before having it. The script:

> "I want to share a pattern I'm watching, early enough that we can redirect. Three times this quarter, when I asked you to walk through a trace in our 1:1, you opened the assistant to help. The unassisted-practice slot on your growth plan hasn't run in six weeks. Last Thursday you couldn't walk me through why the retrieval chunker is 400 tokens; the design call sat with you at the time. What I'm not saying is that your throughput is a problem — it isn't. What I am saying is that I can't tell, from what I'm seeing, whether the judgment under the throughput is growing or quietly thinning. I'd like us to do three things next cycle. First, put the unassisted-practice slot back on and I'll protect the time. Second, I want one unassisted design memo on the chunker decision — not to grade it, to read where your current take is. Third, let's pair-review three of your PRs from last quarter with the assistant off and ask what we'd do differently. I think if we do that, we'll either find the judgment is there and I'll stop worrying, or we'll find the gap and we'll know exactly what to work on. Which parts of this read as fair?"

The conversation is specific, SBI-shaped, redirectable, and leaves the engineer with agency. It does not accuse; it does not pre-rate; it invests in making the situation visible so the next cycle produces evidence either way. Chapter 05 is specific about what the unassisted-practice loop looks like in detail; chapter 06 is specific about how the cycle-end review is graded fairly whether the loop closed the gap or not.

## Summary

- The 1:1 inherits its shape from the canonical management reading; what this chapter adds is a **rolling artefact-class spine** that guarantees the five AI-specific artefact classes are read on an eight-week rotation.
- The **feedback library** is a manager-owned document of SBI-shape behaviours specific to AI engineering — trace-reading, eval methodology, registry hygiene, on-call command, cross-shape mentorship, assistant judgment, external signal. The library is a living artefact the manager writes against as behaviours become worth naming.
- The **honest-cadence split** is the diagnostic that tells apart a senior leaning on the assistant without leading and a junior leveraging the assistant responsibly to learn. The three-question in-1:1 diagnostic is the ritual; the feedback shapes and the growth-plan shapes are different in each situation.
- The **review packet** has eight sections; two of them (artefact-class evidence and drift notes) are the AI-specific adaptations that make the packet hold up at calibration (chapter 06).

The next chapter turns from the cadence to the growth-plan surface: how the manager authors a growth plan that lets an IC cross the applied / agentic / eval / RAG / fine-tune sub-ladders, move laterally into a tech-lead track, or pivot to the manager track in partnership with the `ai-augmented-team-manager`.
