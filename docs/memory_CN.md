# KAIROS — 记忆系统

> 此功能存在于官方 Claude Code 代码库中但尚未完全发布。cc-mini 实现并发布了它。

助手可以跨会话记忆信息，并随时间自动整合记忆。

## 命令

| 命令 | 描述 |
|---------|-------------|
| `/remember <文本>` | 保存笔记到每日日志 |
| `/memory` | 显示当前记忆索引 |
| `/dream` | 手动将每日日志整合为组织好的主题文件 |

## 自动整合（Auto-Dream）

在以下情况后自动运行：
- 距上次整合 >= 24 小时
- 自上次整合以来 >= 5 个新会话

可配置：`--dream-interval`、`--dream-min-sessions`、`--no-auto-dream`

## 试一试

```bash
cc-mini --auto-approve
> /remember 我更喜欢 Python 而不是 JavaScript
> /remember 我们的项目使用 gRPC + PostgreSQL
> /dream                    # 整合到主题文件
> /memory                   # 验证索引

# 新会话 — 模型会回忆起你的偏好
cc-mini
> 你知道我的偏好吗？
```

数据存储在 `~/.mini-claude/`（记忆在 `memory/`，会话在 `sessions/`）。
