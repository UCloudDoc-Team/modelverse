# Wan-AI/Wan2.7 Image API

本文介绍 `wan2.7-image`、`wan2.7-image-pro` 模型调用 API 的输入输出参数，供您使用接口时查阅字段含义。

- 同步接口：`/v1/images/generations`
- 异步接口：`/v1/tasks/submit` + `/v1/tasks/status`

---

## 模型列表

| 模型名 | 说明 |
| ------ | ---- |
| `wan2.7-image` | Wan2.7 图片生成模型 |
| `wan2.7-image-pro` | Wan2.7 图片生成 Pro 模型 |

## 能力概览

Wan2.7 当前支持：

- 文生图
- 单图编辑
- 多图融合 / 多图编辑
- 连续多图生成 / 组图生成

## OpenAI 兼容接口（同步）

### 接口

`POST https://api.modelverse.cn/v1/images/generations`

### 认证方式

`Authorization: Bearer $MODELVERSE_API_KEY`

### 请求参数

| 字段名 | 类型 | 是否必须 | 默认值 | 描述 |
| ------ | ---- | -------- | ------ | ---- |
| model | string | 必须 | - | 模型名称，可选值：`wan2.7-image`、`wan2.7-image-pro` |
| prompt | string | 可选 | - | 文本提示词。`prompt`、`image`、`images` 至少需要提供一项。 |
| image | string | 可选 | - | 单图输入，支持公网 URL 或 Base64。适用于单图编辑场景。 |
| images | array[string] | 可选 | - | 多图输入，支持公网 URL 或 Base64。适用于多图融合、多图编辑场景。若同时传入 `images` 和 `image`，优先使用 `images`。 |
| size | string | 可选 | - | 输出尺寸，支持 `1K`、`2K`、`4K`，也兼容 `1024x1024`、`2048x2048`、`4096x4096` |
| n | int | 可选 | 1 | 生成图片数量。普通模式建议 `1~4`，开启 `enable_sequential` 时可传更多张数。 |
| seed | int | 可选 | - | 随机种子。建议传正整数；传 `0` 或不传时按未设置处理。 |
| watermark | boolean | 可选 | false | 是否添加水印。 |
| thinking_mode | boolean | 可选 | false | 是否开启深度思考模式。 |
| enable_sequential | boolean | 可选 | false | 是否开启连续多图生成 / 组图模式。建议与 `n` 配合使用。 |

### 请求示例

#### 1. 文生图

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image",
    "prompt": "A futuristic cloud data center floating above the sea at sunrise",
    "size": "1K",
    "seed": 123456,
    "watermark": false
  }'
```

#### 2. 单图编辑

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "prompt": "把这只猫改成赛博朋克风格，霓虹灯光，电影感构图",
    "image": "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg",
    "size": "2K",
    "thinking_mode": true,
    "watermark": false
  }'
```

#### 3. 多图融合

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "prompt": "融合两张参考图的主体特征，生成一张高级感品牌海报",
    "images": [
      "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg",
      "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg"
    ],
    "size": "2048x2048",
    "thinking_mode": true,
    "seed": 20260402
  }'
```

#### 4. 连续多图生成

```bash
curl --location 'https://api.modelverse.cn/v1/images/generations' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "prompt": "生成一组同一角色在春夏秋冬四季中的海报，人物形象保持一致",
    "size": "2K",
    "thinking_mode": true,
    "enable_sequential": true,
    "n": 4
  }'
```

### 响应参数

| 字段名 | 类型 | 描述 |
| ------ | ---- | ---- |
| created | integer | 本次请求创建时间的 Unix 时间戳（秒） |
| data | array | 输出图片信息列表 |
| data.url | string | 生成图片的下载地址 |

### 响应示例

```json
{
  "created": 1775558400,
  "data": [
    {
      "url": "https://example.com/generated-image-1.png"
    },
    {
      "url": "https://example.com/generated-image-2.png"
    }
  ]
}
```

## 异步任务接口

### 提交任务

#### 接口

`POST https://api.modelverse.cn/v1/tasks/submit`

#### 认证方式

`Authorization: Bearer $MODELVERSE_API_KEY`

#### 请求体

```json
{
  "model": "wan2.7-image-pro",
  "input": {
    "prompt": "一只坐在咖啡馆窗边的布偶猫",
    "images": [
      "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg"
    ]
  },
  "parameters": {
    "size": "2K",
    "prompt_extend": true,
    "enable_sequential": true,
    "n": 4,
    "seed": 123456,
    "watermark": false
  }
}
```

#### 请求参数

| 字段名 | 类型 | 是否必须 | 描述 |
| ------ | ---- | -------- | ---- |
| model | string | 必须 | 模型名称，可选值：`wan2.7-image`、`wan2.7-image-pro` |
| input.prompt | string | 可选 | 文本提示词。`input.prompt`、`input.images`、`input.img_url`、`input.first_frame_url` 至少需要提供一项。 |
| input.images | array[string] | 可选 | 多图输入，支持公网 URL 或 Base64。若同时传入 `input.images`、`input.img_url`、`input.first_frame_url`，优先使用 `input.images`。 |
| input.img_url | string | 可选 | 单图输入，支持公网 URL 或 Base64。适用于单图编辑场景。 |
| input.first_frame_url | string | 可选 | 单图兼容字段，支持公网 URL 或 Base64。在 Wan2.7 图片任务中会按普通图片输入处理。 |
| parameters.size | string | 可选 | 输出尺寸，支持 `1K`、`2K`、`4K`，也兼容 `1024x1024`、`2048x2048`、`4096x4096`。 |
| parameters.n | int | 可选 | 生成图片数量。取值范围 `1~4` |
| parameters.seed | int | 可选 | 随机种子。建议传正整数；传 `0` 或不传时按未设置处理。 |
| parameters.watermark | boolean | 可选 | 是否添加水印。 |
| parameters.enable_sequential | boolean | 可选 | 是否开启连续多图生成 / 组图模式。通常建议与 `parameters.n` 配合使用。 |
| parameters.prompt_extend | boolean | 可选 | 提示词增强开关，传 `true` 时会映射为 Wan2.7 的 `thinking_mode=true`。 |

#### 请求示例

##### 1. 文生图

```bash
curl --location 'https://api.modelverse.cn/v1/tasks/submit' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image",
    "input": {
      "prompt": "A futuristic cloud data center floating above the sea at sunrise"
    },
    "parameters": {
      "size": "1K",
      "seed": 123456
    }
  }'
```

##### 2. 单图编辑

```bash
curl --location 'https://api.modelverse.cn/v1/tasks/submit' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "input": {
      "prompt": "把这只猫改成赛博朋克风格，霓虹灯光，电影感构图",
      "img_url": "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg"
    },
    "parameters": {
      "size": "2K",
      "prompt_extend": true,
      "watermark": false
    }
  }'
```

##### 3. 多图融合

```bash
curl --location 'https://api.modelverse.cn/v1/tasks/submit' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "input": {
      "prompt": "融合两张参考图的主体特征，生成一张高级感品牌海报",
      "images": [
        "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg",
        "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg"
      ]
    },
    "parameters": {
      "size": "2048x2048",
      "prompt_extend": true,
      "seed": 20260402
    }
  }'
```

##### 4. 连续多图生成

```bash
curl --location 'https://api.modelverse.cn/v1/tasks/submit' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
    "model": "wan2.7-image-pro",
    "input": {
      "prompt": "生成一组同一角色在春夏秋冬四季中的海报，人物形象保持一致"
    },
    "parameters": {
      "size": "2K",
      "prompt_extend": true,
      "enable_sequential": true,
      "n": 4
    }
  }'
```

#### 提交响应参数

| 字段名 | 类型 | 描述 |
| ------ | ---- | ---- |
| output.task_id | string | 异步任务唯一标识 |
| request_id | string | 请求唯一标识 |

#### 提交响应示例

```json
{
  "output": {
    "task_id": "3c7d6b6c-7cdb-4f37-bf41-7f2b0f1a9f0d"
  },
  "request_id": "req_202604021530000001"
}
```

### 查询任务状态

#### 接口

`GET https://api.modelverse.cn/v1/tasks/status?task_id=<task_id>`

#### 请求示例

```bash
curl --location 'https://api.modelverse.cn/v1/tasks/status?task_id=<task_id>' \
  --header "Authorization: Bearer $MODELVERSE_API_KEY"
```

#### 响应参数

| 字段名 | 类型 | 描述 |
| ------ | ---- | ---- |
| output.task_id | string | 异步任务唯一标识 |
| output.task_status | string | 任务状态：`Pending`、`Running`、`Success`、`Failure`、`Cancelled` |
| output.urls | array[string] | 图片结果 URL 列表 |
| output.submit_time | integer | 任务提交时间戳 |
| output.finish_time | integer | 任务完成时间戳 |
| output.error_message | string | 失败时返回的错误信息 |
| usage.completion_tokens | integer | 可选，输出 token 数 |
| usage.total_tokens | integer | 可选，总 token 数 |
| request_id | string | 请求唯一标识 |

#### 响应示例（成功）

```json
{
  "output": {
    "task_id": "3c7d6b6c-7cdb-4f37-bf41-7f2b0f1a9f0d",
    "task_status": "Success",
    "urls": [
      "https://example.com/generated-image-1.png",
      "https://example.com/generated-image-2.png",
      "https://example.com/generated-image-3.png",
      "https://example.com/generated-image-4.png"
    ],
    "submit_time": 1775115000,
    "finish_time": 1775115038
  },
  "usage": {
    "completion_tokens": 4,
    "total_tokens": 4
  },
  "request_id": ""
}
```

#### 响应示例（失败）

```json
{
  "output": {
    "task_id": "3c7d6b6c-7cdb-4f37-bf41-7f2b0f1a9f0d",
    "task_status": "Failure",
    "submit_time": 1775115000,
    "finish_time": 1775115012,
    "error_message": "code=InvalidParameter, message=invalid image url"
  },
  "request_id": ""
}
```
