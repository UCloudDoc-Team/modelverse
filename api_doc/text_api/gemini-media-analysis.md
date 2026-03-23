# Gemini 媒体分析功能和上传文件到`GCS` API

本文档描述通过 ModelVerse 网关调用 Gemini 的媒体分析功能和上传文件到GCS，输入可为图片、视频、音频。
[Gemini官方文档](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/multimodal/image-understanding?hl=zh-cn)
---

## 认证说明

所有接口支持以下任一方式携带 API Key（示例中仅使用脱敏占位符）：

- `Authorization: Bearer <your_api_key>`
- `x-goog-api-key: <your_api_key>`

Base URL 示例：`https://api.modelverse.cn`（海外可用 `https://api.umodelverse.ai`）。


---

## 流程概览

1. 上传媒体文件到GCS平台：`POST /v1/files`
2. 从上传响应中获取文件地址（`s3_url`）
3. 调用 Gemini 媒体分析接口：`POST /v1beta/models/{model}:generateContent`
4. 从响应中提取文本分析结果

---

## 1. 上传媒体文件

将图片、视频或音频文件上传到平台，获取可在后续请求中引用的 `s3_url`。

**请求**

- **方法 / 路径**：`POST /v1/files`
- **Content-Type**：`multipart/form-data`
- **表单字段**：
  - `file`：媒体文件（二进制）
  - `purpose`：必选；（`batch:gcs`）

**示例**

```bash
curl https://api.modelverse.cn/v1/files \
  -H "Authorization: Bearer <$MODELVERSE_API_KEY>" \
  -F purpose="batch:gcs" \
  -F "file=@gemini-3.1-pro-preview.jpg" 
```

**成功返回示例**

```json
{
  "id": "1773236148797418904_demo-image.jpg",
  "object": "file",
  "bytes": 224123,
  "created_at": 1773236150,
  "expires_at": 0,
  "filename": "1773236148797418904_demo-image.jpg",
  "purpose": "",
  "s3_url": "https://example-file-host/modelverse/1773236148797418904_demo-image.jpg"
}
```

后续媒体分析请求可使用 `s3_url` 作为 `file_data.file_uri`。

**异常返回示例**
```json
{
  "error": {
    "message": "this is error meesage",
    "type": "internal_error",
    "param": "e4b4cf3f-24a0-4620-b53e-fee919c9d914",
    "code": "model_server_error"
  }
} 
```
---

## 2. 调用 Gemini 媒体分析接口

**请求**

- **方法 / 路径**：`POST /v1beta/models/{model}:generateContent`
- **Content-Type**：`application/json`

`contents[].parts[]` 常见字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `text` | string | 问题或指令文本 |
| `file_data.mime_type` | string | 媒体 MIME，例如 `image/jpeg`、`video/mp4`、`audio/mpeg` |
| `file_data.file_uri` | string | 媒体地址，建议使用上传接口返回的 `s3_url` |

---

## 3. 示例：文本分析

```bash
IMAGE_URL="https://example-file-host/modelverse/1773236148797418904_demo-image.jpg"

curl -X POST "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
  -H "x-goog-api-key: $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
  "contents": [
    {
      "parts": [
        {
          "fileData": {
            "fileUri": "https://storage.googleapis.com/gemini-batch-001/1774000031757082682_gemini-3-flash-preview.jsonl?Expires=1774604833&GoogleAccessId=bucket%40stacyzhang0708-1218-01.iam.gserviceaccount.com&Signature=c4QsHdf89aWeTNX0JzVqT%2BL4VtM2%2Bx7PEpjjNSVU%2FBNjrvJB2kKbpbIWmKvexQvOA4BO9no0KbZxazCvAQbNttK4Ecng7Za4K4FslNLBJ7fucapF7xdE0b%2BSO690ph%2BXWX5erZJixmzV%2BhiGZay0mBdYAxOfVwZb%2Bc5RA%2FQzPqBsdbdKb1%2FzzD4AcwCHu0w0So54JMQt3qcDQVi56NZaOZA4aC9SLZGcVdYakMF75XKn9Z99e2rXgFtEo2P7%2BuRBY8hD%2Bv%2B6dHC9k6fa9nABZEw38wi3Ozfsf7eQ0PmTmrQ2ue6n97zMjbg6rSn2RmKFWc4iWBl48ikkyFmA1GhKsA%3D%3D",
            "mimeType": "text/plain"
          }
        },
        {
          "text": "What is in the text?"
        }
      ],
      "role": "user"
    }
  ]
}
'
```

---

## 4. 示例：视频分析

```bash
VIDEO_URL="https://example-file-host/modelverse/1773236148797418904_demo-video.mp4"

curl -X POST "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [
      {
        "role": "user",
        "parts": [
          {
            "file_data": {
              "mime_type": "video/mp4",
              "file_uri": "'"$VIDEO_URL"'"
            }
          },
          {
            "text": "请总结这个视频的主要事件，并按时间顺序列出关键片段。"
          }
        ]
      }
    ]
  }'
```

---

## 5. 示例：音频分析

```bash
AUDIO_URL="https://example-file-host/modelverse/1773236148797418904_demo-audio.mp3"

curl -X POST "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
  -H "x-goog-api-key: $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [
      {
        "role": "user",
        "parts": [
          {
            "file_data": {
              "mime_type": "audio/mpeg",
              "file_uri": "'"$AUDIO_URL"'"
            }
          },
          {
            "text": "请转写这段音频，并总结其核心观点。"
          }
        ]
      }
    ]
  }'
```

---

## 6. SDK Python

以下示例使用 Python 完成：

1. 上传媒体文件到 `/v1/files`
2. 使用返回的 `s3_url` 调用 `/v1beta/models/{model}:generateContent`

```python
from google import genai
from google.genai.types import   HttpOptions,Part

client = genai.Client(
    http_options=HttpOptions(
        api_version="v1beta",
        base_url="https://api.modelverse.cn",
    ),
    api_key='<$MODELVERSE_API_KEY>', 
)

prompt = "Describe the key events in this video, providing both audio and visual details. Include timestamps for salient moments."
response = client.models.generate_content(
    model="gemini-3-flash-preview",
    contents=[
        Part.from_uri(
            file_uri="https://storage.googleapis.com/gemini-batch-001/1774000031757082682_gemini-3-flash-preview.jsonl?Expires=1774604833&GoogleAccessId=bucket%40stacyzhang0708-1218-01.iam.gserviceaccount.com&Signature=c4QsHdf89aWeTNX0JzVqT%2BL4VtM2%2Bx7PEpjjNSVU%2FBNjrvJB2kKbpbIWmKvexQvOA4BO9no0KbZxazCvAQbNttK4Ecng7Za4K4FslNLBJ7fucapF7xdE0b%2BSO690ph%2BXWX5erZJixmzV%2BhiGZay0mBdYAxOfVwZb%2Bc5RA%2FQzPqBsdbdKb1%2FzzD4AcwCHu0w0So54JMQt3qcDQVi56NZaOZA4aC9SLZGcVdYakMF75XKn9Z99e2rXgFtEo2P7%2BuRBY8hD%2Bv%2B6dHC9k6fa9nABZEw38wi3Ozfsf7eQ0PmTmrQ2ue6n97zMjbg6rSn2RmKFWc4iWBl48ikkyFmA1GhKsA%3D%3D",
            mime_type="text/plain",
        ),
        "What is in the text?",
    ],
)

print(response.text)
```

---

---

## 7. 常见错误与排查

- `400 INVALID_ARGUMENT`：`mime_type` 与实际文件不匹配，或请求体字段拼写错误
- `400 FAILED_PRECONDITION`：媒体地址不可访问，或文件尚未可读
- `401 UNAUTHENTICATED`：API Key 无效、缺失或格式错误
- `403 PERMISSION_DENIED`：当前 Key 无权限调用对应模型
- `429 RESOURCE_EXHAUSTED`：请求过快触发限流，请重试并加退避

建议优先检查：

1. 是否使用了脱敏 Key 占位符并在运行时注入真实环境变量
2. `file_data.file_uri` 是否可被服务端访问
3. `mime_type` 是否与媒体格式一致
4. `model` 是否为可用的 Gemini 模型 ID
