# **计费说明**

| **功能模块** | **计费方式** | **计费说明**                                                                        |
| ------------ | ------------ | ----------------------------------------------------------------------------------- |
| 模型微调     | 按量后付费   | 根据训练任务的实际运行时长进行计算。按小时收取训练算力费用，不足 1 小时按分钟计算。 |
| 服务管理     | 预付费       | 根据模型自动计算算力单元，用户指定副本数合计费用=单价 _ 单副本算力单元数 _ 副本数   |
| 对象存储     | 按量后付费   | 按数据存储量计费，具体详见[UCloud 文档中心](https://docs.ucloud.cn/ufile/bill/new)  |
| API 调用     | 按量后付费   | 根据调用 token 计费                                                                 |

**大模型 API 定价**

于 2025 年 3 月 31 日开始收费。免费额度：50 万 tokens，额度使用完后，根据 token 使用情况按量后付费。

| **模型名称**                   | **计费方式**  | **计费说明**                                   |
| ------------------------------ | ------------- | ---------------------------------------------- |
| deepseek-ai/DeepSeek-R1        | tokens 后付费 | 输入 4 元/百万 tokens，输出 16/百万 tokens     |
| deepseek-ai/DeepSeek-V3        | tokens 后付费 | 输入 2 元/百万 tokens，输出 8 元/百万 tokens   |
| deepseek-ai/DeepSeek-Prover-V2 | tokens 后付费 | 输入 4 元/百万 tokens，输出 16 元/百万 tokens  |
| Qwen/QwQ-32B                   | tokens 后付费 | 输入 2 元/百万 tokens，输出 6 元/百万 tokens   |
| Qwen/Qwen3-235B-A22B           | tokens 后付费 | 输入 2 元/百万 tokens，输出 8 元/百万 tokens   |
| Qwen3-Coder                    | tokens 后付费 | 输入 4 元/百万 tokens，输出 16/百万 tokens     |
| Qwen2.5-VL-72B-Instruct        | tokens 后付费 | 输入 16 元/百万 tokens，输出 48 元/百万 tokens |
| Kimi-K2-Instruct               | tokens 后付费 | 输入 4 元/百万 tokens，输出 16/百万 tokens     |
| ERNIE-X1-Turbo-32K             | tokens 后付费 | 输入 1 元/百万 tokens，输出 4/百万 tokens      |
| ERNIE-4.5-Turbo-128K           | tokens 后付费 | 输入 0.8 元/千 tokens，输出 0.32/千 tokens     |
| ERNIE-4.5-Turbo-VL-32K         | tokens 后付费 | 输入 3 元/百万 tokens，输出 9/百万 tokens      |
| DeepSeek-R1-Distill-Llama-70B  | tokens 后付费 | 输入 1 元/百万 tokens，输出 3/百万 tokens      |
| GLM-4.5                        | tokens 后付费 | 输入 2 元/百万 tokens，输出 8/百万 tokens      |
| GLM-4.5V                       | tokens 后付费 | 输入 2 元/百万 tokens，输出 6/百万 tokens      |
| DeepSeek-V3.1                  | tokens 后付费 | 输入 4 元/百万 tokens，输出 12/百万 tokens     |
| DeepSeek-V3.2                  | tokens 后付费 | 输入 2 元/百万 tokens，输出 3/百万 tokens     |

**生图模型 API 定价**

于 2025 年 6 月 30 日开始收费。免费额度：免费5张图片/模型，详情请参考[图片生成 API](https://docs.ucloud.cn/modelverse/api_doc/image-generation)

| 模型名称                    | 价格（元/张） | 分类     |
| --------------------------- | ------------- | -------- |
| Flux Kontext Pro Text2Image | 0.35          | 文生图   |
| Flux Kontext Max Text2Image | 0.70          | 文生图   |
| Flux Dev                    | 0.10          | 文生图   |
| Step1X Edit                 | 0.30          | 图生图 |
| Flux Kontext Pro            | 0.35          | 图生图 |
| Flux Kontext Max            | 0.70          | 图生图 |
| Flux Kontext Max Multi      | 0.70          | 多图编辑 |
| Flux Kontext Pro Multi      | 0.35          | 多图编辑 |

| **模型名称**           | **计费方式**  | **计费说明**                                       |
| ---------------------- | ------------- | -------------------------------------------------- |
| gemini-2.5-flash-image | tokens 后付费 | 输入 2.2 元/百万 tokens，输出 18.25 元/百万 tokens |

**视频模型 API 定价**

## 视频生成 API 定价

| 模型名称          | 分辨率 | 价格（元/秒） | 分类     |
| ----------------- | ------ | ------------- | -------- |
| Wan-AI/Wan2.2-I2V | 720p   | 0.35          | 图生视频 |
| Wan-AI/Wan2.2-I2V | 480p   | 0.18          | 图生视频 |
| Wan-AI/Wan2.2-T2V | 720p   | 0.35          | 文生视频 |
| Wan-AI/Wan2.2-T2V | 480p   | 0.18          | 文生视频 |
| Wan-AI/Wan2.5-I2V | 1080p   |1.095          | 图生视频 |
| Wan-AI/Wan2.5-I2V | 720p   | 0.73          | 图生视频 |
| Wan-AI/Wan2.5-I2V | 480p   | 0.365         | 图生视频 |
| Wan-AI/Wan2.2-T2V | 1080p  | 1.095          | 文生视频 |
| Wan-AI/Wan2.2-T2V | 720p   | 0.73          | 文生视频 |
| Wan-AI/Wan2.2-T2V | 480p   | 0.365          | 文生视频 |
