数据类型如下。

## message说明
| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| role | string | 是 | 当前支持以下：   <br/>· user: 表示用户   <br/>· assistant: 表示对话助手   <br/>· system：表示人设 |
| name | string | 否 | message名 |
| content | string | 是 | 对话内容，说明：   <br/>（1）不能为空   <br/>（2）最后一个message对应的content不能为blank字符，如空格、"\n"、“\r”、“\f”等 |
| tool_calls | List[ToolCall]| 否 | 函数调用，function call场景下第一轮对话的返回，第二轮对话作为历史信息在message中传入 |
| tool_call_id | string | 否 | 说明：   <br/>（1）当role=tool时，该字段必填   <br/>（2）模型生成的function call id，对应tool_calls中的tool_calls[].id   <br/>（3）调用方应该传递真实的、由模型生成id，否则效果有损 |


## stream_options说明
| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| include_usage | bool | 否 | 流式响应是否输出usage，说明：   <br/>· true：是，设置为true时，在最后一个chunk会输出一个字段，这个chunk上的usage字段显示整个请求的token统计信息   <br/>· false：否，流式响应默认不输出usage |


## web_search说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| enable | bool | 是否开启联网搜索功能，说明：   <br/>（1）如果关闭实时搜索，角标和溯源信息都不会返回   <br/>（2）可选值：   · true：开启   · false：关闭，默认false |


## choices说明
当stream=false时，返回内容如下：

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| index | int | choice列表中的序号 |
| message | [message] | 响应信息，当stream=false时返回 |
| finish_reason | string | 输出内容标识，说明：   <br/>· normal：输出内容完全由大模型生成，未触发截断、替换   <br/>· stop：输出结果命中入参stop中指定的字段后被截断   <br/>· length：达到了最大的token数   <br/>· content_filter：输出内容被截断、兜底、替换为**等   <br/>· tool_calls：函数调用 |
| flag | int | 安全细分类型，说明：   <br/>当stream=false，flag值含义如下：   <br/>· 0或不返回：安全   <br/>· 1：低危不安全场景，可以继续对话   <br/>· 2：禁聊：不允许继续对话，但是可以展示内容   <br/>· 3：禁止上屏：不允许继续对话且不能上屏展示   <br/>· 4：撤屏 |
| ban_round | int | 当flag 不为 0 时，该字段会告知第几轮对话有敏感信息；如果是当前问题，ban_round = -1 |


## choices中message说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| role | string | 当前支持以下：   <br/>· user: 表示用户   <br/>· assistant: 表示对话助手   <br/>· system：表示人设 |
| name | string | message名 |
| content | string | 对话内容 |
| tool_calls | List[ToolCall] | 函数调用，function call场景下第一轮对话的返回，第二轮对话作为历史信息在message中传入 |
| tool_call_id | string | 说明：   <br/>（1）当role=tool时，该字段必填   <br/>（2）模型生成的function call id，对应tool_calls中的tool_calls[].id   <br/>（3）调用方应该传递真实的、由模型生成id，否则效果有损 |
| reasoning_content | string | 思维链内容，说明：只有当模型为DeepSeek-R1有效 |


## sse_choices说明
当stream=true时，返回内容如下：

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| index | int | choice列表中的序号 |
| delta | [delta] | 响应信息，当stream=true时返回 |
| finish_reason | string | 输出内容标识，说明：   <br/>· normal：输出内容完全由大模型生成，未触发截断、替换   <br/>· stop：输出结果命中入参stop中指定的字段后被截断   <br/>· length：达到了最大的token数   <br/>· content_filter：输出内容被截断、兜底、替换为**等   <br/>· tool_calls：函数调用 |
| flag | int | 安全细分类型，说明：当stream=true时，返回flag表示触发安全 |
| ban_round | int | 当flag 不为 0 时，该字段会告知第几轮对话有敏感信息；如果是当前问题，ban_round = -1 |


## delta说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| content | string | 流式响应内容 |
| reasoning_content | string | 思维链内容，说明：只有当模型为DeepSeek-R1有效 |
| tool_calls | List[ToolCall] | 由模型生成的函数调用，包含函数名称，和调用参数 |


## usage说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| prompt_tokens | int | 问题tokens数（包含历史QA） |
| prompt_tokens_details | int | 问题token详情，说明：当调用对话Chat API返回此参数 |
| completion_tokens | int | 回答tokens数，说明：当调用对话Chat API返回此参数 |
| total_tokens | int | 总tokens数 |


## search_results说明
[bocha](https://aq6ky2b8nql.feishu.cn/wiki/RXEOw02rFiwzGSkd9mUcqoeAnNK)

若联网失败，则返回：
```json
{
    "xxx": "xxx",
    "search_results": 
        "error": {
            "message": "web search error",
            "type": "invalid_request_error",
            "code": "web_search_error"
        }
}
```

