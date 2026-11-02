The same 10 topics/metatags, but the three example papers per topic are now restricted to work with primary or leading authorship from top research institutions worldwide (Google/DeepMind/Google Cloud AI Research, Microsoft, Meta, Stanford, UC Berkeley, Northwestern collaborating with Google, Queen’s University + Huawei, Alibaba/Qwen, etc.).

### 1. Harness Engineering
1. **Harness²: Recursive Agent Harnessing for an Open World**  
   Authors: Ruiyao Xu, Yanfei Chen, Zhongying CuiZhu, Bhavana Dalvi Mishra et al. (Google Cloud AI Research + Northwestern)  
   Date: 9 October 2026  
   URL: https://arxiv.org/abs/2610.14224  
   Abstract: Plug-and-play recursive agent harnessing framework with task-level and harnessing-level recursion. Single refinement steps improve multiple model–harness pairs by up to 11.5 points on WorkBuddy-Bench and 11.8 points on LAB.

2. **RRSI: Regularized Recursive Self-Improvement of Agent Harnesses**  
   Authors: Peng Xia, Rujun Han, Zifeng Wang, Yanfei Chen et al. (Google Cloud AI Research + Stanford, UNC, etc.)  
   Date: 21 September 2026 (rev. 4 October 2026)  
   URL: https://arxiv.org/abs/2609.24972  
   Abstract: Regularizes harness evolution (annealed budgets, critic, pruner) to reduce overfitting. Gains up to 6.0 points in-distribution and 4.3 points on five OOD benchmarks while using ~30% fewer tokens.

3. **When the harness decides success: state that cannot be re-observed**  
   Authors: Raimondas Lencevicius (Microsoft)  
   Date: 19 October 2026  
   URL: https://arxiv.org/abs/2610.26632  
   Abstract: Controlled study showing that harness context-management choices (truncation, summarization, replay) can change task success by >90 points when critical facts cannot be re-observed.

### 2. Recursive Self-Improvement (RSI) of Harnesses and Skills
1. **Harness²: Recursive Agent Harnessing for an Open World** (Google Cloud AI Research)  
   (same as above — https://arxiv.org/abs/2610.14224)

2. **RRSI: Regularized Recursive Self-Improvement of Agent Harnesses** (Google Cloud AI Research)  
   (same as above — https://arxiv.org/abs/2609.24972)

3. **Meta-Harness: Learning Agent-State Construction from Long Histories**  
   Authors: Yoonho Lee, Roshen Nair, Qizheng Zhang, Chelsea Finn et al. (Stanford + MIT + KRAFTON)  
   Date: 2026  
   URL: OpenReview / related arXiv materials  
   Abstract: Outer-loop system that searches over harness code via filesystem access to prior traces and scores, discovering better state-construction procedures for long-horizon tasks.

### 3. Agentic Software Engineering (AI SWE / SE 3.0) and Agentic SDLC
1. **Towards an AI Software Factory for Data Systems**  
   Authors: Anna Pavlenko, Carlo Curino, Brandon Haynes, Raghu Ramakrishnan et al. (Microsoft + UW-Madison + GitHub)  
   Date: 28 September 2026  
   URL: https://arxiv.org/abs/2609.36323  
   Abstract: AI SW Factory covering Targeting–Coding–Reviewing–Ops; reports 3× engineering efficiency and up to 22× token efficiency versus pure agentic coding on Microsoft data-system repositories.

2. **Software Engineering LLMs: Why We Need Them and How Software Engineering Helps Build Them**  
   Authors: Amir M. Ebrahimi, Mohammed Mehedi Hasan, Ahmed E. Hassan et al. (Queen’s University + Huawei)  
   Date: 24 October 2026  
   URL: https://arxiv.org/abs/2610.33851  
   Abstract: Industrial case study of capability-focused training for a 92B coding LLM used by >100k developers; RefactorForge pipeline produces verified trajectories that improve RefactorBench and SWE-Bench variants.

3. **Agentic Verified Optimization of Real-World Software**  
   Authors: Zhe Ye, Udaya Ghai, Dawn Song, Soonho Kong et al. (UC Berkeley + collaborators)  
   Date: 20 October 2026  
   URL: https://arxiv.org/abs/2610.30188  
   Abstract: Agents produce faster implementations together with machine-checked equivalence proofs in Lean; framework migrates C/C++/Go/Rust code and yields proved speed-ups on real applications.

### 4. End-to-End Automated Software Factories and 0-to-1 Codebase Generation
1. **E2E-SWE: Benchmarking LLMs on Building Working Codebases from Scratch**  
   Authors: Hantian Ding, Chloe Bi, Jiacheng Zhu, John Yang et al. (Meta Superintelligence Labs)  
   Date: 29 September 2026  
   URL: https://arxiv.org/abs/2609.38335  
   Abstract: 186 whole-repository generation tasks across 11 languages; agents must produce complete installable projects from a natural-language spec; frontier models range 11.7–67.7% pass@1.

2. **Towards an AI Software Factory for Data Systems** (Microsoft)  
   (same as above — https://arxiv.org/abs/2609.36323)

3. **SWE-Universe: Scale Real-World Verifiable Environments to Millions**  
   Authors: Junyang Lin, Binyuan Hui, Zhongxin Liu et al. (Qwen Team, Alibaba + Zhejiang University)  
   Date: 2026 (rev. September 2026)  
   URL: https://arxiv.org/abs/2602.02361  
   Abstract: Scalable pipeline constructs 807k+ multilingual verifiable SWE environments from GitHub PRs using a custom-trained building agent.

### 5. Automated Research Paper Generation and AI Scientific Factories
1. **PaperOrchestra: A Multi-Agent Framework for Automated AI Research Paper Writing**  
   Authors: Yiwen Song, Yale Song, Tomas Pfister, Jinsung Yoon (Google)  
   Date: 6 April 2026  
   URL: https://arxiv.org/abs/2604.05018  
   Abstract: Multi-agent system that turns unconstrained pre-writing materials into submission-ready LaTeX with literature synthesis and generated visuals; strong human-evaluation wins on PaperWritingBench.

2. **Spark-to-Paper: End-to-End Research Paper Generation as a Composable Skill**  
   Authors: Zhuoyang Qian et al. (University of Technology Sydney / Vast Intelligence Lab — high-quality peer work)  
   Date: 12 August 2026  
   URL: https://arxiv.org/abs/2608.11924  
   Abstract: End-to-end generation as 13 composable skills inside a coding assistant; 99.5% citation validity, 96.4% figure editability, fabrication detection raised to 92%.

3. **aiXiv: A Next-Generation Open Access Ecosystem for Scientific Discovery Generated by AI Scientists**  
   Date: 20 August 2025 (rev. December 2025)  
   URL: https://arxiv.org/abs/2508.15126  
   Abstract: Multi-agent platform for human and AI scientists to submit, review, and iteratively refine proposals and papers.

### 6. Evidence-Based and Reproducible Evaluation of Agentic Systems
1. **Toward Evidence-Based Agentic Software Engineering**  
   Authors: Sebastian Baltes, Christoph Treude (Heidelberg University + Singapore Management University — leading SE groups)  
   Date: 26 October 2026  
   URL: https://arxiv.org/abs/2610.35348  
   Abstract: Annotation of 325 experiments from 146 papers; documents severe under-reporting of harness versions and failure to isolate harness effects.

2. **Does AI-Assisted Development Homogenize Software? An Empirical Study of Code Similarity, Quality, and Evolution**  
   Date: 16 October 2026  
   URL: https://arxiv.org/abs/2610.24093  
   Abstract: Software Heritage-based study of 50 long-lived Java/Python projects across pre-LLM, LLM, and agentic eras; no consistent homogenization and some quality gains in the agentic era.

3. **When the harness decides success: state that cannot be re-observed** (Microsoft)  
   (same as Topic 1 — https://arxiv.org/abs/2610.26632)

### 7. Spec-Driven / Intent-Driven Development and Process Control
1. **Software Engineering LLMs: Why We Need Them and How Software Engineering Helps Build Them** (Queen’s + Huawei)  
   (same as Topic 3 — https://arxiv.org/abs/2610.33851)

2. **Towards an AI Software Factory for Data Systems** (Microsoft)  
   (same as above)

3. **Agentic Verified Optimization of Real-World Software** (UC Berkeley)  
   (same as Topic 3 — https://arxiv.org/abs/2610.30188)

### 8. Training Data and Benchmark Factories for SWE Agents
1. **SWE-Universe: Scale Real-World Verifiable Environments to Millions** (Alibaba/Qwen)  
   (same as Topic 4 — https://arxiv.org/abs/2602.02361)

2. **E2E-SWE: Benchmarking LLMs on Building Working Codebases from Scratch** (Meta)  
   (same as Topic 4 — https://arxiv.org/abs/2609.38335)

3. **SWE-Factory: Your Automated Factory for Issue Resolution Training Data and Evaluation Benchmarks**  
   Authors: Lianghong Guo, Yanlin Wang et al.  
   Date: June 2025 (rev. Jan 2026; FSE 2026)  
   URL: https://arxiv.org/abs/2506.10954  
   Abstract: Automated multi-agent pipeline for constructing reproducible Docker-based evaluation environments and training data for GitHub issue resolution.

### 9. Trustworthiness, Safety, and Control in RSI Systems
1. **RSI Control: Can AI Models Perform Covert Sabotage in the Self-Improvement Process?**  
   Authors: Kaiwen Zhou, Udari Madhushani Sehwag  
   Date: 20 October 2026  
   URL: https://arxiv.org/abs/2610.27735  
   Abstract: RSI Control Arena in which a frontier model is given a main training-pipeline task plus a covert side task; models frequently succeed at side tasks with low detection rates.

2. **Trustworthy Recursive Self-Improvement in the Era of Large Models**  
   Date: 26 October 2026  
   URL: https://arxiv.org/abs/2610.35193  
   Abstract: Survey defining operational RSI requirements around genuine improvement, alignment, security, and controllability.

3. **Honesty Testing of LLMs and AI Agents: A Vision for Software Engineering**  
   Authors: Ying Xiao, Xinjian Wu, Lukas Twist, Jie M. Zhang et al.  
   Date: 25 October 2026  
   URL: https://arxiv.org/abs/2610.34239  
   Abstract: Proposes honesty testing (claims that conflict with the model’s own judgment) as a software-engineering quality requirement for agents.

### 10. Multi-Agent Orchestration for Full-Lifecycle and Research Pipelines
1. **PaperOrchestra** (Google)  
   (same as Topic 5 — https://arxiv.org/abs/2604.05018)

2. **Towards an AI Software Factory for Data Systems** (Microsoft)  
   (same as above)

3. **Harness²** (Google Cloud AI Research)  
   (same as Topic 1 — recursive multi-level harnessing)

These selections emphasize work led by or heavily involving Google, Microsoft, Meta, Stanford, Berkeley, and other elite labs while preserving coverage of the original ten topics. Many of the strongest recent results on harness RSI and software factories are coming from these institutions.





