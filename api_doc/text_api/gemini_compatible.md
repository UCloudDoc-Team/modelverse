# Gemini 快速开始

UModelverse 平台提供了与 Google Gemini API 兼容的 **Models** 接口，开发者可以使用 Gemini SDK 或其他支持的工具直接调用 Modelverse 上的 **Gemini 模型**。

本文将向您介绍如何快速在 UModelverse 平台发出您的第一个 Gemini API 请求。

## 快速开始

### 安装 Google GenAI SDK
安装 python 语言的 sdk

> 使用 Python 3.9 及更高版本，通过以下 pip 命令安装 google-genai 软件包：

```python
pip install google-genai
```

### 示例
以下示例使用 generateContent 方法，通过`gemini-3-flash-preview`模型向 UModelverse API 发送请求。

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
   model="gemini-3-flash-preview",
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
curl "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
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
    model="gemini-3-flash-preview", contents=["Explain how AI works"]
)
for chunk in response:
    print(chunk.text, end="")

```

#### ** curl **

```bash
curl "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:GenerateContent?alt=sse" \
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

## Google Search（联网检索）使用说明

Gemini 支持通过 `tools.google_search` 调用实时网络检索能力。启用后，模型会在需要时自动发起搜索，并在响应中返回可追溯的来源信息（`groundingMetadata`）。

> 详细字段定义与能力说明可参考 Google Gemini 官方文档：https://ai.google.dev/gemini-api/docs/google-search?hl=zh-cn

适用场景：

*   需要最新信息（如近期事件、价格、公告）。
*   需要可验证来源与引用链路。
*   需要降低纯参数知识带来的幻觉风险。

### 请求要点

*   请求体中增加 `tools`，并传入 `{"google_search": {}}`。
*   建议使用支持 Google Search 工具的 Gemini 模型（如 `gemini-3-flash-preview`）。
*   使用 Modelverse 兼容地址与鉴权：
    *   Base URL: `https://api.modelverse.cn`
    *   API Key Header: `x-goog-api-key: $MODELVERSE_API_KEY`

### 示例：Python SDK（Modelverse）

```python
from google import genai
from google.genai import types

client = genai.Client(
    api_key="<MODELVERSE_API_KEY>",
    http_options=types.HttpOptions(base_url="https://api.modelverse.cn"),
)

response = client.models.generate_content(
    model="gemini-3-flash-preview",
    contents="请告诉我今天 AI 领域最重要的三条新闻，并给出来源。",
    config=types.GenerateContentConfig(
        tools=[types.Tool(google_search=types.GoogleSearch())]
    ),
)

print(response.text)

# 可选：读取 grounding 元数据（若模型触发了搜索）
if response.candidates and response.candidates[0].grounding_metadata:
    gm = response.candidates[0].grounding_metadata
    if gm.grounding_chunks:
        for idx, chunk in enumerate(gm.grounding_chunks, 1):
            if chunk.web:
                print(f"[{idx}] {chunk.web.title}: {chunk.web.uri}")
```

### 示例：curl（Modelverse）

```bash
curl "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
  -H "x-goog-api-key: $MODELVERSE_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "请基于网络信息，概括一下今天全球 AI 相关新闻，并附带来源。"
          }
        ]
      }
    ],
    "tools": [
      {
        "google_search": {}
      }
    ]
  }'
```

### 响应字段说明（重点）

当请求触发联网检索时，`candidates[*].groundingMetadata` 常见字段包括：

*   `webSearchQueries`：模型实际发起的搜索词。
*   `groundingChunks`：候选来源列表（含 `uri`、`title`）。
*   `groundingSupports`：回答片段与来源索引的映射，可用于做内联引用。

> 建议在前端展示来源链接，提升回答可解释性与用户信任。

### 文档理解

Gemini 模型可以处理 PDF 格式的文档，并使用原生视觉功能来理解整个文档的上下文。这不仅仅是提取文本，还让 Gemini 能够：

*   分析和解读内容，包括文本、图片、图表、图表和表格，即使是长达 1,000 页的文档也能轻松应对。
*   以结构化输出格式提取信息。
*   根据文档中的视觉和文本元素总结内容并回答问题。
*   转写文档内容（例如转写为 HTML），同时保留布局和格式，以便在下游应用中使用。
您也可以通过相同的方式传递非 PDF 文档，但 Gemini 会将这些文档视为普通文本，从而消除图表或格式等上下文。

#### 以内嵌方式传递 PDF 数据
您可以在向 generateContent 发出的请求中内嵌传递 PDF 数据。此方法最适合处理较小的文档或临时处理，因为您无需在后续请求中引用该文件。

以下示例展示了如何从网址提取 PDF 并将其转换为字节以进行处理：

 <!-- tabs:start -->
#### ** python **

```python
from google import genai
from google.genai import types
import httpx

client = genai.Client(
    api_key="<MODELVERSE_API_KEY>",
    http_options=types.HttpOptions(
        base_url="https://api.modelverse.cn"
    ),
)

doc_url = "https://umodelverse-inference.cn-wlcb.ufileos.com/gemini-pdf.pdf"

# Retrieve and encode the PDF byte
doc_data = httpx.get(doc_url).content

prompt = "Summarize this document"
response = client.models.generate_content(
    model="gemini-3-flash-preview",
    contents=[
        types.Part.from_bytes(
            data=doc_data,
            mime_type='application/pdf',
        ),
        prompt
    ]
)

print(response.text)
```

#### ** curl **

```bash
DOC_URL="https://umodelverse-inference.cn-wlcb.ufileos.com/gemini-pdf.pdf"
PROMPT="Summarize this document"
DISPLAY_NAME="base64_pdf"

# Download the PDF
wget -O "${DISPLAY_NAME}.pdf" "${DOC_URL}"

# Check for FreeBSD base64 and set flags accordingly
if [[ "$(base64 --version 2>&1)" = *"FreeBSD"* ]]; then
  B64FLAGS="--input"
else
  B64FLAGS="-w0"
fi

# Base64 encode the PDF
ENCODED_PDF=$(base64 $B64FLAGS "${DISPLAY_NAME}.pdf")

# Generate content using the base64 encoded PDF
curl "https://api.modelverse.cn/v1beta/models/gemini-3-flash-preview:generateContent" \
    -H "x-goog-api-key: $MODELVERSE_API_KEY" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d '{
      "contents": [{
        "parts":[
          {"inline_data": {"mime_type": "application/pdf", "data": "'"$ENCODED_PDF"'"}},
          {"text": "'$PROMPT'"}
        ]
      }]
    }' 2> /dev/null > response.json

cat response.json
echo

jq ".candidates[].content.parts[].text" response.json

# Clean up the downloaded PDF
rm "${DISPLAY_NAME}.pdf"
```
<!-- tabs:end -->

您还可以从本地文件读取 PDF 以进行处理：

```python
from google import genai
from google.genai import types
import pathlib

client = genai.Client(
    api_key="<MODELVERSE_API_KEY>",
    http_options=types.HttpOptions(
        base_url="https://api.modelverse.cn"
    ),
)

# Retrieve and encode the PDF byte
filepath = pathlib.Path('file.pdf')

prompt = "Summarize this document"
response = client.models.generate_content(
  model="gemini-3-flash-preview",
  contents=[
      types.Part.from_bytes(
        data=filepath.read_bytes(),
        mime_type='application/pdf',
      ),
      prompt])
print(response.text)
```

## 模型ID说明
更多受支持的gemini模型，请参考【获取模型列表】


> 更多字段详情，见[Gemini官方文档](https://ai.google.dev/api/models?hl=zh-cn)
