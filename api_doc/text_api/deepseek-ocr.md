# DeepSeek-OCR 模型

DeepSeek-OCR 是一款先进的 OCR 模型，能够识别图片中的文字并将其转换为指定的文本格式。

## 请求示例

您可以通过向 `https://api.modelverse.cn/v1/chat/completions` 端点发送请求来使用 DeepSeek-OCR 模型。

> **说明：**
> DeepSeek-OCR 支持 `max_tokens` 参数最大设置为 **8192**，目前该模型不收费，免费开放使用。


### 非流式请求

<!-- tabs:start -->
#### **cURL**
```bash
curl https://api.modelverse.cn/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -d '{
    "model": "deepseek-ai/DeepSeek-OCR",
    "messages": [
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "convert to markdown"
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "data:image/jpeg;base64,'$(openssl base64 -A -in ~/Downloads/ocr.png)'"
            }
          }
        ]
      }
    ]
  }'
```
#### **Python**
```python
import base64
import os
from openai import OpenAI

# Function to encode the image
def encode_image(image_path):
  with open(image_path, "rb") as image_file:
    return base64.b64encode(image_file.read()).decode('utf-8')

# Path to your image
image_path = os.path.expanduser("~/Downloads/ocr.png")

# Getting the base64 string
base64_image = encode_image(image_path)

client = OpenAI(
    api_key=os.getenv("MODELVERSE_API_KEY", "<YOUR_MODELVERSE_API_KEY>"),
    base_url="https://api.modelverse.cn/v1/",
)

response = client.chat.completions.create(
  model="deepseek-ai/DeepSeek-OCR",
  messages=[
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "convert to markdown"
        },
        {
          "type": "image_url",
          "image_url": {
            "url": f"data:image/jpeg;base64,{base64_image}"
          }
        }
      ]
    }
  ]
)

print(response.choices[0].message.content)
```
<!-- tabs:end -->

### 流式请求

通过将 `stream` 参数设置为 `true`，您可以实现流式响应。

<!-- tabs:start -->
#### **cURL**
```bash
curl https://api.modelverse.cn/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -d '{
    "model": "deepseek-ai/DeepSeek-OCR",
    "messages": [
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "convert to markdown"
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "data:image/jpeg;base64,'$(openssl base64 -A -in ~/Downloads/ocr.png)'"
            }
          }
        ]
      }
    ],
    "stream": true
  }'
```
#### **Python**
```python
import base64
import os
from openai import OpenAI

# Function to encode the image
def encode_image(image_path):
  with open(image_path, "rb") as image_file:
    return base64.b64encode(image_file.read()).decode('utf-8')

# Path to your image
image_path = os.path.expanduser("~/Downloads/ocr.png")

# Getting the base64 string
base64_image = encode_image(image_path)

client = OpenAI(
    api_key=os.getenv("MODELVERSE_API_KEY", "<YOUR_MODELVERSE_API_KEY>"),
    base_url="https://api.modelverse.cn/v1/",
)

stream = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-OCR",
    messages=[
        {
          "role": "user",
          "content": [
            {
              "type": "text",
              "text": "convert to markdown"
            },
            {
              "type": "image_url",
              "image_url": {
                "url": f"data:image/jpeg;base64,{base64_image}"
              }
            }
          ]
        }
    ],
    stream=True,
)

for chunk in stream:
    print(chunk.choices[0].delta.content or "", end="")
```
<!-- tabs:end -->
