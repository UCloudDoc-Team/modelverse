# OpenAI TTS API 调用文档

UModelverse 平台提供了与 OpenAI TTS（Text-to-Speech）API 完全兼容的语音合成接口。开发者可以使用熟悉的 OpenAI SDK 或标准 HTTP 客户端，无缝调用 Modelverse 上部署的高质量 TTS 模型。

> **🎉 限时免费**：TTS 语音合成服务现正限时免费开放，欢迎体验！

## 快速开始

您可以使用 `curl` 命令或任何支持 OpenAI API 的客户端库来调用 Modelverse TTS API。平台支持多种高质量语音合成模型，如 `IndexTeam/IndexTTS-2` 等。

## 请求参数

| 参数  | 类型   | 必填 | 说明                                                                                                                                |
| ----- | ------ | ---- | ----------------------------------------------------------------------------------------------------------------------------------- |
| model | string | 是   | 要使用的 TTS 模型名称，例如：`IndexTeam/IndexTTS-2`                                                                                 |
| input | string | 是   | 要转换为语音的文本内容（最大支持 600 字符）                                                                                         |
| voice | string | 是   | 使用的音色，可选值：`jack_cheng`, `sales_voice`, `crystla_liu`, `stephen_chow`, `xiaoyueyue`, `mkas`, `entertain`, `novel`, `movie` |

## 调用示例

以下示例展示如何使用 `IndexTeam/IndexTTS-2` 模型进行语音合成。请确保将 `$MODELVERSE_API_KEY` 替换为您自己的 API Key。

 <!-- tabs:start -->

#### ** curl **
```bash
curl https://api.modelverse.cn/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -d '{
    "model": "IndexTeam/IndexTTS-2",
    "input": "你好！欢迎使用 Modelverse 语音合成服务。",
    "voice": "jack_cheng"
  }' \
  --output speech.wav
```
    
#### ** python **
```python
from pathlib import Path
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("MODELVERSE_API_KEY", "<YOUR_MODELVERSE_API_KEY>"),
    base_url="https://api.modelverse.cn/v1/",
)

speech_file_path = Path(__file__).parent / "generated-speech.wav"

with client.audio.speech.with_streaming_response.create(
    model="IndexTeam/IndexTTS-2",
    voice="jay_klee",
    input="你好！欢迎使用 Modelverse 语音合成服务。",
) as response:
    response.stream_to_file(speech_file_path)

print(f"Audio saved to {speech_file_path}")
```

<!-- tabs:end -->

## 响应格式

API 返回二进制音频文件流。

- **音频格式**：目前仅支持 **WAV** 格式输出
- **Content-Type**：`audio/wav`

## 注意事项

1. **限时免费**：当前 TTS 服务限时免费开放，正式计费标准后续将另行通知
2. **音频格式**：响应的二进制流格式目前仅支持 WAV，暂不支持其他音频格式
3. **文本长度限制**：单次请求的文本长度限制因具体模型而异，`IndexTeam/IndexTTS-2` 模型通常支持 600 字符以内的文本
4. **语音类型**：不同的 `voice` 参数会产生不同音色和风格的语音效果，建议根据实际场景选择合适的语音类型

## 错误处理

当请求失败时，API 会返回标准的 JSON 格式错误响应：

```json
{
  "error": {
    "message": "错误描述信息",
    "type": "invalid_request_error",
    "code": "error_code",
    "param": "<请求 ID，用于反馈或排查错误原因>"
  }
}
```
