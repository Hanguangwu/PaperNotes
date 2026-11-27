---
id: concept:world-model
type: concept
title: 预测世界模型（World Model）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: atomic
tags: [type/concept, papers/world-model, papers/llm-agent]
cssclasses: [concept]
---

## 概念内容

预测世界模型（world model）是对环境状态演化的预测器，能前瞻评估动作后果。在 RSI 语境下，它承担三个作用：① 按预期效用/信息增益对候选动作排序；② 引导学习数据与反馈的收集；③ 在可靠性检查通过时被条件性适应（更新）。

关键辨析：策略更新会改变候选分布，使原世界模型的预测支撑失效——因此「策略演化下的预测可靠性」必须作为一等设计约束：可靠性评估限定在「当前模型版本 + 已覆盖候选范围」，版本感知、覆盖敏感，不足时返回「insufficient」而非盲目触发适应（见 RSI-Anything 笔记「可靠性与覆盖的边界」）。

## 在论文中的用法

- [[wiki/sources/RSI-Anything_Unlocking_Recursive_Self-Improvement_with_World_Models]]：世界模型预测引导动作/数据选择；固定世界模型时代码修复成功率 26.3% → 29.2%，世界模型适应后 → 31.2%（在线调用次数相当）。
- [[wiki/sources/AgentRxiv_Towards_Collaborative_Autonomous_Research.zh-CN]]：其「共享预印本」可视为跨实验室的知识预测/检索层，与单系统内的世界模型互补。

## 相关概念

- [[wiki/concepts/recursive-self-improvement]]：世界模型服务的目标框架
- [[wiki/concepts/jepa]]：JEPA-Anything 的跨域表征预测接口
- [[wiki/concepts/scam-town]]：LLM 居民仿真中的「呼叫层 + 故事层」双过程模型
