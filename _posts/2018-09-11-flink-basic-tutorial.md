---
layout: post
title: "flink基础教程"
date: 2018-09-11 14:30:47
image: 'https://adongs.github.io/assets/img/resources/flink.png'
description: 学习flink
category: 'flink'
tags:
- flink
introduction: flink基础教程
---

### 安装flink

```shell
https://adongs.github.io/flink-install/
```

### 启动flink

```shell
#在安装目录中执行如下
./bin/start-cluster.sh
```

> 启动界面如下

![placeholder](https://adongs.github.io/assets/img/blog/flink/flink-start-ui.png "flink")


### 使用idea创建一个maven项目

> 创建一个简单maven项目

![placeholder](https://adongs.github.io/assets/img/blog/flink/1.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/2.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/3.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/4.png "flink")




> 在maven中引入flink包

```xml
  <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.xxxx</groupId>
    <artifactId>flink</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
    <dependency>
        <groupId>org.apache.flink</groupId>
        <artifactId>flink-java</artifactId>
        <version>1.6.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.flink</groupId>
        <artifactId>flink-streaming-java_2.11</artifactId>
        <version>1.6.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.flink</groupId>
        <artifactId>flink-clients_2.11</artifactId>
        <version>1.6.0</version>
    </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.4.3</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>SocketWindowWordCount</mainClass>
                                </transformer>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                    <resource>reference.conf</resource>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```


> 创建一个类


```java
import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.common.functions.ReduceFunction;
import org.apache.flink.api.java.utils.ParameterTool;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.util.Collector;

/**
 * @Author yudong
 * @Date 2018年09月10日 下午3:46
 */
public class SocketWindowWordCount {

    public static void main(String[] args) throws Exception {

         int port;
        try {
        	//接受参数启动时的参数参数为 port 指定端口
            final ParameterTool params = ParameterTool.fromArgs(args);
            port = params.getInt("port");
        } catch (Exception e) {
        	//设置默认端口
           port = 9000;
        }

        // 获取运行环境
        final StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

        // 通过连接到套接字获取输入数据
        DataStream<String> text = env.socketTextStream("localhost", port, "\n");

        // 解析数据，对其进行分组，对其进行窗口化，并聚合计数
        DataStream<WordWithCount> windowCounts = text
                //数据平面转换
                .flatMap(new FlatMapFunction<String, WordWithCount>() {
                    @Override
                    public void flatMap(String value, Collector<WordWithCount> out) {
                    	//对空格进行分组,统计每组次数
                        for (String word : value.split("\\s")) {
                            out.collect(new WordWithCount(word, 1L));
                        }
                    }
                })
                //进行分组,针对相同的word数据进行分组
                .keyBy("word")
                //每一秒钟滑动一次的滑动5秒钟
                .timeWindow(Time.seconds(5), Time.seconds(1))
                //求值调用窗口函数
                .reduce(new ReduceFunction<WordWithCount>() {
                    @Override
                    public WordWithCount reduce(WordWithCount a, WordWithCount b) {
                    	//求和
                        return new WordWithCount(a.word, a.count + b.count);
                    }
                });

        // 使用单个线程打印结果，而不是并行打印
        //你也可以将结果输出到redis或者MQ中,或者输出到你指定API中        
        windowCounts.print().setParallelism(1);

        //因为flink是懒加载的，所以必须调用execute方法，上面的代码才会执行
        env.execute("Socket Window WordCount");
    }

    // 带有count的单词的数据类型
    public static class WordWithCount {

        //分组标识
        public String word;
        //数量
        public long count;

        public WordWithCount() {}

        public WordWithCount(String word, long count) {
            this.word = word;
            this.count = count;
        }

        @Override
        public String toString() {
            return word + " : " + count;
        }
    }
}
```

> 打包项目成jar包,并上传jar

![placeholder](https://adongs.github.io/assets/img/blog/flink/5.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/6.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/7.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/8.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/9.png "flink")

![placeholder](https://adongs.github.io/assets/img/blog/flink/10.png "flink")












