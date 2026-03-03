# 概览

模型服务平台 UModelVerse 旨在为客户提供快速搭建 AGI 应用的能力。我们强大的 API 让您无需关注底层算力调度、模型部署等复杂工作。仅需一个 API Key，即可轻松接入 OpenAI、Gemini 兼容的 API 接口，快速构建您的专属 AGI 应用。

## 产品简介

- [什么是模型服务平台](/modelverse/introduction/introduction.md)
- [产品优势](/modelverse/introduction/advantages.md)

## 计费说明

- [计费说明](/modelverse/price.md)

## API 调用

- [快速开始](/modelverse/api_doc/quick-start.md)
- 通用说明
  - [认证鉴权](/modelverse/api_doc/common/certificate.md)
  - [错误码](/modelverse/api_doc/common/error-code.md)
- 文本生成
  - [如何获取模型列表](/modelverse/api_doc/text_api/models.md)
  - [API支持与UCloud扩展字段说明](/modelverse/api_doc/text_api/api-expand.md)
  - [OpenAI Chat Completions 说明](/modelverse/api_doc/text_api/openai_compatible.md)
  - [OpenAI Response API 说明](/modelverse/api_doc/text_api/response_api.md)
  - [Embeddings 向量嵌入](/modelverse/api_doc/text_api/embeddings.md)
  - [DeepSeek-OCR 模型调用示例](/modelverse/api_doc/text_api/deepseek-ocr.md)
  - [Doubao 豆包模型思考开关](/modelverse/api_doc/text_api/thinking/doubao.md)
  - [Gemini 快速开始](/modelverse/api_doc/text_api/gemini_compatible.md)
  - [Claude (Anthropic) 兼容说明](/modelverse/api_doc/text_api/claude_compatible.md)
- 图片生成
  - [gemini-2.5-flash-image ( Nano Banana )](/modelverse/api_doc/image_api/gemini-2.5-flash-image.md)
  - [gemini-3-pro-image ( Nano Banana Pro )](/modelverse/api_doc/image_api/gemini-3-pro-image.md)
  - [gemini-3.1-flash-image-preview  ( Nano Banana 2 )](/modelverse/api_doc/image_api/gemini-3.1-flash-image.md)
  - [flux-2-pro](/modelverse/api_doc/image_api/flux-2-pro.md)
  - [flux-kontext-pro](/modelverse/api_doc/image_api/flux-kontext-pro.md)
  - [flux-pro-1.1](/modelverse/api_doc/image_api/flux-pro-1.1.md)
  - [black-forest-labs/flux.1-dev](/modelverse/api_doc/image_api/black-forest-labs-flux.1-dev.md)
  - [black-forest-labs/flux-kontext-pro](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-pro.md)
  - [black-forest-labs/flux-kontext-pro/multi](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-pro-multi.md)
  - [black-forest-labs/flux-kontext-pro/text-to-image](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-pro-text-to-image.md)
  - [stepfun-ai/step1x-edit](/modelverse/api_doc/image_api/stepfun-ai-step1x-edit.md)
  - [black-forest-labs/flux-kontext-max](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-max.md)
  - [black-forest-labs/flux-kontext-max/multi](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-max-multi.md)
  - [black-forest-labs/flux-kontext-max/text-to-image](/modelverse/api_doc/image_api/black-forest-labs-flux-kontext-max-text-to-image.md)
  - [Qwen/Qwen-Image-Edit](/modelverse/api_doc/image_api/Qwen-Qwen-Image-Edit.md)
  - [Qwen/Qwen-Image](/modelverse/api_doc/image_api/Qwen-Qwen-Image.md)
  - [gpt-image-1](/modelverse/api_doc/image_api/gpt-image-1.md)
  - [gpt-image-1.5](/modelverse/api_doc/image_api/gpt-image-1.5.md)
  - [doubao-seedream-4.5](/modelverse/api_doc/image_api/doubao-seedream-4.5.md)
- 视频生成
  - [OpenAI/Sora2-T2V](/modelverse/api_doc/video_api/OpenAI-Sora2-T2V.md)
  - [OpenAI/Sora2-T2V-Pro](/modelverse/api_doc/video_api/OpenAI-Sora2-T2V-Pro.md)
  - [OpenAI/Sora2-I2V](/modelverse/api_doc/video_api/OpenAI-Sora2-I2V.md)
  - [OpenAI/Sora2-I2V-Pro](/modelverse/api_doc/video_api/OpenAI-Sora2-I2V-Pro.md)
  - [OpenAI/Sora-2](/modelverse/api_doc/video_api/OpenAI-Sora-2.md)
  - [Wan-AI/Wan2.2-I2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.2-I2V.md)
  - [Wan-AI/Wan2.2-T2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.2-T2V.md)
  - [Wan-AI/Wan2.5-I2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.5-I2V.md)
  - [Wan-AI/Wan2.5-T2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.5-T2V.md)
  - [Wan-AI/Wan2.6-I2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.6-I2V.md)
  - [Wan-AI/Wan2.6-T2V](/modelverse/api_doc/video_api/Wan-AI-Wan2.6-T2V.md)
  - [MiniMax/Hailuo-2.3-I2V](/modelverse/api_doc/video_api/MiniMax-Hailuo-2.3-I2V.md)
  - [MiniMax/Hailuo-2.3-T2V](/modelverse/api_doc/video_api/MiniMax-Hailuo-2.3-T2V.md)
  - [MiniMax/Hailuo-2.3-Fast](/modelverse/api_doc/video_api/MiniMax-Hailuo-2.3-Fast.md)
  - [MiniMax/Hailuo-02](/modelverse/api_doc/video_api/MiniMax-Hailuo-02.md)
  - [Vidu/文生视频](/modelverse/api_doc/video_api/vidu/Vidu-Text2Video.md)
  - [Vidu/图生视频](/modelverse/api_doc/video_api/vidu/Vidu-Img2Video.md)
  - [Vidu/参考图生视频](/modelverse/api_doc/video_api/vidu/Vidu-Reference2Video.md)
  - [Vidu/首尾帧生视频](/modelverse/api_doc/video_api/vidu/Vidu-StartEnd2Video.md)
  - [Vidu/视频延长](/modelverse/api_doc/video_api/vidu/Vidu-Extend.md)
  - [Vidu/对口型](/modelverse/api_doc/video_api/vidu/Vidu-LipSync.md)
  - [Vidu/一键生成MV](/modelverse/api_doc/video_api/vidu/Vidu-Mv.md)
  - [doubao-seedance-1-5-pro](/modelverse/api_doc/video_api/doubao-seedance-1-5-pro-251215.md)
  - [kling-video-o1](/modelverse/api_doc/video_api/Kling-O1.md)
  - [kling-v2-6/图生视频](/modelverse/api_doc/video_api/Kling-v2.6-I2V.md)
  - [kling-v2-6/文生视频](/modelverse/api_doc/video_api/Kling-v2.6-T2V.md)
  - [Veo-3.1/文图生视频](/modelverse/api_doc/video_api/Veo-3.1.md)
- 音频生成
  - [OpenAI TTS 兼容](/modelverse/api_doc/audio_api/ttts.md)
  - [自定义音色](/modelverse/api_doc/audio_api/custom_voice_api.md)
  - [IndexTeam/IndexTTS 扩展参数](/modelverse/api_doc/audio_api/IndexTeam-IndexTTS-extend.md)
  - [suno音乐生成](/modelverse/api_doc/audio_api/suno.md)
  - [MiniMax/speech-hd](/modelverse/api_doc/audio_api/minimax-speech.md)

## 最佳实践

- [Claude Code 接入指南](/modelverse/best_practice/claudecode.md)
- [OpenAI Codex 接入指南](/modelverse/best_practice/codex.md)
- [ComfyUI插件接入](/modelverse/best_practice/comfyui.md)
- 常见客户端接入 API
  - [Chatbox](/modelverse/best_practice/scenario/chatbox.md)
  - [Open WebUI](/modelverse/best_practice/scenario/open-webui.md)
  - [Dify](/modelverse/best_practice/scenario/dify.md)
  - [Cherry-Studio](/modelverse/best_practice/scenario/cherry-studio.md)
- MCP 说明
  - [MCP 简介](/modelverse/best_practice/mcp/mcpgeneral.md)
  - [通过 CLINE 接入 MCP 服务](/modelverse/best_practice/mcp/MCPServer.md)
  - [通过 UCloud API 实现 MCP Client](/modelverse/best_practice/mcp/MCPClient.md)

## UModelVerse 协议

- [UModelVerse 隐私政策](/modelverse/private.md)
