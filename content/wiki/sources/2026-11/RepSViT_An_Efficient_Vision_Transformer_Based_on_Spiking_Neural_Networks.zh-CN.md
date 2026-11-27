---
title: "RepSViT: An Efficient Vision Transformer Based on Spiking Neural Networks for Object Recognition in Satellite On-Orbit Remote Sensing Images"
alias: "RepSViT：基于脉冲神经网络的高效视觉 Transformer，用于卫星在轨遥感图像目标识别"
paper_id: doi:10.1109/TGRS.2024.3367709
authors: [Yanhua Pang, Libo Yao, Yiping Luo, Chengguo Dong, Qinglei Kong, Bo Chen]
institution: Institute of Space Science and Applied Technology, Harbin Institute of Technology（Shenzhen）等
venue: IEEE Transactions on Geoscience and Remote Sensing（Vol. 62，pp. 1–16）
year: 2024
paper_type: method
paper_url: https://ieeexplore.ieee.org/document/10440355
created: 2026-11-26
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/satellite, papers/remote-sensing, papers/spiking-neural-network, papers/vision-transformer, papers/edge-inference]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> RepSViT 将脉冲神经网络（SNN）与结构重参数化融合进视觉 Transformer，设计 D²SC、SGAM、ECFC、RepFFN 等模块，在星上资源极度受限条件下实现遥感目标识别的高精度（约 95%）与低时延（嵌入式 GPU 8.33 ms）推理。

## 原文摘要（译）

卫星在轨计算正从备份手段转变为主要关键功能。然而，卫星有限的计算资源难以部署参数庞大的先进模型；同时，在轨计算对速度与精度要求极高，给适用模型设计带来显著挑战。为应对这些挑战，本文提出面向卫星在轨计算的高效视觉 Transformer——RepSViT。该模型首次将具有高生物合理性、事件驱动特性与低功耗的脉冲神经网络（SNN）引入遥感图像处理与卫星在轨计算领域，并融合结构重参数化技术。具体而言，设计了基于 SNN 的动态扩张脉冲卷积（D²SC），以提升特征提取能力与效率；开发了脉冲引导注意力模块（SGAM），使模型在更低计算成本下更关注目标相关特征；进一步设计高效耦合细–粗粒度块（ECFC）以增强粗细粒度特征提取能力；并设计重参数化前馈网络（RepFFN），在保证有效特征提取的同时提升推理速度并降低计算开销。RepSViT 在嵌入式 GPU 上实现 8.33 ms 推理时延与 95% 识别准确率，参数量 3.77M，计算量仅 0.6 GFLOPs。

## 研究问题

1. 如何在卫星严格功耗、算力与存储限制下部署具有竞争力的视觉识别模型？
2. 如何将 SNN 的事件驱动与低功耗优势有效融入 Transformer 架构，并保持对遥感目标的表征能力？
3. 如何通过结构重参数化与注意力/多粒度设计，在训练表达力与推理效率之间取得平衡？

## 核心贡献

1. **首次将 SNN 引入遥感在轨计算**：利用 SNN 的事件驱动与低功耗特性适配星上资源约束。
2. **D²SC（Dynamic Dilated Spiking Convolution）**：动态扩张脉冲卷积，兼顾感受野与计算效率。
3. **SGAM（Spiking Guided Attention Module）**：脉冲引导注意力，以较低开销聚焦目标相关特征。
4. **ECFC + RepFFN**：高效耦合细–粗粒度块与重参数化前馈网络，结合结构重参数化，训练时保持表达能力、推理时折叠为高效结构。
5. **在轨部署的量化平衡**：在嵌入式 GPU 上实现高准确率（约 95%）与低时延（8.33 ms）、低参数/计算量的平衡，直接面向在轨部署需求。

## 数据与任务定义

**任务**：卫星在轨遥感图像目标识别（分类/识别导向）。

**主要数据集**（公开信息）

- FGSC-23
- RSIOR-9

**评估指标**

- 识别准确率（最高报告约 0.97 / 平均 95%）
- 推理时延（嵌入式 GPU 上 8.33 ms）
- 参数量（3.77M）
- 计算量（0.6 GFLOPs）

**部署场景**：嵌入式 GPU，模拟/面向卫星在轨计算环境。

## 方法主线

### 机制流程

RepSViT 整体为轻量视觉 Transformer，核心创新模块包括：

1. **D²SC（动态扩张脉冲卷积）**
   基于 SNN 设计，引入动态扩张以扩大有效感受野，同时保持脉冲稀疏性与低计算开销，增强特征提取能力。
2. **SGAM（脉冲引导注意力模块）**
   利用脉冲活动引导注意力权重，使网络更关注目标相关区域，降低无关背景计算成本。
3. **ECFC（高效耦合细–粗粒度块）**
   同时建模细粒度细节与粗粒度上下文，提升对多尺度遥感目标的表征。
4. **RepFFN（重参数化前馈网络）**
   训练阶段使用多分支结构增强表达，推理阶段重参数化为单一高效路径，降低时延与 FLOPs。

整体流水线：输入遥感图像 → 脉冲卷积特征提取（D²SC）→ 脉冲引导注意力与多粒度耦合（SGAM + ECFC）→ 重参数化 FFN → 分类头输出识别结果。

![](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261127140414385.png)

## 关键结果

公开报告的核心数字：

| 指标 | 数值 |
|------|------|
| 嵌入式 GPU 推理时延 | 8.33 ms |
| 识别准确率 | 95%（部分设置/表中可达 0.97） |
| 参数量 | 3.77 M |
| 计算量 | 0.6 GFLOPs |

在 FGSC-23 与 RSIOR-9 上的对比实验中，RepSViT 在准确率上优于或与多个轻量 SOTA 方法相当，同时保持有竞争力的时延与计算成本，满足在轨场景对速度与资源的双重约束。消融实验验证了 D²SC、SGAM、ECFC、RepFFN 等组件的有效贡献。

## 深度分析

### 为什么 SNN + 重参数化适合在轨场景

- 卫星功率、散热与算力预算极紧，传统大参数 CNN/Transformer 难以直接部署。
- SNN 的事件驱动特性天然稀疏，理论上可进一步降低能耗；结构重参数化则解决了“训练复杂、推理简单”的经典矛盾。
- 将两者结合进 ViT 骨架，是对“轻量 + 低功耗 + 足够表征力”三角的一次针对性探索。

### 与同类工作的定位

- 相对纯 CNN 轻量模型：引入 Transformer 全局建模与 SNN 脉冲机制。
- 相对通用轻量 ViT：针对遥感多尺度目标与星上部署约束做了专用模块设计。
- 相对已有遥感 SNN 工作：首次系统性地将 SNN 与重参数化 ViT 结合，并给出嵌入式实测时延与准确率。

### 工程价值

8.33 ms / 3.77M / 0.6 GFLOPs 的指标组合，对在轨实时目标识别具有直接参考意义；若后续能配合神经形态芯片，功耗优势可能进一步放大。

## 局限

1. 完整实现细节、超参数、完整消融表与可视化受全文不可得限制，无法逐层验证。
2. 公开结果主要基于分类/识别数据集，对检测、分割等更复杂在轨任务的泛化性有待进一步验证。
3. SNN 在真实神经形态硬件上的能效收益尚未在文中（公开部分）给出实测对比。
4. 与最新轻量 ViT / 重参数化模型（如 RepViT 系列）在相同硬件上的直接对比有限。

## 我的笔记

- **问题驱动清晰**：直接回应“星上资源不够、又要又快又准”的真实矛盾，而非单纯刷榜。
- **模块设计有针对性**：D²SC 解决感受野与稀疏性，SGAM 解决注意力开销，RepFFN 解决训练–推理鸿沟，逻辑链条完整。
- **指标实用**：时延 + 参数 + FLOPs 三元组比单纯 Top-1 更贴近在轨部署决策。
- **后续值得关注**：
  1. 在真实星载硬件（或神经形态芯片）上的端到端功耗与热测试；
  2. 向目标检测 / 变化检测等任务扩展；
  3. 与量化、剪枝、知识蒸馏等其他压缩手段的组合；
  4. 开源代码与预训练权重的公开情况。
- 整体是一篇面向卫星在轨智能的扎实轻量模型工作，SNN + 重参数化的思路对边缘/星上视觉有参考价值。

## 引用

```bibtex
@article{pang2024repsvit,
  title   = {RepSViT: An Efficient Vision Transformer Based on Spiking Neural Networks for Object Recognition in Satellite On-Orbit Remote Sensing Images},
  author  = {Pang, Yanhua and Yao, Libo and Luo, Yiping and Dong, Chengguo and Kong, Qinglei and Chen, Bo},
  journal = {IEEE Transactions on Geoscience and Remote Sensing},
  volume  = {62},
  pages   = {1--16},
  year    = {2024},
  doi     = {10.1109/TGRS.2024.3367709}
}
```

- 论文页面：[https://ieeexplore.ieee.org/document/10440355](https://ieeexplore.ieee.org/document/10440355)
