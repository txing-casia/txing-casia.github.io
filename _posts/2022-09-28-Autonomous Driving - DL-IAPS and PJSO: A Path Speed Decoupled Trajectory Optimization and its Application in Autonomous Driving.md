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
-  ![有地图和无地图对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220826-.png)

- 

### 总结

比较有想法的一个工作，做得比较细致，但是介绍相对粗略，可以仔细研究。
