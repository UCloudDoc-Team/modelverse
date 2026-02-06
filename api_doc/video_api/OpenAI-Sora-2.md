# Sora-2

视频模型，此接口与submit调用方式不同，与`openai` 官方接口调用方式一致，模型功能无差异。

## 异步提交任务

### 接口

`http://api.modelverse.cn/v1/videos`

### 输入

| 参数                       | 类型     | 是否必选 | 描述                                                                      |
|:-------------------------|:-------| :------- |:------------------------------------------------------------------------|
| model                    | string | 是       | 模型名称，此处为 `sora-2`                                                       |
| prompt                   | string | 是       | 提示词，用于指导视频生成                                                            |
| size                     | string | 否       | 生成视频的尺寸。 <br>可选的分辨率： <br>- `720x1280`<br>- `1280x720`<br> |
| duration                 | string | 否       | 视频生成时长（秒），可选值 `4`, `8`, `12`                                            |
| input_reference          | string | 否       | 首帧图                                                                     |

### 请求示例
⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
```shell
curl -X POST "http://api.modelverse.cn/v1/videos" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: multipart/form-data" \
  -F prompt="She turns around and smiles, then slowly walks out of the frame." \
  -F model="sora-2" \
  -F size="1280x720" \
  -F seconds="8" \
  -F input_reference="@sample_720p.jpeg;type=image/jpeg"
```

### 输出

| 参数           | 类型     | 描述        |
| :------------- |:-------|:----------|
| id | string | 异步任务的唯一标识 |
| object     | string | 类型        |
| model     | string | 模型名称      |
| status     | string | 任务状态,queued, in_progress, completed, and failed     |
| progress     | int    | 任务进度      |
| size     | string | 分辨率       |
| seconds     | string | 时长        |

### 响应示例

```json
{
    "id": "video_456",
    "object": "video",
    "model": "sora-2",
    "status": "queued",
    "progress": 0,
    "created_at": 1712698600,
    "size": "720x1280",
    "seconds": "8"
}
```

## 查询任务状态

### 接口

`https://api.modelverse.cn/v1/videos/<task_id>`

### 请求示例

```shell
curl --location 'https://api.modelverse.cn/v1/videos/video_6982fa1cd4f081908b619dc53cfc2435' \
-H "Authorization: Bearer $OPENAI_API_KEY"
```

### 输出
| 参数           | 类型     | 描述        |
| :------------- |:-------|:----------|
| id | string | 异步任务的唯一标识 |
| object     | string | 类型        |
| model     | string | 模型名称      |
| status     | string | 任务状态,queued, in_progress, completed, and failed     |
| progress     | int    | 任务进度      |
| size     | string | 分辨率       |
| seconds     | string | 时长        |

### 响应示例（成功）

```json
{
  "id": "video_456",
  "object": "video",
  "model": "sora-2",
  "status": "queued",
  "progress": 0,
  "created_at": 1712698600,
  "size": "720x1280",
  "seconds": "8"
}
```

### 响应示例（失败）

```json
{
  "error": {
    "message": "error",
    "type": "invalid_request_error",
    "param": null,
    "code": null
  }
}
```

## 下载视频
### 接口
`https://api.modelverse.cn/v1/videos/<task_id>/content`

### 请求示例
```shell
curl --location 'https://api.modelverse.cn/v1/videos/<task_id>/content' \
-H "Authorization: Bearer $OPENAI_API_KEY"
--output video.mp4
```

### 正常返回
 返回二进制流
### 异常返回
```json
{
    "error": {
        "message": "task is not completed yet, current status: Pending",
        "type": "invalid_request_error"
    }
}
```

## remix

### 接口
`https://api.modelverse.cn/v1/videos/<task_id>/remix`

### 请求示例
```shell
curl -X POST "https://api.modelverse.cn/v1/videos/<task_id>/remix" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "赛博朋克风格"
  }'
```

### 正常返回
```json
{
  "id": "video_456",
  "object": "video",
  "model": "sora-2",
  "status": "queued",
  "progress": 0,
  "created_at": 1712698600,
  "size": "720x1280",
  "seconds": "8",
  "remixed_from_video_id": "video_123"
}
```

### 异常返回
```json
{
    "error": {
        "message": "Invalid param: prompt is required",
        "type": "invalid_request_error",
        "param": "6217f8ed-396d-434a-9b78-96775c0d2f99",
        "code": "param_error"
    }
}
```

