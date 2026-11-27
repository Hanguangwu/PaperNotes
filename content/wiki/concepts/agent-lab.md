---
id: concept:agent-lab
type: concept
title: Agent Laboratory（自主研究智能体实验室）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/autonomous-research, papers/llm-agent]
cssclasses: [concept]
---

## 概念内容

Agent Laboratory 是 LLM 智能体实验室工作流单元，含三阶段循环：**规划**（提出候选方法）→ **实验**（写代码/跑实验）→ **写作/评审**（产出报告）。单轮循环本身是隔离的：每次冷启动，无法利用先前实验室的中间产物。

关键辨析：AgentRxiv 不取代实验室内部流水线，而是补上**跨实例、跨时间的知识总线**——实验室完成后上传报告，其他实验室在文献阶段按相关性检索取回（智能体预印本 + 人类 arXiv 混合检索，避免封闭自指）。

## 在论文中的用法

- [[wiki/sources/AgentRxiv_Towards_Collaborative_Autonomous_Research.zh-CN]]：AgentRxiv 以 Agent Laboratory 为协作基础单元；单实验室约 40 代论文使 MATH-500 准确率从 70.2% 阶梯式升至 78.2%（SDA）。

## 相关概念

- [[wiki/concepts/agent-rxiv]]：Agent Laboratory 的协作层
- [[wiki/concepts/recursive-self-improvement]]：代际累积改进的 RSI 实例
- [[wiki/concepts/llm]]：Agent Laboratory 的底层能力来源
