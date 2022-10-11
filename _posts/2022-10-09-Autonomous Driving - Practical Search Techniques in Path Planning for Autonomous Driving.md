---
layout:     post
title:      "Autonomous Driving | Practical Search Techniques in Path Planning for Autonomous Driving"
subtitle:   "Hybrid A* method"
date:       2022-10-09
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Autonomous Driving
    - Planning
---

## Practical Search Techniques in Path Planning for Autonomous Driving

本文是stanford大学在 2007 DARPA Urban Challenge 中产出的一项工作，提出了著名的混合A*算法。

混合A*算法包含两个主要步骤：

- 1. 求解基于改进的A*算法的粗略轨迹，节点之间在动力学上是连续的；
- 2. 使用数值非线性优化提升轨迹质量；

### 1 Introduction

- 结合
- rapidly exploring random trees (RRTs) 的连续路径规划算法，但他们依赖高效的启发式引导：
  - Kavraki, L.; Svestka, P.; Latombe, J.-C.; and Overmars, M. 1996. Probabilistic roadmaps for path planning in high-dimensional configuration spaces. IEEE Transactions on Robotics and Automation 12(4).
  - LaValle, S. 1998. Rapidly-exploring random trees: A new tool for path planning.
  - Plaku, E.; Kavraki, L.; and Vardi, M. 2007. Discrete search leading continuous exploration for kinodynamic motion planning. In Robotics: Science and Systems.
- 步骤：
  - step 1：使用启发式搜索在连续的坐标空间中规划满足动力学的轨迹。但该方法缺少理论上的最优保证；
  - step 2：使用共轭梯度(conjugate gradient, CG)下降来局部改善解的质量，产生至少是局部最优的路径，但通常也达到全局最优。

### 2 Hybrid-State A* Search

- 本文的方法的第一阶段使用3D运动学状态空间的$$A*$$算法的变体，但修改了状态更新规则，该规则在$$A*$$的离散搜索节点中使用连续状态。
- 由于合并了同一网格中的连续坐标状态，因此不保证损失最小解（minimal-cost solution），但路径一定是可行的（drivable）。
- 搜索空间$$(x, y,\theta)$$，每个状态节点之间是动力学连续的
- ![搜索规则对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221011-1.png)
- 






































### 总结

