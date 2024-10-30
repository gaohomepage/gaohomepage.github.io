---
title: optimize transport
date: 2023-06-19 15:45:26
tags:
    - Contrast Learning
    - Math in Machine Learning
---
# 最优传输问题

## 推土机距离

### Problem Setting

给定n个土堆，包含$s_i$量的土，给定m个坑，每个坑可以容纳$d_j$量的土。土和土堆表示为
$$
s \in \R^n.d\in \R^m
$$
假设
$$
\sum_j s_j = \sum_j d_j
$$
定义距离矩阵$M\in \R^{n\times m}.m_{ij}$表示将单位质量的土从堆i搬到坑j需要的代价，希望得到一个传输矩阵$P\in \R^{n\times m}$，使得代价
$$
\lang M,P\rang= \sum_i \sum_j m_{ij} p_{ij}
$$
最小，显然矩阵P应该满足
$$
\sum_{j} p_{ij} = s_i\to P^T 1_{m} =s\\
\sum_i p_{ij} = d_j\to P 1_n = d\\
p_{ij}\geq 0
$$
这是一个线性规划问题

### 推广推土机距离到概率分布

假设s，r都是概率分布，即
$$
\sum_j s_j =\sum_i d_i =1 
$$
最优推土机距离可以表示为两个离散概率分布(维度不一定相同)之间的距离。这种方法被广泛用在无监督学习或者半监督学习的loss函数设计上，比如对比学习中，希望同一个instance通过不同transform得到的概率分布更加相似

## 熵正则的最优传输问题

### 定义：熵正则

矩阵P的熵定义为
$$
H(P) =-\sum_{i,j}p_{ij}\log p_{ij}
$$
矩阵熵越大，说明对应的分布越分散，不希望分布过于集中，因此损失函数记作
$$
D_{M,\lambda}(P) = \inf\lang P,M\rang -\frac1\lambda H(P)
$$
不加正则项，线性规划问题最优解一定落在多边形定点上，导致分布不均衡，在这种情况下可以用`Sinkhorn`迭代得到最优解

### `Sinkhorn`算法

不断在两个向量$u\in \R^n,v\in \R^m$上迭代，初始设置为均匀分布
$$
 u = [\frac 1 n],v =[\frac 1 m]
$$
迭代
$$
u = \frac{s}{ e^{-\lambda M}\cdot v}\\
v = \frac{d}{ e^{-\lambda M}\cdot u}
$$

> $e^{\lambda M}$是一个$n\times m$的矩阵，除法代表两个向量逐个元素相除

迭代收敛$u^*,v^*$，这种情况下最优传输矩阵写成
$$
P^* = diag(u) e^{-\lambda M} diag(v)
$$

### 对比学习中最优传输

![image-20230302170254282](https://s2.loli.net/2023/03/02/uXVoJCvH92OGj6Z.png)

一个Batch包含B个样本，经过feature extraction和两种数据增强后生成一对正的embedding(嵌入维度是m)
$$
z_s,z_t \in \R^m，Z_t= (z_t^1,z_t^2,\cdots,z_t^B)\in \R^{m\times B},Z_s = (z_s^1,z_s^2,\cdots,z_s^B)\in \R^{m\times B}
$$
通过在线聚类（计算一个cluster内K-means）得到K个聚类中心
$$
C = (c_1,c_2,\cdots,c_K)\in \R^{m\times K},c_i\in \R^m
$$
一组$z_s,z_t$在聚类中心c下计算的对比损失记作
$$
l(z_s,z_t) = -\sum_i q_s^i \log p_t^i
$$
$q_s,p_t\in \R^K$来自$z_s,z_t$，可以认为原始embedding在K个中心作用下的投影，$p_t^i$计算为
$$
p_t^i =\frac{\exp{\frac 1 \tau z_t^T c_k}}{\sum_j \exp(\frac{1}{\tau} z_t^T c_j)}
$$
向量$q_s$的求解转化为最优传输问题，实际上将$z_s^i$生成的q向量记作$q_{i}  =(q_{i}^1,q_{i}^2,\cdots,q_{i}^k) \in \R^K$记矩阵
$$
Q =(q_1,q_2,\cdots,q_B) \in \R^{K\times B},q_i\in \R^K
$$
Q是如下最优传输问题的解
$$
\max Tr(Q^T C^T Z) +\epsilon H(Q)
$$
$C\in \R^{m\times K},Z\in \R^{m\times B},C^TZ = \R^{K\times B},Q^TC^T Z = \R^{B\times B}$
$$
C^T Z = (\lang c_i,z_j\rang)_{i,j}^{K,B} = \\
Tr(Q^TC^T Z) = Tr(\lang q_i,y_j\rang)_{i,j}^{B\times B},y_j = (\lang c_1,z_j\rang,\lang c_2,z_j\rang,\cdots,\lang c_K,z_j\rang)\in\R^K,q_i\in \R^K\\
Tr(Q^T C^T Z) = \lang  Q,C^T Z\rang 
$$
实际上求解的是代价矩阵为$C^T Z,K\to B$的最优传输问题，对应的原分布为K,B上的均匀分布

