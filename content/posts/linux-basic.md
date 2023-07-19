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



## 文件权限

`/etc/passwd` 记录了用户信息

分三类：root, 用户账户，系统功能账户

每一行的格式：

`zongwei:x:1000:1000:zongwei,,,:/home/zongwei:/bin/bash`

登录名：密码（隐藏了）：UID：组ID：备注：默认目录：默认Shell

密码保存至`/etc/shadow`下面，root才有权限访问，不过如果有sudo权限是可以修改root密码的

```shell
$ sudo passwd root
...
$ su
$ exit
```

但是cat出来也看不懂，它经过了加密机制

组内可以共享权限设置

> Ubuntu会为每个用户创建一个单独的与用户账户同名的组

查看文件权限：`ls -l`

第一个字符：`-`文件，`d`目录，`l`链接 ...

后面9个字符每3个为一组：属主权限-同组用户权限-其它用户权限

`r`读 `w`写 `x`可执行 如果没有这个权限就是`-`来占位

创建文件时是用的默认权限，用`unmask`可以修改默认，但比较难

对已有的文件进行权限修改：`chmod`

改变文件属主：`chown`，属组`chgrp` （root才可以用）

下载下来的`.run`文件一般是没有可运行的权限的，需要赋予
