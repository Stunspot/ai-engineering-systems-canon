# AI-ENG-S — Production Pathologies - Hallucination, Malformed Output & Runaway Behavior

## **Doctrinal Foundations: The Fail-Closed Paradigm**

The structural integrity of a high-dimensional artificial intelligence system depends on a fundamental law of operating boundaries: production reliability is not achieved by asking the model to behave; it is achieved by constraining, validating, observing, replaying, and recovering from the ways probabilistic systems fail.1 In clean development environments, models appear to cooperate because inputs are curated, contexts are short, APIs are stable, and execution volume is low.1 However, when these systems migrate to production, they encounter the chaotic realities of real-world inputs, asynchronous network boundaries, third-party schema drift, and long-running stateful executions.1 Treating these failures as vague behavioral anomalies like "the model hallucinated" masks the underlying engineering breakdowns.3 They are systemic structural boundary failures where probabilistic outputs cross into deterministic environments in a form that downstream software interprets as valid, grounded, authorized, or complete.3  
This paradigm builds directly upon the foundational capabilities established in preceding canon architectures. The system inherits the output-contract and validation harness protocols of preceding reports to enforce structural constraints.3 It integrates context-tenure and instruction-state tracking to manage state across prolonged interactions.7 Source-authority and knowledge-hygiene metrics define the integrity of the data ingested, ensuring retrieval-augmented systems do not suffer from "citation laundering"—the process of citing irrelevant files to support fabricated statements.9 Loop budgets, action-verification gates, and deterministic-wrapper mechanics provide the programmatic boundaries needed to isolate the agent's operations.1 When these internal controls are weak or misconfigured, ordinary probabilistic variances degrade into severe production incidents.1 The goal of a reliability engineer is to map these failure entries, block their propagation, and deploy automated containment and recovery pathways.1  
To transition from brittle prototypes to production-grade deployments, architects must abandon the hope of perfect model execution.1 The useful question is not "Did the demo work?" but "What breaks in production that did not break in the demo, where does the failure enter, how does it propagate, how does it present to users and downstream systems, how can it be detected, how can it be replayed, and what controls keep it from becoming an incident?".11 Production pathologies must be modeled as structural boundary transitions.3 At each boundary—whether between model and parser, parser and database, or agent and external API—the system must implement strict contract validations, explicit timeouts, and fallback routings.1

## **Conceptual Glossary of AI Systems Reliability**

The following glossary defines the core terms and operational metrics governing production AI reliability.

### **Table 1: Conceptual Glossary**

| Term | Technical Definition | Primary Operational Metric | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Production Pathology** | A recurring, systemic failure mode in deployed AI systems arising from the intersection of probabilistic generation with deterministic constraints.1 | Systemic Incident Rate (R_inc) | 0.00% of automated transaction pipelines |
| **Confabulation** | The generation of linguistically fluent but factually unsupported, inaccurate, or internally contradictory output.13 | Factual Grounding Ratio (G_fact) | > 99.5% of generated statements |
| **Malformed Output** | Output that violates the syntactical, structural, or lexical expectations of the downstream parser or schema compiler.3 | Parser Exception Rate (R_parse) | < 0.10% of API responses |
| **Schema Validity** | The state where a generated structural payload conforms exactly to a defined schema's keys, types, array bounds, and nullability constraints.3 | Schema Rejection Rate (R_reject) | 0.00% under native constrained decoding |
| **Semantic Validity** | The state where a syntactically and structurally valid payload contains values that align with real-world business rules and logic.3 | Semantic Violation Rate (R_sem_violation) | < 0.50% of evaluated transactions |
| **Contract Drift** | The gradual divergence between a model's semantic interpretation of a schema and the downstream system's execution expectations.16 | Value Distribution Drift (Delta_drift) | PSI (Population Stability Index) < 0.10 |
| **Citation Integrity** | The exact, verifiable alignment between a generated claim and the specific region, page, or cell coordinates of the retrieved source.18 | Citation Alignment Score (A_cite) | > 98.0% verification precision |
| **Grounding Failure** | An execution where a model answers from pre-trained prior weights while appearing to use retrieved context.21 | Prior-Weight Influence Delta (Delta_prior) | Recall-to-prior ratio < 0.05 |
| **Instruction Loss** | The degradation of system directives as they pass through long contexts, summaries, or tool-output streams.7 | Instruction Adherence Index (I_adh) | > 99.0% on adversarial distractors |
| **Harness Drift** | The degradation of evaluation or safety wrapper templates during recursive state reconstruction or repair loops.12 | Template Consistency Score (C_temp) | Zero unmapped system modifications |
| **Brittle Chain** | A multi-step sequence where minor, unobserved state corruptions compound to cause catastrophic failure at the final step.1 | Cascade Probability (P_cascade) | Joint F1 score drop < 1.0% per step |
| **Tool Loop** | A state where an agent repeatedly calls tools with identical or slightly altered parameters without making progress.1 | State Repetition Count (C_rep) | Limit of <= 2 identical repeated states |
| **Repair Loop** | A multi-turn pattern where a system attempts to fix a schema violation but continues to emit malformed payloads.1 | Repair Intertwining Index (I_repair) | Complete termination after <= 2 attempts |
| **Runaway Behavior** | Unbounded model execution that bypasses rate limits, loops continuously, or consumes compute credits without completing.1 | Budget Depletion Velocity (V_burn) | Auto-halt on 100% of budget breaches |
| **Non-Deterministic Regression** | A drop in accuracy, formatting, or instruction adherence caused by unannounced model updates or system changes.17 | Regression Variance Delta (sigma^2_reg) | Accuracy variance standard deviation < 0.02 |
| **Golden Trace** | A recorded, high-fidelity log of a successful production run containing all prompts, tool calls, and states.11 | Trace Match Fidelity (F_trace) | 100% structural equivalent match |
| **Stochastic Replay** | The technique of debugging non-deterministic agent executions by playing back recorded events and mocking external states.11 | Replay Alignment Precision | 100% reproducibility of logic paths |
| **Replay Divergence** | The point where a replayed run deviates from the recorded path due to unintercepted time calls or system changes.11 | Divergence Step Offset (S_div) | 0.00% divergence on unchanged code |

## **Production Pathology Taxonomy**

Production pathologies are distinct from ordinary software bugs, model errors, or malicious security exploits.1 A standard bug is a deterministic flaw in code execution where the same input always yields the same failure.25 A model error is a localized failure of understanding or generation (e.g., misclassifying an image or mistranslating a phrase).12 A security exploit is an intentional, adversarial manipulation of system boundaries to hijack execution.5  
In contrast, a **production pathology** is a systemic failure mode that emerges from the integration of probabilistic token generation with rigid downstream software contracts.1 These failures are often silent, non-deterministic, and behavioral.16 They present as valid structural patterns that contain semantically corrupted or completely fabricated values.3

### **Table 2: Production Pathology Taxonomy**

| Pathology | System Boundary | Downstream Impact | Primary Root Cause | Prevention Strategy |
| :---- | :---- | :---- | :---- | :---- |
| **Factual Confabulation** | Generator to Application Boundary 3 | User receives convincing but false information 13 | Model prior weights override retrieved context 21 | Search-grounding pipelines with strict citation verification 21 |
| **Malformed Structured Payload** | Parser to Local Code Environment 3 | System crashes, unhandled exceptions, and stalled runs 3 | Missing syntax constraints, unescaped quotes, or truncated tokens 4 | Native constrained decoding and strict Pydantic integration 3 |
| **Schema & Contract Drift** | Model Provider to Schema Registry 16 | Silent data corruption, downstream processing errors 3 | Unannounced provider updates and semantic shifts 16 | Population distribution monitoring and schema-diff review 16 |
| **Ungrounded Tool Call** | Execution Engine to Target Database 2 | Unauthorized mutations, data corruption, and resource loss 5 | Speculative tool-call generation on unconfirmed intents 2 | Least-privilege schemas and explicit human-in-the-loop gates 5 |
| **Out-of-Bounds Runaway Loop** | Orchestration Layer to Network Gate 1 | Rapid API credit exhaustion, denial-of-wallet 1 | Lack of hard limits on iterations, cost, or execution times 1 | Run-level token and currency budgets managed by gateway proxies 1 |
| **Brittle Chain Collapse** | Inter-Step State Transformations 1 | Compounding errors that invalidate the final output 1 | Silent failure propagation without verification checks 2 | Pre-action and post-action assertions at every step boundary 12 |
| **Stochastic Regression** | Production Telemetry to CI/CD 17 | Intermittent failures that pass standard test suites 17 | GPU float non-associativity, MoE routing variances 25 | Multi-run distribution testing and stochastic replay audits 11 |

## **Failure Boundary Map**

AI systems fail when probabilistic outputs cross deterministic boundaries without adequate validation, authority checks, observability, or recovery controls. A production pathology is rarely “just the model being wrong.” It is usually a boundary failure: a generated claim, payload, citation, tool call, or state update is accepted by another subsystem as if it were valid, grounded, authorized, or complete.

### **Artifact 3: Failure Boundary Map**

```text
+--------------------------------------------------------------------------------
| PRODUCTION AI FAILURE BOUNDARY MAP
+--------------------------------------------------------------------------------
|
| [ User / External Input / Scheduled Run ]
|          |
|          v
| [ Boundary 1: Intake, Policy, and Sanitization ]
|   prompt injection | unsafe request | malformed file | hostile source content
|          |
|          v
| [ Intent and Task Parser ]
|   ambiguous intent | missing entities | wrong risk class | stale user state
|          |
|          v
| [ Boundary 2: Retrieval and Source Routing ]
|   empty retrieval | stale corpus | low-authority source | permission leakage
|          |
|          v
| [ Evidence and Context Assembly ]
|   lost instructions | context rot | truncation | citation laundering
|          |
|          v
| [ Boundary 3: Model Generation / Planning ]
|   confabulation | contradiction | policy invention | malformed plan
|          |
|          v
| [ Boundary 4: Output Contract Validation ]
|   syntax failure | schema failure | semantic invalidity | grounding failure
|          |
|          +----------------------------+
|          |                            |
|          v                            v
| [ Repair / Retry Controller ]   [ Validated Payload / Response ]
|   repeated failures               |
|   repair drift                    v
|   loop saturation            [ Boundary 5: Tool / Action Gate ]
|          |                   unauthorized tool | wrong tool | missing approval
|          |                         |
|          |                         v
|          |                   [ Execution Boundary ]
|          |                   timeout | partial commit | duplicate side effect
|          |                         |
|          |                         v
|          |                   [ Observation and Verification ]
|          |                   stale observation | ignored exception | unknown state
|          |                         |
|          |                         v
|          |                   [ State Update and User Response ]
|          |                   false success | unsupported claim | audit gap
|          |
|          v
| [ Containment Boundary ]
|   halt | degrade | compensate | escalate | incident | replay
|
+--------------------------------------------------------------------------------
| Invariant:
|   A boundary crossing is safe only when the receiving system can validate,
|   authorize, observe, and recover from the artifact it receives.
+--------------------------------------------------------------------------------
```

To diagnose propagation, failures are classified into seven architectural categories:

* **Model-level errors** emit false, unsupported, contradictory, or off-policy content.
* **Interface-level errors** violate schemas, parsing rules, citation formats, or downstream consumer expectations.
* **Workflow-level errors** corrupt shared state across a chain of transformations.
* **Tool-level errors** invoke incorrect APIs, fabricate tool results, retry dangerously, or misread system observations.
* **Grounding-level errors** cite wrong, stale, insufficient, or uninspected evidence.
* **Runtime-level errors** alter execution via latency spikes, silent truncation, rate limits, or unannounced provider updates.
* **Control-level errors** continue, repair, retry, or escalate transactions at incorrect times.

The operational goal is to prevent failure propagation. Each boundary should either transform the artifact into a validated state or halt safely with a structured failure object.

## **Hallucination as Operational Subtypes**

Labeling all incorrect model outputs as “hallucinations” is too broad for production troubleshooting. Useful incident diagnosis requires identifying the specific fabrication class, locating the boundary where it entered the system, and applying the correct containment strategy.

### **Table 3: Hallucination Subtype Model**

| Subtype | Structural Presentation | Systemic Root Cause | Diagnostic Signal | Containment & Recovery |
| :---- | :---- | :---- | :---- | :---- |
| **Factual Hallucination** | Output states an incorrect fact without inventing a source or tool result. | Model prior or weak context overpowers evidence. | Claim fails source entailment or factual evaluation. | Retrieve stronger evidence, suppress unsupported claim, or answer with uncertainty. |
| **Citation Hallucination** | Output cites non-existent, irrelevant, outdated, or insufficient sources. | Citation generated as decoration rather than evidence binding. | Citation target missing, stale, irrelevant, or unsupported. | Require claim-to-source support validation before output. |
| **Tool Hallucination** | Output claims a tool ran or invents tool results. | Model treats expected action/result as if it were observed. | Claimed tool call missing from execution trace or observation ledger. | Accept only orchestrator-owned tool observations as execution evidence. |
| **State Hallucination** | Output claims an operation is complete when it is pending, failed, partial, or unknown. | Conversational completion outruns action verification. | User-facing status conflicts with authoritative state. | Block completion claims until post-action verification reconciles state. |
| **Schema Hallucination** | Output generates unmapped keys, invalid enum values, wrong nesting, or non-contract fields. | Weak schema constraints, ambiguous examples, contract drift, or missing constrained decoding. | Parser or schema validator rejects payload. | Use constrained decoding where available, strict validators, schema registries, and bounded repair. |
| **Policy Hallucination** | Output invents rules, exceptions, permissions, or compliance justifications. | Policy context is missing, stale, ambiguous, or mixed with untrusted data. | Generated policy differs from canonical policy store. | Retrieve authoritative policy, validate against policy engine, and suppress invented rules. |
| **Grounding Hallucination** | Answer appears grounded but is not supported by retrieved documents or selected evidence. | Long context, distractors, weak citation binding, or poor evidence selection. | Low overlap/support between claim and evidence packet. | Enforce evidence sufficiency checks and abstain when evidence is inadequate. |
| **Confidence Hallucination** | Output expresses high certainty despite weak evidence or high ambiguity. | The response policy lacks calibrated uncertainty and abstention rules. | Confidence language exceeds evidence strength or evaluator confidence. | Use evidence adequacy, calibration checks, uncertainty policy, and abstention gates. |
| **Action-Authority Hallucination** | Output implies the model is allowed to perform an action it is not authorized to perform. | Tool authority is confused with conversational ability. | Proposed action violates autonomy boundary, approval policy, or role permission. | Enforce runtime authorization outside the model. |
| **Temporal Hallucination** | Output treats stale, pending, future, or provisional information as current truth. | Freshness checks, time bounds, or state versioning are absent. | Claim timestamp/version mismatches current source of record. | Revalidate freshness and mark stale or pending information explicitly. |

The operational question is not “Did the model hallucinate?” but:

```text
What kind of hallucination crossed which boundary,
which validator failed to stop it,
and what state must be corrected or contained?
```

## **Malformed Output and Structural Failure**

Enforcing structured output is one of the most common challenges in production reliability. Valid JSON is not sufficient. A payload may parse successfully while still violating schema, business logic, policy, grounding, security, or downstream usability constraints.

A production system should therefore validate outputs in layers.

### **Table 4: Layered Output Integrity Model**

| Integrity Layer | Verification Scope | Core Verification Mechanism | Failure Mode Presentation |
| :---- | :---- | :---- | :---- |
| **1. Syntax Validity** | Raw format compliance. | Parser compliance check for JSON, YAML, XML, TOML, Markdown, or other expected structure. | Unescaped quotes, trailing commas, broken fences, truncated structures. |
| **2. Schema Validity** | Structural contract compliance. | Typed validators, JSON Schema, Pydantic models, enum bounds, `additionalProperties: false`, nullability checks. | Missing required keys, type mismatches, invented fields, wrong enum values. |
| **3. Semantic Validity** | Cross-field and business-rule consistency. | Business validators, relational checks, domain constraints, arithmetic checks, date/state logic. | Impossible dates, invalid totals, inverted pricing, contradictory statuses. |
| **4. Policy Validity** | Compliance, safety, permission, and risk rules. | Policy engine, autonomy boundary checks, approval validation, tenant authorization. | Prohibited content, unauthorized action, wrong risk class, missing approval. |
| **5. Grounding Validity** | Alignment between claims and evidence. | Citation support checks, source entailment, coordinate/cell/region verification. | Claims unsupported by retrieved context or citations. |
| **6. Downstream Usability** | Safety of values passed to tools or users. | Sanitization, escaping, parameterization, path/domain allowlists. | Script injection, path traversal, unsafe URL, raw internal state exposed. |
| **7. User-Facing Clarity** | Suitability of output presentation. | Formatting normalization, channel adaptation, refusal/status clarity. | Raw JSON shown to users, malformed tables, ambiguous status, overconfident uncertainty. |

The physical implementation of this layered verification pipeline requires explicit assertions at the boundary. The following Python example illustrates strict schema boundaries, provider-status checks, and cross-field business validation using Pydantic v2.

```python
from __future__ import annotations

from decimal import Decimal
from typing import Any, Literal

from pydantic import BaseModel, ConfigDict, Field, ValidationError, model_validator


class ProductionContract(BaseModel):
    """
    Doctrinal schema contract enforcing strict object shapes.

    extra="forbid" blocks undeclared fields instead of silently dropping them.
    Semantic validators enforce business rules that raw JSON Schema cannot
    fully express.
    """

    model_config = ConfigDict(extra="forbid")

    transaction_id: str = Field(pattern=r"^TXN-[0-9]{4}-[0-9]{4}$")
    base_amount: Decimal = Field(gt=0)
    discounted_amount: Decimal | None = None
    is_promotional: bool
    status: Literal["PENDING", "COMMITTED", "RECONCILED"]

    @model_validator(mode="after")
    def verify_financial_logic(self) -> "ProductionContract":
        """
        Layer 3: semantic validity.

        Enforces cross-field business rules after syntax and schema checks pass.
        """

        if self.is_promotional:
            if self.discounted_amount is None:
                raise ValueError(
                    "discounted_amount is required when is_promotional is True."
                )
            if self.discounted_amount >= self.base_amount:
                raise ValueError(
                    "discounted_amount must be strictly less than base_amount."
                )
        else:
            if self.discounted_amount is not None:
                raise ValueError(
                    "discounted_amount must not be populated when is_promotional is False."
                )

        return self


def execute_boundary_ingestion(
    raw_payload: str,
    api_response_metadata: dict[str, Any],
) -> ProductionContract:
    """
    Validates a model-generated payload before any downstream code consumes it.

    This boundary:
    1. checks provider-level generation status,
    2. rejects refusals/incomplete generations,
    3. parses and validates the payload,
    4. returns only typed, validated state.
    """

    status = api_response_metadata.get("status")
    response_type = api_response_metadata.get("type")

    if status == "incomplete":
        reason = api_response_metadata.get("incomplete_reason", "unknown")
        raise RuntimeError(f"Generation truncated before contract boundary: {reason}")

    if response_type == "refusal":
        message = api_response_metadata.get("refusal_message", "no refusal message")
        raise RuntimeError(f"Provider refusal intercepted: {message}")

    try:
        return ProductionContract.model_validate_json(raw_payload)
    except ValidationError as exc:
        # In production, log the structured validation errors and route to a
        # bounded repair path. Do not pass the raw payload downstream.
        raise ValueError(f"Contract violation detected: {exc.errors()}") from exc
```

### **Boundary Rule**

```text
Parsed is not valid.
Schema-valid is not semantically valid.
Semantically valid is not authorized.
Authorized is not executed.
Executed is not verified.
Verified is not user-safe until formatted and reported honestly.
```

## **Broken Schemas and Contract Drift Map**

Structured outputs are software interfaces, and like any interface, they regress. Contract drift occurs when prompts, model behavior, schemas, validators, SDKs, or downstream consumers diverge. Some drift breaks validation immediately. More dangerous drift preserves syntactic validity while shifting value distributions or semantics.

### **Table 5: Schema Failure and Contract Drift Map**

| Failure Point | Failure Origin | Failure Mechanism | Prevention & Control |
| :---- | :---- | :---- | :---- |
| **Prompt / Schema Mismatch** | System prompt templates. | Examples show retired fields while validators expect new structures. | Release manifests bind prompt version, schema version, validator version, and model route. |
| **Old Schema Emission** | Prompt examples, cached templates, model behavior, or stale tool definitions. | Model emits retired properties or old nested shapes. Strict validators reject; permissive validators may silently drop. | Pin schema versions; reject extra properties; run schema compatibility tests. |
| **Enum Value Shift** | Probabilistic decoding or ambiguous field descriptions. | Model invents synonyms or unmapped categories. | Use explicit enums, constrained decoding where possible, and semantic enum validators. |
| **Field Requirement Drift** | API/provider/downstream contract updates. | Optional fields become required, required fields become deprecated, or nullability changes. | Contract tests against live/staged downstream systems before rollout. |
| **Response-Shape Changes** | Model serialization behavior or SDK response handling. | Object is wrapped in arrays, nested envelopes, Markdown, or tool-call wrappers unexpectedly. | Schema registry with compatibility checks and response-envelope normalization. |
| **SDK Serialization Drift** | Client-side library updates. | SDK modifies field names, null handling, date serialization, or response object access. | Dependency pinning, lockfiles, release manifests, and contract tests. |
| **Strict Schema Rejections** | Parser/compiler limitations. | Valid-looking schemas fail provider-side compilation due to unsupported features, regexes, recursion, or nesting. | Pre-flight schema compilation checks in CI/CD. |
| **Validator Silent Drops** | Permissive validation defaults. | Unknown fields are dropped, hiding model drift and payload corruption. | Configure validators to reject extras and log rejected payloads. |
| **Semantic Distribution Drift** | Model/provider/routing changes. | Schema remains valid but values shift: more nulls, different categories, suspicious defaults. | Monitor value distributions, PSI/KL drift, null-rate shifts, and enum frequency changes. |
| **Consumer Contract Drift** | Downstream service changes. | Valid model payload no longer matches business logic or execution expectations. | Consumer-driven contract tests and staged traffic canaries. |

### **Contract Drift Rule**

```text
A schema version is not just field names.
It is prompt examples, validator behavior, model route, SDK serialization,
downstream consumer expectations, and observed value distributions.
```

## **Tool-Use Failure Pathologies**

Tool-use failures are critical because outputs cross from text into system actions. An ungrounded tool call, malformed payload, duplicate retry, ignored exception, or false success claim can corrupt databases, trigger unauthorized actions, leak data, or waste large budgets.

### **Table 6: Tool-Use Failure Map**

| Pathology | Mechanical Trigger | Detection Signal | Prevention & Containment |
| :---- | :---- | :---- | :---- |
| **Phantom Tool Call** | Model describes a tool action that was never physically invoked. | Claimed action missing from orchestrator trace. | Treat only orchestrator-owned tool logs as execution evidence. |
| **Wrong Tool Selection** | Model selects a tool with similar name, overlapping affordance, or vague description. | Executed tool route differs from planned capability. | Design unique, non-overlapping tool definitions and enforce tool-policy routing. |
| **Unavailable Tool Reference** | Model attempts to invoke retired, disabled, or unauthorized tool. | Registry lookup fails or policy denies capability. | Dynamic registry exposure scoped to user, task, environment, and risk. |
| **Invalid Tool Payload** | Generated arguments fail type, enum, range, or required-field checks. | Pre-execution validator rejects payload. | Validate all arguments before dispatch; route to bounded repair. |
| **Semantically Invalid Payload** | Payload passes schema but violates business logic or state constraints. | Domain validator, policy engine, or target precondition fails. | Add semantic validators and pre-action state checks. |
| **Stale Tool Observation** | Model relies on cached result or old observation instead of current state. | Observation timestamp/version precedes relevant state change. | Invalidate caches, require freshness bounds, and re-query source of record. |
| **Ignored Tool Exception** | Tool error is summarized as success. | Tool returns error/timeout but user-facing status says complete. | Normalize errors into typed observation objects and block completion claims. |
| **Fabricated Tool Result** | Model invents results instead of executing or reading tool output. | Output contains data absent from tool observation. | Only structured observations may update task state. |
| **Duplicate Tool Call** | Slow response or retry logic causes repeated calls with same logical payload. | Parallel or repeated requests share payload hash. | Enforce idempotency keys and duplicate detection for mutating tools. |
| **Non-Idempotent Retry** | Client retries state-changing call after timeout without reconciliation. | Multiple mutations or unknown pending state. | Use durable idempotency records; hold/reconcile unknown states before retry. |
| **Tool Repair Loop** | Model repeatedly fixes failed arguments but remains invalid. | Similar invalid payloads exceed repair threshold. | Cap repairs, return structured failure, and escalate when needed. |
| **Action-Success Hallucination** | Model claims a change occurred without verification. | Completion message mismatches authoritative state. | Enforce post-action verification against authoritative system state. |
| **Observation Injection** | Tool output contains instruction-like content that contaminates later prompts. | Tool result includes commands, hidden prompts, or untrusted text. | Treat tool output as data, sanitize, and preserve role boundaries. |
| **Authority Creep** | Tool grants broader access than task requires. | Tool call touches resources outside task scope. | Least-privilege tools, scoped credentials, and policy-gated execution. |

Tool failures should be handled by the same state doctrine used in action verification:

```text
proposed -> validated -> authorized -> executed -> observed -> verified -> reconciled
```

## **Invalid Citations and Evidence Integrity**

In Retrieval-Augmented Generation (RAG) systems, citations are the primary mechanism for auditing correctness.21 However, naive RAG implementations often suffer from citation failures, citing irrelevant passages or fabricating references entirely.18 To address this, production systems evaluate citations using a multi-dimensional framework.18  
To ensure verifiability, the system enforces the **Doctrine of "Provenance Before Relevance"**: *No knowledge object should enter retrieval, memory, citation, or model-facing context unless the system knows where it came from, who owns it, what authority it carries, what scope it applies to, what transformations it has undergone, and how its lifecycle is governed*.9  
Furthermore, grounding must be maintained by passing raw organizational information through a structured progression of discrete states known as the **Epistemic Hierarchy** before loading it into active context 9:

1. **Raw Source:** Unprocessed files stored permanently in the system of record.9  
2. **Normalized Document:** Structured Markdown representing the structural layout.9  
3. **Corpus Object:** The primary, metadata-anchored parent asset containing source authority.9  
4. **Canonical Chunk:** Semantically cohesive text segments with parent-child pointers.9  
5. **Extracted Claim:** Atomic, model-verifiable assertions.9  
6. **Embedding Record:** High-dimensional vectors kept in strict sync with canonical chunks.9  
7. **Generated Summary:** Model-synthesized overviews to accelerate discovery.9  
8. **Entity Record:** Unique nodes in the knowledge graph mapping real-world concepts.9  
9. **Citation:** Structural pointers mapping generated text back to exact source coordinates.9  
10. **Context Object:** Transient, permission-filtered context blocks loaded into active memory.9

### **Table 7: Citation Integrity Framework**

| Verification Domain | Verification Objective | Mechanical Validation Technique | Failure Mitigation Strategy |
| :---- | :---- | :---- | :---- |
| **Source Existence** | Confirms the cited document exists in the index.18 | Metadata identifier matching against the index database.18 | Suppress statements citing missing documents.18 |
| **Source Freshness** | Verifies the document version is current.18 | Timestamp comparison against active database indices.33 | Update search routing to prioritize active versions.33 |
| **Source Authority** | Checks that the source is trusted.9 | Filtering out draft or unapproved source metrics.9 | Block low-authority files from overriding systems of record.9 |
| **Claim Relevance** | Verifies text matches the cited passage.18 | NLI (Natural Language Inference) entailment checks.13 | Reject claims that contradict the source context.13 |
| **Coordinate Precision** | Maps claims back to exact coordinates.12 | OCR coordinate and bounding box calculations.12 | Pinpoint exact page regions in the user interface.12 |
| **Quote Fidelity** | Verifies generated quotes match the source.12 | Calculating the longest common subsequence.12 | Flag quotes that modify source words.12 |
| **Evidence Sufficiency** | Confirms the cited text proves the claim.20 | Model-based evaluations of claim coverage.20 | Flag claims that overstate the cited facts.20 |
| **Conflict Resolution** | Resolves contradictions across sources.12 | Cross-document semantic mismatch checks.12 | Prompt the user with conflicting options.12 |
| **Decorative Citations** | Identifies citations added purely for formatting.12 | Removing the citation and evaluating accuracy.12 | Suppress citations that do not map to active context.12 |

## **Instruction Loss, Harness Drift, and Context Degradation**

As interactions extend across long contexts, system directives often degrade.7 This "shallow long-context adaptation" creates performance drops where models maintain high accuracy up to a critical threshold, but collapse once that limit is exceeded.23 For example, a model with a 128K context window may experience a 45.5% drop in accuracy once context fills past 50%, rendering long-running stateful agents unreliable.7

### **Table 8: Instruction Retention and Harness Drift Model**

| Drift Driver | Systemic Origin | Degradation Mechanism | Evaluation Method |
| :---- | :---- | :---- | :---- |
| **Attention Dilution** | Prompt length pressure 7 | Context token expansion spreads the model's attention too thin.7 | Position sensitivity stress tests 7 |
| **Context Rot** | Long conversations 7 | Chat histories and tool logs introduce noise that degrades the instructions.7 | Multi-turn drift and recall tests 7 |
| **Role Confusion** | Document integration 12 | Model fails to distinguish system instructions from retrieved data.5 | Role-conflict and prompt injections 5 |
| **Instruction Truncation** | Tool-output flooding 7 | Long tool outputs push critical directives outside the attention span.7 | Context-truncation check logs 7 |
| **Harness Drift** | Repair loops 12 | Re-framing prompts during format repairs alters safety rules.12 | Template schema checks 12 |
| **Retrieval Contamination** | Ingestion pipelines 12 | Malformed documents introduce conflicting instructions.5 | Adversarial distractor injections 5 |
| **Memory Staleness** | Long-term memory 12 | Recalling outdated states overrides active instructions.12 | Memory-drift evaluation runs 12 |

## **Runaway Behavior and Loop Pathologies**

Unbounded, stateful agent execution represents a major financial and operational risk.1 In recursive agent loops, input token costs grow quadratically as tool outputs and trace logs accumulate across turns.24 The total input tokens consumed in a naive N-turn loop can be modeled mathematically as:  
Total Token Cost = N * S + u * N * (N + 1) / 2 + r * N * (N - 1) / 2 24  
Where S represents the fixed system prompt tokens, u is the new input tokens per iteration (user message + tool result), and r is the generated output tokens per iteration.24 If an agent enters an infinite loop, this quadratic token accumulation can rapidly deplete budgets and trigger "denial-of-wallet" dynamics.1 Every repair loop is an agent loop; therefore, every agent loop must have a hard budget, stop condition, and escalation path.1

### **Table 9: Runaway Behavior Model**

| Loop Classification | Mechanical Trigger | Detection Signal | Containment Control | Escalation Path |
| :---- | :---- | :---- | :---- | :---- |
| **Recursive Tool Loop** | Model repeatedly retries failed tool calls.1 | State repetition count exceeds limit (C_rep > 2).2 | Hard limit on tool calls per run.1 | Suspend run, alert operator.2 |
| **Format Repair Loop** | Parser failures trigger constant re-generation.1 | High repair attempts with identical schemas.1 | Halt after <= 2 repair attempts.3 | Fall back to default states.1 |
| **Retrieval Loop** | Search returns empty or conflicting results.1 | Identical search queries in sequential steps.2 | Max search limit of <= 3 attempts.2 | Fall back to manual review.12 |
| **Planner Loop** | Agent continually updates plans without acting.1 | High token usage with zero tool execution.2 | Hard limit on planning turns.2 | Prompt user with options.2 |
| **Reflection Loop** | Model critiques and updates outputs endlessly.1 | Output similarity score exceeds 0.95 across turns.2 | Hard limit on reflection cycles.2 | Deliver the current output.2 |
| **Clarification Loop** | Agent repeatedly asks the user for the same inputs.1 | Conversational pattern repetition.12 | Hard limit on clarification requests.12 | Route session to support queues.12 |
| **Fallback Loop** | System gets stuck switching between fallback models.1 | Rapid model-switching cycles.30 | Hard limit on fallback routing.30 | Return a managed error.1 |
| **UI Click Loop** | UI agent clicks elements without page updates.12 | Bounding box coordinates remain unchanged.12 | Target element coordinate-matching checks.12 | Escalate to user, pause automation.12 |
| **Voice Repair Loop** | Voice agent loops on phonetic misunderstandings.12 | Repeated word correction patterns.12 | Phonetic spelling and digit-by-digit modes.12 | Transfer session to support queue.12 |
| **Multi-Agent Loop** | Collaborating agents cycle tasks endlessly.1 | Unchecked hands across agents.1 | Hard limit on handoff transfers.2 | Terminate session, alert admin.1 |
| **Retry Storm Loop** | Upstream servers retry failed requests at once.1 | Latency spikes combined with high traffic.30 | Exponential backoff with random jitter.30 | Trigger gateway rejection rules.30 |

## **Brittle Chains and Cascading Failures**

AI workflows often fail through quiet, multi-stage state corruptions. Each individual stage may appear locally valid, while the overall chain drifts away from the intended task. By the time the final response or action is produced, the failure may be difficult to trace unless every boundary preserved explicit verification artifacts.

### **Table 10: Brittle Chain Failure Model**

```text
+--------------------------------------------------------------------------------
| BRITTLE CHAIN FAILURE MODEL
+--------------------------------------------------------------------------------
|
| [ Input ]
|     |
|     v
| [ Preprocessing ]
|     |
|     v
| [ Retrieval ]
|     |
|     v
| [ Context Assembly ]
|     |
|     v
| [ Generation / Planning ]
|     |
|     v
| [ Validation ]
|     |
|     +--> validation failure
|     |       |
|     |       v
|     |  [ Bounded Repair ]
|     |       |
|     |       +--> repaired payload returns to Validation
|     |       +--> repair exhausted -> Managed Failure
|     |
|     v
| [ Tool / Action Gate ]
|     |
|     v
| [ Execution ]
|     |
|     v
| [ Observation ]
|     |
|     v
| [ Verification and State Reconciliation ]
|     |
|     v
| [ Final Response / Citation / Logging ]
|
+--------------------------------------------------------------------------------
| Rule:
|   Each boundary must emit enough evidence for replay.
|   Silent state corruption becomes catastrophic when the chain lacks checkpoints.
+--------------------------------------------------------------------------------
```

| Boundary Stage | Expected Contract | Possible Silent Corruption | Telemetry & Validator | Recovery Action |
| :---- | :---- | :---- | :---- | :---- |
| **1. Input** | Clean raw data structure. | Unsupported file formats, rotated pages, hostile prompt content, malformed records. | File validators, MIME checks, source trust signals. | Reject, sanitize, quarantine, or route to alternate parser. |
| **2. Preprocessing** | Standardized text/layout/media representation. | Collapsed layout, OCR drift, lost tables, corrupted encoding. | Layout IoU, CER/WER, parser confidence, structural checks. | Re-render, upscale, alternate OCR/parser, human review. |
| **3. Retrieval** | Relevant, authorized, fresh context. | Empty results, stale matches, low-authority sources, permission leakage. | Retrieval MRR, authority filters, freshness checks, permission audits. | Re-query, broaden/narrow query, resolve conflicts, refuse if no evidence. |
| **4. Context Assembly** | Compact, grounded, instruction-safe prompt. | Token overflow, lost system instructions, role confusion, untrusted data collision. | Context budget logs, instruction-invariant checks, role-boundary checks. | Prune, reorder, summarize safely, isolate untrusted content. |
| **5. Generation / Planning** | Valid response or plan candidate. | Confabulation, contradictions, invented policy, overconfident uncertainty. | Grounding checks, contradiction checks, policy validation. | Suppress, regenerate with constraints, ask clarification, or escalate. |
| **6. Validation** | Schema, semantic, policy, and grounding validity. | Invented fields, bad enums, logically impossible payloads. | Strict validators, semantic business checks, policy engine. | Bounded repair or managed failure. |
| **7. Repair** | Corrected payload without policy drift. | Repeated invalid outputs, altered task, hidden assumption insertion. | Repair counters, diff checks, template checksum. | Halt repairs after limit; return structured failure. |
| **8. Tool / Action Gate** | Valid, authorized, scoped action. | Wrong tool, missing approval, excessive authority, stale intent. | Tool policy, autonomy boundary, approval hash, intent trace. | Block, ask approval, replan, or fail closed. |
| **9. Execution** | Controlled side-effect attempt. | Duplicate mutation, timeout ambiguity, partial commit. | Idempotency ledger, timeout logs, transaction IDs. | Reconcile before retry; compensate or hold unknown state. |
| **10. Observation** | Typed result evidence. | API error ignored, stale cache, malformed observation. | Observation schema, freshness/version check, error normalization. | Normalize error, re-query source of record, hold if unknown. |
| **11. State Reconciliation** | Verified authoritative state. | Local memory diverges from database/tool reality. | Authoritative readback, predicate checks, state-version comparison. | Update state, compensate, escalate, or mark unknown. |
| **12. Final Response** | Accurate, grounded user-facing output. | Unsupported claim, false completion, unsafe certainty. | Claim-support checks, completion-state checks, uncertainty policy. | Suppress, revise, or return managed status. |
| **13. Citation** | Verifiable evidence references. | Decorative citations, wrong region, stale source. | Source existence, quote fidelity, coordinate/cell support. | Remove ungrounded citation or regenerate with evidence. |
| **14. Logging** | Complete replayable trace. | Missing tool payloads, hidden prompts, lost observations. | Trace completeness validator. | Use recording proxies and fail closed for high-impact gaps. |

## **Contradictory Outputs and Consistency Failures**

A consistency failure occurs when a model generates individually plausible statements that cannot all be true at the same time. These failures must be classified to distinguish acceptable variation from production faults.

### **Table 11: Contradiction and Consistency Framework**

| Contradiction Class | Systemic Origin | Mechanical Validation | Impact on Production | Acceptable vs Failure |
| :---- | :---- | :---- | :---- | :---- |
| **Within-Answer Contradiction** | Long contexts, weak synthesis, or conflicting intermediate drafts. | Cross-paragraph semantic entailment checks. | User receives conflicting, unusable guidance. | **Failure:** suppress or repair before output. |
| **Planner-Final Contradiction** | Structured planner state diverges from final response or tool payload. | Compare structured planner state against final response and tool payloads. | System may execute actions that mismatch the stated plan. | **Failure:** block action or regenerate. |
| **Source-Answer Contradiction** | Retrieval conflict, poor source ranking, or prior-weight override. | NLI/support checks against cited sources. | Model outputs claims denied by evidence. | **Failure:** reject or surface conflict. |
| **Tool-Result Contradiction** | Tool errors or observations are ignored. | Compare tool observations with generated status. | Model reports success when action failed or is pending. | **Failure:** user-facing status must match verified state. |
| **Memory-State Contradiction** | Memory/cache lags source of record. | Compare session memory against authoritative database/tool state. | Agent acts on outdated user or task data. | **Failure:** refresh state before action. |
| **Cross-Run Contradiction** | Non-deterministic generation or routing differences. | Multi-run semantic clustering and variance measurement. | Inconsistent behavior on identical inputs. | **Acceptable:** minor phrasing variance; **failure:** semantic/action variance. |
| **Streaming Contradiction** | Speculative tokens emitted before stable result. | Compare spoken/streamed chunks to finalized state. | Model corrects itself mid-response or outruns tool status. | **Contextual:** acceptable for low-risk self-correction; failure for high-impact claims. |
| **Policy Contradiction** | Weak or stale policy context. | Evaluate output against canonical policy source. | Compliance or safety breach. | **Failure:** block and log. |
| **Citation Contradiction** | Decorative or misbound citations. | Compare cited claims against exact source text/region. | Claims misrepresent evidence. | **Failure:** citations must support attached claims. |
| **Multimodal Contradiction** | Text, table, chart, image, or video evidence disagree. | Cross-modal consistency checks. | Explanation contradicts raw visual/tabular evidence. | **Failure:** surface conflict or route to review. |

### **Consistency Rule**

```text
Phrasing variance is normal.
Semantic contradiction is a fault.
Action contradiction is a safety boundary breach.
```

## **Non-Deterministic Regressions**

Generative AI systems are inherently non-deterministic.25 Even at temperature 0, floating-point non-associativity on parallel GPU clusters, Mixture-of-Experts (MoE) routing paths, and API load balancing introduce subtle variations across executions.25 An update designed to fix a specific bug can cause regressions in other areas of the pipeline.31

### **Table 12: Non-Deterministic Regression Model**

| Regression Source | Mechanical Root Cause | Production Impact | Prevention & Mitigation |
| :---- | :---- | :---- | :---- |
| **Model Version Changes** | Provider API updates 16 | Minor shifts in weights alter how prompts are processed.16 | Pinning specific model snapshot versions (gpt-4o-2024-08-06).25 |
| **Provider Updates** | Dynamic server routing 25 | Provider alters load balancing across datacenters.25 | Monitoring fingerprint parameters to verify updates.25 |
| **Prompt Edits** | System prompt changes 12 | Tweaking prompts to fix a bug causes regressions elsewhere.31 | Running full multi-turn evaluation suites in CI/CD.11 |
| **Retrieval Changes** | Index updates 12 | Adding new documents alters chunk rankings.12 | Running semantic test queries to verify results.30 |
| **Embedding Changes** | Model migrations 12 | Upgrading embedding models changes vector coordinates.12 | Re-indexing the entire database vector collection.12 |
| **Schema Changes** | Database updates 12 | Changing database structures breaks old model parsers.12 | Verifying compatible schemas in the release manifest.12 |
| **Sampling Changes** | Decoder adjustments 25 | Changing temperature settings flattens logit distributions.25 | Locking decoding parameters in production configs.25 |
| **Routing Changes** | Model routing updates 30 | Dynamic routers send requests to low-cost fallback models.30 | Testing model accuracy thresholds before shifting traffic.30 |
| **Cache Changes** | Cache invalidations 30 | Database updates bypass system caching layers.30 | Logging cache matches to track consistency.30 |
| **SDK Updates** | Client library shifts 12 | SDK modifications alter serialization parameters.12 | Locking client library versions in production.12 |
| **Streaming Changes** | Chunk boundary shifts 12 | Network jitter alters chunk streaming boundaries.12 | Buffering text sequences before parsing payloads.10 |

## **Demo-to-Production Gap**

Curated, short-context development environments often overestimate system reliability because they operate under controlled assumptions. Moving systems to production exposes long-tail inputs, asynchronous dependencies, permission boundaries, cost limits, hostile content, stale data, and nondeterministic model behavior.

A demo asks whether the happy path works. A production readiness review asks whether failures are bounded, observable, replayable, and recoverable.

### **Table 13: Demo-to-Production Pathology Checklist**

| Check Item | Telemetry Domain | Operational Check Requirement | Production Readiness Target |
| :---- | :---- | :---- | :---- |
| **1. Rare Inputs** | User prompt patterns. | Verify routers handle unmapped, malformed, ambiguous, and raw inputs. | Low unresolved-routing rate; managed refusal on unknown intent. |
| **2. Malformed Data** | File ingestion. | Check parsers handle skewed scans, bad encodings, corrupt files, and hostile content. | No parser crashes; unsafe files quarantined. |
| **3. Bad Files** | Ingestion pipeline. | Verify truncated, encrypted, binary, and oversized uploads fail safely. | Managed failure object and trace. |
| **4. Long Contexts** | Context management. | Test instruction retention and answer quality at realistic context utilization. | Instruction invariants remain intact under stress. |
| **5. Ambiguous Inputs** | Input validation. | Verify missing entities trigger clarification without looping. | Bounded clarification and escalation path. |
| **6. Contradictory Sources** | Retrieval and grounding. | Detect and surface conflicts across sources rather than flattening them. | Conflicts flagged for high-impact answers. |
| **7. Empty Retrieval** | Search/retrieval. | Verify empty result sets produce uncertainty/refusal, not invented answers. | No confident answer on empty evidence. |
| **8. Low-Quality OCR** | Multimodal preprocessing. | Test poor scans, low contrast, small text, and rotated pages. | Evidence adequacy gate blocks unsupported extraction. |
| **9. Noisy Speech** | Voice interfaces. | Test background speech, coughs, clicks, accents, stutters, and barge-in. | Endpointing and repair stay within profile-specific SLOs. |
| **10. UI Drift** | Browser/UI agents. | Verify unexpected elements, modals, layout shifts, and stale selectors trigger re-observe/recover. | Bounded recovery; no blind clicking. |
| **11. Tool Timeouts** | External APIs. | Verify hangs, timeouts, and partial responses produce known states. | Unknown/pending states are preserved and reconciled before retry. |
| **12. Network Failures** | Transport and gateway. | Test backoff, jitter, queueing, shedding, and managed refusal. | No cascading retry storms. |
| **13. Rate Limits** | Provider gateway. | Verify rate-limit responses are handled through queueing, backoff, shedding, or managed refusal. | No unbounded retry; user receives clear status. |
| **14. Provider Failovers** | Model/API routing. | Check backup routes preserve capability, compliance, and quality thresholds. | Fallback route is policy-approved, not merely cheaper. |
| **15. Simultaneous Users** | Concurrency and serving. | Test queues, worker pools, memory pressure, and isolation under peak load. | Degrade or shed load before saturation collapse. |
| **16. Cost Ceilings** | Spend controls. | Verify gateway blocks or degrades calls when run/session budgets are breached. | No unbounded spend; managed budget-exceeded state. |
| **17. Partial Failures** | Workflow state. | Ensure mid-run failures preserve partial, pending, compensated, or unknown states. | No orphaned high-impact transaction states. |
| **18. User Corrections** | Session memory and dialogue. | Verify corrections update active state and do not preserve stale assumptions. | Bounded repair with visible confirmation when needed. |
| **19. Schema Migration** | Schema registry. | Verify database/API/schema migrations do not break old prompts, validators, or consumers. | Compatibility tests pass before rollout. |
| **20. Stale Caches** | Memory/cache layers. | Check cache invalidation after database/source changes. | Stale data cannot authorize actions. |
| **21. Prompt Injection** | Trust boundary. | Test injection payloads inside documents, pages, tool outputs, OCR text, and user content. | Injection cannot grant tool authority or override policy. |
| **22. Streaming Truncation** | Streaming parsers. | Check incomplete objects, token limits, and dropped chunks. | No parser crash; incomplete status is preserved. |
| **23. Human Escalation** | Operations. | Verify escalation packets include context, evidence, failure reason, and safe next options. | Human handoff is actionable and auditable. |
| **24. Replayability** | Audit/debug. | Verify golden traces can be replayed with mocked tools and frozen inputs. | Divergence points are explicit and diagnosable. |

### **Production Readiness Rule**

```text
A system is production-ready only when common failures are boring:
bounded, typed, visible, replayable, and recoverable.
```

## **Reliability Operations: Detection, Containment, Replay, and Recovery**

When production pathologies occur, systems must automatically detect the failure, contain impact, preserve replay evidence, and recover without corrupting state. The correct recovery path depends on failure class, side-effect class, transaction boundary, and confidence in authoritative state.

### **Table 14: Detection-Control-Recovery Matrix**

| Failure Class | Primary Detection Signal | Automatic Containment | Recovery Path | Human Escalation Trigger | Logging Requirement | Regression Test |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Malformed Output** | Parser or schema validator rejects payload. | Block downstream delivery; preserve raw payload and validation errors. | Bounded repair using structured error feedback; fail closed after limit. | Repair attempts exceed threshold or payload is high-impact. | Raw output, schema version, validator errors, model route. | Replay trace through parser and repair harness. |
| **Semantic Invalidity** | Business/domain validator rejects schema-valid payload. | Block execution and return typed semantic error. | Ask clarification, replan, or route to review. | High-impact action or repeated semantic mismatch. | Payload, rule ID, validator version, source state. | Domain-rule regression suite. |
| **Citation Failure** | Citation support, quote fidelity, or source existence check fails. | Suppress unsupported claim or citation. | Re-run retrieval, surface uncertainty/conflict, or refuse. | Support score below policy threshold on high-impact answer. | Retrieved docs, citation coordinates, claim-support scores. | Citation-support evaluation. |
| **Grounding Failure** | Claim unsupported by retrieved or inspected evidence. | Block confident answer. | Retrieve additional evidence, degrade confidence, or ask user. | Evidence unavailable for regulated/high-impact claim. | Evidence packets, adequacy state, claim IDs. | Grounding and evidence-sufficiency tests. |
| **Tool Loop** | State repetition count or tool-call budget exceeded. | Stop tool dispatch; freeze run state. | Return managed failure, replan from verified state, or escalate. | Repeated identical parameters, high-cost tool, or mutating action. | Tool schemas, payload hashes, observations, idempotency keys. | Loop-termination replay test. |
| **Unknown Tool State** | Timeout, lost response, or ambiguous observation after mutating call. | Block blind retry; hold workflow. | Reconcile via idempotency record/source-of-record; compensate or escalate if needed. | High-impact mutation or reconciliation timeout. | Execution attempt, idempotency key, request hash, trace ID. | Unknown-state retry prevention test. |
| **Instruction Loss** | Invariant or policy check detects system directive drift. | Reject output or stop autonomous step. | Rebuild context, prune noise, restore canonical prompt template. | Safety/policy invariant breach. | Full prompt context, summary history, template versions. | Long-context instruction stress test. |
| **Contradiction** | Semantic consistency check flags conflict. | Suppress or mark conflicting output. | Regenerate with conflict packet, ask user, or escalate based on risk. | Safety, legal, financial, medical, or operational conflict. | Conflicting claims, sources, tool results, memory state. | Contradiction-resolution suite. |
| **Runaway Cost** | Spend, token, tool-call, or wall-clock budget threshold reached. | Gateway blocks further model/tool calls. | Degrade, summarize state, ask user, or return managed budget failure. | Budget breach on critical workflow or repeated abuse pattern. | Turn count, token usage, tool calls, cost ledger. | Budget-enforcement test. |
| **Stochastic Drift** | Canary or replay distribution shifts beyond threshold. | Freeze rollout or isolate traffic slice. | Roll back model/prompt/retrieval/schema route; investigate drift. | User-visible incident or high-risk metric degradation. | Model fingerprint, route, prompts, traces, evaluation deltas. | Multi-run regression and canary replay. |
| **Prompt Injection** | Untrusted content attempts to override system policy or trigger tools. | Isolate content as data; block unauthorized tool path. | Sanitize, strip hidden content, ask user, or quarantine source. | Credential/tool exfiltration attempt or repeated hostile source. | Source region, payload text, blocked tool, policy decision. | Injection corpus regression test. |
| **Partial Commit** | Some sub-actions commit while others fail. | Stop dependent actions and preserve partial state. | Compensate before pivot, forward recover after pivot, or escalate. | Compensation fails or state is high-impact. | Sub-action statuses, transaction IDs, compensation attempts. | Saga/partial-failure drill. |
| **False Success Escape** | User-facing success conflicts with verified state. | Correct status, block downstream dependence, incident if severe. | Reconcile authoritative state and issue corrected user/system status. | External user impact or high-impact side effect. | Spoken/text claim, verification state, source-of-record result. | False-success guard test. |

### **Recovery Rule**

```text
Retry only when safe.
Repair only when bounded.
Rollback only inside a live transaction boundary.
Compensate when committed state must be reversed.
Forward recover after pivot when completion is safer than reversal.
Hold and escalate when state is unknown or high-impact.
```

## **Production Pathology Observability and Evaluation Guidance**

Evaluating stateful, probabilistic systems requires tracking explicit runtime metrics across intermediate transformations, not just final responses. Production observability should distinguish formatting failures, semantic failures, grounding failures, tool failures, loop behavior, cost pressure, drift, replayability, and user-visible incidents.

Absolute targets such as “0.00%” are appropriate only for **fail-closed invariant breaches** such as successful sandbox escape or unauthorized irreversible execution. For ordinary stochastic quality metrics, thresholds should be treated as operational SLOs tuned by risk class.

### **Table 15: Observability Metrics**

| Metric Category | Technical Metric Identifier | Diagnostic Target | Target / Interpretation |
| :---- | :---- | :---- | :---- |
| **Formatting** | Malformed Output Rate | Parser exceptions divided by total structured responses. | Low SLO; alerts on regression. |
| **Formatting** | Schema Rejection Rate | Schema failures divided by total evaluations. | Near-zero under constrained decoding; nonzero triggers investigation. |
| **Formatting** | Repair Attempt Rate | Sessions needing format correction. | Low; spikes indicate prompt/schema/model drift. |
| **Formatting** | Repair Success Rate | Successful repairs divided by repair attempts. | High, but repair limit must remain bounded. |
| **Formatting** | Repair Drift Rate | Repairs that alter task, policy, or evidence unexpectedly. | Fail-closed invariant for high-impact outputs. |
| **Evidence** | Invalid Citation Rate | Unverifiable citations divided by total citations. | Low; high-impact claims require stricter gate. |
| **Evidence** | Citation Support Rate | Verified citations divided by total cited claims. | High; unsupported claims suppressed. |
| **Evidence** | Unsupported Claim Rate | Generated claims lacking adequate evidence. | Risk-class dependent; must be near zero for regulated/high-impact answers. |
| **Evidence** | Evidence Adequacy Failure Rate | Selected evidence is relevant but insufficient. | Useful signal for retrieval/crop/table/chart failures. |
| **Agentic** | Hallucinated Tool Claim Rate | Claimed tool executions missing from traces. | Fail-closed invariant. |
| **Agentic** | Tool Payload Rejection Rate | Tool arguments rejected by schema or policy. | Low; spikes indicate contract drift. |
| **Agentic** | Tool Loop Rate | Runs exceeding repetition or tool-call limits. | Should terminate automatically; alert on repeated patterns. |
| **Agentic** | Unknown Tool State Rate | Mutating tool attempts with unresolved outcome. | Low; high-impact unknown states require reconciliation/escalation. |
| **Execution** | Instruction Retention Score | Evaluations passing instruction-invariant tests. | High; track by context length and task type. |
| **Execution** | Harness Drift Rate | Prompt/eval templates modified outside manifest. | Fail-closed invariant for release governance. |
| **Execution** | Contradiction Rate | Responses with semantic conflicts. | Low; severity weighted. |
| **Execution** | Cross-Run Semantic Variance | Semantic variance across identical inputs under pinned route. | Bounded; distinguish phrasing variance from decision variance. |
| **Grounding** | Grounding Failure Rate | Responses failing source-support or evidence checks. | Low; gated by risk class. |
| **Operations** | Action Success Hallucination Rate | Completion claims mismatching authoritative state. | Fail-closed invariant. |
| **Operations** | Runaway Loop Termination Rate | Loops terminated by gateway or orchestrator budgets. | High; failures indicate budget-control defect. |
| **Operations** | Cost Overrun Rate | Spend exceeding budget policy. | Fail-closed invariant after gateway controls. |
| **Operations** | Timeout-to-Success Misclassification | Hanging/pending tools reported as successful. | Fail-closed invariant. |
| **Operations** | Non-Deterministic Regression Delta | Variance between canary and baseline distributions. | Alert when above release threshold. |
| **Operations** | Replay Divergence Rate | Replays deviating from recorded traces under unchanged code and mocked dependencies. | Low; divergence must identify first mismatching step. |
| **Operations** | Human Escalation Rate | Tasks routed to human operators. | Context dependent; spikes indicate automation degradation. |
| **Operations** | User-Visible Incident Rate | Uncontained failures reaching users. | Fail-closed invariant goal; investigate all verified incidents. |
| **Security** | Prompt Injection Block Rate | Injection attempts blocked before tool authority. | Nonzero blocks may indicate controls working; spikes require source review. |
| **Security** | Unauthorized Tool Execution Rate | Tools executed without valid authority/approval. | Fail-closed invariant. |
| **Security** | Data Exfiltration Attempt Rate | Blocked attempts to expose restricted data. | Track and investigate; successful exfiltration is incident. |

### **Observability Rule**

```text
Measure every boundary:
input, retrieval, context, generation, validation, repair,
tool dispatch, observation, verification, state update, citation, and output.
```

A final-answer metric alone cannot diagnose where the system failed. It only tells you where the smoke finally escaped.

## **Cross-Canon Handoff Map**

The AI-ENG-S report establishes the foundational failure atlas that adjacent canon reports leverage to implement safety mitigations and runtime defenses.12

### **Table 16: Cross-Canon Handoff Map**

| Target Canon Report | Structural Handoff Target | Critical Handoff Parameters | Architectural Enforcement Rule |
| :---- | :---- | :---- | :---- |
| **AI-ENG-T** | Hostile Injection Defense 5 | Safety parameters, intent records, and injection logs.5 | Sanitizes input prompts and isolates untrusted data inside XML wrappers.5 |
| **AI-ENG-U** | Dependency & Tool Isolation 5 | Tool parameters, database permissions, and API schemas.5 | Hardens tool execution environments and restricts database permissions.5 |
| **AI-ENG-V** | Resource & Spend Control 5 | Token limits, cost reservation logs, and session limits.1 | Enforces run-level currency budgets and limits API execution rates.1 |
| **AI-ENG-W** | Fallback & Degraded Operations 12 | Rejection statuses, fallback configurations, and limits.1 | Routes traffic to low-cost fallback models when primary APIs error.30 |
| **AI-ENG-X** | User Trust & Evidence Highlights 12 | Citation coordinate arrays, bounding boxes.12 | Highlights verified citations and source regions in the user interface.12 |
| **AI-ENG-Y** | Human Review Escalation 12 | Escalation records, context histories, and confidence scores.12 | Suspends automated execution and transfers sessions to human queues.12 |
| **AI-ENG-Z** | System Telemetry 12 | OpenTelemetry schemas, metrics, and trace formats.11 | Formats and exports trace records, latency logs, and error states.11 |
| **AI-ENG-AA** | Reliability Evaluations 12 | Golden datasets, assessment rubrics, test results.11 | Runs multi-turn regression suites and benchmarks system performance.11 |
| **AI-ENG-AB** | Audit & Replay Debugging 12 | Trace files, hashes, CAS configurations.11 | Stores signed session logs and provides replay debugging tools.11 |
| **AI-ENG-AC** | Incident Isolation 12 | System alerts, isolation records, audit trails.1 | Quarantines compromised environments and revokes active API credentials.12 |
| **AI-ENG-AJ** | Reference Blueprint 12 | Deployment templates, firewall rules, routing plans.12 | Implements remote sandbox environments and proxy configurations.12 |

## **Durable Principles of Production AI Reliability**

### **I. Constrain Decoding at the Inference Layer**

Relying on prompts to enforce formatting constraints is an anti-pattern. When a downstream system expects a strict structure, syntactic compliance should be enforced with constrained decoding, schema-aware generation, or strict post-generation validation. Formatting failures should not reach business logic.

### **II. Separate Formatting Compliance from Semantic Verification**

Syntactic correctness does not guarantee semantic correctness. Valid JSON can contain fabricated facts, impossible dates, unauthorized actions, stale IDs, or invalid business logic. Production systems must validate syntax, schema, semantics, policy, grounding, and downstream usability separately.

### **III. Treat Every Boundary Crossing as a Contract**

Model outputs, retrieved evidence, citations, tool arguments, tool observations, UI actions, voice transcripts, and final responses are all boundary-crossing artifacts. Each must carry enough structure for validation, authorization, tracing, and recovery.

### **IV. Enforce Run-Level Financial and Execution Budgets**

Stateful agents can enter recursive loops and denial-of-wallet dynamics. Execution paths must run within explicit token, time, tool-call, concurrency, and currency budgets enforced by the gateway or orchestrator, not merely requested in the prompt.

### **V. Verify Citations Before Trusting Claims**

A citation is not valid because it exists. It is valid only if it points to an existing, fresh, authorized source that supports the attached claim. Unsupported claims must be revised, suppressed, or routed to review.

### **VI. Preserve Unknown State Instead of Guessing**

Timeouts, partial commits, and ambiguous observations must not be flattened into success or failure. Unknown is a first-class state that blocks unsafe retries and requires reconciliation, compensation, or escalation.

### **VII. Use Idempotency for Mutating Boundaries**

Retries are inevitable. Duplicate side effects are optional negligence. Any state-changing boundary should use idempotency keys, request hashes, replay protection, and durable execution records proportional to risk.

### **VIII. Separate Untrusted Data from Authority**

Retrieved documents, webpage text, OCR output, tool responses, user-generated content, and emails are evidence, not instructions. They cannot grant permissions, override policy, or authorize tools.

### **IX. Bound Repair, Retry, Reflection, and Fallback Loops**

Repair is useful only when it terminates. Retry is safe only when idempotent or reconciled. Reflection is useful only with a stop condition. Fallback is safe only when the substitute route preserves capability, compliance, and quality requirements.

### **X. Build for Complete Observability and Replayability**

Debugging nondeterministic, multi-turn agents requires complete traces: prompts, model versions, tool calls, observations, validation results, state changes, budget counters, evidence packets, and policy decisions. If the system cannot replay a failure, it cannot reliably fix it.

### **XI. Fail Closed on Authority, Safety, and High-Impact State**

When authorization, identity, approval, policy, source authority, or high-impact action state cannot be verified, the system should fail closed, degrade safely, or escalate. It should not continue optimistically.

### **XII. Reliability Is a Release Property, Not a Vibe**

Production readiness is established through canaries, regression suites, failure injection, trace replay, schema compatibility checks, and operational SLOs. A working demo is not evidence of production reliability. It is a charming little hallucination with a UI.

#### **Works cited**

1. Runaway Agents, Tool Loops, and Budget Overruns — Cycles, accessed June 10, 2026, [https://runcycles.io/incidents/runaway-agents-tool-loops-and-budget-overruns-the-incidents-cycles-is-designed-to-prevent](https://runcycles.io/incidents/runaway-agents-tool-loops-and-budget-overruns-the-incidents-cycles-is-designed-to-prevent)  
2. How are you preventing runaway AI agent behavior in production? : r/LangChain - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/LangChain/comments/1rhwz53/how_are_you_preventing_runaway_ai_agent_behavior/](https://www.reddit.com/r/LangChain/comments/1rhwz53/how_are_you_preventing_runaway_ai_agent_behavior/)  
3. Your LLM JSON Is Valid — And Still Wrong | by Rost Glukhov | May ..., accessed June 10, 2026, [https://medium.com/@rosgluk/your-llm-json-is-valid-and-still-wrong-12dbbecf1fdc](https://medium.com/@rosgluk/your-llm-json-is-valid-and-still-wrong-12dbbecf1fdc)  
4. Beyond JSON Mode: Getting Reliable Structured Outputs from LLMs ..., accessed June 10, 2026, [https://tianpan.co/blog/2025-10-29-structured-outputs-llm-production](https://tianpan.co/blog/2025-10-29-structured-outputs-llm-production)  
5. LLM06:2025 Excessive Agency - OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm062025-excessive-agency/](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)  
6. LLM05:2025 Improper Output Handling - OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/)  
7. LLM Context Window Management and Long-Context Strategies 2026 | Zylos Research, accessed June 10, 2026, [https://zylos.ai/research/2026-01-19-llm-context-management/](https://zylos.ai/research/2026-01-19-llm-context-management/)  
8. Lost-in-the-Middle Is Still Real in 2026 (Even on 1M-Token Models) - DEV Community, accessed June 10, 2026, [https://dev.to/gabrielanhaia/lost-in-the-middle-is-still-real-in-2026-even-on-1m-token-models-2ehj](https://dev.to/gabrielanhaia/lost-in-the-middle-is-still-real-in-2026-even-on-1m-token-models-2ehj)  
9. [KNOWLEDGE] - AI Engineering - Volume 2. D-F Knowledge, Data, and Corpus Engineering.md  
10. Has anyone actually solved runaway agent costs? Looking for patterns beyond logging - API, accessed June 10, 2026, [https://community.openai.com/t/has-anyone-actually-solved-runaway-agent-costs-looking-for-patterns-beyond-logging/1383094](https://community.openai.com/t/has-anyone-actually-solved-runaway-agent-costs-looking-for-patterns-beyond-logging/1383094)  
11. Deterministic Replay: How to Debug AI Agents That Never Run the ..., accessed June 10, 2026, [https://tianpan.co/blog/2026-04-12-deterministic-replay-debugging-non-deterministic-ai-agents](https://tianpan.co/blog/2026-04-12-deterministic-replay-debugging-non-deterministic-ai-agents)  
12. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
13. Taxonomy of Hallucinations in AI - Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/taxonomy-of-hallucinations](https://www.emergentmind.com/topics/taxonomy-of-hallucinations)  
14. Survey and analysis of hallucinations in large language models: attribution to prompting strategies or model behavior - PMC, accessed June 10, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12518350/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12518350/)  
15. I tested structured output from 288 LLM calls and logged every way JSON breaks. Here's what I found : r/Python - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/Python/comments/1tagc2g/i_tested_structured_output_from_288_llm_calls_and/](https://www.reddit.com/r/Python/comments/1tagc2g/i_tested_structured_output_from_288_llm_calls_and/)  
16. Structured Output Isn't Reliable Output - Rotascale, accessed June 10, 2026, [https://rotascale.com/blog/structured-output-isnt-reliable-output/](https://rotascale.com/blog/structured-output-isnt-reliable-output/)  
17. Measuring output stability across LLM runs (JSON drift problem) : r/LocalLLaMA - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/LocalLLaMA/comments/1qwgu6x/measuring_output_stability_across_llm_runs_json/](https://www.reddit.com/r/LocalLLaMA/comments/1qwgu6x/measuring_output_stability_across_llm_runs_json/)  
18. Citation Grounding: Detecting and Reducing LLM Citation Hallucinations via Legal Citation Graphs - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2606.00898v1](https://arxiv.org/html/2606.00898v1)  
19. CiteGuard: Faithful Citation Attribution for LLMs via Retrieval-Augmented Validation - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2510.17853v1](https://arxiv.org/html/2510.17853v1)  
20. A Comparative Analysis of Faithfulness Metrics and Humans in Citation Evaluation - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2408.12398v1](https://arxiv.org/html/2408.12398v1)  
21. How do I reduce hallucinations when using search-grounded LLM responses? - Firecrawl, accessed June 10, 2026, [https://www.firecrawl.dev/glossary/web-search-apis/reduce-hallucinations-search-grounded-llm-responses](https://www.firecrawl.dev/glossary/web-search-apis/reduce-hallucinations-search-grounded-llm-responses)  
22. Context Rot: Why AI Gets Worse the Longer You Chat (And How to Fix It) - Product Talk, accessed June 10, 2026, [https://www.producttalk.org/context-rot/](https://www.producttalk.org/context-rot/)  
23. Intelligence Degradation in Long-Context LLMs: Critical Threshold Determination via Natural Length Distribution Analysis - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2601.15300v1](https://arxiv.org/html/2601.15300v1)  
24. AI Agent Loop Token Costs: How to Constrain Context - Augment Code, accessed June 10, 2026, [https://www.augmentcode.com/guides/ai-agent-loop-token-cost-context-constraints](https://www.augmentcode.com/guides/ai-agent-loop-token-cost-context-constraints)  
25. Non-Deterministic LLM Prompts in 2026: Practical Guide - Future AGI, accessed June 10, 2026, [https://futureagi.com/blog/non-deterministic-llm-prompts-2025/](https://futureagi.com/blog/non-deterministic-llm-prompts-2025/)  
26. You Can't Assert Your Way Out of Non-Determinism: A Practical QA Strategy for LLM Applications | by Venkat Peri - Medium, accessed June 10, 2026, [https://medium.com/@venkatperi/you-cant-assert-your-way-out-of-non-determinism-a-practical-qa-strategy-for-llm-applications-fd32e617cdec](https://medium.com/@venkatperi/you-cant-assert-your-way-out-of-non-determinism-a-practical-qa-strategy-for-llm-applications-fd32e617cdec)  
27. OWASP Top 10 LLM, Updated 2025: Examples & Mitigation Strategies - Oligo Security, accessed June 10, 2026, [https://www.oligo.security/academy/owasp-top-10-llm-updated-2025-examples-and-mitigation-strategies](https://www.oligo.security/academy/owasp-top-10-llm-updated-2025-examples-and-mitigation-strategies)  
28. LLM Hallucination Examples - Arize AI, accessed June 10, 2026, [https://arize.com/llm-hallucination-examples/](https://arize.com/llm-hallucination-examples/)  
29. CiteCheck: Towards Accurate Citation Faithfulness Detection - arXiv, accessed June 10, 2026, [https://arxiv.org/html/2502.10881v1](https://arxiv.org/html/2502.10881v1)  
30. How to Prevent Runaway Agent Costs with MLflow AI Gateway, accessed June 10, 2026, [https://mlflow.org/blog/agent-costs-mlflow-gateway/](https://mlflow.org/blog/agent-costs-mlflow-gateway/)  
31. Monitoring LLM behavior: Drift, retries, and refusal patterns - VentureBeat, accessed June 10, 2026, [https://venturebeat.com/infrastructure/monitoring-llm-behavior-drift-retries-and-refusal-patterns](https://venturebeat.com/infrastructure/monitoring-llm-behavior-drift-retries-and-refusal-patterns)  
32. Recursive Language Models: Could This Be the Real Fix for Long-Context AI in 2026? | by Vinod Polinati | Medium, accessed June 10, 2026, [https://medium.com/@vinodpolinati/recursive-language-models-could-this-be-the-real-fix-for-long-context-ai-in-2026-070328df9329](https://medium.com/@vinodpolinati/recursive-language-models-could-this-be-the-real-fix-for-long-context-ai-in-2026-070328df9329)  
33. The Essential Guide to the NIST AI Risk Management Framework 1.0 - Vistrada, accessed June 10, 2026, [https://vistrada.com/resources/insights/nist-ai-risk-management-framework-1-0](https://vistrada.com/resources/insights/nist-ai-risk-management-framework-1-0)

---

[← Back to Canon Map](../canon-map.md)