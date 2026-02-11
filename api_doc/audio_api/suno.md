# suno

音乐生成模型

## 异步提交任务

### 接口

`https://api.modelverse.cn/v1/tasks/submit`

### 输入

| 参数                          | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                          |
| :---------------------------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| model                         | string | 是       | 模型名称，此处对应suno模型版本：<br> `suno/chirp-v4`<br>`suno/chirp-auk`<br>`suno/chirp-bluejay`<br>`suno/chirp-crow`|
| input.prompt                  | string | 否       | 歌词 (自定义模式专用)|
| input.gpt_description_prompt  | string | 否       | 灵感模式提示词(灵感模式专用)|
| parameters.tags               | string | 否       | 风格标签(自定义模式专用)   |
| parameters.title              | string | 否       | 标题(自定义模式专用)         |
| parameters.make_instrumental  | bool   | 否       | 是否生成纯音乐，true 为生成纯音乐 |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
<!-- tabs:start -->
#### ** 灵感模式 **
```bash
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
        "model": "suno/chirp-bluejay",
        "input": {
            "gpt_description_prompt": "一首关于乡愁的歌"
        }
    }'
```
#### ** 纯音乐灵感模式 **
```bash
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
        "model": "suno/chirp-bluejay",
        "input": {
            "gpt_description_prompt": "一首关于乡愁的歌"
        },
        "parameters": {
            "make_instrumental": true
        }
    }'
```
#### ** 自定义歌词歌名 **
```bash
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
  "model": "suno/chirp-bluejay",
  "input": {
    "prompt": "[Verse]\n连续的日子一直忙碌\n文件成堆无尽头\n把梦想藏在抽屉深处\n咖啡杯已经冷透\n\n[Verse 2]\n早上八点打卡上班\n疲惫的眼睛没神采\n同事间的闲聊都没意思\n只盼着时间快快跑起来\n\n[Chorus]\n工作工作老板的呼喊\n做完做完这才算平安\n加班加班才有些钱赚\n梦想梦想何时能实现\n\n[Verse 3]\n午餐时间吃个便当\n看窗外阳光正灿烂\n生活离梦想好远\n眼前只有办公桌和椅子\n\n[Bridge]\n老板的脚步声像雷鸣\n心跳随着节奏加速\n桌上的文件一大堆\n抱怨的声音渐渐消失\n\n[Chorus]\n工作工作老板的呼喊\n做完做完这才算平安\n加班加班才有些钱赚\n梦想梦想何时能实现"
  },
  "parameters": {
    "tags": "pop, ballad",
    "title": "归乡"
  }
}'
```
#### ** 纯音乐自定义 **
```bash
curl --location --globoff 'https://api.modelverse.cn/v1/tasks/submit' \
--header 'Authorization: <YOUR_API_KEY>' \
--header 'Content-Type: application/json' \
--data '{
  "model": "suno/chirp-bluejay",
  "input": {
    "prompt": ""
  },
  "parameters": {
    "tags": "pop, ballad",
    "title": "归乡"
  }
}'
```
<!-- tabs:end -->

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
