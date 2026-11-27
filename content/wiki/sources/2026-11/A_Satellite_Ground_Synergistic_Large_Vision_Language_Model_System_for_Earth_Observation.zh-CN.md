---
title: "A Satellite-Ground Synergistic Large Vision-Language Model System for Earth Observation"
alias: "面向地球观测的星地协同大视觉语言模型系统"
paper_id: arxiv:2507.05731
arxiv: 2507.05731
authors: [Yuxin Zhang, Jiahao Yang, Zhe Chen, Wenjun Zhu, Jin Zhao, Yue Gao]
institution: 复旦大学空间互联网研究院；复旦大学计算机科学与人工智能学院
venue: Proceedings of the 33rd ACM International Conference on Multimedia（ACM MM’25，Dublin）
year: 2025
paper_type: method
paper_url: https://arxiv.org/pdf/2507.05731
created: 2026-11-26
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/satellite, papers/remote-sensing, papers/llm, papers/vision-language-model, papers/edge-inference]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> SpaceVerse 通过星上紧凑 LVLM + 地面大模型的协同架构，结合渐进置信决策与注意力多尺度降冗余，在真实 LEO 网络条件下实现地球观测任务准确率与时延的显著帕累托改进（相对 SOTA 基线平均准确率 +31.2%、时延 −51.2%）。

## 原文摘要（译）

近年来，大视觉语言模型（LVLM）在数据中心对低地球轨道（LEO）卫星地球观测图像展现出强大分析能力。然而，卫星快速运动、星地站（GS）接触窗口短暂以及图像体量大，带来严重的数据下载挑战。为支持近实时地球观测应用（如灾害与极端天气监测），需要探索如何在 LEO 卫星网络中部署 LVLM。本文设计了 SpaceVerse——一种高效的星地协同 LVLM 推理系统。首先，在卫星上部署紧凑型 LVLM 处理轻量任务，而在地面站部署常规规模 LVLM 处理计算密集型任务；随后提出计算与通信协同设计框架，包含渐进置信网络与基于注意力的多尺度预处理，分别用于识别可在星上完成推理的数据，以及在星地传输前降低数据冗余。作者在真实 LEO 星座与数据集上实现并评估 SpaceVerse，相对最先进基线平均准确率提升 31.2%，时延降低 51.2%。

## 研究问题

1. 在卫星严格算力、短时星地窗口与高数据冗余约束下，如何高效部署 LVLM 以支持近实时地球观测？
2. 如何在星上与地面之间智能分配推理任务，既避免简单样本的不必要传输，又保证复杂样本的准确率？
3. 如何在传输前有效降低遥感图像冗余，同时尽量不损害下游视觉-语言任务性能？

## 核心贡献

1. **首个面向地球观测的星地协同 LVLM 推理系统**：据作者称，SpaceVerse 是首个将星上推理与地面辅助相结合的地球观测 LVLM 系统。
2. **渐进置信网络**：利用星上模型逐步生成的图像特征与 token，多阶段估计“是否需要地面协助”，支持早退与资源节约。
3. **文本–图像注意力 + 自适应多尺度预处理**：在传输前识别感兴趣区域并压缩冗余，针对遥感图像“大图中仅小区域相关”的特性。
4. **真实星座端到端评估**：在真实 Starlink 几何、实测带宽（约 110.67 Mbps）与标准遥感任务（RSVQA、场景分类、目标检测）上进行端到端系统评估，并与 Tabi、AI-RG 等边缘–云协同基线对比。

## 数据与任务定义

**评估任务与数据集**

- RSVQA：遥感视觉问答
- RESISC：遥感场景分类
- DOTA：遥感目标检测（报告 IoU）

**系统设置**

- 星上模型：Qwen2-VL-2B（紧凑）
- 地面模型：Qwen2-VL-7B（常规）
- 文本–图像嵌入：CLIP-ViT-Patch/16
- 星座：从 570 km Starlink 星座选取 10 颗卫星，测试数据均匀分配
- 星地带宽：实测约 110.67 Mbps
- 渐进网络超参：$I=2$，$\tau_1=0.5$，$\tau_2=0.4$；多尺度粒度 $N_r^k=100$

**基线**

- Satellite-only / GS-only
- Tabi（单置信度 offload + 词剪枝/加权集成）
- AI-RG（主动推理、无奖励引导的 offload 策略）

**主要指标**：每样本平均推理时延、任务准确率（或 IoU）

## 方法主线

### 机制流程

SpaceVerse 由两个相继组件构成（计算–通信协同设计）：

1. **任务分配：渐进置信网络**（§3.1）
   - 星上紧凑模型 $W_s$ 与地面大模型 $W_g$（$|W_g| > |W_s|$）。
   - 置信网络 $\tilde{g}$ 采用多层感知机 + 多个线性投影，按阶段接收逐步生成的图像特征 $V(x_k)$ 与 token 集合 $A_i$。
   - 第 $i$ 阶段若置信度低于阈值 $\tau_i$，则早退并将样本送往地面；否则继续星上生成更多 token 并再估计。
   - 最终阶段若仍不足置信，则交由 $W_g$ 完成；否则输出星上结果。
   - 训练采用监督 MSE，目标是估计与地面输出的相似度。
2. **降冗余：注意力多尺度预处理**（§3.2）
   - 利用文本提示与图像的交叉注意力定位感兴趣区域。
   - 结合自适应多尺度处理，在保留任务相关细节的同时显著减少传输数据量。

![Figure 1 SpaceVerse 框架总览](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261126232509291.png)

![](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261126232612871.png)

## 关键结果

**总体性能（Figure 9）**

- Satellite-only：准确率最低（受 2B 模型限制）。
- GS-only：准确率高，但因原始遥感数据传输导致时延最高（DOTA 上可达星上推理的 4.14×）。
- Tabi：相对星上模型平均性能提升约 36.2%，但每个 offload 样本需先完成完整星上推理，额外时延开销约 69.9%。
- AI-RG：星上时延开销较低（约 58.7%），但仍不及 SpaceVerse。
- **SpaceVerse**：相对 SOTA 基线，**平均准确率提升 31.2%，平均时延降低 51.2%**，在准确率–时延平面上明显更优。

**消融（Figure 11 等）**

- 渐进置信网络不同变体（完整 $\tilde{g}$ vs 简化版本）对 RSVQA 与 DOTA 的准确率–时延权衡有显著影响；完整设计能在保持更高准确率的同时控制时延。
- 多尺度预处理与注意力机制对降低 offload 体积、维持检测/分类性能至关重要；天真缩放或裁剪会导致目标检测 IoU 明显下降。

## 深度分析

### 动机实验的启示

- 7B 模型显著优于 2B，但星上无法部署；纯地面推理因接触窗口与带宽限制，总时延不可接受。
- 遥感图像存在大量与当前任务无关的区域；盲目传输或粗暴压缩都会损害性能（尤其目标检测）。
- 因此需要“智能决策是否/何时/传什么”的协同机制，而非简单 offload 或仅星上推理。

### 渐进置信的设计价值

传统单次置信度（如 Tabi）要求先完整跑完星上模型再决定是否 offload，浪费算力与时间。渐进设计允许在早期阶段就识别“注定需要地面”的样本并提前终止，同时对边界样本利用更多 token 提高估计可靠性，从而实现更好的资源–准确率权衡。

### 工程现实意义

- 实测 Starlink 带宽与几何使结果更具可迁移性。
- 系统面向灾害监测、极端天气等对时延敏感的应用，31%+ 准确率与 50%+ 时延改进具有实际价值。
- 计算–通信协同设计思路可推广到其他资源异构、链路间歇的边缘–云场景。

## 局限

1. 评估以单星–单站简化模型为基础再扩展到 10 星，大规模星座调度、多地面站选择与星间链路（ISL）的影响讨论有限。
2. 骨干模型固定为 Qwen2-VL 系列，其他 LVLM 家族的迁移性能未充分验证。
3. 任务覆盖 RSVQA、场景分类、目标检测，未穷尽所有地球观测任务（如变化检测、密集描述等）。
4. 超参数（阈值、多尺度粒度）在固定设置下报告，在线自适应能力有待进一步研究。

## 我的笔记

- **问题定义清晰**：把“星上算力不够 + 链路窗口短 + 数据冗余大”三个真实约束写透，并用动机实验量化，说服力强。
- **渐进置信是亮点**：比单点置信度 offload 更符合自回归 LVLM 的生成特性，值得在其他协同推理场景借鉴。
- **系统完整度高**：从动机 → 架构 → 算法细节 → 真实星座实现 → 多数据集对比与消融，形成闭环。
- **可继续探索的方向**：
  1. 多星多站联合优化与 ISL 辅助；
  2. 置信网络对带宽/负载变化的在线自适应；
  3. 更轻量或量化后的星上 LVLM 与更强地面模型的组合；
  4. 将同一框架扩展到时序/多模态地球观测任务。
- 整体是一篇扎实的 ACM MM 系统方法论文，对卫星边缘智能与 LVLM 协同部署均有参考价值。

## 引用

```bibtex
@inproceedings{zhang2025spaceverse,
  title     = {A Satellite-Ground Synergistic Large Vision-Language Model System for Earth Observation},
  author    = {Zhang, Yuxin and Yang, Jiahao and Chen, Zhe and Zhu, Wenjun and Zhao, Jin and Gao, Yue},
  booktitle = {Proceedings of the 33rd ACM International Conference on Multimedia},
  year      = {2025},
  doi       = {10.1145/3570361.3613290},
  note      = {arXiv:2507.05731}
}
```

- 论文页面：[https://arxiv.org/abs/2507.05731](https://arxiv.org/abs/2507.05731)
- PDF：[https://arxiv.org/pdf/2507.05731](https://arxiv.org/pdf/2507.05731)
