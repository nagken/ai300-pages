# AI-300 - Operationalizing Machine Learning and Generative AI Solutions - Visual Study Guide

> Concept-only study aid. No exam questions reproduced. Source PDF (if any) stays local + gitignored. **AI-300 is a beta exam** - Microsoft has not released an official practice assessment yet.

**Skills outline:** <https://aka.ms/AI300-StudyGuide>

---

## How to use this guide

1. Start at this page; click any sidebar entry on the left.
2. **Domain pages 01-05** carry the deep diagrams + decision trees + tables.
3. **Cheatsheet (05)** is the night-before reference.
4. **Extra concepts (07)** + **Architectures (09)** consolidate cross-cutting patterns (MLOps + GenAIOps end-to-end).
5. **Pitfalls (14)** lists the top traps the beta items will probe.
6. **Copilot Quiz (17)** generates infinite practice questions from the official outline.

---

## Domain map

```mermaid
mindmap
  root((AI-300))
    MLOps Infra (22%)
      Azure ML workspace
      Compute & networking
      Data + features
      CI/CD GitHub Actions / Azure DevOps
      Bicep / Terraform IaC
    Model Lifecycle (22%)
      Train + register
      Promote (dev/test/prod)
      Online + batch deploy
      Monitor drift + perf
      Retrain triggers
    GenAIOps Infra (22%)
      Microsoft Foundry hub + project
      Model catalog + provisioned vs PAYG
      Prompt flow + agents
      RAG with Azure AI Search
      Content safety + responsible AI
    GenAI Quality (18%)
      Eval datasets
      Built-in + custom evaluators
      A/B + offline + online evals
      Tracing + spans
      Red-team + groundedness
    Optimize GenAI (16%)
      Latency + token cost
      Caching + batching
      Fine-tuning vs prompt
      Quotas + rate limits
      Routing + load balance
```

---

## Domain weights

```mermaid
pie showData
    title AI-300 domain weights
    "Design and Implement MLOps Infrastructure" : 22
    "ML Model Lifecycle and Operations" : 22
    "Design and Implement GenAIOps Infrastructure" : 22
    "GenAI Quality Assurance and Observability" : 18
    "Optimize GenAI Systems and Performance" : 16
```

---

## End-to-end service map

```mermaid
flowchart LR
    DEV[Developer] --> REPO[GitHub or Azure Repos]
    REPO --> CI{CI / CD<br/>GitHub Actions or Azure Pipelines}
    CI -->|OIDC| AAD[Microsoft Entra ID]
    CI --> IAC[Bicep / Terraform]
    IAC --> AML[(Azure ML Workspace)]
    IAC --> FDR[(Microsoft Foundry hub + project)]
    AML --> CC[Compute cluster]
    AML --> EP[Online + batch endpoints]
    FDR --> MC[Model catalog<br/>Azure OpenAI + Foundry models]
    FDR --> PF[Prompt flow<br/>+ agents]
    PF --> SEARCH[(Azure AI Search<br/>RAG index)]
    PF --> CS[Content Safety]
    EP --> APPI[(Application Insights)]
    PF --> APPI
    APPI --> EVAL[Continuous evaluation]
    EVAL -->|drift / quality alert| CI
```

---

## 6 question patterns to recognize

| Pattern | Trigger words | Likely answer family |
|---|---|---|
| **MLOps infra design** | "design infra", "Bicep", "GitHub Actions", "promote across environments" | Azure ML + IaC + OIDC + environment promotion |
| **Model lifecycle** | "register", "deploy", "blue/green", "monitor", "retrain" | Model registry, online/batch endpoint, data collector, drift monitor |
| **GenAIOps infra** | "Foundry hub", "agent", "prompt flow", "RAG", "content safety" | Foundry project + AI Search + content safety + tracing |
| **GenAI quality** | "evaluator", "groundedness", "A/B", "red-team", "trace" | Built-in evals + custom evaluators + tracing in Foundry |
| **Optimize GenAI** | "latency", "tokens", "throughput", "cache", "fine-tune" | Provisioned throughput, caching, batch, model routing |
| **Responsible AI** | "fairness", "harm", "PII", "jailbreak", "groundedness" | Content Safety + RAI dashboard + groundedness eval |

---

## Magic-words translator (Microsoft phrasing -> service)

| If the question says... | Pick... |
|---|---|
| Predictable peak latency for chat | **Provisioned Throughput Units (PTU)** for Azure OpenAI |
| Cost-effective sporadic GenAI | **Standard (PAYG)** Azure OpenAI |
| "No infrastructure to manage" for ML training | **Serverless compute** |
| "Reusable training workflow" | **Pipeline + components** |
| "Without storing credentials" CI/CD | **OIDC / workload identity federation** |
| "Detect distribution shift" in inputs | **Data drift monitor** |
| "Measure response quality" GenAI | **Built-in evaluators** (groundedness, relevance, fluency, coherence) |
| "Simulated jailbreak" | **AI Red Teaming Agent / safety evaluators** |
| Internal-only enterprise data Q&A | **RAG with Azure AI Search** |
| "Test prompt variants safely" | **Prompt flow** + offline evaluation runs |
| Tracing across agent steps | **Foundry tracing** (OpenTelemetry, App Insights) |
| Block harmful content at runtime | **Azure AI Content Safety** filters |
| Low-latency multi-region GenAI | **Load balancer + multiple deployments** (round-robin or weighted) |
| Cheaper inference for known prompts | **Prompt caching** or **provisioned reservations** |

---

## 14-day study plan

```mermaid
gantt
    title AI-300 14-day plan
    dateFormat  YYYY-MM-DD
    section Week 1 - MLOps + Lifecycle
    Read 01 + 02                  :a1, 2025-12-01, 3d
    Lab: pipeline + endpoint      :a2, after a1, 2d
    section Week 1.5 - GenAIOps
    Read 03 (Foundry + RAG)       :b1, after a2, 2d
    Lab: prompt flow + RAG        :b2, after b1, 2d
    section Week 2 - Quality + Optimize
    Read 04 + 05                  :c1, after b2, 2d
    Lab: evaluators + tracing     :c2, after c1, 1d
    section Wrap
    Cheatsheet + flashcards       :d1, after c2, 1d
    Copilot quiz drills           :d2, after d1, 1d
```

---

## Beta exam reminders

- **No public retired-question dump** - rely on the skills outline + Microsoft Learn.
- **No official Microsoft practice assessment yet** - use [Copilot Quiz](17-copilot-quiz.md) for infinite practice.
- Score is **delayed** ~10 days after testing window closes (standard for beta).
- Heavily weighted toward **GenAIOps** (60%+ when you sum domains 3, 4, 5).

---

[Sidebar: pick a domain to begin ->]
