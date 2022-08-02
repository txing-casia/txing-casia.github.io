---
layout:     post
title:      "Autonomous Driving | Urban Driver: Learning to Drive from Real-world Demonstrations Using Policy Gradients"
subtitle:   ""
date:       2022-08-03
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## Urban Driver: Learning to Drive from Real-world Demonstrations Using Policy Gradients  

- 取得了城市驾驶场景中最好的效果（Urban driving scenarios）
- 数据：使用100小时的城市道路专家示教数据
- 不必添加复杂的状态扰动；
- 不必在训练中收集额外的同策略数据；

### Introduction

![本文的闭环训练算法概览](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220803-1.png)

- 工业界最好轨迹规划器文献：
  - H. Fan, F. Zhu, C. Liu, L. Zhang, L. Zhuang, D. Li, W. Zhu, J. Hu, H. Li, and Q. Kong. Baidu apollo em motion planner. ArXiv, 2018.  

- 本文主要贡献：
  - 复杂城市驾驶场景中，第一个证明了用策略梯度学习，可以从大量真实世界演示数据中学习模仿驾驶策略；
  - 一个新的可微分仿真器，可基于过去的数据进行闭环仿真，并通过时间的反向传播计算策略梯度，实现快速学习；
  - 单纯在仿真器中训练可在真实世界中控制自动驾驶车辆，优于其他方法；
  - 源码可得：https://planning.l5kit.org.  

### Related work

- **Trajectory-based optimization**：

  - 这是当前工业界的主流方法（a dominant approach）

  - 依赖手工定义的损失和奖励
  - 损失的优化可结合一系列经典的算法：
    - A* [11]
    - RRTs [12]
    - POMDP with solver [13]
    - dynamic programming [14]  
  - 整体上是依赖human engineering，而不是数据驱动

- **Reinforcement learning（RL）**：

  - 依赖仿真器的构造、精确编码和优化的奖励信号
    - S. Shalev-Shwartz, S. Shammah, and A. Shashua. Safe, multi-agent, reinforcement learning for autonomous driving. ArXiv, 2016.  
  - 手工编程的仿真器，不能还原真实的长尾场景
  - 本文直接通过 mid-level representations 从真实世界的 log 中构建仿真环境

- **Imitation learning (IL) and Inverse Reinforcement Learning (IRL)**：
  -  原始的行为克隆（Naive behavioral cloning）面临协变量偏移问题（covariate shift）
  -   Adversarial Imitation Learning [31, 32, 33]，还没有在自动驾驶场景使用

- **Neural Motion Planners**：  













### 总结

