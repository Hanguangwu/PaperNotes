---
id: concept:chiplet
type: concept
title: Chiplet
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: mechanism
tags: [type/concept, papers/chiplet, papers/computer-architecture]
cssclasses: [concept]
---

## 概念内容

Chiplet（小芯片）指将多个功能相对独立的小芯片（Die）通过先进封装集成到同一加速器/系统中的技术路线。在后摩尔时代，单芯片面积与光罩尺寸受限，Chiplet 能以更高良率扩大晶体管规模，是扩展 DNN 加速器规模的主要手段（见 [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]）。

关键辨析：Chiplet 在提升良率与规模的同时，引入 Die-to-Die（D2D）接口——相比片上互连，D2D 占用更多面积、消耗更高功率、带宽更低，并带来额外的封装（货币）成本。因此 Chiplet 设计的核心矛盾是「粒度权衡」：更多更小的 Chiplet 良率更高，但封装与 D2D 开销更大。

## 在论文中的用法

- [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]：将 Chiplet 粒度（Die 数量与大小）作为可配置架构参数，结合 D2D 成本做架构-映射协同探索，指出 EDP 随核心数呈非单调曲线，存在「甜蜜点」。

## 相关概念

- [[wiki/concepts/die-to-die]]：Chiplet 的核心代价来源
- [[wiki/concepts/layer-pipeline-mapping]]：DNN 在 Chiplet 加速器上的主流映射范式
- [[wiki/concepts/monetary-cost]]：把封装/D2D 成本货币化的评价维度
