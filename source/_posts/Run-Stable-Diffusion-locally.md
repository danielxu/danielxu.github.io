---
title: 本地运行 Stable Diffusion（SD）来文生图
comments: false
sitemap: false
date: 2023-04-27 14:09:40
tags:
	- SD
	- python
	- M2 Pro
categories: SD
keywords: 'M2 Pro,Stable Diffusion,SD,stable-diffusion-webui'
description: 本地搭建 Stable Diffusion，通过 stable-diffusion-webui 进行文生图
top_img:
cover:
sticky: 100
---
{% note blue 'fas fa-bullhorn' %}

 Stable Diffusion是2022年发布的深度学习文本到图像生成模型。它主要用于根据文本的描述产生详细图像，尽管它也可以应用于其他任务，如内补绘制、外补绘制，以及在提示词（英语）指导下产生图生图的翻译。它是一种潜在扩散模型，由慕尼黑大学的CompVis研究团体开发的各种生成性人工神經网络。它是由初创公司StabilityAI，CompVis与Runway合作开发的，并得到EleutherAI和LAION（英语）的支持。

 Stable Diffusion的代码和模型权重已公开发布，可以在大多数配备有适度GPU的电脑硬件上运行。

{% endnote %}

[Stable Diffusion 源码库](https://github.com/Stability-AI/stablediffusion)，简称SD模型。维基百科说明：[SD](https://zh.wikipedia.org/zh-cn/Stable_Diffusion)。本地运行 SD 模型的 UI 界面 [stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)。

***

## 本地部署 Stable Diffusion

通过 stable-diffusion-webui 项目，由 python 本地运行一个 web ui。

另外，AI 绘画需要进行大量的图像处理和计算，所以对于电脑是有硬件要求的：
> 本地运行需要需要足够大的显存 (独立显卡的内存)，使用N卡，最低配置需要4GB显存，基本配置6GB显存，推荐配置12GB显存以上，越大越好。
> 显存大小决定了你能生成的图片尺寸，一般而言图片尺寸越大，SD 能发挥的地方越多，画面里填充的细节就越多。 
> 通常都是用通过 GPU 来跑 SD，因为 GPU 会加速计算。但用 CPU 跑也是可以的，但是速度会非常慢。

我的电脑是 Macbook M2 Pro 架构，所以参照的官方 wiki 文档 [Installation on Apple Silicon](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon) 配置即可。

步骤：
1. 安装 [Homebrew](https://brew.sh ) 配置到 PATH.
2. 命令行执行 `brew install cmake protobuf rust python@3.10 git wget`。如果 git 已经安装过，可以剔除命令。
3. 接着克隆仓库到本地 `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui`
4. 下载 Stable Diffusion models/checkpoints 模型，复制到 `stable-diffusion-webui/models/Stable-diffusion`文件夹. 下载模型的地方在下面介绍.
5. `cd stable-diffusion-webui` 并执行 `./webui.sh` 运行 web UI. 将使用venv创建和激活一个Python虚拟环境，并且将自动下载和安装任何剩余的缺失依赖项.
6. 如果需要重新启动 web UI进程，请再次执行`./webui.sh`命令。
	> 注意，它不会自动更新web UI; 要更新，请在运行`./webui.sh`之前执行`git pull`命令。

## 目录

```bash
cd ~/diffusion-ai/stable-diffusion-webui/models

.
├── Codeformer
├── ControlNet
├── ESRGAN
├── GFPGAN
├── LDSR
├── Lora # 这里是放 LORA 类型模型的地方
├── RealESRGAN
├── Stable-diffusion # 这里是放 CHECKPOINT 类型模型的地方
├── SwinIR
├── VAE
├── VAE-approx
├── deepbooru
├── hypernetworks
└── karlo
```

## 一些配置

1. 在 Settings - User interface - Quicksettings list 设置 `sd_model_checkpoint,sd_vae,sd_lora`
2. 在 Extensions - Available - Load from , 然后搜索 [Bilingual Localization](https://github.com/journey-ad/sd-webui-bilingual-localization.git) 安装“双语对照翻译”，搜索 [autocompletion](https://github.com/DominikDoom/a1111-sd-webui-tagcomplete.git) 安装 tag 补全，等等。很多插件，自己找需要的安装。

运行起来大概就是这个样子（我加了一些插件和配置，所以多了一些选项）：![stable-diffusion-webui](./img/stable-diffusion-webui.png)


## 下载 Model 的地方

模型格式一般是 `.ckpt` 或者 `.safetensors` 作为文件扩展名。格式的区别是:
* .ckpt: 包含 Python 代码的压缩文件，对于 Python 语言的程序来说很通用方便，缺点是文件很大，一般 2-8G。
* .safetensors: 只包含生成所需的数据，不包含代码，一般只有几十到几百 M，加载文件也更安全和快速。

### [Hugging Face](https://huggingface.co/models?pipeline_tag=text-to-image&sort=downloads)

如果你没有任何模型可以使用，可以从 Hugging Face 下载。要下载，请单击一个模型，然后单击Files and versions 标题。查找以 ".ckpt" or ".safetensors" 扩展名，然后单击文件大小右侧的向下箭头下载它们。


一些流行的官方稳定扩散模型是：
* [Stable DIffusion 1.4](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original) ([sd-v1-4.ckpt](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original/resolve/main/sd-v1-4.ckpt))
* [Stable Diffusion 1.5](https://huggingface.co/runwayml/stable-diffusion-v1-5) ([v1-5-pruned-emaonly.ckpt](https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned-emaonly.ckpt))
* [Stable Diffusion 1.5 Inpainting](https://huggingface.co/runwayml/stable-diffusion-inpainting) ([sd-v1-5-inpainting.ckpt](https://huggingface.co/runwayml/stable-diffusion-inpainting/resolve/main/sd-v1-5-inpainting.ckpt))

Stable Diffusion 2.0和2.1需要模型和配置文件，生成图像时需要将图像宽度和高度设置为768或更高：
* [Stable Diffusion 2.0](https://huggingface.co/stabilityai/stable-diffusion-2) ([768-v-ema.ckpt](https://huggingface.co/stabilityai/stable-diffusion-2/resolve/main/768-v-ema.ckpt))
* [Stable Diffusion 2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1) ([v2-1_768-ema-pruned.ckpt](https://huggingface.co/stabilityai/stable-diffusion-2-1/resolve/main/v2-1_768-ema-pruned.ckpt))

### [Civitai](https://civitai.com/)

Civitai 是目前最知名的 Stable Diffusion AI 艺术模型的社区平台，用户把它称为 C 站，它里面有非常多用户上传的模型。可以通过用户上传的优质图片查看相关提示词文本、模型、采样参数等等。Copy 下来到本地 web ui 里，相同的模型和插件，基本上会生成相似的图片（差异由于gpu、cpu）。帮助还不熟悉 Stable Diffusion 的用户快速上手。

> 这个站点需要梯子访问