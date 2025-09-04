# gemini-2.5-flash-image API

本文介绍 `gemini-2.5-flash-image` 模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

以下只展示部分使用到的字段说明，Gemini API 详细字段见[Gemini 官网文档](https://ai.google.dev/gemini-api/docs/image-generation?hl=zh-cn)

## 请求参数

### 请求体

| 字段名                              | 类型   | 是否必须 | 默认值            | 描述                               |
| ----------------------------------- | ------ | -------- | ----------------- | ---------------------------------- |
| contents                            | array  | 必须     | -                 | 请求的内容，包含一个或多个部分。   |
| contents.role                       | string | 必须     | "user"            | 内容的角色，此处固定为 "user"。    |
| contents.parts                      | array  | 必须     | -                 | 内容的具体部分。                   |
| contents.parts.text                 | string | 必须     | -                 | 提示词文本。                       |
| generationConfig                    | object | 可选     | -                 | 生成配置。                         |
| generationConfig.responseModalities | array  | 可选     | ["TEXT", "IMAGE"] | 期望的响应形式，可以是文本或图像。 |

## 响应参数

| 字段名                                       | 类型      | 描述                                     |
| -------------------------------------------- | --------- | ---------------------------------------- |
| candidates                                   | `array`   | 返回的候选内容列表。                     |
| candidates.content                           | `object`  | 候选内容。                               |
| candidates.content.parts                     | `array`   | 内容的具体部分，可能包含文本和图像数据。 |
| candidates.content.parts.text                | `string`  | 模型返回的文本描述。                     |
| candidates.content.parts.inlineData          | `object`  | 内联的图像数据。                         |
| candidates.content.parts.inlineData.data     | `string`  | Base64 编码的图像数据。                  |
| candidates.content.parts.inlineData.mimeType | `string`  | 数据的 MIME 类型，例如 "image/png"。     |
| candidates.content.role                      | `string`  | 内容的角色，此处为 "model"。             |
| candidates.finishReason                      | `string`  | 生成结束的原因，例如 "STOP"。            |
| usageMetadata                                | `object`  | token 使用情况的元数据。                 |
| usageMetadata.candidatesTokenCount           | `integer` | 候选内容消耗的 token 数。                |
| usageMetadata.promptTokenCount               | `integer` | 提示词消耗的 token 数。                  |
| usageMetadata.totalTokenCount                | `integer` | 总共消耗的 token 数。                    |
| error                                        | `Object`  | 错误信息对象                             |
| error.code                                   | `string`  | 错误码                                   |
| error.message                                | `string`  | 错误提示信息                             |
| error.param                                  | `string`  | 请求 id                                  |

## 示例

### Gemini 兼容接口

`POST https://api.modelverse.cn/v1beta/models/gemini-2.5-flash-image:generateContent`

#### 同步请求

```bash
curl --location 'https://api.modelverse.cn/v1beta/models/gemini-2.5-flash-image:generateContent' \
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "contents": [
        {
            "role": "user",
            "parts": [
                {
                    "text": "Create a picture of a nano banana dish in a fancy restaurant with a Gemini theme"
                }
            ]
        }
    ],
    "generationConfig": {
        "responseModalities": [
            "TEXT",
            "IMAGE"
        ]
    }
}'
```

```python
import os
import requests
import json

api_key = os.getenv("API_KEY", "alk3hpXNq6byfuOkD1C9Ff3930654709BeEf5e5e975673B1")

url = "https://api.modelverse.cn/v1beta/models/gemini-2.5-flash-image:generateContent"

payload = json.dumps({
    "contents": [
        {
            "role": "user",
            "parts": [
                {
                    "text": "Create a picture of a nano banana dish in a fancy restaurant with a Gemini theme"
                }
            ]
        }
    ],
    "generationConfig": {
        "responseModalities": [s
            "TEXT",
            "IMAGE"
        ]
    }
})

headers = {
    'Authorization': f'Bearer {api_key}',
    'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.json())
```

### 响应

```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "That sounds incredibly unique! Here's a picture of a nano banana dish in a fancy restaurant with a Gemini theme:\n\n"
          },
          {
            "inlineData": {
              "data": "xxx",
              "mimeType": "image/png"
            }
          }
        ],
        "role": "model"
      },
      "finishReason": "STOP"
    }
  ],
  "usageMetadata": {
    "candidatesTokenCount": 1315,
    "candidatesTokensDetails": [
      {
        "modality": "IMAGE",
        "tokenCount": 1290
      },
      {
        "modality": "TEXT",
        "tokenCount": 25
      }
    ],
    "promptTokenCount": 16,
    "promptTokensDetails": [
      {
        "modality": "TEXT",
        "tokenCount": 16
      }
    ],
    "totalTokenCount": 1331,
    "trafficType": "ON_DEMAND"
  }
}
```

```json
{
  "error": {
    "message": "error_message",
    "type": "error_type",
    "param": "request_id",
    "code": "error_code"
  }
}
```
