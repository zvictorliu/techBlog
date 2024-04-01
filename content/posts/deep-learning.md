---
title: 机器学习/深度学习入门
date: 2023-07-21
categories:
- "AI"
tags:
- "Machine Learning"
- "Deep Learning"
featuredImagePreview: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230605150849270.png"
---

base on 《Deep Learning with Python》(Python深度学习) by Francois Chollet

# Machine Learning

机器学习思想和传统编程的区别：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721100812858.png" alt="image-20230721100812858" style="zoom:67%;" />

根据数据的答案发现统计规律，这个过程叫做`training`，而传统的是`programming`，即遵循程序员写定的`rules`完成输入到输出

但是，这种统计规律是传统数量统计难以分析的，它并非像是一门数学，而是一种工程经验，实践结果永远大于理论推导

根据一个指标衡量现在的输出和理想输出的距离，用它来自我调整，这个过程就是`learning`

机器学习是在对数据进行有意义地变换，因此需要找到一种有力的数据的表示`representation`方法

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721102005370.png" alt="image-20230721102005370" style="zoom:67%;" />

即经过坐标变换后的数据的新的表示，能够更容易的进行分类任务

机器学习则是自动搜索有用的表示方法，并用评价指标作为反馈进行调整，使之能够用简单的规则完成任务

搜索预定义的一组操作，即假设空间`hypothesis space`

> searching for useful representations and rules over some input data, within a predefined space of possibilities, using guidance from a feedback signal.
>
> 在预先定义好的可能性空间中，利用反馈信号的指引来寻找输入数据的有用表示

## Deep Learning

deep指的是连续的表示层，即深度学习代表了一种“分层表示”的思想

深度学习一般包括数十个甚至上百个层，而其它机器学习方法通常只有一两层，叫做浅层学习

一般是通过神经网络模型来学习得到这些分层的

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721104952013.png" alt="image-20230721104952013" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721105007196.png" alt="image-20230721105007196" style="zoom: 50%;" />

> a multistage way to learn data representations
>
> 学习数据表示的多级方法

一个层对输入的操作体现在其权重上面，权重就是这个层的参数，神经网络就是寻找合适的参数

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721105759712.png" alt="image-20230721105759712" style="zoom:67%;" />

评价指标是loss fuction，通过loss衡量与理想结果的距离作为反馈进行调整，把这个调整操作看做一个优化器来做的

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230721110000882.png" alt="image-20230721110000882" style="zoom:67%;" />

而优化器做的调整，就是`梯度下降算法`，是这个让神经网络有了强大的生命力，这种算法的来由另一篇帖子就讲过了

## 其它机器学习算法

机器学习不只有深度学习这一种，还有很多其它的模型