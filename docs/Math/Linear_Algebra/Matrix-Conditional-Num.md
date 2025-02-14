# 线性代数：线性方程组解的稳定性

## 引子

给定一个线性方程组
$$
Ax = b,A\in \R^{n\times n}
$$
假定A可逆，则其解为
$$
x = A^{-1} b
$$
现在考虑一个问题，对于矩阵$A=\{a_{ij}\}$中的一个元素，施加一个扰动
$$
a_{ij}^\prime = a_{ij} + \delta
$$
会对解产生多大的影响？

## 矩阵范数

向量范数刻画了向量在空间中的长度，现在我们将其定义推广到矩阵上
$$
\parallel A\parallel = \max_x\frac{\parallel Ax\parallel}{\parallel x\parallel }
$$
其中x为任意非0向量，或者定义为
$$
\parallel A\parallel =\max_{x} {\parallel A x\parallel}
$$
其中x是单位向量，此定义满足范数的三条性质

1. 非负，显然
2. 齐次，显然
3. 三角不等式

$$
\parallel A+B\parallel =\max_x \parallel Ax+B x\parallel \leq \max_x \parallel Ax \parallel + \parallel B x\parallel = \parallel A \parallel + \parallel B \parallel
$$

对于矩阵相乘，矩阵范数还满足
$$
\parallel AB\parallel \leq \parallel A\parallel \parallel B\parallel
$$

> 这是因为
> $$
> \begin{aligned}
> \parallel AB\parallel &=\max_x \parallel A B x\parallel
> \end{aligned}
> $$
> 对于任意单位向量x，均有
> $$
> \parallel AB x\parallel \leq \parallel A\parallel \parallel B x\parallel
> $$
> 这是因为，基于向量范数的定义
> $$
> \parallel Ay\parallel \leq \parallel A\parallel \parallel y\parallel
> $$
> 对于任意y均成立，因此得证

根据此定义，对于可逆矩阵A，计算其逆矩阵的范数
$$
\parallel A^{-1}\parallel =\max_y \parallel A^{-1} y\parallel/\parallel y\parallel =1/\min_{y}\frac{\parallel y\parallel}{\parallel A^{-1} y\parallel}
$$
最后我们令$y=Ax$(这是合理的，因为A可逆)得到
$$
\parallel A^{-1}\parallel  =\max_x \parallel x\parallel / \parallel Ax\parallel \Rightarrow \forall y,\parallel A^{-1}\parallel \geq \frac{\parallel y \parallel }{\parallel A y\parallel}\\
\parallel A\parallel =\max_x \parallel Ax \parallel /\parallel x\parallel\Rightarrow \forall y,\parallel A\parallel \geq \frac{\parallel Ay\parallel}{\parallel y\parallel}
$$
现在我们讨论矩阵和矩阵逆范数的乘积，根据相容性，有
$$
\parallel A^{-1}\parallel \parallel A\parallel\geq\parallel A^{-1} A\parallel = 1 
$$
我们称LHS为条件数
$$
\kappa(A) = \parallel A^{-1}\parallel \parallel A\parallel
$$

## 条件数和线性方程的稳定性

首先，我们对线性方程的稳定性加以定义，对于线性方程组
$$
Ax = b
$$
对**右侧**施加扰动，得到一组新的解
$$
A(x + \delta x) = b + \delta b
$$
即
$$
A\delta x = \delta b
$$
两边取范数，利用范数的性质得到
$$
\parallel A\parallel \parallel \delta x\parallel \geq \parallel A\delta x\parallel =\parallel \delta b\parallel\label{dis}
$$
同时我们有
$$
\parallel Ax\parallel = \parallel b\parallel
$$
根据矩阵A逆的性质
$$
\parallel A^{-1}\parallel \geq \frac{\parallel x\parallel}{\parallel Ax \parallel}
$$
得到
$$
{\parallel A^{-1}\parallel}\parallel b\parallel\geq{\parallel x\parallel}\label{ori}
$$
将$\eqref{dis}$和$\eqref{ori}$相乘，得到
$$
\kappa(A)\parallel\delta x\parallel \parallel b\parallel\geq \parallel\delta b\parallel \parallel x\parallel
$$
整理一下，得到
$$
\frac{\parallel \delta x\parallel}{\parallel x\parallel }\geq \frac{\parallel \delta b \parallel}{\parallel b\parallel \kappa(A) }
$$
得到一条主要结论

> 方程解扰动（稳定性）的lower bound取决于矩阵A的条件数，条件数越小，方程组解抵抗扰动的能力越差

进一步考虑$\frac{\parallel \delta x\parallel}{\parallel x\parallel}$的upper bound，采取类似的技巧得到
$$
\frac{\parallel \delta x\parallel }{\parallel x\parallel}\leq \kappa(A)\frac{\parallel \delta b \parallel}{\parallel b \parallel}
$$

> 稳定性的upper bounder和条件数大小逆相关，条件数越大将会导致扰动的上界大幅增加

对于正交矩阵$AA^T = I,A=A^T$，自然有
$$
\parallel A\parallel =\max_x \frac{\parallel Ax\parallel}{\parallel x\parallel} =\max_x \frac{(Ax)^T Ax}{xx^T} = 1
$$
因此正交矩阵的条件数为1，解非常稳定