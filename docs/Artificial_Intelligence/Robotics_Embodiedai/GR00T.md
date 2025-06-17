# GR00T

数据金字塔

![image.png](https://s2.loli.net/2025/05/14/6snYRwZBuWXqIgt.png)

1. 真实世界中的数据总是较少，需要利用来自视频和网络的异构数据
2. 使用 latent-action codebook 并利用逆动态模型推算视频的伪标签（Action）
3. 模型架构（多模态融合VLM+动作生成 DiT）

![image.png](https://s2.loli.net/2025/05/14/Qs9Hz4IE3yjCKoa.png)

## 模型架构

状态/动作编码：MLP，一次预测未来 H 个时刻的动作

### VLM

基于 Internet-scale 数据的预训练模型

### DiT

采用逐步去噪的方式获取真实的动作，输入动作 chunk（包含多个动作）$A_t$，一个加噪步骤以系数$\tau$添加高斯噪声$\epsilon$
$$
A_t^\tau = \tau A_t + (1 - \tau)\epsilon
$$

DiT 希望对输入的加噪动作 $A_t^\tau$进行去噪 ，得到$A_t$

## 数据生成和训练

### 为无标签数据生成 latent action

希望解决无标签数据缺乏可学习的动作标签这一缺点，这里利用连续视频帧学习 latent Action

1. 输入：当前帧 $x_t$和未来帧$x_{t+H}$
2. encoder 输出：latent action $z_t$
3. decoder 输出：根据latent Action 和当前帧恢复未来帧$x_{t+H}$

将训练的得到的 encoder 作为 reverse model 为视频打标签，通过这种方式获取统一的action space

Latent space 采取 VQ-VAE

### 利用可控视频生成模型进行数据生成

给定初始帧和自然语言指令（任务描述）生成视频进行数据扩增

### 训练

1. 预训练：在多个不同的具身数据集上利用 flow-matching训练，将动作gt 和 latent action 作为训练目标用于计算 flow matching loss
2. 后训练：固定文本编码器，微调模型其他部分