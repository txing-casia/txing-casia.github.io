---
layout:     post
title:      "Autonomous Driving | An Optimistic Perspective on Offline Reinforcement Learning (Google)"
subtitle:   ""
date:       2022-08-12
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## An Optimistic Perspective on Offline Reinforcement Learning (Google)

作者用online DQN在60款 Atari 2600游戏上获取数据样本，然后用这些样本(fixed dataset)训练offline强化学习算法，一些offline的算法性能可以超过online的算法。本文提出的Random Ensemble Mixture (REM)算法在离线回放数据上的表现超过了强的基准算法。因此作者认为在离线样本足够多，多样化充分的情况下，使用鲁棒的RL算法可以获得高质量的策略。

代码： github.com/google-research/batch_rl

### 1 Introduction

离线强化学习的设定是不与真实环境的主动交互，而是通过对收集的离线回放数据中学习策略，在评估模型中生成新的交互数据。相应的情景在以下场景均会面临：

- **robotics** 
  - Cabi, S., Colmenarejo, S. G., Novikov, A., Konyushkova, K., Reed, S., Jeong, R., Zołna, K., Aytar, Y., Budden, D., Vecerik, M., et al. A framework for data-driven robotics. arXiv preprint arXiv:1909.12200, 2019.
  - Dasari, S., Ebert, F., Tian, S., Nair, S., Bucher, B., Schmeckpeper, K., Singh, S., Levine, S., and Finn, C. Robonet: Large-scale multi-robot learning. CoRL, 2019.
- **autonomous driving** 
  - Yu, F., Xian, W., Chen, Y., Liu, F., Liao, M., Madhavan, V., and Darrell, T. Bdd100k: A diverse driving video database with scalable annotation tooling. CVPR, 2018.
- **recommendation systems** 
  - Strehl, A. L., Langford, J., Li, L., and Kakade, S. Learning from logged implicit exploration data. NeurIPS, 2010.
  - Bottou, L., Peters, J., Quiñonero-Candela, J., Charles, D. X., Chickering, D. M., Portugaly, E., Ray, D., Simard, P., and Snelson, E. Counterfactual reasoning and learning systems: The example of computational advertising. JMLR, 2013.
- **healthcare**
  - Shortreed, S. M., Laber, E., Lizotte, D. J., Stroup, T. S., Pineau, J., and Murphy, S. A. Informing sequential clinical decisionmaking through reinforcement learning: an empirical study. Machine learning, 2011.

面对离线强化学习问题时，off-policy算法普遍表现不好，设计大的replay buffer反而会损害off-policy算法的性能（由于算法的off-policyness）

- 对比的方法包括：
  - **offline QR-DQN**：Dabney, W., Rowland, M., Bellemare, M. G., and Munos, R. Distributional reinforcement learning with quantile regression. AAAI, 2018.
  - **DQN**：（Nature）
  - **Random Ensemble Mixture**（REM，随机集成混合）：为本文提出方法
  -  **online C51**： 分布式DQN算法。Bellemare, M. G., Dabney, W., and Munos, R. A distributional perspective on reinforcement learning. ICML, 2017.
  -  **distributional QR-DQN (SOTA)**：Dabney, W., Rowland, M., Bellemare, M. G., and Munos, R. Distributional reinforcement learning with quantile regression. AAAI, 2018  

### 2 Off-policy Reinforcement Learning 

- DQN算法介绍

  - Huber loss：介于MSE和MAE之间的，对数据异常值更不敏感的loss

  $$
  l_{\lambda}(u)=
  \begin{align}
  \begin{cases}
  \frac{1}{2}u^2,&\mid u \mid \leq \lambda\\
  \lambda(\mid u\mid-\frac{1}{2}\lambda),& \text{otherwise}
  \end{cases}
  \end{align}
  $$


- baseline方法：分布式RL（Distributional RL）
  - C51
  - QR-DQN

### 3 Offline Reinforcement Learning  

- offline的模式分离了模型对经验的利用、生成能力（exploit） vs 探索效率（explore）

- offline RL面临的挑战是**distribution mismatch**：错误匹配当前使用的策略和固定的离线数据集。例如，当采取了数据集中不存在的行为时，并不知道响应的奖励是多少。  
- 本文尝试在不解决distribution mismatch的基础上，训练高性能的agent

### 4 Developing Robust Offline RL Algorithms  

- 采用集成（Ensemble）可以提高模型的泛化能力，本文使用了Ensemble DQN和REM两个采取该思想的方法。

#### 4.1 Ensemble-DQN  

- 该方法是对DQN算法的简单扩展，使用集成多个参数化的Q函数来近视Q值。

  - Faußer, S. and Schwenker, F. Neural network ensembles in reinforcement learning. Neural Processing Letters, 2015  
  - Osband, I., Blundell, C., Pritzel, A., and Van Roy, B. Deep exploration via bootstrapped DQN. NeurIPS, 2016.  
  - Anschel, O., Baram, N., and Shimkin, N. Averaged-dqn: Variance reduction and stabilization for deep reinforcement learning. ICML, 2017.  

- 每个参数化Q函数的优化目标是近似真实的Q值，参考下面这篇文章：

  - **Bootstrapped-DQN**：Osband, I., Blundell, C., Pritzel, A., and Van Roy, B. Deep exploration via bootstrapped DQN. NeurIPS, 2016. 

- ![损失函数](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220812-2.png)

  其中，$$l_{\lambda}$$是Huber损失。算法使用所有Q函数的均值作为输出。

#### 4.2 Random Ensemble Mixture (REM)

- 引入了dropout：
  - Srivastava, N., Hinton, G., Krizhevsky, A., Sutskever, I., and Salakhutdinov, R. Dropout: a simple way to prevent neural networks from overfitting. JMLR, 2014  

- 不同于Ensemble-DQN，REM构造一个包含多个Q函数近似器的凸组合（convex combination），将多个Q 函数近似器作为1个近似器使用。使用(K − 1)-simplex计算混合的概率。

![模型结构](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220812-3.png)

- 对于每个mini-batch，随机产生一个分类分布（categorical distribution）$$\alpha$$，它定义了一个逼近最优Q-函数的K个估计器的凸组合

- ![REM的loss形式](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220817-3.png)

  其中，$$P_{\Delta}$$表示标准的(K-1)-simplex的概率分布，$$\Delta^{K-1}=\{\alpha\in R^K: \alpha_1+\alpha_2+...+\alpha_K=1,\alpha_k\geq0,k=1,...,K \}$$ 

- 对于$$P_{\Delta}$$：先从Uniform (0,1)分布中采样K个独立同分布的值，然后归一化它们获得有效的分类分布（$$a'_k \sim U(0,1),a_k=a'_k/\sum_k a'_i$$）

- 对于Q值的求解，使用$$Q(s,a)=\frac{1}{K}\sum_k Q_{\theta}^k (s,a)$$

### 5 Offline RL on Atari 2600 Games

- 将Nature DQN在60个Atari游戏上的行为数据用来构建DQN replay数据集，每个游戏2亿帧（200 million frames）
- 每个游戏训练5个智能体，因此60个游戏一共有60个数据集
- ofline RL算法性能对比：

![Offline RL on Atari 2600.](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220812-1.png)

- ![Offline QR-DQN vs. DQN (Nature)](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220818-1.png)
- ![Offline Agents on DQN Replay Dataset](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220818-2.png)

- ![Asymptotic performance of offline agents](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220818-3.png)

- 文中提到offline连续强化学习方法，实验了offline DDPG，offline TD3，offline BCQ等算法

  - Fujimoto, S., Meger, D., and Precup, D. Off-policy deep reinforcement learning without exploration. ICML, 2019b.

    ![Asymptotic performance of offline agents](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220818-4.png)

### 总结

总的来说，文章本身目的更倾向于提供一种乐观的横向对比，证明offline RL在一些情况下可以获取SOTA的性能，甚至超过online RL。提供的几个offline Q-learning变体和offline连续情况的RL算法可以再看看。
