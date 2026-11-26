# Paper Notes — 论文阅读笔记（基于 Quartz 5）

个人论文阅读笔记站，基于 [Quartz 5](https://quartz.jzhao.xyz) 构建：每篇英文论文对应一条中文阅读笔记，
包含结构化 frontmatter、固定章节与 BibTeX 引用，并通过 Bases 动态列表汇总。

## 目录结构

```
content/
├── index.md                 ← 门户页：简介 + 全部论文动态列表（内嵌 base）
├── papers.base              ← Bases 数据视图（论文表格，服务端渲染，随笔记自动更新）
└── wiki/
    ├── sources/             ← 论文阅读笔记（一篇一个 Markdown 文件）
    └── templates/           ← 条目模板（不发布，供 AI/人工参考）
        ├── source-template.md   ← 论文笔记模板（主要使用）
        ├── person-template.md
        ├── work-template.md
        └── concept-template.md
```

## 论文笔记 frontmatter

| 字段 | 说明 |
|------|------|
| `title` | 论文英文原标题（页面主标题，由 article-title 组件渲染，正文不要再写 H1） |
| `alias` | 中文译名（显示在标题下方） |
| `paper_id` | 唯一标识，如 `arxiv:2102.04664`，创建后不可更改 |
| `arxiv` / `doi` | 文献编号；未知则省略整行 |
| `authors` | YAML 数组 |
| `institution` | 作者机构 |
| `venue` | 发表渠道（会议/期刊；预印本写 `arXiv preprint`） |
| `year` | 发表年份 |
| `paper_type` | `benchmark` / `method` / `evaluation` 等 |
| `paper_url` | 论文 PDF 链接 |
| `code` | 代码仓库链接；无则省略 |
| `created` / `updated` | 日期，每次修改更新 `updated` |
| `tags` | 主题标签（Bases 与标签页共用） |
| `cssclasses: [paper]` | 固定加上，使页面应用论文样式 |

完整模板见 `content/wiki/templates/source-template.md`。

## 正文章节（固定顺序）

```
> [!abstract] 一句话总结
## 原文摘要（译）
## 研究问题
## 核心贡献
## 数据与任务定义   （方法类可改为「方法主线」）
## 关键结果
## 深度分析
## 局限
## 我的笔记
## 引用
```

- 论文元信息（作者、机构、发表渠道、链接等）只写在 frontmatter，**不要**在正文里再写「核心信息」区块。
- 文末保留 BibTeX 代码块与链接列表。

## 数学公式

- 行内公式：`$R_{\mathrm{warn}}$`；块级公式：`$$ … $$`（KaTeX，构建期渲染）。
- 不要用 `\( … \)` / `\[ … \]` 定界（本站 markdown 插件不识别，会显示为原文）。

## 快速开始

```bash
npm install

# 本地预览（默认 http://localhost:8080）
npx quartz build --serve

# 构建静态站点
npx quartz build
```

## 部署（Vercel）

> 本仓库**没有** `vercel.json`（Quartz 5 模板默认不生成）；Vercel 对「无框架、无 vercel.json」的仓库
> 默认 Build Command 为 `npm run build`，但本包没有该 script，**必须手动配置**（或见下方 vercel.json 方案）。

**方案 A：控制台手配（推荐，保持仓库干净）**

1. Vercel → **Add New → Project**，导入本仓库（GitHub/GitLab/Bitbucket）。
2. 在配置页手动填写：
   - **Build Command**: `npm install && npx quartz build`
   - **Output Directory**: `public`
   - **Install Command**: 留空（已并入 Build Command）
   - **Node Version**: `22.x`（`.node-version` 声明的是 22.16.0）
3. Deploy。

**方案 B：仓库内置 `vercel.json`**

```json
{
  "buildCommand": "npm install && npx quartz build",
  "outputDirectory": "public",
  "installCommand": "npm install",
  "framework": null
}
```

> 注意：`npm install` 在 Vercel 上会先跑一遍（Install 阶段），Build Command 里再写一次可保证
> `prebuild`（插件安装）在构建前执行；两者不冲突。

**部署后必做：改 `baseUrl`**

1. 把 `quartz.config.yaml` 的 `baseUrl` 改成 Vercel 分配的实际域名（**不带协议**，如 `papers-notes.vercel.app`），再 Redeploy。
   - `baseUrl` 影响：内部链接、sitemap、OG 图片地址。当前值是 `humanwiki.vercel.app`，换新域必须改。
2. 私有仓库走 Vercel 免费版即可，不依赖 GitHub Pages 的付费限制。

### 自定义域名

1. Vercel 项目 **Settings → Domains** → Add，输入你的域名（如 `papers.example.com`）。
2. 按控制台提示在你的 DNS 服务商配置：
   - 子域（`papers.example.com`）：`CNAME → cname.vercel-dns.com`
   - 根域（`example.com`）：按控制台给出的 A/ALIAS 记录（通常 `76.76.21.21` 等）
3. 等 DNS 生效（几分钟到 48h）后，把 `quartz.config.yaml` 的 `baseUrl` 改成该域名并 Redeploy。
4. **不需要**在仓库里放 `CNAME` 文件；`@quartz-community/cname` 插件保持关闭。
5. HTTPS 由 Vercel 自动签发证书，DNS 生效后会显示「Active」。

### 图片 LFS（如已启用 git LFS 存图）

- Vercel 构建默认拉取 LFS，无需额外配置。
- 本地 clone 用 `git clone`（新版 git 默认装 git-lfs 过滤器）；若构建机报「binary file not expanded」，
  在 Build & Release 环境变量加 `GIT_LFS_SKIP_SMUDGE=false`。

## AI Agent 维护

本库支持 AI Agent 自动维护，详见 [AGENTS.md](./AGENTS.md)。

## License

MIT
