# suno-cover

音乐翻唱（cover），可以翻唱上传的音乐，也可以翻唱suno音乐生成的音乐，本文先介绍音乐上传接口，然后介绍翻唱接口的使用。

## 音乐上传接口

`https://api.modelverse.cn/v1/suno/uploads/audio-url`

### 输入

| 参数                          | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                          |
| :---------------------------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| model                | string | 是       | 模型名称，此处填：`suno-uploads`|
| url                  | string | 是       | 需要上传的歌曲url |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
```bash
curl --location --request POST 'https://api.modelverse.cn/v1/suno/uploads/audio-url' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YOUR_API_KEY>' \
-d '{
    "model": "suno-uploads",
    "url": "https://xxxx/test.mp3"
}'
```
### 输出

| 参数           | 类型   | 描述               |
| :------------- | :----- | :----------------- |
| code        | string | 上传结果 |
| message     | string | 失败原因     |
| data        | string | 歌曲片段标识 `clip_id`，翻唱是需要填入到`cover_clip_id`|

### 响应示例

```json
{"code":"success","message":"","data":"xxxxxxx-xxxxx"}
```

## 翻唱异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                          | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                          |
| :---------------------------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| model                         | string | 是       | 模型名称，此处对应suno模型版本：<br> `suno-v4`<br>`suno-v4.5`<br>`suno-v4.5+`<br>`suno-v5`|
| input.prompt                  | string | 否       | 歌词 |
| parameters.generation_type    | string | 是       | 生成类型,此处填`TEXT`|
| parameters.tags               | string | 是       | 音乐风格标签   |
| parameters.negative_tags      | string | 否       | 要避免的标签         |
| parameters.title              | string | 是       | 音乐提交的标题 |
| parameters.task               | string | 是       | 任务类型，此处填`cover` |
| parameters.task_id            | string | 是       | suno音乐生成时的任务ID,或者上传音乐得到的`data` |
| parameters.cover_clip_id      | string | 是       | 音乐片段标识，可以通过生成音乐获取其中的一首歌的`clip_id`值；也可以通过上传接口得到`data`填入此处，这样就可以 cover 自定义音频 |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
```bash
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: Bearer <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
  "model": "suno-v4.5+",
  "input": {
    "prompt": "[Verse]主歌歌词[Chorus]副歌歌词[Bridge]桥段歌词[Outro]结尾歌词"
  },
  "parameters": {
    "generation_type": "TEXT",
    "negative_tags":"",
    "tags": "rock, punk",
    "title": "歌曲名(Cover)",
    "task_id": "xxx-xx-xxx-xx",
    "cover_clip_id": "xxx-xx-xxx-xx",
    "task": "cover"
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
| request_id           | string  | 请求的唯一标识                                    |

### 响应示例（成功）

```json
{
  "output": {
    "task_id": "task_id",
    "task_status": "Success",
    "urls": ["https://xxxxx/xxxx.mp3"],
    "submit_time": 1756959000,
    "finish_time": 1756959050
  },
  "usage": {
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
  "request_id": ""
}
```
