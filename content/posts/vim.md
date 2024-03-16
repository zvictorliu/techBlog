---
title: "Vim配置"
date: 2024-03-03T22:22:50+08:00
draft: false
categories:
- Tools
tags:
- Vim
- Editor
---

将vim改造为一个IDE，接近VSC的程度

<!--more-->

# Vim配置

## 插件

可以手动安装插件，安装在`~/.vim/pack/<vendor>/start`中，不过一般不这样

一般通过插件管理器`vim-plug`

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
  https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

在`.vimrc`中配置：(以NERDTree为例)

```shell
call plug#begin()
Plug 'preservim/NERDTree'
call plug#end()
```

## 键盘映射

插件的一些命令都比较长，输入不便，因此绑定快捷键是常见的
