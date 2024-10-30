---
title: VQ-VAE
date: 2024-05-11 14:26:38
tags:
    - VAE
    - 随笔
---
# 随笔：VQ-VAE如何进行梯度传播

## VQ-VAE前向过程

VQ-VAE包含

1. 编码器  $Enc_\theta$
2. 解码器 $Dec_\phi$
3. 中间离散码表 $\{e_1,e_2,\cdots,e_K\}$

输入x，编码器得到其中间表示
$$
z = Enc_\theta(x)
$$
随后在码表中进行最近邻搜索
$$
z_q(x) = e_k,k = \arg\min_k {\parallel z - e_k\parallel^2_2}
$$
送给Decoder进行重构
$$
x^\prime = Dec_\phi(z_q(x))
$$

## 反向过程（如何更新Codebook和Encoder）

为了更新Encoder，设计如下的重构损失
$$
\parallel x - Dec_\phi(z + sg(z_q - z))\parallel^2
$$

> sg在正向传播设置为1，反向传播视为0

Dec输入还是$z_q$，但是这里的sg操作表明梯度直接传递给z而不是$z_q$，回忆一下传统的Encoder-Decoder架构
$$
x\to Enc_\theta\to z \to Dec_\phi\to x^\prime
$$
梯度写成
$$
\frac{\partial L}{\partial \theta} = \frac{\partial L}{\partial z}\frac{\partial z}{\partial \theta} = \frac{\partial \parallel Dec_\phi(z) -x\parallel}{\partial z} \frac{\partial Enc_\theta(x)\parallel}{\partial \theta}
$$
这里相当于
$$
\frac{\partial L}{\partial z} = \frac{\partial L}{\partial z_q}
$$
更新codebook，目标是尽量让$z_q(x)$和z靠近，设计损失函数
$$
L_{c} = \beta \parallel sg(z)- z_q\parallel_2^2 + \alpha \parallel z - sg(z_q)\parallel_2^2
$$
对应$z_q$的梯度写成
$$
\frac{\partial L_{c}}{\partial z_q} = 2\beta (z - z_q)\\
\frac{\partial L_c}{\partial z} = 2\alpha (z - z_q)
$$
对$z_q$求导因为第二项被stop gradient因此第二项对$z_q$实际不可导

## 借助Gumbel Softmax直接对$\arg\min$优化

### 从正态分布中采样

对于一个带参数正态分布 $\mathcal N(\mu_\theta,\sigma_\theta)$，如何保证采样结果对$\theta$可导

> 从$\mathcal N(0,I)$中采样z，计算 $z^\prime = \sigma_\theta z+ \mu_\theta$，这种技巧被称为**重参数**

积分采样的角度看重参数，计算下列期望
$$
L_\theta = E_{x|p_\theta(x)}[f(x)] = \int p_\theta(z)f(z)dz =\sum_z p_\theta(z) f(z)
$$
希望得到的期望对$\theta$可导，避免直接采样选择从一个无参分布$q$中采样若干$\epsilon|q$，计算、
$$
L_\theta = \sum_i f(\epsilon)\frac{p_\theta(\epsilon)}{g(\epsilon)}
$$
离散情况下当然可以计算每个样本点上的期望，但是对于样本空间特别大的情况（100维0-1向量）很难，我们需要另一套科维采样的方法

### 可微采样

给定离散概率分布$p_1,p_2,\cdots,p_K$，定义Gumbel Softmax采样为
$$
\arg\max_i (\log p_i - \log(-\log \epsilon_i))_{i=1}^k,\epsilon_i | U(0,1)
$$

1. 从$U(0,1)$中采样K个随机数$\epsilon_1,\epsilon_2,\cdots,\epsilon_K$
2. 计算$\arg\max \log p_i - \log(-\log \epsilon_i)$，进行采样

引入softmax使得$\arg\max$可导
$$
softmax(\frac{(\log p_i - \log (-\log \epsilon_i))}{\tau})
$$

> 证明，假定$\log p_1 - \log (-\log \epsilon_1)$最大，则有
> $$
> \log p_1 - \log (-\log \epsilon_1) \geq \log p_i -\log(-\log \epsilon_i)\\
> \frac{p_1}{p_i} \geq  \frac{\log \epsilon_1}{\log \epsilon_i}\\
> p_1 \log \epsilon_i \leq p_i \log \epsilon_1\Rightarrow \epsilon_i < \epsilon_1^{p_i/p_1}\leq 1
> $$
> 因此$p(\epsilon_i\leq \epsilon_1^{p_i/p_1}) = \epsilon_1^{p_i/p_1}$，对于$\forall i\neq 1$的概率为
> $$
> \epsilon_1^{p_2/p_1}\epsilon_1^{p_3/p_1}\cdots \epsilon_1^{p_K/p_1} = \epsilon_1^{\frac 1 {p_1} - 1}
> $$
> 在(0,1)上积分，得到$\log p_1 - \log (-\log \epsilon_1)$最大的概率为$p_1$

这种方式从离散分布时采样
