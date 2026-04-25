# 配置

## API Keys

### Anthropic（默认）

```bash
export ANTHROPIC_API_KEY=sk-ant-...
export ANTHROPIC_BASE_URL=https://your-gateway.example.com  # 可选
```

### OpenAI 兼容

```bash
export CC_MINI_PROVIDER=openai
export OPENAI_API_KEY=sk-...
export OPENAI_BASE_URL=https://your-openai-gateway.example.com
```

### 环境变量

| 变量 | 描述 |
|----------|-------------|
| `CC_MINI_MODEL` | 模型名称（如 `claude-sonnet-4-5`） |
| `CC_MINI_MAX_TOKENS` | 最大输出 token |
| `CC_MINI_EFFORT` | 推理努力程度（`low`、`medium`、`high`） |
| `CC_MINI_PROVIDER` | `anthropic` 或 `openai` |
| `CC_MINI_BUDDY_MODEL` | 伙伴宠物反应的模型 |
| `CC_MINI_BUDDY_SEED` | 覆盖伙伴种子以获得特定伙伴 |

## CLI 标志

```bash
cc-mini \
  --provider anthropic \
  --base-url https://your-gateway.example.com \
  --api-key sk-ant-... \
  --model claude-sonnet-4 \
  --max-tokens 64000 \
  --auto-approve \
  --coordinator \
  --resume 1
```

## TOML 配置文件

按顺序加载（后者的配置覆盖前者）：

1. `~/.config/cc-mini/config.toml`
2. 当前工作目录下的 `.cc-mini.toml`

使用 `--config` 指定特定文件。

### Anthropic 示例

```toml
provider = "anthropic"

[anthropic]
api_key = "sk-ant-..."
base_url = "https://your-gateway.example.com"
model = "claude-sonnet-4"
```

### OpenAI 示例

```toml
provider = "openai"

[openai]
api_key = "sk-..."
base_url = "https://your-openai-gateway.example.com/v1"
model = "gpt-4.1-mini"
max_tokens = 8192
effort = "medium"
buddy_model = "gpt-4.1-mini"
```

### OpenRouter（低成本测试）

```toml
provider = "openai"

[openai]
api_key = "sk-or-..."
base_url = "https://openrouter.ai/api/v1"
model = "qwen/qwen3.6-plus-preview:free"
```

当 `provider = "openai"` 时，使用 `OPENAI_API_KEY` / `OPENAI_BASE_URL`。当 `provider = "anthropic"` 时，使用 `ANTHROPIC_API_KEY` / `ANTHROPIC_BASE_URL`。
