# Vidu/Reference2Video

参考生视频模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                          | 类型     | 是否必选 | 描述                                                                                                |
| :---------------------------- | :------- | :------- | :-------------------------------------------------------------------------------------------------- |
| model                         | string   | 是       | 模型名称，可选值：`viduq2`                                                                          |
| input.images                  | []string | 是       | 参考图像数组，viduq2/viduq1 支持 1-7 张，vidu2.0/vidu1.5 支持 1-3 张，支持图片 URL 或 Base64 编码   |
| input.prompt                  | string   | 是       | 文本提示词，用于指导视频生成，最长 2000 字符                                                        |
| parameters.vidu_type          | string   | 是       | Vidu 接口类型，此处为 `reference2video`                                                             |
| parameters.duration           | int      | 否       | 视频时长参数，默认值依据模型而定： <br> viduq2：默认5秒，可选：1-10                                 |
| parameters.seed               | int      | 否       | 随机种子，默认 0 表示使用随机数                                                                     |
| parameters.aspect_ratio       | string   | 否       | 长宽比，可选值：`16:9`、`9:16`、`4:3`、`3:4`、`1:1`，默认 `16:9`，注：4:3、3:4 仅支持 q2            |
| parameters.resolution         | string   | 否       | 分辨率参数，默认值依据模型和视频时长而定：<br> viduq2 （1-8秒）：默认 720p, 可选：540p、720p、1080p |
| parameters.movement_amplitude | string   | 否       | 运动幅度，可选值：`auto`、`small`、`medium`、`large`，默认 `auto`，注：q2 模型不支持该参数          |
| parameters.bgm                | bool     | 否       | 是否添加背景音乐，默认 `false`                                                                      |

**注意事项：**
- viduq2/viduq1 模型支持 1-7 张参考图片
- vidu2.0/vidu1.5 模型支持 1-3 张参考图片
- 模型将以参考图片中的主题为参考生成具备主体一致的视频
- 图片支持 png、jpeg、jpg、webp 格式
- 图片像素不能小于 128×128
- 图片比例需要小于 1:4 或者 4:1
- 图片大小不超过 50 MB
- Base64 编码格式示例：`data:image/png;base64,{base64_encode}`

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。

```shell
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
    "model": "viduq2",
    "input": {
      "images": [
        "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg",
      ],
      "prompt": "make it dance."
    },
    "parameters": {
      "vidu_type": "reference2video",
      "duration": 5,
      "aspect_ratio": "16:9",
      "resolution": "720p",
      "movement_amplitude": "auto",
      "bgm": true
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
