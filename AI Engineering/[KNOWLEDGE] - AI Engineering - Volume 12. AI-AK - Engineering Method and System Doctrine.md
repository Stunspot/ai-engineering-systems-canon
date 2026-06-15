# [KNOWLEDGE] - AI Engineering - Volume 12. AI-AK - Engineering Method and System Doctrine

[Volume 12. AI-AK - Engineering Method and System Doctrine]
  - [AI-ENG-AI — Contract Thinking - Deterministic Edges Around Probabilistic Cores](#ai-eng-ai--contract-thinking---deterministic-edges-around-probabilistic-cores)
  - [AI-ENG-AJ — AI System Design Patterns - Reference Architectures & Failure-Aware Blueprints](#ai-eng-aj--ai-system-design-patterns---reference-architectures--failure-aware-blueprints)
  - [AI-ENG-AK — The AI Engineering Mindset - Probabilistic Systems, Human Judgment & Iterative Control](#ai-eng-ak--the-ai-engineering-mindset---probabilistic-systems-human-judgment--iterative-control)

---

# AI-ENG-AI — Contract Thinking - Deterministic Edges Around Probabilistic Cores

## **Doctrinal Introduction & The Contract Thinking Doctrine**

In high-dimensional artificial intelligence engineering, the primary challenge is not the elimination of model non-determinism, but rather the containment of that non-determinism within safe, verifiable boundaries. This paradigm, formulated as probabilistic containment architecture, recognizes that the cognitive flexibility of large language models is highly valuable during internal inference, but presents severe risks when allowed to interface directly with stateful, authoritative, or public-facing systems. Good systems design does not attempt to force the entire model into absolute predictability; instead, it establishes a rigid, deterministic envelope around the model's probabilistic core. The model may execute complex synthesis, logical deduction, and creative planning within this bounded space, but every boundary crossing—or seam—must conform to a strict, typed, and enforceable contract.  
This methodology generalizes Bertrand Meyer’s Design by Contract (DbC) framework, originally formulated in 1986 to improve traditional software reliability.1 Meyer identified correctness—the ability of software to perform its exact tasks as defined by its specification—as the supreme quality of software, supplemented by robustness, efficiency, extendibility, reusability, and compatibility.3 While defensive programming suggests adding redundant checks everywhere, it leads to code complexity—the primary obstacle to software quality.2 DbC advocates an "offensive programming" posture: establish clear preconditions, postconditions, and invariants, and "fail hard" immediately when assertions are violated, simplifying debugging and ensuring correctness.1  
Applying the Liskov Substitution Principle to AI contract boundaries establishes clear constraints on function execution 4:

* **Preconditions cannot be strengthened:** An implementation must not accept a narrower range of input than the interface specification dictates.4  
* **Postconditions cannot be weakened:** An implementation must not return a wider, more ambiguous range of output than the specification guarantees.4  
* **Invariants cannot be weakened:** An implementation must maintain core system state within specified tolerances throughout execution.4

In AI engineering, the model is inherently probabilistic. We cannot make the model itself fully deterministic. The useful question is: *Can we make every boundary around the model explicit, enforceable, observable, testable, and safe to breach-handle?* Inside the box, the model operates with probabilistic freedom; at the edges, it must sign deterministic paperwork.  
To establish this discipline, systems architects must maintain clean distinctions across multiple operational dimensions:

* **Flexibility versus Authority:** The generative core is permitted to reason, draft, and propose, but it holds zero administrative authority. Any execution of state changes, data mutation, or outbound communication must be mediated by a deterministic runtime that validates permissions and rules before execution.5  
* **Schema Validity versus Truth:** A model output conforming perfectly to a JSON schema is structurally valid, but syntactic correctness does not imply factual accuracy. Structuring constraints ensure format compliance; separate semantic and factual verifications are required to confirm truth.7  
* **Prompt Instruction versus Enforceable Boundary:** Instructions embedded within prompts (e.g., "never reveal system keys") are instructions, not security controls.6 Actual boundaries must be enforced by isolated software filters, input-scrubbing routines, and policy-as-code sidecars that are completely outside the model's context window.  
* **Model Capability versus System Permission:** A model’s computational ability to generate a tool invocation, generate SQL, or draft a financial transaction must never be confused with systemic authorization to perform that action.5  
* **Generated Action versus Authorized Action:** Actions drafted by the model are treated as unauthenticated proposals. They are only translated into physical side effects after passing through a deterministic authorization engine (such as Open Policy Agent or Cedar).6  
* **Grounding versus Citation-Shaped Decoration:** The presence of inline footnotes and citation links in a generated output often serves as a persuasive UX element rather than factual proof.8 Rigid grounding contracts must programmatically verify that claims are supported by retrieved sources, with proper temporal and logical qualifiers.7  
* **Memory versus Surveillance:** Persistent agentic memory must not become an unconstrained data sink. It requires strict tenant isolation, local pseudonymization, automatic retention rules, and user-controlled deletion paths to prevent liability and privacy leakage.9  
* **Eval Score versus Release Contract:** An eval is a formal release gate, not a subjective vibe check.14  
* **Logs versus Compliance Evidence:** Raw diagnostic logs track errors; immutable, cryptographically signed events serve as compliance evidence.11  
* **User Trust versus User Expectation Contract:** UX must cultivate calibrated trust, ensuring users rely on the system when correct and override it when it fails.17  
* **Vendor Route versus Model-Route Contract:** Gateway abstraction maintains system stability across changing provider deployments.20  
* **Fallback versus Contract Downgrade:** Fault tolerance must preserve safety properties even when dropping back to less capable local models.20

## **Conceptual Glossary**

The following standardized terms form the core vocabulary of the contract thinking doctrine and are defined for uniform engineering application:

| Term | Doctrinal Definition |
| :---- | :---- |
| **Contract Thinking** | The engineering practice of placing deterministic, typed, versioned, enforceable, observable, and testable interfaces around probabilistic model behavior at every architectural seam. |
| **Probabilistic Core** | The internal non-deterministic execution domain of a generative model where token generation, reasoning, and conceptual synthesis occur. |
| **Deterministic Edge** | The rigid, software-enforced boundary surrounding a probabilistic core that validates inputs, filters outputs, enforces budgets, checks permissions, and handles failures. |
| **Contract Surface** | Any distinct architectural seam where model outputs or inputs interface with structured systems, human users, external APIs, or storage. |
| **Contract Stack** | The layered hierarchy of active agreements (from user expectations to deployment manifests) that must remain aligned to prevent silent system failures. |
| **Schema Contract** | A programmatically enforced definition of output structure, typing, and constraints, typically implemented via constrained decoding at the token level.22 |
| **Prompt Contract** | A versioned, testable deployment configuration that formalizes the task boundary, instruction hierarchy, and input/output expectations of a model step. |
| **Retrieval Contract** | An agreement governing context injection, specifying source authority, permission filtering, freshness tolerances, and citation requirements.14 |
| **Tool Contract** | A highly specific API interface that maps natural language intents to deterministic code execution, enforcing schema validation, auth, and idempotency.5 |
| **Permission Contract** | A zero-trust authorization boundary that separates model-generated proposals from system-level action execution based on the underlying user's identity.6 |
| **Resource Contract** | A set of hard runtime constraints governing execution envelopes, including token budgets, call limits, latency ceilings, and cost budgets.16 |
| **Memory Contract** | Rules governing long-term writable state, defining write validation, retention periods, read scoping, and on-device privacy mapping.12 |
| **Model Route Contract** | The operational envelope defining which task classes and risk tiers are mapped to specific models, providers, and fallbacks.21 |
| **Eval Contract** | The quantifiable release gate specifying the performance benchmarks, regression thresholds, and behavioral tests a configuration must pass to ship. |
| **Deployment Contract** | A cryptographically signed or version-pinned bundle containing prompts, schemas, routing policies, and verification artifacts. |
| **Observability Contract** | The schema defining tracing spans, telemetry events, redacted logs, and compliance evidence emitted during system execution.16 |
| **User Expectation Contract** | The user interface patterns and disclosure frameworks that align human trust with actual system competence, preventing automation bias.17 |
| **Breach Behavior** | The deterministic error-handling pipeline executed immediately when any contract boundary is violated at runtime. |
| **Contract Drift** | A progressive misalignment between layered contracts (e.g., a product promise exceeding model capability) that leads to silent, systemic failure. |

## **The Contract Stack Model & Drift Diagnostic**

AI systems fail when contract layers silently diverge. A product promise may imply current policy compliance, while retrieval pulls stale documents. A schema may require citations, while the citation verifier only checks that a field exists. A UI may show an action as complete, while the downstream transaction failed. Contract thinking prevents these failures by aligning every layer from user expectation to deployment evidence.

```text
CONTRACT STACK

[ User Expectation Contract ]
        |
[ Product / Workflow Contract ]
        |
[ Policy / Permission Contract ]
        |
[ Prompt / Context Contract ]
        |
[ Retrieval / Memory Contract ]
        |
[ Model Route Contract ]
        |
[ Schema / Output Contract ]
        |
[ Tool / Action Contract ]
        |
[ Eval / Verification Contract ]
        |
[ Deployment / Observability Contract ]
```

Each layer must be supported by the layers beneath it. The user interface must not promise a capability the product workflow cannot verify. The prompt must not request evidence the retrieval system cannot supply. The model route must not be assigned to a task class it has not passed. The tool contract must not execute an action the permission contract has not authorized.

### **Contract Drift Diagnostic**

| Drift Pattern | Detection Signal | Safe Runtime Response | Structural Owner | Preventive Practice |
| :---- | :---- | :---- | :---- | :---- |
| **User Promise vs. Product Capability** | Users complain that the system did not do what the interface implied. | Downgrade claim, add disclosure, route to human or safer workflow. | Product Owner | Product promises must map to tested capabilities and known limits. |
| **Product Workflow vs. Model Route** | Complex/high-risk tasks routed to low-capability or low-cost model. | Re-route to approved model class or block with explanation. | Product Architect / Platform Owner | Route manifests tied to task class, risk tier, and eval evidence. |
| **Prompt vs. Retrieval** | Prompt asks for current/grounded answer but retrieval lacks current or authoritative sources. | Refuse, ask for source, or disclose unsupported status. | Prompt / Retrieval Owner | Prompt context requirements must be checked against corpus metadata. |
| **Retrieval vs. Freshness** | Retrieved source is stale, superseded, or outside time scope. | Remove stale source, rerun retrieval, disclose conflict or absence. | Corpus / Retrieval Owner | Freshness metadata, source-of-record hierarchy, index lifecycle controls. |
| **Schema vs. Truth** | Output passes schema validation but fails citation, math, or policy checks. | Block downstream use; send to factual/semantic repair or human review. | Eval / Verification Owner | Multi-layer validation beyond JSON shape. |
| **Schema vs. Tool Contract** | Model emits structurally valid arguments that violate tool preconditions. | Deny tool call; return structured breach reason. | Tool Owner | Shared typed schemas, preconditions, and postcondition tests. |
| **Permission vs. Tool Execution** | Tool call authorized by prompt language but not by user identity, role, tenant, or resource policy. | Block before side effect; log policy decision. | Security / Governance Owner | External policy engine, ABAC/RBAC, user-bound delegation. |
| **Fallback vs. Risk Tier** | Failover route has lower capability, weaker policy, or different data handling. | Degrade authority, disable actions, or route to human. | Platform SRE | Fallbacks must preserve safety properties, not just availability. |
| **Memory vs. Authority** | Memory from prior sessions influences a task outside active scope. | Restrict memory read, clear active memory, or require user confirmation. | Memory / Privacy Owner | Principal-scoped memory, provenance tags, retention and deletion rules. |
| **Eval vs. Production Reality** | Offline evals pass but production corrections, incidents, or complaints rise. | Freeze rollout, sample production failures, update eval set. | Evaluation Owner | Production-like eval data and continuous drift monitoring. |
| **Cost vs. Loop Design** | Spend spike from retries, tool loops, long context, or agent recursion. | Terminate loop, return partial result, alert owner. | Platform / FinOps Owner | Hard budgets, retry caps, loop limits, task-level cost telemetry. |
| **Observability vs. Evidence** | Logs exist but cannot prove what happened for audit or incident review. | Preserve scoped evidence package; open evidence-quality issue. | Observability / Compliance Owner | Separate telemetry schema from audit-evidence schema. |

### **Drift Response Rule**

When drift is detected, the system should prefer:

```text
block unsafe action
downgrade authority
disclose uncertainty
reroute only to approved routes
preserve scoped evidence
open review when drift repeats
```

It should not silently “repair” contract drift in ways that hide misalignment. A patched-looking response can be worse than a clean refusal.

## **Contract Surface Inventory**

A contract surface is any seam where probabilistic behavior touches structured systems, users, tools, memory, retrieval, policy, deployment, or evidence. Every surface needs an owner, enforcer, validation method, breach behavior, and evidence boundary.

| Contract Surface | Purpose | Allowed Inputs | Expected Outputs | Owner | Enforcer | Validation | Default Breach Behavior | Evidence Boundary |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Prompt Contract** | Defines task, instruction hierarchy, allowed behavior, refusal behavior. | Typed variables, task state, approved context blocks. | Model request package and expected response mode. | Prompt / Product Owner | Prompt compiler / Gateway | Prompt unit tests, eval linkage, version checks. | Use last stable prompt or block route. | Prompt version, hash, route ID, input references. |
| **Context Contract** | Controls what state enters the model. | User input, session state, retrieved context, system variables. | Permission-filtered context bundle. | Context Owner | Gateway / Context Builder | ACL/RLS, redaction, source allowlist, token budget. | Remove unauthorized context or abort. | Context manifest, source IDs, redaction record. |
| **Schema Contract** | Makes output machine-readable. | Model output stream or structured decoding result. | Typed object or validation failure. | API / Schema Owner | Parser / Validator | JSON/schema validation, enum checks, required fields. | Retry once if safe; otherwise fail closed. | Schema version, validation result, error class. |
| **Semantic Contract** | Checks whether field values make business sense. | Parsed object. | Semantically valid object or violation. | Domain Owner | Business-rule validator | Ranges, invariants, cross-field checks. | Reject or route to human. | Rule version, failed invariant, secure payload reference. |
| **Retrieval Contract** | Defines evidence entering generation. | Query, user permissions, task scope. | Ranked, permissioned, fresh source set. | Retrieval / Corpus Owner | Retriever / Permission Filter | Source authority, freshness, dedupe, relevance, conflict checks. | Refuse grounded answer or disclose unsupported state. | Source IDs, timestamps, retrieval manifest. |
| **Grounding Contract** | Verifies claims against evidence. | Generated claims, cited sources. | Supported, unsupported, or conflicting claim labels. | Evaluation / RAG Owner | Citation / Claim Verifier | Claim-to-evidence, qualifier, time-scope, contradiction checks. | Block or mark unsupported claims. | Claim IDs, source refs, verifier result. |
| **Memory Contract** | Governs persistent writable state. | User-approved memory candidates, feedback, preferences. | Scoped memory write/read or rejection. | Memory / Privacy Owner | Memory Controller | Consent, provenance, scope, retention, conflict, delete rules. | Reject memory write or restrict read. | Memory event hash, provenance, retention class. |
| **Tool Contract** | Converts proposed actions into deterministic calls. | Typed tool arguments and actor context. | Execution result, denial, or error. | Tool Owner | Execution Harness | Preconditions, schema, idempotency, postconditions. | Deny before side effect or compensate if needed. | Action ID, idempotency key, result status. |
| **Permission Contract** | Determines whether a subject may perform an action. | Subject, tenant, resource, action, arguments, environment. | Allow, deny, require approval, or escalate. | Security / Governance Owner | Policy Engine | RBAC/ABAC, tenant, risk, approval, separation-of-duty rules. | Deny by default. | Policy version, decision, reason codes. |
| **Resource Contract** | Limits spend, latency, concurrency, retries, loops, and context. | Request, route, session, tenant, budget state. | Continue, throttle, degrade, or terminate. | Platform / FinOps Owner | Gateway / Runtime Monitor | Token, time, cost, queue, retry, loop limits. | Stop loop, return partial, or route to human. | Budget event, usage counters, route ID. |
| **Model Route Contract** | Maps task class and risk tier to model/provider/fallback. | Task class, data class, risk tier, latency/cost constraints. | Approved route or refusal. | Platform Owner | Model Gateway | Route manifest, eval pass, policy compatibility. | Use approved fallback or fail closed. | Route manifest, provider snapshot, eval status. |
| **Eval Contract** | Defines release and regression gate. | Candidate prompt/schema/route/tool package. | Pass, fail, conditional pass, or block. | Eval Owner | CI/CD / Eval Harness | Task metrics, safety tests, regression thresholds. | Block deployment or rollback. | Eval report, dataset version, manifest hash. |
| **Deployment Contract** | Packages active production configuration. | Prompts, schemas, policies, model routes, tools, eval artifacts. | Signed deployment manifest. | SRE / Release Owner | CI/CD Pipeline | Hashes, signatures, approval checks, canary health. | Halt rollout or rollback. | Manifest, approval, rollout status. |
| **Observability Contract** | Defines runtime telemetry. | Spans, metrics, errors, redacted event fields. | Structured telemetry stream. | Observability Owner | Telemetry Pipeline | Schema validation, redaction, exporter health. | Spool locally or alert. | Trace IDs, metrics, redacted spans. |
| **Audit Evidence Contract** | Preserves proof for compliance or incident review. | Scoped evidence records. | Tamper-evident evidence package. | Compliance / Security Owner | Evidence Store | Hashing, signatures, retention, access controls. | Preserve minimal required evidence; restrict access. | Evidence package ID, hashes, secure references. |
| **User Expectation Contract** | Aligns UI claims with system capability. | Rendered output, evidence, controls, disclosures. | Calibrated user action or review. | Product / UX Owner | UI Runtime | Disclosure checks, evidence display, action-state integrity. | Show uncertainty, block action, or require confirmation. | UI state, action confirmation, user decision event. |
| **Vendor / Sourcing Contract** | Governs external dependency. | Outbound request, data class, provider route. | Service result under contract terms. | Procurement / Legal Owner | Gateway / Vendor Manager | DPA, retention, SLA, no-training, subprocessor controls. | Failover, block route, or open vendor incident. | Vendor route event, contract reference, SLA record. |

The inventory should be maintained as a registry, not as prose buried in a design doc. If a surface has no owner, it has no contract.

## **Prompt Contract Specification**

A prompt is not decorative prose or a venue for model coaxing; it is production code and must be engineered as versioned, testable, and deterministic configuration. Treating the model as the reliability layer is an architectural mistake.5 Instead, prompts must be written inside structured files (such as YAML, JSON, or TOML) that clearly outline the precise runtime conditions under which the model will execute.  
The system context must also conform to a context contract. This contract specifies exactly what state enters the model, from where, and under what systemic authority.6 To prevent context bleed—which degrades model reasoning capacity by up to 90.2% when irrelevant data is carried across distinct tasks—sub-agents must be isolated by job function.5 Context isolation must be structurally enforced, ensuring that user data does not leak across session boundaries and system instructions remain shielded from malicious user manipulation.  
The following Prompt Contract Template defines the schema and execution rules for a production-grade prompt configuration:

```YAML  
meta:  
  name: "InvoiceExtractionContract"  
  owner: "FinanceEngineeringGroup"  
  version: "2.4.1"  
  last_modified: "2026-03-15T09:30:00Z"  
  linked_eval_suite: "eval_invoice_extraction_v2"  
  rollback_target: "2.4.0"  
  change_log: "Added tax identifier extraction field and updated failure escalation rules."

task_boundary:  
  description: "Extract line-item billing information from raw unstructured text files."  
  allowed_languages: ["en", "de", "fr"]  
  max_input_length_characters: 50000  
  instruction_hierarchy:  
    system_directives: 1 # Highest priority, protected from user override  
    formatting_rules: 2  
    context_data: 3  
    user_inputs: 4 # Evaluated as untrusted payload only

input_assumptions:  
  expected_variables:  
    - name: "raw_invoice_text"  
      type: "string"  
      required: true  
    - name: "client_id"  
      type: "uuid"  
      required: true  
  context_sources:  
    - name: "organization_billing_rules"  
      origin: "postgres_db"  
      freshness_tolerance_seconds: 86400

behavioral_constraints:  
  allowed_behavior:  
    - "Extract merchant name, total, subtotal, tax amounts, line-items, and invoice date."  
    - "Perform currency code standardization using ISO-4217."  
  forbidden_behavior:  
    - "Do not calculate or infer missing values; report them as null."  
    - "Do not append conversational text, explanations, or markdown commentary outside the structured schema."  
  uncertainty_behavior:  
    low_confidence_threshold: 0.85  
    action: "Set extraction_confidence metric and populate low_confidence_fields array; do not omit the field."  
  refusal_and_escalation:  
    condition: "Text does not resemble an invoice or is written in an unsupported language."  
    response_mode: "explicit_refusal"  
    refusal_payload:  
      error_code: "UNSUPPORTED_DOCUMENT_TYPE"  
      message: "The provided document could not be identified as a valid invoice."  
      escalate_to: "manual_review_queue"

output_requirements:  
  format: "json_schema"  
  schema_reference: "invoice_extraction_schema_v3.json"  
  evidence_and_citation:  
    require_grounding: true  
    citation_scope: "line_level"  
    minimum_grounding_score: 0.90
```

## **Schema Contract Model & Multi-Layer Validation**

Schemas turn model output into typed artifacts that software can inspect. A schema contract proves structure, not truth. A perfectly valid JSON object can still contain false claims, unsupported citations, unsafe recommendations, or unauthorized actions.

Structured-output mechanisms and constrained decoding can improve schema adherence, but production systems must still validate outputs at multiple layers.

```text
SCHEMA VALIDATION STACK

[ Probabilistic Core ]
        |
        v
[ Structured Output / Constrained Decoding Where Supported ]
        |
        v
[ Serialization Validation ]
  valid JSON / XML / CSV / function call envelope
        |
        v
[ Schema Validation ]
  required fields | types | enums | object shape
        |
        v
[ Semantic Validation ]
  domain rules | ranges | invariants | cross-field logic
        |
        v
[ Factual Validation ]
  evidence | math | source-of-record | citations
        |
        v
[ Policy Validation ]
  safety | privacy | tenant | compliance | risk tier
        |
        v
[ Action Validation ]
  preconditions | authorization | idempotency | postconditions
        |
        v
[ Accepted Artifact or Breach Behavior ]
```

### **Validation Layers**

| Layer | What It Proves | What It Does Not Prove | Enforcement Point | Breach Behavior |
| :---- | :---- | :---- | :---- | :---- |
| **Serialization Validation** | Output can be parsed. | Correct schema, truth, safety. | Parser. | Reject or request one safe repair. |
| **Schema Validation** | Output matches required shape and types. | Values are meaningful or factual. | JSON Schema, Pydantic, Zod, Protobuf, typed SDK. | Reject, retry once, or route to human depending on risk. |
| **Semantic Validation** | Values obey business rules and invariants. | Claims are sourced or policy-safe. | Business-rule validators. | Reject invalid fields or block artifact. |
| **Factual Validation** | Claims match evidence, math, or source-of-record data. | User should act or system has permission. | Grounding verifier, calculation engine, database check. | Mark unsupported, remove, refuse, or escalate. |
| **Policy Validation** | Output is allowed under safety, privacy, tenant, and compliance policy. | Output is useful or complete. | Policy engine / gateway. | Block or redact. |
| **Action Validation** | Proposed side effect satisfies preconditions, authorization, and postconditions. | User expectation is calibrated. | Tool execution harness. | Deny, compensate, rollback, or escalate. |

### **Schema Contract Requirements**

| Requirement | Purpose |
| :---- | :---- |
| **Schema version** | Allows output changes to be tracked and tested. |
| **Required fields** | Prevents missing data from being silently ignored. |
| **Closed object policy where appropriate** | Prevents extra unreviewed fields from entering downstream code. |
| **Explicit nullable fields** | Distinguishes missing, unknown, and not applicable. |
| **Enum constraints** | Prevents invented status labels. |
| **Numeric and string constraints** | Enforces ranges and length where supported; always revalidate client-side. |
| **Cross-field validators** | Catches contradictions such as subtotal greater than total. |
| **Evidence fields** | Links claims to source IDs or verification references. |
| **Breach behavior** | Defines reject, repair, clarify, or escalate. |

### **Important Rule**

Do not treat schema adherence as success.

```text
valid JSON ≠ correct answer
valid schema ≠ grounded answer
grounded answer ≠ authorized action
authorized action ≠ completed action
completed action ≠ user understood the result
```

Each equality must be earned by a separate contract.

## **Retrieval Contract Model & RAG Grounding Verification**

Retrieval determines what reality enters the model. The retrieval contract defines which sources may be used, who may access them, how fresh they must be, how conflicts are handled, and what level of evidence is required before the model may make a claim.

The core doctrine is:

```text
No evidence, no grounded claim.
Weak evidence, weak claim.
Conflicting evidence, disclosed conflict.
Stale evidence, time-bounded claim.
Unauthorized evidence, no context injection.
```

### **Retrieval Contract Specification**

| Contract Element | Required Definition |
| :---- | :---- |
| **Source Authority** | Which sources are authoritative for the task, and in what order. |
| **Permission Filtering** | Which user, tenant, role, or workflow may access each source. |
| **Freshness Requirement** | How current evidence must be for the task. |
| **Time Scope** | Whether the answer is valid as of a date, version, or policy period. |
| **Deduplication Rule** | How duplicate or near-duplicate sources are removed or clustered. |
| **Conflict Policy** | Whether conflicts are disclosed, escalated, or resolved by source hierarchy. |
| **Citation Granularity** | Document, page, paragraph, line, field, or record-level evidence. |
| **Null-Evidence Behavior** | Refuse, ask clarification, or answer explicitly as ungrounded. |
| **Source Exclusion Disclosure** | Whether omitted sources must be disclosed to users. |
| **Grounding Verification** | How claims are checked against cited evidence. |
| **Retrieval Telemetry** | Source IDs, rank, freshness, permission result, and verifier status. |

### **Grounding Verification Checks**

| Check | Purpose |
| :---- | :---- |
| **Claim-to-Evidence Alignment** | Each atomic claim must be supported by cited evidence. |
| **Citation Specificity** | Citation points to the exact passage, record, page, or field used. |
| **Qualifier Preservation** | “Except,” “unless,” “only,” “as of,” and thresholds are preserved. |
| **Temporal Validity** | Source date/version matches the user’s time scope. |
| **Source Authority** | Claim uses the correct source-of-record, not merely a keyword match. |
| **Conflict Disclosure** | Conflicting sources are surfaced rather than flattened. |
| **Cross-Chunk Synthesis** | Logical links between chunks are themselves supported. |
| **Null-Evidence Refusal** | Plausible but unsupported questions do not trigger hallucinated answers. |
| **Paraphrase Fidelity** | Summary preserves obligations, exclusions, numbers, and conditions. |
| **Answer-Context Sensitivity** | Removing or changing evidence changes the answer appropriately. |
| **Human Replayability** | A reviewer can verify the claim from the cited evidence without archaeology. |

### **Retrieval Breach Behaviors**

| Breach | Runtime Behavior |
| :---- | :---- |
| Source unauthorized | Remove source and rerun retrieval; log permission denial. |
| Source stale | Exclude or mark as stale; ask whether historical answer is acceptable. |
| Evidence missing | Refuse grounded answer or ask for source. |
| Evidence weak | Use uncertainty language or require human review. |
| Evidence conflicting | Disclose conflict and source hierarchy. |
| Citation unsupported | Remove claim or block response. |
| Corpus poisoning suspected | Quarantine source and open corpus review. |

A retrieval pipeline that cannot refuse unsupported claims is not grounded. It is just hallucination with footnotes.

## **Tool and Action Contract Model**

Language becomes operationally dangerous at the tool seam. A model may propose an action, but only deterministic code may authorize, execute, verify, and record that action.

A tool contract defines the complete boundary between a model proposal and a real side effect.

### **Tool Contract Structure**

| Contract Field | Meaning |
| :---- | :---- |
| **tool_id** | Stable identifier for the tool. |
| **owner** | Team responsible for behavior, safety, and maintenance. |
| **allowed_callers** | Which routes, agents, users, or services may request this tool. |
| **input_schema** | Typed arguments accepted by the tool. |
| **preconditions** | Required state before execution. |
| **authorization_policy** | Permission rules evaluated outside the model. |
| **idempotency_policy** | How retries avoid duplicate side effects. |
| **execution_timeout** | Maximum runtime. |
| **postconditions** | Required state after execution. |
| **compensation_policy** | How to reverse, repair, or reconcile failure. |
| **evidence_policy** | What evidence is retained and for how long. |
| **breach_behavior** | Deny, retry, compensate, escalate, or open incident. |

### **Idempotency Key Doctrine**

An idempotency key must identify the same logical operation across retries. It should not depend on attempt-specific IDs that change when the framework retries, unless those IDs are guaranteed stable for the logical operation.

```text
idempotency_key =
  hash(
    actor_id
  + operation_id
  + tool_id
  + target_resource_id
  + normalized_arguments
  + authorization_context_hash
  )
```

| Key Component | Purpose |
| :---- | :---- |
| **actor_id** | Binds action to user/service principal. |
| **operation_id** | Stable ID for the intended logical operation. |
| **tool_id** | Prevents collisions across tools. |
| **target_resource_id** | Binds key to affected object. |
| **normalized_arguments** | Ensures equivalent retries deduplicate. |
| **authorization_context_hash** | Prevents reuse under changed permission conditions. |

### **Action State Machine**

```text
ACTION CONTRACT STATE MACHINE

[ Proposed ]
    |
    v
[ Parsed and Schema-Valid ]
    |
    v
[ Permission Checked ]
    |
    +-- deny --> [ Denied ]
    |
    v
[ Preconditions Verified ]
    |
    +-- fail --> [ Blocked / Needs Clarification ]
    |
    v
[ Idempotency Key Reserved ]
    |
    +-- duplicate --> [ Return Prior Result ]
    |
    v
[ Submitted ]
    |
    v
[ Confirmed by Source of Record ]
    |
    +-- fail --> [ Compensate / Escalate ]
    |
    v
[ Completed ]
```

### **Postcondition Verification**

A tool call is not complete when the API returns. It is complete when the source of record confirms the expected state.

| Action Type | Required Postcondition |
| :---- | :---- |
| **Create** | New object exists with expected fields and owner. |
| **Update** | Target object version changed and fields match requested delta. |
| **Delete** | Object removed or marked deleted according to policy. |
| **Send** | Message accepted by delivery system with recipient and content hash. |
| **Payment / Transfer** | Transaction ID confirmed and amount/recipient match approved payload. |
| **Booking / Reservation** | Reservation exists with correct time, party, price, and cancellation terms. |
| **Permission Change** | Policy state reflects intended access and no unintended grants. |

### **Tool Breach Behavior**

| Breach | Behavior |
| :---- | :---- |
| Invalid arguments | Reject before execution. |
| Unauthorized actor | Deny and log policy decision. |
| Failed precondition | Ask clarification or route to human. |
| Duplicate retry | Return cached result for same logical operation. |
| Partial side effect | Compensate or reconcile. |
| Unknown final state | Query source of record; escalate if unresolved. |
| High-impact action | Require human approval before submission. |

The model proposes. The tool contract disposes.

## **Permission and Security Contract Model**

A model’s ability to generate an action is not permission to execute it. Permission must be evaluated by deterministic policy outside the model, using the authenticated subject, tenant, resource, action, arguments, environment, risk tier, and approval state.

The default decision is deny.

### **Permission Decision Inputs**

| Input | Meaning |
| :---- | :---- |
| **subject** | Human user, service account, agent identity, or delegated actor. |
| **tenant** | Organizational boundary for data and authority. |
| **resource** | Object, account, document, record, system, or environment being affected. |
| **action** | Read, write, delete, send, approve, execute, export, etc. |
| **tool** | Tool or API being invoked. |
| **arguments** | Normalized proposed payload. |
| **risk_tier** | Consequence class of the action. |
| **environment** | Production, staging, sandbox, region, network, time, device posture. |
| **approval_state** | Whether required maker-checker, human approval, or dual control exists. |
| **policy_version** | Active policy bundle used to decide. |

### **Permission Contract Outcomes**

| Outcome | Meaning |
| :---- | :---- |
| **allow** | Action may proceed to precondition and idempotency checks. |
| **deny** | Action is blocked. |
| **require_approval** | Human or multi-party approval needed. |
| **require_clarification** | Action intent or target is ambiguous. |
| **degrade_authority** | System may draft or review but not execute. |
| **escalate** | Route to security, compliance, or workflow owner. |

### **Policy Contract Pattern**

```json
{
  "decision_request": {
    "subject": {
      "id": "user_123",
      "role": "finance_reviewer",
      "tenant": "tenant_a",
      "scopes": ["invoice:read", "invoice:review"]
    },
    "resource": {
      "type": "invoice",
      "id": "inv_789",
      "tenant": "tenant_a",
      "classification": "confidential"
    },
    "tool": {
      "id": "invoice_approval_api",
      "action": "approve_payment"
    },
    "arguments": {
      "amount": "1250.00",
      "currency": "USD",
      "payee_id": "vendor_456"
    },
    "context": {
      "risk_tier": "high",
      "environment": "production",
      "approval_state": "maker_submitted_checker_pending",
      "route_id": "finance_review_governed"
    }
  }
}
```

### **Security Contract Controls**

| Control | Purpose |
| :---- | :---- |
| **External Policy Engine** | Keeps authorization outside model context. |
| **Least Privilege** | Agents inherit only the user’s approved scopes, not platform-wide authority. |
| **Tenant Isolation** | Subject, resource, memory, retrieval, and tool scopes must align. |
| **Argument-Level Policy** | Authorization checks the proposed payload, not just the tool name. |
| **Separation of Duties** | Maker and checker roles must be distinct for high-impact actions. |
| **Approval Binding** | Approval must bind to the exact payload hash, not a vague intent. |
| **Time and Environment Conditions** | Sensitive actions may depend on maintenance windows, regions, or environment. |
| **Policy Reason Codes** | Denials should be explainable to system owners and users where safe. |
| **Policy Versioning** | Every decision references the active policy bundle. |

### **Anti-Patterns**

| Anti-Pattern | Why It Fails |
| :---- | :---- |
| Prompt says “only do authorized actions.” | Prompt text is not authorization. |
| Tool name is allowed, so all payloads are allowed. | Argument-level abuse bypasses intent checks. |
| String blacklist blocks dangerous commands. | Attackers route around brittle keyword filters. |
| Agent runs under service-admin identity. | Confused-deputy failure. |
| Approval is recorded before payload finalization. | User approved an intent, not the executed action. |
| Fallback route skips policy sidecar. | Availability destroys security. |

Security contracts must be enforced before side effects, not explained after them.

## **Resource, Memory, and Model Route Contracts**

Resource, memory, and model-route contracts define the runtime envelope around model behavior. They prevent runaway loops, privacy leakage, stale personalization, unsafe fallback, and provider-specific dependency drift.

### **Resource Contract Model**

A resource contract constrains the amount of time, money, context, concurrency, and retry effort a model workflow may consume.

| Resource Limit | Purpose | Breach Behavior |
| :---- | :---- | :---- |
| **max_input_tokens** | Prevents unbounded context growth. | Compress, retrieve less, ask user, or block. |
| **max_output_tokens** | Prevents verbose or runaway generation. | Stop generation and mark truncated. |
| **max_total_tokens** | Caps full session cost. | Return partial result or escalate. |
| **max_loop_iterations** | Prevents agent recursion. | Stop loop; return current state. |
| **max_tool_calls** | Prevents tool abuse and cost bombs. | Block further tool use. |
| **max_retries** | Prevents retry storms. | Fail with structured error. |
| **latency_ceiling_ms** | Protects user experience and workflow SLO. | Timeout, fallback, or queue. |
| **cost_budget** | Controls spend by user, tenant, route, or workflow. | Throttle, degrade, or require approval. |
| **concurrency_limit** | Prevents overload and rate-limit exhaustion. | Queue or reject. |
| **queue_deadline** | Prevents stale work. | Expire or revalidate task. |

### **Resource Contract Template**

```yaml
resource_contract:
  contract_id: "<resource_contract_id>"
  owner: "<platform_or_finops_owner>"
  applies_to_routes:
    - "<route_id>"
  limits:
    max_input_tokens: 0
    max_output_tokens: 0
    max_total_tokens: 0
    max_loop_iterations: 0
    max_tool_calls: 0
    max_retries: 0
    latency_ceiling_ms: 0
    cost_budget_usd: 0
    concurrency_limit: 0
  breach_behavior:
    token_limit: "compress_or_refuse"
    loop_limit: "return_partial_and_escalate"
    cost_limit: "require_approval"
    latency_timeout: "fallback_or_queue"
```

### **Memory Contract Model**

Memory is writable state. It must be scoped, permissioned, reviewable, and deletable. Memory should not become a surveillance landfill or a prompt-injection persistence layer.

| Memory Rule | Purpose |
| :---- | :---- |
| **Write Validation** | Prevents poisoned, false, sensitive, or unauthorized memory writes. |
| **User / Principal Scope** | Prevents memory from leaking across users, tenants, roles, or workflows. |
| **Provenance Tagging** | Records where the memory came from and when. |
| **Confidence and Status** | Distinguishes user-confirmed memory from inferred memory. |
| **Retention Class** | Defines when memory expires or must be reviewed. |
| **Read Authorization** | Checks whether memory may be used in the current task. |
| **Conflict Resolution** | Handles contradictory memories. |
| **User Control** | Provides inspect, edit, disable, and delete where appropriate. |
| **Rollback / Quarantine** | Allows poisoned or harmful memory to be removed. |

### **Memory Contract Template**

```yaml
memory_contract:
  contract_id: "<memory_contract_id>"
  owner: "<memory_owner>"
  allowed_memory_types:
    - preference
    - workflow_context
    - user_confirmed_fact
  prohibited_memory_types:
    - secrets
    - unsupported_inferences
    - sensitive_data_without_policy_basis
  write_policy:
    require_user_confirmation: true
    require_provenance: true
    injection_scan_required: true
  read_policy:
    scope: "user | tenant | workflow | role"
    require_active_task_relevance: true
    require_permission_check: true
  retention:
    default_ttl_days: 0
    review_required: true
  user_controls:
    inspect: true
    edit: true
    delete: true
    disable: true
  breach_behavior:
    suspected_poisoning: "quarantine_memory_and_open_review"
    unauthorized_read: "deny_and_log"
    conflict: "ask_user_or_disclose_conflict"
```

### **Model Route Contract Model**

A model route contract maps task classes and risk tiers to approved execution routes. Routes should be expressed as capability profiles, not as brittle vendor catalogs.

| Route Field | Meaning |
| :---- | :---- |
| **route_id** | Stable internal route name. |
| **task_classes** | Workloads approved for this route. |
| **risk_tiers** | Maximum risk tier allowed. |
| **data_classes** | Data types allowed to enter this route. |
| **capability_profile** | Required reasoning, modality, context, tool, or latency capability. |
| **provider_profile** | Managed API, hosted model, self-hosted, local, deterministic fallback. |
| **eval_gate** | Eval suite and minimum pass condition. |
| **fallback_chain** | Approved fallbacks that preserve safety contract. |
| **degraded_authority** | What actions are disabled under fallback. |
| **observability** | Required telemetry emitted by route. |
| **cost_budget** | Cost limits for route use. |

### **Model Route Contract Template**

```yaml
model_route_contract:
  route_id: "<route_id>"
  owner: "<platform_owner>"
  approved_task_classes:
    - "<task_class>"
  maximum_risk_tier: "low | medium | high | regulated"
  allowed_data_classes:
    - "public"
    - "internal"
  capability_profile:
    context_window: "small | medium | large"
    modalities: ["text"]
    tool_use: "none | read_only | write_with_approval"
    latency_class: "interactive | batch | async"
  execution_profile:
    primary_route: "<provider_or_runtime_alias>"
    fallback_routes:
      - route: "<fallback_route_id>"
        authority_downgrade: "disable_tool_execution"
        reason: "primary_unavailable"
  eval_gate:
    required_suite: "<eval_suite_id>"
    required_status: "pass"
  observability:
    emit_trace: true
    emit_cost: true
    emit_quality_sample: true
  breach_behavior:
    eval_expired: "block_route"
    provider_unavailable: "use_approved_fallback"
    fallback_not_safe: "fail_closed"
```

A fallback route must preserve the safety contract. If the fallback cannot meet the same permission, privacy, grounding, or action-verification requirements, it is not a fallback. It is a contract downgrade and must reduce authority.

## **Evaluation, Deployment, and Observability Contracts**

Evaluation, deployment, and observability contracts turn AI behavior from a demo into an operated system. The eval contract decides whether a configuration may ship. The deployment contract defines exactly what shipped. The observability contract proves what happened at runtime.

### **Evaluation Contract Model**

An evaluation contract defines the test suite, dataset, metrics, thresholds, owners, and release decision for a model route, prompt, schema, retrieval pipeline, tool, or full workflow.

| Eval Contract Element | Required Definition |
| :---- | :---- |
| **eval_suite_id** | Stable identifier for the evaluation suite. |
| **scope** | Prompt, route, retrieval, schema, tool, workflow, or full system. |
| **risk_tier** | Determines required evidence and threshold strictness. |
| **dataset_version** | Golden set, adversarial set, production sample, synthetic set. |
| **metrics** | Task-specific success, failure, safety, cost, and latency metrics. |
| **thresholds** | Pass/fail or conditional release thresholds. |
| **regression_window** | Allowed delta from prior stable manifest. |
| **human_review_required** | Whether expert review is required before release. |
| **failure_behavior** | Block release, allow canary, require mitigation, or rollback. |
| **owner** | Person/team accountable for eval validity. |

### **Risk-Tiered Evaluation Gates**

| Risk Tier | Evaluation Posture |
| :---- | :---- |
| **Low** | Lightweight task tests, schema validation, basic safety and latency checks. |
| **Medium** | Golden set, regression checks, sampled human review, cost/latency gates. |
| **High** | Strong golden set, adversarial tests, grounding/policy checks, human sign-off. |
| **Regulated / Critical** | Formal evidence package, independent review, replayability, approval record. |

Avoid universal thresholds like “1,000 cases” or “100% refusal” unless the workflow justifies them. Thresholds must match task consequence, data quality, and measurement confidence.

### **Deployment Contract Model**

A deployment contract is the signed manifest of the active AI system configuration.

```yaml
deployment_manifest:
  manifest_id: "<manifest_id>"
  release_version: "<version>"
  owner: "<release_owner>"
  created_at: "<iso_datetime>"
  applies_to:
    workflow: "<workflow_id>"
    environment: "staging | production"
  components:
    prompt_contract: "<prompt_contract_id>@<version>"
    schema_contract: "<schema_contract_id>@<version>"
    retrieval_contract: "<retrieval_contract_id>@<version>"
    memory_contract: "<memory_contract_id>@<version>"
    tool_contracts:
      - "<tool_contract_id>@<version>"
    permission_policy_bundle: "<policy_bundle_hash>"
    model_route_contract: "<route_contract_id>@<version>"
    resource_contract: "<resource_contract_id>@<version>"
  eval_gate:
    eval_suite: "<eval_suite_id>"
    status: "pass | conditional | fail"
    report_hash: "<hash>"
  approvals:
    product_owner: "<approval_ref>"
    security_owner: "<approval_ref>"
    eval_owner: "<approval_ref>"
  rollback:
    previous_manifest_id: "<manifest_id>"
    rollback_conditions:
      - "schema_error_spike"
      - "policy_denial_spike"
      - "eval_regression"
```

Any change to prompt, schema, route, retrieval, memory, tool, policy, eval threshold, or resource envelope is a deployment event.

### **Observability Contract Model**

The observability contract defines runtime telemetry. It should record enough to debug, evaluate, and govern the system without overcollecting sensitive data.

| Signal | Purpose |
| :---- | :---- |
| **trace_id / workflow_id** | Correlate steps in a request or task. |
| **manifest_id** | Identify active deployment package. |
| **route_id** | Identify model/provider/runtime path. |
| **contract versions** | Link runtime behavior to exact contract stack. |
| **validation results** | Schema, semantic, factual, policy, and action checks. |
| **resource usage** | Tokens, cost, latency, retries, tool calls, queueing. |
| **breach events** | Contract violations and breach behavior. |
| **user decisions** | Accept, reject, edit, approve, override, escalate. |
| **redaction status** | Whether sensitive fields were removed or referenced securely. |

### **Telemetry vs. Audit Evidence**

| Dimension | Telemetry | Audit Evidence |
| :---- | :---- | :---- |
| **Purpose** | Debugging, monitoring, optimization, drift detection. | Proving policy, compliance, approval, or incident facts. |
| **Content** | Redacted spans, metrics, errors, counters. | Minimal structured records, hashes, approvals, policy decisions, secure refs. |
| **Retention** | Shorter, operationally scoped. | Risk/legal/compliance scoped. |
| **Mutability** | Rotated and managed as operational data. | Tamper-evident where required. |
| **Access** | Engineering and operations access by role. | Strictly controlled access by legal/security/compliance need. |

Audit evidence should not be raw logs with a fancy hat. It should be scoped, structured, minimized, and defensible.

## **User Expectation & Trust Calibration Contract**

The user interface is a contract boundary. It tells the user what the system can do, what it cannot do, what evidence it used, what was excluded, what authority it has, and when the user must verify or decide.

Trust calibration means user reliance matches system competence. Overtrust creates rubber-stamping. Undertrust creates abandonment. The contract must make the system’s role legible.

### **Expectation Contract Elements**

| Element | User-Facing Question |
| :---- | :---- |
| **System Role** | Is the AI drafting, reviewing, recommending, deciding, or acting? |
| **Authority Boundary** | Can the AI execute, or only propose? |
| **Evidence Used** | What sources support this output? |
| **Evidence Excluded** | What sources were unavailable, unauthorized, stale, or omitted? |
| **Uncertainty State** | What is known, uncertain, unsupported, or conflicting? |
| **User Responsibility** | What must the user review or approve? |
| **Action State** | Is this a draft, pending approval, submitted, confirmed, failed, or rolled back? |
| **Correction Path** | How can the user reject, edit, appeal, override, or report an issue? |
| **Fallback State** | Is the system in degraded, reduced-fidelity, or fallback mode? |

### **Trust Calibration Controls**

| Control | Purpose |
| :---- | :---- |
| **Role Label** | Shows whether AI is assistant, reviewer, router, or executor. |
| **Evidence Panel** | Makes source support visible and replayable. |
| **Unsupported Claim Marker** | Prevents unsupported text from appearing equally authoritative. |
| **Conflict Banner** | Surfaces unresolved disagreement among sources. |
| **Omitted Source Notice** | Tells users when search or retrieval was incomplete. |
| **Action Proximity Warning** | Places risk and approval requirements near execution controls. |
| **Draft / Final State Separation** | Prevents users from mistaking generated text for committed action. |
| **Human Approval Gate** | Requires explicit decision for high-impact actions. |
| **Undo / Appeal / Correction Path** | Supports trust repair and contestability. |
| **Degraded Mode Indicator** | Shows when capability or evidence has been reduced. |

### **Confidence Display Rule**

Raw model confidence can mislead users. Prefer concrete status labels tied to verification:

| Weak Display | Better Display |
| :---- | :---- |
| “95% confident” | “Cited source supports this claim.” |
| “High confidence” | “Verified against source of record.” |
| “Probably correct” | “Needs human review: conflicting evidence.” |
| “Low confidence” | “Missing required evidence.” |
| “AI completed this” | “Submitted; awaiting source-of-record confirmation.” |

### **Expectation Breach Examples**

| Breach | Example | Required Response |
| :---- | :---- | :---- |
| **Capability Overclaim** | UI promises “policy compliant” when retrieval is incomplete. | Change language, block claim, or show unsupported status. |
| **Authority Confusion** | User thinks draft was sent. | Separate draft, approval, submitted, confirmed states. |
| **Evidence Illusion** | Citation exists but does not support claim. | Remove claim or mark unsupported. |
| **Fallback Concealment** | System silently uses weaker fallback model. | Show degraded mode and reduce authority. |
| **Automation Bias** | User can one-click approve high-impact output without review. | Add evidence gate or independent judgment step. |

The UI should not ask users to trust the model. It should give users enough context to trust, verify, correct, or refuse the system appropriately.

## **Contract Breach Playbook**

A contract breach occurs when any deterministic edge rejects, cannot verify, or cannot safely process model behavior, context, retrieval, memory, tool execution, policy, route, deployment, or user expectation.

Breach handling must be deterministic. The system should not improvise safety.

```text
CONTRACT BREACH FLOW

[ Breach Detected ]
        |
        v
[ Classify Breach ]
  schema | semantic | factual | policy | permission | tool | resource
  memory | retrieval | route | deployment | observability | user expectation
        |
        v
[ Determine Severity ]
  low | medium | high | security/compliance incident
        |
        v
[ Contain ]
  block action | stop loop | freeze state | remove source | restrict memory | fail closed
        |
        v
[ Resolve or Escalate ]
  repair | retry | clarify | reroute | degrade | human review | incident
        |
        v
[ Preserve Scoped Evidence ]
        |
        v
[ Notify / Recover / Review ]
```

### **Breach Response Matrix**

| Breach Type | Default Response | Notes |
| :---- | :---- | :---- |
| **Serialization Failure** | Reject or safe single repair attempt. | Do not regex-hack high-risk outputs into existence. |
| **Schema Failure** | Retry once if low-risk; otherwise reject or escalate. | Preserve validation error class. |
| **Semantic Failure** | Reject field/object or route to human. | Business invariants beat model fluency. |
| **Factual / Grounding Failure** | Remove unsupported claim, refuse, or request evidence. | Do not hide weak grounding behind citations. |
| **Policy Failure** | Block and log policy decision. | No model retry should bypass policy. |
| **Permission Failure** | Deny before side effect. | Return reason where safe. |
| **Tool Precondition Failure** | Ask clarification, block, or route to human. | Never execute with ambiguous target. |
| **Tool Postcondition Failure** | Query source of record; compensate or escalate. | API return is not proof of completion. |
| **Resource Breach** | Stop loop, throttle, degrade, or require approval. | Cost and loop breaches can become incidents. |
| **Memory Breach** | Quarantine memory, restrict read, or delete per policy. | Preserve provenance and review path. |
| **Retrieval Breach** | Exclude source, rerun retrieval, disclose conflict, or refuse. | Poisoned or stale sources require corpus review. |
| **Route Breach** | Use approved fallback or fail closed. | Fallback must preserve safety properties. |
| **Deployment Breach** | Halt rollout or rollback manifest. | Treat prompt/policy/schema changes as deploys. |
| **Observability Breach** | Spool locally, alert owner, or block high-risk route. | Systems without evidence may be unsafe to operate. |
| **User Expectation Breach** | Update UI state, disclose limitation, require confirmation. | Trust repair is part of breach handling. |

### **Evidence Preservation Rule**

Preserve enough evidence to investigate, prove, and repair the breach without creating an uncontrolled sensitive-data archive.

| Evidence Type | Preferred Form |
| :---- | :---- |
| Prompt / schema / policy / route versions | Hashes and manifest IDs. |
| Input/output payloads | Secure references, redacted excerpts, or hashes by default. |
| Tool/action details | Action ID, idempotency key, target resource, status, payload hash. |
| Policy decision | Policy version, decision, reason code, subject/resource references. |
| Retrieval evidence | Source IDs, timestamps, rank, verifier status. |
| Memory event | Memory ID, provenance, retention class, operation type. |
| User approval | Approval ID, approver role, payload hash, timestamp. |

Raw payload capture should be reserved for incident classes and environments where policy, legal basis, access control, and retention are explicitly defined.

### **Incident Escalation Triggers**

| Trigger | Escalation |
| :---- | :---- |
| Repeated breach of same contract | Open owner review. |
| Breach affects high-risk workflow | Escalate to human/governance owner. |
| Unauthorized data exposure | Security/privacy incident. |
| Tool action executed incorrectly | Operations incident. |
| Vendor route drift causes regression | Vendor/platform incident. |
| Breach evidence cannot be preserved | Observability/compliance incident. |
| Users were misled by UI state | Product/trust incident. |

A breach is not just an error. It is a signal that a contract boundary either worked, failed, or was missing.

## **Contract Review and Lifecycle Model**

Contracts are living system artifacts. They must be created, approved, tested, released, monitored, reviewed, migrated, deprecated, and retired. A contract that exists only in prose is not a contract; it is a wish with headers.

### **Contract Lifecycle**

| Stage | Required Activity | Output |
| :---- | :---- | :---- |
| **1. Creation** | Define purpose, owner, inputs, outputs, validation, breach behavior, evidence boundary. | Draft contract artifact. |
| **2. Review** | Product, security, governance, eval, platform, and domain review as appropriate. | Approval or required changes. |
| **3. Testing** | Run unit tests, schema tests, evals, policy tests, route tests, and breach tests. | Test report. |
| **4. Release** | Bundle into deployment manifest with hashes, versions, and rollback target. | Signed manifest. |
| **5. Monitoring** | Track validation errors, breaches, cost, latency, user overrides, drift, incidents. | Runtime telemetry and evidence. |
| **6. Drift Review** | Compare production behavior to contract assumptions. | Contract update, route change, or incident. |
| **7. Migration** | Move to new model, provider, schema, policy, corpus, tool, or UI surface. | Migration plan and eval comparison. |
| **8. Deprecation** | Mark old contract as no longer preferred; route traffic away. | Deprecation notice and timeline. |
| **9. Retirement** | Disable execution and preserve required evidence. | Archived contract and retirement record. |

### **Review Triggers**

| Trigger | Contracts Affected |
| :---- | :---- |
| **Model/provider change** | Model route, eval, prompt, schema, observability. |
| **Prompt update** | Prompt, eval, schema, user expectation. |
| **Schema/API change** | Schema, tool, deployment, eval. |
| **New tool side effect** | Tool, permission, action verification, evidence. |
| **New data class** | Context, retrieval, memory, permission, privacy. |
| **New user population** | User expectation, adoption, permission, support. |
| **New jurisdiction or regulation** | Policy, memory, evidence, vendor, retention. |
| **Vendor term or subprocessor change** | Vendor, data rights, route, procurement. |
| **Retrieval corpus/index rebuild** | Retrieval, grounding, eval, observability. |
| **Embedding model change** | Retrieval, index, eval, route, exit plan. |
| **Eval regression** | Eval, deployment, route, prompt, retrieval. |
| **Cost anomaly** | Resource, route, tool, observability. |
| **Security incident** | Permission, tool, memory, evidence, deployment. |
| **User trust/adoption failure** | User expectation, workflow, prompt, evaluation. |
| **Repeated contract breach** | Affected contract and upstream/downstream layers. |

### **Contract Registry Fields**

| Field | Purpose |
| :---- | :---- |
| contract_id | Stable identifier. |
| contract_type | Prompt, schema, tool, memory, route, etc. |
| owner | Accountable team/person. |
| status | Draft, active, deprecated, retired. |
| version | Semantic or manifest version. |
| linked_contracts | Related surfaces in the stack. |
| linked_eval_suite | Required eval gate. |
| risk_tier | Determines approval and evidence requirements. |
| deployment_manifest | Active release bundle. |
| evidence_policy | Retention and evidence requirements. |
| breach_policy | Deterministic failure behavior. |
| last_reviewed | Review timestamp. |
| next_review_trigger | Time or event-based trigger. |

Contract lifecycle management is the maintenance discipline that keeps probabilistic systems from becoming folklore.

## **Cross-Canon Handoff Map**

AI-ENG-AI defines the seam discipline that unifies the AI Engineering Systems Canon. Contract thinking turns prompts, schemas, retrieval, memory, tools, permissions, resources, model routes, evaluations, deployments, observability, vendor dependencies, and user expectations into enforceable boundaries around probabilistic cores.

| Canon Report | Handoff Into AI | Contract-Thinking Integration |
| :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context windows, state, instruction hierarchy, authority. | Context Contract and Prompt Contract define what enters the model and under what priority. |
| **AI-ENG-D — Corpus Engineering** | Source ownership, lineage, metadata, lifecycle. | Retrieval Contract requires source authority, provenance, and corpus lifecycle state. |
| **AI-ENG-E — Retrieval Pipeline** | Chunking, ranking, reranking, citation, retrieval telemetry. | Retrieval and Grounding Contracts define evidence admission and claim support. |
| **AI-ENG-F — Freshness and Conflict Detection** | Source freshness, conflict packets, source-of-record logic. | Retrieval Contract requires time scope, freshness, and conflict behavior. |
| **AI-ENG-J — Throughput Mechanics** | Latency, batching, queues, prefill/decode constraints. | Resource and Model Route Contracts define runtime envelopes and latency ceilings. |
| **AI-ENG-K — Weight Dynamics** | Quantization, adapters, model behavior shifts. | Model Route and Eval Contracts bind model variants to tested task classes. |
| **AI-ENG-L — Serving Architecture** | Gateways, routing, failover, deployment patterns. | Model Route and Deployment Contracts control route manifests and fallback safety. |
| **AI-ENG-M — Agentic Orchestration** | Planner/executor loops, agent roles, multi-step tasks. | Resource, Tool, Permission, and Memory Contracts bound agent behavior. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, idempotency, execution interfaces. | AI-ENG-AI generalizes tool boundaries into full contract-stack discipline. |
| **AI-ENG-O — Action Verification** | Source-of-record checks, postconditions, false-success prevention. | Tool and Action Contracts require preconditions, postconditions, and confirmation. |
| **AI-ENG-P — Multimodal Understanding** | Image/audio/video input uncertainty and evidence. | Schema, Context, and User Expectation Contracts define modality-specific confidence and review. |
| **AI-ENG-Q — Speech, Voice, and Real-Time Systems** | Latency, interruption, streaming, voice UX. | Resource and User Expectation Contracts define realtime boundaries and confirmation gates. |
| **AI-ENG-R — UI Agents** | Browser/UI actions, screen state, user authority. | Tool, Permission, and User Expectation Contracts govern UI-side effects. |
| **AI-ENG-S — Production Pathologies** | Failure modes, brittleness, hallucination, drift. | Contract Drift Diagnostic maps pathologies to breached seams. |
| **AI-ENG-T — Boundary Defense** | Tenant isolation, prompt injection, egress, policy hierarchy. | Permission, Context, Retrieval, and Tool Contracts enforce boundaries outside the model. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, artifact trust. | Deployment and Vendor Contracts require signed artifacts and dependency evidence. |
| **AI-ENG-V — Resource Abuse** | Loop abuse, denial-of-wallet, cost bombs. | Resource Contract enforces budgets, loop caps, retry caps, and concurrency limits. |
| **AI-ENG-W — UX Resilience** | Degraded modes, fallback UX, continuity. | Breach Playbook and User Expectation Contract define safe degradation. |
| **AI-ENG-X — User Trust** | Transparency, contestability, disclosure, trust repair. | User Expectation Contract formalizes trust calibration and user authority. |
| **AI-ENG-Y — Human Review** | Maker-checker, approval queues, reviewer burden. | Permission and Tool Contracts bind approvals to payloads and action states. |
| **AI-ENG-Z — Strategic Telemetry** | Traces, metrics, behavior observability. | Observability Contract defines runtime emission and redaction. |
| **AI-ENG-AA — Evaluation Architecture** | Golden sets, rubrics, regression gates. | Eval Contract defines release gates and drift response. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit references. | Audit Evidence Contract defines what proof is retained. |
| **AI-ENG-AC — AI Operations** | Incidents, rollback, runbooks, containment. | Breach Playbook and Contract Lifecycle Model define operational response. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, compliance, accountability. | Permission, Evidence, Vendor, and Deployment Contracts make governance executable. |
| **AI-ENG-AE — Sustainable AI** | Cost, energy, routing, lifecycle efficiency. | Resource and Model Route Contracts enforce cost/resource envelopes. |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, product surface. | User Expectation and Product/Workflow Contracts bind promises to capability. |
| **AI-ENG-AG — Adoption Systems** | Training, feedback loops, incentives, change. | User Expectation and Observability Contracts feed adoption telemetry and correction loops. |
| **AI-ENG-AH — Sourcing and Vendor Strategy** | Build/buy/open/vendor decisions, exit plans. | Vendor and Model Route Contracts prevent sourcing decisions from becoming invisible lock-in. |
| **AI-ENG-AJ — Reference Architectures** | Reusable implementation patterns. | AI-ENG-AI supplies the contract surfaces each reference architecture must instantiate. |

### **Core Canon Rule**

Every probabilistic capability must cross deterministic boundaries before it can affect users, systems, memory, tools, money, permissions, evidence, or production state.


> The model may be probabilistic.
> The **system** must not be.


#### **Works cited**

1. Design by contract - Wikipedia, accessed June 15, 2026, [https://en.wikipedia.org/wiki/Design_by_contract](https://en.wikipedia.org/wiki/Design_by_contract)  
2. Applying 'design by contract' - Michigan Technological University, accessed June 15, 2026, [https://pages.mtu.edu/~aebnenas/teaching/spring2010/cs3141/readings/meyerPDF.pdf](https://pages.mtu.edu/~aebnenas/teaching/spring2010/cs3141/readings/meyerPDF.pdf)  
3. Design By Contract: A Missing Link In The Quest For Quality Software, accessed June 15, 2026, [https://wstomv.win.tue.nl/edu/2ip30/references/design-by-contract/index.html](https://wstomv.win.tue.nl/edu/2ip30/references/design-by-contract/index.html)  
4. API Security through Contract-Driven Programming | CMU Software Engineering Institute, accessed June 15, 2026, [https://www.sei.cmu.edu/blog/api-security-through-contract-driven-programming/](https://www.sei.cmu.edu/blog/api-security-through-contract-driven-programming/)  
5. AI Agent Tool Use Best Practices for Practitioners - MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-agent-tool-use-best-practices-for-practitioners/](https://mlflow.org/articles/ai-agent-tool-use-best-practices-for-practitioners/)  
6. Why Open Policy Agent is the Missing Guardrail for Your AI Agents, accessed June 15, 2026, [https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/](https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/)  
7. Groundedness detection in Azure AI Content Safety - Microsoft Learn, accessed June 15, 2026, [https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness)  
8. RAG Grounding: 11 Tests That Expose Fake Citations | by Nexumo ..., accessed June 15, 2026, [https://medium.com/@Nexumo_/rag-grounding-11-tests-that-expose-fake-citations-30d84140831a](https://medium.com/@Nexumo_/rag-grounding-11-tests-that-expose-fake-citations-30d84140831a)  
9. LLMs and Data Privacy: How to Protect Sensitive Information - Duality Technologies, accessed June 15, 2026, [https://dualitytech.com/blog/llm-data-privacy/](https://dualitytech.com/blog/llm-data-privacy/)  
10. OPA Guardrails - TrueFoundry Docs, accessed June 15, 2026, [https://www.truefoundry.com/docs/ai-gateway/opa-guardrails](https://www.truefoundry.com/docs/ai-gateway/opa-guardrails)  
11. Enforce least-privilege authorization in multi-agent AI delegation chains using Cedar on AWS - GitHub, accessed June 15, 2026, [https://github.com/aws-samples/sample-cedar-agentic-ai-authorization](https://github.com/aws-samples/sample-cedar-agentic-ai-authorization)  
12. A Survey on Long-Term Memory Security in LLM Agents: Attacks, Defenses, and Governance Across the Memory Lifecycle - arXiv, accessed June 15, 2026, [https://arxiv.org/html/2604.16548v2](https://arxiv.org/html/2604.16548v2)  
13. MemPrivacy is a privacy-preserving personalized memory management framework for edge-cloud agents. - GitHub, accessed June 15, 2026, [https://github.com/MemTensor/MemPrivacy](https://github.com/MemTensor/MemPrivacy)  
14. RAG Evals: Retrieval Relevance, Grounding, and Citation Fidelity - Vikas Goyal, accessed June 15, 2026, [https://vikasgoyal.github.io/agentic/observe/rag-evals.html](https://vikasgoyal.github.io/agentic/observe/rag-evals.html)  
15. RAG Testing — Validating Retrieval Accuracy, Grounding, and Context Leakage - Medium, accessed June 15, 2026, [https://medium.com/@gunashekarr11/rag-testing-validating-retrieval-accuracy-grounding-and-context-leakage-b3145e3a7b26](https://medium.com/@gunashekarr11/rag-testing-validating-retrieval-accuracy-grounding-and-context-leakage-b3145e3a7b26)  
16. Building Idempotent Tools for Long-Running Agents | PADISO Blog, accessed June 15, 2026, [https://www.padiso.co/blog/building-idempotent-tools-for-long-running-agents/](https://www.padiso.co/blog/building-idempotent-tools-for-long-running-agents/)  
17. Designing for Trust Calibration: Why AI Tools Need to Stop Pretending to Be Certain | by Lena C | Bootcamp | Medium, accessed June 15, 2026, [https://medium.com/design-bootcamp/designing-for-trust-calibration-why-ai-tools-need-to-stop-pretending-to-be-certain-0e7b74d285be](https://medium.com/design-bootcamp/designing-for-trust-calibration-why-ai-tools-need-to-stop-pretending-to-be-certain-0e7b74d285be)  
18. Design Patterns For Building Trust, accessed June 15, 2026, [https://smart-interface-design-patterns.com/articles/the-trust-calibration-spectrum-in-ux/](https://smart-interface-design-patterns.com/articles/the-trust-calibration-spectrum-in-ux/)  
19. From Trust in Automation to Trust in AI in Healthcare: A 30-Year Longitudinal Review and an Interdisciplinary Framework - PMC, accessed June 15, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12562135/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12562135/)  
20. Best LLM Router and AI Gateway (2026) - Inworld AI, accessed June 15, 2026, [https://inworld.ai/resources/best-llm-router-ai-gateway](https://inworld.ai/resources/best-llm-router-ai-gateway)  
21. What Is an AI Gateway? Why Your Enterprise LLM Infrastructure Needs One - LiteLLM, accessed June 15, 2026, [https://www.litellm.ai/blog/what-is-an-ai-gateway](https://www.litellm.ai/blog/what-is-an-ai-gateway)  
22. Structured outputs with OpenAI and Pydantic - dida.do, accessed June 15, 2026, [https://dida.do/blog/structured-outputs-with-openai-and-pydantic](https://dida.do/blog/structured-outputs-with-openai-and-pydantic)  
23. Agent Idempotency: Build Tool Calls That Are Safe to Retry | Chanl ..., accessed June 15, 2026, [https://www.channel.tel/blog/idempotent-tool-calls-agent-retry-safety](https://www.channel.tel/blog/idempotent-tool-calls-agent-retry-safety)  
24. Explainable recommendation: when design meets trust calibration - PMC, accessed June 15, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC8327305/](https://pmc.ncbi.nlm.nih.gov/articles/PMC8327305/)  
25. Memory scaling for AI agents | Databricks Blog, accessed June 15, 2026, [https://www.databricks.com/blog/memory-scaling-ai-agents](https://www.databricks.com/blog/memory-scaling-ai-agents)  
26. Structured model outputs | OpenAI API, accessed June 15, 2026, [https://developers.openai.com/api/docs/guides/structured-outputs](https://developers.openai.com/api/docs/guides/structured-outputs)  
27. Stop Parsing JSON by Hand: Structured LLM Outputs With Pydantic - DEV Community, accessed June 15, 2026, [https://dev.to/klement_gunndu/stop-parsing-json-by-hand-structured-llm-outputs-with-pydantic-1pg0](https://dev.to/klement_gunndu/stop-parsing-json-by-hand-structured-llm-outputs-with-pydantic-1pg0)  
28. NeurIPS Poster SeCon-RAG: A Two-Stage Semantic Filtering and Conflict-Free Framework for Trustworthy RAG, accessed June 15, 2026, [https://neurips.cc/virtual/2025/poster/115589](https://neurips.cc/virtual/2025/poster/115589)  
29. LLM Gateway Comparison 2025 - what I learned testing 5 options in production : r/AIQuality, accessed June 15, 2026, [https://www.reddit.com/r/AIQuality/comments/1q57lfc/llm_gateway_comparison_2025_what_i_learned/](https://www.reddit.com/r/AIQuality/comments/1q57lfc/llm_gateway_comparison_2025_what_i_learned/)  
30. The Complete Guide to Using Pydantic for Validating LLM Outputs, accessed June 15, 2026, [https://machinelearningmastery.com/the-complete-guide-to-using-pydantic-for-validating-llm-outputs/](https://machinelearningmastery.com/the-complete-guide-to-using-pydantic-for-validating-llm-outputs/)

---

# AI-ENG-AJ — AI System Design Patterns - Reference Architectures & Failure-Aware Blueprints

## **Conceptual Glossary**

* **AI System Design Pattern**: A formalized, reusable template addressing a recurring coordination and execution challenge in non-deterministic computing environments. Unlike traditional software patterns that assume static control flows, an AI design pattern maps state transitions while managing the variability and latent risks of foundation model completions.1  
* **Reference Architecture**: A highly structured blueprint defining the canonical components, data flows, integration surfaces, and interface boundaries required to instantiate a specific class of AI system.3 It details both functional blocks and the exact contracts that isolate failures and enforce deterministic behavior at probabilistic boundaries.4  
* **Failure-Aware Blueprint**: An engineering specification that treats operational failure as a statistical certainty rather than an exceptional anomaly. It embeds detection, mitigation, containment, and recovery mechanisms directly into the system topology to ensure graceful degradation under failure states.4  
* **Pattern Card**: A standardized, multi-dimensional specification document used to evaluate, select, implement, and audit a specific AI system design pattern. It operates as the definitive schema of record for an architecture, establishing boundary conditions, testing obligations, and operational controls.  
* **No-Use Condition**: A set of technical, operational, or business constraints under which a specific AI design pattern must not be deployed. It defines the boundaries of the pattern's viability, routing architects to deterministic or non-AI alternatives when those boundaries are breached.  
* **Anti-Pattern**: A common, superficially attractive design choice or implementation shortcut that consistently yields systemic failures, uncontrollable costs, security vulnerabilities, or operational degradation in production environments.1  
* **Degraded Mode**: A predefined, safe, lower-capability operational state that a system autonomously or semi-autonomously downshifts into when its primary probabilistic pathways fail.4 It prioritizes structural safety, data integrity, and predictability over complete functionality.  
* **Contract Surface Map**: A multi-dimensional cross-reference index defining which structural contracts (such as schema, permission, prompt, and tool contracts) are mandatory, conditional, or optional across a portfolio portfolio of reference architectures.  
* **Eval-by-Pattern**: The systematic mapping of specialized, quantitative evaluation metrics (e.g., faithfulness, context precision, and tool-call validity) to specific system design patterns to validate execution safety and performance.7  
* **Telemetry-by-Pattern**: The prescriptive specification of the exact trace elements, telemetry attributes, and state variables that must be captured and logged for a given pattern to ensure comprehensive observability, auditing, and debugging.4  
* **Human Review Map**: A governance matrix specifying the precise interfaces, roles, and levels of authority reserved for human operators within agentic and semi-autonomous systems, calibrated against operational risk tiers.1  
* **Security Boundary Map**: A structural specification of the isolation zones, data egress controls, credentials access parameters, and sandboxing requirements across different patterns to defend against prompt injection, privilege escalation, and data exfiltration.9  
* **Pattern Maturity**: A six-tier progression framework used to assess the readiness of an AI pattern implementation from initial local prototype (Level 0) to a highly platformized, automated golden path (Level 5).

## **AI Reference Architecture Doctrine**

The foundational thesis of high-dimensional AI engineering asserts that a reference architecture is not merely an illustrative collection of boxes and arrows; it is a decision artifact. A pattern is reusable only when its boundaries, assumptions, and breach behaviors are explicit. Probabilistic cores—such as large language and vision models—must be tightly surrounded by deterministic, typed, versioned, and observable edges. Consequently, an effective reference architecture must encode its contract surfaces, evaluation gates, failure modes, anti-patterns, operating controls, degraded modes, and no-use conditions directly into the structural design.  
Traditional systems engineering optimizes for deterministic reliability: inputs are transformed into outputs via known, verifiable code paths. In contrast, AI engineering systems govern probabilistic engines where identical inputs can yield divergent, semantic completions.1 This non-deterministic quality introduces systemic failure modes—including silent drift, tool-use parameter hallucination, cascade failures, and context attention dilution—which cannot be resolved by standard debugging paradigms.5  
To build production-grade systems, architects must apply the principles of contract-driven architecture. Every transition point between a deterministic service and a probabilistic model must be governed by an explicit contract stack, as established in the systemic canon. This stack spans user expectations, workflows, policies, prompts, retrievals, routes, schemas, tools, evaluations, deployments, and sourcing models. A design pattern represents the repeatable composition of these contracts to resolve a specific workload class. By structuring patterns as failure-aware blueprints, engineering teams can guarantee that when a probabilistic core fails to meet a contract, the surrounding deterministic architecture isolates the failure, records the complete trajectory, triggers appropriate recovery mechanisms, and downgrades the service controlledly.4

## **Pattern Card Template**

Every reference architecture in this doctrine is documented as a Pattern Card. A Pattern Card is not a decorative summary. It is the reusable architecture record for a workload class: when to use the pattern, when not to use it, what contracts are required, how it fails, how it degrades, how it is evaluated, and who remains accountable.

### **Canonical Pattern Card Schema**

| Field | Required Content |
| :---- | :---- |
| **Pattern Name** | Canonical architecture name. |
| **Problem Class** | The recurring system problem this pattern solves. |
| **Best-Fit Use Cases** | Workloads where the pattern is structurally appropriate. |
| **No-Use Conditions** | Conditions that route the team to deterministic software, another pattern, or no-AI. |
| **User Surface** | Chat, inline UI, queue, cockpit, API, background service, review panel, etc. |
| **Architecture Shape** | Primary components and dataflow. |
| **Required Contracts** | Mandatory AI-ENG-AI contract surfaces. |
| **Human Authority** | Human role, approval boundary, veto power, and review burden. |
| **Model Route Strategy** | Capability profile and route class, not brittle provider names. |
| **Retrieval / Context Strategy** | What context enters the system and how it is governed. |
| **Tool / Action Strategy** | Whether tools are read-only, write-capable, sandboxed, or human-approved. |
| **Core Evals** | Pattern-specific quality, safety, latency, cost, and regression checks. |
| **Telemetry** | Operational metrics needed to debug and improve the pattern. |
| **Audit / Evidence Boundary** | Minimal evidence required for compliance, incident review, or replay. |
| **Security Boundary** | Isolation, permission, data egress, credential, and sandbox requirements. |
| **Primary Cost Drivers** | Tokens, retrieval, GPU, storage, review labor, tool calls, or platform operations. |
| **Failure Modes** | Known ways the pattern fails in production. |
| **Anti-Patterns** | Common tempting but unsafe implementations. |
| **Degraded Mode** | Safe reduced-capability behavior. |
| **Adoption and Support** | Training, workflow change, support, and user enablement needs. |
| **Sourcing / Exit** | Portability, vendor dependency, and migration considerations. |
| **Maturity Target** | Expected implementation maturity level for production use. |

### **Pattern Card Markdown Form**

Each card should be written as real Markdown, not fenced pseudo-documentation. This makes the pattern searchable, linkable, diffable, and indexable.

```markdown
### **Pattern Name**

| Field | Specification |
| :---- | :---- |
| **Problem Class** |  |
| **Best-Fit Use Cases** |  |
| **No-Use Conditions** |  |
| **User Surface** |  |
| **Architecture Shape** |  |
| **Required Contracts** |  |
| **Human Authority** |  |
| **Model Route Strategy** |  |
| **Retrieval / Context Strategy** |  |
| **Tool / Action Strategy** |  |
| **Core Evals** |  |
| **Telemetry** |  |
| **Audit / Evidence Boundary** |  |
| **Security Boundary** |  |
| **Primary Cost Drivers** |  |
| **Failure Modes** |  |
| **Anti-Patterns** |  |
| **Degraded Mode** |  |
| **Adoption and Support** |  |
| **Sourcing / Exit** |  |
| **Maturity Target** |  |
```

The Pattern Card is the handoff object between product architecture, engineering implementation, governance, evaluation, operations, adoption, and sourcing.

## **Architecture Pattern Taxonomy**

AI system patterns should be organized by workflow archetype, authority level, state mutation, evidence burden, and user-review structure. A taxonomy is useful only if it helps teams route a real requirement to the correct architecture and reject bad fits early.

```text
AI SYSTEM DESIGN PATTERN TAXONOMY

1. Interactive & Embedded
   ├── Copilot / Embedded Assistant
   └── Personal or Team Productivity Assistant

2. Retrieval & Synthesis
   ├── Research Agent
   └── Enterprise Knowledge System

3. Extraction & Classification
   ├── Document Intelligence Pipeline
   ├── Multimodal Review System
   └── Background Classifier / Router

4. Analytics & Decision Intelligence
   ├── Analytics Assistant
   └── Decision-Support Cockpit

5. Support & Service Operations
   └── Support Assistant

6. Action-Oriented / Agentic
   ├── Workflow Automation Agent
   ├── Coding Agent
   └── Governed Agentic Workflow

7. Human Review & Governance
   └── Human Review and Escalation Queue

8. Platform Infrastructure
   ├── AI Gateway / Control Plane
   └── Evaluation and Shadow-Mode Pattern
```

### **Taxonomy by Control Property**

| Pattern Family | Primary User Surface | Runtime Authority | Primary Risk | Core Contract Emphasis |
| :---- | :---- | :---- | :---- | :---- |
| **Interactive & Embedded** | Inline assistant, sidebar, editor surface. | Suggests; user accepts or edits. | Overtrust, context leakage, poor fit. | Prompt, context, route, observability, user expectation. |
| **Retrieval & Synthesis** | Search portal, research console, cited answer. | Synthesizes from evidence. | Citation theater, stale/unauthorized evidence. | Retrieval, grounding, freshness, permission, eval. |
| **Extraction & Classification** | Queue, parser, background service, review panel. | Produces structured output or route label. | Silent field errors, misrouting, reviewer fatigue. | Schema, evidence, confidence, exception queue, eval. |
| **Analytics & Decision Intelligence** | BI workspace, risk cockpit, scenario panel. | Explains, computes, or recommends; human decides. | Metric hallucination, biased framing, wrong calculation. | Semantic metric, SQL/query, grounding, human review, audit. |
| **Support & Service Operations** | Customer chat, agent assist, support console. | Drafts, triages, or resolves bounded issues. | Deflection theater, policy hallucination, poor handoff. | Retrieval, escalation, user expectation, audit, telemetry. |
| **Action-Oriented / Agentic** | Task dashboard, PR interface, process portal. | Plans or executes bounded steps under policy. | Unauthorized side effects, loops, partial execution. | Tool, permission, idempotency, resource, action verification. |
| **Human Review & Governance** | Review queue, approval panel, audit cockpit. | Human validates and authorizes. | Rubber-stamping, queue overload, weak evidence. | Human review, evidence, override, audit, telemetry. |
| **Platform Infrastructure** | Internal API, gateway, release dashboard. | Controls routes, policy, eval, and observability. | Central failure, bypass, weak evidence, cost blowout. | Deployment, route, policy, resource, observability, sourcing. |

Pattern selection is not model selection. It is authority design.

## **Pattern Selection Tree**

The selection tree routes a workload to the safest viable architecture pattern. It includes deterministic and no-AI paths because not every valuable workflow deserves a model-shaped hole punched through it.

```text
PATTERN SELECTION TREE

[ Candidate Workflow ]
        |
        v
Q1. Is the task fully deterministic, exact, or better solved by rules/database/forms?
        |
        +-- yes --> [ Deterministic Software / No-AI Path ]
        |
        v
Q2. Does the system need to mutate external state or execute side effects?
        |
        +-- yes --> Q3
        |
        +-- no  --> Q7

Q3. Is the task software codebase modification, build/test, or PR generation?
        |
        +-- yes --> [ Coding Agent ]
        |
        +-- no  --> Q4

Q4. Is the action path mostly fixed, schema-bound, and workflow-driven?
        |
        +-- yes --> [ Workflow Automation Agent ]
        |
        +-- no  --> Q5

Q5. Does the task require dynamic planning, multi-step reasoning, or multi-agent checks?
        |
        +-- yes --> [ Governed Agentic Workflow ]
        |
        +-- no  --> Q6

Q6. Is the action high-risk, irreversible, regulated, or approval-sensitive?
        |
        +-- yes --> [ Human Review and Escalation Queue + Deterministic Execution ]
        |
        +-- no  --> [ Workflow Automation Agent ]

Q7. Is the primary task factual retrieval, synthesis, or evidence search?
        |
        +-- yes --> Q8
        |
        +-- no  --> Q11

Q8. Is the corpus enterprise-owned, multi-repository, permissioned, and lifecycle-managed?
        |
        +-- yes --> [ Enterprise Knowledge System ]
        |
        +-- no  --> Q9

Q9. Is the search open-ended, multi-hop, external, or research-oriented?
        |
        +-- yes --> [ Research Agent ]
        |
        +-- no  --> Q10

Q10. Is the output a high-stakes recommendation with alternatives and rationale?
        |
        +-- yes --> [ Decision-Support Cockpit ]
        |
        +-- no  --> [ Enterprise Knowledge System or Deterministic Search ]

Q11. Is the primary task structured extraction from documents or media?
        |
        +-- yes --> Q12
        |
        +-- no  --> Q14

Q12. Does the input include image, audio, video, scanned media, or spatial evidence?
        |
        +-- yes --> [ Multimodal Review System ]
        |
        +-- no  --> Q13

Q13. Is the target output typed fields from documents?
        |
        +-- yes --> [ Document Intelligence Pipeline ]
        |
        +-- no  --> [ Deterministic Parser / No-AI Path ]

Q14. Is the primary task governed analytics, SQL, metrics, or dashboard explanation?
        |
        +-- yes --> [ Analytics Assistant ]
        |
        +-- no  --> Q15

Q15. Is the primary task customer or internal support?
        |
        +-- yes --> [ Support Assistant ]
        |
        +-- no  --> Q16

Q16. Is the workload high-throughput background classification or routing?
        |
        +-- yes --> [ Background Classifier / Router ]
        |
        +-- no  --> Q17

Q17. Is the AI embedded inside an active workspace as inline assistance?
        |
        +-- yes --> [ Copilot / Embedded Assistant ]
        |
        +-- no  --> Q18

Q18. Is the system a personal/team assistant with local context and low action authority?
        |
        +-- yes --> [ Personal or Team Productivity Assistant ]
        |
        +-- no  --> Q19

Q19. Is the requirement infrastructure for model access, routing, policy, or cost control?
        |
        +-- yes --> [ AI Gateway / Control Plane ]
        |
        +-- no  --> Q20

Q20. Is the requirement validating candidate models/prompts/routes without affecting users?
        |
        +-- yes --> [ Evaluation and Shadow-Mode Pattern ]
        |
        +-- no  --> [ Return to Product Discovery / Pattern Not Selected ]
```

### **Selection Rule**

If two patterns appear plausible, choose the one with less autonomy, clearer evidence, lower integration burden, and safer degraded mode. If the task can be solved cleanly without AI, that is not a failure of imagination. It is architecture doing its job.

## **Reference Blueprint Set**

The reference blueprint set defines reusable AI system patterns. Each pattern is expressed as a real Markdown card so it can be searched, indexed, diffed, linked, and reused by architecture teams.

### **1. Copilot / Embedded Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Interactive, context-aware assistance inside an active workspace. |
| **Best-Fit Use Cases** | Inline code suggestions, prose completion, spreadsheet assistance, structured form guidance, drafting aids. |
| **No-Use Conditions** | High-liability transactions, exact calculations, background batch processing, or actions requiring autonomous write authority. |
| **User Surface** | Inline suggestions, ghost text, side panel, contextual menu. |
| **Architecture Shape** | Workspace event → context builder → policy/data filter → fast model route → suggestion renderer → user accept/edit/reject → telemetry/eval loop. |
| **Required Contracts** | Prompt, context, schema where structured, resource, model route, observability, user expectation. |
| **Human Authority** | Human remains active controller; AI suggests only. |
| **Model Route Strategy** | Low-latency route optimized for short completions; escalation only for explicitly requested deeper help. |
| **Retrieval / Context Strategy** | Local workspace state, selected document/code region, nearby context, active user intent. |
| **Tool / Action Strategy** | No external side effects; local UI modifications only. |
| **Core Evals** | Acceptance quality, edit distance, compile/test result where applicable, latency, user correction patterns. |
| **Telemetry** | Suggestion hash, route ID, prompt/schema version, accept/reject/edit event, latency, cost bucket. |
| **Audit / Evidence Boundary** | Usually operational telemetry only; sensitive content should be redacted or referenced securely. |
| **Security Boundary** | Workspace context must be scoped; cloud routes require data filtering and tenant policy. |
| **Primary Cost Drivers** | High-frequency small completions, context assembly, streaming latency. |
| **Failure Modes** | Context distraction, stale suggestions, overtrust, low-quality accepted code/text. |
| **Anti-Patterns** | Chatbox on everything; acceptance rate treated as correctness. |
| **Degraded Mode** | Static templates, local autocomplete, deterministic snippets. |
| **Adoption and Support** | Low training burden, but users need calibration on review and acceptance. |
| **Sourcing / Exit** | Keep completion interface provider-neutral. |
| **Maturity Target** | Level 3–4 for production; Level 5 when platformized across teams. |

### **2. Research Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Open-ended multi-source discovery, analysis, and factual synthesis. |
| **Best-Fit Use Cases** | Market research, literature review, policy analysis, competitor research, legal or technical source aggregation. |
| **No-Use Conditions** | Exact database lookup, simple FAQ retrieval, high-speed customer answer, or unsupported evidence domains. |
| **User Surface** | Research console, plan editor, citation panel, source browser, draft workspace. |
| **Architecture Shape** | Research question → plan/query decomposition → bounded search → source authority filter → evidence clustering → synthesis → citation verifier → human audit. |
| **Required Contracts** | Prompt, retrieval, grounding, source authority, freshness, resource, eval, observability, user expectation. |
| **Human Authority** | Human sets objective, approves plan, reviews sources, and accepts final synthesis. |
| **Model Route Strategy** | Planning/synthesis route for reasoning; cheaper extraction/summarization routes for source processing. |
| **Retrieval / Context Strategy** | Multi-hop search with source authority, freshness, dedupe, and conflict handling. |
| **Tool / Action Strategy** | Read-only search/document tools; sandboxed browsing or parsers. |
| **Core Evals** | Citation fidelity, claim support, source quality, context recall, contradiction handling, synthesis usefulness. |
| **Telemetry** | Query plan, search calls, source IDs, citation verifier status, loop count, cost, user edits. |
| **Audit / Evidence Boundary** | Source manifest, claim/evidence map, verifier result, final draft version. |
| **Security Boundary** | Prevent leakage of confidential queries to external search where prohibited. |
| **Primary Cost Drivers** | Search loops, long-context synthesis, citation verification, human review time. |
| **Failure Modes** | Citation theater, source laundering, endless search, weak source authority, stale evidence. |
| **Anti-Patterns** | Searching until confident; citing documents the system did not verify. |
| **Degraded Mode** | Present source directory and extracted notes without synthesis. |
| **Adoption and Support** | Users need training on source audit and uncertainty handling. |
| **Sourcing / Exit** | Preserve outputs and source maps in open formats. |
| **Maturity Target** | Level 3–4. |

### **3. Support Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Customer or internal support automation and agent-assist. |
| **Best-Fit Use Cases** | Routine support answers, policy explanations, ticket summarization, suggested replies, triage. |
| **No-Use Conditions** | Emergency services, high-emotion disputes without human path, legal/medical advice, unresolved policy exceptions. |
| **User Surface** | Chat, support widget, CRM agent-assist panel, ticket console. |
| **Architecture Shape** | Message/ticket → intent classifier → account/context retrieval → policy/KB retrieval → answer/draft generation → escalation gate → resolution telemetry. |
| **Required Contracts** | Intent schema, retrieval, grounding, permission, escalation, user expectation, observability, eval. |
| **Human Authority** | Human handles exceptions, low-confidence cases, sensitive accounts, and transactional approvals. |
| **Model Route Strategy** | Fast classifier plus governed generation route; fallback to human queue. |
| **Retrieval / Context Strategy** | Customer/account data through permission filters plus versioned support knowledge. |
| **Tool / Action Strategy** | Read-only by default; write actions require deterministic API and approval where material. |
| **Core Evals** | True resolution, repeat-contact rate, escalation quality, policy compliance, CSAT/sentiment, hallucination rate. |
| **Telemetry** | Intent, source IDs, route ID, escalation reason, resolution status, repeat contact, agent edits. |
| **Audit / Evidence Boundary** | Ticket ID, source policy version, final sent message, escalation packet. |
| **Security Boundary** | Tenant isolation, PII redaction, support-role access control. |
| **Primary Cost Drivers** | Multi-turn context, support volume, human escalation, KB maintenance. |
| **Failure Modes** | Deflection theater, policy hallucination, customer frustration, hidden escalation suppression. |
| **Anti-Patterns** | Trapping users in bot loops; optimizing containment over resolution. |
| **Degraded Mode** | Route to human queue with context handoff and static help links. |
| **Adoption and Support** | Requires support-team training and escalation playbooks. |
| **Sourcing / Exit** | Preserve KB, intent taxonomy, ticket metadata, and correction logs. |
| **Maturity Target** | Level 4–5. |

### **4. Document Intelligence Pipeline Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Structured extraction from unstructured or semi-structured documents. |
| **Best-Fit Use Cases** | Invoices, claims, leases, forms, contracts, financial statements, intake packets. |
| **No-Use Conditions** | Clean API/JSON input, exact deterministic parsing availability, high-speed subsecond transaction need. |
| **User Surface** | Review queue with document preview, field table, coordinates, correction UI. |
| **Architecture Shape** | Document ingest → OCR/layout parser → document classifier → extraction model → schema/semantic validator → exception queue → staging write. |
| **Required Contracts** | Input asset, OCR/layout, schema, semantic, evidence coordinate, human review, write-back, eval. |
| **Human Authority** | Auditors correct low-confidence or high-impact fields before commit. |
| **Model Route Strategy** | Extraction route matched to document class and modality; deterministic validators at edge. |
| **Retrieval / Context Strategy** | Layout, OCR, metadata, document class, field schema; no general RAG unless needed. |
| **Tool / Action Strategy** | No direct production write without validator and review/threshold gate. |
| **Core Evals** | Field-level precision/recall, table extraction, coordinate grounding, schema validity, correction rate. |
| **Telemetry** | Document hash, class, field confidence, validation errors, reviewer corrections, processing time. |
| **Audit / Evidence Boundary** | Source document reference, extracted fields, coordinate/evidence refs, reviewer decision. |
| **Security Boundary** | Ephemeral parsing, malware scanning, PII controls, storage permissions. |
| **Primary Cost Drivers** | OCR/layout, visual tokens, storage, review labor. |
| **Failure Modes** | OCR errors, table misalignment, missing pages, wrong document class, reviewer fatigue. |
| **Anti-Patterns** | Direct ERP writes from unverified extraction; ignoring visual layout. |
| **Degraded Mode** | Manual indexing/review queue. |
| **Adoption and Support** | Requires reviewer training and document-class governance. |
| **Sourcing / Exit** | Preserve schemas and extraction records in portable formats. |
| **Maturity Target** | Level 4. |

### **5. Workflow Automation Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Multi-step workflow execution across systems with bounded side effects. |
| **Best-Fit Use Cases** | Employee onboarding, ticket synchronization, procurement coordination, IT operations runbooks. |
| **No-Use Conditions** | Irreversible actions without approval, ambiguous goals, APIs without idempotency, missing source-of-record verification. |
| **User Surface** | Task dashboard, execution plan, approval checkpoints, state timeline. |
| **Architecture Shape** | Goal → plan graph → policy/permission check → tool execution → source-of-record verification → approval gate → ledger. |
| **Required Contracts** | Plan schema, tool, permission, idempotency, resource, action verification, audit, observability. |
| **Human Authority** | Human approves plan and high-impact steps; can halt, edit, or roll back. |
| **Model Route Strategy** | Reasoning route for planning; deterministic execution harness for actions. |
| **Retrieval / Context Strategy** | API specs, workflow state, policy rules, system-of-record data. |
| **Tool / Action Strategy** | Write-capable only through typed tools with idempotency and postcondition checks. |
| **Core Evals** | Task completion, tool validity, permission denial correctness, postcondition success, loop budget. |
| **Telemetry** | Plan graph, tool call IDs, idempotency keys, verification results, approvals, breaches. |
| **Audit / Evidence Boundary** | Execution ledger, payload hashes, approval records, source-of-record confirmation. |
| **Security Boundary** | Least-privilege tools, sandboxing, no broad admin agent identity. |
| **Primary Cost Drivers** | Planning loops, tool retries, approval latency, integration maintenance. |
| **Failure Modes** | Partial transaction, loop, state divergence, unauthorized action. |
| **Anti-Patterns** | Agent with admin rights; missing idempotency; no postcondition checks. |
| **Degraded Mode** | Freeze state, serialize plan, route to manual operator. |
| **Adoption and Support** | Requires operators trained on execution graphs and exception handling. |
| **Sourcing / Exit** | Keep tool schemas and workflow state portable. |
| **Maturity Target** | Level 4. |

### **6. Coding Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Codebase analysis, patch generation, testing, and pull-request preparation. |
| **Best-Fit Use Cases** | Dependency updates, scaffolding, test generation, routine bug fixes, migrations. |
| **No-Use Conditions** | Untestable systems, critical infrastructure without review, production auto-merge, missing sandbox. |
| **User Surface** | IDE, CLI, issue tracker, pull request, CI dashboard. |
| **Architecture Shape** | Issue/request → repo context selector → plan/diff generation → sandbox build/test → security scan → PR → human review. |
| **Required Contracts** | Repo access, context, patch format, sandbox, test execution, security scan, human review, deployment. |
| **Human Authority** | Developer reviews, edits, approves, and merges. |
| **Model Route Strategy** | Reasoning route for patch planning; cheaper route for log analysis. |
| **Retrieval / Context Strategy** | AST, dependency graph, related files, issue text, tests, build logs. |
| **Tool / Action Strategy** | File edits and commands only in sandbox; merge requires human/CI gate. |
| **Core Evals** | Compile success, test pass, security scan, diff minimality, reviewer acceptance, regression rate. |
| **Telemetry** | Diff hash, build logs, test results, scan findings, reviewer edits, merge status. |
| **Audit / Evidence Boundary** | PR record, commit hashes, test reports, reviewer approval. |
| **Security Boundary** | Network-restricted sandbox; secrets masked; no unreviewed production writes. |
| **Primary Cost Drivers** | Repo context, build/test loops, reasoning tokens. |
| **Failure Modes** | Plausible broken code, test overfitting, vulnerability injection, context poisoning. |
| **Anti-Patterns** | Auto-merge coding agent; writing tests to satisfy broken code. |
| **Degraded Mode** | Suggestion-only mode; disable write/PR creation. |
| **Adoption and Support** | Requires issue-writing discipline and reviewer workflow integration. |
| **Sourcing / Exit** | Store patches and scripts in standard Git workflows. |
| **Maturity Target** | Level 3–4. |

### **7. Analytics Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Governed natural-language analytics, metric explanation, SQL/query generation, and dashboard assistance. |
| **Best-Fit Use Cases** | Ad-hoc business questions, operational reporting, metric exploration, dashboard drafting. |
| **No-Use Conditions** | Regulated filings without deterministic controls, missing semantic metric layer, unrestricted database access. |
| **User Surface** | BI assistant, metric builder, SQL preview, chart explanation panel. |
| **Architecture Shape** | User question → metric/schema selector → permission check → query generator → deterministic validator → read-only execution → visualization/explanation. |
| **Required Contracts** | Semantic metric, schema, permission, SQL/query, resource, provenance, eval, observability. |
| **Human Authority** | Analyst validates metric meaning, query, and chart interpretation. |
| **Model Route Strategy** | Reasoning route for query generation; deterministic validator and metric layer are authoritative. |
| **Retrieval / Context Strategy** | Metric definitions, schema metadata, join rules, row-level policy. |
| **Tool / Action Strategy** | Read-only queries with timeouts, row limits, and query validation. |
| **Core Evals** | SQL validity, metric correctness, row-level security, chart-data consistency, explanation fidelity. |
| **Telemetry** | Question, generated query hash, metric IDs, validation result, query cost, user corrections. |
| **Audit / Evidence Boundary** | Query, metric definition, result reference, chart spec, user approval where needed. |
| **Security Boundary** | Read-only credentials, row-level security, query sandbox/resource limits. |
| **Primary Cost Drivers** | Warehouse compute, complex joins, metadata retrieval, explanation generation. |
| **Failure Modes** | Metric hallucination, wrong joins, unauthorized data exposure, misleading charts. |
| **Anti-Patterns** | Querying raw tables without metric layer; model-generated audit numbers. |
| **Degraded Mode** | Disable ad-hoc query; show verified dashboards and metric glossary. |
| **Adoption and Support** | Requires data literacy and metric-governance alignment. |
| **Sourcing / Exit** | Use portable semantic-layer definitions and SQL artifacts. |
| **Maturity Target** | Level 4. |

### **8. Multimodal Review System Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Review and audit of images, audio, video, scanned documents, or spatial/temporal evidence. |
| **Best-Fit Use Cases** | Site inspections, media policy review, brand compliance, real-estate review, document-image validation. |
| **No-Use Conditions** | Clinical/safety-critical diagnosis without regulated validation, poor media quality, no expert review path. |
| **User Surface** | Timeline, media canvas, bounding boxes, transcript, annotation review panel. |
| **Architecture Shape** | Media ingest → preprocessing/sampling → modality route → multimodal analysis → coordinate/timecode mapping → expert review → record. |
| **Required Contracts** | Asset schema, modality, sampling, coordinate/timecode, detection label, human review, audit, eval. |
| **Human Authority** | Domain expert confirms or corrects detections and final judgment. |
| **Model Route Strategy** | Multimodal route selected by asset type and evidence requirements. |
| **Retrieval / Context Strategy** | Metadata, checklists, prior reference images, transcripts where relevant. |
| **Tool / Action Strategy** | Media parsers/samplers in sandbox; no autonomous final decision in high-risk cases. |
| **Core Evals** | Detection precision/recall, coordinate grounding, transcription accuracy, reviewer agreement, false-negative rate. |
| **Telemetry** | Media hash, sampling settings, detection labels, coordinates/timecodes, reviewer adjustments. |
| **Audit / Evidence Boundary** | Media reference, annotation IDs, reviewer decision, coordinate/timecode evidence. |
| **Security Boundary** | Media sandbox, codec exploit protection, PII redaction/blurring where required. |
| **Primary Cost Drivers** | Video/audio processing, visual tokens, storage, expert review. |
| **Failure Modes** | Missed anomalies, timecode drift, bounding errors, transcript hallucination, reviewer fatigue. |
| **Anti-Patterns** | Generic visual descriptions with no coordinate evidence. |
| **Degraded Mode** | Disable overlays; present raw media and manual checklist. |
| **Adoption and Support** | Requires expert review training and annotation standards. |
| **Sourcing / Exit** | Store annotations in open schema with media references. |
| **Maturity Target** | Level 3–4. |

### **9. Enterprise Knowledge System Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Governed enterprise knowledge retrieval and synthesis across permissioned repositories. |
| **Best-Fit Use Cases** | Policy search, internal documentation, compliance research, support grounding, product knowledge. |
| **No-Use Conditions** | Exact transactional lookup, unmanaged corpora, missing ACLs, no content lifecycle. |
| **User Surface** | Search portal, chat/search UI, document preview, citation panel. |
| **Architecture Shape** | Repository sync → ACL processing → chunk/index → hybrid search/rerank → grounding verifier → answer synthesis → feedback loop. |
| **Required Contracts** | Ingestion, permission, retrieval, freshness, grounding, citation, lifecycle, eval, observability. |
| **Human Authority** | Content owners manage source quality; users verify cited answers. |
| **Model Route Strategy** | Retrieval-first; synthesis route only after evidence is permissioned and sufficient. |
| **Retrieval / Context Strategy** | Hybrid retrieval with ACL/RLS, source authority, freshness, dedupe, conflict handling. |
| **Tool / Action Strategy** | Read-only retrieval; no document mutation unless separately governed. |
| **Core Evals** | Context precision/recall, answer faithfulness, citation support, permission safety, freshness. |
| **Telemetry** | Query, user/role scope reference, source IDs, retrieval rank, citation verification, feedback. |
| **Audit / Evidence Boundary** | Source refs, answer version, citation verifier status, access-decision record. |
| **Security Boundary** | Chunk-level permissions, tenant isolation, restricted corpus ingestion. |
| **Primary Cost Drivers** | Ingestion, embeddings/indexes, reranking, storage, source lifecycle. |
| **Failure Modes** | Permission leakage, stale summaries, duplicate/conflicting docs, vector dump chaos. |
| **Anti-Patterns** | Vector Dump Knowledge System; RAG-as-database. |
| **Degraded Mode** | Keyword/file search with document links; no synthesis. |
| **Adoption and Support** | Requires knowledge management and content-owner workflows. |
| **Sourcing / Exit** | Preserve source documents, metadata, and index rebuild path. |
| **Maturity Target** | Level 4. |

### **10. Decision-Support Cockpit Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | High-stakes evidence review, scenario analysis, and human decision support. |
| **Best-Fit Use Cases** | Underwriting, legal strategy, healthcare support, risk review, supply-chain contingency planning. |
| **No-Use Conditions** | Autonomous final decision, high-volume low-review tasks, no expert validation path. |
| **User Surface** | Evidence cockpit, scenario matrix, risk panel, rationale capture. |
| **Architecture Shape** | Case file → evidence gathering → policy/guideline retrieval → scenario generation → risk/uncertainty analysis → human decision record. |
| **Required Contracts** | Case assembly, retrieval, grounding, risk, human review, rationale, audit, user expectation. |
| **Human Authority** | Human is final decision-maker; AI frames options and evidence only. |
| **Model Route Strategy** | High-reasoning route for scenario framing; deterministic calculators for numbers. |
| **Retrieval / Context Strategy** | Case record, policies, historical precedents, external evidence where approved. |
| **Tool / Action Strategy** | Simulations/read-only analysis; external writes require human approval and deterministic execution. |
| **Core Evals** | Evidence completeness, option relevance, risk coverage, calibration, rationale quality, bias review. |
| **Telemetry** | Case ID, evidence refs, scenarios generated, user choice, rationale, override/correction. |
| **Audit / Evidence Boundary** | Decision record, evidence refs, human rationale, versioned policy sources. |
| **Security Boundary** | Regulated workspace, role access, strict retention and evidence policy. |
| **Primary Cost Drivers** | Expert review, long-case context, reasoning, audit requirements. |
| **Failure Modes** | Automation bias, biased framing, missing risk, information overload. |
| **Anti-Patterns** | Human as rubber-stamp for automated decision. |
| **Degraded Mode** | Static checklist and raw evidence package. |
| **Adoption and Support** | Requires training on automation bias and evidence inspection. |
| **Sourcing / Exit** | Preserve case/rationale records and scenario schemas. |
| **Maturity Target** | Level 4. |

### **11. Background Classifier / Router Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | High-throughput classification, triage, and routing. |
| **Best-Fit Use Cases** | Ticket routing, alert triage, anomaly tagging, document category routing, moderation queues. |
| **No-Use Conditions** | High-liability decisions without review, deterministic metadata routing, low-volume tasks. |
| **User Surface** | Mostly headless; exception and audit dashboard. |
| **Architecture Shape** | Event ingest → feature extraction → classifier → confidence/threshold gate → deterministic router → exception queue. |
| **Required Contracts** | Event schema, class taxonomy, confidence threshold, exception queue, route/action schema, eval, observability. |
| **Human Authority** | Queue managers review exceptions and taxonomy drift. |
| **Model Route Strategy** | Fast classifier route; fallback to deterministic rules or manual queue. |
| **Retrieval / Context Strategy** | Taxonomy, route definitions, metadata, short event body. |
| **Tool / Action Strategy** | Queue writes only; high-impact outcomes require human review. |
| **Core Evals** | Precision/recall, confusion matrix, false-negative cost, drift, exception rate. |
| **Telemetry** | Event hash, class, confidence bucket, route target, exception reason, correction. |
| **Audit / Evidence Boundary** | Event reference, class label, route decision, taxonomy version. |
| **Security Boundary** | Input sanitization, queue permission, no arbitrary payload execution. |
| **Primary Cost Drivers** | Event volume, classification calls, exception labor. |
| **Failure Modes** | Silent misrouting, class drift, queue overload, payload manipulation. |
| **Anti-Patterns** | No exception queue; automating high-liability routing. |
| **Degraded Mode** | Route all events to manual triage or deterministic rules. |
| **Adoption and Support** | Requires taxonomy ownership and queue SLA review. |
| **Sourcing / Exit** | Preserve class taxonomy and labeled examples. |
| **Maturity Target** | Level 4–5. |

### **12. Personal or Team Productivity Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Local or team-scoped drafting, summarization, note search, and lightweight assistance. |
| **Best-Fit Use Cases** | Meeting notes, email drafts, personal knowledge search, team document drafting. |
| **No-Use Conditions** | System-of-record writes, regulated workflows, customer-facing automation, enterprise-wide authority. |
| **User Surface** | Sidebar, desktop widget, team chat assistant, document pane. |
| **Architecture Shape** | User prompt → local/team context assembly → safety/policy filter → optional memory/retrieval → model route → user review/copy. |
| **Required Contracts** | Prompt, context, memory where active, resource, user expectation, observability. |
| **Human Authority** | User owns final copy/send/paste/action. |
| **Model Route Strategy** | Low/medium capability route; local/private route for sensitive contexts where needed. |
| **Retrieval / Context Strategy** | Local notes, team docs, current workspace, permissioned memory. |
| **Tool / Action Strategy** | No direct external execution unless separately governed. |
| **Core Evals** | User utility, memory relevance, formatting, safety policy, latency. |
| **Telemetry** | Minimal usage/correction signals; avoid surveillance-style individual monitoring. |
| **Audit / Evidence Boundary** | Usually none beyond operational telemetry unless enterprise data/risk requires. |
| **Security Boundary** | Protect notes, credentials, local files, and team permissions. |
| **Primary Cost Drivers** | Frequent low-value calls, local indexes, memory storage. |
| **Failure Modes** | Memory drift, hallucinated summaries, context leakage, overcollection. |
| **Anti-Patterns** | Mixing team databases without permission boundaries. |
| **Degraded Mode** | Standard editor/search without AI. |
| **Adoption and Support** | Basic training on data boundaries and review. |
| **Sourcing / Exit** | Export notes/memory/context indexes where appropriate. |
| **Maturity Target** | Level 2–3, higher if enterprise-governed. |

### **13. Governed Agentic Workflow Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Complex, stateful, multi-step agentic workflow with governance and checkpoints. |
| **Best-Fit Use Cases** | Compliance auditing, multi-step underwriting support, controlled security review, complex operational routing. |
| **No-Use Conditions** | Linear deterministic workflow, subsecond latency, missing rollback, missing schema/state model. |
| **User Surface** | Process portal, graph visualization, checkpoint ledger, approval queue. |
| **Architecture Shape** | Goal → graph/state machine → planner/executor/auditor nodes → checkpoint ledger → deterministic gate → human escalation. |
| **Required Contracts** | Graph, prompt, context, retrieval, tool, permission, resource, memory, checkpoint, eval, observability, audit. |
| **Human Authority** | Process owner approves plan, handles escalations, can halt/rollback. |
| **Model Route Strategy** | Reasoning route for planning/auditing; deterministic state machine controls execution. |
| **Retrieval / Context Strategy** | Node-specific context, policies, APIs, state, prior checkpoints. |
| **Tool / Action Strategy** | Tool calls gated by node contract, idempotency, permission, and postcondition checks. |
| **Core Evals** | Node transition correctness, loop/budget compliance, task completion, rollback, checkpoint replay. |
| **Telemetry** | Graph path, node states, tool calls, approvals, resource use, breaches. |
| **Audit / Evidence Boundary** | Checkpoints, payload hashes, approval records, state transitions, final confirmation. |
| **Security Boundary** | Isolated node execution, scoped credentials, no cross-agent privilege leakage. |
| **Primary Cost Drivers** | Multi-agent loops, long contexts, checkpoints, human escalation. |
| **Failure Modes** | Consensus deadlock, runaway graph, state divergence, tool abuse. |
| **Anti-Patterns** | Multi-agent framework for a simple linear workflow. |
| **Degraded Mode** | Pause graph, serialize state, route to process supervisor. |
| **Adoption and Support** | Requires operator training on graph debugging and escalation. |
| **Sourcing / Exit** | Keep graph definitions, state, tools, and checkpoints portable. |
| **Maturity Target** | Level 4. |

### **14. AI Gateway / Control Plane Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Central model access, routing, policy, observability, quota, budget, and provider abstraction. |
| **Best-Fit Use Cases** | Enterprise model access, multi-provider routing, cost control, credentials management, policy enforcement. |
| **No-Use Conditions** | Local throwaway prototype with no enterprise data, no shared users, and no production route. |
| **User Surface** | Developer API, platform dashboard, admin console. |
| **Architecture Shape** | App request → identity/policy/quota → cache where safe → route selection → provider/self-hosted model → validation/telemetry. |
| **Required Contracts** | Route, permission, resource, vendor, observability, deployment, eval, sourcing. |
| **Human Authority** | Platform team controls routes, keys, budgets, policy, and emergency shutdown. |
| **Model Route Strategy** | Provider-neutral route aliases tied to task/risk/eval contracts. |
| **Retrieval / Context Strategy** | Optional cache/retrieval only with tenant, freshness, and permission scope. |
| **Tool / Action Strategy** | Usually no direct business action; may broker tool calls through separate contracts. |
| **Core Evals** | Gateway latency, route correctness, policy block correctness, cost control, provider failover. |
| **Telemetry** | Route ID, contract versions, tokens, cost, latency, policy decisions, errors, breaches. |
| **Audit / Evidence Boundary** | Route manifest, vendor contract refs, policy decisions, incident events. |
| **Security Boundary** | Key vault, egress controls, tenant isolation, DLP/policy before provider call. |
| **Primary Cost Drivers** | High-volume proxying, logs/traces, caching, provider calls. |
| **Failure Modes** | Single point of failure, gateway bypass, cache leakage, policy misroute. |
| **Anti-Patterns** | Direct provider SDK sprawl; gateway bypass. |
| **Degraded Mode** | Fail closed or use approved fallback routes preserving contracts. |
| **Adoption and Support** | Requires developer onboarding and platform support. |
| **Sourcing / Exit** | Central mechanism for provider exit and route migration. |
| **Maturity Target** | Level 5. |

### **15. Human Review and Escalation Queue Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Exception handling, human validation, approval, and correction capture. |
| **Best-Fit Use Cases** | Low-confidence extraction, high-risk actions, support escalations, moderation, regulated review. |
| **No-Use Conditions** | Very low-risk high-volume tasks with reliable deterministic validation, or no reviewer capacity. |
| **User Surface** | Review queue, evidence panel, correction editor, approval/deny controls. |
| **Architecture Shape** | AI proposal → escalation gate → priority queue → review canvas → human decision → deterministic validator → downstream commit or rejection. |
| **Required Contracts** | Escalation, human review, evidence, schema, permission, audit, observability. |
| **Human Authority** | Reviewer can approve, reject, correct, escalate, or block. |
| **Model Route Strategy** | Use AI to pre-process and prioritize; do not rely on high-cost retries to avoid review. |
| **Retrieval / Context Strategy** | Evidence packet, source refs, policy, prior corrections. |
| **Tool / Action Strategy** | Human-approved payload goes through deterministic validator and action contract. |
| **Core Evals** | Reviewer agreement, false negatives, correction categories, queue latency, fatigue indicators. |
| **Telemetry** | Queue age, reviewer action, correction delta, approval/rejection, downstream result. |
| **Audit / Evidence Boundary** | Proposal, evidence refs, reviewer ID/role, payload hash, decision reason. |
| **Security Boundary** | Reviewer RBAC, sensitive data minimization, queue access logging. |
| **Primary Cost Drivers** | Skilled review labor, queue tooling, evidence prep. |
| **Failure Modes** | Rubber-stamping, backlog, reviewer fatigue, feedback loss. |
| **Anti-Patterns** | Fake human-in-the-loop with no evidence or veto. |
| **Degraded Mode** | Pause automated writes; hold items in staging queue. |
| **Adoption and Support** | Requires reviewer training and SLA ownership. |
| **Sourcing / Exit** | Keep review records exportable. |
| **Maturity Target** | Level 4. |

### **16. Evaluation and Shadow-Mode Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Candidate model, prompt, route, retrieval, or tool validation before production exposure. |
| **Best-Fit Use Cases** | Model upgrades, prompt migrations, retrieval changes, route comparison, regression detection. |
| **No-Use Conditions** | Production data cannot be mirrored safely, no validation metrics exist, or candidate path may create side effects. |
| **User Surface** | Release dashboard, eval report, regression matrix. |
| **Architecture Shape** | Production sample or mirrored request → candidate route in isolated mode → evaluator → regression detector → release decision. |
| **Required Contracts** | Eval, route, deployment, observability, data handling, evidence, security. |
| **Human Authority** | Release owner approves rollout based on evidence. |
| **Model Route Strategy** | Candidate route isolated from production side effects. |
| **Retrieval / Context Strategy** | Mirrors retrieval where allowed; otherwise uses representative offline dataset. |
| **Tool / Action Strategy** | Read-only or mocked writes; no candidate side effects. |
| **Core Evals** | Quality delta, regression rate, safety, latency, cost, grounding, tool-call validity. |
| **Telemetry** | Candidate output, eval score, cost/latency delta, data class, route IDs. |
| **Audit / Evidence Boundary** | Eval report, dataset/sample version, manifest, approval decision. |
| **Security Boundary** | Shadow data must match production policy; candidate path isolated. |
| **Primary Cost Drivers** | Duplicate calls, evaluator cost, trace storage, review time. |
| **Failure Modes** | Evaluator bias, shadow leakage, candidate side effects, false confidence. |
| **Anti-Patterns** | Production upgrade without shadow/eval evidence. |
| **Degraded Mode** | Disable candidate path; production path unaffected. |
| **Adoption and Support** | Requires release discipline and eval ownership. |
| **Sourcing / Exit** | Enables provider/model migration decisions. |
| **Maturity Target** | Level 4–5. |

## **Systemic Integration and Control Surfaces**

Patterns become production architectures only when their controls are explicit. The matrices below define required contract packs, evaluation classes, telemetry/evidence boundaries, human authority, security boundaries, and cost drivers.

### **1. Pattern Control Pack Matrix**

| Pattern | Mandatory Control Pack |
| :---- | :---- |
| **Copilot / Embedded Assistant** | Prompt, context, route, resource, user expectation, observability. |
| **Research Agent** | Prompt, retrieval, grounding, source authority, freshness, resource, eval, observability. |
| **Support Assistant** | Intent, retrieval, grounding, escalation, permission, user expectation, support telemetry. |
| **Document Intelligence** | Asset, OCR/layout, schema, evidence coordinate, validation, review queue, audit. |
| **Workflow Automation Agent** | Plan, tool, permission, idempotency, resource, action verification, ledger. |
| **Coding Agent** | Repo access, sandbox, patch, test, security scan, human review, CI gate. |
| **Analytics Assistant** | Semantic metric, schema, permission, query validation, read-only execution, provenance. |
| **Multimodal Review** | Asset, modality, coordinate/timecode, detection label, expert review, audit. |
| **Enterprise Knowledge System** | Ingestion, ACL, retrieval, freshness, grounding, citation, lifecycle, eval. |
| **Decision-Support Cockpit** | Case assembly, evidence, scenario, risk, human decision, rationale, audit. |
| **Background Classifier / Router** | Event schema, taxonomy, confidence threshold, exception queue, route validation. |
| **Productivity Assistant** | Prompt, local/team context, memory if active, resource, user expectation. |
| **Governed Agentic Workflow** | Graph, tool, permission, memory, resource, checkpoint, action verification, audit. |
| **AI Gateway / Control Plane** | Route, vendor, permission, resource, deployment, observability, eval, policy. |
| **Human Review Queue** | Escalation, evidence, reviewer role, override, audit, downstream validation. |
| **Evaluation / Shadow Mode** | Eval, route, sample/data policy, candidate isolation, deployment, evidence. |

### **2. Eval-by-Pattern Matrix**

| Pattern | Core Evaluation Classes |
| :---- | :---- |
| **Copilot** | Suggestion usefulness, edit distance, latency, acceptance quality, downstream correctness. |
| **Research Agent** | Citation fidelity, claim support, source authority, contradiction handling, synthesis usefulness. |
| **Support Assistant** | True resolution, repeat contact, policy compliance, escalation quality, CSAT/sentiment. |
| **Document Intelligence** | Field precision/recall, schema validity, table extraction, coordinate grounding, correction rate. |
| **Workflow Automation Agent** | Plan validity, tool-call validity, postcondition success, rollback/compensation, loop budget. |
| **Coding Agent** | Compile/test pass, static scan, diff minimality, reviewer approval, regression rate. |
| **Analytics Assistant** | SQL/query validity, metric correctness, row-level security, chart consistency, explanation fidelity. |
| **Multimodal Review** | Detection precision/recall, coordinate/timecode grounding, transcript accuracy, reviewer agreement. |
| **Enterprise Knowledge System** | Context precision/recall, faithfulness, citation support, permission safety, freshness. |
| **Decision-Support Cockpit** | Evidence completeness, risk coverage, option relevance, calibration, rationale quality. |
| **Background Classifier / Router** | Precision/recall, confusion matrix, false-negative cost, drift, exception rate. |
| **Productivity Assistant** | Utility, memory relevance, safety policy, latency, user correction. |
| **Governed Agentic Workflow** | Node transition correctness, graph safety, checkpoint replay, tool verification, budget compliance. |
| **AI Gateway** | Route correctness, policy enforcement, latency overhead, cost control, failover safety. |
| **Human Review Queue** | Reviewer agreement, queue latency, fatigue, false negatives, downstream correction. |
| **Evaluation / Shadow Mode** | Candidate delta, regression, cost/latency delta, evaluator reliability, release decision quality. |

### **3. Telemetry and Evidence Boundary Matrix**

| Pattern | Operational Telemetry | Evidence / Audit Boundary |
| :---- | :---- | :---- |
| **Copilot** | Accept/reject/edit, latency, route, cost bucket. | Usually none beyond hashes/versions unless regulated. |
| **Research Agent** | Plan, source IDs, verifier status, loop count, cost. | Source manifest, claim-evidence map, final report version. |
| **Support Assistant** | Intent, source IDs, escalation reason, resolution, repeat contact. | Ticket record, final message, policy version, handoff packet. |
| **Document Intelligence** | Field confidence, validation errors, correction, runtime. | Document ref, extracted fields, coordinate refs, reviewer decision. |
| **Workflow Automation Agent** | Plan graph, tool calls, postconditions, approvals, breaches. | Execution ledger, payload hashes, approvals, state confirmation. |
| **Coding Agent** | Diff, build/test, scan, reviewer edits. | PR, commit hash, test report, reviewer approval. |
| **Analytics Assistant** | Query hash, metric IDs, validation, cost, corrections. | Query, metric definition, result ref, chart spec. |
| **Multimodal Review** | Media hash, sampling, detections, reviewer edits. | Media ref, annotation IDs, coordinates/timecodes, final decision. |
| **Enterprise Knowledge System** | Query, retrieval rank, source IDs, citation verification. | Answer version, source refs, access decision, verifier status. |
| **Decision-Support Cockpit** | Evidence refs, options, choice, rationale, overrides. | Decision record, evidence packet, rationale, policy version. |
| **Background Classifier** | Event hash, class, confidence, route, exception. | Event ref, taxonomy version, routing decision. |
| **Productivity Assistant** | Minimal utility/correction/latency signals. | Usually none unless enterprise risk requires. |
| **Governed Agentic Workflow** | Graph state, node events, tool calls, resource use, breaches. | Checkpoints, payload hashes, approvals, final confirmation. |
| **AI Gateway** | Route, tokens, cost, latency, policy decision, errors. | Route manifest, policy version, vendor/SLA event, incident record. |
| **Human Review Queue** | Queue age, reviewer action, correction, downstream result. | Proposal, evidence refs, reviewer decision, payload hash. |
| **Evaluation / Shadow Mode** | Candidate output, eval score, cost/latency delta. | Eval report, sample version, manifest, release approval. |

Telemetry supports operation. Evidence supports proof. Do not turn raw telemetry into an uncontrolled audit landfill.

### **4. Human Authority Matrix**

| Pattern | Human Authority Level |
| :---- | :---- |
| **Copilot** | Human accepts, edits, or rejects suggestions. |
| **Research Agent** | Human directs research, audits sources, approves synthesis. |
| **Support Assistant** | Human handles escalations and material account actions. |
| **Document Intelligence** | Human reviews low-confidence/high-impact fields. |
| **Workflow Automation Agent** | Human approves plans and high-impact steps. |
| **Coding Agent** | Human reviews PR and controls merge. |
| **Analytics Assistant** | Human validates metric interpretation and query output. |
| **Multimodal Review** | Expert confirms detections and final assessment. |
| **Enterprise Knowledge System** | Human verifies cited answers and content owners maintain sources. |
| **Decision-Support Cockpit** | Human is final decision-maker. |
| **Background Classifier** | Human audits exceptions and taxonomy drift. |
| **Productivity Assistant** | User owns final use of generated text. |
| **Governed Agentic Workflow** | Human can halt, approve, rollback, or escalate graph execution. |
| **AI Gateway** | Platform owner controls route, policy, budget, and shutdown. |
| **Human Review Queue** | Reviewer has veto/correction authority. |
| **Evaluation / Shadow Mode** | Release owner approves production promotion. |

### **5. Security Boundary Matrix**

| Pattern | Boundary Requirement |
| :---- | :---- |
| **Copilot** | Workspace context scoping, secret masking, local/cloud data policy. |
| **Research Agent** | Read-only tools, external-query controls, sandboxed browsing/parsing. |
| **Support Assistant** | Tenant isolation, PII redaction, CRM permission scope, escalation path. |
| **Document Intelligence** | File sandboxing, malware/PDF safety, PII controls, staging writes. |
| **Workflow Automation Agent** | Least-privilege tools, idempotency, postcondition checks, no admin identity. |
| **Coding Agent** | Network-restricted sandbox, secret masking, CI gate, human merge. |
| **Analytics Assistant** | Read-only credentials, row-level security, query limits, semantic layer. |
| **Multimodal Review** | Media sandbox, codec protection, PII blur/redaction where required. |
| **Enterprise Knowledge System** | Chunk-level ACL/RLS, tenant isolation, source lifecycle governance. |
| **Decision-Support Cockpit** | Regulated workspace, strict access, evidence retention policy. |
| **Background Classifier** | Input sanitization, queue permission, exception path. |
| **Productivity Assistant** | Local file and memory controls, no silent enterprise data upload. |
| **Governed Agentic Workflow** | Isolated node execution, scoped credentials, checkpointed state. |
| **AI Gateway** | Key vault, route policy, DLP/egress, tenant separation, gateway HA. |
| **Human Review Queue** | Reviewer RBAC, sensitive-data minimization, audit access controls. |
| **Evaluation / Shadow Mode** | Candidate isolation, no side effects, mirrored-data policy. |

### **6. Cost Driver and Mitigation Matrix**

| Pattern | Primary Cost Driver | Structural Mitigation |
| :---- | :---- | :---- |
| **Copilot** | High-frequency completions. | Debounce, local cache, short context, small route. |
| **Research Agent** | Search/synthesis loops. | Step budget, plan approval, source cache, bounded search. |
| **Support Assistant** | Long conversations and escalations. | Summarization, intent routing, KB quality, escalation thresholds. |
| **Document Intelligence** | OCR/visual processing and review labor. | Page filtering, document classification, field-confidence gates. |
| **Workflow Automation Agent** | Planning/tool loops and retries. | Static workflow templates, step caps, idempotency, approval gates. |
| **Coding Agent** | Build/test/retry loops. | Changed-file builds, sandbox reuse, issue scoping. |
| **Analytics Assistant** | Warehouse compute and bad joins. | Metric layer, query limits, dry-run cost estimates. |
| **Multimodal Review** | Media processing and expert review. | Sampling policy, preprocessing, exception thresholds. |
| **Enterprise Knowledge System** | Ingestion/index/reranking. | Deduplication, lifecycle cleanup, hybrid retrieval tuning. |
| **Decision-Support Cockpit** | Long reasoning and expert review. | Evidence templates, deterministic calculators, scenario caps. |
| **Background Classifier** | Event volume and exceptions. | Fast classifier, batching, taxonomy quality. |
| **Productivity Assistant** | Frequent low-value calls. | Local route, caching, memory limits. |
| **Governed Agentic Workflow** | Multi-agent loops and checkpoints. | Graph constraints, budget caps, early human intervention. |
| **AI Gateway** | Proxy scale, telemetry, cache, provider calls. | Sampling, budget enforcement, route optimization. |
| **Human Review Queue** | Reviewer labor. | Better thresholds, prioritization, evidence UI, workflow redesign. |
| **Evaluation / Shadow Mode** | Duplicate inference and eval cost. | Sampling, offline evals, candidate gating. |

## **Degraded Mode Pattern Library**

Degraded modes are safe lower-capability states. They must preserve the system’s safety contracts even when capability, provider availability, retrieval, tools, or latency degrade.

| Pattern | Trigger | Safe Degraded Behavior | Disabled Capability | Disclosure / Recovery |
| :---- | :---- | :---- | :---- | :---- |
| **Copilot / Embedded Assistant** | Model latency, provider outage, context unsafe. | Static snippets, deterministic autocomplete, local templates. | Generative suggestions. | Show reduced-assist status; restore after route health and policy pass. |
| **Research Agent** | Search loop budget, source verifier failure, provider outage. | Present source list, notes, and unresolved questions without synthesis. | Final synthesized answer. | Show unsupported/partial status; recover after evidence verification. |
| **Support Assistant** | Low confidence, policy retrieval failure, customer distress, outage. | Route to human queue with conversation summary and known context. | Bot resolution and transactional actions. | Tell user escalation occurred; recover after support route healthy. |
| **Document Intelligence** | OCR/parser failure, schema failure, low confidence, malware flag. | Manual document review queue. | Automated extraction/write-back. | Mark document needs review; recover after parser/eval pass. |
| **Workflow Automation Agent** | Permission failure, loop budget, unknown final state, API outage. | Freeze workflow, serialize state, notify operator. | Further side effects. | Show paused state and required human action. |
| **Coding Agent** | Sandbox failure, test failure, security scan failure. | Suggestion-only analysis with no PR/write authority. | Automated patch/PR creation. | Show failed gate; recover after tests/sandbox pass. |
| **Analytics Assistant** | Query validator failure, warehouse limit, metric ambiguity. | Static dashboards, metric glossary, or dry-run query only. | Ad-hoc execution. | Explain query blocked; recover after metric/query validation. |
| **Multimodal Review System** | Media parser failure, detection uncertainty, coordinate verifier failure. | Raw media review with manual checklist. | Automated annotations/final assessment. | Mark automation unavailable; recover after media/eval pass. |
| **Enterprise Knowledge System** | Retrieval permission uncertainty, index outage, citation verifier failure. | Keyword search/document list only. | Generated answers/summaries. | Show synthesis disabled; recover after retrieval/grounding healthy. |
| **Decision-Support Cockpit** | Evidence conflict, missing source, high uncertainty, route failure. | Static checklist and raw evidence packet. | Scenario synthesis/recommendation. | Require human decision; recover after evidence sufficiency. |
| **Background Classifier / Router** | Classifier drift, low confidence, queue route failure. | Manual triage queue or deterministic rules. | Automated routing. | Alert queue owner; recover after taxonomy/eval pass. |
| **Personal / Team Productivity Assistant** | Memory unsafe, context too sensitive, local route unavailable. | Standard editor/search without AI. | Memory use and generation. | Show AI unavailable/restricted; recover after policy/route pass. |
| **Governed Agentic Workflow** | Graph deadlock, contract breach, tool failure, resource budget. | Pause graph and checkpoint state. | Node advancement and side effects. | Notify process supervisor; recover by manual resume or rollback. |
| **AI Gateway / Control Plane** | Provider outage, policy failure, key compromise, routing anomaly. | Fail closed or use only approved fallback routes preserving contracts. | Unsafe provider routes and direct bypass. | Alert platform; recover after route/policy health check. |
| **Human Review Queue** | Reviewer overload, queue tooling failure, evidence missing. | Hold items in staging; pause downstream writes. | Approval/commit. | Notify operations; recover after queue capacity/evidence restored. |
| **Evaluation / Shadow Mode** | Shadow cost spike, candidate failure, data policy violation. | Disable candidate path; production path remains isolated. | Candidate evaluation traffic. | Alert release owner; recover after shadow policy/eval fix. |

A degraded mode is not “use a weaker model and hope.” It is a controlled reduction in capability that preserves safety, authority, and evidence boundaries.

## **AI Architecture Anti-Pattern Catalog**

### **Chatbox on Everything**

* **Symptoms**: A single, open-ended conversational chat input area is deployed as the primary user interface, replacing structured web forms, nested menus, action tables, and application buttons.  
* **Why Tempting**: Fast to build and deploy; simplifies UI design; shifts the burden of navigating process complexity directly to the user's phrasing.  
* **Why It Fails**: Increases cognitive load on users, who must guess which inputs are valid; leads to high task friction; and yields highly inconsistent completions where deterministic data writes are required.1  
* **Detection Signals**: Low user engagement metrics; high frequencies of multi-turn user explanation queries; user feedback complaining about lost dashboard features.  
* **Safer Alternatives**: Use the Copilot pattern, embedding task-specific context suggestions inline within structured user interfaces with explicit, validated operation buttons.13

### **RAG-as-Database**

* **Symptoms**: A vector database and Retrieval-Augmented Generation (RAG) pipeline are utilized to perform precise, single-value transactional lookups (e.g., account balances, order status tracking, inventory quantities).  
* **Why Tempting**: Avoids the engineering effort of building relational database indexes or structured REST APIs, allowing developers to query un-parsed files directly via semantic similarity.  
* **Why It Fails**: Vector similarity models do not guarantee exact factual retrievals; models frequently miss target numbers or synthesize adjacent data fields.7  
* **Detection Signals**: Numeric errors in customer responses; low context precision metrics; database retrieval mismatches.  
* **Safer Alternatives**: Use deterministic lookup paths (SQL indices, REST endpoints) for structured data queries, and reserve RAG patterns strictly for unstructured document search.

### **Agent as Intern with Admin Rights**

* **Symptoms**: An autonomous, model-driven agent is deployed to write directly to enterprise production systems of record (e.g., modifying ERPs, updating cloud server states) without sandbox boundaries or human approval gates.1  
* **Why Tempting**: Promises high automation speeds and creates the appearance of a fully automated operational process.  
* **Why It Fails**: Non-deterministic planner behaviors, tool call hallucinations, or prompt injections can trigger unauthorized database deletions, infinite transaction loops, and data corruption.1  
* **Detection Signals**: Corrupt production databases; billing anomalies; security alerts indicating unauthorized administrative API executions.  
* **Safer Alternatives**: Deploy the Governed Agentic Workflow pattern with sandboxed transaction runtimes and strict human verification gates.1

### **Demo Architecture**

* **Symptoms**: A prototype pipeline using complex multi-agent frameworks is deployed to production with the same configurations used during local development, lacking telemetry logging, error routing, and tracing configurations.2  
* **Why Tempting**: Accelerates initial deployment schedules; minimizes system setup and platform management workloads.  
* **Why It Fails**: Production data exposes systemic anomalies, rate-limiting failures, token budget overruns, and security exploits that local testing environments hide.2  
* **Detection Signals**: Uncontrollable API spend; system timeout errors; untraceable transactional failures; lack of structured logging.  
* **Safer Alternatives**: Implement the AI Gateway pattern to manage credentials, rate-limiting parameters, and open telemetry collections across providers.

### **Citation Theater**

* **Symptoms**: Generated documents feature academic-style footnotes and hyperlink annotations that point to irrelevant sources, hallucinated files, or non-existent document subsections.1  
* **Why Tempting**: Footnotes look professional and create the appearance of factual grounding and research reliability.  
* **Why It Fails**: Undermines system trust; users can be misled by plausible-sounding synthetic text backed by hallucinated references.1  
* **Detection Signals**: User feedback flagging broken links; low citation fidelity evaluation scores; automated grounding mismatches.  
* **Safer Alternatives**: Deploy a dedicated Citation Verifier pipeline to validate visual coordinate maps and link offsets before rendering documents.

### **Workflow Double-Work**

* **Symptoms**: Users spend more time reviewing, correcting, and re-verifying model-generated drafts than they would have spent writing the document manually.  
* **Why Tempting**: Promises high-volume automated document drafts at low upfront cost.  
* **Why It Fails**: High-volume, low-quality generations introduce long correction workloads, causing operator fatigue and reducing productivity.  
* **Detection Signals**: High user reject rates; low adoption metrics; longer document processing timelines.  
* **Safer Alternatives**: Deploy bounded Copilot models designed to suggest local completions inline, rather than generating entire complex reports at once.13

### **Model Leaderboard Architecture**

* **Symptoms**: Selecting a core model backend based purely on generic public benchmarks (e.g., MMLU scores) rather than evaluating performance against custom task data.  
* **Why Tempting**: Avoids the operational cost of building local evaluation test suites and benchmark datasets.  
* **Why It Fails**: Public benchmarks rarely reflect specific corporate task parameters, schema requirements, or document formatting realities.  
* **Detection Signals**: Upgraded models underperforming on custom corporate tasks despite higher public benchmark scores.  
* **Safer Alternatives**: Build localized evaluation datasets and regression gates based on production telemetry to validate model upgrades.

### **Single-Provider Hardwire**

* **Symptoms**: Directly referencing vendor-specific SDK libraries and proprietary API payload formats throughout system controller files, without decoupling layers.  
* **Why Tempting**: Simplifies initial integration; leverages vendor-specific helper utilities.  
* **Why It Fails**: Locks the enterprise into a single model provider; introduces operational risk during outages or API deprecations.6  
* **Detection Signals**: Long refactoring timelines during model migration projects; systemic outages when a primary model provider experiences downtime.  
* **Safer Alternatives**: Implement the AI Gateway pattern using standard API specifications to decouple application controllers from provider endpoints.9

### **Unbounded Research Goblin**

* **Symptoms**: Deploying research agents without step execution boundaries, model timeout constraints, or spending ceilings, allowing models to query APIs indefinitely in search of answers.1  
* **Why Tempting**: Promises deep, thorough research by allowing agents to search iteratively.1  
* **Why It Fails**: Stochastic loops can trigger hundreds of search queries and model calls, causing token costs to spike rapidly.1  
* **Detection Signals**: Cost alerts; model requests timing out; execution trace logs showing deep, circular reasoning trees.  
* **Safer Alternatives**: Implement strict execution budgets, step limits, and timeout boundaries in agent planner configurations.1

### **Magic Document Reader**

* **Symptoms**: Multi-page, visually complex document folders (e.g., scanned PDFs with tables) are sent directly to models as raw file streams, relying on model context to interpret visual and spatial data.  
* **Why Tempting**: Avoids configuring specialized layout extraction tools and OCR parsers.  
* **Why It Fails**: Standard models frequently misinterpret multi-column tables, omit visual data, or confuse page-level spatial hierarchies.2  
* **Detection Signals**: Low extraction precision scores; misread data fields; hallucinated table summaries.  
* **Safer Alternatives**: Implement a structured Document Intelligence Pipeline with layout parsing and spatial coordinate validation.2

### **One Pattern to Rule Them All**

* **Symptoms**: Attempting to use a single architectural pattern (e.g., an open-ended conversational agent) to handle all enterprise AI workloads.  
* **Why Tempting**: Standardizes development on a single framework, simplifying platform team operations.  
* **Why It Fails**: Different tasks have divergent constraints; forcing low-latency tasks through agent loops or highly structured extractions through chat interfaces degrades quality.  
* **Detection Signals**: Rising execution latency; user friction; high development costs as teams patch a monolithic system.  
* **Safer Alternatives**: Select specialized reference patterns matched to the constraints of each task class.14

### **Gateway Bypass**

* **Symptoms**: Product development teams bypass central AI Gateways and deploy custom model endpoints directly using local cloud credentials.  
* **Why Tempting**: Allows developers to prototype and deploy changes without coordination bottlenecks with platform engineering teams.  
* **Why It Fails**: Bypasses corporate security, access controls, token spending audits, and threat detection systems, introducing compliance risks.  
* **Detection Signals**: Hidden API cost increases on cloud billing; security audits finding unmonitored external model traffic.  
* **Safer Alternatives**: Implement the AI Gateway pattern as a mandatory routing policy at the network boundary.

### **Fake Human-in-the-Loop**

* **Symptoms**: Human review interfaces are designed as simple confirmation panels with no access to grounding data or verification tools, encouraging rapid clicking.  
* **Why Tempting**: Minimizes workflow delay and reduces human labor costs while claiming safety compliance.  
* **Why It Fails**: Humans cannot verify claims without grounding data, leading to automated rubber-stamping and un-vetted errors reaching production.13  
* **Detection Signals**: 100% manual review approval rates; downstream error occurrences matching automated prototype testing rates.  
* **Safer Alternatives**: Implement a dedicated Human Review and Escalation Queue with side-by-side evidence, coordinate highlighting, and rejection tools.

### **Deflection Theater**

* **Symptoms**: Customer support systems are configured to deflect user requests at all costs, blocking access to human agents or escalation queues.  
* **Why Tempting**: Minimizes customer support center staffing requirements and operational overhead.  
* **Why It Fails**: Traps users in unresolved loops with incorrect answers, leading to customer churn and brand damage.  
* **Detection Signals**: High customer exit rates; negative post-chat feedback ratings; rising repeat-contact frequencies.  
* **Safer Alternatives**: Support Assistant pattern with structured intent classification and low-friction human escalation paths.

### **Metric Hallucination**

* **Symptoms**: Models generate business intelligence analytics directly against database tables without using standard, governed semantic metric layer definitions.  
* **Why Tempting**: Fast to deploy; avoids the engineering effort of building metadata frameworks or semantic layers.  
* **Why It Fails**: Models hallucinate database join paths, use non-standard calculation logic, or expose restricted records to unauthorized users.  
* **Detection Signals**: Inconsistent business metrics across reports; SQL compilation failures; security policy alerts.  
* **Safer Alternatives**: Implement the Analytics Assistant pattern with strict SQL validators and a governed semantic metric layer.

### **Vector Dump Knowledge System**

* **Symptoms**: Thousands of un-curated, multi-format documents are uploaded into a single vector index without metadata, access rules, or lifecycle curation.8  
* **Why Tempting**: Extremely low configuration cost; provides generic conversational answers across the corpus.  
* **Why It Fails**: Returns outdated, duplicate, or conflicting context chunks, causing model synthesis confusion and access control leaks.  
* **Detection Signals**: Stale information in answers; high rate of irrelevant citations; unauthorized access to sensitive files.  
* **Safer Alternatives**: Enterprise Knowledge System pattern with ingestion filters, dynamic chunking, and IAM integration.8

### **Auto-Merge Coding Agent**

* **Symptoms**: Automated coding tools are configured to commit code patches directly to production branches without developer review or verification testing.1  
* **Why Tempting**: Accelerates feature delivery; minimizes developer review overhead.  
* **Why It Fails**: Plausible but broken patches, logic errors, or security vulnerabilities are deployed directly to production systems.1  
* **Detection Signals**: Rising production regressions; build breakages; automated security alert spikes.  
* **Safer Alternatives**: Enforce sandbox testing and manual peer-developer code reviews for all model-generated patches.

### **Fallback Contract Downgrade**

* **Symptoms**: When a primary, secure model provider fails, the system automatically redirects traffic to a simpler backup model while bypassing security, output validation, or policy contracts.  
* **Why Tempting**: Prioritizes system availability during primary provider outages.  
* **Why It Fails**: Lowers system security, allows unsafe outputs to bypass filters, and risks deploying unvalidated data.  
* **Detection Signals**: Security anomalies during primary provider outages; format validation failures on backup routes.  
* **Safer Alternatives**: Fallback configurations must enforce identical security, validation, and policy contracts across all model routes.

## **No-Use Condition Matrix**

No-use conditions prevent pattern overreach. They do not always mean “never use AI”; they often mean “use another pattern,” “add a human gate,” “use deterministic software,” or “complete readiness work first.”

| Pattern | No-Use / Redesign Condition | Why This Pattern Fails | Safer Architecture |
| :---- | :---- | :---- | :---- |
| **Copilot / Embedded Assistant** | Task requires autonomous execution, exact calculation, or unattended batch processing. | Inline suggestion surface does not provide execution assurance. | Deterministic workflow, analytics assistant, or workflow automation with gates. |
| **Research Agent** | User needs exact source-of-record lookup or answer must be instant and deterministic. | Multi-hop synthesis adds latency and hallucination risk. | SQL/API lookup, enterprise search, or deterministic report. |
| **Support Assistant** | Emergency, legal, medical, abuse, or high-emotion dispute without human escalation. | AI may delay proper human intervention. | Human-first support queue with AI evidence assist only. |
| **Document Intelligence Pipeline** | Source system already provides clean structured data. | OCR/extraction adds unnecessary error and cost. | API ingestion or deterministic parser. |
| **Workflow Automation Agent** | Irreversible or high-value mutation lacks approval, idempotency, or verification. | Side effects can be wrong and unrecoverable. | Human approval queue plus deterministic execution. |
| **Coding Agent** | Codebase cannot be built, tested, sandboxed, or reviewed. | No reliable validation path. | Human developer workflow; build test infrastructure first. |
| **Analytics Assistant** | Metrics are undefined, database access is broad, or output is regulated filing. | Model may invent joins/calculations. | Semantic metric layer, governed BI, deterministic reporting. |
| **Multimodal Review System** | Safety-critical diagnosis or measurement lacks regulated validation and expert authority. | False negatives/positives can cause harm. | Expert-led review with AI as evidence-prep only. |
| **Enterprise Knowledge System** | Corpus lacks ACLs, freshness, ownership, or lifecycle governance. | Retrieval can leak data or surface stale/conflicting answers. | Corpus engineering and permission indexing first. |
| **Decision-Support Cockpit** | Organization wants AI to make final high-impact decisions. | Cockpit pattern supports humans; it does not replace authority. | Manual decision workflow with evidence support. |
| **Background Classifier / Router** | False negatives have high consequence and no exception review exists. | Silent misrouting becomes systemic risk. | Manual triage or deterministic rules until eval/review exists. |
| **Productivity Assistant** | System manipulates records, sends customer messages, or processes regulated data. | Local assistant lacks governance and action controls. | Support assistant, workflow automation, or governed enterprise system. |
| **Governed Agentic Workflow** | Workflow is simple, linear, deterministic, or latency-critical. | Agent graph adds unnecessary complexity and cost. | Static workflow engine or deterministic microservice. |
| **AI Gateway / Control Plane** | One-off local toy prototype with no enterprise data or shared use. | Gateway overhead may exceed value. | Direct sandbox SDK with test credentials only. |
| **Human Review Queue** | Task is low-risk, high-volume, and deterministically validated. | Review creates bottleneck and fatigue. | Automated validation with sampled audit. |
| **Evaluation / Shadow Mode** | Production data cannot be mirrored safely or no evaluation metric exists. | Shadow path creates privacy risk or meaningless scores. | Offline eval with sanitized/representative data. |

If the no-use condition is true, do not “prompt harder.” Change the architecture.

## **Implementation Maturity Levels**

AI architecture maturity describes how safely and repeatably a pattern can be operated. Maturity is not model quality. It is the presence of contracts, evals, telemetry, security, human authority, operations, support, and lifecycle controls.

| Level | Name | Allowed Use | Forbidden Use | Required Controls | Exit Gate to Next Level |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **0** | **Demo** | Local exploration with synthetic or non-sensitive data. | Production data, customer exposure, system-of-record access, unmanaged secrets. | Sandbox only, test credentials, no live integrations, no claims of reliability. | Define use case, data class, owner, and prototype boundary. |
| **1** | **Controlled Prototype** | Internal prototype with curated data and limited users. | Production actions, broad rollout, sensitive data without approval. | Basic prompt/schema, sandbox, initial evals, test keys/vaulted secrets, trace capture. | Pass seed eval, security/data review, and product-fit review. |
| **2** | **Pilot** | Bounded real workflow with limited users and explicit monitoring. | Autonomous high-impact actions, unreviewed external outputs. | Named owner, telemetry, human review path, fallback, incident contact, pilot success criteria. | Demonstrate quality, adoption, cost, safety, and operational readiness. |
| **3** | **Production** | Approved production workflow within defined risk tier. | Unbounded route changes, silent model swaps, missing rollback. | Contract stack, eval gate, deployment manifest, observability, runbook, rollback, support. | Prove repeatability across teams/routes and governance review. |
| **4** | **Governed Scale** | Multi-team or high-volume production under platform controls. | Pattern-specific one-off exceptions without review. | Gateway/control plane, standardized evals, policy automation, cost controls, audit evidence, adoption support. | Package as reusable golden path. |
| **5** | **Platform Golden Path** | Reusable pattern available through developer platform templates. | Bypassing mandatory controls. | Automated scaffolding, contract templates, eval harness, observability, security defaults, documentation, support model. | Continuous lifecycle; no higher maturity required. |

### **Maturity Rules**

| Rule | Meaning |
| :---- | :---- |
| **No production without owner.** | Every deployed pattern needs accountable product, technical, and operational ownership. |
| **No write authority before action verification.** | Tools that mutate state require permission, idempotency, and postcondition checks. |
| **No broad rollout without telemetry.** | Adoption, quality, cost, latency, and breach events must be visible. |
| **No model migration without eval evidence.** | Provider/model/prompt/schema changes are deployment events. |
| **No human review theater.** | Reviewers need evidence, veto power, and time. |
| **No prototype secrets sprawl.** | Even demos use test credentials and safe environments. |
| **No golden path without exit path.** | Reusable patterns must include sourcing and migration assumptions. |

Maturity is not achieved when the demo works. Maturity begins when failure is boring, bounded, and recoverable.

## **Cross-Canon Handoff Map**

AI-ENG-AJ converts the AI Engineering Systems Canon into reusable reference architectures. Earlier reports define the doctrine, constraints, controls, and failure modes. AJ packages them into pattern cards that teams can select, instantiate, evaluate, operate, and govern.

| Canon Report | Input to AJ | How AJ Uses It |
| :---- | :---- | :---- |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, no-AI decisions, product surface. | Determines whether a pattern should be selected at all. |
| **AI-ENG-AG — Adoption Systems** | Training, support, feedback loops, user resistance, incentives. | Adds adoption and support requirements to pattern cards. |
| **AI-ENG-AH — Sourcing and Vendor Strategy** | Build/buy/open/hybrid, vendor exit, control plane. | Adds sourcing and exit fields to every pattern. |
| **AI-ENG-AI — Contract Thinking** | Prompt, schema, retrieval, tool, permission, route, eval, observability contracts. | Defines the contract surfaces required by each pattern. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, accountability, procurement, compliance. | Defines governance and evidence requirements. |
| **AI-ENG-AE — Sustainable AI** | Cost, energy, routing, lifecycle impact. | Adds cost drivers and degraded/resource-aware modes. |
| **AI-ENG-J — Throughput Mechanics** | Latency, batching, streaming, queueing. | Shapes route strategy and user-surface expectations. |
| **AI-ENG-K — Weight Dynamics** | Model size, quantization, adaptation. | Informs route class and self-host/open-weight viability. |
| **AI-ENG-L — Serving Architecture** | Gateways, serving topologies, fallback. | Implements patterns through route/control-plane designs. |
| **AI-ENG-M — Agentic Orchestration** | Planners, executors, graph workflows, multi-agent systems. | Grounds agentic and workflow automation patterns. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, idempotency, side effects. | Defines tool/action requirements for action-oriented patterns. |
| **AI-ENG-O — Action Verification** | Source-of-record confirmation and false-success prevention. | Defines postcondition verification for tools and workflows. |
| **AI-ENG-P — Multimodal Understanding** | Vision/audio/video evidence and uncertainty. | Grounds document and multimodal review patterns. |
| **AI-ENG-Q — Speech and Realtime Systems** | Streaming, voice latency, turn-taking. | Informs interactive/embedded and support surfaces. |
| **AI-ENG-R — UI Agents** | Browser/UI control and interface state. | Informs UI-agent authority and action boundaries. |
| **AI-ENG-S — Production Pathologies** | Common production failures. | Feeds anti-patterns, failure modes, and degraded modes. |
| **AI-ENG-T — Boundary Defense** | Prompt injection, tenant isolation, egress policy. | Defines security boundaries for every pattern. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, dependency risk. | Informs sourcing, deployment, and platform patterns. |
| **AI-ENG-V — Resource Abuse** | Loop abuse, denial-of-wallet, budget failures. | Defines resource controls and cost drivers. |
| **AI-ENG-W — UX Resilience** | Fallback, degraded UX, continuity. | Defines degraded-mode library. |
| **AI-ENG-X — User Trust** | Trust calibration, contestability, transparency. | Defines user expectation and human-review surfaces. |
| **AI-ENG-Y — Human Review** | Reviewer authority, queues, maker-checker, fatigue. | Grounds human review and escalation patterns. |
| **AI-ENG-Z — Telemetry** | Runtime traces, metrics, correction signals. | Defines telemetry-by-pattern. |
| **AI-ENG-AA — Evaluation** | Golden sets, eval gates, regression. | Defines eval-by-pattern. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit proof. | Defines evidence boundaries. |
| **AI-ENG-AC — AI Operations** | Incidents, runbooks, rollback, containment. | Defines operational maturity and breach response. |

### **MCP and Tooling Note**

Tooling standards should be referenced carefully. MCP uses JSON-RPC messages over stdio and Streamable HTTP; SSE may be used within Streamable HTTP where supported. Architecture cards should describe the tool contract and transport requirements generically, then name MCP or other protocols as implementation options rather than hard dependencies.

### **Core Handoff Rule**

AJ is where doctrine becomes reusable architecture.

```text
A reference architecture is a reusable failure-aware contract bundle.

It tells engineers:
  when to use the pattern,
  when not to use it,
  what contracts are required,
  how it is evaluated,
  what telemetry proves,
  how humans retain authority,
  how it degrades,
  how it exits,
  and how it fails safely.
```

#### **Works cited**

1. What Are Agentic Design Patterns? 2026 Pattern Catalog | Augment ..., accessed June 15, 2026, [https://www.augmentcode.com/guides/agentic-design-patterns](https://www.augmentcode.com/guides/agentic-design-patterns)  
2. Enterprise AI Agents: Agentic Design Patterns Explained - Tungsten Automation, accessed June 15, 2026, [https://www.tungstenautomation.com/learn/blog/build-enterprise-grade-ai-agents-agentic-design-patterns](https://www.tungstenautomation.com/learn/blog/build-enterprise-grade-ai-agents-agentic-design-patterns)  
3. Agent system design patterns | Databricks on AWS, accessed June 15, 2026, [https://docs.databricks.com/aws/en/agents/agent-system-design-patterns](https://docs.databricks.com/aws/en/agents/agent-system-design-patterns)  
4. Reference architecture: The blueprint for safe and scalable autonomy in SRE and DevOps, accessed June 15, 2026, [https://www.ilert.com/blog/reference-architecture-for-scalable-autonomy-in-sre-and-devops](https://www.ilert.com/blog/reference-architecture-for-scalable-autonomy-in-sre-and-devops)  
5. Design Patterns for Agentic AI and Multi-Agent Systems - AppsTek Corp, accessed June 15, 2026, [https://appstekcorp.com/blog/design-patterns-for-agentic-ai-and-multi-agent-systems/](https://appstekcorp.com/blog/design-patterns-for-agentic-ai-and-multi-agent-systems/)  
6. AI System Design Patterns for 2026: Architecture That Scales, accessed June 15, 2026, [https://zenvanriel.com/ai-engineer-blog/ai-system-design-patterns-2026/](https://zenvanriel.com/ai-engineer-blog/ai-system-design-patterns-2026/)  
7. RAG Evaluation Metrics: Assessing Answer Relevancy, Faithfulness, Contextual Relevancy, And More - Confident AI, accessed June 15, 2026, [https://www.confident-ai.com/blog/rag-evaluation-metrics-answer-relevancy-faithfulness-and-more](https://www.confident-ai.com/blog/rag-evaluation-metrics-answer-relevancy-faithfulness-and-more)  
8. Evaluating the Performance of rag Systems: Metrics Guide ..., accessed June 15, 2026, [https://unstructured.io/insights/rag-evaluation-a-data-pipeline-performance-framework](https://unstructured.io/insights/rag-evaluation-a-data-pipeline-performance-framework)  
9. AI Gateway Architecture: A Guide for Technical Teams | MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/](https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/)  
10. AI control plane: the architecture for AI governance and security | Speakeasy, accessed June 15, 2026, [https://www.speakeasy.com/resources/ai-control-plane](https://www.speakeasy.com/resources/ai-control-plane)  
11. What Is An AI Gateway? | IBM, accessed June 15, 2026, [https://www.ibm.com/think/topics/ai-gateway](https://www.ibm.com/think/topics/ai-gateway)  
12. System Architecture Overview - Envoy AI Gateway, accessed June 15, 2026, [https://aigateway.envoyproxy.io/docs/concepts/architecture/system-architecture](https://aigateway.envoyproxy.io/docs/concepts/architecture/system-architecture)  
13. Agentic Design Patterns | Terezinha Tech Operations (ttoss), accessed June 15, 2026, [https://ttoss.dev/docs/ai/agentic-design-patterns](https://ttoss.dev/docs/ai/agentic-design-patterns)  
14. Choosing the Right Agentic Design Pattern: A Decision-Tree Approach, accessed June 15, 2026, [https://machinelearningmastery.com/choosing-the-right-agentic-design-pattern-a-decision-tree-approach/](https://machinelearningmastery.com/choosing-the-right-agentic-design-pattern-a-decision-tree-approach/)  
15. Model Context Protocol architecture patterns for multi-agent AI systems - IBM Developer, accessed June 15, 2026, [https://developer.ibm.com/articles/mcp-architecture-patterns-ai-systems/](https://developer.ibm.com/articles/mcp-architecture-patterns-ai-systems/)  
16. Model Context Protocol (MCP) explained: A practical technical overview for developers and architects - CodiLime, accessed June 15, 2026, [https://codilime.com/blog/model-context-protocol-explained/](https://codilime.com/blog/model-context-protocol-explained/)  
17. Architecture overview - Model Context Protocol, accessed June 15, 2026, [https://modelcontextprotocol.io/docs/learn/architecture](https://modelcontextprotocol.io/docs/learn/architecture)  
18. What is the Model Context Protocol (MCP)? - Databricks, accessed June 15, 2026, [https://www.databricks.com/blog/what-is-model-context-protocol](https://www.databricks.com/blog/what-is-model-context-protocol)  
19. What is an AI Gateway? The Complete Guide (2026) - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/ai-gateway](https://www.truefoundry.com/blog/ai-gateway)  
20. AI Gateway & LLM Gateway: How They Work and What They Miss - Atlan, accessed June 15, 2026, [https://atlan.com/know/what-is-ai-gateway-llm-gateway/](https://atlan.com/know/what-is-ai-gateway-llm-gateway/)  
21. How to Evaluate RAG Systems: Metrics, Methods, and What to Measure First - Comet, accessed June 15, 2026, [https://www.comet.com/site/blog/rag-evaluation/](https://www.comet.com/site/blog/rag-evaluation/)  
22. RAG Deep Dive Series: Evaluation & Production - Kalvad Blog, accessed June 15, 2026, [https://blog.kalvad.com/rag-deep-dive-series-evaluation-production/](https://blog.kalvad.com/rag-deep-dive-series-evaluation-production/)  
23. AI Gateway Patterns: Cost Control and Reliability at Scale [2026] - Virtido, accessed June 15, 2026, [https://virtido.com/blog/ai-gateway-patterns-production-guide](https://virtido.com/blog/ai-gateway-patterns-production-guide)  
24. Multi Agent Architecture: Patterns, Use Cases & Production Reality - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/multi-agent-architecture](https://www.truefoundry.com/blog/multi-agent-architecture)

---

# AI-ENG-AK — The AI Engineering Mindset - Probabilistic Systems, Human Judgment & Iterative Control

## **Conceptual Glossary**

The formalization of AI engineering as a distinct systems discipline requires a rigorous, shared vocabulary. The following glossary establishes the technical definitions and operational boundaries for the core concepts that define the modern AI engineering mindset.

| Term | Operational Definition |
| :---- | :---- |
| **AI Engineering Mindset** | The systems-level operating philosophy that treats model outputs as non-deterministic behavior distributions rather than static code execution, requiring structural containment, continuous empirical evaluation, and sociotechnical coordination. |
| **Probabilistic Cognitive Infrastructure** | A multi-layered architecture that integrates non-deterministic foundation models to perform high-dimensional cognitive tasks—such as semantic synthesis, parsing, perception, and action preparation—within complex systems. |
| **Deterministic Operational System** | The surrounding traditional software shell consisting of typed schemas, permission boundaries, transactional databases, and hard-coded fallbacks designed to constrain, validate, and isolate probabilistic behaviors. |
| **Technical Humility** | The systematic practice of designing software architectures under the active assumption that models will fail silently, context windows will degrade, prompts will decay, and human operators will exhibit cognitive biases. |
| **Demo Skepticism** | The refusal to equate isolated prototype success under cooperative conditions with systemic production reliability, treating every unverified demo as a latent operational risk. |
| **Decomposition Discipline** | The methodology of breaking complex, open-ended cognitive tasks into discrete, observable, and verifiable steps to minimize the surface area of model autonomy in favor of deterministic control. |
| **Bounded Belief** | The structural restriction of operational trust to a highly specific, evaluated context-space, rejecting generalized assumptions regarding model capability or safety. |
| **Uncertainty Management** | The deliberate architectural capture, classification, and mitigation of non-deterministic system variances—such as factual, retrieval, or tool-state uncertainty—before they cascade into failures. |
| **Iterative Control** | The continuous engineering optimization loop that integrates real-time production traces, automated evaluations, and human corrections back into prompt design, context structures, and system guardrails. |
| **Measurement-Before-Trust** | The operational requirement to establish quantitative, multi-dimensional evaluation baselines grounded in production-realistic data prior to scaling or deploying any model-dependent capability. |
| **Human Judgment Integration** | The sociotechnical design of interfaces and workflows that provide human operators with the evidence, context, time, and explicit authority necessary to actively supervise and override AI systems. |
| **Failure as Signal** | The practice of treating operational errors not as isolated bugs to patch, but as systemic data points that expose flaws in offline evaluations, context boundaries, or architectural constraints. |
| **Anti-Mindset** | A recurring cognitive shortcut or architectural shortcut—such as benchmark theology or prompt mysticism—that introduces systemic risk and operational instability. |

## **AI Engineering Mindset Doctrine**

AI engineering is the discipline of designing probabilistic cognitive capabilities inside bounded, observable, testable, human-governed operational systems. Its mindset is humility under uncertainty, decomposition before automation, measurement before trust, iteration before scale, and production proof before belief.  
The foundational thesis of high-dimensional AI engineering is that a reference architecture is a decision artifact. The useful question is not "Can the model do it?" but rather: "Under what conditions does it do it, how do we know, what happens when it does not, who catches it, what does it cost, what does the user believe, what evidence survives, and why should production trust this behavior?"  
This doctrine shifts the locus of engineering rigor. In classical systems, rigor was embodied in hand-coded algorithms and extensive pre-production unit testing. In probabilistic cognitive systems, rigor relocates to defining clear specifications, upfront guardrails, and architectures that make systems verifiable by design.  
The goal of the AI engineer is to build a system where the internal model behaves probabilistically, but the external system behaves deterministically. The model may be probabilistic; the system must not be confused.

## **AI Engineering Is Not Normal Software Engineering**

AI engineering is not ordinary software engineering with model calls attached. Traditional software engineering assumes that deterministic components, typed interfaces, unit tests, and controlled deployments can produce predictable behavior. AI systems include those same deterministic components, but they also include probabilistic cognitive engines whose outputs vary with context, phrasing, data distribution, model route, retrieval state, and user behavior.

The engineering problem changes.

A model does not fail like a disk, database, or microservice. It may continue producing fluent outputs while violating factual, semantic, policy, permission, or user-expectation constraints. The system can appear healthy while quietly becoming wrong.

AI engineering therefore borrows from several traditions at once:

| Discipline | What AI Engineering Borrows |
| :---- | :---- |
| **Classical Software Engineering** | Typed interfaces, modularity, testing, deployment discipline, rollback, reliability engineering. |
| **Safety Engineering** | Control loops, hazard analysis, degraded modes, safe-state design, incident learning. |
| **Security Engineering** | Least privilege, boundary defense, untrusted input handling, supply-chain review, abuse modeling. |
| **Human Factors** | Trust calibration, review burden, automation bias, interface friction, role clarity. |
| **Data Engineering** | Lineage, freshness, access control, source authority, quality, lifecycle management. |
| **Product Architecture** | Workflow fit, value design, user tolerance, adoption, no-use decisions. |

System-theoretic safety thinking, including STAMP/STPA where consequences justify it, is especially useful because AI failures are often interaction failures: the model, retrieval system, tool, user interface, workflow, and governance layer each behaved locally “as designed,” while the assembled system produced unsafe behavior.

### **Traditional Software vs. AI Systems Engineering**

| Dimensional Vector | Traditional Software Engineering | AI Systems Engineering |
| :---- | :---- | :---- |
| **Logic Definition** | Explicit, hand-written instructions executing deterministic paths. | Learned behavior distributions shaped by prompts, context, weights, retrieval, and route. |
| **Validation Model** | Unit, integration, and acceptance tests against static specifications. | Statistical evals, behavioral regression suites, shadow testing, production correction loops. |
| **Interface Properties** | Rigid APIs and typed contracts. | Typed contracts plus natural-language seams, probabilistic outputs, and dynamic context. |
| **Failure Visibility** | Exceptions, crashes, failed assertions, broken builds. | Silent semantic drift, hallucination, citation failure, tool misuse, overtrust. |
| **Dependency Model** | Versioned libraries and services with explicit APIs. | Model providers, prompts, corpora, embeddings, tools, policies, and user behavior all drift. |
| **Upgrade Strategy** | Code diff plus deterministic regression test. | Route/prompt/schema/corpus/model change plus behavioral eval and deployment manifest. |
| **Security Boundary** | Inputs validated at API edges. | Inputs, retrieved content, model outputs, tool calls, memory, and user actions are all untrusted seams. |
| **UX Principle** | Predictable direct manipulation. | Trust calibration, evidence display, uncertainty handling, strategic friction. |
| **Deployment Meaning** | Development ends and operations begins. | Production begins empirical evidence collection. |
| **Reliability Goal** | Prevent component failure. | Control system behavior despite probabilistic components and shifting context. |

The professional shift is this:

```text
Classical software asks:
  Did the code execute the specified logic?

AI engineering asks:
  Did the system produce acceptable behavior
  under the actual context, evidence, user, policy, route, and risk tier?
```

Rigor does not disappear. It relocates to boundaries, measurements, contracts, evals, telemetry, human authority, and recovery behavior.

## **Probabilistic Cognitive Infrastructure Model**

An enterprise AI application is not a model wrapped in an API call. It is probabilistic cognitive capability embedded inside a deterministic operational system. Reliability emerges from the interaction of model behavior, context, retrieval, tools, permissions, human judgment, telemetry, governance, adoption, sourcing, and operations.

```text
PROBABILISTIC COGNITIVE INFRASTRUCTURE

[ 10. Sourcing / Infrastructure / Vendor Layer ]
        |
[ 9. Adoption / Workflow / Support Layer ]
        |
[ 8. Operations / Governance / Accountability Layer ]
        |
[ 7. Telemetry / Evaluation / Evidence Layer ]
        |
[ 6. Human Judgment / Review / Authority Layer ]
        |
[ 5. Tool / Action / State-Mutation Layer ]
        |
[ 4. Policy / Permission / Boundary Layer ]
        |
[ 3. Context / Evidence / Retrieval Layer ]
        |
[ 2. Deterministic Execution Shell ]
        |
[ 1. Probabilistic Cognitive Core ]
```

These are not perfectly separable boxes. They are interacting control loops. A failure in one layer often appears as a symptom in another: stale retrieval looks like hallucination; weak UI disclosure looks like overtrust; poor adoption looks like model failure; provider drift looks like prompt regression.

### **Infrastructure Layers**

| Layer | Responsibility | Common Failure Mode | Control Mechanism |
| :---- | :---- | :---- | :---- |
| **1. Probabilistic Cognitive Core** | Generates, classifies, summarizes, reasons, perceives, or drafts under context. | Fluent but false or misaligned output. | Route constraints, evals, output validation, bounded task scope. |
| **2. Deterministic Execution Shell** | Parses, validates, retries, structures, routes, and handles exceptions. | Treating model output as already safe or structured. | Schemas, validators, state machines, deterministic fallback. |
| **3. Context / Evidence / Retrieval Layer** | Selects what information enters the model. | Stale, unauthorized, irrelevant, poisoned, or conflicting context. | Source authority, freshness, permission filtering, grounding verification. |
| **4. Policy / Permission / Boundary Layer** | Determines what data, actions, tools, and users are allowed. | Model capability confused with system permission. | RBAC/ABAC, policy-as-code, tenant isolation, pre-egress controls. |
| **5. Tool / Action / State-Mutation Layer** | Converts proposals into real system actions. | Unauthorized, duplicate, partial, or unverifiable side effects. | Tool contracts, idempotency, preconditions, postcondition verification. |
| **6. Human Judgment / Review / Authority Layer** | Gives humans evidence, authority, time, and controls to supervise. | Rubber-stamping, undertrust, overload, unclear accountability. | Review UI, veto, correction tools, escalation, role design. |
| **7. Telemetry / Evaluation / Evidence Layer** | Measures behavior, quality, cost, drift, and incidents. | Logs exist but cannot prove or improve system behavior. | Eval suites, traces, evidence packages, correction capture, dashboards. |
| **8. Operations / Governance / Accountability Layer** | Owns runtime, incidents, policy, compliance, and lifecycle review. | Governance exists only as documents; incidents do not improve the system. | Runbooks, audits, policy gates, inventories, postmortems. |
| **9. Adoption / Workflow / Support Layer** | Ensures humans can use the system safely and productively. | Tool is installed but not adopted, trusted, supported, or understood. | Training, support, incentives, workflow redesign, feedback loops. |
| **10. Sourcing / Infrastructure / Vendor Layer** | Provides compute, models, APIs, hosting, contracts, and exit paths. | Vendor lock-in, route drift, cost shock, data-rights failure. | Gateway abstraction, vendor review, route manifests, exit drills. |

### **Core Principle**

The model is not the system. The system is the coordination of probabilistic cognition with deterministic accountability.

A model can produce language. The infrastructure must decide whether that language may become a claim, a recommendation, a memory, a tool call, a record, a decision, or an action.

## **Humility as Engineering Practice**

Humility in AI engineering is pre-incident realism. It is the discipline of treating unproven assumptions as untrusted until measurement, production evidence, and failure analysis justify confidence.

Humility is not pessimism. It is how professionals avoid being surprised by obvious things slightly later and more expensively.

### **Assumption Audit**

| Assumption to Distrust | Production Countercondition | Required Control |
| :---- | :---- | :---- |
| **The demo represents real use.** | Production inputs are messier, more adversarial, more ambiguous, and less cooperative. | Pilot with representative data, edge cases, and adversarial examples. |
| **Sample data is clean enough.** | Real data has missing fields, duplicates, stale records, malformed files, conflicting sources, and strange user phrasing. | Data readiness review, corpus lifecycle, parser tests, validation gates. |
| **Users will understand the interface.** | Users miss warnings, overtrust outputs, underuse tools, bypass workflows, and invent workarounds. | Trust-calibrated UI, training, feedback capture, adoption telemetry. |
| **Model upgrades are improvements.** | Provider changes can regress task-specific behavior while improving generic benchmarks. | Route-specific regression evals, shadow tests, rollback manifests. |
| **Retrieval grounds the answer.** | Retrieval can surface irrelevant, stale, unauthorized, or poisoned context. | Permission filtering, freshness checks, source authority, citation verification. |
| **Prompt edits are harmless.** | Prompt changes can fix one case and regress another. | Prompt versioning, linked evals, release gates. |
| **Costs scale linearly.** | Retries, long context, agent loops, review burden, support, and shadow evals create hidden cost. | Cost telemetry, resource contracts, loop caps, cost-per-success tracking. |
| **Vendors remain stable.** | Providers change terms, prices, models, limits, routes, regions, and behavior. | Gateway abstraction, vendor inventory, exit plans, provider-change evals. |
| **Humans will review carefully.** | Reviewers rubber-stamp, fatigue, overtrust, or lack evidence to judge. | Evidence UI, review timing signals, veto controls, sampled audits. |
| **Evals catch the important failures.** | Initial eval sets miss rare but important production failures. | Production correction capture, failure replay, eval-set expansion. |
| **Governance documents control behavior.** | Policies are bypassed unless enforced at runtime. | Policy-as-code, gateway checks, permissions, audit evidence. |
| **Fluent output is good output.** | High fluency can hide unsupported claims, wrong calculations, or unsafe advice. | Grounding, semantic validation, source-of-record checks. |
| **Fallbacks are safe by default.** | Backup routes may weaken privacy, capability, policy, or output contracts. | Approved fallback manifests and authority downgrade. |

### **Humility Operating Rule**

Every new AI capability should ship with a written list of assumptions and the evidence required to retire them.

```text
Assumption → Test → Evidence → Boundary → Owner → Review Trigger
```

Until that chain exists, confidence is storytelling.

## **Demo-to-Production Skepticism Model**

A polished demo proves that a system can succeed under cooperative conditions. Production asks whether it can remain useful, safe, affordable, and recoverable under uncooperative conditions.

Every impressive demo is a suspect until production evidence proves otherwise.

| What the Demo Proves | What Production Must Prove | Evidence Required | Release Gate |
| :---- | :---- | :---- | :---- |
| **Possible capability** | Behavior holds across realistic input diversity. | Representative evals, pilot traces, adversarial cases. | Task-specific eval pass. |
| **Interface direction** | Users understand the system role and can use it safely. | Usability tests, correction patterns, trust-calibration review. | Product/UX approval. |
| **Early user excitement** | Users adopt it after novelty fades. | Repeat use, task-completion improvement, support burden, bypass rate. | Adoption review. |
| **Rough technical feasibility** | System works with production data, permissions, latency, scale, and errors. | Integration test, load test, permission test, degraded-mode test. | Operational readiness gate. |
| **Compelling narrative** | Business value survives full cost-to-serve and review burden. | Cost per verified task, human review time, support impact, ROI sensitivity. | Value gate. |
| **Model can answer examples** | Answers are grounded, current, permitted, and verifiable. | Citation fidelity, source authority, freshness, conflict handling. | Grounding gate. |
| **Tool path can execute once** | Tool path handles retries, duplicates, partial state, and postcondition checks. | Idempotency test, rollback/compensation test, source-of-record verification. | Action-verification gate. |
| **Safety appears fine in demo** | System resists prompt injection, data leakage, misuse, and boundary attacks. | Red-team tests, policy checks, tenant isolation tests, egress controls. | Security gate. |
| **Humans can approve output** | Humans have evidence, time, authority, and veto power. | Review UI test, reviewer feedback, approval-quality metrics. | Human-review gate. |
| **Provider route works today** | Route remains stable through drift, outage, rate limits, and migration. | Shadow test, fallback test, vendor review, route manifest. | Sourcing/ops gate. |

### **Skepticism Rule**

A demo is allowed to earn attention. It is not allowed to earn trust.

Trust requires production-shaped evidence.

## **Decomposition Discipline**

The primary failure mode of naive AI design is delegating open-ended tasks to autonomous model loops. Professional AI engineering enforces a strict decomposition discipline: breaking workflows down until the safe automation boundary becomes obvious.  
Before any cognitive workflow is automated, the systems engineer must answer eleven distinct questions:

* **What is the user trying to accomplish?** Map the core business outcome, ignoring the technology, to establish the baseline requirement.  
* **Which steps are deterministic?** Identify tasks—such as mathematical calculations, relational queries, or form submissions—that can be handled by traditional software.  
* **Which steps require language, ambiguity, synthesis, perception, or judgment support?** Isolate the specific tasks that require a probabilistic foundation model.  
* **Which steps require source-of-record lookup?** Pinpoint where the system must fetch data from a traditional database, avoiding model memory retrieval.  
* **Which steps require human authority?** Define high-consequence decision points—such as financial mutations, medical recommendations, or security changes—that must be made by a human operator.  
* **Which steps can be drafted but not executed?** Isolate steps where the model can generate a candidate output for human review, rather than mutating state directly.  
* **Which steps can be automated safely?** Identify low-risk, verifiable steps that can run in the background under strict constraints.  
* **Which steps require approval?** Define the verification boundaries where system execution halts until explicit human sign-off is logged.  
* **Which outputs are verifiable?** Establish which model-generated outputs can be parsed and verified against hard schemas, rules, or test suites.  
* **Which failures are reversible?** Distinguish between actions that can be undone (e.g., drafting an email) and actions that are permanent (e.g., sending an payment).  
* **Which parts should be no-AI?** Identify steps that must be routed completely away from models to ensure absolute determinism and compliance.

Through this decomposition, the engineer isolates the probabilistic core, surrounding it with deterministic verification steps.

## **Uncertainty Management Model**

AI engineering does not eliminate uncertainty. It prevents uncertainty from silently becoming authority.

A mature AI system detects uncertainty, classifies it, chooses a safe response, assigns ownership, and preserves the minimum evidence needed to improve the system.

| Uncertainty Type | Signal | Risk | Safe System Response | Owner | Evidence Boundary |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Factual Uncertainty** | Claim lacks support, conflicts with source, or cannot be verified. | False answer becomes trusted output. | Mark unsupported, remove claim, ask for evidence, or route to human. | Retrieval / Eval Owner | Claim ID, source refs, verifier result. |
| **Retrieval Uncertainty** | Low relevance, sparse results, missing source-of-record, duplicate/conflicting chunks. | Model synthesizes from weak context. | Disclose limited evidence, rerun retrieval, use deterministic lookup, or refuse grounded answer. | Corpus / Retrieval Owner | Retrieval manifest, source IDs, freshness metadata. |
| **Temporal Uncertainty** | Source date/version does not match task time scope. | System answers with obsolete or future-inapplicable information. | Ask time scope, retrieve current source, label historical status, or refuse current claim. | Corpus / Product Owner | Source version/date, user time scope. |
| **Classification Uncertainty** | Low confidence, unstable class assignment, high disagreement, or ambiguous input. | Wrong route, policy, or workflow selected. | Ask clarification, route to exception queue, or use human triage. | Product / Eval Owner | Class candidates, confidence bucket, selected route. |
| **User-Intent Uncertainty** | Request is incomplete, contradictory, underspecified, or high-consequence. | System executes the wrong task. | Present structured choices, ask clarification, or require confirmation. | Product / UX Owner | Intent state, clarification prompt, user selection. |
| **Policy Uncertainty** | Conflicting instructions, unclear data use, missing approval, or boundary condition. | Unsafe or noncompliant output/action. | Deny by default, require approval, or escalate to governance/security. | Policy / Governance Owner | Policy version, decision, reason code. |
| **Permission Uncertainty** | User/resource/tool/tenant scope cannot be proven. | Unauthorized data access or action. | Block before data injection or side effect. | Security Owner | Subject/resource refs, policy decision. |
| **Model-Route Uncertainty** | Route drift, eval expiry, outage, latency anomaly, or unsupported capability. | Weak route handles strong task. | Use approved fallback, downgrade authority, or fail closed. | Platform Owner | Route manifest, eval status, health signal. |
| **Tool-State Uncertainty** | API error, unknown final state, partial write, invalid arguments, duplicate retry. | False success or corrupted state. | Query source of record, compensate/reconcile where possible, escalate if unresolved. | Tool / Ops Owner | Action ID, idempotency key, postcondition result. |
| **Cost / Resource Uncertainty** | Token, tool, latency, retry, or loop budget approaches limit. | Denial-of-wallet, timeout, degraded UX. | Stop loop, return partial, require approval, or queue. | Platform / FinOps Owner | Usage counters, budget state, route ID. |
| **Human-Review Uncertainty** | Very fast approvals, low disagreement, reviewer overload, high correction fatigue. | Rubber-stamping or review collapse. | Add evidence friction, sample secondary review, rebalance queue, or reduce automation. | Human Review / Adoption Owner | Review time bucket, correction/override rate, queue state. |
| **Vendor Uncertainty** | Terms, model behavior, price, rate limit, region, or subprocessor changes. | Contract, quality, or compliance regression. | Trigger vendor review, route eval, fallback test, or exit plan. | Procurement / Platform Owner | Vendor notice, contract ref, route eval result. |

### **Uncertainty Rule**

Uncertainty should change system authority.

```text
higher uncertainty → less autonomy
higher consequence → more evidence
weaker evidence → weaker claim
unknown permission → deny
unknown state → verify before proceeding
```

## **Measurement-Before-Trust Framework**

Trust is not granted by fluency, novelty, confidence, or benchmark rank. Trust is earned through repeated measurement inside bounded contexts.

The system should not ask, “Do we trust this model?” It should ask:

```text
For this task,
under this data class,
with this route,
for this user,
under this policy,
at this cost,
with this evidence,
how often does the system behave acceptably?
```

### **Metric Families**

| Metric Family | What It Measures | Example Signals |
| :---- | :---- | :---- |
| **Task Outcome** | Whether the workflow achieved the intended business result. | Completion rate, verified success, rework, downstream error rate. |
| **Grounding and Evidence** | Whether claims are supported by permitted, current, authoritative sources. | Claim support, citation fidelity, source authority, freshness, conflict handling. |
| **Schema and Semantic Validity** | Whether outputs are structured and meaningful. | Schema validity, enum validity, cross-field invariants, semantic consistency. |
| **Tool and Action Verification** | Whether proposed actions are authorized and completed correctly. | Tool-call validity, permission decision, postcondition success, false-success rate. |
| **Human Review Quality** | Whether humans meaningfully supervise the system. | Review time, override rate, correction categories, reviewer agreement, fatigue signals. |
| **Trust and Adoption** | Whether users rely appropriately and the workflow creates value. | Overtrust, underuse, bypass rate, repeat use, support burden. |
| **Cost and Resource** | Whether value survives full operating cost. | Cost per verified task, token/tool/retry cost, review labor, support cost. |
| **Reliability and Operations** | Whether the system remains available, bounded, and recoverable. | Latency p50/p95/p99, error rate, retry loops, incident rate, rollback rate. |
| **Drift and Regression** | Whether behavior changes over time. | Eval regression, correction clusters, route drift, corpus freshness decay. |
| **Governance and Evidence** | Whether the organization can prove what happened. | Deployment manifest, policy decision, audit evidence, retention compliance. |

### **Metric Selection Rule**

Metrics must be selected by pattern, task, and risk tier. A support assistant needs resolution and repeat-contact metrics. A document pipeline needs field precision and coordinate grounding. A workflow agent needs postcondition success and idempotency. A research agent needs claim support and source authority.

Generic model scores are not enough. Usage is not enough. Token volume is not enough. User delight is not enough.

### **Trust Gate**

| Condition | Trust Decision |
| :---- | :---- |
| Metrics exist only for demo cases. | Do not scale. |
| Metrics measure usage but not correctness. | Do not claim production value. |
| Metrics measure output but not downstream outcome. | Pilot only. |
| Metrics pass offline but fail production correction patterns. | Revise evals and contract boundaries. |
| Metrics show stable behavior within task/risk/data boundaries. | Trust only inside those boundaries. |

Trust is bounded by evidence. Evidence expires when the system changes.

## **Iterative Control Loop**

AI engineering is an empirical control discipline. Production behavior teaches the system what the original design, evals, prompts, data, policies, users, and assumptions missed.

```text
ITERATIVE CONTROL LOOP

specify
  ↓
prototype
  ↓
evaluate
  ↓
constrain
  ↓
deploy narrowly
  ↓
observe
  ↓
capture corrections
  ↓
update evals
  ↓
revise contracts
  ↓
harden architecture
  ↓
expand cautiously
  ↓
monitor drift
  ↓
rollback / recalibrate
  ↓
repeat
```

### **Control Loop Stages**

| Stage | Core Question | Primary Artifact | Owner |
| :---- | :---- | :---- | :---- |
| **Specify** | What task, user, data, risk, and value are in scope? | Use-case decision record, pattern card, requirements. | Product / Architect |
| **Prototype** | Can the capability work under bounded conditions? | Prototype, prompt/schema draft, route candidate. | Engineering |
| **Evaluate** | Does behavior meet task-specific evidence requirements? | Eval suite, golden set, failure taxonomy. | Eval Owner |
| **Constrain** | What contracts must surround the probabilistic core? | Prompt, schema, retrieval, tool, permission, resource contracts. | Architect / Security |
| **Deploy Narrowly** | Can a limited population use it safely? | Pilot plan, rollback route, support path. | Product / Ops |
| **Observe** | What happens in real workflow conditions? | Telemetry, traces, corrections, support signals. | Ops / Observability |
| **Capture Corrections** | What did humans repair, reject, override, or misunderstand? | Correction dataset, feedback events. | Product / Eval |
| **Update Evals** | Which production failures must become regression tests? | Expanded eval suite. | Eval Owner |
| **Revise Contracts** | Which boundary failed or was missing? | Updated contract registry. | Contract Owner |
| **Harden Architecture** | What structural change prevents recurrence? | Architecture patch, route change, UI change, policy update. | Architect / Platform |
| **Expand Cautiously** | Which new users/workflows are safe to add? | Scale decision, risk review. | Product / Governance |
| **Monitor Drift** | Is behavior changing as data, users, models, or vendors change? | Drift dashboard, shadow evals. | Platform / Eval |
| **Rollback / Recalibrate** | Should we revert, degrade, pause, or continue? | Rollback manifest, incident/postmortem. | Ops / Governance |

### **Developer Feedback Speedups**

Iteration fails when feedback is too slow. Mature AI platforms build speed without removing gates.

| Speedup Mechanism | Purpose |
| :---- | :---- |
| **Long-Running Eval Environments** | Keep representative staging systems alive so evals can run without repeated setup. |
| **Failure Replay Harness** | Reconstruct failed trajectories from scoped evidence and secure references. |
| **Context Forking** | Resume debugging from a past state instead of replaying an entire agent run. |
| **Local Mock Services** | Let developers test gateways, tools, databases, and provider responses safely. |
| **Contract Unit Tests** | Quickly test schemas, tool preconditions, policy decisions, and prompt variables. |
| **Shadow Evaluation** | Compare candidate routes without changing production behavior. |

### **Control Loop Question**

The recurring engineering question is:

```text
What did production teach us
that our design, evals, prompts, data, policies, users,
and assumptions missed?
```

## **Human Judgment Integration Model**

Enterprise AI systems must reject both extremes:

```text
AI replaces human judgment.
Humans are safely in control because someone clicked approve.
```

Human judgment must be designed into the workflow. A human who cannot inspect, refuse, correct, or understand the output is not in the loop; they are a liability sponge.

### **Human Judgment Requirements**

| Requirement | What It Provides | Pattern Examples |
| :---- | :---- | :---- |
| **Evidence Access** | Reviewer can inspect sources, records, calculations, coordinates, logs, or rationale. | Research Agent, Decision Cockpit, Document Intelligence. |
| **Context Visibility** | Reviewer sees relevant workflow history and system state. | Support Assistant, Workflow Agent, Human Review Queue. |
| **Time to Review** | Reviewer is not forced into instant rubber-stamping. | High-risk actions, legal/compliance review, underwriting. |
| **Authority Clarity** | Reviewer knows whether they are advising, approving, correcting, or deciding. | Decision Cockpit, Workflow Automation, Governed Agentic Workflow. |
| **Veto Power** | Reviewer can reject or halt output/action. | Human Review Queue, Coding Agent, Tool Execution. |
| **Correction Tools** | Reviewer can edit fields, claims, coordinates, routes, or payloads. | Document Intelligence, Multimodal Review, Support Assist. |
| **Escalation Path** | Reviewer can move the case to a higher authority or expert. | Support, compliance, security, regulated workflows. |
| **Accountability Boundary** | Human and system responsibility are explicit. | Decision Support, HR/finance/legal workflows. |
| **Uncertainty Display** | Reviewer can see unsupported, conflicting, stale, or low-confidence areas. | RAG, research, analytics, decision support. |
| **Anti-Pressure Controls** | System detects review overload, rushed approval, or rubber-stamping risk. | High-risk queues and regulated workflows. |
| **Feedback Capture** | Corrections improve evals, prompts, data, and training. | All review-enabled patterns. |

### **Risk-Tiered Human Authority**

| Risk Tier | Human Role |
| :---- | :---- |
| **Low** | User edits, accepts, or rejects suggestions. |
| **Medium** | Human reviews outputs before external use or material write-back. |
| **High** | Human approves action with evidence, rationale, and audit record. |
| **Regulated / Critical** | Human authority is primary; AI may organize evidence, draft, or check but not decide. |

### **Human Review Anti-Patterns**

| Anti-Pattern | Failure |
| :---- | :---- |
| **Approval Button With No Evidence** | Human cannot judge, only absorb liability. |
| **Review Queue Without Time Budget** | Review becomes rubber-stamping under throughput pressure. |
| **AI Recommendation Framed as Default** | Automation bias increases. |
| **No Reject Path** | Human cannot exercise authority. |
| **No Correction Capture** | System repeats the same failures. |
| **No Escalation Path** | Edge cases become hidden local workarounds. |

Meaningful human control is not a checkbox. It is an interface, workflow, evidence package, authority model, and organizational practice.

## **Bounded Belief Doctrine**

A professional AI engineer never believes in a model generally. Belief is bounded by task, data, route, user, policy, evidence, time, and observed behavior.

Believe an AI system only within the boundaries where it has been evaluated, observed, and operationally constrained.

### **Belief Boundaries**

| Boundary | What May Be Believed | Evidence Required | Breach Behavior |
| :---- | :---- | :---- | :---- |
| **Task Class** | The system works for a narrow, defined task. | Task-specific evals and production traces. | Route to discovery, no-AI, or human review. |
| **Data Class** | The system may process approved data types. | Data classification, permissions, retention policy. | Block context injection or require governance review. |
| **User Role** | Authorized users may access approved capability. | RBAC/ABAC, tenant scope, role mapping. | Deny or escalate. |
| **Model Route** | A route is acceptable for a task/risk tier. | Route manifest, eval pass, monitoring. | Use approved fallback or fail closed. |
| **Prompt Version** | This prompt behaves acceptably for linked evals. | Versioned prompt, hash, regression suite. | Rollback or block release. |
| **Retrieval Scope** | Retrieved context is permitted and authoritative enough. | Source IDs, freshness, ACL/RLS, grounding checks. | Disclose, reretrieve, or refuse. |
| **Tool Authority** | The system may propose or execute specific actions. | Tool contract, permission policy, idempotency, postcondition tests. | Block before side effect or compensate/reconcile. |
| **Eval Evidence** | Offline evidence supports limited deployment. | Golden set, adversarial cases, benchmark by task. | Pilot only or redesign. |
| **Production Behavior** | Live behavior remains within acceptable bounds. | Telemetry, corrections, incidents, support signals. | Recalibrate, roll back, or pause. |
| **Failure Response** | The system can recover safely. | Degraded mode, rollback manifest, runbook, incident drill. | Reduce authority or block production. |
| **Governance Approval** | Risk tier is organizationally accepted. | Approval record proportional to risk. | Require review before expansion. |
| **Time Window** | Evidence remains current enough. | Review date, model/corpus/vendor-change status. | Revalidate before relying. |

### **Bounded Belief Rule**

```text
No evidence outside the boundary.
No trust outside the boundary.
No authority outside the boundary.
```

The boundary is part of the system.

## **Failure as Signal Doctrine**

Operational failures are inevitable in probabilistic environments. The wrong response is to patch the nearest prompt and declare the system healed. A failure is information about which assumption was false, which boundary was weak, which metric was missing, or which workflow was misunderstood.

### **Failure Learning Matrix**

| Failure Reveals | Update Target | Artifact Changed | Owner |
| :---- | :---- | :---- | :---- |
| **Eval missed a case** | Evaluation suite. | New golden/adversarial case, failure taxonomy update. | Eval Owner |
| **Prompt allowed unsafe behavior** | Prompt contract. | Prompt version, refusal behavior, linked evals. | Prompt Owner |
| **Retrieved evidence was wrong or stale** | Retrieval/corpus contract. | Source authority, freshness, chunking, index lifecycle. | Corpus Owner |
| **Output passed schema but was wrong** | Semantic/factual validator. | Business rules, grounding checks, verifier tests. | Domain / Eval Owner |
| **Tool call failed or misfired** | Tool/action contract. | Preconditions, idempotency, postcondition checks, compensation path. | Tool Owner |
| **Permission boundary failed** | Policy contract. | RBAC/ABAC rule, tenant filter, approval state, egress control. | Security / Governance |
| **User overtrusted output** | User expectation design. | Evidence UI, uncertainty display, confirmation gate. | Product / UX |
| **Human review collapsed** | Human review model. | Queue design, review instructions, sampling, workload allocation. | Review Owner |
| **Users bypassed the workflow** | Adoption system. | Training, support, workflow redesign, incentive alignment. | Adoption / Product |
| **Provider/model drifted** | Sourcing and route strategy. | Route manifest, fallback, vendor review, shadow eval. | Platform / Procurement |
| **Cost spiked** | Resource contract. | Token budget, loop cap, retry policy, route selection. | Platform / FinOps |
| **Fallback weakened safety** | Degraded mode. | Fallback manifest, authority downgrade, policy parity test. | Ops / Platform |
| **Incident response was slow** | Runbook. | Alert threshold, owner, escalation, rollback drill. | Operations |
| **Pattern was wrong** | Architecture pattern card. | No-use condition, pattern selection rule, reference blueprint. | Architect |

### **Failure Handling Rule**

A failure should produce at least one durable artifact update:

```text
eval case
contract revision
policy update
runbook change
UI correction
training example
pattern no-use condition
route manifest change
```

If nothing durable changed, the system learned nothing.

## **Anti-Mindset Catalog**

Systemic failures in enterprise AI projects are often caused by the adoption of flawed mental models.  
These "Anti-Mindsets" treat probabilistic systems as if they were simple, deterministic, or inherently safe, leading to poor designs, security vulnerabilities, and project failures.

| Anti-Mindset | Primary Symptoms | Why It Is Tempting | Operational Failure Mode | Healthy Posture |
| :---- | :---- | :---- | :---- | :---- |
| **Demo Worship** | Standardizing core architecture based on a small set of successful prototype runs under optimal conditions. | Provides immediate progress signals; generates high interest and secures funding from leadership teams. | Complete system collapse when exposed to messy production payloads, retrieval noise, and edge cases. | Treat the demo as a highly fragile proof of concept; verify all system capabilities with robust evaluation suites. |
| **Prompt Mysticism** | Treating prompt engineering as an art or magic, manually tweaking words without structured testing. | Requires zero engineering setup; provides immediate feedback loops on single local test inputs. | Extreme system fragility; prompt changes fix one bug while silently regressing dozens of other scenarios. | Treat prompts as versioned, strictly controlled system configuration files verified via regression testing. |
| **Benchmark Theology** | Assuming high ranks on public LLM leaderboards guarantee success in domain-specific workflows. | Simplifies vendor and model selection; avoids the hard engineering work of building custom evaluations. | Poor real-world performance; models fail to handle specific enterprise schemas, terminology, and workflows. | Build domain-specific evaluation suites; rank candidate models using real historical production telemetry. |
| **Automation Maximalism** | Attempting to automate highly complex, open-ended workflows without human review boundaries. | Promises massive cost reduction and complete operational scale-out in a single project release. | High rates of silent failures, compliance violations, and severe brand damage when models fail silently. | Decompose workflows; automate low-risk, highly structured steps and use models to draft actions for human review. |
| **Chatbox Monoculture** | Forcing all user interactions through free-form natural language chat interfaces. | Simplifies design tasks; leverages the popular, familiar design patterns of consumer AI products. | Extremely high user friction, search failures, high latency, and severe automation-bias patterns. | Use structured UI components; deploy chat only when handling high-entropy, highly ambiguous inputs. |
| **Human-in-the-Loop Theater** | Using human review simply to click "approve" without providing context, evidence, or veto power. | Keeps the system fast and high-throughput while claiming regulatory and risk compliance on paper. | Operators experience rapid automation bias, rubber-stamping incorrect outputs and missing silent errors. | Design interfaces that show supporting evidence and force active verification through interaction friction. |
| **Vendor Faith** | Treating vendor claims, dynamic updates, and standard model alignments as robust security layers. | Outsources complex security and safety challenges; reduces initial development overhead and setup times. | Sudden system regression or security failure when models are updated or alignments are bypassed. | Build independent input and output safety filters; treat every external model as a hostile dependency. |
| **Metric Cosmetics** | Tracking usage, user logins, and token volume while ignoring quality, grounding, and error rates. | Dashboards look positive; metrics are easy to gather and show immediate, high-velocity adoption signs. | Silent operational decay; enterprise users abandon the tool because of high error rates and support burdens. | Track task success rates, grounding, citation precision, human veto rates, and error costs. |
| **Agent Romanticism** | Designing systems with autonomous, multi-turn, unstructured planning loops. | Creates highly impressive demos; aligns with the narrative of building fully autonomous systems. | Infinite execution loops, runaway API costs, high latencies, and severe cascading failure paths. | Constrain agents using structured workflow loops with clear state machines and hard stop boundaries. |
| **Scale Prematurely Syndrome** | Rolling out systems to large user bases before establishing evaluations, rollbacks, and support structures. | Driven by market competition and executive pressure to rapidly demonstrate progress. | High incident rates, system outages, massive support ticket volumes, and rapid trust loss among users. | Scale incrementally; prove system stability in shadow and canary releases before expanding user access. |
| **Determinism Delusion** | Treating probabilistic model completions as if they were predictable, static code paths. | Simplifies code; allows developers to write traditional software integrations without exception paths. | Frequent system crashes and validation failures when models return unexpected output formats or schemas. | Isolate models behind strict validation contracts; design robust fallbacks and degraded operational modes. |
| **RAG Salvationism** | Assuming that adding dynamic retrieval (RAG) completely eliminates hallucination and security risks. | Avoids complex model fine-tuning; provides a simple mechanism to ground model outputs in enterprise data. | Models hallucinate over irrelevant search results or execute instructions injected into retrieved context. | Build evaluation layers to verify context relevance, semantic grounding, and data-instruction isolation. |
| **Governance Theater** | Writing extensive legal and policy guidelines while lacking real-time runtime control enforcement. | Satisfies compliance audits on paper; avoids the technical work of building gateway filter systems. | Policy guidelines are bypassed by users or ignored by models; security breaches occur despite written policies. | Implement real-time policy evaluation and runtime gatekeeping at the API gateway layer. |
| **Cost Blindness** | Designing complex agentic chains without tracking token volume, support costs, or review overhead. | Simplifies initial prototyping; assumes API tokens are too cheap to impact business viability. | Runaway operating costs, high cloud bills, and low ROI when human review and support burdens are counted. | Calculate the cost per successful outcome; track token and model execution costs for every user flow. |

## **AI Engineering Maxims**

These maxims define the professional operating code of AI engineering.

| Maxim | Meaning |
| :---- | :---- |
| **The model is not the system.** | The system includes context, retrieval, schemas, tools, permissions, telemetry, humans, governance, operations, sourcing, and adoption. |
| **Fluency is not truth.** | Polished prose is not evidence, correctness, authority, or safety. |
| **Retrieval is not evidence unless verified.** | Retrieved context must be permissioned, current, authoritative, and claim-supporting. |
| **A demo is not a deployment.** | Cooperative success proves possibility, not production reliability. |
| **Human approval is not human judgment.** | Judgment requires evidence, time, authority, correction tools, and veto power. |
| **Failure budget is not harm permission.** | A workflow that cannot tolerate error needs stronger gates or no AI. |
| **Every model route is bounded.** | Routes are approved only for specific tasks, data classes, users, and risk tiers. |
| **Every agent needs a stop condition.** | Unbounded loops become cost, latency, state, and safety failures. |
| **Every tool action needs authorization and verification.** | The system must check permission before execution and confirm state after execution. |
| **Every memory needs scope, provenance, and deletion.** | Persistent state must be permissioned, inspectable, and governed. |
| **Every fallback must preserve safety properties.** | Degraded modes may reduce capability, not policy, privacy, or evidence requirements. |
| **Every eval is a release contract.** | Prompt, schema, route, retrieval, tool, and policy changes require risk-appropriate evaluation. |
| **Every production failure should teach the system.** | Failures should update evals, contracts, policies, runbooks, UI, or pattern cards. |
| **No-use is an architectural decision.** | Deterministic software, manual workflow, or no-AI is often the correct answer. |
| **The system should be more deterministic than the model.** | Variance belongs inside bounded cognitive work, not at state, permission, evidence, or action boundaries. |
| **Inside the box, the model may dance; at the edges, it signs paperwork.** | Let models synthesize internally; enforce strict contracts at every system boundary. |

## **Professional Practices of AI Engineers**

AI engineering is a professional discipline. It requires repeatable habits, not heroic prompt tinkering.

| Practice | Daily Behavior | Durable Artifact | Failure Prevented |
| :---- | :---- | :---- | :---- |
| **Write Assumptions Explicitly** | State task, data, user, route, risk, cost, and failure assumptions before building. | Assumption log / decision record. | Hidden demo assumptions. |
| **Decompose Before Automating** | Split workflows into deterministic, probabilistic, human, and no-AI steps. | Workflow decomposition map. | Over-automation. |
| **Build Evals Before Scaling** | Create task-specific tests before broad rollout. | Eval suite / golden set. | Demo-to-production collapse. |
| **Test Messy and Hostile Conditions** | Include malformed input, missing data, conflicting sources, injection attempts, and edge cases. | Adversarial eval set. | Fragile prompt success. |
| **Instrument What Matters, Minimize What Hurts** | Capture route, cost, validation, correction, and breach signals while redacting or referencing sensitive payloads. | Telemetry and evidence schema. | Blind operation or overcollection. |
| **Use Deterministic Code Where Sufficient** | Prefer rules, SQL, forms, parsers, and APIs when exactness is required. | No-AI / deterministic design record. | RAG-as-database and model overreach. |
| **Isolate Model Behavior Behind Contracts** | Validate inputs, outputs, tools, permissions, resources, routes, and memory. | Contract registry. | Probabilistic spillover. |
| **Route by Task and Risk** | Match model route to task complexity, data class, latency, cost, and consequence. | Route manifest. | Weak model on strong task. |
| **Preserve Human Authority Where Consequence Demands It** | Require human approval with evidence for high-impact decisions/actions. | Human review workflow. | Rubber-stamped harm. |
| **Keep Rollback and Degraded Modes Ready** | Maintain rollback manifests, approved fallback routes, and manual recovery paths. | Runbook / deployment manifest. | Irrecoverable production failure. |
| **Track Cost-to-Serve Continuously** | Measure cost per verified outcome, including model, retrieval, tools, infra, review, support, and failures. | Cost dashboard. | ROI fantasy and cost bombs. |
| **Update Evals From Production Corrections** | Convert edits, overrides, incidents, and support failures into regression cases. | Eval update log. | Repeated production failure. |
| **Treat Vendor Changes as System Changes** | Re-evaluate affected routes when providers change models, terms, limits, regions, or prices. | Vendor-change review / route eval. | Silent dependency drift. |
| **Treat Adoption as Architecture** | Design training, support, incentives, and workflows as part of the system. | Adoption plan / support model. | Installed-but-unused AI. |
| **Document No-Use Conditions** | State when the pattern must not be used. | Pattern card no-use section. | Pattern overreach. |
| **Prefer Boring Recoverable Systems** | Choose designs that are observable, bounded, reversible, and understandable. | Reference architecture / runbook. | Dazzling fragile autonomy. |

The professional AI engineer does not ask, “How impressive can this be?” first.

They ask, “How does this fail, and what survives?”

## **Capstone Cross-Canon Synthesis**

AI-ENG-AK is the operating philosophy that binds the AI Engineering Systems Canon together. Each prior domain provides a specialized discipline. AK defines the mindset required to use those disciplines without being seduced by demos, benchmarks, or model fluency.

| Canon Domain | Engineering Contribution | AK Mindset Commitment |
| :---- | :---- | :---- |
| **Prompt and Context Architecture** | Instruction hierarchy, context windows, memory, state, prompt configuration. | Treat prompts and context as versioned, bounded, testable system configuration. |
| **Corpus Engineering** | Data provenance, metadata, document lifecycle, source authority. | Treat knowledge as governed infrastructure, not loose text dumped into a model. |
| **Retrieval and Freshness** | Search, RAG, reranking, citation, source conflict, freshness. | Treat retrieved context as candidate evidence requiring verification. |
| **Model Selection and Routing** | Model choice, capability profile, cost, route class, fallback. | Trust routes only inside evaluated task/data/risk boundaries. |
| **Model Adaptation** | Fine-tuning, adapters, specialization, weight behavior. | Treat adaptation as a new system behavior requiring regression evidence. |
| **Regression Control** | Evals, golden sets, shadow testing, release gates. | Treat every change as a hypothesis that must survive measurement. |
| **Throughput and Serving** | Latency, batching, queues, streaming, model serving. | Treat performance as part of reliability and user trust. |
| **Agentic Orchestration** | Planning, graph execution, multi-agent workflows. | Constrain autonomy with state machines, budgets, checkpoints, and human authority. |
| **Tool Contracts and Action Verification** | Tools, APIs, idempotency, postconditions, source-of-record checks. | Never confuse generated action text with authorized completed action. |
| **Multimodal and Realtime Systems** | Images, audio, video, speech, streaming interaction. | Treat non-text evidence and realtime behavior as special uncertainty surfaces. |
| **UI Agents** | Browser/UI state, visual control, interface actions. | Give agents no UI authority without permission, verification, and rollback. |
| **Production Pathologies** | Failure modes, retry storms, silent regressions, brittle chains. | Expect failure; design containment before incidents teach it violently. |
| **Boundary Defense and Supply Chain** | Prompt injection, tenant isolation, SBOM/AI-BOM, artifact provenance. | Treat every model, tool, corpus, and vendor dependency as untrusted until bounded. |
| **Resource Abuse** | Cost bombs, loops, denial-of-wallet, resource exhaustion. | Bound every loop, token budget, tool call, retry, and route. |
| **UX Resilience and Trust** | Degraded modes, transparency, contestability, expectation management. | Make uncertainty, authority, fallback, and evidence visible to users. |
| **Human Review** | Review queues, maker-checker, escalation, reviewer burden. | Human approval is meaningful only with evidence, time, veto, and accountability. |
| **Telemetry, Evaluation, and Evidence** | Observability, eval suites, traces, proof artifacts. | Production is the primary teacher; evidence must be scoped, structured, and usable. |
| **Operations and Governance** | Runbooks, incidents, policy, audit, accountability. | Governance must execute at runtime, not merely decorate documents. |
| **Sustainability** | Energy, carbon, cost, routing, lifecycle impact. | Route by value, quality, cost, resource impact, and lifecycle—not model spectacle. |
| **Product Architecture** | Use-case selection, workflow fit, value design, no-AI path. | Ask whether AI belongs before asking which model to use. |
| **Adoption Systems** | Training, incentives, support, workflow redesign, skill preservation. | Adoption is architecture; humans are part of the system. |
| **Sourcing Strategy** | Build/buy/open/vendor, data rights, exit, control plane. | Own or control the surfaces that define safety, value, evidence, and exit. |
| **Contract Thinking** | Deterministic boundaries around probabilistic cores. | Every seam needs an owner, enforcer, validation method, and breach behavior. |
| **Reference Architectures** | Failure-aware pattern cards and no-use conditions. | Reuse patterns only when their assumptions, controls, and degraded modes fit. |

### **Canonical Integration**

```text
AI engineering is probabilistic capability
inside deterministic accountability.

Capability comes from models.
Reliability comes from systems.
Trust comes from evidence.
Safety comes from boundaries.
Value comes from workflow fit.
Durability comes from iteration.
```

AK is the mindset that keeps all of those truths active at the same time.

## **Final Canon Handoff Statement**

The AI Engineering Systems Canon closes with a simple discipline:

```text
Do not believe the model.
Believe the measured system.
```

AI engineering is not normal software engineering with a chat box bolted onto the side. It is the practice of placing probabilistic cognition inside deterministic accountability, then measuring, constraining, observing, correcting, governing, and operating it until production earns trust.

The model may draft. The system must verify.

The model may synthesize. The system must ground.

The model may plan. The system must authorize.

The model may propose action. The system must check permission, preserve state, verify postconditions, and recover from failure.

The model may impress. The system must survive.

Across these volumes, the canon has defined the layers required to make that possible: context architecture, corpus engineering, retrieval, freshness, model routing, serving, orchestration, tools, action verification, multimodal understanding, realtime interaction, UI agents, pathologies, boundary defense, supply-chain security, resilience, trust, human review, telemetry, evaluation, evidence, operations, governance, sustainability, product architecture, adoption, sourcing, contract thinking, and reference architectures.

The final professional posture is unsentimental:

```text
Decompose before automating.
Measure before trusting.
Constrain before scaling.
Observe before believing.
Preserve human judgment where consequence demands it.
Treat failure as instruction.
Prefer boring recoverable systems over dazzling fragile ones.
```

Build systems that can say no.

Build systems that can degrade safely.

Build systems that can prove what happened.

Build systems that can be corrected.

Build systems whose users understand the boundary between assistance and authority.

The future of AI engineering will not be won by whoever worships the largest model. It will be won by whoever builds the most disciplined systems around probabilistic capability.

Inside the box, the model may dance.

At the edges, it signs paperwork.

---