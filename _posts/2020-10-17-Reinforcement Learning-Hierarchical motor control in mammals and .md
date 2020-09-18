---
layout:     post
title:      "Reinforcement Learning | Hierarchical motor control in mammals and machines"
subtitle:   ""
date:       2020-09-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Hierarchical motor control in mammals and machines 

论文链接：https://www.nature.com/articles/s41467-019-13239-6

## 背景

很多人工智能领域的进展启发了神经科学的研究。多数神经科学的研究都致力于探索单个任务空间的离散任务，而AI研究主要在agent玩复杂的游戏上。近期生物上的“synthetic motor control”却很少有人讨论。

本文是一篇讨论AI和神经科学关于HRL以及H control的综述，角度比较好，值得一读。



## 主要工作

- 神经科学启发了机器人中的subsumption architecture，简单搜了一下类似于层级机构的雏形（1986）。
- Optimal feedback control (OFC)研究如何优化运动，降低优化函数的值，取得最优性能；神经科学研究的是如何理解通过神经系统生成不同的行为动作。

下面介绍几个重叠领域的重要工作：

### 1. computational approaches to motor control

- 运动控制在animal and artificial system中都是值得关注的问题；
- 控制通过一个controller或者policy
- 图1提供了三种控制结构，

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200917-1.png)

- 生物：内部前向模型Internal forward models are used to predict the future consequences of actions。该模型是允许虚拟行为的，可以预测行为后果。

- OFC被用于解释volitional control意志控制

  - Diedrichsen, J., Shadmehr, R. & Ivry, R. B. The coordination of movement: optimal feedback control and beyond. Trends Cogn. Sci. 14, 31–39 (2010).
  - Scott, S. H. The computational and neural basis of voluntary motor control and planning. Trends Cogn, Sci, 16, 541–549 (2012).  

- OFC几乎可以概括所有的闭环控制算法，一些扩展的OFC主要是考虑了以下三点：

  - 运动控制目标是最大化一个优化函数；
  - 利用传感反馈来进行行为选择；

  - 内部模型帮助弥补传感延迟和执行状态估计；

- OFC对目标函数的组成结构非常包容，甚至不需要一个直接的目标，但在快速求解上有一点缺陷

### 2. Motor control of synthetic systems  

- 这部分内容主要涉及DRL，没什么值得说的。
- planning methods: 
  - Mordatch, I., Todorov, E. & Popović, Z. Discovery of complex behaviors through contact-invariant optimization. ACM T, Graphics (TOG) 31, 43 (2012).
  - Mordatch, I., Wang, J. M., Todorov, E. & Koltun, V. Animating human lower limbs using contact-invariant optimization. ACM T. Graphic. 32, 203 (2013)  

- 导航任务：Jaderberg, M. et al. Reinforcement learning with unsupervised auxiliary tasks. In International Conference on Learning Representations, 2017.  

- **挑战1：**DRL的优化目标比较狭窄比如走路faster，保持平衡稳定，运动到某个位置等等，很难设计有泛化能力的目标。
- **挑战2：** **处理变化的目标**。通过其他任务的经验生成运动，适应变化的目标。

### 3. Core principles of hierarchical motor control  

- Information factorization （信息因子分解）




















## 方法














## 总结









