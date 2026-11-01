# 提问前的脱敏问题单

[← 返回目录](../README.md)

在 Issue、论坛或官方支持求助前，把下面这份填好。别人能更快定位，你也不会泄露凭证。

```text
发生时间与时区：
系统 / 终端（macOS zsh / Windows PowerShell / WSL Ubuntu ...）：
客户端与版本（codex --version / claude --version）：
安装来源（官方安装器 / npm / Homebrew / winget）：
登录方式（ChatGPT 账号 / 官方 API Key / 第三方 provider / 未确认）：
代理情况（无 / HTTP 代理 / VPN，是否换网络复测过）：
最后一条完整报错（遮住邮箱、Key、路径中的私密信息）：
复现步骤（最小化）：
已经试过的一步及结果：
```

## 绝对不要附上

- 密码、验证码、Cookie、Session
- API Key、`~/.codex/auth.json`、refresh token
- 完整的环境变量导出、未经检查的整个日志目录

## 日志位置（先自己检查再决定是否分享片段）

- Codex 会话记录：`$CODEX_HOME/sessions`（默认 `~/.codex/sessions`）
- macOS Codex App 日志：`~/Library/Logs/com.openai.codex/YYYY/MM/DD`

日志可能含代码、路径和凭据，只截取与本次报错相关的几行。
