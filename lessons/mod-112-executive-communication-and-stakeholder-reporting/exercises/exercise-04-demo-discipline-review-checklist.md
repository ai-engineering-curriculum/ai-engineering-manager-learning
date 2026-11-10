# Exercise 04: Demo Discipline Review Checklist

**Estimated effort:** 2 hours · **Output:** a team-ready demo-review checklist, a fully-prepared demo set for one feature, and a pre-brief script, plus a red-team audit of an existing external-facing artefact.

## Objective

Operationalise the demo-discipline chapter into artefacts the team can use before shipping any external-facing demo, slide, blog post, conference talk, customer webinar, or hiring-loop demo. By the end of the exercise, the team has (1) a checklist the manager chairs for every demo, (2) an example demo set that follows the three-hero / three-hard / two-expected-failure framework, (3) a pre-brief script template, and (4) an honest audit of one existing external-facing artefact (a blog, a slide, a recorded demo) that was shipped without the discipline.

The audit is the hardest part. The exercise asks you to find something that already went out — possibly from your own team — and read it critically.

## Prerequisites

- Chapter 05 (`Demo discipline — the cherry-picked demo is the single most common failure`).
- Mod-104 chapter 04 (prompt / judge / dataset registries) — the registry that sources the hero, hard, and expected-failure prompts.
- Mod-111 chapter 03 (consuming the AI Governance family) — the governance-peer reviewer for external-facing artefacts.
- A real external-facing artefact to audit in Part D — your team's most recent blog post, conference-talk recording, customer-webinar deck, or all-hands demo recording. If you do not have one, use a public AI engineering vendor's product-launch demo as the audit target.

## Problem statement

Four deliverables.

### Part A — The team's demo-review checklist (30 minutes)

Produce a checklist for your team to use. Start from the ten-item checklist in chapter 05, specialise it to your team's artefact types (what features, what model provider, what governance peer, what hiring-loop interviewer), and remove any items that do not apply to your team's context (noting *why* each removal is honest, not convenient). Add any items the chapter's checklist missed for your team.

The output is a single-page markdown document, team-handbook-ready, that an IC authoring an external-facing artefact would read before the manager's review meeting.

### Part B — A fully-prepared demo set for one feature (45 minutes)

Pick one production feature on your team (or a composite feature at the realism of mod-104 chapter 02). Build the eight-prompt demo set:

- Three hero prompts drawn from the eval-registry's high-performing slice. For each: the specific slice-coverage source, the eval-registry entry ID, the recent-run date, the output the system produces.
- Three hard prompts drawn from the mid-band slice. For each: the slice source, the specific reason the prompt is "hard" (long context, multi-step reasoning, adversarial-adjacent phrasing, out-of-distribution input shape), the eval-registry ID, the output the system produces, the quality read.
- Two expected-failure prompts. For each: the specific failure mode the prompt elicits, the measured failure rate on the slice the prompt represents, the production handling for this failure mode (human-review routing, confidence-threshold rejection, fallback prompt), the roadmap-dated remediation if one exists.

For each of the eight prompts, record: the exact prompt text, the exact expected output or output shape, the model snapshot the demo will run against, the system state (retrieval corpus version, tool-schema version, guardrail state), and the ~20-second narration the IC will deliver alongside the output.

### Part C — The pre-brief script (15 minutes)

Produce the two-to-three-sentence pre-brief script for your chosen feature, following the chapter 05 template. The script scopes the task, the input distribution, the quality level, the out-of-envelope path, and primes the audience for the two expected-failure prompts. Target audience is an executive (CEO, SVP, or customer-side equivalent); the register is non-technical but specific.

Alongside the script, produce a 150-word "handoff note" to the IC who will deliver the demo — what you want them to remember, what you do not want them to improvise, what the fallback is if a live prompt fails on the day.

### Part D — Audit an existing external-facing artefact (30 minutes)

Pick one existing external-facing artefact — your team's, a peer team's, or a public example. Read it against your Part A checklist. Produce a 500-word audit that answers:

- Which checklist items did the artefact satisfy? Which did it fail? Be specific — not "it did not use the three-hero / three-hard / two-expected-failure framework" but "the artefact showed four hero prompts with no hard or expected-failure prompts; in particular, prompt three ('summarise this 3-paragraph document') is on the easy slice and does not represent the long-context long-tail where the feature is known to degrade."
- What would an honest version of the artefact have looked like? Rewrite one specific section — a slide, a paragraph of the blog, a 90-second segment of the recording — in the honest register.
- What is the specific credibility cost the team incurred by shipping the dishonest version? Think in terms of trust with the audience, downstream commitment creation (what did the audience go on to promise or ask for based on what they saw), and walk-back cost.
- If this artefact was from your own team, what does the chapter 05 handoff note to your IC authors look like for the *next* artefact — the specific change to the authoring workflow that prevents the recurrence.

## Requirements

- The team-ready checklist exists as a single-page markdown document; chapter 05's ten items are reviewed and either kept (with reasoning for team-specificity) or removed (with reasoning). At least one team-specific item is added.
- The eight-prompt demo set exists for one feature. Each of the eight prompts has text, expected output, model-snapshot pin, system state, and narration script.
- The hero and hard prompts trace to the eval-registry; the expected-failure prompts have measured failure rates sourced from the eval surface, not invented.
- The pre-brief script exists and is two or three sentences long; it is in non-technical register but is specific about what the feature does and does not do.
- The IC handoff note exists, ~150 words, and names the fallback plan.
- The audit exists, ~500 words, read against the team's checklist, with one specific rewrite of a section of the audited artefact in honest register.
- The audit names at least one specific credibility cost of the dishonest version.

## Starter guidance

- The checklist specialisation from chapter 05: resist both over-generalising (keeping all ten items blindly) and over-specialising (removing items because "our team doesn't do that yet"). The honest move is to review each item against the next six months of planned artefacts, not only the current state.
- For the demo set: pick a feature you have already read on the eval surface at depth. The exercise is to source hero, hard, and expected-failure prompts from the *actual* eval data, not to invent prompts. If you cannot find three expected-failure prompts in your eval registry, your eval surface is probably covering too narrow a slice — the inability to produce the discipline is itself a signal.
- For the pre-brief: write the engineering-register version first in a scratch doc, then translate to executive-register. The translation is the chapter 02 translation discipline applied at demo altitude.
- For the audit: resist choosing an easy target. The hardest audit is of your team's most-cited external-facing artefact — the one everyone liked. The hardest audits produce the most calibration.
- If auditing your own team's artefact is too fraught, use the public example — but understand that the exercise is specifically designed for the uncomfortable version. An engineering manager who cannot read their own team's past work critically is a manager who will not catch the next drift.
- For the credibility-cost estimation: the cost is real but often invisible to the team that shipped the artefact. Specific audience-side commitments (the CS team promised the feature to two customers; the prospect converted based on a flawed demo; a conference audience member joined expecting a capability that is not there) are the visible edges of a larger invisible cost.

## Acceptance criteria

- The team-ready checklist is one page, readable in two minutes, and the IC authoring an external artefact would recognise every item.
- The eight-prompt demo set is complete for one feature; all eight prompts have the full specification (text, output, model pin, state, narration).
- The hero / hard / expected-failure structure is honest — the hero prompts are not from the lucky-run bucket; the expected-failure prompts are not softballs.
- The pre-brief is in the executive register, two to three sentences, and names the out-of-envelope path explicitly.
- The IC handoff note names the fallback plan and the specific "do not improvise beyond X" discipline.
- The audit is 500 words, specific, and includes one rewritten section of the audited artefact.
- A peer reader can read the audit and say: what specifically was dishonest, what the honest version looks like, what the credibility cost was.

## Stretch goals

- **The reviewer-roster.** For each of the five artefact types in chapter 05 (internal all-hands, exec packet, external talk/blog/podcast, customer webinar, hiring-loop demo), name the specific reviewer on your team or across your partners. The all-hands reviewer may be the manager; the external talk reviewer must include the AI governance peer (mod-111 chapter 03); the customer webinar reviewer must include the product partner. The reviewer-roster is the operating substrate the checklist drops into.
- **The second-draft demo set.** After one week, re-read your eight-prompt demo set. Which hero prompts drifted toward "easy" after the first authoring? Which hard prompts turned out to not be hard? The second-draft revision is the exercise — the hero and hard slices on a team's registry move month-over-month and the demo set has to move with them.
- **The live-demo fallback.** For the feature in Part B, produce the pre-recorded fallback (90-second video or screenshot sequence) that would run if the live demo failed on the day. The recording includes the hero and one expected-failure prompt. The exercise is to see how long it takes you to produce the fallback honestly — longer than you expected, which is why most live demos ship without one.
- **The anti-cherry-pick team ritual.** Design the team's standing ritual that produces the eight-prompt demo set as a side-effect of regular engineering work, not as a special effort before an exec meeting. The ritual composes with the mod-109 chapter 03 weekly trace-review; the exercise is to draft the ritual spec.
- **The external-artefact rollback policy.** Draft the team's policy for what happens when an external-facing artefact is found — by audit, by audience feedback, by a Twitter reply — to be dishonest after shipping. The policy specifies the walk-back path, the apology register, the specific commitment the team makes about the next artefact. Policy-level artefacts prevent ad-hoc corrections that make the second mistake worse than the first.
- **The cross-team audit exchange.** Trade audits with a peer AI engineering manager — you audit their team's most-cited external artefact, they audit yours. The external eye catches drifts the team is blind to; the exchange is the exercise. The honest finding you produce for their team is the honesty you want to receive about your own.
