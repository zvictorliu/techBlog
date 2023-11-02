---
title: "VSCode配置理解"
date: 2023-10-30
tags:
 - "tools"
categories:
 - "Development"
featuredImage: "https://www.freecodecamp.org/news/content/images/2021/08/vscode.png"
---

从配置中得到的启示

<!--more-->

# VSCode配置理解

本身是一个编辑器，并不是如VS那样的IDE

但可以配置插件（脚本）实现类似的效果，对于轻量开发十分好用

## 逻辑

对于c/c++，要经过编译链接形成成`.o`或`.exe`，使用编译器来做这个事情也是需要输入参数、选项，现在用VSC来帮我们做这件事情，`tasks.json`就是构建build的指示说明

VSC中的c/c++，并没有原生的运行，在这里面都是视为调试，都需要`gdb`

而插件Code Runner才算是在纯粹运行：

```json
"cpp": "cd $dir && g++ $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
"c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
"python": "python -u",
```

看VSC是怎么run c/c++ file的：

```
Executing task: C/C++: g++.exe build active file 

Starting build...
G:/mingw/msys64/ucrt64/bin/g++.exe -fdiagnostics-color=always -g G:\geek\vsc\hello.cpp -o G:\geek\vsc\exe\hello.exe #这是正常的编译生成

Build finished successfully.
Terminal will be reused by tasks, press any key to close it. #这时启动了cppdbg终端
```

```
G:\geek\vsc> cmd /C "c:\Users\zvict\.vscode\extensions\ms-vscode.cpptools-1.17.5-win32-x64\debugAdapters\bin\WindowsDebugLauncher.exe --stdin=Microsoft-MIEngine-In-3cgnpwjs.irb --stdout=Microsoft-MIEngine-Out-krfimryw.n30 --stderr=Microsoft-MIEngine-Error-4y053itd.pmt --pid=Microsoft-MIEngine-Pid-k0bhgb4z.hh4 --dbgExe=G:\mingw\msys64\ucrt64\bin\gdb.exe --interpreter=mi "
Hello C++ World from VS Code and the C++ extension! #这是结果
```

可见是启动了内置的cpptools的调试启动器，启动的是指定的gdb

所以它所谓运行其实还是调试，是忽略断点的调试而已

在VSC，launch是一个重要概念，它指示如何使用调试器来**调试**，可以编写launch.json文件自定义，但实际上它应该是`setting.json`中的一部分

一般是不需要配置的其实，除非有必要的参数输入

最后的`c_cpp_properties.json`则是告诉编译器位置、C++标准、IntelliSense设定等信息的配置

先执行Task，再Launch Debugger，在launch中需指定`preLaunchTask`与tasks中的label名一致

## MinGW vs MSVC

可以这样理解：

GNU计划(GNU's Not Unix)，希望开发类UNIX操作系统，开发了许多组件，但没有开发出内核

后来Linux内核被开发出来，与GNU组件结合，称之为`GNU/Linux`操作系统，现在一般就称之为Linux操作系统

GNU中有很多熟悉的组件：GCC, Bash ...

编译器组件GCC，对于Windows，GNU这边有一个Windows支持版：MinGW，而微软自家也有自己的工具：VC

VSCode市面上主要是用的MinGW

### MSVC配置

太麻烦了，需要从开发者启动

### MinGW配置

- C/C++ Extension
  - IntelliSense会有

- 安装MinGW并添加至环境变量

查看：

```
$ gcc -v
gcc version 8.1.0 (x86_64-posix-seh-rev0, Built by MinGW-W64 project)
```

按VScode官方安装吧

- 首次选择会成为默认tasks

运行后卡住没反应（无解，只能重新安装mingw再来一次了

可见其实还是要信official的
