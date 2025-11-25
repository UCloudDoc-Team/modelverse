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
| voice | string | 是   | 使用的音色，可选值包括：内置音色（`jack_cheng`, `sales_voice`, `crystla_liu`, `stephen_chow`, `xiaoyueyue`, `mkas`, `entertain`, `novel`, `movie`），也可以填写自定义音色 ID（形如 `uspeech:xxxx`，详见下文「使用自定义音色」） |

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

## 使用自定义音色（可选）

除了使用内置的 `voice` 名称外，您还可以上传自己的音色（甚至带有特定情绪的样例语音），生成一个专属的 `voice_id`，在 TTS 请求中通过 `voice` 字段进行引用。

> 注意：当前自定义音色资源会在上传 **7 天** 后自动清理，如需长期使用请注意提前备份或重新上传。（可联系商务团队咨询长期保存需求）

整体流程可以理解为 **三步**：

1. 通过 `/v1/audio/voice/upload` 上传音频，得到 `voice_id`。
2. 在 `/v1/audio/speech` 请求中，把 `voice` 字段设置为该 `voice_id`（例如：`uspeech:xxxx`）。
3. 通过 `/v1/audio/voice/list` / `/v1/audio/voice/delete` 管理已有的自定义音色。

> 提示：自定义音色完全兼容 OpenAI 协议，只是复用了 `voice` 这个字段，不需要学习新的参数名。

### 步骤一：上传自定义音色，获取 voice_id

接口地址：`POST /v1/audio/voice/upload`

- **域名示例**：`https://api.modelverse.cn`
- **完整 URL**：`https://api.modelverse.cn/v1/audio/voice/upload`
- **Content-Type**：推荐 `multipart/form-data`（直接上传文件），也支持通过表单字段传 Base64 字符串或远程 URL。

必填字段：

- `name`：音色名称，用于列表展示，例如「温柔女声」「客服音色A」。
- `speaker_*`：音色语料音频（三选一，必填其一）：
  - `speaker_file`：本地音频文件（推荐方式）。
  - `speaker_file_base64`：音频文件的 Base64 字符串。
  - `speaker_url`：音频文件的公网 URL。

可选字段：

- `emotion_*`：用于承载情绪风格的音频（三选一，可选）：
  - `emotion_file` / `emotion_file_base64` / `emotion_url`

音频文件要求（两类音频都遵守）：

- 格式：仅支持 `MP3`、`WAV`
- 大小：单个音频 ≤ `20MB`
- 时长：`5–30` 秒之间
- 采样率：`16kHz` 及以上

示例：通过文件上传一个带情绪的自定义音色（推荐）

```bash
curl -X POST "https://api.modelverse.cn/v1/audio/voice/upload" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -H "Content-Type: multipart/form-data" \
  -F "name=温柔女声" \
  -F "speaker_file=@/path/to/speaker.wav" \
  -F "emotion_file=@/path/to/emotion.wav"
```

成功时会返回类似结果：

```json
{
  "id": "uspeech:xxxx-xxxx-xxxx-xxxx"
}
```

这里的 `id` 就是后续在 TTS 请求里使用的 `voice_id`。

### 步骤二：在 TTS 请求中使用自定义 voice_id

当您已经拿到一个自定义音色 `id`（例如 `uspeech:xxxx`）后，只需要在 `/v1/audio/speech` 请求中把 `voice` 字段改成该 `id` 即可。

```bash
VOICE_ID="uspeech:xxxx-xxxx-xxxx-xxxx"

curl https://api.modelverse.cn/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -d "{
    \"model\": \"IndexTeam/IndexTTS-2\",\
    \"input\": \"你好，我是自定义的温柔女声。\",\
    \"voice\": \"$VOICE_ID\"\
  }" \
  --output speech-custom.wav
```

行为说明：

- `voice` 为空：使用默认音色或模型自带的默认配置。
- `voice` 为内置名称（如 `jack_cheng` 等）：使用平台内置的预置音色。
- `voice` 为形如 `uspeech:xxxx` 的值：表示使用你上传的自定义音色，平台会根据该 ID 查找并应用对应的音色/情绪素材，无需额外配置。

### 步骤三：管理已有的自定义音色

1. **查看当前账号下有哪些自定义音色**

   接口：`GET /v1/audio/voice/list`

   ```bash
   curl -X GET "https://api.modelverse.cn/v1/audio/voice/list" \
     -H "Authorization: Bearer $MODELVERSE_API_KEY"
   ```

   示例响应：

   ```json
   {
     "list": [
       { "id": "uspeech:xxxx", "name": "温柔女声" },
       { "id": "uspeech:yyyy", "name": "沉稳男声" }
     ]
   }
   ```

2. **删除不再使用的自定义音色**

   接口：`POST /v1/audio/voice/delete`

   ```bash
   curl -X POST "https://api.modelverse.cn/v1/audio/voice/delete" \
     -H "Authorization: Bearer $MODELVERSE_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "id": "uspeech:xxxx"
     }'
   ```

   删除成功后，该 `voice_id` 将无法继续在 TTS 请求中使用，请在确认业务不再引用后再删除。

### 常见问题（FAQ）

- **Q：我上传的音频有什么要求？**  
  A：必须是 MP3/WAV，单个文件 ≤ 20MB，时长 5–30 秒，采样率至少 16kHz，不符合要求会返回 4xx 错误，并在 `error.code` 中标明原因。

- **Q：`voice` 字段应该填什么？**  
  A：
  - 想直接用平台提供的固定音色：填写内置名称（如 `jack_cheng`）；
  - 想用自己录制的音色：先调用上传接口拿到 `id`，然后在 TTS 请求中把 `voice` 设置为这个 `id`（例如 `uspeech:xxxx`）。

- **Q：出现 `invalid_voice_id` / `voice_not_found` 等错误怎么办？**  
  A：说明当前账号下找不到这个 `voice_id`，或者已被删除。可以先调用 `/v1/audio/voice/list` 确认当前可用的 ID，再在请求里使用正确的值。

- **Q：不同账号之间能共享自定义音色吗？**  
  A：同一组织下所有子账号，自定义音色可以共享。非同一账号下，无法共享。

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
