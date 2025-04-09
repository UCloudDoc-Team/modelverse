# 错误码

如果请求错误，服务器返回的JSON文本包含以下参数。

| HTTP 状态码 | 类型 | 错误码 | 错误信息 | 描述 |
|-|-|-|-|-|
| 400 | invalid_request_error | invalid_param | Invalid param | 参数不合法 |
| 400 | invalid_request_error | invalid_messages | Sensitive chat messages | 对话内容触发敏感词合规校验 |
| 400 | invalid_request_error | invalid_model | No permission to use the model | 没有模型权限 |
| 401 | invalid_request_error | invalid_token | Validate Certification failed | token 无效，用户可以参考【鉴权说明】获取最新密钥 |
| 500 | internal_error | internal_error | Internal Server Error |  |
| 500 | llm_server_error | llm_server_error | Request llm server Error |  |
