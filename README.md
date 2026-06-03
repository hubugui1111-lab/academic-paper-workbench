# 📚 Academic Paper Workbench / 学术文献精读工作台

> **Claude Code 驱动的学术文献精读环境** — 多源搜索 + AI 精读 + 结构化输出 + 批量综述
>
> *An AI-powered academic paper reading environment built on Claude Code.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-2.0%2B-blue)](https://claude.ai/code)

---

## 🌟 What is this? / 这是什么？

一套 **完全开源的 Claude Code 工作流配置**，把 AI 编码助手变成学术文献研究伙伴。

**核心能力：**

| 能力 | 说明 |
|------|------|
| 🔍 **多源搜索** | 并行搜 11+ 学术源（OpenAlex/arXiv/PubMed/DBLP/Crossref/Google Scholar...），零 API Key |
| 📖 **AI 精读** | 中文深读模式，自动搜外部文献旁征博引，带证据边界约束防编造 |
| 📊 **批量综述** | 10-100 篇文献自动聚类 → 精读 → 生成综述 |
| 🎯 **结构化输出** | 统一输出格式：关键词→总结→基本信息→分节解析→批判讨论 |
| 🛠 **MCP 工具链** | 开箱即用的 6 种 MCP 服务配置 |

**不包含：** 任何闭源工具、专有模型、收费 API。全部使用开源 MCP 服务和 Claude Code 的免费/付费能力。

---

## 🚀 Quick Start / 快速开始

### Prerequisites / 前置条件

- [Claude Code](https://claude.ai/code) (>= 2.0)
- Python 3.10+
- Node.js 18+（部分 MCP 需要）
- 网络代理（访问境外学术 API 需要，可选但推荐）

### Installation / 安装

```bash
# 1. 克隆仓库
git clone https://github.com/hubugui1111-lab/academic-paper-workbench.git
cd academic-paper-workbench

# 2. Python 环境
python -m venv .venv
source .venv/Scripts/activate        # Windows
# source .venv/bin/activate          # Linux / macOS

# 3. 安装 MCP 工具（按需）
pip install pdf-mcp                  # 本地 PDF 阅读
pip install academic-mcp             # 19 源学术搜索
uv tool install paper-distill-mcp    # 11 源并行搜索

# 4. 复制 MCP 配置
cp .mcp.json.example .mcp.json
# 编辑 .mcp.json，填入代理配置（如需）

# 5. 在项目根目录启动 Claude Code
claude
```

### First Use / 首次使用

```bash
# 把论文 PDF 放入 papers/ 目录
# 然后在 Claude Code 中说：
"读 papers/xxx.pdf"
"精读第 3 章"
"搜一下 diffusion model 的最新论文"
```

---

## 🧰 Toolchain / 工具链

### MCP Services / MCP 服务

所有 MCP 零 API Key 可用，搜索优先级从前到后：

| 服务 | 覆盖源 | 安装方式 | 是否需要代理 |
|------|--------|----------|:----------:|
| **paper-distill-mcp** | OpenAlex + arXiv + PubMed + DBLP + Crossref 等 11 源 | `uv tool install paper-distill-mcp` | ❌ |
| **academic-mcp** | Google Scholar + arXiv + PubMed + OpenAlex + CORE 等 19 源 | `pip install academic-mcp` | ❌ |
| **academix** | OpenAlex(100K/天) + DBLP + arXiv + CrossRef | GitHub 源码 | ❌ |
| **arxiv-paper-mcp** | arXiv 全文解析 | `npm i -g @langgpt/arxiv-paper-mcp` | ✅ |
| **papers-mcp** | arXiv 搜索（备选） | `pip install papers-mcp` | ✅ |
| **pdf-mcp** | 本地 PDF 阅读/搜索/OCR | `pip install pdf-mcp` | ❌ |

### Claude Code Skills / AI 技能

| Skill | 功能 | 触发方式 |
|-------|------|----------|
| `paper-reading-zh` | 单篇/多篇论文中文精读（3 种模式） | 自动触发 |
| `efficient-literature-survey` | 10-100 篇文献批量综述 | `/efficient-literature-survey` |

---

## 📋 Workflows / 工作流

### A. 精读一篇论文

```
1. 放 PDF 到 papers/
2. "读 papers/xxx.pdf"
3. "精读第 5.5 节"
4. "对比这篇和另一篇的方法论差异"
```

系统自动：多源搜索相关文献 → 调用 pdf-mcp 读取 → paper-reading-zh 格式输出

### B. 搜索 + 精读 arXiv 论文

```
"帮我搜一下 chain-of-thought reasoning 的最新论文"
"精读这篇 arxiv 2301.12345 的第 4 节"
```

### C. 批量文献综述

```
"帮我写 papers/ 里这些文献的综述"
```

---

## 📁 Project Structure / 项目结构

```
.
├── .claude/
│   ├── CLAUDE.md                 # Claude Code 项目指令（核心）
│   ├── paper-reading-style.md    # 精读输出风格指南
│   ├── skills/
│   │   ├── paper-reading-zh.md           # 中文精读 skill
│   │   ├── paper-reading-zh-modes.md     # 精读模式参考
│   │   └── efficient-literature-survey.md # 批量综述 skill
│   └── memory/                   # 会话持久记忆（本地）
├── papers/                       # 📄 论文 PDF 放入此目录
├── downloads/                    # 下载的文献
├── .mcp.json                     # MCP 服务注册（个人配置）
├── .mcp.json.example             # MCP 配置模板
├── .gitignore
├── LICENSE                       # MIT
├── CONTRIBUTING.md               # 贡献指南
├── CODE_OF_CONDUCT.md            # 行为准则
└── README.md                     # 本文件
```

---

## 🧪 Evidence Boundary / 证据边界

这是本项目的关键设计原则 — 防止 AI 在解读论文时编造内容：

- ✅ 未核验的 venue/年份/CCF → 标 **"未核验"**
- ✅ 实验数字必须锚定到原文 **Table/Figure/段落**
- ✅ 跨论文比较检查 **口径一致性**
- ✅ 区分 **论文声明** vs **实验支持** vs **合理推断**
- ✅ 不编造公式、表格、图表描述
- ✅ 术语首次 **中英对照**，后文一致

---

## 🤝 Contributing / 贡献

欢迎！见 [CONTRIBUTING.md](CONTRIBUTING.md)。

主要贡献方向：
- **Skills 改进** — 提升 paper-reading-zh 的精读质量
- **MCP 集成** — 新增学术搜索源
- **文档** — 改进风格指南和示例
- **国际化** — English support for paper-reading skill

---

## 📄 License

MIT © 2026 hubugui1111-lab

## 🙏 Acknowledgements

- [Claude Code](https://claude.ai/code) by Anthropic
- [paper-distill-mcp](https://github.com/) by community
- [academic-mcp](https://github.com/) by community
- [pdf-mcp](https://github.com/nickliqian/pdf-mcp) by nickliqian
- [agent-papers-cli](https://github.com/) by community
- All open-source MCP and Python packages used in this project
