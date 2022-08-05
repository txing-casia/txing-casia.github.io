---
layout:     post
title:      "Autonomous Driving | UMBRELLA: Uncertainty-Aware Model-Based Offline Reinforcement Learning Leveraging Planning"
subtitle:   ""
date:       2022-08-04
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## UMBRELLA: Uncertainty-Aware Model-Based Offline Reinforcement Learning Leveraging Planning

- 在学习过程中考虑了随机不确定性的影响，提高了模型的可迁移性和可解释性

### 1 Introduction  

- 目前的模型考虑到多智能体之间的交互和复杂行为，多采用工程设计的驾驶策略，但这样难以适用更复杂的任务。强化学习通过试错学习的方式避免了这些手工设计，但需要大量的试错机会。相比于模仿学习习得次优行为，强化学习可以提升不同类型数据的质量

- 主要贡献：
  - 提出了一个基于模型的离线规划算法UMBRELLA，在观测上考虑了认知和偶然的不确定性（epistemic 和 aleatoric）  
  - 引入了一个消融实验，优化最坏情况模型
  - 实验中，在稠密车流（with dense traffic）的城市和高速场景中，胜过了BC行为克隆方法和MBOP算法

### 2 Related Work  

- **Model-based Offline Reinforcement Learning**：

  其主要的问题是the distributional shift。MOReL和MBOP方法将动态模型的认知不确定性估计结合到奖励函数中，以惩罚未被行为分布覆盖的状态

  - Sergey Levine, Aviral Kumar, George Tucker, and Justin Fu. Offline reinforcement learning: Tutorial, review, and perspectives on open problems. CoRR, 2020. arXiv:2005.01643.  

  这些方法在多智能体环境中测试，没考虑到由行人和其它车辆行为造成的偶然不确定性（aleatoric uncertainty）的影响。[Henaff et al., 2019]通过由条件变分自动编码器（conditional variational autoencoder，CVAE）表示的随机动力学模型解决了这个问题[Kingma and Welling, 2014]。然而，他们的方法依赖于策略学习，这与基于模型的离线规划相比，可解释性和控制灵活性有所降低。

  - Mikael Henaff, Alfredo Canziani, and Yann LeCun. Model-predictive policy learning with uncertainty regularization for driving in dense traffic. In International Conference on Learning Representations, \2019.  
  - Diederik P. Kingma and Max Welling. Auto-Encoding Variational Bayes. In International Conference on Learning Representations, 2014.  

- **Interaction-aware Motion Prediction and Planning**：

  单纯通过规划安全的pass进行自动驾驶忽略了车辆之间包含规划和预测（planning and prediction）的行为交互  

  引入博弈论的方法考虑了多智能体的动力学过程，但带来了计算上的开销。

  一些基于学习的模型可以生成更多的场景，但是没有考虑认知不确定性

  - Jerry Liu, Wenyuan Zeng, Raquel Urtasun, and Ersin Yumer. Deep structured reactive planning. In IEEE International Conference on Robotics and Automation, 2021.  
  - Nicholas Rhinehart, Jeff He, Charles Packer, Matthew A. Wright, Rowan McAllister, Joseph E. Gonzalez, and Sergey Levine. Contingencies from observations: Tractable contingency planning with learned behavior models. In IEEE International Conference on Robotics and Automation, 2021.  

### 3 The UMBRELLA Framework  

- UMBRELLA 是 MBOP 算法的扩展  
  - Arthur Argenson and Gabriel Dulac-Arnold. Model-based offline planning. In International Conference on Learning Representations, 2021.  

- 问题形式化，用MDP的$$(S; A; p; r; \gamma)$$ 表示，在offline设定中，智能体不与环境直接交互，而是从数据集中学习策略$$\pi_d$$。如果观测值$$O$$并不是完全可得，那么问题就变化为（partially observable MDP，POMDP），使用 $$M_{PO}=(S;A;O;p;r;\gamma)$$，处理该问题的常用方法是使用nth-order history 方法，可以近似得到状态估计，然后将其转变为MDP，用标准的RL方法处理。

- UMBRELLA使用连续的潜在变量（continuous latent variable）$$z_t\in Z$$ ，并枚举自车所有的可能行为（行为采样通过学到的BC policy）。预测N条长度界限为H的轨迹。还使用了一个return-weighted trajectory optimizer，处理POMDP问题，用持续观测状态从$$o_{t-_c:t}$$直到时间步t估计缺失的观测状态。

  - $$z_t=\mu_{\phi}+\sigma_{\phi}*\epsilon$$
  - $$(\mu_{\phi},\sigma_{\phi})=q_{\phi}(s_t,s_{t+1})$$
  - $$\epsilon \sim \mathcal{N}(0,1)$$

- 随机动力学模型：

  - 为了建模不同的未来可能情况，学习随机前向动力学模型$$f_{m,\theta}:S\times A\times Z \rightarrow S \times \mathbb{R}$$

  - 该模型采用CVAE架构，预测下一时刻的状态

    - Diederik P. Kingma and Max Welling. Auto-Encoding Variational Bayes. In International Conference on Learning Representations, 2014.  

  - $$\hat{s}_{t+1}=f_m(s_t,a_t,z_t)_s$$

  - $$\hat{r}_{t}=f_m(s_t,a_t,z_t)_r$$

  - 其中潜在变量$$z_t$$建模了不同的未来预测，并确保了输出对于输入是非确定性的。在训练过程中，该潜在变量从后验分布$$q_{\phi}(z\mid s_t,s_{t+1})$$中采样，$$\phi$$是参数。由于实际采样只能从先验分布中采，因此使用Kullback-Leibler (KL) divergence度量后验分布和先验分布$$p(z)$$并最小化距离。

    - 潜变量用于区分细分情况，精细化建模

  - 定义Evidence Lower BOund (ELBO)目标训练VAEs。

    - Evidence Lower BOund： https://zhuanlan.zhihu.com/p/400322786

  - 损失函数为：
    $$
    L(\theta,\phi;s_t,s_{t+1},a_t,r_t)=\mid\mid s_{t+1} - f_{m,\theta}(s_t,a_t,z_t)_s\mid\mid_2^2 + \mid\mid r_t-f_{m,\theta}(s_t,a_t,z_t)_r\mid\mid^2_2 \\
    \zeta D_{KL}(q_{\phi}(z_t\mid s_t,s_{t+1})\mid\mid p(z_t))
    $$

  - BC policies $$f_{b,\psi}:S\times A^{n_c} \rightarrow A $$ ，$$f_b(s_t,a_{(t-n_c):(t-1)})$$，该函数使用当前的状态和$$n_c$$个之前的行为作为输入，输出行为$$a_t$$。通过将之前的行为串联考虑，可以使得输出的行为更加平滑。

  - 训练BC Policy使用最小化损失函数：

    $$L(\psi;s_t,a_{(t-n_c):(t-1)},a_t)=\mid\mid a_t - f_{b,\psi}(s_t,a_{(t-n_c):(t-1)})\mid\mid^2_2$$

  - 截断价值函数$$f_{R,\xi}:S\times A^{n_c}\rightarrow \mathbb{R}$$ 估计H个时间步后的期望回报$$\hat{R}_H$$

- ![Simplified network architecture of the stochastic forward dynamics model during training](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-3.png)

- ![Behavior-cloned policy network architecture](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-4.png)

- ![Truncated value function network architecture](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-5.png)

- ![Signal flow of the stochastic forward dynamics model  ](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-6.png)

- **UMBRELLA-Planning**：采用了MPC，在每一步规划未来H步的最优控制轨迹，然后只执行第一步行为，之后再次进行规划，并不断迭代。通过这样的迭代优化可以降低模型误差带来的影响。

- **UMBRELLA Trajectory Optimizer**：

  最终的轨迹输出使用MPPI framework

  - Grady Williams, Andrew Aldrich, and Evangelos A. Theodorou. Model predictive path integral control: From theory to parallel computation. Journal of Guidance, Control, and Dynamics, 40(2): 344–357, 2017.  

- 最后，模型通过re-weighting获得最优轨迹：

$$
T^*_t = \frac{\sum_{n=1}^N e^{kR_n} A_{n,t+1}}{\sum_{n=1}^N e^{kR_n}}
$$

![UMBRELLA Planning](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-1.png)

- **Pessimistic Trajectory Optimizer**： 面对认知的不确定性，UMBRELLA-P对最坏情况的结果进行优化，并采取悲观的行动

### 4 Experimental Evaluation  

- 环境：
  - **NGSIM**：多智能体自动驾驶环境  
  - **CARLA**：城市多智能体自动驾驶场景
- 基线方法：
  - **1-step IL**：模仿专家行为的BC policy
  - **MBOP**：当前最好的基于模型的离线RL方法
    - Arthur Argenson and Gabriel Dulac-Arnold. Model-based offline planning. In International Conference on Learning Representations, 2021.  
  - **MPUR**：最好的基于模型策略学习方法
    - Mikael Henaff, Alfredo Canziani, and Yann LeCun. Model-predictive policy learning with uncertainty regularization for driving in dense traffic. In International Conference on Learning Representations, 2019.  
- 指标：
  - Success rate (SR)：The rate of collision-free episodes，并要求在时间内到达目标位置
  - Mean distance (MD)：NGSIM中的纵向行驶距离
  - mean successful time (MST)  
  - mean proximity reward $$\overline{r}_{prox}$$
  - mean lane reward $$\overline{r}_{lane}$$  
  - mean final reward $$\overline{r}$$  
- 实验结果：

![性能对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-2.png)




### 总结

奖励函数误设计仍然是一个较大的问题，也是自动驾驶策略不像人的原因之一。

- W. Bradley Knox, Alessandro Allievi, Holger Banzhaf, Felix Schmitt, and Peter Stone. Reward (mis)design for autonomous driving. 2021. arxiv:2104.13906.  

模型考虑偶然不确定性：使用潜变量$$z$$，枚举可能的轨迹

模型考虑认知不确定性：在reward中进行re-weighting和使用参数$$\beta$$

