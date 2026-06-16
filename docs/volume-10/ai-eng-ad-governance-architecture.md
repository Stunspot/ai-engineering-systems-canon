# AI-ENG-AD — Governance Architecture - Policy, Audit, Compliance & Accountability

## **Doctrinal Foundations: The Executable AI Operating System**

In high-dimensional artificial intelligence systems, production reliability cannot be governed by traditional application performance monitoring (APM) paradigms or static, post-hoc compliance checklists.1 Traditional web software assumes a deterministic relationship between code, infrastructure state, and output.1 In contrast, architectures driven by probabilistic large language models (LLMs) and multi-agent loops introduce non-deterministic, behavioral failures where backend services return successful HTTP 200 statuses while simultaneously generating factually false advice, leaking cross-tenant data, fabricating API calls, or bypassing security boundaries.1 This divergence establishes the "Green Dashboard Fallacy": the erroneous and high-risk assumption that an AI system is operating correctly simply because its servers, databases, and APIs are online and responsive.1  
To bridge this visibility and control deficit, modern organizations must deploy a structured **Governance Architecture** designed as the practical operating system for organizational AI control.1 This architecture enforces a rigorous, system-integrated governing doctrine: **AI governance must be lightweight where risk is low, strict where consequences are high, and executable everywhere**.5 Policies, risk classifications, approvals, audit trails, and data rules must be attached directly to production environments, active codebases, and live database transactions—never left to float serenely above production as abstract, unexecutable ethical principles.1 The fundamental metric of organizational safety is not the presence of a "Responsible AI" PDF, but the programmatic capability to identify, isolate, audit, and prove the compliance of every model, tool, dataset, and state mutation in real time.1

## **Unified Frameworks: NIST AI RMF 1.0 and ISO/IEC 42001 Integration**

A high-assurance governance system must map its internal technical controls directly to established international standards.7 This synchronization is achieved by unifying the process-oriented lifecycle of the **NIST AI Risk Management Framework (AI RMF 1.0)** with the formal, auditable requirements of the **ISO/IEC 42001 Artificial Intelligence Management System (AIMS)**.9

### **The NIST AI RMF Core Functions**

The NIST AI RMF Core divides risk management into four continuous, non-linear functions 11:

* **Govern:** Instills a culture of risk awareness, establishes legal and regulatory registries, allocates resources, and defines organizational accountability.7 It functions as a cross-cutting framework that structures and authorizes the other three functions.11  
* **Map:** Captures the situational context of each AI system, documenting its intended purpose, potential benefits, operational boundaries, and context-specific limitations.7  
* **Measure:** Employs quantitative and qualitative evaluations (such as tracking accuracy, measuring bias, and auditing citations) to benchmark risks and track behavioral regressions over time.7  
* **Manage:** Allocates risk-treatment resources to mapped and measured exposures, enabling automated recovery, incident response, or system deactivation when anomalies exceed thresholds.7

```text
NIST AI RMF + ISO/IEC 42001 GOVERNANCE LOOP

+--------------------------------------------------------------------+
| GOVERN                                                             |
| culture | policy | accountability | legal/regulatory register      |
| ISO/IEC 42001 alignment: AIMS scope, roles, policy, objectives     |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MAP                                                                |
| use case | context | affected parties | benefits | limits | risks  |
| ISO/IEC 42001 alignment: AI system inventory, impact assessment    |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MEASURE                                                            |
| TEVV | bias/fairness | robustness | drift | security | evidence    |
| ISO/IEC 42001 alignment: evaluation, monitoring, audit evidence    |
+-----------------------------+--------------------------------------+
                              |
                              v
+--------------------------------------------------------------------+
| MANAGE                                                             |
| risk treatment | runtime controls | incident response | exceptions |
| ISO/IEC 42001 alignment: operating controls and improvement loop   |
+-----------------------------+--------------------------------------+
                              |
                              v
                         back to GOVERN

Rule:
  Governance is not a document layer above production.
  It is a decision-and-control loop attached to systems, evidence,
  owners, release gates, and runtime enforcement points.
```

### **ISO/IEC 42001 (AIMS) and the Statement of Applicability**

While NIST outlines the necessary operational process, ISO/IEC 42001 translates these activities into a set of 38 normative controls organized across nine Annex A control domains, which must be documented within a formal **Statement of Applicability (SoA)**.8 The SoA acts as the primary contract between the organization's technical practices and compliance auditors, documenting the explicit rationale for including or excluding specific controls, detail-mapping their implementation status, and providing pointers to active software verifications.8

### **Control Mapping and Suggested Audit Artifacts**

To satisfy NIST AI RMF and ISO/IEC 42001 together, organizations should map governance functions to implementation controls and audit evidence. The purpose of this map is not to prove compliance by collecting more paper. It is to connect obligations to owners, systems, runtime controls, and verifiable evidence.

| NIST AI RMF Function | Governance Objective | ISO/IEC 42001 Alignment | Preferred Audit Evidence | Runtime / Engineering Evidence |
| :---- | :---- | :---- | :---- | :---- |
| **Govern** | Establish accountability, risk appetite, policy, and oversight. | AIMS scope, AI policy, roles, responsibilities, management review. | AI governance policy, RACI, risk-acceptance register, system owner roster. | Policy bundle versions, approval records, release-gate decisions, exception records. |
| **Map** | Identify the system, use case, stakeholders, context, intended purpose, and foreseeable misuse. | AI system inventory, impact assessment, resource and lifecycle documentation. | AI system inventory, impact assessment, data-flow map, model/tool/data inventory. | Context manifests, corpus lineage records, tool manifests, model route manifests. |
| **Measure** | Evaluate technical and socio-technical risk. | Verification, validation, testing, monitoring, data quality, performance evaluation. | TEVV plans, evaluation results, bias/fairness assessments, robustness tests. | Golden-set results, canary traces, drift signals, citation/evidence verification metrics. |
| **Manage** | Treat, monitor, accept, transfer, or reduce risk. | Operating controls, supplier controls, incident response, continual improvement. | Risk treatment plan, incident records, vendor assessments, control review reports. | Gateway policy decisions, OPA/Rego logs, kill-switch records, runbooks, rollback traces. |
| **Monitor / Improve** | Reassess risk after deployment and incidents. | Internal audit, management review, corrective actions, lifecycle governance. | Post-incident reviews, audit findings, corrective action plans. | Incident-to-eval promotions, telemetry alerts, new regression cases, policy diffs. |

Audit evidence should be proportional to risk. A low-risk internal summarizer does not need the same evidence package as a high-impact credit, medical, employment, financial, safety, or regulated decision-support system.

## **Policy-as-Code (PaC) and Centralized API Gateways**

Relying on system prompts or inline instructions to enforce system rules introduces severe security vulnerabilities.1 When language models are confronted with long contexts, conversational histories, or adversarial inputs, their attention is diluted, causing them to bypass prompt-level restrictions.1 This failure mode makes "Compliance at the Point of Change" (CAPOC) a non-negotiable architectural paradigm: **compliance rules must be written as declarative, machine-readable code, managed within version-controlled repositories, and programmatically evaluated before any build is merged or action executed**.19

### **Decoupling Policy from Application Code via Gateways**

By routing all model transactions through a centralized, budget-aware **AI Agent Gateway**, organizations decouple authorization logic from application code, preventing developers from bypassing compliance gates.6 The gateway functions as an application-layer firewall that intercepts intent, validates input schemas, checks user credentials, and externalizes access decisions to **Open Policy Agent (OPA)** before sending the query to upstream providers.18

```text
POLICY-AS-CODE AI GATEWAY FLOW

[ User / Client / Agent Request ]
        |
        v
+-------------------------------+
| AI Agent Gateway              |
| - authenticate subject        |
| - bind tenant/session scope   |
| - normalize intent/tool call  |
| - attach model/tool metadata  |
| - estimate budget/risk        |
+---------------+---------------+
                |
                v
+-------------------------------+
| Policy Decision Point         |
| OPA / policy engine           |
| - RBAC / ABAC                 |
| - tenant boundary             |
| - tool authority              |
| - budget ceiling              |
| - approval requirement        |
| - artifact allowlist          |
+---------------+---------------+
        | allow                         | deny / review / degrade
        v                               v
+-------------------------------+   +-------------------------------+
| Route to Serving / Tool Layer |   | Block / Escalate / Log        |
| - approved model route        |   | - reason code                 |
| - scoped credential           |   | - audit event                 |
| - bounded execution           |   | - user-safe status            |
+-------------------------------+   +-------------------------------+
```

The gateway is the policy enforcement point. The model may propose an action, but the gateway decides whether the action is allowed, requires review, must degrade, or must fail closed.

To streamline this workflow, modern setups integrate tools like **AICertify** and **Gopal**.22 AICertify automatically collects and generates the required input context (such as model versions, token metrics, and security hashes), while Gopal provides the domain-specific Rego policies that OPA evaluates.22 This integration ensures that AI-specific metrics can be audited using the same Policy-as-Code infrastructure that governs traditional cloud deployments.22

### **The Validation Gap: Testing Against Real Artifacts**

A persistent bottleneck in Policy-as-Code adoption is the "validation gap"—where security teams write OPA policies that check syntax correctly but crash production environments because they cannot test the rules against real software artifacts before they ship.23 To resolve this, platform engineering teams deploy the **JFrog AppTrust PaC Playground**.23  
This playground allows AppSec leads to load actual binaries and Software Bills of Materials (SBOMs) from their system of record and run dry-runs of newly drafted Rego policies against historical artifacts.23 This prevents policies from blocking legitimate releases due to missing context variables, turning continuous governance from a development blocker into a secure business enabler.23

### **Production-Grade Rego Policy for AI Gateway Authorization**

The following Rego policy illustrates how an AI gateway can enforce tenant scope, RBAC, budget ceilings, approval requirements, and artifact integrity before a model-generated tool action is dispatched.

The policy assumes that trusted registry data—such as approved plan hashes, tool manifests, and environment rules—is supplied to OPA as a signed data bundle or trusted sidecar input. OPA should not be treated as casually querying arbitrary production databases during local policy evaluation.

```rego
package ai.gateway.authorization

default allow := false
default decision := {
  "allow": false,
  "reason": "unauthorized_gateway_access",
  "review_required": false,
}

# Expected input shape:
#
# input.subject = {
#   "id": "opaque_user_or_service_id",
#   "tenant_id": "tenant_123",
#   "roles": ["deploy_bot"],
#   "suspended": false,
#   "remaining_session_budget_usd": 12.50
# }
#
# input.tool_call = {
#   "name": "apply_infrastructure",
#   "params": {
#     "tenant_id": "tenant_123",
#     "environment": "staging",
#     "plan_name": "service-update.plan",
#     "plan_hash": "sha256:abc..."
#   },
#   "risk_class": "high_impact_write"
# }
#
# input.token_metrics = {
#   "input_tokens": 2500,
#   "max_output_tokens": 800
# }
#
# data.registry.valid_plan_hashes = {
#   "sha256:abc...": true
# }

allow {
  not subject_suspended
  valid_tenant_scope
  rbac_permissions_met
  not budget_ceiling_breached
  not blocked_destructive_action
  plan_integrity_verified
  approval_requirement_satisfied
}

decision := {
  "allow": true,
  "reason": "allowed",
  "review_required": false,
} {
  allow
}

decision := {
  "allow": false,
  "reason": reason,
  "review_required": review_required,
} {
  not allow
  reason := deny_reason
  review_required := requires_human_review
}

subject_suspended {
  input.subject.suspended == true
}

valid_tenant_scope {
  input.subject.tenant_id == input.tool_call.params.tenant_id
}

rbac_permissions_met {
  input.tool_call.name == "read_status"
  "viewer" in input.subject.roles
}

rbac_permissions_met {
  input.tool_call.name == "apply_infrastructure"
  "deploy_bot" in input.subject.roles
  input.tool_call.params.environment != "production"
}

rbac_permissions_met {
  input.tool_call.name == "apply_infrastructure"
  "sre_admin" in input.subject.roles
}

estimated_cost_usd := cost {
  input_cost := input.token_metrics.input_tokens * 0.000015
  output_cost := input.token_metrics.max_output_tokens * 0.00006
  cost := input_cost + output_cost
}

budget_ceiling_breached {
  estimated_cost_usd > input.subject.remaining_session_budget_usd
}

blocked_destructive_action {
  input.tool_call.name == "apply_infrastructure"
  endswith(input.tool_call.params.plan_name, "-destroy.plan")
  not "break_glass_admin" in input.subject.roles
}

plan_integrity_verified {
  data.registry.valid_plan_hashes[input.tool_call.params.plan_hash] == true
}

approval_requirement_satisfied {
  input.tool_call.risk_class != "high_impact_write"
}

approval_requirement_satisfied {
  input.tool_call.risk_class == "high_impact_write"
  input.approval.status == "approved"
  input.approval.payload_hash == input.tool_call.payload_hash
  input.approval.approver_id != input.subject.id
}

requires_human_review {
  input.tool_call.risk_class == "high_impact_write"
  not approval_requirement_satisfied
}

deny_reason := "subject_suspended" {
  subject_suspended
}

deny_reason := "tenant_id_mismatch" {
  not valid_tenant_scope
}

deny_reason := "insufficient_rbac_permissions" {
  valid_tenant_scope
  not rbac_permissions_met
}

deny_reason := "session_budget_breached" {
  budget_ceiling_breached
}

deny_reason := "blocked_destructive_plan" {
  blocked_destructive_action
}

deny_reason := "plan_hash_unverified" {
  not plan_integrity_verified
}

deny_reason := "approval_required" {
  requires_human_review
}
```

This policy does not rely on the model to decide whether an action is safe. The model proposes; the gateway evaluates; the policy engine returns an auditable decision.

## **Runtime Governance for Agentic Systems**

Policy-as-code and release gates are necessary, but agentic systems also need runtime governance. A model can generate valid-looking actions that become unsafe only after state changes, streaming outputs, tool latency, partial commits, cost accumulation, or human-review uncertainty. Governance must therefore attach constraints directly to the agent loop.

One useful pattern is **SARC-style runtime governance**, where system state, action space, reward/optimization pressure, and constraints are modeled explicitly. SARC should be treated as a reference pattern, not a single mandatory framework. The durable principle is broader: constraints must be executable, traceable, and enforced at the correct physical point in the workflow.

### **Constraint Specification Model**

A runtime governance constraint can be represented as:

```text
C = <source, class, predicate, verification_point, response>
```

| Field | Meaning | Example |
| :---- | :---- | :---- |
| **source** | The policy, legal, contractual, security, or organizational authority behind the constraint. | EU AI Act Article 14, corporate procurement policy, tenant data policy. |
| **class** | Whether the constraint is hard, soft, reviewable, or advisory. | hard block, human review, throttle, warning. |
| **predicate** | The executable condition evaluated by the system. | amount <= user_limit, tenant_id matches, PII not present. |
| **verification_point** | Where the check runs physically. | pre-action gate, action-time monitor, post-action auditor, escalation router. |
| **response** | What the system does when the predicate fails. | block, redact, hold, throttle, compensate, escalate, fail closed. |

```text
RUNTIME GOVERNANCE LOOP

[ Agent State / User Goal / Active Policy ]
        |
        v
+-------------------------------+
| Pre-Action Gate               |
| checks state + proposed action|
| before tool dispatch          |
+---------------+---------------+
                |
                v
+-------------------------------+
| Action-Time Monitor           |
| watches streaming output,     |
| latency, egress, cost, leaks  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Post-Action Auditor           |
| verifies source-of-record     |
| state, cost, commit, effects  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Escalation Router             |
| freezes workflow and packages |
| review when confidence, risk, |
| or policy requires it         |
+-------------------------------+
```

### **The Four Runtime Enforcement Points**

| Enforcement Point | When It Runs | Best For | Example Response |
| :---- | :---- | :---- | :---- |
| **Pre-Action Gate** | Before a tool or external action is invoked. | Tenant scope, approval status, RBAC/ABAC, payload schema, risk class, budget reservation. | Block, require approval, downgrade to read-only, or route to review. |
| **Action-Time Monitor** | During streaming generation, tool execution, browser automation, or long-running workflow. | Token/cost velocity, data leakage, egress attempts, timeout risk, unsafe intermediate outputs. | Cancel call, redact, cut off stream, revoke temporary credential. |
| **Post-Action Auditor** | After an action returns, before the model or UI claims completion. | Source-of-record verification, partial commit detection, actual cost, consistency with requested action. | Mark verified, hold unknown state, reconcile, compensate, or correct user status. |
| **Escalation Router** | Whenever a hard or reviewable constraint requires human judgment. | High-impact actions, low confidence, policy exceptions, contested outcomes, regulatory review. | Freeze workflow, package evidence, route to human review, preserve state. |

### **Specification-Trace Correspondence**

Runtime governance should provide specification-trace correspondence: for each consequential action in the trace, an auditor should be able to identify which constraints applied, where they were evaluated, what decision they produced, and what response occurred.

This is an auditability property, not a mystical proof spell. The system proves governance by preserving policy versions, decision IDs, trace IDs, state hashes, secure payload references, approval records, and action outcomes.

### **Constraint Enforcement Manifest**

| Constraint ID | Source | Class | Predicate | Enforcement Point | Response | Recovery Semantics |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **C_OVERSIGHT_01** | Human oversight policy / regulated high-impact workflow. | Reviewable hard boundary. | High-impact action has valid approval bound to payload hash. | Pre-Action Gate. | Block and escalate to checker. | No external write occurs; preserve draft. |
| **C_PRIVACY_02** | Privacy-by-design policy / GDPR-aligned internal rule. | Hard boundary. | Payload contains no prohibited sensitive data for the selected route. | Action-Time Monitor. | Abort, redact, and open privacy event if needed. | Clear transient workspace; preserve secure evidence reference. |
| **C_FINOPS_03** | Corporate FinOps policy. | Soft boundary until hard budget limit. | Workflow spend remains within tenant/session budget. | Action-Time Monitor. | Throttle, degrade, require approval, or stop. | Preserve state; resume only after budget decision. |
| **C_INTEGRITY_04** | Financial / operational integrity policy. | Hard boundary. | Post-action state matches expected source-of-record predicate. | Post-Action Auditor. | Hold unknown state or block success claim. | Reconcile, compensate, forward recover, or escalate. |
| **C_LOGGING_05** | Audit and retention policy. | Hard boundary for high-impact actions. | Required trace, policy, and action-ledger metadata were written. | Post-Action Auditor. | Freeze workflow and route to operations review. | Retry audit write if safe; otherwise hold and escalate. |

## **Dual-Control "Maker-Checker" Ledger Architecture**

When AI systems act as the "Makers" of financial or administrative changes—such as proposing loan disbursals, generating payments, or updating security parameters—traditional, single-user database writes create significant risks of fraud, compromise, or error.3 To enforce strict segregation of duties, organizations must implement the dual-authorization **Maker-Checker** pattern (Four-Eyes Principle) at the database engine layer.3  
Under this design, a **Maker** (the automated agent or user) initiates a sensitive operation.37 Instead of executing the action immediately, the system intercepts the request, blocks its execution on business tables, and routes the proposed transaction payload to a centralized **Approval Queue**.3 A **Checker** (a different, authorized human supervisor or auditor) must manually review, sign, and commit the proposed change from the queue.3

```text
MAKER-CHECKER CONTROL FLOW

[ Maker: user or agent proposes action ]
        |
        v
+-------------------------------+
| Action Interceptor            |
| - validates schema            |
| - classifies risk             |
| - blocks direct business write|
+---------------+---------------+
                |
                v
+-------------------------------+
| approval_requests Queue       |
| - payload hash / secure ref   |
| - maker identity              |
| - tenant / policy version     |
| - status = PENDING            |
+---------------+---------------+
                |
                v
[ Checker Review ]
  checker must be authorized
  checker must differ from maker
  approval binds to payload hash
        |
        +--> rejected -> record reason, no business mutation
        |
        v
+-------------------------------+
| Execution Worker              |
| - revalidates approval        |
| - executes atomic transaction |
| - writes immutable ledger     |
| - verifies source of record   |
+---------------+---------------+
                |
                v
[ User-facing confirmation only after verified commit ]
```

### **Queue-Based Database Schema**

Traditional table-level maker-checker implementations often add approval columns to every business table. That approach creates inconsistent review semantics and makes it difficult to audit pending approvals across systems. A cleaner pattern is to centralize approval requests while keeping business writes behind a verified execution worker.

The following PostgreSQL schema separates the approval queue from the immutable execution ledger. It stores sensitive payloads by secure reference and hash rather than dumping raw high-impact transaction details directly into an ordinary workflow table.

```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TYPE approval_workflow_status AS ENUM (
    'PENDING',
    'APPROVED',
    'REJECTED',
    'EXPIRED',
    'PROCESSING',
    'COMPLETED',
    'FAILED'
);

CREATE TYPE approval_risk_class AS ENUM (
    'LOW',
    'MEDIUM',
    'HIGH',
    'REGULATED'
);

CREATE TABLE public.approval_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID NOT NULL,
    operation_type TEXT NOT NULL,
    action_type TEXT NOT NULL,
    risk_class approval_risk_class NOT NULL,

    -- Sensitive payloads should live in controlled storage.
    -- The approval record stores the reference and integrity hash.
    payload_ref TEXT NOT NULL,
    payload_hash TEXT NOT NULL CHECK (payload_hash ~ '^sha256:[a-f0-9]{64}$'),

    policy_version TEXT NOT NULL,
    tool_manifest_hash TEXT,
    idempotency_key TEXT UNIQUE NOT NULL,

    maker_id UUID NOT NULL,
    checker_id UUID,
    status approval_workflow_status NOT NULL DEFAULT 'PENDING',

    approval_reason TEXT,
    rejection_reason TEXT,

    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    expires_at TIMESTAMPTZ NOT NULL,
    reviewed_at TIMESTAMPTZ,
    executed_at TIMESTAMPTZ,

    CONSTRAINT chk_checker_differs_from_maker CHECK (
        checker_id IS NULL OR checker_id <> maker_id
    ),

    CONSTRAINT chk_review_fields_when_approved CHECK (
        status <> 'APPROVED'
        OR (
            checker_id IS NOT NULL
            AND reviewed_at IS NOT NULL
            AND approval_reason IS NOT NULL
        )
    ),

    CONSTRAINT chk_review_fields_when_rejected CHECK (
        status <> 'REJECTED'
        OR (
            checker_id IS NOT NULL
            AND reviewed_at IS NOT NULL
            AND rejection_reason IS NOT NULL
        )
    ),

    CONSTRAINT chk_completed_requires_execution CHECK (
        status <> 'COMPLETED'
        OR executed_at IS NOT NULL
    )
);

CREATE INDEX idx_approval_requests_tenant_status
    ON public.approval_requests (tenant_id, status);

CREATE INDEX idx_approval_requests_operation_status
    ON public.approval_requests (operation_type, status);

CREATE TABLE public.approval_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    approval_request_id UUID NOT NULL
        REFERENCES public.approval_requests(id),

    event_type TEXT NOT NULL,
    actor_id UUID NOT NULL,
    event_payload JSONB NOT NULL DEFAULT '{}'::jsonb,
    event_hash TEXT NOT NULL CHECK (event_hash ~ '^sha256:[a-f0-9]{64}$'),

    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_approval_events_request
    ON public.approval_events (approval_request_id, created_at);

CREATE TABLE public.action_execution_ledger (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    approval_request_id UUID NOT NULL
        REFERENCES public.approval_requests(id),

    tenant_id UUID NOT NULL,
    operation_type TEXT NOT NULL,

    pre_action_state_hash TEXT NOT NULL,
    post_action_state_hash TEXT,
    execution_status TEXT NOT NULL,
    compensation_ref TEXT,

    executed_by UUID NOT NULL,
    executed_at TIMESTAMPTZ NOT NULL DEFAULT now(),

    ledger_entry_hash TEXT NOT NULL CHECK (ledger_entry_hash ~ '^sha256:[a-f0-9]{64}$')
);

CREATE INDEX idx_action_execution_ledger_request
    ON public.action_execution_ledger (approval_request_id);
```

This schema is not a complete banking ledger. It is a governance queue and execution ledger pattern. Financial systems still require domain-specific accounting tables, double-entry posting rules, reconciliation, settlement controls, and regulator-specific audit procedures.

### **Double-Entry Ledger, Immutability, and State Verification**

When the checker approves the request, the database executes the operation within an atomic, double-entry transactional block.39 Double-entry rules require that every financial posting writes at least one debit and one credit in equal amounts, ensuring the ledger remains balanced (Sum of Debits + Sum of Credits = 0) and mathematically preventing single-entry anomalies.39  
Once committed, transaction logs are completely immutable—historical records are never updated or deleted.39 Any corrections must be executed by posting opposing "contra" and "re-booked" entries, leaving a chronological, tamper-evident audit trail for financial regulators.39  

For high-impact operations, approval is not the same as execution. The checker approves a specific payload hash under a specific policy version. The execution worker must then re-read the approval record, verify that the payload hash still matches, confirm the approval has not expired, execute the business transaction atomically, and write an immutable execution-ledger entry. The user interface must not report completion until the post-action verifier confirms the source-of-record state.

To guarantee state verification, the platform leverages configuration parameters across distinct instance types 41:

* **Read Instance:** Uses read-only database connections to fetch users, templates, and transaction histories.41  
* **Write Instance:** Persists state-changing commands to the approval queue, ensuring every update is registered.41  
* **Batch Instance:** Automatically executes background verification, audit synchronization, and log archiving routines.41

This division ensures that the conversational interface remains blocked from speaking or displaying transaction confirmations to the user until the Write Instance confirms that the database commit has successfully settled in the core ledger.2

## **Governance Decision Register and Accountability Matrix**

Governance architecture must define who is allowed to make which AI-system decisions, what evidence they need, where the decision is enforced, and how exceptions are reviewed. Without a decision register, governance collapses into vibes, meetings, and someone named Brad saying “seems fine” in Slack. Brad is not a control.

| Governance Decision | Accountable Owner | Trigger | Required Evidence | Enforcement Surface | Review / Exception Path |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **AI System Registration** | Governance Lead / System Owner. | New AI system, material feature, model route, or agent workflow. | System card, owner, intended use, user population, data classes, model/tool inventory. | AI system inventory gate. | Architecture review for unregistered or orphaned systems. |
| **Risk Classification** | Compliance Lead with System Owner. | New use case or material change in affected population, autonomy, or data class. | Impact assessment, harm analysis, regulatory mapping, human oversight plan. | Release gate and policy bundle. | Risk acceptance by accountable executive for exceptions. |
| **Model Route Approval** | Model Owner / AI Operations Lead. | New model, provider, adapter, quantization profile, or fallback route. | Evaluation results, capability profile, data-processing terms, supply-chain verification. | Gateway route manifest. | Route exception expires and requires revalidation. |
| **Tool Permission Approval** | Tool Owner / Security Lead. | New tool, new scope, new mutation class, or expanded credential. | Tool contract, side-effect class, schema, auth model, idempotency plan, verification predicate. | Tool registry and gateway policy. | Human review for high-impact or broad-scope tools. |
| **Human Review Threshold** | Governance Lead / Workflow Owner. | High-impact workflow or repeated uncertainty. | Risk class, confidence thresholds, historical review outcomes, reviewer capacity. | Maker-checker queue and escalation router. | Periodic threshold review; exceptions logged. |
| **Corpus / Data Source Approval** | Data Owner / Retrieval Owner / Privacy Lead. | New corpus, ingestion route, vendor data source, or tenant-shared index. | Provenance, license, sensitivity, retention, data quality, source authority. | Corpus lifecycle state and retrieval eligibility policy. | Quarantine and review for unknown-provenance sources. |
| **Vendor / SaaS Approval** | Procurement, Security, Privacy, Legal. | New AI vendor, embedded-AI feature, subprocessor, or data-sharing route. | Security assessment, DPA, training-use terms, deletion/export rights, audit evidence. | Procurement gate and vendor registry. | Risk acceptance and compensating controls if approved with gaps. |
| **Telemetry Retention Policy** | Privacy Lead / SRE / Governance Lead. | New trace field, payload reference, audit requirement, or incident evidence class. | Data classification, purpose, retention need, access model, deletion obligations. | Telemetry collector and secure payload vault. | Legal/privacy review for extended sensitive retention. |
| **Incident Reporting Decision** | Incident Commander / Compliance / Legal. | SEV-0/SEV-1, privacy event, regulated system incident, or customer impact. | Incident manifest, scope analysis, affected population, legal/regulatory triggers. | Incident management system and notification workflow. | Counsel and executive approval where required. |
| **Decommission / Sunset Decision** | System Owner / Governance Lead. | Vendor exit, model retirement, unacceptable residual risk, or system obsolescence. | Data export plan, deletion certificate, replacement route, archival evidence. | Lifecycle registry and access controls. | Formal exception if system must remain temporarily active. |

The register should be machine-readable where possible. Each decision should bind to policy version, owner, timestamp, evidence references, affected systems, and expiration/review date.

## **Shadow AI Discovery and Context Ingestion Hygiene**

Shadow AI is the use of unapproved AI tools, browser plugins, SaaS products, local agents, or automation scripts outside formal IT, security, privacy, procurement, or governance review. The risk is not that employees are curious. The risk is that sensitive prompts, proprietary code, customer records, regulated data, credentials, or internal documents move into unapproved systems with unknown retention, training, security, and contractual terms.

Shadow AI discovery must itself be governed. Monitoring should be lawful, disclosed where required, proportionate, minimized, access-controlled, and reviewed through privacy and security governance. The goal is to identify risky data flows and route users toward approved tools—not to build an employee panopticon with a compliance sticker slapped on the side.

### **Multi-Layered Shadow AI Discovery Stack**

```text
SHADOW AI DISCOVERY STACK

[ Approved AI Inventory ]
        |
        v
[ Discovery Signals ]
  DNS / SNI / CASB / SaaS logs
  endpoint DLP events
  identity and OAuth grants
  expense/vendor records
  browser extension inventory
  developer package/config scans
        |
        v
[ Risk Correlation ]
  data sensitivity | destination | user role
  volume | frequency | contract status
  vendor approval state
        |
        v
[ Governance Response ]
  allow approved tool
  coach user to safe path
  require vendor review
  block high-risk transfer
  open incident if sensitive data exposed
```

| Discovery Layer | What It Detects | Governance Guardrail |
| :---- | :---- | :---- |
| **Network / CASB Signals** | Connections to unapproved AI services, unusual upload volume, risky SaaS destinations. | Use destination and volume metadata where sufficient; restrict deeper inspection to policy-approved cases. |
| **Endpoint / DLP Signals** | Sensitive data copied, uploaded, or pasted into unapproved browser tabs or applications. | Minimize content capture; use classification events and redacted snippets rather than broad raw collection. |
| **Identity / OAuth Monitoring** | Employees authorizing AI apps, plugins, or connectors with broad scopes. | Require OAuth scope review, app approval, and automatic revocation for prohibited grants. |
| **Browser / Extension Inventory** | AI browser extensions, local agents, or automation plugins installed outside approval. | Maintain allowlist; notify users; block high-risk extensions with data access. |
| **Developer Config Scans** | Unapproved MCP servers, API keys, agent configs, dependency hooks, or local tool servers. | Scan repositories and endpoint configs for risky patterns without collecting unrelated personal data. |
| **Expense / Vendor Records** | Emerging AI SaaS purchases outside procurement review. | Route to vendor governance instead of punishing early adopters by default. |
| **Support / User Reports** | Teams requesting tools or reporting gaps in approved options. | Use demand signals to improve approved golden paths. |

### **Context Ingestion Hygiene and Metadata Context Manifests**

Authorized RAG and context-ingestion pipelines must treat external documents, webpages, emails, tickets, transcripts, and uploaded files as untrusted data. Before content enters retrieval, memory, or the model-facing context window, the ingestion system should verify source, ownership, sensitivity, tenant scope, transformation history, and lifecycle state.

| Hygiene Control | Purpose |
| :---- | :---- |
| **Sandboxed Parsing** | Parse PDFs, Office files, images, HTML, XML, SVG, archives, and other complex formats in constrained, network-restricted environments. |
| **Active Content Removal** | Strip or disable macros, scripts, external entity resolution, hidden text, zero-width payloads, and unsafe embedded objects. |
| **Source and Ownership Binding** | Attach source ID, owner, tenant, license, sensitivity, retention, and lifecycle state. |
| **Transformation Lineage** | Record parser version, chunking version, embedding model, summary generator, and derived artifact hashes. |
| **Permission Inheritance** | Ensure chunks, embeddings, summaries, citations, and cache entries inherit source permissions and classifications. |
| **Retrieval Eligibility Policy** | Admit only active, authorized, non-quarantined sources into retrieval candidate sets. |
| **Prompt-Injection Containment** | Treat document text as evidence, not authority; untrusted content cannot grant tool permissions or override policy. |

A Context Manifest should travel with every model-facing evidence object. It should include source identity, tenant, ACL, sensitivity, parser/chunking lineage, lifecycle state, freshness, and policy version. Context assembly is an access-control decision, not a formatting step.

## **Regulatory Synchronization: Global Compliance Timelines**

AI compliance changes over time. Governance architecture should therefore maintain a regulatory register that distinguishes current legal obligations, future effective dates, provisional political agreements, draft guidance, and organization-specific contractual commitments.

The European Union Artificial Intelligence Act entered into force on August 1, 2024 and applies progressively. Organizations should track two planning views:

1. **Current-law baseline:** obligations and dates in the adopted AI Act and official EU implementation guidance.
2. **Provisional Omnibus planning track:** as of June 2026, EU institutions had reached a provisional agreement on targeted Digital Omnibus changes, including revised timelines for certain high-risk AI obligations. Until formal adoption and publication are complete, organizations should treat these changes as planning assumptions, not as a substitute for legal review.

### **EU AI Act Timeline Planning Matrix**

| Date | Status | Provision / Topic | Governance Action |
| :---- | :---- | :---- | :---- |
| **August 1, 2024** | In force. | EU AI Act enters into force. | Establish AI system inventory, governance owner, regulatory register, and risk-classification workflow. |
| **February 2, 2025** | Current-law baseline. | AI literacy duties and prohibited AI practices begin applying. | Train relevant personnel; block prohibited practices in policy-as-code and procurement gates. |
| **August 2, 2025** | Current-law baseline. | GPAI and AI governance obligations begin applying under the phased timeline. | Maintain model/provider inventory, documentation, vendor evidence, and governance process. |
| **August 2, 2026** | Current-law baseline for broad applicability, subject to exceptions and any formally adopted amendments. | General application date for many AI Act obligations. Article 50 transparency obligations are especially relevant to user-facing AI interactions. | Implement user disclosures, AI interaction labeling, incident and evidence records, and deployer controls for applicable systems. |
| **December 2, 2026** | Provisional Omnibus planning track. | Reported deadline for certain synthetic-content transparency / watermarking-related obligations and new prohibitions under the provisional agreement. | Track final legal text; evaluate watermarking, labeling, output provenance, and prohibited-content controls. Do not hardcode C2PA as the only acceptable mechanism. |
| **December 2, 2027** | Provisional Omnibus planning track. | Reported delayed deadline for many standalone Annex III high-risk AI system obligations. | Continue readiness work: risk management, human oversight, logging, monitoring, post-market controls, and deployer obligations. |
| **August 2, 2028** | Provisional Omnibus planning track. | Reported delayed deadline for product-regulated Annex I high-risk AI systems. | Coordinate with product regulatory, conformity assessment, technical documentation, and safety teams. |

### **Article 26: Deployer Obligations for High-Risk Systems**

For high-risk AI systems, deployers should prepare controls for:

| Obligation Area | Governance Implementation |
| :---- | :---- |
| **Use According to Instructions** | Maintain provider instructions, deployment constraints, operator training, and system-use policies. |
| **Human Oversight** | Assign competent, authorized, supported natural persons for oversight where required. |
| **Input Data Relevance** | Where the deployer controls input data, ensure inputs are relevant and sufficiently representative for the intended purpose. |
| **Monitoring and Suspension** | Monitor operation; suspend use and notify appropriate parties when serious risk is identified. |
| **Log Retention** | Retain logs generated by the high-risk system under deployer control for the legally required period, with at least six months where applicable. |
| **Workplace Notice** | Inform workers and representatives before high-risk AI systems are used in workplace contexts where required. |

This section should be reviewed periodically. Legal deadlines, guidance, standards, and enforcement interpretations can change; the governance architecture must preserve update ownership instead of fossilizing one timeline into the system like a compliance mosquito in amber.

## **The Platform Engineering "Golden Path" to AI Governance**

The goal of a modern platform engineering team is to balance developer velocity with enterprise governance by architecting a **Golden Path**—an opinionated, well-documented, and supported way of building and deploying software.6 Positioned as a standardized self-service route, the Golden Path makes the secure, compliant choice the easiest choice for developers, integrating compliance checks directly into the environment.4

```text
AI GOVERNANCE GOLDEN PATH

[ Developer / Team ]
  declares use case, data class, autonomy level, tools, users, deployment target
        |
        v
+-------------------------------+
| Self-Service Intake           |
| - AI system registration      |
| - risk questionnaire          |
| - data/source declarations    |
+---------------+---------------+
                |
                v
+-------------------------------+
| Template and IaC Generation   |
| - approved service patterns   |
| - logging and telemetry       |
| - sandbox and gateway defaults|
+---------------+---------------+
                |
                v
+-------------------------------+
| Policy-as-Code Gates          |
| - OPA / Kyverno / CI checks   |
| - model/tool/vendor allowlist |
| - cost and security checks    |
+---------------+---------------+
                |
                v
+-------------------------------+
| Evaluation and Review Gates   |
| - golden tests                |
| - security/privacy review     |
| - human approval if required  |
+---------------+---------------+
                |
                v
+-------------------------------+
| Production Runtime Controls   |
| - gateway policy enforcement  |
| - telemetry and audit trails  |
| - incident and rollback hooks |
+---------------+---------------+
                |
                v
+-------------------------------+
| Continuous Governance         |
| - drift monitoring            |
| - vendor review               |
| - incident-to-eval updates    |
| - periodic risk reassessment  |
+-------------------------------+
```

### **The Four Qualities of a Golden Path**

To foster innovation while maintaining structural control, every Golden Path must satisfy four core architectural properties 65:

* **Optional by exception, not by bypass:** Engineering teams may request an alternative approved path when a custom scenario requires it. Mandatory security, privacy, regulatory, and high-impact governance controls remain enforceable regardless of path. Exceptions should be logged, time-bounded, owned, and reviewed.
* **Transparent:** The underlying technologies, security scanners, and container configurations are transparently visible, giving developers the opportunity to learn how the background automation operates.65  
* **Extensible:** The templates are highly modular, allowing teams to add new capabilities, plugins, or evaluation libraries without rewriting the pipeline core.65  
* **Customizable:** The configurations can be adjusted based on the team's existing experience and the business risk of the target application.65

### **The Four Pillars of Platform Control**

A mature, autonomous platform manages compliance across four distinct, AI-driven control mechanisms 4:

1. **Golden Paths:** Self-tuning, automated roadmaps that generate compliant infrastructure and service templates from natural language intents.4  
2. **Guardrails:** Programmatic checks (such as OPA and Kyverno) that prevent non-compliant deployments from reaching production.4  
3. **Safety Nets:** Automated drift remediation systems that continuously scan live environments against desired states, deploying security patches and cleaning up "zombie infrastructure" using embedded time-to-live policies to reduce cloud waste.4  
4. **Manual Review Workflows:** Strategic points of human friction where agents generate comprehensive risk reports, check cost forecasts, and present reviewers with simple risk scores and go/no-go recommendations, ensuring accountability where judgment is required.4

### **FinOps and Security Inner-Loop Integrations**

By shifting governance to "Step Zero" of the developer workflow, the Golden Path prevents compliance failures before resources are ever provisioned.6 OPA-driven security guardrails evaluate Terraform plans to detect contextual privilege escalation paths and verify container signatures.20  
Simultaneously, FinOps guardrails analyze infrastructure changes to output real-time cost impact predictions and enforce resource-tagging policies before merging pull requests, eliminating post-deployment billing surprises and keeping the organization's cloud footprint secure, lean, and auditable.35

## **Cross-Canon Handoff Map**

AI-ENG-AD defines the governance control layer for the canon: policy ownership, accountability, risk classification, approval workflows, audit obligations, procurement gates, regulatory synchronization, and lifecycle governance. It consumes operational evidence from upstream systems and sends enforceable decisions back into gateways, registries, release gates, and runtime monitors.

| Canon Report | Governance Input to AD | AD Decision / Control | Downstream Enforcement |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context object metadata, memory scope, tenure, authority, and state ownership. | Defines governance rules for memory, context retention, and state accountability. | Context compiler, memory policy, retention registry. |
| **AI-ENG-D — Corpus Engineering** | Source authority, data ownership, lifecycle state, provenance, and data quality. | Determines approved data sources, sensitivity rules, retention, and corpus ownership. | Corpus registry, ingestion gates, data lifecycle controls. |
| **AI-ENG-E — Retrieval Pipeline** | Retrieval eligibility, evidence packets, citation bundles, permission filters. | Defines evidence sufficiency, citation requirements, and retrieval governance controls. | Retrieval policy, RLS/ACL enforcement, citation verification gates. |
| **AI-ENG-F — Freshness and Conflict Detection** | Freshness state, conflict packets, source versions, bitemporal records. | Defines acceptable staleness, conflict escalation, and source-of-record hierarchy. | Freshness gates, conflict-resolution policy, user disclosure rules. |
| **AI-ENG-L — Serving Architecture** | Model routes, provider profiles, serving images, latency/cost envelopes. | Approves route classes, fallback eligibility, and model-provider governance. | Gateway route manifests, release gates, fallback policy. |
| **AI-ENG-M — Agentic Orchestration** | Autonomy boundaries, workflow graphs, loop budgets, agent state. | Defines permissible autonomy by risk tier and escalation requirements. | Agent policy, workflow caps, escalation router. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, side-effect class, tool manifests, argument validators. | Approves tool exposure, credential scope, and human-review thresholds. | Tool registry, OPA policy, maker-checker queue. |
| **AI-ENG-O — Action Verification** | Idempotency keys, action ledgers, verification predicates, state hashes. | Defines when completion claims, compensation, and audit obligations are required. | Post-action auditor, source-of-record verification, action ledger. |
| **AI-ENG-P — Multimodal Understanding** | Parser lineage, OCR/layout confidence, media evidence, coordinates. | Defines evidentiary adequacy and review rules for document/media-derived claims. | Evidence adequacy gate, parser approval, human review. |
| **AI-ENG-Q — Speech / Realtime** | Transcript confidence, voice confirmation, interruption, degraded voice state. | Defines when voice channels can authorize or must fall back to visual/text confirmation. | Confirmation policy, high-impact voice gate, transcript retention. |
| **AI-ENG-R — UI Agents** | Browser actions, DOM/screenshot hashes, UI verification, automation uncertainty. | Defines UI-agent autonomy limits and user handoff requirements. | UI automation gate, browser sandbox, action verification. |
| **AI-ENG-S — Production Pathologies** | Failure taxonomy, malformed output, false success, loop pathologies. | Defines which pathology classes require release gates, incidents, or governance review. | Regression tests, incident triggers, reliability SLOs. |
| **AI-ENG-T — Boundary Defense** | Authority hierarchy, tenant isolation, cache scope, prompt-injection events. | Defines non-negotiable security boundaries and escalation paths. | Gateway policy, RLS, cache isolation, fail-closed controls. |
| **AI-ENG-U — Supply Chain Security** | Model signatures, SBOM/AI-BOM, dependency manifests, sandbox profiles. | Defines approved artifacts, vendor requirements, and supply-chain evidence. | Artifact registry, procurement gate, runtime loader policy. |
| **AI-ENG-V — Resource Abuse** | Budget limits, token burn, denial-of-wallet signals, loop caps. | Defines spend authority, quota policy, and abuse-response obligations. | Budget gateway, rate limits, cost approvals. |
| **AI-ENG-W — UX Resilience** | Degraded-mode states, fallback contracts, continuity state, disclosure records. | Defines disclosure, consent, and fallback eligibility by risk tier. | UX state cards, fallback policy, human escalation. |
| **AI-ENG-X — User Trust** | Transparency, contestability, evidence presentation, user correction signals. | Defines user-facing explanation, contestability, and trust-calibration obligations. | UI disclosures, evidence views, appeal/review paths. |
| **AI-ENG-Y — Human Review** | Approval queues, maker-checker records, reviewer decisions, escalation packages. | Defines when human review is mandatory and how review quality is audited. | Approval workflows, reviewer governance, override logs. |
| **AI-ENG-Z — Strategic Telemetry** | Trace IDs, telemetry fields, redaction metadata, drift metrics, SLOs. | Defines required telemetry, retention, access control, and audit scope. | Telemetry collector, secure payload vault, dashboards. |
| **AI-ENG-AA — Evaluations** | Golden sets, canaries, rubrics, adversarial tests, calibration artifacts. | Defines release gates, acceptance criteria, and evaluation ownership. | CI/CD gates, eval registry, model/prompt release approval. |
| **AI-ENG-AB — Verification Artifacts** | Trace manifests, secure references, replay artifacts, evidence trails. | Defines auditability, reproducibility, and evidence-retention requirements. | Audit store, incident package, replay harness. |
| **AI-ENG-AC — AI Operations** | Incident records, runbooks, containment actions, postmortems, corrective actions. | Defines incident reporting, severity governance, accountability, and closure requirements. | Incident workflow, corrective-action tracking, incident-to-eval loop. |
| **AI-ENG-AJ — Reference Architectures** | Blueprint patterns, gateways, registries, sandboxes, audit stores. | Converts governance decisions into default implementation architecture. | Golden paths, platform templates, reference deployments. |

## **Durable Principles of AI Governance Architecture**

To future-proof the enterprise and maintain compliant operations, AI systems architects must adhere to five durable design principles:

### **I. Governance Must Live Outside the Model's Cognitive Path**

Relying on system prompts, inline instructions, or model self-policing to enforce safety boundaries, cost limits, or structural compliance is a critical vulnerability.1 Language models are probabilistic generators whose attention is easily diluted by long contexts, complex reasoning steps, or adversarial prompts.1 True governance must be programmatically enforced outside the model's cognitive path using a centralized, budget-aware API gateway and isolated runtime verification monitors.1

### **II. Context Ingestion is an Access-Control Gate**

No document, database row, memory, or semantic cache entry must enter the model-facing context window unless the system has verified its origin, tenant scope, and user-level permissions.1 Enforcing security filters in application code is an anti-pattern prone to race conditions and logic bugs.1 SaaS platforms must isolate multi-tenant data directly at the database engine layer using Row-Level Security (RLS) policies and partition vector indexes per customer.1

### **III. Spoken and Generated Claims Must Never Outrun Database Realities**

Dialogue orchestrators and conversational interfaces must never vocalize or display a completion confirmation (e.g., stating "Your transfer of $500 is complete") until the underlying API mutation has successfully committed and been verified by a Post-Action Auditor.2 All high-impact, state-changing operations must utilize the Saga distributed transaction pattern to coordinate compensatable, pivot, and retriable steps, ensuring eventual consistency across services before outputs are rendered on the viewport.2

### **IV. Every Consequential Action Requires a Verifiable Evidence Trail**

Every automated transaction, policy override, user correction, and human review decision must leave behind a tamper-evident evidence trail. That trail should bind the prompt version, model route, policy decision, source/evidence identifiers, tool payload hash, approval record, pre/post state hashes, and secure payload references where raw evidence is required.

Governance evidence should be sufficient to audit and replay decisions without dumping raw prompts, context chunks, tool arguments, credentials, or sensitive user data into ordinary logs. Raw evidence belongs in controlled storage with access controls, retention limits, and access auditing.

### **V. Security Controls Must Retain Rigor Under Degradation**

When backend services degrade, model providers timeout, or rate limits are hit, the platform must degrade gracefully along explicit quality, cost, and latency dimensions while keeping its security boundaries strict.5 A system must never fail-open, bypass permission checks, or disable tenant filters to maintain service availability.1 If compliance or safety floors cannot be satisfied by available fallback paths, the architecture must fail-closed, secure the active state, and escalate the transaction to human review queues.1

#### **Works cited**

1. [KNOWLEDGE] - AI Engineering - Volume 7. S-V Failure, Security, and Hostile Environments  
2. [KNOWLEDGE] - AI Engineering - Volume 9. Z-AB Observability, Evaluation, and Verification  
3. Maker-Checker implementation guide for secure FinTech systems, accessed June 12, 2026, [https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029](https://opcitotechnologies.medium.com/makermaker-checker-implementation-guide-for-secure-fintech-systems-eabe13ff7029)  
4. The autonomous enterprise and the four pillars of platform control: 2026 forecast | CNCF, accessed June 12, 2026, [https://www.cncf.io/blog/2026/01/23/the-autonomous-enterprise-and-the-four-pillars-of-platform-control-2026-forecast/](https://www.cncf.io/blog/2026/01/23/the-autonomous-enterprise-and-the-four-pillars-of-platform-control-2026-forecast/)  
5. [KNOWLEDGE] - AI Engineering - Volume 8. W-Y Resilience, Degraded Modes, and Human Trust  
6. Architecting Trust: The Blueprint for a "Golden Standard" Software Supply Chain - Harness, accessed June 12, 2026, [https://www.harness.io/blog/architecting-trust-the-blueprint-for-a-golden-standard-software-supply-chain](https://www.harness.io/blog/architecting-trust-the-blueprint-for-a-golden-standard-software-supply-chain)  
7. How to map AI governance controls to NIST AI RMF functions - Prediction Guard, accessed June 12, 2026, [https://predictionguard.com/blog/how-to-map-ai-governance-controls-to-nist-ai-rmf-functions](https://predictionguard.com/blog/how-to-map-ai-governance-controls-to-nist-ai-rmf-functions)  
8. ISO 42001 Statement of Applicability Explained - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/statement-of-applicability/](https://www.isms.online/iso-42001/statement-of-applicability/)  
9. ISO 42001 Statement Of Applicability (SoA) | Detailed Guide - Cyberzoni.com, accessed June 12, 2026, [https://cyberzoni.com/iso-42001-soa/](https://cyberzoni.com/iso-42001-soa/)  
10. ISO 42001 Annex A Controls Explained - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/annex-a-controls/](https://www.isms.online/iso-42001/annex-a-controls/)  
11. Understanding the NIST AI RMF (AI Risk Management Framework) - Dastra.eu, accessed June 12, 2026, [https://www.dastra.eu/en/blog/understanding-the-nist-ai-risk-management-framework/59940](https://www.dastra.eu/en/blog/understanding-the-nist-ai-risk-management-framework/59940)  
12. AI RMF Core - AIRC - NIST AI Resource Center, accessed June 12, 2026, [https://airc.nist.gov/airmf-resources/airmf/5-sec-core/](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)  
13. NIST AI RMF: Govern, Map, Measure, Manage as a Process | BA Copilot, accessed June 12, 2026, [https://ba-copilot.com/nist-ai-rmf](https://ba-copilot.com/nist-ai-rmf)  
14. ISO 42001 Annex A Controls List: A.5, A.6, A.7, A.8 (38 Controls) | Mindset Cyber, accessed June 12, 2026, [https://mindsetcyber.com.au/iso-42001-controls-list/](https://mindsetcyber.com.au/iso-42001-controls-list/)  
15. ISO 42001 Annex Control Objectives And Controls | Gabriel Consultant Limited, accessed June 12, 2026, [https://gabriel.hk/iso-42001-annex-control-objectives-and-controls/](https://gabriel.hk/iso-42001-annex-control-objectives-and-controls/)  
16. Process Automation and Cross-System Data Integrations | Watabe Digital Insights, accessed June 12, 2026, [https://watabedigital.co.tz/insights/business-process-automation-tanzania/automation-and-system-integration/](https://watabedigital.co.tz/insights/business-process-automation-tanzania/automation-and-system-integration/)  
17. Ready for ISO 42001? Let's Talk Next Steps | Drata Help Center, accessed June 12, 2026, [https://help.drata.com/en/articles/12591328-ready-for-iso-42001-let-s-talk-next-steps](https://help.drata.com/en/articles/12591328-ready-for-iso-42001-let-s-talk-next-steps)  
18. Building a Least-Privilege AI Agent Gateway for Infrastructure Automation with MCP, OPA, and Ephemeral Runners - InfoQ, accessed June 12, 2026, [https://www.infoq.com/articles/building-ai-agent-gateway-mcp/](https://www.infoq.com/articles/building-ai-agent-gateway-mcp/)  
19. What Is Policy-As-Code? Benefits & Best Practices - Apiiro, accessed June 12, 2026, [https://apiiro.com/glossary/policy-as-code-2/](https://apiiro.com/glossary/policy-as-code-2/)  
20. Policy as code: The platform engineer's guide to automated governance and compliance, accessed June 12, 2026, [https://platformengineering.org/blog/policy-as-code](https://platformengineering.org/blog/policy-as-code)  
21. Open Policy Agent (OPA), accessed June 12, 2026, [https://openpolicyagent.org/docs](https://openpolicyagent.org/docs)  
22. Principled Evolution (GOPAL & AICertify) - Open Policy Agent, accessed June 12, 2026, [https://openpolicyagent.org/ecosystem/entry/principled-evolution](https://openpolicyagent.org/ecosystem/entry/principled-evolution)  
23. How to Validate Policy-as-Code Without Breaking Builds (Even When AI Writes the Code), accessed June 12, 2026, [https://jfrog.com/blog/how-to-validate-policy-as-code/](https://jfrog.com/blog/how-to-validate-policy-as-code/)  
24. What is Open Policy Agent (OPA)? Best Practices + Applications - Wiz, accessed June 12, 2026, [https://www.wiz.io/academy/application-security/open-policy-agent-opa](https://www.wiz.io/academy/application-security/open-policy-agent-opa)  
25. What is Policy-as-Code? - Sysdig, accessed June 12, 2026, [https://www.sysdig.com/learn-cloud-native/what-is-policy-as-code](https://www.sysdig.com/learn-cloud-native/what-is-policy-as-code)  
26. SARC: A Governance-by-Architecture Framework for Agentic AI Systems - arXiv, accessed June 12, 2026, [https://arxiv.org/pdf/2605.07728](https://arxiv.org/pdf/2605.07728)  
27. SARC: A Governance-by-Architecture Framework for Agentic AI Systems Compiling Regulatory Obligations into Runtime Constraints - arXiv, accessed June 12, 2026, [https://arxiv.org/html/2605.07728v1](https://arxiv.org/html/2605.07728v1)  
28. SARC: A Governance-by-Architecture Framework for Agentic AI, accessed June 12, 2026, [https://www.aimodels.fyi/papers/arxiv/sarc-governance-by-architecture-framework-agentic-ai](https://www.aimodels.fyi/papers/arxiv/sarc-governance-by-architecture-framework-agentic-ai)  
29. Gaston Besanson's research works - ResearchGate, accessed June 12, 2026, [https://www.researchgate.net/scientific-contributions/Gaston-Besanson-2340161255](https://www.researchgate.net/scientific-contributions/Gaston-Besanson-2340161255)  
30. Outsider Oversight: Designing a Third Party Audit Ecosystem for AI Governance | Request PDF - ResearchGate, accessed June 12, 2026, [https://www.researchgate.net/publication/362295642_Outsider_Oversight_Designing_a_Third_Party_Audit_Ecosystem_for_AI_Governance](https://www.researchgate.net/publication/362295642_Outsider_Oversight_Designing_a_Third_Party_Audit_Ecosystem_for_AI_Governance)  
31. (PDF) SARC: A Governance-by-Architecture Framework for Agentic, accessed June 12, 2026, [https://www.researchgate.net/publication/404712995_SARC_A_Governance-by-Architecture_Framework_for_Agentic_AI_Systems](https://www.researchgate.net/publication/404712995_SARC_A_Governance-by-Architecture_Framework_for_Agentic_AI_Systems)  
32. Article 26: Obligations of Deployers of High-Risk AI Systems | EU Artificial Intelligence Act, accessed June 12, 2026, [https://artificialintelligenceact.eu/article/26/](https://artificialintelligenceact.eu/article/26/)  
33. The CISO's Guide to EU AI Act Compliance - Cequence AI, accessed June 12, 2026, [https://www.cequence.ai/eu-ai-act.html](https://www.cequence.ai/eu-ai-act.html)  
34. AI Data Privacy vs SaaS: What Changes For Enterprise IT In 2026 - CloudNuro.ai, accessed June 12, 2026, [https://www.cloudnuro.ai/blog/ai-data-privacy-vs-saas-what-changes-for-enterprise-it-in-2026](https://www.cloudnuro.ai/blog/ai-data-privacy-vs-saas-what-changes-for-enterprise-it-in-2026)  
35. How AI Is Becoming the Governance Layer Inside Your CI/CD Pipeline - Medium, accessed June 12, 2026, [https://medium.com/@mrigank.online/how-ai-is-becoming-the-governance-layer-inside-your-ci-cd-pipeline-ce485bb96a21](https://medium.com/@mrigank.online/how-ai-is-becoming-the-governance-layer-inside-your-ci-cd-pipeline-ce485bb96a21)  
36. Immutable Payment Audit Trails: Storage, Discovery, and Audits - Chequedb, accessed June 12, 2026, [https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires](https://chequedb.com/resources/blog/immutable-audit-trails-101-what-financial-compliance-actually-requires)  
37. Maker-checker spec: discussion, approach, design - Mifos X - MifosForge, accessed June 12, 2026, [https://mifosforge.jira.com/wiki/pages/viewpage.action?pageId=27164690&navigatingVersions=true](https://mifosforge.jira.com/wiki/pages/viewpage.action?pageId=27164690&navigatingVersions=true)  
38. Maker-Checker Pattern: Dual-Control System Implementation - Opcito, accessed June 12, 2026, [https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems](https://www.opcito.com/blogs/maker-checker-implementation-guide-for-secure-fintech-systems)  
39. Core Banking ERP Part 2 – Data Model, Ledger & Core Products - ClefinCode, accessed June 12, 2026, [https://clefincode.com/blog/global-digital-vibes/en/core-banking-erp-part-2-data-model-ledger-core-products](https://clefincode.com/blog/global-digital-vibes/en/core-banking-erp-part-2-data-model-ledger-core-products)  
40. Double Entry Accounting in a Relational Database | by Robert Chanphakeo | Medium, accessed June 12, 2026, [https://medium.com/@RobertKhou/double-entry-accounting-in-a-relational-database-2b7838a5d7f8](https://medium.com/@RobertKhou/double-entry-accounting-in-a-relational-database-2b7838a5d7f8)  
41. Fineract Platform Documentation, accessed June 12, 2026, [https://fineract.apache.org/docs/current/](https://fineract.apache.org/docs/current/)  
42. AI Contracting: Practical Legal Guidance for Growing Businesses - Koley Jessen, accessed June 12, 2026, [https://www.koleyjessen.com/insights/publications/ai-contracting-practical-legal-guidance-for-growing-businesses](https://www.koleyjessen.com/insights/publications/ai-contracting-practical-legal-guidance-for-growing-businesses)  
43. AI Vendor Risk Management: Assessing Third Party AI Suppliers - ISMS.online, accessed June 12, 2026, [https://www.isms.online/iso-42001/ai-vendor-risk/](https://www.isms.online/iso-42001/ai-vendor-risk/)  
44. AI Contract Clauses That Reduce Vendor, Data, and Liability Risk, accessed June 12, 2026, [https://hernanhuwyler.wordpress.com/2026/03/12/ai-procurement-controls/](https://hernanhuwyler.wordpress.com/2026/03/12/ai-procurement-controls/)  
45. Enterprise AI Vendor RFP: 40 Questions to Ask (2026) - Worqlo, accessed June 12, 2026, [https://worqlo.com/blog/enterprise-ai-vendor-rfp-questions/](https://worqlo.com/blog/enterprise-ai-vendor-rfp-questions/)  
46. AI Clauses In Contracts: The Practical Guide For 2025 - Tascon – Legal, accessed June 12, 2026, [https://tasconlegal.com/ai-clauses-in-contracts-the-practical-guide-for-2025/](https://tasconlegal.com/ai-clauses-in-contracts-the-practical-guide-for-2025/)  
47. Vendor Agreement Review: The 6 Clauses That Decide the Risk - GC AI, accessed June 12, 2026, [https://gc.ai/blog/vendor-agreement-review](https://gc.ai/blog/vendor-agreement-review)  
48. The privacy line AI platforms are crossing with user data - Regolo.AI, accessed June 12, 2026, [https://regolo.ai/the-privacy-line-ai-platforms-are-crossing-with-user-data/](https://regolo.ai/the-privacy-line-ai-platforms-are-crossing-with-user-data/)  
49. What Is Shadow AI? Detection, Risks and Governance in 2026 - Forcepoint, accessed June 12, 2026, [https://www.forcepoint.com/blog/insights/what-is-shadow-ai](https://www.forcepoint.com/blog/insights/what-is-shadow-ai)  
50. What Is Shadow AI? Definition, Risks & Governance Strategies - SentinelOne, accessed June 12, 2026, [https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-shadow-ai/](https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-shadow-ai/)  
51. Shadow AI explained: risks, costs, and enterprise governance - Vectra AI, accessed June 12, 2026, [https://www.vectra.ai/topics/shadow-ai](https://www.vectra.ai/topics/shadow-ai)  
52. Shadow AI: How Unmonitored Tools Bypass Security and Enter Your Business, accessed June 12, 2026, [https://compassmsp.com/resources/articles/how-unmonitored-ai-tools-are-entering-your-business](https://compassmsp.com/resources/articles/how-unmonitored-ai-tools-are-entering-your-business)  
53. What Is Shadow AI? Definition | Proofpoint US, accessed June 12, 2026, [https://www.proofpoint.com/us/threat-reference/shadow-ai](https://www.proofpoint.com/us/threat-reference/shadow-ai)  
54. How to Create a Corporate AI Policy for Your Organization | Otter.ai, accessed June 12, 2026, [https://otter.ai/blog/corporate-ai-policy](https://otter.ai/blog/corporate-ai-policy)  
55. [KNOWLEDGE] - AI Engineering - Volume 6. P-R Multimodal and Interface-Controlling Systems  
56. How to Get AI Act-Ready: A 2026 Compliance Guide | Dawiso Blog, accessed June 12, 2026, [https://www.dawiso.com/blog-post/how-to-get-ai-act-ready-2026](https://www.dawiso.com/blog-post/how-to-get-ai-act-ready-2026)  
57. EU AI Act High-Risk Deadline: Enterprise Readiness Gap - Lab Space, accessed June 12, 2026, [https://labs.cloudsecurityalliance.org/research/csa-research-note-eu-ai-act-high-risk-compliance-deadline-20/](https://labs.cloudsecurityalliance.org/research/csa-research-note-eu-ai-act-high-risk-compliance-deadline-20/)  
58. AI Act Update: EU Resolves to Change Rules and Extend Deadlines, accessed June 12, 2026, [https://www.lw.com/en/insights/ai-act-update-eu-resolves-to-change-rules-and-extend-deadlines](https://www.lw.com/en/insights/ai-act-update-eu-resolves-to-change-rules-and-extend-deadlines)  
59. AI Act reloaded? What the latest AI Act changes mean in practice - Stibbe, accessed June 12, 2026, [https://www.stibbe.com/publications-and-insights/ai-act-reloaded-what-the-latest-ai-act-changes-mean-in-practice](https://www.stibbe.com/publications-and-insights/ai-act-reloaded-what-the-latest-ai-act-changes-mean-in-practice)  
60. EU AI Act Update: Timeline Relief, Targeted Simplification, and New Prohibitions, accessed June 12, 2026, [https://www.insideprivacy.com/artificial-intelligence/eu-ai-act-update-timeline-relief-targeted-simplification-and-new-prohibitions/](https://www.insideprivacy.com/artificial-intelligence/eu-ai-act-update-timeline-relief-targeted-simplification-and-new-prohibitions/)  
61. EU AI Act enforcement starts in 75 days - affects any team building AI agents for European clients : r/artificial - Reddit, accessed June 12, 2026, [https://www.reddit.com/r/artificial/comments/1tgf0gm/eu_ai_act_enforcement_starts_in_75_days_affects/](https://www.reddit.com/r/artificial/comments/1tgf0gm/eu_ai_act_enforcement_starts_in_75_days_affects/)  
62. The EU AI Act's Transparency Rules: A Practical Guide to Article 50, accessed June 12, 2026, [https://artificialintelligenceact.eu/transparency-rules-article-50/](https://artificialintelligenceact.eu/transparency-rules-article-50/)  
63. EU Deepfake Rules in AI Act Will Affect More Businesses Than Usually Expected, accessed June 12, 2026, [https://www.potomaclaw.com/news-EU-Deepfake-Rules-in-AI-Act-Will-Affect-More-Businesses-Than-Usually-Expected](https://www.potomaclaw.com/news-EU-Deepfake-Rules-in-AI-Act-Will-Affect-More-Businesses-Than-Usually-Expected)  
64. EU AI Act Compliance: Complete Deadlines Guide 2025 - Tech Jacks Solutions, accessed June 12, 2026, [https://techjacksolutions.com/ai-brief/eu-ai-act-three-deadlines-three-compliance-programs-which-on/](https://techjacksolutions.com/ai-brief/eu-ai-act-three-deadlines-three-compliance-programs-which-on/)  
65. What is a Golden Path for software development? - Red Hat, accessed June 12, 2026, [https://www.redhat.com/en/topics/platform-engineering/golden-paths](https://www.redhat.com/en/topics/platform-engineering/golden-paths)

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