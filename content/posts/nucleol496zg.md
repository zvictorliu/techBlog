---
title: "基于NUCLEO-L496ZG开发"
date: 2023-04-21
tags:
  - "STM32"
categories:
  - "Development"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/nucleol496zg.png"
---



本文基于STM32CubeMX + Keil5 的开发，介绍该型号的配置和使用方法

<!--more-->

# 写在前面

重要手册：

[UM2179: STM32 Nucleo-144 boards (MB1312)](https://www.st.com/content/ccc/resource/technical/document/user_manual/group0/6e/c2/81/e8/92/5f/41/cc/DM00368330/files/DM00368330.pdf/jcr:content/translations/en.DM00368330.pdf) 是NUCLEO-144系列开发板的相关手册，可谓最重要的参考文档

[DS11585](https://www.st.com/resource/en/datasheet/stm32l496zg.pdf) 是板载的MCU手册，用处不大，现在基本不会用裸机

STM32L073xx_User_Manual.chm 这个一般位于cube安装的pack里面（\STM32Cube\Repository\STM32Cube_FW_L0_V1.12.2\Drivers\STM32L0xx_HAL_Driver），是HAL库的手册，用HAL库的都有类似的文档，因此不必纠结是不是L0系列



# STM32CubeMX

CubeMX最直接的好处就是可视化配置+代码框架，在其中配置好生成代码即可

CubeMX的不同版本界面差别挺大，笔者用的是 STM32CubeMX-Win [6.8.0](https://www.st.com/zh/development-tools/stm32cubemx.html#get-software)，也建议紧跟时代用新版，免费软件没问题的

## 选型

- 在`Pakages Manager` 里面安装L4系列的MCU Pack
- 在new project里，直接选板子，板子和其MCU型号是对应了的，这样生成的界面才能看到一些重要管脚定义

## 配置

主要需要配置 LPUART 和 ADC

- LPUART 用于和PC机直接通过USB线通信（因为它是和STLINK连接的），默认是使能了的，但还需要自行配置其波特率等参数
- ADC 要Enable其引脚，否则即使在引脚端选择了也还是黄色的而非绿色，要注意别忘了
  - 如果需要用中断方式，记得配置 Global Interupt

## 生成代码

注意在generate code的时候记得选分文件（不是必要，但有利于阅读）

并且要注意toolchain的最小版本是否合适

项目名字一旦取定则无法更改

# Keil

在keil上需要安装l4系列的pack，注意由于官网的pack一般是新版，可能直接安装就旧版Keil上会出现这样的问题：



这个并不太会影响正常使用，作为强迫症患者最好换新版Keil

在Keil上的魔法棒（option for target)，要注意配置两个地方

- target下面，勾选`Use MicroLIB`，这个具体什么原因笔者还未了解清楚，只知道如果不选则 LPUART发送数据会卡住，无法继续执行代码（但调试可以），所以如果用户在使用时发现拷了代码进去却没反应，有可能就是卡住了，记得检查这里
- debug下面，选 use `J-LINK / J-TRACE Cortex`，虽然文档说是用的 STLINK V2-1，但实际插上之后显示的是JLINK设备，具体为什么也不清楚。然后在setting里面port选`SW`

若还有其它的问题，会在下载时报错，根据报错信息应该也能很快解决，只需耐心读

# Code

编程要学会重定义 `printf`函数，具体做法是在main函数里：

```c
#include "stdio.h"

int fputc(int ch, FILE *f)
{
    HAL_UART_Transmit(&hlpuart1, (uint8_t *)&ch, 1 , 0xffff);
    return ch;
} 
```

此外注意自己加的内容放到注释里面写的 Begin - End 之间，这样在修改CubeMX之后重新生成不会把自己的代码覆盖掉

建议在VScode或者其它编辑器进行编辑，keil IAR 等软件的代码编辑界面都是否老古董，用着难受，具体配置请自行搜索



整个代码用的是HAL库，虽然在源文件里面看不到对应的include信息，但其实在Keil里面它是自己预定义过了的

网上找到教程有些不是HAL库，因此函数名不一样，HAL库的函数一般都会以`HAL_`开头，注意不要无脑CV 😊



