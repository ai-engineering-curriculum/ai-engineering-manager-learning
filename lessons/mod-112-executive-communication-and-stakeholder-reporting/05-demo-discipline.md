# Demo Discipline — The Cherry-Picked Demo Is the Single Most Common Failure

Every AI engineering team has shipped a demo that looked better than the feature. The engineer prepared two canonical inputs that the system handles well. The slide deck showed the hero outputs. The executive — or the customer, or the conference audience — saw a feature that works. Three weeks later, the real workload surfaced a long tail of failures the demo had walked past, and the team spent the quarter explaining the gap between what was shown and what shipped.

The cherry-picked demo is the single most common failure mode in AI-engineering stakeholder communication. Not because engineers are dishonest — most engineers do not consciously cherry-pick. The cherry-pick is a drift. The prompts the IC remembers best are the ones they debugged most; the outputs they showed the manager in the 1:1 were the surprising wins; the inputs they prepared for the slide deck naturally skew toward the ones the system handles. Without an explicit discipline, every demo becomes a cherry-picked demo by default.

This chapter builds the discipline — the three-hero / three-hard / two-expected-failure framework, the demo-review checklist the manager chairs before any external-facing artefact ships, the pre-brief that sets the audience's expectations, and the five artefact types the discipline applies to.

## Motivation — six demo failures in the recent historical record

Six demo-shaped failures that any AI engineering manager has seen (or will see) and that the discipline below is designed against.

- **The internal all-hands demo that promised a capability the eval did not have.** An engineer demonstrated the new agent on three live prompts; the agent handled all three. The all-hands was impressed; the product org absorbed the capability as "ready" and promised it to two customer-success forums in the following week. The eval-suite number on the full input distribution was 62%; the three live prompts were inside the 38% most-represented input shape. Three weeks later the customer-success team was walking back the "ready" framing to the external forums.
- **The CEO demo that drove a strategic decision the eval would not have supported.** A single demo for a CEO produced authorisation for a cross-team rollout. The CEO, correctly, acted on what they saw. The eval told a more nuanced story — the feature works on some slices and not on others — but the eval was not in the room. The rollout commitment landed before the eval-slice conversation happened; the team then had to run the eval-slice conversation against a commitment already made. The manager had the discipline internally and did not apply it to the demo.
- **The conference talk that cited the hero number.** An engineer gave a conference talk citing "94% accuracy on customer-support ticket classification" — a real number, from the eval-registry, measured on a specific high-signal slice. The full-suite number was 71%. Twitter noticed the discrepancy against a public dataset; the conference organiser asked for a correction; the team's external credibility took a hit the manager had to repair.
- **The screenshot-in-a-blog-post that ran past the known failure mode.** The team's engineering blog ran a case study with a screenshot of the agent handling a complex multi-step task. The screenshot was real; the task shape in the screenshot was one of the two the system handled reliably. Readers who tried the feature on their own workloads hit the long tail; the blog's comments section filled with "I couldn't reproduce this" and the team had to write a follow-up acknowledging the demo had been cherry-picked.
- **The investor-deck "magic moment" that was a lucky run.** A pitch deck for an internal budget forum included a demo-flow recorded as a one-take screen capture. The recording looked flawless. The feature on a typical input produced a flawed result ~25% of the time; the one-take had hit the 75%. The budget was approved; the following quarter's operating report had to explain why the feature's measured quality was below what the deck implied. The approval-rate drop the next quarter was predictable.
- **The customer webinar where the demo failed live.** The manager had not prepared a fallback; the IC was running against the production system with a prompt the IC had not rehearsed on the webinar-day model snapshot. The feature produced a jailbreak-adjacent output in front of forty prospects. Not a product disaster; the customers were forgiving — but the public-facing mishandling was *preventable*. The pre-brief and the fallback-prompt discipline below are specifically for this shape.

Each failure is a specific demo-discipline failure. None is an eval failure; the eval was correct. None is an engineering failure; the engineers were doing honest work. The manager's demo-discipline practice was absent.

## Core concept 1 — the three-hero / three-hard / two-expected-failure framework

The core discipline. For any demo set — a slide of example outputs, a live demo, a screencast, a case-study blog post, a conference talk, a customer webinar — the demo set has:

- **Three hero prompts.** Inputs the system handles well. These are the ones the audience expects to see; they build credibility and teach the audience what the feature is for. The hero prompts come from the eval-registry's high-performing slice — by construction, they are *representative* of a real sub-distribution the team can defend, not cherry-picks from a lucky-run bucket.
- **Three hard prompts.** Inputs that are inside the team's target quality band but are *harder* than the heroes — closer to the edge of the system's envelope, longer context, multi-step reasoning, adversarial-adjacent phrasing. The hard prompts demonstrate the system handles non-trivial inputs, not only the easy ones. The eval-registry's mid-band slice is the source.
- **Two expected-failure prompts.** Inputs the team *knows* the system does not handle well, included deliberately in the demo set. These two prompts do two jobs: they show the audience the honest shape of the feature's envelope, and they prevent the audience from leaving the demo with an inflated model of the system. The team does *not* hide these; they narrate them.

A demo set with this eight-prompt structure takes slightly longer to prepare than a three-hero cherry-picked set. It also takes longer to present — the narration around the hard and expected-failure prompts is non-trivial. The cost is real; the credibility value is the honest demo surface that compounds over months.

A typical narration for the two expected-failure prompts:

> "Two failures worth showing. On this input shape, our feature produces an incorrect answer about 30% of the time; we route these inputs to human review by default when the system's confidence score is below the threshold we set, and we are working on a labelled fixture expansion against the specific input pattern. The eval-registry link below shows the slice; the Q1 2027 roadmap has the fix dated."

That narration — twenty seconds of a six-minute demo — is what distinguishes a demo-disciplined team from a cherry-picked team. The executive leaves with an accurate model; the customer leaves with a trustworthy expectation; the engineering blog's comments section does not fill with disappointed reproductions.

## Core concept 2 — the demo-review checklist

Every external-facing artefact — internal all-hands included, because the internal-all-hands demo travels outward faster than the manager expects — passes a checklist the manager chairs before it ships. Ten items; some apply to every artefact, some only to live demos or recordings.

```
Demo-review checklist (owner: AI eng manager)

Artefact: _______________________    Author: _______________________
Target audience: _______________________    Date to ship: _______________________

[  ] 1. Eight-prompt structure present
         3 hero / 3 hard / 2 expected-failure. Each prompt links to the eval-registry 
         slice it is drawn from.

[  ] 2. Hero prompts not cherry-picked
         Hero prompts come from the high-performing slice *at the current eval 
         baseline*, not from a lucky-run bucket. The eval-registry run that sourced 
         them is dated and linked.

[  ] 3. Expected-failure prompts are narrated honestly
         The narration explicitly names the failure rate on the prompt's slice, the 
         route for handling the failure (human review, confidence threshold, 
         fallback prompt), and the roadmap-dated remediation.

[  ] 4. No single-number quality claim
         Any accuracy / quality number cited in the artefact is paired with its slice, 
         its evaluation date, its judge, and either a confidence interval or a 
         comparison to the full-suite number. "94% on classification" is not 
         acceptable; "94% on classification, measured on the high-signal slice 
         (which is 38% of production traffic) by judge-v3 on 2026-11-01; full-suite 
         baseline is 71%" is.

[  ] 5. Live demos have a fallback
         Any live demo has a pre-recorded fallback (video or screenshots) ready for 
         the specific prompts being demonstrated, on the specific model snapshot, 
         produced in the week before the demo. The demo is rehearsed against the 
         same model snapshot that will be live on the demo day.

[  ] 6. Pre-brief for the audience is prepared
         A two-sentence pre-brief that sets expectations: what the feature does, 
         what it does not, and what the audience will see. The pre-brief is 
         delivered before the first demo prompt, not after.

[  ] 7. Model, prompt, and system state are recorded
         The exact model snapshot, the exact prompt template, and the system state 
         (retrieval corpus version, tool-schema version) are recorded in the 
         artefact's footnotes. The audience should be able to reproduce.

[  ] 8. Safety / guardrail state is noted
         If the demo bypasses a production guardrail (e.g. runs without the 
         confidence-threshold routing to human review that production uses), it is 
         noted explicitly. Demo-mode is never the production configuration without 
         saying so.

[  ] 9. Reviewer other than the author
         A named reviewer other than the artefact's author has signed off. For 
         external-facing artefacts (blog, talk, webinar), the reviewer is the 
         manager and (where applicable) the AI governance peer.

[  ] 10. Walk-back plan if the demo lands differently than planned
         One-line plan for what the team does if the audience leaves with an 
         inflated expectation: who corrects, how, by when. Specifically for CEO 
         and external-facing demos where the audience may act on what they saw.

Reviewer: _______________________    Date: _______________________
```

The checklist is ten minutes to fill out. The alternative — the demo that undoes a quarter of honest reporting — is six weeks of credibility rebuild. The arithmetic is unambiguous.

## Core concept 3 — the pre-brief that sets expectations

The pre-brief is the demo-discipline move that an audience of executives, customers, or external readers notices and remembers. Two sentences, delivered before the first demo prompt lands.

A template:

> "The feature you are about to see handles [X task] on [Y input distribution] at [Z quality level]. It is not meant to handle [W input types] — those are routed to [human review / fallback path / etc.] by design. You will see two prompts the feature does not handle well, included deliberately so you leave with an accurate model of its envelope."

The three moves worth being explicit about in that template. First: the quality claim is specific (task, input distribution, quality level). The audience knows the scope. Second: the *out-of-envelope* path is named. The audience knows what the team does when the feature does not handle an input. Third: the expected-failure prompts are pre-committed. The audience is primed to see them and — critically — to see the narration around them as honesty, not as a failure.

The pre-brief does another thing that matters: it sets the register for the rest of the demo. After a pre-brief that honestly scopes the feature, the audience reads subsequent hero outputs as evidence of *this specific scope*, not as evidence of a general capability the feature does not actually have. The executive's follow-up ask is scoped to the real feature instead of the imagined one.

## Core concept 4 — the five artefact types the discipline applies to

Demo discipline applies to more than live demos. Five artefact types, each with its own application of the checklist.

### Type 1 — the internal all-hands demo

Scope the discipline is lightest, risk the discipline matters most. Internal all-hands demos travel — the product org absorbs capabilities as "ready" from them; the customer-success team quotes them to customers; the sales team cites them in deals. The eight-prompt structure plus the pre-brief is non-optional. The live-demo fallback and the reviewer sign-off are specifically called out — an internal demo gone wrong is a credibility expense across functions.

### Type 2 — the exec packet (board deck, steering committee slide, quarterly review deck)

The demo is typically a slide of screenshots or a one-take recording, not a live run. The checklist applies to the slide: eight-prompt structure if the slide is a demo of input/output pairs, or single-number-claim discipline if the slide is a metric claim. The reviewer sign-off for exec artefacts is the manager and the manager's director. The walk-back plan is the manager's own plan — the executive will act on what they saw, and the manager owns the correction surface if the action lands wrong.

### Type 3 — the external conference talk, blog post, or podcast

Scope the discipline is heaviest. The AI-governance peer (mod-111 chapter 03) is a mandatory reviewer in addition to the manager. The single-number claim discipline is strictly enforced — external audiences have no context for the number's slice, so the narration must carry it. The reproducibility footnote (checklist item 7) is non-optional for blog posts and talks; the audience will try to reproduce, and the team's reputation is on whether they can.

### Type 4 — the customer webinar or sales demo

Scope the discipline is heaviest and the stakes most concrete. The fallback plan (checklist item 5) is specifically important: a live demo failure in front of prospects can tank a deal. The pre-brief is adjusted for the audience — customer-facing language instead of engineering register, but the same scoping discipline.

### Type 5 — the hiring loop demo (interviewer-facing)

Often overlooked. The team's engineers demo the product to candidates in interviews; the demo they show communicates the team's calibre, and the candidate's calibration against the team depends on how honestly the demo is scoped. The eight-prompt structure may be too heavy; the pre-brief is non-optional. A candidate who sees a cherry-picked demo in interviews and joins is a candidate whose first month on the team is a correction against the cherry-pick, which is a worse start than an honest demo would have produced.

## Core concept 5 — the three discipline-breaking pressures the manager resists

Three pressures make the demo discipline hard. The manager who recognises them resists them explicitly.

- **The last-minute compression pressure.** The exec meeting is Thursday; the engineering team is heads-down on a release; the manager has twenty minutes to prepare the slide. The temptation is to pull the three cleanest outputs from Slack and call it a demo. The discipline says no — a slide that needs to ship in twenty minutes gets the slide the team can honestly produce in twenty minutes, which may be one hero output with a sentence of scope rather than three outputs with no scope. The compression lands at the slide depth, not at the honesty depth.
- **The audience-is-non-technical pressure.** "The exec will not understand the hard-prompt narration; let's just show the heroes." A dangerous rationalisation. The executive who sees only heroes builds an inflated model and makes investment decisions against it. The executive who sees heroes and hard-prompt narration — delivered in their register, not in engineering-register — builds an accurate model and makes investment decisions that survive. The pre-brief is specifically the translation surface for this case; use it.
- **The IC-pride pressure.** The IC who built the feature wants to show it at its best; the manager does not want to undermine the IC. The manager's move is not to undermine — it is to *frame*. The IC's hero outputs are preserved; the manager-chaired review *adds* the hard and expected-failure prompts to the set. The IC's work is still the demo; the demo is just honest. If the manager cannot have this conversation with the IC, the credibility of the whole team's reporting surface is at risk, and that is the conversation that has to happen in the next 1:1 (mod-108 chapter 03).

## What good looks like

- Every external-facing artefact passes the demo-review checklist with a named reviewer sign-off.
- Every demo set uses the three-hero / three-hard / two-expected-failure structure.
- Every demo opens with a pre-brief that scopes the feature; the audience leaves with an accurate model.
- Single-number quality claims never ship without the slice, date, judge, and comparison baseline.
- Live demos have rehearsed fallbacks on the specific model snapshot live on demo day.
- The internal all-hands demo and the hiring-loop demo are reviewed with the same discipline as the external talk; the manager does not treat internal audiences as exempt.
- The three discipline-breaking pressures are named in the team's demo handbook and the manager resists them explicitly.

## What bad looks like

- Demo sets are three or four hero prompts, no hard prompts, no expected-failure prompts.
- Quality claims circulate as single numbers; the slice and date are unknown.
- Live demos have no fallback; a failed live demo produces a public walk-back.
- The internal all-hands demo travels to the customer-success team and then to the customer without the manager noticing.
- The IC-pride pressure wins the argument in the manager's head; the manager does not insist on hard-prompt inclusion; the pattern repeats; the team's reporting-honesty surface decays.

## Summary

- The cherry-picked demo is the single most common failure mode in AI-engineering stakeholder communication. It is a drift, not a choice, and only an explicit discipline prevents it.
- The core discipline is the three-hero / three-hard / two-expected-failure structure, framed by a pre-brief that scopes the feature honestly.
- Every external-facing artefact — internal all-hands, exec packet, conference talk, blog, customer webinar, hiring-loop demo — passes a ten-item demo-review checklist chaired by the manager.
- The three pressures that break the discipline — last-minute compression, non-technical-audience rationalisation, IC-pride — are named and resisted explicitly.
- The manager's demo discipline composes with mod-107's incident-narrative discipline, chapter 04's regression-narration discipline, and chapter 03's cited-evidence discipline into a single honesty-by-construction reporting surface.

The next chapter turns from the team's reporting surface to the manager's own: the next-step position on the role ladder and the deliberate-practice plan that builds the artefact portfolio for the step after this one.
