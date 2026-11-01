# token 刷新失败与登录循环

[← 返回目录](../README.md)

## 症状

- 报错里出现 *access token could not be refreshed*、*refresh token* 已失效 / 已被使用、或要求重新登录。
- 浏览器里显示授权成功，终端却一直停在等待登录。
- 同一账号在多台机器上用，某台突然频繁掉登录。

这些都属于**登录态问题**，和额度、会员是否到期是两回事。

## 处理顺序

**1. 退出后重新登录（解决大多数情况）**

```bash
codex logout
codex login
codex login status
```

常见原文类似 `Failed to refresh token: 401 Unauthorized`、`Your refresh token has already been used ... Please try signing in again.`：refresh token 是一次性的，被另一端或旧进程用过后，本地缓存那份就作废了。

**2. 检查是否有多份配置在打架**

- **`OPENAI_API_KEY` 抢权**：shell 里设了这个变量时，客户端可能优先用它而不是刚登录的 ChatGPT 会话。`echo $OPENAI_API_KEY` 看有没有值（别把值贴出来），不需要就 `unset OPENAI_API_KEY` 后再启动，或从 `.zshrc` / `.bashrc` 里删掉。
- **自定义 provider 报 `API_KEY_REQUIRE`**：较新版本里自定义 provider 不再自动继承 `auth.json` 鉴权，按你所用版本的官方配置说明检查 `~/.codex/config.toml` 中该 provider 的鉴权字段（社区反馈涉及 `requires_openai_auth`）。

- 同时设置了 `OPENAI_API_KEY` 环境变量和 ChatGPT 登录，`codex login status` 看实际生效的是哪个。
- 多台设备、多个终端（Windows 原生 + WSL）各自有一份登录缓存，在哪个环境报错就在哪个环境重新登录。
- 不要把一台机器的 `auth.json` 复制到另一台“共享登录”，这既不安全，也容易让刷新令牌冲突。

**2.5 仍然 401：清掉本地登录缓存再登录**

先备份，再删除 `~/.codex/auth.json`（只删这个凭证文件即可，`config.toml` 和会话记录可以保留），然后 `codex login`。

**3. 浏览器授权完终端还在等**

- 确认浏览器登录的是你预期的那个账号 / 工作区。
- 回调要能回到本机：远程服务器、容器、无桌面环境要按官方文档使用对应的登录方式（如设备码流程），不要随意转发端口给陌生服务。
- 系统代理 / VPN 拦了本地回调或长连接时，先换网络复测，再查代理设置，见 [超时与代理](network-timeout-proxy.md)。

**4. 系统时间**

本机时间严重偏差会导致令牌校验失败。开启自动同步时间后再登录一次。

**5. 还不行**

升级客户端到最新版本，再按 [提问前的脱敏问题单](issue-checklist.md) 整理材料找官方支持。

## 绝对不要

- 把 `auth.json`、Session、Cookie、refresh token 发给任何“帮你修复”的人。
- 为了跳过登录去用来源不明的“免登录版”客户端。

ChatGPT 网页端 access token / session 是什么、为什么不能外泄，见 [ChatGPT Access Token 与 Session JSON 说明](https://nano-banana.lol/blog/chatgpt-access-token-session-json-guide-2026)。
