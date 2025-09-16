# OpenAI 兼容 API 调用文档

UModelverse 平台提供了与 OpenAI API 兼容的接口，开发者可以使用熟悉的 OpenAI SDK 或工具直接调用 Modelverse 上的模型。

## 快速开始

您可以使用 `curl` 命令或任何支持 OpenAI API 的客户端库来调用 Modelverse API。

### 调用示例

请确保将 `$MODELVERSE_API_KEY` 替换为您自己的 API Key。

<tabs>
<tab name="curl">

```bash
curl https://api.modelverse.cn/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -d '{
    "model": "{model_name}",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful assistant."
      },
      {
        "role": "user",
        "content": "Hello!"
      }
    ],
    "stream": true
  }'
```

</tab>
<tab name="python">

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.getenv("MODELVERSE_API_KEY", "<YOUR_MODELVERSE_API_KEY>"),
    base_url="https://api.modelverse.cn/v1/",
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "Say hello world",
        }
    ],
    model="{model_name}",
)

print(chat_completion.choices[0].message.content)

```

</tab>
<tab name="node.js">

```javascript
const OpenAI = require("openai");

const openai = new OpenAI({
  apiKey: "YOUR_MODELVERSE_API_KEY",
  baseURL: "https://api.modelverse.cn/v1/",
});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: "user", content: "Say this is a test" }],
    model: "{model_name}",
  });

  console.log(chatCompletion.choices[0].message.content);
}

main();
```

</tab>
</tabs>
