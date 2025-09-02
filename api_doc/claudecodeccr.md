## Claude Code x Claude Code Router 接入指南


> 现在你可以通过[UModelverse平台](https://console.ucloud.cn/modelverse/model-center)使用`GLM-4.5`、`Kimi-K2`、`Qwen3-Coder-480B-A35B`、`DeepSeek v3.1`等模型。          


## 🚀 快速入门

### 1. 安装

首先，请确保您已安装 [Claude Code](https://docs.anthropic.com/en/docs/claude-code/quickstart)：

```shell
npm install -g @anthropic-ai/claude-code
```

然后，安装 Claude Code Router：

```shell
npm install -g @musistudio/claude-code-router
```

### 2. 配置

创建并配置您的 `~/.claude-code-router/config.json` 文件。

`config.json` 文件有几个关键部分：
- **`PROXY_URL`** (可选): 您可以为 API 请求设置代理，例如：`"PROXY_URL": "http://127.0.0.1:7890"`。
- **`LOG`** (可选): 您可以通过将其设置为 `true` 来启用日志记录。当设置为 `false` 时，将不会创建日志文件。默认值为 `true`。
- **`LOG_LEVEL`** (可选): 设置日志级别。可用选项包括：`"fatal"`、`"error"`、`"warn"`、`"info"`、`"debug"`、`"trace"`。默认值为 `"debug"`。
- **日志系统**: Claude Code Router 使用两个独立的日志系统：
  - **服务器级别日志**: HTTP 请求、API 调用和服务器事件使用 pino 记录在 `~/.claude-code-router/logs/` 目录中，文件名类似于 `ccr-*.log`
  - **应用程序级别日志**: 路由决策和业务逻辑事件记录在 `~/.claude-code-router/claude-code-router.log` 文件中
- **`APIKEY`** (可选): 您可以设置一个密钥来进行身份验证。设置后，客户端请求必须在 `Authorization` 请求头 (例如, `Bearer your-secret-key`) 或 `x-api-key` 请求头中提供此密钥。例如：`"APIKEY": "your-secret-key"`。
- **`HOST`** (可选): 您可以设置服务的主机地址。如果未设置 `APIKEY`，出于安全考虑，主机地址将强制设置为 `127.0.0.1`，以防止未经授权的访问。例如：`"HOST": "0.0.0.0"`。
- **`NON_INTERACTIVE_MODE`** (可选): 当设置为 `true` 时，启用与非交互式环境（如 GitHub Actions、Docker 容器或其他 CI/CD 系统）的兼容性。这会设置适当的环境变量（`CI=true`、`FORCE_COLOR=0` 等）并配置 stdin 处理，以防止进程在自动化环境中挂起。例如：`"NON_INTERACTIVE_MODE": true`。
- **`Providers`**: 用于配置不同的模型提供商。
- **`Router`**: 用于设置路由规则。`default` 指定默认模型，如果未配置其他路由，则该模型将用于所有请求。
- **`API_TIMEOUT_MS`**: API 请求超时时间，单位为毫秒。

这是一个综合示例：

```json
{
  "APIKEY": "your-ccr-secret-key", // 这里填写您的ccr的 api-key，用于WEB页面 ccr ui 的访问 
  "PROXY_URL": "",
  "LOG": false,
  "API_TIMEOUT_MS": 600000,
  "NON_INTERACTIVE_MODE": false,
  "Providers": [
    {
      "name": "ucloud",
      "api_base_url": "https://api.modelverse.cn/v1/chat/completions",
      "api_key": "sk-your-umodelverse-api-key", // 填写您的UModelverse的api-key
      "models": [
        "openai/gpt-4.1",
        "deepseek-ai/DeepSeek-V3.1",
        "Qwen/Qwen3-32B",
        "openai/gpt-5",
        "zai-org/glm-4.5v",
        "openai/gpt-5-mini",
        "Qwen/Qwen3-30B-A3B",
        "ByteDance/doubao-seed-1.6",
        "openai/gpt-oss-20b",
        "openai/gpt-oss-120b",
        "grok-4",
        "gpt-4.1-mini",
        "claude-4-opus",
        "gemini-2.5-pro",
        "claude-4-sonnet",
        "gemini-2.5-flash",
        "ByteDance/doubao-seed-1.6-thinking",
        "ByteDance/doubao-1.5-thinking-vision-pro",
        "deepseek-ai/DeepSeek-R1-Distill-Llama-70B",
        "zai-org/glm-4.5",
        "Qwen/Qwen3-Coder",
        "moonshotai/Kimi-K2-Instruct",
        "deepseek-ai/DeepSeek-R1-0528",
        "deepseek-ai/DeepSeek-V3-0324",
        "Qwen/Qwen3-235B-A22B",
        "Qwen/QwQ-32B",
        "deepseek-ai/DeepSeek-Prover-V2-671B",
        "qwen/qwen2.5-vl-72b-instruct",
        "deepseek-ai/DeepSeek-R1"
      ]
    }
  ],
  "Router": {
    "default": "ucloud,claude-4-sonnet",
    "background": "ucloud,claude-4-sonnet",
    "think": "ucloud,claude-4-sonnet",
    "longContext": "ucloud,claude-4-sonnet",
    "longContextThreshold": 60000,
    "webSearch": "ucloud,claude-4-sonnet"
  }
}
```


### 3. 使用 Router 运行 Claude Code

使用 router 启动 Claude Code：

```shell
ccr code
```

> **注意**: 
> 如果您是第一次安装 claude code, 一直弹出登录claude官方网站的页面，您可以先执行 ` ANTHROPIC_AUTH_TOKEN=token claude`, token 可以随意填写，用于避免登录官方账号。
> 
> 修改配置文件后，需要重启服务使配置生效：
> ```shell
> ccr restart
> ```

### 4. UI 模式

为了获得更直观的体验，您可以使用 UI 模式来管理您的配置：

```shell
ccr ui
```

这将打开一个基于 Web 的界面，您可以在其中轻松查看和编辑您的 `config.json` 文件。

#### Router

`Router` 对象定义了在不同场景下使用哪个模型：

-   `default`: 用于常规任务的默认模型。
-   `background`: 用于后台任务的模型。这可以是一个较小的本地模型以节省成本。
-   `think`: 用于推理密集型任务（如计划模式）的模型。
-   `longContext`: 用于处理长上下文（例如，> 60K 令牌）的模型。
-   `longContextThreshold` (可选): 触发长上下文模型的令牌数阈值。如果未指定，默认为 60000。
-   `webSearch`: 用于处理网络搜索任务，需要模型本身支持。如果使用`openrouter`需要在模型后面加上`:online`后缀。


您还可以使用 `/model` 命令在 Claude Code 中动态切换模型：
`/model provider_name,model_name`
示例: `/model ucloud,claude-4-sonnet`


