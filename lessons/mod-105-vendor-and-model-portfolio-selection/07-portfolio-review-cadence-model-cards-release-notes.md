# Portfolio Review Cadence, Model Cards, and Release Notes

The first six chapters build the artefacts: inventory, selection memos, fallback plan, DPA posture, and (where justified) a capability brief for self-host. This chapter is about keeping them alive. A vendor-and-model portfolio managed once at project start and never again is not a portfolio; it is a snapshot that was accurate on one Tuesday. The frontier moves every quarter. The artefacts must move with it.

The cadence has three beats: a **quarterly portfolio review** that is the standing ritual, a **mid-quarter rapid-add** path for capabilities that cannot wait, and a **decommissioning calendar** for models and vendors on the way out. Running underneath all three is the manager's reading discipline: Model Cards, system cards, vendor release notes, and vendor deprecation schedules, read as part of the job.

## Motivation

Three failure shapes recur when no cadence runs.

- **The quiet staleness.** Nothing in the portfolio has been reviewed in a year. The eval evidence column (chapter 02) still reads the first model the team adopted. Three frontier generations have landed in the meantime. The team is behind on quality and cost both; nobody has noticed because no scheduled moment forces the comparison.
- **The surprise deprecation.** The vendor emails a 90-day deprecation notice for a model two of the team's features depend on. The team has not benchmarked the successor. The migration plan does not exist. On day 30 the team is scrambling; on day 60 the migration is shipping unevaluated; on day 90 production is on a model nobody signed off on and the online eval (mod-104 chapter 06) is the only backstop.
- **The capability gap discovered by a competitor.** A new frontier capability lands at one of the labs — reasoning-mode inference, computer-use, a new context ceiling, a new audio or vision modality, a dramatic latency improvement on a specialist provider. Six weeks later a competitor ships a feature using it. The AI Engineering Manager's team is still on the capability the portfolio captured last quarter. The mid-quarter rapid-add path, if it existed, would have caught this inside two weeks.

The three beats below address each failure directly.

## Core concept 1 — the quarterly portfolio review

A standing meeting. Thirty to sixty minutes. Attendees: the AI Engineering Manager (owner), the eval engineer who owns the eval suite (mod-104), one IC applied engineer, the peer AI Infra manager (if self-host is in the portfolio), a procurement liaison on a rotating basis. Output: a one-page memo filed to the team's governance surface.

The agenda is fixed. Walking a fixed agenda every quarter is the discipline that keeps the review from becoming a status meeting.

### The six agenda items

1. **Inventory refresh.** The portfolio inventory (chapter 02) is opened; each row is updated against the vendor's current docs. Prices, context windows, latency numbers, capability tags, DPA posture flags — all are dated. Entries whose numbers have not changed are marked "reviewed, unchanged" with the review date. Entries with material changes are flagged for memo-level attention in item 2 or item 4.
2. **Selection-memo review.** Every feature's selection memo (chapter 03) is opened. The exit criteria from each memo are checked against the last quarter's data. Memos that trigger an exit criterion are re-opened as a formal re-selection. Memos whose eval numbers have drifted but not past exit criteria are noted on a watch list.
3. **Fallback plan check.** The fallback plan per feature (chapter 04) is verified: abstraction layer in place, triggers configured, DPA parity holds, tabletop ran this quarter, restore drill is scheduled or completed. Gaps are tasked.
4. **DPA and contract posture.** Chapter 05's checks: has any vendor amended its DPA? Has the sub-processor list changed? Has a deprecation notice landed? Has a new DPA feature (ZDR newly available, HIPAA BAA newly available, a new region) opened up that the team should adopt? Items escalate to procurement or counsel as appropriate.
5. **Candidates and watch list.** New models or new providers that have landed since the last review. The team's own eval runs on new candidates (if any have been commissioned). A short list of what the team should be benchmarking in the coming quarter; owner and target eval-run date per item.
6. **Decommissioning.** Models and vendors scheduled for removal in the coming quarter — deprecated by the vendor, exited by exit criteria, replaced by a selection re-opening. Timeline, owner, migration plan link.

### The one-page memo

The output is one page, filed where the team's governance artefacts live. Minimum:

- Date, attendees, next review date.
- Material changes to the inventory since last review (three to six bullets).
- Features with re-opened selection memos (named).
- Fallback-plan gaps tasked this quarter (named).
- DPA / contract actions this quarter (named).
- Candidates on the eval-commission list for next quarter (named).
- Decommissionings on the calendar (named with dates).

The memo is short on purpose. If it grows past a page, it is becoming a design doc; split the detail out into the relevant feature's selection memo or the specific escalation thread.

### Who attends

The AI Engineering Manager owns the room. The eval engineer brings the eval evidence. The applied engineer brings the production signals (routing stats, fallback engagements, online-eval deltas). The peer AI Infra manager attends when self-host or inference-platform choices are in scope. Procurement attends on a rotating basis — not every review, but at least once a half — so the commercial posture has a voice at the table.

What the review is *not*: a model-selection meeting. Selection memos are written between reviews, with their own evidence; the review ratifies them, not generates them. A quarterly review that becomes the venue for live model-selection debate has consumed itself into a status meeting.

## Core concept 2 — the mid-quarter rapid-add path

Sometimes a capability lands that cannot wait for the next quarterly review. The vendor ships a new modality, a new price tier, a new region, a new frontier-generation model; a specific product opportunity requires the team to evaluate and potentially adopt within weeks. The quarterly cadence is too slow. The mid-quarter rapid-add path is the pre-authorised mechanism for acceleration.

The rapid-add is explicit on four points:

- **Who can invoke it.** The AI Engineering Manager. In some orgs, the product manager in partnership with the AI Engineering Manager. Not a feature-team lead unilaterally; the invocation triggers organisation-level cost and review surface.
- **What it buys.** A commissioned eval run within two weeks on the new candidate against the surface's eval suite, a draft selection memo (or a draft capability brief if self-host is in scope), a DPA review if the candidate is a new vendor, an expedited slot on procurement's queue if a contract is required.
- **What it costs.** Expedited eval runs cost eval-engineer time; expedited procurement cycles use goodwill in both directions. The manager invokes rapid-add *when it is warranted*, not every time a vendor ships a release. A rough rule-of-thumb: more than one rapid-add per quarter is a signal that the cadence (or the manager's filtering) is miscalibrated.
- **How it closes.** The rapid-add produces either (a) a signed selection memo that updates a feature's primary (or fallback), ratified at the next quarterly review retroactively, or (b) a written decision not to adopt, filed with the reason, so the next rapid-add temptation knows the answer.

The rapid-add is the pressure valve. Without it, every capability-gap conversation becomes an ad-hoc override of the quarterly cadence; with it, the ad-hoc is contained to one named process.

## Core concept 3 — the decommissioning calendar

Every model and every vendor eventually leaves the portfolio. Four reasons:

- **Vendor-initiated deprecation.** The vendor publishes a notice: this model will be retired on `YYYY-MM-DD`. The migration must complete before the retirement.
- **Exit-criterion fire.** A selection memo's exit criteria fire. The model (or vendor) is no longer the right choice; the migration is scheduled.
- **Portfolio consolidation.** The team has accumulated more vendors than it wants to operate; the quarterly review identifies one to retire.
- **Compliance / contract reason.** A DPA posture change (chapter 05), a sub-processor change the organisation cannot accept, a regulatory development.

The decommissioning calendar is a simple artefact: a dated list of what is leaving the portfolio when, with an owner and a migration status. Opened at every quarterly review; worked between reviews; closed when the model is no longer production-callable.

For vendor-initiated deprecations specifically, the manager tracks the *notice window* the vendor committed to in the DPA (chapter 05's model-deprecation clause) and the *actual* notice given. Vendors that reliably honour the notice are more defensible primaries; vendors that do not are documented at the next renewal.

The decommissioning calendar also captures **removal of obsolete entries from the inventory**: the row for the deprecated model stays until the last production dependency is off, then moves to an archive section. The archive is kept because the next time the team considers that model (an older snapshot sometimes outperforms newer ones on a specific surface), the historic eval evidence is already on file.

## Core concept 4 — Model Cards and system cards as the manager's reading

A Model Card is a standardised document a model provider publishes alongside each model release, documenting the model's intended use, training-data provenance (where disclosed), evaluation results, known limitations, safety considerations, and the provider's recommended guardrails. The origin is Mitchell et al.'s 2018 paper *Model Cards for Model Reporting*. In 2024-2026 the convention has matured into **system cards** (the frontier labs' extended format — Anthropic's "Claude [version] System Card", OpenAI's GPT system cards, Google's Gemini technical reports, Meta's Llama release documents) and **Responsible Scaling Policy / Preparedness / Frontier Safety Framework** documents (lab-level governance).

The AI Engineering Manager's required reading, per release:

- **Intended use and out-of-scope use.** What the lab has designed the model for, and what they explicitly decline to warrant. If the team's feature sits in the out-of-scope column, the selection memo must address it or decline the model.
- **Evaluation summary.** The benchmarks the lab ran. Not a substitute for the team's own eval (chapter 03's anti-pattern 2), but a direction-of-travel signal. A dramatic improvement on a benchmark relevant to the team's surface is a candidate for the rapid-add path.
- **Safety and alignment work.** What the lab did on jailbreak-resistance, policy-refusal, bias and fairness, and the categories specific to the lab's framework (RSP risk categories, Preparedness evaluations, FSF Critical Capability Levels). The team's own safety eval (mod-104 chapter 02) is still the floor; the lab's work is context.
- **Known limitations.** The honest section — where the lab tells you what the model is still weak on. Features whose shape sits on a named limitation should wait for the next release.
- **Training-data provenance and data-use policy.** For procurement and counsel, not only for the manager. The Model Card may say more than the DPA on specifics; both are read.
- **Deprecation and succession.** The Model Card may name the predecessor and the deprecation window; the decommissioning calendar is updated.

The manager does not write a book review. The manager reads enough to update the inventory row (chapter 02), to decide whether a rapid-add is warranted, and to update the selection memos whose exit criteria reference capability (chapter 03). A thirty-minute read per release is typical; larger generation shifts justify more.

### The Responsible Scaling / Preparedness / Frontier Safety frameworks

Each frontier lab publishes a governance document that describes how *they* decide whether to release a model and under what safeguards. Anthropic's *Responsible Scaling Policy*, OpenAI's *Preparedness Framework*, and Google DeepMind's *Frontier Safety Framework* are the three to track. These documents are not a substitute for the team's own safety eval, and they do not transfer safety obligations to the vendor. They are an input into the question "how much do I trust this vendor's upstream governance" and into the mod-104 commissioning brief (risk posture section). Read them at least annually, and when they are substantially updated.

## Core concept 5 — vendor release notes and deprecation schedules

Every vendor publishes release notes and (usually) a deprecation schedule. Subscribing to both is the manager's discipline, not an individual engineer's.

- **Release notes.** API behaviour changes, new models and model snapshots, new capabilities (tool-use shapes, structured output modes, audio or vision modalities), pricing changes, region additions. Not every release note matters; the ones that affect the current inventory or open a rapid-add candidate matter.
- **Deprecation schedules.** Models announced for retirement, with dates. The decommissioning calendar is updated from here. A healthy practice: a monthly check (not quarterly — deprecation windows are often shorter than the quarterly cadence) against the published schedule for each vendor the team uses.
- **Changelog on DPAs and sub-processor lists.** Chapter 05's monitoring sits here. Vendors differ in how actively they notify; some require explicit subscription.
- **Status pages and incident history.** Tracked by on-call (mod-107's territory) but the manager reviews the last-quarter history as part of the fallback-plan check (quarterly agenda item 3).

The practical artefact: a small page or document that lists, per vendor, the URLs to monitor (release notes RSS, deprecation page, DPA page, sub-processor page, status page), who owns each subscription, and the last-reviewed date per page.

## A worked example — the quarterly review in motion

A team six months in on the customer-support agent from chapters 02–04 runs its second quarterly portfolio review.

- **Inventory refresh.** Anthropic prices updated; OpenAI released a new snapshot of the primary-family model; Together AI added a new region. The inventory row for Anthropic's prompt-caching rate has moved; the Azure OpenAI row's SLA tier has been renegotiated by procurement. Two rows updated with new eval numbers from last month's online eval.
- **Selection-memo review.** The customer-support agent's exit criteria did not fire. Online eval is trending steady. No re-open. One other feature's exit criterion fired on faithfulness regression two months running; re-selection scheduled for next quarter.
- **Fallback plan check.** Tabletop ran in week 4 of the quarter; output noted that the embedding model also sits with the primary and should have its own fallback (gap tasked). Restore drill completed at 2% traffic for one hour; eval scores held on fallback, latency slightly worse as expected.
- **DPA posture.** Anthropic amended their DPA to add a new sub-processor; the organisation has 30 days from notice (within SLA). Sub-processor reviewed; acceptable; customer-facing DPA notice issued by the DPO team. OpenAI released a new ZDR mode; procurement opened a conversation about adoption.
- **Candidates.** A new release from Google DeepMind on an extended-thinking model variant — the eval engineer will commission a run against the agent's eval suite in the coming quarter. A specialist-inference provider has announced a cost reduction; being watch-listed for the asynchronous-batch workload on a separate feature.
- **Decommissioning.** One embedding model on the team's old retrieval feature is deprecated on `YYYY-MM-DD`; migration to the current recommended embedding is in progress, owner named, ships by week 8.

Output: a one-page memo filed. The inventory, the selection memos, the decommissioning calendar, and the subscription log are all updated. Next review is three months out.

## What good looks like

- Quarterly portfolio review happens on a fixed cadence; memos are filed; nothing lands as a surprise.
- Mid-quarter rapid-add path is pre-authorised; invoked no more than once or twice a quarter.
- Decommissioning calendar is opened at every review; migrations ship before the vendor's deprecation date, not after.
- Model Cards for new releases are read within a week of landing.
- Release notes, deprecation schedules, DPA pages, sub-processor pages, and status pages have named subscribers; subscriptions are checked monthly.
- The inventory, the selection memos, the fallback plans, the capability briefs, and the redline list all carry a last-reviewed date; staleness is visible.

## What bad looks like

- The portfolio review is on the calendar but keeps getting skipped; memos do not exist.
- A deprecation notice lands and the team finds out from a 4xx error.
- A new release lands and the team learns about it from a blog post a competitor wrote.
- The rapid-add path does not exist; every capability gap becomes an escalation; every escalation runs ad-hoc.
- Decommissioning never happens; obsolete models and unused vendor contracts accumulate indefinitely.
- Nobody reads Model Cards; "we use `[model]`" is as deep as the team's knowledge of the model goes.

## Summary

- The vendor-and-model portfolio has a three-beat cadence: quarterly review, mid-quarter rapid-add, decommissioning calendar.
- The quarterly review runs a six-item agenda (inventory, selection memos, fallback, DPA, candidates, decommissioning) and files a one-page memo.
- The mid-quarter rapid-add path is pre-authorised for capabilities that cannot wait, invoked sparingly, and ratified at the next quarterly review.
- The decommissioning calendar tracks every model and vendor leaving the portfolio, with owner and migration status.
- Model Cards and system cards are read on release for intended use, evaluation summary, safety work, known limitations, and training / data posture. RSP / Preparedness / FSF frameworks are read annually and on material update.
- Vendor release notes, deprecation schedules, DPA pages, sub-processor pages, and status pages are monitored with named subscribers and a monthly cross-check.

This closes the module. The four manager-owned artefacts — portfolio inventory and selection memos, fallback and routing plan, DPA and contract posture, review cadence and Model Card discipline — are the operating system around model and vendor choice. The exercises take each in turn and apply it to one product you ship or plan to ship. The exercises are not optional reading; they produce the artefacts the operating system runs on.
