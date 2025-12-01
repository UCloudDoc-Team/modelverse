# Vidu/Img2Video

图生视频模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                          | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                             |
| :---------------------------- | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| model                         | string | 是       | 模型名称，可选值：`viduq2-pro`、`viduq2-turbo` <br> - viduq2-pro：新模型，效果好，细节丰富 <br> - viduq2-turbo：新模型，效果好，生成快                                                                                                                           |
| input.first_frame_url         | string | 是       | 首帧图片，支持图片 URL 或 Base64 编码                                                                                                                                                                                                                            |
| input.prompt                  | string | 否       | 文本提示词，用于指导视频生成，最长 2000 字符。                                                                                                                                                                                                                   |
| parameters.vidu_type          | string | 是       | Vidu 接口类型，此处为 `img2video`                                                                                                                                                                                                                                |
| parameters.duration           | int    | 否       | 视频时长                                                                                              <br>                             viduq2-pro 默认为 5，可选：1、2、3、4、5、6、7、8、9、10  <br> viduq2-turbo 默认为 5，可选：1、2、3、4、5、6、7、8、9、10 |
| parameters.seed               | int    | 否       | 随机种子，默认 0 表示使用随机数                                                                                                                                                                                                                                  |
| parameters.resolution         | string | 否       | 分辨率，viduq2: 540p/720p/1080p，viduq1: 1080p，vidu2.0/1.5: 360p/720p/1080p，默认值依据模型而定                                                                                                                                                                 |
| parameters.movement_amplitude | string | 否       | 运动幅度，可选值：`auto`、`small`、`medium`、`large`，默认 `auto`                                                                                                                                                                                                |
| parameters.bgm                | bool   | 否       | 是否添加背景音乐，默认 `false`                                                                                                                                                                                                                                   |

**注意事项：**
- 图片支持 png、jpeg、jpg、webp 格式
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
    "model": "viduq2-pro",
    "input": {
      "first_frame_url": "https://umodelverse-inference.cn-wlcb.ufileos.com/ucloud-maxcot.jpg",
      "prompt": "make it dance."
    },
    "parameters": {
      "vidu_type": "img2video",
      "duration": 5,
      "resolution": "1080p",
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
