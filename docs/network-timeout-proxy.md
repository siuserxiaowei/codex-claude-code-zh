# 超时、Reconnecting、stream disconnected

[← 返回目录](../README.md)

超时和断流**优先当成传输问题**，不要先当成“没买会员”或“额度没了”。断流本身不是余额证明。

## 30 秒分流

| 线索 | 优先排查 |
| --- | --- |
| 代理端口拒绝连接、超时，换网络后表现不同 | 本地网络 / 代理 |
| 用的是 socks5 地址，或刚改过代理 | 代理协议和端口是否匹配 |
| 多个客户端同时异常，官方状态页有事件 | 服务端事件，记录时间等官方更新 |
| 明确的 401 | 不是网络问题，去 [401 / 403](errors-401-403.md) |
| 明确的 429 | 不是网络问题，去 [429](errors-429-usage.md) |

## 命令行代理

CLI 一般读取标准代理环境变量。**变量值写 `http://`，不要把 SOCKS 端口当 HTTP 端口填**：

```bash
export HTTPS_PROXY="http://127.0.0.1:你的HTTP代理端口"
export HTTP_PROXY="http://127.0.0.1:你的HTTP代理端口"
codex
```

```powershell
$env:HTTPS_PROXY="http://127.0.0.1:你的HTTP代理端口"
$env:HTTP_PROXY="http://127.0.0.1:你的HTTP代理端口"
codex
```

浏览器能打开网页 ≠ 终端里的客户端链路正常。核对代理软件提供的 HTTP 入口和 SOCKS 入口各是哪个端口。

## macOS 桌面端从访达 / Dock 启动连不上

从终端启动正常、从访达启动不行，通常是图形界面进程读不到你在 shell 里 export 的变量，需要用 `launchctl setenv` 给 GUI 会话设代理。步骤见 [Codex Desktop launchctl 会话代理排查](https://nano-banana.lol/how-to/codex-desktop-proxy-launchctl)。

## 自查步骤

1. 同一条命令换一个网络（如手机热点）复测。
2. 检查系统代理、VPN、公司防火墙是否会中断 WebSocket / 长连接。
3. 配了自定义 Base URL：确认地址、TLS 证书和供应商状态页。
4. 区分“连不上”和“连上后中途断开”，后者常见于不稳定代理或网卡休眠。
5. 一次只改一个条件，用不含敏感数据的小请求复现，保留时间戳和脱敏日志。

**不要**为了绕过证书报错关闭 TLS 校验。

国内 HTTPS 代理的完整写法见 [Codex 国内 HTTPS 代理设置](https://nano-banana.lol/how-to/codex-https-proxy-china)。
