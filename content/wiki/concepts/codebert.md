---
id: concept:codebert
type: concept
title: CodeBERT
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/code-intelligence, papers/program-understanding]
cssclasses: [concept]
---

## 概念内容

CodeBERT 是 BERT 风格的代码-文本双模态预训练模型，用于需要语义匹配或分类的 [[wiki/concepts/code-intelligence]] 理解侧任务（克隆检测、缺陷检测、Cloze、代码搜索等），常接前馈或内积打分头。

关键辨析：在 [[wiki/concepts/codexglue]] 中，CodeBERT 总分 88.4 已接近甚至局部超过部分 AST/GNN 结构模型，但论文指出「若进一步利用代码结构仍有提升空间」——说明大规模预训练已学到语义与部分结构信号，显式 AST/数据流/控制流仍是正交增益来源，合理方向是「预训练 + 结构归纳偏置」。

## 在论文中的用法

- [[wiki/sources/CodeXGLUE_A_Machine_Learning_Benchmark_Dataset_for_Code_Understanding_and_Generation.zh-CN]]：理解侧与序列转换侧（EncDec）编码器的主基线；其表示学习成果与 CodeXGLUE 评测载体同期出现、互为互补。

## 相关概念

- [[wiki/concepts/codexglue]]：CodeBERT 的评测载体
- [[wiki/concepts/codegpt]]：生成侧对应基线
- [[wiki/concepts/code-intelligence]]：所属领域
