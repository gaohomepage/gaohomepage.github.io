---
title: sensitive-analysis
date: 2023-04-09 18:45:39
tags:
---
#! https://zhuanlan.zhihu.com/p/599611644
# Duality(KKT example)

## 敏感性分析

实际问题的数学模型(凸优化)
$$
\min f_0(x)\\
s.t\ f_i(x)\leq 0\\
h_i(x)=0
$$

1. 模型是否准确
2. 对模型微调的影响(约束改成$f_i(x)\leq \delta_i$)

约束写成
$$
s.t\quad f_i(x)\leq u_i,i=1,2,\cdots,m\\
h_i(x) = w_i,i=1,2,\cdots,p
$$
若$u_i\geq 0$，则约束放松，定义这个问题是干扰问题

1. 对最优值的影响
2. 对最优解的影响

定义干扰问题的最优解是$p^*(u,w)$，显然
$$
p^*(0,0) = p^*
$$

### 性质：若原问题为凸，则干扰问题最优解$p^*(u,w)$是$(u,w)$的凸函数

$$
\begin{aligned}
p^*(u,w) &= \inf_x\{f_0(x)|f_i(x)\leq u_i,h_j(x)=w_j \}\\
&=\inf_x g(x,u,w)
\end{aligned}\\
g(x,u,w) = f_0(x),dom g = dom f_0\bigcap D,D = \{x|f_i(x)\leq u_i,h_j(x)\leq w_j \}
$$

定义$g(x,u,w)$，实际上是在一个可行解集合中最小值，这里$g$的定义域包含$(x,u,w)$

下面证明$g(x,u,w)$是关于$(x,u,w)$的凸函数

#### $dom g$定义在凸集上

$dom f_0$是凸集，$(u,w)$定义在$\R^{m+p}$的全集上，同时对于不等式约束
$$
f_i(x) - u_i \leq 0
$$
显然关于$(x,u_i)$是凸函数(左侧是凸函数+仿射函数)

同理$h_i(x) - w_i =0$也是关于$(x,w_i)$的凸集，因此D是凸集，同时$f_0(x)$是凸函数，Q.E.D

因此函数簇的极小
$$
p^*(u,v) = \inf_x  g(x,u,v)
$$
也是凸函数

### 性质：原问题为凸，(原问题和干扰问题)对偶间隙为0，$\lambda^*,v^*$为原问题对偶最优解，一定有

$$
p^*(u,w) \geq p^* (0,0) - {\lambda^*}^T u-{v^*}^T w
$$

> 设$\hat x$为干扰问题最优解，一定是干扰问题可行解，满足约束
> $$
> f_i(\hat x)\leq u_i\\
> h_i(\hat x)=w_i
> $$
> 对偶间隙为0，满足
> $$
> p^*(0,0) = g(\lambda^*,v^*)\leq f_0(\hat x) +\sum_i \lambda_i^* f_i(\hat x)+\sum_i v_i^* h_i(\hat x)
> $$
> 根据可行解的性质
> $$
> g(\lambda^*,v^*) \leq f_0(\hat x) + {\lambda^*}^T u +{v^*}^T w
> $$
> 对偶间隙为0，可知
> $$
> f_0(\hat x) = p^*(u,w)
> $$
> 因此
> $$
> p^*(0,0) \leq p^*(u,w) + {\lambda^*}^T u +{v^*}^T w
> $$
> Q.E.D

因此得到干扰问题最优解的下界
$$
p^*(u,w) \geq p^*(0,0) - {\lambda^*}^T u - {v^*}^T w
$$
注意这只是最优解的**下界**

#### 推论

1. $\lambda_i^*$很大，且加紧第i项不等式约束  $u_i<0\to p^*(u,w)$会急剧增大(第i项不等式导致解不鲁棒)影子价格较大的材料
2. $v^*_i>0$很大且$w_i<0$/$v_i^*<0$很小且$w_i>0$ 第i项等式约束导致最优值不鲁棒
3. $\lambda^*_i$很小且$u_i>0$，则提高$u_i$导致下界改变不多
4. $v_i^*>0$很小且$w_i>0$/$v_i^*<0$且绝对值很小且$w_i<0$吗，则等式约束对最优值鲁棒

注意不等式仅仅提供下界，下界增大的情况下容易分析，下界降低则不能得到任何结论

> 如何得到干扰问题最优解的intense bound?

### 性质(局部敏感性)：若原问题为凸问题，对偶间隙为0，并且$p^*(u,w)$在(0,0)处可微，得到

$$
\lambda_i^* =-\frac{\partial p^*(0,0)}{\partial u_i}\\
v_i^* = -\frac{\partial p^*(0,0)}{\partial w_i}
$$

注意这里实际上有两对变量(分别对应对偶变量和约束的扰动)

1. $\lambda_i$和$u_i$和不等式约束有关
2. $v_i$和$w_i$和等式约束有关

> 对$p^*(u,w)$在0，0处做泰勒展开
> $$
> p^*(u,w) = p^*(0,0)+\frac{\partial p^*(u,w)}{\partial u}|_{0,0}^T u +\frac{\partial p^*(u,w)}{\partial w}|_{0,0}^T w+o(\parallel u\parallel+\parallel w\parallel)
> $$

这样在一个局部可以得到$p^*(u,w)$的近似

## Boolen LP问题

原问题(P)
$$
\min c^T x\\
s.t\quad Ax\leq b\\
x_i\in\{0,1\},i=1,2,\cdots,n
$$
显然非凸问题，给出松弛(Q)
$$
\min c^T x\\
Ax\leq b\\
0\leq x_i \leq 1
$$
两个问题的最优值显然有
$$
p^*\geq q^*
$$

> 某个$x_i$很接近0或者1，将其松弛为0-1，求解n-1维boolen LP

### Boolen  LP等价问题

$$
\min c^T x\\
s.t\ Ax \leq b\\
x_i(x_i - 1) = 0,i=1,2,\cdots,n
$$

等式约束不是仿射，因此不是凸问题，写出Lagrange函数
$$
L(x,\lambda,v) = (c+ A \lambda^T -v)^T x+\sum_{i=1}^n v_i x_i^2-\lambda^T b\\
g(\lambda,v) =\inf_x L(x,\lambda,v) 
$$

1. $\exist v_i<0,g(\lambda,v) =-\infty$
2. $\forall v_i >0,g(\lambda,v) =-\lambda^T b -\frac{1}{4}\sum_{i=1}^n\frac {(c_i + a_i^T \lambda - u_i)^2}{v_i}$

记
$$
A = \begin{pmatrix}
a_1\\
a_2\\
\cdots\\
a_n
\end{pmatrix}
$$
对偶问题
$$
\max -\lambda^T b -\frac{1}{4}\sum_{i=1}^n\frac {(c_i + a_i^T \lambda - v_i)^2}{v_i}\\
s.t\quad \lambda\geq 0,v\geq 0
$$
这里用到一条性质(为了求解$\max_{\lambda,v}g(\lambda,v)$)
$$
\max_{\lambda,v} f(\lambda,v) = \max_\lambda\max_vf(\lambda,v)
$$
优化目标写成
$$
\max_\lambda-\lambda^T b+\frac{1}{4} \max_v \sum_{i=1}^n -\frac{(c_i+a_i^T \lambda-v_i)^2}{v_i} 
$$
对v的优化等价为
$$
\max_{v_i} -(v_i -2(c_i+a_i^T \lambda)+\frac{(c_i+a_i^T \lambda)^2}{v_i})
$$
微分
$$
v_i ^2 = (c_i+a_i^T \lambda)^2
$$

$$
-(1 -2\frac{(c_i+a_i^T \lambda)^2}{v_i^2}) = 0\Rightarrow v_i =\frac{1}{\sqrt{2} (c_i+a_i^T \lambda)}
$$

最大值是
$$
-(\frac{1}{\sqrt 2(c_i+a_i^T \lambda)} -2(c_i+a_i^T\lambda)+1)
$$
得到
$$
\max_{v_i\geq 0}-\frac{(c_i+a_i^T\lambda -c_i)^2}{v_i}=\left\{
\begin{aligned}
& c_i+a_i^T \lambda,c_i+a_i^T\lambda\leq 0\\
&0,c_i+a_i^T \lambda>0
\end{aligned}
\right. =\min (c_i+a_i^T \lambda,0)
$$
写成
$$
\max_\lambda -\lambda^T b +\sum_{i=1}^n \min (0,c_i+a_i^T \lambda) 
$$
记
$$
w_i = \min (0,c_i+a_i^T \lambda)
$$
等价问题是
$$
\max_\lambda -\lambda^T b + 1^T w\\
s.t\quad \lambda\geq 0,w_i \leq 0,w_i\leq c_i+a_i^T \lambda
$$
$\max$将会保证$w_i$取到$0,c_i+a_i^T \lambda$中较小的那一个而不是更小

变成线性规划，这是LP bool的对偶问题的等价问题，现在求解对偶问题松弛问题的对偶问题，松弛问题有三个约束
$$
Ax = b\\
x_i \leq 1\\
x_i \geq 0
$$
因此需要引入三组对偶变量$u(Ax =b),w(x_i\leq 1),v(-x_i\leq 0)$
$$
\begin{aligned}
L(x,u,v,w) &= c^T x+u^T (Ax -b)-v^T x+w^T(x-1)\\
(c+A^T u -v+w)^T x -b^T u -1^TW
\end{aligned}\\
\max -b^Tu -1^T w\\
s.t\quad c+A^T u -v+w=0,u\geq 0,v\geq 0,w\geq 0
$$
改写约束，等价为(消除$v\geq 0$)
$$
c+A^tu +w\geq 0,u\geq 0,w\geq 0
$$
对比两个问题

|          | Boolen LP对偶问题                                    | Boolen LP松弛形式的对偶问题         |
| -------- | ---------------------------------------------------- | ----------------------------------- |
| 目标函数 | $\max_\lambda -\lambda^T b + 1^T w$                  | $\max_u -b^T u -1^T w$              |
| 约束     | $\lambda\geq 0,w_i\leq 0,w_i \leq c_i+a_i^T \lambda$ | $c+A^T u +w \geq 0,u\geq 0,w\geq 0$ |

> 两个问题的对偶问题相同，它的原问题不一定相同。。

这里Boolen LP等价问题非凸，导致一系列这个结果，实际上

> Boolen LP等价问题的对偶问题是原问题的松弛，松弛效果等价于离散空间松弛到区间，这个对偶称为拉格朗日松弛

> 对偶问题只是给定了原问题最优解的下界，原问题可能没有这么优

对偶问题必然是等价问题的松弛，当对偶间隙不为0时，必然不能通过解对偶问题得到等价问题的解。另一个重要的结论是

> LP松弛和拉格朗日松弛互为对偶


## 带等式约束的可微凸优化问题

$$
\min  f_0(x)\\
s.t\quad Ax = b
$$

选择$\alpha$，修改为无约束的约束问题，增加目标的**惩罚**，显然$\alpha =\infty$等价为原问题
$$
\min f_\alpha(x)= f_0(x) +\frac{\alpha}{2}\parallel Ax -b \parallel
$$
选择$\alpha$并不断增大，得到一组解
$$
\alpha_i,x_i\\
x_i =\arg\min_x f_0(x) +\frac{\alpha}{2}\parallel Ax -b \parallel_2^2
$$
假设求出无约束优化的最优解$\hat x(\alpha)$，讨论它和原始问题最优解的关系，$\hat x(\alpha)$显然是让$f_0(x)+\frac{\alpha}{2}\parallel Ax -b\parallel$一阶偏导为0
$$
\frac{\partial f_\alpha(x)}{\partial x}|_{x= \hat x(\alpha)} =0\\
\nabla f_0(\hat x(\alpha))+\alpha A^T (A\hat x(\alpha) - b) = 0
$$
这个问题(无约束带惩罚项优化问题)和下列问题等价
$$
\hat x(\alpha) = \arg\min_x f_0(x)+\alpha (A\hat x(\alpha) -b)^T (Ax -b)
$$
因为(37)微分和(36)相同，注意$x\to \hat x(\alpha)$

现在考虑原问题和对偶问题
$$
L(x,v) = f_0(x)+v^T (Ax - b)\\
g(v) = \inf_x L(x,v)\\
\frac{\partial L(x,v)}{\partial x} = \nabla f_0(x) +A^T v = 0
$$

令$v=\alpha (A\hat x(\alpha)-b)$，此时
$$
\inf_x L(x,\alpha (A\hat x(\alpha) -b))
$$
等价于问题(37)，已知原问题和
$$
\max g(v)
$$
对偶

> 惩罚函数方法得到的最优解$\hat x(\alpha)$等价于$v = \alpha(A\hat x(\alpha ) -b)$时对拉格朗日函数优化

何时满足
$$
v =\arg\max g(v)
$$

> 这个问题对偶间距为0，最优满足$\alpha \to \infty$并且$A\hat x(\alpha) -b \to 0$，对偶间隙为0只需要满足$Ax =b$有解存在

考虑
$$
\begin{aligned}
g(\alpha(A\hat x(\alpha) - b))&=\inf_x \{f_0(x)+\alpha (A\hat x(\alpha) -b)^T (Ax-b) \}\\
&=f_0(\hat x(\alpha)) + \alpha \parallel A\hat x(\alpha) - b\parallel_2^2
\end{aligned}
$$

因此$g(v = \alpha(A\hat x(\alpha) -b))$实际上是惩罚后的目标函数在$\hat x(\alpha)$处的取值

并且根据对偶间隙为0得到
$$
p^*(原问题)\geq d^*(对偶问题) \geq g(\alpha (A\hat x(\alpha) -b)) = f_0(\hat x(\alpha)) + \alpha \parallel A\hat x(\alpha) -b \parallel_2^2\geq f_0(\hat x(\alpha))
$$
因此有
$$
f_0(x^*) =p^* \geq f_0(\hat x(\alpha))
$$
使用惩罚函数得到的目标函数必然比最优值小(比最优还要优)。本质上是因为存在对偶问题

$\alpha = 0$则求解的是无约束的
$$
\arg\min f_0(x)
$$
这是原问题的很大松弛，增加惩罚项也是问题的松弛

$\alpha \to \infty$，$p^*$仍然有限，并且$p^*\geq d^*$，$d^*$被bound住，必然找到最优解$A\hat x(\alpha ) = b$

> 有约束的优化问题可以松弛为无约束的优化问题

$Ax = b$约束解在超平面上，惩罚函数希望解尽量不要偏移超平面

> 本节讨论了对于仅包含等式约束的一类优化问题的求解，基本思路是在目标函数中添加一项使得它变成无约束优化的凸问题，并且从对偶问题角度讨论了惩罚因子和对偶变量的关系

## 带线性不等式的可微凸优化问题

$$
\min f_0(x) \\
s.t\quad Ax\geq b,x\in \R^n, A\in R^{m\times n}, b\in  \R^m\\
A = \begin{pmatrix}
a_1\\
a_2\\
\cdots\\
a_m
\end{pmatrix}
$$

log-barrier方法，增添log惩罚项
$$
\min  f_0(x) -\sum_{i=1}^m u_i \log (a_i^T x- b_i),u_i\geq 0
$$
定义在
$$
a^T_i x -b_i >0
$$
同时希望$a_i^T x -b _i $尽量大于0，这是优化目标$\min$所保证的(尽量满足约束)

1. $u_i = 0$，问题和原问题相同，但是影响计算稳定性

从Primal Dual考虑log-barrier和原问题最优解的关系，假设$\hat x$是log-barrier最优解并且$u_i = u$
$$
\nabla f_0(\hat x) -\sum_{i=1}^m  u\frac{a_i}{a_i^T \hat x_i -b_i} = 0
$$
同时$\hat x$也是如下无约束优化问题的最优解(对$L(x,\lambda_i = \frac{u}{a_i^T\hat x-b_i})$x求微分结果相同)
$$
\hat x = \arg\min_x f_0(x) -\sum_{i=1}^m u \frac{a_i^T x- b_i}{a_i^T \hat x - b_i} = \arg\min_{x} f_{\hat x}(x)
$$
因为$f_{\hat x}(x)$的微分和log-barrier相同

> 并且满足$\lambda_i>0$，是有效的对偶变量

原问题的对偶问题
$$
L(x,\lambda) = f_0(x) +\sum_{i=1}^m \lambda_i (b_i -a_i^T x)\\
g(\lambda) =\inf_x L(x,\lambda)
$$
令
$$
\lambda_i = \hat \lambda_i=\frac{u}{a_i^T \hat x-b_i}
$$
则
$$
\arg\min_x L(x,\hat\lambda)= \hat x
$$
不等式约束需要保证对偶变量大于0，这里因为带惩罚项的无约束原问题必须要满足$a_i^T x -b_i>0$，则必然满足
$$
\lambda_i >0
$$

> log约束保证解严格落在约束空间的内点，实际上添加了更严格的约束(内点法)

1. 有约束的原问题，在此基础上添加惩罚
2. 惩罚因子不同对应最优解不同
3. 对偶函数中不同对偶变量和惩罚因子之间一一对应