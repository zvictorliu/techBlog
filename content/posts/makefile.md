---
title: Makefile基础
date: 2023-06-13
categories:
- "coding"
tags:
- "系统开发"
---

从Makefile入门，逐步推进到CMake

<!--more-->

## 几个概念

编译源文件，需要`gcc`

但对于大型项目多个文件，需要专门的构建(`build`)工具，（有些库是需要手动加进去的，比如`lpthread`，所以要有脚本）

`Make`就是这样的工具，它需要`makefile`脚本来知道怎么编译

而`makefile`的生成可以由`Cmake`来生成，根据`CMakeList.txt`文件来生成（自己写`Makefile`有时也很麻烦，而`CMakeList.txt`更简单一些）

像Visual Studio这种有专门的内置build tools

> The goal of Makefiles is to compile whatever files need to be compiled, based on what files have changed.

## `makefile`语法

```makefile
targets: [prerequisites]
	command
	command
	command
```

`targets`: 生成的目标文件名 （可去后缀）

`prerequisities`: 可选，需要存在的文件

`command`: 具体的命令，一定是`Tab`开头，采用的也是shell指令

举例：

```makefile
blah: blah.c
	cc blah.c -o blah
```

如果`blah`文件不存在则会执行

如果已存在，会和`blah.c`比较是否发生改动，然后决定要不要执行

​	具体是通过修改时间来判断的，`timestamps`

### targets概念

如果想让`Makefile`里面每一个都执行：

- 定义一个`all`

```makefile
all: one two three

one:
	touch one
two:
	touch two
three:
	touch three

clean:
	rm -f one two three
```

这样默认是执行`all`，但是发现`one two three`都不存在，需要先执行它们



另外target部分可以是多个：

```makefile
all: f1.o f2.o

f1.o f2.o:
	echo $@
```

### Variables

首先数据类型都是字符串，所以也不必打引号，通过`:=`符号赋值，可以多个赋给一个，等同于define

然后引用变量：`$(x)`

还有转义字符，但要注意使用，以防被理解为普通字符

- `*` ：匹配文件名，一般使用它时需要声明：

  ```makefile
  thing_right := $(wildcard *.o)
  ```

  这等同于把所有.o文件名赋给`thing_right`

-  `%`：比较复杂，之后再将

还有一些自动变量，类似matlab里的ans

`@`: 代表target名

`^`: 代表所有的`prerequisites`

`?`: 代表相比于target有更新的`prerequisites`



make有一种自动编译c/c++的机制，不需要自己写

- 编译C：`$(CC) -c $(CPPFLAGS) $(CFLAGS) $^ -o $@`

- 编译CPP：`$(CXX) -c $(CPPFLAGS) $(CXXFLAGS) $^ -o $@`

- 链接.o文件：`$(CC) $(LDFLAGS) $^ $(LOADLIBES) $(LDLIBS) -o $@`

只需要给`CC` `CXX`这些变量赋值即可

```makefile
CC = gcc # Flag for implicit rules
CFLAGS = -g # Flag for implicit rules. Turn on debug info

# Implicit rule #1: blah is built via the C linker implicit rule
# Implicit rule #2: blah.o is built via the C compilation implicit rule, because blah.c exists
blah: blah.o

blah.c:
	echo "int main() { return 0; }" > blah.c

clean:
	rm -f blah*
```

这里就会自己给编译blah.c



filter函数：

```makefile
$(filter %.o,$(obj_files)): %.o: %.c
	echo "target: $@ prereq: $<"
```

