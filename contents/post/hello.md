---
title: "hugo安装"
date: 2023-06-25T20:50:35+08:00
draft: false
categories:
- 日常
tags:
- hugo
keywords:
- hugo
- PaperMod
- blog
---
记录一下hugo的安装过程，主要参考了 [静态博客Hugo安装及使用教程](https://aixin.me/post/how-to-hugo/)。  

### 1. github仓库创建和配置

创建仓库，配置workflows，整体流程参考 [hosting-on-github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)


### 2. windows环境下hugo的安装  

[安装文件下载](https://github.com/gohugoio/hugo) 下载windows环境下的压缩包，解压缩并配置环境变量。  

`hugo version`查看版本信息，成功显示版本号则安装成功。

`hugo new site simple.com`创建新的网站。

将默认配置文件`hugo.toml`重命名为`config.yml`格式，修改内容格式。

### 3. 模板的配置  

这里使用 [PaperMod](https://themes.gohugo.io/themes/hugo-papermod/)，参考官方文档安装。
配置参考 [PaperMod主题配置](https://shaohanyun.top/posts/env/blog_build2/#%e8%87%aa%e5%ae%9a%e4%b9%89%e5%a4%b4%e9%83%a8)

### 4. 本地文件管理Obsidian使用  
![](assets/Obsidian-file-setting.png)