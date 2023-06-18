---
title: linux基础
date: 2023-06-16
---

What I understand about linux

<!--more-->

# 绪论

## bash shell

通过bash shell与之交互，这个程序是bash，shell的一种，图形界面应该叫terminal（在Windows里面叫cmd）

可执行文件`.o`一般是不显示后缀的

`./hello`是因为外部指令的原理，`./`其实就是字面意思，从当前目录下找到`hello.o`

`/`表示root，`~`一般就是`/home/user`是默认路径，输入`cd`就会调到默认路径

`gcc`一般也可以用`cc`代替

一些常用的shell命令还是要熟悉的：

- 重定向
- 后台执行
- 子shell
- 环境变量

进阶一点的实用命令：

- `strace`
- `sar`
- `ldd`

## linux C

linux的C语言依赖于标准库`glibc`，这里面在ISO定义的标准库之上增加了系统调用库的封装（帮写了汇编语言里的系统调用）

主要是在：

```c
#include <uinstd.h>
```

还有一些在别的`.h`文件内

- OS提供的库：系统调用库
- OS提供的程序：在shell中的一些指令，甚至是bash shell本身

# 进程管理

| libc       | syscall    |
| ---------- | ---------- |
| `fork()`   | `clone()`  |
| `execve()` | `execve()` |





