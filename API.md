# API通用说明

## 功能介绍
本接口用于调用 ModelVerse 平台上的大模型，实现智能对话功能。

## 支持模型列表
| 模型名称 | 模型版本 | 最大输出长度
| --- | --- | ----
| DeepSeek-Reasoner | DeepSeek-R1 | 12288
| DeepSeek-Chat | DeepSeek-V3 | 12288

## 第一步：获取 API Key
打开API列表页面[API列表](https://console.ucloud.cn/uapi/detail?id=GetUMInferService)，无需填写参数，点击「发送请求」
![api1.png](https://www-s.ucloud.cn/2025/02/ae9774428a03a2f52b5c0c904d1cea6b_1739186338233.png)
- 点击弹窗的「确认发送请求」
![api2.png](https://www-s.ucloud.cn/2025/02/06b4aca355f8952569c46e11db71e48b_1739186338242.png)
- 从返回的列表中根据模型名称选择您需要的Key
![api3.png](https://www-s.ucloud.cn/2025/02/67caa1e2c510d270c76b07af9eebc32b_1739186338249.png)

## 第二步：Chat API调用
## 请求
### 请求头域
| 名称 | 类型 | 类型 | 描述 |
| --- | --- | --- | --- |
| Content-Type | string | 是 | 固定值application/json |
| Authorization | string | 是 | 传入第一步中API获取的Key |


### 请求参数
| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| model | string | 是 | 模型ID|
| messages | List[message] | 是 | 聊天上下文信息。说明：   （1）messages成员不能为空，1个成员表示单轮对话，多个成员表示多轮对话，例如：   · 1个成员示例，`"messages": [ {"role": "user","content": "你好"}]`<br/>   · 3个成员示例，`"messages": [ {"role": "user","content": "你好"},{"role":"assistant","content":"需要什么帮助"},{"role":"user","content":"自我介绍下"}]`<br/>    （2） 最后一个message为当前请求的信息，前面的message为历史对话信息   （3）messages的role说明：   ① 第一条message的role必须是user或system   ② 最后一条message的role必须是user或tool   ③ 如果未使用function call功能：   · 当第一条message的role为user，role值需要依次为user -> assistant -> user...，即奇数位message的role值必须为user或function，偶数位message的role值为assistant，例如：示例中message中的role值分别为user、assistant、user、assistant、user；奇数位（红框）message中的role值为user，即第1、3、5个message中的role值为user；偶数位（蓝框）值为assistant，即第2、4个message中的role值为assistant   ![](https://www-s.ucloud.cn/2025/02/e3d47d50249ea9eb3f6194524f84b500_1739116591466.png)|
| stream | bool | 否 | 是否以流式接口的形式返回数据，说明：   （1）beam search模型只能为false   （2）默认false |
| stream_options | stream_options | 否 | 流式响应是否输出usage，说明：true：是，设置为true时，在最后一个chunk会输出一个字段，这个chunk上的usage字段显示整个请求的token统计信息; false：否，流式响应默认不输出usage |


### 请求示例
```bash
curl --location 'https://deepseek.modelverse.cn/v1/chat/completions' \
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "reasoning_effort": "low",
    "stream": true,
    "model": "deepseek-r1",
    "messages": [
        {
            "role": "user",
            "content": "say hello to ucloud"
        }
    ]
}'
```


## 响应

### 响应参数
| 名称              | 类型          | 描述                                                                                                                                  |
|-------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------|
| id                | string        | 本次请求的唯一标识，可用于排查问题                                                                                                     |
| object            | string        | 回包类型 `chat.completion`：多轮对话返回                                                                                               |
| created           | int           | 时间戳                                                                                                                              |
| model             | string        | 说明：<br>(1) 如果是预置服务，返回模型ID<br>(2) 如果是sft后部署的服务，该字段返回`model:modelversionID`，model与请求参数相同，是本次请求使用的大模型ID；modelversionID用于溯源 |
| choices           | choices/sse_choices | stream=false时，返回内容<br>stream=true时，返回内容                                                                                   |
| usage             | usage         | token统计信息，说明：<br>(1) 同步请求默认返回<br>(2) 流式请求默认不返回，当开启`stream_options.include_usage=true`时，会在最后一个chunk返回实际内容，其他chunk返回null |
| search_results    | search_results | 搜索结果列表                                                                                                                         |

### 响应示例
```json
{
    "id": "  ",
    "object": "chat.completion",
    "created":  ,
    "model": "models/DeepSeek-R1",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "\n\nHello, UCloud! 👋 If there's anything specific you'd like to know or discuss about UCloud's services (like cloud computing, storage, AI solutions, etc.), feel free to ask! 😊",
                "reasoning_content": "\nOkay, the user wants to say hello to UCloud. Let me start by greeting UCloud directly.\n\nHmm, should I mention what UCloud is? Maybe a brief intro would help, like it's a cloud service provider.\n\nThen, I can ask if there's anything specific the user needs help with regarding UCloud services.\n\nKeeping it friendly and open-ended makes sense for a helpful response.\n"
            },
            "finish_reason": "stop"
    ],
    "usage": {
        "prompt_tokens": 8,
        "completion_tokens": 129,
        "total_tokens": 137,
        "prompt_tokens_details": null,
        "completion_tokens_details": null
    },
    "system_fingerprint": ""
}
```


## 错误码
如果请求错误，服务器返回的JSON文本包含以下参数。

| HTTP 状态码 | 类型              | 错误码            | 错误信息                        | 描述                                                                 |
|------------|-------------------|-------------------|----------------------------------|----------------------------------------------------------------------|
| 400        | invalid_request_error | invalid_messages   | 信息敏感                        | 消息敏感                                                             |
| 400        | invalid_request_error | characters_too_long | 对话 token 输出限制              | 目前 deepseek 系列模型支持的最大 max_tokens 为 12288               |
| 400        | invalid_request_error | tokens_too_long    | Prompt tokens too long           | 【用户输入错误】请求内容超过大模型内部限制，即用户输入大模型内容过长，可以尝试以下方法解决：<br>• 适当缩短输入 |
| 400        | invalid_request_error | invalid_token      | Validate Certification failed    | bearer token 无效，用户可以参考【鉴权说明】获取最新密钥               |
| 400        | invalid_request_error | invalid_model      | No permission to use the model   | 没有模型权限                                                         |



