---
layout:     post
title:      "Reinforcement Driving | Robust Reinforcement Learning using Offline Data"
subtitle:   "Robust Fitted Q-Iteration (RFQI)"
date:       2022-11-25
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

## Robust Reinforcement Learning using Offline Data

- 鲁邦强化学习可以形式化为一个最大值最小化问题，目标是学习最大化价值的策略，而不是不确定性集合中最差的可能模型。本文提出一个Robust Fitted Q-Iteration (RFQI)算法来应对离线数据收集、模型优化、无偏估计问题。

### 1 Introduction

- 处理RL的实际应用问题：
  - online RL
  - pre-collected offline dataset
- 仿真环境的典型误差：
  - sensor noise
  - action delay
  - friction
  - mass of a mobile robot
- RL算法可能因为环境的细微变化而失效，为了应对这样的问题，因此有了robust RL，旨在处理训练和测试中的环境参数误匹配问题；
- Robust Markov Decision Process (RMDP)：标准MDP只考虑单个模型（转移概率函数），RMDP考虑整个不确定性模型集合（uncertainty set），训练模型，以获得能够在不确定性集中最坏的模型上表现得最好的最优鲁棒策略

#### Offline RL











- ![Planning, acting and training with a learned model](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221020-11.jpg)









- 

  
  
- 




### 总结



​	
