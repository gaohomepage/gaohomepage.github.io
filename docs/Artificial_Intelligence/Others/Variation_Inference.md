---
title: GMM EM VI
date: 2023-04-18 22:12:59
tags:
 - Machine Learning
---
# 机器学习——混合高斯模型/EM算法/变分推断

## 混合模型

包含K个子分布的概率模型

### 高斯混合模型(几何角度)

多个高斯分布合成一个分布(加权平均)
$$
p(x)=\sum_{k=1}^K \alpha_k \mathcal (\mu_k,{\sum}_k),\sum_{k=1}^K \alpha_i=1
$$

### 高斯混合模型(混合模型角度)

之前表示出混合分布的pdf，对于二维情况$x=(x_1,x_2)$，引入隐变量v(对应对应样本x属于哪一个高斯分布)

v是离散随机变量，概率分布记作
$$
p(v=c_k)=p_k,\sum_{i=1}^K p_i =1,x|z
$$
高斯混合模型是生成模型，z是一个高斯分布，生成过程变成1.选择一个概率分布2.在概率分布上采样

## 求解高斯混合模型的参数

高斯混合模型求解$p(x)$，实际上基于采样的结果求解$p_k$或$\alpha_k$以及高斯分布的参数$\mu_k,\sigma_k$

### 求和

$$
p(x)=\sum_z p(x,z)=\sum_{k=1}^K p(z=c_k)p(x|c_k)=\sum_{k=1}^K p_k \mathcal N(x|\mu_k,\sigma_k)
$$

### 任务

1. X观测数据$X=(x_1,x_2,\cdots,x_n)$
2. $(X,Z)$完整数据($z_i$是第i个数据属于哪一个高斯分布的标签)
3. $\theta$为参数$\theta =(p_1,p_2,\cdots,p_K,\mu_1,\mu_2,\cdots,\mu_K,\sigma_1,\cdots,\sigma_K)$

### 极大似然估计

$$
\hat \theta_{MLE} = \arg\max_{\theta} \log p(x)=\arg\max_\theta \sum_{k=1}^n \log p(x_i)=\arg\max_\theta \sum_{i=1}^n \log \sum_{k=1}^K p_k p(x_i|\mathcal N (\mu_k,\sigma_k))
$$

实际上这个问题不能依靠极大似然估计，需要使用EM算法。求导无解析解

## 使用EM算法求解GMM问题

### 记号

假定有N组数据$X_1,X_2,\cdots,X_N$，有K组高斯分布，参数分别为$\mu_i,{\sum}_i,i=1,2,\cdots,K$，约定
$$
\gamma_{jk},j=1,2,\cdots,N,k=1,2,\cdots,K
$$
为第j组数据采样自第k个高斯分布的概率，问题是希望估算每个高斯分布的参数$\mu,{\sum}$，记第k组高斯分布的参数为
$$
\theta_k = (\mu_k,{\sum}_k)
$$
这里增加一个假设，对于每次采样，选择某个分布的概率相同，因此记
$$
 \gamma_k
$$
表示选择分布k的概率，我们记
$$
\phi(x|\theta_i)
$$
为在参数为$\theta_i$的高斯分布中采样得到数据x的概率

### 参数学习：E步

给定K个高斯分布的参数$\theta_k$，求$E[\gamma_k|X,\theta]$(起始就是估算选择每个分布的概率)

> 给定K个高斯分布参数$\theta_i = (\mu_i,{\sum}_i),i=1,2,\cdots,K$，现在已知数据$X=x_1,x_2,\cdots,x_N$采样自K个高斯分布生成的混合高斯过程，如何求解隐变量$\gamma_k$？

#### 极大似然

目标是
$$
\gamma =\arg\max_{\gamma} P(X|\theta,\gamma)=\arg\max_{\gamma } \prod_{x_i}P(x_i|\theta,\gamma)
$$
现在看看概率$P(x_i|\theta,\gamma)$，根据联合分布的pdf，写成
$$
P(x_i|\theta,\gamma) = \sum_{j=1}^K \gamma_j P(x_i|\theta_j)
$$
这个问题很难有解析解(微分不好求)

#### 参数估计

假设上个周期中求出的概率选择参数为$\alpha_k$，则可以用下式估算$\gamma_{jk}$
$$
\gamma_{jk} = \frac{\alpha_k \phi(x_i|\theta_k)} {\sum_{k=1}^K \alpha_k \phi(x_i|\theta_k)}
$$
进一步得到(直接平均)
$$
\alpha_{k} =\frac{\sum_{j=1}^N \gamma_{jk}}{N}
$$

### 参数学习：M步

给定选择每个数据点的概率$\gamma_{jk}$，求解参数$\theta_k$，形式化是
$$
\theta = \arg\max_{\theta} P(X|\theta,\gamma)
$$
类似于高斯参数估计的方法估算概率，将E步估算的参数视为某种**权重**
$$
\mu_k =\frac{\sum_{j=1}^N (\gamma_{jk}x_j)}{\sum_{j=1} \gamma_{jk}}\\
{\sum}_k = \frac{\sum_{j=1}^N \gamma_{jk}(x_j-\mu_k)(x_j-\mu_k)^T}{\sum_{j=1}^N \gamma_{jk}}
$$

> 问题：方差是否为**有偏估计**？E步计算$\gamma_{ik}$中为何直接使用$\alpha_k$而不是$\gamma_{ij}$

### E-step

逐步迭代，即$\max E[\log p(x,z|\theta;\theta^t)]$，记优化目标为$Q(\theta,\theta^t)$
$$
\begin{aligned}
Q(\theta,\theta^t) &= \int_z \log p(x,z|\theta)\times p(z|\theta,x) dz\\
&=\sum_z \log\prod_{i=1}^n p(x_i,z_i|\theta)\prod_{i=1}^n p(z_i|x_i,\theta^t)\\
&=\sum_{z_1,z_2,\cdots,z_n}\sum_{i=1}^n \log p(x_i,z_i|\theta)\prod_{i=1}^n p(z_i|x_i,\theta^t)\\
&=\sum_{z_1,z_2,\cdots,z_n}[\log p(x_1,z_1|\theta)+\log p(x_2,z_2|\theta)+\cdots +\log p(x_n,z_n|\theta)]\prod_{i=1}^n p(z_i|x_i,\theta^t)
\end{aligned}
$$
拆分第一项
$$
\begin{aligned}
\sum_{z_1,z_2,\cdots,z_n}\log p(x_1,z_1|\theta)\prod_{i=1}^n p(z_i|x_i,\theta^t)&=\sum_{z_1} \log p(x_1,z_1|\theta)p(z_1|x_1,\theta^t)\\
&\sum_{z_2,z_3,\cdots,z_n} \prod_{i=2}^n p(z_i|x_i,\theta^t)\\
\end{aligned}
$$
实际上
$$
\sum_{z_2,z_3,\cdots,z_n}\prod_{i=2}^n p(z_i|x_i,\theta^t)=1
$$
可以视为对$(z_2,z_3,\cdots,z_n)$空间上的联合概率分布求和，因此有
$$
Q(\theta,\theta^t)=\sum_{i=1}^n \sum_{z_i} \log p(x_i,z_i|\theta)p(z_i|x_i,\theta^t)
$$
考虑联合概率分布
$$
p(x,z)= p(z)p(x|z)=p_z \mathcal N (x|\mu_z,{\sum}_z)
$$
因此
$$
p(z|x) = \frac{p(x,z)}{p(x)}=\frac{p_z \mathcal N(x|\mu_z,{\sum}_z)}{\sum_{z} p_z \mathcal N(x|\mu_z,{\sum}_z)}
$$
记住：参数不是一个隐变量！因此可以写成
$$
Q(\theta,\theta^t) = \sum_{i=1}^n  \sum_{z_i}\log p_{z_i} \mathcal N (x_i|\mu_z,{\sum}_z)\frac{p_{z_i}\mathcal N(x_i|\mu_{z_i},{\sum}_{z_i})}{p(x_i,z_i)}
$$

迭代
$$
\theta_{t+1} = \arg\max_{\theta}Q(\theta,\theta_t)
$$
说明一点

## EM算法

传统机器学习任务实际上是求解优化问题
$$
\theta_{MLE}=\log p(x|\theta)
$$
引入隐变量z，此时优化目标变为
$$
\theta^{t+1} =\arg\max_\theta \int_z \log p(x,z|\theta)p(z|x,\theta^t) dz
$$
E step
$$
P(z|x,\theta^t) = E_{z|x,\theta^t} [\log p(x,z|\theta)]
$$
M step
$$
\theta^{t+1} =\arg\max_\theta E_{z|x,\theta^t}[\log p(x,z|\theta)]
$$

### ELBO+KL divergence

概率的性质，显然有
$$
\log P(x|\theta) = \log p(x,z|\theta)-\log p(z|x,\theta)
$$
两边对$z$的先验分布$q(z)$进行积分，得到
$$
LHS = \log p(x|\theta)
$$
对于右边，写成
$$
\begin{align}
RHS &= \int_z q(z)\log \frac{p(x,z|\theta)}{q(z)} dz\\
&-\int_z q(z)\log \frac{p(z|x,\theta)}{q(z)}
\end{align}
$$
(28)显然是KL散度，将(27)记作EBLO，显然有
$$
\log p(x|\theta)\geq ELBO
$$
一个简单的想法是优化ELBO扩大下界，EM算法可以分成两步

1. 根据数据X，参数$\theta$估算隐变量z的分布$q(z)=q(z|X,\theta)$
2. 估算ELBO($\log p(x,z|\theta)$在$q(z)$下的期望)

### EBLO+Jensen Inequality

起点仍然是
$$
\log p(X|\theta)
$$
这里假设$X=(x_i)_{i=1}^n ,x_i|p(x),i.i.d$，引入隐变量对隐变量求积分，得到
$$
\begin{align}
\log p(X|\theta)&=\log \int_z p(X,z|\theta) dz\\
&=\log \int_z \frac{p(x,z|\theta)}{q(z)} q(z) dz\\
&=\log E_{z|q(z)}[\frac{p(x,z|\theta)}{q(z)}]
\end{align}
$$
Jensen不等式表明凸函数下期望的性质，对于凸函数$f(x)$，我们有
$$
E_{x|g(x)}[f(x)] \leq  f[E_{x|g(x)}(x)]
$$
因此根据(33)得到
$$
\log p(X|\theta)\geq E_{z|g(z)}[\log \frac{p(x,z|\theta)}{q(z)}]=ELBO
$$
取等号的条件是
$$
\frac{p(z,x|\theta)}{q(z)}=C\Rightarrow q(z)=\frac{p(z,x|\theta)}{C}
$$
按照概率的积分性质
$$
1=\int_z q(z) dz = \int_z \frac{p(x,z|\theta)}{C} dz= \frac{1}{C} p(x|\theta)
$$
因此
$$
q(z) = \frac{p(z,x|\theta)}{p(x|\theta)}=p(z|x,\theta)
$$
这就是为什么要让$q(z)=p(z|x,\theta)$，这是为了让后验概率接近ELBO

### 广义EM

观测数据$X=(x_i)_{i=1}^n$，隐变量$Z=(z_i)_{i=1}^n$，求解目标为
$$
\hat \theta  =\arg\max_\theta  p(x|\theta)=\arg\max_\theta (ELBO+KL(q\parallel p))
$$
EBLO记作
$$
E_{q(z)}[\log \frac{p(x,z|\theta)}{q(z)}]= \mathcal L(q,\theta)
$$
E-step中需要求$p(z|x,\theta^{t+1})$实际上并不是总能求出来(变分推断，MC采样)

$\theta$固定(E step)，此时求解q，应该满足
$$
\hat q = \arg\min_q KL(q\parallel p)= \arg\max_q \mathcal L(q,\theta)(等式左边是固定的，一项min一项必然max)
$$
固定$\hat q$
$$
\theta = \arg\max_\theta \mathcal L(\hat q,\theta)
$$
广义EM即为两个max问题，现在对L函数进行变形
$$
ELBO = E_{q(z)} [\log{p(x,z|\theta)}]- E_{q(z)}[\log q(z)]
$$
$-E_{q(z)}[\log q(z)]$是分布q的熵$H(q)$，M步中q固定，因此优化问题中只对
$$
E_{q(z)}[\log p(x,z|\theta)]
$$
求解最优化问题

## 变分推断

### 背景

概率角度->优化问题(回归)
$$
\arg\max P(X|\theta)
$$
贝叶斯角度->积分
$$
P(\theta|X) = \frac{p(X|\theta)p(\theta)}{p(X)}
$$

### 推断-决策

1. 推断：求解$p(\theta|X)$
2. 决策：预测X-数据集 $\hat x$新样本，求解$P(\hat x|X)$

$$
P(\hat x|X) =\int_\theta P(\hat x,\theta|X) d\theta=\int_\theta P(\hat x|\theta)P(\theta|X)d\theta = E_{\theta |P_X}[p(\hat x|\theta)]
$$

两种推断

1. 精确推断，求后验概率的解析解
2. 近似推断，确定性近似(VI)，随机近似(MCMC)

### 推导

X-观测参数 Z-隐变量(参数包含在其中) (X,Z)-完整数据，写成ELBO和divergence的形式
$$
\log P(X) =ELBO+KL(q(z)\parallel p(z|x))=\mathcal L(q) +KL(q\parallel p)
$$
$\mathcal L(q)$表明ELBO是q的函数，称为变分，E步的目标是
$$
q(z)\approx p(z|x)
$$
现在问题在于求解后验分布

#### 假设

Z是一组隐变量形成的参数，假设可以划分为M组，每组之间互相独立，概率记作(平均场理论)
$$
q(z) =\prod_{i=1}^M q_i(z_i)
$$
求$q_j$，需要将剩下M-1个q固定，ELBO写成
$$
\begin{align}
\mathcal L(q) &= \int_z q(z)\log p(x,z) dz - \\&\int_z q(z)\log q(z) dz\\

\end{align}
$$
(51)写成
$$
\begin{align}
\int_z \prod_{i=1}^M q_i(z_i) \log p(x,z) dz_1dz_2\cdots d z_M&= \int_z q_j(z_j) (\prod_{i\neq j}^M q_i(z_i)\log p(x,z)\prod_{i\neq j}^M d z_i)d z_j\\
&=\int_{z_j} q_j(z_j)\int_{z_i,i\neq j} \log P(x,z)\prod_{i\neq j}^M q_i(z_i)d z_i dz_j\\
&=\int_{z_j} q_j(z_j)E_{\prod_{i\neq j}^M q_i(z_i)}[\log p(x,z)] dz_j\\
&=\int_{z_j}q_j(z_j)\log \hat p(X,z_j) dz_j
\end{align}
$$
(52)写成
$$
\begin{align}
\int_z \prod_{i=1}^M q_i(z_i)\sum_{i=1}^M \log q_i(z_i) dz&=\sum_{i=1}^M \int_{z_i}q_i(z_i)\log q_i(z_i) d z_i\\
&(只关心z_j)=\int_{z_j}q_j(z_j)\log q_j(z_j) dz_j+C
\end{align}\\
\int_z\prod_{i=1}^M q_i(z_i) \log q_j(z_j) dz = \int_{z_j}q_j(z_j)\log q(z_j) dz_j
$$
因此$\mathcal L(q)$写成
$$
\mathcal L(q) =-\int_{z_j} q_j(z_j)\log \frac{q_j(z_j)}{\hat p(x,z_j)} d z_j=-KL(q_j\parallel \hat p(x,z_j))\leq 0
$$
只关心一个独立分量，优化始终在$\leq 0$的半边进行

## 标准正态分布到任意正态分布

希望从标准正态分布生成分布为$\mathcal N(\mu,\sigma^2)$的数据，可以从$\mathcal N(0,1)$中采样z，然后做线性变换
$$
\sigma z+\mu
$$

## 正态分布KL散度

$p=\mathcal N(0,1)$和$q=\mathcal N(\mu,\sigma^2)$之间的KL散度写成
$$
D_{KL}(p\parallel q) = -\frac{1}{2}\sum_{i=1}^J (1+\log \sigma_j^2-\sigma_j^2-\mu_j^2)
$$
