# 获取模型列表

您可以通过 API 端点获取当前所有可用模型的列表。我们同时支持 OpenAI 和 Gemini 两种格式的 API。

## OpenAI 兼容格式

### 请求

#### 请求地址

```
GET https://api.modelverse.cn/v1/models
```

#### 请求示例

您可以使用 `curl` 命令来调用此接口。请确保将 `{YOUR_API_KEY}` 替换为您自己的 API Key。

```bash
curl https://api.modelverse.cn/v1/models \
  -H "Content-Type: application/json" 
```


### 响应

#### 响应示例

调用成功后，会返回一个 JSON 对象，其中包含一个模型对象列表。

```json
{
  "data": [
    {
      "created": 1756119051,
      "id": "Wan-AI/Wan2.2-I2V",
      "object": "model",
      "owned_by": "Uloud_UModelverse"
    },
    {
      "created": 1756119051,
      "id": "Wan-AI/Wan2.2-T2V",
      "object": "model",
      "owned_by": "Uloud_UModelverse"
    },
    {
      "created": 1755859769,
      "id": "openai/gpt-4.1",
      "object": "model",
      "owned_by": "Uloud_UModelverse"
    }
  ],
  "object": "list"
}
```

#### 响应体参数说明

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| `id` | string | 模型的唯一标识符。 |
| `object` | string | 对象类型，此处恒为 `model`。 |
| `created` | integer | 模型创建时间的 Unix 时间戳。 |
| `owned_by` | string | 模型所有者。 |

## Gemini 兼容格式

### 请求

#### 请求地址

```
GET https://api.modelverse.cn/v1beta/models
```

#### 请求示例

您可以使用 `curl` 命令来调用此接口。请确保将 `{YOUR_API_KEY}` 替换为您自己的 API Key。

```bash
curl --location 'https://api.modelverse.cn/v1beta/models' \
  -H "Content-Type: application/json" 
```


### 响应

#### 响应示例

调用成功后，会返回一个 JSON 对象，其中包含一个模型对象列表。

```json
{
    "models": [
        {
            "name": "models/gemini-2.5-flash-image",
            "baseModelId": "gemini-2.5-flash-image",
            "version": "",
            "displayName": "gemini-2.5-flash-image",
            "description": "gemini-2.5-flash-image",
            "inputTokenLimit": 0,
            "outputTokenLimit": 0,
            "supportedActions": [
                "generateContent"
            ]
        },
        {
            "name": "models/gemini-2.5-pro",
            "baseModelId": "gemini-2.5-pro",
            "version": "",
            "displayName": "gemini-2.5-pro",
            "description": "gemini-2.5-pro",
            "inputTokenLimit": 0,
            "outputTokenLimit": 0,
            "supportedActions": [
                "generateContent"
            ]
        },
        {
            "name": "models/gemini-2.5-flash",
            "baseModelId": "gemini-2.5-flash",
            "version": "",
            "displayName": "gemini-2.5-flash",
            "description": "gemini-2.5-flash",
            "inputTokenLimit": 0,
            "outputTokenLimit": 0,
            "supportedActions": [
                "generateContent"
            ]
        }
    ]
}
```

#### 响应体参数说明

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| `name` | string | 模型的完整资源名称。 |
| `baseModelId` | string | 基础模型的 ID。 |
| `version` | string | 模型版本号。 |
| `displayName` | string | 模型的显示名称。 |
| `description` | string | 模型的描述信息。 |
| `inputTokenLimit` | integer | 输入 Token 的最大限制。 |
| `outputTokenLimit` | integer | 输出 Token 的最大限制。 |
| `supportedActions` | array | 模型支持的操作列表。 |
