# Kling/v3-T2V

文生视频模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                      | 类型    | 是否必选 | 描述                                                                                                                                                                                                                                                    |
| :------------------------ | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| model                     | string  | 是       | 模型名称，此处为 `kling-v3-t2v`                                                                                                                                                                                                                         |
| input.prompt              | string  | 条件必选 | 提示词，最多 2500 字符。当 `multi_shot` 为 `false` 时不能为空                                                                                                                                                                                           |
| input.negative_prompt     | string  | 否       | 反向提示词，最多 2500 字符                                                                                                                                                                                                                               |
| parameters.mode           | string  | 否       | 生成模式。`std`：标准模式（720P）；`pro`：专业模式（1080P）。默认 `std`                                                                                                                                                                                  |
| parameters.aspect_ratio   | string  | 否       | 视频长宽比，可选值：`16:9`、`9:16`、`1:1`。默认 `16:9`                                                                                                                                                                                                  |
| parameters.duration       | int     | 否       | 视频时长（秒），可选值：`3` ~ `15`，默认 `5`                                                                                                                                                                                                             |
| parameters.sound          | string  | 否       | 是否生成声音，可选值：`on`、`off`，默认 `off`                                                                                                                                                                                                            |
| parameters.multi_shot     | bool    | 否       | 是否启用多镜头模式，默认 `false`。<br>为 `true` 时 `prompt` 无效；为 `false` 时 `shot_type` 和 `multi_prompt` 无效                                                                                                                                      |
| parameters.shot_type      | string  | 条件必选 | 镜头类型，当 `multi_shot` 为 `true` 时必填，可选值：`customize`                                                                                                                                                                                          |
| parameters.multi_prompt   | array   | 条件必选 | 多镜头提示词列表，当 `multi_shot` 为 `true` 且 `shot_type` 为 `customize` 时不能为空。<br>最少 1 个镜头，最多 6 个；每个镜头内容不超过 512 字符；每个镜头时长不小于 1 秒且不大于总时长；所有镜头时长之和需等于总时长。<br>每项包含：<br>- `index`：镜头序号，从 1 开始（int）<br>- `prompt`：该镜头提示词（string）<br>- `duration`：该镜头时长（string） |
| parameters.watermark_enabled | bool | 否       | 是否生成带水印的结果                                                                                                                                                                                                                                     |
| parameters.external_task_id | string | 否      | 自定义任务 ID，不会覆盖系统生成的任务 ID，但支持通过该 ID 查询任务。请确保单用户下唯一                                                                                                                                                                   |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

**文生视频：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-t2v",
    "input": {
      "prompt": "A beautiful sunset over the ocean with waves gently crashing"
    },
    "parameters": {
      "mode": "pro",
      "aspect_ratio": "16:9",
      "duration": 5,
      "sound": "on"
    }
  }'
```

**多镜头生成：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-t2v",
    "parameters": {
      "mode": "pro",
      "aspect_ratio": "16:9",
      "duration": 5,
      "multi_shot": true,
      "shot_type": "customize",
      "multi_prompt": [
        {"index": 1, "prompt": "A person walking through a misty forest at dawn", "duration": "2"},
        {"index": 2, "prompt": "A car speeding down a rainy street, headlights glowing", "duration": "3"}
      ],
      "sound": "on"
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
