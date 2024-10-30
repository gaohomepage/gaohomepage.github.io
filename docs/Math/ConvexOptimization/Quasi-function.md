---
title: Quasi function
date: 2023-04-09 18:41:27
tags:
 - Convex Optimization
---
# Convex Optimization——Quasi function

## Quasi Convex function

### 定义一

所有$\alpha$-sublevel set都是凸的函数

> $$
> f(x)=\left\{
> \begin{aligned}
> & \sqrt x,x\geq 0\\
> & \sqrt {-x},x\leq 0
> \end{aligned}
> \right.
> $$

Quasi Convex定义为
$$
S_\alpha = \{x\in dom f|f(x)\leq \alpha \}
$$
convex $\forall \alpha \in \R$

同样定义Quasi Concave
$$
S_\alpha^\prime = \{x\in dom f|f(x)\geq \alpha \}
$$
对于$\forall \alpha \in \R$是convex的

定义Quasi linear
$$
S_\alpha^{\prime \prime}=\{x\in dom f|f(x)=\alpha \}
$$
是凸集

Quasi convex优化起来比较容易，尽管不能用convex function方法分析

### 定义二

拟凸下，有
$$
\forall x,y\in dom f,\max(f(x),f(y))\geq f(\theta +(1-\theta(y)))
$$

### 例子

> 向量$x\in \R^n$的长度，定义为x中最后一个非零元素的个数
> $$
> f(x)=\left\{
> \begin{aligned}
> & \max_{i,x_i\neq 0} i,x\neq 0\\
> & 0,x=0
> \end{aligned}
> \right.
> $$
> $f(x)$是拟凸函数，对于它的$\alpha-$sublevel set
> $$
> f(x)\leq \alpha \to x_{\alpha+1},x_{\alpha+2},\cdots,x_{n} = 0
> $$
> 显然是凸集

> 线性分数函数
> $$
> f(x) =\frac{a^Tx+b}{c^T x+d}\\
> dom f = \{x|c^T x+d >0 \}
> $$
> 是拟凸的
> $$
> S_\alpha \to \{x|c^T x+d >0,a^T x+b \leq \alpha (c^T x+d) \}
> $$
> 多面体一定是凸集

> $$
> f(x) = (x_1x_2\cdots x_n)^{\frac{1}{n}}在\R_{++}^n\bigcap \{\parallel R\parallel _2\leq 1\}上是凹函数
> $$
>
> $\forall x,y\in dom f,\theta\in (0,1)$
> $$
> \theta (x_1x_2\cdots x_n)^{1/n}+(1-\theta)(y_1y_2\cdots y_n)^{1/n}\leq {\prod_{i=1}^n \theta x_i +(1-\theta) y_i} ^{1/n}
> $$
> 

> 向量的0范数(非0元素的个数之和)，考虑在一个集合中找稀疏向量
> $$
> \min \parallel x\parallel_0\\
> s.t x\in C
> $$
> 这既不是凸问题也不是拟凸问题
>
> 考虑近似问题(松弛)
> $$
> \min \parallel x\parallel_1\\
> s.t x\in C
> $$
> 变成凸问题
>
> 或者用另一种方法近似
> $$
> f(x) = \log (ax^2 +1)
> $$
> 变成
> $$
> \min \log x^x +1\\
> x\in C
> $$
> 是拟凸函数

## 拟凸函数的其它定义——一阶条件和二阶条件

### 一阶条件

dom f为凸，并且
$$
f(y)\leq f(x)\Leftrightarrow \nabla ^T f(x)(y-x)\leq 0
$$
$dom f\in \R$

> Quasi convex具有
> $$
> \max \{f(x),f(y) \}\geq f(\theta x+(1-\theta y))
> $$
> 则写成
> $$
> f(x)\geq f(\theta x+(1-\theta)y)
> $$
> 梯度写成
> $$
> f^\prime (x) =\lim_{t\to 0}\frac{f(x+t)-f(x)}{t}=\lim_{y\to x}\frac{f( x+(1-\theta)(y-x))-f( x)}{(1-\theta )(y-x)}
> $$
> 写成
> $$
> \frac{f(\theta x+(1-\theta)y)- f(x)}{(1-\theta )(y-x)}(1-\theta)(y-x)\leq 0\\
> \theta \to 1\\
> f^\prime(x)(y-x)\leq 0
> $$
> $\Leftarrow$得证
>
> 考虑
> $$
> \max\{f(y),f(x)\}- f(\theta x+(1-\theta )y)=f(x)-f(\theta x +(1-\theta )y)
> $$
> 写成
> $$
> \begin{aligned}
> \frac{f(x)-f(\theta x +(1-\theta )y)}{(1-\theta)(x-y)}(1-\theta) (x-y)=f^\prime(x)(x-y)\geq 0
> \end{aligned}
> $$
> (这个证明貌似只是在$\theta \to 1$)成立？这里貌似要推广到$\forall \theta \in (0,1)$
>
> > 在第21讲中修正，需要证明
> > $$
> > \forall x,y\in dom f,f(y)\leq f(x) ,\theta\in [0,1]\\
> > \max\{f(x),f(y) \}\leq f(\theta x + (1-\theta) y)
> > $$
> > 令 $z= \theta x+(1-\theta )y$，我们证明
> > $$
> > f(z)\geq f(x)\to f(x)= f(z)
> > $$
> > 如果有$f(z)\geq f(x)$，则
> > $$
> > f(x)\leq f(z),f(y)\leq f(z)
> > $$
> > 同时得到
> > $$
> > f^\prime(z) (y-z)\leq 0\\
> > f^\prime (z)(x-z)\leq 0
> > $$
> > 带入z，得到
> > $$
> > f^\prime(z) (\theta (y-x))\leq 0 \to f^\prime(z)(y-x)\leq 0 \\
> > f^\prime(z) ((1-\theta)(x-y))\leq 0\to f^\prime(z)(x-y)\leq 0
> > $$
> > 因此有
> > $$
> > f^\prime (z)=0
> > $$
> > 则总是存在$z_1$，满足
> > $$
> > f(z_1)\geq f(x)
> > $$
> > 同时$f^\prime (z_1)= 0$，并且对于$\forall z^\prime \in [x,z]$，这个结论都成立

对于convex function，如果有$\nabla f(x)=0$，则$\forall y,f(y)\geq f(x)$

> 这个结论对于Quasi function并不成立
>
> >  对于定义域内均可微的函数，是否最小点一定有$\nabla  f(x)=0$?

### 二阶条件

如果f为拟凸函数，dom f为凸，且
$$
y^T \nabla f(x) \geq 0(每个元素都非负)
$$
一定有
$$
y^T \nabla^2 f(x) y \geq 0
$$

> n = 1 的情况，如果$y\neq 0$，则$f^\prime(x) \geq 0$

如果不满足拟凸函数的二阶条件，则不为二阶条件

## Log concave && Log convex

### 定义

$f(\R^n \to \R)$为log concave，若$f(x)>0,\forall x\in dom f$，且$\log f$为凹函数

### Relations between log concave

1. f为log convex，则f为convex
2. f为concave，则f为log concave
