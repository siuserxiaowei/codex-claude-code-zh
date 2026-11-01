# 429：限流、额度、余额分开看

[← 返回目录](../README.md)

同样是 429，至少有三种完全不同的“用完了”。只看到“429”三个数字时，信息还不够，先找完整消息里的 `error.code` / `error.type`。

## 先分清是哪一种

| 提示更像 | 含义 | 下一步 |
| --- | --- | --- |
| rate limit / Too Many Requests / slow_down | 请求太密或并发太高 | 降并发；有 Retry-After 就按它等；检查是否开了无上限自动重试 |
| usage limit reached / 用量已达上限 | ChatGPT 套餐内的 Codex 用量触顶 | 看本人用量面板的受限项和恢复时间 |
| `insufficient_quota` / `credit_balance_exhausted` | API 预付费余额耗尽 | 到正确组织的 API Billing 查余额 |
| `project_spend_limit_exceeded` / `organization_spend_limit_exceeded` | 项目或组织支出上限 | 请负责人调整预算；单纯充值可能仍过不去 |

## ChatGPT 登录路径：usage limit reached

1. 保存脱敏后的报错原文。
2. CLI 会话里用 `/status`，或去账号用量页，记下**受限项、恢复时间和时区**。没显示就写“未显示”，**不要从首次使用时间自己推算**。
3. 订阅有效期和用量是两份记录：会员没到期 ≠ 现在有用量。
4. 再决定：等恢复 / 在官方入口按资格购买额外用量 / 评估是否换档。付款动作本身不能当作“已经重置”的证据。

官方当前对各档位的限制方式（例如是否有短窗口限制、周限制）会调整，以官方定价与用量说明为准，不要沿用旧截图。

## API Key 路径

API 有自己的速率、配额和账单限制，不是无限兜底。按项目查用量与预算，买 ChatGPT 会员不会增加 API 余额。

## 频繁 429 的省用量办法

- 缩小任务上下文，一次只让它处理一个明确目标。
- 减少并行任务，关掉不必要的自动重试。
- 给出精确的文件路径和期望输出，模糊提问消耗更多。

恢复时间规则和记录模板见 [Codex 额度重置规则](https://nano-banana.lol/how-to/codex-usage-reset-rules) 与 [Codex 提示 usage limit reached 怎么办](https://nano-banana.lol/blog/codex-usage-limit-reached-fix-2026)。
