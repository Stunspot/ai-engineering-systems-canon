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

[← Back to Canon Map](../canon-map.md)