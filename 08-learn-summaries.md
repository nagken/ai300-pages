# Microsoft Learn Summaries

> One-paragraph summaries per concept. Use as flashcards-style review.

---

**Microsoft Foundry hub** - Top-level GenAI workspace. Owns shared infrastructure (storage, key vault, app insights), connections (AOAI, Search, Storage, third-party), and policy. Multiple **projects** sit inside one hub and share its security perimeter while keeping their own assets, indexes, agents, and evaluation runs.

**Foundry project** - Per-app workspace inside a hub. Holds prompt flows, agents, indexes, evaluation runs, deployments, and tracing. Most RBAC and dev work happens at the project scope.

**Azure OpenAI Standard (PAYG)** - Pay-per-token deployment, shared backend capacity. Rate-limited by TPM/RPM quota; can hit 429s under burst. Ideal for dev and intermittent prod traffic.

**Azure OpenAI Provisioned (PTU)** - Reserved capacity sold in PTU units; cost is per PTU/hour regardless of token usage. Predictable latency and no 429s within budget. Used for high-RPS production with SLA.

**Azure OpenAI Batch API** - Async bulk job: upload JSONL, model returns within 24h, 50% discount vs sync. For embeddings backfill, classification at scale, offline summarization.

**Global Standard / DataZone Standard** - Variants of PAYG. Global routes to lowest-latency region globally; DataZone constrains to US or EU regions for residency.

**Prompt flow** - DAG of nodes (prompt template, llm call, python, lookup, etc.). Versioned, runnable in batch, evaluatable offline. Becomes a deployable artifact in a Foundry project.

**Foundry Agent Service** - Hosted agent runtime with tools (file search, code interpreter, function calling, Bing grounding, connected agents). Tracks threads + messages and emits OpenTelemetry spans.

**Connected agent** - A child agent exposed as a tool to a parent. Enables multi-agent orchestration without writing your own router.

**Azure AI Search hybrid retrieval** - Combines BM25 keyword + vector ANN search. Optionally re-ranked by the **semantic ranker** (transformer-based) for higher quality at the top of results.

**Indexer skillset** - Pull pipeline from a data source (Blob, ADLS, SQL, SharePoint) into a search index. Skills include OCR, layout, key-phrase extraction, chunking, embedding via AOAI.

**Azure AI Content Safety** - Standalone service or built-into AOAI. Categories: hate, sexual, violence, self-harm. Adds **Prompt Shields** for jailbreak / indirect-prompt-injection detection. Also **Groundedness detection** and **Protected Material** filters.

**Built-in evaluators** - RelevanceEvaluator, CoherenceEvaluator, FluencyEvaluator, GroundednessEvaluator, SimilarityEvaluator (quality), ContentSafetyEvaluator, ProtectedMaterialEvaluator, IndirectAttackEvaluator, CodeVulnerabilityEvaluator (risk/safety). Run via `azure-ai-evaluation` SDK against `.jsonl` datasets.

**Custom evaluator** - Any Python callable returning a dict, plugged into `evaluate(...)`. Use for domain-specific rules ("must include disclaimer", "must cite a source", "must not echo PII").

**Foundry tracing** - OpenTelemetry-based; emits spans for prompt flow nodes, agent tool calls, llm calls. Exports to Application Insights when wired through the project. Foundry portal renders timeline + token usage.

**Continuous evaluation** - Sample N% of prod traffic, run evaluators on the sample, surface trend metrics + alerts when groundedness or safety regress.

**AI Red Teaming Agent** - Simulates ~50 attack categories (jailbreak, indirect injection, harm, bias, copyright). Produces a risk report with attack-success rates per category.

**Azure Machine Learning workspace** - Top-level resource for classic ML workflows (data, jobs, compute, models, endpoints). Often paired with a Foundry hub when GenAI is added to an existing ML org.

**Compute cluster vs serverless** - Cluster is autoscaling 0..N VMs that you size and own; serverless is per-job compute that Microsoft manages. Cluster is best for predictable training cadences; serverless for sporadic / no-ops scenarios.

**Online endpoint (managed)** - Real-time HTTPS scoring URL backed by Microsoft-managed VMs. Holds 1+ deployments (blue, green) behind a single endpoint, with traffic split + mirror.

**Batch endpoint** - Async, file-based scoring. Submits a job over a folder of inputs to a compute cluster; writes outputs back to storage.

**Data Collector** - Logs request payloads + responses from an online endpoint to Blob storage. Required upstream for drift and performance monitors.

**Model performance monitor** - Joins captured payloads with ground-truth labels you ingest, then computes accuracy / RMSE / etc. and alerts on regression.

**OIDC / federated credential** - GitHub Actions or Azure DevOps trades a short-lived OIDC token for an Azure access token via Microsoft Entra workload-identity-federation. Removes the need for stored client secrets.

**APIM as AI Gateway** - Azure API Management policies for token rate-limit, semantic caching, load-balance across deployments, PII redaction. Lets one URL serve a pool of AOAI deployments.

**RFT (reasoning fine-tune)** - Reinforcement-style fine-tune on `o1`/`o3`-class models for multi-step reasoning tasks. Used when domain reasoning fails despite RAG and SFT.

[<- Master Index](00-MASTER-INDEX.md)
