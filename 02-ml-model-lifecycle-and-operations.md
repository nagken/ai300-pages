# Domain 2 - ML Model Lifecycle and Operations (22%)

> Train -> register -> deploy -> monitor -> retrain. The classic MLOps loop, with AI-300-flavored detail on data collector, drift, and gated rollouts.

---


## Domain mind map

```mermaid
mindmap
  root((Domain 2 - ML Model Lifecycle and Operations 22%))
    2.1 Lifecycle overview
    2.2 Job decision tree
    2.3 Sweep - sampling and termination
    2.4 MLflow integration
    2.5 Endpoint decision tree
    2.6 Safe rollout strategies
    2.7 Model monitoring
    2.8 Retraining triggers
    2.9 Pitfalls
```

## 2.1 Lifecycle overview

```mermaid
flowchart LR
    DATA[(Data asset)] --> TRAIN[Pipeline job]
    TRAIN --> MLF[MLflow run]
    MLF --> REG[Model Registry<br/>tags + version]
    REG --> SCAN[RAI dashboard +<br/>safety scan]
    SCAN --> DEV[Dev online endpoint]
    DEV --> TEST[Test endpoint]
    TEST --> PROD[Prod endpoint<br/>blue/green]
    PROD --> COLLECT[Data Collector]
    COLLECT --> MON[Drift + Perf monitors]
    MON -->|alert| TRAIN
```

---

## 2.2 Job decision tree

```mermaid
flowchart TD
    A{Job intent?}
    A -->|Single script| CMD[Command job]
    A -->|HP search| SWP[Sweep job]
    A -->|Multi-step DAG| PIP[Pipeline job]
    A -->|Map over data partitions| PAR[Parallel job]
```

| Type | YAML `type` | Use when |
|---|---|---|
| Command | `command` | One script, one set of args |
| Sweep | `sweep` | Tune HPs over a search space |
| Pipeline | `pipeline` | DAG of components |
| Parallel | `parallel` | Score / process huge datasets |

---

## 2.3 Sweep - sampling and termination

| Sampling | Continuous | Discrete | Early-term OK? |
|---|---|---|---|
| **Grid** | No | Yes | Yes |
| **Random** | Yes | Yes | Yes |
| **Bayesian** | Yes | Yes | **No** (must run to completion) |

| Policy | Behavior |
|---|---|
| **Bandit** | Kill if outside `slack_factor` of best |
| **Median Stopping** | Kill below running median |
| **Truncation Selection** | Drop bottom N% each interval |
| (none - Bayesian) | Let every trial finish |

---

## 2.4 MLflow integration

```mermaid
flowchart LR
    SCRIPT[train.py] --> AUTOLOG[mlflow.autolog]
    SCRIPT --> METRIC[mlflow.log_metric]
    SCRIPT --> MODEL[mlflow.log_model]
    AUTOLOG --> RUN[(Workspace MLflow run)]
    METRIC --> RUN
    MODEL --> ART[(Artifact + signature)]
    ART --> REG[Model Registry]
```

Two model flavors at registration:

- `mlflow_model` - has signature + conda env -> **no-code online deploy** y
- `custom_model` - opaque files -> requires **scoring script + env**

---

## 2.5 Endpoint decision tree

```mermaid
flowchart TD
    Q{Latency tolerance?}
    Q -->|Real-time| REALTIME{Existing K8s footprint?}
    REALTIME -->|No| MAN[Managed online endpoint]
    REALTIME -->|Yes| K8S[K8s online endpoint]
    Q -->|Async / large input| BATCH[Batch endpoint]
```

| Endpoint | Latency | Compute | Auth |
|---|---|---|---|
| Managed online | Low | Microsoft-managed VMs | key, AML token, Microsoft Entra |
| K8s online | Low | Your AKS / Arc | key, token |
| Batch | Async | Compute cluster | key, Microsoft Entra |

---

## 2.6 Safe rollout strategies

```mermaid
flowchart LR
    EP[Endpoint]
    EP --> BLUE[Blue v1<br/>traffic 100]
    EP --> GREEN[Green v2<br/>traffic 0]
    BLUE -. instant swap .-> SWAP[Blue=0, Green=100]
    BLUE -. canary .-> CAN[Green: 5% -> 25% -> 100%]
    BLUE -. mirror .-> MIRROR[Mirror 10% req to Green<br/>response not returned]
```

| Strategy | Risk | Use when |
|---|---|---|
| **Instant swap** | High | Hotfix only |
| **Canary** | Medium | Normal model upgrade |
| **Mirror** | Lowest | Validate against prod traffic without impact (<=50%) |

---

## 2.7 Model monitoring

```mermaid
flowchart LR
    EP[Online endpoint] -->|enabled| DC[Data Collector]
    DC --> BLOB[(Captured payloads)]
    BLOB --> DRIFT[Data drift monitor<br/>vs baseline]
    BLOB --> PERF[Model performance monitor<br/>vs ground-truth labels]
    EP --> APPI[(App Insights<br/>latency / errors)]
    DRIFT --> ALERT[Alert / Action Group]
    PERF --> ALERT
    APPI --> ALERT
    ALERT --> CICD[Trigger retraining pipeline]
```

| Signal | Source | Threshold |
|---|---|---|
| Feature drift | Data collector + baseline dataset | Wasserstein / PSI |
| Prediction drift | Outputs vs baseline | Distribution shift |
| Model performance | Joined ground-truth | Accuracy / RMSE drop |
| Latency / error rate | App Insights | P95 ms / 5xx rate |

---

## 2.8 Retraining triggers

```mermaid
flowchart TD
    T[Retraining triggers]
    T --> S[Schedule (daily/weekly)]
    T --> D[Drift alert]
    T --> P[Performance alert]
    T --> M[Manual re-run]
    T --> E[New data event]
```

> Use **schedule + drift** together. Schedule guarantees freshness; drift catches sudden shifts.

---

## 2.9 Pitfalls

1. Forgot `goal: maximize` on sweep primary metric -> picks worst run.
2. Bayesian sampling + Bandit policy -> invalid combo.
3. `custom_model` registered without scoring script -> deploy fails.
4. Mirror traffic > 50% -> not allowed.
5. Drift monitor without Data Collector enabled -> no data, no signal.
6. Model perf monitor without ground-truth ingestion -> never fires.
7. Online endpoint with fixed instances -> over- or under-provisioned.

---

[<- Domain 1](01-design-mlops-infrastructure.md) - [<- Master Index](00-MASTER-INDEX.md) - [Domain 3 ->](03-design-genaiops-infrastructure.md)
