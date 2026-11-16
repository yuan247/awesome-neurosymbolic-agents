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

纯思维链、没有符号对象的工具调用、通用检索增强，不在收录范围内。它们可以是系统里的一层，但不是神经符号学习本身。

## 目录

- [综述与问题定义](#综述与问题定义)
- [可微符号学习底座](#可微符号学习底座)
- [符号引擎训练神经网络](#符号引擎训练神经网络)
- [编译成符号再求解](#编译成符号再求解)
- [形式化证明智能体](#形式化证明智能体)
- [技能库、程序归纳与自我更新](#技能库程序归纳与自我更新)
- [规划与约束](#规划与约束)
- [知识图谱上的搜索智能体](#知识图谱上的搜索智能体)
- [视觉与具身](#视觉与具身)
- [语言与框架](#语言与框架)
- [基准](#基准)
- [相关清单](#相关清单)
- [贡献](#贡献)

## 综述与问题定义

先看地图，再看系统。这一节区分“神经符号”和“会调用工具的 LLM”。

- [Neurosymbolic AI: The 3rd Wave](https://arxiv.org/abs/2012.05876) — 把神经符号定义为表示、学习和推理同时用到神经与符号，而不是事后解释。Garcez and Lamb, 2020.
- [Neuro-Symbolic AI in 2024: A Systematic Review](https://arxiv.org/abs/2501.05435) — 167 篇系统综述。知识表示和学习/推理是大头，元认知几乎空着。Colelough and Regli, 2025.
- [Neuro-Symbolic Artificial Intelligence: Towards Improving the Reasoning Abilities of Large Language Models](https://arxiv.org/abs/2508.13678) — IJCAI 2025 综述。按 Symbolic→LLM、LLM→Symbolic、LLM+Symbolic 整理求解器、证明搜索和程序解释器。Yang et al., 2025. [论文表](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy)
- [Converging Paradigms: The Synergy of Symbolic and Connectionist AI in LLM-Empowered Autonomous Agents](https://arxiv.org/abs/2407.08516) — 论证语言智能体本身就是神经与符号的汇合：神经网络加工作流、规则、工具和知识图谱。Xiong et al., 2024.
- [Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427) — CoALA。用工作记忆、情景/语义/程序记忆和内外部动作给语言智能体一个认知架构，不是具体算法。Sumers et al., 2023.

## 可微符号学习底座

这些系统早于 LLM agent，但仍然是“学习一个符号推理器”的参照系。Agent 时代很多工作只是把其中的感知模块换成了 LLM。

- [Learning Explanatory Rules from Noisy Data](https://arxiv.org/abs/1711.04574) — ∂ILP。在噪声数据上做可微归纳逻辑程序设计。Evans and Grefenstette, 2017.
- [DeepProbLog: Neural Probabilistic Logic Programming](https://arxiv.org/abs/1805.10872) — 神经网络作为概率逻辑程序里的神经谓词，端到端反传。Manhaeve et al., 2018. [Code](https://github.com/ML-KULeuven/deepproblog)
- [Neural Logic Machines](https://arxiv.org/abs/1904.11694) — 用可微规则层做关系归纳和组合式推理。Dong et al., 2019.
- [SATNet: Bridging deep learning and logical reasoning using a differentiable satisfiability solver](https://arxiv.org/abs/1905.12149) — 把 SAT 求解层放进网络，用来学习逻辑约束。Wang et al., 2019.
- [The Neuro-Symbolic Concept Learner](https://arxiv.org/abs/1904.12584) — 从图像、问题和答案里联合学习视觉概念和符号程序，不需要程序标注。Mao et al., 2019.
- [DreamCoder: Growing generalizable, interpretable knowledge with wake-sleep Bayesian program learning](https://arxiv.org/abs/2006.08381) — 醒眠循环里同时写程序和压缩出可复用库。后面的 LILO、技能库学习都以它为参照。Ellis et al., 2020.
- [Logical Neural Networks](https://arxiv.org/abs/2006.13155) — 每个神经元对应一个逻辑运算，真值约束和网络权重一起学。Riegel et al., 2020. [Code](https://github.com/IBM/LNN)
- [Logic Tensor Networks](https://arxiv.org/abs/2012.13635) — 用模糊逻辑把一阶公式变成可微损失。Badreddine et al., 2020.
- [NeurASP: Embracing Neural Networks into Answer Set Programming](https://arxiv.org/abs/2307.07700) — 神经网络输出稳定模型上的概率事实，ASP 负责组合约束。Yang et al., 2023.
- [Scallop: A Language for Neurosymbolic Programming](https://arxiv.org/abs/2304.04812) — 可调证明粒度的可微 Datalog，用来接感知模型和关系推理。Li et al., 2023. [Code](https://github.com/scallop-lang/scallop)
- [PyReason: Software for Open World Temporal Logic](https://arxiv.org/abs/2302.13482) — 开放世界、带时间的带标注逻辑，面向图和知识图谱，推理过程可解释。Aditya et al., 2023. [Code](https://github.com/lab-v2/pyreason)
- [The DeepLog Neurosymbolic Machine](https://arxiv.org/abs/2508.13697) — KU Leuven 的“神经符号抽象机”：用代数电路把多种逻辑和神经模块接在一起，并放到 GPU 上。Derkinderen et al., 2025. [Code](https://github.com/ML-KULeuven/deeplog)

## 符号引擎训练神经网络

学习信号来自符号系统，而不是人工逐步标注。这是 agent 时代最清楚的“学习”路线：模型提出候选，证明器或执行器给出对错，再更新策略。

- [Solving olympiad geometry without human demonstrations](https://doi.org/10.1038/s41586-023-06747-5) — AlphaGeometry。语言模型提出辅助构造，符号演绎引擎验证；合成定理不依赖人类证明示范。Trinh et al., Nature 2024. [Code](https://github.com/google-deepmind/alphageometry)
- [Gold-medalist Performance in Solving Olympiad Geometry with AlphaGeometry2](https://arxiv.org/abs/2502.03544) — 扩大形式语言和搜索，并改用 Gemini 架构。Chervonyi, Trinh, Olšák et al., 2025. [Code](https://github.com/google-deepmind/alphageometry2)
- [Wu's Method can Boost Symbolic AI to Rival Silver Medalists and AlphaGeometry to Outperform Gold Medalists at IMO Geometry](https://arxiv.org/abs/2404.06405) — 用吴方法这种代数几何求解器对照 AlphaGeometry，说明符号后端本身还能再换。Sinha et al., 2024.
- [Olympiad-level formal mathematical reasoning with reinforcement learning](https://doi.org/10.1038/s41586-025-09833-y) — AlphaProof。在 Lean 里做 AlphaZero 式强化学习，难题上再做测试时强化学习。与 AlphaGeometry 2 一起达到 IMO 2024 银牌线。Hubert et al., Nature 2025.
- [Mathematical discoveries from program search with large language models](https://doi.org/10.1038/s41586-023-06924-6) — FunSearch。LLM 提出程序，自动评估器做进化选择，用来发现组合构造和装箱启发式。Romera-Paredes et al., Nature 2024. [Code](https://github.com/google-deepmind/funsearch)
- [AlphaEvolve: A coding agent for scientific and algorithmic discovery](https://arxiv.org/abs/2506.13131) — 把 FunSearch 从单函数扩展到整个代码库，用评估级联筛选 Gemini 生成的补丁。Novikov et al., 2025.
- [DeepSeek-Prover: Advancing Theorem Proving in LLMs through Large-Scale Synthetic Data](https://arxiv.org/abs/2405.14333) — 用大规模合成 Lean 证明做监督。Xin, Guo, Shao et al., 2024.
- [DeepSeek-Prover-V1.5: Harnessing Proof Assistant Feedback for Reinforcement Learning and Monte-Carlo Tree Search](https://arxiv.org/abs/2408.08152) — 证明助手的对错进入强化学习和树搜索。Xin, Ren, Song et al., 2024. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V1.5)
- [DeepSeek-Prover-V2: Advancing Formal Mathematical Reasoning via Reinforcement Learning for Subgoal Decomposition](https://arxiv.org/abs/2504.21801) — 子目标分解加上证明器反馈的强化学习。Ren, Shao, Song et al., 2025. [Code](https://github.com/deepseek-ai/DeepSeek-Prover-V2)
- [Goedel-Prover: A Frontier Model for Open-Source Automated Theorem Proving](https://arxiv.org/abs/2502.07640) — 开源 Lean 证明模型，用合成语句和证明训练。Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover)
- [Goedel-Prover-V2: Scaling Formal Theorem Proving with Scaffolded Data Synthesis and Self-Correction](https://arxiv.org/abs/2508.03613) — 脚手架式数据合成，以及验证器引导的自我修正。Lin, Tang, Lyu et al., 2025. [Code](https://github.com/Goedel-LM/Goedel-Prover-V2)
- [Kimina-Prover Preview: Towards Large Formal Reasoning Models with Reinforcement Learning](https://arxiv.org/abs/2504.11354) — 在形式推理模式上做大规模强化学习，而不是只做监督微调。Wang, Unsal, Lin et al., 2025. [Code](https://github.com/MoonshotAI/Kimina-Prover-Preview)

## 编译成符号再求解

模型不直接给最终答案。它只负责翻译；求解器、解释器或定理证明器负责推理。学习如果存在，通常发生在翻译器或自修复循环里。

- [PAL: Program-aided Language Models](https://arxiv.org/abs/2211.10435) — 把推理步骤写成 Python，把计算交给解释器。Gao et al., 2022.
- [Program of Thoughts Prompting: Disentangling Computation from Reasoning for Numerical Reasoning Tasks](https://arxiv.org/abs/2211.12588) — 同样把计算从语言模型里拆出去，强调程序是中间表示。Chen et al., 2022.
- [Binding Language Models in Symbolic Languages](https://arxiv.org/abs/2210.02875) — Binder。SQL/Python 覆盖不了的部分绑定回语言模型调用，再整体执行。Cheng et al., 2022.
- [Parsel: Algorithmic Reasoning with Language Models by Composing Decompositions](https://arxiv.org/abs/2212.10561) — 先写分层的函数说明，再分别实现并验证，用来做竞赛题、规划和证明。Zelikman et al., 2022.
- [Faithful Chain-of-Thought Reasoning](https://arxiv.org/abs/2301.13379) — 先翻译成符号推理链，再由确定性求解器得到答案。Lyu et al., 2023.
- [Logic-LM: Empowering Large Language Models with Symbolic Solvers for Faithful Logical Reasoning](https://arxiv.org/abs/2305.12295) — 译成逻辑程序、一阶逻辑、约束或 SAT，求解失败再修形式化。Pan et al., 2023.
- [LINC: A Neurosymbolic Approach for Logical Reasoning by Combining Language Models with First-Order Logic Provers](https://arxiv.org/abs/2310.15164) — 语言模型做语义分析，一阶逻辑证明器做演绎。Olausson et al., 2023. [Code](https://github.com/benlipkin/linc)
- [SatLM: Satisfiability-Aided Language Models Using Declarative Prompting](https://arxiv.org/abs/2305.09656) — 生成声明式 SAT/SMT 规格，而不是命令式程序。Ye et al., 2023.
- [Leveraging Large Language Models to Generate Answer Set Programs](https://arxiv.org/abs/2307.07699) — 把文本题译成回答集程序，交给 ASP 求解。Ishay, Yang, and Lee, 2023.
- [Faithful Logical Reasoning via Symbolic Chain-of-Thought](https://arxiv.org/abs/2405.18357) — SymbCoT。符号表达式、符号计划、验证器都留在推理回路里。Xu, Fei, Pan et al., 2024.
- [Logic-of-Thought: Injecting Logic into Contexts for Full Reasoning in Large Language Models](https://arxiv.org/abs/2409.17539) — 用命题逻辑把隐含结论补进上下文，再交给语言模型。Liu et al., 2024.
- [Chain of Code: Reasoning with a Language Model-Augmented Code Emulator](https://arxiv.org/abs/2312.04474) — 能执行的代码交给解释器，解释器执行不了的伪代码交回语言模型模拟。Li, Liang, Zeng et al., 2023.

## 形式化证明智能体

证明对象是符号。模型负责选前提、写策略或草拟非正式证明，证明助手负责拒绝错误步骤。

- [Generative Language Modeling for Automated Theorem Proving](https://arxiv.org/abs/2009.03393) — GPT-f。用语言建模生成 Metamath 证明步骤。Polu and Sutskever, 2020.
- [HyperTree Proof Search for Neural Theorem Proving](https://arxiv.org/abs/2205.11491) — 在证明树上做超树搜索，神经网络给策略打分。Lample et al., 2022.
- [Autoformalization with Large Language Models](https://arxiv.org/abs/2205.12615) — 非正式数学到 Isabelle 形式化的自动翻译，是后面所有证明智能体的瓶颈环节。Wu, Jiang, Li et al., 2022.
- [Draft, Sketch, and Prove: Guiding Formal Theorem Provers with Informal Proofs](https://arxiv.org/abs/2210.12283) — 非正式草稿先变成证明草图，再交给形式证明器补全。Jiang, Welleck, Zhou et al., 2022.
- [LeanDojo: Theorem Proving with Retrieval-Augmented Language Models](https://arxiv.org/abs/2306.15626) — 从 Lean 仓库抽出带前提标注的证明，并用检索增强的 ReProver 选前提。Yang, Swope, Gu et al., 2023. [Code](https://github.com/lean-dojo/LeanDojo)
- [LEGO-Prover: Neural Theorem Proving with Growing Libraries](https://arxiv.org/abs/2310.00656) — 证明过程中增长一个已验证引理库，并按块复用。Wang, Xin, Zheng et al., 2023.
- [LLM Library Learning Fails: A LEGO-Prover Case Study](https://arxiv.org/abs/2504.03048) — 对“库真的被复用了吗”做计算量对齐的复查。库学习的收益可能比论文叙事更弱。Berlot-Attwell, Rudzicz, and Si, 2025.
- [ProofNet: Autoformalizing and Formally Proving Undergraduate-Level Mathematics](https://arxiv.org/abs/2302.12433) — 本科数学的自动形式化与证明基准，暴露形式化和证明是两件不同的事。Azerbayev et al., 2023.

## 技能库、程序归纳与自我更新

符号对象会在交互之后变长：新技能、新函数、新算子、新的智能体图。这一节是“学习”而不是“一次翻译”。

- [Hypothesis Search: Inductive Reasoning with Language Models](https://arxiv.org/abs/2309.05660) — 先写自然语言假设，再写成可在样例上检验的程序。面向 ARC 这类少样本归纳。Wang, Zelikman, Poesia et al., 2023.
- [LILO: Learning Interpretable Libraries by Compressing and Documenting Code](https://arxiv.org/abs/2310.19791) — LLM 提出程序，Stitch 压缩出 λ 抽象，再自动写成可被模型读懂的文档。Grand, Wong, Bowers et al., 2023.
- [Voyager: An Open-Ended Embodied Agent with Large Language Models](https://arxiv.org/abs/2305.16291) — Minecraft 里把通过执行检验的程序存进技能库，并用来写后面的技能。Wang, Xie, Jiang et al., 2023. [Code](https://github.com/MineDojo/Voyager)
- [Learning adaptive planning representations with natural language guidance](https://arxiv.org/abs/2312.08566) — Ada。从语言和交互里学可组合的高层动作，每个动作带符号的前提与效果。Wong, Mao, Sharma et al., 2023.
- [Symbolic Learning Enables Self-Evolving Agents](https://arxiv.org/abs/2406.18532) — 把提示、工具和流水线当成符号网络的权重，用语言梯度做整体更新。Zhou, Ou, Ding et al., 2024. [Code](https://github.com/aiwaves-cn/agents)
- [DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines](https://arxiv.org/abs/2310.03714) — 相邻工作，不是逻辑神经符号。它把语言模型调用编译成可优化的声明式程序，常被拿来和符号学习对比。Khattab et al., 2023.

## 规划与约束

规划算子、PDDL 和验证器构成智能体的世界模型。语言模型负责形式化，规划器负责搜索。

- [LLM+P: Empowering Large Language Models with Optimal Planning Proficiency](https://arxiv.org/abs/2304.11477) — 自然语言规划问题译成 PDDL，经典规划器求计划，再译回语言。Liu, Jiang, Zhang et al., 2023.
- [ISR-LLM: Iterative Self-Refined Large Language Model for Long-Horizon Sequential Task Planning](https://arxiv.org/abs/2308.13724) — 长时程任务上反复修正规划表述，而不是一次生成到底。Zhou, Song, Yao et al., 2023.
- [PlanBench: An Extensible Benchmark for Evaluating Large Language Models on Planning and Reasoning about Change](https://arxiv.org/abs/2206.10498) — 用 PDDL 领域和验证器检查模型是不是真的在做关于动作与变化的推理。Valmeekam et al., 2022.
- [NATURAL PLAN: Benchmarking LLMs on Natural Language Planning](https://arxiv.org/abs/2406.04520) — 行程、日历和会议规划。题目是自然语言，约束仍然是符号的。Zheng, Mishra, Zhang et al., 2024.
- [GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models](https://arxiv.org/abs/2410.05229) — 用符号模板生成 GSM8K 变体。换数字或加无关句会让准确率明显下降，用来区分模式匹配和可执行推理。Mirzadeh et al., 2024.

## 知识图谱上的搜索智能体

知识图谱提供关系路径。智能体决定往哪走，路径本身是可追溯的符号证明。

- [Think-on-Graph: Deep and Responsible Reasoning of Large Language Model on Knowledge Graph](https://arxiv.org/abs/2307.07697) — 语言模型作为智能体，在图谱上做束搜索，边走边发现推理路径。Sun, Xu, Tang et al., 2023. [Code](https://github.com/GasolSun36/ToG)
- [Think-on-Graph 2.0: Deep and Faithful Large Language Model Reasoning with Knowledge-guided Retrieval Augmented Generation](https://arxiv.org/abs/2407.10805) — 结构化图谱检索和非结构化文档检索交替进行。Ma, Xu, Jiang et al., 2024.
- [Reasoning on Graphs: Faithful and Interpretable Large Language Model Reasoning](https://arxiv.org/abs/2310.01061) — 先生成图谱上的关系路径计划，再检索真实路径并据此作答。Luo, Li, and Haffari, 2023. [Code](https://github.com/RManLuo/reasoning-on-graphs)

## 视觉与具身

感知是神经的，对外动作或中间程序是符号的。关键差别在于程序或场景图会被执行，而不是只作为解释文本。

- [Code as Policies: Language Model Programs for Embodied Control](https://arxiv.org/abs/2209.07753) — 语言模型写可执行的机器人策略代码，感知 API 留在代码外面。Liang, Huang, Xia et al., 2022.
- [ProgPrompt: Generating Situated Robot Task Plans using Large Language Models](https://arxiv.org/abs/2209.11302) — 用程序形式写带前提检查的机器人任务计划。Singh, Blukis, Mousavian et al., 2022.
- [Visual Programming: Compositional visual reasoning without training](https://arxiv.org/abs/2211.11559) — VisProg。语言模型生成调用视觉模块的程序，中间结果往下传。Gupta and Kembhavi, 2022. [Code](https://github.com/allenai/visprog)
- [ViperGPT: Visual Inference via Python Execution for Reasoning](https://arxiv.org/abs/2303.08128) — 把视觉问题编译成 Python，并在执行时调用视觉模型。Surís, Menon, and Vondrick, 2023.
- [ConceptGraphs: Open-Vocabulary 3D Scene Graphs for Perception and Planning](https://arxiv.org/abs/2309.16650) — 开放词表三维场景图，作为具身智能体可查询的符号地图。Gu, Kuwajerwala, Morin et al., 2023.

## 语言与框架

用来把上面的回路做成可运行系统的语言和库。

- [DeepProbLog](https://github.com/ML-KULeuven/deepproblog) — 概率逻辑程序加神经谓词。
- [Scallop](https://github.com/scallop-lang/scallop) — 神经符号编程语言，关系推理可微。
- [DeepLog](https://github.com/ML-KULeuven/deeplog) — 神经符号抽象机与 GPU 后端。
- [PyReason](https://github.com/lab-v2/pyreason) — 开放世界时序逻辑推理。
- [Logical Neural Networks](https://github.com/IBM/LNN) — IBM 的逻辑神经网络实现。
- [SymbolicAI](https://github.com/ExtensityAI/symbolicai) — 把逻辑运算写成可组合的符号表达式，后端可以是语言模型。
- [LeanDojo](https://github.com/lean-dojo/LeanDojo) — Lean 证明数据、交互环境和检索式证明器。

## 基准

评测按符号对象分组。同一套准确率不能同时说明“会证明”“会规划”和“会归纳”。

### 逻辑问答

- [ProofWriter: Generating Implications, Proofs, and Abductive Statements over Natural Language](https://arxiv.org/abs/2012.13048) — 自然语言规则上的蕴含、证明和反绎。Tafjord, Dalvi Mishra, and Clark, 2020.
- [FOLIO: Natural Language Reasoning with First-Order Logic](https://arxiv.org/abs/2209.00840) — 一阶逻辑专家标注的自然语言推理。Han, Schoelkopf, Zhao et al., 2022.
- [Language Models Are Greedy Reasoners: A Systematic Formal Analysis of Chain-of-Thought](https://arxiv.org/abs/2210.01240) — PrOntoQA。用合成世界检查思维链是不是真的在做演绎。Saparov and He, 2022.
- [LogiQA: A Challenge Dataset for Machine Reading Comprehension with Logical Reasoning](https://arxiv.org/abs/2007.08124) — 公务员考试式逻辑阅读理解。Liu, Cui, Liu et al., 2020.
- [BoardgameQA: A Dataset for Natural Language Reasoning with Contradictory Information](https://arxiv.org/abs/2306.07934) — 带矛盾信息和偏好的推理。Kazemi, Yuan, Bhatia et al., 2023.
- [CLUTRR: A Diagnostic Benchmark for Inductive Reasoning from Text](https://arxiv.org/abs/1908.06177) — 从亲属关系故事里归纳规则。Sinha, Sodhani, Dong et al., 2019.

### 数学与证明

- [MiniF2F: a cross-system benchmark for formal Olympiad-level mathematics](https://arxiv.org/abs/2109.00110) — 跨 Lean、Isabelle、Metamath 的奥赛形式化题。Zheng, Han, and Polu, 2021.
- [PutnamBench: Evaluating Neural Theorem-Provers on the Putnam Mathematical Competition](https://arxiv.org/abs/2407.11214) — Putnam 竞赛的形式化评测。Tsoukalas, Lee, Jennings et al., 2024.
- [GSM-Symbolic](https://arxiv.org/abs/2410.05229) — 符号模板化的小学数学，用来测稳健性而不是单次刷榜。Mirzadeh et al., 2024.

### 归纳、视觉与规划

- [On the Measure of Intelligence](https://arxiv.org/abs/1911.01547) — ARC。少样本、禁止靠背题的抽象归纳。Chollet, 2019.
- [CLEVR: A Diagnostic Dataset for Compositional Language and Elementary Visual Reasoning](https://arxiv.org/abs/1612.06890) — 组合式视觉推理的诊断集，神经符号概念学习的常用试验台。Johnson et al., 2016.
- [GQA: A New Dataset for Real-World Visual Reasoning and Compositional Question Answering](https://arxiv.org/abs/1902.09506) — 真实图像上的组合问答，并带场景图。Hudson and Manning, 2019.
- [PlanBench](https://arxiv.org/abs/2206.10498) — 见上文。规划是否关于动作与变化。Valmeekam et al., 2022.
- [NATURAL PLAN](https://arxiv.org/abs/2406.04520) — 见上文。自然语言约束规划。Zheng et al., 2024.
- [NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry](https://arxiv.org/abs/2608.28481) — 专门测语言模型能不能把几何题自动形式化成 AlphaGeometry 的语言。Xiao, Song, and Hu, 2026.

## 相关清单

本仓库只收 agent 时代的学习回路。更全的神经符号或逻辑深度学习清单：

- [Awesome-LLM-Reasoning-with-NeSy](https://github.com/LAMDA-NeSy/Awesome-LLM-Reasoning-with-NeSy) — LLM 推理里的神经符号论文表，和上面 Yang et al. 综述配套。
- [awesome-deeplogic](https://github.com/ccclyu/awesome-deeplogic) — 深度网络与符号逻辑结合的论文，偏 NLP。
- [awesome-neurosymbolic-ai](https://github.com/sciknoworg/awesome-neurosymbolic-ai) — 神经符号评测资源。

## 贡献

欢迎补 2023 年以后、符合三条回路之一的工作。收录标准、格式和不要交什么，见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可

[CC0 1.0](LICENSE)。论文版权归作者和出版方，本仓库只整理公开链接。
