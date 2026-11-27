---
id: concept:monetary-cost
type: concept
title: 货币成本（Monetary Cost, MC）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/chiplet, papers/computer-architecture]
cssclasses: [concept]
---

## 概念内容

货币成本（Monetary Cost, MC）把硬件的面积、良率、封装与 [[wiki/concepts/die-to-die|D2D]] 开销折算为可比较的成本指标，与性能（延迟）和能效（能耗/EDP）并列作为优化目标。

关键辨析：传统架构搜索中成本常是事后统计；Gemini 将 MC 显式纳入搜索目标，避免「性能很好但封装成本不可接受」的方案。代价是 MC 模型依赖 D2D 能耗/带宽、良率曲线、封装成本等参数，换工艺需重新标定。

## 在论文中的用法

- [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]：在仅增加 14.3% MC 的前提下获得平均 1.98× 性能与 1.41× 能效提升，证明「架构-映射协同 + MC 目标」的可行性。

## 相关概念

- [[wiki/concepts/chiplet]]：Chiplet 粒度直接决定封装与 D2D 成本
- [[wiki/concepts/die-to-die]]：D2D 面积/功耗是 MC 的主要构成
- [[wiki/concepts/layer-pipeline-mapping]]：协同探索中 MC 与性能/能效同时进入搜索目标
