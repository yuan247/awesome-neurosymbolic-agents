# Awesome Neuro-Symbolic Learning in the Agent Era

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Learning that puts a neural model together with a symbolic object: a logical formula, a program, a proof, a PDDL operator, a path on a knowledge graph, a skill library, or a lemma library.

The list follows the three routes in Yang et al., IJCAI 2025 ([paper](https://arxiv.org/abs/2508.13678), [paper table](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy)).

1. **Symbolic to model.** A symbolic system supplies data or a teacher signal, and the model absorbs it into its parameters.
2. **Model to symbolic.** The model writes logic, a program, or a proof, and an external system runs it.
3. **Joint training.** The symbolic layer stays in the computation graph, or the solver's accept/reject signal enters training.

## Contents

- [Surveys](#surveys)
- [Symbolic to model](#symbolic-to-model)
- [Model to symbolic](#model-to-symbolic)
- [Joint training](#joint-training)
- [Benchmarks](#benchmarks)
- [Other lists](#other-lists)
- [Contributing](#contributing)

## Surveys

- [Neuro-Symbolic Artificial Intelligence: Towards Improving the Reasoning Abilities of Large Language Models](https://arxiv.org/abs/2508.13678) — Source of the three routes above: symbolic to LLM, LLM to symbolic, and the two together. Yang et al., IJCAI 2025.
- [Neuro-Symbolic AI in 2024: A Systematic Review](https://arxiv.org/abs/2501.05435) — Counts 2020–2024 work by knowledge representation, learning and inference, logic, explainability, and metacognition. Learning and inference dominates. Metacognition is thin. Colelough and Regli, 2025.
- [Converging Paradigms: The Synergy of Symbolic and Connectionist AI in LLM-Empowered Autonomous Agents](https://arxiv.org/abs/2407.08516) — How a neural net, a workflow, and symbolic constraints split the work inside a language agent. Xiong et al., 2024.
- [A Survey on Symbolic Knowledge Distillation of Large Language Models](https://arxiv.org/abs/2408.10210) — Extracts symbolic structure out of a model. Acharya, Velasquez, and Song, 2024.
- [Neuro-Symbolic Agents for Regulated Process Automation: Challenges and Research Agenda](https://arxiv.org/abs/2606.13405) — In regulated processes the symbolic layer has to be auditable. Rombach et al., 2026.

## Symbolic to model

A symbolic system provides the teacher signal. The model takes it into its parameters.

### Into the weights

Training sees symbolic knowledge or explicit steps. Inference sees the question.

- [SKIntern: Internalizing Symbolic Knowledge for Distilling Better CoT Capabilities into Small Language Models](https://arxiv.org/abs/2409.13183) — Starts with symbolic summaries and examples in the prompt, then drops them on a curriculum, until a small model answers from the question alone. Liao, He, Hao et al., COLING 2025. [Code](https://github.com/Xnhyacinth/SKIntern)
- [From Explicit CoT to Implicit CoT: Learning to Internalize CoT Step by Step](https://arxiv.org/abs/2405.14838) — Removes chain-of-thought tokens step by step, so the intermediate steps move into the hidden state. Deng, Choi, and Shieber, 2024. [Code](https://github.com/da03/Internalize_CoT_Step_by_Step)
- [Neural-Symbolic Collaborative Distillation: Advancing Small Language Models for Complex Reasoning Tasks](https://arxiv.org/abs/2409.13203) — Frequent reasoning patterns go into the parameters. Rare knowledge stays in an external knowledge base. Liao, He, Xu et al., AAAI 2025. [Code](https://github.com/Xnhyacinth/NesyCD)
- [Scaling Neuro-symbolic Problem Solving: Solver-Free Learning of Constraints and Objectives](https://arxiv.org/abs/2508.20978) — Learns constraints and objectives of combinatorial problems with no solver in the training loop. Defresne, Gambardella, and Barbe, 2025.
- [Transformers as Soft Reasoners over Language](https://arxiv.org/abs/2002.05867) — Trains a Transformer on rule data from a symbolic generator, then lets it carry out the multi-step inference. Clark, Tafjord, and Richardson, 2020.

### Imitating symbolic traces

Training traces come from a symbolic engine. At test time the engine still checks.

- [Solving olympiad geometry without human demonstrations](https://doi.org/10.1038/s41586-023-06747-5) — AlphaGeometry. The language model trains on synthetic proofs. At test time a symbolic deduction engine still checks the auxiliary constructions. Trinh et al., Nature 2024. [Code](https://github.com/google-deepmind/alphageometry)
- [Gold-medalist Performance in Solving Olympiad Geometry with AlphaGeometry2](https://arxiv.org/abs/2502.03544) — Larger formal language and a larger search. Chervonyi, Trinh, Olšák et al., 2025. [Code](https://github.com/google-deepmind/alphageometry2)
- [Wu's Method can Boost Symbolic AI to Rival Silver Medalists and AlphaGeometry to Outperform Gold Medalists at IMO Geometry](https://arxiv.org/abs/2404.06405) — Replaces the geometry backend with Wu's method. Sinha et al., 2024.
- [DeepSeek-Prover: Advancing Theorem Proving in LLMs through Large-Scale Synthetic Data](https://arxiv.org/abs/2405.14333) — Supervised training on synthetic Lean proofs. V1.5 and V2 add prover feedback and are listed under [symbolic feedback](#symbolic-feedback). Xin, Guo, Shao et al., 2024.
- [Goedel-Prover: A Frontier Model for Open-Source Automated Theorem Proving](https://arxiv.org/abs/2502.07640) — Open Lean model trained on synthetic statements and proofs. V2 is under symbolic feedback. Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover)
- [EvolProver: Advancing Automated theorem proving by Evolving Formalized Problems via Symmetry and Difficulty](https://iclr.cc/virtual/2026/poster/10008534) — Rewrites formalized problems along symmetry and difficulty, then trains a prover on them. ICLR 2026.

## Model to symbolic

The model writes logic, a program, or a proof. A solver, an interpreter, or a search procedure produces the result.

### Solvers

- [Faithful Logical Reasoning via Symbolic Chain-of-Thought](https://arxiv.org/abs/2405.18357) — SymbCoT. Keeps symbolic expressions and a check inside the reasoning trace. Xu, Fei, Pan et al., 2024.
- [VeriCoT: Neuro-symbolic Chain-of-Thought Validation via Logical Consistency Checks](https://arxiv.org/abs/2511.04662) — Turns a chain of thought into logic that can be checked for consistency. Feng, Weir et al., ICLR 2026.
- [FormalJudge: A Neuro-Symbolic Paradigm for Agentic Oversight](https://arxiv.org/abs/2602.11136) — A formal judge over an agent's outputs. Zhou, Sheng et al., ICML 2026.
- [PrologMCP: A Standardized Prolog Tool Interface for LLM Agents](https://arxiv.org/abs/2606.14935) — The model translates. Prolog does the deduction. Mensfelt et al., 2026.
- [Decodable but Not Faithful: Coupling Natural-Language Rationales to Programmatic Verifiers](https://arxiv.org/abs/2606.21678) — Binds a natural-language rationale to a programmatic verifier. Ananthula et al., 2026.
- [Hilbert: Recursively Building Formal Proofs with Informal Reasoning](https://arxiv.org/abs/2509.22819) — Informal reasoning grows, recursively, into a formal proof. Varambally, Voice et al., ICLR 2026.
- [Mathesis: Towards Formal Theorem Proving from Natural Languages](https://arxiv.org/abs/2506.07047) — From natural language to a formal proof. Yu, Zhong et al., ICLR 2026.
- [LoC-Decomp: LLM Autoformalization via Logical Concept Decomposition and Iterative Feedback Correction](https://iclr.cc/virtual/2026/poster/10011947) — Decomposes a statement into logical concepts, formalizes the pieces, then revises from feedback. ICLR 2026.
- [Numina-Lean-Agent: An Open and General Agentic Reasoning System for Formal Mathematics](https://arxiv.org/abs/2601.14027) — An agentic prover for formal mathematics. Liu, Zhou et al., ICML 2026.
- [LAMP: Lean-based Agentic framework with MCP and Proof Repair](https://arxiv.org/abs/2606.28841) — Repairs a proof after Lean rejects it. Srinivasan and Patawar, 2026.
- [Pythagoras-Prover: Advancing Efficient Formal Proving via Augmented Lean Formalisation](https://arxiv.org/abs/2606.12594) — Puts the work into the Lean formalization step. Leang et al., 2026.
- [Training the Orchestrator: A Supervised Approach to End-to-End PDDL Planning with LLM Agents](https://arxiv.org/abs/2606.21740) — Natural language goes into PDDL. A failed validator sends the specification back for repair. Mangannavar, Coalson, Dugar, and Tadepalli, 2026.
- [UniDomain: Pretraining a Unified PDDL Domain from Real-World Demonstrations for Generalizable Robot Task Planning](https://neurips.cc/virtual/2025/poster/116644) — Learns one shared PDDL domain from demonstrations. NeurIPS 2025.
- [Natural Language PDDL (NL-PDDL) for Open-world Goal-oriented Commonsense Regression Planning in Embodied AI](https://iclr.cc/virtual/2026/poster/10007784) — Open-world planning in a natural-language form of PDDL. ICLR 2026.
- [DecoVer: A Decompose-and-Verify Neuro-Symbolic Framework for Embodied Task Planning with BC+](https://icml.cc/virtual/2026/poster/63653) — Splits a plan, then checks the pieces in a symbolic layer. ICML 2026.
- [APOLLO: Automated LLM and Lean Collaboration for Advanced Formal Reasoning](https://arxiv.org/abs/2505.05758) — An LLM and Lean pass the problem back and forth. Ospanov, Farnia et al., NeurIPS 2025.
- [LeanDojo: Theorem Proving with Retrieval-Augmented Language Models](https://arxiv.org/abs/2306.15626) — A Lean interaction environment and a retrieval prover. Later proof agents still train on this data. Yang, Swope, Gu et al., 2023. [Code](https://github.com/lean-dojo/LeanDojo)
- [OptimAI: Optimization from Natural Language Using LLM-Powered AI Agents](https://arxiv.org/abs/2504.16918) — Agents turn a word problem into a mathematical optimization model and solver code. The solver returns the answer, and a code critic checks each run. Thind, Sun, Liang, and Yang, 2025.

### Programs

- [Forethought: Verifiable Reasoning from Neurosymbolic Primitive Programming](https://arxiv.org/abs/2607.04096) — Writes the reasoning as executable neurosymbolic primitives. Bhat, Vaghasiya, and Gonzalez, 2026.
- [IVIE: A Neuro-symbolic Approach to Incremental and Validated Generation of Interactive Fiction Worlds](https://arxiv.org/abs/2606.13348) — The model generates the world. A symbolic check keeps it consistent. Vaucher et al., 2026.
- [VeriThoughts: Enabling Automated Verilog Code Generation using Reasoning and Formal Verification](https://arxiv.org/abs/2505.20302) — Generates Verilog. Formal verification decides whether to keep it. Yubeaton, Nakkab et al., NeurIPS 2025.
- [VADAOrchestra: Neurosymbolic Orchestration of Adaptive Reasoning Workflows](https://arxiv.org/abs/2606.22485) — The workflow can change. The execution record stays auditable. Baldazzi et al., 2026.
- [NeSyPr: Neurosymbolic Proceduralization For Efficient Embodied Reasoning](https://arxiv.org/abs/2510.19429) — Turns embodied reasoning into a procedure. Choi, Kim et al., NeurIPS 2025.
- [Towards Reliable Code-as-Policies: A Neuro-Symbolic Framework for Embodied Task Planning](https://arxiv.org/abs/2510.21302) — Generated control code carries symbolic constraints that can be checked. Ahn, Choi et al., NeurIPS 2025.
- [Neuro-Symbolic Drive: Rule-Grounded Faithful Reasoning for Driving VLAs](https://arxiv.org/abs/2606.23938) — Driving rationales are grounded in rules, so they line up with the actions. Gao, Huang, Lu, and Zhang, 2026.
- [AutoNumerics: An Autonomous, PDE-Agnostic Multi-Agent Pipeline for Scientific Computing](https://arxiv.org/abs/2602.17607) — The model writes a classical numerical PDE solver as a program. The program runs, and its residual decides whether to keep the solver or start over. Du, Sun, and Yang, 2026. [Code](https://github.com/Daviddjddu/Autonumerics)

### Search

- [Think-on-Graph 2.0: Deep and Faithful Large Language Model Reasoning with Knowledge-guided Retrieval Augmented Generation](https://arxiv.org/abs/2407.10805) — Alternates between the graph and retrieved documents. Ma, Xu, Jiang et al., 2024.
- [CLAUSE: Agentic Neuro-Symbolic Knowledge Graph Reasoning via Dynamic Learnable Context Engineering](https://arxiv.org/abs/2509.21035) — Search on a knowledge graph, with a context that is itself learned. Zhao, Dai et al., ICLR 2026.
- [KARMA: Knowledge graph-based Automated Reasoning Materialization and Alignment](https://arxiv.org/abs/2607.03166) — Materializes the result of graph reasoning and aligns it. Choi et al., 2026.
- [Language models guide symbolic equation discovery by controlling search](https://arxiv.org/abs/2607.04156) — The model controls a symbolic equation search. 2026.
- [AutoSR: Automatic Symbolic Regression by Searching Research States](https://arxiv.org/abs/2608.16876) — Each candidate equation sits in a research state with its motivation, probes, failed attempts, and review. LLM proposers and reviewers grow these states in a tree search. Zhang, Sun, Ren et al., 2026. [Code](https://github.com/AutoResearch-Factory/AutoSR)

Think-on-Graph (2023) and Reasoning on Graphs (2023) are the earlier systems in this section. [ToG code](https://github.com/GasolSun36/ToG). [RoG code](https://github.com/RManLuo/reasoning-on-graphs).

## Joint training

The symbolic layer stays inside training or inference.

### Symbolic intermediate form

- [Logic-of-Thought: Injecting Logic into Contexts for Full Reasoning in Large Language Models](https://arxiv.org/abs/2409.17539) — Propositional logic fills in implied conclusions, and those conclusions go back into the context. Liu et al., 2024.

### Differentiable symbolic layers

Logic or a program sits in the computation graph, so the gradient and the symbolic constraint move together.

- [DeepProbLog: Neural Probabilistic Logic Programming](https://arxiv.org/abs/1805.10872) — A neural network is a predicate inside a probabilistic logic program. Manhaeve et al., 2018. [Code](https://github.com/ML-KULeuven/deepproblog)
- [The Neuro-Symbolic Concept Learner](https://arxiv.org/abs/1904.12584) — Learns visual concepts and symbolic programs together from images, questions, and answers. Mao et al., 2019.
- [DreamCoder: Growing generalizable, interpretable knowledge with wake-sleep Bayesian program learning](https://arxiv.org/abs/2006.08381) — Writes programs and compresses the repeated parts into a library. Ellis et al., 2020.
- [Scallop: A Language for Neurosymbolic Programming](https://arxiv.org/abs/2304.04812) — Differentiable Datalog with a tunable proof granularity. Li et al., 2023. [Code](https://github.com/scallop-lang/scallop)
- [NeurASP: Embracing Neural Networks into Answer Set Programming](https://arxiv.org/abs/2307.07700) — The network emits probabilistic facts. ASP assembles the constraints. Yang et al., 2023.
- [The DeepLog Neurosymbolic Machine](https://arxiv.org/abs/2508.13697) — Connects several logics and neural modules through algebraic circuits on a GPU. Derkinderen et al., 2025. [Code](https://github.com/ML-KULeuven/deeplog)
- [DeepSWIP: Quotient-WMC Counterfactuals for Neural Probabilistic Logic Programs](https://arxiv.org/abs/2606.20526) — Adds a counterfactual semantics to DeepProbLog-style programs. Habib, Belle, and He, 2026.
- [Differentiate the Evaluator, Not the Program: An Efficient Runtime Representation for Neuro-Symbolic Learning](https://arxiv.org/abs/2607.03574) — During program search, the gradient goes through the evaluator. Sheneman, 2026.

### Symbolic feedback

Accept and reject signals from a solver, an interpreter, or an environment update a policy, a skill, or a lemma.

- [Olympiad-level formal mathematical reasoning with reinforcement learning](https://doi.org/10.1038/s41586-025-09833-y) — AlphaProof. Reinforcement learning inside Lean. Hubert et al., Nature 2025.
- [Mathematical discoveries from program search with large language models](https://doi.org/10.1038/s41586-023-06924-6) — FunSearch. The model proposes programs. An evaluator selects them. Romera-Paredes et al., Nature 2024. [Code](https://github.com/google-deepmind/funsearch)
- [AlphaEvolve: A coding agent for scientific and algorithmic discovery](https://arxiv.org/abs/2506.13131) — Evaluated program search over a whole codebase. Novikov et al., 2025.
- [DeepSeek-Prover-V1.5: Harnessing Proof Assistant Feedback for Reinforcement Learning and Monte-Carlo Tree Search](https://arxiv.org/abs/2408.08152) — Proof-assistant verdicts enter reinforcement learning and tree search. Xin, Ren, Song et al., 2024. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V1.5)
- [DeepSeek-Prover-V2: Advancing Formal Mathematical Reasoning via Reinforcement Learning for Subgoal Decomposition](https://arxiv.org/abs/2504.21801) — Decomposes into subgoals, then trains on prover feedback. Ren, Shao, Song et al., 2025. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V2)
- [Goedel-Prover-V2: Scaling Formal Theorem Proving with Scaffolded Data Synthesis and Self-Correction](https://arxiv.org/abs/2508.03613) — A verifier marks the error, and the model revises. Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover-V2)
- [Kimina-Prover Preview: Towards Large Formal Reasoning Models with Reinforcement Learning](https://arxiv.org/abs/2504.11354) — Reinforcement learning for formal reasoning. Wang, Unsal, Lin et al., 2025. [Code](https://github.com/MoonshotAI/Kimina-Prover-Preview)
- [Distilling LLM Feedback for Lean Theorem Proving](https://arxiv.org/abs/2605.30861) — Distills feedback on proof attempts into the model. Lean still checks. Narozniak et al., 2026.
- [Symbolic Learning Enables Self-Evolving Agents](https://arxiv.org/abs/2406.18532) — Treats prompts, tools, and the pipeline as symbolic parameters that can be updated. Zhou, Ou, Ding et al., 2024. [Code](https://github.com/aiwaves-cn/agents)
- [Lifting Traces to Logic: Programmatic Skill Induction with Neuro-Symbolic Learning for Long-Horizon Agentic Tasks](https://arxiv.org/abs/2605.01293) — Lifts long-horizon traces into executable logical skills. Shao, Yin et al., ICML 2026.
- [GLARE: Scalable Neuro-Symbolic Reward Shaping for LLM Agents via Group-Level Automata](https://icml.cc/virtual/2026/poster/66450) — Shapes the agent's reward with automata constraints. ICML 2026.
- [WALL-E: World Alignment by Rule Learning Improves World Model-based LLM Agents](https://arxiv.org/abs/2410.07484) — Learned rules correct the world model. Zhou, Zhou et al., 2024.
- [Skill-Driven Neurosymbolic State Abstractions](https://neurips.cc/virtual/2025/poster/116718) — Skills decide how states are abstracted into symbols. NeurIPS 2025.
- [MINT: Minimal Information Neuro-Symbolic Tree for Objective-Driven Knowledge-Gap Reasoning and Active Elicitation](https://arxiv.org/abs/2602.05048) — A neurosymbolic tree marks the missing information, then asks for it. Fang, Imani et al., ICML 2026.
- [Influence-Guided Symbolic Regression: Scientific Discovery via LLM-Driven Equation Search with Granular Feedback](https://icml.cc/virtual/2026/poster/60987) — Equation search proceeds on fine-grained feedback. ICML 2026.
- [Deliberate Evolution for Sample-Efficient Symbolic Regression with LLM](https://icml.cc/virtual/2026/poster/66263) — Evolutionary search for symbolic regression. ICML 2026.
- [LLM Library Learning Fails: A LEGO-Prover Case Study](https://arxiv.org/abs/2504.03048) — After matching the search budget, checks whether the lemma library is actually reused. Berlot-Attwell, Rudzicz, and Si, 2025.

## Benchmarks

- [PutnamBench: Evaluating Neural Theorem-Provers on the Putnam Mathematical Competition](https://arxiv.org/abs/2407.11214) — Formal evaluation on the Putnam. Tsoukalas, Lee, Jennings et al., 2024.
- [GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://arxiv.org/abs/2410.05229) — Rewrites grade-school math problems from symbolic templates. Mirzadeh et al., 2024.
- [NATURAL PLAN: Benchmarking LLMs on Natural Language Planning](https://arxiv.org/abs/2406.04520) — Trip, calendar, and meeting planning, with symbolic constraints. Zheng, Mishra, Zhang et al., 2024.
- [Socrates or Smartypants: Testing Logic Reasoning Capabilities of Large Language Models with Logic Programming-Based Test Oracles](https://ojs.aaai.org/index.php/AAAI/article/view/39021) — Logic programs as test oracles. AAAI 2026.
- [QMFOL: Benchmarking Large Language Model Reasoning via Quantifiable Monadic First-Order Logic Test Case Generation](https://arxiv.org/abs/2606.20227) — First-order logic test cases. Zheng et al., 2026.
- [Evaluating the Robustness of Proof Autoformalization in Lean 4](https://arxiv.org/abs/2606.14867) — Whether a Lean 4 formalization still holds under perturbation. Gui et al., 2026.
- [FormalRx: Rectify and eXamine Semantic Failures in Autoformalization](https://arxiv.org/abs/2607.04655) — Locates semantic failures in autoformalization. Wang, Huang, Wan, and Zhu, 2026.
- [Faults in Our Formal Benchmarking: Dataset Defects and Evaluation Failures in Lean Theorem Proving](https://arxiv.org/abs/2606.29493) — Defects in the formal evaluation data itself. Ammanamanchi et al., 2026. [Poster](https://icml.cc/virtual/2026/poster/62980)
- [The Signal-Coverage Matrix: Stratifying Type and Semantic Errors in Statement Autoformalization](https://arxiv.org/abs/2606.28013) — Splits autoformalization errors into type errors and semantic errors. Dai et al., 2026.
- [NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry](https://arxiv.org/abs/2608.28481) — Whether a geometry problem can be written in AlphaGeometry's language. Xiao, Song, and Hu, 2026.
- [FVSpec: Real-World Property-Based Tests as Lean Challenges](https://arxiv.org/abs/2606.01008) — Turns property-based tests into Lean problems. Dougherty et al., 2026.
- [MA-ProofBench: A Two-Tiered Evaluation of LLMs for Theorem Proving in Mathematical Analysis](https://arxiv.org/abs/2606.13782) — Proofs in mathematical analysis. Pu et al., 2026.

## Other lists

- [Awesome-LLM-Reasoning-with-NeSy](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy) — Paper table accompanying the Yang et al. survey.
- [awesome-deeplogic](https://github.com/ccclyu/awesome-deeplogic) — Logic inside neural networks, mostly NLP.
- [awesome-neurosymbolic-ai](https://github.com/sciknoworg/awesome-neurosymbolic-ai) — Neuro-symbolic evaluation resources.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[CC0 1.0](LICENSE). Copyright in the papers stays with the authors and publishers.
