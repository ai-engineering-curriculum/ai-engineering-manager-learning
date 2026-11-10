# Exercise 03: Honest Regression Narrative

**Estimated effort:** 2.5 hours · **Output:** twelve regression narrations — four miss shapes × three cadences — produced from four realistic scenarios.

## Objective

Produce the complete regression-narration surface across the four miss shapes chapter 04 identifies (released-with-a-miss, user-facing incident, vendor-upgrade regression, missed bet) at each of the three cadences (weekly, monthly, quarterly). Each narration must carry the five fields chapter 04 names (fact, reason, scope, recovery, systemic learning) at the compression ratio of the cadence, and the three narrations for each miss shape must be internally consistent across cadences.

A reviewer reading only the quarterly for any of the four miss shapes should know the shape of what happened, the reason, and what changed in the system. A reviewer reading the weekly should be able to act on it the same week it is sent.

## Prerequisites

- Chapter 04 (`Narrating regressions honestly`).
- Mod-107 chapter 05 (blameless postmortems) and chapter 07 (regulator-facing incident narrative).
- Mod-104 chapter 06 (eval cadence) and chapter 07 (data flywheel) — the loop the "systemic learning" field reads from.
- Optional: a real regression or incident from your team (anonymised appropriately for the exercise). Composites work; realistic scenarios land better.

## Problem statement

Produce twelve regression narrations total — one per miss-shape-and-cadence combination — against four realistic scenarios.

### Part A — Four scenarios (20 minutes to sketch, scenarios will anchor the narrations below)

Sketch four 150-word scenarios, one per miss shape. Each scenario names: the feature, the specific metric that moved, the window, the discovered cause, the user-facing severity, and the engineering recovery. These are the facts the three narrations per scenario will compress to each cadence's altitude.

1. **Released-with-a-miss scenario.** A release cleared the release-gate on the headline suite but missed a specific slice floor. Choose the slice; choose the specific technical cause (retrieval-corpus drift, judge-drift masking the slice regression, prompt-refactor side effect, etc.).
2. **User-facing incident scenario.** A P2 or P3 incident that hit users (not just internal). Choose the failure class (prompt-injection, excessive-agency, hallucination landing in a user-visible surface, latency SLO breach). Must be bounded enough to reach the "closed" state within the exercise window.
3. **Vendor-upgrade regression scenario.** A primary provider shipped a model update that regressed the team's feature. Choose the specific regression shape (refusal-policy tightening, instruction-following drift, output-format shift, cost-per-token change that invalidated the economic assumption).
4. **Missed-bet scenario.** A quarterly-committed capability that is re-targeting from its original launch window. The re-target is at least six weeks; the reason is specific; a customer-facing commitment was made externally and has to be walked back.

### Part B — The twelve narrations (90 minutes)

For each scenario, produce three narrations:

- **Weekly template.** The week the miss is first known. The miss appears in section 4 of the weekly (`Incidents and misses`); the five fields are present at weekly compression (~80 words).
- **Monthly template.** The monthly that closes the window in which the miss happened. Section 3 of the monthly (`Release summary and miss ledger`) or the incident section, depending on the shape. ~50 words.
- **Quarterly template.** The quarterly that reads on the quarter containing the miss. Executive-register translation. ~30 words plus any policy change the miss produced.

Each narration must be internally consistent with the other two for the same scenario. If the weekly says "closed within SLA," the monthly and quarterly cannot contradict that. If the monthly identifies a systemic fix, the quarterly's "we will not X going forward" language must trace to that fix.

### Part C — The three anti-patterns (30 minutes)

Produce a short reflection (300 words) that identifies, in your twelve narrations, at least two places you had to deliberately resist each of the three anti-patterns from chapter 04:

- **The passive voice.** Where did you first write "a regression was observed" and then rewrite with a named observer?
- **The "proactive" reframing.** Where did you first use words like "proactively," "ahead of," or "early identification" that were doing rhetorical work the facts did not need?
- **The sentiment-as-learning.** Where did you first write "we learned to pay closer attention to X" and then rewrite to a specific policy or checklist change?

The reflection is where the exercise earns its name. If you produce twelve clean narrations on the first try and have no anti-patterns to report, you either over-wrote (avoided the natural drafts that produce the patterns) or under-caught (shipped drafts that still carry the patterns and you did not see them). Both are honest findings worth noting.

### Part D — The "no-miss" variant (10 minutes)

Write a one-week weekly for a hypothetical week with no misses to report. The exercise is to write the "Incidents and misses" section with the `None` populated, honestly, following the chapter 04 no-miss discipline. Bonus credit if the "none" narration includes the brief parenthetical that lists what was *watched* even in the absence of a miss (release-gate posture held, no P0–P3, no vendor regressions, no bet-slip signalled by the trace review).

## Requirements

- Four scenarios are sketched, covering the four miss shapes from chapter 04.
- Twelve narrations are produced (four scenarios × three cadences).
- Every narration carries the five fields (fact, reason, scope, recovery, systemic learning) at the cadence's compression ratio.
- Weekly narrations are ~80 words each; monthly narrations are ~50 words each; quarterly narrations are ~30 words plus policy change where applicable.
- The three narrations for each scenario are internally consistent — no contradictions between cadences.
- At least one of the four scenarios (typically the P2 or P3 incident) is checked against the Article 73 regulator-facing-narrative criteria from mod-107 chapter 07. The check is recorded explicitly in the narrations — either "not Article-73-triggering" or "Article-73 narrative filed on date X; summarised here."
- The reflection in Part C identifies at least two instances of each of the three anti-patterns in your drafts.
- The no-miss weekly from Part D is produced.

## Starter guidance

- Write the scenarios in realistic engineering-register before the narrations. The facts should be specific enough to compress; vague scenarios produce vague narrations.
- For the released-with-a-miss scenario: the five fields are easiest to practise on this shape. Start here; the muscle built generalises to the other three.
- For the user-facing incident scenario: the mod-107 chapter 05 postmortem is the substrate the narrations read from. If you do not have a real postmortem to compress, sketch one before writing the narrations — compressing a non-existent postmortem produces fictional narrations.
- For the vendor-upgrade regression scenario: resist writing only the vendor-side cause. The two-sided framing (vendor-side cause plus team-side gap, chapter 04 core concept 2) is the honesty discipline. If you cannot name a team-side gap, you have not read the regression deeply enough — rewrite.
- For the missed-bet scenario: the walk-back of an externally-shared commitment is the hardest narration to write honestly. Resist euphemism ("re-prioritised," "scope-adjusted," "moved to a subsequent window"). The honest construction is "we committed to X by Y; we are shifting to Z; we will not commit externally in advance of release-gate pass going forward."
- For the quarterly narrations: resist copying the monthly and compressing. The register slip from engineering to executive is a rewrite, not a crop. Re-read chapter 02 core concept 4 if the quarterly sounds like the monthly.
- For the anti-pattern reflection: honest finding is more valuable than clean drafting. If you caught yourself three times writing "proactively," that's three instances for the reflection, which earns the exercise.
- For the no-miss weekly: the temptation is to omit the section. Resist. The whole exercise tests whether you can discipline yourself to say "none" in the specific position every week, honestly.

## Acceptance criteria

- Four scenarios sketched, covering the four miss shapes.
- Twelve narrations produced, each carrying the five fields at the correct compression.
- Internal consistency across cadences — no contradictions.
- At least one scenario checked against Article 73 criteria with the result recorded.
- Part C reflection names at least two instances of each anti-pattern in your drafts.
- The no-miss weekly is produced, with the "none" discipline followed.
- A peer reader can distinguish, from the artefacts alone, which narration belongs to which cadence without seeing the headers.

## Stretch goals

- **The cross-cadence diff.** Produce a side-by-side diff of the three narrations for each scenario, with the specific words that changed highlighted. The diff is the exercise: what information was *added* at the weekly, *preserved* in the monthly, *compressed* at the quarterly, and *translated* from engineering to executive register.
- **The miss-ledger page.** Compose the four scenarios' monthly narrations into a single quarterly miss-ledger page — the artefact a VP would pull up at a quarterly operating review. The ledger's structure is the exercise: how do you order the misses (by severity, by date, by shape); how do you surface the pattern (e.g., three of four involved a partner-team dependency that the release-approval workflow did not catch); how do you close the page without lapsing into sentiment.
- **The Article 73 full-text narrative.** For your user-facing-incident scenario, write the full regulator-facing narrative (mod-107 chapter 07). The exercise is to see how the same facts compress differently when the audience is a regulator versus an executive versus a weekly partner. The three audiences read different artefacts from the same incident.
- **The second-draft discipline.** After producing the twelve narrations, wait 24 hours. Re-read them cold. Rewrite the three you notice most drift in. The 24-hour pause is the manager's most under-used honesty discipline; practising it here is the exercise.
- **The partner-escalation path.** For the missed-bet scenario, produce the 150-word Slack message to the customer-success team that first surfaces the re-target externally. The honest construction of the Slack message is the hardest of the exercise — the register is close to customers, the stakes are a credibility debt, and the discipline from chapter 04 does not let you write euphemism.
