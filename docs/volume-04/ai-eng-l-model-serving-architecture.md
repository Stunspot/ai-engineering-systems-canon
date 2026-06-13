# AI-ENG-L — Model Serving Architecture - Routing, Load Balancing & Deployment Topologies

## **Doctrinal Paradigm: Traffic Governance for Probabilistic Compute**

Model serving in production enterprise systems represents a paradigm shift from hosting deterministic microservices to governing traffic across probabilistic compute pipelines.1 Traditional web serving assumes static request-response lifetimes, uniform resource footprints, and simple CPU or system memory bottlenecks.1 In contrast, serving architecture for large language models (LLMs) and foundation models must manage highly variable request processing times, dynamic memory footprints inside the High-Bandwidth Memory (HBM) of graphics processing units (GPUs), and complex operational dependencies.1  

The core operational doctrine of this report dictates that:  

> *Serving architecture is traffic governance for probabilistic compute: route each request to the cheapest sufficient, authorized, available, latency-appropriate model path while preserving isolation, reliability, observability, and rollback.4*  

Treating model serving as merely hosting a weight checkpoint behind an endpoint ignores the operational realities of hardware scarcity, variable context lengths, and tenant-level resource contention.1 The useful question is not "Where is the model hosted?" but "How does each request move through gateway, authentication, tenant policy, routing, queueing, cache, inference execution, validation, fallback, telemetry, and rollback under real production constraints?".4  
This architectural paradigm requires a decoupled design divided into a control plane and a data plane:

* **The Control Plane:** Manages configuration states, release pipelines, routing topologies, tenant quota definitions, scale targets, safety policies, and model metadata registries. It handles the declarative definition of model-serving environments, enabling operators to modify routing rules or adjust capacity allocations without rebuilding execution containers or disrupting live connections.  

* **The Data Plane:** Executes in the direct path of user requests. It performs request admission, tokenizes incoming prompts, extracts routing keys, queries cache states, schedules iteration-level batches, coordinates KV cache transfers, executes model inference on raw hardware, validates output structures, streams responses, and pumps telemetry events to collectors.2

By segregating these concerns, platform engineers can update routing heuristics, hot-swap model versions, scale capacity pools dynamically, and isolate tenant-level anomalies without introducing downtime or risking structural regressions inside the physical execution layer.2  

This report closes Volume 4: Runtime Architecture and Inference Mechanics, whose concern is how AI systems execute under physical, computational, and operational constraints. AI-ENG-J established throughput mechanics: prefill, decode, KV cache, batching, queueing, cache pressure, memory pressure, and capacity margins. AI-ENG-K established weight dynamics and decoding strategy: quantization, compression, speculative decoding, constrained decoding, sampling policies, kernel compatibility, and behavior-preserving optimization. AI-ENG-L now defines how inference capacity is deployed, exposed, routed, scaled, protected, and recovered in production.  

This report draws clean boundaries. AI-ENG-J owns the physical mechanics inside inference execution. AI-ENG-K owns numerical representation, compression, and decoding optimization. AI-ENG-L owns the serving topology around those engines: gateways, routers, load balancers, inference servers, model residency, local, edge, or cloud deployment, tenant-aware capacity, queues, autoscaling, cold starts, rate limits, backpressure, failover, high availability, and degraded serving patterns.4

## **Conceptual Glossary**

To ensure structural durability for retrieval-augmented generation (RAG) and establish a rigorous baseline for platform engineering, this section defines the core components of high-dimensional serving architectures.

| Term | Architectural Definition | Operational Implication |
| :---- | :---- | :---- |
| **Model Serving Architecture** | The control and data plane coordination layers that expose inference capacity to clients while managing system resources.6 | Governs how requests traverse physical networks to reach optimized, validated hardware partitions.13 |
| **Inference Server** | A specialized runtime engine that loads model weights, manages physical GPU memory, batches requests, and executes optimized compute kernels.14 | Directly interfaces with hardware via runtimes like vLLM, SGLang, and Triton.15 |
| **Model Gateway** | An intelligent proxy plane that centralizes model routing, tokenization, tracking, and telemetry.6 | Acts as a unified entrypoint for upstream client requests, decoupling application logic from backend runtimes.6 |
| **API Gateway** | An edge proxy that handles external client ingress, transport-layer security (TLS), global authentication, and rate-limiting.8 | Acts as the first line of defense, validating incoming HTTP/gRPC requests before forwarding them.19 |
| **Control Plane** | The system layer that manages declarative configurations, release manifests, and routing topologies.13 | Syncs state updates across the fleet asynchronously without interrupting the active request-handling path.13 |
| **Data Plane** | The execution path that tokenizes prompts, manages physical KV caches, executes model kernels, and streams tokens.6 | Operates under strict latency constraints (milliseconds) to handle live inference traffic.17 |
| **Routing Policy** | A behavioral configuration defining the rules that match a request context to a target model endpoint.4 | Maps incoming prompts to the most cost-effective, high-performing model path.4 |
| **Model Portfolio** | The heterogeneous collection of active models, quantization formats, and specialized task adapters.3 | Enables cost-performance optimization by matching tasks to tailored, specialized model tiers.3 |
| **Load Balancing** | Memory-, cache-, and tenant-aware traffic distribution across homogeneous or heterogeneous GPU replicas.1 | Prevents resource imbalances and avoids premature evictions of hot prefix caches.23 |
| **Tenant-Aware Capacity** | The logical or physical partitioning of model execution resources based on tenant identity and priority.2 | Prevents high-volume tenants from starving others of HBM slots or queuing capacity.2 |
| **Admission Control** | The gateway-level mechanism that intercepts, queues, or rejects incoming traffic based on active system saturation.3 | Protects physical GPU clusters from thrashing and memory exhaustion during traffic spikes.3 |
| **Backpressure** | Controlled resistance applied to upstream clients (e.g., HTTP 429 errors or queue delays) when system limits are reached.2 | Prevents the serving cluster from collapsing under unexpected or bursty workloads.24 |
| **Autoscaling** | The elastic provisioning of model replicas and physical GPU nodes based on live queue depth and cache saturation.7 | Automatically adjusts system capacity to match incoming load while minimizing idle hardware costs.9 |
| **Cold Start** | The latency delay incurred when provisioning a new replica, downloading weights, and compiling GPU kernels.8 | Can span minutes, requiring active mitigations to protect user-facing latency SLAs.9 |
| **Model Residency** | The operational state determining whether model weights are kept warm in VRAM, cached on SSD, or stored in cold registries.8 | Governs the tradeoff between immediate latency availability and active hardware memory footprints.8 |
| **Rate Limit** | A resource-aware safety ceiling capping requests, tokens, or concurrency metrics per unit of time. | Enforces usage policies and protects backend infrastructure from runaway client loops. |
| **Quota** | A contract-level budget defining the maximum allowable consumption (e.g., total tokens or spend limits) over long intervals. | Governs tenant-level entitlement limits for billing, budget enforcement, and cost controls. |
| **Fallback Route** | An alternative model or execution path automatically triggered when the primary route degrades or fails.8 | Guarantees continuous service availability during provider outages or local hardware failures.27 |
| **Degraded Mode** | A resilient operating state that serves lower-fidelity responses or limits features when resources are constrained.8 | Preserves basic user functionality and system trust rather than returning abrupt error states.8 |
| **High Availability** | The architectural design that minimizes downtime through replica redundancy, health checks, and automatic failovers.6 | Guarantees continuous platform access across hardware failures, zone drops, and cloud outages.6 |
| **Failover** | The automated routing transition that shifts active traffic from an unhealthy component to a healthy backup.27 | Isolates hardware and network anomalies instantly to preserve user-facing SLAs.27 |
| **Serving Trace** | A distributed log mapping a request's journey across gateways, routers, caches, and execution runtimes.6 | Enables regression audits, security forensic analysis, and granular tenant cost attribution.8 |

## **Serving Stack Reference Architecture**

A robust serving platform must orchestrate multiple software and hardware layers to deliver deterministic SLAs on top of non-deterministic model engines. The reference architecture diagram below traces the path of a client request down to physical GPU clusters.

```
+----------------------------------------------------------------------------------------------------+
|                              SERVING STACK REFERENCE ARCHITECTURE                                  |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    
|                                        [ Client / SDK ]                                            
|                                               |                                                    
|                                               v                                                    
|  +------------------------------------------------------------------------------------------+      
|  |                              API Gateway / Ingress                                       |      
|  |                                                                                          |      
|  |  TLS termination | global auth | corporate SSO | coarse rate limits | request logging    |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                         Tenant Policy Engine & Rate Limiter                              |      
|  |                                                                                          |      
|  |  API key validation | tenant tier | quotas | RPM / TPM / concurrency | budget ceilings   |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                          Request Classification / Context Analysis                       |      
|  |                                                                                          |      
|  |  modality | language | context length | schema need | risk level | latency / cost target |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +---------------------------------------------------------------------------------------------+   
|  |                              Model Router / Gateway                                         |   
|  |                                                                                             |   
|  |  static rules | semantic routing | portfolio selection | fallback policy | canary split     |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Cache Layers            |  | Retrieval / Tool      |  | Control Plane Snapshot      |   
|          |                         |  | Sidecars              |  |                             |   
|          | semantic cache          |  | vector retrieval      |  | routing policy              |   
|          | prompt / prefix cache   |  | tool gateways         |  | model metadata              |   
|          | radix cache telemetry   |  | MCP / API connectors  |  | tenant configs              |   
|          +-----------+-------------+  +-----------+-----------+  | release manifests           |   
|                      |                            |              | rollback state              |   
|                      +------------+---------------+              +-------------+---------------+   
|                                   |                                            |                   
|                                   v                                            |                   
|  +---------------------------------------------------------------------------------------------+      
|  |                         Load Balancer & Dispatch Layer                                      |      
|  |                                                                                             |      
|  |  queue depth | prefix affinity | adapter residency | cache warmth | late-binding flow ctrl  |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Queue / Admission Lane  |  | Queue / Admission Lane|  | Queue / Admission Lane      |   
|          | premium / low-latency   |  | standard interactive  |  | batch / background          |   
|          | priority + headroom     |  | fair-share scheduling |  | async deferral              |   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      +----------------------------+----------------------------+                   
|                                                   |                                                
|                                                   v                                                
|  +------------------------------------------------------------------------------------------+      
|  |                                  Inference Servers                                       |      
|  |                                                                                          |      
|  |  vLLM | SGLang | TensorRT-LLM | Triton | Ray Serve | KServe runtime containers           |      
|  |                                                                                          |      
|  |  model loading | continuous batching | paged KV cache | adapters | telemetry hooks       |      
|  +----------------------+----------------------+----------------------+---------------------+
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | GPU Pool A              |  | GPU Pool B            |  | GPU Pool C                  |   
|          | warm frontier models    |  | quantized specialists |  | batch / overflow / fallback |   
|          | HBM + KV cache          |  | LoRA adapter resident |  | degraded-mode capacity      |   
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|                                                   |                                                
|                                                   v                                                
|  +--------------------------------------------------------------------------------------------+    
|  |                              Output Validation Layer                                       |    
|  |                                                                                            |    
|  |  schema validation | safety filters | tool-result checks | grounding checks | retry gates  |    
|  +----------------------+--------------------------------------+------------------------------+    
|                         |                                      |                                   
|                         v                                      v                                   
|              [ Valid Response Stream ]              [ Fallback / Retry / Human Review ]            
|                         |                                      |                                   
|                         +----------------------+---------------+                                   
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                                   Telemetry & Audit                                      |      
|  |                                                                                          |      
|  |  traces | TTFT | ITL | queue depth | cache hit rate | model SHA | tenant cost | failures |      
|  +------------------------------------------------------------------------------------------+      
|                                                                                                    
+----------------------------------------------------------------------------------------------------+
| Supporting control-plane services: Model Registry, Artifact Store, Autoscaler, Rollback            |
| Controller, Health Checks, Policy Config Store, and Release/Canary Manager.                        |
+----------------------------------------------------------------------------------------------------+
```

### **Serving Stack Layer Matrix**

| Layer | Primary Responsibility | Representative Technologies | Fail-Safe Behavior |
| :--- | :--- | :--- | :--- |
| **API Gateway / Ingress** | Handles client ingress, transport-layer security termination, global rate-limiting, request logging, and corporate SSO integration. | Kong, Envoy, Istio Ingress, AWS API Gateway. | Fails closed on authentication failures and returns a clean authorization error. |
| **Tenant Policy Engine** | Validates API keys, extracts tenant context, resolves entitlement, and enforces quotas, RPM, TPM, concurrency, budget, and model-access limits. | Custom API proxy, Redis Cluster, durable quota ledger, policy store. | Fails closed when tenant identity or policy state cannot be verified. Downgrades only when identity is valid and policy explicitly permits a lower-tier route. |
| **Request Classification** | Inspects request payloads to identify modality, language, task class, context length, schema needs, risk level, and latency target. | ModernBERT classifiers, lightweight transformer sidecars, rules engines. | Defaults to a conservative standard route for low-risk requests; high-risk or policy-sensitive requests fail closed or route to human review. |
| **Model Router** | Evaluates the model portfolio to determine the optimal endpoint based on task difficulty, risk, latency, cost, tenant policy, fallback state, and route availability. | SGLang Model Gateway, RouteLLM, Not Diamond, LiteLLM, custom routers. | Defaults to a configured tenant-approved safe baseline route. High-risk or policy-sensitive requests fail closed rather than silently escalating to an unapproved frontier or external provider. |
| **Load Balancer** | Distributes requests based on queue depth, cache warmth, adapter residency, HBM pressure, tenant fairness, and prefix affinity. | Kubernetes Gateway API Inference Extension, llm-d, sgl-router, custom dispatchers. | Falls back to least-loaded or round-robin only when cache-affinity telemetry is unavailable and route safety remains intact. |
| **Queue / Admission Layer** | Buffers incoming requests, enforces priority scheduling, applies backpressure, and protects backend runtimes from saturation. | Endpoint Proxy Plane flow control, Redis-backed priority queues, custom admission controllers. | Rejects or defers low-priority work with clean HTTP 429/503 responses during peaks. |
| **Cache Layers** | Performs semantic caching, prompt caching, prefix tracking, and cache telemetry while preserving tenant and permission boundaries. | Redis Enterprise, Milvus, SGLang RadixCache, vLLM prefix cache. | Bypasses caching when safety, freshness, or authorization checks fail. Never serves cross-tenant or stale cache entries. |
| **Inference Servers** | Loads model weights, manages KV cache memory, schedules continuous batches, executes optimized kernels, streams tokens, and emits runtime metrics. | vLLM, SGLang Runtime, Triton Inference Server, TensorRT-LLM. | Triggers preemption, recomputation, swap, degraded routing, or circuit breaking depending on failure type. |
| **Model Registry** | Stores and distributes version-pinned, SHA-validated, immutable model artifacts and metadata. | Unity Catalog, MLflow Registry, OCI artifact stores, custom registries. | Halts deployment and rolls back to the preceding SHA-validated active artifact. |
| **Artifact Store** | Stores high-capacity weight checkpoints, quantized variants, adapter files, tokenizer bundles, templates, and startup snapshots. | AWS S3, Google Cloud Storage, MinIO, local SAN, OCI registries. | Blocks initialization on checksum mismatch or timeout; alerts operators and preserves prior active route. |
| **Retrieval / Tool Sidecars** | Coordinates retrieval calls, MCP/API tools, embedding stores, document fetches, and external functional dependencies. | Ray Serve applications, Composio MCP gateways, vector DB clients, tool gateways. | Disables affected tools or routes to direct-answer fallback when tool dependencies are unavailable. |
| **Validators** | Verifies structural output conformance, safety policy, tool arguments, grounding, and action readiness. | Guardrails AI, LlamaGuard, schema validators, custom policy checks. | Retries, escalates, downgrades, or fails closed depending on risk class and validation failure type. |
| **Telemetry** | Collects logs, traces, Prometheus metrics, audit-critical execution metadata, GPU metrics, cost records, and failure signals. | OpenTelemetry Collector, Prometheus, Datadog Agent, ClickHouse, DCGM. | Continues serving only if safety and audit requirements remain satisfied; samples or drops noncritical telemetry, raises alerts, and preserves audit-critical traces. |
| **Fallback Controller** | Coordinates backup routes, degraded modes, provider failover, model downgrade, and retry boundaries. | LiteLLM proxy, custom gateway interceptor, circuit breakers, route policy engine. | Returns standardized degraded-service responses when no safe fallback path remains. |
| **Rollback Controller** | Automates traffic shifts, rollback of model versions, canary aborts, route freezing, and recovery verification. | GitOps controllers, ArgoCD, custom Kubernetes operators, release controllers. | Freezes routing state, blocks new deployments, and restores the last known-good release manifest. |

### **Control Plane / Data Plane Coordination Model**

The control plane and data plane must remain semantically synchronized through versioned snapshots while staying operationally decoupled from request execution. Routing policies, tenant quotas, release manifests, fallback states, safety policies, and autoscaling targets should propagate into the data plane through explicit configuration versions, not live blocking lookups.

The data plane must be able to continue serving with the last known-good configuration if the control plane becomes slow or temporarily unavailable. Control-plane failure should never force live request execution to wait on configuration databases, registry lookups, or policy-management services.

| System Aspect | Control Plane Responsibilities | Data Plane Responsibilities | Coordination & Sync Mechanisms |
| :--- | :--- | :--- | :--- |
| **Configuration & Routing** | Defines target models, route matrices, static rules, semantic-routing policies, fallback paths, and canary split percentages. | Tokenizes prompts, extracts routing keys, evaluates local route snapshots, and dispatches to selected nodes. | Versioned routing snapshots streamed asynchronously through xDS, config maps, or gateway control channels. |
| **Tenant Quotas** | Manages tenant tiers, contract budgets, data residency, model entitlements, spend ceilings, and rate-limit policy. | Enforces RPM, TPM, concurrency, budget, and per-workflow execution limits during request admission. | Distributed counters and policy caches, such as Redis plus durable billing ledger reconciliation. |
| **Release Management** | Publishes release manifests, approves canaries, validates SHA-pinned artifacts, and controls progressive rollout percentages. | Applies canary splits, selects active model versions, records manifest IDs, and routes traffic to warm instances. | Immutable release manifests synchronized to the gateway and inference-serving layers before traffic shift. |
| **Autoscaling** | Computes desired capacity from queue depth, p95/p99 latency, HBM pressure, cache saturation, and forecasted load. | Reports queue depth, preemption, free-block count, active batch size, HBM occupancy, and cold-start readiness. | Prometheus/DCGM metrics feeding KEDA, Karpenter, Kubernetes controllers, or custom GPU autoscalers. |
| **Rollback & Failover** | Declares unhealthy routes, freezes deployments, selects previous stable manifests, and defines recovery criteria. | Trips circuit breakers, shifts traffic, activates degraded paths, and preserves request traces during failover. | Health probes, route-state snapshots, incident runbooks, and automated rollback controllers. |
| **Safety & Compliance** | Defines regulated-route policy, high-risk workflow rules, human-review requirements, and data-residency boundaries. | Enforces local routing decisions, fail-closed behavior, validation gates, and tenant-isolated execution boundaries. | Versioned policy bundles and audit-backed route approvals distributed to gateway and validator layers. |
| **Telemetry & Audit** | Defines required trace fields, retention policy, audit-critical metadata, and incident evidence requirements. | Emits route decisions, model SHAs, prompt hashes, cache state, validation results, fallback events, and cost records. | OpenTelemetry traces, structured logs, metrics stores, cost ledgers, and replayable serving records. |

The essential rule is simple: the control plane defines what is allowed and desired; the data plane executes what is currently authorized and safe. Configuration freshness matters, but request execution must not block on control-plane availability.

## **Inference Server Layer Mechanics & Compatibility Gates**

The base engine of the serving architecture is the inference server. It abstracts raw GPU hardware, exposing a programmatic API while managing memory layouts, continuous batch schedules, and execution optimization.  
The responsibilities of the inference server are:

* **Model Loading and Weight Fetching:** Server engines must fetch, cache, and mount model weights.8 To avoid third-party CDN dependencies during container startups, optimized platforms pull weights from local, immutable OCI-compliant registries.8  
* **Request Scheduling and Batching:** Static batching is unusable in conversational applications due to high variance in output generation lifetimes.32 Modern runtimes execute continuous, iteration-level batching, where completed sequences are evicted and waiting requests are scheduled at the boundary of each individual token step.32  
* **Memory Management (Paged KV Cache):** To prevent memory fragmentation, engines partition key-value attention tensors into logical blocks.17 These are dynamically mapped to non-contiguous physical memory locations inside GPU HBM via lookup tables, mimicking virtual memory in operating systems.17  
* **Adapter and Multi-Model Support:** Runtimes must host a single base model while dynamically mapping Low-Rank Adaptation (LoRA) adapters into active HBM space, ensuring adapter-specific routing without weight replication overhead.3  
* **Telemetry and Hook Points:** The engine must expose granular metrics (e.g., Time to First Token, Inter-Token Latency, HBM Cache occupancy) and support intermediate tensor inspection hooks for downstream validation processes.3

To evaluate how different platforms satisfy these core capabilities, engineers must analyze their architectural execution models.

### **Inference Server Architectural Responsibility Comparison**

Inference servers differ in how they load model artifacts, schedule batches, manage KV cache memory, support adapters, expose telemetry, and fit into broader serving topologies. Platform teams should evaluate these engines not as interchangeable HTTP endpoints, but as runtime systems with distinct memory, scheduling, compilation, and observability behaviors.

| Serving Engine | Weight Loading & Lifecycle | Batch Scheduling Model | Memory Management / KV Cache | Adapter & Multi-Model Support | Telemetry & Observability |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **vLLM** | Loads models from local disk, Hugging Face, or artifact stores; supports SHA-pinned deployment workflows. | Continuous, iteration-level batch scheduling with preemption, recompute, and swap mechanisms. | PagedAttention maps logical KV blocks to non-contiguous physical HBM blocks. | Supports LoRA and multi-LoRA serving patterns depending on runtime configuration. | Exposes Prometheus metrics for TTFT, ITL, queue state, cache usage, throughput, and runtime pressure. |
| **SGLang** | Loads model artifacts through runtime workers; supports OpenAI-compatible serving, structured generation, distributed serving, and high-throughput execution paths. | High-performance scheduling optimized for decode-heavy workloads, prefix reuse, and structured generation. | RadixAttention tracks and caches shared prompt prefixes in an active radix tree, reducing redundant prefill work. | Supports adapters and multi-model serving patterns, with prefix-aware routing and cache-locality considerations. | Provides metrics for radix cache hit rates, queue status, serving latency, throughput, and runtime health. |
| **TensorRT-LLM** | Compiles models into optimized engines and supports high-performance deployment on NVIDIA hardware. | Uses in-flight batching and optimized execution schedules designed for compiled inference graphs. | Supports paged KV cache and optimized tensor block layouts where configured. | Adapter support depends on engine build, model architecture, and predefined runtime pathways. | Exposes engine execution statistics, GPU metrics, and integration points through NVIDIA tooling. |
| **Triton Inference Server** | Manages explicit model repositories, polling, versioned model loading, and backend lifecycle control. | Supports dynamic batching, sequence batching, ensembles, and backend-specific scheduling. | Delegates memory management to backend runtimes for LLM workloads. | Supports multi-model serving through repository structure and custom backends. | Exposes execution counts, queue latency, inference latency, model errors, and backend metrics. |
| **Ray Serve** | Deploys Python-native serving classes across distributed Ray actors and worker pools. | Uses Ray-level ingress and actor scheduling; can wrap vLLM, SGLang, or custom runtimes. | Delegates KV cache and GPU memory behavior to child inference runtimes. | Manages model multiplexing, actor placement, and distributed application composition. | Provides Ray dashboard telemetry, actor health, request metrics, and application-level traces. |
| **KServe** | Provides Kubernetes-native serving abstractions with raw deployments, serverless modes, and pluggable runtimes. | Relies on underlying serving containers for LLM-specific batching and scheduling. | Delegates KV cache and GPU memory management to target runtime containers. | Supports multi-model serving through pluggable runtimes and deployment patterns. | Integrates with Knative, Istio, Prometheus, and Kubernetes-native monitoring. |

A serving engine should be selected based on the target workload’s dominant constraint: prefix reuse, structured generation, cold-start profile, quantization compatibility, distributed serving, operational simplicity, or maximum hardware efficiency. The correct engine is the one whose runtime behavior matches the workload’s memory, latency, observability, and rollback requirements.

### **Serving Compatibility Gate Model**

```
+--------------------------------------------------------------------------------------------------+
|                                SERVING COMPATIBILITY GATE MODEL                                  |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  
|  Goal: prevent incompatible model artifacts from reaching live inference hardware.               
|                                                                                                  
|  [ Candidate Serving Artifact ]                                                                  
|        |                                                                                         
|        |  model weights | tokenizer | prompt template | quantization config | LoRA adapters      
|        v                                                                                         
|  [ Target Serving Environment ]                                                                  
|        |                                                                                         
|        |  runtime engine | GPU type | CUDA stack | kernel set | memory budget | deployment tier  
|        v                                                                                         
|                                                                                                  
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 1: Model Architecture                                                              |    
|  |  Check config files, layer structure, attention type, MoE layout, rope scaling, and      |    
|  |  engine support against vLLM / SGLang / TensorRT-LLM / Triton target runtime.            |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 2: Tokenizer and Template Integrity                                                |    
|  |  Verify vocabulary, special tokens, chat template, BOS/EOS behavior, tool-call tokens,   |    
|  |  and tokenizer compression ratio against expected production prompts.                    |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 3: Precision / Quantization Format                                                 |    
|  |  Validate FP16 / BF16 / FP8 / INT8 / INT4 format against target GPU architecture,        |    
|  |  runtime kernels, calibration assumptions, and dequantization path.                      |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 4: Adapter Compatibility                                                           |    
|  |  Confirm LoRA / adapter target modules, rank, tokenizer assumptions, routing labels,     |    
|  |  and parent-model SHA match the active base checkpoint.                                  |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 5: Kernel and Hardware Compatibility                                               |    
|  |  Verify FlashAttention, paged KV cache, CUDA graph capture, DeepEP / MoE kernels,        |    
|  |  tensor parallelism, and GPU compute capability support.                                 |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +-------------------------------------------------------------------------------------------+   
|  |  Gate 6: Memory Envelope                                                                  |   
|  |  Dry-run VRAM allocation for weights, KV cache, batch size, max context length, adapters, |   
|  |  framework overhead, and safety headroom. Reject likely OOM or swap-heavy deployments.    |   
|  +---------------------------------------------+---------------------------------------------+   
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 7: Compilation and Runtime Capture                                                 |    
|  |  Run startup compile, kernel warmup, CUDA graph capture, tokenizer smoke test, and mock  |    
|  |  request execution. Cache successful templates where supported.                          |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |  Gate 8: Serving Smoke Test                                                              |    
|  |  Execute representative prompts through routing, batching, KV allocation, streaming,     |    
|  |  validation, telemetry, and fallback hooks.                                              |    
|  +---------------------------------------------+--------------------------------------------+    
|                                                | Passed                                          
|                                                v                                                 
|                                      [ Register as Canary Route ]                                
|                                                |                                                 
|                                                v                                                 
|                                      [ Promote if Telemetry Holds ]                              
|                                                                                                  
|  Failure path from any gate:                                                                     
|                                                                                                  
|        [ Gate Failure ]                                                                          
|              |                                                                                   
|              v                                                                                   
|        [ Reject Artifact / Block Promotion ]                                                     
|              |                                                                                   
|              +--> emit compatibility report                                                      
|              +--> keep previous stable route active                                              
|              +--> quarantine artifact or return to build pipeline                                
|                                                                                                  
+--------------------------------------------------------------------------------------------------+
| Doctrine: a model is not deployable merely because it loads. It must fit the runtime, tokenizer, |
| precision path, adapter stack, kernel set, memory envelope, validation layer, and rollback plan. |
+--------------------------------------------------------------------------------------------------+
```

## **Routing Architecture & Multi-Model Orchestration**

A multi-model routing layer transforms an expensive, fragile model cluster into a resilient, cost-efficient utility.4 Most production routing architectures implement a hybrid design: static routing rules process explicit identifiers, semantic routing evaluates prompt intents, and cascading pipelines handle failure recovery.4

```
+----------------------------------------------------------------------------------------------------+
|                         ROUTING ARCHITECTURE & MULTI-MODEL ORCHESTRATION                           |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    
|  [ Incoming Request ]                                                                              
|        |                                                                                           
|        v                                                                                           
|  +---------------------------------------------------------------------------------------------+   
|  |                              Prompt Context Analysis                                        |   
|  |                                                                                             |   
|  |  Extract: tenant | region | modality | language | context length | schema need | risk       |   
|  |           task type | latency target | budget ceiling | tool requirements | user tier       |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|              [ Static Rule? ]        [ Semantic Intent? ]       [ Safety / Policy Risk? ]          
|                    /   \                    /   \                       /   \                      
|              Yes  /     \ No          Yes  /     \ No             High /     \ Normal              
|                  v       v                v       v                   v       v                    
|        [ Explicit Route ]      [ Capability Evaluator ]      [ Secure / Human ]                    
|        model id, tenant tier,         |                       [ Review Route ]                     
|        region, tool contract          |                                |                            
|                  |                    v                                |                            
|                  |        +-------------------------------------+      |                           
|                  |        |       Capability / Cost Evaluator   |      |                          
|                  |        |                                     |      |                           
|                  |        | - reasoning depth                   |      |                           
|                  |        | - tool-use reliability              |      |                           
|                  |        | - schema adherence                  |      |                           
|                  |        | - multimodal fit                    |      |                           
|                  |        | - long-context fit                  |      |                           
|                  |        | - queue depth and availability      |      |                           
|                  |        | - cost per successful outcome       |      |                           
|                  |        +------------------+------------------+      |                           
|                  |                           |                         |                           
|                  +---------------------------+-------------------------+                           
|                                              |                                                     
|                                              v                                                     
|  +---------------------------------------------------------------------------------------------+   
|  |                                Route Selection Layer                                        |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +------------------------------+  
|          | Low-Cost Local Model    |  | Premium Frontier Model|  | Specialized Model Route      |  
|          |                         |  |                       |  |                              |  
|          | classification          |  | deep reasoning        |  | code / math / vision         |  
|          | extraction              |  | high-risk synthesis   |  | embeddings / reranking       |  
|          | simple drafting         |  | long-context analysis |  | structured-output specialist |  
|          +-----------+-------------+  +-----------+-----------+  +-------------+----------------+  
|                      |                            |                            |                   
|                      +----------------------------+----------------------------+                   
|                                                   |                                                
|                                                   v                                                
|  +------------------------------------------------------------------------------------------+      
|  |                                  Execution + Validation                                  |      
|  |                                                                                          |      
|  |  Run model -> validate schema -> check tool args -> verify grounding -> inspect safety   |      
|  +----------------------+--------------------------------------+----------------------------+      
|                         |                                      |                                   
|                         v                                      v                                   
|              [ Validation Pass ]                    [ Validation / Confidence Failure ]            
|                         |                                      |                                   
|                         v                                      v                                   
|              [ Return Response ]              +-------------------------------+                    
|                                               | Cascading Fallback Controller |                    
|                                               +---------------+---------------+                    
|                                                               |                                    
|                                                               v                                    
|                                      [ Retry on stronger / safer route ]                           
|                                                               |                                    
|                                      [ Human Review or Fail-Closed Path ]                          
|                                                                                                    
+----------------------------------------------------------------------------------------------------+
| Routing doctrine: prefer pre-generation routing when the request can be classified cheaply;        |
| use cascades only when the first route is likely to pass validation. Log every routing decision    |
| with rejected alternatives, validation results, cost, latency, and fallback path.                  |
+----------------------------------------------------------------------------------------------------+
```

Task-based and capability-based routing map straightforward requests to low-parameter models, while routing reasoning-heavy tasks to frontier networks.4 Risk-based routing automatically redirects high-stakes workflows (e.g., financial transactions or patient-facing diagnostic requests) to secure, pre-validated local models or human review queues.4  
Modality-based, language-based, and context-length routing evaluate prompt characteristics to match requests to hardware optimized for those targets.4 For example, prompts exceeding 32,000 tokens are routed away from memory-constrained local nodes to high-capacity cloud clusters.11 Tenant-tier and region routing enforce data residency requirements, guaranteeing that sensitive enterprise records remain within compliant geographic boundaries.2  
Cost-aware and latency-aware routing evaluate live metrics to balance execution pricing and speed.4 If the primary model’s queue latency spikes, confidence-based escalation and fallback routing immediately shift traffic to an alternative provider or equivalent open-weight replica.4 When automated tools or reasoning paths fail to return structured outputs, tool-first and human-review routing intercept the request, escalating the output to human administrators to preserve system trust.4  
Evaluating routing strategies requires analyzing their performance and latency overheads.

### **Routing Performance & Latency Matrix**

| Routing Method | Average Latency | Cost Impact | Typical Task | Typical Target Model |
| :---- | :---- | :---- | :---- | :---- |
| **Static Rules** | < 1ms | Fully predictable; zero routing overhead.4 | Structured classification with explicit tags.4 | Quantized 8B parameter model hosted locally.9 |
| **Semantic Routing** | 5-15ms 22 | High savings (60%+) by hitting local cache.28 | Near-duplicate customer support queries.28 | Local embedding similarity lookup with Redis.22 |
| **BERT Classifier** | 10-50ms 22 | Up to 85% cost reduction vs. using GPT-4 alone.29 | General conversational chats on public forums.44 | Fine-tuned 0.5B model classifying prompt difficulty.22 |
| **LLM Classifier** | 200-800ms 22 | Variable; high overhead can negate routing savings.22 | Complex code generation and analysis.44 | Claude Haiku evaluating prompt complexity.4 |
| **Cascading Fallback** | Variable 22 | High latency tax; adds roundtrips on escalations.22 | Multi-step agentic execution workflows.22 | Escalate to Sonnet if Haiku fails validation.4 |
| **Router-R1** | > 1000ms 22 | High; uses extensive reasoning step tokens.22 | Complex mathematical reasoning and proofs.22 | Specialized reasoning model with explicit thinking traces.22 |

A routing decision must be logged with its full context, capturing why alternative paths were rejected.4 If weak routing misclassifies hard tasks or overuses premium models, it can cause cost spikes or degrade system accuracy.4

## **Load Balancing for LLM Serving**

Standard round-robin load balancing is structurally unsuited for LLM serving.5 Request latency in auto-regressive decoding is determined by two factors: prompt length (prefill phase) and generated sequence length (decode phase).1 A round-robin proxy distributing requests evenly will cause hardware imbalances, where one replica is starved while another experiences memory thrashing.3  
To prevent these conditions, modern balancers implement memory-, cache-, and adapter-aware strategies.

### **Load Balancing Model for LLM Serving**

```
+-------------------------------------------------------------------------------------------------+
|                              LOAD BALANCING MODEL FOR LLM SERVING                               |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 
|  [ Incoming Inference Request ]                                                                 
|        |                                                                                        
|        v                                                                                        
|  +------------------------------------------------------------------------------------------+   
|  |                              Routing Key Extraction                                      |   
|  |                                                                                          |   
|  |  Extract stable fields:                                                                  |   
|  |  - target model                                                                          |   
|  |  - tenant / priority tier                                                                |   
|  |  - LoRA adapter or specialist route                                                      |   
|  |  - system prompt prefix                                                                  |   
|  |  - first user-turn prefix                                                                |   
|  |  - strict token / character budget                                                       |   
|  +---------------------------------------------+--------------------------------------------+   
|                                                |                                                
|                                                v                                                
|  +------------------------------------------------------------------------------------------+   
|  |                              Prefix Hash Construction                                    |   
|  |                                                                                          |   
|  |  Tokenize selected prefix window.                                                        |   
|  |  Compute stable hash, e.g.:                                                              |   
|  |                                                                                          |   
|  |      prefix_hash = blake2b(model_id | adapter_id | system_prefix | first_user_prefix)    |   
|  |      candidate_rank = prefix_hash % data_parallel_size                                   |   
|  |                                                                                          |   
|  |  Purpose: keep related turns near the replica holding warm KV / prefix cache blocks.     |   
|  +---------------------------------------------+--------------------------------------------+   
|                                                |                                                
|                                                v                                                
|  +------------------------------------------------------------------------------------------+   
|  |                              Cache-Affinity Lookup                                       |   
|  |                                                                                          |   
|  |  Score candidate replicas by:                                                            |   
|  |  - exact prefix-cache hit                                                                |   
|  |  - partial radix-tree prefix overlap                                                     |   
|  |  - active KV block residency                                                             |   
|  |  - semantic-cache hit, if safe for task                                                  |   
|  |  - adapter already loaded in HBM                                                         |   
|  +---------------------------------------------+--------------------------------------------+   
|                                                |                                                
|                                                v                                                
|  +------------------------------------------------------------------------------------------+   
|  |                              Load-Aware Evaluator                                        |   
|  |                                                                                          |   
|  |  Adjust cache-affinity score using live serving pressure:                                |   
|  |  - queue depth                                                                           |   
|  |  - waiting request count                                                                 |   
|  |  - active decode batch size                                                              |   
|  |  - HBM / KV cache occupancy                                                              |   
|  |  - preemption or recompute rate                                                          |   
|  |  - tenant fairness and reserved headroom                                                 |   
|  |  - p95 / p99 TTFT and inter-token latency                                                |   
|  +---------------------------------------------+--------------------------------------------+   
|                                                |                                                
|                                                v                                                
|  +------------------------------------------------------------------------------------------+   
|  |                         Late-Binding Dispatch Decision                                   |   
|  |                                                                                          |   
|  |  Hold request centrally until a downstream execution slot is actually available.         |   
|  |  Avoid binding early to a node that becomes saturated before the request reaches GPU.    |   
|  +-------------------------+--------------------------+--------------------------+----------+   
|                            |                          |                          |              
|                            v                          v                          v              
|+----------------------------+  +----------------------------+  +----------------------------+   
|| Replica A                  |  | Replica B                  |  | Replica C                  |   
||                            |  |                            |  |                            |   
|| Warm prefix cache: high    |  | Warm prefix cache: medium  |  | Warm prefix cache: low     |   
|| Queue depth: moderate      |  | Queue depth: low           |  | Queue depth: low           |   
|| Adapter resident: yes      |  | Adapter resident: no       |  | Adapter resident: yes      |   
|| HBM pressure: acceptable   |  | HBM pressure: acceptable   |  | HBM pressure: high         |   
|+-------------+--------------+  +-------------+--------------+  +-------------+--------------+   
|              |                               |                               |                  
|              +-------------------------------+-------------------------------+                  
|                                              |                                                  
|                                              v                                                  
|                                  [ Best Replica Selected ]                                      
|                                              |                                                  
|                                              v                                                  
|  +------------------------------------------------------------------------------------------+   
|  |                              Queue / Admission Controller                                |   
|  |                                                                                          |   
|  |  Admit if capacity exists; otherwise apply priority queueing, backpressure, downgrade,   |   
|  |  async deferral, or HTTP 429 / 503 depending on tenant policy and workload class.        |   
|  +---------------------------------------------+--------------------------------------------+   
|                                                |                                                
|                                                v                                                
|                                      [ Inference Server Execution ]                             
|                                                |                                                
|                                                v                                                
|                                  [ Stream Response + Emit Telemetry ]                           
|                                                                                                 
+-------------------------------------------------------------------------------------------------+
| Doctrine: route to the warmest acceptable replica, not merely the emptiest one. Round-robin     |
| ignores prompt length, KV cache locality, adapter residency, and HBM pressure, which is how one |
| gets very expensive chaos with a progress bar.                                                  |
+-------------------------------------------------------------------------------------------------+
```                       

* **Consistent Hashing:** Hashes request parameters (e.g., Tenant ID) to route sequential requests from the same user to the same replica, preserving localized cache affinity.7  
* **Prefix Affinity:** Dynamically routes requests with similar prompt prefixes to the same replica, allowing the engine to reuse prefilled KV states.5  
* **Prefix-Match Hashing:** Tokenizes prompt prefixes and computes a stable hash to assign requests directly to a specific rank.30  
* **Precise Cache-Aware Routing:** Monitored by the load balancer, this tracks physical cache allocations inside GPU memory to route requests to the warmest node.23  
* **Adapter-Aware Dispatch:** Tracks loaded LoRA adapter states to target nodes that already contain the requested adapter in memory, avoiding cold-start load delays.3  
* **Late-Binding Flow Control:** Holds requests in a centralized queue, delaying routing decisions until the last possible millisecond when a downstream slot is verified free.24

#### **The Physics of Prefix Caching**

Prefix-aware load balancing leverages the fact that many LLM workloads share identical system instructions, few-shot examples, tool schemas, output contracts, or conversation-history prefixes. Runtimes such as SGLang and vLLM organize reusable prompt states through radix trees, prefix caches, or hash-indexed KV blocks.

If a request lands on a replica containing matching cached prefix blocks, the runtime can skip prefill for the matched prefix and compute prefill only for the unmatched suffix. This reduces Time to First Token (TTFT), lowers redundant compute, and improves GPU utilization without pretending that dynamic user-specific content becomes free.

The radix tree minimizes memory footprint by ensuring that memory consumption scales with unique content rather than total processed tokens.

Example:

```text
Without prefix reuse:

  Request 1: [800 shared tokens][200 unique tokens]
  Request 2: [800 shared tokens][200 unique tokens]
  Request 3: [800 shared tokens][200 unique tokens]

  Total prefill work:
    3 * 1000 tokens = 3000 token-units


With prefix reuse:

  Shared prefix:
    [800 shared tokens] computed once

  Unique suffixes:
    Request 1: [200 unique tokens]
    Request 2: [200 unique tokens]
    Request 3: [200 unique tokens]

  Total prefill work:
    800 + (3 * 200) = 1400 token-units
```

In this example, prefix reuse reduces prefill work by approximately 53%. The actual savings depend on prompt stability, cache residency, routing affinity, eviction pressure, and whether the request lands on a replica that still holds the matched prefix blocks.

The operational lesson is direct: stable prompt roots create reusable compute. Prompt churn destroys cache locality. System instructions, tool schemas, and long-lived output contracts should appear before dynamic user content whenever the application can safely structure prompts that way.

#### **Hashing and Key Extraction**

To achieve high-efficiency prefix caching, load balancers should route requests with shared stable prefixes to replicas likely to hold the matching KV-cache or radix-cache state. Routing by raw request count is insufficient because two requests with identical compute cost can have very different cache value depending on prefix overlap.

A cache-aware router extracts stable routing fields from the request and constructs a bounded routing key. The routing key should avoid highly volatile content that changes every turn, because over-sensitive hashing sends follow-up requests to cold replicas.

A practical routing-key pattern is:

```text
RoutingKey = Hash(
  model_id,
  adapter_id,
  tenant_scope,
  system_prompt_prefix[0..256],
  first_user_turn_prefix[0..256]
)
```

The proxy may tokenize the selected prefix window before hashing when token-level stability matters:

```text
prefix_tokens = Tokenize(
  system_prompt_prefix[0..256],
  first_user_turn_prefix[0..256]
)

target_rank = Hash(model_id, adapter_id, tenant_scope, prefix_tokens) % data_parallel_size
```

By ignoring later conversation turns, the routing key remains more stable across the session lifecycle. This increases the probability that follow-up requests land on the replica holding warm prefix-cache or KV-cache state.

This does not mean all dynamic content should be ignored for routing. Long-context requests, high-risk tasks, large tool schemas, tenant-isolated workloads, and region-bound data may still require routing overrides. The routing key is a locality heuristic, not a complete authorization or capacity decision.


#### **Prefill-Decode Disaggregation**

When serving heavy workloads, processing prompts (prefills) and generating tokens (decodes) on the same GPU can degrade performance.21 Prefills are compute-bound, saturating FLOPS; decodes are memory-bound, bottlenecked by memory bandwidth.11 Interleaving them on a single node causes head-of-line blocking and latency spikes.21  
Disaggregating these phases onto dedicated prefill and decode pools isolates execution.11 In a disaggregated deployment, a proxy coordinates the multi-step request flow: it dispatches the prompt to a prefill worker, waits for completion, transfers the resulting KV cache to a decode worker, and streams the generated tokens back to the client.21

```
+-------------------------------------------------------------------------------------------------+
|                                  PREFILL-DECODE DISAGGREGATION                                  |
+-------------------------------------------------------------------------------------------------+
|                                                                                                 
|  Goal: separate compute-bound prompt processing from memory-bandwidth-bound token generation.   
|                                                                                                 
|  [ Client Request ]                                                                             
|        |                                                                                        
|        v                                                                                        
|  +------------------------------------------------------------------------------------------+   
|  |                                      Proxy / Router                                      |   
|  |                                                                                          |   
|  |  - selects prefill pool and decode pool                                                  |   
|  |  - reserves decode-side KV blocks                                                        |   
|  |  - coordinates transfer backend                                                          |   
|  |  - streams final tokens back to client                                                   |   
|  +----------------------------+-------------------------------------------------------------+   
|                               |                                                                 
|                               | 1. Bootstrap / preallocate KV                                   
|                               v                                                                 
|  +-------------------------------------------------------------------------------------------+  
|  |                                  Decode Instance Pool                                     |  
|  |                                                                                           |  
|  |  [ Prealloc Queue ] -> [ Transfer Queue ] -> [ Waiting Queue ] -> [ Running Decode Batch ]|  
|  |        ^                     ^                     ^                       |              |  
|  |        |                     |                     |                       |              |  
|  |        |                     |                     |                       v              |  
|  |        |                     |                     |              generate next tokens    |  
|  +--------+---------------------+---------------------+-----------------------+--------------+  
|           ^                     ^                                             |                 
|           |                     | 4. KV received;                             |                 
|           |                     |    request becomes decode-ready             |                 
|           | 2. decode memory    |                                             |                 
|           |    reservation ack  |                                             |                 
|           |                     |                                             v                 
|  +--------+---------------------+------------------------------------------------------------+  
|  |                                  Prefill Instance Pool                                    |  
|  |                                                                                           |  
|  |  [ Bootstrap Queue ] -> [ Waiting Queue ] -> [ Inflight Queue ]                           |  
|  |        |                     |                    |                                       |  
|  |        |                     |                    |                                       |  
|  |        |                     v                    v                                       |  
|  |        |          compute prompt forward pass     monitor KV transfer status              |  
|  |        |                                                                                  |  
|  +--------+----------------------------+-----------------------------------------------------+  
|                                        |                                                        
|                                        | 3. stream / push KV cache blocks                       
|                                        v                                                        
|                         +-------------------------------------------+                           
|                         | KV Transfer Backend                       |                           
|                         |                                           |                           
|                         | RDMA | NVLink | NIXL | Mooncake-style     |                           
|                         | layerwise KV movement into decode memory  |                           
|                         +--------------------+----------------------+                           
|                                              |                                                  
|                                              v                                                  
|                                  [ Decode Pool Resumes Generation ]                             
|                                              |                                                  
|                                              v                                                  
|                                  [ Tokens Streamed to Client ]                                  
|                                                                                                 
+-------------------------------------------------------------------------------------------------+
| Doctrine: prefill is compute-bound; decode is memory-bandwidth-bound. Disaggregation improves   |
| throughput only when KV transfer and decode-side preallocation are cheaper than mixed-node      |
| head-of-line blocking.                                                                          |
+-------------------------------------------------------------------------------------------------+
```

To support disaggregated execution, SGLang manages specialized queues across prefill and decode instances.12

* **Prefill Instance Lifecycles:** Incoming requests enter the *Bootstrap Queue* to execute handshakes and pre-allocate memory on the decode node.12 They transition to the *Waiting Queue* to execute the compute-intensive prefill forward pass.12 Finally, they enter the *Inflight Queue*, where a non-blocking poll monitors the transfer status until the KV cache is fully transmitted to the decoder.12  
* **Decode Instance Lifecycles:** Requests enter the *Prealloc Queue* to initialize the receiver and pre-allocate physical KV blocks.12 They move to the *Transfer Queue* to poll the receiver until weight ingestion is complete.12 Once verified, requests land in the *Waiting Queue* to build a metadata batch that skips the prefill pass, and finally merge into the *Running Batch* for uninterrupted autoregressive token generation.12

These transfers run on high-performance backends like Mooncake (an RDMA-based transfer engine optimized for InfiniBand/RoCE) or NIXL (a UCX/libfabric plugin system).12 In write mode, the proxy dispatches to prefill and decode concurrently; the prefill node pushes the KV cache layer-by-layer directly into the decoder’s pre-allocated memory via RDMA writes as each layer is computed, minimizing transfer overhead.21  
Large-scale Mixture-of-Experts (MoE) deployments (e.g., DeepSeek-V3/R1) combine prefill-decode disaggregation with Wide Expert Parallelism (WideEP).42 These workloads rely on optimized collective communication kernels like DeepEP to handle expert-to-expert token routing, dual-batch overlap (DBO) to mask MoE dispatch overheads, and Expert Parallel Load Balancing (EPLB) weight shuffles to balance token routing skew across physical GPUs.42

## **Tenant-Aware Capacity & Architectural Isolation**

Multi-tenant enterprise serving architectures must protect hardware pools from the noisy-neighbor problem, where a single high-volume customer saturates shared GPU batch slots.2

### **Tenant-Aware Capacity Model**

```
+----------------------------------------------------------------------------------------------------+
|                                  TENANT-AWARE CAPACITY MODEL                                       |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    
|  Goal: prevent one tenant from starving shared GPU capacity, queue slots, HBM, PCIe bandwidth,     
|  or premium model access.                                                                          
|                                                                                                    
|  [ Incoming Request ]                                                                              
|        |                                                                                           
|        v                                                                                           
|  +------------------------------------------------------------------------------------------+      
|  |                                  API Key Lookup                                          |      
|  |                                                                                          |      
|  |  Resolve opaque key -> tenant ID, plan, priority tier, region, allowed models, budgets.  |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                              Tenant Policy Resolution                                    |      
|  |                                                                                          |      
|  |  Load: RPM limit | TPM limit | concurrency cap | spend ceiling | model entitlement       |      
|  |        data residency | isolation requirement | fallback tier | queue priority           |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                          Atomic Limit / Quota Enforcement                                |      
|  |                                                                                          |      
|  |  Redis token buckets and Lua locks enforce:                                              |      
|  |  - requests per minute                                                                   |      
|  |  - tokens per minute                                                                     |      
|  |  - active concurrent requests                                                            |      
|  |  - daily / monthly budget ceilings                                                       |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                      [ Limit Exceeded? ]                                           
|                                         /             \                                            
|                                  Yes   /               \   No                                      
|                                       v                 v                                          
|  +-----------------------------------------+     +--------------------------------------------+    
|  |       Backpressure / Rejection Path     |     |       Weighted Fair Admission Path         |    
|  |                                         |     |                                            |    
|  |  - HTTP 429 for rate-limit breach       |     |  - assign tenant priority class            |    
|  |  - HTTP 402 / budget error if exhausted |     |  - reserve headroom for premium tiers      |    
|  |  - downgrade route if policy permits    |     |  - place request in fair-share queue       |    
|  |  - async deferral for batch workload    |     |  - prevent noisy-neighbor starvation       |    
|  +--------------------+--------------------+     +-------------+------------------------------+    
|                       |                                        |                                   
|                       v                                        v                                   
|              [ Return Clean Error                  +-------------------------------+                
|                or Degraded Route ]                 | Capacity Classifier           |                
|                                                    |                               |               
|                                                    | - interactive vs batch        |               
|                                                    | - low-risk vs high-risk       |               
|                                                    | - premium vs standard tier    |               
|                                                    | - isolation requirement       |               
|                                                    +---------------+---------------+               
|                                                                    |                               
|                                                                    v                               
|  +---------------------------------------------------------------------------------------------+   
|  |                         Logical Scheduling / Queue Isolation                                |   
|  |                                                                                             |   
|  |  Weighted fair queues | priority lanes | per-tenant concurrency slots | reserved headroom   |   
|  |  circuit breakers    | retry limits   | agent recursion limits       | downgrade policy     |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Shared Standard Pool    |  | Premium Low-Latency   |  | Isolated / Regulated Pool   |   
|          |                         |  | Pool                  |  |                             |   
|          | fair-share scheduling   |  | reserved queue slots  |  | tenant-specific routing     |   
|          | small / medium models   |  | warm frontier models  |  | private region / VPC / node |   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      +----------------------------+----------------------------+                   
|                                                   |                                                
|                                                   v                                                
|  +------------------------------------------------------------------------------------------+      
|  |                              Hardware Isolation Layer                                    |      
|  |                                                                                          |      
|  |  Select physical placement based on tenant class and workload pressure:                  |      
|  |                                                                                          |      
|  |  - MIG partitioning: hard GPU/HBM partition for stronger isolation                       |      
|  |  - CUDA MPS allocation: dynamic SM and memory-share control                              |      
|  |  - PCIe-aware placement: avoid shared-bus contention and NUMA hot spots                  |      
|  |  - adapter / cache affinity: route to nodes with warm LoRA or prefix cache state         |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|                                     [ Inference Execution ]                                        
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                               Usage Accounting & Telemetry                               |      
|  |                                                                                          |      
|  |  Record tenant ID, queue wait, TTFT, ITL, tokens, GPU pool, cache hits, failures, spend. |      
|  |  Feed metrics back into quota state, billing, autoscaling, abuse detection, and SLOs.    |      
|  +------------------------------------------------------------------------------------------+      
|                                                                                                    
+----------------------------------------------------------------------------------------------------+
| Doctrine: tenant isolation starts at the gateway but does not end there. Logical quotas prevent    |
| request floods; physical placement prevents one tenant from ruining GPU, HBM, PCIe, and tail       |
| latency for everyone else.                                                                         |
+----------------------------------------------------------------------------------------------------+   
```

* **Opaque Key Verification:** Resolves tenant identity at the gateway using hashed keys, matching requests to billing plans and routing tables.  
* **Token-Bucket Limits:** Enforces usage limits using Redis-backed sorted sets, preventing extreme token or request spikes.  
* **Weighted Fair Queues:** Sorts requests by priority and tenant tier, guaranteeing that premium users bypass background processing queues.2  
* **CUDA MPS Allocation:** Logically partitions GPU Streaming Multiprocessors (SMs) and memory limits, ensuring that tenant containers run isolated on shared hardware.2  
* **MIG Partitioning:** Provides physical, hardware-level isolation by partitioning the GPU silicon and memory bus into distinct, independent instances.10  
* **PCIe-Aware Placement:** Tracks host-level link contention, dynamically migrating active processes across physical cards to avoid PCIe bus hotspots.10

#### **Atomic Token-Locking to Prevent TOCTOU Races**

To prevent Time-of-Check to Time-of-Use (TOCTOU) races in high-concurrency environments, concurrency-slot acquisition must be performed as a single atomic operation. Multiple parallel requests must not be able to read the same active slot count before any of them writes the increment.

A Redis Lua script can acquire a tenant concurrency slot atomically:

```lua
-- Acquire a tenant concurrency slot.
-- KEYS[1] = tenant concurrency key
-- ARGV[1] = concurrency limit
-- ARGV[2] = slot TTL in seconds

local key = KEYS[1]
local limit = tonumber(ARGV[1])
local ttl = tonumber(ARGV[2])

local current = tonumber(redis.call("GET", key) or "0")

if current < limit then
    local next_count = redis.call("INCR", key)

    -- Ensure the key eventually clears even if a worker dies before releasing.
    if next_count == 1 then
        redis.call("EXPIRE", key, ttl)
    end

    return 1 -- Slot acquired.
else
    return 0 -- Limit exceeded.
end
```

The slot must also be released when the request completes, fails, times out, or is cancelled:

```lua
-- Release a tenant concurrency slot.
-- KEYS[1] = tenant concurrency key

local key = KEYS[1]
local current = tonumber(redis.call("GET", key) or "0")

if current <= 1 then
    redis.call("DEL", key)
    return 0
else
    return redis.call("DECR", key)
end
```

The acquisition script prevents burst traffic from bypassing concurrency limits. The release script prevents completed or cancelled requests from permanently occupying slots. The TTL protects against leaked slots when workers crash before cleanup.

This pattern should be paired with request IDs and idempotency keys when clients may retry. Without idempotency handling, a retry storm can acquire multiple legitimate slots for the same logical user operation.

#### **Hardware Isolation: MIG, MPS, and PCIe Contention**

While logical separation at the proxy protects queuing layers, heavy workloads can degrade performance at the physical layer.10 When multiple containers run concurrently on a shared GPU, they can trigger resource contention along the shared Peripheral Component Interconnect Express (PCIe) fabric and Non-Uniform Memory Access (NUMA) domains, causing tail latency spikes and SLO violations.10  
To achieve deterministic execution, systems implement physical hardware isolation strategies:

* **Multi-Instance GPU (MIG):** Physically partitions the GPU silicon, HBM memory, and internal paths into distinct hardware instances.10 While MIG provides absolute compute and memory isolation, it does not isolate the shared PCIe bus, meaning intensive data transfers from background jobs can still bottleneck latency-sensitive inference runs.10  
* **Multi-Process Service (MPS):** Provides logical, software-level partitioning of Streaming Multiprocessors (SMs) and memory limits.2 It is highly dynamic but offers weaker physical isolation than MIG.25  
* **PCIe-Aware Placement:** A host-level controller monitors tail latencies and PCIe bandwidth usage in a real-time feedback loop.10 If a tenant’s p99 latency exceeds its Service Level Objective (SLO), the controller migrates processes across physical cards, pinning process affinities away from IRQ-heavy host CPU cores and busy PCIe root switches.25 Combined with dynamic MIG reconfiguration and cgroup disk limits (io.max throttles), this optimization reduces SLO miss rates by approximately 32% and improves p99 TTFT tails by up to 15% under heavy I/O contention.10

## **Backpressure, Admission Control, and Quota Management**

When request volume outpaces available hardware capacity, model serving architectures must implement backpressure to protect GPUs from catastrophic failures and memory exhaustion.3 Admission control shifts the burden of queuing from the isolated model servers to the gateway.24

### **Backpressure and Admission Control Framework**

| Backpressure Strategy | Triggering Condition | Technical Implementation | Downstream Impact |
| :---- | :---- | :---- | :---- |
| **Request Shedding** | Gateway queues or target backend pools exceed maximum wait times.3 | Rejects incoming requests immediately with an HTTP 503 error.3 | Drops low-priority traffic to protect active interactive streams.3 |
| **Priority Queuing** | Active backend pools report high HBM cache pressure.24 | Re-orders requests in the proxy plane using fair-share queues.24 | Automatically pauses background jobs during interactive traffic peaks.24 |
| **Constraint Clipping** | Incoming prompt lengths exceed target context windows.9 | Rejects payloads or truncates prompts before HBM allocation.8 | Avoids out-of-memory (OOM) failures on physical nodes.8 |
| **Automatic Downgrade** | Primary model execution fails or reports high queue latencies.8 | Redirects the active request stream to a smaller model replica.8 | Retries with a faster, cheaper local model to preserve availability.8 |
| **Async Deferral** | Saturated queues during peak interactive usage.3 | Bypasses the synchronous path; pushes payloads to an offline queue.3 | Delays batch processing to guarantee low latency for active users.3 |
| **Fail-Closed Path** | Detected security risks, template drift, or PII leaks.8 | Abruptly terminates the active execution thread and logs the error.8 | Blocks execution; returns a standardized HTTP 400 error. |

To enforce usage policies, platforms deploy rate limiters that track requests and dynamic execution metrics.

### **Rate Limit and Quota Model**

```
+----------------------------------------------------------------------------------------------------+
|                                  RATE LIMIT AND QUOTA MODEL                                        |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    
|  Goal: enforce tenant usage limits before requests consume scarce GPU, queue, KV-cache,            
|  tool-call, or budget resources.                                                                   
|                                                                                                    
|  [ Incoming Request ]                                                                              
|        |                                                                                           
|        v                                                                                           
|  +------------------------------------------------------------------------------------------+      
|  |                              Tenant Identity Resolution                                  |      
|  |                                                                                          |      
|  |  API key -> tenant ID | plan | priority tier | region | model entitlement | billing scope|      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                              Usage Policy Lookup                                         |      
|  |                                                                                          |      
|  |  Load active limits for the tenant:                                                      |      
|  |                                                                                          |      
|  |  - requests per minute                                                                   |      
|  |  - tokens per minute                                                                     |      
|  |  - concurrent requests                                                                   |      
|  |  - agent recursion / tool-call depth                                                     |      
|  |  - daily, monthly, or contract budget ceiling                                            |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +---------------------------------------------------------------------------------------------+   
|  |                              Atomic Usage Check                                             |   
|  |                                                                                             |   
|  |  Redis / durable counter layer applies token buckets, sliding windows, and Lua locks        |   
|  |  so high-concurrency requests cannot bypass limits through TOCTOU races.                    |   
|  +----------------------+----------------------+----------------------+------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | RPM Meter               |  | TPM Meter             |  | Concurrency Meter           |   
|          |                         |  |                       |  |                             |   
|          | Sliding-window request  |  | Input + output token  |  | Active in-flight requests   |   
|          | count per tenant / key  |  | budget per time slice |  | and reserved execution slots|   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      v                            v                            v                   
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Agent Step Meter        |  | Budget Meter          |  | Abuse / Retry Meter         |   
|          |                         |  |                       |  |                             |   
|          | Max tool calls, loops,  |  | Daily / monthly spend |  | Retry storms, recursion,    |   
|          | recursive turns, and    |  | ceiling and per-query |  | burst spikes, suspicious    |   
|          | workflow depth          |  | cost estimate         |  | usage patterns              |   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      +----------------------------+----------------------------+                   
|                                                   |                                                
|                                                   v                                                
|                                      [ Any Limit Exceeded? ]                                       
|                                             /             \                                        
|                                      Yes   /               \   No                                  
|                                           v                 v                                      
|  +--------------------------------------------+    +---------------------------------------------+ 
|  |             Backpressure Path              |    |             Admission Path                  | 
|  |                                            |    |                                             | 
|  |  Select response by violation type:        |    |  Reserve counters and execution slot:       | 
|  |                                            |    |                                             | 
|  |  - HTTP 429: rate or concurrency exceeded  |    |  - increment request counter                | 
|  |  - HTTP 402 / budget error: spend ceiling  |    |  - reserve estimated token budget           | 
|  |  - HTTP 400: context or policy violation   |    |  - acquire concurrency slot                 | 
|  |  - HTTP 503: backend saturation            |    |  - assign queue priority                    | 
|  |  - downgrade route if policy permits       |    |  - attach tenant and billing metadata       | 
|  |  - async deferral for batch work           |    |  - forward to queue / router / executor     | 
|  +----------------------+---------------------+    +----------------------+----------------------+ 
|                         |                                           |                              
|                         v                                           v                              
|          [ Clean Error / Degraded Route ]              [ Queue / Routing / Execution ]             
|                         |                                           |                              
|                         +--------------------+----------------------+                              
|                                              |                                                     
|                                              v                                                     
|  +------------------------------------------------------------------------------------------+      
|  |                              Usage Finalization                                          |      
|  |                                                                                          |      
|  |  After execution, reconcile actual input tokens, generated tokens, tool calls, latency,  |      
|  |  retries, GPU pool, cache hits, and final cost against reserved quota.                   |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                              Telemetry / Billing / Abuse Feedback                        |      
|  |                                                                                          |      
|  |  Emit tenant usage records for billing, dashboards, quota updates, autoscaling, anomaly  |      
|  |  detection, and circuit-breaker policy.                                                  |      
|  +------------------------------------------------------------------------------------------+      
|                                                                                                    
+----------------------------------------------------------------------------------------------------+
| Doctrine: rate limits protect the request path; quotas protect the business contract;              |
| concurrency limits protect execution slots; budget ceilings protect the wallet from catching       |
| fire and calling it innovation.                                                                    |
+----------------------------------------------------------------------------------------------------+
```                 

* **Requests Per Minute (RPM):** Monitored per tenant using sliding-window Redis sorted sets, rejecting bursts exceeding plans.  
* **Tokens Per Minute (TPM):** Tracks processed input and generated output tokens, soft-rejecting requests when limits are hit.  
* **Concurrent Requests:** Caps in-flight requests per user, holding excess calls in central queues to prevent thread exhaustion.  
* **Agent Recursion Limits:** Caps maximum sequential steps and tool executions inside agentic loops, halting runaway processes.  
* **Budget Ceilings:** Tracks calculated dollar costs per query, enforcing hard stops when monthly or daily spending limits are reached.

When backend systems are overloaded, poorly configured clients can trigger retry storms, compounding system pressure.28 To mitigate this risk, gateways enforce exponential backoff with randomized jitter and deploy circuit breakers that temporarily stop routing traffic to degraded zones, allowing them to recover.6

## **Elastic Scaling & Cold-Start Optimization**

Autoscaling for GPU workloads differs fundamentally from CPU-based services.9 While standard Horizontal Pod Autoscalers (HPAs) trigger on CPU or system memory footprints, GPU runtimes saturate execution cores while keeping CPU metrics low, meaning CPU-based scaling fails to react before users experience massive latency spikes.9  
Autoscalers must evaluate GPU-native signals to scale capacity proactively.

### **Autoscaling and Cold-Start Model**

| Scaling Phase | Primary Metrics | Technical Execution | Latency Impact |
| :---- | :---- | :---- | :---- |
| **Metric Extraction** | vllm:num_requests_waiting and gpu_cache_usage_perc.9 | Scrapes Prometheus-format endpoints via DCGM and engine monitors.9 | Proactively triggers scale actions before queue saturation degrades TTFT.9 |
| **Node Provisioning** | Compute resource requests and pending replica counts.7 | Karpenter on EKS provisions new GPU nodes in ~60 seconds.9 | Introduces structural lag; requires pre-allocated warm pools to buffer spikes.9 |
| **Weight Loading** | Weights size and network bandwidth availability.8 | Fetches model files from local registries; skips remote CDN retrievals.8 | Eliminates variable WAN download latencies; guarantees fast weight transfers.8 |
| **Runtime Init** | CUDA memory allocation and library load states.43 | Mounts images, loads models, and initializes the CUDA runtime.43 | Takes 10–30 seconds to initialize complex libraries. |
| **Optimized Capture** | Kernel compilation and CUDA graph capture steps.26 | Skip compilation with --enforce-eager or load templates via Foundry.26 | Cuts cold-start compilation overhead from minutes to under 4 seconds.26 |
| **Route Registration** | Host-level health-check passing states.6 | Verifies server readiness via mock runs before updating router tables.6 | Verifies node health before exposing it to active traffic.8 |

To protect users from cold-start latencies, architectures implement model residency strategies that map models to warm, on-demand, or batch tiers.

### **Model Residency Strategy**

```
+---------------------------------------------------------------------------------------------------+
|                                      MODEL RESIDENCY STRATEGY                                     |
+---------------------------------------------------------------------------------------------------+
|                                                                                                   
|  Goal: decide which model artifacts stay hot in GPU memory, which remain warm nearby, and         
|  which are loaded only when demand justifies the cold-start cost.                                 
|                                                                                                   
|  +------------------------------------------------------------------------------------------+     
|  |                                Model Registry / Artifact Store                           |     
|  |                                                                                          |     
|  |  SHA-pinned base models | quantized variants | LoRA adapters | CUDA templates | snapshots|     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                                v                                                  
|  +------------------------------------------------------------------------------------------+     
|  |                              Residency Policy Controller                                 |     
|  |                                                                                          |     
|  |  Inputs: traffic frequency | latency SLA | model size | tenant tier | cold-start cost    |     
|  |          GPU supply | adapter demand | batch schedule | fallback requirement             |     
|  +----------------------+----------------------+----------------------+---------------------+     
|                         |                      |                      |                           
|                         v                      v                      v                           
|          +-------------------------+  +-----------------------+  +-----------------------------+  
|          | Tier 1: Hot Residency   |  | Tier 2: Warm Residency |  | Tier 3: On-Demand Residency|  
|          |                         |  |                       |  |                             |  
|          | Frontier / primary      |  | Parent model resident  |  | Quantized specialists,     |  
|          | models kept loaded in   |  | in HBM; adapters,      |  | rarely used tools, regional|  
|          | GPU HBM with warmed     |  | CUDA graphs, tokenizer |  | models, overflow capacity. |  
|          | kernels and captured    |  | state, and snapshots   |  | Loaded into HBM on first   |  
|          | CUDA graphs.            |  | kept close to runtime. |  | qualifying request.        |  
|          |                         |  |                       |  |                             |  
|          | Best for: low-latency   |  | Best for: dynamic      |  | Best for: medium-latency   |  
|          | interactive traffic.    |  | LoRA / adapter traffic.|  | specialized routes.        |  
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+  
|                      |                            |                            |                  
|                      |                            |                            v                  
|                      |                            |               +-----------------------------+ 
|                      |                            |               | Cold-Start Mitigation       | 
|                      |                            |               |                             | 
|                      |                            |               | local weight cache          | 
|                      |                            |               | snapshot restore            | 
|                      |                            |               | precompiled templates       | 
|                      |                            |               | eager startup mode          | 
|                      |                            |               +-------------+---------------+ 
|                      |                            |                             |                 
|                      +----------------------------+-----------------------------+                 
|                                                   |                                               
|                                                   v                                               
|  +------------------------------------------------------------------------------------------+     
|  |                              Tier 4: Async / Batch Residency                             |     
|  |                                                                                          |     
|  |  Offline models loaded only for scheduled jobs, bulk evaluation, embedding refreshes,    |     
|  |  backfills, nightly processing, or non-interactive tenant workloads.                     |     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                                v                                                  
|  +------------------------------------------------------------------------------------------+     
|  |                              Router / Autoscaler Feedback                                |     
|  |                                                                                          |     
|  |  Promote models upward when request volume, SLA pressure, or fallback importance rises.  |     
|  |  Demote models downward when traffic falls, GPU pressure spikes, or cache value decays.  |     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                                v                                                  
|                                  [ Active GPU Pool Allocation ]                                   
|                                                                                                   
+---------------------------------------------------------------------------------------------------+
| Residency rule: keep latency-critical and high-frequency models hot; keep likely specialists      |
| warm; load rare models on demand; push non-interactive work into async batch lanes.               |
+---------------------------------------------------------------------------------------------------+
```

To optimize serverless cold starts, runtimes trade peak execution performance for fast startup times.43 For example, launching vLLM with the --enforce-eager flag skips time-consuming compiler optimizations and CUDA graph captures, reducing cold starts from 460 seconds to 219 seconds.43  
For massive deployments, context materialization platforms (such as Foundry) persist compiled execution templates and GPU state layouts offline, restoring them during scale-up to cut the initialization time of a 235B parameter MoE model from 10 minutes to 3.9 seconds.26 Some platforms also utilize snapshot-restore features, freezing a warmed GPU instance to disk and restoring its memory state on demand to bypass the initialization path entirely.43

## **Deployment Topologies: Cloud, On-Prem, Edge, Local, and Hybrid**

Selecting a deployment topology requires balancing tradeoffs across compliance, latency, cost, and maintenance complexity. The topology matrix below details these architectural options.

### **Deployment Topology Matrix**

| Topology | Capability Access | Latency Footprint | Operational Complexity | Cost Economics | Failure Modes |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Managed APIs** | Immediate access to state-of-the-art frontier models.4 | High variability; vulnerable to WAN network jitter and rate limits.1 | Lowest; vendor manages scaling and infrastructure.4 | Pay-per-token pricing; expensive at sustained volumes.4 | Provider outages, quiet version updates, and pricing shifts.4 |
| **Cloud Self-Hosted** | High; deploy any custom-trained open weights.4 | Lowest latency via dedicated GPU nodes.9 | High; requires cluster scheduling and engineering.9 | Fixed hourly GPU costs; cheap at high, sustained usage.4 | Memory leaks, OOM crashes, and capacity provisioning delays.8 |
| **Private Cloud** | Custom limits based on internal hardware. | Low; constrained by localized interconnects. | Very High; requires dedicated system admins. | Capital expenditure model; high upfront costs. | Internal network congestion and hardware component failures. |
| **On-Premise** | Absolute control; custom model architectures. | Sub-millisecond localized execution. | Highest; team manages hardware lifecycle. | Maximum upfront CAPEX; zero recurring cloud fees. | Physical cooling faults and local power interruptions. |
| **Edge Deployment** | Constrained to small model footprints.4 | Deterministic localized latency. | High; sync configurations across distributed fleets. | Lowest; uses local client compute resources. | Local memory limits and update sync failures. |
| **Hybrid Systems** | Flexible model selection across pools.4 | Dynamic; route dependent.4 | High; requires unified gateways and routers.18 | Optimized; balances cheap local with costly remote.4 | Routing regressions and cascading failures.4 |

## **High Availability, Failover, and Degraded Modes**

Architectures must isolate failures proactively to preserve user-facing SLAs.8 If a component degrades, the serving platform must transition traffic immediately to healthy backups.27

```
+------------------------------------------------------------------------------------------------+
|                         HIGH AVAILABILITY, FAILOVER, AND DEGRADED MODES                        |
+------------------------------------------------------------------------------------------------+
|                                                                                                
|  Goal: preserve service continuity when a serving component, model route, region, GPU pool,    
|  provider, registry, cache layer, or telemetry path degrades.                                  
|                                                                                                
|  [ Live Production Traffic ]                                                                   
|        |                                                                                       
|        v                                                                                       
|  +------------------------------------------------------------------------------------------+  
|  |                         Health Checks / Telemetry Watchers                               |  
|  |                                                                                          |  
|  |  Monitor: API latency | router timeouts | queue depth | GPU OOM | registry failures      |  
|  |           cache loss | provider errors | P95/P99 TTFT | validation failures              |  
|  +---------------------------------------------+--------------------------------------------+  
|                                                |                                               
|                                                v                                               
|                                      [ Component Healthy? ]                                    
|                                         /             \                                        
|                                  Yes   /               \   No                                  
|                                       v                 v                                      
|                          [ Continue Primary Route ]     +-----------------------------------+  
|                                                         | Trip Circuit Breaker              |  
|                                                         | isolate unhealthy component       |  
|                                                         | stop routing new traffic there    |  
|                                                         +----------------+------------------+  
|                                                                          |                     
|                                                                          v                     
|+------------------------------------------------------------------------------------------+    
||                              Failure Classification                                      |    
|+----------------------+----------------------+----------------------+---------------------+    
|                       |                      |                      |                          
|                       v                      v                      v                          
|        +-------------------------+  +-----------------------+  +-----------------------------+ 
|        | Runtime / GPU Failure   |  | Route / Provider      |  | Registry / Artifact Failure | 
|        |                         |  | Failure               |  |                             | 
|        | OOM, SIGSEGV, HBM       |  | router timeout, API   |  | missing weights, checksum   | 
|        | pressure, preemption    |  | outage, region drop   |  | mismatch, mount timeout     | 
|        +-----------+-------------+  +-----------+-----------+  +-------------+---------------+ 
|                    |                            |                            |                 
|                    v                            v                            v                 
|          [ Backup GPU Pool ]       [ Secondary Route / Region ]     [ Previous Stable SHA ]    
|                      |                            |                            |               
|                      +----------------------------+----------------------------+               
|                                                   |                                            
|                                                   v                                            
|                                      [ Capacity Available? ]                                   
|                                         /             \                                        
|                                  Yes   /               \   No                                  
|                                       v                 v                                      
|  +-----------------------------------------+     +-------------------------------------------+ 
|  |             Failover Route              |     |              Degraded Mode                | 
|  |                                         |     |                                           | 
|  |  - reroute to healthy replica           |     |  - downgrade to smaller model             | 
|  |  - shift to secondary region            |     |  - disable tools or long-context mode     | 
|  |  - use external provider fallback       |     |  - return cached-safe response            | 
|  |  - rollback to prior model version      |     |  - async defer batch or low-priority work | 
|  |  - preserve tenant and trace metadata   |     |  - shed traffic with clean 429 / 503      | 
|  +--------------------+--------------------+     +----------------------+--------------------+ 
|                       |                                                 |                      
|                       +--------------------+----------------------------+                      
|                                            |                                                   
|                                            v                                                   
|  +------------------------------------------------------------------------------------------+  
|  |                              Response Preservation                                       |  
|  |                                                                                          |  
|  |  Stream response if safe; otherwise return standardized degraded-service message with    |  
|  |  retry guidance, request ID, and trace linkage.                                          |  
|  +---------------------------------------------+--------------------------------------------+  
|                                                |                                               
|                                                v                                               
|  +------------------------------------------------------------------------------------------+  
|  |                              Recovery Controller                                         |  
|  |                                                                                          |  
|  |  Restart containers | restore snapshots | resync registry | warm replacement nodes       |  
|  |  rehydrate cache    | verify health     | run smoke tests | reopen circuit gradually     |  
|  +---------------------------------------------+--------------------------------------------+  
|                                                |                                               
|                                                v                                               
|  +------------------------------------------------------------------------------------------+  
|  |                              Post-Failover Telemetry                                     |  
|  |                                                                                          |  
|  |  Log failure cause, affected tenants, fallback route, degraded-mode duration, latency,   |  
|  |  cost impact, validation results, and rollback / recovery actions.                       |  
|  +------------------------------------------------------------------------------------------+  
|                                                                                                
+------------------------------------------------------------------------------------------------+
| Doctrine: failover should isolate the failing component, preserve the user-facing workflow when|
| possible, degrade gracefully when capacity is constrained, and never require live traffic to   |
| discover whether the replacement path works.                                                   |
+------------------------------------------------------------------------------------------------+
```

### **High-Availability and Failover Model**

| Component | Primary Failure Mode | Immediate Detection Vector | Failover Strategy | Recovery Execution |
| :--- | :--- | :--- | :--- | :--- |
| **Inference Server** | GPU out-of-memory, CUDA fault, kernel crash, process exit, or unrecoverable preemption loop. | DCGM memory metrics, worker exit codes, SIGSEGV logs, OOM events, health probe failure. | Trip circuit breaker; stop routing new traffic to failed worker; shift queue to backup pool. | Restart container, lower context/batch pressure if needed, run smoke test, then reopen gradually. |
| **Model Registry** | Network timeout, checksum mismatch, stale artifact, unavailable registry, or failed mount. | Failed file-system mount, artifact checksum mismatch, missing model SHA, registry timeout. | Use warm snapshot or previous stable SHA; block new artifact promotion. | Resync registry, verify checksums, invalidate stale local cache, and restore normal promotion path. |
| **Artifact Store** | Weight files, tokenizer bundles, adapters, templates, or snapshots unavailable during scale-up. | Startup timeout, failed download, missing adapter, corrupted cache, readiness probe failure. | Keep prior active route; use locally cached artifact if checksum-valid. | Repair store, rehydrate local caches, rerun compatibility gate, and resume autoscaling. |
| **Model Router** | Classifier timeout, semantic-router failure, policy-table bug, or canary misrouting. | Router timeout, route-confidence collapse, validation failures, fallback spike, cost-per-success regression. | Bypass classifier; pin affected traffic to tenant-approved safe baseline route. | Roll back router policy, replay recent traces, restore canary only after validation. |
| **Load Balancer** | Loss of cache-affinity telemetry, bad endpoint state, or dispatch imbalance. | Empty backend metrics, prefix-hit collapse, uneven queue depth, p95/p99 latency skew by replica. | Fall back to least-loaded routing while preserving tenant and policy constraints. | Resync endpoint state, restore cache telemetry, gradually re-enable prefix-aware routing. |
| **API Gateway** | Edge-to-cloud disconnect, TLS/auth failure, regional outage, or gateway overload. | Downstream connection drops, client timeout spike, auth error surge, gateway saturation. | Route to geo-redundant secondary region or return clean 503 when no safe route exists. | Re-establish DNS and ingress paths, validate auth, then shift traffic back gradually. |
| **Telemetry Collector** | Shared buffer memory exhaustion, collector overload, or downstream metrics-store outage. | Rising packet drops, trace export latency, missing spans, collector queue growth. | Samples or drops noncritical telemetry while preserving audit-critical traces and raising an alert. | Restarts or scales collector pods, drains queued logs, and verifies audit-critical traces before clearing incident state. |
| **Validator / Policy Checker** | Schema validator, safety checker, grounding verifier, or action gate unavailable. | Validation timeout, retry storm, fail-closed spike, tool-call blockage. | Disable affected low-risk feature or fail closed for high-risk workflows. | Restore validator, replay held traces, confirm baseline failure rate before reopening. |
| **External Provider Route** | Provider outage, quota exhaustion, WAN failure, region block, or silent degradation. | Provider error codes, latency spike, degraded response quality, circuit-breaker trip. | Shift to approved secondary provider, local model, or degraded mode. | Verify provider health, compare output quality, and reopen through canary. |

High availability is not merely replica count. A serving platform is highly available only when it can detect failure, isolate the failing component, preserve safe user workflows, and recover without requiring live traffic to discover whether the replacement path works.

### **Degraded-Mode Serving Pattern Library**

```
+----------------------------------------------------------------------------------------------------+
|                              DEGRADED-MODE SERVING PATTERN LIBRARY                                 |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    
|  Goal: preserve essential user workflows during partial capacity, provider, model, storage,        
|  cache, tool, or validation failures.                                                              
|                                                                                                    
|  [ Active Capacity / Reliability Constraint ]                                                      
|        |                                                                                           
|        v                                                                                           
|  +------------------------------------------------------------------------------------------+      
|  |                              Constraint Classifier                                       |      
|  |                                                                                          |      
|  |  Identify: saturation | outage | storage failure | cache loss | tool failure | risk level|      
|  |            affected tenants | affected models | expected duration | SLA impact           |      
|  +----------------------+----------------------+----------------------+---------------------+      
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Primary Route Saturated |  | Local GPU / Region    |  | Dynamic Storage / Registry  |   
|          |                         |  | Outage                |  | Failure                     |   
|          | queue depth high        |  | unhealthy GPU pool    |  | weights unavailable         |   
|          | TTFT / ITL rising       |  | zone unavailable      |  | adapter load failure        |   
|          | HBM pressure high       |  | provider errors       |  | checksum / mount failure    |   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      v                            v                            v                   
|          [ Model Downgrade ]       [ Provider / Region Fallback ]  [ Previous Stable Artifact ]    
|                      |                            |                            |                   
|                      |                            |                            |                   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|          | Use smaller or cheaper  |  | Redirect to healthy   |  | Roll back to known-good     |   
|          | model tier; reduce      |  | region, external API, |  | model SHA, warm snapshot,   |   
|          | max tokens, context,    |  | or backup GPU pool.   |  | or cached local artifact.   |   
|          | or reasoning effort.    |  |                       |  |                             |   
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|                         |                      |                      |                            
|                         v                      v                      v                            
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|          | Cache / Retrieval Loss  |  | Tool / Validator      |  | Queue Saturation / Burst    |   
|          |                         |  | Failure               |  | Traffic                     |   
|          | semantic cache down     |  | tool gateway down     |  | request spike               |   
|          | prefix cache cold       |  | schema validator down |  | retry storm                 |   
|          | retrieval unavailable   |  | policy checker down   |  | batch jobs competing        |   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|                      |                            |                            |                   
|                      v                            v                            v                   
|          [ Cached-Safe / Direct ]   [ Feature Clipping ]        [ Async Deferral / Shedding ]      
|                      |                            |                            |                   
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+   
|          | Return verified cached  |  | Disable affected      |  | Move low-priority work to   |   
|          | responses when exact    |  | tools, long-context   |  | batch lane, enforce 429 /   |   
|          | match is safe; otherwise|  | features, autonomous  |  | 503, preserve premium and   |   
|          | bypass cache and warn.  |  | actions, or strict    |  | interactive headroom.       |   
|          |                         |  | modes as needed.      |  |                             |   
|          +-------------------------+  +-----------------------+  +-----------------------------+   
|                                                   |                                                
|                                                   |                                                
|                                                   v                                                
|  +------------------------------------------------------------------------------------------+      
|  |                              Risk and Safety Gate                                        |      
|  |                                                                                          |      
|  |  Low-risk workflow  -> continue in degraded mode with visible limitations.               |      
|  |  High-risk workflow -> fail closed, human review, or require retry after recovery.       |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                              User-Facing Response Policy                                 |      
|  |                                                                                          |      
|  |  - stream degraded answer if safe                                                        |      
|  |  - disclose reduced capability when relevant                                             |      
|  |  - attach request ID / trace ID                                                          |      
|  |  - avoid silent downgrade for high-stakes tasks                                          |      
|  |  - return clean 429 / 503 / validation error when service cannot safely continue         |      
|  +---------------------------------------------+--------------------------------------------+      
|                                                |                                                   
|                                                v                                                   
|  +------------------------------------------------------------------------------------------+      
|  |                              Recovery and Exit Criteria                                  |      
|  |                                                                                          |      
|  |  Restore normal route only after health checks, smoke tests, capacity recovery, registry |      
|  |  validation, cache rehydration, and telemetry stability confirm the primary path is safe.|      
|  +------------------------------------------------------------------------------------------+      
|                                                                                                    
+----------------------------------------------------------------------------------------------------+
| Degraded-mode rule: reduce capability before losing availability, but fail closed whenever the     |
| degraded path cannot preserve safety, correctness, privacy, or contractual boundaries.             |
+----------------------------------------------------------------------------------------------------+
```

## **Serving Observability and Operations**

Observability in model serving must look beyond host-level CPU and memory footprints.3 Trace records must connect physical hardware performance back to active model versions and tenant identities, allowing SREs to isolate regressions instantly.1

```
+--------------------------------------------------------------------------------------------------+
|                              SERVING OBSERVABILITY AND OPERATIONS                                |
+--------------------------------------------------------------------------------------------------+
|                                                                                                  
|  Goal: connect user-facing behavior, model versions, tenant identity, routing decisions,         
|  runtime state, GPU pressure, validation results, and recovery actions into one traceable view.  
|                                                                                                  
|  [ Live Inference Request ]                                                                      
|        |                                                                                         
|        v                                                                                         
|  +-------------------------------------------------------------------------------------------+   
|  |                              Trace Context Envelope                                       |   
|  |                                                                                           |   
|  |  request_id | session_id | tenant_id | priority_tier | region | route_id | model_target   |   
|  |  prompt_hash | prefix_hash | tool_policy | schema_policy | budget_scope | user-visible SLA|   
|  +---------------------------------------------+---------------------------------------------+   
|                                                |                                                 
|                                                v                                                 
|  +------------------------------------------------------------------------------------------+    
|  |                                  Serving Path Events                                     |    
|  +----------------------+----------------------+----------------------+---------------------+    
|                         |                      |                      |                          
|                         v                      v                      v                          
|          +-------------------------+  +-----------------------+  +----------------------------+  
|          | Gateway / Policy        |  | Router / Load Balancer|  | Queue / Admission          |  
|          |                         |  |                       |  |                            |  
|          | auth result             |  | selected model        |  | queue wait time            |  
|          | rate-limit decision     |  | rejected alternatives |  | priority lane              |  
|          | quota state             |  | cache-affinity score  |  | admission / rejection      |  
|          | tenant entitlement      |  | route confidence      |  | backpressure response      |  
|          +-----------+-------------+  +-----------+-----------+  +-------------+--------------+  
|                      |                            |                            |                 
|                      v                            v                            v                 
|          +-------------------------+  +-----------------------+  +-----------------------------+ 
|          | Inference Runtime       |  | Hardware / GPU State  |  | Validation / Fallback       | 
|          |                         |  |                       |  |                             | 
|          | model version SHA       |  | GPU ID / pool         |  | schema pass / fail          | 
|          | tokenizer version       |  | HBM occupancy         |  | safety pass / fail          | 
|          | decoder parameters      |  | KV cache usage        |  | tool-result verification    | 
|          | TTFT / ITL / TPS        |  | preemption events     |  | retry route / fail-closed   | 
|          | batch size              |  | OOM / kernel errors   |  | degraded-mode activation    | 
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+ 
|                      |                            |                            |                 
|                      +----------------------------+----------------------------+                 
|                                                   |                                              
|                                                   v                                              
|  +------------------------------------------------------------------------------------------+    
|  |                              Telemetry Collection Layer                                  |    
|  |   OpenTelemetry traces | Prometheus metrics |                                            |    
|  |   structured logs | GPU/DCGM metrics | audit events                                      |    
|  +----------------------+----------------------+----------------------+---------------------+    
|                         |                      |                      |                          
|                         v                      v                      v                          
|          +-------------------------+  +-----------------------+  +-----------------------------+ 
|          | Metrics Store           |  | Trace / Log Store     |  | Cost / Usage Ledger         | 
|          |                         |  |                       |  |                             | 
|          | latency histograms      |  | request replay trail  |  | input / output tokens       | 
|          | queue depth             |  | routing decisions     |  | cache savings               | 
|          | cache hit rate          |  | validation failures   |  | retries and fallbacks       | 
|          | GPU saturation          |  | incident evidence     |  | tenant billing allocation   | 
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+ 
|                      |                            |                            |                 
|                      +----------------------------+----------------------------+                 
|                                                   |                                              
|                                                   v                                              
|  +------------------------------------------------------------------------------------------+    
|  |                              Operational Interpretation                                  |    
|  +----------------------+----------------------+----------------------+---------------------+    
|                         |                      |                      |                          
|                         v                      v                      v                          
|          +-------------------------+  +-----------------------+  +-----------------------------+ 
|          | Dashboards              |  | Alerting / SLOs       |  | Root-Cause Analysis         | 
|          |                         |  |                       |  |                             | 
|          | tenant health           |  | p95 / p99 TTFT        |  | router regression           | 
|          | model health            |  | OOM rate              |  | cache-affinity loss         | 
|          | pool utilization        |  | validation error rate |  | cold-start storm            | 
|          | cost per success        |  | queue saturation      |  | GPU / PCIe contention       | 
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+ 
|                      |                            |                            |                 
|                      +----------------------------+----------------------------+                 
|                                                   |                                              
|                                                   v                                              
|  +---------------------------------------------------------------------------------------------+ 
|  |                              Automated Operations Loop                                      | 
|  |                                                                                             | 
|  |  trigger runbook | trip circuit breaker | shift traffic | scale GPU nodes | rollback model  | 
|  |  degrade service | shed low-priority work | rehydrate cache | verify recovery | reopen route| 
|  +---------------------------------------------+-----------------------------------------------+   
|                                                |                                                 
|                                                v                                                 
|                              [ Stable Serving State or Incident Record ]                         
|                                                                                                  
+--------------------------------------------------------------------------------------------------+
| Observability rule: every response should be explainable as a path through tenant policy,        |
| routing, queueing, cache state, runtime execution, hardware pressure, validation, fallback,      |
| and cost accounting.                                                                             |
+--------------------------------------------------------------------------------------------------+
```


This tracking model maps failures to their root causes, identifying bottlenecks across the stack.

### **Serving Failure Mode Map**

| Failure Scenario | Root Cause Mechanism | Immediate System Metric Impact | Operational Remediation |
| :--- | :--- | :--- | :--- |
| **Cold-Start Storm** | Concurrent node scale-ups download large model weights, initialize CUDA, compile kernels, or restore runtime templates at the same time. | Provisioning spike; TTFT rises sharply; readiness probes stay pending; weight-store bandwidth saturates. | Restore from warm snapshots or precompiled templates; stagger scale-ups; keep warm pools for high-SLO routes. |
| **GPU OOM Crash** | Long-context outliers, oversized batches, adapter pressure, or underestimated KV-cache growth exceed the memory envelope. | Worker exits; GPU memory metrics drop to zero after crash; OOM or CUDA allocation errors appear in logs. | Restart container; lower max context, batch size, or KV-cache fraction; route long-context requests to larger pools. |
| **KV Cache Exhaustion** | Active sequence count and context lengths consume all available paged KV blocks. | Free block count falls to zero; preemption/recompute spikes; ITL and TTFT degrade. | Reduce admission rate; lower active concurrency; route large-context traffic to separate lanes; add memory capacity. |
| **Cache-Affinity Collapse** | Router loses prefix-cache telemetry or prompt-template changes invalidate shared prefixes. | Prefix cache hit rate drops; TTFT rises; prefill FLOPS spike; warm replicas behave like cold replicas. | Restore prefix-aware routing; freeze prompt-template churn; rehydrate hot prefixes; temporarily route by stable hash. |
| **Router Regression** | Classification, semantic router, policy table, or canary split sends requests to unsuitable models. | Validation failures rise; fallback routes spike; cost per successful task increases; tenant complaints cluster by route ID. | Roll back routing policy; pin affected tenants to safe baseline route; replay recent traces against router candidates. |
| **Validation Retry Storm** | Model outputs fail schema, tool, or safety validators and trigger repeated retries or escalations. | Retry count rises; latency grows; downstream validator CPU increases; model route churn appears in traces. | Cap retries; fail closed for high-risk workflows; route to schema-specialized model; repair prompt or validator contract. |
| **Provider Fallback Loop** | Primary and fallback providers/routes fail or degrade in sequence, causing repeated route cycling. | Circuit breakers oscillate; external API error rates rise; latency and cost spike. | Break loop with explicit terminal degraded mode; pin to known-good route; disable failing provider until health recovers. |
| **Quota Counter Failure** | Redis, durable counters, or billing ledger becomes stale, slow, or unavailable. | Limit checks timeout; tenants exceed budget/concurrency; false rejections may appear. | Fail closed for unknown identity or high-risk tenants; use last-known-good limits for verified tenants; reconcile ledger after recovery. |
| **Artifact Registry Stale SHA** | Registry, artifact store, or deployment controller serves an old or mismatched model artifact. | Model SHA in traces diverges from release manifest; canary behavior does not match expected version. | Halt promotion; roll back to previous stable SHA; invalidate local artifact cache; verify checksums before reopening route. |
| **Prompt-Template Cache Bust** | System prompts, tool schemas, or formatting wrappers change too frequently, destroying prefix reuse. | Prefix hash cardinality rises; radix cache hit rate falls; TTFT increases across otherwise stable traffic. | Version and freeze prompt roots; move dynamic content after stable prefix; promote template changes through release gates. |
| **Prefill/Decode Transfer Backlog** | Disaggregated serving cannot move KV blocks from prefill nodes to decode nodes fast enough. | Transfer queue grows; decode pool waits idle; TTFT and ITL become unstable. | Increase transfer bandwidth; reserve decode-side KV blocks earlier; co-locate pools; fall back to mixed prefill/decode serving for affected routes. |
| **Queue Saturation** | Arrival rate exceeds available serving capacity or low-priority work competes with interactive traffic. | Waiting request count spikes; queue wait dominates TTFT; 429/503 rates rise. | Shed low-priority work; defer batch jobs; add replicas; adjust admission control and tenant priority lanes. |
| **Telemetry Drop** | Logging or tracing volume saturates collector buffers, network links, or metrics storage. | Packet loss; missing traces; collector queue growth; ClickHouse or metrics ingestion lag. | Sample noncritical telemetry; preserve audit-critical traces; scale collectors; decouple telemetry export from request execution. |
| **MIG Fragmentation** | Available GPU partitions do not match requested model or tenant profiles. | GPU capacity exists but pods remain pending; scheduler cannot place replicas. | Reallocate MIG profiles; maintain standard partition templates; route workloads to compatible pools. |
| **Tenant Noisy Neighbor** | One tenant monopolizes queue slots, KV cache, PCIe bandwidth, or adapter residency. | Tenant-skewed queue depth; p99 latency rises for unrelated tenants; GPU/HBM usage clusters by tenant. | Enforce per-tenant concurrency, TPM/RPM, and fair queues; move noisy tenant to isolated pool. |
| **Backpressure Failure** | Gateway continues admitting traffic after backend saturation. | Queue depth grows monotonically; retry storms begin; OOM and timeout rates rise. | Trip admission control; return clean 429/503; require client backoff with jitter; preserve premium and high-priority headroom. |

Failure-mode maps should be connected directly to operational runbooks. A failure row that does not produce a routing, rollback, scaling, or containment action is documentation confetti.

### **Operational Runbook Execution Guidance**

```                        
+---------------------------------------------------------------------------------------------------+
|                              OPERATIONAL RUNBOOK EXECUTION GUIDANCE                               |
+---------------------------------------------------------------------------------------------------+
|                                                                                                   
|  Goal: convert serving anomalies into repeatable diagnosis, containment, remediation,             
|  verification, and incident documentation steps.                                                  
|                                                                                                   
|  [ Alert / SLO Breach / User-Visible Degradation ]                                                
|        |                                                                                          
|        v                                                                                          
|  +------------------------------------------------------------------------------------------+     
|  |                              Incident Intake                                             |     
|  |                                                                                          |     
|  |  Capture: request IDs | affected tenants | model route | region | severity | start time  |     
|  |           symptoms | recent deploys | canary status | active fallback state              |     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                                v                                                  
|  +---------------------------------------------------------------------------------------------+  
|  |                              First Triage Dashboard                                         |  
|  |                                                                                             |  
|  |  Check Grafana / Prometheus / tracing dashboards for:                                       |  
|  |                                                                                             |  
|  |  - waiting request count                                                                    |  
|  |  - p95 / p99 TTFT and inter-token latency                                                   |  
|  |  - GPU HBM utilization                                                                      |  
|  |  - KV cache occupancy                                                                       |  
|  |  - OOM / SIGSEGV / kernel errors                                                            |  
|  |  - router timeout and validation failure rates                                              |  
|  +----------------------+----------------------+----------------------+------------------------+  
|                         |                      |                      |                           
|                         v                      v                      v                           
|          [ Queue Saturation? ]      [ Runtime Crash / OOM? ]    [ Routing / Validation Spike? ]   
|                /      \                    /      \                     /       \                 
|          Yes  /        \ No          Yes  /        \ No           Yes  /         \ No             
|              v          v                v          v                 v           v               
|  +----------------+  +----------------+  +----------------+  +----------------+  +-------------+  
|  | Provision /    |  | Check Network, |  | Isolate Failed |  | Check Runtime  |  | Inspect     |  
|  | Shift Capacity |  | Registry, and  |  | Replica / GPU  |  | Logs, Kernels, |  | Router,     |  
|  |                |  | Provider Path  |  | Pool           |  | and Memory     |  | Prompts,    |  
|  |                |  |                |  |                |  | Envelope       |  | Schemas     |  
|  +-------+--------+  +-------+--------+  +-------+--------+  +-------+--------+  +------+------+  
|          |                   |                   |                   |                  |         
|          v                   v                   v                   v                  v         
|  +------------------------------------------------------------------------------------------+     
|  |                              Containment Action                                          |     
|  |                                                                                          |     
|  |  Select the least disruptive safe action:                                                |     
|  |                                                                                          |     
|  |  - trip circuit breaker for unhealthy route                                              |     
|  |  - shed or defer low-priority traffic                                                    |     
|  |  - shift traffic to backup pool / region / provider                                      |     
|  |  - downgrade model tier for low-risk workloads                                           |     
|  |  - rollback to previous model or serving image SHA                                       |     
|  |  - freeze new deployments while incident is active                                       |     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                                v                                                  
|  +------------------------------------------------------------------------------------------+     
|  |                              Remediation Path                                            |     
|  +----------------------+----------------------+----------------------+---------------------+     
|                         |                      |                      |                           
|                         v                      v                      v                           
|          +-------------------------+  +-----------------------+  +-----------------------------+  
|          | Capacity Remediation    |  | Runtime Remediation   |  | Routing / Policy Fix        |  
|          |                         |  |                       |  |                             |  
|          | provision GPU nodes     |  | restart container     |  | restore static route        |  
|          | drain overloaded queue  |  | lower max context     |  | disable bad canary          |  
|          | increase warm pool      |  | reduce batch pressure |  | repair schema / validator   |  
|          | defer batch work        |  | reload stable weights |  | update fallback policy      |  
|          +-----------+-------------+  +-----------+-----------+  +-------------+---------------+  
|                      |                            |                            |                  
|                      +----------------------------+----------------------------+                  
|                                                   |                                               
|                                                   v                                               
|  +------------------------------------------------------------------------------------------+     
|  |                              Recovery Verification                                       |     
|  |                                                                                          |     
|  |  Verify before reopening full traffic:                                                   |     
|  |                                                                                          |     
|  |  - health checks pass                                                                    |     
|  |  - mock inference succeeds                                                               |     
|  |  - TTFT / ITL return to SLO range                                                        |     
|  |  - queue depth normalizes                                                                |     
|  |  - GPU memory remains stable                                                             |     
|  |  - validation failures return to baseline                                                |     
|  |  - fallback route is no longer receiving abnormal volume                                 |     
|  +---------------------------------------------+--------------------------------------------+     
|                                                |                                                  
|                                      [ Recovery Verified? ]                                       
|                                         /             \                                           
|                                  Yes   /               \   No                                     
|                                       v                 v                                         
|  +-----------------------------------------+     +-------------------------------------------+    
|  |          Gradual Route Reopen           |     |     Continue Containment / Escalate       |    
|  |                                         |     |                                           |    
|  |  - reopen circuit gradually             |     |  - widen degraded mode                    |    
|  |  - restore canary or primary route      |     |  - escalate to SRE / platform owner       |    
|  |  - monitor p95 / p99 tails              |     |  - invoke vendor / provider support       |    
|  |  - keep rollback ready                  |     |  - extend traffic freeze                  |    
|  +--------------------+--------------------+     +----------------+--------------------------+    
|                       |                                           |                               
|                       v                                           v                               
|           [ Stable Serving State ]                    [ Active Incident Continues ]               
|                       |                                                                           
|                       v                                                                           
|  +------------------------------------------------------------------------------------------+     
|  |                              Incident Record / Review                                    |     
|  |                                                                                          |     
|  |  Store: root cause | detection gap | affected tenants | timeline | remediation steps     |     
|  |         route changes | cost impact | rollback action | follow-up prevention tasks       |     
|  +------------------------------------------------------------------------------------------+     
|                                                                                                   
+---------------------------------------------------------------------------------------------------+
| Runbook rule: contain first, diagnose second, restore gradually, and document enough evidence     |
| that the next recurrence is shorter, safer, and less expensive.                                   |
+---------------------------------------------------------------------------------------------------+
```

## **Cross-Canon Handoff Map**

The serving topologies, load balancers, quota policies, failover systems, and runtime traces established in AI-ENG-L provide the runtime foundation for downstream systems in the canon. AI-ENG-L does not merely describe where models run. It exports the operational contracts that other modules depend on: model routes, fallback paths, tenant identity, quota state, GPU-placement metadata, cache-affinity records, validation boundaries, telemetry traces, model SHAs, latency records, and rollback signals.

```
+------------------------------------------------------------------------------------------------+
|                    AI-ENG-L: SERVING ARCHITECTURE, ROUTING & LOAD BALANCING                    |
+------------------------------------------------------------------------------------------------+
|                                                                                                |
|  Transfers the runtime contracts that downstream systems depend on:                            |
|                                                                                                |
|  - model routes and fallback paths                                                             |
|  - tenant identity, quotas, and capacity limits                                                |
|  - queueing, batching, cache, and GPU-placement metadata                                       |
|  - tool-routing budgets and validation boundaries                                              |
|  - telemetry traces, model SHAs, latency records, and rollback signals                         |
|                                                                                                |
+----------------------+----------------------+----------------------+---------------------------+
                       |                      |                      |
                       v                      v                      v

+--------------------------------+  +--------------------------------+  +--------------------------------+
| Runtime Isolation &            |  | Agent / Tool Execution         |  | Telemetry, Evaluation & Audit  |
| Resource Governance            |  | Boundaries                     |  | Trails                         |
+--------------------------------+  +--------------------------------+  +--------------------------------+
|                                |  |                                |  |                                |
| AI-ENG-T: Tenant Isolation     |  | AI-ENG-M: Agent Loops          |  | AI-ENG-Z: Telemetry, Evals     |
| - tenant cache keys            |  | - model routes by step budget  |  | - route, latency, and failure  |
| - GPU partition boundaries     |  | - recursion and cost ceilings  |  |   telemetry                    |
| - cross-tenant leakage guards  |  | - dynamic execution budgets    |  | - serving-quality observability|
|                                |  |                                |  |                                |
|             |                  |  |             |                  |  |             |                  |
|             v                  |  |             v                  |  |             v                  |
| AI-ENG-V: Resource Abuse       |  | AI-ENG-N: Tool Contracts       |  | AI-ENG-AA: Serving Evaluations |
| - RPM / TPM / concurrency      |  | - schema-aware route choices   |  | - live model scoring           |
| - budget ceilings              |  | - tool argument validation     |  | - route-quality adjustment     |
| - denial-of-wallet controls    |  | - fail-closed tool boundaries  |  | - eval-informed canaries       |
|                                |  |                                |  |                                |
|             |                  |  |             |                  |  |             |                  |
|             v                  |  |             v                  |  |             v                  |
| AI-ENG-AE: Infrastructure      |  | AI-ENG-O: Action Verification  |  | AI-ENG-AB: Replayable Traces   |
| Efficiency                     |  | - high-stakes action gates     |  | - request reconstruction       |
| - cache-hit optimization       |  | - human review routing         |  | - prompt/cache/model evidence  |
| - GPU utilization strategy     |  | - transaction safety paths     |  | - audit-ready execution trails |
+--------------------------------+  +--------------------------------+  +--------------------------------+
                       |                      |
                       v                      v
+--------------------------------+  +--------------------------------+
| Resilience, Fallback &         |  | Production Strategy &          |
| Incident Response              |  | Build / Buy Decisions          |
+--------------------------------+  +--------------------------------+
|                                |  |                                |
| AI-ENG-W: Fallback Chains      |  | AI-ENG-S: Production Pathology |
| - degraded-mode routes         |  | - queue saturation diagnosis   |
| - model downgrade paths        |  | - GPU OOM / cold-start storms  |
| - provider / region failover   |  | - cache-affinity regressions   |
|                                |  |                                |
|             |                  |  |             |                  |
|             v                  |  |             v                  |
| AI-ENG-AC: Incident Response   |  | AI-ENG-AH: Build / Buy Strategy|
| - circuit breakers             |  | - hosting economics            |
| - rollback playbooks           |  | - topology tradeoffs           |
| - recovery verification        |  | - managed vs self-hosted fit   |
+--------------------------------+  +--------------------------------+

+------------------------------------------------------------------------------------------------+
| Handoff rule: AI-ENG-L does not merely describe where models run. It exports the runtime       |
| contracts that downstream reports use to govern tenants, tools, fallbacks, traces, incidents,  |
| evaluations, resource abuse, infrastructure efficiency, and hosting strategy.                  |
+------------------------------------------------------------------------------------------------+
```

### **Cross-Canon Handoff Matrix**

| Target Report | Downstream Dependency | Transferred Interface / Contract | Operational Coupling |
| :--- | :--- | :--- | :--- |
| **AI-ENG-M** | Agent Loops & Budgets | Dynamic tool-execution step counts, recursion limits, model routes by step budget, and dollar budgets per query. | Gateway halts recursive loops when agent execution limits are hit. |
| **AI-ENG-N** | Tool Contract Boundaries | Model routes matched against JSON schemas, tool-call contracts, and structured-output requirements. | Rejects invalid model outputs before forwarding them to downstream tools. |
| **AI-ENG-O** | Action Verification Paths | Interception proxies, validation queues, action-risk classes, and human-review route metadata. | Redirects high-stakes transactions to validation gates or human-in-the-loop review. |
| **AI-ENG-S** | Production Pathology | Prometheus execution metrics, preemption events, queue state, cache-affinity collapse, and cold-start signals. | Isolates and diagnoses performance drops, serving regressions, and runtime failures. |
| **AI-ENG-T** | Tenant Isolation & Leakage | Multi-tenant cache keys, quota state, region constraints, GPU partition mappings, and tenant-aware routing records. | Prevents cross-tenant data leakage and noisy-neighbor behavior inside shared serving systems. |
| **AI-ENG-V** | Resource Abuse Defense | Sliding-window token buckets, RPM/TPM/concurrency limits, retry limits, and budget ceilings. | Defends physical GPU clusters from denial-of-wallet and scheduler-exhaustion vectors. |
| **AI-ENG-W** | Fallback Chains | Degraded-serving maps, model-downgrade paths, provider failover rules, and safe terminal routes. | Preserves core workflows during partial outages while failing closed when safe degradation is impossible. |
| **AI-ENG-Z** | Telemetry, Evals & Audits | Structured execution traces, model SHAs, route IDs, latency records, cache hit rates, and tenant cost records. | Aggregates system metrics to verify SLA, reliability, and compliance targets. |
| **AI-ENG-AA** | Serving Evaluations | Dynamic model validation scores, live routing quality, canary metrics, and route-confidence signals. | Adjusts routing weights and promotion gates based on live and replayed evaluation results. |
| **AI-ENG-AB** | Replayable Traces | Tokenized prefix hashes, session identifiers, release manifests, model SHAs, routing decisions, and fallback paths. | Reconstructs serving states to investigate regressions, incidents, and audit questions. |
| **AI-ENG-AC** | Incident Response Playbooks | Automated circuit breakers, health-check probes, containment actions, degraded modes, and recovery verification. | Triggers restarts, traffic shifts, rollback actions, and gradual route reopening after incidents. |
| **AI-ENG-AD** | Governance, Policy, Compliance & Accountability | Tenant contracts, data-residency rules, regulated-route approvals, model-access policies, audit boundaries, and exception records. | Ensures serving routes, fallback paths, degraded modes, and tenant isolation policies comply with governance requirements before promotion. |
| **AI-ENG-AE** | Infrastructure Efficiency | Radix-tree memory use, prefix-match distribution, GPU utilization, cache-hit optimization, and energy-per-token records. | Maximizes cache reuse and compute-to-watt efficiency across serving pools. |
| **AI-ENG-AH** | Build / Buy Strategy | Hosting cost matrices, latency profiles, topology constraints, provider fallback records, and operating-complexity evidence. | Evaluates managed API, self-hosted, hybrid, edge, and on-prem serving tradeoffs. |

The handoff rule is simple: AI-ENG-L exports the live runtime contracts that downstream systems use to govern tenants, tools, fallbacks, traces, incidents, evaluations, resource abuse, infrastructure efficiency, governance, and hosting strategy.

## **Core Doctrinal Principles**

To conclude this research report, the operational tenets of model serving architecture are synthesized into seven durable engineering principles.

### **I. Model Serving is Traffic Governance, Not Simple Hosting**

The serving stack must act as an intelligent gateway, managing requests across validation, security, and resource boundaries before allocating hardware cycles.1 Exposing raw inference endpoints directly to application consumers bypasses capacity management and risks system-wide cascades.2

### **II. Decouple the Control Plane from the Data Plane**

Changes to routing rules, deployment weights, tenant allocations, and fallback configurations must execute declaratively in the control plane.19 The data plane must focus exclusively on low-latency request execution, using zero-copy pipelines and stable metadata lookups to ensure high-performance streaming.5

### **III. Route Requests via Token and Memory Locality**

LLM serving is stateful by nature; the physical GPU hosting the warm KV cache is the most efficient execution target.5 Load balancers must route requests by analyzing tokenized prefix hashes, utilizing precise cache-state telemetry to avoid redundant prefill passes.23

### **IV. Enforce Multi-Tenant Isolation Down to the Silicon Layer**

Logical rate-limiting at the proxy is insufficient for heavy enterprise serving workloads.2 Platform teams must implement physical hardware partition boundaries, using MPS, MIG, and PCIe-aware process affinity placement to prevent cross-tenant performance interference and tail-latency degradation.10

### **V. Queue Centrally and Bind Late**

Holding requests centrally inside the load balancing plane avoids "scheduling regret," preventing high-priority tasks from getting stuck behind lower-tier requests in localized model queues.24 This delay allows the gateway to dispatch requests to the optimal replica at the last possible millisecond.24

### **VI. Proactive Autoscaling Demands GPU-Native Metrics**

Standard CPU and system memory footprint monitors fail to predict GPU compute bottlenecks.9 Platform operators must scale clusters based on model-specific parameters—such as waiting request queues and active HBM occupancy—to scale reactively before users experience latency spikes.9

### **VII. Plan for Degraded Grace over Absolute Failure**

In high-dimensional serving environments, network drops, hardware faults, and budget exhaustion are inevitable.27 Serving platforms must map detailed fallback routes, dynamic model downgrades, and cached response states to maintain availability even during partial outages.4

#### **Works cited**

1. Load Balancing for AI Inference: Distributing Requests Across 1000+ GPUs - Introl, accessed June 9, 2026, [https://introl.com/blog/load-balancing-ai-inference-distributing-requests-1000-gpus](https://introl.com/blog/load-balancing-ai-inference-distributing-requests-1000-gpus)  
2. Multi-Tenant LLM Serving on GPU Cloud: Per-Customer Isolation ..., accessed June 9, 2026, [https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/](https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/)  
3. Monitor LLM routing with the Kubernetes Inference Extension - Datadog, accessed June 9, 2026, [https://www.datadoghq.com/blog/llm-routing-kubernetes-inference-extension/](https://www.datadoghq.com/blog/llm-routing-kubernetes-inference-extension/)  
4. Intelligent LLM Routing: Cost & Quality-Aware Selection - Truefoundry, accessed June 9, 2026, [https://www.truefoundry.com/blog/llm-routing-cost-quality-aware-model-selection](https://www.truefoundry.com/blog/llm-routing-cost-quality-aware-model-selection)  
5. How we fight GPU scarcity without compromise | Equixly, accessed June 9, 2026, [https://equixly.com/blog/2026/06/05/how-we-fight-gpu-scarcity-without-compromise/](https://equixly.com/blog/2026/06/05/how-we-fight-gpu-scarcity-without-compromise/)  
6. SGLang Model Gateway, accessed June 9, 2026, [https://sgl-project.github.io/advanced_features/sgl_model_gateway.html](https://sgl-project.github.io/advanced_features/sgl_model_gateway.html)  
7. Deploy Ray Serve on GPU Cloud: Production LLM Serving with Python-Native Orchestration (2026 Guide) | Spheron Blog, accessed June 9, 2026, [https://www.spheron.network/blog/ray-serve-gpu-cloud-llm-deployment/](https://www.spheron.network/blog/ray-serve-gpu-cloud-llm-deployment/)  
8. Self-Hosted LLM: The Operator's Checklist for Enterprise Production - Allganize, accessed June 9, 2026, [https://www.allganize.ai/en/blog/self-hosted-llm-operator-checklist](https://www.allganize.ai/en/blog/self-hosted-llm-operator-checklist)  
9. Scaling LLM Workloads on Kubernetes: A Production Engineer's Guide - Zartis, accessed June 9, 2026, [https://www.zartis.com/scaling-llm-workloads-on-kubernetes-a-production-engineers-guide/](https://www.zartis.com/scaling-llm-workloads-on-kubernetes-a-production-engineers-guide/)  
10. Predictable LLM Serving on GPU Clusters - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2508.20274v1](https://arxiv.org/html/2508.20274v1)  
11. Prefill-Decode Disaggregation on GPU Cloud: Split LLM Inference for 2x Throughput (2026 Guide) | Spheron Blog, accessed June 9, 2026, [https://www.spheron.network/blog/prefill-decode-disaggregation-gpu-cloud/](https://www.spheron.network/blog/prefill-decode-disaggregation-gpu-cloud/)  
12. Prefill-Decode Disaggregation - SGLang, accessed June 9, 2026, [https://sgl-project-sglang-93.mintlify.app/distributed/prefill-decode-disaggregation](https://sgl-project-sglang-93.mintlify.app/distributed/prefill-decode-disaggregation)  
13. NVIDIA Inference Reference Architecture | NVIDIA DSX Documentation, accessed June 9, 2026, [https://docs.nvidia.com/dsx/guides/inference-ra/home](https://docs.nvidia.com/dsx/guides/inference-ra/home)  
14. Optimization and Tuning - vLLM Documentation, accessed June 9, 2026, [https://docs.vllm.ai/en/stable/configuration/optimization/](https://docs.vllm.ai/en/stable/configuration/optimization/)  
15. Enabling Performant and Flexible Model-Internal Observability for LLM Inference - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2605.11093v1](https://arxiv.org/html/2605.11093v1)  
16. SGLang Production Deployment Guide: RadixAttention and Multi-Turn Inference on GPU Cloud (2026) | Spheron Blog, accessed June 9, 2026, [https://www.spheron.network/blog/sglang-production-deployment-guide/](https://www.spheron.network/blog/sglang-production-deployment-guide/)  
17. Automatic Prefix Caching - vLLM Documentation, accessed June 9, 2026, [https://docs.vllm.ai/en/v0.7.0/design/automatic_prefix_caching.html](https://docs.vllm.ai/en/v0.7.0/design/automatic_prefix_caching.html)  
18. Roadmap: SGLang Router · Issue #10341 - GitHub, accessed June 9, 2026, [https://github.com/sgl-project/sglang/issues/10341](https://github.com/sgl-project/sglang/issues/10341)  
19. KServe vs Seldon Core vs BentoML on GPU Cloud: Kubernetes ML Serving Guide (2026), accessed June 9, 2026, [https://www.spheron.network/blog/kserve-vs-seldon-core-vs-bentoml-kubernetes-ml-serving-guide/](https://www.spheron.network/blog/kserve-vs-seldon-core-vs-bentoml-kubernetes-ml-serving-guide/)  
20. How Mixpeek runs distributed multimodal ML on Ray: architecture, patterns, and production lessons, accessed June 9, 2026, [https://blog.mixpeek.com/ray-distributed-ml-pipeline-architecture/](https://blog.mixpeek.com/ray-distributed-ml-pipeline-architecture/)  
21. Next-Level Inference: Why Your Single-Node vLLM Setup Needs Prefill-Decode Disaggregation, accessed June 9, 2026, [https://vllm.ai/blog/2026-04-07-moriio-kv-connector](https://vllm.ai/blog/2026-04-07-moriio-kv-connector)  
22. AI Agent Model Routing and Dynamic Model Selection Strategies | Zylos Research, accessed June 9, 2026, [https://zylos.ai/research/2026-03-02-ai-agent-model-routing/](https://zylos.ai/research/2026-03-02-ai-agent-model-routing/)  
23. Inference routing | LLM Inference Handbook - BentoML, accessed June 9, 2026, [https://bentoml.com/llm/inference-optimization/inference-routing](https://bentoml.com/llm/inference-optimization/inference-routing)  
24. Flow Control - Kubernetes Gateway API Inference Extension, accessed June 9, 2026, [https://gateway-api-inference-extension.sigs.k8s.io/guides/flow-control/](https://gateway-api-inference-extension.sigs.k8s.io/guides/flow-control/)  
25. [Literature Review] Predictable LLM Serving on GPU Clusters - Moonlight, accessed June 9, 2026, [https://www.themoonlight.io/en/review/predictable-llm-serving-on-gpu-clusters](https://www.themoonlight.io/en/review/predictable-llm-serving-on-gpu-clusters)  
26. Foundry: Template-Based CUDA Graph Context Materialization for Fast LLM Serving Cold Start - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2604.06664v1](https://arxiv.org/html/2604.06664v1)  
27. AI Deployment Incident Runbook: The First 30 Minutes - GIGAGPU, accessed June 9, 2026, [https://gigagpu.com/ai-deployment-incident-runbook/](https://gigagpu.com/ai-deployment-incident-runbook/)  
28. LLM Routing and Model Cascades: How to Cut AI Costs Without Sacrificing Quality, accessed June 9, 2026, [https://tianpan.co/blog/2025-11-03-llm-routing-model-cascades](https://tianpan.co/blog/2025-11-03-llm-routing-model-cascades)  
29. 5 Best Model Routing Platforms for AI Agent Systems | Augment Code, accessed June 9, 2026, [https://www.augmentcode.com/tools/model-routing-platforms-ai-agent-systems](https://www.augmentcode.com/tools/model-routing-platforms-ai-agent-systems)  
30. DP-attention: add prefix_match load balance for in-instance cache ..., accessed June 9, 2026, [https://github.com/sgl-project/sglang/issues/26611](https://github.com/sgl-project/sglang/issues/26611)  
31. SGLang Router Architecture Improvement Proposal · Issue #7532 - GitHub, accessed June 9, 2026, [https://github.com/sgl-project/sglang/issues/7532](https://github.com/sgl-project/sglang/issues/7532)  
32. vLLM in Production: Ranked Configuration Decisions, Failure Modes, and the Architecture That Makes Them Work - DEV Community, accessed June 9, 2026, [https://dev.to/damasosanoja/vllm-in-production-ranked-configuration-decisions-failure-modes-and-the-architecture-that-makes-2g7p](https://dev.to/damasosanoja/vllm-in-production-ranked-configuration-decisions-failure-modes-and-the-architecture-that-makes-2g7p)  
33. GitHub - sgl-project/sglang: SGLang is a high-performance serving framework for large language models and multimodal models., accessed June 9, 2026, [https://github.com/sgl-project/sglang](https://github.com/sgl-project/sglang)  
34. Databricks - Certified-Machine-Learning-Associate Practice Paper, accessed June 9, 2026, [https://interview.quicktechie.com/certifications/practice-paper/4/12/Databricks/Certified-Machine-Learning-Associate/Practice-Paper-2](https://interview.quicktechie.com/certifications/practice-paper/4/12/Databricks/Certified-Machine-Learning-Associate/Practice-Paper-2)  
35. AI agents on Ray Serve: Single to multi-agent architecture | Anyscale, accessed June 9, 2026, [https://www.anyscale.com/blog/ai-agents-on-ray-serve-single-to-multi-agent-architecture](https://www.anyscale.com/blog/ai-agents-on-ray-serve-single-to-multi-agent-architecture)  
36. RadixAttention - SGLang, accessed June 9, 2026, [https://sgl-project-sglang-93.mintlify.app/concepts/radix-attention](https://sgl-project-sglang-93.mintlify.app/concepts/radix-attention)  
37. Secure Deployment Considerations — NVIDIA Triton Inference Server, accessed June 9, 2026, [https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/customization_guide/deploy.html](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/customization_guide/deploy.html)  
38. Metrics — NVIDIA Triton Inference Server, accessed June 9, 2026, [https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/user_guide/metrics.html](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/user_guide/metrics.html)  
39. Serve PyTorch models at scale with Ray Serve, accessed June 9, 2026, [https://docs.pytorch.org/tutorials/beginner/serving_tutorial.html](https://docs.pytorch.org/tutorials/beginner/serving_tutorial.html)  
40. Develop a Ray Serve application - Anyscale Docs, accessed June 9, 2026, [https://docs.anyscale.com/runtime/serve/develop](https://docs.anyscale.com/runtime/serve/develop)  
41. Architecture overview - Ray Serve, accessed June 9, 2026, [https://docs.ray.io/en/latest/serve/llm/architecture/overview.html](https://docs.ray.io/en/latest/serve/llm/architecture/overview.html)  
42. vLLM Large Scale Serving: DeepSeek @ 2.2k tok/s/H200 with Wide ..., accessed June 9, 2026, [https://vllm.ai/blog/2025-12-17-large-scale-serving](https://vllm.ai/blog/2025-12-17-large-scale-serving)  
43. Can serverless GPU replace local LLMs? I reduced vLLM cold start 6.5x to find out, accessed June 9, 2026, [https://logeshumapathi.com/blog/2026/05/17/vllm-serverless.html](https://logeshumapathi.com/blog/2026/05/17/vllm-serverless.html)  
44. RouteLLM: Learning to Route LLMs with Preference Data - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2406.18665v4](https://arxiv.org/html/2406.18665v4)  
45. Why LLM Inference Needs a New Kind of Router - Part 1 - Modular, accessed June 9, 2026, [https://www.modular.com/blog/why-llm-inference-needs-a-new-kind-of-router-part-1](https://www.modular.com/blog/why-llm-inference-needs-a-new-kind-of-router-part-1)  
46. Removing the Guesswork from Disaggregated Serving | NVIDIA Technical Blog, accessed June 9, 2026, [https://developer.nvidia.com/blog/removing-the-guesswork-from-disaggregated-serving/](https://developer.nvidia.com/blog/removing-the-guesswork-from-disaggregated-serving/)  
47. Ray Serve LLM on Anyscale: APIs for Wide-EP and Disaggregated Serving with vLLM, accessed June 9, 2026, [https://www.anyscale.com/blog/ray-serve-llm-anyscale-apis-wide-ep-disaggregated-serving-vllm](https://www.anyscale.com/blog/ray-serve-llm-anyscale-apis-wide-ep-disaggregated-serving-vllm)  
48. [2508.20274] Predictable LLM Serving on GPU Clusters - arXiv, accessed June 9, 2026, [https://arxiv.org/abs/2508.20274](https://arxiv.org/abs/2508.20274)  
49. HydraServe: Minimizing Cold Start Latency for Serverless LLM Serving in Public Clouds - USENIX, accessed June 9, 2026, [https://www.usenix.org/system/files/conference/nsdi26/nsdi26spring_lou_prepub.pdf](https://www.usenix.org/system/files/conference/nsdi26/nsdi26spring_lou_prepub.pdf)  
50. [2604.06664] Foundry: Template-Based CUDA Graph Context Materialization for Fast LLM Serving Cold Start - arXiv, accessed June 9, 2026, [https://arxiv.org/abs/2604.06664](https://arxiv.org/abs/2604.06664)  
51. PCIe Bandwidth-Aware Scheduling for Multi-Instance GPUs | Request PDF - ResearchGate, accessed June 9, 2026, [https://www.researchgate.net/publication/390241224_PCIe_Bandwidth-Aware_Scheduling_for_Multi-Instance_GPUs](https://www.researchgate.net/publication/390241224_PCIe_Bandwidth-Aware_Scheduling_for_Multi-Instance_GPUs)

---

[← Back to Canon Map](../canon-map.md)