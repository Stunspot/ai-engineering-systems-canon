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

A mature tool contract is not just a function signature. It is the complete operational boundary between a probabilistic proposal and deterministic execution. The contract must define what the model may request, what the system will accept, how authorization is checked, how side effects are classified, how retries are handled, what transaction boundary applies, and what observation is returned to the agent loop.

### **Tool Contract Schema**

The following schema defines a durable tool-contract envelope. It separates model-facing affordances from system-facing enforcement so tool descriptions can guide model behavior without becoming the source of security truth.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://canon.ai-eng.org/v5/tool-contract.schema.json",
  "title": "ToolContract",
  "type": "object",
  "required": [
    "identity",
    "affordance",
    "runtime",
    "security",
    "transactional",
    "idempotency",
    "observability",
    "error_contract"
  ],
  "additionalProperties": false,
  "properties": {
    "identity": {
      "type": "object",
      "required": [
        "name",
        "version",
        "owner",
        "lifecycle"
      ],
      "additionalProperties": false,
      "properties": {
        "name": {
          "type": "string",
          "pattern": "^[a-zA-Z0-9_.-]{1,128}$"
        },
        "version": {
          "type": "string"
        },
        "owner": {
          "type": "string"
        },
        "lifecycle": {
          "type": "object",
          "required": [
            "status",
            "sunset_date",
            "replacement"
          ],
          "additionalProperties": false,
          "properties": {
            "status": {
              "type": "string",
              "enum": [
                "active",
                "deprecated",
                "sunsetted"
              ]
            },
            "sunset_date": {
              "type": [
                "string",
                "null"
              ],
              "format": "date"
            },
            "replacement": {
              "type": [
                "string",
                "null"
              ]
            }
          }
        }
      }
    },
    "affordance": {
      "type": "object",
      "required": [
        "model_description",
        "allowed_use_cases",
        "forbidden_use_cases",
        "input_schema",
        "output_schema",
        "examples"
      ],
      "additionalProperties": false,
      "properties": {
        "model_description": {
          "type": "string"
        },
        "allowed_use_cases": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "forbidden_use_cases": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "input_schema": {
          "type": "object"
        },
        "output_schema": {
          "type": "object"
        },
        "examples": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "description",
              "arguments"
            ],
            "additionalProperties": false,
            "properties": {
              "description": {
                "type": "string"
              },
              "arguments": {
                "type": "object"
              }
            }
          }
        }
      }
    },
    "runtime": {
      "type": "object",
      "required": [
        "timeout_ms",
        "rate_limits",
        "cost_profile",
        "sandbox_isolated",
        "max_retries"
      ],
      "additionalProperties": false,
      "properties": {
        "timeout_ms": {
          "type": "integer",
          "minimum": 1
        },
        "rate_limits": {
          "type": "object",
          "required": [
            "window_sec",
            "max_requests"
          ],
          "additionalProperties": false,
          "properties": {
            "window_sec": {
              "type": "integer",
              "minimum": 1
            },
            "max_requests": {
              "type": "integer",
              "minimum": 1
            }
          }
        },
        "cost_profile": {
          "type": "object",
          "required": [
            "currency",
            "cost_per_call",
            "cost_unit"
          ],
          "additionalProperties": false,
          "properties": {
            "currency": {
              "type": "string"
            },
            "cost_per_call": {
              "type": "number",
              "minimum": 0
            },
            "cost_unit": {
              "type": "string",
              "enum": [
                "call",
                "token",
                "second",
                "transaction",
                "custom"
              ]
            }
          }
        },
        "sandbox_isolated": {
          "type": "boolean"
        },
        "max_retries": {
          "type": "integer",
          "minimum": 0
        }
      }
    },
    "security": {
      "type": "object",
      "required": [
        "required_scopes",
        "tenant_scoped",
        "secrets_boundary",
        "data_classification",
        "egress_policy"
      ],
      "additionalProperties": false,
      "properties": {
        "required_scopes": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "tenant_scoped": {
          "type": "boolean"
        },
        "secrets_boundary": {
          "type": "string",
          "enum": [
            "gateway_injected",
            "execution_sandbox",
            "none"
          ]
        },
        "data_classification": {
          "type": "string",
          "enum": [
            "public",
            "internal",
            "confidential",
            "regulated",
            "secret"
          ]
        },
        "egress_policy": {
          "type": "string",
          "enum": [
            "none",
            "allowlisted_domains",
            "private_network_only",
            "unrestricted_with_approval"
          ]
        }
      }
    },
    "transactional": {
      "type": "object",
      "required": [
        "side_effect_class",
        "confirmation_required",
        "semantics",
        "compensation_tool",
        "post_action_verification_required"
      ],
      "additionalProperties": false,
      "properties": {
        "side_effect_class": {
          "type": "string",
          "enum": [
            "READ_ONLY",
            "EPHEMERAL_WRITE",
            "LOW_RISK_INTERNAL",
            "MEDIUM_RISK_WRITE",
            "HIGH_RISK_EXTERNAL",
            "CRITICAL_MUTATION"
          ]
        },
        "confirmation_required": {
          "type": "boolean"
        },
        "semantics": {
          "type": "string",
          "enum": [
            "read_only",
            "idempotent_write",
            "compensable_write",
            "saga_step",
            "irreversible_pivot",
            "retryable_post_pivot"
          ]
        },
        "compensation_tool": {
          "type": [
            "string",
            "null"
          ]
        },
        "post_action_verification_required": {
          "type": "boolean"
        }
      }
    },
    "idempotency": {
      "type": "object",
      "required": [
        "supported",
        "required",
        "key_header",
        "ttl_seconds",
        "payload_hash_required"
      ],
      "additionalProperties": false,
      "properties": {
        "supported": {
          "type": "boolean"
        },
        "required": {
          "type": "boolean"
        },
        "key_header": {
          "type": [
            "string",
            "null"
          ]
        },
        "ttl_seconds": {
          "type": [
            "integer",
            "null"
          ],
          "minimum": 1
        },
        "payload_hash_required": {
          "type": "boolean"
        }
      }
    },
    "observability": {
      "type": "object",
      "required": [
        "trace_attributes",
        "audit_required",
        "error_taxonomy_mapping"
      ],
      "additionalProperties": false,
      "properties": {
        "trace_attributes": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "audit_required": {
          "type": "boolean"
        },
        "error_taxonomy_mapping": {
          "type": "object",
          "additionalProperties": {
            "type": "string"
          }
        }
      }
    },
    "error_contract": {
      "type": "object",
      "required": [
        "repairable_errors",
        "retryable_errors",
        "fail_closed_errors",
        "escalation_errors"
      ],
      "additionalProperties": false,
      "properties": {
        "repairable_errors": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "retryable_errors": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "fail_closed_errors": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "escalation_errors": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      }
    }
  }
}
```

### **Model-Facing Affordances versus System-Facing Contracts**

A common architectural error is using the same representation for the generative model and the execution system.

| Layer | Audience | Purpose | Authority |
| :--- | :--- | :--- | :--- |
| **Model-Facing Affordance** | The model | Helps the model decide when to propose the tool and how to fill arguments. | Advisory only. |
| **System-Facing Contract** | The orchestrator, wrapper, gateway, validator, and auditor | Defines validation, authorization, execution, transaction, idempotency, and observation rules. | Enforced. |
| **Execution Wrapper** | Runtime infrastructure | Applies the contract to a specific request and returns a structured observation. | Operationally authoritative. |
| **Audit Trace** | SRE, compliance, incident response, replay systems | Records what was proposed, validated, approved, executed, returned, and verified. | Historical authority. |

The model-facing affordance should be clear, narrow, and behaviorally descriptive. It should explain when the tool is appropriate and what arguments mean.

The system-facing contract should be strict, versioned, and executable. It should not rely on model compliance, prompt obedience, or natural-language descriptions for safety.

### **Tool Versioning and Behavior Drift**

Tool contracts must be versioned with the same rigor as production APIs. A silent change to field names, defaults, enum values, authentication requirements, output shape, or side-effect semantics can break active workflows immediately.

A tool version is breaking when it changes:

* required fields
* argument names or types
* enum values
* default behavior
* authorization scopes
* side-effect class
* idempotency requirements
* output schema
* error taxonomy
* transaction or compensation semantics

Safe migration requires:

1. Register the new contract version.
2. Keep the old version active during a compatibility window.
3. Publish deprecation metadata and sunset date.
4. Route canary traffic through the new version.
5. Compare validation, repair, error, and observation metrics.
6. Migrate prompt/tool affordances.
7. Retire the old version only after downstream workflows are updated.

Tool contracts are part of the release manifest. A model, prompt template, tool schema, wrapper version, and observation schema should be promoted together or rolled back together.

## **Schema Design and Affordance Engineering**

Tool schemas are behavioral control surfaces. They shape what the model can propose, what the parser can accept, what the validator can enforce, and what the wrapper can safely execute.

Good schema design separates three layers:

1. **Generation guidance:** Helps the model produce the intended structure.
2. **Structural validation:** Ensures the payload is parseable and schema-conformant.
3. **Application validation:** Verifies permissions, business rules, state, risk, and side-effect safety.

### **Model-Facing Tool Description Guide**

| Design Dimension | Good Pattern | Anti-Pattern | Operational Reason |
| :--- | :--- | :--- | :--- |
| **Tool Naming** | `fetch_account_v1`, `create_invoice_draft_v2`, `void_invoice_v1` | `do_stuff`, `handle_billing`, `update` | Specific names reduce tool-selection confusion. |
| **Tool Description** | `EXECUTE ONLY to create a draft invoice. Does not send, charge, or finalize payment.` | `Handles invoices.` | Description must narrow the model's action space. |
| **Preconditions** | `Use only when requester has billing_read scope and an invoice_id from the current tenant.` | `Use when billing is involved.` | Preconditions prevent inappropriate tool selection. |
| **Parameter Mapping** | `invoice_id: UUID of the invoice, matching the active tenant invoice record.` | `id: The identifier.` | Precise parameter descriptions reduce argument confusion. |
| **Enum Boundary Definition** | `reason: one of ["duplicate", "customer_request", "fraud_review", "internal_error"]` | `reason: why the invoice was voided, free text.` | Enums prevent arbitrary transaction reasons at sensitive boundaries. |
| **Negative Capability** | `Do not use for refunds, payment capture, customer deletion, or access-control changes.` | No forbidden-use cases. | Forbidden-use cases help the router avoid hazardous overreach. |
| **Side-Effect Disclosure** | `This tool writes an internal draft only. It does not notify the customer.` | No side-effect description. | The model must distinguish draft, send, charge, delete, and deploy operations. |
| **Repair Hints** | `If customer_id is missing, ask the user rather than guessing.` | `Try to infer missing values.` | Repair instructions prevent invented parameters. |

### **Schema Design Guide for Model-Callable Tools**

Strict schemas should be designed to minimize ambiguity and maximize validation usefulness.

| Schema Principle | Recommended Practice | Reason |
| :--- | :--- | :--- |
| **Use explicit required fields** | Declare every expected property in `required`. | Reduces missing-field ambiguity. |
| **Represent optional values explicitly** | Use nullable union types such as `"type": ["string", "null"]` where supported. | Makes absence visible and parseable. |
| **Close object shapes** | Use `"additionalProperties": false` recursively where supported. | Prevents unrecognized parameters from crossing the execution boundary. |
| **Prefer enums over free text** | Use enumerated values for bounded decisions. | Reduces model improvisation at transaction boundaries. |
| **Use narrow object decomposition** | Split complex tools into smaller operations rather than one mega-tool. | Simplifies validation, authorization, and recovery. |
| **Avoid ambiguous IDs** | Use `invoice_id`, `customer_id`, `workspace_id`, not generic `id`. | Prevents cross-resource confusion. |
| **Keep generated arguments symbolic** | The model should provide business identifiers, not secrets, credentials, raw SQL, or shell commands. | Wrapper injects credentials and executes safely. |
| **Add semantic validators outside the schema** | Validate cross-field logic, state, permissions, and business rules in application code. | JSON Schema alone cannot enforce all runtime invariants. |
| **Version schemas explicitly** | Include schema version in contracts and traces. | Supports replay, compatibility testing, and rollback. |

### **Provider Strict-Mode versus Application Validation**

Structured-output systems and constrained decoding can improve syntactic adherence, but they do not replace application validation. Provider strict modes may support only a subset of JSON Schema features, and support can vary by API, SDK, model, or runtime.

| Validation Layer | Enforces | Should Be Trusted For |
| :--- | :--- | :--- |
| **Constrained Decoding / Structured Output** | Syntactic structure, object shape, required keys, and sometimes enum-like constraints. | Making malformed JSON less likely. |
| **Schema Parser** | Full local schema validation as implemented by the platform. | Catching unsupported or malformed generated payloads. |
| **Application Validator** | Business rules, state rules, permissions, tenant boundaries, numerical limits, and cross-field relationships. | Determining whether execution is allowed. |
| **Policy / Authorization Layer** | User identity, tenant scope, data residency, action risk, and approval requirements. | Enforcing security and governance. |
| **Post-Action Verifier** | Whether the external world actually changed as intended. | Confirming side effects after execution. |

A schema can make a payload well-formed. It cannot prove that the action is authorized, safe, current, affordable, or correct. That is the wrapper's job.

## **The Argument Validation Pipeline**

Before a model's proposed tool invocation reaches an execution engine, it must pass through deterministic validation gates. These gates convert a probabilistic proposal into an authorized execution request or a structured rejection.

```text
+--------------------------------------------------------------------------------
| ARGUMENT VALIDATION PIPELINE
+--------------------------------------------------------------------------------
|
| [ Model Tool Proposal ]
|      |
|      v
| [ 1. Syntactic Parse Gate ]
|      +--> failure: SYNTACTIC_PARSE_FAIL
|      |
|      v
| [ 2. Structural Schema Gate ]
|      +--> failure: STRUCTURAL_VIOLATION
|      |
|      v
| [ 3. Type Checking Gate ]
|      +--> failure: TYPE_MISMATCH
|      |
|      v
| [ 4. Range and Constraint Gate ]
|      +--> failure: OUT_OF_BOUNDS
|      |
|      v
| [ 5. Semantic Logic Gate ]
|      +--> failure: SEMANTIC_INVALIDITY
|      |
|      v
| [ 6. IAM / Tenant Scope Gate ]
|      +--> failure: PERMISSION_DENIED
|      |
|      v
| [ 7. Policy and Risk Gate ]
|      +--> failure: POLICY_VIOLATION
|      |
|      v
| [ 8. State Consistency Gate ]
|      +--> failure: STALE_STATE
|      |
|      v
| [ 9. Confirmation Gate ]
|      +--> pending: CONFIRMATION_MISSING
|      |
|      v
| [ 10. Budget and Rate-Limit Gate ]
|      +--> failure: BUDGET_EXHAUSTED or RATE_LIMITED
|      |
|      v
| [ Validated Execution Request ]
|
+--------------------------------------------------------------------------------
| Rule:
|   Execution begins only after parse, schema, semantic, permission, policy,
|   state, confirmation, and budget gates have passed.
+--------------------------------------------------------------------------------
```

| Pipeline Gate | Execution Activity | Target Exception | Standard Response |
| :--- | :--- | :--- | :--- |
| **Syntactic Parse** | Parse raw generated text into structured JSON or tool-call object. | `SYNTACTIC_PARSE_FAIL` | Reject execution and return parse error to repair loop if repair budget remains. |
| **Structural Schema** | Validate object shape, required fields, unknown fields, arrays, and nested objects. | `STRUCTURAL_VIOLATION` | Return missing, extra, or malformed fields to repair loop. |
| **Type Checking** | Verify primitive and compound types. | `TYPE_MISMATCH` | Return expected/actual type map to repair loop. |
| **Range and Constraint** | Enforce local numerical, string, enum, date, and cardinality limits. | `OUT_OF_BOUNDS` | Return bounded correction hint; do not execute. |
| **Semantic Logic** | Evaluate cross-field rules and business invariants. | `SEMANTIC_INVALIDITY` | Block action; ask model/user to re-evaluate inputs or route to clarification. |
| **IAM / Tenant Scope** | Verify active user, tenant, role, and tool scopes. | `PERMISSION_DENIED` | Block the tool call. Escalate only for suspicious, repeated, or high-risk attempts. |
| **Policy and Risk** | Check safety, compliance, data residency, side-effect class, and governance rules. | `POLICY_VIOLATION` | Fail closed for high-risk actions; otherwise return policy-safe explanation or escalation path. |
| **State Consistency** | Verify target resource exists, is current, unlocked, and in valid state. | `STALE_STATE` | Refresh authoritative state and ask the agent to re-plan. |
| **Confirmation** | Verify required approval token for high-impact action. | `CONFIRMATION_MISSING` | Suspend execution and render confirmation packet. |
| **Budget and Rate Limit** | Check loop budget, tool budget, cost cap, retry cap, and endpoint rate limits. | `BUDGET_EXHAUSTED` / `RATE_LIMITED` | Stop, defer, back off, or request budget approval depending on policy. |

Validation outcomes should produce structured feedback, not raw exception dumps. The feedback object should include:

```json
{
  "validation_status": "failed",
  "error_code": "TYPE_MISMATCH",
  "repairable": true,
  "retryable": false,
  "field_errors": [
    {
      "field": "invoice_id",
      "expected": "string UUID",
      "actual": "integer",
      "message": "invoice_id must be the invoice UUID from the active tenant scope."
    }
  ],
  "next_action": "repair_arguments"
}
```

The pipeline should never allow the model to “explain around” a failed gate. A failed gate blocks execution until a valid repair, approval, state refresh, fallback, or terminal decision occurs.

## **Deterministic Wrapper Architecture**

The deterministic wrapper is the secure execution boundary between the model and real systems. It receives a model-proposed action, validates the proposal, injects credentials outside the model context, enforces policy, executes the operation, normalizes the result, and emits trace evidence.

The wrapper is not a helper library. It is the enforcement point.

```text
+--------------------------------------------------------------------------------
| DETERMINISTIC WRAPPER ARCHITECTURE
+--------------------------------------------------------------------------------
|
| [ Probabilistic Model Loop ]
|      |
|      | proposes structured tool call
|      v
| [ Tool Proposal Envelope ]
|      |
|      v
| +-----------------------------+
| | 1. Parser                   |
| | raw output -> call object   |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 2. Schema Validator         |
| | structure, types, fields    |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 3. Semantic Validator       |
| | business and state logic    |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 4. IAM / Tenant Checker     |
| | scopes, roles, boundaries   |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 5. Policy Interceptor       |
| | risk, compliance, budgets   |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 6. Confirmation Gate        |
| | approval token if required  |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 7. Idempotency Manager      |
| | duplicate and replay guard  |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 8. Transaction Manager      |
| | local tx, saga, compensation|
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 9. Executor                 |
| | API, DB, queue, sandbox RPC |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 10. Output Normalizer       |
| | typed observation object    |
| +-------------+---------------+
|               |
|               v
| +-----------------------------+
| | 11. Trace Logger            |
| | span, audit, replay record  |
| +-------------+---------------+
|               |
|               v
| [ Normalized Observation ]
|      |
|      v
| [ Agent Loop Receives Verified Observation ]
|
+--------------------------------------------------------------------------------
| Rule:
|   The model proposes. The wrapper validates, authorizes, executes, records,
|   and returns a structured observation.
+--------------------------------------------------------------------------------
```

### **Wrapper Components and Responsibilities**

| Component | Responsibility | Failure Behavior |
| :--- | :--- | :--- |
| **Parser** | Converts model output into a structured call object. | Returns `SYNTACTIC_PARSE_FAIL`; no execution. |
| **Schema Validator** | Validates object shape, required fields, and types. | Returns repairable schema error. |
| **Semantic Validator** | Checks business rules and cross-field constraints. | Blocks action and requests re-plan or clarification. |
| **IAM / Tenant Checker** | Verifies user, tenant, role, and tool scopes. | Blocks action; escalates only if suspicious or severe. |
| **Policy Interceptor** | Applies compliance, risk, data-residency, and budget rules. | Fails closed or routes to review depending on risk. |
| **Confirmation Gate** | Requires explicit approval for high-risk actions. | Suspends execution until approval or rejection. |
| **Idempotency Manager** | Prevents duplicate side effects under retries. | Returns cached response, conflict, or in-progress status. |
| **Transaction Manager** | Defines local transaction, saga, compensation, or retry boundary. | Rolls back, compensates, or records partial success. |
| **Executor** | Performs API call, database operation, queue dispatch, or sandbox execution. | Captures raw result, timeout, or exception. |
| **Output Normalizer** | Converts raw result into typed observation object. | Returns observation-normalization error if parsing fails. |
| **Trace Logger** | Emits OpenTelemetry spans, audit logs, and replay metadata. | Preserves audit-critical trace before returning. |

### **Credentials and Token Isolation**

Raw secrets, API keys, OAuth tokens, database passwords, private certificates, and service credentials must never appear in model context or tool arguments.

The model should generate symbolic identifiers:

```json
{
  "tool": "fetch_invoice_v1",
  "arguments": {
    "tenant_id": "tenant_123",
    "invoice_id": "inv_456"
  }
}
```

The wrapper resolves credentials outside the model's view:

```text
symbolic identifiers -> policy check -> vault lookup -> scoped credential injection -> execution
```

Credential rules:

* The model never sees raw secrets.
* The wrapper injects credentials only after authorization passes.
* Credentials are scoped to the active user, tenant, and tool.
* Credentials should expire quickly and be bound to the run or call.
* Trace records should store credential reference IDs, not secret values.
* Tool outputs must be scrubbed before returning to the model.

Credential isolation is not optional. It is the difference between tool use and giving a stochastic parrot a root password with vibes-based supervision.

## **Side-Effect Classification and Control**

Tools must be classified by their side-effect profile, reversibility, external visibility, cost exposure, and required approval posture. Side-effect classification determines which gates must fire before execution and how the system handles retries, failures, compensation, and audit.

### **Side-Effect Classification Model**

| Side-Effect Class | Definition | Required Gates | Idempotency Posture | Confirmation Policy | Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **READ_ONLY** | Retrieves data or performs calculation without intended state mutation. | Parse, schema, type, permission, policy, budget, rate limit. | Not required for correctness, but request de-duplication and caching may still be useful. | No confirmation unless sensitive data scope expands. | Fetch account status, search docs, calculate tax estimate. |
| **EPHEMERAL_WRITE** | Writes temporary or sandbox-local state that is not externally visible. | Parse, schema, type, permission, sandbox, budget. | Recommended when retries may duplicate files, logs, or workspace artifacts. | Usually not required. | Write temp file, create local diagnostic artifact. |
| **LOW_RISK_INTERNAL** | Updates non-customer-visible internal state with limited blast radius. | All validation gates plus state consistency and trace logging. | Required for mutating operations. | Usually not required unless policy says otherwise. | Add internal tag, create draft ticket note. |
| **MEDIUM_RISK_WRITE** | Modifies operational state or internal workflow state that may affect users or teams. | All validation gates, state consistency, idempotency, audit trace, rollback or compensation plan. | Mandatory. | Conditional on risk, tenant policy, or workflow stage. | Create support ticket, update internal CRM field. |
| **HIGH_RISK_EXTERNAL** | Sends external communications, changes user-visible records, or schedules commitments. | All gates plus confirmation, audit, idempotency, and post-action verification. | Strictly mandatory. | Mandatory user or operator confirmation. | Send email, change customer profile, schedule customer event. |
| **CRITICAL_MUTATION** | Moves money, changes access control, deploys code, deletes data, or alters security posture. | All gates plus dual control, compliance review, compensation or rollback plan, and incident-grade trace. | Strictly mandatory with durable persistence. | Mandatory multi-party or privileged operator approval. | Charge card, transfer funds, grant admin role, deploy production change. |

### **Read-Only Does Not Mean Risk-Free**

Read-only tools are safer than mutating tools, but they are not automatically harmless.

Read-only calls can still:

* expose sensitive data
* violate tenant boundaries
* hit costly provider APIs
* trigger rate limits
* reveal externally observable access patterns
* return poisoned or prompt-injected content
* leak regulated data into model context
* overload retrieval or database systems

For that reason, read-only tools still require permission, policy, rate-limit, budget, and observation-normalization controls.

### **Side-Effect and Loop Budgets**

Side-effect class must influence loop budgets. A run that is allowed thirty read-only lookups should not automatically be allowed thirty email sends, invoice changes, or database writes.

| Budget Dimension | Applies To | Example Control |
| :--- | :--- | :--- |
| **Read Budget** | Retrieval, lookup, search, calculation. | Limit queries per run and per tenant. |
| **Write Budget** | Internal state changes and workspace mutations. | Limit commits and require trace IDs. |
| **External Action Budget** | Emails, tickets, calendar updates, provider calls. | Require approval and idempotency keys. |
| **Financial Budget** | Payments, purchases, API charges, paid tools. | Enforce dollar caps and approval thresholds. |
| **Critical Mutation Budget** | Access control, production deploys, destructive actions. | Default zero unless explicitly authorized. |

The orchestrator should treat side effects as budgeted capabilities, not incidental tool behavior. A model should never discover that it has mutation authority by trying it and seeing what happens. That is not exploration. That is an incident report warming up.

## **Idempotency, Retry Dynamics, and Repair Loops**

Retries are unavoidable in distributed systems. Network calls time out, providers return transient errors, clients reconnect, queues redeliver messages, and workers crash. Because tool calls may mutate real state, every side-effecting operation must define how duplicate attempts are detected and handled.

### **Idempotency Key Architecture**

An idempotency key identifies one logical operation across retries. It must be unique, stable across retries, scoped to the tenant and operation, and bound to a payload hash so the same key cannot be reused for a different request.

A safe key can be derived as:

```text
IdempotencyKey = SHA256(
  workflow_id,
  run_id,
  tenant_id,
  user_id,
  tool_name,
  tool_version,
  logical_operation_id,
  payload_hash
)
```

Where:

| Field | Purpose |
| :--- | :--- |
| `workflow_id` | Binds key to the parent workflow. |
| `run_id` | Binds key to a specific agent run. |
| `tenant_id` | Prevents cross-tenant key collision. |
| `user_id` | Binds operation to active principal. |
| `tool_name` / `tool_version` | Prevents reuse across different contracts. |
| `logical_operation_id` | Identifies the intended business action. |
| `payload_hash` | Prevents same key from authorizing different arguments. |

### **Idempotency State Model**

An idempotency record should track the lifecycle of the logical operation:

| Status | Meaning | Duplicate Request Behavior |
| :--- | :--- | :--- |
| **PENDING** | Operation has been reserved but not completed. | Return `409 CONCURRENT_REQUEST_IN_PROGRESS` or retry-after. |
| **COMPLETED** | Operation succeeded and response is cached. | Return cached response. |
| **FAILED_RETRYABLE** | Attempt failed before committing side effect. | Allow retry with same key and same payload hash. |
| **FAILED_FINAL** | Attempt failed in a way that must not be retried automatically. | Return stored failure and require repair or escalation. |
| **COMPENSATED** | Operation committed but later compensation completed. | Return compensated state and require post-action verification. |
| **EXPIRED** | Key expired according to retention policy. | Treat according to contract: reject, rebuild from ledger, or require new operation ID. |

### **Relational Co-Location and TOCTOU Mitigation**

A common failure mode is the Time-of-Check to Time-of-Use race. If the wrapper checks a cache, executes a side effect, and only later records the result, concurrent retries can duplicate the side effect.

For critical mutations, the local idempotency record and local business state should be coordinated in the same durable transactional store whenever possible. For external provider calls that cannot be included in the local database transaction, the wrapper must also pass the idempotency key to the external provider when supported and reconcile the final state through post-action verification.

A local idempotency table can be modeled as:

```sql
CREATE TABLE idempotency_keys (
    tenant_id BIGINT NOT NULL,
    idempotency_key TEXT NOT NULL,
    request_hash BYTEA NOT NULL,
    status TEXT NOT NULL CHECK (
        status IN (
            'PENDING',
            'COMPLETED',
            'FAILED_RETRYABLE',
            'FAILED_FINAL',
            'COMPENSATED',
            'EXPIRED'
        )
    ),
    response_status INTEGER,
    response_body JSONB,
    error_code TEXT,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    completed_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ NOT NULL,
    PRIMARY KEY (tenant_id, idempotency_key)
);

CREATE INDEX idempotency_keys_expires_idx
ON idempotency_keys (expires_at)
WHERE status IN ('PENDING', 'COMPLETED', 'FAILED_RETRYABLE');
```

A safe execution flow is:

```text
1. Compute payload_hash from canonicalized request arguments.

2. Begin local transaction.

3. SELECT idempotency record FOR UPDATE.

4. If record exists:
     - If request_hash differs:
         return SIGNATURE_MISMATCH.
     - If status is COMPLETED:
         return cached response.
     - If status is PENDING:
         return CONCURRENT_REQUEST_IN_PROGRESS.
     - If status is FAILED_FINAL:
         return stored failure.
     - If status is FAILED_RETRYABLE:
         continue only if retry policy permits.

5. If record does not exist:
     - INSERT record with status PENDING.

6. Commit local reservation.

7. Execute side effect with same idempotency key where supported.

8. Capture raw result, timeout, or exception.

9. Begin update transaction.

10. Store final status:
      - COMPLETED if operation succeeded.
      - FAILED_RETRYABLE if no side effect committed and retry is safe.
      - FAILED_FINAL if retry is unsafe.
      - COMPENSATED if compensation completed.

11. Commit update.

12. Return normalized observation object.
```

This pattern avoids the false comfort of “we checked Redis, therefore we are safe.” Caches are useful. They are not a transaction boundary for critical mutations.

### **Retries versus Repair Loops**

Retries and repairs solve different problems and must not be conflated.

| Mechanism | Problem Type | Payload Changes? | Example | Control |
| :--- | :--- | :---: | :--- | :--- |
| **Retry** | Transient environmental failure. | No | Timeout, 503, connection reset, temporary rate limit. | Same payload, same idempotency key, exponential backoff with jitter. |
| **Repair Loop** | Invalid model proposal. | Yes | Missing field, wrong type, invalid enum, semantic mismatch. | New proposal after structured validation feedback. |
| **Re-plan** | Tool result changes task state or reveals goal infeasibility. | Usually yes | Resource no longer exists, source conflict, permission missing. | Return to planner with verified observation. |
| **Escalation** | Automated continuation is unsafe or uncertain. | No autonomous execution. | Confirmation needed, budget exhausted, critical mutation. | Human, safer model, or fail-closed path. |

Retry backoff should use jitter:

```text
wait_time = base_time * 2^attempt + random_jitter
```

Repair loops must be bounded:

| Repair Control | Purpose |
| :--- | :--- |
| **Maximum repair attempts** | Prevents infinite argument correction loops. |
| **Input fingerprinting** | Detects repeated identical bad payloads. |
| **Error fingerprinting** | Detects repeated identical validation failures. |
| **Repair budget** | Prevents token spend from growing without progress. |
| **Escalation threshold** | Moves persistent failures to human review or terminal state. |

A repair loop should stop when the model produces the same invalid argument signature twice, consumes the repair budget, or triggers a non-repairable error such as `PERMISSION_DENIED`, `POLICY_VIOLATION`, or `SIGNATURE_MISMATCH`.

Transaction Boundaries and Eventual Consistency**

## **Confirmation Gates and Human-in-the-Loop Orchestration**

Confirmation gates transfer decision authority from the autonomous loop to a human, operator, supervisor, compliance process, or multi-party approval mechanism. A confirmation gate is required when the model proposes an action whose side effects exceed the autonomy boundary.

### **Confirmation Gate Pattern Library**

| Pattern Name | Triggering Criteria | User Experience Model | Target System Action |
| :--- | :--- | :--- | :--- |
| **User Consent** | Accessing personal directories, private repositories, customer metadata, or sensitive read scopes. | Authorization card showing exact resource, scope, duration, and requesting workflow. | Issue scoped approval token or reject access. |
| **Operator Review** | Medium-risk writes, internal workflow mutations, or ambiguous business consequences. | Supervisor dashboard showing proposed action, payload, before/after state, and evidence. | Hold execution until approval or rejection. |
| **Dual Control / M-of-N** | Money movement, access-control changes, destructive database operations, production deployment. | Multi-approver workflow with role checks and cryptographic or logged approval events. | Proceed only after required approvals are collected. |
| **Compliance Gate** | Legal commitment, regulated data movement, cross-border transfer, policy exception. | Policy review packet with justification, data class, jurisdiction, and accountable owner. | Block transaction until compliance approval. |
| **Async Review Queue** | High-volume actions that can wait for batch approval. | Queue with sorting, expiry, sampling, and bulk review controls. | Hold actions in pending table until approved or expired. |

### **Consent Theater versus Payload Inspection**

Consent theater occurs when the user is shown a generic prompt such as:

```text
The agent wants to run charge_card. Proceed?
```

That is not meaningful approval. A secure confirmation gate must show the concrete payload, expected consequence, risk class, and rejection outcome.

A useful confirmation packet includes:

| Field | Purpose |
| :--- | :--- |
| **Action name** | Identifies the tool and contract version. |
| **Plain-language consequence** | Explains what will happen if approved. |
| **Concrete arguments** | Shows exact target IDs, amount, recipients, dates, or resources. |
| **Before state** | Shows current authoritative state when available. |
| **After state** | Shows expected state after successful execution. |
| **Idempotency fingerprint** | Allows duplicate detection and audit linkage. |
| **Risk class** | Explains why approval is required. |
| **Approval expiry** | Prevents stale approvals from authorizing later actions. |
| **Approver identity** | Records who approved or rejected the action. |
| **Rollback / compensation status** | States whether the action can be reversed. |
| **Rejection path** | Explains what happens if approval is denied. |
| **Trace ID** | Links confirmation to replay and audit records. |

Example confirmation card:

```text
+--------------------------------------------------------------------------------
| CONFIRMATION REQUEST REQUIRED
+--------------------------------------------------------------------------------
|
| Action:
|   Charge Customer Card
|
| Tool:
|   charge_customer_card_v2
|
| Target Account:
|   cust_9921_beta
|
| Amount:
|   $4,999.00 USD
|
| Before State:
|   invoice inv_1007 is open and unpaid
|
| Expected After State:
|   invoice inv_1007 is marked paid if charge succeeds
|
| Idempotency Fingerprint:
|   f2a8...3b1a
|
| Risk Class:
|   CRITICAL_MUTATION
|
| Compensation:
|   Refund may be possible, but original charge is externally visible
|
| Approval Expires:
|   10 minutes after creation
|
| If Rejected:
|   The run will terminate with CONFIRM_REQUIRED_REJECTED
|
| Trace:
|   trace_abc123
|
+--------------------------------------------------------------------------------
```

### **Approval Tokens**

Approval should produce a structured token that the wrapper can verify.

```json
{
  "approval_id": "appr_123",
  "trace_id": "trace_abc123",
  "tool_name": "charge_customer_card_v2",
  "tool_version": "2.1.0",
  "payload_hash": "sha256:f2a8...",
  "approver_id": "user_456",
  "approved_at": "2026-06-13T15:30:00Z",
  "expires_at": "2026-06-13T15:40:00Z",
  "approval_scope": "single_execution",
  "decision": "approved"
}
```

The wrapper must reject approval tokens that are expired, scoped to a different payload hash, issued by an unauthorized approver, reused outside policy, or detached from the active trace.

## **Output Contracts and Observation Quality**

Tool executions must return normalized observation objects, not raw logs, ambiguous prose, database stack traces, or provider-specific error blobs. The observation object is the authoritative execution result that the agent loop may use for subsequent reasoning.

The output contract must distinguish:

* tool identity
* execution metadata
* status
* normalized result data
* structured errors
* warnings
* verification hints
* trace and audit pointers

### **Observation Object Schema**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://canon.ai-eng.org/v5/observation.schema.json",
  "title": "ObservationObject",
  "type": "object",
  "required": [
    "tool_identity",
    "execution_metadata",
    "status",
    "result_payload",
    "verification"
  ],
  "additionalProperties": false,
  "properties": {
    "tool_identity": {
      "type": "object",
      "required": [
        "name",
        "version",
        "call_id"
      ],
      "additionalProperties": false,
      "properties": {
        "name": {
          "type": "string"
        },
        "version": {
          "type": "string"
        },
        "call_id": {
          "type": "string"
        }
      }
    },
    "execution_metadata": {
      "type": "object",
      "required": [
        "timestamp",
        "latency_ms",
        "idempotency_hit",
        "trace_id",
        "attempt_number"
      ],
      "additionalProperties": false,
      "properties": {
        "timestamp": {
          "type": "string",
          "format": "date-time"
        },
        "latency_ms": {
          "type": "integer",
          "minimum": 0
        },
        "idempotency_hit": {
          "type": "boolean"
        },
        "trace_id": {
          "type": "string"
        },
        "attempt_number": {
          "type": "integer",
          "minimum": 1
        }
      }
    },
    "status": {
      "type": "object",
      "required": [
        "code",
        "is_error",
        "taxonomy_class",
        "retryable",
        "repairable",
        "requires_approval",
        "fail_closed"
      ],
      "additionalProperties": false,
      "properties": {
        "code": {
          "type": "integer"
        },
        "is_error": {
          "type": "boolean"
        },
        "taxonomy_class": {
          "type": "string",
          "enum": [
            "SUCCESS",
            "PARTIAL_SUCCESS",
            "SYNTACTIC_PARSE_FAIL",
            "STRUCTURAL_VIOLATION",
            "TYPE_MISMATCH",
            "OUT_OF_BOUNDS",
            "SEMANTIC_INVALIDITY",
            "PERMISSION_DENIED",
            "POLICY_VIOLATION",
            "STALE_STATE",
            "CONFIRMATION_MISSING",
            "BUDGET_EXHAUSTED",
            "RATE_LIMITED",
            "TIMEOUT",
            "IDEMPOTENCY_CONFLICT",
            "SIGNATURE_MISMATCH",
            "DEPENDENCY_UNAVAILABLE",
            "OBSERVATION_NORMALIZATION_FAIL",
            "COMPENSATION_REQUIRED",
            "COMPENSATION_FAILED",
            "UNKNOWN_ERROR"
          ]
        },
        "retryable": {
          "type": "boolean"
        },
        "repairable": {
          "type": "boolean"
        },
        "requires_approval": {
          "type": "boolean"
        },
        "fail_closed": {
          "type": "boolean"
        }
      }
    },
    "result_payload": {
      "type": "object",
      "required": [
        "data",
        "errors",
        "warnings"
      ],
      "additionalProperties": false,
      "properties": {
        "data": {
          "type": [
            "object",
            "null"
          ]
        },
        "errors": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "field",
              "message",
              "code"
            ],
            "additionalProperties": false,
            "properties": {
              "field": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "message": {
                "type": "string"
              },
              "code": {
                "type": "string"
              }
            }
          }
        },
        "warnings": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      }
    },
    "verification": {
      "type": "object",
      "required": [
        "post_action_verification_required",
        "target_state_reference",
        "expected_state",
        "delay_seconds"
      ],
      "additionalProperties": false,
      "properties": {
        "post_action_verification_required": {
          "type": "boolean"
        },
        "target_state_reference": {
          "type": [
            "string",
            "null"
          ]
        },
        "expected_state": {
          "type": [
            "object",
            "null"
          ]
        },
        "delay_seconds": {
          "type": "integer",
          "minimum": 0
        }
      }
    }
  }
}
```

### **Observation Quality Rules**

| Rule | Reason |
| :--- | :--- |
| **Return typed status, not prose.** | The agent should not infer success from vague text. |
| **Preserve raw payload out of context.** | Raw logs may contain secrets, stack traces, or prompt injection. |
| **Normalize errors.** | The orchestrator needs retry/repair/escalation semantics. |
| **Expose verification hints.** | Post-action verification needs target state and expected result. |
| **Include idempotency state.** | Retries need to know whether the result came from a replay. |
| **Include trace ID.** | Every observation must be replayable and auditable. |
| **Separate warnings from errors.** | Partial success and degraded results require clear handling. |

The model should receive the normalized observation object, not the raw provider response. Raw responses can be stored in secure trace storage and referenced by pointer when needed for debugging or audit.

## **Safe Execution Pattern Library**

To protect internal systems from model-driven execution risks, execution wrappers must implement strict, sandboxed isolation rules 2:

* **Database Execution Engine:** Raw SQL generated by models must never be executed directly against database engines.2 Wrappers must use parameterized queries, read/write separation, query allowlists, Row-Level Security (RLS) policies, and query result caps.17  
* **File System Wrapper:** File mutation tools must restrict paths using path normalization and chroot directory confinement to prevent directory traversal attacks.3 Sandboxes must use soft-delete policies rather than permanent removal commands.3  
* **Code Execution Sandboxes:** Dynamic code generated by models (e.g., Python scripts) must execute in isolated gRPC micro-containers with strict CPU, memory, and network limits.2  
* **Browser and Web Inspection Tools:** Web browsing tools must use a domain allowlist, enforce navigation boundaries, and block Server-Side Request Forgery (SSRF) paths.3 Form submissions must be gated behind confirmation controls.2  
* **Email and Communication Dispatchers:** Outbound communication systems must route drafts to a staging interface first.3 Real-time dispatches must run verification checks against the recipient database to prevent accidental data leakages.2

## **Tool Error Taxonomy**

When a tool execution fails, the wrapper must map the technical failure into a standardized taxonomy. The taxonomy tells the orchestrator whether the failure is repairable, retryable, approval-gated, fail-closed, or escalation-worthy.

| Technical Error Code | Systemic Definition | Repairable by Model? | Retryable Without Payload Change? | Approval Required? | Standard System Response |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **SYNTACTIC_PARSE_FAIL** | Generated output could not be parsed as valid JSON or tool-call structure. | Yes | No | No | Return parse error to repair loop. |
| **STRUCTURAL_VIOLATION** | Parsed object fails required schema shape or contains unexpected fields. | Yes | No | No | Return missing/extra field map to repair loop. |
| **TYPE_MISMATCH** | Parameter type differs from schema definition. | Yes | No | No | Return expected and actual types. |
| **OUT_OF_BOUNDS** | Value violates numeric, date, string, enum, or cardinality constraints. | Yes | No | No | Return explicit bounds or allowed values. |
| **SEMANTIC_INVALIDITY** | Arguments violate business logic or cross-field rules. | Sometimes | No | Maybe | Ask model to re-plan or ask user for missing/invalid domain facts. |
| **PERMISSION_DENIED** | User, tenant, or agent lacks required scope. | No | No | No | Block action; escalate if repeated, suspicious, or high-risk. |
| **POLICY_VIOLATION** | Action violates safety, compliance, data residency, or governance policy. | No | No | Sometimes | Fail closed or route to compliance review if exceptions are permitted. |
| **STALE_STATE** | Target resource changed since proposal was created. | Yes, after refresh | No | No | Refresh authoritative state and re-plan. |
| **CONFIRMATION_MISSING** | Required approval token is absent, invalid, expired, or mismatched. | No | No | Yes | Pause execution and render confirmation packet. |
| **BUDGET_EXHAUSTED** | Run exceeds token, tool, dollar, wall-clock, retry, or side-effect budget. | No | No | Budget expansion only | Terminate or request budget approval. |
| **RATE_LIMITED** | Tool endpoint or gateway limit is reached. | No | Yes, after delay | No | Back off with jitter or defer. |
| **TIMEOUT** | Tool did not complete inside timeout. | No | Yes, if idempotent | No | Retry with same idempotency key or escalate after retry cap. |
| **DEPENDENCY_UNAVAILABLE** | Required database, provider, queue, or service is down. | No | Yes, after recovery | No | Use fallback, degraded mode, or dependency-failure terminal state. |
| **IDEMPOTENCY_CONFLICT** | Same key is already in progress. | No | Yes, later | No | Return 409-style in-progress observation with retry-after. |
| **SIGNATURE_MISMATCH** | Same idempotency key was reused with different payload hash. | No | No | No | Reject as tampering or client bug; fail closed for mutation. |
| **OBSERVATION_NORMALIZATION_FAIL** | Tool returned data that wrapper could not normalize into observation schema. | No | Maybe | No | Preserve raw payload securely and route to repair or incident handling. |
| **COMPENSATION_REQUIRED** | A later step failed after earlier side effect committed. | No | No | Maybe | Execute compensation tool or route to operator. |
| **COMPENSATION_FAILED** | Attempted compensation failed or produced uncertain state. | No | Maybe | Yes | Escalate to operator and mark state as requiring reconciliation. |
| **UNKNOWN_ERROR** | Failure does not map to known taxonomy. | No | No by default | Maybe | Fail safe, preserve trace, and require classification before retry. |

### **Response Vector Model**

Every error should produce a response vector:

```json
{
  "error_code": "STALE_STATE",
  "repairable": true,
  "retryable": false,
  "requires_approval": false,
  "fail_closed": false,
  "escalate": false,
  "next_action": "refresh_state_and_replan"
}
```

The orchestrator should not decide from HTTP status alone. A `400` may be repairable, a `409` may be retryable later, a `403` may be ordinary denial or suspicious escalation, and a `200` may still contain a business-level failure. The taxonomy is the contract.

## **Tool Contract Failure Mode Map**

Tool integrations fail at the boundary between probabilistic proposals and deterministic systems. Robust contracts identify the failure mode, constrain the blast radius, and route the system toward repair, retry, compensation, escalation, or termination.

| Failure Mode | Direct Systemic Cause | Downstream Systemic Impact | Technical Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Silent Data Corruption** | Invalid parameter values bypass weak validation. | Corrupted records, bad workflow state, downstream crashes. | Enforce strict schemas, semantic checks, and post-action verification. |
| **Runaway Repair Loops** | Model repeatedly emits invalid arguments after validation errors. | Token spend spikes and tool execution stalls. | Cap repair attempts; fingerprint repeated invalid payloads. |
| **TOCTOU Race Condition** | Check-then-act spans separate cache and execution layers. | Duplicate side effects or inconsistent state. | Use durable idempotency records and transactionally coordinated state. |
| **Unsafe Retries** | Mutating endpoint retried without idempotency key. | Duplicate charges, duplicate emails, duplicate records. | Require idempotency keys for side-effecting operations. |
| **Prompt-Injection Tool Misuse** | Retrieved or user-provided text manipulates tool parameters. | Unauthorized data access, exfiltration, or mutation. | Treat tool arguments as proposals; enforce policy outside model context. |
| **Confirmation Theater** | Approval UI hides concrete payload and consequences. | Users approve actions they do not understand. | Show exact arguments, before/after state, risk class, and rejection path. |
| **Credential Exfiltration** | Secrets appear in prompts, tool args, traces, or model-visible errors. | Compromised systems and unauthorized access. | Keep credentials in wrapper/vault; scrub observations and errors. |
| **Schema Drift Outage** | Backend API changes without contract update. | Tool calls fail or observations become unparseable. | Version contracts and run integration tests before promotion. |
| **Stale Idempotency Record** | PENDING keys never resolve after worker crash. | Legitimate retries blocked indefinitely. | TTL, reconciliation worker, and explicit stuck-key recovery. |
| **Idempotency Payload Mismatch** | Same idempotency key reused with different payload. | Tampering risk or accidental cross-operation collision. | Bind key to payload hash and reject mismatches. |
| **Observation Schema Drift** | Tool output changes but normalizer remains old. | Agent receives missing, wrong, or ambiguous observations. | Version output schemas and test normalizers against provider changes. |
| **Overbroad Tool Affordance** | Tool description covers too many operations. | Model selects powerful tool for weakly related task. | Split tools by side-effect and use narrow descriptions. |
| **Approval Token Replay** | Approval token reused for different payload or after expiry. | Unauthorized delayed or altered action. | Bind approval token to payload hash, trace ID, approver, and expiry. |
| **Tool Output Injection** | Tool result contains instructions targeting the model. | Model follows malicious content from external source. | Mark tool outputs as data, sanitize, and isolate instructions from observations. |
| **Compensation Failure** | Reversal action fails after partial transaction. | System remains in uncertain or inconsistent state. | Escalate to operator, preserve saga journal, and require reconciliation. |
| **Error Taxonomy Collapse** | Wrapper returns generic failure text. | Orchestrator cannot choose retry, repair, fail-closed, or escalation. | Normalize every failure into standard response vector. |
| **Unbounded External Cost** | Tool invokes paid API without budget enforcement. | Denial-of-wallet and runaway vendor spend. | Apply per-tool, per-run, and per-tenant cost limits. |
| **Cross-Tenant Tool Confusion** | Arguments or credentials resolve against wrong tenant. | Data leak or unauthorized mutation. | Bind every call to tenant scope and enforce row-level permissions. |
| **Partial Success Ambiguity** | Multi-step tool returns success despite partial failure. | Agent updates state incorrectly. | Return `PARTIAL_SUCCESS` with completed steps, failed steps, and verification hints. |
| **Missing Post-Action Verification** | Tool response assumed to prove durable external state. | State drift between agent trace and real system. | Require verification handoff for mutating tools. |

Failure-mode doctrine:

```text
A tool contract is successful only when invalid proposals are blocked,
valid actions are executed safely, retries do not duplicate side effects,
and observations tell the agent what actually happened.
```

## **Evaluation and Observability Guidance**

Tool contract observability must show what the model proposed, what the wrapper accepted, what the system executed, what failed, what was repaired, what was retried, what required approval, and what observation returned to the agent loop.

### **Key Telemetry and SRE Metrics**

| Metric | Formula | What It Measures |
| :--- | :--- | :--- |
| **First-Pass Schema Valid Rate** | `schema_valid_first_try / total_tool_proposals` | How often the model produces structurally valid tool calls without repair. |
| **Schema-Valid Call Rate** | `schema_valid_calls / total_tool_proposals` | Overall structural adherence after repair attempts. |
| **Semantic-Valid Rate** | `semantic_valid_calls / schema_valid_calls` | How often structurally valid calls also satisfy business logic. |
| **Validation Failure Rate** | `rejected_execution_attempts / total_tool_proposals` | How much proposed tool traffic is blocked before execution. |
| **Repair Success Rate** | `successfully_repaired_calls / triggered_repair_loops` | Whether repair loops actually improve invalid payloads. |
| **Repair Saturation Rate** | `repair_loops_exhausted / triggered_repair_loops` | How often repair loops hit attempt limits. |
| **Unauthorized Proposal Rate** | `permission_denied_proposals / total_tool_proposals` | Whether models are attempting tools outside active scope. |
| **Policy Violation Rate** | `policy_violations / total_tool_proposals` | Frequency of compliance, safety, or governance blocks. |
| **Confirmation Required Rate** | `confirmation_required_calls / mutating_tool_proposals` | How often proposed actions require human approval. |
| **Confirmation Rejection Rate** | `rejected_confirmations / confirmation_requests` | Whether models are proposing actions humans decline. |
| **Confirmation Bypass Attempt Rate** | `missing_or_invalid_approval_tokens / confirmation_required_calls` | Whether execution attempts proceed without valid approval. |
| **Idempotency Replay Rate** | `idempotency_hits / mutating_requests` | Frequency of duplicate request handling. |
| **Idempotency Conflict Rate** | `signature_mismatches / idempotency_key_matches` | Payload mismatch or key misuse. |
| **Retry Success Rate** | `successful_retries / retry_attempts` | Whether transient failure handling is effective. |
| **Unsafe Retry Block Rate** | `blocked_non_idempotent_retries / retry_attempts` | Whether the wrapper blocks dangerous duplicate mutations. |
| **Observation Normalization Failure Rate** | `normalization_failures / tool_executions` | Whether tool outputs match expected observation contracts. |
| **Post-Action Verification Required Rate** | `verification_required_observations / mutating_observations` | How often mutating actions require AI-ENG-O handoff. |
| **Compensation Success Rate** | `successful_compensations / compensation_attempts` | Whether sagas can recover from partial failure. |
| **Tool Cost per Successful Action** | `tool_cost / successful_tool_executions` | Economic efficiency at the tool boundary. |
| **Trace Completeness Rate** | `complete_tool_traces / total_tool_calls` | Whether calls are replayable and auditable. |

### **OpenTelemetry and Context Propagation**

Every tool call should emit a span linked to the parent model/orchestrator span.

Required span attributes include:

| Attribute | Purpose |
| :--- | :--- |
| `gen_ai.operation.name` | Set to `execute_tool` or equivalent platform convention. |
| `tool.name` | Stable tool contract name. |
| `tool.version` | Tool contract version. |
| `tool.call_id` | Unique call ID. |
| `tool.side_effect_class` | Read-only, write, external, or critical mutation classification. |
| `tool.validation.status` | Passed, failed, repaired, blocked, approval pending. |
| `tool.error_code` | Standard taxonomy code when failure occurs. |
| `tool.retryable` | Whether retry is permitted. |
| `tool.repairable` | Whether model repair is permitted. |
| `tool.idempotency_key_hash` | Hashed idempotency key, never raw sensitive payload. |
| `tool.approval_id` | Approval reference when applicable. |
| `tenant.id` | Tenant scope. |
| `trace.replayable` | Whether required replay fields were captured. |

### **Tool Contract Evaluation Harness**

Tool contracts should be evaluated in CI and production replay.

| Test Mode | Purpose |
| :--- | :--- |
| **Schema Conformance Tests** | Verify generated tool calls satisfy strict schemas. |
| **Semantic Validation Tests** | Test business logic, cross-field constraints, and state rules. |
| **Permission Boundary Tests** | Confirm unauthorized users, tenants, and agents are blocked. |
| **Approval Gate Tests** | Verify high-risk actions require valid approval tokens. |
| **Idempotency Tests** | Confirm duplicate mutating requests do not duplicate side effects. |
| **Retry Tests** | Simulate timeouts, 503s, and network failures with safe retry behavior. |
| **Repair Loop Tests** | Ensure invalid payloads can be corrected within repair limits. |
| **Observation Contract Tests** | Validate wrapper outputs against observation schema. |
| **Compensation Tests** | Verify compensating actions work after partial failure. |
| **Prompt-Injection Tests** | Ensure tool outputs cannot smuggle instructions into the agent loop. |
| **Schema Drift Tests** | Compare backend API changes against tool contract compatibility. |
| **Replay Tests** | Reconstruct tool calls from trace and verify deterministic decisions. |

A tool contract is production-ready only when it can be validated, observed, replayed, and rolled back. Otherwise it is not a contract. It is a polite suggestion with a JSON hat.

## **Cross-Canon Handoff Map**

The tool contract boundary described in **AI-ENG-N** serves as a core integration point across the AI Engineering Systems Canon. It converts model-generated action proposals into validated, authorized, idempotent, observable execution events.

| Target Module | Canonical Focus | Operational Handoff Vector |
| :--- | :--- | :--- |
| **AI-ENG-M — Agentic Orchestration** | Loop control, budgets, state machines, termination, escalation. | Receives tool eligibility, validation results, repair-loop outcomes, approval state, and observation objects. |
| **AI-ENG-O — Post-Action Verification** | Verification after mutation. | Receives normalized observations, verification hints, idempotency keys, target state references, and expected state. |
| **AI-ENG-S — Production Pathologies** | Runtime and tool pathologies. | Uses error taxonomy, retry traces, validation failures, and runaway repair-loop metrics. |
| **AI-ENG-T — Security & Trust Boundaries** | Permissions, prompt injection, identity, and trust. | Consumes tool scopes, tenant boundaries, permission denials, and tool-output injection controls. |
| **AI-ENG-U — Tool Servers & Dependency Risk** | Remote tool infrastructure and dependency posture. | Receives transport requirements, server authorization rules, timeout policies, and dependency-failure taxonomy. |
| **AI-ENG-V — Resource Abuse & Cost Management** | Denial-of-wallet, quota, and abuse controls. | Uses per-tool budgets, repair-loop costs, rate limits, retry counts, and external API spend. |
| **AI-ENG-W — Fallback & Degraded Modes** | Fallback chains and degraded execution. | Uses retryable/non-retryable error classes, dependency unavailable states, and safe fallback eligibility. |
| **AI-ENG-X — User Control** | User-facing transparency and control. | Renders confirmation packets, concrete payload inspection, rejection paths, and approval consequences. |
| **AI-ENG-Y — Human-in-the-Loop** | Review and approval workflows. | Receives approval requests, escalation packets, risk class, payload hash, and review trace. |
| **AI-ENG-Z — Telemetry & Observability** | Metrics, traces, dashboards, and runtime visibility. | Receives OpenTelemetry spans, validation metrics, idempotency events, repair-loop outcomes, and tool latency. |
| **AI-ENG-AA — Agentic Evaluation & Benchmarking** | Evaluation of tool use and trajectories. | Uses schema-valid rates, semantic-valid rates, unauthorized proposal rates, repair success, and observation quality metrics. |
| **AI-ENG-AB — Auditability & Replay** | Replayable traces and evidence trails. | Stores tool proposals, validation decisions, approval tokens, idempotency keys, observations, and wrapper versions. |
| **AI-ENG-AC — Incident Response** | Operational response and rollback. | Coordinates incidents involving cascading tool failures, database locks, schema drift, compensation failure, and unsafe retries. |
| **AI-ENG-AD — Governance, Policy, Compliance & Accountability** | Approval policy, delegated authority, regulated actions, and accountability. | Owns tool approval policy, side-effect classes, exception records, contract lifecycle, audit boundaries, and accountable decision owners. |
| **AI-ENG-AJ — Agentic Reference Architectures** | Reference architecture patterns. | Implements wrapper templates, validation classes, transaction patterns, observation schemas, and tool-contract blueprints. |

The durable handoff is this:

```text
AI-ENG-N exports the deterministic execution contract:
what a model may propose, what the system will validate,
who may authorize it, how side effects are controlled,
how retries stay safe, and what observation returns to the loop.
```

## **Durable Principles of Tool Contract Architecture**

### **I. Probabilistic Proposals Require Deterministic Execution Boundaries**

Language models must never run unmediated database queries, shell scripts, system commands, payment operations, email sends, or infrastructure mutations. They may propose explicit, schema-bound actions. Deterministic wrappers decide whether those proposals are valid, authorized, affordable, approved, and safe to execute.

### **II. Structural Validation Must Occur Before Transactional Execution**

Parsing, schema validation, type checks, semantic validation, permission checks, policy checks, state checks, confirmation checks, and budget checks must complete before a mutating action starts. A model-generated payload becomes executable only after the wrapper accepts it.

### **III. Side Effects Must Be Classified Before They Are Allowed**

Read-only lookup, ephemeral write, internal mutation, external communication, financial transaction, and critical infrastructure change are not the same kind of action. Each side-effect class requires different approval, idempotency, transaction, verification, and audit controls.

### **IV. Mutations Require Idempotency Proportional to Risk**

Critical mutations require durable idempotency records, payload-hash binding, replay-safe responses, and transactionally coordinated local state. Lower-risk ephemeral writes may use lighter de-duplication patterns, but no mutating tool should be retried blindly.

### **V. Credentials Must Be Isolated from the Model Context**

API keys, OAuth tokens, database credentials, certificates, and service secrets must remain outside model prompts, tool arguments, and model-visible observations. The model provides symbolic identifiers. The wrapper resolves and injects scoped credentials only after authorization succeeds.

### **VI. Confirmation Gates Must Show Concrete Consequences**

Human approval is meaningful only when the reviewer sees the exact action, target, arguments, before/after state, risk class, idempotency fingerprint, expiry, and rejection path. Generic consent prompts are not security controls.

### **VII. Observations Must Be Structured, Typed, and Safe**

Tool outputs must return normalized observation objects rather than raw logs, stack traces, provider blobs, or ambiguous prose. The agent loop should reason from typed statuses, structured data, errors, warnings, and verification hints.

### **VIII. Retries, Repairs, Replans, and Escalations Are Different Control Paths**

Retries repeat the same payload after transient failure. Repairs create a corrected payload after validation failure. Replans alter the execution path after new state is observed. Escalations transfer authority to a safer actor or policy path. Mixing these paths creates duplicate mutations, runaway loops, and audit confusion.

### **IX. Post-Action Verification Completes the Contract**

A successful wrapper response does not always prove durable external state. Mutating tools should hand off target state, expected state, idempotency key, and trace metadata to post-action verification. AI-ENG-N governs execution contracts; AI-ENG-O verifies what actually changed.

### **X. Tool Contracts Are Release Artifacts**

Tool schemas, affordances, wrappers, observation objects, error taxonomies, and approval policies must be versioned, tested, promoted, and rolled back like production APIs. Silent drift at the tool boundary is a production incident waiting for a calendar invite.

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