# Codex CLI / Claude Code 中文安装与排错手册

面向中文开发者的一份**可直接照做**的笔记：OpenAI Codex CLI 和 Anthropic Claude Code 怎么装、怎么登录，以及最常见的 401 / 403 / 429 / token 刷新失败 / 超时断流该按什么顺序排查。

> 非官方文档，与 OpenAI、Anthropic 没有隶属关系。命令和字段会随客户端版本变化，**以你本机 `--help` 输出和官方文档为准**。最后整理：2026-11-01。

## 目录

| 文档 | 解决什么问题 |
| --- | --- |
| [Codex CLI 安装（macOS / Linux / Windows / WSL2）](docs/codex-cli-install.md) | 选哪种装法、包名别装错、Node 版本、多来源冲突 |
| [Codex 登录：ChatGPT 账号 vs API Key](docs/codex-login-auth.md) | 两条身份路径的区别、额度和账单各算在哪里 |
| [Claude Code 安装与接入国内模型](docs/claude-code-install.md) | 原生安装器 / npm、环境变量、settings.json、DeepSeek / GLM / Kimi / 百炼端点 |
| [401 / 403：鉴权与权限](docs/errors-401-403.md) | Missing bearer、invalid_api_key、地区不支持、工作区权限 |
| [429：限流、额度、余额分开看](docs/errors-429-usage.md) | rate limit、usage limit、insufficient_quota 各自的处理 |
| [token 刷新失败与登录循环](docs/token-refresh-login-loop.md) | access token could not be refreshed、浏览器授权完终端还在等 |
| [超时、Reconnecting、stream disconnected](docs/network-timeout-proxy.md) | 代理协议、HTTPS_PROXY、macOS 桌面端代理 |
| [提问前的脱敏问题单](docs/issue-checklist.md) | 求助时该贴什么、绝对不能贴什么 |

## 30 秒分流

| 你看到的 | 多半是 | 先去 |
| --- | --- | --- |
| `command not found: codex` / `claude` | 安装或 PATH | [Codex 安装](docs/codex-cli-install.md) · [Claude Code 安装](docs/claude-code-install.md) |
| 401 / Unauthorized / Missing bearer / invalid_api_key | 鉴权 | [401 / 403](docs/errors-401-403.md) |
| 403 / region not supported / permission denied | 资格或权限 | [401 / 403](docs/errors-401-403.md) |
| 429 / rate limit / usage limit reached | 限流或额度 | [429](docs/errors-429-usage.md) |
| access token could not be refreshed / 登录页反复跳 | 登录态失效 | [token 刷新](docs/token-refresh-login-loop.md) |
| timed out / Reconnecting / stream disconnected | 网络链路 | [超时与代理](docs/network-timeout-proxy.md) |

三条原则，能省掉大部分弯路：

1. **先抄完整报错原文**，不要只记一个状态码。同样是 401，可能是没登录，也可能是 Key 填错。
2. **先确认身份路径**：ChatGPT 账号登录、官方 API Key、还是第三方 provider。三者的额度和账单互不相通。
3. **一次只改一个条件**。同时换代理、换账号、换模型，修好了也不知道是哪一步起作用。

## 安全底线

- 不要把 `~/.codex/auth.json`、API Key、Session、Cookie、验证码发给任何人，包括“客服”和群友。
- 不要为了消除报错关闭 TLS 证书校验，或用 `sudo` 反复覆盖全局 npm 安装。
- 安装脚本只从官方域名下载（`chatgpt.com/codex/install.sh`、`claude.ai/install.sh`）。

## 延伸阅读

本仓库的部分内容整理自 Nano Banana 教程站的公开文章，那边有截图和更新记录：

- [Codex CLI 安装与登录教程](https://nano-banana.lol/tutorials/codex-cli-install-login)
- [Codex 401 / 429 / 超时分流对照表](https://nano-banana.lol/how-to/codex-401-429-timeout-triage)
- [Claude Code 国内安装教程](https://nano-banana.lol/tutorials/claude-code-install-china)

## 参与

发现命令过期或写错，欢迎开 Issue / PR。请附上客户端版本号和官方文档链接，不要贴任何凭证。

## 许可

文字内容采用 [CC BY 4.0](LICENSE)。
