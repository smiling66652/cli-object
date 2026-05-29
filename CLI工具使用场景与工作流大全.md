# CLI 工具使用场景与高效工作流大全

> 2026-05-29 | 基于全网调研 + 用户（机械工程学生/AI开发者/BuddyOS创始人）个性化定制

---

## 一、现代 CLI 工具矩阵（已安装）

| 工具 | 功能 | 版本 | 替代谁 |
|------|------|------|--------|
| **ripgrep (rg)** | 极速文本搜索 | v15.1.0 | grep |
| **fd-find (fd)** | 极速文件查找 | v10.2.0 | find |
| **NuShell (nu)** | 结构化数据 Shell | v0.113.0 | bash/powershell |
| **Bun** | JS 运行时/包管理 | v1.3.14 | node/npm（辅助） |
| **uv** | Python 包管理 | v0.11.17 | pip/conda |

> Fish Shell 暂未安装（4.0.x 无 Windows 原生二进制），NuShell 作为现代 Shell 主力。

---

## 二、经典 CLI 组合工作流

### 工作流 1：fd → fzf → bat 搜索查看链

```bash
# 找文件 → 交互选择 → 预览
fd -e py | fzf --preview 'bat --color=always {}'

# 纯 fd 批量预览（多文件同时查看）
fd -e rs -X bat

# 搜索 + 选择 + 执行（最常用）
fd -t f | fzf -m | xargs bat
```

**场景**：浏览大型代码库、检查文件内容、多文件对比

---

### 工作流 2：rg + fd 全量搜索引擎

```bash
# rg 搜索文本 → 获取文件列表 → 管道到下一个处理
rg -l "TODO" src/ | xargs bat

# fd 按时间过滤 → rg 搜索
fd --changed-within 1d -e py -x rg "def " {}

# rg 按文件类型搜索
rg --type-add 'doc:*.md' -t doc "关键概念"
```

**场景**：代码审查、查找最近改动、知识库全文搜索

---

### 工作流 3：NuShell 数据管道（结构化替代品）

```nu
# JSON 处理（无需 jq）
http get https://api.example.com/data | where status == "active" | select name email

# CSV 数据分析（无需 awk）
open sales.csv | where region == "North" | group-by product | each { ... }

# 文件系统操作（结构化）
ls | where size > 100MB | sort-by size | first 10
```

**场景**：API 数据处理、CSV 分析、文件系统批量操作

---

### 工作流 4：Bun 脚本自动化

```bash
# 写一次性脚本（比 Node.js 快 4x）
bun run scrape.ts     # TypeScript 直接运行
bun run process.ts    # 批处理

# 包管理（比 npm 快 25x）
bun add @org/package
bun install
```

**场景**：快速原型脚本、Web 抓取、构建工具

---

### 工作流 5：uv 极速 Python 环境

```bash
# 创建虚拟环境（比 venv 快 10-100x）
uv venv
uv pip install numpy pandas matplotlib

# 运行脚本
uv run script.py

# 全局工具安装
uv tool install ruff
```

**场景**：Python 项目管理、依赖安装、工具安装

---

## 三、高级多工具协作场景

### 场景 A：代码审计流水线

```bash
# 步骤1：fd 找到所有 Python 文件
# 步骤2：rg 搜索安全问题
# 步骤3：bat 预览问题代码
# 步骤4：管道输出到报告

fd -e py src/ \
  | xargs rg -l "password|secret|token" \
  | xargs bat --style=numbers,changes

# 生成审计报告
fd -e py -e js src/ \
  | xargs rg -n "TODO|FIXME|HACK" \
  > 代码问题清单.txt
```

**用户适用**：BuddyOS 代码质量检查、课程设计代码审查

---

### 场景 B：批量文件处理

```bash
# 步骤1：fd 找到所有目标文件
# 步骤2：xargs 并行处理
# 步骤3：rg 验证结果

# 批量重命名（jpg → webp）
fd -e jpg assets/ | sed 's/.jpg$/.webp/' | xargs -I{} convert {}.jpg {}

# 批量执行 Python 脚本
fd -e py tests/ -x python {}
```

**用户适用**：课程设计素材处理、SolidWorks 导出文件管理

---

### 场景 C：Git + 现代工具 开发流

```bash
# 查看最近修改的文件（带 Git 状态）
fd --changed-within 1d -t f

# 搜索 Git 历史中的内容
git log --all -p | rg "关键字"

# 批量查看 diff 文件
git diff --name-only HEAD~5 | xargs bat

# 查看历史版本内容
git show HEAD:main.py | bat -l python
```

**用户适用**：BuddyOS 开发迭代、课程设计版本管理

---

### 场景 D：NuShell 系统监控管道

```nu
# 一键系统状态报告
{
  cpu: (sys cpu | select brand usage),
  memory: {
    total: (sys mem | get total),
    free: (sys mem | get free),
    percent: ((sys mem | get used) / (sys mem | get total) * 100 | math round)
  },
  disk: (sys disks | select name mount free),
  top_processes: (ps | sort-by cpu | last 5 | select name cpu mem)
}

# 实时监控（循环）
loop { sys cpu | get usage; sleep 2sec }
```

**用户适用**：Y7000P 性能监控、Ollama 运行状态检查

---

### 场景 E：多 Agent 并行开发（Manager-Worker）

```
Manager CLI:     任务分解、进度监控、协调通信
Worker-1:        模块A开发
Worker-2:        模块B开发
Worker-3:        测试编写
Worker-4:        文档更新

通信方式: Git Worktree 隔离 + 共享状态文件
```

**用户适用**：BuddyOS v3.0 多模块并行开发、大创项目团队协作

---

## 四、用户个性化场景（机械工程 + AI 开发）

### 场景 1：课程设计文档自动化

```bash
# 搜索所有课程设计文档
fd -e docx -e pdf "D:\Mypc\桌面\课程设计" | sort

# 在文档中搜索关键公式
rg "斯特林|热效率|功率" "D:\Mypc\桌面\课程设计" -t docx

# 批量统计文档字数
fd -e docx -e md | xargs wc -m

# 用 NuShell 分析文档组织结构
ls "D:\Mypc\桌面\课程设计\" | where type == dir | grid
```

**价值**：快速定位内容、自动统计、批量检查格式规范

---

### 场景 2：SolidWorks 建模文件管理

```bash
# 找到所有 SolidWorks 文件并统计
fd -e sldprt -e sldasm -e slddrw "D:\Mypc" --stats

# 按修改时间排序（最近的建模文件）
fd -e sldprt --changed-within 7d -l

# 批量备份到指定目录
fd -e sldprt -e sldasm "D:\Mypc\桌面\课程设计" \
  -x cp {} "D:\Mypc\备份\SW文件\"

# 在 SW 文件目录搜索特定零件
rg "斯特林" --type-add 'sw:*.sldprt' -t sw
```

**价值**：文件归档、批量备份、项目结构可视化

---

### 场景 3：BuddyOS 项目开发流水线

```bash
# 代码统计
fd -e py "d:/Workbuddy/BuddyOS/src" -x wc -l {} | awk '{sum+=$1} END {print "总行数:", sum}'

# 搜索所有 TODO 标记
rg "TODO|FIXME" "d:/Workbuddy/BuddyOS" -g "*.py"

# 检查依赖是否过期
cd d:/Workbuddy/BuddyOS && uv pip list --outdated

# 批量格式化 Python 代码
fd -e py "d:/Workbuddy/BuddyOS/src" -x ruff format {}

# 一键运行诊断
fd diagnose.py "d:/Workbuddy/BuddyOS" -x uv run {}
```

**价值**：代码质量维护、依赖管理、自动化测试

---

### 场景 4：RAG 知识库构建辅助

```bash
# 找到所有 Markdown 文档
fd -e md "D:\AI环境" -l | sort

# 搜索文档中的技术关键词
rg "Ollama|RAG|ChromaDB|embedding" "D:\AI环境" -g "*.md"

# 统计知识库文档数量
fd -e md -e txt -e pdf | wc -l

# 批量生成文档摘要（结合 Python）
fd -e md | xargs uv run summarize.py
```

**价值**：知识库整理、快速定位技术文档、自动摘要

---

### 场景 5：Python COM SolidWorks MCP 开发

```bash
# 搜索 SolidWorks MCP 相关代码
rg "SW\.|SolidWorks|pywin32" "d:/Workbuddy/BuddyOS" -g "*.py"

# 快速测试 SW COM 连接
uv run -c "import win32com.client; sw = win32com.client.Dispatch('SldWorks.Application'); print('OK')"

# 批量生成建模脚本
fd -e py "SW_scripts/" -x uv run {}
```

**价值**：MCP 开发加速、COM 调试

---

### 场景 6：考研学习辅助

```bash
# 搜索笔记中的概念
rg "伯努利|熵|热力学" "D:\笔记" -g "*.md"

# 查找最近修改的学习笔记（最近复习的内容）
fd "D:\笔记" --changed-within 2d -e md -l

# 统计复习进度
fd -e md "D:\笔记" | wc -l

# 搜索思政/数学相关知识点
rg -l "极限|导数|积分" "D:\笔记\数学"
```

**价值**：笔记搜索、复习进度追踪

---

### 场景 7：AI 工具链一键检查

```nu
# NuShell 脚本：一键检查所有 AI 环境
let checks = [
  {name: "Ollama", cmd: "ollama list"},
  {name: "Python", cmd: "python --version"},
  {name: "Bun", cmd: "bun --version"},
  {name: "uv", cmd: "uv --version"},
  {name: "NuShell", cmd: "version"},
]

$checks | each { |c|
  try {
    let result = (run-external $c.cmd | complete)
    {tool: $c.name, status: "OK", detail: $result.stdout}
  } catch {
    {tool: $c.name, status: "MISSING", detail: ""}
  }
}
```

**价值**：快速诊断、环境健康检查

---

### 场景 8：文件清理与整理

```bash
# 找到大文件（>100MB）
fd -S +100M --changed-before 30d "D:\Mypc"

# 找到重复文件（按大小）
fd -t f "D:\Mypc\下载" | xargs md5sum | sort | uniq -w32 -d

# 清理临时文件（仅扫描，不删除）
fd "*.tmp|*.log|*.cache" "D:\" | head -50
```

**价值**：磁盘空间管理、下载文件夹整理

---

### 场景 9：多项目批量操作

```bash
# 批量更新所有 Python 项目依赖
fd requirements.txt "d:/Workbuddy" --max-depth 3 \
  -x sh -c 'cd {//} && uv pip install -r requirements.txt'

# 批量 git status 检查
fd ".git" "d:/Workbuddy" --max-depth 2 -t d \
  -x sh -c 'echo "=== {//} ===" && cd {//} && git status --short'

# 统计所有项目代码量
fd -t f -e py -e js -e ts -e rs "d:/Workbuddy" \
  -x wc -l {} | awk '{sum+=$1} END {print "总计:", sum, "行"}'
```

**价值**：多项目管理、进度统计

---

## 五、工作流集成建议

### 推荐工具绑定

```bash
# ~/.bashrc / ~/.config/nushell/config.nu 配置

# fd 替代 find
alias find='fd'

# rg 替代 grep  
alias grep='rg'

# bat 替代 cat
alias cat='bat --style=plain'

# uv 快速管理 Python
alias pip='uv pip'
alias venv='uv venv'

# FZF 默认使用 fd
export FZF_DEFAULT_COMMAND="fd --type f"
```

### 快捷操作映射

| 快捷键/别名 | 实际命令 | 场景 |
|-------------|---------|------|
| `ct` | `rg "关键词"` | 代码搜索 |
| `ff` | `fd -e py -e js` | 文件查找 |
| `recent` | `fd --changed-within 1d` | 今日变更 |
| `big` | `fd -S +50M` | 大文件 |
| `pyfind` | `fd -e py -x rg "TODO" {}` | Python TODO 搜索 |
| `sysinfo` | NuShell 系统监控 | 性能检查 |

---

## 六、与其他工具的对比

| 场景 | 旧方式 | 新方式 | 提升 |
|------|--------|--------|------|
| 文件搜索 | `find . -name "*.py"` | `fd -e py` | 6-23x 快 |
| 文本搜索 | `grep -r "keyword" .` | `rg "keyword"` | 5-13x 快 |
| JSON 处理 | `curl x | jq '.field'` | `http get x \| select field` (nu) | 可读性强 |
| Python 安装 | `pip install numpy` | `uv pip install numpy` | 10-100x 快 |
| JS 运行 | `node script.js` | `bun run script.ts` | 3-4x 快 |
| 文件浏览 | `cat file.py` | `bat file.py` | 语法高亮 |

---

## 七、快速上手路径

### 第1天：学会基础替换
- `fd` 代替 `find`：`fd -e py`、`fd --changed-within 1d`
- `rg` 代替 `grep`：`rg "关键词"`、`rg -l "TODO"`
- `bat` 代替 `cat`：`bat file.py`

### 第3天：管道组合
- `fd -e py | fzf --preview 'bat {}'`
- `rg -l "关键词" | xargs bat`

### 第7天：NuShell 结构化思维
- `ls | where size > 100MB | sort-by size`
- `open data.csv | where region == "North" | math avg --columns price`

### 第14天：多工具联动自动化
- 项目中批量操作脚本
- 系统监控一键报告
- Git + fd + rg 开发流形成肌肉记忆
