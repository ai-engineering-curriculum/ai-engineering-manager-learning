# mod-107: AI-Specific Incident Response and Blameless Postmortems

> Seventh module of the AI Engineering Manager track. Builds the manager-owned operating system for what happens *after* a release of consequence is live: an on-call + incident path shaped for AI-specific failure modes, incident command adapted to a surface where prompts and traces are evidence, blameless postmortems that produce permanent eval fixtures rather than resolutions, a regulator-facing narrative where EU AI Act Article 73 applies, and a reading cadence against public AI-incident corpora so the team learns from other organisations' incidents and not only its own. SRE discipline defers to `sre-engineer` / `platform-engineer`; attack-side forensics defer to security operations / DFIR; red-team craft defers to `ai-risk-engineer` and `agentic-safety-engineer`; legal binding defers to counsel. This module teaches the commissioning, leadership, and gating discipline the manager owns around all of them.

**Estimated effort:** 14 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Design the on-call and incident path for AI-specific failure modes (jailbreak, prompt injection, hallucination, regression after model-vendor upgrade, judge drift, tool-use failure, embedding-store corruption, retrieval degradation, outage, moderation-classifier false-positive storm) — adapted from Google SRE Book, Google SRE Workbook, and Etsy's blameless-postmortem practice.
- Operate incident command across the AI-shaped surface: time to detect, time to mitigate (kill switch, routing-to-safer-model, rollback of prompt / judge / tool, on-call escalation to governance when a harm threshold is crossed), evidence preservation for root-cause analysis and compliance.
- Run blameless postmortems that produce permanent eval fixtures — every confirmed incident becomes a regression test in the eval suite (mod-104) and a judge case in the registry; the loop is enforced, not aspirational.
- Shape the incident narrative for regulators where EU AI Act Article 73 (serious-incident reporting for high-risk AI systems) applies — partner with governance / legal, do not author legal opinion; route SOC-side investigation to security operations / DFIR.
- Use the AI Incident Database (AIID), OECD.AI Incidents Monitor, and the MIT AI Risk Repository as reference corpora for what can go wrong — read regularly, not only after your own incident.

## Chapters

1. [The Manager's Altitude for AI Incident Response](01-the-managers-altitude-for-ai-incident-response.md) — what the manager owns (on-call shape, incident-command system, blameless postmortem discipline, incident-to-fixture loop, regulator narrative coordination) vs. what the IC owns (triage, code fixes, forensics); the four manager-owned artefacts; deferrals to SRE / Platform, security operations / DFIR, AI Risk, counsel.
2. [The AI Failure-Mode Catalogue](02-the-ai-failure-mode-catalogue.md) — ten AI-specific failure modes the on-call rotation must be ready for; per failure mode: what it looks like, how it is detected, how it is mitigated, what evidence must be preserved; how the catalogue reads out to the runbook library.
3. [Designing the On-Call and Incident Path](03-designing-the-on-call-and-incident-path.md) — SRE discipline adapted to AI surfaces; severity tiers; paging criteria; runbook anatomy; kill-switches, routing-to-safer-model, rollback mechanics; the AI-shaped observability stack; what manager owns vs. what SRE / Platform own.
4. [Incident Command for the AI-Shaped Surface](04-incident-command-for-the-ai-shaped-surface.md) — Incident Commander, Communications Lead, Scribe, Subject-Matter Lead roles; TTD and TTM metrics; the four mitigation levers; evidence preservation (traces, prompts, retrieved context, tool-call payloads) at an altitude that survives both root-cause analysis and compliance; escalation to governance when a harm threshold is crossed.
5. [Blameless Postmortems That Change the System](05-blameless-postmortems-that-change-the-system.md) — Etsy / Google SRE blameless practice carried to AI; the postmortem template with AI-specific fields (model / prompt / judge / dataset version, harm classification, red-team-finding cross-reference); the facilitation discipline; action-item maturity; the review cadence.
6. [The Incident-to-Fixture Loop](06-the-incident-to-fixture-loop.md) — every confirmed incident becomes an eval fixture (mod-104); the loop mechanics (capture → redact → fixture → judge-case → regression gate → close); ownership; "loop closed" definition; why the loop must be enforced, not aspirational.
7. [Regulator-Facing Incident Narrative](07-regulator-facing-incident-narrative.md) — EU AI Act Article 73 (serious-incident reporting for high-risk AI systems) at EM depth; the partnership with counsel and governance; the engineering-side artefact (facts, timeline, mitigations, impact assessment) that counsel's legal filing reads off; routing SOC-side forensics to security operations / DFIR.
8. [Reading the AI Incident Corpora](08-reading-the-ai-incident-corpora.md) — AI Incident Database, OECD.AI Incidents Monitor, MIT AI Risk Repository, plus other public corpora; what each is; a quarterly reading ritual; how the corpora feed tabletop scenarios, runbook content, and red-team briefs (mod-106 chapter 04).

## Exercises

- [Exercise 01 — AI on-call runbook for one product](exercises/exercise-01-ai-oncall-runbook-for-one-product.md) *(3 h)*
- [Exercise 02 — Blameless postmortem template with AI-specific fields](exercises/exercise-02-blameless-postmortem-template-with-ai-specific-fields.md) *(2 h)*
- [Exercise 03 — Incident-to-fixture loop](exercises/exercise-03-incident-to-fixture-loop.md) *(2 h)*
- [Exercise 04 — Regulator-facing incident summary draft](exercises/exercise-04-regulator-facing-incident-summary-draft.md) *(2 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-104, mod-105, mod-106.** The eval registry from mod-104 is where every incident lands as a fixture (chapter 06). The portfolio and fallback plan from mod-105 are inputs to the "routing-to-safer-model" mitigation lever (chapter 04). The release-triad incident-playbook pin from mod-106 chapter 05 is the pre-condition of every chapter in this module.
- **Pairs with mod-106 (safety and the release triad).** The red-team findings from mod-106 chapter 04 are a reference frame for incident classification (chapter 05); the guardrails spec and the authorisation boundary (mod-106 chapters 02, 07) are the artefacts the postmortem routinely changes.
- **Pairs with mod-108 (performance management).** The blameless discipline is a cultural investment the manager protects on perf reviews (mod-108). Growth of on-call competence is a growth-path dimension for ICs partnering here.
- **Pairs with mod-109 (team shape and operating cadence).** The on-call rotation, the tabletop cadence, the postmortem-review cadence, and the quarterly incident-corpus reading ritual are beats on the operating cadence mod-109 designs.
- **Pairs with mod-111 (cross-functional partnership).** The partnership with security operations / DFIR on forensics, with counsel on Article 73 reporting, with customer communications on user-facing incident messaging, and with governance on serious-incident classification are four cross-functional surfaces mod-111 covers.
- **Pairs with mod-112 (executive communication).** The incident-narrative discipline (chapter 07's engineering-side summary) is an input to mod-112's executive-communication artefacts — the regulator-facing summary the EM drafts for counsel is the same source the EM uses to brief the executive team on a serious incident.
- **Reads out to mod-110 (budget and FinOps).** The cost-blast-radius failure mode (chapter 02) and the "cost-blast runbook" (chapter 03) connect to mod-110's FinOps guardrails.
