# Resources for mod-107: AI-Specific Incident Response and Blameless Postmortems

Curated references for this module. Each entry is a primary or widely-cited source. Specific regulatory texts, system cards, and incident-catalogue entries move; always check publication date and current version against the content as it existed when this file was written (2026-11).

## Site Reliability Engineering foundations (chapters 01, 03, 04)

The on-call and incident-response practice this module adapts.

- Google — *Site Reliability Engineering* (the SRE Book). Free online: [sre.google/sre-book/](https://sre.google/sre-book/table-of-contents/) — especially chapters on *Being On-Call*, *Effective Troubleshooting*, *Emergency Response*, *Managing Incidents*, and *Postmortem Culture*.
- Google — *The Site Reliability Workbook*. Free online: [sre.google/workbook/](https://sre.google/workbook/table-of-contents/) — the companion volume with chapter adaptations on on-call practice, incident response, postmortems, and SLOs.
- Google — *Building Secure and Reliable Systems*. Free online: [sre.google/books/building-secure-reliable-systems/](https://sre.google/books/building-secure-reliable-systems/) — the cross-section with security operations and incident response.
- PagerDuty — *Incident Response Documentation* (public, open-source). [response.pagerduty.com](https://response.pagerduty.com/) — ICS-adapted incident-response playbooks, severity schemas, and post-incident templates.
- Atlassian — *Incident Management Handbook*. [atlassian.com/incident-management](https://www.atlassian.com/incident-management) — vendor-neutral practitioner reference.

## Blameless postmortems and just culture (chapter 05)

Primary sources for the blameless practice.

- Allspaw, John — *Blameless Post-Mortems and a Just Culture* (Etsy / Code as Craft, 2012). [codeascraft.com/2012/05/22/blameless-postmortems/](https://www.etsy.com/codeascraft/blameless-postmortems/) — the canonical articulation.
- Dekker, Sidney — *The Field Guide to Understanding 'Human Error'* (3rd edition). CRC Press. The deeper reading on hindsight bias, counterfactual reasoning, and the "new view" of error.
- Dekker, Sidney — *Just Culture: Restoring Trust and Accountability in Your Organization* (3rd edition). CRC Press.
- Woods, Dekker, Cook, Johannesen, Sarter — *Behind Human Error*.
- Google — *Postmortem Culture* chapter of the SRE Book. [sre.google/sre-book/postmortem-culture/](https://sre.google/sre-book/postmortem-culture/)
- Google — *Postmortem example templates* in the SRE Workbook. [sre.google/workbook/postmortem-culture/](https://sre.google/workbook/postmortem-culture/)
- Allspaw, John; Erik Hollnagel et al. — *Resilience Engineering* literature (Hollnagel, Woods, Leveson).

## AI-specific failure modes and attack taxonomies (chapter 02)

- OWASP — *Top 10 for Large Language Model Applications* (GenAI Security Project). [owasp.org project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [genai.owasp.org](https://genai.owasp.org/)
- OWASP — *AI Security and Privacy Guide*. [Project page](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- MITRE — *ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)*. [atlas.mitre.org](https://atlas.mitre.org/)
- MITRE — *ATT&CK* (underlying tactics catalogue cross-referenced by ATLAS). [attack.mitre.org](https://attack.mitre.org/)
- NIST — *Generative AI Profile* (AI 600-1). [Page](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile) · [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
- NIST — *AI Risk Management Framework 1.0* (AI 100-1). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — *Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations* (AI 100-2 E2023). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2023.pdf)
- US NSA / CISA (joint) — *Deploying AI Systems Securely*. [PDF](https://media.defense.gov/2024/apr/15/2003439257/-1/-1/0/csi-deploying-ai-systems-securely.pdf)
- US CISA — *Secure by Design — Guidance on AI Software*. [Page](https://www.cisa.gov/ai)

## Prompt injection and jailbreak research (chapter 02)

- Greshake, Abdelnabi, Mishra, Endres, Holz, Fritz — *Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection*. [arXiv:2302.12173](https://arxiv.org/abs/2302.12173)
- Perez, Ribeiro — *Ignore Previous Prompt: Attack Techniques For Language Models*. [arXiv:2211.09527](https://arxiv.org/abs/2211.09527)
- Liu et al. — *Prompt Injection Attack against LLM-integrated Applications*. [arXiv:2306.05499](https://arxiv.org/abs/2306.05499)
- Wallace et al. — *The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions*. [arXiv:2404.13208](https://arxiv.org/abs/2404.13208)
- Zou, Wang, Kolter, Fredrikson — *Universal and Transferable Adversarial Attacks on Aligned Language Models*. [arXiv:2307.15043](https://arxiv.org/abs/2307.15043)

## Public AI incident corpora (chapter 08)

- AI Incident Database (AIID) — Responsible AI Collaborative. [incidentdatabase.ai](https://incidentdatabase.ai/) — searchable catalogue of publicly-reported AI incidents with taxonomic classification.
- OECD.AI — *AI Incidents and Hazards Monitor*. [oecd.ai/en/incidents](https://oecd.ai/en/incidents) — policy-focused catalogue with regulator-response cross-referencing.
- OECD.AI Policy Observatory. [oecd.ai](https://oecd.ai/)
- MIT AI Risk Repository. [airisk.mit.edu](https://airisk.mit.edu/) — structured taxonomy of AI risks from the literature, with citations.

## Provider status pages and system cards (chapter 08)

Reading cadence targets for the portfolio.

- Anthropic — [status.anthropic.com](https://status.anthropic.com/) · *Responsible Scaling Policy (RSP)*. [anthropic.com/rsp](https://www.anthropic.com/rsp) · *Model cards and system cards* linked from model docs.
- OpenAI — [status.openai.com](https://status.openai.com/) · *Preparedness Framework*. [openai.com/safety/preparedness](https://openai.com/safety/preparedness/) · *System cards* on the research page.
- Google DeepMind — *Frontier Safety Framework (FSF)*. [Page](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) · *Gemini technical reports* per version.
- Google Cloud — [status.cloud.google.com](https://status.cloud.google.com/)
- AWS — [health.aws.amazon.com](https://health.aws.amazon.com/)
- Microsoft Azure — [status.azure.com](https://status.azure.com/)
- Meta — *Responsible Use Guide* for Llama. [llama.com/responsible-use-guide](https://www.llama.com/responsible-use-guide/)

## EU AI Act and regulator-facing obligations (chapter 07)

- EU AI Act — *Regulation (EU) 2024/1689*. [Consolidated text (EUR-Lex)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — especially Articles 3(49) (definitions), 50 (transparency), 70 (national competent authorities), 72 (post-market monitoring), 73 (serious-incident reporting), Annex III (high-risk AI systems).
- European Commission — *AI Act dashboard / guidance*. [digital-strategy.ec.europa.eu](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- EU AI Office. [digital-strategy.ec.europa.eu/en/policies/ai-office](https://digital-strategy.ec.europa.eu/en/policies/ai-office)
- EU — *GDPR* (Regulation 2016/679). [EUR-Lex](https://eur-lex.europa.eu/eli/reg/2016/679/oj) — especially Articles 32 (security of processing), 33 (notification of a personal data breach to the supervisory authority), 34 (communication to the data subject).
- EDPB — *European Data Protection Board* guidelines. [edpb.europa.eu](https://edpb.europa.eu/)
- EU — *EU-US Data Privacy Framework*. [Page](https://commission.europa.eu/law/law-topic/data-protection/international-dimension-data-protection/eu-us-data-transfers_en)
- UK ICO — *Guidance on AI and data protection*. [ico.org.uk](https://ico.org.uk/)
- UK AI Security Institute (AISI). [aisi.gov.uk](https://www.aisi.gov.uk/)
- US AI Safety Institute (USAISI). [nist.gov/aisi](https://www.nist.gov/aisi)
- US NIST — *AI RMF* cross-walks. [airc.nist.gov](https://airc.nist.gov/)

<!-- needs-research: a current list of US state-level AI regulation (employment-decision, consumer-disclosure, sector-specific) relevant to the module audience; this changes rapidly and should be verified with counsel at reading time rather than pinned here. -->

## Security operations and DFIR partnership (chapter 07)

- SANS Institute — *Incident Handler's Handbook*. [sans.org](https://www.sans.org/white-papers/33901/) — the IR practitioner standard reference for chain-of-custody and forensic handling.
- NIST — *Computer Security Incident Handling Guide* (SP 800-61 Rev. 2). [PDF](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- NIST — *Guide to Integrating Forensic Techniques into Incident Response* (SP 800-86). [PDF](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-86.pdf)
- FIRST (Forum of Incident Response and Security Teams). [first.org](https://www.first.org/)
- FIRST — *Computer Security Incident Response Team (CSIRT) Services Framework*. [first.org/standards/frameworks/csirts](https://www.first.org/standards/frameworks/csirts/)

## Red-team tooling referenced from chapter 02 / fixture broadening

- NVIDIA — *garak* (LLM vulnerability scanner). [GitHub](https://github.com/NVIDIA/garak)
- Microsoft — *PyRIT* (Python Risk Identification Tool for generative AI). [GitHub](https://github.com/Azure/PyRIT)
- Meta — *CyberSecEval* / Purple Llama. [GitHub](https://github.com/meta-llama/PurpleLlama)
- Protect AI — *Rebuff* (prompt-injection detector). [GitHub](https://github.com/protectai/rebuff) · *LLM Guard*. [GitHub](https://github.com/protectai/llm-guard)
- Lakera — *Lakera Guard*. [lakera.ai](https://www.lakera.ai/) · *Gandalf*. [gandalf.lakera.ai](https://gandalf.lakera.ai/)

## Observability tooling referenced from chapter 03

Vendor-neutral orientation; your Platform / SRE team picks the stack.

- OpenTelemetry — [opentelemetry.io](https://opentelemetry.io/) — the vendor-neutral observability standard.
- Grafana — [grafana.com](https://grafana.com/) · Prometheus — [prometheus.io](https://prometheus.io/)
- Datadog — [datadoghq.com](https://www.datadoghq.com/) · New Relic — [newrelic.com](https://newrelic.com/)
- LangSmith — [smith.langchain.com](https://smith.langchain.com/) · Langfuse — [langfuse.com](https://langfuse.com/) · Braintrust — [braintrust.dev](https://www.braintrust.dev/) · Arize — [arize.com](https://arize.com/) — LLM-native tracing / evaluation platforms referenced from the AI-shaped observability stack.

## Incident-management tooling referenced from chapters 03–04

- PagerDuty — [pagerduty.com](https://www.pagerduty.com/) · [response.pagerduty.com](https://response.pagerduty.com/)
- Opsgenie (Atlassian) — [atlassian.com/software/opsgenie](https://www.atlassian.com/software/opsgenie)
- FireHydrant — [firehydrant.com](https://firehydrant.com/)
- Incident.io — [incident.io](https://incident.io/)
- Rootly — [rootly.com](https://rootly.com/)
- Statuspage (Atlassian) — [atlassian.com/software/statuspage](https://www.atlassian.com/software/statuspage)

## Eval and fixture infrastructure referenced from chapter 06

Primary references in mod-104 chapter 04; carried here for the incident-to-fixture loop.

- OpenAI — *Evals*. [GitHub](https://github.com/openai/evals)
- UK AI Security Institute — *Inspect*. [GitHub](https://github.com/UKGovernmentBEIS/inspect_ai)
- promptfoo — [promptfoo.dev](https://www.promptfoo.dev/) · [GitHub](https://github.com/promptfoo/promptfoo)
- Ragas — [docs.ragas.io](https://docs.ragas.io/) · [GitHub](https://github.com/explodinggradients/ragas)
- DeepEval — [GitHub](https://github.com/confident-ai/deepeval)
- Hugging Face — *Datasets*. [huggingface.co/docs/datasets](https://huggingface.co/docs/datasets)

## Standards and governance frameworks (cross-cutting)

- ISO/IEC — *ISO/IEC 42001:2023 — AI Management Systems*. [Standard page](https://www.iso.org/standard/81230.html)
- ISO/IEC — *ISO/IEC 23894:2023 — AI Risk Management*. [Standard page](https://www.iso.org/standard/77304.html)
- ISO/IEC — *ISO/IEC 27001 — Information Security Management Systems*. [Standard page](https://www.iso.org/standard/27001)
- ISO/IEC — *ISO/IEC 27035 — Information Security Incident Management* (parts 1–3). [Standard page](https://www.iso.org/standard/78973.html)

## Tabletop and chaos / game-day practice (chapter 03)

- Netflix — *Chaos Engineering* principles. [principlesofchaos.org](https://principlesofchaos.org/)
- AWS — *Operational Readiness Review (ORR) / GameDay* guidance in the Well-Architected Framework. [aws.amazon.com/architecture/well-architected](https://aws.amazon.com/architecture/well-architected/)
- Google — *Disaster recovery testing / DiRT* chapters of the SRE Workbook. [sre.google/workbook/](https://sre.google/workbook/table-of-contents/)
- CISA — *Tabletop Exercise Packages*. [cisa.gov/resources-tools/resources/cisa-tabletop-exercise-packages](https://www.cisa.gov/resources-tools/resources/cisa-tabletop-exercise-packages)

## Where this module reads out to other modules

- `mod-104-eval-driven-sdlc-and-release-gates` — the eval registry where incident-derived fixtures land (chapter 06); the prompt / judge / dataset registry schema the fixture packet reads against; the release-gate architecture the fixture is wired into.
- `mod-105-vendor-and-model-portfolio-selection` — the fallback plan that powers the route-to-safer-model mitigation lever (chapters 03, 04); the selection memo refreshed on model-vendor regression (chapter 02 mode 5, chapter 08).
- `mod-106-safety-guardrails-and-the-release-triad` — the release-triad incident-playbook pin (mod-106 chapter 05) is this module's entry point; the red-team commissioning posture (mod-106 chapter 04) feeds the fixture broadening (chapter 06); the guardrails spec (mod-106 chapter 02) and the authorisation boundary (mod-106 chapter 07) are the artefacts the postmortem routinely changes.
- `mod-108-performance-management-and-growth-for-ai-engineers` — the blameless discipline is a cultural investment the manager protects on performance reviews; on-call competence is a growth-path dimension.
- `mod-109-team-shape-span-and-ai-operating-cadence` — the on-call rotation, the tabletop cadence, the postmortem-review cadence, the quarterly incident-corpus reading ritual are beats on the operating cadence.
- `mod-110-budget-finops-and-build-vs-buy` — the cost-blast failure-mode runbook and the AI-side observability investment are FinOps line items; the incident-tooling budget is sized here.
- `mod-111-cross-functional-partnership` — the counsel partnership (chapter 07), the security operations / DFIR partnership (chapters 04, 07), the customer-communications partnership (chapter 04).
- `mod-112-executive-communication-and-stakeholder-reporting` — the chapter-07 engineering summary is a feed into the executive-communication artefacts; the incident-corpora quarterly read-out (chapter 08) is a candidate for executive brief content.
