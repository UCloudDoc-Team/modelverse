# 模型列表 API

## 功能介绍

本接口用于获取 ModelVerse 平台上所有可用的模型 ID 列表，兼容 OpenAI API 格式。

## 第一步：获取 API Key

如何获取 api_key 值：请进入 Umodelverse 控制台 -「体验中心」- [API Key 管理](https://console.ucloud.cn/modelverse/experience/api-keys) 进行快速创建。

## 第二步：调用模型列表接口

### 请求方式

`GET /v1/models`

### 请求头域

| 名称          | 类型   | 必填 | 描述                                          |
| ------------- | ------ | ---- | --------------------------------------------- |
| Authorization | string | 是   | Bearer token 格式，传入第一步中获取的 API Key |

### 请求示例

```bash
curl --location 'https://api.modelverse.cn/v1/models' \
--header 'Authorization: Bearer YOUR_API_KEY'
```

**注意**：请将 `YOUR_API_KEY` 替换为您自己的 API Key。

## 响应

### 响应参数

| 名称          | 类型   | 描述                              |
| ------------- | ------ | --------------------------------- |
| object        | string | 固定值 "list"                     |
| data          | array  | 模型列表数组                      |
| data[].id     | string | 模型唯一标识符，用来在 API 中访问 |
| data[].object | string | 固定值 "model"                    |

### 响应示例

```json
{
  "object": "list",
  "data": [
    {
      "id": "deepseek-ai/DeepSeek-R1-0528",
      "object": "model",
      "created": 1,
      "owned_by": "Uloud_UModelverse"
    },
    {
      "id": "deepseek-ai/DeepSeek-V3-0324",
      "object": "model",
      "created": 1,
      "owned_by": "Uloud_UModelverse"
    },
    {
      "id": "Qwen/QwQ-32B",
      "object": "model",
      "created": 1,
      "owned_by": "Uloud_UModelverse"
    },
    {
      "id": "moonshotai/Kimi-K2-Instruct",
      "object": "model",
      "created": 1,
      "owned_by": "Uloud_UModelverse"
    }
  ]
}
```

## 使用说明

- 该接口返回的模型 ID 可直接用于 API 请求的的 `model` 参数
- 接口兼容 OpenAI API 格式，可使用 OpenAI SDK 调用
- 获取的模型 ID 也可用于 Gemini 格式的接口调用
- 建议定期调用该接口获取最新的模型列表
