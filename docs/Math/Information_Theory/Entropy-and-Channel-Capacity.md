---
title: Entropy and Channel Capacity
date: 2023-10-08 14:28:08
tags:
    - Element of Information Theory
    - Information Theory
---
# Elements of Information Theory(Introduction: Entropy and Capacity of Channel)

Entropy刻画了编码随机事件所需要的最小期望编码长度
$$
H(X) = -\sum_x p(x)\log p(x)
$$
条件概率构成条件熵
$$
H(Y|X=x) = -\sum_y p(y|x)\log p(y|x)\\
H(Y|X)= \sum_x p(x) H(Y|X=x)=-\sum_x \sum_y p(x,y)\log p(y|x)
$$
定义互信息，表明了给定先验对随机变量不确定度的缩减
$$
I(X;Y) = H(X) - H(X|Y)=\sum_{x,y} p(y|x)p(x)\log \frac{p(y|x)}{p(y)}
$$
通信系统中，X视为系统的输入，Y是系统输出，它们之间满足条件概率$p(y|x)$，定义信道最大理论容量为
$$
C = \max_{p(x)} I(X;Y)
$$

> 无噪声信道，满足$H(X|Y)=0$，因此Information capacity满足
> $$
> \max_{p(x)}H(x) = -\sum_x p(x) \log p(x) = 1(p(0)=p(1)=\frac 1 2)
> $$
> 这意味着一次发送一个bit在接收端可以正确判别输入 
>
> 给定信道，输入可能是4个事件（1，2，3，4），输出y满足
> $$
> p(y=x|x) =\frac 1 2\\
> p(y=x+1\mod 4|x)=\frac 1 2
> $$
> 例如$p(1|1)=p(2|1)=\frac 1 2$，这种情况下
> $$
> H(Y|X) = -\sum_{x}p(x)\sum_y p(y|x)\log p(y|x) = 1
> $$
> 互信息记作
> $$
> I(X|Y) = H(X) - 1\to \max I(X|Y) = 1
> $$
> 这个互信息如何达到呢？答案是取输入中不会在输出中重合的部分编码，即选择1，3作为系统输入，它们一定不会再输出中重合
>
> 对称交叉信道是编码中常见的情况，指的是$p(1|0)=p(0|1)= \rho$
>
> ![image-20231008115701761](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20231008115701761.png)
>
> 显然
> $$
> H(Y|X=0)=H(Y|X=1)=H(Y|X)=-(\rho \log \rho + (1-\rho)\log (1-\rho))\\
> $$
> Transmission Capacity为
> $$
> C = 1+ \rho \log \rho + (1-\rho )\log (1- \rho)
> $$
> 

