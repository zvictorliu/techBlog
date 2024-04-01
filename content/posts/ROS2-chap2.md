---
title: "ROS2 核心概念"
date: 2024-03-16T15:10:31+08:00
draft: false
categories:
  - "Development"
tags:
  - "ROS"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2024/03/16/e5b0603440bf0bca51955044a6e62eee-ROS-2_logo-1024x1024-c4d53e.png"
---

基本和ROS1是一样的

<!--more-->

# ROS2 核心概念

## workspace

需要注意这个

```
- ws/
    - src/
    - install/
    - build/
    - log
```

`src`中的代码被编译，中间文件在`build`，生成的文件在`install`

开源的代码就是clone到`src`中，一般这些代码会有自己的依赖项，这些不同项目的代码就是一个个功能包package

有python包可以帮助安装依赖：`rosdepc`（鱼香开发的），注意这不是ros2内部命令而是python的，所以风格和ros2不一样

```shell
sudo apt install python3-rosdep2
sudo rosdepc init && rosdepc update
```

```shell
rosdepc install -i --from-path src --rosdistro humble -y # 自动搜索src下的安装包
```

依赖项dependency安装好后才开始编译

ros2引入了新的编译工具`colcon`，而ros1是`catkin`

```shell
sudo apt install python3-colcon-ros
```

然后在ws根目录执行编译

```shell
colcon build
```

如果报错：

```
ModuleNotFoundError: No module named 'em'
```

指的是python的empy包

```
pip install empy==3.3.2 #版本问题
python -m pip install lark-parser
```

还要将当前的工作空间告知系统，配置环境变量

```shell
$ source install/local_setup.sh # 仅在当前终端生效
$ echo " source ~/<ws>/install/local_setup.sh" >> ~/.bashrc # 所有终端均生效
```

## package

除了从别处下载功能包之外，也还可以自己创建

```shell
$ cd src/ # 需要切到src，但是src也不是可以自定义名字，colcon是默认在这里找的
$ ros2 pkg create --build-type <build-type> <package_name>
```

因为ros2可以支持c++和python，因此要选择是哪种build-type，如果使用C++或者C，那这里就跟ament_cmake，如果使用Python，就跟ament_python

`package.xml` 描述该功能包，所以一个package应该是由这个文件来确定的，并不完全受到文件夹分布的影响

```xml
  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>
```

这些就是它的依赖项

C++的还有一个`CMakeList.txt`，Python则没有(Python是解释语言)

Python对应的是`setup.cfg setup.py`文件

## node

每个节点都是一个独立运行的可执行文件，是一个具体的任务进程

支持不同语言，不同平台，不同主机

名称要唯一

package文件夹中有源码，对于python版本，需要指明节点程序的入口，这是在`setup.py`文件中的

```python
entry_points={
        'console_scripts': [
         'node_helloworld       = learning_node.node_helloworld:main',
         'node_helloworld_class = learning_node.node_helloworld_class:main',
         'node_object            = learning_node.node_object:main',
         'node_object_webcam     = learning_node.node_object_webcam:main',
        ],
    },
```

这实际是指明了该功能包下四个节点和它们对应的程序入口

`colcon build`会依照这个，会将源文件复制到install中的一个文件夹，再生成一些带有描述性质的可执行python代码

```
/ws
	install/
		<packageName>/
			lib/
				<packageName> # 生成代码
				<python> # 源码
```

每次修改代码就需要**重新编译**，否则就还是执行池子里的旧版



如果编程时用的相对路径，它还能正确找到吗？

## topic

节点之间的通信，两种方式，一种是topic，一种是service

topic遵循发布/订阅模型，采用`.msg`的消息结构

{{< figure src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2024/03/16/126444b0e548d78237419d14d6799885-image8-14bb5b.gif" title="发布/订阅模型" style="zoom:50%;" >}}

可以多对多通信，只是需要注意优先级

编程时的思路是：

- 编程接口初始化
- 创建节点并初始化
- 创建发布者对象
- 创建并填充话题消息
- 发布话题消息
- 销毁节点并关闭接口

一般都会有一个定时器对象，以及相应的callback函数，这样才能持续工作

## service

即C/S模型，通过request-response，需要回应，所以是一种同步通信

![image9](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.5_%E6%9C%8D%E5%8A%A1/image9.gif)

## interface

其实就是规范了数据结构：`.msg` `.srv` `.action` ，大家都认可这样的结构，自然也就免除了不同语言之间的区别，做到语言无关

```shell
$ ros2 interface list                    # 查看系统接口列表
$ ros2 interface show <interface_name>   # 查看某个接口的详细定义
$ ros2 interface package <package_name>  # 查看某个功能包中的接口定义
```

## action

与服务类似，只是需要周期反馈，便于把握运动的状态和进度

通过服务发送指令，通过话题周期反馈

## param

参数通过字典，属于共享，并且修改后立刻能被其他节点知道并生效

可通过参数文件`.yaml`格式赋予

```shell
$ ros2 param dump turtlesim >> turtlesim.yaml  # 将某个节点的参数保存到参数文件中
$ ros2 param load turtlesim turtlesim.yaml     # 一次性加载某一个文件中的所有参数
$ ros2 param set param_declare robot_name turtle
```

## 分布式通讯

将节点分布在不同主机上，只要在统一局域网，他们就是通的

但是肯定不想所有节点都通，避免混乱

可以进行分组：

```shell
# ~/.bashrc
export ROS_DOMAIN_ID=<your_domain_id>
```

## DDS 

类似于总线通讯
