# Codex 登录：ChatGPT 账号 vs API Key

[← 返回目录](../README.md)

Codex 有两条主要身份路径，决定了**用量从哪扣、账单记在哪**。很多“充了钱还报错”的问题，根源是把这两条路混在一起。

| 路径 | 怎么辨认 | 用量与账单 |
| --- | --- | --- |
| ChatGPT 账号登录 | `codex login status` 显示 ChatGPT；首次运行走浏览器授权 | 计入 ChatGPT 套餐包含的 Codex 用量 |
| 官方 API Key | 环境变量或配置里是 OpenAI 官方 Key | 计入 OpenAI Platform 项目的按量账单 |
| 自定义 provider / 中转 | Base URL 或 provider 名不是官方默认 | 该供应商自己的额度与账单 |

**三者不能互相充值或混算**：ChatGPT 会员不是 API 余额，API 余额也不会让套餐用量变多。

## 登录

```bash
codex login
codex login status
```

- 登录交互由你本人完成；设备码只用于你自己刚发起的那次登录。
- 不要把 Key 写进命令历史、项目文件或聊天记录。用 API Key 时优先放在本机环境变量里。
- 已登录不等于有资格：客户端、模型、工作区权限和地区条件都要分别核对。

## 地区资格

ChatGPT 登录看官方 ChatGPT 支持地区清单，API 路径看官方 API 支持地区清单，两份清单不能混用。安装成功、买了会员或换了客户端，都不等于获得地区资格。

## 切换账号 / 退出登录

```bash
codex logout
codex login
```

退出后重新登录，是处理“登录的不是预期账号”和多数 token 刷新问题的第一步，详见 [token 刷新失败与登录循环](token-refresh-login-loop.md)。

## 怎么选

- 个人日常写代码：ChatGPT 账号登录更省心，不用管按量计费。
- CI、脚本、团队共享：API Key，便于按项目设预算和权限。
- 两条路都配了：明确告诉自己当前用的是哪一条，排错时先跑 `codex login status`。

完整对比见 [Codex 用 ChatGPT 登录还是 API Key](https://nano-banana.lol/tutorials/codex-chatgpt-vs-api-key)。
