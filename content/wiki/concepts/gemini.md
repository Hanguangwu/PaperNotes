---
id: concept:gemini
type: concept
title: Gemini（架构-映射协同探索框架）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/chiplet, papers/dnn-accelerator, papers/computer-architecture]
cssclasses: [concept]
---

## 概念内容

Gemini 是面向大规模 DNN [[wiki/concepts/chiplet]] 加速器的架构与映射协同探索框架：基于 [[wiki/concepts/layer-pipeline-mapping]] 的 Layer-centric 编码与高度可配置硬件模板，把 [[wiki/concepts/monetary-cost|货币成本（MC）]]、性能与能效同时纳入搜索目标，联合探索架构配置与映射方案，而非「先定架构再启发式映射」的串行流程。

关键辨析：相对 SOTA Simba + Tangram 基线，在仅增加 14.3% MC 的前提下平均获得 1.98× 性能与 1.41× 能效提升（论文主结果），并揭示了 Chiplet 粒度选择与减少 D2D 通信的映射策略等可操作洞见。

## 在论文中的用法

- [[wiki/sources/Gemini_Mapping_and_Architecture_Co_exploration_for_Large_scale_DNN_Chiplet_Accelerators]]：框架本身是该工作的核心贡献，已开源（GitHub: SET-Scheduling-Project/GEMINI-HPCA2024）。

## 相关概念

- [[wiki/concepts/chiplet]]：目标硬件范式
- [[wiki/concepts/die-to-die]]：核心约束
- [[wiki/concepts/layer-pipeline-mapping]]：映射范式
- [[wiki/concepts/monetary-cost]]：协同搜索目标
