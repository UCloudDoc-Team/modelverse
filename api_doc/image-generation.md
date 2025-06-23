


`POST https://ark.cn-beijing.volces.com/api/v3/images/generations`

本文介绍图片生成模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

## 生图模型列表
| 模型 | 描述 | 免费额度（张图） |
| - | - | - |
| black-forest-labs/FLUX.1-dev | 英文模型 | 10 |
| black-forest-labs/Flux-Kontext-Max | 英文模型 | 5 |
| black-forest-labs/Flux-Kontext-Max-multi | | 5 |
| black-forest-labs/Flux-Kontext-Max-text2image | | 5 |
| black-forest-labs/Flux-Kontext-Pro | | 5 |
| black-forest-labs/Flux-Kontext-Pro-multi | | 5 |
| black-forest-labs/Flux-Kontext-Pro-text2image | | 5 |
| stepfun-ai/Step1X-Edit | | 5 |


## 请求参数

### 请求体

- **prompt** `string` `必选`
用于生成图像的提示词。

- **model** `string` `必选`
本次请求使用模型的 Model

- **n** `integer` `可选` `默认值 1`
生成图片张数
支持模型：
`black-forest-labs/FLUX.1-dev`
`black-forest-labs/Flux-Kontext-Pro-text2image`
`black-forest-labs/Flux-Kontext-Max-text2image`

- **image** `string`
生图的参考图片，可以是url链接或者base64的编码数据。
支持模型：
`black-forest-labs/FLUX.1-dev` `可选`
`black-forest-labs/Flux-Kontext-Max` `必填`
`black-forest-labs/Flux-Kontext-Pro` `必填`

- **images** `list`
生图的参考图片数组，每个item可以是url链接或者base64的编码数据。
支持模型：
`black-forest-labs/Flux-Kontext-Max-multi` `必填`
`black-forest-labs/Flux-Kontext-Pro-multi` `必填`

- **response_format** `string` `可选` `默认值 url`
指定生成图像的返回格式。支持以下两种取值：
    - "url"：以可下载的图片链接形式返回；
    - "b64_json"：以 Base64 编码字符串的 JSON 格式返回图像数据。

- **size** `string` `可选` `默认值 1024x1024`
生成图像的宽高像素，要求介于 [256x256, 1536x1536] 之间。
支持模型：
`black-forest-labs/FLUX.1-dev`

- **strength** `float` `可选` `默认值 0.8`
转换图像的参考程度，取值 [0.0, 1.0]
支持模型：
`black-forest-labs/FLUX.1-dev`

- **aspect_ratio** `string` `默认值 1:1`
生成图片的尺寸
支持模型：
`black-forest-labs/Flux-Kontext-Max-text2image`
`black-forest-labs/Flux-Kontext-Pro-text2image`

- **steps** `integer` `可选`  `默认值 28`
要执行的推理步骤数
支持模型：
`black-forest-labs/FLUX.1-dev`
`stepfun-ai/Step1X-Edit`

- **seed** `integer` `可选`  `默认值 -1`
随机数种子，用于控制模型生成内容的随机性。取值范围为 [-1, 2147483647]。如果不提供，则自动生成随机种子。如果希望生成内容保持一致，可以使用相同的 seed 参数值。

- **guidance_scale**  `float` `可选` `默认值 2.5`
模型输出结果与prompt的一致程度，即生成图像的自由度；值越大，模型自由度越小，与用户输入的提示词相关性越强。取值范围：[1, 10] 之间的浮点数。

- **negative_prompt** `string` `可选`
负面提示词，用于指定不希望在生成图像中出现的内容
支持模型： `stepfun-ai/Step1X-Edit`

## 响应参数

- **created** `integer`
本次请求创建时间的 Unix 时间戳（秒）。

- **data** `list` 
输出图像的信息，包括图像下载的 URL 或 Base64。
    - 当指定返回生成图像的格式为url时，则相应参数的子字段为url；
    注意：链接将在生成后7天内失效，请务必及时保存图像。
    - 当指定返回生成图像的格式为b64_json时，则相应参数的子字段为b64_json。

- **error**  `Object`
    - error.code `string`
        错误码 TODO: 跳转链接
    - error.message `string`
        错误提示信息
    - error.param `string`
        请求id

## 示例

### 请求
```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "black-forest-labs/FLUX.1-dev",
    "prompt": "draw a cute cat"
}'
```
### 响应

```json
{
    "created": 1750667997,
    "data":
    [
        {
            "url": "https://api.modelverse.cn/image/xxx"
        }
    ],
    "usage":
    {
        "input_tokens_details":
        {}
    }
}
```
