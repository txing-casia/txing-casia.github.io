---
layout:     post
title:      "Memristor‐Based Biologically Plausible Memory Based on Discrete and Continuous Attractor Networks for Neuromorphic Systems"
subtitle:   "基于离散和连续吸引网络的基于忆阻器的生物似然记忆，用于神经形态系统"
date:       2021-03-29
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
    - Attractor Networks
---

# Memristor‐Based Biologically Plausible Memory Based on Discrete and Continuous Attractor Networks for Neuromorphic Systems

论文链接：https://onlinelibrary.wiley.com/doi/10.1002/aisy.202000001

 ## 背景

- 神经形态计算是1990年代提出的一个新概念，它表明未来的计算机系统可以通过直接在物理水平上并行处理模拟信号来模仿人脑的操作原理。
- s按个优势：
  - 高并行性 high parallelism
  - 低功耗 low power consumption
  - 内存计算 in‐memory computing

- **忆阻器**可以根据内部状态和外部刺激（例如电压脉冲）来改变其电阻。基于忆阻器的纵横制结构可以通过依赖欧姆定律和基尔霍夫定律直接将向量矩阵乘法（VMM）（一种最密集的计算组件）映射到电参数来加速各种人工神经网络（ANN）

- VMM计算过程直接在原地发生，从而避免了从内存中获取数据而导致的内存墙（**冯·诺伊曼·瓶颈**）

- 如果吸引子是离散的，则初始状态将落入最近的吸引子。这是联想记忆的模型，该记忆先前已在忆阻器交叉开关上执行

- 然而，这些先前的研究使用称为Hebbian规则的离线学习，这意味着突触的权重已在软件中计算出来，并简单地映射到忆阻器交叉开关上。Hebbian rule的表现很差，无法支持大规模的吸引网络。

- 通过引入平移不变的钟形连接模式，网络吸引子可以奇妙地形成一个平面。这称为**连续吸引子神经网络（CANN）**

- 在记忆方面，人们认为大脑可以在动态分配过程中暂时记住当前状态，并随后使用此信息进行计算。这称为工作存储器，可以由CANN自然实现。

- 通过引入神经元之间的竞争与合作，一种名为**Oja规则**的有效的在线在线学习方法被应用到基于无监督学习的联想记忆中，表明该方法可以将性能提高至少10倍，这将大大减少芯片面积并增强硬件的健壮性。

## 工作内容

### 1. 离散吸引子网络

- 离散吸引子神经网络，也称为Hopfield神经网络，是完全连接的网络，其中每个神经元与其他神经元都有连接，但没有自连接

$$
X_i^{i+1}=sgn\bigg(\sum_j W_{ij}X_i^t+b_i\bigg)
$$

### 2. Oja学习规则

$$
Y=X'\times W\\
\Delta W'=\alpha (X-W'\times Y')\times Y
$$

- 其中*W*是权重矩阵，*α*是学习率

- 与以前使用软件计算权重的工作不同，此规则可以在交叉开关中在线训练网络，因此可以显着提高性能。该规则的主要原理是利用神经元之间的竞争，这种竞争也用在局部竞争算法（LCA）中，[27](https://onlinelibrary.wiley.com/doi/10.1002/aisy.202000001#aisy202000001-bib-0027)解释为Winner Takes All（WTA）。
- Oja规则的存储容量大约是Hebbian规则的10倍，并且由Oja规则训练的神经形态存储系统也具有更好的鲁棒性，这表现为更高的噪声容忍度，显示了整体上显着的性能提升。

### 3. 连续吸引子网络

$$
\tau\frac{\partial U(x,t)}{\partial t}=-U(x,t)+\rho \int_{x'} J(x,x')r(x',t) dx'+I^{ext}(x,t)\\
r(x,t)=\frac{[U(x,t)]^2_+}{1+k\rho\int_{x'}[U(x',t)]^2_+dx'}
$$



## 总结

想看看吸引子网络的，但它是Hopfield Neural Networks，激活函数是$$sgn()$$，和运动基元、增益场还是不一样的。







