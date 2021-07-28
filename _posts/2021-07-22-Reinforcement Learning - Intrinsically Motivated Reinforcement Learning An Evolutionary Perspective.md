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

- 在格子迷宫环境中假设了两只钟情况：
  1. constant condition: 食物总是智能体10000步生命周期中，在封闭的盒子中出现；
  2. step condition: 智能体的生命周期是20000步，食物总是出现在10000步之后出现在封闭盒子中；



- **实验2**
  - 一个三走廊的觅食环境，虫子每次随机出现正在其中一个走廊的尽头。由于每次虫子的位置是随机且未知的，智能体过去的经验不能直接直到新的觅食，所以这个环境是非马尔科夫的。





## 总结



