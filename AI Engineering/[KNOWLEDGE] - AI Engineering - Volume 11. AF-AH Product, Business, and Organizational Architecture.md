# [KNOWLEDGE] - AI Engineering - Volume 11. AF-AH Product, Business, and Organizational Architecture

[Volume 11. AF-AH Product, Business, and Organizational Architecture]
  - [AI-ENG-AF — AI Product Architecture - Use-Case Selection, Workflow Fit & Value Design](#ai-eng-af--ai-product-architecture---use-case-selection-workflow-fit--value-design)
  - [AI-ENG-AG — Adoption Systems - Training, Feedback Loops & Change Management](#ai-eng-ag--adoption-systems---training-feedback-loops--change-management)
  - [AI-ENG-AH — Build, Buy, Open Source & Vendor Strategy - Doctrinal Knowledge Base for Strategic Dependency Architecture](#ai-eng-ah--build-buy-open-source--vendor-strategy---doctrinal-knowledge-base-for-strategic-dependency-architecture)

---


# AI-ENG-AF — AI Product Architecture - Use-Case Selection, Workflow Fit & Value Design

## **1. Doctrinal Foundations and the AI-Shaped Hole Fallacy**

AI product architecture is the engineering and systems discipline of selecting, shaping, rejecting, and structuring artificial intelligence capabilities based on their systemic fit within human workflows. It stands as the architectural gateway before software development begins, establishing the criteria that separate high-yield cognitive automation from expensive, non-deterministic failures.  
Traditional digital product discovery focuses on establishing customer needs, usability, technical feasibility, and business viability.1 AI product architecture, however, must navigate a fundamentally different landscape: the transition from deterministic software—where inputs map reliably to outputs and pass standard quality assurance gates—to probabilistic systems that operate with varying degrees of accuracy and exhibit behavioral drift over time.2 In a traditional software release, shipping a feature allows the team to confirm its status as functional or broken; in an AI-driven architecture, a deployed agent may function correctly in only a portion of execution paths, necessitating continuous validation, feedback loops, and runtime governance to sustain business value.2  
This probabilistic nature gives rise to the **AI-Shaped Hole Fallacy**. This fallacy is the systemic mistake of assuming that because an AI model *can* be integrated into a specific process, the workflow possesses a genuine, valuable, and structurally viable AI-shaped problem. It manifests when product teams start their discovery cycles with the mandate to deploy a specific model class rather than isolating a high-intensity workflow bottleneck, user friction point, or data processing constraint.4  
As Matt Clifford observes, "There is no AI-shaped hole in most organizations".5 While modern frontier models can routinely outperform human experts on theoretical tests and clinical evaluations, the utility of this raw intelligence is constrained by human institutions, entrenched rules, regulatory compliance frameworks, and strict permission barriers.5 Adding unconstrained, probabilistic reasoning into tightly coupled, highly regulated systems introduces high latency, cost, and verification burdens that frequently outweigh the marginal cognitive utility of the model.5  
To diagnose this fallacy early, product architects must dissect the proposed use case against deterministic alternatives, evaluating whether the core bottleneck is a lack of cognitive reasoning or simply a failure of traditional workflow design, database schema, or information architecture.4

### **Doctrinal Governing Principle**

AI product architecture begins by identifying where AI belongs and where it does not. A valid AI use case must have workflow friction, data readiness, user tolerance, evaluation clarity, acceptable risk, cost-to-serve viability, and a value model strong enough to justify probabilistic behavior. The useful question is not "Can we add AI?" but "Is AI the right mechanism for this workflow, for these users, with this data, under this risk, at this cost, with measurable value and a clear definition of success?"

### **AI-Shaped Hole Fallacy Diagnostic**

The diagnostic tool below is designed to isolate and expose instances of the AI-Shaped Hole Fallacy during the initial product discovery phase:

| Symptom | Root Cognitive Cause | Failed Automation Assumption | Deterministic Alternative | Diagnostic Probe |
| :---- | :---- | :---- | :---- | :---- |
| **The Chatbot Anti-Pattern** Users are presented with an open-ended conversational canvas to complete highly structured tasks, resulting in high interaction friction. | Mistaking a model's conversational interface for a natural user workflow.8 | Conversational input is universally more intuitive than structured layouts. | Structured web forms, multi-select filters, relational search syntax, and dynamic templates.8 | Does the user need to converse with their data, or do they simply need a fast, precise way to filter, sort, and display structured fields? |
| **Probabilistic Replacement** A deterministic rules engine or database lookup is replaced by an LLM pipeline, resulting in non-deterministic compliance failures and hallucinations. | Believing that LLMs are drop-in replacements for standard relational database queries or business rules. | Generative models can serve as highly reliable truth engines for exact data retrieval. | Standard SQL queries, structured APIs, rules-based validation engines, and deterministic workflow states.4 | Can this task be represented as a finite set of Boolean conditions, or does it genuinely require semantic context and synthesis of unstructured text? 4 |
| **Verification Debt** The system drafts an artifact in seconds, but requires minutes of manual, line-by-line human verification, resulting in zero net time savings. | Designing for output creation while ignoring the cognitive overhead and liability of output validation.9 | Synthesizing content faster automatically translates to a reduction in total task cycle time. | Structured modular builders, boilerplate components, and rules-based formatting. | Is the cost of validating the probabilistic output higher than the cost of drafting it manually from a structured template? 9 |
| **Unattributed Cost Cascades** The pilot operates at high volume, but the unit economics erode the product's gross margins as model execution costs scale.8 | Ignoring non-deterministic agent loop costs, long context windows, and tool-calling retries during early prototyping.12 | Model call costs scale linearly and remain predictable without active optimization layers.12 | Targeted deterministic automation, local lightweight classifiers, and semantic cache layers.8 | What is the maximum cost-to-serve limit per task, and does the model's non-deterministic execution path fit within that envelope at scale? 12 |
| **The Passive Validation Loop** The user interface fails to capture explicit corrections, allowing model drift and errors to propagate silently into downstream systems. | Treating human-in-the-loop oversight as a passive checkbox rather than an active data collection interface.9 | Human operators will actively catch and correct every subtle semantic error without explicit UI forcing functions.13 | Explicit user confirmation fields, diff rejection tracking, and validation gates.14 | Does the system capture the exact differences between the model's output and the human's final edit to retrain and evaluate future runs? 14 |

## **2. Conceptual Glossary of AI Product Architecture**

Understanding AI product architecture requires a standardized, high-dimensional vocabulary that bridges the gap between machine learning capabilities, interface design, and SaaS financial economics.

| Term | Doctrinal Definition | Systemic Implication |
| :---- | :---- | :---- |
| **AI Product Architecture** | The systemic discipline of selecting, shaping, rejecting, and designing AI capabilities based on workflow fit, user tolerance, data readiness, evaluation clarity, risk level, cost-to-serve, and measurable business value. | Acts as the architectural gateway and control gate before any technical implementation, code construction, or model selection begins. |
| **AI-Shaped Hole Fallacy** | The erroneous assumption that because AI can be integrated into a workflow, the workflow has a genuine need for probabilistic reasoning, leading to features that add cognitive overhead, cost, and verification debt without resolving structural bottlenecks.5 | Causes high-investment product failures by substituting conversational interfaces or generative models where deterministic logic is superior.4 |
| **Automation** | An operational strategy where an AI system executes a complete task or sequence of tasks within a workflow without requiring real-time human intervention or validation prior to execution.9 | Appropriate only where inputs are bounded, outcomes are highly verifiable, and the cost of occasional execution failure is low.9 |
| **Augmentation** | An operational strategy where the AI system assists a human operator by drafting, summarizing, reviewing, or suggesting content, while the human retains full agency, execution authority, and legal accountability.9 | Preserves the human's interpretive space and cognitive engagement, making it highly suitable for nuanced, contextual, and high-consequence tasks.9 |
| **Delegation** | The structured transfer of bounded subtasks from a human to an AI system under strict execution constraints, where the system performs the work but must present its outputs for human validation.16 | Minimizes human cognitive fatigue while maintaining clear accountability, acting as a middle-tier posture between automation and augmentation.17 |
| **Decision Support** | An interaction paradigm where the AI system structures evidence, maps trade-offs, highlights risks, and ranks options, leaving the final selection and execution of authority entirely to the human operator.13 | Mitigates automation bias by preventing the system from proposing a single "correct" answer, forcing active human cognitive processing.13 |
| **Workflow Fit** | The measure of how cleanly an AI capability integrates into the existing sequence of actions, data models, and user interfaces of an enterprise process, without introducing disruptive friction or validation bottlenecks.4 | Determines user adoption; high model accuracy cannot overcome a poor workflow fit that demands frequent context-switching or manual copy-pasting.4 |
| **User Tolerance** | The maximum threshold of latency, uncertainty, error rates, and manual review overhead that a specific user class is willing to accept in exchange for the utility provided by an AI system.19 | Governs the choice of product surface; low-tolerance environments require invisible backend AI or autocomplete, while high-tolerance environments permit chat. |
| **Data Readiness** | The state of structural, legal, and operational fitness of an organization's data assets, determined by whether the required information is accessible, governed, permissioned, clean, and representative of the target use case.21 | Evaluated on a use-case-specific basis; general-purpose data preparation is insufficient to support targeted model reasoning.22 |
| **Evaluation Clarity** | The ability to establish objective, measurable success criteria, baseline performance thresholds, and systematic evaluation rubrics for a probabilistic system prior to writing production code.2 | Ensures the system is testable; if success cannot be programmatically or systematically evaluated, the product cannot be safely scaled.2 |
| **Cost-to-Serve** | The comprehensive financial and infrastructure cost required to deliver a single completed AI task or transaction, spanning token consumption, retrieval overhead, hosting, API calls, logging, and human verification costs.8 | Directly impacts gross margins; high-token reasoning models must be reserved for high-value transactions to prevent margin erosion.8 |
| **Value Design** | The strategic framework that aligns AI capabilities with concrete business outcomes, such as cycle-time reduction, accuracy improvements, and margin protection, rather than relying on qualitative engagement metrics.3 | Rejects the labor-replacement fantasy, focusing instead on throughput enhancement, error mitigation, and specialized knowledge democratization.23 |
| **No-AI Decision** | The deliberate architectural determination that a deterministic software mechanism, workflow redesign, or interface improvement is more reliable, cost-effective, and safe than deploying a probabilistic model.4 | Handled as a positive, successful product outcome that conserves engineering capital and avoids technical debt.4 |
| **Product Surface** | The specific user interface or API gateway through which the AI's capabilities are exposed to and controlled by the user, ranging from open-ended chat inputs to contextual inline suggestions and invisible backend ranking models.14 | Shapes user trust and interaction style; selected dynamically based on risk, latency requirements, and user tolerance metrics.14 |

## **3. The Use-Case Selection Framework**

Selecting viable AI use cases is not a search for technical feasibility alone. The fact that a frontier model can perform a task in a demo does not mean the workflow is ready for probabilistic automation, the data is usable, the users will tolerate the interaction, or the economics survive production scale.

The Use-Case Selection Framework operates as a product architecture gate. It evaluates whether the proposed capability belongs in one of five paths:

1. **No-AI / Deterministic Path** — solve with rules, workflow redesign, database structure, search, forms, templates, or conventional software.
2. **Discovery Experiment** — validate the problem, workflow, user tolerance, and value before model work begins.
3. **Prototype / Pilot** — run a bounded AI experiment with limited blast radius.
4. **Production Design** — proceed to architecture, evals, governance, telemetry, and implementation.
5. **Reject / Defer** — stop because a fatal flaw exists or the value case is weak.

```text
USE-CASE SELECTION FLOW

[ Candidate Use Case ]
        |
        v
[ 1. Is there a real workflow pain? ]
        |
        +-- no --> [ Reject / Defer ]
        |
        v
[ 2. Is AI the right mechanism? ]
        |
        +-- no --> [ No-AI / Deterministic Path ]
        |
        v
[ 3. Is required data available, permissioned, and representative? ]
        |
        +-- no --> [ Data Readiness Work / Defer ]
        |
        v
[ 4. Can success be evaluated before production? ]
        |
        +-- no --> [ Discovery Experiment / Eval Design ]
        |
        v
[ 5. Are risks reversible, governable, and acceptable? ]
        |
        +-- no --> [ Reject / Redesign / Human-Only Gate ]
        |
        v
[ 6. Will users tolerate latency, uncertainty, and review burden? ]
        |
        +-- no --> [ Redesign Product Surface ]
        |
        v
[ 7. Can the system integrate without workflow distortion? ]
        |
        +-- no --> [ Workflow Redesign / API Work First ]
        |
        v
[ 8. Do unit economics survive production volume? ]
        |
        +-- no --> [ Route Optimization / No-AI / Reject ]
        |
        v
[ 9. Can operations monitor, contain, and improve it? ]
        |
        +-- no --> [ Operational Readiness Work ]
        |
        v
[ 10. Is adoption plausible and valuable? ]
        |
        +-- no --> [ Discovery Experiment / Defer ]
        |
        v
[ Production Design Candidate ]
```

### **Use-Case Gate Dimensions**

| Dimension | Core Question | Passing Evidence |
| :---- | :---- | :---- |
| **Workflow Pain** | Is there a real bottleneck, delay, error pattern, or unmet user need? | Baseline cycle time, error rate, support burden, user interviews, workflow observation. |
| **AI Suitability** | Does the task require semantic interpretation, synthesis, classification, or language flexibility? | Clear reason deterministic alternatives are insufficient. |
| **Data Readiness** | Is the required data accessible, permissioned, representative, and governed? | Source inventory, data lineage, access model, sample corpus, privacy review. |
| **Evaluation Clarity** | Can success and failure be measured? | Golden set, rubric, task-specific metrics, human review procedure. |
| **Risk and Reversibility** | What happens when the model is wrong? | Risk tier, reversibility plan, human gate, rollback/compensation policy. |
| **User Tolerance** | Will users accept latency, uncertainty, corrections, and review burden? | Persona-specific tolerance model, usability test, correction-rate target. |
| **Integration Fit** | Does the feature slot into the workflow without copy-paste theater? | API availability, UI surface plan, state ownership, transaction boundaries. |
| **Cost-to-Serve** | Can the feature be delivered profitably at expected usage? | Route cost estimate, token/retrieval/tool/review costs, margin model. |
| **Operational Readiness** | Can the system be monitored, rolled back, and improved? | Telemetry, evals, runbook, owner, incident and feedback loop. |
| **Adoption and Value** | Will users actually change behavior, and will the business capture value? | Adoption hypothesis, value metric, incentive analysis, change plan. |

Scoring is a structured decision aid, not a mathematical oracle. A high score does not override a fatal flaw. A low score does not always kill an idea; it may route the opportunity to workflow redesign, data readiness, or discovery experimentation.

## **4. The Automation versus Augmentation Matrix**

AI product architecture must decide how much authority the system receives. The key design question is not whether the model is “smart enough,” but where human judgment, machine execution, verification, and accountability belong in the workflow.

Automation and augmentation are not opposites. A system can automate information retrieval while preserving human decision authority. It can automate drafting while requiring human approval. It can automate execution only where inputs are bounded, errors are reversible, and verification is reliable.

### **Human-AI Allocation Principles**

| Principle | Meaning |
| :---- | :---- |
| **Automate bounded work, not accountability.** | The system may perform tasks, but responsibility must remain assigned to a human role or organizational control. |
| **Separate recommendation from execution.** | Advice, draft, approval, and action should be distinct workflow states. |
| **Use humans where interpretation matters.** | High-context, high-liability, ambiguous, or value-laden decisions require human authority. |
| **Use machines where verification is cheap.** | Automation fits best where outputs can be checked by schema, tests, source-of-record APIs, or deterministic constraints. |
| **Preserve human situational awareness.** | Highly automated systems need handoff, explanation, rehearsal, and fallback controls. |
| **Design against overtrust and undertrust.** | Interfaces should calibrate confidence, show evidence, capture corrections, and avoid fluent-but-false authority. |

### **Automation vs. Augmentation Matrix**

| Product Posture | System Authority | Best-Fit Task Type | Validation Cost | Failure Consequence | Product Surface | Required Controls |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **No-AI / Deterministic** | No model authority. | Exact math, strict business rules, relational lookup, compliance checks with finite logic. | Low via deterministic tests, or impossibly high for probabilistic output. | Medium to extreme. | Forms, tables, filters, APIs, rules engines. | Schema validation, database constraints, deterministic tests, audit logs. |
| **Decision Support** | System structures evidence and options; human decides. | Legal, medical, financial, compliance, strategy, incident review. | High. | High. | Evidence cockpit, option matrix, risk panel. | Human final decision, source links, independent judgment step, reason logging. |
| **Augmentation** | System drafts or suggests; human edits and approves. | Writing, summarization, customer replies, internal analysis, document review. | Medium. | Low to high depending on domain. | Side-by-side editor, suggested reply card, review panel. | Accept/reject/edit capture, citation/evidence display, user-owned final action. |
| **Delegation** | System performs bounded subtask; human reviews output or exceptions. | Extraction, routing, classification, test generation, structured document processing. | Low to medium. | Low to medium if reversible. | Queue, checklist, structured output panel. | Confidence thresholds, schema validation, exception queue, rollback/retry limits. |
| **Automation** | System acts without real-time human approval. | High-volume, low-risk, reversible, easily verified workflows. | Low. | Low. | Invisible backend route, automated queue, rules-bound agent. | Hard policy gates, source-of-record verification, monitoring, circuit breakers. |
| **Blocked / Human-Only** | System may assist with evidence but cannot decide or execute. | Irreversible, safety-critical, legally sensitive, or existential-risk workflows. | High or uncertain. | Extreme. | Manual workflow with optional evidence support. | Multi-human approval, deterministic controls, complete audit trail, model action disabled. |

### **Design Controls by Cognitive Stage**

| Cognitive Stage | Safe AI Role | Unsafe Anti-Pattern |
| :---- | :---- | :---- |
| **Information Acquisition** | Retrieve, classify, summarize, cluster, and surface relevant material. | Hide missing evidence or retrieve from unauthorized sources. |
| **Information Analysis** | Compare evidence, extract patterns, detect anomalies, draft explanations. | Present unsupported synthesis as fact. |
| **Decision Selection** | Rank options, show tradeoffs, identify risk, ask clarifying questions. | Pick a single “correct” decision in high-impact contexts. |
| **Action Implementation** | Execute bounded, reversible, verified actions. | Mutate external systems without authorization, idempotency, or verification. |

The correct posture is selected by risk, reversibility, validation cost, user tolerance, and workflow value—not by model capability alone.

## **5. Workflow Fit Mapping and Task Decomposition**

AI systems fail when they are treated as magic overlays on top of un-optimized, unchanged operational processes.4 True product value is unlocked when the workflow itself is systematically decomposed, decisions are isolated, and touchpoints are re-engineered around the capabilities and limitations of probabilistic models.4

### **Task Decomposition Mechanics**

Product architects must operate on the fundamental truth that **the model is not the unit of work**.4 Workflows must be decomposed into granular, discrete tasks (e.g., intake, triage, retrieval, synthesis, verification, write-back).4 This allows the architect to isolate the exact bottlenecks where cognitive variability, semantic ambiguity, or unstructured data processing create systemic delays.4 Instead of attempting to automate an entire role, the system targets only the highly specific sub-problems that decompose into independent, evaluable tasks.17

### **Workflow Fit Map**

The operational map below illustrates how AI should be inserted into a high-volume, unstructured document intake and compliance workflow. The model is not the workflow. The workflow is decomposed into stages, and each stage receives the lowest-risk mechanism that can satisfy its task.

```text
WORKFLOW FIT MAP: DOCUMENT INTAKE AND COMPLIANCE REVIEW

[ Emails / customer feedback / PDF contracts / scanned attachments ]
        |
        v
+--------------------------------------------------------------+
| STAGE 1: INTAKE AND NORMALIZATION                            
| Mechanism: parser + OCR + lightweight extraction route        
| Input: messy documents and message text                       
| Output: validated structured JSON + source references         
| AI role: extract and normalize where deterministic parsing ends
+-----------------------------+--------------------------------+
                              |
                              v
+--------------------------------------------------------------+
| STAGE 2: EVIDENCE RETRIEVAL                                  
| Mechanism: hybrid retrieval + permission filter + reranker   
| Input: structured payload, account metadata, policy corpus    
| Output: relevant source passages with citations               
| AI role: retrieve and rank candidate evidence                 
+-----------------------------+--------------------------------+
                              |
                              v
+--------------------------------------------------------------+
| STAGE 3: POLICY ANALYSIS AND DRAFT SYNTHESIS                  
| Mechanism: governed synthesis route                           
| Input: validated JSON + retrieved evidence + policy rules     
| Output: draft memo, risk flags, missing evidence list         
| AI role: synthesize, compare, explain                         
+-----------------------------+--------------------------------+
                              |
                              v
+--------------------------------------------------------------+
| STAGE 4: HUMAN VERIFICATION GATE                              
| Mechanism: evidence cockpit + diff review + approval capture  
| Input: draft memo, citations, extracted fields, risk flags    
| Output: approved, edited, rejected, or escalated package      
| AI role: assist review; never silently approve                
+-----------------------------+--------------------------------+
                              |
                              v
+--------------------------------------------------------------+
| STAGE 5: DETERMINISTIC WRITE-BACK                             
| Mechanism: API / ERP / CRM transaction with schema validation 
| Input: signed and verified payload                            
| Output: committed transaction or explicit failure             
| AI role: none at transaction boundary                         
+--------------------------------------------------------------+
```

### **Workflow-Fit Rules**

| Workflow Stage | AI Belongs When | AI Does Not Belong When |
| :---- | :---- | :---- |
| **Intake** | Inputs are messy, varied, unstructured, or multilingual. | Inputs already arrive in clean structured fields. |
| **Retrieval** | Evidence is spread across unstructured documents or semantic sources. | The answer is a direct database field or exact lookup. |
| **Synthesis** | Human review benefits from summarized evidence, options, or risk flags. | Output requires exact arithmetic, legal execution, or direct authority. |
| **Verification** | AI can highlight inconsistencies, missing evidence, or likely errors. | The system hides uncertainty or makes approval feel automatic. |
| **Write-back** | AI has no direct authority; it may prepare a payload for review. | The model mutates systems without deterministic validation and authorization. |

Use route classes rather than vendor names in architecture diagrams. Model catalogs change. Workflow responsibilities do not.

### **High-Yield versus Low-Yield AI Insertion Points**

When mapping workflows, architects must systematically identify where AI belongs and where it must be aggressively kept out:

| High-Yield AI Insertion Points | Systemic Value Generated | Low-Yield / Prohibited AI Insertion Points | Structural Hazard / Failed Fit |
| :---- | :---- | :---- | :---- |
| **Messy Language Intake** Converting unstructured, multi-format PDFs, emails, and support tickets into validated JSON schemas.4 | Eliminates manual data-entry delays and reduces transcription errors.4 | **Deterministic Math Calculations** Using an LLM to sum line items, calculate tax rates, or process financial statements. | Models are statistically prone to calculation errors; math must always be routed to deterministic code. |
| **Evidence Synthesis** Condensing multi-source policy documents or technical manuals into summaries grounded by direct citations.9 | Reduces manual lookup times, accelerating research and policy verification phases.9 | **Direct Compliance Decisions** Allowing a model to autonomously approve or deny claims, execute legal transactions, or lock user accounts. | High liability, potential for silent failures, and complete loss of human oversight.9 |
| **Triage & Intent Routing** Classifying ambiguous customer requests and routing them to specialized queues based on semantic intent.4 | Automatically routes tasks to the correct expert queues while filtering out noise.4 | **High-Volume Low-Margin SEO Text** Programmatically generating massive volumes of content to flood search indexes. | Incurs massive API bills while degrading search quality and eroding long-term user trust.8 |
| **Synthesized Review Packages** Highlighting missing data, legal risks, or policy violations in draft documents before execution.4 | Minimizes critical compliance errors and ensures consistent policy adherence across departments.4 | **Direct Database Lookups** Forcing users to query exact record fields (e.g., "what is customer X's phone number") via a conversational chat canvas. | Chat increases latency and cost; a simple relational filter UI is faster, cheaper, and exact.5 |

## **6. AI Use-Case Feasibility Scorecard**

Before approving a candidate AI use case for technical design, product leaders and system architects should evaluate it using a structured feasibility scorecard. The scorecard is not a funding slot machine. It is a shared decision artifact that exposes assumptions, fatal flaws, and required readiness work.

### **Scoring Method**

Each dimension is scored from 1 to 5.

| Score | Meaning |
| :---- | :---- |
| **1** | Poor fit; major unresolved blocker. |
| **2** | Weak fit; significant readiness work required. |
| **3** | Plausible but unproven; discovery or prototype needed. |
| **4** | Strong candidate; bounded pilot likely justified. |
| **5** | Production-design candidate with clear evidence. |

### **Feasibility Scorecard Rubric**

| # | Dimension | Low Readiness: 1–2 | Medium Readiness: 3 | High Readiness: 4–5 |
| :---- | :---- | :---- | :---- | :---- |
| **1** | **Workflow Pain** | Low-frequency, low-value, or already solved by deterministic software. | Clear friction exists but baseline cost/error/time is weakly measured. | High-frequency or high-impact bottleneck with measured baseline. |
| **2** | **AI Suitability** | Requires exact deterministic behavior, math, or finite business rules. | Requires extraction, drafting, classification, or bounded synthesis. | Requires semantic interpretation, ambiguous language, synthesis, or adaptive workflow support. |
| **3** | **Data Readiness** | Data is inaccessible, unpermissioned, unrepresentative, or unowned. | Data exists but requires cleaning, lineage, permissions, or parser work. | Data is accessible, permissioned, representative, versioned, and governed. |
| **4** | **Evaluation Clarity** | Success is subjective or undefined. | Human rubric exists but golden data and automated checks are incomplete. | Task-specific metrics, golden data, review process, and regression gates exist. |
| **5** | **Risk and Reversibility** | Errors are high-impact, irreversible, or legally/safety critical. | Errors are contained but require human remediation. | Errors are reversible, detectable, low-impact, and bounded by controls. |
| **6** | **User Tolerance** | Users demand exactness, instant response, and no review burden. | Users accept limited uncertainty or latency for clear benefit. | Users accept probabilistic assistance, review, async processing, or correction loops. |
| **7** | **Integration Complexity** | Requires brittle UI automation, copy-paste, or legacy systems without APIs. | Standard APIs exist but permissions, transactions, or state boundaries are complex. | Clean APIs, state ownership, auth, and transaction boundaries exist. |
| **8** | **Cost-to-Serve Viability** | Model, retrieval, tool, and review costs threaten margins. | Costs look plausible but depend on routing, caching, or usage controls. | Cost per verified task fits target margin with sensitivity analysis. |
| **9** | **Operational Readiness** | No owner, telemetry, rollback, eval, or incident path. | Basic monitoring exists but behavior and quality tracking are immature. | Traceability, evals, runbooks, feedback loops, and owners exist. |
| **10** | **Adoption Likelihood** | Users resist, workflow change is disruptive, or incentives conflict. | Users are interested but workflow/training burden is material. | Users actively want relief and feature slots into existing surfaces. |
| **11** | **Governance Burden** | Regulated/high-impact use with weak lineage, audit, or policy controls. | Governance path exists but requires additional review or tooling. | Compliance, audit, access control, and review obligations are understood and supportable. |
| **12** | **Strategic Value** | Novelty feature, no moat, easy to copy, weak business connection. | Improves workflow but depends heavily on generic third-party capability. | Builds durable advantage through proprietary workflow, data, distribution, or feedback loop. |

### **Decision Bands**

| Average Score / Condition | Decision |
| :---- | :---- |
| **Any fatal flaw present** | Stop, redesign, or route to deterministic/no-AI path. |
| **Average below 2.5** | Reject or defer. |
| **2.5–3.4** | Discovery experiment or readiness work. |
| **3.5–4.2** | Bounded prototype or pilot. |
| **Above 4.2 with no fatal flaws** | Production architecture design candidate. |

### **Architectural Kill-Switches: Fatal Flaws**

| Fatal Flaw | Hazard | Required Product Response |
| :---- | :---- | :---- |
| **No measured baseline** | Cannot prove improvement or ROI. | Measure current workflow time, cost, error, and user burden before building. |
| **No evaluation path** | Model quality cannot be validated or regressed. | Build a seed golden set and review rubric proportional to risk. |
| **Superior deterministic alternative** | AI adds cost and uncertainty where rules would work. | Route to conventional software design. |
| **Unpermissioned or inaccessible data** | Creates privacy, legal, security, or hallucination risk. | Complete data governance and access work first. |
| **High-impact irreversible action without human gate** | Probabilistic system can cause unrecoverable harm. | Block automation; require human approval and deterministic verification. |
| **Unviable cost-to-serve** | Feature cannot survive production usage. | Redesign route, reduce scope, add caching/batching, or reject. |
| **User review burden exceeds value** | AI shifts work rather than reducing it. | Redesign product surface or reject use case. |
| **No operational owner** | Drift, failures, and incidents become orphaned. | Assign owner, runbook, telemetry, and review cadence before pilot. |

## **7. The User Tolerance Model**

User tolerance defines how much latency, uncertainty, correction work, review burden, and workflow interruption a user will accept in exchange for AI assistance. Product fit depends on the user’s task context, consequence of error, existing workflow habits, and perceived control.

```text
USER TOLERANCE =
  latency tolerance
+ uncertainty tolerance
+ correction tolerance
+ review burden tolerance
+ consequence tolerance
+ workflow interruption tolerance
```

### **Tolerance Dimensions**

| Dimension | Low Tolerance Looks Like | High Tolerance Looks Like | Product Implication |
| :---- | :---- | :---- | :---- |
| **Latency** | User expects instant or near-instant response. | User accepts async processing or long-running analysis. | Choose autocomplete, deterministic route, queue, or async review surface accordingly. |
| **Uncertainty** | User needs exact, stable, repeatable outputs. | User accepts variation, drafts, alternatives, or exploration. | Use deterministic software for exactness; use AI for ambiguous or creative work. |
| **Correction Work** | User will not edit, audit, or repair output. | User expects to review and refine. | Avoid draft-heavy surfaces for low-correction users. |
| **Review Burden** | Verification is expensive or cognitively draining. | Verification is natural and already part of the workflow. | Use evidence displays and structured review only where review is valuable. |
| **Consequence of Error** | Errors create legal, financial, safety, privacy, or trust damage. | Errors are low-impact and reversible. | Increase evidence, approval, and human authority as consequence rises. |
| **Workflow Interruption** | User cannot leave primary task surface. | User can work in a dedicated review cockpit. | Inline UI for low interruption; separate cockpit for high-review workflows. |

### **Product Surface Matrix**

| User / Context | Tolerance Profile | Recommended Product Surface | Required Controls |
| :---- | :---- | :---- | :---- |
| **Writer / Creative Worker** | High variation tolerance; moderate latency tolerance; high edit tolerance. | Editor canvas, draft variants, rewrite controls. | Version history, easy undo, style controls, user-owned final text. |
| **Software Engineer** | Low latency tolerance; low syntax-error tolerance; moderate correction tolerance. | Inline autocomplete, code-aware side panel, test-backed suggestions. | Compile/test hooks, diff view, dependency awareness, no blind execution. |
| **Customer Support Agent** | Low false-fact tolerance; low-to-medium latency tolerance; moderate edit tolerance. | Suggested reply card, ticket summary, source-linked answer. | Human send action, citation to customer record, policy snippets, edit capture. |
| **Corporate Lawyer / Compliance Reviewer** | Zero fake-citation tolerance; high review tolerance; accepts async analysis. | Evidence review cockpit, citation grid, risk checklist. | Source-of-record links, line-level evidence, independent human decision. |
| **Financial Analyst** | Zero calculation tolerance; high audit tolerance; medium latency tolerance. | Exception checklist, deterministic calculation panel, AI explanation layer. | Calculations in code/database, audit trail, human approval for conclusions. |
| **Operations Manager** | Low latency tolerance for triage; low manual review tolerance for benign events. | Background router, queue manager, escalation dashboard. | Confidence thresholds, exception queue, monitoring, rollback route. |
| **Voice User** | Very low conversational latency tolerance; low tolerance for awkward pauses. | Streaming voice interface or hybrid voice/text surface. | Barge-in handling, confirmation for high-impact actions, transcript review. |

### **Voice Latency as a Special Case**

Realtime voice has unusually strict interaction constraints. Users experience long pauses as conversational failure. However, voice speed must not outrun safety, confirmation, or audit needs.

| Voice Architecture | Strength | Risk | Product Rule |
| :---- | :---- | :---- | :---- |
| **Chained STT → LLM → TTS** | Modular, auditable, easier to govern. | Higher latency and turn-taking friction. | Use where auditability and control matter more than naturalness. |
| **Native speech-to-speech** | Lower latency and more natural interaction. | Harder to inspect intermediate reasoning and policy state. | Use carefully for low-risk or strongly gated workflows. |
| **Co-located modular stack** | Balances latency and auditability. | Higher infrastructure complexity. | Best for enterprise voice where both speed and control matter. |

The product surface should be selected by user tolerance and consequence, not by fascination with the most conversational interface.

## **8. The Evaluation Clarity Checklist**

A probabilistic feature is not product-ready until the team can define what success means, how failure will be detected, and what happens when confidence drops. “It feels smarter” is not an evaluation architecture. It is a horoscope with a GPU bill.

### **Evaluation Readiness Checklist**

| # | Readiness Item | Required Artifact | Risk-Tier Notes |
| :---- | :---- | :---- | :---- |
| **1** | **Expected output defined** | Schema, rubric, acceptance criteria, or source-of-record predicate. | High-risk workflows require explicit invalid-output handling. |
| **2** | **Human baseline measured** | Current task time, error rate, rework rate, cost, and user pain. | Without baseline, ROI and quality claims are guesswork. |
| **3** | **Representative test set assembled** | Seed golden set or evaluation corpus proportional to risk. | Low-risk may start small; regulated/high-impact needs stronger expert validation. |
| **4** | **Metrics selected by task type** | Extraction, retrieval, generation, tool-use, review, and business metrics. | LLM-as-judge may assist but should not be the only validator for high-risk tasks. |
| **5** | **Failure taxonomy defined** | Hallucination, omission, citation failure, wrong action, unsafe output, privacy leak, bad routing, etc. | Failure classes should map to severity and remediation. |
| **6** | **Escalation behavior specified** | Block, retry, ask clarification, route to human, degrade, or fail closed. | High-impact workflows require explicit human or deterministic gates. |
| **7** | **Feedback capture designed** | Accept, reject, edit, override reason, source correction, user confidence. | Capture sensitive content through secure references and retention policy. |
| **8** | **Release threshold defined** | Minimum quality, maximum failure rate, cost-to-serve ceiling, latency target. | Thresholds should be risk-tiered, not universal. |
| **9** | **Production monitoring planned** | Live drift metrics, user correction trends, cost, latency, incident triggers. | Monitoring must connect to owner and runbook. |
| **10** | **Business outcome mapped** | KPI tied to cycle time, throughput, error reduction, revenue, retention, or risk reduction. | Vanity engagement metrics are insufficient. |

### **Metric Families by Task Type**

| Task Type | Useful Metrics | Weak or Insufficient Metrics Alone |
| :---- | :---- | :---- |
| **Extraction** | Exact match, field-level precision/recall, schema validity, missing-field rate. | Overall thumbs-up score. |
| **Classification / Routing** | Precision, recall, confusion matrix, escalation rate, false-negative cost. | Accuracy alone when classes are imbalanced. |
| **Retrieval / RAG** | Context recall, citation validity, answer support, source permission correctness. | Similarity score alone. |
| **Summarization / Drafting** | Claim support, omission rate, edit distance to accepted version, human acceptance reason. | BLEU/ROUGE alone. |
| **Tool Use / Agentic Action** | Task completion, source-of-record verification, idempotency, action error rate. | Model-reported success. |
| **Decision Support** | Evidence completeness, risk identification, calibration, human decision quality. | Persuasiveness or fluency. |
| **Business Outcome** | Cycle-time reduction, rework reduction, throughput, margin, support deflection, risk reduction. | Session count or novelty engagement. |

### **Evaluation Gate Outcomes**

| Evaluation Result | Product Decision |
| :---- | :---- |
| **Clear improvement with acceptable risk and cost** | Proceed to pilot or production design. |
| **Quality acceptable but cost too high** | Redesign routing, caching, model size, or task scope. |
| **Quality promising but eval weak** | Continue evaluation design before pilot. |
| **Output useful but verification burden too high** | Redesign product surface or reject. |
| **High-risk failure modes unresolved** | Block production; add human gate or deterministic alternative. |
| **No measurable baseline improvement** | Reject or return to workflow discovery. |

Evaluation clarity is a prerequisite to product architecture. If the team cannot say what good means, it is not ready to build.

## **9. The Data Readiness Model**

AI product readiness is use-case-specific. An organization can have excellent analytics data and still be unready for a specific AI workflow if the needed sources are inaccessible, stale, unpermissioned, unrepresentative, or impossible to evaluate.

Data readiness must be assessed before model selection. Poor data readiness does not merely reduce accuracy; it creates hallucination, leakage, evaluation failure, user distrust, and product abandonment.

### **Core Dimensions of Data Readiness**

| Dimension | Product Question | Required Evidence |
| :---- | :---- | :---- |
| **Availability** | Can the system access the required data at the moment of use? | API availability, latency, uptime, retrieval path, fallback behavior. |
| **Permission and Governance** | Is the system allowed to use this data for this user, tenant, purpose, and model route? | ACL/RLS policy, data classification, consent/contract basis, tenant boundary. |
| **Quality and Completeness** | Is the data accurate, deduplicated, complete, and fit for the task? | Data quality report, missingness analysis, duplicate detection, field validation. |
| **Structure and Parseability** | Can the system reliably transform the data into usable context or features? | Schema, parser output, OCR/layout confidence, metadata, chunking policy. |
| **Lineage and Provenance** | Can the system prove where the data came from and how it changed? | Source IDs, version history, transformation lineage, lifecycle state. |
| **Freshness and Conflict State** | Is the data current, and what happens when sources disagree? | Freshness metadata, source-of-record hierarchy, conflict policy. |
| **Representativeness** | Does the data match the real production population and edge cases? | Sampling analysis, historical cases, bias/coverage review. |
| **Evaluation Data** | Does the team have examples with trusted target outputs? | Golden set, review rubric, expert-labeled cases, failure examples. |
| **Retention and Deletion** | Can data be retained, archived, or deleted according to policy? | Retention schedule, deletion workflow, secure evidence references. |
| **Operational Ownership** | Who owns fixes when data breaks? | Data owner, SLA, escalation path, incident workflow. |

### **Data Readiness Maturity Model**

| Level | Readiness State | Characteristics | Allowed Product Use | Required Next Step |
| :---- | :---- | :---- | :---- | :---- |
| **Level 0** | **Unknown** | Data sources, owners, permissions, and quality are undocumented. | No AI use. | Inventory sources and owners. |
| **Level 1** | **Accessible but Ungoverned** | Data can be reached, but permissions, lineage, quality, or retention are unclear. | Offline exploration only with safe data. | Establish governance and classification. |
| **Level 2** | **Prototype Ready** | Data sample exists; basic permissions and quality issues are known. | Local prototype or evaluation sandbox. | Build parser, lineage, and seed eval set. |
| **Level 3** | **Pilot Ready** | Permissions, source lineage, parser behavior, and eval set exist for bounded scope. | Limited pilot with human review. | Add monitoring, freshness, and incident handling. |
| **Level 4** | **Production Ready** | Data is governed, permissioned, versioned, monitored, and evaluable. | Production AI workflow within approved risk tier. | Continuous quality, drift, freshness, and access monitoring. |
| **Level 5** | **Adaptive / Governed at Scale** | Data lifecycle, evals, access controls, telemetry, and feedback loops are automated. | Scaled production and agentic workflows where otherwise appropriate. | Periodic governance and product-value review. |

### **Data Readiness Kill-Switches**

| Kill-Switch | Product Decision |
| :---- | :---- |
| Required data is unpermissioned or legally unclear. | Stop. Complete governance review before model work. |
| Required data is inaccessible at runtime. | Redesign workflow, API, or data pipeline. |
| Source authority is unclear or conflicting. | Define source-of-record hierarchy before launch. |
| Tenant isolation cannot be enforced. | Block production use. |
| No evaluation examples exist. | Build seed evaluation set before pilot. |
| Data is stale and freshness cannot be detected. | Restrict to low-risk or non-current workflows until fixed. |

## **10. The Risk-to-Product Surface Map**

Risk must shape the product surface. The same model behavior may be acceptable as a private draft, dangerous as a recommended decision, and unacceptable as an autonomous action. Product architecture should map risk tier to autonomy, interface, evidence, review, and execution authority.

| Risk Tier | Failure Consequence | Maximum AI Authority | Recommended Product Surface | Evidence Requirement | Execution Control |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Negligible** | Cosmetic, stylistic, or reversible formatting issue. | Suggest or apply reversible changes. | Inline suggestion, editor assist, low-friction autocomplete. | Basic trace and undo history. | User can undo or override immediately. |
| **Low** | Minor inconvenience, benign misrouting, easily corrected internal error. | Delegate bounded task or suggest action. | Suggested draft card, queue route, structured form assist. | Confidence, reason, source field, or simple trace where useful. | Human can reject, edit, undo, or reroute. |
| **Medium** | Meaningful business error, customer-facing mistake, or workflow delay. | Draft, classify, summarize, or recommend; human approves. | Side-by-side review, checklist, evidence-linked summary. | Source links, diff view, validation result, uncertainty display. | Explicit human approval before external send/write. |
| **High** | Legal, financial, privacy, safety, or compliance exposure. | Decision support only; no autonomous execution. | Evidence cockpit, risk register, exception checklist. | Line-level grounding, source-of-record verification, audit trail. | Human decision required; override reason captured. |
| **Extreme** | Physical harm, insolvency, irreversible transaction, regulated enforcement action, or systemwide security risk. | AI may organize evidence only; action authority blocked. | Manual control surface with optional evidence support. | Complete evidence package, deterministic checks, multi-party review. | Multi-human authorization or manual-only execution. |

### **Surface Design Rules**

| Rule | Meaning |
| :---- | :---- |
| **Higher risk requires less autonomy.** | Do not compensate for high consequence with prettier explanations. |
| **Evidence must become more concrete as risk rises.** | Move from confidence hints to source links to source-of-record verification. |
| **Execution is a separate permission.** | A model that can draft a decision is not automatically allowed to execute it. |
| **Undo is not enough for high impact.** | Some actions require pre-approval, not post-hoc correction. |
| **The UI must communicate system role.** | Users should know whether AI is suggesting, drafting, reviewing, deciding, or acting. |
| **Trust repair must be designed.** | Corrections, appeals, overrides, and audit trails are product features, not support leftovers. |

## **11. The ROI and Value Design Model**

AI value is not created by generating outputs. It is created when the product improves a measurable workflow outcome after accounting for serving cost, verification cost, failure cost, adoption cost, and value capture. Time saved is not automatically money saved. A feature can reduce drafting time while increasing review burden, liability, support load, or infrastructure cost.

### **Risk-Adjusted Value Formula**

```text
V_net =
  V_captured
- C_serve
- C_verify
- C_failure_expected
- C_adoption
- C_operational_change
```

Where:

| Variable | Meaning | Examples |
| :---- | :---- | :---- |
| **V_captured** | Value the organization actually captures from the improved workflow. | Paid usage, reduced support tickets, faster cycle time converted to capacity, fewer errors, retained customers. |
| **C_serve** | Cost to run the AI system. | Model calls, retrieval, tools, hosting, storage, telemetry, evals. |
| **C_verify** | Cost of human or automated validation. | Review time, approval queues, expert audit, QA sampling. |
| **C_failure_expected** | Expected remediation cost from model failures. | `P_failure × C_remediation`, including support, legal, refunds, rework, reputation, incidents. |
| **C_adoption** | Cost to change behavior. | Training, workflow redesign, documentation, incentives, migration. |
| **C_operational_change** | Cost to operate the new system over time. | Runbooks, monitoring, drift review, vendor management, governance, incident response. |

### **Baseline-Delta Model**

Product teams should calculate value as a delta from the current workflow:

| Baseline Metric | AI System Metric | Value Question |
| :---- | :---- | :---- |
| Current cycle time | AI-assisted cycle time including review | Did total task time actually fall? |
| Current error rate | AI error + human override + missed-error rate | Did quality improve after verification burden? |
| Current cost per task | AI cost per verified task | Did unit economics improve? |
| Current user effort | AI interaction + correction + review effort | Did the product reduce cognitive load? |
| Current throughput | AI-assisted throughput | Did capacity increase without quality collapse? |
| Current risk exposure | AI residual risk exposure | Did the system reduce or increase downside? |

### **Value Capture Modes**

| Value Mode | Valid When | Common Trap |
| :---- | :---- | :---- |
| **Cycle-Time Reduction** | Faster work creates usable capacity or revenue. | Saved minutes do not translate to actual economic value. |
| **Error Reduction** | AI catches mistakes humans miss or standardizes quality. | AI creates new silent errors that are harder to detect. |
| **Throughput Expansion** | Team can handle more cases without proportional staffing. | More throughput creates more downstream review bottlenecks. |
| **Expertise Democratization** | Non-experts safely perform bounded expert-adjacent tasks. | Product hides uncertainty and encourages overconfidence. |
| **Support Deflection** | Users self-serve accurately with fewer tickets. | Bad answers generate escalations and trust damage. |
| **Risk Reduction** | System improves compliance, auditability, or detection. | Audit trail exists but decision quality does not improve. |
| **Revenue Expansion** | AI unlocks a differentiated paid feature. | Feature is easy to copy or too costly to serve. |

### **ROI Gate**

| Condition | Product Decision |
| :---- | :---- |
| Positive value only under optimistic assumptions. | Run discovery or pilot; do not scale. |
| Value depends on human review being unrealistically fast. | Redesign review surface or reject. |
| Cost-to-serve exceeds margin envelope at expected usage. | Change route, scope, pricing, or reject. |
| AI improves local metric but worsens downstream workflow. | Redesign workflow architecture. |
| Value is measurable, captured, and robust under sensitivity analysis. | Proceed to production design. |

## **12. The Cost-to-Serve Model**

The long-term viability of an AI product is governed by unit economics. Aggregate cloud spend is too blunt. Product architecture needs cost per tenant, request, task, verified task, route, and customer segment.

Static model rate cards should not be embedded as durable doctrine. Provider pricing changes, discounts vary, context tiers differ, cached-token pricing may apply, batch modes may reduce cost, and enterprise agreements can override public rates. Canon should preserve the cost model and rate-card snapshot method, not fossilize one month’s vendor catalog.

### **Cost Allocation Formulas**

```text
Cost_per_tenant =
  (allocated_infrastructure_cost
 + allocated_model_api_cost
 + allocated_storage_cost
 + allocated_observability_cost
 + allocated_support_and_review_cost)
 / active_tenants
```

```text
Cost_per_request =
  request_model_cost
+ retrieval_cost
+ tool_cost
+ storage_and_trace_cost
+ safety_and_eval_cost
+ allocated_infrastructure_cost
```

```text
Cost_per_verified_task =
  total_cost_for_workflow_attempts
  / count(verified_successful_tasks)
```

```text
Cost_waste_ratio =
  cost_of_failed_retried_rejected_or_unverified_work
  / total_ai_workflow_cost
```

### **Cost Components**

| Component | Examples | Product Question |
| :---- | :---- | :---- |
| **Input tokens** | Prompt, context, retrieved chunks, memory, tool descriptions. | Is context bloat driving cost? |
| **Output tokens** | Generated answer, reasoning trace where billed, draft variants. | Is verbosity necessary for value? |
| **Cached tokens** | Prefix cache, semantic cache, provider cache. | Are repeated workloads being deflected safely? |
| **Retrieval** | Embeddings, vector search, reranking, document parsing. | Is evidence retrieval proportionate to task value? |
| **Tool calls** | SaaS APIs, browser actions, database writes, external services. | Are tools bounded and verified? |
| **Review labor** | Human approval, expert validation, QA sampling. | Does verification erase time savings? |
| **Telemetry and evals** | Traces, logs, dashboards, regression runs, shadow evals. | Is operational overhead proportional to risk? |
| **Retries and failures** | Repair loops, rejected outputs, failed tool calls. | Is poor reliability creating hidden COGS? |
| **Infrastructure** | Hosted model pools, GPUs, queues, storage, networking. | Is capacity right-sized and utilized? |

### **Rate-Card Snapshot Template**

Use this table when evaluating a specific product decision. It should be dated and treated as a snapshot.

| Field | Value |
| :---- | :---- |
| **Date checked** |  |
| **Pricing source** |  |
| **Provider** |  |
| **Model / route name** |  |
| **Input price per 1M tokens** |  |
| **Cached input price per 1M tokens** |  |
| **Output price per 1M tokens** |  |
| **Batch discount** |  |
| **Priority / latency premium** |  |
| **Data residency or compliance premium** |  |
| **Context window tier** |  |
| **Tool or hosted feature charges** |  |
| **Enterprise discount assumption** |  |
| **Notes / caveats** |  |

### **Cost-to-Serve Gate**

| Cost Finding | Product Decision |
| :---- | :---- |
| Cost per verified task is below margin target and stable under expected usage. | Proceed. |
| Cost is acceptable only with caching or routing assumptions. | Pilot with explicit telemetry and kill switch. |
| Cost explodes under retries, long context, or agent loops. | Redesign architecture before launch. |
| Cost exceeds willingness-to-pay or margin envelope. | Change pricing, scope, route, or reject. |
| Cost cannot be measured at task level. | Build cost telemetry before scaling. |

Cost-to-serve must be evaluated at the workflow level. A cheap model call can still produce an expensive product if it causes retries, review burden, bad routing, or user abandonment.

## **13. AI Product Pattern Taxonomy**

AI product patterns combine product surface, autonomy level, evaluation method, risk controls, and cost profile. A pattern is not just a UI shape. It defines what the system is allowed to do and how users verify, correct, trust, or reject its behavior.

| Pattern | Best-Fit Workflows | Product Surface | Authority Level | Evaluation Method | Required Controls | Common Anti-Pattern |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Assistant** | Brainstorming, exploratory search, research support, low-risk drafting. | Chat or conversational workspace. | Suggests and explains. | Task success, answer support, user outcome, correction patterns. | Source display where factual, memory boundaries, clear limitations. | Using chat for structured tasks better served by forms or filters. |
| **Copilot** | Code, writing, form completion, active task support. | Inline suggestions, ghost text, side panel. | Suggests; user accepts. | Acceptance rate, edit distance, compile/test result, task completion. | Easy reject/undo, context boundary, validation hooks. | Encouraging blind acceptance in high-risk work. |
| **Autofill / Extraction** | Invoices, forms, CRM fields, receipts, structured document intake. | Field-level suggestions or extracted JSON. | Drafts structured fields. | Exact match, field-level F1, schema validity, missing-field rate. | Human review for material fields, schema validation, source highlights. | Direct database write without review or confidence gating. |
| **Reviewer / Checker** | Contract review, compliance gap detection, security triage, QA. | Risk panel, checklist, highlighted evidence. | Flags issues; does not execute. | Precision/recall, false-negative rate, expert review. | Evidence links, severity labels, reviewer override capture. | Letting checker mutate the source artifact. |
| **Retriever / Synthesizer** | Knowledge search, policy navigation, research synthesis. | Answer with citations, evidence packet, source panel. | Synthesizes from evidence. | Context relevance, citation validity, answer support, freshness. | Permission filters, citation verification, conflict disclosure. | Ingesting unmanaged corpora and trusting generated citations. |
| **Triage / Router** | Support tickets, alerts, intake queues, lead routing. | Invisible or queue-facing classifier. | Routes or prioritizes bounded items. | Confusion matrix, escalation rate, false-negative cost. | Confidence threshold, manual queue, monitoring by class. | Using expensive reasoning models where metadata/rules suffice. |
| **Advisor** | Planning, diagnosis support, risk analysis, expert decision support. | Evidence cockpit, option matrix, scenario panel. | Recommends and structures tradeoffs. | Expert rubric, decision quality, evidence completeness, calibration. | Human final decision, source-of-record links, uncertainty disclosure. | Allowing advisor to execute financial, legal, or medical actions. |
| **Agent** | Bounded cross-application workflows, scheduling, information gathering, low-risk automation. | Task console, plan trace, approval checkpoints. | Executes bounded steps under policy. | Task completion, tool success, verification, intervention rate. | Tool contracts, idempotency, action verification, loop and budget caps. | Unmonitored agent with broad read/write access. |
| **Background Automation** | Spam filtering, benign ranking, deduplication, low-risk routing. | Mostly invisible with settings/appeal surface. | Acts automatically inside bounded scope. | A/B tests, precision/recall, complaint rate, drift monitoring. | User override, audit trace, fallback rules, monitoring. | Hidden manipulation with no user control or appeal. |
| **Evidence-Only AI** | Extreme-risk review, regulated decisions, safety-critical analysis. | Read-only evidence organization surface. | Organizes evidence only; cannot decide or execute. | Evidence completeness, source validity, reviewer usefulness. | Manual authority, deterministic controls, multi-party approval where required. | Treating generated synthesis as authorization. |

Product patterns should be selected after use-case scoring, risk mapping, and user tolerance analysis. The same model can power multiple patterns, but each pattern grants different authority and requires different controls.

## **14. The No-AI Decision Framework**

The defining characteristic of an expert-level AI product architect is the ability to confidently identify when a proposed feature must *not* use artificial intelligence. Rejecting AI in favor of deterministic, robust alternatives is a marker of architectural success, preventing major infrastructure expenses, operational risk, and long-term maintenance burdens.4

### **Deterministic & Relational Triggers**

A proposed product capability must be routed to traditional software engineering development queues (fully bypassing machine learning pipelines) if it matches any of the triggers detailed below:

| Deterministic Trigger Type | Specific Core Criteria | Optimal Software Alternative | Architectural Justification |
| :---- | :---- | :---- | :---- |
| **Exactness Requirements** | Task outputs require absolute mathematical accuracy or adhere to strict compliance rules. | Relational SQL database lookups, transactional rules engines, standard arithmetic calculators.4 | Probabilistic systems operate with statistical variance; they cannot guarantee exact calculations or deterministic logic. |
| **High Validation Costs** | Verifying if a model output is correct is so cognitively demanding that it matches or exceeds the cost of manual work.9 | Standard templates, structured boilerplate builders, and relational database validation fields. | "Verification Debt" erodes the product's entire value proposition, leading to user fatigue and system abandonment.9 |
| **Low Data Readiness** | The required training, fine-tuning, or RAG context data is absent, un-structured, or legally unpermissioned.21 | Clean web form design, structured API metadata capture, and manual data-entry validation flows.22 | AI cannot synthesize data out of thin air; clean structures must exist before deploying cognitive capabilities.22 |
| **Ultra-Low Latency Needs** | Task execution times must remain strictly sub-50ms (e.g., search indexing, standard data table sorting).20 | Standard indexing algorithms, relational search indexing, and client-side JavaScript filters.5 | Large model inference, network routing, and retrieval runs require massive compute budgets that cannot fit within sub-50ms constraints.20 |
| **Friction Out of System** | Process delays are driven by bad organizational incentives, slow human approvals, or outdated software APIs.4 | System process re-engineering, REST API pipeline construction, and user incentive redesigns.4 | Inserting a model on top of a structurally broken process adds complexity without fixing the underlying system constraints.4 |
| **Unviable Gross Margins** | The long-context token, hosting, and retrieval compute costs represent > 30% of the task's transaction value.8 | Traditional, lightweight programming scripts, localized regex parsers, and static templates.8 | Unit economic viability is the backbone of sustainable product growth; unviable features must be built deterministically.8 |

## **15. Product Discovery and Experiment Design Model**

Before scaling production AI, product teams should run discovery experiments that validate workflow pain, user tolerance, data readiness, evaluation clarity, adoption behavior, and cost-to-serve. AI experiments must test the product system, not merely the model demo.

### **Concierge vs. Wizard of Oz**

| Experiment | User Knows What? | Best For | Cannot Prove | Governance Constraint |
| :---- | :---- | :---- | :---- | :---- |
| **Concierge Test** | User knows a human is delivering the service manually. | Validating whether the proposed value actually matters. | Whether users will trust or adopt an automated interface. | Be clear that the service is human-delivered. |
| **Wizard of Oz Test** | User experiences a simulated automated interface while humans perform some backend work. | Validating interface behavior, willingness to use, trust, and workflow fit. | Real model performance, latency, cost, or automation reliability. | Must respect research ethics, privacy, consent, safety, and domain risk. |
| **Hybrid Model-Assisted Pilot** | User or operator knows AI is assisting but bounded by human review. | Learning model limits and correction patterns safely. | Fully autonomous scalability. | Capture edits, failures, and review outcomes with appropriate retention. |
| **Offline Evaluation** | No live user interaction. | Measuring model/task performance on historical cases. | Adoption, workflow fit, or user tolerance. | Use permissioned, representative data. |
| **Internal Controlled Pilot** | Internal users know they are testing a bounded system. | Measuring real workflow impact with limited blast radius. | General market adoption. | Monitor quality, cost, feedback, and incident signals. |

Wizard of Oz experimentation is not a license to deceive users in high-impact contexts. The higher the consequence, the more explicit the disclosure, consent, and review controls must be.

### **Discovery Experiment Taxonomy**

| Experiment Type | What It Proves | Key Metrics | Best Exit Decision |
| :---- | :---- | :---- | :---- |
| **Workflow Observation** | Where bottlenecks, workarounds, and hidden labor exist. | Cycle time, handoff count, rework, interruption, user frustration. | Redesign workflow or select target task. |
| **User Interview** | Whether pain is real and meaningful. | Pain frequency, current workaround, willingness to change. | Continue discovery or reject weak pain. |
| **Concierge MVP** | Whether the proposed service creates value. | Retention, willingness to pay, qualitative delight, task completion. | Define product surface and minimum feature set. |
| **Wizard of Oz MVP** | Whether users behave as expected inside the simulated surface. | Drop-off, trust breakdowns, choices, correction behavior. | Redesign interface or proceed to prototype. |
| **Hybrid Draft-and-Approve Pilot** | Where model output helps, fails, or burdens the user. | Accept rate, edit rate, rejection rate, time saved, failure taxonomy. | Build evals and route policy. |
| **Offline Model Eval** | Whether candidate models/routes meet task thresholds. | Precision, recall, groundedness, schema validity, cost, latency. | Select route or reject model approach. |
| **Internal Pilot** | Whether workflow, operations, and adoption survive real use. | Task delta, quality delta, cost-to-serve, incident rate, user retention. | Proceed, redesign, or stop. |

### **Experiment Design Rules**

| Rule | Meaning |
| :---- | :---- |
| **Test one uncertainty at a time where possible.** | Do not mix model quality, UI design, pricing, and workflow change into one unreadable experiment. |
| **Capture correction data.** | Edits and rejections are product gold; they reveal evaluation cases and workflow mismatch. |
| **Measure total task time.** | Draft speed is irrelevant if review time grows. |
| **Use real-enough data safely.** | Synthetic demos often hide permission, format, edge-case, and messiness problems. |
| **Protect users from high-impact simulation.** | Do not simulate autonomous authority in contexts where users could be harmed or misled. |
| **Define stop conditions.** | Experiments should be able to kill bad ideas cleanly. |

## **16. The Adoption and Change Model**

AI product success is not defined at launch. It is defined when users integrate the system into durable work habits without overtrusting, ignoring, bypassing, or being cognitively dulled by it. Adoption is a behavioral architecture problem.

### **Adoption States and Interventions**

| User State | Observable Signals | Product Risk | Product Intervention | Change Management Response |
| :---- | :---- | :---- | :---- | :---- |
| **Healthy Calibration** | Users accept useful suggestions, reject bad ones, and understand system limits. | Low. | Maintain evidence, correction, and feedback surfaces. | Reinforce best practices and share quality wins. |
| **Overtrust / Complacency** | Users accept outputs with little review; errors propagate downstream. | Silent quality collapse. | Require evidence display, independent judgment step, review prompts, confidence calibration. | Train users on failure modes; reward error catching. |
| **Undertrust / Rejection** | Users ignore feature, revert to manual work, or disable suggestions. | No adoption; wasted investment. | Improve transparency, reliability feedback, undo, and user control. | Involve users in rubric design and pilot iteration. |
| **Motivational Withdrawal** | Users become passive reviewers and stop thinking critically. | Reduced expertise and poor final quality. | Use sequential teaming: human frames problem first, AI assists second. | Redefine roles around expert judgment and final accountability. |
| **Workflow Circumvention** | Users move to unapproved consumer tools or manual side channels. | Shadow AI, data leakage, inconsistent process. | Put useful AI inside the actual workflow with lower friction. | Improve approved tools and procurement path. |
| **Correction Fatigue** | Users spend too much time editing or auditing outputs. | Abandonment or rubber-stamping. | Reduce scope, improve evals, show diffs, route only high-confidence cases. | Reassess value model and staffing assumptions. |
| **Status Threat** | Users fear replacement, deskilling, or surveillance. | Resistance and sabotage. | Make AI role explicit: assistive, bounded, accountable. | Communicate role changes, incentives, and career path. |

### **Adoption Metrics**

| Metric | What It Shows |
| :---- | :---- |
| **Activation Rate** | Whether users try the feature after exposure. |
| **Repeat Use** | Whether the feature becomes habit. |
| **Acceptance / Rejection / Edit Rate** | Whether suggestions are useful and appropriately trusted. |
| **Time-to-Verified-Completion** | Whether total task time improves after review. |
| **Override Reason Distribution** | Why users reject or repair output. |
| **Manual Bypass Rate** | Whether users leave the approved workflow. |
| **Error Catch Rate** | Whether humans remain meaningfully engaged. |
| **Support / Complaint Rate** | Whether the product creates confusion or harm. |
| **Skill Retention Signal** | Whether users can still perform core tasks without AI when needed. |

### **Change Model Requirements**

| Requirement | Purpose |
| :---- | :---- |
| **Role clarity** | Users must know whether AI drafts, reviews, routes, recommends, or acts. |
| **Accountability clarity** | Human and organizational responsibility must be explicit. |
| **Training on failure modes** | Users need examples of plausible AI errors, not just feature demos. |
| **Feedback loops** | Corrections must improve evals, routing, prompts, or product design. |
| **Incentive alignment** | Users should be rewarded for quality and judgment, not blind throughput. |
| **Shadow-AI alternative** | Approved tools must be good enough that users do not flee to unsanctioned ones. |
| **Periodic recalibration** | Adoption patterns should be reviewed as model behavior, workload, and user skill change. |

Adoption is not “users clicked the sparkle button.” Adoption is sustained, safe, value-producing integration into real work.

## **17. Cross-Canon Handoff Map**

AI-ENG-AF opens the Product, Business, and Organizational Architecture volume. It acts as the product gate before implementation: selecting use cases, rejecting bad fits, mapping workflows, defining value, choosing product surfaces, and determining whether AI belongs at all.

AF consumes technical constraints from earlier canon layers and hands product decisions forward into business model, adoption, and enterprise transformation architecture.

### **Upstream Canon Inputs**

| Canon Report | What AF Consumes | Product Architecture Use |
| :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context windows, memory, state, authority, and prompt/context boundaries. | Determines whether the workflow can fit into safe and usable context structures. |
| **AI-ENG-D — Corpus Engineering** | Data ownership, provenance, lifecycle, metadata, and corpus quality. | Evaluates data readiness and corpus suitability for product use. |
| **AI-ENG-E — Retrieval Pipeline** | Retrieval feasibility, citation support, ranking, and evidence quality. | Determines whether RAG can support the user promise. |
| **AI-ENG-F — Freshness and Conflict Detection** | Source freshness, conflicts, and source-of-record hierarchy. | Determines whether product outputs can stay current and trustworthy. |
| **AI-ENG-J — Throughput Mechanics** | Latency, batching, queueing, token throughput. | Shapes user tolerance, cost-to-serve, and surface choice. |
| **AI-ENG-K — Weight Dynamics** | Model size, quantization, adaptation, capability boundaries. | Informs model suitability and route-class selection. |
| **AI-ENG-L — Serving Architecture** | Routing, provider profiles, fallback, caching, scale. | Informs feasibility, latency, cost, and operational readiness. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, side effects, auth, idempotency. | Determines whether agentic or tool-using product patterns are viable. |
| **AI-ENG-O — Action Verification** | State verification, action ledgers, false-success prevention. | Defines whether automation, delegation, or human approval is required. |
| **AI-ENG-S — Production Pathologies** | Common AI failure modes and brittle-chain behavior. | Feeds fatal-flaw and risk-surface design. |
| **AI-ENG-T — Boundary Defense** | Tenant isolation, prompt injection, egress, policy hierarchy. | Determines product risk tier and required security constraints. |
| **AI-ENG-V — Resource Abuse** | Cost bombs, loop controls, denial-of-wallet. | Feeds cost-to-serve and agent pattern constraints. |
| **AI-ENG-W — UX Resilience** | Degraded modes, continuity, fallback, user state. | Shapes user tolerance and failure UX. |
| **AI-ENG-X — User Trust** | Transparency, calibration, contestability, evidence display. | Shapes product surface, trust repair, and adoption design. |
| **AI-ENG-Y — Human Review** | Review queues, escalation, reviewer quality, approval patterns. | Determines augmentation, delegation, and decision-support boundaries. |
| **AI-ENG-Z — Telemetry** | Traceability, cost, latency, behavior, user correction metrics. | Defines product success measurement and operational readiness. |
| **AI-ENG-AA — Evaluation Architecture** | Golden sets, eval gates, rubrics, regression testing. | Determines evaluation clarity and launch readiness. |
| **AI-ENG-AC — AI Operations** | Incident response, runbooks, rollback, containment. | Determines whether product risks can be operated after launch. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, procurement, accountability, risk classification. | Defines compliance burden, approval path, and product authority limits. |
| **AI-ENG-AE — Sustainable AI** | Energy, carbon, routing, lifecycle, and cost of compute. | Adds sustainability and resource efficiency to product fit and value design. |

### **Forward Handoff to Volume 11**

| Target Report | AF Concept Handed Forward | Downstream Use |
| :---- | :---- | :---- |
| **AI-ENG-AG — Business Model, Pricing & Packaging Design** | Cost-to-serve, value model, use-case score, route class, margin risk. | Pricing tiers, usage limits, packaging, COGS controls, expansion strategy. |
| **AI-ENG-AH — Organizational Adoption, Training & Change** | User tolerance, adoption hazards, automation posture, workflow disruption. | Training, incentive design, role redesign, change management, skill retention. |
| **AI-ENG-AI — Enterprise Transformation Reference Architecture** | Workflow fit maps, product pattern, integration constraints, no-AI decisions. | Enterprise rollout patterns, system integration, API architecture, governance pathways. |
| **AI-ENG-AJ — Reference Architectures** | Approved product patterns and decision gates. | Reusable implementation blueprints and golden paths. |

### **Core Handoff Rule**

AF must happen before implementation. If the product architecture gate cannot identify workflow pain, AI suitability, data readiness, evaluation clarity, risk posture, user tolerance, cost-to-serve, and measurable value, the correct engineering action is not “try a better model.”

The correct action is to stop, redesign, run discovery, or choose No-AI.

#### **Works cited**

1. Complete Guide to Product Discovery: Methods, Frameworks, - IdeaPlan, accessed June 14, 2026, [https://www.ideaplan.io/guides/the-complete-guide-to-product-discovery](https://www.ideaplan.io/guides/the-complete-guide-to-product-discovery)  
2. What Is AI Product Management? | Galileo, accessed June 14, 2026, [https://galileo.ai/blog/ai-product-management-guide](https://galileo.ai/blog/ai-product-management-guide)  
3. AI Product Management Roadmap & Frameworks: Step-by-Step Guide - Voltage Control, accessed June 14, 2026, [https://voltagecontrol.com/articles/ai-product-management-roadmap-frameworks-step-by-step-guide/](https://voltagecontrol.com/articles/ai-product-management-roadmap-frameworks-step-by-step-guide/)  
4. Workflow Redesign and Human-AI Collaboration - Umbrex, accessed June 14, 2026, [https://umbrex.com/resources/chief-ai-officer-playbook/workflow-redesign-and-human-ai-collaboration/](https://umbrex.com/resources/chief-ai-officer-playbook/workflow-redesign-and-human-ai-collaboration/)  
5. Technology Will Make the Pace of Change Even Faster - Articles - Advisor Perspectives, accessed June 14, 2026, [https://www.advisorperspectives.com/articles/2024/12/30/technology-will-make-the-pace-of-change-even-faster](https://www.advisorperspectives.com/articles/2024/12/30/technology-will-make-the-pace-of-change-even-faster)  
6. Thinks 1499 – Rajesh Jain, accessed June 14, 2026, [https://rajeshjain.com/2025/02/07/thinks-1499/](https://rajeshjain.com/2025/02/07/thinks-1499/)  
7. Austrian economics and AI scaling - Marginal REVOLUTION, accessed June 14, 2026, [https://marginalrevolution.com/marginalrevolution/2024/11/austrian-economics-and-ai-scaling.html](https://marginalrevolution.com/marginalrevolution/2024/11/austrian-economics-and-ai-scaling.html)  
8. Kubernetes FinOps: SaaS Unit Economics | HostingX, accessed June 14, 2026, [https://hostingx.co.il/articles/kubernetes-finops-unit-economics](https://hostingx.co.il/articles/kubernetes-finops-unit-economics)  
9. Structuring Human-AI Productive Interdependence by Strategic Level of Automation Selection for Qualitative Inquiry - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2605.27634v1](https://arxiv.org/html/2605.27634v1)  
10. Structuring Human-AI Productive Interdependence by Strategic Level of Automation Selection for Qualitative Inquiry - arXiv, accessed June 14, 2026, [https://arxiv.org/pdf/2605.27634](https://arxiv.org/pdf/2605.27634)  
11. Cost-to-Serve in AI: The Most Overlooked Metric for Sustainable Margins - Mavvrik: AI, accessed June 14, 2026, [https://www.mavvrik.ai/blog/cost-to-serve-in-ai/](https://www.mavvrik.ai/blog/cost-to-serve-in-ai/)  
12. What Is AI Agent Observability? Why Cost Is The Signal You're Missing - CloudZero, accessed June 14, 2026, [https://www.cloudzero.com/blog/ai-agent-observability/](https://www.cloudzero.com/blog/ai-agent-observability/)  
13. Human–AI Collaboration Across Decision Support, Autonomous ..., accessed June 14, 2026, [https://www.mdpi.com/2071-1050/18/11/5313](https://www.mdpi.com/2071-1050/18/11/5313)  
14. Wizard of Oz | The Real Startup Book - Kromatic, accessed June 14, 2026, [https://kromatic.com/real-startup-book/4-evaluative-product-experiments/wizard-of-oz/](https://kromatic.com/real-startup-book/4-evaluative-product-experiments/wizard-of-oz/)  
15. LLM Training Data Lineage: Provenance, Tracking & Compliance - Atlan, accessed June 14, 2026, [https://atlan.com/know/training-data-lineage-for-llms/](https://atlan.com/know/training-data-lineage-for-llms/)  
16. The Impact of AI Execution Autonomy on User Task Performance and Intervention Behavior in Digital Workflows - RPubs, accessed June 14, 2026, [https://rpubs.com/Ladan/ai-execution-autonomy](https://rpubs.com/Ladan/ai-execution-autonomy)  
17. Toward Human-AI Complementarity Across Diverse Tasks - arXiv, accessed June 14, 2026, [https://arxiv.org/html/2605.04070v1](https://arxiv.org/html/2605.04070v1)  
18. A Model for Types and Levels of Human Interaction with Automation - SlideServe, accessed June 14, 2026, [https://www.slideserve.com/hila/a-model-for-types-and-levels-of-human-interaction-with-automation](https://www.slideserve.com/hila/a-model-for-types-and-levels-of-human-interaction-with-automation)  
19. [論文評述] Edge Intelligence Optimization for Large Language Model, accessed June 14, 2026, [https://www.themoonlight.io/tw/review/edge-intelligence-optimization-for-large-language-model-inference-with-batching-and-quantization](https://www.themoonlight.io/tw/review/edge-intelligence-optimization-for-large-language-model-inference-with-batching-and-quantization)  
20. The enterprise voice AI split: Why architecture — not model quality — defines your compliance posture | VentureBeat, accessed June 14, 2026, [https://venturebeat.com/security/the-enterprise-voice-ai-split-why-architecture-not-model-quality-defines](https://venturebeat.com/security/the-enterprise-voice-ai-split-why-architecture-not-model-quality-defines)  
21. An Introduction to AI-Ready Data - Alexander Thamm [at], accessed June 14, 2026, [https://www.alexanderthamm.com/en/blog/ai-ready-data/](https://www.alexanderthamm.com/en/blog/ai-ready-data/)  
22. Data Readiness Assessment for AI: Checklist, Framework, and Scoring, accessed June 14, 2026, [https://agility-at-scale.com/ai/data/data-readiness-assessment-for-ai/](https://agility-at-scale.com/ai/data/data-readiness-assessment-for-ai/)  
23. Product Frameworks: AI - Productboard, accessed June 14, 2026, [https://www.productboard.com/wp-content/uploads/2024/01/AI-Product-Framework-Guide.pdf](https://www.productboard.com/wp-content/uploads/2024/01/AI-Product-Framework-Guide.pdf)  
24. The AI-shaped hole in personal finance – Z-Connect by Zerodha, accessed June 14, 2026, [https://zerodha.com/z-connect/subtext/the-ai-shaped-hole-in-personal-finance](https://zerodha.com/z-connect/subtext/the-ai-shaped-hole-in-personal-finance)  
25. Human-Centered Artificial Intelligence: Reliable, Safe & Trustworthy - arXiv, accessed June 14, 2026, [https://arxiv.org/pdf/2002.04087](https://arxiv.org/pdf/2002.04087)  
26. Designing Human-Automation Interaction: a new level of Automation Taxonomy - HFES Europe, accessed June 14, 2026, [https://www.hfes-europe.org/wp-content/uploads/2014/06/Save.pdf](https://www.hfes-europe.org/wp-content/uploads/2014/06/Save.pdf)  
27. Chapter: 6 Human-AI Team Interaction - National Academies of Sciences, Engineering, and Medicine, accessed June 14, 2026, [https://www.nationalacademies.org/read/26355/chapter/8](https://www.nationalacademies.org/read/26355/chapter/8)  
28. Preparing Data for AI and Machine Learning: A Production-Ready Playbook - instinctools, accessed June 14, 2026, [https://www.instinctools.com/blog/preparing-data-for-ml-ai/](https://www.instinctools.com/blog/preparing-data-for-ml-ai/)  
29. LLM API Pricing Comparison In 2026: Every Major Model, Ranked By Cost - CloudZero, accessed June 14, 2026, [https://www.cloudzero.com/blog/llm-api-pricing-comparison/](https://www.cloudzero.com/blog/llm-api-pricing-comparison/)  
30. Page 97 – Marketing, Entrepreneurship, India. Updated daily. - Rajesh Jain, accessed June 14, 2026, [https://rajeshjain.com/page/97/?source=user_profile---------0-](https://rajeshjain.com/page/97/?source=user_profile---------0-)  
31. The Top AI Models And Trends Shaping SaaS in 2026 - CloudZero, accessed June 14, 2026, [https://www.cloudzero.com/blog/top-ai-models/](https://www.cloudzero.com/blog/top-ai-models/)  
32. How SaaS Companies Can Profitably Price AI Agents - CloudZero, accessed June 14, 2026, [https://www.cloudzero.com/blog/ai-agent-pricing-models/](https://www.cloudzero.com/blog/ai-agent-pricing-models/)  
33. AI Product Discovery Frameworks Overview - Productboard, accessed June 14, 2026, [https://www.productboard.com/blog/ai-product-discovery-frameworks/](https://www.productboard.com/blog/ai-product-discovery-frameworks/)  
34. Concierge vs. Wizard of Oz Experiments - Learning Loop, accessed June 14, 2026, [https://learningloop.io/blog/concierge-vs-wizard-of-oz](https://learningloop.io/blog/concierge-vs-wizard-of-oz)  
35. Two MVPs every product manager should know, and how to tell them apart, accessed June 14, 2026, [https://www.mindtheproduct.com/wizard-of-oz-vs-concierge-testing-behind-the-curtain-or-behind-the-desk/](https://www.mindtheproduct.com/wizard-of-oz-vs-concierge-testing-behind-the-curtain-or-behind-the-desk/)  
36. Wizard of Oz Experiment: Definition, How It Works, Examples, Tools, Pros and Cons, accessed June 14, 2026, [https://learningloop.io/plays/wizard-of-oz](https://learningloop.io/plays/wizard-of-oz)  
37. Can LLM Agents Simulate Customers to Evaluate Agentic-AI-based Shopping Assistants?, accessed June 14, 2026, [https://arxiv.org/html/2509.21501v1](https://arxiv.org/html/2509.21501v1)  
38. Modeling the Dynamic Agency in Human-AI Collaboration - DRS Digital Library, accessed June 14, 2026, [https://dl.designresearchsociety.org/cgi/viewcontent.cgi?article=1608&context=iasdr](https://dl.designresearchsociety.org/cgi/viewcontent.cgi?article=1608&context=iasdr)

---

# AI-ENG-AG — Adoption Systems - Training, Feedback Loops & Change Management

## **The Sociotechnical Architecture of Enterprise AI Adoption**

AI adoption systems represent the structural, organizational, and psychological transmission layer that converts a valid artificial intelligence product into a durable, safe, and value-producing human workflow. Within high-dimensional enterprise product architectures, a foundational doctrine governs this transition: AI adoption is not achieved by access. It is achieved by trained, incentivized, supported, feedback-connected humans using AI inside redesigned workflows with clear boundaries, visible value, trusted escalation paths, and correction loops that improve the system over time.1 Consequently, the primary query for an enterprise program owner is not whether users have been provisioned license access, but rather whether those users possess the structural support and behavioral telemetry required to know when to use the AI system, when to refuse it, how to verify its outputs, how to correct its failures, and how the organization systematically rewards safe, useful adoption rather than blind, complacent usage.  
To establish this architecture, organizations must transition from a pure technology deployment mindset toward sociotechnical and job design frameworks.2 Job design acts as the structural enabler of technology adoption by shaping how tasks, autonomy, and feedback are allocated across human and machine actors.4 When integrating intelligent technologies, work design parameters must be intentionally configured to prevent technology from standardizing human labor into highly specialized, deskilled micro-tasks, which systematically erodes cognitive motivation and professional agency.4 Instead, system designers should leverage AI to enhance skill variety—the degree to which a job requires a diverse range of cognitive activities, such as drafting, synthesis, ideation, troubleshooting, and iterative refinement.4 High skill variety creates repeated, high-value use cases for generative models, which naturally increases the frequency and depth of tool integration.4

### **Parker & Grote Work Design Parameters in AI Workflows**

| Work Design Parameter | Legacy Technology Pattern | Generative AI Risk State | Optimized AI Adoption State |
| :---- | :---- | :---- | :---- |
| **Skill Variety** 4 | Low variety; highly repetitive data entry or administrative execution.4 | Hyper-specialization; human reduced to a passive, repetitive "button-clicker" verifying automated outputs.4 | High cognitive variety; human orchestrates multi-agent systems, focusing on edge-case exceptions and strategic design.4 |
| **Task Discretion & Autonomy** 4 | Rigid execution paths dictated by deterministic legacy software rules.4 | Algorithmic management; machine-driven pacing, surveillance, and automated performance ranking.4 | Dynamic authority allocation; user maintains ultimate veto rights, choosing when and how to delegate tasks to AI.5 |
| **Feedback Loop Integration** 4 | Delayed performance metrics or manual supervisory reviews.4 | Algorithmic feedback loops optimized for surveillance and punitive enforcement.4 | Bidirectional, real-time feedback; telemetry tracks edit distance to improve RAG accuracy and reward human insight.9 |
| **Task Significance** 4 | Human executes isolated micro-tasks with limited visibility into systemic outcomes.4 | Demoralization; human feels their intellectual contributions are trivialized by automated generators.5 | Outcome-based significance; human is accountable for context calibration, ethical alignment, and ultimate output quality.3 |

To guide the enterprise toward a mature adoption state, the organization must assess its progress against the four stages of the MIT Center for Information Systems Research (CISR) AI Maturity Model.12 Moving through these stages requires deliberate investments in both technical capabilities and human enablement systems:

### **MIT CISR AI Maturity Model Progression**

To guide enterprise adoption, organizations can assess progress against the MIT CISR Enterprise AI Maturity Model. The model describes four broad stages: **Experiment and Prepare**, **Build Pilots and Capabilities**, **Develop AI Ways of Working**, and **Become AI Future Ready**. These stages should not be treated as a software deployment ladder alone. Each stage requires both technical capability and human adoption capability.

| Maturity Stage | Behavioral Characteristics | Adoption System Capability | Technical Capability | Change Management Intervention |
| :---- | :---- | :---- | :---- | :---- |
| **Stage 1: Experiment and Prepare** | Isolated experimentation; inconsistent prompt sharing; uneven literacy; unclear boundaries. | Basic AI literacy, acceptable-use policy, early role-specific examples. | Sandboxes, limited data access, initial model/tool exploration. | Establish steering group, define safe-use rules, identify priority workflows, and create baseline training. |
| **Stage 2: Build Pilots and Capabilities** | Bounded pilots; emerging champions; early workflow redesign; growing demand for support. | Champion network, pilot playbooks, feedback capture, initial support channel. | Approved tools, basic RAG or workflow integrations, early evals and telemetry. | Run structured pilots, protect learning time, define success metrics, and capture corrections. |
| **Stage 3: Develop AI Ways of Working** | AI becomes embedded in daily workflows; roles and review habits change; teams learn when not to use AI. | Role-specific training, manager coaching, review rituals, adoption telemetry, escalation paths. | Production routes, policy gates, operational monitoring, evaluation loops. | Redesign jobs and incentives around verified outcomes, not raw AI usage. |
| **Stage 4: Become AI Future Ready** | Organization continuously updates workflows, roles, governance, and platforms as AI capability changes. | Adaptive learning system, mature change network, skill-preservation practices, institutional feedback loops. | Governed platform, reusable patterns, lifecycle management, cross-system telemetry. | Tie AI adoption to strategy, workforce planning, governance, and continuous improvement. |

The adoption lesson is that maturity is not measured by license count. It is measured by whether people, workflows, data, governance, training, and incentives have changed enough for AI to produce durable value.

## **The Psychological Dimensions of Change: Identity, Status, and Resistance**

AI adoption is identity-relevant. Intelligent systems do not merely change tools; they change how people understand their expertise, status, autonomy, and value inside the organization. Resistance should therefore be treated as diagnostic signal, not as laziness, irrationality, or simple anti-technology sentiment.

When AI enters a workflow, employees may reasonably ask:

```text
What part of my work is still mine?
What judgment am I expected to retain?
Will this tool make me better, replace me, monitor me, or deskill me?
Who is accountable when the system is wrong?
Will productivity gains become relief, or just more work?
```

A mature adoption system answers those questions structurally through role design, workflow boundaries, governance, training, incentives, and support.

### **Identity-Relevant Adoption Concerns**

| Concern | What It Means | Adoption Risk | Product / Change Response |
| :---- | :---- | :---- | :---- |
| **Autonomy Concern** | Users fear losing professional discretion, pacing, or judgment. | Passive resistance, tool avoidance, low-quality compliance. | Preserve human authority where judgment matters; make delegation voluntary or bounded where possible. |
| **Skill Erosion Concern** | Users fear that expertise will decay or become irrelevant. | Under-use, resentment, anxiety, or overdependence. | Preserve skill-building tasks, unassisted practice, critique steps, and expert calibration roles. |
| **Status Concern** | Users fear loss of distinctiveness, influence, or career value. | Defensive dismissal, political resistance, public skepticism. | Reframe expert roles around review, policy, calibration, edge cases, and workflow design. |
| **Surveillance Concern** | Users fear telemetry will be used to punish or rank them unfairly. | Shadow workflows, data withholding, distrust of feedback systems. | Separate improvement telemetry from punitive monitoring; define transparency, access, appeal, and privacy rules. |
| **Quality Concern** | Users have seen AI fail and do not trust it for serious work. | Rejection or manual bypass. | Show eval results, known limits, confidence, evidence, and escalation paths. |
| **Workload Concern** | Users experience AI as extra review, cleanup, or documentation burden. | Burnout, correction fatigue, abandonment. | Reduce output volume, improve routing, protect learning time, and measure total task time. |
| **Ethical / Labor Concern** | Users object to data use, attribution, labor displacement, or social impact. | Organized opposition or reputational risk. | Create participatory governance, provenance rules, opt-out paths where feasible, and transparent policy. |

### **Resistance Pattern Diagnostic**

| Resistance Pattern | Possible Signal | What Not To Do | Useful Intervention |
| :---- | :---- | :---- | :---- |
| **Open Skepticism** | Users doubt reliability, value, or safety. | Label skeptics as blockers. | Invite them into eval design, red-team pilots, and workflow-fit review. |
| **Quiet Under-Use** | Feature does not fit real work, or users fear looking incompetent. | Increase pressure or mandate usage blindly. | Run manager check-ins, usability studies, and workflow observation. |
| **Shadow Tool Use** | Approved tools are too weak, slow, restricted, or disconnected. | Respond only with bans. | Improve approved tools, clarify policy, and create fast procurement/review paths. |
| **Rubber-Stamping** | Users overtrust outputs or feel pressured for speed. | Reward raw throughput alone. | Require evidence review, correction capture, and quality metrics. |
| **Excessive Manual Rework** | AI output is not good enough or review surface is poor. | Blame users for not “prompting better.” | Redesign route, data, prompt, UI, or task scope. |
| **Public Moral Objection** | Ethical, labor, privacy, or status concerns are unresolved. | Treat objections as bad faith by default. | Create governance forums and make tradeoffs explicit. |
| **Champion Burnout** | Early adopters are doing enablement as unpaid extra labor. | Praise them while adding more work. | Allocate time, recognition, manager support, and backfill capacity. |

### **AI-Inclusive Professional Identity**

The adoption goal is not to make employees subordinate to AI. It is to help them form an AI-inclusive professional identity:

| Dimension | Healthy Form |
| :---- | :---- |
| **Agency** | “I decide when and how to use AI.” |
| **Expertise** | “AI handles some execution; I own judgment, context, and quality.” |
| **Status** | “My role becomes more valuable because I can design, verify, and improve higher-leverage workflows.” |
| **Learning** | “The system helps me grow rather than bypassing the skills I need.” |
| **Accountability** | “The organization has clear rules for who approves, acts, and bears responsibility.” |

Resistance is not a defect to crush. It is information about where the adoption architecture is incomplete.

## **Cognitive Engineering: Skill Preservation and Cognitive Apprenticeship**

AI systems can help people work faster, but they can also change what people practice. When users repeatedly outsource reading, synthesis, drafting, debugging, judgment, or explanation, the organization may lose the very human capabilities it needs to supervise AI well.

The risk is not that every user becomes cognitively weaker. The risk is more specific:

```text
Experts may stop practicing skills they already have.
Novices may skip the practice needed to build those skills in the first place.
Teams may mistake fluent output for retained understanding.
```

Adoption systems must therefore preserve human learning, judgment, and explanation ability while still allowing AI to remove low-value toil.

### **Skill Preservation Model**

| User Class | Adoption Risk | Failure Pattern | Design Response |
| :---- | :---- | :---- | :---- |
| **Novice** | Uses AI before building internal schemas. | Can produce work but cannot explain, debug, or transfer knowledge. | Require pre-draft reasoning, worked examples, manual reps, source explanation, and mentor review. |
| **Intermediate** | Uses AI to avoid difficult synthesis. | Accepts plausible drafts, misses edge cases, weakens independent critique. | Require critique steps, evidence checks, comparison of alternatives, and periodic unassisted tasks. |
| **Expert** | Offloads routine work and loses sharpness in rare cases. | Maintains output volume but becomes slower at novel, ambiguous, or out-of-distribution problems. | Preserve edge-case drills, red-team review, calibration work, and unassisted intervals. |
| **Manager / Reviewer** | Treats AI output as proof of work. | Rewards volume instead of understanding or quality. | Evaluate explanation quality, correction quality, and downstream outcomes. |
| **Team** | Normalizes machine-generated output without shared standards. | Quality appears high until unusual failures reveal weak understanding. | Maintain shared rubrics, review rituals, incident learning, and examples of known AI failure modes. |

### **Desirable Difficulty UX Matrix**

| Cognitive Risk | Interface Intervention | Purpose | Use Carefully When |
| :---- | :---- | :---- | :---- |
| **Blind Acceptance** | Require users to review highlighted claims, calculations, or citations before acceptance. | Prevents passive approval of fluent output. | Low-risk creative drafting may not need heavy friction. |
| **Weak Problem Framing** | Ask user for a short goal, constraints, or proposed outline before generating. | Builds problem formulation skill. | Time-critical workflows may need templates instead. |
| **Poor Source Evaluation** | Show source snippets, provenance, freshness, and confidence next to generated claims. | Keeps evidence visible during review. | Evidence display must not overwhelm the user. |
| **Loss of Explanation Ability** | Ask user to select or write a brief rationale for final decision in high-impact workflows. | Preserves accountability and reasoning. | Do not require rationales for trivial tasks. |
| **Over-Reliance in Novices** | Use apprenticeship mode: hints, examples, partial completions, and delayed full answers. | Helps users learn rather than bypassing learning. | Must be balanced against productivity needs. |
| **Expert Skill Decay** | Schedule unassisted review, drills, or manual fallback practice. | Maintains readiness for AI failure or edge cases. | Avoid turning practice into meaningless compliance theater. |

### **Cognitive Apprenticeship Pattern**

```text
novice observes -> novice attempts -> AI critiques -> human mentor reviews
       -> user retries -> system records learning gaps -> training improves
```

AI training should not only teach prompts. It should teach users how to frame problems, inspect evidence, challenge outputs, understand failure modes, and explain final decisions.

The adoption objective is not frictionless acceptance. It is calibrated assistance that preserves the human ability to think when the machine is unavailable, wrong, or insufficient.

## **Dynamic Authority Regulation and Trust Calibration**

Human-AI collaboration requires explicit authority states. A user should always know whether the AI is informing, drafting, recommending, executing, waiting for approval, or blocked from action. Authority must be dynamic because risk, confidence, task type, user role, and context change during a workflow.

Dynamic Authority Regulation defines how authority moves between human and machine during a task.

### **Authority State Model**

| Authority State | AI Role | Human Role | Allowed Actions | Typical Trigger |
| :---- | :---- | :---- | :---- | :---- |
| **Human Primacy** | Organizes evidence or answers questions. | Makes decision and executes. | Read, summarize, compare, explain. | High ambiguity, high consequence, low confidence, regulated context. |
| **AI Assist** | Drafts, suggests, extracts, or highlights. | Reviews, edits, approves, or rejects. | Draft, classify, flag, recommend. | Medium-risk work with reviewable output. |
| **Shared Review** | Produces analysis and uncertainty; waits for human judgment. | Resolves ambiguity or approves next step. | Risk analysis, options, evidence packages. | Conflicting evidence, edge cases, low confidence, high-value workflow. |
| **AI Delegation** | Executes bounded subtasks under policy. | Monitors exceptions and reviews results. | Route, extract, transform, test, queue. | Low-to-medium risk, reversible, verifiable tasks. |
| **AI Automation** | Acts independently inside narrow constraints. | Owns oversight and periodic audit. | Low-risk routing, filtering, deduplication, background classification. | High-volume, low-consequence, easily verified work. |
| **Human Override / Safe Hold** | Stops, preserves state, and explains why. | Decides recovery path. | Freeze, escalate, rollback, route to manual review. | Safety, privacy, policy, uncertainty, or verification failure. |

### **Authority Transition Triggers**

| Trigger | Direction of Movement |
| :---- | :---- |
| **Confidence drops** | Toward human primacy or shared review. |
| **Risk tier increases** | Toward human approval or safe hold. |
| **Action becomes irreversible** | Toward human approval or manual-only execution. |
| **Task becomes routine and verified** | Toward delegation or automation. |
| **User lacks permission** | Toward block or escalation. |
| **Evidence conflict appears** | Toward shared review. |
| **Policy boundary is hit** | Toward safe hold. |
| **Repeated user correction appears** | Toward lower autonomy and retraining/review. |
| **Long passive monitoring period occurs** | Toward revalidation or human check-in. |

### **Handoff Controls**

| Control | Purpose |
| :---- | :---- |
| **Authority Banner** | UI clearly states whether AI is suggesting, drafting, reviewing, acting, or blocked. |
| **Reason for Handoff** | System explains why authority changed. |
| **Evidence Packet** | Human receives context, source references, uncertainty, and prior actions. |
| **Override Reason Capture** | Human override is recorded as improvement signal, not punishment by default. |
| **Hysteresis Thresholds** | Prevent rapid oscillation between human and AI control. |
| **Safe-Exit Timers** | Require human revalidation after prolonged autonomous monitoring. |
| **State Preservation** | Workflow can pause without losing context or corrupting downstream systems. |
| **Retention Policy** | Authority logs are retained according to risk, privacy, and audit requirements. |

### **Trust Rupture and Recovery**

| Rupture Pattern | What Happened | Recovery Requirement |
| :---- | :---- | :---- |
| **Graceful Limitation** | System disclosed uncertainty and handed off safely. | Reinforce accurate mental model; improve guidance if needed. |
| **Silent Failure** | System appeared confident but was wrong. | Incident review, user notification where appropriate, eval update, visible fix. |
| **Repeated Minor Errors** | Small failures accumulate and erode confidence. | Analyze correction patterns, improve route/data/UI, communicate known limits. |
| **Over-Automation Event** | System acted with too much authority. | Reduce autonomy, add approval gate, update policy and training. |
| **User Overtrust** | Human rubber-stamped output. | Add friction, evidence review, independent judgment step, or manager coaching. |
| **User Undertrust** | Human rejects useful system behavior. | Improve transparency, reliability, onboarding, and scope clarity. |

Trust calibration is not achieved by telling users to “trust the AI.” It is achieved by making system authority visible, bounded, reversible, explainable, and correctable.

## **Change Management Transmission: Champion Networks, Sprints, and Frameworks**

Enterprise transformation programs that rely strictly on top-down executive mandates fail because employees trust peer credibility over corporate policy.13 To bridge the gap between strategic leadership and front-line execution, successful AI adoption systems deploy a coordinated, three-tier change management transmission layer 13:

```
 (5-9 Leaders, Biweekly)  
         │  
         ▼  (Air Cover, Budget & Strategic Mandate)  
 (6-10 Enablement/Ops, Weekly)  
         │  
         ▼  (Playbooks, Tools & Guardrail Templates)  
[Champion Network] (1 Champion per 50-75 Employees)  
         │  
         ▼  (Hands-on Peer Training & Ground-up Use Case Discovery)
```

At the execution layer, organizations often struggle to choose between Kotter's 8-Step Model and the Prosci ADKAR framework.2 In technical and engineering environments, Kotter’s heavy emphasis on creating a top-down sense of organizational urgency often conflicts with technical teams, who prefer evidence-based decision-making over executive mandates.2  
Further, Kotter's extended sequential timeline can introduce bureaucratic overhead that stifles developer autonomy.2  
In contrast, the Prosci ADKAR model focuses on the individual's skill progression, aligning naturally with iterative, sprint-level technical changes.2

### **Comparative Framework Alignment for AI Adoption**

| Change Phase | Kotter 8-Step Application | Prosci ADKAR Application | Recommended Technical Synthesis |
| :---- | :---- | :---- | :---- |
| **Phase 1: Preparation** 1 | **Create Urgency:** Highlight the competitive threat of non-adoption.2 | **Awareness:** Build understanding of the specific strategic need for AI.1 | Execute Prosci Phase 1 (Prepare Approach); run maturity diagnostics and stakeholder power mapping.28 |
| **Phase 2: Enablement** 1 | **Build Guiding Coalition:** Assemble key sponsors and departmental heads.13 | **Desire & Knowledge:** Foster willingness to engage; deliver targeted learning.1 | Launch the Champion Network; establish the Working Group; deliver role-specific hands-on training.13 |
| **Phase 3: Integration** 1 | **Empower Action:** Remove obstacles; update systems and workflows.2 | **Ability:** Provide real-world coaching and structured integration opportunities.1 | Run Two-Week Champion Sprints in live, messy operational conditions to validate playbooks.13 |
| **Phase 4: Reinforcement** 1 | **Institutionalize Change:** Embed technology into corporate culture.29 | **Reinforcement:** Implement rewards, recognition, and continuous correction loops.1 | Connect telemetry (edit distance, click tracking) to performance scorecards and digital rewards.10 |

To operationalize this transmission layer, champions execute iterative **Two-Week Champion Sprints** to discover and validate high-value use cases.13  
During **Week 1 (Explore and Test)**, three to five champions in a specific department pick a real, repetitive manual task and execute it using the AI tool chain under live, messy operational conditions where edge cases naturally surface.13  
During **Week 2 (Validate and Package)**, the champions refine their prompt strategies, draft a simple, highly actionable one-page guide, and present a structured recommendation to the Working Group: roll out the workflow widely, modify its parameters, or kill it.13

Week 1: Real-World Testing ──> Identify Friction & Edge Cases ──> Week 2: Package & Standardize ──> Deploy to Production

To ensure the durability of this network, the Working Group must monitor and actively remediate several enablement anti-patterns 13:

* **Treating Enablement as Unpaid Extra Work:** Champions already have full-time jobs; expecting them to drive change without workload adjustments leads directly to burnout.13 Organizations must explicitly allocate 10% to 20% of their contracted time to enablement and make it a formal part of their performance goals.13  
* **The Expanded Workload Trap:** UC Berkeley research reveals that productivity gains from AI often morph into expanded workloads rather than freed-up time.13 When champions automate tasks, managers frequently dump more tasks onto their plates, collapsing the incentive to innovate.13 Leadership must step in to protect champions from this workflow inflation.13  
* **Technical Echo Chambers:** Recruiting only technologists or enthusiasts to the champion network creates a bias bubble.13 Program owners must intentionally recruit pragmatists, skeptics, and non-technical business users to ensure the tools actually work for everyday employees.13  
* **Unstructured Enthusiasm:** Relying on early volunteer excitement instead of putting established structures, cadences, and boundaries in place.13 Unstructured enthusiasm is unsustainable and eventually kills the program.13

## **Enablement Architectures: The Workload Paradox and Training Curriculum**

AI adoption often creates a workload paradox. Leaders expect productivity gains, while front-line employees experience more review, correction, training, coordination, and tool-management work. A credible adoption system must plan for this transition cost instead of pretending AI instantly creates free capacity.

Published workforce research has reported a sharp gap between executive expectations and employee experience: many leaders expect AI to improve productivity, while many employees report that AI has increased workload. The architectural lesson is not “AI fails.” It is that AI produces value only when workflow redesign, training, review burden, support, and incentives are managed together.

### **The Workload Paradox**

```text
Management expectation:
  AI reduces effort immediately.

Operational reality:
  AI can increase review, correction, training, prompt iteration,
  support load, governance work, and output volume.

Adoption architecture:
  protect learning time, reduce old workload temporarily,
  measure total task time, and redesign workflows around verified outcomes.
```

| Workload Source | Why It Appears | Adoption Control |
| :---- | :---- | :---- |
| **Review Burden** | More AI drafts means more material to inspect. | Limit output volume; use evidence display and risk-tiered review. |
| **Correction Burden** | Early outputs fail in edge cases or messy workflows. | Capture corrections and route them into evals, prompts, data, and training. |
| **Training Burden** | Users need new mental models and safe-use practices. | Provide protected time and role-specific practice. |
| **Coordination Burden** | Teams must decide where AI fits and who owns outcomes. | Define workflow roles, authority states, and escalation paths. |
| **Governance Burden** | Users must learn data, privacy, IP, and policy boundaries. | Build policy into product surfaces and training examples. |
| **Output Inflation** | Faster generation creates more artifacts than teams can validate. | Reward quality and verified outcomes, not volume. |

### **Protected Time Policy**

Protected time is not a perk. It is adoption infrastructure. Teams cannot learn new workflows while being measured as if nothing changed.

| Rollout Intensity | Recommended Protected Time | Management Adjustment |
| :---- | :---- | :---- |
| **Lightweight tool introduction** | Short training blocks and office hours. | Do not penalize early experimentation time. |
| **Role-specific workflow adoption** | Recurring weekly practice or lab time during rollout. | Temporarily reduce competing delivery expectations. |
| **Major workflow redesign** | Formal sprint capacity allocation. | Plan backfill, slower throughput, and manager coaching. |
| **High-risk / regulated workflow** | Structured training, certification, and supervised practice. | Gate access by demonstrated competence. |

Exact time allocations should be set by role, risk, workload, and rollout intensity. The principle is durable: adoption requires protected capacity.

### **Doctrinal Enablement Curriculum**

| Training Level | Core Objective | Target Audience | Curriculum Topics | Evidence of Completion |
| :---- | :---- | :---- | :---- | :---- |
| **Level 1: AI Literacy** | Establish basic understanding and safe boundaries. | All employees exposed to AI tools. | Probabilistic outputs, hallucination, data boundaries, privacy/IP basics, when not to use AI. | Scenario quiz, safe-use acknowledgment, basic practice task. |
| **Level 2: Workflow Adoption** | Teach role-specific use inside real tasks. | Business units and functional teams. | Prompt/context patterns, verification, review surfaces, escalation, correction capture. | Completed workflow lab, accepted/rejected examples, manager sign-off. |
| **Level 3: Domain Transformation** | Redesign work around AI-assisted capability. | Power users, champions, analysts, technical leads. | Workflow decomposition, eval design, feedback loops, authority states, operational metrics. | Validated use-case package, pilot results, playbook contribution. |
| **Level 4: System Stewardship** | Govern, monitor, and improve AI-enabled workflows. | Champions, product owners, governance, operations. | Drift, incidents, runbooks, audit, telemetry, model/data limitations, change management. | Runbook drill, evaluation review, incident simulation, governance review. |

### **Complementary Human Capabilities for AI Adoption**

The MIT EPOCH framing identifies human capabilities that complement AI rather than merely compete with it: empathy, presence, opinion/judgment, creativity, and hope. In enterprise adoption, these map to practical workforce capabilities.

| Capability | Adoption Meaning | Workflow Application |
| :---- | :---- | :---- |
| **Empathy** | Understanding human needs, fears, context, and stakeholder impact. | Customer support, change management, sensitive communications, employee coaching. |
| **Presence** | Being accountable, attentive, and socially available in real situations. | Leadership, facilitation, negotiation, escalation, conflict resolution. |
| **Opinion / Judgment** | Making value-laden decisions under ambiguity. | Risk review, policy interpretation, prioritization, exception handling. |
| **Creativity** | Reframing problems and generating novel constraints or approaches. | Product design, workflow redesign, strategy, synthesis beyond pattern replay. |
| **Hope / Leadership** | Creating shared direction and confidence during uncertainty. | Transformation narratives, adoption sponsorship, team resilience. |

AI training should not merely increase tool usage. It should strengthen the human capabilities that make AI safe and valuable in real organizations.

## **Help Desk and Support Operations: Deflection, Resolution, and Escalation Handoffs**

Support operations are a proving ground for AI adoption because they expose the difference between apparent automation and actual resolution. A bot can inflate deflection numbers by trapping users, hiding escalation, or forcing repeated rephrasing. That is not adoption success. That is containment theater with a headset.

A mature support AI program measures resolved outcomes, customer experience, handoff quality, and repeat-contact reduction.

### **Support Stack Architecture**

```text
SUPPORT AI STACK

Layer 1: Self-Service / Autonomous Resolution
  Best for: high-volume, low-risk, well-documented intents
  Controls: confidence threshold, source grounding, easy escalation

Layer 2: Agent Assist
  Best for: human-led support with AI summaries, suggested replies, policy lookup
  Controls: human send action, source links, edit capture

Layer 3: Human Escalation
  Best for: complex, emotional, regulated, ambiguous, or high-value cases
  Controls: structured handoff packet, full context, escalation reason

Layer 4: Knowledge and Improvement Loop
  Best for: turning failures, repeats, and corrections into better workflows
  Controls: intent review, article updates, evals, routing changes
```

### **Deflection vs. Resolution Metrics**

| Metric | What It Measures | Failure Mode If Used Alone |
| :---- | :---- | :---- |
| **Deflection / Containment Rate** | Share of interactions not escalated to a human. | Can reward trapping users. |
| **True Resolution Rate** | Share of issues solved without repeat contact or negative follow-up. | Requires good identity and ticket linking. |
| **CSAT / Sentiment Delta** | Customer experience by intent, tier, and handoff path. | Can hide failures if only averaged globally. |
| **Repeat Contact Rate** | Whether users return with the same unresolved issue. | Needs intent matching and time-window definition. |
| **Escalation Quality** | Whether handoff to human contains useful context. | Poor packets create “tell me again” frustration. |
| **Context-Loss Rate** | How often users must repeat information after escalation. | Direct indicator of failed AI-human handoff. |
| **Wrong-Answer Rate** | Unsupported, stale, or policy-incorrect answers. | Requires audit sampling or user correction capture. |
| **Agent Handle-Time Delta** | Whether AI assist helps human agents. | Can hide quality decline if speed is overvalued. |
| **Knowledge Gap Rate** | Intents where AI fails due to missing or bad content. | Should feed knowledge-base improvement. |

### **Launch-Control Model**

| Phase | Traffic Exposure | Core Work | Exit Gate |
| :---- | :---- | :---- | :---- |
| **Phase 1: Knowledge and Instrumentation** | No live autonomous traffic. | Inventory intents, SOPs, knowledge articles, escalation paths, baseline CSAT and repeat contact. | Held-out evaluation passes; escalation packet format approved. |
| **Phase 2: Shadow / Agent-Assist Pilot** | Human-visible assist only or shadow mode. | Compare AI suggestions to human actions; tune retrieval and confidence thresholds. | No material quality degradation; useful agent feedback; known failure modes documented. |
| **Phase 3: Limited Self-Service Pilot** | Small slice of low-risk intents. | Allow autonomous resolution only for high-confidence, well-grounded cases. | Stable CSAT, low repeat contact, audited answer quality, clean escalation handoffs. |
| **Phase 4: Controlled Ramp** | Gradual expansion by intent, not blanket traffic. | Add intents only after evidence, knowledge coverage, and handoff quality pass. | Intent-level performance remains within thresholds. |
| **Phase 5: Continuous Improvement** | Mature operation. | Weekly failure review, knowledge updates, eval additions, routing changes. | Measured improvement in resolution, cost, and customer experience. |

### **Escalation Handoff Packet**

Every escalation from AI to human should include:

| Packet Field | Purpose |
| :---- | :---- |
| **Customer intent** | Explains what the user is trying to solve. |
| **Conversation summary** | Prevents the user from repeating the whole interaction. |
| **Known account/context fields** | Gives the agent relevant source-of-record facts. |
| **AI actions already taken** | Prevents duplicate troubleshooting. |
| **Confidence and failure reason** | Explains why escalation occurred. |
| **Relevant source articles / policies** | Supports fast human resolution. |
| **User sentiment / urgency signal** | Helps prioritize distressed or high-risk cases. |
| **Compliance or privacy flags** | Prevents unsafe handling. |

### **Support Platform Selection Criteria**

Instead of embedding a brittle vendor comparison, evaluate support platforms by capability class:

| Capability | Product Requirement |
| :---- | :---- |
| **System-of-record integration** | Connects to ticketing, CRM, account, billing, and knowledge systems. |
| **Grounded answer generation** | Uses approved knowledge with citations and freshness controls. |
| **Escalation orchestration** | Passes structured handoff packets to human agents. |
| **Intent-level analytics** | Reports CSAT, repeat contact, deflection, and failure by intent. |
| **Governance controls** | Supports permissions, audit logs, data retention, and redaction. |
| **Feedback loop** | Converts corrections and unresolved tickets into knowledge/eval updates. |
| **Model/provider flexibility** | Allows route changes as cost, quality, and policy needs change. |

Support AI success is not “fewer humans touched the ticket.” Success is fewer unresolved issues, less repetition, faster correct resolution, better agent leverage, and safer customer experience.

## **Quantitative Telemetry: Feedback Loops, Corrections, and Adoption Signals**

Adoption systems need behavioral telemetry, but not all telemetry should become performance surveillance. The goal is to learn whether AI improves work, where users correct it, when they ignore it, where it creates burden, and how the system should improve.

Telemetry should be designed around product improvement, workflow safety, and user trust.

### **Adoption Telemetry Model**

| Signal Family | Example Metrics | What It Reveals | Guardrail |
| :---- | :---- | :---- | :---- |
| **Usage** | Activation, repeat use, feature path, session frequency. | Whether users try and return to the feature. | Usage alone is not success. |
| **Acceptance** | Accept, reject, edit, regenerate, abandon. | Whether AI output is useful. | Avoid rewarding blind acceptance. |
| **Correction** | Edit distance, field corrections, override reasons, rejected claims. | Where the model, data, prompt, or UI fails. | Store sensitive content by secure reference where needed. |
| **Verification** | Citation checks, schema validation, source-of-record confirmation. | Whether output is safe to use. | Do not rely on model self-report. |
| **Workflow Outcome** | Time-to-completion, repeat work, rework, escalation, handoff success. | Whether total work improved. | Measure end-to-end task, not draft speed. |
| **Trust Calibration** | Over-acceptance, under-use, repeated overrides, manual bypass. | Whether users trust appropriately. | Interpret as signal, not employee defect. |
| **Support Load** | Help tickets, training questions, failure reports, confusion signals. | Whether adoption burden is rising. | Feed training and product redesign. |
| **Quality Drift** | Error trends, eval degradation, correction clusters. | Whether model/data/workflow performance changes. | Connect to owner and runbook. |

### **Edit Distance as One Signal**

Edit distance can be useful for draft workflows: it compares AI-generated text with the human-approved final version. Low edit distance may indicate high usefulness, but it can also indicate rubber-stamping. High edit distance may indicate poor output, but it can also indicate active expert refinement.

| Edit Pattern | Possible Interpretation | Follow-Up |
| :---- | :---- | :---- |
| **Near-zero edits** | Strong fit, or blind acceptance. | Check error rate, task risk, and review behavior. |
| **Moderate edits** | Useful draft with human refinement. | Inspect correction categories and improve prompts/data. |
| **High edits** | Poor model fit, wrong context, or user using AI as rough ideation. | Segment by task type before judging. |
| **Repeated same edits** | Systematic prompt/data/style gap. | Add eval case, template, or product control. |
| **Edits concentrated in sensitive fields** | Risky extraction or hallucination pattern. | Add validation, evidence display, or human gate. |

There is no universal “optimal” edit-distance band. The expected range depends on task, role, risk, surface, and whether the AI is drafting, extracting, summarizing, or brainstorming.

### **Structured Feedback Event Schema**

Use a structured event model rather than raw transcript dumps wherever possible.

```json
{
  "event_type": "ai_feedback_event",
  "workflow_id": "contract_review",
  "route_id": "governed_synthesis",
  "task_type": "draft_review",
  "user_role": "legal_reviewer",
  "risk_tier": "high",
  "ai_action": "drafted_summary",
  "human_action": "edited_and_approved",
  "edit_summary": {
    "edit_distance_bucket": "moderate",
    "correction_categories": ["missing_citation", "overbroad_claim"],
    "sensitive_content_ref": "secure-ref://payload/abc123"
  },
  "verification": {
    "schema_valid": true,
    "citations_verified": false,
    "source_of_record_checked": true
  },
  "outcome": {
    "task_completed": true,
    "escalated": false,
    "rework_required": false
  }
}
```

### **Feedback Routing**

| Signal | Routed To | Action |
| :---- | :---- | :---- |
| Repeated user correction | Product / Prompt owner | Revise template, UI, or route. |
| Citation failure | Retrieval / Corpus owner | Fix source, chunking, ranking, or citation verifier. |
| Schema failure | Tool / Workflow owner | Tighten schema, validators, or field-level UI. |
| High rejection rate | Product owner | Reassess use case, user tolerance, or model fit. |
| Over-acceptance in risky workflow | Adoption / Governance owner | Add friction, training, or independent review. |
| Rising support questions | Enablement owner | Improve training and documentation. |
| Drift or regression | Eval / Ops owner | Add eval case, rollback, or incident review. |
| Privacy-sensitive correction | Governance / Privacy owner | Review retention, redaction, and access controls. |

Adoption telemetry should improve the system and support users. If telemetry becomes a punishment machine, users will route around it, corrupt it, or stop trusting the adoption program.

## **Behavioral Incentives, Performance Systems, and Compensation Architectures**

AI adoption is shaped by incentives. If employees are rewarded only for raw output volume, they will generate more artifacts. If they are rewarded only for speed, they will skip verification. If they are measured by prompts submitted or licenses used, they will game usage metrics. If telemetry feels punitive, they will avoid the system or move work into shadow channels.

The goal is to reward safe, useful, workflow-improving adoption—not AI activity for its own sake.

### **Incentive Design Principles**

| Principle | Meaning |
| :---- | :---- |
| **Reward outcomes, not usage.** | License activation, prompt count, and click volume are weak adoption metrics. |
| **Reward verification, not rubber-stamping.** | Users should be recognized for catching errors, improving workflows, and preserving quality. |
| **Protect learning time.** | Adoption requires time; do not punish teams for the temporary slowdown needed to learn. |
| **Avoid surveillance incentives.** | Telemetry should support improvement, not opaque individual punishment. |
| **Make managers accountable for workflow redesign.** | Front-line workers cannot realize value if managers simply add AI on top of old workloads. |
| **Recognize shared improvement.** | Champions, reviewers, prompt maintainers, data stewards, and support staff all contribute. |
| **Preserve fairness and appeal.** | AI-informed performance systems need transparency, bias review, and human appeal paths. |

### **Adoption Incentive Scorecard**

| Dimension | Good Evidence | Bad Proxy to Avoid | Reward Pattern |
| :---- | :---- | :---- | :---- |
| **Workflow Improvement** | Reduced total task time, lower rework, higher verified throughput. | Number of AI-generated artifacts. | Team recognition, process improvement credit. |
| **Quality and Verification** | Error catches, source corrections, improved eval cases, fewer downstream defects. | Blind accept rate. | Quality credit and reviewer recognition. |
| **Knowledge Sharing** | Reusable playbooks, validated examples, peer coaching, office hours. | Posting random prompt tricks. | Champion credit, protected time, promotion evidence. |
| **Responsible Use** | Safe data handling, correct escalation, policy adherence, good judgment. | Zero reported issues because nobody reports problems. | Recognition for reporting and fixing issues. |
| **Adoption Learning** | Training completion plus demonstrated task competence. | LMS completion alone. | Role readiness badge, access expansion. |
| **Innovation** | Validated use cases with measured value and adoption. | Novel demos with no workflow impact. | Pilot funding, team-level reward. |
| **Manager Enablement** | Protected time, workload adjustment, support of experimentation, reduced blockers. | Mandated usage targets. | Leadership score tied to safe realized value. |

### **Telemetry Use Guardrails**

| Guardrail | Requirement |
| :---- | :---- |
| **Transparency** | Employees should know what adoption telemetry is collected and why. |
| **Purpose Limitation** | Telemetry collected for product improvement should not silently become disciplinary evidence. |
| **Aggregation First** | Prefer team/workflow-level analysis over individual ranking where possible. |
| **Human Review** | Performance decisions should not be made solely by automated telemetry. |
| **Bias Review** | Check whether adoption metrics disadvantage certain roles, regions, disabilities, seniority levels, or work types. |
| **Appeal Path** | Employees need a way to contest or contextualize AI-derived performance signals. |
| **Retention Limits** | Keep telemetry only as long as needed for improvement, audit, or policy. |
| **Sensitive Data Handling** | Store raw content only when necessary, with redaction, secure references, and access controls. |

### **Compensation and Pay Transparency Considerations**

AI-informed performance systems must be designed carefully around fairness, explainability, and equal-treatment obligations. In the EU, the Pay Transparency Directive is Directive (EU) 2023/970, with member states required to transpose it by June 7, 2026. Adoption scorecards should therefore avoid opaque or biased AI-driven compensation effects.

| Risk | Control |
| :---- | :---- |
| **AI usage becomes hidden performance pressure.** | State whether AI adoption is expected, optional, or role-specific. |
| **Telemetry rewards high-volume roles unfairly.** | Normalize by workflow, role, risk, and opportunity. |
| **Correction-heavy users look “less efficient.”** | Treat corrections as quality signals, not automatic negatives. |
| **Managers dump more work onto AI-capable employees.** | Track workload inflation and protect capacity. |
| **Employees fear reporting AI failures.** | Reward transparent failure reporting and improvement contributions. |

The mature incentive system does not ask, “Who used AI the most?” It asks, “Which teams improved verified outcomes safely, fairly, and sustainably?”.16

## **Cross-Canon Handoff Map**

AI-ENG-AG defines the adoption layer of the AI Engineering Systems Canon. It explains how valid AI products become durable human workflows through training, role design, feedback loops, support systems, trust calibration, incentives, and change management.

AG consumes product, governance, UX, telemetry, review, and operations constraints from earlier reports and hands organizational adoption patterns forward to business and enterprise transformation architecture.

### **Upstream Canon Inputs**

| Canon Report | What AG Consumes | Adoption Use |
| :---- | :---- | :---- |
| **AI-ENG-AF — AI Product Architecture** | Use-case fit, workflow mapping, user tolerance, product pattern, no-AI decisions. | Determines what users are being asked to adopt and whether adoption is structurally plausible. |
| **AI-ENG-AD — Governance Architecture** | Policy, accountability, audit, procurement, risk classification. | Defines safe-use rules, escalation paths, and adoption boundaries. |
| **AI-ENG-Y — Human Review** | Review queues, reviewer quality, escalation, maker-checker patterns. | Shapes human oversight training and review workload design. |
| **AI-ENG-X — User Trust** | Transparency, contestability, disclosure, trust calibration. | Informs trust repair, user education, and product-surface expectations. |
| **AI-ENG-W — UX Resilience** | Degraded modes, fallback, continuity, user state. | Defines how adoption survives outages, uncertainty, and fallback states. |
| **AI-ENG-Z — Strategic Telemetry** | Traces, metrics, correction signals, privacy-aware observability. | Feeds adoption telemetry, workflow learning, and support intervention. |
| **AI-ENG-AA — Evaluation Architecture** | Golden sets, rubrics, regression gates, quality thresholds. | Turns user corrections into eval cases and role-specific training. |
| **AI-ENG-AC — AI Operations** | Incidents, runbooks, rollback, containment, postmortems. | Connects adoption failures to operational response and learning loops. |
| **AI-ENG-AE — Sustainable AI** | Workload burden, output inflation, resource efficiency, lifecycle review. | Prevents adoption programs from generating wasteful output and review overload. |
| **AI-ENG-N — Tool Contracts** | Tool authority, schemas, side effects, permission boundaries. | Teaches users what AI can safely do and where approval is required. |
| **AI-ENG-O — Action Verification** | Source-of-record verification, action ledgers, false-success prevention. | Defines when users can trust completion claims and when they must verify. |
| **AI-ENG-T — Boundary Defense** | Prompt injection, tenant isolation, egress, policy hierarchy. | Shapes safe-use training and shadow-AI prevention. |
| **AI-ENG-S — Production Pathologies** | Failure modes, brittle chains, loop failures, false confidence. | Supplies adoption training examples and trust-calibration scenarios. |

### **Forward Handoff to Organizational Architecture**

| Target Report | AG Concept Handed Forward | Downstream Use |
| :---- | :---- | :---- |
| **AI-ENG-AH — Organizational Architecture and Change** | Champion networks, protected time, adoption states, resistance diagnostics, skill preservation. | Defines organizational structures, team incentives, role redesign, and training operations. |
| **AI-ENG-AI — Enterprise Transformation Reference Architecture** | Adoption telemetry, workflow enablement, governance-aware training, support handoffs. | Integrates adoption systems into enterprise rollout, platform support, and management operating model. |
| **AI-ENG-AJ — Reference Architectures** | Adoption playbooks, feedback loops, support stack, authority-state UX patterns. | Converts adoption doctrine into reusable rollout blueprints and golden paths. |

### **Core Handoff Rule**

Adoption is not the final mile of AI deployment. It is part of the architecture. If users are not trained, incentivized, supported, protected from overload, connected to feedback loops, and given clear authority boundaries, the system has not actually been deployed. It has merely been installed.

#### **Works cited**

1. AI Adoption: Driving Change With a People-First Approach - Prosci, accessed June 14, 2026, [https://www.prosci.com/ai-change-management](https://www.prosci.com/ai-change-management)  
2. 6 Change Management Strategies to Scale AI Adoption in Engineering Teams, accessed June 14, 2026, [https://www.augmentcode.com/guides/6-change-management-strategies-to-scale-ai-adoption-in-engineering-teams](https://www.augmentcode.com/guides/6-change-management-strategies-to-scale-ai-adoption-in-engineering-teams)  
3. Your AI change-management plan: How to bring employees along successfully | Robert Half, accessed June 14, 2026, [https://www.roberthalf.com/us/en/insights/management-tips/ai-change-management-for-leaders](https://www.roberthalf.com/us/en/insights/management-tips/ai-change-management-for-leaders)  
4. Work Design and Multidimensional AI Threat as - arXiv, accessed June 14, 2026, [https://arxiv.org/pdf/2602.23278](https://arxiv.org/pdf/2602.23278)  
5. A Moderated Mediation Model of AI-Driven Identity Threats and ..., accessed June 14, 2026, [https://www.mdpi.com/2254-9625/16/4/52](https://www.mdpi.com/2254-9625/16/4/52)  
6. When Humans Stop Thinking: Cognitive Offloading, Atrophy Risk ..., accessed June 14, 2026, [https://digitalcommons.kennesaw.edu/cgi/viewcontent.cgi?article=1005&context=cognoconproceedings](https://digitalcommons.kennesaw.edu/cgi/viewcontent.cgi?article=1005&context=cognoconproceedings)  
7. Change Management in Agentic AI Adoption: A Practical Guide to ..., accessed June 14, 2026, [https://medium.com/aimonks/change-management-in-agentic-ai-adoption-a-practical-guide-to-the-full-journey-5143ebb60fd6](https://medium.com/aimonks/change-management-in-agentic-ai-adoption-a-practical-guide-to-the-full-journey-5143ebb60fd6)  
8. Human–AI Handovers: A Dynamic Authority Reversal Framework for ..., accessed June 14, 2026, [https://www.preprints.org/manuscript/202603.0390](https://www.preprints.org/manuscript/202603.0390)  
9. LLM Evaluation Metrics: The Ultimate LLM Evaluation Guide - Confident AI, accessed June 14, 2026, [https://www.confident-ai.com/blog/llm-evaluation-metrics-everything-you-need-for-llm-evaluation](https://www.confident-ai.com/blog/llm-evaluation-metrics-everything-you-need-for-llm-evaluation)  
10. How Can Companies Incentivize AI Adoption? - Knowledge at ..., accessed June 14, 2026, [https://knowledge.wharton.upenn.edu/article/how-can-companies-incentivize-ai-adoption/](https://knowledge.wharton.upenn.edu/article/how-can-companies-incentivize-ai-adoption/)  
11. I asked ChatGPT why reddit users hate AI, and DAMN it went all out, accessed June 14, 2026, [https://www.reddit.com/r/ChatGPT/comments/1qgg3ed/i_asked_chatgpt_why_reddit_users_hate_ai_and_damn/](https://www.reddit.com/r/ChatGPT/comments/1qgg3ed/i_asked_chatgpt_why_reddit_users_hate_ai_and_damn/)  
12. Use these 3 MIT guides when implementing AI in your organization, accessed June 14, 2026, [https://mitsloan.mit.edu/ideas-made-to-matter/use-these-3-mit-guides-when-implementing-ai-your-organization](https://mitsloan.mit.edu/ideas-made-to-matter/use-these-3-mit-guides-when-implementing-ai-your-organization)  
13. How to build an AI champions network that actually drives adoption ..., accessed June 14, 2026, [https://amitkoth.com/ai-champions-network-guide/](https://amitkoth.com/ai-champions-network-guide/)  
14. Writing ClickHouse queries for new products - Handbook - PostHog, accessed June 14, 2026, [https://posthog.com/handbook/engineering/databases/clickhouse-queries-new-products](https://posthog.com/handbook/engineering/databases/clickhouse-queries-new-products)  
15. AI Performance Reviews: Avoiding 5 Critical Pitfalls - Giftronaut, accessed June 14, 2026, [https://www.giftronaut.com/blog/ai-performance-reviews](https://www.giftronaut.com/blog/ai-performance-reviews)  
16. AI is Changing the Face of Reward, accessed June 14, 2026, [https://www.people-performance-reward.co.uk/post/ai-is-changing-the-face-of-reward](https://www.people-performance-reward.co.uk/post/ai-is-changing-the-face-of-reward)  
17. GENERATIVE ARTIFICIAL INTELLIGENCE: BETWEEN ENHANCEMENT AND COGNITIVE OFFLOADING - Cultura, Ciencia y Deporte, accessed June 14, 2026, [https://ccd.ucam.edu/index.php/revista/article/view/2698/1527](https://ccd.ucam.edu/index.php/revista/article/view/2698/1527)  
18. Full article: Dilemmas of resistance: How concerns for cultural aspects of identity shape and constrain resistance among minority groups - Taylor & Francis, accessed June 14, 2026, [https://www.tandfonline.com/doi/full/10.1080/10463283.2023.2176663](https://www.tandfonline.com/doi/full/10.1080/10463283.2023.2176663)  
19. What happens when trust in an AI system breaks? Do human–AI relationships follow recognizable trajectories? | ResearchGate, accessed June 14, 2026, [https://www.researchgate.net/post/What_happens_when_trust_in_an_AI_system_breaks_Do_human-AI_relationships_follow_recognizable_trajectories](https://www.researchgate.net/post/What_happens_when_trust_in_an_AI_system_breaks_Do_human-AI_relationships_follow_recognizable_trajectories)  
20. Is AI dulling our minds? - Harvard Gazette, accessed June 14, 2026, [https://news.harvard.edu/gazette/story/2025/11/is-ai-dulling-our-minds/](https://news.harvard.edu/gazette/story/2025/11/is-ai-dulling-our-minds/)  
21. Trust Calibration in Human-AI Teaming: Within-Session Dynamics, Transparency, and Performance Effects, accessed June 14, 2026, [https://repository.rit.edu/theses/12379/](https://repository.rit.edu/theses/12379/)  
22. Adults Lose Skills to AI. Children Never Build Them. | Psychology Today, accessed June 14, 2026, [https://www.psychologytoday.com/us/blog/the-algorithmic-mind/202603/adults-lose-skills-to-ai-children-never-build-them](https://www.psychologytoday.com/us/blog/the-algorithmic-mind/202603/adults-lose-skills-to-ai-children-never-build-them)  
23. AI Tools in Society: Impacts on Cognitive Offloading and the Future of Critical Thinking, accessed June 14, 2026, [https://www.mdpi.com/2075-4698/15/1/6](https://www.mdpi.com/2075-4698/15/1/6)  
24. 16 Best AI-Powered Helpdesk Software on the Market Right Now - Kustomer, accessed June 14, 2026, [https://www.kustomer.com/resources/blog/ai-powered-help-desk-software/](https://www.kustomer.com/resources/blog/ai-powered-help-desk-software/)  
25. Designing for Doubt: How to Prevent Automation Complacency in AI ..., accessed June 14, 2026, [https://www.uxmatters.com/mt/archives/2026/06/designing-for-doubt-how-to-prevent-automation-complacency-in-ai-workflows.php](https://www.uxmatters.com/mt/archives/2026/06/designing-for-doubt-how-to-prevent-automation-complacency-in-ai-workflows.php)  
26. Calibrating AI Trust in Complementary Human-AI Collaboration - OpenReview, accessed June 14, 2026, [https://openreview.net/pdf?id=6KcewDz76A](https://openreview.net/pdf?id=6KcewDz76A)  
27. AI Customer Support 2026: 50+ Adoption + ROI Data Points, accessed June 14, 2026, [https://www.digitalapplied.com/blog/ai-customer-support-statistics-2026-adoption-roi-data](https://www.digitalapplied.com/blog/ai-customer-support-statistics-2026-adoption-roi-data)  
28. How to Do Change Management for M365 Copilot AI Implementation - OCM Solution, accessed June 14, 2026, [https://www.ocmsolution.com/best-change-management-for-m365-copilot-implementation/](https://www.ocmsolution.com/best-change-management-for-m365-copilot-implementation/)  
29. Change Management – AI's Secret Weapon Part 1: Adopting AI? Seven key change management strategies to drive success - Oracle Blogs, accessed June 14, 2026, [https://blogs.oracle.com/futurestate/change-management-ais-secret-weapon-part-1](https://blogs.oracle.com/futurestate/change-management-ais-secret-weapon-part-1)  
30. the AI support metrics that actually matter vs the ones that feel good - Reddit, accessed June 14, 2026, [https://www.reddit.com/r/CustomerSuccess/comments/1t5kdbp/the_ai_support_metrics_that_actually_matter_vs/](https://www.reddit.com/r/CustomerSuccess/comments/1t5kdbp/the_ai_support_metrics_that_actually_matter_vs/)  
31. AI service desk: Benefits, features + top 8 tools of 2026 - Zendesk, accessed June 14, 2026, [https://www.zendesk.com/service/help-desk-software/ai-help-desk/](https://www.zendesk.com/service/help-desk-software/ai-help-desk/)  
32. 8 Best AI Tools for IT Support Ticket Triage (2026) - Elementum AI, accessed June 14, 2026, [https://www.elementum.ai/blog/ai-tools-for-it-support-ticket-triage](https://www.elementum.ai/blog/ai-tools-for-it-support-ticket-triage)  
33. 5 best AI help desk software solutions for 2025 : r/CustomerSuccess - Reddit, accessed June 14, 2026, [https://www.reddit.com/r/CustomerSuccess/comments/1qh1ehd/5_best_ai_help_desk_software_solutions_for_2025/](https://www.reddit.com/r/CustomerSuccess/comments/1qh1ehd/5_best_ai_help_desk_software_solutions_for_2025/)  
34. 7 best free and open source LLM observability tools - PostHog, accessed June 14, 2026, [https://posthog.com/blog/best-open-source-llm-observability-tools](https://posthog.com/blog/best-open-source-llm-observability-tools)  
35. How to Build Feedback Loops for LLMs | newline, accessed June 14, 2026, [https://www.newline.co/@zaoyang/how-to-build-feedback-loops-for-llms--386075af](https://www.newline.co/@zaoyang/how-to-build-feedback-loops-for-llms--386075af)  
36. A list of metrics for evaluating LLM-generated content - Microsoft Learn, accessed June 14, 2026, [https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/working-with-llms/evaluation/list-of-eval-metrics](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/working-with-llms/evaluation/list-of-eval-metrics)  
37. How I finally got LLMs to return reliable structured data - DEV Community, accessed June 14, 2026, [https://dev.to/__c1b9e06dc90a7e0a676b/how-i-finally-got-llms-to-return-reliable-structured-data-280i](https://dev.to/__c1b9e06dc90a7e0a676b/how-i-finally-got-llms-to-return-reliable-structured-data-280i)  
38. How JSON Schema Works for LLM Data - Latitude.so, accessed June 14, 2026, [https://latitude.so/blog/how-json-schema-works-for-llm-data](https://latitude.so/blog/how-json-schema-works-for-llm-data)  
39. AI products - Handbook - PostHog, accessed June 14, 2026, [https://posthog.com/handbook/engineering/ai/products](https://posthog.com/handbook/engineering/ai/products)

---

# AI-ENG-AH — Build, Buy, Open Source & Vendor Strategy - Doctrinal Knowledge Base for Strategic Dependency Architecture

## **Conceptual Glossary of AI Sourcing Architecture**

To establish a mathematically precise and legally rigorous foundation for enterprise technology acquisition, the vocabulary of artificial intelligence sourcing must be strictly standardized:

* **AI Sourcing Architecture**: The technical and procurement discipline of designing, deploying, and continuously auditing the mix of proprietary interfaces, self-hosted models, open-weight artifacts, and native code layers that comprise an enterprise execution plane. It establishes how an organization balances operational agility against systemic dependency risk.  
* **Build**: The direct engineering and deployment of proprietary software components, custom orchestration layers, fine-tuned model adapters, or specialized base-model architectures on managed or owned infrastructure.  
* **Buy**: The commercial acquisition of fully integrated, vendor-hosted applications where the software vendor defines the user interface, workflow engine, orchestration logic, and underlying model endpoints.  
* **Managed API**: A consumption-based cloud endpoint hosted by a model provider that delivers inference outputs via standard HTTPS requests, charging fees based on token or request volume while withholding access to model weights and parameters.  
* **Vendor Application**: A finished software-as-a-service (SaaS) product that wraps model capabilities within a specific business workflow, assuming operational control over retrieval pipelines, model routing, and output formatting.  
* **Open Source Software (OSS)**: Code distributed under licenses approved by the Open Source Initiative (OSI) that grant the unrestricted rights to use, study, modify, and share the software system.1  
* **Open Weight Model**: An artificial intelligence model whose learned parameters and weights are made available for download, but whose commercial distribution, training modifications, and usage scale are governed by custom, non-OSI-compliant community agreements.3  
* **Source-Available Model**: A machine learning model whose code and parameter architectures are visible for audit and study, but whose licensing explicitly restricts commercial application, redistribution, or competitive derivative creation.3  
* **Self-Hosting**: The operation of open-weight or proprietary models on infrastructure directly leased, owned, or logically isolated by the deploying organization, including virtual private clouds (VPCs) and private hardware clusters.  
* **Local Deployment**: The execution of models on physical, non-cloud edge hardware, local workstations, or closed on-premises data centers, operating entirely decoupled from external network dependencies.  
* **Hybrid Architecture**: An engineering pattern that partitions execution across both proprietary managed endpoints and self-hosted models, controlled by an internally owned gateway layer to balance cost, latency, and compliance constraints.5  
* **Control Plane**: The central architectural layer that governs routing, identity and access management, policy enforcement, semantic caching, rate limiting, and audit logging across all underlying model executions.5  
* **Execution Plane**: The runtime infrastructure where raw inference calculations are executed, whether hosted on a vendor's public cloud API, a private GPU cluster, or a localized edge device.8  
* **Data Rights**: The contractual terms governing who owns, can access, and can use inputs (prompts), outputs (completions), vector embeddings, telemetry logs, and user feedback generated during model operations.9  
* **Lock-In**: The structural, mathematical, and financial coupling of an application to a specific model provider's API, prompt formatting, embedding coordinate geometry, or commercial terms.11  
* **Exit Plan**: A pre-vetted, contractually supported, and technically validated decommissioning protocol that details how an organization can transition a workload away from a vendor without service interruption, data loss, or degradation of system behavior.10  
* **Real Total Cost of Ownership (TCO)**: The comprehensive, fully loaded financial calculation of all capital and operational expenditures associated with an AI system throughout its lifecycle, extending far beyond raw infrastructure or subscription pricing.14

## **AI Sourcing Strategy Doctrine**

The core tenet of modern enterprise system design is that sourcing decisions must not be driven by model leaderboard rankings, developer advocacy, or temporary demonstration quality. Sourcing strategy is a discipline of strategic dependency design. The primary objective is to optimize for capability, control, cost, compliance, security, portability, and long-term lifecycle viability. The modern enterprise must operate under a foundational mandate: **own the control plane; vary the execution plane**.5

```
                  ┌──────────────────────────────────────────────┐  
                  │          ENTERPRISE CONTROL PLANE            │  
                  │  (SSO/RBAC, CEL Routing, Semantic Cache,     │  
                  │   Audit Logging, Policy Gates, Evals)        │  
                  └──────────────────────┬───────────────────────┘  
                                         │  
                 ┌───────────────────────┼───────────────────────┐  
                 ▼                       ▼                       ▼  
     ┌───────────────────────┐ ┌───────────────────┐ ┌───────────────────────┐  
     │    PROPRIETARY API    │ │  PRIVATE CLOUD    │ │     LOCAL / ON-PREM   │  
     │   (Managed Frontier)  │ │ (Open/Self-Host)  │ │   (Sensitive/Edge)    │  
     └───────────────────────┘ └───────────────────┘ └───────────────────────┘  
     └───────────────────────────────────┬───────────────────────────────────┘  
                                         ▼  
                             ┌───────────────────────┐  
                             │    EXECUTION PLANE    │  
                             └───────────────────────┘
```

An organization cedes strategic viability when it allows its core business logic, proprietary data representations, and user feedback systems to become trapped inside vendor-owned wrappers. The strategic value of artificial intelligence does not reside in the commodity intelligence rented from frontier foundation model providers; it resides in the proprietary data corpus, the domain-specific evaluation metrics, the workflow integration, and the accumulated user interaction loops.  
Consequently, enterprise architects must construct an internal control plane that abstracts away provider-specific dependencies.12 By intercepting all traffic through an internally managed gateway, the enterprise can dynamically direct workloads to the cheapest, fastest, or most compliant execution environment without altering application code.12  
Workloads must be ruthlessly classified. Generic reasoning tasks can be rented from managed APIs, provided robust data boundaries and zero-data-retention terms are contractually guaranteed.9 Conversely, core product differentiators, regulated data pipelines, and high-frequency automated workflows must utilize self-hosted open-weight models, private clouds, or hybrid routing to preserve operational autonomy, ensure cost predictability at scale, and eliminate the existential risk of vendor deprecation.16  
This approach inherits critical constraints from surrounding engineering disciplines:

* **Product-Fit Alignment**: Sourcing choices must directly map to real, validated user workflows and acceptable latency bounds, rejecting complex multi-billion parameter configurations if an optimized small local model satisfies the use case.6  
* **Adoption Compatibility**: Vendor selections must integrate cleanly with corporate training infrastructures, ensuring model behaviors are explainable, supportable, and aligned with real feedback loops.9  
* **Environmental Sustainability**: Sourcing decisions must calculate hardware utilization, power consumption, and carbon impact, balancing local GPU operations against the efficiency gains of centralized cloud architectures.16  
* **Supply Chain Resilience**: Downloaded models, libraries, and containers must undergo rigorous Software Bill of Materials (SBOM) audits to verify provenance, validate cryptographic trust, and eliminate unpatched vulnerability vectors.24

## **Build, Buy, Open, and Hybrid Decision Matrix**

AI sourcing strategy is not a binary build-versus-buy question. Enterprises must choose among finished vendor applications, managed APIs, hosted open models, self-hosted models, open-source stack assembly, in-house build, and hybrid control-plane architectures. The right answer depends on workflow differentiation, data sensitivity, latency, cost profile, internal engineering capacity, compliance exposure, portability needs, and exit risk.

| Sourcing Category | Best-Fit Conditions | Primary Advantages | Primary Risks | Governance Requirements | Cost Shape | Operational Burden | Exit Complexity |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Buy Vendor Application** | Commodity workflow; limited internal engineering capacity; speed-to-market matters more than deep customization. | Fast deployment, managed UX, vendor support, reduced platform burden. | Workflow lock-in, opaque model behavior, subprocessor cascade, limited eval visibility, roadmap dependence. | Vendor risk review, DPA, subprocessor tracking, export rights, audit logging, admin controls. | Seat/license fees plus possible usage tiers. | Low internally, but vendor-management burden remains. | Low to extreme depending on data export, API access, workflow depth, and user retraining cost. |
| **Managed API** | Need frontier capability, variable/spiky demand, rapid experimentation, or multimodal functionality. | Strong capability, fast integration, no hardware operations, elastic scaling. | Provider drift, rate limits, deprecation, data boundary concerns, price changes. | Gateway abstraction, version/eval controls, quota limits, data processing terms, fallback route. | Usage-based; can scale sharply with volume/context. | Medium: integration, gateway, eval, retry, and monitoring required. | Moderate if gateway and eval abstraction exist; high if provider SDKs/prompts are hardcoded. |
| **Hosted Open Model** | Need more control than proprietary API without full self-hosting burden. | More model transparency, potentially better data isolation, simpler operations than self-host. | Hosting-provider lock-in, runtime differences, limited hardware control, unclear support boundaries. | Model/license review, hosting DPA, telemetry access, portability test. | Capacity-based or token-based. | Low-to-medium. | Moderate; portable if weights, adapters, configs, prompts, and evals are exportable. |
| **Self-Hosted Private Model** | Regulated data, predictable high volume, sovereign/private deployment, low-latency internal workflows. | Strong control over data, routing, telemetry, and deployment lifecycle. | Hardware/VRAM limits, talent needs, reliability burden, patching, utilization risk. | AI-BOM/SBOM, model provenance, security review, eval gates, access controls, operational runbooks. | Fixed capacity plus staffing, power, cooling, and lifecycle costs. | High. | Low for weights, higher for serving stack, adapters, quantization format, and operational knowledge. |
| **Open-Source Stack Assembly** | Strong platform engineering team; need custom orchestration, RAG, evals, gateways, or tool execution. | High flexibility, lower vendor coupling, internal control. | Dependency sprawl, license compliance, supply-chain vulnerabilities, upgrade burden. | SBOM, license scanning, dependency patching, maintainer health review. | Engineering payroll dominates. | High. | Moderate; open components reduce lock-in but skills and integration choices still bind. |
| **In-House Build** | Core differentiating capability; proprietary data/feedback loop; no vendor product fits. | Maximum strategic control and differentiation. | Scope creep, long timelines, high staffing cost, eval failures, maintenance burden. | Full lifecycle governance, model/data lineage, eval program, security and compliance review. | High to extreme. | Extreme. | Low for ownership; high for internal maintenance obligations. |
| **Hybrid Architecture** | Mixed sensitivity, variable workload classes, need cost optimization and resilience. | Best balance of control, flexibility, and execution-plane diversity when done well. | Orchestration complexity, split debugging, inconsistent behavior across routes. | Owned control plane, policy-as-code, route manifests, eval-by-route, fallback tests. | Potentially optimized, but not automatically cheaper. | High. | Low if abstraction is real; high if “hybrid” is only informal provider sprawl. |

### **Decision Rule**

The preferred architecture is the lowest-complexity sourcing posture that satisfies:

```text
workflow fit
+ data rights
+ risk tier
+ cost-to-serve
+ operational readiness
+ portability
+ strategic differentiation
```

Do not self-host to look sophisticated. Do not buy a vendor app that captures the core workflow. Do not use managed APIs without an exit route. The goal is controlled optionality, not ideological purity.

## **AI Stack Ownership Map**

An AI system is a layered dependency graph. Each layer should be classified by the degree of strategic control required. Not every layer must be literally owned, but every critical layer must be controlled through architecture, contract, abstraction, export rights, or operational policy.

```text
AI STACK CONTROL MAP

[ User Interface / Workflow Surface ]
        |
[ Prompts / Context / Product Logic ]
        |
[ Evals / Rubrics / Quality Gates ]
        |
[ Internal Gateway / Routing / Policy Plane ]
        |
[ Retrieval Corpus / Embeddings / Indexes ]
        |
[ Tool and Action Execution Layer ]
        |
[ Telemetry / Feedback / Audit Evidence ]
        |
[ Model Providers / Weights / Serving Runtime ]
        |
[ Compute Infrastructure / Hosting Region ]
```

### **Control States**

| Control State | Meaning |
| :---- | :---- |
| **Own** | Enterprise directly controls source, data, configuration, or artifact. |
| **Control** | Enterprise may not own the layer, but governs access, policy, export, and behavior. |
| **Abstract** | Enterprise hides implementation behind a stable internal interface. |
| **Rent** | Enterprise consumes external capability while preserving portability. |
| **Escrow / Export** | Enterprise requires contractual recovery, migration, or extraction rights. |
| **Avoid** | Layer creates unacceptable lock-in, data exposure, legal, or operational risk. |

### **Layer Ownership and Control Requirements**

| AI Stack Layer | Target Control State | Rationale | Minimum Control Requirement |
| :---- | :---- | :---- | :---- |
| **User Interface / Workflow Surface** | Own or strongly control. | The workflow surface captures user habit, trust, corrections, and process structure. | Exportable workflow data, admin controls, UX authority, role-specific configuration. |
| **Prompts / Context Compiler** | Own. | Prompts encode domain logic and task policy. | Version-controlled prompt library, route-specific prompt tests, migration path across models. |
| **Model Route and Provider Selection** | Abstract. | Provider choice should not be hardcoded into applications. | Internal gateway, route aliases, model manifests, fallback configuration. |
| **Base Model Weights** | Rent, swap, or own depending on use case. | Weights are powerful but not always strategic. | License review, eval baseline, portability assessment, version tracking. |
| **Fine-Tunes / Adapters** | Own or export. | Adapters may encode proprietary behavior and training investment. | Contractual export rights, lineage, training-data record, reproducibility notes. |
| **Retrieval Corpus / Knowledge Base** | Own. | Proprietary knowledge is usually more strategic than the base model. | Source ownership, access controls, lineage, retention policy, source-of-record hierarchy. |
| **Embeddings / Vector Indexes** | Own or preserve migration path. | Embedding geometry can create representation lock-in. | Embedding model registry, reindex plan, raw source preservation, index export. |
| **Rerankers / Search Components** | Abstract or own. | Search quality matters, but components should be swappable. | Stable retrieval interface, eval set, ranking metrics, fallback search. |
| **Tool and Action Execution Layer** | Own/control. | This is the system boundary where AI can cause side effects. | Internal authorization, idempotency, action verification, sandboxing, audit logs. |
| **Policy Engine / Access Control** | Own/control. | Policy cannot be delegated blindly to vendors. | RBAC/ABAC, tenant boundaries, policy-as-code, pre-egress controls. |
| **Evaluation Sets and Rubrics** | Own. | Evals define whether models work for the enterprise’s task. | Proprietary golden sets, evaluation harness, model/provider comparison history. |
| **Telemetry and Observability** | Own/control. | Multi-provider systems require independent visibility. | Internal traces, normalized metrics, retention policy, provider-independent dashboards. |
| **Feedback and Correction Data** | Own/control. | User corrections improve workflow and create proprietary learning loops. | Local capture, no-training default unless approved, export rights, privacy controls. |
| **Audit Trails and Evidence** | Own/control with retention policy. | Compliance evidence must survive vendor changes. | Secure references, evidence package, retention/deletion rules, audit access. |
| **Deployment Infrastructure** | Rent or own. | Compute can be rented if workloads remain portable. | Containerization, IaC, region controls, fallback environment, capacity plan. |
| **Support and Adoption Materials** | Own. | Adoption depends on local workflows and roles. | Internal playbooks, training records, champion feedback, workflow-specific examples. |

The durable rule is: own what differentiates you, control what can harm you, abstract what changes quickly, and rent what is commodity.

## **Open Source, Open Weight, and License Model**

A downloadable model is not automatically open source. AI sourcing must distinguish open-source software, open-source AI systems, permissive open-weight models, restricted open-weight models, research-only models, source-available systems, and hosted proprietary APIs.

The term “open source” should be used carefully. Under the Open Source AI Definition, an Open Source AI system must provide freedoms to use, study, modify, and share, and must make available the preferred forms needed to modify the system, including sufficient data information, code, and parameters.

```text
MODEL / AI ASSET LICENSING FLOW

[ AI Asset ]
      |
      v
[ Does it grant rights to use, study, modify, and share? ]
      |
      +-- no --> [ Proprietary / Source-Available / Restricted ]
      |
      v
[ Are required modification artifacts available? ]
  data information | code | parameters
      |
      +-- no --> [ Open Weights or Source-Available, not full Open Source AI ]
      |
      v
[ Are usage, field, scale, or competition restrictions absent? ]
      |
      +-- no --> [ Restricted Open Weights / Custom License ]
      |
      v
[ Open Source AI Candidate ]
```

### **License and Asset Taxonomy**

| Asset Class | Typical Artifacts Available | Commercial Rights | Strategic Risk | Procurement Treatment |
| :---- | :---- | :---- | :---- | :---- |
| **Open Source Software** | Source code under OSI-approved license. | Usually broad commercial use, depending on license. | Standard OSS dependency risk. | SBOM, license scan, vulnerability management. |
| **Open Source AI System** | Data information, training/inference code, parameters, and rights to use/study/modify/share. | Broad rights if definition and license obligations are met. | Capability, provenance, data documentation, maintenance quality. | Legal review plus AI-BOM/model card/provenance review. |
| **Permissive Open-Weight Model** | Weights and sometimes inference/training code under permissive terms. | Often broad commercial use. | May not satisfy full Open Source AI definition if training data information or code is incomplete. | License review, provenance review, model-risk evaluation. |
| **Restricted Open-Weight Model** | Weights available under custom community or acceptable-use license. | Commercial use may be allowed but limited by scale, field, competition, or redistribution terms. | Hidden restrictions, acquisition diligence risk, migration limits. | Legal approval required before production. |
| **Research / Non-Commercial Model** | Weights/code available for evaluation or academic use. | Commercial production prohibited or unclear. | Accidental production misuse. | Sandbox only unless commercial license obtained. |
| **Source-Available System** | Code or architecture visible but rights restricted. | Limited by license. | Misclassified as open source; legal exposure. | Treat as proprietary for procurement purposes. |
| **Hosted Proprietary API** | No weights; behavior exposed through service endpoint. | Governed by contract and terms of service. | Provider lock-in, data rights, drift, deprecation. | Contract, DPA, SLA, data-use review, exit plan. |

### **Required License Review Questions**

| Review Question | Why It Matters |
| :---- | :---- |
| Does the license allow commercial use? | Prevents accidental non-commercial use in production. |
| Are there scale thresholds or revenue/user caps? | Large enterprises may trip thresholds through affiliates or parent-company aggregation. |
| Are there field-of-use restrictions? | Some licenses ban particular industries, products, or competitive uses. |
| Are outputs restricted? | Output-use limits can block synthetic data, distillation, or migration strategies. |
| Are derivative models allowed? | Fine-tuning, adapters, merges, and distillation may be restricted. |
| Is redistribution allowed? | Determines whether the model can be packaged in products or shipped to customers. |
| Is there an explicit patent grant? | Important for enterprise legal risk, especially for code and model artifacts. |
| Are training data details available? | Required for many provenance, risk, and “open source AI” claims. |
| Are acceptable-use policies incorporated by reference? | Policy changes may alter usable scope over time. |
| Does the license survive acquisition, affiliate use, and subcontractor deployment? | Enterprise usage rarely maps cleanly to one legal entity. |

### **Procurement Rule**

Do not ask, “Is the model open?” Ask:

```text
What exact artifact is available?
Under what exact license?
For what exact use?
At what scale?
With what derivative rights?
With what output rights?
With what provenance?
With what exit path?
```

The answer should be recorded in the AI inventory before production use.

## **Managed API Evaluation Model**

Managed APIs provide rapid access to high-capability models without operating the underlying infrastructure. They are often the correct sourcing choice for frontier reasoning, variable demand, multimodal workloads, and fast experimentation. They also introduce dependency, drift, data, availability, cost, and portability risks.

### **Managed API Risk and Control Matrix**

| Evaluation Vector | Risk | Required Control | Risk-Tier Notes |
| :---- | :---- | :---- | :---- |
| **Model Drift** | Provider updates can change output quality, formatting, latency, or safety behavior. | Private eval sets, route-specific regression tests, change monitoring. | Critical workflows may require scheduled evals; low-risk workflows may use periodic sampling. |
| **Endpoint Deprecation** | Model versions may be retired or renamed. | Internal route aliases, migration calendar, fallback model, deprecation notice tracking. | Production routes should never depend on undocumented endpoints. |
| **Version Pinning** | Auto-updating endpoints can silently alter behavior. | Pin model versions where supported; otherwise eval-gate provider changes. | If pinning is unavailable, increase monitoring and fallback readiness. |
| **Rate Limits and Throttling** | Bursts can create user-visible failures or agent stalls. | Gateway quotas, token budgets, backoff queues, circuit breakers. | High-volume workflows need capacity reservation or multi-provider fallback. |
| **Availability and SLA** | Provider outage can interrupt business workflow. | SLA review, status monitoring, fallback route, degraded mode. | SLA requirements should match workflow criticality. |
| **Data Residency** | Requests may be processed or stored in disallowed regions. | Region controls, DPA review, geo-aware routing, approved provider list. | Regulated workflows require explicit evidence. |
| **Data Use and Retention** | Prompts, outputs, files, logs, or telemetry may be retained or used in ways the enterprise does not permit. | Contractual no-training terms, retention limits, enterprise configuration, audit rights. | Consumer tools should not be assumed safe for enterprise data. |
| **Cost Volatility** | Pricing changes, long contexts, retries, and agents can inflate spend. | Cost telemetry, budget caps, route optimization, semantic/prefix caching where safe. | Agentic workflows need hard budget and loop limits. |
| **Provider-Specific Behavior** | Prompts and outputs become tuned to one model’s quirks. | Multi-model evals, prompt compiler, response schemas, portability tests. | Critical workflows should run migration drills. |
| **Security Boundary** | API keys, files, tool outputs, or PII can leak through integration paths. | Key vaulting, secret rotation, DLP, request filtering, tenant isolation. | Central gateway strongly preferred. |

### **Gateway Abstraction Imperative**

Application code should call an internal AI gateway rather than directly hardcoding provider SDKs and endpoints.

```text
[ Application ]
      |
      v
[ Enterprise AI Gateway ]
  identity | quota | policy | routing | logging | cache | eval hooks
      |
      +--> Provider A
      +--> Provider B
      +--> Hosted open model
      +--> Self-hosted model
      +--> Degraded deterministic route
```

### **Gateway Responsibilities**

| Responsibility | Purpose |
| :---- | :---- |
| **Credential Management** | Centralize provider keys, rotate secrets, and prevent developer key sprawl. |
| **Policy Enforcement** | Apply data, tenant, risk, and tool-use policy before requests leave enterprise control. |
| **Quota and Budget Control** | Enforce usage limits by tenant, app, user, route, and workflow. |
| **Routing and Fallback** | Move traffic across providers or local models without application rewrites. |
| **Version and Route Registry** | Record which model/provider/config served each request. |
| **Caching Controls** | Use prefix or semantic caching only with tenant, freshness, permission, and policy scope. |
| **Telemetry Normalization** | Normalize latency, token, cost, error, and quality signals across providers. |
| **Evaluation Hooks** | Run sampled or scheduled evals by route and risk tier. |
| **Degraded Modes** | Route to deterministic, cached, local, or human-review fallback when providers fail. |

Managed APIs are rented capability. The enterprise control plane is what prevents rented capability from becoming strategic captivity.

## **Vendor Application Evaluation Model**

Buying a finished AI vendor application can be the right decision when the workflow is commodity, the vendor product fits the user surface, internal engineering capacity is limited, and speed-to-market matters. The risk is that the vendor may capture the workflow, user feedback, data flows, evaluation surface, and operational dependency.

```text
VENDOR APPLICATION DEPENDENCY CHAIN

[ Enterprise Buyer ]
        |
        v
[ Vendor Application / SaaS UI ]
        |
        +--> model provider
        +--> cloud host
        +--> analytics provider
        +--> support tooling
        +--> subprocessors
        +--> data stores
```

### **Vendor Application Evaluation Matrix**

| Evaluation Area | Risk Question | Required Evidence / Control |
| :---- | :---- | :---- |
| **Workflow Fit** | Does the vendor product fit the actual workflow, or will users create side channels? | Workflow test, pilot feedback, admin configurability, role-based UX review. |
| **Data Ownership** | Who owns prompts, outputs, files, embeddings, corrections, and feedback? | Contractual data ownership clause and export rights. |
| **Subprocessor Chain** | Which downstream providers touch data or model execution? | Current subprocessor list, notification terms, DPA, data-flow diagram. |
| **Model Transparency** | What models, providers, and routing logic are used? | Model/provider disclosure appropriate to risk tier, change-notice terms. |
| **Admin and Identity Controls** | Can the enterprise manage users, roles, groups, and access centrally? | SSO/OIDC/SAML, SCIM, RBAC/ABAC, audit logs. |
| **Data Protection** | What is retained, where, for how long, and for what purpose? | Retention policy, no-training terms, region controls, deletion terms. |
| **Evaluation and Auditability** | Can the enterprise independently verify quality and risk? | Exportable logs, eval access, audit evidence, incident reports. |
| **Integration Depth** | Does the vendor integrate with systems of record without brittle copying? | API coverage, webhooks, event logs, transaction boundaries. |
| **Operational Stability** | What happens if the vendor changes models, pricing, roadmap, or support? | SLA, deprecation notice, change notice, financial health review. |
| **Exit Path** | Can the enterprise leave without losing data, workflow continuity, or audit evidence? | Export format, termination assistance, deletion certificate, migration plan. |

### **Wrapper Risk**

A vendor application may be a thin workflow layer over a foundation model API. This is not automatically bad, but the buyer must understand what value the vendor actually provides.

| Vendor Value Layer | Good Sign | Warning Sign |
| :---- | :---- | :---- |
| **Workflow Design** | Vendor deeply understands the business process. | Generic chat UI over customer data. |
| **Data Integration** | Clean connectors, permissions, lineage, and source sync. | Manual uploads and unclear access boundaries. |
| **Evaluation** | Vendor can show task-specific quality evidence. | Only demos and generic benchmark claims. |
| **Governance** | Admin controls, audit trails, retention, and policy support. | “Trust us” controls with no evidence export. |
| **Model Operations** | Provider changes are tested and communicated. | Silent model swaps and no version visibility. |
| **Support** | Clear incident path and customer success process. | No meaningful escalation beyond generic support. |

### **Federal Procurement Planning Note**

Some federal procurement environments may impose special AI disclosure, data-handling, domestic-sourcing, or use-rights requirements. Proposed GSA AI terms in 2026 illustrate the direction of travel: disclosure of AI systems used in contract performance, downstream provider visibility, data segregation, and special government-use rights may become relevant for contractors.

Treat these requirements as jurisdiction- and contract-specific. Do not generalize draft federal procurement terms into universal enterprise doctrine. For government-related work, legal and contracts teams must verify the current clause status before procurement approval.

### **Vendor Application Decision Rule**

Buy the application when:

```text
workflow is commodity
+ vendor fit is strong
+ data rights are clear
+ exit is tolerable
+ integration burden is lower than building
+ the vendor’s roadmap risk is acceptable
```

Do not buy when the vendor would own the enterprise’s strategic workflow, feedback loop, evaluation surface, or customer relationship.

## **Open Model and Self-Hosting Strategy**

Self-hosting open-weight or proprietary models can improve data control, cost predictability, latency, and sovereign deployment posture. It also transfers operational responsibility to the enterprise. The organization becomes responsible for serving reliability, GPU capacity, memory pressure, patching, model upgrades, telemetry, security, evaluation, and incident response.

### **Self-Hosting Decision Drivers**

| Driver | Self-Hosting Is Attractive When | Warning |
| :---- | :---- | :---- |
| **Data Sensitivity** | Data cannot leave a private environment or approved region. | Self-hosting does not automatically solve access control or logging risk. |
| **Volume Predictability** | Workload is high-volume and steady enough to utilize reserved capacity. | Low utilization destroys the economics. |
| **Latency** | Local/private inference meaningfully improves user experience or operational SLOs. | Model size and batching may still dominate latency. |
| **Cost Control** | Fixed capacity beats variable token pricing at expected utilization. | Staffing and lifecycle costs must be included. |
| **Portability** | Organization needs model and route independence. | Quantization/runtime choices can create new lock-in. |
| **Regulatory / Sovereign Need** | Contract or law requires controlled environment. | Governance burden usually rises, not falls. |

### **VRAM and Memory Planning**

VRAM is often the binding constraint in self-hosted inference. A simple parameter-memory heuristic is useful for early planning, but production sizing must include KV cache, batch size, context length, precision, runtime overhead, parallelism, and fragmentation.

```text
Approximate model memory:
  model_weight_memory
+ KV_cache_memory
+ runtime_overhead
+ batch_overhead
+ safety_margin
<= available_accelerator_memory
```

### **Parameter Memory Heuristic**

| Precision / Quantization | Approximate Weight Memory Per Billion Parameters | Notes |
| :---- | :---- | :---- |
| **FP16 / BF16** | ~2 GB per 1B parameters. | Higher quality and compatibility; expensive memory footprint. |
| **INT8 / 8-bit** | ~1 GB per 1B parameters. | Common compression point. |
| **4-bit** | ~0.5 GB per 1B parameters. | Useful heuristic; actual memory varies by quantization method and runtime. |
| **Lower than 4-bit** | Less than ~0.5 GB per 1B parameters. | Requires careful quality and compatibility testing. |

### **KV Cache Scaling**

The KV cache scales with:

```text
context length
× batch size
× number of layers
× hidden dimensions / attention heads
× cache precision
```

Longer context windows and larger batches can exhaust VRAM even when model weights fit. Increasing context is not free. It can reduce throughput, increase memory pressure, and force smaller batch sizes. For many enterprise workflows, better retrieval, chunking, reranking, and context compression are more efficient than simply expanding the active context window.

### **Common Self-Hosting Failure Modes**

| Failure Mode | Cause | Mitigation |
| :---- | :---- | :---- |
| **Model fits alone but fails under traffic** | KV cache and batch overhead were ignored. | Load test with realistic context lengths and concurrency. |
| **CPU offload / fallback collapse** | VRAM exceeded; runtime spills to system memory. | Reduce model size, precision, context, batch, or use larger GPU pool. |
| **Low utilization economics** | Reserved GPUs sit idle. | Consolidate routes, batch jobs, autoscale, or use managed API for spiky demand. |
| **Quality regression after quantization** | Compression harms target task behavior. | Run task-specific evals for every precision tier. |
| **Serving runtime lock-in** | Runtime-specific config, kernels, or formats become hard to migrate. | Document runtime, quantization format, adapter format, and migration tests. |
| **Patch and driver drift** | CUDA, drivers, runtime, and dependencies diverge. | Maintain reproducible images, SBOM, patch cadence, and rollback images. |

Self-hosting is a control strategy, not a virtue signal. It should be chosen when the organization can operate the system better than it can rent it.

## **Local Deployment Readiness Model**

Local, on-premises, private-cloud, or edge deployment requires more than purchasing GPUs. The organization must be ready to operate hardware, serving runtimes, security controls, telemetry, model lifecycle, incident response, and user support.

### **Readiness Tiers**

| Readiness Tier | Description | Allowed Use |
| :---- | :---- | :---- |
| **Tier 0: Not Ready** | No clear owner, hardware plan, security model, evals, or operations path. | No deployment. |
| **Tier 1: Prototype Only** | Small team can run local models experimentally, but no production SLO or governance. | Offline experiments with non-sensitive or approved test data. |
| **Tier 2: Pilot Ready** | Serving runtime, access controls, evals, telemetry, and support path exist for bounded scope. | Limited pilot with explicit rollback and human oversight. |
| **Tier 3: Production Ready** | Capacity, reliability, security, evals, monitoring, incident response, and lifecycle management are in place. | Production deployment for approved risk tier. |
| **Tier 4: Regulated / Sovereign Ready** | Strong isolation, audit evidence, data residency, supply-chain controls, and compliance operating model exist. | Regulated, high-sensitivity, sovereign, or air-gapped workflows where appropriate. |

### **Readiness Assessment**

| Dimension | Prototype Only | Pilot Ready | Production Ready | Regulated / Sovereign Ready |
| :---- | :---- | :---- | :---- | :---- |
| **Model Serving Expertise** | Can run model manually. | Can deploy serving runtime with basic monitoring. | Can operate runtime with batching, autoscaling, rollback, and SLOs. | Can validate runtime in isolated or compliant environment. |
| **Hardware / Capacity** | Single workstation or ad-hoc GPU. | Known capacity for pilot workload. | Capacity plan, utilization targets, failover, lifecycle plan. | Dedicated or controlled infrastructure with audit evidence. |
| **Power / Cooling / Facility** | Informal. | Verified pilot environment. | Documented power, cooling, thermal, and maintenance plan. | Facility controls meet regulatory/security needs. |
| **Security** | Local access only. | SSO or controlled access for pilot users. | RBAC/ABAC, secrets, network controls, tenant isolation. | Strong isolation, evidence logging, vulnerability management. |
| **Model Provenance** | Informal download. | Hashes, source, license checked. | AI-BOM/model registry, signed artifacts where available. | Formal supply-chain review and approval. |
| **Evaluation** | Manual spot checks. | Seed eval set. | Regression suite by route/task/risk tier. | Auditable eval evidence and review cadence. |
| **Telemetry** | Logs only. | Basic latency/error/cost traces. | Full operational, quality, utilization, and incident telemetry. | Tamper-resistant evidence appropriate to risk. |
| **Operations** | Best effort. | Named owner and pilot runbook. | On-call path, incident response, rollback, patch cadence. | Formal operating procedures and audit trail. |
| **Support and Training** | Expert-only usage. | Pilot support channel. | User onboarding, support desk, escalation path. | Role-based training and access gating. |

### **Consumer GPU and Commodity Hardware Note**

Consumer or prosumer GPUs can be useful for prototyping, labs, edge cases, and cost-sensitive internal workflows. They may also introduce warranty, availability, driver, cooling, power, memory, and support limitations. Use them deliberately, not because the spreadsheet looked heroic before anyone added operations.


## **Hybrid Architecture Pattern Library**

Hybrid AI architectures combine owned control surfaces with multiple execution environments. Their purpose is not complexity for its own sake. A hybrid pattern is justified when it improves cost, privacy, reliability, latency, compliance, or capability while preserving a coherent control plane.

| Pattern | Routing Logic | Best Fit | Core Control Plane Requirement | Primary Risk | Exit Implication |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Owned Control Plane, Rented Model Plane** | Internal UI/gateway/policy routes to managed APIs. | Fast delivery with strong internal product ownership. | Gateway, route registry, evals, quota, data policy. | Provider drift/deprecation. | Good if provider abstraction is real. |
| **Sensitive-Local / General-Cloud Split** | Sensitive data stays private; non-sensitive complex tasks use cloud. | Mixed data sensitivity and capability needs. | Data classifier, policy gate, route audit. | Misclassification can leak sensitive data. | Strong if local route can operate independently. |
| **Small-Local-First Cascade** | Start with small/private model; escalate when confidence/eval fails. | High-volume bounded tasks with occasional complex cases. | Confidence/eval gate, escalation policy, cost telemetry. | Bad confidence scores create poor routing. | Good if prompts/evals work across routes. |
| **Open Baseline with Proprietary Escalation** | Open/self-hosted model handles standard work; frontier API handles hard cases. | Cost control plus occasional high capability. | Route manifest, model comparison evals, fallback logic. | Behavior inconsistency across model classes. | Strong baseline fallback if proprietary route fails. |
| **Vendor App with Owned Data/Eval Layer** | SaaS app used for workflow; enterprise owns exports, evals, and correction data. | Commodity workflow with strategic data retention needs. | Export contracts, audit logs, internal eval mirror. | Vendor UI/workflow lock-in. | Moderate if data and correction history are portable. |
| **Commodity SaaS plus Internal Gateway** | Vendor app or API must pass through enterprise policy/logging layer. | Teams need SaaS speed but governance cannot be delegated. | Proxy/gateway integration, DLP, identity, audit capture. | Vendor API changes or unsupported interception. | Better if vendor supports official APIs/webhooks. |
| **Private Inference for Regulated Workflows** | Approved workloads route only to isolated private environment. | Regulated, sovereign, confidential, or air-gapped workflows. | Strong identity, audit, model registry, operational runbooks. | High cost and operational burden. | Strong if weights, configs, and data are owned. |
| **Multi-Provider Resilience Route** | Gateway routes across multiple providers based on health, cost, latency, and eval. | Availability-sensitive and provider-risk-sensitive workloads. | Provider abstraction, health checks, eval parity, fallback policy. | Lowest-common-denominator API design. | Strong if route-specific quality is validated. |
| **Batch-Private / Interactive-Managed Split** | Offline jobs use self-hosted batch capacity; interactive tasks use managed API. | Heavy batch workloads plus variable user-facing demand. | Scheduler, workload classifier, cost/carbon telemetry. | Queue delays and inconsistent output profile. | Good if artifacts and evals are shared. |

### **Hybrid Architecture Rule**

Hybrid systems need a single owned policy, telemetry, evaluation, and routing story. Without that, “hybrid” means vendor sprawl wearing a fake mustache.

## **Vendor Selection Scorecard**

AI vendors should be evaluated by risk tier, not by a single universal score threshold. A low-risk internal productivity tool, a customer-facing support agent, and a regulated decision-support workflow do not require identical evidence. They do require a consistent review structure.

### **Scoring Dimensions**

| Dimension | Review Question | Evidence Examples |
| :---- | :---- | :---- |
| **Capability Fit** | Does the vendor solve the actual workflow better than alternatives? | Pilot results, proprietary evals, latency tests, user acceptance data. |
| **Security Posture** | Can the vendor protect data, identities, tenants, and infrastructure? | SOC 2 Type II or equivalent, ISO 27001, pen test summary, vulnerability process. |
| **Data Rights and Retention** | Are prompts, outputs, files, embeddings, telemetry, and feedback protected? | DPA, no-training terms, retention schedule, deletion terms. |
| **Subprocessor Transparency** | Are downstream providers known and controlled? | Subprocessor list, notification terms, data-flow diagram. |
| **Governance and Compliance** | Can the vendor support the system’s regulatory and policy obligations? | Risk classification, audit logs, model documentation, region controls. |
| **Integration Depth** | Does it support enterprise identity, admin, provisioning, and APIs? | SSO/OIDC/SAML, SCIM, RBAC/ABAC, webhooks, admin API. |
| **Operational Stability** | Can the vendor provide continuity, support, and change notice? | SLA, incident history, roadmap, deprecation policy, support terms. |
| **Cost Predictability** | Can the enterprise forecast and cap usage cost? | Pricing schedule, overage controls, usage telemetry, renewal terms. |
| **Portability and Exit** | Can the enterprise leave without losing strategic assets? | Export rights, data format, termination assistance, deletion certificate. |
| **Financial and Strategic Viability** | Will the vendor survive and remain aligned with the buyer’s needs? | Funding/financial review, customer references, roadmap dependency assessment. |

### **Risk-Tier Evidence Requirements**

| Risk Tier | Minimum Review Posture |
| :---- | :---- |
| **Low** | Security questionnaire, DPA where data is processed, basic admin controls, export path. |
| **Medium** | Security evidence, SSO/SCIM where needed, audit logs, retention terms, pilot eval. |
| **High** | Legal/security/governance review, data-flow diagram, subprocessor review, SLA, eval evidence, incident process. |
| **Regulated / Critical** | Formal risk assessment, contractual controls, strong auditability, region controls, exit plan, business-continuity review. |

### **Fatal Flaws**

| Fatal Flaw | Applies Especially To | Decision |
| :---- | :---- | :---- |
| Vendor uses customer data for model training without explicit approved consent. | All enterprise tiers. | Reject or renegotiate. |
| Vendor cannot explain data retention, subprocessors, or region handling. | Medium and above. | Reject until clarified. |
| Vendor cannot support required identity and access controls. | Medium and above. | Reject for production. |
| Vendor blocks export of customer-owned data, prompts, corrections, or audit evidence. | Strategic workflows. | Reject or require contractual fix. |
| Vendor cannot meet required regulatory or contractual constraints. | High / regulated. | Reject. |
| Vendor refuses reasonable security or incident disclosure terms. | Medium and above. | Reject or escalate. |
| Product fails proprietary evaluation baseline. | All AI capability claims. | Reject or limit to non-production. |
| Cost cannot be capped, forecasted, or monitored for expected usage. | High-volume workflows. | Reject or require usage controls. |

A scorecard should support judgment, not replace it. Procurement decisions should record risk tier, accepted residual risks, required mitigations, and the named owner of the vendor relationship.

## **Data Rights and Training-Use Checklist**

AI contracts must define rights and restrictions for prompts, completions, uploaded files, embeddings, vector indexes, telemetry, user corrections, fine-tunes, adapters, logs, and derived artifacts. Marketing assurances are not enough. The controlling terms must appear in the contract, DPA, order form, enterprise settings, or incorporated policy.

### **Contract Review Checklist**

| Checklist Item | Required Question | Required Evidence |
| :---- | :---- | :---- |
| **Processing Purpose** | Is customer data processed solely to provide the contracted service? | DPA/service terms. |
| **No-Training Commitment** | Are prompts, completions, files, embeddings, telemetry, and feedback excluded from vendor model training unless explicitly approved? | Contractual no-training clause or enterprise configuration. |
| **Retention Period** | How long does the vendor retain each data class? | Retention schedule by data type. |
| **Deletion Rights** | Can the customer delete data and receive confirmation? | Deletion workflow, certificate/attestation terms. |
| **Subprocessor Disclosure** | Which downstream providers process customer data? | Subprocessor list and notice period. |
| **Region and Residency** | Where is data processed, stored, logged, and backed up? | Region controls and data-flow diagram. |
| **Human Access** | Can vendor personnel review customer inputs/outputs? | Access policy, support access controls, “eyes-off” terms where needed. |
| **Embeddings and Vector Indexes** | Who owns vector representations and derived search indexes? | Ownership/export clause. |
| **Feedback and Corrections** | Who owns user edits, ratings, corrections, and workflow telemetry? | Feedback data clause and export rights. |
| **Fine-Tunes / Adapters** | Who owns custom weights, adapters, datasets, and resulting artifacts? | Fine-tune ownership/export terms. |
| **Output Rights** | Can outputs be used commercially, redistributed, or used for downstream training? | Output-use clause. |
| **Audit Rights** | Can the customer verify compliance with data terms? | Audit reports, certifications, contractual audit language. |
| **Termination Export** | Can the customer export data before termination? | Export format, timeline, assistance clause. |

### **Consumer and Shadow-AI Caution**

Consumer AI tools often have different data-use, retention, training, and admin-control terms than enterprise products. Current provider policies may allow users to opt out of training or choose data-use settings, but those defaults change and vary by product tier. Enterprises should not rely on consumer settings for corporate data protection.

| Tool Type | Enterprise Treatment |
| :---- | :---- |
| **Consumer AI account** | Block or restrict for confidential, regulated, or customer data unless explicitly approved. |
| **Team / Business account** | Review admin settings, data-use terms, retention, and export controls. |
| **Enterprise contract** | Require negotiated data rights, DPA, retention, audit, and no-training language. |
| **Vendor-embedded AI feature** | Treat as a subprocessed AI system and review data flow. |

### **Data Rights Rule**

No production AI system should launch until the organization can answer:

```text
What data enters?
Where does it go?
Who can see it?
How long is it retained?
Can it train a model?
Who owns derived artifacts?
Can we export it?
Can we delete it?
Can we prove the answers?
```

## **AI Security Review Checklist**

AI security review must cover software supply chain, model artifacts, provider infrastructure, tenant isolation, prompt/tool boundaries, data egress, secrets, and incident response. Traditional SaaS security review is necessary but not sufficient.

| Security Area | Review Question | Required Artifact | Fatal Flaw |
| :---- | :---- | :---- | :---- |
| **Model Provenance** | Do we know where the model, weights, tokenizer, adapters, and runtime came from? | Model registry entry, source URL, hash/signature where available. | Unverified model artifact from unofficial source. |
| **AI-BOM / SBOM** | Are model, code, containers, dependencies, datasets, and tools inventoried? | AI-BOM/SBOM with dependency and license metadata. | No inventory for production dependency. |
| **License Compliance** | Are use, modification, redistribution, output, and training rights understood? | Legal review and license record. | Production use violates license or unclear commercial rights. |
| **Provider Security** | Does hosted provider meet required security posture? | SOC 2/ISO evidence or equivalent, pen-test summary, vulnerability process. | Vendor refuses reasonable security evidence for risk tier. |
| **Tenant Isolation** | Can one tenant/user access another tenant’s data, indexes, prompts, or logs? | Isolation design, access tests, row/index-level controls. | Tenant isolation cannot be demonstrated. |
| **Secrets Management** | Are API keys, tokens, and credentials centrally managed? | Vault/KMS configuration, rotation policy, access logs. | Hardcoded or developer-local production secrets. |
| **Prompt Injection and Data Egress** | Can malicious content redirect the system to reveal secrets or exfiltrate data? | Boundary tests, DLP filters, policy hierarchy, red-team cases. | Model can access or leak unauthorized data. |
| **Tool Execution** | Are model-triggered actions sandboxed, authorized, and verified? | Tool contracts, sandbox policy, idempotency and verification tests. | Model can execute unbounded actions. |
| **Logging and Retention** | Are prompts, outputs, files, and traces stored safely and minimally? | Retention policy, redaction, secure references, access controls. | Sensitive logs stored broadly or indefinitely without justification. |
| **Runtime Security** | Are containers, drivers, serving runtimes, and dependencies patched? | Image registry, vulnerability scan, patch cadence. | Unpatched critical vulnerabilities in production path. |
| **Network Controls** | Is model traffic restricted to approved endpoints and regions? | Egress policy, firewall rules, private networking where needed. | Open outbound network from sensitive AI runtime. |
| **Incident Response** | Can AI-specific incidents be detected and contained? | Runbook, owner, escalation path, rollback route. | No owner or containment path for production AI system. |
| **Vulnerability Disclosure** | Does vendor/project report and remediate security issues? | Disclosure policy, SLA, security contact, CVE history. | No credible security response path for critical dependency. |

AI security review should be performed before procurement approval, before production launch, and after material model, provider, tool, or data-flow changes.

## **Integration Cost Model**

The purchase price of an AI dependency is only one part of integration cost. Real integration cost includes identity, permissions, data preparation, retrieval, evals, telemetry, support, governance, fallback, migration, and user enablement.

Avoid universal engineering-hour estimates. Instead, estimate each cost center from system complexity, number of integrations, data volume, risk tier, and required assurance level.

### **Integration Cost Centers**

| Cost Center | Work Included | Estimation Drivers |
| :---- | :---- | :---- |
| **Identity and Provisioning** | SSO, OIDC/SAML, SCIM, RBAC/ABAC, group mapping. | Number of directories, roles, tenants, environments. |
| **Permission Mapping** | Translating enterprise ACLs into retrieval, search, UI, and tool access. | Number of source systems, permission models, tenant boundaries. |
| **Data Preparation** | Cleaning, parsing, OCR, metadata, deduplication, lineage. | Corpus size, format variety, quality, ownership clarity. |
| **Embedding and Indexing** | Embedding generation, vector DB setup, chunking, reindexing. | Document/chunk count, dimensions, model choice, update frequency. |
| **Retrieval Quality** | Reranking, citation verification, source freshness, conflict handling. | Required precision, source complexity, regulated evidence needs. |
| **Gateway Integration** | Routing, provider abstraction, quota, budget, policy, fallback. | Number of providers/routes/apps/tenants. |
| **Observability** | Traces, logs, cost, latency, quality, adoption telemetry. | Risk tier, retention, dashboard, incident requirements. |
| **Evaluation Program** | Golden sets, regression tests, human review, acceptance gates. | Task complexity, failure consequence, domain expertise needed. |
| **Security and Compliance** | DPA, legal review, SBOM/AI-BOM, pen testing, privacy review. | Data sensitivity, jurisdiction, vendor count, audit obligations. |
| **Support and Adoption** | Training, documentation, champion network, help desk, onboarding. | User count, workflow change depth, role diversity. |
| **Resilience and Fallback** | Circuit breakers, retry queues, degraded modes, alternate providers. | Criticality, SLA, provider dependence, traffic volume. |
| **Exit and Migration** | Export tooling, reindexing, prompt migration, eval comparison, user retraining. | Lock-in dimensions, data volume, workflow coupling. |

### **Reindexing and Embedding Migration Cost**

Changing embedding models often requires rebuilding the vector index. The cost depends on:

```text
number of source documents
× chunks per document
× embedding model cost or infrastructure cost
× dimensions and storage footprint
× metadata reconstruction
× validation and retrieval eval effort
× downtime or blue-green migration requirements
```

The safe planning assumption is that embedding migration is a project, not a config flip.

### **Integration Cost Estimate Template**

| Estimate Field | Value |
| :---- | :---- |
| Use case / workflow |  |
| Risk tier |  |
| Vendor / route |  |
| Number of user groups |  |
| Number of source systems |  |
| Number of data classes |  |
| Corpus size / chunk estimate |  |
| Identity integration complexity | Low / Medium / High |
| Permission mapping complexity | Low / Medium / High |
| Evaluation burden | Low / Medium / High |
| Governance burden | Low / Medium / High |
| Migration/exit burden | Low / Medium / High |
| Internal teams required |  |
| External vendor dependency |  |
| One-time integration estimate |  |
| Recurring operating estimate |  |
| Major assumptions |  |

Integration cost is where “cheap API” fantasies go to be politely mugged by reality.

## **Lock-In Taxonomy**

Lock-in is not a singular commercial concern; it is a multi-dimensional technical dependency. Enterprise architects must identify, track, and mitigate eleven distinct dimensions of lock-in.

```
                  ┌──────────────────────────────────────────────┐  
                  │              LOCK-IN CATEGORY                │  
                  └──────────────────────┬───────────────────────┘  
                                         │  
        ┌───────────────┬────────────────┼───────────────┬───────────────┐  
        ▼               ▼                ▼               ▼               ▼  
  ┌───────────┐   ┌───────────┐    ┌───────────┐   ┌───────────┐   ┌───────────┐  
  │   MODEL   │   │    API    │    │  PROMPT   │   │ EMBEDDING │   │   DATA    │  
  └───────────┘   └───────────┘    └───────────┘   └───────────┘   └───────────┘  
        ▼               ▼                ▼               ▼               ▼  
  ┌───────────┐   ┌───────────┐    ┌───────────┐   ┌───────────┐   ┌───────────┐  
  │ WORKFLOW  │   │   EVAL    │    │ AGENT/TOOL│   │COMMERCIAL │   │COMPLIANCE │  
  └───────────┘   └───────────┘    └───────────┘   └───────────┘   └───────────┘  
                                         ▼  
                                   ┌───────────┐  
                                   │ ADOPTION  │  
                                   └───────────┘
```

1. **Model Lock-In**: Product functionality depends on the unique edge-case behaviors, formatting styles, or reasoning quirks of a specific provider's model.12  
2. **API Lock-In**: Application code directly imports vendor-specific SDKs and endpoints, requiring a codebase refactor to swap providers.12  
3. **Prompt Lock-In**: System prompts are highly optimized and tuned to one model architecture, producing degraded or incorrect outputs when sent to other models.12  
4. **Embedding Lock-In (Representation Lock-In)**: Stored vector indexes are tied to the specific mathematical coordinate system and dimensionality of a single embedding model.11 Upgrading or changing the model renders the entire index obsolete overnight.11  
5. **Data Lock-In**: Interaction history, training data, and user correction logs are stored in a vendor's proprietary cloud and cannot be easily exported.9  
6. **Workflow Lock-In**: Internal business processes are redesigned around a vendor's custom SaaS user interface, making system migration difficult.10  
7. **Eval Lock-In**: Performance monitoring and evaluation metrics are managed exclusively inside a vendor’s proprietary dashboard, preventing independent verification.21  
8. **Agent & Tool Lock-In**: The orchestration platform is tied to a vendor's proprietary tool-calling schema, blocking migration to open-source agent runtimes.23  
9. **Commercial Lock-In**: High usage-based discounts or multi-year commitments hide future pricing increases and limit bargaining leverage.10  
10. **Compliance Lock-In**: Audit records, safety proofs, and regulatory reports reside inside a vendor's system, leaving the organization vulnerable during external compliance audits.27  
11. **Adoption Lock-In**: Staff are trained exclusively on a vendor-specific interface, requiring retraining during system migrations.22

## **AI Vendor Exit Plan Template**

Every production AI dependency must have an exit plan before launch. Exit planning is not pessimism. It is dependency hygiene. Vendors change prices, models, policies, regions, roadmaps, ownership, and terms. Systems that cannot leave cannot negotiate.

### **Exit Plan Summary**

| Field | Required Detail |
| :---- | :---- |
| Vendor / dependency |  |
| Workload / product surface |  |
| Risk tier |  |
| Exit trigger | price, outage, breach, deprecation, performance regression, legal change, roadmap failure, acquisition, etc. |
| Replacement route | secondary provider, self-hosted model, deterministic fallback, manual workflow. |
| RTO target | Risk-tier-specific recovery time objective. |
| RPO target | Risk-tier-specific recovery point objective for data/evidence loss. |
| Critical data to export | prompts, outputs, files, embeddings, indexes, logs, evals, feedback, adapters, configs. |
| Non-exportable data | vendor-owned or unavailable artifacts. |
| User impact | retraining, UI change, workflow disruption. |
| Owner | named business, technical, legal, and operational owners. |

### **Exit Phases**

| Phase | Purpose | Required Actions | Acceptance Criteria |
| :---- | :---- | :---- | :---- |
| **1. Trigger and Triage** | Decide whether exit is required. | Confirm trigger, freeze risky changes, notify owners, preserve evidence. | Exit decision recorded with risk and timeline. |
| **2. Alternative Route Activation** | Maintain service continuity. | Shift traffic through gateway, activate fallback provider/model/manual route. | Error, latency, quality, and safety remain within approved thresholds. |
| **3. Data Export** | Recover customer-owned and operational artifacts. | Export prompts, outputs, files, logs, feedback, configs, eval results, and metadata. | Export manifest reconciles against inventory. |
| **4. Embedding / Index Migration** | Avoid representation lock-in. | Rebuild index with target embedding model; run retrieval evals; perform blue-green cutover. | Retrieval quality and latency meet workload-specific thresholds. |
| **5. Prompt and Route Migration** | Preserve behavior. | Recompile prompts for target model/route; validate schema, safety, and task quality. | Eval suite passes acceptance gate. |
| **6. Tool and Integration Migration** | Preserve side-effect safety. | Rebind APIs, credentials, webhooks, tool schemas, action verification. | End-to-end transaction tests pass. |
| **7. User and Support Transition** | Preserve adoption and continuity. | Update training, support docs, announcements, escalation path. | Users can complete critical workflows. |
| **8. Deletion and Termination** | End dependency cleanly. | Request deletion/return, revoke access, rotate keys, obtain deletion attestation where contractually available. | Legal/procurement sign-off complete. |
| **9. Post-Exit Review** | Improve future sourcing. | Document causes, surprises, missing clauses, migration pain, residual risks. | Lessons feed procurement and architecture standards. |

### **Exit Drill Requirements**

| Dependency Criticality | Drill Cadence |
| :---- | :---- |
| **Low** | Paper review before renewal. |
| **Medium** | Annual export and route-switch test. |
| **High** | Semiannual fallback and data-export test. |
| **Critical / Regulated** | Regular operational drills with measured RTO/RPO and evidence review. |

Exit plans should define thresholds by workload. A support chatbot and a regulated transaction system do not need the same RTO, but both need to know how they leave.

## **Real Total Cost of Ownership (TCO) Model**

AI sourcing decisions must be evaluated using real total cost of ownership, not subscription price or token price alone.

```text
Real TCO =
  C_vendor
+ C_infra
+ C_integration
+ C_governance
+ C_ops
+ C_support
+ C_eval
+ C_security
+ C_risk
+ C_lifecycle
+ C_exit
```

| Cost Variable | Meaning |
| :---- | :---- |
| **C_vendor** | SaaS subscription, managed API usage, support tier, overages, renewal increases. |
| **C_infra** | GPUs, cloud instances, storage, networking, power, cooling, reserved capacity. |
| **C_integration** | Identity, permissions, API work, data pipelines, gateway, reindexing, workflow changes. |
| **C_governance** | Legal review, DPA, procurement, compliance documentation, audit preparation. |
| **C_ops** | Platform engineering, MLOps, SRE, incident response, monitoring. |
| **C_support** | User training, help desk, champion network, documentation, change management. |
| **C_eval** | Golden sets, regression testing, human review, model comparison, eval infrastructure. |
| **C_security** | SBOM/AI-BOM, pen testing, vulnerability management, access review. |
| **C_risk** | Expected loss from outages, breaches, hallucinations, compliance failures, support incidents. |
| **C_lifecycle** | Upgrades, patching, hardware refresh, model migration, data retention, decommissioning. |
| **C_exit** | Export, reindexing, prompt migration, retraining, contract termination, deletion verification. |

### **TCO Worksheet**

| Cost Center | Year 0 / Setup | Annual Recurring | Scaling Driver | Confidence |
| :---- | :---- | :---- | :---- | :---- |
| Vendor fees |  |  | seats / tokens / requests / usage tier | High / Medium / Low |
| Infrastructure |  |  | GPU hours / storage / traffic / regions | High / Medium / Low |
| Integration |  |  | source systems / APIs / identity / data volume | High / Medium / Low |
| Governance and legal |  |  | risk tier / jurisdictions / vendors | High / Medium / Low |
| Operations |  |  | routes / SLOs / incidents / model count | High / Medium / Low |
| Support and adoption |  |  | users / roles / training intensity | High / Medium / Low |
| Evaluation |  |  | task types / risk / release cadence | High / Medium / Low |
| Security |  |  | dependencies / data sensitivity / tools | High / Medium / Low |
| Lifecycle |  |  | hardware / model changes / retention | High / Medium / Low |
| Exit |  |  | lock-in dimensions / data volume / migration complexity | High / Medium / Low |

### **Sensitivity Variables**

| Variable | Why It Matters |
| :---- | :---- |
| Token volume growth | Turns small usage fees into major COGS. |
| Context length | Drives input token cost and latency. |
| Output length | Drives generation cost and review burden. |
| Retry / failure rate | Creates hidden cost. |
| Human review time | Can erase AI productivity gains. |
| Utilization | Determines whether self-hosting economics work. |
| Provider price changes | Alters managed API/vendor app economics. |
| Data growth | Drives storage, embedding, and reindexing cost. |
| Risk tier changes | Increases governance, eval, and audit cost. |
| Migration frequency | Turns exit cost into recurring architecture burden. |

### **TCO Comparison Template**

| Decision Option | Strength | Major Cost Driver | Major Risk | Exit Burden | Best When |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Vendor Application** | Fastest workflow deployment. | Seats, premium features, renewal pricing. | Workflow and data lock-in. | Medium to high. | Workflow is commodity and vendor fit is strong. |
| **Managed API** | Fast capability access. | Token/context/retry volume. | Provider drift and cost volatility. | Medium if gateway exists. | Demand is variable or frontier capability matters. |
| **Hosted Open Model** | More control without full ops burden. | Hosting capacity and support. | Runtime/provider dependence. | Medium. | Need data isolation and portable weights. |
| **Self-Hosted Model** | Strong control and predictable capacity. | Staffing, utilization, hardware lifecycle. | Operational complexity. | Medium; depends on serving stack. | High predictable volume or strict data control. |
| **Hybrid Architecture** | Flexible optimization. | Gateway/platform complexity. | Debugging and governance complexity. | Low to medium if designed well. | Workloads differ by sensitivity, cost, and capability. |

TCO should be recalculated at renewal, major model migration, workload growth, incident, regulatory change, and hardware refresh.

## **Procurement Reality Model**

AI procurement is a multi-disciplinary operating process. Legal, security, engineering, governance, finance, product, operations, and the business owner must review different parts of the dependency. The goal is not to slow every request equally. The goal is to route each request through the review depth appropriate to its risk.

```text
AI PROCUREMENT FLOW

[ Intake Request ]
  use case | data classes | users | vendor | model route | risk hypothesis
        |
        v
[ Triage ]
  low | medium | high | regulated / prohibited
        |
        +--> [ Fast Track Review ]
        |
        +--> [ Standard Review ]
        |
        +--> [ Enhanced Review ]
        |
        +--> [ Block / Legal Escalation ]
        |
        v
[ Parallel Review ]
  legal/DPA
  security
  engineering/integration
  governance/compliance
  finance/TCO
  product/workflow fit
  adoption/support
        |
        v
[ Decision ]
  approve
  approve with controls
  sandbox only
  revise and resubmit
  reject
        |
        v
[ Inventory + Monitoring + Renewal Review ]
```

### **Risk-Tier Review Matrix**

| Procurement Tier | Example Use | Review Requirements | Decision Options | Review Cadence |
| :---- | :---- | :---- | :---- | :---- |
| **Tier 1: Low Risk** | Internal productivity tool with non-sensitive data. | Lightweight security/privacy review, acceptable-use rules, vendor terms check. | Approve, approve with limits, sandbox only. | Renewal or material change. |
| **Tier 2: Medium Risk** | Customer-facing drafting, internal RAG, workflow automation with limited data. | DPA, security evidence, data-flow review, admin controls, eval/pilot. | Approve with controls, pilot, revise. | Annual or material change. |
| **Tier 3: High Risk** | Sensitive data, external outputs, important business decisions, automated tool use. | Legal, security, governance, eval, incident plan, exit plan, SLA, subprocessor review. | Controlled approval, limited scope, revise, reject. | Semiannual, renewal, incident, model/provider change. |
| **Tier 4: Regulated / Critical** | High-impact, regulated, safety, employment, finance, healthcare, law, or irreversible actions. | Formal risk assessment, human oversight, audit evidence, model documentation, operational controls. | Executive/legal approval, manual-only support, reject. | Continuous or event-triggered review. |
| **Prohibited / Unacceptable** | Unlawful, discriminatory, non-consensual, unsafe, or policy-prohibited use. | Legal escalation and technical block. | Reject/block. | Immediate. |

### **Review Workstreams**

| Workstream | Reviews |
| :---- | :---- |
| **Product / Business Owner** | Workflow fit, value, user population, adoption burden, success metrics. |
| **Legal / Privacy** | DPA, data rights, retention, subprocessors, SCCs, IP, output rights, deletion. |
| **Security** | Vendor security, tenant isolation, access control, secrets, supply chain, incident terms. |
| **Engineering** | Integration complexity, gateway compatibility, APIs, fallback, telemetry, scalability. |
| **Governance / Compliance** | Risk classification, audit evidence, human oversight, regulatory obligations. |
| **Finance / FinOps** | TCO, cost caps, renewal exposure, usage-based pricing, exit cost. |
| **Operations / Support** | Runbooks, owner, help desk, user support, escalation, incident handling. |
| **Adoption / Enablement** | Training, role impact, champion needs, communication, feedback loop. |

### **Mandatory Procurement Artifacts**

| Artifact | Required When |
| :---- | :---- |
| Intake form | All AI procurements. |
| Data-flow diagram | Medium risk and above. |
| Vendor security evidence | All production uses; depth by risk. |
| DPA / data rights review | Any customer, employee, confidential, or regulated data. |
| AI inventory entry | All approved systems. |
| Evaluation result | Any AI capability claim used in workflow. |
| TCO estimate | Medium/high volume or paid production use. |
| Exit plan | Medium risk and above; all strategic dependencies. |
| Adoption/support plan | Any broad user rollout. |
| Risk acceptance record | Any residual material risk. |

Procurement should end with an owned inventory record, not an email saying “approved.” The inventory record is what enables renewal review, incident response, audit, and exit.

## **Cross-Canon Handoff Map**

AI-ENG-AH defines sourcing architecture and strategic dependency design. It determines when to build, buy, self-host, use open-weight models, consume managed APIs, assemble open-source stacks, or route across hybrid execution planes. It connects product strategy, governance, runtime architecture, security, sustainability, telemetry, evaluation, procurement, and exit planning.

| Canon Report | Handoff Into AH | AH Use | Handoff Back |
| :---- | :---- | :---- | :---- |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, risk tier, product pattern. | Determines whether sourcing is justified and what capability is actually needed. | Sourcing constraints inform product surface and use-case viability. |
| **AI-ENG-AG — Adoption Systems** | Training, support, role impact, feedback loops, adoption burden. | Evaluates vendor support, change cost, and user migration risk. | Vendor choice informs enablement and support architecture. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, procurement controls, accountability, risk classification. | Defines approval paths, vendor obligations, and evidence requirements. | Sourcing decisions populate AI inventory and governance records. |
| **AI-ENG-AE — Sustainable AI** | Energy, lifecycle, hardware utilization, carbon/water impact. | Adds sustainability to build/buy/self-host decisions. | Deployment choice feeds sustainability telemetry and lifecycle review. |
| **AI-ENG-J — Throughput Mechanics** | Batching, latency, queueing, token economics. | Informs serving cost and provider/runtime selection. | Sourcing choices constrain throughput design. |
| **AI-ENG-K — Weight Dynamics** | Quantization, adapters, model variants, portability. | Evaluates open-weight/self-host viability and migration risk. | License and hosting choices constrain model adaptation. |
| **AI-ENG-L — Serving Architecture** | Gateways, routing, fallback, model serving topologies. | Implements “own control plane; vary execution plane.” | Vendor/provider choices become route manifests. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, permissions, side effects. | Evaluates vendor/tool integration risk. | Vendor tools must comply with internal tool contract requirements. |
| **AI-ENG-O — Action Verification** | Verification, idempotency, source-of-record checks. | Determines whether vendor or agentic systems may execute actions. | Procurement requires action verification evidence. |
| **AI-ENG-T — Boundary Defense** | Tenant isolation, prompt injection, egress, data boundaries. | Shapes security review and provider eligibility. | Vendor risks feed boundary defense controls. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, cryptographic trust, vulnerability management. | Drives open-source, open-weight, and vendor security diligence. | Sourcing records feed supply-chain inventory. |
| **AI-ENG-V — Resource Abuse** | Cost bombs, loop limits, denial-of-wallet. | Requires budget caps, gateway quotas, and agent loop controls in contracts/design. | Vendor/API choices feed resource-abuse controls. |
| **AI-ENG-W — UX Resilience** | Fallback, degraded modes, user continuity. | Requires continuity planning and exit-aware user experience. | Sourcing choices define degraded-mode options. |
| **AI-ENG-X — Trust** | Transparency, contestability, user confidence. | Vendor opacity and workflow capture are trust risks. | Vendor disclosures and UI controls feed trust design. |
| **AI-ENG-Y — Human Review** | Human approval, reviewer burden, escalation. | Determines whether vendor app or automation needs human gates. | Sourcing choices affect review workload and staffing. |
| **AI-ENG-Z — Telemetry** | Traces, metrics, cost, latency, quality, adoption telemetry. | Requires provider-independent observability and cost tracking. | Vendor route data feeds telemetry schema. |
| **AI-ENG-AA — Evaluation** | Golden sets, regression gates, model comparisons. | Vendor/model selection must pass proprietary evals. | Sourcing strategy defines eval-by-route requirements. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit records. | Requires exportable evidence and audit trails. | Vendor contracts define evidence availability. |
| **AI-ENG-AC — AI Operations** | Incident response, rollback, runbooks, containment. | Requires vendor incident terms, fallback routes, and exit drills. | Procurement decisions become operational dependencies. |
| **AI-ENG-AJ — Reference Architectures** | Reusable enterprise architecture patterns. | Converts sourcing doctrine into implementation blueprints. | AH patterns become reference architecture variants. |

### **Core Handoff Rule**

Sourcing is architecture. A vendor contract, model license, API endpoint, embedding model, gateway, support term, or export clause can determine whether an AI system is portable, governable, profitable, safe, and durable.

The enterprise should not merely ask:

```text
Which model is best?
```

It should ask:

```text
Which dependencies are we creating?
Who controls them?
How do we verify them?
How do we operate them?
How do we leave?
```

#### **Works cited**

1. Open Source AI, accessed June 15, 2026, [https://opensource.org/ai](https://opensource.org/ai)  
2. The Open Source AI Definition – 1.0 – Open Source Initiative, accessed June 15, 2026, [https://opensource.org/ai/open-source-ai-definition](https://opensource.org/ai/open-source-ai-definition)  
3. The Future of Open Source in the AI Era - Blog - One Horizon, accessed June 15, 2026, [https://onehorizon.ai/blog/the-future-of-open-source-in-the-ai-era](https://onehorizon.ai/blog/the-future-of-open-source-in-the-ai-era)  
4. Meta Llama 3 and the 700M MAU Limit: Who This License Does Not ..., accessed June 15, 2026, [https://wcr.legal/llama-3-license-700m-mau-limit/](https://wcr.legal/llama-3-license-700m-mau-limit/)  
5. AI Gateway Architecture: A Guide for Technical Teams | MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/](https://mlflow.org/articles/ai-gateway-architecture-a-guide-for-technical-teams/)  
6. Hybrid AI routing | Services - SLM-Works, accessed June 15, 2026, [https://slm-works.ai/services/hybrid-routing](https://slm-works.ai/services/hybrid-routing)  
7. LLM Gateway: Architecture, Routing & Governance Guide | Axiom Studio, accessed June 15, 2026, [https://axiomstudio.ai/learn/what-is-an-llm-gateway](https://axiomstudio.ai/learn/what-is-an-llm-gateway)  
8. Deployment - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/deployment](https://www.truefoundry.com/deployment)  
9. Enterprise AI Procurement & Contract Negotiation: The Complete Guide (2026), accessed June 15, 2026, [https://thenegotiationexperts.com/blog/ai-complete-guide](https://thenegotiationexperts.com/blog/ai-complete-guide)  
10. AI Vendor Agreements & Enterprise AI Transactions Explained - Altawil Law Group, accessed June 15, 2026, [https://thepathtojustice.com/ai-vendor-agreements-enterprise-ai-transactions/](https://thepathtojustice.com/ai-vendor-agreements-enterprise-ai-transactions/)  
11. Vendor Lock-In in the Embedding Layer: A Migration Story | by Vassiliki Dalakiari | ITNEXT, accessed June 15, 2026, [https://itnext.io/vendor-lock-in-in-the-embedding-layer-a-migration-story-183ea58e3668](https://itnext.io/vendor-lock-in-in-the-embedding-layer-a-migration-story-183ea58e3668)  
12. What Is an LLM Gateway and How Does It Work? - Truefoundry, accessed June 15, 2026, [https://www.truefoundry.com/blog/llm-gateway](https://www.truefoundry.com/blog/llm-gateway)  
13. Vector Migration Explained: 7 Reasons Moving Vector Data Is Harder Than It Looks | by Syed | Medium, accessed June 15, 2026, [https://medium.com/@syed_33931/vector-migration-explained-7-reasons-moving-vector-data-is-harder-than-it-looks-1edd66f6c439](https://medium.com/@syed_33931/vector-migration-explained-7-reasons-moving-vector-data-is-harder-than-it-looks-1edd66f6c439)  
14. LLM Hosting Cost 2026: Self-Host vs API Pricing Guide - AI Superior, accessed June 15, 2026, [https://aisuperior.com/llm-hosting-cost/](https://aisuperior.com/llm-hosting-cost/)  
15. Self-Hosting LLMs: Hidden Costs You're Missing - Azumo, accessed June 15, 2026, [https://azumo.com/artificial-intelligence/ai-insights/self-hosting-llms-cost](https://azumo.com/artificial-intelligence/ai-insights/self-hosting-llms-cost)  
16. Self-Hosted LLM Guide: Costs, Architecture & Breakeven Point ..., accessed June 15, 2026, [https://alpacked.io/blog/self-hosted-llm-guide/](https://alpacked.io/blog/self-hosted-llm-guide/)  
17. The LLM layer you're probably missing (LLM gateway pattern explained) - Redgate, accessed June 15, 2026, [https://www.red-gate.com/simple-talk/ai/the-llm-layer-youre-probably-missing-llm-gateway-pattern-explained/](https://www.red-gate.com/simple-talk/ai/the-llm-layer-youre-probably-missing-llm-gateway-pattern-explained/)  
18. Top 5 Enterprise AI Gateways for Multi-Model Routing in 2026 - Maxim AI, accessed June 15, 2026, [https://www.getmaxim.ai/articles/top-5-enterprise-ai-gateways-for-multi-model-routing-in-2026/](https://www.getmaxim.ai/articles/top-5-enterprise-ai-gateways-for-multi-model-routing-in-2026/)  
19. DPA for AI vendors: what to actually check before you sign ..., accessed June 15, 2026, [https://companyscope.io/topics/dpa-for-ai-vendors](https://companyscope.io/topics/dpa-for-ai-vendors)  
20. Vector Database Pricing Models: The Hidden Cost, accessed June 15, 2026, [https://www.actian.com/blog/databases/the-hidden-cost-of-vector-database-pricing-models/](https://www.actian.com/blog/databases/the-hidden-cost-of-vector-database-pricing-models/)  
21. LLM Gateway Architecture: 2026 Engineering Reference - Digital Applied, accessed June 15, 2026, [https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference](https://www.digitalapplied.com/blog/llm-gateway-architecture-2026-engineering-reference)  
22. Preparing for EU AI Act Compliance with ISO 42001 - A-LIGN, accessed June 15, 2026, [https://www.a-lign.com/articles/preparing-for-eu-ai-act-compliance](https://www.a-lign.com/articles/preparing-for-eu-ai-act-compliance)  
23. The Full Picture of 2026 Enterprise AI Architecture: A New World of Autonomous Infrastructure Opened by Hybrid LLMs, AI Gateways, and AgentOS - note, accessed June 15, 2026, [https://note.com/betaitohuman/n/n1c9b6d466f6b?hl=en](https://note.com/betaitohuman/n/n1c9b6d466f6b?hl=en)  
24. SOFTWARE BILL OF MATERIAL FOR AI - BSI, accessed June 15, 2026, [https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/SBOM-for-AI_minimum-elements.pdf?__blob=publicationFile&v=4](https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/SBOM-for-AI_minimum-elements.pdf?__blob=publicationFile&v=4)  
25. What Is an AI-BOM (AI Bill of Materials)? & How to Build It - Palo Alto ..., accessed June 15, 2026, [https://www.paloaltonetworks.com/cyberpedia/what-is-an-ai-bom](https://www.paloaltonetworks.com/cyberpedia/what-is-an-ai-bom)  
26. What Is an LLM Gateway? The Missing Layer Between Your App and AI Models, accessed June 15, 2026, [https://openrouter.ai/blog/insights/llm-gateway/](https://openrouter.ai/blog/insights/llm-gateway/)  
27. ISO 42001 Documentation: What's Required for Compliance? - Hicomply, accessed June 15, 2026, [https://www.hicomply.com/en-us/hub/documentation-requirements](https://www.hicomply.com/en-us/hub/documentation-requirements)  
28. ISO 42001 Checklist (2026): 38 Controls for AI Management | Knowlee, accessed June 15, 2026, [https://www.knowlee.ai/blog/iso-42001-checklist-ai-management](https://www.knowlee.ai/blog/iso-42001-checklist-ai-management)  
29. What Is Gemma 4's Apache 2.0 License? Why It Matters More Than the Model Itself, accessed June 15, 2026, [https://www.mindstudio.ai/blog/gemma-4-apache-2-license-commercial-use](https://www.mindstudio.ai/blog/gemma-4-apache-2-license-commercial-use)  
30. Vector Database: Everything You Need to Know - WEKA, accessed June 15, 2026, [https://www.weka.io/learn/guide/ai-ml/vector-database/](https://www.weka.io/learn/guide/ai-ml/vector-database/)  
31. The State of “Open” Source AI – Gabriel Toscano - Sites@Duke Express, accessed June 15, 2026, [https://sites.duke.edu/gabrieltoscano/2026/02/05/the-state-of-open-source-ai/](https://sites.duke.edu/gabrieltoscano/2026/02/05/the-state-of-open-source-ai/)  
32. FinOps for AI: Tools & Services Considerations, accessed June 15, 2026, [https://www.finops.org/wg/finops-for-ai-tools-services-considerations/](https://www.finops.org/wg/finops-for-ai-tools-services-considerations/)  
33. What Is the Gemma 4 Apache 2.0 License? Why It Changes ..., accessed June 15, 2026, [https://www.mindstudio.ai/blog/what-is-gemma-4-apache-2-license-commercial-ai-deployment](https://www.mindstudio.ai/blog/what-is-gemma-4-apache-2-license-commercial-ai-deployment)  
34. GSA AI Procurement Rules Would Introduce New Disclosure and Use-Rights Requirements for Federal Contractors - Gibson Dunn, accessed June 15, 2026, [https://www.gibsondunn.com/gsa-ai-procurement-rules-would-introduce-new-disclosure-and-use-rights-requirements-for-federal-contractors/](https://www.gibsondunn.com/gsa-ai-procurement-rules-would-introduce-new-disclosure-and-use-rights-requirements-for-federal-contractors/)  
35. "Additional Commercial Terms" must be removed from LICENSE to make Llama 3 open source #156 - GitHub, accessed June 15, 2026, [https://github.com/meta-llama/llama3/issues/156](https://github.com/meta-llama/llama3/issues/156)  
36. meta-llama/Llama-3.3-70B-Instruct - Hugging Face, accessed June 15, 2026, [https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct)  
37. Significant Risks in Using AI Models Governed by the Llama License - Open Source Guy, accessed June 15, 2026, [https://shujisado.org/2025/01/27/significant-risks-in-using-ai-models-governed-by-the-llama-license/](https://shujisado.org/2025/01/27/significant-risks-in-using-ai-models-governed-by-the-llama-license/)  
38. Eight essential clauses for AI contracts: A guide for vendors and customers in Northern Ireland - A&L Goodbody, accessed June 15, 2026, [https://www.algoodbody.com/insights-publications/eight-essential-clauses-for-ai-contracts-a-guide-for-vendors-and-customers-in-northern-ireland](https://www.algoodbody.com/insights-publications/eight-essential-clauses-for-ai-contracts-a-guide-for-vendors-and-customers-in-northern-ireland)  
39. Vector Database Challenges: What Breaks in Production - Redis, accessed June 15, 2026, [https://redis.io/blog/common-challenges-working-with-vector-databases/](https://redis.io/blog/common-challenges-working-with-vector-databases/)

---