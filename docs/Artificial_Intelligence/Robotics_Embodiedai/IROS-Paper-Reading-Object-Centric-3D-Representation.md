---
title: IROS Paper Reading(Object-Centric 3D Representation)
date: 2024-09-16 18:10:00
tags:
	- Paper Reading
	- Robot Learning
	- IROS
---
# Paper Reading(Object-Centric 3D representation)

## TLDR

1. 建立以物体为中心的感知模型，专注于任务相关的视觉部分，提升environment上的泛化性
2. 感知具备3D感知能力，这样做的好处在于统一多视角观测，使得模型在视角发生变化的情况下成功泛化

### Task

构建开放世界具备泛化性的策略

1. 基于交互式分割模型以及单帧上的简单标记对任务相关的物体生成mask
2. 基于视频物体分割(VOS)模型传播单帧上的分割结果到争端视频中
3. 将2D segmentation mask传播到3D点云上
4. 基于分割点云训练端到端策略

### Generalization during deployment

1. 基于开放词汇分割模型生成一系列object-level mask
2. 基于DINOv2利用标注的object mask计算分割生成的object-level mask之间的语义相似度

### Dimension of Generalization

作者将generalization分成了以下几类

1. 改变背景
2. 改变相机位姿
3. 改变物体（颜色、大小和几何形态）

## Open-vocabulary Object-Centric 3D end-to-end Policy

### Object-centric 3D representation

1. 人类标注产生task-related 物体
2. 人类仅仅标注一帧需要关注的物体，我们在时许上连续的若干帧追踪标注的结果
3. 将分割结果投影回点云

### 构建端到端基于点云的策略

端到端策略输入包括多物体的局部点云、一个timestep以及一个action token(用于译码)，策略设计的一些细节包括

1. 将输入点云cluster为多个部分
2. 随机mask掉输入点云的一些token
3. 同时接收过去多个timestep的多个observation输入

### Segmentation Corrrespondence Model

这一部分是为了做验证时泛化设计的，在验证时为了避免对第一帧进行人工标记，作者采取SAM对第一帧进行分割，通过计算分割视觉嵌入和训练数据中的mask嵌入进行检索找到一个最匹配的mask，从而实现空间点云提取

## 问题

1. 进入Embodied AI这个领域，看Manipulation RLbench刷点 更加清晰的cluster
2. 是否pretrain-finetune能否提升模型的泛化
3. 大家都在干什么，我们都在干什么
