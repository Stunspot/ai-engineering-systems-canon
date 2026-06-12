# AI-ENG-N — Tool Contracts - Function Calling, Schemas, Validation & Idempotency

## **Architectural Foundations: Probabilistic Proposals and Deterministic Edges**

In the engineering of production-grade agentic AI systems, a critical transition occurs at the boundary where generative models interface with the surrounding runtime environment. Generative language models are inherently probabilistic engines that operate on statistical token distributions. Because their outputs are non-deterministic, treating them as direct execution actors with unmediated access to databases, file systems, network sockets, or external transactional APIs introduces unacceptable systemic risks.1 This report establishes the architectural and engineering doctrine of tool contract engineering:  

> **Probabilistic actors need deterministic edges. A model may propose an action, but the system must validate, constrain, serialize, authorize, execute, and record that action through explicit contracts before anything touches real state.**  

This core principle organizes the design of the execution boundary. The foundational question in systems design is not "Can the model call tools?" but "What exact operation is being proposed, what arguments are valid, what permissions apply, what side effects may occur, what confirmation is required, what happens on retry, what transaction boundary governs execution, and what structured observation returns to the agent loop?"  

Architecturally, tool integration must not be treated as a simple model capability toggle or an inference-time prompt formatting detail. Instead, it must be governed as a highly structured, bidirectional interface discipline.2 While primitive function-calling APIs simply shape model output into JSON payloads, they fail to provide the safety, isolation, authentication, idempotency, or consistency guarantees required by enterprise software.4 The model generates a proposed action; the system must intercept, parse, validate, authorize, execute, and record that proposal inside a deterministic wrapper.2  

This boundary is bidirectional: inputs require strict schemas, semantic validation, credential injection, and transaction isolation, while outputs require structured status, typed results, and normalized observation parameters to prevent models from processing raw, unparsed system exceptions or ambiguous natural language.2

## **Canon Placement and Continuity**

This report belongs to Volume 5: Agentic Systems and Tool-Using Architectures, which details how static token generators are transformed into active systems actors. Within the logical progression of the volume, the architectural concerns are divided into three distinct phases:

* **AI-ENG-M (Agentic Orchestration):** Establishes the orchestration control plane, defining agent loops, state machines, tool selection policies, loop budgets, termination conditions, and escalation triggers.8  
* **AI-ENG-N (Tool Contracts):** Establishes the execution boundary. It governs interface design, schema validation, authorization, deterministic wrappers, idempotency patterns, repair loops, transaction boundaries, confirmation gates, and output contracts.2  
* **AI-ENG-O (Post-Action Verification):** Verifies state mutations *after* execution, reconciling state discrepancies, coordinating distributed recovery patterns, and orchestrating multi-step compensations.10

AI-ENG-N inherits critical principles from earlier canon modules:

* **AI-ENG-A (Model Steering):** Integrates the prompt harness and constrained decoding principles to achieve syntactical output structure.8  
* **AI-ENG-B (Context Tenure):** Restricts the scope of system information exposed to the model context window.8  
* **AI-ENG-C (Cost Attribution):** Establishes cost-tracking mechanisms to prevent run-away financial loops.7  
* **AI-ENG-G (Failure Tolerance):** Sets fallback behaviors and error recovery semantics.12  
* **AI-ENG-I (Release Manifests):** Requires immutable versioning of schemas to prevent behavioral drift.13  
* **AI-ENG-L (API Gateways):** Enforces gateway-level authorization, rate limiting, and credential injection.2

## **Conceptual Glossary**

| Term | Technical Definition | Operational Purpose |
| :---- | :---- | :---- |
| **Tool Contract** | The complete bi-directional specification defining inputs, validation gates, authorization requirements, side-effects, idempotency guarantees, and output formats.4 | Acts as the authoritative boundary between probabilistic models and deterministic state.4 |
| **Function Calling** | An inference primitive where a model is trained to output structured arguments conforming to schema descriptions instead of prose.5 | Acts as the initial serialization interface for generating a tool execution proposal.5 |
| **Structured Output** | A model generation constrained at the logit-sampling level via a Context-Free Grammar engine to ensure schema adherence.14 | Guarantees syntactical and structural compliance of raw outputs before parsing.5 |
| **Schema** | A formal definition of data structure, types, and constraints, written in standard format (e.g., JSON Schema or Protocol Buffers).4 | Establishes the syntax validation rules for tool calls and observation payloads.4 |
| **JSON Schema** | A declarative language for validating the shape, types, constraints, and dependencies of JSON data structures.4 | Standardizes the schema format accepted by tools and generative model APIs.4 |
| **OpenAPI** | A standard specification for describing RESTful HTTP APIs, detailing endpoints, authentication, schemas, and parameters.16 | Provides the structural baseline for generating machine-callable tool definitions.16 |
| **Deterministic Wrapper** | An application-layer wrapper that isolates external endpoints, sanitizes parameters, injects credentials, and enforces policies.2 | Prevents models from executing raw shell code, SQL queries, or unauthorized requests.2 |
| **Argument Validation** | The pipeline that parses, type-checks, and verifies a proposed tool call payload before execution.4 | Intercepts malformed or unauthorized model proposals before they mutate state.4 |
| **Semantic Validation** | Application-level validation of arguments against active business rules and logical state boundaries.14 | Prevents logical errors, such as negative numbers or invalid date orders, which pass standard type-checking.14 |
| **Authorization** | The process of verifying that the executing agent, user session, and tenant possess the required scopes to run a tool.2 | Enforces least-privilege security boundaries at the execution edge.3 |
| **Side Effect** | An external state change (such as database writes, payment charges, or email dispatches) caused by tool execution.18 | Determines the safety classification, confirmation requirement, and idempotency posture of a tool.4 |
| **Idempotency** | The mathematical property where an operation produces the same system state and output regardless of how many times it is run.18 | Ensured by the contract so that retries do not duplicate side-effects.18 |
| **Idempotency Key** | A unique, stable token supplied by the client to correlate retries of a single logical operation.9 | Enables the execution layer to identify duplicate requests and return cached responses.9 |
| **Retry** | The automatic re-execution of a validated request following a transient network or infrastructure failure.19 | Restores service continuity under temporary environmental instability.12 |
| **Repair Loop** | A closed loop where validation errors are returned to the model as structured feedback, prompting payload correction.7 | Automates recovery from semantic or structural validation failures.7 |
| **Transaction Boundary** | The logical boundary defining the scope, atomicity, consistency, and durability of state changes within a tool action.11 | Determines whether a tool's actions are committed, rolled back, or eventually consistent.11 |
| **Partial Success** | An execution state where some steps of a multi-step tool call succeed while others fail.10 | Requires explicit handling in the contract to coordinate partial rollbacks or compensations.10 |
| **Compensation** | An explicit, idempotent action designed to reverse the side effects of a previously committed local transaction.10 | Restores system consistency when subsequent steps in a distributed saga fail.10 |
| **Confirmation Gate** | A structured checkpoint pausing execution to obtain explicit operator or user approval before running a high-risk tool.2 | Prevents runaway agent loops from executing high-impact, irreversible mutations.2 |
| **Observation Object** | A normalized, sanitized, and typed object representing the authoritative result of a tool execution.4 | Feeds the agent's context window with structured, safe execution details.2 |

## **Tool Contract Anatomy**

### **Tool Contract Schema**

A mature tool contract is a declarative schema. It defines not just how to call a tool, but the entire operational, security, resource, and transactional posture of the interaction.4

```JSON  
{  
  "$schema": "https://json-schema.org/draft/2020-12/schema",  
  "id": "https://canon.ai-eng.org/v5/tool-contract.schema.json",  
  "title": "ToolContract",  
  "type": "object",  
  "required": [  
    "identity",  
    "affordance",  
    "runtime",  
    "security",  
    "transactional",  
    "idempotency",  
    "observability"  
  ],  
  "additionalProperties": false,  
  "properties": {  
    "identity": {  
      "type": "object",  
      "required": ["name", "version", "owner", "deprecation"],  
      "additionalProperties": false,  
      "properties": {  
        "name": {   
          "type": "string",   
          "pattern": "^[a-zA-Z0-9_.-]{1,128}$"   
        },  
        "version": { "type": "string" },  
        "owner": { "type": "string" },  
        "deprecation": {  
          "type": "object",  
          "required": ["status", "sunset_date"],  
          "additionalProperties": false,  
          "properties": {  
            "status": { "type": "string", "enum": ["active", "deprecated", "sunsetted"] },  
            "sunset_date": { "type": ["string", "null"], "format": "date" }  
          }  
        }  
      }  
    },  
    "affordance": {  
      "type": "object",  
      "required": ["model_description", "allowed_use_cases", "forbidden_use_cases", "input_schema", "output_schema"],  
      "additionalProperties": false,  
      "properties": {  
        "model_description": { "type": "string" },  
        "allowed_use_cases": { "type": "array", "items": { "type": "string" } },  
        "forbidden_use_cases": { "type": "array", "items": { "type": "string" } },  
        "input_schema": { "type": "object" },  
        "output_schema": { "type": "object" }  
      }  
    },  
    "runtime": {  
      "type": "object",  
      "required": ["timeout_ms", "rate_limits", "cost_profile", "sandbox_isolated"],  
      "additionalProperties": false,  
      "properties": {  
        "timeout_ms": { "type": "integer", "minimum": 1 },  
        "rate_limits": {  
          "type": "object",  
          "required": ["window_sec", "max_requests"],  
          "additionalProperties": false,  
          "properties": {  
            "window_sec": { "type": "integer" },  
            "max_requests": { "type": "integer" }  
          }  
        },  
        "cost_profile": {  
          "type": "object",  
          "required": ["currency", "cost_per_call"],  
          "additionalProperties": false,  
          "properties": {  
            "currency": { "type": "string" },  
            "cost_per_call": { "type": "number" }  
          }  
        },  
        "sandbox_isolated": { "type": "boolean" }  
      }  
    },  
    "security": {  
      "type": "object",  
      "required": ["required_scopes", "tenant_scoped", "secrets_boundary"],  
      "additionalProperties": false,  
      "properties": {  
        "required_scopes": { "type": "array", "items": { "type": "string" } },  
        "tenant_scoped": { "type": "boolean" },  
        "secrets_boundary": {  
          "type": "string",  
          "enum": ["gateway_injected", "execution_sandbox", "none"]  
        }  
      }  
    },  
    "transactional": {  
      "type": "object",  
      "required": ["side_effect_class", "confirmation_required", "semantics", "compensation_tool"],  
      "additionalProperties": false,  
      "properties": {  
        "side_effect_class": {  
          "type": "string",  
          "enum":  
        },  
        "confirmation_required": { "type": "boolean" },  
        "semantics": {  
          "type": "string",  
          "enum":  
        },  
        "compensation_tool": { "type": ["string", "null"] }  
      }  
    },  
    "idempotency": {  
      "type": "object",  
      "required": ["supported", "key_header", "ttl_seconds"],  
      "additionalProperties": false,  
      "properties": {  
        "supported": { "type": "boolean" },  
        "key_header": { "type": "string" },  
        "ttl_seconds": { "type": "integer" }  
      }  
    },  
    "observability": {  
      "type": "object",  
      "required": ["trace_attributes", "error_taxonomy_mapping"],  
      "additionalProperties": false,  
      "properties": {  
        "trace_attributes": { "type": "array", "items": { "type": "string" } },  
        "error_taxonomy_mapping": { "type": "object", "additionalProperties": { "type": "string" } }  
      }  
    }  
  }  
}
```

### **Model-Facing Affordances versus System-Facing Contracts**

A common architectural error is using the same representation for two distinct targets: the generative model and the execution system.

* **Model-Facing Affordances:** These are designed solely to guide model behavior during tool selection and token generation.4 They are written in natural language descriptions engineered for high semantic clarity, instructing the model on *when* to choose a tool, *what* arguments are required, and the logical boundaries of those parameters.4 This is an advisory layer.  

* **System-Facing Contracts:** These are rigid, programmatically enforced boundaries executed by the host application's deterministic wrapper.2 They perform physical schema checks, authorization validations, cost tracking, and transaction safety controls. The system-facing contract does not negotiate with the model; it enforces the invariants of the runtime environment to protect it from incorrect model behavior.

### **Tool Versioning and Behavior Drift**

Tool contracts must be versioned with the same rigor as traditional web APIs.13 Because models encode tool structures into their parametric weights or dynamic prompt contexts, any silent modification to a tool’s properties (such as renaming a field, changing a default value, or narrowing an enum range) can cause immediately failing tool calls or cascading failures in active workflows.13  
Backward compatibility must be preserved. When breaking changes are unavoidable, a formal migration path must be established. The system must register the new version of the contract, deprecate the old version, and phase it out according to strict sunset dates, giving workflow designers time to update agent prompting strategies and schemas.13

## **Schema Design and Affordance Engineering**

### **Model-Facing Tool Description Guide**

To achieve predictable tool selection and argument generation, tool description properties must be optimized for semantic clarity.14 The table below outlines how tool naming, descriptions, and parameter definitions must be formulated:

| Design Dimension | Pattern | Anti-Pattern | Operational Reason |
| :---- | :---- | :---- | :---- |
| **Tool Naming** | fetch_account_v1, charge_invoice_v2.4 | do_stuff, handle_billing, update.4 | Prevent tool selection confusion when model choices overlap.4 |
| **Tool Descriptions** | "EXECUTE ONLY to permanently void an invoice. Requester MUST have verified billing access.".4 | "Voids invoices or updates metadata on client files." | Narrow the model's action space by defining strict preconditions.4 |
| **Parameter Mapping** | "invoice_id": "The UUID of the invoice to void, matching 'inv_.*'." | "id": "The identifier." | Guide the model to extract and map exact parameters from context.14 |
| **Enum Boundary Definition** | "reason":.14 | "reason": "Why the invoice was voided (free text)." | Eliminate free-text generation at the boundary of a system transaction.14 |

### **Schema Design Guide for Model-Callable Tools**

To achieve high schema adherence, schemas must be designed as strict behavioral control surfaces.14 When using strict schema parsing (such as OpenAI's Structured Outputs with strict: true or equivalent frameworks), several key constraints must be built directly into the schema 15:

1. **Mandatory Field Declarations:** Optional keys are not supported in strict validation schemas.13 All properties listed under properties must be declared in the required array.15  
2. **Optional Property Emulation:** To define an optional property, developers must construct a JSON Schema union type with null (e.g., "type": ["string", "null"]), and explicitly provide null as the default value.14  
3. **Strict Object Constraints:** Every object definition must include "additionalProperties": false recursively to prevent the model from injecting parameters outside the schema.15  
4. **Schema Keyword Limitations:** Specific JSON Schema constraints, such as minLength, maxLength, pattern, format, minimum, and maximum are completely unsupported under strict parsing modes in certain primary provider SDKs.15 Consequently, physical range validations must be managed by the application validation layer rather than relying on logit-level restrictions during inference.23

## **The Argument Validation Pipeline**

### **Validation Stages and Gates**

Before a model's proposed tool invocation is sent to the execution engine, it must pass through a multi-stage validation pipeline.4

```
               │  
               ▼  
┌─────────────────────────────┐  
│ 1. Syntactic Parse Gate     │ ──► [Failure] ──► Emit SYNTACTIC_PARSE_FAIL  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 2. Structural Schema Gate   │ ──► [Failure] ──► Emit STRUCTURAL_VIOLATION  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 3. Type Checking Gate       │ ──► [Failure] ──► Emit TYPE_MISMATCH  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 4. Range Validation Gate    │ ──► [Failure] ──► Emit OUT_OF_BOUNDS  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 5. Semantic Logic Gate      │ ──► [Failure] ──► Emit SEMANTIC_INVALIDITY  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 6. IAM Scope Check Gate     │ ──► [Failure] ──► Emit PERMISSION_DENIED  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 7. Policy Compliance Gate   │ ──► [Failure] ──► Emit POLICY_VIOLATION  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 8. State Consistency Gate   │ ──► [Failure] ──► Emit STALE_STATE  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 9. Confirmation Logic Gate  │ ──► [Pending] ──► Emit CONFIRMATION_MISSING  
└─────────────────────────────┘  
               │  
               ▼  
┌─────────────────────────────┐  
│ 10. Loop Budget Gate        │ ──► [Failure] ──► Emit BUDGET_EXHAUSTED  
└─────────────────────────────┘  
               │  
               ▼
```

The table below defines the operation, error output, and error response strategy for each validation stage:

| Pipeline Gate | Execution Activity | Target Exception | Recovery Strategy |
| :---- | :---- | :---- | :---- |
| **Syntactic** | Parse raw generated text into structured JSON; catch format errors.4 | SYNTACTIC_PARSE_FAIL | Reject execution. Route raw error details to the model's repair loop.7 |
| **Structural** | Match parsed JSON against the JSON Schema schema; check for unexpected parameters.4 | STRUCTURAL_VIOLATION | Fail request. Prompt the model with missing or extra properties.7 |
| **Type** | Validate parameter types (e.g., ensure strings are not integers and booleans are not text strings).4 | TYPE_MISMATCH | Reject execution. Provide explicit type requirements to the model.7 |
| **Range** | Enforce numerical boundaries, array item counts, and string lengths.4 | OUT_OF_BOUNDS | Fail request. Enforce range parameters and prompt model for corrective values.7 |
| **Semantic** | Evaluate cross-field logic (e.g., ensure transaction amount is positive, or start date precedes end date).14 | SEMANTIC_INVALIDITY | Reject request. Map the business logic violation and instruct the agent.7 |
| **Permission** | Verify active IAM scopes, tenant bounds, and user session permissions for the tool.2 | PERMISSION_DENIED | Halt execution. Escalate to system logs, log security events, and notify user.2 |
| **Policy** | Validate corporate risk metrics, compliance rules, and security boundaries.3 | POLICY_VIOLATION | Terminate loop. Log policy failure to security dashboards.2 |
| **State** | Verify the target resource exists, is unlocked, and is in a valid state for update.17 | STALE_STATE | Return the actual database state of the object, prompting the agent to recheck intent.17 |
| **Confirmation** | Check if risk profile mandates human review; verify active approval token.2 | CONFIRMATION_MISSING | Pause loop execution. Yield control to user interface, emitting a confirmation card.4 |
| **Budget** | Monitor session token consumption, clock limits, and total monetary budgets.8 | BUDGET_EXHAUSTED | Terminate the run trace immediately, escalating to platform operations.8 |

## **Deterministic Wrapper Architecture**

### **Wrapper Components and Sequence**

The deterministic wrapper acts as the secure boundary layer protecting internal environments from raw, model-driven tool execution requests.2 It intercepts the model's proposed action, parses it, validates it, injects credentials outside the model's view, manages state, and returns a structured observation object.2

```
                [ Probabilistic Model Loop ]  
                             │  
                             ▼ (Proposes Tool Call)  
┌────────────────────────────────────────────────────────────┐  
│                  DETERMINISTIC WRAPPER                     │  
├────────────────────────────────────────────────────────────┤  
│ 1. Parser: Converts raw generated token text to JSON    │  
├────────────────────────────────────────────────────────────┤  
│ 2. Schema & Semantic Validator: Checks types & bounds  │  
├────────────────────────────────────────────────────────────┤  
│ 3. IAM Checker: Validates OAuth scopes & tokens [27]       │  
├────────────────────────────────────────────────────────────┤  
│ 4. Policy Interceptor: Enforces compliance & limits    │  
├────────────────────────────────────────────────────────────┤  
│ 5. Confirmation Gate: Pauses for operator review        │  
├────────────────────────────────────────────────────────────┤  
│ 6. Idempotency Mgr: Deduplicates repeating requests     │  
├────────────────────────────────────────────────────────────┤  
│ 7. Transaction Mgr: Starts transaction or saga         │  
├────────────────────────────────────────────────────────────┤  
│ 8. Executor: Runs isolated code in sandbox           │  
├────────────────────────────────────────────────────────────┤  
│ 9. Output Normalizer: Normalizes observation JSON       │  
├────────────────────────────────────────────────────────────┤  
│ 10. Trace Logger: Emits OpenTelemetry span telemetry   │  
└────────────────────────────────────────────────────────────┘  
                             │  
                             ▼ (Emits Structured Payload)  
                [ Normalized Observation ]
```

### **Credentials and Token Isolation**

Under no circumstances should raw secrets, API keys, OAuth tokens, database passwords, or private certificates be loaded directly into model context windows or defined in tool argument parameters.2 This is a critical security vulnerability.  
The model should only generate symbolic arguments, such as account_id or workspace_id.2 The deterministic wrapper intercepts these symbolic values, retrieves the corresponding credentials from a secure secrets vault, and injects those credentials during the execution phase, keeping sensitive tokens entirely hidden from the model's view.2

## **Side-Effect Classification and Control**

### **Side-Effect Classification Model**

Tools must be cataloged based on their risk profile and the difficulty of reversing their actions. Each classification mandates specific system controls:

| Side-Effect Class | Definition | Target System Gates | Idempotency Posture | Confirmation Policy |
| :---- | :---- | :---- | :---- | :---- |
| **READ_ONLY** | Retrieves data, performs calculations, or queries databases without modifying state.17 | Syntactic, Structural, Type, Range, Permission, State, Budget. | Exempt (assumed safe to retry by default).17 | No confirmation required. |
| **EPHEMERAL_WRITE** | Modifies temporary workspace objects, such as writing to scratch directory or code sandbox memory.2 | Syntactic, Structural, Type, Range, Permission, Budget. | Highly Recommended on state-mutating steps.18 | No confirmation required. |
| **LOW_RISK_INTERNAL** | Updates non-customer-visible database records, such as modifying internal ticket tags.17 | Syntactic, Structural, Type, Range, Permission, State, Budget. | Mandatory (low-latency key tracking).17 | No confirmation required. |
| **MEDIUM_RISK_WRITE** | Modifies operational database state, such as creating ticket drafts or internal comments.4 | All gates except full compliance checks. | Mandatory (24-hour persistent key).18 | Optional (managed by orchestrator rules).25 |
| **HIGH_RISK_EXTERNAL** | Sends emails, alters client profile data, or schedules system calendar updates.18 | All gates including user permission checks.6 | Strictly Mandatory (durable relational persistence).17 | Mandatory user confirmation required.2 |
| **CRITICAL_MUTATION** | Executes financial transfers, modifies access controls, or deploys codebase updates.3 | All gates including dual-control and compliance reviews.2 | Strictly Mandatory (distributed saga tracking).10 | Mandatory multi-party or operator gate.2 |

### **Side-Effect and Loop Budgets**

To prevent runaway agent loops from causing large cloud bills or system outages, execution margins must be monitored and enforced by the orchestrator.8  
The system must track token-consumption rates, clock time, tool invocation counts, and total monetary spend per session.7 If any of these boundaries are exceeded, the orchestrator terminates the loop, saves the trace history, and alerts operators.7

## **Idempotency, Retry Dynamics, and Repair Loops**

### **Idempotency Key Architecture**

Because at-least-once delivery is the only mathematically viable model over unreliable network transports, engineering for idempotency is a non-negotiable requirement for all side-effecting operations.17  
An idempotency key must be unique, stable across retries, and bound strictly to the logical scope of the operation.9 Keys must be generated deterministically on the client or wrapper side using the SHA-256 hash of the parameters and the logical coordinates of the task:  
Key = SHA-256(Workflow ID |  
| Tenant ID |  
| User ID |  
| Payload Hash)  
This formulation prevents key-reuse exploits and accidental collisions.9

### **Relational Co-location and TOCTOU Mitigation**

A common systemic failure is the Time-of-Check to Time-of-Use (TOCTOU) vulnerability.17 Using high-performance distributed key-value caches like Redis for idempotency checks (e.g., executing a SETNX lock, performing the payment API call, and then writing the result) introduces a race condition.17 Two concurrent retries can both check the cache, see that the key does not exist, and proceed to execute the payment twice.17  
The only safe implementation for critical mutation workflows is to co-locate the idempotency record and the business transaction in the same relational database transaction.17 The unique database constraint on the idempotency key column serves as the atomic locking mechanism 9:

```SQL  
-- Production Idempotency Table Schema  
CREATE TABLE idempotency_keys (  
    tenant_id BIGINT NOT NULL,  
    idempotency_key TEXT NOT NULL,  
    request_hash BYTEA NOT NULL,  
    status TEXT NOT NULL CHECK (status IN ('PENDING', 'COMPLETED', 'FAILED')),  
    response_status INTEGER,  
    response_body JSONB,  
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),  
    completed_at TIMESTAMPTZ,  
    expires_at TIMESTAMPTZ NOT NULL,  
    PRIMARY KEY (tenant_id, idempotency_key)  
);

CREATE INDEX idempotency_expires_idx ON idempotency_keys (expires_at)   
WHERE status IN ('PENDING', 'COMPLETED');

Go  
// Example of Relational Co-location of State and Intent  
func ExecuteIdempotentAction(ctx context.Context, db *sql.DB, tenantID int64, key string, hashbyte, action func() (int,byte, error)) (int,byte, error) {  
    tx, err := db.BeginTx(ctx, nil)  
    if err!= nil {  
        return 0, nil, err  
    }  
    defer tx.Rollback()

    // 1. Attempt to claim the key atomically  
    var status string  
    var cachedStatus int  
    var cachedBodybyte  
    var storedHashbyte  
      
    err = tx.QueryRowContext(ctx,   
        `SELECT status, response_status, response_body, request_hash   
         FROM idempotency_keys WHERE tenant_id = $1 AND idempotency_key = $2 FOR UPDATE`,   
        tenantID, key).Scan(&status, &cachedStatus, &cachedBody, &storedHash)

    if err == nil {  
        // Key exists: handle duplicate request  
        if!bytes.Equal(storedHash, hash) {  
            return 422, nil, fmt.Errorf("REQUEST_SIGNATURE_MISMATCH")  
        }  
        if status == "PENDING" {  
            return 409, nil, fmt.Errorf("CONCURRENT_REQUEST_IN_PROGRESS")  
        }  
        return cachedStatus, cachedBody, nil  
    } else if err!= sql.ErrNoRows {  
        return 0, nil, err  
    }

    // Key does not exist: reserve it  
    _, err = tx.ExecContext(ctx,   
        `INSERT INTO idempotency_keys (tenant_id, idempotency_key, request_hash, status, expires_at)   
         VALUES ($1, $2, $3, 'PENDING', now() + INTERVAL '24 hours')`,   
        tenantID, key, hash)  
    if err!= nil {  
        return 0, nil, err  
    }

    if err := tx.Commit(); err!= nil {  
        return 0, nil, err  
    }

    // 2. Execute the actual side-effect outside the locking transaction  
    respCode, respBody, execErr := action()  
      
    txUpdate, err := db.BeginTx(ctx, nil)  
    if err!= nil {  
        return 0, nil, err  
    }  
    defer txUpdate.Rollback()

    if execErr!= nil {  
        // Mark as failed to allow subsequent retries  
        _, _ = txUpdate.ExecContext(ctx,   
            `UPDATE idempotency_keys SET status = 'FAILED'   
             WHERE tenant_id = $1 AND idempotency_key = $2`, tenantID, key)  
        _ = txUpdate.Commit()  
        return respCode, respBody, execErr  
    }

    // 3. Complete the reservation with the execution response  
    _, err = txUpdate.ExecContext(ctx,   
        `UPDATE idempotency_keys   
         SET status = 'COMPLETED', response_status = $3, response_body = $4, completed_at = now()   
         WHERE tenant_id = $1 AND idempotency_key = $2`,   
        tenantID, key, respCode, respBody)  
    if err!= nil {  
        return 0, nil, err  
    }

    return respCode, respBody, txUpdate.Commit()  
}
```

### **Distinguishing Retries from Repair Loops**

* **Retry Dynamics:** Retries address transient, environmental failures (e.g., 503 Server Error, network timeouts).4 The request remains unchanged, and the execution wrapper retries using exponential backoff with random jitter 19:

wait_time = base_time * 2^n +/- jitter

* **Repair Loops:** Repair loops address semantic or structural failures where the model's payload is invalid.7 The client does not repeat the request. Instead, the validation error is formatted and returned to the model.7 The model corrects the parameters, creates a new execution proposal, and submits it to the validation pipeline.7

To prevent infinite runaway repair loops (which can occur if a model continuously attempts to fix bad data using the same incorrect logic), the repair loop must enforce strict constraints 7:

1. **Strict Error Limits:** Restrict repairs to a maximum of three consecutive attempts per execution proposal.7  
2. **Input Fingerprinting:** Compute a SHA-256 hash of the error feedback and input payload. If the model generates an identical argument signature on retry, halt execution, escalate the issue, and alert operators.7

## **Transaction Boundaries and Eventual Consistency**

### **Transaction Boundary Model**

When tool calls trigger complex, multi-step operations across multiple services, maintaining transactional integrity is critical.10

| Transaction Phase | Execution Strategy | Rollback Mechanism | Target Boundary State |
| :---- | :---- | :---- | :---- |
| **Compensable** | Execute local operations sequentially; log state changes in the saga journal.10 | Trigger backward compensating actions in reverse order.10 | Reversible / Tentative.11 |
| **Pivot** | Point of no return. Once this step succeeds, the transaction is legally committed.11 | None (irreversible).11 | Committed / Finalized.11 |
| **Retryable** | Idempotent operations following the pivot step.11 | Forward recovery (retry until successful).11 | Eventually Consistent.11 |

### **Sagas: Orchestration versus Choreography**

* **Choreography (Decentralized):** Services react to domain events without a central coordinator.10 While simple to deploy, this approach can lead to complex cyclic dependencies as workflows expand.10  
* **Orchestration (Centralized):** A central coordinator (the saga orchestrator) explicitly commands each service to execute its action and manages compensating actions if any step fails.10 For agentic architectures, centralized orchestration is required to manage complex dynamic paths and handle partial failures gracefully.10

## **Confirmation Gates and Human-in-the-Loop Orchestration**

### **Confirmation Gate Pattern Library**

Confirmation gates must map directly to the risk level of the tool's side effects 2:

| Pattern Name | Triggering Criteria | User Experience Model | Target System Action |
| :---- | :---- | :---- | :---- |
| **User Consent** | Accessing personal directories, repositories, or read-only customer metadata.2 | Explicit authorization card displaying targeted resources.2 | Proceed after verification token is issued. |
| **Operator Review** | Execution of medium-risk writes; minor transactional variations.2 | Display full argument comparison payload in supervisor dashboard.2 | Hold execution; release to worker queue upon approval. |
| **Dual Control (MofN)** | Money-moving transactions, database drops, or security posture edits.3 | Require approvals from multiple authorized identities.2 | Lock execution; proceed only when cryptographic keys are verified. |
| **Compliance Gate** | Legal commitments or cross-jurisdictional data transfers.3 | Static check of policy matrices combined with corporate sign-off.3 | Block transaction pending sign-off verification. |
| **Async Queue** | High-volume external actions scheduled for future execution.25 | Place actions in a batch-review interface with timeout limits.25 | Move transactions to holding tables; expire unless verified. |

### **Anti-Patterns: Consent Theater versus Payload Inspection**

Many agentic systems suffer from "consent theater," where the user is presented with a generic, uninformative prompt such as: *"The agent wishes to run tool charge_card. Do you want to proceed?"* This fails to provide meaningful security.4  
A secure confirmation gate must present the **concrete payload** and the **expected consequences** of the execution.2 It must show the exact parameters (such as the target account, amount, and fee) and highlight any potential anomalies (e.g., if the recipient address has changed).2

```
┌──────────────────────────────────────────────────────────┐  
│              CONFIRMATION REQUEST REQUIRED               │  
├──────────────────────────────────────────────────────────┤  
│ Action: Charge Customer Card                             │  
│ Target Account ID: cust_9921_beta                        │  
│ Amount: $4,999.00 USD                                    │  
│ Idempotency Fingerprint: f2a8...3b1a                     │  
│ Target Gateway: Stripe Production                        │  
│                                                          │  
└──────────────────────────────────────────────────────────┘
```

## **Output Contracts and Observation Quality**

### **Output Contract and Observation Object Schema**

To ensure the agent receives reliable data, the output returned by the execution wrapper must match a normalized contract.4 Tool executions must never return unstructured, raw logs or database exception traces directly to the model context.2

```JSON  
{  
  "$schema": "https://json-schema.org/draft/2020-12/schema",  
  "id": "https://canon.ai-eng.org/v5/observation.schema.json",  
  "title": "ObservationObject",  
  "type": "object",  
  "required": [  
    "tool_identity",  
    "execution_metadata",  
    "status",  
    "result_payload"  
  ],  
  "additionalProperties": false,  
  "properties": {  
    "tool_identity": {  
      "type": "object",  
      "required": ["name", "version", "call_id"],  
      "additionalProperties": false,  
      "properties": {  
        "name": { "type": "string" },  
        "version": { "type": "string" },  
        "call_id": { "type": "string", "format": "uuid" }  
      }  
    },  
    "execution_metadata": {  
      "type": "object",  
      "required": ["timestamp", "latency_ms", "idempotency_hit", "trace_id"],  
      "additionalProperties": false,  
      "properties": {  
        "timestamp": { "type": "string", "format": "date-time" },  
        "latency_ms": { "type": "integer" },  
        "idempotency_hit": { "type": "boolean" },  
        "trace_id": { "type": "string" }  
      }  
    },  
    "status": {  
      "type": "object",  
      "required": ["code", "is_error", "taxonomy_class"],  
      "additionalProperties": false,  
      "properties": {  
        "code": { "type": "integer" },  
        "is_error": { "type": "boolean" },  
        "taxonomy_class": {   
          "type": "string",  
          "enum":  
        }  
      }  
    },  
    "result_payload": {  
      "type": "object",  
      "required": ["data", "errors", "warnings", "verification_hints"],  
      "additionalProperties": false,  
      "properties": {  
        "data": { "type": "object" },  
        "errors": {  
          "type": "array",  
          "items": {  
            "type": "object",  
            "required": ["field", "message", "retryable"],  
            "additionalProperties": false,  
            "properties": {  
              "field": { "type": "string" },  
              "message": { "type": "string" },  
              "retryable": { "type": "boolean" }  
            }  
          }  
        },  
        "warnings": { "type": "array", "items": { "type": "string" } },  
        "verification_hints": {  
          "type": "object",  
          "required": ["target_state_endpoint", "delay_seconds"],  
          "additionalProperties": false,  
          "properties": {  
            "target_state_endpoint": { "type": ["string", "null"] },  
            "delay_seconds": { "type": "integer" }  
          }  
        }  
      }  
    }  
  }  
}
```

## **Safe Execution Pattern Library**

To protect internal systems from model-driven execution risks, execution wrappers must implement strict, sandboxed isolation rules 2:

* **Database Execution Engine:** Raw SQL generated by models must never be executed directly against database engines.2 Wrappers must use parameterized queries, read/write separation, query allowlists, Row-Level Security (RLS) policies, and query result caps.17  
* **File System Wrapper:** File mutation tools must restrict paths using path normalization and chroot directory confinement to prevent directory traversal attacks.3 Sandboxes must use soft-delete policies rather than permanent removal commands.3  
* **Code Execution Sandboxes:** Dynamic code generated by models (e.g., Python scripts) must execute in isolated gRPC micro-containers with strict CPU, memory, and network limits.2  
* **Browser and Web Inspection Tools:** Web browsing tools must use a domain allowlist, enforce navigation boundaries, and block Server-Side Request Forgery (SSRF) paths.3 Form submissions must be gated behind confirmation controls.2  
* **Email and Communication Dispatchers:** Outbound communication systems must route drafts to a staging interface first.3 Real-time dispatches must run verification checks against the recipient database to prevent accidental data leakages.2

## **Tool Error Taxonomy**

When a tool execution fails, the wrapper must map the technical response to a standardized system error taxonomy.4 This mapping enables the orchestrator to determine if the failure is retryable, if it can be repaired by the model, or if it must be escalated to an operator 17:

| Technical Error Code | Systemic Definition | Is Retryable? | Standard System Response Vector |
| :---- | :---- | :---- | :---- |
| SYNTACTIC_PARSE_FAIL | The model's generated output could not be parsed as valid JSON.4 | Yes (via repair) | Forward parsing details to the model's repair loop.7 |
| STRUCTURAL_VIOLATION | The parsed JSON fails to adhere to the input schema.4 | Yes (via repair) | Return the schema requirements to the repair loop.7 |
| TYPE_MISMATCH | A parameter's type differs from the schema definition.4 | Yes (via repair) | Map types and prompt the repair engine.7 |
| OUT_OF_BOUNDS | A parameter falls outside allowed numeric or range limits.4 | Yes (via repair) | Specify range boundaries and prompt repair.7 |
| SEMANTIC_INVALIDITY | The parameters violate application business rules.14 | No | Halt execution. Prompt agent to recheck inputs.14 |
| PERMISSION_DENIED | The user, tenant, or agent lacks the required authorization scopes.6 | No | Terminate transaction. Route trace to security logs.2 |
| POLICY_VIOLATION | The requested action violates system safety or compliance guidelines.3 | No | Halt execution. Trigger alert to compliance team.25 |
| STALE_STATE | The target resource state has changed since the action was proposed.17 | Yes (on refresh) | Update context with the new state; prompt model to re-evaluate.17 |
| CONFIRMATION_MISSING | The tool requires explicit approval, but no valid token was found.2 | No | Pause loop execution. Emit a card to the operator.4 |
| BUDGET_EXHAUSTED | The session cost, latency, or token limit has been exceeded.8 | No | Terminate the run trace and notify operations.8 |
| RATE_LIMITED | The tool endpoint has hit its execution limits.4 | Yes | Back off and retry using jittered intervals.4 |
| TIMEOUT | The tool execution failed to complete within the timeout window.4 | Yes | Apply retry logic or escalate if failures persist.4 |
| IDEMPOTENCY_CONFLICT | An execution attempt with this key is currently in progress.17 | Yes (later) | Return 409 Conflict with a suggested retry delay.17 |
| SIGNATURE_MISMATCH | A duplicate key was received, but the payload hash does not match.9 | No | Reject the request immediately to prevent payload tampering.17 |

## **Tool Contract Failure Mode Map**

Tool integrations often encounter complex, cascading failure modes where multiple layers of the system interact in unexpected ways. The following map outlines these failures and their systemic mitigations:

| Failure Mode | Direct Systemic Cause | Downstream Systemic Impact | Technical Mitigation Strategy |
| :---- | :---- | :---- | :---- |
| **Silent Data Corruption** | The model generates invalid parameter values that bypass weak string validations.24 | Corrupted database entries, leading to application crashes.24 | Enforce strict schemas, use strict enums, and apply semantic checks.14 |
| **Runaway Repair Loops** | The model repeatedly generates invalid arguments in response to validation errors.7 | Infinite loops that consume excessive tokens and increase costs.7 | Limit repair loops to 3 attempts; enforce input fingerprint checks.7 |
| **TOCTOU Race Condition** | A non-atomic "check-then-act" pattern is used across separate caching and execution layers.17 | Duplicate execution of side-effects, such as double payments.17 | Run key check and transaction commits within a single SQL transaction.17 |
| **Unsafe Retries** | A transient network timeout triggers a retry of a mutating endpoint without an idempotency key.18 | Duplicate state mutations, such as double customer charges.18 | Make the Idempotency-Key header mandatory for all POST requests.17 |
| **Prompt-Injection Tool Misuse** | Indirect prompt injection modifies tool parameters to bypass security boundaries.2 | Unauthorized data exfiltration or system modification.2 | Enforce least-privilege OAuth scopes and use secure sandboxes.2 |
| **Confirmation Theater** | Confirmation prompts ask for generic consent without displaying the concrete payload.2 | Users approve unintended or harmful operations.2 | Render specific argument values and security parameters in the UI.2 |
| **Credential Exfiltration** | Raw security credentials or API tokens are loaded directly into the model context.2 | Compromised system access if logs or outputs are leaked.2 | Ensure credentials live strictly in the wrapper or secure gateway.2 |
| **Schema Drift Outage** | A backend API structure is modified without updating the tool schema.13 | Immediate runtime failures or unparseable outputs in active workflows.13 | Use semantically versioned contracts and automated integration tests.13 |

## **Evaluation and Observability Guidance**

### **Key Telemetry and SRE Metrics**

To run reliable agentic systems, SRE teams must track specific operational metrics at the tool contract boundary:

* **Schema-Valid Call Rate (SVCR):** The ratio of tool execution proposals that successfully pass structural JSON schema checks on the first attempt:

SVCR = Successful Structural Checks / Total Tool Call Proposals

* **Validation Failure Rate (VFR):** The frequency of semantic, permission, and state validation checks that fail before execution:

VFR = Rejected Execution Attempts / Total Tool Call Proposals

* **Repair Success Rate (RSR):** The percentage of validation failures that are successfully corrected by the model's repair loop within the allocated budget:

RSR = Successfully Repaired Calls / Total Triggered Repair Loops

* **Idempotency Key Collision Rate (IKCR):** The rate of duplicate request attempts blocked or replayed by the idempotency layer:

IKCR = Idempotency Key Matches / Total Mutating Requests

### **OpenTelemetry and Context Propagation**

System operators must integrate tool execution spans with distributed tracing standards.28 Every tool call must generate a tracing span conforming to OpenTelemetry GenAI semantic conventions, propagating context through the Model Context Protocol (MCP) metadata layer 28:

1. **Context Propagation:** Inject W3C Trace Context parameters (traceparent and tracestate) directly into the _meta property bag of JSON-RPC requests.28  
2. **Span Linkage:** Ensure the outer model inference span is linked directly to the tool execution wrapper span.28  
3. **Domain Mapping:** Set gen_ai.operation.name to execute_tool, and map the low-cardinality target parameter to the exact tool contract name.28  
4. **Error Recording:** If a tool execution fails with isError: true, the span status must be marked as an error, and the error.type attribute must be set to tool_error.4

## **Cross-Canon Handoff Map**

The tool contract boundary described in **AI-ENG-N** serves as a core integration point across *The AI Engineering Systems Canon*:

| Target Module | Canonical Focus | Operational Handoff Vector |
| :---- | :---- | :---- |
| **AI-ENG-M** | Agentic Orchestration | Enforces high-level loops, task-level budgets, and termination flags that execute tool schemas.8 |
| **AI-ENG-O** | Post-Action Verification | Ingests normalized observation objects to verify successful database state changes.4 |
| **AI-ENG-S** | Tool Pathologies | Uses error codes to catch infinite loops and runaway execution paths.7 |
| **AI-ENG-T** | Security & Trust Boundaries | Restricts parameter spaces to prevent remote code and command injections.2 |
| **AI-ENG-U** | Tool Servers & Dependency Risk | Validates remote transport configurations and checks client authorization parameters.27 |
| **AI-ENG-V** | Resource Abuse & Cost Management | Enforces system rates and monitors token consumption during repair loops.7 |
| **AI-ENG-W** | Fallback & Degraded Modes | Triggers alternative providers when primary endpoints fail validation checks.12 |
| **AI-ENG-X** | User Control | Maps validation states to customer interfaces, allowing users to inspect raw parameters.2 |
| **AI-ENG-Y** | Approval Workflows | Uses verification results to trigger async review lists for high-risk actions.2 |
| **AI-ENG-Z** | Telemetry & Observability | Routes standardized span attributes to OpenTelemetry dashboards.28 |
| **AI-ENG-AA** | Agentic Evaluation & Benchmarking | Uses schema-valid stats to evaluate tool calling accuracy.31 |
| **AI-ENG-AB** | Auditability & Replay | Writes transaction payload details to immutable ledger logs.4 |
| **AI-ENG-AC** | Incident Response | Coordinates emergency procedures when cascading database locks occur.7 |
| **AI-ENG-AJ** | Agentic Reference Architectures | Implements the complete, end-to-end wrapper schemas and validation classes. |

## **Durable Principles of Tool Contract Architecture**

### **Probabilistic proposals require deterministic execution boundaries**

Language models must never be allowed to run unmediated database queries, shell scripts, or system commands.2 Instead, they must generate explicit, schema-bound proposals that are intercepted, evaluated, and executed inside a non-probabilistic application wrapper.2

### **Structural validation must occur prior to transactional execution**

To prevent partial execution failures and corrupt database states, all validation checks—syntactic, structural, semantic, and permission—must complete successfully before any mutating action is started.4

### **Mutations require relational co-location of idempotency keys**

To prevent duplicate execution errors on mutating endpoints (such as double charges or duplicate email dispatches), the verification of idempotency keys must run inside the same database transaction as the business logic itself.9

### **Isolate credentials completely from the model context**

API keys, OAuth tokens, and system secrets must never be exposed within prompt contexts or schema parameters.2 The model generates symbolic, off-context references, and the execution gateway injects the real credentials at the time of execution.2

### **Expose structured observation objects instead of raw logs**

Tool outputs must be cleaned, typed, and structured before they are returned to the model.4 Raw database stack traces or raw system logs must never be written to the model's context window.2 Ensure the model receives explicit statuses rather than inferring success from prose.4

#### **Works cited**

1. Specification - Model Context Protocol, accessed June 9, 2026, [https://modelcontextprotocol.io/specification/2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)  
2. Security Best Practices - Model Context Protocol, accessed June 9, 2026, [https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)  
3. mcp-security-best-practices-2025.md - GitHub, accessed June 9, 2026, [https://github.com/microsoft/mcp-for-beginners/blob/main/02-Security/mcp-security-best-practices-2025.md](https://github.com/microsoft/mcp-for-beginners/blob/main/02-Security/mcp-security-best-practices-2025.md)  
4. Tools - Model Context Protocol, accessed June 9, 2026, [https://modelcontextprotocol.io/specification/2025-11-25/server/tools](https://modelcontextprotocol.io/specification/2025-11-25/server/tools)  
5. Structured model outputs | OpenAI API, accessed June 9, 2026, [https://developers.openai.com/api/docs/guides/structured-outputs](https://developers.openai.com/api/docs/guides/structured-outputs)  
6. MCP Server Security Best Practices to Prevent Risk - Descope, accessed June 9, 2026, [https://www.descope.com/blog/post/mcp-server-security-best-practices](https://www.descope.com/blog/post/mcp-server-security-best-practices)  
7. Postmortem: How a runaway LLM loop burned through tokens for 40 minutes before I caught it : r/SaaS - Reddit, accessed June 9, 2026, [https://www.reddit.com/r/SaaS/comments/1s5jff7/postmortem_how_a_runaway_llm_loop_burned_through/](https://www.reddit.com/r/SaaS/comments/1s5jff7/postmortem_how_a_runaway_llm_loop_burned_through/)  
8. ai-eng-m-agentic-orchestration.md  
9. Idempotency in Distributed Systems: Design Patterns Beyond 'Retry ..., accessed June 9, 2026, [https://aloknecessary.github.io/blogs/idempotency-distributed-systems/](https://aloknecessary.github.io/blogs/idempotency-distributed-systems/)  
10. How to Implement the Saga Pattern for Distributed Transactions - OneUptime, accessed June 9, 2026, [https://oneuptime.com/blog/post/2026-02-20-microservices-saga-pattern/view](https://oneuptime.com/blog/post/2026-02-20-microservices-saga-pattern/view)  
11. Saga Design Pattern - Azure Architecture Center | Microsoft Learn, accessed June 9, 2026, [https://learn.microsoft.com/en-us/azure/architecture/patterns/saga](https://learn.microsoft.com/en-us/azure/architecture/patterns/saga)  
12. LLM Failover & Load Balancing for Provider Outages - Truefoundry, accessed June 9, 2026, [https://www.truefoundry.com/blog/llm-failover-load-balancing-provider-outages](https://www.truefoundry.com/blog/llm-failover-load-balancing-provider-outages)  
13. Codex provider: output_format schemas fail OpenAI Structured Outputs strict-mode (missing additionalProperties:false) · Issue #1843 · coleam00/Archon - GitHub, accessed June 9, 2026, [https://github.com/coleam00/Archon/issues/1843](https://github.com/coleam00/Archon/issues/1843)  
14. OpenAI Structured Outputs: Complete Developer Guide - Digital Applied, accessed June 9, 2026, [https://www.digitalapplied.com/blog/openai-structured-outputs-complete-guide](https://www.digitalapplied.com/blog/openai-structured-outputs-complete-guide)  
15. How to use structured outputs with Azure OpenAI in Microsoft Foundry Models, accessed June 9, 2026, [https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/structured-outputs](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/structured-outputs)  
16. OpenAPI additionalProperties - APIMatic, accessed June 9, 2026, [https://www.apimatic.io/openapi/additionalproperties](https://www.apimatic.io/openapi/additionalproperties)  
17. Idempotency Patterns: Building Retry-Safe Distributed Systems ..., accessed June 9, 2026, [https://backendbytes.com/articles/idempotency-patterns-distributed-systems/](https://backendbytes.com/articles/idempotency-patterns-distributed-systems/)  
18. Idempotency Keys: The API Pattern That Saves You From Duplicate Payments and Phantom Records - DEV Community, accessed June 9, 2026, [https://dev.to/apikumo/idempotency-keys-the-api-pattern-that-saves-you-from-duplicate-payments-and-phantom-records-51b2](https://dev.to/apikumo/idempotency-keys-the-api-pattern-that-saves-you-from-duplicate-payments-and-phantom-records-51b2)  
19. Designing robust and predictable APIs with idempotency - Stripe, accessed June 9, 2026, [https://stripe.com/blog/idempotency](https://stripe.com/blog/idempotency)  
20. Idempotent requests | Stripe API Reference, accessed June 9, 2026, [https://docs.stripe.com/api/idempotent_requests](https://docs.stripe.com/api/idempotent_requests)  
21. Idempotency in Distributed Transaction Systems | The ByteDoodle Blog, accessed June 9, 2026, [https://blog.bytedoodle.com/idempotency-in-distributed-transaction-systems/](https://blog.bytedoodle.com/idempotency-in-distributed-transaction-systems/)  
22. Navigating OpenAI's JSON-Structured Outputs: Limitations and Solutions - Daniel Saiz, accessed June 9, 2026, [https://dsaiztc.com/blog/posts/navigating-openai-json-structured-outputs.html](https://dsaiztc.com/blog/posts/navigating-openai-json-structured-outputs.html)  
23. I got tired of digging through Structured Outputs docs for every provider, so I tested what JSON Schema constraints actually work : r/LLMDevs - Reddit, accessed June 9, 2026, [https://www.reddit.com/r/LLMDevs/comments/1tarfl4/i_got_tired_of_digging_through_structured_outputs/](https://www.reddit.com/r/LLMDevs/comments/1tarfl4/i_got_tired_of_digging_through_structured_outputs/)  
24. Why structured outputs / strict JSON schema became non-negotiable in production agents : r/AI_Agents - Reddit, accessed June 9, 2026, [https://www.reddit.com/r/AI_Agents/comments/1qeetme/why_structured_outputs_strict_json_schema_became/](https://www.reddit.com/r/AI_Agents/comments/1qeetme/why_structured_outputs_strict_json_schema_became/)  
25. MCP security: Risks, MCP server exposure, and best practices for the AI agent era, accessed June 9, 2026, [https://www.nudgesecurity.com/post/mcp-security-risks-mcp-server-exposure-and-best-practices-for-the-ai-agent-era](https://www.nudgesecurity.com/post/mcp-security-risks-mcp-server-exposure-and-best-practices-for-the-ai-agent-era)  
26. Understanding Authorization in MCP - Model Context Protocol, accessed June 9, 2026, [https://modelcontextprotocol.io/docs/tutorials/security/authorization](https://modelcontextprotocol.io/docs/tutorials/security/authorization)  
27. Is that allowed? Authentication and authorization in Model Context Protocol - Stack Overflow, accessed June 9, 2026, [https://stackoverflow.blog/2026/01/21/is-that-allowed-authentication-and-authorization-in-model-context-protocol/](https://stackoverflow.blog/2026/01/21/is-that-allowed-authentication-and-authorization-in-model-context-protocol/)  
28. Semantic conventions for Model Context Protocol (MCP) - OpenTelemetry, accessed June 9, 2026, [https://opentelemetry.io/docs/specs/semconv/gen-ai/mcp/](https://opentelemetry.io/docs/specs/semconv/gen-ai/mcp/)  
29. Idempotency | System Design - AlgoMaster.io, accessed June 9, 2026, [https://algomaster.io/learn/system-design/idempotency](https://algomaster.io/learn/system-design/idempotency)  
30. Model Context Protocol - MLOps Community, accessed June 9, 2026, [https://mlops.community/blog/model-context-protocol](https://mlops.community/blog/model-context-protocol)  
31. How Consistent Are LLM Agents? Measuring Behavioral Reproducibility in Multi-Step Tool-Calling Pipelines - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2605.28840v1](https://arxiv.org/html/2605.28840v1)  
32. The Agent That Spent $47K on Itself: An Autonomous-Loop Postmortem - DEV Community, accessed June 9, 2026, [https://dev.to/gabrielanhaia/the-agent-that-spent-47k-on-itself-an-autonomous-loop-postmortem-3313](https://dev.to/gabrielanhaia/the-agent-that-spent-47k-on-itself-an-autonomous-loop-postmortem-3313)  
33. Saga patterns - AWS Prescriptive Guidance, accessed June 9, 2026, [https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga.html)  
34. What is Model Context Protocol (MCP)? A guide | Google Cloud, accessed June 9, 2026, [https://cloud.google.com/discover/what-is-model-context-protocol](https://cloud.google.com/discover/what-is-model-context-protocol)  
35. Specification (Draft) - Model Context Protocol （MCP）, accessed June 9, 2026, [https://modelcontextprotocol.info/specification/draft/](https://modelcontextprotocol.info/specification/draft/)  
36. The best providers for MCP server authentication in 2026 - WorkOS, accessed June 9, 2026, [https://workos.com/blog/best-mcp-server-authentication-providers](https://workos.com/blog/best-mcp-server-authentication-providers)

---

[← Back to Canon Map](../canon-map.md)