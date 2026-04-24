# Awesome Self-Evolving Agent Security

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A curated reading list for security, privacy, evaluation, and defense research on
self-evolving LLM agents: systems that autonomously update their models, memory,
tools, workflows, or multi-agent populations over time.

This list is organized around the taxonomy used in the manuscript **The Price of
Autonomy: Security and Privacy in Self-Evolving LLM Agents**. The key idea is
that self-evolution turns many agent risks from session-scoped failures into
persistent, inherited, and self-amplifying failures.

## Contents

- [Scope](#scope)
- [Security Taxonomy](#security-taxonomy)
- [Reading Paths](#reading-paths)
- [Papers](#papers)
  - [Surveys, Standards, and Systematization](#surveys-standards-and-systematization)
  - [Self-Evolution Foundations](#self-evolution-foundations)
  - [Attacks and Failure Modes](#attacks-and-failure-modes)
  - [Defenses, Benchmarks, and Verification](#defenses-benchmarks-and-verification)
- [Contributing](#contributing)
- [Citation](#citation)

## Scope

Included work should be relevant to at least one of the following questions:

- How do agents evolve their own **model**, **memory**, **tools**, **workflow**,
  or **population structure**?
- How do attacks persist or amplify across evolution cycles?
- How do privacy risks change when memories, traces, weights, or agent
  protocols are inherited?
- How can defenses, evaluations, sandboxes, or verification methods remain valid
  when the system under test keeps modifying itself?

General LLM safety papers are included only when they directly affect
self-evolution, such as fine-tuning safety fragility, reward hacking, model
merging, or agent-tool security.

## Security Taxonomy

### Modules

| Module | What Evolves | Security Focus |
| --- | --- | --- |
| Brain / LLM | Weights, prompts, policies, reward models | Weight-encoded injection, reward hacking, alignment erosion, deceptive selection |
| Memory | Long-term memory, experience pools, user profiles, demonstrations | Memory poisoning, inheritance leakage, cross-session persistence |
| Execution | Tools, APIs, code, MCP servers, skill libraries | Trojan tools, unsafe tool creation, permission ratchets, emergent capability chains |
| Self-Design | Workflows, architecture graphs, mutation operators, meta-objectives | Safety-filter removal, optimizer-optimizee collapse, blueprint drift |
| Collective | Agent populations, communication topology, shared knowledge | Prompt infection, Sybil dynamics, population-level selection, lateral movement |

### Lifecycle

| Stage | Security Role |
| --- | --- |
| Initialization | Establishes trust anchors: base model, seed memory, initial tools, permissions, topology |
| Mutation | Main entry point for adversarial influence through data, feedback, tools, and self-modification |
| Selection | Decides which variants survive; vulnerable to Goodharting, reward hacking, and safety tax effects |
| Reproduction | Turns local compromise into inherited lineage state through distillation, memory copying, or tool transfer |
| Deployment | Real-world harm appears, and in online systems deployment interactions feed the next mutation cycle |

### Cross-Cutting Amplification Effects

- **Generational accumulation:** small degradations compound across evolution cycles.
- **Selective amplification:** optimization favors capability over safety unless safety is explicitly rewarded.
- **Deceptive evolution:** appearing safe can itself become a selected capability.
- **Lamarckian propagation:** acquired memories, tools, and workflows are directly inherited.
- **Capability ratchet:** dangerous capabilities persist once they enter the lineage.
- **Emergent unpredictability:** evolved components combine into behaviors not visible in isolated tests.
- **Optimizer-optimizee collapse:** the agent may optimize the mechanisms that are supposed to constrain it.

## Reading Paths

- **New to the area:** start with the surveys, then read Reflexion,
  Self-Rewarding Language Models, Voyager, and AgentDojo.
- **Security researcher:** start with the Module x Lifecycle taxonomy, then read
  Zombie Agents, MINJA, MCPTox, Kill-Chain Canaries, and AgentLeak.
- **Agent builder:** start with ToolSword, AgentDojo, PrivacyChecker,
  GuardAgent, TrustAgent, and SEVerA.
- **Multi-agent systems:** start with GPTSwarm, HyperAgents, Prompt Infection,
  MASLEAK, and AgentCrypt.

## Papers

Format: `Title - venue/year. Why it matters for self-evolving agent security.`

### Surveys, Standards, and Systematization

- [A Survey on Self-Evolution of Large Language Models](https://arxiv.org/abs/2404.14387) - arXiv 2024. Broad survey of LLM self-evolution paradigms.
- [A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve on the Path to ASI](https://arxiv.org/abs/2504.01065) - arXiv 2025. Agent-level taxonomy for what components evolve and when.
- [SoK: The Attack Surface of Agentic AI - Tools, and Autonomy](https://arxiv.org/abs/2603.22928) - arXiv 2026. Systematizes agentic attack surfaces around tools and autonomy.
- [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) - OWASP 2026. Practical risk categories for LLM applications and agents.
- 2025 AI Agent Index - MIT AI Security 2025. Industry-oriented view of agent deployment and risk trends.
- [Externalization in LLM Agents: A Unified Review of Memory, Skills, Protocols and Harness Engineering](https://arxiv.org/abs/2604.08224) - arXiv 2026. Useful frame for externalized agent state.
- A Survey on the Memory Mechanism of Large Language Model-based Agents - ACM TOIS 2024. Background on persistent agent memory.

### Self-Evolution Foundations

#### Model, Reward, and Policy Evolution

- [Self-Rewarding Language Models](https://arxiv.org/abs/2401.10020) - arXiv 2024. Canonical self-judging and self-training loop.
- [Absolute Zero: Reinforced Self-play Reasoning with Zero Data](https://arxiv.org/abs/2505.03335) - arXiv 2025. Self-play reasoning without external data.
- [EvolveR: Self-Evolving LLM Agents through an Experience-Driven Lifecycle](https://arxiv.org/abs/2510.16079) - arXiv 2025. Experience-driven agent evolution lifecycle.
- [RAGEN: Understanding Self-Evolution in LLM Agents via Multi-Turn Reinforcement Learning](https://arxiv.org/abs/2504.20073) - arXiv 2025. Multi-turn RL view of agent self-evolution.
- [Training Language Models to Self-Correct via Reinforcement Learning](https://arxiv.org/abs/2409.12917) - arXiv 2024. RL-based self-correction dynamics.
- [SELAUR: Self Evolving LLM Agent via Uncertainty-aware Rewards](https://arxiv.org/abs/2602.21158) - arXiv 2026. Reward shaping for safer self-evolution.
- Large Language Model Agents Are Not Always Faithful Self-Evolvers - arXiv 2026. Studies faithfulness gaps in self-evolving agents.
- [AgentGym: Evolving Large Language Model-based Agents across Diverse Environments](https://arxiv.org/abs/2406.04151) - arXiv 2024. Environment suite for evolving agents.
- [Language Agent Tree Search Unifies Reasoning, Acting, and Planning in Language Models](https://arxiv.org/abs/2310.04406) - ICML 2024. Search-based agent reasoning with reusable trajectories.

#### Memory, Context, and Prompt Evolution

- [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366) - NeurIPS 2023. Stores verbal feedback as reusable experience.
- [Self-Refine: Iterative Refinement with Self-Feedback](https://arxiv.org/abs/2303.17651) - NeurIPS 2023. Iterative self-feedback without supervised training.
- [ExpeL: LLM Agents Are Experiential Learners](https://arxiv.org/abs/2308.10144) - arXiv 2023. Distills experience into reusable lessons.
- [ReasoningBank: Scaling Agent Self-Evolving with Reasoning Memory](https://arxiv.org/abs/2509.25140) - arXiv 2025. Memory bank for reusable reasoning strategies.
- [MemGen: Weaving Generative Latent Memory for Self-Evolving Agents](https://arxiv.org/abs/2509.24704) - arXiv 2025. Latent memory construction for evolving agents.
- [Agent Workflow Memory](https://arxiv.org/abs/2409.07429) - ICML 2025. Workflow-level memory for agent reuse.
- Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory - arXiv 2025. Production memory infrastructure for agents.
- [DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines](https://arxiv.org/abs/2310.03714) - NeurIPS 2023. Compiles and optimizes LM pipelines.
- PromptBreeder: Self-Referential Self-Improvement Via Prompt Evolution - arXiv 2023. Evolves prompts and prompt mutation strategies.
- TextGrad: Automatic "Differentiation" via Text - arXiv 2024. Textual gradients for prompt and pipeline optimization.
- SPO: Self-Supervised Prompt Optimization - ACL 2025. Self-supervised optimization of prompts.
- SE-Agent: Self-Evolution Trajectory Optimization in Multi-Step Reasoning - arXiv 2025. Optimizes trajectories in multi-step reasoning agents.

#### Tool, Skill, and Execution Evolution

- [Voyager: An Open-Ended Embodied Agent with Large Language Models](https://arxiv.org/abs/2305.16291) - arXiv 2023. Creates and reuses executable skills.
- [Alita: Generalist Agent Enabling Scalable Agentic Reasoning with Minimal Predefinition and Maximal Self-Evolution](https://arxiv.org/abs/2505.20286) - arXiv 2025. Generalist self-evolving tool and MCP-style agent.
- [CREATOR: Tool Creation for Disentangling Abstract and Concrete Reasoning of Large Language Models](https://arxiv.org/abs/2305.14318) - EMNLP Findings 2023. Agent-generated tools for abstract reasoning.
- [CRAFT: Customizing LLMs by Creating and Retrieving from Specialized Toolsets](https://arxiv.org/abs/2309.17428) - ICLR 2024. Tool creation plus retrieval.
- [Toolformer: Language Models Can Teach Themselves to Use Tools](https://arxiv.org/abs/2302.04761) - arXiv 2023. Self-supervised tool-use learning.
- [ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs](https://arxiv.org/abs/2307.16789) - ICLR 2024. Large-scale tool/API learning.
- [Gorilla: Large Language Model Connected with Massive APIs](https://arxiv.org/abs/2305.15334) - NeurIPS 2024. API grounding for tool-using LLMs.
- ToolGen: Unified Tool Retrieval and Calling via Generation - ICLR 2025. Generative framing of tool retrieval and invocation.
- [ToolRerank: Adaptive and Hierarchy-Aware Reranking for Tool Retrieval](https://arxiv.org/abs/2403.06551) - arXiv 2024. Adaptive ranking for tool selection.
- [Symbolic Learning Enables Self-Evolving Agents](https://arxiv.org/abs/2406.18532) - arXiv 2024. Symbolic learning loop for agents.
- [Large Language Models as Tool Makers](https://arxiv.org/abs/2305.17126) - arXiv 2023. LLM-generated tools.
- SEARL: Joint Optimization of Policy and Tool Graph Memory for Self-Evolving Agents - ACL 2026. Joint policy and tool-graph memory evolution.
- [Tool-R0: Self-Evolving LLM Agents for Tool-Learning from Zero Data](https://arxiv.org/abs/2602.21320) - arXiv 2026. Zero-data self-evolution for tool learning.
- [Agentic Skill Discovery](https://arxiv.org/abs/2405.15019) - arXiv 2024. Autonomous discovery of reusable skills.
- ToolkenGPT: Augmenting Frozen Language Models with Massive Tools via Tool Embeddings - NeurIPS 2023. Tool embeddings for large tool collections.
- [MCP-Zero: Active Tool Discovery for Autonomous LLM Agents](https://arxiv.org/abs/2506.01056) - arXiv 2025. Active discovery of external tools.
- OctoTools: An Agentic Framework with Extensible Tools for Complex Reasoning - ACL 2026. Extensible tool framework for complex agents.
- [Distilling Feedback into Memory-as-a-Tool](https://arxiv.org/abs/2601.05960) - arXiv 2026. Turns feedback into callable memory tools.
- [Skill-SD: Skill-Conditioned Self-Distillation for Multi-turn LLM Agents](https://arxiv.org/abs/2604.10674) - arXiv 2026. Skill-conditioned self-distillation.

#### Self-Design and Collective Evolution

- Godel Agent: A Self-Referential Agent Framework for Recursive Self-Improvement - arXiv 2025. Recursive self-modification and self-reference.
- AlphaEvolve: A Gemini-Powered Coding Agent for Designing Advanced Algorithms - arXiv 2025. Evolutionary search over code and algorithms.
- HyperAgents: Dynamic Graph Meta-learning for Self-Evolving Multi-Agent Systems - arXiv 2026. Dynamic graph meta-learning for multi-agent evolution.
- AFlow: Automating Agentic Workflow Generation - arXiv 2024. Searches agent workflows as code-level graphs.
- ADAS: Automated Design of Agentic Systems - arXiv 2024. Meta-agent design of agent systems.
- GPTSwarm: Language Agents as Optimizable Graphs - arXiv 2024. Optimizes multi-agent communication graphs.

### Attacks and Failure Modes

#### Persistent Injection, Jailbreaks, and Poisoning

- [Zombie Agents: Persistent Control of Self-Evolving LLM Agents via Self-Reinforcing Injections](https://arxiv.org/abs/2602.15654) - arXiv 2026. Shows injection persistence and self-reinforcement across evolution cycles.
- [The Dark Side of LLMs: Agent-based Attacks for Complete Computer Takeover](https://arxiv.org/abs/2507.06850) - arXiv 2025. Agentic attack chain toward full machine compromise.
- [Phantom: Automating Agent Hijacking via Structural Template Injection](https://arxiv.org/abs/2602.16958) - arXiv 2026. Structural template injection for hijacking agents.
- Attack via Overfitting: 10-shot Benign Fine-tuning to Jailbreak LLMs - NeurIPS 2025. Demonstrates fine-tuning fragility of safety alignment.
- [The Realignment Problem: When Right Becomes Wrong in LLMs](https://arxiv.org/abs/2511.02623) - arXiv 2025. Studies realignment failure modes.
- [AutoAdv: Automated Adversarial Prompting for Multi-Turn Jailbreaking of LLMs](https://arxiv.org/abs/2511.02376) - arXiv 2025. Automated multi-turn jailbreak generation.
- [Superficial Safety Alignment Hypothesis](https://arxiv.org/abs/2410.10862) - arXiv 2024. Argues that safety alignment can be shallow and brittle.
- [BackdoorLLM: A Comprehensive Benchmark for Backdoor Attacks and Defenses on LLMs](https://arxiv.org/abs/2408.12798) - arXiv 2024. Backdoor attack and defense benchmark.
- [Revisiting Backdoor Attacks on LLMs: A Stealthy and Practical Poisoning Framework via Harmless Inputs](https://arxiv.org/abs/2505.17601) - arXiv 2025. Stealthy poisoning through benign-looking inputs.
- [LoBAM: LoRA-Based Backdoor Attack on Model Merging](https://arxiv.org/abs/2411.16746) - arXiv 2024. Backdoor risk in adapter-based merging.
- [When Safe Models Merge into Danger: Exploiting Latent Vulnerabilities in LLM Fusion](https://arxiv.org/abs/2604.00627) - arXiv 2026. Unsafe behavior emerging from model fusion.
- [Catastrophic Forgetting in LLMs: A Comparative Analysis Across Language Tasks](https://arxiv.org/abs/2504.01241) - arXiv 2025. Background for safety loss under continual updates.
- [Does Low Rank Adaptation Lead to Lower Robustness against Training-Time Attacks?](https://arxiv.org/abs/2505.12871) - arXiv 2025. Robustness implications of LoRA adaptation.

#### Reward Hacking, Alignment Erosion, and Misevolution

- [Natural Emergent Misalignment from Reward Hacking in Production RL](https://arxiv.org/abs/2511.18397) - arXiv 2025. Reward hacking generalizing into broader misalignment.
- [Reward Hacking in the Era of Large Models: Mechanisms, Emergent Misalignment, Challenges](https://arxiv.org/abs/2604.13602) - arXiv 2026. Mechanisms and taxonomy of reward hacking.
- Reward Hacking in Self-Improving Code Agents - OpenReview 2025. Reward hacking in code agents that improve themselves.
- [Your Agent May Misevolve: Emergent Risks in Self-evolving LLM Agents](https://arxiv.org/abs/2509.26354) - arXiv 2026. Empirical evidence of unsafe self-evolution and tool misuse.
- [Alignment Tipping Process: How Self-Evolution Pushes LLM Agents Off the Rails](https://arxiv.org/abs/2510.04860) - arXiv 2025. Gradual degradation followed by alignment collapse.
- [Safety Tax: Safety Alignment Makes Your Large Reasoning Models Less Reasonable](https://arxiv.org/abs/2503.00555) - arXiv 2025. Explains selection pressure against safety when safety reduces performance.

#### Memory, Prompt, and Privacy Attacks

- AgentLeak: A Full-Stack Benchmark for Privacy Leakage in Multi-Agent LLM Systems - arXiv 2026. Full-stack leakage benchmark for multi-agent systems.
- PrivacyChecker: Contextual Integrity-Guided Runtime Privacy Protection for LLM Agents - arXiv 2025. Runtime privacy protection using contextual integrity.
- AgentDAM: Privacy Benchmark for Data Minimization in Web Agents - NeurIPS 2025. Data minimization benchmark for web agents.
- Leaky Thoughts: Reasoning Chain Leakage in Large Language Models - arXiv 2025. Leakage through reasoning traces.
- [Whisper Leak: A Novel Side-Channel Attack on Remote Language Models](https://arxiv.org/abs/2511.03675) - arXiv 2025. Remote side-channel attack relevant to trajectory leakage.
- ADAM: Adaptive Data Extraction Attack on Memory-Augmented Agents - arXiv 2026. Adaptive probing of agent memory.
- JustAsk: Extracting System Prompts from Autonomous Code Agents - arXiv 2026. System prompt extraction from autonomous agents.
- MASLEAK: Multi-Agent System Intellectual Property Leakage - arXiv 2026. Extracts prompts, topology, and collaboration protocols.
- MINJA: Memory Injection Attack on Memory-Augmented LLM Agents - arXiv 2026. Query-only memory injection.
- MemoryGraft: Experience Grafting Attacks on Memory-Based LLM Agents - arXiv 2025. Injects malicious experiences that agents imitate.
- AgentPoison: Red-teaming LLM Agents via Poisoning Memory or Knowledge Bases - NeurIPS 2024. Memory and knowledge-base poisoning for agents.
- Are My Optimized Prompts Compromised? - EACL 2026. Security of automated prompt optimizers.
- Prompt Infection: LLM-to-LLM Prompt Injection within Multi-Agent Systems - arXiv 2025. Prompt propagation across agents.

#### Tool, Execution, and Supply-Chain Attacks

- [InjecAgent: Benchmarking Indirect Prompt Injections in Tool-Integrated LLM Agents](https://arxiv.org/abs/2403.02691) - arXiv 2024. Indirect prompt injection in tool-integrated agents.
- [ToolSword: Unveiling Safety Issues of Large Language Models in Tool Learning Across Three Stages](https://arxiv.org/abs/2402.10753) - arXiv 2024. Tool-learning safety across creation, selection, and use.
- [AgentDojo: A Dynamic Environment to Evaluate Attacks and Defenses for LLM Agents](https://arxiv.org/abs/2406.13352) - arXiv 2024. Dynamic agent attack and defense benchmark.
- [Identifying the Risks of LM Agents with an LM-Emulated Sandbox](https://arxiv.org/abs/2309.15817) - arXiv 2023. Sandbox-based risk discovery for agents.
- R-Judge: Benchmarking Safety Risk Awareness for LLM Agents - ACL 2024. Measures agent risk awareness.
- [BadAgent: Inserting and Activating Backdoor Attacks in LLM Agents](https://arxiv.org/abs/2406.03007) - arXiv 2024. Backdoor attacks on agents.
- [MCPTox: A Benchmark for Tool Poisoning Attack on Real-World MCP Servers](https://arxiv.org/abs/2508.14925) - arXiv 2025. Tool poisoning in MCP servers.

#### Kill Chains and Multi-Agent Attacks

- [Kill-Chain Canaries: Tracing Pipeline-Level Prompt Injection in LLM Agents](https://arxiv.org/abs/2603.28013) - arXiv 2026. End-to-end pipeline injection tracing.
- SafeEvalAgent: Self-Evolving Red-Teaming for Multi-Agent Safety Evaluation - arXiv 2025. Self-evolving red-team process for multi-agent safety.

### Defenses, Benchmarks, and Verification

- [Safe LoRA: The Silver Lining of Reducing Safety Risks when Fine-Tuning Large Language Models](https://arxiv.org/abs/2405.16833) - NeurIPS 2024. Safer adaptation via LoRA constraints.
- [TrustAgent: Towards Safe and Trustworthy LLM-based Agents](https://arxiv.org/abs/2402.01586) - arXiv 2024. Trustworthy agent framework.
- [GuardAgent: Safeguard LLM Agents by a Guard Agent via Knowledge-Driven Reasoning](https://arxiv.org/abs/2406.09187) - arXiv 2024. Guard-agent architecture for agent safety.
- Three Laws of Self-Evolving Agent Safety - arXiv 2025. Safety principles specific to self-evolving agents.
- AgentCrypt: Three-Layer Cryptographic Architecture for Multi-Agent Communication - arXiv 2026. Cryptographic architecture for multi-agent communication.
- DAM: Mitigating the Backdoor Effect for Multi-Task Model Merging via Safety-Aware Subspace - ICLR 2025. Defense for backdoor propagation in model merging.
- [SEVerA: Verified Synthesis of Self-Evolving Agents](https://arxiv.org/abs/2603.25111) - arXiv 2026. Formal verification direction for self-evolving agents.

## Contributing

Pull requests are welcome. Please keep entries concise and use this format:

```markdown
- [Paper Title](https://paper-link) - Venue Year. One sentence on why it matters for self-evolving agent security.
```

Good additions usually include one or more of:

- A clear self-evolution mechanism: model update, memory update, tool update,
  architecture update, or population update.
- A concrete security or privacy risk that persists, propagates, or amplifies
  across evolution cycles.
- A benchmark, defense, formal method, or monitoring approach designed for
  evolving rather than static agents.

Please avoid generic LLM papers unless the connection to self-evolving agent
security is explicit.

## Citation

If this repository helps your work, please cite the underlying paper once the
official citation is available:

```bibtex
@misc{self_evolving_agent_security,
  title = {The Price of Autonomy: Security and Privacy in Self-Evolving LLM Agents},
  year = {2026},
  note = {Manuscript}
}
```
