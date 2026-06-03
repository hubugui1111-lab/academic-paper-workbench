# Contributing to Academic Paper Workbench

感谢您有兴趣贡献！🎉

## 项目定位

此项目是一套 **Claude Code 驱动的学术文献精读环境**，核心价值在：

1. **Paper-reading-zh skill** — 中文论文精读 Skill，含证据边界约束
2. **Efficient-literature-survey skill** — 批量文献综述 Skill
3. **MCP 工具集成配置** — paper-distill-mcp / academic-mcp / academix / pdf-mcp 的开箱即用配置
4. **最佳实践文档** — 如何用 AI 高效读论文的工作流

## 贡献方式

### 🐛 报告 Bug

- 提 Issue，包含：
  - 环境说明（OS、Claude Code 版本、Python 版本）
  - 复现步骤
  - 预期行为 vs 实际行为

### 💡 功能建议

- 提 Issue 讨论后再动手
- 说明场景和痛点

### 📝 文档改进

- 修正错别字、翻译、补充示例
- 改进 CLAUDE.md 的 prompt 质量

### 🛠 代码贡献

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feat/your-feature`
3. 提交改动
4. 确保不包含个人敏感信息（token、路径、代理配置）
5. 创建 PR

## 开发指南

```bash
# 环境准备
python -m venv .venv
source .venv/Scripts/activate  # Linux: source .venv/bin/activate

# 安装依赖
pip install -r requirements.txt

# 安装 Claude Code skills
# 见 README.md 中的"安装"章节
```

## 提交规范

- 使用中文或英文提交信息
- 格式：`<type>: <简短描述>`
- type: `feat` / `fix` / `docs` / `style` / `refactor` / `chore`

## 开源礼仪

- 保持友善和专业
- 尊重不同水平贡献者
- 讨论技术，不针对人
- 接受 constructive feedback

---

**Questions?** 开 Discussion 或 Issue 聊。
