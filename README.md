# Awesome 神经符号学习 · Agent 时代

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 神经网络和符号对象放在一起学：逻辑、程序、证明、PDDL、图谱路径、技能库、引理库。

分三节，依据是 Yang 等人 IJCAI 2025 的综述（[论文](https://arxiv.org/abs/2508.13678)，[论文表](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy)）。

1. **符号到模型。** 符号系统提供数据或教师信号，模型学进参数。
2. **模型到符号。** 模型写成逻辑、程序或证明，外部系统执行。
3. **两边一起。** 符号留在计算图里，或求解器的对错进入训练。

## 目录

- [综述](#综述)
- [符号到模型](#符号到模型)
- [模型到符号](#模型到符号)
- [两边一起](#两边一起)
- [基准](#基准)
- [其他清单](#其他清单)
- [贡献](#贡献)

## 综述

- [Neuro-Symbolic Artificial Intelligence: Towards Improving the Reasoning Abilities of Large Language Models](https://arxiv.org/abs/2508.13678) — 本清单的分类来源。符号到 LLM、LLM 到符号、两边一起。Yang et al., IJCAI 2025.
- [Neuro-Symbolic AI in 2024: A Systematic Review](https://arxiv.org/abs/2501.05435) — 按知识表示、学习与推理、逻辑、可解释性、元认知统计 2020–2024 的工作。学习与推理最多，元认知很少。Colelough and Regli, 2025.
- [Converging Paradigms: The Synergy of Symbolic and Connectionist AI in LLM-Empowered Autonomous Agents](https://arxiv.org/abs/2407.08516) — 讨论语言智能体里神经网络、工作流和符号约束各管哪一段。Xiong et al., 2024.
- [A Survey on Symbolic Knowledge Distillation of Large Language Models](https://arxiv.org/abs/2408.10210) — 从模型里抽出符号结构。Acharya, Velasquez, and Song, 2024.
- [Neuro-Symbolic Agents for Regulated Process Automation: Challenges and Research Agenda](https://arxiv.org/abs/2606.13405) — 受监管流程里，符号层要能审计。Rombach et al., 2026.

## 符号到模型

符号系统提供教师信号，模型学进参数。

### 写进参数

训练时看符号知识或显式步骤，推理时只看问题。

- [SKIntern: Internalizing Symbolic Knowledge for Distilling Better CoT Capabilities into Small Language Models](https://arxiv.org/abs/2409.13183) — 先把符号知识摘要和示例放进提示，再按课程删掉，小模型只看问题也能写出推理。Liao, He, Hao et al., COLING 2025. [代码](https://github.com/Xnhyacinth/SKIntern)
- [From Explicit CoT to Implicit CoT: Learning to Internalize CoT Step by Step](https://arxiv.org/abs/2405.14838) — 逐步去掉思维链 token，中间步骤进入隐状态。Deng, Choi, and Shieber, 2024. [代码](https://github.com/da03/Internalize_CoT_Step_by_Step)
- [Neural-Symbolic Collaborative Distillation: Advancing Small Language Models for Complex Reasoning Tasks](https://arxiv.org/abs/2409.13203) — 常见推理进参数，稀缺知识仍放在外部知识库里检索。Liao, He, Xu et al., AAAI 2025. [代码](https://github.com/Xnhyacinth/NesyCD)
- [Scaling Neuro-symbolic Problem Solving: Solver-Free Learning of Constraints and Objectives](https://arxiv.org/abs/2508.20978) — 训练时不接求解器，直接学组合问题的约束和目标。Defresne, Gambardella, and Barbe, 2025.
- [Transformers as Soft Reasoners over Language](https://arxiv.org/abs/2002.05867) — 用符号生成的规则数据训练，Transformer 自己做多步推理。Clark, Tafjord, and Richardson, 2020.

### 模仿符号轨迹

训练轨迹来自符号引擎。测试时引擎负责检查。

- [Solving olympiad geometry without human demonstrations](https://doi.org/10.1038/s41586-023-06747-5) — AlphaGeometry。语言模型在合成证明上训练，测试时仍把辅助构造交给符号演绎引擎。Trinh et al., Nature 2024. [代码](https://github.com/google-deepmind/alphageometry)
- [Gold-medalist Performance in Solving Olympiad Geometry with AlphaGeometry2](https://arxiv.org/abs/2502.03544) — 扩大形式语言和搜索。Chervonyi, Trinh, Olšák et al., 2025. [代码](https://github.com/google-deepmind/alphageometry2)
- [Wu's Method can Boost Symbolic AI to Rival Silver Medalists and AlphaGeometry to Outperform Gold Medalists at IMO Geometry](https://arxiv.org/abs/2404.06405) — 几何后端换成吴方法。Sinha et al., 2024.
- [DeepSeek-Prover: Advancing Theorem Proving in LLMs through Large-Scale Synthetic Data](https://arxiv.org/abs/2405.14333) — 用合成 Lean 证明做监督。后面的 V1.5、V2 把证明器反馈加进强化学习，放在[符号反馈](#符号反馈)。Xin, Guo, Shao et al., 2024.
- [Goedel-Prover: A Frontier Model for Open-Source Automated Theorem Proving](https://arxiv.org/abs/2502.07640) — 用合成语句和证明训练的开源 Lean 模型。V2 放在符号反馈。Lin, Tang, Lyu et al., 2025. [代码](https://github.com/Goedel-LM/Goedel-Prover)
- [EvolProver: Advancing Automated theorem proving by Evolving Formalized Problems via Symmetry and Difficulty](https://iclr.cc/virtual/2026/poster/10008534) — 沿对称性和难度改已形式化的题，用来训练证明器。ICLR 2026.

## 模型到符号

模型写成逻辑、程序或证明，求解器、解释器或搜索过程负责算出结果。

### 交给求解器

- [Faithful Logical Reasoning via Symbolic Chain-of-Thought](https://arxiv.org/abs/2405.18357) — SymbCoT。推理链里保留符号表达式和验证。Xu, Fei, Pan et al., 2024.
- [VeriCoT: Neuro-symbolic Chain-of-Thought Validation via Logical Consistency Checks](https://arxiv.org/abs/2511.04662) — 把思维链翻成可做一致性检查的逻辑。Feng, Weir et al., ICLR 2026.
- [FormalJudge: A Neuro-Symbolic Paradigm for Agentic Oversight](https://arxiv.org/abs/2602.11136) — 用形式判断看住智能体的输出。Zhou, Sheng et al., ICML 2026.
- [PrologMCP: A Standardized Prolog Tool Interface for LLM Agents](https://arxiv.org/abs/2606.14935) — 演绎交给 Prolog，模型负责翻译。Mensfelt et al., 2026.
- [Decodable but Not Faithful: Coupling Natural-Language Rationales to Programmatic Verifiers](https://arxiv.org/abs/2606.21678) — 自然语言理由要绑到程序验证器上。Ananthula et al., 2026.
- [Hilbert: Recursively Building Formal Proofs with Informal Reasoning](https://arxiv.org/abs/2509.22819) — 非正式推理递归长成形式证明。Varambally, Voice et al., ICLR 2026.
- [Mathesis: Towards Formal Theorem Proving from Natural Languages](https://arxiv.org/abs/2506.07047) — 从自然语言到形式证明。Yu, Zhong et al., ICLR 2026.
- [LoC-Decomp: LLM Autoformalization via Logical Concept Decomposition and Iterative Feedback Correction](https://iclr.cc/virtual/2026/poster/10011947) — 按逻辑概念分解后做形式化，再按反馈改。ICLR 2026.
- [Numina-Lean-Agent: An Open and General Agentic Reasoning System for Formal Mathematics](https://arxiv.org/abs/2601.14027) — 形式数学上的智能体证明系统。Liu, Zhou et al., ICML 2026.
- [LAMP: Lean-based Agentic framework with MCP and Proof Repair](https://arxiv.org/abs/2606.28841) — Lean 拒绝以后再修证明。Srinivasan and Patawar, 2026.
- [Pythagoras-Prover: Advancing Efficient Formal Proving via Augmented Lean Formalisation](https://arxiv.org/abs/2606.12594) — 加强 Lean 形式化这一步。Leang et al., 2026.
- [Training the Orchestrator: A Supervised Approach to End-to-End PDDL Planning with LLM Agents](https://arxiv.org/abs/2606.21740) — 自然语言进 PDDL，验证器不通过就修规格。Mangannavar, Coalson, Dugar, and Tadepalli, 2026.
- [UniDomain: Pretraining a Unified PDDL Domain from Real-World Demonstrations for Generalizable Robot Task Planning](https://neurips.cc/virtual/2025/poster/116644) — 从演示里学一个共用的 PDDL domain。NeurIPS 2025.
- [Natural Language PDDL (NL-PDDL) for Open-world Goal-oriented Commonsense Regression Planning in Embodied AI](https://iclr.cc/virtual/2026/poster/10007784) — 用自然语言形式的 PDDL 做开放世界规划。ICLR 2026.
- [DecoVer: A Decompose-and-Verify Neuro-Symbolic Framework for Embodied Task Planning with BC+](https://icml.cc/virtual/2026/poster/63653) — 计划先拆开，再用符号层验证。ICML 2026.
- [APOLLO: Automated LLM and Lean Collaboration for Advanced Formal Reasoning](https://arxiv.org/abs/2505.05758) — LLM 和 Lean 来回协作。Ospanov, Farnia et al., NeurIPS 2025.
- [LeanDojo: Theorem Proving with Retrieval-Augmented Language Models](https://arxiv.org/abs/2306.15626) — Lean 交互环境和检索式证明器。后面很多证明智能体还在用这套数据。Yang, Swope, Gu et al., 2023. [代码](https://github.com/lean-dojo/LeanDojo)

### 交给程序

- [Forethought: Verifiable Reasoning from Neurosymbolic Primitive Programming](https://arxiv.org/abs/2607.04096) — 用神经符号原语写可执行的推理。Bhat, Vaghasiya, and Gonzalez, 2026.
- [IVIE: A Neuro-symbolic Approach to Incremental and Validated Generation of Interactive Fiction Worlds](https://arxiv.org/abs/2606.13348) — 生成交给模型，世界是否自洽交给符号检查。Vaucher et al., 2026.
- [VeriThoughts: Enabling Automated Verilog Code Generation using Reasoning and Formal Verification](https://arxiv.org/abs/2505.20302) — 生成 Verilog，由形式验证决定是否采纳。Yubeaton, Nakkab et al., NeurIPS 2025.
- [VADAOrchestra: Neurosymbolic Orchestration of Adaptive Reasoning Workflows](https://arxiv.org/abs/2606.22485) — 工作流可以改，执行记录要能审计。Baldazzi et al., 2026.
- [NeSyPr: Neurosymbolic Proceduralization For Efficient Embodied Reasoning](https://arxiv.org/abs/2510.19429) — 把具身推理写成过程，少做逐步的语言搜索。Choi, Kim et al., NeurIPS 2025.
- [Towards Reliable Code-as-Policies: A Neuro-Symbolic Framework for Embodied Task Planning](https://arxiv.org/abs/2510.21302) — 生成的控制代码带上可检查的符号约束。Ahn, Choi et al., NeurIPS 2025.
- [Neuro-Symbolic Drive: Rule-Grounded Faithful Reasoning for Driving VLAs](https://arxiv.org/abs/2606.23938) — 驾驶理由要落到规则上，才能和动作对上。Gao, Huang, Lu, and Zhang, 2026.

### 搜索

- [Think-on-Graph 2.0: Deep and Faithful Large Language Model Reasoning with Knowledge-guided Retrieval Augmented Generation](https://arxiv.org/abs/2407.10805) — 在图谱和文档之间交替检索。Ma, Xu, Jiang et al., 2024.
- [CLAUSE: Agentic Neuro-Symbolic Knowledge Graph Reasoning via Dynamic Learnable Context Engineering](https://arxiv.org/abs/2509.21035) — 在知识图谱上搜索，上下文可以学。Zhao, Dai et al., ICLR 2026.
- [KARMA: Knowledge graph-based Automated Reasoning Materialization and Alignment](https://arxiv.org/abs/2607.03166) — 把图谱推理的结果物化并对齐。Choi et al., 2026.
- [Language models guide symbolic equation discovery by controlling search](https://arxiv.org/abs/2607.04156) — 模型不直接写出方程，而是控制符号搜索。2026.

Think-on-Graph（2023）和 Reasoning on Graphs（2023）是这一节的前身。[ToG 代码](https://github.com/GasolSun36/ToG)，[RoG 代码](https://github.com/RManLuo/reasoning-on-graphs)。

## 两边一起

符号层留在训练或推理过程里。

### 符号当中间表示

- [Logic-of-Thought: Injecting Logic into Contexts for Full Reasoning in Large Language Models](https://arxiv.org/abs/2409.17539) — 用命题逻辑把隐含结论补进上下文，再交给模型。Liu et al., 2024.

### 可微符号层

逻辑或程序在计算图里，梯度和符号约束一起走。

- [DeepProbLog: Neural Probabilistic Logic Programming](https://arxiv.org/abs/1805.10872) — 神经网络作为概率逻辑程序里的谓词。Manhaeve et al., 2018. [代码](https://github.com/ML-KULeuven/deepproblog)
- [The Neuro-Symbolic Concept Learner](https://arxiv.org/abs/1904.12584) — 从图像、问题和答案里一起学视觉概念和符号程序。Mao et al., 2019.
- [DreamCoder: Growing generalizable, interpretable knowledge with wake-sleep Bayesian program learning](https://arxiv.org/abs/2006.08381) — 写程序，并把重复的部分压成库。Ellis et al., 2020.
- [Scallop: A Language for Neurosymbolic Programming](https://arxiv.org/abs/2304.04812) — 可微 Datalog，证明粒度可以调。Li et al., 2023. [代码](https://github.com/scallop-lang/scallop)
- [NeurASP: Embracing Neural Networks into Answer Set Programming](https://arxiv.org/abs/2307.07700) — 网络给出概率事实，ASP 组合约束。Yang et al., 2023.
- [The DeepLog Neurosymbolic Machine](https://arxiv.org/abs/2508.13697) — 用代数电路把多种逻辑和神经模块接到 GPU 上。Derkinderen et al., 2025. [代码](https://github.com/ML-KULeuven/deeplog)
- [DeepSWIP: Quotient-WMC Counterfactuals for Neural Probabilistic Logic Programs](https://arxiv.org/abs/2606.20526) — 给 DeepProbLog 补反事实语义。Habib, Belle, and He, 2026.
- [Differentiate the Evaluator, Not the Program: An Efficient Runtime Representation for Neuro-Symbolic Learning](https://arxiv.org/abs/2607.03574) — 程序搜索里，梯度走评估器。Sheneman, 2026.

### 符号反馈

求解器、解释器或环境的对错，用来更新策略、技能或引理。

- [Olympiad-level formal mathematical reasoning with reinforcement learning](https://doi.org/10.1038/s41586-025-09833-y) — AlphaProof。Lean 里的强化学习。Hubert et al., Nature 2025.
- [Mathematical discoveries from program search with large language models](https://doi.org/10.1038/s41586-023-06924-6) — FunSearch。模型提程序，评估器做选择。Romera-Paredes et al., Nature 2024. [代码](https://github.com/google-deepmind/funsearch)
- [AlphaEvolve: A coding agent for scientific and algorithmic discovery](https://arxiv.org/abs/2506.13131) — 在整个代码库上做带评估的程序搜索。Novikov et al., 2025.
- [DeepSeek-Prover-V1.5: Harnessing Proof Assistant Feedback for Reinforcement Learning and Monte-Carlo Tree Search](https://arxiv.org/abs/2408.08152) — 证明助手的对错进入强化学习和树搜索。Xin, Ren, Song et al., 2024. [代码](https://github.com/deepseek-ai/DeepSeek-Prover-V1.5)
- [DeepSeek-Prover-V2: Advancing Formal Mathematical Reasoning via Reinforcement Learning for Subgoal Decomposition](https://arxiv.org/abs/2504.21801) — 子目标分解，再用证明器反馈训练。Ren, Shao, Song et al., 2025. [代码](https://github.com/deepseek-ai/DeepSeek-Prover-V2)
- [Goedel-Prover-V2: Scaling Formal Theorem Proving with Scaffolded Data Synthesis and Self-Correction](https://arxiv.org/abs/2508.03613) — 验证器指出错误后再改。Lin, Tang, Lyu et al., 2025. [代码](https://github.com/Goedel-LM/Goedel-Prover-V2)
- [Kimina-Prover Preview: Towards Large Formal Reasoning Models with Reinforcement Learning](https://arxiv.org/abs/2504.11354) — 形式推理上的强化学习。Wang, Unsal, Lin et al., 2025. [代码](https://github.com/MoonshotAI/Kimina-Prover-Preview)
- [Distilling LLM Feedback for Lean Theorem Proving](https://arxiv.org/abs/2605.30861) — 把对证明尝试的反馈蒸馏进模型。检查仍由 Lean 做。Narozniak et al., 2026.
- [Symbolic Learning Enables Self-Evolving Agents](https://arxiv.org/abs/2406.18532) — 提示、工具和流水线当成可更新的符号参数。Zhou, Ou, Ding et al., 2024. [代码](https://github.com/aiwaves-cn/agents)
- [Lifting Traces to Logic: Programmatic Skill Induction with Neuro-Symbolic Learning for Long-Horizon Agentic Tasks](https://arxiv.org/abs/2605.01293) — 从长时程轨迹里抽出可执行的逻辑技能。Shao, Yin et al., ICML 2026.
- [GLARE: Scalable Neuro-Symbolic Reward Shaping for LLM Agents via Group-Level Automata](https://icml.cc/virtual/2026/poster/66450) — 用自动机约束给智能体做奖励。ICML 2026.
- [WALL-E: World Alignment by Rule Learning Improves World Model-based LLM Agents](https://arxiv.org/abs/2410.07484) — 用学到的规则修正世界模型。Zhou, Zhou et al., 2024.
- [Skill-Driven Neurosymbolic State Abstractions](https://neurips.cc/virtual/2025/poster/116718) — 技能决定状态怎么抽象成符号。NeurIPS 2025.
- [MINT: Minimal Information Neuro-Symbolic Tree for Objective-Driven Knowledge-Gap Reasoning and Active Elicitation](https://arxiv.org/abs/2602.05048) — 用神经符号树标出还缺的信息，再去问。Fang, Imani et al., ICML 2026.
- [Influence-Guided Symbolic Regression: Scientific Discovery via LLM-Driven Equation Search with Granular Feedback](https://icml.cc/virtual/2026/poster/60987) — 方程搜索用细粒度反馈往下走。ICML 2026.
- [Deliberate Evolution for Sample-Efficient Symbolic Regression with LLM](https://icml.cc/virtual/2026/poster/66263) — 符号回归里的进化搜索。ICML 2026.
- [LLM Library Learning Fails: A LEGO-Prover Case Study](https://arxiv.org/abs/2504.03048) — 对齐搜索预算之后，检查引理库是否真被复用。Berlot-Attwell, Rudzicz, and Si, 2025.

## 基准

- [PutnamBench: Evaluating Neural Theorem-Provers on the Putnam Mathematical Competition](https://arxiv.org/abs/2407.11214) — Putnam 的形式化评测。Tsoukalas, Lee, Jennings et al., 2024.
- [GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://arxiv.org/abs/2410.05229) — 用符号模板改数学题，看模型是记住了题还是会算。Mirzadeh et al., 2024.
- [NATURAL PLAN: Benchmarking LLMs on Natural Language Planning](https://arxiv.org/abs/2406.04520) — 行程、日历、会议。约束是符号的。Zheng, Mishra, Zhang et al., 2024.
- [Socrates or Smartypants: Testing Logic Reasoning Capabilities of Large Language Models with Logic Programming-Based Test Oracles](https://ojs.aaai.org/index.php/AAAI/article/view/39021) — 用逻辑程序当测试预言。AAAI 2026.
- [QMFOL: Benchmarking Large Language Model Reasoning via Quantifiable Monadic First-Order Logic Test Case Generation](https://arxiv.org/abs/2606.20227) — 一阶逻辑测试用例。Zheng et al., 2026.
- [Evaluating the Robustness of Proof Autoformalization in Lean 4](https://arxiv.org/abs/2606.14867) — 形式化在扰动下还成不成立。Gui et al., 2026.
- [FormalRx: Rectify and eXamine Semantic Failures in Autoformalization](https://arxiv.org/abs/2607.04655) — 定位自动形式化里的语义错误。Wang, Huang, Wan, and Zhu, 2026.
- [Faults in Our Formal Benchmarking: Dataset Defects and Evaluation Failures in Lean Theorem Proving](https://arxiv.org/abs/2606.29493) — 形式化评测数据本身的问题。Ammanamanchi et al., 2026. [会议页](https://icml.cc/virtual/2026/poster/62980)
- [The Signal-Coverage Matrix: Stratifying Type and Semantic Errors in Statement Autoformalization](https://arxiv.org/abs/2606.28013) — 把形式化错误分成类型错误和语义错误。Dai et al., 2026.
- [NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry](https://arxiv.org/abs/2608.28481) — 几何题能不能写成 AlphaGeometry 的语言。Xiao, Song, and Hu, 2026.
- [FVSpec: Real-World Property-Based Tests as Lean Challenges](https://arxiv.org/abs/2606.01008) — 基于性质的测试转成 Lean 题。Dougherty et al., 2026.
- [MA-ProofBench: A Two-Tiered Evaluation of LLMs for Theorem Proving in Mathematical Analysis](https://arxiv.org/abs/2606.13782) — 数学分析证明。Pu et al., 2026.

## 其他清单

- [Awesome-LLM-Reasoning-with-NeSy](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy) — 和 Yang 等人综述配套的论文表。
- [awesome-deeplogic](https://github.com/ccclyu/awesome-deeplogic) — 逻辑怎么进神经网络，偏 NLP。
- [awesome-neurosymbolic-ai](https://github.com/sciknoworg/awesome-neurosymbolic-ai) — 神经符号评测。

## 贡献

格式见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可

[CC0 1.0](LICENSE)。论文版权归作者和出版方。
