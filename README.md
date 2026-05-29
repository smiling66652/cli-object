# CLI 工具现代化升级项目

> **日期**: 2026-05-29  
> **状态**: P0 工具全部完成 | 使用场景分析完成  
> **作者**: 合肥工业大学学生 | BuddyOS 项目创始人

---

## 🎯 项目起源

在日常开发中，大量使用 `find`、`grep`、`pip install`、`node` 等 CLI 工具，体验老旧、速度慢、功能有限。决定对本地所有 CLI 工具进行一次**全量扫描**，然后基于全网调研，找到现代替代方案，完成升级。

## 📦 项目内容

| 阶段 | 内容 | 状态 |
|------|------|------|
| 1. 全量扫描 | 扫描本地所有 CLI 工具（PATH/npm/pip/cargo 等） | ✅ 完成 |
| 2. 全网调研 | 国内外论坛/博客/GitHub 搜索现代替代方案 | ✅ 完成 |
| 3. 安装升级 | P0 工具（uv/fd/rg/Bun/NuShell）全部安装 | ✅ 完成 |
| 4. 去重整合 | 功能重叠工具的 N 合 1 精简 | ✅ 完成 |
| 5. 功能测试 | A 基础 / B 功能 / C 集成 / D 压力测试 | ✅ A+B+C 完成 |
| 6. 使用场景 | 全网搜索典型工作流 + 个性化分析 | ✅ 完成 |
| 7. 文档交付 | 扫描报告、调研报告、场景分析、升级总结 | ✅ 完成 |

## 🔧 已安装的 P0 工具

| 工具 | 功能 | 版本 | 替代谁 | 提升 |
|------|------|------|--------|------|
| **ripgrep (rg)** | 极速文本搜索 | v15.1.0 | grep | 5-13x |
| **fd-find (fd)** | 极速文件查找 | v10.2.0 | find | 6-23x |
| **NuShell (nu)** | 结构化数据 Shell | v0.113.0 | cmd/bash | 新范式 |
| **Bun** | JS 运行时 + 包管理 | v1.3.14 | Node.js/npm | 3-4x |
| **uv** | Python 包管理 | v0.11.17 | pip/conda | 10-100x |

> Fish Shell 4.0.x 暂未安装（GitHub Releases 无 Windows 原生二进制），NuShell 作为现代 Shell 主力替代。

## 📁 项目文件

| 文件 | 说明 |
|------|------|
| `cli_tools.csv` | 全量扫描结果（CSV） |
| `CLI工具完整报告.md` | 扫描详细报告（102 个工具） |
| `现代CLI工具全量调研报告.md` | 全网调研报告（含方案对比） |
| `CLI工具使用场景与工作流大全.md` | 经典工作流 + 用户个性化场景 |
| `CLI工具现代化升级报告.md` | 项目总结报告（完整版） |

## 🚀 快速开始

### 安装 P0 工具（一键）

```bash
# Windows (WinGet)
winget install BurntSushi.ripgrep.MSVC
winget install sharkdp.fd
winget install Oven-sh.Bun
winget install Nushell.Nushell

# uv - 通过 PowerShell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 验证安装

```bash
rg --version      # ripgrep v15.1.0
fd --version      # fd v10.2.0
nu --version      # Nushell v0.113.0
bun --version     # Bun v1.3.14
uv --version      # uv v0.11.17
```

### 立即使用

```bash
# 替代 find
fd -e py                    # 找所有 Python 文件
fd --changed-within 1d      # 今天修改的文件

# 替代 grep
rg "关键词"                   # 递归搜索
rg -l "TODO" .               # 列出包含 TODO 的文件

# 替代 node/npm
bun run script.ts            # 直接运行 TypeScript
bun add package              # 比 npm 快 25x

# 替代 pip
uv pip install numpy         # 比 pip 快 10-100x
uv venv                      # 创建虚拟环境
```

## 🔗 发布命令

### 推送到 GitHub（需要先认证）

```bash
gh auth login
gh repo create cli-tools-modernization --public --source=. --push
```

### 安装为 WorkBuddy Skill

已在 `~/.workbuddy/skills/cli-tools-modernization/` 就绪，WorkBuddy 会自动发现并加载。

## ⚠️ 踩坑记录

| 问题 | 解决 |
|------|------|
| ripgrep 下载 404（9 字节） | URL 路径错误，通过 GitHub API 找到正确路径 |
| GitHub 终端无法连接 | VPN 仅代理浏览器，需设置 `HTTP_PROXY` 环境变量 |
| Fish Shell tarball 无用 | 4.0.x 无 Windows 原生二进制 |
| PowerShell 安全策略阻止 `iex` | 绕过，改用 WinGet 安装 |
| sandbox 文件访问被拒 | 将文件移到 workspace 目录 |

## 📜 许可证

MIT License
