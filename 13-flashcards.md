# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas. ~40 cards across 5 domains.

<section class="fc-section" data-fc-title="Design and Implement MLOps Infrastructure">
<h2>1 - Design and Implement MLOps Infrastructure</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Foundry hub vs project?</div><div class="fc-a"><strong>Hub</strong> = shared infra (compute, connections, storage). <strong>Project</strong> = workspace under a hub for one team/app.</div></div>

<div class="flashcard"><div class="fc-q">Foundry observability sink?</div><div class="fc-a"><strong>Application Insights</strong> via project tracing (OpenTelemetry).</div></div>

<div class="flashcard"><div class="fc-q">Role for app calling Azure OpenAI as managed identity?</div><div class="fc-a"><strong>Cognitive Services OpenAI User</strong>.</div></div>

<div class="flashcard"><div class="fc-q">No infra to manage for training jobs?</div><div class="fc-a"><strong>Serverless compute</strong>.</div></div>

<div class="flashcard"><div class="fc-q">GitHub Actions to Azure without secret?</div><div class="fc-a"><strong>OIDC federated credential</strong> on a user-assigned managed identity.</div></div>

<div class="flashcard"><div class="fc-q">Lock down Foundry to private network?</div><div class="fc-a">Private endpoints + VNet-injected compute + <code>publicNetworkAccess=Disabled</code>.</div></div>

<div class="flashcard"><div class="fc-q">SKU for EU-only data residency on AOAI?</div><div class="fc-a"><strong>DataZone Standard</strong> deployment.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="ML Model Lifecycle and Operations">
<h2>2 - ML Model Lifecycle and Operations</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Hyperparameter search job type?</div><div class="fc-a"><strong>Sweep</strong> job.</div></div>

<div class="flashcard"><div class="fc-q">Bayesian sampling supports early stop?</div><div class="fc-a"><strong>No</strong> - must run to completion.</div></div>

<div class="flashcard"><div class="fc-q">Async batch scoring of large folder?</div><div class="fc-a"><strong>Batch endpoint</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Shadow deployment without hitting users?</div><div class="fc-a"><strong>Mirror traffic</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Detect input distribution shift?</div><div class="fc-a"><strong>Data drift monitor</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Compare predictions to ground truth?</div><div class="fc-a"><strong>Model performance monitor</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Track training experiments with autologging?</div><div class="fc-a"><strong>MLflow autolog</strong>.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Design and Implement GenAIOps Infrastructure">
<h2>3 - Design and Implement GenAIOps Infrastructure</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Predictable peak latency on Azure OpenAI?</div><div class="fc-a"><strong>PTU</strong> - provisioned throughput units.</div></div>

<div class="flashcard"><div class="fc-q">50% off bulk LLM workload, async OK?</div><div class="fc-a"><strong>Batch API</strong> on Azure OpenAI.</div></div>

<div class="flashcard"><div class="fc-q">Multi-step reasoning, fine-tune?</div><div class="fc-a"><strong>RFT</strong> (reasoning fine-tuning) on o-series.</div></div>

<div class="flashcard"><div class="fc-q">Align preferences (helpful/harmless)?</div><div class="fc-a"><strong>DPO</strong> fine-tuning.</div></div>

<div class="flashcard"><div class="fc-q">Add up-to-date private knowledge?</div><div class="fc-a"><strong>RAG</strong> - not fine-tuning.</div></div>

<div class="flashcard"><div class="fc-q">Best retrieval combo for RAG?</div><div class="fc-a"><strong>Hybrid + semantic ranker</strong> on Azure AI Search.</div></div>

<div class="flashcard"><div class="fc-q">Foundry primitive for multi-agent?</div><div class="fc-a"><strong>Connected agent</strong> as a tool on the parent agent.</div></div>

<div class="flashcard"><div class="fc-q">Cache repeated chat prompts cheaply?</div><div class="fc-a">AOAI <strong>prompt caching</strong> + APIM <strong>semantic cache</strong>.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="GenAI Quality Assurance and Observability">
<h2>4 - GenAI Quality Assurance and Observability</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Score answer against retrieved context?</div><div class="fc-a"><strong>GroundednessEvaluator</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Block jailbreaks at runtime?</div><div class="fc-a"><strong>Prompt Shields</strong> (Azure AI Content Safety).</div></div>

<div class="flashcard"><div class="fc-q">Synthetic adversarial test before launch?</div><div class="fc-a"><strong>AI Red Teaming Agent</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Sample prod traffic for ongoing eval?</div><div class="fc-a"><strong>Continuous evaluation</strong> in Foundry.</div></div>

<div class="flashcard"><div class="fc-q">Built-in evaluator: copyrighted text?</div><div class="fc-a"><strong>ProtectedMaterialEvaluator</strong>.</div></div>

<div class="flashcard"><div class="fc-q">Evaluator for tool/retrieval injection?</div><div class="fc-a"><strong>IndirectAttackEvaluator</strong>.</div></div>

<div class="flashcard"><div class="fc-q">CI quality-gate failure -> next step?</div><div class="fc-a"><strong>Block deploy</strong>, file regression issue, root-cause before re-running.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Optimize GenAI Systems and Performance">
<h2>5 - Optimize GenAI Systems and Performance</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Reduce token spend on long system prompts?</div><div class="fc-a"><strong>Prompt caching</strong> - system prompt cached after first call.</div></div>

<div class="flashcard"><div class="fc-q">Lower latency for simple intents?</div><div class="fc-a">Route to a <strong>smaller/cheaper model</strong> (gpt-4o-mini, phi) via classifier.</div></div>

<div class="flashcard"><div class="fc-q">Reduce hallucination on RAG answers?</div><div class="fc-a">Tighter chunking, hybrid+semantic retrieval, cite sources, Groundedness eval gate.</div></div>

<div class="flashcard"><div class="fc-q">PTU vs PAYGO - when PTU?</div><div class="fc-a">Steady high-volume traffic with strict P95 latency and predictable cost.</div></div>

<div class="flashcard"><div class="fc-q">Cap per-tenant LLM spend?</div><div class="fc-a">APIM <strong>token-limit policy</strong> per subscription/key.</div></div>

<div class="flashcard"><div class="fc-q">Spillover from PTU to PAYGO?</div><div class="fc-a">APIM <strong>load-balancing / spillover policy</strong> across multiple AOAI deployments.</div></div>

</div>
</section>
