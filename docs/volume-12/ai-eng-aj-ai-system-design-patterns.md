# AI-ENG-AJ — AI System Design Patterns - Reference Architectures & Failure-Aware Blueprints

## **Conceptual Glossary**

* **AI System Design Pattern**: A formalized, reusable template addressing a recurring coordination and execution challenge in non-deterministic computing environments. Unlike traditional software patterns that assume static control flows, an AI design pattern maps state transitions while managing the variability and latent risks of foundation model completions.1  
* **Reference Architecture**: A highly structured blueprint defining the canonical components, data flows, integration surfaces, and interface boundaries required to instantiate a specific class of AI system.3 It details both functional blocks and the exact contracts that isolate failures and enforce deterministic behavior at probabilistic boundaries.4  
* **Failure-Aware Blueprint**: An engineering specification that treats operational failure as a statistical certainty rather than an exceptional anomaly. It embeds detection, mitigation, containment, and recovery mechanisms directly into the system topology to ensure graceful degradation under failure states.4  
* **Pattern Card**: A standardized, multi-dimensional specification document used to evaluate, select, implement, and audit a specific AI system design pattern. It operates as the definitive schema of record for an architecture, establishing boundary conditions, testing obligations, and operational controls.  
* **No-Use Condition**: A set of technical, operational, or business constraints under which a specific AI design pattern must not be deployed. It defines the boundaries of the pattern's viability, routing architects to deterministic or non-AI alternatives when those boundaries are breached.  
* **Anti-Pattern**: A common, superficially attractive design choice or implementation shortcut that consistently yields systemic failures, uncontrollable costs, security vulnerabilities, or operational degradation in production environments.1  
* **Degraded Mode**: A predefined, safe, lower-capability operational state that a system autonomously or semi-autonomously downshifts into when its primary probabilistic pathways fail.4 It prioritizes structural safety, data integrity, and predictability over complete functionality.  
* **Contract Surface Map**: A multi-dimensional cross-reference index defining which structural contracts (such as schema, permission, prompt, and tool contracts) are mandatory, conditional, or optional across a portfolio portfolio of reference architectures.  
* **Eval-by-Pattern**: The systematic mapping of specialized, quantitative evaluation metrics (e.g., faithfulness, context precision, and tool-call validity) to specific system design patterns to validate execution safety and performance.7  
* **Telemetry-by-Pattern**: The prescriptive specification of the exact trace elements, telemetry attributes, and state variables that must be captured and logged for a given pattern to ensure comprehensive observability, auditing, and debugging.4  
* **Human Review Map**: A governance matrix specifying the precise interfaces, roles, and levels of authority reserved for human operators within agentic and semi-autonomous systems, calibrated against operational risk tiers.1  
* **Security Boundary Map**: A structural specification of the isolation zones, data egress controls, credentials access parameters, and sandboxing requirements across different patterns to defend against prompt injection, privilege escalation, and data exfiltration.9  
* **Pattern Maturity**: A six-tier progression framework used to assess the readiness of an AI pattern implementation from initial local prototype (Level 0) to a highly platformized, automated golden path (Level 5).

## **AI Reference Architecture Doctrine**

The foundational thesis of high-dimensional AI engineering asserts that a reference architecture is not merely an illustrative collection of boxes and arrows; it is a decision artifact. A pattern is reusable only when its boundaries, assumptions, and breach behaviors are explicit. Probabilistic cores—such as large language and vision models—must be tightly surrounded by deterministic, typed, versioned, and observable edges. Consequently, an effective reference architecture must encode its contract surfaces, evaluation gates, failure modes, anti-patterns, operating controls, degraded modes, and no-use conditions directly into the structural design.  
Traditional systems engineering optimizes for deterministic reliability: inputs are transformed into outputs via known, verifiable code paths. In contrast, AI engineering systems govern probabilistic engines where identical inputs can yield divergent, semantic completions.1 This non-deterministic quality introduces systemic failure modes—including silent drift, tool-use parameter hallucination, cascade failures, and context attention dilution—which cannot be resolved by standard debugging paradigms.5  
To build production-grade systems, architects must apply the principles of contract-driven architecture. Every transition point between a deterministic service and a probabilistic model must be governed by an explicit contract stack, as established in the systemic canon. This stack spans user expectations, workflows, policies, prompts, retrievals, routes, schemas, tools, evaluations, deployments, and sourcing models. A design pattern represents the repeatable composition of these contracts to resolve a specific workload class. By structuring patterns as failure-aware blueprints, engineering teams can guarantee that when a probabilistic core fails to meet a contract, the surrounding deterministic architecture isolates the failure, records the complete trajectory, triggers appropriate recovery mechanisms, and downgrades the service controlledly.4

## **Pattern Card Template**

Every reference architecture in this doctrine is documented as a Pattern Card. A Pattern Card is not a decorative summary. It is the reusable architecture record for a workload class: when to use the pattern, when not to use it, what contracts are required, how it fails, how it degrades, how it is evaluated, and who remains accountable.

### **Canonical Pattern Card Schema**

| Field | Required Content |
| :---- | :---- |
| **Pattern Name** | Canonical architecture name. |
| **Problem Class** | The recurring system problem this pattern solves. |
| **Best-Fit Use Cases** | Workloads where the pattern is structurally appropriate. |
| **No-Use Conditions** | Conditions that route the team to deterministic software, another pattern, or no-AI. |
| **User Surface** | Chat, inline UI, queue, cockpit, API, background service, review panel, etc. |
| **Architecture Shape** | Primary components and dataflow. |
| **Required Contracts** | Mandatory AI-ENG-AI contract surfaces. |
| **Human Authority** | Human role, approval boundary, veto power, and review burden. |
| **Model Route Strategy** | Capability profile and route class, not brittle provider names. |
| **Retrieval / Context Strategy** | What context enters the system and how it is governed. |
| **Tool / Action Strategy** | Whether tools are read-only, write-capable, sandboxed, or human-approved. |
| **Core Evals** | Pattern-specific quality, safety, latency, cost, and regression checks. |
| **Telemetry** | Operational metrics needed to debug and improve the pattern. |
| **Audit / Evidence Boundary** | Minimal evidence required for compliance, incident review, or replay. |
| **Security Boundary** | Isolation, permission, data egress, credential, and sandbox requirements. |
| **Primary Cost Drivers** | Tokens, retrieval, GPU, storage, review labor, tool calls, or platform operations. |
| **Failure Modes** | Known ways the pattern fails in production. |
| **Anti-Patterns** | Common tempting but unsafe implementations. |
| **Degraded Mode** | Safe reduced-capability behavior. |
| **Adoption and Support** | Training, workflow change, support, and user enablement needs. |
| **Sourcing / Exit** | Portability, vendor dependency, and migration considerations. |
| **Maturity Target** | Expected implementation maturity level for production use. |

### **Pattern Card Markdown Form**

Each card should be written as real Markdown, not fenced pseudo-documentation. This makes the pattern searchable, linkable, diffable, and indexable.

```markdown
### **Pattern Name**

| Field | Specification |
| :---- | :---- |
| **Problem Class** |  |
| **Best-Fit Use Cases** |  |
| **No-Use Conditions** |  |
| **User Surface** |  |
| **Architecture Shape** |  |
| **Required Contracts** |  |
| **Human Authority** |  |
| **Model Route Strategy** |  |
| **Retrieval / Context Strategy** |  |
| **Tool / Action Strategy** |  |
| **Core Evals** |  |
| **Telemetry** |  |
| **Audit / Evidence Boundary** |  |
| **Security Boundary** |  |
| **Primary Cost Drivers** |  |
| **Failure Modes** |  |
| **Anti-Patterns** |  |
| **Degraded Mode** |  |
| **Adoption and Support** |  |
| **Sourcing / Exit** |  |
| **Maturity Target** |  |
```

The Pattern Card is the handoff object between product architecture, engineering implementation, governance, evaluation, operations, adoption, and sourcing.

## **Architecture Pattern Taxonomy**

AI system patterns should be organized by workflow archetype, authority level, state mutation, evidence burden, and user-review structure. A taxonomy is useful only if it helps teams route a real requirement to the correct architecture and reject bad fits early.

```text
AI SYSTEM DESIGN PATTERN TAXONOMY

1. Interactive & Embedded
   ├── Copilot / Embedded Assistant
   └── Personal or Team Productivity Assistant

2. Retrieval & Synthesis
   ├── Research Agent
   └── Enterprise Knowledge System

3. Extraction & Classification
   ├── Document Intelligence Pipeline
   ├── Multimodal Review System
   └── Background Classifier / Router

4. Analytics & Decision Intelligence
   ├── Analytics Assistant
   └── Decision-Support Cockpit

5. Support & Service Operations
   └── Support Assistant

6. Action-Oriented / Agentic
   ├── Workflow Automation Agent
   ├── Coding Agent
   └── Governed Agentic Workflow

7. Human Review & Governance
   └── Human Review and Escalation Queue

8. Platform Infrastructure
   ├── AI Gateway / Control Plane
   └── Evaluation and Shadow-Mode Pattern
```

### **Taxonomy by Control Property**

| Pattern Family | Primary User Surface | Runtime Authority | Primary Risk | Core Contract Emphasis |
| :---- | :---- | :---- | :---- | :---- |
| **Interactive & Embedded** | Inline assistant, sidebar, editor surface. | Suggests; user accepts or edits. | Overtrust, context leakage, poor fit. | Prompt, context, route, observability, user expectation. |
| **Retrieval & Synthesis** | Search portal, research console, cited answer. | Synthesizes from evidence. | Citation theater, stale/unauthorized evidence. | Retrieval, grounding, freshness, permission, eval. |
| **Extraction & Classification** | Queue, parser, background service, review panel. | Produces structured output or route label. | Silent field errors, misrouting, reviewer fatigue. | Schema, evidence, confidence, exception queue, eval. |
| **Analytics & Decision Intelligence** | BI workspace, risk cockpit, scenario panel. | Explains, computes, or recommends; human decides. | Metric hallucination, biased framing, wrong calculation. | Semantic metric, SQL/query, grounding, human review, audit. |
| **Support & Service Operations** | Customer chat, agent assist, support console. | Drafts, triages, or resolves bounded issues. | Deflection theater, policy hallucination, poor handoff. | Retrieval, escalation, user expectation, audit, telemetry. |
| **Action-Oriented / Agentic** | Task dashboard, PR interface, process portal. | Plans or executes bounded steps under policy. | Unauthorized side effects, loops, partial execution. | Tool, permission, idempotency, resource, action verification. |
| **Human Review & Governance** | Review queue, approval panel, audit cockpit. | Human validates and authorizes. | Rubber-stamping, queue overload, weak evidence. | Human review, evidence, override, audit, telemetry. |
| **Platform Infrastructure** | Internal API, gateway, release dashboard. | Controls routes, policy, eval, and observability. | Central failure, bypass, weak evidence, cost blowout. | Deployment, route, policy, resource, observability, sourcing. |

Pattern selection is not model selection. It is authority design.

## **Pattern Selection Tree**

The selection tree routes a workload to the safest viable architecture pattern. It includes deterministic and no-AI paths because not every valuable workflow deserves a model-shaped hole punched through it.

```text
PATTERN SELECTION TREE

[ Candidate Workflow ]
        |
        v
Q1. Is the task fully deterministic, exact, or better solved by rules/database/forms?
        |
        +-- yes --> [ Deterministic Software / No-AI Path ]
        |
        v
Q2. Does the system need to mutate external state or execute side effects?
        |
        +-- yes --> Q3
        |
        +-- no  --> Q7

Q3. Is the task software codebase modification, build/test, or PR generation?
        |
        +-- yes --> [ Coding Agent ]
        |
        +-- no  --> Q4

Q4. Is the action path mostly fixed, schema-bound, and workflow-driven?
        |
        +-- yes --> [ Workflow Automation Agent ]
        |
        +-- no  --> Q5

Q5. Does the task require dynamic planning, multi-step reasoning, or multi-agent checks?
        |
        +-- yes --> [ Governed Agentic Workflow ]
        |
        +-- no  --> Q6

Q6. Is the action high-risk, irreversible, regulated, or approval-sensitive?
        |
        +-- yes --> [ Human Review and Escalation Queue + Deterministic Execution ]
        |
        +-- no  --> [ Workflow Automation Agent ]

Q7. Is the primary task factual retrieval, synthesis, or evidence search?
        |
        +-- yes --> Q8
        |
        +-- no  --> Q11

Q8. Is the corpus enterprise-owned, multi-repository, permissioned, and lifecycle-managed?
        |
        +-- yes --> [ Enterprise Knowledge System ]
        |
        +-- no  --> Q9

Q9. Is the search open-ended, multi-hop, external, or research-oriented?
        |
        +-- yes --> [ Research Agent ]
        |
        +-- no  --> Q10

Q10. Is the output a high-stakes recommendation with alternatives and rationale?
        |
        +-- yes --> [ Decision-Support Cockpit ]
        |
        +-- no  --> [ Enterprise Knowledge System or Deterministic Search ]

Q11. Is the primary task structured extraction from documents or media?
        |
        +-- yes --> Q12
        |
        +-- no  --> Q14

Q12. Does the input include image, audio, video, scanned media, or spatial evidence?
        |
        +-- yes --> [ Multimodal Review System ]
        |
        +-- no  --> Q13

Q13. Is the target output typed fields from documents?
        |
        +-- yes --> [ Document Intelligence Pipeline ]
        |
        +-- no  --> [ Deterministic Parser / No-AI Path ]

Q14. Is the primary task governed analytics, SQL, metrics, or dashboard explanation?
        |
        +-- yes --> [ Analytics Assistant ]
        |
        +-- no  --> Q15

Q15. Is the primary task customer or internal support?
        |
        +-- yes --> [ Support Assistant ]
        |
        +-- no  --> Q16

Q16. Is the workload high-throughput background classification or routing?
        |
        +-- yes --> [ Background Classifier / Router ]
        |
        +-- no  --> Q17

Q17. Is the AI embedded inside an active workspace as inline assistance?
        |
        +-- yes --> [ Copilot / Embedded Assistant ]
        |
        +-- no  --> Q18

Q18. Is the system a personal/team assistant with local context and low action authority?
        |
        +-- yes --> [ Personal or Team Productivity Assistant ]
        |
        +-- no  --> Q19

Q19. Is the requirement infrastructure for model access, routing, policy, or cost control?
        |
        +-- yes --> [ AI Gateway / Control Plane ]
        |
        +-- no  --> Q20

Q20. Is the requirement validating candidate models/prompts/routes without affecting users?
        |
        +-- yes --> [ Evaluation and Shadow-Mode Pattern ]
        |
        +-- no  --> [ Return to Product Discovery / Pattern Not Selected ]
```

### **Selection Rule**

If two patterns appear plausible, choose the one with less autonomy, clearer evidence, lower integration burden, and safer degraded mode. If the task can be solved cleanly without AI, that is not a failure of imagination. It is architecture doing its job.

## **Reference Blueprint Set**

The reference blueprint set defines reusable AI system patterns. Each pattern is expressed as a real Markdown card so it can be searched, indexed, diffed, linked, and reused by architecture teams.

### **1. Copilot / Embedded Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Interactive, context-aware assistance inside an active workspace. |
| **Best-Fit Use Cases** | Inline code suggestions, prose completion, spreadsheet assistance, structured form guidance, drafting aids. |
| **No-Use Conditions** | High-liability transactions, exact calculations, background batch processing, or actions requiring autonomous write authority. |
| **User Surface** | Inline suggestions, ghost text, side panel, contextual menu. |
| **Architecture Shape** | Workspace event → context builder → policy/data filter → fast model route → suggestion renderer → user accept/edit/reject → telemetry/eval loop. |
| **Required Contracts** | Prompt, context, schema where structured, resource, model route, observability, user expectation. |
| **Human Authority** | Human remains active controller; AI suggests only. |
| **Model Route Strategy** | Low-latency route optimized for short completions; escalation only for explicitly requested deeper help. |
| **Retrieval / Context Strategy** | Local workspace state, selected document/code region, nearby context, active user intent. |
| **Tool / Action Strategy** | No external side effects; local UI modifications only. |
| **Core Evals** | Acceptance quality, edit distance, compile/test result where applicable, latency, user correction patterns. |
| **Telemetry** | Suggestion hash, route ID, prompt/schema version, accept/reject/edit event, latency, cost bucket. |
| **Audit / Evidence Boundary** | Usually operational telemetry only; sensitive content should be redacted or referenced securely. |
| **Security Boundary** | Workspace context must be scoped; cloud routes require data filtering and tenant policy. |
| **Primary Cost Drivers** | High-frequency small completions, context assembly, streaming latency. |
| **Failure Modes** | Context distraction, stale suggestions, overtrust, low-quality accepted code/text. |
| **Anti-Patterns** | Chatbox on everything; acceptance rate treated as correctness. |
| **Degraded Mode** | Static templates, local autocomplete, deterministic snippets. |
| **Adoption and Support** | Low training burden, but users need calibration on review and acceptance. |
| **Sourcing / Exit** | Keep completion interface provider-neutral. |
| **Maturity Target** | Level 3–4 for production; Level 5 when platformized across teams. |

### **2. Research Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Open-ended multi-source discovery, analysis, and factual synthesis. |
| **Best-Fit Use Cases** | Market research, literature review, policy analysis, competitor research, legal or technical source aggregation. |
| **No-Use Conditions** | Exact database lookup, simple FAQ retrieval, high-speed customer answer, or unsupported evidence domains. |
| **User Surface** | Research console, plan editor, citation panel, source browser, draft workspace. |
| **Architecture Shape** | Research question → plan/query decomposition → bounded search → source authority filter → evidence clustering → synthesis → citation verifier → human audit. |
| **Required Contracts** | Prompt, retrieval, grounding, source authority, freshness, resource, eval, observability, user expectation. |
| **Human Authority** | Human sets objective, approves plan, reviews sources, and accepts final synthesis. |
| **Model Route Strategy** | Planning/synthesis route for reasoning; cheaper extraction/summarization routes for source processing. |
| **Retrieval / Context Strategy** | Multi-hop search with source authority, freshness, dedupe, and conflict handling. |
| **Tool / Action Strategy** | Read-only search/document tools; sandboxed browsing or parsers. |
| **Core Evals** | Citation fidelity, claim support, source quality, context recall, contradiction handling, synthesis usefulness. |
| **Telemetry** | Query plan, search calls, source IDs, citation verifier status, loop count, cost, user edits. |
| **Audit / Evidence Boundary** | Source manifest, claim/evidence map, verifier result, final draft version. |
| **Security Boundary** | Prevent leakage of confidential queries to external search where prohibited. |
| **Primary Cost Drivers** | Search loops, long-context synthesis, citation verification, human review time. |
| **Failure Modes** | Citation theater, source laundering, endless search, weak source authority, stale evidence. |
| **Anti-Patterns** | Searching until confident; citing documents the system did not verify. |
| **Degraded Mode** | Present source directory and extracted notes without synthesis. |
| **Adoption and Support** | Users need training on source audit and uncertainty handling. |
| **Sourcing / Exit** | Preserve outputs and source maps in open formats. |
| **Maturity Target** | Level 3–4. |

### **3. Support Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Customer or internal support automation and agent-assist. |
| **Best-Fit Use Cases** | Routine support answers, policy explanations, ticket summarization, suggested replies, triage. |
| **No-Use Conditions** | Emergency services, high-emotion disputes without human path, legal/medical advice, unresolved policy exceptions. |
| **User Surface** | Chat, support widget, CRM agent-assist panel, ticket console. |
| **Architecture Shape** | Message/ticket → intent classifier → account/context retrieval → policy/KB retrieval → answer/draft generation → escalation gate → resolution telemetry. |
| **Required Contracts** | Intent schema, retrieval, grounding, permission, escalation, user expectation, observability, eval. |
| **Human Authority** | Human handles exceptions, low-confidence cases, sensitive accounts, and transactional approvals. |
| **Model Route Strategy** | Fast classifier plus governed generation route; fallback to human queue. |
| **Retrieval / Context Strategy** | Customer/account data through permission filters plus versioned support knowledge. |
| **Tool / Action Strategy** | Read-only by default; write actions require deterministic API and approval where material. |
| **Core Evals** | True resolution, repeat-contact rate, escalation quality, policy compliance, CSAT/sentiment, hallucination rate. |
| **Telemetry** | Intent, source IDs, route ID, escalation reason, resolution status, repeat contact, agent edits. |
| **Audit / Evidence Boundary** | Ticket ID, source policy version, final sent message, escalation packet. |
| **Security Boundary** | Tenant isolation, PII redaction, support-role access control. |
| **Primary Cost Drivers** | Multi-turn context, support volume, human escalation, KB maintenance. |
| **Failure Modes** | Deflection theater, policy hallucination, customer frustration, hidden escalation suppression. |
| **Anti-Patterns** | Trapping users in bot loops; optimizing containment over resolution. |
| **Degraded Mode** | Route to human queue with context handoff and static help links. |
| **Adoption and Support** | Requires support-team training and escalation playbooks. |
| **Sourcing / Exit** | Preserve KB, intent taxonomy, ticket metadata, and correction logs. |
| **Maturity Target** | Level 4–5. |

### **4. Document Intelligence Pipeline Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Structured extraction from unstructured or semi-structured documents. |
| **Best-Fit Use Cases** | Invoices, claims, leases, forms, contracts, financial statements, intake packets. |
| **No-Use Conditions** | Clean API/JSON input, exact deterministic parsing availability, high-speed subsecond transaction need. |
| **User Surface** | Review queue with document preview, field table, coordinates, correction UI. |
| **Architecture Shape** | Document ingest → OCR/layout parser → document classifier → extraction model → schema/semantic validator → exception queue → staging write. |
| **Required Contracts** | Input asset, OCR/layout, schema, semantic, evidence coordinate, human review, write-back, eval. |
| **Human Authority** | Auditors correct low-confidence or high-impact fields before commit. |
| **Model Route Strategy** | Extraction route matched to document class and modality; deterministic validators at edge. |
| **Retrieval / Context Strategy** | Layout, OCR, metadata, document class, field schema; no general RAG unless needed. |
| **Tool / Action Strategy** | No direct production write without validator and review/threshold gate. |
| **Core Evals** | Field-level precision/recall, table extraction, coordinate grounding, schema validity, correction rate. |
| **Telemetry** | Document hash, class, field confidence, validation errors, reviewer corrections, processing time. |
| **Audit / Evidence Boundary** | Source document reference, extracted fields, coordinate/evidence refs, reviewer decision. |
| **Security Boundary** | Ephemeral parsing, malware scanning, PII controls, storage permissions. |
| **Primary Cost Drivers** | OCR/layout, visual tokens, storage, review labor. |
| **Failure Modes** | OCR errors, table misalignment, missing pages, wrong document class, reviewer fatigue. |
| **Anti-Patterns** | Direct ERP writes from unverified extraction; ignoring visual layout. |
| **Degraded Mode** | Manual indexing/review queue. |
| **Adoption and Support** | Requires reviewer training and document-class governance. |
| **Sourcing / Exit** | Preserve schemas and extraction records in portable formats. |
| **Maturity Target** | Level 4. |

### **5. Workflow Automation Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Multi-step workflow execution across systems with bounded side effects. |
| **Best-Fit Use Cases** | Employee onboarding, ticket synchronization, procurement coordination, IT operations runbooks. |
| **No-Use Conditions** | Irreversible actions without approval, ambiguous goals, APIs without idempotency, missing source-of-record verification. |
| **User Surface** | Task dashboard, execution plan, approval checkpoints, state timeline. |
| **Architecture Shape** | Goal → plan graph → policy/permission check → tool execution → source-of-record verification → approval gate → ledger. |
| **Required Contracts** | Plan schema, tool, permission, idempotency, resource, action verification, audit, observability. |
| **Human Authority** | Human approves plan and high-impact steps; can halt, edit, or roll back. |
| **Model Route Strategy** | Reasoning route for planning; deterministic execution harness for actions. |
| **Retrieval / Context Strategy** | API specs, workflow state, policy rules, system-of-record data. |
| **Tool / Action Strategy** | Write-capable only through typed tools with idempotency and postcondition checks. |
| **Core Evals** | Task completion, tool validity, permission denial correctness, postcondition success, loop budget. |
| **Telemetry** | Plan graph, tool call IDs, idempotency keys, verification results, approvals, breaches. |
| **Audit / Evidence Boundary** | Execution ledger, payload hashes, approval records, source-of-record confirmation. |
| **Security Boundary** | Least-privilege tools, sandboxing, no broad admin agent identity. |
| **Primary Cost Drivers** | Planning loops, tool retries, approval latency, integration maintenance. |
| **Failure Modes** | Partial transaction, loop, state divergence, unauthorized action. |
| **Anti-Patterns** | Agent with admin rights; missing idempotency; no postcondition checks. |
| **Degraded Mode** | Freeze state, serialize plan, route to manual operator. |
| **Adoption and Support** | Requires operators trained on execution graphs and exception handling. |
| **Sourcing / Exit** | Keep tool schemas and workflow state portable. |
| **Maturity Target** | Level 4. |

### **6. Coding Agent Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Codebase analysis, patch generation, testing, and pull-request preparation. |
| **Best-Fit Use Cases** | Dependency updates, scaffolding, test generation, routine bug fixes, migrations. |
| **No-Use Conditions** | Untestable systems, critical infrastructure without review, production auto-merge, missing sandbox. |
| **User Surface** | IDE, CLI, issue tracker, pull request, CI dashboard. |
| **Architecture Shape** | Issue/request → repo context selector → plan/diff generation → sandbox build/test → security scan → PR → human review. |
| **Required Contracts** | Repo access, context, patch format, sandbox, test execution, security scan, human review, deployment. |
| **Human Authority** | Developer reviews, edits, approves, and merges. |
| **Model Route Strategy** | Reasoning route for patch planning; cheaper route for log analysis. |
| **Retrieval / Context Strategy** | AST, dependency graph, related files, issue text, tests, build logs. |
| **Tool / Action Strategy** | File edits and commands only in sandbox; merge requires human/CI gate. |
| **Core Evals** | Compile success, test pass, security scan, diff minimality, reviewer acceptance, regression rate. |
| **Telemetry** | Diff hash, build logs, test results, scan findings, reviewer edits, merge status. |
| **Audit / Evidence Boundary** | PR record, commit hashes, test reports, reviewer approval. |
| **Security Boundary** | Network-restricted sandbox; secrets masked; no unreviewed production writes. |
| **Primary Cost Drivers** | Repo context, build/test loops, reasoning tokens. |
| **Failure Modes** | Plausible broken code, test overfitting, vulnerability injection, context poisoning. |
| **Anti-Patterns** | Auto-merge coding agent; writing tests to satisfy broken code. |
| **Degraded Mode** | Suggestion-only mode; disable write/PR creation. |
| **Adoption and Support** | Requires issue-writing discipline and reviewer workflow integration. |
| **Sourcing / Exit** | Store patches and scripts in standard Git workflows. |
| **Maturity Target** | Level 3–4. |

### **7. Analytics Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Governed natural-language analytics, metric explanation, SQL/query generation, and dashboard assistance. |
| **Best-Fit Use Cases** | Ad-hoc business questions, operational reporting, metric exploration, dashboard drafting. |
| **No-Use Conditions** | Regulated filings without deterministic controls, missing semantic metric layer, unrestricted database access. |
| **User Surface** | BI assistant, metric builder, SQL preview, chart explanation panel. |
| **Architecture Shape** | User question → metric/schema selector → permission check → query generator → deterministic validator → read-only execution → visualization/explanation. |
| **Required Contracts** | Semantic metric, schema, permission, SQL/query, resource, provenance, eval, observability. |
| **Human Authority** | Analyst validates metric meaning, query, and chart interpretation. |
| **Model Route Strategy** | Reasoning route for query generation; deterministic validator and metric layer are authoritative. |
| **Retrieval / Context Strategy** | Metric definitions, schema metadata, join rules, row-level policy. |
| **Tool / Action Strategy** | Read-only queries with timeouts, row limits, and query validation. |
| **Core Evals** | SQL validity, metric correctness, row-level security, chart-data consistency, explanation fidelity. |
| **Telemetry** | Question, generated query hash, metric IDs, validation result, query cost, user corrections. |
| **Audit / Evidence Boundary** | Query, metric definition, result reference, chart spec, user approval where needed. |
| **Security Boundary** | Read-only credentials, row-level security, query sandbox/resource limits. |
| **Primary Cost Drivers** | Warehouse compute, complex joins, metadata retrieval, explanation generation. |
| **Failure Modes** | Metric hallucination, wrong joins, unauthorized data exposure, misleading charts. |
| **Anti-Patterns** | Querying raw tables without metric layer; model-generated audit numbers. |
| **Degraded Mode** | Disable ad-hoc query; show verified dashboards and metric glossary. |
| **Adoption and Support** | Requires data literacy and metric-governance alignment. |
| **Sourcing / Exit** | Use portable semantic-layer definitions and SQL artifacts. |
| **Maturity Target** | Level 4. |

### **8. Multimodal Review System Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Review and audit of images, audio, video, scanned documents, or spatial/temporal evidence. |
| **Best-Fit Use Cases** | Site inspections, media policy review, brand compliance, real-estate review, document-image validation. |
| **No-Use Conditions** | Clinical/safety-critical diagnosis without regulated validation, poor media quality, no expert review path. |
| **User Surface** | Timeline, media canvas, bounding boxes, transcript, annotation review panel. |
| **Architecture Shape** | Media ingest → preprocessing/sampling → modality route → multimodal analysis → coordinate/timecode mapping → expert review → record. |
| **Required Contracts** | Asset schema, modality, sampling, coordinate/timecode, detection label, human review, audit, eval. |
| **Human Authority** | Domain expert confirms or corrects detections and final judgment. |
| **Model Route Strategy** | Multimodal route selected by asset type and evidence requirements. |
| **Retrieval / Context Strategy** | Metadata, checklists, prior reference images, transcripts where relevant. |
| **Tool / Action Strategy** | Media parsers/samplers in sandbox; no autonomous final decision in high-risk cases. |
| **Core Evals** | Detection precision/recall, coordinate grounding, transcription accuracy, reviewer agreement, false-negative rate. |
| **Telemetry** | Media hash, sampling settings, detection labels, coordinates/timecodes, reviewer adjustments. |
| **Audit / Evidence Boundary** | Media reference, annotation IDs, reviewer decision, coordinate/timecode evidence. |
| **Security Boundary** | Media sandbox, codec exploit protection, PII redaction/blurring where required. |
| **Primary Cost Drivers** | Video/audio processing, visual tokens, storage, expert review. |
| **Failure Modes** | Missed anomalies, timecode drift, bounding errors, transcript hallucination, reviewer fatigue. |
| **Anti-Patterns** | Generic visual descriptions with no coordinate evidence. |
| **Degraded Mode** | Disable overlays; present raw media and manual checklist. |
| **Adoption and Support** | Requires expert review training and annotation standards. |
| **Sourcing / Exit** | Store annotations in open schema with media references. |
| **Maturity Target** | Level 3–4. |

### **9. Enterprise Knowledge System Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Governed enterprise knowledge retrieval and synthesis across permissioned repositories. |
| **Best-Fit Use Cases** | Policy search, internal documentation, compliance research, support grounding, product knowledge. |
| **No-Use Conditions** | Exact transactional lookup, unmanaged corpora, missing ACLs, no content lifecycle. |
| **User Surface** | Search portal, chat/search UI, document preview, citation panel. |
| **Architecture Shape** | Repository sync → ACL processing → chunk/index → hybrid search/rerank → grounding verifier → answer synthesis → feedback loop. |
| **Required Contracts** | Ingestion, permission, retrieval, freshness, grounding, citation, lifecycle, eval, observability. |
| **Human Authority** | Content owners manage source quality; users verify cited answers. |
| **Model Route Strategy** | Retrieval-first; synthesis route only after evidence is permissioned and sufficient. |
| **Retrieval / Context Strategy** | Hybrid retrieval with ACL/RLS, source authority, freshness, dedupe, conflict handling. |
| **Tool / Action Strategy** | Read-only retrieval; no document mutation unless separately governed. |
| **Core Evals** | Context precision/recall, answer faithfulness, citation support, permission safety, freshness. |
| **Telemetry** | Query, user/role scope reference, source IDs, retrieval rank, citation verification, feedback. |
| **Audit / Evidence Boundary** | Source refs, answer version, citation verifier status, access-decision record. |
| **Security Boundary** | Chunk-level permissions, tenant isolation, restricted corpus ingestion. |
| **Primary Cost Drivers** | Ingestion, embeddings/indexes, reranking, storage, source lifecycle. |
| **Failure Modes** | Permission leakage, stale summaries, duplicate/conflicting docs, vector dump chaos. |
| **Anti-Patterns** | Vector Dump Knowledge System; RAG-as-database. |
| **Degraded Mode** | Keyword/file search with document links; no synthesis. |
| **Adoption and Support** | Requires knowledge management and content-owner workflows. |
| **Sourcing / Exit** | Preserve source documents, metadata, and index rebuild path. |
| **Maturity Target** | Level 4. |

### **10. Decision-Support Cockpit Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | High-stakes evidence review, scenario analysis, and human decision support. |
| **Best-Fit Use Cases** | Underwriting, legal strategy, healthcare support, risk review, supply-chain contingency planning. |
| **No-Use Conditions** | Autonomous final decision, high-volume low-review tasks, no expert validation path. |
| **User Surface** | Evidence cockpit, scenario matrix, risk panel, rationale capture. |
| **Architecture Shape** | Case file → evidence gathering → policy/guideline retrieval → scenario generation → risk/uncertainty analysis → human decision record. |
| **Required Contracts** | Case assembly, retrieval, grounding, risk, human review, rationale, audit, user expectation. |
| **Human Authority** | Human is final decision-maker; AI frames options and evidence only. |
| **Model Route Strategy** | High-reasoning route for scenario framing; deterministic calculators for numbers. |
| **Retrieval / Context Strategy** | Case record, policies, historical precedents, external evidence where approved. |
| **Tool / Action Strategy** | Simulations/read-only analysis; external writes require human approval and deterministic execution. |
| **Core Evals** | Evidence completeness, option relevance, risk coverage, calibration, rationale quality, bias review. |
| **Telemetry** | Case ID, evidence refs, scenarios generated, user choice, rationale, override/correction. |
| **Audit / Evidence Boundary** | Decision record, evidence refs, human rationale, versioned policy sources. |
| **Security Boundary** | Regulated workspace, role access, strict retention and evidence policy. |
| **Primary Cost Drivers** | Expert review, long-case context, reasoning, audit requirements. |
| **Failure Modes** | Automation bias, biased framing, missing risk, information overload. |
| **Anti-Patterns** | Human as rubber-stamp for automated decision. |
| **Degraded Mode** | Static checklist and raw evidence package. |
| **Adoption and Support** | Requires training on automation bias and evidence inspection. |
| **Sourcing / Exit** | Preserve case/rationale records and scenario schemas. |
| **Maturity Target** | Level 4. |

### **11. Background Classifier / Router Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | High-throughput classification, triage, and routing. |
| **Best-Fit Use Cases** | Ticket routing, alert triage, anomaly tagging, document category routing, moderation queues. |
| **No-Use Conditions** | High-liability decisions without review, deterministic metadata routing, low-volume tasks. |
| **User Surface** | Mostly headless; exception and audit dashboard. |
| **Architecture Shape** | Event ingest → feature extraction → classifier → confidence/threshold gate → deterministic router → exception queue. |
| **Required Contracts** | Event schema, class taxonomy, confidence threshold, exception queue, route/action schema, eval, observability. |
| **Human Authority** | Queue managers review exceptions and taxonomy drift. |
| **Model Route Strategy** | Fast classifier route; fallback to deterministic rules or manual queue. |
| **Retrieval / Context Strategy** | Taxonomy, route definitions, metadata, short event body. |
| **Tool / Action Strategy** | Queue writes only; high-impact outcomes require human review. |
| **Core Evals** | Precision/recall, confusion matrix, false-negative cost, drift, exception rate. |
| **Telemetry** | Event hash, class, confidence bucket, route target, exception reason, correction. |
| **Audit / Evidence Boundary** | Event reference, class label, route decision, taxonomy version. |
| **Security Boundary** | Input sanitization, queue permission, no arbitrary payload execution. |
| **Primary Cost Drivers** | Event volume, classification calls, exception labor. |
| **Failure Modes** | Silent misrouting, class drift, queue overload, payload manipulation. |
| **Anti-Patterns** | No exception queue; automating high-liability routing. |
| **Degraded Mode** | Route all events to manual triage or deterministic rules. |
| **Adoption and Support** | Requires taxonomy ownership and queue SLA review. |
| **Sourcing / Exit** | Preserve class taxonomy and labeled examples. |
| **Maturity Target** | Level 4–5. |

### **12. Personal or Team Productivity Assistant Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Local or team-scoped drafting, summarization, note search, and lightweight assistance. |
| **Best-Fit Use Cases** | Meeting notes, email drafts, personal knowledge search, team document drafting. |
| **No-Use Conditions** | System-of-record writes, regulated workflows, customer-facing automation, enterprise-wide authority. |
| **User Surface** | Sidebar, desktop widget, team chat assistant, document pane. |
| **Architecture Shape** | User prompt → local/team context assembly → safety/policy filter → optional memory/retrieval → model route → user review/copy. |
| **Required Contracts** | Prompt, context, memory where active, resource, user expectation, observability. |
| **Human Authority** | User owns final copy/send/paste/action. |
| **Model Route Strategy** | Low/medium capability route; local/private route for sensitive contexts where needed. |
| **Retrieval / Context Strategy** | Local notes, team docs, current workspace, permissioned memory. |
| **Tool / Action Strategy** | No direct external execution unless separately governed. |
| **Core Evals** | User utility, memory relevance, formatting, safety policy, latency. |
| **Telemetry** | Minimal usage/correction signals; avoid surveillance-style individual monitoring. |
| **Audit / Evidence Boundary** | Usually none beyond operational telemetry unless enterprise data/risk requires. |
| **Security Boundary** | Protect notes, credentials, local files, and team permissions. |
| **Primary Cost Drivers** | Frequent low-value calls, local indexes, memory storage. |
| **Failure Modes** | Memory drift, hallucinated summaries, context leakage, overcollection. |
| **Anti-Patterns** | Mixing team databases without permission boundaries. |
| **Degraded Mode** | Standard editor/search without AI. |
| **Adoption and Support** | Basic training on data boundaries and review. |
| **Sourcing / Exit** | Export notes/memory/context indexes where appropriate. |
| **Maturity Target** | Level 2–3, higher if enterprise-governed. |

### **13. Governed Agentic Workflow Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Complex, stateful, multi-step agentic workflow with governance and checkpoints. |
| **Best-Fit Use Cases** | Compliance auditing, multi-step underwriting support, controlled security review, complex operational routing. |
| **No-Use Conditions** | Linear deterministic workflow, subsecond latency, missing rollback, missing schema/state model. |
| **User Surface** | Process portal, graph visualization, checkpoint ledger, approval queue. |
| **Architecture Shape** | Goal → graph/state machine → planner/executor/auditor nodes → checkpoint ledger → deterministic gate → human escalation. |
| **Required Contracts** | Graph, prompt, context, retrieval, tool, permission, resource, memory, checkpoint, eval, observability, audit. |
| **Human Authority** | Process owner approves plan, handles escalations, can halt/rollback. |
| **Model Route Strategy** | Reasoning route for planning/auditing; deterministic state machine controls execution. |
| **Retrieval / Context Strategy** | Node-specific context, policies, APIs, state, prior checkpoints. |
| **Tool / Action Strategy** | Tool calls gated by node contract, idempotency, permission, and postcondition checks. |
| **Core Evals** | Node transition correctness, loop/budget compliance, task completion, rollback, checkpoint replay. |
| **Telemetry** | Graph path, node states, tool calls, approvals, resource use, breaches. |
| **Audit / Evidence Boundary** | Checkpoints, payload hashes, approval records, state transitions, final confirmation. |
| **Security Boundary** | Isolated node execution, scoped credentials, no cross-agent privilege leakage. |
| **Primary Cost Drivers** | Multi-agent loops, long contexts, checkpoints, human escalation. |
| **Failure Modes** | Consensus deadlock, runaway graph, state divergence, tool abuse. |
| **Anti-Patterns** | Multi-agent framework for a simple linear workflow. |
| **Degraded Mode** | Pause graph, serialize state, route to process supervisor. |
| **Adoption and Support** | Requires operator training on graph debugging and escalation. |
| **Sourcing / Exit** | Keep graph definitions, state, tools, and checkpoints portable. |
| **Maturity Target** | Level 4. |

### **14. AI Gateway / Control Plane Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Central model access, routing, policy, observability, quota, budget, and provider abstraction. |
| **Best-Fit Use Cases** | Enterprise model access, multi-provider routing, cost control, credentials management, policy enforcement. |
| **No-Use Conditions** | Local throwaway prototype with no enterprise data, no shared users, and no production route. |
| **User Surface** | Developer API, platform dashboard, admin console. |
| **Architecture Shape** | App request → identity/policy/quota → cache where safe → route selection → provider/self-hosted model → validation/telemetry. |
| **Required Contracts** | Route, permission, resource, vendor, observability, deployment, eval, sourcing. |
| **Human Authority** | Platform team controls routes, keys, budgets, policy, and emergency shutdown. |
| **Model Route Strategy** | Provider-neutral route aliases tied to task/risk/eval contracts. |
| **Retrieval / Context Strategy** | Optional cache/retrieval only with tenant, freshness, and permission scope. |
| **Tool / Action Strategy** | Usually no direct business action; may broker tool calls through separate contracts. |
| **Core Evals** | Gateway latency, route correctness, policy block correctness, cost control, provider failover. |
| **Telemetry** | Route ID, contract versions, tokens, cost, latency, policy decisions, errors, breaches. |
| **Audit / Evidence Boundary** | Route manifest, vendor contract refs, policy decisions, incident events. |
| **Security Boundary** | Key vault, egress controls, tenant isolation, DLP/policy before provider call. |
| **Primary Cost Drivers** | High-volume proxying, logs/traces, caching, provider calls. |
| **Failure Modes** | Single point of failure, gateway bypass, cache leakage, policy misroute. |
| **Anti-Patterns** | Direct provider SDK sprawl; gateway bypass. |
| **Degraded Mode** | Fail closed or use approved fallback routes preserving contracts. |
| **Adoption and Support** | Requires developer onboarding and platform support. |
| **Sourcing / Exit** | Central mechanism for provider exit and route migration. |
| **Maturity Target** | Level 5. |

### **15. Human Review and Escalation Queue Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Exception handling, human validation, approval, and correction capture. |
| **Best-Fit Use Cases** | Low-confidence extraction, high-risk actions, support escalations, moderation, regulated review. |
| **No-Use Conditions** | Very low-risk high-volume tasks with reliable deterministic validation, or no reviewer capacity. |
| **User Surface** | Review queue, evidence panel, correction editor, approval/deny controls. |
| **Architecture Shape** | AI proposal → escalation gate → priority queue → review canvas → human decision → deterministic validator → downstream commit or rejection. |
| **Required Contracts** | Escalation, human review, evidence, schema, permission, audit, observability. |
| **Human Authority** | Reviewer can approve, reject, correct, escalate, or block. |
| **Model Route Strategy** | Use AI to pre-process and prioritize; do not rely on high-cost retries to avoid review. |
| **Retrieval / Context Strategy** | Evidence packet, source refs, policy, prior corrections. |
| **Tool / Action Strategy** | Human-approved payload goes through deterministic validator and action contract. |
| **Core Evals** | Reviewer agreement, false negatives, correction categories, queue latency, fatigue indicators. |
| **Telemetry** | Queue age, reviewer action, correction delta, approval/rejection, downstream result. |
| **Audit / Evidence Boundary** | Proposal, evidence refs, reviewer ID/role, payload hash, decision reason. |
| **Security Boundary** | Reviewer RBAC, sensitive data minimization, queue access logging. |
| **Primary Cost Drivers** | Skilled review labor, queue tooling, evidence prep. |
| **Failure Modes** | Rubber-stamping, backlog, reviewer fatigue, feedback loss. |
| **Anti-Patterns** | Fake human-in-the-loop with no evidence or veto. |
| **Degraded Mode** | Pause automated writes; hold items in staging queue. |
| **Adoption and Support** | Requires reviewer training and SLA ownership. |
| **Sourcing / Exit** | Keep review records exportable. |
| **Maturity Target** | Level 4. |

### **16. Evaluation and Shadow-Mode Pattern**

| Field | Specification |
| :---- | :---- |
| **Problem Class** | Candidate model, prompt, route, retrieval, or tool validation before production exposure. |
| **Best-Fit Use Cases** | Model upgrades, prompt migrations, retrieval changes, route comparison, regression detection. |
| **No-Use Conditions** | Production data cannot be mirrored safely, no validation metrics exist, or candidate path may create side effects. |
| **User Surface** | Release dashboard, eval report, regression matrix. |
| **Architecture Shape** | Production sample or mirrored request → candidate route in isolated mode → evaluator → regression detector → release decision. |
| **Required Contracts** | Eval, route, deployment, observability, data handling, evidence, security. |
| **Human Authority** | Release owner approves rollout based on evidence. |
| **Model Route Strategy** | Candidate route isolated from production side effects. |
| **Retrieval / Context Strategy** | Mirrors retrieval where allowed; otherwise uses representative offline dataset. |
| **Tool / Action Strategy** | Read-only or mocked writes; no candidate side effects. |
| **Core Evals** | Quality delta, regression rate, safety, latency, cost, grounding, tool-call validity. |
| **Telemetry** | Candidate output, eval score, cost/latency delta, data class, route IDs. |
| **Audit / Evidence Boundary** | Eval report, dataset/sample version, manifest, approval decision. |
| **Security Boundary** | Shadow data must match production policy; candidate path isolated. |
| **Primary Cost Drivers** | Duplicate calls, evaluator cost, trace storage, review time. |
| **Failure Modes** | Evaluator bias, shadow leakage, candidate side effects, false confidence. |
| **Anti-Patterns** | Production upgrade without shadow/eval evidence. |
| **Degraded Mode** | Disable candidate path; production path unaffected. |
| **Adoption and Support** | Requires release discipline and eval ownership. |
| **Sourcing / Exit** | Enables provider/model migration decisions. |
| **Maturity Target** | Level 4–5. |

## **Systemic Integration and Control Surfaces**

Patterns become production architectures only when their controls are explicit. The matrices below define required contract packs, evaluation classes, telemetry/evidence boundaries, human authority, security boundaries, and cost drivers.

### **1. Pattern Control Pack Matrix**

| Pattern | Mandatory Control Pack |
| :---- | :---- |
| **Copilot / Embedded Assistant** | Prompt, context, route, resource, user expectation, observability. |
| **Research Agent** | Prompt, retrieval, grounding, source authority, freshness, resource, eval, observability. |
| **Support Assistant** | Intent, retrieval, grounding, escalation, permission, user expectation, support telemetry. |
| **Document Intelligence** | Asset, OCR/layout, schema, evidence coordinate, validation, review queue, audit. |
| **Workflow Automation Agent** | Plan, tool, permission, idempotency, resource, action verification, ledger. |
| **Coding Agent** | Repo access, sandbox, patch, test, security scan, human review, CI gate. |
| **Analytics Assistant** | Semantic metric, schema, permission, query validation, read-only execution, provenance. |
| **Multimodal Review** | Asset, modality, coordinate/timecode, detection label, expert review, audit. |
| **Enterprise Knowledge System** | Ingestion, ACL, retrieval, freshness, grounding, citation, lifecycle, eval. |
| **Decision-Support Cockpit** | Case assembly, evidence, scenario, risk, human decision, rationale, audit. |
| **Background Classifier / Router** | Event schema, taxonomy, confidence threshold, exception queue, route validation. |
| **Productivity Assistant** | Prompt, local/team context, memory if active, resource, user expectation. |
| **Governed Agentic Workflow** | Graph, tool, permission, memory, resource, checkpoint, action verification, audit. |
| **AI Gateway / Control Plane** | Route, vendor, permission, resource, deployment, observability, eval, policy. |
| **Human Review Queue** | Escalation, evidence, reviewer role, override, audit, downstream validation. |
| **Evaluation / Shadow Mode** | Eval, route, sample/data policy, candidate isolation, deployment, evidence. |

### **2. Eval-by-Pattern Matrix**

| Pattern | Core Evaluation Classes |
| :---- | :---- |
| **Copilot** | Suggestion usefulness, edit distance, latency, acceptance quality, downstream correctness. |
| **Research Agent** | Citation fidelity, claim support, source authority, contradiction handling, synthesis usefulness. |
| **Support Assistant** | True resolution, repeat contact, policy compliance, escalation quality, CSAT/sentiment. |
| **Document Intelligence** | Field precision/recall, schema validity, table extraction, coordinate grounding, correction rate. |
| **Workflow Automation Agent** | Plan validity, tool-call validity, postcondition success, rollback/compensation, loop budget. |
| **Coding Agent** | Compile/test pass, static scan, diff minimality, reviewer approval, regression rate. |
| **Analytics Assistant** | SQL/query validity, metric correctness, row-level security, chart consistency, explanation fidelity. |
| **Multimodal Review** | Detection precision/recall, coordinate/timecode grounding, transcript accuracy, reviewer agreement. |
| **Enterprise Knowledge System** | Context precision/recall, faithfulness, citation support, permission safety, freshness. |
| **Decision-Support Cockpit** | Evidence completeness, risk coverage, option relevance, calibration, rationale quality. |
| **Background Classifier / Router** | Precision/recall, confusion matrix, false-negative cost, drift, exception rate. |
| **Productivity Assistant** | Utility, memory relevance, safety policy, latency, user correction. |
| **Governed Agentic Workflow** | Node transition correctness, graph safety, checkpoint replay, tool verification, budget compliance. |
| **AI Gateway** | Route correctness, policy enforcement, latency overhead, cost control, failover safety. |
| **Human Review Queue** | Reviewer agreement, queue latency, fatigue, false negatives, downstream correction. |
| **Evaluation / Shadow Mode** | Candidate delta, regression, cost/latency delta, evaluator reliability, release decision quality. |

### **3. Telemetry and Evidence Boundary Matrix**

| Pattern | Operational Telemetry | Evidence / Audit Boundary |
| :---- | :---- | :---- |
| **Copilot** | Accept/reject/edit, latency, route, cost bucket. | Usually none beyond hashes/versions unless regulated. |
| **Research Agent** | Plan, source IDs, verifier status, loop count, cost. | Source manifest, claim-evidence map, final report version. |
| **Support Assistant** | Intent, source IDs, escalation reason, resolution, repeat contact. | Ticket record, final message, policy version, handoff packet. |
| **Document Intelligence** | Field confidence, validation errors, correction, runtime. | Document ref, extracted fields, coordinate refs, reviewer decision. |
| **Workflow Automation Agent** | Plan graph, tool calls, postconditions, approvals, breaches. | Execution ledger, payload hashes, approvals, state confirmation. |
| **Coding Agent** | Diff, build/test, scan, reviewer edits. | PR, commit hash, test report, reviewer approval. |
| **Analytics Assistant** | Query hash, metric IDs, validation, cost, corrections. | Query, metric definition, result ref, chart spec. |
| **Multimodal Review** | Media hash, sampling, detections, reviewer edits. | Media ref, annotation IDs, coordinates/timecodes, final decision. |
| **Enterprise Knowledge System** | Query, retrieval rank, source IDs, citation verification. | Answer version, source refs, access decision, verifier status. |
| **Decision-Support Cockpit** | Evidence refs, options, choice, rationale, overrides. | Decision record, evidence packet, rationale, policy version. |
| **Background Classifier** | Event hash, class, confidence, route, exception. | Event ref, taxonomy version, routing decision. |
| **Productivity Assistant** | Minimal utility/correction/latency signals. | Usually none unless enterprise risk requires. |
| **Governed Agentic Workflow** | Graph state, node events, tool calls, resource use, breaches. | Checkpoints, payload hashes, approvals, final confirmation. |
| **AI Gateway** | Route, tokens, cost, latency, policy decision, errors. | Route manifest, policy version, vendor/SLA event, incident record. |
| **Human Review Queue** | Queue age, reviewer action, correction, downstream result. | Proposal, evidence refs, reviewer decision, payload hash. |
| **Evaluation / Shadow Mode** | Candidate output, eval score, cost/latency delta. | Eval report, sample version, manifest, release approval. |

Telemetry supports operation. Evidence supports proof. Do not turn raw telemetry into an uncontrolled audit landfill.

### **4. Human Authority Matrix**

| Pattern | Human Authority Level |
| :---- | :---- |
| **Copilot** | Human accepts, edits, or rejects suggestions. |
| **Research Agent** | Human directs research, audits sources, approves synthesis. |
| **Support Assistant** | Human handles escalations and material account actions. |
| **Document Intelligence** | Human reviews low-confidence/high-impact fields. |
| **Workflow Automation Agent** | Human approves plans and high-impact steps. |
| **Coding Agent** | Human reviews PR and controls merge. |
| **Analytics Assistant** | Human validates metric interpretation and query output. |
| **Multimodal Review** | Expert confirms detections and final assessment. |
| **Enterprise Knowledge System** | Human verifies cited answers and content owners maintain sources. |
| **Decision-Support Cockpit** | Human is final decision-maker. |
| **Background Classifier** | Human audits exceptions and taxonomy drift. |
| **Productivity Assistant** | User owns final use of generated text. |
| **Governed Agentic Workflow** | Human can halt, approve, rollback, or escalate graph execution. |
| **AI Gateway** | Platform owner controls route, policy, budget, and shutdown. |
| **Human Review Queue** | Reviewer has veto/correction authority. |
| **Evaluation / Shadow Mode** | Release owner approves production promotion. |

### **5. Security Boundary Matrix**

| Pattern | Boundary Requirement |
| :---- | :---- |
| **Copilot** | Workspace context scoping, secret masking, local/cloud data policy. |
| **Research Agent** | Read-only tools, external-query controls, sandboxed browsing/parsing. |
| **Support Assistant** | Tenant isolation, PII redaction, CRM permission scope, escalation path. |
| **Document Intelligence** | File sandboxing, malware/PDF safety, PII controls, staging writes. |
| **Workflow Automation Agent** | Least-privilege tools, idempotency, postcondition checks, no admin identity. |
| **Coding Agent** | Network-restricted sandbox, secret masking, CI gate, human merge. |
| **Analytics Assistant** | Read-only credentials, row-level security, query limits, semantic layer. |
| **Multimodal Review** | Media sandbox, codec protection, PII blur/redaction where required. |
| **Enterprise Knowledge System** | Chunk-level ACL/RLS, tenant isolation, source lifecycle governance. |
| **Decision-Support Cockpit** | Regulated workspace, strict access, evidence retention policy. |
| **Background Classifier** | Input sanitization, queue permission, exception path. |
| **Productivity Assistant** | Local file and memory controls, no silent enterprise data upload. |
| **Governed Agentic Workflow** | Isolated node execution, scoped credentials, checkpointed state. |
| **AI Gateway** | Key vault, route policy, DLP/egress, tenant separation, gateway HA. |
| **Human Review Queue** | Reviewer RBAC, sensitive-data minimization, audit access controls. |
| **Evaluation / Shadow Mode** | Candidate isolation, no side effects, mirrored-data policy. |

### **6. Cost Driver and Mitigation Matrix**

| Pattern | Primary Cost Driver | Structural Mitigation |
| :---- | :---- | :---- |
| **Copilot** | High-frequency completions. | Debounce, local cache, short context, small route. |
| **Research Agent** | Search/synthesis loops. | Step budget, plan approval, source cache, bounded search. |
| **Support Assistant** | Long conversations and escalations. | Summarization, intent routing, KB quality, escalation thresholds. |
| **Document Intelligence** | OCR/visual processing and review labor. | Page filtering, document classification, field-confidence gates. |
| **Workflow Automation Agent** | Planning/tool loops and retries. | Static workflow templates, step caps, idempotency, approval gates. |
| **Coding Agent** | Build/test/retry loops. | Changed-file builds, sandbox reuse, issue scoping. |
| **Analytics Assistant** | Warehouse compute and bad joins. | Metric layer, query limits, dry-run cost estimates. |
| **Multimodal Review** | Media processing and expert review. | Sampling policy, preprocessing, exception thresholds. |
| **Enterprise Knowledge System** | Ingestion/index/reranking. | Deduplication, lifecycle cleanup, hybrid retrieval tuning. |
| **Decision-Support Cockpit** | Long reasoning and expert review. | Evidence templates, deterministic calculators, scenario caps. |
| **Background Classifier** | Event volume and exceptions. | Fast classifier, batching, taxonomy quality. |
| **Productivity Assistant** | Frequent low-value calls. | Local route, caching, memory limits. |
| **Governed Agentic Workflow** | Multi-agent loops and checkpoints. | Graph constraints, budget caps, early human intervention. |
| **AI Gateway** | Proxy scale, telemetry, cache, provider calls. | Sampling, budget enforcement, route optimization. |
| **Human Review Queue** | Reviewer labor. | Better thresholds, prioritization, evidence UI, workflow redesign. |
| **Evaluation / Shadow Mode** | Duplicate inference and eval cost. | Sampling, offline evals, candidate gating. |

## **Degraded Mode Pattern Library**

Degraded modes are safe lower-capability states. They must preserve the system’s safety contracts even when capability, provider availability, retrieval, tools, or latency degrade.

| Pattern | Trigger | Safe Degraded Behavior | Disabled Capability | Disclosure / Recovery |
| :---- | :---- | :---- | :---- | :---- |
| **Copilot / Embedded Assistant** | Model latency, provider outage, context unsafe. | Static snippets, deterministic autocomplete, local templates. | Generative suggestions. | Show reduced-assist status; restore after route health and policy pass. |
| **Research Agent** | Search loop budget, source verifier failure, provider outage. | Present source list, notes, and unresolved questions without synthesis. | Final synthesized answer. | Show unsupported/partial status; recover after evidence verification. |
| **Support Assistant** | Low confidence, policy retrieval failure, customer distress, outage. | Route to human queue with conversation summary and known context. | Bot resolution and transactional actions. | Tell user escalation occurred; recover after support route healthy. |
| **Document Intelligence** | OCR/parser failure, schema failure, low confidence, malware flag. | Manual document review queue. | Automated extraction/write-back. | Mark document needs review; recover after parser/eval pass. |
| **Workflow Automation Agent** | Permission failure, loop budget, unknown final state, API outage. | Freeze workflow, serialize state, notify operator. | Further side effects. | Show paused state and required human action. |
| **Coding Agent** | Sandbox failure, test failure, security scan failure. | Suggestion-only analysis with no PR/write authority. | Automated patch/PR creation. | Show failed gate; recover after tests/sandbox pass. |
| **Analytics Assistant** | Query validator failure, warehouse limit, metric ambiguity. | Static dashboards, metric glossary, or dry-run query only. | Ad-hoc execution. | Explain query blocked; recover after metric/query validation. |
| **Multimodal Review System** | Media parser failure, detection uncertainty, coordinate verifier failure. | Raw media review with manual checklist. | Automated annotations/final assessment. | Mark automation unavailable; recover after media/eval pass. |
| **Enterprise Knowledge System** | Retrieval permission uncertainty, index outage, citation verifier failure. | Keyword search/document list only. | Generated answers/summaries. | Show synthesis disabled; recover after retrieval/grounding healthy. |
| **Decision-Support Cockpit** | Evidence conflict, missing source, high uncertainty, route failure. | Static checklist and raw evidence packet. | Scenario synthesis/recommendation. | Require human decision; recover after evidence sufficiency. |
| **Background Classifier / Router** | Classifier drift, low confidence, queue route failure. | Manual triage queue or deterministic rules. | Automated routing. | Alert queue owner; recover after taxonomy/eval pass. |
| **Personal / Team Productivity Assistant** | Memory unsafe, context too sensitive, local route unavailable. | Standard editor/search without AI. | Memory use and generation. | Show AI unavailable/restricted; recover after policy/route pass. |
| **Governed Agentic Workflow** | Graph deadlock, contract breach, tool failure, resource budget. | Pause graph and checkpoint state. | Node advancement and side effects. | Notify process supervisor; recover by manual resume or rollback. |
| **AI Gateway / Control Plane** | Provider outage, policy failure, key compromise, routing anomaly. | Fail closed or use only approved fallback routes preserving contracts. | Unsafe provider routes and direct bypass. | Alert platform; recover after route/policy health check. |
| **Human Review Queue** | Reviewer overload, queue tooling failure, evidence missing. | Hold items in staging; pause downstream writes. | Approval/commit. | Notify operations; recover after queue capacity/evidence restored. |
| **Evaluation / Shadow Mode** | Shadow cost spike, candidate failure, data policy violation. | Disable candidate path; production path remains isolated. | Candidate evaluation traffic. | Alert release owner; recover after shadow policy/eval fix. |

A degraded mode is not “use a weaker model and hope.” It is a controlled reduction in capability that preserves safety, authority, and evidence boundaries.

## **AI Architecture Anti-Pattern Catalog**

### **Chatbox on Everything**

* **Symptoms**: A single, open-ended conversational chat input area is deployed as the primary user interface, replacing structured web forms, nested menus, action tables, and application buttons.  
* **Why Tempting**: Fast to build and deploy; simplifies UI design; shifts the burden of navigating process complexity directly to the user's phrasing.  
* **Why It Fails**: Increases cognitive load on users, who must guess which inputs are valid; leads to high task friction; and yields highly inconsistent completions where deterministic data writes are required.1  
* **Detection Signals**: Low user engagement metrics; high frequencies of multi-turn user explanation queries; user feedback complaining about lost dashboard features.  
* **Safer Alternatives**: Use the Copilot pattern, embedding task-specific context suggestions inline within structured user interfaces with explicit, validated operation buttons.13

### **RAG-as-Database**

* **Symptoms**: A vector database and Retrieval-Augmented Generation (RAG) pipeline are utilized to perform precise, single-value transactional lookups (e.g., account balances, order status tracking, inventory quantities).  
* **Why Tempting**: Avoids the engineering effort of building relational database indexes or structured REST APIs, allowing developers to query un-parsed files directly via semantic similarity.  
* **Why It Fails**: Vector similarity models do not guarantee exact factual retrievals; models frequently miss target numbers or synthesize adjacent data fields.7  
* **Detection Signals**: Numeric errors in customer responses; low context precision metrics; database retrieval mismatches.  
* **Safer Alternatives**: Use deterministic lookup paths (SQL indices, REST endpoints) for structured data queries, and reserve RAG patterns strictly for unstructured document search.

### **Agent as Intern with Admin Rights**

* **Symptoms**: An autonomous, model-driven agent is deployed to write directly to enterprise production systems of record (e.g., modifying ERPs, updating cloud server states) without sandbox boundaries or human approval gates.1  
* **Why Tempting**: Promises high automation speeds and creates the appearance of a fully automated operational process.  
* **Why It Fails**: Non-deterministic planner behaviors, tool call hallucinations, or prompt injections can trigger unauthorized database deletions, infinite transaction loops, and data corruption.1  
* **Detection Signals**: Corrupt production databases; billing anomalies; security alerts indicating unauthorized administrative API executions.  
* **Safer Alternatives**: Deploy the Governed Agentic Workflow pattern with sandboxed transaction runtimes and strict human verification gates.1

### **Demo Architecture**

* **Symptoms**: A prototype pipeline using complex multi-agent frameworks is deployed to production with the same configurations used during local development, lacking telemetry logging, error routing, and tracing configurations.2  
* **Why Tempting**: Accelerates initial deployment schedules; minimizes system setup and platform management workloads.  
* **Why It Fails**: Production data exposes systemic anomalies, rate-limiting failures, token budget overruns, and security exploits that local testing environments hide.2  
* **Detection Signals**: Uncontrollable API spend; system timeout errors; untraceable transactional failures; lack of structured logging.  
* **Safer Alternatives**: Implement the AI Gateway pattern to manage credentials, rate-limiting parameters, and open telemetry collections across providers.

### **Citation Theater**

* **Symptoms**: Generated documents feature academic-style footnotes and hyperlink annotations that point to irrelevant sources, hallucinated files, or non-existent document subsections.1  
* **Why Tempting**: Footnotes look professional and create the appearance of factual grounding and research reliability.  
* **Why It Fails**: Undermines system trust; users can be misled by plausible-sounding synthetic text backed by hallucinated references.1  
* **Detection Signals**: User feedback flagging broken links; low citation fidelity evaluation scores; automated grounding mismatches.  
* **Safer Alternatives**: Deploy a dedicated Citation Verifier pipeline to validate visual coordinate maps and link offsets before rendering documents.

### **Workflow Double-Work**

* **Symptoms**: Users spend more time reviewing, correcting, and re-verifying model-generated drafts than they would have spent writing the document manually.  
* **Why Tempting**: Promises high-volume automated document drafts at low upfront cost.  
* **Why It Fails**: High-volume, low-quality generations introduce long correction workloads, causing operator fatigue and reducing productivity.  
* **Detection Signals**: High user reject rates; low adoption metrics; longer document processing timelines.  
* **Safer Alternatives**: Deploy bounded Copilot models designed to suggest local completions inline, rather than generating entire complex reports at once.13

### **Model Leaderboard Architecture**

* **Symptoms**: Selecting a core model backend based purely on generic public benchmarks (e.g., MMLU scores) rather than evaluating performance against custom task data.  
* **Why Tempting**: Avoids the operational cost of building local evaluation test suites and benchmark datasets.  
* **Why It Fails**: Public benchmarks rarely reflect specific corporate task parameters, schema requirements, or document formatting realities.  
* **Detection Signals**: Upgraded models underperforming on custom corporate tasks despite higher public benchmark scores.  
* **Safer Alternatives**: Build localized evaluation datasets and regression gates based on production telemetry to validate model upgrades.

### **Single-Provider Hardwire**

* **Symptoms**: Directly referencing vendor-specific SDK libraries and proprietary API payload formats throughout system controller files, without decoupling layers.  
* **Why Tempting**: Simplifies initial integration; leverages vendor-specific helper utilities.  
* **Why It Fails**: Locks the enterprise into a single model provider; introduces operational risk during outages or API deprecations.6  
* **Detection Signals**: Long refactoring timelines during model migration projects; systemic outages when a primary model provider experiences downtime.  
* **Safer Alternatives**: Implement the AI Gateway pattern using standard API specifications to decouple application controllers from provider endpoints.9

### **Unbounded Research Goblin**

* **Symptoms**: Deploying research agents without step execution boundaries, model timeout constraints, or spending ceilings, allowing models to query APIs indefinitely in search of answers.1  
* **Why Tempting**: Promises deep, thorough research by allowing agents to search iteratively.1  
* **Why It Fails**: Stochastic loops can trigger hundreds of search queries and model calls, causing token costs to spike rapidly.1  
* **Detection Signals**: Cost alerts; model requests timing out; execution trace logs showing deep, circular reasoning trees.  
* **Safer Alternatives**: Implement strict execution budgets, step limits, and timeout boundaries in agent planner configurations.1

### **Magic Document Reader**

* **Symptoms**: Multi-page, visually complex document folders (e.g., scanned PDFs with tables) are sent directly to models as raw file streams, relying on model context to interpret visual and spatial data.  
* **Why Tempting**: Avoids configuring specialized layout extraction tools and OCR parsers.  
* **Why It Fails**: Standard models frequently misinterpret multi-column tables, omit visual data, or confuse page-level spatial hierarchies.2  
* **Detection Signals**: Low extraction precision scores; misread data fields; hallucinated table summaries.  
* **Safer Alternatives**: Implement a structured Document Intelligence Pipeline with layout parsing and spatial coordinate validation.2

### **One Pattern to Rule Them All**

* **Symptoms**: Attempting to use a single architectural pattern (e.g., an open-ended conversational agent) to handle all enterprise AI workloads.  
* **Why Tempting**: Standardizes development on a single framework, simplifying platform team operations.  
* **Why It Fails**: Different tasks have divergent constraints; forcing low-latency tasks through agent loops or highly structured extractions through chat interfaces degrades quality.  
* **Detection Signals**: Rising execution latency; user friction; high development costs as teams patch a monolithic system.  
* **Safer Alternatives**: Select specialized reference patterns matched to the constraints of each task class.14

### **Gateway Bypass**

* **Symptoms**: Product development teams bypass central AI Gateways and deploy custom model endpoints directly using local cloud credentials.  
* **Why Tempting**: Allows developers to prototype and deploy changes without coordination bottlenecks with platform engineering teams.  
* **Why It Fails**: Bypasses corporate security, access controls, token spending audits, and threat detection systems, introducing compliance risks.  
* **Detection Signals**: Hidden API cost increases on cloud billing; security audits finding unmonitored external model traffic.  
* **Safer Alternatives**: Implement the AI Gateway pattern as a mandatory routing policy at the network boundary.

### **Fake Human-in-the-Loop**

* **Symptoms**: Human review interfaces are designed as simple confirmation panels with no access to grounding data or verification tools, encouraging rapid clicking.  
* **Why Tempting**: Minimizes workflow delay and reduces human labor costs while claiming safety compliance.  
* **Why It Fails**: Humans cannot verify claims without grounding data, leading to automated rubber-stamping and un-vetted errors reaching production.13  
* **Detection Signals**: 100% manual review approval rates; downstream error occurrences matching automated prototype testing rates.  
* **Safer Alternatives**: Implement a dedicated Human Review and Escalation Queue with side-by-side evidence, coordinate highlighting, and rejection tools.

### **Deflection Theater**

* **Symptoms**: Customer support systems are configured to deflect user requests at all costs, blocking access to human agents or escalation queues.  
* **Why Tempting**: Minimizes customer support center staffing requirements and operational overhead.  
* **Why It Fails**: Traps users in unresolved loops with incorrect answers, leading to customer churn and brand damage.  
* **Detection Signals**: High customer exit rates; negative post-chat feedback ratings; rising repeat-contact frequencies.  
* **Safer Alternatives**: Support Assistant pattern with structured intent classification and low-friction human escalation paths.

### **Metric Hallucination**

* **Symptoms**: Models generate business intelligence analytics directly against database tables without using standard, governed semantic metric layer definitions.  
* **Why Tempting**: Fast to deploy; avoids the engineering effort of building metadata frameworks or semantic layers.  
* **Why It Fails**: Models hallucinate database join paths, use non-standard calculation logic, or expose restricted records to unauthorized users.  
* **Detection Signals**: Inconsistent business metrics across reports; SQL compilation failures; security policy alerts.  
* **Safer Alternatives**: Implement the Analytics Assistant pattern with strict SQL validators and a governed semantic metric layer.

### **Vector Dump Knowledge System**

* **Symptoms**: Thousands of un-curated, multi-format documents are uploaded into a single vector index without metadata, access rules, or lifecycle curation.8  
* **Why Tempting**: Extremely low configuration cost; provides generic conversational answers across the corpus.  
* **Why It Fails**: Returns outdated, duplicate, or conflicting context chunks, causing model synthesis confusion and access control leaks.  
* **Detection Signals**: Stale information in answers; high rate of irrelevant citations; unauthorized access to sensitive files.  
* **Safer Alternatives**: Enterprise Knowledge System pattern with ingestion filters, dynamic chunking, and IAM integration.8

### **Auto-Merge Coding Agent**

* **Symptoms**: Automated coding tools are configured to commit code patches directly to production branches without developer review or verification testing.1  
* **Why Tempting**: Accelerates feature delivery; minimizes developer review overhead.  
* **Why It Fails**: Plausible but broken patches, logic errors, or security vulnerabilities are deployed directly to production systems.1  
* **Detection Signals**: Rising production regressions; build breakages; automated security alert spikes.  
* **Safer Alternatives**: Enforce sandbox testing and manual peer-developer code reviews for all model-generated patches.

### **Fallback Contract Downgrade**

* **Symptoms**: When a primary, secure model provider fails, the system automatically redirects traffic to a simpler backup model while bypassing security, output validation, or policy contracts.  
* **Why Tempting**: Prioritizes system availability during primary provider outages.  
* **Why It Fails**: Lowers system security, allows unsafe outputs to bypass filters, and risks deploying unvalidated data.  
* **Detection Signals**: Security anomalies during primary provider outages; format validation failures on backup routes.  
* **Safer Alternatives**: Fallback configurations must enforce identical security, validation, and policy contracts across all model routes.

## **No-Use Condition Matrix**

No-use conditions prevent pattern overreach. They do not always mean “never use AI”; they often mean “use another pattern,” “add a human gate,” “use deterministic software,” or “complete readiness work first.”

| Pattern | No-Use / Redesign Condition | Why This Pattern Fails | Safer Architecture |
| :---- | :---- | :---- | :---- |
| **Copilot / Embedded Assistant** | Task requires autonomous execution, exact calculation, or unattended batch processing. | Inline suggestion surface does not provide execution assurance. | Deterministic workflow, analytics assistant, or workflow automation with gates. |
| **Research Agent** | User needs exact source-of-record lookup or answer must be instant and deterministic. | Multi-hop synthesis adds latency and hallucination risk. | SQL/API lookup, enterprise search, or deterministic report. |
| **Support Assistant** | Emergency, legal, medical, abuse, or high-emotion dispute without human escalation. | AI may delay proper human intervention. | Human-first support queue with AI evidence assist only. |
| **Document Intelligence Pipeline** | Source system already provides clean structured data. | OCR/extraction adds unnecessary error and cost. | API ingestion or deterministic parser. |
| **Workflow Automation Agent** | Irreversible or high-value mutation lacks approval, idempotency, or verification. | Side effects can be wrong and unrecoverable. | Human approval queue plus deterministic execution. |
| **Coding Agent** | Codebase cannot be built, tested, sandboxed, or reviewed. | No reliable validation path. | Human developer workflow; build test infrastructure first. |
| **Analytics Assistant** | Metrics are undefined, database access is broad, or output is regulated filing. | Model may invent joins/calculations. | Semantic metric layer, governed BI, deterministic reporting. |
| **Multimodal Review System** | Safety-critical diagnosis or measurement lacks regulated validation and expert authority. | False negatives/positives can cause harm. | Expert-led review with AI as evidence-prep only. |
| **Enterprise Knowledge System** | Corpus lacks ACLs, freshness, ownership, or lifecycle governance. | Retrieval can leak data or surface stale/conflicting answers. | Corpus engineering and permission indexing first. |
| **Decision-Support Cockpit** | Organization wants AI to make final high-impact decisions. | Cockpit pattern supports humans; it does not replace authority. | Manual decision workflow with evidence support. |
| **Background Classifier / Router** | False negatives have high consequence and no exception review exists. | Silent misrouting becomes systemic risk. | Manual triage or deterministic rules until eval/review exists. |
| **Productivity Assistant** | System manipulates records, sends customer messages, or processes regulated data. | Local assistant lacks governance and action controls. | Support assistant, workflow automation, or governed enterprise system. |
| **Governed Agentic Workflow** | Workflow is simple, linear, deterministic, or latency-critical. | Agent graph adds unnecessary complexity and cost. | Static workflow engine or deterministic microservice. |
| **AI Gateway / Control Plane** | One-off local toy prototype with no enterprise data or shared use. | Gateway overhead may exceed value. | Direct sandbox SDK with test credentials only. |
| **Human Review Queue** | Task is low-risk, high-volume, and deterministically validated. | Review creates bottleneck and fatigue. | Automated validation with sampled audit. |
| **Evaluation / Shadow Mode** | Production data cannot be mirrored safely or no evaluation metric exists. | Shadow path creates privacy risk or meaningless scores. | Offline eval with sanitized/representative data. |

If the no-use condition is true, do not “prompt harder.” Change the architecture.

## **Implementation Maturity Levels**

AI architecture maturity describes how safely and repeatably a pattern can be operated. Maturity is not model quality. It is the presence of contracts, evals, telemetry, security, human authority, operations, support, and lifecycle controls.

| Level | Name | Allowed Use | Forbidden Use | Required Controls | Exit Gate to Next Level |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **0** | **Demo** | Local exploration with synthetic or non-sensitive data. | Production data, customer exposure, system-of-record access, unmanaged secrets. | Sandbox only, test credentials, no live integrations, no claims of reliability. | Define use case, data class, owner, and prototype boundary. |
| **1** | **Controlled Prototype** | Internal prototype with curated data and limited users. | Production actions, broad rollout, sensitive data without approval. | Basic prompt/schema, sandbox, initial evals, test keys/vaulted secrets, trace capture. | Pass seed eval, security/data review, and product-fit review. |
| **2** | **Pilot** | Bounded real workflow with limited users and explicit monitoring. | Autonomous high-impact actions, unreviewed external outputs. | Named owner, telemetry, human review path, fallback, incident contact, pilot success criteria. | Demonstrate quality, adoption, cost, safety, and operational readiness. |
| **3** | **Production** | Approved production workflow within defined risk tier. | Unbounded route changes, silent model swaps, missing rollback. | Contract stack, eval gate, deployment manifest, observability, runbook, rollback, support. | Prove repeatability across teams/routes and governance review. |
| **4** | **Governed Scale** | Multi-team or high-volume production under platform controls. | Pattern-specific one-off exceptions without review. | Gateway/control plane, standardized evals, policy automation, cost controls, audit evidence, adoption support. | Package as reusable golden path. |
| **5** | **Platform Golden Path** | Reusable pattern available through developer platform templates. | Bypassing mandatory controls. | Automated scaffolding, contract templates, eval harness, observability, security defaults, documentation, support model. | Continuous lifecycle; no higher maturity required. |

### **Maturity Rules**

| Rule | Meaning |
| :---- | :---- |
| **No production without owner.** | Every deployed pattern needs accountable product, technical, and operational ownership. |
| **No write authority before action verification.** | Tools that mutate state require permission, idempotency, and postcondition checks. |
| **No broad rollout without telemetry.** | Adoption, quality, cost, latency, and breach events must be visible. |
| **No model migration without eval evidence.** | Provider/model/prompt/schema changes are deployment events. |
| **No human review theater.** | Reviewers need evidence, veto power, and time. |
| **No prototype secrets sprawl.** | Even demos use test credentials and safe environments. |
| **No golden path without exit path.** | Reusable patterns must include sourcing and migration assumptions. |

Maturity is not achieved when the demo works. Maturity begins when failure is boring, bounded, and recoverable.

## **Cross-Canon Handoff Map**

AI-ENG-AJ converts the AI Engineering Systems Canon into reusable reference architectures. Earlier reports define the doctrine, constraints, controls, and failure modes. AJ packages them into pattern cards that teams can select, instantiate, evaluate, operate, and govern.

| Canon Report | Input to AJ | How AJ Uses It |
| :---- | :---- | :---- |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, no-AI decisions, product surface. | Determines whether a pattern should be selected at all. |
| **AI-ENG-AG — Adoption Systems** | Training, support, feedback loops, user resistance, incentives. | Adds adoption and support requirements to pattern cards. |
| **AI-ENG-AH — Sourcing and Vendor Strategy** | Build/buy/open/hybrid, vendor exit, control plane. | Adds sourcing and exit fields to every pattern. |
| **AI-ENG-AI — Contract Thinking** | Prompt, schema, retrieval, tool, permission, route, eval, observability contracts. | Defines the contract surfaces required by each pattern. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, accountability, procurement, compliance. | Defines governance and evidence requirements. |
| **AI-ENG-AE — Sustainable AI** | Cost, energy, routing, lifecycle impact. | Adds cost drivers and degraded/resource-aware modes. |
| **AI-ENG-J — Throughput Mechanics** | Latency, batching, streaming, queueing. | Shapes route strategy and user-surface expectations. |
| **AI-ENG-K — Weight Dynamics** | Model size, quantization, adaptation. | Informs route class and self-host/open-weight viability. |
| **AI-ENG-L — Serving Architecture** | Gateways, serving topologies, fallback. | Implements patterns through route/control-plane designs. |
| **AI-ENG-M — Agentic Orchestration** | Planners, executors, graph workflows, multi-agent systems. | Grounds agentic and workflow automation patterns. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, idempotency, side effects. | Defines tool/action requirements for action-oriented patterns. |
| **AI-ENG-O — Action Verification** | Source-of-record confirmation and false-success prevention. | Defines postcondition verification for tools and workflows. |
| **AI-ENG-P — Multimodal Understanding** | Vision/audio/video evidence and uncertainty. | Grounds document and multimodal review patterns. |
| **AI-ENG-Q — Speech and Realtime Systems** | Streaming, voice latency, turn-taking. | Informs interactive/embedded and support surfaces. |
| **AI-ENG-R — UI Agents** | Browser/UI control and interface state. | Informs UI-agent authority and action boundaries. |
| **AI-ENG-S — Production Pathologies** | Common production failures. | Feeds anti-patterns, failure modes, and degraded modes. |
| **AI-ENG-T — Boundary Defense** | Prompt injection, tenant isolation, egress policy. | Defines security boundaries for every pattern. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, dependency risk. | Informs sourcing, deployment, and platform patterns. |
| **AI-ENG-V — Resource Abuse** | Loop abuse, denial-of-wallet, budget failures. | Defines resource controls and cost drivers. |
| **AI-ENG-W — UX Resilience** | Fallback, degraded UX, continuity. | Defines degraded-mode library. |
| **AI-ENG-X — User Trust** | Trust calibration, contestability, transparency. | Defines user expectation and human-review surfaces. |
| **AI-ENG-Y — Human Review** | Reviewer authority, queues, maker-checker, fatigue. | Grounds human review and escalation patterns. |
| **AI-ENG-Z — Telemetry** | Runtime traces, metrics, correction signals. | Defines telemetry-by-pattern. |
| **AI-ENG-AA — Evaluation** | Golden sets, eval gates, regression. | Defines eval-by-pattern. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit proof. | Defines evidence boundaries. |
| **AI-ENG-AC — AI Operations** | Incidents, runbooks, rollback, containment. | Defines operational maturity and breach response. |

### **MCP and Tooling Note**

Tooling standards should be referenced carefully. MCP uses JSON-RPC messages over stdio and Streamable HTTP; SSE may be used within Streamable HTTP where supported. Architecture cards should describe the tool contract and transport requirements generically, then name MCP or other protocols as implementation options rather than hard dependencies.

### **Core Handoff Rule**

AJ is where doctrine becomes reusable architecture.

```text
A reference architecture is a reusable failure-aware contract bundle.

It tells engineers:
  when to use the pattern,
  when not to use it,
  what contracts are required,
  how it is evaluated,
  what telemetry proves,
  how humans retain authority,
  how it degrades,
  how it exits,
  and how it fails safely.
```

#### **Works cited**

1. What Are Agentic Design Patterns? 2026 Pattern Catalog | Augment ..., accessed June 15, 2026, [https://www.augmentcode.com/guides/agentic-design-patterns](https://www.augmentcode.com/guides/agentic-design-patterns)  
2. Enterprise AI Agents: Agentic Design Patterns Explained - Tungsten Automation, accessed June 15, 2026, [https://www.tungstenautomation.com/learn/blog/build-enterprise-grade-ai-agents-agentic-design-patterns](https://www.tungstenautomation.com/learn/blog/build-enterprise-grade-ai-agents-agentic-design-patterns)  
3. Agent system design patterns | Databricks on AWS, accessed June 15, 2026, [https://docs.databricks.com/aws/en/agents/agent-system-design-patterns](https://docs.databricks.com/aws/en/agents/agent-system-design-patterns)  
4. Reference architecture: The blueprint for safe and scalable autonomy in SRE and DevOps, accessed June 15, 2026, [https://www.ilert.com/blog/reference-architecture-for-scalable-autonomy-in-sre-and-devops](https://www.ilert.com/blog/reference-architecture-for-scalable-autonomy-in-sre-and-devops)  
5. Design Patterns for Agentic AI and Multi-Agent Systems - AppsTek Corp, accessed June 15, 2026, [https://appstekcorp.com/blog/design-patterns-for-agentic-ai-and-multi-agent-systems/](https://appstekcorp.com/blog/design-patterns-for-agentic-ai-and-multi-agent-systems/)  
6. AI System Design Patterns for 2026: Architecture That Scales, accessed June 15, 2026, [https://zenvanriel.com/ai-engineer-blog/ai-system-design-patterns-2026/](https://zenvanriel.com/ai-engineer-blog/ai-system-design-patterns-2026/)  
7. RAG Evaluation Metrics: Assessing Answer Relevancy, Faithfulness, Contextual Relevancy, And More - Confident AI, accessed June 15, 2026, [https://www.confident-ai.com/blog/rag-evaluation-metrics-answer-relevancy-faithfulness-and-more](https://www.confident-ai.com/blog/rag-evaluation-metrics-answer-relevancy-faithfulness-and-more)  
8. Evaluating the Performance of rag Systems: Metrics Guide ..., accessed June 15, 2026, [https://unstructured.io/insights/rag-evaluation-a-data-pipeline-performance-framework](https://unstructured.io/insights/rag-evaluation-a-data-pipeline-performance-framework)  
9. AI Gateway Architecture: A Guide for Technical Teams | MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/](https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/)  
10. AI control plane: the architecture for AI governance and security | Speakeasy, accessed June 15, 2026, [https://www.speakeasy.com/resources/ai-control-plane](https://www.speakeasy.com/resources/ai-control-plane)  
11. What Is An AI Gateway? | IBM, accessed June 15, 2026, [https://www.ibm.com/think/topics/ai-gateway](https://www.ibm.com/think/topics/ai-gateway)  
12. System Architecture Overview - Envoy AI Gateway, accessed June 15, 2026, [https://aigateway.envoyproxy.io/docs/concepts/architecture/system-architecture](https://aigateway.envoyproxy.io/docs/concepts/architecture/system-architecture)  
13. Agentic Design Patterns | Terezinha Tech Operations (ttoss), accessed June 15, 2026, [https://ttoss.dev/docs/ai/agentic-design-patterns](https://ttoss.dev/docs/ai/agentic-design-patterns)  
14. Choosing the Right Agentic Design Pattern: A Decision-Tree Approach, accessed June 15, 2026, [https://machinelearningmastery.com/choosing-the-right-agentic-design-pattern-a-decision-tree-approach/](https://machinelearningmastery.com/choosing-the-right-agentic-design-pattern-a-decision-tree-approach/)  
15. Model Context Protocol architecture patterns for multi-agent AI systems - IBM Developer, accessed June 15, 2026, [https://developer.ibm.com/articles/mcp-architecture-patterns-ai-systems/](https://developer.ibm.com/articles/mcp-architecture-patterns-ai-systems/)  
16. Model Context Protocol (MCP) explained: A practical technical overview for developers and architects - CodiLime, accessed June 15, 2026, [https://codilime.com/blog/model-context-protocol-explained/](https://codilime.com/blog/model-context-protocol-explained/)  
17. Architecture overview - Model Context Protocol, accessed June 15, 2026, [https://modelcontextprotocol.io/docs/learn/architecture](https://modelcontextprotocol.io/docs/learn/architecture)  
18. What is the Model Context Protocol (MCP)? - Databricks, accessed June 15, 2026, [https://www.databricks.com/blog/what-is-model-context-protocol](https://www.databricks.com/blog/what-is-model-context-protocol)  
19. What is an AI Gateway? The Complete Guide (2026) - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/ai-gateway](https://www.truefoundry.com/blog/ai-gateway)  
20. AI Gateway & LLM Gateway: How They Work and What They Miss - Atlan, accessed June 15, 2026, [https://atlan.com/know/what-is-ai-gateway-llm-gateway/](https://atlan.com/know/what-is-ai-gateway-llm-gateway/)  
21. How to Evaluate RAG Systems: Metrics, Methods, and What to Measure First - Comet, accessed June 15, 2026, [https://www.comet.com/site/blog/rag-evaluation/](https://www.comet.com/site/blog/rag-evaluation/)  
22. RAG Deep Dive Series: Evaluation & Production - Kalvad Blog, accessed June 15, 2026, [https://blog.kalvad.com/rag-deep-dive-series-evaluation-production/](https://blog.kalvad.com/rag-deep-dive-series-evaluation-production/)  
23. AI Gateway Patterns: Cost Control and Reliability at Scale [2026] - Virtido, accessed June 15, 2026, [https://virtido.com/blog/ai-gateway-patterns-production-guide](https://virtido.com/blog/ai-gateway-patterns-production-guide)  
24. Multi Agent Architecture: Patterns, Use Cases & Production Reality - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/multi-agent-architecture](https://www.truefoundry.com/blog/multi-agent-architecture)

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