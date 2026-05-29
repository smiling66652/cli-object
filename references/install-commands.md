# 安装命令参考

## P0 工具安装命令（已验证）

### ripgrep (rg) v15.1.0
```powershell
# 通过 WinGet（推荐）
winget install BurntSushi.ripgrep.MSVC

# 手动下载（需要 VPN）
# URL: https://github.com/BurntSushi/ripgrep/releases/download/15.1.0/ripgrep-15.1.0-x86_64-pc-windows-msvc.zip
# 解压后将 rg.exe 放到 ~/.cargo/bin/
```

### fd-find (fd) v10.2.0
```powershell
# 通过 WinGet
winget install sharkdp.fd

# 或通过 cargo（需要 Rust）
cargo install fd-find
```

### NuShell v0.113.0
```powershell
# 通过 WinGet
winget install Nushell.Nushell

# 路径: C:\Users\%USERNAME%\AppData\Local\Programs\nu\bin\nu.exe
```

### Bun v1.3.14
```powershell
# 通过 WinGet
winget install Oven-sh.Bun

# 路径: C:\Users\%USERNAME%\AppData\Local\Microsoft\WinGet\Packages\Oven-sh.Bun_Microsoft.WinGet.Source_8wekyb3d8bbwe\
```

### uv v0.11.17
```powershell
# 通过 PowerShell 安装脚本（需要 VPN 或代理）
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 或通过 pip
pip install uv
```

## 代理配置（终端访问外网）

```powershell
# 设置代理环境变量（假设代理端口 7890，Clash 默认）
$env:HTTP_PROXY = "http://127.0.0.1:7890"
$env:HTTPS_PROXY = "http://127.0.0.1:7890"

# 或者在 Git Bash 中
export HTTP_PROXY="http://127.0.0.1:7890"
export HTTPS_PROXY="http://127.0.0.1:7890"
```

## 验证命令

```bash
# 一键验证所有 P0 工具
rg --version && fd --version && nu --version && bun --version && uv --version
```

## Fish Shell 注意事项

Fish Shell 4.0.x **没有 Windows 原生二进制**，GitHub Releases 提供的文件：
- `fish-4.0.1.app.zip` → macOS 应用
- `fish-4.0.1.pkg` → macOS 安装包
- `fish-static-x86_64-4.0.1.tar.xz` → Linux 静态二进制
- `fish-4.0.1.tar.xz` → 源码

**替代方案**：使用 NuShell（已安装），功能类似
**备选方案**：通过 MSYS2 `pacman -S fish` 或 WSL `apt install fish` 使用 Fish
