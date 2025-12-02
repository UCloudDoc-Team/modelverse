## Claude Code 接入指南

> 现在你可以通过 [UModelverse 平台](https://console.ucloud.cn/modelverse/model-center) 使用 `claude-4-sonnet`、`claude-4-opus` 等模型接入 Claude Code。

## 🚀 快速入门

### 1. 安装

1. 请确保您已安装 npm，请参考 [Node.js 官方网站](https://nodejs.org/zh-cn/download)。

2. 安装 [Claude Code](https://docs.anthropic.com/en/docs/claude-code/quickstart)：

```shell
npm install -g @anthropic-ai/claude-code
```

### 2. 配置

Claude Code 支持通过环境变量配置自定义 API 端点。

#### 2.1 环境变量配置

在 Mac 或 Linux 环境下，将以下内容添加到您的 `~/.bashrc` 或 `~/.zshrc` 文件中：

```bash
export ANTHROPIC_BASE_URL="https://api.modelverse.cn"
export ANTHROPIC_API_KEY="your-umodelverse-api-key"
```

配置完成后，执行以下命令使配置生效：

```bash
source ~/.bashrc  # 或 source ~/.zshrc
```

#### 2.2 临时配置（单次运行）

如果您只想临时使用，可以在运行时直接设置环境变量：

```bash
ANTHROPIC_BASE_URL="https://api.modelverse.cn" ANTHROPIC_API_KEY="your-umodelverse-api-key" claude
```

### 3. 使用 Claude Code

配置完成后，您可以直接在终端中运行 Claude Code：

```shell
claude
```

#### 常用命令

- `claude` - 启动交互式 REPL
- `claude "your question"` - 直接提问
- `claude -p "your prompt"` - 使用 print 模式（非交互式）
- `claude -c` - 继续上一次对话

#### 在 VS Code 中使用

1. 安装 Claude Code VS Code 扩展
2. 确保环境变量已正确配置
3. 在 VS Code 中使用 `Ctrl+Shift+P`（Mac: `Cmd+Shift+P`）打开命令面板
4. 搜索并选择 "Claude Code" 相关命令

### 4. 模型选择

在 Claude Code 中，您可以通过 `/model` 命令切换模型：

```
/model claude-4-sonnet
```

支持的模型包括：
- `claude-4-sonnet` - 平衡性能与成本
- `claude-4-opus` - 最强推理能力

### 5. 高级配置

#### 5.1 配置文件

Claude Code 支持通过配置文件进行更细粒度的控制。创建 `~/.claude/settings.json`：

```json
{
  "permissions": {
    "allow": [
      "Bash(npm install)",
      "Bash(npm run)",
      "Read(*)",
      "Write(*)"
    ],
    "deny": []
  }
}
```

#### 5.2 项目级配置

在项目根目录创建 `.claude/settings.json` 可以为特定项目配置权限和行为。

## 常见问题

### Q: 遇到认证错误怎么办？

确保您的 API Key 正确配置：

```bash
echo $ANTHROPIC_API_KEY
echo $ANTHROPIC_BASE_URL
```

### Q: 如何查看当前使用的模型？

在 Claude Code 交互界面中输入 `/model` 即可查看当前模型。

### Q: 支持哪些功能？

通过 UModelverse 接入的 Claude Code 支持：
- ✅ 代码生成与编辑
- ✅ 文件读写操作
- ✅ 终端命令执行
- ✅ 多轮对话
- ✅ 上下文理解

## 相关链接

- [UModelverse API Key 获取](https://console.ucloud.cn/modelverse/experience/api-keys)
- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code)
- [Anthropic API 文档](https://docs.anthropic.com/en/api/messages)
