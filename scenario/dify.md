# 在Dify中使用
## 3.1 关于Dify
Dify 是一款开源的大语言模型(LLM) 应用开发平台。它融合了后端即服务（Backend as Service）和 LLMOps 的理念，使开发者可以快速搭建生产级的生成式 AI 应用。

## 3.2 在Dify中使用DeepSeek API
#### 第一步：[获取 API Key](https://console.ucloud.cn/modelverse/experience/api-keys)

#### 第二步：进入Dify进行配置
- 在Dify中右上角点击用户名 -> 设置
![](https://www-s.ucloud.cn/2025/05/078d598684b883370efa5a7a84a37d0c_1748427192978.png)
- 在在模型供应商中安装UCloud
![](https://www-s.ucloud.cn/2025/05/3c68c98a1ac97df6332199c423e2ddce_1748427192982.png)
- 点击添加模型
  1. 模型类型选择LLM
  2. 模型名称填写您所使用的模型ID，例如 `deepseek-ai/DeepSeek-R1` `deepseek-ai/DeepSeek-V3-0324` `deepseek-ai/DeepSeek-Prover-V2-671B` `Qwen/QwQ-32B` `Qwen/Qwen3-235B-A22B`
  3. API Key从控制台上获取
  4. URL填写`https://deepseek.modelverse.cn/v1`
![](https://www-s.ucloud.cn/2025/05/7d5bd1aea02a464c28debd1d3bbc5815_1748427192973.png)

#### 第三步：接入完成
