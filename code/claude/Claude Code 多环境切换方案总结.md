# Claude Code 多环境切换方案总结

## 配置目录结构

```
~/.claude-configs/
├── gpt2share/
│   ├── .claude.json
│   └── settings.json
└── zhipu/
    ├── .claude.json
    └── settings.json
```

## 两个环境的配置

### gpt2share 环境

文件：`~/.claude-configs/gpt2share/settings.json`

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "cr_ca05b3111c15833d3f9ab73fe197b07bc57f5bceca42c2318c021603df86d96d",
    "ANTHROPIC_BASE_URL": "http://45.153.246.116:5201/api"
  },
  "model": "opus"
}
```

### zhipu 环境

文件：`~/.claude-configs/zhipu/settings.json`

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "4d665454b890420bbb1e37e3c24bd28c.wthLaFGYlPomKpkE",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic"
  },
  "model": "glm-4.6"
}
```

## 切换函数

添加到 `~/.zshrc` 末尾：

```bash
# Claude Code 环境切换函数
claude-gpt2s() {
    echo "🔄 切换到 gpt2share 环境..."
    cp ~/.claude-configs/gpt2share/.claude.json ~/
    cp ~/.claude-configs/gpt2share/settings.json ~/.claude/
    echo "✅ 已切换到 gpt2share (http://45.153.246.116:5201/api)"
    command claude "$@"
}

claude-zhipu() {
    echo "🔄 切换到 zhipu 环境..."
    cp ~/.claude-configs/zhipu/.claude.json ~/
    cp ~/.claude-configs/zhipu/settings.json ~/.claude/
    echo "✅ 已切换到 zhipu (https://open.bigmodel.cn/api/anthropic)"
    command claude "$@"
}

# 默认 claude 使用 zhipu 环境
alias claude='claude-zhipu'
```

## 使用方式

| 命令 | 效果 |
|------|------|
| `claude` | 默认使用 zhipu 环境 |
| `claude-zhipu` | 显式使用 zhipu 环境 |
| `claude-gpt2s` | 使用 gpt2share 环境 |

## 原理

每次运行切换函数时：
1. 复制对应环境的 `.claude.json` 到 `~/`
2. 复制对应环境的 `settings.json` 到 `~/.claude/`
3. 启动 claude

MCP 配置是共享的，不需要每个环境单独配置。
