---
layout: post
title: "Netflix Zuul"
date: 2018-05-22 14:13:57
image: 'https://adongs.github.io/assets/img/resources/netflix-zuul.png'
description: 学习Netflix Zuul
category: 'Netflix Zuul'
tags:
- Spring boot
- Spring
- Spring Cloud
- Netflix Zuul
introduction: Netflix Zuul(动态路由)搭建和理解
---


### 理解


### 疑问

### 架构图

- 如下图Zuul 服务,就是这个Zuul 只是spring cloud成员的一位
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")



### 创建项目
>前提环境 IDEA,JDK1.8

- 选择中父项目右键选中new->Module
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/1.png "idea创建项目")

- 点击next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/2.png "idea创建项目")

- 设置好group和artifact名称,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/3.png "idea创建项目")

- 选择Core>>Lombok,Cloud Routing>> Zuul
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/4.png "idea创建项目")

- - 设置项目名称,点击Finish
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/5.png "idea创建项目")

- 创建完成后结构如下图(如果看不懂,请转<a href="https://adongs.github.io/SpringCloudConfig">Spring Cloud Config</a>>)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/6.png "idea创建项目")


- 打开eureka中的pom.xml文件,修改parent标签如下groupId和artifactId,version都需要指向父项目groupId和artifactId,version,其他修改如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.springcloud</groupId>
    <artifactId>zuul</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>zuul</name>
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
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zuul</artifactId>
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


- 打开父项目的pom.xml文件,在modules标签里面加入如下

```xml
        <module>zuul</module>
```
- zuul项目默认的配置文件是.properties格式的,为方便好的阅读修改为.yml格式


- 在application.yml 文件中添加如下配置文件(相关配置后续再讲)

```yml
server:
  port: 8881

spring:
  application:
    name: zuul

eureka:
    instance:
        statusPageUrlPath: /info
        healthCheckUrlPath: /health
        prefer-ip-address: true
        ip-address: 127.0.0.1

zuul:
  ignored-services: "*"
  sensitive-headers:
  prefix: /api #为zuul设置一个公共的前缀
  routes:
    config:
      path: /config/**
      serviceId: config
    eureka:
      path: /eureka/**
      serviceId: eureka
```

- 在eureka项目EurekaApplication启动类中添加一个注解@EnableEurekaServer
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/7.png "idea创建项目")

- 启动成功如下

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/8.png "idea创建项目")

- 启动注册中心和config服务,打开http://localhost:8761/,即可看zuul已近在注册中心里面了
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/zuul/9.png "idea创建项目")




