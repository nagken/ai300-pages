# Architectures - AI-300

> Reference architectures combining MLOps + GenAIOps.

---

## 1. End-to-end GenAIOps with RAG

```mermaid
flowchart LR
    DEV[Developer] -->|git push| REPO[GitHub]
    REPO --> CI{GitHub Actions}
    CI -->|OIDC| AAD[Microsoft Entra]
    CI --> IAC[Bicep / AVM]
    IAC --> HUB[(Foundry Hub)]
    IAC --> PRJ[(Foundry Project)]
    IAC --> AOAI[(Azure OpenAI<br/>PTU 100)]
    IAC --> SEARCH[(AI Search<br/>Standard)]
    IAC --> APPI[(App Insights)]
    PRJ --> FLOW[Prompt flow]
    FLOW --> SEARCH
    FLOW --> AOAI
    USER[/User/] --> AFD[Front Door + WAF]
    AFD --> APP[Container Apps]
    APP --> FLOW
    APP --> APPI
    APPI --> EVAL[Continuous evaluation]
    EVAL -->|alert| CI
```

---

## 2. Multi-region PTU with global fallback

```mermaid
flowchart LR
    USER[/Client/] --> APIM[Azure API Management]
    APIM -->|weight 50| EAST[(East US AOAI<br/>PTU)]
    APIM -->|weight 50| WEU[(West Europe AOAI<br/>PTU)]
    APIM -->|fallback on 429| GLB[(Global Standard)]
    APIM --> APPI[(App Insights)]
```

> APIM applies token rate-limit, semantic cache, and PII redaction policies before backend dispatch.

---

## 3. Network-isolated Foundry

```mermaid
flowchart LR
    subgraph VNET[Customer VNet]
        subgraph HUBSUB[Hub subnet]
            PE_HUB[PE -> Foundry hub]
        end
        subgraph DATASUB[Data subnet]
            PE_AOAI[PE -> Azure OpenAI]
            PE_SRCH[PE -> AI Search]
            PE_ST[PE -> Storage]
            PE_KV[PE -> Key Vault]
        end
        subgraph APPSUB[App subnet]
            APP[App / Container Apps]
        end
    end
    USER[End user] --> AFD[Front Door private]
    AFD --> APP
    APP --> PE_AOAI
    APP --> PE_HUB
    PE_HUB --> HUB[(Foundry hub)]
    HUB --> PE_AOAI
    HUB --> PE_SRCH
    HUB --> PE_ST
```

---

## 4. Hybrid MLOps + GenAIOps

```mermaid
flowchart LR
    REPO[Mono-repo] --> CI[CI]
    CI --> ML_PIPE[Azure ML pipeline<br/>train classical model]
    CI --> GENAI[Foundry deploy<br/>prompt flow + agent]
    ML_PIPE --> ML_EP[Online endpoint<br/>fraud score]
    GENAI --> CHAT[Chat agent<br/>customer support]
    APP[App Service] --> ML_EP
    APP --> CHAT
    APP --> APPI[(App Insights)]
```

---

## 5. RAG with private indexer

```mermaid
flowchart LR
    SRC[(Blob - PDFs)] --> IDXR[AI Search indexer<br/>MSI auth]
    IDXR --> SK[Skillset:<br/>OCR + chunk + embed]
    SK --> EMB[(AOAI embeddings)]
    SK --> IDX[(AI Search index)]
    USR[/Query/] --> APP[App]
    APP --> SEARCH_API[Hybrid search + ranker]
    SEARCH_API --> IDX
    SEARCH_API --> CTX[Top-K chunks]
    CTX --> CHAT[(AOAI chat)]
    CHAT --> ANS[/Cited answer/]
```

---

## 6. Continuous evaluation loop

```mermaid
flowchart LR
    USR[/Live traffic/] --> APP[App]
    APP --> APPI[(App Insights)]
    APPI -->|sample 5%| LOG[Sampled traces]
    LOG --> EVAL[Eval run<br/>Groundedness, ContentSafety, Relevance]
    EVAL --> METRICS[Quality dashboard]
    METRICS -->|regression| ALERT[Action group]
    ALERT --> CI[CI/CD<br/>investigate / rollback]
```

---

## 7. Multi-agent system

```mermaid
flowchart LR
    USR[/User/] --> ORCH[Orchestrator agent]
    ORCH --> A1[Specialist: SQL]
    ORCH --> A2[Specialist: RAG]
    ORCH --> A3[Specialist: Code]
    A1 --> SQL[(Azure SQL)]
    A2 --> SEARCH[(AI Search)]
    A3 --> CI_TOOL[Code interpreter]
    ORCH --> APPI[(App Insights<br/>spans across agents)]
```

---

[<- Master Index](00-MASTER-INDEX.md)
