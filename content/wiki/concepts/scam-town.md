---
id: concept:scam-town
type: concept
title: Scam Town（LLM 诈骗防治社区仿真）
created: 2026-11-25
updated: 2026-11-27
status: seedling
layer: framework
tags: [type/concept, papers/scam-prevention, papers/computational-social-science, papers/llm-agent]
cssclasses: [concept]
---

## 概念内容

Scam Town 是由大语言模型扮演每位居民的模拟社区：外部诈骗呼叫中心按强度 $\lambda(t)$ 拨号，通话之后居民可生成并向认识的人转述警示故事。与经典疾病–意识模型不同，**威胁不在居民间传染，只有保护（警示故事）传播**——结构上接近「弱传播疫苗 / 环状接种」。

关键辨析：口碑 alone 时 $R_{\mathrm{warn}} \le 0.68$（文献参数盒内），达不到自维持阈值（$R_{\mathrm{warn}} \ge 1$），因此社区不能指望警告「病毒式传播」；教育的作用机制是「每条警告带来的后续警告数」，而非病原体式传播。

## 在论文中的用法

- [[wiki/sources/Scam_Town_Can_Scam_Education_Spread_Like_a_Vaccine.zh-CN]]：仿真器本身是该工作的核心贡献，并配预承诺 live 对照（80 个 LLM 城镇）。

## 相关概念

- [[wiki/concepts/vaccine-analogy]]：诈骗教育的「弱传播疫苗」建模
- [[wiki/concepts/llm]]：居民为 LLM 替身，结论应视为待真人检验的假说
- [[wiki/concepts/world-model]]：其「呼叫层 + 故事层」双过程是领域特定的世界模型实例
