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