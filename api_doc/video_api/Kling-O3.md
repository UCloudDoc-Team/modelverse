# Kling/V3-Omni

多模态视频生成模型，支持文本、图像和视频作为多模态输入。

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                                    | 类型    | 是否必选 | 描述                                                                                                                                                                                                                                                  |
| :-------------------------------------- | :------ | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| model                                   | string  | 是       | 模型名称，此处为 `kling-v3-omni`                                                                                                                                                                                                                      |
| input.prompt                            | string  | 否       | 提示词，最多 2500 字符。当 `multi_shot` 为 false 时不能为空。<br>可通过 `<<<image_1>>>`、`<<<video_1>>>` 格式引用图片、视频                                                                                                                                 |
| input.negative_prompt                   | string  | 否       | 反向提示词，最多 2500 字符。建议直接在正向 prompt 中通过否定句添加                                                                                                                                                                                        |
| parameters.mode                         | string  | 否       | 生成模式。`std`：标准模式（720P）；`pro`：专业模式（1080P）。默认 `pro`                                                                                                                                                                                   |
| parameters.aspect_ratio                 | string  | 条件必选 | 视频长宽比，可选值：`16:9`、`9:16`、`1:1`。仅在使用视频编辑功能（`refer_type: base`）时可省略，其他场景均必填                                                                                                                                                 |
| parameters.duration                     | int     | 否       | 视频时长（秒），可选值：`3` ~ `15`，默认 `5`。<br>使用视频参考时仅支持 `3` ~ `10` 秒。<br>使用视频编辑功能（`refer_type: base`）时，输出与传入视频时长相同，此参数无效                                                                                         |
| parameters.sound                        | string  | 否       | 是否生成声音，可选值：`on`、`off`，默认 `off`。<br>有参考视频时只能为 `off`                                                                                                                                                                                |
| parameters.multi_shot                   | bool    | 否       | 是否启用多镜头模式，默认 `false`。<br>为 `true` 时 `prompt` 无效；为 `false` 时 `shot_type` 和 `multi_prompt` 无效                                                                                                                                       |
| parameters.shot_type                    | string  | 否       | 镜头类型，当 `multi_shot` 为 `true` 时必填，可选值：`customize`                                                                                                                                                                                           |
| parameters.multi_prompt                 | array   | 否       | 多镜头提示词列表，当 `multi_shot` 为 `true` 且 `shot_type` 为 `customize` 时不能为空。<br>最少 1 个镜头，最多 6 个；每个镜头内容不超过 512 字符；每个镜头时长不小于 1 秒且不大于总时长；所有镜头时长之和需等于总时长。<br>每项包含：<br>- `index`：镜头序号，从 1 开始（int）<br>- `prompt`：该镜头提示词（string）<br>- `duration`：该镜头时长（string） |
| parameters.image_list                   | array   | 否       | 参考图片列表，支持首帧/尾帧/参考图。每项包含：<br>- `image_url`：图片 URL 或 Base64（string，必填）<br>- `type`：图片类型，可选值 `first_frame`、`end_frame`（string，可选）<br><br>约束：<br>- 有参考视频时，图片数量 ≤ 4<br>- 无参考视频时，图片数量 ≤ 7<br>- 图片数量超过 2 时不支持设置尾帧<br>- 暂不支持仅尾帧（有尾帧时必须有首帧） |
| parameters.video_list                   | array   | 否       | 参考视频列表，最多 1 段。每项包含：<br>- `video_url`：视频 URL（string，必填，支持 MP4/MOV）<br>- `refer_type`：参考类型，`feature`（特征参考）或 `base`（待编辑），默认 `base`<br>- `keep_original_sound`：是否保留原声，`yes` 或 `no`<br><br>使用视频参考时仅支持 3-10 秒。待编辑视频时不能定义首尾帧。有参考视频时 `sound` 只能为 `off` |
| parameters.watermark_enabled            | bool    | 否       | 是否生成带水印的结果，暂不支持自定义水印                                                                                                                                                                                                                   |
| parameters.external_task_id             | string  | 否       | 自定义任务 ID，不会覆盖系统生成的任务 ID，但支持通过该 ID 查询任务。请确保单用户下唯一                                                                                                                                                                        |

**图片格式要求**：
- 支持格式：`.jpg`、`.jpeg`、`.png`
- 文件大小：不超过 10MB
- 尺寸要求：宽高最小 300px，宽高比在 1:2.5 ~ 2.5:1 之间
- 使用 Base64 编码时，不要添加 `data:image/png;base64,` 等前缀

**视频格式要求**：
- 支持格式：MP4、MOV
- 视频时长不少于 3 秒
- 宽高尺寸介于 720px ~ 2160px
- 帧率 24fps ~ 60fps，生成视频输出为 24fps
- 最多 1 段视频，大小不超过 200MB

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

**文生视频：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-omni",
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

**图片参考 + 首尾帧：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-omni",
    "input": {
      "prompt": "A girl walking through a garden"
    },
    "parameters": {
      "mode": "pro",
      "aspect_ratio": "16:9",
      "duration": 5,
      "image_list": [
        {"image_url": "https://example.com/first_frame.jpg", "type": "first_frame"},
        {"image_url": "https://example.com/last_frame.jpg", "type": "end_frame"}
      ]
    }
  }'
```

**视频编辑（待编辑视频）：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-omni",
    "input": {
      "prompt": "Add a cinematic color grading effect"
    },
    "parameters": {
      "mode": "pro",
      "video_list": [
        {
          "video_url": "https://example.com/input.mp4",
          "refer_type": "base",
          "keep_original_sound": "yes"
        }
      ]
    }
  }'
```

**多镜头 + 图片引用：**

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "kling-v3-omni",
    "parameters": {
      "mode": "pro",
      "aspect_ratio": "16:9",
      "duration": 5,
      "multi_shot": true,
      "shot_type": "customize",
      "multi_prompt": [
        {"index": 1, "prompt": "<<<image_1>>>A person sitting on a park bench, sunlight filtering through trees", "duration": "2"},
        {"index": 2, "prompt": "A car speeding down a rainy street, headlights glowing", "duration": "3"}
      ],
      "image_list": [
        {"image_url": "https://example.com/person.jpg"}
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

| 错误码    | 描述             |
| :-------- | :--------------- |
| 006001094 | 任务资源不足     |
| 006001095 | 任务响应错误     |
| 006001099 | 任务创建错误     |
