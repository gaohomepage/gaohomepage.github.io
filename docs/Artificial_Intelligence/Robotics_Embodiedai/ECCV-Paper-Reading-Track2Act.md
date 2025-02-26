# Paper Reading(Track2Act)

## Motivation

1. 借助Human video学习可行的手与物体交互经验并将其转化为机器人动作执行序列
2. 预测二维视频流序列
3. 借助2D Tracker推理操作物体的刚体变换（空间中的旋转和偏移）

> 目标：将操作策略视为一系列interaction-plan(IP)，通过大规模人类和机器人操作数据集学到interaction plan， interaction plan在本文中以预测点在空间中的移动情况给出

更具体地说，IP被定义为，给定

1. 场景的初始图片
2. 场景的目标图片（定义task）
3. 初始图片上的一系列随机点

输出initial image到goal image的轨迹

## Related Works

### 从视频中理解手与物体的交互关系

1. 估计物理量，包括hand/object pose estimation，预测抓取操作的热点
2. 基于场景生成视频
3. 获取视频帧之间的visual correspondence

### 学习操作任务的visual representation

1. image observations
2. structure representation(点云/waypoint)
3. flow based representation

## Methods

### Web Video Tracker Prediction Model

采用Dit进行轨迹预测，给定一个gt的二维location $[P_t]_{t=1}^H$，包含未来H张Frame

1. 前向过程，逐步给$P_t$添加一个噪声$\epsilon_k$，k是diffusion step，最终获取的$\overline P_t$等价于标准高斯分布
2. 逆向过程，训练一个noise predictor $\mathcal V_\theta(I_0,G,P_0,k)$进行去噪，将初始图像$z_0$和目标图像$z_g$的embedding作为condition帮助扩展过程

### 推理：从IP到Manipulator Trajectory

基于得到的Tracker Predictor预测一个IP，包含若干二维点组成的轨迹序列，基于深度相机将其第一帧的平面点映射到空间得到三维空间
$$
P_0^{3D} = {(x_0^i,y_0^i,z_0^i)}_{i=1}^p
$$
希望基于轨迹序列推理出一个物体刚性变换的Transformer $\mathcal T$，这个问题formulate一下，给定

1. 初始3D点集合$P_0=(x_0^i,y_0^i,z_0^i)_{i=1}^p$
2. 二维时序上的序列$\tau_{obj} = \{(x_t^i,y_t^i)_{i=1}^p \}_{t=1}^H$
3. 相机内参矩阵K

为了让第t个step得到的三维点落在平面上的对应位置（和二维平面上的序列保持一致），本文目标是为每一步预测一个刚体的变换矩阵$T_t$，这个矩阵刻画了一系列点在空间中如何移动，目标是让
$$
KT_tP_0 = (u_t^i,v_t^i,1)
$$
将上述目标转化为一个Loss，优化每个step对应的Rigid Transformer，随后根据Transformer预测出下一时刻的End-effector Pose $\overline a_t$
$$
\overline a_t = T_t e_0
$$

### 闭环控制，避免执行过程中的意外情况

学习残差策略，目标是学习$\delta a_t$，真实pose由两个pose组合生成
$$
\hat a_t = \overline a_t +\delta a_t,\delta a_t = \pi_{res} (I_t,G,\tau,[\overline a_t]_{t=1}^H)
$$
需要根据当前图像、目标图像和预测轨迹以及开环动作计算resident pose

通过这种方式，可以修正视频数据和部署环境的差异

## Reference Paper

1. Iterative ﬂow minimization for robotic object rearrangement  
2. Toolﬂownet: Robotic manipulation with tools via predicting tool ﬂow from point clouds  
3. Robotap: Tracking arbitrary points for few-shot visual imitation  

