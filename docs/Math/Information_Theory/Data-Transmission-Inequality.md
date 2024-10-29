---
title: Data Transmission Inequality
date: 2023-12-26 16:03:28
tags:
    - Information Theory
---
# Information Theory(Data Transmission Inequality)

## 回顾：Entropy and Mutual Entropy

Entropy是满足三条假设的用于刻画随机变量信息量的函数
$$
H(x)= E_{x}[-\log p(x)]
$$
互信息$I(X;Y)$被视为联合分布$P_{XY}$和两个边缘分布$P_X P_Y$的KL散度
$$
I(X;Y) = D(P(X,Y)\parallel P(X)P(Y)) = \sum_{x,y} P(X,Y)\log \frac{P(X,Y)}{P(X)P(Y)}
$$
可以写成
$$
\begin{aligned}
I(X;Y) &= \sum_{x,y}P(X,Y) \log \frac{P(Y|X)}{P(Y)} \\
&=\sum_{x,y} P(Y|X)P(X)\log P(Y|X) +\sum_y P(Y) \log \frac{1}{P(Y)}\\ &= -H(Y|X) +H(Y)\\
&=-H(X|Y) + H(X)= I(Y;X)
\end{aligned}
$$
从通信角度，互信息I(X;Y)建模了输入X，经过信道得到输出Y这个过程能够获得信息增益$H(Y)- H(Y|X)$或者是否能够根据信道输出反推出输入是什么，假定信道不包含任何噪音，则$H(Y|X) = 0$，此时有
$$
I(X;Y) = H(Y)
$$

## Data Processing Inequality

考虑单向MDP过程
$$
X\to Y \to Z
$$
一定有
$$
I(X;Z|Y) = 9
$$
这是因为给定Y，X/Z相对独立，此时有
$$
I(X;Z)+ I(X;Y|Z) = I(X;Y,Z) = I(X;Y) + I(X;Z|Y) = I(X,Y)
$$
因此
$$
I(X;Z)\leq I(X;Y)
$$
这种情况下，给定Z不会导致从X传输到Y的信息量增加，即
$$
I(X;Y|Z) \leq I(X;Y)
$$
这是因为Z的**知识**完全来自于Y，如果Z可以直接得到X的信息，此时$I(X;Y|Z)> I(X;Y)$也有可能成立

> 假设X、Y独立，则I(X;Y) = 0，规定$Z = X+Y$，此时一定有
> $$
> I(X;Y|X+Y) > 0
> $$
> 给定Z和X一定能推理出Y

同理，对于
$$
X_1\to X_2\to\cdots \to X_n
$$
$\forall 1\leq i\leq j \leq k \leq m \leq n$，有
$$
I(X_i,X_m)\leq I(X_j ,X_k)
$$

### Fano’s Inequality

给定两个随机变量X/Y作为输入/输出，两者满足联合分布P(x,y)，定义e为出现传输错误，即无法从Y中*恢复*出X（这个过程中需要定义decoder函数f，如果$\hat X=f(Y)\neq X$则任务e发生，这是一个0-1随机变量），那么满足
$$
H(X|Y)\leq H_b(e) +P(e)\log (|X|-1)
$$
左边代表从接收端推断传输端的不确定度（译码），取决于译码错误的概率和编码空间大小

这个公式的最大意义是确定了译码操作相对熵的上限

其中
$$
H(X|Y) = -\sum_{x,y}P(x,y)\log P(x|y)\\
H_b(e) = -(1 - P(e))\log (1-P(e)) - P(e)\log P(e)\\
P(e) = P(f(Y)\neq X)
$$
证明

定义
$$
E=1,\hat X\neq X\\
E= 0,\hat X=X,E[E] = P_e
$$
根据链式法则，得到
$$
H(E,X|\hat X) = H(X|\hat X) + H(E|X,\hat X)= H(E|\hat X) + H(X|E,\hat X)
$$
> 这里的证明基于条件熵的拆分公式$H(X,Y) = H(X)+ H(Y|X)$，得到推论$H(X,Y|Z) = H(X|Z) + H(X|X,Z)$多增加一份conditional

显然$H(E|X,\hat X) = 0$(因为$X=\hat X$或$X\neq \hat X$导致E的取值是确定性的)，并且$H(E|\hat X)\leq H(E)$，对应binary entropy

> 这里H(X,Y|Z)表示
> $$
> H(X,Y|Z) =\sum_z p(z) H(X|z,Y|z) = \sum_z p(z)\sum_{x,y} p(X,Y|z) \log \frac 1{\log p(X,Y|z)}
> $$

> $H(X|Y,Z)$表示为
> $$
> H(X|Y,Z) = \sum_{y,z}P(y,z) H(X|y,z)
> $$

拆分$H(X|E,\hat X)$，写成
$$
H(X|E,\hat X)= H(X|E=0,\hat X) P(E=0)+H(X|E=1,\hat X) P(E=1)= H(X|E=1,\hat X)P_{e}
$$
这是因为$H(X|E=0,\hat X)$必然满足$\hat X= X$，此时$H(X|E=1,\hat X)$对应的情况是$X\neq \hat X$，对应的upper bound是在输入空间-1上均匀分布

> 假设$X \in \{0,1,2,3\}$，对应$H(X|E=1,\hat X)$最大对应
> $$
> \hat X = k\to X|U(\{0,1,2,3\} - k)
> $$

即
$$
H(X|E,\hat X) \leq P_e \log _2{(|X| - 1)}
$$
因此有
$$
\begin{aligned}
H(X|\hat X) &= H(E|\hat X) + H(X|E,\hat X)\\
&\leq H(E) +P_e\log_2(|X| - 1)
\end{aligned}
$$

> 另一个证明（sharp bound，等号仅在特定时候成立），将其视为一个MDP
> $$
> X\to Y\to \hat X
> $$
> 引入decoder一个目的是让编码/解码空间相同，例如$X\in \{-1,1\}\Rightarrow Y\in \R$，此时一定有
> $$
> I(X;Y) \geq I(X;\hat X)\Rightarrow H(X) - H(X|Y)\geq H(X) - H(X|\hat X)
> $$
> 因此有
> $$
> H(X|Y)\leq H(X|\hat X)
> $$
> 只要证明$H(X|\hat X)\leq RHS$即可，根据定义
> $$
> P_e = \sum_{x\neq \hat x}P_{X,\hat X}(x,\hat x)\\
> 1-P_e = \sum_x P_{X,\hat X}(x,x)
> $$
> 直接相减
> $$
> \begin{aligned}
> H(X|\hat X) - h_b(P_e) -P_e\log_2(|X| - 1)&=\sum_{x\neq \hat x}P_{X,\hat X}(x,\hat x)\log \frac {1}{P(x|\hat x)}\\
> &+\sum_x P_{X,\hat X}(x,x)\log \frac {1}{P(x|\hat x)} \\&- P_e\log \frac {|X| - 1}{P_e} - (1-P_e)\log {\frac{1}{1-P_e}}
> \end{aligned}
> $$
> 拆分成
> $$
> \sum_{x,\hat x}P_{X,\hat X}(x,\hat x) \log \frac{ P_e}{(|X| - 1) P(x|\hat x)}\\
> \sum_x P_{X,\hat X}(x,x) \log \frac{1 - P_e}{P(x|x)}
> $$
> 将$\ln x$放缩为$x - 1$得到
> $$
> LHS \leq \sum_{x\neq \hat x}P_{X,\hat X}(x,\hat x)(\frac{ P_e}{(|X| - 1)P(x|\hat x)} - 1) +\sum_x P_{X,\hat X}(x,x) (\frac{1 - P_e}{P(x|x)} - 1)
> $$
> 成立仅当
> $$
> \frac{1 - P_e}{P(x|x)} = 1
> $$
> 得到
> $$
> \begin{aligned}
> LHS& \leq \sum_x\sum_{\hat x\neq x}P_{X,\hat X}(x,\hat x)\frac{P_e}{(|X| - 1)P(x|\hat x)} -\sum_x\sum_{\hat x\neq x}P_{X,\hat X}(x,\hat x) \\
> &+\sum_x\sum_{\hat x = x} P_{X,\hat X}(x,\hat x)
> \frac{1 - P_e}{P(x|\hat x)} -P_{X,\hat X}(x,x)\\
> &=\sum_x\sum_{\hat x\neq x} P_X(x) \frac{P_e}{|X| - 1}-\sum_{x}\sum_{\hat x}P_{X,\hat X}(x,\hat x) +\sum_x P_X(x) (1 - P_e) = 0\end{aligned}
> $$
> 

