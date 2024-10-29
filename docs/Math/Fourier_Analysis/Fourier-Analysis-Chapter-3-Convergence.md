---
title: Fourier Analysis Chapter 3 Convergence
date: 2024-05-08 10:49:52
tags:
    - Fourier Analysis
    - Math
---
# Fourier Analysis(Chapter 3, Convergence)

## Review of Chapter 2

第二章中，我们讨论了定义在circle上的函数的傅里叶级数
$$
S_N(f)(\theta) = \sum_{n=-N}^N \hat f(n) e^{in\theta} = D_N * f(\theta),D_N(\theta) = \sum_{n=-N}^N e^{in\theta}
$$
的一些基本性质，例如

> 可积函数的傅里叶系数如果处处为0，则其在连续点处为0（Uniqueness）

另外引入了一种函数算子—卷积，定义为
$$
f*g(\theta) = \frac 1 {2\pi}\int_{-\pi}^\pi f(\phi) g(\theta - \phi)d\phi
$$
卷积的一个很好的性质是能仅仅从可积推出连续，卷积函数的傅里叶系数等于两个函数的乘积，我们希望得到一组函数族$\{K_n(\theta)\}$，使得
$$
\lim_{n\to \infty} K_n * f(\theta )= f(\theta)
$$
满足上述规则的kernel称为good kernel，这里收敛指的是逐点收敛
$$
\forall \theta,\epsilon >0,\exist N_\theta^\epsilon,s.t\ \forall n > N_\theta^\epsilon, |K_n * f(\theta ) - f(\theta)|<\epsilon
$$
但是很不幸的是$D_N$并不是一个Good Kernel，因此我们需要对函数收敛的定义进行延拓，这里引入Cesaro means(引入一个来自Dirichlet kernel的good kernel)
$$
\sigma_N =\frac{\sum_{i=0}^{n-1}s_i}{n},s_i = \sum_{j=0}^{i} c_i
$$
$\sigma_N$被称为$N^{th}$ Casaro mean of sequence $\{s_k\}$，如果$\sigma_N \to \sigma$则称$\sum c_n$是Cesaro summable to $\sigma$

对于Fourier Series，定义Fejer Kernel
$$
F_N(x) = \frac{\sum_{i=0}^{N-1}D_i(x)}{N} =\frac 1 N \frac{\sin^2 (Nx/2)}{\sin^2 (x/2)}
$$
对应
$$
f*F_N(x) = \sigma_N(f)(x) = \frac{\sum_{i=0}^{N-1}S_i(f)(x)}{N}
$$
在这种情况下存在逐点收敛
$$
\lim_{N\to \infty} F_N * f(\theta) = f(\theta)
$$
这种情况下，对于circle上连续函数一定存在三角多项式$\frac{\sum_{i=0}^{N-1}S_i(f)(x)}{N}$逼近之

这个good kernel表明

> 对于circle上的可积函数$f$，则一定有f的Fourier级数$\sum_{|n|\leq N} a_n e^{in\theta}$在任何一个连续处$\theta$都Cesaro summable to f的

这种情况下自然得到

1. $\hat f(n)=0\to f=0$
2. 可以借助三角多项式满足驻点收敛 $\lim_{n\to \infty} |f(x) - P_n(x)| = 0$



将这种情况推广到连续场景，定义序列$c_1,c_2,\cdots,c_n$的Abel sum
$$
A(r) = \sum_{k=0}^\infty c_k r^k
$$
如果$A(r)$对于$\forall r\in [0,1)$均收敛且$\lim_{r\to 1} A(r) = s$，则称$A(r)$为Abel means，提出这个是为了解决圆盘上的收敛问题，对于$c_k(\theta)  = a_k e^{ik\theta}$，其Abel means写成
$$
A_r(f)(\theta) = \sum_{n=-\infty}^\infty r^{|n|} a_n e^{in\theta} = P_r * f(\theta)\\
P_r(\theta) =\sum_{n=-\infty}^\infty r^{|n|} e^{in\theta} = \frac{1 - r^2}{1 - 2r\cos\theta + r^2}
$$
Poisson Kernel是一个good kernel，这意味着$\forall \delta >0,\lim_{r\to 1}\int_{\delta \leq |\theta|\leq \pi} P_r(\theta)d \theta\to 1$，同时根据good kernel的性质得到
$$
\lim_{r\to 1} P_r * f(\theta)  = f(\theta),\forall \theta\in [-\pi,\pi]
$$
引入Poisson Kernel是因为其对应圆盘上传热问题的分离变量解$u(r,\theta) = A_r(f)(\theta)$，这里$f$对应的是边界条件

本章的最后一个定理证明$u(r,\theta)$是满足偏微分方程$\Delta u = 0$和边界条件的唯一解，这里利用了傅里叶系数的唯一性

## Overview of Chapter 3

上一章中，因为Dirichlet Kernel并不是good kernel，因此我们需要其它工具以证明
$$
S_N(f)\to f
$$

## Mean-square convergence of Fourier Series

给定circle上的可积函数$f$，一定有
$$
\lim_{N\to \infty} \frac 1{2\pi} \int_0^{2\pi}|f(\theta) - S_N(f)(\theta)|^2 d\theta \to 0
$$

### 向量和内积空间

#### 例子1：无限维度的复数空间

定义复数C上的向量空间$l^2(\Z)$，是一个双向无限序列
$$
(\cdots,a_{-n},\cdots,a_{-1},a_0,a_1,\cdots,a_n,\cdots)
$$
满足
$$
\sum_{n\in \Z} |a_n|^2<\infty
$$
基于此定义内积为
$$
(A,B) =\sum_{n\in \Z} a_n\overline{b_n}
$$
定义A的norm为
$$
\parallel A\parallel = (\sum_{n\in \Z}|a_n|^2)^{1/2}
$$
一个例子是$A_N$，即对于$|n|>N,a_n=0$
$$
A_N = (\cdots,0,0,a_{-N},\cdots,a_{-1},a_0,a_1,\cdots,a_{N},0,0,\cdots)
$$
内积满足两个条件的空间被定义为希尔伯特空间

1. 严格半正定 $\parallel X\parallel = 0\Leftrightarrow X = 0$
2. 完备的向量空间，任何柯西列都能收敛于本身

#### 例子2：函数空间

对于$[0,2\pi]$上可积的函数组成的空间$\mathcal R$和上面的两个函数f/g，定义函数加法
$$
(f+g)(\theta) = f(\theta) + g(\theta)
$$
定义内积
$$
(f,g) = \frac 1 {2\pi}\int_0^{2\pi} f(\theta)\overline g(\theta) d\theta
$$
对应函数的模为
$$
(\frac 1 {2\pi} \int_{0}^{2\pi} |f(\theta)|^2 d\theta)^{1/2}
$$
这种情况下也一定有Cauchy-Schwarz不等式成立，即
$$
|(f,g)| \leq \parallel f\parallel \parallel g\parallel
$$
这种情况下，是否满足Hilbert Space的第一个定义呢？答案是不一定的，可以轻松构造一个函数在区间上某个点不连续，但是其它位置处处为0，但是这种函数满足**测度为0**

同时函数空间$\mathcal R$不是完备的，对于函数
$$
f(\theta) =
\left\{
\begin{aligned}
& 0,\theta  = 0\\
& \log (\frac 1 \theta),0<\theta\leq 2\pi
\end{aligned}
\right.
$$
定义函数序列
$$
f_n(\theta) = \left\{
\begin{aligned}
& 0,0\leq \theta \leq \frac 1 n\\
& f(\theta), \frac 1 n \leq \theta \leq 2\pi
\end{aligned}
\right.
$$

$f$在区间上是unbounded的，因此不属于$R$

### 证明均方收敛

继续讨论circle上的可积函数空间$\mathcal R$，我们希望证明
$$
d(f,S_N(f)) \to 0,N\to \infty
$$
定义$e_n(\theta) = e^{in\theta}$，满足(orthonormal)
$$
(e_n,e_m) = 1,n=m
$$
计算$f$和$e_n$的内积
$$
(f,e_n) = \frac 1 {2\pi}\int_0^{2\pi }f(\theta) e^{-in\theta} d\theta  = a_n
$$
显然
$$
S_N (f) = \sum_{|n|\leq N} a_n e_n = \sum_{|n|\leq N} (f,e_n) e_n
$$
对于$f - S_N(f)$有如下结论
$$
(f - \sum_{|n|\leq N} a_n e_n)\perp \sum_{|n|\leq N} b_n e_n
$$

> 这是因为
> $$
> \begin{aligned}
> (f - S_N(f),S_N(f))& = \sum_{|n|\leq N} b_n (f,e_n) - \sum_{|n|\leq N} a_n b_n \\
> & =\sum_{|n|\leq N} a_n b_n  - \sum_{|n|\leq N} a_n b_n = 0
> \end{aligned}
> $$
> 得证

进一步，有
$$
f = f-\sum_{|n|\leq N} a_n e_n  + \sum_{|n|\leq N}a_n e_n\\
\parallel f\parallel^2  = \parallel f - \sum_{|n|\leq N} a_n e_n \parallel^2  + \parallel \sum_{|n|\leq N} a_n e_n\parallel^2\\
\parallel \sum_{|n|\leq N} a_n e_n \parallel ^2 = \sum_{|n|\leq N} |a_n|^2
$$
接下来我们证明$f - S_N(f)$是最好的三角多项式，使得$\parallel f - \sum_{|n|\leq N} c_n e_n\parallel$最小

$$
\parallel f - S_N(f)\parallel \leq \parallel f - \sum_{|n|\leq N} c_n e_n \parallel
$$
还是借助正交证明

通过证明三角多项式的一致收敛定理，我们得到在circle上的路i案续汉书存在三角多项式$P(x)$在整个circle上对其一致收敛，即
$$
\forall \epsilon > 0,|f(x) - P(x) |<\epsilon,\forall -\pi \leq x\leq \pi
$$
这个P(x)可以是Fejer kernel和f的卷积，根据最小逼近原理可得
$$
\parallel f - S_N(f)\parallel \leq \parallel f(x)-P(x)\parallel \leq \epsilon
$$
随后我们将条件放松到仅仅是可积函数f的情况，之前我们提到过一个函数近似的引理

> 对于定义在circle上的可积函数f，并且f被B bound住，此时存在函数列$\{f_k\}_{k=1}^\infty$，满足
> $$
> \sup_{x\in [-\pi,\pi]} |f_k(x)|\leq B
> $$
> 此时存在极限
> $$
> \int_{-\pi}^\pi |f(x) - f_k(x) | dx \to 0,k\to \infty
> $$

这种情况下，可以选择同样被B bound住的连续函数g，满足
$$
\int_0 ^{2\pi} |f(\theta) - g(\theta)| d\theta \leq \epsilon
$$
这种情况在用三角多项式P逼近f即可，得到
$$
\parallel f- P\parallel \leq C \epsilon
$$
再借助best estimation逼近LHS

### Parseval Identity

对于可积函数f，则有Parseval Identity
$$
\sum_{n= -\infty}^\infty |a_n|^2 = \parallel f\parallel^2 = \frac 1 {2\pi} \int_0^{2\pi} f(\theta)^2  d\theta
$$

> 对于任何在circle上正交的函数族$\{e_n\}$，定义$a_n = (f,e_n)$，则我们有(Bessel’s inequality)
> $$
> \sum_{n = -\infty}^{\infty}|a_n|^2 \leq \parallel f\parallel^2
> $$
> 等号成立当且仅当$\sum a_n e_n \to f$

> 可以借助一组正交基将函数f投影到无限维空间$\mathcal l^2(\Z)$中，即构建
> $$
> f \to (\cdots,a_{-2},a_{-1},a_0,a_1,a_2,\cdots)
> $$
> 的映射，这个空间加上约束条件$\sum_n |a_n|^2 <\infty$相比可积函数空间$\mathcal R$的好处是后者是完备的，这和$\mathcal R$不完备矛盾(因为必然被$\parallel f\parallel^2$ bound住)，这种情况下必然存在一种情况
>
> > 对于$l^2(\Z)$空间上的一个sequence  $a_n,n\in \Z$，必然存在一种情况，即不存在一个$\mathcal R$上的函数f使得每个对应的Fourier Coefficient和$a_n$相对应

由Coefficient的收敛我们得到

> 对于circle上的可积函数f，必然有$\hat f(n)\to 0$ as $n\to \infty$

进一步我们有，对于两个可积函数和对应的Fourier Series
$$
F\quad \sum a_n e^{in \theta}\\
G\quad \sum b_n e^{in\theta}
$$
利用正交性，得到
$$
(F,G) = \sum a_n \overline b_n
$$

## Pointwise Convergence

mean-square并不能推出point-wise convergence，反之亦然，之前我们证明了存在三角多项式获得circle上的universal近似，这是建立在函数在整个circle上连续的条件上的。本节只希望讨论函数在局部$\theta_0$的性质，先给出一个local的结论（本节并不要求全局连续）

### Local  Result

给定在circle上可积的函数f，在$\theta_0$处可微，则
$$
S_N(f) (\theta_0) \to f(\theta_0)
$$
这表明傅里叶级数在某一点处的收敛性仅仅取决于函数在这一点附近的性质

### Diverging Fourier Series

> 希望构造一个连续函数，使得其傅里叶级数$S_N(\theta) = \sum_{n=-N}^N a_n e^{in\theta}$在某一点$\theta_0$处发散

对于偶锯齿函数$f(\theta) = i(\pi -\theta),0<\theta <\pi$，我们有
$$
S_N(f)= \sum_{n\neq 0,n=-N}^N \frac{e^{in\theta}}{n}
$$
分割为两个部分（破坏对称性）
$$
\overline f_N(\theta) = \sum_{n=-N}^{-1} \frac{e^{in\theta}}{n},f_N(\theta ) = \sum_{1\leq |n|\leq N}\frac{e^{in\theta}}{n}
$$
对应$\overline f_N(\theta)$不是一个可积函数的傅里叶级数

> 证明，假设$\overline  f_N(\theta)$是一个函数$\overline f$的傅里叶系数，对应的Abel Sum写成
> $$
> |A_r(\overline f)(\theta) | = |\sum_{n=-\infty} ^{-1} r^{|n|} \frac{e^{in\theta}}{n}|
> $$
> 带入0，得到
> $$
> |A_r(\overline f(0))| = \sum_{n=1}^\infty\frac{r^n}{n} 
> $$
> 后者趋向于无穷$r \to 1^{-1}$，但是写成卷积形式
> $$
> |A_r(\overline f)(0)| \leq \frac 1 {2\pi}\int_{-\pi}^\pi |\overline f(\theta)|P_r(\theta)d\theta\leq\frac 1 {2\pi} \max_\theta |\overline f(\theta)| \int_{-\pi}^\pi P_r(\theta) d\theta
> $$
> 显然被bound住

显然有

1. $|\overline f_N(0)| \geq c\log N$(积分面积)
2. $f_N(\theta)$ is uniformly bounded in N and $\theta$（一致有界）

下面证明一个引理，有关Abel means的性质(Abel Sum被bound，并且系数满足某种特定递减的性质，则r=1时收敛)

> 这种Abel means只考虑$n\geq 0$的情况

> 对于序列$\sum_{n=1}^\infty c_n$的Abel means $A_r  =\sum_{n=1}^\infty r^n c_n,r\in (0,1)$，假定在$r\to 1$时$A_r$被bound住，如果$c_n = O(\frac 1 n)$，则$S_N = \sum_{n=1}^N c_n$被bound

将这个结论应用于序列
$$
\sum_{n\neq 0}\frac {e^{in\theta}}n,c_n = \frac{e^{in\theta}}{n} + \frac{e^{-in\theta}}{-n}
$$
并且$|A_r(f)(\theta)| \leq \frac 1 {2\pi} \int_{-\pi}^\pi f(\theta -\phi)P_r(\phi) d\phi \leq \sup_\theta f$

得到$f_N(\theta)$被N和$\theta$ bound住

进一步定义$P_N,\overline P_N$
$$
P_N(\theta) = e^{i2N\theta}f_N(\theta) = e^{i2N\theta}\sum_{1\leq |n|\leq N} \frac{e^{in\theta}}{n}，[N,2N-1]\bigcup[2N+1,3N] \\
\overline P_N(\theta) = e^{i2N\theta}\overline f_N(\theta)=e^{i2N\theta}\sum_{-N\leq n\leq -1}\frac{e^{in\theta}}{n},[N,2N-1] \\
\overline f_N(\theta) = \sum_{n=-N}^{-1} \frac{e^{in\theta}}{n},f_N(\theta ) = \sum_{1\leq |n|\leq N}\frac{e^{in\theta}}{n}
$$
$f_N,\overline f_N$在N=0时没有定义，因此$P_N$在2N处三角多项式为空，对于这样的三角多项式定义其部分和
$$
S_M(P_N)
$$

> TBD
