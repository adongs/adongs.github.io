---
layout: post
title: "Netflix Hystrix"
date: 2018-05-22 14:13:57
image: 'https://adongs.github.io/assets/img/resources/netflix-hystrix.png'
description: 学习Netflix Hystrix
category: 'Netflix Hystrix'
tags:
- Spring boot
- Spring
- Spring Cloud
- Netflix Hystrix
introduction: Netflix Hystrix(熔断)搭建和理解
---

### 理解


### 疑问


### 架构图

- 架构图

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")

### 创建项目


>前提环境 IDEA,JDK1.8

- 阅读这一章需要阅读<a href="https://adongs.github.io/NetflixRibbon/">Ribbon</a>

- 在user项目中application.yml文件中添加如下配置

```yml
feign:
  hystrix:
    enabled: true
```

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/hystrix/1.png "idea创建项目")

- 创建接口FeignTest的实现类
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/hystrix/2.png "idea创建项目")

- 修改@FeignClient注解为@FeignClient(value = "manage",fallback = FeignTestFallback.class)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/hystrix/3.png "idea创建项目")

- 不启动manage服务,其他服务均启动
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/hystrix/4.png "idea创建项目")

- 访问user(如下提示,当超过指定时间,或者服务访问失败,会调用实现类里面方法,返回结果)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/hystrix/5.png "idea创建项目")


