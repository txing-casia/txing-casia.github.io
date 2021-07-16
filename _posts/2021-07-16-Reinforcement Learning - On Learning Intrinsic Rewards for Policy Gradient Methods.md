---
layout:     post
title:      "Reinforcement Learning | On Learning Intrinsic Rewards for Policy Gradient Methods"
subtitle:   ""
date:       2021-07-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# On Learning Intrinsic Rewards for Policy Gradient Methods

论文链接：https://dl.acm.org/doi/pdf/10.5555/3327345.3327375

## 主要内容

- 本文提出基于 **optimal reward framework** [Singh et al., 2010] 的本质奖励学习方法，并和 PPO 算法（Mujoco）结合

Satinder Singh, Richard L Lewis, Andrew G Barto, and Jonathan Sorg. Intrinsically motivated reinforcement learning: An evolutionary perspective. IEEE Transactions on Autonomous Mental Development, 2(2):70–82, 2010.

- 在一些游戏场景中缺少清晰的奖励函数选择方式。有时候存在多个目标，最小化能量消耗，最大化吞吐量，最小化延时等（minimizing energy consumption and maximizing throughput and minimizing latency）
- 早期的工作证明了最优策略的奖励函数并不唯一，例如使用一个势能函数（potential-based reward [Ng et al., 1999]）的奖励函数不会改变最优策略。

Andrew Y Ng, Daishi Harada, and Stuart J Russell. Policy invariance under reward transformations: Theory and application to reward shaping. In Proceedings of the Sixteenth International Conference on Machine Learning, pages 278–287. Morgan Kaufmann Publishers Inc., 1999.

- 另一方面，由于实际任务中的各种限制（e.g., inadequate memory, representational capacity, computation, training data, etc.），智能体很难学习到最优策略。因此，在解决奖励设计问题时，希望设计能改变最优策略的奖励函数变换方式。

- 构造奖励函数的好处：
  - 比原始的奖励函数更少的稀疏性，加快学习过程 [Rajeswaran et al., 2017]；
  - 帮助探索状态边界的区域，例如count-based reward 鼓励探索很少探索到的区域 [Bellemare et al., 2016, Ostrovski et al., 2017, Tang et al., 2017]；

- 相关的方法：
  - preference elicitation
  - inverse RL
  - intrinsically motivated RL
  - optimal rewards
  - potentialbased shaping rewards
  - more general reward shaping
  - mechanism design
- 主要贡献：
  - 基于策略梯度的方法推导intrinsic rewards学习方法，并与 task-specifying (hereafter extrinsic) reward 相结合，最大化外部奖励。
  - 内部和外部奖励之和的参数通过策略梯度方法更新训练

- 使用本质奖励的主要原因是对RL agent的训练都是有限的（表征能力，可得到的数据，计算能力限制等等），通过集合内部奖励可以环节这些限制。The main intuition is that in practice all RL agents are bounded (computationally, representationally, in terms of data availability, etc.) and the optimal intrinsic reward can help mitigate these bounds.






![错误图片](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210714-2.png)



## 总结

