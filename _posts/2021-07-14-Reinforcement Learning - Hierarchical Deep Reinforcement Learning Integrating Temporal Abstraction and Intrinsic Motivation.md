---
layout:     post
title:      "Reinforcement Learning | Hierarchical Deep Reinforcement Learning: Integrating Temporal Abstraction and Intrinsic Motivation"
subtitle:   "H-DQN"
date:       2021-07-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Hierarchical Deep Reinforcement Learning: Integrating Temporal Abstraction and Intrinsic Motivation

论文链接：https://proceedings.neurips.cc/paper/2016/file/f442d33fa06832082290ad8544a8da27-Paper.pdf

## 主要内容

- 稀疏反馈下，不充分的探索使得agent很难学习到鲁棒的行为策略。本文通过设计本质动机来驱动行为探索，并结合分层的结构来完成行为，最后在奖励稀疏和延迟的任务中进行了实验。

- 结构分为两层，顶层的meta-controller学习定义子目标，底层的controller学习根据状态和子目标执行行动，直到目标完成或者情节结束。

- 这项工作是基于 option 框架完成的。相关的分层强化学习框架还有 MAXQ 等。这篇文章没有对每个option单独使用Q函数。这一点和 [21] 是一致的，这会带来两个好处：（1）不同选项之间存在共享学习；（2）该模型可扩展到大量options的情况；

  [21]. T. Schaul, D. Horgan, K. Gregor, and D. Silver. Universal value function approximators. In Proceedings of the 32nd International Conference on Machine Learning (ICML-15), pages 1312–1320, 2015.

-  In another paper, Singh et al. [26] take an evolutionary perspective to optimize over the space of reward functions for the agent, leading to a notion of extrinsically and intrinsically motivated behavior.

- Cognitive Science and Neuroscience The nature and origin of intrinsic goals in humans is a thorny issue but there are some notable insights from existing literature. There is converging evidence in developmental psychology that human infants, primates, children, and adults in diverse cultures base their core knowledge on certain cognitive systems including – entities, agents and their actions, numerical quantities, space, social-structures and intuitive theories [29]. During curiositydriven activities, toddlers use this knowledge to generate intrinsic goals such as building physically stable block structures. In order to accomplish these goals, toddlers seem to construct subgoals in the space of their core knowledge. Knowledge of space can also be utilized to learn a hierarchical decomposition of spatial environments. This has been explored in Neuroscience with the successor representation, which represents value functions in terms of the expected future state occupancy. Decomposition of the successor representation have shown to yield reasonable subgoals for spatial navigation problems [5, 30].

- MDP过程中的高效探索是一个难题。$$\epsilon-greedy$$ 在局部探索中表现还好，但是对于整个状态空间不同区域的探索比较失败。这里通过设计本质目标 $$g$$ 和本质奖励intrinsic rewards

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210714-1.png)

- agent设置：
  - meta-controller: 接收状态 $$s_t$$ 然后选择子目标 $$g_t$$.
  - controller: 根据 $$s_t$$ 和 $$g_t$$ 选择行为 $$a_t$$.

- 一个内部的critic用来为 controller 评估 $$r_t(g)$$ （这篇文章使用的是一个二值的奖励）。controller 的目标就是最大化累积的本质奖励intrinsic reward $$R_t(g)=\sum_{t'=t}^{\infty} \gamma^{t'-t}r_{t'}(g)$$

- meta-controller 的目标是最大化累积的外部奖励 extrinsic reward $$F_t=\sum_{t'=t}^{\infty} \gamma^{t'-t}f_{t'}$$，其中$$f_{t'}$$是从环境中收到的奖励信号，并且$$F_{t}$$和$$R_{t}$$的时间尺度是不同的。

- Controller:
  $$
  Q_1^*(s,a;g)=\max_{\pi_{a,g}} \mathbb{E} \Big[r_t+\gamma \max_{a_{t+1}}Q_1^*(s_{t+1},a_{t+1};g)\Big]
  $$
  
- meta-controller:
  $$
  Q_2^*(s,g)=\max_{\pi_{g}} \mathbb{E} \Big[\sum_{t'=t}^{t+N} f_{t'}+\gamma \max_{g'}Q_2^*(s_{t+N},g')\Big]
  $$
  N 代表当控制器停止时所经过的步数。

![算法步骤](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210714-2.png)



## 总结

很简单直接的思路，直接在DQN上做出了分层的改进，但是感觉和option的结合并不是很紧密，比如终止函数没有单独设计，时间缩放步数N可能需要预设等等。

