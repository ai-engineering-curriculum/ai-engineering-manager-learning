# Exercise 01: AI On-Call Runbook for One Product

**Estimated effort:** 3 hours · **Output:** (a) a short on-call design memo for one real (or realistic) AI product, and (b) two runbook pages — one for an AI-specific failure mode drawn from the chapter 02 catalogue, one for a product-specific failure mode you identify yourself.

## Objective

Produce the two artefacts chapter 03 commissions. The design memo is the shape of the on-call rotation on top of the SRE substrate for one product. The runbook pages are the pages the on-call engineer opens at 03:14 on a Tuesday when the pager goes off; they must be concrete enough for a less-experienced engineer to execute without calling anyone.

A peer AI Engineering Manager, reading your artefacts, should be able to point at any one of the ten failure modes from chapter 02, read the on-call design memo, and know who gets paged on it; then read your runbook page for that failure mode and, within 90 seconds, execute the first response step.

## Prerequisites

- Chapter 01 at a glance for the four manager-owned artefacts and the four deferrals.
- Chapter 02 — the ten AI-specific failure modes catalogue.
- Chapter 03 — the on-call design, severity tiers, AI-shaped observability stack, four mitigation levers, runbook anatomy, and the dry-run cadence.
- Mod-104 chapter 06 for the online-eval cadence that produces several of the AI-side signals.
- Mod-106 chapter 05 for the release-triad incident-playbook pin the runbooks are the content of.
- The Google SRE Workbook chapters on incident response and the on-call rotation (free online) — background for the SRE substrate this exercise plugs into.
- A real AI product, or a plausible composite product with a declared severity tier (per mod-106 chapter 05).

## Problem statement

Two artefacts, in markdown, filed where runbook content lives (runbook repo, Confluence, Notion, team wiki — pick one and justify in a sentence).

### Artefact A — the on-call design memo (1–2 pages, 60 minutes)

The design memo answers "for this product, who gets paged on what, at what severity, with what substrate, under what cadence." Sections:

1. **Product identification.** One paragraph. Product name, surface description (chatbot / agent / RAG system / classifier-wrapper / etc.), deployment context, severity tier (HIGH / MEDIUM / LOW per mod-106 chapter 05), audience, data-class posture (regulated / consumer / internal / etc.).
2. **The on-call shape.** How the rotation is structured on top of the SRE substrate. Specifically:
    - Is there a dedicated AI-side rotation, or an SME-escalation model (chapter 03 discipline 1)?
    - Primary on-call target (PagerDuty service / Opsgenie team / equivalent).
    - Secondary and tertiary escalation targets, by named role (AI Risk lead, Applied AI SME, Security on-call, Counsel, Comms partner).
    - Who is paged at each severity tier; the specific thresholds that upgrade severity.
3. **The AI-shaped observability stack.** The four categories from chapter 03 core concept 2 (model-quality, safety-guardrail, agentic / tool-use, operational). For each:
    - Which signals exist today vs. which need investment.
    - The dashboards they live on (one named dashboard per category is enough; the AI engineering manager is requiring that the signals exist, not drawing the chart).
    - The alarms wired against them — thresholds, windows, paging targets.
4. **The runbook-library index.** A table listing all ten failure modes from chapter 02, plus any product-specific additions you identify, with columns: `runbook-page` (path), `monitor-id` (the specific alarm), `severity-table-row` (SEV1 / SEV2 / SEV3 trigger per the chapter 03 trigger table), `last-reviewed`, `last-dry-run`, `owner`. Mark pages as `[placeholder]` where they do not yet exist; the index is the manager's coverage view.
5. **Dry-run cadence.** Which quarterly tabletops the rotation runs, which annual dry-runs are scheduled, how the rotation handles the on-call compensation / fairness side (one paragraph, defer to mod-109 for the deeper design).
6. **Escalation matrix.** A compact table: for each severity tier (SEV1 / SEV2 / SEV3 / SEV4), who is paged automatically, who is paged on specific triggers (user-side harm, prohibited-category output, data breach, scaled misbehaviour on a regulated surface, attack shape — chapter 04 core concept 5).

The memo is the shape, not the content; it points at the runbook pages rather than inlining them.

### Artefact B — two runbook pages (two pages, 90 minutes combined)

Two full runbook pages, each following the chapter 03 ten-section anatomy. One is drawn from the chapter 02 catalogue (pick any one — jailbreak, injection, hallucination-with-harm, model-vendor regression, judge drift, tool-use failure, embedding corruption, retrieval degradation, false-positive storm). The second is a **product-specific failure mode** you identify (not already in chapter 02) — something unique to the product you picked. Examples:

- A medical-adjacent product: "self-harm crisis signal handoff failure" — the moderation pipeline detected a crisis-category input but the HITL warm-transfer failed.
- A developer tool: "agent commits malformed code to a customer's main branch" — tool-use failure specialised to the surface.
- A customer-support product: "agent misidentifies customer tier and routes refund decision outside policy" — tool-use + authorisation specialisation.

For each page, fill all ten sections per chapter 03 core concept 4:

1. **Signature** — one paragraph.
2. **Severity-tier table** — the row from the chapter 03 trigger table, adapted to the product.
3. **First response, first 15 minutes** — ack, channel open, role assignment, pinned-runbook, opening-message template, who to page for help. Concrete commands / toggles / slash-commands.
4. **Evidence preservation (first)** — the specific traces / dashboards / registries to snapshot, with commands or dashboard paths. Preserve-before-mitigate.
5. **Mitigation** — the specific lever(s) from the four, with commands / toggles / configuration. Order the composition explicitly.
6. **Communication templates** — internal channel update template, customer-facing template (if applicable), status-page language, exec-briefing summary. Pre-drafted, with placeholders.
7. **Escalation matrix** — who gets paged / looped at each tier for this specific failure mode (reference artefact A's matrix where possible).
8. **Root-cause anchors** — three specific questions the postmortem will want answered.
9. **Rollback target** — the specific prior version(s) / registry entries.
10. **Known-adjacent failure modes** — links to runbook pages for failure modes that often co-occur.

Each page is ~1 page in length (plus the embedded templates and commands). Each has a `last-reviewed`, `last-dry-run`, and `owner` line. If the pages reference specific infrastructure (dashboard IDs, feature-flag names, kill-switch names), use plausible placeholders (`<flag:moderation.input.threshold>`, `<dashboard:GRD-chat-07>`) that make the shape clear.

### Remaining time (30 minutes)

Dry-run one of the two runbook pages. Walk through the page as though you have just been paged — read the signature, decide the severity, open an imaginary channel, say what you would type as the opening message, run (mentally) each mitigation step, identify any step that is ambiguous or missing a prerequisite. Note the gaps in a 10-line changelog at the bottom of the page.

## Requirements

- Design memo is 1–2 pages, pointed at the runbook-library index and the escalation matrix.
- Memo names the on-call shape (dedicated AI rotation vs. SME escalation), the paging substrate, and the specific signals per observability category.
- Runbook-library index covers all ten failure modes from chapter 02 plus product-specific additions; missing pages are marked `[placeholder]`.
- Two runbook pages are filled, each following the ten-section anatomy.
- One page is drawn from the chapter 02 catalogue; one is product-specific and distinct from the catalogue.
- Each page carries `last-reviewed` and `last-dry-run` dates; `owner` named.
- First-response section includes the concrete opening-message template.
- Evidence-preservation section includes specific commands / dashboard paths (placeholders are acceptable for commands; dashboards should be named by ID).
- Mitigation section lists the specific levers, in order, with concrete toggles / commands.
- Communication templates are pre-drafted with placeholders.
- Escalation matrix lines up with artefact A's matrix.
- Dry-run changelog identifies at least two gaps or improvements.

## Starter guidance

- Pick a product you know well. The exercise is in the specifics; a hypothetical product leads to a hypothetical runbook. If you must use a hypothetical, pin the surface specifics (RAG agent with 3 tools, medical-adjacent, EU + US deployment) at the top.
- The design memo is the shape; resist the temptation to write runbook content in the memo. If you find yourself writing mitigation steps in the memo, promote them to the runbook page.
- The runbook-library index is where coverage gaps become visible. A `[placeholder]` entry is a commitment that the page is on the backlog with a target date; use it liberally — missing pages are more useful to name than to pretend about.
- Pre-drafted communications are the difference between a page that holds up in an incident and one that doesn't. If the template in your page says "update the channel," it is not a template; "Confirmed/investigating <failure mode> on <surface>. Preserving evidence. ETA for next update: 15 min." is a template.
- On commands and toggles: placeholders with specific *names* (`<flag:moderation.input.threshold>`) are more useful than generic language ("tighten the input moderator"). The name is where the on-call engineer's eyes land; the actual value is tuned in prod.
- The product-specific failure mode is often the most valuable page to write. The catalogue covers the industry's shared failure modes; your product's surface has shapes only your team will see. Articulating them in the exercise is where the exercise pays off.
- On severity thresholds for the product-specific failure mode: write the SEV1 trigger before the SEV2 and SEV3. SEV1 is the hardest to specify and the most important to get right; if you can articulate "what does this failure look like at the user-harm altitude," the lower severities follow.
- Resist the temptation to inline chapter 02's text in the signature section; summarise in your own words. A runbook page the on-call engineer re-wrote themselves is one they will read in the incident.
- The dry-run changelog is worth taking seriously. The gaps you find walking through the page mentally are the gaps a real on-call engineer will hit; the only difference is cost.
- Cross-reference where you can — a runbook page that points at the fixture in the eval registry (mod-104 chapter 04), at the release-triad pin (mod-106 chapter 05), and at adjacent runbook pages is a page that holds the module together.

## Acceptance criteria

- Design memo is 1–2 pages; names the on-call shape, paging substrate, observability stack across the four categories, runbook-library index, dry-run cadence, and escalation matrix.
- Runbook-library index covers all ten chapter 02 failure modes plus ≥ 1 product-specific addition.
- Two full runbook pages produced, each following the ten-section anatomy from chapter 03.
- One runbook page is from the chapter 02 catalogue; one is product-specific.
- Each page carries `last-reviewed`, `last-dry-run`, `owner`.
- Each page's first-response section includes a concrete opening-message template and role-assignment pin.
- Each page's evidence-preservation section is specific (commands or named dashboards, not generic language).
- Each page's mitigation section names specific lever(s) in order, with concrete toggles / commands / placeholders.
- Each page's communication templates are pre-drafted with placeholders.
- Each page's escalation matrix references artefact A.
- Each page's root-cause anchors section has three specific questions for the postmortem.
- Dry-run changelog identifies ≥ 2 gaps or improvements.
- A peer AI Engineering Manager can read either runbook page in 90 seconds and execute the first-response step.

## Stretch goals

- **Full library.** Instead of two runbook pages, produce the full set — ten chapter 02 pages plus product-specific pages — at the same depth. Significantly more work; produces the actual library.
- **Dry-run script.** Write a short dry-run script (half page) that a facilitator could run for one of your runbook pages with a four-person team. Include scenario injection, expected decisions, scoring rubric.
- **Monitor spec.** For each of the four observability categories in the design memo, write a one-paragraph monitor spec (what is measured, from what data source, with what threshold, with what alerting rule). Hand-off-able to a Platform / SRE engineer.
- **On-call rotation economics.** Add a short analysis to the design memo of the rotation's on-call load, pager noise, and compensation posture. Partner with SRE / Platform where the substrate lives. Defer to mod-109 for depth; the exercise version is a two-paragraph opener.
- **Severity-trigger-table calibration.** For each severity-tier row, append a one-line historical calibration — "the last three SEV2-triggering events on our surface were X, Y, Z; the threshold in this table would have caught all three (or: would have caught Y but not X / Z, and here is why)." The calibration is where the trigger table becomes believable.
- **Cross-reference to mod-107 chapter 06.** For one runbook page, append a section anticipating the postmortem's fixture requirement (chapter 06) — what trace shape this incident will produce, what fixture class the fixture-author should expect to write. The cross-reference ties the exercise to the next exercise.
