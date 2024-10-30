---
title: Visual Language Model:CLIP and Prompt Learning
date: 2023-07-26 09:34:51
tags:
    - Visual Language Model
    - Prompt Learning
    - Prompt Tuning
---
# Paper Reading(Break the Boundary of Image and Text :CLIP and some related work)

这两篇文章通过文本对图片的描述生成图片

## CLIP: Learning Transferable Visual Models From Natural Language Supervision

本文提供了一个(图像，文本)配对数据集用于实现图像-文本的对齐表征，这样帮助视觉模型在下游任务上实现zero-shot transfer，性能和监督学习的训练结果相匹配

### Overview

#### 核心思想：预训练+对比学习

本文通过最大化文本特征和图像特征的余弦相似性对Encoder的视觉分支进行预训练

#### 在下游任务上对视觉模型做zero-shot迁移

1. 对于N分类任务，构建N个prompt(每个class的描述文本)
2. 计算图像和每个文本embedding的相似度，相似度最大的文本即为预测结果

#### Summary

1. 文本描述来自Web，本质上承担了和标签相似的功能，这弥补了数据集外标签的泛化问题
2. 后续任务可以多种多样，例如用文本指导图像生成等

### Approach

作者提到同时学习文本和图像的表征可以利用文本包含的自然的语义信息用于后续任务以进行zero-shot adaptation

#### 数据集生成

图片的文本藐视就是图片的title

#### 模型预训练

1. 最初作者采取的方法是直接利用CNN提取Image特征以预测对应的title，但是这样代价太高了
2. 无需复现Text Description的每个word，只需要保证contrastive representation上的接近

#### Prompt Template

定义一个句子模板

`A Photo of xxx` xxx是物体，用句子是为了和训练title对齐，训练的embedding真正具有了对应文本语义（适用于不同粒度的语义信息）

### Prompt Engineering and ensembling

设计prompt存在一些潜在问题

1. 标签存在歧义，不同prompt对应的可能是语义上相似的物体
2. 仅仅采取一个单次作为prompt容易导致prompt和pair sentence不一致

prompt如果能给出更详细的描述，将会提升模型性能，比如全是数字的数据集中增添描述
$$
A \ number
$$

#### Ensemble With Several Zero-Shot Classifier

借助多种prompt template可以挖掘image更细粒度的信息

### Experiment

#### 特征提取

1. image特征提取：ResNet/Vit
2. text特征提取：Bert

## CoOp: Learning to Prompt for Vision Language Models

CLIP采取从网络上获取包含title图片的方式解决了缺少监督数据的问题，并通过设计prompt实现zero-shot adaptation。本文主要从设计prompt角度分析了CLIP存在的缺陷，prompt往往依赖数据集的先验知识或者专家知识。本文的主要工作是从prompt engineer角度考虑，能否learn一个好的prompt

### Solved Problem

CLIP中对Prompt Template的微小改动可以显著影响性能，作者通过本文讨论如何让模型基于特定的数据集**学习到**特定的Prompt

### Unified Context

本质上不论样本的类别提供一个通用模板，模板包含M+1个token，形式为
$$
t = [V_1][V_2]\cdots [V_M][CLASS]\\
$$
$[V_i]$被称为Context Token，CLIP通过计算每个class实例化的prompt嵌入和样本嵌入之间的余弦相似度进行预测

$[V_i]$是相同维度的词向量嵌入，本文提出了另一个Unified Context
$$
t = [V_1]\cdots [V_\frac{M}{2}][CLASS][V_{\frac M 2+1}]\cdots [V_M]
$$

Unified Context要求多个class对应的prompt

### Class-Specific Context

Class-Specific情况下满足每个class的Context Embedding不相同
$$
[V_1^i][V_2^i]\cdots [V_M^i]\neq [V_1^j][V_2^j]\cdots [V_M^j]
$$

### Learn the Context Token

Token实际上以token Embedding给出，学习好的Context Token Embedding需要通过优化分类损失函数反向传播优化Token Embedding

### Context初始化

1. 随机初始化
2. 用预定义的Word Embedding初始化

本文需要在迁移数据集上计算Cross-Entropy Loss，因此需要和few-shot clip相比

### Summay

CoOp可以说是Prompt Tuning的开篇之作，相比Finetune通过修改模型参数提升下游任务上的性能，Prompt Tuning的本质是通过设计更合适的Prompt更准确地描述下游任务

## CoCoOp: Conditional Prompt Learning for Vision-Language Models

CoOp的泛化性，我觉得这里指的是Unified Context的泛化性能，在所有Class都共享一套Context Token Embedding，给出一组新的Class Name，CoOp并不能在新class的分类任务上取得好的结果

论文中的Conditional指的是相对Image Embedding Conditional，Text Embedding写成
$$
v_m(x) = v_m + \pi
$$
 $\pi$根据image embedding生成
