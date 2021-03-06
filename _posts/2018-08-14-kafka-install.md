---
layout: post
title: "kafka安装"
date: 2018-07-09 10:25:04
image: 'https://adongs.github.io/assets/img/resources/kafka.jpeg'
description: 学习kafka安装
category: 'kafka'
tags:
- kafka
introduction: 单机部署kafka伪集群
---


### 环境

- 主机数量: 1台

- 系统: ArchLinux(x86-64 GNU/Linux)

- cpu: Intel(R) Xeon(R) CPU E5-2620 v2 @ 2.10GHz

- 内存: 4G

- 硬盘: 32G

### 安装

> 1.安装docker

```shell
# ArchLinux安装docker
sudo pacman -S docker

# Ubuntu 安装docker
sudo apt-get install docker.io

# CentOS 安装Docker
sudo yum -y install docker
```

> 2.在docker中安装zookeeper

```shell
# 安装zookeeper
sudo docker pull zookeeper:3.4.9
```

> 3.在docker中安装kafka

```shell
# 安装kafka
sudo docker pull wurstmeister/kafka:0.10.2.0

```

> 4.在docker中安装kafka-manager

```shell
# 安装kafka-manager
sudo docker pull kafka-manager:alpine
```

> 5.查看docker中的image

```shell
# 查看image
sudo docker images
```
![placeholder](https://adongs.github.io/assets/img/blog/kafka/1.png "kafka")

> 6.创建zookeeper和kafka配置文件

```shell
# 创建文件夹
sudo mkdir /var/kafka_cluster

# 下载kafka压缩包:https://archive.apache.org/dist/kafka/0.10.2.0/kafka_2.10-0.10.2.0.tgz
# 下载zookeeper压缩包:http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz
# 解压这两个压缩包,分别把配置文件拷贝到kafka和zookeeper文件夹中,结构如下
```
![placeholder](https://adongs.github.io/assets/img/blog/kafka/5.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/3.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/4.png "kafka")



> 7.修改/var/kafka_cluster/zookeeper1/conf/zoo.cfg

```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data
dataLogDir=/datalog
clientPort=2181
# 下面的1,2,3 对应着myid中的值
server.1=127.0.0.1:2887:3887 
server.2=127.0.0.1:2888:3888
server.3=127.0.0.1:2889:3889
```

> 8.修改/var/kafka_cluster/zookeeper1/data/myid

```
1
```

> 9.docker镜像生成容器

```shell
# 创建3个zookeeper镜像

sudo docker run -tid --name=zookeeper1 --restart=always --net=host -v /var/kafka_cluster/zookeeper1/cong:/conf -v /var/kafka_cluster/zookeeper1/data:/data -v /var/kafka_cluster/zookeeper1/datalog:/datalog zookeeper:3.4.9

sudo docker run -tid --name=zookeeper2 --restart=always --net=host -v /var/kafka_cluster/zookeeper2/cong:/conf -v /var/kafka_cluster/zookeeper2/data:/data -v /var/kafka_cluster/zookeeper2/datalog:/datalog zookeeper:3.4.9

sudo docker run -tid --name=zookeeper3 --restart=always --net=host -v /var/kafka_cluster/zookeeper3/cong:/conf -v /var/kafka_cluster/zookeeper3/data:/data -v /var/kafka_cluster/zookeeper3/datalog:/datalog zookeeper:3.4.9


# 创建3个kafka镜像

sudo docker run -itd --name=kafka1 --restart=always --net=host -v /etc/hosts:/etc/hosts -v /var/kafka_cluster/kafka1/data:/kafka -v /var/kafka_cluster/kafka1/config:/opt/kafka/config -v /var/kafka_cluster/kafka1/logs:/opt/kafka/logs -e KAFKA_ADVERTISED_HOST_NAME=127.0.0.1 -e JMX_PORT=9997 -e HOST_IP=127.0.0.1 -e KAFKA_ADVERTISED_PORT=9092 -e KAFKA_ZOOKEEPER_CONNECT=127.0.0.1:2181 -e KAFKA_BROKER_ID=1 -e KAFKA_HEAP_OPTS="-Xmx256M -Xms256M" wurstmeister/kafka:0.10.2.0

sudo docker run -itd --name=kafka2 --restart=always --net=host -v /etc/hosts:/etc/hosts -v /var/kafka_cluster/kafka2/data:/kafka -v /var/kafka_cluster/kafka2/config:/opt/kafka/config -v /var/kafka_cluster/kafka2/logs:/opt/kafka/logs -e KAFKA_ADVERTISED_HOST_NAME=127.0.0.1 -e JMX_PORT=9998 -e HOST_IP=127.0.0.1 -e KAFKA_ADVERTISED_PORT=9093 -e KAFKA_ZOOKEEPER_CONNECT=127.0.0.1:2182 -e KAFKA_BROKER_ID=2 -e KAFKA_HEAP_OPTS="-Xmx256M -Xms256M" wurstmeister/kafka:0.10.2.0

sudo docker run -itd --name=kafka3 --restart=always --net=host -v /etc/hosts:/etc/hosts -v /var/kafka_cluster/kafka3/data:/kafka -v /var/kafka_cluster/kafka3/config:/opt/kafka/config -v /var/kafka_cluster/kafka3/logs:/opt/kafka/logs -e KAFKA_ADVERTISED_HOST_NAME=127.0.0.1 -e JMX_PORT=9999 -e HOST_IP=127.0.0.1 -e KAFKA_ADVERTISED_PORT=9094 -e KAFKA_ZOOKEEPER_CONNECT=127.0.0.1:2183 -e KAFKA_BROKER_ID=3 -e KAFKA_HEAP_OPTS="-Xmx256M -Xms256M" wurstmeister/kafka:0.10.2.0


# 创建1个kafka-manager镜像

sudo docker run -itd --restart=always --name=kafka-manager -p 9000:9000 --net=host sheepkiller/kafka-manager

```
![placeholder](https://adongs.github.io/assets/img/blog/kafka/2.png "kafka")


> 10.kafka-manager 查看服务

![placeholder](https://adongs.github.io/assets/img/blog/kafka/6.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/7.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/8.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/9.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/10.png "kafka")
![placeholder](https://adongs.github.io/assets/img/blog/kafka/11.png "kafka")



















