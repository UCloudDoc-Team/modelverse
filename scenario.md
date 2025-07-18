# 常见客户端接入API
## 一、参数说明
| **参数名称** | **说明**                                                                 | **示例值**                                      |
|--------------|--------------------------------------------------------------------------|-------------------------------------------------|
| **URL**      | API的请求地址，用于指定调用的接口。                                       | `https://api.modelverse.cn/v1/chat/completions` `https://api.modelverse.cn/v1`|
| **模型ID**   | 指定调用的模型名称，用于确定API调用的具体功能。                           | `deepseek-ai/DeepSeek-R1-0528` `deepseek-ai/DeepSeek-R1` `deepseek-ai/DeepSeek-V3-0324` `deepseek-ai/DeepSeek-Prover-V2-671B` `Qwen/QwQ-32B` `Qwen/Qwen3-235B-A22B` `moonshotai/Kimi-K2-Instruct`                         |
| **API Key**  | 用于验证用户身份的密钥，确保只有授权用户可以访问API服务。                 | [点此链接获取Key](https://console.ucloud.cn/modelverse/experience/api-keys)| 


## 二、域名链接说明
- 请注意：不同客户端可能根据其功能需求选择不同的URL链接，请严格按照客户端的说明填写信息。

| **客户端类型** | **URL链接**                       | **说明**                                                                 |
|----------------|----------------------------------|--------------------------------------------------------------------------|
| 通用API调用    | `https://deepseek.modelverse.cn/v1` | 基础API接口，适用于通用功能调用，可能需要根据具体功能指定更多参数。       |
| 聊天功能调用   | `https://deepseek.modelverse.cn/v1/chat/completions` | 专门用于聊天功能的API接口，针对对话生成等任务优化，参数和返回结果更聚焦于聊天场景。 |

## 三、常见客户端举例
## 1 在 Chatbox 中使用

### 1.1 关于ChatBox
Chatbox 是一个流行的大语言模型的全平台聊天客户端，特点是功能强大、安装简单。你可以用它接入各种大语言模型，然后在任何设备（电脑、手机、网页）上和 AI 聊天。

### 1.2 在 Chatbox 中使用 DeepSeek API
#### 第一步：[获取 API Key](https://console.ucloud.cn/modelverse/experience/api-keys)

#### 第二步：打开ChatBox，按图片说明进行配置
![](https://www-s.ucloud.cn/2025/02/f157d3cc11001adf71511734d28032ed_1739959761948.png)
![](https://www-s.ucloud.cn/2025/02/5025a54f7588bfddcd5ed6cfa34e7d23_1739959761957.png)
![](https://www-s.ucloud.cn/2025/02/9ccaf962b2276fc17d2e8bd55a774eb2_1739961410410.png)

#### 第三步：开始聊天
![](https://www-s.ucloud.cn/2025/02/828fdba2b6d9d0fd239b997b373e526f_1739961608007.png)

## 2 在Open WebUI中使用
### 2.1 关于Open WebUI
Open WebUI适合企业内部部署 (https://github.com/open-webui/)
它会在计算机上启动一个服务，获得一个内网网址，企业内部的人就都可以进行访问。Open-WebUI功能十分完整，支持对多用户进行管理，支持函数调用、RAG、联网等功能。

### 2.2 在OpenWeb UI中使用DeepSeek API
#### 第一步：[获取 API Key](https://console.ucloud.cn/modelverse/experience/api-keys)

#### 第二步：进入Open WebUI进行配置
- 从左下角的图标中进入「设置」
![](https://www-s.ucloud.cn/2025/02/11ac091a723823ad40c91fe8675eed49_1739963047863.png)
- 点击「外部链接」——「编辑链接」，按图片信息填写。注意这里的模型id填后要点击加号。
![](https://www-s.ucloud.cn/2025/02/430b950a41d45286c68fb393b5d99dc4_1739963047859.png)

#### 第三步：开始聊天
![](https://www-s.ucloud.cn/2025/02/c9174bd62ee33d00587935ba2e070da0_1739964081012.png)

## 3 在Dify中使用
### 3.1 关于Dify
Dify 是一款开源的大语言模型(LLM) 应用开发平台。它融合了后端即服务（Backend as Service）和 LLMOps 的理念，使开发者可以快速搭建生产级的生成式 AI 应用。

### 3.2 在Dify中使用DeepSeek API
#### 第一步：[获取 API Key](https://console.ucloud.cn/modelverse/experience/api-keys)

#### 第二步：进入Dify进行配置
- 在Dify中右上角点击用户名 -> 设置
![](https://www-s.ucloud.cn/2025/05/078d598684b883370efa5a7a84a37d0c_1748427192978.png)
- 在在模型供应商中安装OpenAI-API-compatible
![](https://www-s.ucloud.cn/2025/05/3c68c98a1ac97df6332199c423e2ddce_1748427192982.png)
- 点击添加模型
  1. 模型类型选择LLM
  2. 模型名称填写您所使用的模型ID，例如 `deepseek-ai/DeepSeek-R1` `deepseek-ai/DeepSeek-V3-0324` `deepseek-ai/DeepSeek-Prover-V2-671B` `Qwen/QwQ-32B` `Qwen/Qwen3-235B-A22B`
  3. API Key从控制台上获取
  4. URL填写`https://deepseek.modelverse.cn/v1`
![](https://www-s.ucloud.cn/2025/05/7d5bd1aea02a464c28debd1d3bbc5815_1748427192973.png)

#### 第三步：接入完成
