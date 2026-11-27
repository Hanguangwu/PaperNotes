---
id: concept:codegpt
type: concept
title: CodeGPT / CodeGPT-adapted
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/code-intelligence, papers/program-generation]
cssclasses: [concept]
---

## 概念内容

CodeGPT 是 GPT 风格在代码语料上继续预训练的生成模型，用于代码补全与 text-to-code。CodeGPT-adapted 在领域数据上进一步适配，通常略优于直接微调的 GPT-2。

关键辨析：在 [[wiki/concepts/codexglue]] 中，适配后的 CodeGPT 总分 72.34，略高于 CodeGPT（71.60）与 GPT-2（70.57）；但生成任务 Exact Match 绝对值仍偏低（text-to-code 约 20%），难以单独支撑「可用代码生成」的强声明。

## 在论文中的用法

- [[wiki/sources/CodeXGLUE_A_Machine_Learning_Benchmark_Dataset_for_Code_Understanding_and_Generation.zh-CN]]：生成侧主基线；复现建议中生成侧优先 CodeGPT-adapted 与 CodeBERT-EncDec。

## 相关概念

- [[wiki/concepts/codexglue]]：CodeGPT 的评测载体
- [[wiki/concepts/codebert]]：理解侧对应基线
- [[wiki/concepts/program-generation]]：CodeGPT 所属任务族
