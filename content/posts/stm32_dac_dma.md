---
title: STM32 DAC+DMA 输出正弦波
date: 2023-05-19
tags:
  - "STM32"
categories:
  - "Development"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519135841116.png"
---

采用DMA方式用DAC输出正弦波

<!--more-->

## DAC

我们希望每隔一定时间输出一个电压值

这需要DAC到时间被触发*Trigger*，所以，对DAC的配置是：

![image-20230519134549540](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519134549540.png)

这里Trigger选择了定时器5来触发DAC，其它几个参数可无视，并没有很大影响

*注意：STM32L496ZG的DAC只有一个，且通道固定是`PA4, PA5`，而这个引脚也同时是`SPI1`的默认引脚，注意不要公用时冲突*

同时，DAC要转换的数值由DMA提供，从内存中直接经其通道传输给DAC的通道进行转换

需要配置其DMA，添加：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519134659146.png" alt="image-20230519134659146" style="zoom:67%;" />

表示将内存中的数值送到`DAC_CH1`，也就是`PA4`，注意选择`circluar`，这样才可以循环输出产生持续的正弦波

## TIM

定时器需要让它产生Trigger信号

![image-20230519135328642](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519135328642.png)

这里的update event指的就是定时器计数器满，定时器计数值满后就会触发DAC，这里的触发不是中断

所以定时器触发的频率等同于采样率

## 程序

启动：

```c
HAL_TIM_Base_Start(&htim5); // 启动tim5 无需中断
HAL_DAC_Start_DMA(&hdac1, DAC_CHANNEL_1, (uint32_t *)Sine12bit, 100, DAC_ALIGN_12B_R); // 启动DAC_DMA模式
```

这样就可以把Sine12bit输出了

`&hdac1`指出了dac对象，`DAC_CHHANNEL_1`为通道，`SIine12bit`传输的是指针，代表的是内存地址，也就是输出的源地址，100是数量，`DAC_ALIGN_12B_R`代表对齐方式，这个和寄存器有关，这里不细讲了

这里提供一个`Sine12bit`数组：

```c
const uint16_t Sine12bit[100]={
0x0800,0x0881,0x0901,0x0980,0x09FD,0x0A79,0x0AF2,0x0B68,0x0BDA,0x0C49,
0x0CB3,0x0D19,0x0D79,0x0DD4,0x0E29,0x0E78,0x0EC0,0x0F02,0x0F3C,0x0F6F,
0x0F9B,0x0FBF,0x0FDB,0x0FEF,0x0FFB,0x0FFF,0x0FFB,0x0FEF,0x0FDB,0x0FBF,
0x0F9B,0x0F6F,0x0F3C,0x0F02,0x0EC0,0x0E78,0x0E29,0x0DD4,0x0D79,0x0D19,
0x0CB3,0x0C49,0x0BDA,0x0B68,0x0AF2,0x0A79,0x09FD,0x0980,0x0901,0x0881,
0x0800,0x077F,0x06FF,0x0680,0x0603,0x0587,0x050E,0x0498,0x0426,0x03B7,
0x034D,0x02E7,0x0287,0x022C,0x01D7,0x0188,0x0140,0x00FE,0x00C4,0x0091,
0x0065,0x0041,0x0025,0x0011,0x0005,0x0001,0x0005,0x0011,0x0025,0x0041,
0x0065,0x0091,0x00C4,0x00FE,0x0140,0x0188,0x01D7,0x022C,0x0287,0x02E7,
0x034D,0x03B7,0x0426,0x0498,0x050E,0x0587,0x0603,0x0680,0x06FF,0x077F,
};
```

这里是$0 \sim 2^{12} -1$的值，转换成电压为 $0 \sim 3.3V$

在`PA4`处便可得到正弦波：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/e30cd767b338482b07b2ffeb343c5b7.jpg" alt="e30cd767b338482b07b2ffeb343c5b7" style="zoom:40%;" />





PS. *当试图用DAC做信号发生器时要考虑接口电流电压特性是否能够带得动外设，比如扬声器模块就需要前置级，不能直接驱动*