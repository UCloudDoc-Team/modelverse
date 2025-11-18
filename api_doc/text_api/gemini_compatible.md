# Gemini 快速开始

UModelverse 平台提供了与 Google Gemini API 兼容的 **Models** 接口，开发者可以使用 Gemini SDK 或其他支持的工具直接调用 Modelverse 上的 **Gemini 模型**。

本文将向您介绍如何快速在 UModelverse 平台发出您的第一个 Gemini API 请求。

## 快速开始

### 安装 Google GenAI SDK
安装 python 语言的 sdk

> 使用 Python 3.9 及更高版本，通过以下 pip 命令安装 google-genai 软件包：

```python
pip install google-genai==1.32
```

> **注意事项：**
> - UModelverse 平台目前仅兼容 `google-genai` **1.32** 版本的 Python SDK
> - 如需更好的兼容性，建议使用原生的 HTTP JSON 格式访问（如使用 curl 或其他 HTTP 客户端），兼容性更加稳定可靠


### 示例
以下示例使用 generateContent 方法，通过`gemini-2.5-flash`模型向 UModelverse API 发送请求。

> 请确保将 `$MODELVERSE_API_KEY` 替换为您自己的 API Key，获取 [API Key](https://console.ucloud.cn/modelverse/experience/api-keys)。


#### 非流式调用
您可以使用以下代码进行调用。请注意，我们需要通过 `http_options` 来指定 Modelverse 的 API 地址。

 <!-- tabs:start -->
#### ** python **

```python
from google import genai
from google.genai import types

client = genai.Client(
   api_key="<MODELVERSE_API_KEY>",
   http_options=types.HttpOptions(
       base_url="https://api.modelverse.cn"
   ),
)

response = client.models.generate_content(
   model="gemini-2.5-flash",
   contents=[
       {"text": "How does AI work?"},
   ],
   config=types.GenerateContentConfig(
       thinking_config=types.ThinkingConfig(thinking_budget=0),
   ),
)
print(response.text)
```
#### 参数说明：开启思考总结
详细内容可参考[官方文档](https://ai.google.dev/gemini-api/docs/thinking?hl=zh-cn#summaries)

如需开启思考总结，可在 `thinking_config` 中添加：

```python
config=types.GenerateContentConfig(
    thinking_config=types.ThinkingConfig(
        include_thoughts=True
    )
)
```
#### ** curl **

```bash
curl "https://api.modelverse.cn/v1beta/models/deepseek-ai/DeepSeek-V3.1:generateContent" \
    -H "x-goog-api-key: $MODELVERSE_API_KEY" \
    -H "Content-Type: application/json" \
    -X POST \
    -d '{
          "contents": [
            {
              "parts": [
                {
                  "text": "How does AI work?"
                }
              ]
            }
          ],
          "generationConfig": {
            "thinkingConfig": {
              "thinkingBudget": 0
            }
          }
        }'
```
<!-- tabs:end -->


#### 流式调用

 <!-- tabs:start -->
#### ** python **

```python
from google import genai
from google.genai import types

client = genai.Client(
    api_key="<MODELVERSE_API_KEY>",
    http_options=types.HttpOptions(
        base_url="https://api.modelverse.cn"
    ),
)

response = client.models.generate_content_stream(
    model="gemini-2.5-flash", contents=["Explain how AI works"]
)
for chunk in response:
    print(chunk.text, end="")

```

#### ** curl **

```bash
curl "https://api.modelverse.cn/v1beta/models/gemini-2.5-flash:GenerateContent?alt=sse" \
    -H "Authorization: Bearer $MODELVERSE_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
      "contents": [
        {
          "role": "user",
          "parts": [
            {
              "text": "Explain how AI works"
            }
          ]
        }
      ]
    }'
```
<!-- tabs:end -->

## 模型ID说明
更多受支持的gemini模型，请参考【获取模型列表】


> 更多字段详情，见[Gemini官方文档](https://ai.google.dev/api/models?hl=zh-cn)

