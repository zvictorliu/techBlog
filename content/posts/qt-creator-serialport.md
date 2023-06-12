---
title: Qt 开发上位机程序
description: qt usage tutorial
lead: 显示程序开发
date: 2023-04-21
categories:
  - "Development"
tags:
  - "Qt"
  - "串口"

featuredImage: "https://picx.zhimg.com/v2-33907f5bcba1b1fcd3f8fd1bf95519b8_1440w.jpg?source=172ae18b"
---



利用`Qt creator` 开发一个上位机程序，能够实现显示单片机返回的数据，增加展示效果，但可调式性必然没有以及成熟的好

## 基本认识

Qt 生成一个主窗口，然后会陷入事件循环，直至用户关闭程序才会执行main函数后面的代码

Qt 生成的窗口样式是和操作系统相关的，Qt 可以设置一些属性，但一些特别的属性依赖于系统主题，Qt 无法控制，所以改了也没有用



QMainWindow继承自QWidget，增加了菜单栏、状态栏之类的东西，比如经常有看到的“文件、工具”等待选项，这些并非是必要的

在ui界面上的东西，是属于ui这个对象的成员

## 自定义窗口

如果完全自定义则关闭、最大化、最小化这些都要自己配置了

实在太麻烦，还是就这样了吧，win11的也不算丑了