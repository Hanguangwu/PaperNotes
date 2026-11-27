---
id: concept:sec-llm-holmes
type: concept
title: SecLLMHolmes
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/security, papers/vulnerability-detection]
cssclasses: [concept]
---

## 概念内容

SecLLMHolmes 是全自动评估框架，对 LLM 能否可靠识别并推理安全相关缺陷进行多维度调查：构建 228 个代码场景（覆盖多种 CWE、不同难度、合成与真实世界代码），从八个调查维度分析八种最先进 LLM，覆盖准确率、推理质量、忠实度、确定性与鲁棒性。

关键辨析：框架首次把「推理忠实度」「非确定性」「对表面扰动的鲁棒性」作为独立指标系统量化，而不仅看最终判断对错；其场景与维度设计可直接用于新模型或新提示策略的回归测试。

## 在论文中的用法

- [[wiki/sources/LLMs_Cannot_Reliably_Identify_and_Reason_About_Security_Vulnerabilities_Yet_A_Comprehensive_Evaluation_Framework_and_Benchmarks]]：框架本身是该工作的核心贡献（「创新点」第 1 条）。

## 相关概念

- [[wiki/concepts/llm]]：被评估对象
- [[wiki/concepts/vulnerability-detection]]：评估的任务
