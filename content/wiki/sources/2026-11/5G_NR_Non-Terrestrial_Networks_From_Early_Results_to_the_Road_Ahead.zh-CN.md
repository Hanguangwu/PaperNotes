---
title: "5G NR Non-Terrestrial Networks: From Early Results to the Road Ahead"
alias: "5G NR 非地面网络：从早期结果到未来路线图"
paper_id: arxiv:2601.04882
arxiv: 2601.04882
authors: [Mattia Figaro, Francesco Rossato, Marco Giordani, Alessandro Traspadini, Takayuki Shimizu, Chinmay Mahabal, Sanjeewa Herath, Chunghan Lee, Dogan Kutay Pekcan, Michele Zorzi]
institution: University of Padova（意大利帕多瓦大学信息工程系）；Toyota Motor North America Inc. R&D InfoTech Labs（美国）
venue: npj Wireless Technology（2026）
year: 2026
paper_type: method
paper_url: https://arxiv.org/pdf/2601.04882
code: https://gitlab.com/mattiasandri/ns-3-ntn
created: 2026-11-26
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/communications, papers/satellite, papers/6g, papers/non-terrestrial-networks]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> 本文在梳理 3GPP 5G NR-NTN 从 Rel. 17 到 Rel. 20 标准演进的同时，用对齐校准参数的 ns-3 全栈仿真量化了不同轨道与频段配置的端到端性能，指出 LEO 在 S 频段、GEO 在 Ka 频段各具优势，并为 6G 时代的 NTN 研究提供可复现基线。

## 原文摘要（译）

本文概述了 3GPP 5G NR-NTN 标准，详细介绍了从 Rel. 18 到 Rel. 19 的演进以及 Rel. 20 的创新方向。作者使用经过 3GPP 校准数据验证的真实 ns-3 仿真，评估了多种卫星网络配置。结果表明，非地面网络（NTN）在扩展偏远地区无线连接、支持紧急场景请求以及缓解地面网络拥塞方面具有潜力。

## 研究问题

1. 3GPP 5G NR-NTN 从 Rel. 17 到 Rel. 20 的关键技术演进与架构选择是什么？
2. 在严格对齐 3GPP 校准场景与协议规格的条件下，LEO/GEO 卫星在 S/Ka 频段的端到端吞吐量、PDR 与时延表现如何？
3. 现有文献中的 NTN 性能评估与 3GPP 规范存在哪些错位？如何用全栈仿真弥补？

## 核心贡献

1. **标准全景更新至 Rel. 20**：提供截至 Rel. 20 的 3GPP 5G NR-NTN 标准化最新全景，弥补早期综述（如 2021 年工作）对近期 Release 覆盖不足的问题。
2. **3GPP 校准对齐的全栈 ns-3 仿真**：采用与 3GPP 校准规格严格对齐的全栈 ns-3 仿真，而非仅链路层或概念性评估，填补“协议栈对齐不足”的研究空白。
3. **轨道–频段交叉性能规律**：系统对比 LEO（600 km）与 GEO（35 786 km）在 S 频段与 Ka 频段下的端到端吞吐量、分组投递率（PDR）与时延，揭示轨道–频段交叉性能规律。
4. **公开可复现代码**：公开 ns3-NTN 代码仓库（`ns-3-ntn`），支持后续复现与扩展。

## 数据与任务定义

**仿真场景（对齐 3GPP 校准）**

- 架构：再生载荷（regenerative payload），卫星充当 gNB。
- 用户：单个地面 UE，固定位置，Rural LOS 条件。
- 业务：远程主机以恒定源速率 $R$ 生成 UDP 分组，UE 下载数据。
- 评估指标：端到端吞吐量、PDR、应用层时延（从远程主机生成到 UE 成功接收）。
- 轨道与频段组合（四组）：
  - LEO $h = 600$ km + S 频段
  - LEO $h = 600$ km + Ka 频段
  - GEO $h = 35\,786$ km + S 频段
  - GEO $h = 35\,786$ km + Ka 频段
- 路径损耗与 SNR 计算严格使用 3GPP 给出的 FSPL 公式、EIRP、G/T、大气/闪烁等附加损耗参数。

**任务本质**：在 3GPP 定义的校准体制下，量化不同卫星配置对端到端性能的影响，而非提出新算法。

## 方法主线

### 机制流程

1. **标准梳理**：系统整理 RAN 相关 TS/TR（TR 38.811、TR 38.821、TS 38.300 等），覆盖透明/再生载荷、频段扩展（含 L/S、Ka）、移动性、GNSS 韧性等从 Rel. 17 到 Rel. 20 的演进。
2. **架构建模**：
   - **透明载荷（bentpipe）**：卫星仅做滤波/放大/转发，gNB 在地面关口站，Uu 接口终止于地面。
   - **再生载荷（decode-and-forward）**：卫星具备解调/解码/路由能力，充当空中 gNB，Uu 在服务链路，支持 ISL。本文仿真采用再生载荷。
3. **信道与天线**：基于 3GPP NTN 信道模型（Dense Urban / Urban / Suburban / Rural），含大气吸收与闪烁；卫星圆形孔径天线，UE 采用 UPA 或 VSAT。
4. **仿真平台**：ns-3 + 自定义 NTN 扩展，全协议栈，参数对齐 3GPP 校准表。
5. **评估循环**：固定几何 → 计算 FSPL/SNR → 在不同 $R$ 下运行 → 统计吞吐、PDR、时延。

![Fig. 1 3GPP NTN 架构：透明（上）与再生（下）](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261126233014182.png)

## 关键结果

仿真在 Rural LOS、再生载荷、UDP 恒定速率下进行，主要观察：

- **吞吐量**：
  - Ka 频段整体显著高于 S 频段（带宽与可用资源优势）。
  - 在 Ka 频段，GEO 端到端吞吐量可比 LEO 高约 50%（文中归因于 3GPP 校准对 LEO 平台硬件约束的建模）。
  - 源速率 $R$ 升高后，系统进入拥塞，吞吐量饱和。
- **PDR**：低速率（$R < 500$ Mbps 量级）时接近理想；高速率后因拥塞下降。
- **时延**：
  - GEO 固有传播时延约 120 ms，LEO 约 2 ms 量级。
  - S 频段在 $R > 250$ Mbps 时，时延迅速攀升并超过 500 ms，网络进入严重拥塞。
  - Ka 频段在更高速率下仍能维持相对可接受的时延。

**核心交叉结论**：在 3GPP 校准参数下，**LEO 在 S 频段表现更好，GEO 在 Ka 频段吞吐量更高**。这并非“轨道本身必然优劣”，而是当前规格对平台能力与信道的建模结果。

## 深度分析

### 为什么 GEO 在 Ka 频段反而吞吐量更高？

论文明确指出，这源于仿真参数严格遵循 3GPP 校准场景：LEO 被建模为硬件资源受限平台，而 GEO 在给定 EIRP、天线与带宽配置下获得更高有效容量。该结果提醒研究者：不能脱离具体校准假设直接宣称“LEO 必然优于 GEO”。

### 与既有工作的定位

- Hosseinian 等（2021）较早综述 5G NR-NTN，但未覆盖 Rel. 18–20 最新进展。
- Lin 的路线图覆盖面广，但 NTN 部分篇幅有限。
- Tong 等聚焦 Rel. 17 起 NTN，但仍偏高层描述，缺乏定量全栈评估。
- 多数链路层或概念性研究未完整建模 MAC/RRC/协议栈与 3GPP 校准信道。

本文的价值在于：**标准时间线更新 + 校准对齐的全栈仿真** 的组合，为后续研究提供可对照基线。

### 工程含义

- S 频段传播条件更优、对天气更稳健，适合对可靠性敏感、速率要求不极致的场景；但高速率下易拥塞。
- Ka 频段容量潜力大，更适合高吞吐需求，但需应对更高路径损耗与天气敏感度。
- 再生载荷便于实现更丰富的空侧处理与 ISL，是当前仿真与未来演进的重点方向。

## 局限

1. **场景简化**：静态单 UE、固定位置、UDP 恒定速率，与 3GPP 当前校准规格一致，但未覆盖多 UE 调度、TCP 变体、移动性与切换。
2. **几何静态**：未建模时间变化的 LEO 星座几何与频繁切换。
3. **指标范围**：主要关注吞吐、PDR、时延，未深入多普勒补偿、GNSS 韧性、功耗等。
4. **架构选择**：仅评估再生载荷，未对透明载荷做同等深度的端到端对比。

因此，结果应理解为“在 3GPP 校准基线下的相对趋势”，而非通用最优架构结论。

## 我的笔记

- **可复现性是亮点**：代码已公开（ns3-NTN），参数表对齐 3GPP，后续工作可直接在此基线上加多用户、切换、TCP、ISL。
- **“校准即现实”的提醒**：许多“LEO 完胜 GEO”的直觉在严格校准参数下并不成立，尤其是 Ka 频段。做系统设计时必须同时看轨道、频段、平台能力假设。
- **标准与仿真的闭环**：本文展示了“读懂 Release 文档 → 把校准表写进仿真器 → 用端到端指标说话”的完整研究范式，对 NTN/6G 方向的学生与工程师很有参考价值。
- **后续最值得做的扩展**：
  1. 多 UE + 调度算法对 Ka/LEO 优势的影响；
  2. TCP 与拥塞控制在高时延链路下的行为；
  3. LEO 切换与波束管理对 PDR 的实际代价；
  4. Rel. 20 拟议功能（如更强的 GNSS 韧性、更高频段）对上述指标的边际收益。

整体评价：这是一篇“标准地图 + 可复现基线”型工作，适合作为 5G/6G NTN 研究的入门与对照锚点，而非追求极致新算法的论文。

## 引用

```bibtex
@article{figaro2026nrntn,
  title   = {5G NR Non-Terrestrial Networks: From Early Results to the Road Ahead},
  author  = {Figaro, Mattia and Rossato, Francesco and Giordani, Marco and Traspadini, Alessandro and Shimizu, Takayuki and Mahabal, Chinmay and Herath, Sanjeewa and Lee, Chunghan and Pekcan, Dogan Kutay and Zorzi, Michele},
  journal = {npj Wireless Technology},
  volume  = {2},
  number  = {1},
  pages   = {31},
  year    = {2026},
  doi     = {10.1038/s44459-026-00029-y},
  note    = {arXiv:2601.04882}
}
```

- 论文页面：[https://arxiv.org/abs/2601.04882](https://arxiv.org/abs/2601.04882)
- PDF：[https://arxiv.org/pdf/2601.04882](https://arxiv.org/pdf/2601.04882)
- 代码：[https://gitlab.com/mattiasandri/ns-3-ntn](https://gitlab.com/mattiasandri/ns-3-ntn)
