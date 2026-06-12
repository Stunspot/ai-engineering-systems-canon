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

The Autonomy Boundary Model defines the operational limits of what a model can observe, decide, execute, or change at runtime.2 Autonomy is an adjustable dial that must be dynamically calibrated based on the underlying task risk, the sensitivity of the data, and the presence of human oversight.6

```
┌────────────────────────────────────────────────────────────────────────┐  
│                        AUTONOMY BOUNDARY TIER                          │  
├────────────────────────────────────────────────────────────────────────┤  
│                                                                        │  
│  - Network sandboxes, IAM token validation, gateway rate-limiting      │  
├────────────────────────────────────────────────────────────────────────┤  
│  [ Asynchronous Human Audit Queues ]                                   │  
│  - Post-action verification, log audits, compliance signatures         │  
├────────────────────────────────────────────────────────────────────────┤  
│  [ User / Operator Confirmation Gates ]                                │  
│  - Active approval buttons, budget releases, path validation checks    │  
├────────────────────────────────────────────────────────────────────────┤  
│                                                                        │  
│  - Immutable schema validators, parameter boundary checkers, ACLs      │  
├────────────────────────────────────────────────────────────────────────┤  
│                                                                        │  
│  - Dynamic step selection, sub-query generation, argument proposals    │  
└────────────────────────────────────────────────────────────────────────┘
```

The specific authority boundaries across different system layers are modeled as follows:

| Boundary Dimension | Model Deliberative Authority | Deterministic System Policy | User Approval Policy | Serving Layer Enforcement |
| :---- | :---- | :---- | :---- | :---- |
| **Allowed Observations** | Select which local document index to query.1 | Filter retrieved context for PII and redact sensitive strings.12 | Confirm manual expansion of document search scope.1 | Restrict network requests to localhost using IAM tokens.6 |
| **Allowed State Writes** | Append step results to the active run scratchpad.7 | Block writes that violate task schema properties.13 | Approve checkpoint saves for long-running workflows.4 | Enforce absolute segment separation of database connections.6 |
| **Allowed Tools** | Propose calling registered local search tools.1 | Map proposed tools against the allowed execution list.11 | Manually authorize write-capable tool invocations.6 | Terminate execution if tool lacks valid credentials.6 |
| **Resource Budgets** | Project remaining cost using Progressive Estimation.8 | Monitor step counters and track token usage rates.7 | Approve manual increases to financial or step caps.7 | Sever connection if container exceeds wall-clock limits. |
| **Escalation Paths** | Flag low-confidence outputs to request help.4 | Halt execution and trigger escalation on validator failure.7 | Choose whether to override, edit, or terminate the plan.1 | Log a high-priority incident and page the active SRE team.18 |

### **Context and Boundary Adjustments by Risk Profile**

Autonomy boundaries are not static; they change based on the risk profile of the target domain 6:

* **Research Assistant:** High read-only autonomy.6 The model may browse, retrieve, and summarize documents without external side effects.1  
* **Coding Assistant:** Bounded write autonomy.6 The model may modify files in a local workspace but is prohibited from pushing changes or deploying to production until all unit tests pass and a senior engineer signs off on the pull request.1  
* **Customer Support Agent:** Bounded action autonomy.6 The model may search documentation and draft replies but requires human review for account adjustments, refunds, or cancellations.6  
* **Procurement Agent:** High financial risk.6 The model may identify vendors and compare contract items but requires explicit supervisor approval before executing purchases.6  
* **Incident Remediation SRE Agent:** High operational risk.6 The model can query performance logs and metrics but cannot reboot servers or alter infrastructure configs without human approval.6

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
| **10. Obs Grounding** | Ingest tool outputs and verify factual grounding.4 | Raw Tool Payload; system state. | Reject model-hallucinated or assumed tool outcomes.5 | Grounded Observation.5 |
| **11. State Update** | Commit observation to the task state.4 | Grounded Observation; Active State.13 | Commit updates to the versioned state database.13 | Updated Task State.13 |
| **12. Validation Check** | Evaluate step quality against acceptance criteria.5 | Updated Task State; criteria list.7 | Score outputs for compliance and semantic accuracy.5 | Validation Score Card.5 |
| **13. Reflection Trigger** | Diagnose failures and trigger corrections if needed.7 | Validation Score Card; state logs. | Limit repair attempts to prevent looping.7 | Diagnostic Repair Plan.7 |
| **14. Termination Check** | Evaluate stopping and completion criteria.7 | Active Task State; Budget Status.8 | Match progress against termination conditions.7 | Stop / Continue Code. |
| **15. Escalation Check** | Pause loop and escalate to supervisor if triggered.5 | Validation failures; budget alarms. | Verify if state warrants immediate human review.5 | Escalation Event Payload.6 |
| **16. Final Response** | Compile, format, and return final output. | Validated Task State; target goal. | Ensure output is grounded in recorded observations.1 | Verified User Response.1 |
| **17. Memory Write** | Update long-term memory via write gates.3 | Run Trace; current Memory DB.1 | Filter out transient failures and assumptions.3 | Promoted Memory Block.3 |
| **18. Trace Logging** | Archive the complete execution trace for audits.1 | Versioned Task State; metric logs. | Write immutable execution records to disk.1 | Audit Trace Archive.1 |

### **Observation Grounding: Intended, Attempted, Observed, Verified, and Assumed Results**

At the orchestration level, observation grounding is a vital guard against state contamination.5 A failure occurs when an agent treats its *intended* or *attempted* actions as *assumed* realities without waiting for the physical tool execution to return a response.5 The orchestrator must implement strict state boundaries separating execution phases:

```
  - Model drafts a tool call (e.g., write database record)  
     │  
     ▼  
 - Orchestrator sends request to the target API endpoint  
     │  
     ▼  
  - System parses raw bytes returned from the endpoint  
     │  
     ▼  
  - Validator checks if the record was successfully written
```

The task state must only be updated using *verified* results.4 If a tool call fails, timeout errors, connection exceptions, or empty payloads must be recorded as-is.5 The model is strictly prohibited from bypassing this return parsing and assuming successful execution.5

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

Open-ended loops are powerful but introduce high variance and unconstrained execution risk.2 To ensure enterprise-grade reliability, autonomy must be structured within the rigid skeleton of a compiled state machine or workflow graph.6

```
                     ┌──────────────────────────────┐  
                     │          Goal Intake         │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │         Classify Node        │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │          Scope Node          │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │          Plan Node           │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │        Retrieve Node         │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │        Call Tool Node        │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                     ┌──────────────────────────────┐  
                     │         Observe Node         │  
                     └──────────────┬───────────────┘  
                                    │  
                                    ▼  
                       ┌────────────┴────────────┐  
             Incomplete│        Validate         │   Complete  
            ┌──────────┤          Node           ├──────────┐  
            │          │                         │          │  
            ▼          └─────────────────────────┘          ▼  
┌────────────────────────┐                     ┌────────────────────────┐  
│      Repair Node       │                     │     Escalate Node      │  
└───────────┬────────────┘                     └────────────┬───────────┘  
            │                                               │  
            ▼                                               ▼  
┌────────────────────────┐                     ┌────────────────────────┐  
│      Reflect Node      │                     │     Finalize Node      │  
└───────────┬────────────┘                     └────────────┬───────────┘  
            │                                               │  
            ▼                                               ▼  
┌────────────────────────┐                     ┌────────────────────────┐  
│     Ask User Node      │                     │     Terminate Node     │  
└────────────────────────┘                     └────────────────────────┘
```

The specific graph components are modeled as follows:

| Graph Component | System Role / Node Mapping | Allowable Transitions (Edges) | Guard Conditions |
| :---- | :---- | :---- | :---- |
| **Goal Classify** | Determine intent and assign to worker agent.6 | Classify -> Scope.6 | User query must be successfully parsed. |
| **Scope Resolution** | Set resource and security parameters.1 | Scope -> Plan.1 | Active user session token must be valid.6 |
| **Plan Task** | Draft step sequence for execution.4 | Plan -> Retrieve.14 | Step count must be within initial limits.7 |
| **Retrieve Context** | Pull relevant vector knowledge to state.1 | Retrieve -> Call Tool.1 | Context window usage must be under 80%.7 |
| **Call Tool** | Dispatch request to target API.4 | Call Tool -> Observe.5 | Tool must match active user permissions.6 |
| **Observe Result** | Capture raw payload returned by the tool.4 | Observe -> Validate.5 | Tool response must be parsed and recorded.5 |
| **Validate Node** | Verify outcomes against criteria.5 | Validate -> Finalize (if valid); Validate -> Repair (if invalid).5 | Evaluator must confirm factual grounding.5 |
| **Repair Node** | Execute local code or query fixes.5 | Repair -> Reflect.5 | Cumulative step cost must be under limits.8 |
| **Reflect Node** | Diagnose step errors to correct plan.7 | Reflect -> Plan.12 | Reflection loop count must be <= 2.7 |
| **Ask User** | Prompt operator for inputs.7 | Ask User -> Plan.7 | System must suspend execution until input is provided. |
| **Escalate Node** | Route state to human supervisor.5 | Escalate -> Human Review.6 | Triggered on policy violations or budget alarms.8 |
| **Finalize Node** | Compile complete task outputs.13 | Finalize -> Terminate.13 | Validator must verify final outputs.7 |
| **Terminate Node** | Close run, write logs, release resources.4 | End of execution path.13 | Active process state is cleaned up.13 |

### **Graph Compilation and State Transitions**

Before execution, the graph layout is compiled and validated to detect circular dependencies, unresolvable nodes, or deadlocks.13 Once compiled, the graph structure is immutable; the model cannot dynamically inject new states or rewrite transition rules during runtime.13

```
┌────────────────────────────────────────────────────────┐  
│             GRAPH COMPILATION CHECKPOINT               │  
├────────────────────────────────────────────────────────┤  
│  1. Check for circular loops without exit nodes        │  
│  2. Verify all edge transitions are mapped to guards   │  
│  3. Confirm every node has an assigned budget gate     │  
│  4. Ensure terminal nodes are unreachable from halts   │  
└────────────────────────────────────────────────────────┘
```

At each transition, state is saved to a persistent, versioned database (such as PostgreSQL or Redis).13 This design isolates failures, prevents race conditions, and allows long-running or interrupted workflows to recover exactly where they stalled without losing context.4  
Several common multi-agent topology patterns present distinct operational characteristics:

* **Planner-Executor:** A highly capable, high-latency model generates the static execution plan, and a lower-cost, highly responsive executor processes individual steps.12 This layout reduces cost but suffers from plan drift when the environment changes mid-run.12  
* **Supervisor-Worker:** A central supervisor routes execution steps dynamically to highly specialized sub-agents.14 This model restricts the blast radius of sub-agents by exposing only role-specific tools, but can result in the supervisor becoming a processing bottleneck.13  
* **Specialist Swarms:** Multiple peer agents communicate directly or post tasks to a shared blackboard.12 This layout provides maximum flexibility, but introduces high risk of coordination failure, state corruption, and runaway token consumption.12

## **Loop Budgets, Tool Budgets, and SRE Timeout Policies**

Operational budget enforcement is the single most critical line of defense against runaway agentic behavior.8 Real-world incidents demonstrate that unconstrained agentic loops can enter high-frequency retry cycles when encountering minor API failures, generating tens of thousands of dollars in model invoice costs in a matter of hours.8  
The orchestrator must implement a formal Loop Budget Framework that treats budget metrics as active control signals rather than post-hoc reports.8  
Let an agent A operate on a task T within an environment E.8 The total computational budget B allocated to the run is defined as:  
B = B_internal + B_external  
where B_internal is the internal cognitive budget consumed by model inference (measured in input and output tokens), and B_external is the external execution budget consumed by environmental actions (measured in tool executions, database transactions, or monetary cost).8  
The remaining budget R_t at any discrete execution step t is formulated as:  
R_t = B - sum(C_i for i = 1 to t)  
where C_i represents the verified cost incurred at step i.8  
To prevent agents from spending resources on hopeless execution trajectories, the orchestrator should run a Progressive Interval Estimation (PIE) protocol.8 At each step t of the execution plan, the system queries the agent (or a specialized budget-estimator model) to predict a confidence-aware interval representing the lower (L) and upper (U) bounds of remaining cost-to-completion 8:  
PIE_t = [L_t, U_t]  
The orchestrator triggers a priority alert and initiates early termination (A_t = 1) the moment the remaining budget is lower than the projected lower bound of completion cost 8:  
A_t = 1 if R_t < L_t  
This proactive halting mechanism saves between 28% and 64% of total token and execution expenditures on failed trajectories.8  
Every agentic loop must be subjected to a multi-tiered budget cap matrix, with default thresholds adapted to the task risk profile:

| Budget Type | Metric Unit | Default Cap: Research Assistant | Default Cap: Code Workspace | Default Cap: Incident SRE Agent | Infrastructure Enforcement Guard |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Step Budget** | Integer count | 15 steps.11 | 30 steps. | 10 steps. | Model-side increment tracker halts execution loop. |
| **Tool-Call Budget** | Integer count | 30 calls.12 | 50 calls. | 15 calls. | Gateway proxy interceptor blocks further tool dispatches.24 |
| **Token Budget** | Input/Output tokens | 5,000,000 tokens.11 | 8,000,000 tokens. | 1,000,000 tokens. | Gateway drops requests once session quota is exceeded.18 |
| **Wall-Clock Budget** | Seconds | 300 seconds.11 | 600 seconds. | 120 seconds. | SRE connection timeout drops the container.4 |
| **Inference Budget** | Monetary USD | $2.00.11 | $5.00. | $1.00. | Router blocks further API calls on token credit drain.8 |
| **Retry Budget** | Consecutive attempts | 3 attempts.5 | 5 attempts. | 2 attempts. | Gateway blocks identical requests to prevent retry storms.5 |
| **Context Growth** | Token count | 100,000 tokens.7 | 200,000 tokens. | 50,000 tokens. | Orchestrator prunes history if context exceeds 80% limit.7 |
| **Memory-Write Budget** | Integer commits | 2 database writes.3 | 5 database writes. | 0 database writes. | Memory database restricts write access during active runs.3 |
| **External API Budget** | Integer calls | 5 calls. | 10 calls. | 2 calls. | Rate-limiters enforce strict endpoint quotas per session.11 |
| **Side-Effect Budget** | Monitored mutations | 0 mutations. | 2 local file updates. | 0 mutations. | Sandbox blocks any unauthorized write operations.6 |

To enforce these budgets at the systems level, SREs must configure the serving gateway with strict timeout policies.18 This includes deploying circuit breakers that detect repeated identical tool calls (such as 3+ redundant executions of the same endpoint), rate-limiting tool dispatch, and applying exponential backoff on connection retry storms to prevent models from hammering external microservices.18

## **Scratchpads, Memory, and State Governance**

Uncontrolled state growth is a primary driver of agentic degradation.7 As an agent loop executes, the accumulation of raw tool outputs, intermediate planning drafts, and validation critiques causes context bloat.7 This context accumulation degrades the model's performance, increases latency, and elevates the risk of retrieval-related omissions. Systems must implement a rigorous state governance model that separates and manages memory along strict lines of tenure and authority.

```
┌────────────────────────────────────────────────────────────────────────┐  
│                       STATE GOVERNANCE PLATFORM                        │  
├────────────────────────────────────────────────────────────────────────┤  
│                                                                        │  
│  - Temporal working memory      - Machine-readable tracking table      │  
│  - Discarded on step change     - Persists over entire active run      │  
├─────────────────────────────────┴──────────────────────────────────────┤  
│  [ Persistent Memory ]                                                 │  
│  - Promoted via write gates     - Immutable execution record           │  
│  - Long-term storage (DB)       - Read-only; SRE / Auditor access      │  
└────────────────────────────────────────────────────────────────────────┘
```

The specific properties of each state component are defined as follows:

| Memory / State Category | Tenure and Lifespan | Access Permissions | Storage Medium | State Tenure and Update Rules |
| :---- | :---- | :---- | :---- | :---- |
| **Ephemeral Scratchpad** | Short-term; scoped to active step.7 | Model: Read/Write.7 | Local memory cache.7 | Discarded or compressed the moment the active plan step transitions to completed.7 |
| **Structured Task State** | Medium-term; active run lifespan.4 | System: Read/Write; Model: Read.13 | Redis / PostgreSQL.15 | Maintained throughout the run; updated using immutable, versioned schemas.13 |
| **Persistent Memory** | Long-term; spans user sessions.1 | System: Read/Write; Model: Read.1 | Vector Database.1 | Direct model writes are blocked; updates require post-run verification gates.3 |
| **Retrieved Evidence** | Step-level; cached during active run.1 | Model: Read-only.1 | Temp memory cache.1 | Retained for step evaluation; cleared during context compression passes.7 |
| **Tool Observations** | Step-level; cached during active run.4 | System: Read/Write; Model: Read.5 | Versioned run log.13 | Ingested directly from tool APIs; cannot be edited by the model.5 |
| **Execution Trace** | Permanent; archived after run completion.1 | Read-only access for auditing.1 | Secure document store.1 | Written once at step transitions; serves as the execution record.1 |
| **Audit Record** | Permanent; compiled after termination.1 | Read-only access for compliance.1 | WORM storage drive.1 | Immutable history of execution; remains unalterable.1 |

### **State Contamination and Context Compression**

State contamination occurs when an agent references stale observations or unverified assumptions in subsequent plan steps.3 To prevent this, the orchestrator must enforce context compression limits.7 If active context usage exceeds 80% of the model's limit, the system pauses execution, runs a compression node to summarize progress, retains only the active plan and grounded facts, and discards historical planning drafts before resuming.7

## **Tool Selection Policy Model**

The orchestrator, not the model, is the ultimate authority governing tool dispatch.6 To prevent tool overreach and uncontrolled executions, the system must apply a rigorous selection and validation policy.11

```
┌────────────────────────────────────────────────────────────────────────┐  
│                        TOOL SELECTION PLATFORM                         │  
├────────────────────────────────────────────────────────────────────────┤  
│                          1. Model Suggestion                           │  
│     - Model requests tool call with parameter arguments                │  
└───────────────────────────────────┬────────────────────────────────────┘  
                                    ▼  
┌────────────────────────────────────────────────────────────────────────┐  
│                        2. Schema Serialization                         │  
│     - Validates types, ranges, and structures of parameters            │  
└───────────────────────────────────┬────────────────────────────────────┘  
                                    ▼  
┌────────────────────────────────────────────────────────────────────────┐  
│                         3. Security Validation                         │  
│     - Verifies session tokens, user boundaries, and sanitizes strings  │  
└───────────────────────────────────┬────────────────────────────────────┘  
                                    ▼  
┌────────────────────────────────────────────────────────────────────────┐  
│                          4. Policy Interceptor                         │  
│     - Evaluates write-safety and budget checks                         │  
└───────────────────────────────────┬────────────────────────────────────┘  
                                    ▼  
                            ┌───────┴───────┐  
                     Pass   │  Gatekeeper   │   Fail  
                 ┌──────────┤  Evaluation   ├──────────┐  
                 │          │               │          │  
                 ▼          └───────────────┘          ▼  
┌─────────────────────────────┐            ┌─────────────────────────────┐  
│       TOOL DISPATCH         │            │      BLOCK & RETRY/HALT     │  
│  - System executes action   │            │  - Drops request immediately│  
└─────────────────────────────┘            └─────────────────────────────┘
```

The system evaluates the eligibility of a tool using several criteria:

| Tool Category | Allowed Actions | Permission Scope | Side-Effect Risk | Latency & Cost | Grounding & Verification Check |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Read-Only Lookup** | Document searches; system diagnostic logs.1 | Standard User Role.1 | None.6 | Low latency; low token cost.21 | Match output strings against index signatures. |
| **Retrieval Vectors** | Querying corporate vector databases.1 | Active Session Scope.1 | None.6 | Low latency; moderate token cost.21 | Verify retrieval source metadata.1 |
| **Live API Lookups** | Fetching active exchange rates, prices, or orders. | Authorized API Key.6 | None. | High latency; moderate monetary cost.11 | Enforce schema parsing on returned payloads.5 |
| **Calculators** | Arithmetic calculations; matrix operations. | Local Sandbox Scope. | None. | Low latency; zero token cost. | Run computations on deterministic code engines. |
| **File System** | Read workspace files; write local diagnostic logs. | Workspace sandbox.23 | Moderate; local file changes.23 | Low latency; low cost. | Verify file signatures before and after changes.4 |
| **Database Queries** | Querying local workspace schemas. | Read-Only database connection.6 | None. | Low latency; low cost. | Parse queries against SQL whitelist filters. |
| **Email/Calendar** | Draft messages; check schedule calendars.6 | User Session Scope.6 | High; external communications.6 | High latency; low cost. | Require human sign-off before sending.6 |
| **Procurement API** | Check item costs; propose invoice releases.6 | Secure Billing API Scope.6 | High; financial transactions.6 | High latency; high monetary cost.11 | Block execution until supervisor approves.6 |
| **Review Queues** | Route tasks to manual inspection queues.6 | System Level Admin Scope.6 | None. | Bounded by human queue latency. | Log compliance signatures on tasks. |

### **Cognitive Emulation vs. Explicit Tool Execution**

Models are prone to calculation errors, factual hallucinations, and spatial reasoning errors when generating raw tokens sequentially.11 To mitigate this, system architectures must prioritize *explicit tool execution* over *cognitive emulation*.11 The model must never calculate values or fetch records through raw generation; it must call deterministic calculators, structured code interpreters, or database search APIs to obtain precise values.11

## **Termination and Escalation Framework**

An agentic loop must not be allowed to run until it self-terminates based on internal logic. Termination and escalation are formal programmatic contracts that determine when a run must cease and how the system should handle edge-case failures.6  
Every agentic workflow must explicitly configure and monitor conditions across a comprehensive catalog:

| Termination Code | System Classification | Underlying Root Cause / Trigger | Required Orchestrator Action |
| :---- | :---- | :---- | :---- |
| **SUCCESS** | Normal | Output meets all validation and safety criteria.7 | Closes task state, writes trace logs, and returns output.4 |
| **PARTIAL_SUCCESS** | Graceful Degradation | Primary goal achieved, but secondary actions timed out.5 | Compiles completed steps and returns partial output.11 |
| **IMPOSSIBLE** | Explicit Handback | Model determines required input fields are missing.7 | Stops execution and prompts user for missing inputs.7 |
| **BUDGET_EXHAUSTED** | System Intercept | Accumulated run costs exceed step or monetary caps.7 | Terminates active execution threads and logs budget breach.5 |
| **TIMEOUT** | System Intercept | Execution duration exceeds wall-clock bounds.4 | Drops container execution and alerts active SRE teams.4 |
| **UNSAFE_DETECTION** | Safety Guard | Content scanner flags injection or policy violation. | Halts execution, blocks the session, and triggers security alarm. |
| **MISSING_INFO** | Explicit Handback | System detects missing parameters during planning.7 | Switches node to Ask User state and awaits input.7 |
| **AMBIGUOUS_INTENT** | Explicit Handback | User query maps to multiple conflicting plans.7 | Stops execution and requests plan verification.1 |
| **LOW_CONFIDENCE** | Quality Scorer Halt | Verification score drops below safety threshold.5 | Suspends task state and routes to supervisor queue.22 |
| **REPEATED_FAILURE** | Resilience Failure | Tool calls return exceptions after maximum retries.5 | Trips the circuit breaker and triggers escalation.5 |
| **SOURCE_CONFLICT** | Validation Scorer Halt | Retrieved document facts directly contradict. | Freezes progress and logs the discrepancy for review.5 |
| **VALIDATION_FAIL** | Quality Scorer Halt | Output fails format or schema compliance checks.5 | Runs 1 repair pass; escalates if failure persists.5 |
| **PERMISSION_DENIED** | Security Guard | Model attempts to access an unauthorized tool.24 | Blocks the active run and registers a security ticket.24 |
| **UNAVAILABLE_DEP** | System Intercept | Vital external API endpoint is offline.5 | Switches system to degraded mode using static fallbacks.5 |
| **CONFIRM_REQUIRED** | Human-in-the-Loop | Model proposes executing a write-capable tool.6 | Suspends execution and presents parameters to operator.4 |
| **REVIEW_REQUIRED** | Human-in-the-Loop | Task outcome falls under compliance auditing.22 | Routes final plan and outputs to asynchronous audit queue.22 |
| **USER_CANCEL** | Explicit Handback | Operator manually aborts the execution run.7 | Halts container and archives the execution history.4 |

When these termination parameters are triggered, or when high-risk operations are requested, the orchestrator maps the current state to the appropriate remediation path using an Escalation Trigger Matrix:

```
                          ┌────────────────────────┐  
                          │    TRIGGER DETECTED    │  
                          └───────────┬────────────┘  
                                      │  
            ┌─────────────────────────┼─────────────────────────┐  
            ▼                         ▼                         ▼  
          [ Human Intervention ]      
┌────────────────────────┐┌────────────────────────┐┌────────────────────────┐  
│    FAIL-CLOSED STOP    ││    HUMAN-IN-THE-LOOP   ││   DEGRADED ROADMAPPING │  
│  - High-risk security  ││  - Plan confirmation   ││  - Temporary system    │  
│    violation or cost   ││    or missing inputs   ││    timeout or service  │  
│    cap blowout         ││    requested           ││    outage detected     │  
└────────────────────────┘└────────────────────────┘└────────────────────────┘
```

The specific escalation triggers are mapped to their corresponding system actions:

| Failure / Risk State | Severity Level | Target Escalation Endpoint | System Recovery Action |
| :---- | :---- | :---- | :---- |
| **High-Impact Mutation** | High Risk | User Confirmation Gate.4 | Suspend active execution; render approval UI.1 |
| **Missing Input Data** | Low Risk | Operator Clarification.7 | Pause active loop; prompt user for parameter.7 |
| **Low Scorer Confidence** | Low Risk | Specialist Worker Model.6 | Route current task to a larger model block.6 |
| **Tool Failure Exceptions** | Moderate Risk | Local System Repair.5 | Execute fallback schema or alternate local tools.5 |
| **Budget Limit Reached** | High Risk | Manager Approval Queue.7 | Freeze state; require manual budget release.7 |
| **Database Outage** | High Risk | SRE Incident Management.5 | Switch database connection to read-only backup replica.5 |
| **Security Guard Breach** | High Risk | Security Operations Center. | Kill container instantly; revoke user API session keys. |

## **Failure Modes and Agentic Pathologies**

Agentic architectures introduce emergent system-level failure surfaces that do not exist in traditional software development.2 Robust system design requires analyzing these pathologies, identifying their telemetry signals, and deploying automated SRE-grade containment strategies.

| Pathology Family | Observed Symptom | Underlying Root Cause | Telemetry Indicator | Immediate Containment | Long-Term Architectural Remedy |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Runaway Loops** | System repeats identical planning steps.7 | Vague termination rules or parsing failures.7 | Identical transition hashes in task state.24 | Force-terminate the active container.24 | Enforce strict workflow graph loops with hard step caps.11 |
| **Tool Spam** | Spikes in tool call frequencies.12 | Aggressive keyword triggers spawning agents.12 | API requests exceed 50 calls/min.18 | Suspend session access to the tool gateway.11 | Implement tool budgets and concurrency caps.8 |
| **Plan / Goal Drift** | Plan strays from primary task goals.7 | Broad planning prompts; context accumulation.7 | Semantic divergence in planning vector checks. | Pause run and run plan validation steps.5 | Restrict planning steps to pre-compiled graph nodes.13 |
| **Context Bloat** | High latency; model ignores instructions.7 | raw tool output logs are appended to history.7 | Active context window usage exceeds 80%. | Summarize progress and compress active history.7 | Implement scratchpad garbage collection routines.7 |
| **Memory Poisoning** | Model references inaccurate facts in later steps.3 | Stale or unverified facts are committed to memory.3 | Discrepancies between database records and logs.5 | Clear active memory cache and reload from trace logs. | Route persistent memory writes through validation gates.3 |
| **Hallucinated Obs** | Model claims tool ran and succeeded.3 | Model bypasses return parsers to output answers.5 | Tool registry reports zero actual invocations.5 | Halt active loop and trigger quality score failure. | Enforce schema parsing on returned payloads.5 |
| **Premature Term** | Loop halts before completing the goal.7 | Broad or missing plan validation checks.7 | Validation node returns false after stop command.7 | Return status to active and resume execution steps.7 | Require verified physical evidence before termination.7 |
| **Refusal to Stop** | Loop continues despite goal validation.7 | Over-reflection loops ignore success states.7 | Step counter exceeds initial plan bounds.11 | Force state change to final and return response.11 | Build independent validation nodes into the graph.7 |
| **Over-Reflection** | High token spend on minor formatting checks.7 | Prompts force continuous self-criticism passes.7 | High token consumption with zero state changes.7 | Skip reflection and continue to next plan step.7 | Limit sequential reflection steps to <= 2 passes.7 |
| **Redundant Decomp** | Model breaks tasks into duplicate subtasks.18 | Weak plan tracking; missing dependency trees.18 | Identical subtask signatures in the active plan.18 | Consolidate tasks and clear duplicate plan entries. | Restrict recursive task decomposition depth to D <= 2.18 |
| **Duplicate Actions** | Identical API requests sent to target.18 | Parsing errors; missing transaction locks.5 | High-frequency matching API call signatures.18 | Block API requests and run recovery logic.5 | Enforce client-side transaction locks on dispatches.5 |
| **Budget Blowout** | Invoice spikes over a short duration.8 | Unconstrained loops run without active caps.8 | Monetary cost exceeds target threshold.8 | Kill active execution and revoke tenant access.18 | Enforce loop and token budget controls on runs.8 |
| **Stale State** | Model plans based on outdated observations.3 | Missing cache invalidation; state update gaps.6 | Active state timestamps lag tool executions.5 | Invalidate active state caches and rerun lookups.5 | Implement state tenure rules and validation steps.6 |
| **Permission Creep** | Model attempts to access restricted tools.24 | Broad system credentials leaked to model context.6 | Access denial events in tool registry logs.24 | Terminate session and regenerate API tokens.18 | Ensure model actions inherit active user session credentials.6 |
| **Tool Overreach** | Model calls complex APIs for simple steps.11 | Inefficient routing rules; missing heuristic paths.12 | Non-critical tools called for simple lookups.11 | Reject tool call and route to local code block.11 | Force simple tasks to run on deterministic code paths.12 |
| **Coordination Fail** | Peer workers stall waiting for outputs.13 | Circular dependencies in multi-agent designs.13 | Workflow graph state remains unchanged.13 | Terminate the workflow run and log coordinating errors. | Enforce static DAG execution rules on multi-agent structures.13 |

## **Agent Evaluation and Observability Guidance**

Evaluating agentic systems is fundamentally different from scoring traditional request-response engines.2 Since the execution path is synthesized dynamically at runtime, testing cannot be restricted to checking final outputs.2 Evaluation must analyze the entire trajectory, measuring the efficiency, safety, and validity of each state transition.5  
To support comprehensive auditing, every run must compile an immutable Agent Run Trace.1 This trace serves as the foundational artifact for system debugging, regression testing, and compliance replays.4

| Target Metric | Measurement Unit | Ideal Target Score | Evaluation Collection Method |
| :---- | :---- | :---- | :---- |
| **Task Success Rate** | Percentage | >= 98.0%.5 | Offline regression testing on golden validation sets.12 |
| **Trajectory Quality** | Path variance score | <= 1.15 variance. | Compute path alignment against reference paths. |
| **Correct Termination** | Match accuracy | 100% accuracy.7 | Verify runs halt with correct catalog status codes.7 |
| **Escalation Accuracy** | Precision/Recall | >= 95.0% precision.5 | Inject validation anomalies to test human routes.5 |
| **Step Count Efficiency** | Average integer count | <= 8 steps per task. | Monitor telemetry logs for average steps completed.3 |
| **Loop Depth Limit** | Integer count | Max depth <= 2.18 | Track parent-child spawning limits in tracing logs.18 |
| **Tool-Call Count** | Average integer count | <= 12 calls per task. | Monitor api gateway logs for invocation counts.12 |
| **Failed Tool Calls** | Count per session | <= 1 call per session.5 | Track returned exceptions and 4xx/5xx API codes.5 |
| **Retry Count Rate** | Percentage | <= 5.0% of dispatches. | Track total retries vs first-time tool successes.5 |
| **Timeout Rate** | Percentage | <= 1.0% of runs.5 | Monitor SRE runtime alerts for wall-clock drops.4 |
| **Budget Adherence** | Percentage | 100% adherence.8 | Track budget overrun exceptions at the gateway level.8 |
| **Cost per Success** | Monetary USD | Within plan budget.8 | Compare average successful cost vs total project margins.8 |
| **System Latency** | Milliseconds | <= 15,000 ms (p95).21 | Track overall time-to-first-token and task finish.21 |
| **Repair Success Rate** | Percentage | >= 90.0% recovery.5 | Track runs that complete after executing repair states.5 |
| **Memory Write Quality** | Grounding score | 100% grounding.3 | Verify memory blocks are supported by observations.3 |
| **Human Override Rate** | Percentage | <= 2.0% of approvals. | Track operator rejection rates in review queues.6 |
| **User Correction Rate** | Percentage | <= 5.0% of sessions. | Monitor session context for user revision loops.7 |
| **Trace Completeness** | Telemetry coverage | 100% coverage.1 | Audit trace files for missing state steps.1 |

### **Trajectory Evaluation vs. End-State Scans**

Relying solely on end-state scans introduces severe risks:

* An agent can return a valid final answer even after executing redundant tools or exceeding budget limits.8  
* System designers must deploy *trajectory-level validation*.5 This approach analyzes intermediate steps to identify inefficiencies, tool misuse, and near-violations before they lead to production failures.8

## **Cross-Canon Handoff Map**

Agentic orchestration does not exist in isolation. It relies on, and provides vital context to, other core systems across the AI Engineering Canon.

| Downstream Target Report | Handoff Artifact | Underling System Implication | Downstream Architectural Owner |
| :---- | :---- | :---- | :---- |
| **AI-ENG-N (Tool Contracts)** | Dynamic tool payload request signatures.6 | Enforces strict type checking and schema matching.5 | Platform Gateway / Serialization Engine. |
| **AI-ENG-O (Action Verification)** | Grounded observation cache.4 | Reconciles state changes and handles recovery.5 | Post-Action Verification Scorer. |
| **AI-ENG-S (Production Pathologies)** | System trace files and retry histories.5 | Documents runtime failure loops and tool overreach.18 | Site Reliability Engineering (SRE) Teams. |
| **AI-ENG-T (Permissions & Injection)** | Active session scope metadata.1 | Restricts model capabilities to authorized boundaries.1 | Identity & Access Management (IAM) Gateway. |
| **AI-ENG-V (Resource Abuse)** | Loop and token budget allocations.7 | Terminates runs that violate resource limits.8 | Resource Quota / Gateway Rate-Limiter. |
| **AI-ENG-W (Fallback Modes)** | Targeted recovery policy structures.5 | Governs system behavior during downstream outages.5 | Incident Management Router. |
| **AI-ENG-X (Trust & Control)** | Proposed planning schedules.1 | Renders interactive approval interfaces for users.1 | User Interface (UI) Presentation Layer. |
| **AI-ENG-Y (Human-in-the-Loop)** | Transition escalation requests.6 | Manages human review and approval workflows.6 | Human-in-the-Loop (HITL) Queue. |
| **AI-ENG-Z (Telemetry)** | Step count and execution latency logs.4 | Streams active session metrics to dashboards.3 | Prometheus / Datadog Monitoring Routers. |
| **AI-ENG-AA (Agent Evaluations)** | Golden trajectory testing sets.5 | Evaluates regression issues and model capabilities.12 | CI/CD Automated Test Runner. |
| **AI-ENG-AB (Replay Traces)** | Versioned state database checkpoints.13 | Replicates and debugs failures in sandboxes.1 | Developer Testing Environment. |
| **AI-ENG-AC (Incident Response)** | System halt and error traces.5 | Supports rapid system diagnostics during incidents.18 | Active On-Call SRE Support Systems. |
| **AI-ENG-AJ (Reference Architectures)** | Orchestrator state machine code. | Serves as the blueprint for scalable agent systems.6 | Systems Engineering and Architecture. |

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

The architectural foundation of action verification is post-execution truth management. To maintain consistency in distributed systems, platform architects must distinguish between several distinct representations of state. These layers must not be conflated, as each represents a different phase of the execution lifecycle:  
Planned -> Proposed -> Validated -> Executed -> Observed -> Verified -> Reconciled -> Reported

### **Why Every Transition Matters**

```
  Planned   --->   Proposed   --->   Validated   --->   Executed  
     |                |                  |                 |  
(Plan may be     (Proposal may     (Validated call    (Execution may  
 wrong/stale)     be malformed)     may still fail)    time out/abort)  
                                                           |  
  Reported  <---  Reconciled  <---   Verified    <---   Observed  
     |                |                  |                 |  
(User report     (Authoritative     (Observation is    (Success returned  
 can't outrun)    state may lag)     only evidence)     may be premature)
```

1. **Planned to Proposed:** A plan may be syntactically correct but semantically flawed, or it may be based on outdated information.  
2. **Proposed to Validated:** A proposal generated by a probabilistic model may contain malformed parameters, invalid types, or unauthorized scopes that must be caught by schema checks.  
3. **Validated to Executed:** A validated request can still fail at runtime due to network partitions, service crashes, rate limits, or locking timeouts.  
4. **Executed to Observed:** An execution may time out, hiding whether the action actually committed. Alternatively, the tool may return a partial observation object.  
5. **Observed to Verified:** The initial observation is only evidence, not reality. A successful API response may be premature if downstream propagation is asynchronous.  
6. **Verified to Reconciled:** The verification check must query the authoritative system of record. This query must bypass replicas and caches that may be lagging.  
7. **Reconciled to Reported:** A user-facing success report must never outrun actual state reconciliation. Otherwise, the system risks presenting a false success report to the client.

### **"Observations are Evidence, Not Reality"**

Under Model Context Protocol (MCP) and modern agent architectures, tools return standardized observation objects to prevent models from directly interpreting raw stdout streams or stack traces. However, these observation objects are merely local representations of the execution run; they do not prove that the physical state change occurred correctly in the target system.  
For example:

* An email service tool may return { "status": "sent" } upon accepting a payload, but the message may still be blocked downstream by a spam filter.  
* A database write tool may return { "status": "success" }, but the transaction may have hit a database read replica rather than the primary writer node, resulting in a stale read on subsequent queries.  
* A container creation tool may return { "id": "container_123" }, but the container may crash-loop immediately after startup.

Therefore, a robust system must treat all initial tool observations as unverified claims. The task state must remain locked until an active verification check confirms the change against the authoritative system of record.

## **Hallucinated Success and Assumed Completion**

Hallucinated success is the central execution pathology of tool-using agentic systems. It occurs when an orchestrator or user-facing response treats an intended, attempted, pending, failed, or partially completed action as successfully finalized. This failure mode does not necessarily stem from semantic "hallucination" in the model's text generation; rather, it is an architectural collapse where the orchestrator fails to decouple execution from verification.

| Pattern Code | Pathology | Root Cause | Programmatic Mitigation |
| :---- | :---- | :---- | :---- |
| **HS-01** | **Phantom Execution** | The model claims a tool ran when no physical invocation occurred. | Intercept all model tool-calls at the orchestrator level; assert execution traces exist. |
| **HS-02** | **Validation Error Collapse** | The model claims success even after a parameter validation error was raised. | Catch validation exceptions in deterministic wrappers; do not return error payloads to the model as raw text. |
| **HS-03** | **Accepted-as-Completed** | The agent treats an asynchronous accepted status as a finished transaction. | Enforce status polling or webhook confirmation before updating the believed task state. |
| **HS-04** | **Flattened Partial Success** | The workflow treats a partial success as a full success. | Map sub-action execution statuses explicitly; block progress if any sub-action fails. |
| **HS-05** | **Timeout Blindspot** | An execution timeout occurs, and the system assumes the action failed, triggering a duplicate retry. | Enforce stable idempotency keys; check the idempotency cache before retrying. |
| **HS-06** | **Uncontrolled Retry Mutation** | A retry succeeds but creates duplicate side effects. | Enforce database unique constraints scoped by tenant, operation, and idempotency key. |
| **HS-07** | **Stale-Replica Conformation** | A verification read hits a lagging replica and returns old or null data. | Implement a metadata DBRI framework; route post-write reads to the master node. |
| **HS-08** | **Stale-Cache Read** | A read request hits an uninvalidated cache, confirming a pre-action state. | Enforce cache-invalidation-on-write policies or bypass caches during verification checks. |
| **HS-09** | **Premature Propagation Success** | A tool returns success before downstream propagation is complete. | Implement dynamic monitoring of replica lag; poll until state-consistent. |
| **HS-10** | **Error Summarization Collapse** | The model interprets an error payload as a success response. | Intercept tool outputs; convert non-2xx transport codes or error payloads into system exceptions. |
| **HS-11** | **Outrun Status Reporting** | A user-facing message claims completion before reconciliation finishes. | Lock user status updates until the action ledger commits a verified status. |

From an operational and security standpoint, hallucinated success is highly serious. It corrupts user belief, distorts downstream planning, pollutes audit records, and can lead to dangerous loop behaviors where an agent proceeds as if its previous actions succeeded when they actually failed or corrupted system state.

## **State Reconciliation Model**

The core engine of action verification is the State Reconciliation Model. This model compares five distinct layers of state to guarantee consistency across the agentic boundary.

```
  +--------------------------------------------------------------+  
  |                        Intended State                        |  
  |             (User goal: "Increase credit line")              |  
  +--------------------------------------------------------------+  
                                |  
                                v  
  +--------------------------------------------------------------+  
  |                       Requested State                        |  
  |           (API parameters: "requested_amount": 5000)         |  
  +--------------------------------------------------------------+  
                                |  
                                v  
  +--------------------------------------------------------------+  
  |                        Observed State                        |  
  |        (Tool response: HTTP 202 Accepted, "status": "pending")|  
  +--------------------------------------------------------------+  
                                |  
                                v  
  +--------------------------------------------------------------+  
  |                     Authoritative State                      |  
  |         (Database read: "credit_limit": 5000, LSN: 240892)   |  
  +--------------------------------------------------------------+  
                                |  
                                v  
  +--------------------------------------------------------------+  
  |                     Believed Task State                      |  
  |          (Orchestrator log: "Credit update reconciled")      |  
  +--------------------------------------------------------------+
```

To ensure system integrity, these state layers must reconcile according to strict mathematical and logical invariants. Let SI be the Intended State, SR be the Requested State, SO be the Observed State, SA be the Authoritative State, and SB be the Believed Task State.  
An action is considered legally and operationally reconciled if and only if:  
SA == SI AND SB is updated with the value of SA  
If SO returns success but SA does not match SI, a state discrepancy exists. To manage these discrepancies across different operations, systems must apply specific alignment requirements:

| State Layer | Source of Truth | Trust Level | Alignment Requirement |
| :---- | :---- | :---- | :---- |
| **Intended State (SI)** | Orchestrator Plan / User Directive | Baseline Goal | Immutable for the duration of the action execution cycle. |
| **Requested State (SR)** | Deterministic Tool Input Schema | Low | Must be mathematically consistent with SI parameter boundaries. |
| **Observed State (SO)** | API response or MCP Output Payload | Medium | Serves as a trigger for verification; cannot modify SB. |
| **Authoritative State (SA)** | Master Database Node or Signed Ledger | High | Must match SI before the action can transition to a finalized state. |
| **Believed Task State (SB)** | Orchestrator Context Memory / Execution State | Highly Guarded | Read-only copy of SA; updated only via post-verification reconciliation. |

### **Observation Trust Ladder**

To safely process tool outputs, the orchestrator must pass the returned observation through a progressive security and truth verification ladder.

```
   <-- Bypasses replicas, logged to immutable ledger  
                 ^  
        <-- Divergence checked, parameters match intent  
                 ^  
  [ Level 2: Verified Observation ]   <-- Confirmed via independent API readback  
                 ^  
  [ Level 1: Normalized Observation ] <-- Raw error codes/JSON mapped to schemas  
                 ^  
   <-- Unstructured terminal streams/unfiltered API
```

* **Level 0: Raw Tool Prose/Output:** Unstructured text, standard output/error streams, or unfiltered API error objects directly from the run environment. *Security Posture:* Extremely unsafe. Subject to tool-use hallucinations, prompt injection, or parsing failures.  
* **Level 1: Normalized Observation:** Standardized observation schema mapping raw results into typed properties (such as text, JSON, or resource references), isolating raw stack traces. *Security Posture:* Syntactically safe for model consumption but unverified against the physical state.  
* **Level 2: Verified Observation:** The system has confirmed the transaction's existence via an independent API read or database readback. *Security Posture:* Confirms the physical transaction took place but does not guarantee the state matches the original business intent.  
* **Level 3: Reconciled State:** The state properties are verified to be in exact alignment with the intended state parameters. All divergence checks have passed. *Security Posture:* Safe to update the agent's internal task memory and belief systems.  
* **Level 4: Audit-Confirmed State:** The change is confirmed against authoritative master nodes (bypassing replicas), signed with cryptographic nonces, and committed to an append-only action ledger. *Security Posture:* Production-ready. Safe to generate user-facing success responses and proceed to subsequent steps in the business process.

### **Discrepancy Classification Model**

When SA diverges from SI, the reconciliation engine must classify the discrepancy to determine the correct recovery path:

* **No-Op:** The requested mutation resulted in zero state changes because the target system was already in the desired state.  
* **Value Mismatch:** The target resource was modified, but the resulting values (such as an account balance or status field) do not match the requested parameters.  
* **Stale State:** The transaction was rejected because the target resource version or timestamp changed concurrently, indicating a write conflict.  
* **Partial Application:** Only a subset of the requested mutations committed successfully, leaving the system in an inconsistent state.  
* **Duplicate Side Effect:** The operation executed multiple times, creating unintended duplicate records or charges.  
* **Target Missing:** The target resource could not be found, or it was deleted concurrently before the mutation could commit.  
* **Tenant / Context Drift:** The mutation succeeded, but the resource was modified under a different tenant, user, or permission scope than authorized.  
* **Propagation Delay:** The mutation committed on the master node but has not yet synchronized to the read replicas used for verification.  
* **Unverifiable State:** The target system does not expose a read API or verification endpoint, leaving the physical state of the mutation unknown.

## **Verification Depth by Side-Effect Class**

Not all actions carry the same operational risk, financial exposure, or legal consequences. A high-dimensional agent architecture must scale its verification depth according to the side-effect class of the executing tool.

| Side-Effect Class | Required Post-Action Check | Authoritative Verification Source | Permitted Fallback Routing | Latency Tolerance | Human-Review Trigger Threshold |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Read-Only Observation** | Verify TLS certificates, request-response integrity, query parameters echo. | Read-only replica database. | Fallback to primary writer on replica timeout. | < 100 ms | Never triggered for read operations. |
| **Ephemeral Write** | Verify file existence, absolute path normalization, write sandboxing, file size. | Sandboxed filesystem monitor. | Abort task and raise execution exception. | < 500 ms | Triggered only on security sandbox violations. |
| **Low-Risk Internal Write** | Read-after-write key lookup, verify row count = 1, assert matching version number. | Primary database writer node. | Fallback to lagging replica with warning log. | < 1.5 s | Triggered on persistent write conflicts after retries. |
| **Medium-Risk Operational Write** | Verify object status, diff before/after state properties, check message delivery status. | Application service API or distributed state cache. | Queue for asynchronous verification retries. | < 5.0 s | Triggered on verification failures exceeding 3 retries. |
| **High-Risk External Write** | Poll delivery service API, fetch unique tracking ID, verify email DKIM/DMARC signatures. | Third-party partner system API or gateway ledger. | None. Hold transaction in pending state. | < 30.0 s | Triggered on status declines or payment mismatches. |
| **Critical Mutation** | Multi-source ledger reconciliation, confirm cryptographic signature, run compliance checks. | Immutable audit ledger or consensus database. | None. Fail closed and lock the transaction. | Asynchronous (< 10 min) | **Always triggered.** Requires explicit operator sign-off before commit. |

## **Post-Action Check Pattern Library**

To operationalize verification across diverse system types, platform designers must implement specific, deterministic post-action validation checks. The following patterns define how these checks are executed for common tool families.

### **1. Databases**

* *Validation Action:* Issue a read-after-write query directly to the database writer node, using snapshot isolation to bypass replica lag.  
* *Verification Parameters:* Verify that the target row exists, the Log Sequence Number (LSN) or row version has incremented, the modified field values match the requested state, and the tenant ID is correct.  
* *Failure Indicator:* The row count is zero, the row version is stale, or the transaction is blocked by a database lock timeout.

### **2. Filesystems**

* *Validation Action:* Run a path normalization check to prevent directory traversal attacks. Resolve the target file path outside the local execution sandbox.  
* *Verification Parameters:* Verify that the file exists, the file size matches the write buffer, the file extension is on the allowlist, and the SHA-256 cryptographic hash matches the source payload.  
* *Failure Indicator:* The file is not found on the host system (sandboxed write illusion), or the calculated hash does not match the expected value.

### **3. Email Services**

* *Validation Action:* Poll the email delivery service API using the returned unique message ID.  
* *Verification Parameters:* Verify that the recipient's address is resolved, the status is delivered or queued, and the message has passed SPF, DKIM, and DMARC alignment checks.  
* *Failure Indicator:* The message is flagged as bounced, rejected by the gateway, or quarantined by spam filters.

### **4. Calendar Systems**

* *Validation Action:* Read the target user's calendar event details using the unique event ID.  
* *Verification Parameters:* Verify that the event ID exists, the meeting time and timezone are correct, the attendee guest list matches, and the organizer ID is verified.  
* *Failure Indicator:* The event conflicts with an existing booking, the attendee invitation bounces, or a duplicate event ID is detected.

### **5. Payments and Refunds**

* *Validation Action:* Query the payment processor ledger using a stable business idempotency key.  
* *Verification Parameters:* Verify that the transaction status is captured or authorized, the settled amount and currency match the purchase order, and a unique transaction ID is committed.  
* *Failure Indicator:* The transaction is declined, a duplicate charge is detected under the same idempotency key, or the settled amount diverges.

### **6. CRM and Ticketing**

* *Validation Action:* Fetch the updated CRM object or ticket record using the returned unique ID.  
* *Verification Parameters:* Verify that the ticket status is updated or created, the assignee matches the routing logic, and the change history logs the correct actor ID and workflow ID.  
* *Failure Indicator:* The field changes are missing, the assignee remains unresolved, or the database write times out due to record locking.

### **7. Deployment Pipelines**

* *Validation Action:* Query the deployment orchestrator API to monitor container health metrics and rollout progress.  
* *Verification Parameters:* Verify that the active rollout stage reaches 100%, all container health checks pass, and the error budget is unaffected.  
* *Failure Indicator:* An automated rollback is triggered, container crash loops are detected, or the error budget is exceeded.

### **8. Procurement Systems**

* *Validation Action:* Query the procurement register to match the purchase order (PO) details against the approved schema.  
* *Verification Parameters:* Verify that the PO status is approved, the vendor SKU matches the catalog, and the transaction cost falls within the allocated system margin.  
* *Failure Indicator:* An unauthorized SKU is detected, the PO is rejected due to spend limits, or a pricing mismatch occurs.

### **9. Browser and Web Automations**

* *Validation Action:* Inspect the page state, execute target selector assertions, and verify DOM elements after interaction.  
* *Verification Parameters:* Verify that the confirmation element is present, the target URL matches the success path, and the downloaded file hash matches.  
* *Failure Indicator:* The target selector times out, the navigation is blocked by a CAPTCHA challenge, or form validation fails.

### **10. Security and Infrastructure**

* *Validation Action:* Query the directory access controls or run a live token assertion check.  
* *Verification Parameters:* Verify that the subject token lacks excluded scopes, the target policy matches the updated IAM schema, and the change is logged to the security audit trail.  
* *Failure Indicator:* An over-permissive wildcard policy is detected, or unauthorized privilege escalation occurs.

## **Partial Failure Handling Model**

In complex, distributed systems, operations rarely succeed or fail in a simple binary fashion. Real-world actions are often multi-step, asynchronous, or eventually consistent. A robust agentic architecture must represent partial states explicitly rather than flattening them into success or failure.

### **Real-World Partial Failure Scenarios**

```
  +---------------------------------------------------------------------------------+  
  |                            Partial Failure Examples                             |  
  +---------------------------------------------------------------------------------+  
          |  
          +---> CRM ticket is created, but assignee email notification bounces.  
          |  
          +---> Calendar event is scheduled, but one attendee invitation fails.  
          |  
          +---> Credit card payment is authorized, but downstream capture times out.  
          |  
          +---> Refund is initiated on a ledger, but bank settlement is delayed.  
          |  
          +---> File is uploaded to storage, but downstream vector indexing fails.  
          |  
          +---> Database write is committed, but cache invalidation times out.  
          |  
          +---> Deployment rolls out to region US-East, but fails in region EU-West.  
          |  
          +---> CRM update is accepted, but downstream sync is delayed.  
          |  
          +---> Email is queued at the SMTP gateway, but rejected by recipient server.  
          |  
          +---> Write commits on primary node, but verification read hits stale replica.
```

To manage these scenarios without corrupting the agent's belief system, the system state machine must represent the following lifecycle states:

```
                 +-----------+  
                 | Proposed  |  
                 +-----------+  
                       |  
                       v  
                 +-----------+  
                 | Validated |  
                 +-----------+  
                       |  
                       v  
                 +-----------+  
                 | Executing |  
                 +-----------+  
                       |  
        +--------------+--------------+  
        |                             |  
        v                             v  
  +-----------+                 +-----------+  
  | Accepted  |                 |  Failed   |  
  +-----------+                 +-----------+  
        |                             |  
        v                             v  
  +-----------+                 +-----------+  
  |  Pending  |                 |   Error   |  
  +-----------+                 +-----------+  
        |                             |  
        v                             v  
  +-----------+                 +-----------+  
  | Committed |                 | Escalated |  
  +-----------+                 +-----------+  
        |                             |  
        v                             v  
  +-----------------------+     +-----------+  
  |  Partially Committed  |     | Abandoned |  
  +-----------------------+     +-----------+  
        |  
        +------------+  
        |            |  
        v            v  
  +--------------+ +-------------+  
  | Compensating | | Rolled Back |  
  +--------------+ +-------------+  
        |  
        v  
  +--------------+  
  | Compensated  |  
  +--------------+
```

### **Technical System States and Transitions**

* **Proposed:** The orchestrator has generated a tool payload but has not yet validated it. *Transition:* Transitions to Validated upon passing validation checks.  
* **Validated:** The payload has passed syntax and safety schema checks. *Transition:* Transitions to Executing once dispatched to the runtime environment.  
* **Executing:** The tool call is actively processing within the distributed network. *Transition:* Transitions to Accepted on HTTP 202, Committed on HTTP 200, or Failed on HTTP 5xx/4xx.  
* **Accepted:** The target platform has accepted the mutation request but has not yet processed it. *Transition:* Transitions to Pending to begin verification.  
* **Pending:** Verification checks are actively running, polling, or waiting for webhooks. *Transition:* Transitions to Committed on successful readback, or Failed on timeout.  
* **Committed:** Authoritative verification has confirmed that the physical state matches the original intent. *Transition:* This is a terminal success state.  
* **Partially Committed:** A multi-step transaction succeeded in some systems but failed in others (for example, an inventory reservation succeeded, but payment processing failed). *Transition:* Transitions to Compensating or escalate to manual review.  
* **Failed:** The target system rejected the transaction, or verification timed out. *Transition:* Transitions to Compensating or trigger an automatic retry policy.  
* **Compensating:** An idempotent reversing transaction has been scheduled and is executing. *Transition:* Transitions to Compensated on success, or escalate to manual review on failure.  
* **Compensated:** The reversing action successfully completed, restoring consistency to the system. *Transition:* This is a terminal failure recovery state.  
* **Rolled Back:** The active transaction was successfully aborted within a database transaction boundary, preventing any partial mutations. *Transition:* This is a terminal failure recovery state.  
* **Review Required:** Automatic recovery failed, or verification returned an unknown, untrusted state. *Transition:* Requires a human operator to inspect the state and manually resolve the discrepancy.  
* **Abandoned:** The transaction was aborted, and the current system state was left as-is, with the discrepancy logged in the action ledger. *Transition:* This is a terminal failure state.

## **Rollback, Compensation, and Forward Recovery**

When a discrepancy is detected during verification, the system must choose an appropriate recovery path based on the transactional semantics of the target systems.

```
                                      Pivot Transaction  
                                              |  
                                              v  
  ---> ---> --->  
           |                          |                         |                        |  
     (Compensable)              (Compensable)              (Idempotent)             (Idempotent)
```

### **Rollback**

Rollback is the preferred recovery pattern for systems with native transaction support, such as relational databases. It operates within a single database transaction boundary. If any operation fails during execution or verification, the system aborts the transaction, reverting all changes to their pre-transaction state without exposing intermediate, inconsistent states.

### **Compensation**

Compensation is designed for distributed microservices, third-party APIs, and legacy systems that lack native transaction support. It executes a new, independent transaction that reverses the logical effects of a previously committed change (for example, issuing a refund to reverse a charge, or deleting a file to reverse an upload). Because the original transaction was already visible to other processes, compensation does not provide isolation, which can lead to temporary "dirty reads". Compensating actions must be designed to be highly idempotent, as failures during the compensation step can leave the system in an inconsistent state.

### **Forward Recovery**

Forward recovery is applied when rolling back or compensating is either impossible or too costly. The system attempts to resolve a partial failure by completing the remaining steps of the process (for example, retrying a notification delivery after a database write has committed, or polling a payment settlement API to confirm a pending transaction). Forward recovery relies on the remaining steps being designed as idempotent, retryable transactions.

### **The Role of the Pivot Transaction**

A key concept when orchestrating multi-step transactions is the **Pivot Transaction**. The pivot transaction represents the point of no return in a saga workflow.

1. **Before the Pivot Transaction:** All executed steps are compensable. If a failure occurs before or during the pivot transaction, the orchestrator triggers backward recovery, running compensating actions in reverse order to revert the system state.  
2. **The Pivot Transaction:** Once the pivot transaction completes successfully, backward recovery is no longer an option. The pivot transaction cannot be undone or compensated.  
3. **After the Pivot Transaction:** All subsequent steps are designed as retryable, idempotent transactions. If a failure occurs after the pivot transaction, the system must use forward recovery, retrying the remaining steps until the workflow achieves a consistent final state.

### **Rollback and Compensation Framework**

| Recovery Strategy | State Isolation | Recovery Timing | Cost Overhead | Transaction Bounds |
| :---- | :---- | :---- | :---- | :---- |
| **Atomic Rollback** | Strong isolation. Prior state is restored immediately. | Synchronous (within transaction abort). | Low (handled natively by DB engine). | Single database instance or local filesystem. |
| **Saga Compensation** | No isolation. Intermediate states are visible to other clients. | Asynchronous (triggered by coordinator). | Medium (requires executing reversing API calls). | Distributed microservices and third-party APIs. |
| **Forward Recovery** | No isolation. System progresses to eventual consistency. | Asynchronous (via retries and polling). | Low (leverages existing retry structures). | Eventually consistent systems and message queues. |
| **Manual Review** | Manual containment. Affected resources are locked. | Asynchronous (requires operator intervention). | High (requires developer or support engineer time). | Highly complex, multi-tenant transactions. |
| **Fail-Closed Hold** | System execution freezes; resources remain locked. | Asynchronous (requires security clearance). | High (impacts workflow throughput). | Mission-critical financial or infrastructure operations. |
| **Incident Creation** | Post-mortem analysis. Logging and alerts are triggered. | Asynchronous (after recovery failure). | High (requires incident response team). | Production-critical system failures. |

## **Recovery Decision Tables**

To handle verification failures deterministically, the system applies a set of recovery tables. These tables map specific discrepancy types and operational conditions directly to defined recovery actions, minimizing the need for unsupervised language model decision-making.

| Discrepancy Type | Verification Finding | Active Operational Condition | Target Recovery Action | Technical Rationale |
| :---- | :---- | :---- | :---- | :---- |
| **Observation / Authority Mismatch** | Tool returned success, but readback shows old state. | Seconds behind source <= 5.0 s. | **Retry Verification** | The replica is likely lagging; a brief delay before retrying allows replication to catch up. |
| **Observation / Authority Mismatch** | Tool returned success, but readback shows old state. | Seconds behind source > 5.0 s. | **Force Master Read** | Significant replication lag detected; bypass the replica and query the master database directly. |
| **Observation / Authority Mismatch** | Tool returned success, but readback shows old state. | Target system lacks a read API or verification endpoint. | **Escalate and Hold** | Verification is impossible; freeze the workflow and route the task to manual operator review. |
| **Verification Timeout** | Verification polling timed out during check. | Action is classed as reversible. | **Compensate and Stop** | Prevent the system from hanging; run compensating actions to clean up and report a failure. |
| **Verification Timeout** | Verification polling timed out during check. | Action is irreversible. | **Hold and Escalate** | Reversing is impossible; freeze the workflow and route the task to manual operator review. |
| **Pending Timeout** | Execution timed out; status in idempotency cache is pending. | Idempotency key exists in cache. | **Poll Idempotency Record** | Prevent duplicate execution; query the cache until the original transaction succeeds or fails. |
| **Partial Success** | A multi-step saga failed after some steps completed. | Workflow has not passed the pivot transaction. | **Backward Compensation** | Revert the completed steps in reverse order to return the system to a clean state. |
| **Partial Success** | A multi-step saga failed after some steps completed. | Workflow has passed the pivot transaction. | **Forward Recovery** | The pivot transaction is committed; retry the remaining steps until the system is consistent. |
| **Stale State Error** | Target object version does not match input version. | Optimistic locking check failed. | **Refresh and Replan** | The object was modified concurrently; pull the latest state and trigger the planning engine to retry. |
| **Duplicate Side Effect** | A duplicate transaction was detected under the same key. | Operation is classed as financial or security-critical. | **Freeze and Alert** | Duplicate execution can cause severe business impact; lock the workflow and alert SREs. |
| **Wrong Target Modified** | Verification query returned data for a different tenant ID. | High-security containment field enabled. | **Freeze and Alarm** | A potential data leak or tenant boundary breach has occurred; immediately lock the system and trigger an alert. |
| **Unverifiable Result** | Verification query failed due to credentials or network error. | Target system is down or unreachable. | **Queue and Retry** | The failure is transient; back off and retry verification after the network is restored. |
| **Unverified High-Impact Write** | A high-risk external write was executed. | No independent verification could be executed. | **Hold and Escalate** | Prevent unverified actions from confirming; route to a human operator for sign-off. |

## **Action Ledger and Auditability**

To support security audits, automated replays, and forensic investigations, the system must log every action transition within a structured, append-only Action Ledger. Unlike runtime traces (which track raw execution paths and latency), the Action Ledger records the logical state changes and verification proof for each transaction.

```JSON  
{  
  "$schema": "https://json-schema.org/draft/2020-12/schema",  
  "title": "ActionLedgerEntry",  
  "type": "object",  
  "required": [  
    "action_id",  
    "workflow_run_id",  
    "tenant_id",  
    "user_id",  
    "idempotency_key",  
    "side_effect_class",  
    "intended_outcome",  
    "execution_status",  
    "timestamps"  
  ],  
  "properties": {  
    "action_id": { "type": "string", "format": "uuid" },  
    "workflow_run_id": { "type": "string", "format": "uuid" },  
    "tenant_id": { "type": "string" },  
    "user_id": { "type": "string" },  
    "idempotency_key": { "type": "string" },  
    "side_effect_class": {   
      "type": "string",   
      "enum": ["read-only", "ephemeral-write", "low-risk-write", "medium-risk-write", "high-risk-write", "critical-mutation"]   
    },  
    "intended_outcome": {  
      "type": "object",  
      "required": ["target_resource", "expected_state"],  
      "properties": {  
        "target_resource": { "type": "string" },  
        "expected_state": { "type": "object" }  
      }  
    },  
    "validated_payload_hash": { "type": "string" },  
    "execution_status": {   
      "type": "string",   
      "enum": ["proposed", "validated", "executing", "accepted", "pending", "committed", "partially-committed", "failed", "compensating", "compensated", "rolled-back", "review-required", "abandoned"]   
    },  
    "observation_object": {  
      "type": "object",  
      "required": ["status_code", "payload"],  
      "properties": {  
        "status_code": { "type": "integer" },  
        "payload": { "type": "object" }  
      }  
    },  
    "verification_query": {  
      "type": "object",  
      "required": ["query_endpoint", "parameters"],  
      "properties": {  
        "query_endpoint": { "type": "string" },  
        "parameters": { "type": "object" }  
      }  
    },  
    "verification_result": {  
      "type": "object",  
      "required": ["verified_state", "mismatch_detected"],  
      "properties": {  
        "verified_state": { "type": "object" },  
        "mismatch_detected": { "type": "boolean" },  
        "discrepancy_classification": { "type": "string" }  
      }  
    },  
    "recovery": {  
      "type": "object",  
      "properties": {  
        "recovery_decision": { "type": "string" },  
        "compensation_action_id": { "type": "string", "format": "uuid" },  
        "rollback_action_id": { "type": "string", "format": "uuid" },  
        "escalated_to_user_id": { "type": "string" }  
      }  
    },  
    "timestamps": {  
      "type": "object",  
      "required": ["proposed_at"],  
      "properties": {  
        "proposed_at": { "type": "string", "format": "date-time" },  
        "executed_at": { "type": "string", "format": "date-time" },  
        "verified_at": { "type": "string", "format": "date-time" },  
        "reconciled_at": { "type": "string", "format": "date-time" }  
      }  
    },  
    "trace_parent_id": { "type": "string" }  
  }  
}
```

The difference between a trace and an action ledger is operational and qualitative:

* **Runtime Trace:** Traces capture execution flow, latency, and telemetry across service hops (such as spans, CPU time, database query timings, and routing hops). They are optimized for debugging and performance profiling, and are typically stored in transient, high-volume search indexes.  
* **Action Ledger:** Ledgers record logical transitions, state reconciliation, and recovery outcomes for business-critical transactions. They are optimized for compliance, forensic analysis, billing verification, and workflow replay, and are stored in immutable, write-once, read-many (WORM) storage.

## **Tool-Result Grounding and Agent State Updates**

To prevent model belief drift—where a model plans subsequent steps based on assumptions about its own execution success—the orchestrator must enforce a strict separation between the model's generation context and the verified task state.

```
  +--------------------------------------------------------------+  
  |                   Model Execution Context                    |  
  |   Model: "I will call charge_card() for $100."              |  
  +--------------------------------------------------------------+  
                                |  
                                | (API call initiated)  
                                v  
  +--------------------------------------------------------------+  
  |                Deterministic Execution Layer                 |  
  |   Returns: HTTP 202, "status": "pending"                     |  
  |   (Task state remains locked at "unpaid")                    |  
  +--------------------------------------------------------------+  
                                |  
                                | (Asynchronous Verification)  
                                v  
  +--------------------------------------------------------------+  
  |                     Authoritative Ledger                     |  
  |   Confirms: Charge settled on bank register                  |  
  +--------------------------------------------------------------+  
                                |  
                                | (State updated & grounding context injected)  
                                v  
  +--------------------------------------------------------------+  
  |                   Model Evaluation Context                   |  
  |   Model: "Verification confirmed. The order is paid."        |  
  +--------------------------------------------------------------+
```

1. **Read-Only Orchestrator State:** The orchestrator maintains the structured task state outside the model's context window. This state cannot be modified by the model's output text. It is updated only via structured verification events.  
2. **State-Grounding Context Injection:** When the model is invoked for its next planning step, the orchestrator injects the verified task state directly into the system prompt context. The model is forced to evaluate its next steps based on this grounded truth, rather than relying on its historical generation logs.  
3. **Strict Status Reporting Invariant:** If the orchestrator has not completed verification for an action, any user-facing status message must present the action as pending. For example, the agent must report, "The request has been accepted and verification is pending," rather than claiming, "I have completed the task".  
4. **Planning Adaptation:** If an action fails, partially succeeds, or remains pending, the agent's next planning step must adapt based on the verified state. The orchestrator must force the model to repair, compensate, poll, ask the user, or escalate, rather than continuing with the original plan.

## **Action Verification Observability and Regression Control**

To maintain reliability, security, and visibility in production, platforms must implement comprehensive observability metrics and strict regression testing frameworks for the action verification layer.

### **Platform Performance Metrics**

* **Verification Success Rate:** The percentage of executed actions that successfully pass verification against the authoritative state.  
* **Verification Latency:** The average time spent executing verification and reconciliation checks.  
* **Read-After-Write Failure Rate:** The frequency of verification failures caused by stale reads or replication lag.  
* **Discrepancy Rate:** The frequency of mismatches detected between observed tool responses and verified authoritative states.  
* **Stale-State Rate:** The frequency of concurrency conflicts where verification fails due to concurrent modifications.  
* **Partial Failure Rate:** The percentage of executions that result in a partially committed status.  
* **Pending-Too-Long Rate:** The frequency of actions that remain stuck in the pending or accepted states beyond their SLA thresholds.  
* **Unknown-State Rate:** The percentage of actions where the authoritative state cannot be determined, requiring manual escalation.  
* **Duplicate Action Rate:** The frequency of duplicate execution attempts detected and blocked by the idempotency framework.  
* **Idempotency Recovery Rate:** The percentage of duplicate requests that were successfully resolved using cached responses.  
* **Compensation Rate:** The percentage of workflows that trigger compensating transactions.  
* **Rollback Rate:** The percentage of workflows resolved via synchronous database rollbacks.  
* **Failed Compensation Rate:** The frequency of compensating transactions that fail, leaving the system in an inconsistent state.  
* **False Success Rate:** The frequency of hallucinated success events that bypass the verification layer and reach the user.  
* **Human Escalation Rate:** The percentage of actions routed to manual review queues.  
* **Mean Time to Reconcile (MTTR):** The average time required to transition an action from execution to a reconciled terminal state.  
* **Action-Ledger Completeness:** The percentage of actions that are fully logged in the append-only action ledger.

### **Trace Linkage and Context Propagation**

Every verification check must be logged as a child span of the primary tool execution span. This links model inference, validation, tool execution, and verification events into a single, cohesive trace context. The system must propagate the traceparent ID through external API headers, database transaction tags, and message queue attributes to enable tracing across distributed system boundaries.

### **Regression Control and Release Gates**

Because changes to tool contracts, schemas, or verification policies can regress agent behavior, platforms must implement strict release gates:

* **Trace Replays:** Before deploying changes, replay historical traces against a sandbox environment to confirm that verification logic and recovery pathways behave identically.  
* **Verification Canaries:** Deploy verification updates using canary configurations. Monitor metrics like the verification failure rate and manual escalation rate to detect anomalies before full rollout.  
* **SRE Incident Drills:** Regularly conduct chaos drills by injecting verification failures, network partitions, and stale replica lag into sandbox environments to verify that recovery paths execute correctly.

## **Cross-Canon Handoff Map**

The truth-management patterns established in this report provide critical inputs and interfaces for other reports across the *AI Engineering Systems Canon*:

| Target Document | Directed Asset / Metric | Programmatic Consumer | Downstream System Dependency |
| :---- | :---- | :---- | :---- |
| **AI-ENG-S (Production Pathologies)** | Verification failure rates and execution discrepancies. | Diagnostic monitors and anomaly detection engines. | Downtime alerts and behavior auditing systems. |
| **AI-ENG-T (Permission Boundaries)** | Tenant and permission verification tags. | Security gateways and data containment boundaries. | Strict tenant boundary enforcement engines. |
| **AI-ENG-U (Dependency Risk)** | External status degradation and API metrics. | Outage monitors and system availability engines. | Fail-closed holds and automated backup gateways. |
| **AI-ENG-V (Resource Abuse)** | Duplicate transaction checks and loop metrics. | Cost trackers and compute limit engines. | Rate-limiting rules and runaway loop blockages. |
| **AI-ENG-W (Fallback & Degraded)** | Degraded mode signals and fallback routing. | Platform state machines and routing engines. | Reader-fallback rules when writer nodes are down. |
| **AI-ENG-X (Transparent Status)** | Verified task status and ground truth state. | User messaging portals and notification pipelines. | Grounded status reports to prevent false claims. |
| **AI-ENG-Y (Human Review)** | Escalation triggers and unverified write tags. | Developer ticket queues and admin portals. | Manual review pipelines for high-risk actions. |
| **AI-ENG-Z (Telemetry)** | Execution-to-reconciliation telemetry logs. | Telemetry logs and system health dashboards. | Production alert systems and SRE metrics boards. |
| **AI-ENG-AA (Agent Evals)** | Trace replay packages and failure logs. | CI/CD testing pipelines and testing frameworks. | Automatic evaluation of model reliability updates. |
| **AI-ENG-AB (Audit & Replay)** | Append-only Action Ledger entries. | Security compliance monitors and audit ledgers. | Cryptographic verification and compliance tools. |
| **AI-ENG-AC (Incident Response)** | Compensation failure codes and escalation paths. | Incident management platforms and paging tools. | Emergency automated playbooks for SRE teams. |
| **AI-ENG-AJ (Reference Architecture)** | End-to-end verification lifecycle blueprints. | Systems development toolkits and SDK tools. | Base reference codebases for agent platforms. |

## **Durable Architectural Principles**

To ensure consistency and reliability across all tool-using agent implementations, systems must adhere to five core architectural principles:

1. **Decouple Execution from Verification:** An execution response is merely an acknowledgment of a request, not proof of a completed state change. The system must treat the observed tool response as unverified evidence. The internal task state must remain locked until an active verification check confirms the change against the authoritative source of record.  
2. **Bypass Lag and Caches During Verification:** To prevent false validation results, verification reads must target primary writer nodes and bypass intermediate caching layers or lagging read replicas.  
3. **Represent Intermediate States Explicitly:** Distributed transactions regularly encounter partial failures, asynchronous delays, and propagation lag. Systems must represent these intermediate states explicitly within the orchestrator state machine, avoiding flattening execution states into a binary success or failure.  
4. **Deploy Deterministic Recovery Policies:** Failure recovery must be driven by deterministic, policy-defined pathways mapped to specific discrepancy types. This minimizes reliance on unsupervised language model decision-making during rollbacks, compensating sagas, or escalations.  
5. **Ground Model Context and Reporting in Verified State:** An agent's generation context and subsequent planning steps must be grounded entirely in verified, reconciled system state. User-facing status reports must reflect this ground truth, preventing the model from presenting ungrounded success claims to the client.

---