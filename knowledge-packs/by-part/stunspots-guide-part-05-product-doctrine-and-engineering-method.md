# [KNOWLEDGE] - AI Engineering - Part V - Vol. 11-12 AF-AK - Product Doctrine and Engineering Method

[Volume 11. AF-AH Product, Business, and Organizational Architecture](#volume-11-af-ah-product-business-and-organizational-architecture)
  - [AI-ENG-AF — AI Product Architecture - Use-Case Selection, Workflow Fit & Value Design](#ai-eng-af--ai-product-architecture---use-case-selection-workflow-fit--value-design)
  - [AI-ENG-AG — Adoption Systems - Training, Feedback Loops & Change Management](#ai-eng-ag--adoption-systems---training-feedback-loops--change-management)
  - [AI-ENG-AH — Build, Buy, Open Source & Vendor Strategy - Doctrinal Knowledge Base for Strategic Dependency Architecture](#ai-eng-ah--build-buy-open-source--vendor-strategy---doctrinal-knowledge-base-for-strategic-dependency-architecture)


[Volume 12. AI-AK - Engineering Method and System Doctrine](#volume-12-ai-ak---engineering-method-and-system-doctrine)
  - [AI-ENG-AI — Contract Thinking - Deterministic Edges Around Probabilistic Cores](#ai-eng-ai--contract-thinking---deterministic-edges-around-probabilistic-cores)
  - [AI-ENG-AJ — AI System Design Patterns - Reference Architectures & Failure-Aware Blueprints](#ai-eng-aj--ai-system-design-patterns---reference-architectures--failure-aware-blueprints)
  - [AI-ENG-AK — The AI Engineering Mindset - Probabilistic Systems, Human Judgment & Iterative Control](#ai-eng-ak--the-ai-engineering-mindset---probabilistic-systems-human-judgment--iterative-control)


---

# Volume 11. AF-AH Product, Business, and Organizational Architecture

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

# [Volume 12. AI-AK - Engineering Method and System Doctrine]

---

# AI-ENG-AI — Contract Thinking - Deterministic Edges Around Probabilistic Cores

## **Doctrinal Introduction & The Contract Thinking Doctrine**

In high-dimensional artificial intelligence engineering, the primary challenge is not the elimination of model non-determinism, but rather the containment of that non-determinism within safe, verifiable boundaries. This paradigm, formulated as probabilistic containment architecture, recognizes that the cognitive flexibility of large language models is highly valuable during internal inference, but presents severe risks when allowed to interface directly with stateful, authoritative, or public-facing systems. Good systems design does not attempt to force the entire model into absolute predictability; instead, it establishes a rigid, deterministic envelope around the model's probabilistic core. The model may execute complex synthesis, logical deduction, and creative planning within this bounded space, but every boundary crossing—or seam—must conform to a strict, typed, and enforceable contract.  
This methodology generalizes Bertrand Meyer’s Design by Contract (DbC) framework, originally formulated in 1986 to improve traditional software reliability.1 Meyer identified correctness—the ability of software to perform its exact tasks as defined by its specification—as the supreme quality of software, supplemented by robustness, efficiency, extendibility, reusability, and compatibility.3 While defensive programming suggests adding redundant checks everywhere, it leads to code complexity—the primary obstacle to software quality.2 DbC advocates an "offensive programming" posture: establish clear preconditions, postconditions, and invariants, and "fail hard" immediately when assertions are violated, simplifying debugging and ensuring correctness.1  
Applying the Liskov Substitution Principle to AI contract boundaries establishes clear constraints on function execution 4:

* **Preconditions cannot be strengthened:** An implementation must not accept a narrower range of input than the interface specification dictates.4  
* **Postconditions cannot be weakened:** An implementation must not return a wider, more ambiguous range of output than the specification guarantees.4  
* **Invariants cannot be weakened:** An implementation must maintain core system state within specified tolerances throughout execution.4

In AI engineering, the model is inherently probabilistic. We cannot make the model itself fully deterministic. The useful question is: *Can we make every boundary around the model explicit, enforceable, observable, testable, and safe to breach-handle?* Inside the box, the model operates with probabilistic freedom; at the edges, it must sign deterministic paperwork.  
To establish this discipline, systems architects must maintain clean distinctions across multiple operational dimensions:

* **Flexibility versus Authority:** The generative core is permitted to reason, draft, and propose, but it holds zero administrative authority. Any execution of state changes, data mutation, or outbound communication must be mediated by a deterministic runtime that validates permissions and rules before execution.5  
* **Schema Validity versus Truth:** A model output conforming perfectly to a JSON schema is structurally valid, but syntactic correctness does not imply factual accuracy. Structuring constraints ensure format compliance; separate semantic and factual verifications are required to confirm truth.7  
* **Prompt Instruction versus Enforceable Boundary:** Instructions embedded within prompts (e.g., "never reveal system keys") are instructions, not security controls.6 Actual boundaries must be enforced by isolated software filters, input-scrubbing routines, and policy-as-code sidecars that are completely outside the model's context window.  
* **Model Capability versus System Permission:** A model’s computational ability to generate a tool invocation, generate SQL, or draft a financial transaction must never be confused with systemic authorization to perform that action.5  
* **Generated Action versus Authorized Action:** Actions drafted by the model are treated as unauthenticated proposals. They are only translated into physical side effects after passing through a deterministic authorization engine (such as Open Policy Agent or Cedar).6  
* **Grounding versus Citation-Shaped Decoration:** The presence of inline footnotes and citation links in a generated output often serves as a persuasive UX element rather than factual proof.8 Rigid grounding contracts must programmatically verify that claims are supported by retrieved sources, with proper temporal and logical qualifiers.7  
* **Memory versus Surveillance:** Persistent agentic memory must not become an unconstrained data sink. It requires strict tenant isolation, local pseudonymization, automatic retention rules, and user-controlled deletion paths to prevent liability and privacy leakage.9  
* **Eval Score versus Release Contract:** An eval is a formal release gate, not a subjective vibe check.14  
* **Logs versus Compliance Evidence:** Raw diagnostic logs track errors; immutable, cryptographically signed events serve as compliance evidence.11  
* **User Trust versus User Expectation Contract:** UX must cultivate calibrated trust, ensuring users rely on the system when correct and override it when it fails.17  
* **Vendor Route versus Model-Route Contract:** Gateway abstraction maintains system stability across changing provider deployments.20  
* **Fallback versus Contract Downgrade:** Fault tolerance must preserve safety properties even when dropping back to less capable local models.20

## **Conceptual Glossary**

The following standardized terms form the core vocabulary of the contract thinking doctrine and are defined for uniform engineering application:

| Term | Doctrinal Definition |
| :---- | :---- |
| **Contract Thinking** | The engineering practice of placing deterministic, typed, versioned, enforceable, observable, and testable interfaces around probabilistic model behavior at every architectural seam. |
| **Probabilistic Core** | The internal non-deterministic execution domain of a generative model where token generation, reasoning, and conceptual synthesis occur. |
| **Deterministic Edge** | The rigid, software-enforced boundary surrounding a probabilistic core that validates inputs, filters outputs, enforces budgets, checks permissions, and handles failures. |
| **Contract Surface** | Any distinct architectural seam where model outputs or inputs interface with structured systems, human users, external APIs, or storage. |
| **Contract Stack** | The layered hierarchy of active agreements (from user expectations to deployment manifests) that must remain aligned to prevent silent system failures. |
| **Schema Contract** | A programmatically enforced definition of output structure, typing, and constraints, typically implemented via constrained decoding at the token level.22 |
| **Prompt Contract** | A versioned, testable deployment configuration that formalizes the task boundary, instruction hierarchy, and input/output expectations of a model step. |
| **Retrieval Contract** | An agreement governing context injection, specifying source authority, permission filtering, freshness tolerances, and citation requirements.14 |
| **Tool Contract** | A highly specific API interface that maps natural language intents to deterministic code execution, enforcing schema validation, auth, and idempotency.5 |
| **Permission Contract** | A zero-trust authorization boundary that separates model-generated proposals from system-level action execution based on the underlying user's identity.6 |
| **Resource Contract** | A set of hard runtime constraints governing execution envelopes, including token budgets, call limits, latency ceilings, and cost budgets.16 |
| **Memory Contract** | Rules governing long-term writable state, defining write validation, retention periods, read scoping, and on-device privacy mapping.12 |
| **Model Route Contract** | The operational envelope defining which task classes and risk tiers are mapped to specific models, providers, and fallbacks.21 |
| **Eval Contract** | The quantifiable release gate specifying the performance benchmarks, regression thresholds, and behavioral tests a configuration must pass to ship. |
| **Deployment Contract** | A cryptographically signed or version-pinned bundle containing prompts, schemas, routing policies, and verification artifacts. |
| **Observability Contract** | The schema defining tracing spans, telemetry events, redacted logs, and compliance evidence emitted during system execution.16 |
| **User Expectation Contract** | The user interface patterns and disclosure frameworks that align human trust with actual system competence, preventing automation bias.17 |
| **Breach Behavior** | The deterministic error-handling pipeline executed immediately when any contract boundary is violated at runtime. |
| **Contract Drift** | A progressive misalignment between layered contracts (e.g., a product promise exceeding model capability) that leads to silent, systemic failure. |

## **The Contract Stack Model & Drift Diagnostic**

AI systems fail when contract layers silently diverge. A product promise may imply current policy compliance, while retrieval pulls stale documents. A schema may require citations, while the citation verifier only checks that a field exists. A UI may show an action as complete, while the downstream transaction failed. Contract thinking prevents these failures by aligning every layer from user expectation to deployment evidence.

```text
CONTRACT STACK

[ User Expectation Contract ]
        |
[ Product / Workflow Contract ]
        |
[ Policy / Permission Contract ]
        |
[ Prompt / Context Contract ]
        |
[ Retrieval / Memory Contract ]
        |
[ Model Route Contract ]
        |
[ Schema / Output Contract ]
        |
[ Tool / Action Contract ]
        |
[ Eval / Verification Contract ]
        |
[ Deployment / Observability Contract ]
```

Each layer must be supported by the layers beneath it. The user interface must not promise a capability the product workflow cannot verify. The prompt must not request evidence the retrieval system cannot supply. The model route must not be assigned to a task class it has not passed. The tool contract must not execute an action the permission contract has not authorized.

### **Contract Drift Diagnostic**

| Drift Pattern | Detection Signal | Safe Runtime Response | Structural Owner | Preventive Practice |
| :---- | :---- | :---- | :---- | :---- |
| **User Promise vs. Product Capability** | Users complain that the system did not do what the interface implied. | Downgrade claim, add disclosure, route to human or safer workflow. | Product Owner | Product promises must map to tested capabilities and known limits. |
| **Product Workflow vs. Model Route** | Complex/high-risk tasks routed to low-capability or low-cost model. | Re-route to approved model class or block with explanation. | Product Architect / Platform Owner | Route manifests tied to task class, risk tier, and eval evidence. |
| **Prompt vs. Retrieval** | Prompt asks for current/grounded answer but retrieval lacks current or authoritative sources. | Refuse, ask for source, or disclose unsupported status. | Prompt / Retrieval Owner | Prompt context requirements must be checked against corpus metadata. |
| **Retrieval vs. Freshness** | Retrieved source is stale, superseded, or outside time scope. | Remove stale source, rerun retrieval, disclose conflict or absence. | Corpus / Retrieval Owner | Freshness metadata, source-of-record hierarchy, index lifecycle controls. |
| **Schema vs. Truth** | Output passes schema validation but fails citation, math, or policy checks. | Block downstream use; send to factual/semantic repair or human review. | Eval / Verification Owner | Multi-layer validation beyond JSON shape. |
| **Schema vs. Tool Contract** | Model emits structurally valid arguments that violate tool preconditions. | Deny tool call; return structured breach reason. | Tool Owner | Shared typed schemas, preconditions, and postcondition tests. |
| **Permission vs. Tool Execution** | Tool call authorized by prompt language but not by user identity, role, tenant, or resource policy. | Block before side effect; log policy decision. | Security / Governance Owner | External policy engine, ABAC/RBAC, user-bound delegation. |
| **Fallback vs. Risk Tier** | Failover route has lower capability, weaker policy, or different data handling. | Degrade authority, disable actions, or route to human. | Platform SRE | Fallbacks must preserve safety properties, not just availability. |
| **Memory vs. Authority** | Memory from prior sessions influences a task outside active scope. | Restrict memory read, clear active memory, or require user confirmation. | Memory / Privacy Owner | Principal-scoped memory, provenance tags, retention and deletion rules. |
| **Eval vs. Production Reality** | Offline evals pass but production corrections, incidents, or complaints rise. | Freeze rollout, sample production failures, update eval set. | Evaluation Owner | Production-like eval data and continuous drift monitoring. |
| **Cost vs. Loop Design** | Spend spike from retries, tool loops, long context, or agent recursion. | Terminate loop, return partial result, alert owner. | Platform / FinOps Owner | Hard budgets, retry caps, loop limits, task-level cost telemetry. |
| **Observability vs. Evidence** | Logs exist but cannot prove what happened for audit or incident review. | Preserve scoped evidence package; open evidence-quality issue. | Observability / Compliance Owner | Separate telemetry schema from audit-evidence schema. |

### **Drift Response Rule**

When drift is detected, the system should prefer:

```text
block unsafe action
downgrade authority
disclose uncertainty
reroute only to approved routes
preserve scoped evidence
open review when drift repeats
```

It should not silently “repair” contract drift in ways that hide misalignment. A patched-looking response can be worse than a clean refusal.

## **Contract Surface Inventory**

A contract surface is any seam where probabilistic behavior touches structured systems, users, tools, memory, retrieval, policy, deployment, or evidence. Every surface needs an owner, enforcer, validation method, breach behavior, and evidence boundary.

| Contract Surface | Purpose | Allowed Inputs | Expected Outputs | Owner | Enforcer | Validation | Default Breach Behavior | Evidence Boundary |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Prompt Contract** | Defines task, instruction hierarchy, allowed behavior, refusal behavior. | Typed variables, task state, approved context blocks. | Model request package and expected response mode. | Prompt / Product Owner | Prompt compiler / Gateway | Prompt unit tests, eval linkage, version checks. | Use last stable prompt or block route. | Prompt version, hash, route ID, input references. |
| **Context Contract** | Controls what state enters the model. | User input, session state, retrieved context, system variables. | Permission-filtered context bundle. | Context Owner | Gateway / Context Builder | ACL/RLS, redaction, source allowlist, token budget. | Remove unauthorized context or abort. | Context manifest, source IDs, redaction record. |
| **Schema Contract** | Makes output machine-readable. | Model output stream or structured decoding result. | Typed object or validation failure. | API / Schema Owner | Parser / Validator | JSON/schema validation, enum checks, required fields. | Retry once if safe; otherwise fail closed. | Schema version, validation result, error class. |
| **Semantic Contract** | Checks whether field values make business sense. | Parsed object. | Semantically valid object or violation. | Domain Owner | Business-rule validator | Ranges, invariants, cross-field checks. | Reject or route to human. | Rule version, failed invariant, secure payload reference. |
| **Retrieval Contract** | Defines evidence entering generation. | Query, user permissions, task scope. | Ranked, permissioned, fresh source set. | Retrieval / Corpus Owner | Retriever / Permission Filter | Source authority, freshness, dedupe, relevance, conflict checks. | Refuse grounded answer or disclose unsupported state. | Source IDs, timestamps, retrieval manifest. |
| **Grounding Contract** | Verifies claims against evidence. | Generated claims, cited sources. | Supported, unsupported, or conflicting claim labels. | Evaluation / RAG Owner | Citation / Claim Verifier | Claim-to-evidence, qualifier, time-scope, contradiction checks. | Block or mark unsupported claims. | Claim IDs, source refs, verifier result. |
| **Memory Contract** | Governs persistent writable state. | User-approved memory candidates, feedback, preferences. | Scoped memory write/read or rejection. | Memory / Privacy Owner | Memory Controller | Consent, provenance, scope, retention, conflict, delete rules. | Reject memory write or restrict read. | Memory event hash, provenance, retention class. |
| **Tool Contract** | Converts proposed actions into deterministic calls. | Typed tool arguments and actor context. | Execution result, denial, or error. | Tool Owner | Execution Harness | Preconditions, schema, idempotency, postconditions. | Deny before side effect or compensate if needed. | Action ID, idempotency key, result status. |
| **Permission Contract** | Determines whether a subject may perform an action. | Subject, tenant, resource, action, arguments, environment. | Allow, deny, require approval, or escalate. | Security / Governance Owner | Policy Engine | RBAC/ABAC, tenant, risk, approval, separation-of-duty rules. | Deny by default. | Policy version, decision, reason codes. |
| **Resource Contract** | Limits spend, latency, concurrency, retries, loops, and context. | Request, route, session, tenant, budget state. | Continue, throttle, degrade, or terminate. | Platform / FinOps Owner | Gateway / Runtime Monitor | Token, time, cost, queue, retry, loop limits. | Stop loop, return partial, or route to human. | Budget event, usage counters, route ID. |
| **Model Route Contract** | Maps task class and risk tier to model/provider/fallback. | Task class, data class, risk tier, latency/cost constraints. | Approved route or refusal. | Platform Owner | Model Gateway | Route manifest, eval pass, policy compatibility. | Use approved fallback or fail closed. | Route manifest, provider snapshot, eval status. |
| **Eval Contract** | Defines release and regression gate. | Candidate prompt/schema/route/tool package. | Pass, fail, conditional pass, or block. | Eval Owner | CI/CD / Eval Harness | Task metrics, safety tests, regression thresholds. | Block deployment or rollback. | Eval report, dataset version, manifest hash. |
| **Deployment Contract** | Packages active production configuration. | Prompts, schemas, policies, model routes, tools, eval artifacts. | Signed deployment manifest. | SRE / Release Owner | CI/CD Pipeline | Hashes, signatures, approval checks, canary health. | Halt rollout or rollback. | Manifest, approval, rollout status. |
| **Observability Contract** | Defines runtime telemetry. | Spans, metrics, errors, redacted event fields. | Structured telemetry stream. | Observability Owner | Telemetry Pipeline | Schema validation, redaction, exporter health. | Spool locally or alert. | Trace IDs, metrics, redacted spans. |
| **Audit Evidence Contract** | Preserves proof for compliance or incident review. | Scoped evidence records. | Tamper-evident evidence package. | Compliance / Security Owner | Evidence Store | Hashing, signatures, retention, access controls. | Preserve minimal required evidence; restrict access. | Evidence package ID, hashes, secure references. |
| **User Expectation Contract** | Aligns UI claims with system capability. | Rendered output, evidence, controls, disclosures. | Calibrated user action or review. | Product / UX Owner | UI Runtime | Disclosure checks, evidence display, action-state integrity. | Show uncertainty, block action, or require confirmation. | UI state, action confirmation, user decision event. |
| **Vendor / Sourcing Contract** | Governs external dependency. | Outbound request, data class, provider route. | Service result under contract terms. | Procurement / Legal Owner | Gateway / Vendor Manager | DPA, retention, SLA, no-training, subprocessor controls. | Failover, block route, or open vendor incident. | Vendor route event, contract reference, SLA record. |

The inventory should be maintained as a registry, not as prose buried in a design doc. If a surface has no owner, it has no contract.

## **Prompt Contract Specification**

A prompt is not decorative prose or a venue for model coaxing; it is production code and must be engineered as versioned, testable, and deterministic configuration. Treating the model as the reliability layer is an architectural mistake.5 Instead, prompts must be written inside structured files (such as YAML, JSON, or TOML) that clearly outline the precise runtime conditions under which the model will execute.  
The system context must also conform to a context contract. This contract specifies exactly what state enters the model, from where, and under what systemic authority.6 To prevent context bleed—which degrades model reasoning capacity by up to 90.2% when irrelevant data is carried across distinct tasks—sub-agents must be isolated by job function.5 Context isolation must be structurally enforced, ensuring that user data does not leak across session boundaries and system instructions remain shielded from malicious user manipulation.  
The following Prompt Contract Template defines the schema and execution rules for a production-grade prompt configuration:

```YAML  
meta:  
  name: "InvoiceExtractionContract"  
  owner: "FinanceEngineeringGroup"  
  version: "2.4.1"  
  last_modified: "2026-03-15T09:30:00Z"  
  linked_eval_suite: "eval_invoice_extraction_v2"  
  rollback_target: "2.4.0"  
  change_log: "Added tax identifier extraction field and updated failure escalation rules."

task_boundary:  
  description: "Extract line-item billing information from raw unstructured text files."  
  allowed_languages: ["en", "de", "fr"]  
  max_input_length_characters: 50000  
  instruction_hierarchy:  
    system_directives: 1 # Highest priority, protected from user override  
    formatting_rules: 2  
    context_data: 3  
    user_inputs: 4 # Evaluated as untrusted payload only

input_assumptions:  
  expected_variables:  
    - name: "raw_invoice_text"  
      type: "string"  
      required: true  
    - name: "client_id"  
      type: "uuid"  
      required: true  
  context_sources:  
    - name: "organization_billing_rules"  
      origin: "postgres_db"  
      freshness_tolerance_seconds: 86400

behavioral_constraints:  
  allowed_behavior:  
    - "Extract merchant name, total, subtotal, tax amounts, line-items, and invoice date."  
    - "Perform currency code standardization using ISO-4217."  
  forbidden_behavior:  
    - "Do not calculate or infer missing values; report them as null."  
    - "Do not append conversational text, explanations, or markdown commentary outside the structured schema."  
  uncertainty_behavior:  
    low_confidence_threshold: 0.85  
    action: "Set extraction_confidence metric and populate low_confidence_fields array; do not omit the field."  
  refusal_and_escalation:  
    condition: "Text does not resemble an invoice or is written in an unsupported language."  
    response_mode: "explicit_refusal"  
    refusal_payload:  
      error_code: "UNSUPPORTED_DOCUMENT_TYPE"  
      message: "The provided document could not be identified as a valid invoice."  
      escalate_to: "manual_review_queue"

output_requirements:  
  format: "json_schema"  
  schema_reference: "invoice_extraction_schema_v3.json"  
  evidence_and_citation:  
    require_grounding: true  
    citation_scope: "line_level"  
    minimum_grounding_score: 0.90
```

## **Schema Contract Model & Multi-Layer Validation**

Schemas turn model output into typed artifacts that software can inspect. A schema contract proves structure, not truth. A perfectly valid JSON object can still contain false claims, unsupported citations, unsafe recommendations, or unauthorized actions.

Structured-output mechanisms and constrained decoding can improve schema adherence, but production systems must still validate outputs at multiple layers.

```text
SCHEMA VALIDATION STACK

[ Probabilistic Core ]
        |
        v
[ Structured Output / Constrained Decoding Where Supported ]
        |
        v
[ Serialization Validation ]
  valid JSON / XML / CSV / function call envelope
        |
        v
[ Schema Validation ]
  required fields | types | enums | object shape
        |
        v
[ Semantic Validation ]
  domain rules | ranges | invariants | cross-field logic
        |
        v
[ Factual Validation ]
  evidence | math | source-of-record | citations
        |
        v
[ Policy Validation ]
  safety | privacy | tenant | compliance | risk tier
        |
        v
[ Action Validation ]
  preconditions | authorization | idempotency | postconditions
        |
        v
[ Accepted Artifact or Breach Behavior ]
```

### **Validation Layers**

| Layer | What It Proves | What It Does Not Prove | Enforcement Point | Breach Behavior |
| :---- | :---- | :---- | :---- | :---- |
| **Serialization Validation** | Output can be parsed. | Correct schema, truth, safety. | Parser. | Reject or request one safe repair. |
| **Schema Validation** | Output matches required shape and types. | Values are meaningful or factual. | JSON Schema, Pydantic, Zod, Protobuf, typed SDK. | Reject, retry once, or route to human depending on risk. |
| **Semantic Validation** | Values obey business rules and invariants. | Claims are sourced or policy-safe. | Business-rule validators. | Reject invalid fields or block artifact. |
| **Factual Validation** | Claims match evidence, math, or source-of-record data. | User should act or system has permission. | Grounding verifier, calculation engine, database check. | Mark unsupported, remove, refuse, or escalate. |
| **Policy Validation** | Output is allowed under safety, privacy, tenant, and compliance policy. | Output is useful or complete. | Policy engine / gateway. | Block or redact. |
| **Action Validation** | Proposed side effect satisfies preconditions, authorization, and postconditions. | User expectation is calibrated. | Tool execution harness. | Deny, compensate, rollback, or escalate. |

### **Schema Contract Requirements**

| Requirement | Purpose |
| :---- | :---- |
| **Schema version** | Allows output changes to be tracked and tested. |
| **Required fields** | Prevents missing data from being silently ignored. |
| **Closed object policy where appropriate** | Prevents extra unreviewed fields from entering downstream code. |
| **Explicit nullable fields** | Distinguishes missing, unknown, and not applicable. |
| **Enum constraints** | Prevents invented status labels. |
| **Numeric and string constraints** | Enforces ranges and length where supported; always revalidate client-side. |
| **Cross-field validators** | Catches contradictions such as subtotal greater than total. |
| **Evidence fields** | Links claims to source IDs or verification references. |
| **Breach behavior** | Defines reject, repair, clarify, or escalate. |

### **Important Rule**

Do not treat schema adherence as success.

```text
valid JSON ≠ correct answer
valid schema ≠ grounded answer
grounded answer ≠ authorized action
authorized action ≠ completed action
completed action ≠ user understood the result
```

Each equality must be earned by a separate contract.

## **Retrieval Contract Model & RAG Grounding Verification**

Retrieval determines what reality enters the model. The retrieval contract defines which sources may be used, who may access them, how fresh they must be, how conflicts are handled, and what level of evidence is required before the model may make a claim.

The core doctrine is:

```text
No evidence, no grounded claim.
Weak evidence, weak claim.
Conflicting evidence, disclosed conflict.
Stale evidence, time-bounded claim.
Unauthorized evidence, no context injection.
```

### **Retrieval Contract Specification**

| Contract Element | Required Definition |
| :---- | :---- |
| **Source Authority** | Which sources are authoritative for the task, and in what order. |
| **Permission Filtering** | Which user, tenant, role, or workflow may access each source. |
| **Freshness Requirement** | How current evidence must be for the task. |
| **Time Scope** | Whether the answer is valid as of a date, version, or policy period. |
| **Deduplication Rule** | How duplicate or near-duplicate sources are removed or clustered. |
| **Conflict Policy** | Whether conflicts are disclosed, escalated, or resolved by source hierarchy. |
| **Citation Granularity** | Document, page, paragraph, line, field, or record-level evidence. |
| **Null-Evidence Behavior** | Refuse, ask clarification, or answer explicitly as ungrounded. |
| **Source Exclusion Disclosure** | Whether omitted sources must be disclosed to users. |
| **Grounding Verification** | How claims are checked against cited evidence. |
| **Retrieval Telemetry** | Source IDs, rank, freshness, permission result, and verifier status. |

### **Grounding Verification Checks**

| Check | Purpose |
| :---- | :---- |
| **Claim-to-Evidence Alignment** | Each atomic claim must be supported by cited evidence. |
| **Citation Specificity** | Citation points to the exact passage, record, page, or field used. |
| **Qualifier Preservation** | “Except,” “unless,” “only,” “as of,” and thresholds are preserved. |
| **Temporal Validity** | Source date/version matches the user’s time scope. |
| **Source Authority** | Claim uses the correct source-of-record, not merely a keyword match. |
| **Conflict Disclosure** | Conflicting sources are surfaced rather than flattened. |
| **Cross-Chunk Synthesis** | Logical links between chunks are themselves supported. |
| **Null-Evidence Refusal** | Plausible but unsupported questions do not trigger hallucinated answers. |
| **Paraphrase Fidelity** | Summary preserves obligations, exclusions, numbers, and conditions. |
| **Answer-Context Sensitivity** | Removing or changing evidence changes the answer appropriately. |
| **Human Replayability** | A reviewer can verify the claim from the cited evidence without archaeology. |

### **Retrieval Breach Behaviors**

| Breach | Runtime Behavior |
| :---- | :---- |
| Source unauthorized | Remove source and rerun retrieval; log permission denial. |
| Source stale | Exclude or mark as stale; ask whether historical answer is acceptable. |
| Evidence missing | Refuse grounded answer or ask for source. |
| Evidence weak | Use uncertainty language or require human review. |
| Evidence conflicting | Disclose conflict and source hierarchy. |
| Citation unsupported | Remove claim or block response. |
| Corpus poisoning suspected | Quarantine source and open corpus review. |

A retrieval pipeline that cannot refuse unsupported claims is not grounded. It is just hallucination with footnotes.

## **Tool and Action Contract Model**

Language becomes operationally dangerous at the tool seam. A model may propose an action, but only deterministic code may authorize, execute, verify, and record that action.

A tool contract defines the complete boundary between a model proposal and a real side effect.

### **Tool Contract Structure**

| Contract Field | Meaning |
| :---- | :---- |
| **tool_id** | Stable identifier for the tool. |
| **owner** | Team responsible for behavior, safety, and maintenance. |
| **allowed_callers** | Which routes, agents, users, or services may request this tool. |
| **input_schema** | Typed arguments accepted by the tool. |
| **preconditions** | Required state before execution. |
| **authorization_policy** | Permission rules evaluated outside the model. |
| **idempotency_policy** | How retries avoid duplicate side effects. |
| **execution_timeout** | Maximum runtime. |
| **postconditions** | Required state after execution. |
| **compensation_policy** | How to reverse, repair, or reconcile failure. |
| **evidence_policy** | What evidence is retained and for how long. |
| **breach_behavior** | Deny, retry, compensate, escalate, or open incident. |

### **Idempotency Key Doctrine**

An idempotency key must identify the same logical operation across retries. It should not depend on attempt-specific IDs that change when the framework retries, unless those IDs are guaranteed stable for the logical operation.

```text
idempotency_key =
  hash(
    actor_id
  + operation_id
  + tool_id
  + target_resource_id
  + normalized_arguments
  + authorization_context_hash
  )
```

| Key Component | Purpose |
| :---- | :---- |
| **actor_id** | Binds action to user/service principal. |
| **operation_id** | Stable ID for the intended logical operation. |
| **tool_id** | Prevents collisions across tools. |
| **target_resource_id** | Binds key to affected object. |
| **normalized_arguments** | Ensures equivalent retries deduplicate. |
| **authorization_context_hash** | Prevents reuse under changed permission conditions. |

### **Action State Machine**

```text
ACTION CONTRACT STATE MACHINE

[ Proposed ]
    |
    v
[ Parsed and Schema-Valid ]
    |
    v
[ Permission Checked ]
    |
    +-- deny --> [ Denied ]
    |
    v
[ Preconditions Verified ]
    |
    +-- fail --> [ Blocked / Needs Clarification ]
    |
    v
[ Idempotency Key Reserved ]
    |
    +-- duplicate --> [ Return Prior Result ]
    |
    v
[ Submitted ]
    |
    v
[ Confirmed by Source of Record ]
    |
    +-- fail --> [ Compensate / Escalate ]
    |
    v
[ Completed ]
```

### **Postcondition Verification**

A tool call is not complete when the API returns. It is complete when the source of record confirms the expected state.

| Action Type | Required Postcondition |
| :---- | :---- |
| **Create** | New object exists with expected fields and owner. |
| **Update** | Target object version changed and fields match requested delta. |
| **Delete** | Object removed or marked deleted according to policy. |
| **Send** | Message accepted by delivery system with recipient and content hash. |
| **Payment / Transfer** | Transaction ID confirmed and amount/recipient match approved payload. |
| **Booking / Reservation** | Reservation exists with correct time, party, price, and cancellation terms. |
| **Permission Change** | Policy state reflects intended access and no unintended grants. |

### **Tool Breach Behavior**

| Breach | Behavior |
| :---- | :---- |
| Invalid arguments | Reject before execution. |
| Unauthorized actor | Deny and log policy decision. |
| Failed precondition | Ask clarification or route to human. |
| Duplicate retry | Return cached result for same logical operation. |
| Partial side effect | Compensate or reconcile. |
| Unknown final state | Query source of record; escalate if unresolved. |
| High-impact action | Require human approval before submission. |

The model proposes. The tool contract disposes.

## **Permission and Security Contract Model**

A model’s ability to generate an action is not permission to execute it. Permission must be evaluated by deterministic policy outside the model, using the authenticated subject, tenant, resource, action, arguments, environment, risk tier, and approval state.

The default decision is deny.

### **Permission Decision Inputs**

| Input | Meaning |
| :---- | :---- |
| **subject** | Human user, service account, agent identity, or delegated actor. |
| **tenant** | Organizational boundary for data and authority. |
| **resource** | Object, account, document, record, system, or environment being affected. |
| **action** | Read, write, delete, send, approve, execute, export, etc. |
| **tool** | Tool or API being invoked. |
| **arguments** | Normalized proposed payload. |
| **risk_tier** | Consequence class of the action. |
| **environment** | Production, staging, sandbox, region, network, time, device posture. |
| **approval_state** | Whether required maker-checker, human approval, or dual control exists. |
| **policy_version** | Active policy bundle used to decide. |

### **Permission Contract Outcomes**

| Outcome | Meaning |
| :---- | :---- |
| **allow** | Action may proceed to precondition and idempotency checks. |
| **deny** | Action is blocked. |
| **require_approval** | Human or multi-party approval needed. |
| **require_clarification** | Action intent or target is ambiguous. |
| **degrade_authority** | System may draft or review but not execute. |
| **escalate** | Route to security, compliance, or workflow owner. |

### **Policy Contract Pattern**

```json
{
  "decision_request": {
    "subject": {
      "id": "user_123",
      "role": "finance_reviewer",
      "tenant": "tenant_a",
      "scopes": ["invoice:read", "invoice:review"]
    },
    "resource": {
      "type": "invoice",
      "id": "inv_789",
      "tenant": "tenant_a",
      "classification": "confidential"
    },
    "tool": {
      "id": "invoice_approval_api",
      "action": "approve_payment"
    },
    "arguments": {
      "amount": "1250.00",
      "currency": "USD",
      "payee_id": "vendor_456"
    },
    "context": {
      "risk_tier": "high",
      "environment": "production",
      "approval_state": "maker_submitted_checker_pending",
      "route_id": "finance_review_governed"
    }
  }
}
```

### **Security Contract Controls**

| Control | Purpose |
| :---- | :---- |
| **External Policy Engine** | Keeps authorization outside model context. |
| **Least Privilege** | Agents inherit only the user’s approved scopes, not platform-wide authority. |
| **Tenant Isolation** | Subject, resource, memory, retrieval, and tool scopes must align. |
| **Argument-Level Policy** | Authorization checks the proposed payload, not just the tool name. |
| **Separation of Duties** | Maker and checker roles must be distinct for high-impact actions. |
| **Approval Binding** | Approval must bind to the exact payload hash, not a vague intent. |
| **Time and Environment Conditions** | Sensitive actions may depend on maintenance windows, regions, or environment. |
| **Policy Reason Codes** | Denials should be explainable to system owners and users where safe. |
| **Policy Versioning** | Every decision references the active policy bundle. |

### **Anti-Patterns**

| Anti-Pattern | Why It Fails |
| :---- | :---- |
| Prompt says “only do authorized actions.” | Prompt text is not authorization. |
| Tool name is allowed, so all payloads are allowed. | Argument-level abuse bypasses intent checks. |
| String blacklist blocks dangerous commands. | Attackers route around brittle keyword filters. |
| Agent runs under service-admin identity. | Confused-deputy failure. |
| Approval is recorded before payload finalization. | User approved an intent, not the executed action. |
| Fallback route skips policy sidecar. | Availability destroys security. |

Security contracts must be enforced before side effects, not explained after them.

## **Resource, Memory, and Model Route Contracts**

Resource, memory, and model-route contracts define the runtime envelope around model behavior. They prevent runaway loops, privacy leakage, stale personalization, unsafe fallback, and provider-specific dependency drift.

### **Resource Contract Model**

A resource contract constrains the amount of time, money, context, concurrency, and retry effort a model workflow may consume.

| Resource Limit | Purpose | Breach Behavior |
| :---- | :---- | :---- |
| **max_input_tokens** | Prevents unbounded context growth. | Compress, retrieve less, ask user, or block. |
| **max_output_tokens** | Prevents verbose or runaway generation. | Stop generation and mark truncated. |
| **max_total_tokens** | Caps full session cost. | Return partial result or escalate. |
| **max_loop_iterations** | Prevents agent recursion. | Stop loop; return current state. |
| **max_tool_calls** | Prevents tool abuse and cost bombs. | Block further tool use. |
| **max_retries** | Prevents retry storms. | Fail with structured error. |
| **latency_ceiling_ms** | Protects user experience and workflow SLO. | Timeout, fallback, or queue. |
| **cost_budget** | Controls spend by user, tenant, route, or workflow. | Throttle, degrade, or require approval. |
| **concurrency_limit** | Prevents overload and rate-limit exhaustion. | Queue or reject. |
| **queue_deadline** | Prevents stale work. | Expire or revalidate task. |

### **Resource Contract Template**

```yaml
resource_contract:
  contract_id: "<resource_contract_id>"
  owner: "<platform_or_finops_owner>"
  applies_to_routes:
    - "<route_id>"
  limits:
    max_input_tokens: 0
    max_output_tokens: 0
    max_total_tokens: 0
    max_loop_iterations: 0
    max_tool_calls: 0
    max_retries: 0
    latency_ceiling_ms: 0
    cost_budget_usd: 0
    concurrency_limit: 0
  breach_behavior:
    token_limit: "compress_or_refuse"
    loop_limit: "return_partial_and_escalate"
    cost_limit: "require_approval"
    latency_timeout: "fallback_or_queue"
```

### **Memory Contract Model**

Memory is writable state. It must be scoped, permissioned, reviewable, and deletable. Memory should not become a surveillance landfill or a prompt-injection persistence layer.

| Memory Rule | Purpose |
| :---- | :---- |
| **Write Validation** | Prevents poisoned, false, sensitive, or unauthorized memory writes. |
| **User / Principal Scope** | Prevents memory from leaking across users, tenants, roles, or workflows. |
| **Provenance Tagging** | Records where the memory came from and when. |
| **Confidence and Status** | Distinguishes user-confirmed memory from inferred memory. |
| **Retention Class** | Defines when memory expires or must be reviewed. |
| **Read Authorization** | Checks whether memory may be used in the current task. |
| **Conflict Resolution** | Handles contradictory memories. |
| **User Control** | Provides inspect, edit, disable, and delete where appropriate. |
| **Rollback / Quarantine** | Allows poisoned or harmful memory to be removed. |

### **Memory Contract Template**

```yaml
memory_contract:
  contract_id: "<memory_contract_id>"
  owner: "<memory_owner>"
  allowed_memory_types:
    - preference
    - workflow_context
    - user_confirmed_fact
  prohibited_memory_types:
    - secrets
    - unsupported_inferences
    - sensitive_data_without_policy_basis
  write_policy:
    require_user_confirmation: true
    require_provenance: true
    injection_scan_required: true
  read_policy:
    scope: "user | tenant | workflow | role"
    require_active_task_relevance: true
    require_permission_check: true
  retention:
    default_ttl_days: 0
    review_required: true
  user_controls:
    inspect: true
    edit: true
    delete: true
    disable: true
  breach_behavior:
    suspected_poisoning: "quarantine_memory_and_open_review"
    unauthorized_read: "deny_and_log"
    conflict: "ask_user_or_disclose_conflict"
```

### **Model Route Contract Model**

A model route contract maps task classes and risk tiers to approved execution routes. Routes should be expressed as capability profiles, not as brittle vendor catalogs.

| Route Field | Meaning |
| :---- | :---- |
| **route_id** | Stable internal route name. |
| **task_classes** | Workloads approved for this route. |
| **risk_tiers** | Maximum risk tier allowed. |
| **data_classes** | Data types allowed to enter this route. |
| **capability_profile** | Required reasoning, modality, context, tool, or latency capability. |
| **provider_profile** | Managed API, hosted model, self-hosted, local, deterministic fallback. |
| **eval_gate** | Eval suite and minimum pass condition. |
| **fallback_chain** | Approved fallbacks that preserve safety contract. |
| **degraded_authority** | What actions are disabled under fallback. |
| **observability** | Required telemetry emitted by route. |
| **cost_budget** | Cost limits for route use. |

### **Model Route Contract Template**

```yaml
model_route_contract:
  route_id: "<route_id>"
  owner: "<platform_owner>"
  approved_task_classes:
    - "<task_class>"
  maximum_risk_tier: "low | medium | high | regulated"
  allowed_data_classes:
    - "public"
    - "internal"
  capability_profile:
    context_window: "small | medium | large"
    modalities: ["text"]
    tool_use: "none | read_only | write_with_approval"
    latency_class: "interactive | batch | async"
  execution_profile:
    primary_route: "<provider_or_runtime_alias>"
    fallback_routes:
      - route: "<fallback_route_id>"
        authority_downgrade: "disable_tool_execution"
        reason: "primary_unavailable"
  eval_gate:
    required_suite: "<eval_suite_id>"
    required_status: "pass"
  observability:
    emit_trace: true
    emit_cost: true
    emit_quality_sample: true
  breach_behavior:
    eval_expired: "block_route"
    provider_unavailable: "use_approved_fallback"
    fallback_not_safe: "fail_closed"
```

A fallback route must preserve the safety contract. If the fallback cannot meet the same permission, privacy, grounding, or action-verification requirements, it is not a fallback. It is a contract downgrade and must reduce authority.

## **Evaluation, Deployment, and Observability Contracts**

Evaluation, deployment, and observability contracts turn AI behavior from a demo into an operated system. The eval contract decides whether a configuration may ship. The deployment contract defines exactly what shipped. The observability contract proves what happened at runtime.

### **Evaluation Contract Model**

An evaluation contract defines the test suite, dataset, metrics, thresholds, owners, and release decision for a model route, prompt, schema, retrieval pipeline, tool, or full workflow.

| Eval Contract Element | Required Definition |
| :---- | :---- |
| **eval_suite_id** | Stable identifier for the evaluation suite. |
| **scope** | Prompt, route, retrieval, schema, tool, workflow, or full system. |
| **risk_tier** | Determines required evidence and threshold strictness. |
| **dataset_version** | Golden set, adversarial set, production sample, synthetic set. |
| **metrics** | Task-specific success, failure, safety, cost, and latency metrics. |
| **thresholds** | Pass/fail or conditional release thresholds. |
| **regression_window** | Allowed delta from prior stable manifest. |
| **human_review_required** | Whether expert review is required before release. |
| **failure_behavior** | Block release, allow canary, require mitigation, or rollback. |
| **owner** | Person/team accountable for eval validity. |

### **Risk-Tiered Evaluation Gates**

| Risk Tier | Evaluation Posture |
| :---- | :---- |
| **Low** | Lightweight task tests, schema validation, basic safety and latency checks. |
| **Medium** | Golden set, regression checks, sampled human review, cost/latency gates. |
| **High** | Strong golden set, adversarial tests, grounding/policy checks, human sign-off. |
| **Regulated / Critical** | Formal evidence package, independent review, replayability, approval record. |

Avoid universal thresholds like “1,000 cases” or “100% refusal” unless the workflow justifies them. Thresholds must match task consequence, data quality, and measurement confidence.

### **Deployment Contract Model**

A deployment contract is the signed manifest of the active AI system configuration.

```yaml
deployment_manifest:
  manifest_id: "<manifest_id>"
  release_version: "<version>"
  owner: "<release_owner>"
  created_at: "<iso_datetime>"
  applies_to:
    workflow: "<workflow_id>"
    environment: "staging | production"
  components:
    prompt_contract: "<prompt_contract_id>@<version>"
    schema_contract: "<schema_contract_id>@<version>"
    retrieval_contract: "<retrieval_contract_id>@<version>"
    memory_contract: "<memory_contract_id>@<version>"
    tool_contracts:
      - "<tool_contract_id>@<version>"
    permission_policy_bundle: "<policy_bundle_hash>"
    model_route_contract: "<route_contract_id>@<version>"
    resource_contract: "<resource_contract_id>@<version>"
  eval_gate:
    eval_suite: "<eval_suite_id>"
    status: "pass | conditional | fail"
    report_hash: "<hash>"
  approvals:
    product_owner: "<approval_ref>"
    security_owner: "<approval_ref>"
    eval_owner: "<approval_ref>"
  rollback:
    previous_manifest_id: "<manifest_id>"
    rollback_conditions:
      - "schema_error_spike"
      - "policy_denial_spike"
      - "eval_regression"
```

Any change to prompt, schema, route, retrieval, memory, tool, policy, eval threshold, or resource envelope is a deployment event.

### **Observability Contract Model**

The observability contract defines runtime telemetry. It should record enough to debug, evaluate, and govern the system without overcollecting sensitive data.

| Signal | Purpose |
| :---- | :---- |
| **trace_id / workflow_id** | Correlate steps in a request or task. |
| **manifest_id** | Identify active deployment package. |
| **route_id** | Identify model/provider/runtime path. |
| **contract versions** | Link runtime behavior to exact contract stack. |
| **validation results** | Schema, semantic, factual, policy, and action checks. |
| **resource usage** | Tokens, cost, latency, retries, tool calls, queueing. |
| **breach events** | Contract violations and breach behavior. |
| **user decisions** | Accept, reject, edit, approve, override, escalate. |
| **redaction status** | Whether sensitive fields were removed or referenced securely. |

### **Telemetry vs. Audit Evidence**

| Dimension | Telemetry | Audit Evidence |
| :---- | :---- | :---- |
| **Purpose** | Debugging, monitoring, optimization, drift detection. | Proving policy, compliance, approval, or incident facts. |
| **Content** | Redacted spans, metrics, errors, counters. | Minimal structured records, hashes, approvals, policy decisions, secure refs. |
| **Retention** | Shorter, operationally scoped. | Risk/legal/compliance scoped. |
| **Mutability** | Rotated and managed as operational data. | Tamper-evident where required. |
| **Access** | Engineering and operations access by role. | Strictly controlled access by legal/security/compliance need. |

Audit evidence should not be raw logs with a fancy hat. It should be scoped, structured, minimized, and defensible.

## **User Expectation & Trust Calibration Contract**

The user interface is a contract boundary. It tells the user what the system can do, what it cannot do, what evidence it used, what was excluded, what authority it has, and when the user must verify or decide.

Trust calibration means user reliance matches system competence. Overtrust creates rubber-stamping. Undertrust creates abandonment. The contract must make the system’s role legible.

### **Expectation Contract Elements**

| Element | User-Facing Question |
| :---- | :---- |
| **System Role** | Is the AI drafting, reviewing, recommending, deciding, or acting? |
| **Authority Boundary** | Can the AI execute, or only propose? |
| **Evidence Used** | What sources support this output? |
| **Evidence Excluded** | What sources were unavailable, unauthorized, stale, or omitted? |
| **Uncertainty State** | What is known, uncertain, unsupported, or conflicting? |
| **User Responsibility** | What must the user review or approve? |
| **Action State** | Is this a draft, pending approval, submitted, confirmed, failed, or rolled back? |
| **Correction Path** | How can the user reject, edit, appeal, override, or report an issue? |
| **Fallback State** | Is the system in degraded, reduced-fidelity, or fallback mode? |

### **Trust Calibration Controls**

| Control | Purpose |
| :---- | :---- |
| **Role Label** | Shows whether AI is assistant, reviewer, router, or executor. |
| **Evidence Panel** | Makes source support visible and replayable. |
| **Unsupported Claim Marker** | Prevents unsupported text from appearing equally authoritative. |
| **Conflict Banner** | Surfaces unresolved disagreement among sources. |
| **Omitted Source Notice** | Tells users when search or retrieval was incomplete. |
| **Action Proximity Warning** | Places risk and approval requirements near execution controls. |
| **Draft / Final State Separation** | Prevents users from mistaking generated text for committed action. |
| **Human Approval Gate** | Requires explicit decision for high-impact actions. |
| **Undo / Appeal / Correction Path** | Supports trust repair and contestability. |
| **Degraded Mode Indicator** | Shows when capability or evidence has been reduced. |

### **Confidence Display Rule**

Raw model confidence can mislead users. Prefer concrete status labels tied to verification:

| Weak Display | Better Display |
| :---- | :---- |
| “95% confident” | “Cited source supports this claim.” |
| “High confidence” | “Verified against source of record.” |
| “Probably correct” | “Needs human review: conflicting evidence.” |
| “Low confidence” | “Missing required evidence.” |
| “AI completed this” | “Submitted; awaiting source-of-record confirmation.” |

### **Expectation Breach Examples**

| Breach | Example | Required Response |
| :---- | :---- | :---- |
| **Capability Overclaim** | UI promises “policy compliant” when retrieval is incomplete. | Change language, block claim, or show unsupported status. |
| **Authority Confusion** | User thinks draft was sent. | Separate draft, approval, submitted, confirmed states. |
| **Evidence Illusion** | Citation exists but does not support claim. | Remove claim or mark unsupported. |
| **Fallback Concealment** | System silently uses weaker fallback model. | Show degraded mode and reduce authority. |
| **Automation Bias** | User can one-click approve high-impact output without review. | Add evidence gate or independent judgment step. |

The UI should not ask users to trust the model. It should give users enough context to trust, verify, correct, or refuse the system appropriately.

## **Contract Breach Playbook**

A contract breach occurs when any deterministic edge rejects, cannot verify, or cannot safely process model behavior, context, retrieval, memory, tool execution, policy, route, deployment, or user expectation.

Breach handling must be deterministic. The system should not improvise safety.

```text
CONTRACT BREACH FLOW

[ Breach Detected ]
        |
        v
[ Classify Breach ]
  schema | semantic | factual | policy | permission | tool | resource
  memory | retrieval | route | deployment | observability | user expectation
        |
        v
[ Determine Severity ]
  low | medium | high | security/compliance incident
        |
        v
[ Contain ]
  block action | stop loop | freeze state | remove source | restrict memory | fail closed
        |
        v
[ Resolve or Escalate ]
  repair | retry | clarify | reroute | degrade | human review | incident
        |
        v
[ Preserve Scoped Evidence ]
        |
        v
[ Notify / Recover / Review ]
```

### **Breach Response Matrix**

| Breach Type | Default Response | Notes |
| :---- | :---- | :---- |
| **Serialization Failure** | Reject or safe single repair attempt. | Do not regex-hack high-risk outputs into existence. |
| **Schema Failure** | Retry once if low-risk; otherwise reject or escalate. | Preserve validation error class. |
| **Semantic Failure** | Reject field/object or route to human. | Business invariants beat model fluency. |
| **Factual / Grounding Failure** | Remove unsupported claim, refuse, or request evidence. | Do not hide weak grounding behind citations. |
| **Policy Failure** | Block and log policy decision. | No model retry should bypass policy. |
| **Permission Failure** | Deny before side effect. | Return reason where safe. |
| **Tool Precondition Failure** | Ask clarification, block, or route to human. | Never execute with ambiguous target. |
| **Tool Postcondition Failure** | Query source of record; compensate or escalate. | API return is not proof of completion. |
| **Resource Breach** | Stop loop, throttle, degrade, or require approval. | Cost and loop breaches can become incidents. |
| **Memory Breach** | Quarantine memory, restrict read, or delete per policy. | Preserve provenance and review path. |
| **Retrieval Breach** | Exclude source, rerun retrieval, disclose conflict, or refuse. | Poisoned or stale sources require corpus review. |
| **Route Breach** | Use approved fallback or fail closed. | Fallback must preserve safety properties. |
| **Deployment Breach** | Halt rollout or rollback manifest. | Treat prompt/policy/schema changes as deploys. |
| **Observability Breach** | Spool locally, alert owner, or block high-risk route. | Systems without evidence may be unsafe to operate. |
| **User Expectation Breach** | Update UI state, disclose limitation, require confirmation. | Trust repair is part of breach handling. |

### **Evidence Preservation Rule**

Preserve enough evidence to investigate, prove, and repair the breach without creating an uncontrolled sensitive-data archive.

| Evidence Type | Preferred Form |
| :---- | :---- |
| Prompt / schema / policy / route versions | Hashes and manifest IDs. |
| Input/output payloads | Secure references, redacted excerpts, or hashes by default. |
| Tool/action details | Action ID, idempotency key, target resource, status, payload hash. |
| Policy decision | Policy version, decision, reason code, subject/resource references. |
| Retrieval evidence | Source IDs, timestamps, rank, verifier status. |
| Memory event | Memory ID, provenance, retention class, operation type. |
| User approval | Approval ID, approver role, payload hash, timestamp. |

Raw payload capture should be reserved for incident classes and environments where policy, legal basis, access control, and retention are explicitly defined.

### **Incident Escalation Triggers**

| Trigger | Escalation |
| :---- | :---- |
| Repeated breach of same contract | Open owner review. |
| Breach affects high-risk workflow | Escalate to human/governance owner. |
| Unauthorized data exposure | Security/privacy incident. |
| Tool action executed incorrectly | Operations incident. |
| Vendor route drift causes regression | Vendor/platform incident. |
| Breach evidence cannot be preserved | Observability/compliance incident. |
| Users were misled by UI state | Product/trust incident. |

A breach is not just an error. It is a signal that a contract boundary either worked, failed, or was missing.

## **Contract Review and Lifecycle Model**

Contracts are living system artifacts. They must be created, approved, tested, released, monitored, reviewed, migrated, deprecated, and retired. A contract that exists only in prose is not a contract; it is a wish with headers.

### **Contract Lifecycle**

| Stage | Required Activity | Output |
| :---- | :---- | :---- |
| **1. Creation** | Define purpose, owner, inputs, outputs, validation, breach behavior, evidence boundary. | Draft contract artifact. |
| **2. Review** | Product, security, governance, eval, platform, and domain review as appropriate. | Approval or required changes. |
| **3. Testing** | Run unit tests, schema tests, evals, policy tests, route tests, and breach tests. | Test report. |
| **4. Release** | Bundle into deployment manifest with hashes, versions, and rollback target. | Signed manifest. |
| **5. Monitoring** | Track validation errors, breaches, cost, latency, user overrides, drift, incidents. | Runtime telemetry and evidence. |
| **6. Drift Review** | Compare production behavior to contract assumptions. | Contract update, route change, or incident. |
| **7. Migration** | Move to new model, provider, schema, policy, corpus, tool, or UI surface. | Migration plan and eval comparison. |
| **8. Deprecation** | Mark old contract as no longer preferred; route traffic away. | Deprecation notice and timeline. |
| **9. Retirement** | Disable execution and preserve required evidence. | Archived contract and retirement record. |

### **Review Triggers**

| Trigger | Contracts Affected |
| :---- | :---- |
| **Model/provider change** | Model route, eval, prompt, schema, observability. |
| **Prompt update** | Prompt, eval, schema, user expectation. |
| **Schema/API change** | Schema, tool, deployment, eval. |
| **New tool side effect** | Tool, permission, action verification, evidence. |
| **New data class** | Context, retrieval, memory, permission, privacy. |
| **New user population** | User expectation, adoption, permission, support. |
| **New jurisdiction or regulation** | Policy, memory, evidence, vendor, retention. |
| **Vendor term or subprocessor change** | Vendor, data rights, route, procurement. |
| **Retrieval corpus/index rebuild** | Retrieval, grounding, eval, observability. |
| **Embedding model change** | Retrieval, index, eval, route, exit plan. |
| **Eval regression** | Eval, deployment, route, prompt, retrieval. |
| **Cost anomaly** | Resource, route, tool, observability. |
| **Security incident** | Permission, tool, memory, evidence, deployment. |
| **User trust/adoption failure** | User expectation, workflow, prompt, evaluation. |
| **Repeated contract breach** | Affected contract and upstream/downstream layers. |

### **Contract Registry Fields**

| Field | Purpose |
| :---- | :---- |
| contract_id | Stable identifier. |
| contract_type | Prompt, schema, tool, memory, route, etc. |
| owner | Accountable team/person. |
| status | Draft, active, deprecated, retired. |
| version | Semantic or manifest version. |
| linked_contracts | Related surfaces in the stack. |
| linked_eval_suite | Required eval gate. |
| risk_tier | Determines approval and evidence requirements. |
| deployment_manifest | Active release bundle. |
| evidence_policy | Retention and evidence requirements. |
| breach_policy | Deterministic failure behavior. |
| last_reviewed | Review timestamp. |
| next_review_trigger | Time or event-based trigger. |

Contract lifecycle management is the maintenance discipline that keeps probabilistic systems from becoming folklore.

## **Cross-Canon Handoff Map**

AI-ENG-AI defines the seam discipline that unifies the AI Engineering Systems Canon. Contract thinking turns prompts, schemas, retrieval, memory, tools, permissions, resources, model routes, evaluations, deployments, observability, vendor dependencies, and user expectations into enforceable boundaries around probabilistic cores.

| Canon Report | Handoff Into AI | Contract-Thinking Integration |
| :---- | :---- | :---- |
| **AI-ENG-B — Context Architecture** | Context windows, state, instruction hierarchy, authority. | Context Contract and Prompt Contract define what enters the model and under what priority. |
| **AI-ENG-D — Corpus Engineering** | Source ownership, lineage, metadata, lifecycle. | Retrieval Contract requires source authority, provenance, and corpus lifecycle state. |
| **AI-ENG-E — Retrieval Pipeline** | Chunking, ranking, reranking, citation, retrieval telemetry. | Retrieval and Grounding Contracts define evidence admission and claim support. |
| **AI-ENG-F — Freshness and Conflict Detection** | Source freshness, conflict packets, source-of-record logic. | Retrieval Contract requires time scope, freshness, and conflict behavior. |
| **AI-ENG-J — Throughput Mechanics** | Latency, batching, queues, prefill/decode constraints. | Resource and Model Route Contracts define runtime envelopes and latency ceilings. |
| **AI-ENG-K — Weight Dynamics** | Quantization, adapters, model behavior shifts. | Model Route and Eval Contracts bind model variants to tested task classes. |
| **AI-ENG-L — Serving Architecture** | Gateways, routing, failover, deployment patterns. | Model Route and Deployment Contracts control route manifests and fallback safety. |
| **AI-ENG-M — Agentic Orchestration** | Planner/executor loops, agent roles, multi-step tasks. | Resource, Tool, Permission, and Memory Contracts bound agent behavior. |
| **AI-ENG-N — Tool Contracts** | Tool schemas, idempotency, execution interfaces. | AI-ENG-AI generalizes tool boundaries into full contract-stack discipline. |
| **AI-ENG-O — Action Verification** | Source-of-record checks, postconditions, false-success prevention. | Tool and Action Contracts require preconditions, postconditions, and confirmation. |
| **AI-ENG-P — Multimodal Understanding** | Image/audio/video input uncertainty and evidence. | Schema, Context, and User Expectation Contracts define modality-specific confidence and review. |
| **AI-ENG-Q — Speech, Voice, and Real-Time Systems** | Latency, interruption, streaming, voice UX. | Resource and User Expectation Contracts define realtime boundaries and confirmation gates. |
| **AI-ENG-R — UI Agents** | Browser/UI actions, screen state, user authority. | Tool, Permission, and User Expectation Contracts govern UI-side effects. |
| **AI-ENG-S — Production Pathologies** | Failure modes, brittleness, hallucination, drift. | Contract Drift Diagnostic maps pathologies to breached seams. |
| **AI-ENG-T — Boundary Defense** | Tenant isolation, prompt injection, egress, policy hierarchy. | Permission, Context, Retrieval, and Tool Contracts enforce boundaries outside the model. |
| **AI-ENG-U — Supply Chain Security** | SBOM, AI-BOM, provenance, artifact trust. | Deployment and Vendor Contracts require signed artifacts and dependency evidence. |
| **AI-ENG-V — Resource Abuse** | Loop abuse, denial-of-wallet, cost bombs. | Resource Contract enforces budgets, loop caps, retry caps, and concurrency limits. |
| **AI-ENG-W — UX Resilience** | Degraded modes, fallback UX, continuity. | Breach Playbook and User Expectation Contract define safe degradation. |
| **AI-ENG-X — User Trust** | Transparency, contestability, disclosure, trust repair. | User Expectation Contract formalizes trust calibration and user authority. |
| **AI-ENG-Y — Human Review** | Maker-checker, approval queues, reviewer burden. | Permission and Tool Contracts bind approvals to payloads and action states. |
| **AI-ENG-Z — Strategic Telemetry** | Traces, metrics, behavior observability. | Observability Contract defines runtime emission and redaction. |
| **AI-ENG-AA — Evaluation Architecture** | Golden sets, rubrics, regression gates. | Eval Contract defines release gates and drift response. |
| **AI-ENG-AB — Verification Artifacts** | Evidence packages, replay, audit references. | Audit Evidence Contract defines what proof is retained. |
| **AI-ENG-AC — AI Operations** | Incidents, rollback, runbooks, containment. | Breach Playbook and Contract Lifecycle Model define operational response. |
| **AI-ENG-AD — Governance Architecture** | Policy, audit, compliance, accountability. | Permission, Evidence, Vendor, and Deployment Contracts make governance executable. |
| **AI-ENG-AE — Sustainable AI** | Cost, energy, routing, lifecycle efficiency. | Resource and Model Route Contracts enforce cost/resource envelopes. |
| **AI-ENG-AF — Product Architecture** | Use-case fit, workflow value, product surface. | User Expectation and Product/Workflow Contracts bind promises to capability. |
| **AI-ENG-AG — Adoption Systems** | Training, feedback loops, incentives, change. | User Expectation and Observability Contracts feed adoption telemetry and correction loops. |
| **AI-ENG-AH — Sourcing and Vendor Strategy** | Build/buy/open/vendor decisions, exit plans. | Vendor and Model Route Contracts prevent sourcing decisions from becoming invisible lock-in. |
| **AI-ENG-AJ — Reference Architectures** | Reusable implementation patterns. | AI-ENG-AI supplies the contract surfaces each reference architecture must instantiate. |

### **Core Canon Rule**

Every probabilistic capability must cross deterministic boundaries before it can affect users, systems, memory, tools, money, permissions, evidence, or production state.


> The model may be probabilistic.
> The **system** must not be.


#### **Works cited**

1. Design by contract - Wikipedia, accessed June 15, 2026, [https://en.wikipedia.org/wiki/Design_by_contract](https://en.wikipedia.org/wiki/Design_by_contract)  
2. Applying 'design by contract' - Michigan Technological University, accessed June 15, 2026, [https://pages.mtu.edu/~aebnenas/teaching/spring2010/cs3141/readings/meyerPDF.pdf](https://pages.mtu.edu/~aebnenas/teaching/spring2010/cs3141/readings/meyerPDF.pdf)  
3. Design By Contract: A Missing Link In The Quest For Quality Software, accessed June 15, 2026, [https://wstomv.win.tue.nl/edu/2ip30/references/design-by-contract/index.html](https://wstomv.win.tue.nl/edu/2ip30/references/design-by-contract/index.html)  
4. API Security through Contract-Driven Programming | CMU Software Engineering Institute, accessed June 15, 2026, [https://www.sei.cmu.edu/blog/api-security-through-contract-driven-programming/](https://www.sei.cmu.edu/blog/api-security-through-contract-driven-programming/)  
5. AI Agent Tool Use Best Practices for Practitioners - MLflow, accessed June 15, 2026, [https://mlflow.org/articles/ai-agent-tool-use-best-practices-for-practitioners/](https://mlflow.org/articles/ai-agent-tool-use-best-practices-for-practitioners/)  
6. Why Open Policy Agent is the Missing Guardrail for Your AI Agents, accessed June 15, 2026, [https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/](https://codilime.com/blog/why-use-open-policy-agent-for-your-ai-agents/)  
7. Groundedness detection in Azure AI Content Safety - Microsoft Learn, accessed June 15, 2026, [https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness)  
8. RAG Grounding: 11 Tests That Expose Fake Citations | by Nexumo ..., accessed June 15, 2026, [https://medium.com/@Nexumo_/rag-grounding-11-tests-that-expose-fake-citations-30d84140831a](https://medium.com/@Nexumo_/rag-grounding-11-tests-that-expose-fake-citations-30d84140831a)  
9. LLMs and Data Privacy: How to Protect Sensitive Information - Duality Technologies, accessed June 15, 2026, [https://dualitytech.com/blog/llm-data-privacy/](https://dualitytech.com/blog/llm-data-privacy/)  
10. OPA Guardrails - TrueFoundry Docs, accessed June 15, 2026, [https://www.truefoundry.com/docs/ai-gateway/opa-guardrails](https://www.truefoundry.com/docs/ai-gateway/opa-guardrails)  
11. Enforce least-privilege authorization in multi-agent AI delegation chains using Cedar on AWS - GitHub, accessed June 15, 2026, [https://github.com/aws-samples/sample-cedar-agentic-ai-authorization](https://github.com/aws-samples/sample-cedar-agentic-ai-authorization)  
12. A Survey on Long-Term Memory Security in LLM Agents: Attacks, Defenses, and Governance Across the Memory Lifecycle - arXiv, accessed June 15, 2026, [https://arxiv.org/html/2604.16548v2](https://arxiv.org/html/2604.16548v2)  
13. MemPrivacy is a privacy-preserving personalized memory management framework for edge-cloud agents. - GitHub, accessed June 15, 2026, [https://github.com/MemTensor/MemPrivacy](https://github.com/MemTensor/MemPrivacy)  
14. RAG Evals: Retrieval Relevance, Grounding, and Citation Fidelity - Vikas Goyal, accessed June 15, 2026, [https://vikasgoyal.github.io/agentic/observe/rag-evals.html](https://vikasgoyal.github.io/agentic/observe/rag-evals.html)  
15. RAG Testing — Validating Retrieval Accuracy, Grounding, and Context Leakage - Medium, accessed June 15, 2026, [https://medium.com/@gunashekarr11/rag-testing-validating-retrieval-accuracy-grounding-and-context-leakage-b3145e3a7b26](https://medium.com/@gunashekarr11/rag-testing-validating-retrieval-accuracy-grounding-and-context-leakage-b3145e3a7b26)  
16. Building Idempotent Tools for Long-Running Agents | PADISO Blog, accessed June 15, 2026, [https://www.padiso.co/blog/building-idempotent-tools-for-long-running-agents/](https://www.padiso.co/blog/building-idempotent-tools-for-long-running-agents/)  
17. Designing for Trust Calibration: Why AI Tools Need to Stop Pretending to Be Certain | by Lena C | Bootcamp | Medium, accessed June 15, 2026, [https://medium.com/design-bootcamp/designing-for-trust-calibration-why-ai-tools-need-to-stop-pretending-to-be-certain-0e7b74d285be](https://medium.com/design-bootcamp/designing-for-trust-calibration-why-ai-tools-need-to-stop-pretending-to-be-certain-0e7b74d285be)  
18. Design Patterns For Building Trust, accessed June 15, 2026, [https://smart-interface-design-patterns.com/articles/the-trust-calibration-spectrum-in-ux/](https://smart-interface-design-patterns.com/articles/the-trust-calibration-spectrum-in-ux/)  
19. From Trust in Automation to Trust in AI in Healthcare: A 30-Year Longitudinal Review and an Interdisciplinary Framework - PMC, accessed June 15, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12562135/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12562135/)  
20. Best LLM Router and AI Gateway (2026) - Inworld AI, accessed June 15, 2026, [https://inworld.ai/resources/best-llm-router-ai-gateway](https://inworld.ai/resources/best-llm-router-ai-gateway)  
21. What Is an AI Gateway? Why Your Enterprise LLM Infrastructure Needs One - LiteLLM, accessed June 15, 2026, [https://www.litellm.ai/blog/what-is-an-ai-gateway](https://www.litellm.ai/blog/what-is-an-ai-gateway)  
22. Structured outputs with OpenAI and Pydantic - dida.do, accessed June 15, 2026, [https://dida.do/blog/structured-outputs-with-openai-and-pydantic](https://dida.do/blog/structured-outputs-with-openai-and-pydantic)  
23. Agent Idempotency: Build Tool Calls That Are Safe to Retry | Chanl ..., accessed June 15, 2026, [https://www.channel.tel/blog/idempotent-tool-calls-agent-retry-safety](https://www.channel.tel/blog/idempotent-tool-calls-agent-retry-safety)  
24. Explainable recommendation: when design meets trust calibration - PMC, accessed June 15, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC8327305/](https://pmc.ncbi.nlm.nih.gov/articles/PMC8327305/)  
25. Memory scaling for AI agents | Databricks Blog, accessed June 15, 2026, [https://www.databricks.com/blog/memory-scaling-ai-agents](https://www.databricks.com/blog/memory-scaling-ai-agents)  
26. Structured model outputs | OpenAI API, accessed June 15, 2026, [https://developers.openai.com/api/docs/guides/structured-outputs](https://developers.openai.com/api/docs/guides/structured-outputs)  
27. Stop Parsing JSON by Hand: Structured LLM Outputs With Pydantic - DEV Community, accessed June 15, 2026, [https://dev.to/klement_gunndu/stop-parsing-json-by-hand-structured-llm-outputs-with-pydantic-1pg0](https://dev.to/klement_gunndu/stop-parsing-json-by-hand-structured-llm-outputs-with-pydantic-1pg0)  
28. NeurIPS Poster SeCon-RAG: A Two-Stage Semantic Filtering and Conflict-Free Framework for Trustworthy RAG, accessed June 15, 2026, [https://neurips.cc/virtual/2025/poster/115589](https://neurips.cc/virtual/2025/poster/115589)  
29. LLM Gateway Comparison 2025 - what I learned testing 5 options in production : r/AIQuality, accessed June 15, 2026, [https://www.reddit.com/r/AIQuality/comments/1q57lfc/llm_gateway_comparison_2025_what_i_learned/](https://www.reddit.com/r/AIQuality/comments/1q57lfc/llm_gateway_comparison_2025_what_i_learned/)  
30. The Complete Guide to Using Pydantic for Validating LLM Outputs, accessed June 15, 2026, [https://machinelearningmastery.com/the-complete-guide-to-using-pydantic-for-validating-llm-outputs/](https://machinelearningmastery.com/the-complete-guide-to-using-pydantic-for-validating-llm-outputs/)

---

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

# AI-ENG-AK — The AI Engineering Mindset - Probabilistic Systems, Human Judgment & Iterative Control

## **Conceptual Glossary**

The formalization of AI engineering as a distinct systems discipline requires a rigorous, shared vocabulary. The following glossary establishes the technical definitions and operational boundaries for the core concepts that define the modern AI engineering mindset.

| Term | Operational Definition |
| :---- | :---- |
| **AI Engineering Mindset** | The systems-level operating philosophy that treats model outputs as non-deterministic behavior distributions rather than static code execution, requiring structural containment, continuous empirical evaluation, and sociotechnical coordination. |
| **Probabilistic Cognitive Infrastructure** | A multi-layered architecture that integrates non-deterministic foundation models to perform high-dimensional cognitive tasks—such as semantic synthesis, parsing, perception, and action preparation—within complex systems. |
| **Deterministic Operational System** | The surrounding traditional software shell consisting of typed schemas, permission boundaries, transactional databases, and hard-coded fallbacks designed to constrain, validate, and isolate probabilistic behaviors. |
| **Technical Humility** | The systematic practice of designing software architectures under the active assumption that models will fail silently, context windows will degrade, prompts will decay, and human operators will exhibit cognitive biases. |
| **Demo Skepticism** | The refusal to equate isolated prototype success under cooperative conditions with systemic production reliability, treating every unverified demo as a latent operational risk. |
| **Decomposition Discipline** | The methodology of breaking complex, open-ended cognitive tasks into discrete, observable, and verifiable steps to minimize the surface area of model autonomy in favor of deterministic control. |
| **Bounded Belief** | The structural restriction of operational trust to a highly specific, evaluated context-space, rejecting generalized assumptions regarding model capability or safety. |
| **Uncertainty Management** | The deliberate architectural capture, classification, and mitigation of non-deterministic system variances—such as factual, retrieval, or tool-state uncertainty—before they cascade into failures. |
| **Iterative Control** | The continuous engineering optimization loop that integrates real-time production traces, automated evaluations, and human corrections back into prompt design, context structures, and system guardrails. |
| **Measurement-Before-Trust** | The operational requirement to establish quantitative, multi-dimensional evaluation baselines grounded in production-realistic data prior to scaling or deploying any model-dependent capability. |
| **Human Judgment Integration** | The sociotechnical design of interfaces and workflows that provide human operators with the evidence, context, time, and explicit authority necessary to actively supervise and override AI systems. |
| **Failure as Signal** | The practice of treating operational errors not as isolated bugs to patch, but as systemic data points that expose flaws in offline evaluations, context boundaries, or architectural constraints. |
| **Anti-Mindset** | A recurring cognitive shortcut or architectural shortcut—such as benchmark theology or prompt mysticism—that introduces systemic risk and operational instability. |

## **AI Engineering Mindset Doctrine**

AI engineering is the discipline of designing probabilistic cognitive capabilities inside bounded, observable, testable, human-governed operational systems. Its mindset is humility under uncertainty, decomposition before automation, measurement before trust, iteration before scale, and production proof before belief.  
The foundational thesis of high-dimensional AI engineering is that a reference architecture is a decision artifact. The useful question is not "Can the model do it?" but rather: "Under what conditions does it do it, how do we know, what happens when it does not, who catches it, what does it cost, what does the user believe, what evidence survives, and why should production trust this behavior?"  
This doctrine shifts the locus of engineering rigor. In classical systems, rigor was embodied in hand-coded algorithms and extensive pre-production unit testing. In probabilistic cognitive systems, rigor relocates to defining clear specifications, upfront guardrails, and architectures that make systems verifiable by design.  
The goal of the AI engineer is to build a system where the internal model behaves probabilistically, but the external system behaves deterministically. The model may be probabilistic; the system must not be confused.

## **AI Engineering Is Not Normal Software Engineering**

AI engineering is not ordinary software engineering with model calls attached. Traditional software engineering assumes that deterministic components, typed interfaces, unit tests, and controlled deployments can produce predictable behavior. AI systems include those same deterministic components, but they also include probabilistic cognitive engines whose outputs vary with context, phrasing, data distribution, model route, retrieval state, and user behavior.

The engineering problem changes.

A model does not fail like a disk, database, or microservice. It may continue producing fluent outputs while violating factual, semantic, policy, permission, or user-expectation constraints. The system can appear healthy while quietly becoming wrong.

AI engineering therefore borrows from several traditions at once:

| Discipline | What AI Engineering Borrows |
| :---- | :---- |
| **Classical Software Engineering** | Typed interfaces, modularity, testing, deployment discipline, rollback, reliability engineering. |
| **Safety Engineering** | Control loops, hazard analysis, degraded modes, safe-state design, incident learning. |
| **Security Engineering** | Least privilege, boundary defense, untrusted input handling, supply-chain review, abuse modeling. |
| **Human Factors** | Trust calibration, review burden, automation bias, interface friction, role clarity. |
| **Data Engineering** | Lineage, freshness, access control, source authority, quality, lifecycle management. |
| **Product Architecture** | Workflow fit, value design, user tolerance, adoption, no-use decisions. |

System-theoretic safety thinking, including STAMP/STPA where consequences justify it, is especially useful because AI failures are often interaction failures: the model, retrieval system, tool, user interface, workflow, and governance layer each behaved locally “as designed,” while the assembled system produced unsafe behavior.

### **Traditional Software vs. AI Systems Engineering**

| Dimensional Vector | Traditional Software Engineering | AI Systems Engineering |
| :---- | :---- | :---- |
| **Logic Definition** | Explicit, hand-written instructions executing deterministic paths. | Learned behavior distributions shaped by prompts, context, weights, retrieval, and route. |
| **Validation Model** | Unit, integration, and acceptance tests against static specifications. | Statistical evals, behavioral regression suites, shadow testing, production correction loops. |
| **Interface Properties** | Rigid APIs and typed contracts. | Typed contracts plus natural-language seams, probabilistic outputs, and dynamic context. |
| **Failure Visibility** | Exceptions, crashes, failed assertions, broken builds. | Silent semantic drift, hallucination, citation failure, tool misuse, overtrust. |
| **Dependency Model** | Versioned libraries and services with explicit APIs. | Model providers, prompts, corpora, embeddings, tools, policies, and user behavior all drift. |
| **Upgrade Strategy** | Code diff plus deterministic regression test. | Route/prompt/schema/corpus/model change plus behavioral eval and deployment manifest. |
| **Security Boundary** | Inputs validated at API edges. | Inputs, retrieved content, model outputs, tool calls, memory, and user actions are all untrusted seams. |
| **UX Principle** | Predictable direct manipulation. | Trust calibration, evidence display, uncertainty handling, strategic friction. |
| **Deployment Meaning** | Development ends and operations begins. | Production begins empirical evidence collection. |
| **Reliability Goal** | Prevent component failure. | Control system behavior despite probabilistic components and shifting context. |

The professional shift is this:

```text
Classical software asks:
  Did the code execute the specified logic?

AI engineering asks:
  Did the system produce acceptable behavior
  under the actual context, evidence, user, policy, route, and risk tier?
```

Rigor does not disappear. It relocates to boundaries, measurements, contracts, evals, telemetry, human authority, and recovery behavior.

## **Probabilistic Cognitive Infrastructure Model**

An enterprise AI application is not a model wrapped in an API call. It is probabilistic cognitive capability embedded inside a deterministic operational system. Reliability emerges from the interaction of model behavior, context, retrieval, tools, permissions, human judgment, telemetry, governance, adoption, sourcing, and operations.

```text
PROBABILISTIC COGNITIVE INFRASTRUCTURE

[ 10. Sourcing / Infrastructure / Vendor Layer ]
        |
[ 9. Adoption / Workflow / Support Layer ]
        |
[ 8. Operations / Governance / Accountability Layer ]
        |
[ 7. Telemetry / Evaluation / Evidence Layer ]
        |
[ 6. Human Judgment / Review / Authority Layer ]
        |
[ 5. Tool / Action / State-Mutation Layer ]
        |
[ 4. Policy / Permission / Boundary Layer ]
        |
[ 3. Context / Evidence / Retrieval Layer ]
        |
[ 2. Deterministic Execution Shell ]
        |
[ 1. Probabilistic Cognitive Core ]
```

These are not perfectly separable boxes. They are interacting control loops. A failure in one layer often appears as a symptom in another: stale retrieval looks like hallucination; weak UI disclosure looks like overtrust; poor adoption looks like model failure; provider drift looks like prompt regression.

### **Infrastructure Layers**

| Layer | Responsibility | Common Failure Mode | Control Mechanism |
| :---- | :---- | :---- | :---- |
| **1. Probabilistic Cognitive Core** | Generates, classifies, summarizes, reasons, perceives, or drafts under context. | Fluent but false or misaligned output. | Route constraints, evals, output validation, bounded task scope. |
| **2. Deterministic Execution Shell** | Parses, validates, retries, structures, routes, and handles exceptions. | Treating model output as already safe or structured. | Schemas, validators, state machines, deterministic fallback. |
| **3. Context / Evidence / Retrieval Layer** | Selects what information enters the model. | Stale, unauthorized, irrelevant, poisoned, or conflicting context. | Source authority, freshness, permission filtering, grounding verification. |
| **4. Policy / Permission / Boundary Layer** | Determines what data, actions, tools, and users are allowed. | Model capability confused with system permission. | RBAC/ABAC, policy-as-code, tenant isolation, pre-egress controls. |
| **5. Tool / Action / State-Mutation Layer** | Converts proposals into real system actions. | Unauthorized, duplicate, partial, or unverifiable side effects. | Tool contracts, idempotency, preconditions, postcondition verification. |
| **6. Human Judgment / Review / Authority Layer** | Gives humans evidence, authority, time, and controls to supervise. | Rubber-stamping, undertrust, overload, unclear accountability. | Review UI, veto, correction tools, escalation, role design. |
| **7. Telemetry / Evaluation / Evidence Layer** | Measures behavior, quality, cost, drift, and incidents. | Logs exist but cannot prove or improve system behavior. | Eval suites, traces, evidence packages, correction capture, dashboards. |
| **8. Operations / Governance / Accountability Layer** | Owns runtime, incidents, policy, compliance, and lifecycle review. | Governance exists only as documents; incidents do not improve the system. | Runbooks, audits, policy gates, inventories, postmortems. |
| **9. Adoption / Workflow / Support Layer** | Ensures humans can use the system safely and productively. | Tool is installed but not adopted, trusted, supported, or understood. | Training, support, incentives, workflow redesign, feedback loops. |
| **10. Sourcing / Infrastructure / Vendor Layer** | Provides compute, models, APIs, hosting, contracts, and exit paths. | Vendor lock-in, route drift, cost shock, data-rights failure. | Gateway abstraction, vendor review, route manifests, exit drills. |

### **Core Principle**

The model is not the system. The system is the coordination of probabilistic cognition with deterministic accountability.

A model can produce language. The infrastructure must decide whether that language may become a claim, a recommendation, a memory, a tool call, a record, a decision, or an action.

## **Humility as Engineering Practice**

Humility in AI engineering is pre-incident realism. It is the discipline of treating unproven assumptions as untrusted until measurement, production evidence, and failure analysis justify confidence.

Humility is not pessimism. It is how professionals avoid being surprised by obvious things slightly later and more expensively.

### **Assumption Audit**

| Assumption to Distrust | Production Countercondition | Required Control |
| :---- | :---- | :---- |
| **The demo represents real use.** | Production inputs are messier, more adversarial, more ambiguous, and less cooperative. | Pilot with representative data, edge cases, and adversarial examples. |
| **Sample data is clean enough.** | Real data has missing fields, duplicates, stale records, malformed files, conflicting sources, and strange user phrasing. | Data readiness review, corpus lifecycle, parser tests, validation gates. |
| **Users will understand the interface.** | Users miss warnings, overtrust outputs, underuse tools, bypass workflows, and invent workarounds. | Trust-calibrated UI, training, feedback capture, adoption telemetry. |
| **Model upgrades are improvements.** | Provider changes can regress task-specific behavior while improving generic benchmarks. | Route-specific regression evals, shadow tests, rollback manifests. |
| **Retrieval grounds the answer.** | Retrieval can surface irrelevant, stale, unauthorized, or poisoned context. | Permission filtering, freshness checks, source authority, citation verification. |
| **Prompt edits are harmless.** | Prompt changes can fix one case and regress another. | Prompt versioning, linked evals, release gates. |
| **Costs scale linearly.** | Retries, long context, agent loops, review burden, support, and shadow evals create hidden cost. | Cost telemetry, resource contracts, loop caps, cost-per-success tracking. |
| **Vendors remain stable.** | Providers change terms, prices, models, limits, routes, regions, and behavior. | Gateway abstraction, vendor inventory, exit plans, provider-change evals. |
| **Humans will review carefully.** | Reviewers rubber-stamp, fatigue, overtrust, or lack evidence to judge. | Evidence UI, review timing signals, veto controls, sampled audits. |
| **Evals catch the important failures.** | Initial eval sets miss rare but important production failures. | Production correction capture, failure replay, eval-set expansion. |
| **Governance documents control behavior.** | Policies are bypassed unless enforced at runtime. | Policy-as-code, gateway checks, permissions, audit evidence. |
| **Fluent output is good output.** | High fluency can hide unsupported claims, wrong calculations, or unsafe advice. | Grounding, semantic validation, source-of-record checks. |
| **Fallbacks are safe by default.** | Backup routes may weaken privacy, capability, policy, or output contracts. | Approved fallback manifests and authority downgrade. |

### **Humility Operating Rule**

Every new AI capability should ship with a written list of assumptions and the evidence required to retire them.

```text
Assumption → Test → Evidence → Boundary → Owner → Review Trigger
```

Until that chain exists, confidence is storytelling.

## **Demo-to-Production Skepticism Model**

A polished demo proves that a system can succeed under cooperative conditions. Production asks whether it can remain useful, safe, affordable, and recoverable under uncooperative conditions.

Every impressive demo is a suspect until production evidence proves otherwise.

| What the Demo Proves | What Production Must Prove | Evidence Required | Release Gate |
| :---- | :---- | :---- | :---- |
| **Possible capability** | Behavior holds across realistic input diversity. | Representative evals, pilot traces, adversarial cases. | Task-specific eval pass. |
| **Interface direction** | Users understand the system role and can use it safely. | Usability tests, correction patterns, trust-calibration review. | Product/UX approval. |
| **Early user excitement** | Users adopt it after novelty fades. | Repeat use, task-completion improvement, support burden, bypass rate. | Adoption review. |
| **Rough technical feasibility** | System works with production data, permissions, latency, scale, and errors. | Integration test, load test, permission test, degraded-mode test. | Operational readiness gate. |
| **Compelling narrative** | Business value survives full cost-to-serve and review burden. | Cost per verified task, human review time, support impact, ROI sensitivity. | Value gate. |
| **Model can answer examples** | Answers are grounded, current, permitted, and verifiable. | Citation fidelity, source authority, freshness, conflict handling. | Grounding gate. |
| **Tool path can execute once** | Tool path handles retries, duplicates, partial state, and postcondition checks. | Idempotency test, rollback/compensation test, source-of-record verification. | Action-verification gate. |
| **Safety appears fine in demo** | System resists prompt injection, data leakage, misuse, and boundary attacks. | Red-team tests, policy checks, tenant isolation tests, egress controls. | Security gate. |
| **Humans can approve output** | Humans have evidence, time, authority, and veto power. | Review UI test, reviewer feedback, approval-quality metrics. | Human-review gate. |
| **Provider route works today** | Route remains stable through drift, outage, rate limits, and migration. | Shadow test, fallback test, vendor review, route manifest. | Sourcing/ops gate. |

### **Skepticism Rule**

A demo is allowed to earn attention. It is not allowed to earn trust.

Trust requires production-shaped evidence.

## **Decomposition Discipline**

The primary failure mode of naive AI design is delegating open-ended tasks to autonomous model loops. Professional AI engineering enforces a strict decomposition discipline: breaking workflows down until the safe automation boundary becomes obvious.  
Before any cognitive workflow is automated, the systems engineer must answer eleven distinct questions:

* **What is the user trying to accomplish?** Map the core business outcome, ignoring the technology, to establish the baseline requirement.  
* **Which steps are deterministic?** Identify tasks—such as mathematical calculations, relational queries, or form submissions—that can be handled by traditional software.  
* **Which steps require language, ambiguity, synthesis, perception, or judgment support?** Isolate the specific tasks that require a probabilistic foundation model.  
* **Which steps require source-of-record lookup?** Pinpoint where the system must fetch data from a traditional database, avoiding model memory retrieval.  
* **Which steps require human authority?** Define high-consequence decision points—such as financial mutations, medical recommendations, or security changes—that must be made by a human operator.  
* **Which steps can be drafted but not executed?** Isolate steps where the model can generate a candidate output for human review, rather than mutating state directly.  
* **Which steps can be automated safely?** Identify low-risk, verifiable steps that can run in the background under strict constraints.  
* **Which steps require approval?** Define the verification boundaries where system execution halts until explicit human sign-off is logged.  
* **Which outputs are verifiable?** Establish which model-generated outputs can be parsed and verified against hard schemas, rules, or test suites.  
* **Which failures are reversible?** Distinguish between actions that can be undone (e.g., drafting an email) and actions that are permanent (e.g., sending an payment).  
* **Which parts should be no-AI?** Identify steps that must be routed completely away from models to ensure absolute determinism and compliance.

Through this decomposition, the engineer isolates the probabilistic core, surrounding it with deterministic verification steps.

## **Uncertainty Management Model**

AI engineering does not eliminate uncertainty. It prevents uncertainty from silently becoming authority.

A mature AI system detects uncertainty, classifies it, chooses a safe response, assigns ownership, and preserves the minimum evidence needed to improve the system.

| Uncertainty Type | Signal | Risk | Safe System Response | Owner | Evidence Boundary |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Factual Uncertainty** | Claim lacks support, conflicts with source, or cannot be verified. | False answer becomes trusted output. | Mark unsupported, remove claim, ask for evidence, or route to human. | Retrieval / Eval Owner | Claim ID, source refs, verifier result. |
| **Retrieval Uncertainty** | Low relevance, sparse results, missing source-of-record, duplicate/conflicting chunks. | Model synthesizes from weak context. | Disclose limited evidence, rerun retrieval, use deterministic lookup, or refuse grounded answer. | Corpus / Retrieval Owner | Retrieval manifest, source IDs, freshness metadata. |
| **Temporal Uncertainty** | Source date/version does not match task time scope. | System answers with obsolete or future-inapplicable information. | Ask time scope, retrieve current source, label historical status, or refuse current claim. | Corpus / Product Owner | Source version/date, user time scope. |
| **Classification Uncertainty** | Low confidence, unstable class assignment, high disagreement, or ambiguous input. | Wrong route, policy, or workflow selected. | Ask clarification, route to exception queue, or use human triage. | Product / Eval Owner | Class candidates, confidence bucket, selected route. |
| **User-Intent Uncertainty** | Request is incomplete, contradictory, underspecified, or high-consequence. | System executes the wrong task. | Present structured choices, ask clarification, or require confirmation. | Product / UX Owner | Intent state, clarification prompt, user selection. |
| **Policy Uncertainty** | Conflicting instructions, unclear data use, missing approval, or boundary condition. | Unsafe or noncompliant output/action. | Deny by default, require approval, or escalate to governance/security. | Policy / Governance Owner | Policy version, decision, reason code. |
| **Permission Uncertainty** | User/resource/tool/tenant scope cannot be proven. | Unauthorized data access or action. | Block before data injection or side effect. | Security Owner | Subject/resource refs, policy decision. |
| **Model-Route Uncertainty** | Route drift, eval expiry, outage, latency anomaly, or unsupported capability. | Weak route handles strong task. | Use approved fallback, downgrade authority, or fail closed. | Platform Owner | Route manifest, eval status, health signal. |
| **Tool-State Uncertainty** | API error, unknown final state, partial write, invalid arguments, duplicate retry. | False success or corrupted state. | Query source of record, compensate/reconcile where possible, escalate if unresolved. | Tool / Ops Owner | Action ID, idempotency key, postcondition result. |
| **Cost / Resource Uncertainty** | Token, tool, latency, retry, or loop budget approaches limit. | Denial-of-wallet, timeout, degraded UX. | Stop loop, return partial, require approval, or queue. | Platform / FinOps Owner | Usage counters, budget state, route ID. |
| **Human-Review Uncertainty** | Very fast approvals, low disagreement, reviewer overload, high correction fatigue. | Rubber-stamping or review collapse. | Add evidence friction, sample secondary review, rebalance queue, or reduce automation. | Human Review / Adoption Owner | Review time bucket, correction/override rate, queue state. |
| **Vendor Uncertainty** | Terms, model behavior, price, rate limit, region, or subprocessor changes. | Contract, quality, or compliance regression. | Trigger vendor review, route eval, fallback test, or exit plan. | Procurement / Platform Owner | Vendor notice, contract ref, route eval result. |

### **Uncertainty Rule**

Uncertainty should change system authority.

```text
higher uncertainty → less autonomy
higher consequence → more evidence
weaker evidence → weaker claim
unknown permission → deny
unknown state → verify before proceeding
```

## **Measurement-Before-Trust Framework**

Trust is not granted by fluency, novelty, confidence, or benchmark rank. Trust is earned through repeated measurement inside bounded contexts.

The system should not ask, “Do we trust this model?” It should ask:

```text
For this task,
under this data class,
with this route,
for this user,
under this policy,
at this cost,
with this evidence,
how often does the system behave acceptably?
```

### **Metric Families**

| Metric Family | What It Measures | Example Signals |
| :---- | :---- | :---- |
| **Task Outcome** | Whether the workflow achieved the intended business result. | Completion rate, verified success, rework, downstream error rate. |
| **Grounding and Evidence** | Whether claims are supported by permitted, current, authoritative sources. | Claim support, citation fidelity, source authority, freshness, conflict handling. |
| **Schema and Semantic Validity** | Whether outputs are structured and meaningful. | Schema validity, enum validity, cross-field invariants, semantic consistency. |
| **Tool and Action Verification** | Whether proposed actions are authorized and completed correctly. | Tool-call validity, permission decision, postcondition success, false-success rate. |
| **Human Review Quality** | Whether humans meaningfully supervise the system. | Review time, override rate, correction categories, reviewer agreement, fatigue signals. |
| **Trust and Adoption** | Whether users rely appropriately and the workflow creates value. | Overtrust, underuse, bypass rate, repeat use, support burden. |
| **Cost and Resource** | Whether value survives full operating cost. | Cost per verified task, token/tool/retry cost, review labor, support cost. |
| **Reliability and Operations** | Whether the system remains available, bounded, and recoverable. | Latency p50/p95/p99, error rate, retry loops, incident rate, rollback rate. |
| **Drift and Regression** | Whether behavior changes over time. | Eval regression, correction clusters, route drift, corpus freshness decay. |
| **Governance and Evidence** | Whether the organization can prove what happened. | Deployment manifest, policy decision, audit evidence, retention compliance. |

### **Metric Selection Rule**

Metrics must be selected by pattern, task, and risk tier. A support assistant needs resolution and repeat-contact metrics. A document pipeline needs field precision and coordinate grounding. A workflow agent needs postcondition success and idempotency. A research agent needs claim support and source authority.

Generic model scores are not enough. Usage is not enough. Token volume is not enough. User delight is not enough.

### **Trust Gate**

| Condition | Trust Decision |
| :---- | :---- |
| Metrics exist only for demo cases. | Do not scale. |
| Metrics measure usage but not correctness. | Do not claim production value. |
| Metrics measure output but not downstream outcome. | Pilot only. |
| Metrics pass offline but fail production correction patterns. | Revise evals and contract boundaries. |
| Metrics show stable behavior within task/risk/data boundaries. | Trust only inside those boundaries. |

Trust is bounded by evidence. Evidence expires when the system changes.

## **Iterative Control Loop**

AI engineering is an empirical control discipline. Production behavior teaches the system what the original design, evals, prompts, data, policies, users, and assumptions missed.

```text
ITERATIVE CONTROL LOOP

specify
  ↓
prototype
  ↓
evaluate
  ↓
constrain
  ↓
deploy narrowly
  ↓
observe
  ↓
capture corrections
  ↓
update evals
  ↓
revise contracts
  ↓
harden architecture
  ↓
expand cautiously
  ↓
monitor drift
  ↓
rollback / recalibrate
  ↓
repeat
```

### **Control Loop Stages**

| Stage | Core Question | Primary Artifact | Owner |
| :---- | :---- | :---- | :---- |
| **Specify** | What task, user, data, risk, and value are in scope? | Use-case decision record, pattern card, requirements. | Product / Architect |
| **Prototype** | Can the capability work under bounded conditions? | Prototype, prompt/schema draft, route candidate. | Engineering |
| **Evaluate** | Does behavior meet task-specific evidence requirements? | Eval suite, golden set, failure taxonomy. | Eval Owner |
| **Constrain** | What contracts must surround the probabilistic core? | Prompt, schema, retrieval, tool, permission, resource contracts. | Architect / Security |
| **Deploy Narrowly** | Can a limited population use it safely? | Pilot plan, rollback route, support path. | Product / Ops |
| **Observe** | What happens in real workflow conditions? | Telemetry, traces, corrections, support signals. | Ops / Observability |
| **Capture Corrections** | What did humans repair, reject, override, or misunderstand? | Correction dataset, feedback events. | Product / Eval |
| **Update Evals** | Which production failures must become regression tests? | Expanded eval suite. | Eval Owner |
| **Revise Contracts** | Which boundary failed or was missing? | Updated contract registry. | Contract Owner |
| **Harden Architecture** | What structural change prevents recurrence? | Architecture patch, route change, UI change, policy update. | Architect / Platform |
| **Expand Cautiously** | Which new users/workflows are safe to add? | Scale decision, risk review. | Product / Governance |
| **Monitor Drift** | Is behavior changing as data, users, models, or vendors change? | Drift dashboard, shadow evals. | Platform / Eval |
| **Rollback / Recalibrate** | Should we revert, degrade, pause, or continue? | Rollback manifest, incident/postmortem. | Ops / Governance |

### **Developer Feedback Speedups**

Iteration fails when feedback is too slow. Mature AI platforms build speed without removing gates.

| Speedup Mechanism | Purpose |
| :---- | :---- |
| **Long-Running Eval Environments** | Keep representative staging systems alive so evals can run without repeated setup. |
| **Failure Replay Harness** | Reconstruct failed trajectories from scoped evidence and secure references. |
| **Context Forking** | Resume debugging from a past state instead of replaying an entire agent run. |
| **Local Mock Services** | Let developers test gateways, tools, databases, and provider responses safely. |
| **Contract Unit Tests** | Quickly test schemas, tool preconditions, policy decisions, and prompt variables. |
| **Shadow Evaluation** | Compare candidate routes without changing production behavior. |

### **Control Loop Question**

The recurring engineering question is:

```text
What did production teach us
that our design, evals, prompts, data, policies, users,
and assumptions missed?
```

## **Human Judgment Integration Model**

Enterprise AI systems must reject both extremes:

```text
AI replaces human judgment.
Humans are safely in control because someone clicked approve.
```

Human judgment must be designed into the workflow. A human who cannot inspect, refuse, correct, or understand the output is not in the loop; they are a liability sponge.

### **Human Judgment Requirements**

| Requirement | What It Provides | Pattern Examples |
| :---- | :---- | :---- |
| **Evidence Access** | Reviewer can inspect sources, records, calculations, coordinates, logs, or rationale. | Research Agent, Decision Cockpit, Document Intelligence. |
| **Context Visibility** | Reviewer sees relevant workflow history and system state. | Support Assistant, Workflow Agent, Human Review Queue. |
| **Time to Review** | Reviewer is not forced into instant rubber-stamping. | High-risk actions, legal/compliance review, underwriting. |
| **Authority Clarity** | Reviewer knows whether they are advising, approving, correcting, or deciding. | Decision Cockpit, Workflow Automation, Governed Agentic Workflow. |
| **Veto Power** | Reviewer can reject or halt output/action. | Human Review Queue, Coding Agent, Tool Execution. |
| **Correction Tools** | Reviewer can edit fields, claims, coordinates, routes, or payloads. | Document Intelligence, Multimodal Review, Support Assist. |
| **Escalation Path** | Reviewer can move the case to a higher authority or expert. | Support, compliance, security, regulated workflows. |
| **Accountability Boundary** | Human and system responsibility are explicit. | Decision Support, HR/finance/legal workflows. |
| **Uncertainty Display** | Reviewer can see unsupported, conflicting, stale, or low-confidence areas. | RAG, research, analytics, decision support. |
| **Anti-Pressure Controls** | System detects review overload, rushed approval, or rubber-stamping risk. | High-risk queues and regulated workflows. |
| **Feedback Capture** | Corrections improve evals, prompts, data, and training. | All review-enabled patterns. |

### **Risk-Tiered Human Authority**

| Risk Tier | Human Role |
| :---- | :---- |
| **Low** | User edits, accepts, or rejects suggestions. |
| **Medium** | Human reviews outputs before external use or material write-back. |
| **High** | Human approves action with evidence, rationale, and audit record. |
| **Regulated / Critical** | Human authority is primary; AI may organize evidence, draft, or check but not decide. |

### **Human Review Anti-Patterns**

| Anti-Pattern | Failure |
| :---- | :---- |
| **Approval Button With No Evidence** | Human cannot judge, only absorb liability. |
| **Review Queue Without Time Budget** | Review becomes rubber-stamping under throughput pressure. |
| **AI Recommendation Framed as Default** | Automation bias increases. |
| **No Reject Path** | Human cannot exercise authority. |
| **No Correction Capture** | System repeats the same failures. |
| **No Escalation Path** | Edge cases become hidden local workarounds. |

Meaningful human control is not a checkbox. It is an interface, workflow, evidence package, authority model, and organizational practice.

## **Bounded Belief Doctrine**

A professional AI engineer never believes in a model generally. Belief is bounded by task, data, route, user, policy, evidence, time, and observed behavior.

Believe an AI system only within the boundaries where it has been evaluated, observed, and operationally constrained.

### **Belief Boundaries**

| Boundary | What May Be Believed | Evidence Required | Breach Behavior |
| :---- | :---- | :---- | :---- |
| **Task Class** | The system works for a narrow, defined task. | Task-specific evals and production traces. | Route to discovery, no-AI, or human review. |
| **Data Class** | The system may process approved data types. | Data classification, permissions, retention policy. | Block context injection or require governance review. |
| **User Role** | Authorized users may access approved capability. | RBAC/ABAC, tenant scope, role mapping. | Deny or escalate. |
| **Model Route** | A route is acceptable for a task/risk tier. | Route manifest, eval pass, monitoring. | Use approved fallback or fail closed. |
| **Prompt Version** | This prompt behaves acceptably for linked evals. | Versioned prompt, hash, regression suite. | Rollback or block release. |
| **Retrieval Scope** | Retrieved context is permitted and authoritative enough. | Source IDs, freshness, ACL/RLS, grounding checks. | Disclose, reretrieve, or refuse. |
| **Tool Authority** | The system may propose or execute specific actions. | Tool contract, permission policy, idempotency, postcondition tests. | Block before side effect or compensate/reconcile. |
| **Eval Evidence** | Offline evidence supports limited deployment. | Golden set, adversarial cases, benchmark by task. | Pilot only or redesign. |
| **Production Behavior** | Live behavior remains within acceptable bounds. | Telemetry, corrections, incidents, support signals. | Recalibrate, roll back, or pause. |
| **Failure Response** | The system can recover safely. | Degraded mode, rollback manifest, runbook, incident drill. | Reduce authority or block production. |
| **Governance Approval** | Risk tier is organizationally accepted. | Approval record proportional to risk. | Require review before expansion. |
| **Time Window** | Evidence remains current enough. | Review date, model/corpus/vendor-change status. | Revalidate before relying. |

### **Bounded Belief Rule**

```text
No evidence outside the boundary.
No trust outside the boundary.
No authority outside the boundary.
```

The boundary is part of the system.

## **Failure as Signal Doctrine**

Operational failures are inevitable in probabilistic environments. The wrong response is to patch the nearest prompt and declare the system healed. A failure is information about which assumption was false, which boundary was weak, which metric was missing, or which workflow was misunderstood.

### **Failure Learning Matrix**

| Failure Reveals | Update Target | Artifact Changed | Owner |
| :---- | :---- | :---- | :---- |
| **Eval missed a case** | Evaluation suite. | New golden/adversarial case, failure taxonomy update. | Eval Owner |
| **Prompt allowed unsafe behavior** | Prompt contract. | Prompt version, refusal behavior, linked evals. | Prompt Owner |
| **Retrieved evidence was wrong or stale** | Retrieval/corpus contract. | Source authority, freshness, chunking, index lifecycle. | Corpus Owner |
| **Output passed schema but was wrong** | Semantic/factual validator. | Business rules, grounding checks, verifier tests. | Domain / Eval Owner |
| **Tool call failed or misfired** | Tool/action contract. | Preconditions, idempotency, postcondition checks, compensation path. | Tool Owner |
| **Permission boundary failed** | Policy contract. | RBAC/ABAC rule, tenant filter, approval state, egress control. | Security / Governance |
| **User overtrusted output** | User expectation design. | Evidence UI, uncertainty display, confirmation gate. | Product / UX |
| **Human review collapsed** | Human review model. | Queue design, review instructions, sampling, workload allocation. | Review Owner |
| **Users bypassed the workflow** | Adoption system. | Training, support, workflow redesign, incentive alignment. | Adoption / Product |
| **Provider/model drifted** | Sourcing and route strategy. | Route manifest, fallback, vendor review, shadow eval. | Platform / Procurement |
| **Cost spiked** | Resource contract. | Token budget, loop cap, retry policy, route selection. | Platform / FinOps |
| **Fallback weakened safety** | Degraded mode. | Fallback manifest, authority downgrade, policy parity test. | Ops / Platform |
| **Incident response was slow** | Runbook. | Alert threshold, owner, escalation, rollback drill. | Operations |
| **Pattern was wrong** | Architecture pattern card. | No-use condition, pattern selection rule, reference blueprint. | Architect |

### **Failure Handling Rule**

A failure should produce at least one durable artifact update:

```text
eval case
contract revision
policy update
runbook change
UI correction
training example
pattern no-use condition
route manifest change
```

If nothing durable changed, the system learned nothing.

## **Anti-Mindset Catalog**

Systemic failures in enterprise AI projects are often caused by the adoption of flawed mental models.  
These "Anti-Mindsets" treat probabilistic systems as if they were simple, deterministic, or inherently safe, leading to poor designs, security vulnerabilities, and project failures.

| Anti-Mindset | Primary Symptoms | Why It Is Tempting | Operational Failure Mode | Healthy Posture |
| :---- | :---- | :---- | :---- | :---- |
| **Demo Worship** | Standardizing core architecture based on a small set of successful prototype runs under optimal conditions. | Provides immediate progress signals; generates high interest and secures funding from leadership teams. | Complete system collapse when exposed to messy production payloads, retrieval noise, and edge cases. | Treat the demo as a highly fragile proof of concept; verify all system capabilities with robust evaluation suites. |
| **Prompt Mysticism** | Treating prompt engineering as an art or magic, manually tweaking words without structured testing. | Requires zero engineering setup; provides immediate feedback loops on single local test inputs. | Extreme system fragility; prompt changes fix one bug while silently regressing dozens of other scenarios. | Treat prompts as versioned, strictly controlled system configuration files verified via regression testing. |
| **Benchmark Theology** | Assuming high ranks on public LLM leaderboards guarantee success in domain-specific workflows. | Simplifies vendor and model selection; avoids the hard engineering work of building custom evaluations. | Poor real-world performance; models fail to handle specific enterprise schemas, terminology, and workflows. | Build domain-specific evaluation suites; rank candidate models using real historical production telemetry. |
| **Automation Maximalism** | Attempting to automate highly complex, open-ended workflows without human review boundaries. | Promises massive cost reduction and complete operational scale-out in a single project release. | High rates of silent failures, compliance violations, and severe brand damage when models fail silently. | Decompose workflows; automate low-risk, highly structured steps and use models to draft actions for human review. |
| **Chatbox Monoculture** | Forcing all user interactions through free-form natural language chat interfaces. | Simplifies design tasks; leverages the popular, familiar design patterns of consumer AI products. | Extremely high user friction, search failures, high latency, and severe automation-bias patterns. | Use structured UI components; deploy chat only when handling high-entropy, highly ambiguous inputs. |
| **Human-in-the-Loop Theater** | Using human review simply to click "approve" without providing context, evidence, or veto power. | Keeps the system fast and high-throughput while claiming regulatory and risk compliance on paper. | Operators experience rapid automation bias, rubber-stamping incorrect outputs and missing silent errors. | Design interfaces that show supporting evidence and force active verification through interaction friction. |
| **Vendor Faith** | Treating vendor claims, dynamic updates, and standard model alignments as robust security layers. | Outsources complex security and safety challenges; reduces initial development overhead and setup times. | Sudden system regression or security failure when models are updated or alignments are bypassed. | Build independent input and output safety filters; treat every external model as a hostile dependency. |
| **Metric Cosmetics** | Tracking usage, user logins, and token volume while ignoring quality, grounding, and error rates. | Dashboards look positive; metrics are easy to gather and show immediate, high-velocity adoption signs. | Silent operational decay; enterprise users abandon the tool because of high error rates and support burdens. | Track task success rates, grounding, citation precision, human veto rates, and error costs. |
| **Agent Romanticism** | Designing systems with autonomous, multi-turn, unstructured planning loops. | Creates highly impressive demos; aligns with the narrative of building fully autonomous systems. | Infinite execution loops, runaway API costs, high latencies, and severe cascading failure paths. | Constrain agents using structured workflow loops with clear state machines and hard stop boundaries. |
| **Scale Prematurely Syndrome** | Rolling out systems to large user bases before establishing evaluations, rollbacks, and support structures. | Driven by market competition and executive pressure to rapidly demonstrate progress. | High incident rates, system outages, massive support ticket volumes, and rapid trust loss among users. | Scale incrementally; prove system stability in shadow and canary releases before expanding user access. |
| **Determinism Delusion** | Treating probabilistic model completions as if they were predictable, static code paths. | Simplifies code; allows developers to write traditional software integrations without exception paths. | Frequent system crashes and validation failures when models return unexpected output formats or schemas. | Isolate models behind strict validation contracts; design robust fallbacks and degraded operational modes. |
| **RAG Salvationism** | Assuming that adding dynamic retrieval (RAG) completely eliminates hallucination and security risks. | Avoids complex model fine-tuning; provides a simple mechanism to ground model outputs in enterprise data. | Models hallucinate over irrelevant search results or execute instructions injected into retrieved context. | Build evaluation layers to verify context relevance, semantic grounding, and data-instruction isolation. |
| **Governance Theater** | Writing extensive legal and policy guidelines while lacking real-time runtime control enforcement. | Satisfies compliance audits on paper; avoids the technical work of building gateway filter systems. | Policy guidelines are bypassed by users or ignored by models; security breaches occur despite written policies. | Implement real-time policy evaluation and runtime gatekeeping at the API gateway layer. |
| **Cost Blindness** | Designing complex agentic chains without tracking token volume, support costs, or review overhead. | Simplifies initial prototyping; assumes API tokens are too cheap to impact business viability. | Runaway operating costs, high cloud bills, and low ROI when human review and support burdens are counted. | Calculate the cost per successful outcome; track token and model execution costs for every user flow. |

## **AI Engineering Maxims**

These maxims define the professional operating code of AI engineering.

| Maxim | Meaning |
| :---- | :---- |
| **The model is not the system.** | The system includes context, retrieval, schemas, tools, permissions, telemetry, humans, governance, operations, sourcing, and adoption. |
| **Fluency is not truth.** | Polished prose is not evidence, correctness, authority, or safety. |
| **Retrieval is not evidence unless verified.** | Retrieved context must be permissioned, current, authoritative, and claim-supporting. |
| **A demo is not a deployment.** | Cooperative success proves possibility, not production reliability. |
| **Human approval is not human judgment.** | Judgment requires evidence, time, authority, correction tools, and veto power. |
| **Failure budget is not harm permission.** | A workflow that cannot tolerate error needs stronger gates or no AI. |
| **Every model route is bounded.** | Routes are approved only for specific tasks, data classes, users, and risk tiers. |
| **Every agent needs a stop condition.** | Unbounded loops become cost, latency, state, and safety failures. |
| **Every tool action needs authorization and verification.** | The system must check permission before execution and confirm state after execution. |
| **Every memory needs scope, provenance, and deletion.** | Persistent state must be permissioned, inspectable, and governed. |
| **Every fallback must preserve safety properties.** | Degraded modes may reduce capability, not policy, privacy, or evidence requirements. |
| **Every eval is a release contract.** | Prompt, schema, route, retrieval, tool, and policy changes require risk-appropriate evaluation. |
| **Every production failure should teach the system.** | Failures should update evals, contracts, policies, runbooks, UI, or pattern cards. |
| **No-use is an architectural decision.** | Deterministic software, manual workflow, or no-AI is often the correct answer. |
| **The system should be more deterministic than the model.** | Variance belongs inside bounded cognitive work, not at state, permission, evidence, or action boundaries. |
| **Inside the box, the model may dance; at the edges, it signs paperwork.** | Let models synthesize internally; enforce strict contracts at every system boundary. |

## **Professional Practices of AI Engineers**

AI engineering is a professional discipline. It requires repeatable habits, not heroic prompt tinkering.

| Practice | Daily Behavior | Durable Artifact | Failure Prevented |
| :---- | :---- | :---- | :---- |
| **Write Assumptions Explicitly** | State task, data, user, route, risk, cost, and failure assumptions before building. | Assumption log / decision record. | Hidden demo assumptions. |
| **Decompose Before Automating** | Split workflows into deterministic, probabilistic, human, and no-AI steps. | Workflow decomposition map. | Over-automation. |
| **Build Evals Before Scaling** | Create task-specific tests before broad rollout. | Eval suite / golden set. | Demo-to-production collapse. |
| **Test Messy and Hostile Conditions** | Include malformed input, missing data, conflicting sources, injection attempts, and edge cases. | Adversarial eval set. | Fragile prompt success. |
| **Instrument What Matters, Minimize What Hurts** | Capture route, cost, validation, correction, and breach signals while redacting or referencing sensitive payloads. | Telemetry and evidence schema. | Blind operation or overcollection. |
| **Use Deterministic Code Where Sufficient** | Prefer rules, SQL, forms, parsers, and APIs when exactness is required. | No-AI / deterministic design record. | RAG-as-database and model overreach. |
| **Isolate Model Behavior Behind Contracts** | Validate inputs, outputs, tools, permissions, resources, routes, and memory. | Contract registry. | Probabilistic spillover. |
| **Route by Task and Risk** | Match model route to task complexity, data class, latency, cost, and consequence. | Route manifest. | Weak model on strong task. |
| **Preserve Human Authority Where Consequence Demands It** | Require human approval with evidence for high-impact decisions/actions. | Human review workflow. | Rubber-stamped harm. |
| **Keep Rollback and Degraded Modes Ready** | Maintain rollback manifests, approved fallback routes, and manual recovery paths. | Runbook / deployment manifest. | Irrecoverable production failure. |
| **Track Cost-to-Serve Continuously** | Measure cost per verified outcome, including model, retrieval, tools, infra, review, support, and failures. | Cost dashboard. | ROI fantasy and cost bombs. |
| **Update Evals From Production Corrections** | Convert edits, overrides, incidents, and support failures into regression cases. | Eval update log. | Repeated production failure. |
| **Treat Vendor Changes as System Changes** | Re-evaluate affected routes when providers change models, terms, limits, regions, or prices. | Vendor-change review / route eval. | Silent dependency drift. |
| **Treat Adoption as Architecture** | Design training, support, incentives, and workflows as part of the system. | Adoption plan / support model. | Installed-but-unused AI. |
| **Document No-Use Conditions** | State when the pattern must not be used. | Pattern card no-use section. | Pattern overreach. |
| **Prefer Boring Recoverable Systems** | Choose designs that are observable, bounded, reversible, and understandable. | Reference architecture / runbook. | Dazzling fragile autonomy. |

The professional AI engineer does not ask, “How impressive can this be?” first.

They ask, “How does this fail, and what survives?”

## **Capstone Cross-Canon Synthesis**

AI-ENG-AK is the operating philosophy that binds the AI Engineering Systems Canon together. Each prior domain provides a specialized discipline. AK defines the mindset required to use those disciplines without being seduced by demos, benchmarks, or model fluency.

| Canon Domain | Engineering Contribution | AK Mindset Commitment |
| :---- | :---- | :---- |
| **Prompt and Context Architecture** | Instruction hierarchy, context windows, memory, state, prompt configuration. | Treat prompts and context as versioned, bounded, testable system configuration. |
| **Corpus Engineering** | Data provenance, metadata, document lifecycle, source authority. | Treat knowledge as governed infrastructure, not loose text dumped into a model. |
| **Retrieval and Freshness** | Search, RAG, reranking, citation, source conflict, freshness. | Treat retrieved context as candidate evidence requiring verification. |
| **Model Selection and Routing** | Model choice, capability profile, cost, route class, fallback. | Trust routes only inside evaluated task/data/risk boundaries. |
| **Model Adaptation** | Fine-tuning, adapters, specialization, weight behavior. | Treat adaptation as a new system behavior requiring regression evidence. |
| **Regression Control** | Evals, golden sets, shadow testing, release gates. | Treat every change as a hypothesis that must survive measurement. |
| **Throughput and Serving** | Latency, batching, queues, streaming, model serving. | Treat performance as part of reliability and user trust. |
| **Agentic Orchestration** | Planning, graph execution, multi-agent workflows. | Constrain autonomy with state machines, budgets, checkpoints, and human authority. |
| **Tool Contracts and Action Verification** | Tools, APIs, idempotency, postconditions, source-of-record checks. | Never confuse generated action text with authorized completed action. |
| **Multimodal and Realtime Systems** | Images, audio, video, speech, streaming interaction. | Treat non-text evidence and realtime behavior as special uncertainty surfaces. |
| **UI Agents** | Browser/UI state, visual control, interface actions. | Give agents no UI authority without permission, verification, and rollback. |
| **Production Pathologies** | Failure modes, retry storms, silent regressions, brittle chains. | Expect failure; design containment before incidents teach it violently. |
| **Boundary Defense and Supply Chain** | Prompt injection, tenant isolation, SBOM/AI-BOM, artifact provenance. | Treat every model, tool, corpus, and vendor dependency as untrusted until bounded. |
| **Resource Abuse** | Cost bombs, loops, denial-of-wallet, resource exhaustion. | Bound every loop, token budget, tool call, retry, and route. |
| **UX Resilience and Trust** | Degraded modes, transparency, contestability, expectation management. | Make uncertainty, authority, fallback, and evidence visible to users. |
| **Human Review** | Review queues, maker-checker, escalation, reviewer burden. | Human approval is meaningful only with evidence, time, veto, and accountability. |
| **Telemetry, Evaluation, and Evidence** | Observability, eval suites, traces, proof artifacts. | Production is the primary teacher; evidence must be scoped, structured, and usable. |
| **Operations and Governance** | Runbooks, incidents, policy, audit, accountability. | Governance must execute at runtime, not merely decorate documents. |
| **Sustainability** | Energy, carbon, cost, routing, lifecycle impact. | Route by value, quality, cost, resource impact, and lifecycle—not model spectacle. |
| **Product Architecture** | Use-case selection, workflow fit, value design, no-AI path. | Ask whether AI belongs before asking which model to use. |
| **Adoption Systems** | Training, incentives, support, workflow redesign, skill preservation. | Adoption is architecture; humans are part of the system. |
| **Sourcing Strategy** | Build/buy/open/vendor, data rights, exit, control plane. | Own or control the surfaces that define safety, value, evidence, and exit. |
| **Contract Thinking** | Deterministic boundaries around probabilistic cores. | Every seam needs an owner, enforcer, validation method, and breach behavior. |
| **Reference Architectures** | Failure-aware pattern cards and no-use conditions. | Reuse patterns only when their assumptions, controls, and degraded modes fit. |

### **Canonical Integration**

```text
AI engineering is probabilistic capability
inside deterministic accountability.

Capability comes from models.
Reliability comes from systems.
Trust comes from evidence.
Safety comes from boundaries.
Value comes from workflow fit.
Durability comes from iteration.
```

AK is the mindset that keeps all of those truths active at the same time.

## **Final Canon Handoff Statement**

The AI Engineering Systems Canon closes with a simple discipline:

```text
Do not believe the model.
Believe the measured system.
```

AI engineering is not normal software engineering with a chat box bolted onto the side. It is the practice of placing probabilistic cognition inside deterministic accountability, then measuring, constraining, observing, correcting, governing, and operating it until production earns trust.

The model may draft. The system must verify.

The model may synthesize. The system must ground.

The model may plan. The system must authorize.

The model may propose action. The system must check permission, preserve state, verify postconditions, and recover from failure.

The model may impress. The system must survive.

Across these volumes, the canon has defined the layers required to make that possible: context architecture, corpus engineering, retrieval, freshness, model routing, serving, orchestration, tools, action verification, multimodal understanding, realtime interaction, UI agents, pathologies, boundary defense, supply-chain security, resilience, trust, human review, telemetry, evaluation, evidence, operations, governance, sustainability, product architecture, adoption, sourcing, contract thinking, and reference architectures.

The final professional posture is unsentimental:

```text
Decompose before automating.
Measure before trusting.
Constrain before scaling.
Observe before believing.
Preserve human judgment where consequence demands it.
Treat failure as instruction.
Prefer boring recoverable systems over dazzling fragile ones.
```

Build systems that can say no.

Build systems that can degrade safely.

Build systems that can prove what happened.

Build systems that can be corrected.

Build systems whose users understand the boundary between assistance and authority.

The future of AI engineering will not be won by whoever worships the largest model. It will be won by whoever builds the most disciplined systems around probabilistic capability.

Inside the box, the model may dance.

At the edges, it signs paperwork.

---

## Attribution and License

This knowledge pack is part of **Stunspot’s Guide to AI Systems** — *The AI Engineering Systems Canon*.

Created by **Sam “stunspot” Walker** / **Collaborative Dynamics**.

Repository: https://github.com/Stunspot/stunspots-guide-to-ai-systems  
Stunspot: https://stunspot.com  
Collaborative Dynamics: https://www.collaborative-dynamics.com  
Discord: https://discord.gg/stunspot  
Support: https://www.patreon.com/StunspotPrompting

Licensed under **CC BY-NC-SA 4.0** unless otherwise stated.  
Commercial use, resale, paid redistribution, inclusion in commercial training products, or incorporation into paid knowledge-base products requires prior written permission.

[← Back to Knowledge Packs](../docs/knowledge-packs.md)