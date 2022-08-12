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

作者用online DQN在60款 Atari 2600游戏上获取数据样本，然后用这些样本训练offline强化学习算法，一些offline的算法性能可以超过online的算法。本文提出的Random Ensemble Mixture (REM)算法在离线回放数据上的表现超过了强的基准算法。因此作者认为在离线样本足够多，多样化充分的情况下，使用鲁棒的RL算法可以获得高质量的策略。

### Introduction

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









![性能对比](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/20220804-.png)




### 总结

