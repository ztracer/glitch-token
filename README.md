# Dirty Tokens — 为何异常词元会让大模型输出错乱

> 从 SolidGoldMagikarp 到跨语言碎片：系统性解析 Glitch Token 的分类、技术机制、学术论文依据与检测修复方法

🌐 **在线访问**: [https://ztracer.github.io/glitch-token/](https://ztracer.github.io/glitch-token/)

---

## 项目背景

大语言模型（LLM）的分词器在训练过程中，会将高频字符串（如 Reddit 用户名、HTML 标签、UI 模板文字）贪婪地合并为独立 token。然而，当这些字符串在模型训练语料中极为罕见或完全缺失时，模型就从未为这些 token 更新梯度——它们成为了词汇表中的"幽灵"，即 **Glitch Token**（又称 Dirty Token、anomalous token、under-trained token）。

当 Glitch Token 出现在输入中时，会引发从嵌入异常到注意力崩溃的连锁反应，导致模型产生乱码、幻觉、重复循环、拒答甚至输出辱骂性内容。研究表明，主流模型中约 **4.3%** 的词汇表条目表现出故障行为，跨语言不完整 token 的幻觉率高达 **33-77%**。

本项目包含一份完整的中文研究报告，从 15 个真实案例出发，深入解析 Glitch Token 的分类体系、七层技术机制、MiniMax"马嘉祺"事件深度复盘，以及当前学术界检测与修复方法的最新进展。

---

## 项目内容

| 页面 | 说明 |
|------|------|
| [**研究报告**](https://ztracer.github.io/glitch-token/glitch-tokens-research.html) | 7 大章节、15 个案例、38 条参考文献 |

### 研究报告章节

1. **15 个 Dirty Token 案例分析** — 代码标识符、跨语言混合字符串、网页 UI 模板、特定名称四大类
2. **什么是 Dirty Token** — 定义、历史起源（Rumbelow & Watkins 2023）、关键统计数据
3. **分类体系与经典案例** — GlitchHunter 五分类法（FSE 2024）+ Cohere 补充分类
4. **技术机制深度解析** — 七层级联：未初始化嵌入 → 质心聚类 → L2 范数坍缩 → 注意力干扰 → 高预测熵 → 数值不稳定 → 后训练遗忘
5. **学术论文** — 8 篇核心论文卡片（含获奖标注）
6. **检测与修复** — Magikarp / GlitchHunter / GlitchProber / GlitchMiner 对比 + 分词器/模型/架构三层修复策略
7. **结论** — BPE-梯度不一致性的根本问题

### 深度案例：MiniMax"马嘉祺"事件

完整复盘 2026 年 MiniMax 大模型无法识别"马嘉祺"事件：时间线、症状、官方六步排查、根因（输入/输出 embedding 解耦）、量化数据（日语退化 29.7%、4.9% token 退化）、修复方案（全词表合成数据覆盖）。

---

## 技术栈

| 技术 | 用途 |
|------|------|
| **HTML5 / CSS3** | 页面结构与样式，CSS Custom Properties 浅色主题 |
| **Poppins / Lora** | 正文字体（Google Fonts） |
| **JetBrains Mono** | 代码/Token 等宽字体（外部 TTF） |
| **GitHub Pages** | 静态站点部署 |
| **GitHub Actions** | CI/CD 自动部署工作流 |

---

## 研究参考

### 核心学术论文

| # | 论文 | 会议/来源 | 年份 |
|---|------|----------|------|
| 1 | Glitch Tokens in Large Language Models: Categorization Taxonomy and Effective Detection (**GlitchHunter**) | FSE 2024 | 2024 |
| 2 | Fishing for Magikarp: Automatically Detecting Under-trained Tokens in Large Language Models | EMNLP 2024 Outstanding Paper | 2024 |
| 3 | SolidGoldMagikarp (plus, prompt generation) | Alignment Forum | 2023 |
| 4 | GlitchMiner: Mining Glitch Tokens via Gradient-based Discrete Optimization | AAAI 2025 | 2025 |
| 5 | GlitchProber: Advancing Effective Detection and Mitigation of Glitch Tokens (**GlitchProber**) | ASE 2024 | 2024 |
| 6 | Mitigating Forgetting in LLM Fine-Tuning via Low-Perplexity Token Learning | NeurIPS 2025 | 2025 |
| 7 | Improbable Bigrams Expose Vulnerabilities of Incomplete Tokens in Byte-Level Tokenizers | EMNLP 2025 | 2025 |
| 8 | Adversarial Tokenization | ACL 2025 | 2025 |
| 9 | Membership Inference Attacks on Tokenizers of Large Language Models | USENIX Security 2026 | 2026 |

### 行业报告与深度分析

| # | 来源 | 主题 |
|---|------|------|
| 10 | [bestaiweb.ai](https://www.bestaiweb.ai/glossary/glitch-tokens/) | Glitch Tokens 术语解释 |
| 11 | [perfecXion.ai](https://perfecxion.ai/articles/tokenization-exploits.html) | 7 大分词漏洞系统性整理（TokenBreak、MetaBreak 等） |
| 12 | [bestaiweb.ai](https://www.bestaiweb.ai/glitch-tokens-fertility-gaps-and-the-unsolved-technical-limits-of-subword-tokenization/) | BPE 结构性缺陷深度分析 |
| 13 | [Sharlayan](https://forsworns.github.io/zh/blogs/20240423/) | Karpathy Tokenizer 教学中文笔记 |
| 14 | [Tom Archer](https://tomarcher.io/posts/how-large-language-models-tokenize-text/) | 更多 Reddit 用户名 glitch token 案例 |

### MiniMax"马嘉祺"事件参考

| # | 来源 | 说明 |
|---|------|------|
| 15 | [MiniMax 官方博客](https://www.minimaxi.com/blog/sparse-token-forgetting-investigation) | 官方全链路技术排查报告 |
| 16 | [机器之心](http://news.qq.com/rain/a/20260509A062II00) | 详细技术分析，含输入/输出侧解耦发现 |
| 17 | [IT 之家](https://www.ithome.com/0/948/092.htm) | 官方排查结果媒体报道 |
| 18 | [快科技](https://www.163.com/dy/article/KSGKN4880511CPVM.html) | 含 4.9% token 退化、日语 29.7% 等关键数据 |
| 19 | [张鸿茹/腾讯研究院](https://m.sohu.com/a/1029435323_455313/) | 后训练灾难性遗忘机制分析 |
| 20 | [新智元](https://hub.baai.ac.cn/view/36580) | GlitchHunter 论文中文报道 |
| 21 | [量子位](https://hub.baai.ac.cn/view/37069) | Fishing for Magikarp 论文中文报道 |

完整 38 条参考文献详见[研究报告末尾](https://ztracer.github.io/glitch-token/glitch-tokens-research.html)。

---

## 项目结构

```
├── index.html                          # 首页
├── glitch-tokens-research.html         # 研究报告
├── assets/
│   ├── favicon.svg                     # 站点图标
│   ├── css/
│   │   └── style.css                   # 研究报告样式
│   ├── fonts/
│   │   ├── JetBrainsMono-Regular.ttf   # 等宽字体
│   │   └── JetBrainsMono-Bold.ttf      # 等宽字体（粗体）
│   └── js/
│       └── back-to-top.js              # 回到顶部按钮
└── .github/
    └── workflows/
        └── deploy.yml                  # GitHub Pages 自动部署
```

---

## 本地开发

本项目为纯静态站点，无需构建步骤：

```bash
# 克隆仓库
git clone https://github.com/ztracer/glitch-token.git
cd glitch-token

# 本地预览（任选一种）
python3 -m http.server 8080
# 或
npx serve .
```

然后访问 `http://localhost:8080`。

---

## 部署

推送到 `main` 分支后，GitHub Actions 自动部署到 GitHub Pages。

```bash
git add -A && git commit -m "update" && git push
```

---

## Vibe-Coding 产出的结构性缺陷反思

本项目初始版本由 Trae（vibe-coding agent）生成，后经人工审查与工程重构。以下是 vibe-coding 产出的系统性问题及其根因分析，供参考。

### 1. "能跑就行"陷阱 — 功能与工程质量的错位

Trae 产出的 v2 是一个 **1.9 MB 的单文件 HTML**，其中 88% 是 base64 编码的二进制数据。它"能跑"——浏览器打开确实能渲染——但这是工程意义上的反模式：

| 问题 | 具体表现 | 根因 |
|------|---------|------|
| Base64 内联一切 | 字体 230KB + ECharts 1.37MB 全部 base64 嵌入 HTML | Agent 追求"单文件可运行"，把所有依赖内联是最快确保不出错的方式，不关心加载性能、缓存、带宽成本 |
| ECharts 加载但未使用 | 1.37MB 的库被 base64 嵌入，但 HTML 中零个 `echarts.init()` 调用 | Agent 在某次迭代中添加了 ECharts，但后续修改图表方案时忘记移除依赖，缺乏"依赖审计"意识 |
| 未引用文件残留 | `_shared/fonts/` 下 4 个 TTF、`echarts.min.js` 共 1.37MB 完全未被引用 | Agent 改变了资源加载策略（外部文件 → base64 内联），但没有清理旧文件，只关心"当前能跑" |

**本质**：Vibe-coding agent 的优化目标是"输出能通过用户即时验收"，而非"产出可维护的工程资产"。

### 2. IDE 污染注入 — 环境泄漏到产物

v2 中包含 **~102KB 的 Trae/ICube IDE 注入代码**：12 个 `<style>` 块包含 `trae-browser-inspect`、`commentEditorCard`、`--vscode-*` 变量，2 个 Vite HMR `<script>` 包含 `import('/@vite/client')`，末尾残留 `<div data-trae-edit-ui="true"></div>`。

更严重的是，注入块中有**重复复制**（部分 style 块内容完全相同），说明 Agent 在迭代过程中多次从预览窗口复制，每次都叠加了一层 IDE 注入。

**本质**：Agent 直接从 IDE 预览窗口保存 DOM 产出最终 HTML，把 IDE 运行时环境一起打包——无法区分"我的代码"和"环境代码"的边界。

### 3. 关注分离缺失 — 单文件屎山的形成

v2 的 3983 行中，CSS 散落在多个 `<style>` 块中，JS 粘在 `</body>` 前，字体和库塞进 base64——每次修改都是"打补丁"而非"重构"。

**本质**：Vibe-coding agent 的工作模式是增量式 DOM 操作——每次用户说"加个导航栏"、"加个回到顶部按钮"，Agent 就在当前 HTML 末尾追加一段 `<style>` 和对应的 HTML，不会重构已有代码，不会提取公共样式，不会考虑文件组织。

### 4. 声明与实现不一致 — 文档漂移

README 声称"5 个 ECharts 数据可视化图表"，但 v2 中零个图表实现。技术栈列表包含 ECharts，项目结构列出 `echarts.min.js`——全是过时信息。

**本质**：Agent 在某次迭代中实现了图表，但后续版本替换后文档未同步。Vibe-coding 不会自动维护文档与代码的一致性——文档成为历史快照而非当前状态的反映。

### 5. 路径意识缺失 — 部署环境盲区

提取 CSS 到 `assets/css/style.css` 时，`@font-face` 中写了 `url('assets/fonts/...')`——这在 HTML 文件中正确，但在 CSS 文件中路径是**相对于 CSS 文件位置**解析的，实际解析成了 `assets/css/assets/fonts/...`（不存在）。

**本质**：Agent 对"代码运行环境"的理解是扁平的——知道"这个路径在 HTML 里能工作"，但不会推理"如果这段 CSS 被移到子目录下，路径解析规则会改变"。这种上下文迁移后的路径失效是静态站点重构中的经典陷阱。

### 总结

| 局限 | 表现 | 本质 |
|------|------|------|
| 单文件偏好 | Base64 内联一切，1.9MB 单文件 | 优化"即时可运行"而非"可维护性" |
| 环境泄漏 | IDE 注入代码打包进产物 | 无法区分"我的代码"和"运行时环境" |
| 增量补丁式演进 | CSS/JS 散落多处，从不重构 | 只做加法不做减法，缺乏全局视角 |
| 文档漂移 | README 声称不存在的功能 | 文档与代码无同步机制 |
| 路径/环境盲区 | CSS 提取后路径失效 | 缺乏"代码将在何处执行"的推理 |

Vibe-coding 产出的代码像"毛坯房"——结构上能住人，但管线乱走、废料堆积、图纸过时。它擅长快速生成"看起来对"的东西，但缺乏工程师的**全局一致性意识**和**清理习惯**。这正是为什么 vibe-coding 产出需要结构化审查和重构——不是因为它"错"，而是因为它"只对了一半"。

---

## License

本项目仅供学习与研究用途。论文版权归原作者所有。
