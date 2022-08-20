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
- extrapolation error：未见过的状态-动作对被错误地估计为具有不现实的价值的现象。

- 引入了batch-constrained reinforcement learning来克服extrapolation error，最大化奖励的同时，最小化批量数据中状态-行为对和策略访问的状态行为对的误匹配。
- 本文提出算法：**Batch-Constrained deep Q-learning** (BCQ)，利用一个与Q-network结合的条件状态生成模型，只生成过去见过的行为。在较弱的假设下，证明了这种批约束范式对于有限确定MDP的不完全数据集的无偏估计是必要的。
- 代码开源：https://github.com/sfujim/BCQ

### 2 Background

- 贝尔曼算子（Bellman operator）$$\Tau^{\pi}$$：
  $$
  \Tau^{\pi}Q(s,a)=\mathbb{E}_{s'}[r+\gamma Q(s',\pi(s'))]
  $$

### 3 Extrapolation Error (外推误差)

- Extrapolation error is an error in off-policy value learning which is introduced by the mismatch between the dataset and true state-action visitation of the current policy，或者$$(s,a)$$在数据集中不存在

- 外推误差的成因：

  - **数据缺省**（Absent Data）：state-action pair (s, a) is unavailable，因此在估计状态-行为价值Q的时候会引入误差。

  - **模型偏置**（Model Bias）：when performing off-policy Q-learning with a batch B，状态转移动力学的有偏估计为：

    $$\Tau^{\pi}Q(s,a) \approx \mathbb{E}_{s'\sim B}[r+\gamma Q(s',\pi(s'))]$$ 其中，状态转移依据buffer B，而不是真实的MDP。

  - **训练误匹配**（Training Mismatch）：当数据的分布和当前策略的分布不匹配，对action的估计会有误差

- As a result, learning a value estimate with off-policy data can result in large amounts of extrapolation
  error if the policy selects actions which are not similar to the data found in the batch.

#### 3.1 Extrapolation Error in Deep Reinforcement Learning














![Levels of multi-agent learning in autonomous driving](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220817-.png)



### 总结

