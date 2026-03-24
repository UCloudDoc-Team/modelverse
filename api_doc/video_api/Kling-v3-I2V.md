# Kling/v3-I2V

图生视频模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                         | 类型    | 是否必选 | 描述                                                                                                                                                                                                                                                    |
| :--------------------------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| model                        | string  | 是       | 模型名称，此处为 `kling-v3-i2v`                                                                                                                                                                                                                         |
| parameters.image             | string  | 条件必选 | 参考图像，支持图片 Base64 编码或图片 URL（确保可访问）。与 `image_tail` 至少二选一，不能同时为空。<br>Base64 编码时请勿添加 `data:image/png;base64,` 等前缀，直接传入编码字符串即可。<br>- 图片格式支持 `.jpg`、`.jpeg`、`.png`<br>- 文件大小不超过 10MB<br>- 宽高尺寸不小于 300px，宽高比介于 1:2.5 ~ 2.5:1 之间 |
| parameters.image_tail        | string  | 条件必选 | 视频尾帧图片，支持 URL 或 Base64 编码。与 `image` 至少二选一。具体格式要求参照 `image` 参数                                                                                                                                                              |
| input.prompt                 | string  | 条件必选 | 提示词，最多 2500 字符。当 `multi_shot` 为 `false` 时不能为空                                                                                                                                                                                           |
| input.negative_prompt        | string  | 否       | 反向提示词，最多 2500 字符                                                                                                                                                                                                                               |
| parameters.mode              | string  | 否       | 生成模式。`std`：标准模式（720P）；`pro`：专业模式（1080P）。默认 `std`                                                                                                                                                                                  |
| parameters.duration          | int     | 否       | 视频时长（秒），可选值：`3` ~ `15`，默认 `5`                                                                                                                                                                                                             |
| parameters.sound             | string  | 否       | 是否生成声音，可选值：`on`、`off`，默认 `off`                                                                                                                                                                                                            |
| parameters.multi_shot        | bool    | 否       | 是否启用多镜头模式，默认 `false`。<br>为 `true` 时 `prompt` 无效；为 `false` 时 `shot_type` 和 `multi_prompt` 无效                                                                                                                                      |
| parameters.shot_type         | string  | 条件必选 | 镜头类型，当 `multi_shot` 为 `true` 时必填，可选值：`customize`                                                                                                                                                                                          |
| parameters.multi_prompt      | array   | 条件必选 | 多镜头提示词列表，当 `multi_shot` 为 `true` 且 `shot_type` 为 `customize` 时不能为空。<br>最少 1 个镜头，最多 6 个；每个镜头内容不超过 512 字符；每个镜头时长不小于 1 秒且不大于总时长；所有镜头时长之和需等于总时长。<br>每项包含：<br>- `index`：镜头序号，从 1 开始（int）<br>- `prompt`：该镜头提示词（string）<br>- `duration`：该镜头时长（string） |
| parameters.watermark_enabled | bool    | 否       | 是否生成带水印的结果                                                                                                                                                                                                                                     |
| parameters.external_task_id  | string  | 否       | 自定义任务 ID，不会覆盖系统生成的任务 ID，但支持通过该 ID 查询任务。请确保单用户下唯一                                                                                                                                                                   |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

**图生视频（首帧）：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-i2v",
    "input": {
      "prompt": "The image comes to life with gentle movement"
    },
    "parameters": {
      "mode": "pro",
      "duration": 5,
      "image": "https://example.com/first_frame.jpg"
    }
  }'
```

**首尾帧控制：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-i2v",
    "input": {
      "prompt": "A girl walking through a garden"
    },
    "parameters": {
      "mode": "pro",
      "duration": 5,
      "image": "https://example.com/first_frame.jpg",
      "image_tail": "https://example.com/last_frame.jpg"
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
