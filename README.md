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
| [**研究报告**](https://ztracer.github.io/glitch-token/glitch-tokens-research.html) | 7 大章节、15 个案例、5 个 ECharts 数据可视化、28 条参考文献 |

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
| **HTML5 / CSS3** | 页面结构与样式，CSS Custom Properties 暗色主题 |
| **Apache ECharts** | 5 个数据可视化图表（类型分布、普遍程度、语种退化、余弦相似度、检测方法雷达图） |
| **Instrument Sans** | 正文字体（Regular / Bold） |
| **JetBrains Mono** | 代码/Token 等宽字体（Regular / Bold） |
| **GitHub Pages** | 静态站点部署 |
| **GitHub Actions** | CI/CD 自动部署工作流 |

---

## 数据可视化

研究报告内嵌 5 个 ECharts 交互式图表：

1. **Token 类型分布** — 15 个案例按四大类别的水平柱状图
2. **Glitch Token 普遍程度** — 主流模型故障比例与幻觉率对比
3. **各语种 Token 退化比例** — 日语 29.7% 显著高于其他语种
4. **lm_head 余弦相似度对比** — 实验组（+全词表覆盖）vs Baseline
5. **检测方法能力雷达图** — Magikarp / GlitchHunter / GlitchProber / GlitchMiner 五维对比

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

完整 28 条参考文献详见[研究报告末尾](https://ztracer.github.io/glitch-token/glitch-tokens-research.html)。

---

## 项目结构

```
├── index.html                          # 首页
├── glitch-tokens-research.html         # 研究报告（含 ECharts 图表）
├── assets/
│   └── favicon.svg                     # 站点图标
├── _shared/
│   ├── css/                            # 共享样式（预留）
│   ├── fonts/
│   │   ├── InstrumentSans-Regular.ttf
│   │   ├── InstrumentSans-Bold.ttf
│   │   ├── JetBrainsMono-Regular.ttf
│   │   └── JetBrainsMono-Bold.ttf
│   └── js/
│       └── echarts.min.js              # ECharts 可视化库
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

## License

本项目仅供学习与研究用途。论文版权归原作者所有。
