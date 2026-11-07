# Exercise 03: Release Triad Go/No-Go Template

**Estimated effort:** 3 hours · **Output:** (a) a reusable 1-page launch-memo template with explicit evidence requirements per leg, (b) a filled copy of the template for one real (or realistic) upcoming launch, and (c) a half-page override-protocol document the template references.

## Objective

Produce the three artefacts chapter 05 commissions. The template is the shape your team runs releases against; the filled copy is the evidence that the template works on a real launch; the override protocol is the discipline that keeps the triad honest when a demo-week push wants to bypass it.

A peer AI Engineering Manager, on reading the filled template, should be able to decide "ship / do not ship" in five minutes, and know exactly which signature is missing or which leg is amber.

## Prerequisites

- Chapter 05 (`The Model + Data + Safety Release Triad`) for the three legs, the incident-playbook pin, the one-page memo shape, the override protocol, the severity tiers.
- Chapter 01 at a glance for the four manager-owned artefacts the triad reads.
- Exercises 01 and 02 — the guardrails spec and the red-team brief are inputs the safety leg points to.
- Chapter 03 of mod-104 (release gates), chapter 03 of mod-105 (selection memo), chapter 05 of mod-105 (DPA posture) — the triad is the composition of these artefacts into a single gate.
- A real upcoming launch at your organisation, or a hypothetical launch on a composite team with the feature's severity tier you declare.

## Problem statement

Three artefacts, in markdown, filed where release artefacts live (runbook repo, product-release tracker, or equivalent).

### Artefact A — the launch-memo template (1 page, 45 minutes)

A reusable template that the team fills per launch of consequence. The template carries placeholders, section headers, and inline guidance; not evidence. Content:

1. **Header** — feature name, version, launch date, severity tier (declared per chapter 05 core concept 3), owner (manager), feature IC lead, red-team lead, data owner, on-call lead.
2. **Model leg** — placeholder table / inline-guidance for: eval report run id and headline metric; selection memo link; shadow / canary summary; IC attestation signature. One-line reminder of the "GREEN definition" from chapter 05 core concept 1.
3. **Data leg** — placeholder table / inline-guidance for: sub-processor diff since last launch; DPA posture; data residency per audience; retention windows; legal / procurement sign-off. One-line reminder of the "GREEN definition."
4. **Safety leg** — placeholder table / inline-guidance for: guardrails spec date and version; red-team engagement date and outcome counts (critical / high / medium / low); accepted-risk entries with ids and expiries; automated safety-eval dashboard id; safety reviewer sign-off. One-line reminder of the "GREEN definition."
5. **Incident playbook pin** — placeholder for: runbook pages present (list); last dry-run date; rollback trigger; rollback target; on-call lead sign-off.
6. **Known-issues / accepted-risks register snapshot** — placeholder table for ids, severities, owners, expiry dates for everything this launch ships with.
7. **Sign-off block** — signature lines for all five roles with "I have read the evidence linked above and found it adequate for launch at the declared severity tier."
8. **Next review** — placeholder for review-clock dates (monthly for 3 months, then quarterly; triggered on material change).

Include **inline instructions** (as HTML-style comments, or in a sidebar) for each placeholder — what counts as GREEN vs. AMBER, how to fill the row, when to escalate. The template should be usable by a less-experienced manager on a different team.

### Artefact B — the filled template for one real (or realistic) launch (1 page of template + evidence links, 75 minutes)

Pick one upcoming launch (real or realistic). Fill the template. Guidance:

- Use real values where you have them; use `[placeholder: …]` with a specific action and date where you do not.
- At least one leg should be **AMBER** with a concrete amber-resolution plan (chapter 05 core concept 2 element 2). A launch with three greens and no ambers is possible but should be the exception, not the default; a template that cannot express amber is not useful.
- At least two accepted-risk entries in the known-issues register, each with a dated co-signature, an expiry, and a mitigation summary.
- Rollback trigger and rollback target are specific — "if content-moderator false-negative rate exceeds X% over Y minutes, roll back to version Z" — not generic.
- Signatures are named (or `[placeholder: <role> to sign by <date>]`).

The filled template is primarily a **proof** that the template works; it is a second-order artefact. Keep it short.

### Artefact C — override-protocol document (0.5 page, 30 minutes)

A half-page document the launch-memo template references under its sign-off block. Content:

- **Who can override.** Named roles that constitute the two-signature override quorum per leg. Different legs may have different quorums (data-leg override may require legal counsel; safety-leg override may require head of security / risk; model-leg override may require VP engineering).
- **How overrides are logged.** In the same launch-memo artefact, with a dated rationale and a time-bound commitment ("override valid for N days").
- **The expiry-and-rollback rule.** By the expiry, the overridden leg is re-run GREEN or the launch is rolled back; neither "the override is renewed" nor "we're just going to leave it" is an option.
- **The pattern-review trigger.** Two overrides on the same leg in a quarter trigger a pattern review; the pattern review's output is either a gate adjustment, an investment, or a confirmation that the current discipline is correct and the pressure is misdirected.
- **Who may not override.** For a HIGH-severity feature, name any override that is explicitly out of bounds (an EU AI Act Article 14 obligation cannot be overridden by a "we'll ship it anyway"; a known-critical red-team finding cannot be overridden except by the CISO-level role, if that; etc.).

### Remaining time (15 minutes)

Dry-run the template with a different scenario. Pick a different severity tier (if your filled copy was HIGH, try MEDIUM; if LOW, try HIGH). Walk through the template; do the evidence requirements still make sense; does the template guide a less-experienced manager to the right call; are there sections that are too strict for the lower tier or too loose for the higher tier? Note any revisions in a short changelog at the bottom of the template.

## Requirements

- Template is 1 page, reusable, carries inline guidance that is a less-experienced manager could follow.
- Template names all three legs plus the incident-playbook pin, with GREEN definitions inline.
- Template carries signature lines for all five roles with explicit "I have read the evidence" attestation language.
- Filled copy carries at least one AMBER with a concrete resolution plan, at least two accepted-risk entries with dated signatures and expiries.
- Filled copy's rollback trigger is specific (numbers + target version, not "if things look bad").
- Override-protocol document is a half page; names roles, logging, expiry-and-rollback rule, pattern-review trigger, and out-of-bounds overrides for HIGH tier.
- All three artefacts cross-reference each other (template to override protocol; filled copy to template).
- Template cites the chapter 05 severity-tier rubric so a filler can calibrate.

## Starter guidance

- The template is the artefact, not the filled copy. Spend 45 minutes on the template, not 20; the filled copy is proof the template works.
- A good template is boring on purpose. Resist decorating it with checklists that are not decisions. A reviewer who sees 12 checkboxes stops reading at the third; a reviewer who sees three sign-off blocks and three evidence columns reads them all.
- The GREEN definitions inline are the difference between a template that works and one that is a scaffold. Copy the chapter 05 definitions and tighten them to your team's thresholds where you can.
- The override protocol is the part the team will be most tempted to skip. If your team has never had an override, write the protocol as though the next launch will be the one — not because you expect it, but because the cost of not having the protocol when you need it is high.
- For the filled copy, resist the temptation to backfill an accepted-risk entry that does not have a real owner. If you do not know who co-signs, write `[placeholder: co-signature from <role> required]` and name the role.
- Rollback triggers are often where filled copies fail. The trigger is a number against a dashboard; the target is a version string. Both should be copy-pasteable.
- Sign-off language matters. "Signed" is not an attestation; "I have read the evidence linked above and found it adequate for launch" is. Future-you will thank present-you for including the phrase.
- Resist expanding the template with edge cases. A launch whose shape requires a custom section should be the exception; the template covers 95 % of launches.
- Cross-reference where you can — a template that points at the guardrails spec from exercise 01 and the red-team brief from exercise 02 is a template the team will actually trust as the authoritative gate.

## Acceptance criteria

- Template is 1 page; all three legs + pin + known-issues register + sign-off block + next-review placeholder present.
- Each leg in the template carries its GREEN definition inline.
- Template is reusable: rename the feature, swap the IDs, and it still works.
- Filled copy carries ≥ 1 AMBER with an explicit resolution plan.
- Filled copy carries ≥ 2 accepted-risk entries with dated signatures, co-signatures (where severity warrants), mitigations, and expiries.
- Filled copy's rollback trigger is a number + a target version.
- Override-protocol document is 0.5 page; all five sub-sections present.
- The three artefacts cross-reference each other.
- A peer AI Engineering Manager can read the filled copy in 5 minutes and decide ship / do not ship with confidence.

## Stretch goals

- **Severity-tier variants.** Produce the lighter MEDIUM and the paragraph-length LOW variants of the template (chapter 05 core concept 3) alongside the HIGH. The three live as one file with three sections or as three files that share a parent template — your choice; document the choice.
- **Review-clock automation sketch.** Write a one-page description of how the review-clock could be automated (calendar invites generated from the memo's "next review" field; expiry alerts on accepted-risk entries; a dashboard showing memos older than 90 days without re-sign). You are specifying, not implementing.
- **Dashboard pin.** Name three specific dashboards (eval regression, classifier false-negative rate, HITL queue depth) that the triad reads for the "monitor" leg; link those dashboards into the template.
- **Post-mortem cross-reference.** Append a half-page "when a triad-gated launch produces an incident" section cross-referencing mod-107 post-mortem practice — the triad memo for the launch is a required input to the post-mortem, specifically to answer "which leg should have caught this, and why did it not."
- **The capability-tier trigger.** Add to the template an explicit section that fires when the model-tier migration brief (mod-105 chapter 07) lands — the triad is re-signed as a *re-launch* when the model generation changes materially. Name the trigger explicitly so the team does not treat a migration as a "silent" change.
