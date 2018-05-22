---
layout: post
title: "Spring Cloud Config"
date: 2018-05-21 18:43:58
image: 'https://adongs.github.io/assets/img/resources/springcloud.svg'
description: 学习Spring Cloud Config
category: 'Spring Cloud Config'
tags:
- Spring boot
- Spring
- Spring Cloud
- Spring Cloud Config
introduction: Spring Cloud Config 搭建和理解
---

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

