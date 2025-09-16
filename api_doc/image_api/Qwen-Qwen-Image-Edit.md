# Qwen/Qwen-Image-Edit API

本文介绍 `Qwen/Qwen-Image-Edit` 模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

## 请求参数

### 请求体

| 字段名          | 类型   | 是否必须 | 默认值 | 描述                                                                                                                          |
| --------------- | ------ | -------- | ------ | ----------------------------------------------------------------------------------------------------------------------------- |
| prompt          | string | 必须     | -      | 提示词                                                                                                                        |
| model           | string | 必须     | -      | 本次请求使用的模型名称，此处为 `Qwen/Qwen-Image-Edit`。                                                                       |
| image           | string | 必须     | -      | base64 数据或图片链接 http://xxx                                                                                              |
| n               | int    | 可选     | 1      | 生成图片数量，取值范围为 1~4                                                                                                  |
| strength        | float  | 可选     | 0.8    | 转换参考图像的程度, 取值范围 0~1                                                                                              |
| seed            | int    | 可选     | -1     | 随机数种子，用于控制模型生成内容的随机性。如果希望生成内容保持一致，可以使用相同的 seed 参数值。                              |
| steps           | int    | 可选     | 20     | 推理次数, 取值范围 1~50                                                                                                       |
| guidance_scale  | float  | 可选     | 2.5    | 模型输出结果与 prompt 的一致程度，即生成图像的自由度；值越大，模型自由度越小，与用户输入的提示词相关性越强。<br>取值[1, 10]。 |
| negative_prompt | string | 可选     | -      | 负面提示词，用于指定不希望在生成图像中出现的内容                                                                              |
| response_format | string | 可選     | "url"  | 指定返回生成图像的格式，默认为 `url`，可选 `b64_json`                                                                         |

## 响应参数

| 字段名        | 类型      | 描述                                                                                                                                                                                                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| created       | `integer` | 本次请求创建时间的 Unix 时间戳（秒）。                                                                                                                                                                                                                  |
| data          | `array`   | 输出图像的信息，包括图像下载的 URL 或 Base64。<br>• 当指定返回生成图像的格式为 url 时，则相应参数的子字段为 url；<br>• 当指定返回生成图像的格式为 b64_json 时，则相应参数的子字段为 b64_json。<br>注意：链接将在生成后 7 天内失效，请务必及时保存图像。 |
| error         | `Object`  | 错误信息对象                                                                                                                                                                                                                                            |
| error.code    | `string`  | 错误码                                                                                                                                                                                                                                                  |
| error.message | `string`  | 错误提示信息                                                                                                                                                                                                                                            |
| error.param   | `string`  | 请求 id                                                                                                                                                                                                                                                 |

## 示例

### OPENAI 兼容接口

`POST https://api.modelverse.cn/v1/images/generations`

#### 同步请求

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
--header "Authorization: Bearer $MODELVERSE_API_KEY" \
--header 'Content-Type: application/json' \
--data '{
    "model": "Qwen/Qwen-Image-Edit",
    "prompt": "Convert to quick pencil sketch",
    "image": "data:image/png;base64,{image_base64_string}",
    "negative_prompt": "blurry, low quality"
}'
```

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url=os.getenv("BASE_URL", "https://api.modelverse.cn/v1"),
    api_key=os.getenv("API_KEY", "$MODELVERSE_API_KEY")
)

response = client.images.generate(
    model="Qwen/Qwen-Image-Edit",
    prompt="Convert to quick pencil sketch",
    extra_body={
        "image": "data:image/png;base64,{image_base64_string}",
        "negative_prompt": "blurry, low quality"
    }
)

print(response.data[0].url)
```

### 响应

```json
{
  "created": 1750667997,
  "data": [
    {
      "url": "https://xxxxx/xxxx.png",
      "b64_json": "data:image/png;base64,{image_base64_string}"
    }
  ],
  "usage": {
    "input_tokens_details": {}
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

<!--
TODO:异步请求
### 异步请求

``` -->
