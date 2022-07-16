---
layout:     post
title:      "Autonomous Driving | ChauffeurNet: Learning to Drive by Imitating the Best and Synthesizing the Worst (Waymo, 2018)"
subtitle:   ""
date:       2022-07-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## ChauffeurNet: Learning to Drive by Imitating the Best and Synthesizing the Worst

### 1. Introduction

- 提出通过扰动专家的驾驶数据创建更多有趣的驾驶情景，例如碰撞和off-road等。

- 在模仿学习中，并不是模仿所有的数据，而是利imitation loss惩罚不期望的情况。

- 数据：30 million real-world expert driving examples, corresponding to about 60 days of continual driving  
- 由于无论是原始传感器输入还是直接的控制器输出都很难产生扰动，因此在mid-level的输入输出数据上实施扰动。

### 2. Related Work

Decades-old work on ALVINN (Pomerleau (1989)) showed how a shallow neural network could follow the road by directly consuming camera and laser range data. Learning to drive in an end-to-end manner has seen a resurgence in recent years. Recent work by Chen et al. (2015) demonstrated a convolutional net to estimate affordances such as distance to the preceding car that could be used to program a controller to control the car on the highway. Researchers at NVIDIA (Bojarski et al. (2016, 2017)) showed how to train an end-to-end deep convolutional neural network that steers a car by consuming camera input. Xu et al. (2017) trained a neural network for predicting discrete or continuous actions also based on camera inputs. Codevilla et al. (2018) also train a network using camera inputs and conditioned on high-level commands to output steering and acceleration. Kuefler et al. (2017) use Generative Adversarial Imitation Learning (GAIL) with simple affordance-style features as inputs to overcome cascading errors typically present in behavior cloned policies so that they are more robust to perturbations. Recent work from Hecker et al. (2018) learns
a driving model using 360-degree camera inputs and desired route planner to predict steering and speed. The CARLA simulator (Dosovitskiy et al. (2017)) has enabled recent work such as Sauer et al. (2018), which estimates several affordances from sensor inputs to drive a car in a simulated urban environment. Using mid-level representations in a spirit similar to our own, M¨uller et al. (2018) train a system in simulation using CARLA by training a driving policy from a scene segmentation network to output high-level control, thereby enabling transfer learning to the real world using a different segmentation network trained on real
data. Pan et al. (2017) also describes achieving transfer of an agent trained in simulation to the real world using a learned intermediate scene labeling representation. Reinforcement learning may also be used in a simulator to train drivers on difficult interactive tasks such as merging which require a lot of exploration, as shown in Shalev-Shwartz et al. (2016). A convolutional network operating on a space-time volume of bird’s eye-view representations is also employed by Luo et al. (2018); Djuric et al. (2018); Lee et al. (2017) for tasks like 3D detection, tracking and motion forecasting. Finally, there exists a large volume of work on vehicle motion planning outside the machine learning context and Paden et al. (2016) present a notable survey.

### 3. Model Architecture

#### 3.1 Input Output Representation  

- a top-down coordinate system：
  - agent位姿$$P_t=(x_t,y_t)$$
  - 方向角$$\theta_t$$
  - 速度$$s_t$$ 
  - 图像大小：W × H pixels, $$\varphi$$ meters/pixel  
  - 因此agent只能看见前方的 $$R_{forward}=(H-v_0)\phi$$ 米

- 表征内容：
  - a. Roadmap:  3通道彩色图片，包含车道线、停止信号、人行道、路边等等。
  - b. Traffic lights：灰度图像的时间序列，其中该序列的每一帧表示在每个过去的时间步交通灯的已知状态。在每一帧中，我们用一个灰度级给每一个车道中心着色，最亮的灰度级代表红灯，中间灰度级代表黄灯，较暗的灰度级代表绿灯或未知灯。
  - c. Speed limit：单通道图像，并且车道中心的颜色与其速度限制成比例对应。
  - d. Route：生成的希望行驶的预定路线。
  - e. Current agent box：显示agent在当前时间步的完整边界框。
  - f. Dynamic objects in the environment：一个时序的图像序列，展示所有潜在的动态物体。（车辆、自行车、行人等）
  - g. Past agent poses：agent过去的姿态被渲染成一个单一的灰度图像，显示为位置点轨迹。

![Driving model inputs (a-g) and output (h)](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220716-1.png)

![Training the driving model](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220716-2.png)

- 用$$I$$表示上述枚举的各种输入，ChauffeurNet模型循环地预测未来的自车姿态，并用绿色点表示。
  $$
  P_{t+\delta t} = \text{ChauffeurNet}(I,P_t)
  $$

#### 3.2 Model Design  

- 

  

  
  $$
  P_k,B_k = \text{AgentRNN}(k,F,M_{k-1},B_{k-1})
  $$
  







