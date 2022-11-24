---
layout:     post
title:      "Reinforcement Driving | Mastering Atari, Go, chess and shogi by planning with a learned model"
subtitle:   "MuZero"
date:       2022-10-20
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

## Mastering Atari, Go, chess and shogi by planning with a learned model

MuZero是AlphaGo Zero之上的第二次改进。第一次改进为AlphaZero，一个模型实现围棋、国际象棋、将棋的超人水平。此次在AlphaZero基础上，更是从棋类拓展到了更具有普遍意义的Atari游戏。

训练模型的规划能力通常是较为困难的，本文提出MuZero，一个结合学习模型的树搜索方法

### 1 Introduction

- model-based RL 需要   
  - 状态转移模型：根据是s,a预测下一时刻的状态
  - 奖励模型：预测期望的奖励

### 2 MuZero algorithm

- 构建一个模型 $$\mu_{\theta}$$ 预测三个未来的量：

  - Policy $$p^k_t \approx \pi(a_{t+k+1}\mid o_1,...,o_t,a_{t+1},...,a_{t+k})$$
  - Value function $$v_t^k\approx \mathbb{E}[u_{t+k+1}+\gamma u_{t+k+2}+...\mid o_1,...,o_t,a_{t+1},...,a_{t+k}]$$
  - immediate reward $$r_t^k\approx u_{t+k}$$

  $$u$$表示真实的观测到的奖励，$$\pi$$表示用于选择真实行为的策略

- 具体地，模型包括三个部分：
  - dynamics function: $$r^k,s^k=g_{\theta}(s^{k-1},a^k)$$，在每个假设步骤k执行，$$r^k$$是即时奖励；$$s^k$$是内部状态表征（internal state），其没有环境信息的语义含义，只是用于准确预测未来变量policies, values and rewards
  - prediction function: $$p^k,v^k=f_{\theta}(s^k)$$
  - representation function: $$s^0=h_{\theta}(o_1,...,o_t)$$，根据过去的观测信息生成根节点的内部状态表征
- 利用上述模型，可以利用过去的观测信息$$o_1,...,o_t$$，生成假设的未来轨迹$$a^1,...,a^k$$
- 损失函数：（每一项都是L2损失函数）

$$
l_t(\theta)=\sum_{k=0}^K l^P(\pi_{t+k},p_t^k)+\sum_{k=0}^K l^V(z_{t+k},v_t^k)+\sum_{k=1}^K l^r(u_{t+k},r_t^k)+c\mid \mid\theta\mid\mid^2
$$

policy损失, value损失 and 即时reward损失

![Planning, acting and training with a learned model](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20221020-1.jpg)









- 

  
  
- 




### 总结



​	
