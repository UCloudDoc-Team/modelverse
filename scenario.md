# 场景实践

在 ModelVerse 大模型服务平台，DeepSeek 推理服务提供强大的模型推理能力，广泛适用于各种应用场景，如文本分析、数据挖掘等。以下将为您提供详细的操作指南和 API 调用示例，帮助您快速集成 DeepSeek 推理服务。

## 调用 DeepSeek 推理服务

### 操作指南

访问「Modelverse 模型服务平台」—— 访问「模型广场」—— 选择需要的「DeepSeek」模型 —— 点击「API 文档」—— 申请获取 API Key。

### 三步快速接入

1. **获取 API Key**
   - 登录到您的Umodelverse账户。
   - 进入「模型广场」，相应模型的API文档页面。
   - 创建一个新的 API Key 并记录下该 Key，您将在后续步骤中使用它。

2. **安装必要的库**
   - 确保您已经安装了必要的库，例如 `requests` 库，用于发送 HTTP 请求。如果尚未安装，可以使用以下命令安装：
     ```bash
     pip install requests
     ```

3. **发送推理请求**
   - 以下是一个使用 Python 发送推理请求的示例代码：
     ```python
     import requests

     # 1. 获取 API 密钥
     DEEPSEEK_API_KEY = "sk-xxxxxxxxxxxxxxxx"  # 替换成你的密钥

     # 2. 创建请求
     headers = {
         "Authorization": f"Bearer {DEEPSEEK_API_KEY}",
         "Content-Type": "application/json"
     }

     data = {
         "model": "deepseek-chat",
         "messages": [
             {"role": "user", "content": "你好！"}
         ]
     }

     # 3. 发送请求
     response = requests.post(
         "https://deepseek.modelverse.cn/v1/chat/completions",
         headers=headers,
         json=data
     )

     print(response.json()["choices"][0]["message"]["content"])
     ```

### 核心参数说明

以下是一个包含核心参数的示例：

```json
{
  "model": "deepseek-chat"  
  "messages": [  
    {
      "role": "system",
      "content": "你是有帮助的 AI 助手"
    },
    {
      "role": "user",
      "content": "请解释量子计算"
    }
  ], 
  "max_tokens": 12288,  
  "stream": true  
}
```

### 最佳实践

为了确保您能够高效且安全地使用 DeepSeek 推理服务，我们提供了以下最佳实践指南。

#### 密钥获取

根据 API 文档，创建并获取您的 API Key。请妥善保管您的 API Key，避免泄露。

#### 模型选择

根据您应用场景的不同，选择合适的模型：

- **deepseek-reasoner**：推理模型，即 DeepSeek 最新推出的推理模型 DeepSeek-R1。通过指定 `model='deepseek-reasoner'`，即可调用 DeepSeek-R1。

#### 错误处理
在您的代码中，正确处理不同的响应状态码，以便在出现问题时能够及时响应：
| http状态码 | 类型               | 错误码            | 错误信息                | 描述                                                                 |
|------------|--------------------|-------------------|-------------------------|----------------------------------------------------------------------|
| 400        | invalid_request_error | invalid_messages   | 信息敏感                | 消息敏感                                                             |
| 400        | invalid_request_error | characters_too_long | 对话 token 输出限制      | 目前 deepseek 系列模型支持的最大 max_tokens 为 4096*3                  |
| 400        | invalid_request_error | tokens_too_long    | Prompt tokens too long   | 【用户输入错误】请求内容超过大模型内部限制，即用户输入大模型内容过长，可以尝试以下方法解决：<br>• 适当缩短输入 |
| 400        | invalid_request_error | invalid_token      | Validate Certification failed | bearer token无效，用户可以参考【鉴权说明】获取最新密钥              |
| 400        | invalid_request_error | invalid_model      | No permission to use the model | 没有模型权限                                                       |

