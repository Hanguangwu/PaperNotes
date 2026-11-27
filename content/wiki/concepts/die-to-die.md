---
id: concept:die-to-die
type: concept
title: Die-to-Die（D2D）互连
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/chiplet, papers/computer-architecture]
cssclasses: [concept]
---

## 概念内容

Die-to-Die（D2D）互连是 Chiplet 架构中裸片（Die）之间的通信接口。相比片上互连（NoC），D2D 链路占用更多面积、消耗更高功率、提供更低带宽，是 [[wiki/concepts/chiplet]] 设计中的「一等公民」约束：它同时决定了架构侧的可行成本（封装成本）与映射侧的通信代价。

关键辨析：D2D 的能耗/带宽/面积特性是工艺与封装技术相关的参数，建模结果需按工艺重新标定（见 Gemini 笔记「复现与扩展注意点」）。

## 在论文中的用法

- [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]：把 D2D 特性纳入货币成本（MC）模型；Layer-centric 编码使搜索器优先把高数据量层间依赖放在同一 Die 内或 D2D 带宽更充裕的路径上，显著降低通信瓶颈。

## 相关概念

- [[wiki/concepts/chiplet]]：D2D 是 Chiplet 架构的核心代价来源
- [[wiki/concepts/layer-pipeline-mapping]]：映射策略需主动减少跨 Die 通信
- [[wiki/concepts/monetary-cost]]：D2D 面积/功耗折算为货币成本
