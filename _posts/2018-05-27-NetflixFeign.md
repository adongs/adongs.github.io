---
layout: post
title: "Netflix Feign"
date: 2018-05-27 21:07:56
image: 'https://adongs.github.io/assets/img/resources/netflix-feign.png'
description: 学习Netflix Feign
category: 'Netflix Feign'
tags:
- Spring boot
- Spring
- Spring Cloud
- Netflix Feign
introduction: Netflix Feign(模板化客户端)搭建和理解
---

### 理解
- Feign是受到Retrofit，JAXRS-2.0和WebSocket启发的Java to HTTP客户端联编程序。Feign的第一个目标是将绑定分母的复杂性统一降低到HTTP API，而不管ReSTfulness如何。

- 白话理解:Feign 帮助你更好的模块之间的调用,同时结合Hystrix(熔断)和(Ribbon)负载均衡

### 疑问
> 怎么结合Hystrix(熔断)的?
- 将在 <a href="https://adongs.github.io/NetflixHystrix/">Netflix Hystrix </a> 博客中讲述

> 怎么结合Ribbon(负载均衡)?
- 将在 <a href="https://adongs.github.io/NetflixRibbon/">Netflix Ribbon</a>博客中讲述

### 架构图

- 架构图

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")


### 创建项目

>前提环境 IDEA,JDK1.8


- 为了测试Feign,我们需要创建两个自定义Web模块,也就是我们自己的业务模块,这里使用用户模块(user)和管理模块来举例子(manage),让用户模块调用管理模块


- 创建用户模块,选择中父项目右键选中new->Module
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/1.png "idea创建项目")

- 点击next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/2.png "idea创建项目")

- 设置好group和artifact名称,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/3.png "idea创建项目")

- 选择Core>>Lombok
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/4.png "idea创建项目")

- 设置项目名称,点击Finish
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/5.png "idea创建项目")

- 项目结构如下
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/feign/6.png "idea创建项目")

- 按照上面的步骤再创建一个manage项目
















