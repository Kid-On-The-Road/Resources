# 介绍

Elaticsearch，简称为es，es是一个开源的高扩展的分布式全文搜索服务，它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。es也是使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

# ElasticSearch对比Solr

+ Solr利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能。
+ Solr支持更多格式的数据，而Elasticsearch仅支持json文件格式。
+ Solr官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能都有第三方插件提供。
+ Solr在传统的搜索应用中表现好于Elasticsearch，但在处理实时搜索应用时效率明显低于  Elasticsearch当单纯的对已有数据进行搜索时，Solr更快,当实时建立索引时, Solr会产生io阻塞，查询性能较差, Elasticsearch具有明显的优势。随着数据量的增加，Solr的搜索效率会变得更低，而Elasticsearch却没有明显的变化。综上所述，**Solr的架构不适合实时搜索的应用**。Solr是传统搜索应用的有力解决方案，但 **Elasticsearch 更适用于新兴的实时搜索应用**。

关系型数据库与ES索引库类比:

| 关系型数据库  | 数据库    | 表     | 行   | 列      |
| ------------- | --------- | ------ | ---- | ------- |
| Relational DB | Databases | Tables | Rows | Columns |

| ES服务        | 索引库  | 类型  | 文档      | 字段   |
| ------------- | ------- | ----- | --------- | ------ |
| ElasticSearch | Indices | Types | Documents | Fields |

# 核心概念

![1551061589694](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1551061589694.png) 

+ **近实时 NRT**(Near Realtime)--速度快

  Elasticsearch是一个接近实时的搜索平台。这意味着，从索引一个文档直到这个文档能够被搜索到有一个轻微的延迟（通常是1秒以内）

+ **集群 cluster**

  一个集群就是由一个或多个节点组织在一起，它们共同持有整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，==这个名字默认就是“elasticsearch”。==这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群。

+ **节点 node**

  一个节点是集群中的一个服务器，作为集群的一部分，它存储数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于Elasticsearch集群中的哪些节点。 一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫 做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此， 它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。 在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何Elasticsearch节点， 这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的集群。

+ **索引 index(重点)**--索引库

  + 几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母的），并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。
  + 一个索引就是一个拥有几分相似特征的文档的集合。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母的），并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。

+ **类型 type(重点)**--表

  在一个索引中，你只能定义一种类型。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。比如说，我们假设你运营一个博客平台并且将你所有的数据存储到一个索引中。在这个索引中，你可以为用户数据定义一个类型，为博客数据定义另一个类型，当然，也可以为评论数据定义另一个类型。

+ **文档 document(重点)**--json

  一个文档是一个可被索引的基础信息单元。比如，你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以JSON（Javascript Object Notation）格式来表示，而JSON是一个到处存在的互联网数据交互格式。 在一个index/type里面，你可以存储任意多的文档。注意，尽管一个文档，物理上存在于一个索引之中，文档必须 被索引/赋予一个索引的type。

+ **分片和复制shards&replicas**--自我、修复、备份

  一个索引可以存储超出单个节点硬件限制的大量数据。比如，一个具有10亿文档的索引占据1TB的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。分片很重要，主要有两方面的原因：

  + 允许你水平分割/扩展你的内容容量。
  + 允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量。至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由Elasticsearch管理的，对于作为用户的你来说，这些都是透明的。在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。复制之所以重要，有两个主要原因： 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行。总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。默认情况下，Elasticsearch中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。

+ **映射 mapping(重点)**

  mapping是处理数据的方式和规则方面做一些限制，如某个字段的数据类型、默认值、分析器、是否被索引等等，这些都是映射里面可以设置的，其它就是处理es里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好？和建立表结构表关系数据库三范式类似。【是否分词、是否索引、是否存储】

# 依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>transport</artifactId>
        <version>6.6.2</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.9.1</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

# log4j2.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="warn">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%m%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="INFO">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>
```

# 创建索引库

```java
public class Demo1 {
    /** 创建索引库 */
    @Test
    public void test1() throws Exception{
        // 1. 创建Settings配置信息对象(主要配置集群名称)
        // 参数一: 集群key (固定不变)
        // 参数二：集群环境名称,默认的ES的环境集群名称为 "elasticsearch"
        Settings settings = Settings.builder()
                .put("cluster.name", "elasticsearch").build();
        // 2. 创建ES传输客户端对象
        TransportClient transportClient = new PreBuiltTransportClient(settings);
        // 2.1 添加传输地址对象
        // 参数一：主机
        // 参数二：端口
        transportClient.addTransportAddress(new TransportAddress(
                InetAddress.getByName("127.0.0.1"), 9300));
        // 3. 创建索引库(index)
        // 获取索引库管理客户端执行创建索引库，并执行请求
        transportClient.admin().indices().prepareCreate("blog1").get();
        // 4. 释放资源
        transportClient.close();
    }
}
```

# 文档操作

## 添加文档

- 第一种方式

```java
/**
+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 创建文档对象，创建一个json格式的字符串，或者使用XContentBuilder
+ 传输客户端对象把文档添加到索引库中
+ 释放资源
*/
// 3. 创建内容构建对象
    XContentBuilder builder = XContentFactory.jsonBuilder()
        .startObject()
        .field("id", 1)
        .field("title", "elasticsearch搜索服务")
        .field("content","ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎。")
        .endObject();
    // 4. 执行索引库、类型、文档
    transportClient.prepareIndex("blog1","article", "1")
        .setSource(builder).get(); // 执行请求
    // 5. 释放资源
    transportClient.close();
```

- 第二种方式

```java
/**
- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建Map集合，封装文档数据
- 传输客户端对象把文档添加到索引库中
- 释放资源
*/
 // 3. 定义Map集合封装文档
    Map<String, Object> map = new HashMap();
    map.put("id", 2);
    map.put("title", "dubbo分布式服务框架");
    map.put("content", "dubbo阿里巴巴开源的高性能的RPC框架。");
    // 4. 添加文档
    transportClient.prepareIndex("blog1","article", "2")
        .setSource(map).get();
    // 5. 释放资源
    transportClient.close();
```

- 第三种方式

引入jackson依赖

```xml
 <dependency>
     <groupId>com.fasterxml.jackson.core</groupId>
     <artifactId>jackson-databind</artifactId>
     <version>2.9.6</version>
 </dependency>
```

定义pojo实体类

```java
public class Article {
    private long id;
    private String title;
    private String content;
    }
```

```java
/**
- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建pojo对象，封装文档数据
- 把实体对象转化成json字符串
- 传输客户端对象把文档添加到索引库中
- 释放资源
*/
// 3. 创建实体对象
    Article article = new Article();
    article.setId(3);
    article.setTitle("lucene全文检索框架");
    article.setContent("lucene是apache组织开源的全文检索框架。");
    // 4. 把article转化成json字符串
    String jsonStr = new ObjectMapper().writeValueAsString(article);
    // 5. 添加文档
    transportClient.prepareIndex("blog1","article", "3")
        .setSource(jsonStr, XContentType.JSON).get();
    // 6. 释放资源
    transportClient.close();
```

## 批量添加文档

```java
/**
- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建批量请求构建对象
- 批量请求构建对象 循环添加 索引请求对象
- 批量请求构建对象提交请求
- 释放资源
*/
/ 3. 创建批量请求构建对象
    BulkRequestBuilder bulkRequestBuilder = transportClient.prepareBulk();
    long begin = System.currentTimeMillis();
    // 4. 循环创建文档，添加索引请求对象
    for (long i = 1; i <= 1000; i++){
        Article article = new Article();
        article.setId(i);
        article.setTitle("dubbo分布式服务框架" + i);
        article.setContent("dubbo阿里巴巴开源的高性能的RPC框架" + i);
        // 4.1 创建索引请求对象
        IndexRequest indexRequest = new IndexRequest("blog1","article", i + "")
            .source(new ObjectMapper().writeValueAsString(article),
                    XContentType.JSON);
        // 4.2 添加索引请求对象
        bulkRequestBuilder.add(indexRequest);
    }
    // 5. 提交请求
    bulkRequestBuilder.get();
    long end = System.currentTimeMillis();
    System.out.println("毫秒数：" + (end - begin));
    // 6. 释放资源
    transportClient.close();
```

## 修改文档

```java
/**
+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 创建pojo对象，封装文档数据
+ 把实体对象转化成json字符串
+ 传输客户端对象修改文档到索引库
+ 释放资源
*/
// 3. 创建实体对象
    Article article = new Article();
    article.setId(1);
    article.setTitle("lucene全文检索框架");
    article.setContent("lucene是apache组织开源的全文检索框架。");
    // 4. 把article转化成json字符串
    String jsonStr = new ObjectMapper().writeValueAsString(article);
    // 5. 修改文档
    transportClient.prepareUpdate("blog1","article", "1")
        .setDoc(jsonStr, XContentType.JSON).get();
    // 6. 释放资源
    transportClient.close();
```

注意: 修改的时候，如果不存在这个id，会报错(id改成了10000)

## 删除文档

```java
/**
- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 传输客户端对象删除文档
- 释放资源
*/
 // 3. 删除文档
    transportClient.prepareDelete("blog1", "article", "1").get();
    // 4. 释放资源
    transportClient.close();
```

## 删除索引库

```java
/**
+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 索引库管理客户端删除索引库
+ 释放资源
*/
  // 3. 删除索引库
    transportClient.admin().indices().prepareDelete("blog1").get();
    // 4. 释放资源
    transportClient.close();
```

# IK分词器

分词算法

+ 最小切分

  请求地址：[http://127.0.0.1:9200/_analyze](http://127.0.0.1:9200/_analyze?analyzer=ik_smart)

  请求参数：{"analyzer" : "**ik_smart**", "text" : "中国程序员"}

  ![1573522583719](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522583719.png) 

+ 最细粒度切分

  请求地址：[http://127.0.0.1:9200/_analyze](http://127.0.0.1:9200/_analyze?analyzer=ik_smart)

  请求参数：{"analyzer" : "**ik_max_word**", "text" : "中国程序员"}

  ![1573522603197](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522603197.png) 

# 创建映射

Mapping就是定义Document中的每个Field的特征（数据类型，是否存储，是否索引，是否分词等）

+ 类型名称: 就是前面讲的type的概念，类似于数据库中的表。
+ 字段名: 任意填写，可以指定许多属性，例如
  + type：类型，可以是text、long、short、date、integer、object等
  + index：是否索引，默认为true
  + store：是否存储，默认为false
  + analyzer：分词器，这里的ik_max_word即使用ik分词器

```java
/**
- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建索引库管理客户端，创建空的索引库
- 创建映射信息json格式字符串，使用XContentBuilder
- 创建映射请求对象，封装请求信息
- 索引库管理客户端，为索引库添加映射
- 释放资源
*/
 // 3. 创建索引库管理客户端
    IndicesAdminClient indices = transportClient.admin().indices();
    // 3.1 创建空的索引库
    indices.prepareCreate("blog2").get();

    // 4. 创建映射信息json格式字符串，使用XContentBuilder
    XContentBuilder builder = XContentFactory.jsonBuilder();
    builder.startObject()
        .startObject("article")
        .startObject("properties");

    builder.startObject("id")
        .field("type", "long")
        .field("store", true)
        .endObject();

    builder.startObject("title")
        .field("type", "text")
        .field("store", true)
        .field("analyzer", "ik_smart")
        .endObject();

    builder.startObject("content")
        .field("type", "text")
        .field("store", true)
        .field("analyzer","ik_smart")
        .endObject();

    builder.endObject();
    builder.endObject();
    builder.endObject();

    // 5. 创建映射请求对象，封装请求信息
    PutMappingRequest mappingRequest = new PutMappingRequest("blog2")
        .type("article").source(builder);
    // 6. 索引库管理客户端，为索引库添加映射
    indices.putMapping(mappingRequest).get();
    // 7. 释放资源
    transportClient.close();
```

# 查询

## 匹配全部查询

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件)

  ```java
   // 设置查询条件(匹配全部)
   searchRequestBuilder.setQuery(QueryBuilders.matchAllQuery());
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
        // 3. 创建搜索请求构建对象
        SearchRequestBuilder searchRequestBuilder = transportClient
                .prepareSearch("blog2").setTypes("article");
        // 3.1 设置查询条件 (匹配全部)
        searchRequestBuilder.setQuery(QueryBuilders.matchAllQuery());

        // 4. 执行请求，得到搜索响应对象
        SearchResponse searchResponse = searchRequestBuilder.get();

        // 5. 获取搜索结果
        SearchHits hits = searchResponse.getHits();
        System.out.println("总命中数：" + hits.totalHits);

        // 6. 迭代搜索结果
        for (SearchHit hit : hits) {
            System.out.println("JSON字符串：" + hit.getSourceAsString());
            System.out.println("id: " + hit.getSourceAsMap().get("id"));
            System.out.println("title: " + hit.getSourceAsMap().get("title"));
            System.out.println("content: " + hit.getSourceAsMap().get("content"));
        }
        // 7. 释放资源
        transportClient.close();
    }
}
```

## 字符串查询

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件)

  ```JAVA
  // 设置查询条件(字符串查询)
  searchRequestBuilder.setQuery(QueryBuilders.queryStringQuery("搜索服务"));
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件(字符串查询)
    searchRequestBuilder.setQuery(QueryBuilders.queryStringQuery("搜索服务"));

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();

    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        System.out.println("JSON字符串：" + hit.getSourceAsString());
        System.out.println("id: " + hit.getSourceAsMap().get("id"));
        System.out.println("title: " + hit.getSourceAsMap().get("title"));
        System.out.println("content: " + hit.getSourceAsMap().get("content"));
    }
    // 7. 释放资源
    transportClient.close();
}
```

## 词条查询

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件)

  ```java
  // 设置查询条件(词条查询)
  searchRequestBuilder.setQuery(QueryBuilders.termQuery("title","搜索服务"));
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件(词条查询)
    searchRequestBuilder.setQuery(QueryBuilders.termQuery("title","搜索服务"));

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();

    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        System.out.println("JSON字符串：" + hit.getSourceAsString());
        System.out.println("id: " + hit.getSourceAsMap().get("id"));
        System.out.println("title: " + hit.getSourceAsMap().get("title"));
        System.out.println("content: " + hit.getSourceAsMap().get("content"));
    }
    // 7. 释放资源
    transportClient.close();
}
```

## 根据ID查询

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件)

  ```java
  // 设置查询条件(多个主键id)
  searchRequestBuilder.setQuery(QueryBuilders.idsQuery().addIds("2","3"));
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件(多个主键id)
    searchRequestBuilder.setQuery(QueryBuilders.idsQuery().addIds("2","3"));

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();

    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        System.out.println("JSON字符串：" + hit.getSourceAsString());
        System.out.println("id: " + hit.getSourceAsMap().get("id"));
        System.out.println("title: " + hit.getSourceAsMap().get("title"));
        System.out.println("content: " + hit.getSourceAsMap().get("content"));
    }
    // 7. 释放资源
    transportClient.close();
}
```

## 范围查询

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件)

  ```java
  // 设置查询条件(范围查询)
  // from("1", false): 开始(是否包含开始)
  // to("3", false): 结束(是否包含结束)
  searchRequestBuilder.setQuery(QueryBuilders.rangeQuery("id")
                                .from("1", false)
                                .to("3", false));
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件(范围查询)
    // from("1", false): 开始(是否包含开始)
    // to("3", false): 结束(是否包含结束)
    searchRequestBuilder.setQuery(QueryBuilders.rangeQuery("id")
                                  .from("1", false).to("3", false));

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();

    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        System.out.println("JSON字符串：" + hit.getSourceAsString());
        System.out.println("id: " + hit.getSourceAsMap().get("id"));
        System.out.println("title: " + hit.getSourceAsMap().get("title"));
        System.out.println("content: " + hit.getSourceAsMap().get("content"));
    }
    // 7. 释放资源
    transportClient.close();
}
```

## 分页和排序

### 编程步骤

- 创建Settings配置信息对象

- 创建ES传输客户端对象

- 创建搜索请求构建对象(封装查询条件、分页、排序)

  ```java
  // 设置查询条件(匹配查询)
  searchRequestBuilder.setQuery(QueryBuilders.matchQuery("title","服务框架"));
  // 设置分页起始数 (当前页码 - 1) * 页大小
  searchRequestBuilder.setFrom(0);
  // 设置页大小
  searchRequestBuilder.setSize(1);
  // 设置根据id排序(升序)
  searchRequestBuilder.addSort("id", SortOrder.ASC);
  ```

- 执行请求,得到搜索响应对象

- 获取搜索结果

- 迭代搜索结果

- 释放资源

### 代码实现

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件(匹配查询)
    searchRequestBuilder.setQuery(QueryBuilders.matchQuery("title","服务框架"));
    // 3.2 设置分页起始数 (当前页码 - 1) * 页大小
    searchRequestBuilder.setFrom(2);
    // 3.3 设置页大小
    searchRequestBuilder.setSize(1);
    // 3.4 设置根据id排序(升序)
    searchRequestBuilder.addSort("id", SortOrder.ASC);

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();

    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        System.out.println("JSON字符串：" + hit.getSourceAsString());
    }
    // 7. 释放资源
    transportClient.close();
}
```

## 高亮显示

**编程步骤**

+ 创建Settings配置信息对象

+ 创建ES传输客户端对象

+ 创建搜索请求构建对象(封装查询条件、设置高亮对象)

  ```java
  // 设置查询条件
  searchRequestBuilder.setQuery(QueryBuilders.termQuery("content","开源"));
  
  // 创建高亮构建对象
  HighlightBuilder highlightBuilder = new HighlightBuilder();
  // 设置高亮字段
  highlightBuilder.field("content");
  // 设置高亮格式器前缀
  highlightBuilder.preTags("<font color='red'>");
  // 设置高亮格式器后缀
  highlightBuilder.postTags("</font>");
  // 设置高亮对象
  searchRequestBuilder.highlighter(highlightBuilder);
  ```

+ 执行请求,得到搜索响应对象

+ 获取搜索结果

+ 迭代搜索结果(获取高亮内容)

  ```java
  // 获取高亮字段集合
  Map<String, HighlightField> highlightFields = hit.getHighlightFields();
  // 获取content字段的高亮内容
  String content = highlightFields.get("content").getFragments()[0].toString();
  ```

+ 释放资源

**代码实现**

```java
    // 3. 创建搜索请求构建对象
    SearchRequestBuilder searchRequestBuilder = transportClient
        .prepareSearch("blog2").setTypes("article");
    // 3.1 设置查询条件
    searchRequestBuilder.setQuery(QueryBuilders.termQuery("content","开源"));

    // 3.2 创建高亮构建对象
    HighlightBuilder highlightBuilder = new HighlightBuilder();
    // 3.2.1 设置高亮字段
    highlightBuilder.field("content");
    // 3.2.2 设置高亮格式器前缀
    highlightBuilder.preTags("<font color='red'>");
    // 3.2.3 设置高亮格式器后缀
    highlightBuilder.postTags("</font>");
    // 3.3 设置高亮对象
    searchRequestBuilder.highlighter(highlightBuilder);

    // 4. 执行请求，得到搜索响应对象
    SearchResponse searchResponse = searchRequestBuilder.get();
    // 5. 获取搜索结果
    SearchHits hits = searchResponse.getHits();
    System.out.println("总命中数：" + hits.totalHits);

    // 6. 迭代搜索结果
    for (SearchHit hit : hits) {
        // 获取高亮字段集合
        Map<String, HighlightField> highlightFields = hit.getHighlightFields();
        // 获取content字段的高亮内容
        String content = highlightFields.get("content")
                              .getFragments()[0].toString();
        System.out.println(hit.getSourceAsMap().get("id") + "\t"
                           + hit.getSourceAsMap().get("title") + "\t" + content);
    }
    // 7. 释放资源
    transportClient.close();
}
```

