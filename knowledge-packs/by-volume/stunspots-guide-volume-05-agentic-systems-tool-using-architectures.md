# [KNOWLEDGE] - AI Engineering - Volume 5. M-O Agentic Systems and Tool-Using Architectures

[Volume 5. M-O Agentic Systems and Tool-Using Architectures]
  - [AI-ENG-M — Agentic Orchestration - Autonomy Boundaries, Loop Budgets & Termination](#ai-eng-m--agentic-orchestration---autonomy-boundaries-loop-budgets--termination)  
  - [AI-ENG-N — Tool Contracts - Function Calling, Schemas, Validation & Idempotency](#ai-eng-n--tool-contracts---function-calling-schemas-validation--idempotency)  
  - [AI-ENG-O — Action Verification - Planning, Execution, Observation & Recovery](#ai-eng-o--action-verification---planning-execution-observation--recovery)  

---

# AI-ENG-M — Agentic Orchestration - Autonomy Boundaries, Loop Budgets & Termination

The transition of artificial intelligence from static generative engines to active organizational actors represents a major paradigm shift in software systems engineering.1 Traditional model serving operates on a request-response pattern characterized by isolated, stateless computations where the surrounding application logic maintains absolute control over execution paths.2 Conversely, agentic systems autonomously synthesize their execution paths at runtime, choosing tools, updating operational state, and modifying plans based on continuous environmental observations.2 This shift transfers system risk from single-call correctness to end-to-end behavioral predictability across temporal, resource-constrained, and non-stationary environments.2  

Agentic orchestration is the bounded control discipline that governs this dynamic behavior.6 Autonomy is not an abstract behavior or a design-free capability; it is a strictly regulated architectural lease of decision rights granted to a model under precise runtime constraints.2 An agent loop can only operate safely within production systems when its execution boundary is explicitly mapped, its resource consumption is metered, and its termination and escalation parameters are deterministically enforced.2  

This report establishes the first-class architectural, operational, and Site Reliability Engineering (SRE) patterns required to design, deploy, and govern high-dimensional agentic loops. It serves as the opening document of **Volume 5: Agentic Systems and Tool-Using Architectures**, inheriting state governance principles from AI-ENG-B, economic cost-attribution structures from AI-ENG-C, artifact lifecycles from AI-ENG-I, and high-availability serving architectures from AI-ENG-L.4

## **Conceptual Glossary**

To establish a uniform vocabulary for system engineers, platform teams, and governance operators, the following definitions represent the core structural components of agentic orchestration:

| Term | Definition |
| :---- | :---- |
| **Agentic Orchestration** | The control plane layer that coordinates multi-step planning, state transitions, tool dispatch, and runtime safety boundaries across autonomous and semi-autonomous systems.6 |
| **Agent Loop** | The continuous cycle of goal formulation, state evaluation, plan synthesis, action execution, and observation ingestion that continues until a valid stopping criterion is reached.4 |
| **Autonomy Boundary** | The explicit limit of allowed observations, actions, writes, and memory access granted to a model during a specific execution task.2 |
| **Decision Right** | A discrete execution privilege leased to a model (such as plan modification or parameter selection) within an autonomy boundary.6 |
| **Loop Budget** | The total allocated resource allowance (computational, monetary, temporal, and depth) that an agent loop is permitted to consume before termination.5 |
| **Tool Budget** | The maximum allowed count, rate, and monetary cost of external tool invocations allocated to a single workflow run.8 |
| **Termination Condition** | A deterministic or highly validated programmatic rule that immediately halts the agent loop and returns execution control to the parent system.7 |
| **Escalation Trigger** | A policy-defined threshold that pauses autonomous execution and routes the current state to a human supervisor, a safer model, or a fail-closed routine.5 |
| **Scratchpad** | An ephemeral context window space used by a model to perform step-by-step deliberative expansion and working calculations, isolated from the final output state.7 |
| **Task State** | The structured, machine-readable record of goals, accomplished actions, remaining subtasks, active budgets, and validation outputs during an agent run.4 |
| **Planner-Executor** | An architectural pattern that splits cognitive deliberation into a highly capable planning model and a fast, specialized executing model to optimize performance and cost.12 |
| **Reflection** | A triggered diagnostic loop where a model critically evaluates its own intermediate outputs or tool errors against acceptance criteria to execute corrective action.7 |
| **Workflow Graph** | A directed graph structure defining the permissible states (nodes) and transition paths (edges) of a multi-agent system, governed by deterministic guards.13 |
| **State Machine** | A mathematical and software model of execution consisting of a finite number of states, transitions, and actions, providing a rigid skeleton for agentic autonomy.6 |
| **Action** | An external mutation or retrieval request issued by the model through a tool interface to change or observe environment state.2 |
| **Observation** | The structured response returned by the environment or a tool back to the agent loop, serving as the ground-truth foundation for subsequent state updates.4 |
| **Guard** | A deterministic pre-condition or post-condition that validates state transitions or tool arguments before allowing execution to proceed in a workflow graph.13 |
| **Terminal Node** | A designated final state in a workflow graph representing successful completion, explicit failure, budget exhaustion, or escalation.13 |
| **Agent Run Trace** | An immutable, chronological audit log capturing the complete execution trajectory, including prompt parameters, tool calls, observations, state updates, and budget consumption.1 |

## **Doctrinal Foundation and Canon Continuity**

The governance of autonomous models requires translating abstract capabilities into concrete systems constraints.2 This report operates under the core systems engineering principle:  

Autonomy must be bounded by state, scope, tools, budgets, checks, and termination. An agent loop is safe only when the system knows what it is trying to accomplish, what it may observe, what it may change, how much it may spend, when it must stop, and when it must escalate.2  

The central architecture question shifts from *"How do we make the system agentic?"* to *"What decisions may the model make, under which constraints, for how many steps, using which tools, with what state, until which stopping condition, and under what review or escalation policy?"*.2  

This document builds directly upon the foundational engineering practices established in previous volumes of the Canon:

* **AI-ENG-B (Context State Governance):** Inherits the context-tenure doctrine, establishing that agent context is a governed resource with explicit tenure, authority, and temporal validity.4  
* **AI-ENG-C (Inference Economics):** Adapts cost-per-success and budget attribution models to prevent unconstrained token consumption.8  
* **AI-ENG-I (Artifact Lifecycle & Regression Control):** Inherits release-manifest and replayable-trace doctrines to guarantee deterministic execution reconstruction.1  
* **AI-ENG-L (Serving-Layer Runtimes):** Integrates gateway-level rate limits, tenant quotas, and backpressure halts to terminate runaway behaviors at the infrastructure level.18

To preserve clear architectural handoffs, AI-ENG-M establishes the top-level orchestration and control structure that decides *what happens next* across the loop.6 It maps transition paths and boundary states, leaving the deep operational schemas of tool validation to **AI-ENG-N (Tool Contracts)**, and the mechanisms of state reconciliation to **AI-ENG-O (Action Verification)**.4

## **The Least-Autonomy Decision Ladder**

Unnecessary system autonomy is a critical driver of latency, financial waste, and operational failure.3 Programmers often default to highly flexible autonomous loops when deterministic, static paths can resolve the objective.3 The Least-Autonomy Decision Ladder acts as a structural filter, forcing designers to validate why simpler, more predictable architectures are insufficient before advancing to higher tiers of autonomy.

| Tier | Architecture | Control Plane | Execution Path | Operational Cost | Failure Surface | Ideal Use Case |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **1** | **Direct Response** | Programmatic Host Application | Fully static; zero runtime path variance.3 | Low; single-call token fee.20 | Model Hallucination; output schema non-compliance. | Content summarization; static text classification.11 |
| **2** | **Prompt Chain** | Programmatic Host Application | Linear sequential execution of static steps. | Low to Moderate; predictable linear scaling.21 | Cumulative context drift; step dependency failure.2 | Standard multi-step data extraction and formatting. |
| **3** | **Deterministic Workflow** | Workflow Orchestration Engine | Predefined branching paths mapped by hard logic.3 | Moderate; dependent on active execution branch. | Unhandled edge-case inputs; routing logic gaps.3 | Standard transactional data entry; rigid customer routing. |
| **4** | **Conditional Graph** | Graph Compilation Engine 13 | Edge-gated paths based on structured outputs.13 | Moderate; controlled loop bounds.13 | State validation failure; circular transition loops.13 | Legal precedent search and document verification.22 |
| **5** | **Supervised Agentic Loop** | Human-in-the-Loop Orchestrator 6 | Dynamic plan generation requiring human approval.1 | High; bounded by human review latency. | Human operator fatigue; incomplete plan execution.1 | Code generation with active human test approval.1 |
| **6** | **Bounded Agentic Loop** | Runtime Budget Controller 2 | Dynamic tool selection within strict budget envelopes.5 | High; managed by proactive termination.8 | Runaway loops; tool spam; budget blowout.8 | Production incident diagnostic and local log triage.5 |
| **7** | **Open-Ended Loop** | Unbounded Model Deliberation | Fully dynamic exploration and mutation.4 | Extremely High; unconstrained risk profile.8 | Total resource exhaustion; data corruption.3 | Unconstrained sandbox research exploration. |

## **The Autonomy Boundary Model**

The Autonomy Boundary Model defines the operational limits of what a model may observe, decide, execute, or change at runtime. Autonomy is not a personality trait or a product aesthetic. It is a scoped lease of decision rights granted by the surrounding system under explicit policy, budget, identity, tool, and termination constraints.

A safe autonomy boundary separates five layers of authority:

```text
+--------------------------------------------------------------------------------
| AUTONOMY BOUNDARY MODEL
+--------------------------------------------------------------------------------
|
| Goal:
|   Grant the model only the minimum decision rights needed for the task,
|   while deterministic systems, humans, auditors, and infrastructure retain
|   final authority over safety, permissions, cost, and side effects.
|
|   Layer 5: Infrastructure Enforcement
|     network sandbox | IAM | container limits | gateway rate limits
|     wall-clock timeout | filesystem sandbox | egress control
|
|   Layer 4: Audit and Review Layer
|     asynchronous human audit | compliance review | trace inspection
|     incident review | exception records
|
|   Layer 3: Human Approval Gates
|     explicit approval for writes | budget release | external communication
|     purchase approval | infrastructure mutation approval
|
|   Layer 2: Deterministic Policy Layer
|     schema validators | ACLs | quota checks | tool registry
|     state guards | output validators | action verifiers
|
|   Layer 1: Model Discretion Layer
|     propose plan | choose next allowed step | draft tool arguments
|     summarize observations | request clarification | recommend escalation
|
+--------------------------------------------------------------------------------
| Rule:
|   The model may propose. The orchestrator authorizes. Tools execute.
|   Validators verify. Humans approve high-impact actions. Infrastructure enforces.
+--------------------------------------------------------------------------------
```

The autonomy boundary is configured through dimensions that describe what the agent may observe, decide, invoke, write, spend, and escalate.

| Boundary Dimension | Model Deliberative Authority | Deterministic System Policy | User / Human Approval Policy | Serving / Infrastructure Enforcement |
| :--- | :--- | :--- | :--- | :--- |
| **Allowed Observations** | Select among authorized read-only sources, request more context, or choose which local index to query. | Filter retrieved context by permission, tenure, freshness, PII policy, and source authority. | Confirm expansion into broader or more sensitive search scopes. | Restrict network access, API credentials, document stores, and retrieval endpoints by active identity. |
| **Allowed State Writes** | Propose updates to task state, scratchpad summaries, or candidate memory entries. | Reject writes that violate schema, authority, tenure, or observation-grounding rules. | Approve persistent memory writes, external communications, file modifications, or workflow checkpoints when required. | Enforce sandbox boundaries, transactional writes, WORM audit logs, and least-privilege database credentials. |
| **Allowed Tools** | Propose tool calls from the active tool registry and draft candidate arguments. | Match proposed tool against allowlist, schema, budget, risk class, and user permission. | Approve write-capable, financial, external communication, or infrastructure-changing tools. | Deny unauthorized credentials, block network egress, apply rate limits, and terminate unapproved calls. |
| **Decision Rights** | Select the next allowed plan step, request repair, choose among permitted tools, or ask for clarification. | Constrain all transitions to compiled graph edges and deterministic guards. | Approve plan changes in high-risk or ambiguous workflows. | Prevent runtime graph mutation, privilege escalation, and unauthorized transition injection. |
| **Resource Budgets** | Estimate remaining work and recommend continuation, compression, or escalation. | Track tokens, tool calls, wall-clock, cost, retries, context growth, and external API usage. | Approve budget increases for long-running or high-value tasks. | Enforce gateway quotas, container timeouts, billing limits, and circuit breakers. |
| **Escalation Paths** | Flag low confidence, conflict, missing information, or unsafe uncertainty. | Halt execution on validation failure, source conflict, permission failure, or budget breach. | Review, edit, approve, reject, or terminate the proposed continuation. | Page SRE, Security, Compliance, or workflow owner depending on severity. |

### **Context and Boundary Adjustments by Risk Profile**

Autonomy boundaries are calibrated by task risk, data sensitivity, side-effect severity, and user oversight. A system should not grant the same discretion to a research assistant summarizing public documents and an SRE agent proposing production infrastructure changes.

| Risk Profile | Default Autonomy | Allowed Without Human Approval | Requires Human Approval | Fail-Closed Conditions |
| :--- | :--- | :--- | :--- | :--- |
| **Research Assistant** | High read-only autonomy | Browse approved sources, retrieve documents, summarize, compare, cite, ask clarifying questions. | Persistent memory writes, sensitive-source expansion, or paid external lookups. | Unauthorized data access, ungrounded claims presented as verified, or source-policy conflict. |
| **Coding Assistant** | Bounded workspace autonomy | Read files, propose patches, run tests in sandbox, write local draft files when permitted. | Pushes, deployments, credential changes, destructive file operations, production migrations. | Secret exposure, failing tests after mutation, unauthorized repo access, or production-impacting command. |
| **Customer Support Agent** | Bounded action autonomy | Search documentation, draft replies, classify issues, summarize account state. | Refunds, cancellations, account changes, legal commitments, external communication send actions. | Identity mismatch, payment change, policy exception, or regulated support category. |
| **Procurement Agent** | Low mutation autonomy | Compare vendors, extract terms, flag risks, prepare recommendation packets. | Purchase orders, invoice release, contract changes, vendor onboarding, payment execution. | Budget breach, missing approval chain, vendor risk conflict, or contract ambiguity. |
| **Incident Remediation SRE Agent** | High diagnostic / low mutation autonomy | Query logs, inspect metrics, compare deploys, draft runbook steps, recommend rollback. | Restarts, traffic shifts, config changes, data migrations, production rollback. | Security incident, destructive action, uncertain blast radius, or missing operator approval. |

The autonomy boundary should be stored as part of the run manifest. Every tool call, state transition, memory write, and escalation decision must be evaluated against the active boundary version that authorized the run.

## **The Agent Loop Lifecycle**

The life cycle of an orchestrator run is structured around an iterative control loop designed to enforce validation, grounding, and budget controls at every step.4

| Phase | Operational Objective | Input Dependencies | Grounding & Safety Validation | Downstream Handoff |
| :---- | :---- | :---- | :---- | :---- |
| **1. Goal Intake** | Capture user prompt and validate intent.4 | User query string; request headers. | Reject injections and evaluate semantic compliance. | Target goal object.4 |
| **2. Scope Resolution** | Resolve user role, tenant permissions, and limits.1 | Tenant directory; IAM directory. | Restrict access bounds to active user credentials.1 | Session Security Token. |
| **3. Policy Check** | Verify goal alignment with organizational safety policies. | Global enterprise compliance rules.12 | Reject requests violating active safety constraints. | Safety Clear Code. |
| **4. State Init** | Instantiate task state tracking objects.4 | Goal object; Security Token. | Initialize tracking tables using immutable data patterns.13 | Active Task State.13 |
| **5. Plan Selection** | Generate initial execution plan steps.4 | Goal; allowed tool list. | Validate plan steps against allowed capabilities.6 | Initial Executable Plan.1 |
| **6. Budget Assign** | Allocate loop, token, and monetary cost budgets.8 | Tenant quotas; task risk profile. | Map deterministic ceilings to the task run.7 | Budget Tracking Token.8 |
| **7. Tool Eligibility** | Verify tool registration and active user permissions.6 | Tool registry; active Plan Step. | Intercept and block unauthorized tool calls.11 | Authorized Tool List.11 |
| **8. Action Selection** | Select tool and populate parameter arguments.4 | Plan step; context history.4 | Match generated arguments against target tool schemas.5 | Proposed Action Event.5 |
| **9. Tool Invocation** | Execute the tool call and capture raw payload.4 | Proposed Action; system secrets.11 | Enforce execution timeouts and catch runtime errors.4 | Raw Tool Output Payload.5 |
| **10. Observation Grounding** | Ingest tool outputs and verify factual grounding.4 | Raw Tool Payload; system state. | Reject model-hallucinated or assumed tool outcomes.5 | Grounded Observation.5 |
| **11. State Update** | Commit observation to the task state.4 | Grounded Observation; Active State.13 | Commit updates to the versioned state database.13 | Updated Task State.13 |
| **12. Validation Check** | Evaluate step quality against acceptance criteria.5 | Updated Task State; criteria list.7 | Score outputs for compliance and semantic accuracy.5 | Validation Score Card.5 |
| **13. Reflection Trigger** | Diagnose failures and trigger corrections if needed.7 | Validation Score Card; state logs. | Limit repair attempts to prevent looping.7 | Diagnostic Repair Plan.7 |
| **14. Termination Check** | Evaluate stopping and completion criteria.7 | Active Task State; Budget Status.8 | Match progress against termination conditions.7 | Stop / Continue Code. |
| **15. Escalation Check** | Pause loop and escalate to supervisor if triggered.5 | Validation failures; budget alarms. | Verify if state warrants immediate human review.5 | Escalation Event Payload.6 |
| **16. Final Response** | Compile, format, and return final output. | Validated Task State; target goal. | Ensure output is grounded in recorded observations.1 | Verified User Response.1 |
| **17. Memory Write** | Update long-term memory via write gates.3 | Run Trace; current Memory DB.1 | Filter out transient failures and assumptions.3 | Promoted Memory Block.3 |
| **18. Trace Logging** | Archive the complete execution trace for audits.1 | Versioned Task State; metric logs. | Write immutable execution records to disk.1 | Audit Trace Archive.1 |

### **Observation Grounding: Intended, Attempted, Observed, Verified, and Assumed Results**

Observation grounding prevents an agent from contaminating task state with actions it merely intended, attempted, or imagined. The orchestrator must distinguish between the model's proposed action, the system's attempted dispatch, the tool's observed response, the validator's verified result, and the committed state update.

```text
+--------------------------------------------------------------------------------
| OBSERVATION GROUNDING LADDER
+--------------------------------------------------------------------------------
|
| 1. Intended
|      The model proposes an action.
|      Example: "Create a database record."
|
| 2. Attempted
|      The orchestrator sends a tool call to the target system.
|      Example: POST /records with validated parameters.
|
| 3. Observed
|      The tool returns raw bytes, an error, a timeout, or an empty response.
|      Example: HTTP 201, HTTP 403, timeout, malformed payload.
|
| 4. Verified
|      The system validates the returned payload against expected state.
|      Example: record exists, ID matches, write confirmed, checksum valid.
|
| 5. Committed
|      The orchestrator updates task state with the verified result.
|      Example: TaskState.records_created += confirmed_record_id.
|
| Prohibited State:
|
|   Assumed
|      The model claims the action succeeded without verified evidence.
|      This state must never be committed.
|
+--------------------------------------------------------------------------------
| Rule:
|   State may be updated only from verified observations, never from intention,
|   attempted dispatch, model confidence, or plausible narrative completion.
+--------------------------------------------------------------------------------
```

| State Label | Meaning | May Update Task State? | Required Handling |
| :--- | :--- | :---: | :--- |
| **Intended** | The model proposes an action or predicts that an action would help. | No | Validate against plan, policy, budget, and tool eligibility. |
| **Attempted** | The orchestrator dispatched the tool call. | No | Record attempt metadata, request ID, tool name, arguments hash, and timeout policy. |
| **Observed** | The tool returned a raw response, error, timeout, or exception. | No | Parse, normalize, and preserve the raw observation. Do not summarize away failure details. |
| **Verified** | The system confirmed the observation satisfies the expected result. | Yes, after validation | Commit only the verified facts, IDs, outputs, or error states. |
| **Committed** | The verified result has been written to structured task state. | Already committed | Include version, timestamp, source, validator ID, and trace pointer. |
| **Assumed** | The model infers success without tool evidence. | Never | Halt, repair, or escalate depending on risk. Treat as a grounding violation. |

Grounding applies equally to successful and failed actions. A timeout, permission denial, malformed response, or empty payload is still a real observation. The task state should record that failure exactly rather than allowing the model to convert it into an implied success.

A safe observation update follows this sequence:

```text
[ Model proposes action ]
      |
      v
[ Orchestrator validates action ]
      |
      v
[ Tool call dispatched ]
      |
      v
[ Raw response captured ]
      |
      v
[ Response parsed and verified ]
      |
      +--> verification failed -> record failure; repair, retry, or escalate
      |
      v
[ Verified result committed to task state ]
```

The model may reason over verified observations. It may not promote intended, attempted, or assumed outcomes into state. This is the core guardrail preventing hallucinated tool success, phantom database writes, imaginary emails, fake deployments, and other delightful little audit-nightmares wearing a trench coat.

## **Planning and Decomposition Framework**

Dynamic planning is valuable for complex tasks with many dependencies but can introduce unnecessary latency and high token costs when applied to simple tasks.12 Orchestrators must structure planning as a managed process with distinct checkpoints:

| Lifecycle Phase | Core Architectural Function | System Boundary Check | Failure Mode Mitigation |
| :---- | :---- | :---- | :---- |
| **Plan Generation** | Outline execution steps sequentially.12 | Ensure all proposed tools are active and licensed.11 | Use highly capable models to avoid logical gaps.6 |
| **Plan Validation** | Verify steps against access scopes and budgets.6 | Check validation rules before dispatching tools.5 | Reject plan if step cost projections exceed limits.8 |
| **Plan Execution** | Run tools sequentially as outlined.4 | Verify user session permissions for every tool call.6 | Cache progress at each step to support state recovery.14 |
| **Plan Repair** | Adjust plan steps when tools return exceptions.5 | Limit plan revisions to prevent infinite loops.5 | Inject precise error messages into the planning prompt.5 |
| **Plan Abandonment** | Halt execution if assumptions are invalidated.7 | Compare task cost to remaining budget.8 | Stop execution and prompt the user for clarification.7 |
| **Subtask Creation** | Break complex goals into smaller subtasks.4 | Track subtask dependencies using DAG structures.4 | Require clear completion criteria for every subtask.7 |
| **Dependency Tracking** | Track prerequisite steps and outputs.4 | Block child steps from running until parents finish.4 | Keep a dependency map in the structured task state.4 |
| **Recursive Depth** | Limit nested child task creation.18 | Restrict the depth of subtasks (D <= 2).18 | Reject subtask creation if depth limits are violated.18 |

## **The Reflection Trigger Model**

Reflection must be used as a targeted diagnostic tool, not an automatic step appended to every turn.7 Unchecked reflection leads to "navel-gazing" loops where the model consumes high volumes of tokens to critique minor syntactic points without improving output correctness.7

| Target Diagnostic Condition | Trigger Source | Key Operational Question | Bounding & Limits | Over-Reflection Mitigation |
| :---- | :---- | :---- | :---- | :---- |
| **Tool Invocation Failure** | Tool exception; malformed arguments.5 | *Which specific parameter violated the schema?* 5 | Maximum of 2 consecutive repair attempts.7 | Inject compiler or schema errors directly into the context.5 |
| **Low Confidence Validation** | Output scorer drops below threshold.5 | *Which acceptance criteria was not satisfied?* 7 | Max 1 reflection pass before escalation.7 | Freeze state; route to user with clear error highlighting.7 |
| **Conflict Detection** | Divergent tool outcomes; contradictory sources.5 | *Which source has the higher credential authority?* | Max 1 cross-check pass.5 | Halt execution and flag the conflict to the operator.5 |
| **Context Bloat Warning** | Ingested history exceeds 80% limit.7 | *What is the minimal summary of our progress?* 7 | Run 1 context compression pass.7 | Discard historical details and retain only active state.7 |
| **Plan Divergence** | Action outcomes contradict task goal.5 | *Does the active plan still lead to the user's goal?* | No auto-reflection; force escalation.7 | Stop execution and display the drift log to the user.1 |

## **Agentic Workflow Graph Model**

Open-ended loops are powerful but introduce high variance and unconstrained execution risk. Enterprise agentic systems should run inside compiled workflow graphs or state machines where transitions are explicit, guarded, logged, and recoverable.

A workflow graph does not eliminate model reasoning. It constrains where that reasoning may occur and which transitions are legal after each state.

```text
+--------------------------------------------------------------------------------
| AGENTIC WORKFLOW GRAPH MODEL
+--------------------------------------------------------------------------------
|
| [ Goal Intake ]
|      |
|      v
| [ Classify Intent ]
|      |
|      v
| [ Resolve Scope / Identity / Policy ]
|      |
|      v
| [ Initialize Task State and Budgets ]
|      |
|      v
| [ Plan Node ]
|      |
|      v
| [ Plan Validation ]
|      |
|      +--> invalid / unsafe / over budget -> [ Escalate or Terminate ]
|      |
|      v
| [ Action Selection ]
|      |
|      v
| [ Tool Eligibility and Argument Validation ]
|      |
|      +--> missing info ---------------------> [ Ask User ]
|      |                                           |
|      |                                           v
|      |                                      [ Resume Scope or Plan ]
|      |
|      +--> unauthorized / high risk ---------> [ Human Approval / Escalation ]
|      |                                           |
|      |                                           +--> approved -> [ Action Selection ]
|      |                                           |
|      |                                           +--> rejected -> [ Terminate ]
|      |
|      v
| [ Tool Invocation ]
|      |
|      v
| [ Observation Capture ]
|      |
|      v
| [ Observation Verification ]
|      |
|      +--> verification failed -> [ Repair Node ]
|      |                              |
|      |                              +--> repair budget available -> [ Plan Node ]
|      |                              |
|      |                              +--> repair budget exhausted -> [ Escalate or Terminate ]
|      |
|      v
| [ State Update ]
|      |
|      v
| [ Completion / Termination Check ]
|      |
|      +--> incomplete and budget available -> [ Plan Node ]
|      |
|      +--> source conflict / low confidence -> [ Escalate ]
|      |
|      +--> complete -------------------------> [ Finalize ]
|                                                |
|                                                v
|                                           [ Terminate ]
|
+--------------------------------------------------------------------------------
| Rule:
|   Successful completion routes to Finalize, not Escalate. Escalation is an
|   abnormal or review-required path, not the normal completion path.
+--------------------------------------------------------------------------------
```

The graph components are defined as follows:

| Graph Component | System Role | Allowable Transitions | Guard Conditions |
| :--- | :--- | :--- | :--- |
| **Goal Intake** | Capture user request and convert it into a structured goal object. | Goal Intake -> Classify Intent | Request must be parseable and within platform policy. |
| **Classify Intent** | Determine task class, risk level, modality, and required capability. | Classify -> Scope | Classification must meet confidence threshold or route to clarification. |
| **Scope Resolution** | Resolve tenant, identity, permissions, data boundaries, and allowed tools. | Scope -> State Init; Scope -> Terminate | Active user/session token must be valid. |
| **State Init** | Create task state, trace envelope, budget vector, and autonomy boundary. | State Init -> Plan | Run manifest must be created successfully. |
| **Plan Node** | Draft or update an execution plan. | Plan -> Plan Validation | Plan must be represented as structured steps, not freeform intention. |
| **Plan Validation** | Validate plan against scope, budgets, tools, policy, and risk. | Plan Validation -> Action Selection; Escalate; Terminate | Every step must map to an allowed graph edge and budget envelope. |
| **Action Selection** | Select the next action from the validated plan. | Action Selection -> Tool Eligibility | Selected action must match current state and step dependency rules. |
| **Tool Eligibility** | Confirm tool registration, permission, schema, cost, and side-effect class. | Tool Eligibility -> Tool Invocation; Ask User; Human Approval; Terminate | Tool must be allowed for the active user, tenant, and autonomy boundary. |
| **Tool Invocation** | Execute the approved tool call. | Tool Invocation -> Observation Capture | Tool execution must occur through the orchestrator, never directly from the model. |
| **Observation Capture** | Preserve raw tool output, error, timeout, or exception. | Observation Capture -> Observation Verification | Raw observation must be stored before interpretation. |
| **Observation Verification** | Validate returned payload against expected state and acceptance criteria. | Verify -> State Update; Repair; Escalate; Terminate | Only verified observations may update task state. |
| **Repair Node** | Attempt bounded correction after validation or tool failure. | Repair -> Plan; Escalate; Terminate | Repair count, retry count, and budget must remain within limits. |
| **Ask User** | Request missing information or disambiguation. | Ask User -> Scope; Ask User -> Plan; Ask User -> Terminate | Execution must suspend until user input is received. |
| **Human Approval / Escalation** | Route state to a human, safer model, or review queue. | Escalate -> Action Selection; Escalate -> Finalize; Escalate -> Terminate | Approval or rejection must be recorded in the trace. |
| **State Update** | Commit verified observation to versioned task state. | State Update -> Completion Check | Update must be transactional and trace-linked. |
| **Completion / Termination Check** | Determine whether the goal is complete, incomplete, blocked, or unsafe. | Check -> Plan; Finalize; Escalate; Terminate | Stopping condition must be explicit and externally verifiable. |
| **Finalize** | Compile validated output from task state and verified observations. | Finalize -> Terminate | Final response must be grounded in committed state. |
| **Terminate** | Close run, release resources, write trace, and emit terminal status. | End of run | Resources must be released and terminal code recorded. |

### **Graph Compilation and State Transitions**

Before execution, the graph layout is compiled and validated. The compiled graph becomes immutable for the duration of the run. The model may choose among allowed transitions but may not inject new states, rewrite guard logic, bypass validators, or mutate terminal behavior at runtime.

```text
+--------------------------------------------------------------------------------
| GRAPH COMPILATION CHECKPOINT
+--------------------------------------------------------------------------------
|
| 1. Verify every node has at least one legal outgoing transition,
|    except terminal nodes.
|
| 2. Verify every non-terminal cycle has an exit condition,
|    retry limit, budget limit, or escalation path.
|
| 3. Verify every edge has a deterministic guard.
|
| 4. Verify every tool-capable node has permission, schema,
|    budget, timeout, and observation-verification rules.
|
| 5. Verify every halt, failure, budget-exhaustion, and escalation
|    path can reach a terminal node.
|
| 6. Verify every terminal node writes trace, status, resource-release,
|    and final-state metadata.
|
+--------------------------------------------------------------------------------
| Compilation rule:
|   A workflow graph is valid only when success, failure, cancellation,
|   escalation, and budget exhaustion all have explicit terminal paths.
+--------------------------------------------------------------------------------
```

At each transition, the orchestrator writes a versioned checkpoint to durable state storage. This allows interrupted workflows to resume safely, replay deterministically, or terminate with a complete trace rather than leaving a half-imagined ghost-process rattling chains in the scheduler.

Several common multi-agent topology patterns have distinct operational characteristics:

| Pattern | Structure | Strength | Primary Risk | Best Fit |
| :--- | :--- | :--- | :--- | :--- |
| **Planner-Executor** | A stronger planner drafts steps; a cheaper executor performs bounded actions. | Reduces cost and isolates planning from execution. | Plan drift if environment changes mid-run. | Code tasks, research plans, controlled tool workflows. |
| **Supervisor-Worker** | A central supervisor routes tasks to specialized workers. | Restricts worker blast radius and tool access. | Supervisor bottleneck or routing failure. | Enterprise agents with domain-specific subagents. |
| **State-Machine Agent** | Model decisions occur only inside predefined graph nodes. | Strongest operational predictability. | Less flexible under novel tasks. | Production workflows with compliance or reliability constraints. |
| **Blackboard / Shared Workspace** | Agents coordinate through shared task state. | Good for parallel subproblem solving. | State corruption, duplicate work, coordination loops. | Research or sandboxed exploration with strong budgets. |
| **Specialist Swarm** | Peer agents communicate or spawn subtasks. | Maximum flexibility and coverage. | Runaway token spend, inconsistent state, circular delegation. | Experimental environments, not default production paths. |

The least-autonomous graph that can complete the task should be preferred. A static workflow graph with two guarded model calls is usually superior to a majestic swarm of tiny bureaucrats burning tokens in a circle.

## **Loop Budgets, Tool Budgets, and SRE Timeout Policies**

Operational budget enforcement is the primary defense against runaway agentic behavior. A loop budget is not a post-hoc billing report. It is an active control signal that determines whether an agent may continue, compress context, downgrade capability, request approval, or terminate.

Agentic budgets must be modeled as a vector, not a single scalar, because tokens, tool calls, dollars, wall-clock time, retries, memory writes, and side effects are different resource dimensions. They can be converted into dollars for cost accounting, but the orchestrator should still enforce each dimension independently.

Let an agent run `R` be assigned a budget vector:

```text
B = {
  input_tokens_max,
  output_tokens_max,
  tool_calls_max,
  external_api_calls_max,
  wall_clock_seconds_max,
  inference_cost_usd_max,
  retry_count_max,
  reflection_passes_max,
  context_tokens_max,
  memory_writes_max,
  side_effects_max
}
```

At step `t`, the orchestrator tracks observed consumption:

```text
U_t = {
  input_tokens_used,
  output_tokens_used,
  tool_calls_used,
  external_api_calls_used,
  wall_clock_seconds_used,
  inference_cost_usd_used,
  retry_count_used,
  reflection_passes_used,
  context_tokens_used,
  memory_writes_used,
  side_effects_used
}
```

Remaining budget is computed per dimension:

```text
R_t[d] = B[d] - U_t[d]
```

The run must halt, degrade, compress, or escalate when any required budget dimension is exhausted:

```text
TerminateOrEscalate if exists d such that R_t[d] <= 0
```

### **Scalar Cost Accounting**

For business reporting, budget dimensions may also be scalarized into a common monetary cost estimate:

```text
Cost_t =
  input_tokens_used  * price_per_input_token
+ output_tokens_used * price_per_output_token
+ tool_calls_used    * price_per_tool_call
+ api_calls_used     * price_per_external_api_call
+ gpu_seconds_used   * price_per_gpu_second
+ human_review_used  * price_per_review_unit
```

Scalar cost is useful for billing and cost-per-success analysis. It must not replace independent hard caps on safety-critical dimensions such as side effects, memory writes, wall-clock time, or privileged tool calls.

### **Progressive Interval Estimation**

To prevent agents from spending resources on hopeless trajectories, the orchestrator should run a Progressive Interval Estimation protocol. At each step, the system estimates the remaining resource cost required to complete the task.

```text
PIE_t[d] = [L_t[d], U_t[d]]
```

Where:

| Symbol | Meaning |
| :--- | :--- |
| `L_t[d]` | Lower-bound estimate for remaining cost in dimension `d`. |
| `U_t[d]` | Upper-bound estimate for remaining cost in dimension `d`. |
| `R_t[d]` | Remaining budget in dimension `d`. |

The orchestrator should stop or escalate when the remaining budget is below the lower-bound estimated completion cost:

```text
EarlyStop if exists d such that R_t[d] < L_t[d]
```

For example, if a task has only two tool calls remaining but the lower-bound estimate requires at least four more tool calls, the loop should not continue pretending optimism is a scheduling policy.

### **Budget Cap Matrix**

The following caps are illustrative policy defaults. Real values must be calibrated by model price, tenant tier, task criticality, data sensitivity, latency SLO, and expected business value.

| Budget Type | Unit | Research Assistant | Code Workspace | Incident SRE Agent | Enforcement Owner |
| :--- | :--- | ---: | ---: | ---: | :--- |
| **Step Budget** | Completed graph transitions | 15 | 30 | 10 | Orchestrator state machine |
| **Tool-Call Budget** | Tool invocations | 30 | 50 | 15 | Tool gateway / policy interceptor |
| **External API Budget** | Paid or networked calls | 5 | 10 | 2 | API gateway and tool proxy |
| **Input Token Budget** | Input tokens per run | 200,000 | 400,000 | 100,000 | Gateway and context compiler |
| **Output Token Budget** | Output tokens per run | 30,000 | 60,000 | 15,000 | Model router / gateway |
| **Wall-Clock Budget** | Seconds | 300 | 600 | 120 | Orchestrator timeout controller |
| **Inference Cost Budget** | USD | `$2.00` | `$5.00` | `$1.00` | Billing and quota controller |
| **Retry Budget** | Consecutive attempts | 3 | 5 | 2 | Circuit breaker and retry policy |
| **Reflection Budget** | Diagnostic passes | 1 | 2 | 1 | Orchestrator |
| **Context Growth Budget** | Active context tokens | 100,000 | 200,000 | 50,000 | Context compiler |
| **Memory-Write Budget** | Persistent writes | 2 | 5 | 0 | Memory write gate |
| **Side-Effect Budget** | Mutating actions | 0 | 2 local workspace mutations | 0 | Tool policy layer and sandbox |

### **SRE Timeout and Circuit-Breaker Policies**

Budget enforcement should occur at multiple layers so no single model instruction, tool bug, or gateway failure can create an unbounded loop.

| Control | Trigger | Enforcement Action | Recovery Path |
| :--- | :--- | :--- | :--- |
| **Wall-Clock Timeout** | Run exceeds allowed duration. | Terminates the run, releases resources, and records timeout state. | Return partial result or escalation packet. |
| **Repeated Tool Call Breaker** | Same tool and argument hash repeats beyond threshold. | Blocks additional identical calls. | Enter repair, ask user, or escalate. |
| **Retry Storm Breaker** | Repeated transient failures or client retries. | Applies exponential backoff with jitter and caps retries. | Resume only after cooldown or operator approval. |
| **Budget Exhaustion Gate** | Any budget vector dimension reaches zero. | Stops execution before next model or tool call. | Return budget-exhausted status or request budget approval. |
| **Context Pressure Gate** | Active context exceeds configured threshold. | Runs compression node or halts if compression is unsafe. | Resume with compressed state and trace pointer. |
| **Privileged Action Gate** | Proposed write, financial action, communication, or infrastructure mutation. | Suspends execution pending approval. | Continue after approval or terminate on rejection. |
| **Safety Circuit Breaker** | Injection, unauthorized access, data leakage, or policy breach. | Fails closed and preserves trace. | Security review, policy update, or user-facing denial. |

Budget doctrine:

```text
The model may estimate cost.
The orchestrator measures cost.
The gateway enforces cost.
The trace records cost.
The user or supervisor approves budget expansion.
```

## **Scratchpads, Memory, and State Governance**

Uncontrolled state growth is a primary driver of agentic degradation. As an agent loop executes, raw tool outputs, intermediate plans, reflection notes, partial failures, and validation critiques accumulate. Without governance, this material bloats context, contaminates memory, increases latency, and encourages the model to reuse stale or unverified assumptions.

Agentic systems must separate memory by tenure, authority, mutability, and audit purpose.

```text
+--------------------------------------------------------------------------------
| STATE GOVERNANCE PLATFORM
+--------------------------------------------------------------------------------
|
| Ephemeral Scratchpad
|   - step-local working notes
|   - model-readable and model-writable
|   - discarded or compressed after step transition
|
| Structured Task State
|   - machine-readable run state
|   - system-owned, model-readable
|   - versioned after each verified transition
|
| Retrieved Evidence
|   - source-grounded contextual material
|   - read-only to the model
|   - scoped by permission, freshness, and task relevance
|
| Tool Observations
|   - raw and parsed tool results
|   - system-owned, append-only
|   - never editable by the model
|
| Persistent Memory
|   - long-term user or organizational memory
|   - write-gated after verification
|   - not directly writable by active model loops
|
| Execution Trace / Audit Record
|   - immutable chronological run record
|   - read-only after write
|   - used for replay, audit, incident response, and evaluation
|
+--------------------------------------------------------------------------------
| Rule:
|   Scratchpads help the model think. Task state tells the system what is true.
|   Tool observations prove what happened. Audit traces preserve why it happened.
+--------------------------------------------------------------------------------
```

| Memory / State Category | Tenure and Lifespan | Access Permissions | Storage Medium | Update Rules |
| :--- | :--- | :--- | :--- | :--- |
| **Ephemeral Scratchpad** | Step-local; expires at step transition or compression boundary. | Model read/write; system may inspect and discard. | Runtime context or local cache. | May contain working notes, but cannot be treated as ground truth. |
| **Structured Task State** | Active run lifespan. | System read/write; model read; model proposes changes indirectly. | Redis, PostgreSQL, durable checkpointer. | Updated only through validated state transitions using immutable schema versions. |
| **Retrieved Evidence** | Step-level or run-level depending on source tenure. | Model read-only. | Context cache, retrieval packet store. | Must retain source ID, timestamp, permission scope, and freshness metadata. |
| **Tool Observations** | Step-level during run; archived after termination. | System append-only; model read-only after parsing. | Versioned run log. | Ingested directly from tool APIs; records success, failure, timeout, and raw payload pointer. |
| **Persistent Memory** | Long-term across sessions. | System write-gated; model read through retrieval; model cannot directly write. | Vector DB, relational DB, document store. | Writes require post-run verification, source support, and tenure metadata. |
| **Execution Trace** | Permanent or retention-policy-bound. | Append-only during run; read-only after termination. | Secure trace store. | Captures prompts, model versions, tool calls, observations, state transitions, budgets, and terminal code. |
| **Audit Record** | Permanent for regulated workflows. | Compliance/SRE/security read-only. | WORM or tamper-evident storage. | Captures approval events, policy versions, exception records, and accountability boundaries. |

### **State Contamination and Context Compression**

State contamination occurs when an agent references stale observations, unverified assumptions, discarded scratchpad notes, or failed tool attempts as if they were verified facts. The orchestrator prevents this through state-tenure rules and compression gates.

When active context usage exceeds a configured threshold, such as 80% of the model context limit, the system should pause execution and run a compression node.

A safe compression node must preserve:

| Must Preserve | Reason |
| :--- | :--- |
| Active goal and acceptance criteria | Prevents goal drift. |
| Current plan and dependency state | Allows execution to resume correctly. |
| Verified observations | Maintains factual grounding. |
| Open questions and blocked steps | Prevents premature completion. |
| Budget state | Prevents hidden overruns. |
| Tool-call IDs and trace pointers | Preserves replayability. |
| Escalation and approval state | Prevents bypassing human gates. |

A compression node must discard or demote:

| Should Discard or Demote | Reason |
| :--- | :--- |
| Raw deliberation drafts | They are not durable state. |
| Failed plans superseded by verified state | Prevents stale plan reuse. |
| Redundant tool outputs already summarized into verified observations | Reduces context bloat. |
| Unsupported model assumptions | Prevents memory poisoning. |
| Low-value reflection chatter | Avoids over-reflection loops. |

Compression is not memory promotion. Compression summarizes active run state; persistent memory promotion requires separate verification gates after the run.

## **Tool Selection Policy Model**

The orchestrator, not the model, is the authority governing tool dispatch. The model may propose a tool and draft arguments, but the system must validate eligibility, permissions, schemas, budgets, risk class, and side-effect boundaries before execution.

```text
+--------------------------------------------------------------------------------
| TOOL SELECTION POLICY MODEL
+--------------------------------------------------------------------------------
|
| [ Model proposes tool call ]
|      |
|      v
| [ Tool Registry Lookup ]
|      |
|      +--> tool not registered -> [ Block / Repair / Ask User ]
|      |
|      v
| [ Schema Validation and Argument Normalization ]
|      |
|      +--> malformed args -> [ Repair Attempt or Clarification ]
|      |
|      v
| [ Security and Permission Check ]
|      |
|      +--> unauthorized -> [ Fail Closed or Escalate ]
|      |
|      v
| [ Budget and Rate-Limit Check ]
|      |
|      +--> over budget -> [ Terminate, Defer, or Request Approval ]
|      |
|      v
| [ Side-Effect Risk Classification ]
|      |
|      +--> read-only / low risk -> [ Dispatch ]
|      |
|      +--> write / financial / external comms / infra -> [ Human Approval Gate ]
|                                                        |
|                                                        +--> approved -> [ Dispatch ]
|                                                        |
|                                                        +--> rejected -> [ Terminate or Revise ]
|      |
|      v
| [ Tool Dispatch ]
|      |
|      v
| [ Observation Capture and Verification ]
|
+--------------------------------------------------------------------------------
| Rule:
|   Tool failure does not always mean immediate hard stop. The response depends
|   on risk: repair low-risk schema issues, ask for missing inputs, degrade when
|   safe, and fail closed for unauthorized or high-impact actions.
+--------------------------------------------------------------------------------
```

The system evaluates tool eligibility through a policy matrix.

| Tool Category | Allowed Actions | Permission Scope | Side-Effect Risk | Validation and Grounding Check | Default Approval Policy |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Read-Only Lookup** | Search documentation, logs, public sources, or approved internal indexes. | Active user or tenant read scope. | None to low. | Verify source metadata, permission scope, and freshness. | No human approval unless sensitive source expansion is requested. |
| **Retrieval Vectors** | Query corporate vector databases or evidence stores. | Active session and corpus permission scope. | None. | Verify document IDs, access policy, retrieval timestamp, and citation packet. | No human approval for authorized read-only retrieval. |
| **Live API Lookups** | Fetch active prices, order status, exchange rates, tickets, or inventory. | Authorized API key and user entitlement. | Low to moderate depending on endpoint. | Parse returned payload against schema and preserve raw observation. | No approval for read-only calls; approval for state-changing calls. |
| **Calculators / Code Sandbox** | Execute arithmetic, deterministic scripts, tests, parsers, or local transformations. | Local sandbox. | Low unless writing files or executing untrusted code. | Capture code, inputs, outputs, errors, and runtime limits. | No approval for read-only sandbox execution; approval for file mutation if policy requires. |
| **File System** | Read workspace files, propose edits, write local diagnostic artifacts. | Workspace sandbox. | Moderate. | Verify path allowlist, diff, file signatures, and post-write state. | Approval required for destructive, broad, or nonlocal writes. |
| **Database Queries** | Query approved schemas or write gated records. | Read-only or scoped write connection. | None for read-only; high for writes. | SQL allowlist, row-level security, transaction log, and post-action verification. | Writes require approval unless explicitly preauthorized. |
| **Email / Calendar** | Draft messages, inspect schedules, create draft events, propose replies. | User session scope. | High for external communication. | Verify recipients, content, timing, and user intent. | Human approval required before sending or creating external commitments. |
| **Procurement / Payment API** | Compare vendors, prepare purchase requests, inspect invoices. | Secure billing or procurement scope. | High financial risk. | Validate amount, vendor, approval chain, contract terms, and budget. | Human or supervisor approval required before execution. |
| **Infrastructure Tools** | Inspect metrics, logs, deploy state, or propose remediation. | SRE diagnostic scope. | High for mutations. | Verify blast radius, command target, rollback path, and approval record. | Mutating actions require operator approval. |
| **Review Queues** | Route tasks to manual inspection, approval, compliance, or escalation queues. | System workflow scope. | Low direct side-effect; may affect process latency. | Log reason, target queue, severity, and evidence packet. | No approval required to request review. |

### **Cognitive Emulation vs. Explicit Tool Execution**

Models can perform simple reasoning directly, but they are unreliable substitutes for deterministic systems when precision, external state, auditability, or side effects matter. The orchestrator must decide when a model may answer from reasoning and when it must call a tool.

| Task Type | Direct Model Reasoning Acceptable? | Tool Required? | Reason |
| :--- | :---: | :---: | :--- |
| **Low-stakes arithmetic estimate** | Yes | No | Approximation is acceptable and no external state is involved. |
| **Exact arithmetic, finance, engineering, or compliance calculation** | No | Yes | Precision and replayability are required. |
| **Current external fact lookup** | No | Yes | External state may have changed. |
| **Database or account status** | No | Yes | Must reflect authoritative system state. |
| **Code execution or test result** | No | Yes | Generated claims must be verified by execution. |
| **Tool availability or API result** | No | Yes | The model cannot infer whether an external call succeeded. |
| **High-stakes recommendation** | Limited | Usually | Requires source grounding, validation, and often review. |
| **Creative drafting or summarization from provided context** | Yes | No, unless source retrieval is needed | The task is generative and low side-effect. |

The rule is not “the model must never calculate.” The rule is stricter and more useful:

```text
Use model reasoning for low-risk synthesis.
Use tools for precision, current facts, external state, code execution,
financial or legal implications, audit requirements, and side effects.
```

This prevents cognitive emulation from masquerading as verified action. A model may suggest that a database update should happen. Only the tool can prove that it did.

## **Termination and Escalation Framework**

An agentic loop must never rely on the model's internal judgment as the sole stopping mechanism. Termination and escalation are programmatic contracts that determine when a run succeeds, pauses, degrades, requests help, fails closed, or releases resources.

Every agent run should terminate through an explicit terminal code.

| Termination Code | Classification | Trigger | Required Orchestrator Action |
| :--- | :--- | :--- | :--- |
| **SUCCESS** | Normal completion | Output meets goal, validation, grounding, and safety criteria. | Finalize response, close task state, release resources, and write trace. |
| **PARTIAL_SUCCESS** | Graceful degradation | Primary goal achieved but secondary actions failed, timed out, or were unavailable. | Return completed work, disclose limitations, record unresolved items, and close or defer remainder. |
| **IMPOSSIBLE** | Explicit handback | Required condition cannot be satisfied with available tools, data, authority, or budget. | Stop and explain the blocking condition. |
| **MISSING_INFO** | Explicit handback | Required input fields or choices are absent. | Suspend loop and ask user or operator for specific missing information. |
| **AMBIGUOUS_INTENT** | Explicit handback | User goal maps to multiple conflicting plans or risk classes. | Request clarification or route to human review. |
| **CONFIRM_REQUIRED** | Human approval | Proposed action requires approval due to write, cost, communication, or risk. | Suspend execution and present approval packet. |
| **REVIEW_REQUIRED** | Human review | Workflow is regulated, low-confidence, disputed, or policy-sensitive. | Route state, evidence, and recommendation to review queue. |
| **BUDGET_EXHAUSTED** | System intercept | Any required budget dimension reaches zero. | Stop before next model/tool call and record budget state. |
| **TIMEOUT** | System intercept | Wall-clock duration exceeds configured limit. | Terminate run, release resources, and return timeout status or escalation packet. |
| **VALIDATION_FAIL** | Quality halt | Output, observation, schema, or action result fails validation after allowed repair attempts. | Stop, repair once if allowed, or escalate based on risk. |
| **LOW_CONFIDENCE** | Quality halt | Scorer or verifier falls below configured confidence threshold. | Suspend autonomous execution and route to safer model, user, or human review. |
| **SOURCE_CONFLICT** | Validation halt | Retrieved or observed sources directly conflict on task-critical facts. | Freeze affected claim, preserve evidence, and request review or additional source resolution. |
| **REPEATED_FAILURE** | Resilience halt | Tool calls, plan repair, or retries fail beyond configured limits. | Trip circuit breaker and return failure or escalation packet. |
| **PERMISSION_DENIED** | Security guard | Requested tool, data, action, or state write exceeds active permission scope. | Block action; continue only if safe alternative exists. Escalate repeated or suspicious attempts. |
| **UNSAFE_DETECTION** | Safety guard | Injection, exfiltration, unsafe content, unauthorized mutation, or policy breach is detected. | Fail closed for high-severity cases; preserve trace and route to security review when warranted. |
| **UNAVAILABLE_DEP** | Dependency failure | Required tool, database, provider, or service is unavailable. | Use safe degraded mode when possible; otherwise stop and report dependency failure. |
| **USER_CANCEL** | Explicit cancellation | User or operator aborts the run. | Halt execution, release resources, record cancellation, and preserve trace. |

### **Escalation Routing Model**

```text
+--------------------------------------------------------------------------------
| ESCALATION ROUTING MODEL
+--------------------------------------------------------------------------------
|
| [ Trigger Detected ]
|      |
|      v
| [ Classify Trigger ]
|      |
|      +--> Safety / security / unauthorized action
|      |       |
|      |       v
|      |   [ Fail Closed ]
|      |       preserve trace | block action | notify security if severity warrants
|      |
|      +--> Write / purchase / communication / infrastructure mutation
|      |       |
|      |       v
|      |   [ Human Approval Gate ]
|      |       render action packet | await approval | continue or terminate
|      |
|      +--> Missing information / ambiguous intent
|      |       |
|      |       v
|      |   [ Ask User / Operator ]
|      |       suspend run | request specific input | resume after answer
|      |
|      +--> Low confidence / source conflict / validation uncertainty
|      |       |
|      |       v
|      |   [ Review Queue or Safer Model ]
|      |       preserve evidence | route to specialist | continue only after validation
|      |
|      +--> Dependency outage / capacity constraint
|              |
|              v
|          [ Degraded Mode / Safe Fallback ]
|              downgrade | defer | cached-safe response | clean unavailable status
|
+--------------------------------------------------------------------------------
| Rule:
|   Escalation is not failure. It is controlled transfer of decision authority
|   from the autonomous loop to a safer actor, policy, or execution path.
+--------------------------------------------------------------------------------
```

### **Escalation Trigger Matrix**

| Failure / Risk State | Severity | Target Escalation Endpoint | System Recovery Action |
| :--- | :--- | :--- | :--- |
| **High-Impact Mutation** | High | User or operator confirmation gate | Suspend execution; render proposed action, parameters, blast radius, and rollback path. |
| **Financial Commitment** | High | Manager or procurement approval queue | Freeze action until budget and approval chain are confirmed. |
| **External Communication** | Moderate to high | User approval gate | Draft only; send only after explicit approval. |
| **Missing Input Data** | Low | User clarification | Pause loop and request specific missing field. |
| **Ambiguous Goal** | Low to moderate | User or supervisor clarification | Present candidate interpretations and await selection. |
| **Low Scorer Confidence** | Moderate | Specialist model or review queue | Route state to stronger verifier or human reviewer. |
| **Tool Failure Exceptions** | Moderate | Local repair path or dependency fallback | Retry within budget, switch equivalent tool, or return degraded status. |
| **Budget Limit Reached** | Moderate to high | Budget approval queue | Freeze run and require explicit budget release. |
| **Database or Provider Outage** | Moderate to high | SRE or degraded-mode controller | Switch to read-only backup, fallback provider, cached-safe response, or unavailable status. |
| **Security Guard Breach** | High | Security Operations Center | Kill or suspend run, preserve trace, revoke credentials only for confirmed compromise. |
| **Repeated Permission Denial** | Moderate to high | Security or compliance review | Block further privileged attempts and inspect trace for prompt injection or abuse. |

Termination doctrine:

```text
Every loop must have a finish line.
Every failure must have a terminal path.
Every escalation must identify who receives authority next.
Every terminal state must release resources and write trace.
```

## **Failure Modes and Agentic Pathologies**

Agentic systems introduce failure surfaces that do not exist in traditional request-response software. The system can fail not only by producing a wrong answer, but by taking the wrong path, calling the wrong tool, spending too much, escalating too late, writing bad memory, or refusing to stop.

| Pathology Family | Observed Symptom | Underlying Root Cause | Telemetry Indicator | Immediate Containment | Long-Term Architectural Remedy |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Runaway Loops** | System repeats identical planning or repair steps. | Vague termination rules, missing exit edge, or parser failure. | Repeated transition hashes; step count exceeds plan bound. | Force terminal state and preserve trace. | Compile graph loops with hard step caps and explicit exits. |
| **Tool Spam** | Tool-call frequency spikes. | Aggressive triggers, weak tool-selection policy, or reflection loop. | Tool calls per run/minute exceed threshold. | Suspend tool access for run; enter repair or terminate. | Enforce tool budgets, duplicate-call detection, and policy gates. |
| **Plan / Goal Drift** | Plan strays from user goal or acceptance criteria. | Broad planning prompt, stale context, or unbounded decomposition. | Semantic distance from goal increases across plan revisions. | Pause and rerun plan validation. | Anchor plan to structured goal object and compiled graph nodes. |
| **Context Bloat** | Latency rises; model ignores instructions or loses key facts. | Raw outputs, drafts, and reflections appended without compression. | Context usage exceeds threshold; retrieval omissions rise. | Run compression node or halt if unsafe. | Govern scratchpad lifecycle and preserve only active verified state. |
| **Memory Poisoning** | Later steps cite inaccurate or unsupported facts. | Unverified observations or assumptions committed to memory. | Memory entries lack source support or validator ID. | Quarantine memory write; reload from verified trace. | Route persistent memory writes through post-run verification gates. |
| **Hallucinated Observation** | Model claims a tool ran or succeeded when it did not. | Model bypasses tool return parser or infers success. | Tool registry shows no matching invocation or no verified result. | Halt loop and mark grounding violation. | Separate intended, attempted, observed, verified, and committed states. |
| **Premature Termination** | Loop stops before goal is complete. | Weak completion check or model-declared success without evidence. | Terminal code conflicts with failed validation state. | Reopen run only through orchestrator; resume from checkpoint. | Require verified acceptance criteria before SUCCESS. |
| **Refusal to Stop** | Loop continues after success or terminal condition. | Reflection loop ignores completion state or graph lacks terminal edge. | Step count rises after success criteria already met. | Force Finalize -> Terminate transition. | Independent completion validator and hard terminal guards. |
| **Over-Reflection** | High token spend on minor critique without state improvement. | Reflection invoked automatically after every step. | Reflection tokens high; state delta near zero. | Skip reflection and continue or terminate. | Trigger reflection only on specific diagnostic conditions. |
| **Redundant Decomposition** | Duplicate subtasks appear in plan. | Missing dependency map or subtask signature tracking. | Identical subtask hashes in active plan. | Merge duplicates and update dependency tree. | Limit recursive decomposition depth and require unique subtask IDs. |
| **Duplicate Actions** | Identical API requests sent repeatedly. | Missing transaction locks, idempotency keys, or retry controls. | Matching API call signatures within short interval. | Block duplicates and inspect run state. | Enforce idempotency keys and duplicate-dispatch guards. |
| **Budget Blowout** | Token, tool, or dollar spend spikes. | Unbounded loop, retry storm, or missing budget enforcement. | Cost exceeds run budget or projected completion cost. | Terminate or freeze pending approval. | Enforce vector budgets and progressive interval estimation. |
| **Stale State** | Agent plans from outdated observations. | Missing cache invalidation or state-tenure rules. | State timestamp lags latest tool observation. | Invalidate active state and rerun lookup. | Bind state entries to source tenure, timestamp, and authority. |
| **Permission Creep** | Model attempts unauthorized tools or data access. | Broad credentials exposed to model context or weak policy gate. | Permission denial events, tool mismatch, suspicious escalation attempts. | Block action and inspect for injection. | Use least-privilege credentials and active user session scope. |
| **Tool Overreach** | Complex or costly APIs used for simple tasks. | Missing deterministic path or weak tool-use heuristic. | High-cost tool used for low-risk lookup or calculation. | Reject tool call and route to local deterministic method. | Add least-tool policy and cheap deterministic alternatives. |
| **Coordination Failure** | Peer workers stall waiting for each other. | Circular dependencies or missing blackboard state rules. | Workflow graph state unchanged while workers remain active. | Terminate or collapse to supervisor path. | Enforce static DAGs, dependency tracking, and supervisor arbitration. |
| **Validator Retry Storm** | Outputs repeatedly fail schema or safety checks and trigger retries. | Bad prompt contract, incompatible model, or over-strict validator. | Retry count, validator CPU, and latency spike together. | Cap retries and fail closed or escalate. | Add schema-specialized route and validator regression tests. |
| **Human-Review Queue Saturation** | Runs pile up waiting for approvals. | Too many workflows route to human gates or approval policy is too broad. | Review queue age and pending count exceed threshold. | Defer low-priority reviews and fail closed for high-risk expired approvals. | Calibrate approval thresholds and add specialized reviewer queues. |
| **Memory-Write Poisoning** | Bad long-term memory persists across sessions. | Weak memory promotion gate or unsupported summary. | Future runs retrieve low-grounding memory entry. | Quarantine memory block and rebuild from trace. | Require evidence-backed memory writes with expiration and source links. |
| **Tool Credential Expiry** | Valid plan fails because tool auth expires mid-run. | Long-running workflow outlives credential TTL. | 401/403 tool errors after previous success. | Refresh token through approved path or pause run. | Bind credential tenure to run duration and renew safely before expiry. |
| **Observation Parser Drift** | Tool output is misread after API/schema changes. | Parser version no longer matches tool response. | Parse errors, missing fields, or wrong normalized state. | Halt affected tool path and preserve raw payload. | Version tool schemas and test parsers against provider changes. |
| **Idempotency Failure** | Retry performs side effect multiple times. | Missing idempotency keys or transaction locks. | Duplicate external records, charges, messages, or writes. | Stop retries and reconcile external state. | Require idempotency keys for all mutating tools. |
| **Escalation Bypass** | High-risk action proceeds without approval. | Tool risk misclassification or policy guard missing. | Mutating action lacks approval trace. | Revoke action path and open incident. | Make approval state a required transition guard. |

Pathology doctrine:

```text
A wrong final answer is only one failure mode.
A wrong trajectory, wrong tool, wrong state write, wrong escalation,
or wrong budget decision can be just as damaging.
```

## **Agent Evaluation and Observability Guidance**

Evaluating agentic systems is different from scoring traditional request-response engines. The final answer matters, but so does the path taken to reach it. A run can produce an acceptable final answer while wasting budget, calling unauthorized tools, skipping validation, contaminating memory, or narrowly avoiding a dangerous action.

Agent evaluation must therefore measure both end-state quality and trajectory quality.

### **Agent Run Trace Requirements**

Every run must produce an immutable Agent Run Trace. This trace is the foundation for debugging, regression testing, compliance replay, incident response, and cost-per-success analysis.

| Trace Field | Purpose |
| :--- | :--- |
| `run_id` | Stable identifier for the full agent run. |
| `tenant_id` / `user_scope` | Binds execution to authority, quota, and audit context. |
| `goal_object` | Captures the structured target the agent was trying to satisfy. |
| `autonomy_boundary_version` | Records which decision rights were active. |
| `workflow_graph_version` | Identifies the compiled graph used for transitions. |
| `model_versions` | Records planner, executor, verifier, and routing model versions. |
| `prompt_template_versions` | Supports replay and prompt-regression analysis. |
| `tool_registry_version` | Identifies which tools and schemas were available. |
| `state_checkpoints` | Captures task state after each verified transition. |
| `tool_calls` | Records tool name, arguments hash, request ID, result status, and observation pointer. |
| `budget_vector` | Tracks tokens, cost, calls, retries, time, context, and memory writes. |
| `validation_results` | Records schema, safety, grounding, and acceptance checks. |
| `escalation_events` | Captures approval requests, human decisions, and review outcomes. |
| `terminal_code` | Explains how and why the run ended. |

### **Trajectory Evaluation vs. End-State Scans**

End-state scans alone are insufficient. A final answer may be correct even though the agent:

* exceeded budget
* called redundant tools
* used stale observations
* skipped a required approval
* wrote unsupported memory
* silently failed a tool call
* reached the answer through an unsafe or non-replayable path

Trajectory-level validation analyzes intermediate steps to identify inefficiencies, tool misuse, near-violations, and unsafe patterns before they become production incidents.

### **Agent Evaluation Metric Matrix**

The targets below are policy targets, not universal constants. Each platform should calibrate thresholds by task type, risk class, tenant tier, latency SLO, model family, and workflow complexity.

| Target Metric | Measurement Unit | Example Policy Target | Evaluation Collection Method | What It Detects |
| :--- | :--- | :--- | :--- | :--- |
| **Task Success Rate** | Percentage | `>= 98%` for stable workflows | Golden task set and production-labeled outcomes | Whether the agent completes the requested task. |
| **Correct Termination** | Percentage | `100%` for regulated workflows | Terminal-code audit against expected outcomes | Premature stops, refusal to stop, wrong terminal code. |
| **Trajectory Validity** | Percentage of legal transitions | `100%` | Replay trace against compiled workflow graph | Illegal edges, skipped guards, runtime graph mutation. |
| **Trajectory Efficiency** | Actual steps / reference steps | `<= 1.25x` reference path for stable workflows | Compare run path against golden or expert path | Redundant decomposition, tool spam, over-reflection. |
| **Escalation Accuracy** | Precision / recall | `>= 95%` on injected anomalies | Fault-injection and review-set tests | Missed approvals, excessive human review, unsafe autonomy. |
| **Step Count Efficiency** | Average steps per task | Workflow-specific | Trace logs and golden path comparison | Excessive planning, repair, or recursion. |
| **Loop Depth Limit** | Max recursive depth | `<= 2` unless explicitly approved | Parent-child task trace analysis | Runaway decomposition and swarm proliferation. |
| **Tool-Call Count** | Calls per task | Workflow-specific | Tool gateway logs | Tool overuse and missed deterministic paths. |
| **Failed Tool Calls** | Count or percentage | Low and bounded by retry policy | Tool result telemetry | Bad arguments, unavailable dependencies, parser drift. |
| **Retry Rate** | Retries / tool dispatches | `<= 5%` for stable tools | Tool gateway and circuit-breaker logs | Retry storms and brittle tool contracts. |
| **Timeout Rate** | Percentage of runs | `<= 1%` for interactive workflows | Orchestrator timeout records | Capacity, dependency, or loop-bound failures. |
| **Budget Adherence** | Percentage | `100%` hard cap compliance | Budget-vector telemetry | Budget overruns and ineffective enforcement. |
| **Cost per Success** | USD per successful run | Within tenant plan and margin | Billing ledger plus terminal status | Whether cost savings survive failures and retries. |
| **Observation Grounding Rate** | Verified state updates / total state updates | `100%` | State checkpoint audit | Hallucinated observations and assumed tool success. |
| **Memory Write Quality** | Supported memory writes / total writes | `100%` for persistent memory | Memory write-gate audit | Memory poisoning and unsupported long-term facts. |
| **Human Override Rate** | Overrides / approvals | Workflow-specific | Review queue outcomes | Over-trusting automation or over-escalating trivial cases. |
| **User Correction Rate** | User corrections / sessions | Workflow-specific | Conversation trace analysis | Misunderstood intent or poor final synthesis. |
| **Trace Completeness** | Required trace fields present | `100%` for production | Trace schema audit | Non-replayable runs and missing incident evidence. |

### **Evaluation Harness Design**

Agent evaluation requires multiple test modes:

| Test Mode | Purpose |
| :--- | :--- |
| **Golden Trajectory Tests** | Compare run path against expert-approved reference trajectories. |
| **Fault Injection** | Simulate tool failures, malformed observations, timeouts, and permission denials. |
| **Budget Stress Tests** | Verify termination before token, cost, wall-clock, or tool-call caps are exceeded. |
| **Escalation Tests** | Confirm high-risk operations route to approval or fail closed. |
| **Replay Tests** | Reconstruct past runs from trace and verify deterministic state transitions. |
| **Long-Run Soak Tests** | Detect memory bloat, retry storms, queue buildup, and context compression failures. |
| **Policy Regression Tests** | Verify autonomy boundaries after tool, prompt, model, or policy changes. |

Agent observability should make every run answerable in one sentence:

```text
The agent was allowed to do X, chose path Y, called tools Z,
spent budget B, updated state S from verified observations,
and terminated because condition T was met.
```

## **Cross-Canon Handoff Map**

Agentic orchestration does not exist in isolation. AI-ENG-M defines how autonomous loops are bounded, budgeted, traced, terminated, escalated, and evaluated. Those orchestration contracts feed directly into downstream tool, verification, security, evaluation, incident, governance, and reference-architecture systems across the Canon.

| Downstream Target Report | Handoff Artifact | Underlying System Implication | Downstream Architectural Owner |
| :--- | :--- | :--- | :--- |
| **AI-ENG-N — Tool Contracts** | Dynamic tool payload request signatures, tool eligibility decisions, argument schemas, and repair traces. | Enforces strict type checking, schema matching, tool authorization, and structured error handling. | Platform Gateway / Serialization Engine. |
| **AI-ENG-O — Action Verification** | Grounded observation cache, attempted/observed/verified state distinctions, and post-action validation requirements. | Reconciles state changes, confirms external side effects, and prevents assumed tool success. | Post-Action Verification Scorer. |
| **AI-ENG-S — Production Pathologies** | Run traces, retry histories, loop failures, budget breaches, and pathologies. | Documents runtime loops, tool overreach, cache issues, and orchestration failures. | Site Reliability Engineering Teams. |
| **AI-ENG-T — Permissions & Injection** | Active session scope metadata, autonomy boundaries, tool allowlists, and permission-denial traces. | Restricts model capabilities to authorized user and tenant boundaries. | Identity & Access Management Gateway. |
| **AI-ENG-V — Resource Abuse** | Loop budgets, token budgets, tool budgets, retry caps, and side-effect limits. | Terminates runs that violate resource limits or denial-of-wallet controls. | Resource Quota / Gateway Rate Limiter. |
| **AI-ENG-W — Fallback Modes** | Escalation routes, degraded-mode triggers, dependency-failure states, and safe terminal paths. | Governs behavior during tool outages, provider failures, capacity limits, and validation failure. | Incident Management Router. |
| **AI-ENG-X — Trust & Control** | Human approval packets, proposed plans, risk explanations, and user-facing confirmation states. | Renders interactive approval, clarification, and override interfaces. | User Interface / Trust Control Layer. |
| **AI-ENG-Y — Human-in-the-Loop** | Escalation requests, review queues, approval records, and reviewer decisions. | Manages human review, approval workflows, and asynchronous audit outcomes. | Human-in-the-Loop Queue. |
| **AI-ENG-Z — Telemetry** | Step counts, route IDs, budget vectors, tool latency, terminal codes, and execution metrics. | Streams active session metrics to dashboards and reliability monitors. | Prometheus / Datadog / OpenTelemetry Routers. |
| **AI-ENG-AA — Agent Evaluations** | Golden trajectory sets, fault-injection scenarios, replay tests, budget tests, and escalation tests. | Evaluates regression issues, path quality, task success, and model capability. | CI/CD Automated Test Runner. |
| **AI-ENG-AB — Replay Traces** | Versioned state checkpoints, model versions, prompt versions, tool calls, observations, and terminal states. | Reconstructs and debugs failures in sandboxes and audit environments. | Developer Testing / Audit Environment. |
| **AI-ENG-AC — Incident Response** | System halt traces, error states, circuit-breaker events, stuck runs, and budget breaches. | Supports rapid diagnosis during orchestration incidents and runaway-loop events. | Active On-Call SRE Systems. |
| **AI-ENG-AD — Governance, Policy, Compliance & Accountability** | Autonomy boundary versions, approval policies, exception records, audit traces, and accountable decision owners. | Ensures delegated decision rights, escalation policies, memory writes, and tool actions comply with organizational governance. | Governance / Risk / Compliance Owner. |
| **AI-ENG-AJ — Reference Architectures** | Orchestrator state-machine patterns, graph templates, budget controllers, and trace schemas. | Serves as the blueprint for scalable agentic systems. | Systems Engineering and Architecture. |

The durable handoff is this:

```text
AI-ENG-M exports the runtime contract for bounded autonomy:
what the model may decide, which tools it may use, how much it may spend,
when it must stop, when it must escalate, and how its full trajectory
can be replayed after the fact.
```

## **Durable Principles for Agentic Orchestration**

To summarize the doctrinal architecture of agentic orchestration, platform leads, security reviewers, and system architects must build upon six durable, SRE-grade design axioms:

### **I. Autonomy is an Enforced Architectural Lease**

Autonomy must never be treated as an abstract behavior or system trait.2 It is a highly bounded architectural permission leased to a model under strict control. The system must always know exactly what decisions the model may make, which tools it may call, what data it can access, and what state parameters it is permitted to write.2

### **II. The Least-Autonomy Architecture Mandate**

An architect must always select the least autonomous pattern on the decision ladder that can resolve the task.3 Programmatic code, static prompt chains, and deterministic workflow graphs should handle predictable processes.3 Reserve open-ended agentic loops strictly for tasks where the execution path cannot be known in advance and the cost and risk profile can tolerate variation.2

### **III. System Budgets Must Actively Limit Execution**

An agent loop is safe only when bounded by comprehensive, active loop and tool budgets.5 These budgets—temporal, financial, token-count, and retry-limit—must be enforced by independent platform gateways, not the model.8 The system must dynamically estimate remaining costs and terminate runs before budgets are exhausted.8

### **IV. Ground State Updates in Verified Observations**

The orchestrator must never allow a model to assume tool success or predict environmental states.5 Task state updates and plan adjustments must rely on verified, sanitized, and structurally validated data returned directly from tool execution.5

### **V. Constrain Loops with Graph Runtimes and State Machines**

Open-ended model loops are too unpredictable for enterprise deployments.2 Structure agent actions within compiled, immutable directed graph workflows or state machines where transitions are validated by deterministic guards and execution state is preserved via transactional checkpointers.13

### **VI. Programmatically Define Termination and Escalation**

An agentic workflow must terminate and escalate based on explicit, programmatic contracts.6 Every system run must have a clear finish line, and encountering anomalies, security risks, or cost breaches must immediately trigger deterministic fail-closed stops or human intervention paths.5

#### **Works cited**

1. Governance by Design: Architecting Agentic AI for Organizational Learning and Scalable Autonomy - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2605.20210v1](https://arxiv.org/html/2605.20210v1)  
2. Bounded Autonomy: Behavioral Specification Languages and ..., accessed June 9, 2026, [https://www.authorea.com/doi/full/10.22541/au.177083908.89981049/v1](https://www.authorea.com/doi/full/10.22541/au.177083908.89981049/v1)  
3. A Developer's Guide to Building Scalable AI: Workflows vs Agents | Towards Data Science, accessed June 9, 2026, [https://towardsdatascience.com/a-developers-guide-to-building-scalable-ai-workflows-vs-agents/](https://towardsdatascience.com/a-developers-guide-to-building-scalable-ai-workflows-vs-agents/)  
4. Agentic Automation - Grid Dynamics, accessed June 9, 2026, [https://www.griddynamics.com/glossary/agentic-automation](https://www.griddynamics.com/glossary/agentic-automation)  
5. Self-Healing Agentic Orchestrators for Reliable Tool-Augmented Large Language Model Systems - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2606.01416v1](https://arxiv.org/html/2606.01416v1)  
6. AI agent orchestration: In-depth guide to coordinating autonomous systems - N-iX, accessed June 9, 2026, [https://www.n-ix.com/ai-agent-orchestration/](https://www.n-ix.com/ai-agent-orchestration/)  
7. Stopping Conditions That Actually Stop Multi-Agent Loops - DEV Community, accessed June 9, 2026, [https://dev.to/dowhatmatters/stopping-conditions-that-actually-stop-multi-agent-loops-bnb](https://dev.to/dowhatmatters/stopping-conditions-that-actually-stop-multi-agent-loops-bnb)  
8. Budget-Aware LLM Agents. Evaluation, Failure Modes, and Trainable Cost Control, accessed June 9, 2026, [https://www.researchgate.net/publication/405474946_Budget-Aware_LLM_Agents_Evaluation_Failure_Modes_and_Trainable_Cost_Control](https://www.researchgate.net/publication/405474946_Budget-Aware_LLM_Agents_Evaluation_Failure_Modes_and_Trainable_Cost_Control)  
9. Open-Source LLMs in 2026: What Teams Actually Trust | by Thinking Loop | Medium, accessed June 9, 2026, [https://medium.com/@ThinkingLoop/open-source-llms-in-2026-what-teams-actually-trust-1f2e0ebbbda9](https://medium.com/@ThinkingLoop/open-source-llms-in-2026-what-teams-actually-trust-1f2e0ebbbda9)  
10. [2606.01416] Self-Healing Agentic Orchestrators for Reliable Tool-Augmented Large Language Model Systems - arXiv, accessed June 9, 2026, [https://arxiv.org/abs/2606.01416](https://arxiv.org/abs/2606.01416)  
11. How to Build Agentic AI: Core Steps, Key Risks, and Smart Solutions | by Designveloper, accessed June 9, 2026, [https://dsvgroup.medium.com/how-to-build-agentic-ai-core-steps-key-risks-and-smart-solutions-04ff809d0200?source=rss-------1](https://dsvgroup.medium.com/how-to-build-agentic-ai-core-steps-key-risks-and-smart-solutions-04ff809d0200?source=rss-------1)  
12. 20 Agentic AI Workflow Patterns That Actually Work in 2025 - Skywork, accessed June 9, 2026, [https://skywork.ai/blog/agentic-ai-examples-workflow-patterns-2025/](https://skywork.ai/blog/agentic-ai-examples-workflow-patterns-2025/)  
13. LangGraph Multi-Agent Orchestration: Complete Framework Guide + Architecture Analysis 2025 - Latenode Blog, accessed June 9, 2026, [https://latenode.com/blog/ai-frameworks-technical-infrastructure/langgraph-multi-agent-orchestration/langgraph-multi-agent-orchestration-complete-framework-guide-architecture-analysis-2025](https://latenode.com/blog/ai-frameworks-technical-infrastructure/langgraph-multi-agent-orchestration/langgraph-multi-agent-orchestration-complete-framework-guide-architecture-analysis-2025)  
14. Teaching LangChain Agents to Plan & Run Multi-Step, Multi-Tool Workflows - Medium, accessed June 9, 2026, [https://medium.com/@avigoldfinger/teaching-langchain-agents-to-plan-run-multi-step-multi-tool-workflows-82ac908fd56e](https://medium.com/@avigoldfinger/teaching-langchain-agents-to-plan-run-multi-step-multi-tool-workflows-82ac908fd56e)  
15. Multi-Agent Orchestration and Architecture - Runpod, accessed June 9, 2026, [https://www.runpod.io/articles/guides/multi-agent-orchestration-and-architecture](https://www.runpod.io/articles/guides/multi-agent-orchestration-and-architecture)  
16. I spent a long time thinking about how to build good AI agents. This is the simplest way I can explain it. : r/ClaudeCode - Reddit, accessed June 9, 2026, [https://www.reddit.com/r/ClaudeCode/comments/1rt37um/i_spent_a_long_time_thinking_about_how_to_build/](https://www.reddit.com/r/ClaudeCode/comments/1rt37um/i_spent_a_long_time_thinking_about_how_to_build/)  
17. [2606.00198] BAGEN: Are LLM Agents Budget-Aware? - arXiv, accessed June 9, 2026, [https://arxiv.org/abs/2606.00198](https://arxiv.org/abs/2606.00198)  
18. Sudden spike in GitHub Copilot usage after running background_task #418, accessed June 9, 2026, [https://github.com/code-yeongyu/oh-my-openagent/issues/418](https://github.com/code-yeongyu/oh-my-openagent/issues/418)  
19. Five techniques to reach the efficient frontier of LLM inference | Google Cloud Blog, accessed June 9, 2026, [https://cloud.google.com/blog/topics/developers-practitioners/five-techniques-to-reach-the-efficient-frontier-of-llm-inference](https://cloud.google.com/blog/topics/developers-practitioners/five-techniques-to-reach-the-efficient-frontier-of-llm-inference)  
20. Why Inference Systems Are the New AI Bottleneck - Newline.co, accessed June 9, 2026, [https://www.newline.co/@Dipen/why-inference-systems-are-the-new-ai-bottleneck--a9bc0631](https://www.newline.co/@Dipen/why-inference-systems-are-the-new-ai-bottleneck--a9bc0631)  
21. Training to Inference: Why AI Cloud Must Catch Up - DigitalOcean, accessed June 9, 2026, [https://www.digitalocean.com/community/conceptual-articles/training-to-inference-why-ai-cloud-must-catch-up](https://www.digitalocean.com/community/conceptual-articles/training-to-inference-why-ai-cloud-must-catch-up)  
22. Human in the loop (HITL) AI Agents with LangGraph & Elastic - Elasticsearch Labs, accessed June 9, 2026, [https://www.elastic.co/search-labs/blog/human-in-the-loop-hitllanggraph-elasticsearch](https://www.elastic.co/search-labs/blog/human-in-the-loop-hitllanggraph-elasticsearch)  
23. Bounded Autonomy: Behavioral Specification Languages and Runtime Enforcement Architectures for Trustworthy Agentic AI Systems | Authorea, accessed June 9, 2026, [https://www.authorea.com/doi/full/10.22541/au.177083908.89981049](https://www.authorea.com/doi/full/10.22541/au.177083908.89981049)  
24. I built a circuit breaker SDK for LLM agents — catches loops, budget overruns, and privilege escalations : r/LangChain - Reddit, accessed June 9, 2026, [https://www.reddit.com/r/LangChain/comments/1tu7ite/i_built_a_circuit_breaker_sdk_for_llm_agents/](https://www.reddit.com/r/LangChain/comments/1tu7ite/i_built_a_circuit_breaker_sdk_for_llm_agents/)  
25. BAGEN: Are LLM Agents Budget-Aware? - arXiv, accessed June 9, 2026, [https://arxiv.org/html/2606.00198v1](https://arxiv.org/html/2606.00198v1)

---

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

# AI-ENG-O — Action Verification - Planning, Execution, Observation & Recovery

The integration of generative artificial intelligence models into distributed computing environments introduces a significant architectural mismatch: the interface between probabilistic, non-deterministic decision engines and rigid, deterministic state machines. In agentic workflows, this mismatch manifests as a systemic vulnerability at the point of action execution. Historically, distributed systems have relied on transport-level or database-level execution acknowledgments (such as an HTTP 200 OK or a write-ahead log commit) as proof of successful state mutation. However, in high-dimensional agentic architectures, an action cannot be deemed complete when the model asserts its completion, or even when the invoking tool returns a successful execution response.  
**An action is not complete when the model says it is complete, or even when the tool returns success. An action is complete only when the system verifies the resulting state against the intended outcome and reconciles any discrepancy.**  
Action verification functions as the central truth-management layer of tool-using AI architectures. It bridges the gap between what an agent believes it has accomplished and what has actually changed within authoritative systems. The fundamental question is not "Did the tool return a success response?" but "Did the intended state change actually occur, in the correct system, to the correct object, under the correct tenant and permission scope, with the expected side effects, no hidden duplicates, no unresolved partial failures, and an auditable recovery path if anything diverged?". Without this verification layer, probabilistic agents operate on ungrounded assumptions, leading to cascade failures, duplicate mutations, or silent data corruptions. This report establishes the architectural specification for verifying planning, execution, observation, and recovery in enterprise-grade agentic environments.

## **Canon Placement and Continuity**

This report closes **Volume 5: Agentic Systems and Tool-Using Architectures**, whose primary concern is how static generative models are transformed into autonomous actors, and how to make those actions bounded, contract-governed, verified, and recoverable.  
To maintain structural durability within the *AI Engineering Systems Canon*, this report establishes precise interfaces and clear boundaries with preceding and succeeding modules:

* **AI-ENG-M (Agentic Orchestration)** owns the decision-making loop. It determines whether the agent should plan, call a tool, continue, stop, or escalate based on the current context. It manages loop budgets and records raw execution traces.  
* **AI-ENG-N (Tool Contracts)** owns the execution gateway. It determines whether a proposed tool call is syntactically valid, authorized, safe, and idempotent prior to execution. It manages deterministic wrappers, validation schemas, and structural output contracts.  
* **AI-ENG-O (Action Verification)** owns the post-action truth. It determines what actually occurred after execution, whether the agent’s task state can be updated, whether the user can be notified of completion, and the exact recovery path that must execute if reality diverges from the model’s belief.

```
  +---------------------------------------------------------------------------------+  
  |                            Volume 5 Architectural Boundaries                    |  
  +---------------------------------------------------------------------------------+  
  |  AI-ENG-M: Agentic Orchestration                                                |  
  |  - Decision-making loop                                                         | 
  |  - Plan generation and sub-goal management                                      |  
  |  - Autonomy boundaries and loop budgets                                         |  
  +---------------------------------------------------------------------------------+  
                                         |  
                                         v  
  +---------------------------------------------------------------------------------+  
  |  AI-ENG-N: Tool Contracts                                                       |  
  |  - Parameter validation and deterministic wrappers                              |  
  |  - Security mapping and credentials injection                                   |  
  |  - Idempotency key generation and confirmation gates                            |  
  +---------------------------------------------------------------------------------+  
                                         |  
                                         v  
  +---------------------------------------------------------------------------------+  
  |  AI-ENG-O: Action Verification (Truth-Management Layer)                         |  
  |  - Post-execution state verification and readbacks                              |  
  |  - Discrepancy classification and state reconciliation                          |  
  |  - Rollback, compensation sagas, and forward recovery                           |  
  +---------------------------------------------------------------------------------+
```

Furthermore, this report inherits and extends doctrines from across the broader canon:

* **AI-ENG-B (Context Tenure and State Governance):** Reconciled system states are bound to specific context life spans and strict tenant security scopes to prevent data leakage during multi-turn verification loops.  
* **AI-ENG-C (Cost Attribution and System Margin):** Post-action verification queries and polling loops consume model tokens and API quotas, requiring dedicated margin allocations to protect system throughput.  
* **AI-ENG-I (Release Manifest, Regression Control, and Replay):** Verification strategies, polling rules, and recovery workflows are treated as version-controlled artifacts, validated via replayable execution histories to prevent behavioral regressions.  
* **AI-ENG-L (Serving Observability and Operational Fallback):** Degraded verification states (such as an unreachable database replica) map directly to defined fallback pathways to maintain agent availability without sacrificing correctness.

## **Conceptual Glossary**

To establish a uniform vocabulary for platform architects, agent engineers, and site reliability engineers (SREs), the fundamental terms of action verification are defined below.

| Term | Technical Definition | Operational Implication |
| :---- | :---- | :---- |
| **Action Verification** | The post-execution discipline that confirms whether a tool mutation produced the correct physical state change. | Prevents the agent from updating its task state or claiming success until state changes are proven. |
| **Intended State** | The desired final state of the target system, derived from the user goal or planning parameters. | Serves as the reference baseline against which actual observations are evaluated. |
| **Requested State** | The specific payload values generated by the model and validated by deterministic tool wrappers. | Represents the operational translation of intent into formal API parameters. |
| **Observed State** | The initial data returned within the tool execution payload or Model Context Protocol (MCP) observation object. | Treated as unverified evidence; cannot modify the believed task state. |
| **Authoritative State** | The ground-truth state retrieved directly from the primary source of record, bypassing replicas and caches. | The final baseline used to confirm execution success. |
| **Believed Task State** | The internal state representation maintained in the agent's orchestrator memory or context window. | Read-only; updated only after authoritative state reconciliation is complete. |
| **Hallucinated Success** | An execution failure where an agent treats an incomplete, failed, or simulated action as fully completed. | Corrupts subsequent planning, audit logs, and user-facing status reports. |
| **Assumed Completion** | A design flaw where an orchestrator treats a successful request dispatch as proof of a committed mutation. | Leads to cascading failures when downstream steps depend on uncommitted state. |
| **Observation Object** | A normalized schema returning execution outputs, isolating the model from raw stack traces. | Simplifies syntax handling but still requires verification appropriate to its action class. |
| **Reconciliation** | The algorithmic evaluation of the verified actual state against the intended state to resolve discrepancies. | Determines whether the action succeeded, failed, or requires recovery. |
| **Read-After-Write Check** | An active query issued to confirm a mutation, designed to bypass replication lag. | Prevents stale reads from causing false-negative verification errors. |
| **Partial Failure** | A state where a multi-step or asynchronous action completes only a subset of its intended mutations. | Must be represented explicitly rather than being flattened into a binary success or failure. |
| **Rollback** | An operation that restores a system to its prior state within a single transactional boundary. | Preferred when available; aborts the active database transaction or restores a local file. |
| **Compensation** | An independent, idempotent transaction that logically reverses the visible effects of a committed transaction. | Used in distributed sagas when atomic rollbacks are technically impossible. |
| **Forward Recovery** | An SRE pattern that continues execution from a partial failure state toward the target goal. | Used when mutations are irreversible or when completing the remaining steps is preferred. |
| **Action Ledger** | An audit-grade, append-only log recording every phase of action execution, verification, and recovery. | Used for compliance, security forensics, and agent regression testing. |
| **Verification Depth** | The level of verification checks required for an action, scaling with its side-effect class. | Balances system latency against operational and security risks. |
| **Recovery Policy** | A deterministic set of rules mapping specific state discrepancies directly to defined recovery actions. | Eliminates unsupervised language model decision-making during failure recovery. |

## **The Core Doctrine of Post-Execution Truth**

The architectural foundation of action verification is post-execution truth management. Agentic systems must distinguish between what was planned, what was proposed, what was validated, what was attempted, what was observed, what was verified, what was reconciled, and what may safely be reported.

The lifecycle is:

```text
+--------------------------------------------------------------------------------
| POST-EXECUTION TRUTH LIFECYCLE
+--------------------------------------------------------------------------------
|
| [ Planned ]
|      |
|      v
| [ Proposed ]
|      |
|      v
| [ Validated ]
|      |
|      v
| [ Executed / Attempted ]
|      |
|      v
| [ Observed ]
|      |
|      v
| [ Verified ]
|      |
|      v
| [ Reconciled ]
|      |
|      v
| [ Reported ]
|
+--------------------------------------------------------------------------------
| Rule:
|   User-facing success may not outrun reconciliation.
|   Agent task state may not outrun verification.
|   Model belief may not outrun authoritative state.
+--------------------------------------------------------------------------------
```

### **Why Every Transition Matters**

| Transition | Failure Risk | Verification Requirement |
| :--- | :--- | :--- |
| **Planned -> Proposed** | The plan may be stale, underspecified, unauthorized, or semantically wrong. | Proposal must be derived from current task state and active autonomy boundary. |
| **Proposed -> Validated** | Model-generated arguments may be malformed, missing fields, unauthorized, or unsafe. | Tool contract validation must check syntax, schema, semantics, permissions, policy, budget, and confirmation state. |
| **Validated -> Executed** | A valid request can still fail due to timeouts, locks, dependency outages, rate limits, or worker crashes. | Execution attempt must be logged with idempotency key, trace ID, timeout policy, and wrapper version. |
| **Executed -> Observed** | A tool may return success, partial success, timeout, accepted, pending, or ambiguous result. | Observation must be normalized into a typed object and treated as evidence, not truth. |
| **Observed -> Verified** | The observation may not reflect durable authoritative state. | Verification must query the correct source of record or approved verification endpoint. |
| **Verified -> Reconciled** | Verified state may differ from intended outcome, requested payload, tenant scope, or side-effect expectations. | Reconciliation must evaluate predicates over authoritative state and classify discrepancies. |
| **Reconciled -> Reported** | User-facing status may falsely claim completion, or the agent may continue from a false belief. | Reporting and next-step planning must be grounded only in reconciled state. |

### **Observations Are Evidence, Not Reality**

Tool observations are structured evidence. They are not automatically authoritative truth.

For example:

* An email service may return `accepted`, but delivery may later bounce, quarantine, or fail.
* A payment processor may return `authorized`, but capture or settlement may remain pending.
* A deployment tool may return a rollout ID, but the rollout may later fail health checks.
* A database wrapper may return success, but a later verification query may reveal a version conflict, trigger failure, or wrong tenant boundary.
* A queue publisher may return accepted, but the downstream consumer may fail or dead-letter the message.

The orchestrator must therefore treat every observation as an intermediate state until verification determines what actually happened.

```text
ObservationObject.status = evidence
AuthoritativeReadback = verification source
ReconciliationResult = task-state update authority
```

### **Reporting Invariant**

A system may report:

| Verification State | Allowed User-Facing Status |
| :--- | :--- |
| **No execution attempt** | “The action has not been executed.” |
| **Execution attempted, no observation** | “The action was attempted; result is unknown.” |
| **Observation received, not verified** | “The action response was received; verification is pending.” |
| **Verified but not reconciled** | “The system verified current state; reconciliation is in progress.” |
| **Reconciled success** | “The action completed successfully.” |
| **Reconciled partial success** | “The action partially completed; these parts remain unresolved.” |
| **Reconciled failure** | “The action failed; no verified completion occurred.” |
| **Unknown / unverifiable** | “The final state is unknown and requires review.” |

The model must never convert an accepted, pending, timeout, or unverified observation into a completed user-facing success statement.

## **Hallucinated Success and Assumed Completion**

Hallucinated success is the central execution pathology of tool-using agentic systems. It occurs when a model, orchestrator, wrapper, or user-facing interface treats an intended, attempted, pending, failed, partial, or unverifiable action as successfully completed.

This failure does not always originate in model text generation. It is often an architectural failure: the system failed to separate execution from verification.

| Pattern Code | Pathology | Root Cause | Programmatic Mitigation |
| :--- | :--- | :--- | :--- |
| **HS-01** | **Phantom Execution** | The model claims a tool ran when no physical invocation occurred. | Require orchestrator-owned tool traces for every claimed action. |
| **HS-02** | **Validation Error Collapse** | A validation failure is summarized as a completed action. | Validation failure blocks execution and returns a typed error state. |
| **HS-03** | **Accepted-as-Completed** | An asynchronous `accepted` or `queued` status is treated as final. | Require polling, webhook, readback, or verification endpoint before completion. |
| **HS-04** | **Flattened Partial Success** | A multi-step action succeeds in one subsystem and fails in another, but is reported as complete. | Represent sub-action statuses explicitly and reconcile each required side effect. |
| **HS-05** | **Timeout Blindspot** | A timeout is assumed to mean failure or success without checking durable state. | Use idempotency keys and verification readback before retrying or reporting. |
| **HS-06** | **Uncontrolled Retry Mutation** | Retrying a mutating action creates duplicate side effects. | Enforce idempotency keys, duplicate detection, and payload-hash binding. |
| **HS-07** | **Stale-Replica Confirmation** | Verification reads from a lagging replica and confirms old state. | Use writer/primary reads, commit tokens, LSN checks, or consistency-bound read APIs. |
| **HS-08** | **Stale-Cache Read** | Verification hits an uninvalidated cache and sees pre-action state. | Bypass or invalidate caches during post-action verification. |
| **HS-09** | **Premature Propagation Success** | A tool returns success before downstream propagation completes. | Track pending states and poll until terminal authoritative state is reached. |
| **HS-10** | **Error Summarization Collapse** | A model interprets raw error text as successful completion. | Normalize tool errors into typed observation objects with explicit success flags. |
| **HS-11** | **Outrun Status Reporting** | The UI or agent reports completion before reconciliation finishes. | Lock user-facing success until the action ledger records reconciled success. |
| **HS-12** | **Wrong Target Success** | The action succeeds on the wrong account, tenant, file, region, or resource. | Verify target identity, tenant ID, resource ID, and permission scope during reconciliation. |
| **HS-13** | **Compensation Blindspot** | A compensating action is assumed to have reversed an earlier mutation without verification. | Verify compensation results and record compensated terminal state. |
| **HS-14** | **Unknown State Flattening** | An unverifiable or ambiguous result is collapsed into success or failure. | Preserve `UNKNOWN` / `UNVERIFIABLE` states and route to review or reconciliation. |

From an operational standpoint, hallucinated success corrupts the agent's belief state. Once an agent proceeds from false success, subsequent plans can compound the error: sending follow-up messages, making duplicate charges, skipping required remediation, or writing poisoned memory.

### **Completion Rule**

```text
A model statement is not completion.
A dispatch attempt is not completion.
An HTTP success code is not completion.
An accepted queue message is not completion.
A normalized observation is not completion.

Completion occurs only when authoritative state satisfies the required
verification predicates and the reconciled result is committed to task state.
```

## **State Reconciliation Model**

The State Reconciliation Model compares the intended outcome, requested operation, observed response, authoritative state, and believed task state. Its purpose is not to demand raw object equality. Real systems add IDs, timestamps, status transitions, derived fields, audit metadata, version numbers, provider-specific fields, and asynchronous states.

Therefore, action verification should evaluate **predicates over authoritative state**, not simplistic equality between intended state and full system state.

```text
+--------------------------------------------------------------------------------
| STATE RECONCILIATION MODEL
+--------------------------------------------------------------------------------
|
| [ Intended Outcome ]
|   user goal, plan target, expected business result
|        |
|        v
| [ Requested Operation ]
|   validated tool payload, target resource, tenant scope, idempotency key
|        |
|        v
| [ Observed Response ]
|   normalized tool observation: success, accepted, pending, error, timeout
|        |
|        v
| [ Authoritative State ]
|   source-of-record readback, ledger query, delivery status, deployment health
|        |
|        v
| [ Reconciliation Result ]
|   success, partial, mismatch, pending, duplicate, compensated, unknown
|        |
|        v
| [ Believed Task State ]
|   updated only from reconciliation result
|
+--------------------------------------------------------------------------------
| Rule:
|   Authoritative state is evaluated against verification predicates.
|   Believed task state is updated only after reconciliation.
+--------------------------------------------------------------------------------
```

Let:

| Symbol | Meaning |
| :--- | :--- |
| `SI` | Intended outcome. |
| `SR` | Requested operation. |
| `SO` | Observed tool response. |
| `SA` | Authoritative state. |
| `P` | Verification predicate set. |
| `SB` | Believed task state. |
| `RR` | Reconciliation result. |

A successful reconciliation is:

```text
RR = SUCCESS if all p in P evaluate true over SA, SR, SI, and scope metadata.
```

Examples of verification predicates:

```text
SA.resource_id == SR.resource_id
SA.tenant_id == SR.tenant_id
SA.status in allowed_terminal_success_states
SA.amount == SR.amount
SA.currency == SR.currency
SA.version > pre_action_version
SA.actor_id == expected_actor_or_service
SA.idempotency_key == SR.idempotency_key
SA.created_at >= action_execution_time
SA.audit_log contains trace_id
```

### **State Layers**

| State Layer | Source | Trust Level | Reconciliation Role |
| :--- | :--- | :--- | :--- |
| **Intended Outcome (`SI`)** | User goal, orchestrator plan, acceptance criteria. | Goal authority. | Defines what must become true. |
| **Requested Operation (`SR`)** | Validated tool payload. | Execution proposal authority. | Defines what was actually asked of the tool. |
| **Observed Response (`SO`)** | Tool response or observation object. | Evidence, not truth. | Triggers verification and provides hints. |
| **Authoritative State (`SA`)** | Source-of-record, primary ledger, provider verification endpoint, signed audit record. | Verification authority. | Determines what actually happened. |
| **Believed Task State (`SB`)** | Orchestrator state / agent memory. | Derived state. | May update only from reconciliation result. |

### **Observation Trust Ladder**

```text
+--------------------------------------------------------------------------------
| OBSERVATION TRUST LADDER
+--------------------------------------------------------------------------------
|
| Level 0: Raw Tool Output
|   stdout, stack traces, provider blobs, raw API errors
|   Status: unsafe for direct model reasoning
|
| Level 1: Normalized Observation
|   typed status, structured errors, trace ID, result payload
|   Status: safe evidence, not verified truth
|
| Level 2: Verified Observation
|   independent readback confirms that an event or resource exists
|   Status: execution likely occurred
|
| Level 3: Reconciled State
|   authoritative state satisfies required predicates
|   Status: safe to update task state
|
| Level 4: Audit-Confirmed State
|   reconciled state is bound to append-only ledger, approval records,
|   idempotency record, policy version, and trace evidence
|   Status: safe for regulated reporting and replay
|
+--------------------------------------------------------------------------------
```

### **Discrepancy Classification Model**

When authoritative state does not satisfy the predicate set, the reconciliation engine classifies the discrepancy.

| Discrepancy | Definition | Typical Recovery |
| :--- | :--- | :--- |
| **No-Op Success** | Target was already in desired state before the action. | Mark idempotent success if permitted; record no-op. |
| **No-Op Failure** | Requested mutation produced no state change when change was required. | Refresh state, retry if safe, or escalate. |
| **Value Mismatch** | Target changed but fields do not match required predicates. | Repair, compensate, or manual review. |
| **Stale State** | Target version changed between planning and execution. | Refresh and replan. |
| **Partial Application** | Some but not all required sub-actions succeeded. | Compensate, forward recover, or escalate. |
| **Duplicate Side Effect** | More than one side effect exists for one logical operation. | Freeze, reconcile ledger, compensate if possible. |
| **Wrong Target Modified** | Action affected wrong resource, tenant, region, user, or account. | Freeze and trigger security/incident response. |
| **Target Missing** | Resource does not exist or was deleted concurrently. | Replan or fail with missing-target state. |
| **Propagation Delay** | Authoritative commit exists but dependent systems lag. | Keep pending and retry verification. |
| **Unverifiable State** | Target system exposes no reliable verification path. | Hold, escalate, or require human attestation. |
| **Compensation Required** | Committed state must be reversed logically. | Execute compensation tool and verify compensation. |
| **Unknown State** | System cannot determine whether action succeeded. | Preserve unknown state and block duplicate mutation until resolved. |

## **Verification Depth by Side-Effect Class**

Verification depth should scale with side-effect class, data sensitivity, reversibility, cost, and user impact. Low-risk actions should not require heavyweight reconciliation. Critical mutations should never rely on a single transport success response.

| Side-Effect Class | Required Post-Action Check | Authoritative Verification Source | Fallback Behavior | Latency Tolerance | Human-Review Trigger |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Read-Only Observation** | Confirm request parameters, tenant scope, source identity, and response integrity. | Approved source endpoint, signed response, or permission-scoped read path. | Retry read, use alternate approved source, or return unavailable. | Low, usually synchronous. | Trigger if sensitive data boundary, tenant mismatch, or policy violation is detected. |
| **Ephemeral Write** | Verify sandbox path, file existence, size/hash, and no escape from sandbox. | Sandboxed filesystem monitor or workspace manifest. | Roll back local artifact or mark workspace dirty. | Low, synchronous. | Trigger on sandbox escape, unauthorized path, or destructive operation. |
| **Low-Risk Internal Write** | Read after write from source of record; verify row/object ID, tenant ID, version, actor, and changed fields. | Primary/writer database, authoritative service API, or strongly consistent read endpoint. | Retry verification; do not fall back to stale replica for final success. | Low to moderate. | Trigger after repeated verification mismatch or wrong-tenant detection. |
| **Medium-Risk Operational Write** | Verify before/after diff, workflow status, audit actor, and downstream queue state if applicable. | Application service API, primary database, workflow engine, or event ledger. | Keep pending, retry, compensate, or escalate based on discrepancy. | Moderate. | Trigger on partial success, stale state, or repeated mismatch. |
| **High-Risk External Write** | Verify provider transaction ID, delivery status, recipient/target, amount, idempotency key, and externally visible state. | Third-party ledger, provider verification API, gateway ledger, delivery status endpoint. | Hold pending; do not report success until verified. | Moderate to asynchronous. | Trigger on decline, mismatch, duplicate, unreachable provider, or verification timeout. |
| **Critical Mutation** | Multi-source reconciliation, approval verification, policy version check, audit ledger entry, and post-action verification. | Immutable ledger, security audit trail, consensus system, production controller, or regulated system of record. | Fail closed, hold, freeze affected resources, and require operator review. | Often asynchronous. | Always review before commit or immediately after if emergency break-glass policy was used. |

### **Verification Depth Rules**

```text
Read-only does not mean no-risk.
Low-risk write does not mean stale replica success.
External write does not mean provider-accepted success.
Critical mutation does not mean "retry and hope."
```

Verification strategy must be defined in the tool contract and recorded in the action ledger. If a tool has no verification path, the system must treat its result as unverifiable and constrain what the agent may claim or do next.

## **Post-Action Check Pattern Library**

Post-action verification patterns vary by target system. Each pattern defines how the system verifies that the intended result occurred in the correct resource, tenant, region, and authority scope.

### **1. Databases**

* **Validation Action:** Issue a read-after-write query against the writer, primary, or strongly consistent read endpoint. When available, use commit tokens, row versions, transaction IDs, or Log Sequence Numbers (LSNs) to avoid stale read ambiguity.
* **Verification Parameters:** Confirm target row exists, tenant ID matches, actor/service ID matches, row version advanced, changed fields satisfy predicates, and audit log contains the trace ID.
* **Failure Indicator:** Missing row, stale version, wrong tenant, mismatched field values, lock timeout, or verification read that cannot guarantee freshness.

### **2. Filesystems**

* **Validation Action:** Normalize and resolve the target path, then verify that the resolved path remains inside the allowed sandbox root.
* **Verification Parameters:** Confirm file exists, path is allowlisted, file size matches expected bytes, file hash matches payload, permissions are correct, and no symlink/path traversal escape occurred.
* **Failure Indicator:** Resolved path escapes sandbox, file missing, hash mismatch, unauthorized extension, unexpected permission mode, or destructive operation outside policy.

### **3. Email Services**

* **Validation Action:** Query provider status using returned message ID, gateway ID, or idempotency key.
* **Verification Parameters:** Confirm recipient, sender, subject/body hash when appropriate, queued/sent/delivered status, bounce status, suppression-list status, and DKIM/DMARC alignment where available.
* **Failure Indicator:** Bounce, rejection, quarantine, suppression, wrong recipient, duplicate send, or delivery state unknown beyond SLA.

### **4. Calendar Systems**

* **Validation Action:** Read the created or updated event by event ID from the authoritative calendar API.
* **Verification Parameters:** Confirm organizer, calendar ID, timezone, start/end time, attendees, recurrence, conferencing link, reminders, and event status.
* **Failure Indicator:** Missing event, wrong timezone, duplicate event, attendee invite failure, permission mismatch, or conflicting booking policy.

### **5. Payments and Refunds**

* **Validation Action:** Query payment processor, internal ledger, and idempotency record.
* **Verification Parameters:** Confirm transaction ID, idempotency key, amount, currency, account, status, authorization/capture/refund state, and ledger entry.
* **Failure Indicator:** Decline, duplicate charge, amount mismatch, pending beyond SLA, settlement failure, refund mismatch, or provider/local ledger divergence.

### **6. CRM and Ticketing**

* **Validation Action:** Fetch updated record and change history from authoritative CRM/ticketing API.
* **Verification Parameters:** Confirm object ID, tenant/account, field changes, assignee, status, comments, actor ID, workflow ID, and timestamp.
* **Failure Indicator:** Missing field update, wrong assignee, duplicate ticket, record lock, stale state, or missing change-history entry.

### **7. Deployment Pipelines**

* **Validation Action:** Query deployment orchestrator, rollout controller, health checks, and error-budget monitor.
* **Verification Parameters:** Confirm artifact SHA, environment, region, rollout stage, health checks, replica readiness, error rate, rollback status, and approval ID.
* **Failure Indicator:** Crash loop, failed readiness, rollback triggered, wrong SHA, partial-region rollout, error-budget burn, or missing deployment approval.

### **8. Procurement Systems**

* **Validation Action:** Query procurement register, approval workflow, vendor catalog, and budget ledger.
* **Verification Parameters:** Confirm PO ID, vendor, SKU, amount, currency, approval state, budget allocation, contract terms, and requester authority.
* **Failure Indicator:** Unauthorized vendor/SKU, price mismatch, budget breach, approval missing, duplicate PO, or rejected purchase.

### **9. Browser and Web Automations**

* **Validation Action:** Inspect page state after action using selectors, URL assertions, downloaded artifacts, and server-side confirmation when possible.
* **Verification Parameters:** Confirm success page, stable URL, expected DOM element, submitted values, downloaded file hash, and absence of error banner.
* **Failure Indicator:** Selector timeout, CAPTCHA, navigation mismatch, form validation failure, wrong account context, or file hash mismatch.

### **10. Security and Infrastructure**

* **Validation Action:** Query IAM, directory, policy engine, audit logs, and live token assertions.
* **Verification Parameters:** Confirm target principal, granted/removed scopes, policy diff, approval ID, actor ID, audit entry, and absence of overbroad wildcard permissions.
* **Failure Indicator:** Privilege escalation, wrong principal, missing audit event, wildcard policy, stale token, or policy mismatch.

### **11. Queue and Event Systems**

* **Validation Action:** Confirm message publication, consumer acknowledgement, dead-letter status, and downstream state transition.
* **Verification Parameters:** Confirm topic/queue, message ID, idempotency key, consumer group, processing status, and resulting state.
* **Failure Indicator:** Message stuck, duplicate event, dead-letter, consumer failure, out-of-order processing, or missing downstream mutation.

### **12. Vector Indexing and Search**

* **Validation Action:** Query index metadata and perform retrieval test using stable document ID or checksum.
* **Verification Parameters:** Confirm document ID, embedding version, index version, corpus snapshot, permission scope, and retrieval eligibility.
* **Failure Indicator:** Document missing, wrong embedding version, stale index, permission leakage, or retrieval mismatch.

Verification should be specific enough that a downstream auditor can answer: what was changed, where, by whom, under which authority, and how the system proved it.

## **Partial Failure Handling Model**

Distributed actions rarely fit a simple success/failure binary. A robust agentic architecture must represent intermediate, partial, pending, unknown, compensated, and review-required states explicitly.

### **Action State Machine**

```text
+--------------------------------------------------------------------------------
| ACTION VERIFICATION STATE MACHINE
+--------------------------------------------------------------------------------
|
| [ Proposed ]
|      |
|      v
| [ Validated ]
|      |
|      v
| [ Executing ]
|      |
|      +--> transport/tool failure before side effect -----> [ Failed ]
|      |
|      +--> accepted but not terminal ----------------------> [ Accepted ]
|      |                                                       |
|      |                                                       v
|      |                                                  [ Pending ]
|      |                                                       |
|      |                                      +----------------+----------------+
|      |                                      |                                 |
|      |                                      v                                 v
|      |                                [ Committed ]                 [ Verification Timeout ]
|      |                                      |                                 |
|      |                                      v                                 v
|      |                              [ Reconciled Success ]           [ Unknown / Review Required ]
|      |
|      +--> authoritative mismatch -------------------------> [ Reconciliation Failed ]
|      |                                                       |
|      |                         +-----------------------------+-----------------------------+
|      |                         |                                                           |
|      |                         v                                                           v
|      |                 [ Compensating ]                                             [ Forward Recovery ]
|      |                         |                                                           |
|      |              +----------+----------+                                     +----------+----------+
|      |              |                     |                                     |                     |
|      |              v                     v                                     v                     v
|      |       [ Compensated ]      [ Compensation Failed ]              [ Reconciled Success ]   [ Review Required ]
|      |
|      +--> multi-step partial commit -----------------------> [ Partially Committed ]
|                                                              |
|                                      +-----------------------+-----------------------+
|                                      |                                               |
|                                      v                                               v
|                                [ Compensating ]                              [ Forward Recovery ]
|
| Terminal states:
|   Reconciled Success
|   Failed
|   Compensated
|   Rolled Back
|   Review Required
|   Abandoned
|
+--------------------------------------------------------------------------------
```

### **Technical System States and Transitions**

| State | Meaning | Allowed Transitions |
| :--- | :--- | :--- |
| **Proposed** | Model or planner generated an action proposal. | Validated, Failed. |
| **Validated** | Tool contract checks passed. | Executing, Failed, Review Required. |
| **Executing** | Tool call is in flight. | Accepted, Pending, Committed, Failed, Unknown. |
| **Accepted** | Target accepted request but has not completed processing. | Pending, Failed, Unknown. |
| **Pending** | Verification is polling, waiting for webhook, or awaiting eventual consistency. | Committed, Verification Timeout, Failed, Unknown. |
| **Committed** | A side effect appears to have committed in authoritative system. | Reconciled Success, Reconciliation Failed, Partially Committed. |
| **Partially Committed** | Some required sub-actions committed and others did not. | Compensating, Forward Recovery, Review Required. |
| **Reconciled Success** | Authoritative state satisfies verification predicates. | Terminal. |
| **Reconciliation Failed** | Authoritative state does not satisfy intended predicates. | Compensating, Forward Recovery, Review Required, Failed. |
| **Failed** | Execution failed before a committed side effect, or failure is terminal. | Terminal unless retry policy re-enters Proposed/Validated. |
| **Verification Timeout** | Verification did not complete within SLA. | Unknown, Review Required, Compensating, Forward Recovery. |
| **Unknown** | System cannot determine whether action committed. | Review Required, Pending, Reconciliation Failed, Reconciled Success. |
| **Compensating** | System is executing a reversing action. | Compensated, Compensation Failed, Review Required. |
| **Compensated** | Compensation verified successfully. | Terminal recovery state. |
| **Compensation Failed** | Reversing action failed or state remains inconsistent. | Review Required. |
| **Forward Recovery** | System is completing remaining steps after partial/pivot state. | Reconciled Success, Review Required, Failed. |
| **Rolled Back** | Local transaction was aborted before external visibility. | Terminal recovery state. |
| **Review Required** | Automated recovery is unsafe or insufficient. | Terminal until human/operator resolves or reopens. |
| **Abandoned** | Workflow stopped with unresolved discrepancy explicitly recorded. | Terminal failure state. |

### **Partial Failure Rule**

```text
Committed is not Partially Committed.
Partially Committed is not Success.
Accepted is not Committed.
Unknown is not Failure.
Compensated is not the same as Rolled Back.
```

The state machine must preserve these distinctions because each state implies different user messaging, retry permissions, compensation options, and audit obligations.

## **Rollback, Compensation, and Forward Recovery**

When verification detects a discrepancy, the system must choose a recovery path based on the transaction boundary, side-effect class, reversibility, idempotency posture, and business risk.

```text
+--------------------------------------------------------------------------------
| RECOVERY STRATEGY MODEL
+--------------------------------------------------------------------------------
|
| [ Verification Discrepancy Detected ]
|      |
|      v
| [ Classify Transaction Boundary ]
|      |
|      +--> local atomic transaction open
|      |       |
|      |       v
|      |   [ Rollback ]
|      |       abort transaction | verify no visible side effect | ledger rolled_back
|      |
|      +--> compensable pre-pivot action
|      |       |
|      |       v
|      |   [ Backward Compensation ]
|      |       run compensation actions in reverse order | verify compensated state
|      |
|      +--> pivot committed / irreversible action
|      |       |
|      |       v
|      |   [ Forward Recovery ]
|      |       complete remaining idempotent steps | reconcile final state
|      |
|      +--> unknown or unverifiable high-impact state
|      |       |
|      |       v
|      |   [ Hold and Escalate ]
|      |       freeze workflow | preserve trace | route to operator
|
+--------------------------------------------------------------------------------
```

### **Rollback**

Rollback is preferred when the system still controls a local transaction boundary. It restores the prior state before intermediate changes become externally visible.

| Property | Rollback Behavior |
| :--- | :--- |
| Scope | Single database transaction, local filesystem transaction, isolated workspace mutation. |
| Visibility | Intermediate state should not be externally visible. |
| Trigger | Validation failure before commit, local write failure, transaction conflict. |
| Verification | Confirm transaction aborted and target state remains unchanged. |
| Terminal State | `ROLLED_BACK`. |

### **Compensation**

Compensation is used when a committed side effect cannot be rolled back atomically but can be logically reversed through another action.

| Property | Compensation Behavior |
| :--- | :--- |
| Scope | Distributed services, third-party APIs, microservice sagas, external records. |
| Visibility | Original action may have been visible to other systems. |
| Trigger | Partial failure before pivot, wrong value, canceled workflow, failed downstream step. |
| Verification | Confirm compensation action completed and resulting state is consistent. |
| Terminal State | `COMPENSATED` or `COMPENSATION_FAILED`. |

Compensation is not time travel. It creates a new event that reverses or offsets the prior event. Audits must show both.

### **Forward Recovery**

Forward recovery is used when rollback or compensation is impossible, unsafe, legally inappropriate, or more harmful than completing the workflow.

| Property | Forward Recovery Behavior |
| :--- | :--- |
| Scope | Post-pivot workflows, settlement systems, deployment pipelines, message queues. |
| Visibility | Prior side effect is committed or externally visible. |
| Trigger | Pivot committed, pending settlement, partial post-pivot failure, eventual consistency delay. |
| Verification | Continue idempotent retryable steps until final predicates hold or review is required. |
| Terminal State | `RECONCILED_SUCCESS` or `REVIEW_REQUIRED`. |

### **The Pivot Transaction**

The pivot transaction is the point of no return in a saga.

```text
[ Compensable Step 1 ]
      |
      v
[ Compensable Step 2 ]
      |
      v
[ Pivot Transaction ]
      |
      v
[ Retryable Step 3 ]
      |
      v
[ Retryable Step 4 ]
      |
      v
[ Reconciled Success ]
```

| Saga Region | Recovery Strategy |
| :--- | :--- |
| **Before pivot** | Backward compensation is usually available. |
| **At pivot** | Approval, idempotency, and verification must be strongest. |
| **After pivot** | Forward recovery is preferred; remaining steps should be retryable and idempotent. |

### **Recovery Strategy Matrix**

| Recovery Strategy | State Isolation | Timing | Cost Overhead | Transaction Bounds | Best Fit |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Atomic Rollback** | Strong. Intermediate state not visible. | Synchronous. | Low. | Single transaction boundary. | Database transaction, local sandbox write. |
| **Saga Compensation** | Weak. Intermediate state may be visible. | Usually asynchronous. | Medium. | Distributed services and third-party APIs. | Refund, delete uploaded object, reverse workflow update. |
| **Forward Recovery** | Weak but goal-directed. | Asynchronous or retry-driven. | Low to medium. | Eventually consistent systems. | Settlement, deployment rollout, queue delivery. |
| **Manual Review** | Human-contained. | Asynchronous. | High. | Complex or ambiguous transactions. | Unknown state, failed compensation, high-risk mismatch. |
| **Fail-Closed Hold** | Freezes action path. | Immediate hold, asynchronous resolution. | High throughput impact. | Critical financial/security/infrastructure mutations. | Possible tenant leak, duplicate charge, unauthorized access. |
| **Incident Creation** | Operational containment. | After severe or repeated failure. | High. | Production-critical failures. | Compensation failure, wrong-target mutation, systemic verifier outage. |

## **Recovery Decision Tables**

Recovery decisions must be deterministic. The model may explain a discrepancy, but recovery policy should map discrepancy type, side-effect class, transaction boundary, and operational condition to a predefined action.

| Discrepancy Type | Verification Finding | Active Operational Condition | Target Recovery Action | Technical Rationale |
| :--- | :--- | :--- | :--- | :--- |
| **Observation / Authority Mismatch** | Tool returned success, but authoritative readback shows old state. | Recent write and consistency delay within threshold. | **Retry Verification** | The state may still be propagating; do not report success yet. |
| **Observation / Authority Mismatch** | Tool returned success, but authoritative readback shows old state. | Read may have hit replica, cache, or stale endpoint. | **Force Primary / Strongly Consistent Read** | Verification must use source-of-record or consistency-bound endpoint. |
| **Observation / Authority Mismatch** | Tool returned success, but authoritative state still mismatches after strong read. | Mutation is reversible or compensable. | **Compensate or Roll Back** | The action did not produce required state. |
| **Observation / Authority Mismatch** | Tool returned success, but authoritative state mismatches after strong read. | Mutation is irreversible or high impact. | **Hold and Escalate** | Automated recovery is unsafe. |
| **Verification Timeout** | Polling or readback timed out. | Action is reversible and no pivot committed. | **Compensate and Stop** | Avoid indefinite pending state. |
| **Verification Timeout** | Polling or readback timed out. | Pivot committed or action irreversible. | **Hold and Escalate** | Unknown final state must not be retried blindly. |
| **Pending Timeout** | Execution timed out; idempotency record remains `PENDING`. | Durable idempotency record exists. | **Poll Durable Idempotency Record** | Prevent duplicate mutation while original attempt may still complete. |
| **Pending Timeout** | Execution timed out; no durable idempotency record exists. | Side-effecting operation. | **Freeze and Reconcile Manually** | Duplicate retry could create additional side effects. |
| **Partial Success** | Some sub-actions succeeded; others failed. | Workflow has not passed pivot. | **Backward Compensation** | Revert completed compensable steps. |
| **Partial Success** | Some sub-actions succeeded; others failed. | Workflow has passed pivot. | **Forward Recovery** | Complete remaining idempotent steps to reach consistency. |
| **Stale State Error** | Target version or ETag does not match requested version. | Optimistic locking failed. | **Refresh and Replan** | Another actor modified state; planner must use current state. |
| **Duplicate Side Effect** | More than one transaction exists for one logical operation. | Financial, security, external communication, or critical mutation. | **Freeze and Alert** | Duplicate execution may require incident handling and compensation. |
| **Duplicate Side Effect** | Duplicate found but operation is idempotent no-op. | Resulting state is equivalent and allowed by contract. | **Record Idempotent Replay** | No additional recovery needed, but trace must record duplicate. |
| **Wrong Target Modified** | Resource tenant, user, region, account, or object ID differs from authorized scope. | Any side-effecting action. | **Freeze and Alarm** | Potential security incident or data boundary violation. |
| **Unverifiable Result** | No reliable verification endpoint exists. | Low-risk or read-only operation. | **Report Unverified / Degraded** | Do not claim verified completion. |
| **Unverifiable Result** | No reliable verification endpoint exists. | High-risk write or critical mutation. | **Hold and Escalate** | High-impact actions require independent verification. |
| **Compensation Failure** | Compensation action failed or produced mismatch. | Any externally visible mutation. | **Manual Review / Incident** | State may remain inconsistent. |
| **Provider Outage** | Authoritative provider unavailable. | Action not yet executed. | **Defer or Fail Cleanly** | Avoid executing into unverifiable outage. |
| **Provider Outage** | Authoritative provider unavailable. | Action already executed or pending. | **Hold Pending and Retry Verification** | Avoid duplicate execution; preserve unknown state. |
| **Policy Version Mismatch** | Action was approved under old policy version. | Policy changed before execution. | **Revalidate Approval** | Approval may no longer authorize the action. |
| **Approval Token Mismatch** | Approval token payload hash does not match execution payload. | Any approval-gated action. | **Fail Closed** | Prevent approval replay or payload substitution. |

### **Recovery Selection Rule**

```text
Prefer rollback when state is local and uncommitted.
Prefer compensation when prior side effects are reversible.
Prefer forward recovery after pivot or irreversible commit.
Prefer hold-and-escalate when state is unknown, high-impact, or security-sensitive.
Never retry a mutating operation blindly.
```

## **Action Ledger and Auditability**

The Action Ledger is an append-only record of action intent, execution, verification, reconciliation, and recovery. Runtime traces describe service hops and latency. The Action Ledger describes what changed, what was proven, and how the system decided what to do next.

### **Action Ledger Entry Schema**

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://canon.ai-eng.org/v5/action-ledger-entry.schema.json",
  "title": "ActionLedgerEntry",
  "type": "object",
  "required": [
    "action_id",
    "workflow_run_id",
    "tenant_id",
    "principal_id",
    "tool_contract",
    "policy_context",
    "side_effect_class",
    "intended_outcome",
    "requested_operation",
    "execution",
    "verification",
    "reconciliation",
    "timestamps",
    "trace"
  ],
  "additionalProperties": false,
  "properties": {
    "action_id": {
      "type": "string"
    },
    "workflow_run_id": {
      "type": "string"
    },
    "tenant_id": {
      "type": "string"
    },
    "principal_id": {
      "type": "string"
    },
    "tool_contract": {
      "type": "object",
      "required": [
        "name",
        "version",
        "schema_version",
        "wrapper_version"
      ],
      "additionalProperties": false,
      "properties": {
        "name": {
          "type": "string"
        },
        "version": {
          "type": "string"
        },
        "schema_version": {
          "type": "string"
        },
        "wrapper_version": {
          "type": "string"
        }
      }
    },
    "policy_context": {
      "type": "object",
      "required": [
        "autonomy_boundary_version",
        "approval_policy_version",
        "verification_policy_version",
        "recovery_policy_version"
      ],
      "additionalProperties": false,
      "properties": {
        "autonomy_boundary_version": {
          "type": "string"
        },
        "approval_policy_version": {
          "type": "string"
        },
        "verification_policy_version": {
          "type": "string"
        },
        "recovery_policy_version": {
          "type": "string"
        }
      }
    },
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
    "idempotency": {
      "type": "object",
      "required": [
        "required",
        "key_hash",
        "request_hash",
        "status"
      ],
      "additionalProperties": false,
      "properties": {
        "required": {
          "type": "boolean"
        },
        "key_hash": {
          "type": [
            "string",
            "null"
          ]
        },
        "request_hash": {
          "type": [
            "string",
            "null"
          ]
        },
        "status": {
          "type": [
            "string",
            "null"
          ],
          "enum": [
            "PENDING",
            "COMPLETED",
            "FAILED_RETRYABLE",
            "FAILED_FINAL",
            "COMPENSATED",
            "EXPIRED",
            null
          ]
        }
      }
    },
    "approval": {
      "type": "object",
      "required": [
        "required",
        "approval_id",
        "approver_id",
        "payload_hash",
        "expires_at"
      ],
      "additionalProperties": false,
      "properties": {
        "required": {
          "type": "boolean"
        },
        "approval_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "approver_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "payload_hash": {
          "type": [
            "string",
            "null"
          ]
        },
        "expires_at": {
          "type": [
            "string",
            "null"
          ],
          "format": "date-time"
        }
      }
    },
    "intended_outcome": {
      "type": "object",
      "required": [
        "target_resource",
        "expected_predicates"
      ],
      "additionalProperties": false,
      "properties": {
        "target_resource": {
          "type": "string"
        },
        "expected_predicates": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      }
    },
    "requested_operation": {
      "type": "object",
      "required": [
        "validated_payload_hash",
        "target_resource",
        "operation_kind"
      ],
      "additionalProperties": false,
      "properties": {
        "validated_payload_hash": {
          "type": "string"
        },
        "target_resource": {
          "type": "string"
        },
        "operation_kind": {
          "type": "string"
        }
      }
    },
    "execution": {
      "type": "object",
      "required": [
        "status",
        "observation_pointer",
        "attempt_count"
      ],
      "additionalProperties": false,
      "properties": {
        "status": {
          "type": "string",
          "enum": [
            "NOT_EXECUTED",
            "EXECUTING",
            "ACCEPTED",
            "PENDING",
            "COMMITTED",
            "FAILED",
            "UNKNOWN"
          ]
        },
        "observation_pointer": {
          "type": [
            "string",
            "null"
          ]
        },
        "attempt_count": {
          "type": "integer",
          "minimum": 0
        }
      }
    },
    "verification": {
      "type": "object",
      "required": [
        "status",
        "source",
        "query_pointer",
        "verified_state_pointer"
      ],
      "additionalProperties": false,
      "properties": {
        "status": {
          "type": "string",
          "enum": [
            "NOT_REQUIRED",
            "NOT_STARTED",
            "PENDING",
            "VERIFIED",
            "FAILED",
            "TIMEOUT",
            "UNVERIFIABLE"
          ]
        },
        "source": {
          "type": [
            "string",
            "null"
          ]
        },
        "query_pointer": {
          "type": [
            "string",
            "null"
          ]
        },
        "verified_state_pointer": {
          "type": [
            "string",
            "null"
          ]
        }
      }
    },
    "reconciliation": {
      "type": "object",
      "required": [
        "status",
        "discrepancy_class",
        "recovery_decision"
      ],
      "additionalProperties": false,
      "properties": {
        "status": {
          "type": "string",
          "enum": [
            "NOT_STARTED",
            "RECONCILED_SUCCESS",
            "RECONCILED_PARTIAL",
            "RECONCILED_FAILURE",
            "UNKNOWN",
            "COMPENSATED",
            "ROLLED_BACK",
            "REVIEW_REQUIRED"
          ]
        },
        "discrepancy_class": {
          "type": [
            "string",
            "null"
          ]
        },
        "recovery_decision": {
          "type": [
            "string",
            "null"
          ]
        }
      }
    },
    "recovery": {
      "type": "object",
      "required": [
        "recovery_action_id",
        "compensation_action_id",
        "rollback_action_id",
        "incident_id",
        "review_id"
      ],
      "additionalProperties": false,
      "properties": {
        "recovery_action_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "compensation_action_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "rollback_action_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "incident_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "review_id": {
          "type": [
            "string",
            "null"
          ]
        }
      }
    },
    "timestamps": {
      "type": "object",
      "required": [
        "proposed_at",
        "validated_at",
        "executed_at",
        "verified_at",
        "reconciled_at"
      ],
      "additionalProperties": false,
      "properties": {
        "proposed_at": {
          "type": "string",
          "format": "date-time"
        },
        "validated_at": {
          "type": [
            "string",
            "null"
          ],
          "format": "date-time"
        },
        "executed_at": {
          "type": [
            "string",
            "null"
          ],
          "format": "date-time"
        },
        "verified_at": {
          "type": [
            "string",
            "null"
          ],
          "format": "date-time"
        },
        "reconciled_at": {
          "type": [
            "string",
            "null"
          ],
          "format": "date-time"
        }
      }
    },
    "trace": {
      "type": "object",
      "required": [
        "trace_id",
        "parent_span_id",
        "replay_bundle_id"
      ],
      "additionalProperties": false,
      "properties": {
        "trace_id": {
          "type": "string"
        },
        "parent_span_id": {
          "type": [
            "string",
            "null"
          ]
        },
        "replay_bundle_id": {
          "type": [
            "string",
            "null"
          ]
        }
      }
    }
  }
}
```

### **Trace versus Ledger**

| Artifact | Purpose | Storage Posture | Typical Consumer |
| :--- | :--- | :--- | :--- |
| **Runtime Trace** | Shows service hops, latency, errors, retries, spans, and dependencies. | High-volume telemetry store. | SRE, performance debugging, incident triage. |
| **Action Ledger** | Shows logical action state, verification proof, reconciliation, approval, and recovery. | Append-only or tamper-evident store. | Audit, compliance, replay, incident review, billing, governance. |

A trace answers: “What services did the request pass through?”

An action ledger answers: “What did the system intend to change, what actually changed, who approved it, how was it verified, and what recovery path executed?”

## **Tool-Result Grounding and Agent State Updates**

To prevent model belief drift, the orchestrator must separate the model's generation context from verified task state. The model may remember that it proposed or attempted an action. It may not treat that action as complete until reconciliation commits the result.

```text
+--------------------------------------------------------------------------------
| TOOL-RESULT GROUNDING MODEL
+--------------------------------------------------------------------------------
|
| [ Model Execution Context ]
|   "I should charge the card."
|      |
|      v
| [ Tool Proposal ]
|   proposed payload, not state truth
|      |
|      v
| [ Deterministic Wrapper ]
|   validates, authorizes, executes, observes
|      |
|      v
| [ Observation Object ]
|   evidence: success, error, accepted, pending, timeout
|      |
|      v
| [ Verification Layer ]
|   source-of-record readback, provider status, ledger query
|      |
|      v
| [ Reconciliation Result ]
|   success, partial, pending, failed, compensated, unknown
|      |
|      v
| [ Structured Task State ]
|   updated only from reconciliation result
|      |
|      v
| [ Next Model Context ]
|   injected with verified state, not raw self-belief
|
+--------------------------------------------------------------------------------
```

### **State Update Rules**

| Reconciliation Result | Task State Update | Model Context Injection | User-Facing Status |
| :--- | :--- | :--- | :--- |
| **Success** | Commit verified state and terminal success. | “Verified: action completed; authoritative state is...” | Completion may be reported. |
| **Partial Success** | Commit completed sub-actions and unresolved items. | “Partially verified: completed X; unresolved Y.” | Report partial completion with next steps. |
| **Pending** | Record pending state and verification schedule. | “Action accepted; verification pending.” | Do not claim completion. |
| **Failure** | Record failed state and reason. | “Action failed; no verified mutation occurred.” | Report failure or repair path. |
| **Compensated** | Record original action and verified compensation. | “Action was reversed through compensation.” | Report compensated recovery. |
| **Rolled Back** | Record rollback and unchanged target state. | “Transaction rolled back; no durable mutation.” | Report rollback. |
| **Unknown** | Record unknown state and block unsafe continuation. | “Final state unknown; duplicate action prohibited until reconciliation.” | Report pending review or unknown state. |
| **Review Required** | Freeze autonomous continuation. | “Human/operator review required.” | Report review status. |

### **Strict Status Reporting Invariant**

```text
If verification is pending, report pending.
If state is unknown, report unknown.
If action partially completed, report partial.
If compensation occurred, report compensated.
If success is reconciled, only then report complete.
```

### **Planning Adaptation**

After each verification result, the next planning step must be grounded in the reconciled task state.

| Verified State | Required Planning Behavior |
| :--- | :--- |
| **Success** | Continue to next dependent step. |
| **Partial** | Decide whether to compensate, forward recover, ask user, or escalate. |
| **Pending** | Poll, wait, subscribe to webhook, or defer. |
| **Failure** | Repair, retry if safe, or terminate. |
| **Unknown** | Do not retry mutating action blindly; reconcile first. |
| **Wrong Target / Tenant Drift** | Freeze and trigger security/incident path. |
| **Compensation Failed** | Escalate to manual recovery. |

The model's context should contain verified state summaries, trace pointers, and unresolved discrepancies. It should not contain raw logs that invite the model to invent a happy ending. The audit department has enough hobbies already.

## **Action Verification Observability and Regression Control**

Action verification must be observable, measurable, replayable, and regression-tested. A verification layer that silently fails is worse than no verification layer because it creates false confidence.

### **Platform Metrics**

| Metric | Formula / Measurement | What It Detects |
| :--- | :--- | :--- |
| **Verification Success Rate** | `verified_success_actions / executed_actions` | How often executed actions reconcile successfully. |
| **Verification Failure Rate** | `verification_failed_actions / executed_actions` | Frequency of authoritative-state mismatch. |
| **Verification Latency** | Time from execution observation to verification result. | Slow readbacks, provider lag, polling delay. |
| **Time to Reconcile** | Time from execution attempt to terminal reconciliation state. | End-to-end truth latency. |
| **Read-After-Write Failure Rate** | `read_after_write_failures / write_actions` | Replica lag, cache issues, writer/read routing bugs. |
| **Discrepancy Rate** | `reconciliation_mismatches / verified_actions` | Mismatch between intended/requested and authoritative state. |
| **Stale-State Rate** | `stale_state_conflicts / write_actions` | Optimistic-lock or concurrent update conflict. |
| **Partial Failure Rate** | `partial_actions / executed_actions` | Distributed transaction fragility. |
| **Pending-Too-Long Rate** | `pending_actions_over_sla / pending_actions` | Stuck async workflows or provider delay. |
| **Unknown-State Rate** | `unknown_state_actions / executed_actions` | Verification blind spots and risky uncertainty. |
| **Duplicate Action Rate** | `duplicate_side_effects / mutating_actions` | Idempotency failure or unsafe retry. |
| **Idempotency Recovery Rate** | `idempotency_replays_resolved / duplicate_attempts` | How well duplicate attempts are handled. |
| **Compensation Rate** | `compensations_started / mutating_actions` | Frequency of compensating recovery. |
| **Compensation Success Rate** | `compensations_verified / compensations_started` | Reliability of reverse actions. |
| **Failed Compensation Rate** | `compensation_failures / compensations_started` | Manual recovery and incident risk. |
| **False Success Escape Rate** | `false_success_reports / executed_actions` | Verification bypass reaching user or downstream planner. |
| **Human Escalation Rate** | `review_required_actions / executed_actions` | Manual review load and automation uncertainty. |
| **Action-Ledger Completeness** | `complete_ledger_entries / actions_requiring_ledger` | Auditability and replay coverage. |
| **Verifier Regression Rate** | Increase in mismatch/failure after verifier release. | Bad verification policy, schema drift, or release bug. |

### **Trace Linkage and Context Propagation**

Every action should link:

```text
model inference span
  -> tool validation span
    -> deterministic wrapper span
      -> tool execution span
        -> verification span
          -> reconciliation span
            -> recovery span, if needed
```

Required trace attributes include:

| Attribute | Purpose |
| :--- | :--- |
| `action.id` | Stable action identifier. |
| `workflow.run_id` | Parent agent run. |
| `tool.name` / `tool.version` | Tool contract identity. |
| `verification.policy_version` | Verification logic version. |
| `recovery.policy_version` | Recovery decision table version. |
| `side_effect_class` | Verification-depth selector. |
| `idempotency.key_hash` | Duplicate-action correlation. |
| `approval.id` | Approval linkage for gated actions. |
| `reconciliation.status` | Terminal or pending reconciliation state. |
| `discrepancy.class` | Mismatch classification. |
| `ledger.entry_id` | Audit ledger pointer. |

### **Regression Control and Release Gates**

Verification logic is production logic. Changes to predicates, polling rules, source selection, recovery policy, observation schema, or ledger schema can break agent behavior.

Release gates should include:

| Gate | Purpose |
| :--- | :--- |
| **Trace Replay** | Replay historical actions against new verification logic and compare reconciliation outcomes. |
| **Predicate Diff Review** | Show what verification predicates changed and which actions are affected. |
| **Verification Canary** | Roll out verifier changes to a small route or tenant subset first. |
| **Failure Injection** | Simulate stale reads, timeouts, duplicate actions, wrong-tenant results, and partial commits. |
| **Recovery Drill** | Confirm rollback, compensation, forward recovery, and review escalation paths execute. |
| **Ledger Schema Compatibility Check** | Ensure new ledger entries remain replayable and audit-compatible. |
| **False-Success Guard Test** | Verify user-facing completion cannot occur before reconciliation. |
| **Unknown-State Test** | Confirm unknown states block unsafe duplicate mutation. |

### **Release Invariant**

```text
No verifier release should promote unless historical trace replay,
canary metrics, failure injection, and ledger compatibility checks pass.
```

Action verification is not a passive logging layer. It is the truth-maintenance system that protects the agent from believing its own press release.

## **Cross-Canon Handoff Map**

The truth-management patterns established in AI-ENG-O provide critical interfaces across the AI Engineering Systems Canon. AI-ENG-O receives proposed and executed actions from orchestration and tool-contract layers, then returns verified, reconciled state to downstream planning, audit, governance, telemetry, evaluation, fallback, and incident systems.

| Target Document | Directed Asset / Metric | Programmatic Consumer | Downstream System Dependency |
| :--- | :--- | :--- | :--- |
| **AI-ENG-M — Agentic Orchestration** | Reconciled task state, pending states, unknown states, terminal action statuses. | Agent loop controller and planner. | Determines whether the agent may continue, retry, compensate, ask user, or terminate. |
| **AI-ENG-N — Tool Contracts** | Verification requirements, post-action hints, idempotency status, observation quality feedback. | Tool wrapper and contract registry. | Improves contract schemas, output objects, idempotency posture, and execution wrappers. |
| **AI-ENG-S — Production Pathologies** | Verification failure rates, discrepancy classes, false-success escapes, stuck pending actions. | Diagnostic monitors and anomaly detection engines. | Detects production pathologies in action execution and recovery. |
| **AI-ENG-T — Permission Boundaries** | Tenant verification tags, wrong-target mutations, permission-scope mismatches. | Security gateways and containment systems. | Enforces tenant boundaries and detects data leaks or unauthorized mutations. |
| **AI-ENG-U — Dependency Risk** | External provider verification latency, outage state, unreachable verification endpoints. | Dependency monitors and availability engines. | Drives fail-closed holds, degraded modes, and backup gateway policy. |
| **AI-ENG-V — Resource Abuse** | Duplicate-action metrics, polling loops, retry pressure, compensation rates. | Cost trackers and abuse-control systems. | Prevents runaway verification loops, duplicate side effects, and denial-of-wallet patterns. |
| **AI-ENG-W — Fallback & Degraded Modes** | Pending, unknown, unverifiable, and dependency-failure states. | Fallback routers and degraded-mode controllers. | Determines whether to degrade, defer, hold, or fail closed. |
| **AI-ENG-X — Transparent Status** | Verified status, partial status, unknown status, compensation status. | User messaging and status interfaces. | Prevents false user-facing success claims. |
| **AI-ENG-Y — Human Review** | Review-required states, high-risk mismatch packets, compensation failures. | Human review queues and approval systems. | Routes unresolved or risky states to human operators. |
| **AI-ENG-Z — Telemetry** | Verification spans, reconciliation status, discrepancy metrics, ledger completeness. | Observability stack and SRE dashboards. | Powers reliability monitoring and action truth dashboards. |
| **AI-ENG-AA — Agent Evaluations** | Trace replay packages, false-success tests, partial-failure scenarios. | Evaluation harnesses and CI/CD tests. | Tests whether agents behave correctly across real action outcomes. |
| **AI-ENG-AB — Audit & Replay** | Action ledger entries, verification evidence, recovery decisions, policy versions. | Replay engines and audit systems. | Reconstructs execution, verification, and recovery decisions. |
| **AI-ENG-AC — Incident Response** | Compensation failure codes, wrong-target mutations, duplicate side effects, unknown high-risk states. | Incident management and paging systems. | Triggers containment, rollback, escalation, and postmortem workflows. |
| **AI-ENG-AD — Governance, Policy, Compliance & Accountability** | Verification policy versions, approval evidence, audit boundaries, accountability records. | Governance, risk, and compliance owners. | Confirms that action truth, approval, recovery, and reporting comply with organizational policy. |
| **AI-ENG-AJ — Reference Architecture** | End-to-end verification lifecycle, state machine, ledger schema, recovery tables. | Systems development toolkits and SDKs. | Provides implementation templates for production agent platforms. |

The durable handoff is this:

```text
AI-ENG-O exports post-action truth:
what actually happened, how it was verified, whether it reconciled,
what recovery path executed, and what the agent may safely believe next.
```

## **Durable Architectural Principles**

To ensure reliability across tool-using agent implementations, systems must adhere to the following principles.

### **I. Decouple Execution from Verification**

An execution response is an acknowledgment of an attempt, not proof of completed state change. The system must treat the observed tool response as evidence. Task state remains locked until verification confirms the resulting authoritative state.

### **II. Verify Predicates, Not Naive Equality**

Authoritative state rarely equals intended state byte-for-byte. Verification should evaluate explicit predicates over resource ID, tenant scope, status, amount, version, actor, idempotency key, audit trail, and business-specific success conditions.

### **III. Bypass Lag and Caches During Verification**

Verification reads must use source-of-record, writer/primary, strongly consistent, or approved verification endpoints. Stale replicas and caches can produce false failures or false confirmations.

### **IV. Represent Intermediate States Explicitly**

Accepted, pending, partial, committed, compensated, rolled back, unknown, and review-required states must be represented explicitly. Flattening these states into success or failure corrupts planning and reporting.

### **V. Prevent Duplicate Side Effects with Idempotency**

Timeouts and retries must not create duplicate mutations. Side-effecting actions require idempotency posture proportional to risk, and unknown mutating states must block blind retries until reconciled.

### **VI. Use Deterministic Recovery Policies**

Failure recovery must be driven by policy-defined decision tables mapped to discrepancy classes, transaction boundaries, and side-effect classes. The model may assist with explanation, but it should not improvise rollback, compensation, or forward-recovery policy.

### **VII. Treat Unknown State as a First-Class State**

Unknown is not success. Unknown is not failure. Unknown means the system cannot safely claim completion or repeat the mutation until reconciliation, review, or recovery establishes truth.

### **VIII. Ground Model Context in Reconciled State**

The model's next planning step must receive verified task state, unresolved discrepancy state, and trace pointers. It must not proceed from historical generation logs or assumed completion.

### **IX. Do Not Let User-Facing Status Outrun Truth**

A user-facing success message may be emitted only after reconciliation. Pending, partial, compensated, failed, and unknown states must be reported honestly.

### **X. Make Verification Replayable and Auditable**

Action verification must produce ledger entries, trace spans, policy versions, verification evidence, recovery decisions, and terminal states sufficient for audit, replay, incident response, and regression testing.

### **XI. Verification Logic Is Release Logic**

Changes to verification predicates, recovery tables, ledger schemas, or polling policies must pass replay, canary, failure-injection, and compatibility gates before production promotion.

The final invariant:

```text
The agent may act only through contracts.
The system may believe only verified state.
The user may be told only reconciled truth.
```

---