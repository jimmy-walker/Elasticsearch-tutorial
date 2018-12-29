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

```java
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.search.SearchType;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.TransportAddress;

import org.elasticsearch.index.query.BoolQueryBuilder;
import org.elasticsearch.index.query.QueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import java.net.InetAddress;

public class Demo {

    private static TransportClient client;

    static{
        try{
            client = new PreBuiltTransportClient(Settings.EMPTY)
                    .addTransportAddress(new TransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
        } catch(Exception e){
            e.printStackTrace();
        }
    }
    public static void main(String[] args) throws Exception
    {
        searchIndex();
        client.close();
    }

    public static void searchIndex() {
        QueryBuilder queryBuilder = QueryBuilders.termQuery("singer","周杰伦");
        BoolQueryBuilder query = QueryBuilders.boolQuery()
                .filter(QueryBuilders.termsQuery("singer", "周杰伦"))
                .filter(QueryBuilders.termsQuery("song", "告白气球"));
        SearchResponse response = client.prepareSearch("entertainment")
                .setTypes("music")
                .setSearchType(SearchType.DFS_QUERY_THEN_FETCH)
                .setQuery(query)
                .execute()
                .actionGet();
        //获取命中数
        System.out.println(response.getHits().totalHits);
        //获取响应字符串
        System.out.println(response.toString());
        //遍历查询结果输出相关度分值和文档内容
        SearchHits searchHits =  response.getHits();
        for(SearchHit searchHit : searchHits){
            System.out.println(searchHit.getScore());
            System.out.println(searchHit.getSourceAsString());
        }
    }
}
```

### 代码解释

The `TransportClient` connects remotely to an Elasticsearch cluster using the transport module. It does not join the cluster, but simply gets one or more initial transport addresses and communicates with them in round robin fashion on each action (though most actions will probably be "two hop" operations).  

```java
// on startup
TransportClient client = new PreBuiltTransportClient(Settings.EMPTY)
        .addTransportAddress(new TransportAddress(InetAddress.getByName("host1"), 9300))
        .addTransportAddress(new TransportAddress(InetAddress.getByName("host2"), 9300));
// on shutdown
client.close();
```

The query builders are used to create the query to execute within a search request. There is a query builder for every type of query supported by the Query DSL. Each query builder implements the QueryBuilder interface and allows to set the specific options for a given type of query. Once created, the QueryBuilder object can be set as the query parameter of SearchSourceBuilder.

```
client.prepareSearch用来创建一个SearchRequestBuilder，搜索即由SearchRequestBuilder执行。client.prepareSearch方法有参数为一个或多个index，表现在数据库中，即零个或多个数据库名。
```

```
SearchRequestBuilder有很多很实用的方法：
setTypes(String... types)：参数可为一个或多个字符串，表示要进行检索的type，当参数为0个或者不调用此方法时，表示查询所有的type；
setSearchType(SearchType searchType)：执行检索的类别，值为org.elasticsearch.action.search.SearchType的元素，SearchType是一个枚举类型的类。
setQuery，设置查询使用的Query；
```

```
1、其中，execute() 方法中，创建了一个ActionListener，用来监听action的执行结果；然后在调用actionGet(timeout)，获取最终返回结果，actionGet是一个阻塞的方法，可以设置一个超时时间，也可以不设置。
2、get(timeout)方法：
通过源码可以发现，get(timeout)方法内部就是封装了execute().actionGet(timeout)方法，其中参数timeout也是超时时间，当然也可以不设置，一致阻塞知道有返回结果。
```



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
- [java api相关代码3](https://my.oschina.net/UpBoy/blog/699673)
- [java搜索相关参数讲解](https://blog.csdn.net/geloin/article/details/8907731)