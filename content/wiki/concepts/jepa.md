---
id: concept:jepa
type: concept
title: JEPA（联合嵌入预测架构）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/world-model]
cssclasses: [concept]
---

## 概念内容

JEPA（Joint-Embedding Predictive Architecture）是表征预测架构：不在像素/词元空间预测，而是在潜在表征空间预测目标表征。RSI-Anything 所基于的 `JEPA-Anything` 进一步引入**正交预测分解（OPF）**：将潜在目标分解为互补因子，分别学习预测器，再重组为完整潜在状态，提供跨域表征预测接口。

关键辨析：JEPA 类模型预测的是「表征」而非原始观测，适合做候选动作的前瞻评估与信息增益排序，是 RSI-Anything 跨域通用性的第一层抽象（见 RSI-Anything 笔记「跨域通用性的来源」）。

## 在论文中的用法

- [[wiki/sources/RSI-Anything_Unlocking_Recursive_Self-Improvement_with_World_Models]]：RSI-Anything 建立在 JEPA-Anything 的跨域预测基础之上，域侧只负责动作编码、结果读出与启用路径配置。

## 相关概念

- [[wiki/concepts/world-model]]：JEPA 是 RSI 场景下世界模型的具体实现路线
- [[wiki/concepts/recursive-self-improvement]]：JEPA 预测服务其中的选择/数据筛选环节
