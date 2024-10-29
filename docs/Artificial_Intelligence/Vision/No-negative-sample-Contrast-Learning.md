---
title: No negative sample Contrast Learning
date: 2023-06-19 15:49:41
tags:
    - Contrast Learning
---
# Contrastive Learning Introduction(III，不用负样本的对比学习)

## BYOL(Bootstrap Your Own Latent A New Approach to Self-Supervised Learning)

通过自举(仅仅在正样本之间对比学习)学习好的特征表示，同时避免模型退化

![image-20230216175143509](https://s2.loli.net/2023/02/16/mJBUMe4r8oCGj9A.png)

同样的instance通过两个数据增强得到$v$和$v^\prime$，通过两个编码器$f_\theta,f_\xi$和两个projection head $g_\theta,g_\xi$得到representation。使用动量更新$f_\xi,g_\xi$

### Predictor

增加一个Prediction Network $q_\theta$(MLP)，使得预测结果和$z_\xi^\prime$尽量接近，输出的$y_\theta$用于下游任务

### Loss

$$
\parallel q_\theta(z_\theta) - z^\prime_\xi\parallel_2^2
$$

### 问什么不会出现模型坍塌

projector/prediction包含两个Batch Normal
$$
Linear \to BN \to ReLU\to BN\tag{MLP}\label{MLP}
$$
Batch Normal会泄露样本中其它数据的特征(和平均数据的差别)，本质上是隐式的对比试验

## SimSiam(Exploring Simple Siamese Representation Learning)

1. 无需正样本
2. 无需batch size
3. 无需动量编码器

![image-20230216180732377](https://s2.loli.net/2023/02/16/lJQzLWFV9gHeid6.png)

共享参数的两个编码器(没有动量更新，完全copy参数)，predictor同时作用于$x_1,x_2$，随后互相预测，计算`mse loss`
$$
x_1 \to p_1\to h_1,x_2\to p_2\to h_2\\
\mathcal{L} = D(p_1,h_2)+D(p_2,h_1)
$$

### 几个工作的对比

![image-20230216181120466](https://s2.loli.net/2023/02/16/jK8zE5PvwMO4XHt.png)

1. `SimCLR`和`SwAV`计算正负样本embedding，优化`infonce loss`，区别是前者需要迭代两个编码器，后者只需要更新一个
2. `BYOL`和`SimSiam`无需计算`infonce loss`，转化为预测问题，前者采用动量更新保持参数一致性，后者直接采用同一个网络+stop gradient

## 结合Transformer和对比学习

### Moco-v3

解决自监督Transformer训练不稳定的问题，Backbone换成visual transformer，这种情况下大batch_size performance反而不好

> 随机初始化patch transformer(tokenization，图片序列化)

结合`infonce loss`和`predictive loss`

### Dino(Self-distillation with no labels)

![image-20230216200246056](https://s2.loli.net/2023/02/16/KUqm7XgtpcTxOu8.png)

添加centering避免模型坍塌
