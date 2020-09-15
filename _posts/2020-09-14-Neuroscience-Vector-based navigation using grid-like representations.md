---
layout:     post
title:      "Neuroscience | Vector-based navigation using grid-like representations"
subtitle:   ""
date:       2020-09-15
author:     "Txing" 
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
---

# Vector-based navigation using grid-like representations in artificial agent

论文链接：https://www.nature.com/articles/s41586-018-0102-6/

## 背景

RL在许多方面取得了好的成绩，但在哺乳动物的空间行为（mammalian spatial behaviour）的熟练度上，人工agent的性能仍然不足。 动物的空间感知由网格细胞（grid cell）支持，它提供了一种度量编码空间（a metric for coding space）的能力。本文模仿grid cell在DRL的基础上开发一种类哺乳动物的导航能力（mammal-like navigational abilities）。

被赋予了grid-like representations的agent在导航任务上性能超过了人类专家in challenging, unfamiliar, and changeable environments，甚至有走捷径的能力。

### 概念

- 路径整合（path integration）是指巡航者对与自身运动有关的信息进行整合来完成巡航任务的过程。例如agent在地图上绕了一圈到达目标位置，那么第二次出发的时候，能够整合之前的路径信息，得出更直接到达目标位置的路径。（He, Q., & Mcnamara, T. P. (2018). Spatial Updating Strategy Affects the Reference Frame in Path Integration. *Psychonomic Bulletin & Review*, 25(3), 1073-1079.）

## 主要工作

- 把速度信息作为输入给到一个LSTM循环神经网络，网络更新通过Backpropagation through time。LSTM映射位置和方向到一个线性层，在线性层中使用dropout，每个时刻随机50%的神经元被沉默。

- foraging behaviour觅食行为涉及到路径集成能力
- entorhinal network内嗅皮层网络帮助形成稳定的空间活动
- 实验效果如下图：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200914-1.png)

- vector-based navigation: 内嗅网格细胞产生目标驱动向量度量欧式空间，使得动物能够沿着一条直接路径到达目标。文中利用监督学习来实现这一能力。
- grid network输入是视觉信号，通过一个视觉网络实现。线性层输出是位置信息，相当于agent当前的位置。同时，线性层的信号（goal grid code）作为输入传入一个Policy LSTM网络，这个网络也是用于控制agent的行为。



## 方法

### 1 Path integration: supervised learning experiments

#### 1.1 Ground truth place cell distribution  

- 地图直径为$$L$$

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200914-2.png)

- 符号说明：
  - Place cell激活$$\overrightarrow{c} \in [0,1]^N$$
  - 位置$$\overrightarrow{x} \in R^2$$
  - place cell centres  $$\overrightarrow{\mu}_i^{(c)} \in R^2$$
  - place cell scale   $$\sigma^{(c)}$$

#### 1.2 Ground truth head-direction cell distribution  

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200914-3.png)

- 参数说明：
  - Head-direction cell激活$$\overrightarrow{h} \in [0,1]^M$$
  - 给定面向角度$$\varphi$$
  - 使用mixture of Von Mises distributions with concentration parameter $$k^{(h)}$$

#### 1.3 Supervised learning inputs  

- 每个时刻传入速度$$v_t\in \mathbb{R}$$以及the sine and cosine of its angular velocity $$\varphi_t$$  

#### 1.4 Grid cell network architecture  

- consists of three layers: **a recurrent layer**(LSTM, 128 hidden units ,with no peephole connections), **a linear layer**, and **an output layer** (Fig 1a).
- Input to the recurrent LSTM layer:  $$[v_t,\sin(\varphi_t),\cos(\varphi_t)]$$
- LSTM的初始**细胞状态**和**隐层状态**为 $$\overrightarrow{l}_0$$ and $$\overrightarrow{m}_0$$

$$
\overrightarrow{l}_0=W^{cp}\overrightarrow{c}_0+W^{(cd)}\overrightarrow{h}_0\\
\overrightarrow{m}_0=W^{hp}\overrightarrow{c}_0+W^{(hd)}\overrightarrow{h}_0
$$

$$(W^{cp},W^{cd},W^{hp},W^{hd})$$是两个线性转换器的参数

- 通过线性编码网络的均值，以及LSTM的输出$$\overrightarrow{m}_t$$，计算预测的place cells$$\overrightarrow{y}_t$$和head direction cells $$\overrightarrow{z}_t$$ 
- 线性编码网络包含三组权重和偏置。
  - 第一组：从LSTM的隐层状态$$\overrightarrow{m}_t$$映射到线性激活层$$\overrightarrow{g}_t\in \mathbb{R}^{512}$$的权重和偏置
  - 第二组：从线性激活层$$\overrightarrow{g}_t$$映射到预测的头部朝向$$\overrightarrow{z}_t$$的权重和偏置通过softmax functions  
  - 第三组：从线性激活层$$\overrightarrow{g}_t$$映射到预测的位置细胞输出$$\overrightarrow{y}_t$$的权重和偏置通过softmax functions  

- dropout：线性激活层$$\overrightarrow{g}_t$$每个单元的沉默概率是0.5
- 注意线性编码网络中间没有非线性层

#### 1.5 Supervised learning loss  

- 使用交叉熵损失函数

$$
L(\overrightarrow{y},\overrightarrow{z},\overrightarrow{c},\overrightarrow{h})=-\sum_{i=1}^{N} c_i \log(y_i)-\sum_{j=1}^{M} h_j \log(z_j)
$$

监督学习部分的结构:

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200915-1.png)

Grid cell agent整体结构：

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200915-2.png)

## 总结

grid cells的相关生物学研究获得过[2014年诺贝尔生理学奖](https://link.zhihu.com/?target=https%3A//www.nobelprize.org/nobel_prizes/medicine/laureates/2014/)for their discoveries of cells that constitute a positioning system in the brain。Deepmind甚至请了这个诺贝尔奖获得者来为本文写[序言](https://link.zhihu.com/?target=https%3A//f1000.com/prime/733198068%3Fkey%3DnvlnlWetE8dlZTy)。

这项工作如果用在导航上是比较有意义的，利用grid cells的空间信息处理机制，agent可以自动寻路，还可以做路径整合，得到更优化的路径。

当用在机器人运动轨迹的优化上时，由于机器人本体（基坐标）位置并没有发生变动，所以并不能像导航任务中那样自由自在地探索，可以仔细考虑一下其中的差别，制定一个新的实验范式。



