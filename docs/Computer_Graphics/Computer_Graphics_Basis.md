---
title: CG Basics
date: 2024-04-25 15:12:22
tags:
    - Computer Graphics
---
# 计算机图形学基础

## 视角(view)和投影(projection)变换

### Reference

1. [https://krasjet.github.io/quaternion/quaternion.pdf](四元数)
2. [视角和投影变换]([视图变换和投影变换矩阵的原理及推导，以及OpenGL，DirectX和Unity的对应矩阵 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/362713511))

### 相机变换

变换的目的是将三维图形投影到二维平面上，通过光栅化获得像素的离散表征，给定相机的位置$e$，look at方向$g$（相机朝向），up方向$t$，获得一个新坐标轴，原点在e，y轴方向和t相同，z轴方向和g相反，视角变换矩阵$M_{view}$拆分成旋转$R_{view}$和平移$T_{view}$两项
$$
M_{view} = R_{view}T_{view},M,R,T\in \R^4
$$
> 考虑三维空间中的旋转，v分解为平行和垂直分量
> $$
> v = v_\parallel + v_\perp\\
> v_\parallel = (u\cdot v) u\\
> v_\perp = v - (u\cdot v) u
> $$
> 仅仅旋转$v_\perp$，增加一个轴，记作
> $$
> w = u \times v_{\perp}\\
> \parallel w \parallel  = \parallel u \parallel \parallel v_\perp\parallel = \parallel v_\perp \parallel
> $$
> 将变换后的$v_\perp^\prime$分解到w和$v_\perp$方向，得到
> $$
> v_\perp^\prime  = v_v^\prime + v_w^\prime =\cos \theta v_\perp  + \sin \theta (u\times v_\perp)
> $$
> 它们的关系是
>
> ![image-20240423233028735](https://s2.loli.net/2024/04/23/jFJTCP3DdrLvhcn.png)
>
> 因此新的旋转结果记作
> $$
> \begin{aligned}
> v^\prime  &= v_\parallel + \cos\theta v_\perp  +\sin\theta(u\times v_\perp)\\& = (u\cdot v)u +\cos\theta (v - (u\cdot v)u) + \sin\theta(u\times v) \\&= \cos\theta v + (1 -\cos\theta)(u\cdot v) u +\sin\theta(u\times v)
> \end{aligned}\label{rotate_eq}
> $$
> 旋转轴是一个单位向量

为了进一步讨论三维空间中的旋转，我们引入四元数，在这种情况下空间中任何一个点表示为
$$
(x,y,z) \to q = w+ i x+ j y + kz=s+v
$$
对应的共轭四元数为$s - v$，定义
$$
ij = k,jk = i,ki = j,i^2 = j^2 = k^2 = -1
$$
两个四元数相乘记作
$$
q_1 q_2 =s_1 s_2 - v_1 \cdot v_2 +s_1 v_2 + s_2 v_1 + v_1 \times v_2
$$
写成矩阵形式

![image-20240423234104826](https://s2.loli.net/2024/04/23/EYiKmsnhl47eZub.png)

一对互逆的四元数满足
$$
q q^{-1} = 1
$$
共轭四元数指的是
$$
q = w + v\\
\overline q = w -v\\
q \overline q = \parallel q\parallel ^2 = w^2 + \parallel v\parallel ^2
$$
我们将$\ref{rotate_eq}$中的向量视为纯四元数
$$
v = [0,v],v_\perp = [0,v_\perp],v_\parallel  = [0,v_\parallel],v = v_\perp + v_\parallel
$$
考虑垂直分量，写成纯四元数的形式
$$
v_\perp^\prime = \cos\theta v_\perp + \sin\theta(u\times v_\perp)=\cos\theta v_\perp +\sin\theta (uv_\perp) = (\cos\theta +\sin\theta u)v_\perp = qv_\perp\\
q = (\cos\theta,\sin\theta u)
$$
这是垂直向量的旋转公式，因此旋转被记作
$$
v^\prime = v_\parallel + qv_\perp
$$

> 对于单位向量u以及$q = [\cos\theta ,\sin\theta u]$，有$q^2 = qq = [\cos2\theta,\sin 2\theta u]$，并且q也是单位四元数，此时若$p =[\cos\frac \theta 2,\sin \frac \theta 2u]$，有$p^2 = q,p^{-1} = [\cos\frac{\theta}2 ,- \sin\frac\theta 2u]=p^*$

进一步写出
$$
v^\prime = v_\parallel +qv_\perp = pp^{-1}v_\parallel+ pp v_{\perp} =pp^*v_\parallel +pp v_\perp
$$
简化这个公式，介绍两个引理

![image-20240424001049867](https://s2.loli.net/2024/04/24/oMmv2cVASJjGQRl.png)

![image-20240424001108272](https://s2.loli.net/2024/04/24/CiNQj2Khfgn9tr5.png)

$v^\prime$进一步优化为
$$
v^\prime = p(v_\parallel + v_\perp)p^* = pv p^* = pvp^{-1},p^2 = q = (\cos\theta,\sin\theta u)
$$

> 这个公式表明对$v_\perp$做了正反两次旋转

最终写成终极形式，左乘四元数$a + bi +cj +dk$等价于乘以

![image-20240424001358950](https://s2.loli.net/2024/04/24/nHubKF8kg9RoeGT.png)

右乘等价于乘以矩阵

![image-20240424001456714](https://s2.loli.net/2024/04/24/GTCpvtMPkhDXr1U.png)

最终写成
$$
qv q^* = L(q) R(q^*)v
$$
最终旋转定理写成

![image-20240424001541332](https://s2.loli.net/2024/04/24/6IYCRchLa7ABjky.png)

v最后一维是0，因此取上三角

![image-20240424001605722](https://s2.loli.net/2024/04/24/dBLfF5uSRKeO82H.png)

## alpha-blending

渲染过程中引入透明度的概念，考虑光线可以经过面元的情况，对于渲染管线，引入$\alpha$参数描述图层的透明度

### 深度缓冲(物体之间完全遮挡，不考虑物体的透明度)

为每个像素构建如下缓冲

1. 深度缓冲，记录距离相机最近的面元深度
2. 帧缓冲，记录像素点颜色

渲染对于每个面元先进行深度测试，舍去比Z-buffer更远的面元，深度写入更新Z-buffer

### 透明度混合：存在透明物体进行渲染

不能直接放弃深度更大的物体，深度阈值对应距离成像平面最近的**不透明**片元的深度值，渲染透明片元仍然应该进行深度测试，避免不透明物体在透明物体之前的情况

> 先渲染透明片元，在渲染不透明片元，因为透明片元不写到Z-buffer中，如何判断两者的深度，这里提供两条原则
>
> 1. 透明片元按照深度逐个渲染
> 2. 不透明片元优先于透明片元进行渲染（深度测试不为空）
