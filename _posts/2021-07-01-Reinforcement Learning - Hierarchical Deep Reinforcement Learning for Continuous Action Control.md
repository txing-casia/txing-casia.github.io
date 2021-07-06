---
layout:     post
title:      "Reinforcement Learning | Hierarchical Deep Reinforcement Learning for Continuous Action Control"
subtitle:   "H-DDPG"
date:       2021-07-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Hierarchical Deep Reinforcement Learning for Continuous Action Control

论文链接：https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8310962&tag=1

## 背景

机器人连续行为空间的控制是一个难题，尤其是在应对复合任务的时候。

-  In addition, many works introduced linear function approximations to enhance the generalization ability of algorithms [5], [6] to handle more complex environments.
- Moreover, learning agents also need to consider many physical factors to keep the robot moving smoothly



## 主要内容

- hierarchical-deep deterministic policy gradient (h-DDPG)

- 第一层使用多个actor网络学习基本技能，第二层学习复合的技能。
- Results show that the proposed algorithm can successfully learn high performance basic skills and compound skills simultaneously. Results also show that its performance in solving compound tasks in the three scenarios outperform discrete action deep reinforcement learning algorithm.

- Contributions:
  - 1) learn multiple basic skills at the same time; 
  - 2) learn compound skills to solve compound tasks by reusing basic skills; 
  - 3) handle the above two kinds of skill learning within the same process and time scale.
- 一些方法：
  - deterministic policy gradients [18], [26], [27] 
  - stochastic policy gradients [28], [29]
  -  trust region policy optimization [30]
  - Some work has also been done to integrate modelbased methods to accelerate learning in continuous action spaces [31]
- 分层强化学习：
  - Various works on hierarchical reinforcement learning exist [33], [34]
  -  including work considering deep architectures [35], [36]
  - Work in **[36]** is focuses on finding the best hierarchical structure of the tasks with clustering methods, and is mainly concerned with how to decompose tasks
- Most recent work on deep hierarchical reinforcement learning can be found in [37] and [38].
- 针对图像输入的DDPG，更改了conv网络的激活函数，思想来自the mlpconv layer。可以减少网络参数数量
- 基本能力的定义：Basic tasks are fundamentally nontask specific and rather are tied to the physical capabilities of the robot, such as rotating a wheel or bending a joint to a specific direction.
- 符合任务定义：Conversely, compound tasks are defined to be tasks that can only be achieved by combining different patterns.




## 总结

这篇文章讲述并不是很清晰，也缺少理论证明，感觉最大的贡献就是使用了多个actor网络学习基础技能，但这个也是比较典型的分层强化学习思路，和option类似。


