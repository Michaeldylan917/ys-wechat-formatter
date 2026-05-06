# ys-wechat-formatter

一个 Claude Code Skill，将 Markdown 文章排版为微信公众号可直接粘贴的内联 CSS HTML，支持 18 种视觉主题。

## 功能

- Markdown → 微信公众号 HTML（所有 CSS 内联，无需外部样式表）
- 18 种精选视觉主题（晚点、金融时报、纽约时报、Claude、Jony Ive 等）
- 段落自动拆分、强调克制、留白优化
- 支持"先看方案"模式，确认排版策略后再生成
- 生成后可微调（改引用、加粗、调整克制度等）

## 安装

将 `ys-wechat-formatter/` 目录放入你的 Claude Code 项目目录或 skills 目录下即可。

## 使用

在 Claude Code 中输入以下任一触发词：

- `排版`
- `格式化公众号`
- `公众号排版`
- `format article`

然后选择主题、提供 Markdown 文件或粘贴文本。

## 主题一览

| # | 主题 | 风格 |
|---|------|------|
| 1 | 晚点风格 | 红色强调，深度报道感 |
| 2 | 金融时报 | 衬线字体，淡粉底，英伦财经 |
| 3 | Claude | Anthropic 暖橙渐变，现代感 |
| 4 | 技术风格 | 蓝绿配色，代码友好 |
| 5 | Hische·编辑部 | 衬线+红色，杂志感 |
| 6 | 纽约时报 | Georgia 大字标题，经典报纸 |
| 7 | 默认公众号 | 蓝色系，通用 |
| 8 | 优雅简约 | 宋体，首行缩进，中文传统 |
| 9 | 深度阅读 | 无装饰，纯黑白 |
| 10 | Jony Ive | 超轻字重，大量留白 |
| 11 | Medium 长文 | Georgia 标题，无衬线正文 |
| 12 | Apple 极简 | SF Pro，极简灰调 |
| 13 | 原研哉·空 | 极宽行高，大量呼吸空间 |
| 14 | 安藤·清水 | 灰调混凝土质感 |
| 15 | 高迪·有机 | 彩色渐变，有机曲线 |
| 16 | Guardian 卫报 | 蓝底黄标，英式新闻 |
| 17 | Nikkei 日経 | 日文排版，紧凑信息密度 |
| 18 | Le Monde 世界报 | Didot 衬线，法式优雅 |

## 技术约束

- 所有 CSS 内联（`style="..."`），兼容微信公众号编辑器
- 不使用 CSS Grid / Flexbox
- Markdown 元素全覆盖：标题、段落、加粗、斜体、引用、代码块、列表、表格、图片、链接、分隔线

## 致谢

本项目的样式灵感与排版理念参考自[花叔的 huasheng_editor](https://github.com/alchaincyf/huasheng_editor)，感谢花叔在公众号排版领域的探索与分享。

## License

MIT
