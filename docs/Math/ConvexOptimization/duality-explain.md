---
title: duality explain
date: 2023-04-09 18:43:57
tags:
 - Convex Optimization
---
# Convex Optimization(Duality from different perspectives)

## 回顾——对偶问题

原问题具有最优解$p^*$
$$
\min f_0(x)\\
s.t\quad f_i(x)\leq 0,h_j(x) = 0
$$
Lagrange Function定义为
$$
L(x,\lambda,v) = f_0(x)+ \sum_{i=1}^m \lambda_i f_i(x) +\sum_{i=1}^p v_i h_i(x)
$$
Dual Function定义为
$$
g(\lambda,v) =\inf_x L(x,\lambda,v)
$$
Dual Problem定义为
$$
\max g(\lambda,v)\\
s.t\ \lambda\geq 0
$$
定义的域是$\R^{m+p}$，它的解记作$d^*$

对偶问题具有两个重要结论

1. 对偶问题是凸优化问题
2. $d^*\leq p^*$

对偶问题约束较为简单，关键问题是寻找强对偶条件$d^*=p^*$满足的条件（对偶间隙$d^*-p^*=0$）

### 定义——相对内部

原始优化问题域D的相对内部定义为
$$
Relint D = \{x\in D|B(x,r)\bigcap aff D\subseteq D,\exists r>0 \}
$$
对于$\R^3$中的一个二维圆，显然每个点都是它的边界，但是除了圆的边缘，其它点都是相对内部

### Slater条件(充分条件)

若有**凸问题**，并且等式约束是仿射
$$
\min f_0(x)\\
s.t f_i(x)\leq 0\\
Ax = b
$$
满足如下条件时，对偶间隙为0

> $\exists x\in relint D$，使得$f_i(x)<0$并且$Ax= b$，则$p^*=d^*$(严格满足不等式)

Weaker Slater Condition，若不等式约束也是仿射约束

> 可行域非空，则必然满足$p^*= d^*$

仿射约束在空间构成多面体，一定存在相对内点

> $cx+d\leq 0$表示一个半空间，$cx+d<0$表示除了超平面以外的半空间，考虑D的相对内部，实际上
> $$
> relint D = relint\{dom f_i\bigcap dom f_0 \}
> $$
> $relint D$实际上是$relint f_0 = relint\{dom f_0 \}$
>
> 1. $f_0$定义在全空间上，则一定能满足Slater条件(多面体内部和超平面相交)
> 2. $dom f_0$并非定义在全空间
>
> 因为假定可行解存在，一定能找到可行域D上的相对内点，和多面体内部相交

> 推论：线性规划(目标函数也是仿射)存在可行解，则对偶间隙为0

> 二次规划问题
> $$
> \min X^T X\\
> s.t\quad Ax = b
> $$
> 一旦存在可行解，则对偶间隙为0，它的对偶问题是
> $$
> L(x,v) = x^T x+ v^T(Ax -b)\\
> \frac{\partial L}{\partial x} = 2 x + A^T v =0\\
> x =-\frac{1}{2} A^T v\to g(v )= -\frac{1}{4} v^T AA^T v - v^Tb
> $$
> 是无约束二次规划
> $$
> \max -\frac{1}{4} v6T AA^T v- v^T b
> $$
> 的解相同

> QCQP，目标函数二次矩阵是正定的
> $$
> \min \frac{1}{2} x^T P_0 x +q_0^Tx +r _0\\
> s.t\quad \frac{1}{2}x^T P_i x+q_ix + r_i \leq 0
> $$
> Lagrange函数和对偶函数是
> $$
> L(x,\lambda) = \frac{1}{2}x^T (\sum_{i=0}^m\lambda_i  P_i)x +\sum_{i=0}^m\lambda_i q_i^T x+ \sum_{i=0}^m \lambda_i r_i\\
> \lambda_0 = 1
> $$
> $\lambda_i \geq 0$时，$L(x,\lambda)$是x的凸函数
> $$
> P(\lambda) = \sum_{i=0}^m \lambda_i P_i\\
> q(\lambda) = \sum_{i=0}^m \lambda_i q_i\\
> g(\lambda) = -\frac{1}{2}q^T(\lambda) P^{-1}(\lambda) q(\lambda)+ r(\lambda)
> $$
> $\lambda \geq 0$时，$g(\lambda)$是凹函数
> $$
> \max g(\lambda)\\
> s.t \quad \lambda \geq 0
> $$
> 验证Slater条件，如果$\exists x\in D=\R^n$，满足
> $$
> \frac{1}{2}x^T P_i x + q_i^T x+r_i <0,\forall i
> $$
> 则$p^* = d^*$
>
> > 若$ q_i = r_i=0$，则不存在x满足$x^T P_i x<0$，此时可行解只有$x= 0$，最优值为0，对偶问题最优值也是0(不满足Slater条件但是对偶间隙为0)

## 对偶间隙为0的解释

### 几何解释

仅包含不等式约束的优化
$$
\min f_0(x)\\
s.t \quad f_1(x)\leq 0
$$
定义
$$
G = \{(f_1(x),f_0(x)) \}
$$
最优值定义为
$$
p^* =\inf\{t|(u,t)\in G,u\leq 0\}
$$
对偶函数定义为
$$
g(\lambda) = \inf\{\lambda u + t |(u,t)\in G \}
$$
$g(\lambda)$首先是$\lambda$的函数，对域中所有点$(u,t)$取$\lambda u + t$最小值

几何解释：现在二维区域上绘制出集合G，$p^*$即为落在u轴负半轴中t的最小值

![image-20230102213447162](https://s2.loli.net/2023/01/02/bOt9vrysc6TKMWS.png)

现在讨论对于某个$\lambda \geq 0,g(\lambda)$的几何含义，显然
$$
\lambda u + t = c
$$
代表一条直线，斜率是$-\lambda$，它在t轴上的截距为c，$g(\lambda)$对应的直线满足两个条件

1. 与G至少有一个交点
2. 在t轴上的截距尽量小

![image-20230102214144316](https://s2.loli.net/2023/01/02/4jPncqVFGp2XCRQ.png)

对偶问题的目标是

1. 直线斜率为负
2. 不断平移直线，使得截距逐渐提升（对于同一个$\lambda$，应该获得最小截距）

这种情况下可以看出来弱对偶一定成立

### 鞍点Saddle Point

对于原始问题的拉格朗日函数$L(x,\lambda)$，这个函数的鞍点定义为
$$
\inf_{x\in D}\sup_{\lambda \geq 0}L(x,\lambda) =\sup_{\lambda\geq 0}\inf_{x\in D}L(x,\lambda)
$$
存在$(x^*,\lambda^*)$同时满足左右两侧

显然右侧对应$d^*$
$$
d^* = \sup_{\lambda \geq 0}\inf_{x\in D}L(x,\lambda)
$$
考虑左侧，对应的
$$
\inf_{x\in  D}\sup_{\lambda\geq 0} f_0(x)+\sum_i \lambda_i f_i(x)
$$
如果对于$x\in D,\forall i,f_i(x)<0$，则显然
$$
\sup_{\lambda \geq 0}f_0(x) +\sum_i \lambda_i f_i(x) = f_0(x)
$$
如果$\exists i,f_i(x)>0$，则
$$
\sup_{\lambda \geq 0}L(x,\lambda) =\infty
$$
考虑$\inf$，显然等价为
$$
\inf_{x\in D} f_0(x),f_i(x)\leq 0,\forall i =1,2,\cdots,m
$$
因此
$$
p^* = \inf_{x\in D}\sup_{\lambda \geq 0}L(x,\lambda)
$$
如果能找到一个点同时满足左右两侧，则一定可以保证对偶间距为0

> 一般的函数$f(w,z),w\in S_w,z\in S_z$，一定有
> $$
> \inf_w \sup_z f(w,z) \geq \sup_z \inf_w f(w,z)
> $$
> 何时等式成立并且对应的w,z相等？
> $$
> (w^*,z^*) =\arg\min_w \max_z f(w,z) =\arg\max_z \min_w f(w,z)
> $$
> 称为函数的鞍点，显然鞍点等价的定义是
> $$
> f(w,z^*)\geq f(w^*,z^*)\geq f(w^*,z),\forall w,z
> $$

对偶问题中引入鞍点，拉格朗日函数
$$
L(x,\lambda) = f_0(x) +\sum_i \lambda_i f_i(x)
$$
对$\lambda$求极大
$$
\sup_{\lambda \geq 0}L(x,\lambda) =f_0(x)\leq \infty ,\forall i,f_i(x)\leq 0
$$
否则$\sup_{\lambda\geq }L(x,\lambda)$为$\infty$

因此
$$
p^* =\min_x\{f_0(x)|f_i(x)\leq 0,\forall i \} =\inf_x \sup_{\lambda \geq 0}L(x,\lambda)
$$
对偶间距为0意味着
$$
\inf_x \sup_{\lambda\geq 0 }L(x,\lambda) =\sup_{\lambda \geq 0}\inf_x L(x,\lambda)
$$

> 课程中并没有说明这种情况下存在$(x^*,\lambda^*)$同时满足左右两侧

#### 鞍点定理

若$(x^*,\lambda^*)$为$L(x,\lambda)$的鞍点，等价于

1. 强对偶存在
2. 且$(x^*,\lambda^*)$为原始优化问题和对偶问题最优解

$\Rightarrow$显然(鞍点定义，强对偶存在)，并且
$$
(x^*,\lambda^*) = \arg\sup_{\lambda\geq 0}\inf_x L(x,\lambda)
$$
进一步写成
$$
\lambda^* =\arg\sup_{\lambda}\inf_x L(x,\lambda) = \arg d^*(对偶问题目标是求解\lambda)
$$
因此$\lambda^*$为对偶问题最优解

> 没太看懂这里想表达什么，为什么不直接通过达到上界证明对偶问题最优解是$(x^*,\lambda^*)$

$\Leftarrow $，最优解一定是可行解，因此有
$$
f_i(x^*)\leq 0,\forall \ i =1,2,\cdots,m\\
\lambda_i \geq 0
$$
对偶间隙为0意味着
$$
f_0(x^*) = g(\lambda^*) = \inf_x f_0(x)+\sum_i  \lambda_i^* f_i(x)\leq f_0(x^*)+\sum_u \lambda_i^* f_i(x^*)\leq f_0(x^*)
$$
最后一步源自$f_i(x)\leq 0,\lambda_i \geq 0$，所有的不等式都是等式，因此有
$$
\inf_x f_0(x)+\sum_i \lambda_i^* f_i(x) = f_0(x^*) + \sum_i \lambda_i^* f_i(x^*)\\
f_0(x^*)+\sum_i \lambda_i^* f_i(x^*) = f_0(x^*)
$$

上述第一个等式等价为
$$
\inf_x L(x,\lambda^*) = L(x^*,\lambda^*)
$$
考虑如下定义
$$
\sup_{\lambda \geq 0}L(x^*,\lambda)=\sup_{\lambda \geq 0}f_0(x^*)+\sum_{i=1}^m \lambda_i f_i(x^*) = f_0(x^*)(\forall i f_i(x^*)<0,最优解一定是可行解)
$$
 同时，根据(39)第二个等式可知
$$
f_0(x^*) = L(x^*,\lambda^*)
$$
因此得到
$$
\sup_{\lambda \geq 0}L(x^*,\lambda) =L(x^*,\lambda^*)\to \forall \lambda,L(x^*,\lambda)\leq L(x^*,\lambda^*)
$$
同时显然有（$(x^*,\lambda^*)$是对偶问题的解）
$$
\inf_{x}L(x,\lambda^*) = L(x^*,\lambda^*)\to \forall x,L(x,\lambda^*)\geq L(x^*,\lambda^*)
$$
这实际上是鞍点的第二个定义

### 多目标优化

$$
\min \{f_0(x),f_1(x),\cdots,f_m(x) \}
$$

可以通过非负加权$\lambda_i$得到一组帕累托最优解
$$
\min \sum_i \lambda_i f_i(x)
$$
这是对应的单目标优化问题的拉格朗日函数$L(x,\lambda)$，给定$\lambda$，极小化
$$
\hat x = \arg\min_{x\in D}L(x,\lambda)
$$
求出的极小值是$\lambda$函数$g(\lambda)$，在$\lambda\geq 0$上对g求极大，得到
$$
\hat \lambda = \arg\max_\lambda g(\lambda)
$$
带入$\hat \lambda$到$L(x,\lambda)$，并求解
$$
\hat{\hat x} = \arg\min_{x\in  D}L(x,\hat \lambda)
$$
得到的$\hat{\hat x}$实际上是$\hat\lambda $对应的帕累托最优面上的一组解，如果对偶间隙为0，则多目标优化在$\hat \lambda$下的最优解和$g(\lambda)$等价

### 经济学解释

产品产量x，$-f_0(x)$代表产品利润，目标
$$
\min f_0(x)
$$
约束$f_i(x)\leq 0$表明生产x件产品第i类原材料不超过给定值

> 原材料可以交易，价格为$\lambda_i\geq 0$，剩余原材料卖出的利润是
> $$
> -\sum_i \lambda_i f_i(x)
> $$
> 这样目标是
> $$
> g(\lambda)=\min f_0(x) +\sum_i \lambda_i f_i(x)
> $$
> $f_i(x)\geq 0$意味着要从市场买入原材料，这样
> $$
> g(\lambda) \leq p^*
> $$
> 意味着自由市场一定比商品不能自由交换的社会更有效率(弱对偶)

现在讨论强对偶的条件，假设
$$
d^* = p^*
$$
这种情况下对应的价格$\lambda^*$一定是对偶问题最优解(达到对偶问题目标函数的上界)，称为**影子价格**

> 1. 存在剩余$f_i(x^*)<0$，此时$\lambda_i^*= 0$
> 2. 若$\lambda_i^*>0$，则$f_i(x^*) = 0$

