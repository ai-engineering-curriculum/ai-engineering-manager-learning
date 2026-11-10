# Narrating Regressions Honestly

Every quarter a team ships, something will miss. A release will clear the release gate on the headline suite and regress on a slice that was not represented in the gate. A canary will surface a user-impacting failure the pre-release evals did not see. A vendor will ship a model update that regresses a specific feature, and the team will spend two weeks on a pin-and-recover. An agent will hit a jailbreak corner case the red-team pass walked past.

None of these is an engineering failure. Non-determinism guarantees some fraction of releases will miss. The engineering function is designed to catch and recover — mod-104's trace-review closes the loop, mod-107's postmortem institutionalises the learning, mod-109's operating cadence keeps the rhythm. The fraction of misses is not the credibility surface; the *honesty of the narration* is. A team that publishes its misses and the reasons for them compounds trust across quarters. A team that surfaces misses only when a partner forces them is in a one-way credibility decline.

This chapter is about the narration. Four miss shapes recur. Each has a specific template for publishing the miss honestly, in the three cadences from chapter 02, with the reason named and the lesson fed back. The templates are not polite framings for inherently bad events — they are the shape the honest narration takes.

## Motivation — four miss-narration failures that compound

Four failure patterns the templates below are designed against.

- **The silent absence.** A release missed on a slice; the manager knew; the weekly that followed said "release v1.4 shipped, headline eval 78.2 against a 76.0 gate" and did not mention the slice miss. Six weeks later a product partner surfaced the regression from a customer-success report. The director read it for the first time in Slack. The team's credibility dropped — not because the regression was unusual, but because the manager appeared to have hidden it. The honest narration would have cost one sentence in the weekly; the hidden narration cost a quarter of credibility rebuild.
- **The post-hoc reframe.** A release missed; the manager narrated it as "within noise" in the weekly that followed, then — when the slice regression stabilised below the floor two weeks later — had to re-narrate it as "a confirmed regression on the X slice." The two narrations did not line up. The first was defensible at the time; the second contradicted the first; the partners who had read the first now distrust the second, even when the second is correct. The honest first-narration would have been "we saw a -3.1 slice delta on X; the current read is that this is inside our ±5 noise band for the slice, and we will re-read after this week's eval cycle to confirm." The caveat at the time saves the later re-narration from reading as a cover-up.
- **The vendor-blame narration.** A vendor's model update regressed the team's feature. The manager narrated it as "vendor regressed our feature; we're pinning to the previous snapshot." True, and incomplete. The team's pre-release evals did not catch the regression because the evals ran on the previous snapshot; the team had no canary-on-new-snapshot discipline for the vendor's release cycle. The vendor-blame narration is honest about the vendor but dishonest about the team's own eval gap. The partner reads it, and later — when the next vendor regression also catches the team unprepared — the pattern becomes visible and the first narration reads as a deflection.
- **The "we handled it" summary.** An incident hit users; the P2 was closed inside the SLA; the postmortem was written; the fixtures were added; the follow-ups have owners. The manager narrates this to the exec as "we had a P2 incident on Tuesday; it was resolved within SLA; postmortem is linked." True, and under-narrated. The exec reader does not know *what caused it*, *whether it is systemic*, or *what the team learned*. The exec will approve the ask the next quarter, but the next miss they read will carry the memory of under-narration, and the trust surface will have thinned. Honest incident narration at exec altitude reads the cause in one sentence, the systemic learning in one sentence, and the specific protection that was added in one sentence. Compression, not opacity.

Each failure is a specific shape; each has a template. The rest of this chapter builds them.

## Core concept 1 — the five fields every miss narration carries

Before the templates, the common fields. Every miss narration in every cadence carries these five, varied in compression for the audience.

1. **The fact of the miss.** A specific metric moved against a specific threshold on a specific surface. "Slice X regressed -3.1 against the 70.0 slice floor on release v1.4." Not "the release underperformed"; not "we saw some quality drift." The specific number against the specific floor.
2. **The reason.** The team's current best read of the cause, scoped to the honesty of what is known. "Retrieval-corpus drift against a new billing-template corpus the support team rolled out 2026-10-28 without eval-fixture sync." If the reason is not yet known, say so — "reason under investigation; current working hypothesis is retrieval-corpus drift; confirmation expected inside 48 hours." The honest unknown is better than the invented known.
3. **The scope of the impact.** Who was affected, over what window, at what severity. "The slice covers ~8% of production traffic (per the handbook routing-analytics page). Window: v1.4 shipped 2026-11-05, miss visible from 2026-11-07. Severity: measurable quality drop on the affected tickets; escalated to human review at the same rate as baseline so no user-facing surface degraded."
4. **The recovery.** What was done or is being done. "Fixture queued (eval-registry#412); re-train opens Monday 2026-11-10; expected recovery inside the v1.5 release window." Not "we will investigate"; what specifically is happening.
5. **The systemic learning.** What changed in the engineering surface so this class of miss is less likely next time. "Retrieval-corpus drift on partner-team-owned corpora is now in the fixture-sync checklist (mod-104 chapter 04 registry); the support team's corpus owner is on the model-watch cc list going forward." If no systemic learning yet, say so — "systemic learning TBD at postmortem; postmortem scheduled Tuesday."

All five fields in every miss narration. Compression varies by cadence; the fields do not vary. A weekly covers the five in five sentences; a monthly in one paragraph; a quarterly in one sentence. The field set is invariant.

## Core concept 2 — four miss shapes and their templates

### Miss shape 1 — the released-with-a-miss

The release cleared the release-gate on the headline suite but missed a named threshold on a named slice, or missed an absolute floor the gate protected. The release shipped because the overall policy allowed it (the slice floor is advisory; the regression is inside the configured slice-tolerance band; a manual override from the release-triad chair was executed) and the miss is a tracked debt.

**Weekly template (section 4 of chapter 02's weekly).**

```
Release miss (v1.4, 2026-11-05):
  Fact — billing-dispute slice 68.4, Δ -3.1 vs. train, below the 70.0 slice floor.
  Reason — retrieval-corpus drift against the billing-template corpus the support 
    team rolled out 2026-10-28 without eval-fixture sync.
  Scope — ~8% of production traffic; measurable quality drop; escalation rate 
    unchanged, no user-facing severity increase.
  Recovery — fixture queued (eval-registry#412); re-train Monday; expected 
    recovery in v1.5 release window (2026-11-19).
  Systemic learning — partner-team-owned corpora now in fixture-sync checklist; 
    corpus-owner cc'd on model-watch going forward.
```

**Monthly template (section 3 of chapter 02's monthly).**

```
v1.4 shipped with a slice miss (billing-dispute, -3.1 vs. train against a 70.0 slice 
floor); traced to retrieval-corpus drift from a partner-team rollout that bypassed 
the fixture-sync. Recovered in v1.5 (71.4, above floor). Systemic fix landed in the 
mod-104 registry checklist; one similar partner-team corpus added to the model-watch 
distribution.
```

**Quarterly template (part of section 3 of chapter 02's quarterly).**

```
One release this quarter shipped with a tracked regression on a specific user slice; 
the regression was recovered the following release after a corpus-drift fix. The fix 
is now in our release checklist so this class of regression will be caught before 
release going forward.
```

The compression ratio across the three cadences is ~90 words → ~50 words → ~30 words. The field set is the same at every altitude; the specificity lands at the reader's altitude. The weekly is for the partners who will read the eval-registry link; the quarterly is for the executive who needs the shape of the quarter.

### Miss shape 2 — the user-facing incident

A P0–P3 incident hit users — a prompt-injection succeeded, a data-exfiltration surface was triggered, a tool-use error produced a visible wrong answer, latency spiked beyond the SLO. The incident is already running through mod-107's response and postmortem path; this chapter composes the postmortem into the reporting cadences.

**Weekly template (section 4).**

```
P3 incident (2026-11-06, closed 2026-11-07):
  Fact — prompt-escape on the dev-facing agent-v2; 14 users saw the agent emit a 
    tool-call signature in-chat rather than invoking the tool.
  Reason — the tool-use prompt template was updated in v2.3 without a corresponding 
    regression test for the tool-call parsing boundary (mod-104 registry fixture 
    was not updated).
  Scope — 14 affected sessions over a 38-minute window; dev-facing only (not 
    customer-facing); no data-exfiltration; no auth-boundary breach.
  Recovery — rolled back the prompt-template change in v2.3.1 (20 minutes); 
    postmortem R16 complete (handbook/postmortems#2026-11-06).
  Systemic learning — the tool-call parsing-boundary fixture is now a release-gate 
    requirement, not a release-note suggestion; three additional fixtures added for 
    adjacent parsing boundaries.
```

**Monthly template.**

```
One P3 this month, a prompt-escape on the dev-facing agent-v2 that released an 
unparsed tool-call in-chat for 14 sessions over 38 minutes. Rolled back inside 
mitigation SLA; postmortem complete; fixture added as release-gate requirement. 
No P0 or P1; no customer-facing exposure; no auth-boundary or data-exfiltration 
activity.
```

**Quarterly template.**

```
The quarter had one dev-facing P3 incident (prompt-escape, 14 sessions over 38 
minutes, no customer exposure, mitigated inside SLA). The specific fixture-gap 
that allowed it is now a release-gate requirement; the regulator-facing incident 
narrative was not required for this incident (mod-107 Article 73 criteria not met).
```

The P0 and P1 shapes are more elaborate — the weekly becomes a running update, the quarterly includes the regulator-facing summary. The field set is the same.

### Miss shape 3 — the vendor-upgrade regression

The vendor shipped a new model snapshot, the team's product regressed on that snapshot, and the team pinned to the previous snapshot. The failure surface includes the vendor's behaviour and the team's own canary discipline — a narration that blames only the vendor misses the team's eval-gap.

**Weekly template.**

```
Vendor regression (2026-11-04, resolved 2026-11-05):
  Fact — the primary provider shipped the Nov 4 snapshot; our overnight eval run 
    on the new snapshot showed a -5.1 point regression on the headline suite 
    (below the 2-point week-over-week tolerance).
  Reason — two layers. Vendor-side: the new snapshot has a more cautious refusal 
    policy on billing-context prompts; our prompts trigger the refusal on ~3% of 
    inputs that previously went through. Team-side: our model-watch (mod-109 ch 03) 
    had the snapshot on the watch-list but we did not have a pre-release canary 
    against the pre-GA preview; we evaluated on day-of release.
  Scope — no production impact (we hold on the pinned snapshot by default; the new 
    snapshot is not promoted until the eval re-baseline passes).
  Recovery — held on the previous snapshot; opened a prompt-refactor branch to 
    address the refusal-trigger pattern; target re-baseline in two weeks.
  Systemic learning — opened a vendor preview-access ask with Procurement for 
    pre-GA canary access to the primary provider; mod-109 ch 03 model-watch ritual 
    expanded to include preview-cycle fixtures where available.
```

**Monthly template.**

```
One vendor-snapshot regression this month on the primary provider (Nov 4 snapshot), 
blocked from promotion by our default-pin discipline (no production impact). Reason 
two-sided — vendor refusal-policy tightening on billing-context plus team gap on 
pre-GA canary. Fix landed in the prompt-refactor branch; preview-access ask open 
with Procurement so we catch this earlier next cycle.
```

**Quarterly template.**

```
One vendor-model regression this quarter caught by our default-pin discipline 
before production (no user impact). Our team gap — late canary on the vendor's 
pre-release cycle — is now closed via a procurement ask for pre-GA preview access.
```

The two-sided framing — vendor-side cause, team-side gap — is the honesty discipline. A narration that only names the vendor reads as deflection at the first reading and as a pattern at the third. The two-sided narration surfaces the team's own gap immediately and compounds credibility because the next vendor regression will be narrated against a closed gap.

### Miss shape 4 — the missed bet

A quarterly bet did not land — the feature's quality numbers did not reach the launch bar; the capability was later than planned; the market-facing promise the manager made in the prior quarterly was not kept. This is the hardest narration shape because it does not have a clean postmortem artefact to lean on; it is a *planning* miss, not an engineering incident.

**Weekly template.** Rarely a weekly-only story; usually a monthly or quarterly pattern. In the weekly, the fact appears briefly if the plan is slipping:

```
Watch-list (chapter 02 section 5):
  csrep-agent v2 (the summariser-on-long-tickets capability we committed to in the 
  Q3 quarterly) is tracking behind the Q4 launch commitment; current eval run shows 
  the quality bar is ~6 points below the launch floor on the long-ticket slice. 
  Likely re-targeting to Q1 2027; honest read-out coming in the November monthly.
```

**Monthly template.**

```
The csrep-agent v2 capability committed to in the Q3 quarterly is re-targeting from 
Q4 2026 to Q1 2027. Fact — long-ticket slice is currently 70 against a 76 launch 
floor; six points below the bar. Reason — the summariser's long-context behaviour 
degrades on tickets with embedded product-ID tables (~18% of long tickets have 
these); the eval-slice coverage did not catch this in the Q3 planning because the 
labelled fixtures were drawn from short-context tickets. Scope — product has been 
notified; the Q1 2027 launch commitment is being negotiated with the customer-success 
team who shared the Q4 timing externally. Recovery — opened a labelled fixture 
expansion against the long-context / product-ID-table shape (~800 fixtures, Q4 
budget already approved); re-baseline expected mid-December. Systemic learning — 
the eval-slice-coverage review that chapter 02's single-source-of-truth page runs 
is now an explicit input to quarterly-commitment planning, not only release-gate 
planning.
```

**Quarterly template.**

```
One of our three Q3-committed bets — the long-ticket summariser capability — is 
re-targeting from Q4 2026 to Q1 2027. Reason: an eval-coverage gap on a specific 
input shape (long tickets with embedded product-ID tables) that was not represented 
in our Q3 planning fixtures. We have the fixtures labelled, the re-baseline is in 
flight, and the re-targeted launch is a confidence-mid bet (chapter 03 bands) for 
Q1. The externally-shared Q4 timing has been walked back with customer success this 
week; we will not commit externally in advance of the release-gate pass going 
forward.
```

Four honesty moves in this quarterly narration worth naming. First: the re-targeting is explicit and dated — Q4 to Q1, with the month. Second: the reason is scoped to a specific technical cause, not to generic "scope grew" framing. Third: the externally-shared commitment is named as the specific credibility debt and the walk-back is dated. Fourth: the systemic learning — "we will not commit externally in advance of the release-gate pass going forward" — is a policy change, not a wish. The executive reads this and sees a team that missed and *changed how it plans* so the next miss is less likely. That is what a quarterly miss narration earns when it is honest.

## Core concept 3 — the Article 73 regulator-facing narrative is a separate artefact

For any incident meeting the EU AI Act Article 73 reporting criteria (or equivalent regimes), the regulator-facing narrative is a mod-107 chapter 07 artefact — a specific document, authored by the incident commander with legal counsel's review, that is *not* this chapter's cadence. It composes with the quarterly's miss ledger (one line, "an Article-73-triggering incident was filed with the regulator on date X") but the regulator-facing document is its own thing.

Two confusions worth naming. First: the quarterly cannot be the regulator's artefact. The regulator reads a document written to them; the exec reads a document written to them; they are different registers and different legal postures. The manager does not conflate them. Second: the weekly narration of the incident has to be *consistent with* the regulator-facing narrative, and the inconsistency is a legal exposure the manager owns. A weekly that says "no customer-facing surface degraded" and a regulator filing that says "customer-facing degradation observed" is a discovery risk. The two documents are both drafted from the mod-107 postmortem; the manager owns their consistency.

## Core concept 4 — the "no-miss weeks" discipline

A week with no misses is still narrated with the misses section present and populated with "none." The reason is honesty-by-construction: a reader who sees the misses section only in weeks with misses learns the section is optional, and when the section is absent in a future week they will reasonably wonder whether a miss was hidden.

```
Incidents and misses:
  - None this week. (Release-gate posture held across both candidate releases; 
    no P0–P3 incidents; no vendor regressions; no bet-slip signalled by the trace 
    review.)
```

Five lines, including "none." The next week, when there is a miss, the reader opens the same section at the same position in the document and reads the miss without ambiguity about whether the section is populated completely. The cost of saying "none" is nothing; the credibility value is that the reader trusts the section's completeness.

## Core concept 5 — the three anti-patterns the manager actively resists

Three failure patterns recur enough to name directly.

- **The passive voice.** "A regression was observed on the billing-dispute slice." Honest, but under-narrated — who observed, when, what action is in flight. The honest construction names the observer, the window, and the action: "The trace-review (mod-109 ch 03) on Monday surfaced a -3.1 slice regression; eval-registry fixture queued; re-train Monday." The passive voice is always a hint that a specific actor is being left out of the story, and the partners who read the narration quickly learn the pattern.
- **The "proactive" reframing.** "We proactively identified and addressed a slice-level regression." The word "proactively" is the giveaway — it is doing rhetorical work the facts do not need. The honest construction states the facts: "The trace-review surfaced a slice regression one week after release; we fixtured it, re-trained, and recovered in the next release." The exec reader is not impressed by "proactively"; they are impressed by the specific numbers and the specific actions.
- **The "lessons learned" that are not systemic.** "We learned to pay closer attention to slice-level regressions." Not a learning; a sentiment. The honest construction names the specific systemic change: "The trace-review's slice-coverage check is now part of every release-gate decision, not only the quarterly review." If no systemic change happened, the narration says "no systemic change yet; postmortem on Tuesday will produce one." Sentiment-as-learning degrades the signal.

Each anti-pattern is easy to slip into under time pressure. The manager's job is to catch themselves — and the IC writing the first draft — before the narration ships.

## What good looks like

- Every miss has a narration in the week it is first known, the month it closes, and the quarter it falls in.
- Every miss narration carries the five fields (fact, reason, scope, recovery, systemic learning) at the compression ratio of the cadence.
- Every miss narration is consistent across cadences — the quarterly's summary matches the monthly's paragraph which matches the weekly's bullets.
- The regulator-facing Article 73 narrative (where applicable) is its own artefact; the reporting cadences compose with it, not duplicate it.
- No-miss weeks say "none" rather than omitting the section.
- The passive voice, the "proactive" framing, and the sentiment-as-learning are caught in the draft and rewritten.

## What bad looks like

- Misses appear in the reporting only when a partner forces them.
- The first narration of a miss contradicts the second narration two weeks later.
- Vendor-regression narrations blame only the vendor; the team's own eval or canary gap is unstated.
- Incident narrations say "we handled it" without naming the cause, scope, or systemic learning.
- No-miss weeks omit the section; readers learn to assume a miss was hidden.
- The "proactively addressed" pattern recurs; partners learn the manager's rhetorical tells and discount the narration.

## Summary

- Non-determinism guarantees some fraction of releases will miss. The honesty of the narration, not the fraction, is the credibility surface.
- Every miss narration carries the five fields: fact, reason, scope, recovery, systemic learning.
- Four miss shapes recur — released-with-a-miss, user-facing incident, vendor-upgrade regression, missed bet — each with a template at weekly, monthly, and quarterly compression.
- The regulator-facing Article 73 narrative is a separate artefact (mod-107 chapter 07); the reporting cadences compose with it.
- No-miss weeks say "none." The passive voice, the "proactive" reframing, and the sentiment-as-learning are caught and rewritten.

The next chapter turns to the demo surface — the single most-recurrent failure in AI engineering stakeholder communication, and the discipline that keeps it from undoing the honest reporting cadences this chapter and the previous one establish.
