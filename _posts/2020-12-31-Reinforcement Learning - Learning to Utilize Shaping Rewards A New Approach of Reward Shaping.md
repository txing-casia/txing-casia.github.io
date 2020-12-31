---
layout:     post
title:      "Reinforcement Learning | Learning to Utilize Shaping Rewards: A New Approach of Reward Shaping"
subtitle:   ""
date:       2020-12-31
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Reinforcement Learning
    - Reward Shaping
---

# Learning to Utilize Shaping Rewards A New Approach of Reward Shaping

论文链接：NIPS 2020

## 背景

人工方式塑造奖励函数有着先天的缺陷，这里提出了一种双层优化（ bi-level optimization problem）的方式来自适应的设计奖励函数。实验在sparse-reward的环境中进行验证。

- lower level使用塑造的奖励优化策略；

- upper level优化参数化的塑造权重函数来最大化真实的奖励；



## 主要工作

- Reward Shaping (RS)：一个常用的提高样本效率的方法是把可能的领域知识迁移到额外的奖励函数中，从而在原始和新的奖励驱使下学习更快更好。

- 但是奖励的设计不可避免的涉及了人工的操作，

### 3 Parameterized Reward Shaping

#### 3.1 Bi-level Optimization

- 定义参数化奖励塑造函数：

$$
\tilde{r}(s,a)=r(s,a)+z_{\phi}(s,a)f(s,a)
$$

$$z_{\phi}(s,a)$$是权重向量，形式是使用参数化函数。$$f$$是shaping 奖励函数。

$$
\text{Step 1: }\ 
\max_{\phi} \mathbb{E}_{s\sim \rho^{\pi},a\sim\pi_{\theta}} [r(s,a)]\\
\text{Step 2: }\ 
\theta=\arg \max_{\theta'} \mathbb{E}_{s\sim \rho^{\pi},a\sim\pi_{\theta'}}[r(s,a)+z_{\phi}(s,a)f(s,a)]
$$
通过真实的环境奖励$$r(s,a)$$更新$$z_{\phi}(s,a)$$，在根据shaping reward function更新policy。

#### 3.2 Gradient Computation

- 固定$$z_{\phi}$$，计算累计修正奖励$$\tilde J$$关于参数$$\theta$$的梯度：
  $$
  \nabla_{\phi}\tilde J(\pi_{\theta})= \mathbb{E}_{s\sim \rho^{\pi},a\sim\pi_{\theta}} \big[\nabla_{\theta}\log \pi_{\theta}(s,a)\tilde Q(s,a)\big]
  $$

- 定理1：目标函数$$J(z_{\phi})$$关于参数$$\phi$$的梯度为：
  $$
  \nabla_{\phi} J(z_{\phi})= \mathbb{E}_{s\sim \rho^{\pi},a\sim\pi_{\theta}} \big[\nabla_{\theta}\log \pi_{\theta}(s,a) Q(s,a)\big]
  $$
  这个定理首先假设了$$\pi_{\theta}$$关于$$\phi$$的梯度存在，但是即使有了定理1也不能直接计算出这个梯度，因为$$\pi_{\theta}$$关于$$\phi$$的梯度不能直接计算，下面会讨论如何计算$$\nabla_{\phi}\pi_{\theta}(s,a)$$

### 4 Gradient Approximation

#### 4.1 Explicit Maping

- 假设行为输出在$$[0,1]$$，构建一个扩展的状态空间（extended state space）$$\mathcal{S}_z=\{(s,z_{\phi}(s))|{\forall}s\in \mathcal{S} \}$$，根据链式法则，得到$$\nabla_{\phi}\pi_{\theta}(s,a,z_{\phi}(s))=\nabla_{z}\pi_{\theta}(s,a,z_{\phi}(s))\nabla_{\phi}z_{\phi}(s)$$，相应的，上级目标函数$$J(z_{\phi})$$关于$$\phi$$的梯度为：
  $$
  \nabla_{\phi}J(z_{\phi})=\mathbb{E}_{s\sim \rho^{\pi},a \sim \pi_{\theta}}[\nabla_z \log \pi_{\theta}(s,a,z)|_{z=z_{\phi}(s)}\nabla_{\phi}z_{\phi}(s)Q^{\pi}(s,a)]
  $$

#### 4.2 Meta-Gradient Learning

- 考虑到参数$$\theta$$和$$\phi$$之间的关系，可以通过计算元梯度（meta-gradient）$$\nabla_{\phi}\theta$$的方式计算：
  $$
  \nabla_{\phi}\pi_{\theta}(s,a)=\nabla_{\theta}\pi_{\theta}(s,a)\nabla_{\phi}\theta
  $$

- 对$$\theta$$的更新：
  $$
  \theta'=\theta+\alpha\sum_{i=1}^N \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)\tilde Q(s_i,a_i)
  $$
  其中$$N$$是batch样本数，$$\alpha$$是学习率

- meta-gradient$$\nabla_{\phi}\theta'$$：
  $$
  \nabla_{\phi}\theta'=\nabla_{\phi}(\theta+\alpha\sum_{i=1}^{N}\nabla_{\theta}\log \pi_{\theta}(s_i,a_i)\tilde Q(s_i,a_i)\\
  =\alpha \sum_{i=1}^{N} \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top} \nabla_{\phi}\tilde Q(s_i,a_i)\\
  $$
  这里$$\theta$$是一个常数。

  在计算过程中，例如采用蒙特卡洛返回，对每一个在buffer中的样本$$i$$，定义$$\tau_i=(s^0_i,a^0_i,\tilde r^0_i,s^1_i,a^1_i,\tilde r^1_i,...)$$表示采样的轨迹由于$$\tilde r_i^t=r^t_i+z_{\phi}(s^t_i,a^t_i)f(s^t_i,a^t_i)$$，其中$$r^t_i$$是采样的真实奖励。
  $$
  \nabla_{\phi}\theta'=\alpha \sum_{i=1}^{N} \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top} \nabla_{\phi}\tilde Q(s_i,a_i)\\
  \approx \alpha\sum_{i=1}^N  \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top} \nabla_{\phi} \sum_{t=0}^{|\tau_i|-1} \gamma^t (r^t_i +z_{\phi}(s^t_i,a^t_i)f(s^t_i,a^t_i))\\
  =\alpha\sum_{i=1}^N  \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top} \sum_{t=0}^{|\tau_i|-1}\gamma^t f(s^t_i,a^t_i)\nabla_{\phi}z_{\phi}(s^t_i,a^t_i)
  $$

#### 4.3 Incremental Meta-Gradient Learning
- 考虑之前假设的策略参数$$\theta$$关于$$\phi$$是恒定的，实际上，可以看做不是恒定的。
  
- Incremental Meta-Gradient Learning (IMGL)
  $$
  \nabla_{\phi}\theta'=\nabla_{\phi}\theta + \alpha \sum_{i=1}^{N} \nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top}  \tilde Q(s_i,a_i) + \alpha \sum_{i=1}^{N} \log \pi_{\theta}(s_i,a_i)^{\top}  \nabla_{\theta}\tilde Q(s_i,a_i)\\
  =\nabla_{\phi}\theta + \alpha \sum_{i=1}^{N} \big(\nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top}\nabla_{\phi}\theta \big)  \tilde Q(s_i,a_i) + \alpha \sum_{i=1}^{N} \log \pi_{\theta}(s_i,a_i)^{\top}  \nabla_{\theta}\tilde Q(s_i,a_i)\\
  =\big( I_n + \alpha \sum_{i=1}^{N} \tilde Q(s_i,a_i)\nabla_{\theta}\log \pi_{\theta}(s_i,a_i)^{\top}    \big) \nabla_{\phi}\theta+ \alpha \sum_{i=1}^{N} \log \pi_{\theta}(s_i,a_i)^{\top}  \nabla_{\theta}\tilde Q(s_i,a_i)\\
  $$
  



## 总结

挺有意思的一个工作，提供了三种梯度更新方式，分别对应精确映射，蒙特卡洛更新和TD更新。三个方式有着不同的梯度近似精度。





