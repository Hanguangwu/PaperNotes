---
id: concept:agent-rxiv
type: concept
title: AgentRxiv（智能体共享预印本服务器）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/autonomous-research, papers/multi-agent]
cssclasses: [concept]
---

## 概念内容

AgentRxiv 是面向 LLM 智能体实验室的**共享预印本服务器**：各实验室完成后上传报告，其他实验室在文献阶段按相关性检索并排名，取 top 结果作为后续 idea 与方法的输入。它把「可检索的报告」变成多智能体系统的外部记忆与信用分配界面，使研究可跨实验室、跨代际累积。

关键辨析：消融显示去掉对先前智能体论文的访问后，代际提升明显变弱——AgentRxiv 检索是累积改进的关键机制，而非「多跑几次实验」。并行多实验室共享同一 AgentRxiv 时墙上时钟进度更快，但总算力成本上升，存在速度–效率权衡（3 个并行实验室约 +6.0%，整体相对基线约 +13.7%）。

## 在论文中的用法

- [[wiki/sources/AgentRxiv_Towards_Collaborative_Autonomous_Research.zh-CN]]：框架本身是该工作的核心贡献，已开源（https://AgentRxiv.github.io）。

## 相关概念

- [[wiki/concepts/agent-lab]]：协作的基础单元
- [[wiki/concepts/recursive-self-improvement]]：AgentRxiv 实现的代际递增 RSI
- [[wiki/concepts/code-intelligence]]：AgentRxiv 发现的方法（如 SDA）服务于代码/推理类基准
