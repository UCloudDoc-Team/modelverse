# 错误码
如果请求错误，服务器返回的JSON文本包含以下参数。

| HTTP 状态码 | 类型 | 错误码 | 错误信息 | 描述 |
|-|-|-|-|-|
| 400 | invalid_request_error | invalid_messages | 信息敏感 | 消息敏感 |
| 400 | invalid_request_error | characters_too_long | 对话 token 输出限制 | 目前 deepseek 系列模型支持的最大 max_tokens 为 12288 |
| 400 | invalid_request_error | tokens_too_long | Prompt tokens too long | 【用户输入错误】请求内容超过大模型内部限制，即用户输入大模型内容过长，可以尝试以下方法解决：<br>• 适当缩短输入 |
| 400 | invalid_request_error | invalid_token | Validate Certification failed | bearer token 无效，用户可以参考【鉴权说明】获取最新密钥 |
| 400 | invalid_request_error | invalid_model | No permission to use the model | 没有模型权限 |
