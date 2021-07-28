---
layout:     post
title:      "Reinforcement Learning | Intrinsically Motivated Reinforcement Learning: An Evolutionary Perspective"
subtitle:   ""
date:       2021-07-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
    - Reward Shaping
---

# Intrinsically Motivated Reinforcement Learning: An Evolutionary Perspective

论文链接：https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5471106

## 主要内容

- intrinsically motivated 这个词最开始是在1950年的文章中提出的，文章认为需要有一种内在的操纵驱动力来解释为什么猴子会在没有任何外在奖励的情况下精力充沛地持续工作数小时来解决复杂的机械难题。内在动机行为对智力增长至关重要

- 本文解决了如何在人工系统中实现类似于内在动机的过程的问题，特别关注可能会或可能不会区分内在动机和外在动机的因素，其中外在动机是指由行为的特定奖励结果产生的动机，而不是行为本身。

- 相关工作：

  - 定义有趣度：Lenat's AM system [18], for example, focused on heuristic definitions of “interestingness,” 

  - 建立好奇心框架：Schmidhuber [32] [33] [34] [35] [36] [37] introduced methods for implementing forms of curiosity using the framework of computational reinforcement learning (RL)1 [47].

  - 奖励函数设计：Other researchers have reported interesting results of computational experiments involving evolutionary search for RL reward functions [1], [8], [19], [31], [43], but they did not directly address the motivational issues on which we focus.

  - 寻找本质奖励：Uchibe and Doya [51] do address intrinsic reward in an evolutionary context, but their aim and approach differ significantly from ours.

    [51]. E. Uchibe and K. Doya, "Finding intrinsic rewards by embodied evolution and constrained reinforcement learning", *Neural Netw.*, vol. 21, no. 10, pp. 1447-1455, 2008.

  - 与我们最接近的研究是 Elfwing 等人的研究。[11]其中使用遗传算法来搜索塑造奖励[23]和其他提高 RL 学习系统性能的学习算法参数。

    [11]. S. Elfwing, E. Uchibe, K. Doya and H. I. Christensen, "Co-evolution of shaping rewards and meta-parameters in reinforcement learning", *Adapt. Behav.*, vol. 16, pp. 400-412, 2008.

- 使用RL的模型结构示意图

![Agent–environment interactions in reinforcement learning](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20210722-1.png)

- 这篇文章[3]使用属于内部奖励（intrinsic reward），并将其与强化学习框架结合。所谓内部奖励，就是内部动机生成的奖励函数；外部奖励就是常规的强化学习框架的奖励函数。

  [3]. A. G. Barto, S. Singh and N. Chentanez, "Intrinsically motivated learning of hierarchical collections of skills", *Proc. Int. Conf. Develop. Learn.*, 2004.

- 目前的方法基本都是构建特殊的内部奖励函数，并且将其与一般的强化学习算法结合。例如基于好奇心的内部驱动方法[32]。

  [32]. J. Schmidhuber, *Adaptive Confidence and Adaptive Curiosity*, 1991.

- 这种内部动机的RL框架更加符合生物实际。并且奖励和奖励信号是不同的概念，RL模型中使用的更类似于奖励信号。

  - 奖励（Reward）指的是获得的好处；

  - 奖励信号（Reward signal）指的是大脑中奖励神经元发放的信号；

    Schultz [38], [39] writes that “Rewards are objects or events that make us come back for more,” whereas reward signals are produced by reward neurons in the brain.

- RL中的环境应该分为an external environment 和 an internal environment.

- 但是并不能从行为上区分是否使用了内部奖励[25]。但是内部动机是通过外部奖励形成的。

  [25]. P.-Y. Oudeyer and F. Kaplan, "What is intrinsic motivation? A typology of computational approaches", *Frontiers Neurorobot.*, 2007.

- Primary and Secondary Reward：使用内部动机意义在于作为一种次要的奖励信号，配合主要的外部奖励信号完成行为

- 生物动机心理学依据：

  Among the most influential theories of motivation in psychology is the drive theory of Hull [13] [14] [15]. According to Hull's theory, all behavior is motivated either by an organism's survival and reproductive needs giving rise to primary drives (such as hunger, thirst, sex, and the avoidance of pain), or by derivative drives that have acquired their motivational significance through learning. Primary drives are the result of physiological deficits—“tissue needs”— and they energize behavior whose result is to reduce the deficit. A key additional feature of Hull's theory is that a need reduction, and hence a drive reduction, acts as a primary reinforcer for learning: behavior that reduces a primary drive is reinforced. Additionally, through the process of secondary reinforcement in which a neutral stimulus is paired with a primary reinforcer, the formerly neutral stimulus becomes a secondary reinforcer, i.e., acquires the reinforcing power of the primary reinforcer. In this way, stimuli that predict primary reward, i.e., predict a reduction in a primary drive, become rewarding themselves. According to this influential theory (in its several variants), all behavior is energized and directed by its relevance to primal drives, either directly or as the result of learning through secondary reinforcement.

- 一些驱动力对行为影响的例子，在某些条件下，饥饿的老鼠宁愿探索不熟悉的空间，也不愿进食；他们忍受穿越电网的痛苦，去探索新的空间。

- 但是饥饿、口渴、繁衍这些驱动都伴随着满足的状态，他们是否能作为次要驱动还是有待研究。下面介绍基于进化的观点。

- 使用一个适应度函数和一些环境兴趣的分布（an explicit fitness function and some distribution of environments of interest），这个适应度可以是累积的外部奖励和等形式。



- **实验1：**
- 一个6x6的格子空间中分割了4个3x3的子空间，格子之间的墙壁不是全部封闭的，其中2个子格子空间中分别有一个打开的盒子和一个封闭的盒子（盒子位置在智能体生命周期内是不再变化的），一个打开的盒子在每个时间步以 0.1 的概率关闭，密闭的盒子里总是装着食物。智能体在这样的格子迷宫中要找到打开的盒子，并在封闭的盒子中寻找食物。智能体的行动分为上下左右4个方向。
- 当代理食用食物时，它会在一个时间步长内感到饱足。代理在所有其他时间步都饥饿。智能体每吃一次食物，它的适应度就会增加 1

- 在格子迷宫环境中假设了两只钟情况：
  1. constant condition: 食物总是智能体10000步生命周期中，在封闭的盒子中出现；
  2. step condition: 智能体的生命周期是20000步，食物总是出现在10000步之后出现在封闭盒子中；

- 符号说明：
  - agent $$\mathcal{A}$$
  - a space of reward function $$R_\mathcal{A}$$
  - a specific reward function $$r_\mathcal{A} \in R_\mathcal{A}$$
  - a sampled environment $$E \sim P(\varepsilon)$$
  - history of agent $$\mathcal{A}$$  adapting to environment $$E$$ over its lifetime using the reward function $$r_\mathcal{A}$$, i.e., $$h \sim \langle \mathcal{A}(r_\mathcal{A}),E\rangle$$ 
  - fitness function $$\mathcal{F}$$ produces a scalar evaluation $$\mathcal{F}(h)$$ for each history $$h$$
  - optimal reward function $$r^*_{\mathcal{A}} \in R_{\mathcal{A}}$$  

$$
r^*_{\mathcal{A}}=\arg \max_{r_\mathcal{A} \in R_\mathcal{A}} \mathbb{E}_{E\sim P(\varepsilon)} \mathbb{E}_{h \sim \langle \mathcal{A}(r_\mathcal{A}),E\rangle}[\mathcal{F}(h)]
$$

- 使用的算法 the lookup-table -greedy Q-learning [52].

  [52]. C. J. C. H. Watkins, “Learning from Delayed Rewards,” Ph.D. dissertation, Cambridge Univ., Cambridge, U.K., 1989

$$
Q_{t+1}(s_t,a_t)=(1-\alpha)Q_t(s_t,a_t)+\alpha[r_t + \gamma \max_b (Q_t(s_{t+1},b))]
$$

之后通过实验验证了$$r*$$的奖励函数趋近最优奖励函数



- **实验2**

  - 一个三走廊的觅食环境，虫子每次随机出现正在其中一个走廊的尽头。由于每次虫子的位置是随机且未知的，智能体过去的经验不能直接直到新的觅食，所以这个环境是非马尔科夫的。
  - 实验要求智能体在固定的10000步之内尽可能多地吃到虫子。

  $$
  Q_d(s,a)=r_{\mathcal{A}}(s,a)+\gamma\sum_{s'\in \mathcal{S}} \hat{T}(s'|s,a)\max_{a'} Q_{d-1}(s',a')\\
  \hat{T}(s'|s,a)=\frac{n_{s,a,s'}}{n_{s,a}}
  $$

  其中 $$n_{s,a,s'}$$ 表示在 $$s$$ 状态下，选择 $$a$$ 后转移到 $$s'$$ 的次数。 $$n_{s,a}$$ 表示在 $$s$$ 状态下，选择 $$a$$ 的次数。

- 奖励函数：$$r_{\mathcal{A}}(s,a)=\beta_F\varPhi_F(s) + \beta_c\varPhi_c(s,a,h)$$ ，其中 $$\beta_F$$ 和 $$\beta_c$$ 是权重系数。特征 $$\varPhi_F(s)$$ 在智能体饱的时候是 1 ，其他时候是  0 。特征 $$\varPhi_c(s,a,h) = 1-\frac{1}{c(s,a,h)}$$ ，其中 $$c(s,a,h)$$  是智能体在历史 $$h$$ 中执行行为的时间步数。
- 当参数 $$\beta_c$$ 为正时，智能体会因为最近没有从当前状态采取的行动而获得奖励。这种奖励不是外部环境的固定函数. feature $$\varPhi_F(s)$$ is a hunger-status feature, and thus when $$\beta_F=1$$ and $$\beta_c=0$$, the reward function is the fitness-based reward function

最终基于适应度的奖励函数表现次于最优奖励函数2个数量级。因此利用内在奖励 $$\varPhi_c(s,a,h)$$ 对提升探索能力很有意义。



## 总结

视角很独特，实验很新奇。

但是文章发表在自主心理发展IEEE TRANSACTIONS ON AUTONOMOUS MENTAL DEVELOPMENT，因此作为一篇概念文章比较好。

