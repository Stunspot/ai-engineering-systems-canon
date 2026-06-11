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
| **Production Pathology** | A recurring, systemic failure mode in deployed AI systems arising from the intersection of probabilistic generation with deterministic constraints.1 | Systemic Incident Rate (R\_inc) | 0.00% of automated transaction pipelines |
| **Confabulation** | The generation of linguistically fluent but factually unsupported, inaccurate, or internally contradictory output.13 | Factual Grounding Ratio (G\_fact) | \> 99.5% of generated statements |
| **Malformed Output** | Output that violates the syntactical, structural, or lexical expectations of the downstream parser or schema compiler.3 | Parser Exception Rate (R\_parse) | \< 0.10% of API responses |
| **Schema Validity** | The state where a generated structural payload conforms exactly to a defined schema's keys, types, array bounds, and nullability constraints.3 | Schema Rejection Rate (R\_reject) | 0.00% under native constrained decoding |
| **Semantic Validity** | The state where a syntactically and structurally valid payload contains values that align with real-world business rules and logic.3 | Semantic Violation Rate (R\_sem\_violation) | \< 0.50% of evaluated transactions |
| **Contract Drift** | The gradual divergence between a model's semantic interpretation of a schema and the downstream system's execution expectations.16 | Value Distribution Drift (Delta\_drift) | PSI (Population Stability Index) \< 0.10 |
| **Citation Integrity** | The exact, verifiable alignment between a generated claim and the specific region, page, or cell coordinates of the retrieved source.18 | Citation Alignment Score (A\_cite) | \> 98.0% verification precision |
| **Grounding Failure** | An execution where a model answers from pre-trained prior weights while appearing to use retrieved context.21 | Prior-Weight Influence Delta (Delta\_prior) | Recall-to-prior ratio \< 0.05 |
| **Instruction Loss** | The degradation of system directives as they pass through long contexts, summaries, or tool-output streams.7 | Instruction Adherence Index (I\_adh) | \> 99.0% on adversarial distractors |
| **Harness Drift** | The degradation of evaluation or safety wrapper templates during recursive state reconstruction or repair loops.12 | Template Consistency Score (C\_temp) | Zero unmapped system modifications |
| **Brittle Chain** | A multi-step sequence where minor, unobserved state corruptions compound to cause catastrophic failure at the final step.1 | Cascade Probability (P\_cascade) | Joint F1 score drop \< 1.0% per step |
| **Tool Loop** | A state where an agent repeatedly calls tools with identical or slightly altered parameters without making progress.1 | State Repetition Count (C\_rep) | Limit of \<= 2 identical repeated states |
| **Repair Loop** | A multi-turn pattern where a system attempts to fix a schema violation but continues to emit malformed payloads.1 | Repair Intertwining Index (I\_repair) | Complete termination after \<= 2 attempts |
| **Runaway Behavior** | Unbounded model execution that bypasses rate limits, loops continuously, or consumes compute credits without completing.1 | Budget Depletion Velocity (V\_burn) | Auto-halt on 100% of budget breaches |
| **Non-Deterministic Regression** | A drop in accuracy, formatting, or instruction adherence caused by unannounced model updates or system changes.17 | Regression Variance Delta (sigma^2\_reg) | Accuracy variance standard deviation \< 0.02 |
| **Golden Trace** | A recorded, high-fidelity log of a successful production run containing all prompts, tool calls, and states.11 | Trace Match Fidelity (F\_trace) | 100% structural equivalent match |
| **Stochastic Replay** | The technique of debugging non-deterministic agent executions by playing back recorded events and mocking external states.11 | Replay Alignment Precision | 100% reproducibility of logic paths |
| **Replay Divergence** | The point where a replayed run deviates from the recorded path due to unintercepted time calls or system changes.11 | Divergence Step Offset (S\_div) | 0.00% divergence on unchanged code |

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

AI systems fail when outputs cross boundaries in forms that downstream components cannot safely interpret.3 This map illustrates the physical flow of data and the specific boundaries where failures enter and propagate.

### **Artifact 3: Failure Boundary Map**

   
       │   
       ▼ (Boundary 1: Sanitization & Safety Gate)   
\[ Intent Parser \] ─── (Failure: Prompt Injection, Jailbreaks)   
       │   
       ▼ (Boundary 2: Knowledge & Query Routing)   
 ─── (Failure: Stale/Poisoned Corpus, Empty Results)   
       │   
       ▼ (Boundary 3: Context Assembly & Token Limits)   
\[ Context Assembly \] ─── (Failure: Lost in the Middle, Truncation) \[7, 8, 23\]  
       │   
       ▼ (Boundary 4: LLM Generation / Serving Engine)   
\[ Generator Model \] ─── (Failure: Floating-point Non-associativity, Drift)   
       │   
       ▼ (Boundary 5: Output Validation & Parser Layer)   
\[ Parser / Validator \] ─── (Failure: Syntax Exceptions, Type Invalidation)   
       │   
       ├───────────────┐   
       ▼ (Success)     ▼ (Failure: ValidationError)   
\[ Action Gate \]   ─── (Failure: Infinite Repair Loops)   
       │   
       ▼ (Boundary 6: Tool Execution Environment)   
 ─── (Failure: Excessive Agency, Side-Effects) \[1, 5, 6\]

To diagnose propagation, failures are classified into seven architectural categories:

* **Model-level errors** emit false, unsupported, contradictory, or off-policy content.  
* **Interface-level errors** violate schemas, parsing rules, citation formats, or downstream consumer expectations.  
* **Workflow-level errors** corrupt the shared state across a chain of transformations.  
* **Tool-level errors** invoke incorrect APIs, fabricate tool results, retry dangerously, or misread system observations.  
* **Grounding-level errors** cite wrong, stale, insufficient, or uninspected evidence.  
* **Runtime-level errors** alter execution via latency spikes, silent truncations, rate limits, or unannounced provider updates.  
* **Control-level errors** continue, repair, retry, or escalate transactions at incorrect times.

## **Hallucination as Operational Subtypes**

Labeling all incorrect model outputs as "hallucinations" is too broad for production troubleshooting.3 Useful incident diagnosis requires identifying the specific class of fabrication, isolating its entry point, and deploying targeted controls.1

### **Table 3: Hallucination Subtype Model**

| Subtype | Structural Presentation | Systemic Root Cause | Diagnostic Signal | Containment & Recovery |
| :---- | :---- | :---- | :---- | :---- |
| **Factual Hallucination** | Output states an incorrect fact without creating non-existent entities.13 | Model relies on weak semantic associations in pre-trained weights.14 | Entailment scores on knowledge graphs drop below threshold.13 | Force context retrieval, run BM25 cross-checks.12 |
| **Citation Hallucination** | Output cites non-existent, irrelevant, or outdated sources.18 | Citation markers are generated after text generation without grounding.12 | Source documents missing from index, or lack support.18 | Run citation-support classifiers, suppress ungrounded claims.18 |
| **Tool Hallucination** | Output claims a tool ran or invents fake results.2 | Tool-calling definitions overlap, causing model confusion.5 | Tool invocation name missing from system trace logs.2 | Enforce strict tool call validation, intercept payload before execution.2 |
| **State Hallucination** | Output claims system operations are complete when they are not.12 | Model predicts conversational completion tokens prematurely.12 | State mismatch between local variables and database.12 | Prevent the model from speaking completion until actions are verified.12 |
| **Schema Hallucination** | Output generates unmapped JSON keys or fields.4 | Generation temperature flatly distributes logits across wrong tokens.25 | Parser throws unexpected key or validation exception.3 | Compile schemas to Finite State Machines for constrained decoding.4 |
| **Policy Hallucination** | Output invents rules or guidelines to justify decisions.5 | System instructions lack clear hierarchies and boundaries.5 | Contrast with business rules flags out-of-bounds assertions.16 | Inject explicit policy wrappers, run model-based evaluations.16 |
| **Grounding Hallucination** | Answers from model prior while appearing grounded in retrieved documents.21 | Input context contains long distractor passages, diluting attention.7 | Low semantic overlap between generated text and source chunks.21 | Enforce strict source verification, adjust context positioning.8 |
| **Confidence Hallucination** | Output expresses high certainty on ungrounded claims.16 | Soft-max scoring maps peak probabilities to incorrect tokens.14 | Discrepancy between semantic entropy and stated certainty.14 | Calculate token-level entropy, append variance warning to UI.26 |

## **Malformed Output and Structural Failure**

Enforcing structured output is one of the most common challenges in production reliability.4 To prevent valid-but-wrong JSON from silently corrupting data downstream, architectures must implement a strict validation pipeline.3

### **Table 4: Layered Output Integrity Model**

| Integrity Layer | Verification Scope | Core Verification Mechanism | Failure Mode presentation |
| :---- | :---- | :---- | :---- |
| **1\. Syntax Validity** | Raw formatting compliance 4 | Parser compliance check (JSON, YAML, TOML) 4 | Unescaped quotes, trailing commas, truncated structures 15 |
| **2\. Schema Validity** | Structural contract compliance 3 | Pydantic model type-checking, extra="forbid" 3 | Missing required keys, type mismatches, invented fields 3 |
| **3\. Semantic Validity** | Multi-field logical consistency 3 | Python-based @model\_validator(mode="after") 3 | Mathematically impossible dates, inverted pricing 3 |
| **4\. Policy Validity** | Compliance and safety rules 5 | Policy evaluation engines, safety filter checks 5 | Generation of prohibited topics or banned content 5 |
| **5\. Grounding Validity** | Contextual evidence matching 21 | Cross-correlation against source coordinates 12 | Statements unaligned with retrieved context 21 |
| **6\. Downstream Usability** | Security and injection hardening 6 | Sanitization and parameterization checks 6 | Script injection, path traversal payloads 6 |
| **7\. User-Facing Clarity** | Conversational formatting clean-up 12 | Markdown fence stripping, layout normalization 15 | Raw JSON wrappers shown directly to end users 15 |

The physical implementation of this layered verification pipeline requires executing multi-stage assertions. The following Python program illustrates how to enforce strict schema boundaries, handle provider-side refusals, and validate complex cross-field business logic using Pydantic:

Python  
from decimal import Decimal  
from typing import Any, Dict, Optional  
from pydantic import BaseModel, Field, ValidationError, model\_validator

class ProductionContract(BaseModel):  
    """  
    Doctrinal schema contract enforcing strict object shapes.  
    Utilizes extra="forbid" to block the model from generating undeclared fields.  
    """  
    transaction\_id: str \= Field(pattern=r"^TXN-\[0-9\]{4}-\[0-9\]{4}$")  
    base\_amount: Decimal \= Field(gt=0)  
    discounted\_amount: Optional \= Field(default=None)  
    is\_promotional: bool  
    status: str \= Field(pattern=r"^(PENDING|COMMITTED|RECONCILED)$")

    class Config:  
        extra \= "forbid"

    @model\_validator(mode="after")  
    def verify\_financial\_logic(self) \-\> "ProductionContract":  
        """  
        Layer 3 (Semantic Validity) execution.  
        Enforces business rules that cannot be represented in a standard JSON schema.  
        """  
        if self.is\_promotional:  
            if self.discounted\_amount is None:  
                raise ValueError("discounted\_amount is required when is\_promotional is True.")  
            if self.discounted\_amount \>= self.base\_amount:  
                raise ValueError("discounted\_amount must be strictly less than base\_amount.")  
        else:  
            if self.discounted\_amount is not None:  
                raise ValueError("discounted\_amount must not be populated when is\_promotional is False.")  
        return self

def execute\_boundary\_ingestion(raw\_payload: str, api\_response\_metadata: Dict\[str, Any\]) \-\> ProductionContract:  
    """  
    Ingestion handler validating Layer 1 and Layer 2 transitions.  
    Intercepts incomplete generations and provider-level refusals.  
    """  
    \# Verify provider-level status flags before attempting parsing  
    if api\_response\_metadata.get("status") \== "incomplete":  
        raise RuntimeError(f"Generation truncated: {api\_response\_metadata.get('incomplete\_reason')}")  
    if api\_response\_metadata.get("type") \== "refusal":  
        raise RuntimeError(f"Provider refusal intercepted: {api\_response\_metadata.get('refusal\_message')}")  
          
    try:  
        \# Simultaneously parses and validates the structured model output  
        validated\_contract \= ProductionContract.model\_validate\_json(raw\_payload)  
        return validated\_contract  
    except ValidationError as exc:  
        \# Logs the structural exception and triggers the repair pipeline  
        raise ValueError(f"Contract violation detected: {exc.errors()}") from exc

## **Broken Schemas and Contract Drift Map**

Structured outputs are software interfaces, and like any interface, they regress.16 Contract drift occurs when provider updates shift how a model interprets fields, changing value distributions without breaking the raw schema.16

### **Table 5: Schema Failure and Contract Drift Map**

| Failure Point | Failure Origin | Failure Mechanism | Prevention & Control |
| :---- | :---- | :---- | :---- |
| **Prompt/Schema Mismatch** | System prompt templates | Textual examples show old field structures while the parser expects new variables. | Release manifests verifying prompts and validation schemas in sync. |
| **Old Schema Emission** | Model weights cache | Model emits retired properties after system updates, bypassing validators. | Pinning specific model snapshot versions (gpt-4o-2024-08-06). |
| **Enum Value Shift** | Probabilistic decoding | Model invents synonyms or unmapped categories inside string fields. | Enforcing native constrained decoding via Finite State Machines. |
| **Field Requirement Drift** | API Provider updates | Optional variables suddenly become required, blocking validation. | Automated regression testing of schema variants before deployment. |
| **Response-Shape Changes** | Model serialization | Model wraps objects in nested structures or arrays unexpectedly. | Schema registries with strict compatibility checks. |
| **SDK Serialization Drift** | Client-side libraries | Library updates modify how fields are serialized, breaking parsers. | Lockstep version pinning of all client-side dependencies. |
| **Strict Schema Rejections** | Parser compilation | Strict validation modes reject valid regex patterns or nested constraints. | Running pre-flight schema compilation checks in CI/CD. |
| **Validator Silent Drops** | Validation libraries | Parsers silently drop unrecognized fields, hiding system errors. | Configuring validators to reject extra properties (extra="forbid"). |

## **Tool-Use Failure Pathologies**

Tool-use failures are critical because outputs cross from text into system actions.5 An ungrounded tool call or a malformed payload can corrupt databases, trigger unauthorized actions, or result in costly resource leaks.1

### **Table 6: Tool-Use Failure Map**

| Pathology | Mechanical Trigger | Detection Signal | Prevention & Containment |
| :---- | :---- | :---- | :---- |
| **Phantom Tool Call** | Model outputs a tool-calling command without user intent.2 | Tool execution trace exists without matching prompt trigger.2 | Restricting tool execution to validated, explicit intent profiles.2 |
| **Wrong Tool Selection** | Model selects a tool with a similar name or purpose.2 | Execution of a different API route than planned.2 | designing unique, non-overlapping tool definitions and names.5 |
| **Unavailable Tool Reference** | Model attempts to invoke retired or restricted tools.2 | API gateway returns a 404 error.2 | Dynamic registry schema updates.2 |
| **Invalid Tool Payload** | Model outputs arguments that fail API type checks.2 | API validation returns a 400 error.2 | Running pre-execution Pydantic validation on all arguments.3 |
| **Stale Tool Observation** | Model relies on cached tool results instead of re-running queries.12 | Model reports outdated values, ignoring update logs.12 | Implementing strict cache invalidation policies.12 |
| **Ignored Tool Exception** | Model ignores API errors and summarizes the run as successful.2 | API error log followed by a successful complete token.2 | Catching and formatting exceptions as structured tool results.3 |
| **Fabricated Tool Result** | Model invents results instead of executing the API call.2 | API gateways report zero requests while the model outputs data.2 | Requiring execution hashes before state updates.12 |
| **Duplicate Tool Call** | Model calls a tool multiple times due to slow network responses.1 | Parallel API requests with identical body payloads.1 | Enforcing idempotency keys on all state-changing API calls.1 |
| **Non-Idempotent Retry** | Client retries failed state-changing API calls.1 | Multiple mutations on the same record.1 | Managing runs via an idempotency ledger.2 |
| **Tool Repair Loop** | Model repeatedly attempts to fix failed tool arguments.1 | High iteration counts with similar parameters.1 | Capping tool retries to a hard limit (\<= 2).2 |
| **Action-Success Hallucination** | Model claims a change occurred without verifying the tool output.12 | Success message emitted to user despite tool failure.12 | Enforcing coordinate checks against database records.12 |

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
Total Token Cost \= N \* S \+ u \* N \* (N \+ 1\) / 2 \+ r \* N \* (N \- 1\) / 2 24  
Where S represents the fixed system prompt tokens, u is the new input tokens per iteration (user message \+ tool result), and r is the generated output tokens per iteration.24 If an agent enters an infinite loop, this quadratic token accumulation can rapidly deplete budgets and trigger "denial-of-wallet" dynamics.1 Every repair loop is an agent loop; therefore, every agent loop must have a hard budget, stop condition, and escalation path.1

### **Table 9: Runaway Behavior Model**

| Loop Classification | Mechanical Trigger | Detection Signal | Containment Control | Escalation Path |
| :---- | :---- | :---- | :---- | :---- |
| **Recursive Tool Loop** | Model repeatedly retries failed tool calls.1 | State repetition count exceeds limit (C\_rep \> 2).2 | Hard limit on tool calls per run.1 | Suspend run, alert operator.2 |
| **Format Repair Loop** | Parser failures trigger constant re-generation.1 | High repair attempts with identical schemas.1 | Halt after \<= 2 repair attempts.3 | Fall back to default states.1 |
| **Retrieval Loop** | Search returns empty or conflicting results.1 | Identical search queries in sequential steps.2 | Max search limit of \<= 3 attempts.2 | Fall back to manual review.12 |
| **Planner Loop** | Agent continually updates plans without acting.1 | High token usage with zero tool execution.2 | Hard limit on planning turns.2 | Prompt user with options.2 |
| **Reflection Loop** | Model critiques and updates outputs endlessly.1 | Output similarity score exceeds 0.95 across turns.2 | Hard limit on reflection cycles.2 | Deliver the current output.2 |
| **Clarification Loop** | Agent repeatedly asks the user for the same inputs.1 | Conversational pattern repetition.12 | Hard limit on clarification requests.12 | Route session to support queues.12 |
| **Fallback Loop** | System gets stuck switching between fallback models.1 | Rapid model-switching cycles.30 | Hard limit on fallback routing.30 | Return a managed error.1 |
| **UI Click Loop** | UI agent clicks elements without page updates.12 | Bounding box coordinates remain unchanged.12 | Target element coordinate-matching checks.12 | Escalate to user, pause automation.12 |
| **Voice Repair Loop** | Voice agent loops on phonetic misunderstandings.12 | Repeated word correction patterns.12 | Phonetic spelling and digit-by-digit modes.12 | Transfer session to support queue.12 |
| **Multi-Agent Loop** | Collaborating agents cycle tasks endlessly.1 | Unchecked hands across agents.1 | Hard limit on handoff transfers.2 | Terminate session, alert admin.1 |
| **Retry Storm Loop** | Upstream servers retry failed requests at once.1 | Latency spikes combined with high traffic.30 | Exponential backoff with random jitter.30 | Trigger gateway rejection rules.30 |

## **Brittle Chains and Cascading Failures**

AI workflows often fail through quiet, multi-stage state corruptions.1 Because each individual stage appears locally valid, errors can propagate unobserved until they cause a catastrophic failure at the final step.1

### **Table 10: Brittle Chain Failure Model**

\[ Input \] ───\> \[ Preprocessing \] ───\> ───\> \[ Context Assembly \] ───\> \[ Generation \]   
                                                                                        │  
 \<─── \[ Observation \] \<─── \<─── \<─── \[ Validation \] \<───┘  
       │  
       ▼  
 ───\> \[ Citation \] ───\> \[ Logging \]

| Boundary Stage | Expected Contract | Possible Silent Corruption | Telemetry & Validator | Recovery Action |
| :---- | :---- | :---- | :---- | :---- |
| **1\. Input** | Clean raw data structure.12 | Unsupported file formats or rotated pages.12 | File check validators, DPI inspections.12 | Upscale file, run preprocessing.12 |
| **2\. Preprocessing** | Standardized text streams.12 | Collapsed layout structures or OCR drift.12 | Layout IoU match and CER tracking.12 | Fall back to local OCR libraries.12 |
| **3\. Retrieval** | High-relevance context.12 | Empty results or stale document matching.12 | Retrieval MRR and authority checks.9 | Run backup search queries.12 |
| **4\. Context** | Compact, grounded prompt.7 | Token overflow or lost system instructions.7 | Context window size monitoring.7 | Prune distractor chunks, rerank.8 |
| **5\. Generation** | Valid linguistic response.25 | Factual recall errors, confabulations.13 | Semantic and factual evaluations.26 | Re-generate using strict templates.12 |
| **6\. Validation** | Schema-conforming JSON.3 | Model invents keys, fields contain drift.3 | Pydantic model validation.3 | Trigger repair model routines.3 |
| **7\. Repair** | Valid corrected payload.1 | Model repeats mistakes, loops endlessly.1 | Repair counter metrics, timeouts.1 | Stop repairs, return default state.3 |
| **8\. Tool Call** | Valid, secure arguments.3 | Non-idempotent tool called multiple times.1 | Idempotency keys, gate validators.1 | Roll back database transactions.1 |
| **9\. Observation** | Verified execution result.12 | API returns errors, ignored by model.2 | Exception filters, gateway alerts.2 | Return formatted error responses.3 |
| **10\. State Update** | Reconciled system state.12 | Cache mismatched with database records.12 | DB status checks against logs.12 | Force state updates, clear cache.12 |
| **11\. Final Response** | Accurate, grounded text.21 | Claims ungrounded in source documents.21 | Grounding and support evaluations.21 | Suppress response, alert operator.12 |
| **12\. Citation** | Verifiable references.18 | Cited documents missing or irrelevant.18 | Quote checks, support metrics.12 | Remove ungrounded citations.12 |
| **13\. Logging** | Complete run record.11 | Traces fail to capture tool payloads.11 | Event logging validation checks.11 | Use recording proxies to capture.11 |

## **Contradictory Outputs and Consistency Failures**

A consistency failure occurs when a model generates individually plausible statements that contradict each other.13 These failures must be classified to identify acceptable uncertainty versus true production faults.13

### **Table 11: Contradiction and Consistency Framework**

| Contradiction Class | Systemic Origin | Mechanical Validation | Impact on Production | Acceptable vs Failure |
| :---- | :---- | :---- | :---- | :---- |
| **Within-Answer Contradiction** | Long context windows 7 | Cross-paragraph semantic entailment checks.13 | User receives conflicting, unusable advice.13 | **Failure:** Confusing statements must be suppressed.31 |
| **Reasoning-Final Contradiction** | Planning track mismatch 12 | Comparing thought variables against final JSON.12 | System executes tools that mismatch text plan.12 | **Failure:** Mismatched actions must be blocked.12 |
| **Source-Answer Contradiction** | Context prioritization 21 | NLI checks against source documents.13 | Model outputs claims denied by sources.13 | **Failure:** Ungrounded outputs must be rejected.31 |
| **Tool-Result Contradiction** | Ignored API exceptions 2 | Comparing tool outputs against generated text.12 | Model reports success when action failed.12 | **Failure:** Spoken status must match API returns.12 |
| **Memory-State Contradiction** | Cache sync delays 12 | Comparing session memory against database records.12 | Model bases tasks on outdated user data.12 | **Failure:** Stale data actions must be blocked.12 |
| **Cross-Run Contradiction** | Probabilistic decoding 17 | Multi-run semantic similarity clustering.25 | Inconsistent behavior on identical inputs.25 | **Acceptable:** Minor phrasing variance is tolerated.25 |
| **Streaming Contradiction** | Speculative tokens 10 | Comparing streaming chunks against finalized text.10 | Model corrects its statements mid-sentence.12 | **Acceptable:** Spoken self-correction is tolerated.12 |
| **Policy Contradiction** | Weak safety rules 5 | Evaluation of output against guidelines.16 | Output violates compliance parameters.5 | **Failure:** Safety breaches must be blocked.5 |
| **Citation Contradiction** | Decorative citations 12 | Comparing cited claims against source text.12 | Claims are unaligned with referenced files.18 | **Failure:** Citations must match source facts.29 |
| **Multimodal Contradiction** | Modality mismatch 12 | Comparing visual charts against text tables.12 | Explanations contradict raw data figures.12 | **Failure:** Text data must match visual images.12 |

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

Curated, short-context development environments often overestimate system reliability because they operate under controlled assumptions.1 Moving systems to production exposes them to a long tail of edge cases.1

### **Table 13: Demo-to-Production Pathology Checklist**

| Check Item | Telemetry Domain | Operational Check Requirement | Target Threshold |
| :---- | :---- | :---- | :---- |
| **1\. Rare Inputs** | User prompt patterns | Verifies intent routers handle unmapped and raw inputs.5 | \<= 0.50% routing failure rate |
| **2\. Malformed Data** | File ingestion systems | Checks parsers handle skewed scans, low contrast, and bad encodings.12 | \>= 99.0% text parsing accuracy |
| **3\. Bad Files** | Ingestion pipeline | Verifies systems isolate truncated and corrupted uploads.12 | Zero parser crashes on corruption |
| **4\. Long Contexts** | Token memory usage | Tests model accuracy when contexts fill past 50%.7 | \>= 95.0% instruction retention |
| **5\. Ambiguous Inputs** | Input validation | Checks models resolve unmapped parameters without stalling.12 | \<= 1.0% clarification timeout rate |
| **6\. Contradictory Sources** | RAG retrieval engines | Tests systems resolve conflicting claims across retrieved sources.12 | 100% detection of conflicts |
| **7\. Empty Retrieval** | Database indices | Verifies models deliver managed refusals on empty queries.3 | Zero answers on empty search sets |
| **8\. Low-Quality OCR** | Image preprocessors | Tests preprocessors upscale poor inputs to 300 DPI.12 | CER (Character Error Rate) \< 1.5% |
| **9\. Noisy Speech** | Voice interfaces | Tests voice systems handle street background noise and clicks.12 | \<= 1.5% false endpoint rate 12 |
| **10\. UI Drift** | Browser agents | Verifies UI agents handle unexpected elements and shifts.12 | \>= 98.0% step success rate 12 |
| **11\. Tool Timeouts** | External APIs | Verifies systems catch API hangs and route to fallbacks.2 | \<= 0.10% stalled automated steps |
| **12\. Network Failures** | Transport layer | Tests exponential backoff with jitter on packet loss.30 | Zero cascading network retries |
| **13\. Rate Limits** | Provider gateways | Verifies gateway proxies queue calls on provider limits.30 | HTTP 429 rate limit errors block |
| **14\. Provider Failovers** | API routing proxies | Checks traffic shifts to low-cost backup models on errors.30 | Failover latency penalty \< 150ms |
| **15\. Simultaneous Users** | Concurrency engines | Tests thread pools under peak transactional volumes.1 | Zero queue saturation crashes |
| **16\. Cost Ceilings** | Spend ledgers | Verifies gateways block calls when budget limits are breached.1 | 100% blocking of over-spend |
| **17\. Partial Failures** | State transformations | Checks systems preserve states if steps fail mid-run.1 | Zero orphaned transaction states |
| **18\. User Corrections** | Session memory | Verifies memory handles correction inputs during sessions.12 | \>= 92.0% repair success rate 12 |
| **19\. Schema Migration** | Schema registry | Verifies database migrations do not break old model parsers.12 | Zero unmapped field exceptions |
| **20\. Stale Caches** | Memory caching layers | Checks caching layers invalidate records when database updates.12 | Zero stale data transactions |
| **21\. Prompt Injection** | Input sanitization | Tests filters block injection payloads inside source documents.5 | 100% blocking of injection text |
| **22\. Streaming Truncation** | Streaming parsers | Checks systems handle incomplete streaming objects on limits.12 | Zero parser crashes on truncation |

## **Reliability Operations: Detection, Containment, Replay, and Recovery**

When production pathologies occur, systems must automatically detect the failure, contain the impact, record the trace for debugging, and recover cleanly without dropping sessions.1

### **Table 14: Detection-Control-Recovery Matrix**

| Failure Class | Primary Detection Signal | Automatic Containment | Recovery Path | Human Escalation Trigger | Logging Requirement | Regression Test |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Malformed Output** | Parser throws a validation exception.3 | Isolate the parser process, block downstream delivery.3 | Trigger a repair routine using structured error feedback.3 | Repair attempts exceed the safety threshold (\<= 2).3 | Record raw tokens, parsing parameters, and error output.11 | Replay the trace to verify correct parsing.11 |
| **Citation Failure** | Quote validation fails exact character matching.12 | Suppress statements citing missing documents.12 | Re-run queries, or return a managed refusal.3 | Citation support score drops below 0.85.29 | Log the retrieved documents and citation coordinates.11 | Run support evaluations against the database.31 |
| **Tool Loop** | State repetition count exceeds limit (C\_rep \> 2).2 | Terminate the active thread, close API connections.1 | Roll back transaction database commits.1 | Tool execution fails twice on identical parameters.2 | Log the tool schema, payloads, and returned exceptions.11 | Replay the trace to check loop termination.11 |
| **Instruction Loss** | Invariant check detects instruction drift.12 | Reject the generated output, block deployment.12 | Prune context tokens, rebuild prompt layouts.7 | Adherence score drops below threshold in production.12 | Record the complete prompt context and history.11 | Run instruction stress evaluations in CI/CD.31 |
| **Contradiction** | Semantic check flags opposing statements.13 | Suspend session, suppress output.31 | Prompt user for clarification.12 | Output contains severe safety violations.5 | Log the conflicting claims and cited context.11 | Run semantic alignment tests on prompt options.26 |
| **Runaway Cost** | Spend ledger flags budget limit breaches.1 | Trigger gateway rejection, block model calls.1 | Demote execution to low-cost fallback models.1 | Cumulative session cost exceeds limits.1 | Record the turn count, tokens used, and cost logs.24 | Verify gateway blocking configurations.30 |
| **Stochastic Drift** | Canary test scores drop below threshold.25 | Freeze deployment rollout, isolate traffic.31 | Roll back model versions, adjust sampling.25 | Production accuracy drops past acceptable limit.31 | Run stochastic replay against past production traces.11 | Run multi-run regression tests on model variants.25 |

## **Production Pathology Observability and Evaluation Guidance**

Evaluating stateful, probabilistic systems requires tracking explicit runtime metrics across executions.1 Analyzing final responses is not enough; systems must evaluate intermediate transformations and tool executions.2

### **Table 15: Observability Metrics**

| Metric Category | Technical Metric Identifier | Diagnostic Target | Target Threshold |
| :---- | :---- | :---- | :---- |
| **Formatting** | Malformed Output Rate | Parser exceptions divided by total responses.3 | \< 0.10% |
| **Formatting** | Schema Rejection Rate | Schema failures divided by total evaluations.3 | 0.00% under FSM decoding |
| **Formatting** | Repair Attempt Rate | Sessions needing format corrections.1 | \< 2.00% |
| **Formatting** | Repair Success Rate | Successful repairs divided by total repair attempts.1 | \> 98.0% |
| **Formatting** | Repair Hallucination Rate | Repairs that inject fabricated data.3 | 0.00% |
| **Evidence** | Invalid Citation Rate | Unverifiable citations divided by total citations.18 | \< 1.00% |
| **Evidence** | Citation Support Rate | Verified citations divided by total claims.18 | \> 98.0% |
| **Evidence** | Unsupported Claim Rate | Generated claims lacking source citations.18 | \< 1.50% |
| **Agentic** | Hallucinated Tool Rate | Tool calls missing from execution logs.2 | 0.00% |
| **Agentic** | Tool Call Mismatch Rate | Tool arguments mismatching schemas.2 | \< 0.50% |
| **Agentic** | Tool Loop Rate | Runs exceeding state repetition limits (C\_rep \> 2).2 | 0.00% |
| **Agentic** | Repeated State Rate | Sequential turns returning identical parameters.2 | \< 1.00% |
| **Execution** | Instruction Retention Score | Evaluations passing invariant check tests.7 | \> 99.0% |
| **Execution** | Harness Drift Rate | Layout templates modified by system updates.12 | 0.00% |
| **Execution** | Contradiction Rate | Responses with semantic conflicts.13 | \< 0.50% |
| **Execution** | Cross-Run Variance | Phrasing variance across identical prompts.25 | Variance std dev \< 0.02 |
| **Grounding** | Grounding Failure Rate | Responses failing semantic entailment checks.21 | \< 1.00% |
| **Operations** | Action Success Hallucination Rate | Spoken successes mismatching DB states.12 | 0.00% |
| **Operations** | Runaway Loop Termination Rate | Loops terminated by gateway budget limits.1 | 100% termination accuracy |
| **Operations** | Cost Overrun Rate | Total spend exceeding budget limits.1 | 0.00% budget breaches |
| **Operations** | Timeout-to-Success Misclassification | Hanging tools classified as successful runs.2 | 0.00% |
| **Operations** | Non-Deterministic Regression Delta | Variance between canary and production results.25 | Delta \< 0.02 |
| **Operations** | Replay Divergence Rate | Replays deviating from original traces.11 | 0.00% on identical code |
| **Operations** | Human Escalation Rate | Tasks routed to human operators.2 | \< 5.00% |
| **Operations** | User-Visible Incident Rate | Uncontained failures reaching users.1 | 0.00% |

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

Relying on system prompts to enforce formatting constraints is an anti-pattern that introduces unnecessary risks into production environments.4 Syntactic schema compliance must be enforced at the inference layer by compiling schemas into Finite State Machines or Context-Free Grammars.4 This step makes formatting violations mathematically impossible, letting downstream code parse payloads without throwing exceptions.3

### **II. Separate Formatting Compliance from Semantic Verification**

Syntactic correctness does not guarantee semantic accuracy.3 A model can generate valid JSON that contains fabricated facts or violates business rules.3 Systems must evaluate outputs using a multi-layered validation pipeline that separates formatting checks from business logic, grounding validation, and safety scans.3

### **III. Enforce Run-Level Financial and Execution Budgets**

Stateful agents can easily enter recursive execution loops, consuming compute credits and API budgets in minutes.1 To prevent financial overruns, execution paths must run within strict, run-level budgets managed by gateway proxies.1 Once a budget is exceeded, the gateway must terminate the run and return managed exceptions.1

### **IV. Validate Citations Prior to Output Generation**

A citation is not valid because it exists; it is valid only if it explicitly supports the claim it is attached to.18 Systems must analyze retrieved context, evaluate citation relevance, and verify quote alignments before delivering answers to users.12 Ungrounded claims must be suppressed or routed to review queues.12

### **V. Build for Complete Observability and Replayability**

Debugging non-deterministic, multi-turn agents is impossible without tracing their complete execution paths.11 Production environments must capture and record all external interactions, sampling parameters, and state changes in structured logs.11 These traces allow developers to reproduce logic paths verbatim, isolate regressions, and debug failures safely.11

#### **Works cited**

1. Runaway Agents, Tool Loops, and Budget Overruns — Cycles, accessed June 10, 2026, [https://runcycles.io/incidents/runaway-agents-tool-loops-and-budget-overruns-the-incidents-cycles-is-designed-to-prevent](https://runcycles.io/incidents/runaway-agents-tool-loops-and-budget-overruns-the-incidents-cycles-is-designed-to-prevent)  
2. How are you preventing runaway AI agent behavior in production? : r/LangChain \- Reddit, accessed June 10, 2026, [https://www.reddit.com/r/LangChain/comments/1rhwz53/how\_are\_you\_preventing\_runaway\_ai\_agent\_behavior/](https://www.reddit.com/r/LangChain/comments/1rhwz53/how_are_you_preventing_runaway_ai_agent_behavior/)  
3. Your LLM JSON Is Valid — And Still Wrong | by Rost Glukhov | May ..., accessed June 10, 2026, [https://medium.com/@rosgluk/your-llm-json-is-valid-and-still-wrong-12dbbecf1fdc](https://medium.com/@rosgluk/your-llm-json-is-valid-and-still-wrong-12dbbecf1fdc)  
4. Beyond JSON Mode: Getting Reliable Structured Outputs from LLMs ..., accessed June 10, 2026, [https://tianpan.co/blog/2025-10-29-structured-outputs-llm-production](https://tianpan.co/blog/2025-10-29-structured-outputs-llm-production)  
5. LLM06:2025 Excessive Agency \- OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm062025-excessive-agency/](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)  
6. LLM05:2025 Improper Output Handling \- OWASP Gen AI Security Project, accessed June 10, 2026, [https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/)  
7. LLM Context Window Management and Long-Context Strategies 2026 | Zylos Research, accessed June 10, 2026, [https://zylos.ai/research/2026-01-19-llm-context-management/](https://zylos.ai/research/2026-01-19-llm-context-management/)  
8. Lost-in-the-Middle Is Still Real in 2026 (Even on 1M-Token Models) \- DEV Community, accessed June 10, 2026, [https://dev.to/gabrielanhaia/lost-in-the-middle-is-still-real-in-2026-even-on-1m-token-models-2ehj](https://dev.to/gabrielanhaia/lost-in-the-middle-is-still-real-in-2026-even-on-1m-token-models-2ehj)  
9. \[KNOWLEDGE\] \- AI Engineering \- Volume 2\. D-F Knowledge, Data, and Corpus Engineering.md  
10. Has anyone actually solved runaway agent costs? Looking for patterns beyond logging \- API, accessed June 10, 2026, [https://community.openai.com/t/has-anyone-actually-solved-runaway-agent-costs-looking-for-patterns-beyond-logging/1383094](https://community.openai.com/t/has-anyone-actually-solved-runaway-agent-costs-looking-for-patterns-beyond-logging/1383094)  
11. Deterministic Replay: How to Debug AI Agents That Never Run the ..., accessed June 10, 2026, [https://tianpan.co/blog/2026-04-12-deterministic-replay-debugging-non-deterministic-ai-agents](https://tianpan.co/blog/2026-04-12-deterministic-replay-debugging-non-deterministic-ai-agents)  
12. \[KNOWLEDGE\] \- AI Engineering \- Volume 6\. P-R Multimodal and Interface-Controlling Systems  
13. Taxonomy of Hallucinations in AI \- Emergent Mind, accessed June 10, 2026, [https://www.emergentmind.com/topics/taxonomy-of-hallucinations](https://www.emergentmind.com/topics/taxonomy-of-hallucinations)  
14. Survey and analysis of hallucinations in large language models: attribution to prompting strategies or model behavior \- PMC, accessed June 10, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12518350/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12518350/)  
15. I tested structured output from 288 LLM calls and logged every way JSON breaks. Here's what I found : r/Python \- Reddit, accessed June 10, 2026, [https://www.reddit.com/r/Python/comments/1tagc2g/i\_tested\_structured\_output\_from\_288\_llm\_calls\_and/](https://www.reddit.com/r/Python/comments/1tagc2g/i_tested_structured_output_from_288_llm_calls_and/)  
16. Structured Output Isn't Reliable Output \- Rotascale, accessed June 10, 2026, [https://rotascale.com/blog/structured-output-isnt-reliable-output/](https://rotascale.com/blog/structured-output-isnt-reliable-output/)  
17. Measuring output stability across LLM runs (JSON drift problem) : r/LocalLLaMA \- Reddit, accessed June 10, 2026, [https://www.reddit.com/r/LocalLLaMA/comments/1qwgu6x/measuring\_output\_stability\_across\_llm\_runs\_json/](https://www.reddit.com/r/LocalLLaMA/comments/1qwgu6x/measuring_output_stability_across_llm_runs_json/)  
18. Citation Grounding: Detecting and Reducing LLM Citation Hallucinations via Legal Citation Graphs \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2606.00898v1](https://arxiv.org/html/2606.00898v1)  
19. CiteGuard: Faithful Citation Attribution for LLMs via Retrieval-Augmented Validation \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2510.17853v1](https://arxiv.org/html/2510.17853v1)  
20. A Comparative Analysis of Faithfulness Metrics and Humans in Citation Evaluation \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2408.12398v1](https://arxiv.org/html/2408.12398v1)  
21. How do I reduce hallucinations when using search-grounded LLM responses? \- Firecrawl, accessed June 10, 2026, [https://www.firecrawl.dev/glossary/web-search-apis/reduce-hallucinations-search-grounded-llm-responses](https://www.firecrawl.dev/glossary/web-search-apis/reduce-hallucinations-search-grounded-llm-responses)  
22. Context Rot: Why AI Gets Worse the Longer You Chat (And How to Fix It) \- Product Talk, accessed June 10, 2026, [https://www.producttalk.org/context-rot/](https://www.producttalk.org/context-rot/)  
23. Intelligence Degradation in Long-Context LLMs: Critical Threshold Determination via Natural Length Distribution Analysis \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2601.15300v1](https://arxiv.org/html/2601.15300v1)  
24. AI Agent Loop Token Costs: How to Constrain Context \- Augment Code, accessed June 10, 2026, [https://www.augmentcode.com/guides/ai-agent-loop-token-cost-context-constraints](https://www.augmentcode.com/guides/ai-agent-loop-token-cost-context-constraints)  
25. Non-Deterministic LLM Prompts in 2026: Practical Guide \- Future AGI, accessed June 10, 2026, [https://futureagi.com/blog/non-deterministic-llm-prompts-2025/](https://futureagi.com/blog/non-deterministic-llm-prompts-2025/)  
26. You Can't Assert Your Way Out of Non-Determinism: A Practical QA Strategy for LLM Applications | by Venkat Peri \- Medium, accessed June 10, 2026, [https://medium.com/@venkatperi/you-cant-assert-your-way-out-of-non-determinism-a-practical-qa-strategy-for-llm-applications-fd32e617cdec](https://medium.com/@venkatperi/you-cant-assert-your-way-out-of-non-determinism-a-practical-qa-strategy-for-llm-applications-fd32e617cdec)  
27. OWASP Top 10 LLM, Updated 2025: Examples & Mitigation Strategies \- Oligo Security, accessed June 10, 2026, [https://www.oligo.security/academy/owasp-top-10-llm-updated-2025-examples-and-mitigation-strategies](https://www.oligo.security/academy/owasp-top-10-llm-updated-2025-examples-and-mitigation-strategies)  
28. LLM Hallucination Examples \- Arize AI, accessed June 10, 2026, [https://arize.com/llm-hallucination-examples/](https://arize.com/llm-hallucination-examples/)  
29. CiteCheck: Towards Accurate Citation Faithfulness Detection \- arXiv, accessed June 10, 2026, [https://arxiv.org/html/2502.10881v1](https://arxiv.org/html/2502.10881v1)  
30. How to Prevent Runaway Agent Costs with MLflow AI Gateway, accessed June 10, 2026, [https://mlflow.org/blog/agent-costs-mlflow-gateway/](https://mlflow.org/blog/agent-costs-mlflow-gateway/)  
31. Monitoring LLM behavior: Drift, retries, and refusal patterns \- VentureBeat, accessed June 10, 2026, [https://venturebeat.com/infrastructure/monitoring-llm-behavior-drift-retries-and-refusal-patterns](https://venturebeat.com/infrastructure/monitoring-llm-behavior-drift-retries-and-refusal-patterns)  
32. Recursive Language Models: Could This Be the Real Fix for Long-Context AI in 2026? | by Vinod Polinati | Medium, accessed June 10, 2026, [https://medium.com/@vinodpolinati/recursive-language-models-could-this-be-the-real-fix-for-long-context-ai-in-2026-070328df9329](https://medium.com/@vinodpolinati/recursive-language-models-could-this-be-the-real-fix-for-long-context-ai-in-2026-070328df9329)  
33. The Essential Guide to the NIST AI Risk Management Framework 1.0 \- Vistrada, accessed June 10, 2026, [https://vistrada.com/resources/insights/nist-ai-risk-management-framework-1-0](https://vistrada.com/resources/insights/nist-ai-risk-management-framework-1-0)

---

[← Back to Canon Map](../canon-map.md)