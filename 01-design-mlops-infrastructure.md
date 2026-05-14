# Domain 1 - Design and Implement MLOps Infrastructure (22%)

> Build the workspace, networking, identity, IaC, and CI/CD foundation that all model lifecycle operations sit on.

---


## Domain mind map

```mermaid
mindmap
  root((Domain 1))
    1.1 Azure ML workspace topology
    1.2 Compute decision tree
    1.3 Networking - secure-by-default workspace
    1.4 Identity & RBAC
    1.5 Infrastructure as Code
    1.6 CI CD pipeline shape
    1.7 Multi-environment promotion
    1.8 Pitfalls
```

## 1.1 Azure ML workspace topology

```mermaid
flowchart LR
    WS[(Azure ML Workspace)]
    WS --> ST[(Storage Account<br/>Blob + File)]
    WS --> KV[(Key Vault)]
    WS --> APPI[(Application Insights)]
    WS --> ACR[(Azure Container Registry)]
    WS -.optional.- FDR[(Microsoft Foundry hub<br/>same RG)]
    WS --> CC[Compute cluster]
    WS --> CI[Compute instance]
    WS --> SVL[Serverless compute]
    WS --> EP[Endpoints]
```

> An **Azure ML workspace** is the top-level container; **Foundry hub** is its GenAI-aware sibling. Many AI-300 scenarios deploy both side-by-side.

---

## 1.2 Compute decision tree

```mermaid
flowchart TD
    Start{What workload?}
    Start -->|Interactive notebook| CI[Compute Instance]
    Start -->|Submitted training job| CC[Compute Cluster<br/>autoscale 0..N]
    Start -->|Job, no infra to manage| SVL[Serverless compute]
    Start -->|Existing K8s footprint| K8S[Azure ML extension on AKS / Arc]
    CC -->|Spot / low-priority OK| SPOT[Use spot tier for cost]
    CC -->|Reserve warm capacity| WARM[min_instances > 0]
```

| Compute | Min nodes | Autoscale | Best for |
|---|---|---|---|
| Compute instance | 1 (always on) | No | Single-user authoring |
| Compute cluster | 0 -> N | Yes | Submitted jobs, sweep, pipelines |
| Serverless | 0 -> N | Yes (managed) | Job-by-job, no cluster mgmt |
| Kubernetes (AKS/Arc) | Per node pool | Per node pool | Hybrid / on-prem / GPU sharing |

---

## 1.3 Networking - secure-by-default workspace

```mermaid
flowchart LR
    subgraph VNET[Customer VNet]
      subgraph H[Hub]
        FW[Azure Firewall<br/>egress allowlist]
        BAS[Bastion]
      end
      subgraph SP[Spoke - ML]
        WPE[PE -> Workspace]
        SPE[PE -> Storage]
        KPE[PE -> Key Vault]
        APE[PE -> ACR]
        IPE[PE -> App Insights]
        CSUB[Compute subnet<br/>VNet-injected cluster]
      end
    end
    USR[Engineer via Bastion / VPN] --> WPE
    WPE --> WS[(Workspace)]
    CSUB --> SPE
    CSUB --> KPE
    CSUB --> APE
    CSUB --> FW
    FW --> INET[Allowed packages / curated registries]
```

Required toggles:

| Setting | Value |
|---|---|
| `public_network_access_enabled` (workspace) | `false` |
| Storage **public network access** | Disabled (selected networks) |
| ACR **SKU** | Premium (private endpoint requires) |
| Compute **VNet injection** | `subnet_id` set |
| Outbound rules | Managed VNet OR Firewall allowlist |

---

## 1.4 Identity & RBAC

```mermaid
flowchart LR
    DEV[Developer]:::user -->|AzureML Data Scientist| WS[(Workspace)]
    OPS[ML Ops]:::user -->|AzureML Compute Operator + Contributor| WS
    SEC[Sec admin]:::user -->|Owner on RG| WS
    CICD[GitHub Actions]:::svc -->|federated cred OIDC| WS
    EP[Online endpoint]:::svc -->|System MSI<br/>Storage Blob Data Reader| ST[(Storage)]
    EP -->|Key Vault Secrets User| KV[(Key Vault)]
    classDef user fill:#dff6dd,stroke:#107c10
    classDef svc fill:#deecf9,stroke:#0f6cbd
```

Built-in roles cheat sheet:

| Role | Scope | Use for |
|---|---|---|
| **AzureML Data Scientist** | Workspace | Submit jobs, register models, deploy endpoints; **cannot** create compute |
| **AzureML Compute Operator** | Workspace / compute | Manage compute clusters & instances |
| **Reader** | RG / sub | View only |
| **Contributor** | RG | Provision via IaC |
| **Owner** | RG / sub | Add role assignments |

---

## 1.5 Infrastructure as Code

```mermaid
flowchart LR
    GIT[git repo<br/>infra/main.bicep] --> WHAT[Bicep what-if]
    WHAT --> APPROVE{PR review}
    APPROVE -->|approved| CD[GitHub Actions deploy]
    CD -->|OIDC| AAD[Microsoft Entra]
    AAD --> AZ[az deployment sub create]
    AZ --> RG[(Resource Group)]
    RG --> WS[(Workspace + AML deps)]
    RG --> FDR[(Foundry hub + project)]
```

Modules to know:

- `Microsoft.MachineLearningServices/workspaces`
- `Microsoft.MachineLearningServices/workspaces/computes`
- `Microsoft.MachineLearningServices/workspaces/onlineEndpoints`
- `Microsoft.CognitiveServices/accounts` (Azure OpenAI / Foundry)
- `Microsoft.MachineLearningServices/workspaces/connections` (datastores, hub->AOAI)

> Prefer **Azure Verified Modules (AVM)** over hand-rolled.

---

## 1.6 CI/CD pipeline shape

```mermaid
flowchart LR
    PR[PR opened] --> LINT[Lint + unit tests]
    LINT --> WHATIF[Bicep what-if + diff]
    WHATIF --> REVIEW{Reviewer approves}
    REVIEW -->|merge to main| INFRA[Deploy infra]
    INFRA --> TRAIN[Trigger training pipeline]
    TRAIN --> REG[Register model<br/>tags: git_sha, dataset_version]
    REG --> DEV_EP[Deploy to dev endpoint]
    DEV_EP --> SMOKE[Smoke + integration tests]
    SMOKE --> APPROVE2{Manual gate}
    APPROVE2 --> PROD_EP[Promote to prod<br/>blue/green]
    PROD_EP --> MON[Enable monitors]
```

GitHub Actions OIDC pieces:

```yaml
# .github/workflows/deploy.yml
permissions:
  id-token: write
  contents: read
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      - run: az ml online-endpoint create -f endpoint.yml
```

> No client secret stored. The federated credential maps `repo:org/repo:ref:refs/heads/main` -> service principal.

---

## 1.7 Multi-environment promotion

```mermaid
flowchart LR
    DEV[(Dev RG / WS)] -->|export model| ART[(Model artifact + signature)]
    ART --> TEST[(Test RG / WS)]
    TEST -->|gated| PROD[(Prod RG / WS)]
    PROD --> CDN[Multi-region endpoints]
```

- **Workspace per env** (dev / test / prod) is the recommended pattern.
- Promote **registered models** (not training code) across environments via cross-workspace references or model export/import.
- IaC + `azd env` or Terraform workspaces handle the per-env wiring.

---

## 1.8 Pitfalls

1. Workspace + storage in **different regions** -> egress cost + latency.
2. **Public workspace** but private compute -> exposes the control plane.
3. Stored client secrets in CI/CD -> use **OIDC**.
4. Owner role on data scientists -> use **AzureML Data Scientist**.
5. ACR Basic SKU -> no private endpoint support.
6. Compute cluster `min_instances > 0` left running -> cost spike.
7. No **diagnostic settings** on workspace -> no audit trail.

---

[<- Master Index](00-MASTER-INDEX.md) - [Domain 2 ->](02-ml-model-lifecycle-and-operations.md)
