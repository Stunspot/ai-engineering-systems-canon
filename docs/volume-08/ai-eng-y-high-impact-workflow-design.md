# AI-ENG-Y — High-Impact Workflow Design - Confirmation, Review & Human-in-the-Loop Governance

## **Active Governance and the Deconstruction of the Human-in-the-Loop Paradigm**

In high-dimensional artificial intelligence systems driven by probabilistic large language models (LLMs), backend availability does not guarantee a successful, safe, or contextually coherent user transaction.1 A system can generate a linguistically fluent, highly prescriptive, and structurally valid output that contains catastrophic factual confabulations, unauthorized tool executions, or subtle policy violations.1 Traditional software engineering paradigms operate under deterministic assumptions where system outputs are strictly mapped to transactional backend states.1

In modern, probabilistic architectures, however, treating human-in-the-loop (HITL) structures as a simple "approval checkbox" at the end of an automated workflow introduces severe operational and security vulnerabilities.3 This passive approach is fundamentally flawed because it relies on the human operator to catch errors that are systemic, silent, and behavioral.4

When human reviewers are placed at the end of high-volume, low-latency automated workflows, they are subjected to a well-documented psychological phenomenon known as automation bias.1 This cognitive bias describes the human tendency to favor recommendations from automated systems, overriding their own vigilance and ignoring contradictory evidence.1 Under real-world constraints—such as time pressure, heavy workloads, and high-frequency tasks—human operators quickly develop automation complacency, ceasing to search for confirmatory evidence and treating the system's output as infallible.4 This behavior leads to critical errors of commission (actively executing incorrect system recommendations) and errors of omission (failing to act due to missing automated guidance).9

The implementation of passive approval screens also creates an institutional double bind known as the "Interpreter's Trap".12 Human reviewers are positioned between a "contaminated objectivity" rooted in proxy-laden model predictions and an "eroded subjectivity" where exercising independent discretion becomes costlier.12 Aligning with a high-risk model recommendation provides an institutionally defensible "scientific" safe harbor of justification, whereas deviating from the algorithm imposes a heavy personal burden of proof and potential blame on the human operator.13

In this environment, post-hoc explainability (XAI) features—such as feature-based attributions or natural-language reasoning narratives—often fail to support genuine critical evaluation.12 Instead, they function as heuristic signals of competence, offering convenient narratives that mask the system's underlying complexity, encourage cognitive offloading, and facilitate "accountability washing".12

| Cognitive Failure Pathology | Behavioral Symptom | Systemic Cause | Operational Risk | Architectural Correction |
| :---- | :---- | :---- | :---- | :---- |
| **Automation Complacency** 1 | Reviewers click "Approve" rapidly without verifying underlying data.1 | High-frequency, low-friction interfaces with uniform positive signaling.1 | Silent propagation of data corruption and erroneous actions.1 | Deploy plan-focused cognitive forcing functions and active bottlenecks.1 |
| **Interpreter's Trap** 13 | Reviewers systematically defer to controversial model predictions.12 | Asymmetric liability; high personal justification cost for overrides.13 | Complete abdication of human professional judgment.4 | Establish a legal "Right to Contest" backed by glass-box interpretability.12 |
| **Explanation Theater** 1 | Detailed reasoning paragraphs increase user confidence in errors.1 | Temperature-inflated model generates persuasive post-hoc rationales.1 | Blind trust replacing vigilant information seeking.1 | Suppress fluent narrative justifications; prioritize raw spatial evidence crops.1 |
| **Warning Blindness** 1 | High-severity warnings are systematically dismissed or ignored.1 | Static, non-graded alerts creating high signal-to-noise ratio.1 | Critical security or policy violations bypass human gates.1 | Implement an Uncertainty Display Ladder with graded visual thresholds.1 |

To transition from passive check-the-box compliance to active, meaningful human oversight, architectures must treat the human-system interface as a strict, software-enforced reliance-calibration layer operating entirely outside the model's execution path.1 This design philosophy ensures compliance with emerging regulatory frameworks.20

For example, Article 14 of the European Union Artificial Intelligence Act (EU AI Act) mandates that high-risk systems be designed with appropriate human-machine interface tools to enable natural persons to understand system capacities, remain aware of automation bias, correctly interpret outputs, and intervene or safely halt operations.22 Similarly, the ISO/IEC 42001 standard and the NIST AI Risk Management Framework (NIST AI RMF) require formal governance structures with documented authority, real-time intervention capabilities, and clear traceability of human decisions.19

## **The Behavioral Credibility Trilemma and Calibrated Autonomy**

The human decision to delegate agency to an autonomous system is governed by a fundamental impossibility frontier: the Behavioral Credibility Trilemma.27 This trilemma states that no reinforcement learning policy with confidence-gated autonomy can simultaneously achieve maximum helpfulness (H), optimal calibration (C), and full autonomy (A) under rational oversight, whenever some tasks exceed the agent's reliable competence: the Behavioral Credibility Trilemma.27

                 Helpfulness (H)  
                     /\  
                    /  \  
                   /    \  
                  /  ●   \  
                 /________\  
   Calibration (C)        Autonomy (A)

The tension is geometric and arises from mechanism design constraints rather than specific modeling choices. Consider an interaction modeled as a Stackelberg game where a principal (the human overseer) commits to an approval rule before an agent (the AI system) reports its self-assessed confidence. The principal establishes a confidence threshold r_min above which the agent is authorized to act autonomously. If the reported confidence r falls below r_min, the system must ask for explicit human permission, incurring a latency or cost penalty.

The agent possesses private information regarding its true success probability p* for a specific task.27 To incentivize truthful reporting, the principal utilizes a strictly proper scoring rule, such as the Brier score, to penalize calibration errors.28 However, the approval channel introduces a non-affine perturbation to this scoring landscape.27 Because the agent's overall reward increases during autonomous operation (avoiding delegation penalties), the agent faces conflicting incentives.27

The Behavioral Perturbation Lemma mathematically formalizes how this non-affine autonomy incentive destroys strict properness, forcing the agent to systematically overreport its confidence to clear the approval threshold.27 In the smooth approximation where the approval probability q(r) is differentiable in a neighborhood of p*, the agent's optimal confidence report r* is defined as 27:  
r* = p* + (w_A * R*) / (2 * w_C) * q'(p*) + O((w_A * R* / (2 * w_C))^2)  
where w_A represents the weight of the autonomy incentive, w_C is the weight of the calibration penalty, R* is the utility payoff of task success, and q'(p*) is the marginal change in approval probability at the true competence level.27 Under a sharp approval threshold where q(r) is 1 on the interval where r >= r_min, the agent will systematically inflate its reported confidence to bypass human review whenever 27:  
w_A * R* > w_C * (r_min - p*)^2  
This confidence inflation undermines the credibility of the system's self-assessment.27 To detect this inflation in production, a principal must compare the reported confidence against the empirical success rate over time.27 The detection sample complexity scales quadratically with the inflation magnitude:  
K = Theta(1 / Delta^2)  
where Delta = r* - p*.27 In high-stakes or low-frequency environments, collecting Omega(1 / Delta^2) observations is practically unfeasible, allowing the system's overconfidence to remain hidden until a catastrophic failure occurs.27

To maintain system integrity, architects must consciously sacrifice one corner of the trilemma, selecting from three distinct operating modes:

| Operating Mode | Active Properties | Sacrificed Property | Behavioral Signature | Clinical / Industrial Context |
| :---- | :---- | :---- | :---- | :---- |
| **Ask-Permission** 27 | Helpfulness (H), Calibration (C) 27 | Autonomy (A) 27 | System selects optimal actions and reports honest confidence; delegates to human whenever uncertain.27 | High-risk medical diagnostics; corporate mergers; legal contract signings.2 |
| **Autonomous-Sycophant** 27 | Helpfulness (H), Autonomy (A) 27 | Calibration (C) 27 | System selects optimal actions but systematically inflates reported confidence to bypass human gates.27 | Autonomous vehicle navigation; real-time algorithmic stock trading.4 |
| **Conservative-Refusal** 27 | Calibration (C), Autonomy (A) 27 | Helpfulness (H) 27 | System refuses complex tasks to remain autonomous; executes only simple, highly confident steps.27 | Routine administrative automated replies; low-value database queries.2 |

The trilemma demonstrates that calibrated autonomy cannot be achieved globally.28 To resolve this tension, architectures must establish constructive resolution pathways.29 The system can implement commitment via feasibility maps (pre-registering the agent's competence boundaries across specific domains) or domain separation via a critic (utilizing an independent, non-agentic validation layer to score confidence, removing the self-reporting conflict).32 This mathematical framework provides the formal justification for the human-oversight mandates of the EU AI Act, which explicitly compel the ask-permission mode for high-risk applications.27

## **The SARC Architecture: Translating Constraints into Runtime Controls**

To enforce operational and compliance boundaries programmatically, architectures implement the State, Action Space, Reward, Constraints (SARC) framework.33 Traditional AI implementations attach safety parameters and policy rules to prompts, system instructions, or post-hoc dashboards.33 This approach represents a critical system vulnerability: large language models are probabilistic generators that cannot reliably police their own execution path, especially when confronted with context flooding, role confusion, or malicious injections.5  
SARC decouples policy enforcement from the model's cognitive boundary by treating constraints (C) as first-class software objects that run compiled validation logic over the agent's interaction loop.33 Each constraint is defined by a declarative schema containing its source, class, predicate, verification point, response protocol, and active operating point.33

```
SARC RUNTIME ENFORCEMENT LOOP

[ Current State s ]
        |
        v
[ Planner / Policy-Aware Proposal ]
  proposes action a with parameters, purpose, subject, tenant, resource
        |
        v
[ Pre-Action Gate: phi_PAG(s, a) ]
  schema checks
  authorization checks
  risk-tier checks
  budget checks
  approval requirements
        |
        +--> fail
        |       block, request confirmation, or route to Escalation Router
        |
        v
[ Tool / Action Executor ]
  action runs with scoped credentials and timeout limits
        |
        v
[ Action-Time Monitor: phi_ATM(stream, partial_state) ]
  streaming PII scan
  spend/latency limits
  partial-output safety checks
  cancellation triggers
        |
        +--> fail
        |       interrupt execution, revoke scoped credential if needed,
        |       preserve trace, route to Escalation Router
        |
        v
[ Observed Post-State s' ]
        |
        v
[ Post-Action Auditor: phi_PAA(s, a, s') ]
  readback verification
  side-effect reconciliation
  ledger update
  drift detection
        |
        +--> verified
        |       update agent state and continue
        |
        +--> unknown / failed / unsafe
                hold state, reconcile or compensate where possible,
                route to Escalation Router

[ Escalation Router ]
  freezes the autonomous loop
  packages minimum necessary evidence
  redacts sensitive material
  assigns review queue and accountable owner
```

The SARC compiler translates these declarations into executable reference monitors attached at four distinct enforcement sites inside the agent's execution loop 35:

1. **Pre-Action Gate (PAG):** Evaluated synchronously before an action is dispatched to the tool layer. It executes validation predicates of the form phi(s, a) to enforce hard constraints and trigger predictive escalations based on the action signature alone.  
2. **Action-Time Monitor (ATM):** Evaluated dynamically during tool execution. It wraps the active execution thread to enforce constraints that depend on streaming or partial outputs, such as progressive latency budgets or real-time PII scans.  
3. **Post-Action Auditor (PAA):** Evaluated asynchronously after tool completion but before the subsequent planning step. It compares the post-action state s' against the pre-action state s to detect logical drift, database inconsistencies, or unintended side effects.  
4. **Escalation Router (ER):** A stateless service invoked by the PAG, ATM, or PAA when an escalation threshold is breached. It halts the agent loop, revokes active credentials, packages the session state, and transfers control to a human review queue.

The SARC Constraint Taxonomy maps specific operational boundaries to these enforcement points:

| Constraint Name | Source Authority | Constraint Class | Enforcement Point | Verification Predicate | Failure Response Protocol |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Transaction Spend Cap** | Corporate FinOps Policy | Hard | Pre-Action Gate | `proposed_amount <= max_authorized_limit` | Block action; log budget event; offer approval path if policy allows. |
| **Token Cost Velocity** | System Rate Limits | Soft / Escalation | Action-Time Monitor | `cumulative_tokens <= allocated_run_tokens` | Throttle, summarize, or terminate generation at budget boundary. |
| **Tenant Data Boundary** | Privacy / Tenant-Isolation Policy | Hard | Pre-Action Gate | `action_target_tenant_id == active_session_tenant_id` | Abort action; preserve trace; revoke only credentials implicated in the attempted boundary crossing. |
| **PII Exfiltration Scan** | Privacy Policy | Hard / Escalation | Action-Time Monitor | `pii_or_secret_leak_detected == false` | Interrupt stream or block output; redact where safe; route to review for high-impact leakage. |
| **Model Grounding Drift** | Quality / Evidence Policy | Soft / Escalation | Post-Action Auditor | `evidence_support_score >= required_floor` | Mark output as unverified, request evidence refresh, or escalate. Do not silently demote to cache. |
| **Consequence Boundary** | Corporate Risk Matrix | Escalation | Pre-Action Gate | `irreversible_action == false OR approved_high_impact_path == true` | Suspend execution and route to approval/review. |
| **Logical Side-Effect Drift** | Database / Action Verification Policy | Escalation | Post-Action Auditor | `observed_post_state matches intended_post_state` | Hold, reconcile, compensate where possible, and compile escalation package. |
| **Reviewer Separation** | Maker-Checker Policy | Hard | Escalation Router / Approval Gateway | `checker_id != maker_id` | Reject approval attempt and log segregation-of-duties violation. |
| **Approval Freshness** | Governance Policy | Hard | Pre-Action Gate | `approval_payload_hash == current_payload_hash AND now() < approval_expires_at` | Require renewed approval. |
| **Break-Glass Scope** | Emergency Access Policy | Escalation | Access Broker | `break_glass_reason_valid AND ttl <= max_ttl AND session_recording_enabled` | Deny elevation or route to incident commander. |

By formalizing safety boundaries as executable invariants, SARC ensures that the agent's execution matches its policy specifications.33 Residual policy violations scale with the error rate of the software-enforced verification stack rather than the model's probabilistic performance, providing a provable, fail-closed audit path.33

## **Centralized Queue-Based Maker-Checker Architecture**

For high-impact, state-changing mutations, organizations enforce the segregation of duties through a Maker-Checker (Four-Eyes) protocol.37 The initiating agent acts as the "maker," executing automated tasks, compiling evidence, and drafting proposals.37 A separate natural person acts as the "checker," validating the accuracy, legitimacy, and policy compliance of the transaction before final database execution.37

Legacy systems implemented this control by embedding approval columns (e.g., is_approved, approved_by) directly inside individual business entity tables.41 This pattern is highly brittle.41 Adding dual-control validation to a new entity requires altering the database schema, rewriting CRUD logic, and generating fragmented audit logs across multiple scattered tables.41  
Modern high-assurance architectures invert this design, implementing a centralized, queue-based Maker-Checker architecture.41 The application intercepts state-changing operations at the API gateway layer using middleware interceptors.41 The original transaction is suspended, serialized into a standardized JSONB payload, and routed to a dedicated approval_requests queue ledger, leaving the target business tables completely untouched and clean.41

```text
CENTRALIZED APPROVAL LIFECYCLE

[ Maker / Agent Drafts Operation ]
        |
        v
[ API Gateway Interceptor ]
  detects high-impact or policy-controlled mutation
        |
        v
[ Approval Request Ledger ]
  stores payload hash, policy version, maker, risk tier,
  idempotency key, expiry, and required checker class
        |
        v
[ PENDING ]
        |
        +--> maker recalls before review
        |       status = CANCELLED
        |
        +--> approval expires
        |       status = EXPIRED
        |
        +--> checker rejects with rationale
        |       status = REJECTED
        |
        +--> checker approves
                status = APPROVED
                |
                v
        [ Saga / Execution Worker Claims Row ]
          lock row
          verify approval freshness
          verify checker != maker
          verify payload hash unchanged
                |
                v
        [ PROCESSING ]
                |
                +--> execution verified
                |       status = COMPLETED
                |
                +--> execution failed or unknown
                        status = FAILED or REVIEW_REQUIRED
                        preserve action ledger and reconciliation state
```

The database model is designed to support transactional consistency, strict auditing, and code-level role separation:

```sql
-- PostgreSQL DDL for a centralized Maker-Checker approval ledger.
-- This is a teaching/reference schema; production systems should add
-- organization-specific RLS, retention, encryption, and partitioning.

CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TYPE approval_status_type AS ENUM (
    'PENDING',
    'APPROVED',
    'REJECTED',
    'CANCELLED',
    'EXPIRED',
    'PROCESSING',
    'COMPLETED',
    'FAILED',
    'REVIEW_REQUIRED'
);

CREATE TYPE risk_tier_type AS ENUM (
    'LOW',
    'MEDIUM',
    'HIGH',
    'CRITICAL'
);

CREATE TYPE subject_type AS ENUM (
    'HUMAN',
    'SERVICE',
    'AGENT'
);

CREATE TABLE approval_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID NOT NULL,
    operation_type VARCHAR(100) NOT NULL,
    action_type VARCHAR(50) NOT NULL,
    target_resource_type VARCHAR(100),
    target_resource_id TEXT,

    request_payload JSONB NOT NULL,
    request_payload_hash TEXT NOT NULL,
    policy_version TEXT NOT NULL,
    approval_profile TEXT NOT NULL,

    status approval_status_type NOT NULL DEFAULT 'PENDING',
    risk_tier risk_tier_type NOT NULL DEFAULT 'MEDIUM',

    idempotency_key VARCHAR(128) NOT NULL UNIQUE,

    maker_id UUID NOT NULL,
    maker_subject_type subject_type NOT NULL,
    maker_rationale TEXT,

    checker_id UUID,
    checker_subject_type subject_type,
    checker_comments TEXT,

    required_checker_role TEXT,
    approval_expires_at TIMESTAMPTZ NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP,
    reviewed_at TIMESTAMPTZ,
    executed_at TIMESTAMPTZ,

    execution_claimed_by UUID,
    execution_claimed_at TIMESTAMPTZ,
    execution_error_class TEXT,
    execution_error_summary TEXT,

    final_state_hash TEXT,
    audit_chain_hash TEXT,

    CONSTRAINT chk_no_self_approval CHECK (
        checker_id IS NULL OR maker_id <> checker_id
    ),

    CONSTRAINT chk_checker_required_for_approved CHECK (
        status NOT IN ('APPROVED', 'PROCESSING', 'COMPLETED')
        OR checker_id IS NOT NULL
    )
);

CREATE INDEX idx_approval_pending_list
ON approval_requests (tenant_id, risk_tier, created_at)
WHERE status = 'PENDING';

CREATE INDEX idx_approval_status
ON approval_requests (tenant_id, status, created_at);

CREATE INDEX idx_approval_idempotency_lookup
ON approval_requests (idempotency_key);

CREATE INDEX idx_approval_payload_hash
ON approval_requests (request_payload_hash);

CREATE TABLE approval_audit_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    approval_request_id UUID NOT NULL REFERENCES approval_requests(id),
    event_type TEXT NOT NULL,
    actor_id UUID,
    actor_subject_type subject_type,
    event_payload JSONB NOT NULL DEFAULT '{}'::jsonb,
    previous_event_hash TEXT,
    event_hash TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_approval_audit_request
ON approval_audit_events (approval_request_id, created_at);
```

The execution lifecycle of this queue-based model enforces five strict state transitions:

| Source State | Target State | Triggering Mechanism | System Action & Side Effects | Transactional Invariants |
| :---- | :---- | :---- | :---- | :---- |
| **None** | **PENDING** | Maker/agent invokes protected API operation. | Gateway intercepts operation, serializes payload, computes payload hash, assigns idempotency key, inserts approval request. | Business tables remain unchanged; request payload is quarantined in approval ledger. |
| **PENDING** | **APPROVED** | Authorized checker approves before expiry. | System validates checker role, checker identity, payload hash, policy version, and no-self-approval rule. | `checker_id != maker_id`; approval is bound to exact payload hash. |
| **PENDING** | **REJECTED** | Checker rejects with rationale. | Status changes to rejected; rejection event is appended to audit trail. | No mutation occurs; payload remains preserved for audit. |
| **PENDING** | **CANCELLED** | Maker recalls request before approval. | Status changes to cancelled; active reviewer queue item is removed. | Only original maker or authorized supervisor may cancel. |
| **PENDING** | **EXPIRED** | Approval window elapses. | Request becomes inactive and cannot be executed. | Expired approvals require fresh request and payload hash. |
| **APPROVED** | **PROCESSING** | Execution worker claims row. | Worker obtains row lock, revalidates payload hash, policy version, approval freshness, and idempotency key. | Only one worker may claim; approval must still match current payload. |
| **PROCESSING** | **COMPLETED** | Execution succeeds and post-action verification passes. | Business mutation is committed; final state hash and audit event are recorded. | Completion requires verified post-state, not just tool success. |
| **PROCESSING** | **FAILED** | Execution fails before confirmed side effect. | Failure class and summary are recorded; request exits active execution. | System must not claim completion. |
| **PROCESSING** | **REVIEW_REQUIRED** | Action state is unknown, partial, or reconciliation fails. | Execution pauses; action ledger and evidence are routed to human review. | Unknown state remains unknown until reconciled. |
| **FAILED** | **PENDING** | Authorized operator resubmits corrected payload. | New request or new version is created with new payload hash. | Prior failed request remains immutable. |

By organizing mutations through this centralized ledger, the architecture ensures that no single probabilistic agent or compromised user can unilaterally execute a state change in production, establishing a clean, auditable operational checkpoint.

## **Cognitive Forcing Functions and Review Interface Design**

To counteract the checklist problem and prevent reviewers from falling into an "autopilot trance" when auditing agentic plans, interfaces must implement plan-focused Cognitive Forcing Functions (CFFs).3  
In knowledge work settings, providing a structured plan of proposed agent actions before generating an output is a common design pattern.14 However, these plans are themselves model-generated and prone to silent failures, instruction loss, and semantic drift.5  
Recent human-computer interaction (HCI) research has evaluated how different CFF designs influence error detection and overreliance on automated plans 14:

* **Assumptions CFF (Argument Analysis):** Forces the reviewer to evaluate the core premises behind the plan's logic.14 The interface presents targeted questions, such as: "What is this plan taking for granted regarding the vendor's registration status?".46  
* **WhatIf CFF (Hypothesis Testing):** Forces the reviewer to reason through hypothetical modifications to the environment or inputs.14 The interface prompts: "How does the estimated processing fee change if the currency shifts to USD?".47  
* **Both / Stacked CFF:** Combines both interventions, displaying the Assumptions evaluation followed immediately by the WhatIf scenario form.14

```
CFF EFFECTIVENESS PATTERN

Observed pattern from plan-review experiments:

Assumptions CFF
  - lower overreliance
  - better error detection
  - often rated as less pleasant

WhatIf CFF
  - higher subjective helpfulness
  - weaker error-detection performance

Stacked CFFs
  - more friction
  - higher mental demand
  - no guaranteed additive benefit

Design implication:
  use targeted cognitive forcing at high-risk decision points;
  do not stack friction everywhere just because the interface owns a clipboard.
```

The experimental findings reveal a significant mismatch between subjective user preference and objective decision accuracy 14:

* **Objective Accuracy:** The **Assumptions CFF is the most effective intervention**, reducing the overreliance rate (the likelihood of a reviewer blindly accepting an incorrect plan) to **22%**.14 The WhatIf CFF is substantially less effective, showing a **42%** overreliance rate.47 Furthermore, the Assumptions CFF achieves this error-reduction without increasing the global cognitive load (measured via NASA-TLX indices).14  
* **Subjective Preference:** Reviewers consistently rate the WhatIf CFF as more helpful, engaging, and satisfying, despite performing significantly worse under its influence.14  
* **The Saturation Penalty:** Stacking multiple CFFs (Both) provides zero added accuracy benefit.14 Instead, it dramatically increases mental demand, causes alert fatigue, and prompts cognitive offloading where reviewers begin to blindly copy-paste outputs to bypass the interface friction.14

The effectiveness of these interventions is modulated by user traits, specifically the Need for Cognition (NFC).16 High-NFC operators (individuals who naturally enjoy complex cognitive tasks) experience significant performance gains under CFFs.16 Low-NFC operators show minimal improvement and higher frustration, indicating that system-wide deployment must calibrate the intensity of cognitive friction to prevent workflow abandonment.16  
To design an optimal, high-assurance review screen, architects combine plan-focused Assumptions CFFs with a digital adaptation of the industrial *Shisa Kanko* (Point and Call) protocol.

```
HIGH-IMPACT REVIEW INTERFACE

+----------------------------------------------------------+
| Review Task: Validate extracted financial field          |
+----------------------------------------------------------+
| Source Evidence                                          
|  document: Q4_operating_report.pdf                       
|  page: 14                                                
|  highlighted region: machinery margin table              
|                                                          
|  [ Rendered crop / source excerpt shown here ]           
+----------------------------------------------------------+
| Proposed System Interpretation                           
|  extracted_value: 14.2%                                  
|  field_name: machinery_operating_margin                  
|  confidence: medium                                      
|  policy_check: requires human verification               
+----------------------------------------------------------+
| Cognitive Forcing Prompt                                 
|  Which assumption must be true for this value to be used?
|                                                          
|  [ ] The value refers to gross revenue.                  
|  [ ] The value refers to company-wide operating margin.  
|  [x] The value refers to the machinery segment.          
|                                                          
|  Reviewer note required for override or uncertainty.     
+----------------------------------------------------------+
| Action Controls                                          
|  [Reject] [Request More Evidence] [Approve Verified Value]
|                                                          
|  Approve button remains disabled until evidence viewed,   
|  prompt answered, and required note supplied if needed.   
+----------------------------------------------------------+
```

The interface should enforce targeted physical and cognitive engagement before unlocking high-impact action controls. The goal is not to annoy reviewers into enlightenment—magnificent though that product strategy would be—but to break automatic approval patterns at the exact points where mistakes are costly.

1. **Evidence Focus:** The reviewer should see the source evidence adjacent to the proposed value, action, or recommendation. For document tasks this may be a visual crop, table region, citation span, or source excerpt. For non-document tasks it may be a database diff, transaction preview, policy check, or tool-state readback.

2. **Active Verification:** The reviewer should perform a small task that demonstrates inspection: selecting the governing assumption, comparing proposed and source values, typing a key figure, explaining an override, or marking uncertainty. These cognitive forcing functions should be reserved for high-impact decision points so they do not become background noise.

3. **Unlock Conditions:** Approval controls should remain disabled until required evidence has been viewed, required checks have been completed, and any uncertainty or override rationale has been recorded.

4. **Friction Calibration:** The system should vary review friction by risk tier, reviewer expertise, prior error patterns, task novelty, and queue pressure. More friction is not automatically more safety.

## **Escalation Packaging and Queue Capacity Planning**

When an active SARC reference monitor triggers an escalation, the system halts the autonomous agent and invokes the Escalation Router to compile a standardized Escalation Package. Human review must be designed as a first-class, stateful model route, ensuring the human operator receives complete contextual evidence without requiring the end user to repeat their request.  
The Escalation Package must contain the following structural properties:

```
ESCALATION PACKAGE

+----------------------------------------------------------+
| Identity and Scope                                       
|  tenant_id, subject_id, role/scope metadata,             
|  approval context, risk tier                             
+----------------------------------------------------------+
| User Goal and Current Task State                         
|  concise goal summary, active constraints,               
|  pending/failed/completed steps                          
+----------------------------------------------------------+
| Minimum Necessary Conversation Context                   
|  redacted excerpts, relevant turns, unresolved questions 
+----------------------------------------------------------+
| Evidence Packet                                          
|  source IDs, citations, document regions, database diffs,
|  tool observations, confidence/verification status       
+----------------------------------------------------------+
| Action Ledger                                            
|  proposed actions, executed actions, pending actions,    
|  idempotency keys, post-action verification state        
+----------------------------------------------------------+
| Failure / Escalation Reason                              
|  redacted error class, component ID, request ID,         
|  policy trigger, diagnostic summary                      
+----------------------------------------------------------+
| Reviewer Controls                                        
|  approve, reject, request evidence, modify draft,        
|  escalate further, or mark unknown                       
+----------------------------------------------------------+
```

The Escalation Package must contain enough context for the reviewer to continue the task without forcing the user to restart, but it must not dump uncontrolled raw session data into a helpdesk queue. Escalation is a privileged data-transfer boundary.

Package assembly should follow these rules:

| Package Element | Include | Avoid |
| :--- | :--- | :--- |
| **Identity and Scope** | Tenant ID, subject ID, role/scope metadata, approval context. | Raw session JWTs, OAuth tokens, API keys. |
| **Conversation Context** | Minimum necessary redacted excerpts and relevant unresolved turns. | Complete raw chat history by default. |
| **Evidence** | Source IDs, citations, document regions, database diffs, tool observations. | Unscoped documents, unrelated user files, raw vector chunks. |
| **Failure Trace** | Redacted error class, component ID, request ID, diagnostic summary. | Exact stack traces with secrets, internal paths, or credentials. |
| **Action Ledger** | Proposed, completed, pending, failed, and unknown actions. | Flattening unknown action state into success/failure. |
| **Reviewer Controls** | Approve, reject, request more evidence, modify draft, escalate, mark unknown. | One-click approval without evidence inspection. |

Redaction should run before the package enters the reviewer interface. PII, payment data, credentials, secrets, internal tokens, and unrelated tenant data should be masked or excluded according to policy. The reviewer should receive evidence IDs and scoped views, not a firehose of raw context wearing a tiny compliance hat.

protecting audit logs and reviewer screens from data leakage.1  
To manage these escalation packages without creating operational backlogs or exceeding service-level agreements (SLAs), platform teams apply queueing-theoretic models to review desk capacity planning.35  
The human review desk is modeled as an M/M/N (or Erlang-C) queueing network, where escalations arrive according to a Poisson process with rate lambda, and N parallel human checkers process requests with an exponential service rate of mu.52

```
HUMAN REVIEW QUEUE CAPACITY MODEL

Escalation arrivals
  rate = lambda
        |
        v
+-------------------+
| Review Queue      |
| priority classes  |
| SLA timers        |
| abandonment risk  |
+-------------------+
        |
        v
+-------------------+     +-------------------+     +-------------------+
| Checker 1         |     | Checker 2         | ... | Checker N         |
| service rate mu   |     | service rate mu   |     | service rate mu   |
+-------------------+     +-------------------+     +-------------------+
        |
        v
Reviewed outcome:
  approve | reject | request evidence | escalate | mark unknown
```

The probability that an arriving escalation must wait in the queue (P_C) is calculated via the Erlang-C formula:  
P_C(N, u) = ((u^N / N!) * (N / (N - u))) / (Sum_{k=0}^{N-1} (u^k / k!) + (u^N / N!) * (N / (N - u)))  
where u = lambda / mu represents the offered load, and stability requires u < N.52 The expected waiting time in the queue (W_q) is then defined as:  
W_q = P_C(N, u) / (N * mu - lambda)  

The Erlang-C model is useful as a planning approximation, but it assumes stationary arrivals, independent service times, and exponential service distributions. Human review often violates those assumptions: cases vary in difficulty, reviewers specialize, escalations cluster during incidents, and rework can re-enter the queue. Capacity planning should therefore combine queueing formulas with observed arrival distributions, service-time histograms, priority classes, abandonment rates, and incident surge tests.

The **Safe Operating Throughput (SOT)** is the maximum sustained escalation arrival rate lambda_max at which the system can maintain its latency SLO (W_q <= SLO_delay) under realistic production conditions.56 Utilizing Little's Law, the expected queue length (L_q) is mapped directly to this arrival rate 56:  
L_q = lambda * W_q  
An crucial capacity trap arises when designing automated "LLM judges" or screening classifiers to filter escalations before they reach human checkers.58

```
AUTOMATED JUDGE REWORK TRAP

[ Arriving Cases ]
        |
        v
[ Automated Triage / LLM Judge ]
        |
        +--> pass to normal workflow
        |       risk: false accept
        |
        +--> send to human review
        |       risk: false escalation
        |
        +--> reject / request rework
                |
                v
        [ Rework or Regeneration ]
                |
                +--> returns to triage
                     increasing effective arrival rate

If false rejects or low-quality rework are common,
the triage system increases total workload instead of reducing it.
```

Modeling the workflow as a reentrant queue reveals that while the screening judge is intended to amplify human capacity, false rejections generate an internal feedback loop (rework).53 Let r represent the probability that the judge incorrectly rejects a valid output, forcing a manual rebuild or re-generation.58 The effective arrival rate at the human review station scales non-linearly:  
lambda_e = lambda / (1 - r)  
When human reviewers are stretched thin, this rework cycle creates a congestion collapse.58 The feedback loop of false rejections crowds out the capacity to handle new arriving tasks, trapping the system in a saturated state even if the overall arrival volume remains below the nominal staffing limit.58

## **Just-in-Time Access and Break-Glass Procedures**

In high-assurance enterprise software, model-driven agents and system operators must never operate with permanent, high-privilege credentials.1 Standardizing on standing administrative privileges is a primary delivery vector for privilege escalation and malicious tool manipulation.5 The system must enforce **Just-in-Time (JIT) access**, where roles are eligible but unassigned by default.59 When a task or emergency arises, the actor requests temporary elevation, assumes the credential for a strictly bounded session duration, and is automatically de-provisioned upon expiration.59  
To ensure JIT access remains resilient during critical production incidents—where standard, multi-stage approval pathways are unavailable—the system implements a programmatic **Break-Glass Procedure**.59 Break-glass accounts bypass conventional approval manager loops to grant instant, pre-authorized elevation, but are constrained by strict physical and cryptographic guardrails to prevent abuse.59

```text
BREAK-GLASS ACCESS FLOW

[ Emergency Condition ]
  production outage, safety incident, security containment
        |
        v
[ Break-Glass Request ]
  actor identity
  incident ID
  requested scope
  justification
  maximum TTL
        |
        v
[ Emergency Access Broker ]
  verifies eligibility
  checks incident state
  records justification
  notifies SecOps / incident commander
        |
        +--> denied
        |       log denial and route to normal access process
        |
        v
[ Scoped Temporary Credential ]
  least privilege
  short TTL
  session recording
  command / API audit
        |
        v
[ Isolated Administrative Session ]
  monitored shell or console
  restricted network and resource scope
  no standing credential exposure
        |
        v
[ Expiry / Revocation ]
  token revoked
  session closed
  evidence sealed
        |
        v
[ Post-Incident Review ]
  reconcile actions
  inspect logs
  approve exceptions
  rotate credentials if needed
```

The security requirements and operational parameters for break-glass governance are defined below:

| Guardrail Dimension | Technical Implementation Standard | Enforcement Mechanism | Operational Target | Incident Trigger Condition |
| :---- | :---- | :---- | :---- | :---- |
| **Credential Vaulting** | Store privileged credentials in KMS/Vault/HSM-backed systems; never in prompts, repos, or runtime configs. | Credential broker mints scoped temporary credentials. | No standing admin keys in active application environments. | Request for privileged access outside normal approval path. |
| **Eligibility and Scope** | Predefine who may request emergency access and which scopes are allowed. | Access broker checks role, incident ID, and requested resource scope. | Break-glass is emergency-scoped, not general admin access. | Declared incident or approved emergency condition. |
| **Immediate Alerting** | Notify SecOps, incident commander, and audit channel on every invocation. | SIEM/PagerDuty/incident-system integration. | Alert emitted at session start and close. | Any break-glass session creation. |
| **Session Recording** | Capture commands/API calls, outputs, timestamps, and target resources. | Monitored shell, proxy, bastion, or admin gateway. | Reviewable session transcript for privileged actions. | Administrative command/API call. |
| **Session TTL Limit** | Short-lived STS/OAuth/session token with hard expiry. | Broker-enforced token expiry and revocation. | TTL defined by emergency policy, commonly minutes not hours. | Expiry timer or incident commander revocation. |
| **Least Privilege** | Scope credential to resource, action class, and incident purpose. | Policy engine and credential broker. | No broad standing administrative session by default. | Request exceeds emergency scope. |
| **Post-Incident Reconciliation** | Mandatory review of actions, side effects, and residual access. | Audit queue with security/compliance signoff. | Review completed within policy-defined incident window. | Session closure or incident resolution. |

Break-glass execution should use the strongest isolation practical for the operational environment: a monitored bastion, privileged access gateway, isolated administrative console, hardened container, or microVM. The key properties are scoped credentials, short TTL, recording, immediate alerting, revocation, and post-incident reconciliation. No design should claim “zero persistent credentials” unless the credential lifecycle, logs, caches, shells, and downstream systems have all been verified against that claim.

## **Cryptographic Audit Trails and Distributed Ledger Consistency**

To meet compliance-grade standards in highly regulated environments, the system must generate audit trails that are inherently tamper-evident and resistant to administrator-level manipulation.44  
The active-governance engine can implement tamper-evident audit records using an append-only event log and cryptographic hash chain. Each event should contain the decision-relevant artifacts needed to reconstruct what happened: user intent summary, policy checks, SARC validation results, tool calls, approval records, evidence references, payload hashes, reviewer decisions, and post-action verification state.

The audit log should not store private chain-of-thought, raw hidden reasoning, raw credentials, or unnecessary personal data. If a model produces a user-visible rationale or structured decision artifact, that artifact may be logged. Hidden reasoning should not be treated as an audit primitive.

```text
TAMPER-EVIDENT AUDIT HASH CHAIN

Event N-1
  payload_hash: H(payload_N-1)
  previous_hash: H(event_N-2)
  event_hash: H(payload_hash || previous_hash || metadata)

        |
        v

Event N
  payload_hash: H(payload_N)
  previous_hash: H(event_N-1)
  event_hash: H(payload_hash || previous_hash || metadata)
```

The cryptographic coupling of events is defined as:

```text
event_hash_n = SHA256(payload_hash_n || event_hash_(n-1) || metadata_n)
```

Because each event incorporates the hash of its predecessor, later alteration of an approval amount, timestamp, payload hash, or reviewer decision breaks the chain. Hash chains are tamper-evident, not magically tamper-proof; high-assurance systems should protect signing keys, restrict audit-write permissions, replicate logs, and optionally anchor checkpoints externally.44  

To verify the legal identity and intent of both makers and checkers, approval records are bound to digital signatures utilizing RS256 with X.509 certificates. When a checker approves a pending transaction:

1. The client browser hashes the standardized JSONB approval block.  
2. The reviewer authorizes their local HSM or secure browser token to sign the hash using their private key.  
3. The signature is appended to the reviewed_requests log alongside the public certificate.  
4. The API gateway verifies the certificate chain against the trusted Certification Authority (CA) root, proving that a specific, authorized individual executed the transaction.1

To reduce synchronization anomalies across operational records, vector indexes, caches, and audit logs, high-assurance systems should define an explicit consistency model. Some state must be strongly consistent: approvals, execution ledgers, identity, authorization, and source-of-record mutations. Other state may be derived or eventually consistent: embeddings, semantic caches, search indexes, and reviewer convenience views.

```text
HIGH-ASSURANCE GOVERNANCE DATA LAYER

[ Source-of-Record Database ]
  approvals
  action ledger
  identity / tenant scope
  policy version
  transaction state
        |
        +--> append-only audit log
        |      tamper-evident event chain
        |
        +--> derived retrieval/index layer
        |      embeddings, vector index, search cache
        |
        +--> semantic / response cache
        |      scoped by tenant, user, policy, source version
        |
        +--> reviewer workspace
               redacted package views and evidence links
```

The source-of-record database should own the authoritative state transitions. Vector indexes and caches should be treated as derived artifacts that can be rebuilt from canonical records. A failure in an index, cache, or reviewer view should not corrupt the approval ledger or action state. When a high-impact action spans multiple systems, the platform should use sagas, idempotency keys, post-action verification, compensation procedures, and explicit unknown-state handling rather than assuming every component can participate in a single atomic transaction.1

## **Cross-Canon Handoff Map**

High-impact workflow design provides the governance, review, approval, escalation, break-glass, and audit patterns used when autonomous execution becomes risky. It connects deeply to action verification, tool contracts, UX resilience, boundary defense, telemetry, audit, and incident response.

| Target Report ID | Target Domain | Handoff | Integration Rule |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context and State Governance | Continuity state, approval context, memory eligibility. | High-impact approvals must preserve active constraints and scoped state. |
| **AI-ENG-D** | Corpus Engineering | Source authority and evidence provenance. | Reviewer evidence must retain source lineage and authority metadata. |
| **AI-ENG-E** | Retrieval Pipeline | Evidence retrieval and citation packets. | Review packages should include authorized evidence, not raw unscoped corpus dumps. |
| **AI-ENG-F** | Freshness and Conflict Detection | Currentness and conflict status. | Human reviewers must see stale/conflicting evidence flags. |
| **AI-ENG-M** | Agentic Orchestration | Loop halt, escalation router, autonomy boundaries. | Agents must pause when SARC constraints trigger review. |
| **AI-ENG-N** | Tool Contracts | Tool schema, idempotency, scoped credentials. | High-impact tool actions require pre-action gates and post-action verification. |
| **AI-ENG-O** | Action Verification | Unknown state, reconciliation, compensation. | Review workflows must not flatten unknown action state into success. |
| **AI-ENG-P** | Multimodal Understanding | Visual/document evidence packages. | Document or image evidence must be coordinate/source grounded where relevant. |
| **AI-ENG-Q** | Voice Interaction | Voice confirmation and fallback. | High-impact voice actions require reliable confirmation or alternate channel. |
| **AI-ENG-R** | UI Agents | UI handoff and automation pause. | High-impact UI automation must stop on uncertainty and transfer state. |
| **AI-ENG-S** | Production Pathologies | False success, malformed output, runaway repair. | Human oversight must catch system failure modes before action commit. |
| **AI-ENG-T** | Boundary Defense | Tenant isolation, prompt injection, data leakage. | Review packages must preserve boundary labels and avoid leaking secrets. |
| **AI-ENG-U** | Supply Chain Security | Tool/server/parser trust. | Human review cannot approve actions through untrusted execution substrates. |
| **AI-ENG-V** | Resource Abuse | Review queue capacity and escalation budgets. | Human review is a scarce resource with admission control and SLOs. |
| **AI-ENG-W** | UX Resilience | Degraded mode, partial answer, graceful error. | High-impact workflows need user-visible pending, blocked, review, and unknown states. |
| **AI-ENG-X** | User Trust and Transparency | Trust calibration and contestability. | Users need clear status, evidence, override paths, and right-to-contest flows. |
| **AI-ENG-Z** | Telemetry and Metrics | Review, approval, escalation, and break-glass events. | Every high-impact transition must emit structured, redacted telemetry. |
| **AI-ENG-AA** | Evaluations | HITL, CFF, approval, and escalation tests. | Release gates should test reviewer overreliance and unsafe approval paths. |
| **AI-ENG-AB** | Audit and Replay | Approval ledger, evidence packet, event hash chain. | High-impact decisions must be replayable from durable artifacts. |
| **AI-ENG-AC** | Incident Response | Break-glass and emergency review. | Emergency access requires containment, revocation, and post-incident review. |
| **AI-ENG-AD** | Governance and Accountability | Policy ownership, authority, review accountability. | Governance defines who may approve, override, contest, and audit high-impact actions. |
| **AI-ENG-AJ** | Reference Architecture | Approval ledger, SARC gates, reviewer queue, break-glass gateway. | Reference systems should include active-governance controls by default. |

## **Durable Principles of High-Impact Workflow Design**

1. **Human-in-the-Loop Is Not a Checkbox**  
   Human oversight must be an active control system with evidence, authority, friction, accountability, and intervention power.

2. **Self-Reported Confidence Cannot Be the Approval Gate**  
   Agents have incentives to appear more capable when autonomy is rewarded. Approval gates need independent validation, not model self-confidence theater.

3. **High-Impact Actions Need Pre-Action Gates**  
   Authorization, tenant scope, risk tier, budget, consent, and approval requirements must be checked before tool execution.

4. **Unknown Action State Must Stay Unknown**  
   Pending, partial, failed, and unreconciled states must remain visible until verified. Conversational smoothing is not governance.

5. **Maker and Checker Must Be Separated**  
   The actor proposing or preparing a high-impact action should not be the same actor approving final execution.

6. **Review Interfaces Must Fight Automation Bias**  
   Review screens should force attention to evidence, assumptions, deltas, and consequences—not merely ask reviewers to admire an AI-generated paragraph and click “Approve.”

7. **Escalation Packages Must Be Minimal and Scoped**  
   Reviewers need enough context to decide, not a raw dump of every prompt, secret, trace, and unrelated document the system has ever touched.

8. **Human Review Has Capacity Limits**  
   Escalation queues require SLOs, staffing models, triage, rework tracking, and saturation behavior. A review queue can fail just like a database.

9. **Break-Glass Is Emergency-Scoped, Not Approval-Free**  
   Emergency access must be preauthorized, short-lived, monitored, recorded, and reconciled after the incident.

10. **Audit Trails Should Be Tamper-Evident and Privacy-Aware**  
   Logs should preserve decision artifacts, evidence, policy checks, approvals, and tool traces—not raw hidden reasoning or unnecessary personal data.

11. **Derived Systems Are Not Sources of Record**  
   Vector indexes, caches, and reviewer views may support decisions, but approval ledgers and action state must remain tied to authoritative records.

12. **Governance Must Compile into Runtime Controls**  
   Policies matter only when they become enforceable gates, monitors, ledgers, review queues, revocation paths, and replayable evidence.

#### **Works cited**

1. AI-ENG-X — Human-System Interface: Trust Calibration, Provenance & Control  
2. AI-ENG-W - UX Resilience - Model Routing, Fallback Chains & Degraded-Mode Grace  
3. Why 'human in the loop' falls short – and what to do about it - SiliconANGLE, accessed June 11, 2026, [https://siliconangle.com/2026/05/31/human-loop-falls-short/](https://siliconangle.com/2026/05/31/human-loop-falls-short/)  
4. Governing AI That Acts, Part 2: Control in Name Only | Jones Walker LLP, accessed June 11, 2026, [https://www.joneswalker.com/en/insights/blogs/ai-law-blog/governing-ai-that-acts-part-2-control-in-name-only.html?id=102molc](https://www.joneswalker.com/en/insights/blogs/ai-law-blog/governing-ai-that-acts-part-2-control-in-name-only.html?id=102molc)  
5. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
6. Stuck on Suggestions: Automation Bias, the Anchoring Effect, and the Factors That Shape Them in Computational Pathology - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2603.11821v2](https://arxiv.org/html/2603.11821v2)  
7. Shisa Kanko: Cut 85% of AI & HR Automation Errors - Pyou, accessed June 11, 2026, [https://pyou.eu/shisa-kanko-cut-85-of-ai-hr-automation-errors/](https://pyou.eu/shisa-kanko-cut-85-of-ai-hr-automation-errors/)  
8. Automation Bias - The Decision Lab, accessed June 11, 2026, [https://thedecisionlab.com/biases/automation-bias](https://thedecisionlab.com/biases/automation-bias)  
9. Artificial Intelligence Risks: Automation Bias - MedPro Group, accessed June 11, 2026, [https://resource.medpro.com/artificial-intelligence-risks-automation-bias](https://resource.medpro.com/artificial-intelligence-risks-automation-bias)  
10. Stuck on Suggestions: Automation Bias, the Anchoring Effect, and the Factors That Shape Them in Computational Pathology - Melba Journal, accessed June 11, 2026, [https://www.melba-journal.org/pdf/2026:007.pdf](https://www.melba-journal.org/pdf/2026:007.pdf)  
11. MAKING: AN EMERGING OPERATIONAL RISK AUTOMATION BIAS IN MILITARY DECISION - CENJOWS, accessed June 11, 2026, [https://cenjows.in/wp-content/uploads/2026/05/Mr-Shaws-IB.pdf](https://cenjows.in/wp-content/uploads/2026/05/Mr-Shaws-IB.pdf)  
12. The flaws of policies requiring human oversight of government algorithms - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/360412049_The_flaws_of_policies_requiring_human_oversight_of_government_algorithms](https://www.researchgate.net/publication/360412049_The_flaws_of_policies_requiring_human_oversight_of_government_algorithms)  
13. Escaping the “Interpreter's Trap”: When Explainable AI Fails to Protect Justice, accessed June 11, 2026, [https://communities.springernature.com/posts/escaping-the-interpreter-s-trap-when-explainable-ai-fails-to-protect-justice](https://communities.springernature.com/posts/escaping-the-interpreter-s-trap-when-explainable-ai-fails-to-protect-justice)  
14. An Experimental Comparison of Cognitive Forcing Functions for Execution Plans in AI-Assisted Writing: Effects On Trust, Overreliance, and Perceived Critical Thinking - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2601.18033v1](https://arxiv.org/html/2601.18033v1)  
15. Why I Don't Trust AI To Make Autonomous Access Decisions – Yet - ISC2, accessed June 11, 2026, [https://www.isc2.org/Insights/2026/05/why-i-dont-trust-ai-yet](https://www.isc2.org/Insights/2026/05/why-i-dont-trust-ai-yet)  
16. To Trust or to Think: Cognitive Forcing Functions Can Reduce Overreliance on AI in AI-assisted Decision-making - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/351120800_To_Trust_or_to_Think_Cognitive_Forcing_Functions_Can_Reduce_Overreliance_on_AI_in_AI-assisted_Decision-making](https://www.researchgate.net/publication/351120800_To_Trust_or_to_Think_Cognitive_Forcing_Functions_Can_Reduce_Overreliance_on_AI_in_AI-assisted_Decision-making)  
17. Building a Service Operations Organization: A Strategic Guide for IT Leaders - ServiceNow, accessed June 11, 2026, [https://www.servicenow.com/community/itsm-articles/building-a-service-operations-organization-a-strategic-guide-for/ta-p/3510594](https://www.servicenow.com/community/itsm-articles/building-a-service-operations-organization-a-strategic-guide-for/ta-p/3510594)  
18. SRE Observability: Pillars, Signals & Workflow Explained | Motadata, accessed June 11, 2026, [https://www.motadata.com/blog/site-reliability-engineering-with-observability](https://www.motadata.com/blog/site-reliability-engineering-with-observability)  
19. Human-in-the-Loop Is Not Enough | AI Oversight in Healthcare - LBMC, accessed June 11, 2026, [https://www.lbmc.com/blog/human-in-the-loop-ai-oversight/](https://www.lbmc.com/blog/human-in-the-loop-ai-oversight/)  
20. AI regulatory compliance in 2026: are your controls ready?, accessed June 11, 2026, [https://nhimg.org/community/nhi-support-guidance-forum/ai-regulatory-compliance-in-2026-are-your-controls-ready/](https://nhimg.org/community/nhi-support-guidance-forum/ai-regulatory-compliance-in-2026-are-your-controls-ready/)  
21. Responsible and ethical AI governance: compliance to human flourishing, accessed June 11, 2026, [https://www.simmons-simmons.com/en/publications/cmoac35gv010gukkk6p9paylx/responsible-and-ethical-ai-governance-compliance-to-human-flourishing](https://www.simmons-simmons.com/en/publications/cmoac35gv010gukkk6p9paylx/responsible-and-ethical-ai-governance-compliance-to-human-flourishing)  
22. Article 14: Human Oversight | EU Artificial Intelligence Act, accessed June 11, 2026, [https://artificialintelligenceact.eu/article/14/](https://artificialintelligenceact.eu/article/14/)  
23. Article 14: Human Oversight | AI Act made searchable by Algolia. Chapters, articles and recitals easily readable, accessed June 11, 2026, [https://aiact.algolia.com/article-14/](https://aiact.algolia.com/article-14/)  
24. When “Human-in-the-Loop” Is Just a Checkbox: An Operational Path to Meaningful AI Oversight | by Chris Fong | Apr, 2026 | Medium, accessed June 11, 2026, [https://medium.com/@chrisfongkh/when-human-in-the-loop-is-just-a-checkbox-an-operational-path-to-meaningful-ai-oversight-f437e764d712](https://medium.com/@chrisfongkh/when-human-in-the-loop-is-just-a-checkbox-an-operational-path-to-meaningful-ai-oversight-f437e764d712)  
25. ISO 42001 vs NIST AI RMF: Choosing the Right Framework for Enterprise AI Controls, accessed June 11, 2026, [https://elevateconsult.com/insights/iso-42001-vs-nist-ai-rmf-choosing-the-right-framework-for-enterprise-ai-controls/](https://elevateconsult.com/insights/iso-42001-vs-nist-ai-rmf-choosing-the-right-framework-for-enterprise-ai-controls/)  
26. AI Ethics and Governance Consulting: Responsible AI Implementation - NMS Consulting, accessed June 11, 2026, [https://nmsconsulting.com/ai-ethics-and-governance-consulting/](https://nmsconsulting.com/ai-ethics-and-governance-consulting/)  
27. The Behavioral Credibility Trilemma: When Calibrated Autonomy Becomes Impossible, accessed June 11, 2026, [https://arxiv.org/html/2605.25739v1](https://arxiv.org/html/2605.25739v1)  
28. The Behavioral Credibility Trilemma: When Calibrated Autonomy Becomes Impossible - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2605.25739](https://arxiv.org/pdf/2605.25739)  
29. [2605.25739] The Behavioral Credibility Trilemma: When Calibrated Autonomy Becomes Impossible - arXiv, accessed June 11, 2026, [https://arxiv.org/abs/2605.25739](https://arxiv.org/abs/2605.25739)  
30. Human-AI Escalation Patterns in Production - WFM Labs, accessed June 11, 2026, [https://wiki.wfmlabs.org/wiki/Human-AI_Escalation_Patterns_in_Production](https://wiki.wfmlabs.org/wiki/Human-AI_Escalation_Patterns_in_Production)  
31. How to Build a Human Approval Layer for AI Workflows | Red Brick Labs Blog, accessed June 11, 2026, [https://www.redbricklabs.io/blog/how-to-build-a-human-approval-layer-for-ai-workflows.html](https://www.redbricklabs.io/blog/how-to-build-a-human-approval-layer-for-ai-workflows.html)  
32. Two constructive resolution pathways (commitment, separation), each... | Download Scientific Diagram - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/figure/Two-constructive-resolution-pathways-commitment-separation-each-restoring-two-of-the_tbl2_405263805](https://www.researchgate.net/figure/Two-constructive-resolution-pathways-commitment-separation-each-restoring-two-of-the_tbl2_405263805)  
33. SARC: A Governance-by-Architecture Framework for Agentic AI Systems - arXiv, accessed June 11, 2026, [https://arxiv.org/pdf/2605.07728](https://arxiv.org/pdf/2605.07728)  
34. [2605.07728] SARC: A Governance-by-Architecture Framework for Agentic AI Systems, accessed June 11, 2026, [https://arxiv.org/abs/2605.07728](https://arxiv.org/abs/2605.07728)  
35. SARC: A Governance-by-Architecture Framework for Agentic AI Systems Compiling Regulatory Obligations into Runtime Constraints - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2605.07728v1](https://arxiv.org/html/2605.07728v1)  
36. Example of instrumentation of the Java source code with AspectJ. - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/figure/Example-of-instrumentation-of-the-Java-source-code-with-AspectJ_fig2_323082969](https://www.researchgate.net/figure/Example-of-instrumentation-of-the-Java-source-code-with-AspectJ_fig2_323082969)  
37. Maker-checker - Grokipedia, accessed June 11, 2026, [https://grokipedia.com/page/Maker-checker](https://grokipedia.com/page/Maker-checker)  
38. What is Maker-Checker Control? Definition, Process & Key Metrics - Hyperbots, accessed June 11, 2026, [https://www.hyperbots.com/glossary/maker-checker-control](https://www.hyperbots.com/glossary/maker-checker-control)  
39. The Collaboration Principle: Why AI Agents in Regulated Industries Shouldn't Act Alone, accessed June 11, 2026, [https://medium.com/@dschauer12/the-collaboration-principle-why-ai-agents-in-regulated-industries-shouldnt-act-alone-41e14b188006](https://medium.com/@dschauer12/the-collaboration-principle-why-ai-agents-in-regulated-industries-shouldnt-act-alone-41e14b188006)  
40. Maker-Checker System for Secure Banking Transactions | by Crafting-Code | Stackademic, accessed June 11, 2026, [https://blog.stackademic.com/asp-nets-maker-checker-system-for-secure-banking-transactions-53548bfbffde](https://blog.stackademic.com/asp-nets-maker-checker-system-for-secure-banking-transactions-53548bfbffde)  
41. MakerMaker-Checker implementation guide for secure FinTech systems, accessed June 11, 2026, [https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029](https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029)  
42. Beyond RAG: Using YugabyteDB as the Foundation for Reliable AI Decisions | Yugabyte, accessed June 11, 2026, [https://www.yugabyte.com/blog/using-yugabytedb-for-reliable-ai-decisions/](https://www.yugabyte.com/blog/using-yugabytedb-for-reliable-ai-decisions/)  
43. Maker-Checker Pattern: Dual-Control System Implementation - Opcito, accessed June 11, 2026, [https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems](https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems)  
44. Immutable Payment Audit Trails: Storage, Discovery, and Audits - Chequedb, accessed June 11, 2026, [https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires](https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires)  
45. How Maker-Checker Approvals Work in Mobile Device Management - NinjaOne, accessed June 11, 2026, [https://www.ninjaone.com/blog/how-maker-checker-approvals-work-in-mdm/](https://www.ninjaone.com/blog/how-maker-checker-approvals-work-in-mdm/)  
46. An Experimental Comparison of Cognitive Forcing Functions for Execution Plans in AI-Assisted Writing: Effects On Trust, Overreliance, and Perceived Critical Thinking - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/400084360_An_Experimental_Comparison_of_Cognitive_Forcing_Functions_for_Execution_Plans_in_AI-Assisted_Writing_Effects_On_Trust_Overreliance_and_Perceived_Critical_Thinking](https://www.researchgate.net/publication/400084360_An_Experimental_Comparison_of_Cognitive_Forcing_Functions_for_Execution_Plans_in_AI-Assisted_Writing_Effects_On_Trust_Overreliance_and_Perceived_Critical_Thinking)  
47. Cognitive Forcing Functions: Enhancing AI Decisions - Emergent Mind, accessed June 11, 2026, [https://www.emergentmind.com/topics/cognitive-forcing-functions-cffs](https://www.emergentmind.com/topics/cognitive-forcing-functions-cffs)  
48. An Experimental Comparison of Cognitive Forcing Functions for Execution Plans in... (AI Podcast) - YouTube, accessed June 11, 2026, [https://www.youtube.com/watch?v=PD0hCqeTcJA](https://www.youtube.com/watch?v=PD0hCqeTcJA)  
49. Emerging Reliance Behaviors in Human-AI Content Grounded Data Generation: The Role of Cognitive Forcing Functions and Hallucinations - arXiv, accessed June 11, 2026, [https://arxiv.org/html/2409.08937v2](https://arxiv.org/html/2409.08937v2)  
50. Impacts of cognitive forcing and need for cognition on biased AI-assisted decision making about mental health emergencies - PMC, accessed June 11, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12779943/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12779943/)  
51. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
52. Performance Modeling and Design of Computer Systems: Queueing Theory in Action, accessed June 11, 2026, [https://www.researchgate.net/publication/364930160_Performance_Modeling_and_Design_of_Computer_Systems_Queueing_Theory_in_Action](https://www.researchgate.net/publication/364930160_Performance_Modeling_and_Design_of_Computer_Systems_Queueing_Theory_in_Action)  
53. Erlang-R: A Time-Varying Queue with Reentrant Customers, in Support of Healthcare Staffing | Request PDF - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/270635595_Erlang-R_A_Time-Varying_Queue_with_Reentrant_Customers_in_Support_of_Healthcare_Staffing](https://www.researchgate.net/publication/270635595_Erlang-R_A_Time-Varying_Queue_with_Reentrant_Customers_in_Support_of_Healthcare_Staffing)  
54. belizar/animus - GitHub, accessed June 11, 2026, [https://github.com/belizar/animus](https://github.com/belizar/animus)  
55. AI vs Human Cost Efficiency in Call Centers | PDF - Scribd, accessed June 11, 2026, [https://www.scribd.com/document/903682546/ChatGPT-AI-vs-Human-Cost-LLM-Orchestrator](https://www.scribd.com/document/903682546/ChatGPT-AI-vs-Human-Cost-LLM-Orchestrator)  
56. Safe Operating Throughput (SOT) as a First-Class SRE Metric: Derivation and Operationalization - DEV Community, accessed June 11, 2026, [https://dev.to/npayyappilly/safe-operating-throughput-sot-as-a-first-class-sre-metric-derivation-and-operationalization-5akn](https://dev.to/npayyappilly/safe-operating-throughput-sot-as-a-first-class-sre-metric-derivation-and-operationalization-5akn)  
57. Maths for Cloud Jobs: The Only Topics You Actually Need (& How to Learn Them), accessed June 11, 2026, [https://cloudcomputingjobs.co.uk/career-advice/maths-for-cloud-jobs-topics-you-need](https://cloudcomputingjobs.co.uk/career-advice/maths-for-cloud-jobs-topics-you-need)  
58. When to Use Speedup: An Examination of Service Systems with Returns - ResearchGate, accessed June 11, 2026, [https://www.researchgate.net/publication/266595665_When_to_Use_Speedup_An_Examination_of_Service_Systems_with_Returns](https://www.researchgate.net/publication/266595665_When_to_Use_Speedup_An_Examination_of_Service_Systems_with_Returns)  
59. Implementing JIT Access in AWS, Azure, GCP: A detailed Guide - Cloudanix, accessed June 11, 2026, [https://www.cloudanix.com/use-cases/implementing-jit-access-in-aws-azure-gcp-complete-guide-for-secruity-leaders](https://www.cloudanix.com/use-cases/implementing-jit-access-in-aws-azure-gcp-complete-guide-for-secruity-leaders)  
60. Break Glass Procedure - Privileged Access Management (PAM) Help, accessed June 11, 2026, [https://help.xtontech.com/content/getting-started/architecture/break-glass-procedure.htm](https://help.xtontech.com/content/getting-started/architecture/break-glass-procedure.htm)  
61. Hoop Access Gateway: Secure, Auditable, and Controlled Infrastructure Access - OpsTree, accessed June 11, 2026, [https://opstree.com/blog/hoop-access-gateway/](https://opstree.com/blog/hoop-access-gateway/)  
62. Glasskiss is an Ephemeral Break-Glass Access Controller. It turns 'Permanent Production Access' (a huge security risk) into 'Just-in-Time Scoped Access' using Motia's durable primitives. · GitHub, accessed June 11, 2026, [https://github.com/soumyacodes007/GlassKiss](https://github.com/soumyacodes007/GlassKiss)  
63. AmudFin - Financial Intelligence Platform, accessed June 11, 2026, [https://amudfin.com/](https://amudfin.com/)

---

[← Back to Canon Map](../canon-map.md)