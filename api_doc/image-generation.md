# 图片生成 API

`POST https://api.modelverse.cn/v1/images/generations`

本文介绍图片生成模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

## 生图模型列表

| 模型                                             | 免费额度（张图） |
| ------------------------------------------------ | ---------------- |
| black-forest-labs/flux.1-dev                     | 10               |
| black-forest-labs/flux-kontext-max               | 5                |
| black-forest-labs/flux-kontext-max/multi         | 5                |
| black-forest-labs/flux-kontext-max/text-to-image | 5                |
| black-forest-labs/flux-kontext-pro               | 5                |
| black-forest-labs/flux-kontext-pro/multi         | 5                |
| black-forest-labs/flux-kontext-pro/text-to-image | 5                |
| stepfun-ai/step1x-edit                       | 5                |

## 请求参数

### 请求体

参数支持列表

| 字段名          | 是否必须 | 默认值    | 支持模型                                                                                                                             | 描述                                                                                                                                                                                                                                                |
| --------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| prompt          | 条件必填 | -         | 全部                                                                                                                                 | 用于生成图像的提示词。                                                                                                                                                                                                                              |
| model           | 必须     | -         | 全部                                                                                                                                 | 本次请求使用的模型名称。                                                                                                                                                                                                                            |
| n               | 可选     | 1         | black-forest-labs/flux.1-dev<br>black-forest-labs/flux-kontext-pro/text-to-image<br>black-forest-labs/flux-kontext-max/text-to-image | 生成图片的数量。                                                                                                                                                                                                                                    |
| image           | 条件必填 | -         | black-forest-labs/flux.1-dev（可选）<br>black-forest-labs/flux-kontext-max（必填）<br>black-forest-labs/flux-kontext-pro（必填）     | 参考图图片链接或 Base64 编码。<br>图片限制：10MB 以内，支持 PNG、JPG、JPEG、WEBP、BMP、ICO，长宽比例不超过 4 倍，长或宽不低于 128 像素。<br>仅支持 irag-1.0。<br>支持图片链接或 Base64 编码（格式为：data:image/<图片格式>;base64,<Base64 编码>）。 |
| images          | 必填     | -         | black-forest-labs/flux-kontext-max/multi<br>black-forest-labs/flux-kontext-pro/multi                                                 | 参考图片数组，每个 item 应为 公网 url 链接 或 图片内容的 base64 编码。                                                                                                                                                                              |
| response_format | 可选     | url       | black-forest-labs/flux.1-dev                                                                                                         | 指定生成图像的返回格式。<br>url：返回图片链接；<br>b64_json：返回 Base64 编码字符串。                                                                                                                                                               |
| size            | 可选     | 1024x1024 | black-forest-labs/flux.1-dev                                                                                                         | 生成图像的宽高像素，要求介于[256x256, 1536x1536]之间。                                                                                                                                                                                              |
| strength        | 可选     | 0.8       | black-forest-labs/flux.1-dev                                                                                                         | 转换图像的参考程度，取值[0.0, 1.0]。                                                                                                                                                                                                                |
| aspect_ratio    | 可选     | 1:1       | black-forest-labs/flux-kontext-max/text-to-image<br>black-forest-labs/flux-kontext-pro/text-to-image                                 | 生成图片的尺寸比例。                                                                                                                                                                                                                                |
| steps           | 可选     | 28        | black-forest-labs/flux.1-dev<br>stepfun-ai/step1x-edit                                                                           | 推理步骤数。                                                                                                                                                                                                                                        |
| seed            | 可选     | -1        | 全部                                                                                                                                 | 随机数种子，控制生成内容的随机性。取值范围[-1, 2147483647]。如不提供则自动生成。相同 seed 可复现相同内容。                                                                                                                                          |
| guidance_scale  | 可选     | 2.5       | 全部                                                                                                                                 | 模型输出与 prompt 一致程度，值越大自由度越小，与提示词相关性越强。取值[1, 10]。                                                                                                                                                                     |
| negative_prompt | 可选     | -         | stepfun-ai/step1x-edit                                                                                                           | 负面提示词，用于指定不希望在生成图像中出现的内容。                                                                                                                                                                                                  |

## 响应参数

| 字段名        | 类型      | 描述                                                                                                                                                                                                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| created       | `integer` | 本次请求创建时间的 Unix 时间戳（秒）。                                                                                                                                                                                                                  |
| data          | `list`    | 输出图像的信息，包括图像下载的 URL 或 Base64。<br>• 当指定返回生成图像的格式为 url 时，则相应参数的子字段为 url；<br>• 当指定返回生成图像的格式为 b64_json 时，则相应参数的子字段为 b64_json。<br>注意：链接将在生成后 7 天内失效，请务必及时保存图像。 |
| error         | `Object`  | 错误信息对象                                                                                                                                                                                                                                            |
| error.code    | `string`  | 错误码 TODO: 跳转链接                                                                                                                                                                                                                                   |
| error.message | `string`  | 错误提示信息                                                                                                                                                                                                                                            |
| error.param   | `string`  | 请求 id                                                                                                                                                                                                                                                 |

## 示例

### 请求

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "black-forest-labs/flux.1-dev",
    "prompt": "draw a cute cat"
}'
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
