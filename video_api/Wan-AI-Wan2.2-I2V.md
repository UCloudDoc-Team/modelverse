# Wan-AI/Wan2.2-I2V

图生视频模型

## 异步提交任务

### 接口

`{{api_proxy}}/v1/tasks/submit`

### 输入

| 参数 | 类型 | 是否必选 | 描述 |
| :--- | :--- | :--- | :--- |
| model | string | 是 | 模型名称，此处为 `Wan-AI/Wan2.2-I2V` |
| input.image_url | string | 是 | 输入图片的 URL |
| input.prompt | string | 否 | 提示词，用于指导视频生成 |
| parameters.resolution | string | 否 | 分辨率, 默认为`480P`。 |

### 请求示例

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "Wan-AI/Wan2.2-I2V",
    "input": {
      "image_url": "https://example.com/source_image.jpg",
      "prompt": "make the person in image wave their hand"
    },
    "parameters": {
      "resolution": "480P"
    }
  }'
```

### 输出

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| output.task_id | string | 异步任务的唯一标识 |
| request_id | string | 请求的唯一标识 |

### 响应示例

```json
{
    "output": {
        "task_id": "a1b2c3d4e5f67890a1b2c3d4e5f67890"
    },
    "request_id": "12345678-1234-1234-1234-1234567890ab"
}
```

## 查询任务状态

### 接口

`{{api_proxy}}/v1/tasks/status?task_id=<task_id>`

### 请求示例

```shell
curl --location 'https://api.modelverse.cn/v1/tasks/status?task_id=a1b2c3d4e5f67890a1b2c3d4e5f67890' \
--header 'Authorization: <YOUR_API_KEY>' \
--data ''
```

### 输出

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| output.task_id | string | 异步任务的唯一标识 |
| output.task_status | string | 任务状态，如 `Success`, `Running`, `Failed` |
| output.urls | array | 视频结果的 URL 列表 |
| output.submit_time | integer | 任务提交时间戳 |
| output.finish_time | integer | 任务完成时间戳 |
| usage.duration | integer | 任务执行时长（秒） |
| request_id | string | 请求的唯一标识 |

### 响应示例

```json
{
    "output": {
        "task_id": "a1b2c3d4e5f67890a1b2c3d4e5f67890",
        "task_status": "Success",
        "urls": [
            "https://d2p7pge43lyniu.cloudfront.net/output/some-video-id.mp4"
        ],
        "submit_time": 1756959000,
        "finish_time": 1756959050
    },
    "usage": {
        "duration": 5
    },
    "request_id": ""
}
```
