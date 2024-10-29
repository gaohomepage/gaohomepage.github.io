---
title: Information Theory AEP
date: 2024-04-22 00:02:13
tags:
    - Information Theory
    - math
---
# Information Theory(AEP)

## law of large number

随机变量序列$\{X_i\}$收敛于随机变量X，有三种解释

1. 依概率收敛 $\forall \epsilon>0,Pr(|X_n -X|>\epsilon) \to 0\Leftrightarrow \lim_{n\to \infty} Pr(X_n = X) = 1$本质上是将一个事件发生的概率视为数列，借用数列极限的定义，允许不落在这个区间内，但是这个区间应该越来越小
2. 均方收敛 $E[(X_n-X)^2]\to 0$
3. 按照概率1收敛 $Pr(\lim_{n\to \infty} X_n =X)=1]\Leftrightarrow \forall \epsilon >0,\exist N\in N^+,s.t\ \forall n > N,P(|X_n- X|<\epsilon )=1 $，对应确定一个阈值，一定能保证在某个随机变量之后的所有随机变量全部落在这个区间内

> 对于按概率收敛，相当于事件$A_n = \{|X_n -X|>\epsilon\}$这件事发生的概率趋向于0
>
> 对于均方收敛，定义随机变量$Y_n = (X_n -X)$，有 $\lim_{n\to \infty}Y_n = 0$
>
> 对于按照概率1收敛，随机变量序列$X_n$的极限记作$Y = \lim_{n\to \infty} X_n$，即$,\forall \epsilon>0,\exist N,s.t\ \forall n> N,|X_n - Y |\leq \epsilon$，并且$Pr(Y=X) = 1$(对应极限无线逼近随机变量X)

三个收敛的强弱满足$2\to 1,3\to 1$，表明上看1/3十分相似，但是3关注的是两个随机变量**完全相等**的概率，而1关注的是两个随机变量**足够接近**的概率。举一个反例满足1但不能推出3

> 对于一组随机变量，它们是某个区间上的均匀分布，现在给定$[0,1)$区间上均匀分布的随机变量w
>
> 1. $X_1$固定为1
> 2. $X_2=1, w\in [0,1/2),0,w\in [1/2,1)$，$X_3 = 1,w\in [1/2,1),0,w\in [0,1/2)$
> 3. $X_4,X_5,X_6,X_7$当w落在$[0,1/4),[1/4,1/2),[1/2,3/4),[3/4,1)$时取1，其余时候为0
>
> $X_{2^n +k},k=0,1,2,\cdots,2^n-1$当X落在$[\frac{k}{2^n},\frac{k+1}{2^n})$上取1，其它时候取0，显然满足
>
> $\forall 2^m \leq n \leq 2^{m+1}-1$，都有$P(|X_n -0|\geq \epsilon ) = \frac 1 {2^m}$（只有一个长度为$\frac 1{2^m}$的区间取1，其余均为0），因此有$X_n$按概率收敛到0
> $$
> \forall\epsilon >0, P(|X_n-0|\geq \epsilon)\to 0
> $$
> 这是满足**依概率收敛**的，但是它不满足按照概率1收敛，对于随机变量$X_n(w)$，以及$\forall w\in (0,1)$
> $$
> \exist k \in [2^m,2^{m+1}-1],s.t\quad X_n(w) = 1
> $$
> 这种情况下
> $$
> \lim_{n\to \infty} X_n = 0
> $$
> 并不成立

大数定律中定义了一系列iid采样自分布$p(x)$的随机变量序列
$$
X_1,X_2,\cdots,X_n | p(x_n)
$$
定义如下序列
$$
\overline {X_n} = \frac{\sum_{i=1}^n X_i}{n}
$$
强大数定律表明$\overline X_n$按照概率1收敛于$E[X_1]$，即($\overline X_n$将会收敛到固定点)
$$
Pr(\lim_{n\to \infty} \overline X_n = E[X_1]) = 1
$$
弱大数定义表明$\overline X_n$依概率收敛到$E[X_1]$，即($\overline X_n$将会收敛到固定点附近$\overline X_n\to E[X_1]$)
$$
\lim_{n\to \infty} Pr(\overline X_n = E[X_1]) = 1\Rightarrow \forall \epsilon >0,\lim_{n\to \infty} Pr(|\overline X_n - E[X_1]|\leq \epsilon) = 1
$$

## AEP(渐进均分性)

### AEP

对于iid的随机变量$X_1,X_2,\cdots|p(x)$，满足(Weak)，联合概率分布的对数均值**依概率**收敛于随机变量的熵
$$
-\frac 1n\log p(x_1,x_2,\cdots,x_n) \to H(X)
$$
对于随机变量$x$，其发生的概率$p(x)$也是随机变量，这里其实定义的是
$$
LHS= \frac{-\sum_{i=1}^n \log p(x_i)}{n}
$$
定义新的随机变量$q = -\log p(x_i)$，显然其期望满足
$$
E[q] = \sum q Pr(q) = \sum_{x\in \mathcal X} -p(x_i)\log p(x_i) = H(X)  
$$
例如对于3-离散分布的随机变量X，有如下概率

| X    | 1    | 2    | 3    |
| ---- | ---- | ---- | ---- |
| P(X) | 0.3  | 0.6  | 0.1  |

定义$q=p(X)$，显然有

| q     | 0.1  | 0.3  | 0.6  |
| ----- | ---- | ---- | ---- |
| Pr(q) | 0.1  | 0.3  | 0.6  |

利用弱大数定律得到这个结论

> 这是什么近似？

### 典型集

对于IID的随机变量序列$X_1,X_2,\cdots$，满足如下条件（1/2等价）

1. $|-\frac 1 n \log p(x_1,x_2,\cdots,x_n) - H(X)|\leq \epsilon$
2. $2^{-n(H(X) +\epsilon)} \leq p(X_1,X_2,\cdots ,X_n)\leq 2^{-n(H(X)-\epsilon)}$

满足这两个条件的序列成为典型集，记作$A_\epsilon^{n}$

上面说了随着序列长度的增加均值$-\frac 1 n\log p(x_1,x_2,\cdots,x_n)$将会按概率收敛于$H(X)$，但是实际长度是有限的，渐进均分性回答了对于有限长度的iid序列$X_1,X_2,\cdots,X_n$逼近$H(X)$的速度

> 例子：对于二项分布X $P(X = 1)=0.6,P(X=0) = 0.4$，对于长度为25的序列，$\epsilon = 0.1$，序列中有多少个1，保证序列落在典型集$A_{\epsilon}^n$中
>
> 序列中包含m个1的概率为
> $$
> q = 0.6 ^m \times 0.4 ^{25 -m}
> $$
> 要求落在典型集中，意味着
> $$
> |-\frac{\log q}{25} - H(X) | \leq \epsilon
> $$
> 得到$m\in [11,19]$

接下来研究典型集的性质

> 对于充分大的n，典型集发生的概率很高，即$Pr(A_\epsilon^n)\geq 1 -\epsilon$

对于$(X_1,X_2,\cdots,X_n)\in A_\epsilon^n$，根据AEP有
$$
-\frac 1 n \log p(x_1,x_2,\cdots,x_n)\to H(X),as\ n\to \infty
$$
这意味着$\forall \delta>0,\exist n_0\in N$，满足（此时$\epsilon$已经事先给定）
$$
\forall n >n_0,Pr(|-\frac 1 n \log p(x_1,x_2,\cdots,x_n) - H(x)|\leq \epsilon) > 1-\delta
$$
令$\delta  = \epsilon$得证（对应的Pr内部指的是事件$x_1,x_2,\cdots,x_n$落在典型集中这个事件）

> 典型集合中元素的个数满足如下上下界
> $$
> (1-\epsilon)2^{n(H(X) -\epsilon)} \leq |A_\epsilon^n | \leq 2^{n (H(X) + \epsilon)}
> $$
> 在$\epsilon$很小的情况下，典型集的大小接近于$2^{n H(X)}$，相对于样本空间大小很小

先证明集合大小上界，显然有
$$
1 = \sum_{x\in \mathcal X} p(x) \geq \sum_{x\in A_\epsilon^n }p(x) \geq \sum_{x\in A^n_\epsilon} 2^{-n ({H(X) +\epsilon)}}  = 2^{-n(H(X) +\epsilon)}|A_\epsilon^n|
$$
落在全空间上的概率之和一定为1，放缩到全空间的子集$A_\epsilon^n$上，这种情况下有
$$
\frac{|A_\epsilon^n |}{|\mathcal X|} \leq 2^{n (H(X) +\epsilon -\log |\mathcal X|)}
$$
并且$H(X) -\log H(X)\leq 0$，这意味随着n的增加，典型集在整体中占据的比例将会降低

再证明其下界，借助概率下界
$$
Pr(A_\epsilon^n )>1-\epsilon
$$
得到
$$
1 - \epsilon \leq \sum_{x\in A_\epsilon^n} 2^{-n(H(X) - \epsilon)}
$$
得证

> 以上结论表明，随着n的增加，尽管典型集仅仅在全空间中占据很小一部分，但是随机采样有很大概率采到典型集中的元素

问题：能否进一步减少典型集中的元素，使得被“裁剪”后的典型集同样是高概率的？

### 高概率集

$A_\epsilon^n$是一个高概率集，它是典型的

选择全空间的一个子集，记作
$$
B_\delta^n \subseteq \mathcal X^n
$$
如果满足
$$
Pr(B_\delta^n) \geq 1 - \delta
$$
称$B_\delta^n$为smallest set

关于高概率集有如下定理

> 对于iid的随机变量序列$X_1,X_2,\cdots,X_n$，如果$B_\delta^n$是一个smallest set并且$\delta < \frac 1 2$，则$\forall \delta^\prime> 0$，都有
> $$
> \frac 1 n \log|B_\delta^n| > H-\delta^\prime
> $$
> 对于n足够大是成立的

借助$A_\epsilon^n \bigcap B_\delta^n$的概率证明这个结论，对于两个事件集合A/B，并且 $Pr(A) \geq 1 -\epsilon_1,Pr(B)\geq 1 - \epsilon_2$，我们有
$$
Pr(A\bigcap B)>1 -\epsilon_1 -\epsilon_2\\
A\bigcap B = (A+B) - A\bigcup B 
$$
在这里，我们有
$$
Pr(A_\epsilon^n) > 1- \epsilon(as\ n \ is \ large\ enough)\\
Pr(B_\delta^n) \geq 1 -\delta\\
Pr(A_\epsilon^n \bigcap B_\delta^n)> 1 - \epsilon - \delta\\
1 - \epsilon -\delta \leq \sum_{A\bigcap B} p(x) \leq \sum_{A\bigcap B} 2^{-n(H(x) -\epsilon)}
$$
因此得到
$$
|B|2^{-n(H-\epsilon)}\geq  |A\bigcap B| 2^{-n(H-\epsilon)}\geq 1 -\epsilon -\delta
$$
得证
$$
|B_\delta^n| \geq 2^{n(H-\epsilon)}(1 - \epsilon-\delta)
$$

## Data Compression

选择一种编码方案，使得平均编码长度最短

1. 输入Source sequence $X^n=(X_1,\cdots,X_n),X_i|p(X)$,iid
2. 通过Encoder-Decoder得到序列$\hat X_n$，编码得到m bit中间数据传输
3. 定义错误率 $P_e = P(X^n \neq \hat X^n)$，编码率 $R = \frac m n$

AEP在原始样本空间中划分出很小一部分典型集，发生这些事件的概率很高，可以用很短的编码方式表示它，典型集大小为
$$
2^{n(H+\epsilon)}
$$
需要用$n(H+\epsilon)+1 +1$比特进行编码（+1是为了在起始增加一个bit用于区分元素是否处于典型集中）

对于$x\notin A_\epsilon^n$，用$2\log |X| +1+ 1$比特编码

> 以上情况一定保证编码可区分（无损编码）

平均编码长度为
$$
E[l(X^n)] = \sum_{x^n \in A_\epsilon^n} p(x^n) l(x^n) +\sum_{x^n \notin A_\epsilon^n} p(x^n)l(x^n)\\
\leq Pr(A_\epsilon^n)(n(H+\epsilon)+2) + Pr({A_\epsilon^n}^c )(n\log |X|+2)\\
\leq n(H+\epsilon) + \epsilon n( \log |X|+2) = n(H+\epsilon^\prime)
$$
这种情况下压缩比为$H+\epsilon^\prime$

> 是否存在编码码率比$H$更小的情况？为了证明不存在这种编码，我们希望证明如下定理
>
> > 对于编码码率$r<H(X)$的情况，一定有$P_e \to 1$
>
> 证明：令$r = H(x) - \epsilon$，这种情况下编码出序列长度为$nr$，最多只能表示$2^{nr}$个不同序列，假设其都是落在典型集中的序列，则正确编码概率
> $$
> 1 - P_e = 2^{nr}2^{-nH} = 2^{-n(H-r)}
> $$
>
> > 这是因为典型集在全体序列中有为$2^{nH}$个，但是只能对其中的$2^{nr}$个可能做编码，并且典型集为最小高概率集
>
> 一定在$n\to \infty$趋向于0

