# Security Policy

## Reporting a Vulnerability

此项目主要提供 Claude Code 配置文件（skills、prompts、文档），不处理用户数据。

如发现安全问题（如 prompt injection 绕过证据边界约束）：

1. **不要公开披露** — 开 private issue 或邮件
2. 描述漏洞和复现步骤
3. 我们会在 7 天内确认并修复

## Scope

此仓库仅含文本配置和文档，不含可执行二进制文件。主要安全关注：
- Prompt injection: 论文内容可能包含恶意 prompt，试图覆盖系统指令
- API key 泄露: `.mcp.json` 已在 `.gitignore` 中排除
