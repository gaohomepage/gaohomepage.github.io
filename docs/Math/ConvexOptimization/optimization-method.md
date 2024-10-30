---
title: optimization method
date: 2023-04-09 18:49:29
tags:
 - Convex Optimization
---
# Convex Optimization(优化算法)

设计算法两个原则是

1. 所有优化算法都是迭代算法
2. 迭代第k步，进行更新

$$
x^{k+1} = x^k + \alpha^k d^k
$$

1. $d_k$：k时刻的方向
2. $\alpha^k$：步长

选择让目标函数尽可能小的步长，优化问题变成
$$
\alpha^k = \arg\min_{\alpha\geq 0} f_0(x^k +\alpha  d^k)\\
g(\alpha) = f_0(x^k+\alpha d^k)
$$
如果$f_0(x)$是凸函数，则$g(\alpha)$是$\alpha$的凸函数

## Line search

在一维line $\alpha \geq 0$上搜索

### 黄金分割

$g(0) = f_0(x^k)$，假设我们需要在$[0,\alpha_\max]$中搜索$\alpha$，将区间分成三段($\beta = 0.618$是黄金分割比)

1. $[0,(1-\beta)\alpha_\max]$
2. $[(1-\beta)\alpha_\max,\beta \alpha_\max]$
3. $[\beta \alpha_\max,\alpha_\max]$

比较$x_1 =(1-\beta)\alpha_\max,x_2 = \beta \alpha_\max$处函数值

1. 如果$x_1<x_2$，舍弃$[x_2,\alpha_\max]$
2. $x_1>x_2$，舍弃$[0,x_1]$

### Amijo Rule

若$f_0(x^k+\alpha d^k)> f_0(x^k)+\gamma \alpha \nabla f_0^T(x^k)d^k$，则
$$
\alpha  = \alpha \beta
$$
否则算法停止

$\gamma$表明我们希望函数值下降的程度，$\beta$是在函数值下降的情况下使得步长尽量小

对$g(\alpha)$泰勒展开到一阶
$$
f_0(x^k) + \alpha \nabla f^T (x^k) d^k
$$
对应$\gamma =1$时的直线

减小$\gamma$意味着选择一条不那么**陡峭**的下降路线

$\gamma = 1$满足在邻域内均有
$$
g(\alpha )\geq f_0(x^k) +\alpha \nabla f^T(x^k) d^k,\forall \alpha \leq \alpha_{bound}
$$
这仅仅在邻域内成立，迭代算法从$\alpha_\max$出发，最开始必然满足(3)，逐步减少$\alpha$使得落在直线下方

$\gamma$被设置为[0,0.5]，保证每一步得到足够多的优化

## 例题

### 第一题

证明，给定问题1
$$
\max_{v,\lambda_1,\lambda_2} -b^T v -\lambda^T_1 u +\lambda_2^T l\\
s.t A^T v +\lambda_1 -\lambda_2 +c = 0\\
\lambda_1 \geq 0,\lambda_2\geq 0
$$
问题2
$$
\max_v -b^T v +l^T (A^T v+c )^+-u^T (A^T v+c)^-
$$
等价

> 两个优化问题是等价指的是两者最优解相同，即满足$v^*$是问题1的最优解，它必然是问题2的最优解

令$\lambda_2 = (A^T v+c)^+,\lambda_1 = (A^Tv+c)^-$，则问题2和下列问题完全等价
$$
\max_v -b^T v + l^T \lambda_2-u^T \lambda_1\\
\lambda_2 = (A^T v+c)^+\\
\lambda_1 = (A^T v+c)^-\\
$$
约束显然满足
$$
A^T v+c= \lambda_1 -\lambda_2
$$

### 第二题

若$g(x,u,w)$为凸，证明
$$
p(u,w) =\inf_x g(x,u,w)
$$
为凸

> $\forall (u_1,w_1),(u_2,w_2)\in dom g,\forall \theta\in (0,1)$
> $$
> u=\theta u_1 + (1-\theta)u_2\\
> \begin{aligned}
> g(u,w) &= \inf_x g(x,u,w) \\
> &=\inf_x g(x,\theta u_1 +(1-\theta)u_2,\theta w_1 +(1-\theta) w_2)\\
> &\geq \inf_x  \theta g(x,u_1,w_1)+(1-\theta) g(x,u_2,w_2) \\
> \end{aligned}
> $$
> 
>Q.E.D
> 

## 优化函数f(x)性质分析(强凸性)

### 迭代搜索的最优性条件

对于凸函数$f(x)$，若其一阶可微，则最优解$x^*$必然满足
$$
\nabla f(x)
$$
考虑$\nabla f(x)=\epsilon \to 0$，此时是否有
$$
x\to x^*\\
f(x)\to f(x^*)
$$

> 优化问题的最优解和最优值同等重要

### 强凸性

假设$f(x)$二阶可微并具有强凸性质
$$
\exist m,\forall x\in dom f,\nabla^2f(x) \succeq m I
$$
> $\nabla ^2 f(x)- mI$仍然是半正定矩阵，减去一个$m(x_1^2+x_2^2+\cdots +x_n^2)$仍然是凸函数

等价于
$$
\forall x,y\in dom f ,f(y)\geq f(x)+\nabla ^Tf(x)(y-x)+\frac{1}{2}m \parallel y -x \parallel_2^2,m>0
$$

$m=0$退化为凸函数一阶条件，令$y = x+\Delta x$，得到
$$
f(x+\Delta x) \geq f(x)+\nabla f^T (x)\Delta x+\frac{1}{m} \parallel\Delta x\parallel_2^2
$$

### 强凸性保证下，是否有$\nabla f(x) \to 0\to f(x)\to f(x^*)$？

给定x
$$
g(y) = f(x) +\nabla f^T(x)(y-x) +\frac{1}{2}m \parallel y -x \parallel_2^2
$$
是关于y的凸函数，得到
$$
\frac{\partial g}{\partial y} = \nabla f(x) + m(y -x)
$$
最优满足
$$
y^* = x - \frac{\nabla f(x)}{m}
$$
带入得到
$$
g(y) \geq g(y^*) =f(x)-\frac{1}{2m}\parallel \nabla f(x)\parallel_2^2
$$
对$\forall x$均成立

结合强凸性，有
$$
\forall x,y, f(y) \geq g(y) \geq g(y^*) =f(x)-\frac{1}{2m}\parallel f(x)\parallel_2^2
$$
令$y=x^*$，为优化问题的最优解
$$
p^*= f(x^*) \geq f(x)-\frac{1}{2m}\parallel \nabla  f(x)\parallel_2^2\Leftrightarrow p^*\geq f(x)-\frac{1}{2m} \parallel \nabla f(x)\parallel_2^2
$$
再做一些简单的等价变换
$$
\begin{aligned}
p^* +\frac{1}{2m} \parallel \nabla f(x)\parallel_2^2\geq f(x)\geq p^*
\end{aligned}
$$
任意给定$f(x)$，一定满足上述不等式(上界bound和下界bound)，等价于
$$
| f(x) - p^* |\leq \frac{1}{2m}\parallel \nabla f(x)\parallel_2^2
$$
显然$\nabla f(x)\to 0$，此时$|f(x)-p^*|$被bound住

> 以上结论均在优化目标满足强凸条件下获得，本小节我们从强凸的一个等价定义出发，根据全局最优的定义推出了一个在全局定义域上都成立的，证明了最优值和当前梯度的函数值的关系，从而得到强凸条件下目标函数收敛的速度，本节还推出了最优值的lower bound

### 考虑$\nabla f(x)\to 0$是否有$x\to x^*$

仍然有
$$
p^*\geq f(x)+\nabla f^T(x)(x^*-x)+\frac{m}{2}\parallel x^* -x\parallel_2^2
$$
Cauchy Schwaz不等式
$$
\lang a,b\rang +\parallel a\parallel \parallel b\parallel \geq 0\\
a = \nabla f^T(x), b = x^* -x\\
\nabla f^T(x)(x^*-x)\geq -\parallel \nabla f^T(x)\parallel \parallel x^* -x \parallel
$$
进一步有
$$
f(x)\geq p^* \geq f(x)-\parallel \nabla f^T(x)\parallel \parallel x^* -x\parallel_2 +\frac{m}{2}\parallel x^*-x\parallel_2^2
$$
令$\Delta x = \parallel x^* - x\parallel$，有
$$
\parallel \nabla f^T(x)\parallel\Delta x\geq \frac{m}{2}\Delta x ^2\\
\Delta x\leq \frac{2}{m} \parallel \nabla f^T(x)\parallel
$$

> 1. m代表函数的**凸性**，如果m越大，则梯度可以更加显著地表示最优解和最优值的接近程度。本质上体现了优化难度
> 2. 函数值的上界比最优解的上界更小

### 凸性的上界

$$
\exist M >0,\forall x\in dom f,\nabla ^2f(x)\preceq MI
$$

等价定义
$$
\forall x,y\in dom f,f(y)\leq f(x)+\nabla f^T (x)(y-x)+\frac{M}{2}\parallel y -x \parallel_2^2
$$
同样有如下结论
$$
p^* \leq f(x)-\frac{1}{2M}\parallel \nabla f(x)\parallel_2^2
$$

### Bound of Hessian Matrix

|                            | $\nabla ^2 f(x)\preceq MI$                                   | $\nabla ^2f(x)\succeq mI$                                    |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 等价定义                   | $\forall x,y\in dom f,f(y)\leq f(x)+\nabla f^T(x)(y-x)+\frac{M}{2}\parallel y -x \parallel_2^2$ | $\forall x,y\in dom f,f(y)\geq f(x)+\nabla f^T(x)(y-x)+\frac{m}{2}\parallel y -x \parallel_2^2$ |
| Optimal Value and Gradient | $p^* \leq f(x)-\frac{1}{2M}\parallel \nabla f(x)\parallel_2^2$ | $p^* \geq f(x)-\frac{1}{2m}\parallel \nabla f(x)\parallel_2^2$ |



## 梯度下降收敛性分析

### 梯度下降

$d^k = -\nabla f(x_k)$，迭代
$$
x_{k+1} = x_k + \alpha^k d^k=x_k -\alpha_k \nabla f(x_k)
$$
精确搜索指的是在确定步长时，每一步都是求解
$$
\alpha_k = \arg\min_\alpha f(x_k+\alpha d_k)
$$

> 下文中将会证明，梯度下降下无论是否采用精确收敛，都有很好的收敛性质

假设
$$
\forall x\in domf ,MI \succeq  \nabla ^2 f(x)\succeq mI
$$

#### 精确搜索exact line search

定义
$$
\hat f(\alpha) = f(x_k -\alpha \nabla f(x_k))
$$
是$\alpha$的函数，exact line search下有
$$
\hat f(\alpha) = f(x_{k+1})
$$
满足强凸，得到
$$
f(x_{k+1})\leq f(x_k)- \nabla f^T(x_k)\alpha \nabla f(x_k)+\frac{M}{2}\parallel \alpha \nabla f(x)\parallel^2_2\\
\hat f(\alpha) \leq f(x_k)-\alpha \parallel \nabla f(x_k)\parallel_2^2 +\frac{M\alpha^2}{2}\parallel \nabla f(x)\parallel_2^2
$$
$f(x_k)$视为一个常数，则$\hat f(\alpha)$始终被一个关于$\alpha$的二次函数bound住，令$\nabla = \parallel \nabla f(x)\parallel_2^2$，右侧写成
$$
g(\alpha) = f(x_k)-\alpha \nabla +\frac{M\alpha^2}{2}\nabla\\
\frac{\partial g(\alpha)}{\partial \alpha} = -\nabla +M\alpha \nabla = 0\to \alpha  = \frac{1}{M}
$$
必然有
$$
\min_\alpha \hat f(\alpha) \leq f(x_k) -\frac{1}{M}\nabla +\frac{1}{2M }\nabla = f(x_k) -\frac{1}{2M} \nabla
$$
左边对应精确搜索(精确搜索得到的最优解被bound住)

> 给定梯度至少会有$\frac{1}{2M }\nabla$的下降
> $$
> f(x_{k+1})\leq f(x_k)-\frac{1}{2M}\parallel \nabla f(x_k)\parallel_2^2\to p^* \leq f(x_k)-\frac{1}{2M}\parallel \nabla f(x_k)\parallel_2\label{equal1}
> $$
>  也可以得到完全等价的形式
> $$
> f(x_{k+1})+\frac{1}{2M}\parallel \nabla f(x_k)\parallel_2^2 -p^* \leq f(x_k) - p^*\label{equal}
> $$

同时最优值满足
$$
p^* \geq f(x_k) -\frac{1}{2m}\parallel \nabla f(x_k)\parallel_2^2\label{less}
$$

对$\eqref{less}$做等价变换
$$
f(x_k)-\frac{1}{2m}\parallel \nabla f(x_k)\parallel_2^2 -p^*\leq 0 \label{equal3}
$$
将$\eqref{equal}$两边同乘以M，$\eqref{equal3}$两边同乘以m，相加得到
$$
M(f(x_{k+1})-p^*) + m(f(x_k)-p^*)\leq M(f(x_k)-p^*)\Rightarrow  (f(x_{k+1})-p^*)\leq \frac{M-m}{M}(f(x_k)-p^*)\label{conspeed}
$$
$\eqref{conspeed}$实际上表明了收敛到最优点的速度

