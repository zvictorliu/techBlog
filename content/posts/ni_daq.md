---
title: "NI DAQ使用教程"
date: 2023-10-04
tags:
 - "NI"
categories:
 - "Tools"
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