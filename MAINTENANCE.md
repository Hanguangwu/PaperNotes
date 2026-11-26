# 维护手册 — 论文数量激增时怎么办

## 现状（单仓库、静态构建）

- 站点基于 Quartz 5：`npx quartz build` 全量重建 `public/`，构建成本 = O(笔记数)。
  当前 9 篇笔记 + 13 个概念约 3–12s；**几百篇以内无需改架构**，瓶颈不会出现在构建上。
- 图片随仓库走：`content/wiki/sources/<slug>_images/`（AgentRxiv 6 张、Scam Town 6 张）+
  `content/wiki/sources/DeepPaperNote_output/images/`（RSI 图）。
  图片会被 Quartz `Assets` 插件**按路径原样拷贝**到 `public/`，页面引用相对路径。

## 增长策略（按阶段）

### 阶段 1：~30 篇（现在到不久后）

- 什么都不用做。保持一篇一目录的结构：
  - `wiki/sources/<slug>.md`
  - `wiki/sources/<slug>_images/`（该论文专有图）
- 新增论文：复制模板 → 写 frontmatter → 图放 `<slug>_images/`，引用 `![x](<slug>_images/figure_1.png)`。
- 唯一需要留意的：**文件名长度**（见下节「URL 与文件名」）。

### 阶段 2：30–100 篇

1. **给 slug 设上限**：新论文文件名用「短名 + 年份」，例如 `agentrxiv-2025.md`、
   `scamtown-2026.md`；长标题只放在 `title` 字段里，页面 H1 显示长标题，URL 保持短。
   旧文件可重命名（全库 ripgrep 双链后改，见 AGENTS.md「重命名条目」）。
2. **图片瘦身**：论文原图 50–300KB 一张，100 篇 ≈ 20–30MB，仓库会明显变重。
   两个选项（可并存）：
   - 提交前压一遍：`sharp` 或 ImageMagick 把 PNG 转 WebP/AVIF，体积降到 1/3–1/5；
   - 或把「原图」移出仓库（见阶段 3），只留压缩后的展示图。
3. **可选：git LFS 只管图片**。`git lfs track "*.png" "*.webp"`。
   好处：仓库本体轻量、clone 快；坏处：Vercel 构建需要 `GIT_LFS_SKIP_SMUDGE=false`
   （默认就是 false，一般无需配置），且 LFS 文件数 >1000 或单仓库 LFS >50GB 才需要付费档，
   100 篇以内免费额度够。

### 阶段 3：100 篇以上 — 图片单独存储

「图片是否拆出去」的决策依据不是数量，而是**单张图大小 × 图数** 与「是否要 git 历史」：

| 方案 | 做法 | 适合 |
|------|------|------|
| 留在仓库 | 现状 | 总图量 < 30MB，想保持单仓库简单、离线可构建 |
| 仓库外 + 相对引用 | 图存本地目录或另一仓库，页面写 `https://<cdn>/figures/<slug>/f1.png` | 图很大（>500KB/张）或数量多，不想进 git 历史 |
| Vercel Blob / R2 | 笔记里只写 URL，构建机不存图 | 图持续增量、需要 CDN 与上传脚本 |

注意两点：

- **不建议用 GitHub Pages + 独立图仓库**：GitHub 页面只能引用本仓库相对路径或绝对 URL，
  第二仓库的图必须走 CDN/对象存储，而不是「Pages 子路径」。
- Quartz 只会拷贝 `content/` 下的非 markdown 文件；**放 `content/` 外 = 不随站发布**，
  必须改成绝对 URL 引用。拆出去时写一个小脚本：
  `scripts/upload-figures.mjs`（Vercel Blob 或 R2 上传 + 把 markdown 里的相对路径批量换成 URL）。

## URL 与文件名（解决「名字太长」）

Quartz 的 URL = slugify（文件名 + 目录，小写、空格转 `-`、去掉特殊字符），`title` 只影响页面 H1 与
`<title>`，**不影响 URL**。所以：

1. 新笔记一律用短 slug 文件名：`agentrxiv-2025.md`、`gemini-hpca24.md`、`scamtown-2026.md`。
2. 旧长文件名按需重命名：
   ```bash
   # 1. 全库找旧路径的双链
   npx rg "gemini_mapping_and_architecture_co_exploration_for_large_scale_dnn_chiplet_accelerators" content/
   # 2. 批量替换（含 concepts/、index.md、papers.base 无关）
   # 3. mv 文件 + 同步 mv 对应 <slug>_images/ 目录
   # 4. npx quartz build 确认 0 断链
   ```
   重命名后旧 URL 会 404；已有分享出去的链接可临时加 `alias-redirects` 规则（该插件已启用）。
3. Bases 表格首列链接的是「文件名」而非 title——短文件名也会让表格更易读。

## 构建性能（真到了几百篇再考虑）

- 默认全量构建。若 build > 60s，可用 `--watch`（增量）开发；CI 上开 `--concurrency` 调大。
- `paper_url`/`arxiv` 等元数据都在 frontmatter，搜索/Bases 都是构建期一次性处理，无运行时成本。

## 一条建议的最小路线

1. 现在：保持现状，新论文一律短 slug + 专有图片目录（阶段 1）。
2. 30 篇时：加 `git lfs track` 图片 + 提交前压 WebP（阶段 2），旧长文件名批量缩短。
3. 100 篇且图 > 30MB：图迁 Vercel Blob/R2，markdown 改绝对 URL（阶段 3）。
