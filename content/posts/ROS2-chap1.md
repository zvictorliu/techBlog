---
title: "ROS2 系统架构"
date: 2024-03-16T13:53:34+08:00
draft: false
categories:
  - "Development"
tags:
  - "ROS"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2024/03/16/e5b0603440bf0bca51955044a6e62eee-ROS-2_logo-1024x1024-c4d53e.png"
---

ROS学习笔记

<!--more-->

# ROS2系统架构

要区分`ROS2`和`ROS1`，二者还是有许多区别的，ROS2是对ROS1的推到重来、重新设计

由于一些开源代码时间比较久，大概率是ROS1的，它们的命令是有一些区别的，好在容易区分

 `ROS`有诸多发行版，但是LTS目前是2022年发布的 *Humble Hawksbill*

## ROS2安装

```shell
# 设置编码
$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 
$ export LANG=en_US.UTF-8
$ sudo apt update # 生效
```

```shell
# 添加ROS软件源
$ sudo apt install curl gnupg lsb-release # 一些安装工具
$ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg # 设置密钥
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null # 配置软件源
$ sudo apt update
$ sudo apt upgrade
```

```shell
sudo apt install ros-humble-desktop
```

有三个版本：`desktop-full` `desktop` `base`

```shell
# 环境变量
$ source /opt/ros/humble/setup.bash #也是安装位置
$ echo " source /opt/ros/humble/setup.bash" >> ~/.bashrc 
```

## 仿真

需要多个终端下达指令

```shell
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
```

注意要在命令行上按才有效，类似于从终端输入

{{< figure src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2024/03/16/86a41e9bdb82b7ee684f3e3c6c7a7ef3-image-20240316144311548-811111.png" title="从这里输入" style="zoom:50%;" >}}

放置另一只海龟是需要调用服务：

```shell
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}" #空就是默认名
```



## 命令规范

```shell
ros2 <obj/cmd> # 不加后面的参数就是帮助说明
```

而在ros1里面则没有前面的`ros2`总命令，而是细分了，即

```
rosrun rosnode rosbag ...
```

常用的：

```shell
ros2 run <pkg> <node> # 运行一个node
ros2 node list #当前正在运行的Node
ros2 node info <path/to/node> # 查看node信息
ros2 topic list
ros2 topic echo <path/to/topic>
ros2 topic pub xxx # 发布消息

ros2 bag record xxx
ros2 bag play xxx
```

