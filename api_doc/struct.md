## message说明
| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| role | string | 是 | 当前支持以下：   <br/>· user: 表示用户   <br/>· assistant: 表示对话助手   <br/>· system：表示人设 |
| name | string | 否 | message名 |
| content | string | 是 | 对话内容，说明：   <br/>（1）不能为空   <br/>（2）最后一个message对应的content不能为blank字符，如空格、"\n"、“\r”、“\f”等 |


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
| id | string | 对话序列号 |
| message | [message] | 响应信息，当stream=false时返回 |
| finish_reason | string | 输出内容标识，说明：   <br/>· normal：输出内容完全由大模型生成，未触发截断、替换   <br/>· stop：输出结果命中入参stop中指定的字段后被截断   <br/>· length：达到了最大的token数   <br/>· content_filter：输出内容被截断、兜底、替换为**等   <br/>· tool_calls：函数调用 |


## choices中message说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| role | string | 当前支持以下：   <br/>· user: 表示用户   <br/>· system：表示人设 |
| name | string | message名 |
| content | string | 对话内容 |
| reasoning_content | string | 思维链内容，说明：只有当模型为DeepSeek-R1有效 |


## sse_choices说明
当stream=true时，返回内容如下：

| 名称 | 类型 | 描述 |
| --- | --- | --- |
| id | int | choice列表中的序号 |
| delta | [delta] | 响应信息，当stream=true时返回 |
| finish_reason | string | 输出内容标识，说明：   <br/>· normal：输出内容完全由大模型生成，未触发截断、替换   <br/>· stop：输出结果命中入参stop中指定的字段后被截断   <br/>· length：达到了最大的token数   <br/>· content_filter：输出内容被截断、兜底、替换为**等   <br/>· tool_calls：函数调用 |

## delta说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| content | string | 流式响应内容 |
| reasoning_content | string | 思维链内容，说明：只有当模型为DeepSeek-R1有效 |


## usage说明
| 名称 | 类型 | 描述 |
| --- | --- | --- |
| prompt_tokens | int | 问题tokens数 |
| completion_tokens | int | 回答tokens数 |
| total_tokens | int | 总tokens数 |


## search_results说明
[联网搜索功能结果说明](https://aq6ky2b8nql.feishu.cn/wiki/RXEOw02rFiwzGSkd9mUcqoeAnNK)

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

### 搜索引擎

#### 搜狗

```json
{
    "RequestId": "", // Optional
    "Query": "今日金价如何",
    "Uuid": "", // Optional
    "Pages": [
      {
        // 网页抓取日期
        "date": "2025-02-28 15:30:34",
        // 网站图标
        "favicon": "http://img02.sogoucdn.com/app/a/200913/ab735a258a90e8e1f3e3dcf231bf53a9.png",
        // 描述图片(如果有)
        "images": [],
        // 搜索结果摘要
        "passage": "今天是2月28日,国际金价保持下跌...",
        // 相关性得分
        "score": 0.81802785,
        // 网站名称
        "site": "腾讯网",
        // 标题
        "title": "金价下跌6元！2025年2月28日各大金店黄金价格多少钱一克？",
        "url": "https://new.xxxx.com/xxx/a/xxxxxx"
      },
      {
        "date": "2025-02-28 17:05:08",
        "favicon": "http://img02.sogoucdn.com/app/a/200913/ab735a258a90e8e1f3e3dcf231bf53a9.png",
        "images": [],
        "passage": "当下的黄金价格走势一直牵动着投资者的心弦..",
        "score": 0.78595155,
        "site": "腾讯网",
        "title": "热搜第一！金价大跳水",
        "url": "https://new.xxxxx.com/xx/xxxxxx"
      },
     ]
}
```

