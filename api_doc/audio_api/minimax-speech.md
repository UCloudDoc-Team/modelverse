# minimax-speech

本文介绍`minimax`语音合成模型 `hd` 系列调用 API 的输入输出参数，供您使用接口时查阅字段含义。

---

## 同步合成

### 接口

`https://api.modelverse.cn/v1/t2a_v2`

### 输入

| 参数                          | 类型   | 是否必选 | 描述                                                                                                                                                                                                                                                                                                          |
| :---------------------------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| model                         | string | 是       | 请求的模型版本，可选范围：<br>`speech-2.8-hd`<br>`speech-2.6-hd`<br>`speech-02-hd`|
| text                          | string | 是      | 需要合成语音的文本，长度限制小于`10000`字符，若文本长度大于`3000`字符，推荐使用流式输出<br> •段落切换用换行符标记<br> •停顿控制：支持自定义文本之间的语音时间间隔，以实现自定义文本语音停顿时间的效果。使用方式：在文本中增加`<#x#>`标记，`x` 为停顿时长（单位：秒），范围 `[0.01, 99.99]`，最多保留两位小数。文本间隔时间需设置在两个可以语音发音的文本之间，不可连续使用多个停顿标记<br> •语气词标签：仅当模型选择 `speech-2.8-hd`时，支持在文本中插入语气词标签。支持的语气词：`(laughs)（笑声）`、`(chuckle)（轻笑）`、`(coughs)（咳嗽）`、`(clear-throat)（清嗓子）`、`(groans)（呻吟）`、`(breath)（正常换气）`、`(pant)（喘气）`、`(inhale)（吸气）`、`(exhale)（呼气）`、`(gasps)（倒吸气）`、`(sniffs)（吸鼻子）`、`(sighs)（叹气）`、`(snorts)（喷鼻息）`、`(burps)（打嗝）`、`(lip-smacking)（咂嘴）`、`(humming)（哼唱）`、`(hissing)（嘶嘶声）`、`(emm)（嗯）`、`(sneezes)（喷嚏）`|
| stream                        | boolean | 否       | 控制是否流式输出。默认 `false`，即不开启流式|
| stream_options                | object | 否       | `stream`输出控制   |
| stream_options.<br>exclude_aggregated_audio       | boolean | 否       | 设置最后一个 `chunk` 是否包含拼接后的语音 `hex` 数据。默认值为 `False`，即最后一个 `chunk` 中包含拼接后的完整语音 `hex` 数据        |
| voice_setting                 | object   | 否       | 声音设置 |
| voice_setting.voice_id        | string | 否       | 合成音频的音色编号。若需要设置混合音色，请设置 `timbre_weights` 参数，本参数设置为空值。支持系统音色、复刻音色以及文生音色三种类型，音色ID可查看[系统音色列表](https://platform.minimaxi.com/docs/faq/system-voice-id)     |
| voice_setting.speed           | float   | 否       | 合成音频的语速，取值越大，语速越快。取值范围 `[0.5,2]`，默认值为`1.0` |
| voice_setting.vol             | float | 否       | 合成音频的音量，取值越大，音量越高。取值范围 `(0,10]`，默认值为 `1.0`   |
| voice_setting.pitch           | int | 否       | 合成音频的语调，取值范围 `[-12,12]`，默认值为 `0`，其中 `0` 为原音色输出         |
| voice_setting.emotion         | enum<string>   | 否       | 控制合成语音的情绪，参数范围 `["happy", "sad", "angry", "fearful", "disgusted", "surprised", "calm", "fluent", "whisper"]`，分别对应 8 种情绪：`高兴，悲伤，愤怒，害怕，厌恶，惊讶，中性，生动，低语`<br> 选项 `fluent, whisper` 仅对 `speech-2.6-hd` 模型生效，`speech-2.8-hd` 模型不支持 `whisper`|
| voice_setting.text_normalization | boolean | 否       | 是否启用中文、英语文本规范化，开启后可提升数字阅读场景的性能，但会略微增加延迟，默认值为 `false`   |
| voice_setting.latex_read      | boolean | 否       | 控制是否朗读 `latex` 公式，默认为 `false`<br> •仅支持中文，开启该参数后，`language_boost` 参数会被设置为 `Chinese`<br> •请求中的公式需要在公式的首尾加上 $$<br> •请求中公式若有 `"\"`，需转义成 `"\\"`.|
| audio_setting                 | object   | 否     | 音频设置 |
| audio_setting.sample_rate     | int | 否       | 生成音频的采样率。可选范围`[8000，16000，22050，24000，32000，44100]`，默认为 `32000`  |
| audio_setting.bitrate         | int | 否       | 生成音频的比特率。可选范围  `[32000，64000，128000，256000]`，默认值为 `128000`。该参数仅对 mp3 格式的音频生效         |
| audio_setting.format          | enum<string>   | 否       | 生成音频的格式，默认值:`mp3`。<br> •`wav` 仅在非流式输出下支持。<br> •可用选项: `mp3, pcm, flac, wav` |
| audio_setting.channel         | int | 否       | 生成音频的声道数。可选范围：`[1,2]`，其中 `1` 为单声道，`2` 为双声道，默认值为 `1`   |
| audio_setting.force_cbr       | boolean | 否       | 对于音频恒定比特率（cbr）控制，可选 `false`、 `true`。当此参数设置为`true`，将以恒定比特率方式进行音频编码。<br>注意：本参数仅当音频设置为流式输出，且音频格式为 `mp3` 时生效。         |
| pronunciation_dict            | object   | 否       | 声调 |
| pronunciation_dict.tone       | string[] | 否       | 定义需要特殊标注的文字或符号对应的注音或发音替换规则。在中文文本中，声调用数字表示：<br>一声为 1，二声为 2，三声为 3，四声为 4，轻声为 5<br> 示例如下：<br>`["燕少飞/(yan4)(shao3)(fei1)", "omg/oh my god"]` |
| timber_weights                | object[] | 否       | 音色比重         |
| timber_weights.voice_id  | string   | 是       | 合成音频的音色编号，须和weight参数同步填写。支持系统音色、复刻音色以及文生音色三种类型。系统支持的全部音色可查看 [系统音色列表](https://platform.minimaxi.com/docs/faq/system-voice-id) |
| timber_weights.weight         | int | 是       | 合成音频各音色所占的权重，须与 `voice_id` 同步填写。可选值范围为`[1, 100]`，最多支持 `4` 种音色混合，单一音色取值占比越高，合成音色与该音色相似度越高.   |
| language_boost                | enum<string> | 否       | 是否增强对指定的小语种和方言的识别能力。默认值为 `null`，可设置为 `auto` 让模型自主判断。  可用选项: `Chinese, Chinese,Yue, English, Arabic, Russian, Spanish, French, Portuguese, German, Turkish, Dutch, Ukrainian, Vietnamese, Indonesian, Japanese, Italian, Korean, Thai, Polish, Romanian, Greek, Czech, Finnish, Hindi, Bulgarian, Danish, Hebrew, Malay, Persian, Slovak, Swedish, Croatian, Filipino, Hungarian, Norwegian, Slovenian, Catalan, Nynorsk, Tamil, Afrikaans, auto`      |
| voice_modify                  | object   | 否       | 声音效果器设置，该参数支持的音频格式：<br>非流式：mp3, wav, flac <br>流式：mp3|
| voice_modify.pitch            | int | 否       | 音高调整（低沉/明亮），范围 `[-100,100]`，数值接近 `-100`，声音更低沉；接近 `100`，声音更明亮  |
| voice_modify.intensity        | int | 否       | 强度调整（力量感/柔和），范围 `[-100,100]`，数值接近 `-100`，声音更刚劲；接近 `100`，声音更轻柔         |
| voice_modify.timbre           | int   | 否       | 音色调整（磁性/清脆），范围 `[-100,100]`，数值接近 `-100`，声音更浑厚；数值接近 `100`，声音更清脆 |
| voice_modify.sound_effects    | enum<string> | 否       | 音效设置，单次仅能选择一种，可选值：<br> `spacious_echo`（空旷回音）<br> `auditorium_echo`（礼堂广播）<br> `lofi_telephone`（电话失真）<br> `robotic`（电音）   |
| subtitle_enable               | boolean | 否       | 控制是否开启字幕服务，默认值为 `false`。此参数仅在非流式输出场景下有效         |
| output_format                 | enum<string>   | 否       | 控制输出结果形式的参数，可选值范围为`[url, hex]`，默认值为`hex` 。该参数仅在非流式场景生效，流式场景仅支持返回 `hex` 形式。返回的 `url` 有效期为 24 小时 |
| aigc_watermark                | bool   | 否       | 控制在合成音频的末尾添加音频节奏标识，默认值为 `false`。该参数仅对非流式合成生效 |

### 请求示例

⚠️ 如果您使用 Windows 系统，建议使用 Postman 或其他 API 调用工具。
```shell
curl --location --globoff 'https://api.modelverse.cn/v1/t2a_v2' \
--header 'Authorization: Bearer 1pUozzMYSELkn7PX71BcD64c-1926-42Bf-a494-6dE42d88' \
--header 'Content-Type: application/json' \
--data '{
  "model": "speech-2.8-hd",
  "text": "今天是不是很开心呀(laughs)，当然了！",
  "stream": false,
  "voice_setting": {
    "voice_id": "male-qn-qingse",
    "speed": 1,
    "vol": 1,
    "pitch": 0,
    "emotion": "happy"
  },
  "audio_setting": {
    "sample_rate": 32000,
    "bitrate": 128000,
    "format": "mp3",
    "channel": 1
  },
  "pronunciation_dict": {
    "tone": [
      "处理/(chu3)(li3)",
      "危险/dangerous"
    ]
  },
  "subtitle_enable": false
}'
```
### 输出

| 参数           | 类型   | 描述               |
| :------------- | :----- | :----------------- |
| data                  | object | 返回的合成数据对象，可能为 null，需进行非空判断 |
| data.audio            | string | 合成后的音频数据，采用 hex 编码，格式与请求中指定的输出格式一致     |
| data.subtitle_file    | string | 合成的字幕下载链接。音频文件对应的字幕，精确到句（不超过 50 字），单位为毫秒，格式为 `json`     |
| data.status           | int | 当前音频流状态：1 表示合成中，2 表示合成结束     |
| trace_id              | string | 本次会话的 id，用于在咨询/反馈时帮助定位问题     |
| extra_info            | object | 音频的附加信息     |
| extra_info.audio_length     | int | 音频时长（毫秒）     |
| extra_info.audio_sample_rate     | int | 音频采样率     |
| extra_info.audio_size     | int | 音频文件大小（字节）     |
| extra_info.bitrate     | int | 音频比特率     |
| extra_info.audio_format     | enum<string> | 生成音频文件的格式。取值范围 `[mp3, pcm, flac]`    |
| extra_info.audio_channel     | int | 生成音频声道数,1：单声道，2：双声道     |
| extra_info.invisible_character_ratio     | number | 非法字符占比.非法字符不超过 10%（包含 10%），音频会正常生成,并返回非法字符占比数据；如超过 10% 将进行报错     |
| extra_info.usage_characters     | int | 计费字符数     |
| extra_info.word_count     | int | 已发音的字数统计，包含汉字、数字、字母，不包含标点符号     |
| base_resp     | object | 本次请求的状态码和详情    |
| base_resp.status_code     | int | 状态码。<br>0: 请求结果正常<br>1000: 未知错误<br>1001: 超时<br>1002: 触发限流<br>1004: 鉴权失败<br>1039: 触发 TPM 限流<br>1042: 非法字符超过 10%<br>2013: 输入参数信息不正常    |
| base_resp.status_msg     | string | 状态详情    |


### 响应示例

```json
{
  "data": {
    "audio": "<hex编码的audio>",
    "status": 2
  },
  "extra_info": {
    "audio_length": 9900,
    "audio_sample_rate": 32000,
    "audio_size": 160323,
    "bitrate": 128000,
    "word_count": 52,
    "invisible_character_ratio": 0,
    "usage_characters": 26,
    "audio_format": "mp3",
    "audio_channel": 1
  },
  "trace_id": "01b8bf9bb7433cc75c18eee6cfa8fe21",
  "base_resp": {
    "status_code": 0,
    "status_msg": "success"
  }
}
```
