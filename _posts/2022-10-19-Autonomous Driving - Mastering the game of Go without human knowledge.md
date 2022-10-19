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

  ![A hierarchical scheme of Autonomous Ground Vehicle systems.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221019-1.jpg)

- 因此，基于以下两点，可以实现训练：
  1. MCTS 优于网络 $$f_{\theta}(s)$$ ；
  2. $$f_{\theta}(s)$$ 性能提升后，MCTS自博弈的结果也更好，两者相互促进提升；













### 总结

这篇文章介绍的范围太大，涵盖的研究方向和方法过多，受篇幅限制，讲的东西又很浅显，价值不大。



​	
