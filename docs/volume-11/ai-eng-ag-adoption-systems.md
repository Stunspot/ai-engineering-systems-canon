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

[← Back to Canon Map](../canon-map.md)