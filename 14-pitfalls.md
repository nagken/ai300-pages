# Common Pitfalls and Distractor Patterns

> Mistakes that look right on the AI-300 exam but lose points. Each entry pairs the wrong choice candidates pick with the correct one and the rule.

## MLOps and GenAIOps Infrastructure

### Public Azure OpenAI behind a private app

**Pitfall**: Locking the front-end app to a private network but leaving Azure OpenAI on the public internet.

**Reality**: All-or-nothing. Add a **private endpoint** on the AOAI account, set `publicNetworkAccess=Disabled`, and route the app to the private endpoint via the workload VNet.

### Storing secrets in CI/CD pipelines

**Pitfall**: Storing a service principal client secret in GitHub Actions or Azure Pipelines.

**Reality**: Rotation pain + leak risk. Use **OIDC federated credential** on a user-assigned managed identity. No secret in CI.

### Single workspace for dev and prod

**Pitfall**: One Foundry hub serving both environments.

**Reality**: Blast radius + governance fail. Use one hub/project per environment in separate resource groups, with promotion via CI/CD.

### Missing diagnostic settings

**Pitfall**: Standing up Foundry, AOAI, AML, and AI Search without diagnostics.

**Reality**: No retention, no audit, no SOC2 evidence. Each resource needs a **diagnostic setting** to Log Analytics or Storage.

### Compute cluster pinned to a high `min_instances`

**Pitfall**: `min_instances: 4` "to keep nodes warm."

**Reality**: Pays around the clock even at zero load. Default to `min_instances: 0`; only raise it when SLA requires warm capacity.

## Model Lifecycle and Operations

### Bayesian sampling with early termination

**Pitfall**: Pairing Bayesian sampling with a Bandit policy.

**Reality**: **Invalid combination**. Bayesian must run trials to completion to update its model. Use Random or Grid for early termination.

### `score.py` written for an `mlflow_model` flavor

**Pitfall**: Hand-writing inference code for an MLflow-logged model.

**Reality**: With `mlflow_model` flavor you get **no-code deploy**; the SDK injects the scoring server. Skip the custom `score.py`.

### No traffic split after `update`

**Pitfall**: Deploying a new version and assuming it gets traffic automatically.

**Reality**: Default is **0%** to the new deployment - old still serves 100%. Always set traffic explicitly post-deploy.

### Mirror traffic treated as A/B testing

**Pitfall**: Believing mirror responses count toward metrics.

**Reality**: Mirror duplicates the request to the new deployment but **drops the response**. You pay for compute; you don't get user-graded results. Use blue/green for real A/B.

### Drift monitor without data collector

**Pitfall**: Creating a drift monitor on a deployment that never logs inputs.

**Reality**: Drift monitor needs the **Data Collector** enabled on the deployment first. Otherwise no inputs land in storage; the monitor never triggers.

### Retraining on every drift alert

**Pitfall**: Wiring drift alerts directly to a retrain pipeline.

**Reality**: Wasteful and risky without ground truth. Gate retrain on **label availability + performance monitor**, not on drift alone.

## GenAI Architecture

### PTU bought for the wrong model family

**Pitfall**: "We bought PTU for gpt-4o, that should help gpt-4o-mini too."

**Reality**: PTU is **per-model-family**. PTU on `gpt-4o` does nothing for `gpt-4o-mini`. Right-size PTU to the deployment that actually carries the load.

### Long system prompt repeated every call

**Pitfall**: Pasting the full system prompt in every request.

**Reality**: Burns tokens and latency. Use **prompt caching**: stable cached prefix + small variable suffix.

### Top-K set to 20 in RAG

**Pitfall**: Returning 20 documents from AI Search to "give the model more context."

**Reality**: Hurts grounding (noise crowds the answer) and cost (more tokens). Right range is **3-5** with semantic ranker.

### Embedding model mismatch

**Pitfall**: Index built with `text-embedding-3-large` but query embedded with `-small`.

**Reality**: Vector spaces don't align - recall collapses. Always use the **same embedding model and version** for index and query.

### Indexer authenticating with key

**Pitfall**: Wiring AI Search indexer to storage with an account key.

**Reality**: Secret to rotate, hard to audit. Use **MSI on the indexer** + `Storage Blob Data Reader` on the storage account.

### Skipping the semantic ranker

**Pitfall**: Defaulting to plain BM25 or pure vector retrieval.

**Reality**: On Standard tier+ semantic ranker is a 1-line config that lifts answer quality dramatically. **Hybrid + semantic** is the default for RAG.

### Foundry "On Your Data" without RBAC plumbing

**Pitfall**: Wiring Azure OpenAI On Your Data to AI Search and expecting it to work.

**Reality**: OYD requires the **AOAI managed identity** to have `Search Index Data Reader` (or Contributor) on AI Search and `Storage Blob Data Reader` on the storage account.

### No prompt template versioning

**Pitfall**: Editing prompts directly in the portal.

**Reality**: No diff, no rollback, no eval gate. Treat prompts as code - git-controlled with CI evaluation gates before promotion.

## Quality and Safety

### Evaluating only Relevance

**Pitfall**: Reporting Relevance and calling it done.

**Reality**: Misses hallucinations and harm. Always include **Groundedness + Content Safety** evaluators (and Protected Material for IP risk).

### Custom evaluator returns a string

**Pitfall**: `def __call__(self, response): return "good"`.

**Reality**: Must return a **dict**; the eval SDK aggregates by metric name. String returns break the pipeline.

### Full sampling of production traffic

**Pitfall**: Running continuous evaluation on 100% of production calls.

**Reality**: Cost explodes. Sample **1-10%** for ongoing eval; reserve full sweeps for offline regression suites.

### Skipping AI Red Teaming

**Pitfall**: Going to GA without adversarial testing.

**Reality**: Required step before launching anything with user-generated input. Use the **AI Red Teaming Agent** to generate adversarial probes and confirm Prompt Shields catches them.

### Ignoring Protected Material evaluator

**Pitfall**: Only running Groundedness and Content Safety.

**Reality**: Misses verbatim copyrighted output. Add **ProtectedMaterialEvaluator** to the eval suite.

## Optimization and Cost

### Cache key set to the full prompt

**Pitfall**: Caching every full chat message.

**Reality**: Hit rate near zero - every prompt differs. Cache the **prefix** (system + tool definitions); let user input be the variable suffix.

### Batch API for an interactive chat experience

**Pitfall**: Routing chat UX to the AOAI Batch API for the discount.

**Reality**: Users see a **24-hour delay**. Batch is for offline workloads only. Use PAYGO or PTU for interactive.

### Fine-tuning to add knowledge

**Pitfall**: Fine-tuning a model so it "knows the docs."

**Reality**: Fine-tuning teaches **style and format**, not new facts. Use **RAG** for fresh knowledge; fine-tune for tone, structured output, or task-specific behavior.

### No retry policy on 429

**Pitfall**: Surfacing rate-limit errors as user-facing 500s.

**Reality**: Use **exponential backoff + jitter** with deployment failover via APIM (load-balancing across multiple AOAI deployments).

### Single-region PTU with a global user base

**Pitfall**: All PTU in East US, users in EU and APAC.

**Reality**: P99 latency explodes. Front with **APIM or Front Door** in multi-region; deploy PTU close to users or shard traffic by geography.

### Hard-coding the model name in the app

**Pitfall**: `model="gpt-4o-2024-08-06"` baked into source.

**Reality**: Model swaps require a rebuild + redeploy. Reference a **deployment alias / Foundry connection** so model swaps are config changes.

## Looks Like Option A, Is Actually B

| Tempting wrong answer | Right answer | Why |
| --- | --- | --- |
| Fine-tune to add private knowledge | RAG with hybrid + semantic | Fine-tune teaches style, not facts |
| Pure vector retrieval | Hybrid + semantic ranker | BM25 catches keywords vectors miss |
| PTU for spiky low-volume traffic | PAYGO | PTU bills steady; PAYGO bills per call |
| Batch API for interactive chat | PAYGO or PTU | Batch is async with 24h SLA |
| Single AOAI deployment | APIM in front of multiple deployments | Burst handling + spillover |
| Custom safety filter | Azure AI Content Safety + Prompt Shields | Managed, evaluated, classified |
| Account key for AI Search indexer | MSI + Storage RBAC | Identity-based |
| Mirror at 80% | Mirror capped at 50% | Service rejects above 50 |

[<- Master Index](00-MASTER-INDEX.md)
