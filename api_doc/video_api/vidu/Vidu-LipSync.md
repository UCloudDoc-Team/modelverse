# Vidu/LipSync

对口型视频生成模型，支持音频驱动和文字驱动两种方式。

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                 | 类型   | 是否必选 | 描述                                                                                                                     |
| :------------------- | :----- | :------- | :----------------------------------------------------------------------------------------------------------------------- |
| model                | string | 是       | 模型名称，请固定填写：`vidu-lip-sync`                                                                                    |
| input.video_url      | string | 是       | 原视频 URL，需要公网可访问。支持 mp4、mov、avi 格式，时长 1-600 秒（建议 10-120 秒），大小不超过 5GB，编码格式需为 H.264 |
| input.audio_url      | string | 否       | 音频文件 URL（与 text 二选一）。支持 wav、mp3、wma、m4a、aac、ogg 格式，时长 1-600 秒，大小不超过 100MB                  |
| input.text           | string | 否       | 文本内容（与 audio_url 二选一）。4-2000 字符，支持停顿控制如 `你好<#2#>世界`                                             |
| input.ref_photo_url  | string | 否       | 人脸参考图 URL，用于指定视频中的目标人物。支持 jpg、jpeg、png、bmp、webp 格式，分辨率 192-4096px，大小不超过 10MB        |
| parameters.vidu_type | string | 是       | Vidu 接口类型，此处为 `lip-sync`                                                                                         |
| parameters.speed     | float  | 否       | 语速，默认 1.0，范围 [0.5, 2]，仅文字驱动时生效                                                                          |
| parameters.voice_id  | string | 否       | 音色 ID，仅文字驱动时生效。参考 [音色列表](https://shengshu.feishu.cn/sheets/EgFvs6DShhiEBStmjzccr5gonOg)                |
| parameters.volume    | int    | 否       | 音量大小，范围 0-10，默认 0（正常音量），仅文字驱动时生效                                                                |

**注意事项：**

- `audio_url` 和 `text` 必须提供其中一个
- 视频素材要求：
  - 人脸画面：要求真人出镜，人脸说话时建议正对镜头，水平转动不超过 45 度，俯仰不超过 15 度
  - 人脸尽量不遮挡，面部光线稳定
- 若不输入 `ref_photo_url`，默认选择视频中第一个有人脸的画面中人脸占比最大的人物为目标

### 请求示例（音频驱动）

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "vidu-lip-sync",
    "input": {
      "video_url": "https://umodelverse-inference.cn-wlcb.ufileos.com/%E9%94%A6%E7%91%9F.mp3",
      "audio_url": "https://umodelverse-inference.cn-wlcb.ufileos.com/%E6%AC%A2%E8%BF%8E%E4%BD%BF%E7%94%A8Modelverse_API.mp3"
    },
    "parameters": {
      "vidu_type": "lip-sync"
    }
  }'
```

### 请求示例（文字驱动）

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "vidu-lip-sync",
    "input": {
      "video_url": "https://umodelverse-inference.cn-wlcb.ufileos.com/%E9%94%A6%E7%91%9F.mp3",
      "text": "你好，欢迎使用对口型功能"
    },
    "parameters": {
      "vidu_type": "lip-sync",
      "voice_id": "your_voice_id",
      "speed": 1.0,
      "volume": 4
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
| output.task_status   | string  | 任务状态：`Pending`,`Running`,`Success`,`Failure` |
| output.urls          | array   | 视频结果的 URL 列表                               |
| output.submit_time   | integer | 任务提交时间戳                                    |
| output.finish_time   | integer | 任务完成时间戳                                    |
| output.error_message | string  | 失败时返回的错误信息                              |
| usage.duration       | integer | 视频时长（秒）                                    |
| request_id           | string  | 请求的唯一标识                                    |

### 响应示例

```json
{
  "output": {
    "task_id": "task_id",
    "task_status": "Success",
    "urls": ["https://xxxxx/xxxx-lipsync.mp4"],
    "submit_time": 1756959000,
    "finish_time": 1756959050
  },
  "usage": {
    "duration": 30
  },
  "request_id": ""
}
```