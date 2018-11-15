# Java Api

## pom文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.music.search</groupId>
    <artifactId>es_test</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>transport</artifactId>
            <version>6.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.10.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.10.0</version>
        </dependency>
        <dependency>
            <groupId>com.carrotsearch</groupId>
            <artifactId>hppc</artifactId>
            <version>0.8.1</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.8.10</version>
        </dependency>
    </dependencies>
</project>
```

## 相关代码

java api的相关代码

## 错误排除

1. 本来最新版是6.4.3，但是会发生版本不兼容，卡住的问题，改成6.3.0，就没有问题了。
2. 此外会报错：

```
ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console.
```

此时根据网上提示，需要新建一个log4j2.xml放到`src/main/resources`中。内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Root level="error">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

3. 需要使用9300端口。因为Elastic listen for all transport request at port 9300, even other nodes. 而不是一开始学习到的9200端口。

## Reference

- [TransportClient.builder() hang](https://discuss.elastic.co/t/transportclient-builder-hang-indefinitely/51088/9)
- [Elasticsearch v5.0.0: No log4j2 configuration file found](https://discuss.elastic.co/t/elasticsearch-v5-0-0-no-log4j2-configuration-file-found/65476/3)
- [java api相关代码1](https://blog.csdn.net/feinifi/article/details/80799487)
- [java api相关代码2](http://aoyouzi.iteye.com/blog/2116597)