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
- 将在 <a href="">Netflix Ribbon</a>博客中讲述