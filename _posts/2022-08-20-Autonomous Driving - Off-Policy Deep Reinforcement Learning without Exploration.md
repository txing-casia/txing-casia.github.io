---
layout:     post
title:      "Autonomous Driving | Off-Policy Deep Reinforcement Learning without Exploration"
subtitle:   ""
date:       2022-08-20
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## Off-Policy Deep Reinforcement Learning without Exploration (2019)

类似DQN和DDPG的off-policy RL算法在被禁止探索，并在没有数据策略分布修正的的情况下，难以取得好的效果。本文通过限制off-policy agent的行为空间，使其行为类似与on-policy算法，最后提出了一个较为通用的，针对连续控制的deep reinforcement learning algorithm。

### 1 Introduction

- **batch reinforcement learning**：在一些数据收集面临costly, risky, or time-consuming的情境时，智能体只能从固定的数据集中学习策略，并且其对数据质量的要求较低。
- 当数据集中的分布与当前算法策略的分布不同时，标准的off-policy RL算法将会失败。















![Levels of multi-agent learning in autonomous driving](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220817-.png)



### 总结

