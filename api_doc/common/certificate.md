# 认证鉴权

## 简介

ModelVerse API 使用 API Key 进行认证。所有 API 请求都必须在 HTTP 请求头中包含有效的 API Key。

## 获取 API Key

请访问 [ModelVerse 控制台](https://console.ucloud.cn/modelverse/experience/api-keys) 来创建和管理您的 API Key。

![](https://www-s.ucloud.cn/2025/03/a427b4a6c0ff2d4dc2f2ee3cdad95098_1743154241648.png)

## 将 API 密钥设置为环境变量

以下是 API 密钥在本地设置为环境变量 MODELVERSE_API_KEY 的方法。

```bash
export MODELVERSE_API_KEY=<YOUR_API_KEY_HERE>
```

### 请求示例

这是一个使用 `curl` 命令调用 API 的示例，展示了如何在请求头中加入 API Key：

```bash
curl --location 'https://api.modelverse.cn/v1/chat/completions' \
--header "Authorization: Bearer $MODELVERSE_API_KEY" \
--header 'Content-Type: application/json' \
--data '{
    "model": "deepseek-ai/DeepSeek-R1",
    "messages": [
        {
            "role": "user",
            "content": "say hello to ucloud"
        }
    ]
}'
```

请将 `$MODELVERSE_API_KEY` 替换为您在 ModelVerse 控制台中获取的实际 API Key。
