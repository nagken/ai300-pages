# Azure Architecture Center

> The **[Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)** is the single most important external resource for AI-300. It hosts every reference architecture, design pattern, and Well-Architected workload review you may be tested on. Browse the full catalog at [learn.microsoft.com/azure/architecture/browse](https://learn.microsoft.com/azure/architecture/browse/) and filter by product, category, or scenario.

## How to use it for AI-300

1. Read the **WAF for AI workloads** entry first; understand reliability, security, cost, ops, and performance trade-offs for GenAI systems.
2. Pick **two or three reference architectures per topic area** (chat baseline, RAG, multi-agent, AI gateway, landing zone) and walk them end-to-end.
3. Memorize the **decision points** - PTU vs PAYGO, hybrid vs vector, single-region vs multi-region, public vs private endpoint.
4. Use the **browse filters** (cost, security, reliability) to find the WAF-aligned variant of each scenario.

## Top entry points

| Resource | Why it matters for AI-300 |
| --- | --- |
| [Architecture Center home](https://learn.microsoft.com/azure/architecture/) | Curated landing page; start here. |
| [Browse architectures](https://learn.microsoft.com/azure/architecture/browse/) | Filterable catalog of every reference architecture. |
| [AI and machine learning architectures](https://learn.microsoft.com/azure/architecture/ai-ml/) | Top-level AI/ML scenario index. |
| [WAF for AI workloads](https://learn.microsoft.com/azure/well-architected/ai/) | Five-pillar guidance applied to GenAI. |
| [Cloud Adoption Framework - AI](https://learn.microsoft.com/azure/cloud-adoption-framework/scenarios/ai/) | Adoption, governance, and AI landing-zone guidance. |
| [Cloud design patterns](https://learn.microsoft.com/azure/architecture/patterns/) | Retry, throttling, queue-based load levelling - useful for LLM front-ends. |

## Reference architectures by AI-300 topic

### Baseline chat and RAG

- [Baseline Azure OpenAI end-to-end chat](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-openai-e2e-chat)
- [Baseline Azure AI Foundry chat](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-azure-ai-foundry-chat)
- [Baseline AKS chat with AOAI](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-azure-ai-foundry-landing-zone)
- [RAG solution design and evaluation guide](https://learn.microsoft.com/azure/architecture/ai-ml/guide/rag/rag-solution-design-and-evaluation-guide)

### MLOps and GenAIOps

- [MLOps with Azure ML](https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-technical-paper)
- [MLOps maturity model](https://learn.microsoft.com/azure/architecture/ai-ml/guide/mlops-maturity-model)
- [GenAIOps maturity model](https://learn.microsoft.com/azure/architecture/ai-ml/guide/genaiops-maturity-model)

### Real-time and batch scoring

- [Real-time scoring with Azure ML](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/realtime-scoring-r)
- [Batch scoring with Azure ML](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/batch-scoring-python)

### Landing zones and platform

- [Azure OpenAI Landing Zone (CAF)](https://learn.microsoft.com/azure/cloud-adoption-framework/scenarios/ai/platform/landing-zone)
- [AI Foundry Landing Zone](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-azure-ai-foundry-landing-zone)
- [Enterprise-scale for AI](https://learn.microsoft.com/azure/cloud-adoption-framework/scenarios/ai/)

### AI gateway and traffic management

- [API Management as AI Gateway](https://learn.microsoft.com/azure/api-management/genai-gateway-capabilities)
- [Front Door multi-region active-active](https://learn.microsoft.com/azure/architecture/web-apps/guides/multi-region-app-service)

### Multi-agent and connected systems

- [Multi-agent design with connected agents](https://learn.microsoft.com/azure/ai-foundry/agents/concepts/connected-agents)
- [Hub-and-spoke private network](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke)

### Well-Architected for AI workloads

- [WAF for AI workloads](https://learn.microsoft.com/azure/well-architected/ai/)
- [WAF design principles for AI](https://learn.microsoft.com/azure/well-architected/ai/design-principles)

[<- Master Index](00-MASTER-INDEX.md)
