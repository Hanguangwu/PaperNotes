---
title: "Gemini: Mapping and Architecture Co-exploration for Large-scale DNN Chiplet Accelerators"
alias: "Gemini：面向大规模 DNN Chiplet 加速器的映射与架构协同探索"
paper_id: arxiv:2312.16436
arxiv: 2312.16436
authors: [Jingwei Cai, Zuotong Wu, Sen Peng, Yuchen Wei, Zhanhong Tan, Guiming Shi, Mingyu Gao, Kaisheng Ma]
institution: Tsinghua University（清华大学）、Xi’an Jiaotong University（西安交通大学）、IIISCT、Shanghai AI Laboratory（上海人工智能实验室）
venue: 2024 IEEE International Symposium on High-Performance Computer Architecture（HPCA 2024）
year: 2024
paper_type: method
paper_url: https://arxiv.org/pdf/2312.16436
code: https://github.com/SET-Scheduling-Project/GEMINI-HPCA2024
created: 2026-11-25
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/computer-architecture, papers/chiplet, papers/dnn-accelerator]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> Gemini 通过 Layer-centric 编码与可配置硬件模板，实现了大规模 DNN Chiplet 加速器的架构-映射协同探索，在仅增加 14.3% 货币成本的情况下同时获得近 2 倍性能与 1.4 倍能效提升，并揭示了 Chiplet 场景下映射与粒度选择的关键规律。

## 原文摘要（译）

Chiplet 技术（小芯片技术）能够在后摩尔时代以更高良率将更多晶体管集成到单一加速器上，从而应对 AI 快速发展带来的巨大计算需求。然而，它也引入了更昂贵的封装成本以及成本高昂的 Die-to-Die（D2D，裸片到裸片）接口——这些接口比片上互连占用更多面积、消耗更高功率，且提供更低带宽。最大化 Chiplet 技术的收益并最小化其弊端，对开发大规模 DNN Chiplet 加速器至关重要，这对架构与映射均提出了挑战。尽管在后摩尔时代意义重大，但解决这些挑战的方法仍然稀缺。

为填补这一空白，我们首先提出一种以层为中心（layer-centric）的编码方法，用于编码大规模 DNN 推理加速器的 Layer-Pipeline（LP，层流水线）空间映射，并刻画其优化空间。基于此，我们分析了该空间中未被探索的优化机会，这些机会在 Chiplet 场景下更为关键。基于该编码方法以及高度可配置的通用硬件模板，我们提出了架构与映射协同探索框架 Gemini，以在考虑货币成本（Monetary Cost, MC）、性能与能效的同时，探索大规模 DNN Chiplet 加速器的设计与映射空间。与最先进的（SOTA）Simba 架构搭配 SOTA Tangram LP 映射相比，Gemini 协同优化的架构与映射在多种 DNN 与 batch size 上平均实现 1.98× 的性能提升与 1.41× 的能效提升，同时货币成本仅增加 14.3%。此外，我们利用 Gemini 揭示了在架构设计中利用 Chiplet 技术以及在 Chiplet 场景下映射 DNN 工作负载的有趣洞见。Gemini 框架已开源：[https://github.com/SET-Scheduling-Project/GEMINI-HPCA2024](https://github.com/SET-Scheduling-Project/GEMINI-HPCA2024)。

## 研究问题

后摩尔时代，单芯片面积与光罩尺寸限制使得继续通过工艺进步扩大单体加速器变得困难。Chiplet 技术通过先进封装将多个小 Die（裸片）组合，能够提升良率并继续扩大晶体管规模，但带来两类核心挑战：

1. **架构侧挑战（Chiplet 粒度权衡）**
   使用更多更小的 Chiplet 可提高良率，但会显著增加封装成本与 D2D 接口开销（面积更大、功耗更高、带宽更低）。反之，使用更少更大的 Chiplet 可降低 Chiplet 成本，但良率下降。如何确定最优粒度仍是未解问题。

2. **映射侧挑战（大规模 + 昂贵 D2D）**
   加速器规模增大后，维持高利用率与高能效越来越难。Layer-Pipeline（LP）映射将多层在空间上同时映射到加速器，是学术界与工业界常用手段。其核心是空间映射（Spatial Mapping, SPM）——决定「哪一层的哪一部分被分配到哪个核心」。现有 LP SPM 大多依赖启发式，优化空间未被清晰定义与充分探索。同时，D2D 链路能耗更高、带宽更低，映射必须主动减少跨 Die 通信。

现有工作在架构与映射两侧均缺乏系统性的联合优化方法，无法同时兼顾货币成本、性能与能效。

## 核心贡献

1. **Layer-centric 编码方法**：首次系统性地对大规模加速器中的 Layer-Pipeline 空间映射（SPM）进行可搜索编码，明确了优化空间边界，并揭示了在 Chiplet 场景下此前未被充分探索的关键机会（尤其是跨 Die 通信相关机会）。
2. **高度可配置的通用硬件模板**：支持不同 Chiplet 粒度、核心数量、NoC（片上网络）与 D2D 配置，使架构侧搜索与映射侧搜索能够在同一框架下联合进行。
3. **Gemini 协同探索框架**：将货币成本（MC）、延迟与能耗同时纳入目标，实现架构-映射联合优化，而非传统「先定架构再启发式映射」的串行流程。
4. **量化收益与新洞察**：相对 SOTA Simba + Tangram，在仅增加 14.3% MC 的前提下获得平均 1.98× 性能与 1.41× 能效；并给出 Chiplet 粒度选择原则与减少 D2D 通信的映射策略等可操作洞见。

## 数据与任务定义

- **工作负载**：多种深度神经网络推理模型，包括 ResNet-50、ResNeXt、PNASNet、Inception-ResNet 等。
- **Batch size**：覆盖小 batch（1）与大 batch（64），考察不同流水线压力下的表现。
- **硬件与成本模型**：基于高度可配置模板，建模计算核心、片上存储、NoC、D2D 接口的面积、功耗与带宽；货币成本（MC）综合考虑硅片成本（与面积、良率相关）与封装/D2D 成本。
- **评价指标**：
  - 性能：延迟（Delay）
  - 能效：能耗（Energy）或 EDP（Energy-Delay Product）
  - 货币成本：MC（相对基线的增量）
- **主要基线**：SOTA Chiplet 加速器架构 Simba + SOTA LP 映射方法 Tangram（记为 S-Arch + T-Map）。
- **对比设置**：G-Arch + G-Map（Gemini 协同结果） vs 单独架构优化 vs 单独映射优化 vs 基线。

## 方法主线

![](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261127141026909.png)

### 机制流程

1. **输入**：DNN 描述文件（层拓扑与计算特征）+ 硬件模板配置空间（Chiplet 数量、每 Chiplet 核心数、存储层次、NoC/D2D 参数）+ 目标权重（MC / 延迟 / 能耗）。
2. **操作 A（模型解析与层组划分）**：模型解析器生成拓扑图并提取每层特征；层组生成算法将网络切分为适合流水线的层组，并确定每个流水线阶段处理的样本数（batch unit）。
3. **操作 B（Layer-centric 编码与空间映射搜索）**：用层中心编码将「层组内各层到核心的空间分配」表示为可搜索结构；在编码空间上搜索以最小化跨 Die 通信、平衡负载并匹配硬件资源。
4. **操作 C（架构-映射联合评估与搜索）**：对候选架构配置与对应映射方案，用统一成本模型评估延迟、能耗与 MC；通过协同搜索找到 Pareto 更优的（架构，映射）组合。
5. **输出**：推荐的 G-Arch（架构）与 G-Map（映射）方案，以及相对基线的量化收益与设计洞察。

### 关键组件说明

- **Layer-centric 编码**：将 LP 空间映射从「启发式规则」提升为「可枚举/可搜索的表示」，使优化空间边界清晰，便于发现 Chiplet 场景下被忽视的通信局部性机会。
- **硬件模板**：参数化 Chiplet 粒度、核心组织、存储与互连，支持快速生成不同成本-性能-能效权衡点的候选架构。
- **协同目标**：MC 不再是事后统计，而是与性能、能效一起进入搜索目标，避免「性能很好但封装成本不可接受」的方案。

## 关键结果

主结果（相对 S-Arch + T-Map 基线，跨多种 DNN 与 batch size 平均）：

- **性能**：1.98× 提升（延迟显著下降）
- **能效**：1.41× 提升
- **货币成本**：仅增加 14.3%

更细粒度对比（G-Arch + G-Map vs S-Arch + T-Map）：

- 延迟降低约 46.8%
- 能耗降低约 28.8%

消融与分析表明：

- 仅优化架构或仅优化映射均无法同时达到上述性能与能效水平，协同探索带来额外收益。
- EDP 随核心数增加呈现先降后升的非单调曲线，存在最优区间，并非「核心越多越好」。
- 通过映射主动减少跨 Die 通信量，对能效与延迟均有明显贡献，印证了 D2D 约束下映射策略的重要性。

论文同时给出了不同 Chiplet 粒度下的成本-收益权衡曲线，以及若干可复用的设计经验（见深度分析）。

## 深度分析

### 为什么协同探索有效

在单体或片上 NoC 场景中，通信代价相对可控，启发式映射往往足够。Chiplet 引入后，D2D 成为一等公民约束：带宽更低、能耗更高、面积开销更大。此时：

- 架构选择（粒度、Die 间带宽供给）直接决定映射的可行空间与通信代价上限；
- 映射选择（层到核心的空间布局）直接决定实际发生的 D2D 流量。

两者强耦合。先固定架构再跑启发式映射，或先固定映射再调架构，都容易陷入局部最优。Gemini 将两者放入同一搜索环路，并用 MC 把「封装与 D2D 的真实代价」显式暴露给优化器，因此能找到更接近全局 Pareto 前沿的点。

### Chiplet 粒度与 EDP 的非单调性

增加核心数/Die 数通常降低计算延迟，但会推高 D2D 与封装相关能耗与 MC。EDP 先降后升说明存在「甜蜜点」：过少核心算力不足，过多核心则通信与成本惩罚超过收益。这为「不是越大越好」提供了定量证据。

### 映射对 D2D 的敏感度

并非所有层间依赖数据量相同。Layer-centric 编码使搜索器能够优先把高数据量依赖放在同一 Die 内，或放在 D2D 带宽更充裕的路径上，从而在不增加太多计算资源的情况下显著降低通信瓶颈。这对未来百 Die 级系统尤其重要。

### 复现与扩展注意点

- 结果强依赖硬件成本模型参数（D2D 能耗/带宽、良率曲线、封装成本）。换工艺或封装技术时需重新标定。
- 当前聚焦推理与 LP 范式，训练、动态 shape、稀疏/混合精度会改变最优粒度与映射形态。
- 开源代码已公开，可直接替换硬件模板或工作负载进行二次探索。

## 局限

1. **建模假设**：面积、功耗、D2D 特性与 MC 均为模型估算，未经实际硅片测量验证；真实流片数字可能存在偏差。
2. **覆盖范围**：主要针对推理加速器与 Layer-Pipeline 空间映射，未系统覆盖训练、非 LP 映射、异构 Chiplet 或动态工作负载。
3. **成本模型简化**：货币成本模型未包含全部制造、测试、良率学习曲线与供应链因素。
4. **规模与网络多样性**：实验覆盖的 DNN 与 batch size 有限，向更大模型（LLM 级）或更新网络结构泛化时需额外验证。
5. **搜索可扩展性**：当 Die 数与层数进一步增大时，编码空间与搜索代价的增长特性尚未充分讨论。

## 我的笔记

**可立即复用的洞见**

- 做 Chiplet 加速器时，必须把 D2D 成本与映射策略放在同一个优化闭环里，而不是「架构定完再想办法映射」。
- 货币成本（MC）应与性能、能效并列成为架构探索的一等目标，否则容易得到「学术好看、工业不可落地」的设计。
- Layer-centric 编码提供了一种清晰的 LP 映射表示，后续研究可直接复用或扩展该表示，避免继续依赖黑盒启发式。
- EDP 随资源增加的非单调性提醒我们：在 Chiplet 时代，「堆核心」策略需要重新被成本模型约束。

**后续值得做的方向**

- 将框架扩展到训练场景、混合精度与稀疏性，观察最优 Chiplet 粒度如何变化。
- 用真实 D2D 测试芯片或先进封装数据校准模型，缩小「模型 vs 硅片」差距。
- 探索百 Die 级或异构 Chiplet 下的编码压缩与分层搜索，保证可扩展性。
- 与编译栈（TVM / MLIR / 专用调度器）打通，形成从模型到 Chiplet 映射的端到端自动流程。

**一句话个人评价**

这是后摩尔时代 Chiplet 加速器领域少有的「把架构与映射真正放在同一搜索框架、并把货币成本说清楚」的工作，方法完整、结果扎实、开源可用，值得作为后续 Chiplet 系统设计的参考基线。

## 引用

```bibtex
@inproceedings{cai2024gemini,
  title     = {Gemini: Mapping and Architecture Co-exploration for Large-scale DNN Chiplet Accelerators},
  author    = {Cai, Jingwei and Wu, Zuotong and Peng, Sen and Wei, Yuchen and Tan, Zhanhong and Shi, Guiming and Gao, Mingyu and Ma, Kaisheng},
  booktitle = {2024 IEEE International Symposium on High-Performance Computer Architecture (HPCA)},
  year      = {2024}
}
```

- arXiv: https://arxiv.org/abs/2312.16436
- PDF: https://arxiv.org/pdf/2312.16436
- Code: https://github.com/SET-Scheduling-Project/GEMINI-HPCA2024
