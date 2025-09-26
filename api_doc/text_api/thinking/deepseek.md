# DeepSeek V3.1模型开启关闭思考说明

## 概念介绍

思考参数(thinking)用于控制模型在响应前是否显示思考过程。该功能适用于需要观察模型推理过程的场景。

## 参数说明

`thinking` 参数是一个结构，用于配置模型的思考过程。

**对象结构:**

| 字段            | 类型   | 必填 | 描述                       |
| :-------------- | :----- | :--- | :------------------------- |
| `thinking.type` | String | 是   | 用于控制是否显示思考过程。 |

**`type` 字段可选值:**

- `enabled`: 强制开启思考过程。
- `disabled`: 强制关闭思考过程。

json示例
```json
{
    "model": "ByteDance/doubao-seed-1.6",
    ...
    "thinking": {
        "type": "enabled"
    }
}
```

模型支持情况见下表
- deepseek-ai/DeepSeek-V3.1
- deepseek-ai/DeepSeek-V3.1-Terminus

## API 接口示例

```python
import json
import requests

# 配置API密钥
api_key = "******"  # 替换为你的 APIKEY
url = "https://api.modelverse.cn/v1/chat/completions"

headers = {"Authorization": f"Bearer {api_key}", "Content-Type": "application/json"}
data = {
    "model": "deepseek-ai/DeepSeek-V3.1-Terminus",  # 指定模型
    "messages": [
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "9.9和9.11哪个大",  # 用户提问
                }
            ],
        }
    ],
    "thinking": {"type": "enabled"},  # 开启思考功能，可选 enabled  disabled  auto
}

try:
    response = requests.post(url, headers=headers, json=data)
    response.raise_for_status() 

    print("请求成功!")
    print(json.dumps(response.json(), indent=2, ensure_ascii=False))

except requests.exceptions.RequestException as e:
    print(f"请求失败: {e}")

```

## 注意事项

1. 思考功能会增加少量响应时间
2. 对于简单问题建议使用disabled或auto模式
3. 复杂推理问题使用enabled模式可获得更好的可解释性