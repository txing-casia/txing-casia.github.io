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

- 用$$I$$表示上述枚举的各种输入，ChauffeurNet模型循环地预测未来的自车姿态，并用绿色点表示。
  $$
  P_{t+\delta t} = \text{ChauffeurNet}(I,P_t)
  $$

#### 3.2 Model Design  

![Training the driving model](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220716-2.png)

- **a convolutional feature network (FeatureNet)：**

  - 输入：input data
  - 输出：处理后的上下文特征表征（a digested contextual feature representation）
- **a recurrent agent network (AgentRNN)：**

  - 输入：处理后的特征（consumed features）
  - 输出：迭代地预测连续的路径点（iteratively predicts successive points）以及其它信息

  - AgentRNN还将车辆的边界框预测为每个未来时间步的空间热图。（The AgentRNN also predicts the bounding box of the vehicle as a spatial heatmap at each future timestep）  
  - point $$P_k$$ 
  - the agent bounding box heatmap $$B_k$$  
  - memory  $$M_k$$
  - FeatureNet  输出的特征$$F$$
  - $$P_k,B_k = \text{AgentRNN}(k,F,M_{k-1},B_{k-1})$$

- **Road Mask Network：**

  - 输入：feature representation  

  - 输出：预测可行驶的区域（predicts the drivable areas of the field of view (on-road vs. off-road)）

- **recurrent perception network (PerceptionRNN)：**

  - 输入：feature representation  

  - 输出：迭代预测空间热图（iteratively predicts a spatial heatmap (of every other agent in the scene))  
  

#### 3.3 System Architecture  

![Software architecture for the end-to-end driving pipeline](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220718-1.png)

### 4. Imitating the Expert

- AgentRNN在每次迭代中预测三个输出：
  - 概率分布$$P_k(x,y)$$
  - 预测边界的热图$$B_k(x,y)$$
  - 回归边界朝向输出$$\theta_k$$ (a regressed box heading output $$\theta_k$$)  

- 定义上述三个变量响应的损失函数：(上标$$gt$$表示相应的ground-truth值；$$H(a,b)$$表示交叉熵函数；$$P^{gt}_k$$是二值图像，并且只有ground-truth目标坐标取值为1)

  - $$L_p=H(P_k,P^{gt}_k)$$
  - $$L_B=\frac{1}{WH}\sum_x \sum_y H(B_k(x,y),B_k^{gt}(x,y))$$
  - $$L_{\theta}=\mid\mid \theta_k - \theta_k^{gt} \mid\mid_1$$

- 精细的位置预测用$$\delta P_k^{gt}$$表示，并用$$L_1$$损失计算误差：

  $$L_{p-subpixel} = \mid\mid \delta P_k-\delta P_k^{gt} \mid\mid_1$$

  $$L_{speed}=\mid\mid s_k-s_k^{gt} \mid\mid_1$$

  其中，$$\delta P^{gt}_k = P^{gt}_k-\lfloor P^{gt}_k \rfloor$$是ground-truth位置坐标的小数部分（the fractional part）

### 5. Beyond Pure Imitation

- 合成扰动（Synthesizing Perturbations），路径的起始点和终点不变，晃动中间某个点的位置，幅度是$$[-0.5,0.5]$$米，扰动的车头朝向角度为$$[-\pi/3,\pi/3]$$弧度。最后结合扰动点和端点，拟合出一条平滑的轨迹，让车能在干扰之后回到原来的轨迹。
- 通过与最大曲率阈值比较，去除生成的一些不切实际的轨迹
- 允许生成的轨迹与其它车辆发生碰撞的情况，这些cases可以帮助训练避免这些情况。

- Beyond the Imitation Loss：

  - **Collision Loss**

    直接测量预测的智能体边框$$B_k$$和ground-truth场景中其它目标边框的重叠区域（directly measures the overlap of the predicted agent box Bk with the ground-truth boxes of all the scene objects at each timestep）
    $$
    L_{collosion}=\frac{1}{WH}\sum_x \sum_y B_{k}(x,y)\cdot Obj^{gt}_k (x,y)
    $$
    其中，$$B_k$$是输出的预测的智能体边框似然度地图，$$Obj^{gt}_k$$是一个二值掩码图，其中其它的动态目标用1表示

  - **On road loss**

    避免agent冲出道路边界
    $$
    L_{onroad}=\frac{1}{WH}\sum_x \sum_y B_k(x,y)\cdot (1-Road^{gt}(x,y))
    $$

  - **Geometry loss**

    不与目标几何位置重叠的区域作为一个惩罚项损失
    $$
    L_{geom}=\frac{1}{WH}\sum_x \sum_yB_k(x,y)\cdot(1-Geom^{gt}(x,y))
    $$

  - **Auxiliary losses**

    使用a recurrent perception network PerceptionRNN预测他车轨迹
    $$
    L_{objects}=\frac{1}{WH}\sum_x \sum_y H(Obj_k (x,y),obj^{gt}_k(x,y))
    $$

- 本文使用的损失函数用两组损失组成：模仿损失（imitation losses）和环境损失（environment losses）：

  - 模仿损失：$$L_{imit}=\{L_p,L_B,L_{\theta},L_{p-subpixel},L_{speed}\}$$
  - 环境损失：$$L_{env}=\{L_{collision},L_{onroad},L_{geom},L_{objects},L_{road}\}$$ 
  - 总损失：$$L=\omega_{imit}\sum_{l\in L_{imit}}l +\omega_{env}\sum_{l \in L_{env}} l$$

  模仿损失导致模型模仿专家的演示，而环境损失阻止不期望的行为，例如碰撞。(The imitation losses cause the model to imitate the expert’s demonstrations, while the environment losses discourage undesirable behavior such as collisions.)

  ![Visualization of predictions and loss functions on an example input](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220718-2.png)

- 参数情况：

| $$T_{scene}$$ | $$T_{pose}$$ | $$\delta t$$ |  $$N$$  | $$\Delta$$ |
| :-----------: | :----------: | :----------: | :-----: | :--------: |
|     1.0 s     |    8.0 s     |    0.2 s     |   10    |   25 deg   |
|     $$W$$     |    $$H$$     |   $$u_0$$    | $$v_0$$ |  $$\phi$$  |
|    400 px     |    400 px    |    200 px    | 320 px  |  0.2 m/px  |

