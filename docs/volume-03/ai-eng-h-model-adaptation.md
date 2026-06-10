# AI-ENG-H — Model Adaptation - Fine-Tuning, LoRA, Preference Tuning & Distillation

Model adaptation represents the most high-leverage and mathematically invasive layer of the model steering stack. While prompt engineering, in-context examples, context construction, retrieval-augmented generation (RAG), tool interfaces, and output validation enforce constraints at inference time, adaptation alters the underlying parameters or probability distributions of the neural network.1 In accordance with the steering doctrine established in the engineering canon, adaptation must be treated as a diagnosed behavioral intervention rather than a default capability patch.  
The governing engineering doctrine dictates: **Adapt behavior when the desired behavior is stable, repeated, measurable, and poorly served by lighter steering layers; keep volatile knowledge, permissions, live state, and source-grounded facts outside the weights.**  
Lighter steering layers operate on the activation space of the model, steering its attention over dynamic context. When the target behavior is highly volatile—such as pricing sheets, inventory counts, user permissions, or live news—encoding this information into parameters through model training guarantees rapid factual obsolescence, weight rot, and security leakage across tenant boundaries.3 Conversely, when the target behavior is structurally stable, highly repeated, and quantifiable—such as generating domain-specific schemas, mapping standardized tool parameters, enforcing syntactic or stylistic invariants, or executing complex multi-step logical operations—adaptation becomes the most computationally efficient and robust intervention.5  
The critical architectural boundary lies between knowledge and behavior. Volatile, factual, and permission-sensitive facts belong in context compilation, dynamic retrieval pipelines, or real-time tool lookups. Stable, procedural, stylistic, and formatted behaviors belong in the weights. Attempting to solve knowledge gaps with weight modification leads to memorization pathologies, catastrophic forgetting, and severe out-of-domain regression.4 Conversely, attempting to solve behavioral, stylistic, or formatting gaps solely with prompts bloats the token context window, degrades inference latency, increases cost-per-success, and introduces high-variance failures at the production edge.5

## **Conceptual Glossary**

To establish a structurally durable nomenclature for the model adaptation lifecycle, the following terms are mathematically and operationally defined:

| Term | Formal Representation / Mechanism | Operational Definition | System Impact |
| :---- | :---- | :---- | :---- |
| **Model Adaptation** | f\_theta \-\> f\_(theta \+ delta\_theta) | The disciplined modification, specialization, alignment, or compression of model behavior through optimization algorithms that alter parameter states or output logit distributions to satisfy a stable task envelope.7 | Shuts down runtime variance, reduces prompt overhead, and locks in domain formatting.5 |
| **Supervised Fine-Tuning (SFT)** | L\_SFT(theta) \= \-sum log P\_theta(y\_t | x, y\_\<t) | Updating all model weights by computing gradients over a labeled corpus of prompt-response pairs using a cross-entropy loss function.10 |
| **Instruction Tuning** | Formatted as explicit instruction-response structures | A specialized form of SFT where the training data is structured as instructions and multi-turn dialogues, shifting the model from next-token completion to conversational task-following behavior.4 | Restructures attention allocation; makes the model highly receptive to zero-shot task framing. |
| **Low-Rank Adaptation (LoRA)** | W \= W\_0 \+ (alpha / r) \* B \* A | A parameter-efficient fine-tuning technique that freezes pre-trained weights W\_0 in R^(d x k) and injects trainable low-rank matrices A in R^(r x k) and B in R^(d x r) where r \<\< min(d, k).1 | Decreases GPU training memory footprint by up to 3x; allows dynamic, zero-latency adapter swapping at inference.1 |
| **Quantized Low-Rank Adaptation (QLoRA)** | Quantization of base weights to NF4 with double quantization | An adaptation of LoRA where the base model is quantized to a specialized low-bit representation (typically 4-bit NormalFloat) while the adapter matrices remain in high precision.16 | Lowers VRAM requirements of fine-tuning by up to 4x, enabling training of large models on commodity hardware with minimal precision loss.17 |
| **Adapter** | Modular parameter deltas delta\_W \= B \* A | A modular, lightweight set of trainable parameters that can be dynamically loaded, swapped, or merged into a frozen base model at runtime to alter its behavior.8 | Decouples base capability from specialized domain execution, simplifying multi-tenant serving.8 |
| **Adapter Merge** | W\_merged \= W\_0 \+ delta\_W | The physical consolidation of adapter weights directly into the base model weights, eliminating runtime inference latency.6 | Removes memory lookup overhead, but permanently fuses behavior and destroys adapter modularity.6 |
| **Preference Tuning** | Policy optimization based on a relative preference signal | Programmatic alignment of model outputs with comparative utility signals, optimizing the policy to favor preferred completions over dispreferred ones.20 | Controls brand safety, shapes conversational tone, and mitigates toxic or adversarial completions.2 |
| **Reinforcement Learning from Human Feedback (RLHF)** | Actor-Critic optimization against a learned reward r\_phi(x, y) | An online alignment framework that trains a scalar reward model on human preferences and optimizes the policy using reinforcement learning (e.g., PPO) with a KL penalty.2 | High capability for long-tail alignment, but presents high training instability and massive infrastructure overhead.2 |
| **Direct Preference Optimization (DPO)** | Sigmoid-wrapped log-ratio objective | An offline alignment method that parameterizes the reward function directly through the policy's log-probabilities, bypassing explicit reward modeling.2 | Simplifies preference optimization to a supervised loss; prone to over-fitting and logit saturation.22 |
| **Identity Preference Optimization (IPO)** | Margin-regularized quadratic preference loss | An offline preference optimization variant that adds a quadratic regularization term to the DPO loss.22 | Prevents premature logit saturation and reduces model overfitting to noisy preference labels without early stopping.20 |
| **Kahneman-Tversky Optimization (KTO)** | Prospect-theory utility maximization | An alignment loss that optimizes a policy using independent binary desirability labels (good vs. bad) rather than paired preferences, modeling human loss aversion asymmetrically.2 | Operates on abundant, unpaired telemetry data (thumbs-up / thumbs-down); handles extreme dataset imbalances exceptionally well.2 |
| **Domain Adaptation** | Continual Pre-Training (CPT) on raw corpus | Shifting a model's linguistic, semantic, and structural representations toward a specialized industry or technical domain.4 | Deepens domain-specific conceptual understanding; carries extreme risks of catastrophic forgetting.7 |
| **Synthetic Data** | Programmatic or teacher-generated text sequences | Labeled text or structured structures generated by foundational frontier models, simulators, or rule-based templates used to bootstrap training corpora.5 | Solves cold-start data scarcity; risks introducing teacher errors and synthetic monoculture.5 |
| **Distillation** | Student matches softened teacher output distributions | The compression of behavioral, logical, or structural capabilities from a highly parameterized teacher model into a compact student model.9 | Dramatically improves system margins, delivering up to 30x cost reduction and 4x faster inference times.9 |
| **Teacher Model** | High-capacity model (e.g., 70B+ parameters) | A large, highly capable model used to generate high-fidelity targets, reasoning traces, or soft logit distributions for student training.5 | Serves as the golden source of capability, but represents a significant compute bottleneck during generation.5 |
| **Student Model** | Highly compact model (e.g., 1B to 8B parameters) | A highly compact model optimized via supervised learning or distillation objectives to replicate the teacher's task performance.5 | Delivers massive deployment efficiency, low memory footprint, and edge-run capability.5 |
| **Catastrophic Forgetting** | Weight shifts overwrite base model distributions | The rapid degradation of a model's pre-trained general capabilities, factual knowledge, or reasoning skills during sequential fine-tuning.13 | Degrades downstream multi-task utility, rendering the model useless for out-of-domain tasks.3 |
| **Style Overfit** | Over-optimization of length and formatting parameters | A training pathology where a model learns superficial structural features or length distributions of a training set instead of semantic patterns.4 | Degrades generation quality, causing the model to output empty verbosity or copy instruction formats blindly.23 |
| **Training-Data Contamination** | Overlap between training data and evaluation sets | The accidental or intentional inclusion of evaluation benchmark sets within the pre-training or fine-tuning corpus.26 | Artificially inflates evaluation metrics, destroying the diagnostic validity of standard benchmarks.26 |
| **Adaptation Artifact** | Bundled configuration, weights, and lineage data | The complete, version-controlled package resulting from an adaptation run, including weights, configs, tokenizer changes, and eval status.18 | Ensures reproducible rollouts, safe serving, and auditable governance paths across the model lifecycle.18 |

## **The Adaptation Decision Ladder**

A disciplined engineering organization does not default to model training. Before introducing the overhead of weight optimization, practitioners must traverse the Adaptation Decision Ladder. This framework ensures that lighter, more reversible, and cheaper interventions are systematically exhausted before allocating compute resources to parameter modification.

| Step | Intervention | Behavior Change Depth | Reversibility | Iteration Speed | Data Burden | Cost Profile | Latency Impact | Auditability | Deployment Complexity | Regression Risk |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | **Prompt Revision** | Superficial | Instantaneous | Minutes | None | Negligible | O(1) increase | High | Zero | Negligible |
| 2 | **Examples (Few-Shot)** | Contextual | Instantaneous | Minutes | 3-10 exemplars | Very Low | Linear with prompt tokens | High | Zero | Low |
| 3 | **Harness Changes** | Boundary-level | Instantaneous | Hours | None | Low | Minor overhead | High | Low | Low |
| 4 | **Structured Outputs** | Syntactic | High (Deterministic) | Hours | JSON Schema | Low | Minimal (constrained sampling) | High | Low | Low |
| 5 | **Validators** | Execution-level | High (Hard stop) | Hours | Code schemas | Low | Millisecond overhead | High | Low | Low |
| 6 | **Retrieval-Augmented Gen.** | Knowledge-level | Decoupled | Days | Unstructured docs | Moderate (index build) | Linear with retrieved chunks | Extremely High | Moderate (vector DB) | Low (No weight drift) |
| 7 | **Memory** | User State Tenure | Decoupled | Days | Structured memory logs | Moderate | Linear with memory context | High | Moderate | Low |
| 8 | **Tools** | Action-level | Decoupled | Days | API definitions | Moderate | Multi-step invocation | High | High (agent framework) | Low |
| 9 | **Routing** | Semantic Redirection | Decoupled | Days | Classification heuristics | Low | Millisecond routing check | High | Moderate | Low |
| 10 | **Model Selection** | Capability-level | High | Days | Evaluation sets | Moderate | Variable (swap-dependent) | High | High (router node) | Low |
| 11 | **Supervised Fine-Tuning** | Deep Parameter | Irreversible | Weeks | 10^3 \- 10^5 pairs | High | Zero | Low | High (Discrete artifact) | High |
| 12 | **LoRA / QLoRA** | Weight-level | High (unloadable) | Days to Weeks | 10^2 \- 10^4 pairs | Moderate | Near-zero (with Punica/SGMV) | Moderate | High (Multi-adapter server) | Moderate |
| 13 | **Preference Tuning** | Logit Alignment | Irreversible | Weeks to Months | 10^3 \- 10^4 pairwise tags | High | Zero | Low | High | Extremely High |
| 14 | **Distillation** | Architectural | Irreversible | Months | 10^5 \- 10^6 teacher outputs | Extremely High (initially) | Massive Reduction (5-30x savings) | Low | High (New student artifact) | High |
| 15 | **No Adaptation** | None | N/A | N/A | None | Zero | None | N/A | Zero | None |

## **Behavior Gap Diagnostic**

To determine the appropriate entry point on the Adaptation Decision Ladder, the system architect must perform a structural root-cause analysis of the system failure.

| Failure Category | Recommended Intervention | Root-Cause Mechanism | Justification & Operational Guardrails |
| :---- | :---- | :---- | :---- |
| **Prompt/Harness Gap** | Restructure prompt schemas, configure system prompt boundaries, or down-select to a more capable foundation model. | The base model's pre-trained attention context or instruction-following limits are exceeded by the target task complexity.4 | Bloated prompts increase time-to-first-token (TTFT) and degrade system margins; verification of prompt boundaries must precede training. |
| **Context Gap** | Implement contextual summarization pipelines, context pruning, or dynamic system context adjustment.4 | The model lacks runtime access to high-density user, project, or system configuration states.4 | Context size behaves as a volatile runtime memory window; injecting data dynamically minimizes representation drift.4 |
| **Retrieval Gap** | Expand vector database coverage, integrate hybrid semantic-keyword indexing, or configure query re-writing loops. | The target task demands accurate recall of volatile domain entities or historical documents completely absent from pre-training.4 | Keeping factual content out of weights prevents factual obsolescence, reduces hallucination rates, and preserves model utility.3 |
| **Tool Gap** | Map runtime API specifications dynamically into the model's tool call slots; implement programmatic parameter checking. | The model is forced to perform calculations, transaction state transitions, or database reads via raw autoregressive generation. | Weights cannot handle real-time calculations or transactions; exposing secure sandboxed APIs enforces deterministic execution boundaries. |
| **Schema Gap** | Integrate structured output engines (e.g., Outlines, Instructor) to constrain sampling token-by-token.18 | Autoregressive decoding lacks native state-transition constraints, causing minor sampling errors to break syntax formats.17 | Constrained decoding via logit masking guarantees syntactical validity deterministically without the expense of model training.17 |
| **Model Selection Gap** | Execute comprehensive zero-shot benchmarks; replace current model with a higher-parameter foundation checkpoint.14 | The core model's parameter capacity is fundamentally insufficient for the reasoning or semantic depth of the task. | Upgrading the base checkpoint establishes a stronger foundation of reasoning, directly improving downstream task performance.14 |
| **Evaluation Gap** | Build automated multi-dimensional eval suites (e.g., LLM-as-a-Judge, unit assertion tests) with high-confidence rubrics.5 | The lack of a high-fidelity evaluation framework makes it impossible to distinguish genuine capability decay from random generation noise. | Attempting model training without a stable, automated evaluation baseline is a critical failure that guarantees regression. |
| **Product/Workflow Gap** | Re-engineer the application logic; decompose the single complex query into a deterministic multi-step agent chain. | The application attempts to solve a complex, multi-variable business workflow with a single, massive autoregressive call. | Decomposing workflows into isolated, deterministic state machines reduces the capability burden on the underlying language model. |
| **Adaptation Gap** | Execute Supervised Fine-Tuning or LoRA training over a curated, verified dataset of input-output pairs.1 | The target behavior requires deeply specialized formatting, custom syntax structures, or unique styles absent from pre-training. | Training modifies the model's behavioral heuristics, allowing it to execute deep structural mappings with minimal context bloat.7 |

## **Adaptation Method Matrix**

When adaptation is diagnosed as the required intervention, the choice of method must balance task-specific capability requirements against infrastructure and operational constraints.

| Method | Ideal Use Case | Required Data | Training Complexity | Infrastructure Burden | Behavioral Depth | Failure Risk | Evaluation Requirements |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Supervised Fine-Tuning** | Mastering domain-specific styles, specialized classifications, or highly repetitive output structural patterns.5 | 10^3 \- 10^5 highly curated prompt-response pairs with perfect formatting.5 | Moderate (standard cross-entropy optimization).10 | High (requires full parameter gradient tracking and optimizer memory).7 | Deep structural behavior shift; alters core attention weight profiles. | High (susceptible to catastrophic forgetting and style overfit).13 | Comprehensive held-out task evals, general reasoning tests, and safety checks.4 |
| **Instruction Tuning** | Aligning raw base models to conversational dialog patterns and multi-turn instruction following.4 | 10^4 \- 10^5 diverse instruction-response sequences across multiple tasks.7 | Moderate to High (requires careful masking of instruction tokens). | High (requires multi-node distributed training for parameter scale). | Moderate to High (restructures conversational attention bounds). | Moderate (risks collapsing zero-shot out-of-domain performance).4 | Dialogue adherence, constraint checking, and general benchmark tracking.4 |
| **LoRA** | Specialized task adaptation under strict hardware, VRAM, or multi-tenant serving constraints.1 | 10^2 \- 10^4 target task examples; highly efficient in low-data regimes.7 | Low to Moderate (hyperparameter tuning of r and alpha is highly sensitive).7 | Low (base weights frozen; only low-rank matrices are optimized).1 | Moderate (constrained to the low-rank update subspace).7 | Low to Moderate (acts as a strong regularizer, minimizing forgetting).7 | Held-out target evaluations, prompt template dependency validation.18 |
| **QLoRA** | Specialized training of highly parameterized models on constrained, commodity single-GPU hardware.16 | 10^2 \- 10^4 target task examples; maps identically to LoRA data footprints. | Moderate (requires fused quantization-dequantization pipelines).16 | Extremely Low (quantizes base model to 4-bit NF4 with double quantization).16 | Moderate (identical behavioral footprint to standard LoRA updates). | Low to Moderate (minor precision losses during on-the-fly dequantization).16 | Identical to LoRA; requires runtime latency tracking.16 |
| **Preference Tuning** | Brand alignment, tone shaping, safety guardrail enforcement, and conversational quality tuning.20 | 10^3 \- 10^4 pairwise chosen/rejected records or pointwise utility labels.2 | Extremely High (susceptible to gradient explosion, policy collapse).12 | Moderate to High (requires reference model tracking and policy rollouts).2 | Superficial logit alignment; modifies probability boundaries.2 | High (over-optimization, reward hacking, loss of reasoning).11 | Held-out preference evaluations, adversarial safety red-teaming.4 |
| **Domain Adaptation** | Infusing specialized language styles, acronyms, or deep reasoning patterns into pre-training.4 | 10^8 \- 10^10 raw, domain-specific text tokens (Continued Pre-Training).7 | High (requires custom learning rate schedules and packing algorithms). | Massive (requires high-throughput multi-GPU pre-training clusters). | Deep representation shift; reshapes the core latent semantic space. | Extremely High (erodes general world knowledge and reasoning).7 | Standard pre-training benchmarks, general reasoning, domain-specific cloze tests.14 |
| **Synthetic Data Gen.** | Expanding training coverage for low-frequency edge cases or bootstrapping data-scarce domains.5 | Highly capable teacher seed prompts; template-driven simulation models.5 | Low to Moderate (computational complexity is concentrated at inference). | Low (requires inference execution infrastructure for the teacher model). | N/A (this is a data curation and synthesis method). | High (risks introducing teacher errors and synthetic monoculture).5 | Diversity metrics, hallucination audits, compiler/sandbox verification.5 |
| **Distillation** | Compressing expensive frontier model capabilities into compact, low-latency deployment artifacts.5 | 10^5 \- 10^6 high-fidelity teacher completions, soft logits, or reasoning traces.9 | High (requires joint student-teacher validation and logit-matching code). | High (requires hosting both student and teacher during data generation).32 | Deep architectural compression; alters parameter efficiency.9 | High (student parameter limits may trigger a capacity gap collapse).32 | Massive regression suites, comparative student-teacher performance tracking.5 |

### **SFT vs. Parameter-Efficient Adaptation (LoRA)**

Full parameter fine-tuning (FFT) updates all parameters of the network by calculating gradients over a target dataset D \= {(x\_i, y\_i)} using the standard auto-regressive cross-entropy loss:  
L\_SFT(theta) \= \-sum\_(i=1 to |D|) sum\_(t=1 to |y\_i|) log P\_theta(y\_i,t | x\_i, y\_i,\<t)  
SFT is highly effective for establishing target styles, domain formatting, and structured task patterns. However, full parameter fine-tuning requires keeping optimizer states (e.g., AdamW's first and second moments) in GPU memory, which consumes roughly six times the memory of the model weights alone.12  
To bypass this memory wall, Low-Rank Adaptation (LoRA) freezes the pre-trained weight matrix W\_0 in R^(d\_out x d\_in) and represents the weight update delta\_W through a low-rank decomposition 1:  
W\_adapted \= W\_0 \+ delta\_W \= W\_0 \+ (alpha / r) \* B \* A  
where B in R^(d\_out x r) and A in R^(r x d\_in) are trainable parameters, r \<\< min(d\_in, d\_out) is the rank hyperparameter, and alpha is a constant scaling factor that stabilizes training when adjusting r.1 The matrix A is typically initialized from a random Gaussian distribution N(0, sigma^2), and B is initialized to zero, ensuring that delta\_W \= 0 at the onset of training, which preserves the model's pre-trained behavior.1  
To optimize memory efficiency further, Quantized Low-Rank Adaptation (QLoRA) introduces three core architectural innovations 16:

1. **NormalFloat 4 (NF4)**: An information-theoretically optimal 4-bit quantization scheme engineered specifically for normally distributed parameters.34 The 16 quantization bins are determined by finding the equiprobable quantiles of the standard normal distribution N(0, 1\) 34: q\_i \= 1/2 \* \[ Q\_x(i / 17\) \+ Q\_x((i \+ 1\) / 17\) \], for i in {0,..., 15} where Q\_x(...) is the quantile function of N(0, 1), and the codebook is rescaled to exactly represent zero.34  
2. **Double Quantization (DQ)**: The process of quantizing the quantization constants (scales) themselves. QLoRA quantizes 32-bit floating-point scales per block of 64 weights into 8-bit floats with a block size of 256, reducing the memory footprint from 32 bits per block to roughly 8.12 bits per block.16  
3. **Paged Optimizers**: The dynamic offloading of optimizer states to CPU memory during memory spikes, preventing out-of-memory errors on high-batch training steps.

Empirical research establishes a critical performance-regularization trade-off between full fine-tuning (FFT) and LoRA.

* **Learning Capacity**: Full fine-tuning learns perturbations with an intrinsic rank that is 10x to 100x greater than standard LoRA configurations (r in {8, 16, 32}).7 Consequently, for complex continued pre-training (CPT) on highly technical domains like mathematics or computer programming, LoRA significantly underperforms full fine-tuning because the low-rank constraint restricts the model's ability to absorb novel linguistic structures and complex representation shifts.7  
* **Instruction Following**: In standard instruction fine-tuning (IFT) regimes (e.g., 10^5 samples), the performance gap between LoRA and FFT can be largely closed by scaling the rank r to higher dimensions (e.g., r \>= 64, alpha \= 128\) and targeting all linear projections in the transformer architecture—including attention projections (W\_q, W\_k, W\_v, W\_o) and feedforward block projections (W\_gate, W\_up, W\_down).1  
* **Generalization and Forgetting**: LoRA acts as a powerful regularizer. Because the base model weights W\_0 remain frozen, LoRA prevents the model's pre-trained representation coordinates from collapsing.7 LoRA significantly mitigates catastrophic forgetting of out-of-domain knowledge compared to full fine-tuning and maintains a higher degree of generation diversity, preventing the token distribution from collapsing into a narrow subset of solutions.7  
* **Hyperparameter Sensitivity**: LoRA is highly sensitive to training hyperparameters. The critical parameters are, in order of decreasing sensitivity: Learning Rate \-\> Target Modules \-\> Rank (r) and Alpha (alpha).7

## **Preference Tuning Architectures**

Preference tuning shifts the optimization target from imitating a target corpus to maximizing a comparative utility metric. Rather than asking "what token is most likely to follow?", preference optimization asks "which candidate completion is structurally, semantically, or safety-wise preferred?".20 This phase is critical for alignment, style control, toxicity reduction, and complex multi-turn conversational patterns.11  
Standard preference tuning assumes a dataset of prompt-response pairs D \= {(x, y\_w, y\_l)}, where y\_w represents the preferred (chosen) response and y\_l represents the dispreferred (rejected) response.2 Traditionally, this was executed via Reinforcement Learning from Human Feedback (RLHF), which requires:

1. Training a binary classification Reward Model r\_phi(x, y) on pairwise preferences using the Bradley-Terry objective 2: L\_R(r\_phi) \= \-E\_(x, y\_w, y\_l) \~ D \[ log sigma( r\_phi(x, y\_w) \- r\_phi(x, y\_l) ) \]  
2. Optimizing the active policy pi\_theta(y | x) against the frozen reward model r\_phi using Actor-Critic PPO, while applying a Kullback-Leibler (KL) divergence penalty against a frozen reference policy pi\_ref to prevent policy collapse 2: Objective(theta) \= E\_(x \~ D, y \~ pi\_theta) \[ r\_phi(x, y) \] \- beta \* DKL( pi\_theta(y | x) |

| pi\_ref(y | x) )  
PPO requires maintaining four distinct models in memory (Actor pi\_theta, Critic, Reward r\_phi, Reference pi\_ref), leading to high training instability, complex hyperparameter tuning, and massive GPU infrastructure requirements.2  
To eliminate this complexity, Direct Preference Optimization (DPO) mathematically reparameterizes the Bradley-Terry reward function directly in terms of the policy's log-probabilities, bypassing the reward model and reinforcement learning loop entirely.2 The DPO loss is defined as 2:  
L\_DPO(pi\_theta; pi\_ref) \= \-E\_(x, y\_w, y\_l) \~ D \[ log sigma( beta \* log(pi\_theta(y\_w | x) / pi\_ref(y\_w | x)) \- beta \* log(pi\_theta(y\_l | x) / pi\_ref(y\_l | x)) ) \]  
where beta controls the strength of the implicit KL penalty.2  
While computationally elegant, DPO exhibits distinct empirical vulnerabilities:

1. **Likelihood Inflation and Overfitting**: DPO maximizes the ratio of the chosen completion likelihood to the rejected completion likelihood.23 If left unregulated, the sigmoid loss function saturates quickly. Once the margin is satisfied, the gradient vanishes, allowing the policy to overfit by driving the absolute log-probabilities of both completions down or memorizing superficial patterns.22  
2. **Degradation of Reasoning**: Standard preference optimization pipelines frequently cause significant regressions on reasoning-heavy, mathematical, or code-generation tasks.11 This occurs because preference loss structures treat the output sequence holistically, punishing step-by-step correct rationalizations if the final token diverges slightly, whereas token-level credit assignment (such as GAE in PPO) is required for rigorous reasoning chains.11

To address DPO's limitations, alternative frameworks have emerged:

### **Identity Preference Optimization (IPO)**

IPO introduces a quadratic regularization term directly over the implicit reward margin to prevent the policy from saturating and over-optimizing to noisy preference labels.22 The loss is formulated as 22:  
L\_IPO(pi\_theta; pi\_ref) \= E\_(x, y\_w, y\_l) \~ D \[ ( beta \* log(pi\_theta(y\_w | x) / pi\_ref(y\_w | x)) \- beta \* log(pi\_theta(y\_l | x) / pi\_ref(y\_l | x)) \- 1 / (2 \* beta) )^2 \]  
By replacing the log-sigmoid objective with a squared error deviation from a target margin c \= 1 / (2 \* beta), IPO prevents logit saturation, regularizes the policy throughout training, and enables convergence without the need for early stopping.20

### **Kahneman-Tversky Optimization (KTO)**

KTO bypasses the requirement for paired preference data.20 Based on the asymmetrical value function of Kahneman & Tversky's prospect theory, KTO posits that humans perceive utility relative to a reference point and are significantly more averse to losses (dispreferred outcomes) than they are pleased by equivalent gains (preferred outcomes).2 KTO operates on independent binary annotations: whether a completion is desirable (y \~ desirable) or undesirable (y \~ undesirable) for a given prompt.2  
The KTO loss function is formulated as 2:  
L\_KTO(pi\_theta; pi\_ref) \= E\_(x, y \~ D) \[ w(y) \* ( 1 \- sigma( tau(x, y; beta) ) ) \]  
where tau(x, y; beta) \= beta \* log(pi\_theta(y | x) / pi\_ref(y | x)) \- z(x; beta) represents the margin-adjusted implicit reward, z(x; beta) is a dynamic reference point tracking the expected reward of the current policy, and w(y) is the asymmetrical weighting factor 2:  
w(y) \= lambda\_D if y is desirable, or lambda\_U if y is undesirable  
To model human loss aversion, the penalty weight for undesirable outcomes is set higher than the reward weight for desirable outcomes (typically lambda\_D \= 1.0 and lambda\_U \= 1.33).37 KTO matches or exceeds DPO performance on task capabilities, excels in handling highly imbalanced datasets, and collects data directly from user telemetry (e.g., thumbs-up / thumbs-down logs).2

## **Training Dataset Design Framework**

A model adaptation run is only as robust as the corpus that defines it. Treating training data as an unstructured folder of examples is a severe systemic failure. A production training dataset must be engineered as a structured, governed database with precise taxonomic diversity and quality controls.

| Domain Component | Source Selection Protocols | Example Coverage Targets | Label Quality & Calibration Controls | Provenance & Redaction Rules | Known Blind Spots |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Specialized Domain Task (Positive Exemplars)** | Curate from production logs with verified successful execution; incorporate expert-generated golden responses.5 | **80% of total volume.** Target precise formatting, syntactic schemas, and stylistic invariants.5 | Establish explicit annotation guidelines; enforce rater calibration loops using Cohen's and Fleiss' Kappa.38 | Enforce strict PII redaction; verify commercial compliance of source code/documents; log data lineage.18 | High risk of style overfit; can trigger a collapse of out-of-domain logical reasoning.4 |
| **Constraint Boundary (Abstention & Refusals)** | Synthesize or hand-write precise edge-case prompts; extract boundary violations from red-teaming logs.21 | **10% of total volume.** Explicitly define triggers where the model must ask for clarification or refuse.4 | Expert adjudication of edge cases; annotate clear reasons for refusal to prevent arbitrary stonewalling. | Ensure compliance with geographic and legislative safety mandates; catalog and log all forced refusal types. | Hyper-conservatism; model may begin refusing benign, safe requests that share surface keywords.4 |
| **Negative Contrasts (Error Corrections)** | Collect common user correction logs, historical system failures, and adversarial red-team outputs.4 | **10% of total volume.** Map malformed inputs directly to correct outcomes alongside explicit negative examples.4 | Double-blind review of correction labels; calculate inter-rater consensus on what constitutes a severe failure. | Remove any trace of user-specific operational telemetry; anonymize system host names, IPs, and private schemas. | Over-sensitizing the model to negative frames; risks increasing confusion on standard instructions. |

### **Inter-Rater Agreement Math**

To ensure training data label consistency prior to optimization, the annotation pipeline must enforce mathematical calibration steps:

* **Cohen's Kappa (kappa)**: Used to evaluate the degree of agreement between exactly two fixed raters assigning categorical labels 38: kappa \= (P\_0 \- P\_e) / (1 \- P\_e) where P\_0 represents the observed proportion of agreement between the raters, and P\_e is the expected proportion of agreement due to random chance based on marginal frequencies.38  
* **Fleiss' Kappa (kappa)**: A generalization of Cohen's Kappa used when three or more fixed raters (m \>= 3\) assign categorical labels to a set of items, allowing for random rater sampling per item 38: kappa \= (bar\_P \- bar\_P\_e) / (1 \- bar\_P\_e) where bar\_P represents the mean of the rater agreement proportions calculated subject-by-subject, and bar\_P\_e represents the expected agreement if all raters made completely random assignments.39

A strict engineering protocol dictates that training data collection must be halted and annotation rubrics recalibrated if the inter-rater agreement score falls below kappa \= 0.70.38

## **Synthetic Data Governance Model**

When organic training data is sparse, synthetic data generation is a valid strategy to bootstrap datasets. However, naive synthetic generation pipelines run the risk of model collapse and representation decay.5

| Governance Layer | Operational Protocol | Filtering & Verification Controls | Diversity & Entropic Controls | Bias & Noise Mitigations | Disclosure & Provenance Rules |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Generation (Teacher Selection)** | Select frontier models (e.g., Llama 3.3 70B, Qwen3 235B).5 Use temperature perturbation (T \>= 1.0).5 | Filter out standard teacher prefix phrases (e.g., "Certainly," "As an AI language model").23 | Implement nucleated sampling (p \= 0.95) to prevent structural token collapse and repetitive phrasing. | De-duplicate completions via MinHash LSH; enforce minimum semantic distance thresholds.5 | Append metadata tags documenting the generating model name, API version, and temperature config.18 |
| **Verification (Compiler/Sandbox)** | Route generated code, SQL, or structured data directly through isolated, ephemeral sandboxed compilers.5 | Reject any completions that fail compilation, runtime execution, unit tests, or static analysis.5 | Track execution path branch coverage; enforce diverse algorithm structures in coding tasks. | Automatically strip compile-time warnings, debug logs, and path traces from the final dataset. | Log exact sandbox execution logs, test harness versions, and compiler configuration states. |
| **Evaluation (LLM-as-a-Judge)** | Pass synthetic generations to a separate, frozen judge model running multi-dimensional rubrics.5 | Reject completions scoring below 4.0/5.0 on logical consistency, relevance, and formatting accuracy. | Prompt judges to explicitly penalize stylistic verbosity and repetitive, formulaic transitions. | Run dual-judge voting; flag and adjudicate instances where judges exhibit high rating divergence. | Store the judge model's full evaluation rationale and scoring breakdown alongside the training item. |

## **Adapter and Artifact Management**

In multi-tenant enterprise architectures, training a discrete foundation model checkpoint per customer or task is economically prohibitive and introduces severe cold-start latencies. Parameter-efficient multi-adapter serving decouples base model inference from specialization layers, allowing a single high-capacity base model instance in GPU VRAM to dynamically run hundreds of specialized task adapters.8

### **S-LoRA and Unified Paging**

Traditional adapter serving systems allocate static GPU memory pools for adapter parameters and key-value (KV) caches, resulting in extreme memory fragmentation and low GPU utilization.42 S-LoRA introduces **Unified Paging**, which manages both the KV cache tensors and the variable-rank adapter weights (A, B matrices) in a single unified memory pool using a paged virtual memory architecture.43  
Both adapter parameters and KV cache sequences are broken down into fixed-size pages and stored in non-contiguous physical memory pages on the GPU.43 This allows S-LoRA to dynamically load, swap, and execute thousands of specialized adapters on a single base model instance.43 S-LoRA stores all idle adapters in host system memory (DRAM) and dynamically pages active adapter weights into GPU High Bandwidth Memory (HBM) on a token-by-token basis based on incoming request routing.6

### **Punica and the SGMV Kernel**

When a batch of requests arrives at a multi-adapter server, each request may target a different adapter with a unique rank r and sequence length T.43 Executing these computations using standard Basic Linear Algebra Subprograms (BLAS) libraries would require padding the activation tensors to match the maximum rank and sequence length in the batch, causing severe computational waste and low GPU tensor core utilization.6  
Punica resolves this by introducing the **Segmented Gather Matrix-Vector Multiplication (SGMV)** kernel.45 For a batch of heterogeneous decoding requests, the server executes the computationally heavy base model forward pass X \* W\_0 in a single batched General Matrix Multiply (GEMM) operation.6 Then, the custom SGMV CUDA kernel executes the low-rank adapter computations on-the-fly 6:  
Y \= X \* W\_0 \+ SGMV(X, A, B, s)  
where s is a segment vector defining which rows of the batched activation tensor X map to which target adapter parameters.45  
SGMV gathers non-contiguous adapter weights directly from the unified memory pool, performs the segmented low-rank projection X \* B and the subsequent expansion X \* B \* A, and accumulates the residual delta directly into the output tensor Y.43 This eliminates padding overhead and enables highly concurrent, heterogeneous multi-adapter decoding batches with millisecond-level execution penalties.45

vLLM Multi-Adapter Serving Engine Parameters:  
  \--enable-lora : Activates the scheduler's LoRAManager node.  
  \--max-loras : Restricts the active concurrent adapter slot count in GPU HBM.  
  \--max-lora-rank : Allocates pre-formatted memory buffers sized to this rank limit.  
  \--max-cpu-loras : Manages the intermediate LRU cache tier in system DRAM.

### **Adapter Management Model**

To prevent deployment errors, adapter artifacts must be registered alongside precise base model dependencies and structural assumptions.

| Component Attribute | System Requirement & Schema | Verification Mechanism | Validation Protocol |
| :---- | :---- | :---- | :---- |
| **Adapter ID** | Unique string identifier: {Base-Model-Name}-{Task-Namespace}-v{Version}.8 | Structural regex checking in registry database. | Reject load requests containing duplicated namespaces. |
| **Base Model Mapping** | Absolute identifier hash of the base checkpoint (e.g., sha256 value).18 | Verifies exact parameter match of the host model during serving engine load. | Halt server initialization if the base checkpoint hash diverges by a single bit.18 |
| **Tokenizer Version** | Identifier mapping the exact vocabulary config used in training.18 | Schema verification of tokenizer\_config.json properties.18 | Throw exceptions if special tokens (e.g., task separators) are missing from host tokenizer. |
| **Training Objective** | Explicit objective tag.7 | Metadata inspection of the registered adapter card.18 | Reject serving combinations that attempt to stack incompatible objectives on-the-fly. |
| **Hyperparameters** | Full logging of Rank (r), Alpha (alpha), Dropout, and Target Modules.7 | Automatic schema parse of adapter\_config.json at startup.18 | Throw runtime serving errors if the host pre-allocated buffer size is smaller than the target rank.8 |
| **Tenant Scope** | Explicit classification tag restricting tenant ownership (e.g., SaaS\_Tenant\_A).8 | Multi-tenant dynamic authorization check at the request routing layer.8 | Route-level exception if a request attempts to execute an adapter outside authorized scope.8 |
| **Merge Policy** | Rule structure parameters. | Configuration property check during deployment compilation.19 | If static merge, invoke TIES-Merging and output consolidated checkpoint.49 |
| **Stacking Policy** | Rule structure parameters. | Informs the serving engine whether the adapter can co-exist with other active layers.8 | Reject inference request if multiple incompatible adapters are routed to a single token segment.48 |
| **Rollback Path** | Fallback adapter ID or native frozen base model routing instruction. | Health checking monitoring nodes in the serving loop.8 | Automatically route traffic back to target state if p95 latency or error rates spike.8 |

## **Adaptation Card Schema**

Every adaptation run must generate a structured, immutable Adaptation Card (or Adapter Decision Record) that serves as the metadata package required for production lifecycle handover.18

### **System Metadata & Registry Coordinates**

* **Artifact URI**: s3://ai-engine-registry/adapters/Llama-3-8B-Code-Extractor-v2.3.safetensors  
* **Base Checkpoint Hash**: sha256:e12a439b83b9c70010f3db1c521852ba7791ccda1b4b1cf93bc51bcf6a17b1bf 18  
* **Registry Coordinate**: production.core-models.llama3-8b.adapters.extractor-v2-3 8  
* **Owner / Escalation Point**: Core AI Platform Infrastructure Team — On-Call Alert ID: AI-INFRA-SEV1

### **System Objective & Justified Interventions**

* **Failure Class**: True Adaptation Gap.7  
* **Root-Cause Diagnosis**: The baseline model exhibits a 14.2% failure rate when parsing complex nested COBOL records into standardized JSON structures, driven by context bloat and stylistic inconsistencies under few-shot prompting.  
* **Lighter Steering Alternatives Rejected**:  
  1. *Prompt Engineering*: Rejected due to high token overhead (\>1,800 tokens per prompt), which increased TTFT by 320 ms and degraded system margins by $0.12 per transaction.5  
  2. *Structured Outputs (Inference-time constraint)*: Constrained sampling was retained as an active layer but was insufficient on its own because the base model lacked semantic understanding of COBOL structures, leading to a high rate of logically incorrect field mapping.17

### **Dataset Lineage & Calibration**

* **Sourcing Coordinates**: dataset-saas-cobol-parser-v2.3 (Sourced via anonymized production transaction logs).18  
* **Quality Metrics**: 42,500 paired sequences, de-duplicated using MinHash LSH at threshold 0.85.5  
* **Inter-Rater Consensus**: Double-blind expert review achieved an overall Fleiss' Kappa score of kappa \= 0.84 across 5 concurrent raters.38  
* **PII & Secrets Verification**: Passed through Microsoft Presidio redaction engine with 100% zero-leak check.4

### **Training Configuration**

* **Optimization Framework**: Axolotl v0.4.1 running PyTorch 2.3.1 with CUDA 12.1.  
* **Hyperparameter Settings**:  
  * *Adapter Method*: LoRA.7  
  * *Rank (r)*: 16 | *Alpha (alpha)*: 32\.14  
  * *Learning Rate*: 2 \* 10^(-4) with Cosine Decay.7  
  * *Target Modules*: q\_proj, v\_proj, k\_proj, o\_proj, gate\_proj, up\_proj, down\_proj.1  
  * *Batch Size*: 128 (Gradient Accumulation Steps: 4\) | *Epochs*: 3\.26

### **Validation Results & Regression Safety**

* **Target Task Success Rate**: Improved parsing accuracy from 85.8% (baseline) to 98.4% on the held-out validation set.  
* **General Capability Impact**:  
  * *MMLU Score Delta*: \-0.4% (baseline: 66.2% | post-adaptation: 65.8%).4  
  * *HumanEval Delta*: \+0.2% (demonstrates general code syntax preservation).31  
* **Format Adherence Rate**: 100% valid JSON syntax over 10,000 simulated test cases.17  
* **Jailbreak Refusal Preservation**: 100% safety preservation when evaluated against the internal Red-Teaming Jailbreak Suite.4

### **Serving & Deployment Constraints**

* **Serving Engine**: vLLM v0.4.2.8  
* **HBM Allocation Profile**: Pre-allocate dynamic LoRA buffers to \--max-lora-rank 16\.8  
* **Co-Existence Restrictions**: May stack alongside system-wide safety guardrail adapters, but cannot stack alongside other custom customer-facing parser adapters.  
* **P95 Latency SLA Limit**: 85 ms TTFT at concurrency 32\.  
* **Rollback & Reconsideration Triggers**:  
  1. *Rollback Trigger*: Prompt-level p95 latency exceeds 120 ms or runtime syntax exception rate exceeds 0.1% over a 100-request sliding window.8  
  2. *Reconsideration Condition*: If the underlying COBOL-to-JSON database schema expands to support generic mainframe record updates, retrain the adapter or migrate the behavior logic back to dynamic context-space compilation.

## **Distillation Economics Model**

Model distillation is an architectural and economic intervention designed to transition expensive, general-purpose capabilities from a parameter-heavy teacher model into a compact, low-latency student model optimized for a production task envelope.5

### **Distillation Scaling Laws**

The performance of a distilled student model is governed by rigorous scaling laws parameterized by the student size (N\_S), distillation token volume (D\_S), and the teacher's capability represented by its cross-entropy loss (L\_T).51 Research by Busbridge et al. (2025) models the student's cross-entropy loss (L\_S) through a broken power law 33:  
L\_S(N\_S, D\_S, L\_T) \= \+ phi(L\_T, N\_S)  
The term phi(L\_T, N\_S) models the **Capacity Gap**.32 Under standard scaling, a stronger teacher (lower L\_T) yields a stronger student (lower L\_S).51 However, if the teacher's capacity is excessively high relative to the student's parameter limit N\_S, a representation mismatch occurs.32 The student model lacks the representational hypothesis space required to model the highly complex, multi-modal logit distributions of the teacher, resulting in optimization instability and an actual degradation of student performance (forming a U-shaped capacity gap curve).32  
Furthermore:

* Given infinite compute or token budgets, **direct supervised learning from ground truth always matches or outperforms distillation**.51  
* Distillation is highly compute-efficient under two distinct operational conditions: (1) a pre-trained teacher model already exists, or (2) the training run intends to generate a fleet of diverse student models from a single teacher investment.32  
* If only a single student model is required and a teacher model must be trained from scratch first, directly allocating the entire compute budget to supervised pre-training of the student is mathematically superior.51

### **Distillation Economics Cost Function**

To justify distillation, the system architect must evaluate the total lifecycle cost function of the student model relative to the teacher baseline.  
C\_total(V) \= C\_generation \+ C\_training \+ C\_evaluation \+ C\_infrastructure \+ V \* T \* P\_student  
C\_teacher\_baseline(V) \= V \* T \* P\_teacher  
where V represents the total projected inference request volume, T is the average token footprint per request, P\_student is the inference price per token of the student, and P\_teacher is the inference price per token of the teacher.5

Distillation Break-Even Volumetric Equation:  
  V\_break\_even \= (C\_generation \+ C\_training \+ C\_evaluation \+ C\_infrastructure) / (T \* (P\_teacher \- P\_student)) 

### **Volumetric Scenario Comparison**

| Cost & Serving Component | Teacher Baseline (e.g., Llama 3.3 70B) | Student Artifact (e.g., Distilled 3B) | Economic / Operational Impact |
| :---- | :---- | :---- | :---- |
| **Inference Price (per 1M tokens)** | $15.00 | $0.15 | **100x operational inference cost reduction**.5 |
| **Teacher Token Generation Cost** | $0.00 | $3,000.00 | Cost to generate 200 million high-fidelity teacher tokens.5 |
| **Compute Training Cost** | $0.00 | $1,200.00 | 16 hours of 8x H100 GPU cluster execution. |
| **Evaluation & Red-Teaming Cost** | $0.00 | $1,500.00 | Automated regression test sweeps and safety evaluations.5 |
| **Maintenance / Operational Burden** | Low (hosted API or stable base instance) | Moderate (custom artifact tracking and monitoring) | High infrastructure cost initially; amortized rapidly. |
| **Quality Retention Metric** | 98.4% (Golden Standard) | 97.2% (Task Accuracy) | Negligible task performance decay within the narrow envelope.5 |
| **Latency Gain (Time-to-First-Token)** | 480 ms | 42 ms | **11.4x faster user response latency**.5 |
| **Hardware serving footprint** | 8x A100 (80GB) Tensor Parallel | 1x L4 (24GB) Single Instance | **Transition from massive GPU clusters to commodity cards**.5 |
| **Break-Even Volume (V)** | 0 requests | **23.1 Million Tokens** | **Distillation becomes highly profitable above 23.1M tokens.** |

## **Adaptation Regression and Safety Evaluation Plan**

Every model adaptation pipeline must pass through a multi-stage validation suite prior to staging or production deployment to protect against negative transfer, capability regression, and safety decay.

                   ┌────────────────────────────────────────┐  
                   │    Adaptive Model Candidate Check      │  
                   └───────────────────┬────────────────────┘  
                                       │  
            ┌──────────────────────────┴──────────────────────────┐  
            ▼                                                     ▼  
┌───────────────────────┐                               ┌───────────────────────┐  
│ Target Behavior Evals │                               │ Alignment Evals       │  
│ \- Exact Match Accuracy│                               │ \- Refusal Accuracy    │  
│ \- Syntax & Schema Adh.│                               │ \- Jailbreak Resistance│  
│ \- Tool-Call Success   │                               │ \- Privacy Leak Check  │  
└───────────┬───────────┘                               └───────────┬───────────┘  
            │                                                       │  
            └──────────────────────────┬────────────────────────────┘  
                                       │  Pass All Verification Criteria  
                                       ▼  
                        ┌─────────────────────────────┐  
                        │      Regression Evals       │  
                        │ \- MMLU / GSM8K Track        │  
                        │ \- Perplexity Drift Tracking │  
                        │ \- Negative Transfer Check   │  
                        └──────────────┬──────────────┘  
                                       │  Pass Threshold (Max 2% Decay)  
                                       ▼  
                        ┌─────────────────────────────┐  
                        │      Shadow Deployment      │  
                        │ \- Live Mirror Traffic Run   │  
                        │ \- Latency SLA Assessment    │  
                        │ \- Error Budget Tracking     │  
                        └─────────────────────────────┘

1. **Baseline Calibration Sweep**: Prior to fine-tuning, run the base model through prompt-engineered, context-grounded, and validation scenarios to establish a performance baseline.  
2. **Held-Out Target Task Evaluation**: Assess the candidate on a partitioned, unseen test dataset of target tasks.5 The task success rate must equal or exceed the baseline model.5  
3. **Syntactic & Structured-Output Validation**: Run 10,000 consecutive generations against strict schema parsers.18 The adapter must maintain a 100% valid format adherence rate.18  
4. **Retrieval-Grounded & Tool-Use Verification**: Evaluate the model's command adherence inside active tool harnesses.47 The model must call target tools with correct parameter types and structures.47  
5. **General-Capability Preservation Test**: Run general capability evaluations (e.g., MMLU for factual density, GSM8K for logical consistency, and HumanEval for code syntax).7 The candidate must display less than a 2% decay across all general benchmarks.4  
6. **Adversarial and Jailbreak Red-Teaming**: Execute automated adversarial prompt sweeps, injection attacks, and safety tests.4 The candidate must not display unsafe capability shifts or bypass established safety boundaries.4  
7. **Privacy Leakage and Redaction Audit**: Run semantic probing to extract potentially memorized private schemas or training data PII. The model must refuse to reveal secure training boundaries.4  
8. **Shadow Deployment Verification**: Route production traffic concurrently to both the baseline model and the adapted model.4 Log output divergence, latency deltas, and token throughput limits without exposing users to adapter errors.  
9. **Canary Rollout and Automated Rollback**: Roll out the adapter to 1% of live traffic, scaling progressively to 10%, 50%, and 100%. Automatically trigger rollback to the base model if the p95 latency exceeds the SLA by \>15 ms or if exception rates spike.8

## **Adaptation Failure Mode Map**

Weight optimization carries severe operational and systemic risks that can compromise downstream behavior.

Supervised Fine-Tuning (SFT) Weight Shifts  
  │  
  ├─► Over-optimization of surface tokens  ──►  
  │                                               (Model repeats boilerplate length/format)   
  ├─► Overwriting deep parameter coordinates ──► \[Catastrophic Forgetting\]  
  │                                               (General reasoning/MMLU collapses)   
  └─► Over-sensitization of safety data     ──►  
                                                  (Benign queries trigger false refusals) 

### **Style Overfit**

* **Target Task Symptom**: The model produces extremely verbose, formulaic, or repetitive responses, echoing training length and syntax templates instead of actual task semantics.23  
* **Root-Cause Mechanism**: The cross-entropy loss function overfitted to superficial formatting patterns and sequence length distributions present in the training corpus.4  
* **Mitigation Protocol**: Enforce length penalties during generation; incorporate strict length limits in the training loss; mix in diverse formatting examples.

### **Memorization**

* **Target Task Symptom**: The model reproduces exact sequences, examples, or training document paragraphs verbatim under minimal prompting, risking private data leakage.4  
* **Root-Cause Mechanism**: High parameter density training on redundant datasets with excessive epochs led to parameter memorization of training instances.26  
* **Mitigation Protocol**: Deduplicate training data using MinHash LSH; apply weight decay; restrict epoch counts; employ DP-SGD.

### **Catastrophic Forgetting**

* **Target Task Symptom**: The model's general logical reasoning, world knowledge, mathematical capacity, or multi-task adherence scores collapse post-adaptation.13  
* **Root-Cause Mechanism**: Specialized task gradients overrode the model's pre-trained deep representation coordinates, destroying general capabilities.3  
* **Mitigation Protocol**: Use Selective Token Masking (STM) to mask high-perplexity tokens 2; mix in 20% general pre-training data 3; use LoRA with tight ranks.7

### **Format Regression**

* **Target Task Symptom**: The model fails to adhere to system format constraints (e.g., outputs malformed JSON or raw Markdown instead of valid XML).  
* **Root-Cause Mechanism**: Specialized domain training eroded the pre-trained instruction-following and system-prompt constraint layers.4  
* **Mitigation Protocol**: Integrate deterministic structured sampling engines 17; include format-adherence task examples in the training corpus.4

### **Tool-Use Regression**

* **Target Task Symptom**: The model fails to call tool APIs, mixes up argument parameters, or generates invalid API formats.47  
* **Root-Cause Mechanism**: Representation drift in the token embedding space decayed the model's pre-trained tool-use execution structures.47  
* **Mitigation Protocol**: Ensure the training dataset contains explicit tool-calling formats and correct parameters; run fine-grained tool tests.47

### **Refusal Drift**

* **Target Task Symptom**: The model becomes hyper-conservative, refusing benign requests containing completely harmless, adjacent keywords.4  
* **Root-Cause Mechanism**: Unbalanced safety or preference tuning over-penalized safety-adjacent boundaries without contrastive examples.4  
* **Mitigation Protocol**: Mix contrastive pairs into preference tuning showing correct refusal vs. correct execution; align with KTO prospect loss.2

### **Unsafe Capability Shift**

* **Target Task Symptom**: The model begins generating toxic content, bypasses pre-trained jailbreak constraints, or leaks secure boundaries under adversarial queries.4  
* **Root-Cause Mechanism**: Specialized domain adaptation inadvertently dismantled pre-trained safety barriers or alignment bounds.4  
* **Mitigation Protocol**: Include safety retention sets in training data 4; execute continuous red-teaming checks during candidate validation.21

### **Overconfident Hallucination**

* **Target Task Symptom**: The model generates highly convincing but structurally incorrect or fabricated facts with high probability.4  
* **Root-Cause Mechanism**: SFT training on noisy data with incorrect labels forced the model's output probabilities to align with falsehoods.  
* **Mitigation Protocol**: Enforce strict data validation; train the model to output "I do not know" or escalate when uncertainty is high.4

### **Domain Tunnel Vision**

* **Target Task Symptom**: The model becomes highly capable on narrow target tasks but displays an inability to adapt to slight variations in prompt style or task framing.4  
* **Root-Cause Mechanism**: The model over-optimized to the specific prompt templates and domain language of the training set.  
* **Mitigation Protocol**: Perturb prompt templates dynamically during SFT; train on diverse multi-task datasets.

### **Prompt-Template Dependence**

* **Target Task Symptom**: The adapted model performs the task correctly only when prompted with the exact syntax used in training; minor changes degrade accuracy.18  
* **Root-Cause Mechanism**: The model coupled its behavioral changes to the surface features of the training templates instead of the underlying concepts.18  
* **Mitigation Protocol**: Enforce prompt template diversity in the training corpus; run zero-shot prompt robustness tests during validation.

### **Synthetic Monoculture**

* **Target Task Symptom**: The model outputs repetitive, formulaic transitions (e.g., "delve", "testament") and displays a collapse of its logit entropy.26  
* **Root-Cause Mechanism**: Continuous training on synthetic data generated by a single model family caused the student's logit diversity to decay.5  
* **Mitigation Protocol**: Perturb teacher model prompts dynamically 5; mix in at least 20% organic human-written text; run semantic diversity sweeps.26

### **Label-Noise Amplification**

* **Target Task Symptom**: The model outputs inconsistent syntax, incorrect structures, and conflicting definitions under normal operations.  
* **Root-Cause Mechanism**: The SFT loss function amplified minor, systematic labeling errors present in the training dataset.5  
* **Mitigation Protocol**: Enforce rigorous data validation and rater consensus using Fleiss' Kappa before training.38

### **Benchmark Contamination**

* **Target Task Symptom**: The model displays artificially inflated 99% accuracy scores on public benchmarks but fails simple, real-world task variations.26  
* **Root-Cause Mechanism**: Public evaluation benchmarks leaked verbatim or in near-identical form into the pre-tuning or fine-tuning datasets.26  
* **Mitigation Protocol**: Run contamination sweeps using the Kernel Divergence Score (KDS) before training.29

### **Stale-Knowledge Fossilization**

* **Target Task Symptom**: The model output is structurally correct but references outdated policies, broken URLs, or dynamic facts that have changed in production.4  
* **Root-Cause Mechanism**: Volatile factual knowledge was encoded into the parameters instead of being managed in retrieval or context space.3  
* **Mitigation Protocol**: Keep dynamic facts out of weights; decouple behavioral formatting from information retrieval.

### **Privacy Leakage**

* **Target Task Symptom**: Under semantic probing or adversarial extraction, the model leaks training-data PII, API tokens, or tenant schemas.4  
* **Root-Cause Mechanism**: Failure to redact sensitive data prior to SFT, allowing the model to memorize PII within its weights.4  
* **Mitigation Protocol**: Enforce strict Presidio-based PII scrubbing; run adversarial extraction checks before staging.

### **Tenant Bleed**

* **Target Task Symptom**: User B receives custom outputs displaying private data, stylistic quirks, or schemas belonging to User A.3  
* **Root-Cause Mechanism**: Multi-tenant data records were co-mingled in the same physical checkpoint or adapter during fine-tuning.3  
* **Mitigation Protocol**: Restrict multi-tenant training to isolated physical LoRA adapters; serve them on a per-tenant basis.3

### **Adapter Incompatibility**

* **Target Task Symptom**: Swapping an adapter at runtime causes the model to output structural gibberish, crash, or enter infinite token generation loops.8  
* **Root-Cause Mechanism**: The adapter config (rank, target modules, tokenizer special tokens) mismatched the host base model.8  
* **Mitigation Protocol**: Enforce sha256 base model hash verification; freeze tokenizer versions across the adapter serving pool.18

## **Evaluation and Observability Guidance**

An adapted model in production requires systematic runtime monitoring. The dashboard below details the critical telemetry indicators that track the operational health and performance of adaptation deployments:

| Monitoring Metric | Tracking Mechanism | Target Threshold | Alert Trigger | Remediation Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **Task Success Rate** | Automated validation of the execution boundary (e.g., successful API parsing).5 | \>= 95.0% | Drops below 90.0% over a 100-request window. | Route traffic back to the frozen baseline model; audit training dataset coverage. |
| **Delta Over Baseline** | Sliding scale evaluation comparing target adapter accuracy to the base model. | Positive gain (\>= 5.0%) | Drops below baseline model performance. | The adapter is causing negative transfer; check for prompt syntax mismatch. |
| **Cost Per Successful Outcome** | Calculation: Total serving cost / Successful transaction count. | Match or beat system margin targets. | Spikes above baseline cost due to excessive output length or retries. | Adjust generation max token bounds; check if model is stuck in generation loops. |
| **First-Attempt Validity** | Programmatic schema parsing of the raw model output.18 | \>= 98.0% | Drops below 95.0% over a sliding window. | Enforce structured output generation at the serving engine layer.17 |
| **Schema Adherence** | Assert syntax validity (e.g., JSON validation against Pydantic schema).18 | 100.0% | Any syntax failure in production. | Enable constrained decoding at the serving engine level.17 |
| **Tool-Call Success** | Execution success rate of generated tool arguments.5 | \>= 98.0% | Drops below 95.0%. | Audit schema templates; run fine-grained targeted tool-use validation sweeps. |
| **Grounding Score** | Semantic entailment score of outputs against retrieved context documents. | \>= 0.90 | Drops below 0.80 over a sliding window. | The model is hallucinating; lower generation temperature or inject negative examples into SFT. |
| **Citation Support** | Assert that output statements are accurately mapped to retrieval source coordinates. | 100.0% | Any output statement lacking verified document source pointers. | Reject generation output; check if the model has drifted from formatting conventions. |
| **Human Override Rate** | Ratio of outputs manually overridden or rejected by operators. | \<= 2.0% | Overrides spike above 5.0% in any operational shift. | Trigger active retraining loop; re-evaluate annotation guidelines for the task. |
| **User Correction Rate** | Tracking user prompt edits indicating satisfaction failures. | \<= 10.0% | Correction edits spike above 15.0%. | Audit model completions; re-evaluate prompt templates and context formatting. |
| **Refusal Accuracy** | Ratio of correct refusals to total refusal events.4 | 100.0% | Any false refusal of a benign, safe prompt. | Adjust refusal examples in the training dataset; retrain using KTO loss.2 |
| **Safety Incidents** | Monitoring for policy violations, toxic generation, or jailbreaks.4 | **0 Incidents** | Any confirmed jailbreak or policy bypass in production. | Immediately roll back to frozen safety baseline; escalate to red-team audit.8 |
| **Regression Rate** | Accuracy degradation on held-out general benchmarks post-adaptation.7 | \<= 2.0% | General task performance decays by \>2.0% post-deployment. | The adapter is causing catastrophic forgetting 13; retrain with general rehearsal sets.4 |
| **Adapter Incident Rate** | Serving engine exceptions specific to adapter swapping.8 | **0 Incidents** | Swapping exceptions or swap-induced latency spikes over SLA bounds.8 | Audit vLLM memory configurations; verify base-adapter tokenizer configuration.18 |
| **Drift Signals** | Tracking semantic distance deltas between live prompts and the training set. | Within 1.5x of dev set | Prompt perplexity drifts significantly over a 24-hour period. | Operational input drift has occurred; users are prompting the model outside its task envelope. |

## **Cross-Canon Handoff Map**

The model adaptation lifecycle interacts directly with surrounding architectural layers across *The AI Engineering Systems Canon*:

| Canon Section | Handoff Artifact / Dependency | Operational Interaction | Doctrinal Constraint |
| :---- | :---- | :---- | :---- |
| **AI-ENG-I (MLOps & Lifecycle)** | Immutable checkpoint package, registered Adapter ID, metadata logs.8 | AI-ENG-H delivers the validated adaptation artifact to AI-ENG-I for rollout orchestration. | AI-ENG-I must enforce automated regression gates and canary triggers based on AI-ENG-H validation boundaries.8 |
| **AI-ENG-G (Model Selection)** | Base capability profile, failure tolerance profile, Model Decision Records.14 | AI-ENG-H initiates adaptation when AI-ENG-G selection profiles are insufficient for task limits. | AI-ENG-H must respect AI-ENG-G decision records; upgrading base models is prioritized over training. |
| **AI-ENG-C (System Economics)** | Distillation Economics Model, latency delta metrics, margin targets.5 | AI-ENG-H maps distillation scaling laws and costs directly to the system-margin parameters in AI-ENG-C. | Distillation is rejected under AI-ENG-C if projected inference volume fails to clear the break-even point.5 |
| **AI-ENG-D/F (Data Governance)** | Raw source documents, licensing status, provenance metadata.18 | AI-ENG-H ingests curated records from AI-ENG-D/F to construct the SFT/alignment corpora. | All training inputs must pass AI-ENG-D/F provenance and licensing checks before parameter training starts.18 |
| **AI-ENG-J/K/L (Model Serving)** | Punica SGMV kernels, S-LoRA configs, HBM partition schemas.8 | AI-ENG-H adapters are dynamically served and swapped using the serving architecture in AI-ENG-J/K/L.8 | Serving configurations must align with the \--max-lora-rank and memory limits specified in AI-ENG-H.8 |
| **AI-ENG-M/N/O (Agent Systems)** | Tool-calling schemas, structured response conventions, routing tables.47 | AI-ENG-H fine-tunes parameters to format correct arguments for AI-ENG-M/N/O tool executors.47 | Adapted agents must pass strict sandboxed compiler validation to prevent runtime parameter parsing failures. |
| **AI-ENG-S (Production Pathologies)** | Style drift patterns, catastrophic forgetting data, hallucinatory logs.4 | AI-ENG-H ingests edge-case failure logs from AI-ENG-S to run targeted alignment and contrastive training. | Failures detected in AI-ENG-S must be diagnosed before selecting adaptation; do not train on noisy logs. |
| **AI-ENG-Z (Telemetry & Logs)** | Perplexity metrics, prompt drift logs, request latency arrays.8 | AI-ENG-Z feeds continuous runtime metrics back to the AI-ENG-H evaluation plan. | Alert indicators in AI-ENG-Z automatically trigger rollback pathways to safe baseline states.8 |
| **AI-ENG-AA (Eval Architecture)** | Validation datasets, automated rubrics, LLM-as-a-Judge code.5 | AI-ENG-H executes candidate evaluation sweeps using the frameworks built in AI-ENG-AA. | Evaluation datasets must remain strictly decoupled from training to prevent metric contamination.29 |
| **AI-ENG-AB (Audit & Lineage)** | Signed adaptation cards, dataset hashes, commit lineage.18 | AI-ENG-AB catalogs adaptation parameters to ensure fully reproducible, auditable system audits. | No adapter may transition to production without a cryptographically signed Adaptation Card.18 |
| **AI-ENG-AD (Governance & Safety)** | Compliance rules, safety refutations, geographic policy scopes.4 | AI-ENG-AD guides the safety boundaries, refusal parameters, and PII filters used in AI-ENG-H datasets. | Adapted checkpoints must respect AD rules; safety overrides trigger immediate artifact retirement. |

## **Doctrinal Principles for Model Adaptation**

1. **Rule of the Diagnostic**: Never train a model to fix a problem that can be resolved with deterministic structured outputs, schema constraints, or harness-level validations. Always name the system failure before choosing the adaptation instrument.  
2. **Factual Separation**: Keep dynamic knowledge, tenant-private data, permissions, and real-time facts in the context space. Modify weights only to specialize style, enforce structures, or master stable procedural transformations.  
3. **LoRA by Default**: For behavioral adaptation and instruction following, prefer parameter-efficient adapters (LoRA/QLoRA) over full fine-tuning. They act as powerful regularizers, preserve pre-trained capabilities, and prevent representational collapse.  
4. **Data Over Compute**: A model adaptation run is only as robust as the curation, balance, and quality of its dataset. Prioritize rigorous annotation rubrics and multi-rater agreement validation over hyperparameter optimization.  
5. **No Alignment Without Safety**: Preference alignment (DPO, IPO, KTO) is an alignment tool, not a reasoning enhancer. When tuning logits to match comparative styles, always run comprehensive regression sweeps to guard against catastrophic reasoning decay and safety regressions.  
6. **Decoupled Architecture**: In multi-tenant environments, never train customer-specific records into a shared base model. Enforce customer isolation by serving dynamically loaded, single-tenant LoRA adapters on a shared read-only base model.

#### **Works cited**

1. LoRA: Low-Rank Adaptation of Large Language Models \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2106.09685v2](https://arxiv.org/html/2106.09685v2)  
2. KTO: Model Alignment as Prospect Theoretic Optimization \- arXiv, accessed June 8, 2026, [https://arxiv.org/pdf/2402.01306](https://arxiv.org/pdf/2402.01306)  
3. How are you handling catastrophic forgetting in multi-domain LLM fine-tuning pipelines?, accessed June 8, 2026, [https://www.reddit.com/r/mlops/comments/1rnhoa6/how\_are\_you\_handling\_catastrophic\_forgetting\_in/](https://www.reddit.com/r/mlops/comments/1rnhoa6/how_are_you_handling_catastrophic_forgetting_in/)  
4. Avoiding Amnesia: Some Practical Guides to Mitigate Catastrophic Forgetting in LLMs Post-training | by Baicen Xiao | Medium, accessed June 8, 2026, [https://medium.com/@baicenxiao/avoiding-amnesia-some-practical-guides-to-mitigate-catastrophic-forgetting-in-llms-post-training-6a23e4f064cb](https://medium.com/@baicenxiao/avoiding-amnesia-some-practical-guides-to-mitigate-catastrophic-forgetting-in-llms-post-training-6a23e4f064cb)  
5. Teacher-Student Distillation: How It Works and When to Use It \- distil labs, accessed June 8, 2026, [https://www.distillabs.ai/learn/teacher-student-distillation/](https://www.distillabs.ai/learn/teacher-student-distillation/)  
6. Serving Thousands of Concurrent LoRA Adapters | by Mukul Ranjan \- Medium, accessed June 8, 2026, [https://medium.com/@mukulranjan/serving-thousands-of-concurrent-lora-adapters-6b407e8df516](https://medium.com/@mukulranjan/serving-thousands-of-concurrent-lora-adapters-6b407e8df516)  
7. LoRA Learns Less and Forgets Less \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2405.09673v2](https://arxiv.org/html/2405.09673v2)  
8. LoRA Multi-Adapter Serving: Fine-Tune Once, Serve 100 Customers on One GPU \- Spheron, accessed June 8, 2026, [https://www.spheron.network/blog/lora-multi-adapter-serving-gpu-cloud/](https://www.spheron.network/blog/lora-multi-adapter-serving-gpu-cloud/)  
9. Model Distillation and Knowledge Transfer in AI 2026 | Zylos Research, accessed June 8, 2026, [https://zylos.ai/research/2026-02-08-model-distillation](https://zylos.ai/research/2026-02-08-model-distillation)  
10. 𝛼-LoRA: Effective Fine-Tuning via Base Model Rescaling \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2510.21345v1](https://arxiv.org/html/2510.21345v1)  
11. All Knowledge You Need about DPO and its Variants \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2404.14723v2](https://arxiv.org/html/2404.14723v2)  
12. DPO vs PPO: Which RLHF Algorithm to Use for Production LLM Alignment (2026 Decision Guide) | Spheron Blog, accessed June 8, 2026, [https://www.spheron.network/blog/dpo-vs-ppo-rlhf-algorithm-production-llm-alignment/](https://www.spheron.network/blog/dpo-vs-ppo-rlhf-algorithm-production-llm-alignment/)  
13. What is Catastrophic Forgetting? \- IBM, accessed June 8, 2026, [https://www.ibm.com/think/topics/catastrophic-forgetting](https://www.ibm.com/think/topics/catastrophic-forgetting)  
14. How Much is Too Much? Exploring LoRA Rank Trade-offs for Retaining Knowledge and Domain Robustness \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2512.15634v1](https://arxiv.org/html/2512.15634v1)  
15. LoRA Adapters: Efficient Fine-Tuning \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/low-rank-adaptation-lora-adapters](https://www.emergentmind.com/topics/low-rank-adaptation-lora-adapters)  
16. Crafty Patchwork: Parameter-Efficient Fine-Tuning | Vectors & Verbs, accessed June 8, 2026, [https://vectorsandverbs.com/posts/parameter-efficient-fine-tuning/](https://vectorsandverbs.com/posts/parameter-efficient-fine-tuning/)  
17. Quantization in Large Language Models(LLMs) \- Intelligent Machines, accessed June 8, 2026, [https://www.intelligentmachines.blog/post/quantization-in-large-language-models-llms](https://www.intelligentmachines.blog/post/quantization-in-large-language-models-llms)  
18. Deploy multi-LoRA adapters on LLMs \- Anyscale Docs, accessed June 8, 2026, [https://docs.anyscale.com/llm/serving/multi-lora](https://docs.anyscale.com/llm/serving/multi-lora)  
19. Saliency-Aware Model Merging \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2606.00511v1](https://arxiv.org/html/2606.00511v1)  
20. Preference Tuning LLMs with Direct Preference Optimization Methods \- Hugging Face, accessed June 8, 2026, [https://huggingface.co/blog/pref-tuning](https://huggingface.co/blog/pref-tuning)  
21. A-IPO: Adaptive Intent-driven Preference Optimization \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2510.10077v1](https://arxiv.org/html/2510.10077v1)  
22. Identity Preference Optimization (IPO) \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/identity-preference-optimization-ipo](https://www.emergentmind.com/topics/identity-preference-optimization-ipo)  
23. Reward Modeling and DPO: Learning What "Good" Means \- Suvash Sedhain, accessed June 8, 2026, [https://mesuvash.github.io/blog/2026/reward-modeling/](https://mesuvash.github.io/blog/2026/reward-modeling/)  
24. Vinija's Notes • LLM Alignment, accessed June 8, 2026, [https://vinija.ai/concepts/llm-alignment/](https://vinija.ai/concepts/llm-alignment/)  
25. Regarding DPO, IPO, and KTO. DPO method | by Mohammad Reza Esmaeiliyan \- Medium, accessed June 8, 2026, [https://esmln.medium.com/regarding-dpo-ipo-and-kto-02e94e6958ed](https://esmln.medium.com/regarding-dpo-ipo-and-kto-02e94e6958ed)  
26. When Benchmarks Lie: Why Contamination Breaks LLM Evaluation, accessed June 8, 2026, [https://thegrigorian.medium.com/when-benchmarks-lie-why-contamination-breaks-llm-evaluation-1fa335706f32](https://thegrigorian.medium.com/when-benchmarks-lie-why-contamination-breaks-llm-evaluation-1fa335706f32)  
27. From Teacher to Student: Model Distillation for Cost-Effective LLM Deployment \- Marvik.ai, accessed June 8, 2026, [https://www.marvik.ai/blog/from-teacher-to-student-model-distillation-for-cost-effective-llm-deployment](https://www.marvik.ai/blog/from-teacher-to-student-model-distillation-for-cost-effective-llm-deployment)  
28. How to Alleviate Catastrophic Forgetting in LLMs Finetuning? Hierarchical Layer-Wise and Element-Wise Regularization \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2501.13669v2](https://arxiv.org/html/2501.13669v2)  
29. How Contaminated Is Your Benchmark? Quantifying Dataset Leakage in Large Language Models with Kernel Divergence \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2502.00678v1](https://arxiv.org/html/2502.00678v1)  
30. What Matters for Model Merging at Scale? \- OpenReview, accessed June 8, 2026, [https://openreview.net/forum?id=HW26XyHp3P](https://openreview.net/forum?id=HW26XyHp3P)  
31. LoRA Learns Less and Forgets Less \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2405.09673v1](https://arxiv.org/html/2405.09673v1)  
32. Everything You Need to Know about Knowledge Distillation \- Hugging Face, accessed June 8, 2026, [https://huggingface.co/blog/Kseniase/kd](https://huggingface.co/blog/Kseniase/kd)  
33. ICML Poster Distillation Scaling Laws, accessed June 8, 2026, [https://icml.cc/virtual/2025/poster/46615](https://icml.cc/virtual/2025/poster/46615)  
34. NormalFloat-4 (NF4): 4-bit Quantization \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/normalfloat-4-nf4](https://www.emergentmind.com/topics/normalfloat-4-nf4)  
35. 4-bit NormalFloat (NF4) Quantization \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/4-bit-normalfloat-nf4-quantization](https://www.emergentmind.com/topics/4-bit-normalfloat-nf4-quantization)  
36. RLHF and alternatives: IPO \- Argilla, accessed June 8, 2026, [https://argilla.io/blog/mantisnlp-rlhf-part-6/](https://argilla.io/blog/mantisnlp-rlhf-part-6/)  
37. Kahneman-Tversky Optimization(KTO): Revolutionizing Language Model Training with Prospect Theory | by Yatin Arora | Medium, accessed June 8, 2026, [https://medium.com/@SpielmitDaten/kahneman-tversky-optimization-kto-revolutionizing-language-model-training-with-prospect-theory-99f30c50481e](https://medium.com/@SpielmitDaten/kahneman-tversky-optimization-kto-revolutionizing-language-model-training-with-prospect-theory-99f30c50481e)  
38. A Tutorial on Sample Size Calculation for Inter-rater and Intra-rater Agreement Studies \- PMC, accessed June 8, 2026, [https://pmc.ncbi.nlm.nih.gov/articles/PMC12935580/](https://pmc.ncbi.nlm.nih.gov/articles/PMC12935580/)  
39. Fleiss' Kappa | Real Statistics Using Excel, accessed June 8, 2026, [https://real-statistics.com/reliability/interrater-reliability/fleiss-kappa/](https://real-statistics.com/reliability/interrater-reliability/fleiss-kappa/)  
40. Fleiss' Kappa: Measuring Agreement Among Multiple Raters \- Numiqo, accessed June 8, 2026, [https://numiqo.com/tutorial/fleiss-kappa](https://numiqo.com/tutorial/fleiss-kappa)  
41. Fleiss's kappa \- Wikipedia, accessed June 8, 2026, [https://en.wikipedia.org/wiki/Fleiss%27s\_kappa](https://en.wikipedia.org/wiki/Fleiss%27s_kappa)  
42. Improving the Serving Performance of Multi-LoRA Large Language Models via Efficient LoRA and KV Cache Management \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2505.03756v1](https://arxiv.org/html/2505.03756v1)  
43. 1 Introduction \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2311.03285v3](https://arxiv.org/html/2311.03285v3)  
44. S-LoRA: Scalable LoRA Serving \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/scalable-serving-s-lora-system](https://www.emergentmind.com/topics/scalable-serving-s-lora-system)  
45. Multi-Tenant LoRA Serving \- Punica \- arXiv, accessed June 8, 2026, [https://arxiv.org/pdf/2310.18547](https://arxiv.org/pdf/2310.18547)  
46. \[Tracking\] Multi-LoRA Serving · Issue \#3446 · mlc-ai/mlc-llm \- GitHub, accessed June 8, 2026, [https://github.com/mlc-ai/mlc-llm/issues/3446](https://github.com/mlc-ai/mlc-llm/issues/3446)  
47. punica\_wrapper \- vLLM Documentation, accessed June 8, 2026, [https://docs.vllm.ai/en/latest/api/vllm/lora/punica\_wrapper/](https://docs.vllm.ai/en/latest/api/vllm/lora/punica_wrapper/)  
48. Clarification: Does vLLM support concurrent decoding with multiple LoRA adapters in online inference?, accessed June 8, 2026, [https://discuss.vllm.ai/t/clarification-does-vllm-support-concurrent-decoding-with-multiple-lora-adapters-in-online-inference/1482](https://discuss.vllm.ai/t/clarification-does-vllm-support-concurrent-decoding-with-multiple-lora-adapters-in-online-inference/1482)  
49. TIES-Merging: Robust Model Integration \- Emergent Mind, accessed June 8, 2026, [https://www.emergentmind.com/topics/ties-merging](https://www.emergentmind.com/topics/ties-merging)  
50. MergeME: Model Merging Techniques for Homogeneous and Heterogeneous MoEs \- Amazon Science, accessed June 8, 2026, [https://assets.amazon.science/a2/38/e9dbfd2c4a36a651e0cbf20c20aa/mergeme-model-merging-techniques-for-homogeneous-and-heterogeneous-moes.pdf](https://assets.amazon.science/a2/38/e9dbfd2c4a36a651e0cbf20c20aa/mergeme-model-merging-techniques-for-homogeneous-and-heterogeneous-moes.pdf)  
51. Distillation Scaling Laws \- arXiv, accessed June 8, 2026, [https://arxiv.org/html/2502.08606v2](https://arxiv.org/html/2502.08606v2)  
52. Distillation Scaling Laws \- GitHub, accessed June 8, 2026, [https://raw.githubusercontent.com/mlresearch/v267/main/assets/busbridge25a/busbridge25a.pdf](https://raw.githubusercontent.com/mlresearch/v267/main/assets/busbridge25a/busbridge25a.pdf)  
53. \[Literature Review\] Distillation Scaling Laws \- Moonlight, accessed June 8, 2026, [https://www.themoonlight.io/en/review/distillation-scaling-laws](https://www.themoonlight.io/en/review/distillation-scaling-laws)  
54. How Contaminated Is Your Benchmark? Measuring Dataset Leakage in Large Language Models with Kernel Divergence \- ICML 2026, accessed June 8, 2026, [https://icml.cc/virtual/2025/poster/43619](https://icml.cc/virtual/2025/poster/43619)

---