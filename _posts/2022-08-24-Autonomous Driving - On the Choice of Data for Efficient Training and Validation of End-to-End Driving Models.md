---
layout:     post
title:      "Autonomous Driving | On the Choice of Data for Efficient Training and Validation of End-to-End Driving Models"
subtitle:   ""
date:       2022-08-24
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Imitation Learning
    - Autonomous Driving
---

## On the Choice of Data for Efficient Training and Validation of End-to-End Driving Models

本文关注数据集的设计，包括针对自动驾驶端到端模型训练集和验证集。

主要工作：

- 调研训练数据量如何影响最终驾驶表现，以及通过当前使用的生成训练数据的机制导致了哪些表现限制；
- 相关性分析表明，验证设计使得在验证期间测量的驾驶性能能够很好地推广到未知的测试环境；
- 调查了随机种子和不确定性的影响，给出了哪些报告的改进可以被认为是显著的；

![本文关注数据而非模型](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-1.png)

### 1 Introduction

- Towards End-to-End Deep Driving: 高维场景信息输入，训练自动驾驶算法。

- Training of Deep Driving Models：端到端的自动驾驶模型的初衷是移除人工的中间表征。
  - Felipe Codevilla, Matthias Müller, Antonio López, Vladlen Koltun, and Alexey Dosovitskiy. **End-to-end Driving via Conditional Imitation Learning**. In Proc. of ICRA, pages 4693–4700, Brisbane, Australia, May 2018.
  - Kashyap Chitta, Aditya Prakash, and Andreas Geiger. NEAT: **Neural Attention Fields for End-to-End Autonomous Driving**. In Proc. of ICCV, pages 15793–15803, Virtual, Oct. 2021.
  - Keishi Ishihara, Anssi Kanervisto, Jun Miura, and Ville Hautamaki. **Multi-task Learning with Attention for End-to-end Autonomous Driving**. In Proc. of CVPR - Workshops, pages 2902–2911, Virtual, June 2021

- Evaluation of Deep Driving Models：可以开环和offline专家对比，也可以闭环仿真评估，但是评估是非确定性的，不会执行两次相同的评估。
  - Jeffrey Hawke, Richard Shen, Corina Gurau, Siddharth Sharma, Daniele Reda, Nikolay Nikolov, Przemysław
    Mazur, Sean Micklethwaite, Nicolas Griffiths, Amar Shah, and Alex Kendall. **Urban Driving with Conditional Imitation Learning**. In Proc. of ICRA, pages 251–257, Virtual, May 2020.

- Contributions:
  - 调研了end2end模型的训练集大小对性能的影响
  - 分析了end2end模型的局限性
  - 为驾驶模型的验证集设计提供了建议
  - 调研了random seed和非确定性end2end模型

### 2 Related Work

#### Training of End-to-End Deep Driving Models

- Reinforcement Learning
  - Alex Kendall, Jeffrey Hawke, David Janz, Przemyslaw Mazur, Daniele Reda, John-Mark Allen, Vinh-Dieu Lam,
    Alex Bewley, and Amar Shah. **Learning to Drive in a Day**. In Proc. of ICRA, pages 8248–8254, Montréal, Canada, May 2019.
  - Marin Toromanoff, Emilie Wirbel, and Fabien Moutarde. **End-to-End Model-Free Reinforcement Learningfor Urban Driving using Implicit Affordances**. In Proc. of CVPR, pages 7153–7162, Virtual, June 2020.
  - Dian Chen, Vladlen Koltun, and Philipp Krähenbühl. **Learning to Drive from a World on Rails**. In Proc. of ICCV, pages 15590–15599, Virtual, Oct. 2021

- two-stage training: 先训练一个专家，然后将知识传递给自动驾驶智能体
  - Dian Chen, Brady Zhou, Vladlen Koltun, and Philipp Krähenbühl. **Learning by Cheating**. In Proc. of CoRL, pages 66–75, Virtual, Nov. 2020.
  - Jiaming Zhang, Kailun Yang, Angela Constantinescu, Kunyu Peng, Karin Müller, and Rainer Stiefelhagen. **Trans4Trans: Efficient Transformer for Transparent Object Segmentation To Help Visually Impaired People Navigate in the Real World**. In Proc. of ICCV - Workshops, pages 1760–1770, Virtual, Oct. 2021

- inverse reinforcement learning  
  - Sascha Rosbach, Vinit James, Simon Großjohann, Silviu Homoceanu, and Stefan Roth. **Driving with Style: Inverse Reinforcement Learning in General-PurposePlanning for Automated Driving**. In Proc. of IROS, pages 2658–2665, Macau, China, Nov. 2019.
  - Sahand Sharifzadeh, Ioannis Chiotellis, Rudolph Triebel, and Daniel Cremers. **Learning to Drive using Inverse Reinforcement Learning and Deep Q-Networks**. In Proc. of NIPS Workshops, pages 1–7, Barcelona, Spain, Dec. 2016.

- LSTM
  - Huazhe Xu, Yang Gao, Fisher Yu, and Trevor Darrell. **End-to-end Learning of Driving Models from Large-scale Video Datasets**. In Proc. of CVPR, pages 2174–2182, Honolulu, HI, USA, July 2017.
- self-attention
  - Keishi Ishihara, Anssi Kanervisto, Jun Miura, and Ville Hautamaki. **Multi-task Learning with Attention for End-to-end Autonomous Driving**. In Proc. of CVPR - Workshops, pages 2902–2911, Virtual, June 2021.
- multi-task networks
  - Dan Wang, Junjie Wen, Yuyong Wang, Xiangdong Huang, and Feng Pei. **End-to-End Self-Driving Using Deep Neural Networks with Multi-auxiliary Tasks**. Automotive Innovation, 2(2):127–136, May 2019.
  - Zhengyuan Yang, Yixuan Zhang, Jerry Yu, Junjie Cai, and Jiebo Luo. **End-to-end Multi-Modal Multi-Task Vehicle Controlfor Self-Driving Cars with Visual Perceptions**. In Proc. of ICPR, pages 2289–2294, Beijing, China, Aug. 2018.

- The fusion of different input modalities
  - Aditya Prakash, Kashyap Chitta, and Andreas Geiger. **Multi-Modal Fusion Transformer for End-to-End Autonomous Driving**. In Proc. of CVPR, pages 7077–7087, Virtual, June 2021.
- affordances
  - Axel Sauer, Nikolay Savinov, and Andreas Geiger. **Conditional Affordance Learning for Driving in Urban Environments**. In Proc. of CoRL, pages 237–252, Zürich, Switzerland, Oct. 2018.
-  waypoints 替代速度
  - Kashyap Chitta, Aditya Prakash, and Andreas Geiger. NEAT: **Neural Attention Fields for End-to-End Autonomous Driving**. In Proc. of ICCV, pages 15793–15803, Virtual, Oct. 2021.

- probabilistic output
  - Alexander Amini, Guy Rosman, Sertac Karaman, and Daniela Rus. **Variational End-to-End Navigation and Localization**. In Proc. of ICRA, pages 8958–8964, Montréal, QC, Canada, May 2019.

- sim-2-real
  - Blażej Osiński, Adam Jakubowski, Pawel Ziecina, Piotr Miloś, Christopher Galias, Silviu Homoceanu, and Henryk Michalewski. **Simulation-Based Reinforcement Learningfor Real-World Autonomous Driving**. In Proc. of ICRA, pages 6411–6418, Virtual, May 2020
  - GAN-based
    - Matthias Müller, Alexey Dosovitskiy, Bernard Ghanem, and Vladlen Koltun. **Driving Policy Transfer via Modularity and Abstraction**. In Proc. of CoRL, pages 1–15, Zürich, Switzerland, Oct. 2018.
    - Luona Yang, Xiaodan Liang, Tairui Wang, and Eric Xing. **Real-to-Virtual Domain Unification for End-to-**
      **EndAutonomous Driving**. In Proc. of ECCV, pages 530–545, Munich, Germany, Sept. 2018.
- attention
  - Bob Wei, Mengye Ren, Wenyuan Zeng, Ming Liang, Bin Yang, and Raquel Urtasun. **Perceive, Attend, and Drive: Learning Spatial Attention for Safe Self-Driving**. In Proc. of ICRA, pages 4875–4881, Virtual, May 2021.
  - Luca Cultrera, Lorenzo Seidenari, Federico Becattini, Pietro Pala, and Alberto Del Bimbo. **Explaining Autonomous Driving by Learning End-to-End Visual Attention**. In Proc. of CVPR - Workshops, pages 340–341, Virtual, June 2020.

- intermediate semantic representation
  - Abbas Sadat, Sergio Casas, Mengye Ren, Xinyu Wu, Pranaab Dhawan, and Raquel Urtasun. **Perceive, Predict, and Plan: Safe Motion Planning Through Interpretable Semantic Representations**. In Proc. of ECCV, pages 414–430, Virtual, Aug. 2020.

- on-line data selection techniques (视觉输入情况)
  - Aditya Prakash, Aseem Behl, Eshed Ohn-Bar, Kashyap Chitta, and Andreas Geiger. **Exploring Data Aggregation in Policy Learning for Vision-based Urban Autonomous Driving**. In Proc. of CVPR, pages 11763–11773, Virtual, June 2020.
  - Soumi Das, Harikrishna Patibandla, Suparna Bhattacharya, Kshounis Bera, Niloy Ganguly, and Sourangshu Bhat tacharya. **TMCOSS: Thresholded Multi-Criteria Online Subset Selection forData-Efficient Autonomous Driving**. In Proc. of ICCV, pages 6341–6350, Virtual, Oct. 2021. 2

#### Evaluation of End-to-End Deep Driving Models

- CARLA benchmarks CoRL2017 [20]
  - Alexey Dosovitskiy, German Ros, Felipe Codevilla, Antonio Lopez, and Vladlen Koltun. CARLA: An Open Urban Driving Simulator. In Proc. of CoRL, pages 1–16, Mountain View, CA, USA, Nov. 2017.
- NoCrash [17]
  - Felipe Codevilla, Eder Santana, Antonio M. López, and Adrien Gaidon. Exploring the Limitations of Behavior
    Cloning for Autonomous Driving. In Proc. of ICCV, pages 9329–9338, Seoul, Korea, Oct. 2019.
- Leaderboard [1]
  - CARLA Autonomous Driving Leaderboard. https://leaderboard.carla.org, 2020. 3, 5, 6

### 3 End-to-End Deep Driving

- 文章设置了一个城市驾驶环境的点到点导航任务，预先定义一个稀疏点的路线为粗略的行驶路线，车辆需要避免违法和碰撞，从初始点驾驶到终止点（CARLA Leaderboard标准配置）。

- 输入：一个RGB前置相机图像、一个LiDAR点云

![End-to-end driving method](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-2.png)

- 使用Conditional Imitation Learning

- 不同训练集设置情况：

  ![Training set design](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-3.png)

- 验证集和测试集路线类型

![Validation and test route types](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-4.png)

- 不同验证集和测试集设置情况

![Validation and test design](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-5.png)

- 不同数据集上的驾驶得分$$DS\in[0,1]$$，包括两部分分数：

  - 完成路线  route completion percentage $$RC\in [0, 1]$$;

  - 避免事故  infraction score $$IS \in [0, 1]$$;

![Driving scores](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-6.png)

- 不同训练集和验证集的性能对比

![Pearson correlation between the validation set performance and the test set performance (given by the driving score)](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-7.png)

- 不同验证集下的测试集得分对比

![Test driving scores given in (%) obtained on R^test, having used different validation sets](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-8.png)

- 不同训练集下的性能对比

![Performance on different training sets](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220824-9.png)

- 结论（不完整）：

  - 随机性对仿真实验的影响大。随机种子、carla等。
  - 训练集和验证集的选择对结果影响大。
  - 数据量从100k-220k变化时，对大约160，000幅图像的训练似乎已经在性能和复杂性之间提供了良好的平衡，同时使用更大但计算上更昂贵的数据量可以获得最佳结果。（Training on approximately 160, 000 images already seems to provide a good trade-off between performance and complexity, while best results are obtained using larger but computationally more expensive amounts of data.）。
  - 数据越少路线完成率越高，但事故率也高；数据越多，事故率显著降低，但路线完成率也降低；

  - 专家数据是必要的。不完美的专家数据对结果影响大

### 总结

总的来说比较玄学，对于视觉输入的模型而言，数据量偏小（100k-220k），给出的经验参数不具有普遍意义。
