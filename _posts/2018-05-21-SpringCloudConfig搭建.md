---
layout: post
title: "Spring Cloud Config"
date: 2018-05-21 18:43:58
image: 'https://adongs.github.io/assets/img/resources/spring-cloud-config.png'
description: 学习Spring Cloud Config
category: 'Spring Cloud Config'
tags:
- Spring boot
- Spring
- Spring Cloud
- Spring Cloud Config
introduction: Spring Cloud Config 搭建和理解
---

### 理解
- Spring Cloud Config为分布式系统中的外部化配置提供服务器和客户端支持。通过Config Server，您可以在所有环境中管理应用程序的外部属性。客户端和服务器上的概念与Spring Environment和PropertySource抽象，因此它们非常适合Spring应用程序，但可以与任何运行在任何语言的应用程序一起使用。随着应用程序从开发到测试转移到部署管道中，您可以管理这些环境之间的配置，并确保应用程序具有迁移时所需的所有内容。服务器存储后端的默认实现使用git，因此它可以轻松支持配置环境的标签版本，并且可以用于管理内容的各种工具。使用Spring配置很容易添加替代实现并将其插入。

- 白话理解: 其他模块可以通过这个config模块获取配置文件,自己不用管理配置文件
(例如:如果你有50台服务器需要部署微服务,你总不为每一个服务写配置文件,你可以用其中的一台做config服务,其他49台服务都去访问这台服务,获取自己相应的配置文件.例如,现在需要修改49个服务的配置文件,你总不能每一个都去修改,有了config服务,你可以只修改这一服务管理的配置文件就能够修改49个服务的配置)

### 疑问
> config服务怎么知道49台服务需要哪些配置?
- 49个服务通过指定自己想要的配置文件的规则,通过这个规则config服务就知道服务需要哪个配置文件(具体规则请看后续博客讲述config获取规则)

> 如果我更新cofnig配置,49台服务怎么自动更新配置文件?(config服务怎么通知49服务更新配置文件)?
- 可以使用Spring Cloud Bus来实现热部署,这个实现关注后续博客

### 架构图

- 如下图config 服务,就是这个config 只是spring cloud成员的一位
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")



### 创建项目
>前提环境 IDEA,JDK1.8

- 打开idea创建项目,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/1.jpg "idea创建项目")

- 设置好group和artifact名称,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/2.jpg "idea创建项目")

- 什么都不选,创建一个空的项目,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/3.jpg "idea创建项目")

- 设置项目名称,点击Finish
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/4.jpg "idea创建项目")

- 创建完成后结构如下图(我们需要创建多个微服务,所以这个创建的是父项目)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/5.jpg "idea创建项目")

- 删除不需要的文件(.mvn目录,scr目录,mvnw,mvnw.cmd)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/6.jpg "idea创建项目")

- 选中项目右键->New->Module->Spring Initializr(又回到第一步),点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/7.jpg "idea创建项目")

- 设置好group和artifact名称,点击Next(创建config项目),点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/8.jpg "idea创建项目")

- 选择 Core-><a href="https://www.zhihu.com/question/42348457">Lombok</a>,Cloud Config -> Config Server ,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/9.jpg "idea创建项目")

- 设置项目名称,点击Finish (项目名为config,你也可以自定义)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/10.jpg "idea创建项目")

- 创建好后结构如下图
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/11.jpg "idea创建项目")

- 打开config中的pom.xml文件,修改parent标签如下groupId和artifactId,version都需要指向父项目groupId和artifactId,version

```xml
    <parent>
        <groupId>com.springcloud</groupId>
        <artifactId>cloud</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

- 打开父项目的pom.xml文件,在dependencies标签上面添加如下

```xml
    <modules>
        <module>config</module>
    </modules>
```

- 在config项目ConfigApplication启动类中添加一个注解@EnableConfigServer
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/12.jpg "idea创建项目")

- config项目默认的配置文件是.properties格式的,为方便好的阅读修改为.yml格式


- 在application.yml 文件中添加如下配置文件(config 总的有三种读取方式git,svn,本地文件),这里配置本地读取模式

```yml
server:
  port: 8888  //设置tomcat端口

spring:
    application:
        name: config  //配置应用名称
    profiles:
      active: native  //本地文件读取模式
    cloud:
      config:
        server:
          native:
            search-locations: //你放配置文件的目录
```

- 启动项目如下图,到此config服务创建完成
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/13.jpg "idea创建项目")


### config服务注册到注册中心(eureka)

- 注册前提是<a href="https://adongs.github.io/NetflixEureka/">搭建注册中心</a>

- 在config 服务 pom.xml 加入如下包
```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>

```

- 配置文件修改如下
```xml
server:
  port: 8888  //设置tomcat端口

spring:
    application:
        name: config  //配置应用名称
    profiles:
      active: native  //本地文件读取模式
    cloud:
      config:
        server:
          native:
            search-locations: //你放配置文件的目录

eureka:           
    instance:
        statusPageUrlPath: /info  //状态页面
        healthCheckUrlPath: /health //健康验证页面
        prefer-ip-address: true
        ip-address: 127.0.0.1  //连接地址,由于注册中心端口默认配置的8761,所以不需要写端口
```

- 在config服务的启动类添加注解@EnableEurekaClient,如下图
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/15.jpg "idea创建项目")


- 先启动注册中心,如下
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/16.jpg "idea创建项目")

-再启动config服务
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/17.jpg "idea创建项目")

-打开http://localhost:8761/,即可看到config在注册中心里
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/18.jpg "idea创建项目")



