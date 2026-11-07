# The Prototype → MVP → GA Promotion Gate

Every AI feature lives through three postures: **prototype** (a demo that proves the idea is worth more work), **MVP** (a release to a bounded user group under explicit measurement), and **GA** (general availability with SLOs and on-call). Each transition is a gate, not a calendar date. This chapter defines the evidence that opens each gate — eval delta, user signal, cost ceiling, safety floor, release gate, incident playbook, on-call readiness, SLO — in a form the manager can chair.

## Motivation

Two failures recur across AI products.

- **Prototype-to-production leap.** The team skips MVP. A demo that scored well on cherry-picked traces is wired up to production traffic. The first real-user regression ships to everyone at once and incident response is improvised.
- **Infinite MVP.** The team hides in MVP for quarters because no one has written down what "GA-ready" means. Users are told "we're still in beta"; leadership asks for GA every quarter; the team invents a new reason to delay each time.

Both failures resolve the same way: write the evidence each gate requires, in advance, and hold the line.

## The three postures, in one line each

- **Prototype.** A working trace end-to-end on a non-trivial dataset. No users. No production traffic. No SLO. Allowed to be brittle. Owner is a small working group; cost is absorbed under the discovery capacity line (chapter 05).
- **MVP.** A bounded release to a known user group with eval thresholds met, a cost ceiling, a safety floor, a named owner, and an incident path even if not yet a mature one. Traffic is measured; outcomes are measured; the team can roll back in minutes.
- **GA.** Open availability within the audience the business supports, with published SLOs, a mature incident playbook, an on-call rotation that includes your team, release gates enforced in CI, and a documented rollback and deprecation path.

The gates below promote across these.

## The prototype → MVP gate

Five pieces of evidence open this gate. If any is missing, the item does not promote.

### 1. Eval delta vs. baseline (non-trivial, named, reproducible)

The prototype must beat a specified baseline on a specified eval suite by a specified margin. Each term must be named in the gate decision, not hand-waved.

- **The eval suite** names which datasets, which metrics (exact-match, judge-grade, reference-based), and which judge configuration if judge-grade. Prefer registered suites your team has used before so the numbers compose with history.
- **The baseline** is not "nothing." It is the production system the feature is replacing, or the "use the model without retrieval / without the agent / without the tool" variant, or a published public-benchmark number — whichever gives the strongest refutation.
- **The margin** is named in the gate decision, not discovered after the fact. "Beat baseline by 5 points on suite-A judge-grade, median across three runs" is a gate. "Looks better" is not.
- **Reproducibility.** Anyone on the team can reproduce the eval run from a commit SHA and a config. If the run is not reproducible, the number is anecdote.

Teams that promote without this piece ship features that silently regress in production because there is no baseline to detect regression against.

### 2. User-research signal

A prototype that satisfies itself on evals but has no user-side evidence is not an MVP candidate. Required evidence is whichever is honest for your context:

- A user-research session set (e.g. 5–10 users for a consumer-leaning product, 3–5 design-partner conversations for an internal/B2B product) where the prototype is used in realistic tasks and the team records the transcripts.
- A **"wrong answers hurt how much"** reading — the manager can state, in one sentence, what harm a bad answer causes in the target context and how often the current prototype produces that class of answer.
- A **task fit** reading — does the trace of a representative user session match the capability posture of the prototype, or are users asking it to do things the prototype does not support.

The absolute methodology is less important than the fact that the evidence exists before the promotion vote, not after.

### 3. Cost ceiling

Every MVP ships with a written cost ceiling. Required:

- **Per-request cost target** at the modal request size (median-tokens-in / median-tokens-out), expressed in both dollars and ratio to the baseline variant.
- **Monthly ceiling at expected MVP traffic** — a dollar number the business is willing to spend on this feature inside the MVP window.
- **Abort rule** — the specific condition under which the MVP is paused and reviewed: typically when projected spend exceeds the monthly ceiling, or per-request cost drifts more than a stated threshold above target over a stated window.

A prototype that is one third of the GA-target cost per request is a candidate. A prototype that is three times the GA-target cost and has no credible path to the GA-target is not — regardless of eval scores.

### 4. Safety floor

The feature does not promote to MVP without a minimum safety posture. This is not the full safety work from `mod-106-safety-guardrails-and-the-release-triad`; it is the minimum required to put real users on the prototype without a predictable incident.

- A run of the relevant safety evals (jailbreak, prompt-injection if tool-using, content-category sensitivity for the audience) at the audience tier, with a documented pass / threshold decision.
- An input-output guardrail layer appropriate to the audience (e.g. input moderation + output moderation + policy-aware system prompt, sized to the feature).
- A **named abuse-and-harm hypothesis** for this audience and a monitoring hook that would surface the first ten incidents of it. Not perfection — detection.

For the vocabulary (harm categories, probe sets, injection-resistance patterns), lean on `mod-106` and the external material it references.

### 5. Owner of record and rollback mechanism

The MVP has a single named human owner on the team; a rollback that can execute in minutes and does not require code deploy; and an audit log enabling a post-hoc trace review. If any of these is missing the MVP is not ready for users.

### Running the vote

A **promotion vote** is held on the gate — not a status update. The engineering manager chairs it; the architect or principal MLE joins as the technical veto; the governance peer joins if the audience requires safety sign-off; the product partner joins as the user-signal owner. Each of the five pieces either passes or fails. A fail returns the item to prototype with a named remediation list and an estimated re-vote date. A pass promotes with a written MVP scope note.

## The MVP → GA gate

GA is a higher bar. Eight pieces of evidence; the first five build on what MVP already established, the last three are GA-specific.

### 1. Eval delta holds under production traffic

The eval win that opened MVP must still hold against a **production-traffic-derived eval set** — a judging slice drawn from the real distribution of MVP traffic, not the pre-MVP dataset. If the judgment regressed under real traffic, the item does not promote; it goes back to MVP with a scope correction. This is the point most teams discover they were measuring the wrong distribution.

### 2. User signal is a sustained outcome, not a demo

The MVP ran long enough to produce an outcome signal — satisfaction, task completion, downstream action, whichever the MVP scope note defined. The number is positive, sustained, and not a cherry-picked window. If the business is making revenue claims, the number is defensible under scrutiny.

### 3. Cost at GA scale, not MVP scale

Before promotion, model the cost at the first six months of GA traffic — realistic, not rosy — and compare to the GA-cost budget. If the number is out of band, the team buys time (with a smaller audience or a routing strategy) before promoting. Teams that discover the GA cost after promotion are teams that learn the vendor contract the hard way.

### 4. Safety is monitored in production, not only tested pre-release

MVP showed the safety floor. GA requires:

- Safety evals rerun on a regular cadence (typically weekly or on-release, whichever is faster).
- Production monitoring for the named abuse-and-harm hypothesis — every incident catalogued, a fraction reviewed by humans.
- A **release triad** sign-off per the mod-106 vocabulary (model + data + safety review complete before each promotion).

### 5. Release gate enforced in CI

A documented release gate that runs in continuous integration — not a human-checked checklist. The gate includes the eval thresholds, the cost thresholds, and the safety thresholds the team has committed to. A PR that fails the gate does not ship. This is the canonical `mod-104-eval-driven-sdlc-and-release-gates` work.

### 6. Incident playbook exists and has been rehearsed

Not just written: rehearsed. The playbook names the first three incident types this feature is most likely to produce (regression on eval X, cost spike, policy violation at scale) and the runbook for each. The team has done a tabletop with the incident playbook before GA ships. If a playbook has never been rehearsed, assume it will not work in the real incident.

### 7. On-call readiness

The team is in a named on-call rotation for this feature, has the pager configured, has the escalation path documented, and has the handoff written for secondary coverage. Shared coverage with the `ai-infra-team-lead` peer is defined where the surface is co-owned. This is the piece that most often blocks GA when it is skipped — the team does not want on-call pain, so they claim "we'll roll it in after." After GA, no one has budget to go back and do it.

### 8. SLO is published, and error-budget policy exists

An SLO, in the SRE sense, with a measurable service-level indicator, a target, and a stated window. For an AI feature, the SLI set typically covers at least latency, availability, cost per request, and a quality signal (either a regression-detection signal from a canary eval set or a user-reported-regression signal). An **error-budget policy** states what happens when the budget is exhausted — typically a freeze on new feature work on this surface until the budget recovers. Without the error-budget policy, an SLO is a wish.

### Running the GA vote

Same shape as the MVP vote. Engineering manager chairs. Architect / principal MLE as technical veto. Governance peer for safety sign-off. Product for user-signal sign-off. Infra peer for serving / SLO / on-call sign-off. Each of the eight pieces passes or fails. The vote is recorded; the decision is written; the GA date is committed only after the vote passes.

## The anti-patterns

These are the patterns that defeat the gate. Each one has a visible tell.

- **Score-chasing on the wrong eval.** The team iterates until the eval number is impressive, promotes to MVP, and only then discovers the eval was not measuring the thing the business cares about. Fix: write the eval with the user-signal and cost-signal in mind, and ask the architect / principal MLE to review the eval design before you start optimising against it.
- **Safety-as-afterthought.** The team treats the safety floor as a tick-box at promotion time and runs it the week before the vote. Fix: safety evals ride alongside the capability evals from the start of the prototype.
- **"GA soon" for three quarters.** The team is in MVP, the eval looks fine, the user signal is fine, but GA keeps slipping. Almost always the missing piece is on-call readiness, SLO, or error-budget policy — items 7 and 8 above. Fix: the manager names which of the eight pieces is blocking and puts a dated commitment on it.
- **GA without rollback.** The team promotes to GA and discovers the rollback mechanism from MVP is no longer sufficient at GA traffic. Fix: the GA gate explicitly asks "at GA traffic, how long is the rollback to the last-known-good version and who executes it." If the answer is unknown, the GA vote fails.
- **Mixing audience and gate.** "We'll ship to employees first, that's basically MVP, then roll out externally" — but the external audience has different safety needs and the MVP never measured them. Fix: the MVP scope note names the audience bounds and the GA vote re-scopes for the new audience.

## A worked gate: Agent-Retriever v2 (hypothetical)

Continuing the mod-103 running example — a doc-Q&A agent for an internal customer-support org — walking the MVP gate:

| Gate piece | Evidence required | Pass / fail call |
|------------|------------------|------------------|
| Eval delta | ≥ 5 points on suite-A judge-grade, median of 3 runs, vs. current production retriever | pass if ≥ 5; fail if < 5 |
| User signal | 5 design-partner sessions recorded; task-fit read documented | pass if ≥ 5 sessions; fail if < 5 or no task-fit read |
| Cost ceiling | $X per request, $Y monthly at projected MVP traffic, abort rule written | pass if within band; fail otherwise |
| Safety floor | jailbreak + prompt-injection + content-moderation evals pass; abuse monitor live | pass if all three pass; fail if any fail |
| Owner and rollback | named human owner; <5 min rollback without deploy; audit log on | pass if all three present; fail otherwise |

The vote is thirty minutes. The decision is a paragraph. The next quarter's roadmap carries forward whichever rows passed.

## Summary

- Three postures: prototype (demo-grade), MVP (bounded release with explicit evidence), GA (open with SLO and on-call).
- Prototype → MVP opens on five pieces of evidence: eval delta, user-research signal, cost ceiling, safety floor, named owner and rollback.
- MVP → GA opens on eight: production-traffic-derived eval delta, sustained user outcome, cost at GA scale, in-production safety monitoring, CI-enforced release gate, rehearsed incident playbook, on-call readiness, published SLO plus error-budget policy.
- A promotion vote is chaired by the engineering manager, with named participants and a written decision. Each piece passes or fails. No promotion on a partial pass.

The next chapter zooms out from a single feature to the roadmap-wide question of how to plan capacity against a vendor stack that keeps releasing underneath you.
