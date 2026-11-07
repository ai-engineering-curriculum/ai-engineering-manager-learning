# Resources for mod-105: Vendor and Model Portfolio Selection

Curated references for this module. Each entry is a primary or widely-cited source. Vendor product pages, pricing, DPAs, and model documents change frequently; always check publication date and current version against the content as it existed when this file was written.

## Vendor pricing, model, and capability documentation (chapters 02, 03, 07)

The inventory's row-level truth lives on the vendors' own docs pages. Pin these as subscriptions; read the release notes and model pages when a new generation lands.

### Frontier closed-weights

- OpenAI — *Pricing*. [Page](https://openai.com/api/pricing/) · *Model documentation*. [Docs](https://platform.openai.com/docs/models) · *Deprecation policy*. [Docs](https://platform.openai.com/docs/deprecations)
- Anthropic — *Pricing*. [Page](https://www.anthropic.com/pricing) · *Models overview*. [Docs](https://docs.anthropic.com/en/docs/about-claude/models) · *Model deprecations*. [Docs](https://docs.anthropic.com/en/docs/about-claude/model-deprecations)
- Google — *Vertex AI generative AI pricing*. [Page](https://cloud.google.com/vertex-ai/generative-ai/pricing) · *Gemini models*. [Docs](https://ai.google.dev/gemini-api/docs/models/gemini) · *Vertex AI model lifecycle*. [Docs](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/models)

### Frontier open-weights

- Meta — *Llama models*. [Page](https://www.llama.com/) · *Llama Community Licence* and current model-specific licences published per release.
- Mistral — *Models*. [Docs](https://docs.mistral.ai/getting-started/models/) · *Licences* per model (Apache 2.0 for several; Mistral Research Licence / commercial-licence for others — read the specific model page).
- Google — *Gemma*. [Page](https://ai.google.dev/gemma) · *Gemma Terms of Use*. [Docs](https://ai.google.dev/gemma/terms)
- DeepSeek — *Models and licences*. [Site](https://api-docs.deepseek.com/)
- Alibaba — *Qwen*. [Site](https://qwenlm.github.io/)

### Enterprise and specialist

- Cohere — *Models*. [Docs](https://docs.cohere.com/docs/models) · *Pricing*. [Page](https://cohere.com/pricing) · *Rerank*. [Docs](https://docs.cohere.com/docs/rerank-overview)
- AI21 — *Models*. [Docs](https://docs.ai21.com/)
- Voyage — *Embeddings and rerankers*. [Site](https://docs.voyageai.com/)

### Inference platforms and cloud offerings

- AWS Bedrock — *Service page*. [Page](https://aws.amazon.com/bedrock/) · *Models available*. [Docs](https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html) · *Pricing*. [Page](https://aws.amazon.com/bedrock/pricing/)
- Microsoft Azure — *AI Foundry* (formerly Azure AI Studio). [Page](https://azure.microsoft.com/en-us/products/ai-foundry) · *Azure OpenAI Service*. [Docs](https://learn.microsoft.com/azure/ai-services/openai/)
- Google Cloud — *Vertex AI Model Garden*. [Docs](https://cloud.google.com/model-garden)
- Databricks — *Mosaic AI*. [Page](https://www.databricks.com/product/machine-learning/mosaic-ai)
- IBM — *watsonx.ai*. [Page](https://www.ibm.com/products/watsonx-ai)
- Together AI — [Site](https://www.together.ai/) · [Pricing](https://www.together.ai/pricing)
- Fireworks AI — [Site](https://fireworks.ai/) · [Pricing](https://fireworks.ai/pricing)
- Groq — [Site](https://groq.com/) · [Models](https://console.groq.com/docs/models)
- Cerebras — [Cerebras Inference](https://www.cerebras.net/inference/)
- SambaNova — *SambaNova Cloud*. [Site](https://cloud.sambanova.ai/)
- Replicate — [Site](https://replicate.com/)
- Modal — [Site](https://modal.com/)
- Anyscale — [Site](https://www.anyscale.com/)

## DPA, data-use policy, and enterprise terms (chapter 05)

Primary documents the manager reads on every renewal. Pin the version date; vendors update these.

- OpenAI — *Data Processing Addendum*. [Page](https://openai.com/policies/data-processing-addendum/) · *Enterprise privacy*. [Page](https://openai.com/enterprise-privacy) · *Sub-processors list*. [Page](https://openai.com/policies/sub-processor-list/)
- Anthropic — *Data Processing Addendum*. [Page](https://www.anthropic.com/legal/dpa) · *Commercial Terms of Service*. [Page](https://www.anthropic.com/legal/commercial-terms) · *Sub-processor list*. [Page](https://www.anthropic.com/legal/subprocessors) · *Privacy Policy*. [Page](https://www.anthropic.com/legal/privacy)
- Google Cloud — *Data Processing and Security Terms*. [Page](https://cloud.google.com/terms/data-processing-terms) · *Vertex AI — generative AI data governance*. [Docs](https://cloud.google.com/vertex-ai/generative-ai/docs/data-governance) · *Sub-processors*. [Page](https://cloud.google.com/terms/subprocessors)
- Microsoft — *Product Terms and DPA*. [Page](https://www.microsoft.com/licensing/docs/customeragreement) · *Azure OpenAI — data, privacy, and security*. [Docs](https://learn.microsoft.com/azure/ai-services/openai/concepts/data-privacy) · *Sub-processors*. [Page](https://aka.ms/subcontractors)
- AWS — *GDPR Center and DPA*. [Page](https://aws.amazon.com/compliance/gdpr-center/) · *Bedrock data protection*. [Docs](https://docs.aws.amazon.com/bedrock/latest/userguide/data-protection.html)
- Cohere — *DPA* published on legal page. [Page](https://cohere.com/terms-of-use)
- Mistral — *DPA and Terms*. [Page](https://mistral.ai/terms)

## Model Cards, system cards, and lab governance frameworks (chapters 03, 07)

Required reading per release; chapter 07's reading discipline.

### Lab-level governance

- Anthropic — *Responsible Scaling Policy (RSP)*. [Page](https://www.anthropic.com/rsp)
- OpenAI — *Preparedness Framework*. [Page](https://openai.com/safety/preparedness/)
- Google DeepMind — *Frontier Safety Framework (FSF)*. [Page](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/)
- Meta — *Responsible Use Guide* for Llama. [Site](https://www.llama.com/responsible-use-guide/)

### System cards and technical reports

- Anthropic — per-model System Cards (e.g. "Claude [version] System Card"), linked from the model's docs page.
- OpenAI — per-model System Cards (e.g. GPT-4, GPT-4o, o-series preparedness evaluations), published on the OpenAI research page.
- Google DeepMind — *Gemini* technical reports, per version. [Page](https://deepmind.google/technologies/gemini/)
- Meta — *Llama* model cards, published per release on the Llama models page.

### Model-Card origins and companion papers

- Mitchell, Wu, Zaldivar, Barnes, Vasserman, Hutchinson, Spitzer, Raji, Gebru — *Model Cards for Model Reporting* (FAT* 2019). [arXiv:1810.03993](https://arxiv.org/abs/1810.03993)
- Gebru, Morgenstern, Vecchione, Vaughan, Wallach, Daumé III, Crawford — *Datasheets for Datasets* (CACM 2021). [arXiv:1803.09010](https://arxiv.org/abs/1803.09010)
- Pushkarna, Zaldivar, Kjartansson — *Data Cards — Purposeful and Transparent Dataset Documentation*. [arXiv:2204.01075](https://arxiv.org/abs/2204.01075)

## Open-weights serving stacks (chapter 06)

Serving-stack landscape at commissioning-manager depth.

- vLLM — [Site](https://docs.vllm.ai/) · [GitHub](https://github.com/vllm-project/vllm)
- SGLang — [GitHub](https://github.com/sgl-project/sglang) · [Docs](https://docs.sglang.ai/)
- Hugging Face — *Text Generation Inference (TGI)*. [GitHub](https://github.com/huggingface/text-generation-inference)
- Hugging Face — *Text Embeddings Inference (TEI)*. [GitHub](https://github.com/huggingface/text-embeddings-inference)
- NVIDIA — *TensorRT-LLM*. [Docs](https://nvidia.github.io/TensorRT-LLM/) · [GitHub](https://github.com/NVIDIA/TensorRT-LLM)
- NVIDIA — *Triton Inference Server*. [Docs](https://docs.nvidia.com/deeplearning/triton-inference-server/)
- Ollama — [Site](https://ollama.com/) (prototyping / developer-local)
- LM Studio — [Site](https://lmstudio.ai/) (developer-local)

Inference research primers at manager depth:

- Kwon, Li, Zhuang, Sheng, Zheng, Yu, Gonzalez, Zhang, Stoica — *Efficient Memory Management for Large Language Model Serving with PagedAttention* (vLLM). [arXiv:2309.06180](https://arxiv.org/abs/2309.06180)
- Leviathan, Kalman, Matias — *Fast Inference from Transformers via Speculative Decoding*. [arXiv:2211.17192](https://arxiv.org/abs/2211.17192)
- Dao — *FlashAttention-2*. [arXiv:2307.08691](https://arxiv.org/abs/2307.08691)

## Abstraction layers and multi-provider gateways (chapter 04)

The abstraction every production call should route through.

- LiteLLM — [Docs](https://docs.litellm.ai/) · [GitHub](https://github.com/BerriAI/litellm)
- LangChain — [Docs](https://python.langchain.com/) · *LangGraph* [Docs](https://langchain-ai.github.io/langgraph/)
- Portkey AI — [Docs](https://docs.portkey.ai/)
- OpenRouter — [Site](https://openrouter.ai/)
- Vercel — *AI SDK*. [Docs](https://sdk.vercel.ai/docs)
- Mastra — [Site](https://mastra.ai/)

OpenTelemetry integration — pin the convention even if you pick a proprietary store:

- OpenTelemetry — *Generative AI semantic conventions*. [Spec](https://opentelemetry.io/docs/specs/semconv/gen-ai/)

## Benchmarks, leaderboards, and direction-of-travel signals (chapter 02, chapter 07)

Public benchmarks matter only as direction-of-travel for the inventory and the rapid-add path. Not a substitute for the team's own eval; see chapter 03's anti-pattern 2.

- Stanford HELM — *Holistic Evaluation of Language Models*. [Project page](https://crfm.stanford.edu/helm/) · [arXiv:2211.09110](https://arxiv.org/abs/2211.09110)
- LMSYS — *Chatbot Arena* (now `lmarena.ai`). [Site](https://lmarena.ai/)
- Artificial Analysis — independent cost / latency / quality benchmarks across providers. [Site](https://artificialanalysis.ai/)
- Hendrycks et al. — *MMLU*. [arXiv:2009.03300](https://arxiv.org/abs/2009.03300)
- Chen et al. — *HumanEval*. [arXiv:2107.03374](https://arxiv.org/abs/2107.03374)
- *GPQA* (graduate-level Q&A). [arXiv:2311.12022](https://arxiv.org/abs/2311.12022)
- *MMLU-Pro*. [arXiv:2406.01574](https://arxiv.org/abs/2406.01574)
- *SWE-bench* (software-engineering agents). [arXiv:2310.06770](https://arxiv.org/abs/2310.06770)
- Yao et al. — *τ-bench* (tool-agent-user). [arXiv:2406.12045](https://arxiv.org/abs/2406.12045)

## Risk management and governance frames (chapters 03, 05, 07)

Pin alongside the mod-104 safety-floor documents for the risk-posture section of the selection memo.

- NIST — *AI Risk Management Framework 1.0*. [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — *Generative AI Profile* (AI 600-1). [Page](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile)
- OWASP — *Top 10 for LLM Applications*. [Project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- EU AI Act — *Regulation (EU) 2024/1689*. [Consolidated text](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 25 (obligations along the AI value chain), 50 (transparency obligations), 53–55 (general-purpose AI models).
- ISO/IEC — *ISO/IEC 42001:2023 — AI Management Systems*. [Standard page](https://www.iso.org/standard/81230.html)
- US EDPB — *EU-US Data Privacy Framework*. [Page](https://commission.europa.eu/law/law-topic/data-protection/international-dimension-data-protection/eu-us-data-transfers_en)

## Status pages and incident monitoring (chapters 04, 07)

For the on-call runbook and the fallback plan.

- OpenAI — [status.openai.com](https://status.openai.com/)
- Anthropic — [status.anthropic.com](https://status.anthropic.com/)
- Google Cloud — [status.cloud.google.com](https://status.cloud.google.com/) (Vertex AI listed)
- AWS — [health.aws.amazon.com](https://health.aws.amazon.com/)
- Azure — [status.azure.com](https://status.azure.com/)
- Mistral — [status.mistral.ai](https://status.mistral.ai/)
- Cohere — [status.cohere.com](https://status.cohere.com/)
- Together AI — [status.together.ai](https://status.together.ai/)
- Fireworks AI — [status.fireworks.ai](https://status.fireworks.ai/) (if available for your contract)
- Groq — [groqstatus.com](https://groqstatus.com/)
- Cerebras — provider-published status page.

## Procurement, SOC 2, and compliance evidence (chapter 05)

- AICPA — *SOC 2* overview. [Page](https://www.aicpa-cima.com/resources/landing/system-and-organization-controls-soc-suite-of-services)
- ISO/IEC — *ISO/IEC 27001*. [Standard page](https://www.iso.org/standard/27001)
- HHS — *HIPAA Business Associate Agreement* reference. [Page](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)
- US FedRAMP — [Page](https://www.fedramp.gov/)
- Cloud Security Alliance — *CAIQ* and *STAR Registry*. [Page](https://cloudsecurityalliance.org/star/)

## SRE, FinOps, and build-vs-buy practice (chapters 03, 06)

- Google — *Site Reliability Engineering*. [Free online](https://sre.google/sre-book/table-of-contents/)
- Google — *The Site Reliability Workbook*. [Free online](https://sre.google/workbook/table-of-contents/)
- FinOps Foundation — *FinOps Framework*. [Page](https://www.finops.org/framework/)
- Thiry — *The Cloud Economics Playbook*. (Referenced; practitioners' guide to cloud cost accounting.)

## Model-change announcements and release note channels (chapter 07)

Subscribe; the subscription log in chapter 07 names who owns each.

- OpenAI — *Changelog*. [Docs](https://platform.openai.com/docs/changelog) · *Release notes* (blog). [Page](https://openai.com/blog/)
- Anthropic — *Changelog*. [Docs](https://docs.anthropic.com/en/release-notes/overview) · *News and research*. [Page](https://www.anthropic.com/news)
- Google — *Vertex AI release notes*. [Docs](https://cloud.google.com/vertex-ai/docs/release-notes) · *Gemini API release notes*. [Docs](https://ai.google.dev/gemini-api/docs/changelog)
- Mistral — *Changelog*. [Docs](https://docs.mistral.ai/getting-started/changelog/)
- Cohere — *Release notes*. [Docs](https://docs.cohere.com/release-notes)
- AWS Bedrock — *What's New*. [Page](https://aws.amazon.com/about-aws/whats-new/machine-learning/)
- Azure AI Foundry — *What's new*. [Docs](https://learn.microsoft.com/azure/ai-studio/whats-new)

## Where this module reads out to other modules

- `mod-104-eval-driven-sdlc-and-release-gates` — chapters 02 and 06 of mod-104 are the depth on the eval evidence this module cites in every selection memo.
- `mod-106-safety-guardrails-and-the-release-triad` — the vendor's RSP / Preparedness / FSF framework is one of the inputs to mod-106's release-triad safety case.
- `mod-107-ai-incident-response-and-postmortems` — the fallback plan (chapter 04) and the tabletop are mod-107's day-one playbooks; vendor outages are the incidents.
- `mod-109-team-shape-span-and-ai-operating-cadence` — the quarterly portfolio review (chapter 07) is a beat on mod-109's operating cadence; the peer AI Infra manager partnership is one of mod-109's cross-ladder spans.
- `mod-110-budget-finops-and-build-vs-buy` — the cost models in chapters 03 and 06 are line items mod-110 defends in the quarterly plan; the self-host build-vs-buy call is the AI-specific instance of mod-110's broader frame.
- `mod-111-cross-functional-partnership` — the procurement and legal partnerships from chapter 05 and the Infra partnership from chapter 06 are three of the cross-functional surfaces mod-111 covers.
- `mod-112-executive-communication-and-stakeholder-reporting` — the quarterly-review memo (chapter 07) and the self-host capability brief (chapter 06) are inputs to mod-112's executive-communication artefacts.
