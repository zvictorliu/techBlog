---
title: "NI DAQ使用教程"
date: 2023-10-04
tags:
 - "NI"
categories:
 - "Development"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20231004131822597.png"
---

NI DAQ以及相关软件

<!--more-->

## NI系列工具

现在的NI换了新logo，各方面也更现代化

NI现在使用Manager的思想，有Package Manager、Liscense Manager、启动器等，管理所有NI产品

~~破解也是一个工具可以破解全部：NI Liscense Activator~~

有一些软件是收费的（如Labview)，无法在Manager上搜到，在官网上也无法直接下载

可以直接下载的软件可以以online通过Manager安装，也可offline安装，通常可以选择安装路径，不过若是此前安装过NI产品在一个路径，后续安装是能识别的，在Manager中的安装也几乎是都放在这个目录下（尽管不能选）

## DAQ软件依赖

DAQ是一个数据采集器，一般需要ELVIS Driver，安装它一般也会顺带安装了DAQmx

在MAX里面可以管理硬件设备，ELVIS还可以提供虚拟仪器

Express VI 在labview中使用DAQ助手

。。。。

安装顺序：labview -> ELVIS

要想在labview中使用DAQ，需要与版本兼容的DAQmx，新版labview不完全兼容旧版的DAQmx

DAQ code generation在2023Q1中还有问题，得安装2021 SP1才能用，那我何必呢，干嘛不直接使用2021 SP1呢（好吧2021没有conda virtual env

- 安装ELVIS时要选一些东西

  ![image-20231004114929542](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20231004114929542.png)

![image-20231004114723314](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20231004114723314.png)

