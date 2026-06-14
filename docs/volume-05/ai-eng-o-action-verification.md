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

[← Back to Canon Map](../canon-map.md)