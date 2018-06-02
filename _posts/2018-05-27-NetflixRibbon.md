---
layout: post
title: "Netflix Ribbon"
date: 2018-05-27 21:07:56
image: 'https://adongs.github.io/assets/img/resources/netflix-ribbon.png'
description: 学习Netflix Ribbon
category: 'Netflix Ribbon'
tags:
- Spring boot
- Spring
- Spring Cloud
- Netflix Ribbon
introduction: Netflix Ribbon(负债均衡)搭建和理解
---

### 理解


### 疑问


### 架构图

- 架构图

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")


### 创建项目

>前提环境 IDEA,JDK1.8

- 阅读这一章需要阅读<a href="https://adongs.github.io/NetflixFeign/">Feign</a>

- 创建用户模块,选择中父项目右键选中new->Module
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/1.png "idea创建项目")

- 点击next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/2.png "idea创建项目")

- 设置好group和artifact名称,点击Next(我们之前已经创建过manage,现在我们再创建一个manage1)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/3.png "idea创建项目")

- 选择Core>>Lombok
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/4.png "idea创建项目")

- 设置项目名称,点击Finish
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/5.png "idea创建项目")

- 项目结构如下
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/6.png "idea创建项目")



- 项目默认的配置文件是.properties格式的,为方便好的阅读修改为.yml格式


- 在application.yml 文件中添加如下配置文件(负载均衡需要name名称相同,否则eureka会认为两个服务)

```yml
server:
  port: 8083

spring:
  application:
    name: manage

eureka:
    instance:
        statusPageUrlPath: /info
        healthCheckUrlPath: /health
        prefer-ip-address: true
        ip-address: 127.0.0.1
```

- 打开父项目的pom.xml文件,在modules标签里面加入如下

```xml
        <module>manage1</module>
```

- 打开user中的pom.xml文件,修改parent标签如下groupId和artifactId,version都需要指向父项目groupId和artifactId,version,其他修改如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.springcloud</groupId>
    <artifactId>manage1</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>manage1</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>com.springcloud</groupId>
        <artifactId>cloud</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-feign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```
- 在manage1项目的启动类上加上@EnableFeignClients,@EnableEurekaClient注解
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/7.png "idea创建项目")

- 在manage1项目中定义一个controller

```java
package com.springcloud.manage.rest;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * 测试controller
 * @Author yudong
 * @Date 2018年05月29日 上午12:48
 */
@RestController
public class TestController {

    @RequestMapping(value = "/getuserinfo", method = RequestMethod.GET)
    public String getuserinfo() {
        return "我是manage1";
    }
}

```

- 结构如图所示
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/11.png "idea创建项目")



- 启动这个项目如图
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/8.png "idea创建项目")

- 在注册中心查看是否存活
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/9.png "idea创建项目")

- 测试负载均衡是否成功(浏览器请求一个地址,却有两种结果,分别为manage何manage1服务的各自controller)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/ribbon/10.png "idea创建项目")



