---
layout:     post
title:      "Collective Intelligence |  A Constructive Model for Collective Intelligence"
subtitle:   "群体智能主题，北大梅宏院士文章"
date:       2020-12-23
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Collective Intelligence
---
# A Constructive Model for Collective Intelligence

论文地址：https://academic.oup.com/nsr/article/7/8/1273/5831734

- collective intelligence (CI): 在很多社会昆虫中发现了自然地群体智能现象，即单个个体缺少智能，单它们组成的集体表现出很高的智能水平。

- 研究现状：
  - 不仅仅物理空间中存在CI现象，在网络空间也存在，例如线上合作开源软件开发（unanmous.ai系统），生物医学领域多人在线解决复杂蛋白质结构问题（EteRNA系统）。
  - 主要集中在如何发现、解释、干预这些CI现象。但缺少重要的理论支撑，帮助构建人工的CI系统。
- 一个建设性的模型：
  - 目标：解决基于大规模协作的复杂问题
  - Exploration-Integration-Feedback Loop (EIFL)
    - Exploration：每个个体自由地探索解决方案空间，并提供一组对解决问题有价值的信息片段
    - Integration：所有这些信息片段都被集成在一起，形成一组结构良好的，针对该问题的部分解决方案
    - Feedback：整合的结果会反馈给个体，刺激他们改善他们贡献的信息片段集
  - EIFL是增量、迭代、并行的求解过程，关键问题在于：
    1. 信息碎片如何表现
    
    2. 信息碎片如何集成
    
    3. 集成的结果如何反馈给群体
    
    4. 由谁来进行集成和反馈
  
- EIFL过程可以是人工或者半人工的。人类的知识通过书本集成，在各地发行，每个人可以去阅读学习，这就是一个EIFL过程

- 三个技术问题：
    - 如何定量评估CI现象是否出现，并衡量其CI程度。需要设计目标函数，以指导对EIFL的不同实现方式的探索。
    - 其它领域的模型和机制可以在CI系统中表示，并得到重用（reuse）。
    - 是否可以开发形式化理论来解决基于人类的进化问题。这方面研究可以发现CI系统的局限性，提供效率上的理论保证