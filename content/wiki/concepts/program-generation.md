---
id: concept:program-generation
type: concept
title: 程序生成（Program Generation）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: atomic
tags: [type/concept, papers/program-generation, papers/code-intelligence]
cssclasses: [concept]
---

## 概念内容

程序生成指从给定条件（注释、查询、部分代码）自动生成代码的 [[wiki/concepts/code-intelligence]] 子方向，在 CodeXGLUE 中覆盖：代码补全（PY150、Github Java）、text-to-code（CONCODE）、代码修复（Bugs2Fix）、代码翻译（CodeTrans）。生成类任务普遍引入 CodeBLEU 以兼顾语法与语义。

关键辨析：生成任务的精确匹配率多在 10%–20% 区间，BLEU 与 CodeBLEU 更高；该方向适合做相对排序，但距离「可直接辅助开发者」仍有距离（见 CodeXGLUE 笔记「指标与真实效用」）。

## 在论文中的用法

- [[wiki/sources/CodeXGLUE_A_Machine_Learning_Benchmark_Dataset_for_Code_Understanding_and_Generation.zh-CN]]：四类任务中的「文本–代码」「代码–代码（生成类）」任务族，基线为 [[wiki/concepts/codegpt]] 与 Encoder-Decoder。

## 相关概念

- [[wiki/concepts/code-intelligence]]：所属领域
- [[wiki/concepts/codegpt]]：生成侧主基线
- [[wiki/concepts/vulnerability-detection]]：程序生成的安全应用（漏洞修复）
