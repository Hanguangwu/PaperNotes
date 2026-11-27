---
id: concept:recursive-self-improvement
type: concept
title: 递归自我改进（RSI）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/recursive-self-improvement, papers/llm-agent]
cssclasses: [concept]
---

## 概念内容

递归自我改进（Recursive Self-Improvement, RSI）指把交互经验转化为**持久**能力提升的过程：系统在自身更新后仍能继续改进后续行为。难点在于外部反馈成本高昂，且随着策略演化，世界模型预测可能变得不可靠（策略漂移使原模型训练支撑失效）。

关键辨析：RSI 的改进单位不是「最新策略」或「最新模型」，而是**保留的策略–世界模型对**——只有通过组件检查与兼容性检查的更新才被保留并决定下一轮候选分布（见 RSI-Anything 笔记）。这与一次性微调或单轮 RL 有本质区别。

## 在论文中的用法

- [[wiki/sources/RSI-Anything_Unlocking_Recursive_Self-Improvement_with_World_Models]]：提出领域通用的 RSI 框架，在代码修复、GUI、实验室规划、反应优化与免疫科学端到端应用中验证递归收益。
- [[wiki/sources/AgentRxiv_Towards_Collaborative_Autonomous_Research.zh-CN]]：从「共享预印本 + 跨代检索」角度实现研究智能体的代际递增，是 RSI 在科研协作场景的实例。

## 相关概念

- [[wiki/concepts/world-model]]：RSI 循环中的预测组件
- [[wiki/concepts/jepa]]：RSI-Anything 的跨域预测基础（JEPA-Anything / OPF）
- [[wiki/concepts/agent-lab]]：自主研究智能体的工作流单元
