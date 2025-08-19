# **计费说明**



| **功能模块** | **计费方式** | **计费说明**                                                 |
| ------------ | ------------ | ------------------------------------------------------------ |
| 模型微调     | 按量后付费   | 根据训练任务的实际运行时长进行计算。按小时收取训练算力费用，不足1小时按分钟计算。 |
| 服务管理     | 预付费       | 根据模型自动计算算力单元，用户指定副本数合计费用=单价 * 单副本算力单元数 * 副本数 |
| 对象存储     | 按量后付费       | 按数据存储量计费，具体详见[UCloud文档中心](https://docs.ucloud.cn/ufile/bill/new) |
| API调用     | 按量后付费       | 根据调用token计费 |



**大模型API定价**

于2025年3月31日开始收费。免费额度：50万tokens，额度使用完后，根据token使用情况按量后付费。


| **模型名称** | **计费方式** | **计费说明**                                                 |
| ------------ | ------------ | ------------------------------------------------------------ |
| deepseek-ai/DeepSeek-R1 | tokens后付费   |输入4元/百万tokens，输出16/百万tokens |
| deepseek-ai/DeepSeek-V3     | tokens后付费   |输入2元/百万tokens，输出8元/百万tokens |
| deepseek-ai/DeepSeek-Prover-V2 | tokens后付费   |输入4元/百万tokens，输出16元/百万tokens |
| Qwen/QwQ-32B         | tokens后付费   |输入2元/百万tokens，输出6元/百万tokens |
| Qwen/Qwen3-235B-A22B | tokens后付费   |输入2元/百万tokens，输出8元/百万tokens |
| Qwen3-Coder | tokens后付费   |输入4元/百万tokens，输出16/百万tokens |
| Qwen2.5-VL-72B-Instruct | tokens后付费   |输入16元/百万tokens，输出48元/百万tokens |
| Kimi-K2-Instruct | tokens后付费   |输入4元/百万tokens，输出16/百万tokens |
| ERNIE-X1-Turbo-32K | tokens后付费   |输入1元/百万tokens，输出4/百万tokens |
| ERNIE-4.5-Turbo-128K  | tokens后付费   |输入0.8元/千tokens，输出0.32/千tokens |
| ERNIE-4.5-Turbo-VL-32K | tokens后付费   |输入3元/百万tokens，输出9/百万tokens |
| DeepSeek-R1-Distill-Llama-70B | tokens后付费   |输入1元/百万tokens，输出3/百万tokens |
| GLM-4.5 | tokens后付费   |输入2元/百万tokens，输出8/百万tokens |
| GLM-4.5V | tokens后付费   |输入2元/百万tokens，输出6/百万tokens |

**生图模型API定价**

于2025年6月30日开始收费。免费额度：根据不同的模型区分免费次数，详情请参考[图片生成API](https://docs.ucloud.cn/modelverse/api_doc/image-generation)

| 模型名称                        | 价格（元/张） | 分类     |
|----------------------------------|-----------|----------|
| Flux Kontext Pro Text2Image      | 0.35      | 文生图   |
| Flux Kontext Max Text2Image      | 0.70      | 文生图   |
| Flux Dev                         | 0.10      | 文生图   |
| Step1X Edit                      | 0.30      | 图片编辑 |
| Flux Kontext Pro                 | 0.35      | 图片编辑 |
| Flux Kontext Max                 | 0.70      | 图片编辑 |
| Flux Kontext Max Multi           | 0.70      | 多图编辑 |
| Flux Kontext Pro Multi           | 0.35      | 多图编辑 |
