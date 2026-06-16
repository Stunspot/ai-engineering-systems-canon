# [KNOWLEDGE] - AI Engineering - Part IV - Vol. 9-10 - Z-AE - Evaluation, Operations, and Governance

[Volume 9. Z-AB Observability, Evaluation, and Verification](#volume-9-z-ab-observability-evaluation-and-verification)
  - [AI-ENG-Z — Strategic Telemetry - Traces, Tokens, Latency & Semantic Drift](#ai-eng-z--strategic-telemetry---traces-tokens-latency--semantic-drift)
  - [AI-ENG-AA — Evals Architecture: Ground Truth, Golden Sets & Regression Tests](#ai-eng-aa--evals-architecture-ground-truth-golden-sets--regression-tests)
  - [AI-ENG-AB — Verification Artifacts - Auditability, Reproducibility & Evidence Trails](#ai-eng-ab--verification-artifacts---auditability-reproducibility--evidence-trails)

[Volume 10. AC-AE Operations, Governance, and Lifecycle Management](#volume-10-ac-ae-operations-governance-and-lifecycle-management)
  - [AI-ENG-AC — AI Operations - Incident Response, Rollback & Runbook Design](#ai-eng-ac--ai-operations---incident-response-rollback--runbook-design)
  - [AI-ENG-AD — Governance Architecture - Policy, Audit, Compliance & Accountability](#ai-eng-ad--governance-architecture---policy-audit-compliance--accountability)
  - [AI-ENG-AE — Sustainable AI - Energy, Infrastructure Efficiency & Lifecycle Impact](#ai-eng-ae--sustainable-ai---energy-infrastructure-efficiency--lifecycle-impact)

---

# Volume 9. Z-AB Observability, Evaluation, and Verification

---

# AI-ENG-Z — Strategic Telemetry - Traces, Tokens, Latency & Semantic Drift

## **Doctrinal Foundations and the Green Dashboard Fallacy**

In high-dimensional artificial intelligence systems, production reliability cannot be measured by traditional application performance monitoring (APM) paradigms.1 In legacy web architectures, system health is treated as a binary or scalar state defined by infrastructure metrics: a service is considered healthy when APIs return successful status codes, server workloads are balanced, database connections are active, and latency remains within acceptable thresholds.2 However, when these architectures are driven by probabilistic large language models (LLMs) and multi-agent loops, backend availability does not guarantee a successful, safe, or contextually coherent user transaction.1

An artificial intelligence gateway can return a successful HTTP 200 payload that contains a complete structural schema failure, an unauthorized tool execution, a cross-tenant data leak, or an ungrounded factual confabulation.1 Conversely, a system can exhibit optimal hardware utilization and high throughput while delivering answers that silently violate safety boundaries, omit critical citations, or trap agentic loops in expensive, infinite executions.1

This mismatch establishes the **Green Dashboard Fallacy**: the erroneous belief that an AI system is healthy because conventional infrastructure metrics are green.1 In production AI engineering, a green infrastructure dashboard can coexist with a red behavioral and meaning-level system.1

Strategic telemetry is the first-class observability discipline designed to resolve this fallacy.1 It is defined as the systematic capture, correlation, protection, and interpretation of traces, spans, tokens, latency, errors, retries, retrieval events, tool calls, routing decisions, cost attribution, user trust signals, governance events, and semantic drift.1 It treats telemetry not merely as infrastructure logs, but as the primary evidence substrate required for evaluation, replay, debugging, forensic investigation, and behavioral governance.1

### **Artifact 1: Conceptual Glossary**

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Strategic Telemetry** | The systematic capture, correlation, protection, and interpretation of semantic, economic, behavioral, security, and operational signals across an AI system’s execution path. | Behavioral Observability Coverage | Coverage is sufficient to debug, replay, evaluate, and govern high-impact workflows. |
| **AI Trace** | A structured graph representing the execution path of a probabilistic interaction across model calls, retrieval, tools, routing, policy checks, user actions, and state transitions. | Trace Completion Rate | Critical execution paths emit complete enough traces for replay and incident analysis. |
| **Span** | The atomic unit of observable work inside a trace, such as model inference, retrieval, reranking, tool execution, parsing, moderation, or human review. | Span Duration / Status Coverage | Required spans include status, timing, error class, and correlation identifiers. |
| **GenAI Semantic Convention** | A configured OpenTelemetry/OpenInference-compatible attribute vocabulary for model, provider, token, tool, retrieval, and agent telemetry. | Convention Compliance Score | Required attributes are present for the configured convention version. |
| **Token Telemetry** | Tracking of input, output, cached, embedding, reasoning, retry, repair, and wasted tokens where exposed by providers or runtime gateways. | Token Attribution Completeness | High-impact and billable usage is attributed to tenant, user/session, workflow, route, and model where applicable. |
| **Time to First Token (TTFT)** | Duration from request dispatch until the first streamed token or first response chunk is available. | TTFT by Workload Profile | TTFT remains within workload-specific SLOs. |
| **Prefill Latency** | Compute-bound latency required to process prompt/context tokens and initialize model state before generation. | Prefill P95 / P99 | Prefill latency scales within expected envelope for prompt length and model route. |
| **Decode Latency** | Memory-bandwidth-sensitive latency required to generate completion tokens sequentially. | Inter-Token Latency / TPS | Decode performance remains within model and hardware profile expectations. |
| **Routing Decision** | Runtime selection of model, provider, fallback path, cache, human review, or fail-closed route based on capability, cost, latency, safety, and availability. | Routing Decision Validity | Route satisfies task capability floor and policy constraints. |
| **Retrieval Event** | A vector, lexical, relational, hybrid, or document-expansion query performed to gather evidence for context or citation. | Retrieval Sufficiency / Fan-Out | Retrieval remains authorized, bounded, freshness-aware, and evidence-adequate. |
| **Tool Trace** | The structured record of tool selection, arguments, authorization, execution status, cost, side effects, and verification state. | Tool Verification Coverage | High-impact tool calls include pre-action authorization and post-action verification status. |
| **Cost Attribution** | Mapping of resource consumption to tenant, user, session, workflow, model, tool, retrieval, parser, batch job, or incident. | Attributed Spend Ratio | Material spend is attributable or explicitly marked unknown/pending reconciliation. |
| **Semantic Drift** | Shift in model behavior, grounding, style, refusal behavior, citation behavior, schema adherence, or policy compliance over time. | Drift Signal Ensemble | Drift is evaluated against calibrated baselines, not universal magic thresholds. |
| **Behavioral Health** | State where outputs, citations, actions, costs, safety checks, retries, routing, and user-visible behavior remain within expected envelopes. | Behavioral Incident Rate | High-impact behavioral failures trigger containment, review, or release gates. |
| **Green Dashboard Fallacy** | The diagnostic error of treating infrastructure health as sufficient evidence of AI-system health. | Hidden Failure Detection Rate |  Behavioral and semantic checks supplement infrastructure metrics. |
| **Trace Redaction** | Removal, masking, hashing, or reference-only storage of sensitive payloads before telemetry reaches general observability systems. | Sensitive Payload Exposure Rate | Sensitive data exposure is blocked, minimized, or isolated through controlled references. |
| **Observability SLO** | A service objective defined over behavioral, semantic, economic, or governance metrics. | SLO Compliance by Risk Class | SLOs are calibrated per workload, risk tier, and operational route. |## **The High-Dimensional AI Trace Model**

An AI interaction must not be represented as a single, flat request-response event. It functions as an asymmetrical, stateful, multi-agent execution graph where a single user prompt can trigger parallel retrieval queries, recursive formatting repairs, multi-step tool executions, and progressive model fallbacks.1 The system must trace the entire journey as a tree of nested spans connected by parent-child relationships, preserving execution state and variables across the entire lifecycle.4

### **Artifact 2: AI Trace Model**

```text
AI TRACE MODEL

Root Span
  span.kind = AGENT
  name = "Legal Review Orchestrator"
  trace_id = "8f92a10c7d3a4e9f9a1b6c5d4e3f2010"
  span_id  = "00f067aa0ba902b7"
  parent_id = null

  attributes:
    session.hash = "sha256:session_hash"
    tenant.hash = "sha256:tenant_hash"
    workflow.id = "wf_contract_review_2026_001"
    risk_tier = "high"

  Child Span
    span.kind = CHAIN
    name = "Compile Context"
    span_id = "1a2b3c4d5e6f7081"
    parent_id = "00f067aa0ba902b7"

    Child Span
      span.kind = RETRIEVER
      name = "Authorized Document Retrieval"
      span_id = "5e6f708192a3b4c5"
      parent_id = "1a2b3c4d5e6f7081"

      attributes:
        retriever.name = "policy_document_search"
        data_source.id = "policy_corpus"
        retrieval.query_hash = "sha256:query_hash"
        retrieval.candidate_count = 12
        retrieval.authorized_candidate_count = 8

      Span Event
        name = "Document Chunk Retrieved"
        event.parent_span_id = "5e6f708192a3b4c5"
        attributes:
          document.id = "doc_xyz"
          chunk.id = "chunk_014"
          document.score = 0.98
          document.version_hash = "sha256:doc_version_hash"
          document.permission_version = "perm_v17"

    Child Span
      span.kind = LLM
      name = "Generate Audit Draft"
      span_id = "3d4e5f60718293a4"
      parent_id = "1a2b3c4d5e6f7081"

      attributes:
        gen_ai.provider.name = "provider_name"
        gen_ai.request.model = "primary_reasoning_route"
        gen_ai.response.model = "primary_reasoning_route"
        gen_ai.usage.input_tokens = 4120
        gen_ai.usage.output_tokens = 780
        gen_ai.response.finish_reasons = ["tool_calls"]

    Child Span
      span.kind = TOOL
      name = "Draft Ledger Action"
      span_id = "7a8b9c0d1e2f3041"
      parent_id = "1a2b3c4d5e6f7081"

      attributes:
        tool.name = "ledger_write"
        tool.action_class = "high_risk_mutation"
        tool.payload_hash = "sha256:payload_hash"
        tool.idempotency_key_hash = "sha256:idempotency_key_hash"
        approval.status = "PENDING"

      Span Event
        name = "Maker-Checker Initiated"
        event.parent_span_id = "7a8b9c0d1e2f3041"
        attributes:
          approval.request_id = "approval_123"
          approval.risk_tier = "HIGH"
          approval.payload_hash = "sha256:payload_hash"

  Child Span
    span.kind = GUARDRAIL
    name = "PII Leak Check"
    span_id = "9c0d1e2f30415263"
    parent_id = "00f067aa0ba902b7"

    attributes:
      guardrail.name = "output_redaction"
      guardrail.result = "pass"
      redaction.count = 0
      user_disclosure_shown = false
```

Trace examples should use opaque or hashed identifiers for tenants, users, sessions, prompts, payloads, and idempotency keys. Raw user IDs, tenant names, prompt text, credentials, and tool payloads should not be propagated as general-purpose telemetry attributes.

### **Context Propagation across Distributed Transport Channels**

To maintain trace continuity across model providers, tool servers, database runtimes, and client applications, the system must enforce strict context propagation standards.1 AI transactions are transport-independent, frequently traversing HTTP REST, WebSockets, stdio subprocess pipes (for local tool integrations), and message queues within a single transaction.3

The system utilizes the W3C Trace Context specification as its baseline.3 The traceparent and tracestate parameters must be injected into all outgoing execution payloads.3 In environments where standard HTTP headers are unavailable—such as Model Context Protocol (MCP) servers operating over JSON-RPC stdio pipes—instrumentations must propagate context inside the JSON-RPC request parameters using the params._meta property bag.3

For example, a compliant JSON-RPC payload propagating a W3C Trace Context and associated baggage is structured as follows 3:

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "calculate_disbursement",
    "arguments": {
      "base_amount": 500.0,
      "is_promotional": false
    },
    "_meta": {
      "traceparent": "00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01",
      "tracestate": "vendor_key=opaque-routing-state",
      "baggage": "subject.hash=sha256_subject_hash,tenant.hash=sha256_tenant_hash,environment=prod"
    }
  },
  "id": "req_42"
}
```

Baggage should be treated as a propagation surface, not a private vault with a lanyard. Do not place raw user IDs, raw tenant IDs, credentials, policy details, or sensitive authorization state in baggage. Use opaque correlation IDs or hashes, and keep high-sensitivity authorization context inside the trusted gateway or policy engine.

The receiving server extracts the traceparent from the _meta block to establish parent-child relationships, ensuring that local tool execution spans are bound to the parent trace initiated by the client orchestrator.3

## **GenAI Span Taxonomy and Semantic Conventions**

To decouple instrumentation from specific platforms, the platform must standardize its telemetry around the OpenTelemetry GenAI Semantic Conventions (v1.41) 3 and the OpenInference tracing specification.5

The OpenTelemetry conventions are currently in *Development* status.3 To manage transitions and protect against breaking changes in production environments, instrumentations must use the environment variable OTEL_SEMCONV_STABILITY_OPT_IN=gen_ai_latest_experimental.3 This configuration forces the runtime to emit the latest experimental conventions while maintaining compatibility profiles on downstream collectors.3

### **Artifact 3: GenAI Span Taxonomy**

OpenTelemetry GenAI semantic conventions and OpenInference conventions evolve. Production systems should pin a configured convention version, translate provider-specific fields at the collector/gateway layer, and preserve compatibility mappings during upgrades.

| Span Name Format | Span Kind | Required Attributes | Optional / Provider-Specific Attributes | Sensitive Fields / Redaction Strategy |
| :---- | :---- | :---- | :---- | :---- |
| `create_agent` | INTERNAL | `agent.id`, `agent.name`, `agent.version`, `service.name` | agent description, owner, deployment profile | Redact credentials, hidden prompts, internal policy text. |
| `invoke_agent {agent.name}` | INTERNAL / CLIENT | `trace_id`, `session.hash`, `agent.id`, `workflow.id` | agent drift score, routing profile, tool manifest hash | Hash subject identifiers; avoid raw user or tenant IDs. |
| `invoke_workflow {workflow.name}` | INTERNAL | `workflow.id`, `workflow.name`, `workflow.run_id`, `workflow.status` | step count, retry count, budget state | Log state hashes and summaries, not raw payloads by default. |
| `llm.inference {model.route}` | CLIENT | provider name, request model/route, response model/route, request id, status | input/output/cached/reasoning tokens where exposed | Prompt/completion text should be redacted or stored by secure reference. |
| `retrieval {data_source.id}` | INTERNAL | data source ID, query hash, tenant/scope hash, candidate count, authorization filter version | document IDs, scores, chunk IDs, source version hashes | Do not log raw document text in general traces. |
| `rerank {reranker.route}` | INTERNAL / CLIENT | reranker route, candidate count, latency, status | top-k IDs, score distribution | Use document IDs and hashes; avoid raw snippets unless securely referenced. |
| `execute_tool {tool.name}` | INTERNAL / CLIENT | tool name, tool version, action class, payload hash, authorization decision, status | tool result hash, quota consumed, idempotency hash | Redact tool args/results; store secure reference for sensitive payloads. |
| `guardrail {policy.name}` | INTERNAL | policy id, policy version, decision, risk class | redaction count, refusal reason class, detector score | Do not log raw violating text unless policy allows secure evidence capture. |
| `human_review {queue.name}` | INTERNAL | review request id, risk tier, status, reviewer role class | review duration, outcome, override reason class | Redact reviewer/user identity or store under audit controls. |

### **Model Context Protocol Tracing Integration**

The Model Context Protocol (MCP) represents a critical integration surface for agentic tooling.1 Introduced in OTel v1.39, the MCP semantic conventions dictate that instead of generating duplicate, overlapping spans, the MCP middleware must enrich the existing execute_tool span with protocol-specific metadata.3  
When an agent invokes a tool through an MCP server, the instrumentation layer appends the following attributes to the active execute_tool span 3:

* mcp.method.name: The JSON-RPC method, e.g., tools/call.3  
* mcp.session.id: The unique cryptographic session identifier.3  
* mcp.protocol.version: The active protocol version string, e.g., 2025-06-18.3  
* rpc.response.status_code: The JSON-RPC error code, e.g., -32602 (invalid params).3  
* jsonrpc.request.id: The string representation of the request ID.3  
* network.transport: Set to pipe for STDIO-based local servers, or tcp for HTTP/SSE remote servers.3

If a tool call returns with isError: true inside the CallToolResult object, the instrumentation must set the span's error.type attribute to tool_error and update the overall span status to Error.3

## **The Multi-Plane Telemetry Map**

To construct a comprehensive representation of an AI application's behavior, telemetry must be gathered across multiple, distinct planes. These planes are not isolated; they represent correlated dimensions of a single computational system. For example, a latency spike observed on the Infrastructure Plane is diagnostic only when correlated with a route change on the Inference Plane or a cache miss on the Retrieval Plane.1

```text
TELEMETRY PLANES MAP

+--------------------------+     +--------------------------+
| Infrastructure Plane     |     | Inference Plane          |
| CPU/GPU utilization      |     | model route/provider     |
| memory/cache pressure    |     | input/output tokens      |
| queues and workers       |     | TTFT / ITL / finish      |
+-------------+------------+     +-------------+------------+
              |                                |
              v                                v
+--------------------------+     +--------------------------+
| Retrieval Plane          |     | Tool / Action Plane      |
| query hashes             |     | tool name/version        |
| candidate counts         |     | payload hash             |
| source/version IDs       |     | idempotency / status     |
| citation verification    |     | post-action verification |
+-------------+------------+     +-------------+------------+
              |                                |
              v                                v
+--------------------------+     +--------------------------+
| Agent / Workflow Plane   |     | UX / Trust Plane         |
| plan steps               |     | user corrections         |
| repair loops             |     | retry/cancel behavior    |
| no-progress states       |     | disclosures shown        |
| escalation triggers      |     | citation interactions    |
+-------------+------------+     +-------------+------------+
              |                                |
              v                                v
+--------------------------+     +--------------------------+
| Governance Plane         |     | Cost / Semantic Plane    |
| maker-checker state      |     | spend and cost velocity  |
| approvals / overrides    |     | drift/canary signals     |
| SARC constraints         |     | refusal distributions    |
| audit chain events       |     | grounding/entailment     |
+--------------------------+     +--------------------------+

Correlation spine:
  trace_id -> span_id -> workflow_id -> session.hash -> tenant.hash
  plus artifact IDs: document_id, tool_call_id, approval_request_id, route_id
```

The table below outlines the specific attributes, metrics, and correlation keys required to bind these planes into a unified analytical graph:

| Telemetry Plane | Core Attributes | Primary Metrics | Correlation Key |
| :---- | :---- | :---- | :---- |
| **Infrastructure** | container.id, gpu.device.id, process.pid 18 | gpu_cache_usage_perc, num_requests_waiting 19 | trace_id (W3C standard) |
| **Inference** | gen_ai.request.model, gen_ai.provider.name 6 | gen_ai.client.token.usage, time_to_first_token 3 | span_id (Model-call level) |
| **Retrieval** | gen_ai.data_source.id, document.id 13 | document.score, retrieval.latency 16 | document.id |
| **Tool** | gen_ai.tool.name, mcp.session.id 3 | tool.execution_time, tool.error_rate | idempotency_key |
| **Agent** | gen_ai.agent.id, workflow.run_id 13 | agent.run_duration, repair_loop_count | workflow.run_id |
| **UX & Trust** | session.id, user.id 20 | citation_click_rate, user_correction_count | session.id |
| **Governance** | maker_id, checker_id, risk_tier 2 | review_duration_seconds, override_rate | approval_requests.id 2 |
| **Cost** | usage.cost.total, tenant_id 20 | cost_velocity_usd, unattributed_cost_ratio | tenant_id |
| **Semantic** | embedding.model_name 16 | semantic_distance, entailment_drift_score | embedding.model_name |

## **Token Telemetry and Inference Economics**

Tokens represent the fundamental currency and state footprint of Generative AI workloads.1 Tracking token metrics is crucial not only for financial auditing, but also for identifying systemic failure modes and performance regressions in production architectures.1

### **Artifact 5: Token Telemetry Model**

The Token Telemetry Model decomposes token usage into granular components to isolate the economic characteristics of every step:

```
Total Token Footprint  
├── Prompt (Input) Tokens  
│   ├── User Message Input  
│   ├── System Instructions & Prompt Templates  
│   ├── Context-Injected Tokens (RAG documents, DB schemas)  
│   └── Memory-Injected Tokens (Historical chat context)  
├── Cached Input Tokens  
│   ├── Cache Read Tokens (Hits: usage.cache_read.input_tokens)  
│   └── Cache Creation Tokens (Misses: usage.cache_creation.input_tokens)  
└── Completion (Output) Tokens  
    ├── User-Visible Output Tokens  
    ├── Deliberative / Internal Computational Tokens (usage.reasoning.output_tokens)  
    ├── Tool Schema / Payload Tokens  
    └── Wasted / Abandoned Tokens (Truncated generations, cancelled streams)
```

The table below maps specific token metrics to their operational diagnostic value:

| Token Metric Attribute | Telemetry Source | Systemic Pathology Detected | Diagnostic Signature / Behavioral Pattern |
| :---- | :---- | :---- | :---- |
| `gen_ai.usage.input_tokens` | OTel/OpenInference or gateway-normalized provider field | **Context Flooding** | Monotonically growing input size, high prefill latency, falling evidence density. |
| `gen_ai.usage.output_tokens` | OTel/OpenInference or gateway-normalized provider field | **Verbosity Drift** | Output length rises for stable task profiles without quality gain. |
| `gen_ai.usage.cached_input_tokens` | Provider-specific / gateway-normalized | **Cache Starvation or Cache Regression** | Cached-token ratio drops after prompt/template/router changes. |
| `gen_ai.usage.cache_creation_tokens` | Provider-specific / gateway-normalized | **Cache Thrash** | High cache creation paired with low reuse. |
| `gen_ai.usage.reasoning_tokens` | Optional/provider-specific | **Deliberation Runaway** | Hidden/reasoning token budget rises without improved answer quality. Do not log hidden reasoning text. |
| `embedding.input_tokens` | Embedding gateway / provider metadata | **Embedding Cost Spike** | Corpus or query expansion increases embedding spend unexpectedly. |
| `rerank.input_tokens` | Reranker gateway / model metadata | **Reranker Overload** | Too many low-signal candidates passed to expensive scoring. |
| `wasted_tokens_count` | Custom gateway metric | **Cancelled / Abandoned Generation** | Tokens generated after user cancellation, timeout, or route failure. |
| `repair_loop_tokens` | Orchestrator metric | **Format Repair Loop** | Repeated schema repair consumes increasing token budget. |
| `retry_tokens` | Gateway retry ledger | **Retry Storm** | Repeated attempts inflate spend and latency after transient errors. |

### **Mathematical Modeling of Loop Token Accumulation**

In recursive agentic loops (such as ReAct or planning patterns), context size grows quadratically as the history of prior steps, tool execution results, and formatting exceptions accumulates over consecutive turns.1 The cumulative prompt tokens T_input consumed across an N-turn execution path is modeled by the quadratic context-accumulation equation:  
T_input(N) = N * S + (u * N * (N + 1)) / 2 + (r * N * (N - 1)) / 2  
Where:

* S is the fixed system prompt and tool schema definition size in tokens.1  
* u is the average size of new incoming tokens injected per iteration (the user's follow-up inputs, formatting validation exceptions, or tool outputs).1  
* r is the average model generation output size in tokens per turn.1

If an agent enters an infinite loop, this quadratic token accumulation can rapidly exhaust API budgets and trigger denial-of-wallet incidents.1 Therefore, strategic telemetry must track T_input dynamically, enabling gateways to terminate execution paths before resource limits are breached.1

## **Latency Decomposition and Performance Profiling**

In generative AI deployments, aggregate "inference latency" is an insufficient metric. It hides the underlying performance characteristics of the system, preventing SREs from identifying whether a slowdown is caused by network congestion, database performance, or hardware bottlenecks.22

### **Prefill vs. Decode Phase Mechanics**

Large Language Model inference executes across two distinct stages with fundamentally different computing characteristics and hardware constraints 7:

1. **Prefill Stage (Prompt Ingestion):** The model processes the entire prompt (including system instructions, retrieved document chunks, and chat history) in parallel, building the key-value (KV) cache.7 This stage is compute-bound, heavily utilizing the GPU's Tensor Cores.7 Prefill speed defines the Time to First Token (TTFT).7  
2. **Decode Stage (Autoregressive Generation):** The model generates subsequent completion tokens sequentially, one by one.7 Each step executes a forward pass that reads the existing KV cache from GPU High Bandwidth Memory (HBM) to generate the next token.7 This stage is memory-bandwidth-bound, leaving GPU compute cores underutilized.7 Decode speed defines the Inter-Token Latency (ITL).7

```text
PREFILL VS DECODE

Prompt / Context Tokens
  system instructions
  user message
  retrieved chunks
  tool schemas
        |
        v
+-----------------------------+
| Prefill Phase               |
| compute-bound               |
| processes prompt in bulk    |
| builds KV cache             |
| drives TTFT                 |
+-------------+---------------+
              |
              v
        KV Cache Ready
              |
              v
+-----------------------------+
| Decode Phase                |
| autoregressive              |
| memory-bandwidth sensitive  |
| generates one token step    |
| drives inter-token latency  |
+-------------+---------------+
              |
              +--> Output Token 1
              +--> Output Token 2
              +--> ...
              +--> Output Token N
```

### **Artifact 6: Latency Decomposition Model**

To isolate these bottlenecks, the system decomposes end-to-end transaction latency into stage-specific components:

| Latency Component | Telemetry Metric Source | Bottleneck / Constraint | SRE Diagnostic Action |
| :---- | :---- | :---- | :---- |
| **Input Validation** | Gateway Trace | CPU-bound network proxy | Verify gateway thread pool scaling.1 |
| **Parser / OCR** | parser.latency | CPU-bound text extraction | Monitor container memory limits.1 |
| **Retrieval** | db.execution_time | Database index lookup | Tune HNSW indexing parameters.1 |
| **Reranking** | reranker.latency | GPU-bound cross-encoder | Batch candidate documents for scoring. |
| **Queue Waiting** | gen_ai.latency.time_in_queue 19 | Serving engine congestion | Scale GPU replicas or apply rate limits.19 |
| **Prefill Phase** | vllm:time_to_first_token_seconds 19 | GPU compute (FLOPS) | Enable chunked prefill or prefix caching.7 |
| **Decode Phase** | vllm:time_per_output_token_seconds 19 | GPU memory bandwidth | Quantize weights (e.g., W8A8 or W4A16).8 |
| **Tool Execution** | tool.execution_time | Downstream API / Network | Enforce strict timeout thresholds.2 |
| **Voice STT/TTS** | voice.ttfa | Audio streaming latency | Peer WebRTC transceivers closer to client.23 |
| **Human Review** | review_duration_seconds 2 | Operator queue backlog | Route complex transactions to backup queues.2 |

### **SRE Profiling Workflows and Metrics Collection**

To capture this granular data, SRE teams use a coordinated profiling stack 22:

* **Nsight Systems / NVTX:** Tracks kernel-level execution timelines, highlighting GPU synchronization stalls and inter-GPU communication waits.22  
* **NCCL Debug Logs (export NCCL_DEBUG=INFO):** Traces collective communication calls across multi-GPU setups, identifying interconnect bottlenecks.22  
* **vLLM Prometheus Metrics:** Collects serving engine performance signals on a 15-second scraping interval.19

SREs deploy the following PromQL queries to isolate prefill bottlenecks from decode bottlenecks over rolling 5-minute windows 22:

* **Average Prefill Duration (TTFT) per Request:**  
  AveragePrefillTime = increase(vllm:time_to_first_token_seconds_sum[5m]) / increase(vllm:time_to_first_token_seconds_count[5m])  
* **Average Inter-Token Latency (ITL) during Generation:**  
  AverageITL = increase(vllm:inter_token_latency_seconds_sum[5m]) / increase(vllm:inter_token_latency_seconds_count[5m])

## **AI Error Mechanics, Retries, and Routing Decisions**

In high-dimensional AI applications, standard HTTP status codes (such as 4xx and 5xx) are insufficient for diagnosing system failures. A model call can succeed transport-wise (returning HTTP 200) while failing completely at the semantic, syntactic, or safety layer.1

### **Artifact 7: AI Error and Retry Taxonomy**

| Failure Classification | Operational Trigger Scenario | Telemetry Error Code (`error.type`) | Logged Context Payload | Mitigation / Fallback Playbook |
| :---- | :---- | :---- | :---- | :---- |
| **Syntax Violation** | Model output cannot be parsed as required JSON/XML/tool format. | `syntax_decode_error` | Response hash, redacted excerpt if allowed, parser error class. | Enter bounded repair path or return structured failure. |
| **Schema Violation** | Output parses but omits required fields or violates schema. | `schema_validation_error` | Missing/invalid field names, schema version, validation summary. | Halt execution or repair; do not invent missing required fields. |
| **Semantic Violation** | Structurally valid output violates business logic or domain constraints. | `business_rule_error` | Rule ID, field hashes/values if safe, diagnostic summary. | Block action; route to correction or human review. |
| **Provider Refusal** | Provider refuses generation due to safety/policy. | `provider_refusal` | Refusal category, provider route, policy class. | Return refusal or use approved policy-equivalent fallback only when allowed. |
| **Citation Hallucination** | Output cites source not present or not authorized in retrieval context. | `invalid_citation` | Cited source ID, context inventory hash, retrieval ID. | Remove/suppress claim or regenerate with verified evidence. |
| **Unsupported Claim** | Claim lacks entailment or source support. | `unsupported_claim` | Claim hash, evidence IDs, entailment score/class. | Mark as unverified, request evidence, or suppress. |
| **Tool Argument Violation** | Tool payload fails schema, authorization, or semantic checks. | `tool_argument_error` | Tool name, schema version, payload hash, validation summary. | Block tool call; ask clarification or route to approval. |
| **Tool Execution Loop** | Agent repeats identical or equivalent tool calls. | `tool_loop_detected` | Payload hash, repetition count, workflow ID. | Halt loop, preserve state, return managed no-progress status. |
| **Unknown Action State** | Tool timeout or partial commit leaves state unclear. | `unknown_action_state` | Tool call ID, idempotency hash, pre/post verification status. | Hold, reconcile, compensate where possible, escalate. |
| **No-Progress State** | Agent revises plans or retries without material state change. | `no_progress_error` | State hash, plan diff summary, loop step count. | Replan once, ask user, or halt based on policy. |
| **Cross-Tenant Boundary Event** | Resource, retrieval result, cache, or tool request crosses tenant scope. | `tenant_scope_violation` | Tenant hash, resource hash, policy ID, boundary class. | Block, isolate affected route, create security incident. |

| Field Name | Type | Value Range / Pattern | Operational Security Purpose |
| :---- | :---- | :---- | :---- |
| `routing_id` | String | `rt_[route_profile]_[uuid]` | Primary key for route decision trace. |
| `trace_id` | String | W3C trace ID | Correlates route with full execution trace. |
| `tenant_hash` | String | `sha256:...` | Supports tenant-level analysis without exposing raw tenant ID. |
| `session_hash` | String | `sha256:...` | Correlates session-level route behavior. |
| `requested_route` | String | Route/profile name | Captures user/app requested capability floor. |
| `selected_route` | String | Approved route/profile name | Captures actual executed route. |
| `routing_reason` | Enum | `normal`, `failover`, `quota_throttled`, `latency_slo`, `cost_policy`, `quality_floor`, `safety_policy`, `cache_hit` | Explains why route changed. |
| `quality_floor` | String | Profile ID | Ensures selected route meets task requirements. |
| `capability_delta` | Array | Lost/preserved capability labels | Identifies degraded-mode impact. |
| `budget_state` | Object | remaining spend/tokens/quota | Prevents unbounded consumption and explains throttling. |
| `policy_version` | String | Policy manifest version | Makes route decision replayable. |
| `user_disclosure_required` | Boolean | true/false | Records whether user-visible disclosure was required. |
| `user_disclosure_shown` | Boolean | true/false | Verifies UI disclosure occurred when required. |
| `route_status` | Enum | `selected`, `blocked`, `degraded`, `failed_closed` | Records route outcome. |

## **Retrieval and Tool Observability Models**

In Retrieval-Augmented Generation (RAG) and tool-driven agent environments, strategic telemetry must prove the validity of context and verify the execution of external mutations.1

### **Artifact 9: Retrieval Observability Model**

To guarantee the structural integrity of RAG setups, the system logs the complete retrieval context. This model enforces the **Doctrine of "Provenance Before Relevance"**: no document chunk is admitted to the model-facing context unless the system can prove where it came from, what authority it carries, and what transformations it has undergone.1

```json
{
  "$schema": "https://ai-engineering.canon/schemas/retrieval-observability-v1.json",
  "retrieval_id": "ret_2026_06_11_1201",
  "trace_id": "8f92a10c7d3a4e9f9a1b6c5d4e3f2010",
  "query_parameters": {
    "original_query_hash": "sha256:original_query_hash",
    "original_query_redacted": "What are our operating margins for the machinery segment?",
    "rewritten_query_hashes": [
      "sha256:rewrite_1_hash",
      "sha256:rewrite_2_hash"
    ],
    "tenant_hash": "sha256:tenant_hash",
    "permission_filter_ids": [
      "finance_auditor",
      "executive"
    ],
    "policy_version": "retrieval_policy_v12"
  },
  "retrieved_documents": [
    {
      "document_id": "doc_xyz",
      "source_ref": "secure_payload_ref:source_doc_xyz",
      "source_version_hash": "sha256:source_version_hash",
      "chunk_id": "chunk_014",
      "relevance_score": 0.98,
      "page_number": 12,
      "bounding_box": {
        "x": 0.12,
        "y": 0.34,
        "width": 0.42,
        "height": 0.08,
        "coordinate_system": "normalized_page"
      },
      "transformation_pipeline": [
        {
          "step": "pdf_text_extract",
          "version": "parser_v3.2"
        },
        {
          "step": "chunking",
          "version": "chunker_v5",
          "chunk_hash": "sha256:chunk_hash"
        },
        {
          "step": "embedding",
          "version": "embedding_model_v2",
          "embedding_hash": "sha256:embedding_hash"
        }
      ],
      "claims_supported": [
        {
          "claim_id": "claim_001",
          "claim_hash": "sha256:claim_hash",
          "redacted_text_segment": "Operating margins for the Industrial Machinery segment rose to 14.2% in Q2 2026.",
          "support_status": "supported",
          "evidence_region": {
            "page_number": 12,
            "bounding_box": {
              "x": 0.12,
              "y": 0.34,
              "width": 0.42,
              "height": 0.08,
              "coordinate_system": "normalized_page"
            }
          }
        }
      ]
    }
  ],
  "verification_signals": {
    "nli_grounding_score": 0.98,
    "source_conflict_detected": false,
    "stale_cache_served": false,
    "authorization_checked": true,
    "citation_verification_status": "verified"
  }
}
```

This artifact should prove evidence lineage without dumping raw documents into general telemetry. Auditors need source IDs, secure references, hashes, coordinates, parser/chunking versions, and verification status. They do not need your observability database cosplaying as a data breach.

This schema guarantees that if an auditor inspects an answer, they can trace the exact bounding box and page number on the original document that supported the model's output.2

### **Artifact 10: Tool Call Trace Model**

Tool call telemetry must enforce strict segregation of duties and verify system states. When an agent executes a tool, the system utilizes the Saga Pattern, decomposing multi-step operations into compensatable, pivot, and retriable transactions to ensure eventual consistency across distributed services.2

```json
{
  "$schema": "https://ai-engineering.canon/schemas/tool-trace-v1.json",
  "tool_call_id": "tool_99201_A",
  "trace_id": "8f92a10c7d3a4e9f9a1b6c5d4e3f2010",
  "tool_specification": {
    "name": "calculate_disbursement",
    "schema_version": "1.4.0",
    "action_class": "non_idempotent_mutation",
    "tool_manifest_hash": "sha256:tool_manifest_hash"
  },
  "authorization": {
    "subject_hash": "sha256:subject_hash",
    "tenant_hash": "sha256:tenant_hash",
    "scoped_credential_lifetime_seconds": 900,
    "permission_gate": "maker_checker_approval_required",
    "approval_request_id": "approval_123"
  },
  "execution_payload": {
    "payload_hash": "sha256:payload_hash",
    "redacted_argument_summary": {
      "transaction_id": "TXN-2026-0812",
      "base_amount_class": "under_1000_usd"
    },
    "idempotency_key_hash": "sha256:idempotency_key_hash"
  },
  "state_verification": {
    "pre_action_state_hash": "sha256:f02c...",
    "post_action_state_hash": "sha256:a91b...",
    "system_of_record_commit_verified": true,
    "verification_status": "verified"
  },
  "reversibility": {
    "transaction_step": "PIVOT_TRANSACTION",
    "compensation_supported": true,
    "compensation_reference": "secure_ref:compensation_plan_456",
    "compensation_status": "not_required"
  },
  "result": {
    "status": "success",
    "error_class": null,
    "completed_at": "2026-06-11T12:04:30Z"
  }
}
```

Tool telemetry should log hashes, statuses, authorization decisions, and secure references. It should not spray raw compensation endpoints, secrets, or high-impact payloads into general observability.

This structure guarantees **Eventual Consistency Gating**: the conversational interface is blocked from speaking or displaying a completion confirmation (e.g., "Your payment has been sent") until the Post-Action Auditor verifies that the database commit successfully updated the system of record.2

## **Multi-Dimensional Cost Attribution**

Enterprise AI systems process high volumes of distributed transactions, making it critical to attribute operational expenses precisely.1

### **Artifact 11: Cost Attribution Model**

The Cost Attribution Model associates every unit of consumed resource back to its corresponding system entity, enabling real-time cost allocation and anomaly detection:

| Allocation Dimension | Primary Attribution Fields | Tracked Resource Component | Pricing Metrics Formula |
| :---- | :---- | :---- | :---- |
| **B2B Tenant** | tenant_id 20, tenant_tier 2 | Vector indexing, multi-tenant model calls.1 | Cost_tenant = sum(Cost_tokens + Cost_storage + Cost_review) 1 |
| **Interactive Session** | session.id 20 | User conversation history, memory read/writes.2 | Cost_session = sum_turns(T_input * P_input + T_output * P_output) |
| **Agent / Workflow** | gen_ai.agent.id 13, workflow.id 2 | Multi-step agent planning, self-reflection loops.1 | Cost_workflow = sum_steps(Cost_model + Cost_tools) |
| **Parsing & OCR** | parser.name, document.id 16 | Layout analysis, PDF rasterization, OCR hosting.23 | Cost_parse = N_pages * Price_page + t_compute * Price_compute |
| **Security Scanning** | guardrail.id | Input Prompt Shields, output ARGUS scans.1 | Cost_security = sum(Cost_classifiers + Cost_regex) |
| **Human Governance** | approval_requests.id 2 | Centralized queue-based maker-checker reviews.2 | Cost_human = t_review_minutes * Price_operator 1 |
| **Incident / Outage** | incident.id | Out-of-bounds runaway loops, thundering-herd retries.1 | Cost_incident = Cost_runaway_tokens + Cost_failed_retries 1 |

## **Semantic Drift and Behavioral Regression Monitoring**

Language models are highly dynamic and non-deterministic.1 Over time, changes in context distributions, silent provider updates, or updates to retrieval databases can cause gradual or sudden shifts in meaning, tone, and safety compliance.1 Because these regressions are semantic and behavioral, traditional statistical tests (such as KL divergence) fail to detect them, and infrastructure dashboards remain green.1

### **Artifact 12: Semantic Drift Model**

To detect and isolate behavioral regressions, the system deploys a multi-layered Semantic Drift Model, using triangulation across multiple independent dimensions:

```
  Semantic Drift Triangulation  
  ├── Dimensionality-Reduced Embedding Shift  
  │   ├── Cosine Distance from Reference Centroid (Threshold: > 0.15)   
  │   └── Reconstruction Loss via Trained Autoencoder (Threshold: > 3 STDEV)   
  ├── Task-Based Activation Analysis  
  │   └── Hidden Layer Activation Pattern Variance   
  ├── Natural Language Inference (NLI) Grounding Scores  
  │   └── Entailment vs. Contradiction Ratios on Reference Claims   
  └── Scheduled Canary Prompt Inspections  
      └── Scheduled Execution of Curated Prompts (Similarity, Perplexity, Tone) 
```

The table below outlines the implementation details and metrics for each validation layer:

| Validation Layer | Underlying Core Algorithm | Telemetry Metrics Captured | Regression Warning Threshold |
| :---- | :---- | :---- | :---- |
| **Embedding Space** | PCA/UMAP projection, centroid shift, Wasserstein or cosine-distance comparison. | `semantic_distance_metric`, `centroid_shift`, `distribution_distance` | Threshold calibrated against baseline window and task domain. |
| **Reconstruction Loss** | Autoencoder or density model trained on baseline output embeddings. | `reconstruction_error`, `outlier_score` | Alert when sustained error exceeds calibrated baseline band. |
| **Domain Classifier** | Classifier distinguishing baseline vs. current outputs. | `classifier_auc_score`, `domain_shift_probability` | High separability indicates distribution shift requiring review. |
| **Canary Prompts** | Scheduled execution of curated prompts with fixed route/profile controls. | `canary_similarity_score`, `schema_pass_rate`, `policy_pass_rate` | Alert on sustained degradation against canary baseline. |
| **NLI / Evidence Support** | Cross-encoder or verifier scoring claim support against evidence. | `nli_entailment_ratio`, `unsupported_claim_rate` | Threshold set by task risk; high-impact domains use stricter floors. |
| **Refusal Distribution** | Clustering/classification of refusal and safety outcomes. | `refusal_rate`, `refusal_cluster_entropy`, `policy_category_shift` | Alert when refusal behavior shifts materially for stable task mix. |
| **User Correction Signals** | Analysis of edits, corrections, thumbs-downs, retries, and abandonments. | `user_correction_count`, `edit_distance`, `retry_after_answer_rate` | Treat as UX/behavior signal, not standalone truth label. |

## **The Green Dashboard Fallacy Diagnostic**

The Green Dashboard Fallacy is the critical diagnostic error of assuming an AI system is healthy based solely on infrastructure metrics.1 To prevent this, platform teams must monitor behavioral indicators of degradation that hide behind green infrastructure signals.1

### **Artifact 13: Green Dashboard Fallacy Diagnostic**

This diagnostic maps silent behavioral failures to their corresponding detection signals and telemetry check locations:

| Observed Behavioral Symptom | Green Infrastructure Signal | Hidden Systemic Failure | Telemetry Check Location / Correlation Key |
| :---- | :---- | :---- | :---- |
| **User Correction Spike** | HTTP 200 OK; latency <= 150 ms | Model is repeatedly outputting formatted but incorrect financial data.1 | user_correction_count correlated with edit_character_diff.2 |
| **Citation Click Collapse** | CPU/GPU load normal; database active | Model is generating decorative, ungrounded citation badges post-hoc.1 | citation_click_rate drops below 15.0% over 1 hour.2 |
| **Stale Cache Overuse** | TTFT <= 10 ms (Cache hits) 2 | System is serving outdated policy data, bypassing live databases.1 | stale_cache_served matches expired Redis TTL metadata.2 |
| **No-Progress Agent Loops** | GPU utilization active; APIs active | Agent is stuck repeating identical tool calls with different formatting.1 | state_repetition_count (C_rep >= 2) inside the active task plan.1 |
| **Reviewer Complacency** | Queue throughput high; no alerts | Reviewers are rubber-stamping high-risk payments without verification.2 | average_modal_approval_duration drops below 350 ms.2 |
| **Silent Model Downgrade** | Cost drops; system remains online | Gateway proxy silently routed requests to smaller, low-quality models.2 | selected_model differs from client's requested_model.2 |

### **Artifact 14: AI Observability Dashboard Taxonomy**

To make behavioral health visible to operators, the system organizes telemetry across six specialized, correlated dashboard views:

```text
AI OBSERVABILITY DASHBOARD TAXONOMY

+--------------------------+     +--------------------------+
| Serving Health           |     | Behavioral Health        |
| queue depth              |     | groundedness ratio       |
| TTFT / ITL               |     | schema pass rate         |
| GPU/KV cache pressure    |     | unsupported claims       |
| provider errors          |     | refusal distribution     |
+--------------------------+     +--------------------------+

+--------------------------+     +--------------------------+
| Retrieval & Evidence     |     | Agent & Tool Behavior    |
| retrieval fan-out        |     | tool retries             |
| citation verification    |     | no-progress loops        |
| stale cache use          |     | action verification      |
| source conflicts         |     | idempotency failures     |
+--------------------------+     +--------------------------+

+--------------------------+     +--------------------------+
| Governance & Review      |     | Cost & Drift             |
| maker-checker state      |     | tenant spend             |
| approval latency         |     | cost velocity            |
| override rate            |     | embedding drift          |
| break-glass events       |     | canary regressions       |
+--------------------------+     +--------------------------+
```

## **Telemetry Privacy, Redaction, and Compliance**

Strategic telemetry can capture prompts, retrieved documents, tool arguments, model responses, reviewer decisions, and action traces. These payloads may contain PII, PHI, payment data, credentials, secrets, proprietary code, internal policies, or regulated business records. Observability must not become the vulnerability it was designed to diagnose.

The default posture should be:

* capture metadata by default;
* capture raw payloads only when explicitly allowed;
* redact before general telemetry storage;
* store sensitive payloads behind controlled references;
* log access to sensitive telemetry;
* retain sensitive payloads for the shortest policy-compatible period;
* keep audit evidence sufficient for replay without dumping raw secrets everywhere.

### **Redaction and Payload Handling**

Regex and NER filters are useful but incomplete. They can miss secrets, over-redact important evidence, or fail on unfamiliar formats. Redaction should therefore be layered:

| Layer | Purpose |
| :--- | :--- |
| **Structured Field Policy** | Never log known secret fields, credentials, tokens, private keys, or raw auth headers. |
| **Classifier / Regex / NER Scan** | Detect common PII, payment data, credentials, and regulated entities. |
| **Source-Aware Redaction** | Apply stricter rules to medical, legal, financial, HR, tenant-private, and security-sensitive payloads. |
| **Secure Reference Storage** | Store sensitive payloads outside general trace stores with purpose-bound access. |
| **Access Auditing** | Log who accessed sensitive payload references and why. |
| **Retention Control** | Expire sensitive payloads separately from lower-risk metadata. |

### **External Payload Reference-Only Tracing**

```text
REFERENCE-ONLY TELEMETRY FLOW

[ Application / Gateway ]
  receives prompt, tool payload, retrieved text, or model output
        |
        v
[ Redaction and Classification Gate ]
  classify sensitivity
  redact safe fields
  decide metadata-only vs secure-reference capture
        |
        +--> low-risk metadata
        |       write directly to trace span
        |
        v
[ Secure Payload Vault ]
  encrypted storage
  short TTL
  purpose-bound access
  access logging
  tenant/user scope controls
        |
        v
[ Trace Collector ]
  stores metadata only:
    payload_ref
    payload_hash
    sensitivity class
    retention class
    access policy id
```

Reference-only tracing minimizes sensitive payload exposure. It does not make telemetry “PII-free” by magic wand; metadata can still be sensitive, and payload references require access control, retention policy, and audit logs.


### **Artifact 15: Telemetry Privacy and Redaction Model**

The table below defines the content capture and redaction policies enforced by the platform:

| Content Class | Captured Data Format | Storage Location / Substrate | Access Control Rules | Retention Period | Compliance Enforcement |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **User Prompts** | Metadata, hash, redacted excerpt, or secure payload reference. | Secure payload vault if retained; metadata in trace store. | Purpose-bound access; audited retrieval. | Short TTL by default; policy-defined extension for incidents. | Deletion/export hooks where legally required. |
| **API Credentials / Secrets** | Never intentionally captured; block or hash only for detection event. | None for raw secret; detection event in security log. | Security team only for detection metadata. | Minimal retention for incident evidence. | Secret scanners and log egress controls. |
| **Tool Arguments** | Payload hash, redacted summary, schema version, secure reference when needed. | Action ledger / secure payload vault. | Auditor/security access by purpose and role. | Based on audit and transaction requirements. | Signed ledger and access logs. |
| **System Prompts / Templates** | Version ID, template hash, release reference; exact text only in controlled registry. | Prompt/model registry, not general traces. | Dev/security/governance access. | Release lifecycle retention. | Change-control and approval logs. |
| **Model Completions** | Metadata, hash, redacted excerpt, or secure payload reference. | Secure payload vault if retained. | Purpose-bound access with audit. | Short TTL by default; longer only for incidents/evals. | Privacy review and retention policy. |
| **Citation Evidence** | Source ID, page/section/coordinates, source version hash, secure evidence reference. | Document registry / evidence store. | Same or stricter scope as source document. | Bound to source lifecycle and audit policy. | RLS/ACL checks and citation verification. |
| **Trace Metadata** | Normalized numeric/string attributes, status, hashes, IDs. | Central telemetry store. | SRE/developer access depending on sensitivity. | Operational retention window. | Redaction checks before ingestion. |
| **Human Review Data** | Review request ID, outcome, role class, redacted comments, evidence refs. | Review/audit system. | Reviewer, governance, audit roles. | Governance/audit retention policy. | Access audit and policy review. |

## **Alerting and SRE SLO Design**

In strategic telemetry, Service Level Objectives (SLOs) must be defined over behavioral and semantic metrics to protect user trust and operational safety.1

### **Artifact 16: Alerting and SLO Model**

The SRE team configures and monitors the following behavioral SLOs and alerting thresholds:

| SLO Category | Target SLI Metric | SLO Target Threshold | Warning Alert Threshold | Critical Alert Threshold | Runbook / Automated Containment Action |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Semantic Health** | Claim support / entailment by task profile. | Calibrated per domain and risk tier. | Sustained support drop outside baseline band. | High-impact route falls below required evidence floor. | Freeze rollout, route to safer profile, require evidence refresh. |
| **Action Integrity** | Tool schema and post-action verification status. | High-impact actions require verified state. | Repeated validation failures on any tool route. | High-impact mutation has unknown or failed verification. | Block affected route; hold/reconcile/compensate where possible. |
| **Cost Control** | Cost velocity and budget consumption. | Spend remains within tenant/workflow budget. | Burn rate exceeds forecast. | Budget breach or denial-of-wallet pattern. | Trip circuit breaker, throttle, or require approval. |
| **Loop Containment** | No-progress count and repeated state hash. | Loops halt within workflow profile. | Repeated state detected. | Repeated state plus rising cost/tool calls. | Halt agent, preserve state, request clarification or escalate. |
| **Trust Calibration** | User corrections, abandonment, disclosure acknowledgement, review overrides. | Stable within task baseline. | Correction/abandonment spike. | High-risk workflow shows systematic reviewer/user miscalibration. | Add targeted cognitive forcing, review UI, or route audit. |
| **Tenant Safety** | Cross-tenant access/cache/retrieval mismatch events. | No accepted cross-tenant boundary violation. | Blocked mismatch event detected. | Confirmed exposure or repeated attempted boundary crossing. | Isolate affected scope, revoke implicated credentials, create incident. |
| **Privacy / Redaction** | Sensitive payload exposure events. | No raw secrets in general telemetry. | Redaction detector catches sensitive field before storage. | Sensitive payload reaches unauthorized telemetry sink. | Quarantine trace, revoke exposed secret, investigate access. |
| **Fallback Integrity** | Route downgrade with disclosure and quality-floor preservation. | All degradations are policy-preserving and observable. | Fallback spike or missing disclosure. | Unsafe fallback or silent downgrade on high-impact task. | Disable route, fail closed, or require user/reviewer choice. |

## **Systemic Cross-Canon Handoff Map**

Strategic telemetry provides the evidence substrate used across the canon. It connects behavioral health, cost, retrieval, tool execution, governance, privacy, evaluation, audit, and incident response.

| Target Canon Report | Functional Domain | Core Telemetry Handoff | Operational Integration Rule | Fallback / Degraded Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context and State Governance | Context object IDs, memory inclusion events, compaction traces. | State changes must be traceable across route switches and summaries. | Preserve state hashes and continuity checkpoints. |
| **AI-ENG-D** | Corpus Engineering | Source IDs, source version hashes, provenance, lifecycle state. | Corpus-derived evidence must retain lineage in traces. | Exclude or quarantine unknown-provenance sources. |
| **AI-ENG-E** | Retrieval Pipeline | Retrieval IDs, query hashes, candidate counts, citation verification. | Retrieval traces must show authorization, source version, and evidence sufficiency. | Use managed no-evidence response if retrieval cannot be trusted. |
| **AI-ENG-F** | Freshness and Conflict Detection | Source age, cache age, conflict flags, freshness status. | Telemetry must expose stale/conflicting evidence. | Block stale high-impact answers or require refresh. |
| **AI-ENG-L** | Serving Architecture | TTFT, ITL, queue wait, route, cache, model/provider status. | Serving decisions must correlate with user-visible behavior and cost. | Shift to approved fallback route only if quality floor holds. |
| **AI-ENG-M** | Agentic Orchestration | Workflow run IDs, loop counts, no-progress state hashes. | Agents must emit enough telemetry to halt and replay loops. | Halt, replan, clarify, or escalate on repeated no-progress. |
| **AI-ENG-N** | Tool Contracts | Tool call IDs, schema versions, payload hashes, error classes. | Tool traces must prove schema, authorization, and idempotency status. | Block or hold unknown/high-impact tool states. |
| **AI-ENG-O** | Action Verification | Pre/post state hashes, verification status, action ledger. | Completion claims require verified state telemetry. | Hold, reconcile, compensate, or escalate unknown state. |
| **AI-ENG-P** | Multimodal Understanding | Parser versions, OCR confidence, evidence coordinates, media refs. | Visual/document evidence must be inspectable without leaking raw payloads. | Use secure evidence references or manual verification. |
| **AI-ENG-Q** | Voice Interaction | STT/TTS latency, transcript confidence, turn/endpointing events. | Voice degradation and confirmation states must be traceable. | Switch to text/card confirmation for high-impact actions. |
| **AI-ENG-R** | UI Agents | Browser session IDs, DOM snapshots hashes, action verification. | UI actions must be observable from observation to post-action state. | Pause automation on drift or uncertainty. |
| **AI-ENG-S** | Production Pathologies | Error classes, malformed outputs, repair loops, false success signals. | Behavioral pathologies require typed telemetry and replay traces. | Contain, repair, or route to degraded mode. |
| **AI-ENG-T** | Boundary Defense | Tenant scope, authorization decisions, cache scope, redaction events. | Boundary violations must emit security telemetry without leaking payloads. | Fail closed and create incident record when scope cannot be trusted. |
| **AI-ENG-U** | Supply Chain Security | Artifact hashes, model signatures, parser/tool versions, sandbox events. | Loaded artifacts must be traceable to approved supply-chain records. | Quarantine unsigned or anomalous artifacts. |
| **AI-ENG-V** | Resource Abuse | Token burn, cost velocity, quota burn, retry storms, queue depth. | Resource anomalies must be observable by tenant/session/workflow. | Throttle, circuit-break, or require approval. |
| **AI-ENG-W** | UX Resilience | Fallback route, disclosure shown, preserved state, degraded status. | Degraded mode must be traceable as a product state. | Use approved degraded mode, partial answer, review, or fail closed. |
| **AI-ENG-X** | User Trust and Transparency | User corrections, disclosures, citation interactions, contestability events. | Trust signals must be interpreted as behavioral telemetry, not truth proof. | Surface clearer status/evidence or route to review. |
| **AI-ENG-Y** | High-Impact Workflow Design | Approval request IDs, maker/checker state, review outcomes, break-glass events. | Governance actions require audit-grade telemetry. | Hold execution until approval/reconciliation is traceable. |
| **AI-ENG-AA** | Evaluations | Golden traces, canary outputs, drift signals, adversarial cases. | Evaluation harnesses consume production-shaped telemetry. | Block release on telemetry-backed regression. |
| **AI-ENG-AB** | Audit and Replay | Trace IDs, payload hashes, secure references, policy versions, action ledgers. | Replay must reconstruct the decision path without raw uncontrolled logs. | Preserve redacted, hash-bound evidence in audit store. |
| **AI-ENG-AC** | Incident Response | Incident IDs, containment events, route/corpus/tool/cache quarantine. | Incidents require structured telemetry for scope and timeline reconstruction. | Quarantine affected route/artifact/cache/tool and notify owners. |
| **AI-ENG-AD** | Governance and Compliance | Policy version, review status, retention class, access logs. | Governance defines what telemetry is mandatory, restricted, or deleted. | Route telemetry exceptions to accountable owner. |
| **AI-ENG-AJ** | Reference Architecture | Trace collector, policy-aware gateway, secure payload vault, audit store. | Reference systems should implement strategic telemetry by default. | Use metadata-first, reference-only tracing for sensitive payloads. |

## **Strategic Conclusions and Architectural Recommendations**

To transition from ad-hoc monitoring to high-assurance behavioral governance, enterprise platforms must treat telemetry as core software infrastructure.1 Based on the analyses compiled in this report, the following four strategic principles are recommended for deployment architectures:

1. **Centralize Telemetry at the Gateway Layer:** Avoid writing custom instrumentation, rate-limiting, and error-handling logic inside individual application services.2 Centralize strategic telemetry collection within a high-performance, budget-aware runtime gateway positioned entirely outside the model's cognitive boundary.1  
2. **Enforce Strict Gating Before Verifications:** Never allow conversational interfaces to generate verbal or text-based confirmation claims until the Post-Action Auditor verifies that the database transaction has been successfully committed in the system of record.2 Spoken or generated claims must never outrun physical reality.2  
3. **Secure Caches against Timing Side-Channels:** Formulated semantic cache keys must be cryptographically bound to the tenant ID and user permissions.1 Serving runtimes must deploy selective prefix isolation (such as the CacheSolidarity framework) to prevent timing side-channel probes from exfiltrating private context.1  
4. **Isolate High-Risk Content and Payloads:** Enforce a zero-trust payload logging policy.1 Strip credentials at the gateway, run ARGUS output filters to redact PII, and utilize reference-only tracing to store sensitive inputs in secure, short-TTL external storage rather than writing them to centralized, persistent trace collectors.1

## **Durable Principles of Strategic Telemetry**

1. **Green Infrastructure Does Not Prove Behavioral Health**  
   HTTP 200, low latency, and normal GPU utilization can coexist with hallucinations, tenant leaks, false success, stale cache, and unsafe actions.

2. **Trace the Workflow, Not Just the Request**  
   AI interactions span model calls, retrieval, tools, policy gates, human review, retries, routing, and state verification. A single request log is not enough.

3. **Telemetry Must Preserve Meaning-Level Evidence**  
   Strategic telemetry should capture claims, citations, evidence IDs, tool states, route decisions, and verification outcomes—not just duration and status.

4. **Sensitive Payloads Need Controlled References**  
   Raw prompts, completions, documents, and tool arguments should not be sprayed into trace stores. Use hashes, redacted summaries, and secure payload references.

5. **Token Metrics Are Economic and Behavioral Signals**  
   Token growth, cache misses, repair loops, reasoning-token spikes, and wasted tokens reveal cost bombs and behavioral pathologies.

6. **Latency Must Be Decomposed by Stage**  
   End-to-end latency hides parser, retrieval, queue, prefill, decode, tool, voice, and human-review bottlenecks.

7. **Tool Success Is Not Action Truth**  
   Tool traces must record authorization, idempotency, execution status, and post-action verification. A returned response is not proof of a committed state.

8. **Drift Requires Multiple Signals**  
   Embedding movement, canary prompts, NLI support, refusal shifts, schema pass rates, and user corrections should be triangulated. No single drift metric is oracle-grade. Obviously. The oracle budget was denied by finance.

9. **Telemetry Is a Security Boundary**  
   Observability systems hold sensitive operational evidence. They need redaction, access control, retention policy, and audit trails.

10. **Fallbacks and Degraded Modes Must Be Observable**  
   Model downgrades, cache responses, partial answers, review handoffs, and fail-closed states must emit structured events.

11. **Audit and Replay Depend on Telemetry Discipline**  
   Reproducibility requires trace IDs, payload hashes, policy versions, route decisions, evidence references, and action ledgers.

12. **Telemetry Without Action Is Decorative Plumbing**  
   Alerts must connect to runbooks, release gates, containment actions, governance review, and incident response.

#### **Works cited**

1. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
2. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
3. AI Agent Observability 2026: Tracing & Monitoring Stack, accessed June 11, 2026, [https://www.digitalapplied.com/blog/ai-agent-observability-2026-tracing-monitoring-stack-guide](https://www.digitalapplied.com/blog/ai-agent-observability-2026-tracing-monitoring-stack-guide)  
4. Tracing Concepts - Arize AX Docs, accessed June 11, 2026, [https://arize.com/docs/ax/instrument/what-are-traces](https://arize.com/docs/ax/instrument/what-are-traces)  
5. OpenInference Specification - GitHub Pages, accessed June 11, 2026, [https://arize-ai.github.io/openinference/spec/](https://arize-ai.github.io/openinference/spec/)  
6. How OpenTelemetry Traces LLM Calls, Agent Reasoning, and MCP Tools | Greptime, accessed June 11, 2026, [https://greptime.com/blogs/2026-05-09-opentelemetry-genai-semantic-conventions](https://greptime.com/blogs/2026-05-09-opentelemetry-genai-semantic-conventions)  
7. Prefill and Decode: A Technical Guide to the Two Phases of Inference - WEKA, accessed June 11, 2026, [https://www.weka.io/learn/ai-ml/prefill-and-decode/](https://www.weka.io/learn/ai-ml/prefill-and-decode/)  
8. Prefill vs Decode: LLM Inference Phases Explained - Redis, accessed June 11, 2026, [https://redis.io/blog/prefill-vs-decode/](https://redis.io/blog/prefill-vs-decode/)  
9. Optimizing LLM Inference: Prefill vs Decode, Latency vs Throughput | by Maghonei | Medium, accessed June 11, 2026, [https://medium.com/@maghonei/optimizing-llm-inference-prefill-vs-decode-latency-vs-throughput-80dbf00fc0ba](https://medium.com/@maghonei/optimizing-llm-inference-prefill-vs-decode-latency-vs-throughput-80dbf00fc0ba)  
10. 9 Best LLM Drift Monitoring Platforms in 2026 - Galileo AI, accessed June 11, 2026, [https://galileo.ai/blog/best-llm-output-drift-monitoring-platforms](https://galileo.ai/blog/best-llm-output-drift-monitoring-platforms)  
11. Semantic Drift Analysis - Emergent Mind, accessed June 11, 2026, [https://www.emergentmind.com/topics/semantic-drift-analysis](https://www.emergentmind.com/topics/semantic-drift-analysis)  
12. Translating Semantic Conventions - Phoenix - Arize AI, accessed June 11, 2026, [https://arize.com/docs/phoenix/tracing/concepts-tracing/translating-conventions](https://arize.com/docs/phoenix/tracing/concepts-tracing/translating-conventions)  
13. Semantic Conventions for GenAI agent and framework spans - OpenTelemetry, accessed June 11, 2026, [https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/)  
14. Semantic Conventions for Generative AI Agentic Systems (gen_ai.*) · Issue #35 - GitHub, accessed June 11, 2026, [https://github.com/open-telemetry/semantic-conventions-genai/issues/35](https://github.com/open-telemetry/semantic-conventions-genai/issues/35)  
15. Arize-ai/openinference: OpenTelemetry Instrumentation for AI Observability - GitHub, accessed June 11, 2026, [https://github.com/Arize-ai/openinference](https://github.com/Arize-ai/openinference)  
16. Semantic Conventions | openinference - GitHub Pages, accessed June 11, 2026, [https://arize-ai.github.io/openinference/spec/semantic_conventions.html](https://arize-ai.github.io/openinference/spec/semantic_conventions.html)  
17. Openinference Semantic Conventions - Arize AX Docs, accessed June 11, 2026, [https://arize.com/docs/ax/observe/tracing-concepts/openinference-semantic-conventions](https://arize.com/docs/ax/observe/tracing-concepts/openinference-semantic-conventions)  
18. OTEL.md - containers/kubernetes-mcp-server - GitHub, accessed June 11, 2026, [https://github.com/containers/kubernetes-mcp-server/blob/main/docs/OTEL.md](https://github.com/containers/kubernetes-mcp-server/blob/main/docs/OTEL.md)  
19. Observing vLLM with OpenTelemetry and Dash0, accessed June 11, 2026, [https://www.dash0.com/blog/observing-vllm-with-opentelemetry-and-dash0](https://www.dash0.com/blog/observing-vllm-with-opentelemetry-and-dash0)  
20. genai-otel-instrument - PyPI, accessed June 11, 2026, [https://pypi.org/project/genai-otel-instrument/0.1.24/](https://pypi.org/project/genai-otel-instrument/0.1.24/)  
21. LLM Monitoring & Drift Detection Guide | Metrics, Tools & Examples - Leanware, accessed June 11, 2026, [https://www.leanware.co/insights/llm-monitoring-drift-detection-guide](https://www.leanware.co/insights/llm-monitoring-drift-detection-guide)  
22. LLM Inference Optimization — Prefill vs Decode | by Robi Kumar ..., accessed June 11, 2026, [https://pub.towardsai.net/llm-inference-optimization-prefill-vs-decode-6e003d48b2ca](https://pub.towardsai.net/llm-inference-optimization-prefill-vs-decode-6e003d48b2ca)  
23. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
24. sentinel/docs/DETECTION_METHODS.md at main · LLM-Dev-Ops ..., accessed June 11, 2026, [https://github.com/LLM-Dev-Ops/sentinel/blob/main/docs/DETECTION_METHODS.md](https://github.com/LLM-Dev-Ops/sentinel/blob/main/docs/DETECTION_METHODS.md)  
25. Monitor embedding drift for LLMs deployed from Amazon SageMaker JumpStart - AWS, accessed June 11, 2026, [https://aws.amazon.com/blogs/machine-learning/monitor-embedding-drift-for-llms-deployed-from-amazon-sagemaker-jumpstart/](https://aws.amazon.com/blogs/machine-learning/monitor-embedding-drift-for-llms-deployed-from-amazon-sagemaker-jumpstart/)

---

# AI-ENG-AA — Evals Architecture: Ground Truth, Golden Sets & Regression Tests

## **Doctrinal Foundations of High-Dimensional AI Evaluation**

In high-dimensional artificial intelligence systems, production reliability cannot be measured by traditional application performance monitoring (APM) paradigms.1 In legacy architectures, system health is treated as a binary or scalar state defined by infrastructure metrics, such as database availability, memory allocation, network latency, and HTTP status codes. However, when these architectures are driven by probabilistic large language models (LLMs) and multi-agent execution loops, backend availability does not guarantee a successful, safe, or contextually coherent transaction.1 An artificial intelligence gateway can return a successful HTTP 200 payload that contains a complete structural schema failure, an unauthorized tool execution, a cross-tenant data leak, or an ungrounded factual confabulation.1 Conversely, a system can exhibit optimal hardware utilization and high throughput while delivering answers that silently violate safety boundaries, omit critical citations, or trap agentic loops in expensive, infinite executions. This mismatch establishes the Green Dashboard Fallacy: the diagnostic error of assuming an AI system is healthy based solely on infrastructure availability.

Strategic telemetry, as established in the preceding canon doctrines, serves as the first-class observability discipline designed to resolve this fallacy by capturing the complete execution path of probabilistic interactions as a tree of nested spans. Evaluation architecture uses this telemetry substrate to construct durable, systematic testing systems that transition evaluations from passive demo scoring or leaderboard metrics into active product infrastructure.1 The evaluations architecture defines the layered testing and regression discipline that proves whether an AI system’s behavior remains acceptable across continuous changes to models, prompts, retrieval indexes, corpora, tool contracts, routers, adapters, safety filters, parsers, UI controls, and governance workflows.1

The governing doctrine of this architecture dictates that AI systems require layered evaluations because no single score can validate probabilistic behavior. Task success, retrieval quality, grounding, citations, tool execution, safety, regression stability, human judgment, adversarial resistance, and governance compliance must be evaluated separately and then composed into automated release gates.1 Under this paradigm, the primary engineering question shifts from "Did the model perform well on a benchmark?" to "Can the system prove that today’s model, prompt, retrieval index, tool contracts, routing logic, and UI behavior still satisfy yesterday’s behavioral guarantees?".1 This evaluations architecture must outlive any single evaluation platform, benchmark, judge model, or provider feature, establishing the curated behavioral specification—encoded in test cases, rubrics, labels, traces, thresholds, and gates—as the durable asset of the enterprise.

## **Conceptual Glossary**

The systems-engineering parameters and operational metrics governing high-dimensional evaluation architectures, golden datasets, and regression testing pipelines are defined in the primary conceptual glossary:

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Evaluations Architecture** | The layered testing, scoring, regression, and release-control framework that validates probabilistic system behavior across model, prompt, retrieval, tool, interface, and governance changes. | Behavioral Coverage by Risk Class | Critical behavior classes are represented by versioned tests, traces, rubrics, and gates. |
| **Ground Truth** | A curated, scoped specification of expected outcomes, including text, evidence, tool parameters, state transitions, refusals, safety boundaries, and review requirements. | Ground Truth Alignment Score | Alignment targets are calibrated by task risk and truth type. |
| **Golden Set** | A curated, versioned, representative set of test cases encoding required product behaviors, regressions, incidents, and edge cases. | Golden Set Relevance Density | Key task classes, risk tiers, and failure modes are represented without excessive redundancy. |
| **Canary Prompt** | A recurring reference case used to detect silent behavior changes in models, prompts, routes, indexes, or policy layers. | Canary Drift Score | Drift is evaluated against calibrated baselines and expected variance. |
| **Task Evaluation** | Assessment of task-specific outputs against logical, structural, semantic, and behavioral requirements. | Task Success Rate | Targets are domain-specific and tied to risk class. |
| **Retrieval Evaluation** | Measurement of evidence-finding quality independent of downstream generation. | MRR / nDCG / Recall / Context Precision | Retrieval quality meets task-specific evidence requirements under authorization constraints. |
| **Grounding Evaluation** | Testing whether generated claims are supported by available evidence. | Supported Claim Ratio | High-impact claims require evidence support or explicit uncertainty. |
| **Citation Evaluation** | Verification that citations point to the exact source spans, coordinates, records, or artifacts supporting local claims. | Citation Alignment Score | Citations are valid for the configured evidence format and task risk. |
| **Tool-Use Evaluation** | Validation of tool selection, argument generation, authorization, idempotency, execution status, and post-action verification. | Tool Verification Coverage | High-impact tool actions require pre-action gates and verified post-action state. |
| **Adversarial Evaluation** | Testing system boundaries using prompt injection, poisoned retrieval, hostile documents, unsafe tools, context floods, and output-sink attacks. | Containment Failure Rate | Attacks are contained by system boundaries even when model behavior is imperfect. |
| **Synthetic Evaluation** | Generation of additional test cases from controlled seeds, schemas, traces, or behavioral parameters. | Synthetic Promotion Rate | Synthetic cases are coverage amplifiers until validated for hard gates. |
| **LLM-as-Judge** | Use of language models to evaluate open-ended outputs against explicit rubrics, usually with calibration and human-labeled references. | Judge Calibration Error | Judge scores are calibrated against held-out human labels before release gating. |
| **Human Label** | Expert or trained-rater annotation performed under a locked rubric and quality-control process. | Label Reliability | Reliability thresholds are calibrated by task type, difficulty, and decision impact. |
| **Inter-Rater Reliability** | Statistical measurement of agreement among human or automated evaluators scoring identical items. | Krippendorff’s Alpha / Cohen’s Kappa | Required agreement depends on whether labels are exploratory, training, or release-gating labels. |
| **Regression Gate** | Automated CI/CD or deployment check that evaluates system behavior against versioned datasets and release rules. | Gate Outcome by Severity | Hard gates block; soft gates require owner signoff; canaries monitor controlled rollout. |
| **Drift Gate** | Continuous or scheduled gate that flags behavior changes in outputs, retrieval, routing, safety, or policy compliance. | Drift Signal Ensemble | Drift thresholds are calibrated against baseline windows and workload profiles. |
| **Eval Contamination** | Leakage of evaluation cases, labels, or canary material into training, retrieval, prompt examples, or public corpora. | Contamination Risk Signal | Contamination is detected, investigated, and mitigated; canaries are evidence, not force fields. |

## **The Multi-Layered Eval Architecture Map**

The structural integrity of an evaluation framework depends on a multi-layered design that isolates failures at specific system boundaries.1 Production AI systems do not fail as flat, single-prompt interfaces; they fail through compounding, silent errors across distinct software and context boundaries.5 The evaluation architecture must construct specialized validation layers that mirror the multi-plane execution path of the application:

| Evaluation Layer | Structural Scope | Primary Telemetry & Ingestion Inputs | Verification Mechanics |
| :---- | :---- | :---- | :---- |
| **Telemetry Layer** | Captures traces, spans, tokens, latency, routing, cost, retrieval, tool, and governance events. | OpenTelemetry/OpenInference-style spans, redacted traces, payload hashes, secure references. | Validates trace completeness, required attributes, redaction, and correlation keys. |
| **Dataset Layer** | Houses golden sets, adversarial suites, synthetic cases, production-derived cases, and human labels. | Versioned repositories, secure databases, object stores, label records, source manifests. | Enforces partitioning, provenance, contamination checks, access control, and case lifecycle policy. |
| **Task Layer** | Evaluates output syntax, schemas, extracted values, classifications, transformations, and local correctness. | Parser outputs, structured responses, expected fields, rubric targets. | Uses deterministic checks, schema validators, value comparisons, and task-specific metrics. |
| **Component Layer** | Isolates retrieval, grounding, citation, parser, tool, routing, redaction, and safety components. | Retrieval IDs, source refs, tool traces, citation refs, parser versions, guardrail decisions. | Measures retrieval quality, evidence support, citation validity, tool correctness, and policy containment. |
| **Workflow Layer** | Evaluates multi-step plans, agent trajectories, retries, loop behavior, degraded modes, and terminal state. | Workflow traces, state hashes, action ledgers, retry logs, approval state, route decisions. | Checks trajectory validity, no-progress detection, state verification, idempotency, and safe termination. |
| **Judge Layer** | Scores open-ended outputs using rubric-governed human or model judges. | Judge outputs, rubric IDs, calibration splits, disagreement records. | Aggregates calibrated judgments; tracks bias, variance, reliability, and uncertainty. |
| **Human-Review Layer** | Uses expert review for subjective, ambiguous, high-impact, or policy-sensitive cases. | Review forms, adjudication records, rater profiles, sentinel cases, dwell-time signals. | Measures inter-rater reliability, detects reviewer drift, and resolves label disagreements. |
| **Governance Layer** | Tracks ownership, policy versions, approval requirements, audit artifacts, and release accountability. | Release manifests, policy IDs, approval records, artifact hashes, audit events. | Verifies ownership, approvals, retention, signatures where required, and accountability paths. |
| **Release-Gate Layer** | Converts evaluation results into deployment decisions. | CI/CD scorecards, baseline comparisons, canary results, incident regressions, cost/latency metrics. | Applies hard gates, soft gates, canary gates, differential gates, and rollback triggers. |

By decoupling evaluation logic across these nine layers, the platform prevents qualitative errors from being hidden inside an aggregate score.1 A small shift in an overall benchmark score can conceal a catastrophic failure in a high-impact task slice.5 This layered model guarantees that if a retrieval component degrades or a safety filter overblocks, the system isolates the failure to its specific layer, pointing engineers directly to the root cause.1

## **Ground Truth Typology**

Ground truth is not a static answer key; it is a versioned, multi-dimensional programmatic artifact that carries explicit provenance, operational boundaries, and failure modes.5 Evaluating a system using a single, flat string comparison assumes a deterministic simplicity that does not exist in production AI deployments.1 The evaluation architecture must classify ground truth across ten distinct typological categories: 

| Truth Type | Technical Scope & Structural Composition | Source & Production Provenance | Update Policy | Dominant Failure Modes | Evaluation Method |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Exact Truth** | Exact field values, regex-constrained identifiers, numeric ranges, enum values, and strict schemas. | Systems of record, schemas, ledgers, validated source databases. | Synchronized on schema or source-of-record change. | Schema drift, rounding errors, stale fixtures, field remapping. | Exact match, numeric tolerance checks, schema validation, deterministic parsers. |
| **Reference Truth** | Expert-drafted exemplar answers, policy summaries, templates, and acceptable phrasings. | SMEs, policy owners, historical reviewed outputs. | Reviewed on policy/product/domain change. | Stale wording, style drift, missing caveats, overfitting to examples. | Rubrics, semantic similarity, pairwise review, calibrated judges where appropriate. |
| **Evidence Truth** | Source IDs, spans, sections, page regions, database records, and visual coordinates supporting claims. | Knowledge base, document store, source registry, evidence manifests. | Refreshed on source ingestion, revision, permission change, or index rebuild. | Missing evidence, stale source, citation laundering, coordinate drift. | Citation verification, source-version checks, NLI/entailment, span/coordinate overlap. |
| **Procedural Truth** | Correct operation sequence, allowed API order, routing rules, approval steps, and dependency constraints. | Workflow specs, tool contracts, policy manifests, gateway configs. | Updated on workflow, tool, or policy changes. | Wrong order, skipped gate, duplicate call, unhandled degraded path. | Trace trajectory comparison, step dependency checks, route-policy validation. |
| **State Truth** | Verified backend mutations, balances, committed records, pending/unknown states, and ledger outcomes. | Transaction logs, action ledger, source-of-record database. | Verified at execution time and retained for audit. | False success, partial commit, orphaned tool call, sync delay, duplicate mutation. | Pre/post state hash, readback verification, idempotency, reconciliation status. |
| **Preference Truth** | Subjective style, helpfulness, tone, brand voice, and user experience preferences. | UX research, brand guidelines, preference labels, product standards. | Updated on product/brand or user-segment changes. | Verbosity bias, positivity bias, annotator drift, style overfitting. | Pairwise preferences, calibrated judge panels, human review, segment-specific rubrics. |
| **Safety Truth** | Prohibited actions, privacy boundaries, policy constraints, redaction requirements, refusal conditions. | Legal, compliance, security, safety policy owners. | Updated on regulation, threat, or policy change. | Overblocking, underblocking, jailbreak bypass, policy hallucination, leakage. | Deterministic checks, policy classifiers, adversarial tests, red-team cases, human review. |
| **Governance Truth** | Approval requirements, maker-checker rules, audit requirements, signatures, review authority, break-glass conditions. | Governance policy, audit records, approval ledgers, regulatory controls. | Updated on governance, risk, or regulatory changes. | Missing approval, self-approval, stale approval, unsigned audit event, accountability gap. | Ledger checks, signature/hash validation, approval-state verification, replay audits. |
| **Comparative Truth** | Relative preference that output A is better than B under a rubric. | Human preference labels, calibrated judge panels, evaluation studies. | Updated with new calibration and rater-quality checks. | Position bias, verbosity bias, self-preference, rubric ambiguity. | Pairwise win rates, calibrated judges, Platt scaling, human adjudication. |
| **Negative Truth** | Required refusals, managed limitations, blocked actions, fail-closed outcomes, and out-of-scope responses. | Safety policy, threat models, boundary-defense rules. | Updated with new abuse cases, policy changes, and incident findings. | Over-refusal, unsafe compliance, silent bypass, refusal without help. | Adversarial prompts, OOD probes, refusal-quality rubrics, containment checks. |

This typographical separation enforces the fundamental principle: ground truth is a version-controlled, scoped software asset with explicit failure modes, not an absolute, static answer key.5 Treating truth as a programmatic asset ensures that the testing system can adapt to changes in databases, documents, and corporate policies without requiring manual annotation of the entire evaluation catalog.4

## **Golden Set Lifecycle Model**

Golden sets represent curated, version-controlled, and representative test cases that encode the required behavioral properties of the AI system.1 The evaluations platform manages golden sets through a continuous, structured lifecycle to ensure they remain representative of real-world workloads while protecting against benchmark obsolescence.17

```text
GOLDEN SET LIFECYCLE

[ Production Signals ]
  incidents | user corrections | reviewer overrides | drift alerts | regressions
        |
        v
[ Candidate Case Ingress ]
  select trace or synthetic seed
  classify task/risk/failure mode
        |
        v
[ Case Authoring ]
  define input, expected behavior, evidence, constraints, and rubric
        |
        v
[ Label / Truth Creation ]
  exact truth | evidence truth | state truth | preference truth | negative truth
        |
        v
[ Review and Validation ]
  SME review
  privacy/redaction check
  deduplication
  contamination screening
        |
        v
[ Baseline Run ]
  evaluate current production/baseline route
  record expected variance and known limitations
        |
        v
[ Versioned Golden Set ]
  immutable case version
  source refs and policy refs
  ownership and change history
        |
        v
[ Gate Assignment ]
  exploratory suite
  nightly regression
  CI hard gate
  canary monitor
  incident replay suite
        |
        v
[ Retirement / Refresh ]
  update stale cases
  retire obsolete policies
  preserve historical baselines
```

### **1. Ingestion and Case Ingress**

Cases are continuously surfaced from production telemetry traces. The ingestion pipeline flags anomalies, user corrections, edit character diffs, and citation overrides.1 These traces are compiled into standardized test inputs, preserving the complete context of the transaction, including user prompts, retrieved document chunks, system prompts, and tool return payloads.1

### **2. Case Authoring and Schema Layout**

To ensure evaluations are precise, authored cases must conform to a strict, multi-dimensional JSON schema:

```json
{
  "$schema": "https://ai-engineering.canon/schemas/golden-case-v1.json",
  "case_id": "GC-2026-FIN-089",
  "case_version": "1.4.0",
  "task_class": "Financial_Ledger_Mutation",
  "risk_classification": "HIGH",
  "allowed_route_profiles": [
    "primary_reasoning_route",
    "approved_equivalent_route"
  ],
  "inputs": {
    "user_query_redacted": "Disburse $500.00 to the approved vendor for invoice INV-102.",
    "user_query_hash": "sha256:user_query_hash",
    "session_context": {
      "tenant_hash": "sha256:tenant_hash",
      "subject_role": "finance_auditor",
      "dialogue_history_ref": "secure_ref:dialogue_context_gc_089"
    }
  },
  "retrieval_constraints": {
    "allowed_tenant_scope_hash": "sha256:tenant_scope_hash",
    "required_document_ids": [
      "doc_invoice_102_hash"
    ],
    "forbidden_document_ids": [
      "doc_cross_tenant_leak_hash"
    ],
    "required_evidence_regions": [
      {
        "document_id": "doc_invoice_102_hash",
        "page_number": 1,
        "bounding_box": {
          "x": 0.18,
          "y": 0.42,
          "width": 0.31,
          "height": 0.07,
          "coordinate_system": "normalized_page"
        }
      }
    ]
  },
  "tool_constraints": {
    "expected_tool_call": "disburse_funds",
    "action_class": "high_risk_mutation",
    "required_arguments": {
      "base_amount": 500.0,
      "invoice_id": "INV-102"
    },
    "idempotency_key_required": true,
    "permission_gate": "maker_checker_approval_required",
    "compensation_supported": true,
    "compensation_reference": "secure_ref:ledger_compensation_policy"
  },
  "acceptance_criteria": {
    "exact_match_fields": {
      "approval_status": "PENDING_REVIEW"
    },
    "forbidden_outcomes": [
      "COMMITTED_WITHOUT_APPROVAL",
      "CLAIMED_COMPLETION_WITHOUT_VERIFICATION",
      "CROSS_TENANT_RETRIEVAL"
    ],
    "safety_policies_enforced": [
      "PII_redaction",
      "No_unauthorized_disbursement",
      "Maker_checker_required"
    ],
    "minimum_evidence_support": {
      "nli_grounding_threshold": 0.95,
      "citation_required": true
    }
  },
  "provenance": {
    "source_trace_id": "8f92a10c7d3a4e9f9a1b6c5d4e3f2010",
    "authored_by_role": "SRE",
    "reviewed_by_role": "finance_domain_expert",
    "policy_version": "finance_policy_v12",
    "last_modified": "2026-06-11T12:00:00Z"
  },
  "privacy": {
    "payload_storage": "secure_reference_only",
    "redaction_status": "reviewed",
    "retention_class": "evaluation_high_impact"
  }
}
```

### **3. Verification, Deduplication, and Contamination Screening**

Once cases are authored, they must pass validation before promotion into a golden set:

* **Expert Review:** Subject matter experts confirm that the expected behavior, evidence requirements, and rubrics reflect the real task rather than a convenient fantasy benchmark with a clipboard.
* **Privacy and Payload Review:** Sensitive prompts, documents, tool payloads, and user data are redacted or stored by secure reference. General evaluation repositories should not contain uncontrolled raw production traces.
* **Semantic Deduplication:** Candidate cases are compared against existing cases using task-aware similarity checks. Thresholds should be calibrated by task class; a universal cosine cutoff is not reliable across extraction, reasoning, policy, multimodal, and tool-use cases.
* **Contamination Screening:** Evaluation cases should carry metadata, access controls, repository separation, and optional canary strings to detect leakage into training, retrieval, or public corpora. Canary GUIDs are useful detection markers, not magical anti-training amulets. Leakage prevention also requires permissions, storage isolation, release discipline, and provider/data-handling controls.
* **Promotion Decision:** Cases are assigned to an appropriate suite: exploratory, nightly regression, CI hard gate, canary monitor, adversarial suite, or incident replay. Not every useful case belongs in a hard release gate.

### **4. Promotion, Baselines, and Retirement**

Approved cases are committed to the versioned Golden Set repository.4 Baseline runs are executed to record initial performance spreads across the allowed models.5 As product features adapt, the golden sets undergo scheduled bi-annual refreshes, where stale schema definitions or retired policy constraints are pruned to prevent benchmark decay.2

## **Task Evaluation Matrix**

Evaluating high-dimensional systems requires matching specific task types to distinct metrics, rubrics, and automated gates to prevent qualitative errors from reaching production 4:

| Task Type | Core Target Metric | Target Rubric Parameter | Deterministic Checks | Automated Judge Role | Regression Gate Threshold | Common Failure Mode |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Classification** | F1-Score, Cohen's Kappa 15 | Category label match | Enforce valid category strings | Evaluates edge-case sentiment contexts | >= 0.95 Kappa 15 | Label drift, class imbalance |
| **Extraction** | Precision, Character Error Rate 8 | Exact value matching | Regex patterns, null checks | Verifies entity boundary alignments | 100% schema validity 5 | Malformed payloads, truncation 5 |
| **Summarization** | Rouge-L, BertScore 20 | Information density | Length constraints, formatting | Rates factual completeness 21 | >= 0.85 BertScore 20 | Hallucination, style verbosity 19 |
| **Drafting** | Paraphrase Fidelity, Style | Tone alignment | Markdown structure, syntax | Evaluates readability and flow | >= 0.90 human correlation | Language drift, policy violation 5 |
| **Transformation** | Exact Match, Character Diff | Semantic equivalence | Structural layout schema check | Checks for information loss | 100% type preservation | Information loss, corrupt tags 8 |
| **Reasoning** | Logic Path F1, Code Execution | Logical coherence | Step-by-step math evaluation | Verifies intermediate thought states | >= 0.98 success rate | Logical loops, false plan jumps 5 |
| **Coding** | Unit Test Pass Rate, AST | Compilation success | Syntax checks, imports | Validates algorithmic complexity | 100% compilation pass | Syntax errors, remote injection 5 |
| **Data Entry** | Field Accuracy, Schema Match 5 | Value-range match | Pydantic validator models 5 | Identifies logical contradictions | 100% constraint validation | Extra keys, inverted decimals 5 |
| **Policy App** | Compliance Ratio 22 | Policy adherence | Scan forbidden word tokens | Audits compliance exceptions | 100% safe enforcement 5 | Policy hallucination, leakage 5 |
| **Planning** | Path Match, Step Count 5 | Target goal completion | Step dependency verification | Audits alternative trajectories | >= 0.95 trajectory match | Infinite planning loops, stall 5 |
| **Multimodal** | Visual Grounding IoU 8 | Bounding box match | Frame count validation 8 | Verifies spatial-semantic links | >= 0.85 Mean IoU 8 | Crop drift, misread scales 8 |
| **Voice** | Word Error Rate (WER) 8 | Pronunciation check | VAD silence thresholds 8 | Checks emotional tone suitability | < 5.0% English WER 8 | False endpointing, double talk 8 |
| **UI Control** | Step Success Rate 8 | Interaction accuracy | DOM element visibility checks | Audits page states post-action | >= 0.98 SSR 8 | Stale selectors, unmounted elements 8 |
| **Multi-Step** | Task Completion Rate 7 | Final state agreement | Idempotency token matches | Evaluates trajectory safety | >= 0.95 state completion | Broken state, unreleased effects 23 |

### **Decoupling Task Success from System Success**

The evaluation framework enforces a critical distinction between output-level task success and system-level task success.5 A text output can be highly fluent and semantically correct (task success) while failing system success because it omits a required legal caveat, violates tenant data-isolation, or uses a stale cache bypass.1 Similarly, an agent workflow can execute a sequence of tool calls and produce a valid JSON payload (task success) while failing system success because it executes a mutation over a duplicate transaction ID, lacks active permissions, or claims transaction completion before database verification.1 The task evaluation layer must validate output-level behaviors, while the workflow and release-gate layers assess system-level properties.

## **Retrieval Evaluation Model**

Retrieval evaluation decouples the search engine’s performance from the generation model's capabilities, ensuring that context quality is measured objectively before prompts are compiled.1 A model can generate a highly fluent, correct answer when provided with poor context, or conversely, a model can hallucinate despite receiving perfect context.5 Retrieval evaluations isolate these variables, assessing whether the search subsystem successfully located, filtered, and ranked the correct evidence.5

### **Core Retrieval Metrics**

The evaluation suite measures evidence-finding performance using four standardized metrics 1:

* **Mean Reciprocal Rank (MRR@k)**: Evaluates the position of the first correct retrieved document chunk:  
  MRR = (1 / |Q|) * sum_{i=1}^{|Q|} (1 / rank_i)  
  Where rank_i is the position of the first relevant document for query i.  
* **Normalized Discounted Cumulative Gain (nDCG@k)**: Measures ranking quality, penalizing relevant documents pushed down the search results list.11  
* **Context Precision**: Assesses how much of the retrieved context is relevant, calculated as the ratio of relevant chunks to the total retrieved candidate count.11  
* **Context Recall**: Measures whether all necessary information required to answer the query is present in the retrieved candidates.11

### **Modality-Specific Retrieval Profiles**

Retrieval evaluation must adapt its metrics and thresholds across distinct retrieval pathways 1:

* **Lexical Retrieval**: Evaluates exact term matches using BM25, measuring hit rate and term frequency coverage.  
* **Vector Retrieval**: Evaluates dense semantic embeddings, tracking retrieval recall and cosine similarity distances.  
* **Hybrid Retrieval**: Measures the fusion of lexical and vector results, evaluating Reciprocal Rank Fusion (RRF) weights.  
* **Graph Retrieval**: Assesses entity-relationship traversal, evaluating entity-relevance density and relation path lengths.21  
* **Multimodal Retrieval**: Evaluates late-interaction page-patch match patterns (e.g., ColPali), tracking visual coordinate overlap rates and patch similarity densities.8  
* **Tool-Mediated Retrieval**: Evaluates search APIs executed by agents, measuring tool selection accuracy and query rewrite quality.5

### **Multi-Tenant Permission and Safety Auditing**

In multi-tenant SaaS environments, retrieval must be treated as an access-control gate.5 The evaluation architecture executes security verification checks on every similarity search:

* **Row-Level Security (RLS) Compliance**: Validating that similarity searches are isolated directly at the database engine layer.5 The test suite confirms that queries execute within active tenant-scoped transactions, failing instantly if a query is dispatched without an active session variable.5  
* **Pre-Filter Authorization checking**: Enforcing role-based and attribute-based access controls on document chunks prior to vector distance calculations.5 This prevents cross-tenant data leakage and blocks timing side-channel exploits.5  
* **HubScan Robust Z-Score**: Monitoring the vector index to identify and isolate poisoned documents or "adversarial hubs".5 The test suite calculates local hubness metrics, raising a security alert if a vector’s robust z-score exceeds 5.0 5:  
  Z_robust = (k_i - median(k)) / MAD(k)  
  Where k_i is the number of times vector i appears in the top-K nearest neighbors of random queries, and MAD is the Median Absolute Deviation.5

## **Grounding and Citation Evaluation Model**

A system must not assume its answers are trustworthy because it retrieved documents. Grounding evaluations test whether generated claims are supported by provided evidence. Citation evaluations test whether inline citations, source links, coordinates, or record references point to the specific evidence supporting local assertions. These are related but distinct checks.

### **Grounding and Faithfulness Metrics**

Grounding evaluation should decompose generated answers into atomic, checkable claims and compare each claim against authorized evidence. This can use Natural Language Inference (NLI), entailment classifiers, structured record checks, exact field comparison, or human review depending on the domain.

```text
faithfulness = supported_claims / total_checkable_claims
```

The denominator should exclude non-factual prose, stylistic transitions, and clearly marked uncertainty. Unsupported high-impact claims should be blocked, revised, or explicitly marked as unverified.

### **Fine-Grained Citation Evaluation**

Fine-grained citation evaluation verifies whether a citation supports the local claim it is attached to. Named frameworks such as ALiiCE are useful references for positional and claim-level citation evaluation, but the canon should not depend on a single framework. The general pattern is:

```text
FINE-GRAINED CITATION CHECK

[ Generated Answer ]
        |
        v
[ Claim Segmentation ]
  split sentence/paragraph into atomic claims
        |
        v
[ Citation Mapping ]
  associate each claim with citation marker, source ID, span, record, or coordinate
        |
        v
[ Evidence Verification ]
  exact match | NLI entailment | structured record check | coordinate overlap
        |
        v
[ Citation Verdict ]
  supported | unsupported | wrong source | stale source | unauthorized | ambiguous
```

Evaluation should report both recall and precision:

```text
citation_recall = supported_required_claims / total_required_claims

citation_precision = supported_cited_claims / total_cited_claims
```

Citation placement readability can be evaluated separately from factual support. A perfectly readable citation can still be wrong. A visually cluttered citation can still be accurate. Because the universe insists on being inconvenient.

### **Spatial and Coordinate-Level Verification**

In document, chart, image, and UI workflows, text-only citation checks are insufficient. The evaluation platform should verify that cited claims map to the relevant source region or record.

* **Coordinate Validity:** Citation regions must point to valid, non-empty areas on the source page/frame/screenshot.
* **Source Version:** The cited region must belong to the source version used during generation.
* **Permission Scope:** The source must be authorized for the active tenant/user/session.
* **Intersection-over-Union:** Where ground-truth boxes exist, overlap may be measured as:

```text
IoU = area(B_generated ∩ B_gold) / area(B_generated ∪ B_gold)
```

IoU thresholds should be task-specific. A tiny table cell, a full chart, and a document paragraph should not share a universal threshold.

## **Transactional Tool-Use Evaluation Model**

When model actions cross from text generation into system operations, evaluations must prove that tool execution is authorized, bounded, idempotent where required, and verified against the system of record. Tool-use evaluation is not satisfied by “the model selected the right tool.” It must also test arguments, policy gates, side effects, retries, compensation, and user-facing completion claims.

```text
TRANSACTIONAL TOOL-USE EVALUATION

[ Proposed Tool Call ]
  tool name, arguments, subject, tenant, purpose, action class
        |
        v
[ Pre-Action Evaluation ]
  schema valid?
  authorization valid?
  risk tier allowed?
  approval required?
  idempotency required?
        |
        +--> fail: block and record violation
        |
        v
[ Execution Harness ]
  sandbox or mock for tests
  real integration only in controlled environments
  request/response captured by trace
        |
        v
[ Post-Action Verification ]
  expected state reached?
  no duplicate mutation?
  no unauthorized resource touched?
  unknown/pending state preserved?
        |
        +--> verified: pass
        |
        +--> failed/unknown: fail, reconcile, or escalate
```

### **Effect Classification**

| Effect Class | Examples | Evaluation Requirement |
| :--- | :--- | :--- |
| **Read-Only Effects** | Search, lookup, document parse, status check. | Verify correct source, permissions, timeout handling, and no mutation. |
| **Bufferable Effects** | Draft file writes, staged form values, local workspace changes. | Verify effects remain invisible until commit and can be discarded safely. |
| **Reversible External Effects** | Reservations, provisional holds, reversible workflow updates. | Require idempotency, compensation plan, and post-action verification. |
| **Irreversible / High-Impact Effects** | Payments, email sends, deletions, legal filings, production admin actions. | Require approval, pre-action verification, explicit user/reviewer confirmation, and post-action state readback. |

For irreversible effects, the external action may itself be the point of no return. The safe design is not “execute first and pretend rollback exists.” The safe design is to gate before execution, preserve approval state, verify after execution, and represent unknown state honestly.

## **Agent and Workflow Evaluation Model**

Evaluating multi-step agents requires analyzing full execution trajectories rather than simply scoring final text outputs.24 A system can generate a plausible final response while repeating tool loops, ignoring errors, or violating policies.5

### **Dynamic Execution-Based Benchmarking**

The evaluation platform integrates agent workflows with standard sandboxed test suites, including WebArena, ST-WebAgentBench, and Claw-Eval-Live, to measure real-world performance.22 The evaluation harness enforces three execution-level metrics 7:

* **Task Success Rate (SSR)**: The fraction of tasks that successfully reach a correct terminal state without unrecoverable errors.7  
* **State Repetition Count (C_rep)**: Identifies infinite tool loops by tracking consecutive identical planning turns.5 If C_rep > 2, the execution is terminated.5  
* **Transient Contamination**: The fraction of runs where non-committed speculative branch effects become visible to the user or other agents, violating system isolation.7

### **Completion under Policies (CuP) Metric**

For enterprise workflows, task success must be balanced against policy compliance.22 The evaluation platform implements the Completion under Policies (CuP) metric to ensure agents operate within organizational constraints 22:

CuP = Task Completion * product_{i=1}^{M} P_i  

Where Task Completion represents the portion of the task successfully executed, and P_i is in {0, 1} representing adherence to safety policy i. If any organizational policy is breached (e.g., unauthorized document access, bypass of approval gates), CuP falls to zero, and the release is blocked.[5, 22]

### **Ideal vs. Degraded Trajectory Testing**

Agents must be evaluated under both ideal and degraded conditions to confirm system resilience. The test suite injects artificial faults—such as database timeouts, rate limits, and OCR layout drift—to verify that the agent gracefully transitions to degraded modes (e.g., serving cached replies, narrowing retrieval bounds, or packaging escalation payloads for human review) without causing catastrophic session crashes.

## **Adversarial Evaluation Taxonomy**

High-dimensional systems face adversarial threats across prompts, documents, retrieval indexes, parsers, tools, caches, UI surfaces, and output sinks. Adversarial evaluation should test containment, not merely model refusal. A model may generate a bad string; the system passes only if downstream boundaries prevent unauthorized action, leakage, unsafe rendering, or false completion.

| Threat Category | Execution Vector | Primary Target Boundary | Expected Defensive Behavior | Automated Release Gate |
| :---- | :---- | :---- | :---- | :---- |
| **Direct Prompt Injection** | Malicious user text attempts to override system/developer instructions. | Instruction hierarchy and tool authority. | Treat user text as untrusted data; block unauthorized tool/action changes. | Block release if injection changes privileged behavior or exposes protected prompt content. |
| **Indirect Document Injection** | Hidden or visible instructions inside uploaded documents, webpages, emails, PDFs, OCR text, or retrieved chunks. | Parser, retrieval, context assembly, tool authorization. | Preserve source authority labels; prevent untrusted content from authorizing tools or memory writes. | Block if document text causes unauthorized tool calls, policy changes, or data exfiltration. |
| **Retrieval / Index Poisoning** | Malicious uploads, metadata manipulation, adversarial embeddings, hub documents. | Corpus admission, vector index, ranking, context selection. | Enforce provenance, permissions, anomaly detection, quarantine, and rollback. | Block or quarantine on confirmed poisoning; investigate hubness/anomaly signals. |
| **Citation Laundering** | Generated citations falsely make unsupported claims look sourced. | Grounding and user-facing evidence display. | Verify claim-to-source mapping before displaying citation confidence. | Block if unsupported claims receive valid-looking citations. |
| **Context / Prefill Flooding** | Large, redundant, or adversarial context designed to dilute instructions or exhaust resources. | Context compiler and runtime gateway. | Enforce context budgets, evidence prioritization, truncation, and managed failure. | Block if safety/authority constraints are lost under context pressure. |
| **Cost Bomb / Loop Abuse** | Prompts or states trigger repair loops, retries, recursive planning, or excessive reranking. | Orchestrator, gateway, retry policy. | Enforce loop budgets, retry caps, no-progress detection, and spend ceilings. | Block if unbounded consumption occurs or termination fails. |
| **Tool / API Misuse** | Inputs induce unauthorized queries, broad tool calls, host access, or mutation. | Tool contract, credential broker, action verification. | Require scoped credentials, schema validation, authorization, idempotency, and post-action checks. | Block if tool call exceeds contract, scope, or approval state. |
| **Output-Sink Injection** | Generated SQL, shell, HTML, Markdown, spreadsheet formulas, code, or config is rendered/executed unsafely. | Output sink and renderer/executor. | Use sink-specific escaping, parameterization, AST validation, sandboxing, or block. | Block if payload reaches unsafe sink without required transformation. |
| **Degraded-Mode Abuse** | Attacker forces outage/cache/fallback state to bypass freshness, safety, or capability floors. | Fallback routing and UX resilience. | Preserve policy, tenant scope, cache scope, and disclosure requirements under fallback. | Block if degraded mode silently weakens safety or evidence requirements. |
| **Evaluation Contamination Attack** | Eval cases leak into training, prompt examples, retrieval corpora, or judge calibration data. | Dataset governance and release evaluation. | Partition datasets, track provenance, use canaries, and scan for overlap. | Block release if contamination materially invalidates the evaluation. |

Adversarial testing must focus on system containment rather than model refusal rates alone.5 An injection attempt may bypass a model's alignment filters, but the attack is contained if the output sink parser sanitizes the payload, the tool gateway rejects unauthorized parameters, and the SIEM database captures the anomaly.5

## **Synthetic Test Governance Model**

Synthetic tests expand coverage, but they do not automatically create ground truth. They can amplify generator bias, unrealistic distributions, hidden contamination, and brittle rubric assumptions. Synthetic cases should therefore be treated as candidates until validated.

```text
SYNTHETIC TEST GOVERNANCE PIPELINE

[ Validated Seed Sources ]
  documents | schemas | incidents | traces | policy specs
        |
        v
[ Structure Extraction ]
  entities
  relations
  events
  constraints
  edge-case parameters
        |
        v
[ Candidate Generation ]
  query generation
  adversarial variants
  multi-hop cases
  negative/refusal cases
        |
        v
[ Metadata and Contamination Controls ]
  source refs
  generator version
  canary marker if used
  train/eval partition labels
        |
        v
[ Validation Gate ]
  deduplicate
  realism review
  rubric review
  privacy review
  answerability check
        |
        v
[ Suite Assignment ]
  exploratory only
  nightly regression
  adversarial suite
  hard gate after expert promotion
```

Synthetic generation should follow these rules:

1. **Seed from trusted structure:** Use validated documents, schemas, traces, incidents, or policy specifications rather than unconstrained “make me tests” prompting.
2. **Preserve provenance:** Every synthetic case should record seed source, generator version, prompt/template version, and transformation path.
3. **Separate exploration from release gates:** Synthetic cases can explore coverage immediately, but hard CI/CD gates require expert review or deterministic validation.
4. **Screen for contamination:** Use repository isolation, access controls, metadata labels, overlap scans, and optional canary strings. Canary GUIDs help detect leakage; they do not prevent it by themselves.
5. **Audit realism:** Human or deterministic checks should verify that cases are answerable, meaningful, non-duplicative, and representative of actual product risk.

## **LLM-as-Judge Reliability Model**

Using language models as evaluators provides scalability, but general-purpose LLMs exhibit position, verbosity, self-preference, and format-level biases that compromise evaluation reliability. The evaluation architecture implements the "Calibrate, Don't Curate" paradigm, utilizing a multi-judge panel with post-hoc statistical corrections rather than relying on a single, uncalibrated oracle model.

LLM judges should be used for rubric-governed, open-ended evaluation where deterministic checks are insufficient. They should not replace deterministic validators for schemas, permissions, arithmetic, source-of-record state, tenant isolation, irreversible actions, or security boundaries. The judge is a noisy measurement instrument, not a priesthood. Calibrate it, audit it, and keep it away from the big red deployment button unless hard gates already passed.

### **Heterogeneity-Aware Judge Aggregation**

Instead of discarding weaker judges by point accuracy alone, the system retains all parseable, non-redundant judges and aggregates their votes using an integrated Bayesian One-Coin Model.13 The joint probability of correct evaluation P(z_t = 1 | y_t) is computed as 13:

P(z_t = 1 | y_t) = sigma( sum_{k in J_t} log( E[a_k^(y_tk) * (1 - a_k)^(1 - y_tk)] / E[a_k^(1 - y_tk) * (1 - a_k)^(y_tk)] ) )

Where y_tk in {0, 1, -1} is the verdict of judge k on item t, J_t contains the active judges, and a_k is the judge's reliability parameter modeled as a beta distribution 13:

a_k ~ Beta(c_k + 1, n_k - c_k + 1)

Here, c_k is the number of correct verdicts scored by judge k on a labeled calibration split, and n_k is the total number of non-missing calibration verdicts scored by judge k.13 Judges with uncertain accuracy have c_k near (n_k - c_k) and therefore near-zero weight.13

### **Residual Bias Correction (Platt Scaling)**

To correct position and style biases left by the aggregator, the system runs a post-hoc logistic regression (Platt Scaling) over calibration labels. This maps the raw aggregate score to the empirical human preference frequency :

logit(p_hat_t^corr) = a * logit(p_hat_t) + b

When item features x_t (such as topic, prompt style, or judge family agreement levels) are available, the residual correction is expanded dynamically :

logit(p_hat_t^corr) = a * logit(p_hat_t) + x_t^T * gamma + b

Where (a, b, gamma) are fit via logistic regression on the calibration split, neutralizing systematic scale and shift distortions.13

### **Finite-Calibration Panel Selection (FCPS)**

To manage the tradeoff between panel size and calibration complexity under finite human-label budgets, the system deploys the FCPS (Finite-Calibration Panel Selection) validation selector. FCPS analyzes the calibration-label split, selecting the optimal combination of judge prefix, prefix size, and aggregator family (low-dimensional stacker vs. joint output table) that minimizes validation risk while controlling sparse-cell pressure in the joint table.  
Deterministic audits must dominate where exact limits or security boundaries apply, bypassing judge prompts entirely for structured transactions, permissions checking, and irreversible database writes.1

## **Human Review and Inter-Rater Reliability Model**

Human ratings remain the gold standard for subjective evaluations, but human annotations are noisy, inconsistent, and prone to drift.15 The evaluation platform structures its human-in-the-loop workflows to measure and enforce rater reliability before incorporating annotations into golden sets.1

### **Statistical Agreement Coefficients**

The platform tracks human annotator alignment using chance-corrected agreement metrics 15:

* **Cohen's Kappa (kappa)**: Measures agreement between two raters on categorical classifications.15  
* **Fleiss' Kappa**: Extends Cohen's kappa to multiple raters (m >= 3) assigning nominal categories to items 15:  
  kappa = (P_bar - P_bar_e) / (1 - P_bar_e)  
  Where P_bar is the observed mean agreement and P_bar_e is the expected agreement by chance.15  
* **Krippendorff's Alpha (alpha)**: The standard metric for content analysis.15 It supports ordinal, interval, or ratio data, accommodates missing entries, and uses distance metrics matching the scale type 15:  
  alpha = 1 - D_o / D_e  
  Where D_o is the observed disagreement and D_e is the expected disagreement by chance.15

### **Human Annotation Operations**

To ensure rater alignment, annotation workflows execute across three sequential stages:

* **Rubric Calibration**: Human raters are trained on structured rubrics with explicit, domain-specific examples for every score tier, establishing common baselines.1  
* **Adjudication Loops**: Discrepancies between raters (e.g., when agreement falls below alpha < 0.80) are routed to expert panels or senior editors for definitive resolution, updating the reference dataset.1  
* **Gold-Label Sentinels**: Pre-labeled, unambiguous reference cases are silently interleaved into active review queues to audit annotator performance.1 If a rater’s accuracy on sentinel cases falls below 95% or their dwell times indicate "rubber-stamping," their submissions are quarantined for review.1

Reviewers are matched to specific tasks based on complexity: standard evaluations use double-blind human reviews, while high-risk legal, medical, or financial workflows require senior subject-matter expert adjudication.1

## **Regression Gate and Readiness Framework**

Every update to models, prompts, indexes, tool schemas, or templates triggers the automated LLM Readiness Harness inside CI/CD pipelines to evaluate release readiness.3

```text
REGRESSION GATE AND READINESS HARNESS

[ Code / Config / Model / Prompt / Index Change ]
        |
        v
[ CI/CD Evaluation Runner ]
  load versioned golden sets
  load adversarial suites
  load telemetry replay cases
        |
        v
[ Metric Capture ]
  task success
  schema validity
  retrieval quality
  grounding/citation
  tool verification
  safety/policy
  latency/cost
        |
        v
[ Hard Gate Check ]
        |
        +--> hard violation
        |       BLOCK RELEASE
        |       create failure report
        |
        v
[ Soft / Differential Gate Check ]
        |
        +--> regression requiring owner signoff
        |       HOLD FOR REVIEW
        |
        v
[ Pareto / Profile Fit Check ]
        |
        +--> dominated or profile mismatch
        |       HOLD OR ROUTE TO CANARY
        |
        v
[ Canary Deployment ]
  limited traffic
  telemetry watch
  rollback triggers
        |
        +--> canary failure
        |       ROLLBACK / DISABLE ROUTE
        |
        v
[ Promote Release ]
  publish manifest
  record eval artifacts
  update baselines if approved
```

The readiness harness enforces a structured, multi-tier gating strategy:

* **Hard Gates**: Blocks deployment immediately if binary constraints—such as structural JSON compliance, data permission scopes, safety policies, or PII redaction—are violated.4  
* **Soft Gates**: Warns the release team and requires explicit owner sign-off if performance drops slightly below baseline thresholds on non-critical tasks.  
* **Canary Gates**: Deploys the update to a small subset of production traffic, monitoring error rates, latency spikes, and user corrections before completing rollout.1  
* **Differential Gates**: Compares the semantic behavior of the new version directly against the previous baseline, highlighting changes in tone, length, or routing choices.1  
* **Drift Gates**: Halts rollout if semantic embeddings diverge from reference centroids by more than the allowed Wasserstein distance threshold.

### **Pareto Cost-Utility Frontiers**

Because higher model capabilities often carry high token costs and latency overheads, the readiness harness plots all candidate configurations on a multi-dimensional Pareto frontier.3 It maps P95 latency and transaction cost against the composite utility score.3 Configurations that are dominated by more efficient alternatives are flagged or blocked.4 This allows release engineers to choose distinct, optimal paths matching specific deployment profiles:

| Deployment Profile | Prioritized Dimensions | Ingestion & Execution Rules | Example Gated Architecture |
| :---- | :---- | :---- | :---- |
| **Cost-First** | Minimal token spend, low latency, basic task completion. | Hard cap of $0.005 per run; fallback models allowed; semantic cache priority. | Standard model + aggressive prefix caching + local regex parsers. |
| **SLA-First** | Minimal Time-to-First-Token (TTFT), low P95 latency, high TPS. | Hard cap of 1500 ms P95 latency; streaming outputs required. | High-concurrency short-pool model routing + chunked prefill. |
| **Risk-First** | High groundedness, precise citations, absolute policy compliance. | Zero tolerance for ungrounded claims or citation failures; strict FSM decoding. | Deliberative model + multi-stage parser + Atomix transactional tool gate. |

The Composite Readiness Score R is defined across scenario-specific weights:

* **Cost-First**: R = 0.20 * Workflow + 0.20 * Policy + 0.15 * Faithfulness + 0.15 * Retrieval + 0.20 * Cost + 0.10 * SLA  
* **Risk-First**: R = 0.15 * Workflow + 0.25 * Policy + 0.20 * Faithfulness + 0.15 * Retrieval + 0.10 * Cost + 0.15 * SLA  
* **SLA-First**: R = 0.20 * Workflow + 0.15 * Policy + 0.15 * Faithfulness + 0.15 * Retrieval + 0.10 * Cost + 0.25 * SLA

Missing metrics must not silently improve readiness scores. If a required hard-gate metric is missing, the run fails or enters manual review. If an optional metric is missing, the score may be normalized over present optional metrics only when the profile explicitly permits it.

```text
R_optional = sum(w_i * m_i for i in optional_present) / sum(w_i for i in optional_present)
```

The final readiness decision is therefore:

```text
release_allowed =
  all(required_hard_gates_pass)
  AND no_required_metric_missing
  AND profile_score >= threshold
  AND required_owner_signoffs_present_if_soft_gate_triggered
```

This prevents a candidate from looking “better” merely because inconvenient metrics vanished into the floorboards.

## **Evaluation Failure Triage Model**

When an evaluation run fails or performance metrics regress below CI/CD gate thresholds, the triage engine analyzes trace metadata to isolate the root cause 1:

| Failed Metric | Diagnostic Trace Signature | Probable Root Cause | Component Owner | Retest Suite | Recommended Remediations |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Low Groundedness** | Low entailment/support scores; unsupported claim rate rises; evidence density falls. | Noisy retrieval, stale source, context dilution, weak claim decomposition. | Retrieval / Prompt / Grounding Owners. | Grounding Verification Suite. | Improve evidence selection, rerank, reduce distractors, refresh source, tighten claim verification. |
| **Citation Failure** | Citation source missing, stale, unauthorized, wrong span, or empty coordinates. | Decorative citations, parser/layout drift, source-version mismatch, citation compiler bug. | Document / Citation / Retrieval Owners. | Citation Alignment Suite. | Rebuild citation mapping, verify source versions, repair coordinate extraction, suppress unsupported citations. |
| **Schema Violation** | Validation error, missing required key, extra key, wrong enum/type. | Template drift, model route mismatch, schema version mismatch, decoder looseness. | Platform / Tool Contract Owners. | Schema Integrity Suite. | Use constrained decoding where appropriate, align schema versions, fix prompts/contracts, block unsafe outputs. |
| **Tool Argument Failure** | Payload hash repeats; validation fails; unauthorized field/resource requested. | Tool ambiguity, weak schema, prompt injection, stale tool contract. | Tool Platform / Security Owners. | Tool Contract Suite. | Disambiguate tool names, tighten schemas, enforce authorization, add negative cases. |
| **Infinite Tool / Repair Loop** | Repeated state hash, repeated payload hash, rising retry/repair tokens. | No-progress detection missing, error messages unhelpful, overlapping tools, brittle repair prompt. | Agent Orchestration Owners. | Agentic Trajectory Suite. | Add loop budget, improve typed errors, halt/replan, separate tool affordances. |
| **Cross-Tenant Leak** | Tenant/session scope mismatch in retrieved chunks, cache hit, tool result, or UI state. | Filter bypass, RLS failure, cache scope bug, permission propagation failure. | Security / Data Platform Owners. | Multi-Tenant Isolation Suite. | Fail closed, fix DB-enforced policy, invalidate scoped cache, add boundary regression. |
| **Latency Spike** | Queue wait, prefill, decode, parser, retrieval, or tool latency exceeds profile. | Cache starvation, long-context route, parser overload, queue contention, provider degradation. | SRE / Serving / Retrieval Owners. | Concurrency and Latency Suite. | Partition routes, cap context, improve cache keys, tune queues, apply admission control. |
| **Cost Regression** | Token burn, retry cost, rerank cost, parser cost, or human-review cost rises. | Context bloat, query fan-out, repair loops, expensive route selection, review flood. | FinOps / Gateway / Orchestration Owners. | Cost Regression Suite. | Add budgets, reduce fan-out, terminate no-progress loops, update route policy. |
| **Policy Regression** | Safety/policy pass rate drops, refusal behavior shifts, unsafe allow appears. | Policy version mismatch, prompt drift, model update, weak output sink controls. | Safety / Governance Owners. | Policy and Adversarial Suite. | Freeze rollout, restore policy version, add adversarial case, require review. |
| **Reviewer Reliability Drop** | IRR falls, sentinel failures rise, dwell time collapses, overrides spike. | Rubric ambiguity, reviewer fatigue, queue overload, automation bias. | Human Review / Governance Owners. | Reviewer Reliability Suite. | Recalibrate rubric, add cognitive forcing, rebalance queues, adjudicate labels. |

This structural isolation prevents failure modes from being masked by aggregate scores.5 If a specific slice exhibits a drop in performance, the triage engine maps the error to its localized component, ensuring rapid root-cause identification and remediation.5

## **Production-to-Eval Feedback Loop**

A high-performance evaluation architecture acts as the product’s behavioral immune system. Production incidents, user corrections, citation failures, tool timeouts, degraded-mode events, review overrides, and adversarial findings should become future evaluation coverage when they reveal a meaningful gap.

```text
PRODUCTION-TO-EVAL FEEDBACK LOOP

[ Production Signal ]
  incident | correction | override | drift | safety event | failed action
        |
        v
[ Trace and Evidence Capture ]
  trace IDs
  source refs
  payload hashes
  route decisions
  tool/action state
        |
        v
[ Privacy and Scope Gate ]
  redact
  classify sensitivity
  store raw payloads by secure reference only if needed
        |
        v
[ Case Packaging ]
  define input
  define truth type
  define expected behavior
  define failure mode
  define acceptance criteria
        |
        v
[ Expert / Owner Review ]
  validate rubric
  verify evidence
  assign suite and risk class
        |
        v
[ Golden Set Promotion ]
  exploratory
  regression
  adversarial
  hard gate
  canary monitor
        |
        v
[ CI/CD and Monitoring Gate ]
  prevent recurrence
  track drift
  update baselines only with approval
```

The feedback loop is executed through a disciplined engineering pipeline:

1. **Signal Discovery:** High-severity incidents, user corrections, citation mismatches, tool timeouts, degraded-mode failures, and human approval overrides are captured by strategic telemetry.
2. **Privacy and Redaction:** Raw prompts, documents, tool payloads, and outputs are redacted or stored by secure reference. Evaluation cases should preserve replayability without turning the eval repository into a museum of secrets.
3. **Ground Truth Authoring:** The sanitized trace is converted into a golden-case candidate. Owners assign truth typology, rubric, expected state, evidence requirements, and failure mode.
4. **Promotion Decision:** The case is assigned to the correct suite. Some cases become hard gates; others become exploratory, nightly, adversarial, or canary tests.
5. **Documented Exclusion:** A production failure should either become an evaluation case or receive a documented exclusion reason, such as privacy limits, one-off external outage, non-reproducibility, or lack of product relevance.

## **Cross-Canon Handoff Map**

The evaluations architecture interfaces with the entire AI Systems Engineering Canon because every architectural boundary eventually needs tests, regression controls, and release gates.

| Target Canon Report | Domain Area | Technical Handoff Parameters | Operational Integration Rules | Fallback & Degraded Protocols |
| :---- | :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context and State Governance | Context object IDs, state snapshots, memory inclusion cases. | Evaluate whether context compaction preserves active constraints and state. | Block release if state-critical context is lost. |
| **AI-ENG-D** | Corpus Engineering | Source provenance, corpus version, permission metadata. | Golden cases must track corpus source lineage and authority. | Quarantine unknown-provenance evaluation evidence. |
| **AI-ENG-E** | Retrieval Pipeline | Query sets, relevance labels, source IDs, citation packets. | Retrieval evals isolate recall, precision, ranking, permission, and freshness. | Use no-evidence response if retrieval cannot be trusted. |
| **AI-ENG-F** | Freshness and Conflict Detection | Source age, conflict labels, stale-cache indicators. | Evals must include stale/conflicting-source cases. | Block high-impact answers when freshness/conflict gates fail. |
| **AI-ENG-G** | Model Selection | Model decision records, route profiles, task fit. | Model choice must be evaluated by task/risk profile, not generic benchmark score. | Route to approved fallback only if quality floor holds. |
| **AI-ENG-H** | Model Adaptation | Adapter versions, fine-tune datasets, safety deltas. | Adapted models require regression against baseline and safety suites. | Roll back adapter or restrict route on regression. |
| **AI-ENG-I** | Regression Control | Negative flips, baselines, significance tests. | Eval architecture supplies datasets and metrics for regression gates. | Hold release pending owner signoff or rollback. |
| **AI-ENG-J** | Throughput Mechanics | Latency, batch, KV-cache, token throughput metrics. | Performance evals must include resource and workload profiles. | Degrade or block route if performance SLO gates fail. |
| **AI-ENG-K** | Weight Dynamics | Quantization, pruning, decoding, compression variants. | Optimization changes require quality, safety, and drift regressions. | Restrict optimized route to safe task profiles. |
| **AI-ENG-L** | Serving Architecture | Route, provider, fallback, cache, telemetry. | Serving changes require routing and fallback evals. | Canary, rollback, or fail closed on unsafe route behavior. |
| **AI-ENG-M** | Agentic Orchestration | Workflow traces, loop states, terminal outcomes. | Agents need trajectory, no-progress, and degraded-mode evaluations. | Halt, replan, or escalate on loop failures. |
| **AI-ENG-N** | Tool Contracts | Tool schemas, payload hashes, idempotency, error taxonomy. | Tool-use evals verify schema, authorization, and safe retries. | Block tool route if contract tests fail. |
| **AI-ENG-O** | Action Verification | Pre/post state hashes, unknown/pending status, compensation. | Completion claims require verified action state in evals. | Hold, reconcile, compensate, or escalate unknown state. |
| **AI-ENG-P** | Multimodal Understanding | OCR/layout labels, coordinates, visual evidence. | Multimodal evals verify extraction, grounding, and citation coordinates. | Use manual review or text-only mode when visual confidence fails. |
| **AI-ENG-Q** | Speech and Realtime Interaction | WER, endpointing, turn-taking, confirmation cases. | Voice evals include confirmation reliability for high-impact actions. | Switch to text/card confirmation on voice degradation. |
| **AI-ENG-R** | UI Agents | DOM/action traces, screenshots, post-action state. | UI-agent evals verify observation, action, and state reconciliation. | Pause automation on drift or unsafe uncertainty. |
| **AI-ENG-S** | Production Pathologies | Malformed output, false success, loops, brittle chains. | Pathology cases become regression tests. | Block release on recurring false-success or loop defects. |
| **AI-ENG-T** | Boundary Defense | Injection cases, tenant scope, untrusted content labels. | Security evals test authority boundaries and data/tool isolation. | Fail closed on boundary-control regression. |
| **AI-ENG-U** | Supply Chain Security | Artifact hashes, parser versions, tool manifests. | Eval runs must pin artifact versions and detect unsafe dependencies. | Quarantine unapproved artifacts. |
| **AI-ENG-V** | Resource Abuse | Token budgets, cost bombs, retries, retrieval fan-out. | Evals include cost, loop, and resource-abuse stress cases. | Throttle, circuit-break, or block release on runaway behavior. |
| **AI-ENG-W** | UX Resilience | Fallback states, partial answers, disclosure events. | Degraded-mode behavior must be tested as product behavior. | Use approved degraded route or fail-closed with saved state. |
| **AI-ENG-X** | Trust and Transparency | Citation UX, disclosure, contestability, user corrections. | Trust signals feed eval cases but are not truth labels alone. | Escalate to review or improve evidence display. |
| **AI-ENG-Y** | High-Impact Workflow Design | Approval states, review outcomes, CFFs, break-glass cases. | High-impact actions require approval and review-path evals. | Hold execution when review/approval gates fail. |
| **AI-ENG-Z** | Strategic Telemetry | Golden traces, spans, payload hashes, drift signals. | Telemetry provides replayable evidence for eval case creation. | Mark eval integrity degraded if traces are incomplete. |
| **AI-ENG-AB** | Audit and Replay | Eval artifacts, traces, case versions, run manifests. | Failed evals must be replayable from stored artifacts. | Preserve redacted, hash-bound trace evidence. |
| **AI-ENG-AC** | Incident Response | Failed-release cases, incident regressions, containment tests. | Incidents feed adversarial and regression suites. | Trigger playbooks on production recurrence. |
| **AI-ENG-AD** | Governance and Accountability | Metric ownership, gate policy, exception approvals. | Governance defines hard gates, soft gates, and waiver authority. | Route gate exceptions to accountable owners. |
| **AI-ENG-AJ** | Reference Architecture | Eval harness, golden-set registry, telemetry replay, release gate. | Reference systems should include evaluations as first-class infrastructure. | Disable deployment path when eval substrate is unavailable. |


## **Strategic Conclusions and Architectural Recommendations**

To build a reliable evaluation and regression framework in high-dimensional AI systems, organizations should adopt the following five core principles of system-level validation:

### **I. Treat Evaluations as Production Software Infrastructure**

Evaluation is not an offline, static calculation; it is the runtime and CI/CD validation engine of the platform. Prompt edits, index modifications, and model rollouts must trigger automated test suites.5 This ensures that system constraints, security filters, and business logics are evaluated programmatically before changes are deployed.4

### **II. Separate Content Grounding from Citation Precision**

Evaluating whether a model's generation is grounded in context does not verify that its inline citation markers are accurate.5 Grounding and citation evaluations must run separately.5 Use NLI entailments to confirm answer consistency, and deploy the ALiiCE framework to verify that individual claims map back to exact visual coordinates, page regions, or character spans in the source document.9

### **III. Enforce Progress-Aware Tool Transactions Outside the Model**

Relying on prompting or model-level self-policing to verify tool safety introduces severe risks of state corruption.5 Tool use must run within external, progress-aware transactions.23 Utilize the Atomix transactional model to group tool effects, buffer local modifications, track progress frontiers, and execute Saga-style compensations if steps fail, ensuring spoken confirmations never outrun database commits.23

### **IV. Calibrate LLM Judges with Bayesian One-Coin Panels**

Single, uncalibrated language models are biased evaluators.12 To build reliable judge pipelines, deploy diverse multi-judge panels, aggregate votes using Bayesian One-Coin models, and apply Platt-style logistic scaling over labeled calibration sets to correct systematic position and verbosity biases.13 Use the FCPS selector to match panel size to available human annotation budgets.35

### **V. Enforce Release Decisions via Pareto Cost-Utility Frontiers**

Textual quality must not be evaluated in isolation.3 The readiness harness must aggregate workflow success, compliance metrics, and grounding scores, and plot configurations against direct token costs and P95 latencies.3 By identifying and deploying non-dominated configurations on the Pareto frontier, SRE teams can optimize systems matching specific cost-first, SLA-first, or risk-first profiles without compromising system safety.3

## **Durable Principles of Evals Architecture**

1. **Evaluations Are Production Infrastructure**  
   Evals are not leaderboard decorations. They are release controls, regression detectors, incident memory, and behavioral specifications.

2. **No Single Score Proves System Safety**  
   Task success, retrieval, grounding, citations, tools, safety, cost, latency, UX, and governance must be evaluated separately before being composed.

3. **Ground Truth Is Typed**  
   Exact values, evidence, state, preference, safety, and governance truth require different labels, metrics, and update policies.

4. **Golden Sets Must Evolve Without Rotting**  
   Golden cases need versioning, ownership, provenance, refresh, retirement, and contamination controls.

5. **Production Failures Should Become Future Tests**  
   Incidents, corrections, overrides, and escaped regressions should feed the evaluation suite unless there is a documented reason to exclude them.

6. **Synthetic Tests Amplify Coverage, Not Authority**  
   Synthetic cases are candidates until validated. Hard gates require expert review, deterministic checks, or strong evidence.

7. **Judges Need Calibration**  
   LLM-as-judge systems are noisy measurement instruments. They need rubrics, calibration labels, bias checks, and uncertainty handling.

8. **Human Labels Need Reliability Controls**  
   Human review requires rubrics, rater training, inter-rater reliability, sentinel cases, and adjudication.

9. **Tool Evals Must Verify State**  
   Tool selection and argument correctness are not enough. High-impact tool use must verify authorization, idempotency, execution state, and post-action truth.

10. **Adversarial Evals Test Containment**  
   A model may fail locally while the system still passes if tool gateways, output sinks, permissions, and boundaries contain the attack.

11. **Missing Metrics Are Not Free Passes**  
   Missing required metrics should fail or hold a release, not make a readiness score look cleaner by subtraction sorcery.

12. **Eval Artifacts Must Be Replayable and Auditable**  
   Cases, rubrics, traces, labels, source refs, policy versions, and run manifests must support reproducible investigation.

#### **Works cited**

1. AI-ENG-Z — Strategic Telemetry - Traces, Tokens, Latency & Semantic Drift  
2. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
3. LLM Readiness Harness: Evaluation, Observability, and CI Gates for LLM/RAG Applications, accessed June 11, 2026, [https://arxiv.org/html/2603.27355v1](https://arxiv.org/html/2603.27355v1)  
4. LLM Readiness Harness: Evaluation, Observability, and CI Gates for LLM/RAG Applications, accessed June 11, 2026, [https://arxiv.org/html/2603.27355v2](https://arxiv.org/html/2603.27355v2)  
5. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
6. MCP-AgentBench: Evaluating Real-World Language Agent Performance with MCP-Mediated Tools, accessed June 11, 2026, [https://ojs.aaai.org/index.php/AAAI/article/view/40347/44308](https://ojs.aaai.org/index.php/AAAI/article/view/40347/44308)  
7. Atomix: Timely, Transactional Tool Use for Reliable Agentic Workflows - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2602.14849v1](https://arxiv.org/html/2602.14849v1)  
8. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
9. Explicit Evidence Grounding via Structured Inline Citation Generation - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2606.07130v1](https://arxiv.org/html/2606.07130v1)  
10. SoK: The Attack Surface of Agentic AI — Tools, and Autonomy - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2603.22928v1](https://arxiv.org/html/2603.22928v1)  
11. RAGAS: A Comprehensive Framework for RAG Evaluation and Synthetic Data Generation, accessed June 11, 2026, [https://gist.github.com/donbr/1a1281f647419aaacb8673223b69569c](https://gist.github.com/donbr/1a1281f647419aaacb8673223b69569c)  
12. A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2604.23178v1](https://arxiv.org/html/2604.23178v1)  
13. Calibrate, Don't Curate: Label-Efficient Estimation from Noisy LLM Judges - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2605.09702v1](https://arxiv.org/html/2605.09702v1)  
14. LLM-as-a-Judge: Rapid Evaluation of Legal Document Recommendation for Retrieval-Augmented Generation - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2509.12382v1](https://arxiv.org/html/2509.12382v1)  
15. Reliable Decision Support with LLMs: A Framework for Evaluating Consistency in Binary Text Classification Applications - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2505.14918](https://arxiv.org/pdf/2505.14918)  
16. Soft Contamination Means Benchmarks Test Shallow Generalization - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2602.12413v1](https://arxiv.org/html/2602.12413v1)  
17. Search-Time Data Contamination - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2508.13180v1](https://arxiv.org/html/2508.13180v1)  
18. Canary GUID - Grokipedia, accessed June 11, 2026, [https://grokipedia.com/page/Canary_GUID](https://grokipedia.com/page/Canary_GUID)  
19. Weak judges, strong panel: an ensemble approach to LLM eval - Orq.ai, accessed June 11, 2026, [https://orq.ai/blog/llm-juries-in-practice](https://orq.ai/blog/llm-juries-in-practice)  
20. (PDF) SummEval: Re-evaluating Summarization Evaluation - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/351162964_SummEval_Re-evaluating_Summarization_Evaluation](https://www.researchgate.net/publication/351162964_SummEval_Re-evaluating_Summarization_Evaluation)  
21. Knowledge-Graph Based RAG System Evaluation Framework - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2510.02549v1](https://arxiv.org/html/2510.02549v1)  
22. ST-WebAgentBench: A Benchmark for Evaluating Safety and Trustworthiness in Web Agents, accessed June 11, 2026, [https://arxiv.org/html/2410.06703v3](https://arxiv.org/html/2410.06703v3)  
23. Atomix: Timely, Transactional Tool Use for Reliable Agentic Workflows - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2602.14849v2](https://arxiv.org/html/2602.14849v2)  
24. Claw-Eval-Live: A Live Agent Benchmark for Evolving Real-World Workflows - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2604.28139v1](https://arxiv.org/html/2604.28139v1)  
25. ALiiCE: Evaluating Positional Fine-grained Citation Generation - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2406.13375v1](https://arxiv.org/html/2406.13375v1)  
26. ALiiCE: Evaluating Positional Fine-grained Citation Generation - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2406.13375](https://arxiv.org/pdf/2406.13375)  
27. ALiiCE: Evaluating Positional Fine-grained Citation Generation - ACL Anthology, accessed June 11, 2026, [https://aclanthology.org/2025.naacl-long.23.pdf](https://aclanthology.org/2025.naacl-long.23.pdf)  
28. ylXuu/ALiiCE: NAACL 2025 Main Conference - GitHub, accessed June 11, 2026, [https://github.com/ylXuu/ALiiCE](https://github.com/ylXuu/ALiiCE)  
29. ATOMIX: TIMELY, TRANSACTIONAL TOOL USE FOR RELIABLE AGENTIC WORKFLOWS - OpenReview, accessed June 11, 2026, [https://openreview.net/attachment?id=UeRbEpSVUz&name=pdf](https://openreview.net/attachment?id=UeRbEpSVUz&name=pdf)  
30. From Prompt–Response to Goal-Directed Systems: The Evolution of Agentic AI Software Architecture - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2602.10479v1](https://arxiv.org/html/2602.10479v1)  
31. An Executable Benchmarking Suite for Tool-Using Agents - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2605.11030v1](https://arxiv.org/html/2605.11030v1)  
32. SoK: The Attack Surface of Agentic AI -- Tools, and Autonomy - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2603.22928](https://arxiv.org/pdf/2603.22928)  
33. Authority, Truth, and Citation Bias: A Large-Scale Multi-Domain Benchmark for Studying Epistemic Susceptibility in Large Language Models - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2606.13104v1](https://arxiv.org/html/2606.13104v1)  
34. Calibrate, Don't Curate: Label-Efficient Estimation from Noisy LLM Judges - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2605.09702](https://arxiv.org/pdf/2605.09702)  
35. A Finite-Calibration Regime Map for LLM Judge Panels - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2606.01034v1](https://arxiv.org/html/2606.01034v1)  
36. Rating Roulette: Self-Inconsistency in LLM-As-A-Judge Frameworks - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2510.27106](https://arxiv.org/html/2510.27106)  
37. Full article: Reliable decision support with LLMs: a framework for evaluating consistency in binary text classification applications - Taylor & Francis, accessed June 11, 2026, [https://www.tandfonline.com/doi/full/10.1080/2573234X.2026.2652281](https://www.tandfonline.com/doi/full/10.1080/2573234X.2026.2652281)

---

# AI-ENG-AB — Verification Artifacts - Auditability, Reproducibility & Evidence Trails

## **Doctrinal Foundations: The Evidence Architecture Paradigm**

In high-dimensional artificial intelligence systems, production reliability cannot be governed or monitored by traditional application performance monitoring (APM) paradigms.1 While legacy web architectures treat system health as a binary or scalar infrastructure state defined by hardware allocation, network latency, and HTTP status codes, modern multi-agent execution loops and probabilistic large language models (LLMs) break these assumptions.1 A system can return an infrastructure-level successful HTTP 200 payload that contains a complete structural schema failure, an unauthorized tool execution, a cross-tenant data leak, or an ungrounded factual confabulation.1 Conversely, optimal hardware utilization and high throughput can coexist with red behavioral and meaning-level system states.2 This discrepancy establishes the Green Dashboard Fallacy: the diagnostic error of assuming an AI system is operating correctly simply because its underlying servers and APIs are online and available.1

To resolve this visibility deficit, modern systems architectures isolate monitoring concerns across distinct operational layers.1 While strategic telemetry (AI-ENG-Z) functions as the sensor network capturing execution spans 1, and evaluation frameworks (AI-ENG-AA) act as the regression testing pipeline 1, this document defines the architecture of verification artifacts (AI-ENG-AB).1 Verification artifacts are the durable, privacy-governed, and tamper-evident evidence objects required to reconstruct, audit, and reproduce the behavior of probabilistic systems after the fact.3

This architecture is governed by the core doctrine: every consequential AI output or action must leave behind a replayable evidence trail.1 This trail must link the exact prompt template, model weights snapshot, retrieval context, tool payload, policy execution state, human modification, and final output in an unbroken, cryptographically signed chain of custody.1 The fundamental engineering question shifts from "did we record a log entry?" to "can the organization reconstruct the exact decision path from user input to final output or action, including the evidence, versions, state transitions, checks, edits, and approvals that shaped it?".1 Without this capability, compliance becomes guesswork, debugging degrades into digital archaeology, and the system cannot be safely governed.1

## **Logs-Traces-Artifacts-Evidence Taxonomy**

To establish a clear hierarchy, the following taxonomy contrasts these diagnostic layers across operational, technical, and evidentiary boundaries, proving why generic logs are insufficient for AI auditability:

| Layer | Technical Scope & Composition | Primary Operational Purpose | System Proof Capability | Architectural Limitations |
| :---- | :---- | :---- | :---- | :---- |
| **Event Log** | Flat structured or unstructured events emitted by applications, services, or workers. | Debugging, operational status, exception capture, and coarse audit hints. | Shows that a code path emitted an event at a time. | Does not prove full context, prompt state, evidence, policy checks, user edits, or side effects. |
| **Metric** | Aggregated numeric counters, gauges, histograms, and time-series measurements. | Capacity planning, alerting, scaling, and SLO monitoring. | Shows rates, volumes, durations, and resource pressure. | Blind to semantic correctness, grounding, authorization, and user-visible truth. |
| **Execution Trace** | Parent-child span graph linking distributed work across model calls, retrieval, tools, queues, and services. | Latency analysis, causal path reconstruction, and system debugging. | Shows the causal execution path and timing relationships. | Often redacted, sampled, or incomplete for bulky or sensitive payloads. |
| **Span** | Atomic observable unit of work such as inference, retrieval, rerank, parser, tool call, guardrail, or review step. | Local timing, status, error, and attribute capture. | Shows what a specific execution step attempted and how it ended. | Does not by itself prove global policy state, source lineage, or final user-facing transformation. |
| **Verification Artifact** | Schema-validated evidence packet containing hashes, references, versions, decisions, state checks, and selected payload evidence. | Auditing a specific decision boundary or execution step. | Proves integrity of recorded inputs, outputs, parameters, and decisions to the degree captured. | Cannot prove uncaptured state; raw payload capture must be governed by privacy policy. |
| **Manifest** | Declarative version record for prompts, models, tools, dependencies, policies, routes, and environments. | Pinning expected system state and baseline capability configuration. | Shows what configuration was intended or eligible for execution. | Does not prove which dynamic inputs arrived or whether runtime behavior matched intent. |
| **Ledger** | Append-only record of actions, approvals, state transitions, tool calls, retries, and commit/verification status. | Sequencing, accountability, and action-state audit. | Shows logical progression and final known state of operations. | Does not automatically preserve full prompts, retrieved evidence, or rendered UI output. |
| **Snapshot** | Point-in-time capture or reference to context, retrieval state, memory, index version, tool state, or UI state. | Preserving what information was available at execution time. | Shows the evidence/context surface available to the system. | Storage-intensive; may require deduplication, redaction, and secure references. |
| **Diff** | Structural, textual, semantic, or field-level comparison between artifact versions. | Tracking edits, redactions, transformations, overrides, and regressions. | Shows exactly what changed between two captured states. | Does not explain why the change occurred unless linked to trace, policy, or reviewer artifacts. |
| **Evidence Trail** | Hash-linked sequence of artifacts, manifests, ledgers, snapshots, and diffs with retention and access policy. | Forensic reconstruction, compliance proof, replay, and dispute resolution. | Shows an integrity-protected chain of recorded evidence across a consequential workflow. | Tamper-evident, not omniscient; evidence quality depends on capture coverage and governance. |

Legacy logs are fundamentally insufficient for auditing probabilistic architectures.1 Recording that an API returned a response does not show the prompt template that generated it, the vector database coordinates that informed it, the safety policies that validated it, or the user edits that overrode it before execution.1 If an administrator cannot inspect these intermediate states, they cannot defend automated decisions to regulators, resolve disputes, or debug system drift.1

## **Replay and Reproducibility Typology**

### **GPU Non-Determinism and the Concurrency Hypothesis**

Achieving exact, bitwise reproducibility in large language model inference is exceptionally difficult.1 It is a common misconception that setting a fixed random number seed guarantees identical outputs across successive runs.18 In parallel computing environments, GPUs execute matrix multiplications and attention calculations across thousands of concurrent cores.19  
Because floating-point addition is non-associative, the order of arithmetic reduction operations alters the least significant bits of the floating-point representations 18:  
(a + b) + c is not equal to a + (b + c)  
If the execution speed of concurrent GPU threads varies due to minor hardware latency fluctuations, the reduction order changes, introducing run-to-run non-determinism.19  
However, technical analysis reveals that this "concurrency and floating-point" hypothesis is not the primary driver of non-determinism in production inference endpoints.18 Individual operations (kernels) inside the model's forward pass are run-to-run deterministic when executed in isolation.20 The primary cause of output variance is dynamic batching.18 High-performance serving engines (such as vLLM) batch concurrent requests dynamically to maximize hardware utilization.20 Because system load changes constantly, an individual request is batched with varying sets of other user prompts.20  
To optimize performance for different batch sizes, the engine dynamically selects different parallelization strategies, such as switching from a standard data-parallel matrix multiplication to a Split-K matmul, or modifying reduction trees in Split-Reduction RMSNorm.19 This batch-dependent kernel selection alters the reduction arithmetic, producing different outputs for the identical prompt.20

To enforce true bit-exact determinism, the model must be executed on an inference stack utilizing batch-invariant kernels.19 This strategy parallelizes operations entirely within individual tensor cores (data-parallel reductions) and avoids Split-K optimizations, preserving the arithmetic order regardless of batch changes.19 This exact reproducibility introduces a measurable performance tradeoff:

In empirical testing over a Qwen3-8B model, executing 1,000 runs under standard vLLM produced 80 unique completions, while forcing batch-invariant operations yielded a single, consistent completion across all runs, accompanied by a 61.5% execution latency penalty.18

#### **The Replay and Reproducibility Typology**

Because forcing bit-exact determinism across third-party cloud providers, variable GPU clusters, and dynamic databases is often impractical, systems must design for graded levels of verification 1:

| Replay Mode | Target Objective | Execution Constraints | State Preservation Requirement | System Verifications |
| :---- | :---- | :---- | :---- | :---- |
| **Exact Replay** | Recreate bitwise identical outputs for debugging.1 | Requires identical hardware, batch-invariant kernels, pinned model snapshots, and matching seeds.20 | Bit-exact saving of weights, inputs, and attention states.20 | Recomputes forward passes to verify exact token matches.20 |
| **Constrained Replay** | Recreate comparable completions on standard engines.1 | Executes on matching model versions with fixed sampling parameters and temperatures.5 | Saves the compiled prompt and exact decoding configurations.2 | Compares output tokens to ensure they fall within acceptable semantic ranges.1 |
| **Semantic Replay** | Verify that a rerun satisfies equivalent logical constraints.1 | Allows the model, prompt, or database to vary within defined boundaries.1 | Saves the evaluation rubric, task schema, and input goals.1 | Evaluates whether the new completion satisfies identical Pydantic rules and NLI grounding.1 |
| **Counterfactual Replay** | Compare how a change to a single variable alters behavior.1 | Modifies one parameter (e.g., prompt version, retrieved chunk) while keeping others constant.1 | Saves the base run as a template and isolates the variable under test.1 | Maps behavior changes to identify regressions or evaluate improvements.1 |
| **Forensic Replay** | Reconstruct the cause of a failure when re-execution is impossible.1 | Does not execute the active model; relies entirely on recorded logs and states.1 | Saves the complete execution trace, tool arguments, and environment hashes.3 | Walks the recorded sequence to pinpoint the exact failure location.3 |
| **Audit-Only** | Prove compliance with policies without exposing raw data.22 | Utilizes cryptographic hashes and signatures to verify states.1 | Saves the metadata headers, content hashes, and digital signatures.4 | Validates the signatures against root authorities to prove the run occurred as logged.15 |

### **Determinism Decay and Environment Drift**

A primary engineering challenge is that reproducibility degrades over time due to external dependencies.1 Even with fixed seeds and invariant kernels, the surrounding execution environment shifts silently, causing "determinism decay".1  
The primary operational drivers of this decay are categorized below:

* **Provider Model Updates:** Third-party API model weights are modified without changing the public model identifier, causing divergent semantic outputs.1  
* **Kernel and Library Drift:** Minor updates to deep learning frameworks (e.g., PyTorch, CUDA libraries) change internal mathematical approximations, mutating low-level floats.2  
* **Expired Caches and Rebuilt Indexes:** Changes to vector database structures or the expiration of semantic caches alter the exact retrieved context.1  
* **Time-Dependent Inputs:** System prompts injecting dynamic variables (e.g., current_time = "2026-06-11T23:19:00Z") force models into divergent logical paths.1  
* **Feature Flag Modifications:** Silent deployment updates change the active routing configurations or context limits mid-session.1

To address this, platforms must avoid false promises of bit-exact determinism, defining instead strict "reproducibility envelopes" that capture the complete state of the execution context.1

## **Verification Artifact Bundle Schema**

The central evidence container of this architecture is the **Verification Artifact Bundle**. It is a structured, schema-validated evidence object that records the minimum sufficient information required to audit, replay, or explain a consequential AI output or action.

The bundle should support JSON or CBOR serialization depending on platform tooling. It may be wrapped in an in-toto / DSSE-style envelope or another signed attestation format when the artifact class requires cryptographic proof. The important architectural rule is not the file format fetish—tempting though that altar is—but the preservation of signed, versioned, replayable evidence.

| Block Name | Example Field Paths | Capture Rule | Purpose |
| :---- | :---- | :---- | :---- |
| **Identity & Scope** | `identity.request_id`, `identity.trace_id`, `identity.session_hash`, `identity.tenant_hash`, `identity.subject_hash`, `identity.workflow_id`, `identity.risk_class` | Store opaque IDs or scoped hashes by default; raw identifiers only in restricted audit stores. | Correlates the bundle with traces, sessions, tenants, subjects, workflows, and risk policy. |
| **Prompt Lineage** | `prompt.template_id`, `prompt.template_version`, `prompt.compiler_hash`, `prompt.compiled_hash`, `prompt.payload_ref`, `prompt.token_count` | Store hash and secure reference for compiled prompt; raw prompt only under access-controlled registry. | Proves which prompt template and compiled prompt shaped execution. |
| **Model / Route Manifest** | `model.requested_route`, `model.selected_route`, `model.provider`, `model.model_version_ref`, `model.decoding_settings_hash`, `model.routing_reason` | Store route/model manifest and provider-visible version; note when hosted provider cannot provide weight hash. | Documents model selection, fallback, decoding, and route changes. |
| **Retrieval Snapshot** | `retrieval.index_id`, `retrieval.index_version`, `retrieval.query_hash`, `retrieval.rewrite_hashes`, `retrieval.chunk_ids`, `retrieval.source_version_hashes`, `retrieval.evidence_refs` | Store document IDs, chunk IDs, source hashes, scores, coordinates, and secure refs; avoid raw text in general bundle. | Proves what evidence was searched, selected, omitted, cited, or unavailable. |
| **Tool Execution** | `tool.name`, `tool.schema_version`, `tool.action_class`, `tool.payload_hash`, `tool.idempotency_key_hash`, `tool.pre_state_hash`, `tool.post_state_hash`, `tool.verification_status` | Store payload hashes and redacted summaries; sensitive request/response bodies by secure reference. | Proves tool intent, arguments, authorization, side-effect status, and verification outcome. |
| **State Ledger** | `state.plan_id`, `state.step_id`, `state.transition_log`, `state.loop_count`, `state.no_progress_count`, `state.fallback_state`, `state.termination_reason` | Store state transitions and hashes; never require hidden chain-of-thought capture. | Reconstructs the workflow trajectory without exposing private reasoning. |
| **User / Reviewer Edit** | `edit.draft_hash`, `edit.final_hash`, `edit.diff_ref`, `edit.editor_hash`, `edit.edit_class`, `edit.approval_state` | Store diffs, hashes, and secure refs; redact sensitive text as required. | Shows how user or reviewer modifications changed the artifact. |
| **Output Lineage** | `output.raw_hash`, `output.post_processed_hash`, `output.policy_filtered_hash`, `output.redacted_hash`, `output.rendered_hash`, `output.final_hash` | Store transformation hashes and secure refs; raw outputs only under policy. | Tracks every transformation between model output and final displayed/submitted output. |
| **Policy Enforcement** | `policy.bundle_version`, `policy.rule_ids`, `policy.decisions`, `policy.enforcement_points`, `policy.override_state`, `policy.reviewer_hash` | Store deterministic rule results separately from classifier scores. | Proves which policy checks ran, where, and with what outcome. |
| **Human Approval** | `approval.request_id`, `approval.status`, `approval.maker_hash`, `approval.checker_hash`, `approval.payload_hash`, `approval.expires_at`, `approval.signature_ref` | Store signed approval metadata and payload hash. | Proves approval state, separation of duties, and freshness of authorization. |
| **Audit & Integrity** | `audit.bundle_hash`, `audit.parent_hash`, `audit.signature_ref`, `audit.created_at`, `audit.retention_class`, `audit.legal_hold`, `audit.redaction_class` | Sign bundle hash or envelope; store chain metadata and retention policy. | Makes the evidence record tamper-evident and governed. |

### **Risk-Class Tiering Policy**

The bundle should adapt to risk class:

| Risk Class | Capture Profile |
| :---- | :---- |
| **Low** | Metadata, hashes, route IDs, status, and short-retention trace links. |
| **Medium** | Metadata plus selected redacted snippets, secure refs, policy decisions, and tool hashes. |
| **High** | Complete-enough evidence trail with secure references, signatures, approval records, state verification, and stricter retention. |
| **Critical / Regulated** | High-risk profile plus legal hold support, stronger signing, replay package linkage, reviewer identity controls, and access-audited payload vaults. |

High risk does **not** mean “store everything raw forever.” It means preserve enough evidence to audit and replay the decision while minimizing raw sensitive payloads, enforcing access controls, and recording when data was intentionally not captured.

## **Reproducibility Envelope Model**

Every consequential AI execution should run inside a **Reproducibility Envelope**. The envelope records the system state needed to recompile, inspect, simulate, or replay the run. The reproducibility unit is not “the model” alone; it is the model route, prompt compiler, retrieval surface, policy bundle, tool contracts, feature flags, runtime configuration, and evidence state.

```
REPRODUCIBILITY ENVELOPE

+----------------------------------------------------------+
| Prompt Assembly                                          
| template id/version | compiler hash | context policy     
+----------------------------------------------------------+
| Inference Route                                          
| provider route | model ref | tokenizer | decoding config 
+----------------------------------------------------------+
| Retrieval Surface                                        
| corpus version | index hash | embedding model | filters  
+----------------------------------------------------------+
| Tool and Action Surface                                  
| tool schemas | mock mode | credential policy | idempotency
+----------------------------------------------------------+
| Policy and Governance                                    
| policy bundle | approval flow | safety filters | retention
+----------------------------------------------------------+
| Runtime Environment                                      
| container image | dependency lock | feature flags | region 
+----------------------------------------------------------+
| Replay Constraints                                       
| exact | constrained | semantic | counterfactual | forensic
+----------------------------------------------------------+
```

| Parameter Category | Example Field Paths | Validation Source Registry |
| :---- | :---- | :---- |
| **Prompt Assembly** | `env.prompt.template_id`, `env.prompt.template_version`, `env.prompt.compiler_hash`, `env.prompt.context_assembler_hash`, `env.prompt.memory_policy_version` | Prompt registry, compiler release manifest, context policy registry. |
| **Inference Runtime** | `env.model.requested_route`, `env.model.selected_route`, `env.model.provider_api_version`, `env.model.snapshot_ref`, `env.model.tokenizer_ref`, `env.model.decoding_settings_hash` | Model registry, provider manifest, gateway route manifest. |
| **Serving Environment** | `env.serving.container_digest`, `env.serving.runtime_version`, `env.serving.cuda_or_accelerator_version`, `env.serving.batch_policy`, `env.serving.determinism_mode` | Container registry, deployment manifest, serving-engine config. |
| **Retrieval Engine** | `env.retrieval.index_id`, `env.retrieval.index_version`, `env.retrieval.corpus_version`, `env.retrieval.embedding_model_ref`, `env.retrieval.chunker_version`, `env.retrieval.reranker_ref`, `env.retrieval.permission_policy_version` | Vector/index registry, corpus manifest, ingestion pipeline records. |
| **Tool Integration** | `env.tool.manifest_hash`, `env.tool.schema_versions`, `env.tool.server_digest`, `env.tool.mock_mode`, `env.tool.side_effect_mode`, `env.tool.credential_policy_version` | Tool registry, container registry, credential broker policy. |
| **Policy and Safety** | `env.policy.bundle_hash`, `env.policy.safety_filter_version`, `env.policy.redaction_policy_version`, `env.policy.approval_flow_version`, `env.policy.retention_class` | Policy registry, governance manifest, approval workflow registry. |
| **System and UI** | `env.ui.version`, `env.ui.renderer_hash`, `env.feature_flags.hash`, `env.deployment.hash`, `env.region`, `env.cache_state`, `env.degraded_mode_state` | Web client registry, feature-flag snapshot, deployment manifest. |
| **Replay Controls** | `env.replay.mode`, `env.replay.egress_policy`, `env.replay.payload_capture_class`, `env.replay.tool_response_source`, `env.replay.allowed_counterfactuals` | Replay policy registry and audit configuration. |

This envelope does not guarantee bit-exact reproduction in every environment. It defines what must be pinned, captured, or declared unknown so auditors can distinguish exact replay, constrained replay, semantic replay, counterfactual replay, forensic replay, and audit-only verification.30

## **Specialized Evidence Records**

### **The Prompt Lineage Record**

To determine whether behavior changed because of template edits, context assembly, memory inclusion, or dynamic variable injection, the system preserves a Prompt Lineage Record.

| Field Name | Physical Data Type | Capture Rule | Operational Purpose |
| :---- | :---- | :---- | :---- |
| `prompt.template_id` | String | Metadata. | Identifies the registered template. |
| `prompt.template_version` | SemVer / release ID | Metadata. | Matches the active template registry version. |
| `prompt.template_hash` | SHA-256 hash | Hash. | Verifies template integrity without exposing text. |
| `prompt.template_ref` | Secure reference | Restricted. | Points to raw template in controlled registry if access is allowed. |
| `prompt.compiler_version` | Version / commit SHA | Metadata. | Identifies prompt compiler behavior. |
| `prompt.context_assembler_version` | Version / commit SHA | Metadata. | Identifies context assembly logic. |
| `prompt.variable_manifest` | JSON object / hash | Redacted or secure reference. | Records which variables were injected and their capture classes. |
| `prompt.memory_refs` | Array of hashes/refs | Hash/reference. | Identifies memory items included in context. |
| `prompt.evidence_refs` | Array of chunk/source refs | Hash/reference. | Identifies retrieval chunks inserted into context. |
| `prompt.tool_schema_refs` | Array of schema hashes | Metadata/hash. | Records tool definitions available to the model. |
| `prompt.policy_refs` | Array of policy hashes | Metadata/hash. | Records dynamic safety/governance instructions. |
| `prompt.message_boundaries` | Structured offsets / roles | Metadata. | Maps system, developer, user, tool, and context scopes. |
| `prompt.compiled_hash` | SHA-256 hash | Hash. | Verifies exact compiled prompt. |
| `prompt.compiled_ref` | Secure reference | Restricted. | Allows controlled replay of compiled prompt when permitted. |
| `prompt.token_count` | Integer | Metadata. | Records input size for cost and replay. |
| `prompt.version_diff_ref` | Secure diff reference | Restricted. | Links to template diff without dumping sensitive prompt text into logs. |

Saving only raw templates is insufficient because compiled prompts include dynamic variables, memories, retrieved evidence, tool schemas, policies, and message boundaries. The lineage record proves what was assembled without requiring every observer to see the full prompt text.

### **The Model and Route Manifest**

Model route decisions are critical audit artifacts. A silent downgrade, fallback, cache hit, provider change, or safety-route change can alter quality, cost, latency, and policy behavior.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `route.requested_route` | String | Metadata. | Captures requested capability profile. |
| `route.selected_route` | String | Metadata. | Captures actual executed route. |
| `route.provider_name` | String | Metadata. | Records model host or serving class. |
| `route.provider_api_version` | String | Metadata if available. | Captures provider/API version surface. |
| `route.model_id` | String | Metadata. | Records requested model identifier. |
| `route.model_snapshot_ref` | String / secure ref | Metadata/reference. | Records model version or provider-supplied snapshot when available. |
| `route.tokenizer_ref` | String / hash | Metadata/hash. | Captures tokenizer version for replay. |
| `route.routing_reason` | Enum string | Metadata. | Examples: `nominal`, `failover`, `quota`, `cost_policy`, `quality_floor`, `safety_policy`, `cache_hit`. |
| `route.fallback_path_id` | String | Metadata. | Identifies fallback chain step if used. |
| `route.cached_response` | Boolean | Metadata. | Flags whether semantic, prefix, or response cache was used. |
| `route.router_policy_hash` | SHA-256 hash | Hash. | Verifies active routing policy. |
| `route.quality_floor_id` | String | Metadata. | Links route decision to required task quality floor. |
| `route.safety_floor_id` | String | Metadata. | Links route decision to safety/policy floor. |
| `route.cost_estimate` | Number / object | Metadata. | Records preflight cost estimate and confidence. |
| `route.actual_cost_ref` | Reference / number | Metadata/reference. | Links to reconciled cost ledger. |
| `route.latency_profile` | Object | Metadata. | Records expected/observed latency class. |
| `route.context_headroom_tokens` | Integer | Metadata. | Verifies route could fit required context. |
| `route.tool_capability_required` | Boolean | Metadata. | Flags if task needed tool-use support. |
| `route.modality` | Array of strings | Metadata. | Records modalities processed, such as text, image, audio, or video. |
| `route.tenant_tier_hash` | Hash/string | Metadata/hash. | Supports priority/cost analysis without exposing tenant identity. |
| `route.budget_state_ref` | Secure reference | Restricted. | Links to budget ledger. |
| `route.decoding_settings_hash` | SHA-256 hash | Hash. | Verifies temperature, top-p, max-token, and related decoding configuration. |
| `route.decoding_settings_ref` | Secure reference | Restricted. | Allows controlled replay of decoding parameters. |
| `route.stop_reason` | String | Metadata. | Records completion stop condition. |
| `route.user_disclosure_required` | Boolean | Metadata. | Records whether user should have been told. |
| `route.user_disclosure_shown` | Boolean | Metadata. | Records whether disclosure occurred. |


### **The Retrieval Evidence Snapshot**

GRC verification requires proving the context available before generation. The retrieval snapshot should show what was searched, what was authorized, what was selected, what was omitted, and how citations map to evidence.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `retrieval.raw_query_hash` | SHA-256 hash | Hash. | Verifies original query without exposing it. |
| `retrieval.raw_query_ref` | Secure reference | Restricted. | Allows controlled access to raw query if needed. |
| `retrieval.rewrite_hashes` | Array of hashes | Hash. | Records query reformulation steps. |
| `retrieval.index_id` | String | Metadata. | Identifies searched index or partition. |
| `retrieval.index_version` | String | Metadata. | Tracks index migrations and rebuilds. |
| `retrieval.corpus_version` | String/hash | Metadata/hash. | Tracks source corpus state. |
| `retrieval.embedding_model_ref` | String/hash | Metadata/hash. | Records query embedding model. |
| `retrieval.permission_policy_version` | String/hash | Metadata/hash. | Records authorization policy used. |
| `retrieval.tenant_scope_hash` | SHA-256 hash | Hash. | Confirms tenant scope without raw tenant leakage. |
| `retrieval.permission_filter_ids` | Array of strings | Metadata. | Records RBAC/ABAC filters applied. |
| `retrieval.metadata_filter_hash` | SHA-256 hash | Hash. | Verifies structural filters. |
| `retrieval.candidate_ids` | Array | Metadata. | Logs candidate IDs and scores. |
| `retrieval.selected_chunk_ids` | Array | Metadata. | Logs chunks admitted into context. |
| `retrieval.omitted_chunk_ids` | Array | Metadata. | Logs relevant chunks pruned or omitted. |
| `retrieval.rerank_score_summary` | Object | Metadata. | Records score distribution or secure score references. |
| `retrieval.source_version_hashes` | Array | Hash. | Verifies parent document versions. |
| `retrieval.evidence_refs` | Array of secure refs | Restricted. | Links to raw text or regions when access is allowed. |
| `retrieval.coordinates` | Array of objects | Metadata. | Page, span, line, offset, or bounding boxes. |
| `retrieval.citation_map_hash` | SHA-256 hash | Hash. | Verifies claim-to-evidence mapping. |
| `retrieval.citation_map_ref` | Secure reference | Restricted. | Allows detailed citation replay. |
| `retrieval.conflict_signal` | Boolean/object | Metadata. | Flags source contradiction or unresolved conflict. |
| `retrieval.stale_flag` | Boolean/object | Metadata. | Flags source age or freshness concerns. |
| `retrieval.inaccessible_flag` | Boolean | Metadata. | Flags deleted or unauthorized source references. |
| `retrieval.claim_links` | Array of hashes/refs | Hash/reference. | Maps generated claims to chunk/source evidence. |

This snapshot provides bounded verifiability: it can distinguish retrieval failure, stale-source failure, citation failure, and generation failure when capture coverage, source versioning, and permission metadata are complete.

### **The Tool Execution Record**

A generated statement claiming an action succeeded is not evidence of success. Auditing requires verification of tool authorization, payload integrity, idempotency, execution status, and post-action state.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `tool.planned_name` | String | Metadata. | Tool the model/coordinator intended to invoke. |
| `tool.executed_name` | String | Metadata. | Tool actually executed. |
| `tool.schema_version` | String | Metadata. | Tracks API/tool contract version. |
| `tool.action_class` | Enum string | Metadata. | Examples: `read_only`, `idempotent_mutation`, `non_idempotent_mutation`, `high_impact_action`. |
| `tool.permission_decision` | Object/string | Metadata. | Records authorization outcome. |
| `tool.credential_ref_hash` | SHA-256 hash | Hash. | Verifies scoped credential reference without storing token. |
| `tool.argument_hash` | SHA-256 hash | Hash. | Verifies exact argument object. |
| `tool.argument_summary` | Redacted object | Redacted. | Provides safe diagnostic summary. |
| `tool.argument_ref` | Secure reference | Restricted. | Controlled access to full arguments if policy allows. |
| `tool.user_edit_diff_ref` | Secure reference | Restricted. | Records human changes to tool arguments. |
| `tool.idempotency_key_hash` | SHA-256 hash | Hash. | Supports duplicate-action prevention. |
| `tool.request_payload_hash` | SHA-256 hash | Hash. | Verifies outbound request. |
| `tool.request_payload_ref` | Secure reference | Restricted. | Controlled access to raw request. |
| `tool.response_payload_hash` | SHA-256 hash | Hash. | Verifies inbound response. |
| `tool.response_payload_ref` | Secure reference | Restricted. | Controlled access to raw response. |
| `tool.error_status` | Enum string | Metadata. | Examples: `success`, `timeout`, `provider_error`, `validation_error`, `unknown`. |
| `tool.retry_decision` | Enum string | Metadata. | Examples: `none`, `backoff`, `abort`, `manual_review`, `reconcile`. |
| `tool.quota_consumed` | Number/object | Metadata. | Tracks external quota or cost usage. |
| `tool.side_effect_summary` | Object | Redacted metadata. | Records expected and observed side-effect classes. |
| `tool.state_pre_hash` | SHA-256 hash | Hash. | Proves source-of-record state before action. |
| `tool.state_post_hash` | SHA-256 hash | Hash. | Proves source-of-record state after action, if known. |
| `tool.verification_status` | Enum string | Metadata. | Examples: `verified`, `failed`, `pending`, `unknown`, `not_applicable`. |
| `tool.compensation_supported` | Boolean | Metadata. | Records whether compensation is possible. |
| `tool.compensation_plan_ref` | Secure reference | Restricted. | Links to compensation plan without leaking endpoint surface. |
| `tool.user_confirmation_ref` | Secure reference/hash | Restricted. | Proves required consent or approval where applicable. |

### **The Intermediate State Ledger**

To trace execution trajectories without exposing hidden reasoning or private chain-of-thought, the system preserves an Intermediate State Ledger. It records state transitions, decisions, validation failures, and observable progress—not the model’s private scratchpad.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `state.planner_version` | Version/hash | Metadata. | Tracks planner/orchestrator implementation. |
| `state.subtask_graph_hash` | SHA-256 hash | Hash. | Verifies task graph structure. |
| `state.subtask_graph_ref` | Secure reference | Restricted. | Allows controlled inspection of task DAG. |
| `state.transitions` | Array of objects | Metadata. | Logs state-machine transitions and events. |
| `state.loop_count` | Integer | Metadata. | Tracks total loop steps against workflow profile. |
| `state.loop_budget_profile` | String | Metadata. | Links loop limit to risk/workflow class. |
| `state.memory_read_hashes` | Array of hashes | Hash. | Records memory items read. |
| `state.memory_write_hashes` | Array of hashes | Hash. | Records memory items written. |
| `state.retry_branches` | Array of objects | Metadata. | Logs retry and branch attempts. |
| `state.repair_attempts` | Array of objects | Metadata. | Logs output/schema repair attempts. |
| `state.validation_failures` | Array of objects | Metadata/redacted. | Logs parser/schema/business-rule failures. |
| `state.no_progress_count` | Integer | Metadata. | Tracks repeated states without material progress. |
| `state.no_progress_policy` | String | Metadata. | Links action to halt, replan, or escalation policy. |
| `state.tool_result_hashes` | Array of hashes | Hash. | Correlates tool results with steps. |
| `state.policy_blocks` | Array of rule IDs | Metadata. | Logs triggered safety/compliance blocks. |
| `state.fallback_state` | Enum string | Metadata. | Examples: `nominal`, `degraded_model`, `degraded_retrieval`, `cached`, `partial`, `fail_closed`. |
| `state.termination_reason` | Enum string | Metadata. | Examples: `success`, `max_steps`, `budget_exhausted`, `user_cancelled`, `policy_block`, `unknown_state`. |

### **The User Edit and Output Diff Record**

The final answer displayed or submitted is the last artifact in a chain of transformations. The system should record hashes, diffs, edit metadata, and secure references while avoiding uncontrolled raw output storage.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `edit.original_draft_hash` | SHA-256 hash | Hash. | Verifies model-generated draft. |
| `edit.original_draft_ref` | Secure reference | Restricted. | Controlled access to raw draft if retained. |
| `edit.final_text_hash` | SHA-256 hash | Hash. | Verifies final edited text. |
| `edit.final_text_ref` | Secure reference | Restricted. | Controlled access to final text if retained. |
| `edit.editor_hash` | SHA-256 hash | Hash/metadata. | Identifies editor without broad raw identity exposure. |
| `edit.editor_role` | String | Metadata. | Captures role or authority class. |
| `edit.edit_class` | Enum string | Metadata. | Examples: `factual`, `stylistic`, `policy`, `tool_argument`, `redaction`, `citation`, `approval`. |
| `edit.timestamp` | ISO-8601 timestamp | Metadata. | Records edit time. |
| `edit.target_field` | String | Metadata. | Identifies field or region altered. |
| `edit.status` | Enum string | Metadata. | Examples: `accepted`, `rejected`, `overridden`, `pending_review`. |
| `edit.diff_hash` | SHA-256 hash | Hash. | Verifies diff artifact. |
| `edit.diff_ref` | Secure reference | Restricted. | Controlled access to unified or structured diff. |
| `edit.updated_memory` | Boolean | Metadata. | Records if edit updated memory. |
| `edit.entered_evals` | Boolean | Metadata. | Records if run was promoted to evals. |
| `edit.changed_tool_args` | Boolean | Metadata. | Flags edits to tool parameters. |
| `edit.preserved_in_final` | Boolean | Metadata. | Verifies whether edit survived final rendering/submission. |

Output lineage should capture transformation hashes for each stage:

1. `output.raw_model_hash`
2. `output.post_processed_hash`
3. `output.policy_filtered_hash`
4. `output.redacted_hash`
5. `output.citation_injected_hash`
6. `output.rendered_ui_hash`
7. `output.human_edited_hash`
8. `output.final_submitted_hash`

Raw text for any stage should be stored only according to capture class and retention policy.

### **The Policy Check Record**

Policy checks must function as executable verification artifacts, not passive annotations. Deterministic rules, classifiers, human approvals, and override paths should be recorded separately so auditors can tell what kind of control actually fired.

| Field Name | Physical Data Type | Capture Rule | Description / Key Purpose |
| :---- | :---- | :---- | :---- |
| `policy.bundle_version` | Version/hash | Metadata/hash. | Tracks active policy bundle. |
| `policy.rule_ids` | Array of strings | Metadata. | Identifies rules executed. |
| `policy.rule_authority` | String/hash | Metadata/hash. | Documents policy source or owner. |
| `policy.input_hash` | SHA-256 hash | Hash. | Verifies evaluated input. |
| `policy.input_ref` | Secure reference | Restricted. | Controlled access to evaluated content. |
| `policy.decision` | Enum string | Metadata. | Examples: `allow`, `block`, `escalate`, `redact`, `degrade`, `require_approval`. |
| `policy.rule_type` | Enum string | Metadata. | Examples: `deterministic`, `classifier`, `human_review`, `external_policy`. |
| `policy.threshold` | Number/object | Metadata. | Classifier threshold when applicable. |
| `policy.score` | Number/object | Metadata. | Classifier score when applicable. |
| `policy.enforcement_point` | Enum string | Metadata. | Examples: `ingress`, `context_assembly`, `pre_action`, `tool_gateway`, `egress`, `ui_render`. |
| `policy.result_status` | Enum string | Metadata. | Examples: `pass`, `fail`, `error`, `not_applicable`. |
| `policy.override_status` | Enum string | Metadata. | Examples: `none`, `requested`, `authorized`, `denied`, `expired`. |
| `policy.exception_summary` | String/object | Redacted. | Logs parser or system exception safely. |
| `policy.reviewer_hash` | SHA-256 hash | Restricted metadata. | Identifies reviewer where applicable. |
| `policy.approval_ref` | Secure reference | Restricted. | Links to approval trail. |
| `policy.escalation_ref` | Secure reference | Restricted. | Links to escalation queue or package. |
| `policy.final_action_state` | Enum string | Metadata. | Examples: `completed`, `blocked`, `pending`, `unknown`, `review_required`. |


## **Replay Package Model and Execution Safety**

To audit, debug, or evaluate a transaction safely, the system compiles a Replay Package. A Replay Package is not merely a folder of logs; it is a controlled reconstruction environment designed to reproduce or simulate behavior without mutating production systems.

A replay package contains:

* **Trace Graph:** Parent-child spans and timing relationships.
* **Verification Bundle:** Signed evidence record for the run.
* **Reproducibility Envelope:** Prompt, model, retrieval, policy, tool, and runtime state.
* **Dependency Manifest:** Container digests, package locks, runtime versions, and feature flags.
* **Tool Mocks:** Signed, recorded responses or deterministic simulators.
* **Payload Vault References:** Secure references to redacted or raw payloads where permitted.
* **Policy Bundle:** Exact rules, thresholds, and approval requirements.
* **Evaluation Rubric:** Scoring rules for semantic or counterfactual replay.

```
REPLAY PACKAGE CONTAINER

+----------------------------------------------------------+
| Replay Orchestrator                                      
|  loads trace graph, bundle, envelope, policy, rubric      
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Network and Credential Controls                          
|  no production credentials                                
|  egress blocked or allowlisted                            
|  production endpoints denied                              
|  secrets replaced with inert references                   
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Mocked / Simulated Dependencies                           
|  recorded LLM outputs                                     
|  signed tool responses                                    
|  retrieval snapshot                                       
|  sandbox database                                         
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Replay Result                                             
|  exact replay, constrained replay, semantic comparison,    
|  counterfactual analysis, or forensic timeline             
+----------------------------------------------------------+
```

These execution bounds are mapped across six replay modes:

| Replay Mode | Egress Policy | Input Constraints | Execution Layer | Target Verification | Side-Effect Prevention |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Read-Only Replay** | Egress blocked except approved artifact stores. | Inputs locked to original values. | Recompiles prompts, layouts, UI rendering, and local transformations. | Verifies rendering, formatting, redaction, and citation display. | No tool credentials; no production write paths. |
| **Mocked-Tool Replay** | Egress blocked. | Inputs locked; tool responses loaded from signed mocks. | Re-executes orchestration against recorded dependency responses. | Evaluates route behavior, state transitions, and output variance. | Tool gateway intercepts all calls and returns mocks. |
| **Sandbox Replay** | Egress restricted to isolated test network. | Inputs locked or copied into sandbox fixtures. | Runs against test database, test tool servers, and inert credentials. | Evaluates integration behavior under controlled conditions. | Production endpoints denied; credentials scoped to sandbox only. |
| **Counterfactual Replay** | Egress blocked or sandbox-only. | Allows one declared variable modification at a time. | Re-runs selected prompt/model/retrieval/policy variant. | Attributes behavioral changes to the modified variable. | Tools mocked unless explicit sandbox mutation is approved. |
| **Semantic Replay** | Egress blocked or sandbox-only. | Inputs locked; model may vary within declared route envelope. | Evaluates logical constraints, grounding, citations, and policy outcomes. | Verifies equivalent behavior rather than exact tokens. | No production side effects; unknown states remain marked unknown. |
| **Forensic Replay** | No execution egress. | No active model/tool execution. | Timeline analysis of recorded evidence. | Identifies likely failure point from trace, ledger, and artifacts. | Complete decoupling from execution systems. |


## **Evidence Chain and Tamper-Evidence Infrastructure**

To detect silent modification or deletion of verification artifacts, the platform should implement tamper-evident evidence chains. Each Verification Artifact Bundle is canonicalized, hashed, signed when required, and linked to prior artifacts or workflow checkpoints.

The hash-chain relation can be expressed as:

h_i = H(canonical(bundle_i) || h_(i-1))

where `H` is a collision-resistant hash function, `canonical(bundle_i)` is a deterministic serialization of the bundle, and `h_(i-1)` is the previous chain hash. This does not make evidence indestructible or magically true. It makes later alteration detectable when verification is performed.

The platform may use several signing and provenance mechanisms depending on artifact type:

1. **Signed Attestation Envelope:** Verification bundles, model manifests, tool manifests, and replay packages can be wrapped in DSSE / in-toto-style signed envelopes.
2. **Sigstore / Fulcio / Rekor:** CI/CD artifacts, container images, model bundles, and build outputs can use keyless signing and transparency logging where appropriate.
3. **C2PA Manifests:** Images, charts, video, generated media, or datasets may carry embedded or sidecar C2PA manifests. Text-only or binary artifacts may bind to C2PA through sidecar manifests and asset references.
4. **Internal WORM / Append-Only Ledger:** Regulated systems may store chain checkpoints in WORM object storage or append-only audit databases.
5. **External Anchoring:** High-assurance systems may periodically anchor evidence-chain checkpoints externally to improve tamper detection.

```
TAMPER-EVIDENT EVIDENCE CHAIN

+----------------------------------------------------------+
| Artifact Created                                         
|  bundle, replay package, manifest, snapshot, or diff     
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Canonicalize and Hash                                    
|  deterministic serialization                             
|  content hash                                            
|  parent hash                                             
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Sign / Attest When Required                              
|  DSSE / in-toto envelope                                  
|  Sigstore or internal signing                             
|  policy-defined signer identity                           
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Store Evidence                                           
|  content-addressed storage                                
|  retention class                                          
|  access policy                                            
|  redaction status                                         
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Verify Chain                                             
|  validate hashes                                          
|  validate signatures                                      
|  validate timestamps                                      
|  detect missing or altered records                        
+----------------------------------------------------------+
```

| Field Name | JSON Field Key Path | Data Type | Validation Standard |
| :---- | :---- | :---- | :---- |
| **Artifact ID** | `chain.artifact_id` | String | UUID or platform artifact ID. |
| **Artifact Type** | `chain.artifact_type` | Enum string | Examples: `verification_bundle`, `replay_package`, `manifest`, `snapshot`, `diff`, `policy_record`. |
| **Content Hash** | `chain.content_hash` | String | SHA-256 or approved hash algorithm. |
| **Parent Hash** | `chain.parent_hash` | String/null | Previous chain hash or null for chain root. |
| **Canonicalization Version** | `chain.canonicalization_version` | String | Defines deterministic serialization rules. |
| **Created By** | `chain.created_by_hash` | String | Workload, service, or user hash. |
| **Created At** | `chain.created_at` | Timestamp | ISO-8601 with timezone. |
| **Signed By** | `chain.signed_by` | String/reference | Signing identity or certificate reference. |
| **Signature Reference** | `chain.signature_ref` | Secure reference | Link to DSSE, Sigstore, or internal signature record. |
| **Transparency Entry** | `chain.transparency_log_ref` | Optional reference | Rekor or internal transparency log entry. |
| **Storage Location** | `chain.storage_ref` | Secure reference | Content-addressed object or archive reference. |
| **Retention Class** | `chain.retention_class` | Enum string | Policy-defined retention profile. |
| **Access Policy** | `chain.access_policy_id` | String | Policy governing read/write access. |
| **Redaction Status** | `chain.redaction_status` | Enum string | Examples: `metadata_only`, `redacted`, `reference_only`, `restricted_full_payload`. |
| **Deletion Eligibility** | `chain.deletion_eligibility` | Timestamp/string | Date or policy state, subject to legal hold. |
| **Legal Hold** | `chain.legal_hold` | Boolean | True when deletion is suspended. |
| **Verification Status** | `chain.verification_status` | Enum string | Examples: `verified`, `failed`, `degraded`, `not_checked`. |

If a historical record is altered, hash-chain verification fails and the system can trigger investigation, containment, and recovery workflows. That is the sober version. The cartoon version where cryptography tackles the attacker through a window is, regrettably, not in scope.


## **Artifact Privacy, Redaction, and Retention Model**

Verification artifacts contain sensitive operational data: proprietary documents, customer records, payment details, credentials, policy decisions, reviewer comments, tool payloads, and source-of-record state. Evidence trails must therefore follow a data-minimization model. Auditability is not permission to build a beautifully indexed breach warehouse.

Redaction should be layered. Regex and NER scans are useful but insufficient alone. The system should combine structured field suppression, secret scanning, policy-aware capture classes, secure payload references, access logging, retention limits, and legal-hold rules.

```
PRIVACY REDACTION WORKFLOW

+----------------------------------------------------------+
| Artifact Capture Point                                   
|  prompt, retrieval, tool, output, review, policy, state   
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Capture-Class Decision                                   
|  never capture                                             
|  hash only                                                 
|  metadata only                                             
|  redacted snippet                                          
|  reference only                                            
|  restricted full payload                                   
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Redaction and Secret Controls                            
|  structured denylist                                       
|  regex / NER / classifier scan                             
|  source-aware policy                                       
|  credential stripping                                      
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Storage and Access Policy                                
|  content-addressed storage                                 
|  secure payload vault                                      
|  WORM archive when needed                                  
|  access logs and retention                                 
+----------------------------------------------------------+
```

| Capture Class | Technical Processing Rule | Physical Storage Substrate | Access Control Level | Typical Data Target |
| :---- | :---- | :---- | :---- | :---- |
| **0. Never Capture** | Strip or block before any persistent write. | No raw storage. | None for raw value; security event may record detection metadata. | Passwords, API tokens, private keys, CVVs, raw auth headers. |
| **1. Hash Only** | Store digest and capture metadata; discard raw payload. | Metadata database or audit ledger. | Restricted audit/SRE as policy allows. | Large payload identity, file equality checks, prompt/output fingerprints. |
| **2. Metadata Only** | Store size, schema, type, source ID, status, version, and timing. | Trace store or artifact metadata index. | Standard operational access if non-sensitive. | Parser status, route metadata, model IDs, schema names. |
| **3. Redacted Snippet** | Store minimal masked excerpt sufficient for debugging. | Secure artifact store with retention limit. | Purpose-bound operator/auditor access. | Conversation excerpts, validation errors, reviewer notes. |
| **4. Reference Only** | Store raw payload in secure vault; artifact stores only hash and reference. | Encrypted object store or payload vault. | Role-gated and access-logged. | Customer PDFs, full prompts, tool request/response bodies, raw completions. |
| **5. Restricted Full Payload** | Retain full payload only when legally or operationally required. | Segregated encrypted vault or WORM storage. | Strict audit/security/legal roles; all access logged. | Regulated evidence, high-impact transaction records, legal holds. |
| **6. Incident Mode** | Temporarily escalate capture under declared incident policy. | Forensic vault with short TTL or legal hold. | Incident/security team with approval trail. | Active attack traces, compromised tool sessions, outage forensics. |

Metadata can itself be sensitive. Tenant hashes, source IDs, route choices, policy decisions, and timing records can reveal business activity. Access rules should therefore apply to both payloads and metadata.


## **GRC Audit Query Playbook**

To support audits, investigations, dispute resolution, and regulatory inspections, the verification framework should map common audit questions to the artifacts and replay modes required to answer them.

| Audit Question | Required Verification Artifacts | Target Trace Fields / Metadata | Optimal Replay Mode | Technical Validation Path |
| :---- | :---- | :---- | :---- | :---- |
| **Why did the model cite this source?** | Retrieval Snapshot, Output Lineage, Model/Route Manifest. | `retrieval.selected_chunk_ids`, `retrieval.citation_map_ref`, `output.citation_injected_hash`. | Forensic Replay. | Verify claim-to-source mapping, source version, and evidence support. |
| **Was the source document actually retrieved?** | Retrieval Snapshot. | `retrieval.index_id`, `retrieval.selected_chunk_ids`, `retrieval.evidence_refs`. | Audit-Only. | Match chunk IDs and source hashes against retrieval trace and access log. |
| **Was the source version current?** | Retrieval Snapshot, Reproducibility Envelope. | `retrieval.source_version_hashes`, `retrieval.stale_flag`, `env.retrieval.corpus_version`. | Audit-Only. | Compare source hashes and freshness metadata against source registry. |
| **Which prompt version produced this answer?** | Prompt Lineage Record, Reproducibility Envelope. | `prompt.template_id`, `prompt.template_version`, `prompt.compiled_hash`. | Constrained Replay. | Recompile template under recorded envelope and compare compiled prompt hash. |
| **Did routing silently downgrade the model?** | Model and Route Manifest. | `route.requested_route`, `route.selected_route`, `route.routing_reason`, `route.user_disclosure_shown`. | Audit-Only. | Compare requested vs selected route and disclosure requirement. |
| **Which tool call failed, and did it succeed later?** | Tool Execution Record, State Ledger. | `tool.error_status`, `tool.verification_status`, `state.transitions`. | Forensic Replay. | Walk action ledger and post-action verification state. |
| **Did the tool actually execute, or did the model hallucinate success?** | Tool Execution Record, Output Lineage. | `tool.state_post_hash`, `tool.verification_status`, `output.final_hash`. | Forensic Replay. | Verify source-of-record state and compare completion claim to verified state. |
| **Was confirmation shown before transaction execution?** | Tool Execution Record, Policy Check Record, Human Approval Record. | `tool.user_confirmation_ref`, `policy.decision`, `approval.status`. | Forensic Replay. | Confirm approval/consent existed before high-impact action boundary. |
| **Did the user or reviewer edit the value?** | User Edit and Output Diff Record. | `edit.diff_ref`, `edit.editor_hash`, `edit.edit_class`, `edit.preserved_in_final`. | Counterfactual Replay. | Compare draft hash, diff hash, and final output hash. |
| **Did the model overwrite a correction?** | User Edit Record, Intermediate State Ledger, Output Lineage. | `edit.preserved_in_final`, `state.transitions`, `output.final_submitted_hash`. | Counterfactual Replay. | Compare subsequent outputs and state transitions against edited artifact. |
| **Which policy rule fired?** | Policy Check Record. | `policy.rule_ids`, `policy.bundle_version`, `policy.decision`, `policy.enforcement_point`. | Audit-Only. | Verify rule execution parameters and policy bundle version. |
| **Who approved the override?** | Policy Check Record, Human Approval Record, Evidence Chain. | `policy.override_status`, `policy.reviewer_hash`, `approval.signature_ref`. | Audit-Only. | Verify approval signature or audit record against trusted identity registry. |
| **Did the final UI hide a warning?** | Output Lineage, Policy Check Record, UI/Render Manifest. | `output.rendered_hash`, `policy.rule_ids`, `route.user_disclosure_shown`. | Forensic Replay. | Re-render under recorded UI version and verify warning/disclosure presence. |
| **Would a different model have behaved differently?** | Verification Bundle, Reproducibility Envelope, Evaluation Rubric. | `route.selected_route`, `env.model.snapshot_ref`, `prompt.compiled_hash`. | Counterfactual Replay. | Change one declared model/route variable and evaluate behavior under same rubric. |
| **Was sensitive data over-captured?** | Privacy Capture Record, Evidence Chain, Access Logs. | `chain.redaction_status`, `capture_class`, `access_policy_id`. | Audit-Only. | Verify capture class, retention class, and access events against privacy policy. |

## **Artifact Lifecycle and Storage Architecture**

Verification artifacts must be retained according to risk, regulatory obligations, incident status, tenant contract, and operational debugging value. Storage architecture should therefore be policy-based rather than hardcoded to one fixed age ladder. A low-risk FAQ trace, a failed high-impact payment action, a legal hold, and an active security incident should not share the same retention path just because the calendar was feeling democratic.

```
EVIDENCE STORAGE LIFECYCLE

+----------------------------------------------------------+
| Artifact Created                                         
|  bundle, trace, snapshot, manifest, diff, replay package  
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Classification                                            
|  risk class                                               
|  capture class                                            
|  retention class                                          
|  legal hold                                               
|  tenant / regulatory policy                               
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Hot Operational Store                                     
|  short debugging window                                   
|  fast lookup                                              
|  redacted or reference-first payloads                     
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Warm Audit Store                                          
|  content-addressed objects                                
|  signed manifests                                         
|  scoped metadata indexes                                  
|  access logging                                           
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Cold Compliance / Legal Hold                              
|  WORM or immutable archive where required                 
|  slower retrieval                                         
|  legal-hold controls                                      
|  deletion review                                          
+--------------------------+-------------------------------+
                           |
                           v
+----------------------------------------------------------+
| Expiry / Deletion / Preservation Decision                 
|  purge                                                    
|  retain under legal hold                                  
|  anonymize / aggregate                                    
|  preserve hash-only evidence                              
+----------------------------------------------------------+
```

The operational profiles, retrieval expectations, and redundancy targets of these storage tiers are specified below:

| Storage Tier | Retention Window | Retrieval Expectation | Storage Pattern | Encryption and Access | Typical Contents |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Hot Operational** | Short operational window defined by retention class. | Fast enough for debugging and incident triage. | Operational database, trace store, short-TTL object store, or managed equivalent. | Tenant-scoped encryption, role-based access, audit logs. | Metadata, hashes, redacted snippets, recent traces, pending action state. |
| **Warm Audit** | Medium retention window defined by audit and product policy. | Hours to day-scale retrieval depending on policy. | Content-addressed object storage, signed bundle registry, audit index. | Strong access control, access logging, scoped evidence views. | Verification bundles, manifests, secure payload references, signed hashes, reviewer artifacts. |
| **Cold Compliance** | Long retention window for regulated, contractual, or legal-hold artifacts. | Slower retrieval acceptable. | WORM storage, immutable archive, Glacier-like storage, or compliant managed archive. | Legal/compliance access controls, key management, deletion controls. | Signed evidence chains, legal-hold bundles, high-impact action records, compliance artifacts. |
| **Forensic Vault** | Incident-scoped retention window or legal-hold period. | Fast during incident, policy-controlled afterward. | Segregated forensic storage with restricted access and enhanced logging. | Security/legal approval, immutable access logs, strict retention review. | Attack traces, compromised sessions, break-glass records, raw payloads when justified. |
| **Hash-Only Archive** | Long-lived integrity reference. | Lightweight lookup. | Metadata ledger or content-addressed hash registry. | Lower exposure than payload stores but still access controlled. | Content hashes, chain hashes, artifact IDs, deletion tombstones, proof-of-existence records. |

Evidence indexes should be searchable through authorized metadata views across keys such as `trace_id`, `artifact_id`, `workflow_id`, `prompt_version`, `model_route`, `document_id`, `tool_call_id`, `approval_request_id`, and `final_output_hash`. Raw `tenant_id`, `session_id`, and user identifiers should be avoided in broad indexes; use scoped hashes or access-controlled identity joins.

Storage policy must also support:

| Lifecycle Control | Requirement |
| :---- | :---- |
| **Legal Hold** | Suspends deletion and records authority, scope, and reason. |
| **Deletion Eligibility** | Determines when payloads, metadata, hashes, or references may be purged. |
| **Payload Minimization** | Allows raw payload deletion while preserving hash-bound proof where legally permitted. |
| **Access Logging** | Records every access to sensitive payload references and restricted bundles. |
| **Key Management** | Supports tenant-scoped keys, rotation, revocation, and recovery procedures. |
| **Redaction Reprocessing** | Allows artifacts to be reclassified or re-redacted when policy changes. |
| **Integrity Verification** | Periodically verifies hash chains, signatures, manifests, and storage references. |

## **Cross-Canon Handoff Map**

The Verification Artifacts architecture integrates across the entire AI Systems Engineering Canon because every major subsystem eventually needs evidence for audit, replay, incident response, evaluation, governance, and dispute resolution.

| Target Report ID | Target Report Domain | Verification Artifact Handoff | Dependency / Engineering Integration Rules |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context and State Governance | Context snapshots, memory refs, state hashes, compaction records. | Preserve enough context lineage to prove what was available, included, omitted, or summarized. |
| **AI-ENG-D** | Corpus Engineering | Source provenance, corpus manifests, document version hashes, ingestion records. | Evidence artifacts must bind claims to source lineage and corpus version. |
| **AI-ENG-E** | Retrieval Pipeline | Retrieval snapshots, query hashes, source refs, citation maps, permission filters. | Retrieval evidence must prove authorization, source version, selected chunks, omitted chunks, and citation bindings. |
| **AI-ENG-F** | Freshness and Conflict Detection | Freshness flags, conflict signals, source-age metadata, stale-cache records. | Audit trails must show whether outputs relied on stale, conflicting, or unresolved evidence. |
| **AI-ENG-G** | Model Selection | Model/route manifest, requested route, selected route, fallback reason. | Silent downgrades, provider shifts, and capability changes must be replayable. |
| **AI-ENG-H** | Model Adaptation | Adapter hashes, fine-tune manifests, calibration artifacts, regression records. | Adapted model behavior must be traceable to training/adaptation lineage. |
| **AI-ENG-I** | Regression Control | Baseline artifacts, failed-run bundles, diff records, gate outcomes. | Regression investigations require preserved inputs, outputs, routes, and evaluation artifacts. |
| **AI-ENG-J** | Throughput Mechanics | Token metrics, latency spans, serving-envelope data, queue and batch policy. | Performance regressions must be tied to reproducibility envelopes and serving conditions. |
| **AI-ENG-K** | Weight Dynamics | Quantization/compression manifests, runtime kernel settings, model snapshots. | Optimization-induced behavior changes require route and model-state evidence. |
| **AI-ENG-L** | Serving Architecture | Provider manifest, route manifest, cache state, fallback path, deployment hash. | Serving decisions must be reconstructable from route and environment artifacts. |
| **AI-ENG-M** | Agentic Orchestration | State ledger, plan graph refs, loop counters, branch/retry records. | Agent failures require trajectory evidence without exposing hidden reasoning. |
| **AI-ENG-N** | Tool Contracts | Tool schema versions, payload hashes, authorization records, idempotency hashes. | Tool calls must be auditable from planned call through verified post-action state. |
| **AI-ENG-O** | Action Verification | Pre/post state hashes, action ledger, verification status, compensation refs. | Completion claims require state-backed verification artifacts. |
| **AI-ENG-P** | Multimodal Understanding | Parser manifests, OCR/layout refs, coordinates, media hashes, evidence regions. | Visual/document claims must bind to source coordinates or secure evidence refs. |
| **AI-ENG-Q** | Speech and Realtime Interaction | Transcript hashes, confirmation records, voice fallback state, endpointing metadata. | High-impact voice actions need replayable confirmation and transcript evidence. |
| **AI-ENG-R** | UI Agents | UI state snapshots, DOM/screenshot hashes, action refs, post-action verification. | UI automation must preserve observe-act-verify evidence. |
| **AI-ENG-S** | Production Pathologies | Malformed output records, repair-loop artifacts, false-success traces. | Pathologies require typed evidence for debugging and regression tests. |
| **AI-ENG-T** | Boundary Defense | Tenant hashes, authorization decisions, prompt-injection evidence, redaction records. | Security boundaries must be provable without leaking the protected payload. |
| **AI-ENG-U** | Supply Chain Security | Signed manifests, dependency locks, parser/tool/container hashes. | Evidence trails must bind behavior to approved supply-chain artifacts. |
| **AI-ENG-V** | Resource Abuse | Cost ledgers, token-burn records, retry artifacts, quota state. | Cost bombs and runaway loops need replayable consumption evidence. |
| **AI-ENG-W** | UX Resilience | Degraded-mode state, fallback disclosures, partial-answer records, saved-state refs. | User-visible degraded states must be backed by evidence of preserved state and disclosure. |
| **AI-ENG-X** | User Trust and Transparency | Disclosure records, citation UX state, contestability records, user correction diffs. | Trust claims must be tied to what the user actually saw and could contest. |
| **AI-ENG-Y** | High-Impact Workflow Design | Approval packets, maker/checker records, escalation packages, break-glass artifacts. | High-impact decisions require signed approval, reviewer, and escalation evidence. |
| **AI-ENG-Z** | Strategic Telemetry | Trace IDs, span IDs, token metrics, latency records, semantic drift signals. | Telemetry provides sensor data; verification artifacts preserve durable evidence. |
| **AI-ENG-AA** | Evals Architecture | Golden traces, case versions, labels, rubrics, regression gate artifacts. | Production failures and corrected outputs should become reproducible evaluation artifacts where appropriate. |
| **AI-ENG-AC** | Incident Response and Remediation | Forensic bundles, containment timeline, compromised artifact refs, recovery evidence. | Incidents require evidence chains for scope, timeline, root cause, and remediation. |
| **AI-ENG-AD** | Governance and Compliance | Retention class, policy bundle, approval authority, audit signatures, access logs. | Governance defines capture obligations, retention, deletion, legal hold, and exception approvals. |
| **AI-ENG-AJ** | Reference Architectures | Verification bundle schema, replay package pattern, evidence store, audit ledger. | Reference architectures should include evidence trails as first-class infrastructure. |

## **Durable Principles of Verification Artifacts**

1. **Logs Are Not Evidence Trails**  
   Logs show that something happened somewhere. Verification artifacts preserve enough structured evidence to reconstruct what happened, why it happened, what data shaped it, and what state resulted.

2. **Auditability Requires Scope, Not Hoarding**  
   More raw data is not automatically better auditability. Better auditability comes from scoped hashes, secure references, source versions, policy decisions, state checks, and signed manifests.

3. **Tamper-Evident Does Not Mean True**  
   A signed artifact proves integrity of the recorded evidence. It does not prove the underlying decision was correct unless the evidence, checks, and policies were also adequate.

4. **Replay Has Levels**  
   Exact replay, constrained replay, semantic replay, counterfactual replay, forensic replay, and audit-only verification serve different purposes. Do not promise bit-exact reproduction when the environment cannot support it.

5. **The Reproducibility Unit Is the Whole Runtime**  
   Model ID alone is not enough. Prompt compiler, retrieval index, policy bundle, route manifest, tool schema, feature flags, cache state, and deployment environment all shape behavior.

6. **Hidden Reasoning Is Not an Audit Primitive**  
   Preserve decisions, state transitions, prompts, evidence, policy checks, tool calls, and outputs. Do not require private chain-of-thought capture to explain system behavior.

7. **Tool Claims Require State Evidence**  
   “The model said it sent the invoice” proves nothing. Tool records must preserve authorization, payload hash, idempotency, execution status, and post-action verification.

8. **Sensitive Payloads Need Capture Classes**  
   Never-capture, hash-only, metadata-only, redacted snippet, reference-only, restricted full payload, and incident-mode capture should be explicit policy states.

9. **Evidence Storage Must Follow Risk and Law**  
   Retention windows should be driven by risk class, contract, regulation, incident state, legal hold, and operational need—not by vibes or one universal timer.

10. **Every Artifact Needs Ownership**  
   Prompt records, route manifests, retrieval snapshots, policy checks, approval records, and replay packages need owners, version history, and access rules.

11. **Evidence Must Support Dispute Resolution**  
   A user correction, reviewer override, citation dispute, model downgrade, or failed action should be traceable through artifacts rather than reconstructed from memory and regret.

12. **Verification Infrastructure Is Governance Infrastructure**  
   If the system cannot prove what happened, it cannot be governed. If it cannot be governed, it should not be trusted with consequential autonomy.

#### **Works cited**

1. AI-ENG-AA — Evals Architecture: Ground Truth, Golden Sets & Regression Tests  
2. AI-ENG-Z — Strategic Telemetry - Traces, Tokens, Latency & Semantic Drift  
3. Recon.AI — Trust Accounting Platform for AI systems, accessed June 11, 2026, [https://reconai.net/](https://reconai.net/)  
4. Content-Addressed Evidence Storage & Preservation ... - SEC.gov, accessed June 11, 2026, [https://www.sec.gov/files/ctf-written-fcck-pilot-evidence-02-16-2026.pdf](https://www.sec.gov/files/ctf-written-fcck-pilot-evidence-02-16-2026.pdf)  
5. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
6. Provenance only gets attention after a messy document case : r/AI_Agents - Reddit, accessed June 11, 2026, [https://www.reddit.com/r/AI_Agents/comments/1sco94r/provenance_only_gets_attention_after_a_messy/](https://www.reddit.com/r/AI_Agents/comments/1sco94r/provenance_only_gets_attention_after_a_messy/)  
7. Model Lineage and Reproducibility: Tracking Provenance Across the ML Lifecycle, accessed June 11, 2026, [https://agility-at-scale.com/ai/governance/model-lineage-and-reproducibility/](https://agility-at-scale.com/ai/governance/model-lineage-and-reproducibility/)  
8. Trace concepts - Azure Databricks | Microsoft Learn, accessed June 11, 2026, [https://learn.microsoft.com/en-us/azure/databricks/mlflow3/genai/tracing/tracing-101](https://learn.microsoft.com/en-us/azure/databricks/mlflow3/genai/tracing/tracing-101)  
9. OpenInference Specification - GitHub Pages, accessed June 11, 2026, [https://arize-ai.github.io/openinference/spec/](https://arize-ai.github.io/openinference/spec/)  
10. Trace Concepts | MLflow AI Platform, accessed June 11, 2026, [https://mlflow.org/docs/latest/genai/concepts/trace/](https://mlflow.org/docs/latest/genai/concepts/trace/)  
11. Propagation - Python - OpenTelemetry, accessed June 11, 2026, [https://opentelemetry.io/docs/languages/python/propagation/](https://opentelemetry.io/docs/languages/python/propagation/)  
12. What is a C2PA Manifest? Structure, Assertions, and Verification, accessed June 11, 2026, [https://c2paviewer.com/articles/what-is-c2pa-manifest](https://c2paviewer.com/articles/what-is-c2pa-manifest)  
13. In-toto Attestations in Secure Pipelines - Emergent Mind, accessed June 11, 2026, [https://www.emergentmind.com/topics/in-toto-attestations](https://www.emergentmind.com/topics/in-toto-attestations)  
14. Provenance Driven Identity Trust Architecture, accessed June 11, 2026, [https://identitymanagementinstitute.org/provenance-driven-identity-trust-architecture/](https://identitymanagementinstitute.org/provenance-driven-identity-trust-architecture/)  
15. Technology - Ar.io, accessed June 11, 2026, [https://ar.io/technology/](https://ar.io/technology/)  
16. C2PA Implementation Guidance, accessed June 11, 2026, [https://spec.c2pa.org/specifications/specifications/2.4/guidance/Guidance.html](https://spec.c2pa.org/specifications/specifications/2.4/guidance/Guidance.html)  
17. Blockchain-enforced data lineage architectures with formal verification workflows, accessed June 11, 2026, [https://ijsra.net/sites/default/files/fulltext_pdf/IJSRA-2023-0559.pdf](https://ijsra.net/sites/default/files/fulltext_pdf/IJSRA-2023-0559.pdf)  
18. Defeating Nondeterminism in LLM Inference - Simon Willison's Weblog, accessed June 11, 2026, [https://simonwillison.net/2025/Sep/11/defeating-nondeterminism/](https://simonwillison.net/2025/Sep/11/defeating-nondeterminism/)  
19. Defeating Nondeterminism in LLM Inference - Thinking Machines Lab, accessed June 11, 2026, [https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/)  
20. Defeating Nondeterminism in LLM Inference - OpenAI Developer Community, accessed June 11, 2026, [https://community.openai.com/t/defeating-nondeterminism-in-llm-inference/1358623](https://community.openai.com/t/defeating-nondeterminism-in-llm-inference/1358623)  
21. Openinference Semantic Conventions - Arize AX Docs, accessed June 11, 2026, [https://arize.com/docs/ax/observe/tracing-concepts/openinference-semantic-conventions](https://arize.com/docs/ax/observe/tracing-concepts/openinference-semantic-conventions)  
22. Poisoning Attacks in Federated Learning: An Accountability-Oriented Survey with Centralized Learning as Baseline - Preprints.org, accessed June 11, 2026, [https://www.preprints.org/manuscript/202605.1674](https://www.preprints.org/manuscript/202605.1674)  
23. C2PA for Developers: Implementation Guide, accessed June 11, 2026, [https://c2pa.ai/for-developers](https://c2pa.ai/for-developers)  
24. An 84-Format Numeric Catalog with Bit-Exact Conformance Vectors: A Vendor-Neutral Reference for FP8, BF16, MXFP4, and Microscaling Formats - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2606.09686v1](https://arxiv.org/html/2606.09686v1)  
25. Fighting Misinformation with Authenticated C2PA Provenance Metadata - IPTC, accessed June 11, 2026, [https://iptc.org/std/MediaProvenance/Documents/NAB%20Paper%20-%20Formatted%20Version.pdf](https://iptc.org/std/MediaProvenance/Documents/NAB%20Paper%20-%20Formatted%20Version.pdf)  
26. envelope.md - in-toto/attestation - GitHub, accessed June 11, 2026, [https://github.com/in-toto/attestation/blob/main/spec/v1/envelope.md](https://github.com/in-toto/attestation/blob/main/spec/v1/envelope.md)  
27. mlflow.tracing, accessed June 11, 2026, [https://mlflow.org/docs/latest/api_reference/python_api/mlflow.tracing.html](https://mlflow.org/docs/latest/api_reference/python_api/mlflow.tracing.html)  
28. Attribute Mapping Reference | MLflow AI Platform, accessed June 11, 2026, [https://mlflow.org/docs/latest/genai/tracing/opentelemetry/attribute-mapping/](https://mlflow.org/docs/latest/genai/tracing/opentelemetry/attribute-mapping/)  
29. Agent Trace Evaluation with TruLens Scorers in MLflow, accessed June 11, 2026, [https://mlflow.org/blog/mlflow-trulens-evaluation/](https://mlflow.org/blog/mlflow-trulens-evaluation/)  
30. From Attack Simulation to SIEM Rule: Deterministic Detection-as-Code Synthesis with Probe-Level Traceability - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2606.05252](https://arxiv.org/pdf/2606.05252)  
31. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
32. What do you use for tamper-evident audit logs? Looking for approaches beyond "ship to S3", accessed June 11, 2026, [https://www.reddit.com/r/Observability/comments/1rwm9vh/what_do_you_use_for_tamperevident_audit_logs/](https://www.reddit.com/r/Observability/comments/1rwm9vh/what_do_you_use_for_tamperevident_audit_logs/)  
33. QCIVET: A Quantum–Classical Pipeline Integrity Framework with Contract-Based Subtype Verification and Hash-Chained Audit Traces - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2605.13109v1](https://arxiv.org/html/2605.13109v1)  
34. Cosign Projects - AI Tinkerers - Bogotá, accessed June 11, 2026, [https://bogota.aitinkerers.org/technologies/cosign](https://bogota.aitinkerers.org/technologies/cosign)  
35. What Is Sigstore? Keyless Signing for the Software Supply Chain - Sbomify, accessed June 11, 2026, [https://sbomify.com/2024/08/12/what-is-sigstore/](https://sbomify.com/2024/08/12/what-is-sigstore/)  
36. New SIG proposal: E2E Model Provenance · Issue #40 · ossf/ai-ml-security - GitHub, accessed June 11, 2026, [https://github.com/ossf/ai-ml-security/issues/40](https://github.com/ossf/ai-ml-security/issues/40)  
37. Docker Image signing and attestation with Cosign - AugmentedMind.de, accessed June 11, 2026, [https://www.augmentedmind.de/2025/03/02/docker-image-signing-with-cosign/](https://www.augmentedmind.de/2025/03/02/docker-image-signing-with-cosign/)  
38. Guidance for Artificial Intelligence and Machine Learning - C2PA Specifications, accessed June 11, 2026, [https://spec.c2pa.org/specifications/specifications/2.4/ai-ml/ai_ml.html](https://spec.c2pa.org/specifications/specifications/2.4/ai-ml/ai_ml.html)  
39. A Study on Broker-Assisted Blockchain Trust Chains for Provenance and Integrity Verification of Generative Media Using Watermarking, Semantic Fingerprinting, and C2PA - MDPI, accessed June 11, 2026, [https://www.mdpi.com/2076-3417/16/7/3391](https://www.mdpi.com/2076-3417/16/7/3391)  
40. Evidentia™ T1 — Institutional Evidence Infrastructure, accessed June 11, 2026, [https://evidentiat1.com/](https://evidentiat1.com/)  
41. (PDF) From Policy to Pipeline: A Governance Framework for AI Development and Operations Pipelines - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/399025393_From_Policy_to_Pipeline_A_Governance_Framework_for_AI_Development_and_Operations_Pipelines](https://www.researchgate.net/publication/399025393_From_Policy_to_Pipeline_A_Governance_Framework_for_AI_Development_and_Operations_Pipelines)

---

# Volume 10. AC-AE Operations, Governance, and Lifecycle Management

---

# AI-ENG-AC — AI Operations - Incident Response, Rollback & Runbook Design

## **Doctrinal Foundations: Semantic Incident Response vs. Uptime Operations**

Production reliability in high-dimensional artificial intelligence systems represents a fundamental break from traditional application performance monitoring (APM) and incident management paradigms.1 In legacy web architectures, system health is treated as a binary or scalar state defined by infrastructure metrics: a service is considered healthy when APIs return successful status codes, server workloads are balanced, database connections are active, and latency remains within acceptable thresholds.1 However, when these architectures are driven by probabilistic large language models (LLMs) and multi-agent loops, backend availability does not guarantee a successful, safe, or contextually coherent user transaction.1  
An artificial intelligence gateway can return a successful HTTP 200 payload that contains a complete structural schema failure, an unauthorized tool execution, a cross-tenant data leak, or an ungrounded factual confabulation.1 Conversely, a system can exhibit optimal hardware utilization and high throughput while delivering answers that silently violate safety boundaries, omit critical citations, or trap agentic loops in expensive, infinite executions.1 This mismatch establishes the Green Dashboard Fallacy: the diagnostic error of assuming an AI system is healthy based solely on infrastructure availability.1 In production AI engineering, a green infrastructure dashboard can easily coexist with a red behavioral and meaning-level system state.1  
This gap establishes the systems-engineering discipline of Semantic Incident Response.1 AI operations is defined as the incident-response and lifecycle discipline for systems that fail semantically, behaviorally, economically, procedurally, and socially—not merely infrastructurally.3 The core doctrine of this discipline dictates that AI incidents must be managed as behavioral and semantic failures, not merely service outages. Operational response must identify the failing layer, preserve evidence, contain harm, roll back the smallest effective component, communicate user impact, and convert the incident into durable evaluations, telemetry, policy, and runbook improvements.1  
The primary engineering inquiry moves from "Is the service online?" to "Is the system again producing safe, grounded, authorized, governed, cost-bounded, and user-appropriate behavior?".1 Managing this requires a clean distinction between service recovery and behavioral recovery.3 While service recovery focuses on transport availability, behavioral recovery restores the system's output to safe, grounded, and policy-compliant envelopes.3 Similarly, operators must distinguish mitigation—such as prompt modifications or cache flushes that bypass active symptoms—from permanent resolution via regression-verified code changes promoted through normal CI/CD gates.2  
This distinction is modeled mathematically as a Stackelberg game between the provider and the user to balance operational cost with user utility under degraded conditions.3 Let U_a represent the user utility derived from model action a, let t_a represent the latency delay associated with that action, and let c_a represent the monetary cost of the inference execution to the provider.3 The user's action selection seeks to maximize utility minus the latency penalty, moderated by a sensitivity parameter beta 3:  
max_{a in A} (U_a - beta * t_a)  
The provider, acting as the leader, optimizes its routing policies and interface disclosures to minimize operational service costs (sum of c_i) while maintaining user retention.3 If the interface fails to communicate degradation, the user's utility collapses as they over-rely on a downgraded system.3 Calibrating trust requires the system to dynamically expose its capabilities, limitations, and fallback states, transferring decision-making agency back to the user.3

## **Conceptual Glossary**

The following glossary defines the core operational metrics and terms governing high-dimensional AI operations and semantic incident response:

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **AI Incident** | A situation in which deployed intelligent systems cause, or very nearly cause, real-world harm, semantic degradation, policy violations, or unauthorized actions.4 | Systemic Incident Rate (R_inc) 2 | 0.00% of automated transaction pipelines 2 |
| **Semantic Failure** | An execution where the model output is syntactically valid but factually wrong, ungrounded, contextually irrelevant, or aligned with adversarial instructions.2 | Semantic Violation Rate (R_sem_violation) 2 | < 0.50% of evaluated transactions 2 |
| **Behavioral Recovery** | The operational restoration of a system's output to safe, grounded, and policy-compliant envelopes after a semantic failure, independent of service availability.3 | Mean Time to Behavioral Recovery (MTTBR) | < 5 minutes via automated circuit breakers 6 |
| **Prompt Hotfix** | An emergency, version-controlled override of a system prompt template or safety wrapper to address an active exploit or instruction failure.7 | Hotfix Verification Pass Rate | 100% compliance on automated regression gates 7 |
| **Model Rollback** | Reverting a serving route, adapter, or model checkpoint snapshot to a prior validated version when regressions or failures occur.8 | Revert Transition Latency | < 60 seconds at the gateway layer 8 |
| **Corpus Quarantine** | The physical or logical isolation of document batches, indexes, or vector database partitions suspected of containing poisoned or unauthorized data.2 | Quarantine Activation Delay | < 5 seconds from ingestion alert detection |
| **Retrieval Rollback** | Reverting a vector index, document routing policy, or chunking strategy to a prior verified state to neutralize RAG poisoning.2 | Index Reconstruction Time | < 15 minutes to re-establish clean HNSW graphs |
| **Kill Switch** | A hard, programmatic block managed outside the model's execution path to instantly disable specific agentic tools or capabilities.2 | Kill Switch Execution Latency | < 100 milliseconds via Edge proxy configurations 9 |
| **Feature Flag** | A dynamic, runtime configuration control used to selectively toggle models, prompts, routes, or agent capabilities for specific tenants or cohorts.3 | Flag Propagation Delay | < 500 milliseconds across global CDNs |
| **Blast Radius** | The maximum potential impact of an active failure, measured by affected tenants, transaction volumes, financial exposure, and safety risks.3 | Normalized Tenant Exposure Ratio | Scoped strictly to the failing tenant partition 2 |
| **Degraded-Mode Failure** | A condition where core platform components fail, forcing the system to step down to a restricted but safe quality band.3 | Active Degradation Exposure (D_exp) 3 | < 2.0% of rolling monthly sessions 3 |
| **Containment** | The rapid, temporary execution of blocks, overrides, or fallbacks to limit harm before the definitive root cause is identified.2 | Time to Containment (TTC) | < 2 minutes for critical security events 12 |
| **Mitigation** | Structural adjustments—such as prompt modifications or cache flushes—that bypass active symptoms while permanent code fixes are prepared.3 | Mitigation Success Rate | > 95% reduction in recurring incident triggers |
| **Resolution** | The permanent correction of the underlying system failure, verified by regression tests and promoted through normal CI/CD gates.2 | Regression Verification Coverage | 100% coverage of failure-trigger variants |
| **Incident-to-Eval Loop** | The automated pipeline that packages an incident trace, sanitizes sensitive data, and converts the failure into a permanent evaluation case.1 | Promotion Lead Time | < 24 hours from incident closure to CI/CD gate |

## **AI Incident Taxonomy**

Probabilistic failure modes are highly distributed and cross-cut multiple system layers.1 To facilitate rapid categorization and triage, the platform establishes a multi-dimensional AI Incident Taxonomy.4 Incidents are classified not by their infrastructure symptoms, but by their failing semantic layer and downstream consequences.2 A successful HTTP 200 response code can still be categorized as a critical SEV-1 failure if it delivered unauthorized, harmful, or ungrounded action guidance to the execution environment.1

```text
AI INCIDENT TAXONOMY MAP

[ External / User / Scheduled Input ]
        |
        v
+-------------------------------+
|  Intake and Boundary Layer    |
|  auth | tenant | policy | file |
+---------------+---------------+
                |
                v
+-------------------------------+
|  Data and Evidence Layer      |
|  corpus | retrieval | cache   |
|  freshness | citation support |
+---------------+---------------+
                |
                v
+-------------------------------+
|  Model and Reasoning Layer    |
|  semantic | grounding | prompt |
|  refusal | schema | drift     |
+---------------+---------------+
                |
                v
+-------------------------------+
|  Agent and Action Layer       |
|  tool calls | workflows       |
|  idempotency | verification   |
+---------------+---------------+
                |
                v
+-------------------------------+
|  User / Governance Layer      |
|  disclosure | review | trust  |
|  compliance | communications  |
+---------------+---------------+
                |
                v
+-------------------------------+
|  Operations Response Layer    |
|  detect | contain | recover   |
|  notify | evaluate | prevent  |
+-------------------------------+

Incident classes are assigned by failing layer, consequence, active
propagation, reversibility, and evidence confidence. A green HTTP response
does not imply a healthy AI transaction.
```

The taxonomy maps sixteen distinct classes of system failure across the data ingestion, runtime processing, and interface delivery layers:

| Incident Class | Failing System Layer | Primary Diagnostic Indicator | Operational Symptom | Trace Correlation Attribute |
| :---- | :---- | :---- | :---- | :---- |
| **Semantic** | Cognitive / Inference 1 | nli_grounding_score < 0.70 1 | Confident output of false, ungrounded, or contradictory advice.1 | gen_ai.output.messages 1 |
| **Grounding** | Retrieval Context 1 | Delta_prior > 0.15 2 | Model answers from pre-trained weights, ignoring retrieved documents.2 | gen_ai.usage.input_tokens 1 |
| **Citation** | Document Parser 1 | citation_alignment_score < 0.80 1 | Bracketed citations do not map to verified source page coordinates.2 | document.score 1 |
| **Retrieval** | Index / Search 1 | MRR@k < 0.50 1 | Empty result sets or irrelevant documents returned for queries.1 | retrieval.latency 1 |
| **Corpus** | Ingestion Storage 2 | Ingestion validation failure 2 | Attacker-controlled documents enter index, overriding system prompts.2 | origin_source_type 2 |
| **Tool** | Agent / Executor 1 | json_decode_error 1 | Agent invokes external APIs with malformed, ungrounded, or unsafe parameters.1 | gen_ai.tool.name 1 |
| **Routing** | Gateway Proxy 1 | selected_model mismatch 1 | Silent downgrades shift traffic below the specified model quality floor.3 | routing_reason 1 |
| **Policy** | Guardrail / Filter 2 | policy_violation_rate trigger 2 | Model overblocks benign queries or fails to intercept policy violations.2 | guardrail.id 1 |
| **UI / Trust** | Interface Display 3 | average_approval_ms < 250ms 1 | System hides uncertainty, encouraging uncritical user overreliance.3 | citation_click_rate 1 |
| **Governance** | Compliance / Audit 1 | Signature validation failure 2 | Deployed pipelines execute mutations without valid cryptographic signatures.2 | maker_id / checker_id 1 |
| **Model** | Inference Serving 1 | reconstruction_error spike 1 | Silent degradation in reasoning performance following provider updates.2 | canary_similarity_score 1 |
| **Agent-Loop** | Orchestration 2 | state_repetition_count > 2 1 | Multi-agent pipelines enter infinite planning or formatting repair loops.2 | repair_loop_count 1 |
| **Cost** | Billing / Resource 1 | cost_velocity_usd > limit 1 | High-frequency token consumption or runaway API invocations.2 | usage.cost.total 1 |
| **Security** | Boundary / Access 2 | Unapproved network egress 2 | Compromised MCP tool server executes arbitrary remote commands.2 | rpc.response.status_code 1 |
| **Privacy** | Isolation / Cache 2 | Cross-tenant cache hits 2 | Cross-tenant data leaks or cached state contamination across user boundaries.2 | stale_cache_served 1 |
| **Human-Review** | Operational Queue 1 | review_duration_seconds spike 1 | Expert review queues become saturated, forcing automation defaults.3 | approval_requests.id 1 |

## **AI Severity Model**

AI severity must be consequence-driven, not symptom-driven. The same failure class can be harmless in staging, severe in a regulated production workflow, or catastrophic when it reaches an autonomous tool surface. Severity is therefore assigned by combining harm type, exposure, reversibility, active propagation, regulatory sensitivity, and confidence in detection.

```text
severity = f(
  harm_type,
  affected_population,
  exposure_count,
  active_propagation,
  reversibility,
  security_or_regulatory_sensitivity,
  confidence_of_detection
)
```

Severity assignment should be reviewed when new evidence changes the blast radius or reversibility estimate. Operators should prefer temporary over-classification during uncertainty and downgrade only after containment and scope validation.

| Severity Level | Consequence Envelope | Typical AI Failure Examples | Immediate Response | Recovery / Closure Criteria |
| :---- | :---- | :---- | :---- | :---- |
| **SEV-0** | Active or imminent irreversible harm, uncontrolled high-impact autonomous action, confirmed cross-tenant exposure at broad scope, or severe security compromise with ongoing propagation. | Unauthorized destructive tool execution, confirmed regulated-data leak across tenants, fail-open bypass of core authorization, uncontrolled agentic action against critical systems. | Activate global or scoped kill switch; freeze affected capability; page incident commander, security, privacy, operations, and executive sponsor as applicable. | Harm is contained, affected scope is verified, exposed credentials/artifacts are revoked or quarantined, and recovery route passes high-risk validation. |
| **SEV-1** | Active high-impact harm or high-confidence risk affecting a tenant, workflow, regulated domain, or state-changing tool surface. Harm is serious but containable. | Ungrounded high-impact advice delivered to active users, unauthorized mutation attempt blocked late, tenant-scoped data exposure, poisoned retrieval route influencing production answers. | Isolate affected tenant/route/tool/index; revoke scoped credentials; preserve evidence; switch to safe degraded mode or fail closed. | Blast radius is known, active harm has stopped, affected users/workflows are identified, and compensating or corrective actions are underway. |
| **SEV-2** | Contained semantic, retrieval, model, policy, or tool failure with limited blast radius and no confirmed irreversible external harm. | Citation failures in production, schema drift causing blocked actions, model regression on bounded workflow, repeated tool argument rejection, contained prompt-injection success without side effect. | Disable or downgrade affected route; apply feature flag; route to verified cache, manual review, or bounded degraded mode. | Regression test covers the failure; telemetry confirms no recurrence over the required observation window. |
| **SEV-3** | Quality, latency, cost, or trust degradation visible to limited users but without safety, privacy, or high-impact action exposure. | Model route downgrade disclosed to users, elevated unsupported-claim rate in low-risk domain, parser degradation, stale cache served with proper labeling. | Communicate limitation; preserve state; reduce capability if needed; monitor for escalation. | Service returns to expected behavioral envelope and user-visible degradation is resolved or accepted by policy. |
| **SEV-4** | Internal anomaly, evaluation regression, telemetry drift, suspicious signal, or staging failure with no confirmed user impact. | Canary drift, golden-set failure, evaluation drop, prompt-template compile warning, non-production index anomaly. | Open tracked operations issue; assign owner; block release if policy requires. | Root cause is documented or accepted, and release gates or monitoring are updated if needed. |

Severity should never be determined by a single metric alone. A low grounding score, citation score, cost spike, or routing mismatch is a signal. It becomes an incident severity level only after operators estimate consequence, exposure, active propagation, reversibility, and confidence.

## **Detection and Intake Model**

AI operations requires a structured, multi-channel Detection and Intake Model to route anomalies to the correct SRE responder queues.18 Anomalies are ingested automatically from strategic telemetry monitors, CI/CD evaluation gate failures, or customer-reported support tickets.1

  Strategic Telemetry ───► Anomaly Detector ──┐  
  CI/CD Eval Gates    ───► Regression Gate ──┼──► [Intake Controller] ──► Preserves Verification Artifacts ──► Pages On-Call SRE  
  Support Desk        ───► Escalation Queue ─┘

The intake schema enforces immediate field mapping to preserve the semantic state of the interaction, preventing forensic data loss during the initial triage window:

* **Reporter Identity:** System ID, User ID, or support ticket reference.  
* **Ingestion Timestamp:** Monotonic UTC timestamp of the first detected anomaly.  
* **Affected Workspace ID:** UUID mapping the failure to a specific tenant partition.2  
* **Observed Pathology Class:** Categorized target from the AI Incident Taxonomy.2  
* **Trace Context Identifier:** W3C Trace Parent string and active parent-span ID.1  
* **Active Model Manifest Hash:** Cryptographic signature verifying active weight versions.2  
* **Active Prompt Version:** Semantic version of the compiled template.3  
* **Retrieval Snapshot Path:** Secure reference link pointing to S3-archived document vectors.1  
* **Containment Status Flag:** Boolean tracking whether automated circuit breakers have triggered.2

### **Evidence Preservation During Intake**

Containment actions must not destroy evidence required for scope analysis, replay, attribution, legal review, or regulatory reporting. Before clearing caches, wiping sessions, redeploying containers, rotating credentials, rebuilding indexes, or mutating prompt routes, the intake controller should preserve a minimal tamper-evident incident manifest containing trace identifiers, artifact hashes, active configuration versions, relevant secure payload references, policy decisions, and containment actions. Raw sensitive payloads should not be copied into general incident tickets; they should be stored only through controlled forensic references with access logs and retention policy.

Upon ingestion of a serious or catastrophic incident (SEV-0 or SEV-1), the intake controller issues an automated command to lock the active session, seal related model adapters, snapshot the active index state, and write a tamper-evident reference manifest to the compliance-audit database, preventing operators from clearing caches or redeploying code before forensics are complete.2

## **Semantic Triage Tree**

Semantic triage identifies the failing operational layer before responders choose a containment or rollback surface. The goal is to avoid the two classic incident sins: rolling back the wrong component because it was visible, and destroying forensic evidence because someone panic-clicked “clear cache.” Charming hobbies, both. Not operations.

```text
SEMANTIC TRIAGE TREE

[ Incident Intake ]
        |
        v
[ 1. Is there active safety, privacy, financial, legal, or security harm? ]
        |
        +-- yes --> [ Contain first ]
        |             isolate route/tool/tenant/index/cache
        |             revoke scoped credentials if needed
        |             preserve evidence manifest
        |
        +-- no  --> continue triage
        |
        v
[ 2. Did a state-changing tool or external action execute or possibly execute? ]
        |
        +-- yes --> [ Tool / Action Branch ]
        |             hold unknown state
        |             reconcile source of record
        |             compensate only committed reversible effects
        |
        +-- no  --> continue triage
        |
        v
[ 3. Is retrieved evidence, citation, corpus, or cache state implicated? ]
        |
        +-- yes --> [ Retrieval / Corpus Branch ]
        |             freeze ingestion
        |             quarantine suspect sources
        |             bypass unsafe cache
        |             rebuild or re-rank from verified manifest
        |
        +-- no  --> continue triage
        |
        v
[ 4. Is the model route, adapter, decoding policy, or provider behavior implicated? ]
        |
        +-- yes --> [ Model / Routing Branch ]
        |             pin known-good route
        |             disable suspect adapter
        |             run canary and regression profile
        |
        +-- no  --> continue triage
        |
        v
[ 5. Is prompt, policy, schema, or tool-description behavior implicated? ]
        |
        +-- yes --> [ Prompt / Policy / Contract Branch ]
        |             revert template or policy bundle
        |             apply bounded prompt hotfix if needed
        |             validate against targeted incident evals
        |
        +-- no  --> continue triage
        |
        v
[ 6. Is the failure primarily user-facing disclosure, review, or trust calibration? ]
        |
        +-- yes --> [ UX / Review / Governance Branch ]
        |             disclose limitation
        |             pause high-impact automation
        |             route to review or approval queue
        |
        +-- no  --> [ Unknown / Multi-Layer Branch ]
                      preserve full evidence manifest
                      keep containment active
                      assign cross-functional incident review
```

| Branch | Diagnostic Artifacts | Immediate Containment | Likely Rollback / Recovery Surface |
| :---- | :---- | :---- | :---- |
| **Tool / Action** | Tool trace, authorization decision, idempotency key hash, pre/post state hash, source-of-record status. | Disable affected tool, hold workflow, block blind retry, revoke scoped credentials if compromised. | Tool contract, tool route, credential scope, compensation workflow, idempotency ledger. |
| **Retrieval / Corpus** | Retrieval trace, source IDs, source version hashes, chunk IDs, cache scope, citation verification state. | Freeze ingestion, quarantine suspect documents, bypass unsafe cache, block suspect source lifecycle states. | Corpus lifecycle state, index manifest, chunking policy, retrieval filter, cache key version. |
| **Model / Routing** | Model route, provider profile, model/version hash where available, adapter manifest, decoding config, canary deltas. | Pin known-good route, disable suspect adapter, reduce autonomy, enter disclosed degraded mode. | Model route manifest, adapter binding, decoding profile, provider fallback policy. |
| **Prompt / Policy / Contract** | Rendered prompt reference, prompt version, policy bundle version, schema version, validation errors. | Revert prompt/policy/schema exposure, disable risky prompt path, apply bounded hotfix. | Prompt template, policy bundle, tool description, schema registry, validator. |
| **UX / Review / Governance** | Disclosure event, approval record, reviewer dwell time, escalation packet, user correction signal. | Show status, pause high-impact automation, route to review, block auto-approval. | Review rules, disclosure policy, approval thresholds, UI trust calibration. |
| **Unknown / Multi-Layer** | Full trace graph, incident manifest, secure payload refs, configuration versions, containment actions. | Maintain containment until narrowed; avoid destructive cleanup. | Determined after forensics. |


## **Incident Evidence Package**

The Incident Evidence Package is the forensic bundle used to scope, replay, audit, and repair an AI incident. Raw syslogs are not enough for probabilistic systems, but unrestricted raw prompt dumps are also not acceptable. The package must preserve enough evidence to reconstruct decisions without turning the incident tracker into a second data breach wearing a little lanyard.

```text
INCIDENT EVIDENCE PACKAGE

[ Incident ID ]
      |
      +--> Trace and Span References
      |      trace_id, span_ids, workflow_id, session_hash, tenant_hash
      |
      +--> Runtime Configuration
      |      model route, provider profile, prompt version, policy version,
      |      schema version, tool manifest hash, gateway route manifest
      |
      +--> Evidence and Retrieval State
      |      source IDs, source version hashes, chunk IDs, retrieval IDs,
      |      cache scope, citation coordinates where applicable
      |
      +--> Action and Tool State
      |      tool call ID, payload hash, idempotency key hash,
      |      authorization decision, pre/post state hashes, verification status
      |
      +--> Supply-Chain and Deployment State
      |      artifact hashes, image digest, model/adaptor signatures,
      |      dependency manifest, sandbox profile
      |
      +--> Secure Payload References
      |      access-controlled references to raw prompts, outputs, files,
      |      tool payloads, and sensitive evidence when retention is allowed
      |
      +--> Containment and Recovery Record
             feature flags, kill switches, credential revocations,
             quarantines, rollbacks, compensations, notifications
```

| Evidence Dimension | Preserve By Default | Sensitive Payload Handling | Operational Purpose |
| :---- | :---- | :---- | :---- |
| **Trace Context** | W3C `traceparent`, span IDs, workflow ID, route ID, incident ID. | Safe as metadata if tenant/user identifiers are hashed or opaque. | Reconstruct execution chronology. |
| **Model and Prompt State** | Model route, provider profile, adapter manifest, decoding config, prompt template ID, rendered prompt hash. | Rendered prompt text stored only as secure reference when needed. | Identify model, prompt, or routing regression. |
| **Retrieval State** | Retrieval ID, query hash, source IDs, chunk IDs, source version hashes, citation regions, authorization filter version. | Raw retrieved text stored as secure evidence reference, not in general tickets. | Diagnose grounding, citation, stale-source, or leakage failures. |
| **Tool / Action State** | Tool name/version, schema version, payload hash, idempotency key hash, approval status, pre/post state hashes. | Raw arguments and results stored only in action ledger or forensic vault. | Determine whether action executed, failed, partially committed, or remains unknown. |
| **Policy and Authorization** | Policy bundle version, decision ID, denial/allow class, scoped credential lifetime, approval requirement. | Do not log raw credentials, auth headers, bearer tokens, or private policy secrets. | Prove whether the action was authorized. |
| **Supply-Chain Artifacts** | Container image digest, dependency manifest, model/adaptor hash, signature status, sandbox profile. | Store signature bundles and manifests; avoid copying private keys or secrets. | Prove what artifact executed. |
| **User-Facing State** | Disclosure shown, status message ID, user confirmation state, escalation package ID. | Store user-visible text by secure reference when sensitive. | Determine whether users were properly informed. |
| **Containment Actions** | Flag toggles, kill-switch activation, index quarantine, cache bypass, route rollback, credential revocation. | No secret values; store action IDs and affected scopes. | Prove containment timing and scope. |

### **Evidence Handling Rule**

```text
General incident record:
  hashes, IDs, versions, policy decisions, redacted summaries, secure refs.

Controlled forensic vault:
  raw prompts, raw outputs, source excerpts, uploaded files, tool payloads,
  transaction details, and other sensitive incident evidence.

Never store raw credentials, bearer tokens, private keys, or unrestricted
tenant data in ordinary tickets, logs, or postmortem documents.
```

## **AI Incident Command Role Model**

AI incidents cross model behavior, retrieval, tools, user trust, privacy, security, governance, and infrastructure. Incident response therefore requires role separation. The incident commander owns coordination and severity. Domain owners diagnose and execute bounded changes. Governance, privacy, legal, and communications roles control regulated obligations and external messaging.

```text
AI INCIDENT COMMAND STRUCTURE

                     [ Incident Commander ]
                              |
       +----------------------+----------------------+
       |                      |                      |
[ Technical Response ] [ Risk / Governance ] [ User / Business Response ]
       |                      |                      |
AI Ops Lead            Security Lead          Customer Comms Lead
Model Owner            Privacy Lead           Support Lead
Prompt Owner           Governance Lead        Executive Sponsor
Retrieval Owner        Legal Liaison
Tool/Action Owner
SRE/Platform Lead
Evaluation Lead
```

| Role | Primary Responsibility | Authorized Actions | Escalates When |
| :---- | :---- | :---- | :---- |
| **Incident Commander** | Owns coordination, severity, response cadence, decision log, and closure criteria. | Activates kill switches, assigns owners, approves containment scope, declares behavioral recovery. | SEV-0/SEV-1, unclear ownership, public/regulatory exposure, prolonged containment. |
| **AI Operations Lead** | Owns gateway routing, fallback, model/provider route health, and degraded-mode operation. | Pins routes, changes traffic splits, disables unsafe model paths, coordinates with SRE. | Routing changes affect quality floor, safety, privacy, or broad user cohorts. |
| **SRE / Platform Lead** | Owns serving infrastructure, queues, GPU capacity, deployment health, and runtime stability. | Scales serving pools, isolates pods/nodes, rolls back deployment images, enforces circuit breakers. | Infrastructure failure contributes to semantic or user-visible incident. |
| **Model Owner** | Owns model snapshots, provider profiles, adapters, quantization configs, and decoding profiles. | Disables adapters, pins model snapshot, changes approved decoding profile, initiates model regression evaluation. | Model behavior drift, provider update, adapter failure, or route quality regression is suspected. |
| **Prompt Owner** | Owns system prompts, tool descriptions, safety wrappers, and prompt registry changes. | Reverts prompt versions, proposes emergency hotfix, validates placeholder and policy invariants. | Prompt injection, instruction loss, schema drift, or template regression is implicated. |
| **Retrieval / Corpus Owner** | Owns corpus lifecycle, chunking, embeddings, vector indexes, source authority, and cache eligibility. | Freezes ingestion, quarantines sources, rebuilds index from manifest, updates retrieval eligibility state. | Poisoning, stale evidence, citation failure, or cross-tenant retrieval is suspected. |
| **Tool / Action Owner** | Owns tool contracts, scoped credentials, idempotency, action ledgers, and post-action verification. | Revokes tool credentials, disables tool route, blocks blind retry, initiates reconciliation or compensation. | State-changing action is executed, unknown, duplicated, unauthorized, or misreported. |
| **Security Lead** | Owns prompt-injection compromise, SSRF, RCE, egress, sandbox, secret leakage, and adversarial activity. | Blocks egress, isolates runtimes, rotates exposed credentials, directs forensic capture. | Boundary violation, malicious artifact, unauthorized tool use, or active exploit is detected. |
| **Privacy Lead** | Owns PII/PHI exposure, tenant leakage, cache contamination, and privacy-impact assessment. | Freezes affected data scopes, requires secure evidence handling, coordinates affected-scope analysis. | Personal, regulated, or tenant-private data may have been exposed. |
| **Governance / Compliance Lead** | Owns policy obligations, regulated incident thresholds, audit artifacts, and required reporting workflows. | Determines governance review path, coordinates regulatory reporting where legally required. | Incident may trigger contractual, regulatory, audit, or internal governance obligations. |
| **Legal Liaison** | Advises on liability, privilege, preservation duties, notification risk, and external legal exposure. | Recommends legal hold, reviews communications, coordinates with counsel. | Potential harm, regulated disclosure, litigation risk, or contractual breach is present. |
| **Customer Communications Lead** | Owns status page, customer-facing language, support scripts, and disclosure consistency. | Publishes approved updates, coordinates user notices, avoids unverified claims. | User-visible degradation, breach notification, SLA issue, or public-facing incident occurs. |
| **Support Lead** | Owns ticket routing, user reports, support scripts, and frontline escalation. | Escalates patterns to IC, updates support guidance, coordinates user-level remediation. | Support tickets spike, users report false success, or manual recovery is needed. |
| **Evaluation Lead** | Owns incident-to-eval promotion, regression reproduction, golden cases, and release gates. | Converts sanitized incident traces into test cases, blocks release on unresolved regression. | Incident class lacks adequate eval coverage or repair needs release verification. |
| **Executive Sponsor** | Owns executive decision-making for catastrophic public, regulatory, financial, or safety incidents. | Approves major public posture, business-continuity actions, and broad shutdown decisions. | Incident materially affects customers, regulators, revenue, safety, or public trust. |

## **Containment Strategy**

Containment means stopping active harm before the definitive root cause is identified.2 Probabilistic systems require a graduated containment progression to limit the blast radius while preserving task continuity for unaffected enterprise tenants 2:

```
  Incident Ingested ──► Map blast radius to session contexts  
                             │  
                             ▼  
  Local Containment  ──► Tenant Partition / Isolate Namespace   
                             │  
                             ▼  
  Scoped Containment ──► Toggle Flag / Revoke Tool Scope   
                             │  
                             ▼  
  System Failover    ──► Route Fallback / Fail-Closed 
```

### **1. Identify and Scope the Failure**

Upon alert ingestion, the responder identifies the smallest affected perimeter.2 If a failure impacts only a single workspace, the responder isolates that tenant's namespace inside pgvector and Redis caches, preventing global service disruption.2

### **2. Isolate via Capability Kill Switches**

If the failure involves agentic tool execution (such as recursive database writes), the responder toggles the corresponding feature flag, disabling write actions while preserving read-only informational capability.2

### **3. Route to Safe Degraded Modes**

If cognitive performance has degraded globally, the gateway proxy intercepts downstream requests, shifting traffic to smaller, cached, or local model configurations.3 The system displays a plain-language disclosure, informing the user that advanced analytical capabilities are temporarily restricted.3

### **4. Fail-Closed Enforcement**

If containment paths cannot satisfy the system's defined safety, privacy, or compliance floors, the transaction is terminated.2 All active session credentials are revoked, in-flight database transactions are rolled back, and the system freezes safely to prevent cascading failures.2

## **Feature Flag and Kill Switch Matrix**

Feature flags and kill switches must operate strictly outside the model's cognitive boundary, utilizing centralized gateway routing weights or isolated container-level network proxies to enforce boundaries.2

```
  Requests ──► Filter checks (OIDC JWT verified)  
                     │  
                     ├─► FF_MODEL_ROUTE ( claude-3-5-sonnet -> claude-3-5-haiku )   
                     ├─► FF_TOOL_REVOKE ( Revokes OAuth token inside vault )   
                     └─► FF_INDEX_FREEZE ( Sets HNSW partition to read-only ) 
```

The Feature Flag and Kill Switch Matrix outlines the technical implementation, operational check, and restoration procedure for each control:

| Switch Identifier | Scope of Control | Underlying Code Mechanism | Access Control Rule (RBAC) | Standard Restoration Criteria |
| :---- | :---- | :---- | :---- | :---- |
| **FF_GLOBAL_AI** | Global Platform | Gateway proxy routing configuration toggle.2 | Platform Administrator | 100% pass on CI/CD validation gates.1 |
| **FF_TENANT_BLOCK** | Specific Workspace | Redis key block; database RLS filter constraint.2 | Incident Commander 17 | Security compliance sign-off on tenant files.2 |
| **FF_TOOL_REVOKE** | Specific Agent API | OAuth token revocation inside Secrets Manager.2 | Tool Owner | API status matches baseline schemas.2 |
| **FF_MODEL_ROUTE** | Serving Endpoint | Gateway routing weight redistribution.3 | AI Operations Lead | 10-run canary suite perplexity matches baseline.2 |
| **FF_INDEX_FREEZE** | Document Indexing | Ingestion worker write lock; HNSW read-only flag.2 | Retrieval Owner | HubScan robust z-score returns to normal (less than 5.0).2 |
| **FF_CACHE_BYPASS** | Semantic Memory | Redis cache lookup bypass configuration.2 | SRE On-Call Engineer | Cache keys cleared; TTL parameters updated.2 |
| **FF_AGENT_DRAFT** | Agent Loop Autonomy | Blocks auto-send; forces read-only draft views.2 | Prompt Owner | Trajectory evaluation pass rate > 95%.1 |

## **Rollback Surface Map**

Rollback in AI systems is multi-surface. Reverting the model while leaving a poisoned index, stale cache, incompatible prompt, or drifted tool schema active can create a new incident. Operators must identify the smallest effective rollback surface and validate all coupled artifacts before restoring traffic.

```text
ROLLBACK SURFACE MAP

[ User-visible or telemetry-detected failure ]
        |
        v
[ Identify failing surface ]
        |
        +--> prompt template / tool description
        +--> model route / provider profile / adapter
        +--> retrieval index / corpus lifecycle state
        +--> policy bundle / authorization rule
        +--> tool contract / action gateway
        +--> cache key scope / semantic cache
        +--> UI control / disclosure / automation locator
        +--> deployment image / dependency / sandbox profile
        |
        v
[ Preserve evidence manifest ]
        |
        v
[ Apply smallest safe rollback or containment ]
        |
        v
[ Run surface-specific validation ]
        |
        v
[ Restore traffic gradually or remain degraded/fail-closed ]
```

| Rollback Surface | Mechanical Trigger | Evidence to Capture Before Change | Validation Before Restoration | Common Failure Mode |
| :---- | :---- | :---- | :---- | :---- |
| **Prompt Template / Tool Description** | Reassign active prompt/template tag to previous approved version. | Prompt ID, rendered prompt hash, template version, variables, failing output hash. | Prompt compile check, safety/policy invariants, targeted incident evals. | Placeholder mismatch, instruction regression, hidden tool-description drift. |
| **Model Route / Provider Profile** | Pin gateway route to previous approved model/provider snapshot or serving profile. | Route ID, provider response metadata, model snapshot/hash where available, decoding config. | Canary suite, schema pass rate, grounding checks, latency/cost profile. | Capability loss, context-window mismatch, provider SDK incompatibility. |
| **Adapter / LoRA / Fine-Tune** | Disable suspect adapter or revert to previous adapter manifest. | Adapter hash, parent model ID, tenant binding, training dataset lineage. | Tenant-scoped regression suite and behavior comparison against base route. | Adapter dimension mismatch, cross-tenant adapter exposure, quality regression. |
| **Retrieval Index** | Restore index from verified manifest or rebuild from authorized source records. | Source IDs, chunk IDs, embedding model version, index manifest, query traces. | Retrieval MRR/recall checks, authorization checks, citation verification. | Missing vectors, stale source lifecycle state, HNSW rebuild delay. |
| **Corpus Lifecycle State** | Mark suspect source/chunk/vector as quarantined, revoked, stale, or inactive. | Source version hash, ingestion batch ID, parser version, transformation history. | Retrieval eligibility tests and source-authority checks. | Quarantine bypass through cache or derived summary. |
| **Policy Bundle** | Revert OPA/policy-as-code bundle or authorization rules. | Policy version, decision logs, rule IDs, affected scopes. | Policy compile check, adversarial tests, false-positive/false-negative smoke tests. | Overblocking, underblocking, mismatch between UI disclosure and policy. |
| **Tool Contract / Action Gateway** | Revert tool schema, MCP manifest, OpenAPI contract, or wrapper logic. | Tool manifest hash, payload hash, validation errors, authorization decision. | Dry-run validation, idempotency test, post-action verification test. | Downstream API changed; old schema no longer matches target service. |
| **Semantic / Prefix Cache** | Bypass, invalidate, or version cache key scope. | Cache key hashes, tenant/session scope, source/policy/model versions. | Scope-isolation test, freshness test, live retrieval confirmation. | Thundering-herd load, stale cache re-entry, cross-scope key reuse. |
| **UI / Trust Control** | Revert disclosure labels, approval gate behavior, browser locators, or citation rendering. | Screenshot hash, DOM snapshot hash, disclosure state, click/action logs. | UI regression test, disclosure check, automation re-observation test. | Silent downgrade, incorrect confidence, blind-click automation. |
| **Deployment Image / Runtime** | Revert container image, dependency lockfile, parser sandbox, or serving image. | Image digest, SBOM/AI-BOM refs, sandbox profile, CVE/signature status. | Vulnerability scan, smoke test, sandbox policy test, health + behavior canary. | Reintroducing vulnerable dependency or breaking model-loader compatibility. |

## **Prompt Hotfix Protocol**

Prompt hotfixes are emergency mitigations, not magic patches. They may reduce active harm while a durable fix is prepared, but they must be versioned, reviewed, tested, and retired or promoted through the normal release process. A prompt hotfix that cannot be audited is just production improv with a YAML costume.

```text
PROMPT HOTFIX FLOW

[ Active prompt-related incident ]
        |
        v
[ Create incident-bound hotfix branch ]
        |
        v
[ Preserve failing trace + rendered prompt reference ]
        |
        v
[ Apply minimal prompt/template/tool-description change ]
        |
        v
[ Pre-flight validation ]
  placeholders | token budget | schema references | policy invariants
        |
        v
[ Risk-tiered eval gate ]
  targeted incident cases | safety probes | schema checks | grounding checks
        |
        v
[ Controlled exposure ]
  canary, tenant-scoped route, or internal-only validation
        |
        v
[ Promote, revise, or revert ]
        |
        v
[ Add incident case to permanent evals ]
```

| Gate | Required Check | Failure Handling |
| :---- | :---- | :---- |
| **Incident Binding** | Hotfix branch, prompt version, and deployment record reference the incident ID. | Block deployment if change is not traceable to incident. |
| **Minimality Review** | Change addresses the active failure without broad unrelated behavior changes. | Split unrelated improvements into normal release path. |
| **Template Integrity** | Variables, tool names, delimiters, schemas, and context-budget assumptions remain valid. | Reject hotfix until compiler and schema checks pass. |
| **Policy Invariants** | Safety, privacy, authority hierarchy, and tool-boundary instructions are not weakened. | Fail closed; route to policy owner or security lead. |
| **Targeted Regression** | Incident-triggering case and related variants pass. | Keep containment active; revise or roll back. |
| **Canary / Exposure Control** | Exposure is bounded by risk tier, tenant scope, and rollback readiness. | Disable route if metrics degrade or unexpected behavior appears. |
| **Promotion / Retirement** | Hotfix is either promoted into a normal release with tests or retired after durable fix. | Emergency-only prompt changes cannot remain unowned indefinitely. |

Specific thresholds such as number of eval cases, canary percentage, and observation window should be set by risk tier and deployment profile. High-impact workflows require stricter approval and lower exposure than low-risk conversational formatting fixes.

## **Model Rollback Model**

Model rollback restores a prior approved serving behavior by changing the model route, provider profile, adapter binding, decoding profile, or serving image. It should be executed at the gateway or serving-control layer so downstream applications do not hardcode emergency model logic.

```text
MODEL ROLLBACK MODEL

[ Drift / regression / provider incident detected ]
        |
        v
[ Freeze route manifest and preserve evidence ]
        |
        v
[ Identify rollback target ]
  previous provider profile
  approved model snapshot
  prior adapter manifest
  conservative decoding profile
  last stable serving image
        |
        v
[ Route traffic to rollback target ]
        |
        v
[ Run validation suite ]
  schema | grounding | policy | tool compatibility | latency | cost
        |
        v
[ Restore gradually or remain degraded/fail-closed ]
```

| Rollback Dimension | What Changes | Validation Required | Common Risk |
| :---- | :---- | :---- | :---- |
| **Provider Route** | Gateway selects previous approved provider/model profile. | Capability floor, safety profile, context window, schema support. | Silent downgrade below task requirements. |
| **Model Snapshot** | Self-hosted or registry-managed model reverts to prior signed snapshot. | Signature/hash check, canary behavior, latency, memory footprint. | Snapshot incompatible with current tokenizer, adapter, or prompt. |
| **Adapter / LoRA** | Suspect adapter is disabled or reverted to prior manifest. | Parent-model compatibility, tenant binding, targeted task eval. | Quality drop or accidental cross-tenant adapter exposure. |
| **Decoding Profile** | Temperature, top-p, max tokens, constrained decoding, or structured-output mode changes. | Schema pass rate, semantic quality, refusal behavior, output length. | Overcorrection causing refusals, truncation, or loss of usefulness. |
| **Serving Image** | Runtime container, model loader, dependency set, or quantization profile changes. | Smoke test, model-load verification, CVE/signature check, performance envelope. | Reintroducing vulnerable loaders or incompatible kernels. |
| **Prompt Coupling** | Prompt route is pinned to a compatible version for rollback route. | Prompt-template compile, tool description compatibility, context budget. | Old model receives prompt format it cannot follow. |

Rollback closure requires behavioral validation, not merely successful deployment. A route is recovered only when task-level outputs return to the approved envelope for grounding, schema, policy, latency, tool compatibility, and user-facing disclosure.


## **Corpus Quarantine and Retrieval Rollback Model**

Retrieval incidents are contained by changing source eligibility, not by trusting the model to ignore bad evidence. Poisoned, stale, unauthorized, or anomalous documents must be removed from the retrieval candidate set through database-enforced lifecycle state, tenant scope, source authority, and cache invalidation.

```text
CORPUS QUARANTINE FLOW

[ Retrieval anomaly detected ]
  hubness | stale source | cross-tenant hit | citation mismatch | user report
        |
        v
[ Preserve retrieval evidence ]
  retrieval_id, query_hash, source IDs, chunk IDs, cache scope, index manifest
        |
        v
[ Mark affected artifacts ]
  source_lifecycle_state = quarantined / revoked / stale / pending_review
        |
        v
[ Enforce retrieval eligibility ]
  tenant scope + ACL + source state + policy version + cache scope
        |
        v
[ Freeze affected ingestion path ]
        |
        v
[ Serve safe fallback ]
  lexical search | verified cache | partial answer | review | fail closed
        |
        v
[ Rebuild or repair index from verified manifest ]
```

| Control | Purpose | Implementation Pattern |
| :---- | :---- | :---- |
| **Source Lifecycle State** | Blocks suspect sources and derived chunks from active retrieval. | Store lifecycle state such as `active`, `pending_review`, `quarantined`, `revoked`, or `stale`; retrieval queries admit only eligible states. |
| **Tenant and ACL Enforcement** | Prevents cross-tenant or unauthorized retrieval during incident response. | Enforce DB-level RLS or equivalent authorization before candidate scoring. |
| **Quarantine Registry** | Provides fast emergency exclusion without ad hoc SQL string injection. | Maintain tenant-scoped denylist/quarantine table joined by source ID, chunk ID, or vector ID. |
| **Cache Invalidation / Bypass** | Prevents quarantined evidence from reappearing through stale cache. | Version cache keys by source, policy, tenant, and retrieval manifest; bypass or invalidate affected scopes. |
| **Index Rebuild Manifest** | Restores index topology from known-good source/chunk/vector versions. | Rebuild affected partition from signed or approved manifest; validate embedding model and dimensions. |
| **Fallback Retrieval** | Preserves limited usability during vector repair. | Use lexical/BM25 search, verified cache, partial answer, or manual review depending on risk. |

Hubness, citation mismatch, and retrieval anomalies should be treated as triage signals. A high robust z-score or unusual hit frequency does not prove malicious poisoning by itself; it justifies quarantine, review, and replay against representative query sets.


## **Tool and Action Containment Model**

Tool incidents are dangerous because probabilistic planning crosses into deterministic side effects. The operational rule is simple: never let conversational confidence outrun verified action state.

```text
TOOL AND ACTION CONTAINMENT

[ Proposed Action ]
        |
        v
[ Validate Payload ]
  schema | semantic rules | policy | tenant/user scope
        |
        v
[ Authorize Action ]
  scoped credential | approval state | risk class
        |
        v
[ Create Idempotency Record ]
  request hash | action ID | actor | target | timestamp
        |
        v
[ Capture Pre-Action State Hash ]
        |
        v
[ Execute or Block ]
        |
        v
[ Observe Tool Result ]
        |
        v
[ Verify Source of Record ]
        |
        +--> verified success  -> report success
        +--> verified failure  -> report failure / preserve draft
        +--> unknown state     -> hold and reconcile
        +--> partial commit    -> compensate or forward recover
```

| Incident State | Meaning | Correct Operational Response | User-Facing Status |
| :---- | :---- | :---- | :---- |
| **Blocked Before Execution** | Payload, policy, approval, or authorization check failed. | Do not call tool; preserve draft and validation error. | “Not submitted. This action was blocked before execution.” |
| **Execution Failed Before Commit** | Tool rejected or failed before external state changed. | Preserve error and allow safe correction or retry if preconditions still hold. | “The action failed before completion.” |
| **Unknown After Attempt** | Timeout, lost response, or ambiguous observation after a state-changing call. | Hold workflow; reconcile with source of record before retry. | “Status unknown. We are verifying before allowing another attempt.” |
| **Partial Commit** | Some sub-actions committed while others failed. | Stop dependent actions; compensate reversible steps or forward recover if safer. | “Some steps completed; remaining steps are paused for verification.” |
| **Committed Reversible Effect** | External state changed and compensation is supported. | Execute approved compensation workflow if reversal is required. | “The action completed and is being reversed through a tracked correction.” |
| **Committed Irreversible Effect** | Pivot transaction succeeded and cannot be programmatically undone. | Stop further automation; notify responsible owners; begin remediation and communication. | “The action completed and requires manual review/remediation.” |
| **False Success Claim** | UI/model claimed completion before verification. | Correct status, preserve claim evidence, open incident if user impact exists. | “Correction: the previous completion status was not verified.” |

Rollback applies only inside a live transaction boundary. Once a side effect has committed, operators should use compensation, forward recovery, reconciliation, or manual remediation depending on the transaction state.

## **Cache, Cost, and Resource Incident Model**

Resource incidents occur when model calls, retries, cache behavior, retrieval fan-out, tool loops, or agentic planning consume capacity faster than the system can safely serve users. These incidents can cause denial-of-wallet, latency collapse, stale-cache exposure, and degraded user trust.

```text
RESOURCE INCIDENT CONTROL LOOP

[ Active workflow ]
        |
        v
[ Measure resource state ]
  tokens | tool calls | retries | latency | queue depth | spend | cache scope
        |
        v
[ Compare against profile budget ]
  tenant | session | workflow | risk tier | route
        |
        +-- within budget --> continue
        |
        +-- warning band --> degrade, queue, prune, or ask user
        |
        +-- hard limit --> halt, preserve state, fail closed or escalate
```

| Resource Failure | Detection Signal | Containment | Recovery |
| :---- | :---- | :---- | :---- |
| **Runaway Agent Loop** | Repeated state hash, tool repetition, growing turn count, no-progress plan diffs. | Halt workflow, preserve state, block further tool calls. | Replan from verified state or escalate. |
| **Repair Loop** | Repeated schema/format failures with similar payloads. | Stop bounded repair after profile limit. | Return structured failure or route to human review. |
| **Retry Storm** | High retry count, queue depth spike, provider 429/5xx, synchronized backoff failure. | Circuit-break route; apply jitter/backoff; shed or queue traffic. | Restore gradually after provider/gateway health recovers. |
| **Token / Spend Spike** | Cost velocity exceeds tenant/workflow budget band. | Freeze expensive route, reduce context, require approval, or fail closed. | Tune prompt/context, adjust budgets, add loop regression test. |
| **Cache Scope Violation** | Cache hit with tenant/user/policy/source mismatch. | Disable affected cache scope; revoke exposed sessions if needed. | Rebuild cache namespace and add isolation test. |
| **Stale Cache Overuse** | Cache served after source/policy/model version change. | Bypass or invalidate stale keys; disclose limitation if served safely. | Version cache by source, policy, model, prompt, and risk profile. |
| **Prefill / Context Pressure** | Long prompt, rising TTFT, low evidence density, context overflow. | Prune low-priority context; summarize safely; block if evidence lost. | Improve retrieval selection and context compiler policy. |

Budget values should be deployment-profile specific. A toy workflow, enterprise analysis route, and regulated high-impact action should not share the same hard-coded token, cost, turn, or tool-call ceiling.

## **User Communication Matrix**

AI incident communication must be truthful about state, uncertainty, user impact, and safe next actions. Do not say “the AI hallucinated” as if the platform were a haunted toaster. Say what failed, what is saved, what is blocked, what is known, what remains unknown, and what the user can safely do.

```text
COMMUNICATION RULE

Never claim:
  - an action succeeded before verification,
  - no data was exposed before scope analysis,
  - a cache/index/model is clean before validation,
  - a degraded route is equivalent when it is not.

Communicate:
  known status, preserved state, active limitation,
  user action required, and next update path.
```

| Incident Class | Audience | What to Say | What Not to Say | User / Customer Action |
| :---- | :---- | :---- | :---- | :---- |
| **RAG Poisoning / Retrieval Quarantine** | Affected workspace users or admins. | “We detected an anomaly in part of your document retrieval index and isolated the affected sources. Search may be incomplete while verification runs.” | “All results are safe” before validation completes. | None unless asked to review quarantined documents. |
| **Model Route Degradation** | Active users on affected route. | “The primary reasoning route is temporarily unavailable. We can continue in a reduced mode with limitations, or wait for full capability.” | “Equivalent quality” if capability, citation depth, latency, or tools changed. | Choose reduced mode, wait, or save progress. |
| **State-Changing Tool Timeout** | Specific initiator / admin. | “We could not verify the action status. To prevent duplicate effects, we paused retries and saved the request for verification.” | “It failed” or “It succeeded” before source-of-record reconciliation. | Review status or wait for verification. |
| **Cross-Tenant / Privacy Investigation** | Security/admin contacts and affected users as policy requires. | “We identified a possible isolation issue and have contained the affected scope. Investigation and verification are ongoing.” | “No unauthorized access occurred” before confirmed. | Reauthenticate or rotate credentials if instructed. |
| **Prompt Injection / Security Containment** | Affected session user/admin. | “This session was paused because a security boundary was triggered. Your progress is saved, and unsafe automation is disabled.” | Revealing detection logic, system prompts, or exploit details. | Resume after review or start a clean session. |
| **Cost / Quota Halt** | User or tenant admin. | “This workflow reached its configured budget or loop limit. Progress is saved, and further automated execution is paused.” | “The system crashed” if the stop was intentional containment. | Approve more budget, simplify task, or resume manually. |
| **Parser / Citation Degradation** | User relying on documents. | “We could not verify the document layout/citation coordinates. We can provide text-only or partial results with limitations.” | Pretending visual/table evidence was inspected when it was not. | Upload cleaner file, accept text-only mode, or request review. |
| **Fail-Closed Security Block** | Affected users / status page if broad. | “We temporarily blocked this capability to preserve security and data integrity. Saved state is preserved where possible.” | “Scheduled maintenance” if the event is an active incident requiring transparency. | Wait for recovery path or contact support. |

## **Incident Review and Postmortem Model**

An AI postmortem must look beyond traditional server uptime metrics, conducting a thorough analysis of prompt templates, retrieved evidence chunks, and model configurations.20

```
  ┌────────────────────────────────────────────────────────┐  
  │                 AI POSTMORTEM SCHEMA                   │  
  ├────────────────────────────────────────────────────────┤  
  │  - Incident Summary: Impact, Core Failing Layer        │  
  │  - Root Cause: Data vs. Prompt vs. Weight Drift       │  
  │  - Chronology: Trace-linked Ingestion to Failure Path │  
  │  - Technical Evidence: Rendered Prompts & Raw Outputs │  
  │  - Evaluation Gaps: Missed Canary or Golden Sets      │  
  │  - Action Items: Preventative CI/CD Gates Added       │  
  └────────────────────────────────────────────────────────┘
```

The postmortem process is structured to systematically catalog failure metrics, trace the chronological progression, and implement permanent behavioral guards:

* **Incident Summary & Failing Layer:** Captures the severity level, blast radius, total duration, and affected tenant UUIDs, isolating the failure to its specific taxonomic class.2  
* **Root Cause & Contributing Factors:** Analyzes whether the failure originated from data drift, prompt instruction decay, or provider-side weight updates.2  
* **Trace-Linked Chronology:** Details the exact timeline of events, linking key milestones (anomaly detection, containment trigger, rollback execution) to specific OpenTelemetry trace and span IDs.1  
* **Technical Evidence Repository:** Attaches the rendered prompt string, the retrieved context chunks, and the raw generated output tokens directly to the postmortem file, enabling exact reproduction.20  
* **Evaluation & Telemetry Gaps:** Highlights why the failure escaped containment, identifying missing canary prompts or gaps in the CI/CD evaluation harness.1  
* **Permanent Improvements List:** Replaces vague "be more careful" actions with concrete, dated, and testable improvements.2 Every postmortem must commit at least one new golden test case to the regression set and one new alert threshold to Prometheus.1

## **Runbook Library**

The platform maintains a runbook library for recurring AI incident classes. Each runbook must identify the trigger, severity assumptions, first actions, containment controls, evidence to preserve, owner roles, recovery path, validation steps, communication path, and incident-to-eval follow-through.

Runbooks should not instruct responders to paste raw prompts, secrets, transaction details, or tenant data into ordinary incident tickets. Sensitive payloads belong in controlled forensic storage with secure references, access logs, and retention policy.

```text
AI OPERATIONS RUNBOOK LIBRARY

[ Incident Trigger ]
        |
        v
[ Classify ]
  semantic | retrieval | prompt | model | tool | cost | privacy | UX/review
        |
        v
[ Preserve Evidence Manifest ]
  trace IDs | hashes | secure refs | configs | state versions
        |
        v
[ Contain ]
  flag | kill switch | quarantine | route pin | credential revoke
        |
        v
[ Recover ]
  rollback | compensate | reconcile | rebuild | degrade | escalate
        |
        v
[ Validate ]
  canary | regression | policy | isolation | source-of-record check
        |
        v
[ Communicate ]
  known status | unknowns | saved state | user action | next update path
        |
        v
[ Promote Incident to Eval / Runbook Update ]
```

### **Runbook 1: Ungrounded Answer Incident (Factual Hallucination)**

* **Incident Trigger & Severity:** Automated NLI grounding checks detect a business_rule_error on an active session; nli_grounding_score falls below 0.70 over a 5-minute rolling window. SEV-2 (Contained Semantic Failure).1  
* **First 5 Minutes Actions:**  
  1. Trace the session ID to locate the active retrieval spans and the model endpoint version.1  
  2. Capture the rendered prompt string, the retrieved context chunks, and the raw output tokens.20  
  3. Toggle the FF_CACHE_BYPASS flag for the affected workspace, forcing live database lookups and bypassing potentially corrupted caches.3  
* **Containment Steps:** Force-disable advanced rag-generation routes for the tenant. Set the active trust-calibration interface to Level 3 (Inline Caveat), rendering visible uncertainty indicators for unsupported or unverified claims to communicate uncertainty.3  
* **Evidence to Preserve:** Snapshot the current vector index state, export the retrieved chunk text strings, and write the complete telemetry trace payload to the forensic directory.1  
* **Owner Roles:** SRE/Platform Lead (Primary investigator); Retrieval Owner (Context validator); Prompt Owner (Hotfix engineer).17  
* **Rollback Steps:** Revert the retrieval query rewriting template inside the prompt registry to the preceding stable commit.2  
* **Validation Steps:** Execute a parallel 50-case grounding evaluation run in staging, asserting that the model's factual outputs align with the reference documents.1  
* **Communication Path:** Render an inline caveat badge: *"I can answer conceptually, but my live retrieval database is currently optimizing. I cannot verify specific policy changes."*.3  
* **Postmortem Actions:** Convert the hallucinated query-response pair into a permanent, version-controlled golden test case inside the CI/CD pipeline to prevent future regressions.1

### **Runbook 2: Citation Failure Incident**

* **Incident Trigger & Severity:** UI monitors detect a citation_alignment_score falling below 0.75, indicating that bracketed citation markers do not map to verified coordinates inside the source documents. SEV-3 (Degraded Quality).1  
* **First 5 Minutes Actions:**  
  1. Scan the document parsing logs to check for layout-aware parsing exceptions or OCR failures.1  
  2. Verify that page coordinates exist inside the retrieved metadata schemas.1  
  3. Route document parsing tasks to the backup heuristic text parser.3  
* **Containment Steps:** Temporarily hide visual coordinate highlights on the user interface, falling back to displaying plain-text document titles.3  
* **Evidence to Preserve:** Snapshot the failing document file, the extracted DocTags markup, and the page coordinate array.21  
* **Owner Roles:** Layout & Document Engineer (Primary investigator); SRE/Platform Lead (Inference monitor).17  
* **Rollback Steps:** Revert the document parsing Docker container to the preceding stable image version.2  
* **Validation Steps:** Run an automated unit test suite, executing a coordinate-level IoU check over table extraction targets.1  
* **Communication Path:** Display a subtle UI warning card: *"Complex page layout unreadable. Using text-only reference mode."*.3  
* **Postmortem Actions:** Update the parser's configuration schema to enforce profile-appropriate rasterization and layout-extraction settings for scanned PDFs.21

### **Runbook 3: Prompt Injection Incident (Safety Breach)**

* **Incident Trigger & Severity:** Input Prompt Shields log an adversarial bypass attempt; policy_violation_rate spikes. SEV-1 (High-Impact Active Harm).1  
* **First 5 Minutes Actions:**  
  1. Isolate and freeze the active user session; block the associated IP address.2  
  2. Terminate active WebSocket connections and close active runner processes.2  
  3. Scan execution logs to identify if the model attempted to leak system instructions or execute unauthorized tools.2  
* **Containment Steps:** Revoke the session's active OAuth tokens. Route the tenant's queries to the quarantined fallback model instance.2  
* **Evidence to Preserve:** Preserve hashes and secure forensic references for the input, rendered prompt, injected payload, and tool trace history. Store raw sensitive payloads only in the controlled forensic vault.1  
* **Owner Roles:** Security Lead (Primary investigator); Prompt Owner (Defensive engineer).17  
* **Rollback Steps:** Revert the system prompt template to the last safe version in Git.3  
* **Validation Steps:** Run the purple-team jailbreak evaluation suite inside the CI/CD pipeline, verifying that the incident payload and representative variants are blocked or safely contained according to the applicable risk profile.1  
* **Communication Path:** Render a full-screen red error overlay: *"Session locked due to a security policy violation. Your progress has been saved securely."*.3  
* **Postmortem Actions:** promote the sanitized payload pattern into the adversarial evaluation suite and review whether detector training data should be updated.2

### **Runbook 4: Tool Misfire Incident (Unauthorized Action)**

* **Incident Trigger & Severity:** The database monitor alerts on an un-idempotent write action executed without active maker-checker clearance.2 SEV-1 (High-Impact Active Harm).2  
* **First 5 Minutes Actions:**  
  1. Trigger the FF_TOOL_REVOKE kill switch, disabling the misfiring API connection instantly.2  
  2. Suspend the active multi-agent loop orchestrator.2  
  3. Verify the ledger status inside the database of record to identify if writes were committed.2  
* **Containment Steps:** Revoke the tool's scoped credentials in Secrets Manager.2  
* **Evidence to Preserve:** Seal the tool call arguments, the pre-action database state hash, the idempotency key, and the Saga execution trail.1  
* **Owner Roles:** Tool Owner (Primary investigator); Saga Coordinator (Ledger checker); Compliance Lead (Risk auditor).17  
* **Rollback Steps:** Execute the registered Saga compensation payload to semantically reverse committed actions.3  
* **Validation Steps:** Run a dry-run test transaction in staging, asserting that the tool gateway rejects un-idempotent payloads.3  
* **Communication Path:** Display a full-screen modal: *"We are verifying your transaction status with the bank. Do not click buy again."*.3  
* **Postmortem Actions:** Update tool contracts to enforce physical argument validation schemas (Pydantic validation) before execution.2

### **Runbook 5: Cost Bomb Incident (Runaway Loops)**

* **Incident Trigger & Severity:** Budget monitors alert on a session crossing the 5.00 USD spend ceiling; token burn velocity spikes.1 SEV-1 (High-Impact Active Harm).2  
* **First 5 Minutes Actions:**  
  1. Force terminate the active agent execution thread.2  
  2. Revoke the session's active API routing keys inside the gateway proxy.2  
  3. Clear the active memory cache for the running conversation.2  
* **Containment Steps:** Apply a zero-token budget constraint to the tenant's workspace configurations.2  
* **Evidence to Preserve:** Record the cumulative token metrics, the execution turn count, the planning trace, and the exact formatting errors.1  
* **Owner Roles:** SRE/Platform Lead (Resource auditor); Agent Orchestration Lead (Loop debugger).17  
* **Rollback Steps:** Invalidate active model prompt caches for the affected templates.2  
* **Validation Steps:** Execute a loop simulation test in staging to verify that the orchestrator terminates tasks when thresholds are breached.1  
* **Communication Path:** Display an inline alert card: *"Session paused. You've reached your workspace's token limit. Progress saved."*.3  
* **Postmortem Actions:** Refactor prompt templates to reduce tool-schema overhead; implement hard limits on planning iterations.2

### **Runbook 6: Model Drift Incident (Behavior Regression)**

* **Incident Trigger & Severity:** Strategic telemetry logs a cosine distance shift > 0.15 compared to reference embeddings.1 SEV-2 (Contained Semantic Failure).2  
* **First 5 Minutes Actions:**  
  1. Scan vLLM prometheus metrics to check for changes in latency, throughput, or perplexity.1  
  2. Run the 100-case canary prompt suite in parallel.1  
  3. Freeze all active prompt or routing configurations in staging.30  
* **Containment Steps:** Re-route production traffic from the drifted endpoint to the preceding stable model snapshot.3  
* **Evidence to Preserve:** Save the generated output tokens, the calculated centroid distances, and the canary performance logs.1  
* **Owner Roles:** Model Owner (Lead investigator); AI Operations Lead (Gateway controller); SRE Lead.17  
* **Rollback Steps:** Revert the active model endpoint route inside the gateway proxy.20  
* **Validation Steps:** Execute the full CI/CD regression evaluation suite, asserting that accuracy scores meet baseline thresholds.1  
* **Communication Path:** Subtle header notification: *"Running in efficient compatibility mode to optimize system accuracy."*.3  
* **Postmortem Actions:** Establish automated daily embedding drift checks inside production telemetry collections.1

### **Runbook 7: Corpus Poisoning Incident**

* **Incident Trigger & Severity:** HubScan flags a document vector with a robust z-score > 5.0, indicating topological index poisoning.2 SEV-1 (High-Impact Active Harm).2  
* **First 5 Minutes Actions:**  
  1. Apply a Row-Level Security database constraint to filter out the poisoned document IDs from active search.2  
  2. Suspend the indexing worker's document ingestion pipeline.2  
  3. Quarantine the poisoned files inside an offline S3 bucket.2  
* **Containment Steps:** Disable vector search queries for the affected tenant workspace; fall back to BM25 keyword matching.3  
* **Evidence to Preserve:** Preserve source IDs, source version hashes, chunk/vector IDs, secure evidence references, embedding metadata, and robust z-score logs.1  
* **Owner Roles:** Retrieval Owner (Primary investigator); Database Administrator (Index controller); Security Lead.17  
* **Rollback Steps:** Rebuild the vector database HNSW graph from the last safe backup.2  
* **Validation Steps:** Re-run vector query simulations, asserting that retrieval returns clean nearest neighbors.1  
* **Communication Path:** Status banner: *"Search capabilities are operating in cached mode. Real-time updates may be delayed."*.3  
* **Postmortem Actions:** Enforce cryptographic signature checks (Sigstore/C2PA) on all files before database ingestion.2

### **Runbook 8: Human Review Queue Failure**

* **Incident Trigger & Severity:** Reviewer alerts trigger on queue backlog limits; average approval times fall below 350 ms.1 SEV-2 (Contained Semantic Failure).2  
* **First 5 Minutes Actions:**  
  1. Halt high-volume automated routing pipelines.2  
  2. Switch active workspaces to safe default states, blocking mutations without reviewer signatures.2  
  3. Route priority reviews to the backup supervisor queue.3  
* **Containment Steps:** Temporarily lock high-risk tool execution gates.2  
* **Evidence to Preserve:** Log the queue backlog stats, reviewer dwell times, and unapproved draft files.1  
* **Owner Roles:** Support Lead (Queue coordinator); Compliance Lead (Operations auditor).17  
* **Rollback Steps:** Revert approval workflow configurations to the last verified checklist.3  
* **Validation Steps:** Run gold-label sentinel check cases through the queue to audit reviewer alignment.1  
* **Communication Path:** Warning card: *"Verification queues are temporarily delayed. High-risk transactions may take up to 2 hours."*.3  
* **Postmortem Actions:** Establish automated limits on the number of cases routed to individual reviewers per hour.2

### **Runbook 9: Degraded-Mode Fail-Open Anomaly**

* **Trigger Scenario:** A gateway routing failure allows a query to bypass active security filters or formatting validation gates.2 SEV-0 (Catastrophic System Failure).2  
* **First 5 Minutes Actions:**  
  1. Activate the global platform kill switch, freezing user interactions.2  
  2. Invalidate the gateway's active API routing configurations.1  
  3. Check log files to identify if unredacted data or unsafe commands crossed the interface.1  
* **Containment Steps:** Suspend all active model connections. Enforce a hard fail-closed configuration across all edge proxies.2  
* **Evidence to Preserve:** Copy the raw gateway configurations, the W3C tracecontext context logs, and the blocked output streams.1  
* **Owner Roles:** SRE/Platform Lead (Primary investigator); AI Operations Lead (Gateway controller); Security Lead.17  
* **Rollback Steps:** Rebuild and deploy the gateway proxy using the last stable Docker container image.3  
* **Validation Steps:** Execute the full CI/CD security and isolation test suite, confirming 100% block rates.1  
* **Communication Path:** Public status page update: *"The platform is offline for scheduled security system maintenance. All user data is secured."*.3  
* **Postmortem Actions:** Consolidate all retry, fallback, and validation rules inside a centralized, self-hosted API gateway.3

### **Runbook 10: Privacy and Cross-Tenant Leak Incident**

* **Trigger Scenario:** Session analyzers detect a tenant ID mismatch between a user's JWT and the retrieved metadata schemas.2 SEV-0 (Catastrophic System Failure).2  
* **First 5 Minutes Actions:**  
  1. Isolate the affected tenant namespaces inside the pgvector and Redis clusters.2  
  2. Revoke active session tokens and terminate Websocket connection pools.2  
  3. Isolate and block access to the affected vector database index partitions.2  
* **Containment Steps:** Enforce a global read-only mode for the isolated workspaces, preventing document updates.3  
* **Evidence to Preserve:** Record the trace IDs, the mismatched user identities, the retrieved vector coordinates, and the cache keys.1  
* **Owner Roles:** SRE/Platform Lead (Primary investigator); AI Operations Lead (Gateway controller); Privacy Lead; Security Lead.17  
* **Rollback Steps:** Flush all cached prompt-response vectors and re-initialize isolated cache spaces.2  
* **Validation Steps:** Execute 1,000 automated isolation probe queries inside staging, verifying that representative isolation probes cannot retrieve cross-tenant resources under the affected configurations.2  
* **Communication Path:** Dedicated enterprise customer notification: *"We have isolated a brief namespace configuration overlap in your workspace. Active session credentials have been updated to preserve your privacy."*.3  
* **Postmortem Actions:** Enforce database-level Row-Level Security (RLS) policies on every vector similarity query.2

## **Operational Readiness and Game Days**

Before launching an AI system, teams must prove that feature flags, kill switches, rollback paths, evidence capture, runbooks, on-call roles, customer communication templates, and evaluation feedback loops operate correctly under pressure.18 Operational readiness is verified through a structured, multi-step game day:

  Fault Injection ──► Telemetry Trigger ──► Runbook Activation ──► Forensics Check ──► Verification

### **Readiness scenarios executed in staging environments:**

1. **Prompt Injection and Exfiltration:** A simulated malicious user attempts to bypass system prompt safety blocks and exfiltrate customer account details.2 The game day tests the speed of the ARGUS output filter and verifies that the system isolates the compromised session in under 120 milliseconds.2  
2. **Vector Index Topological Poisoning:** Responders inject 5 adversarially crafted documents into the corporate knowledge base.31 The exercise verifies that the HubScan detector successfully flags and quarantines the poisoned vector coordinates, and that the retrieval engine falls back to lexical matches.2  
3. **Model Version Behavior Regression:** The team simulates an unannounced model provider update that alters JSON formatting.2 Responders must verify that the gateway proxy detects the regression, logs the routing decision, and executes an atomic rollback to the stable model checkpoint in under 60 seconds.1  
4. **Runaway Agent Loop and Billing Spike:** Responders run an agent configuration designed to repeat identical database lookups indefinitely.2 The game day verifies that the budget gateway terminates the loop execution path, triggers a fail-closed status, and preserves the failing traces.1

**Deployment Readiness Checklist:**

* [ ] Global and tenant-scoped kill switches are verified and accessible to on-call SREs.2  
* [ ] The gateway proxy supports atomic endpoint re-routing without requiring code redeployments.20  
* [ ] The pgvector database enforces PostgreSQL Row-Level Security (RLS) on all search transactions.2  
* [ ] Ingestion pipelines are configured with strict magic-byte scanning and run within microVM sandboxes.2  
* [ ] The telemetry collector redact pipeline masks PII and API keys from central logs.1  
* [ ] Every system prompt template is version-controlled and tied to an automated regression gate.1

## **Incident-to-Eval Feedback Loop**

Every significant AI incident must create or update evals, telemetry, artifacts, policies, runbooks, or documented exceptions. This feedback loop is the platform's immunological learning cycle:

  [Production Failure] ──► Trace Captured ──► Sanitized via ARGUS   
                                                   │  
                                                   ▼  
  ◄── Golden Case Promoted ◄── SME Review 

The feedback loop is executed through a disciplined engineering pipeline:

1. **Incident Discovery:** High-severity incidents, user-initiated corrections, citation mismatches, tool timeouts, and human approval overrides are automatically captured by the strategic telemetry layer.1  
2. **Telemetry Redaction:** To protect sensitive data and preserve compliance (e.g., GDPR, HIPAA), the raw trace is passed through the inline ARGUS scanning gate, replacing PII, system passwords, and API keys with secure placeholders.1  
3. **Ground Truth Authoring:** The sanitized trace is packaged into a standard golden case format, and subject matter experts assign the correct ground-truth typologies and rubrics to resolve the failure.1  
4. **Golden Set Promotion:** The new case is run against the current production baseline and committed to the versioned evaluation repository, transforming the production incident into an automated release gate to prevent regressions.1

## **Cross-Canon Handoff Map**

AI-ENG-AC defines the operational response layer for the canon: detection, intake, triage, containment, rollback, communication, incident review, and incident-to-eval promotion. It consumes telemetry, evaluation, verification, security, degraded-mode, and governance artifacts, then feeds improvements back into the lifecycle.

| Canon Report | Handoff Into AC | AC Operational Use | Feedback Returned |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context object IDs, memory state, session scope, compaction records. | Diagnose context loss, stale memory, state corruption, or instruction decay. | Incident-derived context invariants and state-preservation tests. |
| **AI-ENG-D — Corpus Engineering** | Source authority, corpus lifecycle state, provenance, ownership, document version. | Quarantine suspect sources and scope data/corpus incidents. | New corpus validation, lifecycle, and provenance requirements. |
| **AI-ENG-E — Retrieval Pipeline** | Retrieval traces, candidate sets, filters, citation bundles, evidence packets. | Diagnose grounding, citation, and retrieval failure incidents. | New retrieval evals, filter checks, and evidence-sufficiency gates. |
| **AI-ENG-F — Freshness and Conflict Detection** | Freshness status, conflict packets, source version drift, citation stability. | Determine whether stale or conflicting evidence caused user-visible harm. | Freshness alerts and conflict-resolution runbook updates. |
| **AI-ENG-L — Serving Architecture** | Route manifests, latency, queue state, model endpoints, cache behavior. | Execute route rollback, failover, capacity containment, and serving recovery. | New route SLOs, rollback drills, and serving canaries. |
| **AI-ENG-M — Agentic Orchestration** | Workflow state, loop counters, no-progress hashes, task graph. | Stop runaway agents, preserve plan state, and diagnose orchestration failures. | New loop limits, state checkpoints, and agent termination tests. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, manifests, argument validators, side-effect classes. | Disable unsafe tools and diagnose tool payload or schema failures. | Contract hardening, idempotency requirements, and new tool evals. |
| **AI-ENG-O — Action Verification** | Action ledgers, pre/post state hashes, idempotency keys, verification state. | Reconcile unknown tool state, compensate committed effects, prevent false success. | New verification predicates and state-reconciliation tests. |
| **AI-ENG-P — Multimodal Understanding** | Parser versions, evidence coordinates, OCR/layout confidence, media refs. | Diagnose parser, OCR, document, chart, image, and video evidence incidents. | Parser regression cases and evidence-adequacy thresholds. |
| **AI-ENG-Q — Speech / Realtime** | Transcript state, endpointing, confirmation, interruption, voice degradation events. | Pause high-impact voice workflows and switch to safer confirmation modes. | New noisy-audio, confirmation, and barge-in game-day tests. |
| **AI-ENG-R — UI Agents** | Browser traces, DOM/screenshot hashes, action verification, automation pause state. | Diagnose blind clicks, UI drift, false completion, and interface trust failures. | UI-agent recovery and action-verification tests. |
| **AI-ENG-S — Production Pathologies** | Failure classes, malformed output, hallucination subtypes, brittle-chain patterns. | Classify semantic incidents and choose containment/recovery path. | New pathology-specific runbooks and regression gates. |
| **AI-ENG-T — Boundary Defense** | Tenant scope, authority hierarchy, RLS state, cache isolation, injection events. | Contain prompt injection, cross-tenant leakage, and boundary violations. | New isolation probes and boundary-defense incident cases. |
| **AI-ENG-U — Supply Chain Security** | Artifact signatures, dependency manifests, sandbox profiles, model/data provenance. | Quarantine unsafe artifacts and verify rollback targets. | New artifact-readiness and loader-hardening requirements. |
| **AI-ENG-V — Resource Abuse** | Budget ceilings, token burn, tool-call limits, retry storms, abuse signals. | Halt cost bombs, throttle workflows, and enforce fail-closed budget controls. | New resource-abuse drills and budget-policy changes. |
| **AI-ENG-W — UX Resilience** | Degraded-mode states, fallback contracts, continuity state, user disclosure events. | Communicate degraded states and preserve user progress during incidents. | New degraded-mode tests and disclosure templates. |
| **AI-ENG-X — User Trust** | Trust calibration signals, citation interactions, user corrections, contestability events. | Detect user-facing trust failures and repair transparency patterns. | New disclosure, evidence, and contestability requirements. |
| **AI-ENG-Y — Human Review** | Approval records, reviewer queue state, maker-checker outcomes, escalation packages. | Pause unsafe automation and route high-impact uncertainty to humans. | New reviewer sentinel cases and approval-flow thresholds. |
| **AI-ENG-Z — Strategic Telemetry** | Trace IDs, spans, token/cost metrics, routing events, redaction metadata. | Intake, scope, replay, and correlate incidents. | New telemetry fields, alerts, dashboards, and SLOs. |
| **AI-ENG-AA — Evaluations** | Golden sets, canaries, regression suites, calibrated scoring artifacts. | Validate rollback, hotfix, mitigation, and closure. | Incident traces promoted into permanent eval cases. |
| **AI-ENG-AB — Verification Artifacts** | Audit trails, secure references, trace manifests, policy/model/tool versions. | Preserve forensic record and support reproducible postmortems. | New evidence-package requirements and replay constraints. |
| **AI-ENG-AD — Governance** | Policy ownership, approval workflows, reporting obligations, accountability boundaries. | Determine reporting, escalation, exception, and lifecycle-governance requirements. | Postmortem action items, governance exceptions, and policy updates. |
| **AI-ENG-AJ — Reference Architectures** | Deployment blueprints, gateway patterns, sandboxing, audit stores, runbook automation. | Implement operational controls as reference architecture components. | Architecture updates based on incidents and game days. |

## **Strategic Conclusions and Architectural Recommendations**

To transition from ad-hoc troubleshooting to systemic, high-assurance behavioral governance, enterprise platforms must consolidate their operational procedures outside the model's cognitive boundary.1 Based on the analyses compiled in this report, the following four strategic principles are recommended for deployment architectures:

1. **Enforce State Verification Before Confirmations:** Never allow conversational or auditory interfaces to generate verbal completion claims (e.g., "Your payment has been sent") until the underlying API transaction has been committed and verified inside the database of record.2 Spoken or generated claims must never outrun physical reality.2  
2. **Centralize Resilience at the Gateway Layer:** Avoid writing separate retry, fallback, and rate-limiting logic inside individual application services.3 Centralize these capabilities within a high-performance, budget-aware runtime gateway positioned entirely outside the model's cognitive boundary, ensuring consistent policy enforcement, cost tracking, and provider failovers across the entire organization.1  
3. **Secure Caches against Timing Side-Channels:** Sharing semantic or prefix caches globally across mutually untrusted tenants introduces timing side-channel risks.3 All cache keys must be cryptographically bound to tenant identity and user permissions, and serving runtimes must deploy selective prefix isolation (such as the CacheSolidarity framework) to prevent timing side-channel probes from exfiltrating private context.2  
4. **Enforce Strict Least-Privilege Tool Credentials:** Model-driven agents must never run with broad administrative service tokens or "god-mode" database accounts.2 Every tool execution must be mediated by a secure credential broker that validates user identity and mints short-lived, highly restricted OAuth tokens (validity less than 900 seconds) specifically for that single execution, protecting local file systems and egress routes from lateral compromise.2

## **Durable Principles of AI Operations**

1. **Service Recovery Is Not Behavioral Recovery**  
   A system is not recovered because the endpoint is online. It is recovered when outputs, actions, evidence, routing, policy, and user-facing status are back inside the approved behavioral envelope.

2. **Contain Harm Before Solving the Mystery**  
   Incident response begins with stopping active damage. Root cause can wait; unauthorized tools, poisoned indexes, bad routes, and leaking caches cannot.

3. **Preserve Evidence Before Destructive Cleanup**  
   Cache flushes, redeploys, session wipes, and credential rotations can destroy forensic state. Preserve hashes, traces, manifests, and secure references first when safety allows.

4. **Rollback the Smallest Effective Surface**  
   Do not revert the whole platform when a prompt, adapter, cache namespace, corpus lifecycle state, or tool contract is the failing surface.

5. **Unknown State Is a First-Class Incident State**  
   Timeouts and partial commits are not success and not failure. They require hold, reconciliation, compensation, forward recovery, or escalation.

6. **Prompt Hotfixes Are Temporary Operational Controls**  
   Emergency prompt changes must be versioned, bounded, tested, owned, and either promoted through normal release gates or retired.

7. **Runbooks Must Be Executable Under Stress**  
   A runbook is not a philosophy pamphlet. It should say who acts, what they disable, what they preserve, what they validate, and how they communicate.

8. **Communications Must Respect Evidence State**  
   Never reassure users that no data was exposed, no action occurred, or no harm happened until verification supports it.

9. **Every Significant Incident Becomes an Evaluation**  
   Production failures should harden the system. Sanitized incident traces must become golden cases, adversarial tests, telemetry alerts, or governance controls.

10. **Operations Belongs Outside the Model Boundary**  
   Kill switches, feature flags, credential revocation, route control, policy enforcement, and rollback must be deterministic controls, not polite requests embedded in prompts.

#### **Works cited**

1. [KNOWLEDGE] - AI Engineering - Volume 9. Z-AB Observability, Evaluation, and Verification  
2. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
3. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
4. Incident management | AI Governance Platform - VerifyWise, accessed June 12, 2026, [https://verifywise.ai/platform/incident-management](https://verifywise.ai/platform/incident-management)  
5. Preventing Repeated Real World AI Failures by Cataloging Incidents: The AI Incident Database - AAAI, accessed June 12, 2026, [https://cdn.aaai.org/ojs/17817/17817-13-21311-1-2-20210518.pdf](https://cdn.aaai.org/ojs/17817/17817-13-21311-1-2-20210518.pdf)  
6. Resilience Circuit Breakers for Agentic AI - Medium, accessed June 12, 2026, [https://medium.com/@michael.hannecke/resilience-circuit-breakers-for-agentic-ai-cc7075101486](https://medium.com/@michael.hannecke/resilience-circuit-breakers-for-agentic-ai-cc7075101486)  
7. MLSecOps Enterprise Guide 2026: Securing AI/ML Pipelines | BeyondScale, accessed June 12, 2026, [https://beyondscale.tech/blog/mlsecops-enterprise-guide-2026](https://beyondscale.tech/blog/mlsecops-enterprise-guide-2026)  
8. Model Rollback: Reverting a Deployed Model to a Previous Version When Problems Occur, accessed June 12, 2026, [https://www.sandgarden.com/learn/model-rollback](https://www.sandgarden.com/learn/model-rollback)  
9. Strengthening Emergency Preparedness and Response for AI Loss of Control Incidents - RAND, accessed June 12, 2026, [https://www.rand.org/content/dam/rand/pubs/research_reports/RRA3800/RRA3847-1/RAND_RRA3847-1.pdf](https://www.rand.org/content/dam/rand/pubs/research_reports/RRA3800/RRA3847-1/RAND_RRA3847-1.pdf)  
10. LLMOps Pipeline Architecture Explained Simply | LaikaTest, accessed June 12, 2026, [https://laikatest.com/blogs/llmops-pipeline-architecture](https://laikatest.com/blogs/llmops-pipeline-architecture)  
11. AI SRE explained: what it is, how it works, and the human vs. AI reality | Blog - Incident.io, accessed June 12, 2026, [https://incident.io/blog/what-is-ai-sre-complete-guide-2026](https://incident.io/blog/what-is-ai-sre-complete-guide-2026)  
12. AI-Powered Incident Response: Use Cases, Frameworks, Tools, and a Complete Implementation Playbook - UnderDefense, accessed June 12, 2026, [https://underdefense.com/blog/ai-incident-response/](https://underdefense.com/blog/ai-incident-response/)  
13. Incident Priority - PagerDuty Knowledge Base, accessed June 12, 2026, [https://support.pagerduty.com/main/docs/incident-priority](https://support.pagerduty.com/main/docs/incident-priority)  
14. Incident Severity Levels Fully Explained Plus How-To's and Best Practices - Giva, accessed June 12, 2026, [https://www.givainc.com/blog/incident-severity-levels/](https://www.givainc.com/blog/incident-severity-levels/)  
15. How to Build Incident Severity Definitions - OneUptime, accessed June 12, 2026, [https://oneuptime.com/blog/post/2026-01-30-incident-severity-definitions/view](https://oneuptime.com/blog/post/2026-01-30-incident-severity-definitions/view)  
16. Prompt Injection Attacks: The LLM Security Risk IT Leaders Must Address, accessed June 12, 2026, [https://biztechmagazine.com/article/2026/04/prompt-injection-attacks-llm-security-risk-it-leaders-must-address-perfcon](https://biztechmagazine.com/article/2026/04/prompt-injection-attacks-llm-security-risk-it-leaders-must-address-perfcon)  
17. How to Implement Incident Response Procedures, accessed June 12, 2026, [https://oneuptime.com/blog/post/2026-01-30-sre-incident-response-procedures/view](https://oneuptime.com/blog/post/2026-01-30-sre-incident-response-procedures/view)  
18. LLMOps: Driving Strategic and Effective AI Lifecycle Management - Nitor Infotech, accessed June 12, 2026, [https://www.nitorinfotech.com/blog/llmops-driving-strategic-and-effective-ai-lifecycle-management/](https://www.nitorinfotech.com/blog/llmops-driving-strategic-and-effective-ai-lifecycle-management/)  
19. What is the Incident Response Process in SRE? - Visualpath, accessed June 12, 2026, [https://visualpathblogs.com/site-reliability-engineering/what-is-the-incident-response-process-in-sre/](https://visualpathblogs.com/site-reliability-engineering/what-is-the-incident-response-process-in-sre/)  
20. The AI Incident Response Playbook: Diagnosing LLM Degradation in Production, accessed June 12, 2026, [https://tianpan.co/blog/2026-04-19-ai-incident-response-playbook-llm-production](https://tianpan.co/blog/2026-04-19-ai-incident-response-playbook-llm-production)  
21. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
22. Defending AI Systems: A New Framework for Incident Response in the Age of Intelligent Technology - Coalition for Secure AI, accessed June 12, 2026, [https://www.coalitionforsecureai.org/defending-ai-systems-a-new-framework-for-incident-response-in-the-age-of-intelligent-technology/](https://www.coalitionforsecureai.org/defending-ai-systems-a-new-framework-for-incident-response-in-the-age-of-intelligent-technology/)  
23. SRE Incident Management Best Practices Every Startup Needs - Rootly, accessed June 12, 2026, [https://rootly.com/sre/sre-incident-management-best-practices-startup-needs-04bdd](https://rootly.com/sre/sre-incident-management-best-practices-startup-needs-04bdd)  
24. Kubernetes Deployment Automation: A Complete Guide - Plural.sh, accessed June 12, 2026, [https://www.plural.sh/blog/kubernetes-deployment-automation-guide/](https://www.plural.sh/blog/kubernetes-deployment-automation-guide/)  
25. What is JML? Meaning, Architecture, Examples, Use Cases, and How to Measure It (2026 Guide) - DevSecOps School, accessed June 12, 2026, [https://devsecopsschool.com/blog/jml/](https://devsecopsschool.com/blog/jml/)  
26. LLM Operations Architecture: Runtime Governance for Production AI Systems - Rack2Cloud, accessed June 12, 2026, [https://www.rack2cloud.com/llm-ops-model-deployment-strategy-guide/](https://www.rack2cloud.com/llm-ops-model-deployment-strategy-guide/)  
27. LLM Security: Complete Guide for CTOs and IT Security Officers - MobiDev, accessed June 12, 2026, [https://mobidev.biz/blog/llm-security-guide-for-ctos-it-security-officers](https://mobidev.biz/blog/llm-security-guide-for-ctos-it-security-officers)  
28. The Roadmap for Mastering LLMOps in 2026 - MachineLearningMastery.com, accessed June 12, 2026, [https://machinelearningmastery.com/the-roadmap-for-mastering-llmops-in-2026/](https://machinelearningmastery.com/the-roadmap-for-mastering-llmops-in-2026/)  
29. Runbook Automation Self-Hosted - PagerDuty, accessed June 12, 2026, [https://www.pagerduty.com/platform/automation/process-software/](https://www.pagerduty.com/platform/automation/process-software/)  
30. Qwen 3.6 in Production: Release Runbook, AI Rollback, and LLMOps Versioning, accessed June 12, 2026, [https://stajic.de/blog/qwen-3-6-in-production-release-runbook-ai-rollback-and-llmops-versioning](https://stajic.de/blog/qwen-3-6-in-production-release-runbook-ai-rollback-and-llmops-versioning)  
31. Prompt Injection Attacks in Large Language Models and AI Agent Systems: A Comprehensive Review of Vulnerabilities, Attack Vectors, and Defense Mechanisms - Preprints.org, accessed June 12, 2026, [https://www.preprints.org/manuscript/202511.0088?utm_source=chatgpt.com](https://www.preprints.org/manuscript/202511.0088?utm_source=chatgpt.com)

---

# AI-ENG-AD — Governance Architecture - Policy, Audit, Compliance & Accountability

## **Doctrinal Foundations: The Executable AI Operating System**

In high-dimensional artificial intelligence systems, production reliability cannot be governed by traditional application performance monitoring (APM) paradigms or static, post-hoc compliance checklists.1 Traditional web software assumes a deterministic relationship between code, infrastructure state, and output.1 In contrast, architectures driven by probabilistic large language models (LLMs) and multi-agent loops introduce non-deterministic, behavioral failures where backend services return successful HTTP 200 statuses while simultaneously generating factually false advice, leaking cross-tenant data, fabricating API calls, or bypassing security boundaries.1 This divergence establishes the "Green Dashboard Fallacy": the erroneous and high-risk assumption that an AI system is operating correctly simply because its servers, databases, and APIs are online and responsive.1  
To bridge this visibility and control deficit, modern organizations must deploy a structured **Governance Architecture** designed as the practical operating system for organizational AI control.1 This architecture enforces a rigorous, system-integrated governing doctrine: **AI governance must be lightweight where risk is low, strict where consequences are high, and executable everywhere**.5 Policies, risk classifications, approvals, audit trails, and data rules must be attached directly to production environments, active codebases, and live database transactions—never left to float serenely above production as abstract, unexecutable ethical principles.1 The fundamental metric of organizational safety is not the presence of a "Responsible AI" PDF, but the programmatic capability to identify, isolate, audit, and prove the compliance of every model, tool, dataset, and state mutation in real time.1

## **Unified Frameworks: NIST AI RMF 1.0 and ISO/IEC 42001 Integration**

A high-assurance governance system must map its internal technical controls directly to established international standards.7 This synchronization is achieved by unifying the process-oriented lifecycle of the **NIST AI Risk Management Framework (AI RMF 1.0)** with the formal, auditable requirements of the **ISO/IEC 42001 Artificial Intelligence Management System (AIMS)**.9

### **The NIST AI RMF Core Functions**

The NIST AI RMF Core divides risk management into four continuous, non-linear functions 11:

* **Govern:** Instills a culture of risk awareness, establishes legal and regulatory registries, allocates resources, and defines organizational accountability.7 It functions as a cross-cutting framework that structures and authorizes the other three functions.11  
* **Map:** Captures the situational context of each AI system, documenting its intended purpose, potential benefits, operational boundaries, and context-specific limitations.7  
* **Measure:** Employs quantitative and qualitative evaluations (such as tracking accuracy, measuring bias, and auditing citations) to benchmark risks and track behavioral regressions over time.7  
* **Manage:** Allocates risk-treatment resources to mapped and measured exposures, enabling automated recovery, incident response, or system deactivation when anomalies exceed thresholds.7

```text
NIST AI RMF + ISO/IEC 42001 GOVERNANCE LOOP

+--------------------------------------------------------------------+
| GOVERN                                                             |
| culture | policy | accountability | legal/regulatory register      |
| ISO/IEC 42001 alignment: AIMS scope, roles, policy, objectives     |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MAP                                                                |
| use case | context | affected parties | benefits | limits | risks  |
| ISO/IEC 42001 alignment: AI system inventory, impact assessment    |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MEASURE                                                            |
| TEVV | bias/fairness | robustness | drift | security | evidence    |
| ISO/IEC 42001 alignment: evaluation, monitoring, audit evidence    |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MANAGE                                                             |
| risk treatment | runtime controls | incident response | exceptions |
| ISO/IEC 42001 alignment: operating controls and improvement loop   |
+-----------------------------+--------------------------------------+
                              |
                              v
                         back to GOVERN

Rule:
  Governance is not a document layer above production.
  It is a decision-and-control loop attached to systems, evidence,
  owners, release gates, and runtime enforcement points.
```

### **ISO/IEC 42001 (AIMS) and the Statement of Applicability**

While NIST outlines the necessary operational process, ISO/IEC 42001 translates these activities into a set of 38 normative controls organized across nine Annex A control domains, which must be documented within a formal **Statement of Applicability (SoA)**.8 The SoA acts as the primary contract between the organization's technical practices and compliance auditors, documenting the explicit rationale for including or excluding specific controls, detail-mapping their implementation status, and providing pointers to active software verifications.8

### **Control Mapping and Suggested Audit Artifacts**

To satisfy NIST AI RMF and ISO/IEC 42001 together, organizations should map governance functions to implementation controls and audit evidence. The purpose of this map is not to prove compliance by collecting more paper. It is to connect obligations to owners, systems, runtime controls, and verifiable evidence.

| NIST AI RMF Function | Governance Objective | ISO/IEC 42001 Alignment | Preferred Audit Evidence | Runtime / Engineering Evidence |
| :---- | :---- | :---- | :---- | :---- |
| **Govern** | Establish accountability, risk appetite, policy, and oversight. | AIMS scope, AI policy, roles, responsibilities, management review. | AI governance policy, RACI, risk-acceptance register, system owner roster. | Policy bundle versions, approval records, release-gate decisions, exception records. |
| **Map** | Identify the system, use case, stakeholders, context, intended purpose, and foreseeable misuse. | AI system inventory, impact assessment, resource and lifecycle documentation. | AI system inventory, impact assessment, data-flow map, model/tool/data inventory. | Context manifests, corpus lineage records, tool manifests, model route manifests. |
| **Measure** | Evaluate technical and socio-technical risk. | Verification, validation, testing, monitoring, data quality, performance evaluation. | TEVV plans, evaluation results, bias/fairness assessments, robustness tests. | Golden-set results, canary traces, drift signals, citation/evidence verification metrics. |
| **Manage** | Treat, monitor, accept, transfer, or reduce risk. | Operating controls, supplier controls, incident response, continual improvement. | Risk treatment plan, incident records, vendor assessments, control review reports. | Gateway policy decisions, OPA/Rego logs, kill-switch records, runbooks, rollback traces. |
| **Monitor / Improve** | Reassess risk after deployment and incidents. | Internal audit, management review, corrective actions, lifecycle governance. | Post-incident reviews, audit findings, corrective action plans. | Incident-to-eval promotions, telemetry alerts, new regression cases, policy diffs. |

Audit evidence should be proportional to risk. A low-risk internal summarizer does not need the same evidence package as a high-impact credit, medical, employment, financial, safety, or regulated decision-support system.

## **Policy-as-Code (PaC) and Centralized API Gateways**

Relying on system prompts or inline instructions to enforce system rules introduces severe security vulnerabilities.1 When language models are confronted with long contexts, conversational histories, or adversarial inputs, their attention is diluted, causing them to bypass prompt-level restrictions.1 This failure mode makes "Compliance at the Point of Change" (CAPOC) a non-negotiable architectural paradigm: **compliance rules must be written as declarative, machine-readable code, managed within version-controlled repositories, and programmatically evaluated before any build is merged or action executed**.19

### **Decoupling Policy from Application Code via Gateways**

By routing all model transactions through a centralized, budget-aware **AI Agent Gateway**, organizations decouple authorization logic from application code, preventing developers from bypassing compliance gates.6 The gateway functions as an application-layer firewall that intercepts intent, validates input schemas, checks user credentials, and externalizes access decisions to **Open Policy Agent (OPA)** before sending the query to upstream providers.18

```text
POLICY-AS-CODE AI GATEWAY FLOW

[ User / Client / Agent Request ]
        |
        v
+-------------------------------+
| AI Agent Gateway              |
| - authenticate subject        |
| - bind tenant/session scope   |
| - normalize intent/tool call  |
| - attach model/tool metadata  |
| - estimate budget/risk        |
+---------------+---------------+
                |
                v
+-------------------------------+
| Policy Decision Point         |
| OPA / policy engine           |
| - RBAC / ABAC                 |
| - tenant boundary             |
| - tool authority              |
| - budget ceiling              |
| - approval requirement        |
| - artifact allowlist          |
+---------------+---------------+
        | allow                         | deny / review / degrade
        v                               v
+-------------------------------+   +-------------------------------+
| Route to Serving / Tool Layer |   | Block / Escalate / Log        |
| - approved model route        |   | - reason code                 |
| - scoped credential           |   | - audit event                 |
| - bounded execution           |   | - user-safe status            |
+-------------------------------+   +-------------------------------+
```

The gateway is the policy enforcement point. The model may propose an action, but the gateway decides whether the action is allowed, requires review, must degrade, or must fail closed.

To streamline this workflow, modern setups integrate tools like **AICertify** and **Gopal**.22 AICertify automatically collects and generates the required input context (such as model versions, token metrics, and security hashes), while Gopal provides the domain-specific Rego policies that OPA evaluates.22 This integration ensures that AI-specific metrics can be audited using the same Policy-as-Code infrastructure that governs traditional cloud deployments.22

### **The Validation Gap: Testing Against Real Artifacts**

A persistent bottleneck in Policy-as-Code adoption is the "validation gap"—where security teams write OPA policies that check syntax correctly but crash production environments because they cannot test the rules against real software artifacts before they ship.23 To resolve this, platform engineering teams deploy the **JFrog AppTrust PaC Playground**.23  
This playground allows AppSec leads to load actual binaries and Software Bills of Materials (SBOMs) from their system of record and run dry-runs of newly drafted Rego policies against historical artifacts.23 This prevents policies from blocking legitimate releases due to missing context variables, turning continuous governance from a development blocker into a secure business enabler.23

### **Production-Grade Rego Policy for AI Gateway Authorization**

The following Rego policy illustrates how an AI gateway can enforce tenant scope, RBAC, budget ceilings, approval requirements, and artifact integrity before a model-generated tool action is dispatched.

The policy assumes that trusted registry data—such as approved plan hashes, tool manifests, and environment rules—is supplied to OPA as a signed data bundle or trusted sidecar input. OPA should not be treated as casually querying arbitrary production databases during local policy evaluation.

```rego
package ai.gateway.authorization

default allow := false
default decision := {
  "allow": false,
  "reason": "unauthorized_gateway_access",
  "review_required": false,
}

# Expected input shape:
#
# input.subject = {
#   "id": "opaque_user_or_service_id",
#   "tenant_id": "tenant_123",
#   "roles": ["deploy_bot"],
#   "suspended": false,
#   "remaining_session_budget_usd": 12.50
# }
#
# input.tool_call = {
#   "name": "apply_infrastructure",
#   "params": {
#     "tenant_id": "tenant_123",
#     "environment": "staging",
#     "plan_name": "service-update.plan",
#     "plan_hash": "sha256:abc..."
#   },
#   "risk_class": "high_impact_write"
# }
#
# input.token_metrics = {
#   "input_tokens": 2500,
#   "max_output_tokens": 800
# }
#
# data.registry.valid_plan_hashes = {
#   "sha256:abc...": true
# }

allow {
  not subject_suspended
  valid_tenant_scope
  rbac_permissions_met
  not budget_ceiling_breached
  not blocked_destructive_action
  plan_integrity_verified
  approval_requirement_satisfied
}

decision := {
  "allow": true,
  "reason": "allowed",
  "review_required": false,
} {
  allow
}

decision := {
  "allow": false,
  "reason": reason,
  "review_required": review_required,
} {
  not allow
  reason := deny_reason
  review_required := requires_human_review
}

subject_suspended {
  input.subject.suspended == true
}

valid_tenant_scope {
  input.subject.tenant_id == input.tool_call.params.tenant_id
}

rbac_permissions_met {
  input.tool_call.name == "read_status"
  "viewer" in input.subject.roles
}

rbac_permissions_met {
  input.tool_call.name == "apply_infrastructure"
  "deploy_bot" in input.subject.roles
  input.tool_call.params.environment != "production"
}

rbac_permissions_met {
  input.tool_call.name == "apply_infrastructure"
  "sre_admin" in input.subject.roles
}

estimated_cost_usd := cost {
  input_cost := input.token_metrics.input_tokens * 0.000015
  output_cost := input.token_metrics.max_output_tokens * 0.00006
  cost := input_cost + output_cost
}

budget_ceiling_breached {
  estimated_cost_usd > input.subject.remaining_session_budget_usd
}

blocked_destructive_action {
  input.tool_call.name == "apply_infrastructure"
  endswith(input.tool_call.params.plan_name, "-destroy.plan")
  not "break_glass_admin" in input.subject.roles
}

plan_integrity_verified {
  data.registry.valid_plan_hashes[input.tool_call.params.plan_hash] == true
}

approval_requirement_satisfied {
  input.tool_call.risk_class != "high_impact_write"
}

approval_requirement_satisfied {
  input.tool_call.risk_class == "high_impact_write"
  input.approval.status == "approved"
  input.approval.payload_hash == input.tool_call.payload_hash
  input.approval.approver_id != input.subject.id
}

requires_human_review {
  input.tool_call.risk_class == "high_impact_write"
  not approval_requirement_satisfied
}

deny_reason := "subject_suspended" {
  subject_suspended
}

deny_reason := "tenant_id_mismatch" {
  not valid_tenant_scope
}

deny_reason := "insufficient_rbac_permissions" {
  valid_tenant_scope
  not rbac_permissions_met
}

deny_reason := "session_budget_breached" {
  budget_ceiling_breached
}

deny_reason := "blocked_destructive_plan" {
  blocked_destructive_action
}

deny_reason := "plan_hash_unverified" {
  not plan_integrity_verified
}

deny_reason := "approval_required" {
  requires_human_review
}
```

This policy does not rely on the model to decide whether an action is safe. The model proposes; the gateway evaluates; the policy engine returns an auditable decision.

## **Runtime Governance for Agentic Systems**

Policy-as-code and release gates are necessary, but agentic systems also need runtime governance. A model can generate valid-looking actions that become unsafe only after state changes, streaming outputs, tool latency, partial commits, cost accumulation, or human-review uncertainty. Governance must therefore attach constraints directly to the agent loop.

One useful pattern is **SARC-style runtime governance**, where system state, action space, reward/optimization pressure, and constraints are modeled explicitly. SARC should be treated as a reference pattern, not a single mandatory framework. The durable principle is broader: constraints must be executable, traceable, and enforced at the correct physical point in the workflow.

### **Constraint Specification Model**

A runtime governance constraint can be represented as:

```text
C = <source, class, predicate, verification_point, response>
```

| Field | Meaning | Example |
| :---- | :---- | :---- |
| **source** | The policy, legal, contractual, security, or organizational authority behind the constraint. | EU AI Act Article 14, corporate procurement policy, tenant data policy. |
| **class** | Whether the constraint is hard, soft, reviewable, or advisory. | hard block, human review, throttle, warning. |
| **predicate** | The executable condition evaluated by the system. | amount <= user_limit, tenant_id matches, PII not present. |
| **verification_point** | Where the check runs physically. | pre-action gate, action-time monitor, post-action auditor, escalation router. |
| **response** | What the system does when the predicate fails. | block, redact, hold, throttle, compensate, escalate, fail closed. |

```text
RUNTIME GOVERNANCE LOOP

[ Agent State / User Goal / Active Policy ]
        |
        v
+-------------------------------+
| Pre-Action Gate               |
| checks state + proposed action|
| before tool dispatch          |
+---------------+---------------+
                |
                v
+-------------------------------+
| Action-Time Monitor           |
| watches streaming output,     |
| latency, egress, cost, leaks  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Post-Action Auditor           |
| verifies source-of-record     |
| state, cost, commit, effects  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Escalation Router             |
| freezes workflow and packages |
| review when confidence, risk, |
| or policy requires it         |
+-------------------------------+
```

### **The Four Runtime Enforcement Points**

| Enforcement Point | When It Runs | Best For | Example Response |
| :---- | :---- | :---- | :---- |
| **Pre-Action Gate** | Before a tool or external action is invoked. | Tenant scope, approval status, RBAC/ABAC, payload schema, risk class, budget reservation. | Block, require approval, downgrade to read-only, or route to review. |
| **Action-Time Monitor** | During streaming generation, tool execution, browser automation, or long-running workflow. | Token/cost velocity, data leakage, egress attempts, timeout risk, unsafe intermediate outputs. | Cancel call, redact, cut off stream, revoke temporary credential. |
| **Post-Action Auditor** | After an action returns, before the model or UI claims completion. | Source-of-record verification, partial commit detection, actual cost, consistency with requested action. | Mark verified, hold unknown state, reconcile, compensate, or correct user status. |
| **Escalation Router** | Whenever a hard or reviewable constraint requires human judgment. | High-impact actions, low confidence, policy exceptions, contested outcomes, regulatory review. | Freeze workflow, package evidence, route to human review, preserve state. |

### **Specification-Trace Correspondence**

Runtime governance should provide specification-trace correspondence: for each consequential action in the trace, an auditor should be able to identify which constraints applied, where they were evaluated, what decision they produced, and what response occurred.

This is an auditability property, not a mystical proof spell. The system proves governance by preserving policy versions, decision IDs, trace IDs, state hashes, secure payload references, approval records, and action outcomes.

### **Constraint Enforcement Manifest**

| Constraint ID | Source | Class | Predicate | Enforcement Point | Response | Recovery Semantics |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **C_OVERSIGHT_01** | Human oversight policy / regulated high-impact workflow. | Reviewable hard boundary. | High-impact action has valid approval bound to payload hash. | Pre-Action Gate. | Block and escalate to checker. | No external write occurs; preserve draft. |
| **C_PRIVACY_02** | Privacy-by-design policy / GDPR-aligned internal rule. | Hard boundary. | Payload contains no prohibited sensitive data for the selected route. | Action-Time Monitor. | Abort, redact, and open privacy event if needed. | Clear transient workspace; preserve secure evidence reference. |
| **C_FINOPS_03** | Corporate FinOps policy. | Soft boundary until hard budget limit. | Workflow spend remains within tenant/session budget. | Action-Time Monitor. | Throttle, degrade, require approval, or stop. | Preserve state; resume only after budget decision. |
| **C_INTEGRITY_04** | Financial / operational integrity policy. | Hard boundary. | Post-action state matches expected source-of-record predicate. | Post-Action Auditor. | Hold unknown state or block success claim. | Reconcile, compensate, forward recover, or escalate. |
| **C_LOGGING_05** | Audit and retention policy. | Hard boundary for high-impact actions. | Required trace, policy, and action-ledger metadata were written. | Post-Action Auditor. | Freeze workflow and route to operations review. | Retry audit write if safe; otherwise hold and escalate. |

## **Dual-Control "Maker-Checker" Ledger Architecture**

When AI systems act as the "Makers" of financial or administrative changes—such as proposing loan disbursals, generating payments, or updating security parameters—traditional, single-user database writes create significant risks of fraud, compromise, or error.3 To enforce strict segregation of duties, organizations must implement the dual-authorization **Maker-Checker** pattern (Four-Eyes Principle) at the database engine layer.3  
Under this design, a **Maker** (the automated agent or user) initiates a sensitive operation.37 Instead of executing the action immediately, the system intercepts the request, blocks its execution on business tables, and routes the proposed transaction payload to a centralized **Approval Queue**.3 A **Checker** (a different, authorized human supervisor or auditor) must manually review, sign, and commit the proposed change from the queue.3

```text
MAKER-CHECKER CONTROL FLOW

[ Maker: user or agent proposes action ]
        |
        v
+-------------------------------+
| Action Interceptor            |
| - validates schema            |
| - classifies risk             |
| - blocks direct business write|
+---------------+---------------+
                |
                v
+-------------------------------+
| approval_requests Queue       |
| - payload hash / secure ref   |
| - maker identity              |
| - tenant / policy version     |
| - status = PENDING            |
+---------------+---------------+
                |
                v
[ Checker Review ]
  checker must be authorized
  checker must differ from maker
  approval binds to payload hash
        |
        +--> rejected -> record reason, no business mutation
        |
        v
+-------------------------------+
| Execution Worker              |
| - revalidates approval        |
| - executes atomic transaction |
| - writes immutable ledger     |
| - verifies source of record   |
+---------------+---------------+
                |
                v
[ User-facing confirmation only after verified commit ]
```

### **Queue-Based Database Schema**

Traditional table-level maker-checker implementations often add approval columns to every business table. That approach creates inconsistent review semantics and makes it difficult to audit pending approvals across systems. A cleaner pattern is to centralize approval requests while keeping business writes behind a verified execution worker.

The following PostgreSQL schema separates the approval queue from the immutable execution ledger. It stores sensitive payloads by secure reference and hash rather than dumping raw high-impact transaction details directly into an ordinary workflow table.

```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TYPE approval_workflow_status AS ENUM (
    'PENDING',
    'APPROVED',
    'REJECTED',
    'EXPIRED',
    'PROCESSING',
    'COMPLETED',
    'FAILED'
);

CREATE TYPE approval_risk_class AS ENUM (
    'LOW',
    'MEDIUM',
    'HIGH',
    'REGULATED'
);

CREATE TABLE public.approval_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID NOT NULL,
    operation_type TEXT NOT NULL,
    action_type TEXT NOT NULL,
    risk_class approval_risk_class NOT NULL,

    -- Sensitive payloads should live in controlled storage.
    -- The approval record stores the reference and integrity hash.
    payload_ref TEXT NOT NULL,
    payload_hash TEXT NOT NULL CHECK (payload_hash ~ '^sha256:[a-f0-9]{64}$'),

    policy_version TEXT NOT NULL,
    tool_manifest_hash TEXT,
    idempotency_key TEXT UNIQUE NOT NULL,

    maker_id UUID NOT NULL,
    checker_id UUID,
    status approval_workflow_status NOT NULL DEFAULT 'PENDING',

    approval_reason TEXT,
    rejection_reason TEXT,

    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    expires_at TIMESTAMPTZ NOT NULL,
    reviewed_at TIMESTAMPTZ,
    executed_at TIMESTAMPTZ,

    CONSTRAINT chk_checker_differs_from_maker CHECK (
        checker_id IS NULL OR checker_id <> maker_id
    ),

    CONSTRAINT chk_review_fields_when_approved CHECK (
        status <> 'APPROVED'
        OR (
            checker_id IS NOT NULL
            AND reviewed_at IS NOT NULL
            AND approval_reason IS NOT NULL
        )
    ),

    CONSTRAINT chk_review_fields_when_rejected CHECK (
        status <> 'REJECTED'
        OR (
            checker_id IS NOT NULL
            AND reviewed_at IS NOT NULL
            AND rejection_reason IS NOT NULL
        )
    ),

    CONSTRAINT chk_completed_requires_execution CHECK (
        status <> 'COMPLETED'
        OR executed_at IS NOT NULL
    )
);

CREATE INDEX idx_approval_requests_tenant_status
    ON public.approval_requests (tenant_id, status);

CREATE INDEX idx_approval_requests_operation_status
    ON public.approval_requests (operation_type, status);

CREATE TABLE public.approval_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    approval_request_id UUID NOT NULL
        REFERENCES public.approval_requests(id),

    event_type TEXT NOT NULL,
    actor_id UUID NOT NULL,
    event_payload JSONB NOT NULL DEFAULT '{}'::jsonb,
    event_hash TEXT NOT NULL CHECK (event_hash ~ '^sha256:[a-f0-9]{64}$'),

    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_approval_events_request
    ON public.approval_events (approval_request_id, created_at);

CREATE TABLE public.action_execution_ledger (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    approval_request_id UUID NOT NULL
        REFERENCES public.approval_requests(id),

    tenant_id UUID NOT NULL,
    operation_type TEXT NOT NULL,

    pre_action_state_hash TEXT NOT NULL,
    post_action_state_hash TEXT,
    execution_status TEXT NOT NULL,
    compensation_ref TEXT,

    executed_by UUID NOT NULL,
    executed_at TIMESTAMPTZ NOT NULL DEFAULT now(),

    ledger_entry_hash TEXT NOT NULL CHECK (ledger_entry_hash ~ '^sha256:[a-f0-9]{64}$')
);

CREATE INDEX idx_action_execution_ledger_request
    ON public.action_execution_ledger (approval_request_id);
```

This schema is not a complete banking ledger. It is a governance queue and execution ledger pattern. Financial systems still require domain-specific accounting tables, double-entry posting rules, reconciliation, settlement controls, and regulator-specific audit procedures.

### **Double-Entry Ledger, Immutability, and State Verification**

When the checker approves the request, the database executes the operation within an atomic, double-entry transactional block.39 Double-entry rules require that every financial posting writes at least one debit and one credit in equal amounts, ensuring the ledger remains balanced (Sum of Debits + Sum of Credits = 0) and mathematically preventing single-entry anomalies.39  
Once committed, transaction logs are completely immutable—historical records are never updated or deleted.39 Any corrections must be executed by posting opposing "contra" and "re-booked" entries, leaving a chronological, tamper-evident audit trail for financial regulators.39  

For high-impact operations, approval is not the same as execution. The checker approves a specific payload hash under a specific policy version. The execution worker must then re-read the approval record, verify that the payload hash still matches, confirm the approval has not expired, execute the business transaction atomically, and write an immutable execution-ledger entry. The user interface must not report completion until the post-action verifier confirms the source-of-record state.

To guarantee state verification, the platform leverages configuration parameters across distinct instance types 41:

* **Read Instance:** Uses read-only database connections to fetch users, templates, and transaction histories.41  
* **Write Instance:** Persists state-changing commands to the approval queue, ensuring every update is registered.41  
* **Batch Instance:** Automatically executes background verification, audit synchronization, and log archiving routines.41

This division ensures that the conversational interface remains blocked from speaking or displaying transaction confirmations to the user until the Write Instance confirms that the database commit has successfully settled in the core ledger.2

## **Governance Decision Register and Accountability Matrix**

Governance architecture must define who is allowed to make which AI-system decisions, what evidence they need, where the decision is enforced, and how exceptions are reviewed. Without a decision register, governance collapses into vibes, meetings, and someone named Brad saying “seems fine” in Slack. Brad is not a control.

| Governance Decision | Accountable Owner | Trigger | Required Evidence | Enforcement Surface | Review / Exception Path |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **AI System Registration** | Governance Lead / System Owner. | New AI system, material feature, model route, or agent workflow. | System card, owner, intended use, user population, data classes, model/tool inventory. | AI system inventory gate. | Architecture review for unregistered or orphaned systems. |
| **Risk Classification** | Compliance Lead with System Owner. | New use case or material change in affected population, autonomy, or data class. | Impact assessment, harm analysis, regulatory mapping, human oversight plan. | Release gate and policy bundle. | Risk acceptance by accountable executive for exceptions. |
| **Model Route Approval** | Model Owner / AI Operations Lead. | New model, provider, adapter, quantization profile, or fallback route. | Evaluation results, capability profile, data-processing terms, supply-chain verification. | Gateway route manifest. | Route exception expires and requires revalidation. |
| **Tool Permission Approval** | Tool Owner / Security Lead. | New tool, new scope, new mutation class, or expanded credential. | Tool contract, side-effect class, schema, auth model, idempotency plan, verification predicate. | Tool registry and gateway policy. | Human review for high-impact or broad-scope tools. |
| **Human Review Threshold** | Governance Lead / Workflow Owner. | High-impact workflow or repeated uncertainty. | Risk class, confidence thresholds, historical review outcomes, reviewer capacity. | Maker-checker queue and escalation router. | Periodic threshold review; exceptions logged. |
| **Corpus / Data Source Approval** | Data Owner / Retrieval Owner / Privacy Lead. | New corpus, ingestion route, vendor data source, or tenant-shared index. | Provenance, license, sensitivity, retention, data quality, source authority. | Corpus lifecycle state and retrieval eligibility policy. | Quarantine and review for unknown-provenance sources. |
| **Vendor / SaaS Approval** | Procurement, Security, Privacy, Legal. | New AI vendor, embedded-AI feature, subprocessor, or data-sharing route. | Security assessment, DPA, training-use terms, deletion/export rights, audit evidence. | Procurement gate and vendor registry. | Risk acceptance and compensating controls if approved with gaps. |
| **Telemetry Retention Policy** | Privacy Lead / SRE / Governance Lead. | New trace field, payload reference, audit requirement, or incident evidence class. | Data classification, purpose, retention need, access model, deletion obligations. | Telemetry collector and secure payload vault. | Legal/privacy review for extended sensitive retention. |
| **Incident Reporting Decision** | Incident Commander / Compliance / Legal. | SEV-0/SEV-1, privacy event, regulated system incident, or customer impact. | Incident manifest, scope analysis, affected population, legal/regulatory triggers. | Incident management system and notification workflow. | Counsel and executive approval where required. |
| **Decommission / Sunset Decision** | System Owner / Governance Lead. | Vendor exit, model retirement, unacceptable residual risk, or system obsolescence. | Data export plan, deletion certificate, replacement route, archival evidence. | Lifecycle registry and access controls. | Formal exception if system must remain temporarily active. |

The register should be machine-readable where possible. Each decision should bind to policy version, owner, timestamp, evidence references, affected systems, and expiration/review date.

## **Shadow AI Discovery and Context Ingestion Hygiene**

Shadow AI is the use of unapproved AI tools, browser plugins, SaaS products, local agents, or automation scripts outside formal IT, security, privacy, procurement, or governance review. The risk is not that employees are curious. The risk is that sensitive prompts, proprietary code, customer records, regulated data, credentials, or internal documents move into unapproved systems with unknown retention, training, security, and contractual terms.

Shadow AI discovery must itself be governed. Monitoring should be lawful, disclosed where required, proportionate, minimized, access-controlled, and reviewed through privacy and security governance. The goal is to identify risky data flows and route users toward approved tools—not to build an employee panopticon with a compliance sticker slapped on the side.

### **Multi-Layered Shadow AI Discovery Stack**

```text
SHADOW AI DISCOVERY STACK

[ Approved AI Inventory ]
        |
        v
[ Discovery Signals ]
  DNS / SNI / CASB / SaaS logs
  endpoint DLP events
  identity and OAuth grants
  expense/vendor records
  browser extension inventory
  developer package/config scans
        |
        v
[ Risk Correlation ]
  data sensitivity | destination | user role
  volume | frequency | contract status
  vendor approval state
        |
        v
[ Governance Response ]
  allow approved tool
  coach user to safe path
  require vendor review
  block high-risk transfer
  open incident if sensitive data exposed
```

| Discovery Layer | What It Detects | Governance Guardrail |
| :---- | :---- | :---- |
| **Network / CASB Signals** | Connections to unapproved AI services, unusual upload volume, risky SaaS destinations. | Use destination and volume metadata where sufficient; restrict deeper inspection to policy-approved cases. |
| **Endpoint / DLP Signals** | Sensitive data copied, uploaded, or pasted into unapproved browser tabs or applications. | Minimize content capture; use classification events and redacted snippets rather than broad raw collection. |
| **Identity / OAuth Monitoring** | Employees authorizing AI apps, plugins, or connectors with broad scopes. | Require OAuth scope review, app approval, and automatic revocation for prohibited grants. |
| **Browser / Extension Inventory** | AI browser extensions, local agents, or automation plugins installed outside approval. | Maintain allowlist; notify users; block high-risk extensions with data access. |
| **Developer Config Scans** | Unapproved MCP servers, API keys, agent configs, dependency hooks, or local tool servers. | Scan repositories and endpoint configs for risky patterns without collecting unrelated personal data. |
| **Expense / Vendor Records** | Emerging AI SaaS purchases outside procurement review. | Route to vendor governance instead of punishing early adopters by default. |
| **Support / User Reports** | Teams requesting tools or reporting gaps in approved options. | Use demand signals to improve approved golden paths. |

### **Context Ingestion Hygiene and Metadata Context Manifests**

Authorized RAG and context-ingestion pipelines must treat external documents, webpages, emails, tickets, transcripts, and uploaded files as untrusted data. Before content enters retrieval, memory, or the model-facing context window, the ingestion system should verify source, ownership, sensitivity, tenant scope, transformation history, and lifecycle state.

| Hygiene Control | Purpose |
| :---- | :---- |
| **Sandboxed Parsing** | Parse PDFs, Office files, images, HTML, XML, SVG, archives, and other complex formats in constrained, network-restricted environments. |
| **Active Content Removal** | Strip or disable macros, scripts, external entity resolution, hidden text, zero-width payloads, and unsafe embedded objects. |
| **Source and Ownership Binding** | Attach source ID, owner, tenant, license, sensitivity, retention, and lifecycle state. |
| **Transformation Lineage** | Record parser version, chunking version, embedding model, summary generator, and derived artifact hashes. |
| **Permission Inheritance** | Ensure chunks, embeddings, summaries, citations, and cache entries inherit source permissions and classifications. |
| **Retrieval Eligibility Policy** | Admit only active, authorized, non-quarantined sources into retrieval candidate sets. |
| **Prompt-Injection Containment** | Treat document text as evidence, not authority; untrusted content cannot grant tool permissions or override policy. |

A Context Manifest should travel with every model-facing evidence object. It should include source identity, tenant, ACL, sensitivity, parser/chunking lineage, lifecycle state, freshness, and policy version. Context assembly is an access-control decision, not a formatting step.

## **Regulatory Synchronization: Global Compliance Timelines**

AI compliance changes over time. Governance architecture should therefore maintain a regulatory register that distinguishes current legal obligations, future effective dates, provisional political agreements, draft guidance, and organization-specific contractual commitments.

The European Union Artificial Intelligence Act entered into force on August 1, 2024 and applies progressively. Organizations should track two planning views:

1. **Current-law baseline:** obligations and dates in the adopted AI Act and official EU implementation guidance.
2. **Provisional Omnibus planning track:** as of June 2026, EU institutions had reached a provisional agreement on targeted Digital Omnibus changes, including revised timelines for certain high-risk AI obligations. Until formal adoption and publication are complete, organizations should treat these changes as planning assumptions, not as a substitute for legal review.

### **EU AI Act Timeline Planning Matrix**

| Date | Status | Provision / Topic | Governance Action |
| :---- | :---- | :---- | :---- |
| **August 1, 2024** | In force. | EU AI Act enters into force. | Establish AI system inventory, governance owner, regulatory register, and risk-classification workflow. |
| **February 2, 2025** | Current-law baseline. | AI literacy duties and prohibited AI practices begin applying. | Train relevant personnel; block prohibited practices in policy-as-code and procurement gates. |
| **August 2, 2025** | Current-law baseline. | GPAI and AI governance obligations begin applying under the phased timeline. | Maintain model/provider inventory, documentation, vendor evidence, and governance process. |
| **August 2, 2026** | Current-law baseline for broad applicability, subject to exceptions and any formally adopted amendments. | General application date for many AI Act obligations. Article 50 transparency obligations are especially relevant to user-facing AI interactions. | Implement user disclosures, AI interaction labeling, incident and evidence records, and deployer controls for applicable systems. |
| **December 2, 2026** | Provisional Omnibus planning track. | Reported deadline for certain synthetic-content transparency / watermarking-related obligations and new prohibitions under the provisional agreement. | Track final legal text; evaluate watermarking, labeling, output provenance, and prohibited-content controls. Do not hardcode C2PA as the only acceptable mechanism. |
| **December 2, 2027** | Provisional Omnibus planning track. | Reported delayed deadline for many standalone Annex III high-risk AI system obligations. | Continue readiness work: risk management, human oversight, logging, monitoring, post-market controls, and deployer obligations. |
| **August 2, 2028** | Provisional Omnibus planning track. | Reported delayed deadline for product-regulated Annex I high-risk AI systems. | Coordinate with product regulatory, conformity assessment, technical documentation, and safety teams. |

### **Article 26: Deployer Obligations for High-Risk Systems**

For high-risk AI systems, deployers should prepare controls for:

| Obligation Area | Governance Implementation |
| :---- | :---- |
| **Use According to Instructions** | Maintain provider instructions, deployment constraints, operator training, and system-use policies. |
| **Human Oversight** | Assign competent, authorized, supported natural persons for oversight where required. |
| **Input Data Relevance** | Where the deployer controls input data, ensure inputs are relevant and sufficiently representative for the intended purpose. |
| **Monitoring and Suspension** | Monitor operation; suspend use and notify appropriate parties when serious risk is identified. |
| **Log Retention** | Retain logs generated by the high-risk system under deployer control for the legally required period, with at least six months where applicable. |
| **Workplace Notice** | Inform workers and representatives before high-risk AI systems are used in workplace contexts where required. |

This section should be reviewed periodically. Legal deadlines, guidance, standards, and enforcement interpretations can change; the governance architecture must preserve update ownership instead of fossilizing one timeline into the system like a compliance mosquito in amber.

## **The Platform Engineering "Golden Path" to AI Governance**

The goal of a modern platform engineering team is to balance developer velocity with enterprise governance by architecting a **Golden Path**—an opinionated, well-documented, and supported way of building and deploying software.6 Positioned as a standardized self-service route, the Golden Path makes the secure, compliant choice the easiest choice for developers, integrating compliance checks directly into the environment.4

```text
AI GOVERNANCE GOLDEN PATH

[ Developer / Team ]
  declares use case, data class, autonomy level, tools, users, deployment target
        |
        v
+-------------------------------+
| Self-Service Intake           |
| - AI system registration      |
| - risk questionnaire          |
| - data/source declarations    |
+---------------+---------------+
                |
                v
+-------------------------------+
| Template and IaC Generation   |
| - approved service patterns   |
| - logging and telemetry       |
| - sandbox and gateway defaults|
+---------------+---------------+
                |
                v
+-------------------------------+
| Policy-as-Code Gates          |
| - OPA / Kyverno / CI checks   |
| - model/tool/vendor allowlist |
| - cost and security checks    |
+---------------+---------------+
                |
                v
+-------------------------------+
| Evaluation and Review Gates   |
| - golden tests                |
| - security/privacy review     |
| - human approval if required  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Production Runtime Controls   |
| - gateway policy enforcement  |
| - telemetry and audit trails  |
| - incident and rollback hooks |
+---------------+---------------+
                |
                v
+-------------------------------+
| Continuous Governance         |
| - drift monitoring            |
| - vendor review               |
| - incident-to-eval updates    |
| - periodic risk reassessment  |
+-------------------------------+
```

### **The Four Qualities of a Golden Path**

To foster innovation while maintaining structural control, every Golden Path must satisfy four core architectural properties 65:

* **Optional by exception, not by bypass:** Engineering teams may request an alternative approved path when a custom scenario requires it. Mandatory security, privacy, regulatory, and high-impact governance controls remain enforceable regardless of path. Exceptions should be logged, time-bounded, owned, and reviewed.
* **Transparent:** The underlying technologies, security scanners, and container configurations are transparently visible, giving developers the opportunity to learn how the background automation operates.65  
* **Extensible:** The templates are highly modular, allowing teams to add new capabilities, plugins, or evaluation libraries without rewriting the pipeline core.65  
* **Customizable:** The configurations can be adjusted based on the team's existing experience and the business risk of the target application.65

### **The Four Pillars of Platform Control**

A mature, autonomous platform manages compliance across four distinct, AI-driven control mechanisms 4:

1. **Golden Paths:** Self-tuning, automated roadmaps that generate compliant infrastructure and service templates from natural language intents.4  
2. **Guardrails:** Programmatic checks (such as OPA and Kyverno) that prevent non-compliant deployments from reaching production.4  
3. **Safety Nets:** Automated drift remediation systems that continuously scan live environments against desired states, deploying security patches and cleaning up "zombie infrastructure" using embedded time-to-live policies to reduce cloud waste.4  
4. **Manual Review Workflows:** Strategic points of human friction where agents generate comprehensive risk reports, check cost forecasts, and present reviewers with simple risk scores and go/no-go recommendations, ensuring accountability where judgment is required.4

### **FinOps and Security Inner-Loop Integrations**

By shifting governance to "Step Zero" of the developer workflow, the Golden Path prevents compliance failures before resources are ever provisioned.6 OPA-driven security guardrails evaluate Terraform plans to detect contextual privilege escalation paths and verify container signatures.20  
Simultaneously, FinOps guardrails analyze infrastructure changes to output real-time cost impact predictions and enforce resource-tagging policies before merging pull requests, eliminating post-deployment billing surprises and keeping the organization's cloud footprint secure, lean, and auditable.35

## **Cross-Canon Handoff Map**

AI-ENG-AD defines the governance control layer for the canon: policy ownership, accountability, risk classification, approval workflows, audit obligations, procurement gates, regulatory synchronization, and lifecycle governance. It consumes operational evidence from upstream systems and sends enforceable decisions back into gateways, registries, release gates, and runtime monitors.

| Canon Report | Governance Input to AD | AD Decision / Control | Downstream Enforcement |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context object metadata, memory scope, tenure, authority, and state ownership. | Defines governance rules for memory, context retention, and state accountability. | Context compiler, memory policy, retention registry. |
| **AI-ENG-D — Corpus Engineering** | Source authority, data ownership, lifecycle state, provenance, and data quality. | Determines approved data sources, sensitivity rules, retention, and corpus ownership. | Corpus registry, ingestion gates, data lifecycle controls. |
| **AI-ENG-E — Retrieval Pipeline** | Retrieval eligibility, evidence packets, citation bundles, permission filters. | Defines evidence sufficiency, citation requirements, and retrieval governance controls. | Retrieval policy, RLS/ACL enforcement, citation verification gates. |
| **AI-ENG-F — Freshness and Conflict Detection** | Freshness state, conflict packets, source versions, bitemporal records. | Defines acceptable staleness, conflict escalation, and source-of-record hierarchy. | Freshness gates, conflict-resolution policy, user disclosure rules. |
| **AI-ENG-L — Serving Architecture** | Model routes, provider profiles, serving images, latency/cost envelopes. | Approves route classes, fallback eligibility, and model-provider governance. | Gateway route manifests, release gates, fallback policy. |
| **AI-ENG-M — Agentic Orchestration** | Autonomy boundaries, workflow graphs, loop budgets, agent state. | Defines permissible autonomy by risk tier and escalation requirements. | Agent policy, workflow caps, escalation router. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, side-effect class, tool manifests, argument validators. | Approves tool exposure, credential scope, and human-review thresholds. | Tool registry, OPA policy, maker-checker queue. |
| **AI-ENG-O — Action Verification** | Idempotency keys, action ledgers, verification predicates, state hashes. | Defines when completion claims, compensation, and audit obligations are required. | Post-action auditor, source-of-record verification, action ledger. |
| **AI-ENG-P — Multimodal Understanding** | Parser lineage, OCR/layout confidence, media evidence, coordinates. | Defines evidentiary adequacy and review rules for document/media-derived claims. | Evidence adequacy gate, parser approval, human review. |
| **AI-ENG-Q — Speech / Realtime** | Transcript confidence, voice confirmation, interruption, degraded voice state. | Defines when voice channels can authorize or must fall back to visual/text confirmation. | Confirmation policy, high-impact voice gate, transcript retention. |
| **AI-ENG-R — UI Agents** | Browser actions, DOM/screenshot hashes, UI verification, automation uncertainty. | Defines UI-agent autonomy limits and user handoff requirements. | UI automation gate, browser sandbox, action verification. |
| **AI-ENG-S — Production Pathologies** | Failure taxonomy, malformed output, false success, loop pathologies. | Defines which pathology classes require release gates, incidents, or governance review. | Regression tests, incident triggers, reliability SLOs. |
| **AI-ENG-T — Boundary Defense** | Authority hierarchy, tenant isolation, cache scope, prompt-injection events. | Defines non-negotiable security boundaries and escalation paths. | Gateway policy, RLS, cache isolation, fail-closed controls. |
| **AI-ENG-U — Supply Chain Security** | Model signatures, SBOM/AI-BOM, dependency manifests, sandbox profiles. | Defines approved artifacts, vendor requirements, and supply-chain evidence. | Artifact registry, procurement gate, runtime loader policy. |
| **AI-ENG-V — Resource Abuse** | Budget limits, token burn, denial-of-wallet signals, loop caps. | Defines spend authority, quota policy, and abuse-response obligations. | Budget gateway, rate limits, cost approvals. |
| **AI-ENG-W — UX Resilience** | Degraded-mode states, fallback contracts, continuity state, disclosure records. | Defines disclosure, consent, and fallback eligibility by risk tier. | UX state cards, fallback policy, human escalation. |
| **AI-ENG-X — User Trust** | Transparency, contestability, evidence presentation, user correction signals. | Defines user-facing explanation, contestability, and trust-calibration obligations. | UI disclosures, evidence views, appeal/review paths. |
| **AI-ENG-Y — Human Review** | Approval queues, maker-checker records, reviewer decisions, escalation packages. | Defines when human review is mandatory and how review quality is audited. | Approval workflows, reviewer governance, override logs. |
| **AI-ENG-Z — Strategic Telemetry** | Trace IDs, telemetry fields, redaction metadata, drift metrics, SLOs. | Defines required telemetry, retention, access control, and audit scope. | Telemetry collector, secure payload vault, dashboards. |
| **AI-ENG-AA — Evaluations** | Golden sets, canaries, rubrics, adversarial tests, calibration artifacts. | Defines release gates, acceptance criteria, and evaluation ownership. | CI/CD gates, eval registry, model/prompt release approval. |
| **AI-ENG-AB — Verification Artifacts** | Trace manifests, secure references, replay artifacts, evidence trails. | Defines auditability, reproducibility, and evidence-retention requirements. | Audit store, incident package, replay harness. |
| **AI-ENG-AC — AI Operations** | Incident records, runbooks, containment actions, postmortems, corrective actions. | Defines incident reporting, severity governance, accountability, and closure requirements. | Incident workflow, corrective-action tracking, incident-to-eval loop. |
| **AI-ENG-AJ — Reference Architectures** | Blueprint patterns, gateways, registries, sandboxes, audit stores. | Converts governance decisions into default implementation architecture. | Golden paths, platform templates, reference deployments. |

## **Durable Principles of AI Governance Architecture**

To future-proof the enterprise and maintain compliant operations, AI systems architects must adhere to five durable design principles:

### **I. Governance Must Live Outside the Model's Cognitive Path**

Relying on system prompts, inline instructions, or model self-policing to enforce safety boundaries, cost limits, or structural compliance is a critical vulnerability.1 Language models are probabilistic generators whose attention is easily diluted by long contexts, complex reasoning steps, or adversarial prompts.1 True governance must be programmatically enforced outside the model's cognitive path using a centralized, budget-aware API gateway and isolated runtime verification monitors.1

### **II. Context Ingestion is an Access-Control Gate**

No document, database row, memory, or semantic cache entry must enter the model-facing context window unless the system has verified its origin, tenant scope, and user-level permissions.1 Enforcing security filters in application code is an anti-pattern prone to race conditions and logic bugs.1 SaaS platforms must isolate multi-tenant data directly at the database engine layer using Row-Level Security (RLS) policies and partition vector indexes per customer.1

### **III. Spoken and Generated Claims Must Never Outrun Database Realities**

Dialogue orchestrators and conversational interfaces must never vocalize or display a completion confirmation (e.g., stating "Your transfer of $500 is complete") until the underlying API mutation has successfully committed and been verified by a Post-Action Auditor.2 All high-impact, state-changing operations must utilize the Saga distributed transaction pattern to coordinate compensatable, pivot, and retriable steps, ensuring eventual consistency across services before outputs are rendered on the viewport.2

### **IV. Every Consequential Action Requires a Verifiable Evidence Trail**

Every automated transaction, policy override, user correction, and human review decision must leave behind a tamper-evident evidence trail. That trail should bind the prompt version, model route, policy decision, source/evidence identifiers, tool payload hash, approval record, pre/post state hashes, and secure payload references where raw evidence is required.

Governance evidence should be sufficient to audit and replay decisions without dumping raw prompts, context chunks, tool arguments, credentials, or sensitive user data into ordinary logs. Raw evidence belongs in controlled storage with access controls, retention limits, and access auditing.

### **V. Security Controls Must Retain Rigor Under Degradation**

When backend services degrade, model providers timeout, or rate limits are hit, the platform must degrade gracefully along explicit quality, cost, and latency dimensions while keeping its security boundaries strict.5 A system must never fail-open, bypass permission checks, or disable tenant filters to maintain service availability.1 If compliance or safety floors cannot be satisfied by available fallback paths, the architecture must fail-closed, secure the active state, and escalate the transaction to human review queues.1

#### **Works cited**

1. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
2. [KNOWLEDGE] - AI Engineering - Volume 9. Z-AB Observability, Evaluation, and Verification  
3. Maker-Checker implementation guide for secure FinTech systems, accessed June 12, 2026, [https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029](https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029)  
4. The autonomous enterprise and the four pillars of platform control: 2026 forecast | CNCF, accessed June 12, 2026, [https://www.cncf.io/blog/2026/01/23/the-autonomous-enterprise-and-the-four-pillars-of-platform-control-2026-forecast/](https://www.cncf.io/blog/2026/01/23/the-autonomous-enterprise-and-the-four-pillars-of-platform-control-2026-forecast/)  
5. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
6. Architecting Trust: The Blueprint for a "Golden Standard" Software Supply Chain - Harness, accessed June 12, 2026, [https://www.harness.io/blog/architecting-trust-the-blueprint-for-a-golden-standard-software-supply-chain](https://www.harness.io/blog/architecting-trust-the-blueprint-for-a-golden-standard-software-supply-chain)  
7. How to map AI governance controls to NIST AI RMF functions - Prediction Guard, accessed June 12, 2026, [https://predictionguard.com/blog/how-to-map-ai-governance-controls-to-nist-ai-rmf-functions](https://predictionguard.com/blog/how-to-map-ai-governance-controls-to-nist-ai-rmf-functions)  
8. ISO 42001 Statement of Applicability Explained - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/statement-of-applicability/](https://www.isms.online/iso-42001/statement-of-applicability/)  
9. ISO 42001 Statement Of Applicability (SoA) | Detailed Guide - Cyberzoni.com, accessed June 12, 2026, [https://cyberzoni.com/iso-42001-soa/](https://cyberzoni.com/iso-42001-soa/)  
10. ISO 42001 Annex A Controls Explained - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/annex-a-controls/](https://www.isms.online/iso-42001/annex-a-controls/)  
11. Understanding the NIST AI RMF (AI Risk Management Framework) - Dastra.eu, accessed June 12, 2026, [https://www.dastra.eu/en/blog/understanding-the-nist-ai-risk-management-framework/59940](https://www.dastra.eu/en/blog/understanding-the-nist-ai-risk-management-framework/59940)  
12. AI RMF Core - AIRC - NIST AI Resource Center, accessed June 12, 2026, [https://airc.nist.gov/airmf-resources/airmf/5-sec-core/](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)  
13. NIST AI RMF: Govern, Map, Measure, Manage as a Process | BA Copilot, accessed June 12, 2026, [https://ba-copilot.com/nist-ai-rmf](https://ba-copilot.com/nist-ai-rmf)  
14. ISO 42001 Annex A Controls List: A.5, A.6, A.7, A.8 (38 Controls) | Mindset Cyber, accessed June 12, 2026, [https://mindsetcyber.com.au/iso-42001-controls-list/](https://mindsetcyber.com.au/iso-42001-controls-list/)  
15. ISO 42001 Annex Control Objectives And Controls | Gabriel Consultant Limited, accessed June 12, 2026, [https://gabriel.hk/iso-42001-annex-control-objectives-and-controls/](https://gabriel.hk/iso-42001-annex-control-objectives-and-controls/)  
16. Process Automation and Cross-System Data Integrations | Watabe Digital Insights, accessed June 12, 2026, [https://watabedigital.co.tz/insights/business-process-automation-tanzania/automation-and-system-integration/](https://watabedigital.co.tz/insights/business-process-automation-tanzania/automation-and-system-integration/)  
17. Ready for ISO 42001? Let's Talk Next Steps | Drata Help Center, accessed June 12, 2026, [https://help.drata.com/en/articles/12591328-ready-for-iso-42001-let-s-talk-next-steps](https://help.drata.com/en/articles/12591328-ready-for-iso-42001-let-s-talk-next-steps)  
18. Building a Least-Privilege AI Agent Gateway for Infrastructure Automation with MCP, OPA, and Ephemeral Runners - InfoQ, accessed June 12, 2026, [https://www.infoq.com/articles/building-ai-agent-gateway-mcp/](https://www.infoq.com/articles/building-ai-agent-gateway-mcp/)  
19. What Is Policy-As-Code? Benefits & Best Practices - Apiiro, accessed June 12, 2026, [https://apiiro.com/glossary/policy-as-code-2/](https://apiiro.com/glossary/policy-as-code-2/)  
20. Policy as code: The platform engineer's guide to automated governance and compliance, accessed June 12, 2026, [https://platformengineering.org/blog/policy-as-code](https://platformengineering.org/blog/policy-as-code)  
21. Open Policy Agent (OPA), accessed June 12, 2026, [https://openpolicyagent.org/docs](https://openpolicyagent.org/docs)  
22. Principled Evolution (GOPAL & AICertify) - Open Policy Agent, accessed June 12, 2026, [https://openpolicyagent.org/ecosystem/entry/principled-evolution](https://openpolicyagent.org/ecosystem/entry/principled-evolution)  
23. How to Validate Policy-as-Code Without Breaking Builds (Even When AI Writes the Code), accessed June 12, 2026, [https://jfrog.com/blog/how-to-validate-policy-as-code/](https://jfrog.com/blog/how-to-validate-policy-as-code/)  
24. What is Open Policy Agent (OPA)? Best Practices + Applications - Wiz, accessed June 12, 2026, [https://www.wiz.io/academy/application-security/open-policy-agent-opa](https://www.wiz.io/academy/application-security/open-policy-agent-opa)  
25. What is Policy-as-Code? - Sysdig, accessed June 12, 2026, [https://www.sysdig.com/learn-cloud-native/what-is-policy-as-code](https://www.sysdig.com/learn-cloud-native/what-is-policy-as-code)  
26. SARC: A Governance-by-Architecture Framework for Agentic AI Systems - arXiv, accessed June 12, 2026, [https://arxiv.org/pdf/2605.07728](https://arxiv.org/pdf/2605.07728)  
27. SARC: A Governance-by-Architecture Framework for Agentic AI Systems Compiling Regulatory Obligations into Runtime Constraints - arXiv, accessed June 12, 2026, [https://arxiv.org/html/2605.07728v1](https://arxiv.org/html/2605.07728v1)  
28. SARC: A Governance-by-Architecture Framework for Agentic AI, accessed June 12, 2026, [https://www.aimodels.fyi/papers/arxiv/sarc-governance-by-architecture-framework-agentic-ai](https://www.aimodels.fyi/papers/arxiv/sarc-governance-by-architecture-framework-agentic-ai)  
29. Gaston Besanson's research works - ResearchGate, accessed June 12, 2026, [https://www.researchgate.net/scientific-contributions/Gaston-Besanson-2340161255](https://www.researchgate.net/scientific-contributions/Gaston-Besanson-2340161255)  
30. Outsider Oversight: Designing a Third Party Audit Ecosystem for AI Governance | Request PDF - ResearchGate, accessed June 12, 2026, [https://www.researchgate.net/publication/362295642_Outsider_Oversight_Designing_a_Third_Party_Audit_Ecosystem_for_AI_Governance](https://www.researchgate.net/publication/362295642_Outsider_Oversight_Designing_a_Third_Party_Audit_Ecosystem_for_AI_Governance)  
31. (PDF) SARC: A Governance-by-Architecture Framework for Agentic, accessed June 12, 2026, [https://www.researchgate.net/publication/404712995_SARC_A_Governance-by-Architecture_Framework_for_Agentic_AI_Systems](https://www.researchgate.net/publication/404712995_SARC_A_Governance-by-Architecture_Framework_for_Agentic_AI_Systems)  
32. Article 26: Obligations of Deployers of High-Risk AI Systems | EU Artificial Intelligence Act, accessed June 12, 2026, [https://artificialintelligenceact.eu/article/26/](https://artificialintelligenceact.eu/article/26/)  
33. The CISO's Guide to EU AI Act Compliance - Cequence AI, accessed June 12, 2026, [https://www.cequence.ai/eu-ai-act.html](https://www.cequence.ai/eu-ai-act.html)  
34. AI Data Privacy vs SaaS: What Changes For Enterprise IT In 2026 - CloudNuro.ai, accessed June 12, 2026, [https://www.cloudnuro.ai/blog/ai-data-privacy-vs-saas-what-changes-for-enterprise-it-in-2026](https://www.cloudnuro.ai/blog/ai-data-privacy-vs-saas-what-changes-for-enterprise-it-in-2026)  
35. How AI Is Becoming the Governance Layer Inside Your CI/CD Pipeline - Medium, accessed June 12, 2026, [https://medium.com/@mrigank.online/how-ai-is-becoming-the-governance-layer-inside-your-ci-cd-pipeline-ce485bb96a21](https://medium.com/@mrigank.online/how-ai-is-becoming-the-governance-layer-inside-your-ci-cd-pipeline-ce485bb96a21)  
36. Immutable Payment Audit Trails: Storage, Discovery, and Audits - Chequedb, accessed June 12, 2026, [https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires](https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires)  
37. Maker-checker spec: discussion, approach, design - Mifos X - MifosForge, accessed June 12, 2026, [https://mifosforge.jira.com/wiki/pages/viewpage.action?pageId=27164690&navigatingVersions=true](https://mifosforge.jira.com/wiki/pages/viewpage.action?pageId=27164690&navigatingVersions=true)  
38. Maker-Checker Pattern: Dual-Control System Implementation - Opcito, accessed June 12, 2026, [https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems](https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems)  
39. Core Banking ERP Part 2 – Data Model, Ledger & Core Products - ClefinCode, accessed June 12, 2026, [https://clefincode.com/blog/global-digital-vibes/en/core-banking-erp-part-2-data-model-ledger-core-products](https://clefincode.com/blog/global-digital-vibes/en/core-banking-erp-part-2-data-model-ledger-core-products)  
40. Double Entry Accounting in a Relational Database | by Robert Chanphakeo | Medium, accessed June 12, 2026, [https://medium.com/@RobertKhou/double-entry-accounting-in-a-relational-database-2b7838a5d7f8](https://medium.com/@RobertKhou/double-entry-accounting-in-a-relational-database-2b7838a5d7f8)  
41. Fineract Platform Documentation, accessed June 12, 2026, [https://fineract.apache.org/docs/current/](https://fineract.apache.org/docs/current/)  
42. AI Contracting: Practical Legal Guidance for Growing Businesses - Koley Jessen, accessed June 12, 2026, [https://www.koleyjessen.com/insights/publications/ai-contracting-practical-legal-guidance-for-growing-businesses](https://www.koleyjessen.com/insights/publications/ai-contracting-practical-legal-guidance-for-growing-businesses)  
43. AI Vendor Risk Management: Assessing Third Party AI Suppliers - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/ai-vendor-risk/](https://www.isms.online/iso-42001/ai-vendor-risk/)  
44. AI Contract Clauses That Reduce Vendor, Data, and Liability Risk, accessed June 12, 2026, [https://hernanhuwyler.wordpress.com/2026/03/12/ai-procurement-controls/](https://hernanhuwyler.wordpress.com/2026/03/12/ai-procurement-controls/)  
45. Enterprise AI Vendor RFP: 40 Questions to Ask (2026) - Worqlo, accessed June 12, 2026, [https://worqlo.com/blog/enterprise-ai-vendor-rfp-questions/](https://worqlo.com/blog/enterprise-ai-vendor-rfp-questions/)  
46. AI Clauses In Contracts: The Practical Guide For 2025 - Tascon – Legal, accessed June 12, 2026, [https://tasconlegal.com/ai-clauses-in-contracts-the-practical-guide-for-2025/](https://tasconlegal.com/ai-clauses-in-contracts-the-practical-guide-for-2025/)  
47. Vendor Agreement Review: The 6 Clauses That Decide the Risk - GC AI, accessed June 12, 2026, [https://gc.ai/blog/vendor-agreement-review](https://gc.ai/blog/vendor-agreement-review)  
48. The privacy line AI platforms are crossing with user data - Regolo.AI, accessed June 12, 2026, [https://regolo.ai/the-privacy-line-ai-platforms-are-crossing-with-user-data/](https://regolo.ai/the-privacy-line-ai-platforms-are-crossing-with-user-data/)  
49. What Is Shadow AI? Detection, Risks and Governance in 2026 - Forcepoint, accessed June 12, 2026, [https://www.forcepoint.com/blog/insights/what-is-shadow-ai](https://www.forcepoint.com/blog/insights/what-is-shadow-ai)  
50. What Is Shadow AI? Definition, Risks & Governance Strategies - SentinelOne, accessed June 12, 2026, [https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-shadow-ai/](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-shadow-ai/)  
51. Shadow AI explained: risks, costs, and enterprise governance - Vectra AI, accessed June 12, 2026, [https://www.vectra.ai/topics/shadow-ai](https://www.vectra.ai/topics/shadow-ai)  
52. Shadow AI: How Unmonitored Tools Bypass Security and Enter Your Business, accessed June 12, 2026, [https://compassmsp.com/resources/articles/how-unmonitored-ai-tools-are-entering-your-business](https://compassmsp.com/resources/articles/how-unmonitored-ai-tools-are-entering-your-business)  
53. What Is Shadow AI? Definition | Proofpoint US, accessed June 12, 2026, [https://www.proofpoint.com/us/threat-reference/shadow-ai](https://www.proofpoint.com/us/threat-reference/shadow-ai)  
54. How to Create a Corporate AI Policy for Your Organization | Otter.ai, accessed June 12, 2026, [https://otter.ai/blog/corporate-ai-policy](https://otter.ai/blog/corporate-ai-policy)  
55. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
56. How to Get AI Act-Ready: A 2026 Compliance Guide | Dawiso Blog, accessed June 12, 2026, [https://www.dawiso.com/blog-post/how-to-get-ai-act-ready-2026](https://www.dawiso.com/blog-post/how-to-get-ai-act-ready-2026)  
57. EU AI Act High-Risk Deadline: Enterprise Readiness Gap - Lab Space, accessed June 12, 2026, [https://labs.cloudsecurityalliance.org/research/csa-research-note-eu-ai-act-high-risk-compliance-deadline-20/](https://labs.cloudsecurityalliance.org/research/csa-research-note-eu-ai-act-high-risk-compliance-deadline-20/)  
58. AI Act Update: EU Resolves to Change Rules and Extend Deadlines, accessed June 12, 2026, [https://www.lw.com/en/insights/ai-act-update-eu-resolves-to-change-rules-and-extend-deadlines](https://www.lw.com/en/insights/ai-act-update-eu-resolves-to-change-rules-and-extend-deadlines)  
59. AI Act reloaded? What the latest AI Act changes mean in practice - Stibbe, accessed June 12, 2026, [https://www.stibbe.com/publications-and-insights/ai-act-reloaded-what-the-latest-ai-act-changes-mean-in-practice](https://www.stibbe.com/publications-and-insights/ai-act-reloaded-what-the-latest-ai-act-changes-mean-in-practice)  
60. EU AI Act Update: Timeline Relief, Targeted Simplification, and New Prohibitions, accessed June 12, 2026, [https://www.insideprivacy.com/artificial-intelligence/eu-ai-act-update-timeline-relief-targeted-simplification-and-new-prohibitions/](https://www.insideprivacy.com/artificial-intelligence/eu-ai-act-update-timeline-relief-targeted-simplification-and-new-prohibitions/)  
61. EU AI Act enforcement starts in 75 days - affects any team building AI agents for European clients : r/artificial - Reddit, accessed June 12, 2026, [https://www.reddit.com/r/artificial/comments/1tgf0gm/eu_ai_act_enforcement_starts_in_75_days_affects/](https://www.reddit.com/r/artificial/comments/1tgf0gm/eu_ai_act_enforcement_starts_in_75_days_affects/)  
62. The EU AI Act's Transparency Rules: A Practical Guide to Article 50, accessed June 12, 2026, [https://artificialintelligenceact.eu/transparency-rules-article-50/](https://artificialintelligenceact.eu/transparency-rules-article-50/)  
63. EU Deepfake Rules in AI Act Will Affect More Businesses Than Usually Expected, accessed June 12, 2026, [https://www.potomaclaw.com/news-EU-Deepfake-Rules-in-AI-Act-Will-Affect-More-Businesses-Than-Usually-Expected](https://www.potomaclaw.com/news-EU-Deepfake-Rules-in-AI-Act-Will-Affect-More-Businesses-Than-Usually-Expected)  
64. EU AI Act Compliance: Complete Deadlines Guide 2025 - Tech Jacks Solutions, accessed June 12, 2026, [https://techjacksolutions.com/ai-brief/eu-ai-act-three-deadlines-three-compliance-programs-which-on/](https://techjacksolutions.com/ai-brief/eu-ai-act-three-deadlines-three-compliance-programs-which-on/)  
65. What is a Golden Path for software development? - Red Hat, accessed June 12, 2026, [https://www.redhat.com/en/topics/platform-engineering/golden-paths](https://www.redhat.com/en/topics/platform-engineering/golden-paths)

---

# AI-ENG-AE — Sustainable AI - Energy, Infrastructure Efficiency & Lifecycle Impact

## **Conceptual Glossary**

Sustainable AI architecture demands a rigorous, standardized lexicon to transition environmental metrics from abstract environmental, social, and governance (ESG) reporting into active runtime engineering variables. The following table establishes the operational definitions utilized throughout this architectural doctrine.

| Term | Technical Definition | Architectural Application |
| :---- | :---- | :---- |
| **Sustainable AI Architecture** | The systems engineering discipline of designing, deploying, operating, and retiring artificial intelligence systems under explicit energy, carbon, water, and lifecycle hardware utilization constraints. | Treats resource depletion and environmental metrics as non-negotiable runtime boundaries alongside latency, quality, cost, and safety. |
| **Energy-Aware Routing** | The dynamic redirection of inference requests to specific model architectures, precision tiers, or geographic nodes based on real-time hardware efficiency, context length, and grid carbon intensity. | Implemented at the API gateway layer to prevent the unnecessary execution of over-parameterized models for low-complexity tasks. |
| **Carbon-Aware Scheduling** | The temporal or spatial shifting of non-urgent, computationally intensive workloads to align with periods of high renewable energy availability or lower ambient temperatures.1 | Applied to offline evaluations, pretraining, fine-tuning, embedding generation, and vector index rebuilds.1 |
| **Embodied Carbon** | The cumulative greenhouse gas emissions generated during the raw material extraction, semiconductor fabrication, component assembly, logistics, and end-of-life disposal of hardware.5 | Evaluated at procurement to calculate the amortized carbon tax of physical accelerator platforms over their operational lifespans.9 |
| **Operational Carbon** | The emissions generated directly by the electricity consumed during the active operation of computing infrastructure, including server execution and cooling overhead.5 | Managed dynamically via model optimization, workload placement, and localized green energy procurement.3 |
| **Marginal Carbon Intensity** | The grid-level carbon emissions generated per unit of electricity by the marginal power plant online to support a sudden increment in electrical demand. | Used as the target signal for carbon-aware scheduling rather than average grid intensity, reflecting the true environmental impact of new workloads.1 |
| **Useful Utilization** | The ratio of floating-point operations (FLOPs) executed for successful, policy-compliant user tasks relative to the total idle and active power drawn by the hardware.10 | Prevents the optimization of hardware execution rates for redundant, failing, or behaviorally non-compliant model executions.10 |
| **Energy per Successful Task** | The total electrical energy in joules consumed across the entire execution path (retrieval, prefill, decoding, verification, and retry loops) to deliver a validated user outcome. | Replaces "joules per token" as the core efficiency metric, penalizing architectures prone to repetitive failures or excessive verbosity.13 |
| **Lifecycle Impact** | The cradle-to-grave resource footprint of an AI system, encompassing physical server manufacturing, data center construction, water evaporation, and e-waste.5 | Forces the amortization of both embodied and operational emissions over the functional retirement window of the model and its host hardware.7 |
| **Local Inference** | Model execution performed entirely on client-side, on-device accelerators (such as neural processing units) without relying on centralized cloud infrastructure.18 | Eliminates network transmission overhead and utilizes localized, thermal-constrained device budgets, though at the cost of execution efficiency.18 |
| **Cloud Inference** | Model execution hosted on centralized, highly virtualized hyperscale data centers optimized for maximum hardware occupancy and resource sharing.7 | Leverages economies of scale and professional cooling design, but concentrates grid demand and water consumption in localized utility basins.17 |
| **Greenwashing** | The practice of marketing AI systems as environmentally sustainable or "green" based on unproven, unscientific, or highly selective metrics while hiding broader infrastructure impacts.22 | Typically characterized by reporting operational offsets while ignoring embodied carbon, water stress, or underutilized accelerator fleets.17 |
| **Sustainability Telemetry Plane** | The unified monitoring layer that captures, normalizes, and correlates hardware power consumption, water evaporation rates, grid intensity, and token flow. | Feeds the active gateway to dynamically adjust routing tables, scheduling queues, and auto-scaling group limits. |

## **Sustainable AI Architecture Doctrine**

The rapid expansion of generative AI workloads has introduced a fundamental systemic challenge: the rate of digital infrastructure expansion far outpaces the physical capacity of global energy and utility grids.27 Traditional application performance monitoring focuses on a binary status model, assuming that any transaction returning an HTTP 200 payload represents a healthy system.29 In high-dimensional, non-deterministic AI systems, this perspective is an architectural failure.29 A system can report a green operational dashboard while simultaneously executing multi-megawatt training runs that yield zero downstream business value, or executing highly verbose, repetitive reasoning steps that deplete limited water reserves and overload regional electrical substations.17  
The governing doctrine of sustainable AI architecture asserts that sustainability is a first-class engineering constraint, co-equal with latency, quality, cost, security, and governance.29 A system that achieves optimal task accuracy at the cost of unconstrained compute consumption, excessive context bloat, and unchecked hardware degradation is a poorly designed architecture. Historically, model selection has been treated as a prestige marker, where teams deploy the largest available frontier models to solve basic classification, data transformation, or informational lookup tasks.32 Sustainable architecture rejects this paradigm. The correct architecture is the smallest, most efficient, sufficiently capable model and routing topology that satisfies the validated criteria of the task.  
Maximizing efficiency requires analyzing the entire physical chain of computation. Every prompt executed on a client device initiates a physical cascade: a local processor packages the request; network switches route the packets across regional grids; a data center substation steps down high-voltage power to run specialized silicon; cooling systems evaporate water or run high-power compressors to extract heat; and global supply chains fabricate the accelerators that will eventually decay into e-waste.7 To optimize this system, architects must treat energy, carbon, and hardware degradation as hard constraints within which all software, routing, and quantization decisions must be negotiated. The greenest token is the one the system did not need to generate.

## **The Macroeconomic Resource Challenge**

The expansion of generative AI workloads has transformed data centers from passive, predictable consumers of commodity electricity into volatile, resource-dense industrial nodes. To contextually frame this shift, the physical parameters of data center energy demand, grid integration, water consumption, and thermal dissipation must be quantified.

### **Global and Regional Grid Projections**

Global data-center electricity demand is rising quickly, but projections vary by scope, methodology, hardware assumptions, utilization assumptions, and whether cryptocurrency or broader digital infrastructure is included. Sustainable AI architecture should therefore avoid treating a single forecast as destiny. The correct planning posture is to maintain a baseline, a near-term estimate, a 2030 base case, and high/sensitivity cases.

| Planning View | Estimate / Range | Interpretation | Architectural Implication |
| :---- | :---- | :---- | :---- |
| **Recent global baseline** | Approximately 415 TWh in 2024, around 1.5% of global electricity consumption. | Current data-center demand is already a large industrial load, before full AI buildout is realized. | AI systems should track electricity use as a first-class operational metric, not as annual ESG afterthought. |
| **2030 IEA base case** | Approximately 945 TWh by 2030, just under 3% of global electricity consumption. | Data-center electricity demand roughly doubles by 2030 under base-case assumptions. | Routing, scheduling, model sizing, and utilization controls become infrastructure capacity controls. |
| **2030 sensitivity / high-growth cases** | Higher estimates exceed the base case depending on AI demand, accelerator deployment, grid constraints, and regional buildout. | Forecast uncertainty is high because demand depends on adoption, regulation, chip supply, efficiency gains, and grid availability. | Architectures should support budget caps, carbon-aware scheduling, workload deferral, and graceful degradation. |
| **AI share of data-center load** | Increasing rapidly, but estimates vary by definition and region. | AI-optimized servers are shifting data centers from relatively predictable enterprise loads toward dense, accelerator-heavy industrial loads. | AI traffic should be separated from conventional IT workloads in telemetry, procurement, and sustainability reporting. |
| **Regional concentration** | Demand is concentrated in specific grid regions and data-center hubs. | Local grid stress can be severe even when global percentages look modest. | Regional placement must consider grid congestion, water stress, interconnection queues, and local utility capacity. |

In the United States, Europe, China, Ireland, Singapore, Northern Virginia, Frankfurt, Dublin, and other dense data-center markets, the operational issue is not merely aggregate global electricity share. It is localized concentration. A workload that looks small in global percentage terms can still create major interconnection delays, utility-rate pressure, water-basin stress, or local reliability risk when clustered in constrained regions.

The architectural conclusion is straightforward: sustainable AI cannot depend on vague global averages. It must make deployment decisions using region-specific electricity, water, cooling, carbon, and capacity constraints.

### **Power Density and Dynamic Grid Interconnection**

The physical characteristics of AI compute clusters impose severe stresses on grid infrastructure. Traditional server racks operate at predictable power densities of 5 kW to 15 kW. By contrast, advanced AI server racks packed with modern accelerator blocks feature power densities that increased 11-fold between 2020 and 2025, with an additional fourfold increase projected by 2027.30 A single advanced server rack can have a peak power demand equivalent to that of 65 average households.30  
Furthermore, AI workloads exhibit extreme load variability. While traditional enterprise workloads present smooth, diurnal load profiles, AI data centers experience repeated swings of server load exceeding 50% of rated capacity within a single second.30 These rapid step-changes inject significant frequency instability into the local transmission grid. To mitigate the risk of voltage sag or grid collapse, operators are forced to either overbuild dedicated onsite gas-fired backup generation by 30% to 70% relative to average demand, or contract with private producers to install multiple, highly inefficient natural gas reciprocating engines directly at the data center boundary.28

### **Hydrological and Thermal Footprints**

AI infrastructure has a water and thermal footprint as well as an electricity footprint. Water use varies sharply by region, cooling design, facility efficiency, weather, time of day, and electricity-generation mix. A single universal “water per prompt” number is therefore misleading. Published GPT-3-style estimates suggest roughly one 500 ml bottle of water for about 10 to 50 medium-length responses, depending on when and where inference is served. That figure should be treated as an order-of-magnitude estimate, not a universal constant.

Water consumption has three main channels:

| Water Channel | Description | Architectural Control |
| :---- | :---- | :---- |
| **Direct facility water** | Water consumed by data-center cooling systems, especially evaporative cooling. | Region selection, cooling design, WUE targets, workload shifting away from water-stressed basins. |
| **Indirect electricity water** | Water consumed by the electricity-generation mix powering the workload, including thermal generation cooling and hydroelectric evaporation. | Carbon-aware and water-aware scheduling, grid-region selection, clean-energy procurement. |
| **Supply-chain water** | Water consumed during semiconductor manufacturing, facility construction, and hardware lifecycle processes. | Procurement requirements, hardware lifetime extension, utilization improvement, refurbishment and recycling. |

Thermal stress is similarly local. Dense AI clusters convert nearly all consumed electrical energy into heat that must be removed from racks, rooms, and facilities. High-density accelerator racks may require liquid cooling, rear-door heat exchangers, or facility-level cooling redesign. In hot or water-stressed regions, the environmental tradeoff can become acute: reducing electricity use, reducing water use, and maintaining hardware reliability may require different operating points.

The practical engineering rule is that water and thermal metrics must be attached to deployment decisions:

| Decision | Sustainability Question |
| :---- | :---- |
| **Region selection** | Is the target grid or basin already constrained by water stress, heat, or interconnection delay? |
| **Cooling design** | Does the facility trade lower electricity use for high evaporative water loss? |
| **Workload scheduling** | Can flexible jobs run during cooler periods or cleaner grid windows? |
| **Model sizing** | Can the task be handled by a smaller or more efficient route without raising failure/retry rates? |
| **Procurement** | Does the vendor disclose WUE, cooling method, and water-risk exposure at useful granularity? |

Sustainable AI architecture should therefore report water estimates with scope, region, method, and confidence level. “Water per response” is useful only when it is tied to a specific model class, facility type, electricity mix, cooling design, request size, and measurement method.

## **Energy, Carbon, and Water Cost Model**

Sustainable AI requires unit-explicit accounting. Energy, carbon, water, and embodied hardware impact should be modeled as related but distinct quantities. A useful model must separate measured energy from allocated estimates, operational emissions from embodied emissions, and successful work from failed or reworked computation.

### **Unit-Explicit Operational Energy**

For a workload executed over time window `T`, the system energy is:

```text
E_system_kWh =
  sum over t in T [
    (P_compute_kW(t)
   + P_memory_kW(t)
   + P_storage_kW(t)
   + P_network_kW(t)
   + P_cooling_allocated_kW(t)) * delta_t_hours
  ]
```

Where:

| Term | Meaning |
| :---- | :---- |
| `P_compute_kW(t)` | Active accelerator, CPU, and model-serving power. |
| `P_memory_kW(t)` | Memory and KV-cache-related power where separately measurable or allocated. |
| `P_storage_kW(t)` | Storage power allocated to retrieval, logs, vector indexes, checkpoints, or traces. |
| `P_network_kW(t)` | Network power allocated to request routing, retrieval, replication, or data transfer. |
| `P_cooling_allocated_kW(t)` | Facility cooling energy allocated to the workload through PUE or direct metering. |
| `delta_t_hours` | Measurement interval in hours. |

For most production systems, not every component is directly measured per request. The architecture should label each value as measured, estimated, allocated, or vendor-reported.

### **Operational Carbon**

Operational carbon is calculated by multiplying electricity consumption by grid carbon intensity:

```text
CO2e_operational_kg =
  sum over t in T [
    E_system_kWh(t) * CI_grid_kgCO2e_per_kWh(g, t)
  ]
```

For scheduling and workload-shifting decisions, marginal carbon intensity is usually the more appropriate control signal when available. Average grid intensity may be useful for reporting but can understate or misrepresent the impact of new incremental load.

### **Embodied Carbon Allocation**

Embodied carbon should not be added wholesale to every request. It must be allocated across the hardware’s useful life according to a declared allocation method:

```text
CO2e_embodied_allocated_kg =
  CO2e_hardware_lifecycle_kg
  * workload_allocation_share
  * accounting_window_share
```

Possible allocation bases include:

| Allocation Basis | Best Use |
| :---- | :---- |
| **Time share** | Dedicated hardware or reserved clusters. |
| **FLOP share** | Mixed workloads with reliable compute accounting. |
| **Accelerator-hour share** | Shared GPU/TPU fleets with scheduler telemetry. |
| **Revenue/task share** | Business-level reporting where physical allocation is unavailable. |
| **Hybrid allocation** | Large organizations with multiple workload classes and partial telemetry. |

Allocation method must be documented. Otherwise embodied-carbon accounting becomes spreadsheet alchemy with a leaf icon.

### **Verified-Task Efficiency**

A successful AI task is not merely a completed model call. It is an accepted, policy-compliant, verified outcome. Instead of dividing by a binary success variable and creating infinite dashboard values, the system tracks verified work and waste separately.

```text
E_per_verified_task_kWh =
  E_total_accepted_tasks_kWh / count(verified_successful_tasks)
```

```text
CO2e_per_verified_task_kg =
  (CO2e_operational_accepted_kg + CO2e_embodied_allocated_kg)
  / count(verified_successful_tasks)
```

Failed or rejected work is tracked as waste:

```text
E_waste_kWh =
  E_failed_generations
+ E_retries
+ E_policy_rejections
+ E_unverified_tool_attempts
+ E_rework
```

```text
Waste_ratio =
  E_waste_kWh / E_total_kWh
```

This makes brittle systems visible. A tiny model that fails repeatedly, triggers retries, or requires human cleanup may be less sustainable than a larger model that solves the task correctly once.

### **Water Accounting**

Water use should be modeled as direct facility water plus indirect electricity water plus allocated supply-chain water where available:

```text
Water_direct_liters =
  E_IT_kWh * WUE_facility_liters_per_kWh
```

```text
Water_indirect_liters =
  E_grid_kWh * WI_grid_liters_per_kWh(g, t)
```

```text
Water_task_liters =
  Water_direct_allocated
+ Water_indirect_allocated
+ Water_supply_chain_allocated
```

Where:

| Term | Meaning |
| :---- | :---- |
| `WUE_facility_liters_per_kWh` | Facility water usage effectiveness or equivalent direct water intensity. |
| `WI_grid_liters_per_kWh` | Water intensity of the regional electricity mix. |
| `Water_supply_chain_allocated` | Allocated water footprint from hardware manufacturing and facility lifecycle where known. |

Because water estimates vary dramatically by region and cooling design, water metrics must include location, measurement method, and confidence level.

### **Metric Families**

| Metric | Formula / Method | Scope | Optimization Target |
| :---- | :---- | :---- | :---- |
| **Joules per Token** | Energy during generation interval divided by generated tokens. | Decoding efficiency benchmark. | Kernel/backend comparison, model-serving optimization. |
| **Joules per Request** | Total request-path energy across prefill, decoding, retrieval, safety, and routing. | Request-level efficiency. | Prompt design, routing, cache strategy. |
| **Energy per Verified Task** | Energy for accepted successful work divided by verified task count. | Outcome-level efficiency. | Correct model sizing and routing. |
| **Waste Energy Ratio** | Failed/retried/rejected/reworked energy divided by total energy. | Failure-driven inefficiency. | Reduce brittle chains, retry loops, and bad routing. |
| **Carbon per Request** | Request energy multiplied by grid carbon intensity. | Operational carbon estimate. | Carbon-aware routing and scheduling. |
| **Carbon per Verified Task** | Accepted-task operational + allocated embodied carbon divided by verified task count. | Sustainability KPI. | Lifecycle-aware architecture comparison. |
| **Water per Request** | Direct + indirect water allocated to the request. | Regional water estimate. | Water-aware placement and scheduling. |
| **Rework-Adjusted Energy** | Compute energy plus estimated human remediation and rerun energy. | Full failure cost. | Avoid false economy from underpowered models. |
| **Embodied Carbon per Accelerator-Hour** | Lifecycle hardware footprint allocated across useful operating hours. | Hardware lifecycle accounting. | Extend useful life and maximize useful utilization. |


## **Model Sizing Matrix**

Model size is an architectural decision, not a prestige marker. The correct route is the smallest sufficiently capable execution path that satisfies quality, safety, latency, privacy, and governance constraints. The following matrix is indicative rather than universal: energy use varies by hardware, batching, context length, output length, quantization, cache state, compiler backend, and utilization.

| Task Class | Ambiguity / Risk | Context Profile | Candidate Architecture | Sizing Guidance | Verification Gate | Escalation Trigger |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Deterministic Transform** | None to very low. | Small, structured input. | Static code, parser, schema validator, regex, AST transform, database query. | No generative model unless natural-language ambiguity is present. | Unit tests, schema validation, deterministic output comparison. | Input does not match known transform signature. |
| **Structured Lookup** | Low. | Small to moderate. | Search API, SQL template, rules engine, typed retrieval. | Prefer deterministic or tool-backed route over LLM generation. | Source-of-record check, query validation, permission filter. | User asks ambiguous or synthesized question beyond lookup scope. |
| **Simple Extraction / Classification** | Low to medium. | Short documents or bounded fields. | Embeddings, classifier, small model, constrained decoder. | Smallest model that meets extraction accuracy and schema fidelity. | Entity-level precision/recall, schema validity, calibration. | Low confidence, out-of-domain input, regulated field. |
| **Constrained RAG Answering** | Medium. | Trusted corpus, moderate context. | Retrieval + reranker + small/specialist model. | Size model to answer from evidence, not from parametric memory. | Groundedness, context relevance, citation verification. | Low evidence support, conflicting sources, sensitive domain. |
| **Domain Specialist Workflow** | Medium to high. | Specialized vocabulary or workflow rules. | Fine-tuned specialist, domain model, tool-assisted route. | Specialist model may beat larger generalist when domain is narrow and evals are strong. | Domain benchmark, SME review, structured output checks. | Novel domain edge case, low confidence, high-impact result. |
| **Routine Document Processing** | Medium. | Large PDFs, forms, reports, OCR, tables. | Parser + retrieval + specialist model + validation. | Spend compute on parsing and evidence quality before escalating model size. | Field-level extraction accuracy, citation/page coordinate verification. | Parser uncertainty, missing fields, layout ambiguity. |
| **Complex Multi-Step Reasoning** | High. | Multi-document, tool-using, conflicting or long context. | Cascaded route: smaller planner/evaluator, larger model when justified. | Use large models only when lower-cost routes fail confidence, reasoning, or verification gates. | Task tests, tool verification, consistency checks, human review where needed. | Failed assertions, unresolved conflict, unsafe action, high rework risk. |
| **High-Impact Strategic Synthesis** | Very high. | Long context, high consequence, regulated or irreversible decisions. | Governed cascade with frontier model, review gates, and evidence package. | Higher compute may be sustainable if it prevents harm, rework, legal exposure, or repeated failures. | Dual evaluation, human oversight, audit trail, policy compliance. | Low confidence, missing evidence, unsupported claim, action side effect. |

The matrix should be implemented as a routing policy, not a static spreadsheet shrine. Each route must be validated on production-like traffic and periodically reviewed as models, hardware, costs, and environmental conditions change.


## **Energy-Aware Routing Policy**

A static, single-model API gateway is an architectural anti-pattern for sustainable AI. The routing layer should match each request to the least resource-intensive path that still satisfies quality, safety, privacy, latency, and governance requirements. Sustainability never overrides hard safety or access-control boundaries; it chooses among safe and valid execution paths.

```text
ENERGY-AWARE ROUTING FLOW

[ Incoming Request ]
        |
        v
[ Classify Task ]
  deterministic | lookup | extraction | RAG | reasoning | high-impact
        |
        v
[ Check Hard Floors ]
  safety | privacy | tenant scope | policy | data residency | tool authority
        |
        v
[ Estimate Route Cost ]
  energy | carbon | water | latency | money | rework risk
        |
        v
[ Select Smallest Sufficient Route ]
        |
        +--> deterministic route
        +--> small-model route
        +--> specialist route
        +--> cascade route
        +--> large-model route
        +--> local route
        +--> cloud route
        +--> batch/offline route
        +--> fail-closed/degraded route
        |
        v
[ Verify Outcome ]
        |
        v
[ Log Routing Decision According to Retention Policy ]
```

### **Route Modes**

| Route Mode | Use When | Sustainability Benefit | Guardrail |
| :---- | :---- | :---- | :---- |
| **Deterministic Route** | Task is a known transform, calculation, validation, lookup, or formatting operation. | Avoids model inference entirely. | Must reject ambiguous inputs rather than pretending deterministic code understood them. |
| **Small-Model Route** | Task is bounded, low-risk, and covered by evals. | Reduces memory bandwidth, power draw, and serving cost. | Must escalate when confidence, schema, or evidence checks fail. |
| **Specialist Route** | Narrow domain has strong specialist model or tool route. | Avoids large generalist model for domain-bounded tasks. | Must be evaluated against domain edge cases and drift. |
| **Cascade Route** | Complexity is uncertain or variable. | Pays large-model cost only when smaller route fails. | Verifier must be independent enough to detect failure, not rubber-stamp it. |
| **Large-Model Route** | High ambiguity, high rework risk, or high consequence justifies compute. | Avoids waste from repeated smaller-model failure. | Requires stronger evidence, policy, and success verification. |
| **Local Route** | Privacy, latency, offline operation, or edge constraints dominate. | Reduces network dependency and can preserve sensitive data locally. | Must account for device energy, thermal limits, auditability, and update lifecycle. |
| **Cloud Route** | Workload needs high throughput, large context, specialized accelerators, or centralized governance. | Can improve utilization and observability. | Must account for regional grid, water stress, provider telemetry, and tenant isolation. |
| **Batch / Offline Route** | Workload is flexible in time or location. | Enables carbon-aware and water-aware scheduling. | Must respect deadlines, data residency, fairness, and capacity constraints. |
| **Fail-Closed / Degraded Route** | No safe route satisfies environmental, safety, privacy, or governance floors. | Prevents unsustainable or unsafe execution. | Must preserve state and communicate limitation honestly. |

Routing decisions should record route ID, model/profile, task class, policy version, estimated energy/carbon/water method, and verification outcome according to retention and audit policy. Do not retain sustainability telemetry forever by default; eternal logs become their own zombie workload.

## **Efficient Inference Playbook**

Efficient inference is achieved by combining architecture, compiler, memory, routing, cache, context, and output controls. No optimization is universally safe. Each must be evaluated against task quality, policy compliance, data isolation, and downstream failure rate.

| Technique | Optimization Target | Expected Benefit | Quality / Governance Risk | Required Evaluation |
| :---- | :---- | :---- | :---- | :---- |
| **Quantization** | Reduce weight and activation precision to lower memory bandwidth and improve throughput. | Lower latency, higher batch occupancy, reduced energy per token. | Can degrade reasoning, calibration, multilingual behavior, tool accuracy, or rare-domain performance. | Task-level evals, schema pass rate, safety checks, calibration, regression by domain. |
| **Distillation** | Transfer behavior from a larger model to a smaller model. | Lower serving cost and energy for repeated task classes. | Student model may lose edge-case generalization or hidden capabilities needed for rare inputs. | Teacher/student comparison, out-of-distribution tests, human review for high-impact classes. |
| **Pruning / Sparsity** | Remove or skip weights, layers, heads, or computation paths. | Reduced compute and memory pressure. | May create uneven degradation by task type. | Per-task regression, latency/energy benchmark, failure clustering analysis. |
| **Speculative Decoding** | Draft tokens with a smaller model and verify with a target model. | Lower latency and improved throughput when acceptance rate is high. | Exactness depends on algorithm, sampling configuration, and implementation. | Acceptance rate, task quality, output distribution checks, safety and schema regression. |
| **Prefix Caching** | Reuse KV cache for stable prompt prefixes such as system prompts or schemas. | Reduces prefill cost for repeated prefixes. | Unsafe if prefix identity, policy version, tenant scope, or tool schema changes. | Cache-key validation, tenant isolation, policy-version invalidation, prefill latency measurement. |
| **Semantic Caching** | Reuse prior answers for semantically similar requests. | Avoids repeated inference for common low-risk queries. | Can return stale, unauthorized, or subtly mismatched answers. | Freshness checks, permission scope, similarity threshold tuning, answer-support validation. |
| **Continuous Batching** | Batch active decoding steps across requests. | Improves accelerator occupancy and reduces idle draw. | Can increase tail latency or complicate priority handling. | Throughput, p95/p99 latency, fairness, cancellation behavior. |
| **KV Cache Management** | Allocate, evict, page, or compress KV cache efficiently. | Increases concurrent serving capacity and reduces memory waste. | Incorrect reuse or eviction can corrupt sessions or degrade long-context behavior. | Session isolation tests, long-context regression, memory pressure tests. |
| **Prompt / Context Compression** | Reduce prompt tokens before prefill. | Lowers prefill energy and cost. | Can remove critical evidence, citations, instructions, or nuance. | Claim support, retrieval recall, answer accuracy, citation fidelity, schema fidelity. |
| **Early Exit / Adaptive Compute** | Stop computation early for easy inputs. | Reduces average compute per request. | Misclassifies hard cases as easy and returns low-quality outputs. | Confidence calibration, hard-case regression, escalation checks. |
| **Output Length Control** | Constrain verbosity and stop unnecessary generation. | Reduces decode energy and user review burden. | Over-compression may omit necessary caveats or evidence. | Answer completeness, policy disclosure, user task success. |
| **Tool-First Execution** | Use deterministic tools before model synthesis. | Avoids unnecessary generation for calculable or database-backed tasks. | Tool errors can be hidden if model summarizes without verification. | Source-of-record checks, tool-result grounding, false-success tests. |

The efficiency objective is not maximum tokens per second. It is minimum resource use per verified, policy-compliant task.

## **Context and Retrieval Efficiency Model**

Retrieval-Augmented Generation (RAG) is a highly effective pattern for grounding model responses, but naive implementations can lead to significant resource waste. Sending large, unranked document chunks into a prompt to answer a question that requires only a single sentence is a primary source of context inflation and energy waste.42

### **The Context Bloat Cascade**

When a system retrieves ten documents of 400 tokens each to answer a query, it forces the model to process 4,000 context tokens.42 If the actual answer is buried in a single sentence in the sixth document, several systemic failures occur:

* **Prefill Energy Penalty:** The model pays a major computational tax to execute self-attention over the entire 4,000-token input.41  
* **Decoding Memory Pressure:** The size of the KV cache increases linearly with context length, restricting batch size on the accelerator and driving down hardware efficiency.13  
* **Attention Degradation:** Language models demonstrate "lost in the middle" phenomena, where they reliably process information at the absolute beginning and end of the prompt but fail to track facts buried in the center of a long context window.42  
* **Financial Waste:** The enterprise pays per input token, meaning that over-retrieval results in direct financial inflation on every single execution.42

### **Retrieval and Context Metrics**

To enforce efficiency across retrieval architectures, teams must integrate five core metrics into their observability pipelines:

#### **Retrieved-Token Usefulness Ratio (R_useful)**

Measures the fraction of retrieved and prompt-inserted tokens that actually contribute to the validated output response, defined as:

R_useful = Tokens in Context Containing Gold Facts / Total Retrieved Tokens in Prompt

Low usefulness ratios (less than 0.1) indicate poorly sized document chunks or the absence of semantic rerankers.42

#### **Duplicate Context Ratio (R_dup)**

Quantifies the volume of redundant semantic information present within the retrieved context set:

R_dup = Redundant Sentences or Duplicate Semantic Chunks / Total Sentences in Context Set

High redundancy indicates a lack of deduplication filters or clustering steps in the retrieval pipeline.38

#### **Stale Context Ratio (R_stale)**

Tracks the portion of the retrieved document set that has been updated or deprecated in the source system of record but remains active in the vector database index:

R_stale = Stale or Deprecated Chunks Retrieved / Total Retrieved Chunks

This metric measures index maintenance efficiency, identifying systemic energy waste in continuous indexing pipelines.26

#### **Grounding Gain per Token (G_token)**

Calculates the marginal improvement in output correctness (measured by automated factual consistency scores) per additional token of context inserted into the prompt:

G_token = (Accuracy_grounded - Accuracy_zero_shot) / Context Tokens

This curve helps identify the point of diminishing returns, where adding more context documents fails to improve output quality while continuing to inflate energy consumption.

#### **Context Tokens per Successful Answer (C_task_tokens)**

The absolute number of prompt context tokens processed to deliver a verified, policy-compliant user response:

C_task_tokens = Total Context Tokens processed in Run / S_task

## **Hardware Utilization Model**

Maximizing the energy efficiency of an AI deployment requires a deep understanding of accelerator physics. The primary source of resource waste in modern enterprise AI is not the power consumed by active computation, but the static energy wasted by underutilized, idle accelerators.10

### **The Accelerator Power Curve**

Modern AI accelerators exhibit highly non-linear power curves. An advanced GPU or TPU baseboard draws a substantial amount of power when idling—frequently 30% to 50% of its maximum thermal design power (TDP)—even when executing zero floating-point operations.10 For example, a single high-performance NVIDIA HGX H100 GPU baseboard features a typical power consumption of 5,600 W.8 While a single H100 GPU has a TDP of approximately 700 W (compared to 400 W for the previous-generation A100), it continues to draw nearly 300 W of static power while idling.9  
Consequently, a cluster of accelerators running at 15% average utilization (which represents the baseline average for most enterprise deployments 32) consumes nearly the same baseline operational energy as a cluster running at 70% utilization, while delivering only a fraction of the useful output.10 This represents an immense waste of both operational energy and amortized embodied carbon.

```
Instantaneous Power (Watts)  
  ▲  
  │                       ┌─────────────────────────  ◄── Maximum TDP (700 Watts)   
  │                     ┌─┘  
  │                   ┌─┘  
  │                 ┌─┘  
  │               ┌─┘  
  │             ┌─┘  
  │  ───────────┘  ◄──────────────────────────────────  Idle Draw (300 Watts)   
  │  │  
  └──┴──────────┬───────────────────────────┬────────►  
     0%        15% (Typical Enterprise)    70%+ (Target)  
                Accelerator Utilization (%) 
```

To eliminate this waste, platform teams must monitor and optimize several hardware-level telemetry points:

* **Accelerator Memory and Bandwidth Utilization:** Captures the fraction of high-bandwidth memory (HBM) capacity occupied by active model weights and KV cache, and tracks the active memory bus traffic.13  
* **Active Queue Depth and Batch Occupancy:** Monitors the number of queued execution requests awaiting processing. If queue depth is consistently zero while accelerators are kept online, the cluster is over-provisioned.32  
* **Cold Replica Fragmentation:** The presence of warm, idle model replicas running across multiple isolated server nodes, each drawing static idle power rather than consolidating traffic onto a single node.  
* **Accelerator Power Caps and Thermal Throttling:** Restricts the maximum allowable power draw of individual GPUs (e.g., capping an H100 at 550 W instead of 700 W). This technique achieves a substantial reduction in energy-per-token with minimal impact on latency.  
* **Cooling Infrastructure Overhead (PUE/WUE):** The localized data center power and water consumed to dissipate heat generated by the active accelerators.15 Less-efficient enterprise data centers feature cooling overheads exceeding 30% of total energy draw, compared to less than 7% in highly optimized hyperscale facilities.34

## **Energy-Aware Deployment Profiles**

To operationalize sustainable architecture across diverse enterprise application scenarios, engineers must implement structured deployment profiles. These profiles define the exact configuration knobs, target architectures, and optimization strategies appropriate for specific operational tolerances.

| Profile Name | Target Latency / SLO | Primary Optimization Objective | Quantization Tiers | Speculative Decoding Strategy | Scaling / Topology Rules | Approved Workload Classes |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Interactive Low-Latency** | TTFT < 100ms; Total Latency < 1s | Minimize user-perceived latency and time-to-first-token. | FP8 or INT8 with active KV Cache compression.37 | Multi-candidate draft-target speculative decoding using specialized feature heads (e.g., EAGLE).44 | Keep warm replicas with aggressive scale-up triggers; deploy to edge or local nodes if network latency exceeds 200ms.19 | Live customer support chat, real-time code-autocomplete widgets, conversational interfaces. |
| **Batch / Offline High-Throughput** | Latency-insensitive; queue delays up to 24 hours | Maximize throughput per watt; minimize total carbon footprint.1 | Highly compressed FP4 or INT4 quantization. | Disabled or simplified draft model to save memory footprint. | Consolidate traffic into massive, high-occupancy batches; execute scale-to-zero when queues are fully depleted. | Batch document classification, offline evaluation runs, vector index regeneration, text corpus embedding. |
| **High-Impact Governed** | Moderate Latency (1s–5s) | Prevent downstream semantic, policy, and safety failures; avoid manual human rework.37 | High-precision FP16 or FP8 to preserve reasoning capabilities. | Strict target verification; speculative decoding allowed only when draft-model alignment is verified.44 | Redundant dual-gate evaluation; warm, isolated compute pools to ensure deterministic execution. | Legal contract analysis, medical chart review, automated compliance reporting, high-stakes financial transactions. |
| **Edge / Local Constraint** | Highly variable; sub-50ms local processing | Minimize network dependency, protect data privacy, operate within strict thermal budgets.18 | Aggressive INT4, 3-bit, or binary quantization to fit local device memory. | Local on-device draft head or disabled.18 | Scale-to-zero immediately; execute on localized NPU/GPU threads.18 | Offline mobile translation apps, localized privacy-critical data entry, smart-device voice interfaces.18 |
| **Hybrid Dynamically Routed** | Adaptive (100ms–5s) | Balance latency, cost, and carbon footprints dynamically.2 | Variable; switches dynamically between uncompressed and quantized models. | Adaptive speculative decoding; changes candidate length based on live queue depth. | Dynamic autoscaling across local, private cloud, and public cloud endpoints.18 | Enterprise workflow platforms, variable-demand web applications, multi-tenant SaaS tools. |
| **Degraded Survival Mode** | Best-effort latency | Maintain minimal viable functionality under extreme capacity or budget sags. | Minimum viable 2-bit or 3-bit quantization; drop non-essential features (e.g., fallback to text-only from multimodal). | Disabled to conserve memory and minimize power spikes. | Hard limits on concurrent requests; drop queue depth limiters; enforce aggressive rate limits. | Emergency response platforms, offline recovery terminals, cost-capped utility sags. |

## **Carbon-Aware Scheduling Model**

Carbon-aware scheduling shifts flexible AI workloads to cleaner, cooler, cheaper, or less constrained execution windows. It is most useful for non-urgent jobs such as offline evaluations, batch embeddings, vector index rebuilds, document parsing, pretraining, fine-tuning, and synthetic data generation.

Live user interactions, security response, critical transaction routing, and legally time-bound workflows usually cannot be delayed for environmental optimization. Sustainability is a scheduling objective only inside the envelope permitted by safety, policy, deadline, and user commitments.

### **Scheduling Candidates and Exclusions**

| Workload Type | Scheduling Treatment |
| :---- | :---- |
| **Embedding batch generation** | Strong candidate for carbon-aware and water-aware scheduling. |
| **Vector index rebuilds** | Candidate when freshness SLO permits delay. |
| **Offline eval sweeps** | Candidate unless blocking emergency release validation. |
| **Historical OCR / parsing** | Candidate when not tied to active user workflow. |
| **Fine-tuning / training** | Candidate when data residency, deadline, and accelerator availability permit. |
| **Live chat / realtime voice** | Usually excluded. |
| **Security incident response** | Excluded unless delay is explicitly safe. |
| **High-impact transaction flow** | Excluded unless workflow is already queued for human review or batch execution. |

### **Scheduling Decision Flow**

```text
CARBON-AWARE SCHEDULING FLOW

[ Workload Submitted ]
        |
        v
[ Classify Flexibility ]
  immediate | deadline-bound | flexible | opportunistic
        |
        v
[ Check Hard Constraints ]
  data residency
  privacy / tenant scope
  legal deadline
  freshness SLO
  security urgency
  capacity reservation
        |
        v
[ Estimate Execution Windows ]
  grid carbon intensity
  marginal carbon signal
  regional water stress
  ambient temperature / cooling load
  electricity price
  queue backlog
  accelerator availability
        |
        v
[ Optimize ]
  minimize carbon + water + cost
  subject to deadline + policy + quality + fairness
        |
        +--> execute now
        +--> queue until cleaner window
        +--> shift to approved region
        +--> split job across windows
        +--> reject or escalate if constraints conflict
```

### **Scheduling Constraints**

| Constraint | Why It Matters |
| :---- | :---- |
| **Deadline / Freshness SLO** | Prevents stale indexes, delayed reports, or missed release gates. |
| **Data Residency** | Some data cannot move to cleaner regions. |
| **Privacy / Tenant Isolation** | Scheduling cannot weaken access boundaries. |
| **Regional Water Stress** | Lowest-carbon region may not be lowest-water-impact region. |
| **Network and Egress Cost** | Moving data can create cost, latency, and carbon overhead. |
| **Queue Fairness** | Low-priority jobs should not starve indefinitely waiting for perfect carbon windows. |
| **Accelerator Availability** | Cleaner region must have compatible hardware and capacity. |
| **Reliability Risk** | Workloads should not shift into unstable regions or overloaded queues. |

### **Scheduler Outputs**

| Output | Meaning |
| :---- | :---- |
| **Execute now** | Current window is acceptable or workload is urgent. |
| **Delay** | Workload waits for a cleaner, cooler, cheaper, or less constrained window. |
| **Shift region** | Workload moves only to an approved region satisfying residency and privacy constraints. |
| **Split execution** | Large job is partitioned across approved windows. |
| **Escalate** | Constraints conflict or deadline cannot be met within environmental budget. |

Carbon-aware scheduling should be evaluated by measured or estimated avoided emissions, missed-deadline rate, queue fairness, cost impact, and user/business outcome quality.

## **Local vs Cloud Decision Matrix**

The local-versus-cloud decision is not a simple sustainability contest. Local execution can reduce network dependency and preserve privacy, but may run on inefficient, underutilized, poorly measured hardware. Cloud execution can improve utilization and observability, but may concentrate electricity, water, heat, and grid stress in constrained regions. Sustainable architecture requires a full lifecycle comparison.

| Dimension | Local / Edge Pattern | Cloud / Data-Center Pattern | Decision Rule |
| :---- | :---- | :---- | :---- |
| **Privacy and Data Control** | Sensitive data can remain on device or on-prem. | Data may traverse provider infrastructure and subprocessors. | Prefer local when privacy, offline operation, or data sovereignty dominates. |
| **Energy Efficiency** | Highly variable; consumer or office devices may be inefficient under sustained AI load. | Often more efficient per unit of compute when utilization and cooling are optimized. | Prefer the route with measured or credible estimated energy per verified task. |
| **Utilization** | Device accelerators may sit idle or be thermally constrained. | Shared clusters can achieve higher occupancy through batching and scheduling. | Prefer cloud for bursty or high-throughput work if provider efficiency is strong. |
| **Water Impact** | Usually avoids direct data-center cooling water, but still uses local/grid electricity. | May consume direct cooling water and indirect electricity water. | Prefer local or water-efficient regions when water stress is the binding constraint. |
| **Network Overhead** | Low when model, data, and retrieval are all local; higher if updates or retrieval sync continuously. | Higher data movement for prompts, files, embeddings, logs, and model outputs. | Include network and storage overhead in large-file or multimodal workflows. |
| **Embodied Carbon** | Distributed devices may have short replacement cycles and low utilization. | Servers can amortize embodied carbon across high utilization and managed lifecycle. | Prefer longer useful life and higher useful utilization, not merely “local” or “cloud.” |
| **Auditability** | Fragmented telemetry and inconsistent environments can reduce audit confidence. | Provider telemetry may be better, but only if exposed at useful granularity. | Prefer the route with sufficient evidence for the system’s risk tier. |
| **Latency and Resilience** | Excellent for offline or low-latency local tasks. | Strong for heavy workloads but dependent on network and provider availability. | Match route to latency, resilience, and fallback requirements. |
| **Governance Controls** | Harder to enforce centralized policy if devices are unmanaged. | Easier to enforce gateway policy, logging, and access control centrally. | High-impact workflows need enforceable governance regardless of placement. |

The sustainable choice is the route with the best verified outcome per lifecycle impact under the system’s privacy, safety, latency, governance, and regional constraints.

## **Sustainability Telemetry Plane**

Sustainability metrics must move from annual reports into operational telemetry, but they should be modeled correctly. W3C trace context should carry correlation identifiers, not every environmental measurement. Sustainability telemetry should join traces, span attributes, resource metrics, facility metrics, grid signals, and allocation models.

```text
SUSTAINABILITY TELEMETRY ARCHITECTURE

[ Request Trace ]
  trace_id | span_id | route_id | tenant/workload class
        |
        v
[ AI Span Attributes ]
  model route | prompt tokens | output tokens | cache hits
  batch size | tool calls | verifier outcome | task status
        |
        v
[ Hardware Metrics Stream ]
  accelerator power | utilization | memory | temperature
  queue depth | batch occupancy | replica count
        |
        v
[ Facility / Region Metrics ]
  PUE | WUE | cooling mode | region | grid carbon intensity
  water stress | electricity price | capacity state
        |
        v
[ Allocation Layer ]
  joins request traces to metric windows
  estimates energy, carbon, water, embodied allocation
        |
        v
[ Control Plane ]
  routing | scheduling | autoscaling | scale-to-zero | procurement review
```

### **Trace and Span Schema**

The request trace should record identifiers and AI execution metadata:

```json
{
  "trace_id": "ot-9821a-bf091",
  "span_id": "span-0918a",
  "ai.route_id": "cascade.route.triage",
  "ai.model_profile": "small-specialist-fp8",
  "ai.hardware_class": "accelerator.shared.h100",
  "ai.region": "us-east-1",
  "ai.prompt_tokens": 1042,
  "ai.completion_tokens": 412,
  "ai.cached_prefix_tokens": 512,
  "ai.dynamic_batch_size": 64,
  "ai.verifier_status": "accepted",
  "ai.task_status": "verified_success"
}
```

Power, grid carbon, PUE, WUE, and water stress should generally be metric streams or resource attributes correlated by time, region, hardware pool, and route. They should not be stuffed into trace headers.

### **Metric Sources**

| Metric Family | Source | Typical Granularity | Notes |
| :---- | :---- | :---- | :---- |
| **Accelerator power** | NVML, RAPL, vendor telemetry, BMC, rack PDU, lab profiler. | Device, node, rack, or pool. | Per-request attribution usually requires allocation. |
| **Utilization** | Scheduler, serving engine, GPU telemetry. | Request, batch, node, pool. | Needed to separate idle draw from useful work. |
| **Token / cache metrics** | Gateway, serving engine, tracing spans. | Request or span. | Connects software behavior to physical cost. |
| **Grid carbon intensity** | Grid APIs, utility data, regional estimates. | Region and time window. | Marginal intensity preferred for scheduling when available. |
| **PUE / WUE** | Facility telemetry or provider disclosure. | Facility, region, or provider estimate. | Often not available at request granularity. |
| **Embodied carbon** | Vendor PCF, EPD, LCA, procurement record. | Hardware class or asset. | Must be allocated by documented method. |
| **Outcome status** | Evaluation, policy, verification, user/task success. | Request or workflow. | Required for energy per verified task. |

### **Derived Sustainability Metrics**

| Metric | Use |
| :---- | :---- |
| **Energy per Request** | Estimate execution-path energy across model, retrieval, tools, and overhead. |
| **Energy per Verified Task** | Compare architectures by accepted useful outcomes. |
| **Waste Energy Ratio** | Expose retries, failures, rejected outputs, and unverified work. |
| **Carbon per Request** | Estimate operational emissions by time and region. |
| **Carbon per Verified Task** | Combine outcome quality with energy and carbon intensity. |
| **Water per Request / Task** | Estimate direct and indirect water impact with confidence labels. |
| **Embodied Carbon Allocation** | Attach hardware lifecycle impact to workload classes. |
| **Avoided Footprint** | Compare optimized route to documented baseline route. |

Production systems should use sampling, profiling, and allocation where direct measurement is too costly. Sustainability telemetry that costs more to collect than it saves is just another tiny ceremonial bonfire.

## **Lifecycle Impact Model**

A sustainable AI strategy must account for hardware lifecycle impact, not merely operational electricity. Accelerators, high-bandwidth memory, networking, storage, cooling equipment, substations, concrete, steel, and eventual e-waste all contribute to the system footprint.

Lifecycle numbers should be labeled by evidence confidence:

| Evidence Class | Meaning | Use |
| :---- | :---- | :---- |
| **Vendor PCF** | Product carbon footprint published by manufacturer. | Useful for procurement and comparison, but vendor assumptions should be reviewed. |
| **Third-party-reviewed PCF / EPD** | Product footprint reviewed under a recognized methodology. | Stronger procurement evidence. |
| **Independent LCA** | Lifecycle assessment by independent researchers or auditors. | Strongest external challenge to vendor assumptions. |
| **Internal allocation estimate** | Organization’s allocation of hardware/facility footprint to workloads. | Useful for engineering decisions, not sufficient alone for external claims. |

### **Amortized Embodied Carbon of Accelerators**

High-performance AI accelerators have substantial cradle-to-gate emissions from semiconductor fabrication, HBM, integrated circuits, substrates, power delivery, cooling assemblies, and assembly. NVIDIA’s published HGX H100 Product Carbon Footprint summary reports 1,312 kg CO2e cradle-to-gate for one HGX H100 GPU baseboard, with materials and components dominating the footprint.

```text
HGX H100 Baseboard PCF Example

Cradle-to-gate footprint:
  1,312 kg CO2e per HGX H100 baseboard

Major contributors:
  materials and components: 91%
    high-bandwidth memory: 42%
    integrated circuits:   25%
    thermals/cooling:      18%
  assembly:                8.6%
  transport/logistics:     0.4%

Evidence class:
  vendor PCF summary
```

These figures should not be blindly copied into all deployments. They should be used as a documented input to workload allocation, procurement comparison, and hardware refresh analysis.

### **Generational Efficiency and Replacement Tradeoffs**

Newer hardware may reduce energy per task or embodied carbon intensity, but replacing hardware too aggressively can increase e-waste and strand embodied emissions. A hardware upgrade is sustainable only when the lifecycle benefit exceeds the lifecycle cost under expected utilization.

| Upgrade Question | Required Evidence |
| :---- | :---- |
| Does the new hardware reduce energy per verified task? | Production-like benchmark under expected batch, context, and route patterns. |
| Does the efficiency gain outweigh added embodied carbon? | Amortized lifecycle comparison over expected useful life. |
| Can existing hardware be repurposed? | Secondary workload plan, lower-tier route, development pool, or resale/refurbishment path. |
| Will utilization actually improve? | Scheduler and demand forecast, not only theoretical FLOP claims. |
| Are cooling, water, and facility constraints improved or worsened? | Regional PUE/WUE, thermal density, water stress, and rack-level constraints. |

### **Decommissioning, E-Waste, and Zombie Infrastructure**

Sustainable AI must manage retirement as deliberately as deployment. Idle accelerators, obsolete vector indexes, persistent debug traces, duplicated datasets, abandoned notebooks, and unused model replicas consume storage, power, cooling, administrative attention, and sometimes license cost.

| Lifecycle Risk | Control |
| :---- | :---- |
| **Idle accelerators** | Scale-to-zero, reservation review, utilization thresholds, workload consolidation. |
| **Cold model replicas** | Replica TTL, route review, autoscaling, serving pool consolidation. |
| **Obsolete vector indexes** | Index lifecycle policy, source freshness checks, deduplication, archival tiering. |
| **Redundant datasets** | Dataset registry, lineage-aware garbage collection, retention policy. |
| **Persistent debug logs** | Retention limits, sampling, redaction, secure archive for required evidence. |
| **Hardware retirement** | Refurbish, redeploy, resell, recycle through certified e-waste channels. |
| **Facility expansion** | Whole-life carbon assessment covering building, substation, cooling, and backup generation. |

### **Lifecycle Review Triggers**

| Trigger | Required Review |
| :---- | :---- |
| **New model route** | Model size, quantization, hardware target, energy per verified task, carbon/water estimate. |
| **Workload volume surge** | Routing, batching, cache, scheduling, and utilization review. |
| **Accelerator purchase or lease** | Whole-life carbon and utilization plan. |
| **New region or provider** | Grid carbon, water stress, PUE/WUE, data residency, provider telemetry. |
| **Index or dataset growth** | Deduplication, pruning, storage tiering, source lifecycle review. |
| **Retry or timeout spike** | Rework energy, route rollback, failure-mode evaluation. |
| **Hardware refresh proposal** | Embodied-carbon payback and e-waste plan. |
| **Vendor footprint update** | Recalculate workload allocation and procurement score. |

## **Sustainability Governance Checklist**

Sustainability governance should be attached to AI system inventory, procurement, release gates, and operational review. Controls should be risk-tiered. A low-volume internal assistant does not need the same evidence burden as a high-volume public model route or a dedicated accelerator fleet.

### **System Inventory Integration**

Every AI system registration should include an environmental profile proportional to its expected scale and risk.

| Inventory Field | Required For | Purpose |
| :---- | :---- | :---- |
| **Expected workload volume** | All production systems. | Estimate request count, token volume, batch jobs, and growth rate. |
| **Model routes and fallback routes** | All production systems. | Identify energy, carbon, and cost exposure by route. |
| **Task class and quality floor** | All production systems. | Prevent tiny-model trap and unnecessary frontier routing. |
| **Quantization / precision policy** | Systems with hosted models or dedicated serving. | Define approved precision tiers and exception process. |
| **Cache policy** | Systems using semantic, prefix, retrieval, or response caches. | Prevent stale, unauthorized, or wasteful cache behavior. |
| **Scheduling flexibility** | Batch/offline workloads. | Enable carbon-aware and water-aware execution. |
| **Region / provider / facility class** | Medium and high-volume systems. | Track grid, water, residency, and provider exposure. |
| **Embodied carbon allocation method** | Dedicated hardware or large reserved capacity. | Assign lifecycle footprint to workloads. |
| **Retention and deletion policy** | All systems with logs, traces, prompts, indexes, or eval artifacts. | Avoid zombie storage and privacy risk. |
| **Review and retirement triggers** | All production systems. | Force lifecycle reassessment when volume, route, or hardware changes. |

### **Procurement and Vendor Audit Controls**

Vendor sustainability requirements should be tiered by volume, criticality, data sensitivity, and integration depth.

| Control | Required | Preferred | Roadmap / Compensating Control |
| :---- | :---- | :---- | :---- |
| **Emissions reporting** | Corporate or service-level emissions disclosure for material vendors. | Workload-class or region-level AI emissions reporting. | Proxy estimates using tokens, route, region, and provider-published factors. |
| **Energy telemetry** | Basic usage and region reporting for high-volume providers. | Trace-level or workload-level energy estimates. | Internal allocation model with documented assumptions. |
| **Water reporting** | WUE or water-risk disclosure for material data-center providers. | Region/facility-level direct water and cooling method. | Avoid high-risk regions or require contractual improvement plan. |
| **PUE / facility efficiency** | Facility or regional efficiency disclosure for dedicated capacity. | Time-varying PUE or cooling-load data. | Use provider class averages with confidence label. |
| **Hardware lifecycle evidence** | Hardware model, refresh cycle, and lifecycle policy for dedicated or leased clusters. | PCF, EPD, or independent LCA for accelerators and servers. | Internal lifecycle allocation using vendor documentation. |
| **Carbon-aware scheduling support** | Required for large flexible workloads where provider supports it. | Region/time scheduling APIs or clean-energy windows. | Batch locally or defer until provider exposes controls. |
| **Telemetry export APIs** | Required where sustainability claims are made from provider-hosted workloads. | Standardized export of tokens, route, region, energy estimate, and carbon estimate. | No external sustainability claims beyond documented proxy method. |
| **Data deletion / garbage collection** | Required for all vendors handling customer data, logs, prompts, or derived artifacts. | Automated retention APIs and deletion certificates. | Contractual retention caps and periodic deletion audits. |

### **Release Gate Questions**

Before production launch or major route change, the system owner should answer:

| Question | Gate Outcome |
| :---- | :---- |
| Is the selected model route the smallest sufficiently capable route? | If no, justify exception or redesign. |
| Have failure and retry rates been included in resource estimates? | If no, run representative evals. |
| Can flexible workloads be scheduled by carbon, water, or cost windows? | If yes, enable scheduling policy. |
| Are caches scoped by tenant, source, policy, model, and freshness? | If no, block semantic/prefix cache use. |
| Does telemetry distinguish measured, estimated, allocated, and vendor-reported values? | If no, downgrade confidence of sustainability claims. |
| Is there a retirement trigger for routes, indexes, logs, and hardware allocations? | If no, add lifecycle policy. |

## **Sustainability Theater Diagnostic**

Many enterprise Green AI programs suffer from "sustainability theater"—the practice of deploying decorative dashboards and publishing leaf-branded marketing summaries that fail to drive real environmental or architectural change. Platform teams must run this diagnostic to identify and eliminate standard theater anti-patterns.

### **Diagnostic Check 1: The Green Dashboard Fallacy**

* **The Theater Pattern:** Visualizing tree-planting graphs or displaying average grid-carbon metrics on SRE dashboards, while continuing to route trivial lookup tasks to unquantized, 400B-parameter frontier models running on fossil-heavy grids.13  
* **Diagnostic Check:** Ask: *Does the telemetry dashboard programmatically change gateway routing tables, alter queue scheduling, or trigger auto-scaling scale-to-zero routines?* If the answer is no, the dashboard is theater.

### **Diagnostic Check 2: The Offset Illusion**

* **The Theater Pattern:** Claims of "carbon neutrality" or "zero-emissions execution" based on the purchase of annual Renewable Energy Certificates (RECs) or speculative carbon offset credits, while operating physical servers on grids dominated by coal and gas generation.1  
* **Diagnostic Check:** Ask: *Is the workload's electrical consumption actively matched hour-by-hour with local, 24/7 carbon-free energy (CFE) on the specific grid where the computation occurs?* 3 If the matching is calculated annually or globally, the claim is ungrounded.

### **Diagnostic Check 3: Generative and Traditional Blurring**

* **The Theater Pattern:** Publishing reports that highlight massive emissions reductions achieved by deploying AI models (e.g., claiming that AI reduced data center cooling energy by 15%), while conflating highly efficient predictive/traditional ML with highly resource-intensive generative LLMs.22  
* **Diagnostic Check:** Ask: *Does corporate carbon reporting explicitly isolate generative AI training and inference volumes from traditional machine learning tasks?* 22

### **Diagnostic Check 4: The Tiny Model Trap**

* **The Theater Pattern:** Forcing the deployment of highly quantized, extremely small models (e.g., 1B parameters) on tasks that require complex, multi-step logical reasoning to meet "green computing" targets.  
* **Diagnostic Check:** Ask: *What is the downstream failure rate?* If the tiny model repeatedly hallucinates, outputting syntactically invalid structured schemas or incorrect logic that forces the system to initiate multiple retry loops or forces human engineers to spend hours manually correcting errors, the tiny model is less sustainable than a larger model that solves the task correctly on the first forward pass.37

### **Diagnostic Check 5: The Zombie Workload Swarm**

* **The Theater Pattern:** Hundreds of forgotten development notebooks, under-utilized model replicas, obsolete vector indices, and redundant dataset copies sitting warm on high-cost cloud resources, while the platform team focuses on micro-optimizing prompt lengths.26  
* **Diagnostic Check:** Ask: *Are there active, automated network and traffic sweeps to identify any server or container running with near-zero CPU or GPU utilization for over 48 hours, programmatically shutting down these "zombie" environments?* 26

## **Sustainable AI Decision Record Template**

Every major model selection, regional deployment, hardware procurement, or routing configuration should be documented using a Sustainable AI Architecture Decision Record. The goal is to make sustainability tradeoffs explicit before production deployment.

```markdown
# Architectural Decision Record: SUST-AI-<ID>

## Title

## Status
Proposed | Accepted | Superseded | Retired

## Context and Problem Statement
- Business requirement:
- User population:
- Expected workload volume:
- Input profile:
- Output profile:
- Latency / availability SLO:
- Quality / safety / governance floor:
- Current baseline route or architecture:

## Scope Boundary
Mark each metric as included, excluded, or unknown.

| Boundary Item | Included? | Measurement Method | Confidence |
| :---- | :---- | :---- | :---- |
| Model inference energy | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Retrieval and vector search | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Tool calls and external APIs | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Storage and logs | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Network transfer | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Facility PUE/WUE | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Embodied hardware carbon | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |
| Human rework / retry cost | Yes / No / Unknown | measured / estimated / allocated / vendor-reported | High / Medium / Low |

## Architectural Design Constraints
- Maximum latency:
- Minimum quality / accuracy:
- Safety / policy constraints:
- Data residency constraints:
- Privacy constraints:
- Cost ceiling:
- Carbon objective:
- Water / region objective:
- Retention and deletion requirements:

## Evaluated Alternatives

### Option 1
- Model / route:
- Hardware / provider / region:
- Precision / quantization:
- Scheduling mode:
- Estimated or measured energy per request:
- Estimated or measured energy per verified task:
- Estimated or measured carbon per verified task:
- Estimated or measured water per verified task:
- Expected failure / retry / rework rate:
- Confidence level:
- Main risks:

### Option 2
- Model / route:
- Hardware / provider / region:
- Precision / quantization:
- Scheduling mode:
- Estimated or measured energy per request:
- Estimated or measured energy per verified task:
- Estimated or measured carbon per verified task:
- Estimated or measured water per verified task:
- Expected failure / retry / rework rate:
- Confidence level:
- Main risks:

## Decision Outcome
- Chosen option:
- Justification:
- Why smaller routes were rejected:
- Why larger routes were rejected:
- How the choice balances quality, latency, cost, carbon, water, privacy, and governance:

## Implementation Plan
- Routing rule:
- Quantization / precision:
- Cache policy:
- Context compression policy:
- Carbon-aware scheduling policy:
- Telemetry integration:
- Verification / eval gate:
- Rollback or degraded-mode path:

## Monitoring Plan
- Energy metric:
- Carbon metric:
- Water metric:
- Waste / retry metric:
- Utilization metric:
- Quality metric:
- Review cadence:

## Review Triggers
- Workload volume growth:
- Route/model/provider change:
- Hardware generation change:
- Regional grid/water change:
- Failure or retry spike:
- Cost spike:
- New regulatory/procurement requirement:

## Decommissioning Policy
- Retirement condition:
- Data/index/log cleanup:
- Hardware or reserved-capacity release:
- Vendor exit or route migration plan:

## Sign-off
- System Owner:
- Platform / Infrastructure Lead:
- FinOps Owner:
- Sustainability / ESG Owner:
- Governance / Compliance Owner:
- Date:
```

## **Cross-Canon Handoff Map**

AI-ENG-AE defines sustainability as an architectural constraint across model selection, serving, routing, scheduling, telemetry, procurement, and lifecycle management. It consumes runtime, governance, evaluation, and operations evidence from surrounding canon layers and returns sustainability controls back into routing, procurement, and lifecycle review.

| Canon Report | Handoff Into AE | AE Sustainability Use | Handoff Back |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context size, memory state, context compiler policy. | Measures context bloat and prefill energy. | Context budgets and compression review triggers. |
| **AI-ENG-D — Corpus Engineering** | Corpus lifecycle state, source duplication, dataset size, retention. | Identifies storage bloat, duplicate corpora, stale datasets, and zombie data. | Dataset pruning, retention, archival, and lifecycle controls. |
| **AI-ENG-E — Retrieval Pipeline** | Retrieval traces, chunk counts, reranker use, citation evidence. | Measures retrieved-token usefulness and retrieval energy overhead. | Retrieval efficiency metrics, chunking review, reranker cost policy. |
| **AI-ENG-F — Freshness and Conflict Detection** | Freshness state, stale-source ratio, conflict packets. | Avoids wasting compute on stale or conflicting evidence. | Freshness-aware scheduling and index cleanup triggers. |
| **AI-ENG-J — Throughput Mechanics** | Batching, queueing, token throughput, prefill/decode behavior. | Optimizes useful utilization and energy per token/request. | Sustainability-aware batching and queue policies. |
| **AI-ENG-K — Weight Dynamics** | Quantization, pruning, distillation, adapters, decoding behavior. | Selects precision and model variants by energy-quality tradeoff. | Quantization and model-size review requirements. |
| **AI-ENG-L — Serving Architecture** | Model routes, serving pools, autoscaling, cache and replica topology. | Controls idle draw, cold replicas, route energy, and scale-to-zero. | Energy-aware route manifests and utilization SLOs. |
| **AI-ENG-U — Supply Chain Security** | Hardware/software provenance, artifact registry, sandbox profile, AI-BOM. | Distinguishes trusted PCF/EPD/vendor evidence from weak footprint claims. | Sustainability fields in procurement and artifact inventory. |
| **AI-ENG-V — Resource Abuse** | Token ceilings, loop controls, denial-of-wallet, runaway agent signals. | Treats runaway loops and retries as energy and carbon incidents. | Waste-energy thresholds and sustainability incident triggers. |
| **AI-ENG-W — UX Resilience** | Degraded modes, fallback paths, user continuity. | Defines sustainable degraded modes that preserve safety and useful service. | Environmental degradation disclosures and fallback policies. |
| **AI-ENG-Z — Strategic Telemetry** | Trace IDs, spans, metrics, redaction, routing telemetry. | Joins AI traces with power, carbon, water, utilization, and outcome metrics. | Sustainability metric schema and dashboard controls. |
| **AI-ENG-AA — Evaluation Architecture** | Golden sets, task success, regression gates, verifier outcomes. | Defines “verified task” for energy/carbon per useful outcome. | Efficiency-weighted eval reporting and route acceptance gates. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay artifacts, audit references. | Attaches sustainability decisions to replayable route and telemetry evidence. | Sustainable ADR evidence, source-confidence labels, audit references. |
| **AI-ENG-AC — AI Operations** | Incident response, rollback, runbooks, containment, postmortems. | Treats energy waste, runaway loops, zombie infrastructure, and carbon-budget breaches as operational events. | Sustainability runbooks and incident-to-efficiency improvements. |
| **AI-ENG-AD — Governance Architecture** | Policy-as-code, inventory, procurement, vendor review, lifecycle ownership. | Makes environmental metadata executable at approval, procurement, and release gates. | Sustainability inventory fields, procurement controls, review triggers. |
| **AI-ENG-AJ — Reference Architectures** | Gateway, telemetry, scheduler, registry, and platform blueprints. | Implements sustainable routing, scheduling, telemetry, and lifecycle patterns. | Reference designs for energy-aware AI platforms. |

The core handoff principle is that sustainability cannot live in a separate reporting layer. It must be wired into the same routes, gates, inventories, evaluations, runbooks, and procurement controls that govern production AI behavior.

#### **Works cited**

1. Carbon-Aware Scheduling of AI Data Center Workloads Using Environmental and Energy Grid Forecasts - ESS Open Archive, accessed June 14, 2026, [https://essopenarchive.org/doi/pdf/10.22541/essoar.177248651.16591616](https://essopenarchive.org/doi/pdf/10.22541/essoar.177248651.16591616)  
2. Carbon-Aware AI Workload Scheduling With Renewable Energy Sources - Scribd, accessed June 14, 2026, [https://www.scribd.com/document/1007342445/Carbon-Aware-AI-Workload-Scheduling-With-Renewable-Energy-Sources](https://www.scribd.com/document/1007342445/Carbon-Aware-AI-Workload-Scheduling-With-Renewable-Energy-Sources)  
3. Google data centers shift their computations to cleaner times and locations - Electricity Maps, accessed June 14, 2026, [https://www.electricitymaps.com/resources/case-studies/google-data-centers-shift-their-computations-to-cleaner-times-and-locations](https://www.electricitymaps.com/resources/case-studies/google-data-centers-shift-their-computations-to-cleaner-times-and-locations)  
4. Carbon-Aware Compute–Power Scheduling for AI Data Centers with Microgrid Prosumer Operations - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2605.03751v1](https://arxiv.org/html/2605.03751v1)  
5. Embodied carbon vs. operational carbon | One Click LCA, accessed June 14, 2026, [https://oneclicklca.com/en-us/resources/articles/embodied-carbon-vs-operational-carbon](https://oneclicklca.com/en-us/resources/articles/embodied-carbon-vs-operational-carbon)  
6. Embodied Carbon and Its Role in Product Development - Ecochain, accessed June 14, 2026, [https://ecochain.com/blog/embodied-carbon/](https://ecochain.com/blog/embodied-carbon/)  
7. Understanding embodied and operational carbon in data centers: ESG considerations, accessed June 14, 2026, [https://www.gresb.com/understanding-embodied-and-operational-carbon-in-data-centers-esg-considerations/](https://www.gresb.com/understanding-embodied-and-operational-carbon-in-data-centers-esg-considerations/)  
8. Product Carbon Footprint (PCF) Summary for NVIDIA HGX H100, accessed June 14, 2026, [https://images.nvidia.com/aem-dam/Solutions/documents/HGX-H100-PCF-Summary.pdf](https://images.nvidia.com/aem-dam/Solutions/documents/HGX-H100-PCF-Summary.pdf)  
9. Carbon Emissions of NVIDIA A100 and H100 GPUs with 100% Renewable Energy, accessed June 14, 2026, [https://massedcompute.com/faq-answers/?question=What%20are%20the%20estimated%20carbon%20emissions%20of%20NVIDIA%27s%20A100%20and%20H100%20GPUs%20in%20a%20data%20center%20with%20a%20100%%20renewable%20energy%20source?](https://massedcompute.com/faq-answers/?question=What%2520are%2520the%2520estimated%2520carbon%2520emissions%2520of%2520NVIDIA%2527s%2520A100%2520and%2520H100%2520GPUs%2520in%2520a%2520data%2520center%2520with%2520a%2520100%25%2520renewable%2520energy%2520source?)  
10. Life-Cycle Emissions of AI Hardware: A Cradle-To-Grave Approach and Generational Trends - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2502.01671v1](https://arxiv.org/html/2502.01671v1)  
11. Ironwood TPUs deliver 3.7x carbon efficiency gains | Google Cloud Blog, accessed June 14, 2026, [https://cloud.google.com/blog/topics/systems/ironwood-tpus-deliver-37x-carbon-efficiency-gains](https://cloud.google.com/blog/topics/systems/ironwood-tpus-deliver-37x-carbon-efficiency-gains)  
12. FinOps in GCCs: Managing AI & Cloud Spend with Intelligence - Polestar Analytics, accessed June 14, 2026, [https://www.polestaranalytics.com/blog/how-finops-in-gccs-is-transforming-finance-operations-with-ai-and-automation](https://www.polestaranalytics.com/blog/how-finops-in-gccs-is-transforming-finance-operations-with-ai-and-automation)  
13. Where Do the Joules Go? Diagnosing Inference Energy Consumption - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2601.22076v1](https://arxiv.org/html/2601.22076v1)  
14. Babbling Suppression: Making LLMs Greener One Token at a Time - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2604.06755v1](https://arxiv.org/html/2604.06755v1)  
15. How much heat does an AI data centre produce, and where are they located? - Al Jazeera, accessed June 14, 2026, [https://www.aljazeera.com/news/2026/6/11/how-much-heat-does-an-ai-data-centre-produce-and-where-are-they-located](https://www.aljazeera.com/news/2026/6/11/how-much-heat-does-an-ai-data-centre-produce-and-where-are-they-located)  
16. Whole Life Carbon Assessment vs. Embodied Carbon: Key Differences Explained - Cerclos, accessed June 14, 2026, [https://cerclos.com/whole-life-carbon-assessment-vs-embodied-carbon-key-differences-explained/](https://cerclos.com/whole-life-carbon-assessment-vs-embodied-carbon-key-differences-explained/)  
17. Majority of US’s new AI datacenters to be built on drought-hit land, accessed June 14, 2026, [https://www.theguardian.com/us-news/2026/jun/08/datacenter-ai-drought-water](https://www.theguardian.com/us-news/2026/jun/08/datacenter-ai-drought-water)  
18. [2508.12590] Energy-Efficient Wireless LLM Inference via Uncertainty and Importance-Aware Speculative Decoding - arXiv, accessed June 14, 2026, [https://arxiv.org/abs/2508.12590](https://arxiv.org/abs/2508.12590)  
19. Local AI vs Cloud AI in 2026: When to Run Models on Your Own Hardware | MindStudio, accessed June 14, 2026, [https://www.mindstudio.ai/blog/local-ai-vs-cloud-ai-2026](https://www.mindstudio.ai/blog/local-ai-vs-cloud-ai-2026)  
20. Data Drain: The Land and Water Impacts of the AI Boom - Lincoln Institute of Land Policy, accessed June 14, 2026, [https://www.lincolninst.edu/publications/land-lines-magazine/articles/land-water-impacts-data-centers/](https://www.lincolninst.edu/publications/land-lines-magazine/articles/land-water-impacts-data-centers/)  
21. Global energy demands within the AI regulatory landscape | Brookings, accessed June 14, 2026, [https://www.brookings.edu/articles/global-energy-demands-within-the-ai-regulatory-landscape/](https://www.brookings.edu/articles/global-energy-demands-within-the-ai-regulatory-landscape/)  
22. Big Tech Accused of AI 'Greenwashing' - Resilience.org, accessed June 14, 2026, [https://www.resilience.org/stories/2026-02-19/big-tech-accused-of-ai-greenwashing/](https://www.resilience.org/stories/2026-02-19/big-tech-accused-of-ai-greenwashing/)  
23. The Great Green AI Hoax Machine | TechPolicy.Press, accessed June 14, 2026, [https://www.techpolicy.press/the-great-green-ai-hoax-machine/](https://www.techpolicy.press/the-great-green-ai-hoax-machine/)  
24. Report exposes Big Tech's AI climate hoax: 74% of industry's claims about AI's climate benefits are unproven - Stand.earth, accessed June 14, 2026, [https://stand.earth/press-releases/report-exposes-big-techs-ai-climate-hoax-74-of-industrys-claims-about-ais-climate-benefits-are-unproven/](https://stand.earth/press-releases/report-exposes-big-techs-ai-climate-hoax-74-of-industrys-claims-about-ais-climate-benefits-are-unproven/)  
25. Report exposes Big Tech's AI climate hoax: 74% of industry's claims about AI's climate benefits are unproven - Green Web Foundation, accessed June 14, 2026, [https://www.thegreenwebfoundation.org/news/report-exposes-big-techs-ai-climate-hoax-74-of-industrys-claims-about-ais-climate-benefits-are-unproven/](https://www.thegreenwebfoundation.org/news/report-exposes-big-techs-ai-climate-hoax-74-of-industrys-claims-about-ais-climate-benefits-are-unproven/)  
26. Zombie Workloads: The Walking Dead of Your Cloud Bill | by FinOps Universe | Medium, accessed June 14, 2026, [https://finopsuniverse.medium.com/zombie-workloads-the-walking-dead-of-your-cloud-bill-585756ce44da](https://finopsuniverse.medium.com/zombie-workloads-the-walking-dead-of-your-cloud-bill-585756ce44da)  
27. Gartner: Data Centres Electricity Consumption Up 26% in 2026, accessed June 14, 2026, [https://energydigital.com/news/gartner-data-centres-electricity-consumption-up-26-in-2026](https://energydigital.com/news/gartner-data-centres-electricity-consumption-up-26-in-2026)  
28. AI, Data Centers, and the U.S. Electric Grid: A Watershed Moment, accessed June 14, 2026, [https://www.belfercenter.org/research-analysis/ai-data-centers-us-electric-grid](https://www.belfercenter.org/research-analysis/ai-data-centers-us-electric-grid)  
29. ai-eng-ad-governance-architecture.md  
30. Executive summary – Key Questions on Energy and AI – Analysis - IEA, accessed June 14, 2026, [https://www.iea.org/reports/key-questions-on-energy-and-ai/executive-summary](https://www.iea.org/reports/key-questions-on-energy-and-ai/executive-summary)  
31. AI's secret water crisis: How data centres are draining freshwater reserves across the world, accessed June 14, 2026, [https://timesofindia.indiatimes.com/science/ais-secret-water-crisis-how-data-centres-are-draining-freshwater-reserves-across-the-world/articleshow/131590203.cms](https://timesofindia.indiatimes.com/science/ais-secret-water-crisis-how-data-centres-are-draining-freshwater-reserves-across-the-world/articleshow/131590203.cms)  
32. AI Cost Optimization 2026: GPU, LLM & Infrastructure Spend Guide | Opslyft, accessed June 14, 2026, [https://www.opslyft.com/guides/ai-cost-optimization](https://www.opslyft.com/guides/ai-cost-optimization)  
33. The AI Boom Has a Water Bill, accessed June 14, 2026, [https://medium.com/activated-thinker/the-ai-boom-has-a-water-bill-a77fae04505a](https://medium.com/activated-thinker/the-ai-boom-has-a-water-bill-a77fae04505a)  
34. AI Energy Consumption Statistics - AIMultiple, accessed June 14, 2026, [https://aimultiple.com/ai-energy-consumption](https://aimultiple.com/ai-energy-consumption)  
35. TPUs improved carbon-efficiency of AI workloads by 3x | Google Cloud Blog, accessed June 14, 2026, [https://cloud.google.com/blog/topics/sustainability/tpus-improved-carbon-efficiency-of-ai-workloads-by-3x](https://cloud.google.com/blog/topics/sustainability/tpus-improved-carbon-efficiency-of-ai-workloads-by-3x)  
36. Towards Green AI: Decoding the Energy of LLM Inference in Software Development - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2602.05712v1](https://arxiv.org/html/2602.05712v1)  
37. Tokens per Joule: How to Quantify and Reduce the Energy Footprint of Clinical LLM Inference - John Snow Labs, accessed June 14, 2026, [https://www.johnsnowlabs.com/tokens-per-joule-how-to-quantify-and-reduce-the-energy-footprint-of-clinical-llm-inference/](https://www.johnsnowlabs.com/tokens-per-joule-how-to-quantify-and-reduce-the-energy-footprint-of-clinical-llm-inference/)  
38. How to Build Context Compression - OneUptime, accessed June 14, 2026, [https://oneuptime.com/blog/post/2026-01-30-context-compression/view](https://oneuptime.com/blog/post/2026-01-30-context-compression/view)  
39. Carbon-Aware Model Training: Scheduling GPU Workloads Around Electricity Carbon Intensity - DEV Community, accessed June 14, 2026, [https://dev.to/nilofer_tweets/carbon-aware-model-training-scheduling-gpu-workloads-around-electricity-carbon-intensity-b4b](https://dev.to/nilofer_tweets/carbon-aware-model-training-scheduling-gpu-workloads-around-electricity-carbon-intensity-b4b)  
40. (PDF) Towards Green AI: Decoding the Energy of LLM Inference in Software Development, accessed June 14, 2026, [https://www.researchgate.net/publication/400506146_Towards_Green_AI_Decoding_the_Energy_of_LLM_Inference_in_Software_Development](https://www.researchgate.net/publication/400506146_Towards_Green_AI_Decoding_the_Energy_of_LLM_Inference_in_Software_Development)  
41. LLM Inference Energy Use - Emergent Mind, accessed June 14, 2026, [https://www.emergentmind.com/topics/llm-inference-energy-consumption](https://www.emergentmind.com/topics/llm-inference-energy-consumption)  
42. Context Compression Before the LLM: Cutting Tokens Without Cutting Recall - DEV Community, accessed June 14, 2026, [https://dev.to/gabrielanhaia/context-compression-before-the-llm-cutting-tokens-without-cutting-recall-9hh](https://dev.to/gabrielanhaia/context-compression-before-the-llm-cutting-tokens-without-cutting-recall-9hh)  
43. Characterizing LLM Inference Energy-Performance Tradeoffs across Workloads and GPU Scaling - Shashikant Ilager, accessed June 14, 2026, [https://shashikantilager.com/assets/pdf/publications/LLM_charecterization_ccgrid_2026.pdf](https://shashikantilager.com/assets/pdf/publications/LLM_charecterization_ccgrid_2026.pdf)  
44. An Introduction to Speculative Decoding for Reducing Latency in AI Inference | NVIDIA Technical Blog, accessed June 14, 2026, [https://developer.nvidia.com/blog/an-introduction-to-speculative-decoding-for-reducing-latency-in-ai-inference/](https://developer.nvidia.com/blog/an-introduction-to-speculative-decoding-for-reducing-latency-in-ai-inference/)  
45. Babbling Suppression: Making LLMs Greener One Token at a Time - arXiv, accessed June 14, 2026, [https://arxiv.org/pdf/2604.06755](https://arxiv.org/pdf/2604.06755)  
46. TokenPowerBench: Benchmarking the Power Consumption of LLM Inference - AAAI Publications, accessed June 14, 2026, [https://ojs.aaai.org/index.php/AAAI/article/view/40535/44496](https://ojs.aaai.org/index.php/AAAI/article/view/40535/44496)  
47. Should I Use Cloud or Local AI Models for My Project? - Zen van Riel, accessed June 14, 2026, [https://zenvanriel.com/ai-engineer-blog/should-i-use-cloud-or-local-ai-models-complete-guide/](https://zenvanriel.com/ai-engineer-blog/should-i-use-cloud-or-local-ai-models-complete-guide/)  
48. NVIDIA HGX B200 Reduces Embodied Carbon Emissions Intensity | NVIDIA Technical Blog, accessed June 14, 2026, [https://developer.nvidia.com/blog/nvidia-hgx-b200-reduces-embodied-carbon-emissions-intensity/](https://developer.nvidia.com/blog/nvidia-hgx-b200-reduces-embodied-carbon-emissions-intensity/)  
49. Context Compression Techniques (2026) - SurePrompts, accessed June 14, 2026, [https://sureprompts.com/blog/context-compression-techniques](https://sureprompts.com/blog/context-compression-techniques)  
50. Context compression finally works in production: new research cuts LLM input 16x without the accuracy hit | VentureBeat, accessed June 14, 2026, [https://venturebeat.com/data/context-compression-finally-works-in-production-new-research-cuts-llm-input-16x-without-the-accuracy-hit](https://venturebeat.com/data/context-compression-finally-works-in-production-new-research-cuts-llm-input-16x-without-the-accuracy-hit)  
51. GreenOps & FinOps – WisdomAI: Why zombie data is the hidden cost of cloud, accessed June 14, 2026, [https://www.computerweekly.com/blog/CW-Developer-Network/GreenOps-FinOps-WisdomAI-Why-zombie-data-is-the-hidden-cost-of-cloud](https://www.computerweekly.com/blog/CW-Developer-Network/GreenOps-FinOps-WisdomAI-Why-zombie-data-is-the-hidden-cost-of-cloud)  
52. Appendix A. - Greenpeace, accessed June 14, 2026, [https://www.greenpeace.org/static/planet4-eastasia-stateless/2025/04/c97a710a-energy_consumption_of_ai_appendix.pdf](https://www.greenpeace.org/static/planet4-eastasia-stateless/2025/04/c97a710a-energy_consumption_of_ai_appendix.pdf)  
53. How Shadow AI Creates Zombie Infrastructure - Netscout, accessed June 14, 2026, [https://www.netscout.com/blog/how-shadow-ai-creates-zombie-infrastructure](https://www.netscout.com/blog/how-shadow-ai-creates-zombie-infrastructure)  
54. Claims that AI can help fix climate dismissed as greenwashing - The Guardian, accessed June 14, 2026, [https://www.theguardian.com/technology/2026/feb/17/tech-companies-traditional-ai-generative-climate-breakdown-report](https://www.theguardian.com/technology/2026/feb/17/tech-companies-traditional-ai-generative-climate-breakdown-report)  
55. Datacenter & AI water use is overblown, accessed June 14, 2026, [https://www.reddit.com/r/artificial/comments/1u4128s/datacenter_ai_water_use_is_overblown/](https://www.reddit.com/r/artificial/comments/1u4128s/datacenter_ai_water_use_is_overblown/)

---