---
title: STM32 定时器测量高频率
date: 2023-05-13
tags:
  - "STM32"
categories:
  - "Development"
featuredImagePreview: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230605150849270.png"
---

基于定时器实现对较高频率的测量

<!--more-->

# 捕获模式（测周法）

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519165322387.png" alt="image-20230519165322387" style="zoom:67%;" />

定时器可以对输入的PWM波进行边缘检测，同时也可以发出PWM波

定时器内部自己在对时钟进行计数，到达一定值(`AAR`)后重载为0

最开始想的是测频法，因为这样不仅可以测周期还可以调整捕获的极性以测占空比，但实验发现在频率上了$kHz$就有问题了，而且是难以言明的问题，捕获和计数器变得不靠谱起来，所以这个方法也还是只适合低频

## 生成 PWM波

先设置一个定时器的时钟为内部时钟`Internal Clock`，值就是时钟图里的HCLK，配置一个通道为`PWM Generation CHx`

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519170029338.png" alt="image-20230519170029338" style="zoom:67%;" />

然后设置它的：

- PSC：预分频系数，$\mathrm{HCLK} / (\mathrm{PSC}+1)$ 才是定时器的内部时钟频率
- counter period：这个决定了PWM波的周期（注意也要-1），计数到这个值后归零
- pulse: 这个和period决定了占空比

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519170057979.png" alt="image-20230519170057979" style="zoom:50%;" />

比如这里，如果HCLK设置的是50MHz（在时钟图里改），那么定时器的内部时钟频率就是：
$$
\mathrm{CLK_{internal}} = \frac{50M}{500} = 100 \mathrm{kHz}
$$
产生的PWM波频率是：
$$
f = \frac{100k}{100} = 1 \mathrm{kHz}
$$
占空比为：
$$
duty = \frac{50}{100} * 100\% = 50\%
$$
为向上的高电平



在main.c里面使用

```c
HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1); // 启动PWM
```

如此便能生成了，在对应的引脚便可得到

## 内部时钟-捕获

通道选择`Input Capture Direct Mode`

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519170648534.png" alt="image-20230519170648534" style="zoom:67%;" />

然后内部时钟也是一样有个预分频系数可以设置

然后设置AAR即自动重载值，计数器计到这个就会自动归0

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519170634559.png" alt="image-20230519170634559" style="zoom:67%;" />

极性选择上升沿，它就会捕获输入信号的上升沿，注意**开启中断**

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519170815569.png" alt="image-20230519170815569" style="zoom:50%;" />

它的计数器频率同理



在程序中调用

```c
HAL_TIM_Base_Start_IT(&htim4);
```

这样其实就同时开启了捕获中断和溢出中断，也可以分别开启

```c
void _HAL_TIM_ENABLE_IT(&htim4, TIM_IT_UPDATE); //溢出中断
void HAL_TIM_IC_Start_IT(&htim4,TIM_CHANNEL_1); // IC捕获中断
```

回调函数是：

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim); // 溢出回调
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim); // 捕获回调函数
```

修改极性的办法：

```c
__HAL_TIM_SET_CAPTUREPOLARITY(&htim4,TIM_CHANNEL_1,TIM_INPUTCHANNELPOLARITY_FALLING); //设置成下降沿触发
```



当捕获到上升沿后，在回调函数内切换为捕获下降沿，再次捕获，根据两次的计数值和中间溢出的次数就可以算出输入的信号的脉宽

# 外部时钟（测频法）

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519165703330.png" alt="image-20230519165703330" style="zoom:67%;" />

定时器的计数器所作用的时钟是可以改成外部时钟的

最开始想到测频法，还是利用的输入捕获的方式，也就是自己定义一个变量，捕获一次就+1这种想法，这种由于回调函数内容变得只有一行：

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim){
    cnt ++;
}
```

所以它确实能够提高测量的频率上限，但是在测到1MHz还是不行了

本来打算麻烦队友改电路降低频率了，但是我又在睡着前想到：单片机有能力对50MHz的HCLK进行分频，那么肯定不会说1MHz的频率太快了来不及响应

然后我想到之间把输入信号接到定时器上去，替换原本的内部时钟，这样就和分频是一样的道理，肯定是能够记录到脉冲个数的

事实确实如此

定时器可以通过外部时钟触发计数，经过实验也得到验证，10MHz也不是问题了

**配置方法：**

CubeMX里面：定时器`Clock Source`选择`ETR2`即可，无需中断，无需配置channel，它有写死的一个`TIMx_ETR`通道，将外部信号接到这个就行了

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230519171227580.png" alt="image-20230519171227580" style="zoom:67%;" />

它会自动对外接的信号进行计数，所以也需要配置一下预分频和重载值，就等同于平替了原来的HCLK



在程序中：

启动：

```c
HAL_TIM_Base_Start(&htim2);
TIM2->CNT = 0;
```

然后设以`TIM4`作为参考时钟，设置其溢出中断间隔为1s即1Hz，溢出时在其回调函数内读取TIM2的CNT即可（配置好TIM2不会计数溢出）

就这么简单