# Awesome 神经符号学习 · Agent 时代

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 一份面向 agent 时代的神经符号学习清单：神经网络负责提出结构，符号对象负责被执行、被检验，并且能反过来训练或改写模型。
>
> A curated list of neuro-symbolic *learning* after language agents: neural models propose symbolic objects, solvers and environments check them, and the resulting signal updates a policy, a library, or a proof strategy.

Agent 时代的神经符号学习不是“给 LLM 再接一个计算器”。能收进这份清单的工作，至少有一个显式符号对象，并且学习或推理会经过它。符号对象包括逻辑公式、程序、证明、PDDL 算子、知识图谱上的关系路径，以及可复用的技能库或引理库。

常见的是三条回路：

1. **符号产生监督。** 求解器、证明助手或合成引擎造出可验证轨迹，神经网络学习的是提出构造、引理、子目标或程序补丁。
2. **神经模型编译符号。** 模型把语言或感知译成逻辑、SMT、PDDL、SQL 或 Python，符号执行器负责得到可检验的答案。
3. **闭环改写符号对象。** 智能体把环境或证明器的失败写回技能库、规划算子、引理库或智能体结构本身。

纯思维链、没有符号对象的工具调用、通用检索增强，不在收录范围内。2024 年之前只保留还在被直接调用的接口，不再按年份铺开。

## 目录

- [综述与问题定义](#综述与问题定义)
- [2024 年之前只留接口](#2024-年之前只留接口)
- [符号引擎训练神经网络](#符号引擎训练神经网络)
- [编译成符号再求解](#编译成符号再求解)
- [形式化证明智能体](#形式化证明智能体)
- [技能库与符号对象更新](#技能库与符号对象更新)
- [规划与 PDDL](#规划与-pddl)
- [知识图谱上的搜索智能体](#知识图谱上的搜索智能体)
- [具身与可执行策略](#具身与可执行策略)
- [语言与框架](#语言与框架)
- [基准](#基准)
- [相关清单](#相关清单)
- [贡献](#贡献)

## 综述与问题定义

- [Neuro-Symbolic AI in 2024: A Systematic Review](https://arxiv.org/abs/2501.05435) — 167 篇系统综述。知识表示和学习/推理是大头，元认知几乎空着。Colelough and Regli, 2025.
- [Neuro-Symbolic Artificial Intelligence: Towards Improving the Reasoning Abilities of Large Language Models](https://arxiv.org/abs/2508.13678) — IJCAI 2025 综述。按 Symbolic→LLM、LLM→Symbolic、LLM+Symbolic 整理求解器、证明搜索和程序解释器。Yang et al., 2025. [论文表](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy)
- [Converging Paradigms: The Synergy of Symbolic and Connectionist AI in LLM-Empowered Autonomous Agents](https://arxiv.org/abs/2407.08516) — 论证语言智能体本身就是神经与符号的汇合：神经网络加工作流、规则、工具和知识图谱。Xiong et al., 2024.
- [Neuro-Symbolic Agents for Regulated Process Automation: Challenges and Research Agenda](https://arxiv.org/abs/2606.13405) — 受监管流程里的神经符号智能体研究议程：符号层要承担可审计的约束，而不是事后解释。Rombach et al., 2026.

2020 年的 The 3rd Wave 和 2023 年的 CoALA 仍是定义和智能体记忆架构的参照，这里不再展开。

## 2024 年之前只留接口

这些系统早于当前这波语言智能体，但后面的工作仍把它们当求解器、编程语言或证明环境。∂ILP、Neural Logic Machines、SATNet、Logic Tensor Networks、LNN、PyReason，以及 PAL、Program-of-Thoughts、Binder、Logic-LM、LINC、SatLM、Code-as-Policies、VisProg、Voyager、LILO，见文末相关清单，不逐条保留。

- [DeepProbLog: Neural Probabilistic Logic Programming](https://arxiv.org/abs/1805.10872) — 神经网络作为概率逻辑程序里的神经谓词。Manhaeve et al., 2018. [Code](https://github.com/ML-KULeuven/deepproblog)
- [The Neuro-Symbolic Concept Learner](https://arxiv.org/abs/1904.12584) — 从图像、问题和答案里联合学习视觉概念和符号程序。Mao et al., 2019.
- [DreamCoder: Growing generalizable, interpretable knowledge with wake-sleep Bayesian program learning](https://arxiv.org/abs/2006.08381) — 醒眠循环里写程序并压缩出可复用库。Ellis et al., 2020.
- [Scallop: A Language for Neurosymbolic Programming](https://arxiv.org/abs/2304.04812) — 可调证明粒度的可微 Datalog。Li et al., 2023. [Code](https://github.com/scallop-lang/scallop)
- [NeurASP: Embracing Neural Networks into Answer Set Programming](https://arxiv.org/abs/2307.07700) — 神经网络输出稳定模型上的概率事实，ASP 负责组合约束。Yang et al., 2023.
- [LeanDojo: Theorem Proving with Retrieval-Augmented Language Models](https://arxiv.org/abs/2306.15626) — Lean 交互环境、带前提标注的证明，以及检索式证明器 ReProver。Yang, Swope, Gu et al., 2023. [Code](https://github.com/lean-dojo/LeanDojo)

## 符号引擎训练神经网络

学习信号来自符号系统。模型提出候选，证明器、解释器或评估器给出对错，再更新策略。

- [Solving olympiad geometry without human demonstrations](https://doi.org/10.1038/s41586-023-06747-5) — AlphaGeometry。语言模型提出辅助构造，符号演绎引擎验证。Trinh et al., Nature 2024. [Code](https://github.com/google-deepmind/alphageometry)
- [Gold-medalist Performance in Solving Olympiad Geometry with AlphaGeometry2](https://arxiv.org/abs/2502.03544) — 扩大形式语言和搜索。Chervonyi, Trinh, Olšák et al., 2025. [Code](https://github.com/google-deepmind/alphageometry2)
- [Wu's Method can Boost Symbolic AI to Rival Silver Medalists and AlphaGeometry to Outperform Gold Medalists at IMO Geometry](https://arxiv.org/abs/2404.06405) — 用吴方法换掉几何符号后端。Sinha et al., 2024.
- [Olympiad-level formal mathematical reasoning with reinforcement learning](https://doi.org/10.1038/s41586-025-09833-y) — AlphaProof。在 Lean 里做强化学习，与 AlphaGeometry 2 一起达到 IMO 2024 银牌线。Hubert et al., Nature 2025.
- [Mathematical discoveries from program search with large language models](https://doi.org/10.1038/s41586-023-06924-6) — FunSearch。LLM 提出程序，自动评估器做进化选择。Romera-Paredes et al., Nature 2024. [Code](https://github.com/google-deepmind/funsearch)
- [AlphaEvolve: A coding agent for scientific and algorithmic discovery](https://arxiv.org/abs/2506.13131) — 把程序搜索扩到整个代码库，用评估级联筛选补丁。Novikov et al., 2025.
- [DeepSeek-Prover: Advancing Theorem Proving in LLMs through Large-Scale Synthetic Data](https://arxiv.org/abs/2405.14333) — 用大规模合成 Lean 证明做监督。Xin, Guo, Shao et al., 2024.
- [DeepSeek-Prover-V1.5: Harnessing Proof Assistant Feedback for Reinforcement Learning and Monte-Carlo Tree Search](https://arxiv.org/abs/2408.08152) — 证明助手的对错进入强化学习和树搜索。Xin, Ren, Song et al., 2024. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V1.5)
- [DeepSeek-Prover-V2: Advancing Formal Mathematical Reasoning via Reinforcement Learning for Subgoal Decomposition](https://arxiv.org/abs/2504.21801) — 子目标分解加上证明器反馈。Ren, Shao, Song et al., 2025. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V2)
- [Goedel-Prover: A Frontier Model for Open-Source Automated Theorem Proving](https://arxiv.org/abs/2502.07640) — 开源 Lean 证明模型。Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover)
- [Goedel-Prover-V2: Scaling Formal Theorem Proving with Scaffolded Data Synthesis and Self-Correction](https://arxiv.org/abs/2508.03613) — 脚手架式数据合成，以及验证器引导的自我修正。Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover-V2)
- [Kimina-Prover Preview: Towards Large Formal Reasoning Models with Reinforcement Learning](https://arxiv.org/abs/2504.11354) — 在形式推理模式上做强化学习。Wang, Unsal, Lin et al., 2025. [Code](https://github.com/MoonshotAI/Kimina-Prover-Preview)
- [APOLLO: Automated LLM and Lean Collaboration for Advanced Formal Reasoning](https://arxiv.org/abs/2505.05758) — LLM 与 Lean 协作做形式推理。Ospanov, Farnia et al., NeurIPS 2025.
- [Language models guide symbolic equation discovery by controlling search](https://arxiv.org/abs/2607.04156) — 语言模型不直接输出方程，而是控制符号方程搜索。2026.

## 编译成符号再求解

模型负责翻译或检查翻译；求解器、证明器或逻辑引擎负责得到可检验的结果。

- [Faithful Logical Reasoning via Symbolic Chain-of-Thought](https://arxiv.org/abs/2405.18357) — SymbCoT。符号表达式、符号计划和验证器留在推理回路里。Xu, Fei, Pan et al., 2024.
- [Logic-of-Thought: Injecting Logic into Contexts for Full Reasoning in Large Language Models](https://arxiv.org/abs/2409.17539) — 用命题逻辑把隐含结论补进上下文。Liu et al., 2024.
- [VeriCoT: Neuro-symbolic Chain-of-Thought Validation via Logical Consistency Checks](https://arxiv.org/abs/2511.04662) — 把思维链翻成可做逻辑一致性检查的形式，用来验收而不是再生成一段话。Feng, Weir et al., ICLR 2026.
- [FormalJudge: A Neuro-Symbolic Paradigm for Agentic Oversight](https://arxiv.org/abs/2602.11136) — 用神经符号判断器监督智能体，而不是只看自然语言理由。Zhou, Sheng et al., ICML 2026.
- [PrologMCP: A Standardized Prolog Tool Interface for LLM Agents](https://arxiv.org/abs/2606.14935) — 把 Prolog 做成语言模型智能体的标准工具接口，演绎交给求解器。Mensfelt et al., 2026.
- [Forethought: Verifiable Reasoning from Neurosymbolic Primitive Programming](https://arxiv.org/abs/2607.04096) — 用神经符号原语编程产生可验证推理，而不是把工具结果堆进上下文。Bhat, Vaghasiya, and Gonzalez, 2026.
- [IVIE: A Neuro-symbolic Approach to Incremental and Validated Generation of Interactive Fiction Worlds](https://arxiv.org/abs/2606.13348) — 语言模型负责生成，符号层增量检查交互小说世界是否自洽。Vaucher et al., 2026.
- [VADAOrchestra: Neurosymbolic Orchestration of Adaptive Reasoning Workflows](https://arxiv.org/abs/2606.22485) — 用神经符号编排可适应的推理工作流，保留流程系统的可审计执行。Baldazzi et al., 2026.
- [VeriThoughts: Enabling Automated Verilog Code Generation using Reasoning and Formal Verification](https://arxiv.org/abs/2505.20302) — 推理生成 Verilog，形式验证负责接受或拒绝。Yubeaton, Nakkab et al., NeurIPS 2025.
- [LogicSAGE: Neuro-Symbolic Reasoning with Socratic-Guided Enhancement](https://icml.cc/virtual/2026/poster/65084) — 苏格拉底式引导下的神经符号推理。ICML 2026.
- [Decodable but Not Faithful: Coupling Natural-Language Rationales to Programmatic Verifiers](https://arxiv.org/abs/2606.21678) — 自然语言理由可以读，但不等于忠实；需要绑到程序验证器上。Ananthula et al., 2026.

## 形式化证明智能体

证明对象是符号。模型负责形式化、选引理或写策略，证明助手负责拒绝错误步骤。

- [Hilbert: Recursively Building Formal Proofs with Informal Reasoning](https://arxiv.org/abs/2509.22819) — 非正式推理递归地长成形式证明。Varambally, Voice et al., ICLR 2026.
- [Mathesis: Towards Formal Theorem Proving from Natural Languages](https://arxiv.org/abs/2506.07047) — 从自然语言走到形式定理证明。Yu, Zhong et al., ICLR 2026.
- [LoC-Decomp: LLM Autoformalization via Logical Concept Decomposition and Iterative Feedback Correction](https://iclr.cc/virtual/2026/poster/10011947) — 按逻辑概念分解做自动形式化，再用反馈迭代修正。ICLR 2026.
- [Numina-Lean-Agent: An Open and General Agentic Reasoning System for Formal Mathematics](https://arxiv.org/abs/2601.14027) — 面向形式数学的开源智能体证明系统。Liu, Zhou et al., ICML 2026.
- [LAMP: Lean-based Agentic framework with MCP and Proof Repair](https://arxiv.org/abs/2606.28841) — Lean 智能体加 MCP 工具，失败的证明再修。Srinivasan and Patawar, 2026.
- [FormalRx: Rectify and eXamine Semantic Failures in Autoformalization](https://arxiv.org/abs/2607.04655) — 不满足于形式化的对错分，专门定位语义没对齐的位置。Wang, Huang, Wan, and Zhu, 2026.
- [Distilling LLM Feedback for Lean Theorem Proving](https://arxiv.org/abs/2605.30861) — 把语言模型对证明尝试的反馈蒸馏进 Lean 证明模型。Narozniak et al., 2026.
- [Pythagoras-Prover: Advancing Efficient Formal Proving via Augmented Lean Formalisation](https://arxiv.org/abs/2606.12594) — 用增强的 Lean 形式化提高证明效率。Leang et al., 2026.
- [LLM Library Learning Fails: A LEGO-Prover Case Study](https://arxiv.org/abs/2504.03048) — 对“引理库真的被复用了吗”做计算量对齐的复查。Berlot-Attwell, Rudzicz, and Si, 2025.
- [EvolProver: Advancing Automated theorem proving by Evolving Formalized Problems via Symmetry and Difficulty](https://iclr.cc/virtual/2026/poster/10008534) — 沿对称性和难度演化已形式化的题目，用来训练证明器。ICLR 2026.

LEGO-Prover（2023）把已验证引理放进增长库。上面的 2025 复查说明，这种库学习的收益需要和同等搜索预算比，不能只看库变长了没有。

## 技能库与符号对象更新

符号对象会在交互之后变长：新技能、新程序、新的评估器或智能体结构。

- [Symbolic Learning Enables Self-Evolving Agents](https://arxiv.org/abs/2406.18532) — 把提示、工具和流水线当成符号网络的权重，用语言梯度做整体更新。Zhou, Ou, Ding et al., 2024. [Code](https://github.com/aiwaves-cn/agents)
- [Lifting Traces to Logic: Programmatic Skill Induction with Neuro-Symbolic Learning for Long-Horizon Agentic Tasks](https://arxiv.org/abs/2605.01293) — 把长时程轨迹提升成可执行的逻辑技能。Shao, Yin et al., ICML 2026.
- [Differentiate the Evaluator, Not the Program: An Efficient Runtime Representation for Neuro-Symbolic Learning](https://arxiv.org/abs/2607.03574) — 程序搜索的瓶颈在参数拟合。可微的是评估器，不是每一条候选程序。Sheneman, 2026.
- [DeepSWIP: Quotient-WMC Counterfactuals for Neural Probabilistic Logic Programs](https://arxiv.org/abs/2606.20526) — 给 DeepProbLog 一类神经概率逻辑程序补上反事实语义。Habib, Belle, and He, 2026.
- [MINT: Minimal Information Neuro-Symbolic Tree for Objective-Driven Knowledge-Gap Reasoning and Active Elicitation](https://arxiv.org/abs/2602.05048) — 用神经符号树找出还缺的信息，再主动询问。Fang, Imani et al., ICML 2026.
- [GLARE: Scalable Neuro-Symbolic Reward Shaping for LLM Agents via Group-Level Automata](https://icml.cc/virtual/2026/poster/66450) — 用群体级自动机给语言模型智能体做可扩展的神经符号奖励塑造。ICML 2026.
- [Influence-Guided Symbolic Regression: Scientific Discovery via LLM-Driven Equation Search with Granular Feedback](https://icml.cc/virtual/2026/poster/60987) — 语言模型提议方程，细粒度反馈引导符号回归。ICML 2026.
- [Deliberate Evolution for Sample-Efficient Symbolic Regression with LLM](https://icml.cc/virtual/2026/poster/66263) — 用审慎的进化搜索提高语言模型符号回归的样本效率。ICML 2026.
- [Skill-Driven Neurosymbolic State Abstractions](https://neurips.cc/virtual/2025/poster/116718) — 由技能驱动神经符号状态抽象。NeurIPS 2025.

## 规划与 PDDL

语言模型负责把任务写成规划问题，符号规划器或验证器负责搜索和接受计划。

- [Training the Orchestrator: A Supervised Approach to End-to-End PDDL Planning with LLM Agents](https://arxiv.org/abs/2606.21740) — 自然语言意图进 PDDL，一组修复智能体在验证器回路里改规格，再监督训练编排器。Mangannavar, Coalson, Dugar, and Tadepalli, 2026.
- [UniDomain: Pretraining a Unified PDDL Domain from Real-World Demonstrations for Generalizable Robot Task Planning](https://neurips.cc/virtual/2025/poster/116644) — 从真实演示里预训练统一的 PDDL domain，而不是每个任务手写算子。NeurIPS 2025.
- [Natural Language PDDL (NL-PDDL) for Open-world Goal-oriented Commonsense Regression Planning in Embodied AI](https://iclr.cc/virtual/2026/poster/10007784) — 用自然语言形式的 PDDL 做开放世界目标导向规划。ICLR 2026.
- [DecoVer: A Decompose-and-Verify Neuro-Symbolic Framework for Embodied Task Planning with BC+](https://icml.cc/virtual/2026/poster/63653) — 分解之后用神经符号层验证具身任务计划。ICML 2026.
- [NATURAL PLAN: Benchmarking LLMs on Natural Language Planning](https://arxiv.org/abs/2406.04520) — 行程、日历和会议规划。约束是符号的，题目是自然语言。Zheng, Mishra, Zhang et al., 2024.
- [GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://arxiv.org/abs/2410.05229) — 用符号模板生成数学题变体，用来区分模式匹配和可执行推理。Mirzadeh et al., 2024.

LLM+P（2023）和 PlanBench（2022）仍是“译成 PDDL、再交给规划器/验证器”的参照，这里不单列。

## 知识图谱上的搜索智能体

- [Think-on-Graph 2.0: Deep and Faithful Large Language Model Reasoning with Knowledge-guided Retrieval Augmented Generation](https://arxiv.org/abs/2407.10805) — 结构化图谱检索和非结构化文档检索交替进行。Ma, Xu, Jiang et al., 2024.
- [CLAUSE: Agentic Neuro-Symbolic Knowledge Graph Reasoning via Dynamic Learnable Context Engineering](https://arxiv.org/abs/2509.21035) — 智能体在知识图谱上做神经符号推理，上下文本身可学习。Zhao, Dai et al., ICLR 2026.
- [KARMA: Knowledge graph-based Automated Reasoning Materialization and Alignment](https://arxiv.org/abs/2607.03166) — 把知识图谱上的推理结果物化并对齐。Choi et al., 2026.

Think-on-Graph（2023）和 Reasoning on Graphs（2023）是这一路的前身。[ToG 代码](https://github.com/GasolSun36/ToG)，[RoG 代码](https://github.com/RManLuo/reasoning-on-graphs)。

## 具身与可执行策略

感知是神经的，中间程序、规则或状态抽象是符号的，并且会被执行。

- [WALL-E: World Alignment by Rule Learning Improves World Model-based LLM Agents](https://arxiv.org/abs/2410.07484) — 用规则学习把世界模型和语言模型智能体对齐。Zhou, Zhou et al., 2024.
- [NeSyPr: Neurosymbolic Proceduralization For Efficient Embodied Reasoning](https://arxiv.org/abs/2510.19429) — 把具身推理程序化，走神经符号过程而不是逐步语言搜索。Choi, Kim et al., NeurIPS 2025.
- [Towards Reliable Code-as-Policies: A Neuro-Symbolic Framework for Embodied Task Planning](https://arxiv.org/abs/2510.21302) — 在 Code-as-Policies 上加神经符号约束，让生成的控制程序可检查。Ahn, Choi et al., NeurIPS 2025.
- [Neuro-Symbolic Drive: Rule-Grounded Faithful Reasoning for Driving VLAs](https://arxiv.org/abs/2606.23938) — 驾驶视觉语言动作模型的理由要落到规则上，才能和动作因果相连。Gao, Huang, Lu, and Zhang, 2026.

## 语言与框架

- [DeepProbLog](https://github.com/ML-KULeuven/deepproblog) — 概率逻辑程序加神经谓词。
- [Scallop](https://github.com/scallop-lang/scallop) — 神经符号编程语言。
- [DeepLog](https://github.com/ML-KULeuven/deeplog) — 神经符号抽象机与 GPU 后端。论文：[The DeepLog Neurosymbolic Machine](https://arxiv.org/abs/2508.13697)，Derkinderen et al., 2025.
- [LeanDojo](https://github.com/lean-dojo/LeanDojo) — Lean 证明数据、交互环境和检索式证明器。
- [PrologMCP](https://arxiv.org/abs/2606.14935) — 给语言模型智能体的 Prolog 工具接口。

## 基准

2024 年之前的逻辑问答和视觉诊断集（ProofWriter、FOLIO、PrOntoQA、LogiQA、CLUTRR、CLEVR、GQA）以及 miniF2F、ARC、PlanBench 仍被引用，这里不逐条展开。

- [PutnamBench: Evaluating Neural Theorem-Provers on the Putnam Mathematical Competition](https://arxiv.org/abs/2407.11214) — Putnam 竞赛的形式化评测。Tsoukalas, Lee, Jennings et al., 2024.
- [GSM-Symbolic](https://arxiv.org/abs/2410.05229) — 符号模板化的数学题，用来测稳健性。Mirzadeh et al., 2024.
- [Socrates or Smartypants: Testing Logic Reasoning Capabilities of Large Language Models with Logic Programming-Based Test Oracles](https://ojs.aaai.org/index.php/AAAI/article/view/39021) — 用逻辑程序做测试预言，检查语言模型的逻辑推理。AAAI 2026.
- [QMFOL: Benchmarking Large Language Model Reasoning via Quantifiable Monadic First-Order Logic Test Case Generation](https://arxiv.org/abs/2606.20227) — 用可量化的一阶逻辑测试用例评测推理。Zheng et al., 2026.
- [Evaluating the Robustness of Proof Autoformalization in Lean 4](https://arxiv.org/abs/2606.14867) — 测 Lean 4 自动形式化在扰动下还是否成立。Gui et al., 2026.
- [Faults in Our Formal Benchmarking: Dataset Defects and Evaluation Failures in Lean Theorem Proving](https://arxiv.org/abs/2606.29493) — 形式化证明评测本身的数据缺陷和评测失败。Ammanamanchi et al., 2026. [同题会议页](https://icml.cc/virtual/2026/poster/62980)
- [The Signal-Coverage Matrix: Stratifying Type and Semantic Errors in Statement Autoformalization](https://arxiv.org/abs/2606.28013) — 把自动形式化的错误分成类型错误和语义错误。Dai et al., 2026.
- [NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry](https://arxiv.org/abs/2608.28481) — 测语言模型能不能把几何题形式化成 AlphaGeometry 的语言。Xiao, Song, and Hu, 2026.
- [FVSpec: Real-World Property-Based Tests as Lean Challenges](https://arxiv.org/abs/2606.01008) — 把真实的基于性质的测试变成 Lean 题目。Dougherty et al., 2026.
- [MA-ProofBench: A Two-Tiered Evaluation of LLMs for Theorem Proving in Mathematical Analysis](https://arxiv.org/abs/2606.13782) — 数学分析证明的两层评测。Pu et al., 2026.

## 相关清单

更全的神经符号论文表，包括本清单故意收起的 2024 年之前工作：

- [Awesome-LLM-Reasoning-with-NeSy](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy) — LLM 推理里的神经符号论文表。
- [awesome-deeplogic](https://github.com/ccclyu/awesome-deeplogic) — 深度网络与符号逻辑结合的论文，偏 NLP。
- [awesome-neurosymbolic-ai](https://github.com/sciknoworg/awesome-neurosymbolic-ai) — 神经符号评测资源。

## 贡献

欢迎补 2024 年以后、符合三条回路之一的工作。收录标准见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可

[CC0 1.0](LICENSE)。论文版权归作者和出版方，本仓库只整理公开链接。
