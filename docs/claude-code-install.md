# Claude Code 安装与接入国内模型

[← 返回目录](../README.md)

三件事分开做：**安装客户端 → 拿到对应厂商的 Key → 配置兼容端点和模型映射**。接入国产模型 API 不代表在运行 Claude 模型，也不会开通 Claude Pro / Max。

## 1. 安装

**原生安装器（推荐，不需要 Node.js）**

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash
claude --version
```

```powershell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
claude --version
```

**npm（已有 Node 时）**

```bash
npm install -g @anthropic-ai/claude-code
claude --version
```

查阅时 Claude Code 文档要求 npm 路径使用 Node.js 22 或以上；部分厂商教程仍写 Node 18+，以客户端当前文档为准。macOS 也可 `brew install --cask claude-code`。

| 环境 | 验证方式 |
| --- | --- |
| macOS | 新开终端运行 `claude --version` |
| Windows 原生 | `Get-Command claude -All` 检查实际命令来源 |
| WSL | `command -v claude`，确认不是误用 Windows 路径 |

## 2. 端点和 Key 必须一一对应

下表来自各厂商官方接入文档（查阅日 2026-11-01）。这里填的是 Base URL，**不要自己再拼 `/v1/messages`，也不要填 OpenAI 兼容的 `/v1` 地址**。模型名更新很快，以厂商当前文档为准。

| 服务 | ANTHROPIC_BASE_URL | 说明 |
| --- | --- | --- |
| DeepSeek | `https://api.deepseek.com/anthropic` | DeepSeek 平台 Key |
| 智谱 GLM | `https://open.bigmodel.cn/api/anthropic` | GLM Coding Plan Key |
| Kimi / Moonshot | `https://api.moonshot.ai/anthropic` | Kimi 平台 Key |
| 阿里云百炼 Coding Plan | `https://coding.dashscope.aliyuncs.com/apps/anthropic` | Coding Plan 专用 Key，不能和按量计费混用 |

## 3. 环境变量（当前终端临时生效）

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_AUTH_TOKEN="替换为你自己的 Key"
export ANTHROPIC_MODEL="厂商文档给出的主模型"
export ANTHROPIC_DEFAULT_OPUS_MODEL="同上"
export ANTHROPIC_DEFAULT_SONNET_MODEL="同上"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="厂商文档给出的轻量模型"
export CLAUDE_CODE_SUBAGENT_MODEL="厂商文档给出的轻量模型"
claude
```

PowerShell 把 `export NAME=` 换成 `$env:NAME=`。**切换厂商时 Base URL、Token、模型映射要一起换**，只换 Key 必然出错。

## 4. 写进 settings.json（长期生效）

`~/.claude/settings.json` 的 `env` 字段（Windows 在用户目录下 `.claude/settings.json`）。编辑前先备份，合并而不是覆盖原有的权限、插件设置；JSON 不能有注释和尾随逗号。

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "替换为你自己的 Key",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "厂商文档给出的模型",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "厂商文档给出的模型",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "厂商文档给出的轻量模型"
  }
}
```

配置文件里的旧 env 会覆盖终端里的同名变量，换厂商后一定要回来检查。

## 5. 确认接入成功

1. 新开终端，`claude --version`。
2. 在一个练习目录启动 `claude`，输入 `/status`，核对 Base URL 和模型属于同一家（截图记得遮住凭证）。
3. 只做一个小任务，比如解释 README。
4. 回厂商控制台看到实际请求和账单，才算真的通了。

## 常见报错

- **找不到 claude / npm EACCES**：先查 PATH 和安装来源，改用原生安装器，不要 sudo 覆盖。
- **401 / 403**：核对 Token、端点、产品、地域是否是同一家同一个产品；Claude 网页会员、ChatGPT 订阅都不能当这里的 Key 用。
- **主会话能用、后台任务报 model not found**：检查 Haiku / Sonnet / Opus 和子任务模型映射，只改 `ANTHROPIC_MODEL` 会漏掉其他档位。
- **400 invalid thinking**：部分模型要求开启 Thinking，按厂商文档调整。
- **超时**：区分是下载安装、DNS/TLS 还是模型请求超时，不要关闭证书校验。

四家厂商完整配置示例和更新记录见 [Claude Code 国内安装教程：DeepSeek、GLM、Kimi 与百炼接入](https://nano-banana.lol/tutorials/claude-code-install-china)。
