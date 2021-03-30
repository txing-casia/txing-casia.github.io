---
layout:     post
title:      "Reinforcement Learning | Sim-to-Real Transfer in Deep Reinforcement Learning for Robotics a Survey"
subtitle:   "机器人领域sim-to-real方向的迁移学习综述"
date:       2021-03-30
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - Transfer Learning
---

# Sim-to-Real Transfer in Deep Reinforcement Learning for Robotics: a Survey

论文链接：https://ieeexplore.ieee.org/document/9308468

## 背景

- 真实的机器人数据难以获得，或者成本代价高，因此需要用仿真环境模拟，降低成本，保证安全性。Owing to the limitations of gathering real-world data, i.e., sample inefficiency and the cost of collecting it, simulation environments are utilized for training the different agents
- 问题是仿真和真实之间存在差距。Nonetheless, **the gap between the simulated and real worlds** degrades the performance of the policies once the models are transferred into real robots

- 主要研究方向：
  - 域随机化 domain randomization
  - 域适应 domain adaptation
  - 模仿学习 imitation learning
  - 元学习 meta-learning
  - 知识蒸馏 knowledge distillation

- Moreover, learning with real robots requires the consideration of **potentially dangerous** or **unexpected behaviors in safety-critical applications** [4]

- **关键问题**：如何通过**转移知识**并相应地**调整策略**，在真实环境中利用基于模拟的训练。how to exploit simulation-based training in real-world settings by transferring the knowledge and adapting the policies accordingly

- 仿真的设置与真实设置之间有着固有的不匹配。Simulation-based training provides data at low-cost, but involves **inherent mismatches** with real-world settings

- 一些基于深度学习的相关算法：

  - adversarial attacks on computer vision algorithms [7].
  - introduce perturbances in the environment [8]
  - domain randomization [9]
  - Another key aspect to take into account is that an agent deployed in the real world will potentially be exposed to novel experiences that were not present in the simulations [10]
  - meta learning [11]
  - continual learning [12]
  -  更好的仿真引擎 physics engines: Airsim [13], CARLA [14], RotorS [15], [16], and others [17]

- 安全的强化学习这也是一个方向 safe reinforcement learning [4]

- 几个方向的关系如下图所示：

https://gitee.com/txing-z/txing-casia.github.io/blob/master/img/20210330-1.png

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210330-1.png)

### A. Deep Reinforcement Learning

- 略

### B. Sim-to-Real Transfer

- Transferring DRL policies form simulation environments to reality is a necessary step towards more complex robotic systems that have DL-defined controllers.

### C. Transfer Learning and Domain Adaptation

- **迁移学习**：Transfer learning aims at improving the performance of target learners on **target domains** by transferring the knowledge contained in different but related **source domains** [18]. In this way, transfer learning can reduce the dependence of target domain data when constructing target learners.
- **域适应**：它指定了当我们有足够的源域标记数据和与目标任务相同的单个任务，但没有或很少目标域数据时的情况。It specifies the situation when we have sufficient source domain labeled data and the same single task as the target task, but without or very few target domain data. In sim-to-real robotics, researchers tend to employ a simulator to train the RL model and then deploy it in the realistic environment, where we should take advantage of the domain adaptation techniques in order to transfer the simulation based model well

### D. Knowledge Distillation

- 一般用于大型网络，例如使用视觉图像输入的深层网络
- 使用网络教另一个网络，这样使得两个网络性能差不多，但是学得更快。In these set-ups, the two networks are typically called teacher and student.

### E. Meta Reinforcement Learning

- 通常采用LSTM。MetaRL usually implements an LSTM policy and incorporates the last reward $$r_{t−1}$$ and last action $$a_{t−1}$$ into the current policy observation

### F. Robust RL and Imitation Learning

- Robust RL [23] was proposed quite early as a new RL paradigm that explicitly takes into account input disturbances as well as modeling errors
- It considers a bad, or even adversarial model and tries to maximize the reward as a optimization problem [24], [25].

- **Imitation learning** proposes to employ expert demonstration or trajectories instead of manually constructing a fixed reward function to train RL agents.
  - **behaviour cloning** where an agent learns a mapping from observations to actions given demonstrations [26], [27]
  - **inverse reinforcement learning** where an agent attempts to estimate a reward function that describes the given demonstrations [28].

- 机器人导航任务的方法：
  - curriculum learning [37]
  - incremental environment complexity [39]
  - continual learning and policy distillation for multiple tasks [12]


## 总结

即时限定在机器人领域，迁移学习也不是一个很具体的范畴。




