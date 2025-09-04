# black-forest-labs/flux.1-dev API

本文介绍 `black-forest-labs/flux.1-dev` 模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

## 请求参数

### 请求体

| 字段名          | 类型   | 是否必须 | 默认值    | 描述                                                                                                                                                                        |
| --------------- | ------ | -------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| prompt          | string | 条件必填 | -         | 用于生成图像的提示词。                                                                                                                                                      |
| model           | string | 必须     | -         | 本次请求使用的模型名称，此处为 `black-forest-labs/flux.1-dev`。                                                                                                                                                    |
| n               | int    | 可选     | 1         | 生成图片的数量。<br>取值 [1, 4]                                                                                                                                             |
| image           | string | 可选     | -         | 支持图片链接或 Base64 编码（格式为：data:image/<图片格式>;base64,<Base64 编码>）。                                                                                          |
| response_format | string | 可选     | url       | 指定生成图像的返回格式。<br>url：返回图片链接；<br>b64_json：返回 Base64 编码字符串。                                                                                       |
| size            | string | 可选     | 1024x1024 | 生成图像的宽高像素，要求介于[256x256, 1536x1536]之间。                                                                                                                      |
| strength        | float  | 可选     | 0.8       | 转换图像的参考程度，取值[0.0, 1.0]。                                                                                                                                        |
| steps           | int    | 可选     | 28        | 推理步骤数,数值越大，效果更精细，运行时间更长。                                                                                                                             |
| seed            | int    | 可选     | -1        | 随机数种子，控制生成内容的随机性。取值范围[-1, 9999999999]。如不提供则自动生成。相同 seed 可复现相同内容。                                                                  |
| guidance_scale  | float  | 可选     | 2.5       | 用于在图像生成过程中调整模型的创造性与文本指导的紧密度。较高的值会使得生成的图像更忠于文本提示，但可能减少多样性；较低的值则允许更多创造性，增加图像变化。<br>取值[1, 10]。 |

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
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "black-forest-labs/flux.1-dev",
    "prompt": "A cute cat",
    "n": 1,
    "size": "1024x1024"
}'
```

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url=os.getenv("BASE_URL", "https://api.modelverse.cn/v1"),
    api_key=os.getenv("API_KEY", "<你的API Key>")
)

response = client.images.generate(
    model="black-forest-labs/flux.1-dev",
    prompt="A cute cat",
    n=1,
    size="1024x1024"
)

print(response.data[0].url)
```



### 响应

```json
{
  "created": 1750667997,
  "data": [
    {
      "url": "https://api.modelverse.cn/image/xxx",
      "b64_json": "data:image/png;base64,iVBORw0KGgoAAAANSUhEU..."
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
    "message": "xxx",
    "type": "",
    "param": "b4a7b49c-203c-43c9-88ce-9e636e77ace8",
    "code": "xxx"
  }
}
```
<!-- 
TODO:异步请求
### 异步请求

``` -->