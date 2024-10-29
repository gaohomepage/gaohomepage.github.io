---
title: Fourier Analysis(Basic Properties of Fourier Series)
date: 2024-03-07 15:27:33
tags:
    - Fourier Analysis
    - math
---
# Fourier Analysis(Chapter 2: Basic Properties of Fourier Series)

## Definition

### Fourier Coefficient

形如
$$
a_n =\frac 1 L \int_0^L f(x) e^{-2\pi i n x/L } dx
$$
的系数称为傅里叶系数，这是因为
$$
f(x) = \sum_{n=-\infty}^\infty a_n e^{2\pi i n x/L} dx
$$
对于$\forall m\neq n\in N$，有
$$
\int_0^L e^{\frac{2\pi i x(m - n)}{L}} dx  = \int_0^L \cos \frac{2\pi x(m-n)}{L} dL  + i\int _0^L \sin \frac{2\pi x(m-n)}{L} dx = 0
$$
这里$f:\R\to \C$，f需要在$[0,L]$中黎曼可积

更一般地，一个函数的傅里叶级数写成
$$
\sum_{n=-\infty}^\infty \hat f(n) e^{2\pi n i x/L},\hat f( n) =\frac 1 L \int_a^b f(x)e ^{-2\pi i x/L} dx
$$

#### Functions in a circle

周期为$2\pi$的函数$f(x)$，满足
$$
\int_a^b f(x) dx =\int_{a+2\pi}^{b+2\pi} f(x) dx  = 0
$$
傅里叶系数选择无需考虑区间选择

$\sum_{-\infty}^\infty c_n e^{2\pi i n x/L}$被定义为三角级数

### 傅里叶级数的收敛性

讨论partial sum
$$
S_N =\sum_{n=-\infty}^N c_n e^{i2\pi n x/L}
$$
考虑Dirichlet kernel
$$
D_N (x) = \sum_{n=-N}^N e^{inx}
$$
拆分为
$$
\sum_{n=0}^N w^n+\sum_{n=-N}^{-1} w^{-n} = \frac{1 - w^{N+1}}{1 - w}+ \frac{w^{-N} - 1}{1 - w}\\
=\frac{\sin(N+\frac 1 2)x}{\sin \frac x 2}
\\w=e^{ix}
$$
对于Poisson kernel
$$
P_r(\theta) =\sum_{n=-\infty}^{\infty} r^{|n|} e^{in \theta},\theta\in [-\pi,\pi]
$$
对于$\forall r\in [0,1]$一致收敛，这是因为其可以拆分为
$$
\sum_{-\infty}^{-1} r^{-n} e^{in\theta} +\sum_{i=0}^\infty r^n e^{in\theta}=\lim_{N\to \infty}\sum_{-N}^{-1} (\frac{e^{i\theta}}{r})^n+\lim_{N\to \infty} \sum_{0}^N{(e^{i\theta}r)}^n=\frac{1-|w|^2}{|1-w|^2},w = re^{i\theta}
$$
得到
$$
P_r(\theta) =\frac{1 - r^2}{(1 - r\cos\theta)^2  + (r\sin\theta)^2} = \frac{1 -r^2}{1 + r^2 - 2r\cos\theta}
$$


> 一致收敛的函数级数可以进行换序，写成
> $$
> \int \sum_i f_i = \sum_i \int f_i
> $$

> 一致收敛和逐点收敛的区别在于，前者先确定$\epsilon$和对应的$N(\epsilon)$，对于$\forall n > N(\epsilon)$和**所有在定义域中的x**都有$|f_n (x) - f(x)|<\epsilon$。后者要求对$\forall x$都有$\lim_{n\to \infty} f_n(x) = f(x)$，不同x对应的$\epsilon$是不同的，一致收敛又被记作
> $$
> \lim_{n\to \infty}\sup \{|f_n(x) - f(x)|,x\in I\} = 0
> $$

### Uniqueness of Fourier Series

两个函数$f(x),g(x)$傅里叶家属相同$\hat f(n )= \hat g(n)$是否一定能够得到$f(x)\neq g(x)$

> 不一定，在某一点不连续并不会影响积分值

给定以下条件，满足

> 假设$f$是一个在$2\pi$上可积的函数，并且$\hat f(n) = 0$，如果$f$在$\theta_0$处连续，则有
> $$
> f(\theta_0) = 0
> $$
> 证明，根据傅里叶系数得到
> $$
> \hat f(n) = 0\Rightarrow \int_{-\pi}^\pi f(\theta) e^{-in \theta} d\theta = 0
> $$
> 得到其线性组合（系数为$c_n$）也是0
> $$
> \int_{-\pi}^\pi f(\theta)\sum_n c_n e^{-in \theta} d\theta = 0
> $$
> 记求和号内的三角多项式为$P_k(\theta)$，得到
> $$
> \int_{-\pi}^\pi f(\theta)P_k (\theta) d\theta  = 0,P_k(\theta) =\sum_{n=-\infty}^k c_n e^{-in\theta}
> $$
> 不妨设$\theta_0= 0,f(0)>0$，由连续的
> $$
> \exists \delta,s.t \forall |x|\leq \delta,f(x) >\frac{f(0)}{2}
> $$
> 令
> $$
> p(\theta) = \epsilon + \cos \theta
> $$
> 选择一个足够小的$\epsilon >0$，使得，考虑$\delta \leq \theta\leq \pi$这个区间，显然有
> $$
> |p(\theta)|\leq 1 - \frac\epsilon 2
> $$
> 另外必然$\exist \eta > 0,\eta < \delta$，满足
> $$
> p(\theta) \geq 1 + \frac \epsilon 2 ,\forall |\theta|<\eta
> $$
> 构造一个级数函数
> $$
> p_k(\theta) = [p(\theta)^k]= (\epsilon +\cos\theta)^k
> $$
> 这里$\epsilon$不是无穷小，因此$\lim_{k\to \infty} p_k(0) = \infty$
>
> 傅里叶系数为0得到
> $$
> \hat f(n) = 0\Leftrightarrow \int_{-\pi}^{\pi}f(\theta)\cos\theta d \theta = 0,\int_{-\pi}^\pi f(\theta) \sin\theta d \theta = 0 
> $$
> 因此有
> $$
> \int_{-\pi}^\pi f(\theta) p_k(\theta) d\theta = 0\label{int}
> $$
>
> > 这是因为$p_k(\theta)$本身可以拆分为三角级数的线性组合
>
> 这里对$\ref{int}$进行估计，首先根据连续得到函数有界，即
> $$
> |f(\theta)|\leq B,\forall B\in [-\pi,\pi]
> $$
> 因此对于$|\theta| \geq \delta$，有如下bound
> $$
> |\int_{\delta\leq|\theta|} f(\theta)p_k(\theta) d\theta|\leq 2\pi B (1- \frac \epsilon 2)^k(对f(\theta)和p_k(\theta)进行放缩),\forall k
> $$
> 对$|\theta|\leq \delta $进行估计，有
> $$
> \int_{\eta\leq |\theta|< \delta} f(\theta)p_k(\theta) d\theta \geq 0（均为非负）
> $$
> 同时
> $$
> \int_{|\theta|<\eta}f(\theta)p_k(\theta) d\theta \geq 2 \eta \frac{f(0)}{2}(1 +\frac \epsilon 2)^k
> $$
> 显然$(1 +\frac\epsilon 2)^k - (1 -\frac{\epsilon}{2})^k$在$k\to \infty$是趋向于$\infty$，得到和$\ref{int}$矛盾的结论

### 一致收敛性

对于定义在圆上的函数$f$，并且$\sum_{n =\infty}^{\infty} |\hat f(n)|<\infty$，此时它的傅里叶级数一致收敛到$f(\theta)$，即
$$
\lim_{N\to \infty}S_N(f) = f(\theta),S_N(f) = \sum_{n=-\infty}^N \hat f(n) e^{in\theta},\forall \theta \in [-\pi,\pi]
$$

> $|\sum_{n=-\infty}^\infty \hat f(n) e^{in\theta}| \leq \sum_{n=-\infty}^\infty |\hat f(n)|<\infty$，因此$S_N(f)$一致收敛，下面证明收敛到$f(\theta)$，定义$g(\theta)$
> $$
> g(\theta) = \lim_{N\to \infty}\sum_{n =-N}^N \hat f(n) e^{in\theta}
> $$
> 显然两个函数的傅里叶级数相等，得到$f = g$(一致收敛求和积分可以换顺序)

### Smoothness and Decay

考虑傅里叶系数的阶，对于定义在circle上的二阶可微函数$f$
$$
\hat f(n) = O(\frac {1}{|n|^2})\Rightarrow |\hat f(n)|\leq \frac{C}{|n|^2}
$$

> $n\neq 0$，有
> $$
> \begin{aligned}
> 2\pi \hat f(n)& = \int_0^{2\pi} f(\theta )e^{-in \theta }  d\theta\\
> &=\int_0^{2\pi} f(\theta) d {(-\frac{1}{in}e^{-in\theta})} \\
> &=-{\frac{f(\theta) e^{-in\theta}}{in}}|_0^{2\pi} - \int _0^{2\pi} f^\prime(\theta)\frac{1}{in} e^{-in\theta} d\theta\\
> &=-\frac{1}{in}\int_0^{2\pi} f^\prime (\theta) e^{-in \theta} d \theta \\
> &=\frac{1}{(in)^2} \int_0^{2\pi} f^{\prime\prime}(\theta) e^{-in \theta} d\theta
> \end{aligned}
> $$
> 再用二阶微分上界bound积分

$f(\theta)$和$f^\prime(\theta)$的傅里叶系数满足
$$
\hat f(n) = -\frac{1}{in}\hat f^\prime(n)
$$
同理推导到$C^k$，因此得到

再circle上的函数只要是k阶可微的$k\geq 2$，则其傅里叶级数一致收敛

## 卷积

显然，根据积分的可加性，有
$$
\hat f(n) + \hat g(n) = \hat{f+g}(n)
$$
考虑两个函数的乘积
$$
\hat{fg}(n) = \frac{1}{2\pi}\int_{-\pi}^{\pi} f(\theta)g(\theta) e^{-in\theta} d\theta
$$
定义$f/g$的卷积
$$
f*g(x) = \frac{1}{2\pi}\int_{-\pi}^\pi f(y)g(x-y) dy=\frac{1}{2\pi}\int_{x-\pi}^{x+\pi} f(x-z) g(z) d z(z=  x-y)
$$
$f(x- z) g(z)$也是关于$2\pi$的函数，因此
$$
f* g(x) =\frac{1}{2\pi} \int_{-\pi}^{\pi} f(x- y) g(y)   dy= g*f(x)
$$
卷积满足可交换性（对于周期函数），给定$g= 1$，得到
$$
f*g(x) = \frac{1}{2\pi}\int_{-\pi}^\pi f(y)d y
$$
傅里叶级数的partial sum满足
$$
\begin{aligned}
S_N(f)(x)&=\sum_{n=-N}^N \frac{1}{2\pi}\int_{-\pi}^\pi f(y) e^{-iny} dy  e^{inx}\\
&=\frac{1}{2\pi}\int_{-\pi}^\pi f(y)\sum_{n=-N}^N e^{in(x-y) } dy\\
&=\frac{1}{2\pi}\int_{-\pi}^\pi f(y) g_N(x-y) dy,g_N(z) = \sum_{n=-N}^N e^{inz}\\
&=f * g_N
\end{aligned}
$$

卷积的一个很好的性质是
$$
\hat{f * g}(n) = \hat {f(n)} \hat {g(n)}
$$
卷积的傅里叶级数等于两个函数对应的傅里叶级数乘积，证明如下
$$
\begin{aligned}
\hat {f * g}(x) &=\frac{1}{2\pi}\int_{-\pi}^\pi f(x-y) g(y) d y\\ 
LHS &= (\frac{1}{2\pi})^2 \int_{-\pi}^{\pi}\int _{-\pi}^{\pi} f(x-y) g(y) dy  e^{-inx} dx \\
&=(\frac{1}{2\pi})^2\int_{\Omega = (x,y)\in (-\pi,\pi)^2}f(x-y) e^{-in(x-y)} g(y) e^{-iny} d x dy \\
&{=}(\frac{1}{2\pi})^2\int _{\Eta}f(u) e^{-inu} g(v)e^{-inv} d u dv,H对应二维空间上的菱形区域,u = x-y,v = y\\
&=(\frac{1}{2\pi})^2\int_{-\pi}^\pi g(v )e ^{-inv} \int_{-\pi -v}^{\pi -v} f(u)e ^{-in u} d u  dv 
\end{aligned}
$$
再根据周期函数定积分性质易得

接下来我们将会证明，如果$f/g$是连续的则它们的卷积是连续的，即估算
$$
\forall x_1,x_2,|f*g (x_1)  - f* g(x_2)|\label{abs_cov},|x_1-x_2|\leq \delta
$$
根据卷积交换性，$\ref{abs_cov}$写成
$$
\begin{aligned}
\frac{1}{2\pi}|\int_{-\pi}^\pi  f(y)  [g(x_1- y)- g(x_2-y) dy]|&\leq\frac{\epsilon}{2\pi} \int_{-\pi}^\pi |f(y)| dy,g(x_1-y)-g(x_2-y)\leq \epsilon\\
&\leq \epsilon B,f(y)被bound住
\end{aligned}
$$
 然而，卷积连续的连续的前提**仅仅要求$f/g$可积**，并不要求连续或者一阶可导之类更强的条件，证明此结论需要借助下列引理

> $f$说可积函数并且被B bound住，则存在连续函数序列$\{f_k\}_{k=1}^\infty$满足
> $$
> \sup_{x\in [-\pi,\pi]} |f_k(x)|\leq B
> $$
> 并且
> $$
> \int _{-\pi}^\pi |f(x) - f_k(x)|dx \to 0
> $$
> 这提供了一种用函数列积分估算可微周期函数积分的方式

此时可以构造$\{f_k\},\{g_k\}$两个函数列在黎曼积分上逼近$f,g$，此时
$$
f* g - f_k * g_k = (f-f_k) g + f_k* (g - g_k),(f_k * g = g * f_k)
$$
这两部分都可以被容易地bound住
$$
|(f - f_k)  * g| = \frac{1}{2\pi} \int_{-\pi}^\pi |f(x-y) - f_k (x- y)||g(y)| dy\\
\leq \frac{1}{2\pi}\sup_y |g(y)| \int_{-\pi}^\pi |f(y) - f_k (y)| dy \to 0
$$
考虑到$f_k/g_k$是连续的，结论可证

> 利用另一个序列逼近要证明函数，并且另一个序列连续



