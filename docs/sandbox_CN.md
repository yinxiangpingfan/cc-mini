# 沙箱

> 此功能存在于官方 Claude Code 代码库中但尚未完全发布。cc-mini 实现并发布了它。

在 Linux 上使用 [bubblewrap (bwrap)](https://github.com/containers/bubblewrap) 隔离运行 Bash 命令。

## 工作原理

- 文件系统以**只读**方式挂载（`--ro-bind / /`）
- 只有当前工作目录**可写**（`--bind $CWD $CWD`）
- 网络默认**隔离**（`--unshare-net`）
- 配置文件受保护，无法修改
- PID 命名空间隔离（`--unshare-pid`）

## 模式

| 模式 | 行为 |
|------|----------|
| `auto-allow` | 沙箱开启，bash 自动批准 |
| `regular` | 沙箱开启，bash 需要确认 |
| `disabled` | 无沙箱（默认） |

## REPL 命令

```
> /sandbox                     # 交互式模式选择器
> /sandbox status              # 显示状态 + 依赖检查
> /sandbox mode auto-allow     # 启用并自动批准
> /sandbox mode disabled       # 禁用
> /sandbox exclude "docker *"  # 对匹配的命令跳过沙箱
```

## TOML 配置

```toml
[sandbox]
enabled = true
auto_allow_bash = true
excluded_commands = ["docker *", "npm run *"]
unshare_net = true

[sandbox.filesystem]
allow_write = ["."]
```

## 排除命令

模式支持：精确匹配（`"git"`）、前缀匹配（`"npm run"`）、通配符（`"docker *"`）。
被排除的命令仍需要正常的权限提示。

## 优雅降级

如果未安装 bwrap（非 Linux、Docker 环境），沙箱会自动禁用。使用 `/sandbox status` 检查。
