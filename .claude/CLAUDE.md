# Academic Paper Workbench — 文献精读与论文分析工作台

此项目是 Claude Code 驱动的学术文献精读环境。核心能力：多源学术搜索 → MCP 工具链 → AI 精读技能 → 结构化输出。

---

## Session Start Protocol

【强制】每次会话开始，按以下步骤执行：

1. **Read `MEMORY.md`** → `.claude/memory/MEMORY.md`
2. **Read 所有引用的 memory 文件**
3. **Read LEARNINGS.md（tail -30）** → `.learnings/LEARNINGS.md`
4. **然后才回"准备好了"**

---

## 文献优先原则（强制）

**每次回答问题前，必须先查学术文献。** 不限于"读论文"请求。搜到就列出参考文献（3-5 篇），找不到写"未找到相关文献"。

### 搜索顺序

1. **paper-distill-mcp** — 11 源并行（OpenAlex/arXiv/PubMed/DBLP/Crossref），零 key ✅
2. **academic-mcp** — 19 源（含 Google Scholar/arXiv/PubMed/OpenAlex/CORE），零 key
3. **academix** — OpenAlex（100K 免费/天）+ DBLP + arXiv + CrossRef，BibTeX
4. **arxiv-paper-mcp** — arXiv 搜索与全文解析（npm）
5. **papers-mcp** — arXiv 搜索与提取（系统 pip）
6. **pdf-mcp** — 本地 PDF 手术刀式阅读（venv）

### 引用规范

- 至少 3-5 篇，覆盖经典奠基 + 近期综述 + 相关工作对比
- 标注来源链接，术语首次中英对照
- 搜不到 → 明确说"未找到相关文献"
- 仅部分相关 → 标注"仅找到部分相关文献"
- 全部不可用 → 说明情况，不编造

### 证据边界

- 未核验 venue/年份/CCF/代码链接 → 标"未核验"
- 实验数字锚定到原文 Table/Figure/段落
- 跨论文比较检查口径一致性
- 区分论文声明、实验支持、合理推断、不确定信息
- 不编造公式、表格、图表描述

---

## 论文精读 — 默认响应风格

询问/精读/分析论文时，**必自动加载 `paper-reading-zh` skill** 并遵循深读模式。完整风格指南见 `.claude/paper-reading-style.md`。

### 输出结构

```
关键词：3-5 个
一段话总结：≤150 字
论文基本信息：标题 / venue/年份 / 链接 / 任务领域
1. 核心问题与贡献
2. 方法深度解析
3. 实验与结果
4. 批判性讨论
5. 复现/应用提示（条件性省略）
材料范围：[标注读了哪些内容]
```

---

## 环境准备

```bash
python -m venv .venv
source .venv/Scripts/activate   # Windows
# source .venv/bin/activate     # Linux/macOS
```

## MCP 工具

所有 MCP 在 `.mcp.json`（项目根目录）中注册。复制 `.mcp.json.example` 并按需配置。

| 服务 | 用途 | 来源 |
|------|------|------|
| **paper-distill-mcp** | 11 源并行搜索，零 key | `uv tool install paper-distill-mcp` |
| **academic-mcp** | 19 源搜索+下载+阅读，零 key | `pip install academic-mcp` |
| **academix** | OpenAlex/DBLP/arXiv 专精 | GitHub 源码安装 |
| **arxiv-paper-mcp** | arXiv 全文解析 | `npm i -g @langgpt/arxiv-paper-mcp` |
| **papers-mcp** | arXiv 备选搜索 | `pip install papers-mcp`（系统 pip）|
| **pdf-mcp** | 本地 PDF 阅读 | `pip install pdf-mcp` |

## Skills

| Skill | 触发方式 | 用途 |
|-------|----------|------|
| paper-reading-zh | 自动触发（深读/工程拆解/调研比较） | 单篇/多篇论文精读 |
| efficient-literature-survey | `/efficient-literature-survey` | 批量 10-100 篇文献综述 |

## 工作流

### A. 精读一篇论文（MCP 推荐）
```
把 PDF 放进 papers/ → "读 papers/xxx.pdf" → "精读第 5.5 节"
```
### B. 精读（CLI 备选）
```
paper read path/to/paper.pdf
paper outline path/to/paper.pdf
```
### C. 搜 arXiv 论文
```
"在 arXiv 上搜一下 diffusion model 的最新论文"
```
### D. 批量综述
```
"帮我写这个文件夹里文献的综述"
```

## 项目结构

```
├── .claude/
│   ├── CLAUDE.md                     # 本文件
│   ├── paper-reading-style.md        # 精读风格指南
│   ├── skills/
│   │   ├── paper-reading-zh.md
│   │   ├── paper-reading-zh-modes.md
│   │   └── efficient-literature-survey.md
│   └── memory/                       # 会话记忆（gitignored）
├── papers/                           # PDF 放这里（gitignored）
├── downloads/                        # 下载文献（gitignored）
├── .mcp.json                         # MCP 服务配置（gitignored）
├── .mcp.json.example                 # MCP 配置模板
├── .gitignore
├── LICENSE (MIT)
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
└── README.md
```

## 注意事项

- 多数 MCP 服务需要代理访问境外 API，在 `.mcp.json` 的 `env` 中配置 `HTTP_PROXY`
- 推荐优先用 MCP 服务，无需切命令行
- 新装 MCP 需重启 Claude Code 会话加载
- academic-mcp 的 ScienceDirect/Springer/IEEE/Scopus 收费源需 API key（不用不影响免费源）
- `paper` CLI 有 Windows `rename` bug（`storage.py` 中 `rename`→`replace`），见 [agent-papers-cli#issues](https://github.com/agent-papers-cli/issues)

---

## Session End Protocol

用户道别时，先做：
1. 新学到的追加到 `.learnings/LEARNINGS.md`
2. 更新 `.claude/memory/` 下的记忆文件
3. 更新此文件或其他文档（如有变化）
