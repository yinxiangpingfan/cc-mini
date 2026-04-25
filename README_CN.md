<div align="center">

# cc-mini

**超轻量 AI Agent 脚手架**

**Agentic** &nbsp;·&nbsp; **可扩展** &nbsp;·&nbsp; **源自 Claude Code**
<br>

核心代码仅 `~1000 行 Python`

</div>

---

### **新功能: Buddy — 带自定义精灵的 AI 伙伴**

> 你的编码伙伴生活在终端中。输入 `/buddy` 孵化它。支持自定义 ASCII 物种——带上你自己的皮卡丘！

![自定义皮卡丘伙伴](../assets/buddy-pikachu.jpg)

[完整 Buddy 文档 &rarr;](buddy.md)

---

## 功能特性

### 核心功能

- **交互式 REPL** — 流式输出、命令历史、斜杠命令自动补全
- **Agentic 工具循环** — Claude 自主调用工具直到任务完成
- **9 个内置工具**: `Read`, `Edit`, `Write`, `Glob`, `Grep`, `Bash`, `AskUser`, `EnterPlanMode`, `ExitPlanMode`
- **计划模式** — 并行子代理在实现前探索代码库，具备权限隔离
- **权限系统** — 模式感知（默认/计划），读取自动批准，写入/bash 需确认
- **会话持久化** — 自动保存对话，`/resume` 继续上次会话
- **上下文压缩** — 接近 token 限制时自动压缩
- **兼容 Anthropic + OpenAI** — 支持任何兼容的 API 端点

### 高级功能（来自未发布的 Claude Code 特性）

| 功能 | 描述 | 文档 |
|---------|-------------|------|
| **协调器模式** | 后台工作进程，支持并行研究和实现 | [文档 &rarr;](coordinator.md) |
| **Buddy** | 电子宠物风格的 AI 伙伴，有个性、属性、心情和对话气泡 | [文档 &rarr;](buddy.md) |
| **KAIROS 记忆** | 跨会话记忆，自动整合 | [文档 &rarr;](memory.md) |
| **技能系统** | 一键工作流：`/review`, `/commit`, `/test`, `/simplify` | [文档 &rarr;](skills.md) |
| **沙箱** | 使用 Bubblewrap 隔离 bash 命令 | [文档 &rarr;](sandbox.md) |

---

## 快速开始

### 环境要求

- Python 3.10+（推荐 3.11+）
- [Anthropic](https://console.anthropic.com/) 或任何 OpenAI 兼容提供商的 API Key

### 安装

```bash
# 一行安装（推荐）
curl -fsSL https://raw.githubusercontent.com/e10nMa2k/cc-mini/main/install.sh | bash

# 或手动安装
git clone https://github.com/e10nMa2k/cc-mini.git
cd cc-mini
pip install -e ".[dev]"
```

### 设置 API Key

```bash
# Anthropic
export ANTHROPIC_API_KEY=sk-ant-...

# 或 OpenAI 兼容
export CC_MINI_PROVIDER=openai # 协议类型，不是供应商名称
# Azure AI Foundry 和其他 OpenAI 兼容网关仍使用 "openai"
#（不要将 provider 设置为 "foundry"、"bedrock" 等）
export OPENAI_API_KEY=sk-...
export OPENAI_BASE_URL=https://your-gateway.example.com/v1
export CC_MINI_MODEL=gpt-... # 可选，默认为 "gpt-5.1-codex"
```

### 运行

```bash
cc-mini                              # 交互式 REPL
cc-mini "what tests exist?"          # 单次提示
cc-mini -p "summarize this codebase" # 打印并退出
cc-mini --auto-approve               # 跳过权限确认
cc-mini --resume 1                   # 恢复之前的会话
cc-mini --coordinator                # 协调器模式
```

### 首次会话演示

```
cc-mini

> list all python files in this project
↳ Glob(**/*.py) ✓
Found 12 Python files...

> read engine.py and explain the tool loop
↳ Read(src/core/engine.py) ✓
The submit() method implements an agentic loop...

> /buddy
Hatching your companion...
✨ SHINY LEGENDARY DUCK
Glitch Quack hatched! ★★★★★

> /buddy mood
Glitch Quack's mood:
  Happy      ████████████████░░░░  65 (high)
  Bored      ██████████░░░░░░░░░░  50 (neutral)

> /review
Running skill: /review…
↳ Bash(git diff) … ✓ done
## Code Review: no issues found ✓
```

[完整配置文档 &rarr;](configuration.md)

---

## 工具

| 工具 | 描述 | 权限 |
|------|-------------|------------|
| `Read` | 读取文件内容 | 自动批准 |
| `Glob` | 按模式查找文件 | 自动批准 |
| `Grep` | 搜索文件内容 | 自动批准 |
| `Edit` | 编辑文件（字符串替换） | 需要确认 |
| `Write` | 写入/创建文件 | 需要确认 |
| `Bash` | 运行 shell 命令 | 需要确认 |
| `AskUser` | 向用户提问 | 自动批准 |
| `EnterPlanMode` | 进入计划模式 | 自动批准 |
| `ExitPlanMode` | 退出计划模式 | 自动批准 |

协调器模式额外提供：`Agent`（生成工作进程）、`SendMessage`（继续工作进程）、`TaskStop`（停止工作进程）。计划模式也使用 `Agent` 启动并行只读探索/计划子代理。详见[协调器文档](coordinator.md)。

---

## 数据路径

| 数据 | 路径 |
|------|------|
| 安装目录（源代码） | `~/.cc-mini/` |
| 会话 | `~/.config/cc-mini/sessions/` |
| 记忆（KAIROS） | `~/.config/cc-mini/memory/` |
| 计划 | `~/.config/cc-mini/plans/` |
| REPL 历史 | `~/.config/cc-mini/history` |
| 伙伴数据 | `~/.config/cc-mini/companion.json` |
| 用户技能 | `~/.cc-mini/skills/` |
| 项目技能 | `{cwd}/.cc-mini/skills/` |
| 项目配置 | `.cc-mini.toml` |

---

## 斜杠命令

| 命令 | 描述 |
|---------|-------------|
| `/help` | 显示所有可用命令 |
| `/compact` | 压缩对话上下文 |
| `/resume` | 恢复之前的会话 |
| `/history` | 列出保存的会话 |
| `/clear` | 清除对话，开始新会话 |
| `/skills` | 列出所有可用技能 |
| `/buddy` | 伙伴宠物 — 孵化、抚摸、属性、心情 |
| `/buddy help` | 显示所有 buddy 命令和玩法指南 |
| `/review` | 代码审查（技能） |
| `/commit` | Git 提交（技能） |
| `/test` | 运行测试（技能） |
| `/simplify` | 审查并修复代码（技能） |

输入 `/` 查看自动补全建议。

---

## 项目结构

```
src/
├── core/                  # 纯脚手架 — 引擎、LLM、配置
│   ├── engine.py          # 流式 API 循环 + 工具执行
│   ├── llm.py             # LLM 客户端（Anthropic + OpenAI）
│   ├── config.py          # 配置（CLI、环境变量、TOML）
│   ├── context.py         # 系统提示构建器
│   ├── tool.py            # 基础 Tool 协议 + ToolResult
│   ├── permissions.py     # 权限检查器
│   └── session.py         # 会话持久化
│
├── tools/                 # 工具实现（每个文件一个）
│   ├── bash.py            # Shell 命令执行
│   ├── file_read.py       # 读取文件
│   ├── file_edit.py       # 编辑文件（字符串替换）
│   ├── file_write.py      # 写入/创建文件
│   ├── glob_tool.py       # 按模式查找文件
│   ├── grep_tool.py       # 搜索文件内容
│   ├── ask_user.py        # 向用户提问
│   ├── plan_tools.py      # EnterPlanMode / ExitPlanMode
│   └── agent.py           # 协调器代理工具
│
├── features/              # 可插拔功能
│   ├── compact.py         # 上下文压缩
│   ├── coordinator.py     # 协调器模式
│   ├── worker_manager.py  # 后台工作进程生命周期
│   ├── cost_tracker.py    # Token 使用追踪
│   ├── memory.py          # KAIROS 记忆系统
│   ├── plan.py            # 计划模式逻辑
│   ├── skills.py          # 技能加载器和注册表
│   ├── skills_bundled.py  # 内置技能（review、commit、test、simplify）
│   └── sandbox/           # Bubblewrap 沙箱子系统
│
├── tui/                   # 终端 UI
│   ├── app.py             # CLI 入口点 + REPL
│   ├── query.py           # 查询提交 + 流式显示
│   ├── rendering.py       # Rich 控制台渲染
│   ├── prompt.py          # 输入提示
│   ├── input_parser.py    # 输入解析
│   ├── shell.py           # Shell 集成
│   └── keylistener.py     # Esc/Ctrl+C 检测
│
├── commands/              # 斜杠命令处理器
└── buddy/                 # AI 伙伴宠物系统
```

## 运行测试

```bash
pytest tests/ -v
pytest tests/ -v -k "not integration"  # 跳过 bwrap 测试
```

---

## 文档

| 主题 | 链接 |
|-------|------|
| 配置（API keys、TOML、CLI 标志） | [docs/configuration.md](configuration.md) |
| Buddy（AI 伙伴宠物） | [docs/buddy.md](buddy.md) |
| 协调器模式（后台工作进程） | [docs/coordinator.md](coordinator.md) |
| KAIROS 记忆系统 | [docs/memory.md](memory.md) |
| 技能（自定义工作流） | [docs/skills.md](skills.md) |
| 沙箱（bash 隔离） | [docs/sandbox.md](sandbox.md) |
