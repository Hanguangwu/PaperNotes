---
id: concept:code-intelligence
type: concept
title: 代码智能（Code Intelligence）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: atomic
tags: [type/concept, papers/code-intelligence, papers/program-understanding, papers/program-generation]
cssclasses: [concept]
---

## 概念内容

代码智能指用机器学习方法自动完成与程序相关的任务，主要分两个方向：程序理解（克隆检测、缺陷检测、完形填空、代码搜索、代码摘要）与程序生成（代码补全、text-to-code、代码修复、代码翻译、文档翻译）。

关键辨析：领域长期缺乏覆盖广泛任务的统一评测体系，既有工作多为单任务数据集或专用模型，难以横向比较方法、也难以检验预训练表示在多任务上的迁移能力——这正是 [[wiki/concepts/codexglue]] 的动机。

## 在论文中的用法

- [[wiki/sources/CodeXGLUE_A_Machine_Learning_Benchmark_Dataset_for_Code_Understanding_and_Generation.zh-CN]]：需求随开发者规模增长而上升；CodeXGLUE 把分散任务收敛到同一评测体系。
- [[wiki/sources/LLMs_Cannot_Reliably_Identify_and_Reason_About_Security_Vulnerabilities_Yet_A_Comprehensive_Evaluation_Framework_and_Benchmarks]]：LLM 的代码理解/生成能力被用于漏洞修复建议，但可靠性存疑（见「研究问题」）。

## 相关概念

- [[wiki/concepts/codexglue]]：代码智能的统一评测基准
- [[wiki/concepts/codebert]]：代码感知双模态预训练表示
- [[wiki/concepts/program-generation]]：代码智能的子方向
- [[wiki/concepts/vulnerability-detection]]：代码智能的安全应用
