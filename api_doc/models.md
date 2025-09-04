# 获取模型列表

您可以通过 `/v1/models` API 端点获取当前所有可用模型的列表。

## 请求

### 请求地址

```
GET https://api.modelverse.cn/v1/models
```

### 请求示例

您可以使用 `curl` 命令来调用此接口。请确保将 `{YOUR_API_KEY}` 替换为您自己的 API Key。

```bash
curl https://api.modelverse.cn/v1/models \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {YOUR_API_KEY}"
```

### 请求头

| 参数 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| `Authorization` | string | 是 | 您的 API Key，格式为 `Bearer {YOUR_API_KEY}`。 |

## 响应

### 响应示例

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

### 响应体参数说明

| 参数 | 类型 | 描述 |
| :--- | :--- | :--- |
| `id` | string | 模型的唯一标识符。 |
| `object` | string | 对象类型，此处恒为 `model`。 |
| `created` | integer | 模型创建时间的 Unix 时间戳。 |
| `owned_by` | string | 模型所有者。 |
