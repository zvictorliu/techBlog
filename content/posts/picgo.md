---
title: "picgo图床配置"
date: 2023-10-12
tags:
 - "markdown"
categories:
 - "Tools"
featuredImage: "https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20231012143025627.png"
---

基于typora+picgo-core，实现自动上传至github

<!--more-->

# picgo图床配置

一般建议采用picgo-core，而不是picgo客户端，个人在windows平台使用时极容易卡死使得上传不成功

## 安装picgo core

两种办法

【方法一】：

在typora里面安装，这种最方便

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20231012143230586.png" alt="image-20231012143230586" style="zoom:67%;" />

【方法二】：

通过`nodejs`+`npm`方式安装，这种要麻烦一点，而且在ubuntu中安装时二者版本还不匹配，还特别难以匹配，但好处是这样安装的picgo是系统知道的，即可以使用`picgo xxx`命令

## 安装插件

至少需要安装一个github uploader，一般用`githubPlus`，其它平台也有类似插件

如果需要自动重命名（因为同名文件上传会冲突导致失败），还需要安装`rename-file`插件

如果是按【方法二】安装的picgo，则可以使用命令：

```powershell
picgo install github-plus
picgo install rename-file
```

如果是【方法一】，则需要指定picgo的位置，可以通过typora中的`验证图片上传选项`看它用的命令找到`picgo.exe`(windows)或`picgo`(linux)的位置，替代上述中的`picgo`

例如：`C:\Users\[yourname]\AppData\Roaming\Typora\picgo\win64\picgo.exe install xxx`

## 修改配置文件

无论那种方法安装，配置文件都是同一个，在windows中一般是`C:\Users\zvict\.picgo\config.json`，在linux中一般是`~/.picgo/config.json`

这里提供一个模板

```json
{
  "picBed": {
    "uploader": "githubPlus",
    "current": "githubPlus",
    "githubPlus": {
      "repo": "用户名/仓库名",
      "branch": "分支名",
      "token": "从github仓库中获得token",
      "path": "img/",
      "customUrl": "https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名",
      "origin": "github"
    },
    "transformer": "path"
  },
  "picgoPlugins": {
    "picgo-plugin-github-plus": true,
    "picgo-plugin-rename-file": true
  },
  "picgo-plugin-github-plus": {
    "lastSync": "2023-10-12 02:32:32"
  },
  "picgo-plugin-rename-file": {
    "format": "{y}/{m}/{d}/{hash}-{origin}-{rand:6}"
  }
}
```

`jsdelivr`是国内加速平台

## 运行

typora上传的本质也是执行`picgo u filepath/name`命令（u可换成upload）

点击`验证图片上传选项`会上传两张样例图片，看是否成功

如果不成功看报错是什么

个人遇到的问题：

- 找不到uploader：重启、重装githubplus可解决
- invalid request：可以不用管，直接在编辑器粘贴上传即可

也可脱离typora，自己执行这个命令也可以，在typora自定义执行命令也可以改成`picgo -u`同样的效果，在obsidian中也是执行的这个命令