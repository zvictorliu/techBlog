---
title: "RTK浮点解"
date: 2023-12-13T11:00:50+08:00
draft: false
categories:
- "theory"
- "GNSS"
tags:
- "RTK"
featuredImagePreview: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230605150849270.png"
---

介绍RTK定位的浮点解怎么来的

<!--more-->

## 非线性最小二乘

一个线性最小二乘方程的标准形式是：
$$
Y = AX + V
$$
$$A$$是设计矩阵，$$X$$是待估计参数，$$Y$$是测量量，$$V$$是残差，注意不要再带入直线拟合的符号标记

若带权（一般是方差倒数），解为：
$$
\hat{X} = (A^T R^{-1} A) ^{-1} A^T R^{-1} Y
$$
其中$$R^{-1}$$为权重矩阵

现在拓展到非线性最小二乘：

