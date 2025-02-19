# OpenAI接口兼容
Modelverse提供了与OpenAI兼容的使用方式，用户只需调整api_key、base_url、model等参数，就可以通过OpenAI SDK调用API。

## base_url说明
base_url指模型服务的请求地址。通过该地址，可以请求服务提供的功能或数据。当使用OpenAI兼容接口进行调用时，需要配置base_url。
- 需要配置的base_url如下：https://deepseek.modelverse.cn/v1

## 支持模型列表
| 模型名称 | 模型版本 | 最大输出长度
| --- | --- | ----
| DeepSeek-Reasoner | DeepSeek-R1 | 12288
| DeepSeek-Chat | DeepSeek-V3 | 12288 

## api_key说明
如何获取api_key值，请点击[API列表](https://console.ucloud.cn/uapi/detail?id=GetUMInferService)，无需填写参数，点击「发送请求」即可根据模型名称选择你需要的API Key。
![](https://www-s.ucloud.cn/2025/02/d51820006284a8c28160dc669c505987_1739523878908.png)

## 通过OpenAI SDK调用
- 请确保您已安装了最新版OpenAI SDK。
- 调用示例

```python
from openai import OpenAI

client = OpenAI(
    api_key=" ",  # Modelverse平台bearer token
    base_url="https://deepseek.modelverse.cn/v0.1",  # Modelverse平台域名
)

completion = client.chat.completions.create(
    model="DeepSeek-Reasoner",  # 预置服务请查看支持的模型列表
    messages=[
        {'role': 'system', 'content': 'You are a helpful assistant.'},
        {'role': 'user', 'content': 'Hello！'}
    ]
)

print(completion.choices[0].message)
```
