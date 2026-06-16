# Stunspot’s Guide to AI Systems

**The AI Engineering Systems Canon**  
*A comprehensive field manual for practical AI systems design.*

*Stunspot’s Guide to AI Systems* is a structured field manual for designing, steering, evaluating, operating, and governing modern AI systems. It treats AI engineering as the design of probabilistic cognitive infrastructure inside deterministic operational systems.
## Start Here

- [Volume 1 — The Informational/Epistemic Layer](./volume-01/)
- [Volume 2 — Knowledge, Data, and Corpus Engineering](./volume-02/)
- [Volume 3 — Model Lifecycle and Adaptation](./volume-03/)
- [Volume 4 — Runtime Architecture and Inference Mechanics](./volume-04/)
- [Volume 5 — Agentic Systems and Tool-Using Architectures](./volume-05/)
- [Volume 6 — Multimodal and Interface-Controlling Systems](./volume-06/)
- [Volume 7 — Failure, Security, and Hostile Environments](./volume-07/)
- [Volume 8 — Resilience, Degraded Modes, and Human Trust](./volume-08/)
- [Volume 9 — Observability, Evaluation, and Verification](./volume-09/)
- [Volume 10 — Operations, Governance, and Lifecycle Management](./volume-10/)
- [Volume 11 — Product, Business, and Organizational Architecture](./volume-11/)
- [Volume 12 — Engineering Method and System Doctrine](./volume-12/)

---

## Part I — Foundations of AI Systems

### [Volume 1 — The Informational/Epistemic Layer](./volume-01/)
*How models think, how meaning is steered, and how state becomes usable.*

- [AI-ENG-A — Model Steering: Harness Engineering, Prompt Semantics & Adaptation Choice](./volume-01/ai-eng-a-model-steering.md)  
  Moves beyond prompt engineering into systematic model steering. Covers prompt structure, harness design, instruction hierarchy, latent-space shaping, task framing, output contracts, and the strategic choice between prompting, RAG, memory, fine-tuning, distillation, and tool use.

- [AI-ENG-B — Context Architecture: State Management & The Tenure Principle](./volume-01/ai-eng-b-context-architecture.md)  
  Treats context as a managed state layer rather than a bag of semantically similar text. Covers session state, cross-session memory, belief-state design, scope isolation, named entity resolution, temporal validity, and stale-memory prevention.

- [AI-ENG-C — The Economic Physics of Inference: Tradeoffs, Cost Attribution & System Margins](./volume-01/ai-eng-c-economic-physics-of-inference.md)  
  Defines the physical and financial laws of AI systems. Covers the tradeoffs among latency, quality, reliability, cost, throughput, context size, model class, and infrastructure.

### [Volume 2 — Knowledge, Data, and Corpus Engineering](./volume-02/)
*Where trustworthy external knowledge comes from, how it is shaped, and how it enters the system.*

- [AI-ENG-D — Corpus Engineering: Data Provenance, Knowledge Hygiene & Source Authority](./volume-02/ai-eng-d-corpus-engineering.md)  
  Establishes the knowledge substrate beneath RAG and memory. Covers ingestion, normalization, deduplication, lineage, metadata, source authority, permission-aware indexing, canonical IDs, alias tables, versioning, retention, redaction, archival policy, and conflict resolution.

- [AI-ENG-E — The Retrieval Pipeline: RAG Architecture, Hybrid Search & Semantic Injection](./volume-02/ai-eng-e-retrieval-pipeline.md)  
  Covers retrieval as a precision delivery system for external context. Includes chunking, embeddings, lexical search, hybrid retrieval, reranking, query rewriting, metadata filtering, freshness checks, citation construction, attribution quality, and context assembly.

- [AI-ENG-F — Knowledge Freshness, Conflict Detection & Context Rot Prevention](./volume-02/ai-eng-f-knowledge-freshness-conflict-detection-context-rot-prevention.md)  
  Focuses on the decay of information over time. Covers stale retrieval, superseded documents, contradictory policies, orphaned facts, version drift, temporal scoping, expiration rules, recency weighting, source precedence, and corpus rot detection.

### [Volume 3 — Model Lifecycle and Adaptation](./volume-03/)
*How models are selected, modified, evaluated, compressed, deployed, and retired.*

- [AI-ENG-G — Model Selection: Capability Fit, Deployment Fit & Failure Tolerance](./volume-03/ai-eng-g-model-selection.md)  
  Covers choosing models by task profile, reasoning depth, context length, tool use, modality, latency tolerance, cost ceiling, license constraints, language coverage, privacy requirements, hardware target, and acceptable failure modes.

- [AI-ENG-H — Model Adaptation: Fine-Tuning, LoRA, Preference Tuning & Distillation](./volume-03/ai-eng-h-model-adaptation.md)  
  Covers supervised fine-tuning, LoRA/QLoRA, preference tuning, domain adaptation, synthetic data generation, dataset design, adapter management, and distillation.

- [AI-ENG-I — Regression Control: Model Registries, Rollouts & Behavioral Drift](./volume-03/ai-eng-i-regression-control.md)  
  Covers model registries, experiment tracking, artifact versioning, model cards, canary deploys, shadow testing, A/B tests, rollback procedures, and silent regression detection.

### [Volume 4 — Runtime Architecture and Inference Mechanics](./volume-04/)
*How AI systems actually execute under physical, computational, and operational constraints.*

- [AI-ENG-J — Throughput Mechanics: Memory Pressure, KV Cache & Hardware Realities](./volume-04/ai-eng-j-throughput-mechanics.md)  
  Covers the brutal physics of scaling inference. Includes prefill vs. decode latency, KV cache management, eviction policies, prompt caching, semantic caching, continuous batching, paged attention, GPU memory pressure, CPU/GPU tradeoffs, and capacity planning.

- [AI-ENG-K — Weight Dynamics: Quantization, Compression & Decoding Strategy](./volume-04/ai-eng-k-weight-dynamics.md)  
  Covers INT8, INT4, FP8, AWQ, GPTQ, pruning, speculative decoding, draft models, constrained decoding, sampling behavior, and quality degradation boundaries.

- [AI-ENG-L — Model Serving Architecture: Routing, Load Balancing & Deployment Topologies](./volume-04/ai-eng-l-model-serving-architecture.md)  
  Covers inference servers, API gateways, local deployment, edge deployment, cloud deployment, multi-model routing, tenant-aware capacity, queueing, backpressure, autoscaling, cold starts, rate limits, and high-availability serving patterns.

---

## Part II — Agentic and Multimodal Systems

### [Volume 5 — Agentic Systems and Tool-Using Architectures](./volume-05/)
*How static generators become actors, and how to keep them from becoming raccoons with API keys.*

- [AI-ENG-M — Agentic Orchestration: Autonomy Boundaries, Loop Budgets & Termination](./volume-05/ai-eng-m-agentic-orchestration.md)  
  Covers agent loops, planning, reflection, decomposition, memory use, tool selection, scratchpads, state machines, graph-based workflows, loop budgets, tool budgets, timeout policies, escalation triggers, and termination conditions.

- [AI-ENG-N — Tool Contracts: Function Calling, Schemas, Validation & Idempotency](./volume-05/ai-eng-n-tool-contracts.md)  
  Covers tool interfaces, schema design, argument validation, retry logic, repair loops, deterministic wrappers, idempotency, side-effect control, transactional boundaries, confirmation gates, and safe execution patterns.

- [AI-ENG-O — Action Verification: Planning, Execution, Observation & Recovery](./volume-05/ai-eng-o-action-verification.md)  
  Covers verifying that tools did what the model thinks they did. Includes post-action checks, state reconciliation, rollback paths, partial failure handling, compensating actions, tool-result grounding, and preventing hallucinated success.

### [Volume 6 — Multimodal and Interface-Controlling Systems](./volume-06/)
*How AI engineering changes when the system reads, sees, hears, speaks, and acts through interfaces.*

- [AI-ENG-P — Multimodal Understanding: Documents, Images, Tables, Charts & Video](./volume-06/ai-eng-p-multimodal-understanding.md)  
  Covers OCR, layout-aware document parsing, table extraction, form understanding, chart interpretation, image retrieval, video sampling, visual grounding, multimodal embeddings, and evidence selection.

- [AI-ENG-Q — Speech, Voice, and Real-Time Interaction Systems](./volume-06/ai-eng-q-speech-voice-real-time-interaction-systems.md)  
  Covers speech-to-text, text-to-speech, turn-taking, interruption handling, latency budgets, streaming generation, conversational repair, voice identity, accessibility, and the UX consequences of real-time AI behavior.

- [AI-ENG-R — UI Agents: Browser Control, Desktop Automation & Visual State](./volume-06/ai-eng-r-ui-agents.md)  
  Covers agents that operate software interfaces. Includes DOM inspection, screenshot reasoning, visual state tracking, action planning, click/type verification, form submission safety, browser sandboxing, and recovery from interface drift.

---

## Part III — Failure, Security, and Resilience

### [Volume 7 — Failure, Security, and Hostile Environments](./volume-07/)
*How AI systems break, leak, get attacked, or quietly become cursed.*

- [AI-ENG-S — Production Pathologies: Hallucination, Malformed Output & Runaway Behavior](./volume-07/ai-eng-s-production-pathologies.md)  
  Covers hallucinated tool calls, malformed JSON, broken schemas, invalid citations, instruction loss, tool loops, brittle chains, contradictory outputs, non-deterministic regressions, and production failure patterns that do not appear in demos.

- [AI-ENG-T — Boundary Defense: Prompt Injection, Data Leakage & Tenant Isolation](./volume-07/ai-eng-t-boundary-defense.md)  
  Covers prompt injection, indirect prompt injection, cross-user contamination, system prompt exposure, sensitive data leakage, retrieval poisoning, cache leakage, multi-tenant isolation, permission boundaries, and context separation in B2B environments.

- [AI-ENG-U — AI Supply Chain Security: Models, Datasets, Dependencies & Tool Surfaces](./volume-07/ai-eng-u-ai-supply-chain-security.md)  
  Covers model provenance, malicious artifacts, dataset poisoning, embedding poisoning, dependency risk, parser risk, plugin risk, tool-server risk, secrets handling, sandboxing, egress control, and output-handling vulnerabilities.

- [AI-ENG-V — Resource Abuse, Cost Bombs & Unbounded Consumption](./volume-07/ai-eng-v-resource-abuse-cost-bombs-unbounded-consumption.md)  
  Covers denial-of-wallet attacks, recursive agents, context flooding, retrieval flooding, tool-loop exhaustion, adversarial latency inflation, runaway batch jobs, quota exhaustion, and budget-aware containment systems.

### [Volume 8 — Resilience, Degraded Modes, and Human Trust](./volume-08/)
*How systems fail gracefully enough that users do not feel the machinery grinding underneath them.*

- [AI-ENG-W — UX Resilience: Model Routing, Fallback Chains & Degraded-Mode Grace](./volume-08/ai-eng-w-ux-resilience.md)  
  Covers model routing, fallback models, dynamic quality/cost switching, cached answers, partial answers, graceful error states, escalation to humans, retry UX, and user-facing continuity when backend systems degrade.

- [AI-ENG-X — Human-System Interface: Trust Calibration, Provenance & Control](./volume-08/ai-eng-x-human-system-interface.md)  
  Covers uncertainty display, citation UX, explanation design, approval flows, edit capture, undo mechanisms, confidence signaling, human correction loops, and interfaces that prevent both overtrust and undertrust.

- [AI-ENG-Y — High-Impact Workflow Design: Confirmation, Review & Human-in-the-Loop Governance](./volume-08/ai-eng-y-high-impact-workflow-design.md)  
  Covers workflows where AI actions affect money, legal exposure, customer relationships, safety, reputation, or operational continuity. Includes approval thresholds, review queues, audit trails, escalation paths, role-specific permissions, and human accountability design.

---

## Part IV — Evaluation, Operations, and Governance

### [Volume 9 — Observability, Evaluation, and Verification](./volume-09/)
*How to know whether the system is actually doing what it claims to do.*

- [AI-ENG-Z — Strategic Telemetry: Traces, Tokens, Latency & Semantic Drift](./volume-09/ai-eng-z-strategic-telemetry.md)  
  Covers LLM observability as a first-class engineering discipline. Includes traces, spans, tokens, latency, errors, retries, tool calls, retrieval events, routing decisions, cost attribution, semantic drift, and the Green Dashboard Fallacy.

- [AI-ENG-AA — Evals Architecture: Ground Truth, Golden Sets & Regression Tests](./volume-09/ai-eng-aa-evals-architecture.md)  
  Covers task evals, retrieval evals, grounding evals, citation evals, tool-use evals, adversarial evals, golden sets, synthetic test generation, human review, LLM-as-judge limits, inter-rater reliability, and regression gates.

- [AI-ENG-AB — Verification Artifacts: Auditability, Reproducibility & Evidence Trails](./volume-09/ai-eng-ab-verification-artifacts.md)  
  Covers preserving the evidence needed to understand why a system answered or acted as it did. Includes prompt versions, model versions, retrieved chunks, tool calls, intermediate states, user edits, policy checks, output diffs, and replayable execution traces.

### [Volume 10 — Operations, Governance, and Lifecycle Management](./volume-10/)
*How AI systems are maintained as living infrastructure rather than one-time builds.*

- [AI-ENG-AC — AI Operations: Incident Response, Rollback & Runbook Design](./volume-10/ai-eng-ac-ai-operations.md)  
  Covers AI-specific incidents, severity levels, triage, rollback, feature flags, kill switches, prompt hotfixes, corpus rollback, model rollback, user communication, incident review, and operational playbooks for semantic failures.

- [AI-ENG-AD — Governance Architecture: Policy, Audit, Compliance & Accountability](./volume-10/ai-eng-ad-governance-architecture.md)  
  Covers lightweight AI governance, internal policy, audit trails, risk classification, procurement review, data handling, access control, approval workflows, documentation, accountability boundaries, and practical governance systems.

- [AI-ENG-AE — Sustainable AI: Energy, Infrastructure Efficiency & Lifecycle Impact](./volume-10/ai-eng-ae-sustainable-ai.md)  
  Covers energy-aware deployment, model sizing, hardware utilization, carbon-aware scheduling, efficient inference, lifecycle infrastructure costs, local vs. cloud tradeoffs, and sustainability as an architectural constraint.

---

## Part V — Product Doctrine and Engineering Method

### [Volume 11 — Product, Business, and Organizational Architecture](./volume-11/)
*How to ensure the system matters, survives adoption, and creates value instead of expensive theater.*

- [AI-ENG-AF — AI Product Architecture: Use-Case Selection, Workflow Fit & Value Design](./volume-11/ai-eng-af-ai-product-architecture.md)  
  Covers identifying where AI belongs and where it does not. Includes automation vs. augmentation, feasibility scoring, workflow mapping, user tolerance, evaluation clarity, risk level, data readiness, ROI modeling, cost-to-serve, and the AI-Shaped Hole Fallacy.

- [AI-ENG-AG — Adoption Systems: Training, Feedback Loops & Change Management](./volume-11/ai-eng-ag-adoption-systems.md)  
  Covers onboarding, user education, internal champions, feedback capture, support burden, behavioral incentives, team resistance, process redesign, and the practical sociology of getting humans to use AI systems well.

- [AI-ENG-AH — Build, Buy, Open Source & Vendor Strategy](./volume-11/ai-eng-ah-build-buy-open-source-vendor-strategy.md)  
  Covers vendor selection, open-source model strategy, local deployment, managed APIs, hybrid architectures, procurement realities, lock-in, licensing, data rights, security review, integration costs, exit plans, and total cost of ownership.

### [Volume 12 — Engineering Method and System Doctrine](./volume-12/)
*The cross-cutting principles that govern the entire canon.*

- [AI-ENG-AI — Contract Thinking: Deterministic Edges Around Probabilistic Cores](./volume-12/ai-eng-ai-contract-thinking.md)  
  Establishes contracts at every seam: prompts, schemas, tools, retrieval, permissions, costs, evals, model behavior, memory, deployment, and user expectations.

- [AI-ENG-AJ — AI System Design Patterns: Reference Architectures & Failure-Aware Blueprints](./volume-12/ai-eng-aj-ai-system-design-patterns.md)  
  Covers reusable architectures for copilots, research agents, support assistants, document intelligence, workflow automation, coding agents, analytics assistants, multimodal review systems, and enterprise knowledge systems.

- [AI-ENG-AK — The AI Engineering Mindset: Probabilistic Systems, Human Judgment & Iterative Control](./volume-12/ai-eng-ak-ai-engineering-mindset.md)  
  Defines the discipline’s operating philosophy: AI engineering is not normal software engineering with a chat box attached; it is the design of probabilistic cognitive infrastructure inside deterministic operational systems.

---

## Citation

If you use or reference this canon, please cite the repository and credit Sam “stunspot” Walker / Collaborative Dynamics.

See [CITATION.cff](../CITATION.cff) for citation metadata.

## License

See [LICENSE.md](../LICENSE.md).