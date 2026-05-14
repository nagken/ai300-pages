# Hands-On Labs and Sample Repositories

> Curated, executable references for AI-300 topics. All links point to official Microsoft Learn modules, Azure-Samples repositories, or Cloud Adoption Framework / Well-Architected Framework assessments.

## Microsoft Learn - Foundry and OpenAI Labs
- [Get started with Azure OpenAI](https://learn.microsoft.com/training/modules/get-started-openai/)
- [Build a generative AI app with Azure AI Foundry](https://learn.microsoft.com/training/modules/build-generative-ai-app-azure-ai-foundry/)
- [Implement RAG using Azure AI Search](https://learn.microsoft.com/training/modules/use-own-data-azure-openai/)
- [Develop a prompt flow](https://learn.microsoft.com/training/modules/get-started-prompt-flow-ai-studio/)
- [Build an agent with Azure AI Foundry](https://learn.microsoft.com/training/modules/develop-ai-agent-azure/)
- [Evaluate generative AI in Azure AI Foundry](https://learn.microsoft.com/training/modules/evaluate-models-azure-ai-studio/)
- [Trace generative AI applications](https://learn.microsoft.com/training/modules/monitor-generative-ai-app/)
- [Apply Content Safety](https://learn.microsoft.com/training/modules/responsible-generative-ai/)

## GitHub repos to clone & run
- [`MicrosoftLearning/mslearn-aifnd`](https://github.com/MicrosoftLearning/mslearn-aifnd)
- [`Azure/azureml-examples`](https://github.com/Azure/azureml-examples) - `cli/jobs`, `cli/endpoints/online`, `cli/endpoints/batch`
- [`Azure-Samples/azure-search-openai-demo`](https://github.com/Azure-Samples/azure-search-openai-demo)
- [`Azure-Samples/contoso-chat`](https://github.com/Azure-Samples/contoso-chat)
- [`Azure/AI-in-a-Box`](https://github.com/Azure/AI-in-a-Box)

## Exam-tier challenges (do all 10)
1. Provision a Foundry hub + project with Bicep + private endpoints.
2. Deploy `gpt-4o-mini` Standard + Provisioned and configure APIM to round-robin between them.
3. Build a RAG index over PDFs with hybrid search + semantic ranker.
4. Wire AOAI On-Your-Data to the index using MSI auth (no keys).
5. Author a prompt flow with a custom python eval node, run it batch.
6. Create a Foundry agent with `file_search` and a `function_call` tool.
7. Run `azure-ai-evaluation` with Relevance + Groundedness + ContentSafety.
8. Train a sklearn model in AML, register in MLflow, deploy to managed online with blue/green.
9. Enable Data Collector + drift monitor + perf monitor on the endpoint.
10. Set up GitHub Actions with OIDC, deploy a Foundry change with a quality gate (eval threshold).

[<- Master Index](00-MASTER-INDEX.md)
