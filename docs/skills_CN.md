# 技能系统

技能是一键工作流。输入 `/名称`，AI 会运行完整的步骤序列。

## 内置技能

| 命令 | 功能 |
|---------|-------------|
| `/simplify` | 审查修改的代码，检查重复、质量、效率——**然后修复它** |
| `/review` | 审查代码变更并报告问题——**只读，不编辑** |
| `/commit` | 运行 `git add`，生成提交信息并提交 |
| `/test` | 检测测试框架，运行测试并分析失败 |

所有技能接受可选参数：

```
/simplify 关注安全性
/review 只检查 API 路由
/commit 修复登录页面样式
/test 只运行 test_auth.py
```

## 示例

```
> /review

Running skill: /review…
↳ Bash(git diff) …  ✓ done

## Code Review Report
### Warning
- fib_recursive() does not handle negative input
### Suggestion
- Consider adding @functools.lru_cache

> /simplify

Running skill: /simplify…
↳ Read(fib.py) …    ✓ done
↳ Edit(fib.py) …    ✓ done
Fixed: added negative check, type annotations, lru_cache...
```

## 自定义技能

**步骤 1**: 在 `.cc-mini/skills/` 下创建目录

```bash
mkdir -p .cc-mini/skills/deploy
```

**步骤 2**: 编写 `SKILL.md` 文件

```markdown
---
name: deploy
description: 部署到预发布环境
---

# Deploy

1. 运行 `git status` 检查未提交的变更
2. 运行 `./scripts/deploy.sh $ARGUMENTS`
3. 报告部署状态
```

`$ARGUMENTS` 会被替换为你在命令后输入的内容。

**步骤 3**: 使用它

```
> /deploy staging
Running skill: /deploy…
```

## 发现位置

| 位置 | 范围 |
|----------|-------|
| 内置 | 4 个捆绑技能，始终可用 |
| `~/.cc-mini/skills/` | 个人技能，所有项目 |
| `<project>/.cc-mini/skills/` | 项目技能，与团队共享 |

## SKILL.md Frontmatter

```markdown
---
name: deploy
description: 部署到预发布环境
context: fork          # fork = 隔离, inline = 在对话中（默认）
allowed-tools: Bash, Read
arguments: target
---
```
