# IndexTeam/IndexTTS 系列模型扩展参数说明

> 本文档为 **IndexTeam/IndexTTS 系列 TTS 模型**（例如：`IndexTeam/IndexTTS-2`、`IndexTeam/IndexTTS-shizhenfei`）的 **特供说明**，仅适用于在 UModelverse 平台上通过 `/v1/audio/speech` 调用这些模型的场景。
>
> - 基础调用方式、路径、认证方式等，请参考《[OpenAI TTS API 调用文档](/modelverse/api_doc/audio_api/ttts.md)》。
> - 自定义音色（`voice_id`）相关内容，请参考《[自定义音色管理 API 文档](/modelverse/api_doc/audio_api/custom_voice_api.md)》。
> - 本文介绍的部分参数 **不属于 OpenAI 官方 TTS 协议的一部分**，是 UModelverse 对 IndexTTS 模型的扩展能力，仅在本平台生效。

---

## 一、与 OpenAI 标准协议的关系

- `/v1/audio/speech` 的 **基础字段**（如 `model`、`input`、`voice`、`response_format`、`speed`、`instructions` 等）完全兼容 OpenAI 的 TTS 协议，具体含义请参考快速开始文档。
- 在此基础上，对于 IndexTTS 系列模型，UModelverse 额外支持一组 **扩展字段**，用于更细粒度地控制：
  - 采样率与音量
  - 情绪控制方式与权重
  - 情绪向量 / 文本
  - 句子划分与静音行为
  - 是否以流式形式返回等
- 这些扩展字段：
  - **仅在 UModelverse 上调用 IndexTeam/IndexTTS 系列模型时有效**；
  - 对官方 OpenAI 端点并无含义，也不保证可直接透传；
  - 不填写时，后台会使用模型侧的默认行为。

> 建议：如需保持“可以无修改切回官方 OpenAI 端点”的强兼容性，请只使用基础字段；
> 如希望充分利用 IndexTTS 模型的高级控制能力，可以在 UModelverse 环境下使用本文描述的扩展字段。

---

## 二、IndexTTS 扩展字段一览

以下字段在请求体 JSON 中与基础字段处于同一层级，例如：

```jsonc
{
  "model": "IndexTeam/IndexTTS-2",
  "input": "你好，欢迎使用 Modelverse TTS。",
  "voice": "jack_cheng",           // 基础字段

  "sample_rate": 24000,             // 扩展字段
  "gain": 1.0,                      // 扩展字段
  "emo_control_method": 1,          // 扩展字段
  "emo_weight": 0.8,                // 扩展字段
  "emo_text": "愉快",              // 扩展字段
  "interval_silence": true          // 扩展字段
}
```

> 注意：所有扩展字段均为 **可选**，不填写时不会出现在下游请求中，由供应商采用默认设置。

### 2.1 音频与增益相关

| 字段名       | 类型    | 是否必填 | 适用模型                     | 说明 |
| ------------ | ------- | -------- | ---------------------------- | ---- |
| `sample_rate`| int     | 否       | IndexTeam/IndexTTS 系列模型 | 目标音频采样率。由供应商定义支持的具体取值，如 16000、22050、24000 等。不填时使用模型默认采样率。 |
| `gain`       | float64 | 否       | IndexTeam/IndexTTS 系列模型 | 输出音量增益系数，用于整体放大或减小合成语音的音量。具体推荐范围由供应商侧定义，不填时使用默认音量。 |

### 2.2 情绪控制相关

> 下列字段配合自定义情绪音频 / 文本提示一起使用，可实现更细粒度的情感控制。具体数值含义由 IndexTTS 供应商定义。

| 字段名              | 类型       | 是否必填 | 适用模型                     | 说明 |
| ------------------- | ---------- | -------- | ---------------------------- | ---- |
| `emo_control_method`| int        | 否       | IndexTeam/IndexTTS 系列模型 | 情绪控制方式的标识（枚举/整数），用于指示模型采用哪种情感控制策略，例如基于样例音频、基于文本提示或其他方式。具体取值与含义由供应商文档约定。 |
| `emo_weight`        | float64    | 否       | IndexTeam/IndexTTS 系列模型 | 情绪控制权重，用于调节模型在情绪因素上的敏感度和影响程度。具体数值范围和默认值由供应商约定。 |
| `emo_vec`           | float64[]  | 否       | IndexTeam/IndexTTS 系列模型 | 情绪向量表示，可用于在向量空间中精细控制情绪特征。通常由上游工具或特征提取模块生成，长度与含义依赖于供应商实现。 |
| `emo_text`          | string     | 否       | IndexTeam/IndexTTS 系列模型 | 以自然语言描述情绪的文本提示，例如“愉快”“平静”“激动”等，由供应商作为情绪控制的文字条件输入。 |
| `emo_random`        | bool       | 否       | IndexTeam/IndexTTS 系列模型 | 是否在情绪控制中引入一定随机性，用于增加多样性或避免每句语音完全一致的情绪表达。具体效果由供应商实现。 |

### 2.3 句子划分与静音控制

| 字段名                      | 类型  | 是否必填 | 适用模型                     | 说明 |
| --------------------------- | ----- | -------- | ---------------------------- | ---- |
| `interval_silence`          | bool  | 否       | IndexTeam/IndexTTS 系列模型 | 是否在句子之间插入间隔静音。例如在多句文本合成时，控制句间是否留出停顿。具体时长由供应商实现。 |
| `max_text_tokens_per_sentence` | int | 否       | IndexTeam/IndexTTS 系列模型 | 单句文本切分的最大 token 数 / 长度阈值。用于在长文本场景下控制内部句子划分策略，具体含义由供应商解释。不填时使用模型默认切分策略。 |

### 2.4 流式返回控制

| 字段名         | 类型  | 是否必填 | 适用模型                     | 说明 |
| -------------- | ----- | -------- | ---------------------------- | ---- |
| `stream_return`| bool  | 否       | IndexTeam/IndexTTS 系列模型 | 是否以流式形式返回 IndexTTS 端的推理结果。该字段影响 UModelverse 与下游供应商之间的交互方式，具体生效行为取决于当前通道与模型配置。 |

---

## 三、示例：带扩展参数的 IndexTTS 调用

下面示例演示如何在保持 OpenAI 调用风格的同时，向 IndexTTS 模型传入扩展参数。

### 3.1 curl 示例

```bash
curl https://api.modelverse.cn/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -d '{
    "model": "IndexTeam/IndexTTS-2",
    "input": "你好，这是一段带有愉快情绪的语音示例。",
    "voice": "jack_cheng",

    "response_format": "wav",

    // 以下为 IndexTTS 扩展参数，仅在本平台上对 IndexTTS 系列模型有效
    "sample_rate": 24000,
    "gain": 1.0,
    "emo_control_method": 1,
    "emo_weight": 0.8,
    "emo_text": "愉快",
    "interval_silence": true
  }' \
  --output speech-indextts.wav
```

### 3.2 使用自定义音色 + 扩展参数（示意）

```bash
VOICE_ID="uspeech:xxxx-xxxx-xxxx-xxxx"  # 通过 /v1/audio/voice/upload 获取

curl https://api.modelverse.cn/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $MODELVERSE_API_KEY" \
  -d '{
    "model": "IndexTeam/IndexTTS-shizhenfei",
    "input": "你好，我是带情绪的自定义音色示例。",
    "voice": "'$VOICE_ID'",

    // IndexTTS 扩展参数
    "emo_control_method": 2,
    "emo_weight": 0.6,
    "emo_random": true
  }' \
  --output speech-indextts-custom.wav
```

> 说明：
>
> - 上述示例中的扩展参数值仅为演示用途，实际推荐取值范围与语义，请以供应商提供的文档及试听效果为准。
> - 不传扩展字段时，IndexTTS 模型会采用默认推理配置，行为与快速开始文档中的示例一致。

---

## 四、与其他文档的关系

- **快速开始文档**：《[OpenAI TTS API 调用文档](/modelverse/api_doc/audio_api/ttts.md)》适合希望快速接入、只使用标准参数的用户。
- **自定义音色文档**：《[自定义音色管理 API 文档](/modelverse/api_doc/audio_api/custom_voice_api.md)》专注描述 `voice_id` 的上传与管理。
- **本文档**：专门面向需要深度控制 IndexTTS 系列模型行为的用户，说明在 UModelverse 环境下可用的 **非 OpenAI 标准扩展字段**，不会影响或污染基础的快速开始文档。
