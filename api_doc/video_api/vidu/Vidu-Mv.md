# Vidu/一键生成mv

一键生成MV

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                 | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :------------------- | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| model                     | string        | 是       | 模型名称，请固定填写：`vidu-mv`    |
| input.images              | Array[String] | 是       | 用户生成mv时的模特图片或风格图片<br> 注1：支持传入图片 Base64 编码或图片URL（确保可访问）；<br> 注2：支持输入 1~7 张图；<br> 注3：图片支持 png、jpeg、jpg、webp格式；<br> 注4：图片比例需要小于 1:4 或者 4:1 ；<br> 注5：图片大小不超过 50 MB；<br> 注6：请注意，http请求的post body不超过 20MB，且编码必须包含适当的内容类型字符串，例如：<br>data:image/png;base64,{base64_encode} |
| input.audio_url           | string        | 是       | 需要生成mv的音频<br>注1：支持传入 Base64 编码或音频URL（确保可访问）；<br>注2：支持输入 1 个音频；<br>注3：支持 mp3、wav、aac、m4a格式；<br>注4：音频最少10秒，最多180秒；<br>注5：请注意，http请求的post body不超过20MB，且编码必须包含适当的内容类型字符串，例如：<br>data:audio/mp3;base64,{base64_encode} |
| input.prompt              | string        | 否       | 用户提示词<br>生成mv视频的文本描述。<br>注1：字符长度不能超过 3000 个字符 |
| parameters.vidu_type      | string        | 是       | Vidu 接口类型，此处为 `one-click/mv`    |
| parameters.aspect_ratio   | string        | 否       | 输出比例，默认16:9<br>可选值 1:1，16:9，9:16，4:3，3:4    |
| parameters.resolution     | string        | 否       | 清晰度，默认720p<br>可选值：540p、720p、1080p|
| parameters.add_subtitle   | bool          | 否       | 是否需要字幕，默认为 false<br>true：需要字幕<br>false：不需要字幕       |
| parameters.language       | string        | 否       | 音频语言类型，默认为自动<br>可选 en、zh|
| parameters.srt_url        | string        | 否       | 字幕文件url      |


### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "vidu-mv",
    "input": {
        "images": ["https://xxxxxxxxx/image2video.png"],
        "prompt": "图中人物徘徊在路上。",
        "audio_url": "https://xxxxxxxxx/49ab8a8f-f564-4b96-80a2-b38cca527475.mp3"
    },
    "parameters": {
      "vidu_type": "one-click/mv",
      "aspect_ratio": "16:9",
      "resolution": "540p",
      "add_subtitle": true,
      "language": "en",
      "srt_url": "https://xxxxxxxxx/mv_subtitle.srt"
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

### 响应示例（成功）

```json
{
  "output": {
    "task_id": "task_id",
    "task_status": "Success",
    "urls": ["https://xxxxx/xxxx-mv.mp4"],
    "submit_time": 1756959000,
    "finish_time": 1756959050
  },
  "usage": {
    "duration": 30
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