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
- 低级策略储存经验$$(s_t,g_t,a_t,r_t,s_{t+1},h(s_t,g_t,s_{s+1}))$$

- 高级策略储存经验$$(s_{t:t+c-1},g_{t:t+c-1},a_{t:t+c-1},R_{t:t+c-1},s_{t+c})$$

### 参数化Rewards

- goal transition model $$h$$ is defined as: $$h(s_t,g_t,s_{t+1}) = s_t + g_t - s_{t+1}$$ 
-  intrinsic reward: $$ r(s_t,g_t,a_t,s_{t+1}) = -||s_t + g_t - s_{t+1}||_2 $$ ，这里$$g_t$$相当于是一个状态增量，学习目标是使得 $$ s_t + g_t = s_{t+1} $$ 
-  实验中$$g_t$$是期望的坐标$$(x,y,z)$$，obs也仅仅包含位置的观测

- 高级策略state-action-reward transition $$(s_t,g_t,\sum R_{t:t+c-1},s_{t+c})$$
- 对于旧的经验，给定$$\widetilde{g}_t$$最大化$$\mu^{lo}(a_{t:t+c-1}|s_{t:t+c-1},\widetilde{g}_{t:t+c-1})$$ 
- 中间目标 $$\widetilde{g}_{t+1:t+c-1}$$ 用固定目标转移函数$$h$$生成
-  log probability $$ \log \mu^{lo}(a_{t:t+c-1}|s_{t:t+c-1},\widetilde{g}_{t:t+c-1}) $$ 通过下式计算得到 

$$
\log \mu^{lo}(a_{t:t+c-1}|s_{t:t+c-1},\widetilde{g}_{t:t+c-1})\propto -\frac{1}{2}\sum_{i=t}^{t+c-1}||a_i-\mu^{lo}(s_i,\widetilde{g}_i)||^2_2+\text{const}
$$


## 总结

HIRO整体节奏还是很明快的，整体结构基于DDPG和TD3经验修正的地方稍微有些含糊。文中的目标 $$ g_t $$ 就相当于是一个状态增量，修正就是用10个高斯分布在 $$ [0,|s_{t+1}-s_t|] $$ 或者 $$ [-|s_{t+1}-s_t|,0] $$ 之间取值目标 $$ \widetilde{g}_t $$ 。至于为什么是10个高斯，怎么取值，这些正文中没有说明，附录里面应该有，细看的时候可以找一找。







