---
name: ys-wechat-formatter
description: 将 Markdown 文章排版为微信公众号可直接粘贴的 HTML，支持 18 种视觉主题。触发词：排版、格式化公众号、公众号排版
---

# ys-wechat-formatter

将 Markdown 文章排版为微信公众号可直接粘贴的 HTML，所有 CSS 内联，无需外部样式表。支持 18 种视觉主题。

## 触发条件

用户说出以下任一表达时触发：
- "排版"
- "格式化公众号"
- "公众号排版"
- "format article"
- "format wechat"
- 用户提供 Markdown 文件或文本并要求排版为公众号格式

## 样式选择

使用 AskUserQuestion 让用户选择主题。先展示 6 个常用样式（编号 1-6）：

1. 晚点风格 (latepost-depth)
2. 金融时报 (wechat-ft)
3. Claude (wechat-anthropic)
4. 技术风格 (wechat-tech)
5. Hische·编辑部 (hische-editorial)
6. 纽约时报 (wechat-nyt)

然后添加"更多样式"选项。如果用户选择"更多样式"，展示剩余 12 个：

7. 默认公众号风格 (wechat-default)
8. 优雅简约 (wechat-elegant)
9. 深度阅读 (wechat-deepread)
10. Jony Ive (wechat-jonyive)
11. Medium 长文 (wechat-medium)
12. Apple 极简 (wechat-apple)
13. 原研哉·空 (kenya-emptiness)
14. 安藤·清水 (ando-concrete)
15. 高迪·有机 (gaudi-organic)
16. Guardian 卫报 (guardian)
17. Nikkei 日経 (nikkei)
18. Le Monde 世界报 (lemonde)

## 输入处理

- 用户提供文件路径：读取 .md 文件内容
- 用户直接粘贴文本：使用粘贴的文本
- 不明确时主动询问用户输入来源

## 工作流

### 默认流程（用户未说"先看方案"时）

1. 确认输入来源（文件路径 or 粘贴文本）
2. 读取/获取 Markdown 内容
3. 用户选择样式主题
4. 读取 `references/styles.json` 获取所选主题的 CSS
5. 应用内容格式化规则（见下方）
6. 生成带内联 CSS 的 HTML
7. 保存为 .html 文件 + 在对话中展示

### "先看方案"流程

1. 同上步骤 1-3
2. 不生成 HTML，而是输出排版方案：
   - 标题层级规划
   - 强调策略（加粗/高亮/引用块的使用计划）
   - 段落节奏与留白规划
3. 等待用户确认（"确认方案，开始排版"或提出修改意见）
4. 确认后执行默认流程步骤 5-7

## 内容格式化规则

### 段落

- 单段不超过 2-4 行
- 大段文字必须拆分为短段落
- 核心观点前后必须有空行

### 强调层级

- 仅加粗关键词或关键句，不整段加粗
- 引用块用于观点/情绪/价值判断，每篇最多 3 个
- 分隔线（---）用于：结构转换、情绪转折、总结前

### 留白策略

- 章节之间用空行留出呼吸感
- 不出现视觉疲劳块（密集文字）

### 约束

- 不改变内容，仅优化呈现形式
- Emoji 极度克制使用，每篇最多 1 个
- 阅读优先级：手机端连续阅读不疲劳

## HTML 生成规则

从 `references/styles.json` 读取所选主题。主题的 `styles` 对象包含各 HTML 元素的 CSS 字符串。

### Markdown → HTML 映射

| Markdown | HTML 标签 | CSS 来源 |
|----------|-----------|----------|
| `# Title` | `<h1 style="...">` | theme.styles.h1 |
| `## Subtitle` | `<h2 style="...">` | theme.styles.h2 |
| `### Heading3` | `<h3 style="...">` | theme.styles.h3 |
| `####` 等 | `<h4>`, `<h5>`, `<h6>` | theme.styles.h4-h6 |
| `**bold**` | `<strong style="...">` | theme.styles.strong |
| `*italic*` | `<em style="...">` | theme.styles.em |
| `> quote` | `<blockquote style="...">` | theme.styles.blockquote |
| `---` | `<hr style="...">` | theme.styles.hr |
| 段落 | `<p style="...">` | theme.styles.p |
| `- item` | `<ul><li style="...">` | theme.styles.ul + theme.styles.li |
| `1. item` | `<ol><li style="...">` | theme.styles.ol + theme.styles.li |
| `` `code` `` | `<code style="...">` | theme.styles.code |
| ` ```block``` ` | `<pre style="..."><code style="...">` | theme.styles.pre + theme.styles.code |
| `[text](url)` | `<a href="url" style="...">` | theme.styles.a |
| `![alt](url)` | `<img src="url" style="...">` | theme.styles.img |
| 表格 | `<table>`, `<tr>`, `<th>`, `<td>` | theme.styles.table/tr/th/td |

### 最终 HTML 结构

```html
<section style="[theme.styles.container]">
  [带内联样式的格式化内容]
</section>
```

## 输出规则

### 文件保存

- 文件输入：保存为 `原文件名.formatted.html`，与源 .md 文件同目录
- 粘贴文本：保存到 `D:\DWJG\D盘桌面\公众号\output\文件名.html`（目录不存在则创建）
- 粘贴文本的文件名基于文章标题或时间戳生成合理文件名

### 对话输出

- 在对话中用代码块展示完整 HTML
- 告知用户文件保存路径

## 微调

初始输出后，用户可请求调整：
- "把第X段改成引用" → 将该段落改为 blockquote
- "这句加重" → 添加加粗
- "整体再克制一点" → 减少强调
- 重新生成 HTML 并覆盖已保存的文件

## 技术约束

- 所有 CSS 必须内联（style="..."），不使用外部样式表
- 不使用 CSS Grid 或 Flexbox（微信公众号不支持）
- 图片保留为 `<img>` 标签并应用主题的 img 样式，不转为 Base64
- Skill 自包含，从自身 `references/` 目录读取 styles.json
