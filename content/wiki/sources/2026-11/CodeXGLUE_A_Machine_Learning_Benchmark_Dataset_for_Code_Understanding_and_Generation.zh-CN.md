---
title: "CodeXGLUE: A Machine Learning Benchmark Dataset for Code Understanding and Generation"
alias: "CodeXGLUE：面向代码理解与生成的机器学习基准数据集"
paper_id: arxiv:2102.04664
arxiv: 2102.04664
authors: [Shuai Lu, Daya Guo, Shuo Ren, Junjie Huang, Alexey Svyatkovskiy, Ambrosio Blanco, Colin Clement, Dawn Drain, Daxin Jiang, Duyu Tang, Ge Li, Lidong Zhou, Linjun Shou, Long Zhou, Michele Tufano, Ming Gong, Ming Zhou, Nan Duan, Neel Sundaresan, Shao Kun Deng, Shengyu Fu, Shujie Liu]
institution: 北京大学、中山大学、北京航空航天大学、Microsoft / Microsoft Research Asia 等
venue: NeurIPS 2021 Datasets and Benchmarks Track
year: 2021
paper_type: benchmark
paper_url: https://datasets-benchmarks-proceedings.neurips.cc/paper_files/paper/2021/file/c16a5320fa475530d9583c34fd356ef5-Paper-round1.pdf
code: https://github.com/microsoft/CodeXGLUE
created: 2026-11-25
updated: 2026-11-27
read_month: "2026-11"
month_status: "done"
tags: [papers/code-intelligence, papers/benchmark, papers/program-understanding, papers/program-generation]
cssclasses: [paper]
---

> [!abstract]+ 一句话总结
>
> CodeXGLUE 是面向代码理解与生成的 GLUE 式多任务基准，配套 14 个数据集、10 项任务、三类可复现基线（CodeBERT / CodeGPT / Encoder-Decoder）与开放评测平台。

## 原文摘要（译）

基准数据集对加速编程语言相关任务的研究具有显著影响。本文提出 CodeXGLUE，一个面向程序理解与生成的机器学习基准，旨在促进相关方法研究。CodeXGLUE 包含覆盖 14 个数据集的 10 项任务，以及用于模型评估与比较的平台。同时提供三类基线系统——BERT 风格、GPT 风格与 Encoder-Decoder 模型——以便研究者直接使用该平台。这些数据与基线有助于开发并验证可迁移到多种程序理解与生成问题的新方法。

## 研究问题

代码智能（code intelligence）需求随开发者规模增长而上升，但领域长期缺乏覆盖广泛任务的统一基准套件。既有工作多为单任务数据集或专用模型，难以横向比较方法、也难以检验预训练表示在多任务上的迁移能力。论文要回答的核心问题是：能否构建一套任务多样、数据可获取、基线可复现的代码智能评测体系，使新方法能在统一规则下被评估与比较。

## 核心贡献

1. **统一多任务代码智能基准**：对标 NLP 的 GLUE 与 CV 的 ImageNet，把原先分散的代码理解/生成任务收敛到同一评测体系。
2. **任务与数据覆盖广**：10 个任务跨越「代码–代码」「文本–代码」「代码–文本」「文本–文本」四类；14 个数据集既包含既有资源，也包含 CT-all、CT-maxmin、AdvTest、WebQueryTest、CodeTrans 等新建或扩展部分。
3. **可复现基线三分法**：理解侧用 CodeBERT，生成侧用 CodeGPT / CodeGPT-adapted，序列转换侧用 Encoder-Decoder（常以 CodeBERT 编码器初始化），降低后续工作的对比成本。
4. **开放平台**：GitHub 公开数据与基线，支持邮件提交结果，形成可累积的排行生态。

## 数据与任务定义

按模态关系划分为四类，共 10 个任务、14 个数据集（论文 Table 1 为总览）。

![](https://cdn.jsdelivr.net/gh/Hanguangwu/MyImageBed01/img/20261127141155286.png)

### Code-Code

- **Clone Detection**：BigCloneBench（Java，900K/416K/416K，F1）、POJ-104（C/C++，32K/8K/12K，MAP）。
- **Defect Detection**：Devign（C，21K/2.7K/2.7K，Accuracy）。
- **Cloze Test**：CT-all†（六语言合计约 176K）、CT-maxmin†（约 2.6K），指标 Accuracy；目标词经 BPE 与规则筛选。
- **代码补全**：PY150（Python，95K/5K/50K）与 Github Java 语料‡；词元级准确率，行级精确匹配与编辑相似度。
- **Code Repair**：Bugs2Fix（Java，98K/12K/12K），BLEU / Accuracy / CodeBLEU。
- **代码翻译**：CodeTrans†（Java 与 C# 互译，约 10K/0.5K/1K）；BLEU、准确率与 CodeBLEU。

### Text-Code

- **自然语言代码搜索**：基于 CodeSearchNet 的 AdvTest† 与 WebQueryTest†（Python）；MRR 等检索指标。
- **Text-to-Code Generation**：CONCODE（Java），Exact Match / BLEU / CodeBLEU。

### Code-Text

- **Code Summarization**：过滤后的 CodeSearchNet（六语言），BLEU。

### Text-Text

- **Documentation Translation**：Microsoft Docs（英↔丹麦/拉脱维亚/挪威/中文），BLEU。

† 新建；‡ 对既有数据引入新划分或新子任务。指标与预处理在正文各小节给出，生成类任务普遍引入 CodeBLEU 以兼顾语法与语义。任务与数据集总览见论文 Table 1。

## 方法主线

### 机制流程

官方不提出单一新架构，而是按任务类型给出三类可复现基线：

1. **CodeBERT（理解）**
   双模态预训练（代码 + 自然语言），用于克隆检测、缺陷检测、Cloze、代码搜索等需要语义匹配或分类的任务；常接前馈或内积打分头。

2. **CodeGPT / CodeGPT-adapted（生成）**
   GPT 风格在代码语料上继续预训练或适配；用于代码补全与 text-to-code。CodeGPT-adapted 在领域数据上进一步适配，通常略优于直接微调的 GPT-2。

3. **Encoder-Decoder（序列转换）**
   编码器常以 CodeBERT 初始化，解码器为 Transformer 解码器；覆盖代码修复、代码翻译、代码摘要等「输入序列 → 输出序列」任务。文档翻译则使用多语言 Transformer / XLM-R 初始化变体。

对比基线还包括循环网络、卷积神经网络、普通 Transformer、RoBERTa、统计机器翻译，以及若干基于抽象语法树或图网络的专用结构模型。

## 关键结果

以下数值均来自论文第 4 节各结果表（表 4 至表 13），取各任务主表结论。

| 任务 | 关键结论（节选） |
|---|---|
| 克隆检测 | CodeBERT 总分 88.4（BigCloneBench 的 F1 为 94.1，POJ-104 的 MAP 为 82.67），优于 RoBERTa 的 84.0；与最强结构模型接近，说明预训练已具竞争力，结构信息仍有空间 |
| 缺陷检测 | CodeBERT 准确率 62.08，高于 RoBERTa（61.05）、TextCNN（60.69）与双向 LSTM（59.37） |
| 完形填空 | 仅用掩码语言模型目标预训练的 CodeBERT 整体优于 RoBERTa；max/min 子集更能区分细微语义 |
| 代码补全 | 适配后的 CodeGPT 总分 72.34，略高于 CodeGPT（71.60）与 GPT-2（70.57），显著高于普通序列模型 |
| 代码修复 | 以 CodeBERT 为编码器的序列到序列模型在 small/medium 集上的准确率与 CodeBLEU 均优于普通 Transformer 与 LSTM；直接复制基线准确率为 0 |
| 代码翻译 | 以 CodeBERT 为编码器的序列到序列模型整体强于仅在代码上继续训练的 RoBERTa 与统计机器翻译基线 |
| 代码搜索 | CodeBERT 在更难的 AdvTest 与 WebQueryTest 上相对强基线有提升（见表 10） |
| 文本到代码 | 适配后的 CodeGPT：精确匹配 20.10、BLEU 32.79、CodeBLEU 35.98，优于 CodeGPT、GPT-2 与经典序列到序列方法 |
| 代码摘要 | 以 CodeBERT 为编码器的序列到序列模型总分 BLEU 17.83，高于 RoBERTa（16.57）、普通 Transformer（15.56）与经典序列到序列（14.32） |
| 文档翻译 | XLM-R 总分 66.16，明显高于普通多语言 Transformer（52.67） |

总体趋势：代码感知预训练在理解与生成两侧都提供了强而可迁移的基线；纯结构模型在部分任务仍略优；生成任务 Exact Match 绝对值仍偏低。

## 深度分析

### 基准定位是否成立

论文把 CodeXGLUE 明确对标 GLUE 与 ImageNet。其价值不在于「再做一个数据集」，而在于：

- **统一接口**：同一仓库、同一评测脚本、同一提交渠道，使「跨任务泛化」成为可检验命题。
- **任务谱系完整**：从词级 cloze、token 补全到函数级摘要、跨语言翻译，覆盖了代码智能的主要形态。
- **与模型工作互补**：CodeBERT 是表示学习成果，CodeXGLUE 是其评测载体；二者同期出现，降低了后续工作「各说各话」的成本。

### 预训练 vs 结构模型

克隆检测结果最有代表性：CodeBERT 已接近甚至局部超过部分 AST/GNN 方法，但论文坦承「若进一步利用代码结构仍有提升空间」。这说明：

- 大规模预训练已经学到相当多语义与部分结构信号；
- 显式 AST、数据流、控制流仍是正交增益来源，而不是被预训练完全替代。

后续工作的合理方向是「预训练 + 结构归纳偏置」，而不是二选一。

### 指标与真实效用

生成任务（文本到代码、代码修复）的精确匹配率多在 10%–20% 区间，BLEU 与 CodeBLEU 更高。该基准适合做相对排序，但距离「可直接辅助开发者」仍有距离。文档翻译依赖通用多语言模型 XLM-R，说明代码基准也可以吸收跨语言 NLP 进展，但并非代码专用预训练的主战场。

### 数据集构造取舍

CT-all 通过子词切分与频次阈值控制目标词，CT-maxmin 聚焦最大值与最小值的语义对立，更难检索集与真实查询集试图提高难度并贴近真实场景。这些设计比直接复用原始划分更利于区分模型，但也带来与原始论文数字不完全可比的问题；引用时需注明是 CodeXGLUE 版本。

## 局限

1. **结构利用不足**：预训练基线未系统融合 AST/控制流；与最强结构模型的差距被指出但未通过新方法消除。
2. **生成指标天花板低**：text-to-code 等任务 EM 仍低，难以单独支撑「可用代码生成」的强声明。
3. **部分数据为重分割/过滤版本**：与原始数据集结果不可直接横比。
4. **真实场景外部效度未验证**：指标均为离线分类/生成/检索指标，缺少开发者研究或在线 A/B。
5. **文档翻译非代码中心**：更多依赖通用多语言预训练，与代码智能主线耦合较弱。

## 我的笔记

- **复现优先级**：若只做理解侧，先跑 CodeBERT 在 clone / defect / search；若做生成侧，优先 CodeGPT-adapted 与 CodeBERT-EncDec。
- **报告规范**：新方法应尽量覆盖 CodeXGLUE 全任务或至少覆盖同一模态下的多个任务，避免只挑一个数据集报点。
- **可做的后续问题**：
  1. 结构感知预训练（AST path、data-flow graph）能否在不损失迁移性的前提下稳定超过 CodeBERT 基线？
  2. 在 CodeXGLUE 上做多任务联合训练，是否比单任务微调更稳？
  3. 用人类评估或仓库级工具链指标校准 BLEU/CodeBLEU。
- **引用时注意**：区分 arXiv 版本与 NeurIPS Datasets & Benchmarks 正式版本；结果表以正式 PDF 为准。

## 引用

```bibtex
@inproceedings{lu2021codexglue,
  title     = {CodeXGLUE: A Machine Learning Benchmark Dataset for Code Understanding and Generation},
  author    = {Lu, Shuai and Guo, Daya and Ren, Shuo and Huang, Junjie and Svyatkovskiy, Alexey and Blanco, Ambrosio and Clement, Colin and Drain, Dawn and Jiang, Daxin and Tang, Duyu and Li, Ge and Zhou, Lidong and Shou, Linjun and Zhou, Long and Tufano, Michele and Gong, Ming and Zhou, Ming and Duan, Nan and Sundaresan, Neel and Deng, Shao Kun and Fu, Shengyu and Liu, Shujie},
  booktitle = {Advances in Neural Information Processing Systems 34 (NeurIPS 2021), Datasets and Benchmarks Track},
  year      = {2021}
}
```

- arXiv: https://arxiv.org/abs/2102.04664
- NeurIPS Datasets & Benchmarks PDF: https://datasets-benchmarks-proceedings.neurips.cc/paper_files/paper/2021/file/c16a5320fa475530d9583c34fd356ef5-Paper-round1.pdf
- GitHub: https://github.com/microsoft/CodeXGLUE
