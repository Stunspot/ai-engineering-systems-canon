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

[← Back to Canon Map](../canon-map.md)