# 在ComfyUI中使用ModelVerse API

## 关于ComfyUI
ComfyUI 是一个基于节点流程式界面的 Stable Diffusion GUI，通过将 Stable Diffusion 的流程拆分成节点，实现了更精准的工作流定制和完善的流程复现。

## 准备工作
### 第一步：安装ComfyUI
请参考 [ComfyUI 官方文档](https://www.comfy.org/zh-cn/) 进行安装。

### 第二步：安装UCloud插件
1. 打开ComfyUI右上角 **Manager**，点击 **Custom Nodes Manager**。
2. 查询 "ComfyUI-UCloud"。
3. 点击 **Install**。
4. 重启ComfyUI。

### 第三步：获取UModelVerse API Key
1. [点击这里获取您的 API Key](https://console.ucloud.cn/modelverse/experience/api-keys)。
2. 将获取的 API Key 添加到 **Modelverse Client** 节点中。

![api-key-1](../images/comfyui/api_key_1.png)
![api-key-2](../images/comfyui/api_key_2.png)

## 快速上手：自动加载工作流
现在，你的笔记本也能轻松生成在自媒体上爆火的萌宠自拍图像，比如让一只橘猫去巴黎旅行：
![cat-paris](../images/comfyui/cat_paris.png)

**提示词**：
> This is an iPhone selfie perspective photograph, orange tabby cat wearing sunglasses, sitting in front of Eiffel Tower in Paris, happy expression, warm sunset lighting, travel photography style.

## 进阶玩法
### 多图批量处理
用 **Flux Kontext Pro (Multi-inputs)** 可以批量生成系列作品：
![multi-input](../images/comfyui/multi_input.png)

一次输入，可以同时生成：
![multi-output](../images/comfyui/multi_output.png)

### 细节优化：Step1X-Edit的威力
对于不满意的细节，可以用 **Step1X-Edit** 精修：

```
# 精修眼神光
Add bright reflection in cat's eyes

# 调整胡须细节
Enhance whiskers detail and texture

# 优化背景虚化
Improve background bokeh effect
```
![edit-1](../images/comfyui/edit_1.png)
![edit-2](../images/comfyui/edit_2.png)

这种局部编辑能力，让最终效果达到专业摄影水准。

## 技巧分享：提示词优化实战
经过大量测试，我们总结了几个关键技巧：

### 1. 提示词结构化
**基础结构**
```
[拍摄设备] + [视角描述] + [宠物特征] + [环境背景] + [光影氛围] + [风格定义]
```

**实战例子**
```
iPhone 13 Pro selfie perspective + 
golden retriever with happy expression + 
Santorini blue dome background + 
golden hour lighting + 
travel photography style
```

### 2. 模型选择策略
**文生图首选**
- **Flux Kontext Max Text2Image**: 质量最高

**图片编辑优选**
- **Flux Kontext Pro**: 细节处理强

**创意探索用**
- **Flux Dev**: 风格化能力出众

**批量制作用**
- **Multi-inputs版本**: 效率最高