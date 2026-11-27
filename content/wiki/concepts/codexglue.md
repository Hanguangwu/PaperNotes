---
id: concept:codexglue
type: concept
title: CodeXGLUE
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/code-intelligence, papers/benchmark]
cssclasses: [concept]
---

## 概念内容

CodeXGLUE 是面向程序理解与生成的 GLUE 式多任务基准，对标 NLP 的 GLUE 与 CV 的 ImageNet。它包含覆盖 14 个数据集的 10 项任务，并配套三类可复现基线：理解侧 CodeBERT、生成侧 CodeGPT / CodeGPT-adapted、序列转换侧 Encoder-Decoder（常以 CodeBERT 编码器初始化）。

关键辨析：其价值不在「再做一个数据集」，而在统一接口（同一仓库、同一评测脚本、同一提交渠道），使「跨任务泛化」成为可检验命题。部分数据集为新建或对既有数据引入新划分（CT-all、CT-maxmin、AdvTest、WebQueryTest、CodeTrans），与原始论文数字不完全可比，引用时需注明是 CodeXGLUE 版本。

## 在论文中的用法

- [[wiki/sources/CodeXGLUE_A_Machine_Learning_Benchmark_Dataset_for_Code_Understanding_and_Generation.zh-CN]]：该基准的提出与全部评测结果（论文第 4 节表 4—表 13）。

## 相关概念

- [[wiki/concepts/code-intelligence]]：CodeXGLUE 所属领域
- [[wiki/concepts/codebert]]：理解侧基线模型
- [[wiki/concepts/codegpt]]：生成侧基线模型
- [[wiki/concepts/program-generation]]：基准覆盖的任务族之一
