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