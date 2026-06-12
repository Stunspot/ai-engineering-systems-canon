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

[← Back to Canon Map](../canon-map.md)