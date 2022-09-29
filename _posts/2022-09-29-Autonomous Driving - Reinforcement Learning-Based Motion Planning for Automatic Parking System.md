---
layout:     post
title:      "Autonomous Driving | Reinforcement Learning-Based Motion Planning for Automatic Parking System"
subtitle:   ""
date:       2022-09-29
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Autonomous Driving
    - Reinforcement Learning
---

## Reinforcement Learning-Based Motion Planning for Automatic Parking System

### 1 Introduction

- 典型的自动泊车系统包括以下组件：
  - 停车位检测 parking slot detection, 
  - 运动规划 motion planning (or path planning and tracking),
  - 自我车辆的姿态估计 ego-vehicle’s posture estimation, 
  - 底盘控制 chassis control,
- 自动泊车算法分类：
  - 基于专家驾驶员泊车数据的方法 expert drivers’ parking data-based method,
  - 基于人类先验知识的方法 human prior knowledge-based method,
  - 基于强化学习的方法 reinforcement learning-based method.

#### 1) 基于专家驾驶员泊车数据的方法

- 使用监督学习训练神经网络，训练数据需要尽可能覆盖实际使用场景（图像输入）
- 局限：
  - 专家数据是模型的上限；
  - 专家数据难获得；
  - 数据质量要求高；

#### 2) 基于先验知识的方法

- abstracting human parking experience into prior knowledge and then using it to guide the planning.

- 几何方法：
  - Reeds-Shepp (RS) curve [6], [7],
  - clothoid curve [8], [9], 
  - Bezier curve [10], 
  - spline curve [11],
  - polynomial curve [12]
- 启发式方法：
  - A* [13]
- 模糊逻辑方法：
  - fuzzy rules [14], [15]

#### 3) 基于强化学习的方法














![非线性路径平滑优化问题](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-.jpg)


### 总结

