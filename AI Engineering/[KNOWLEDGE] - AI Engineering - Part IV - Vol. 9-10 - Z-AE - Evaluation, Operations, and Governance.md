# [KNOWLEDGE] - AI Engineering - Part IV - Vol. 9-10 - Z-AE - Evaluation, Operations, and Governance

[Volume 9. Z-AB Observability, Evaluation, and Verification](#volume-9-z-ab-observability-evaluation-and-verification)
  - [AI-ENG-Z — Strategic Telemetry - Traces, Tokens, Latency & Semantic Drift](#ai-eng-z--strategic-telemetry---traces-tokens-latency--semantic-drift)
  - [AI-ENG-AA — Evals Architecture: Ground Truth, Golden Sets & Regression Tests](#ai-eng-aa--evals-architecture-ground-truth-golden-sets--regression-tests)
  - [AI-ENG-AB — Verification Artifacts - Auditability, Reproducibility & Evidence Trails](#ai-eng-ab--verification-artifacts---auditability-reproducibility--evidence-trails)

---

# [Volume 9. Z-AB Observability, Evaluation, and Verification]

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
| **Green Dashboard Fallacy** | The diagnostic error of treating infrastructure health as sufficient evidence of AI-system health. | Hidden Failure Detection Rate | Behavioral and semantic checks supplement infrastructure metrics. |
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