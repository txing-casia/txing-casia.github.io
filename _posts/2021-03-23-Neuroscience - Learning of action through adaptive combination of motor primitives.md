---
layout:     post
title:      "Neuroscience|Learning of action through adaptive combination of motor primitives (Nature)"
subtitle:   "运动基元"
date:       2021-03-23
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Neuroscience
    - Motor Primitives
---

# Learning of action through adaptive combination of motor primitives

论文链接：https://www.nature.com/articles/35037588

## 背景

理解大脑如何构造运动仍然是神经科学领域的一项基本挑战。

大脑可以通过运动基元灵活组合来控制复杂的运动，其中每个基元都是感觉运动地图中的计算元素，它将所需的肢体轨迹转换为运动命令。

## 主要工作

- 人在运动之前，通过预测力或者力矩，构建运动指令。然后不断调整预测，使得形成的轨迹近似期望的路径。

- 训练区域外的运动会被训练区域内的训练影响，说明大脑构建了一个状态依赖的近似外部力的**内部模型**（internal model）

- 内部模型的泛化性能通过“抓取实验”来量化验证

  Shadmehr, R. & Mussa-Ivaldi, F. A. Adaptive representation of dynamics during learning of a motor task. J. Neurosci. 14, 3208–3224 (1994).

  Ghez, C., Krakauer, J. W., Sainburg, R. L. & Ghilardi, M. F. in The New Cognitive Neurosciences (ed. Gazzaniga, M. S.) 501–514 (MIT Press, Cambridge, Massachusetts, 2000).

- **本文主要介绍了如何通过灵活地组合运动基元构建内部模型，形成感觉-运动地图（sensorimotor map），将期望的轨迹变为力矩，并形成泛化能力。**

- 学习粘滞力场（viscous forces）$$f=B\dot{x}$$

- 随着场空间频率的增加，对于所有基函数宽度，内部模型的精度都会下降。但是，更广泛的基础的学习能力在较低的频率下崩溃了。这与最近的发现是一致的，即人类在较高的空间频率力场中表现出的适应能力较弱。

  Matsouka, Y. Models of generalization in motor control. PhD thesis, MIT ( 1998).

## 总结

算是运动基元的比较早的也是比较权威的工作（Nature）

