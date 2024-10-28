---
title: Convex Optimization:set and functions
date: 2023-04-05 11:00:35
tags:
    - Convex Optimization
---
# Convex Optimization——Sets and Functions

## Define and Examples of Convex Set

### Define

1. 凸集
2. n个点的凸组合
3. 集合C的凸锥 $\{tx+(1-t)y|\forall x,y\in C,\forall t\in[0,1] \}$
4. Cone:$\forall x\in C,t\geq 0 \to tx \in C$
5. Convex Cone $x_1,x_2\in C\to t_1 x_1 +t_2 x_2\in C,\forall t_1,t_2\geq 0$
6. Conic Combination $x_1,x_2,\cdots,x_k\in \R$.Conic Combination is $\sum_{i=1}^k \theta_i x_i,\theta_i\geq 0$
7. Conic hull of set C:C中所有点组成的Conic Combination

### Example oof Convex Cone

>  Normal Cone: the normal cone of a given set C and $x\in C$ is
> $$
> \mathcal N_{C} =\{g|g^T x\geq g^T y \forall y\in C\}
> $$
> 必然是Convex，随后不等式两边同乘正标量并不影响不等式，理论解释是和给定一个凸锥中一个向量和凸集中一点内积大于其和凸集中任何一点向量内积

## Properties and Operation of Convex Set

### Properties

#### 分割超平面

两个不相交凸集存在分割超平面

#### 支撑超平面

非空凸集在边界点处存在支撑超平面

### Operations

1. 凸集之交
2. 线性变换$aC+b =\{ax+b|x\in C \}$(拉伸+平移)
3. Affine images and preimages $f(x) = Ax+b,x\in C$，反之假设D为Affine Set，则$\{x:f(x)\in D \}$为Convex Set

> 讲义中还提到一个例子称为Linear matrix inequality
> $$
> A_1,A_2,\cdots,A_k,B\in S^n,x=(x_1,x_2,\cdots,x_k)\in \R^k\newline
> x_1A_1+x_2A_2 +\cdots+x_k A_k\succeq B
> $$
> 给定$A_1,A_2,\cdots,A_k,B$，则满足上述不等式的$x\in \R^k$组成的集合C是凸集
>
> > Suppose $x,y\in C,t\in [0,1]$，则要证明
> > $$
> > (t x_1+(1-t) y_1) A_1 +(t x_2 +(1-t) y_2) A_2 +\cdots+(tx_k +(1-y) y_k) A_k\succeq B
> > $$
> > 结论显然

> perspective images and preimages，定义$\R^n \times R_{++}\to \R^n$上的perspective function$R_{++}$是由正实数组成的空间
> $$
> P(x,z) = \frac{x}{z}
> $$
> $\forall z>0$如果$C\subseteq dom(P)$是凸的那么$P(C)$也是Convex的

## Define and Example of Convex functions

### Define

$$
f(t x+(1-t)y)\leq t f(x) + (1-t) f(y),t\in [0,1]
$$

### Example

1. Quadratic function $f(x) = x^T Q x+b^T x+c,Q$是正定矩阵
2. Least Square loss $\parallel y -Ax \parallel_2^2$
3. 任何向量范数$\parallel x\parallel$

> 1的证明，只需要证明
> $$
> (t x+(1-t) y)^T Q (tx +(1-t)) \leq t x^T Q x + (1-t) y^T Q y\newline
> $$
> 等价于
> $$
> \quad t^2 x^T Q x+(1-t)^2 y^T Q y + t(1-t) x^T Q y+ t(1-t) y^T Q x \leq t  x^T Q x +(1-t) y^T Qy
> $$
> 写成
\begin{aligned}
t(1-t) x^T Q x+t(1-t) y^T Q y - t(1-t) x^T Q y - t(1-t) y^T Q x&\geq 0\newline
x^TQ x +y^T Qy - x^T Q y - y^TQ x&\geq 0\newline
(x-y)^T Q(x-y)&\geq 0
\end{aligned}

> 2写成$f(x) = (y-Ax)^T (y -A x) =y^T y +x^T A^T A x-y^T A x-x^T A y$

## Properties of convex function

1. Epi graph定义为$epi(f) = (x,t)\in dom (f)\times \R:f(x)\leq t$，一个函数是convex当且仅当epigraph是convex
2. Sublevel set对于所有t都是convex

> Epigraph是$\R^{n+1}$维，而Sublevel set是$\R^n$维

## Convex Optimization(USTC)

### Affine Set

$x_1,x_2$，经过两点的直线表示为
$$
\theta x_1 + (1-\theta )x_2,\theta\in \R
$$
$\theta\in [0,1]$表示线段

定义Affine Set

> A Set $C$ is Affine Set $\Rightarrow ,\forall x_1,x_2\in C,\theta\in \R,(1-\theta)x_1+\theta x_2\in C$

### Affine Combination

$x_1,x_2,\cdots,x_n\in C,\theta_1,\theta_2,\cdots,\theta_n\in \R$并且
$$
\sum_{i=1}^n \theta_i =1
$$
仿射组合定义为
$$
\sum_{i=1}^n \theta_i x_i 
$$
仿射集的第二个定义为

> 对于$\forall x_1,x_2,\cdots,x_n\in C$，它们的仿射组合属于C

讨论特殊的仿射集，满足
$$
\theta x_1+ (1-\theta)x_2\in C\to \alpha x_1+\beta x_2\in C
$$
显然这样的集合需要满足$0\in C$，假设C是Affine Set，定义集合
$$
V = C-x_0 =\{x-x_0|x\in C \quad x_0\in C\}
$$
满足上述条件

> V定义了仿射集的一个变换，这是$\R^n$空间的一个子空间

### 例子

> 线性方程组的解集合$\{x|Ax=b \}$是Affine Set，对应的子空间是满足$\{x|Ax=0 \}$解空间(A的化0空间)

> 反之：任意仿射集可以看成线性方程解空间
>
> > 假设C是$\R^n$上的仿射集，必然$\exist A\in \R^{m\times n},b\in R^m,\forall x\in C,Ax = b$
> >

> 基于任意集合C，构造尽可能小的仿射集
>
> > 仿射包
> > $$
> > Aff \ C = \{\sum_{i=1}^k \theta_i x_i|x_1,x_2,\cdots,x_k \in C,\sum_{i=1}^k \theta_i =1 \}
> > $$
> > 仿射集的仿射包是它本身

### 锥和凸锥

> 1. 锥 $x\in C,\forall \theta \geq 0,\theta x\in C$
> 2. 凸锥 $x_1,x_2\in C,\forall \theta_1,\theta_2\geq 0$
> 3. 凸锥组合 $x_1,x_2,\cdots,x_k,\theta_1,\theta_2,\cdots,\theta_k\geq 0$, $\sum_{i=1}^k \theta_i x_i$称为k个点的凸锥组合
> 4. 凸锥包

### 空间和子空间

满足对加减数乘闭合并且是空间的子集

### 超平面与半空间

定义超平面
$$
\{x|a^T x=b \}
$$
半空间
$$
\{x|a^T x\geq b \}
$$

### 定义多面体

> $$
> P=\{x|a_j^T x\leq b_j,j=1,2,\cdots,m,c_i^Tx=d_i,i=1,2,\cdots,n \}
> $$

### 定义单纯性

> Suppose $x_0,x_1,\cdots,x_k\in\R^n$，$x_1-x_0,x_2-x_0,\cdots,x_k-x_0$线性无关，则称
> $$
> \{c|c =\sum_{i=0}^k \theta_i x_i,\sum_{i=0}^k \theta_i =1,\theta_i\in [0,1] \}
> $$

> 每一个单纯性都能写成多面体的形式
>
> > proof
> > $$
> > c = v_0 +\sum_{i=1}^k \theta_i(v_i-v_0)= v_0 +By\newline
> > B=(v_1-v_0,v_2-v_0,\cdots,v_k -v_0)\newline
> > y = (\theta_1,\theta_2,\cdots,\theta_k)^T
> > $$
> > $B\in \R^{n\times k},rank(B)=k$，因此可以变换为上三角矩阵
> > $$
> > AB = \begin{pmatrix}
> > I_k\newline
> > 0
> > \end{pmatrix}\newline
> > A= \begin{pmatrix}
> > A_1\newline
> > A_2
> > \end{pmatrix}
> > $$
> > 两边乘上A，得到
> > $$
> > Ac = Av_0 + AB y\Rightarrow \begin{pmatrix}
> > A_1\newline
> > A_2
> > \end{pmatrix}c +\begin{pmatrix}
> > I_k\newline
> > 0\end{pmatrix} y
> > $$
> > 得到
> > $$
> > A_1 x = A_1 v_0 +y\newline
> > A_2 x = A_2 v_0
> > $$
> > 考虑到
> > $$
> > \sum_{i=1}^k \theta_i \leq 1,\theta_i\geq 0
> > $$
> > 因此(21)写成
> > $$
> > A_1 x \geq A_1 v_0\newline
> > 1^TA_1  x\leq 1+ 1^T A_1 v_0\newline
> > A_2x= A_2 v_0
> > $$
> > 这样推导出它的多面体形式

### 对称矩阵集合

$$
S^n =\{x\in \R^{n\times n}|x=x6T \}
$$

对称半正定矩阵集合和正定矩阵集合
$$
S_+^n,S_{++}^n
$$
n=1 $S_+^1 = \R_+$

n=2，写成
$$
S_+^2 = \{\begin{pmatrix}x & y\newline y & x \end{pmatrix}|x>0,z>0,xz>zy \}
$$

### 保持凸性的操作

1. 交集
2. 仿射函数，m维到n维的线性变换

> 对应的逆映射也可以保证凸性
> $$
> f^{-1}(S) = \{x| f(x)\in S \}
> $$
> 如果S是凸集那么$f^{-1}(S)$也是凸集

3. 两个凸集的和$S_1+S_2 = \{x+y|x\in S_1,y\in S_2 \}$两个凸集的乘$S_1\times S_2 = \{(x,y)|x\in S_1,y\in S_2 \}$(先仿射变换，再相加)
4. 线性矩阵不等式

$$
A_i,x_i,B\in S^m(对称矩阵)\newline
A(x) = x_1 A +\cdots + x_n A_n \preceq B\to (A(x)-B)\preceq 0（半正定）
$$

则集合
$$
\{x|A(x)\preceq B \}
$$
为凸集

> 定义仿射变换
> $$
> f(x) = B - A (x)
> $$
> 输入是n个对称m维矩阵，输出一个m维对称矩阵(n维空间映射到1维空间，每个元素是一个对称矩阵)，$S_+^m$是凸集，则它的逆变换
> $$
> f^{-1}(S_+^m) = \{x|B-A(x)\succeq 0 \}
> $$
> $f^{-1}(S_+^m)$是凸集，因为$f$是仿射变换，得证

5. 椭球是球的仿射映射

> 定义椭球
> $$
> \epsilon =\{x|(x-x_c)^T P^{-1} (x-x_c)\leq 1 \},P\succeq 0
> $$
> 定义仿射变换
> $$
> f(u) = p^{1/2} u+x_c\newline
> \parallel u \parallel \leq 1\newline
> P^{1/2}P^{1/2}= P
> $$
> u表示一个球形空间
> $$
> \begin{aligned}
> \{f(u)|\parallel u\parallel \leq 1 \}& = \{p^{1/2} u +x_c|\parallel u \parallel \leq 1 \}\newline
> &=\{x|\parallel p^{1/2} (x-x_c)\parallel \leq 1 \}(u = p^{-1/2} (x-x_c))\newline
> &= \{x|(x-x_c)^T P^{-1} (x-x_c)\leq 1 \}
> \end{aligned}
> $$
>

6. 透视函数

$$
f:\R^{n+1}\to \R^{n}
$$

它定义在
$$
P = \R^n \times R_{++}
$$
第n+1维必须是正实数，定义为
$$
P(z,t) = \frac{z}{t},z\in \R^n ,t\in R_{++}
$$
透过原点**消灭**一个维度看到的坐标取负

透视函数是保证集合的凹凸性

> 考虑$\R^{n+1}$中的线段
> $$
> x= (\overline x,x_{n+1})\newline
> y = (\overline y,y_{n+1})\newline
> \overline x,\overline y\in \R^{n},x_{n+1},y_{n+1}\in R_{++}
> $$
> 线段经过透视函数得到n维线段
> $$
> P(\theta x+(1-\theta y)) = \frac{\theta \overline x+(1-\theta)\overline y}{\theta x_{n+1}+(1-\theta )y_{n+1}}\newline
> =\frac{\theta x_{n+1}}{\theta x_{n+1} + (1-\theta) y_{n+1}} \frac{\overline x}{x_{n+1}} + \frac{(1-\theta)y_{n+1}}{\theta x_{n+1} + (1-\theta) y_{n+1}} \frac{\overline y}{y_{n+1}}\newline
> =\mu P(x)+(1-\mu) P(y)
> $$

> 任意凸集的反透视都是凸集，定义反透视
> $$
> P^{-1}(C) = \{(x,t)\in \R^{n+1}|\frac{x}{t}\in C,t\geq 0 \}
> $$
> 只要C是凸集，则$P^{-1}(C)$仍然是凸集

7. 线性分数函数(先线性，再仿射)

$$
g:\R^n \to \R^{m+1}
$$

为仿射映射，写成
$$
g(x) = \begin{pmatrix} A\newline C^T \end{pmatrix} x + \begin{pmatrix} b \newline d\end{pmatrix}\newline
A\in \R^{m\times n},C\in \R^n
$$

实际上是线性变换

定义透视变换
$$
P: \R^{m+1}\to \R^m
$$
定义线性分数函数
$$
f:\R^n \to \R^m = P \odot g
$$
实际上写成
$$
f(x) = \frac{Ax+b}{c^T x +d},dom f = \{x|c^T x+d \}
$$
任意凸集经过两次变换后还是凸集(两个变换都是保凸的)

8. 两个随机变量的联合概率映射为条件概率

随机变量$u=1,2,\cdots,n,v=1,2,\cdots,m$，概率定义为
$$
p_{ij} = P(u=i,v=j)\newline
f_{ij} = P(u=i|v=j) = \frac{p_{ij}}{\sum_{k=1}^n p_{kj}}
$$
条件概率是线性分数映射，假设
$$
x = (p_{1j},\cdots p_{2j},\cdots,p_{nj})\in \R^n
$$
A可以看成
$$
A =(0,0,\cdots,1,\cdots,0)\newline
c = (1,1,\cdots,1)
$$
现在考虑联合概率是否为凸的，定义集合
$$
C = \{p|\exists \ i,j,s.t\ p_{ij}  = p \}
$$
对于离散情况应该不成立？

### 凸函数的延拓

$$
f = \left\{
\begin{aligned}
&f(x),x\in dom f\newline
&\infty ,x\notin dom f
\end{aligned}
\right.
$$

### 集合的示性函数是凸函数

定义凸集$C\in \R^n$是凸集
$$
f_c(x)=\left\{
\begin{aligned}
&\infty ,x\notin C\newline
& 0,x\in C
\end{aligned}
\right.
$$
必须是$\infty$，如果是有限值则不能保证凹凸性

### 凸函数的一阶条件

若$f:\R^n \to \R$可微，$\nabla f$在$dom f$上存在(f必须定义在开集上)，f是凸函数等价于满足

1. dom f是凸集
2. $f(y)\geq f(x)+\nabla f(x)^T (y-x),\forall x,y\in dom f$

> 如果$\exists x_0,s.t \ \nabla f(x_0) = 0$，则
> $$
> \forall y,f(y)\geq f(x_0)
> $$

### 映射凸函数到低维(曲面映射到曲线)

$$
\forall x\in dom f,\forall v,g(t) = f(x+tv)
$$

为凸，g定义在
$$
dom  g= \{t|x+tv\in dom f \}
$$
显然如果$dom f$凸，则dom g凸

> $\forall x,y\in dom f,v= y-x$，define
> $$
> g(t) = f(x+t(y-x))
> $$
> $g(0) = f(x),g(1) = f(y)$。Q.E.D

> 证明高维凸函数的一阶条件，convex $\to$一阶条件
> $$
> f(y)\geq f(x)+\nabla f(x)^T (y-x) 
> $$
> 定义
> $$
> g(t) = f(ty+(1-t)x) = f(x+t(y-x))
> $$
> $g(t)$是凸函数
> $$
> g^\prime(t) = \nabla f^T(x+t(y-x))(y-x)
> $$
> 则
> $$
> g(1) = f(y)\newline
> \nabla g(0) = \nabla f^T(x)\newline
> g(0) =f(x)
> $$
> 则
> $$
> g(1)\geq g(0)+\nabla g(0)
> $$
> 在dom f中找两个点
> $$
> a= x + t(y-x )\in dom f\quad
> b=x+\overline t(y-x) \in \dim f
> $$
> f满足一阶条件，写成
> $$
> f(a)\geq f(b) +\nabla f(b)(a-b)\newline
> f(x+t(y-x))\geq f(x+\overline t(y-x)) + \nabla ^T f(x+\overline t(y-x))(y-x)(t-\overline t)
> $$
> 带入$g(t)$
>
> 拆分成
> $$
> g(t)\geq \nabla g(\overline t)(t-\overline t) + g(\overline t)
> $$
> x,y任意选择，根据第二个条件的任意性得证

### 二阶条件

二阶hessian矩阵半正定(等价条件)
$$
\nabla ^2 f(x)\succeq 0
$$

> 如果Hessian矩阵是正定的，则它严格凸(整个结论反命题并不一定成立！$f(x)=x^4$)

需要保证定义在凸集上，比如
$$
f(x) = \frac{1}{x^2}
$$

### 范数

#### 0-范数(刻画向量的稀疏程度)

$x\in \R^n,\parallel x\parallel_0$是x中非零元素数量，对于n=1，写成
$$
\parallel x\parallel_0 =\left\{
\begin{aligned}
& 0 \quad x=0\newline
& 1 \quad x\neq 0
\end{aligned}
\right.
$$
但是它显然不满足凸函数，因为不满足
$$
\parallel x + y\parallel\leq \parallel x\parallel +\parallel y\parallel
$$

### 极大值

$$
f(x) = \max\{x_1,x_2,\cdots,x_n \}
$$

验证其凹凸性，一定有
$$
\max{x+y}\leq \max  x+ \max y
$$

#### 极大极小问题

$$
\min_x \max_y f(x,y)
$$

### 不可导函数的解析逼近(log-sum-up)

定义
$$
f(x) = \log {\sum_{i=1}^n e^{x_i}},x\in \R^n
$$
一定有
$$
\max\{x_1,x_2,\cdots,x_n \}\leq f(x)\leq \max\{ x_1,x_2,\cdots,x_n\}+ \log n
$$
对$f(x)$求偏导
$$
\frac{\partial f}{\partial x_i} = \frac{e^{x_i}}{\sum_{j=1}^n e^{x_j}}\newline
\frac{\partial^2 f}{\partial x_i \partial x_j} = H=(h_{ij})_{i=1,j=1}^{n,n}
$$
定义
$$
z = [e^{x_1},e^{x_2},\cdots,e^{x_n}]
$$
于是Hessian矩阵写成
$$
h_{ij} = \left\{
\begin{aligned}
& -\frac{e^{x_i+x_j}}{\parallel z\parallel_1^2},\quad i\neq j\newline
& \frac{-e^{2x_i}+e^{x_1}\parallel z\parallel_1}{\parallel z\parallel_1^2},\quad i=j
\end{aligned}
\right.\newline
\parallel z \parallel_1 = 1^T z
$$

Hessian矩阵写成
$$
H = \frac{1}{\parallel z\parallel_1^2}(diag(e^{x_i}(\sum_{j=1}^n e^{x_j})) - \begin{pmatrix}
e^{x_1}\newline
e^{x_2}\newline
\cdots \newline
e^{x_n}
\end{pmatrix}\begin{pmatrix}
e^{x_1},e^{x_2},\cdots,e^{x_n}
\end{pmatrix})=\frac{1}{\parallel z\parallel_1^2}(\parallel z\parallel_1^2 diag(z) - z z^T)=\frac{1}{\parallel z\parallel_1^2}K
$$
K是否为半正定？$\forall v\in \R^n$
$$
v^T K v = \sum_{i=1} z_i \sum_i v_i^2 z_i - (\sum_i v_i z_i)^2
$$
定义
$$
a_i = v_i \sqrt{z_i},b_i = \sqrt{z_i}\newline
$$
写成
$$
v^T K v = (b^T b)(a^T a) - (a^T b)^2\geq0
$$
Cauchy-Schwartz不等式
$$
\lang a,b\rang ^2 \leq \lang a,a\rang\lang b,b\rang
$$


### 行列式的对数

$$
f(x) = \log \det (X),X\in S_{++}^n
$$

X的特征值为$\lambda_1,\lambda_2,\cdots,\lambda_n> 0$，满足
$$
f(x) =\sum_{i=1}^n \log \lambda_i
$$
只需要证明
$$
\forall z\in S_{++}^n ,v\in\R^{n\times n},\forall t\in R\newline
z+tv\in S_{++}^n,g(t) = f(z+tv)
$$
是凹函数，可知$v\in S_{++}^n$，于是有
$$
g(t) = \log \det (z^{1/2}(I+t z^{-1/2} v z^{-1/2}) z^{1/2})=2\log \det {z^{1/2}}+\log\det(I+t z^{-1/2} vz^{-1/2})
$$
假设$\lambda_i$为$P =z^{-1/2}v z^{-1/2}$的特征值，写成
$$
g(t) = \log \det (z) + \sum_{i=1}^n \log (1+t\lambda_i)
$$
P是对称矩阵(多个对称矩阵乘积)，因此可以进行正交变换
$$
P = Q\Lambda Q^T,QQ^T = I
$$

考虑微分
$$
g^\prime(t) = \sum_{i=1}^n \frac{\lambda_i}{1+t\lambda_i}\newline
g^{\prime\prime}(t) =-\sum_{i=1}^i \frac{\lambda_i^2}{(1+t\lambda_i)^2}\leq 0
$$

### Operations that conserve convex

#### 非负加权和

$$
\sum_{i=1}^n w_i f_i,w_i\geq 0
$$

#### 积分

若$f(x,y),\forall x\in A$，都有$f(x,y)$都是凸函数(并不一定对(x,y)joint convex)，假设
$$
w(y)\geq 0,\forall y\in A(A不一定是凸集)
$$
定义
$$
g(x) =\int_{y\in A}w(y)f(x,y) dy 
$$
为凸

#### 仿射映射

$f(\R^n \to R),A\in \R^{m\times n},b\in \R^n$，定义
$$
g(x) = f(Ax+b)
$$
在`dom g`上是凸的

> 仿射映射保证`dom g`是凸集

$f_i(\R^n \to \R),i=1,2,\cdots,m$为凸，$A\in \R^{n},b\in \R$，定义
$$
g(x) = A^T [f_1(x),f_2(x),\cdots,f_m(x)]^T +b
$$
**不一定保证凸**，必须要限制**A>0**

#### max函数

$$
f(x) =\max (f_1(x),f_2(x))
$$

> 向量$x\in \R^n$中r个最大元素的和
> $$
> f(x) = \sum_{i=1}^r x^i
> $$
> 定义$1_n^r$表示n维向量，其中包含r个1，剩下位置是0，显然有
> $$
> C_n^r
> $$
> 个$1_n^r$向量，写成
> $$
> f(x )=\max 1_n^r x
> $$
> 因此这是凸函数

> 实对称矩阵的最大特征值(奇异值分解，主方向)
> $$
> f(x) =\lambda_{\max}(X),dom f= S^m
> $$
> 特征值满足
> $$
> X y = \lambda y
> $$
> 可以写成
> $$
> f(X) = \max_{\parallel v\parallel =1} v ^T X  v
> $$
> 因此凸

#### 函数的函数

$$
f(x) = h(g(x))
$$

$f(x),h(x)$均为convex function，讨论$g(x)$的凹凸性(一维，可微)

$$
f^{\prime\prime}(x) = h^{\prime\prime}(g(x)) g^\prime(x)^2 + h^\prime(g(x))g^{\prime\prime}(x)
$$

1. $h$是凸函数且不降，g为凸函数，则f为凸函数
2. h为凸函数，不增，g为凹函数，则f为凸函数

讨论
$$
h(x) = -\log x,x\in R_{++}
$$
并将其延拓到$\R$
$$
\overline h(x) = \left\{
\begin{aligned}
& -\log x,x\in R_{++}\newline
&\infty ,else
\end{aligned}
\right.
$$

1. $h$为凸,$\overline h$不降，g凸，则f为凸
2. h为凸,$\overline h$增，g凹，则f为凸

> $\forall x,y\in domf ,0\leq\theta \leq 1 $，g为凸，因此$dom g$是凸的，$\theta x+(1-\theta)y\in dom \ g$，并且
> $$
> g(x),g(y)\in dom h
> $$
> 因为g是凸的
> $$
> g(\theta x+(1-\theta)y)\leq \theta g(x)+(1-\theta)g(y)
> $$
> h为凸的，因此
> $$
> \theta g(x) + (1-\theta )g(y)\in dom h\newline
> h(\theta g(x) + (1-\theta )g(y))\leq \theta h(g(x)) + (1-\theta) h(g(y))=\theta f(x)+(1-\theta) f(y)
> $$
> 同时在(99)两边都加上h，得到
> $$
> h(g(\theta x+(1-\theta)y))\leq h( \theta g(x)+(1-\theta)g(y))
> $$
> 这个式子不是显然的，因为不能确定$g(\theta x +(1-\theta )y)\in dom h$
>
> 因此需要对h进行延拓，所以(101)总是成立

> $g$为凸，$\exp{g(x)}$为凸

#### 函数的透视

定义$f(\R^n\to \R)g(\R^n \times \R_{++}\to \R)$为透视函数，定义为
$$
g(x,t) = tf(\frac{x}{t})
$$
g定义在
$$
dom g = \{(x,t) |t>0\quad\frac{x}{t}\in dom f \}
$$
f是凸，g为凸，f为凹，g为凹

> $\forall (x_1,t_1),(x_2,t_2)\in dom g$，先证明dom g是凸集，$\forall 0\leq \theta \leq 1$，必然有
> $$
> \frac{x_1}{t_1},\frac{x_2}{t_2}\in dom f
> $$
> 

> 下面对于$\theta \in (0,1)$，构造凸组合
>$$
> x = \theta x_1 + (1-\theta ) x_2\newline
> t = \theta t_1 + (1-\theta )t_2
> $$
> 考虑
> $$
> \frac{x}{t} =\frac{\theta x_1 +(1-\theta)x_2 }{\theta t_1+(1-\theta)t_2}
> $$
> 已知
> $$
> \theta \frac{x_1}{t_1} + (1-\theta )\frac{x_2}{t_2}\in dom f
> $$
> 做变形
> $$
> \frac{x}{t} =\frac{\theta t_1}{t}  \frac{x_1}{t_1} + \frac{(1-\theta)t_2}{t}\frac{x_2}{t_2} = \Theta \frac{x_1}{t_1} + (1-\Theta)\frac{x_2}{t_2}
> $$
> 因此dom g也是凸集
> 
> 现在证明函数的凸性，对于$g(x,t)$，有
>$$
> \begin{aligned}
> g(x,t)& = (\theta t_1 +(1-\theta)t_2 )f(\frac{\theta x_1+(1-\theta)x_2}{\theta t_1 + (1-\theta)t_2})\newline
> &\leq (\theta t_1 + (1-\theta )t_2) (\frac{\theta t_1}{t} f(\frac{x_1}{t_1})+\frac{(1-\theta )t_2}{t}f(\frac{x_2}{t}))\newline
> &\leq \theta g(x_1,t_1) + (1-\theta)g(x_2,t_2)
> \end{aligned}
> 得证
> $$

例子：范数平方

> $f(x) = x^T x,dom f = \R^n$,定义
> $$
> g(x,t)=\frac{x^T x}{t}
> $$
> 对$(x,t)$联合凸

例子：负对数

> $f(x) = -\log x,dom f = R_{++}$
> $$
> g(x,t) = t(-\log \frac{x}{t}) = t \log \frac{t}{x}
> $$
> 实际上是交叉熵，进一步考虑
> $$
> g(u,v) = \sum_{i=1}^n u_i \log \frac{u_i}{v_i} = \sum_{i=1}^n g(u_i,v_i)
> $$
> 不能直接使用非负加权和的性质判断，因为不同函数实际上对应相同变量

扩展到Bregman Divergence，定义为
$$
D_B(u,v) = f(u) - f(v)-\nabla f(v)(u-v)
$$
当$f(x) = \sum_{i=1}^n x_i \log x_i -\sum_{i=1}^n x_i$时，`Bregman`退化为KL散度，实际上不能保证凸性

#### 函数的共轭

对于复数共轭的推广，假设$f(\R^n \to \R),$定义它的共轭为$f^*$，满足
$$
f^*(y) = \sup_{x\in dom f}(y^T x- f(x))
$$
对于n=1，$y^Tx$可以看成斜率为y的过原点直线在x处取值

> 1. 若$f(x)$可微，则$f^*(y)$对应的x满足$f^\prime(x) = y$(直线和某点处曲线的切线平行)
> 2. $f(x)$无论是否为凸函数，$f^*(y)$必然是凸函数(多个线性函数的最大值)，实际上$f^*(y)$是分段线性函数

> 关于函数扩展单调性的讨论(为何需要对h进行扩展)，对于$f:h\odot g$
> $$
> g(x)=x^2,dom g \in \R\newline
> h(z) = 0,dom z = [0,1]
> $$
> h(z)是凸函数，并且不增不降，现在考虑复合函数
> $$
> \overline h(g(x)) = \begin{aligned}
> &0,x\in [-\sqrt 2,-1]\bigcup [1,\sqrt 2]\newline
> &not define
> \end{aligned}
> $$
> 显然这个函数没有凹凸性，因为定义域不是convex set，因此对于复合函数$f = h\odot g$
>
> **需要保证h的扩展在全集上的单调性，才能判断复合函数的单调性**

> $g(x)\geq 0$为凸，则$g^p(x),p\geq 1$是凸的
>
> > 定义$h(z)$
> > $$
> > h(z)=\begin{aligned}
> > &z^p,z\geq 0\newline
> > &0,z<0
> > \end{aligned}
> > $$
> > 现在考虑复合函数$h(g(x))$，满足
> >
> > 1. g(x)为凸
> > 2. $h(z)$全局增
> >
> > 因此复合函数是凸函数

> 计算函数的对偶
> $$
> f(x)= ax+b
> $$
> 对偶是
> $$
> f^*(y) = \sup_{x\in dom f} (yx -(ax+b))=\left\{
> \begin{aligned}
> & -b,y=a\newline
> & \infty,else
> \end{aligned}
> \right.

> $$
>考虑对数函数
> $$
> f(x)=-\log x,x\in R_{++}
> $$
> 
>$$
> f^*(y) = \sup_{x> 0}(yx+\log x)\newline
> y+\frac{1}{x} = 0\to x=-\frac{1}{y} \newline
> f^*(y) = \left\{
> \begin{aligned}
> &-1-\log -y, y<0\newline
> &\infty,y\geq 0
> \end{aligned}
> \right.
> $$
> 考虑二次函数
>$$
> f(x)= \frac{1}{2}x^T Q x,Q\in S_{++}^n
> $$
> 计算
> $$
> f^*(y) =\sup_x (y^T x-\frac{1}{2}x^T Qx)
> $$
> 计算
> $$
> \frac{\partial y^T x-\frac{1}{2}x^T Qx }{\partial x}= y -Qx
> $$
> 得到
> $$
> x = Q^{-1}y,Q^{-1}\in S_{++}^n \newline
> f^*(y) = y^T Q^{-1} y - \frac{1}{2} y^T Q^{-1} y =\frac{1}{2} y^T Q^{-1}y
> $$
> 它的共轭仍然是二次项
> 
> 一个复数$a+bj$的共轭是$a-bj$，满足$\overline {\overline z}=z$，考虑函数共轭的共轭，显然没有
>$$
> f^{**} = f
> $$
> 因为共轭的共轭都是凸函数，如果约定f是凸函数并且f是闭函数，则共轭的共轭是其自身

### 凸集和凸函数

$\alpha-$sub level set

> 若$f(\R^n \to \R)$定义其$\alpha-sublevel$ set \
> $$
> C_{\alpha} = \{x\in dom f|f(x)\leq \alpha \}
> $$

性质：凸函数的所有$\alpha-$sublevel set都是凸集

> 反之不一定成立：如果函数的所有$\alpha-$sublevel set都是凸集，则它不一定是凸函数$f(x)=-e^x$

考虑$f(\R^2\to \R)$的$\alpha-$sublevel set $f(x_1,x_2)=x^T x$，$\alpha-$sublevel set满足
$$
x_1^2+x_2^2\leq \alpha
$$

### Quasi Convex function

所有$\alpha$-sublevel set都是凸的函数

> $$
> f(x)=\left\{
> \begin{aligned}
> & \sqrt x,x\geq 0\newline
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

拟凸下，有
$$
\forall x,y\in dom f,\max(f(x),f(y))\geq f(\theta +(1-\theta(y)))
$$

> 向量$x\in \R^n$的长度，定义为x中最后一个非零元素的个数
> $$
> f(x)=\left\{
> \begin{aligned}
> & \max_{i,x_i\neq 0} i,x\neq 0\newline
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
> f(x) =\frac{a^Tx+b}{c^T x+d}\newline
> dom f = \{x|c^T x+d >0 \}
> $$
> 是拟凸的
> $$
> S_\alpha \to \{x|c^T x+d >0,a^T x+b \leq \alpha (c^T x+d) \}
> $$
> 多面体一定是凸集

> $$
> f(x) = (x_1x_2\cdots x_n)^{\frac{1}{n}}在\R_{++}^n\bigcap \{\parallel R\parallel _2\leq 1\}上是凸函数
> $$
>
> 
