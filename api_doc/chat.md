# API通用说明

## 功能介绍
本接口用于调用 ModelVerse 平台上的大模型，实现智能对话功能。

## 模型列表
| 模型ID | 模型版本 
| --- |  --- 
| deepseek-ai/DeepSeek-R1 | DeepSeek-R1 
| deepseek-ai/DeepSeek-V3-0324| DeepSeek-V3-0324 
| deepseek-ai/DeepSeek-Prover-V2-671B| DeepSeek-Prover-V2-671B 
| Qwen/QwQ-32B| QwQ-32B 
| Qwen/Qwen3-235B-A22B| QwQ3-235B


## 第一步：获取 API Key
如何获取api_key值：请进入Umodelverse控制台 -「体验中心」- [API Key管理](https://console.ucloud.cn/modelverse/experience/api-keys) 进行快速创建。
![](https://www-s.ucloud.cn/2025/03/a427b4a6c0ff2d4dc2f2ee3cdad95098_1743154241648.png)

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
| messages | List[message] | 是 | 聊天上下文信息。说明：   <br/>（1）messages成员不能为空，1个成员表示单轮对话，多个成员表示多轮对话，例如：   · 1个成员示例，`"messages": [ {"role": "user","content": "你好"}]`<br/>   · 3个成员示例，`"messages": [ {"role": "user","content": "你好"},{"role":"assistant","content":"需要什么帮助"},{"role":"user","content":"自我介绍下"}]`<br/>    （2） 最后一个message为当前请求的信息，前面的message为历史对话信息   （3）messages的role说明：   ① 第一条message的role必须是user或system   ② 最后一条message的role必须是user或tool   ③ 如果未使用function call功能：   · 当第一条message的role为user，role值需要依次为user -> assistant -> user...，即奇数位message的role值必须为user或function，偶数位message的role值为assistant，例如：示例中message中的role值分别为user、assistant、user、assistant、user；奇数位（红框）message中的role值为user，即第1、3、5个message中的role值为user；偶数位（蓝框）值为assistant，即第2、4个message中的role值为assistant   ![](https://www-s.ucloud.cn/2025/02/e3d47d50249ea9eb3f6194524f84b500_1739116591466.png)|
| stream | bool | 否 | 是否以流式接口的形式返回数据，说明：   <br/>（1）beam search模型只能为false   <br/>（2）默认false |
| stream_options | stream_options | 否 | 流式响应是否输出usage，说明：true：是，设置为true时，在最后一个chunk会输出一个字段，这个chunk上的usage字段显示整个请求的token统计信息; false：否，流式响应默认不输出usage |
| temperature | float | 否 | 说明：   <br/>（1）较高的数值会使输出更加随机，而较低的数值会使其更加集中和确定 |
| top_p | float | 否 | 说明：   <br/>（1）影响输出文本的多样性，取值越大，生成文本的多样性越强   <br/>（2）默认0.7，取值范围 [0, 1.0] |
| penalty_score | float | 否 | 通过对已生成的token增加惩罚，减少重复生成的现象。说明：   <br/>（1）值越大表示惩罚越大   <br/>（2）默认1.0，取值范围：[1.0, 2.0] |
| max_completion_tokens | int | 否 | 指定模型最大输出token数，说明：   <br/>（1）该参数取值，请查看本文[支持模型列表-max_completion_tokens取值范围]<br/>列。 |
| seed | int | 否 | 说明：   <br/>（1）取值范围: （0,2147483647‌），会由模型随机生成，默认值为空   <br/>（2）如果指定，系统将尽最大努力进行确定性采样，以便使用相同seed和参数的重复请求返回相同的结果 |
| stop | List<string> | 否 | 生成停止标识，当模型生成结果以stop中某个元素结尾时，停止文本生成。说明：   <br/>（1）每个元素长度不超过20字符   <br/>（2）最多4个元素 |
| user | string | 否 | 表示最终用户的唯一标识符 |
| frequency_penalty | float | 否 | 说明：   <br/>（1）正值根据迄今为止文本中的现有频率对新token进行惩罚，从而降低模型逐字重复同一行的可能性   <br/>（2）取值范围：[-2.0, 2.0] |
| presence_penalty | float | 否 | 说明：   <br/>（1）正值根据token记目前是否出现在文本中来对其进行惩罚，从而增加模型谈论新主题的可能性   <br/>（2）取值范围：[-2.0, 2.0] |
| web_search | [web_search] | 否 | 搜索增强的选项，说明：   <br/>（1）默认不传关闭   |
| metadata | map<string,string> | 否 | 说明：   <br/>（1）元素个数最大支持16个   <br/>（2）key和value必须都是string类型 |




### 请求示例
```bash
curl --location 'https://deepseek.modelverse.cn/v1/chat/completions' \
--header 'Authorization: Bearer <你的API Key>' \
--header 'Content-Type: application/json' \
--data '{
    "stream": true,
    "model": "deepseek-ai/DeepSeek-R1",
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
| 名称 | 类型 | 描述 |
|-|-|-|
| id | string | 本次请求的唯一标识，可用于排查问题 |
| object | string | 回包类型 `chat.completion`：多轮对话返回 |
| created | int | 时间戳 |
| model | string | 说明：<br>(1) 如果是预置服务，返回模型ID<br>(2) 如果是sft后部署的服务，该字段返回`model:modelversionID`，model与请求参数相同，是本次请求使用的大模型ID；modelversionID用于溯源 |
| choices | choices/sse_choices | stream=false时，返回内容<br>stream=true时，返回内容 |
| usage | usage | token统计信息，说明：<br>(1) 同步请求默认返回<br>(2) 流式请求默认返回，当开启`stream_options.include_usage=true`时，会在最后一个chunk返回实际内容，其他chunk返回null|
| search_results    | search_results | 搜索结果 |

### 响应示例
```json
{
    "id": "  ",
    "object": "chat.completion",
    "created":  ,
    "model": "deepseek-ai/DeepSeek-R1",
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





