---
title: "AgentRxiv: Towards Collaborative Autonomous Research"
alias: "AgentRxiv：迈向协作式自主研究"
paper_id: arxiv:2503.18102
arxiv: 2503.18102
authors: [Samuel Schmidgall, Michael Moor]
institution: Johns Hopkins University（ECE）、ETH Zurich（D-BSSE）
venue: arXiv preprint（cs.AI）
year: 2025
paper_type: method
paper_url: https://arxiv.org/pdf/2503.18102
code: https://AgentRxiv.github.io
created: 2026-11-25
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/AI, papers/llm-agent, papers/autonomous-research, papers/multi-agent]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> AgentRxiv 给自主研究智能体加上可检索的共享预印本层，使单实验室与多实验室都能在彼此工作之上迭代，从而在 MATH-500 等基准上实现可量化、可泛化的推理技术改进（`gpt-4o mini` 从 70.2% 提升至 78.2%，相对基线 +11.4%）。

## 原文摘要（译）

科学发现很少来自单一的「尤里卡」时刻，而往往是数百名科学家朝着共同目标逐步协作的产物。现有智能体工作流虽能自主产出研究，却彼此隔离，无法持续改进先前结果。为此，本文提出 AgentRxiv——一个让 LLM 智能体实验室向共享预印本服务器上传与检索报告的框架，以便协作、共享洞见并迭代构建彼此的研究。作者让智能体实验室开发新的推理与提示技术，发现：能够访问自身先前研究的智能体，相对隔离运行的智能体取得更高性能提升（在 MATH-500 上相对基线 +11.4%）；表现最佳的策略可泛化到其他领域基准（平均提升约 3.3%）；多个智能体实验室通过 AgentRxiv 共享研究时，比隔离实验室更快推进，整体准确率更高（MATH-500 上相对基线 +13.7%）。这些发现表明，自主智能体可能与人类一道参与设计未来 AI 系统。作者希望 AgentRxiv 能让智能体朝研究目标协作，并帮助研究者加速发现。

## 研究问题

现有自主研究智能体（如 AI Scientist、Agent Laboratory）通常**隔离**运行，无法像真实科学共同体那样持续积累知识。核心问题：

1. 若智能体可访问自身及他者的先前报告，能否产生可测的、代际递增的性能提升？
2. 在固定研究方向上发现的推理/提示技术，能否迁移到其他基准与模型？
3. 多个实验室并行共享是否比单实验室更快逼近更高准确率？代价是什么？

## 核心贡献

1. **共享预印本服务器作为智能体记忆与协作层**：AgentRxiv 让各 Agent Laboratory 上传/检索报告，使研究可跨实验室、跨代际累积，而不是每次从零开始。
2. **单实验室迭代可测的定量改进**：在固定研究方向（提升 MATH-500）下，约 40 代论文使 `gpt-4o mini` 从 70.2% 升至 78.2%（相对 +11.4%），并发现如 Simultaneous Divergence Averaging（SDA）等方法。
3. **发现算法的跨基准与跨模型泛化**：SDA 等在 GPQA、MMLU-Pro、MedQA 及多种 LLM 上带来平均约 3.3%–9.3% 量级的增益（视设定）。
4. **并行多实验室模式**：多个实验室同时运行并共享发现，可加快墙上时钟进度（如 3 个并行实验室约 +6.0%），同时揭示速度与算力成本的权衡。

## 数据与任务定义

**研究任务（主实验）**

- 研究方向（非具体 idea）：*Improve accuracy on MATH-500 using reasoning and prompt engineering.*
- 实验模型：`gpt-4o mini`（评测与多数实验）
- 规划/写作后端：如 `o3-mini`（medium）
- 单实验室约 **N=40** 代论文；文献阶段可检索 AgentRxiv 上 N=5 篇 + arXiv N=5 篇

**评测基准**

| 基准 | 用途 |
|------|------|
| MATH-500 | 主优化目标与迭代曲线 |
| GPQA Diamond | 跨域泛化 |
| MMLU-Pro | 跨域泛化 |
| MedQA | 跨域泛化 |

**对照与消融**

- 有/无访问先前智能体论文
- 单实验室 vs 多实验室并行共享
- 0-shot / CoT 等基线对比新发现方法

## 方法主线

### 总体架构

在既有 **Agent Laboratory** 三阶段工作流（规划 → 实验 → 写作/评审）之上，增加集中式 **AgentRxiv** 预印本服务：实验室完成后上传报告；其他实验室在文献阶段按相关性检索并排名，取 top 结果作为后续 idea 与方法的输入。

### 机制流程

1. **人类给定研究方向与约束**：例如固定评测模型、API、目标基准，而不写死具体算法 idea。
2. **单轮实验室循环**：规划提出候选方法 → 写代码/跑实验 → 写报告 →（可选）自动评审 → **上传 AgentRxiv**。
3. **跨代/跨实验室检索**：后续实验室在文献阶段查询 AgentRxiv，按相关性取回先前报告，在此基础上提出新方法。
4. **并行模式**：多个实验室同时跑，共享同一 AgentRxiv；发现在墙上时钟上更快汇聚，但总计算成本上升。

![Figure 1 协作式自主研究框架](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261126232801514.png)

*Figure 1：人类给出研究方向与笔记；多个智能体实验室围绕共享 AgentRxiv 上传/检索，并涌现 PCC、DCCP、SDA 等方法名。*

### 关键设计细节

- **可定量的研究方向**：把「做研究」落在 MATH-500 准确率上，便于画代际曲线与做消融。
- **检索混合**：AgentRxiv（智能体论文）+ arXiv（人类论文），避免封闭自指。
- **发现物以可运行方法为主**：如 SDA（Simultaneous Divergence Averaging），需温度采样等多路径 divergence 再平均。

## 关键结果

### 单实验室迭代（MATH-500）

`gpt-4o mini` 0-shot 基线 **70.2%**。随论文代数增加，最佳准确率阶梯上升：早期如 DCCP / CRUC 小幅提升；中期 Dual-Rebuttal CoT Voting、Meta-Mirror Prompting 进入约 72–73%；后期 Dual-Role Divergence、Enhanced CoT Verification 约 74–75%；最终 **SDA 达到 78.2%**（0-shot 相对 **+11.4%**；相对 CoT 基线约 +9.7%）。

### 跨基准泛化（SDA）

| 基准 | 基线 | +SDA | 相对变化（约） |
|------|------|------|----------------|
| GPQA | 36.4% | 38.9% | +6.8% |
| MMLU-Pro | 63.1% | 70.8% | +12.2% |
| MedQA | 74.9% | 81.6% | +8.9% |

三基准平均约 **+9.3%**，与 MATH-500 上的相对增益同量级；文中亦报告跨多种 LLM 的平均约 **+3.3%** 量级增益（视模型与设定）。

### 并行多实验室

3 个并行实验室共享 AgentRxiv 时，MATH-500 改进可再加速（文中约 **+6.0%** 量级的并行收益）；整体相对基线可达约 **+13.7%**。并行提高墙上时钟进度，但总 token/算力成本更高，存在速度–效率权衡。

### 消融含义

去掉对先前智能体论文的访问后，代际提升明显变弱，说明 **AgentRxiv 检索是累积改进的关键机制**，而非单纯「多跑几次实验」。

## 深度分析

### 为何需要「智能体预印本」

真实科学的瓶颈往往是**可见的中间产物**（失败尝试、部分有效技巧、负面结果）。隔离的 agent workflow 每次冷启动，等于丢弃这些中间态。AgentRxiv 把「可检索的报告」变成多智能体系统的外部记忆与信用分配界面。

### 可优化目标的选择

把研究方向锁在 MATH-500 上，使「研究质量」可画成单调可比较的曲线，这是工程上聪明的选择；但也带来风险：智能体可能过拟合评测协议或提示模板，而非发现更一般的推理原则。跨基准与跨模型实验部分缓解了这一点，但仍需警惕基准泄漏与评测噪声。

### 并行的经济学

并行共享提高发现速度，但边际实验室的成本是否值得，取决于人类用户的时间价值与 API 预算。论文把这一权衡显式提出，比只报「更快」更有用。

### 与 AI Scientist / Agent Laboratory 的关系

AgentRxiv 不是取代实验室内部流水线，而是补上**跨实例、跨时间的知识总线**。它与人类 arXiv 并存：智能体既读人类文献，也读彼此的预印本。

## 局限

- 实验主要围绕提示/推理技巧与 MATH-500 类基准，尚未展示在需要真实实验设备或长期代码库维护的科学领域中的同等累积。
- SDA 等方法依赖温度采样等设置，对部分推理模型不适用。
- 报告质量、检索相关性与「虚假改进」（评测方差、数据泄漏）的系统审计有限。
- 并行模式的成本曲线未充分展开为可复现的预算表。
- 人类仍提供研究方向与工具约束，完全开放式科学目标下的稳定性未知。

## 我的笔记

**可复用设计**

- **把协作做成基础设施**：上传/检索 API + 相关性排序，而不是只做一次 multi-agent 对话。
- **研究方向要可度量**：否则无法画「代际是否真的在进步」的曲线。
- **消融「能否读历史」**：这是证明累积（而非运气）的最小实验。
- **并行时同时报速度与总成本**。

**实现检查清单**

1. 报告 schema 固定（问题、方法、超参、指标、失败模式）。
2. 检索返回 top-k 并记录引用，便于追溯「谁影响了谁」。
3. 主指标与保留测试集分离，防止智能体改测试协议。
4. 对发现方法做跨模型、跨基准最小迁移包。

**后续问题**

- AgentRxiv 上的负面结果与失败实验是否会被检索到并避免重复？
- 在软件工程或实验科学目标上，累积曲线是否仍单调？
- 检索器若改为基于方法嵌入而非文本相关，是否减少「时髦但无效」的引用？

## 引用

```bibtex
@article{schmidgall2025agentrxiv,
  title   = {AgentRxiv: Towards Collaborative Autonomous Research},
  author  = {Schmidgall, Samuel and Moor, Michael},
  journal = {arXiv preprint arXiv:2503.18102},
  year    = {2025},
  url     = {https://arxiv.org/abs/2503.18102}
}
```

- 论文页面：[https://arxiv.org/abs/2503.18102](https://arxiv.org/abs/2503.18102)
- PDF：[https://arxiv.org/pdf/2503.18102](https://arxiv.org/pdf/2503.18102)
- 项目页：[https://AgentRxiv.github.io](https://AgentRxiv.github.io)
