---
name: cli-tools-modernization
description: CLI工具现代化升级指南。一键扫描、升级、替换老旧CLI工具为现代替代品（rg/fd/nu/Bun/uv），含全网调研、使用场景分析和高效工作流。触发词：CLI升级、现代CLI工具、命令行优化、ripgrep、fd-find、NuShell、uv。
agent_created: true
---

# CLI 工具现代化升级 Skill

## 何时使用

当用户询问以下内容时，加载此 Skill：
- "我的 CLI 工具有哪些可以升级？"
- "用什么替代 grep/find/pip？"
- "有没有更快的命令行工具？"
- "帮我搭建现代 CLI 工作流"
- "CLI 工具扫描/调研/升级"

## 核心工具

| 工具 | 替代 | 安装命令（Windows WinGet） | 提升 |
|------|------|---------------------------|------|
| ripgrep (rg) | grep | `winget install BurntSushi.ripgrep.MSVC` | 5-13x |
| fd-find (fd) | find | `winget install sharkdp.fd` | 6-23x |
| NuShell (nu) | cmd/bash | `winget install Nushell.Nushell` | 结构化 |
| Bun | Node.js/npm | `winget install Oven-sh.Bun` | 3-4x |
| uv | pip/conda | `irm https://astral.sh/uv/install.ps1 \| iex` | 10-100x |

## 执行流程

### 步骤 1：扫描

```bash
# 扫描所有 CLI 工具来源
where * 2>/dev/null | head -200
npm list -g --depth=0 2>/dev/null
pip list 2>/dev/null
cargo install --list 2>/dev/null
```

### 步骤 2：安装（Windows）

```powershell
winget install BurntSushi.ripgrep.MSVC
winget install sharkdp.fd
winget install Nushell.Nushell
winget install Oven-sh.Bun
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 步骤 3：验证

```bash
rg --version && fd --version && nu --version && bun --version && uv --version
```

### 步骤 4：配置别名（~/.bashrc 或 ~/.config/nushell/config.nu）

```bash
alias find='fd'
alias grep='rg'
alias cat='bat --style=plain'  # 需要额外安装 bat
alias pip='uv pip'
```

## 推荐工作流

### 基础搜索流

```bash
# fd 找文件 → fzf 选择 → bat 预览
fd -e py | fzf --preview 'bat {}'

# rg 内容搜索
rg -l "TODO" src/ | xargs bat
```

### 数据处理流（NuShell）

```nu
# JSON API 处理
http get https://api.github.com/repos/nushell/nushell | select stargazers_count forks_count

# CSV 分析
open data.csv | where region == "North" | math avg --columns price
```

### Python 项目管理（uv）

```bash
uv venv && uv pip install -r requirements.txt && uv run main.py
```

### 批量操作

```bash
# 批量执行 Python 测试
fd -e py tests/ -x uv run {}

# 批量 git status
fd ".git" --max-depth 2 -t d -x sh -c 'cd {//} && git status --short'
```

## 已知问题与解决

| 问题 | 解决方案 |
|------|---------|
| GitHub 终端下载失败 | 设置代理：`export HTTPS_PROXY=http://127.0.0.1:7890` |
| Fish Shell 4.0.x 无 Windows 版 | 使用 NuShell 代替 |
| ripgrep WinGet 下载慢 | 手动从 GitHub Releases 下载 zip |
| PowerShell `iex` 被阻止 | 使用 WinGet 代替 |

## 相关文件

- `README.md` — 项目 README
- `cli_tools.csv` — 全量扫描结果
- `CLI工具完整报告.md` — 102 个工具扫描报告
- `现代CLI工具全量调研报告.md` — 全网调研
- `CLI工具使用场景与工作流大全.md` — 9 个使用场景
- `references/install-commands.md` — 安装命令详细参考
