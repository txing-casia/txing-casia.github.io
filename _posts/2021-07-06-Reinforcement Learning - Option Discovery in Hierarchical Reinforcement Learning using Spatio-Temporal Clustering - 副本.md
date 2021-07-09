---
layout:     post
title:      "Reinforcement Learning | Option Discovery in Hierarchical Reinforcement Learning using Spatio-Temporal Clustering"
subtitle:   ""
date:       2021-07-06
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - HRL
---

# Option Discovery in Hierarchical Reinforcement Learning using Spatio-Temporal Clustering

论文链接：https://arxiv.org/pdf/1605.05359v3.pdf



## 主要内容

- 本文旨在提出一个自动的技能获取框架，可以分层描述任务，对状态抽象以及在抽象状态之间拓展行为。这样的分层结构可以加速学习过程


- 使用动态系统中寻找状态空间中亚稳态区域和抽象状态的思想。We use ideas from dynamical systems to find metastable regions in the state space and associate them with abstract states
-  使用空间聚类算法PCCA+，做状态抽象，形成抽象的状态量
-  These skills are independent of the learning task and can be efficiently reused across a variety of tasks defined over the same model
- The core idea of hierarchical reinforcement learning is to break down the reinforcement learning problem into subtasks through a hierarchy of abstractions.

- 自动发现不同的技能，这是一个小领域。Our focus in this paper is to present our framework on automated discovery of skills
  - 定义状态空间中的瓶颈状态，这些状态被划分为集合，两组罕见状态之间的转换会在这种转换的相应点引入瓶颈状态。达到这种瓶颈状态的策略被缓存为选项 (McGovern and Barto 2001).
  - 使用因式分解来表示状态空间中存在的结构，识别很少变化的动作序列，并把这些序列作为选项被缓存起来 (Hengst 2004)
  - 使用智能体与环境交互的图表征，并使用中间中心性度量来识别子任务
  - 使用聚类方法(光谱或其他方法)分离出MDP的不同强关联成分，并识别连接不同聚类的瓶颈 (Menache, Mannor, and Shimkin 2002).

- 文章的目标是在没有任何先验知识的情况下获得技能和发现抽象

- 使用SMDP Q-Learning，Intra Option Q-Learning

- 主要贡献：
  - 使用PCCA+抽象MDP的state
  - 设计一种组合option的方式
  - 大状态空间下，不好使用，设计一种聚合的状态空间

- Option框架：The option framework is one of the formalisations used to represent hierarchies in RL (Sutton, Precup, and Singh 1999). Formally, an option is a tuple $O = (I, \mu, \beta)$ here:
  - $I$ is the initiation set: a set of states from which the action can be activated
  - $\mu$ is a policy function where µ(s, a) represents the preference value given to action a in state s following option O.
  - $\beta$ is the termination function: When an agent enters a state s while following option O, the option could be terminated with a probability $\beta(s)$.

- Semi-Markov Decision Process (SMDP)

$$
Q(s,o)\leftarrow Q(s,o)+\alpha[r+\gamma^k \max_{a\in O} Q(s,a)-Q(s,o)]
$$

- SMDP学习方法的一个主要缺点是，在了解其结果之前，选项需要完全执行到终止。

- Perron Cluster Analysis (PCCA+)



## 总结

主要工作是利用一个谱聚类算法自动地寻找option，算是options类算法的变体。option可以赋予策略语义特征，可以看看原始文章。


