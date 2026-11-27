---
id: concept:llm
type: concept
title: 大语言模型（LLM）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: atomic
tags: [type/concept, papers/llm, papers/security]
cssclasses: [concept]
---

## 概念内容

大语言模型（Large Language Models, LLMs）在代码生成、文档与调试上已展现潜力，也被提议用于自动化漏洞修复；SecLLMHolmes 评估的八种模型包括 GPT-4、PaLM2、GPT-3.5、CodeLlama 等。

关键辨析：LLM 对代码的理解在很大程度上仍依赖表面统计模式（标识符名称、常见库调用模式）——最小改动（改名、加库函数）即可使判断翻转（PaLM2 约 26%、GPT-4 约 17% 案例出错）。模型输出的「思考过程」可能不忠实，不能直接当作可审计证据。

## 在论文中的用法

- [[wiki/sources/LLMs_Cannot_Reliably_Identify_and_Reason_About_Security_Vulnerabilities_Yet_A_Comprehensive_Evaluation_Framework_and_Benchmarks]]：即使最先进 LLM 在安全漏洞识别与推理上仍不鲁棒、推理不忠实，尚不能作为可靠通用安全助手；可行路径是 LLM 辅助层 + 静态分析/人工审核流水线。

## 相关概念

- [[wiki/concepts/vulnerability-detection]]：LLM 的安全应用
- [[wiki/concepts/code-intelligence]]：LLM 的代码能力基础
- [[wiki/concepts/program-generation]]：LLM 生成代码的质量与安全性问题
