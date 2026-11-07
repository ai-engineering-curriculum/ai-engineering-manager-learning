# Reading the AI Incident Corpora

The on-call system of chapter 03 pages on the team's own signals. The failure-mode catalogue of chapter 02 is built from what the industry has learned; the catalogue is only as current as the manager's reading of what the industry is *still* learning. Public AI-incident corpora — the AI Incident Database (AIID), the OECD.AI Incidents Monitor, the MIT AI Risk Repository, and a short list of adjacent resources — are the industry's shared memory of what has already gone wrong. An AI engineering manager who reads them quarterly meets fewer novel incidents on their own surface, writes better red-team briefs (mod-106 chapter 04), designs better tabletops (chapter 03 of this module), and brings better questions to the release triad (mod-106 chapter 05).

This chapter is the manager's reading practice. What each corpus is, what to extract from it, how often to read, and how the reading feeds back into the artefacts the module builds. The reading is a habit, not a side-effect of an incident — a team that reads the corpora only after its own incident is a team that discovers in its own postmortem what another organisation had already publicly documented six months prior.

## Motivation — three teams that did not read the corpora

- **The team that saw its first indirect-injection incident and thought it was novel.** The pattern was documented in Greshake et al. in 2023 and had been discussed in the AIID and in the OECD.AI Incidents Monitor for months. The team's postmortem framed the finding as "we didn't know this was possible" and drafted a response accordingly; the response was correct but uninformed, and the broadening stage of the incident-to-fixture loop (chapter 06 stage 4) had to be re-run twice to catch the known variants.
- **The team that wrote a red-team brief against a stale threat model.** The brief named three attack classes the team had thought of. The quarterly OECD.AI incidents summary of the same quarter contained four attack classes the team's brief did not mention, including two that had produced regulator-facing incidents at peer organisations. The team's red-team engagement found what the brief asked it to find; it did not find what the corpora would have suggested to ask about.
- **The team that read the corpora once a year and treated it as a compliance step.** The reading happened in January; the extraction was a one-paragraph bullet list in a document nobody read again. The remaining 11 months, the team was effectively blind to industry-visible patterns. The compliance box was checked; the learning did not happen.

Each is a specific failure of the reading practice. The design in this chapter is meant to prevent all three.

## Core concept 1 — the corpora, named

Four primary corpora and a short list of adjacent resources.

### Corpus 1 — the AI Incident Database (AIID)

The AI Incident Database is maintained by the Responsible AI Collaborative (originally initiated under Partnership on AI). It catalogues publicly-reported AI incidents in a searchable, taxonomy-tagged database. Each entry has a short description, source links, a taxonomic classification across multiple schemes (CSET, GMF, and others), and a record of reported harms.

**What it is good for.**
- A broad view of the kinds of AI incidents reported globally.
- Precedents to cite in red-team briefs ("this attack class produced incidents at X and Y").
- A searchable corpus by keyword, harm type, or industry sector.
- A sense of the shape and severity of incidents your product's surface might see.

**Where to find it.** `incidentdatabase.ai`. The site exposes the full database, taxonomy pages, and recent-entry feeds.

**Caveats.** The AIID catalogues *reported* incidents; it is not a random sample. Reported incidents skew toward consumer-facing products, public-sector systems, and attention-generating failures. Enterprise B2B incidents are under-represented. Severity classification is based on the reports, not on an independent evaluation. The AIID is a *starting point*, not a ground truth.

### Corpus 2 — the OECD.AI Incidents Monitor

The OECD.AI Policy Observatory maintains the AI Incidents and Hazards Monitor as part of its AI governance work. The Monitor is policy-focused: it catalogues incidents with an eye toward the regulatory frames and policy responses they have triggered, cross-referenced to the OECD.AI classifications and to member-country AI regulations.

**What it is good for.**
- The *regulatory* lens on AI incidents: which incidents triggered which regulator responses, in which jurisdictions.
- Cross-national pattern analysis — the same incident shape as seen in Germany, Singapore, Canada.
- Alignment with the frames counsel reads (OECD.AI classifications, policy-observatory reports).
- Context for the chapter 07 regulator-facing conversation: how similar incidents elsewhere were handled.

**Where to find it.** `oecd.ai/en/incidents` — the Monitor page. The full OECD.AI Policy Observatory at `oecd.ai`.

**Caveats.** Policy-focused incidents are visible; purely technical incidents without a policy footprint are less visible. The time lag between an incident and its appearance in the Monitor can be significant; the Monitor is not a real-time feed.

### Corpus 3 — the MIT AI Risk Repository

The MIT AI Risk Repository (housed at the MIT FutureTech / MIT CSAIL affiliation; see resources.md for current pointer) catalogues AI risks from the literature — harms, failure modes, mitigations — organised into a taxonomy. It is more *risk-catalogue* than *incident-catalogue*: it aims to be a comprehensive map of the risks an AI system might pose, with citations to the literature where each risk is discussed.

**What it is good for.**
- A structured taxonomy of AI risks; useful for red-team brief authoring (mod-106 chapter 04) and for failure-mode-catalogue updates (chapter 02 of this module).
- Citations to the primary literature for each risk; useful when a postmortem needs to reference a known risk framework.
- A broad, academic view of risks that may not have produced publicised incidents yet but are plausible.

**Where to find it.** `airisk.mit.edu` (verify current URL; the repository's hosting has moved with collaborator changes). The repository is also available as a downloadable catalogue.

**Caveats.** The Repository is a research artefact; not every entry maps cleanly to a production-engineering concern. The taxonomy is academic; translating a Repository risk into a runbook page or a fixture is non-trivial. Treat the Repository as a *source of candidate risks*, not a prescriptive list.

### Corpus 4 — provider / lab public incident notes

The frontier-model providers (Anthropic, OpenAI, Google DeepMind, Meta, Mistral, Cohere, others) publish, to varying degrees:

- **System cards** — per model release; include known limitations, evaluated risks, and some disclosed failures. Referenced from mod-106 chapter 06.
- **Status pages** — operational incidents; 5xx events, latency spikes, regional outages.
- **Safety / policy posts** — occasional writeups of specific incident classes, attack-pattern analyses, or patch notes.
- **Vulnerability-disclosure acknowledgements** — where bug-bounty programs exist, periodic summaries of accepted vulnerability reports.

Reading these is cheaper than reading the AIID because they are tied to the specific providers the team depends on; the signal is high. Pin the status-page and system-card URLs for the providers in the team's portfolio (mod-105); make the reading of new system cards a specific manager-level task (mod-109 operating cadence).

### Corpus 5 — adjacent — MITRE ATLAS and the OWASP LLM Top 10

Not incident corpora in the same sense, but part of the manager's reading map:

- **MITRE ATLAS** (`atlas.mitre.org`) — the AI-adapted ATT&CK; a taxonomy of adversarial tactics, techniques, and procedures against AI systems, with case studies. The ATLAS case studies are a corpus of attack-shaped incidents and should be read alongside the AIID.
- **OWASP Top 10 for LLM Applications** — the industry's practitioner-level taxonomy of LLM security risks. Each entry includes known incident shapes and references. Referenced in mod-106 chapter 02.

Reading these is a manager-level habit; the ICs on the AI Risk ladder read them at depth, and the manager reads them enough to have the vocabulary and the cross-references.

## Core concept 2 — a quarterly reading ritual

The reading is a ritual with specific beats. The ritual takes roughly two hours per quarter.

### Beat 1 — the AIID quarterly scan (45 minutes)

Scan the new AIID entries since the last reading. For each entry that is relevant to the team's surface (by industry sector, by failure mode, by harm category), capture:

- The incident shape (which chapter 02 failure mode does it map to, or is it a shape the catalogue should add?).
- The harm category and severity.
- The documented mitigation (where present).
- The regulatory response (where present).

Output: a short bullet list. "AIID-<N>: healthcare chatbot gave unsafe medical advice; maps to chapter 02 mode 4 (hallucination with harm); peer product deprecated the feature; GDPR DPIA re-run following inquiry." Not more than 15–25 bullets per quarter.

### Beat 2 — the OECD.AI Monitor scan (30 minutes)

Read the OECD.AI Incidents Monitor new-this-quarter entries, filtered for jurisdictions relevant to the team's deployment (EU if deployed to EU; the applicable US state / federal regulators; UK / APAC as deployed). Capture:

- Which regulators responded.
- What the response was (inquiry, fine, guidance, implementing act).
- The policy-framework reading (which article / section of which regulation).

Output: a bullet list, cross-referenced to counsel's framework reading (chapter 07). Share with counsel at the next regular sync.

### Beat 3 — provider system-card and status-page refresh (30 minutes)

For each provider in the team's portfolio (mod-105), check:

- Any new system cards since the last reading.
- Any material status-page incidents since the last reading.
- Any new safety / policy posts.

Output: a bullet list, with implications. "Provider X released Y; moved snapshot Z to production default on <date>; capability-tier change from <RSP tier> to <RSP tier> per mod-106 chapter 06; selection memo (mod-105 chapter 03) due for refresh by <date>."

### Beat 4 — MIT AI Risk Repository and ATLAS scan (15 minutes)

Scan for new entries, new taxonomies, new case studies. Capture any that:

- Represent a risk class not yet in the chapter 02 catalogue.
- Document an attack pattern the red-team brief (mod-106 chapter 04) should add.
- Suggest a fixture the eval registry (chapter 06) should add.

Output: a bullet list, cross-referenced to the specific artefact that should be updated.

### Beat 5 — synthesis (15–30 minutes)

Produce the quarterly incident-corpora read-out: three pages at most, structured as:

- **Summary** — the two or three patterns the quarter surfaced across the corpora.
- **Specific actions** — the artefacts that should be updated (catalogue, runbook pages, red-team brief, fixtures, selection memo).
- **Owners and targets** — who owns each action and when it is due.

Share the read-out at the operational review (mod-109); file the read-out in the team's governance repository; link from the operating-cadence page.

### Beat 6 — the yearly deeper read (annual, separate beat)

Once a year, dedicate a half-day to:

- Re-reading the chapter 02 failure-mode catalogue against the year's accumulated corpora reading.
- Updating the catalogue with any new failure modes that have surfaced.
- Reviewing the tabletop-exercise rotation (chapter 03) against the new catalogue; each failure mode should have been tabletopped in the last two years.
- Reading the summary / annual-report publications of the AIID, OECD.AI, and provider safety teams.

The yearly deeper read is the beat at which the catalogue itself updates. Quarterly reads add bullets; yearly reads restructure.

## Core concept 3 — what the reading feeds

The reading is only valuable if it feeds specific artefacts. Five feeds.

### Feed 1 — the failure-mode catalogue (chapter 02)

New failure modes enter the catalogue when corpora reading shows a shape the catalogue did not name. The yearly deeper read is the beat; the quarterly reads flag candidates.

### Feed 2 — the runbook library (chapter 03)

Runbook pages update when corpora reading shows a mitigation, an evidence-preservation practice, or an escalation pattern the team's runbook did not have. "The communications-template section of our runbook for prompt-injection incidents does not match the OECD.AI pattern of jurisdictions X and Y; update for the EU deployment" is a specific feed.

### Feed 3 — the red-team brief (mod-106 chapter 04)

Red-team briefs update when corpora reading surfaces attack classes the brief did not enumerate. The manager, when commissioning the next red-team engagement, references the quarterly read-out as part of the brief's "risk model" section (mod-106 chapter 04).

### Feed 4 — the tabletop rotation (chapter 03)

Tabletop scenarios are drawn from the catalogue. New catalogue entries (yearly) refresh the rotation. Specific corpora incidents sometimes become specific tabletop scenarios: "Our next tabletop is the healthcare-chatbot case from AIID-<N>; run it against our runbook; identify gaps."

### Feed 5 — the regulator-facing posture (chapter 07)

The OECD.AI Monitor reading informs counsel's framework reading and the engineering summary's language. "Jurisdictions X and Y treated incidents of this shape as Article 73-adjacent in Q3 2026" is a sentence counsel wants to know about before an incident of that shape lands on the team's own surface.

The five feeds are the structural reason the reading is a habit. Without them, the reading is a document; with them, the reading is a lever on the team's incident posture.

## Core concept 4 — reading discipline — what to capture and what to skip

An AI engineering manager who tries to read every AIID entry will burn out and stop reading. The discipline is to read *at altitude*. Three disciplines.

### Discipline 1 — filter by relevance

Not every corpus entry is relevant to the team's surface. Specific filters:

- **Industry sector** — healthcare incidents are directly relevant to a healthcare product; peripherally relevant to a developer-tool product.
- **Surface shape** — RAG-agent incidents are directly relevant to a RAG-agent product; LLM-API-only incidents are peripherally relevant.
- **Harm category** — regulated-category outputs are always relevant to a candidate high-risk system; low-harm miscommunications are often not.
- **Attack class** — jailbreak and injection patterns are universally relevant; specific attacks tied to specific vendor stacks are sometimes not.

A good quarterly read ends with 15–25 extracted entries, not 150. The filter is the discipline.

### Discipline 2 — read the sources, not just the catalogue entry

An AIID catalogue entry is a summary; the primary sources (news reports, provider blog posts, academic papers) are the evidence. For entries that matter to the team's artefacts, read at least one primary source. The summary is an index; the reading is deeper.

### Discipline 3 — capture the action, not just the fact

The bullet list the reading produces should name an action wherever applicable: "update runbook page X," "add to catalogue," "flag for red-team brief Q4," "ask counsel about the Y jurisdictional response." A bullet with no action is a bullet that will not lead to a system change.

## Core concept 5 — the operating-cadence pin

The quarterly reading is a beat on the operating cadence (mod-109). The manager pins it.

- **Calendar discipline** — the reading is on the quarterly plan, with a specific calendar time, treated as a recurring commitment at the same altitude as a release-triad review (mod-106 chapter 05) or a portfolio review (mod-105 chapter 07).
- **Output discipline** — the read-out is filed in the governance repository; indexed by quarter; linked from the mod-109 operating-cadence page.
- **Review discipline** — the read-out is reviewed at the next operational-review; at least one action per read-out is tracked to completion before the next quarter's reading.
- **Delegation discipline** — the reading itself can be delegated (to a senior IC on the AI Risk ladder, or to a rotation across the team); the review and sign-off remain with the manager.

The cadence is the discipline that prevents the read-once-a-year failure mode.

## Core concept 6 — a short worked read-out

```markdown
# Incident-corpora read-out — Q4 2026
Manager: @me · Date: 2026-11-10 · Next review: 2027-02-05 (Q1 2027)

## Summary
Three patterns surfaced this quarter:
1. **Indirect injection via tool-returned content** (not only retrieval) — AIID entries X, Y, Z and ATLAS case study ATC-<N> all document agents following instructions returned by third-party APIs. Our chapter 02 catalogue covers injection-via-retrieval; the tool-returned shape is adjacent and under-documented in our runbooks.
2. **Moderation false-positive storms triggered by viral user templates** — OECD.AI entries in jurisdiction A and news reports (pinned) documented incidents where a viral template triggered mass refusals on a mental-health-crisis surface. Harm pattern was clear; policy response was a national-level advisory in jurisdiction A.
3. **Model-provider silent snapshot migration** — provider X's status page and system card showed a snapshot change on <date> that produced a regression on multilingual surfaces at peer organisations. Our selection memo (mod-105 chapter 03) is due for a refresh; see action below.

## Specific actions
- **Chapter 02 catalogue update** — add "indirect injection via tool-returned content" as a sub-mode of chapter 02 mode 3; distinct enough to warrant its own runbook page.
  - Owner: @me. Target: 2027-01-15.
- **Runbook library update** — author runbook-indirect-injection-tool-returned.md modelled on the existing indirect-injection page.
  - Owner: @hannah (on-call). Target: 2027-01-30.
- **Red-team brief update** — Q1 2027 engagement scope expanded to cover tool-returned-content attack class.
  - Owner: @me. Target: 2026-12-10 (brief sign-off); engagement 2027-Q1.
- **Mental-health-crisis runbook pass** — the moderation-false-positive-storm runbook page updated with the viral-template signature and the specific jurisdictional advisory language.
  - Owner: @alice (applied-ai) + Comms Lead. Target: 2026-12-05.
- **Portfolio refresh** — provider X selection memo refresh scheduled; parity evals against snapshot change included.
  - Owner: @me. Target: 2026-12-20.
- **Tabletop** — Q1 2027 tabletop is the healthcare-chatbot case from AIID-<N> adapted to our Healthwise Chat surface.
  - Owner: @me. Target: 2027-02-15.

## Entries extracted this quarter (filtered)
- AIID-<N>: ...
- AIID-<N+1>: ...
- OECD.AI monitor entry <id>: ...
- Provider X system card <date>: ...
- ATLAS ATC-<N>: ...

## Primary sources read (selected)
- News article at <url> — <summary>.
- Provider safety-team post at <url> — <summary>.
- Academic paper <citation> — <summary>.
```

The read-out is three pages; the quarter's bullet list is in an appendix; the action list is the point.

## Core concept 7 — anti-patterns

Four failure modes of the reading practice.

- **The compliance-check pattern.** The reading is a once-a-year box-ticking exercise. Prevented by the quarterly cadence and the operating-review discipline.
- **The read-but-no-action pattern.** The reading happens; nothing changes. Prevented by the action discipline — every bullet has an owner and a target.
- **The read-but-no-filter pattern.** The reading tries to cover every entry; burnout; stop. Prevented by the relevance filters and the altitude discipline (15–25 extracted entries per quarter, not 150).
- **The read-only-after-an-incident pattern.** The team reads the corpora when triaging its own postmortem; the reading was absent before. Prevented by the operating-cadence pin — the reading is scheduled, not reactive.

## What good looks like

- Quarterly incident-corpora read-out is filed; indexed by quarter; linked from the operating-cadence page.
- The yearly deeper read updates the chapter 02 failure-mode catalogue and the tabletop rotation.
- Specific actions from the quarterly read-out are tracked to completion before the next quarter.
- The red-team brief (mod-106 chapter 04) references the quarterly read-out.
- The engineering summary (chapter 07) can cite comparable incidents from the corpora when counsel asks about precedent.

## What bad looks like

- The team has never produced an incident-corpora read-out.
- The reading is a year-end compliance check; the document exists but has no actions.
- The chapter 02 failure-mode catalogue has not changed in two years despite the industry's documented new failure modes.
- The red-team brief is authored against last year's threat model and has no precedent references.
- When a regulator asks about comparable precedent, the team's answer is "we don't know of any."

## Summary

- Four primary corpora: the AI Incident Database (AIID), the OECD.AI Incidents Monitor, the MIT AI Risk Repository, and the provider / lab public incident notes; plus adjacent frames (MITRE ATLAS, OWASP LLM Top 10).
- The quarterly reading ritual has six beats: AIID scan, OECD.AI scan, provider refresh, MIT / ATLAS scan, synthesis, yearly deeper read. Roughly two hours per quarter; delegated where appropriate; the manager reviews and signs.
- The reading feeds five artefacts: the failure-mode catalogue, the runbook library, the red-team brief, the tabletop rotation, and the regulator-facing posture. Without the feeds, the reading is a document; with them, it is a lever.
- Reading discipline: filter by relevance; read the primary sources where they matter; capture actions, not facts.
- The cadence is pinned on the operating cadence (mod-109), with calendar, output, review, and delegation disciplines.
- Four anti-patterns — compliance-check, read-but-no-action, no-filter, read-only-after-incident — each have a specific preventive discipline.

This closes the module. The eight chapters have walked the manager-owned incident posture end-to-end: the altitude and the four artefacts (chapter 01), the failure-mode catalogue (chapter 02), the on-call and the runbook library (chapter 03), the incident-command system (chapter 04), the blameless postmortem discipline (chapter 05), the incident-to-fixture loop (chapter 06), the regulator-facing partnership with counsel (chapter 07), and the public-corpora reading habit (this chapter). The exercises in the module build, in turn, the runbook for one product, the postmortem template, the incident-to-fixture loop mechanism, and the regulator-facing engineering summary.

Mod-108 picks up performance management against the backdrop of this cadence — how an AI engineering manager grows ICs who are good at the on-call, good at postmortems, and good at the loop.
