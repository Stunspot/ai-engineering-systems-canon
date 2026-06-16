# AI-ENG-AH — Build, Buy, Open Source & Vendor Strategy - Doctrinal Knowledge Base for Strategic Dependency Architecture

## **Conceptual Glossary of AI Sourcing Architecture**

To establish a mathematically precise and legally rigorous foundation for enterprise technology acquisition, the vocabulary of artificial intelligence sourcing must be strictly standardized:

* **AI Sourcing Architecture**: The technical and procurement discipline of designing, deploying, and continuously auditing the mix of proprietary interfaces, self-hosted models, open-weight artifacts, and native code layers that comprise an enterprise execution plane. It establishes how an organization balances operational agility against systemic dependency risk.  
* **Build**: The direct engineering and deployment of proprietary software components, custom orchestration layers, fine-tuned model adapters, or specialized base-model architectures on managed or owned infrastructure.  
* **Buy**: The commercial acquisition of fully integrated, vendor-hosted applications where the software vendor defines the user interface, workflow engine, orchestration logic, and underlying model endpoints.  
* **Managed API**: A consumption-based cloud endpoint hosted by a model provider that delivers inference outputs via standard HTTPS requests, charging fees based on token or request volume while withholding access to model weights and parameters.  
* **Vendor Application**: A finished software-as-a-service (SaaS) product that wraps model capabilities within a specific business workflow, assuming operational control over retrieval pipelines, model routing, and output formatting.  
* **Open Source Software (OSS)**: Code distributed under licenses approved by the Open Source Initiative (OSI) that grant the unrestricted rights to use, study, modify, and share the software system.1  
* **Open Weight Model**: An artificial intelligence model whose learned parameters and weights are made available for download, but whose commercial distribution, training modifications, and usage scale are governed by custom, non-OSI-compliant community agreements.3  
* **Source-Available Model**: A machine learning model whose code and parameter architectures are visible for audit and study, but whose licensing explicitly restricts commercial application, redistribution, or competitive derivative creation.3  
* **Self-Hosting**: The operation of open-weight or proprietary models on infrastructure directly leased, owned, or logically isolated by the deploying organization, including virtual private clouds (VPCs) and private hardware clusters.  
* **Local Deployment**: The execution of models on physical, non-cloud edge hardware, local workstations, or closed on-premises data centers, operating entirely decoupled from external network dependencies.  
* **Hybrid Architecture**: An engineering pattern that partitions execution across both proprietary managed endpoints and self-hosted models, controlled by an internally owned gateway layer to balance cost, latency, and compliance constraints.5  
* **Control Plane**: The central architectural layer that governs routing, identity and access management, policy enforcement, semantic caching, rate limiting, and audit logging across all underlying model executions.5  
* **Execution Plane**: The runtime infrastructure where raw inference calculations are executed, whether hosted on a vendor's public cloud API, a private GPU cluster, or a localized edge device.8  
* **Data Rights**: The contractual terms governing who owns, can access, and can use inputs (prompts), outputs (completions), vector embeddings, telemetry logs, and user feedback generated during model operations.9  
* **Lock-In**: The structural, mathematical, and financial coupling of an application to a specific model provider's API, prompt formatting, embedding coordinate geometry, or commercial terms.11  
* **Exit Plan**: A pre-vetted, contractually supported, and technically validated decommissioning protocol that details how an organization can transition a workload away from a vendor without service interruption, data loss, or degradation of system behavior.10  
* **Real Total Cost of Ownership (TCO)**: The comprehensive, fully loaded financial calculation of all capital and operational expenditures associated with an AI system throughout its lifecycle, extending far beyond raw infrastructure or subscription pricing.14

## **AI Sourcing Strategy Doctrine**

The core tenet of modern enterprise system design is that sourcing decisions must not be driven by model leaderboard rankings, developer advocacy, or temporary demonstration quality. Sourcing strategy is a discipline of strategic dependency design. The primary objective is to optimize for capability, control, cost, compliance, security, portability, and long-term lifecycle viability. The modern enterprise must operate under a foundational mandate: **own the control plane; vary the execution plane**.5

```
                  ┌──────────────────────────────────────────────┐  
                  │          ENTERPRISE CONTROL PLANE            │  
                  │  (SSO/RBAC, CEL Routing, Semantic Cache,     │  
                  │   Audit Logging, Policy Gates, Evals)        │  
                  └──────────────────────┬───────────────────────┘  
                                         │  
                 ┌───────────────────────┼───────────────────────┐  
                 ▼                       ▼                       ▼  
     ┌───────────────────────┐ ┌───────────────────┐ ┌───────────────────────┐  
     │    PROPRIETARY API    │ │  PRIVATE CLOUD    │ │     LOCAL / ON-PREM   │  
     │   (Managed Frontier)  │ │ (Open/Self-Host)  │ │   (Sensitive/Edge)    │  
     └───────────────────────┘ └───────────────────┘ └───────────────────────┘  
     └───────────────────────────────────┬───────────────────────────────────┘  
                                         ▼  
                             ┌───────────────────────┐  
                             │    EXECUTION PLANE    │  
                             └───────────────────────┘
```

An organization cedes strategic viability when it allows its core business logic, proprietary data representations, and user feedback systems to become trapped inside vendor-owned wrappers. The strategic value of artificial intelligence does not reside in the commodity intelligence rented from frontier foundation model providers; it resides in the proprietary data corpus, the domain-specific evaluation metrics, the workflow integration, and the accumulated user interaction loops.  
Consequently, enterprise architects must construct an internal control plane that abstracts away provider-specific dependencies.12 By intercepting all traffic through an internally managed gateway, the enterprise can dynamically direct workloads to the cheapest, fastest, or most compliant execution environment without altering application code.12  
Workloads must be ruthlessly classified. Generic reasoning tasks can be rented from managed APIs, provided robust data boundaries and zero-data-retention terms are contractually guaranteed.9 Conversely, core product differentiators, regulated data pipelines, and high-frequency automated workflows must utilize self-hosted open-weight models, private clouds, or hybrid routing to preserve operational autonomy, ensure cost predictability at scale, and eliminate the existential risk of vendor deprecation.16  
This approach inherits critical constraints from surrounding engineering disciplines:

* **Product-Fit Alignment**: Sourcing choices must directly map to real, validated user workflows and acceptable latency bounds, rejecting complex multi-billion parameter configurations if an optimized small local model satisfies the use case.6  
* **Adoption Compatibility**: Vendor selections must integrate cleanly with corporate training infrastructures, ensuring model behaviors are explainable, supportable, and aligned with real feedback loops.9  
* **Environmental Sustainability**: Sourcing decisions must calculate hardware utilization, power consumption, and carbon impact, balancing local GPU operations against the efficiency gains of centralized cloud architectures.16  
* **Supply Chain Resilience**: Downloaded models, libraries, and containers must undergo rigorous Software Bill of Materials (SBOM) audits to verify provenance, validate cryptographic trust, and eliminate unpatched vulnerability vectors.24

## **Build, Buy, Open, and Hybrid Decision Matrix**

AI sourcing strategy is not a binary build-versus-buy question. Enterprises must choose among finished vendor applications, managed APIs, hosted open models, self-hosted models, open-source stack assembly, in-house build, and hybrid control-plane architectures. The right answer depends on workflow differentiation, data sensitivity, latency, cost profile, internal engineering capacity, compliance exposure, portability needs, and exit risk.

| Sourcing Category | Best-Fit Conditions | Primary Advantages | Primary Risks | Governance Requirements | Cost Shape | Operational Burden | Exit Complexity |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Buy Vendor Application** | Commodity workflow; limited internal engineering capacity; speed-to-market matters more than deep customization. | Fast deployment, managed UX, vendor support, reduced platform burden. | Workflow lock-in, opaque model behavior, subprocessor cascade, limited eval visibility, roadmap dependence. | Vendor risk review, DPA, subprocessor tracking, export rights, audit logging, admin controls. | Seat/license fees plus possible usage tiers. | Low internally, but vendor-management burden remains. | Low to extreme depending on data export, API access, workflow depth, and user retraining cost. |
| **Managed API** | Need frontier capability, variable/spiky demand, rapid experimentation, or multimodal functionality. | Strong capability, fast integration, no hardware operations, elastic scaling. | Provider drift, rate limits, deprecation, data boundary concerns, price changes. | Gateway abstraction, version/eval controls, quota limits, data processing terms, fallback route. | Usage-based; can scale sharply with volume/context. | Medium: integration, gateway, eval, retry, and monitoring required. | Moderate if gateway and eval abstraction exist; high if provider SDKs/prompts are hardcoded. |
| **Hosted Open Model** | Need more control than proprietary API without full self-hosting burden. | More model transparency, potentially better data isolation, simpler operations than self-host. | Hosting-provider lock-in, runtime differences, limited hardware control, unclear support boundaries. | Model/license review, hosting DPA, telemetry access, portability test. | Capacity-based or token-based. | Low-to-medium. | Moderate; portable if weights, adapters, configs, prompts, and evals are exportable. |
| **Self-Hosted Private Model** | Regulated data, predictable high volume, sovereign/private deployment, low-latency internal workflows. | Strong control over data, routing, telemetry, and deployment lifecycle. | Hardware/VRAM limits, talent needs, reliability burden, patching, utilization risk. | AI-BOM/SBOM, model provenance, security review, eval gates, access controls, operational runbooks. | Fixed capacity plus staffing, power, cooling, and lifecycle costs. | High. | Low for weights, higher for serving stack, adapters, quantization format, and operational knowledge. |
| **Open-Source Stack Assembly** | Strong platform engineering team; need custom orchestration, RAG, evals, gateways, or tool execution. | High flexibility, lower vendor coupling, internal control. | Dependency sprawl, license compliance, supply-chain vulnerabilities, upgrade burden. | SBOM, license scanning, dependency patching, maintainer health review. | Engineering payroll dominates. | High. | Moderate; open components reduce lock-in but skills and integration choices still bind. |
| **In-House Build** | Core differentiating capability; proprietary data/feedback loop; no vendor product fits. | Maximum strategic control and differentiation. | Scope creep, long timelines, high staffing cost, eval failures, maintenance burden. | Full lifecycle governance, model/data lineage, eval program, security and compliance review. | High to extreme. | Extreme. | Low for ownership; high for internal maintenance obligations. |
| **Hybrid Architecture** | Mixed sensitivity, variable workload classes, need cost optimization and resilience. | Best balance of control, flexibility, and execution-plane diversity when done well. | Orchestration complexity, split debugging, inconsistent behavior across routes. | Owned control plane, policy-as-code, route manifests, eval-by-route, fallback tests. | Potentially optimized, but not automatically cheaper. | High. | Low if abstraction is real; high if “hybrid” is only informal provider sprawl. |

### **Decision Rule**

The preferred architecture is the lowest-complexity sourcing posture that satisfies:

```text
workflow fit
+ data rights
+ risk tier
+ cost-to-serve
+ operational readiness
+ portability
+ strategic differentiation
```

Do not self-host to look sophisticated. Do not buy a vendor app that captures the core workflow. Do not use managed APIs without an exit route. The goal is controlled optionality, not ideological purity.

## **AI Stack Ownership Map**

An AI system is a layered dependency graph. Each layer should be classified by the degree of strategic control required. Not every layer must be literally owned, but every critical layer must be controlled through architecture, contract, abstraction, export rights, or operational policy.

```text
AI STACK CONTROL MAP

[ User Interface / Workflow Surface ]
        |
[ Prompts / Context / Product Logic ]
        |
[ Evals / Rubrics / Quality Gates ]
        |
[ Internal Gateway / Routing / Policy Plane ]
        |
[ Retrieval Corpus / Embeddings / Indexes ]
        |
[ Tool and Action Execution Layer ]
        |
[ Telemetry / Feedback / Audit Evidence ]
        |
[ Model Providers / Weights / Serving Runtime ]
        |
[ Compute Infrastructure / Hosting Region ]
```

### **Control States**

| Control State | Meaning |
| :---- | :---- |
| **Own** | Enterprise directly controls source, data, configuration, or artifact. |
| **Control** | Enterprise may not own the layer, but governs access, policy, export, and behavior. |
| **Abstract** | Enterprise hides implementation behind a stable internal interface. |
| **Rent** | Enterprise consumes external capability while preserving portability. |
| **Escrow / Export** | Enterprise requires contractual recovery, migration, or extraction rights. |
| **Avoid** | Layer creates unacceptable lock-in, data exposure, legal, or operational risk. |

### **Layer Ownership and Control Requirements**

| AI Stack Layer | Target Control State | Rationale | Minimum Control Requirement |
| :---- | :---- | :---- | :---- |
| **User Interface / Workflow Surface** | Own or strongly control. | The workflow surface captures user habit, trust, corrections, and process structure. | Exportable workflow data, admin controls, UX authority, role-specific configuration. |
| **Prompts / Context Compiler** | Own. | Prompts encode domain logic and task policy. | Version-controlled prompt library, route-specific prompt tests, migration path across models. |
| **Model Route and Provider Selection** | Abstract. | Provider choice should not be hardcoded into applications. | Internal gateway, route aliases, model manifests, fallback configuration. |
| **Base Model Weights** | Rent, swap, or own depending on use case. | Weights are powerful but not always strategic. | License review, eval baseline, portability assessment, version tracking. |
| **Fine-Tunes / Adapters** | Own or export. | Adapters may encode proprietary behavior and training investment. | Contractual export rights, lineage, training-data record, reproducibility notes. |
| **Retrieval Corpus / Knowledge Base** | Own. | Proprietary knowledge is usually more strategic than the base model. | Source ownership, access controls, lineage, retention policy, source-of-record hierarchy. |
| **Embeddings / Vector Indexes** | Own or preserve migration path. | Embedding geometry can create representation lock-in. | Embedding model registry, reindex plan, raw source preservation, index export. |
| **Rerankers / Search Components** | Abstract or own. | Search quality matters, but components should be swappable. | Stable retrieval interface, eval set, ranking metrics, fallback search. |
| **Tool and Action Execution Layer** | Own/control. | This is the system boundary where AI can cause side effects. | Internal authorization, idempotency, action verification, sandboxing, audit logs. |
| **Policy Engine / Access Control** | Own/control. | Policy cannot be delegated blindly to vendors. | RBAC/ABAC, tenant boundaries, policy-as-code, pre-egress controls. |
| **Evaluation Sets and Rubrics** | Own. | Evals define whether models work for the enterprise’s task. | Proprietary golden sets, evaluation harness, model/provider comparison history. |
| **Telemetry and Observability** | Own/control. | Multi-provider systems require independent visibility. | Internal traces, normalized metrics, retention policy, provider-independent dashboards. |
| **Feedback and Correction Data** | Own/control. | User corrections improve workflow and create proprietary learning loops. | Local capture, no-training default unless approved, export rights, privacy controls. |
| **Audit Trails and Evidence** | Own/control with retention policy. | Compliance evidence must survive vendor changes. | Secure references, evidence package, retention/deletion rules, audit access. |
| **Deployment Infrastructure** | Rent or own. | Compute can be rented if workloads remain portable. | Containerization, IaC, region controls, fallback environment, capacity plan. |
| **Support and Adoption Materials** | Own. | Adoption depends on local workflows and roles. | Internal playbooks, training records, champion feedback, workflow-specific examples. |

The durable rule is: own what differentiates you, control what can harm you, abstract what changes quickly, and rent what is commodity.

## **Open Source, Open Weight, and License Model**

A downloadable model is not automatically open source. AI sourcing must distinguish open-source software, open-source AI systems, permissive open-weight models, restricted open-weight models, research-only models, source-available systems, and hosted proprietary APIs.

The term “open source” should be used carefully. Under the Open Source AI Definition, an Open Source AI system must provide freedoms to use, study, modify, and share, and must make available the preferred forms needed to modify the system, including sufficient data information, code, and parameters.

```text
MODEL / AI ASSET LICENSING FLOW

[ AI Asset ]
      |
      v
[ Does it grant rights to use, study, modify, and share? ]
      |
      +-- no --> [ Proprietary / Source-Available / Restricted ]
      |
      v
[ Are required modification artifacts available? ]
  data information | code | parameters
      |
      +-- no --> [ Open Weights or Source-Available, not full Open Source AI ]
      |
      v
[ Are usage, field, scale, or competition restrictions absent? ]
      |
      +-- no --> [ Restricted Open Weights / Custom License ]
      |
      v
[ Open Source AI Candidate ]
```

### **License and Asset Taxonomy**

| Asset Class | Typical Artifacts Available | Commercial Rights | Strategic Risk | Procurement Treatment |
| :---- | :---- | :---- | :---- | :---- |
| **Open Source Software** | Source code under OSI-approved license. | Usually broad commercial use, depending on license. | Standard OSS dependency risk. | SBOM, license scan, vulnerability management. |
| **Open Source AI System** | Data information, training/inference code, parameters, and rights to use/study/modify/share. | Broad rights if definition and license obligations are met. | Capability, provenance, data documentation, maintenance quality. | Legal review plus AI-BOM/model card/provenance review. |
| **Permissive Open-Weight Model** | Weights and sometimes inference/training code under permissive terms. | Often broad commercial use. | May not satisfy full Open Source AI definition if training data information or code is incomplete. | License review, provenance review, model-risk evaluation. |
| **Restricted Open-Weight Model** | Weights available under custom community or acceptable-use license. | Commercial use may be allowed but limited by scale, field, competition, or redistribution terms. | Hidden restrictions, acquisition diligence risk, migration limits. | Legal approval required before production. |
| **Research / Non-Commercial Model** | Weights/code available for evaluation or academic use. | Commercial production prohibited or unclear. | Accidental production misuse. | Sandbox only unless commercial license obtained. |
| **Source-Available System** | Code or architecture visible but rights restricted. | Limited by license. | Misclassified as open source; legal exposure. | Treat as proprietary for procurement purposes. |
| **Hosted Proprietary API** | No weights; behavior exposed through service endpoint. | Governed by contract and terms of service. | Provider lock-in, data rights, drift, deprecation. | Contract, DPA, SLA, data-use review, exit plan. |

### **Required License Review Questions**

| Review Question | Why It Matters |
| :---- | :---- |
| Does the license allow commercial use? | Prevents accidental non-commercial use in production. |
| Are there scale thresholds or revenue/user caps? | Large enterprises may trip thresholds through affiliates or parent-company aggregation. |
| Are there field-of-use restrictions? | Some licenses ban particular industries, products, or competitive uses. |
| Are outputs restricted? | Output-use limits can block synthetic data, distillation, or migration strategies. |
| Are derivative models allowed? | Fine-tuning, adapters, merges, and distillation may be restricted. |
| Is redistribution allowed? | Determines whether the model can be packaged in products or shipped to customers. |
| Is there an explicit patent grant? | Important for enterprise legal risk, especially for code and model artifacts. |
| Are training data details available? | Required for many provenance, risk, and “open source AI” claims. |
| Are acceptable-use policies incorporated by reference? | Policy changes may alter usable scope over time. |
| Does the license survive acquisition, affiliate use, and subcontractor deployment? | Enterprise usage rarely maps cleanly to one legal entity. |

### **Procurement Rule**

Do not ask, “Is the model open?” Ask:

```text
What exact artifact is available?
Under what exact license?
For what exact use?
At what scale?
With what derivative rights?
With what output rights?
With what provenance?
With what exit path?
```

The answer should be recorded in the AI inventory before production use.

## **Managed API Evaluation Model**

Managed APIs provide rapid access to high-capability models without operating the underlying infrastructure. They are often the correct sourcing choice for frontier reasoning, variable demand, multimodal workloads, and fast experimentation. They also introduce dependency, drift, data, availability, cost, and portability risks.

### **Managed API Risk and Control Matrix**

| Evaluation Vector | Risk | Required Control | Risk-Tier Notes |
| :---- | :---- | :---- | :---- |
| **Model Drift** | Provider updates can change output quality, formatting, latency, or safety behavior. | Private eval sets, route-specific regression tests, change monitoring. | Critical workflows may require scheduled evals; low-risk workflows may use periodic sampling. |
| **Endpoint Deprecation** | Model versions may be retired or renamed. | Internal route aliases, migration calendar, fallback model, deprecation notice tracking. | Production routes should never depend on undocumented endpoints. |
| **Version Pinning** | Auto-updating endpoints can silently alter behavior. | Pin model versions where supported; otherwise eval-gate provider changes. | If pinning is unavailable, increase monitoring and fallback readiness. |
| **Rate Limits and Throttling** | Bursts can create user-visible failures or agent stalls. | Gateway quotas, token budgets, backoff queues, circuit breakers. | High-volume workflows need capacity reservation or multi-provider fallback. |
| **Availability and SLA** | Provider outage can interrupt business workflow. | SLA review, status monitoring, fallback route, degraded mode. | SLA requirements should match workflow criticality. |
| **Data Residency** | Requests may be processed or stored in disallowed regions. | Region controls, DPA review, geo-aware routing, approved provider list. | Regulated workflows require explicit evidence. |
| **Data Use and Retention** | Prompts, outputs, files, logs, or telemetry may be retained or used in ways the enterprise does not permit. | Contractual no-training terms, retention limits, enterprise configuration, audit rights. | Consumer tools should not be assumed safe for enterprise data. |
| **Cost Volatility** | Pricing changes, long contexts, retries, and agents can inflate spend. | Cost telemetry, budget caps, route optimization, semantic/prefix caching where safe. | Agentic workflows need hard budget and loop limits. |
| **Provider-Specific Behavior** | Prompts and outputs become tuned to one model’s quirks. | Multi-model evals, prompt compiler, response schemas, portability tests. | Critical workflows should run migration drills. |
| **Security Boundary** | API keys, files, tool outputs, or PII can leak through integration paths. | Key vaulting, secret rotation, DLP, request filtering, tenant isolation. | Central gateway strongly preferred. |

### **Gateway Abstraction Imperative**

Application code should call an internal AI gateway rather than directly hardcoding provider SDKs and endpoints.

```text
[ Application ]
      |
      v
[ Enterprise AI Gateway ]
  identity | quota | policy | routing | logging | cache | eval hooks
      |
      +--> Provider A
      +--> Provider B
      +--> Hosted open model
      +--> Self-hosted model
      +--> Degraded deterministic route
```

### **Gateway Responsibilities**

| Responsibility | Purpose |
| :---- | :---- |
| **Credential Management** | Centralize provider keys, rotate secrets, and prevent developer key sprawl. |
| **Policy Enforcement** | Apply data, tenant, risk, and tool-use policy before requests leave enterprise control. |
| **Quota and Budget Control** | Enforce usage limits by tenant, app, user, route, and workflow. |
| **Routing and Fallback** | Move traffic across providers or local models without application rewrites. |
| **Version and Route Registry** | Record which model/provider/config served each request. |
| **Caching Controls** | Use prefix or semantic caching only with tenant, freshness, permission, and policy scope. |
| **Telemetry Normalization** | Normalize latency, token, cost, error, and quality signals across providers. |
| **Evaluation Hooks** | Run sampled or scheduled evals by route and risk tier. |
| **Degraded Modes** | Route to deterministic, cached, local, or human-review fallback when providers fail. |

Managed APIs are rented capability. The enterprise control plane is what prevents rented capability from becoming strategic captivity.

## **Vendor Application Evaluation Model**

Buying a finished AI vendor application can be the right decision when the workflow is commodity, the vendor product fits the user surface, internal engineering capacity is limited, and speed-to-market matters. The risk is that the vendor may capture the workflow, user feedback, data flows, evaluation surface, and operational dependency.

```text
VENDOR APPLICATION DEPENDENCY CHAIN

[ Enterprise Buyer ]
        |
        v
[ Vendor Application / SaaS UI ]
        |
        +--> model provider
        +--> cloud host
        +--> analytics provider
        +--> support tooling
        +--> subprocessors
        +--> data stores
```

### **Vendor Application Evaluation Matrix**

| Evaluation Area | Risk Question | Required Evidence / Control |
| :---- | :---- | :---- |
| **Workflow Fit** | Does the vendor product fit the actual workflow, or will users create side channels? | Workflow test, pilot feedback, admin configurability, role-based UX review. |
| **Data Ownership** | Who owns prompts, outputs, files, embeddings, corrections, and feedback? | Contractual data ownership clause and export rights. |
| **Subprocessor Chain** | Which downstream providers touch data or model execution? | Current subprocessor list, notification terms, DPA, data-flow diagram. |
| **Model Transparency** | What models, providers, and routing logic are used? | Model/provider disclosure appropriate to risk tier, change-notice terms. |
| **Admin and Identity Controls** | Can the enterprise manage users, roles, groups, and access centrally? | SSO/OIDC/SAML, SCIM, RBAC/ABAC, audit logs. |
| **Data Protection** | What is retained, where, for how long, and for what purpose? | Retention policy, no-training terms, region controls, deletion terms. |
| **Evaluation and Auditability** | Can the enterprise independently verify quality and risk? | Exportable logs, eval access, audit evidence, incident reports. |
| **Integration Depth** | Does the vendor integrate with systems of record without brittle copying? | API coverage, webhooks, event logs, transaction boundaries. |
| **Operational Stability** | What happens if the vendor changes models, pricing, roadmap, or support? | SLA, deprecation notice, change notice, financial health review. |
| **Exit Path** | Can the enterprise leave without losing data, workflow continuity, or audit evidence? | Export format, termination assistance, deletion certificate, migration plan. |

### **Wrapper Risk**

A vendor application may be a thin workflow layer over a foundation model API. This is not automatically bad, but the buyer must understand what value the vendor actually provides.

| Vendor Value Layer | Good Sign | Warning Sign |
| :---- | :---- | :---- |
| **Workflow Design** | Vendor deeply understands the business process. | Generic chat UI over customer data. |
| **Data Integration** | Clean connectors, permissions, lineage, and source sync. | Manual uploads and unclear access boundaries. |
| **Evaluation** | Vendor can show task-specific quality evidence. | Only demos and generic benchmark claims. |
| **Governance** | Admin controls, audit trails, retention, and policy support. | “Trust us” controls with no evidence export. |
| **Model Operations** | Provider changes are tested and communicated. | Silent model swaps and no version visibility. |
| **Support** | Clear incident path and customer success process. | No meaningful escalation beyond generic support. |

### **Federal Procurement Planning Note**

Some federal procurement environments may impose special AI disclosure, data-handling, domestic-sourcing, or use-rights requirements. Proposed GSA AI terms in 2026 illustrate the direction of travel: disclosure of AI systems used in contract performance, downstream provider visibility, data segregation, and special government-use rights may become relevant for contractors.

Treat these requirements as jurisdiction- and contract-specific. Do not generalize draft federal procurement terms into universal enterprise doctrine. For government-related work, legal and contracts teams must verify the current clause status before procurement approval.

### **Vendor Application Decision Rule**

Buy the application when:

```text
workflow is commodity
+ vendor fit is strong
+ data rights are clear
+ exit is tolerable
+ integration burden is lower than building
+ the vendor’s roadmap risk is acceptable
```

Do not buy when the vendor would own the enterprise’s strategic workflow, feedback loop, evaluation surface, or customer relationship.

## **Open Model and Self-Hosting Strategy**

Self-hosting open-weight or proprietary models can improve data control, cost predictability, latency, and sovereign deployment posture. It also transfers operational responsibility to the enterprise. The organization becomes responsible for serving reliability, GPU capacity, memory pressure, patching, model upgrades, telemetry, security, evaluation, and incident response.

### **Self-Hosting Decision Drivers**

| Driver | Self-Hosting Is Attractive When | Warning |
| :---- | :---- | :---- |
| **Data Sensitivity** | Data cannot leave a private environment or approved region. | Self-hosting does not automatically solve access control or logging risk. |
| **Volume Predictability** | Workload is high-volume and steady enough to utilize reserved capacity. | Low utilization destroys the economics. |
| **Latency** | Local/private inference meaningfully improves user experience or operational SLOs. | Model size and batching may still dominate latency. |
| **Cost Control** | Fixed capacity beats variable token pricing at expected utilization. | Staffing and lifecycle costs must be included. |
| **Portability** | Organization needs model and route independence. | Quantization/runtime choices can create new lock-in. |
| **Regulatory / Sovereign Need** | Contract or law requires controlled environment. | Governance burden usually rises, not falls. |

### **VRAM and Memory Planning**

VRAM is often the binding constraint in self-hosted inference. A simple parameter-memory heuristic is useful for early planning, but production sizing must include KV cache, batch size, context length, precision, runtime overhead, parallelism, and fragmentation.

```text
Approximate model memory:
  model_weight_memory
+ KV_cache_memory
+ runtime_overhead
+ batch_overhead
+ safety_margin
<= available_accelerator_memory
```

### **Parameter Memory Heuristic**

| Precision / Quantization | Approximate Weight Memory Per Billion Parameters | Notes |
| :---- | :---- | :---- |
| **FP16 / BF16** | ~2 GB per 1B parameters. | Higher quality and compatibility; expensive memory footprint. |
| **INT8 / 8-bit** | ~1 GB per 1B parameters. | Common compression point. |
| **4-bit** | ~0.5 GB per 1B parameters. | Useful heuristic; actual memory varies by quantization method and runtime. |
| **Lower than 4-bit** | Less than ~0.5 GB per 1B parameters. | Requires careful quality and compatibility testing. |

### **KV Cache Scaling**

The KV cache scales with:

```text
context length
× batch size
× number of layers
× hidden dimensions / attention heads
× cache precision
```

Longer context windows and larger batches can exhaust VRAM even when model weights fit. Increasing context is not free. It can reduce throughput, increase memory pressure, and force smaller batch sizes. For many enterprise workflows, better retrieval, chunking, reranking, and context compression are more efficient than simply expanding the active context window.

### **Common Self-Hosting Failure Modes**

| Failure Mode | Cause | Mitigation |
| :---- | :---- | :---- |
| **Model fits alone but fails under traffic** | KV cache and batch overhead were ignored. | Load test with realistic context lengths and concurrency. |
| **CPU offload / fallback collapse** | VRAM exceeded; runtime spills to system memory. | Reduce model size, precision, context, batch, or use larger GPU pool. |
| **Low utilization economics** | Reserved GPUs sit idle. | Consolidate routes, batch jobs, autoscale, or use managed API for spiky demand. |
| **Quality regression after quantization** | Compression harms target task behavior. | Run task-specific evals for every precision tier. |
| **Serving runtime lock-in** | Runtime-specific config, kernels, or formats become hard to migrate. | Document runtime, quantization format, adapter format, and migration tests. |
| **Patch and driver drift** | CUDA, drivers, runtime, and dependencies diverge. | Maintain reproducible images, SBOM, patch cadence, and rollback images. |

Self-hosting is a control strategy, not a virtue signal. It should be chosen when the organization can operate the system better than it can rent it.

## **Local Deployment Readiness Model**

Local, on-premises, private-cloud, or edge deployment requires more than purchasing GPUs. The organization must be ready to operate hardware, serving runtimes, security controls, telemetry, model lifecycle, incident response, and user support.

### **Readiness Tiers**

| Readiness Tier | Description | Allowed Use |
| :---- | :---- | :---- |
| **Tier 0: Not Ready** | No clear owner, hardware plan, security model, evals, or operations path. | No deployment. |
| **Tier 1: Prototype Only** | Small team can run local models experimentally, but no production SLO or governance. | Offline experiments with non-sensitive or approved test data. |
| **Tier 2: Pilot Ready** | Serving runtime, access controls, evals, telemetry, and support path exist for bounded scope. | Limited pilot with explicit rollback and human oversight. |
| **Tier 3: Production Ready** | Capacity, reliability, security, evals, monitoring, incident response, and lifecycle management are in place. | Production deployment for approved risk tier. |
| **Tier 4: Regulated / Sovereign Ready** | Strong isolation, audit evidence, data residency, supply-chain controls, and compliance operating model exist. | Regulated, high-sensitivity, sovereign, or air-gapped workflows where appropriate. |

### **Readiness Assessment**

| Dimension | Prototype Only | Pilot Ready | Production Ready | Regulated / Sovereign Ready |
| :---- | :---- | :---- | :---- | :---- |
| **Model Serving Expertise** | Can run model manually. | Can deploy serving runtime with basic monitoring. | Can operate runtime with batching, autoscaling, rollback, and SLOs. | Can validate runtime in isolated or compliant environment. |
| **Hardware / Capacity** | Single workstation or ad-hoc GPU. | Known capacity for pilot workload. | Capacity plan, utilization targets, failover, lifecycle plan. | Dedicated or controlled infrastructure with audit evidence. |
| **Power / Cooling / Facility** | Informal. | Verified pilot environment. | Documented power, cooling, thermal, and maintenance plan. | Facility controls meet regulatory/security needs. |
| **Security** | Local access only. | SSO or controlled access for pilot users. | RBAC/ABAC, secrets, network controls, tenant isolation. | Strong isolation, evidence logging, vulnerability management. |
| **Model Provenance** | Informal download. | Hashes, source, license checked. | AI-BOM/model registry, signed artifacts where available. | Formal supply-chain review and approval. |
| **Evaluation** | Manual spot checks. | Seed eval set. | Regression suite by route/task/risk tier. | Auditable eval evidence and review cadence. |
| **Telemetry** | Logs only. | Basic latency/error/cost traces. | Full operational, quality, utilization, and incident telemetry. | Tamper-resistant evidence appropriate to risk. |
| **Operations** | Best effort. | Named owner and pilot runbook. | On-call path, incident response, rollback, patch cadence. | Formal operating procedures and audit trail. |
| **Support and Training** | Expert-only usage. | Pilot support channel. | User onboarding, support desk, escalation path. | Role-based training and access gating. |

### **Consumer GPU and Commodity Hardware Note**

Consumer or prosumer GPUs can be useful for prototyping, labs, edge cases, and cost-sensitive internal workflows. They may also introduce warranty, availability, driver, cooling, power, memory, and support limitations. Use them deliberately, not because the spreadsheet looked heroic before anyone added operations.


## **Hybrid Architecture Pattern Library**

Hybrid AI architectures combine owned control surfaces with multiple execution environments. Their purpose is not complexity for its own sake. A hybrid pattern is justified when it improves cost, privacy, reliability, latency, compliance, or capability while preserving a coherent control plane.

| Pattern | Routing Logic | Best Fit | Core Control Plane Requirement | Primary Risk | Exit Implication |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Owned Control Plane, Rented Model Plane** | Internal UI/gateway/policy routes to managed APIs. | Fast delivery with strong internal product ownership. | Gateway, route registry, evals, quota, data policy. | Provider drift/deprecation. | Good if provider abstraction is real. |
| **Sensitive-Local / General-Cloud Split** | Sensitive data stays private; non-sensitive complex tasks use cloud. | Mixed data sensitivity and capability needs. | Data classifier, policy gate, route audit. | Misclassification can leak sensitive data. | Strong if local route can operate independently. |
| **Small-Local-First Cascade** | Start with small/private model; escalate when confidence/eval fails. | High-volume bounded tasks with occasional complex cases. | Confidence/eval gate, escalation policy, cost telemetry. | Bad confidence scores create poor routing. | Good if prompts/evals work across routes. |
| **Open Baseline with Proprietary Escalation** | Open/self-hosted model handles standard work; frontier API handles hard cases. | Cost control plus occasional high capability. | Route manifest, model comparison evals, fallback logic. | Behavior inconsistency across model classes. | Strong baseline fallback if proprietary route fails. |
| **Vendor App with Owned Data/Eval Layer** | SaaS app used for workflow; enterprise owns exports, evals, and correction data. | Commodity workflow with strategic data retention needs. | Export contracts, audit logs, internal eval mirror. | Vendor UI/workflow lock-in. | Moderate if data and correction history are portable. |
| **Commodity SaaS plus Internal Gateway** | Vendor app or API must pass through enterprise policy/logging layer. | Teams need SaaS speed but governance cannot be delegated. | Proxy/gateway integration, DLP, identity, audit capture. | Vendor API changes or unsupported interception. | Better if vendor supports official APIs/webhooks. |
| **Private Inference for Regulated Workflows** | Approved workloads route only to isolated private environment. | Regulated, sovereign, confidential, or air-gapped workflows. | Strong identity, audit, model registry, operational runbooks. | High cost and operational burden. | Strong if weights, configs, and data are owned. |
| **Multi-Provider Resilience Route** | Gateway routes across multiple providers based on health, cost, latency, and eval. | Availability-sensitive and provider-risk-sensitive workloads. | Provider abstraction, health checks, eval parity, fallback policy. | Lowest-common-denominator API design. | Strong if route-specific quality is validated. |
| **Batch-Private / Interactive-Managed Split** | Offline jobs use self-hosted batch capacity; interactive tasks use managed API. | Heavy batch workloads plus variable user-facing demand. | Scheduler, workload classifier, cost/carbon telemetry. | Queue delays and inconsistent output profile. | Good if artifacts and evals are shared. |

### **Hybrid Architecture Rule**

Hybrid systems need a single owned policy, telemetry, evaluation, and routing story. Without that, “hybrid” means vendor sprawl wearing a fake mustache.

## **Vendor Selection Scorecard**

AI vendors should be evaluated by risk tier, not by a single universal score threshold. A low-risk internal productivity tool, a customer-facing support agent, and a regulated decision-support workflow do not require identical evidence. They do require a consistent review structure.

### **Scoring Dimensions**

| Dimension | Review Question | Evidence Examples |
| :---- | :---- | :---- |
| **Capability Fit** | Does the vendor solve the actual workflow better than alternatives? | Pilot results, proprietary evals, latency tests, user acceptance data. |
| **Security Posture** | Can the vendor protect data, identities, tenants, and infrastructure? | SOC 2 Type II or equivalent, ISO 27001, pen test summary, vulnerability process. |
| **Data Rights and Retention** | Are prompts, outputs, files, embeddings, telemetry, and feedback protected? | DPA, no-training terms, retention schedule, deletion terms. |
| **Subprocessor Transparency** | Are downstream providers known and controlled? | Subprocessor list, notification terms, data-flow diagram. |
| **Governance and Compliance** | Can the vendor support the system’s regulatory and policy obligations? | Risk classification, audit logs, model documentation, region controls. |
| **Integration Depth** | Does it support enterprise identity, admin, provisioning, and APIs? | SSO/OIDC/SAML, SCIM, RBAC/ABAC, webhooks, admin API. |
| **Operational Stability** | Can the vendor provide continuity, support, and change notice? | SLA, incident history, roadmap, deprecation policy, support terms. |
| **Cost Predictability** | Can the enterprise forecast and cap usage cost? | Pricing schedule, overage controls, usage telemetry, renewal terms. |
| **Portability and Exit** | Can the enterprise leave without losing strategic assets? | Export rights, data format, termination assistance, deletion certificate. |
| **Financial and Strategic Viability** | Will the vendor survive and remain aligned with the buyer’s needs? | Funding/financial review, customer references, roadmap dependency assessment. |

### **Risk-Tier Evidence Requirements**

| Risk Tier | Minimum Review Posture |
| :---- | :---- |
| **Low** | Security questionnaire, DPA where data is processed, basic admin controls, export path. |
| **Medium** | Security evidence, SSO/SCIM where needed, audit logs, retention terms, pilot eval. |
| **High** | Legal/security/governance review, data-flow diagram, subprocessor review, SLA, eval evidence, incident process. |
| **Regulated / Critical** | Formal risk assessment, contractual controls, strong auditability, region controls, exit plan, business-continuity review. |

### **Fatal Flaws**

| Fatal Flaw | Applies Especially To | Decision |
| :---- | :---- | :---- |
| Vendor uses customer data for model training without explicit approved consent. | All enterprise tiers. | Reject or renegotiate. |
| Vendor cannot explain data retention, subprocessors, or region handling. | Medium and above. | Reject until clarified. |
| Vendor cannot support required identity and access controls. | Medium and above. | Reject for production. |
| Vendor blocks export of customer-owned data, prompts, corrections, or audit evidence. | Strategic workflows. | Reject or require contractual fix. |
| Vendor cannot meet required regulatory or contractual constraints. | High / regulated. | Reject. |
| Vendor refuses reasonable security or incident disclosure terms. | Medium and above. | Reject or escalate. |
| Product fails proprietary evaluation baseline. | All AI capability claims. | Reject or limit to non-production. |
| Cost cannot be capped, forecasted, or monitored for expected usage. | High-volume workflows. | Reject or require usage controls. |

A scorecard should support judgment, not replace it. Procurement decisions should record risk tier, accepted residual risks, required mitigations, and the named owner of the vendor relationship.

## **Data Rights and Training-Use Checklist**

AI contracts must define rights and restrictions for prompts, completions, uploaded files, embeddings, vector indexes, telemetry, user corrections, fine-tunes, adapters, logs, and derived artifacts. Marketing assurances are not enough. The controlling terms must appear in the contract, DPA, order form, enterprise settings, or incorporated policy.

### **Contract Review Checklist**

| Checklist Item | Required Question | Required Evidence |
| :---- | :---- | :---- |
| **Processing Purpose** | Is customer data processed solely to provide the contracted service? | DPA/service terms. |
| **No-Training Commitment** | Are prompts, completions, files, embeddings, telemetry, and feedback excluded from vendor model training unless explicitly approved? | Contractual no-training clause or enterprise configuration. |
| **Retention Period** | How long does the vendor retain each data class? | Retention schedule by data type. |
| **Deletion Rights** | Can the customer delete data and receive confirmation? | Deletion workflow, certificate/attestation terms. |
| **Subprocessor Disclosure** | Which downstream providers process customer data? | Subprocessor list and notice period. |
| **Region and Residency** | Where is data processed, stored, logged, and backed up? | Region controls and data-flow diagram. |
| **Human Access** | Can vendor personnel review customer inputs/outputs? | Access policy, support access controls, “eyes-off” terms where needed. |
| **Embeddings and Vector Indexes** | Who owns vector representations and derived search indexes? | Ownership/export clause. |
| **Feedback and Corrections** | Who owns user edits, ratings, corrections, and workflow telemetry? | Feedback data clause and export rights. |
| **Fine-Tunes / Adapters** | Who owns custom weights, adapters, datasets, and resulting artifacts? | Fine-tune ownership/export terms. |
| **Output Rights** | Can outputs be used commercially, redistributed, or used for downstream training? | Output-use clause. |
| **Audit Rights** | Can the customer verify compliance with data terms? | Audit reports, certifications, contractual audit language. |
| **Termination Export** | Can the customer export data before termination? | Export format, timeline, assistance clause. |

### **Consumer and Shadow-AI Caution**

Consumer AI tools often have different data-use, retention, training, and admin-control terms than enterprise products. Current provider policies may allow users to opt out of training or choose data-use settings, but those defaults change and vary by product tier. Enterprises should not rely on consumer settings for corporate data protection.

| Tool Type | Enterprise Treatment |
| :---- | :---- |
| **Consumer AI account** | Block or restrict for confidential, regulated, or customer data unless explicitly approved. |
| **Team / Business account** | Review admin settings, data-use terms, retention, and export controls. |
| **Enterprise contract** | Require negotiated data rights, DPA, retention, audit, and no-training language. |
| **Vendor-embedded AI feature** | Treat as a subprocessed AI system and review data flow. |

### **Data Rights Rule**

No production AI system should launch until the organization can answer:

```text
What data enters?
Where does it go?
Who can see it?
How long is it retained?
Can it train a model?
Who owns derived artifacts?
Can we export it?
Can we delete it?
Can we prove the answers?
```

## **AI Security Review Checklist**

AI security review must cover software supply chain, model artifacts, provider infrastructure, tenant isolation, prompt/tool boundaries, data egress, secrets, and incident response. Traditional SaaS security review is necessary but not sufficient.

| Security Area | Review Question | Required Artifact | Fatal Flaw |
| :---- | :---- | :---- | :---- |
| **Model Provenance** | Do we know where the model, weights, tokenizer, adapters, and runtime came from? | Model registry entry, source URL, hash/signature where available. | Unverified model artifact from unofficial source. |
| **AI-BOM / SBOM** | Are model, code, containers, dependencies, datasets, and tools inventoried? | AI-BOM/SBOM with dependency and license metadata. | No inventory for production dependency. |
| **License Compliance** | Are use, modification, redistribution, output, and training rights understood? | Legal review and license record. | Production use violates license or unclear commercial rights. |
| **Provider Security** | Does hosted provider meet required security posture? | SOC 2/ISO evidence or equivalent, pen-test summary, vulnerability process. | Vendor refuses reasonable security evidence for risk tier. |
| **Tenant Isolation** | Can one tenant/user access another tenant’s data, indexes, prompts, or logs? | Isolation design, access tests, row/index-level controls. | Tenant isolation cannot be demonstrated. |
| **Secrets Management** | Are API keys, tokens, and credentials centrally managed? | Vault/KMS configuration, rotation policy, access logs. | Hardcoded or developer-local production secrets. |
| **Prompt Injection and Data Egress** | Can malicious content redirect the system to reveal secrets or exfiltrate data? | Boundary tests, DLP filters, policy hierarchy, red-team cases. | Model can access or leak unauthorized data. |
| **Tool Execution** | Are model-triggered actions sandboxed, authorized, and verified? | Tool contracts, sandbox policy, idempotency and verification tests. | Model can execute unbounded actions. |
| **Logging and Retention** | Are prompts, outputs, files, and traces stored safely and minimally? | Retention policy, redaction, secure references, access controls. | Sensitive logs stored broadly or indefinitely without justification. |
| **Runtime Security** | Are containers, drivers, serving runtimes, and dependencies patched? | Image registry, vulnerability scan, patch cadence. | Unpatched critical vulnerabilities in production path. |
| **Network Controls** | Is model traffic restricted to approved endpoints and regions? | Egress policy, firewall rules, private networking where needed. | Open outbound network from sensitive AI runtime. |
| **Incident Response** | Can AI-specific incidents be detected and contained? | Runbook, owner, escalation path, rollback route. | No owner or containment path for production AI system. |
| **Vulnerability Disclosure** | Does vendor/project report and remediate security issues? | Disclosure policy, SLA, security contact, CVE history. | No credible security response path for critical dependency. |

AI security review should be performed before procurement approval, before production launch, and after material model, provider, tool, or data-flow changes.

## **Integration Cost Model**

The purchase price of an AI dependency is only one part of integration cost. Real integration cost includes identity, permissions, data preparation, retrieval, evals, telemetry, support, governance, fallback, migration, and user enablement.

Avoid universal engineering-hour estimates. Instead, estimate each cost center from system complexity, number of integrations, data volume, risk tier, and required assurance level.

### **Integration Cost Centers**

| Cost Center | Work Included | Estimation Drivers |
| :---- | :---- | :---- |
| **Identity and Provisioning** | SSO, OIDC/SAML, SCIM, RBAC/ABAC, group mapping. | Number of directories, roles, tenants, environments. |
| **Permission Mapping** | Translating enterprise ACLs into retrieval, search, UI, and tool access. | Number of source systems, permission models, tenant boundaries. |
| **Data Preparation** | Cleaning, parsing, OCR, metadata, deduplication, lineage. | Corpus size, format variety, quality, ownership clarity. |
| **Embedding and Indexing** | Embedding generation, vector DB setup, chunking, reindexing. | Document/chunk count, dimensions, model choice, update frequency. |
| **Retrieval Quality** | Reranking, citation verification, source freshness, conflict handling. | Required precision, source complexity, regulated evidence needs. |
| **Gateway Integration** | Routing, provider abstraction, quota, budget, policy, fallback. | Number of providers/routes/apps/tenants. |
| **Observability** | Traces, logs, cost, latency, quality, adoption telemetry. | Risk tier, retention, dashboard, incident requirements. |
| **Evaluation Program** | Golden sets, regression tests, human review, acceptance gates. | Task complexity, failure consequence, domain expertise needed. |
| **Security and Compliance** | DPA, legal review, SBOM/AI-BOM, pen testing, privacy review. | Data sensitivity, jurisdiction, vendor count, audit obligations. |
| **Support and Adoption** | Training, documentation, champion network, help desk, onboarding. | User count, workflow change depth, role diversity. |
| **Resilience and Fallback** | Circuit breakers, retry queues, degraded modes, alternate providers. | Criticality, SLA, provider dependence, traffic volume. |
| **Exit and Migration** | Export tooling, reindexing, prompt migration, eval comparison, user retraining. | Lock-in dimensions, data volume, workflow coupling. |

### **Reindexing and Embedding Migration Cost**

Changing embedding models often requires rebuilding the vector index. The cost depends on:

```text
number of source documents
× chunks per document
× embedding model cost or infrastructure cost
× dimensions and storage footprint
× metadata reconstruction
× validation and retrieval eval effort
× downtime or blue-green migration requirements
```

The safe planning assumption is that embedding migration is a project, not a config flip.

### **Integration Cost Estimate Template**

| Estimate Field | Value |
| :---- | :---- |
| Use case / workflow |  |
| Risk tier |  |
| Vendor / route |  |
| Number of user groups |  |
| Number of source systems |  |
| Number of data classes |  |
| Corpus size / chunk estimate |  |
| Identity integration complexity | Low / Medium / High |
| Permission mapping complexity | Low / Medium / High |
| Evaluation burden | Low / Medium / High |
| Governance burden | Low / Medium / High |
| Migration/exit burden | Low / Medium / High |
| Internal teams required |  |
| External vendor dependency |  |
| One-time integration estimate |  |
| Recurring operating estimate |  |
| Major assumptions |  |

Integration cost is where “cheap API” fantasies go to be politely mugged by reality.

## **Lock-In Taxonomy**

Lock-in is not a singular commercial concern; it is a multi-dimensional technical dependency. Enterprise architects must identify, track, and mitigate eleven distinct dimensions of lock-in.

```
                  ┌──────────────────────────────────────────────┐  
                  │              LOCK-IN CATEGORY                │  
                  └──────────────────────┬───────────────────────┘  
                                         │  
        ┌───────────────┬────────────────┼───────────────┬───────────────┐  
        ▼               ▼                ▼               ▼               ▼  
  ┌───────────┐   ┌───────────┐    ┌───────────┐   ┌───────────┐   ┌───────────┐  
  │   MODEL   │   │    API    │    │  PROMPT   │   │ EMBEDDING │   │   DATA    │  
  └───────────┘   └───────────┘    └───────────┘   └───────────┘   └───────────┘  
        ▼               ▼                ▼               ▼               ▼  
  ┌───────────┐   ┌───────────┐    ┌───────────┐   ┌───────────┐   ┌───────────┐  
  │ WORKFLOW  │   │   EVAL    │    │ AGENT/TOOL│   │COMMERCIAL │   │COMPLIANCE │  
  └───────────┘   └───────────┘    └───────────┘   └───────────┘   └───────────┘  
                                         ▼  
                                   ┌───────────┐  
                                   │ ADOPTION  │  
                                   └───────────┘
```

1. **Model Lock-In**: Product functionality depends on the unique edge-case behaviors, formatting styles, or reasoning quirks of a specific provider's model.12  
2. **API Lock-In**: Application code directly imports vendor-specific SDKs and endpoints, requiring a codebase refactor to swap providers.12  
3. **Prompt Lock-In**: System prompts are highly optimized and tuned to one model architecture, producing degraded or incorrect outputs when sent to other models.12  
4. **Embedding Lock-In (Representation Lock-In)**: Stored vector indexes are tied to the specific mathematical coordinate system and dimensionality of a single embedding model.11 Upgrading or changing the model renders the entire index obsolete overnight.11  
5. **Data Lock-In**: Interaction history, training data, and user correction logs are stored in a vendor's proprietary cloud and cannot be easily exported.9  
6. **Workflow Lock-In**: Internal business processes are redesigned around a vendor's custom SaaS user interface, making system migration difficult.10  
7. **Eval Lock-In**: Performance monitoring and evaluation metrics are managed exclusively inside a vendor’s proprietary dashboard, preventing independent verification.21  
8. **Agent & Tool Lock-In**: The orchestration platform is tied to a vendor's proprietary tool-calling schema, blocking migration to open-source agent runtimes.23  
9. **Commercial Lock-In**: High usage-based discounts or multi-year commitments hide future pricing increases and limit bargaining leverage.10  
10. **Compliance Lock-In**: Audit records, safety proofs, and regulatory reports reside inside a vendor's system, leaving the organization vulnerable during external compliance audits.27  
11. **Adoption Lock-In**: Staff are trained exclusively on a vendor-specific interface, requiring retraining during system migrations.22

## **AI Vendor Exit Plan Template**

Every production AI dependency must have an exit plan before launch. Exit planning is not pessimism. It is dependency hygiene. Vendors change prices, models, policies, regions, roadmaps, ownership, and terms. Systems that cannot leave cannot negotiate.

### **Exit Plan Summary**

| Field | Required Detail |
| :---- | :---- |
| Vendor / dependency |  |
| Workload / product surface |  |
| Risk tier |  |
| Exit trigger | price, outage, breach, deprecation, performance regression, legal change, roadmap failure, acquisition, etc. |
| Replacement route | secondary provider, self-hosted model, deterministic fallback, manual workflow. |
| RTO target | Risk-tier-specific recovery time objective. |
| RPO target | Risk-tier-specific recovery point objective for data/evidence loss. |
| Critical data to export | prompts, outputs, files, embeddings, indexes, logs, evals, feedback, adapters, configs. |
| Non-exportable data | vendor-owned or unavailable artifacts. |
| User impact | retraining, UI change, workflow disruption. |
| Owner | named business, technical, legal, and operational owners. |

### **Exit Phases**

| Phase | Purpose | Required Actions | Acceptance Criteria |
| :---- | :---- | :---- | :---- |
| **1. Trigger and Triage** | Decide whether exit is required. | Confirm trigger, freeze risky changes, notify owners, preserve evidence. | Exit decision recorded with risk and timeline. |
| **2. Alternative Route Activation** | Maintain service continuity. | Shift traffic through gateway, activate fallback provider/model/manual route. | Error, latency, quality, and safety remain within approved thresholds. |
| **3. Data Export** | Recover customer-owned and operational artifacts. | Export prompts, outputs, files, logs, feedback, configs, eval results, and metadata. | Export manifest reconciles against inventory. |
| **4. Embedding / Index Migration** | Avoid representation lock-in. | Rebuild index with target embedding model; run retrieval evals; perform blue-green cutover. | Retrieval quality and latency meet workload-specific thresholds. |
| **5. Prompt and Route Migration** | Preserve behavior. | Recompile prompts for target model/route; validate schema, safety, and task quality. | Eval suite passes acceptance gate. |
| **6. Tool and Integration Migration** | Preserve side-effect safety. | Rebind APIs, credentials, webhooks, tool schemas, action verification. | End-to-end transaction tests pass. |
| **7. User and Support Transition** | Preserve adoption and continuity. | Update training, support docs, announcements, escalation path. | Users can complete critical workflows. |
| **8. Deletion and Termination** | End dependency cleanly. | Request deletion/return, revoke access, rotate keys, obtain deletion attestation where contractually available. | Legal/procurement sign-off complete. |
| **9. Post-Exit Review** | Improve future sourcing. | Document causes, surprises, missing clauses, migration pain, residual risks. | Lessons feed procurement and architecture standards. |

### **Exit Drill Requirements**

| Dependency Criticality | Drill Cadence |
| :---- | :---- |
| **Low** | Paper review before renewal. |
| **Medium** | Annual export and route-switch test. |
| **High** | Semiannual fallback and data-export test. |
| **Critical / Regulated** | Regular operational drills with measured RTO/RPO and evidence review. |

Exit plans should define thresholds by workload. A support chatbot and a regulated transaction system do not need the same RTO, but both need to know how they leave.

## **Real Total Cost of Ownership (TCO) Model**

AI sourcing decisions must be evaluated using real total cost of ownership, not subscription price or token price alone.

```text
Real TCO =
  C_vendor
+ C_infra
+ C_integration
+ C_governance
+ C_ops
+ C_support
+ C_eval
+ C_security
+ C_risk
+ C_lifecycle
+ C_exit
```

| Cost Variable | Meaning |
| :---- | :---- |
| **C_vendor** | SaaS subscription, managed API usage, support tier, overages, renewal increases. |
| **C_infra** | GPUs, cloud instances, storage, networking, power, cooling, reserved capacity. |
| **C_integration** | Identity, permissions, API work, data pipelines, gateway, reindexing, workflow changes. |
| **C_governance** | Legal review, DPA, procurement, compliance documentation, audit preparation. |
| **C_ops** | Platform engineering, MLOps, SRE, incident response, monitoring. |
| **C_support** | User training, help desk, champion network, documentation, change management. |
| **C_eval** | Golden sets, regression testing, human review, model comparison, eval infrastructure. |
| **C_security** | SBOM/AI-BOM, pen testing, vulnerability management, access review. |
| **C_risk** | Expected loss from outages, breaches, hallucinations, compliance failures, support incidents. |
| **C_lifecycle** | Upgrades, patching, hardware refresh, model migration, data retention, decommissioning. |
| **C_exit** | Export, reindexing, prompt migration, retraining, contract termination, deletion verification. |

### **TCO Worksheet**

| Cost Center | Year 0 / Setup | Annual Recurring | Scaling Driver | Confidence |
| :---- | :---- | :---- | :---- | :---- |
| Vendor fees |  |  | seats / tokens / requests / usage tier | High / Medium / Low |
| Infrastructure |  |  | GPU hours / storage / traffic / regions | High / Medium / Low |
| Integration |  |  | source systems / APIs / identity / data volume | High / Medium / Low |
| Governance and legal |  |  | risk tier / jurisdictions / vendors | High / Medium / Low |
| Operations |  |  | routes / SLOs / incidents / model count | High / Medium / Low |
| Support and adoption |  |  | users / roles / training intensity | High / Medium / Low |
| Evaluation |  |  | task types / risk / release cadence | High / Medium / Low |
| Security |  |  | dependencies / data sensitivity / tools | High / Medium / Low |
| Lifecycle |  |  | hardware / model changes / retention | High / Medium / Low |
| Exit |  |  | lock-in dimensions / data volume / migration complexity | High / Medium / Low |

### **Sensitivity Variables**

| Variable | Why It Matters |
| :---- | :---- |
| Token volume growth | Turns small usage fees into major COGS. |
| Context length | Drives input token cost and latency. |
| Output length | Drives generation cost and review burden. |
| Retry / failure rate | Creates hidden cost. |
| Human review time | Can erase AI productivity gains. |
| Utilization | Determines whether self-hosting economics work. |
| Provider price changes | Alters managed API/vendor app economics. |
| Data growth | Drives storage, embedding, and reindexing cost. |
| Risk tier changes | Increases governance, eval, and audit cost. |
| Migration frequency | Turns exit cost into recurring architecture burden. |

### **TCO Comparison Template**

| Decision Option | Strength | Major Cost Driver | Major Risk | Exit Burden | Best When |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Vendor Application** | Fastest workflow deployment. | Seats, premium features, renewal pricing. | Workflow and data lock-in. | Medium to high. | Workflow is commodity and vendor fit is strong. |
| **Managed API** | Fast capability access. | Token/context/retry volume. | Provider drift and cost volatility. | Medium if gateway exists. | Demand is variable or frontier capability matters. |
| **Hosted Open Model** | More control without full ops burden. | Hosting capacity and support. | Runtime/provider dependence. | Medium. | Need data isolation and portable weights. |
| **Self-Hosted Model** | Strong control and predictable capacity. | Staffing, utilization, hardware lifecycle. | Operational complexity. | Medium; depends on serving stack. | High predictable volume or strict data control. |
| **Hybrid Architecture** | Flexible optimization. | Gateway/platform complexity. | Debugging and governance complexity. | Low to medium if designed well. | Workloads differ by sensitivity, cost, and capability. |

TCO should be recalculated at renewal, major model migration, workload growth, incident, regulatory change, and hardware refresh.

## **Procurement Reality Model**

AI procurement is a multi-disciplinary operating process. Legal, security, engineering, governance, finance, product, operations, and the business owner must review different parts of the dependency. The goal is not to slow every request equally. The goal is to route each request through the review depth appropriate to its risk.

```text
AI PROCUREMENT FLOW

[ Intake Request ]
  use case | data classes | users | vendor | model route | risk hypothesis
        |
        v
[ Triage ]
  low | medium | high | regulated / prohibited
        |
        +--> [ Fast Track Review ]
        |
        +--> [ Standard Review ]
        |
        +--> [ Enhanced Review ]
        |
        +--> [ Block / Legal Escalation ]
        |
        v
[ Parallel Review ]
  legal/DPA
  security
  engineering/integration
  governance/compliance
  finance/TCO
  product/workflow fit
  adoption/support
        |
        v
[ Decision ]
  approve
  approve with controls
  sandbox only
  revise and resubmit
  reject
        |
        v
[ Inventory + Monitoring + Renewal Review ]
```

### **Risk-Tier Review Matrix**

| Procurement Tier | Example Use | Review Requirements | Decision Options | Review Cadence |
| :---- | :---- | :---- | :---- | :---- |
| **Tier 1: Low Risk** | Internal productivity tool with non-sensitive data. | Lightweight security/privacy review, acceptable-use rules, vendor terms check. | Approve, approve with limits, sandbox only. | Renewal or material change. |
| **Tier 2: Medium Risk** | Customer-facing drafting, internal RAG, workflow automation with limited data. | DPA, security evidence, data-flow review, admin controls, eval/pilot. | Approve with controls, pilot, revise. | Annual or material change. |
| **Tier 3: High Risk** | Sensitive data, external outputs, important business decisions, automated tool use. | Legal, security, governance, eval, incident plan, exit plan, SLA, subprocessor review. | Controlled approval, limited scope, revise, reject. | Semiannual, renewal, incident, model/provider change. |
| **Tier 4: Regulated / Critical** | High-impact, regulated, safety, employment, finance, healthcare, law, or irreversible actions. | Formal risk assessment, human oversight, audit evidence, model documentation, operational controls. | Executive/legal approval, manual-only support, reject. | Continuous or event-triggered review. |
| **Prohibited / Unacceptable** | Unlawful, discriminatory, non-consensual, unsafe, or policy-prohibited use. | Legal escalation and technical block. | Reject/block. | Immediate. |

### **Review Workstreams**

| Workstream | Reviews |
| :---- | :---- |
| **Product / Business Owner** | Workflow fit, value, user population, adoption burden, success metrics. |
| **Legal / Privacy** | DPA, data rights, retention, subprocessors, SCCs, IP, output rights, deletion. |
| **Security** | Vendor security, tenant isolation, access control, secrets, supply chain, incident terms. |
| **Engineering** | Integration complexity, gateway compatibility, APIs, fallback, telemetry, scalability. |
| **Governance / Compliance** | Risk classification, audit evidence, human oversight, regulatory obligations. |
| **Finance / FinOps** | TCO, cost caps, renewal exposure, usage-based pricing, exit cost. |
| **Operations / Support** | Runbooks, owner, help desk, user support, escalation, incident handling. |
| **Adoption / Enablement** | Training, role impact, champion needs, communication, feedback loop. |

### **Mandatory Procurement Artifacts**

| Artifact | Required When |
| :---- | :---- |
| Intake form | All AI procurements. |
| Data-flow diagram | Medium risk and above. |
| Vendor security evidence | All production uses; depth by risk. |
| DPA / data rights review | Any customer, employee, confidential, or regulated data. |
| AI inventory entry | All approved systems. |
| Evaluation result | Any AI capability claim used in workflow. |
| TCO estimate | Medium/high volume or paid production use. |
| Exit plan | Medium risk and above; all strategic dependencies. |
| Adoption/support plan | Any broad user rollout. |
| Risk acceptance record | Any residual material risk. |

Procurement should end with an owned inventory record, not an email saying “approved.” The inventory record is what enables renewal review, incident response, audit, and exit.

## **Cross-Canon Handoff Map**

AI-ENG-AH defines sourcing architecture and strategic dependency design. It determines when to build, buy, self-host, use open-weight models, consume managed APIs, assemble open-source stacks, or route across hybrid execution planes. It connects product strategy, governance, runtime architecture, security, sustainability, telemetry, evaluation, procurement, and exit planning.

| Canon Report | Handoff Into AH | AH Use | Handoff Back |
| :---- | :---- | :---- | :---- |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, risk tier, product pattern. | Determines whether sourcing is justified and what capability is actually needed. | Sourcing constraints inform product surface and use-case viability. |
| **AI-ENG-AG — Adoption Systems** | Training, support, role impact, feedback loops, adoption burden. | Evaluates vendor support, change cost, and user migration risk. | Vendor choice informs enablement and support architecture. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, procurement controls, accountability, risk classification. | Defines approval paths, vendor obligations, and evidence requirements. | Sourcing decisions populate AI inventory and governance records. |
| **AI-ENG-AE — Sustainable AI** | Energy, lifecycle, hardware utilization, carbon/water impact. | Adds sustainability to build/buy/self-host decisions. | Deployment choice feeds sustainability telemetry and lifecycle review. |
| **AI-ENG-J — Throughput Mechanics** | Batching, latency, queueing, token economics. | Informs serving cost and provider/runtime selection. | Sourcing choices constrain throughput design. |
| **AI-ENG-K — Weight Dynamics** | Quantization, adapters, model variants, portability. | Evaluates open-weight/self-host viability and migration risk. | License and hosting choices constrain model adaptation. |
| **AI-ENG-L — Serving Architecture** | Gateways, routing, fallback, model serving topologies. | Implements “own control plane; vary execution plane.” | Vendor/provider choices become route manifests. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, permissions, side effects. | Evaluates vendor/tool integration risk. | Vendor tools must comply with internal tool contract requirements. |
| **AI-ENG-O — Action Verification** | Verification, idempotency, source-of-record checks. | Determines whether vendor or agentic systems may execute actions. | Procurement requires action verification evidence. |
| **AI-ENG-T — Boundary Defense** | Tenant isolation, prompt injection, egress, data boundaries. | Shapes security review and provider eligibility. | Vendor risks feed boundary defense controls. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, cryptographic trust, vulnerability management. | Drives open-source, open-weight, and vendor security diligence. | Sourcing records feed supply-chain inventory. |
| **AI-ENG-V — Resource Abuse** | Cost bombs, loop limits, denial-of-wallet. | Requires budget caps, gateway quotas, and agent loop controls in contracts/design. | Vendor/API choices feed resource-abuse controls. |
| **AI-ENG-W — UX Resilience** | Fallback, degraded modes, user continuity. | Requires continuity planning and exit-aware user experience. | Sourcing choices define degraded-mode options. |
| **AI-ENG-X — Trust** | Transparency, contestability, user confidence. | Vendor opacity and workflow capture are trust risks. | Vendor disclosures and UI controls feed trust design. |
| **AI-ENG-Y — Human Review** | Human approval, reviewer burden, escalation. | Determines whether vendor app or automation needs human gates. | Sourcing choices affect review workload and staffing. |
| **AI-ENG-Z — Telemetry** | Traces, metrics, cost, latency, quality, adoption telemetry. | Requires provider-independent observability and cost tracking. | Vendor route data feeds telemetry schema. |
| **AI-ENG-AA — Evaluation** | Golden sets, regression gates, model comparisons. | Vendor/model selection must pass proprietary evals. | Sourcing strategy defines eval-by-route requirements. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit records. | Requires exportable evidence and audit trails. | Vendor contracts define evidence availability. |
| **AI-ENG-AC — AI Operations** | Incident response, rollback, runbooks, containment. | Requires vendor incident terms, fallback routes, and exit drills. | Procurement decisions become operational dependencies. |
| **AI-ENG-AJ — Reference Architectures** | Reusable enterprise architecture patterns. | Converts sourcing doctrine into implementation blueprints. | AH patterns become reference architecture variants. |

### **Core Handoff Rule**

Sourcing is architecture. A vendor contract, model license, API endpoint, embedding model, gateway, support term, or export clause can determine whether an AI system is portable, governable, profitable, safe, and durable.

The enterprise should not merely ask:

```text
Which model is best?
```

It should ask:

```text
Which dependencies are we creating?
Who controls them?
How do we verify them?
How do we operate them?
How do we leave?
```

#### **Works cited**

1. Open Source AI, accessed June 15, 2026, [https://opensource.org/ai](https://opensource.org/ai)  
2. The Open Source AI Definition – 1.0 – Open Source Initiative, accessed June 15, 2026, [https://opensource.org/ai/open-source-ai-definition](https://opensource.org/ai/open-source-ai-definition)  
3. The Future of Open Source in the AI Era - Blog - One Horizon, accessed June 15, 2026, [https://onehorizon.ai/blog/the-future-of-open-source-in-the-ai-era](https://onehorizon.ai/blog/the-future-of-open-source-in-the-ai-era)  
4. Meta Llama 3 and the 700M MAU Limit: Who This License Does Not ..., accessed June 15, 2026, [https://wcr.legal/llama-3-license-700m-mau-limit/](https://wcr.legal/llama-3-license-700m-mau-limit/)  
5. AI Gateway Architecture: A Guide for Technical Teams | MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/](https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/)  
6. Hybrid AI routing | Services - SLM-Works, accessed June 15, 2026, [https://slm-works.ai/services/hybrid-routing](https://slm-works.ai/services/hybrid-routing)  
7. LLM Gateway: Architecture, Routing & Governance Guide | Axiom Studio, accessed June 15, 2026, [https://axiomstudio.ai/learn/what-is-an-llm-gateway](https://axiomstudio.ai/learn/what-is-an-llm-gateway)  
8. Deployment - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/deployment](https://www.truefoundry.com/deployment)  
9. Enterprise AI Procurement & Contract Negotiation: The Complete Guide (2026), accessed June 15, 2026, [https://thenegotiationexperts.com/blog/ai-complete-guide](https://thenegotiationexperts.com/blog/ai-complete-guide)  
10. AI Vendor Agreements & Enterprise AI Transactions Explained - Altawil Law Group, accessed June 15, 2026, [https://thepathtojustice.com/ai-vendor-agreements-enterprise-ai-transactions/](https://thepathtojustice.com/ai-vendor-agreements-enterprise-ai-transactions/)  
11. Vendor Lock-In in the Embedding Layer: A Migration Story | by Vassiliki Dalakiari | ITNEXT, accessed June 15, 2026, [https://itnext.io/vendor-lock-in-in-the-embedding-layer-a-migration-story-183ea58e3668](https://itnext.io/vendor-lock-in-in-the-embedding-layer-a-migration-story-183ea58e3668)  
12. What Is an LLM Gateway and How Does It Work? - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/llm-gateway](https://www.truefoundry.com/blog/llm-gateway)  
13. Vector Migration Explained: 7 Reasons Moving Vector Data Is Harder Than It Looks | by Syed | Medium, accessed June 15, 2026, [https://medium.com/@syed_33931/vector-migration-explained-7-reasons-moving-vector-data-is-harder-than-it-looks-1edd66f6c439](https://medium.com/@syed_33931/vector-migration-explained-7-reasons-moving-vector-data-is-harder-than-it-looks-1edd66f6c439)  
14. LLM Hosting Cost 2026: Self-Host vs API Pricing Guide - AI Superior, accessed June 15, 2026, [https://aisuperior.com/llm-hosting-cost/](https://aisuperior.com/llm-hosting-cost/)  
15. Self-Hosting LLMs: Hidden Costs You're Missing - Azumo, accessed June 15, 2026, [https://azumo.com/artificial-intelligence/ai-insights/self-hosting-llms-cost](https://azumo.com/artificial-intelligence/ai-insights/self-hosting-llms-cost)  
16. Self-Hosted LLM Guide: Costs, Architecture & Breakeven Point ..., accessed June 15, 2026, [https://alpacked.io/blog/self-hosted-llm-guide/](https://alpacked.io/blog/self-hosted-llm-guide/)  
17. The LLM layer you're probably missing (LLM gateway pattern explained) - Redgate, accessed June 15, 2026, [https://www.red-gate.com/simple-talk/ai/the-llm-layer-youre-probably-missing-llm-gateway-pattern-explained/](https://www.red-gate.com/simple-talk/ai/the-llm-layer-youre-probably-missing-llm-gateway-pattern-explained/)  
18. Top 5 Enterprise AI Gateways for Multi-Model Routing in 2026 - Maxim AI, accessed June 15, 2026, [https://www.getmaxim.ai/articles/top-5-enterprise-ai-gateways-for-multi-model-routing-in-2026/](https://www.getmaxim.ai/articles/top-5-enterprise-ai-gateways-for-multi-model-routing-in-2026/)  
19. DPA for AI vendors: what to actually check before you sign ..., accessed June 15, 2026, [https://companyscope.io/topics/dpa-for-ai-vendors](https://companyscope.io/topics/dpa-for-ai-vendors)  
20. Vector Database Pricing Models: The Hidden Cost, accessed June 15, 2026, [https://www.actian.com/blog/databases/the-hidden-cost-of-vector-database-pricing-models/](https://www.actian.com/blog/databases/the-hidden-cost-of-vector-database-pricing-models/)  
21. LLM Gateway Architecture: 2026 Engineering Reference - Digital Applied, accessed June 15, 2026, [https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference](https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference)  
22. Preparing for EU AI Act Compliance with ISO 42001 - A-LIGN, accessed June 15, 2026, [https://www.a-lign.com/articles/preparing-for-eu-ai-act-compliance](https://www.a-lign.com/articles/preparing-for-eu-ai-act-compliance)  
23. The Full Picture of 2026 Enterprise AI Architecture: A New World of Autonomous Infrastructure Opened by Hybrid LLMs, AI Gateways, and AgentOS - note, accessed June 15, 2026, [https://note.com/betaitohuman/n/n1c9b6d466f6b?hl=en](https://note.com/betaitohuman/n/n1c9b6d466f6b?hl=en)  
24. SOFTWARE BILL OF MATERIAL FOR AI - BSI, accessed June 15, 2026, [https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/SBOM-for-AI_minimum-elements.pdf?__blob=publicationFile&v=4](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/SBOM-for-AI_minimum-elements.pdf?__blob=publicationFile&v=4)  
25. What Is an AI-BOM (AI Bill of Materials)? & How to Build It - Palo Alto ..., accessed June 15, 2026, [https://www.paloaltonetworks.com/cyberpedia/what-is-an-ai-bom](https://www.paloaltonetworks.com/cyberpedia/what-is-an-ai-bom)  
26. What Is an LLM Gateway? The Missing Layer Between Your App and AI Models, accessed June 15, 2026, [https://openrouter.ai/blog/insights/llm-gateway/](https://openrouter.ai/blog/insights/llm-gateway/)  
27. ISO 42001 Documentation: What's Required for Compliance? - Hicomply, accessed June 15, 2026, [https://www.hicomply.com/en-us/hub/documentation-requirements](https://www.hicomply.com/en-us/hub/documentation-requirements)  
28. ISO 42001 Checklist (2026): 38 Controls for AI Management | Knowlee, accessed June 15, 2026, [https://www.knowlee.ai/blog/iso-42001-checklist-ai-management](https://www.knowlee.ai/blog/iso-42001-checklist-ai-management)  
29. What Is Gemma 4's Apache 2.0 License? Why It Matters More Than the Model Itself, accessed June 15, 2026, [https://www.mindstudio.ai/blog/gemma-4-apache-2-license-commercial-use](https://www.mindstudio.ai/blog/gemma-4-apache-2-license-commercial-use)  
30. Vector Database: Everything You Need to Know - WEKA, accessed June 15, 2026, [https://www.weka.io/learn/guide/ai-ml/vector-database/](https://www.weka.io/learn/guide/ai-ml/vector-database/)  
31. The State of “Open” Source AI – Gabriel Toscano - Sites@Duke Express, accessed June 15, 2026, [https://sites.duke.edu/gabrieltoscano/2026/02/05/the-state-of-open-source-ai/](https://sites.duke.edu/gabrieltoscano/2026/02/05/the-state-of-open-source-ai/)  
32. FinOps for AI: Tools & Services Considerations, accessed June 15, 2026, [https://www.finops.org/wg/finops-for-ai-tools-services-considerations/](https://www.finops.org/wg/finops-for-ai-tools-services-considerations/)  
33. What Is the Gemma 4 Apache 2.0 License? Why It Changes ..., accessed June 15, 2026, [https://www.mindstudio.ai/blog/what-is-gemma-4-apache-2-license-commercial-ai-deployment](https://www.mindstudio.ai/blog/what-is-gemma-4-apache-2-license-commercial-ai-deployment)  
34. GSA AI Procurement Rules Would Introduce New Disclosure and Use-Rights Requirements for Federal Contractors - Gibson Dunn, accessed June 15, 2026, [https://www.gibsondunn.com/gsa-ai-procurement-rules-would-introduce-new-disclosure-and-use-rights-requirements-for-federal-contractors/](https://www.gibsondunn.com/gsa-ai-procurement-rules-would-introduce-new-disclosure-and-use-rights-requirements-for-federal-contractors/)  
35. "Additional Commercial Terms" must be removed from LICENSE to make Llama 3 open source #156 - GitHub, accessed June 15, 2026, [https://github.com/meta-llama/llama3/issues/156](https://github.com/meta-llama/llama3/issues/156)  
36. meta-llama/Llama-3.3-70B-Instruct - Hugging Face, accessed June 15, 2026, [https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct)  
37. Significant Risks in Using AI Models Governed by the Llama License - Open Source Guy, accessed June 15, 2026, [https://shujisado.org/2025/01/27/significant-risks-in-using-ai-models-governed-by-the-llama-license/](https://shujisado.org/2025/01/27/significant-risks-in-using-ai-models-governed-by-the-llama-license/)  
38. Eight essential clauses for AI contracts: A guide for vendors and customers in Northern Ireland - A&L Goodbody, accessed June 15, 2026, [https://www.algoodbody.com/insights-publications/eight-essential-clauses-for-ai-contracts-a-guide-for-vendors-and-customers-in-northern-ireland](https://www.algoodbody.com/insights-publications/eight-essential-clauses-for-ai-contracts-a-guide-for-vendors-and-customers-in-northern-ireland)  
39. Vector Database Challenges: What Breaks in Production - Redis, accessed June 15, 2026, [https://redis.io/blog/common-challenges-working-with-vector-databases/](https://redis.io/blog/common-challenges-working-with-vector-databases/)

---

## Attribution

Part of **Stunspot’s Guide to AI Systems** — *The AI Engineering Systems Canon*.

Created by **Sam “stunspot” Walker** / **Collaborative Dynamics**.

Repository: https://github.com/Stunspot/stunspots-guide-to-ai-systems  
Stunspot: https://stunspot.com  
Collaborative Dynamics: https://www.collaborative-dynamics.com  
Discord: https://discord.gg/stunspot  

Licensed under **CC BY-NC-SA 4.0** unless otherwise stated.  
Commercial use, resale, paid redistribution, inclusion in commercial training products, or incorporation into paid knowledge-base products requires prior written permission.

[← Back to Canon Map](../canon-map.md)