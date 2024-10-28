
---
title: Convex Optimization:Introduction
date: 2023-04-06 13:08:29
tags:
 - Convex Optimization
---
# Convex Optimization——Optimization and Mathematical Programming Introduction

## 优化

1. 给定可行解集合
2. 找到集合中最优对象

$$
\min  f_o(x)\\
s.t  f_i(x)\leq b_i
$$

1. x优化变量
2. $f_0:\R^n\to \R$目标函数
3. $f_i:\R^n \to \R$不等式约束

$x^*$为最优$\Leftrightarrow$ $\forall z\in \{f_i(z)\leq b_i,i=1,2,\cdots,m \},f_0(z)\geq f(x^*)$

$x^*$组成可行解集合(feasible set)

### 例子：线性二次调节器

$$
x_k = Ax_{k-1}+ B u_k
$$

刻画状态转移，目标函数是
$$
\min_{u_k} J =\sum_{k=1}^N (x_k^T Q x_k +u_k^T Ru_k)
$$

### 例子：多用户能量控制

通讯系统由若干发射者$TX$到接收者$RX$组成，空间中存在若干用户$u_i$，以能量$p_i$发送信号，具有上限$b_i$，数据发送会产生干扰$a_{ij}$($u_i$发送单位能量信号对$u_j$的影响)，能量具有满足方差为$\sigma_i^2$的高斯噪声，定义$STNR_i$为用户i的信号/干扰比
$$
STNR_i = \frac{p_i}{\sigma_i^2 +\sum_{j\neq i} p_j \alpha_{ji}}
$$
信道容量满足
$$
f_i = k\log (STNR_i +1)
$$
优化目标和约束是
$$
\max_{i=1}^N f_i\\
s.t \ 0\leq p_i \leq b_i
$$

### 例子：图像处理中的优化

带噪声的图像$\Phi_0(x,y)$去噪，加上如下先验

> 图像分片光滑，即不能有稠密的连续变化，定义TV范数刻画
> $$
> \parallel \Phi\parallel_{TV} = \sum_y \sum_x \sqrt{(\Phi(x,y)-\Phi(x,y-1))^2+(\Phi(x,y)-\Phi(x-1,y))^2}
> $$

优化问题目标是
$$
\min_{\Phi}  \parallel \Phi\parallel_{TV} +\lambda \parallel \Phi-\Phi_0\parallel
$$

### 例子：集成电路设计

给定若干门电路$\{g_1,g_2,\cdots,g_n \}$将其相连实现功能并达到性能指标，已知有$t_1,t_2,\cdots,t_M$连接方法，优化性能指标
$$
\max_i p(t_i),i=1,2,\cdots,M
$$

### 例子：最短路径

给定无向图$v_1,v_2,\cdots,v_n$，每个边权重$\omega(e_i)$，我们需要找到每个边一个**连接权**$x_{ij}$，表示节点i和节点j之间的连接权(前提是它们之间存在边)
$$
\min \sum_{ij\in E} x_{ij}\\
s.t\quad \sum_j x_{ij} - \sum_j x_{ji}=\left\{\begin{aligned}
& 1,i\in S\\
& -1 ,i\in D\\
& 0,else
\end{aligned} \right.\\

x_{ij} = 0 \ or \ 1(表示边是否在最短路径中)
$$
将其改成$x_{ij}\geq 0$(线性约束)

## Optimization terminology(CMU)

基本形式

![image-20221020175438072](https://raw.githubusercontent.com/Gaoustcer/picbed/main/image-20221020175438072.png)

Solution Set是feasible point中最优点
