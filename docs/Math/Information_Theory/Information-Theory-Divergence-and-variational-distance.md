---
title: Information Theory(Divergence and variational distance)
date: 2024-01-20 23:53:12
tags:
    - Information Theory
---
# Information Theory(Divergence and variational distance)

## Divergence and Variational distance

Divergence(Relative entropy/discrimination) of X and Y定义为
$$
D(X\parallel Y) = \sum_x p(x)\log \frac{p(x)}{q(x)}
$$

> Relative Entropy：之前提到过对某个随机事件的最小压缩长度记作
> $$
> H(X) = \sum_x p(x)\log \frac{1}{p(x)}
> $$
> 概率越小编码长度越大，平均编码长度最短
>
> 考虑数据传输模型，输入$X$输出Y，按照Y的概率对输入编码得到编码长度为
> $$
> \log \frac {1}{q(Y)}
> $$
> 此时编码长度为
> $$
> \sum_x p(x)\log \frac{1}{q(x)}
> $$
> p/q对应输入/输出端在全概率空间的分布，和最优编码$\sum_x p(x)\log \frac{1}{p(x)}$之差为$D(p\parallel q)$

### 性质

$$
D(p\parallel q)\geq 0
$$

错误的数据传输将会导致压缩效率下降

Mutual Information被定义为联合概率和边缘概率乘积之间的Divergence
$$
I(X;Y) = D(P(X,Y)\parallel P(X)P(Y))
$$
之间的差异本质上反映了Y在多少程度上受到X影响

### Refinement of Distribution

给定概率空间$\mathcal X$，在上面切分出k个不相交的子集$\mathcal U_i,i=1,2,\cdots,k$
$$
\mathcal X = \bigcup_i^k \mathcal U_i
$$
定义$U_i$上的几率(Refinement)
$$
P_U(i) = \sum_{x\in \mathcal U_i}P_X(x)
$$
划分是针对全概率空间而言的，下面的结论揭示了划分会让Divergence降低
$$
D(p\parallel q) \geq D(p_U\parallel q_U)
$$
$p_u/q_u$对应的是在划分空间上的离散分布，定义为
$$
D(p_U\parallel q_U) =\sum_{i=1}^k \sum_{x\in \mathcal U_i} P(x)\log _2\frac{\sum_{x\in \mathcal U_i}P_X(x)}{\sum_{x\in U_i}Q_X(x)}
$$
对应LHS写成
$$
\sum_{i=1}^k \sum_{x\in U_i}P(x)\log_2\frac{P(x)}{Q(x)}
$$

基于
$$
\sum_i a_i \log \frac{a_i}{b_i}\geq a\log \frac a b,\sum_i a_i =a,\sum_i b_i = b
$$

> 这是基于$f(x) = x\log x$是凹函数，即
> $$
> \begin{aligned}
> \sum_i a_i \log \frac{a_i}{b_i} &=\sum_i b_i f(\frac{a_i}{b_i})\\
> &=b\sum_i\frac {b_i}{b}f(\frac{a_i}{b_i})\\
> &\geq b f(\sum_i \frac{b_i}{b} \frac{a_i}{b_i})\\
> &=bf(\frac a b)
> \end{aligned}
> $$
> Q.E.D

这不满足数学上对distance的定义（不满足三角不等式/对称性）

## Variational Distance

提出更符合数学直觉的概率距离定义
$$
\parallel P_X - P_Y\parallel= \sum_{x\in \mathcal X}|P_X(x) - P_Y(x)|
$$
满足
$$
\parallel P_X - P_Y\parallel = 2\sum_{x\in \mathcal X,P_X(x)>P_Y(x)}|P_X(x) - P_Y(x)| = 2\sup_{E\subset \mathcal X}|P_X(E) - P_Y(E)|
$$
实际上 $\sup E$满足
$$
\forall x\in E,P_X(x) \geq P_Y(x),\forall x\notin E,P_X(x) < P_Y(x)
$$

### Pinsker’s inequality

揭示了Divergence和Variational distance之间的差别
$$
D(X\parallel Y)\geq  \frac{\log_2 e}{2}\parallel P_X - P_Y\parallel^2
$$
按照Variational Distance定义切割
$$
U = \{x\in \mathcal X|P_X(x)\geq P_Y(x)\}
$$
一定有
$$
D(X\parallel Y)\geq D(P_X(U)\parallel P_Y(U))
$$
希望证明
$$
D(P_X(U)\parallel P_Y(U)) \geq 2\log_2 e |P_X(U) - P_Y(U)|^2
$$
定义$P_X(U) = p,P_Y(U) = q$，写成
$$
p\ln \frac p q + (1 -p )\ln \frac {1- p}{1-q} \geq 2 (p-1)
$$
得证

### Conditional Divergence

给定条件z，是否会帮助减少两者的Divergence
$$
D(X\parallel Y|Z) = \sum_z P_Z(z) D(X|z\parallel Y|z)
$$
定义Conditional Mutual Information
$$
I(X;Y|Z) = \sum_z P_Z(z) I(X|z;Y|z) = \sum_z P_Z(z) D(P(X,Y|z)\parallel P(X|z)P(Y|z))
$$

给定conditional，能否帮助减少两个分布之间的差异

#### Chain Rule for Divergence

对于两个二维分布$P(X_1,X_2),Q(X_1,X_2)$以及对应的边缘分布$P(X_1),Q(X_1)$
$$
D(P_{X_1,X_2}\parallel Q_{X_1,X_2}) = D(P_{X_1}\parallel Q_{X_1}) + D(P_{X_2|X_1}\parallel Q_{X_2|X_1}|P_{X_1})
$$
先看第一维的分布差异，再看条件分布的差异

#### Condition will not Decrease Divergence

给定先验条件不会让两个分布之间的距离变小（注意和条件熵的区别，条件熵只会让信息量减小）
$$
D(P_{X|Z}\parallel P_{Y|Z}|P_Z) \geq D(P_X\parallel P_Y)
$$
如果Z和X/Y相互独立，则取等号

