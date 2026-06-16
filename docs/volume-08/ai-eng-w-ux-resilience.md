# AI-ENG-W - UX Resilience - Model Routing, Fallback Chains & Degraded-Mode Grace

## **Doctrinal Foundations of UX Resilience**

Production reliability in high-dimensional artificial intelligence systems is fundamentally distinct from traditional software availability.1 In legacy web architectures, system health is treated as a binary state: a service is either operational or throwing exceptions, and mitigation relies on standard redundant failovers and load balancers.2 In modern systems driven by probabilistic large language models, however, backend availability does not guarantee a successful, safe, or contextually coherent user transaction.1 An artificial intelligence gateway may return a successful HTTP 200 payload that contains a complete structural schema failure, a hallucinated tool execution, a cross-tenant data leak, or an ungrounded factual confabulation.1 Conversely, transient network congestion, upstream rate limits, or regional outages can trigger sudden API timeouts that interrupt multi-turn workflows and erase critical user progress.1  
This gap establishes the systems-engineering discipline of User Experience (UX) Resilience.1 UX Resilience is the user-facing practice of preserving continuity, honesty, usefulness, and safety when backend systems degrade, slow down, or fail.1 Rather than treating service exceptions as terminal application crashes covered by generic error messages, a resilient system treats degraded capability as an explicitly designed product state.2 The system may route to fallback models, use cached answers, narrow the search space of retrieval-augmented systems, deliver partial answers, pause automated tool executions, escalate to human reviewers, or present safe retry options—but it must do so without silently violating user expectations around quality, freshness, evidence, cost, latency, privacy, or task completion.1  
The architecture must distinguish clearly between a backend fallback and a user-facing degraded mode.1 A backend fallback is a silent transport-layer redirection that shifts a request from a failing primary endpoint to a secondary provider or model.6 While fallback routing is a necessary infrastructure primitive, it is not automatically a resilient experience.1 If a primary, logic-heavy model fails and the gateway silently routes the query to a smaller model, the user may receive an answer that loses structural formatting, drops citation tracking, fails to execute nested tools, or violates safety boundaries.1 A user-facing degraded mode, by contrast, preserves the task state, avoids duplicate actions on state-changing APIs, enforces safety floors, and communicates the platform's active limitations transparently.1 This distinction is governed by the core doctrine:  
**AI systems should degrade along explicit quality, cost, latency, evidence, and safety dimensions while preserving user intent, session continuity, and transparent status. A fallback is not successful because something answered; it is successful because the user receives the best safe answer the system can honestly provide under degraded conditions.**  
The primary engineering question shifts from "Do we have a fallback model?" to "When the ideal path is unavailable, expensive, slow, partial, or unsafe, can the system still provide a coherent next-best experience without lying about quality, losing state, duplicating actions, or forcing the user to restart?" 1 To resolve this, the system must balance provider-optimal cost metrics with user-preferred utility.8 In a single-provider, multi-tenant interaction, the relationship between the provider's cost and the user's utility can be modeled as a Stackelberg game.8 Let U_a represent the user utility derived from model action a, let t_a represent the latency delay associated with that action, and let c_a represent the monetary cost of the inference execution to the provider.8 The user's action selection seeks to maximize utility minus latency delay 8:  
max_{a in A} (U_a - beta * t_a)  
where beta is a normalization parameter representing user sensitivity to delay.8 The provider, as the leader, optimizes its routing policies to minimize operational service costs (sum of c_i) while maintaining user retention.8 Under high load or system degradation, providers may be incentivized to throttle latency or downgrade model tiers to minimize costs, creating a misalignment gap that depresses user utility.8 A resilient UX architecture reconciles this misalignment by making degradation explicit, giving the user control over the tradeoff between speed, cost, and output quality.5

### **Conceptual Glossary**

This glossary defines the core operational metrics and terms governing UX resilience and degraded-mode orchestration:

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **UX Resilience** | The systematic preservation of task progress, data integrity, user orientation, and trust during backend service failures or degraded capability. | User-Visible Incident Rate | User-visible degradation is disclosed, state-preserving, and bounded by severity policy. |
| **Degraded Mode** | A designed, stateful product condition that communicates reduced platform capability while guiding the user toward safe task continuation. | Active Degradation Exposure | Exposure remains within service SLO; high-impact tasks preserve explicit status and controls. |
| **Fallback Chain** | A declarative, ordered sequence of alternative execution paths triggered when the primary path fails, slows, exceeds budget, or loses required capability. | Fallback Trigger Rate | Fallbacks are observable, tested, and policy-preserving. |
| **Model Routing** | The real-time decision to direct a request to a model or provider based on task requirements, latency, cost, safety, context size, and system health. | Routing Accuracy | Routing decisions satisfy task capability and safety floors under evaluation. |
| **Quality Floor** | The minimum acceptable level of model capability, grounding, structure, safety, privacy, and action verification required for a task. | Floor Breach Rate | Breaches block, escalate, or enter managed degraded mode. |
| **Cached Answer** | A previously generated and verified response stored with freshness, scope, source, policy, and permission metadata. | Cache Return Rate | Cache is served only when freshness, permission, source version, and task risk allow. |
| **Stale Answer** | A cached response whose freshness window has expired but may be shown under emergency or low-risk conditions with clear labeling. | Stale Cache Rate | Stale answers are blocked for high-impact/current tasks unless explicitly approved and disclosed. |
| **Partial Answer** | A response that delivers verified completed components while clearly declaring failed, skipped, unavailable, or unexecuted dependencies. | Partial Answer Rate | Partial answers preserve truth boundaries and do not imply task completion. |
| **Graceful Error** | A state-preserving terminal feedback condition explaining what failed, what succeeded, what is saved, and what the user can safely do next. | Lost-Progress Rate | Critical user inputs, drafts, files, and action state are preserved or explicitly marked unrecoverable. |
| **Retry UX** | A user-facing pattern that coordinates retries, backoff, cancellation, idempotency, and duplicate-action prevention. | Duplicate Transaction Count | State-changing actions are not automatically retried without idempotency and verification. |
| **Continuity State** | The serialized state required to resume a task across route switches, degraded modes, retries, or human escalation. | State Preservation Accuracy | Critical task state is preserved and verified across transitions. |
| **Escalation Package** | A structured, redacted, scoped payload containing the information a human reviewer needs to continue or resolve a degraded workflow. | Post-Escalation Resolution Time | Escalation packets are complete enough for review without exposing unnecessary sensitive data. |
| **Fail-Closed Mode** | A protective terminal state where execution halts because no available fallback can satisfy safety, privacy, quality, evidence, or verification requirements. | Uncontained Unsafe Completion Rate | Unsafe fallback completion is blocked; user receives saved-state and recovery options. |

## **Degraded Mode Taxonomy**

Failures in AI systems are rarely binary. A platform may still respond while losing freshness, grounding, tool capability, latency guarantees, multimodal fidelity, or action authority. Degraded-mode handling must therefore identify which capability changed, preserve state, disclose the change when it affects user expectations, and prevent unsafe fallback.

| Degraded Mode | Trigger Condition | User-Visible Symptom | Safe Fallback Option | Unacceptable Fallback | Required Disclosure | Continuity Requirement | Telemetry Event | Escalation Path |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Model Degradation** | Primary route returns 429/5xx, times out, exceeds latency budget, or fails quality checks. | Slower response, shorter answer, reduced reasoning depth, or lower formatting fidelity. | Route to an approved model that satisfies the task’s quality, safety, schema, context, and tool requirements. | Route to a cheaper model that lacks required capabilities. | Disclose when quality, latency, citations, structure, or tools materially change. | Preserve prompt state, user intent, files, tool state, and active constraints. | `model_route_degraded` | Escalate if no model satisfies quality floor. |
| **Retrieval Degradation** | Vector index, keyword index, citation service, or document store times out or returns insufficient evidence. | Missing citations, narrower evidence set, slower research, or “cached references only.” | Use authorized lexical search, verified cache, narrower search, or ask clarification. | Generate unsupported current/policy/legal/financial claims as if grounded. | Disclose evidence limitation and freshness status. | Preserve original query, filters, tenant scope, and evidence requirements. | `retrieval_degraded` | Route to research/review queue for high-impact answers. |
| **Tool Degradation** | External API, connector, browser, or database tool times out, hits quota, or fails schema/policy validation. | Action buttons disabled, draft saved, status pending, or tool-specific warning. | Preserve draft payload, show last verified read-only data, or let user retry after verification. | Mock execution, invent tool results, or claim success without verified state. | State whether the action was not executed, pending, failed, or unknown. | Preserve tool arguments, idempotency keys, approval state, and verification status. | `tool_degraded` | Escalate high-impact pending/unknown states. |
| **Parser Degradation** | Document converter, OCR, layout parser, table extractor, or media parser fails. | Basic preview, missing tables, unavailable layout, lower confidence extraction. | Use text-only extraction, metadata preview, sampled pages, or request better file. | Discard upload, hallucinate unread content, or treat low-confidence extraction as verified. | Disclose parser limitation and affected evidence. | Preserve raw file, source metadata, parser errors, and upload state. | `parser_degraded` | Route to manual verification for high-impact documents. |
| **Multimodal Degradation** | Image, video, audio, or chart analysis route is unavailable, overloaded, or low confidence. | Missing visual details, delayed media analysis, text-only mode, sampled frames. | Use bounded OCR, metadata, sampled frames, or defer analysis. | Invent visual details or ignore media while implying it was inspected. | Disclose what was and was not visually inspected. | Preserve raw media, timestamps, frame/page references, and extracted evidence. | `multimodal_degraded` | Human verification for safety-critical visual evidence. |
| **Voice Degradation** | STT instability, TTS outage, packet loss, endpointing failures, or noisy input. | Audio gaps, repeated confirmations, text fallback, muted automation. | Switch to text input, keypad/card confirmation, or slower confirmation mode. | Force repeated speech over a degraded channel for high-impact actions. | Tell the user the voice channel is unreliable and offer a fallback. | Preserve transcript, task intent, confirmations, and interruption state. | `voice_degraded` | Transfer or hold for live operator when needed. |
| **UI-Agent Degradation** | DOM drift, browser crash, stale selector, occlusion, unsafe origin, or automation uncertainty. | Automation pauses, user handoff, re-observe indicator, disabled click automation. | Re-observe the UI, re-plan, ask user to take over, or switch to instruction-only mode. | Blind clicking, stale coordinates, or continuing after origin uncertainty. | Disclose automation uncertainty and paused state. | Preserve form inputs, navigation history, screenshots, and verified UI state. | `ui_agent_degraded` | Handoff to user/operator for high-risk interfaces. |
| **Quota Degradation** | Tenant budget exhausted, provider rate limit hit, or gateway circuit breaker open. | Cooldown, queue wait, reduced mode, quota banner. | Queue, throttle, offer lower-cost approved mode, or return managed capacity status. | Bypass policy, use unapproved route, or continue until provider errors cascade. | Show quota/rate-limit state and available options. | Preserve queue position, active input, drafts, and budget decision. | `quota_degraded` | Admin/support escalation for business-critical workloads. |
| **Cache Degradation** | Fresh route unavailable and verified cache exists, or cache is stale/partial. | Timestamped cached answer, freshness warning, limited interaction. | Serve cache only if permission, source version, policy version, freshness, and task risk allow. | Serve stale or cross-scope cache as fresh. | Display timestamp, source/freshness status, and limitation. | Preserve query parameters, source IDs, cache key scope, and freshness metadata. | `cache_degraded` | Force fresh route or review for high-impact/current tasks. |
| **Human-Review Degradation** | Escalation queue full, reviewer unavailable, or review SLA exceeded. | “Awaiting review,” delayed completion, limited automation. | Pause high-impact decisions, triage low-risk cases, or provide saved-state options. | Auto-approve unverified high-risk mutations or fabricate review outcome. | Explain review delay and what remains blocked. | Preserve escalation package, evidence, approvals, and user-visible status. | `review_degraded` | Route to priority queue or accountable owner. |

## **Model Routing Policy**

A production-grade AI platform must avoid hardcoding model choices inside application services.27 Instead, the architecture routes traffic through a centralized, budget-aware gateway that dynamically evaluates where to direct each query.28 This routing layer evaluates multiple dimensions, balancing quality demands against real-time cost, latency, and system availability constraints.11  
Model routing must not operate as a simple cost-reduction engine.1 Routing a task to a cheaper model to save token spend is an anti-pattern if that model lacks the necessary capabilities.1 For example, directing a long-context legal query to a model with a small context window causes silent truncation and critical instruction loss.1 Similarly, routing a financial transaction query to a model that cannot process structured JSON schemas leads to parsing exceptions and downstream system crashes.1  
To manage these tradeoffs, the gateway classifies routing decisions across four distinct visibility levels:

* **Silent Routing Allowed**: The system may route requests silently when the target models are functionally equivalent (e.g., load balancing across identical model deployments or switching between equivalent providers).4 The user experience remains consistent in quality, speed, safety, and correctness, leaving no reason to interrupt the transaction flow.11  
* **Disclosure Required**: The system must notify the user when the fallback route changes the response quality, latency, data freshness, or citation completeness.5 For example, if a primary, deliberative model is unavailable and the system falls back to a standard model, the UI must disclose that the answer may be less detailed or lack complete source grounding.5  
* **User Choice Required**: The user must explicitly approve the routing decision when the fallback path alters the financial cost, uses stale cached data for time-sensitive tasks, returns a partial answer, or escalates to a human operator.8 The system presents these options clearly, allowing the user to select the best tradeoff for their task.8  
* **Blocked Routing Required**: The system must block routing entirely when no available fallback model can satisfy the task's safety, privacy, compliance, or accuracy floors.1 In these scenarios, the platform must fail-closed, returning a managed exception rather than risking data leaks, security breaches, or corrupted transactions.1

### **Model Routing Policy Matrix**

The gateway enforces this model selection policy using a structured multi-dimensional matrix:

| Task Type | Risk Class | Latency Target | Quality Floor | Cost Ceiling | Allowed Routes | Disclosure Rule | Blocked Routes |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Financial Ledger Mutation** 1 | High | < 1500 ms 3 | High capability; structured FSM JSON output; strict semantic checks.1 | $0.05 / request | Primary deliberative model; multi-provider equivalent models.4 | Silent Routing Allowed (if performance is equivalent).11 | Smaller models lacking strict JSON compilation or float-point precision.1 |
| **Customer Support FAQ** 1 | Low | < 500 ms 3 | Efficient model; basic context extraction.1 | $0.001 / request | Standard model; semantic cache; local keyword search.10 | Silent Routing Allowed (if standard model handles query).11 | None; standard and cached routes are acceptable.10 |
| **Legal Contract Auditing** 3 | High | < 8000 ms | Deep analytical tracking; long-context (128K); source citation precision.1 | $0.15 / request | Flagship deliberative model; long-context optimized models.1 | Disclosure Required (if falling back to standard long-context model).5 | Models lacking citation tracking or context window lengths < 100K tokens.1 |
| **Real-time Voice Navigation** 3 | Medium | < 300 ms 3 | Voice-optimized model; low TTFT; semantic turn-taking.3 | $0.01 / minute | Edge voice-tuned model; streaming speech API fallback.3 | Silent Routing Allowed (to balance latency across local/cloud engines).3 | Non-streaming models; high-latency deliberation routes (>1s TTFT).3 |
| **Interactive Code Generation** 1 | Medium | < 2000 ms | Code-specialized model; exact syntax matching.1 | $0.02 / request | Code-expert model; standard model with syntax validators.1 | User Choice Required (if switching to a less capable model for edits).30 | Models failing syntax-compilation checks or lacking schema compliance.1 |
| **Enterprise Medical Diagnosis** 23 | High | < 5000 ms | Flagship deliberative model; expert-level accuracy; strict safety bounds.23 | $0.20 / request | Flagship deliberative model with human-in-the-loop gate.23 | User Choice Required (before escalating to human review desk).23 | Any automated route lacking active safety filters or human-review paths.23 |

## **Fallback Chain Contract Model**

A fallback chain must operate as a declarative, testable, and observable software contract rather than an ad-hoc loop of cheaper model attempts.10 Each fallback step defines what triggers it, what capability is sacrificed, and how the user's progress and safety bounds are preserved.1 This fallback path is structured as a progressive degradation loop:  
Primary Model Route --> Context Pruning --> Efficient Model Route --> Cache Lookup --> Partial Answer --> Human Escalation --> Fail-Closed  
By formalizing this sequence, the system avoids random model-switching loops.1 Each transition is governed by a strict, declarative schema that preserves the user's context across different platforms and providers.6 The following YAML manifest defines a resilient fallback chain contract managed centrally at the gateway:

```yaml
route_id: "contract_audit_resilience_v1"
tenant_scope: "tenant_enterprise_standard"
task_profile: "high_risk_document_analysis"
primary_target:
  route: "primary_reasoning_route"
  timeout_ms: 5000
  retry_policy:
    max_attempts: 2
    backoff_factor: 1.5
    jitter: true
    on_status_codes:
      - 429
      - 500
      - 502
      - 503
      - 504

quality_floors:
  schema_validity: "strict"
  evidence_support: "required"
  citation_fidelity: "required"
  tenant_isolation: "required"
  tool_authority: "required"
  high_impact_action_verification: "required"

fallback_chain:
  - step: 1
    trigger: "context_overflow_or_latency_risk"
    target: "context_pruning_filter"
    allowed_loss:
      - "low_priority_history"
      - "redundant_retrieval_chunks"
    preserved:
      - "system_policy"
      - "tenant_scope"
      - "user_goal"
      - "active_constraints"
      - "approvals"
      - "evidence_requirements"
    disclosure_rule: "subtle_status_if_user_relevant"

  - step: 2
    trigger: "primary_route_unavailable"
    target: "approved_capability_equivalent_route"
    allowed_loss:
      - "minor_style_variation"
      - "nonessential_verbosity"
    preserved:
      - "schema_support"
      - "safety_profile"
      - "tool_policy"
      - "context_window_requirement"
      - "evidence_support"
    disclosure_rule: "none_if_equivalent_else_banner"

  - step: 3
    trigger: "equivalent_route_unavailable"
    target: "approved_degraded_route"
    allowed_loss:
      - "answer_depth"
      - "number_of_citations"
      - "advanced_formatting"
    preserved:
      - "safety_floor"
      - "tenant_scope"
      - "schema_validity"
      - "privacy_policy"
      - "truthful_disclosure"
    disclosure_rule: "banner_warning"

  - step: 4
    trigger: "fresh_generation_unavailable"
    target: "verified_cache_lookup"
    allowed_loss:
      - "freshness"
      - "interactive_followup"
    preserved:
      - "cache_scope_match"
      - "source_version_match"
      - "policy_version_match"
      - "permission_check"
      - "timestamp_disclosure"
    disclosure_rule: "freshness_badge_required"

  - step: 5
    trigger: "cache_miss_or_cache_not_allowed"
    target: "partial_answer_generator"
    allowed_loss:
      - "complete_task_resolution"
      - "unverified_subtasks"
    preserved:
      - "known_unknown_boundary"
      - "completed_step_status"
      - "unexecuted_step_status"
      - "no_false_completion_claim"
    disclosure_rule: "detailed_status_card"

  - step: 6
    trigger: "partial_answer_not_safe_or_review_required"
    target: "human_review_queue"
    allowed_loss:
      - "instant_response"
    preserved:
      - "redacted_session_context"
      - "evidence_ids"
      - "tool_ledger"
      - "approval_state"
      - "user_goal"
    disclosure_rule: "review_status_card"

  - step: 7
    trigger: "no_safe_fallback_available"
    target: "fail_closed"
    allowed_loss:
      - "availability"
    preserved:
      - "saved_progress"
      - "security_state"
      - "audit_trace"
      - "recovery_options"
    disclosure_rule: "managed_failure_message"
```

### **Fallback Chain Contract Table**

The fallback chain is safe only when every transition preserves the relevant quality floor. The system should not move “down” the chain merely to get any answer. It should move only to a route that can still satisfy the task’s required safety, evidence, privacy, and action-verification constraints.

| Step | Trigger Event | Fallback Target | Acceptable Loss | Preserved Floor | User Disclosure | Retry Policy | State Preservation | Evidence Handling | Escalation Rule |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **0** | Normal session start. | Primary route. | None. | Full task capability. | None. | Profile-specific retry with backoff. | Full active state. | Fresh retrieval and normal citation validation. | Move to Step 1 on overflow, latency risk, or primary-route failure. |
| **1** | Context too large, latency risk, or low-priority evidence overflow. | Context pruning / compaction. | Redundant history, low-priority chunks, nonessential verbosity. | System policy, user goal, active constraints, approvals, evidence requirements. | Subtle status only if visible quality changes. | One bounded attempt. | Summarize or externalize low-priority state; do not drop unresolved constraints. | Preserve high-authority and task-critical evidence. | Move to Step 2 if primary remains unavailable or task cannot fit safely. |
| **2** | Primary model/provider unavailable or throttled. | Capability-equivalent approved route. | Minor style or latency variation. | Safety profile, schema support, context window, tool policy, privacy scope. | None if truly equivalent; otherwise banner. | Bounded retry. | Serialize session variables and route manifest. | Citation/evidence handling must remain equivalent. | Move to Step 3 if no equivalent route exists. |
| **3** | Equivalent route unavailable. | Approved degraded route. | Depth, advanced formatting, number of citations, nonessential elaboration. | Safety, tenant isolation, schema validity, privacy, truthful limitation disclosure. | Required. | No repeated downgrade loops. | Preserve task state and show changed capability. | Use only evidence the degraded route can validate. | Move to Step 4 if generation is unavailable or not safe. |
| **4** | Fresh generation unavailable or over capacity. | Verified cache lookup. | Freshness and interactivity. | Permission scope, source version, policy version, cache key scope, task suitability. | Timestamp/freshness badge required. | No generation retry. | Freeze state at cache-serving point. | Cached citations must still be valid and authorized. | Move to Step 5 if cache misses, is stale-for-risk, or lacks scope match. |
| **5** | Cache unavailable or insufficient. | Partial answer. | Complete task resolution. | Completed/uncompleted boundary, no unsupported claims, no false completion. | Detailed status card. | No automated retries unless read-only and bounded. | Save completed steps and mark unexecuted steps. | Cite only verified completed evidence. | Move to Step 6 if task requires review or partial answer fails safety. |
| **6** | High-impact uncertainty, policy block, failed verification, or review requirement. | Human review. | Instant automated completion. | Redacted context, evidence IDs, action ledger, user goal, approval state. | Review status card. | None; enters review workflow. | Package minimum necessary context. | Provide reviewer evidence links, not uncontrolled raw dumps. | Move to Step 7 if review is unavailable and no safe automated option exists. |
| **7** | No safe fallback path remains. | Fail-closed managed state. | Availability. | Saved progress, security, privacy, audit trace, recovery options. | Required. | Disabled until recovery path exists. | Save state and block risky execution. | Preserve evidence trace for replay. | Terminal; surface support/retry options. |

## **Quality, Cost, and Latency Tradeoff Model**

Managing degraded states requires a structured approach to quality, cost, and latency tradeoffs.11 When a platform experiences high traffic or component failures, it must know exactly which features can be sacrificed and which must remain protected to preserve system integrity.1  
To guide these decisions, system features are divided into two clear operational classes:

* **Sacrificable Dimensions**: Under high system load, the platform can reduce response verbosity, limit the depth of multi-step tool logic-planning, prune the number of retrieved context citations, lower image resolution from 300 to 150 DPI, or serve cached data within acceptable freshness limits.2 These adjustments reduce compute and network overhead without impacting the core transaction.1  
* **Non-Sacrificable Dimensions**: The system must never compromise tenant isolation, data privacy, security permissions, database transaction verification, or user consent for irreversible actions.1 Security controls must remain strict in all degraded states: a system must never default to an open state or bypass authorization checks to keep a service online.34

These tradeoffs are managed across seven distinct quality bands, allowing the platform to adapt its behavior to the complexity of the incoming query and the health of the underlying services:

[Full Fidelity] ──(High Load)──> [Efficient Mode] ──(Quota Limit)──> [Cached Mode] ──(Timeout)──> [Partial Mode] ──(Policy Block)──> [Assistive Mode] ──(Failure)──> [Escalation Mode] ──(Saturated)──> [Fail-Closed]

### **Quality Bands and Operational Parameters**

The operating boundaries of these seven quality bands are defined in the following matrix:

| Quality Band | Processing Characteristics | Allowed Sacrifices | Non-Sacrificable Dimensions | User-Visible Signatures |
| :---- | :---- | :---- | :---- | :---- |
| **Full Fidelity** | Primary deliberative model; fresh document retrieval; full multi-step tool execution; complete citation mapping.1 | None; all features run at peak capability. | Strict data permissions; database-level tenant isolation; full safety verification.1 | Default platform UI; complete citation coordinates and rich layout structures.3 |
| **Efficient Mode** | Smaller model; pruned context window; single-pass tool execution; limited retrieval citations.1 | Language style; summary depth; number of background search turns.1 | Structured JSON schema compliance; input validation; user access tokens.1 | Light banner notification; slightly shorter, more direct responses.5 |
| **Cached Mode** | Immediate response served from the semantic cache; no model inference executed.14 | Real-time data freshness; interactive dialogue adjustments.14 | Hashed tenant access checks; cache key permission scopes.1 | Freshness badge: "Showing verified response from [timestamp]." 5 |
| **Partial Mode** | Delivers completed subtasks; flags failed tool integrations or retrieval blocks.17 | Task completeness; execution of secondary backend tools.17 | Verification of completed transactions; clear state tracking.1 | Status card showing completed subtasks alongside unexecuted blocks.36 |
| **Assistive Mode** | Pauses automation; guides the user to perform actions manually.1 | Autonomous agent actions and background process runs.1 | Security sandboxes; local device directory restrictions.1 | Informational walkthroughs: "I've drafted the data. Click here to submit." 26 |
| **Escalation Mode** | Suspends automation; compiles and packages active session state for manual human review.19 | Instant response latency; automated transaction processing.1 | Complete trace logging; compliance audit trails; data masking.1 | Loading indicator: "Routing task to a specialist. Your progress is saved." 7 |
| **Fail-Closed Mode** | Halts execution; revokes active tool credentials; saves state and terminates connection.1 | All service availability and interactive features.1 | Session security; database transaction rollbacks; credential safety.1 | Error message overlay: "System offline. Progress saved securely." 1 |

## **Cached Answer Validity Model**

Serving cached answers can preserve continuity and reduce cost during outages, rate limits, or repeated queries. But cached answers are not automatically safe. A cache entry can be stale, cross-scoped, unsupported by current policy, or semantically similar while being wrong for the user’s actual task.

Shared prefix and semantic caching can also create side-channel risks in multi-tenant systems. Defenses such as tenant-scoped keys, permission-aware cache lookup, selective prefix isolation, timing padding, and cache-key versioning should be selected according to serving architecture and threat model. The point is not that one named framework must always be used; the point is that cache reuse must be scoped, fresh enough, and honest.

### **Cached Answer Validity Matrix**

| Cache Class | Typical TTL Profile | Permission Scope | Source / Policy Versioning | Must Block When | User UI Labeling |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Static Policies & Docs** | Hours to days, depending on release cadence. | Global, tenant, role, or workspace scoped. | Policy repository version, release manifest, source hash. | Policy version changed, user lacks scope, or answer affects high-impact decision without current validation. | “Using verified policy answer from [timestamp/version].” |
| **Product Catalogs** | Minutes to hours. | Tenant, group, or storefront scoped. | Catalog timestamp, inventory version, pricing version. | Price/inventory freshness is required or source version changed. | “Showing product details cached [duration] ago.” |
| **General Q&A** | Short to medium TTL. | User, workspace, or public scope depending on content. | Prompt template version, safety policy version, model route. | Query is materially different, safety policy changed, or answer requires current facts. | “Showing cached answer for a similar repeat question.” |
| **Real-Time Inventory / Status** | Very short TTL or disabled. | Tenant/workspace scoped. | Transactional record timestamp and source-of-record version. | User is about to act on availability, finance, legal, medical, security, or operational state. | “Showing cached status from [timestamp]; refresh recommended before acting.” |
| **Active Session State** | Session TTL. | Private user/session scope. | Session sequence, tool ledger, approval state. | New user message, changed parameters, changed approval payload, or tool state update. | “Restoring your saved draft/session state.” |
| **Generated Drafts** | Session or workspace TTL. | User/workspace scoped. | Draft version and editing history. | Draft contains stale tool results or unverified claims. | “Restored draft from [timestamp]. Review before sending/submitting.” |
| **Evidence Snippets** | Bound to source lifecycle. | Same scope as source document. | Source ID, source version, coordinates/section IDs. | Source document changed, permissions changed, or coordinates no longer match. | “Using saved evidence from [source/version].” |

Cache eligibility checks should include:

| Check | Purpose |
| :--- | :--- |
| **Tenant/User/Role Scope** | Prevent cross-user or cross-tenant leakage. |
| **Source Version** | Prevent stale evidence after document or database updates. |
| **Policy Version** | Prevent outdated safety/compliance behavior. |
| **Prompt/Schema Version** | Prevent structurally incompatible cached responses. |
| **Task Risk Class** | Block stale/cache-only answers for high-impact tasks. |
| **Freshness Requirement** | Ensure current questions receive current answers. |
| **Disclosure Requirement** | Make freshness and degraded state visible to the user. |

## **Partial Answer Policy**

During multi-step AI agent workflows or distributed database queries, a single subtask failure—such as a tool timeout, a failed document parse, or a locked database record—should not force the entire session to crash.1 A resilient platform applies a Partial Answer Policy, isolating the failure and delivering the successfully processed components rather than returning a generic error page.5  
The system's generation layer must clearly separate and label different informational boundaries to maintain readability and trust:

* **What Is Known**: The system displays the successfully processed data points, validated tool outputs, and verified facts, ensuring the user has access to all currently available information.5  
* **What Is Unavailable**: The system explicitly lists the failed tool connections, offline databases, or unreadable files, preventing the model from generating speculative or ungrounded answers for these missing components.1  
* **Next Steps and Alternatives**: The interface presents clear, actionable options, allowing the user to retry the failed step manually, adjust their inputs, or escalate the task for human review.5

### **Partial Answer Formulation Model**

| Ingestion / Execution Failure | What Is Known | What Is Unavailable | User Disclosure Prompt | Continuity Action | Retry Safety |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Retrieval Database Timeout** | Any answerable low-risk conceptual material already available in current context. | Fresh source documents, current policy checks, citation verification. | “I can give a limited conceptual answer, but I cannot verify it against live sources right now.” | Preserve query, filters, and evidence requirements for retry. | Safe to retry; no side effects. |
| **Current / Regulated Evidence Unavailable** | The user’s question and required evidence scope. | Verified current answer. | “I can’t safely answer this without current verified sources.” | Offer retry, narrower query, or escalation. | Safe to retry; do not produce speculative answer. |
| **Accounting Tool Crash** | Draft invoice/payment fields already entered and validation status. | Verified execution of the payment or accounting mutation. | “I saved the draft details, but the accounting tool is offline. No payment/update has been confirmed.” | Save validated draft payload and idempotency state. | Unsafe to retry automatically; requires verification or user approval. |
| **Document Parser Failure** | File name, size, type, upload status, and any successfully extracted low-confidence preview. | Verified structured text, tables, layout, or citations. | “Your file is saved, but I couldn’t reliably read its layout.” | Preserve raw upload and parser trace. | Safe to retry; file parsing is read-only. |
| **Partial Tool Workflow Failure** | Completed tool steps and verified results. | Failed, pending, or unverified tool steps. | “Some steps completed; the remaining steps were not executed or could not be verified.” | Save action ledger with completed/pending/failed statuses. | Retry depends on idempotency and verification status. |
| **Human Review Delayed** | Escalation package status and submitted time. | Reviewer decision or approval. | “Your request is waiting for review. I won’t complete the high-impact action until it is approved.” | Preserve escalation packet and notify when status changes. | Not automatically retryable; waits for reviewer or user decision. |

## **Graceful Error State Model**

When fallback chains are exhausted or a task hits a critical safety block, the platform must transition to a terminal error state.10 A graceful error must focus on preserving user orientation and state rather than simply displaying polite apologies.1  
A graceful error state must satisfy five core design requirements:

1. **Explain What Failed**: Use plain, non-technical language to explain what broke, avoiding cryptic system codes or stack traces (e.g., "The payment database is not responding" instead of "Error 503: Connection Refused on Shard 4").1  
2. **Declare What Succeeded**: Confirm which parts of the task were completed successfully and highlight any saved progress to reduce user anxiety.1  
3. **State Save Condition**: Provide a clear, visual indicator (such as a green checkmark) showing that draft inputs, files, and variables were saved securely.7  
4. **Enforce Retry Safety**: Explicitly state whether retrying is safe, preventing duplicate submissions on state-changing actions.1  
5. **Provide Clear Next Steps**: Present 2-3 prominent recovery options, such as retrying with backoff, switching to basic mode, or escalating to human support.1

### **Graceful Error State Matrix**

The platform maps specific system failures to these graceful error patterns:

| Failure Cause | User-Facing Plain Language | Preserved State | Action Status | Retry Safety | Alternative Options | Human Escalation |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Model Outage (HTTP 503)** 1 | "Our core logic-processing system is temporarily over capacity. We've saved your draft." 7 | All typed form fields, chat history, and uploaded files.1 | Blocked; not submitted.1 | Safe to retry; no side effects.1 | "Switch to basic high-speed mode" or "Wait in queue".7 | Automatically route to review queue after 3 failed retries.23 |
| **Rate Limit / Quota Hit (HTTP 429)** 1 | "You've reached your hourly message limit. Your workspace is saved." | All active session variables and draft documents.1 | Throttled; paused. | Safe to retry after the specified reset window.4 | "Use cached search mode" or "View remaining quota".1 | Route session to priority support desk for tier upgrades. |
| **Tool Execution Timeout** 1 | "We could not verify your payment status because the bank's API timed out." 1 | Populated payment arguments and transaction details.1 | Unverified; transaction state is pending.1 | **Unsafe to retry automatically**; risk of duplicate payment.1 | "Check account status manually" or "Draft transaction details".26 | Route the pending transaction to the bank audit queue.19 |
| **Parser Crash on Document** 3 | "We could not read the layout of your scanned document. Your file is saved." 3 | Raw uploaded file and basic metadata.3 | Failed.3 | Safe to retry; read-only action.3 | "Use standard OCR text mode" or "Manually enter key fields".3 | Route document to data entry verification desk.3 |
| **Unsafe Output Blocked** 1 | "Our safety filters blocked this response. Your chat history is preserved." 1 | Safe conversational turns and user settings.1 | Blocked and purged.1 | Unsafe to retry with identical input.1 | "Rephrase your query" or "Review our safety guidelines".1 | Route the blocked interaction flag to trust-and-safety team.1 |

## **Retry UX and Duplicate Action Prevention**

When transient network errors or rate limits disrupt an interaction, backend recovery logic must be aligned with the user interface.4 In traditional web systems, a simple retry loop runs silently in the background. In AI systems, however, retries can consume significant token budgets and, if they involve state-changing tools, can cause duplicate database writes or duplicate financial transactions.1  
To manage this, the system enforces a strict division between interaction types:

* **Idempotent / Read-Only Actions**: Tasks like text generation, database searches, or document parsing carry no risk of side effects.1 The system runs automatic retries using exponential backoff with randomized jitter to desynchronize requests and avoid thundering-herd storms 4:  
  T_wait = 2^attempt * base_delay +/- jitter  
  The UI displays a gentle, non-obtrusive activity indicator, allowing the user to pause or cancel the retry loop at any point.25  
* **Non-Idempotent / State-Changing Actions**: Tasks like processing payments, sending emails, or updating database records must never be retried automatically without strict validation.1 Every write transaction must carry a unique, cryptographically signed idempotency key generated at the client boundary.1 The backend uses this key to deduplicate incoming requests, ensuring that even if a network timeout forces a retry, the transaction is executed exactly once.1

### **Retry UX Model**

| Interaction Type | Idempotency Key Required | UI State Transition | Waiting Indicator | User Interruption Path | Max Retries | Fail-Through Target |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Stateless Text Generation** | No | active -> retrying -> complete or failed | Inline spinner or streaming status. | “Stop generation” cancels remaining attempts. | Profile-specific bounded retries. | Managed error or verified cache only if task-safe. |
| **Read-Only Document Search** | No, but request hash recommended. | active -> searching -> results or limited mode | Skeleton result cards. | “Cancel search” returns to draft/query state. | Bounded retries with backoff. | Local keyword search, narrower query, or retrieval-limit status. |
| **Document Parsing / OCR** | No, but file hash required. | uploaded -> parsing -> preview or failed | File processing indicator. | User can cancel parsing while preserving upload. | Bounded retries on read-only parser paths. | Text-only preview, metadata-only view, or manual entry option. |
| **Stateful CRM / Database Update** | Yes. | draft -> verifying -> committed / failed / unknown | Transaction progress indicator. | User may pause subsequent steps; committed request cannot be casually canceled. | No blind automated retries. | Save draft payload; reconcile state before retry. |
| **Financial Disbursement / Payment** | Yes, signed and scoped. | draft -> authorizing -> submitted -> verified / pending / failed | Full-screen or high-salience transaction state. | User can cancel before authorization; after submission system reconciles state. | No automated retry without idempotency and source-of-record check. | Hold pending state; route to manual verification if unknown. |
| **Email Send / External Message** | Yes for send operation. | draft -> reviewing -> sending -> sent / failed / unknown | Send status with recipient summary. | User can cancel before send; after send verify provider status. | No duplicate send retries without provider/idempotency verification. | Preserve draft and delivery status. |
| **Browser Automation** | Action/session ID required. | observing -> acting -> verifying -> paused / complete | Visible automation status. | “Pause automation” stops future actions. | Bounded retries only after re-observation. | Pause and hand control to user. |

## **Continuity State Model**

When backend services degrade, model routes switch, or an active session is escalated to a human operator, the user must not lose their progress.1 A resilient platform serializes and preserves the complete session state, ensuring that the transition across quality bands is seamless and transparent.16  
The Continuity State Model structures this session metadata across ten distinct categories:

| State Category | Physical Data Schema | Storage Substrate | Lifetime (TTL) | Route-Switch Handling | Preservation Verification |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **User Goal** 1 | {"goal_id": "str", "intent": "str", "parameters": "dict"} | Redis Session Store 42 | 24 Hours 35 | Passed unchanged to fallback model context.1 | Schema match against primary validation templates.1 |
| **Task Plan** 1 | {"steps": [{"step": "int", "status": "enum", "payload": "json"}]} | PostgreSQL state database.1 | 48 Hours | The system pauses the active plan, flags unexecuted blocks, and updates the UI.36 | Plan step hash matches the system execution log.1 |
| **Files & Artifacts** 1 | {"file_id": "uuid", "filename": "str", "storage_path": "str"} | Encrypted Object Storage (MinIO) 3 | 30 Days | Retains file links in the user's workspace; blocks reprocessing.16 | Cryptographic file checksum (SHA-256) match.3 |
| **Evidence & Citations** 1 | {"citation_id": "str", "source_id": "uuid", "coordinates": "array"} | pgvector document registry.3 | 24 Hours | Converts coordinate maps to standard text snippets if visual engines fail.3 | Coordinate overlap checks (Intersection-over-Union >= 0.90).3 |
| **Form Values** | {"form_id": "str", "fields": [{"field": "str", "value": "any"}]} | Redis ephemeral cache 42 | 4 Hours 35 | Retains typed inputs in form elements; marks failed fields.3 | Input validation check against JSON schemas.1 |
| **Draft Work** | {"draft_id": "str", "content_type": "str", "body": "str"} | Redis Session Store 42 | 24 Hours 35 | Retains active text or code drafts; blocks overwrite cycles.26 | Diff validation checks against prior session saves. |
| **Transcripts** 3 | {"session_id": "str", "messages": [{"timestamp": "int", "text": "str"}]} | Redis Time-Series DB 42 | 7 Days | Streaming segments are locked and appended to chat history.3 | Sequence number continuity check on transcript logs.3 |
| **Tool Results** 1 | {"tool_id": "str", "args": "json", "result": "json", "status": "enum"} | PostgreSQL transaction log.1 | 48 Hours | Retains completed tool outputs; skips re-execution on retry.1 | Transaction database commit verification checks.1 |
| **Confirmations** | {"confirm_id": "str", "action_id": "str", "user_approved": "bool"} | PostgreSQL audit tables.1 | Permanent | Clears temporary approval flags if parameters change.1 | Cryptographic signature match on approval tokens.3 |
| **Action Ledger** 1 | {"ledger_id": "str", "transactions": [{"tx_id": "str", "status": "str"}]} | Append-only audit database.1 | Permanent | Locked to prevent modifications; serves as audit source.1 | Hash-chain integrity check across ledger blocks.3 |

## **Human Escalation Packaging Model**

Human escalation must be designed as an active, stateful model route rather than a simple helpdesk redirect.19 When an AI system encounters high-risk uncertainty, a policy block, or repeated failures, it must package and transfer the complete session context.19 This ensures the human reviewer has all necessary information immediately, preventing the user from having to repeat their request from scratch.19  
To protect user privacy and comply with regional regulations (such as GDPR and HIPAA), the escalation engine runs an automated redactor (such as the ARGUS system).1 This engine scans the payload and masks personally identifiable information (PII), payment details, and system credentials before they enter the reviewer interface.1

### **Escalation Package Model**

The structure and data parameters of the escalation package are defined below:

| Packaged Component | Data Type / Format | Operational Purpose | Security / Privacy Isolation | Reviewer UI Representation |
| :---- | :---- | :---- | :---- | :---- |
| **Dialogue History** | JSON Array of Message Objects | Provides full conversational context and user inputs.19 | PII, credentials, and payment details redacted via the ARGUS engine.1 | Chronological chat bubble timeline with highlighted user queries.3 |
| **User Goal** | Structured JSON Object | Establishes the user's primary intent and requested parameters.1 | Field-level column masking on sensitive identifiers.1 | Focus card: "Target Action: Bank Wire Transfer of $500".3 |
| **Attempted Task Plan** | JSON State Array 1 | Illustrates the agent's planned steps and subtasks.1 | None; technical workflow logs only.1 | Step-by-step progress checklist with status indicators.36 |
| **Failed Component Trace** | Standardized Stack Trace JSON | Pinpoints the exact tool, parser, or database failure point.1 | Mask API keys and internal database server paths.1 | Technical diagnostic card showing the exception error.5 |
| **Partial Output** | Markdown Text / JSON String 1 | Displays the successfully generated draft or text components.1 | Redact sensitive entities from generated text blocks.1 | Editable text draft block for manual adjustment.43 |
| **Evidence & Citations** | Bounding Box Coordinate Arrays 3 | Highlights the exact document page regions used for system logic.3 | Access control list filters verify document permissions.1 | Split-screen document viewer with orange bounding highlights.3 |
| **Tool Call Ledger** | Append-Only Transaction Ledger 1 | Shows all completed, pending, and unexecuted API mutations.1 | Scoped tool credentials are redacted and invalidated.1 | Log timeline: "Tool billing_lookup succeeded; payment failed".1 |
| **State Checkpoint** | Serialized Binary / JSON 32 | Allows the human reviewer to resume or modify the active session.32 | Cryptographically signed payload bound to user session.3 | Interactive control panel: "Approve," "Modify," or "Reject".19 |

## **Degraded-Mode Observability**

Maintaining platform reliability requires comprehensive, real-time observability of degraded states.2 When an AI system shifts traffic to fallback routes, serves cached answers, or generates partial responses, platform teams must track these transitions using detailed telemetry.1 This observability prevents silent failures from going unnoticed, where the platform continues responding but at a significantly reduced level of accuracy or utility.5  
Every degraded response must emit a structured trace containing the following metadata parameters:

```json
{
  "trace_id": "TR-99210-A",
  "trigger": "provider_rate_limit",
  "route_before": "primary_reasoning_route",
  "route_after": "approved_efficient_route",
  "degradation_class": "model_degradation",
  "lost_capabilities": [
    "extended_reasoning_depth",
    "full_citation_expansion"
  ],
  "preserved_capabilities": [
    "tenant_scope",
    "safety_policy",
    "schema_validation",
    "session_state"
  ],
  "user_disclosure_shown": true,
  "disclosure_type": "banner_warning",
  "preserved_state_size_bytes": 14202,
  "fallback_status": "degraded_success",
  "recovery_time_ms": 112
}
```

This trace telemetry allows platform SREs to monitor system health and detect service anomalies before they impact the user experience.2

### **Degraded-Mode Observability Metrics**

The platform tracks and evaluates these key reliability metrics:

| Metric Name | Technical Formula | Primary System Source | SLA Alert Threshold |
| :---- | :---- | :---- | :---- |
| **Fallback Rate** | R_fallback = N_fallback_calls / N_total_requests 1 | AI Gateway Routing Logs.29 | > 2.0% of total hourly traffic |
| **Downgrade Rate** | R_downgrade = N_downgraded_sessions / N_total_sessions | AI Gateway Performance Telemetry. | > 5.0% of active daily sessions |
| **Cache Hit Rate** | R_cache = N_cache_hits / N_total_requests 1 | Redis Semantic Cache Logs.15 | Target: 20% to 30% on repeat volumes 14 |
| **Stale Cache Rate** | R_stale = N_stale_cache_hits / N_total_requests | Redis Cache TTL Metadata.40 | > 1.0% of total requests |
| **Partial Answer Rate** | R_partial = N_partial_responses / N_total_responses | Dialogue State Machine Logs.17 | > 3.0% of multi-agent runs |
| **Retry Count** | C_retries = sum(attempts_per_request) 31 | AI Gateway Outbound Logs.31 | > 2 average attempts per failure |
| **Retry Success Rate** | R_retry_success = N_resolved_retries / N_total_retry_attempts 1 | AI Gateway Exception Tracker.31 | < 95.0% of retried transactions |
| **Graceful Error Rate** | R_graceful_err = N_graceful_errors / N_total_system_errors | Client-side Error Handler Logs.7 | Target: 100.0% of terminal failures |
| **User Abandonment** | R_abandon = N_abandoned_sessions_on_degrade / N_total_degraded_sessions | Client Web Analytics / Session Logs.21 | > 8.0% of degraded interactions |
| **Session Resume Rate** | R_resume = N_resumed_sessions_post_degrade / N_total_degraded_sessions 1 | PostgreSQL Session State Registry.1 | < 90.0% of interrupted sessions |
| **Escalation Rate** | R_escalation = N_escalations / N_total_sessions 1 | Human-in-the-Loop Queue Database.21 | > 5.0% of active sessions 1 |
| **State Preservation Success** | A_state = N_successful_state_restorations / N_total_session_resumptions | PostgreSQL Session State Registry.1 | < 100.0% of session resumptions |
| **Recovery Time (MTTR)** | T_recovery = t_healthy_state - t_degradation_onset | Prometheus System Monitors.34 | > 15 minutes recovery window |

## **Degraded-Mode Test Matrix**

To verify fallback logic and degraded states under pressure, teams should run degraded-mode and chaos tests in staging. Tests should assert not only that the backend recovers, but that the user experience remains honest, state-preserving, and safe.

| Simulated Failure | Injected Chaos Trigger | Expected System Behavior | UX Assertion Target | Verification Tooling |
| :---- | :---- | :---- | :---- | :---- |
| **Primary Model Outage** | Mock 503/timeout from primary route. | Gateway moves only to an approved fallback satisfying the task profile. | UI preserves chat state and discloses degradation if capability changed. | Gateway chaos test / provider mock. |
| **Equivalent Route Unavailable** | Disable all capability-equivalent fallback models. | System blocks or moves to disclosed degraded mode; no silent unsafe downgrade. | User sees limitation and safe next options. | Routing policy test. |
| **Retrieval Database Failure** | Block vector/keyword DB or induce timeout. | System uses verified cache, narrower retrieval, or limited conceptual mode depending on risk. | Citations are hidden or marked unavailable; no unsupported grounded claim. | Network fault injection / retrieval mock. |
| **State-Changing Tool Timeout** | Delay tool response past timeout. | Orchestrator marks action as pending/unknown and prevents duplicate retry. | UI shows unverified state and locks unsafe resubmission. | Tool gateway latency injection. |
| **Document Parser Crash** | Upload malformed/corrupted file. | Parser sandbox fails safely; raw upload and forensic trace are preserved per policy. | UI shows file saved and parser limitation; no hallucinated extraction. | Parser fuzzing harness. |
| **Semantic Cache Stale State** | Seed cache with expired or old-policy answer. | Cache is blocked or served only with timestamp/freshness disclosure if task-safe. | UI displays freshness warning or requests refresh. | Cache TTL/source-version test. |
| **Upstream Rate Limit** | Inject 429 with `Retry-After`. | Gateway backs off, queues, or returns managed capacity status. | UI shows cooldown/queue state and preserves draft. | Provider mock / proxy fault test. |
| **Tenant Quota Exhaustion** | Set tenant budget/token bucket to zero. | Gateway blocks new cost-incurring calls and preserves active inputs. | UI shows quota warning and recovery options. | Quota service test. |
| **Review Queue Saturated** | Simulate full human-review queue. | High-impact decisions remain paused; low-risk cases use triage only if policy allows. | UI shows review delay and preserved escalation package. | Queue load test. |
| **Cache Scope Mismatch** | Attempt cache lookup with different tenant/user/policy scope. | Cache refuses hit and recomputes or fails safely. | No cross-scope data appears in UI. | Cache isolation regression test. |
| **UI-Agent Drift** | Change DOM between observe and click. | Agent re-observes, re-plans, or pauses. | UI shows automation paused; no blind click occurs. | Browser automation chaos test. |
| **Voice Degradation** | Inject noisy audio or STT instability. | System switches to confirmation/text fallback for important fields. | User is told voice capture is unreliable; task state preserved. | Audio/STT fault simulation. |

## **Cross-Canon Handoff Map**

The UX resilience architecture establishes user-facing degraded states, fallback contracts, continuity-state requirements, retry UX, partial answer policy, graceful error handling, and escalation packaging. These patterns connect broadly across the canon because degraded mode is where backend failures become user-visible product behavior.

| Target Canon Report | Domain Area | Core Dependency | Operational Rule | Fallback Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context Tenure & State Governance | Continuity state, task state, memory scope, compaction. | Route switches must preserve active constraints, approvals, and unresolved state. | Save/restore scoped session state before changing route. |
| **AI-ENG-E** | Retrieval Pipeline | Retrieval fallback, citation availability, cache eligibility. | Degraded retrieval must disclose missing freshness or citation support. | Use verified cache, lexical fallback, narrower query, or managed no-evidence response. |
| **AI-ENG-F** | Freshness & Conflict Detection | Stale-answer labeling and source-version checks. | Cached/degraded answers must preserve freshness and conflict status. | Block stale high-impact answers or require refresh/review. |
| **AI-ENG-L** | Serving Architecture | Model routing, provider failover, gateway status. | Backend fallback is not user success unless quality floors are preserved. | Route to equivalent model, degraded route, cache, partial answer, review, or fail-closed. |
| **AI-ENG-M** | Agentic Orchestration | Loop state, task plans, partial completion. | Degraded mode must not lose plan state or duplicate agent actions. | Pause, replan, serialize state, or escalate. |
| **AI-ENG-N** | Tool Contracts | Tool failure, schema mismatch, scoped credentials. | Tool degradation must preserve arguments, idempotency, and verification status. | Save draft, block unsafe retry, reconcile unknown state. |
| **AI-ENG-O** | Action Verification | Post-action status, unknown/pending/failed state. | User-facing success requires verified state. | Show pending/unknown status; hold, reconcile, compensate, or escalate. |
| **AI-ENG-P** | Multimodal Understanding | Parser/VLM degradation, evidence adequacy. | If visual/layout evidence is unavailable, the UI must disclose what was not inspected. | Text-only preview, sampled evidence, manual verification, or fail safely. |
| **AI-ENG-Q** | Speech and Realtime Interaction | Voice fallback, confirmation, transcript continuity. | Voice degradation must not force high-impact action through unreliable audio. | Switch to text/card/keypad confirmation or live handoff. |
| **AI-ENG-R** | UI Agents | Automation pause, drift handling, user handoff. | UI-agent fallback must not blind-click or rely on stale coordinates. | Re-observe, re-plan, pause, or hand control to user. |
| **AI-ENG-S** | Production Pathologies | False success, malformed output, brittle chains. | Degraded UX must prevent production failures from appearing as success. | Surface partial/failed/unknown states honestly. |
| **AI-ENG-T** | Boundary Defense | Tenant scope, cache scope, prompt-injection resistance. | Fallback must not bypass authorization, cache isolation, or policy boundaries. | Fail closed when security scope cannot be preserved. |
| **AI-ENG-U** | Supply Chain Security | Parser/tool/model dependency degradation. | Fallback components must be approved, isolated, and observable. | Use approved fallback components or block. |
| **AI-ENG-V** | Resource Abuse | Quota degradation, rate limits, budget-aware routing. | Resource limits should become clear UX states, not unexplained failure. | Queue, throttle, degrade, or show managed capacity status. |
| **AI-ENG-X** | User Trust & Transparency | Status language, disclosures, evidence visibility. | Users must know what changed, what is saved, and what remains unsafe/unavailable. | Plain-language degraded-mode cards and freshness/status labels. |
| **AI-ENG-Y** | Human Review & Approval | Escalation packages, reviewer workflows. | Human escalation must transfer enough scoped context to avoid forcing user restart. | Redacted escalation package with evidence IDs and action ledger. |
| **AI-ENG-Z** | Telemetry and Metrics | Degraded-mode events and user-visible incident metrics. | Every fallback/degraded state emits structured telemetry. | Alert on fallback spikes, lost-state rates, or silent degradation. |
| **AI-ENG-AA** | Reliability Evaluations | Degraded-mode regression tests. | Releases must test fallback chains, cache freshness, partial answers, and retry safety. | Block releases on unsafe degradation regressions. |
| **AI-ENG-AB** | Audit & Replay Debugging | Fallback trace, state checkpoint, route manifest. | Degraded sessions must be replayable from trace and state artifacts. | Preserve route decisions, disclosure state, and continuity checkpoints. |
| **AI-ENG-AC** | Incident Response Protocols | User-visible incidents, degraded-mode comms. | Incidents require user-facing status, containment, recovery, and trust repair. | Escalate to incident playbook when degradation exceeds threshold. |
| **AI-ENG-AD** | Governance & Accountability | Policy for disclosure, fallback eligibility, and user choice. | Governance defines which degraded states need disclosure, consent, review, or blocking. | Route policy exceptions to accountable owner. |
| **AI-ENG-AJ** | Reference Architectures | Resilience gateway, state store, fallback manifest, review queue. | Reference systems should include degraded-mode UX as a first-class architecture component. | Implement declarative fallback contracts and state-preserving error flows. |

## **Strategic Conclusions and Architectural Recommendations**

Building a resilient user experience in production AI environments requires a fundamental shift in engineering posture.1 Probabilistic systems fail in complex, non-deterministic ways that traditional web infrastructure is not designed to contain.1 Platforms that rely on system prompts or inline instructions to control resource limits, formatting compliance, or safety boundaries remain highly vulnerable to context dilution, prompt injection, and catastrophic session crashes.1 True resilience is achieved by enforcing strict software controls, state-verification loops, and budget limits entirely outside the model's execution path.3  
To implement a robust, production-grade UX Resilience architecture, organizations should adopt the following strategic principles:

* **Enforce State Verification Before Confirmations**: The dialogue coordinator must never generate verbal or text-based confirmations (e.g., "Your payment has been sent") until the underlying API transaction has been successfully committed and verified inside the database of record.3 Spoken or generated claims must never outrun physical reality.3  
* **Consolidate Resilience at the Gateway Layer**: Avoid writing separate retry, fallback, and rate-limiting logic inside individual application services.10 Centralize these capabilities within a high-performance, self-hostable AI gateway (such as Bifrost or LiteLLM) to ensure consistent policy enforcement, cost tracking, and provider failovers across the entire organization.4  
* **Secure Caching Layers Against Timing Side-Channels**: Sharing semantic or prefix caches globally across mutually untrusted tenants introduces timing side-channel risks.37 All cache keys must be cryptographically bound to tenant identity and user permissions, and serving runtimes must deploy selective prefix isolation (CacheSolidarity) to block adversarial timing probes.37  
* **Enforce Strict Least-Privilege Tool Credentials**: Model-driven agents must never run with broad administrative service tokens or "god-mode" database accounts.3 Every tool invocation must be mediated by a secure credential broker that validates user identity and mints short-lived, highly restricted OAuth tokens (validity < 900 seconds) specifically for that single execution.3  
* **Treat Escalation and Partial States as First-Class Workflows**: Human-in-the-loop review and partial answer delivery are not system exceptions; they are designed, stateful product phases.32 Ensure every escalation is accompanied by a structured evidence package, preserving the user's progress and orientation without forcing them to restart their journey.19

## **Durable Principles of UX Resilience**

1. **Fallback Is Not Success**  
   A fallback succeeds only when it preserves the task’s safety, quality, state, privacy, evidence, and user expectations.

2. **Degraded Mode Must Be Designed, Not Discovered**  
   Users should not experience degraded capability as random weirdness. Degraded states need labels, continuity, safe options, and clear next steps.

3. **Silent Routing Is Allowed Only for Equivalent Capability**  
   If the fallback changes quality, freshness, latency, cost, evidence, or action authority, the user or UI needs to know.

4. **Never Trade Safety for Availability**  
   Tenant isolation, privacy, authorization, action verification, and consent are non-sacrificable. A system that stays “up” by dropping those is not resilient. It is just failing with jazz hands.

5. **Preserve State Before Changing Route**  
   Route switches, retries, partial answers, and escalations must preserve user goal, active constraints, files, drafts, approvals, and action ledger state.

6. **Unknown Action State Must Be Shown as Unknown**  
   Tool timeouts, payment uncertainty, browser crashes, and partial commits must not become conversational success claims.

7. **Cached Answers Require Scope, Freshness, and Disclosure**  
   Cache reuse must respect tenant/user scope, source version, policy version, and task risk. Stale cache must be labeled or blocked.

8. **Partial Answers Must Preserve Boundaries**  
   A partial answer should clearly separate what is known, unavailable, failed, pending, unverified, and safe to retry.

9. **Human Escalation Is a Product State**  
   Escalation should transfer scoped, redacted, actionable context—not dump raw chat history into a queue and call it “support.”

10. **Graceful Errors Should Reduce User Work**  
   A graceful error tells the user what failed, what succeeded, what was saved, whether retry is safe, and what options remain.

11. **Retry UX Must Prevent Duplicate Harm**  
   Read-only retries may be automated within limits. State-changing retries require idempotency, verification, and sometimes explicit user approval.

12. **Degradation Must Be Observable**  
   Every fallback, cache response, partial answer, retry sequence, fail-closed event, and escalation should emit traceable telemetry.

#### **Works cited**

1. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
2. What is graceful degradation? Meaning, Examples, Use Cases & Complete Guide?, accessed June 11, 2026, [https://www.devopsschool.nl/graceful-degradation/](https://www.devopsschool.nl/graceful-degradation/)  
3. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
4. LLM Failover & Load Balancing for Provider Outages - Truefoundry, accessed June 11, 2026, [https://www.truefoundry.com/blog/llm-failover-load-balancing-provider-outages](https://www.truefoundry.com/blog/llm-failover-load-balancing-provider-outages)  
5. After months with Claude Code, the biggest time sink isn't bugs — it's silent fake success, accessed June 11, 2026, [https://www.reddit.com/r/ClaudeAI/comments/1sdmohb/after_months_with_claude_code_the_biggest_time/](https://www.reddit.com/r/ClaudeAI/comments/1sdmohb/after_months_with_claude_code_the_biggest_time/)  
6. Adaptive Model Routing and Fallback Logic: Routing Around LLM Provider Outages with Bifrost - DEV Community, accessed June 11, 2026, [https://dev.to/kuldeep_paul/adaptive-model-routing-and-fallback-logic-routing-around-llm-provider-outages-with-bifrost-4g3m](https://dev.to/kuldeep_paul/adaptive-model-routing-and-fallback-logic-routing-around-llm-provider-outages-with-bifrost-4g3m)  
7. Error Recovery & Graceful Degradation - Free AI UX Audit Tool, accessed June 11, 2026, [https://www.aiuxdesign.guide/patterns/error-recovery](https://www.aiuxdesign.guide/patterns/error-recovery)  
8. Routing, Cascades, and User Choice for LLMs - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2602.09902](https://arxiv.org/pdf/2602.09902)  
9. [2602.09902] Routing, Cascades, and User Choice for LLMs - arXiv, accessed June 11, 2026, [https://arxiv.org/abs/2602.09902](https://arxiv.org/abs/2602.09902)  
10. Rate Limiting AI Agents: Preventing LLM API Exhaustion with a 3-Layer Gateway, accessed June 11, 2026, [https://www.truefoundry.com/fr/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion](https://www.truefoundry.com/fr/blog/rate-limiting-ai-agents-preventing-llm-api-exhaustion)  
11. Dynamic Model Routing and Cascading for Efficient LLM Inference: A Survey - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2603.04445](https://arxiv.org/pdf/2603.04445)  
12. Dynamic Model Routing and Cascading for Efficient LLM Inference: A Survey - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2603.04445v2](https://arxiv.org/html/2603.04445v2)  
13. LLM Routing and Model Cascades: How to Cut AI Costs Without Sacrificing Quality, accessed June 11, 2026, [https://tianpan.co/blog/2025-11-03-llm-routing-model-cascades](https://tianpan.co/blog/2025-11-03-llm-routing-model-cascades)  
14. Semantic Caching and Intent-Driven Context Optimization for Multi-Agent Natural Language to Code Systems A Production Study in Enterprise Analytics - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2601.11687v1](https://arxiv.org/html/2601.11687v1)  
15. Speed, Context, and Savings: Mastering Caching in the Capella AI Model Service, accessed June 11, 2026, [https://www.couchbase.com/blog/speed-context-and-savings-mastering-caching-in-the-capella-ai-model-service/](https://www.couchbase.com/blog/speed-context-and-savings-mastering-caching-in-the-capella-ai-model-service/)  
16. 100 Tips & Tricks for Building Your Own Personal AI Agent | by Shubh Jain - Stackademic, accessed June 11, 2026, [https://blog.stackademic.com/100-tips-tricks-for-building-your-own-personal-ai-agent-a05468c68473](https://blog.stackademic.com/100-tips-tricks-for-building-your-own-personal-ai-agent-a05468c68473)  
17. What Is the ReAct Loop? How AI Agents Reason, Act, and Iterate Toward a Goal, accessed June 11, 2026, [https://www.mindstudio.ai/blog/what-is-react-loop-ai-agents-reason-act-iterate](https://www.mindstudio.ai/blog/what-is-react-loop-ai-agents-reason-act-iterate)  
18. Retries, Fallbacks, and Circuit Breakers in LLM Apps: A Production Guide - Maxim AI, accessed June 11, 2026, [https://www.getmaxim.ai/articles/retries-fallbacks-and-circuit-breakers-in-llm-apps-a-production-guide/](https://www.getmaxim.ai/articles/retries-fallbacks-and-circuit-breakers-in-llm-apps-a-production-guide/)  
19. The Human-in-the-Loop (HITL) Pattern for Voice Agents | LiveKit, accessed June 11, 2026, [https://livekit.com/blog/human-in-the-loop-voice-agents](https://livekit.com/blog/human-in-the-loop-voice-agents)  
20. 7 Leading Enterprise Platforms for Hybrid AI-Human Support Operations [2026 Comparison], accessed June 11, 2026, [https://www.usefini.com/guides/leading-enterprise-platforms-hybrid-ai-human-support-operations](https://www.usefini.com/guides/leading-enterprise-platforms-hybrid-ai-human-support-operations)  
21. Human-in-the-loop AI in CX explained - Parloa, accessed June 11, 2026, [https://www.parloa.com/knowledge-hub/human-in-the-loop-ai/](https://www.parloa.com/knowledge-hub/human-in-the-loop-ai/)  
22. The Dependency Trap: Why Modern Software Fails Through Third-Party Fragility - Medium, accessed June 11, 2026, [https://medium.com/@Iyanudavid/the-dependency-trap-why-modern-software-fails-through-third-party-fragility-2d307dfd9fbd](https://medium.com/@Iyanudavid/the-dependency-trap-why-modern-software-fails-through-third-party-fragility-2d307dfd9fbd)  
23. Cascaded Language Models for Cost-Effective Human–AI Decision-Making - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2506.11887v3](https://arxiv.org/html/2506.11887v3)  
24. How to Handle Graceful Service Degradation with Istio - OneUptime, accessed June 11, 2026, [https://oneuptime.com/blog/post/2026-02-24-how-to-handle-graceful-service-degradation-with-istio/view](https://oneuptime.com/blog/post/2026-02-24-how-to-handle-graceful-service-degradation-with-istio/view)  
25. Degraded experiences - Primer, accessed June 11, 2026, [https://primer.style/ui-patterns/degraded-experiences](https://primer.style/ui-patterns/degraded-experiences)  
26. How to Build a Vibe Coding Chatbot with AI in 2026 | QuantumByte Success Story, accessed June 11, 2026, [https://quantumbyte.ai/articles/how-to-build-a-vibe-coding-chatbot-with-ai-in-2026](https://quantumbyte.ai/articles/how-to-build-a-vibe-coding-chatbot-with-ai-in-2026)  
27. Best Enterprise AI Gateway for Intelligent Routing - Maxim AI, accessed June 11, 2026, [https://www.getmaxim.ai/articles/best-enterprise-ai-gateway-for-intelligent-routing/](https://www.getmaxim.ai/articles/best-enterprise-ai-gateway-for-intelligent-routing/)  
28. Top 5 LLM Routing Techniques - Maxim AI, accessed June 11, 2026, [https://www.getmaxim.ai/articles/top-5-llm-routing-techniques/](https://www.getmaxim.ai/articles/top-5-llm-routing-techniques/)  
29. LLM Gateway Architecture: 2026 Engineering Reference - Digital Applied, accessed June 11, 2026, [https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference](https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference)  
30. Ask HN: What's your biggest LLM cost multiplier? - Hacker News, accessed June 11, 2026, [https://news.ycombinator.com/item?id=46838390](https://news.ycombinator.com/item?id=46838390)  
31. Retries and Fallbacks - Orq.ai Documentation - AI Gateway & LLM Collaboration Platform, accessed June 11, 2026, [https://docs.orq.ai/docs/proxy/retries](https://docs.orq.ai/docs/proxy/retries)  
32. AI Human in the Loop: Production Oversight Patterns - Redis, accessed June 11, 2026, [https://redis.io/blog/ai-human-in-the-loop/](https://redis.io/blog/ai-human-in-the-loop/)  
33. LLM Cost Optimization Techniques: A Production Playbook, accessed June 11, 2026, [https://bigdataboutique.com/blog/llm-cost-optimization-techniques](https://bigdataboutique.com/blog/llm-cost-optimization-techniques)  
34. Comprehensive Tutorial on Graceful Degradation in Site Reliability Engineering, accessed June 11, 2026, [https://sreschool.com/blog/comprehensive-tutorial-on-graceful-degradation-in-site-reliability-engineering/](https://sreschool.com/blog/comprehensive-tutorial-on-graceful-degradation-in-site-reliability-engineering/)  
35. Best practices - Amazon ElastiCache, accessed June 11, 2026, [https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/semantic-caching-best-practices.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/semantic-caching-best-practices.html)  
36. Agent UX Patterns: Chat-First UX Fails. Use These Patterns Instead - HatchWorks AI, accessed June 11, 2026, [https://hatchworks.com/blog/ai-agents/agent-ux-patterns/](https://hatchworks.com/blog/ai-agents/agent-ux-patterns/)  
37. CacheSolidarity: Preventing Prefix Caching Side Channels in Multi-tenant LLM Serving Systems - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2603.10726v1](https://arxiv.org/html/2603.10726v1)  
38. CacheSolidarity: Preventing Prefix Caching Side Channels in Multi-tenant LLM Serving Systems - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2603.10726](https://arxiv.org/pdf/2603.10726)  
39. PrefixWall: Mitigating Prefix Caching Side Channels in Shared LLM Systems - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2603.10726v2](https://arxiv.org/html/2603.10726v2)  
40. Implementing a semantic cache with ElastiCache for Valkey - AWS Documentation, accessed June 11, 2026, [https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/semantic-caching-implementation.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/semantic-caching-implementation.html)  
41. Advanced Error Handling and Retry Patterns in Enterprise REST Integrations - DZone, accessed June 11, 2026, [https://dzone.com/articles/rest-error-retry-patterns](https://dzone.com/articles/rest-error-retry-patterns)  
42. redis-ai-resources/python-recipes/semantic-cache/03_context_enabled_semantic_caching.ipynb at main - GitHub, accessed June 11, 2026, [https://github.com/redis-developer/redis-ai-resources/blob/main/python-recipes/semantic-cache/03_context_enabled_semantic_caching.ipynb](https://github.com/redis-developer/redis-ai-resources/blob/main/python-recipes/semantic-cache/03_context_enabled_semantic_caching.ipynb)  
43. What is human in the loop (HITL) in AI? Definition & examples | Decagon, accessed June 11, 2026, [https://decagon.ai/glossary/what-is-human-in-the-loop-hitl](https://decagon.ai/glossary/what-is-human-in-the-loop-hitl)  
44. Building Conversational Analytics on Databricks: What Survives Production and What Doesn't | by Himanshu Gaurav | Towards Data Experience | Medium, accessed June 11, 2026, [https://medium.com/towards-data-experience/building-ai-analytics-on-databricks-what-survives-production-and-what-doesnt-176fc5f54aec](https://medium.com/towards-data-experience/building-ai-analytics-on-databricks-what-survives-production-and-what-doesnt-176fc5f54aec)  
45. Google SRE lessons - key principles of site reliability engineering, accessed June 11, 2026, [https://sre.google/resources/practices-and-processes/twenty-years-of-sre-lessons-learned/](https://sre.google/resources/practices-and-processes/twenty-years-of-sre-lessons-learned/)  
46. Best AI Gateway to Route Codex CLI to Any Model - Maxim AI, accessed June 11, 2026, [https://www.getmaxim.ai/articles/best-ai-gateway-to-route-codex-cli-to-any-model/](https://www.getmaxim.ai/articles/best-ai-gateway-to-route-codex-cli-to-any-model/)

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