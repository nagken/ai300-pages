# Glossary and Acronym Reference

> Authoritative definitions for the GenAI, Azure OpenAI, Azure AI Foundry, and Azure ML terms that appear in AI-300 scenarios.

## Foundry and Workspaces

| Term | Definition |
| --- | --- |
| **Hub** | Foundry **hub** - top-level resource holding shared infrastructure, connections, and storage. |
| **Project** | Foundry **project** - per-app workspace inside a hub for one team or application. |
| **Connection** | Auth + endpoint pair stored in the hub for AOAI, AI Search, storage, etc. |
| **Workspace** | Azure ML top-level resource. Each Foundry project has a backing workspace. |
| **Foundry SDK** | `azure-ai-projects` plus companion SDKs (inference, evaluation). |
| **Evaluation SDK** | `azure-ai-evaluation` - built-in and custom evaluators. |
| **Tracing** | OpenTelemetry spans for prompts, tools, and LLM calls. |
| **App Insights** | Azure Monitor APM, sink for traces and metrics. |

## Azure OpenAI

| Term | Definition |
| --- | --- |
| **AOAI** | Azure OpenAI. |
| **MaaS** | Models-as-a-Service serverless deployments in Foundry. |
| **PTU** | Provisioned Throughput Unit - reserved AOAI capacity for predictable latency. |
| **TPM / RPM** | Tokens-per-minute / requests-per-minute AOAI quota dimensions. |
| **Standard / Global / DataZone** | AOAI deployment SKU variants for residency and routing. |
| **Batch API** | Async AOAI bulk inference at ~50% discount with up-to-24h SLA. |
| **Prompt cache** | AOAI native cache of long stable prefixes. |
| **System message** | Persistent `role=system` prompt that anchors model behavior. |

## Agents and Prompt Orchestration

| Term | Definition |
| --- | --- |
| **Agent** | Foundry-hosted LLM with tools and a persistent thread. |
| **Connected agent** | Sub-agent invoked as a tool by a parent agent. |
| **Prompt flow** | DAG of prompt, Python, and LLM nodes. |
| **Function calling / tool use** | Structured tool invocation by the LLM. |
| **Few-shot** | In-context examples that steer model output. |

## Retrieval and RAG

| Term | Definition |
| --- | --- |
| **RAG** | Retrieval-Augmented Generation. |
| **Hybrid search** | BM25 plus vector ANN combined into a single result set. |
| **Semantic ranker** | Transformer-based reranker on top of search results (Standard tier+). |
| **Top-K** | Number of chunks returned to the model. Right range for RAG is 3-5. |
| **Indexer / skillset** | Pull pipeline into a search index, with built-in AI enrichment. |

## Fine-Tuning

| Term | Definition |
| --- | --- |
| **SFT** | Supervised Fine-Tune. |
| **DPO** | Direct Preference Optimization fine-tune. |
| **RFT** | Reinforcement Fine-Tune (o-series, multi-step reasoning). |
| **JSONL** | One JSON object per line - training and batch input format. |

## Quality and Safety

| Term | Definition |
| --- | --- |
| **Content Safety** | Hate / Sexual / Violence / Self-harm filter service. |
| **Prompt Shields** | Jailbreak and indirect prompt injection detection. |
| **XPIA** | Cross-Prompt Indirect Prompt Injection attack. |
| **RAI** | Responsible AI. |
| **RAI dashboard** | Azure ML Responsible AI dashboard. |
| **Groundedness** | Whether the answer is supported by retrieved context. |
| **Relevance / Coherence / Fluency / Similarity** | Built-in quality evaluators. |
| **Protected Material evaluator** | Detects copyrighted or leaked verbatim content. |
| **Indirect Attack evaluator** | Detects XPIA in tool or retrieval outputs. |
| **Code Vulnerability evaluator** | Static checks on generated code. |
| **AI Red Teaming Agent** | Synthetic adversarial probe runner. |
| **Continuous evaluation** | Periodic eval over sampled production traffic. |

## Azure ML Operations

| Term | Definition |
| --- | --- |
| **Compute instance / cluster** | Personal VM versus autoscaling pool. |
| **Serverless compute** | Microsoft-managed per-job compute. |
| **Command / Sweep / Pipeline / Parallel** | AML job types. |
| **Sweep** | Hyperparameter search job. |
| **Bayesian / Random / Grid** | Sweep sampling strategies. |
| **Bandit / MedianStop / TruncationSelection** | Early-termination policies. |
| **MLflow** | Tracking and model registry standard. |
| **Online endpoint** | Real-time HTTPS scoring URL. |
| **Batch endpoint** | Async file-based scoring endpoint. |
| **Managed online / K8s online** | Endpoint compute flavors. |
| **Blue-green / Canary / Mirror** | Deployment rollout strategies. **Mirror is capped at 50%.** |
| **Data collector** | Logs payloads from an online endpoint. |
| **Data drift / Performance monitor** | Scheduled comparisons of features and labels. |

## Networking and Identity

| Term | Definition |
| --- | --- |
| **MSI** | Managed Service Identity (system-assigned or user-assigned). |
| **OIDC** | OpenID Connect federated credential - no client secret. |
| **PE** | Private Endpoint. |
| **CMK** | Customer-Managed Key. |
| **APIM** | Azure API Management. |
| **AI Gateway** | APIM configured with GenAI-specific policies (token limit, semantic cache, content safety). |
| **Front Door** | Microsoft global L7 load balancer. |
| **Semantic cache** | APIM cache keyed on embedding similarity. |

## Tooling

| Term | Definition |
| --- | --- |
| **azd** | Azure Developer CLI. |
| **Bicep** | Azure-native IaC DSL. |
| **AVM** | Azure Verified Modules (Bicep and Terraform). |
| **AzureAIChatCompletionsModel** | Inference SDK client. |

[<- Master Index](00-MASTER-INDEX.md)
