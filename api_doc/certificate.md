# 认证鉴权

## 简介
ModelVerse API 使用 API Key 进行认证。所有API请求都必须在HTTP请求头中包含有效的API Key。

## 获取 API Key
请访问 [ModelVerse 控制台](https://console.ucloud.cn/modelverse/experience/api-keys) 来创建和管理您的API Key。

![](https://www-s.ucloud.cn/2025/03/a427b4a6c0ff2d4dc2f2ee3cdad95098_1743154241648.png)

## 使用 API Key
API Key 需要放在 HTTP 请求的 `Authorization` 头中。

### 请求头
| 名称            | 类型   | 是否必须 | 描述                                              |
| --------------- | ------ | -------- | ------------------------------------------------- |
| `Authorization` | string | 是       | 使用 `Bearer <YOUR_API_KEY>` 的格式传入 API Key。 |
| `Content-Type`  | string | 是       | 固定值为 `application/json`。                     |

### 请求示例
这是一个使用 `curl` 命令调用 API 的示例，展示了如何在请求头中加入 API Key：

```bash
curl --location 'https://deepseek.modelverse.cn/v1/chat/completions' \
--header 'Authorization: Bearer <你的API Key>' \
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

请将 `<你的API Key>` 替换为您在 ModelVerse 控制台中获取的实际 API Key。
