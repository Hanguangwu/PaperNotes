---
id: concept:layer-pipeline-mapping
type: concept
title: Layer-Pipeline（LP）映射
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/dnn-accelerator, papers/computer-architecture]
cssclasses: [concept]
---

## 概念内容

Layer-Pipeline（LP）映射是把 DNN 的多层在空间上同时映射到加速器核心（spatial mapping）的范式，是学术界与工业界常用的大规模加速器映射手段。其核心决策是 Spatial Mapping（SPM）：「哪一层的哪一部分被分配到哪个核心」。

关键辨析：现有 LP SPM 大多依赖启发式规则，优化空间未被清晰定义与充分探索；Gemini 提出的 Layer-centric 编码把 SPM 从「启发式」提升为「可枚举/可搜索的表示」，使优化空间边界清晰。

## 在论文中的用法

- [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]：定义 LP 空间映射的 Layer-centric 编码，与可配置硬件模板联合搜索；基线采用 SOTA Tangram LP 映射。

## 相关概念

- [[wiki/concepts/chiplet]]：LP 映射在 Chiplet 场景下必须主动减少跨 Die 通信
- [[wiki/concepts/die-to-die]]：D2D 约束决定了映射的可行空间
- [[wiki/concepts/monetary-cost]]：LP 映射与架构协同时把 MC 纳入搜索目标
