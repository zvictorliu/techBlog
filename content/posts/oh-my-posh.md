---
title: "Oh My Posh"
date: 2024-02-27T19:25:26+08:00
draft: false
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/2024/02/27/89a8d0e0929c16c78a7b7a473d242304-image-20240227193539780-fc195b.png"
categories:
- Tools
tags:
- shell
---

oh-my-posh 使用基础教程

<!--more-->

# oh-my-posh

`oh-my-posh`是一种优化shell的prompt的工具，就是这部分

```
(base) zongwei@V-7000:~ $
```

## 安装oh-my-posh

### windows-powershell

windows一般是`Windows Terminal`管理powershell, cmd等shell，`Windows Terminal`的主题是一回事，而prompt又是另一回事，`Windows Terminal`配置的一般是字体、背景、输出高亮这些，主题可在https://windowsterminalthemes.dev/ 上复制，然后粘贴到`Windows Terminal`的配置文件中再选择（`Ctrl+Shitf+,`打开配置`json`文件，复制到`schemes`中)

下载遵照官网指示

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

为了图标的正确显示，需要采用Nerd Fonts，其实就是给系统里添加这种字体，官网的命令行下载反倒有些问题，直接将下载下来的压缩包解压，全选`.ttf`右键安装即可，然后在`Windows Terminal`配置为这种字体（设置里面）

```powershell
notepad $PROFILE
```

进入新建编辑，填入：

```powershell
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json" | Invoke-Expression
```

可以不要`--config`的，那样就是默认主题

然后生效：

```powershell
. $PROFILE
```

### wsl-bash

经过踩坑，一定要用`brew`安装，否则wsl启动会找不到`oh-my-posh`，若是手动安装的则直接删除文件夹即可

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

记得安装时一定要遵照里面`Next Steps`里说的要执行的两个命令，即将`homebrew`的环境变量导入到`.bashrc`里面，否则会找不到`brew`

安装`oh-my-posh`需要确保`build-essential`和`gcc`安装(因为要build)

然后：

```bash
brew install jandedobbeleer/oh-my-posh/oh-my-posh
# brew install oh-my-posh
```

这样将安装成功了，重启`shell`

写入`~/.bashrc`中，注意一定是`brew shellenv`先于`oh-my-posh`

```bash
# ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
eval "$(oh-my-posh init bash --config $(brew --prefix oh-my-posh)/themes/jandedobbeleer.omp.json)"
```

重启即可

由于`WSL`也是由`Windows Terminal`管理的，所以字体也是和`Windows-powershell`的方式一样

### linux-bash

和`wsl`的一样，只是字体安装是要`Ubuntu`的方式

`oh-my-posh`也有安装字体的命令，但似乎一般网络连接会有问题，总之它就是把字体安装到系统，然后直接修改`Terminal`的设置就可以看到了

```bash
oh-my-posh font install
```

奇怪为什么推荐的字体反而不在这里面
