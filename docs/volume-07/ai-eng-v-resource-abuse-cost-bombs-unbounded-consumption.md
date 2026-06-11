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
| **Denial-of-Wallet (DoW)** | An exploit vector targeting pay-per-token or pay-per-use utility pricing models to exhaust financial resources rather than causing raw hardware downtime.1 | Cost Velocity per Identity Tuple | $0.00 unbudgeted overruns 9 |
| **Resource Envelope** | The complete set of multi-dimensional limits (tokens, turns, time, tools, dollars) enclosing an active execution path, enforced outside the model's cognitive boundary.9 | Envelope Enforce Rate | 1.00 (Absolute coverage) |
| **Cost Bomb** | A crafted input payload or state designed to trigger disproportionate resource consumption, latency inflation, or billing spikes.1 | Amplification Cost Ratio | 1.00 (Observed vs Estimated) |
| **Context Flooding** | Overloading a model's active context window with redundant, verbose, or adversarial data to dilute attention mechanisms or force expensive prefill computations.5 | Context Utilization Density | \<50% window saturation |
| **Retrieval Flooding** | Forcing excessive vector, semantic, or relational search operations across databases via broad, ambiguous, or recursively expanded queries.12 | Retrieval Fan-out Factor | \<= 20 chunks per query 12 |
| **Tool Exhaustion** | The rapid depletion of third-party API quotas, local process pools, or connection limits through unmonitored model-driven invocations.3 | Tool Quota Exhaustion Frequency | 0.00 rate-limit cascades 7 |
| **Latency Inflation** | The deliberate or accidental degradation of token generation or execution speeds, holding active worker threads and starving system queues.13 | Tail Latency Delta (P99) | \<1.2x baseline average 15 |
| **Quota Exhaustion** | Depleting upstream model provider or internal hardware rate limits (RPM/TPM), resulting in cascade failures across tenant sessions.5 | Upstream 429 Rate | \<0.10% of outbound calls 9 |
| **Loop Budget** | The maximum step count, wall-clock time, token volume, and dollar spend allocated to an iterative, agentic workflow.9 | Loop Step Count | \<= 10 steps per execution 18 |
| **Progress Signal** | A verifiable change in system or environment state indicating an agent is converging toward a valid terminal state.17 | State Delta Metric | Delta State\!= 0 19 |
| **No-Progress State** | An execution state where an agent repeats identical or semantically equivalent tool calls or plans without converging.5 | State Repetition Count (C\_rep) | 0 repeated states on limit 5 |
| **Budget-Aware Gateway** | An external proxy terminating LLM traffic to enforce cost limits, token caps, fallback routing, and circuit breakers.9 | Gateway Transit Latency | \<15 microseconds overhead 3 |
| **Tenant Resource Isolation** | Partitioning and scheduling compute resources to prevent a single tenant from starving neighbor workloads.15 | Inter-Tenant Latency Variance | \<10 milliseconds variance |
| **Cost Attribution** | The precise trace associating every unit of consumed resource back to a specific tenant, user, session, and asset.5 | Unattributed Cost Ratio | 0.00% unattributed spend |

## **Resource Abuse Taxonomy**

To establish strict boundary controls, the system must differentiate among the primary modes of resource exhaustion:

                              RESOURCE EXHAUSTION MATRIX  
                                          │  
         ┌────────────────────────────────┴────────────────────────────────┐  
         ▼                                                                 ▼  
 \[ Availability Exhaustion \]                                      \[ Financial Exhaustion \]  
 ├─ GPU memory saturation (prefill storms)                        ├─ Pay-per-token pricing spikes  
 ├─ Worker thread starvation (Slowloris LLM)                      ├─ Upstream API credit depletion  
 └─ Connection pool lockups (hung tools)                          └─ Unmonitored recursive loops  
         │                                                                 │  
         ├────────────────────────────────┼────────────────────────────────┤  
         ▼                                                                 ▼  
 \[ Quota Exhaustion \]                                             \[ Latency Inflation \]  
 ├─ Upstream TPM/RPM rate limits hit                              ├─ Attention dilution in prefill  
 ├─ Multi-tenant rate-limit cascades                              ├─ Slow layout-aware parsing (OCR)  
 └─ Downstream third-party API lockout                            └─ Dynamic page rendering waits  
         │                                                                 │  
         ├────────────────────────────────┼────────────────────────────────┤  
         ▼                                                                 ▼  
                                              
 ├─ Head-of-line blocking (FCFS)                                  ├─ Unbounded tenant-wide reindexing  
 ├─ High-priority queue saturation                                ├─ Quadratic context amplification  
 └─ In-flight KV-cache preemption storms                          └─ Disconnected worker processes  
         │                                                                 │  
         └────────────────────────────────┬────────────────────────────────┘  
                                          ▼  
                             
                             ├─ Cascading low-confidence escalations  
                             ├─ Malformed OCR layout failures  
                             └─ Repetitive formatting repair loops

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
| **Turn Limits** | dialogue steps, retries, and repair iterations.5 | loop-step limits; max retry thresholds capped at \<= 2 attempts.5 |
| **Time Limits** | wall-clock, processing, and queue wait times.5 | asynchronous cancellation; worker thread isolation limits.8 |
| **Latency Limits** | model prefill, model decode, and tool latencies.13 | real-time anomaly detection; load-shedding triggers.13 |
| **Batch / Concurrency Limits** | parallel sessions, batch jobs, and GPU slices.5 | multi-tenant queues; weighted fair-share slot scheduling.22 |
| **Financial Limits** | dollar velocity per user, key, and tenant space.9 | token-aware pricing lookup; optimistic pre-consumption budgets.2 |
| **Human Review Limits** | queue depth and manual review time budgets.5 | dynamic validation rules; escalation throttling caps.5 |

## **Cost Bomb Classification**

The characteristics, metric signatures, and containment strategies of the primary cost bomb variants are structured in the classification database:

| Cost Bomb Variant | Entry Point | Amplification Path | Metric Signature | Primary Containment | Fallback Target | Regression Test |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Token Bomb** | chat inputs, dynamic scripts.5 | loop tokens triggering continuous repetitive generation.5 | output token rate matches maximum generate limit.2 | finite state machine constrained decoding.5 | terminate stream and return partial output.2 | repetitive token simulation checks.5 |
| **Context Bomb** | document uploads, meeting traces.6 | processing huge, uncompressed text arrays.6 | prefill latency surge; KV cache capacity drops.6 | hard physical limits on input file size.5 | compress files using a quarantined model.6 | long-context position sensitivity test.5 |
| **Retrieval Bomb** | search fields, query expanders.12 | query expansion causing broad vector index fan-outs.12 | database query QPS spikes; reranker delay rises.7 | pre-filtering vector query limits.31 | route search to a static lexical index.6 | multi-index search stress evaluations.5 |
| **Tool Bomb** | database updates, dynamic APIs.5 | un-buffered tool retries and loops.5 | external API connections hit limits.3 | tool consumption logs; turn limits.7 | return a simulated tool error response.5 | tool execution loop simulation runs.5 |
| **Vision Bomb** | PDF, image, video uploads.5 | multi-page scanned document parsing.5 | parser CPU usage rises; VLM processing lags.5 | selective ingestion routing filters.8 | fall back to a local heuristic OCR engine.5 | image parsing queue stress checks.5 |
| **Browser Bomb** | dynamic webpages, web crawling.8 | wait loops on responsive elements.5 | browser process count spikes; CPU use rises.7 | isolated browser sandboxing limits.7 | close tab and return raw HTML text.7 | dynamic page wait loop tests.5 |
| **Voice Bomb** | real-time telephony, WebRTC. | background noise triggering continuous audio turns. | WebRTC transceiver port use rises; packet drop rises. | client-side VAD audio triggers. | mute stream and prompt user to type. | background noise loop simulations. |
| **Batch Bomb** | reindexing triggers, log parsers.5 | background jobs expanding beyond limits.5 | queue task latency; memory limit crashes.5 | sample-first checks; spend caps.5 | split batch job and throttle execution.5 | batch job expansion limit tests.5 |
| **Cache-Bypass** | prefix guessed token arrays.6 | prefix timing probes invalidating caches.6 | prefill token rate rises; cache hits drop.6 | selective prefix isolation keys.6 | bypass cache and run standard model.6 | cache key collision attack tests.6 |
| **Review Bomb** | low-confidence audit queries.5 | fallback loops sending cases to reviews.5 | support queue backlog depth spikes.8 | dynamic verification bounds checks.8 | fall back to low-cost default states.5 | low-confidence escalation checks.5 |

## **Recursive Agents and Loop Amplification**

Agentic systems multiply resource consumption because each execution step can append context, invoke tools, query vector indexes, call models, and trigger recursive repair or self-reflection routines.5 In a naive ReAct or planning loop, the input token volume scales quadratically as the history of prior execution steps, formatting exceptions, and tool observations accumulates.5  
The total input tokens consumed across an N-turn agentic loop is modeled by the following quadratic context-accumulation equation:  
T\_total \= N \* S \+ (u \* N \* (N \+ 1)) / 2 \+ (r \* N \* (N \- 1)) / 2 5  
In this model, S represents the size (in tokens) of the system prompt and tool schema definitions, u is the average size of new incoming tokens per iteration (user inputs, formatting exceptions, or tool outputs), and r is the model's generated output per step.5  
When an agent enters an unmitigated loop, this quadratic context growth can rapidly deplete daily budgets and trigger denial-of-wallet incidents.5 Consequently, every iterative agent path must carry an explicit loop budget managed strictly outside the model by the orchestration engine.9  
The orchestration layer must enforce a multi-dimensional constraint model on all active loops:

* **Step Cap**: Hard limit on total execution turns (\<= 10 steps per session).10  
* **Time Cap**: Hard wall-clock limit on overall loop execution.10  
* **Token/Spend Cap**: Maximum cumulative tokens or dollars allocated per run.9  
* **Tool/Action Cap**: Maximum allowable external database queries or API calls.10  
* **Progress Auditing**: Dynamic validation checks to verify execution is moving toward completion.10  
* **Repeated-State Detection**: Pattern-matching checks to catch cyclical state transitions.10  
* **Timeout & Fail-Closed Gates**: Graceful error handling and rollback routines if execution stalls.10  
* **Escalation Path**: Handing off the session context to manual operators when limits are hit.5

### **Progress versus No-Progress States**

To prevent early termination of long-running, legitimate workflows, the loop manager must differentiate between active progress and repetitive, non-productive execution patterns.18

* **Progress Signals**: Indicated by measurable transitions that bring the loop closer to completion.17 These include acquiring unique, non-overlapping document IDs from vector indexes; reducing schema validation errors across repair turns; completing subtasks in the active plan; executing successful, non-duplicate API calls; or confirming parameter fields via user feedback.5  
* **No-Progress Signatures**: Indicated by repetitive or cyclical patterns that fail to transition the system's state.5 These include generating identical planner strings across steps; submitting duplicate tool payloads; repeating the same database query with identical results; encountering repeating validation errors; or executing identical mouse coordinates without UI layout changes.5

The progress detection framework maps these signals across active workflow domains:

| Workflow Domain | Verifiable Progress Signal | No-Progress Signatures | Recovery Action |
| :---- | :---- | :---- | :---- |
| **Planning** | subtask completion; updated sub-plans; narrowed search space.18 | repeated generation of identical plan strings across steps.5 | suspend planning; prompt user for clarification.5 |
| **Retrieval** | acquisition of unique, non-overlapping document IDs.8 | repeating identical search queries with identical results.5 | throttle search; fall back to cached results.5 |
| **Tools** | successful API response matching target schemas.5 | repeating identical tool payloads with repeating errors.5 | isolate tool; restrict retry budget to \<= 2 attempts.5 |
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
* **Conversation Compaction**: Recursively condensing older conversational turns into structured semantic summaries once the context utilization exceeds 50%.5  
* **Document-Size Limits**: Limits on raw page counts, layout nodes, or tabular elements processed in a single run.5  
* **Modality-Specific Limits**: Restricting image inputs to 300 DPI, limiting video capture to query-relevant keyframes, and enforcing maximum audio duration caps.8  
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

### **Gated Retrieval via Hidden-State Probing**

To mitigate retrieval flooding while preserving retrieval quality, the system must deploy a failure-state-aware framework like *Skill-RAG*.38 Rather than executing query expansion unconditionally, *Skill-RAG* uses a lightweight hidden-state prober to assess whether the model's parametric knowledge or current context is sufficient.38 The prober is a feed-forward network with a single hidden layer and a binary classification head, trained on correctness labels of representations extracted from the final two-thirds of the model's layers.39 If the prober detects a failure state, the system routes the query to one of four targeted corrective skills:

                            SKILL-RAG PIPELINE FLOW  
                                      │  
                                User Query  
                                      │  
                                      ▼  
                        
                         ├─ High confidence? ───\> Return Parametric Answer  
                         └─ Low confidence? ───\> Proceed to Retrieval  
                                      │  
                                      ▼  
                             
                                      │  
                                      ▼  
                       \[ Verification & Feedback Gate \]  
                         ├─ Answer sufficient? ──\> Return Grounded Answer  
                         └─ Stalled/Misaligned? ──\> Trigger Skill Router  
                                      │  
              ┌───────────────────────┼───────────────────────┐  
              ▼                       ▼                       ▼  
             \[ Evidence Focusing \]  
      ├─ Reformulates terms  ├─ Splits multi-hop steps  ├─ Crops precise regions  
      └─ Rectifies synonyms  └─ Solves logical chains   └─ Reduces token waste

* **Query Rewriting**: Applied when the input query is poorly specified for the target evidence space, reformulating search terms using verified synonyms.38  
* **Question Decomposition**: Applied when the query contains multi-hop dependencies, splitting it into sequential sub-questions to resolve logical chains.18  
* **Evidence Focusing**: Applied when the candidate set is broad or semantically noisy, using page coordinates to isolate and extract precise text regions.8  
* **Exit Skill**: Applied when the query represents an irreducible case, terminating retrieval gracefully to prevent infinite retry storms.5

To maintain strict cost control, retrieval budgets must be decoupled from authorization boundaries 6:

* **Permission-Aware Retrieval**: Enforces security constraints, validating *who* has authorization to view specific document directories.6  
* **Retrieval Budgets**: Enforces workload constraints, capping *how much* search, page-rendering, and citation-checking work a user or agent is permitted to trigger in a single session.6

The parameters of the *Retrieval Budget Model* are defined in the primary budget matrix:

| Parameter | Metric Limit Ceiling | Containment Action |
| :---- | :---- | :---- |
| **Query Cap** | \<= 3 rewritten queries per user request.5 | discard subsequent queries if budget is exhausted.5 |
| **Candidate Cap** | \<= 20 document chunks per query.12 | truncate candidate set size before passing to the reranker.12 |
| **Rerank Cap** | \<= 5 chunks scored per model call.8 | restrict reranker workload; enforce strict timeout parameters.8 |
| **Document Expansion** | \<= 2 neighboring pages retrieved.8 | restrict page-continuation retrieval; apply layout bounds.5 |
| **Page Rendering** | \<= 1 image rendered at 300 DPI.8 | limit visual rendering budgets; use text parsers when possible.8 |
| **Citation Checks** | 100% validation of generated quotes.5 | remove ungrounded citations; suppress unverified statements.5 |
| **Index Fan-Out** | \<= 2 vector partitions searched.6 | enforce database-level Row-Level Security and metadata filters.6 |

## **Tool Consumption Ledger**

Tools bridge the gap between generative reasoning and system operations, transforming model outputs into external actions.7 Uncontrolled tool execution can exhaust third-party API limits, trigger rate-limit cascades, saturate database connection pools, and deplete internal quotas.5 To manage this, the gateway must record every tool invocation in a centralized *Tool Consumption Ledger*.7  
The structure of the Tool Consumption Ledger is defined in the primary log schema:

| Parameter Field | Schema Data Type | System Governance Rule |
| :---- | :---- | :---- |
| transaction\_id | UUID (v4) | unique identifier generated for every tool invocation attempt.6 |
| tenant\_uuid | UUID (v4) | maps execution directly to the active B2B tenant space.6 |
| user\_session\_id | UUID (v4) | binds execution context to the authenticated user's session.6 |
| tool\_name | VARCHAR (255) | name of the tool, matching a registered, non-overlapping schema.36 |
| action\_class | VARCHAR (64) | categorizes tool as READ\_ONLY, LOW\_RISK\_MUTATION, or HIGH\_RISK.6 |
| cost\_class | VARCHAR (32) | maps tool to pricing tiers (e.g., FREE, EPHEMERAL\_PAY-PER-CALL).7 |
| idempotency\_key | VARCHAR (255) | mandatory key preventing duplicate mutations on retry attempts.5 |
| attempt\_count | INT | tracks current invocation try; strict limit of \<= 2 attempts.5 |
| retry\_reason | VARCHAR (512) | logs the specific exception or error code that triggered the retry.5 |
| latency\_ms | INT | execution time of the tool, bounded by hard timeout limits.8 |
| quota\_consumed | INT | amount of provider-specific quota consumed by this transaction.7 |
| result\_status | VARCHAR (32) | output status of the tool (e.g., SUCCESS, FAILED, BLOCKED).7 |
| error\_class | VARCHAR (128) | categorizes returned exception types for pattern tracking.5 |
| cumulative\_spend | DECIMAL (10, 4\) | total financial cost incurred by this tool session in USD.7 |

Every tool invocation must be evaluated against the user's active session role and tenant budget before execution.6 State-changing operations require stronger gates. The system must verify the success of a transaction inside the database of record before generating any verbal or text-based confirmation to the user, ensuring the agent's statements align with system state.5

## **Adversarial Latency Inflation Threat Model**

Adversarial latency inflation exploits serving-engine scheduling behavior to degrade system availability. Under long-context workloads, inference accuracy becomes highly variable, and when incorrect answers trigger retries or escalation, these routing mistakes inflate cumulative, user-visible delays.14 The actual performance of a long-context system is therefore modeled using the *Time-to-Correct-Answer (TTCA)* metric, which measures the total wall-clock time required to obtain the first correct, verified response, accounting for retries and escalations.14  
Attackers can deliberately inflate latency by submitting inputs designed to maximize processing times. High-resolution images, multi-page scanned PDFs, uncacheable prefixes, and complex math-heavy prompts force serving engines to spend excessive cycles in the compute-bound prefill phase.6 Since modern engines process requests in batches, a small number of latency-inflated requests can occupy active worker threads, saturate GPU caches, and cause queue starvation for all other concurrent users.13  
To protect serving availability, the platform must enforce stage-wise latency budgets and isolate workloads.

                         STAGE-WISE LATENCY BUDGETS  
                                      │  
                             \[ Input Validation \] (10ms)  
                                      │  
                                 (120ms)  
                                      │  
                                    (45ms)  
                                      │  
                                    (80ms)  
                                      │  
                             \[ Model Prefill \]    (150ms)  
                                      │  
                                 (250ms)  
                                      │  
                               (180ms)  
                                      │  
                                 (90ms)  
                                      │  
                                 User Speaker

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
While physical GPU partitioning using Multi-Instance GPU (MIG) slices provides absolute latency isolation, it reduces overall fleet flexibility and underutilizes accelerators during low-traffic periods.23 To balance efficiency with reliability, platforms must deploy intelligent overload and admission control at the gateway layer.15 The system must maintain separate CoDel (Controlled Delay) queues for distinct operation types—such as point lookups (Read Queue), data writes (Write Queue), and long-running scans (Slow Queue)—to prevent background tasks from blocking real-time user traffic.15 Concurrency must be managed dynamically based on Little's Law:  
Concurrency \= Throughput \* Latency 15  
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

                       RESERVATION & REFUND FLOW  
       │  
       ▼  
┌────────────────────────────────────────────────────────┐  
│ Phase 1: Upfront Reservation                           │  
│ 1\. Tokenize prompt: L\_in \= prompt\_tokens               │  
│ 2\. Read request parameter: L\_out\_max \= max\_tokens      │  
│ 3\. Compute Reserved Cost \= L\_in\*Pin \+ L\_out\_max\*Pout   │  
│ 4\. Deduct Reserved Cost atomically from Redis bucket   │  
└────────────────────────┬───────────────────────────────┘  
                         │  
                 (Passes Quota Check)  
                         │  
                         ▼  
            \[ LLM Generation Execution \]  
                         │  
               (Generation Completes)  
                         │  
                         ▼  
┌────────────────────────────────────────────────────────┐  
│ Phase 2: Reconciliation & Refund                       │  
│ 1\. Read actual usage: L\_out\_actual \= completion\_tokens │  
│ 2\. Compute Actual Cost \= L\_in\*Pin \+ L\_out\_actual\*Pout  │  
│ 3\. Compute Refund \= Reserved Cost \- Actual Cost        │  
│ 4\. Credit Refund value back to the user's Redis bucket │  
└────────────────────────────────────────────────────────┘

1. **Phase 1: The Reservation (Upfront Estimation)**  
   When a request arrives, the gateway tokenizes the incoming prompt to determine the input token count (L\_in).2 It reads the max\_tokens parameter (L\_out\_max) from the request and computes the worst-case estimated cost 2:  
   Reserved Cost \= L\_in \* P\_input \+ L\_out\_max \* P\_output 2  
   The gateway checks the user's token bucket in Redis. If the bucket has insufficient tokens, the request is immediately blocked, returning an HTTP 429 Too Many Tokens error.2 If sufficient, the gateway atomically decrements the estimated cost from the user's quota.43  
2. **Phase 2: The Refund (Reconciliation)**  
   Once response generation completes, the gateway reads the actual completed token count (L\_out\_actual) from the provider's response metadata.2 It calculates the actual cost and refunds the unused tokens back to the user's bucket 2:  
   Actual Cost \= L\_in \* P\_input \+ L\_out\_actual \* P\_output 43  
   Refund \= Reserved Cost \- Actual Cost 2  
   For streaming connections, the gateway counts tokens progressively as individual chunks arrive over the WebSocket.2 It captures the final chunk's usage.completion\_tokens metadata to compute and apply the remaining refund before the HTTP socket is closed.2

### **Three-Layer Rate Limiting Architecture**

The gateway enforces rate limiting across three complementary layers to defend against both volume and pattern-based resource abuse.9

* **Layer 1: Token Bucket Per Identity**  
  Every identity tuple, represented as (user, repo, model), is assigned its own token bucket in Redis to isolate rogue processes without blocking unrelated workloads.9 It operates using a continuous refill algorithm: the bucket stores a last\_refill\_timestamp in Redis, and on every request, the system calculates the time elapsed and adds tokens proportionally based on the configured refill rate.42  
* **Layer 2: Pattern-Based Circuit Breakers**  
  Circuit breakers monitor traffic signatures in real time and trip (failing fast for 60 seconds) when they detect anomalous behaviors.9 Breakers trip when an agent ignores standard Retry-After headers and generates consecutive 429s, when error rates exceed 50% in a 60-second window, when cost velocity exceeds the planned budget rate, or when call shapes indicate runaway loops (e.g., identical prompts or monotonically growing context sizes).9  
* **Layer 3: Fallback Chains**  
  When a primary route is throttled or its circuit breaker is open, the gateway routes traffic down a declarative fallback chain.9 It transitions from the primary model to a cheaper, faster model, then to a semantic cache hit, and finally returns a managed HTTP 503 Service Unavailable status code if all other options are exhausted.9

### **Token-Budget-Aware Pool Routing**

To optimize serving instance utilization, the gateway implements a fleet-level token-budget-aware routing algorithm. Standard vLLM fleets configure every serving instance for the maximum context window (C\_max), wasting substantial GPU memory capacity on short requests. The gateway partitions a homogeneous fleet into two specialized pools: a high-throughput Short Pool (P\_s) right-sized for short contexts, and a High-Capacity Long Pool (P\_l) configured for the maximum context.  
At dispatch time, the gateway estimates the total token budget (L\_total) for request r of content category k 25:  
L\_total \= ceil(|r| / c\_k\*) \+ r.max\_output\_tokens 25  
where |r| represents the byte length of request r, and c\_k\* is the conservative bytes-to-token ratio calculated as:  
c\_k\* \= c\_k\_hat \- gamma \* sigma\_k\_hat 25  
The variables c\_k\_hat and sigma\_k\_hat (positive real numbers) represent the per-category exponential moving average (EMA) of the bytes-to-token ratio and its standard deviation, learned online from incoming usage.prompt\_tokens feedback 25:  
c\_obs \= |r| / usage.prompt\_tokens 25  
c\_k\_hat \= beta \* c\_k\_hat \+ (1 \- beta) \* c\_obs 25  
sigma\_k\_hat \= EMA(sigma\_k\_hat, |c\_obs \- c\_k\_hat|) 25  
The constant gamma (positive real number) represents an asymmetric, error-aware safety parameter ensuring conservative budget estimation.25 If L\_total \> C\_max(P\_s), the gateway routes the request directly to the long pool P\_l; otherwise, it dispatches the request to the high-concurrency short pool P\_s, optimizing GPU memory utilization without the latency overhead of running model-specific tokenizers.25  
This analytical split is governed by a closed-form fleet-level cost savings model:  
Savings \= alpha \* (1 \- 1/rho) 25  
where alpha (between 0 and 1\) is the short-traffic fraction of the workload, and rho (greater than 1\) is the throughput gain ratio achieved by running the short pool under a tighter memory configuration.25

## **Cost Attribution Architecture**

The platform cannot defend its resource boundaries if it cannot attribute consumption. The gateway must record and trace every transaction, associating all token, compute, and integration expenses back to specific system entities.5  
Required parameters captured in the cost attribution schema:

* **Identifiers**: request\_id, tenant\_id, user\_id, session\_id, workflow\_id, model\_id.  
* **Token Metrics**: prompt\_tokens, completion\_tokens, cached\_tokens, embedding\_tokens.  
* **Pipelining & Retrieval**: retrieved\_candidates\_count, rerank\_calls, cache\_hit\_status.  
* **Modality & Parse**: ocr\_pages, video\_frames\_sampled, browser\_actions\_count.  
* **Tools & Escalations**: tool\_calls\_count, retries\_count, human\_review\_minutes.  
* **Infrastructure Metrics**: wall-clock\_time\_ms, queue\_wait\_time\_ms, gpu\_execution\_time\_ms.  
* **Financials**: external\_api\_spend\_usd, internal\_resource\_cost\_usd.  
* **Termination Status**: termination\_reason (e.g., success, max\_turns\_exceeded, budget\_breach).

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

                      INCIDENT RESPONSE PROTOCOL ENGINE  
                                      │  
                         SIEM Alert / Anomaly Trigger  
                                      │  
                                      ▼  
                           \[ Containment Phase \]  
                           ├─ Freeze actor JWT credentials  
                           ├─ Terminate runaway batch jobs  
                           └─ Stop loop-offending threads  
                                      │  
                                      ▼  
                           \[ Mitigation Phase \]  
                           ├─ Pause compromised queues  
                           ├─ Lower model routing tiers  
                           └─ Rotate exposed integration keys  
                                      │  
                                      ▼  
                            
                           ├─ Invalidate cache partitions  
                           ├─ Recalibrate tenant quotas  
                           └─ Inject regression test to CI/CD

* **Containment**: The gateway identifies a budget breach or loop anomaly and automatically freezes the affected user's session tokens.5 Cost circuit breakers trip globally on the compromised model routes, blocking further API calls.9 Runaway background batch jobs and agent workflows are immediately terminated by the orchestrator.5  
* **Isolation**: The platform pauses processing on compromised task queues to prevent cascading failures.5 Exposed tool credentials and API tokens are revoked and rotated.6 Model routing parameters are updated to force traffic to low-cost, local fallback instances while the primary environment is investigated.5  
* **Traceback & Recovery**: SRE teams analyze execution traces and log hashes to calculate the precise financial impact and identify the exploit pathway.5 Vector database indexes are audited to locate and isolate poisoned documents.6 Platform quotas are restored, and affected customer accounts are credited.5  
* **Hardening & Post-Mortem**: A root-cause analysis is published to catalog the failure mechanism.7 Security teams construct a regression test reproducing the exploit pattern, integrating it into the CI/CD pipeline to prevent future regressions.5

## **Cross-Canon Handoff Map**

The resource-control architecture established in this report interfaces directly with adjacent disciplines across the AI Systems Engineering Canon.

| Downstream Target Report | Volume & Area | Core Dependency | Operational Rule | Fallback Action |
| :---- | :---- | :---- | :---- | :---- |
| **AI-ENG-W** | Vol 8: Fallback Modes | progressive degradation thresholds.5 | trigger local, CPU-bound Tesseract parsing when VLM rate limits are hit.8 | fail closed; return a managed system exception.5 |
| **AI-ENG-X** | Vol 8: User Trust | real-time coordinate highlights.8 | render visual bounding box overlays directly on user interface screens.8 | display source document name and text snippet only.6 |
| **AI-ENG-Y** | Vol 8: Human Review | validation confidence scores.5 | route character OCR results below 0.60 confidence to manual review.8 | apply low-cost default states; skip review.5 |
| **AI-ENG-Z** | Vol 9: Telemetry | standardized syslog telemetry schema with PII masking.5 | redact credentials and API keys in log streams before writing to SIEM.6 | purge logs automatically if unmasked secrets are found.6 |
| **AI-ENG-AA** | Vol 9: Adversarial Evals | adversarial injection test suites.6 | block software releases in CI/CD if injection protection scores drop.6 | roll back build branch to last stable release.6 |
| **AI-ENG-AB** | Vol 9: Audit & Replay | signed transaction logs and hashes.5 | store complete variable dependency maps alongside conversation traces.5 | log unhashed transactions in local syslog volumes.6 |
| **AI-ENG-AC** | Vol 9: Incident Response | index quarantine playbooks.6 | rebuild HNSW index partitions from backups if poisoning is flagged.6 | terminate vector search; fall back to lexical search.6 |
| **AI-ENG-AJ** | Vol 10: Ref Architecture | multi-tenant pgvector schema maps.6 | enforce database-level Row-Level Security on every similarity query.6 | separate physical database partitions per tenant.6 |

## **Durable Principles of Resource Boundary Governance**

1. **Mechanical Enforcement Outside the Model**: Models are probabilistic generation engines that cannot reliably police their own resource consumption. System prompts, structural instructions, and self-reflection loops fail under context pressure or adversarial input. Resource envelopes must be enforced programmatically by external software frameworks.  
2. **Every Execution Path Needs a Resource Envelope**: An unbounded system component is an active availability and financial risk. Every model call, tool execution, database retrieval, batch process, browser automation, or voice session must operate within strict ceilings for tokens, turns, time, and spend.  
3. **The Context Window is a Metered Execution Surface**: Context is not free; it carries quadratic cost characteristics and directly affects tail latency and memory utilization. The system must actively manage context lifetimes, truncating and compressing logs before they saturate KV caches or trigger preemption storms.  
4. **No Cost Discovery After Execution**: Batch jobs, background tasks, and agentic workflows must not discover their token and financial footprints after they run. The system must calculate preflight estimates, perform dry-run checks over sample data, and verify execution budgets before scheduling bulk operations.  
5. **Data Isolation Demands Quota Isolation**: Multi-tenant security requires more than separating relational database rows. To prevent denial-of-wallet attacks and noisy-neighbor queue starvation, systems must enforce tenant-specific rate limits, fair-share scheduling slots, and budget ceilings at the gateway layer.

#### **Works cited**

1. LLM10:2025 Unbounded Consumption \- OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/)  
2. Stop a “Denial-of-Wallet” Attack with Token-Aware Rate Limiting | by ..., accessed June 10, 2026, [https://medium.com/towardsdev/token-aware-rate-limiting-denial-of-wallet-attack-c8f56adcfc97](https://medium.com/towardsdev/token-aware-rate-limiting-denial-of-wallet-attack-c8f56adcfc97)  
3. Top 5 Tools to Tackle Rate Limiting for LLM Apps \- Maxim AI, accessed June 10, 2026, [https://www.getmaxim.ai/articles/top-5-tools-to-tackle-rate-limiting-for-llm-apps/](https://www.getmaxim.ai/articles/top-5-tools-to-tackle-rate-limiting-for-llm-apps/)  
4. OWASP LLM10: Unbounded Consumption in AI Systems \- Indusface, accessed June 10, 2026, [https://www.indusface.com/learning/owasp-llm-unbounded-consumption/](https://www.indusface.com/learning/owasp-llm-unbounded-consumption/)  
5. AI-ENG-S — Production Pathologies \- Hallucination, Malformed Output & Runaway Behavior  
6. AI-ENG-T — Boundary Defense \- Prompt Injection, Data Leakage & Tenant Isolation  
7. AI-ENG-U — AI Supply Chain Security \- Models, Datasets, Dependencies & Tool Surfaces  
8. \[KNOWLEDGE\] \- AI Engineering \- Volume 6\. P-R Multimodal and Interface-Controlling Systems  
9. Rate Limiting AI Agents: Preventing LLM API Exhaustion with a 3 ..., accessed June 10, 2026, [https://www.truefoundry.com/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion](https://www.truefoundry.com/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion)  
10. Agentic Loop \- Albert Masoliver's learning site, accessed June 10, 2026, [https://albertml.com/Permanent/AI/AI+Agents+and+Patterns/Agentic+Loop](https://albertml.com/Permanent/AI/AI+Agents+and+Patterns/Agentic+Loop)  
11. Context Flooding | DeepTeam by Confident AI \- The LLM Red Teaming Framework, accessed June 10, 2026, [https://www.trydeepteam.com/docs/red-teaming-adversarial-attacks-context-flooding](https://www.trydeepteam.com/docs/red-teaming-adversarial-attacks-context-flooding)  
12. When Query Expansion Hurts RAG. How “helpful” rewrites quietly ..., accessed June 10, 2026, [https://medium.com/@ThinkingLoop/when-query-expansion-hurts-rag-23139f06d8d4](https://medium.com/@ThinkingLoop/when-query-expansion-hurts-rag-23139f06d8d4)  
13. LatencyPrism: Zero-Intrusion LLM Profiling \- Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/latencyprism](https://www.emergentmind.com/topics/latencyprism)  
14. Accuracy Is Speed: Towards Long-Context-Aware Routing for Distributed LLM Serving, accessed June 10, 2026, [https://arxiv.org/html/2604.15732v1](https://arxiv.org/html/2604.15732v1)  
15. How Uber Conquered Database Overload: The Journey from Static Rate-Limiting to Intelligent Load Management, accessed June 10, 2026, [https://www.uber.com/gb/en/blog/from-static-rate-limiting-to-intelligent-load-management/](https://www.uber.com/gb/en/blog/from-static-rate-limiting-to-intelligent-load-management/)  
16. Rate limiting for LLM applications: Why it matters and how to implement it \- Portkey, accessed June 10, 2026, [https://portkey.ai/blog/rate-limiting-for-llm-applications/](https://portkey.ai/blog/rate-limiting-for-llm-applications/)  
17. What Is an Agent Loop? The Real Definition Business Leaders Need Right Now, accessed June 10, 2026, [https://rmgassociatesllc.com/insights/what-is-an-agent-loop](https://rmgassociatesllc.com/insights/what-is-an-agent-loop)  
18. What Is Loop Engineering? The New Meta for AI Coding Agents | MindStudio, accessed June 10, 2026, [https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents](https://www.mindstudio.ai/blog/what-is-loop-engineering-ai-coding-agents)  
19. A simple idea: separating a "Thinker" and "Observer" model to detect reasoning loops, accessed June 10, 2026, [https://discuss.huggingface.co/t/a-simple-idea-separating-a-thinker-and-observer-model-to-detect-reasoning-loops/174134](https://discuss.huggingface.co/t/a-simple-idea-separating-a-thinker-and-observer-model-to-detect-reasoning-loops/174134)  
20. \[Bug\]: Midscene aiAct may get stuck in local interaction loops during, accessed June 10, 2026, [https://github.com/web-infra-dev/midscene/issues/2115](https://github.com/web-infra-dev/midscene/issues/2115)  
21. Rate Limiting in LLM Applications: Why You Need It and How to Build It \- DEV Community, accessed June 10, 2026, [https://dev.to/pranay\_batta/rate-limiting-in-llm-applications-why-you-need-it-and-how-to-build-it-5gf4](https://dev.to/pranay_batta/rate-limiting-in-llm-applications-why-you-need-it-and-how-to-build-it-5gf4)  
22. Multi-Tenant LLM Serving on GPU Cloud: Per-Customer Isolation, Token Quotas, and Production SaaS Architecture Guide (2026) | Spheron Blog, accessed June 10, 2026, [https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/](https://www.spheron.network/blog/multi-tenant-llm-serving-gpu-cloud/)  
23. Benchmarking Noisy-Neighbor Isolation on an A100: Shared vLLM vs 1g.5gb MIG Slices | by Owumi Festus | Medium, accessed June 10, 2026, [https://medium.com/@owumifestus/benchmarking-noisy-neighbor-isolation-on-an-a100-shared-vllm-vs-1g-5gb-mig-slices-d45f777d99f0](https://medium.com/@owumifestus/benchmarking-noisy-neighbor-isolation-on-an-a100-shared-vllm-vs-1g-5gb-mig-slices-d45f777d99f0)  
24. 10 Unbounded Consumption \- owasp-llm \- GitHub, accessed June 10, 2026, [https://github.com/microsoft/hve-core/blob/main/.github/skills/security/owasp-llm/references/10-unbounded-consumption.md](https://github.com/microsoft/hve-core/blob/main/.github/skills/security/owasp-llm/references/10-unbounded-consumption.md)  
25. Token-Budget-Aware Pool Routing for Cost-Efficient LLM Inference \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.09613v1](https://arxiv.org/html/2604.09613v1)  
26. Optimization and Tuning \- vLLM Documentation, accessed June 10, 2026, [https://docs.vllm.ai/en/v0.7.3/performance/optimization.html](https://docs.vllm.ai/en/v0.7.3/performance/optimization.html)  
27. Understanding vLLM Scheduling: Token Budgets, Chunked Prefill, and Policies, accessed June 10, 2026, [https://audreywongkg.medium.com/understanding-vllm-scheduling-token-budgets-chunked-prefill-and-policies-2c879e3980e3](https://audreywongkg.medium.com/understanding-vllm-scheduling-token-budgets-chunked-prefill-and-policies-2c879e3980e3)  
28. Scheduling in inference engines \- Fergus Finn, accessed June 10, 2026, [https://fergusfinn.com/blog/scheduling-in-inference-engines/](https://fergusfinn.com/blog/scheduling-in-inference-engines/)  
29. What Is the AI Agent Loop? The Core Architecture Behind Autonomous AI Systems, accessed June 10, 2026, [https://blogs.oracle.com/developers/what-is-the-ai-agent-loop-the-core-architecture-behind-autonomous-ai-systems](https://blogs.oracle.com/developers/what-is-the-ai-agent-loop-the-core-architecture-behind-autonomous-ai-systems)  
30. Evaluation of Prompt Injection Defenses in Large Language Models \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.23887v2](https://arxiv.org/html/2604.23887v2)  
31. CDRAG: RAG with LLM-guided document retrieval — outperforms standard cosine retrieval on legal QA \- Reddit, accessed June 10, 2026, [https://www.reddit.com/r/Rag/comments/1skldsb/cdrag\_rag\_with\_llmguided\_document\_retrieval/](https://www.reddit.com/r/Rag/comments/1skldsb/cdrag_rag_with_llmguided_document_retrieval/)  
32. vllm.config.scheduler, accessed June 10, 2026, [https://docs.vllm.ai/en/latest/api/vllm/config/scheduler/](https://docs.vllm.ai/en/latest/api/vllm/config/scheduler/)  
33. Multi-tenant application patterns | Temporal Platform Documentation, accessed June 10, 2026, [https://docs.temporal.io/production-deployment/multi-tenant-patterns](https://docs.temporal.io/production-deployment/multi-tenant-patterns)  
34. Multi-Layer Scheduling for MoE-Based LLM Reasoning \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2602.21626v1](https://arxiv.org/html/2602.21626v1)  
35. An Empirical Catalog of 63 LLM-Agent Budget-Overrun Incidents, with an Affine-Typed Rust Mitigation as a Case Study \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2606.04056v1](https://arxiv.org/html/2606.04056v1)  
36. Building effective database retrieval tools for context engineering \- Elastic, accessed June 10, 2026, [https://www.elastic.co/search-labs/blog/database-retrieval-tools-context-engineering](https://www.elastic.co/search-labs/blog/database-retrieval-tools-context-engineering)  
37. Structured, Agentic RAG for Ecommerce \- /research \- Fin AI, accessed June 10, 2026, [https://fin.ai/research/structured-agentic-rag-for-e-commerce/](https://fin.ai/research/structured-agentic-rag-for-e-commerce/)  
38. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.15771v1](https://arxiv.org/html/2604.15771v1)  
39. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2604.15771v2](https://arxiv.org/html/2604.15771v2)  
40. Skill-RAG: Failure-State-Aware Retrieval Augmentation via Hidden-State Probing and Skill Routing \- arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2604.15771](https://arxiv.org/pdf/2604.15771)  
41. Enabling Performant and Flexible Model-Internal Observability for LLM Inference \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2605.11093v1](https://arxiv.org/html/2605.11093v1)  
42. Denial of Wallet: Cost-Aware Rate Limiting for Generative AI ..., accessed June 10, 2026, [https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/](https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/)  
43. Denial of Wallet: Cost-Aware Rate Limiting for Generative AI Applications \- Hands-On Implementation (Part 3), accessed June 10, 2026, [https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/?utm\_source=jvm-bloggers.com\&utm\_medium=link\&utm\_campaign=jvm-bloggers](https://handsonarchitects.com/blog/2026/denial-of-wallet-cost-aware-rate-limiting-part-3/?utm_source=jvm-bloggers.com&utm_medium=link&utm_campaign=jvm-bloggers)  
44. Dual-Pool Token-Budget Routing for Cost-Efficient and Reliable LLM Serving, accessed June 10, 2026, [https://www.researchgate.net/publication/403683060\_Dual-Pool\_Token-Budget\_Routing\_for\_Cost-Efficient\_and\_Reliable\_LLM\_Serving](https://www.researchgate.net/publication/403683060_Dual-Pool_Token-Budget_Routing_for_Cost-Efficient_and_Reliable_LLM_Serving)  
45. \[2604.09613\] Token-Budget-Aware Pool Routing for Cost-Efficient LLM Inference \- arXiv, accessed June 10, 2026, [https://arxiv.org/abs/2604.09613](https://arxiv.org/abs/2604.09613)

---

[← Back to Canon Map](../canon-map.md)