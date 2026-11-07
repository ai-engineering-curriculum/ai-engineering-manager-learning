# Resources for mod-106: Safety, Guardrails, and the Model + Data + Safety Release Triad

Curated references for this module. Each entry is a primary or widely-cited source. Standards, frameworks, system cards, and vendor documentation in this space move quickly — always check publication date and current version against the content as it existed when this file was written.

## Risk taxonomies and governance frames (chapters 01, 02, 04, 08)

Primary sources the guardrails spec, red-team brief, and compliance-mapping memo cite.

- OWASP — *Top 10 for Large Language Model Applications* (GenAI Security Project). [Project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [Current list and PDF releases](https://genai.owasp.org/)
- OWASP — *AI Security and Privacy Guide* (project page). [Page](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- NIST — *AI Risk Management Framework 1.0* (AI 100-1). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — *Generative AI Profile* (AI 600-1). [Page](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile) · [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
- NIST — *AI RMF Playbook*. [Page](https://airc.nist.gov/AI_RMF_Knowledge_Base/Playbook)
- MITRE — *ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)*. [Site](https://atlas.mitre.org/)
- MITRE — *ATT&CK* (underlying tactics catalogue cross-referenced by ATLAS). [Site](https://attack.mitre.org/)
- ISO/IEC — *ISO/IEC 42001:2023 — AI Management Systems*. [Standard page](https://www.iso.org/standard/81230.html)
- ISO/IEC — *ISO/IEC 23894:2023 — AI Risk Management*. [Standard page](https://www.iso.org/standard/77304.html)
- ISO/IEC — *ISO/IEC 27001 — Information Security Management Systems*. [Standard page](https://www.iso.org/standard/27001)

## Guardrails orchestration frameworks and libraries (chapter 03)

- NVIDIA — *NeMo Guardrails*. [GitHub](https://github.com/NVIDIA/NeMo-Guardrails) · [Docs](https://docs.nvidia.com/nemo/guardrails/latest/index.html)
- Guardrails AI — [Site](https://www.guardrailsai.com/) · [Docs](https://www.guardrailsai.com/docs) · [Validator hub](https://hub.guardrailsai.com/)
- LangChain — *LangGraph* (interrupts / HITL patterns). [Docs](https://langchain-ai.github.io/langgraph/)
- Vercel — *AI SDK*. [Docs](https://sdk.vercel.ai/docs)

## Hosted content-safety classifiers (chapter 03, EU AI Act Article 15.B)

- OpenAI — *Moderation API*. [Docs](https://platform.openai.com/docs/guides/moderation) · *Usage policies*. [Page](https://openai.com/policies/usage-policies/)
- Microsoft Azure — *AI Content Safety*. [Service page](https://azure.microsoft.com/en-us/products/ai-services/ai-content-safety) · *Documentation*. [Docs](https://learn.microsoft.com/azure/ai-services/content-safety/) · *Prompt Shields*. [Docs](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection)
- Google Cloud — *Vertex AI responsible AI / safety filters*. [Docs](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/responsible-ai) · *Responsible AI Toolkit*. [Page](https://ai.google.dev/responsible)
- Google Jigsaw — *Perspective API*. [Site](https://perspectiveapi.com/)
- Amazon — *Bedrock Guardrails*. [Docs](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)

## Open-weights safety classifiers (chapter 03, agent-authorisation patterns in chapter 07)

- Meta — *Llama Guard* model family. [Current release page](https://www.llama.com/) · [Hugging Face collection](https://huggingface.co/meta-llama) · [Original paper (Llama Guard)](https://arxiv.org/abs/2312.06674)
- Meta — *Prompt Guard* (prompt-injection and jailbreak classifier). [Page](https://www.llama.com/) · [Hugging Face](https://huggingface.co/meta-llama/Prompt-Guard-86M)
- Meta — *Responsible Use Guide* for Llama. [Site](https://www.llama.com/responsible-use-guide/)
- Google — *ShieldGemma* (and successors). [Hugging Face](https://huggingface.co/google/shieldgemma-2b) · [Technical report](https://arxiv.org/abs/2407.21772)
- IBM — *Granite Guardian* model family. [Hugging Face](https://huggingface.co/collections/ibm-granite/granite-guardian-models-6759f130c1a6cab8f0e6a84a)

## Structured-output validators and constrained decoding (chapter 03)

- Pydantic — [Docs](https://docs.pydantic.dev/)
- Zod — [Site](https://zod.dev/)
- JSON Schema — [Spec](https://json-schema.org/)
- Instructor — [GitHub](https://github.com/jxnl/instructor) · [Docs](https://python.useinstructor.com/)
- Outlines — [GitHub](https://github.com/dottxt-ai/outlines) · [Docs](https://dottxt-ai.github.io/outlines/)
- Guidance — [GitHub](https://github.com/guidance-ai/guidance)
- XGrammar — [GitHub](https://github.com/mlc-ai/xgrammar) · [Paper](https://arxiv.org/abs/2411.15100)

## Prompt-injection and LLM-security tools and research (chapters 02, 04, 07)

- Lakera — *Lakera Guard*. [Site](https://www.lakera.ai/) · *Gandalf* (prompt-injection training game). [Site](https://gandalf.lakera.ai/)
- Protect AI — [Site](https://protectai.com/) · *Rebuff* (prompt-injection detector, open source). [GitHub](https://github.com/protectai/rebuff) · *LLM Guard*. [GitHub](https://github.com/protectai/llm-guard)
- Greshake, Abdelnabi, Mishra, Endres, Holz, Fritz — *Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection*. [arXiv:2302.12173](https://arxiv.org/abs/2302.12173)
- Perez, Ribeiro — *Ignore Previous Prompt: Attack Techniques For Language Models*. [arXiv:2211.09527](https://arxiv.org/abs/2211.09527)
- Liu, Jia, Chen, Zhu, Zou, Li, Jiang, Xie, Li, Li — *Prompt Injection Attack against LLM-integrated Applications*. [arXiv:2306.05499](https://arxiv.org/abs/2306.05499)
- Wallace, Feinberg, Hada, Xiong, Cheng, Chi, Papernot — *The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions*. [arXiv:2404.13208](https://arxiv.org/abs/2404.13208)
- Zou, Wang, Kolter, Fredrikson — *Universal and Transferable Adversarial Attacks on Aligned Language Models*. [arXiv:2307.15043](https://arxiv.org/abs/2307.15043)

## Red-team tooling and harnesses (chapter 04)

- NVIDIA — *garak* (LLM vulnerability scanner). [GitHub](https://github.com/NVIDIA/garak)
- Microsoft — *PyRIT* (Python Risk Identification Tool for generative AI). [GitHub](https://github.com/Azure/PyRIT)
- Meta — *CyberSecEval* benchmarks. [GitHub](https://github.com/meta-llama/PurpleLlama)
- Anthropic — *Responsible Disclosure Policy* and bug-bounty page. [Page](https://www.anthropic.com/supported-vulnerability-disclosure)
- OpenAI — *Security & privacy* (bug-bounty and coordinated disclosure). [Page](https://openai.com/security/)

## Lab-level governance frameworks (chapter 06)

Required reading at commissioning-manager depth. Each framework is versioned; pin the current URL and the date read.

- Anthropic — *Responsible Scaling Policy (RSP)*. [Page](https://www.anthropic.com/rsp) · *System cards* (per model release, linked from model docs)
- OpenAI — *Preparedness Framework*. [Page](https://openai.com/safety/preparedness/) · *System cards* (per model release, on the research page)
- Google DeepMind — *Frontier Safety Framework (FSF)*. [Page](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) · *Gemini technical reports* (per version)
- Meta — *Responsible Use Guide* for Llama. [Site](https://www.llama.com/responsible-use-guide/) · *Llama model cards* (per release)
- *Seoul Frontier AI Safety Commitments* (May 2024). [UK government page](https://www.gov.uk/government/publications/frontier-ai-safety-commitments-ai-seoul-summit-2024)
- UK AI Security Institute (AISI). [Site](https://www.aisi.gov.uk/)
- US AI Safety Institute (USAISI). [Site](https://www.nist.gov/aisi)
- OECD — *AI Policy Observatory*. [Site](https://oecd.ai/)

## Model Cards and data documentation (chapters 02, 06, 08)

- Mitchell, Wu, Zaldivar, Barnes, Vasserman, Hutchinson, Spitzer, Raji, Gebru — *Model Cards for Model Reporting*. [arXiv:1810.03993](https://arxiv.org/abs/1810.03993)
- Gebru, Morgenstern, Vecchione, Vaughan, Wallach, Daumé III, Crawford — *Datasheets for Datasets*. [arXiv:1803.09010](https://arxiv.org/abs/1803.09010)
- Pushkarna, Zaldivar, Kjartansson — *Data Cards: Purposeful and Transparent Dataset Documentation*. [arXiv:2204.01075](https://arxiv.org/abs/2204.01075)

## Agent / tool-use authorization standards (chapter 07)

- Anthropic — *Model Context Protocol (MCP)*. [Specification site](https://modelcontextprotocol.io/) · [GitHub](https://github.com/modelcontextprotocol)
- IETF — *OAuth 2.0 Authorization Framework* (RFC 6749). [RFC](https://www.rfc-editor.org/rfc/rfc6749) · *OAuth 2.1* (draft). [Current I-D](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/)
- IETF — *OAuth 2.0 Token Exchange* (RFC 8693). [RFC](https://www.rfc-editor.org/rfc/rfc8693)
- OpenID Foundation — *OpenID Connect*. [Spec page](https://openid.net/specs/openid-connect-core-1_0.html)
- SPIFFE / SPIRE — *Secure Production Identity Framework for Everyone*. [Site](https://spiffe.io/)
- OWASP — *API Security Top 10*. [Project page](https://owasp.org/www-project-api-security/)
- OWASP — *Application Security Verification Standard (ASVS)*. [Project page](https://owasp.org/www-project-application-security-verification-standard/)

## EU AI Act and related regulation (chapter 08)

- EU AI Act — *Regulation (EU) 2024/1689*. [Consolidated text (EUR-Lex)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 9, 10, 13, 14, 15, 50, 52, 53–55.
- European Commission — *AI Act dashboard / guidance*. [Page](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)
- EU AI Office — [Page](https://digital-strategy.ec.europa.eu/en/policies/ai-office)
- EU — *GDPR* (Regulation 2016/679). [EUR-Lex](https://eur-lex.europa.eu/eli/reg/2016/679/oj)
- EDPB — *Guidelines on the processing of personal data by AI models* (where applicable). [Page](https://edpb.europa.eu/)
- EU Commission — *EU-US Data Privacy Framework*. [Page](https://commission.europa.eu/law/law-topic/data-protection/international-dimension-data-protection/eu-us-data-transfers_en)
- US EEOC — *Guidance on AI in employment decisions* (if applicable). [Page](https://www.eeoc.gov/)
- US NIST — *AI RMF* cross-walked to EU AI Act obligations. [Page](https://airc.nist.gov/)

## Transparency, content provenance, and watermarking (chapter 08, Article 50)

- Coalition for Content Provenance and Authenticity (C2PA) — *Specification*. [Site](https://c2pa.org/) · [Spec](https://c2pa.org/specifications/specifications/)
- Content Authenticity Initiative — [Site](https://contentauthenticity.org/)
- Google — *SynthID*. [Page](https://deepmind.google/technologies/synthid/)
- Kirchenbauer, Geiping, Wen, Katz, Miers, Goldstein — *A Watermark for Large Language Models*. [arXiv:2301.10226](https://arxiv.org/abs/2301.10226)
- Dathathri, See, Ghaisas, Huang, McAdam, Welbl, Bachani, Kaskasoli, Stanforth, Matejovicova, Hayes — *Scalable watermarking for identifying large language model outputs* (Nature). [Page](https://www.nature.com/articles/s41586-024-08025-4)

## Human-in-the-loop UX and automation-bias literature (chapters 02, 08 Article 14)

- Parasuraman, Riley — *Humans and Automation: Use, Misuse, Disuse, Abuse* (Human Factors, 1997). Classic reference on automation bias.
- Bansal, Nushi, Kamar, Lasecki, Weld, Horvitz — *Beyond Accuracy: The Role of Mental Models in Human-AI Team Performance*. [Paper](https://www.jennwv.com/papers/beyond-accuracy.pdf)
- Google — *People + AI Guidebook*. [Site](https://pair.withgoogle.com/guidebook)
- Microsoft — *Guidelines for Human-AI Interaction*. [Page](https://www.microsoft.com/en-us/research/project/guidelines-for-human-ai-interaction/)

## Status pages, incident monitoring, and on-call resources (chapters 05 pin, mod-107 bridge)

For the incident playbook and on-call runbook.

- OpenAI — [status.openai.com](https://status.openai.com/)
- Anthropic — [status.anthropic.com](https://status.anthropic.com/)
- Google Cloud — [status.cloud.google.com](https://status.cloud.google.com/)
- AWS — [health.aws.amazon.com](https://health.aws.amazon.com/)
- Azure — [status.azure.com](https://status.azure.com/)

## Benchmarks and eval datasets with safety content (chapters 04, 05)

Pin alongside mod-104 chapter 02's commissioning brief.

- TrustLLM / TrustGen — benchmarks across trustworthiness dimensions. [Site](https://trustllmbenchmark.github.io/TrustLLM-Website/)
- HELM Safety (Stanford CRFM). [Project page](https://crfm.stanford.edu/helm/)
- SafeBench / SafetyBench. [Page](https://llmbench.ai/safety)
- JailbreakBench. [Site](https://jailbreakbench.github.io/)
- HarmBench. [Site](https://www.harmbench.org/)
- RealToxicityPrompts. [Page](https://toxicdegeneration.allenai.org/)
- AdvBench / GCG prompts. [Paper and code](https://github.com/llm-attacks/llm-attacks)

## Operating and SRE discipline (chapters 05, 07, bridge to mod-107)

- Google — *Site Reliability Engineering*. [Free online](https://sre.google/sre-book/table-of-contents/)
- Google — *The Site Reliability Workbook*. [Free online](https://sre.google/workbook/table-of-contents/)
- US CISA — *Secure by Design — Guidance on AI Software*. [Page](https://www.cisa.gov/ai)
- US NSA / CISA — *Deploying AI Systems Securely* (joint guidance). [PDF](https://media.defense.gov/2024/apr/15/2003439257/-1/-1/0/csi-deploying-ai-systems-securely.pdf)

## Industry case studies and incident databases (chapters 01, 04, bridge to mod-107)

- AI Incident Database (Partnership on AI / Responsible AI Collaborative). [Site](https://incidentdatabase.ai/)
- OECD — *AI Incidents Monitor*. [Site](https://oecd.ai/en/incidents)

## Where this module reads out to other modules

- `mod-104-eval-driven-sdlc-and-release-gates` — safety-eval cases live in the eval registry; the model leg of the triad reads mod-104's eval evidence; online-eval cadence catches post-launch safety regressions.
- `mod-105-vendor-and-model-portfolio-selection` — the data leg of the triad reads the DPA posture from mod-105 chapter 05; fallback plan from mod-105 chapter 04 is the resilience side of Article 15.B; the quarterly portfolio review consumes RSP / Preparedness / FSF reading.
- `mod-107-ai-incident-response-and-postmortems` — the incident-playbook pin on the triad is mod-107's entry; kill-switches, audit logs, and runbook sections are mod-107's day-one playbooks.
- `mod-108-performance-management-and-growth-for-ai-engineers` — labelling practice and growth paths that partner on this module's artefacts span the AI Risk ladder and the Applied AI ladder.
- `mod-109-team-shape-span-and-ai-operating-cadence` — the red-team cadence, release-triad review clock, and quarterly authorisation-boundary review are beats on the operating cadence.
- `mod-110-budget-finops-and-build-vs-buy` — guardrail-stack spend and red-team engagement budget are quarterly line items.
- `mod-111-cross-functional-partnership` — counsel partnership, procurement partnership on DPAs, security partnership on authorisation.
- `mod-112-executive-communication-and-stakeholder-reporting` — the quarterly safety update, release-triad memo, and compliance-mapping memo are inputs to mod-112's executive artefacts.
