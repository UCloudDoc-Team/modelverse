# Gemini 兼容 API 调用文档

UModelverse 平台提供了与 Google Gemini API 兼容的接口，开发者可以使用 Gemini SDK 或其他支持的工具直接调用 Modelverse 上的模型。

## 快速开始

您可以使用 `curl` 命令或 Gemini 客户端库来调用 Modelverse API。

### Curl 示例

#### 非流式调用

下面是一个使用 `curl` 调用 `generateContent` 接口的示例。请注意，这里的认证头使用的是 `Authorization: Bearer`，与 Gemini 原生的 `x-goog-api-key` 不同。

```bash
curl "https://api.modelverse.cn/v1beta/models/{model_name}:generateContent" \
    -H "Authorization: Bearer $MODELVERSE_API_KEY" \
    -H "Content-Type: application/json" \
    -X POST \
    -d '{
      "contents": [
        {
          "parts": [
            {
              "text": "hello"
            }
          ]
        }
      ]
    }'
```

#### 流式调用

下面是一个使用 `curl` 调用 `streamGenerateContent` 接口以实现流式返回的示例：

```bash
curl "https://api.modelverse.cn/v1beta/models/{model_name}:GenerateContent?alt=sse" \
    -H "Authorization: Bearer $MODELVERSE_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
      "contents": [
        {
          "role": "user",
          "parts": [
            {
              "text": "hello"
            }
          ]
        }
      ]
    }'
```

请确保将 `$MODELVERSE_API_KEY` 替换为您自己的 API Key。

### Python 示例

您需要先安装 `google-generativeai` 库：
```bash
pip install google-generativeai
```

然后，您可以使用以下代码进行调用。请注意，我们需要通过 `client_options` 来指定 Modelverse 的 API 地址。

```python
import google.generativeai as genai

# 配置 API Key 和自定义端点
genai.configure(
    api_key="YOUR_MODELVERSE_API_KEY",
    client_options={"api_endpoint": "api.modelverse.cn"}
)

# 创建模型实例
model = genai.GenerativeModel('{model_name}')

# 调用模型并获取非流式响应
response = model.generate_content("hello")
print(response.text)

# 调用模型并获取流式响应
response_stream = model.generate_content("hello", stream=True)
for chunk in response_stream:
  print(chunk.text)
```
