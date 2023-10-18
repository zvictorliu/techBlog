---
title: "ROS基础知识"
date: 2023-10-18
tags:
 - "platform"
categories:
 - "Development"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2023/10/18/79c5191b4048edded72b54c4638d1493-fittosize__752_0_53ab91fb2e1755765c20d5d1df8d5f9d_l_ros_logo_3c_2018_08_1000x562-mobile-1596543825-1af651.jpg"
---

关于ROS需要知道的事情

<!--more-->

# ROS基础

Robot Operating System，但其实并非是运行在机器人上控制机器人的操作系统，而是一个软件框架，用以创建和管理机器人应用程序的（就相当于NAOqi OS和qi framework）

> The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications

在ROS中，不同的软件组件叫做节点Node，通过主题topics和服务services进行通信，模块化编程

它采用自己的构建工具：`catkin`

## ROS发行版

主要就是这三个：ROS Kinetic、ROS Melodic和ROS Noetic

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2023/10/18/bbc1936e2a8250d5a213f1c664fddf00-image-20231018125541342-c38f64.png" alt="image-20231018125541342" style="zoom: 50%;" />

## 基本概念

ROS采用点对点网络，称之为*计算图*

基本概念包括：*nodes*, *Master*, *Parameter Server*, *messages*, *services*, *topics*, and *bags*

这些数据结构在`ros_comm`库里面实现

- 节点Nodes: 执行计算处理的基本执行单元，可以理解为一个进程
- 主机Master：信息中心，充当服务器，管理节点通信
- 参数服务器：存储和共享节点的参数、配置信息
- 消息：节点传递数据的标准格式（可以理解为报文吧）
- 服务：节点之间*请求-响应*的通信方式
- 话题：节点之间*发布-订阅*，实现异步通信的方式
- 数据包：存档？

![http://ros.org/images/wiki/ROS_basic_concepts.png](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2023/10/18/fe480c0b0a52052aca6bf4efcaaf7181-ROS_basic_concepts-188707.png)

## RVIZ

`rviz`是ROS的一个三维可视化工具

## launch文件

定义启动配置的`xml`文件，按照这上面的说明进行启动，只是描述语言并不是脚本