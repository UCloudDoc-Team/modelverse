# Wan-AI/Wan2.2-I2V

图生视频模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数 | 类型 | 是否必选 | 描述 |
| :--- | :--- | :--- | :--- |
| model | string | 是 | 模型名称，此处为 `Wan-AI/Wan2.2-I2V` |
| input.first_frame_url | string | 是 | 视频首帧图片URL，可为URL或Base64 |
| input.last_frame_url | string | 是 | 视频末帧图片URL，可为URL或Base64 |
| input.prompt | string | 否 | 提示词，用于指导视频生成 |
| input.negative_prompt | string | 否 | 反向提示词，用于限制不希望出现的内容 |
| parameters.size | string | 否 | 输出尺寸，例如："832x480", "480x832", "1280x720", "720x1280" |
| parameters.resolution | string | 否 | 生成视频的分辨率档位，当前仅支持`720P`、`480P` |
| parameters.duration | int | 否 | 视频生成时长（秒），当前固定为`5` |
| parameters.seed | int | 否 | 随机数种子，范围`[0, 2147483647]` |

### 请求示例

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "Wan-AI/Wan2.2-I2V",
    "input": {
      "first_frame_url": "https://example.com/first_frame.jpg",
      "last_frame_url": "https://example.com/last_frame.jpg",
      "prompt": "a beautiful flower"
    },
    "parameters": {
      "resolution": "720P"
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
        "task_id": "task_id"
    },
    "request_id": "request_id"
}
```

## 查询任务状态

### 接口

`https://api.modelverse.cn/v1/tasks/status?task_id=<task_id>`

### 请求示例

```shell
curl --location 'https://api.modelverse.cn/v1/tasks/status?task_id=<task_id>' \
--header 'Authorization: <YOUR_API_KEY>'
```

### 输出

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| output.task_id | string | 异步任务的唯一标识 |
| output.task_status | string | 任务状态：`Pending`,`Running`,`Success`,`Failure` |
| output.urls | array | 视频结果的 URL 列表 |
| output.submit_time | integer | 任务提交时间戳 |
| output.finish_time | integer | 任务完成时间戳 |
| output.error_message | string | 失败时返回的错误信息 |
| usage.duration | integer | 任务执行时长（秒） |
| request_id | string | 请求的唯一标识 |

### 响应示例

```json
{
    "output": {
        "task_id": "task_id",
        "task_status": "Success",
        "urls": [
            "https://xxxxx/xxxx.mp4"
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
