# Response API 调用文档

UModelverse 平台提供了与 OpenAI Response API 兼容的接口，开发者可以使用 OpenAI SDK 直接调用 Modelverse 上的模型，支持工具调用（Function Calling）等高级功能。

本文将向您介绍如何快速在 UModelverse 平台使用 Response API。

## 快速开始

### 安装 OpenAI SDK

使用 Python 3.8 及更高版本，通过以下 pip 命令安装 openai 软件包：

```bash
pip install openai
```

### 调用示例

以下示例展示了如何使用 Response API 进行基本的对话：

```python
from openai import OpenAI
import os

API_SECRET_KEY = "<YOUR_MODELVERSE_API_KEY>"

client = OpenAI(
    api_key=os.getenv("MODELVERSE_API_KEY", "<YOUR_MODELVERSE_API_KEY>"),
    base_url="https://api.modelverse.cn/v1/",
)

input_list = [{"role": "user", "content": "解释一下人工智能是如何工作的？"}]

response = client.responses.create(
    model="openai/gpt-4.1",
    input=input_list,
)

print(response.output_text)

```

## 模型ID说明

更多受支持的openai模型，请参考【获取模型列表】
