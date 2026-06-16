# AI-ENG-V — Resource Abuse, Cost Bombs & Unbounded Consumption

The structural integrity of a high-dimensional artificial intelligence system depends on a fundamental law of resource governance: an execution path without a strictly enforced resource boundary is an active vulnerability. Traditional web services operate over deterministic interfaces where request payloads carry relatively uniform computational overhead. In contrast, generative AI architectures collapse the distinction between input data weight and downstream processing complexity, introducing execution surfaces where a single request can trigger massive, unpredicted, and adversarially inflated resource consumption.1  
Resource abuse is the systemic failure mode where an AI system consumes unbounded, disproportionate, or poorly attributed tokens, compute, retrieval, tool calls, latency, quota, storage, batch capacity, queue capacity, human review, or money relative to the authorized task.1 Treating resource boundaries as a post-hoc financial optimization task or a passive dashboard concern is a critical architectural error.4 Instead, resource control must be handled as a core security, reliability, and system boundary discipline.4  
This report defines the system mechanics, threat models, rate-limiting frameworks, and containment protocols required to isolate and secure AI execution paths.

## **Canon Placement and Continuity**

This report belongs to **Volume 7: S–V Failure, Security, and Hostile Environments** of the AI Engineering Systems Canon.5 It functions alongside its preceding components:

* **AI-ENG-S (Production Pathologies)**, which mapped behavioral failures including malformed output, tool loops, brittle chains, and runaway execution paths.5  
* **AI-ENG-T (Boundary Defense)**, which established logical separation boundaries for prompt injection, tenant isolation, permission-aware retrieval, cache scope, and context separation.6  
* **AI-ENG-U (AI Supply Chain Security)**, which hardens the underlying execution substrate, including model checkpoints, dataset provenance, document parsers, Model Context Protocol (MCP) servers, sandboxed environments, and egress control gateways.7

AI-ENG-V establishes the resource boundary, mapping how trusted, degraded, or compromised systems can consume too much, too long, too often, too expensively, or too broadly. It inherits and extends cost-attribution and inference-economics doctrines; throughput, batching, queueing, KV-cache, and memory-pressure doctrines; serving, rate-limiting, and tenant-aware routing frameworks; and loop-budget and action-verification protocols.5

## **Conceptual Glossary**

The systems-engineering parameters governing resource control and containment are defined in the primary conceptual glossary:

| Term | Technical Definition | Primary Operational Metric | Standard Target |
| :---- | :---- | :---- | :---- |
| **Denial-of-Wallet (DoW)** | An exploit vector targeting pay-per-token or pay-per-use utility pricing models to exhaust financial resources rather than causing raw hardware downtime. | Cost Velocity per Identity Tuple | No unapproved budget breach; unbudgeted overruns trigger immediate containment. |
| **Resource Envelope** | The complete set of multi-dimensional limits—tokens, turns, time, tools, dollars, queue slots, retrieval work, and human review—enclosing an active execution path. | Envelope Enforcement Coverage | Coverage enforced for every production execution path. |
| **Cost Bomb** | A crafted input payload or workflow state designed to trigger disproportionate resource consumption, latency inflation, quota burn, or billing spikes. | Amplification Cost Ratio | Amplification ratio stays within policy-defined tolerance; anomalies trigger circuit breakers. |
| **Context Flooding** | Overloading a model’s active context window with redundant, verbose, low-value, or adversarial data to dilute attention, increase prefill cost, or force expensive compression. | Context Utilization Density | Context utilization remains inside task/profile budget; high-priority evidence and instructions are preserved. |
| **Retrieval Flooding** | Forcing excessive vector, semantic, relational, page-rendering, or reranking operations through broad, ambiguous, or recursively expanded queries. | Retrieval Fan-Out Factor | Candidate fan-out remains inside retrieval budget for task class. |
| **Tool Exhaustion** | Depleting third-party API quotas, local process pools, browser workers, database connections, or tool-server capacity through unbounded model-driven invocations. | Tool Quota Exhaustion Frequency | No cascading quota failures; retries and tool calls are bounded by policy. |
| **Latency Inflation** | Deliberate or accidental degradation of request latency by forcing expensive parsing, retrieval, prefill, decoding, tool execution, rendering, or queue occupancy. | Tail Latency Delta | P95/P99 latency remains within service SLO for workload class. |
| **Quota Exhaustion** | Depleting upstream provider, tenant, model, API, or infrastructure rate limits, causing failures for unrelated users or sessions. | Upstream / Tenant Quota Burn Rate | Provider and tenant quota burn remain below alert thresholds. |
| **Loop Budget** | The maximum step count, wall-clock time, token volume, tool usage, retrieval work, and dollar spend allocated to an iterative or agentic workflow. | Loop Budget Utilization | Step and spend caps are defined by workflow risk profile and approval state. |
| **Progress Signal** | A verifiable change in task, system, environment, evidence, or action state indicating that an agent is converging toward a valid terminal state. | State Delta Metric | `state_delta != 0` before additional loop budget is granted. |
| **No-Progress State** | An execution state where an agent repeats identical or semantically equivalent plans, tool calls, retrieval queries, UI actions, or repair attempts without convergence. | State Repetition Count | Repeated states trigger bounded repair, replan, clarification, escalation, or halt. |
| **Budget-Aware Gateway** | An external proxy or control plane that terminates model/tool traffic to enforce cost limits, token caps, fallback routing, reservations, quotas, and circuit breakers. | Gateway Enforcement Latency | Gateway overhead remains within service latency budget. |
| **Tenant Resource Isolation** | Partitioning, scheduling, and budgeting compute resources so one tenant cannot starve neighbor workloads or consume shared provider quotas. | Tenant Impact / Noisy-Neighbor Score | No tenant can starve shared queues beyond policy threshold. |
| **Cost Attribution** | The trace associating every unit of consumed resource back to a tenant, user, session, workflow, model route, tool, corpus, batch job, or artifact. | Unattributed Cost Ratio | Unattributed spend is investigated; high-impact spend requires full attribution. |

## **Resource Abuse Taxonomy**

To establish strict boundary controls, the system must differentiate among the primary modes of resource exhaustion:

```
RESOURCE EXHAUSTION MATRIX

                              [ Resource Abuse ]
                                      |
        +-----------------------------+-----------------------------+
        |                             |                             |
        v                             v                             v
[ Availability Exhaustion ]   [ Financial Exhaustion ]      [ Quota Exhaustion ]
  GPU memory saturation         pay-per-token spikes          upstream TPM/RPM limits
  worker starvation             API credit depletion          provider key lockout
  connection pool lockups       recursive paid calls          tenant quota cascades

        +-----------------------------+-----------------------------+
        |                             |                             |
        v                             v                             v
[ Latency Inflation ]          [ Queue Starvation ]          [ Batch Runaway ]
  long prefill requests          head-of-line blocking         unbounded reindexing
  slow OCR/rendering             priority queue saturation     embedding backfill loops
  uncacheable prefixes           KV-cache preemption storms    disconnected workers

        +-----------------------------+-----------------------------+
                                      |
                                      v
                         [ Human-Review Exhaustion ]
                           excessive escalations
                           low-confidence floods
                           repetitive repair reviews

                                      |
                                      v
                         [ Agentic Amplification ]
                           recursive planning
                           repeated tool calls
                           expanding trace context
                           sub-agent delegation
```

* **Availability Exhaustion**: Renders the system unresponsive to legitimate users. In AI serving runtimes, this occurs when long-context prefill operations saturate the GPU memory cache, triggering preemption storms, execution swaps, or out-of-memory (OOM) kernel panics that crash the active container.  
* **Financial Exhaustion**: Depletes the deployer's operating budget without necessarily triggering hardware downtime. Attackers exploit unauthenticated endpoints, submitting prompts designed to maximize generation length or reasoning tokens, incurring high financial costs.  
* **Quota Exhaustion**: Occurs when unmonitored sessions hit upstream model provider rate limits, blocking all other sessions across shared API keys. This risk is heightened in multi-tenant SaaS deployments lacking per-tenant virtual key caps.  
* **Latency Inflation**: Starves downstream execution lines by deliberately slowing down response generation. An attacker crafts prompts requiring extensive layout-aware parsing, multi-page image processing, or broad vector retrieval to maximize prefill processing times.  
* **Queue Starvation**: Occurs when long-context, compute-bound prefill requests block shorter, memory-bound decode iterations. Under standard First-Come-First-Served (FCFS) scheduling, a single heavy request causes head-of-line blocking, degrading performance for all other users.  
* **Batch Runaway**: Occurs when background operations—such as document reindexing, embedding backfills, or evaluation sweeps—fail to bound their execution steps. A single parsing exception or schema drift can trap workers in infinite loop states, consuming tokens and API budgets.  
* **Human-Review Exhaustion**: Floods administrative queues with validation escalations. If an ingestion pipeline routes all low-confidence OCR segments or formatting violations to human moderators without strict limits, the queue becomes a bottleneck, stalling system workflows.  
* **Agentic Amplification**: Occurs when a single user request triggers a cascade of recursive planning, tool calling, and sub-agent delegation. Each step generates more trace history, causing context sizes to grow quadratically and multiplying costs.

## **The Resource Envelope Model**

To prevent resource-abuse patterns, the system must enforce strict operational envelopes outside the model's cognitive path. Relying on system prompts or inline instructions to control resource usage is fundamentally insecure. When models are confronted with complex contexts, long conversational histories, or adversarial inputs, their attention is diluted, leading them to ignore prompt-level constraints and generate unbounded outputs.5 The platform must enforce resource ceilings deterministically using a decoupled gateway and orchestrator architecture.9  
The parameters of the multi-dimensional Resource Envelope are defined in the primary model matrix:

| Envelope Dimension | Enforced Metric Parameter | Algorithmic & SRE Containment |
| :---- | :---- | :---- |
| **Token Limits** | total, input, output, cached, and embedding counts.2 | preflight tokenization check; hard ceiling limits on output generation.2 |
| **Model Call Limits** | aggregate model invocations per session or workflow.5 | session call counters; hard limits terminating after M steps.10 |
| **Tool Call Limits** | total third-party API invocations.5 | tool gateway tracking; dynamic token validation against schemas.7 |
| **Retrieval Limits** | vector queries, document expansions, page renders.8 | query caps; candidate limit constraints; cluster-level bounds.31 |
| **Turn Limits** | dialogue steps, retries, and repair iterations.5 | loop-step limits; max retry thresholds capped at <= 2 attempts.5 |
| **Time Limits** | wall-clock, processing, and queue wait times.5 | asynchronous cancellation; worker thread isolation limits.8 |
| **Latency Limits** | model prefill, model decode, and tool latencies.13 | real-time anomaly detection; load-shedding triggers.13 |
| **Batch / Concurrency Limits** | parallel sessions, batch jobs, and GPU slices.5 | multi-tenant queues; weighted fair-share slot scheduling.22 |
| **Financial Limits** | dollar velocity per user, key, and tenant space.9 | token-aware pricing lookup; optimistic pre-consumption budgets.2 |
| **Human Review Limits** | queue depth and manual review time budgets.5 | dynamic validation rules; escalation throttling caps.5 |

| Cost Bomb Variant | Entry Point | Amplification Path | Metric Signature | Primary Containment | Safer Fallback | Regression Test |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Token Bomb** | Chat inputs, prompt templates, generated continuations. | Request maximizes output length or triggers repetitive generation. | Output tokens hit configured maximum; low information density. | Output cap, stop conditions, repetition detection. | Terminate stream with partial/managed response. | Repetitive-generation and max-token tests. |
| **Context Bomb** | Uploads, logs, meeting traces, pasted documents. | Large or redundant text causes expensive prefill and attention dilution. | Prefill latency spikes; context utilization exceeds budget. | Input size caps, context admission control, evidence prioritization. | Summarize or sample inside bounded processing route. | Long-context saturation tests. |
| **Retrieval Bomb** | Search fields, query expansion, agentic RAG loops. | Broad query expansion causes excessive vector search, reranking, and page expansion. | Query count, candidate count, rerank latency, and index fan-out spike. | Query/candidate/rerank/page-render caps. | Return narrowed-search prompt or managed retrieval-limit status. | Retrieval fan-out stress tests. |
| **Tool Bomb** | APIs, DB tools, SaaS connectors, browser tools. | Retries, loops, or broad tool calls exhaust quotas and connection pools. | Tool retry rate, quota burn, connection errors, repeated payload hashes. | Tool ledger, retry caps, idempotency, circuit breakers. | Return typed managed failure without invoking the tool. | Tool loop and duplicate-call simulations. |
| **Vision Bomb** | PDFs, images, video, screenshots. | Multi-page rendering, OCR, VLM calls, or high-resolution processing expands cost. | Parser CPU, VLM calls, page renders, megapixels, frame count spike. | Page/frame/resolution limits; selective routing. | Extract sampled pages/frames or request narrower evidence scope. | Visual parsing queue stress tests. |
| **Browser Bomb** | Web crawling, dynamic pages, UI agents. | Wait loops, redirects, scripts, and dynamic rendering hold browser workers. | Browser process count, wait time, CPU, navigation retries spike. | Browser session caps, navigation timeout, domain allowlist. | Close session and return bounded page/error summary. | Dynamic page wait-loop tests. |
| **Voice Bomb** | Realtime audio, telephony, meetings. | Noise or background speech triggers continuous turns or transcription loops. | Turn count, STT duration, endpoint resets, audio buffer growth. | VAD/endpointing limits, audio duration caps, turn budget. | Pause audio capture and request typed/explicit input. | Background-noise loop simulations. |
| **Batch Bomb** | Reindexing, embedding backfills, eval sweeps, log parsers. | Background jobs expand beyond estimates or repeat failed records. | Queue depth, records processed per dollar, failure retries, memory usage spike. | Dry-run, sample-first estimate, manifest caps, checkpointing. | Pause job and require operator approval to resume. | Batch expansion and retry-limit tests. |
| **Cache-Bypass Bomb** | Adversarial prefixes, randomized prompts, semantic-cache collision attacks. | Requests defeat cache reuse or force recomputation. | Cache hit rate drops; prefill rate rises; TTFT worsens. | Scoped cache keys, prefix isolation, admission throttles. | Force recompute within budget or return capacity status. | Cache-bypass and collision tests. |
| **Review Bomb** | Low-confidence OCR, validation failures, safety escalations. | Excessive borderline cases flood human review queues. | Review backlog, review minutes, escalation rate spike. | Escalation caps, sampling, confidence thresholds, triage classes. | Degrade to bounded review sampling or managed capacity status. | Low-confidence escalation tests. |

## **Recursive Agents and Loop Amplification**

Agentic systems multiply resource consumption because each execution step can append context, invoke tools, query vector indexes, call models, and trigger recursive repair or self-reflection routines.5 In a naive ReAct or planning loop, the input token volume scales quadratically as the history of prior execution steps, formatting exceptions, and tool observations accumulates.5  
The total input tokens consumed across an N-turn agentic loop is modeled by the following quadratic context-accumulation equation:  
T_total = N * S + (u * N * (N + 1)) / 2 + (r * N * (N - 1)) / 2 5  
In this model, S represents the size (in tokens) of the system prompt and tool schema definitions, u is the average size of new incoming tokens per iteration (user inputs, formatting exceptions, or tool outputs), and r is the model's generated output per step.5  
When an agent enters an unmitigated loop, this quadratic context growth can rapidly deplete daily budgets and trigger denial-of-wallet incidents.5 Consequently, every iterative agent path must carry an explicit loop budget managed strictly outside the model by the orchestration engine.9  
The orchestration layer must enforce a multi-dimensional constraint model on active loops. The numeric ceilings should be policy-profile defaults, not universal constants. A low-risk search assistant, a high-impact financial workflow, and a batch repair job should not share the same loop envelope.

| Constraint | What It Controls | Enforcement Guidance |
| :--- | :--- | :--- |
| **Step Cap** | Total reasoning/tool/action iterations. | Set by workflow risk class; hard stop on repeated no-progress states. |
| **Wall-Clock Cap** | Total elapsed execution time. | Cancel or degrade when the workflow exceeds profile budget. |
| **Token / Spend Cap** | Cumulative model, embedding, rerank, and tool cost. | Enforced by gateway reservation and runtime ledger. |
| **Tool / Action Cap** | Number and class of external calls. | Separate read-only, low-risk mutation, and high-risk mutation limits. |
| **Retry Cap** | Repeated attempts after failure. | Small default retry budget; mutation retries require idempotency/reconciliation. |
| **Progress Audit** | Evidence of convergence. | Additional budget is granted only when state changes materially. |
| **Repeated-State Detection** | Cyclic plans, tool calls, payloads, UI clicks, or search queries. | Halt, replan, ask user, or escalate after bounded attempts. |
| **Escalation Path** | Human/operator transfer. | Triggered by high-risk state, repeated failure, unknown action state, or budget exhaustion. |

Quadratic context growth depends on trace-retention policy. If every prior plan, tool result, error, and repair attempt is appended to the next step, loop cost grows quickly. If traces are summarized, pruned, or externalized into state, the growth can be controlled. The resource doctrine is therefore not merely “cap loops,” but “cap loops and control what history they carry forward.”

### **Progress versus No-Progress States**

To prevent early termination of long-running, legitimate workflows, the loop manager must differentiate between active progress and repetitive, non-productive execution patterns.18

* **Progress Signals**: Indicated by measurable transitions that bring the loop closer to completion.17 These include acquiring unique, non-overlapping document IDs from vector indexes; reducing schema validation errors across repair turns; completing subtasks in the active plan; executing successful, non-duplicate API calls; or confirming parameter fields via user feedback.5  
* **No-Progress Signatures**: Indicated by repetitive or cyclical patterns that fail to transition the system's state.5 These include generating identical planner strings across steps; submitting duplicate tool payloads; repeating the same database query with identical results; encountering repeating validation errors; or executing identical mouse coordinates without UI layout changes.5

The progress detection framework maps these signals across active workflow domains:

| Workflow Domain | Verifiable Progress Signal | No-Progress Signatures | Recovery Action |
| :---- | :---- | :---- | :---- |
| **Planning** | subtask completion; updated sub-plans; narrowed search space.18 | repeated generation of identical plan strings across steps.5 | suspend planning; prompt user for clarification.5 |
| **Retrieval** | acquisition of unique, non-overlapping document IDs.8 | repeating identical search queries with identical results.5 | throttle search; fall back to cached results.5 |
| **Tools** | successful API response matching target schemas.5 | repeating identical tool payloads with repeating errors.5 | isolate tool; restrict retry budget to <= 2 attempts.5 |
| **Repair** | decrease in schema validation errors across turns.5 | cyclical code generation repeating identical errors.5 | halt execution; revert to last clean state.5 |
| **UI Control** | verified DOM mutation; page URL redirect.8 | clicking identical coordinates without layout changes.5 | pause run; escalate session to human operator.5 |
| **Voice** | user input confirming captured parameter fields. | repeated conversational clarification prompts.5 | switch session to a visual keypad input card. |
| **Batch** | incremental checkpointing of completed records.5 | zero records processed across multiple thread cycles.5 | pause job; trigger alerting; revoke run token.5 |

## **Context Flooding Defense Model**

Context flooding exploits the large inputs supported by modern models, using them to overwhelm attention mechanisms, bypass safety boundaries, and inflate processing costs.5 When a model's context window is filled with verbose document text, system logs, or conversational history, the salience of system instructions is diluted, resulting in safety rule decay.5 Attacks like ContextFlooding use this behavior, overloading the context with realistic, non-malicious text before appending harmful prompts.11  
To defend against context inflation, the system must enforce strict boundaries at the ingestion and serialization stages:

* **Input-Size Caps**: Strict physical limitations on user text submissions and upload payloads, enforced before tokenization occurs.1  
* **Session Context Budgets**: Hard limits on rolling context sizes in multi-turn sessions, triggering truncation or semantic compression when thresholds are breached.5  
* **Tool-Output Truncation**: Mandatory truncation and summarization rules for API responses before appending them to the active context window.7  
* **Retrieval-Token Budgets**: Capping the total retrieved document chunks inserted per turn to prevent context bloating.6  
* **Memory-Inclusion Budgets**: Restricting the number of historical conversation memories loaded into active reasoning contexts.6  
* **Conversation Compaction**: Condensing older turns into structured summaries when context utilization approaches the task profile’s budget threshold, while preserving active instructions, approvals, constraints, and unresolved state.
* **Document-Size Limits**: Limits on raw page counts, layout nodes, or tabular elements processed in a single run.5  
* **Modality-Specific Limits**: Restrict images by pixel dimensions, megapixels, page count, and file size; restrict video by duration, frame count, resolution, and query-relevant keyframes; restrict audio by duration, channel count, sample rate, and turn budget.
* **Context Admission Control**: Prioritized scheduling that drops low-priority or un-cacheable text blocks during periods of queue congestion.15  
* **Lossy vs Lossless Compression**: Stripping redundant formatting, comments, and empty tokens (lossless) or executing semantic summarization (lossy) based on context priorities.6  
* **Fail-Closed Behavior**: Automatically terminating the execution path and returning a managed system exception if required evidence cannot safely fit within context limits.5

## **Retrieval Flooding and RAG Cost Control**

Query expansion techniques are designed to improve recall by rewriting vague user inputs into multiple richer, searchable queries.12 In production RAG systems, however, this expansion can trigger severe retrieval flooding.12 A single user query rewritten into several variations forces the system to execute parallel vector searches, merge redundant candidate sets, and run expensive cross-encoder rerankers over massive chunk lists, leading to latency spikes and high compute costs.12 This expansion can also introduce subtle failure modes that degrade answer quality:

* **Intent Broadening**: Precise procedural queries are rewritten into broad, conceptual terms (e.g., expanding *“How do I rotate AWS access keys safely?”* into *“secret management”*), causing the system to retrieve generic documentation instead of exact runbooks.12  
* **Synonym Inflation**: Substituting terms that are legally or operationally distinct (e.g., treating *“term sheet”* as *“contract”*), violating the precise taxonomy of technical domains.12  
* **Metadata Filter Dilution**: Rewriting the semantic core of a query but dropping its structural constraints, causing retrieval to pull chunks from incorrect regions, versions, or tenants.12  
* **Reranker Overload**: Handing too many low-signal, expanded candidates to the reranker, introducing semantic noise that dilutes the presence of precise chunks.12  
* **Pseudo-Relevance Drift**: Using early retrieval errors as feedback to guide subsequent search steps, driving the search further from the user's intent.12  
* **Logical Flattening**: Flattening multi-hop questions into a single semantic query, erasing the logical and temporal relationships between sub-questions.12  
* **Lexical Anchor Removal**: Paraphrasing away unique technical identifiers (build names, error codes), making it impossible to locate the correct document.12  
* **Redundancy Congestion**: Allowing near-duplicate candidates from varied queries to dominate the merged set, burying diverse, relevant documents.12  
* **Evaluation Distortion**: Tracking candidate presence in the top-k while ignoring position, hiding the fact that the single best chunk has been pushed below the reranker's window of attention.12  
* **Accidental Policy Agency**: The expansion model implicitly decides what the user "really meant," making the system's behavior non-deterministic and hard to audit.12

### **Gated Retrieval and Retrieval-Budget Control**

Retrieval expansion should not run unconditionally. A vague or difficult query may need rewriting, decomposition, or evidence focusing; a simple query may need no retrieval at all; an unsafe or overbroad query may need clarification instead of fan-out.

Hidden-state probing methods such as Skill-RAG are one advanced option when the serving stack exposes model-internal representations. Many hosted-model deployments do not expose hidden states. In those environments, the same gating pattern can be approximated with retrieval confidence models, answerability classifiers, lightweight verifier models, query-specific uncertainty checks, or explicit evidence-sufficiency tests.

```text
GATED RETRIEVAL PIPELINE

[ User Query ]
      |
      v
[ Request Classifier ]
  task type | risk class | tenant scope | evidence requirement
      |
      v
[ Answerability / Retrieval Gate ]
      |
      +--> sufficient current context
      |       answer from available evidence or parametric route
      |
      +--> evidence required
      |       proceed to bounded retrieval
      |
      +--> query too broad / unsafe / under-specified
              ask clarification or return managed limit status

[ Bounded Retrieval ]
  query cap | candidate cap | partition cap | page-render cap | rerank cap
      |
      v
[ Evidence Evaluation ]
  relevance | authority | freshness | conflict | sufficiency
      |
      +--> sufficient evidence
      |       generate grounded answer
      |
      +--> insufficient but budget remains
      |       choose one corrective skill
      |
      +--> insufficient and budget exhausted
              stop retrieval and report limitation

Corrective Skills:
  - Query Rewriting: preserve lexical anchors and metadata constraints.
  - Question Decomposition: split true multi-hop questions.
  - Evidence Focusing: narrow candidate regions/pages/tables.
  - Exit: stop retrieval before it becomes a ritual sacrifice to the vector gods.
```

| Retrieval Control | Default Function |
| :--- | :--- |
| **Query Cap** | Limits rewritten/decomposed searches per user request. |
| **Candidate Cap** | Limits chunks/documents passed forward per query. |
| **Rerank Cap** | Limits expensive cross-encoder or LLM reranking calls. |
| **Document Expansion Cap** | Limits neighboring pages, sections, or table continuations. |
| **Page Rendering Cap** | Limits visual/OCR render work. |
| **Partition Cap** | Limits index/database partitions searched per request. |
| **Evidence Sufficiency Gate** | Stops when available evidence cannot support the requested answer. |

Permission-aware retrieval and retrieval budgets are separate controls. Permission-aware retrieval decides **what the user is allowed to see**. Retrieval budgeting decides **how much search work this request is allowed to trigger**.

## **Tool Consumption Ledger**

Tools bridge the gap between generative reasoning and system operations, transforming model outputs into external actions.7 Uncontrolled tool execution can exhaust third-party API limits, trigger rate-limit cascades, saturate database connection pools, and deplete internal quotas.5 To manage this, the gateway must record every tool invocation in a centralized *Tool Consumption Ledger*.7  
The structure of the Tool Consumption Ledger is defined in the primary log schema:

| Parameter Field | Schema Data Type | System Governance Rule |
| :---- | :---- | :---- |
| `transaction_id` | UUID | Unique identifier for every tool invocation attempt. |
| `tenant_id` | UUID | Binds execution to active tenant. |
| `user_id` | UUID | Binds execution to authenticated user. |
| `session_id` | UUID | Binds execution to active session/workflow. |
| `workflow_id` | UUID / nullable | Groups tool calls within an agentic or batch workflow. |
| `tool_name` | String | Must match registered tool contract. |
| `tool_version` | String | Records manifest/schema version used at execution. |
| `action_class` | Enum | `READ_ONLY`, `LOW_RISK_MUTATION`, `HIGH_RISK_MUTATION`, `EXTERNAL_COMMUNICATION`, `ADMINISTRATIVE`. |
| `cost_class` | Enum | `FREE`, `INTERNAL_FIXED`, `METERED_API`, `TOKEN_METERED`, `HUMAN_REVIEW`, `UNKNOWN`. |
| `idempotency_key` | String / nullable | Required for mutations; optional request hash for reads. |
| `payload_hash` | String | Detects duplicate or repeating calls without logging raw secrets. |
| `attempt_count` | Integer | Bounded by retry policy for tool and action class. |
| `retry_reason` | String / nullable | Records typed reason for retry. |
| `timeout_ms` | Integer | Maximum allowed runtime for this call. |
| `latency_ms` | Integer | Observed execution time. |
| `quota_consumed` | Decimal / JSON | Provider-specific quota units consumed. |
| `estimated_cost_usd` | Decimal | Pre-execution estimate. |
| `actual_cost_usd` | Decimal / nullable | Reconciled cost after completion. |
| `cumulative_workflow_spend_usd` | Decimal | Running workflow/session spend total. |
| `result_status` | Enum | `SUCCESS`, `FAILED`, `BLOCKED`, `TIMEOUT`, `UNKNOWN`, `PARTIAL`, `PENDING`. |
| `error_class` | String / nullable | Normalized exception/failure class. |
| `verification_status` | Enum | `NOT_REQUIRED`, `VERIFIED`, `FAILED`, `UNKNOWN`, `PENDING`. |
| `created_at` | Timestamp | Invocation attempt time. |
| `completed_at` | Timestamp / nullable | Completion or timeout time. |

Every tool invocation must be checked against the user’s session role, tenant scope, tool policy, and active budget before execution. Mutating calls require idempotency or an equivalent duplicate-prevention mechanism. High-risk mutations require post-action verification before the agent can claim completion. Unknown tool state must be preserved as unknown; it must not be flattened into success or failure for conversational convenience.

## **Adversarial Latency Inflation Threat Model**

Adversarial latency inflation exploits parsing, retrieval, serving-engine scheduling, tool execution, or queueing behavior to degrade availability. In some workflows, incorrect answers also trigger retries or escalation, making Time-to-Correct-Answer useful; in others, the incident is simpler worker starvation or queue saturation.14 The actual performance of a long-context system is therefore modeled using the *Time-to-Correct-Answer (TTCA)* metric, which measures the total wall-clock time required to obtain the first correct, verified response, accounting for retries and escalations.14  
Attackers can deliberately inflate latency by submitting inputs designed to maximize processing times. High-resolution images, multi-page scanned PDFs, uncacheable prefixes, and complex math-heavy prompts force serving engines to spend excessive cycles in the compute-bound prefill phase.6 Since modern engines process requests in batches, a small number of latency-inflated requests can occupy active worker threads, saturate GPU caches, and cause queue starvation for all other concurrent users.13  
To protect serving availability, the platform must enforce stage-wise latency budgets and isolate workloads.

```
STAGE-WISE LATENCY BUDGET MODEL

[ Request Intake ]
   validate size, auth, tenant, policy, and declared modality
        |
        v
[ Parsing / Modality Processing ]
   OCR, document layout, image/video/audio preprocessing
        |
        v
[ Retrieval ]
   authorized search, candidate limits, partition limits
        |
        v
[ Reranking / Evidence Selection ]
   bounded rerank calls and evidence sufficiency checks
        |
        v
[ Model Prefill ]
   prompt/context processing; chunked prefill where supported
        |
        v
[ Model Decode ]
   output generation under token/time limits
        |
        v
[ Tool Execution ]
   bounded API/browser/database calls when required
        |
        v
[ Output / Post-Processing ]
   validation, redaction, formatting, TTS or UI delivery if applicable
        |
        v
[ User-visible response ]

Latency budgets should be profile-specific:
  realtime voice/chat       -> tight first-response and streaming budgets
  document analysis         -> larger parser/OCR budget
  batch processing          -> throughput and total-job budget
  high-impact tool action   -> verification budget matters more than speed
```

* **Input Validation (10ms)**: Quickly filters out-of-bounds or malformed requests.5  
* **Parser/OCR (120ms)**: Executes layout extraction in CPU-bound, isolated containers.7  
* **Retrieval (45ms)**: Queries vector indexes with strict candidate-set limits.6  
* **Reranking (80ms)**: Scores chunks, capping the candidate list size.12  
* **Model Prefill (150ms)**: Computes attention metrics over chunks, using chunked prefill.26  
* **Model Decode (250ms)**: Generates output tokens, prioritizing decode iterations.23  
* **Tool Execution (180ms)**: Invokes APIs, bounded by hard timeout ceilings.8  
* **Downlink TTS (90ms)**: Streams synthesized audio chunks progressively.

## **Runaway Batch Jobs**

Batch operations—such as database reindexing, vector embedding generation, and model evaluation sweeps—represent massive cost risks because they execute at scale over extensive datasets.5 A minor configuration drift or a malformed document parser can turn a routine background job into a severe cost event.5  
To contain these risks, all background batch jobs must be governed by the *Batch Job Containment Model*:

* **Estimation & Dry-Runs**: Mandating upfront cost, token, and database write calculations before scheduling any bulk jobs.2 Jobs must be run over a 1% sample subset to verify that actual costs align with estimates before launching full execution.5  
* **Scope & Limit Constraints**: Every batch manifest must carry hard ceilings on the total number of records processed, documents parsed, embeddings generated, and cumulative tokens consumed.5  
* **Checkpointing**: Requiring incremental state saving at fixed intervals, allowing jobs to pause, resume, or roll back cleanly without duplicate token consumption if a process crashes.5  
* **Kill Switches**: Providing administrative endpoints and automated triggers inside the gateway to terminate batch processes instantly if cost limits or error thresholds are breached.5  
* **Approval Gates**: Restricting high-cost batch execution to explicit multi-party clearance from compliance and FinOps operators.7

## **Tenant Resource Isolation and Quota Fairness**

In multi-tenant SaaS deployments, ensuring data isolation is only half the battle; resource and quota isolation are equally critical.6 Without robust resource isolation, a single tenant running bursty, long-context workloads can monopolize shared GPU memory and serve as a "noisy neighbor," causing latency spikes, preemption storms, and out-of-memory (OOM) crashes for all other tenants on the host.23  
Physical GPU partitioning using mechanisms such as Multi-Instance GPU (MIG) can provide stronger hardware-level isolation, but it does not eliminate every shared bottleneck, and it may reduce fleet flexibility or utilization during low-traffic periods.23 To balance efficiency with reliability, platforms must deploy intelligent overload and admission control at the gateway layer.15 The system should maintain separate admission lanes or queues for distinct operation classes—such as interactive reads, writes, long-running scans, batch jobs, and realtime sessions—to prevent background work from blocking latency-sensitive traffic. CoDel-style queue management is one possible technique, not a universal requirement.15 Concurrency must be managed dynamically based on Little's Law:  
Concurrency = Throughput * Latency 15  
To distribute remaining capacity fairly, the platform implements a rule-based admission control engine that tracks and enforces multi-level resource quotas per tenant space.15  
These controls span several critical mechanisms:

* **Fairness Scheduling**: Enforcing per-tenant queues and fair-share scheduling to prevent a single customer from monopolizing shared GPU memory caches.22  
* **Priority Tiers**: Isolating real-time, user-facing traffic from background batch workloads, routing them to separate execution lanes.15  
* **Burst Limits & Soft/Hard Caps**: Allowing tenants to briefly burst above their steady-state limits while enforcing strict hard caps on daily and weekly consumption.22  
* **Reserved Capacity**: Allocating dedicated GPU slices or container instances for premium enterprise agreements to guarantee latency SLOs.22  
* **Noisy-Neighbor Detection**: Monitoring tenant-specific latency percentiles and cost velocity, triggering automated throttling if a tenant impacts shared queues.15  
* **Grace Windows**: Providing temporary overrides or warning banners before hard rate-limit blocks are applied to active sessions.22  
* **Customer-Visible Status**: Exposing remaining quota and reset times in the response headers, enabling developers to self-correct.22

## **The Budget-Aware Runtime Gateway Architecture**

The primary defense against resource abuse is the *Budget-Aware Runtime Gateway*, positioned entirely outside the model's execution path and agent orchestrator.9 This architecture acts as an application-layer firewall, checking preflight estimates, reserving compute budgets, and terminating runaway sessions before they reach model providers.2

### **The Two-Phase Reservation and Reconcile Pattern**

Because the exact number of output tokens cannot be predicted prior to generation, the gateway must execute a financial-style reservation and reconciliation pattern.2

```
                       RESERVATION & REFUND FLOW  
       │  
       ▼  
┌────────────────────────────────────────────────────────┐  
│ Phase 1: Upfront Reservation                           │  
│ 1. Tokenize prompt: L_in = prompt_tokens               │  
│ 2. Read request parameter: L_out_max = max_tokens      │  
│ 3. Compute Reserved Cost = L_in*Pin + L_out_max*Pout   │  
│ 4. Deduct Reserved Cost atomically from Redis bucket   │  
└────────────────────────┬───────────────────────────────┘  
                         │  
                 (Passes Quota Check)  
                         │  
                         ▼  
            [ LLM Generation Execution ]  
                         │  
               (Generation Completes)  
                         │  
                         ▼  
┌────────────────────────────────────────────────────────┐  
│ Phase 2: Reconciliation & Refund                       │  
│ 1. Read actual usage: L_out_actual = completion_tokens │  
│ 2. Compute Actual Cost = L_in*Pin + L_out_actual*Pout  │  
│ 3. Compute Refund = Reserved Cost - Actual Cost        │  
│ 4. Credit Refund value back to the user's Redis bucket │  
└────────────────────────────────────────────────────────┘
```

1. **Phase 1: The Reservation (Upfront Estimation)**  
   When a request arrives, the gateway tokenizes the incoming prompt to determine the input token count (L_in).2 It reads the max_tokens parameter (L_out_max) from the request and computes the worst-case estimated cost 2:  
   Reserved Cost = L_in * P_input + L_out_max * P_output 2  
   The gateway checks the user's token bucket in Redis. If the bucket has insufficient tokens, the request is immediately blocked, returning an HTTP 429 Too Many Tokens error.2 If sufficient, the gateway atomically decrements the estimated cost from the user's quota.43  
2. **Phase 2: The Refund (Reconciliation)**  
   Once response generation completes, the gateway reads the actual completed token count (L_out_actual) from the provider's response metadata.2 It calculates the actual cost and refunds the unused tokens back to the user's bucket 2:  
   Actual Cost = L_in * P_input + L_out_actual * P_output 43  
   Refund = Reserved Cost - Actual Cost 2  
   For streaming connections, the gateway counts tokens progressively as individual chunks arrive over the WebSocket.2 It captures the final chunk's usage.completion_tokens metadata to compute and apply the remaining refund before the HTTP socket is closed.2

The reservation pattern must also define failure handling:

| Failure Case | Risk | Required Handling |
| :--- | :--- | :--- |
| **Provider timeout before usage metadata** | Actual cost may be unknown. | Mark reservation as pending; reconcile from provider logs or expire with conservative policy. |
| **Streaming disconnect** | User connection drops while provider may continue generating. | Cancel upstream request where possible; reconcile partial usage. |
| **Gateway crash after reservation** | Reserved budget may leak. | Store reservation record durably with expiry and recovery worker. |
| **Provider returns incomplete usage** | Actual cost cannot be trusted. | Use conservative estimate and flag for billing reconciliation. |
| **Reconciliation write fails** | Refund or debit may not be recorded. | Retry idempotently; preserve transaction as unresolved. |
| **Duplicate request retry** | Same logical request may reserve twice. | Use idempotency key or request hash for reservation records. |
| **Budget breached mid-stream** | Generation exceeds allowed spend. | Terminate stream at budget boundary and return managed budget status. |

Reservation is not merely a math trick. It is a financial transaction boundary and needs durable state, idempotency, expiry, and reconciliation.

### **Three-Layer Rate Limiting Architecture**

The gateway enforces rate limiting across three complementary layers to defend against both volume and pattern-based resource abuse.9

* **Layer 1: Token Bucket Per Identity**  
  Every identity tuple, represented as (user, repo, model), is assigned its own token bucket in Redis to isolate rogue processes without blocking unrelated workloads.9 It operates using a continuous refill algorithm: the bucket stores a last_refill_timestamp in Redis, and on every request, the system calculates the time elapsed and adds tokens proportionally based on the configured refill rate.42  
* **Layer 2: Pattern-Based Circuit Breakers**  
  Circuit breakers monitor traffic signatures in real time and trip (failing fast for 60 seconds) when they detect anomalous behaviors.9 Breakers trip when an agent ignores standard Retry-After headers and generates consecutive 429s, when error rates exceed 50% in a 60-second window, when cost velocity exceeds the planned budget rate, or when call shapes indicate runaway loops (e.g., identical prompts or monotonically growing context sizes).9  
* **Layer 3: Policy-Preserving Fallback Chains**  
  When a primary route is throttled or its circuit breaker is open, the gateway may route traffic down a declarative fallback chain. Fallback must preserve tenant scope, data classification, tool permissions, quality threshold, and task fitness. A cheaper model is acceptable only when it can safely perform the task. A semantic cache is acceptable only when the cache key, source freshness, tenant scope, and policy version match. If no safe fallback exists, the system returns a managed 429/503/capacity response rather than silently downgrading into wrongness.9

### **Token-Budget-Aware Pool Routing**

To optimize serving instance utilization, the gateway implements a fleet-level token-budget-aware routing algorithm. Standard vLLM fleets configure every serving instance for the maximum context window (C_max), wasting substantial GPU memory capacity on short requests. The gateway partitions a homogeneous fleet into two specialized pools: a high-throughput Short Pool (P_s) right-sized for short contexts, and a High-Capacity Long Pool (P_l) configured for the maximum context.  
At dispatch time, the gateway estimates the total token budget (L_total) for request r of content category k 25:  
L_total = ceil(|r| / c_k*) + r.max_output_tokens 25  
where |r| represents the byte length of request r, and c_k* is the conservative bytes-to-token ratio calculated as:  
c_k* = c_k_hat - gamma * sigma_k_hat 25  
The variables c_k_hat and sigma_k_hat (positive real numbers) represent the per-category exponential moving average (EMA) of the bytes-to-token ratio and its standard deviation, learned online from incoming usage.prompt_tokens feedback 25:  
c_obs = |r| / usage.prompt_tokens 25  
c_k_hat = beta * c_k_hat + (1 - beta) * c_obs 25  
sigma_k_hat = EMA(sigma_k_hat, |c_obs - c_k_hat|) 25  
The constant gamma (positive real number) represents an asymmetric, error-aware safety parameter ensuring conservative budget estimation.25 If L_total > C_max(P_s), the gateway routes the request directly to the long pool P_l; otherwise, it dispatches the request to the high-concurrency short pool P_s, optimizing GPU memory utilization without the latency overhead of running model-specific tokenizers.25  
This analytical split is governed by a closed-form fleet-level cost savings model:  
Savings = alpha * (1 - 1/rho) 25  
where alpha (between 0 and 1) is the short-traffic fraction of the workload, and rho (greater than 1) is the throughput gain ratio achieved by running the short pool under a tighter memory configuration.25

## **Cost Attribution Architecture**

The platform cannot defend its resource boundaries if it cannot attribute consumption. The gateway must record and trace every transaction, associating all token, compute, and integration expenses back to specific system entities.5  
Required parameters captured in the cost attribution schema:

* **Identifiers**: request_id, tenant_id, user_id, session_id, workflow_id, model_id.  
* **Token Metrics**: prompt_tokens, completion_tokens, cached_tokens, embedding_tokens.  
* **Pipelining & Retrieval**: retrieved_candidates_count, rerank_calls, cache_hit_status.  
* **Modality & Parse**: ocr_pages, video_frames_sampled, browser_actions_count.  
* **Tools & Escalations**: tool_calls_count, retries_count, human_review_minutes.  
* **Infrastructure Metrics**: wall-clock_time_ms, queue_wait_time_ms, gpu_execution_time_ms.  
* **Financials**: external_api_spend_usd, internal_resource_cost_usd.  
* **Termination Status**: termination_reason (e.g., success, max_turns_exceeded, budget_breach).

This data feeds downstream telemetry systems to automate pricing, billing, and anomaly detection.5

## **Observability, Alerting, and Incident Response**

Maintaining reliability requires continuous monitoring of resource metrics across all serving pipelines.4 Security and platform teams must establish real-time alerting to identify anomalies before they impact service availability or billing budgets.4  
The platform tracks and evaluates these key telemetry metrics:

* **Token Burn Rate**: Prompt and completion tokens processed per second.2  
* **Cost Velocity**: Cumulative financial spend over rolling windows.9  
* **Quota Burn Rate**: Consumption of upstream provider rate limits.5  
* **Loop Step Count**: Execution steps per active session.5  
* **Tool Retry Rate**: Frequency of repeated tool invocation tries.5  
* **No-Progress Count**: Consecutive steps with unchanged states.5  
* **Retrieval Fan-out**: Chunks retrieved per user query.12  
* **Queue Depth**: Requests waiting in scheduling queues.5  
* **TTFT Tail Latency**: Time to first token for user batches.13  
* **Cache Miss Rate**: Frequency of prefix cache misses.6  
* **Parser Exception Rate**: Document parsing failures and hangs.5  
* **Review Backlog Depth**: Pending cases in human review queues.5

When an alert triggers a resource incident, the platform executes a structured, multi-tier incident response playbook to contain the impact and restore normal operating parameters:

```text
RESOURCE INCIDENT RESPONSE FLOW

[ Alert / Anomaly Trigger ]
  cost velocity | quota burn | loop count | queue depth | latency | tool retries
        |
        v
[ Scope the Incident ]
  tenant | user | session | workflow | model route | tool | batch job | queue
        |
        v
[ Containment ]
  suspend affected sessions
  revoke active tokens if abuse/compromise is suspected
  terminate runaway jobs or loops
  trip route/tool circuit breakers
        |
        v
[ Isolation ]
  pause affected queue or tenant partition
  isolate noisy workload class
  block expensive tool/model route if needed
        |
        v
[ Recovery ]
  reconcile spend and quota
  restore safe capacity
  credit affected customers when appropriate
  resume traffic gradually
        |
        v
[ Hardening ]
  update budgets, limits, classifiers, tests, and runbooks
  add regression case for the triggering pattern
```


* **Containment**: The gateway identifies a budget breach or loop anomaly and suspends the affected sessions or revokes active tokens when abuse or compromise is suspected.5 Cost circuit breakers trip globally on the compromised model routes, blocking further API calls.9 Runaway background batch jobs and agent workflows are immediately terminated by the orchestrator.5  
* **Isolation**: The platform pauses processing on compromised task queues to prevent cascading failures.5 Exposed tool credentials and API tokens are revoked and rotated.6 Model routing may be shifted to approved fallback routes only when those routes preserve task fitness, policy, tenant scope, and quality requirements.5  
* **Traceback & Recovery**: SRE teams analyze execution traces and log hashes to calculate the precise financial impact and identify the exploit pathway.5 Vector database indexes are audited to locate and isolate poisoned documents.6 Platform quotas are restored, and affected customer accounts are credited.5  
* **Hardening & Post-Mortem**: A root-cause analysis is published to catalog the failure mechanism.7 Security teams construct a regression test reproducing the exploit pattern, integrating it into the CI/CD pipeline to prevent future regressions.5

## **Cross-Canon Handoff Map**

The resource-control architecture established in this report interfaces with context, retrieval, serving, orchestration, tool, action-verification, multimodal, voice, UI, security, fallback, telemetry, audit, and governance systems. Resource limits are not merely downstream operational concerns; they shape safe execution across the canon.

| Target Report ID | Target Domain | Resource-Control Handoff | Integration Rule |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context Tenure & State Governance | Context retention budgets, memory inclusion limits, compaction thresholds. | Long sessions must preserve active state while bounding carried-forward context. |
| **AI-ENG-C** | Cost, Latency & Margin Mechanics | Token pricing, spend attribution, latency/cost models. | Gateway budgets must align with cost and margin models. |
| **AI-ENG-E** | Retrieval Pipeline | Query caps, candidate caps, rerank caps, page-render caps. | Retrieval must be authorized and budgeted before context assembly. |
| **AI-ENG-J** | Throughput Mechanics | KV cache pressure, prefill/decode scheduling, queue starvation. | Serving-level resource limits must reflect memory and scheduling behavior. |
| **AI-ENG-L** | Serving Architecture | Gateway routing, rate limits, circuit breakers, pool selection. | Serving routes must enforce budget, latency, and quota envelopes. |
| **AI-ENG-M** | Agentic Orchestration | Loop budgets, progress detection, no-progress states. | Agents must halt, replan, or escalate when loop/resource budgets are exhausted. |
| **AI-ENG-N** | Tool Contracts | Tool quota, retry budgets, idempotency, tool ledger. | Tool calls require budget checks and action-class-specific retry limits. |
| **AI-ENG-O** | Action Verification | Unknown state, partial commit, post-action verification cost. | Verification work must be budgeted, but completion claims cannot bypass verification. |
| **AI-ENG-P** | Multimodal Understanding | OCR pages, image megapixels, video frames, parser/VLM budgets. | Multimodal extraction must be bounded by modality-specific resource envelopes. |
| **AI-ENG-Q** | Speech and Realtime Interaction | Audio duration, turn count, STT/TTS budget, realtime latency. | Voice sessions need streaming resource budgets and interruption-safe limits. |
| **AI-ENG-R** | UI Agents | Browser sessions, waits, clicks, page renders, downloads. | UI automation must enforce browser/session/time/action budgets. |
| **AI-ENG-S** | Production Pathologies | Runaway loops, malformed repair loops, brittle chains. | Resource incidents should be typed, observable, and replayable. |
| **AI-ENG-T** | Boundary Defense | Tenant isolation, cache scope, authorization before retrieval. | Resource fallback must not bypass tenant, policy, or permission boundaries. |
| **AI-ENG-U** | Supply Chain Security | Parser/tool/dependency sandbox budgets and egress limits. | Compromised dependencies must be contained by resource and network boundaries. |
| **AI-ENG-W** | Fallback and Degraded Modes | Capacity-aware fallback and graceful degradation. | Fallback routes must preserve policy and task fitness, not just reduce cost. |
| **AI-ENG-X** | User Trust & Transparency | Budget status, degraded-mode explanations, quota visibility. | Users should receive honest status when limits block or degrade execution. |
| **AI-ENG-Y** | Human Review | Review queue budgets and escalation throttles. | Human review is a scarce resource and requires admission control. |
| **AI-ENG-Z** | Telemetry & Metrics | Token burn, cost velocity, queue depth, quota burn, tool retries. | Resource telemetry must be attributed by tenant/user/session/workflow. |
| **AI-ENG-AA** | Evaluations | Cost-bomb, loop, retrieval-flood, batch-runaway tests. | Releases must test for unbounded consumption regressions. |
| **AI-ENG-AB** | Audit & Replay | Resource ledger, reservation records, request traces. | Replay must reconstruct cost, quota, and budget decisions. |
| **AI-ENG-AC** | Incident Response | Cost-bomb and quota-exhaustion playbooks. | Resource incidents require containment, reconciliation, customer impact analysis, and hardening. |
| **AI-ENG-AD** | Governance & Accountability | Budget policy, approval thresholds, exception handling. | Governance defines who may exceed budgets, when, and under what audit trail. |
| **AI-ENG-AJ** | Reference Architecture | Budget-aware gateway, quota service, ledger, circuit breaker. | Reference systems should include resource boundaries by default. |

## **Durable Principles of Resource Boundary Governance**

1. **Mechanical Enforcement Outside the Model**  
   Models cannot reliably police their own resource use. Resource envelopes must be enforced by gateways, orchestrators, schedulers, tool brokers, and batch controllers.

2. **Every Execution Path Needs a Resource Envelope**  
   Model calls, retrieval, reranking, parsing, browser sessions, tools, voice streams, human review, and batch jobs all require ceilings for tokens, time, calls, concurrency, quota, and spend.

3. **Context Is a Metered Execution Surface**  
   Context consumes memory, prefill time, money, and attention. Systems must budget context admission, compaction, retrieval insertion, memory loading, and tool-output retention.

4. **Fallback Must Preserve Fitness and Policy**  
   A cheaper model, cached answer, local parser, or degraded tool route is safe only if it preserves tenant scope, permission, data classification, task fitness, and user trust.

5. **Retry Requires Idempotency or Reconciliation**  
   Retrying model calls may waste money. Retrying tools may duplicate side effects. Mutating retries require idempotency keys, unknown-state handling, or explicit reconciliation.

6. **No-Progress Loops Must Halt**  
   Repeated plans, repeated tool payloads, repeated retrieval results, repeated UI clicks, and repeated repair errors are resource incidents. They require replan, clarification, escalation, or termination.

7. **Human Review Is a Scarce Resource**  
   Review queues can be attacked or accidentally flooded. Escalation requires budgets, triage, sampling, and clear fallback statuses.

8. **Batch Work Requires Preflight Economics**  
   Reindexing, embedding backfills, evaluations, and bulk parsing must estimate cost, dry-run on samples, checkpoint progress, and expose kill switches before full execution.

9. **Quota Isolation Complements Data Isolation**  
   Tenant data isolation prevents leaks. Tenant resource isolation prevents noisy-neighbor starvation and denial-of-wallet abuse.

10. **Cost Must Be Attributable**  
   Every token, tool call, retrieval expansion, parser run, browser action, and human-review minute should trace back to tenant, user, session, workflow, and route.

11. **Unknown Resource State Must Be Reconciled**  
   Timeouts, stream disconnects, provider failures, and gateway crashes can leave budget state uncertain. Reservations and usage records need durable reconciliation.

12. **Budget Breaches Are Security Signals**  
   Sudden cost velocity, retrieval fan-out, parser CPU spikes, or tool retry storms may indicate attack, compromise, drift, or pathological workload. Treat them as operational security events, not merely a billing annoyance.

#### **Works cited**

1. LLM10:2025 Unbounded Consumption - OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/)  
2. Stop a “Denial-of-Wallet” Attack with Token-Aware Rate Limiting | by ..., accessed June 10, 2026, [https://medium.com/towardsdev/token-aware-rate-limiting-denial-of-wallet-attack-c8f56adcfc97](https://medium.com/towardsdev/token-aware-rate-limiting-denial-of-wallet-attack-c8f56adcfc97)  
3. Top 5 Tools to Tackle Rate Limiting for LLM Apps - Maxim AI, accessed June 10, 2026, [https://www.getmaxim.ai/articles/top-5-tools-to-tackle-rate-limiting-for-llm-apps/](https://www.getmaxim.ai/articles/top-5-tools-to-tackle-rate-limiting-for-llm-apps/)  
4. OWASP LLM10: Unbounded Consumption in AI Systems - Indusface, accessed June 10, 2026, [https://www.indusface.com/learning/owasp-llm-unbounded-consumption/](https://www.indusface.com/learning/owasp-llm-unbounded-consumption/)  
5. AI-ENG-S — Production Pathologies - Hallucination, Malformed Output & Runaway Behavior  
6. AI-ENG-T — Boundary Defense - Prompt Injection, Data Leakage & Tenant Isolation  
7. AI-ENG-U — AI Supply Chain Security - Models, Datasets, Dependencies & Tool Surfaces  
8. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
9. Rate Limiting AI Agents: Preventing LLM API Exhaustion with a 3 ..., accessed June 10, 2026, [https://www.truefoundry.com/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion](https://www.truefoundry.com/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion)  
10. Agentic Loop - Albert Masoliver's learning site, accessed June 10, 2026, [https://albertml.com/Permanent/AI/AI+Agents+and+Patterns/Agentic+Loop](https://albertml.com/Permanent/AI/AI+Agents+and+Patterns/Agentic+Loop)  
11. Context Flooding | DeepTeam by Confident AI - The LLM Red Teaming Framework, accessed June 10, 2026, [https://www.trydeepteam.com/docs/red-teaming-adversarial-attacks-context-flooding](https://www.trydeepteam.com/docs/red-teaming-adversarial-attacks-context-flooding)  
12. When Query Expansion Hurts RAG. How “helpful” rewrites quietly ..., accessed June 10, 2026, [https://medium.com/@ThinkingLoop/when-query-expansion-hurts-rag-23139f06d8d4](https://medium.com/@ThinkingLoop/when-query-expansion-hurts-rag-23139f06d8d4)  
13. LatencyPrism: Zero-Intrusion LLM Profiling - Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/latencyprism](https://www.emergentmind.com/topics/latencyprism)  
14. Accuracy Is Speed: Towards Long-Context-Aware Routing for Distributed LLM Serving, accessed June 10, 2026, [https://arxiv.org/html/2604.15732v1](https://arxiv.org/html/2604.15732v1)  
15. How Uber Conquered Database Overload: The Journey from Static Rate-Limiting to Intelligent Load Management, accessed June 10, 2026, [https://www.uber.com/gb/en/blog/from-static-rate-limiting-to-intelligent-load-management/](https://www.uber.com/gb/en/blog/from-static-rate-limiting-to-intelligent-load-management/)  
16. Rate limiting for LLM applications: Why it matters and how to implement it - Portkey, accessed June 10, 2026, [https://portkey.ai/blog/rate-limiting-for-llm-applications/](https://portkey.ai/blog/rate-limiting-for-llm-applications/)  
17. What Is an Agent Loop? The Real Definition Business Leaders Need Right Now, accessed June 10, 2026, [https://rmgassociatesllc.com/insights/what-is-an-agent-loop](https://rmgassociatesllc.com/insights/what-is-an-agent-loop)  
18. What Is Loop Engineering? The New Meta for AI Coding Agents | MindStudio, accessed June 10, 2026, [https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents](https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents)  
19. A simple idea: separating a "Thinker" and "Observer" model to detect reasoning loops, accessed June 10, 2026, [https://discuss.huggingface.co/t/a-simple-idea-separating-a-thinker-and-observer-model-to-detect-reasoning-loops/174134](https://discuss.huggingface.co/t/a-simple-idea-separating-a-thinker-and-observer-model-to-detect-reasoning-loops/174134)  
20. [Bug]: Midscene aiAct may get stuck in local interaction loops during, accessed June 10, 2026, [https://github.com/web-infra-dev/midscene/issues/2115](https://github.com/web-infra-dev/midscene/issues/2115)  
21. Rate Limiting in LLM Applications: Why You Need It and How to Build It - DEV Community, accessed June 10, 2026, [https://dev.to/pranay_batta/rate-limiting-in-llm-applications-why-you-need-it-and-how-to-build-it-5gf4](https://dev.to/pranay_batta/rate-limiting-in-llm-applications-why-you-need-it-and-how-to-build-it-5gf4)  
22. Multi-Tenant LLM Serving on GPU Cloud: Per-Customer Isolation, Token Quotas, and Production SaaS Architecture Guide (2026) | Spheron Blog, accessed June 10, 2026, [https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/](https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/)  
23. Benchmarking Noisy-Neighbor Isolation on an A100: Shared vLLM vs 1g.5gb MIG Slices | by Owumi Festus | Medium, accessed June 10, 2026, [https://medium.com/@owumifestus/benchmarking-noisy-neighbor-isolation-on-an-a100-shared-vllm-vs-1g-5gb-mig-slices-d45f777d99f0](https://medium.com/@owumifestus/benchmarking-noisy-neighbor-isolation-on-an-a100-shared-vllm-vs-1g-5gb-mig-slices-d45f777d99f0)  
24. 10 Unbounded Consumption - owasp-llm - GitHub, accessed June 10, 2026, [https://github.com/microsoft/hve-core/blob/main/.github/skills/security/owasp-llm/references/10-unbounded-consumption.md](https://github.com/microsoft/hve-core/blob/main/.github/skills/security/owasp-llm/references/10-unbounded-consumption.md)  
25. Token-Budget-Aware Pool Routing for Cost-Efficient LLM Inference - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.09613v1](https://arxiv.org/html/2604.09613v1)  
26. Optimization and Tuning - vLLM Documentation, accessed June 10, 2026, [https://docs.vllm.ai/en/v0.7.3/performance/optimization.html](https://docs.vllm.ai/en/v0.7.3/performance/optimization.html)  
27. Understanding vLLM Scheduling: Token Budgets, Chunked Prefill, and Policies, accessed June 10, 2026, [https://audreywongkg.medium.com/understanding-vllm-scheduling-token-budgets-chunked-prefill-and-policies-2c879e3980e3](https://audreywongkg.medium.com/understanding-vllm-scheduling-token-budgets-chunked-prefill-and-policies-2c879e3980e3)  
28. Scheduling in inference engines - Fergus Finn, accessed June 10, 2026, [https://fergusfinn.com/blog/scheduling-in-inference-engines/](https://fergusfinn.com/blog/scheduling-in-inference-engines/)  
29. What Is the AI Agent Loop? The Core Architecture Behind Autonomous AI Systems, accessed June 10, 2026, [https://blogs.oracle.com/developers/what-is-the-ai-agent-loop-the-core-architecture-behind-autonomous-ai-systems](https://blogs.oracle.com/developers/what-is-the-ai-agent-loop-the-core-architecture-behind-autonomous-ai-systems)  
30. Evaluation of Prompt Injection Defenses in Large Language Models - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.23887v2](https://arxiv.org/html/2604.23887v2)  
31. CDRAG: RAG with LLM-guided document retrieval — outperforms standard cosine retrieval on legal QA - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/Rag/comments/1skldsb/cdrag_rag_with_llmguided_document_retrieval/](https://www.reddit.com/r/Rag/comments/1skldsb/cdrag_rag_with_llmguided_document_retrieval/)  
32. vllm.config.scheduler, accessed June 10, 2026, [https://docs.vllm.ai/en/latest/api/vllm/config/scheduler/](https://docs.vllm.ai/en/latest/api/vllm/config/scheduler/)  
33. Multi-tenant application patterns | Temporal Platform Documentation, accessed June 10, 2026, [https://docs.temporal.io/production-deployment/multi-tenant-patterns](https://docs.temporal.io/production-deployment/multi-tenant-patterns)  
34. Multi-Layer Scheduling for MoE-Based LLM Reasoning - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2602.21626v1](https://arxiv.org/html/2602.21626v1)  
35. An Empirical Catalog of 63 LLM-Agent Budget-Overrun Incidents, with an Affine-Typed Rust Mitigation as a Case Study - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2606.04056v1](https://arxiv.org/html/2606.04056v1)  
36. Building effective database retrieval tools for context engineering - Elastic, accessed June 10, 2026, [https://www.elastic.co/search-labs/blog/database-retrieval-tools-context-engineering](https://www.elastic.co/search-labs/blog/database-retrieval-tools-context-engineering)  
37. Structured, Agentic RAG for Ecommerce - /research - Fin AI, accessed June 10, 2026, [https://fin.ai/research/structured-agentic-rag-for-e-commerce/](https://fin.ai/research/structured-agentic-rag-for-e-commerce/)  
38. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.15771v1](https://arxiv.org/html/2604.15771v1)  
39. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.15771v2](https://arxiv.org/html/2604.15771v2)  
40. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2604.15771](https://arxiv.org/pdf/2604.15771)  
41. Enabling Performant and Flexible Model-Internal Observability for LLM Inference - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2605.11093v1](https://arxiv.org/html/2605.11093v1)  
42. Denial of Wallet: Cost-Aware Rate Limiting for Generative AI ..., accessed June 10, 2026, [https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/](https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/)  
43. Denial of Wallet: Cost-Aware Rate Limiting for Generative AI Applications - Hands-On Implementation (Part 3), accessed June 10, 2026, [https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/?utm_source=jvm-bloggers.com&utm_medium=link&utm_campaign=jvm-bloggers](https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/?utm_source=jvm-bloggers.com&utm_medium=link&utm_campaign=jvm-bloggers)  
44. Dual-Pool Token-Budget Routing for Cost-Efficient and Reliable LLM Serving, accessed June 10, 2026, [https://www.researchgate.net/publication/403683060_Dual-Pool_Token-Budget_Routing_for_Cost-Efficient_and_Reliable_LLM_Serving](https://www.researchgate.net/publication/403683060_Dual-Pool_Token-Budget_Routing_for_Cost-Efficient_and_Reliable_LLM_Serving)  
45. [2604.09613] Token-Budget-Aware Pool Routing for Cost-Efficient LLM Inference - arXiv, accessed June 10, 2026, [https://arxiv.org/abs/2604.09613](https://arxiv.org/abs/2604.09613)

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