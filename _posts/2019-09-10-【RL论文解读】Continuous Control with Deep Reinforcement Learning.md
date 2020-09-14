---
layout:     post
title:      "Reinforcement Learning | Deep Deterministic Policy Gradient algorithm (DDPG)"
subtitle:   "DDPG"
date:       2019-09-10
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
---

# Continuous Control with Deep Reinforcement Learning

论文链接：https://arxiv.org/abs/1509.02971v2

## 前言

> Reinforcement Learning是2019年9月新开的专栏企划，旨在为今后RL的学习提供比较广阔的专业视野，同时贡献于New Idea的产生。

## 方法

- **意义：**DDPG诞生于2016年的ICRL，作为Deep Q-learning的进阶算法，将Q-learning从离散action space拓展到了连续action space的应用领域，并实现了一个end-to-end ( image to action ) 的决策框架。

- **难点：**DDPG提供了一个以未处理的高维传感信号作输入，解决复杂任务的算法框架。One of the primary goals of the field of artificial intelligence is to solve complex tasks from unprocessed, high-dimensional, sensory input.

- DQN的缺点：However, while DQN solves problems with high-dimensional observation spaces, it can only handle discrete and low-dimensional action spaces.

- 面向的应用场景：Many tasks of interest, most notably physical control tasks, have continuous (real valued) and high dimensional action spaces.

- 如果想把DQN用在连续的action space中，有一种办法是把action离散化。但是这样会大大增加action的维度，从而带来决策上的困难。例如一个3自由度的机器人，把每个关节角离散为10个角度，那么action space维度就会变为$$10^{3}$$。在需要精确控制的时候，这个问题会更加严重。（the number of actions increases exponentially with the number of degrees of freedom）。此外，这种粗略的离散化丢失了action的结构信息，可能会影响到问题的求解。

- DDPG本质：In this work we present a model-free, off-policy actor-critic algorithm using deep function approximators that can learn policies in high-dimensional, continuous action spaces.

- DDPG基于DPG算法，DPG由于只是加入了简单的神经网络函数近似，因此算法并不稳定。

- DQN is able to learn value functions using such function approximators in a stable and robust way due to two innovations: 

  - the network is trained off-policy with samples from a **replay buffer** to minimize correlations between samples; 
  - the network is trained with a **target Q network** to give consistent targets during temporal difference backups 

- DDPG在实验中用摄像机的画面提取低维信息（例如：cartesian coordinates  or joint angles 笛卡尔坐标或者关节角），将其作为observation。using both low-dimensional observations (e.g. joint angles) and directly from pixels.

- Here, we assumed the environment is fully-observed so $$s_t = x_t$$. 

- replay buffer + soft updates

**DDPG algorithm**

  > ---
  >
  > Randomly initialize critic network $$Q(s,a\mid \theta^Q)$$ and actor $$\mu(s\mid\theta^{\,u})$$ with weights $$\theta^Q$$ and $$\theta^{\mu}$$
  >
  > Initialize target network $$Q'$$ and $$\mu'$$ with weights $$\theta^{Q'}\leftarrow \theta^{Q}$$, $$\theta^{\mu'}\leftarrow \theta^{\mu}$$
  >
  > Initialize replay buffer $$R$$
  >**For** $$episode = 1$$, $$M$$ **do** 
  > 
  > ---- Initialize a random process $$\mathscr{N}$$ for action exploration
  >---- Receive initial observation state $$s_1$$
  > 
  > ---- **For** $$t = 1, T$$ **do** 
  >
  > ---- ---- Select action $$a_t = \mu(s_t\mid\theta^{\mu}) + \mathscr{N}_t$$ according to the current policy and exploration noise 
  >
  > ---- ---- Execute action at and observe reward $$r_t$$ and observe new state $$s_{t+1}$$
  >
  > ---- ---- Store transition $$(s_t,a_t,r_t,s_{t+1})$$ in $$R$$
  >
  > ---- ---- Sample a random minibatch of $$N$$ transitions $$(s_t,a_t,r_t,s_{t+1})$$ from $$R$$
  >
  > ---- ---- Set $$y_i = r_i + \gamma Q'(s_{i+1},\mu'(s_{i+1}\midθ^{\mu'})|\theta ^{Q'})$$
  >
  > ---- ---- Update critic by minimizing the loss: $$L = \frac{1}{N} \sum_i(y_i - Q(s_i,a_i \mid \theta^Q))^2$$
  >
  > ---- ---- Update the actor policy using the sampled policy gradient: 
  >
  > ---- ---- ---- $$\triangledown _{\theta^{\mu}}J \approx \frac{1}{N}\sum\triangledown_a Q(s,a\mid\theta^Q)\mid _{s=s_i,a=\mu(s_i)}\triangledown_{\theta^{\mu}}\mu(s\mid\theta^{\mu})|_{s_i}$$
  >
  > ---- ---- Update the target networks: 
  >
  > ---- ---- ---- $$\theta^{Q'}\leftarrow\tau\theta^Q+(1-\tau)\theta^{Q'}$$, 
  >
  > ---- ---- ---- $$\theta^{\mu'}\leftarrow\tau\theta^{\mu}+(1-\tau)\theta^{\mu'}$$
  >
  > ---- **end For** 
  >
  > **end For**



---

## 总结

不知道为什么看完之后觉得DDPG并没有想象中的复杂，也没有什么推导过程，架构也比较简单清晰，文章中大量的实验验证了算法的普适性。可能这些点正是一个RL算法的可贵之处吧。































