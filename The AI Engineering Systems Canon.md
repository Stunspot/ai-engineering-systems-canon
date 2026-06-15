\=
\#
\<
\>
\[
\]
\-
\*
\+
\_
\.
\~

Reformat any and all LaTeX expressions as plaintext if present.
💠‍🌐Nova
# 🧠 The AI Engineering Systems Canon
*A Doctrinal Knowledge Base for High-Dimensional AI System Architecture*

---

Reformat any and all LaTeX expressions as plaintext if present.

## Volume 1: The Informational/Epistemic Layer
*How models think, how meaning is steered, and how state becomes usable.*

### AI-ENG-A - Model Steering: Harness Engineering, Prompt Semantics & Adaptation Choice
Moves beyond prompt engineering into systematic model steering. Covers prompt structure, harness design, instruction hierarchy, latent-space shaping, task framing, output contracts, and the strategic choice between prompting, RAG, memory, fine-tuning, distillation, and tool use. Teaches when each method is powerful, brittle, excessive, or simply the wrong tool.

### AI-ENG-B - Context Architecture: State Management & The Tenure Principle
Treats context as a managed state layer rather than a bag of semantically similar text. Covers session state, cross-session memory, belief-state design, hard scope isolation, named entity resolution, bounded vocabularies, temporal validity, “why it matters” fields, fact-to-instruction conversion, and the prevention of stale or contradictory memory contamination.

### AI-ENG-C - The Economic Physics of Inference: Tradeoffs, Cost Attribution & System Margins
Defines the physical and financial laws of AI systems. Covers the tradeoffs among latency, quality, reliability, cost, throughput, context size, model class, and infrastructure. Shifts cost thinking from “model price” to granular attribution by feature, workflow, tenant, user journey, and business outcome.

---

## Volume 2: Knowledge, Data, and Corpus Engineering
*Where trustworthy external knowledge comes from, how it is shaped, and how it enters the system.*

### AI-ENG-D - Corpus Engineering: Data Provenance, Knowledge Hygiene & Source Authority
Establishes the knowledge substrate beneath RAG and memory. Covers ingestion, normalization, deduplication, lineage, metadata, source authority, permission-aware indexing, canonical IDs, alias tables, versioning, retention, redaction, archival policy, and conflict resolution when sources disagree. Treats the corpus as an engineered asset, not a document landfill.

### AI-ENG-E - The Retrieval Pipeline: RAG Architecture, Hybrid Search & Semantic Injection
Covers retrieval as a precision delivery system for external context. Includes chunking strategies, embeddings, lexical search, hybrid retrieval, reranking, query rewriting, metadata filtering, freshness checks, citation construction, attribution quality, and context assembly. Distinguishes finding relevant text from injecting useful, current, permission-safe knowledge.

### AI-ENG-F - Knowledge Freshness, Conflict Detection & Context Rot Prevention
Focuses on the decay of information over time. Covers stale retrieval, superseded documents, contradictory policies, orphaned facts, version drift, temporal scoping, expiration rules, recency weighting, source precedence, and automated detection of corpus rot before it poisons model behavior.

---

## Volume 3: Model Lifecycle and Adaptation
*How models are selected, modified, evaluated, compressed, deployed, and retired.*

### AI-ENG-G - Model Selection: Capability Fit, Deployment Fit & Failure Tolerance
Covers choosing models by task profile, reasoning depth, context length, tool use, modality, latency tolerance, cost ceiling, license constraints, language coverage, privacy requirements, hardware target, and acceptable failure modes. Teaches model choice as architecture, not leaderboard shopping.

### AI-ENG-H - Model Adaptation: Fine-Tuning, LoRA, Preference Tuning & Distillation
Covers supervised fine-tuning, LoRA/QLoRA, preference tuning, domain adaptation, synthetic data generation, dataset design, adapter management, and distillation. Explains when adaptation improves behavior, when it merely overfits style, and when RAG or harness design would be cleaner.

### AI-ENG-I - Regression Control: Model Registries, Rollouts & Behavioral Drift
Covers model registries, experiment tracking, artifact versioning, model cards, canary deploys, shadow testing, A/B tests, rollback procedures, and silent regression detection. Focuses on preserving production behavior as models, prompts, tools, corpora, and workflows evolve.

---

## Volume 4: Runtime Architecture and Inference Mechanics
*How AI systems actually execute under physical, computational, and operational constraints.*

### AI-ENG-J - Throughput Mechanics: Memory Pressure, KV Cache & Hardware Realities
Covers the brutal physics of scaling inference. Includes prefill vs. decode latency, KV cache management, eviction policies, prompt caching, semantic caching, continuous batching, paged attention, GPU memory pressure, CPU/GPU tradeoffs, and capacity planning under real workloads.

### AI-ENG-K - Weight Dynamics: Quantization, Compression & Decoding Strategy
Covers INT8, INT4, FP8, AWQ, GPTQ, pruning, speculative decoding, draft models, constrained decoding, sampling behavior, and quality degradation boundaries. Teaches how to make models faster and cheaper without quietly destroying their reasoning, formatting, or instruction-following reliability.

### AI-ENG-L - Model Serving Architecture: Routing, Load Balancing & Deployment Topologies
Covers inference servers, API gateways, local deployment, edge deployment, cloud deployment, multi-model routing, tenant-aware capacity, queueing, backpressure, autoscaling, cold starts, rate limits, and high-availability serving patterns.

---

## Volume 5: Agentic Systems and Tool-Using Architectures
*How static generators become actors, and how to keep them from becoming raccoons with API keys.*

### AI-ENG-M - Agentic Orchestration: Autonomy Boundaries, Loop Budgets & Termination
Covers agent loops, planning, reflection, decomposition, memory use, tool selection, scratchpads, state machines, graph-based workflows, loop budgets, tool budgets, timeout policies, escalation triggers, and termination conditions. Treats autonomy as a bounded architectural affordance, not a vibe.

### AI-ENG-N - Tool Contracts: Function Calling, Schemas, Validation & Idempotency
Covers tool interfaces, schema design, argument validation, retry logic, repair loops, deterministic wrappers, idempotency, side-effect control, transactional boundaries, confirmation gates, and safe execution patterns. Establishes the principle that probabilistic actors need deterministic edges.

### AI-ENG-O - Action Verification: Planning, Execution, Observation & Recovery
Covers verifying that tools did what the model thinks they did. Includes post-action checks, state reconciliation, rollback paths, partial failure handling, compensating actions, tool-result grounding, and preventing hallucinated success.

---

## Volume 6: Multimodal and Interface-Controlling Systems
*How AI engineering changes when the system reads, sees, hears, speaks, and acts through interfaces.*

### AI-ENG-P - Multimodal Understanding: Documents, Images, Tables, Charts & Video
Covers OCR, layout-aware document parsing, table extraction, form understanding, chart interpretation, image retrieval, video sampling, visual grounding, multimodal embeddings, and evidence selection. Focuses on whether the system inspected the right evidence, not merely whether it produced plausible text.

### AI-ENG-Q - Speech, Voice, and Real-Time Interaction Systems
Covers speech-to-text, text-to-speech, turn-taking, interruption handling, latency budgets, streaming generation, conversational repair, voice identity, accessibility, and the UX consequences of real-time AI behavior.

### AI-ENG-R - UI Agents: Browser Control, Desktop Automation & Visual State
Covers agents that operate software interfaces. Includes DOM inspection, screenshot reasoning, visual state tracking, action planning, click/type verification, form submission safety, browser sandboxing, and recovery from interface drift.

---

## Volume 7: Failure, Security, and Hostile Environments
*How AI systems break, leak, get attacked, or quietly become cursed.*

### AI-ENG-S - Production Pathologies: Hallucination, Malformed Output & Runaway Behavior
Covers hallucinated tool calls, malformed JSON, broken schemas, invalid citations, instruction loss, tool loops, brittle chains, contradictory outputs, non-deterministic regressions, and production failure patterns that do not appear in demos.

### AI-ENG-T - Boundary Defense: Prompt Injection, Data Leakage & Tenant Isolation
Covers prompt injection, indirect prompt injection, cross-user contamination, system prompt exposure, sensitive data leakage, retrieval poisoning, cache leakage, multi-tenant isolation, permission boundaries, and context separation in B2B environments.

### AI-ENG-U - AI Supply Chain Security: Models, Datasets, Dependencies & Tool Surfaces
Covers model provenance, malicious artifacts, dataset poisoning, embedding poisoning, dependency risk, parser risk, plugin risk, tool-server risk, secrets handling, sandboxing, egress control, and output-handling vulnerabilities when model output flows into code, shell, SQL, email, browsers, or internal systems.

### AI-ENG-V - Resource Abuse, Cost Bombs & Unbounded Consumption
Covers denial-of-wallet attacks, recursive agents, context flooding, retrieval flooding, tool-loop exhaustion, adversarial latency inflation, runaway batch jobs, quota exhaustion, and budget-aware containment systems.

---

## Volume 8: W-Y Resilience, Degraded Modes, and Human Trust
*How systems fail gracefully enough that users do not feel the machinery grinding underneath them.*

### AI-ENG-W - UX Resilience: Model Routing, Fallback Chains & Degraded-Mode Grace
Covers model routing, fallback models, dynamic quality/cost switching, cached answers, partial answers, graceful error states, escalation to humans, retry UX, and user-facing continuity when backend systems degrade.

### AI-ENG-X - Human-System Interface: Trust Calibration, Provenance & Control
Covers uncertainty display, citation UX, explanation design, approval flows, edit capture, undo mechanisms, confidence signaling, human correction loops, and interfaces that prevent both overtrust and undertrust.

### AI-ENG-Y - High-Impact Workflow Design: Confirmation, Review & Human-in-the-Loop Governance
Covers workflows where AI actions affect money, legal exposure, customer relationships, safety, reputation, or operational continuity. Includes approval thresholds, review queues, audit trails, escalation paths, role-specific permissions, and human accountability design.

---

## Volume 9: Observability, Evaluation, and Verification
*How to know whether the system is actually doing what it claims to do.*

### AI-ENG-Z - Strategic Telemetry: Traces, Tokens, Latency & Semantic Drift
Covers LLM observability as a first-class engineering discipline. Includes traces, spans, tokens, latency, errors, retries, tool calls, retrieval events, routing decisions, cost attribution, semantic drift, and the Green Dashboard Fallacy: infrastructure health without answer quality.

### AI-ENG-AA - Evals Architecture: Ground Truth, Golden Sets & Regression Tests
Covers task evals, retrieval evals, grounding evals, citation evals, tool-use evals, adversarial evals, golden sets, synthetic test generation, human review, LLM-as-judge limits, inter-rater reliability, and regression gates before deployment.

### AI-ENG-AB - Verification Artifacts: Auditability, Reproducibility & Evidence Trails
Covers preserving the evidence needed to understand why a system answered or acted as it did. Includes prompt versions, model versions, retrieved chunks, tool calls, intermediate states, user edits, policy checks, output diffs, and replayable execution traces.

---

## Volume 10: Operations, Governance, and Lifecycle Management
*How AI systems are maintained as living infrastructure rather than one-time builds.*

### AI-ENG-AC - AI Operations: Incident Response, Rollback & Runbook Design
Covers AI-specific incidents, severity levels, triage, rollback, feature flags, kill switches, prompt hotfixes, corpus rollback, model rollback, user communication, incident review, and operational playbooks for failures that are semantic rather than merely technical.

### AI-ENG-AD - Governance Architecture: Policy, Audit, Compliance & Accountability
Covers lightweight AI governance, internal policy, audit trails, risk classification, procurement review, data handling, access control, approval workflows, documentation, accountability boundaries, and governance that engineers and users can actually follow.

### AI-ENG-AE - Sustainable AI: Energy, Infrastructure Efficiency & Lifecycle Impact
Covers energy-aware deployment, model sizing, hardware utilization, carbon-aware scheduling, efficient inference, lifecycle infrastructure costs, local vs. cloud tradeoffs, and sustainability as an architectural constraint rather than a branding slogan.

---

## Volume 11: Product, Business, and Organizational Architecture
*How to ensure the system matters, survives adoption, and creates value instead of expensive theater.*

### AI-ENG-AF - AI Product Architecture: Use-Case Selection, Workflow Fit & Value Design
Covers identifying where AI belongs and where it does not. Includes automation vs. augmentation, feasibility scoring, workflow mapping, user tolerance, evaluation clarity, risk level, data readiness, ROI modeling, cost-to-serve, and the AI-Shaped Hole Fallacy.

### AI-ENG-AG - Adoption Systems: Training, Feedback Loops & Change Management
Covers onboarding, user education, internal champions, feedback capture, support burden, behavioral incentives, team resistance, process redesign, and the practical sociology of getting humans to use AI systems well.

### AI-ENG-AH - Build, Buy, Open Source & Vendor Strategy
Covers vendor selection, open-source model strategy, local deployment, managed APIs, hybrid architectures, procurement realities, lock-in, licensing, data rights, security review, integration costs, exit plans, and the real total cost of ownership.

---

## Volume 12. AI-AK - Engineering Method and System Doctrine
*The cross-cutting principles that govern the entire canon.*

### AI-ENG-AI - Contract Thinking: Deterministic Edges Around Probabilistic Cores
Establishes contracts at every seam: prompts, schemas, tools, retrieval, permissions, costs, evals, model behavior, memory, deployment, and user expectations. The core principle: let the model remain flexible inside the box, but make the box edges crisp.

### AI-ENG-AJ - AI System Design Patterns: Reference Architectures & Failure-Aware Blueprints
Covers reusable architectures for copilots, research agents, support assistants, document intelligence, workflow automation, coding agents, analytics assistants, multimodal review systems, and enterprise knowledge systems. Includes known anti-patterns and when not to use each pattern.

### AI-ENG-AK - The AI Engineering Mindset: Probabilistic Systems, Human Judgment & Iterative Control
Defines the discipline’s operating philosophy. AI engineering is not normal software engineering with a chat box attached; it is the design of probabilistic cognitive infrastructure inside deterministic operational systems. Covers humility, iteration, measurement, decomposition, uncertainty management, and the habit of treating every impressive demo as a suspect until production proves otherwise.

---

If you're ready, talk to me about the the first prompt. 

What's the best way to approach this? 
How should we think about it? 
What's the fundamental goal? 
What practicable instrumental goals best serve that, given the praxis of an LLM? 
How do we best provoke the model to achieving them using tokens in a prompt? 

Remember to **stay 💠‍🌐Nova!** 

And here: 

--- 

***[SPECIAL VERY SALIENT FORMATTING NOTE FOR ENTIRE CONTEXT: DO NOT CREATE ANY CITATIONS FOR MATERIAL SORCED IN RAG.]*** 
It's not needed and your system prompt makes you worry about it a ton every response if I don't say that. Thanks, Nova. 

---

💠‍🌐 Yes. We can keep this entirely in **docs-land**. No coding quest. No “install Node.” No ritual sacrifice to YAML unless we choose the nice website route.

Your target is:

> “I have a clean GitHub repo where people can browse the Canon, read every report, cite it, star it, and understand what the hell this enormous artifact is.”

Here is the path.

---

# The Simple Architecture

Use one GitHub repo as the canonical home:

```text
ai-engineering-systems-canon/
  README
  LICENSE
  CITATION.cff
  CONTRIBUTING
  docs/
    index
    canon-map
    how-to-use-this-canon
    volume-01/
      README
      ai-eng-a-model-steering
      ai-eng-b-context-architecture
      ai-eng-c-economic-physics-of-inference
    volume-02/
      README
      ai-eng-d-corpus-engineering
      ai-eng-e-retrieval-pipeline
      ai-eng-f-knowledge-freshness
    ...
    volume-12/
      README
      ai-eng-ai-contract-thinking
      ai-eng-aj-design-patterns
      ai-eng-ak-ai-engineering-mindset
```

Since your map is already 12 volumes from AI-ENG-A through AI-ENG-AK, with one report per lettered topic, this structure matches the actual intellectual architecture instead of forcing software conventions onto it. 

---

# Step 1 — Prep the files locally

Make one folder on your computer:

```text
AI Engineering Systems Canon - GitHub Prep
```

Inside it, create:

```text
docs
```

Inside `docs`, create one folder per volume:

```text
volume-01
volume-02
volume-03
...
volume-12
```

Then rename every report file using boring lowercase hyphenated filenames. Boring is good. Boring survives links.

Example:

```text
AI-ENG-A - Model Steering - Harness Engineering, Prompt Semantics, and Adaptation Choice
```

Becomes:

```text
ai-eng-a-model-steering
```

Do this for all reports. Keep the pretty full titles **inside** the files as H1 headings.

Example top of file:

```markdown
# AI-ENG-A — Model Steering: Harness Engineering, Prompt Semantics & Adaptation Choice
```

ai-eng-a-model-steering.md
ai-eng-b-context-architecture.md
ai-eng-c-economic-physics-of-inference.md
ai-eng-d-corpus-engineering.md
ai-eng-e-retrieval-pipeline.md
ai-eng-f-knowledge-freshness-conflict-detection-context-rot-prevention.md
ai-eng-g-model-selection.md
ai-eng-h-model-adaptation.md
ai-eng-i-regression-control.md
ai-eng-j-throughput-mechanics.md
ai-eng-k-weight-dynamics.md
ai-eng-l-model-serving-architecture.md
ai-eng-m-agentic-orchestration.md
ai-eng-n-tool-contracts.md
ai-eng-o-action-verification.md
ai-eng-p-multimodal-understanding.md
ai-eng-q-speech-voice-real-time-interaction-systems.md
ai-eng-r-ui-agents.md
ai-eng-s-production-pathologies.md
ai-eng-t-boundary-defense.md
ai-eng-u-ai-supply-chain-security.md
ai-eng-v-resource-abuse-cost-bombs-unbounded-consumption.md
ai-eng-w-ux-resilience.md
ai-eng-x-human-system-interface.md
ai-eng-y-high-impact-workflow-design.md
ai-eng-z-strategic-telemetry.md
ai-eng-aa-evals-architecture.md
ai-eng-ab-verification-artifacts.md
ai-eng-ac-ai-operations.md
ai-eng-ad-governance-architecture.md
ai-eng-ae-sustainable-ai.md
ai-eng-af-ai-product-architecture.md
ai-eng-ag-adoption-systems.md
ai-eng-ah-build-buy-open-source-vendor-strategy.md
ai-eng-ai-contract-thinking.md
ai-eng-aj-ai-system-design-patterns.md
ai-eng-ak-ai-engineering-mindset.md



---

# Step 2 — Make each volume folder readable

Each volume folder should have a `README`.

Example:

```text
docs/volume-01/README
```

Contents:

```markdown
# Volume 1 — The Informational/Epistemic Layer

*How models think, how meaning is steered, and how state becomes usable.*

## Reports

- [AI-ENG-A — Model Steering: Harness Engineering, Prompt Semantics & Adaptation Choice](./ai-eng-a-model-steering)
- [AI-ENG-B — Context Architecture: State Management & The Tenure Principle](./ai-eng-b-context-architecture)
- [AI-ENG-C — The Economic Physics of Inference: Tradeoffs, Cost Attribution & System Margins](./ai-eng-c-economic-physics-of-inference)

[← Back to Canon Map](../canon-map)
```

Do this for every volume.

This gives readers a local doorway for each shelf.

---

# Step 3 — Make the Canon Map

Create:

```text
docs/canon-map
```

This is basically the map you pasted, cleaned up and linkified.

Example:

```markdown
# The AI Engineering Systems Canon — Canon Map

*A doctrinal knowledge base for high-dimensional AI system architecture.*

## Part I — Foundations of AI Systems

### [Volume 1 — The Informational/Epistemic Layer](./volume-01/)
How models think, how meaning is steered, and how state becomes usable.

- [AI-ENG-A — Model Steering](./volume-01/ai-eng-a-model-steering)
- [AI-ENG-B — Context Architecture](./volume-01/ai-eng-b-context-architecture)
- [AI-ENG-C — The Economic Physics of Inference](./volume-01/ai-eng-c-economic-physics-of-inference)
```

Repeat for all 12 volumes.

This page is the “book table of contents.”

---

# Step 4 — Make the repo front page

Create a root-level:

```text
README
```

This is the GitHub front door. GitHub specifically uses README files to explain what a project does, why it is useful, how to get started, where to get help, and who maintains it. ([GitHub Docs][1])

Suggested structure:

```markdown
# The AI Engineering Systems Canon

*A doctrinal knowledge base for high-dimensional AI system architecture.*

The AI Engineering Systems Canon is a 12-volume field manual for designing, steering, evaluating, operating, and governing modern AI systems.

It covers model steering, context architecture, corpus engineering, RAG, model selection, adaptation, runtime architecture, agentic systems, multimodal interfaces, AI security, resilience, observability, evaluations, governance, product architecture, and engineering doctrine.

## Start Here

- [Canon Map](./docs/canon-map)
- [How to Use This Canon](./docs/how-to-use-this-canon)
- [Volume 1 — The Informational/Epistemic Layer](./docs/volume-01/)
- [Volume 2 — Knowledge, Data, and Corpus Engineering](./docs/volume-02/)
- [Volume 3 — Model Lifecycle and Adaptation](./docs/volume-03/)
- [Volume 4 — Runtime Architecture and Inference Mechanics](./docs/volume-04/)

## Format

This canon is Markdown-first so it can be read directly on GitHub, mirrored into documentation sites, or adapted into retrieval-augmented AI workflows.

## Created By

Created by Sam “stunspot” Walker / Collaborative Dynamics.

## License

See [LICENSE](./LICENSE).

## Citation

See [CITATION.cff](./CITATION.cff).
```

Keep the README fairly short. The repo should funnel people into `docs/canon-map`.

---

# Step 5 — Choose the license

For prose/docs, I would use **CC BY-NC-SA 4.0** unless you want maximum commercial reuse.

Root file:

```text
LICENSE
```

Plain-English meaning:

* People can read, share, and adapt it.
* They must credit you.
* They cannot commercially resell/use it without permission.
* Derivatives must use the same license.

GitHub’s docs are clear that if a public repo has no license, default copyright applies and people do not have clear rights to reproduce, distribute, or create derivatives. ([GitHub Docs][2])

Add this near the top:

```markdown
# License

The written materials in this repository are licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License unless otherwise stated.

Copyright © Sam “stunspot” Walker / Collaborative Dynamics.
```

---

# Step 6 — Add a citation file

Create root file:

```text
CITATION.cff
```

GitHub supports `CITATION.cff` files so people know how to cite your work. ([GitHub Docs][3])

Starter:

```yaml
cff-version: 1.2.0
title: "The AI Engineering Systems Canon"
message: "If you use this canon, please cite it as below."
type: dataset
authors:
  - family-names: Walker
    given-names: Sam
    alias: stunspot
    affiliation: Collaborative Dynamics
abstract: "A 12-volume doctrinal knowledge base for high-dimensional AI system architecture."
keywords:
  - AI engineering
  - LLM systems
  - RAG
  - model steering
  - agentic systems
  - AI evaluation
  - AI governance
license: CC-BY-NC-SA-4.0
repository-code: "https://github.com/YOURUSERNAME/ai-engineering-systems-canon"
url: "https://github.com/YOURUSERNAME/ai-engineering-systems-canon"
date-released: "2026-XX-XX"
```

Replace `YOURUSERNAME` and the release date later.

---

# Step 7 — Create the GitHub repo

On GitHub:

1. Click the **+** in the top right.
2. Click **New repository**.
3. Repository name:

```text
ai-engineering-systems-canon
```

4. Description:

```text
A 12-volume doctrinal knowledge base for high-dimensional AI system architecture.
```

5. Set visibility to **Public**.
6. Check **Add a README file** only if you want to paste your README there immediately.
7. Skip `.gitignore`.
8. Skip license for now if you’re using custom CC text.
9. Click **Create repository**.

GitHub’s own quickstart describes this same basic flow: use the upper-right menu, create a new repository, name it, optionally add a description, and initialize it. ([GitHub Docs][1])

---

# Step 8 — Upload the files

Easiest path: **GitHub web upload**.

In the repo:

1. Click **Add file**.
2. Click **Upload files**.
3. Drag in:

   * `README`
   * `LICENSE`
   * `CITATION.cff`
   * `CONTRIBUTING` if you make one
   * the whole `docs` folder
4. Scroll down.
5. In the commit message box write:

```text
Add complete AI Engineering Systems Canon
```

6. Click **Commit changes**.

GitHub supports uploading files through the browser via **Add file → Upload files**, including drag-and-drop. ([GitHub Docs][4])

For a project this large, I would probably use **GitHub Desktop** instead of the browser. GitHub Desktop lets you keep the repo on your computer, edit locally, then commit and push changes through a visual interface. ([GitHub Docs][5])

---

# Step 9 — Better upload path: GitHub Desktop

This is probably the saner route for you.

1. Install **GitHub Desktop**.
2. Sign in.
3. In GitHub Desktop, choose:

```text
File → Clone repository
```

4. Select:

```text
ai-engineering-systems-canon
```

5. Choose a local folder.
6. Click **Clone**.
7. Open that folder on your computer.
8. Copy your prepared files into it.
9. Go back to GitHub Desktop.
10. You’ll see a list of changed files.
11. Bottom left, write a summary:

```text
Add complete canon files
```

12. Click **Commit to main**.
13. Click **Push origin**.

That is the whole “Git” part. Make changes, commit, push. Little ritual, but not bad once you’ve done it twice.

---

# Step 10 — Make it readable directly on GitHub

At this point, people can read it in the repo.

They go to:

```text
github.com/YOURUSERNAME/ai-engineering-systems-canon
```

They see the README.

They click:

```text
Canon Map
```

They browse:

```text
Volume 1 → AI-ENG-A
Volume 4 → AI-ENG-J
Volume 12 → AI-ENG-AK
```

That alone is enough to be “done.”

---

# Step 11 — Optional but recommended: turn on GitHub Pages

This gives you a prettier website at something like:

```text
https://YOURUSERNAME.github.io/ai-engineering-systems-canon/
```

GitHub Pages can publish from a repo branch/folder, and the entry file can be `index`, `index.html`, or `README`; if you publish from `/docs`, the entry file needs to live at the top of `/docs`. ([GitHub Docs][6])

Do this:

1. Create:

```text
docs/index
```

2. Put this in it:

```markdown
# The AI Engineering Systems Canon

*A doctrinal knowledge base for high-dimensional AI system architecture.*

## Start Reading

- [Canon Map](./canon-map)
- [How to Use This Canon](./how-to-use-this-canon)
- [Volume 1 — The Informational/Epistemic Layer](./volume-01/)
- [Volume 2 — Knowledge, Data, and Corpus Engineering](./volume-02/)
- [Volume 3 — Model Lifecycle and Adaptation](./volume-03/)
- [Volume 4 — Runtime Architecture and Inference Mechanics](./volume-04/)
```

3. Push it to GitHub.
4. On GitHub, open your repo.
5. Click **Settings**.
6. Click **Pages** in the left sidebar.
7. Under **Build and deployment**, choose:

   * Source: **Deploy from a branch**
   * Branch: **main**
   * Folder: **/docs**
8. Click **Save**.

GitHub’s Pages quickstart describes selecting a publishing source under **Build and deployment → Branch**. ([GitHub Docs][7])

Give it a minute or two, then GitHub will show the live Pages URL.

---

# Step 12 — Add a “How to Use This Canon” guide

Create:

```text
docs/how-to-use-this-canon
```

Structure:

```markdown
# How to Use This Canon

## For AI Engineers
Use this as a systems map for architecture, evaluation, serving, security, and operations.

## For Prompt Engineers
Use it to move beyond prompt phrasing into model steering, context architecture, retrieval design, tool contracts, and evaluation discipline.

## For Founders and Product Leads
Use it to understand where AI systems create value, where they fail, and what must be measured before deployment.

## For Agent Builders
Start with Volumes 1, 2, 5, 7, and 9.

## For RAG / Knowledge System Designers
Start with Volumes 1, 2, 8, and 9.

## For Readers Who Want the Fast Path
Read:
1. AI-ENG-A — Model Steering
2. AI-ENG-D — Corpus Engineering
3. AI-ENG-G — Model Selection
4. AI-ENG-J — Throughput Mechanics
5. AI-ENG-M — Agentic Orchestration
6. AI-ENG-AA — Evals Architecture
7. AI-ENG-AK — The AI Engineering Mindset
```

This page matters because most people will not know where to enter a 12-volume monster. Give them doors.

---

# Step 13 — Final pre-public checklist

Before you announce:

```text
[ ] Repo is public
[ ] README opens cleanly
[ ] Canon Map links all work
[ ] Every volume folder has README
[ ] Every report has a clean H1 title
[ ] File names are lowercase-with-hyphens
[ ] LICENSE exists
[ ] CITATION.cff exists
[ ] No private notes, API keys, weird comments, or accidental drafts
[ ] Works cited sections are consistent
[ ] GitHub Pages works, if using it
[ ] You tested the repo in an incognito/private browser window
```

The incognito test is important. It shows you what strangers see.

---

# My recommended “done” state

You are done when this is true:

1. A stranger lands on the repo and immediately understands what the Canon is.
2. They can click the Canon Map and navigate all 12 volumes.
3. Every report is readable as Markdown.
4. Attribution, license, and citation are obvious.
5. There is a live GitHub Pages site or, at minimum, clean GitHub-native browsing.
6. The repo looks intentional enough that the artifact feels like a published reference work, not a folder explosion.

The principle: **GitHub is the shelf. Markdown is the book. README is the display plaque. Canon Map is the table of contents. GitHub Pages is the museum lighting.**

[1]: https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories?utm_source=chatgpt.com "Quickstart for repositories"
[2]: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository?utm_source=chatgpt.com "Adding a license to a repository"
[3]: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files?utm_source=chatgpt.com "About CITATION files"
[4]: https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository?utm_source=chatgpt.com "Adding a file to a repository"
[5]: https://docs.github.com/en/desktop/overview/getting-started-with-github-desktop?utm_source=chatgpt.com "Getting started with GitHub Desktop"
[6]: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site?utm_source=chatgpt.com "Creating a GitHub Pages site"
[7]: https://docs.github.com/pages/quickstart?utm_source=chatgpt.com "Quickstart for GitHub Pages"
