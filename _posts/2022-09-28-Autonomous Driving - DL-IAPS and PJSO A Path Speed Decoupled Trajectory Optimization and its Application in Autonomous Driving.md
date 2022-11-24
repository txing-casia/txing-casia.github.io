---
layout:     post
title:      "Autonomous Driving | DL-IAPS and PJSO: A Path/Speed Decoupled Trajectory Optimization and its Application in Autonomous Driving (Baidu, 2020)"
subtitle:   ""
date:       2022-09-28
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Autonomous Driving
---

## DL-IAPS and PJSO: A Path/Speed Decoupled Trajectory Optimization and its Application in Autonomous Driving

本文是以下三者的结合：

- collision-free trajectory planning problem
- Dual-Loop Iterative Anchoring Path Smoothing (DL-IAPS)
- Piece-wise Jerk Speed Optimization (PJSO)

### 1 Introduction

- 一些环境不支持车辆倒车，但在parking上倒车是必要的，因此需要自由空间轨迹规划算法 Free space trajectory planning algorithm
- 当前主流的自由空间轨迹规划方法有两类：
  - **路径-速度联合优化方法**（path/speed coupled method）: Nonlinear Model Predictive Control (NMPC), Mixed Interger Programming (MIP), Hierarchical Optimization-Based Collision Avoidance (H-OBCA)。这些方法可以优雅地将车辆动力学和避障结合成一个单一的优化问题，但具有高计算复杂度和低鲁棒性；
  - **路径-速度解耦优化方法**（path/speed decoupled method）: 先计算平滑路径后计算轨迹轮廓。该方式具有更高的计算效率，但可能不具有控制可行性，在极端情况下无法保证路径或速度的平滑性。
- **Hybrid A* method: ** “Path planning for autonomous vehicles in unknown semi-structured environments,” The International Journal of Robotics Research, vol. 29, no. 5, pp. 485–501, 2010.

- 迭代曲率约束路径平滑 iterative curvature constrained path smoothing (i.e., DL-IAPS)

- 舒适的最小时间分段加加速度优化 comfortable minimum-time piece-wise jerk speed optimization (i.e., PJSO)
- 主要贡献：
  - 精确实时的碰撞避免轨迹生成，Precise Collision Avoidance in Real-Time，轨迹长度9-14s，检测时间0.18−0.21s
  - 具备可行性的控制，Control Feasibility
  - 兼顾驾驶舒适性和最短行驶时间，Driving comfort and Minimum Traversal Time

### 2 Problem Statement

- 模型包括三部分：
  - Region of Interest (RoI)：感兴趣区域
  - Trajectory Generation：
  - Trajectory Post-processing

### 3 Path speed decoupled trajectory optimization

#### Inner Loop for Curvature Constrained Path Smoothing:

- the nonlinear path smoothing optimization problem is formulated in as:

![非线性路径平滑优化问题](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-1.jpg)

#### Outer Loop for Collision Avoidance:

- collision check and $$B_k$$ updates

![碰撞检测](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-2.jpg)

#### Piece-wise Jerk Speed Optimization

![Jerk形式推导](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-3.jpg)

- 二次规划优化问题：

![二次规划优化问题](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-4.jpg)

- 侧方位停车对比：

  ![Optimized path trajectory from starting pose $$[x = -6m, y = 2.5m, \theta = 0.0]$$ with Hybrid A* path generator and DL-IAPS smoothing](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-5.jpg)

- parking 系列实验：

![Various simulation test cases with multiple numbers of boundaries and obstacles](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220929-6.jpg)



### 总结

整体而言就是在混合A*的基础上，加上了轨迹平滑、碰撞检测、速度规划三个后续模块，在parking任务上取得了一定效果，并集成与Apollo平台中
