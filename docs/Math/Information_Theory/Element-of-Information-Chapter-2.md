---
title: Element of Information(Chapter 2)
date: 2023-10-21 23:59:09
tags:
---
# Element of Information Theory(Entropy/Relative Entropy and Mutual Information)

## Entropy/Joint Entropy and Conditional Entropy

Entropy定义为概率倒数对数的的期望
$$
H(X) = E[\log \frac {1}{p(x)}]
$$
Joint Entropy需要同时对两个变量求期望
$$
H(X,Y) = E_{x,y} [\log \frac {1}{p(x,y)}]
$$
Conditional Entropy需要遍历所有的先验条件
$$
H(Y|X) =\sum_x p(x) H(Y|X= x) = \sum_x p(x) E[\log \frac{1}{p(y|x)}] = -\sum_{x\in X}\sum_{y\in Y}p(x,y) \log p(y|x)
$$
显然有
$$
H(Y,X) = H(X) + H(Y|X)\\
H(X|X) = 0
$$
下面的推论表明了Conditional Entropy具有的拆分性质
$$
H(X,Y|Z) = H(X|Z) + H(Y|X,Z)
$$
条件熵**不具备**交换性，即
$$
H(Y|X)\neq H(X|Y)
$$
但是满足
$$
H(X) - H(X|Y) = H(Y) - H(Y|X)
$$

## Relative Entropy/Mutual Information

相对熵被用于刻画两个概率分布$p(x),q(x)$之间的距离，写成
$$
D(p\parallel q) = E_p[\log \frac{p(X)}{q(X)}]
$$
除了分布之间的距离，我们还希望讨论两个变量之间的相关性，随机变量独立意味着存在
$$
p(x,y) = p(x)p(y),\forall x\in X,y\in Y
$$
因此采用互信息定义变量的相关程度
$$
I(X;Y) = D(p(x,y)\parallel p(x)p(y)) = E_{p(x,y)} \log \frac{p(x,y)}{p(x)p(y)}=-H(X,Y)+H(X)+H(Y)
$$
互信息可以拆分成
$$
I(X;Y) = H(X) -H(X|Y) = H(Y) -H(Y|X)
$$

## 链式法则分解

$$
H(X_1,X_2,\cdots,X_n) = \sum_{i=1} H(X_i|X_{i-1},\cdots,X_1)\\
H(X_1,X_2,\cdots,X_n|Y) =\sum_{i=1} H(X_i|X_{i-1},\cdots,X_1,Y)\\
Proof:\quad
RHS = H(X_1,X_2,\cdots,X_n,Y) - H(Y)\\
LHS = \sum_{i=1} H(H_i,H_{i-1},\cdots,X_1,Y) - H(X_{i-1},X_{i-2},\cdots,X_1,Y)
$$

条件互信息表明了给定先验条件下两个随机变量的相关程度
$$
I(X;Y|Z) = E_{z\in p(z)}[D(p(X,Y|z)\parallel P(X|z)P(Y|z))] = E_{x,y,z\in p(x,y,z)}\log \frac{P(x,y|z)}{p(x|z)p(y|z)}
$$
Mutual Information也遵循类似的链式法则，表明了高维随机变量的互信息如何拆分成每一位随机变量的条件互信息
$$
I(X_1,X_2,\cdots,X_n;Y) = \sum_{i=1}^n I(X_i;Y|X_{i-1},X_{i-2},\cdots,X_1)
$$

> 证明
> $$
> \begin{aligned}
> I(X_1,X_2,\cdots,X_n;Y) &= H(X_1,X_2,\cdots,X_n) - H(X_1,X_2,\cdots,X_n|Y)\\
> &=\sum_i H(X_i|X_{i-1},\cdots,X_1) -\sum_i H(X_i|X_{i-1},\cdots,X_1,Y)
> \end{aligned}
> $$

定义条件概率的之间的距离
$$
D(p(y|x)\parallel q(y|x)) = \sum_x p(x)\sum_y p(y|x)\log \frac{p(y|x)}{q(y|x)}
$$

下面的定理表明了联合概率距离/边缘概率距离/条件概率距离之间的关系
$$
D(p(x,y)\parallel q(x,y)) = D(p(x)\parallel q(x)) + D(p(y|x)\parallel q(y|x))
$$

> 证明
> $$
> \begin{aligned}
> RHS&=\int p(x,y) \log \frac{p(x,y)}{q(x,y)} dx dy\\
> &=\int p(x)p(y|x)\log \frac{p(y|x)p(x)}{q(y|x)q(x)} dx d y
> \end{aligned}
> $$
> Q.E.D

## Convex函数的Jensen不等式

对于满足Strict Convex的函数
$$
\forall \lambda\in (0,1),x_1,x_2\in (a,b)\\
f(\lambda x_1 + (1 - \lambda) x_2) \leq \lambda f(x_1) + (1-\lambda) f(x_2)
$$
等号仅仅在$\lambda$为0或1时成立

### 凸函数的期望性质

对于凸函数f和随机变量X，满足
$$
E(f(X))\geq f[EX]
$$

这个性质和X分布无关

### 概率距离的非负性

给定概率分布$p,q$，一定有
$$
D(p\parallel q )\geq 0
$$

> $$
> \begin{aligned}
> -D(p\parallel q) &= \int p(x)\log \frac{q(x)}{p(x)} dx \\
> &=E[\log\frac{q(x)}{p(x)}]
> \end{aligned}
> $$
>
> 令
> $$
> f(x) = \frac {q(x)}{p(x)}
> $$
> 写成
> $$
> \begin{aligned}
> -D(p\parallel q) &= E_{p(x)}[\log f(x)]\leq \log E_{p(x)}[f(x)]\\
> &\leq \log \int_x f(x)p(x) dx   = 0
> \end{aligned}
> $$
> Q.E.D

### 互信息的非负性

添加一个额外的随机变量总不能减少对当前变量的估计的准确度
$$
\begin{aligned}
I(X;Y) &= H(X) - H(X|Y)\\
&=D(p(x,y)\parallel p(x)p(y))
\end{aligned}
$$
显然有
$$
I(X;Y)\geq 0
$$
当且仅当
$$
p(x,y) = p(x)p(y)
$$
时取=号，因此显然有
$$
H(X|Y)\leq H(X)
$$
条件熵不增

## Log Sum inequality

给定非负实数$a_1,a_2,\cdots,a_n$和$b_1,b_2,\cdots,b_n$，满足
$$
\sum_{i=1}^n a_i \log \frac {a_i} {b_i} \geq \sum_i a_i \log \frac{\sum_i a_i}{\sum_i b_i}
$$
当且仅当
$$
\frac{a_i}{b_i} = c
$$
取等号，证明

> Proof，定义
> $$
> f(t) = t \ln t\\
> f^\prime(t) = \ln t + 1\\
> f^{\prime \prime}(t) = \frac 1 t > 0
> $$
> 因此f(t)是strict convex的，满足
> $$
> \sum_i \alpha_i f(t_i)\geq f(\sum_i \alpha_i t_i),if\ \sum_i \alpha_i = 1
> $$
> 令
> $$
> \alpha_i =\frac{b_i}{\sum_j b_j}\\
> t_i =\frac {a_i}{b_i}
> $$
> 有
> $$
> \sum_i \frac{b_i}{\sum_j b_j} \frac{a_i}{b_i}\ln \frac{a_i}{b_i} = \frac{1}{\sum_j b_j}\sum_i a_i \ln\frac{a_i}{b_i}\geq (\sum_i a_i /\sum_j b_j)\ln \frac{\sum_i a_i}{\sum_i b_i}
> $$

特殊地，如果有$\sum_i a_i = \sum_i b_i = 1$，则
$$
\sum_i a_i \ln \frac{a_i}{b_i}\geq 0
$$

### 相对熵的是凸的

我们称相对熵$D(p\parallel q)$是凸的，意味着对于两个概率对$(p_1,q_1),(p_2,q_2)$和实数$\lambda\in (0,1)$
$$
D(\lambda p_1 +(1-\lambda )p_2\parallel \lambda q_1 + (1-\lambda)q_2) \leq \lambda D(p_1\parallel q_1) + (1 - \lambda) D(p_2\parallel q_2)
$$

### 熵是凹函数

定义样本空间$\mathcal X$上的均匀分布U，KL散度写成
$$
D(p\parallel u) = \sum_x p(x)\ln \frac{p(x)}{c} dx  = -H(p)+ \ln c 
$$
给定同一个样本空间$\mathcal X$上的随机变量$X_1,X_2$，满足概率分布$p_1,p_2$，定义落在[1,2]上的随机变量$\theta$，满足
$$
P(\theta = 1) = \lambda\\
P(\theta =2) = 1- \lambda
$$
随机变量Z定义为
$$
Z = X_\theta
$$
Z的分布为$\lambda p_1 + (1-\lambda)p_2$

根据相对熵不减，有
$$
H(Z)\geq H(Z|\theta)\\
H(\lambda p_1+(1-\lambda)p_2) \geq \lambda H(p_1) + (1-\lambda )H(p_2)
$$

### 互信息的凹凸性

关于互信息
$$
I(X;Y) = H(Y) - H(Y|X)
$$
的凹凸性，有如下结论

1. 对于固定的概率$p(x)$，$I(X;Y)$是凹函数（仅仅改变先验概率p(y|x)而固定边缘分布p(x)）
2. 对于固定的概率$p(y|x)$，$I(X;Y)$是凸函数

证明结论2
$$
I(X;Y) = H(Y) - \sum_x p(x) H(Y|X=x)
$$
$p(y|x)$是固定的，意味着第二项是$p(x)$的线性组合($H(Y|X=x)$是常数)，H(Y)是凹函数

证明结论1，需要说明对于两个条件概率
$$
p_1(y|x) ,p_2(y|x)
$$
和它们的线性组合
$$
p_{\lambda}(y|x) = \lambda p_1(y|x) + (1-\lambda)p_2(y|x)
$$
两边同时乘以p(x)，得到
$$
p_\lambda(x,y) = \lambda p_1(x,y) + (1-\lambda)p_2(x,y)
$$