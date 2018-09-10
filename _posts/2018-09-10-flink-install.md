---
layout: post
title: "flink安装"
date: 2018-09-10 12:27:32
image: 'https://adongs.github.io/assets/img/resources/flink.png'
description: 学习flink安装
category: 'flink'
tags:
- flink
introduction: flink在mac上安装
---



### 安装 Flink(1.5.0)

```shell
# 需要MAC安装:jdk1.8 ,homebrew   
brew install apache-flink
#查看版本
flink -v
# 安装的位置在:/usr/local/Cellar/apache-flink/1.5.0/libexec
cd /usr/local/Cellar/apache-flink/1.5.0/libexec
#启动flink
./bin/start-cluster.sh
#用浏览器打开:http://localhost:8081
```

### 安装 homebrew

> Homebrew是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

```shell
#打开终端执行如下脚本即可
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 安装java8

```shell
brew cask install java

brew tap caskroom/versions

brew cask install java8
```






