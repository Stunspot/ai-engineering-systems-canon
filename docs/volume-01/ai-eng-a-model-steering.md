# AI-ENG-A - Model Steering - Harness Engineering, Prompt Semantics, and Adaptation Choice

## **The Paradigm of Model Steering**

The deployment of large language models into enterprise software architectures requires a departure from traditional deterministic programming paradigms.1 In standard software systems, execution is governed by concrete execution paths, predictable memory registers, and strict type constraints. In contrast, large language models operate as probabilistic semantic engines whose native interface is unstructured natural language.1 Consequently, application developers cannot treat models as stateless computing functions; instead, they must implement a multi-layered discipline of behavioral guidance.1  
This discipline is defined as **model steering**: the systematic control of probabilistic model outputs through a coordinated suite of semantic, contextual, architectural, runtime, evaluative, and parameter-adaptation control surfaces.1 Within this framework, prompt engineering is no longer positioned as the complete field of study, but rather as a highly localized, linguistic control surface nested inside a much larger systems engineering architecture.1  
To organize and validate any engineering intervention within this paradigm, the system architect must operate under a single, central governing question:  
Steering Intervention = min_{Cost, Latency, Complexity} f(Delta Behavior, Reversibility, Maintainability)  
This governing question acts as the structural filter for the entire system design. Every behavioral deviation, syntax failure, or security vulnerability must be routed to the lightest, most maintainable, and most easily reversed layer of the architecture capable of robustly solving the problem.4  
This report provides the foundational doctrinal knowledge base for Volume 1: The Informational/Epistemic Layer, establishing the vocabulary, concepts, and decision boundaries required to build resilient, production-grade AI systems.2

## **Conceptual Glossary**

To establish a precise and durable vocabulary across subsequent volumes of the canon, the following terms are defined in compact, mathematically and architecturally precise language:

| Term | Definition | Primary Substrate |
| :---- | :---- | :---- |
| **Model Steering** | The programmatic and semantic manipulation of token probability distributions to yield aligned, predictable, and secure model outputs.1 | System Architecture |
| **Semantic Steering** | The design of natural language contexts, structural tags, and performance examples to prime specific regions of the model's token distribution.6 | Token Space |
| **Harness Engineering** | The deterministic application envelope that coordinates prompt rendering, message structures, API constraints, and validation loops.4 | Application Runtime |
| **Instruction Hierarchy** | The prioritized classification of instruction payloads based on trust boundaries to resolve conflicts (e.g., Platform > Developer > User > Tool).8 | Post-Training Weights |
| **Constrained Decoding** | The runtime filtering of logit distributions at the sampling level to guarantee syntactical adherence to context-free grammars or schemas.10 | Inference Engine |
| **Self-Routing** | The dynamic classification of incoming queries to route tasks between low-cost retrieval-augmented pathways and deep context windows.12 | Orchestration Layer |
| **Temporal Knowledge Graph** | A database architecture that maps entity relations alongside temporal validity windows to maintain conversational state across sessions.2 | Memory Substrate |
| **Direct Preference Optimization (SecAlign)** | A training paradigm that directly optimizes the relative log-likelihood of secure output paths over insecure or adversarial trajectories.13 | Parameter Weights |
| **Usable Context Ceiling** | The empirical token-boundary where multi-fact retrieval rates degrade significantly below the advertised maximum context window size.12 | Attention Space |
| **Forced Fabrication** | A decoding pathology where schema constraints force a model to output a hallucinated value because it lacks the capacity to express uncertainty.11 | Logit Mask Layer |

## **Semantic Steering as Behavioral Interface Design**

Linguistic promptcraft is often mischaracterized as a superficial trial-and-error process. In a professional systems context, semantic steering represents first-class behavioral interface design: the engineering of a performance-seeded linguistic runway that leverages the model’s pre-existing token weights to minimize next-token entropy.3 Language models do not follow instructions through semantic understanding; instead, they continue token sequences based on conditional probability distributions.3  
The prompt's structural choreography, rhythm, delimiters, and examples determine how effectively the model navigates its parametric memory without drifting into unaligned or hallucinated states.6 Semantic steering achieves high-fidelity control by orchestrating twelve distinct linguistic framing vectors:

* **Task Framing:** Establishing the exact boundaries, scope, and objective of the immediate transformation, clearly defining what is inside and outside the execution contract.4  
* **Role Framing:** Activating specific, high-density token regions in the model’s parametric weights by assigning a highly localized professional archetype (e.g., "senior systems architect specializing in distributed databases").4  
* **Audience Framing:** Explicitly defining the consumer of the generated output, which automatically calibrates the vocabulary, structural complexity, tone, and technical depth of the token sequence.15  
* **Evidence Framing:** Wrapping verified data and retrieved documents in clear, custom-tagged structural containers to direct the model's attention heads to prioritize specific factual inputs over its own parametric memories.4  
* **Decision Framing:** Providing the exact logical heuristics and conditional steps the model must follow when encountering edge cases, missing data, or conflicting priorities.4  
* **Failure Framing:** Programmatically defining the fallback behaviors, error responses, and refusal patterns when requested tasks are outside safe operational boundaries or lack sufficient input data.4  
* **Output Framing:** Structuring the expected generation format through explicit constraints, such as defining specific XML wrappers, prohibiting specific markdown styles, or dictating output length.6  
* **Example Framing (Few-Shot):** Showcasing 3 to 5 highly relevant, diverse, and structured input-output pairs inside XML containers to demonstrate tone, format, and edge-case handling without resorting to negative rules.6  
* **Interaction Framing:** Defining the conversational protocol, such as instructing the model to propose multiple design paths first and wait for approval before generating code.6  
* **Salience and Continuation Momentum:** Utilizing structural techniques, such as placing the primary query at the absolute end of the input sequence, to sustain generation trajectory and prevent attention drift.6  
* **Genre Continuation:** Structuring the prompt as the natural preface to the desired response, forcing the model’s autoregressive decoder to generate the exact style required.6  
* **Lexical Priors:** Choosing highly specific, unambiguous domain terminology to trigger exact parametric sub-networks, avoiding generalized vocabulary that leads to conversational drift.4

For example, when using frontier models like Claude 4.8 or Claude Opus 4.5, formatting defaults can be overridden by embedding aesthetic rules in explicit <frontend_aesthetics> XML blocks.6 This mechanism steers the model away from generic, overused styles—often referred to as "AI slop"—by setting explicit constraints on margins, typography, and palette hex codes.6

## **Harness Engineering and the Message Assembly Layer**

To transition from experimental prompting to production-grade AI systems, developers must establish a clear separation of concerns between the prompt and the harness.4 The **prompt** represents the linguistic payload delivered to the model.15 The **harness** is the surrounding deterministic application envelope that orchestrates state, handles input sanitation, manages execution constraints, and monitors the model's interaction with the external environment.4

```
*-----------------------------------------------------------------------------------+  
|                                PRODUCTION HARNESS                                 |  
|                                                                                   |  
|  *---------------------+   *---------------------+   *-------------------------+  |  
|  |   State Assembly    |   | Context Retrieval   |   |   Memory Selection      |  |  
|  | (User/Org Metadata) |   |    (Vector/RAG)     |   | (Dynamic Fact Eviction) |  |    
|  *---------------------+   *---------------------+   *-------------------------+  |  
|             │                         │                           │               |  
|             └─────────────────────────┼───────────────────────────┘               |  
|                                       ▼                                           |  
|  *-----------------------------------------------------------------------------+  |  
|  | Message Generation & Input Isolation                                        |  |  
|  | - System Instruction Role Injection (Platform vs. Developer Roles)          |  |  
|  | - Structural Delimiter Isolation (System vs. User vs. Tool Payloads)        |  |  
|  | - Untrusted Data Wrapping (XML/Data Separators)                             |  |  
|  *-----------------------------------------------------------------------------+  |  
|                                       │                                           |  
|                                       ▼                                           |  
|  *-----------------------------------------------------------------------------+  |  
|  | Inference Runtime & Sampling Constraints                                    |  |  
|  | - Token Budgets & Effort Calibration (max, xhigh, high, medium, low)        |  |  
|  | - Logit Masking & Constrained Decoding (XGrammar / Guidance)                |  |  
|  *-----------------------------------------------------------------------------+  |  
|                                       │                                           |  
|                                       ▼                                           |  
|  *-----------------------------------------------------------------------------+  |  
|  | Post-Inference Validation                                                   |  |  
|  | - Structured Parser Check & Regex Type Matching                             |  |  
|  | - Evaluator Judge Call & Retry Routing Policy                               |  |  
|  *-----------------------------------------------------------------------------+  |  
|                                       │                                           |  
|             ┌─────────────────────────┴───────────────────────────┐               |  
|             ▼                                                     ▼               |  
|  *---------------------+                                 *---------------------+  |  
|  |     SUCCESS PATH    |                                 |    FALLBACK PATH    |  |  
|  |  (State Sync/Save)  |                                 | (Graceful Refusal)  |  |  
|  *---------------------+                                 *---------------------+  |  
*-----------------------------------------------------------------------------------+
```

A production-grade harness manages several critical functions:

* **Message Generation and Input Isolation:** Programmatically assembling system, developer, user, and tool roles while wrapping untrusted content inside isolated structural tags.4 This ensures that user data or tool outputs cannot masquerade as high-privilege instructions.4  
* **Role and Delimiter Injection:** Appending developer-defined authority tags to the beginning of context packets, and injecting clear role separators (e.g., <user_input> or <retrieved_evidence>) dynamically during construction.4  
* **Context Construction and Caching Optimization:** Curating the input context block to maximize cache hit rates.18 For instance, placing highly static system instructions and template rules at the top of the context window allows API providers to leverage KV caching discounts, while dynamic inputs are appended at the end.6  
* **Inference Execution and Sampling Constraints:** Programmatically passing hyperparameters like temperature, top-p, token budgets, and effort configurations (e.g., setting Anthropic's effort parameter to xhigh or high to calibrate reasoning depth).6  
* **Post-Action Checks and Retry Routing:** Checking the returned payload against expected types, schemas, or semantic rules.11 If a validator fails, the harness catches the error, formats the violation, appends it as a correction prompt, and triggers an automated retry loop.4  
* **Graceful Refusal and Fallbacks:** Intercepting model failures, timeouts, rate-limit blocks, or unparseable outputs, and routing the system to pre-defined deterministic fallback paths to protect the user experience.4

## **High-Dimensional Control Surfaces**

### **Multi-Tier Instruction Hierarchies and Authority Resolution**

As large language models transition into autonomous, agentic roles, they must consume instructions from multiple heterogeneous sources: developer guidelines, direct user queries, external web page contexts, and dynamic tool schemas.19 When these instructions conflict—such as an untrusted webpage directing the model to ignore previous commands and delete a customer database—the model must possess an explicit priority resolution framework.19  
The modern industry paradigm relies on a clearly structured authority scale 8:

```
*-------------------------------------------------------------------+  
|               AUTHORITY LAYER HIGHER-PRIORITY SCALE               |  
|                                                                   |  
| [LEVEL 0] PLATFORM RULES (Safety boundaries set by provider)      |  
| [LEVEL 1] DEVELOPER RULES (Session house style & schemas)         |  
| [LEVEL 2] USER QUERIES (Direct human turn-level requests)         |  
| [LEVEL 3] DATA & TOOLS (Web scrape, API payloads, text files)     |  
*-------------------------------------------------------------------+
```

API providers like OpenAI enforce these boundaries at the API level.8 In their reasoning model lines (such as o1, o3, and gpt-5), the old system role has been deprecated in favor of the developer role.8 Under this architecture, the platform role sits at the absolute peak of authority, enforcing safety boundaries that cannot be overridden by anyone.8 The developer role acts as the second-highest authority level, defining the persistent task context, house styles, and operational rules for the session.8 The user role occupies the third-highest tier, while tool outputs, assistant messages, and uploaded files carry zero inherent authority and are treated as untrusted data.8  
However, real-world multi-user collaborative environments demand a more granular authority structure.19 Under the Many-Tier Instruction Hierarchy (ManyIH) framework, instructions are decoupled from simple role labels.19 Instead, privilege levels are dynamically specified as ordinal or scalar values at inference time, allowing up to 12 levels of distinct authority 19:  
P(Instruction_i) in the range  
Empirical tests on ManyIH-Bench show that even frontier models (such as GPT-5.4 and Claude Opus 4.6) are highly fragile when managing scaled instruction conflicts, achieving under 40% accuracy, and showing extreme sensitivity to how privilege information is formatted in the prompt.19  
To address these vulnerabilities structurally, systems can integrate **Instructional Segment Embedding (ISE)**.22 Derived from BERT’s classic segment architecture, ISE appends learnable segment embeddings directly to token representations within the network’s self-attention layers 22:  
h_i = Attention(W_Q * (e_i * s_i), W_K * (e_j * s_j), W_V * (e_j * s_j))  
where e_i represents the combined token and position embedding, and s_i is a learned segment embedding corresponding to its authority class (e.g., system, user, data, or output).22 This architectural separation prevents lower-priority tokens from overriding high-priority instructions, leading to up to an 18.68% increase in robustness against adversarial prompt injections.22  
At the post-training level, systems can deploy **SecAlign (Secure Alignment)**.13 Rather than relying on simple prompt reminders, SecAlign uses Direct Preference Optimization (DPO) to align model weights against adversarial instructions.13 It first constructs a specialized preference dataset containing prompt-injected inputs paired with a secure output (which follows the developer instruction) and an insecure output (which complies with the injected instruction).13 By preference-optimizing the model on this dataset, SecAlign reduces the attack success rate of sophisticated prompt injections to nearly 0% without degrading the model's core utility.13

### **Structured Output, Syntax Compliance, and Constrained Decoding**

When language models must deliver structured outputs (such as JSON payloads) to interface with downstream databases or APIs, prompt-only instructions (e.g., "respond only with valid JSON") are fundamentally insufficient.11 Under standard prompt-only execution, OpenAI benchmarks show that gpt-4o's syntax adherence scores below 40% on complex JSON schemas.11  
To guarantee 100% syntactical compliance, developers must deploy **constrained decoding** (also called structured sampling or guided generation).10 This technique integrates a deterministic constraint engine directly between the model's raw logit computation and the token selection step.10 Before any token is sampled, the constraint engine analyzes the token sequence generated so far and applies a binary token mask to filter the model's vocabulary 11:  
z_tilde_t = z_t * log(m_t)  
where z_t is the model's raw logit vector at step t, and m_t (where m_t is in {0, 1}^V) is the validation mask.11 This ensures that any token carrying a non-zero probability after masking is mathematically guaranteed to keep the output sequence on a structurally valid path defined by a context-free grammar or JSON schema.10  
Several specialized constrained decoding engines exist, displaying highly varied performance profiles:

| Framework | Core Mechanism | Strengths | Limits | Ideal Use Case |
| :---- | :---- | :---- | :---- | :---- |
| **XGrammar** 3 | Pushdown Automaton (PDA) batch decoding. 3 | Precomputes context-independent token validity; reduces mask time to <40 microseconds. Default backend for vLLM and SGLang. 11 | Does not support highly complex non-context-free structures. | High-throughput production deployments and dynamic per-request schemas.11 |
| **Guidance** 10 | Prefix trie traversal with regex derivatives. 10 | Average mask calculation <50 microseconds; fast-forwards deterministic tokens to bypass sampling entirely. 10 | Deeply integrated into Microsoft/llguidance Rust backend dependencies. 10 | Real-time interactive generation demanding extremely low latency.11 |
| **Outlines** 3 | Finite-State Machine (FSM) tracking at token level. 3 | Clean Python API; supports Pydantic models, regex, and EBNF grammars. 11 | Slow compilation overhead (3 to 12 second cold-start times on new schemas).11 | Applications with a small, static set of output schemas where compilation is a one-time cost.11 |
| **llama.cpp** 10 | GGML Backus-Naur Form (GBNF) tracking. 10 | Fully local execution; integrated directly with edge-device compile targets. 10 | Limited schema parsing speeds on resource-constrained embedded systems. | Embedded local deployments and offline edge device environments.11 |

According to the JSONSchemaBench study (January 2025), which evaluated frameworks across nearly 10,000 real-world schemas, modern engines like Guidance and XGrammar actually reduce per-token generation latency compared to unconstrained decoding (averaging 6–9ms per token versus 15–16ms for unconstrained baselines) because the engine's deterministic fast-forwarding bypasses model sampling entirely when the grammar allows only a single valid token pathway.11  
Despite these benefits, constrained decoding introduces critical cognitive trade-offs:

1. **Forced Fabrication:** If a schema enforces a strict required constraint (such as a non-nullable integer field) and the model does not possess the factual knowledge to answer the question, the constraint engine blocks the tokens required to say "I do not know".11 This forces the model to emit a highly plausible but completely fabricated value to satisfy the grammar.11  
2. **State-Space Attenuation:** Forcing a model to generate tokens that violate its natural probability distributions (such as forced JSON syntax characters) pushes the transformer's hidden state representations into out-of-distribution spaces.11 This attenuation degrades the semantic quality, coherence, and accuracy of any free-text blocks nested within the structured fields.11

### **Context Architecture, Retrieval, and the Usable Context Ceiling**

The choice between Retrieval-Augmented Generation (RAG) and ultra-long context windows represents a critical tension in context architecture.12

```
*-----------------------------------------------------------------------------------------+  
|                         CONTEXT PATHWAY COMPARISON MATRIX                               |  
|                                                                                         |  
|  RAG Path (Order-Preserving Selective Context):                                         |  
|  [Query] ──► ──► [Metadata Filter] ──► [In-Context Prompt] ──► [LLM]                    |  
|  * Latency: ~1.0 Second                                                                 |  
|  * Cost: ~$0.00008 per query                                                            |  
|  * Accuracy: Excellent local chunk recall; sidesteps middle-context degradation         |  
|                                                                                         |  
|  Long-Context Path (Full-Document Attention):                                           |  
|  [Query] ──────────────────────────► ───────────► [LLM]                                 |  
|  * Latency: 20 to 60+ Seconds                                                           |  
|  * Cost: ~$2.00 per query                                                               |  
|  * Accuracy: Captures global relationships; suffers from primacy/recency bias           |  
*-----------------------------------------------------------------------------------------+
```

While marketing claims celebrate context capacities of up to 2 million tokens, realistic production workloads reveal a distinct **usable context ceiling**.12 In multi-fact retrieval scenarios, average model recall drops to approximately 60% (a 40% miss rate), particularly when context blocks are semantically noisy or require multi-hop reasoning.12  
Furthermore, attention mechanisms suffer from the **"Lost-in-the-Middle" effect**, where model recall is highly position-dependent.12 Attention distributions follow a U-shaped curve: models attend robustly to information placed at the absolute beginning of the prompt (primacy bias) or the absolute end (recency bias), but show a performance drop of over 20 percentage points when the targeted facts reside in the middle of long contexts.12  
Databricks research across 13 major open-source and commercial LLMs demonstrated that RAG performance begins to degrade after 32,000 tokens for Llama-3.1-405B and after 64,000 tokens for GPT-4-0125-preview.12 While GPT-4o maintains relatively stable performance across wide context lengths, Claude-3.5-Sonnet experiences a sharp drop-off near its context boundaries (averaging 0.723 correctness at 8,000 tokens but falling to 0.706 at 125,000 tokens).12  
To resolve these trade-offs, advanced production environments utilize the **Self-Route** framework (presented at EMNLP 2024).12 Self-Route implements a dynamic routing layer based on the model’s self-reflection.12 It analyzes incoming queries to determine whether a task represents a localized factual lookup or requires global synthesis across the entire corpus.12 Simple queries are routed to low-latency, low-cost RAG pipelines, while complex tasks requiring global document understanding are routed to long-context pathways.12  
This routing mechanism optimizes the overall cost profile: RAG queries cost roughly 0.00008 dollars per query, whereas a 1-million-token long-context call costs 2.00 dollars in input tokens alone—making pure long-context execution roughly 1,250x more expensive per query.12

### **State Synchronization and Agentic Memory Systems**

Because language models are stateless by design, maintaining personalization and context across multi-session interactions requires an external state-synchronization layer.2 A production-grade **AI memory system** categorizes and manages user state across four distinct architectural layers 2:

* **Working Memory:** The immediate context window loaded into active attention during an inference call.2 It has zero persistence, resets at the end of the session, and represents the most expensive form of state by token cost.2  
* **Episodic Memory:** Logged records of prior user-assistant interactions across multiple sessions.2 It captures conversational history, such as remembering a specific architectural design pattern discussed in a previous work session.2  
* **Semantic Memory:** The agent's structured world-knowledge layer, storing factual details, entities, preferences, business definitions, and policies.2  
* **Procedural Memory:** The model’s library of learned workflows, repeatable tool-use schemas, and specific execution habits.2

To maintain state integrity, platforms like MemMachine combine short-term episodic streams with long-term profile memories.32 By indexing episodes at the sentence level and using the model only for high-level abstraction rather than constant raw text extraction, MemMachine reduces memory-related token overhead by up to 80%.32  
When managing memory at scale, systems like Mem0 support scoping dimensions such as user_id (individual user history), run_id (session-specific threads), agent_id (subagent contexts), and org_id (shared corporate policies).31 To prevent stale, outdated, or low-relevance facts from polluting the active context window, memory systems must implement explicit decay and eviction mechanics 2:  
Memory Weight(t) = Base Similarity Score * e^(-lambda * (t - t_last))  
where lambda represents the temporal decay rate, and (t - t_last) is the duration since the memory was last retrieved.31 Underperforming or stale memory chunks are systematically evicted, keeping the most relevant context at the top of the retrieval stack.2

## **The Intervention Ladder and Adaptation Choice Framework**

### **The Intervention Ladder**

When correcting behavioral errors or enforcing constraints in a production system, engineers must climb an explicit **Intervention Ladder**. Systems should be designed to execute the lightest, most reversible, and most maintainable change first before committing resources to heavy weight-adaptation or training cycles.4

```       
          ▲  
          │   10. Model Distillation   
          │    9. Preference Tuning (DPO / SecAlign)   
          │    8. Parameter Adaptation (LoRA / SFT) [5, 33]  
          │    7. Model-Level Self-Routing   
          │    6. Constrained Decoding (XGrammar / Guidance)   
          │    5. Tool Contracts & State Validation Loops   
          │    4. Context Architecture, Memory, & RAG Pipes   
          │    3. Harness-Level Role/Delimiter Assembly   
          │    2. Few-Shot Example & Output Frame Tuning   
          │    1. Semantic & Instruction-Level Editing   
```
      

### **Adaptation Choice Matrix**

The architectural trade-offs of each steering technique must be calculated systematically before committing engineering resources:

| Technique | Behavior Depth | Ideal Use Case | Iteration Speed | Data Quality | Deployment Complexity | Cost Profile | Latency Impact | Auditability | Regression Risk |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Semantic Prompts** 6 | Surface | Prototyping, tone changes, quick formatting rules.6 | Minutes | None | Extremely Low | Negligible | Near-Zero | Direct | Very Low |
| **Few-Shot Examples** 6 | Moderate | Explaining nuanced formatting or domain styles.6 | Minutes | Low (3-5 verified examples).6 | Low | Minor token overhead | Linear with token count.6 | Direct | Low |
| **System Rules** 8 | High (Behavioral) | Setting persistent policies, safety guardrails.18 | Minutes | None | Low | Minor token overhead | Near-Zero | Direct | Low |
| **Structured Output** 11 | Strict (Syntactic) | Database ingestion, API call generation.11 | Seconds | Schema definitions | Medium | None | Negative (Faster per-token).11 | High | Medium (Hallucination risk).11 |
| **RAG Pipes** 34 | Dynamic Factual | Ingesting volatile documents, search queries.35 | Hours | Document chunk validation.34 | High | Storage & Vector Database costs.35 | Retrieval step (~1s delay).12 | Complete trace | Low |
| **Long Context** 12 | Holistic Synthesis | Codebase audits, multi-hop document analysis.12 | Minutes | None | Medium | High (quadratic token cost).12 | Quadratic scaling latency.12 | Poor (Opaque attention) | Low |
| **Dynamic Memory** 31 | Historical | Multi-session personalization, chat continuity.2 | Seconds | Dynamic entity schemas | High | DB write & storage costs.2 | Minimal DB lookup | Complete trace | Low |
| **Tool Use** 36 | Interactive | Dynamic actions, executing external calculations.17 | Minutes | Schema design | High | Minor token overhead | Execution dependent | High | Low |
| **Model Routing** 12 | Structural | Balancing cost, speed, and accuracy dynamically.12 | Hours | Classification labels | High | Optimizes overall costs.12 | Negligible | Complete trace | Low |
| **Fine-Tuning (SFT)** 5 | Deep | Formatting habits, formatting speed, style.5 | Days/Weeks | High (Thousands of clean pairs) | High | Upfront training compute.5 | None | Opaque | High (Forgetting) 5 |
| **LoRA/QLoRA** 5 | Deep (Localized) | Adapting small models to run custom tasks.5 | Days/Weeks | High (Clean domain samples) | High | Low-cost training runs.5 | None | Opaque | High (Utility regression) |
| **Preference Tuning** 13 | High (Security) | Preventing jailbreaks, security defense.13 | Weeks | Extremely High (Pairs).13 | High | Upfront training compute | None | Opaque | High (Over-refusal) 37 |
| **Synthetic Gen** 33 | Domain Alignment | Creating high-volume training samples.33 | Days | High (Verification pipeline) | High | Generation compute costs | None | High | Medium |
| **Distillation** 5 | Downscaling | Porting logic from frontier models to edge LLMs.5 | Weeks | Extremely High (Teacher logs) | High | Heavy training investment | Negative (Runs faster) | Opaque | Extremely High |

### **Facts, Behaviors, and Parameters: Directing the Optimization Path**

A foundational error in AI systems architecture is using the wrong tool to address a behavioral or factual shortfall.5 Developers must understand the distinct boundaries of these layers:

1. **Dynamic/Volatile Facts:** Facts, proprietary customer documents, fast-changing database records, and transaction histories belong entirely in **RAG, tools, or memory layers**.2 Retraining weights to inject volatile facts is economically unsustainable and structurally fragile.5  
2. **Stable Behaviors/Formatting:** Tone, formatting habits, stylistic preferences, secure boundaries, and parsing patterns are best handled at the **weight-adaptation layer (LoRA/SFT)** once semantic prompts or structured decoding reach their limits.5  
3. **The RAFT Synthesis:** To optimize performance, systems can deploy Retrieval-Augmented Fine-Tuning (RAFT).5 Rather than using fine-tuning to memorize facts, RAFT trains model weights specifically *how to read and cite* retrieved documents, teaching the model to ignore distractor chunks and formulate structured reasoning paths.5

## **Conflict Mapping and Architectural Tensions**

System design in model steering is defined by trade-offs where competing architectures present distinct, mutually exclusive advantages:

* **Long Context vs. Retrieval (RAG):** RAG is highly cost-effective and provides low-latency execution with an auditable document trail.29 However, it fails at global document analysis because semantic chunking breaks relationships between disparate pages.12 Long context captures holistic document synthesis but introduces high token costs and slow latency profiles, and is subject to attention degradation near context centers.12 *Resolution:* Deploy Self-Routing to dynamically delegate factual queries to RAG, while routing global synthesis tasks to long context.12  
* **RAG vs. Fine-Tuning:** RAG acts as the dynamic information-access layer, while fine-tuning alters behavioral and reasoning patterns.5 Fine-tuning alone fails to inject dynamic knowledge reliably, yielding only a 19% accuracy rate on direct factual updates.5 *Resolution:* Treat them as complementary. Use RAG to supply real-time context and fine-tune model parameters using RAFT to optimize document processing and citation habits.5  
* **Promptcraft vs. Harness Engineering:** Naive prompting relies on the model to self-regulate, parse its own outputs, and enforce security.7 Harness engineering wraps the model in programmatic isolation, handling state synchronization, schema validation, and security at the application layer.4 *Resolution:* Enforce safety, schema compliance, and database execution deterministically in harness code; reserve prompt editing for stylistic refinement.4  
* **Agents vs. Bounded Workflows:** Agentic models run with loose, open-ended system loops and self-directed tools, maximizing task flexibility but risking execution loops and unpredictability.4 Bounded workflows enforce strict, step-by-step state-machine execution.4 *Resolution:* For customer-facing, high-risk systems, enforce bounded workflows; for internal diagnostic, coding, or data synthesis pipelines, utilize agentic thinking at high effort settings.6  
* **Provider-Native Tools vs. Framework-Mediated Systems:** Provider-native tool-calling features are optimized directly at the model weight level, achieving low latency and high accuracy.36 Framework-mediated tools (such as LangChain or custom orchestrators) support provider-agnostic abstraction but introduce execution overhead and prompt formatting sensitivity.39 *Resolution:* Default to provider-native tool APIs to maximize stability, utilizing lightweight application-level adapters to preserve vendor portability.36  
* **Structured Output vs. Free-Form Generation:** Structured output guarantees 100% syntactic conformity for database integration.11 Free-form generation allows the model’s attention heads to navigate natural token distributions, yielding higher reasoning quality and detail.11 *Resolution:* Run reasoning-intensive steps in unconstrained free-form prose, and pipe the final synthesized answer through a secondary, highly constrained schema parser.11  
* **LLM-as-Judge vs. Human Review:** LLM-as-judge pipelines scale to run millions of evaluations at minimal cost.40 Human review remains the absolute standard for clinical, legal, or high-stakes validation but scales poorly and is vulnerable to evaluator fatigue.40 *Resolution:* Use domain experts to validate and calibrate evaluation rubrics and establish verified golden reference datasets, then deploy grounded LLM judges to handle scaled regression testing.40  
* **Model Routing vs. Single-Model Standardization:** Single-model standardization simplifies the CI/CD pipeline and codebase, but locks the system into a single vendor's cost and performance profile.12 Model routing optimizes performance, cost, and latency dynamically but increases deployment complexity.12 *Resolution:* Standardize on a single frontier model for complex core operations, and implement a lightweight router to offload trivial classification or text-processing sub-tasks to low-cost local models.  
* **Explicit Memory vs. Conversation-History Replay:** Replaying raw conversational history provides absolute factual reconstruction but causes token inflation and context-window clogging.2 Explicit memory platforms extract, summarize, and decay state continuously.31 *Resolution:* Deploy episodic memory with dynamic decay and fact eviction to maintain long-term personalization, while using raw replay only for immediate, high-context debugging sessions.2

## **Failure Diagnostics by Steering Layer**

When a production system fails, identifying which layer of the steering architecture owns the diagnostic signature is critical to applying the smallest effective corrective action.

```
+-----------------------------------------------------------------------------------------+
|                              FAILURE CASCADE FLOW CHART                                 |
|                                                                                         |
|  Production failure appears. Diagnose the failure signature before changing the model.  |
|                                                                                         |
|                         +-----------------------------+                                 |
|                         |   OBSERVED FAILURE MODE     |                                 |
|                         +--------------+--------------+                                 |
|                                        |                                                |
|          +-----------------------------+-----------------------------+                  |
|          |                             |                             |                  |
|          v                             v                             v                  |
|  +-------------------+       +-------------------+       +---------------------------+  |
|  | STRUCTURE FAILURE |       | KNOWLEDGE FAILURE |       | AUTHORITY / SAFETY FAILURE|  |
|  |                   |       |                   |       |                           |  |
|  | Invalid JSON,     |       | Hallucinations,   |       | Prompt injection,         |  |
|  | schema mismatch,  |       | stale facts,      |       | jailbreaks, tool/data     |  |
|  | parser rejection  |       | missing evidence  |       | instructions overriding   |  |
|  |                   |       |                   |       | trusted instructions      |  |
|  +---------+---------+       +---------+---------+       +-------------+-------------+  |
|            |                           |                               |                |
|            v                           v                               v                |
|  DO NOT edit prompt          DO NOT fine-tune              DO NOT rely on prompt        |
|  semantics endlessly.        parameters to memorize        safety warnings alone.       |
|                              volatile facts.                                            |
|            |                           |                               |                |
|            v                           v                               v                |
|  FIX via constrained         FIX via RAG, metadata         FIX via instruction          |
|  decoding, strict schema     filters, context refresh,     hierarchy, privilege         |
|  validation, nullable        document deduplication,       separation, SecAlign,        |
|  fields, and parser          or retrieval repair.          ISE, and harness-level       |
|  retry handling.                                           isolation.                   |
|                                                                                         |
+-----------------------------------------------------------------------------------------+
| Core rule: route the symptom to the lightest robust layer capable of fixing it.         |
+-----------------------------------------------------------------------------------------+
```


| Steering Layer | Production Failure Symptom | Likely Root Cause | Diagnostic Observability Signal | Smallest Effective Correction |
| :---- | :---- | :---- | :---- | :---- |
| **Semantic** 6 | Robotic, repetitive outputs; excessive markdown formatting.6 | Mismatched prompt style, or reliance on negative constraints (e.g. "Do not write lists").6 | Unusually short token distributions, high frequency of bullet-point characters in outputs.6 | Rewrite instructions using positive framing; implement clear, structural XML formatting wrappers.6 |
| **Harness** 4 | Model mixes separate customer data profiles or context details.4 | Direct string concatenation of dynamic variables; lack of message role isolation.4 | Overlapping context tags in application traces, parameter collision errors. | Programmatically isolate dynamic context from baseline developer instructions using message API roles.4 |
| **Context** 4 | The model systematically ignores primary safety rules or templates.12 | Place high-priority rules in the middle of long prompts, triggering middle-context decay.12 | Spikes in instruction violation rates on payloads exceeding 32,000 tokens.12 | Reposition primary instructions and safety rules to the absolute bottom of the input payload.6 |
| **Retrieval** 34 | The model answers queries using outdated or duplicate knowledge blocks.2 | Vector store pollution; failure to run data deduplication or ingest document metadata.2 | High cosine similarity scores returned for irrelevant or superseded document chunks.2 | Implement mandatory document-date metadata filters at the query retrieval layer.34 |
| **Memory** 31 | Assistant insists on using stale user preferences from past weeks.31 | Absence of temporal decay formulas; failure to evict low-relevance memory facts.2 | Memory size accumulation in database logs, drop-off in user personalization metrics.31 | Apply a temporal decay weight to memory retrieval, forcing older matching facts to evict.31 |
| **Tool** 36 | Model enters infinite execution loops or generates unparseable arguments.7 | Loose, ambiguous function schemas; absence of hard limits on harness execution loops.4 | High rates of HTTP 400 errors from target APIs, execution timeout exceptions.7 | Restrict schema parameters to strict typing, and implement a hard execution cap (e.g. max 3 iterations).4 |
| **Decoding** 11 | Syntactically correct JSON blocks contain invalid values or fabrications.11 | Strict schema constraint applied to a field where the model lacked factual context.11 | Logit probability distributions compressed to highly unnatural token selections.11 | Redesign the JSON schema to make fields nullable, allowing the model to return "null" when uncertain.11 |
| **Routing** 12 | Complex coding or multi-hop synthesis queries return flat, superficial lookups.12 | Dynamic router incorrectly classified a global synthesis query as a simple factual lookup.12 | Slower resolution times on complex tasks, high rates of manual query restarts. | Optimize the router's classification instructions to flag implicit, open-ended, and comparative queries.12 |
| **Adaptation** 5 | Model exhibits extreme over-refusal, or forgets basic formatting habits.5 | Catastrophic forgetting from excessive preference optimization or poorly curated SFT pairs.5 | Drop-off in standard model benchmark scores (e.g. AlpacaEval win-rate).22 | Re-introduce a baseline instruction-following corpus (e.g., Alpaca cleaned sets) into the training mix.26 |
| **Evaluation** 41 | System regressions pass unnoticed, or evaluations show highly inconsistent scores.40 | LLM evaluation judges grading correctness without human-grounded reference responses.41 | Zero correlation between automated LLM judge scores and human verification logs.41 | Ground the evaluator judge prompt with a verified, human-written reference answer.41 |

## **Production System Patterns and Anti-Patterns**

### **Architectural Habits**

* **XML Schema Isolation:** Programmatically separating all dynamic prompt variables, few-shot examples, retrieved document chunks, and untrusted contents using explicit XML tags (e.g., <user_query> or <context_document index="0">).4 This provides clean structural boundaries and prevents context pollution.4  
* **Logit-Level Syntax Enforcement:** Resolving all syntactical and structure-compliance challenges at the decoding layer using high-performance engines like XGrammar or Guidance, freeing up model parameter space for semantic logic.10  
* **Grounded Evaluation Loops:** Calibrating all automated evaluation pipelines by injecting verified, human-written gold references into the judge model prompts, neutralizing self-preference and stylistic evaluation biases.41  
* **Parallel Execution Pipelines:** Designing system prompts and harnesses to exploit parallel tool-calling capabilities (e.g., executing multiple independent document-reading calls simultaneously) to minimize time-to-first-token.6  
* **Dynamic State Eviction:** Integrating structured memory systems that apply temporal decay and deduplication to conversational history, keeping irrelevant or expired facts out of active context windows.2

### **Failure-Prone Habits**

* **The Infinite Semantic Polish:** Attempting to resolve syntax formatting crashes, tool parameter errors, or database injection vulnerabilities by continually editing the wording of natural language instructions.4  
* **Parametric Fact Overloading:** Using Supervised Fine-Tuning or weight adjustment to "teach" a model fast-changing factual knowledge, which results in catastrophic forgetting and factual interference.5  
* **Promiscuous Context Ingestion:** Concatenating untrusted user inputs or raw web scrapes directly into system instruction blocks without wrapping them in low-privilege delimiters, exposing the application to prompt injections.4  
* **Unbounded Retry Loops:** Implementing basic, catch-all retry loops that re-prompt the model with the exact same input on failure, leading to infinite loops, high API bills, and unhandled runtime crashes.4  
* **Opaque Evaluation Trust:** Relying on uncalibrated, ungrounded "LLM-as-a-judge" pipelines to validate production changes, leading to false-positive passes caused by self-recognition and length-bias artifacts.40

## **System Evaluation and Regression Discipline**

Evaluating a multi-layered model steering system requires isolating semantic variables from harness, retrieval, and decoding layers.4 A professional evaluation architecture must be constructed across five distinct core pillars:

```
*-------------------------------------------------------------------------+  
|                  FIVE PILLARS OF SYSTEM EVALUATION                      |  
|                                                                         |  
|  1. GOLDEN REFERENCE SETS ──► Ground truth human-verified baselines     |  
|                                                                         |  
|  2. REFERENCE GROUNDING   ──► Eliminates self-bias in LLM Judges        |  
|                                                                         |  
|  3. SYNTAX COMPLIANCE     ──► Verifies schema validity of outputs       |  
|                                                                         |  
|  4. CITATION AUDITING     ──► Enforces factual grounding via RAG        |  
|                                                                         |  
|  5. TELEMETRY MONITORING  ──► Tracks runtime performance metrics        |  
|                                                                         |  
*-------------------------------------------------------------------------+
```

### **1. Golden Reference Sets**

System testing must be anchored on a static, high-diversity golden evaluation dataset of at least 50 to 100 domain-specific scenarios.4 This dataset must contain expert-annotated ground-truth outputs, edge cases, and safety probes to establish baseline operational compliance.4

### **2. Reference Grounding for Evaluator Judges**

When deploying LLM-as-a-judge pipelines to evaluate outputs at scale, evaluations must use human-grounded reference responses.40 Research reveals that LLM judges exhibit significant biases, including self-preference (favoring generations from their own model family) and stylistic bias (rating assertive but incorrect answers 15% to 20% higher than accurate, cautious responses).40  
Crucially, without an expert reference, a judge model's grading accuracy degrades on questions it cannot correctly answer itself.41 Providing the judge with a verified human-written reference answer resolves this discrepancy, enabling high agreement with human annotators.41 Agreement rates must be validated using chance-corrected metrics like Cohen’s Kappa (Kappa):  
Kappa = (p_o - p_e) / (1 - p_e)  
where p_o represents observed agreement and p_e represents expected agreement under random conditions.45

### **3. Syntax Compliance**

Outputs generated via constrained decoding must be validated against downstream schemas (e.g., Pydantic or OpenAPI models).11 The evaluation harness must log schema violation rates, parsing errors, and logit compression latency.4

### **4. Citation Auditing**

For RAG workloads, evaluations must ensure the model's generations are factually grounded in retrieved sources.12 The evaluation harness must parse citations, verify that cited substrings exist in the retrieved context documents, and run semantic overlap validations.12

### **5. Telemetry and Regression Gates**

In production, systems must log token usage, API latency, cache hit rates, cost per query, and user feedback signals.4 Any updates to the semantic prompts or harness assembly must pass through automated regression gates, comparing candidate performance against current production baselines before deployment.4

## **Cross-Canon Handoff Map**

This report establishes the baseline concepts of model steering, serving as the conceptual foundation for downstream reports in the AI Engineering Systems Canon:

| Downstream Canon Report | Direct Dependency | Operational Integration |
| :---- | :---- | :---- |
| **AI-ENG-B: Context Architecture** | Prompt delimiters, role hierarchy, and token caching structures.4 | Defines how the context compiler packs, orders, and structures multi-source input payloads to maximize cache hit rates.6 |
| **AI-ENG-C: Inference Economics** | RAG vs. long-context costs and token budgets.12 | Integrates token-count estimations and self-routing mechanics to optimize GPU hardware allocation.12 |
| **AI-ENG-D through F: Corpus Engineering & RAG** | Retrieval wrappers, semantic chunking, and metadata parsing.34 | Establishes the interface between the vector database indexing pipelines and the harness assembly layer.4 |
| **AI-ENG-H: Model Adaptation** | Weight modification, SFT, LoRA datasets, and DPO alignment.5 | Guides the generation of clean input-output pairs to run supervised fine-tuning and secure preference optimization.5 |
| **AI-ENG-N: Tool Contracts** | Function schemas, JSON structures, parallel calling, and error handling.36 | Enforces structured API types and manages deterministic execution loops around agent workflows.4 |
| **AI-ENG-S: Production Pathologies** | State-space attenuation, forced fabrication, and attention decay.11 | Analyzes and mitigates silent systemic regressions, such as logit trapping and lost-in-the-middle degradation.11 |
| **AI-ENG-Z: Telemetry & Tracing** | Trace schemas, token tracking, and logging.4 | Implements distributed tracing across tool execution limits, prompt variables, and retry sequences.4 |
| **AI-ENG-AA: Evaluations** | Golden reference sets, Cohen's Kappa, and LLM-as-judge calibration.41 | Operates automated regression testing gates, validating semantic adjustments against verified human references.41 |

## **Durable Principles for Model Steering**

1. **Enforce Separation of Concerns:** Solve syntax, formatting, and database integration rules at the harness and decoding layers; reserve semantic prompt adjustments for tone, style, and task-context definition.4  
2. **Route by Data Volatility:** Address fast-changing or volatile facts entirely through retrieval-augmented pipelines; reserve weight adaptation (fine-tuning) for stable behaviors, domain-specific habits, and safety alignments.5  
3. **Default to Platform Defenses:** Never trust natural language prompts to secure an application. Enforce safety, access control, and privilege boundaries programmatically using strict, deterministic application-layer security systems.7  
4. **Prioritize Ground Truth in Evaluations:** Never trust evaluation judges that evaluate complex answers without a human-verified reference. Calibrate LLM judges continuously against grounded human standards.41  
5. **Calculate the Total Cost of Control:** Choose the lightest steering intervention that satisfies behavioral requirements.29 Factor compilation overheads, latency scales, caching mechanics, and maintenance debt into every architectural decision.4

#### **Works cited**

1. Difference between the roles used in the OpenAI API, accessed June 5, 2026, [https://community.openai.com/t/difference-between-the-roles-used-in-the-openai-api/1262886](https://community.openai.com/t/difference-between-the-roles-used-in-the-openai-api/1262886)  
2. AI Memory System: Types, Architecture, and Enterprise Use Cases, accessed June 5, 2026, [https://atlan.com/know/ai-memory-system/](https://atlan.com/know/ai-memory-system/)  
3. Structured Decoding in vLLM: a gentle introduction, accessed June 5, 2026, [https://vllm.ai/blog/2025-01-14-struct-decode-intro](https://vllm.ai/blog/2025-01-14-struct-decode-intro)  
4. Applying Anthropic's Prompt Guide: Practical Insights for Claude - PromptLayer Blog, accessed June 5, 2026, [https://blog.promptlayer.com/how-to-apply-anthropic-s-prompt-guide/](https://blog.promptlayer.com/how-to-apply-anthropic-s-prompt-guide/)  
5. RAG vs Fine-Tuning: A 2026 Decision Framework | Zartis, accessed June 5, 2026, [https://www.zartis.com/rag-vs-fine-tuning-a-2026-decision-framework/](https://www.zartis.com/rag-vs-fine-tuning-a-2026-decision-framework/)  
6. Prompting best practices - Claude API Docs - Claude Console, accessed June 5, 2026, [https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)  
7. Evaluation of Prompt Injection Defenses in Large Language Models - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2604.23887v1](https://arxiv.org/html/2604.23887v1)  
8. OpenAI Developer Role | Aurelio AI, accessed June 5, 2026, [https://www.aurelio.ai/reference/openai-developer-role](https://www.aurelio.ai/reference/openai-developer-role)  
9. Instruction Hierarchy in LLMs - Ylang Labs, accessed June 5, 2026, [https://ylanglabs.com/blogs/instruction-hierarchy-in-llms](https://ylanglabs.com/blogs/instruction-hierarchy-in-llms)  
10. guidance-ai/llguidance: Super-fast Structured Outputs - GitHub, accessed June 5, 2026, [https://github.com/guidance-ai/llguidance](https://github.com/guidance-ai/llguidance)  
11. Grammar-Constrained Generation: The Output Reliability Technique ..., accessed June 5, 2026, [https://tianpan.co/blog/2026-04-16-grammar-constrained-generation-output-reliability](https://tianpan.co/blog/2026-04-16-grammar-constrained-generation-output-reliability)  
12. Long-Context Models vs. RAG: When the 1M-Token Window Is the ..., accessed June 5, 2026, [https://tianpan.co/blog/2026-04-09-long-context-vs-rag-production-decision-framework](https://tianpan.co/blog/2026-04-09-long-context-vs-rag-production-decision-framework)  
13. SecAlign: Defending Against Prompt Injection with Preference Optimization - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2410.05451v2](https://arxiv.org/html/2410.05451v2)  
14. [PDF] SecAlign: Defending Against Prompt Injection with Preference Optimization, accessed June 5, 2026, [https://www.semanticscholar.org/paper/SecAlign%3A-Defending-Against-Prompt-Injection-with-Chen-Zharmagambetov/3e1d812a1ef4b02b7d60020cc8aaa596d28373da](https://www.semanticscholar.org/paper/SecAlign%3A-Defending-Against-Prompt-Injection-with-Chen-Zharmagambetov/3e1d812a1ef4b02b7d60020cc8aaa596d28373da)  
15. Claude System Prompt Explained: What's Inside and Why It Matters - Tactiq, accessed June 5, 2026, [https://tactiq.io/learn/claude-system-prompt](https://tactiq.io/learn/claude-system-prompt)  
16. What goes in the system vs developer role - API, accessed June 5, 2026, [https://community.openai.com/t/what-goes-in-the-system-vs-developer-role/1347594](https://community.openai.com/t/what-goes-in-the-system-vs-developer-role/1347594)  
17. How to Interact with APIs Using Function Calling in Gemini | Google Codelabs, accessed June 5, 2026, [https://codelabs.developers.google.com/codelabs/gemini-function-calling](https://codelabs.developers.google.com/codelabs/gemini-function-calling)  
18. System and Developer Roles in messages and Instructions in Responses.Create? - API, accessed June 5, 2026, [https://community.openai.com/t/system-and-developer-roles-in-messages-and-instructions-in-responses-create/1370516](https://community.openai.com/t/system-and-developer-roles-in-messages-and-instructions-in-responses-create/1370516)  
19. Many-Tier Instruction Hierarchy in LLM Agents - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2604.09443v2](https://arxiv.org/html/2604.09443v2)  
20. Mitigating the risk of prompt injections in browser use - Anthropic, accessed June 5, 2026, [https://www.anthropic.com/research/prompt-injection-defenses](https://www.anthropic.com/research/prompt-injection-defenses)  
21. Many-Tier Instruction Hierarchy in LLM Agents - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2604.09443v3](https://arxiv.org/html/2604.09443v3)  
22. Instructional Segment Embedding: Improving LLM Safety with Instruction Hierarchy - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2410.09102v1](https://arxiv.org/html/2410.09102v1)  
23. Instructional Segment Embedding: Improving LLM Safety with Instruction Hierarchy | Request PDF - ResearchGate, accessed June 5, 2026, [https://www.researchgate.net/publication/384929764_Instructional_Segment_Embedding_Improving_LLM_Safety_with_Instruction_Hierarchy](https://www.researchgate.net/publication/384929764_Instructional_Segment_Embedding_Improving_LLM_Safety_with_Instruction_Hierarchy)  
24. Instructional Segment Embedding: Improving LLM Safety with Instruction Hierarchy - GitHub, accessed June 5, 2026, [https://github.com/tongwu2020/ISE](https://github.com/tongwu2020/ISE)  
25. Instructional Segment Embedding: Improving LLM Safety with Instruction Hierarchy - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2410.09102v2](https://arxiv.org/html/2410.09102v2)  
26. SecAlign: Defending Against Prompt Injection with Preference Optimization - Sizhe Chen, accessed June 5, 2026, [https://sizhe-chen.github.io/SecAlign-Website/](https://sizhe-chen.github.io/SecAlign-Website/)  
27. Generating Structured Outputs from Language Models: Benchmark and Studies - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2501.10868v1](https://arxiv.org/html/2501.10868v1)  
28. Structured Outputs - SGLang Documentation, accessed June 5, 2026, [https://sgl-project.github.io/advanced_features/structured_outputs.html](https://sgl-project.github.io/advanced_features/structured_outputs.html)  
29. RAG vs. long-context LLMs: A side-by-side comparison | Meilisearch, accessed June 5, 2026, [https://www.meilisearch.com/blog/rag-vs-long-context-llms](https://www.meilisearch.com/blog/rag-vs-long-context-llms)  
30. RAG vs Large Context Window: Real Trade-offs for AI Apps - Redis, accessed June 5, 2026, [https://redis.io/blog/rag-vs-large-context-window-ai-apps/](https://redis.io/blog/rag-vs-large-context-window-ai-apps/)  
31. AI Memory Management for LLMs and Agents - Mem0, accessed June 5, 2026, [https://mem0.ai/blog/ai-memory-management-for-llms-and-agents](https://mem0.ai/blog/ai-memory-management-for-llms-and-agents)  
32. MemMachine: A Ground-Truth-Preserving Memory System for Personalized AI Agents, accessed June 5, 2026, [https://arxiv.org/html/2604.04853v1](https://arxiv.org/html/2604.04853v1)  
33. RAG vs Fine-tuning: Pipelines, Tradeoffs, and a Case Study on Agriculture - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2401.08406v2](https://arxiv.org/html/2401.08406v2)  
34. RAG vs. Fine-tuning - IBM, accessed June 5, 2026, [https://www.ibm.com/think/topics/rag-vs-fine-tuning](https://www.ibm.com/think/topics/rag-vs-fine-tuning)  
35. Should You Use RAG or Fine-Tune Your LLM? - Actian Corporation, accessed June 5, 2026, [https://www.actian.com/blog/databases/should-you-use-rag-or-fine-tune-your-llm/](https://www.actian.com/blog/databases/should-you-use-rag-or-fine-tune-your-llm/)  
36. Using Tools with Gemini API | Google AI for Developers, accessed June 5, 2026, [https://ai.google.dev/gemini-api/docs/tools](https://ai.google.dev/gemini-api/docs/tools)  
37. The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions - GitHub, accessed June 5, 2026, [https://github.com/AIResponsibly/PaperSummaries/blob/main/summaries/safety/instruction_hierarchy_llm.md](https://github.com/AIResponsibly/PaperSummaries/blob/main/summaries/safety/instruction_hierarchy_llm.md)  
38. RAG vs Long-Context LLMs: A Comprehensive Comparison | by Rost Glukhov | Medium, accessed June 5, 2026, [https://medium.com/@rosgluk/rag-vs-long-context-llms-a-comprehensive-comparison-9b30594c445e](https://medium.com/@rosgluk/rag-vs-long-context-llms-a-comprehensive-comparison-9b30594c445e)  
39. Gemini | Mendix Documentation, accessed June 5, 2026, [https://docs.mendix.com/agents/reference-guide/external-connectors/gemini/](https://docs.mendix.com/agents/reference-guide/external-connectors/gemini/)  
40. LLM-as-a-Judge vs Human Evaluation - Galileo AI, accessed June 5, 2026, [https://galileo.ai/blog/llm-as-a-judge-vs-human-evaluation](https://galileo.ai/blog/llm-as-a-judge-vs-human-evaluation)  
41. No Free Labels: Limitations of LLM-as-a-Judge Without Human Grounding - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2503.05061v2](https://arxiv.org/html/2503.05061v2)  
42. Human Evaluation of Large Language Models: A Review and Protocol Selection Framework, accessed June 5, 2026, [https://www.mdpi.com/2673-2688/7/5/174](https://www.mdpi.com/2673-2688/7/5/174)  
43. Repo for the research paper "SecAlign: Defending Against Prompt Injection with Preference Optimization" - GitHub, accessed June 5, 2026, [https://github.com/facebookresearch/SecAlign](https://github.com/facebookresearch/SecAlign)  
44. No Free Labels: Limitations of LLM-as-a-Judge Without Human Grounding - arXiv, accessed June 5, 2026, [https://arxiv.org/html/2503.05061v1](https://arxiv.org/html/2503.05061v1)  
45. [Literature Review] No Free Labels: Limitations of LLM-as-a-Judge Without Human Grounding, accessed June 5, 2026, [https://www.themoonlight.io/en/review/no-free-labels-limitations-of-llm-as-a-judge-without-human-grounding](https://www.themoonlight.io/en/review/no-free-labels-limitations-of-llm-as-a-judge-without-human-grounding)  
46. Judge's Verdict: A Comprehensive Analysis of LLM Judge Capability Through Human Agreement | OpenReview, accessed June 5, 2026, [https://openreview.net/forum?id=jVyUlri4Rw](https://openreview.net/forum?id=jVyUlri4Rw)

---

[← Back to Canon Map](../canon-map.md)