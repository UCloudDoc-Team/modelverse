# Wan-AI/Wan2.2-T2V

文生视频模型

## 异步提交任务

### 接口

`{{api_proxy}}/v1/tasks/submit`

### 输入

| 参数 | 类型 | 是否必选 | 描述 |
| :--- | :--- | :--- | :--- |
| model | string | 是 | 模型名称，此处为 `Wan-AI/Wan2.2-T2V` |
| input.prompt | string | 是 | 提示词，用于生成视频的内容描述 |
| parameters.resolution | string | 否 | 分辨率, 默认为`480P`。 |

### 请求示例

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "Wan-AI/Wan2.2-T2V",
    "input": {
      "prompt": "杨幂和冯绍峰"
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
        "task_id": "f151ae7f80f44db190194e36c4c6d2b1"
    },
    "request_id": "44a3fd36-5951-4a40-b483-27c72cbb7840"
}
```

## 查询任务状态

### 接口

`{{api_proxy}}/v1/tasks/status?task_id=<task_id>`

### 请求示例

```shell
curl --location 'https://api.modelverse.cn/v1/tasks/status?task_id=f151ae7f80f44db190194e36c4c6d2b1' \
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
        "task_id": "f151ae7f80f44db190194e36c4c6d2b1",
        "task_status": "Success",
        "urls": [
            "https://d2p7pge43lyniu.cloudfront.net/output/48170c13-a8a0-4961-8d35-ec84410ba3c5-u1_b5889a37-cc50-4449-ab25-f8385bc339e6.mp4"
        ],
        "submit_time": 1756958375,
        "finish_time": 1756958428
    },
    "usage": {
        "duration": 5
    },
    "request_id": ""
}
```
