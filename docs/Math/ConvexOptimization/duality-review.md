---
title: duality review
date: 2023-04-09 18:47:46
tags:
 - Convex Optimization
---
# Convex Optimization——Duality(review)

## Dual Problem and Primal Problem

原始问题
$$
\min f_0(x)\\
s.t \ f_i(x)\leq 0,i=1,2,\cdots,m\\
h_j(x) = 0,j=1,2,\cdots,p
$$
拉格朗日函数
$$
L(x,\lambda,v) = f_0(x) +\sum_{i=1}^{m} \lambda_i f_i(x) + \sum_{j=1}^p v_j h_j(x)
$$
定义拉格朗日对偶函数
$$
g(\lambda,v) =\inf_x L(x,\lambda,v)
$$
对偶函数两条性质

1. 一定是凸函数
2. 一定是原问题最优解$p^*$的lower bound $g(\lambda,v) \leq p^*$

求解对偶问题
$$
\max_{\lambda,v} g(\lambda,v)\\
s.t \lambda \geq 0
$$

1. $d^*\leq p^*$称为弱对偶
2. $d^*=p^*$称为强对偶

> 共轭函数，定义函数$f(x)$的共轭函数是
> $$
> f^*(y) = \sup_{x\in dom f} (y^T x-f(x))
> $$
> 定理：$f^*$是凸函数
>
> 例子，范数的共轭函数
> $$
> f^*(y) = \sup_{x\in dom f} (y^T x -\parallel x\parallel)
> $$
> 满足
> $$
> f^*(y) =\left\{
> \begin{aligned}
> & 0,\parallel y \parallel \leq 1\\
> & \infty,else
> \end{aligned}
> \right.
> $$
> 

> 例子，对于二元函数
> $$
> z = f(x,y) 
> $$
> 求解在约束
> $$
> g(x,y) = 0
> $$
> 条件下的极值点，根据拉格朗日函数
> $$
> F(x,y,\lambda) = f(x,y )+ \lambda g(x,y)
> $$
> 求偏导
> $$
> \frac{\partial F}{\partial x} = \frac{\partial f}{\partial x} + \lambda \frac{\partial g}{\partial x} = 0\\
> \frac{\partial F}{\partial y }= \frac{\partial f}{\partial y} + \lambda \frac{\partial g}{\partial y} = 0\\
> \frac{\partial F}{\partial \lambda} = f(x,y) = 0
> $$
> 上述方程的解$(x,y)$可能是函数在约束条件下的极值点，含义找到$f(x,y)$等高线和$g(x,y) =0$相切的

> 最小化范数
> $$
> \min \parallel x\parallel\\
> s.t\quad Ax = b
> $$
> 拉格朗日函数写成
> $$
> g(v) =\inf_x (\parallel x\parallel + v^T (b-Ax)) 
> $$
> 

## 何时强对偶成立？

### Slater条件

#### 相对内点

C为非空凸集，x为C的相对内点，如果$x\in C$并且存在开球$S_x$使得
$$
aff(C)\bigcap S \subset C
$$
称x为C的相对内点，C的所有相对内点集合记作$ri(C)$

#### Slater条件

1. 原问题是凸问题
2. 存在可行域相对内点$x\in relint(D)$，满足所有约束
