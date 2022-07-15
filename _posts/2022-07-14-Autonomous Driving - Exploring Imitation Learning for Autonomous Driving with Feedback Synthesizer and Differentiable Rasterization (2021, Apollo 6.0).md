---
layout:     post
title:      "Exploring Imitation Learning for Autonomous Driving with Feedback Synthesizer and Differentiable Rasterization (2021, Apollo 6.0)"
subtitle:   ""
date:       2022-07-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---
## Exploring Imitation Learning for Autonomous Driving with Feedback Synthesizer and Differentiable Rasterization (2021, Apollo 6.0)

### Introduction

- our work adopts a **mid-to-mid approach** (mid2mid允许便利地增加数据，并且通过任务依赖的loss超过单纯地模仿。allows us to augment data handily and go beyond pure imitation by having task-specific losses) where our system’s input is constructed by building a top-down image representation of the environment that incorporates both static and dynamic information from our **HD Map** and **perception system**.  
- Following the philosophy of **DAgger** [2], we introduce a **feedback synthesizer**（反馈合成器）that generates and perturbs on-policy data based on the current policy.  Then we train the next policy on the aggregate of collected datasets. The feedback synthesizer addresses the **distributional shift issue**,
  thus improving the overall performance shown in Section IV.
  - [2]. S. Ross, G. Gordon, and D. Bagnell, “A reduction of imitation learning and structured prediction to no-regret online learning,” in AISTATS, 2011, pp. 627–635.  
- 启发本文的文章：
  - V. Blukis, N. Brukhim, A. Bennett, R. Knepper, and Y. Artzi, “Following high-level navigation instructions on a simulated quadcopter with imitation learning,” in RSS, June 2018.
  - M. Bansal, A. Krizhevsky, and A. Ogale, “ChauffeurNet: Learning to drive by imitating the best and synthesizing the worst,” in RSS, 2019.
  - A. Buhler, A. Gaidon, A. Cramariuc, R. Ambrus, G. Rosman, and W. Burgard, “Driving through ghosts: Behavioral cloning with false positives,” in IROS, 2020.
  - D. Chen, B. Zhou, V. Koltun, and P. Krahenbuhl, “Learning by cheating,” in CoRL, 2020, pp. 66–75.  
- 单纯的模仿学习没有关于任务的明确的重要目标和约束，因此难免会学习到非期望的行为。通过引入task losses可以控制这些非期望的行为。

- task losses直接映射到输出轨迹到车辆光栅图像中。The task losses are implemented by directly projecting the output trajectories into top-down images using a differentiable vehicle rasterizer.  

- They（task losses） effectively penalize behaviors, such as **obstacle collision**（障碍碰撞）, **traffic rule violation**（违反交规）, and so on, by building losses between **rasterized images** and **specific object masks**.  

- Inspired by recent works [9], our output trajectories are produced by a **trajectory decoding module** that includes an **LSTM** [10] and a **kinematic layer** that assures our output trajectories’ feasibility.  On the whole, these designs help us  avoid using the heavy AgentRNN network in Chauffeurnet [5], which functions similarly to a Convolutional-LSTM network [11].  

  - [5]. M. Bansal, A. Krizhevsky, and A. Ogale, “ChauffeurNet: Learning to drive by imitating the best and synthesizing the worst,” in RSS, 2019.  

  - [9]. H. Cui, T. Nguyen, F.-C. Chou, T.-H. Lin, J. Schneider, D. Bradley, and N. Djuric, “Deep kinematic models for kinematically feasible vehicle trajectory predictions,” in ICRA, 2020, pp. 10 563–10 569. 
  - [11]. X. Shi, Z. Chen, H. Wang, D.-Y. Yeung, W.-k. Wong, and W.-c. Woo, “Convolutional LSTM network: A machine learning approach for precipitation nowcasting,” in NeurIPS, vol. 28, 2015, pp. 802–810.  

- Moreover, to further improve the performance, similar to recent works [12], [13], we introduce a **spatial attention module**（空间注意力机制） in our network design.  
  - [12]. S. Hecker, D. Dai, A. Liniger, and L. Van Gool, “Learning accurate and human-like driving using semantic maps and attention,” in IROS, 2020, pp. 2346–2353.
  - [13].  J. Kim and M. Bansal, “Attentional bottleneck: Towards an interpretable deep driving network,” in CVPR Workshops, 2020.  
- 为了提高舒适度，提出了可选择的后处理规划器作为看门人，进行高级别的决策引导和组成新的轨迹。we propose to add an **optional post-processing planner** as a gatekeeper which manages to interpret them as high-level decision guidance and composes a new trajectory that offers better comfort.  

- Our models are trained with **400 hours of human driving data**. We evaluated our system using **70 autonomous driving test scenarios (ADS)** that are specifically created for evaluating the fundamental driving capabilities of a self-driving vehicle.  

- We show that our **learning-based planner (M2)** trained via imitation learning achieves **70.0% ADS**
  **pass rate** and can intelligently handle different challenging scenarios, including **overtaking a dynamic vehicle**, **stopping for a red traffic light**, and so on, as shown in Figure 1.

### Related works

- **Imitation Learning**：

  一般遵循end-to-end philosophy，本文采取mid-to-mid approach提高数据增强便利性和任务依赖的loss  

- **Loss and Differentiable Rasterization**：

  Imitation learning for motion planning typically applies a loss between **inferred** and **ground truth trajectories**. Therefore, the ideas of avoiding collisions or off-road situations are implicit and don’t generalize well.  

  Wang et al. [8] leverage a differentiable rasterizer, and it allows gradients to flow from a discriminator to a generator, enhancing a trajectory prediction network powered by GANs [21].  

  **General-purpose differentiable mesh renderers** [23], [24] have also been employed to solve other computer vision tasks.

- **Attention**：

  by providing spatial attention heatmaps highlighting image areas that the network attends
  to in their tasks. In this work, we introduce the Atrous Spatial Attentional Bottleneck from [13], providing easily interpretable attention heatmaps while also enhancing the network’s performance.  

  - [13]. J. Kim and M. Bansal, “Attentional bottleneck: Towards an interpretable deep driving network,” in CVPR Workshops, 2020.  

- **Data Augmentation**：

  DAgger及其变体提出通过拥有更多关于代理可能遇到的状态的数据来解决分布转移问题。特别是，他们基于从当前策略推断的动作在每次迭代中采样新状态，让专家代理演示他们在这些新状态下将采取的动作，并在收集的数据集的集合上训练下一个策略。DAgger [2] and its variants [32], [4], [3] propose to address the distributional shift issue by having more data with states that the agent is likely to encounter. In particular, they sample new states at each iteration based on the actions inferred from the current policy, let expert agents demonstrate the actions they would take under these new states, and train the next policy on the aggregate of collected datasets.  

  - S. Ross, G. Gordon, and D. Bagnell, “A reduction of imitation learning and structured prediction to no-regret online learning,” in AISTATS, 2011, pp. 627–635.  

  ChauffeurNet引入了一个随机合成器，通过合成轨迹的扰动来增加演示数据。ChauffeurNet [5] introduces a random synthesizer that augments the demonstration data by synthesizing perturbations to the trajectories. In this work, we explore both ideas and propose a feedback synthesizer improving the overall performance.  

### Model Architecture

B-CNN（branched CNN）

- refence：Zhu X , Bain M . B-CNN: Branch Convolutional Neural Network for Hierarchical Classification[J]. 2017.

- **Model Input**：
  - 使用栅格化的多通道鸟瞰图。bird‘s eye view (BEV) representation with multiple channels by scene rasterization
  - The image size is W × H with ρ meters per pixel in resolution.
  - agent汽车的位置永远在图像的中心位置 $$p_0 = [i_0,j_0]^T$$ ，这种图片模式称为ego-centered
  - 模型输入$$\mathcal{I}$$是多通道图像，不仅包括ego-vehicle、路况信息、还有车辆速度信息$$v_0$$
  
- **Model Design**：

  - 整个模型分为三个部分：

    - A. 一个带空间注意力分支的CNN模型
    - B. 一个LSTM decoder
    - C. 一个可微分的光栅化模块（加在LSTM decoder上）  

  - **A.** CNN骨架模型使用MobileNetV2 [37]以平衡输出精度和推断速度。输出特征是$$F_h$$，经过一个MLP（multilayer perceptron）层之后，输出扁平的特征$$h_0$$。该特征作为同时作为LSTM decoder的初始隐藏状态被使用。

    为了减轻计算量，主干CNN的中间特征$$F_I$$传递给空间注意力模块。注意力使用的是Atrous Spatial Attentional Bottleneck from [13]  （J. Kim and M. Bansal, “Attentional bottleneck: Towards an interpretable deep driving network,” in CVPR Workshops, 2020）

  - **B.** LSTM的 cell state $$c_0$$ is initialized by the Glorot initialization [38]  

    模型输出是汽车的转向角序列（steering angle）和加速度序列，记为$$(\delta_{t-1},a_{t-1})$$。

    动力学层：
    $$
    \begin{align}
    \begin{cases}
    	x_t=v_{t-1}\sin (\phi_{t-1})\Delta t + y_{t-1}\\
    	y_t=v_{t-1}\cos (\phi_{t-1})\Delta t + x_{t-1}\\
    	\phi_t=v_{t-1}\frac{\tan (\delta_{t-1})}{L} \Delta t + \phi_{t-1}\\
    	v_t=a_{t-1}\Delta t + v_{t-1}
    \end{cases}
    \end{align}
    $$
    
  - **C.** Differentiable Rasterizer使用三个高斯基函数来描述汽车的形状。
  
    光栅化函数：$$g_{i,j}(s_t)=\max_{k=1,2,3} (N(\mu^k,\Sigma^k))$$
    $$
    \begin{align}
    \begin{cases}
    \mu^k=\frac{1}{\rho}(x_t^k-x_0)+P_0\\
    \Sigma^k=R(\phi_t)^T \text{diag}(\sigma_l,\sigma_{\omega})R(\phi_t)
    \end{cases}
    \end{align}
    $$
    其中，$$(\sigma_l,sigma_{\omega})=(\frac{1}{3}\alpha l,\alpha \omega)$$，$$\alpha$$是固定的比例系数，$$l$$和$$\omega$$是车辆的长度和宽度。$$R(\phi_t)$$表示旋转矩阵。
  
- **Loss：**

  trajectory imitation loss：$$L_{imit}=\sum^{N-1}_{t=0}\lambda\mid\mid s_t - \hat{s}_t\mid\mid_2$$

  four task losses：

  - obstacle collision：$$L_{obs}=\sum^{N-1}_{t=0}\frac{1}{WH}\sum_i\sum_j g_{i,j} \tau_{i,j}^{obs}$$
  
  - off-route：$$L_{route}=\sum^{N-1}_{t=0}\frac{1}{WH}\sum_i\sum_j g_{i,j} \tau_{i,j}^{route}$$
  
  - off-road：$$L_{road}=\sum^{N-1}_{t=0}\frac{1}{WH}\sum_i\sum_j g_{i,j} \tau_{i,j}^{road}$$
  
  - traffic signal violation：$$L_{signal}=\sum^{N-1}_{t=0}\frac{1}{WH}\sum_i\sum_j g_{i,j} \tau_{i,j}^{signal}$$
    其中，$$\tau^{obs}$$, $$\tau^{route}$$, $$\tau^{road}$$, and $$\tau^{signal}$$ 是响应的二值掩码（binary masks） 
    
  - 总的损失函数：
  
    $$L=L_{imit}+\lambda_{task}(L_{obs}+L_{route}+L_{road}+L_{signal})$$

### Data Augmentation  

- **Random Synthesizer**：随机扰动轨迹，产生off-road和碰撞的场景。起始点和终止点保持不变，扰动中间过程的**一个**点，并平滑路径轨迹，通过对最大曲率进行阈值处理，仅保留真实的扰动轨迹。

- **Feedback Synthesizer**：学习一个驾驶策略帮助生成新的轨迹数据。用上一次迭代的策略$$\pi_{t-1}$$生成轨迹数据，用于训练策略$$\pi_t$$。具体算法如下：

  ![Feedback Synthesizer](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220714-1.png)

- **Post-processing Planner**：保证舒适和安全。相关实现在 Apollo 6.0 中（https://github.com/ApolloAuto/apollo/tree/master/modules/planning）
  $$
  \text{Quadratic Optimization} \leftarrow
  \begin{cases}
  \text{Safety Bounds}\\
  \text{Learning-based Objective}\\
  \text{Comfort Constrains}
  \end{cases}
  $$

### Experiments

- **Implementation Details**

  方形的BEV图（俯瞰图）宽度$$W\times H=200\times200$$，$$\rho=0.2m/pixel$$。ego-vehicle位于图片的$$i_0=100,j_0=160$$的位置。使用Apollo的感知模块（“Baidu Apollo open platform,” http://apollo.auto/）光栅化2秒的历史数据或者预测数据。使用Adam optimizer，且初始学习率为$$0.0003$$。

- **Dataset and Augmented Data**  

  - 400 hours’ driving data demonstrated by human-drivers in southern San Francisco bay area。

  - 经过数据预处理后，保留了250k帧作为原始的训练数据$$D_0$$
  - 利用random synthesizer生成400k帧数据，记为$$D_r$$
  - 设置feedback steps T = 5，feedback synthesizer生成465k帧数据，记为$$D_f$$  
  
  ![不同模型配置的性能对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220714-2.png)
  
  ![offroad,speeding,collision,and failed to arrive性能对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220714-3.png)
  
- **Evaluation Scenarios**

  使用Apollo Dreamland simulator。主要包括4类场景：

  - **巡航 Cruising**: normal cruising in straight or curved roads without other traffic participants.
  - **岔道口 Junction**: junction related scenarios including left or right turns, U-turns, stop before a traffic signal, etc.
  - **静态交互 Static Interaction**: interaction with static obstacles, such as overtaking a stopped car.
  - **动态交互 Dynamic Interaction**: interaction with dynamic obstacles, for example, overtaking a slow vehicle.  

- **Evaluation Metrics**  

  除了通过率和成功率以外，还使用了舒适度评分。舒适度评分是根据自动驾驶状态和人类驾驶状态的相识度计算得到的。

  在人驾数据集$$D_0$$中还包括了角速度和角加加速度$$(\omega,j)$$，使用这两个数据，可得舒适度评分为：$$c=\frac{\sum_{i=1}^N P(\omega,j\mid D_0)}{n}$$。其中，$$P(\omega,j\mid D_0)$$表示状态$$(\omega,j)$$在人驾数据$$D_0$$中出现的概率，$$n$$表示帧数。$$\omega$$和$$j$$都经过离散化处理（0.1 and 1.0），直接查表读取响应的概率。

  agent被要求在时间限制之内到达目的地，同时避免碰撞等事故（出现则判定失败）。

- **Runtime Analysis**

  an Nvidia Titan-V GPU, Intel Core i7-9700K CPU, and 16GB Memory.  

  The online inference time per frame is **10ms** in rendering, **22 ms** in model inference, and **15 ms** (optional) in the post-processing planner. Note that our model inference time is much shorter than the prior work [5]  



模型出现的主要问题是超速、不能到达和碰撞。超速可以通过添加速度损失来解决，不能到达是由于速度过慢，碰撞多是出现了追尾，因此发生被动碰撞。值得注意的是，实验中其它的车辆都是按照既定轨迹运动的，不会避让ego车辆，因此容易出现追尾风险。

