# 快速开始

## 第一步：获取 API Key

如何获取 api_key 值：请进入 Umodelverse 控制台 -「体验中心」- [API Key 管理](https://console.ucloud.cn/modelverse/experience/api-keys) 进行快速创建。
![](https://www-s.ucloud.cn/2025/03/a427b4a6c0ff2d4dc2f2ee3cdad95098_1743154241648.png)
## 第二步：将 API 密钥设置为环境变量

<!-- 以下是 API 密钥在本地设置为环境变量 MODELVERSE_API_KEY 的方法。 -->

```bash
export MODELVERSE_API_KEY=<YOUR_API_KEY_HERE>
```

## 第三步：Chat API 调用

## 请求
## LLM model id list
| 厂商/系列         | 模型 ID |
|------------------|--------------------------------------------------|
| **OpenAI**       | openai/gpt-4o |
|                  | openai/gpt-5-nano |
|                  | openai/gpt-4.1 |
|                  | openai/gpt-5 |
|                  | openai/gpt-5-mini |
|                  | openai/gpt-oss-20b |
|                  | openai/gpt-oss-120b |
|                  | gpt-4.1-mini |
| **Anthropic Claude** | claude-opus-4-1 |
|                  | claude-4-opus |
|                  | claude-4-sonnet |
| **ByteDance 豆包** | ByteDance/doubao-1-5-pro-32k-250115 |
|                  | ByteDance/doubao-1-5-pro-256k-250115 |
|                  | ByteDance/doubao-seed-1.6 |
|                  | ByteDance/doubao-seed-1.6-thinking |
|                  | ByteDance/doubao-1.5-thinking-vision-pro |
| **DeepSeek**     | deepseek-ai/DeepSeek-V3.1 |
|                  | deepseek-ai/DeepSeek-R1-Distill-Llama-70B |
|                  | deepseek-ai/DeepSeek-R1-0528 |
|                  | deepseek-ai/DeepSeek-V3-0324 |
|                  | deepseek-ai/DeepSeek-Prover-V2-671B |
|                  | deepseek-ai/DeepSeek-R1 |
| **Qwen**         | Qwen/Qwen3-32B |
|                  | Qwen/Qwen3-30B-A3B |
|                  | Qwen/Qwen3-Coder |
|                  | Qwen/Qwen3-235B-A22B |
|                  | Qwen/QwQ-32B |
|                  | qwen/qwen2.5-vl-72b-instruct |
| **Google Gemini** | gemini-2.5-pro |
|                  | gemini-2.5-flash |
| **智谱 GLM**     | zai-org/glm-4.5v |
|                  | zai-org/glm-4.5 |
| **其他厂商**     | grok-4 |
|                  | moonshotai/Kimi-K2-Instruct |
|                  | baidu/ernie-x1-turbo-32k |
|                  | baidu/ernie-4.5-turbo-128k |
|                  | baidu/ernie-4.5-turbo-vl-32k |

### 请求示例

```bash
curl --location 'https://api.modelverse.cn/v1/chat/completions' \
--header "Authorization: Bearer $MODELVERSE_API_KEY" \
--header 'Content-Type: application/json' \
--data '{
    "stream": true,
    "model": "deepseek-ai/DeepSeek-R1",
    "messages": [
        {
            "role": "user",
            "content": "say hello to ucloud"
        }
    ]
}'
```

## 响应

### 响应示例

```json
{
    "id": "  ",
    "object": "chat.completion",
    "created":  ,
    "model": "deepseek-ai/DeepSeek-R1",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "\n\nHello, UCloud! 👋 If there's anything specific you'd like to know or discuss about UCloud's services (like cloud computing, storage, AI solutions, etc.), feel free to ask! 😊",
                "reasoning_content": "\nOkay, the user wants to say hello to UCloud. Let me start by greeting UCloud directly.\n\nHmm, should I mention what UCloud is? Maybe a brief intro would help, like it's a cloud service provider.\n\nThen, I can ask if there's anything specific the user needs help with regarding UCloud services.\n\nKeeping it friendly and open-ended makes sense for a helpful response.\n"
            },
            "finish_reason": "stop"
    ],
    "usage": {
        "prompt_tokens": 8,
        "completion_tokens": 129,
        "total_tokens": 137,
        "prompt_tokens_details": null,
        "completion_tokens_details": null
    },
    "system_fingerprint": ""
}
```
