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

- RL
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
  - Dan Wang, Junjie Wen, Yuyong Wang, Xiangdong Huang, and Feng Pei. End-to-End Self-Driving Using Deep Neural Networks with Multi-auxiliary Tasks. Automotive Innovation, 2(2):127–136, May 2019.
  - Zhengyuan Yang, Yixuan Zhang, Jerry Yu, Junjie Cai, and Jiebo Luo. End-to-end Multi-Modal Multi-Task Vehicle Controlfor Self-Driving Cars with Visual Perceptions. In Proc. of ICPR, pages 2289–2294, Beijing, China, Aug. 2018.












### 总结

