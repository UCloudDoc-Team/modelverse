# Kling/v3-Motion-Control

运动控制视频生成模型，通过参考图片和参考视频生成具有指定运动控制的视频。

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                         | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                  |
| :--------------------------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| model                        | string | 是       | 模型名称，此处为 `kling-v3-motion-control`                                                                                                                                                                                                                                                            |
| input.img_url                | string | 是       | 参考图片，支持图片 Base64 编码或图片 URL（确保可访问）。<br>- 图片格式支持 `.jpg`、`.png`<br>- 文件大小不超过 10MB<br>- 宽高尺寸介于 300px ~ 65536px 之间<br>- 宽高比介于 1:2.5 ~ 2.5:1 之间                                                                                                          |
| input.video_url              | string | 是       | 参考视频 URL，支持 MP4、MOV 格式。<br>- 文件大小不超过 100MB<br>- 宽高尺寸介于 340px ~ 3850px 之间<br>- 视频时长不少于 3 秒；横向视频时长不超过 30 秒，竖向视频时长不超过 10 秒                                                                                                                       |
| parameters.character_orientation | string | 是   | 角色朝向，决定角色面朝方向。可选值：`image`（朝向与参考图一致）、`video`（朝向与参考视频一致）                                                                                                                                                                                                        |
| parameters.mode              | string | 是       | 生成模式。`std`：标准模式；`pro`：专业模式（高质量）                                                                                                                                                                                                                                                  |
| input.prompt                 | string | 否       | 提示词，最多 2500 字符                                                                                                                                                                                                                                                                                |
| parameters.duration          | int    | 否       | 视频时长（秒），可选值：`5`、`10`，默认 `5`                                                                                                                                                                                                                                                           |
| parameters.aspect_ratio      | string | 否       | 视频宽高比，可选值：`16:9`、`9:16`、`1:1`，默认 `16:9`                                                                                                                                                                                                                                               |
| parameters.keep_original_sound | string | 否     | 是否保留参考视频原声，可选值：`yes`、`no`，默认 `yes`                                                                                                                                                                                                                                                 |
| parameters.watermark_enabled | bool   | 否       | 是否生成带水印的结果                                                                                                                                                                                                                                                                                  |
| parameters.external_task_id  | string | 否       | 自定义任务 ID，不会覆盖系统生成的任务 ID，但支持通过该 ID 查询任务。请确保单用户下唯一                                                                                                                                                                                                                |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

```shell
curl -X POST 'https://api.modelverse.cn/v1/tasks/submit' \
-H 'Authorization: <YOUR_API_KEY>' \
-H 'Content-Type: application/json' \
-d '{
    "model": "kling-v3-motion-control",
    "input": {
      "img_url": "https://example.com/character.jpg",
      "video_url": "https://example.com/motion_reference.mp4",
      "prompt": "A person dancing gracefully"
    },
    "parameters": {
      "duration": 5,
      "aspect_ratio": "16:9",
      "character_orientation": "image",
      "mode": "std"
    }
  }'
```

### 输出

| 参数           | 类型   | 描述               |
| :------------- | :----- | :----------------- |
| output.task_id | string | 异步任务的唯一标识 |
| request_id     | string | 请求的唯一标识     |

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

| 参数                 | 类型    | 描述                                              |
| :------------------- | :------ | :------------------------------------------------ |
| output.task_id       | string  | 异步任务的唯一标识                                |
| output.task_status   | string  | 任务状态：`Pending`、`Running`、`Success`、`Failure` |
| output.urls          | array   | 视频结果的 URL 列表                               |
| output.submit_time   | integer | 任务提交时间戳                                    |
| output.finish_time   | integer | 任务完成时间戳                                    |
| output.error_message | string  | 失败时返回的错误信息                              |
| usage.duration       | integer | 视频时长（秒）                                    |
| request_id           | string  | 请求的唯一标识                                    |

### 响应示例（成功）

```json
{
  "output": {
    "task_id": "task_id",
    "task_status": "Success",
    "urls": ["https://xxxxx/xxxx.mp4"],
    "submit_time": 1756959000,
    "finish_time": 1756959050
  },
  "usage": {
    "duration": 5
  },
  "request_id": ""
}
```

### 响应示例（失败）

```json
{
  "output": {
    "task_id": "task_id",
    "task_status": "Failure",
    "submit_time": 1756959000,
    "finish_time": 1756959019,
    "error_message": "error_message"
  },
  "usage": {
    "duration": 5
  },
  "request_id": ""
}
```

## 错误码

| 错误码    | 描述         |
| :-------- | :----------- |
| 006001094 | 任务资源不足 |
| 006001095 | 任务响应错误 |
| 006001099 | 任务创建错误 |
