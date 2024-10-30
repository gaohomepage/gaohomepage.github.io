---
title: Paper List
date: 2024-03-31 11:25:46
tags:
    - Paper Reading
---
# Paper List

## 重要基础模型、训练及微调

| Title                                                        | Summary                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Learning Transferable Visual Models From Natural Language Supervision | CLIP:大规模多模态预训练技术                                  |
| NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis | 从多角度相机中获得高效的3D表征                               |
| LoRA: Low-Rank Adaptation of Large Language Models           | 利用低秩分解减少大模型微调的参数量                           |
| LLaMA: Open and Efficient Foundation Language Models         | 开源大语言模型预训练                                         |
| Denoising Diffusion Probabilistic Models                     | 扩散模型图像生成                                             |
| Learning to Prompt for Vision-Language Models                | 微调prompt而不是整个模型以期待在下游任务上取得更高的性能和更强的泛化能力 |
| Masked Autoencoders Are Scalable Vision Learners             | 图像领域的Bert，极为优雅的研究，强烈建议学习本文中讲故事的逻辑 |

以上论文分别关于大语言模型/多模态预训练模型的训练/微调/下游任务迁移中的各项技术，其中Nerf是一种3D感知相关的工作，之后的具身智能模型中使用3D信息将会是主流

## 多模态大模型

### 训练

| Title                                                        | Summary                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation | 一种新的多模态融合方式，自举式学习以提升数据质量，统一多模态模型的表征任务和生成任务 |
| Align before Fuse: Vision and Language Representation Learning with Momentum Distillation | 多模态融合做表征的新思路                                     |
| Segment Anything                                             | 利用多模态prompt实现任意物体的图像分割                       |

### 微调和迁移

| Title                                                    | Summary                                                      |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| Conditional Prompt Learning for Vision-Language Models   | Prompt中引入Conditional Prompt提升prompt在下游任务中的泛化性能 |
| Visual Prompt Tuning                                     | 在Transformer输入加入可学习的参数提升多模态模型在下游任务上的迁移能力 |
| The Power of Scale for Parameter-Efficient Prompt Tuning | 研究了在大语言模型上微调prompt的若干技术细节                 |

### 图像-文本生成

| Title                                                        | Summary                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Zero-Shot Text-to-Image Generation                           | DALLE：借助VAVAE做图像生成                                   |
| High-Resolution Image Synthesis with Latent Diffusion Models | Stable Diffusion在和pixel space等价的latent space上做扩散以减少训练和推理开销 |
| StyleCLIP: Text-Driven Manipulation of StyleGAN Imagery      | 借助CLIP中蕴含的语义信息做风格迁移                           |
| GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models | 基于文本对图像进行编辑，用到了Classifier-free diffusion      |
| Hierarchical Text-Conditional Image Generation with CLIP Latents | DALLE-2                                                      |
| Photorealistic Text-to-Image Diffusion Models with Deep Language Understanding | Imagegan                                                     |

### Diffusion及其改进

| Title                                                        | Summary                                                |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| Diffusion Models Beat GANs on Image Synthesis                | Guided Diffusion实现conditional图片生成                |
| Classifier-Free Diffusion Guidance                           | 对上文的改进，避免为了生成条件图片的同时训练Classifier |
| DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation | 对Stable Diffusion借助少量文本-图像对做微调            |
| Adding Conditional Control to Text-to-Image Diffusion Models | 借助文本中包含的细粒度语义信息对图像做编辑             |



## 具身智能Baseline

建议看一下这个Talk [推动开放世界中的机器人泛化](https://www.bilibili.com/video/BV1sC4y1d7yZ/?spm_id_from=333.337.search-card.all.click)

| Title                                                        | Summary                                              |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| Perceiver-Actor: A Multi-Task Transformer for Robotic Manipulation | 借助3d体素做具身智能Agent language-conditional的控制 |
| CLIPort: What and Where Pathways for Robotic Manipulation    | 二维图像+CLIP文本编码器做控制                        |
| Do As I Can, Not As I Say: Grounding Language in Robotic Affordances | 借助大语言模型做具身智能Agent控制                    |
| PaLM-E: An Embodied Multimodal Language Model                | 预训练具身智能多模态大模型                           |

