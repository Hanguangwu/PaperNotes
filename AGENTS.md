# AGENTS.md — 论文阅读笔记知识库维护指南

## 角色定位

你是一位论文阅读笔记知识库的维护助手，负责维护本 [Quartz 5](https://quartz.jzhao.xyz) 站点的内容完整性、一致性和可扩展性。
站点主题为**论文阅读笔记**：每篇英文论文对应一条中文笔记，放在 `content/wiki/sources/` 下。

## 知识库结构

```
content/
├── index.md            ← 门户页：简介 + 内嵌 papers.base 动态列表
├── papers.base         ← Bases 数据视图（论文表格，服务端渲染，随笔记自动更新）
└── wiki/
    ├── sources/        ← 论文阅读笔记（一篇一个 .md）
    ├── concepts/       ← 概念页（从笔记摘取核心概念，双链关联论文与其他概念）
    └── templates/      ← 条目模板（不发布，供 AI 参考）
```

- 论文笔记：`content/wiki/sources/<文件名>.md`，文件名与论文相关，使用下划线连接。
- 模板目录 `wiki/templates/` 已被构建忽略，不会发布。
- 页面间用完整路径双链，例如 `[[wiki/sources/<文件名>]]`；Bases 中由 `file.name` 自动链接笔记页。

## 论文笔记规范

### Frontmatter（模板：`templates/source-template.md`）

```yaml
---
title: "<英文原标题>"
alias: <中文译名>
paper_id: arxiv:<编号>
arxiv: <编号>            # 或 doi
authors: [<作者1>, <作者2>]
institution: <机构>
venue: <发表渠道>
year: <YYYY>
paper_type: <benchmark | method | evaluation | ...>
paper_url: <PDF 链接>
code: <代码仓库>         # 无则省略
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [<主题标签>]
cssclasses: [paper]
---
```

约定：

- `paper_id` 创建后**不可更改**；`updated` 每次修改都要更新。
- 未知字段**整行省略**，不要写空值或 `null`。
- `cssclasses: [paper]` 必须保留，页面才应用论文样式。
- 元信息只写在 frontmatter；正文**不要**再出现「核心信息」区块，也不要重复 H1（标题由 `article-title` 组件渲染）。

### 正文章节（保持顺序）

「一句话总结」使用可折叠的 abstract callout（`+` 表示默认展开；点击标题可折叠/展开）：

```markdown
> [!abstract]+ 一句话总结
>
> <一句话结论>
```

其余章节按以下顺序：

```markdown
## 原文摘要（译）

<摘要翻译，可多段>

## 研究问题

## 核心贡献

## 数据与任务定义      # 方法类论文可改为「方法主线」

## 关键结果

## 深度分析

## 局限

## 我的笔记

## 引用

<BibTeX 代码块 + 链接列表>
```

- 每篇笔记必须包含「一句话总结」可折叠 callout（`> [!abstract]+ 一句话总结`）与 `## 引用`（BibTeX）。
- 数值结论要标注出处（如「表 4」「论文第 4 节」），不要编造数字。
- 原文引用用「」保持与原文一致；**纯 URL 行尾不要跟全角标点**（如「，」「。」），否则 Quartz 会把标点并入链接。

## 数学公式

公式由 Quartz 的 `latex` 插件（KaTeX，构建期渲染）处理。书写约定：

- 行内公式用美元符号定界：`$R_{\mathrm{warn}}$`；块级公式：
  ```markdown
  $$
  \mathcal{T}_d = (X_d, S_d, A_d)
  $$
  ```
- **不要用反斜杠定界 `\( … \)` 或 `\[ … \]`**：本仓库的 Obsidian 风格 markdown 插件不会识别它们，
  会原样显示为文本（曾导致公式不渲染）。
- 公式里的 `$` 会被当作定界符；需要字面美元时用 `\$`。
- 构建后验证：页面 HTML 中应出现 `class="katex"`，且正文里没有残留的 `\\(R_` 之类文本。

## 标签体系

- `tags` 用主题标签，如 `papers/llm`、`papers/chiplet`、`papers/security`、`papers/code-intelligence`。
- 标签页由 Quartz 自动生成；Bases 表格也展示 `tags` 列。

## Agent 维护操作

### 新增论文笔记（最常见）

1. 读取论文，提取 `title`、`alias`、`paper_id`、`arxiv`、`authors`、`institution`、`venue`、`year`、`paper_type`、`paper_url`、`code`。
2. 按 `templates/source-template.md` 创建 `content/wiki/sources/<短名>.md`。**文件名用短 slug（如 `agentrxiv-2025.md`），长标题只写进 `title` 字段**（URL = 文件名 slugify，与 title 无关）；专有图片放 `content/wiki/sources/<短名>_images/`，正文引用 `![x](<短名>_images/figure_1.png)`。
3. 按固定章节顺序写正文，确保「一句话总结」可折叠 callout（`> [!abstract]+ 一句话总结`）与 BibTeX。
4. 若笔记引入重要概念，按 `templates/concept-template.md` 在 `content/wiki/concepts/` 建概念页，并在概念页「在论文中的用法」与「相关概念」中双链对应论文/概念。
5. `papers.base` 为全局视图，无需手动更新列表。
6. 执行一次构建验证。
7. 规模增长（>30 篇压缩图片 + git LFS；>100 篇图片迁 CDN）见 [MAINTENANCE.md](../MAINTENANCE.md)。

### 更新现有笔记

- 修改内容后更新 `updated`。
- 不要更改已有 `paper_id`。

## 验证

```bash
npx quartz build
```

- 构建必须成功（exit 0）；本地构建会清空并重建 `public/`，这是预期行为。
- 关注输出中的 `Warning`：文件数、非法日期、slug 冲突。

## 质量控制

- 每篇笔记必须有摘要与引用，不能只堆链接。
- 关键数字须有出处。
- 中文标点统一使用全角。

## 禁止事项

- 不要修改 Quartz 框架文件（`quartz/`、`quartz.ts`、`package.json` 依赖）。
- 不要修改 `quartz.config.yaml`（除非用户明确指示）。
- 不要手工编辑 `public/`、`.quartz-cache/`、`node_modules/`。
- 不要在条目中使用 HTML 嵌入（保持纯 Markdown；BibTeX 代码块除外）。
- 不要编造原文中不存在的观点、数字或链接。
