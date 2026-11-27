---
title: Paper Notes
cssclasses: [index]
---

# Paper Notes

基于 [Quartz 5](https://quartz.jzhao.xyz) 构建的个人**论文阅读笔记**站点。每篇英文论文对应一条中文笔记：
结构化 frontmatter（作者、机构、发表渠道、arXiv、代码）+ 固定章节（一句话总结、原文摘要、研究问题、核心贡献、
关键结果、深度分析、局限、个人笔记、BibTeX 引用），并通过 Bases 视图自动汇总。

## 论文列表

```base
filters:
  and:
    - 'file.hasTag("papers")'
    - 'file.hasProperty("paper_id")'
properties:
  title:
    displayName: 论文
  alias:
    displayName: 中文标题
  authors:
    displayName: 作者
  institution:
    displayName: 机构
  venue:
    displayName: 发表渠道
  year:
    displayName: 年份
  arxiv:
    displayName: arXiv
  paper_url:
    displayName: 链接
  code:
    displayName: 代码
  paper_type:
    displayName: 类型
  tags:
    displayName: 标签
  read_month:
    displayName: 阅读月份
  month_status:
    displayName: 本月状态
views:
  - type: table
    name: 全部论文
    groupBy:
      property: read_month
      direction: DESC
    order:
      - title
      - year
      - paper_type
      - venue
      - arxiv
      - code
      - tags
    sort:
      - property: year
        direction: DESC
      - property: file.name
        direction: ASC
  - type: table
    name: "本月在读"
    filters:
      and:
        - 'file.hasProperty("read_month")'
        - 'note.read_month == today().date().format("YYYY-MM")'
        - 'note.month_status == "reading"'
    order:
      - file.name
```

## 概念

从六篇笔记中摘取的核心概念，每页含定义、关键辨析、在论文中的用法与关联概念（双链）：

- 体系结构（Gemini 笔记）：[[wiki/concepts/gemini]]（framework）、[[wiki/concepts/chiplet]]（mechanism）、[[wiki/concepts/die-to-die]]（mechanism）、[[wiki/concepts/layer-pipeline-mapping]]（mechanism）、[[wiki/concepts/monetary-cost]]（mechanism）
- 代码智能（CodeXGLUE 笔记）：[[wiki/concepts/codexglue]]（framework）、[[wiki/concepts/code-intelligence]]（atomic）、[[wiki/concepts/codebert]]（framework）、[[wiki/concepts/codegpt]]（framework）、[[wiki/concepts/program-generation]]（atomic）
- LLM 安全（LLMs 笔记）：[[wiki/concepts/llm]]（atomic）、[[wiki/concepts/vulnerability-detection]]（atomic）、[[wiki/concepts/sec-llm-holmes]]（framework）
- 递归自我改进（RSI-Anything 笔记）：[[wiki/concepts/recursive-self-improvement]]（mechanism）、[[wiki/concepts/world-model]]（atomic）、[[wiki/concepts/jepa]]（mechanism）
- 自主科研协作（AgentRxiv 笔记）：[[wiki/concepts/agent-lab]]（framework）、[[wiki/concepts/agent-rxiv]]（framework）
- 诈骗防治仿真（Scam Town 笔记）：[[wiki/concepts/scam-town]]（framework）、[[wiki/concepts/vaccine-analogy]]（mechanism）

## 阅读方法

每条笔记包含两部分：**结构化速览**（frontmatter + 一句话总结 + 关键结果表）与**批判性分析**（深度分析、局限、
个人笔记）。建议先读 frontmatter 与一句话总结定位价值，再按需深入对应章节。

- 新增笔记：复制 [`templates/source-template.md`](wiki/templates/source-template.md) 到 `wiki/sources/`，
  填写 frontmatter 与固定章节；`papers.base` 列表会自动更新，无需手动维护。
- 维护规范：见 [AGENTS.md](../AGENTS.md)；论文数量增长后的仓库维护策略见 [MAINTENANCE.md](../MAINTENANCE.md)。

## 标签导航

- [#papers/AI](tags/papers/AI) — AI / LLM 智能体（AgentRxiv、RSI-Anything、Scam Town）
- [#papers/llm-agent](tags/papers/llm-agent) — LLM 智能体
- [#papers/autonomous-research](tags/papers/autonomous-research) — 自主科研
- [#papers/multi-agent](tags/papers/multi-agent) — 多智能体
- [#papers/recursive-self-improvement](tags/papers/recursive-self-improvement) — 递归自我改进
- [#papers/world-model](tags/papers/world-model) — 世界模型
- [#papers/computational-social-science](tags/papers/computational-social-science) — 计算社会科学
- [#papers/scam-prevention](tags/papers/scam-prevention) — 诈骗防治
- [#papers/code-intelligence](tags/papers/code-intelligence) — 代码理解 / 生成 / 基准
- [#papers/benchmark](tags/papers/benchmark) — 基准数据集
- [#papers/program-understanding](tags/papers/program-understanding) / [#papers/program-generation](tags/papers/program-generation)
- [#papers/computer-architecture](tags/papers/computer-architecture) — 体系结构
- [#papers/chiplet](tags/papers/chiplet) — Chiplet 加速器
- [#papers/dnn-accelerator](tags/papers/dnn-accelerator) — DNN 加速器
- [#papers/security](tags/papers/security) — 软件安全
- [#papers/llm](tags/papers/llm) — 大语言模型
- [#papers/vulnerability-detection](tags/papers/vulnerability-detection) — 漏洞检测

## 快速部署

```bash
npm install
npx quartz build --serve
```
