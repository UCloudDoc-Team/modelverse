# deprecated

# 常见客户端接入API
## 一、参数说明
| **参数名称** | **说明**                                                                 | **示例值**                                      |
|--------------|--------------------------------------------------------------------------|-------------------------------------------------|
| **URL**      | API的请求地址，用于指定调用的接口。                                       | `https://api.modelverse.cn/v1/chat/completions` `https://api.modelverse.cn/v1`|
| **模型ID**   | 指定调用的模型名称，用于确定API调用的具体功能。                           | `deepseek-ai/DeepSeek-R1-0528` `deepseek-ai/DeepSeek-R1` `deepseek-ai/DeepSeek-V3-0324` `deepseek-ai/DeepSeek-Prover-V2-671B` `Qwen/QwQ-32B` `Qwen/Qwen3-235B-A22B` `moonshotai/Kimi-K2-Instruct` `deepseek-ai/DeepSeek-V3.1`                        |
| **API Key**  | 用于验证用户身份的密钥，确保只有授权用户可以访问API服务。                 | [点此链接获取Key](https://console.ucloud.cn/modelverse/experience/api-keys)| 


## 二、域名链接说明
- 请注意：不同客户端可能根据其功能需求选择不同的URL链接，请严格按照客户端的说明填写信息。

| **客户端类型** | **URL链接**                       | **说明**                                                                 |
|----------------|----------------------------------|--------------------------------------------------------------------------|
| 通用API调用    | `https://deepseek.modelverse.cn/v1` | 基础API接口，适用于通用功能调用，可能需要根据具体功能指定更多参数。       |
| 聊天功能调用   | `https://deepseek.modelverse.cn/v1/chat/completions` | 专门用于聊天功能的API接口，针对对话生成等任务优化，参数和返回结果更聚焦于聊天场景。 |
