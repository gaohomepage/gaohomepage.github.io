---
title: PytorchNLLloss
date: 2023-09-28 10:52:48
tags:
    - Pytorch
    - 随笔
---
# Pytorch NLL loss and Cross Entropy Loss

## CrossEntropy

1. 输入不要求归一化到1
2. 输入：logits,target

logits是$K\times C$的矩阵，C是分类维数，K是Batch_size，记作
$$
logits = (x_{n,c})_{n=1,2,\cdots,K}^{c=1,2,\cdots,C}
$$
target对应每个instance的label
$$
target = (y_1,y_2,\cdots,y_K)
$$
对应的Loss根据每个instance的logits和对应的ground truth计算得到
$$
l_n  = -w_{y_n}\log \frac{\exp (x_{n,y_n})}{\sum_{c=1}^C \exp (x_{n,c})}
$$

> 自动计算infonce loss

## NLL Loss

NLL  Loss要求输入归一化为概率
$$
\sum_{c=1}^C x_{n,c} = 1
$$
随后生成log-likelihood
$$
x_{n,c} = \log x_{n,c}
$$
计算损失函数无需归一化
$$
l_n = -w_{y_n}x_{ny_n}
$$
