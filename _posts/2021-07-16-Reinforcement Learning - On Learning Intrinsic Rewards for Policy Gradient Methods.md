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

 








![错误图片](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210714-2.png)



## 总结

