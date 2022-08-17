---
layout:     post
title:      "Autonomous Driving | SMARTS Scalable Multi-Agent Reinforcement Learning Training School for Autonomous Driving (Huawei)"
subtitle:   ""
date:       2022-08-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## SMARTS: Scalable Multi-Agent Reinforcement Learning Training School for Autonomous Driving

 顾名思义，SMARTS是针对多智能体算法的自动驾驶强化学习仿真平台。

开源了基准任务和代码：https://github.com/huawei-noah/SMARTS

### 1 Introduction

- 自动驾驶仿真的挑战之一是天气问题；绝大部分数据是在好天气（fair weather）下采集的；当前的L4自动驾驶面对复杂的交互情况时。倾向于减速等待，而不是提前主动找办法通过；
-  Waymo的California2018的自动驾驶事故数据中, 57%是发生了追尾（rear endings），29%是发生了侧面碰撞（sideswipes），并且事故都是由于他车造成的，因此说明过于保守的驾驶策略
- waymo的汽车相比人类驾驶员经常过分刹车，导致乘客晕车

- 多智能体交互的分级标准：“multi-agent learning levels”, or “M-levels“

- double merge道路场景（即>--<形道路）是多智能体交互的难点，车辆需考虑是继续走还是等待；在间隙不够大的时候是否需要变道；其他车开到了自车车道上，是否和它交换位置？等

- 平台设计目标：

  - Bootstrapping Realistic Interaction

    - (1) physics,

    - (2) behavior of road users, 

    - (3) road structure & regula-tions, 

    - (4) background traffic flow.

  - Heterogeneous Agent Computing（异构智能体计算）

  - Simulation Providers

  - Interaction Scenarios

  - Distributed Computing

- Key Features：

![Levels of multi-agent learning in autonomous driving](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220817-1.png)

- 场景：

![Levels of multi-agent learning in autonomous driving](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220817-2.png)

- **Observation.** The observation is a stack of three consecutive frames, which covers the dynamic objects and key events. For each frame, it contains: 1) relative position of goal; 2) distance to the center of lane; 3) speed; 4) steering; 5) a list of heading errors; 6) at most eight neighboring vehicles’ driving states (relative distance, speed and position); 7) a bird’s-eye view gray-scale image with the agent at the center.

- **Action.** The action used here is a four-dimensional vector of discrete values, for longitudinal control—keep lane and slow down—and lateral control—turn right and turn left. 

- **Reward.** The reward is a weighted sum of the reward components shaped according to ego vehicle states, interactions involving surrounding vehicles, and key traffic events. More details can be found in our implementation code.

















![模型结构](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220812-3.png)











### 总结

