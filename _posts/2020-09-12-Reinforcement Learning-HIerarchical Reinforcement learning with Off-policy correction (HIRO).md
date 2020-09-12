---
layout:     post
title:      "Reinforcement Learning | HIerarchical Reinforcement learning with Off-policy correction (HIRO)"
subtitle:   ""
date:       2020-09-13
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Data-Efficient Hierarchical Reinforcement Learning  

论文链接：http://papers.nips.cc/paper/7591-data-efficient-hierarchical-reinforcement-learning.pdf

## 背景

HRL是处理复杂任务的有效方式，之前的方法大多需要针对具体任务进行特殊设计，并使用on-policy的方法进行训练，这些因素导致HRL很难被引用到真实的场景中。本文用off-policy的方式训练higher- and lower-level controllers，提高了模型的数据利用率，性能超过了之前SOTA的Option-Critic。

**Option-Critic**：Pierre-Luc Bacon, Jean Harb, and Doina Precup. The option-critic architecture. In AAAI, pages 1726–1734, 2017.  

## 主要工作

- 首先需要明确的是什么是复杂的任务，文中定义的一个复杂任务是perform exploratory navigaive as well as complex sequences of interaction with ovjects inthe environment. 作者认为这些任务是unsolvable by non-HRL，主要是因为奖励稀疏，完成任务需要多个步骤；

- 低级/高级策略如何定义并训练？之前的方法用人工设计来解决，因此方法没有一般性。

- Note that 本文只用的是state observation而不是高维图像输入，这个与FuN是不同的

- 本文使用的是off-policy的学习方法（TD3, [[Addressing Function Approximation Error in Actor-Critic Methods](https://link.zhihu.com/?target=http%3A//proceedings.mlr.press/v80/fujimoto18a/fujimoto18a.pdf)，它是DDPG的一种变体，同样针对连续的控制），试图提高sample的利用效率，但是这会带来两个问题：

  - off-policy会带来学习上的不稳定，在多重policy的情况下这种影响还会更大。
  - 低级的policy在训练过程中会不断变化，因此之前收集到的samples就不再是valid experience for training

  针对这两个问题，本文建立了**off-policy修正模型**，re-label过去的经验，并用高级策略选择可能获得最大奖励的行为。

## 方法

![](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20200912-1.png)



- 两层策略：a lower-level policy $$\mu^{lo}$$ and a higher-level policy $$\mu^{hi}$$ 

- $$\mu^{hi}$$以从环境获取的obs为输入，获得high-level action (goal) $$g_t\in \mathbb{R}^d$$。$$g_t$$要么用$$\mu^{hi}$$以$$c$$步为间隔进行采样$$g_t\sim \mu^{hi}\  (when\ t\equiv 0)$$，要么使用一个固定目标的转换函数$$g_t=h(s_{t-1},g_{t-1},s_t)$$，获得的奖励为$$R(s_t,a_t)$$。

- $$\mu^{lo}$$观测状态$$s_t$$和目标$$ g_t$$，并产生低维的行为$$a_t\sim \mu^{lo}(s_t, g_t)$$，获得奖励$$r_t=r(s_t,g_t,a_t,s_{t+1})$$
- 储存经验$$(s_t,g_t,a_t,r_t,s_{t+1},h(s_t,g_t,s_{s+1}))$$







## 总结

文章种在多个游戏中进行了实验对比，均取得了良好的实验效果。但遗憾的是，几乎所有的复现都说性能达不到文章中的水平。可能是实验中还有一些优化并未在文章中说明。







