# AI-300 Exam Decision Reference (Cheatsheet)

> Night-before / morning-of summary. All trees + magic words on one page.

---

## Compute

| Need | Pick |
|---|---|
| Notebook authoring | Compute instance |
| Submitted training job | Compute cluster |
| No infra to manage | Serverless compute |
| Hybrid / on-prem | K8s online (AKS / Arc) |

## Job

| Intent | Job type |
|---|---|
| One script | Command |
| HP search | Sweep |
| DAG | Pipeline |
| Map over data | Parallel |

## Sampling x Termination

| Sampling | OK with early-term? |
|---|---|
| Grid | y |
| Random | y |
| Bayesian | n (must run to completion) |

## Endpoint

| Tolerance | Endpoint |
|---|---|
| Real-time, managed | Managed online |
| Real-time, your K8s | K8s online |
| Async / large file | Batch |

## Rollout

| Strategy | Risk |
|---|---|
| Mirror (<=50%) | Lowest |
| Canary (5/25/100) | Medium |
| Blue/green swap | High |

---

## GenAI deployment SKU

| Need | SKU |
|---|---|
| Predictable peak latency | **PTU** |
| Sporadic, cheap | Standard PAYG |
| Bulk offline, 50% cheaper | Batch |
| Lowest p50 globally | Global Standard |
| EU/US data residency | DataZone Standard |

## Quality vs cost lever

| Goal | Lever |
|---|---|
| Add up-to-date knowledge | RAG |
| Style / format | Few-shot |
| Narrow repetitive task | SFT |
| Align preferences | DPO |
| Multi-step reasoning | RFT (o-series) |

## Top-K / RAG defaults

| Knob | Default |
|---|---|
| Chunk size | 500-1500 tokens |
| Overlap | 10-20% |
| Top-K | 3-5 |
| Embedding | `text-embedding-3-large` (high) / `-small` (cost) |
| Retrieval | Hybrid + semantic ranker |

---

## Identity quick reference

| Caller | Role | Resource |
|---|---|---|
| GitHub Actions | Federated cred + custom RBAC | RG / WS |
| Online endpoint MSI | Storage Blob Data Reader | Storage |
| AOAI MSI (On Your Data) | Search Index Data Reader | AI Search |
| AOAI MSI (On Your Data) | Storage Blob Data Reader | Source storage |
| App backend MSI | Cognitive Services OpenAI User | AOAI |
| Indexer MSI | Storage Blob Data Reader | Source storage |
| Developer | Azure AI Developer | Foundry project |

---

## Magic-words -> answer

| Phrase | Answer |
|---|---|
| "predictable peak latency" | **PTU** |
| "no infrastructure to manage" (training) | **Serverless compute** |
| "no infrastructure to manage" (GenAI) | **Foundry / Standard deployment** |
| "without storing credentials" | **OIDC / managed identity** |
| "shadow test" | **Mirror traffic** |
| "without writing scoring code" | **mlflow_model** |
| "detect distribution shift" | **Data drift monitor** |
| "answer must cite sources" | **RAG + GroundednessEvaluator** |
| "block harmful content at runtime" | **Content Safety filters** |
| "test prompts at scale" | **Prompt flow batch + offline eval** |
| "trace agent steps" | **Foundry tracing -> App Insights** |
| "simulate jailbreak" | **AI Red Teaming Agent** |
| "50% cheaper bulk LLM workload" | **Batch API** |
| "cache repeated prompts" | **AOAI prompt cache or APIM semantic cache** |
| "regional residency" | **DataZone Standard** |
| "burst capacity beyond PTU" | **Global Standard fallback via APIM/Front Door** |

---

## 30-second mental checklist

1. **Latency** sensitive? -> managed online OR PTU.
2. **Cost** sensitive? -> serverless / Standard / Batch / cache.
3. **Quality** issue? -> RAG -> few-shot -> SFT -> DPO -> RFT.
4. **Identity**? -> MSI + OIDC, no secrets.
5. **Network**? -> private endpoints + VNet injection.
6. **Observability**? -> App Insights + Foundry tracing + evaluators.
7. **Safety**? -> Content Safety + Prompt Shields + red-team.

[<- Master Index](00-MASTER-INDEX.md)
