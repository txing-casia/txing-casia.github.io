---
layout:     post
title:      "Reinforcement Driving | Mastering the game of Go without human knowledge"
subtitle:   "AlphaGo Zero"
date:       2022-10-19
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

## Mastering the game of Go without human knowledge

AlphaGo和AlphaGo Master之后的又一力作，不同于AlphaGo学习人类棋谱，利用人类先验知识，AlphaGo Zero完全自学，并在学习三天后超越了AlphaGo，40天超越了AlphaGo Master。

### 1 Introduction

#### Reinforcement learning in AlphaGo Zero

- 算法的特点是将RL的策略网络和价值网络合并，只改变输入层。神经网络表示为 $$(p,v)=f_{\theta}(s)$$

  - 输入：棋局表征 $$s$$ 及其历史信息
  - 输出：下一步的落子概率 $$p$$ ，当前局面获胜概率 $$v$$
  - 训练目标是去拟合自我对弈里面产生的真实胜率 $$z$$ 和下面提到的 MCTS 产生的落子概率 $$π$$，即 $$(p,v)\rightarrow(π,z)$$。

- MCTS（蒙特卡洛树搜索）使用 $$f_{\theta}$$ 进行自博弈，搜索结果 $$\pi$$ 一般优于网络估计的结果 $$p$$

  ![Self-play reinforcement learning in AlphaGo Zero](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221019-1.jpg)

- 因此，基于以下两点，可以实现训练：

  1. MCTS 优于网络 $$f_{\theta}(s)$$ ；
  2. $$f_{\theta}(s)$$ 性能提升后，MCTS自博弈的结果也更好，两者相互促进提升；

- MCTS:

  - 搜索树使用神经网络策略引导仿真（自博弈）

  - 搜索树的每一条边有一个先验概率 $$P(s,a)$$，一个访问数（visit count） $$N(s,a)$$，一个行为价值 $$Q(s,a)$$

  - 每次仿真从根节点开始迭代进行，最大化置信上界 $$Q(s,a)+U(s,a)$$，直到达到访问过的叶子节点。$$U(s,a)\propto P(s,a)/(1+N(s,a))$$

  - 每个叶子节点位置仅被网络扩展和评估一次，以生成先验概率和评估值；$$(P(s',\cdot),V(s'))=f_{\theta}(s')$$

  - 每条边$$(s,a)$$每使用一次就增加访问数$$N(s,a)$$，并更新行为价值为仿真中的平均估计值，$$Q(s,a)=1/N(s,a)\sum_{s'\mid s,a\rightarrow s'} V(s')$$ 

  - MCTS 使用的节点选择策略正比与节点访问数 $$\pi_{\alpha} \propto N(s,a)^{1/\tau}$$，$$\tau$$ 是温度系数；

  - 仿真直到搜索的价值低于阈值或者到达最大次数，此时给出最终奖励$$r_T\in \{-1,+1\}$$

  - 每一时间步的数据存为$$(s_t,\pi_t,z_t)$$，$$z_t=\pm r_T$$是当前玩家在第$$t$$步成为赢家获得的奖励

  - 网络优化：$$v\rightarrow z,p\rightarrow \pi$$

  - 损失函数为均方误差+交叉熵损失+网络权重的平方：

    $$l=(z-v)^2-\pi^T\log p +c\mid\mid\theta\mid\mid^2$$

#### Empirical analysis of AlphaGo Zero training

![MCTS](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221019-2.jpg)











### 总结

这篇文章介绍的范围太大，涵盖的研究方向和方法过多，受篇幅限制，讲的东西又很浅显，价值不大。



​	
