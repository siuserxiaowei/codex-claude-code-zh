# Codex CLI 安装（macOS / Linux / Windows / WSL2）

[← 返回目录](../README.md)

安装渠道**选一种就够了**。装完先确认当前终端实际调用的是哪一份 `codex`，再登录。

## macOS / Linux

**官方独立安装器（不需要 Node.js）**

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
codex --version
```

**npm**

```bash
node --version   # 建议使用仍在维护的 Node LTS
npm install -g @openai/codex
codex --version
```

注意包名是带 scope 的 `@openai/codex`。npm 上不带 scope 的 `codex` 是一个无关的老包，装错了不会报错，但根本不是这个工具。

**Homebrew**

```bash
brew install --cask codex
codex --version
```

部分环境里 cask 装上的是桌面 App 而不是 CLI。装完终端里没有 `codex` 命令，就改用官方安装器或 npm。

## Windows 原生

在新的 PowerShell 窗口运行：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
codex --version
```

也可以用 `winget install OpenAI.Codex`。`ByPass` 只作用于这一次官方命令，不需要永久修改执行策略；公司电脑被策略拦截时找 IT 走受管安装。

## WSL2

仓库、构建工具本来就在 Linux 里时选 WSL2：

1. 装好 WSL2 + Ubuntu。
2. **在 WSL 的 shell 里**按上面 macOS / Linux 的方式再装一次。Windows 里装过不代表 WSL 里也有。
3. 仓库放在 Linux 家目录（如 `~/code/my-app`），不要放 `/mnt/c/...`，跨文件系统明显更慢，还可能有权限和符号链接差异。

## 多来源冲突：先看实际调用的是哪一个

```bash
# macOS / Linux
command -v codex
```

```powershell
# Windows
Get-Command codex -All | Select-Object CommandType, Name, Source
```

| 原安装来源 | 更新方式 |
| --- | --- |
| npm | `npm install -g @openai/codex` |
| Homebrew | `brew upgrade --cask codex` |
| 官方安装器 | 重新运行同一条安装命令 |

不要为了“更新”把三种来源都装一遍。更新后**重开终端**再看 `codex --version`，版本没变多半是 PATH 里还有旧的一份。

## 装好后的三分钟验证

```bash
codex --version        # 有版本号 = 程序能跑
codex login status     # 看当前是哪种登录方式
codex doctor --summary # 若你的版本支持，检查安装和配置
cd your-project && codex
```

版本号能输出只说明程序装好了，不代表已经登录、账号有资格或有额度。下一步看 [登录：ChatGPT 账号 vs API Key](codex-login-auth.md)。

## 常见安装问题

- **EACCES / 权限不足**：用用户级 Node 管理工具（nvm、fnm 等）或改用官方安装器，不要 `sudo npm install -g` 反复覆盖。
- **EBADENGINE**：Node 版本不满足，先升级 Node 或改用不依赖 Node 的官方安装器。
- **PowerShell 里粘了 `export`**：PowerShell 用 `$env:NAME="value"`，Bash 语法不通用。

更多截图和历史实测记录见 [Codex CLI 安装与登录教程](https://nano-banana.lol/tutorials/codex-cli-install-login)。
