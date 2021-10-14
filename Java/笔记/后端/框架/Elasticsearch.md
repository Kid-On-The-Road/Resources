## 课程目标

目标1: 了解 ElasticSearch 的作用

目标2: 了解 ElasticSearch 安装

目标3: 熟悉 ElasticSearch 相关概念

目标4: 掌握 ElasticSearch java客户端操作

目标5: 掌握 分词器 的作用

目标6: 熟悉 ElasticSearch IK分词器集成

目标7: 掌握 创建索引 的代码实现

目标8: 掌握 删除索引 的代码实现

目标9: 掌握 创建映射 的代码实现

目标10: 掌握 文档增删改查 的代码实现

目标11: 掌握 文档分页 的代码实现

目标12: 掌握 文档高亮查询 的代码实现



## 01、ElasticSearch简介

#### 1.1 什么是ElasticSearch

Elaticsearch，简称为es，es是一个开源的高扩展的分布式全文搜索服务，它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。es也是使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

#### 1.2 ElasticSearch使用案例

+ 2013年初，GitHub抛弃了Solr，采取ElasticSearch来做PB级的搜索。“GitHub使用ElasticSearch搜索20TB的数据，包括13亿文件和1300亿行代码”。
+ 维基百科：启动以elasticsearch为基础的核心搜索架构。
+ SoundCloud：“SoundCloud使用ElasticSearch为1.8亿用户提供即时而精准的音乐搜索服务”。
+ 百度：百度目前广泛使用ElasticSearch作为文本数据分析，采集百度所有服务器上的各类指标数据及用户自定义数据，通过对各种数据进行多维分析展示，辅助定位分析实例异常或业务层面异常。目前覆盖百度内部20多个业务线（包括casio、云分析、网盟、预测、文库、直达号、钱包、风控等），单集群最大100台机器，200个ES节点，每天导入30TB+数据。
+ 新浪使用ES 分析处理32亿条实时日志。
+ 阿里使用ES  构建挖财自己的日志采集和分析体系。

#### 1.3 ElasticSearch对比Solr

+ Solr利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能。
+ Solr支持更多格式的数据，而Elasticsearch仅支持json文件格式。
+ Solr官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能都有第三方插件提供。
+ Solr在传统的搜索应用中表现好于Elasticsearch，但在处理实时搜索应用时效率明显低于  Elasticsearch当单纯的对已有数据进行搜索时，Solr更快,当实时建立索引时, Solr会产生io阻塞，查询性能较差, Elasticsearch具有明显的优势。随着数据量的增加，Solr的搜索效率会变得更低，而Elasticsearch却没有明显的变化。综上所述，**Solr的架构不适合实时搜索的应用**。Solr是传统搜索应用的有力解决方案，但 **Elasticsearch 更适用于新兴的实时搜索应用**。

## 02、ElasticSearch：安装&启动

> 目标: 学会下载与安装ElasticSearch搜索服务

#### 2.1 下载

+ ElasticSearch分为Linux和Window版本，基于我们主要学习的是ElasticSearch的Java客户端的使用，所以我们课程中使用的是安装较为简便的Window版本，项目上线后，公司的运维人员会安装Linux版的ES供我们连接使用。

+ ElasticSearch官方地址：[https://www.elastic.co/cn/products/elasticsearch](https://www.elastic.co/products/elasticsearch)

  ![1573459143961](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573459143961.png)

  ![1573459250193](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573459250193.png)

  ![1573459372487](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573459372487.png)

  ![1573459444499](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573459444499.png) 

  在“资料”中已经提供了下载好的elasticsearch-6.2.2.zip压缩包

  ![1573460192291](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573460192291.png) 

#### 2.2 安装

Window版的ElasticSearch的安装很简单，类似Window版的Tomcat，解压开即安装完毕，解压后的ElasticSearch的目录结构如下:

![1573460207272](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573460207272.png) 

#### 2.3 启动

进入elasticsearch-6.6.2\bin目录，点击elasticsearch.bat启动:

![1573460314105](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1572261981080.png) 

![1573462020231](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573462020231.png) 

![1573461655107](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573461655107.png) 

**注意：启动时可能会出现JVM堆内存不够的错误。需修改elasticsearch-6.6.2/config目录下的配置文件jvm.options**

![1573460440883](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573460440883.png) 

浏览器访问：`http://localhost:9200` 看到如下返回的json信息，代表ES服务启动成功:

![1573460766189](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573460766189.png) 

> **注意：ElasticSearch是用java开发的，且本版本的es需要的jdk版本要是JDK1.8+，并配置好JDK环境变量，否则启动ElasticSearch失败。** 

## 03、ElasticSearch：管理应用部署

ElasticSearch不同于Solr自带图形化界面，我们可以通过安装ElasticSearch的head插件，完成图形化界面的效果，完成索引数据的查看。在资料中已经提供了head插件压缩包。

+ 下载head插件：https://github.com/mobz/elasticsearch-head

  ![1573462353305](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573462353305.png) 

+ 部署elasticsearch-head-master插件

  + 拷贝apache-tomcat-8.5.28.zip到D:\es目录下解压，更名为tomcat-head

    ![1573462523115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573462523115.png) 

  + 删除tomcat-head\webapps目录下全部项目

    ![1573462635549](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573462635549.png) 

  + 拷贝“资料”目录下的elasticsearch-head-master.zip到D:\es\tomcat-head\webapps目录下，解压，更名为ROOT

    ![1573462711945](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573462711945.png) 

  + 启动tomcat，打开浏览器输入 http://localhost:8080，看到如下页面:

    ![1573463206657](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573463206657.png) 

  + head管理应用默认连接不上ES服务端的，会报以下错误：

    ![1573463286436](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573463286436.png) 

    原因: Elasticsearch-head  跨域 访问ES服务端，产生跨域请求错误。

+ 配置ES服务端允许跨域访问

  修改elasticsearch-6.6.2\config目录下的elasticsearch.yml，增加以下配置：

  ```properties
  # 配置跨域
  http.cors.enabled: true 
  http.cors.allow-origin: "*"
  ```

  ![img](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/wps1.jpg) 

  重新启动ES服务，用Head插件连接ES服务，效果如下：

  ![img](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/wps2.jpg)

## 04、ElasticSearch：核心概念

> 目标: 了解ElasticSearch的相关概念: 集群、节点、索引、类型、文档、分片、映射是什么？

#### 4.1 概述

Elasticsearch面向文档(document oriented)的，这意味着它可以存储整个对象或文档(document)。然而它不仅仅是存储，还会索引(index)每个文档的内容使之可以被搜索。在Elasticsearch中，你可以对文档进行索引、搜索、排序、过滤。

关系型数据库与ES索引库类比:

| 关系型数据库  | 数据库    | 表     | 行   | 列      |
| ------------- | --------- | ------ | ---- | ------- |
| Relational DB | Databases | Tables | Rows | Columns |

| ES服务        | 索引库  | 类型  | 文档      | 字段   |
| ------------- | ------- | ----- | --------- | ------ |
| ElasticSearch | Indices | Types | Documents | Fields |

#### 4.2 核心概念

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

#### 4.3 小结

+ 什么是集群

+ > 给所有的es节点，定义一个相同的名字: 默认名字是elasticsearch,就可以自动转换成集群。

+ 什么是节点

  > 每一个es服务就是一个节点。只要节点名字不相同，就可以形成一个集群。

+ 什么是索引(Indices)（重点）

  > 一个索引就是一个拥有相似特征的文档的集合,（足够灵活。数据结构可以不一样）。

+ 什么是类型(Types)（重点）

  > 类型数据的表，进行文档的区分。

+ 什么是文档(Document)（重点）

  > 数据存储的结构，数据结构：json

+ 什么是映射(Mapping)

  > 是定义文档数据结构的属性的规范。比如属性：是否要存储，是否要索引，采用什么样的分词等。

## 05、ElasticSearch：搭建测试环境

#### 5.1 创建测试模块

![1573466182754](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573466182754.png)

#### 5.2 配置依赖

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

#### 5.3 提供log4j2.xml

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

## 06、ElasticSearch：创建索引库

#### 目标

> 掌握创建索引库的代码实现

#### 编程步骤

+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 使用传输客户端对象创建索引库
+ 释放资源

#### 核心代码

```java
package cn.itcast.elasticsearch;

import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.TransportAddress;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import org.junit.Test;
import java.net.InetAddress;

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

浏览器访问: http://localhost:8080

![1573482309057](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573482309057.png)   

#### 小结

> 创建索引库核心代码:
>
> **transportClient.admin().indices().prepareCreate("book").get();**

## 07、ElasticSearch：添加文档

> 目标: 掌握 文档增加 的代码实现

#### 7.1 第一种方式

**编程步骤**

+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 创建文档对象，创建一个json格式的字符串，或者使用XContentBuilder
+ 传输客户端对象把文档添加到索引库中
+ 释放资源

**代码实现**

```java
/** 添加文档: 第一种方式(XContentBuilder) */
@Test
public void test2() throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new
         TransportAddress(InetAddress.getByName("127.0.0.1"), 9300));

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
}
```

#### 7.2 第二种方式

**编程步骤**

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建Map集合，封装文档数据
- 传输客户端对象把文档添加到索引库中
- 释放资源

**代码实现**

```java
/** 创建文档: 第二种方式(使用Map集合) */
@Test
public void test3() throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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
}
```

#### 7.3 第三种方式

**准备工作**

+ 引入jackson依赖

  ```xml
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.9.6</version>
   </dependency>
  ```

+ 定义pojo实体类

  ```java
  package cn.itcast.elasticsearch.pojo;
  
  public class Article {
      private long id;
      private String title;
      private String content;
      public long getId() {
          return id;
      }
      public void setId(long id) {
          this.id = id;
      }
      public String getTitle() {
          return title;
      }
      public void setTitle(String title) {
          this.title = title;
      }
      public String getContent() {
          return content;
      }
      public void setContent(String content) {
          this.content = content;
      }
  }
  ```

**编程步骤**

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建pojo对象，封装文档数据
- 把实体对象转化成json字符串
- 传输客户端对象把文档添加到索引库中
- 释放资源

**代码实现**

```java
/** 创建文档: 第三种方式(使用POJO) */
@Test
public void test4() throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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
}
```

#### 7.4 小结

+ 第一种方式: XContentBuilder封装文档数据
+ 第二种方式: Map集合封装文档数据
+ 第三种方式: pojo实体类封装文档数据

## 08、ElasticSearch：批量添加文档

> 目标: 掌握文档批量增加的代码实现

#### 编程步骤

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建批量请求构建对象
- 批量请求构建对象 循环添加 索引请求对象
- 批量请求构建对象提交请求
- 释放资源

#### 代码实现

```java
/** 批量添加文档 */
@Test
public void test5()throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));
    // 3. 创建批量请求构建对象
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
}
```

## 09、ElasticSearch：修改文档

> 目标: 掌握文档修改的代码实现

#### 编程步骤

+ 创建Settings配置信息对象
+ 创建ES传输客户端对象
+ 创建pojo对象，封装文档数据
+ 把实体对象转化成json字符串
+ 传输客户端对象修改文档到索引库
+ 释放资源

#### 代码实现

```java
/** 修改文档 */
@Test
public void test6() throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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
}
```

注意: 修改的时候，如果不存在这个id，会报错(id改成了10000)

![1573487830588](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573487830588.png)   

## 10、ElasticSearch：删除文档

> 目标: 掌握文档删除的代码实现

#### 编程步骤

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 传输客户端对象删除文档
- 释放资源

#### 代码实现

```java
/** 删除文档 */
@Test
public void test7() throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

    // 3. 删除文档
    transportClient.prepareDelete("blog1", "article", "1").get();
    // 4. 释放资源
    transportClient.close();
}
```

## 11、ElasticSearch：删除索引库

> 目标: 掌握删除索引库的实现

#### 第一种方式

![1573488203069](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573488203069.png) 

#### 第二种方式

+ 操作步骤

  + 创建Settings配置信息对象
  + 创建ES传输客户端对象
  + 索引库管理客户端删除索引库
  + 释放资源

+ 代码实现

  ```java
  /** 删除索引库 */
  @Test
  public void test8() throws Exception{
      // 1. 创建Settings配置信息对象
      Settings settings = Settings.builder()
          .put("cluster.name", "elasticsearch").build();
      // 2. 创建ES传输客户端对象
      TransportClient transportClient = new PreBuiltTransportClient(settings);
      // 2.1 添加传输地址对象
      transportClient.addTransportAddress(new TransportAddress(
          InetAddress.getByName("127.0.0.1"), 9300));
  
      // 3. 删除索引库
      transportClient.admin().indices().prepareDelete("blog1").get();
      // 4. 释放资源
      transportClient.close();
  }
  ```

## 12、ElasticSearch：IK中文分词器

> 目标: 熟悉 ElasticSearch IK分词器集成

ElasticSearch的**默认分词器是单字分词器**，当我们创建索引时，没有特定的进行映射的创建，所以会使用默认的分词器进行分词，即每个字单独分成一个词。 例如：我是程序员 分词后的效果为：我、是、程、序、员 而我们需要的分词效果是：我、是、程序、程序员 这样的话就需要对中文支持良好的分词器，支持中文分词的分词器有很多，word分词器、庖丁解牛、盘古分词、Ansj分词等，但我们常用的还是下面要介绍的**IK分词器**。

![1573521971232](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573521971232.png) 

#### 12.1 IK分词器介绍

+ IKAnalyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包。从2006年12月推出1.0版开始，IKAnalyzer已经推出 了3个大版本。最初，它是以开源项目Lucene为应用主体的，结合词典分词和文法分析算法的中文分词组件。新版本的IKAnalyzer3.0则发展为 面向Java的公用分词组件，独立于Lucene项目，同时提供了对Lucene的默认优化实现。
+ IK分词器3.0的特性如下:
  + 采用了特有的“正向迭代最细粒度切分算法“，具有60万字/秒的高速处理能力。
  + 采用了多子处理器分析模式，支持：英文字母（IP地址、Email、URL）、数字（日期，常用中文数量词，罗马数字，科学计数法），中文词汇（姓名、地名处理）等分词处理。
  + 对中英联合支持不是很好,在这方面的处理比较麻烦.需再做一次查询,同时是支持个人词条的优化的词典存储，更小的内存占用。
  + 支持用户词典扩展定义。
  + 针对Lucene全文检索优化的查询分析器IKQueryParser；采用歧义分析算法优化查询关键字的搜索排列组合，能极大的提高Lucene检索的命中率。

#### 12.2 IK分词器集成

+ 第一步: 下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases 课程资料也提供了IK分词器的压缩包：

  ![1573522114892](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522114892.png) 

+ 第二步: 解压，将解压后的elasticsearch文件夹拷贝到elasticsearch-6.6.2\plugins下，并重命名文件夹为ik

  ![1573522345001](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522345001.png) 

+ 第三步: 重新启动ES服务器，即可加载IK分词器

  ![1573522383705](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522383705.png)  

#### 12.3 IK分词器测试

+ IK提供了两个分词算法**ik_smart** 和 **ik_max_word** 其中 ik_smart 为最少切分，ik_max_word为最细粒度切分。

  + 最小切分

    请求地址：[http://127.0.0.1:9200/_analyze](http://127.0.0.1:9200/_analyze?analyzer=ik_smart)

    请求参数：{"analyzer" : "**ik_smart**", "text" : "中国程序员"}

    ![1573522583719](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522583719.png) 

  + 最细粒度切分

    请求地址：[http://127.0.0.1:9200/_analyze](http://127.0.0.1:9200/_analyze?analyzer=ik_smart)

    请求参数：{"analyzer" : "**ik_max_word**", "text" : "中国程序员"}

    ![1573522603197](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573522603197.png) 


## 13、ElasticSearch：创建映射

> 目标: 掌握 创建映射 的代码实现

Mapping就是定义Document中的每个Field的特征（数据类型，是否存储，是否索引，是否分词等）

+ 类型名称: 就是前面讲的type的概念，类似于数据库中的表。
+ 字段名: 任意填写，可以指定许多属性，例如
  + type：类型，可以是text、long、short、date、integer、object等
  + index：是否索引，默认为true
  + store：是否存储，默认为false
  + analyzer：分词器，这里的ik_max_word即使用ik分词器

#### 编程步骤

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 创建索引库管理客户端，创建空的索引库
- 创建映射信息json格式字符串，使用XContentBuilder
- 创建映射请求对象，封装请求信息
- 索引库管理客户端，为索引库添加映射
- 释放资源

#### 代码实现

```java
/** 创建索引库映射 */
@Test
public void test9()throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name", "elasticsearch").build();
    // 2. 创建ES传输客户端
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"),9300));

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
}
```

![1573525882047](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573525882047.png)  

![1573523917877](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573523917877.png) 

**小结**

+ 映射不可以覆盖, 一定是建完索引库马上去做。
+ 创建映射，如果索引库不存在的会报错。因为它没有自动创建索引库的功能。

## 14、ElasticSearch查询：匹配全部查询

> 目标: 掌握 匹配全部查询 的代码实现

先运行以前添加文档的三个测试方法(修改索引库名称为blog2)。

![1573526457688](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573526457688.png)  

#### 编程步骤

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

#### 代码实现

```java
package cn.itcast.elasticsearch;

import org.elasticsearch.action.search.SearchRequestBuilder;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.TransportAddress;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import org.junit.Test;

import java.net.InetAddress;

public class Demo2 {

    /** 匹配全部 */
    @Test
    public void test1()throws Exception{
        // 1. 创建Settings配置信息对象
        Settings settings = Settings.builder()
                .put("cluster.name","elasticsearch").build();
        // 2. 创建ES传输客户端对象
        TransportClient transportClient = new PreBuiltTransportClient(Settings.EMPTY);
        // 2.1 添加传输地址对象
        transportClient.addTransportAddress(new TransportAddress(
                InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573526308208](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573526308208.png) 

## 15、ElasticSearch查询：字符串查询

> 目标: 掌握 字符串查询 的代码实现

#### 编程步骤

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

#### 代码实现

```java
/** 字符串查询 */
@Test
public void test2()throws Exception{

    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(Settings.EMPTY);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573526909527](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573526909527.png) 

## 16、ElasticSearch查询：词条查询

> 目标: 掌握 词条查询 的代码实现

#### 编程步骤

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

#### 代码实现

```java
/** 词条查询(搜索条件不分词) */
@Test
public void test3()throws Exception{

    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(Settings.EMPTY);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573527275140](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573527275140.png) 

> 注意: 搜索条件不分词。

## 17、ElasticSearch查询：根据ID查询

> 目标: 掌握 根据多个id主键查询 的代码实现

#### 编程步骤

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

#### 代码实现

```java
/** 根据id主键查询*/
@Test
public void test4()throws Exception{

    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(Settings.EMPTY);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573528234404](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573528234404.png) 

## 18、ElasticSearch查询：范围查询

> 目标: 掌握 范围查询 的代码实现

#### 编程步骤

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

#### 代码实现

```java
/** 范围查询 */
@Test
public void test5()throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(Settings.EMPTY);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573528844880](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573528844880.png) 

## 19、ElasticSearch查询：分页和排序

> 目标: 掌握 分页排序查询 的代码实现

#### 编程步骤

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

#### 代码实现

```java
/** 搜索分页、排序 */
@Test
public void test6()throws Exception{

    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));
    
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

运行效果:

![1573529973243](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573529973243.png) 

## 20、ElasticSearch查询：高亮显示

> 目标: 掌握 文档高亮显示 的代码实现

#### 20.1 什么是高亮显示

+ 根据关键字搜索时，搜索出的内容中的关键字会显示不同的颜色，称之为高亮百度搜索关键字"elasticsearch"

  ![1573531763857](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573531763857.png) 

+ 京东商城搜索“iphone xs max”

  ![1573531786291](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573531786291.png) 

#### 20.2 高亮显示html分析

+ 通过开发者工具查看高亮数据的html代码实现:

  ![1573531831288](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573531831288.png) 

  说明: ElasticSearch可以对查询出的内容中关键字部分进行标签和样式的设置，但是你需要告诉ElasticSearch使用什么标签对高亮关键字进行包裹。

#### 20.3 高亮显示实现

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
/** 高亮显示 */
@Test
public void test7()throws Exception{
    // 1. 创建Settings配置信息对象
    Settings settings = Settings.builder()
        .put("cluster.name","elasticsearch").build();
    // 2. 创建ES传输客户端对象
    TransportClient transportClient = new PreBuiltTransportClient(settings);
    // 2.1 添加传输地址对象
    transportClient.addTransportAddress(new TransportAddress(
        InetAddress.getByName("127.0.0.1"), 9300));

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

运行结果:

![1573532677574](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573532677574.png) 

## 21、课程总结

 **今天课程重点**

+ Elasticsearch安装与启动

+ Elasticsearch创建索引库

+ Elasticsearch添加文档

+ Elasticsearch修改文档

+ Elasticsearch删除文档

+ Elasticsearch创建映射

+ Elasticsearch查询

  ![1573543487274](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573543487274.png) 

## == 拓展：ElasticSearch集群搭建 ==

### 集群介绍

+ ES集群是一个P2P类型（使用gossip协议）的分布式系统，除了集群状态管理以外，其他所有的请求都可以发送到集群内任意一台节点上，这个节点可以自己找到需要转发给那些节点，并且直接跟这些节点通信。所以从网络架构及服务配置上来说，构建集群所需要的配置及其简单。在ES2.0之前，无阻碍的网络下，所有配置了相同`cluster.name`的节点都自动归属到一个集群中。2.0版本之后，基于安全的考虑避免开发环境过于随便造成的麻烦，从2.0版本开始，默认的自动默认的发现方式改为了广播（unicast）方式。配置里提供几台节点的地址。ES将其视作gossip router角色，借以完成集群的发现。由于这只是ES内一个很小的功能，索引gossip router角色并不需要单独配置，每个ES节点都可以担任，索引采用广播方式的集群，各节点都配置相同的几个节点列表作为router即可。
+ 集群中节点数量没有限制，一般大于等于2个节点就可以看做是集群了。一般处于高性能及高可用方面来考虑一般集群中的节点数量都是3个及3个以上。

### 集群概念

+ 集群 cluster

  一个集群就是由一个或多个节点组织在一起，它们共同持有整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是“elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群

+ 节点 node

  一个节点是集群中的一个服务器，作为集群的一部分，它存储数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于Elasticsearch集群中的哪些节点。 一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫 做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此， 它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。 在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何Elasticsearch节点， 这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的集群。

+ 分片和复制shards&replicas

  一个索引可以存储超出单个节点硬件限制的大量数据。比如，一个具有10亿文档的索引占据1TB的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。分片很重要，主要有两方面的原因：

  + 允许你水平分割/扩展你的内容容量。
  + 允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量。至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由Elasticsearch管理的，对于作为用户的你来说，这些都是透明的。在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。复制之所以重要，有两个主要原因： 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行。总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。默认情况下，Elasticsearch中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。

### 集群原理

+ 我们面临的第一个问题就是数据量太大，单点存储量有限的问题。大家觉得应该如何解决？我们可以把数据拆分成多份，每一份存储到不同机器节点（node），从而实现减少每个节点数据量的目的。这就是数据的分布式存储，也叫做：数据分片（Shard）。

  ![1573541795932](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573541795932.png) 

+ 数据分片解决了海量数据存储的问题，但是如果出现单点故障，那么分片数据就不再完整，这又该如何解决呢？就像大家为了备份手机数据，会额外存储一份到移动硬盘一样。我们可以给每个分片数据进行备份，存储到其它节点，防止数据丢失，这就是数据备份，也叫数据副本（replica）。

+ 数据备份可以保证高可用，但是每个分片备份一份，所需要的节点数量就会翻一倍，成本实在是太高了！

+ 为了在高可用和成本间寻求平衡，我们可以这样做：首先对数据分片，存储到不同节点

  然后对每个分片进行备份，放到对方节点，完成互相备份这样可以大大减少所需要的服务节点数量，如图，我们以3分片，每个分片备份一份为例：

  ![1573541887624](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573541887624.png) 

  在这个集群中，如果出现单节点故障，并不会导致数据缺失，所以保证了集群的高可用，同时也减少了节点中数据存储量。并且因为是多个节点存储数据，因此用户请求也会分发到不同服务器，并发能力也得到了一定的提升。

### 集群搭建

+ 复制es节点三份，并且删除`data`目录

+ 修改三个节点上的信息，内容如下:

  + 三个节点端口号: http端口号(9201、9202、9203)  tcp端口号(9301、9302、9303)

  + 第一个节点配置(elasticsearch.yml)

    ```properties
    # 集群的名字，保证唯一，所有都必须一致 (17行)
    cluster.name: cluster-es
    # 节点名称，必须不一样 (23行)
    node.name: node-1
    # 必须为本机的ip地址 (55行)
    network.host: 127.0.0.1
    # 服务器断开，在同一机器下必须不一样 (59行)
    http.port: 9201
    # 集群间通讯端口号，在同一机器下必须不一样 (60行)
    transport.tcp.port: 9301
    # 设置集群自动发现机器ip:port集合，采用广播模式 (70行)
    discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
    # 防止脑裂。声明大于几个的投票主节点有效，请设置为（nodes / 2） + 1 (75行)
    discovery.zen.minimum_master_nodes: 2
    
    # 允许跨域 (92、93行)
    http.cors.enabled: true
    http.cors.allow-origin: "*"
    ```

  + 第二个节点配置(elasticsearch.yml)

    ```properties
    # 集群的名字，保证唯一，所有都必须一致 (17行)
    cluster.name: cluster-es
    # 节点名称，必须不一样 (23行  改)
    node.name: node-2
    # 必须为本机的ip地址 (55行)
    network.host: 127.0.0.1
    # 服务器断开，在同一机器下必须不一样 (59行 改)
    http.port: 9202
    # 集群间通讯端口号，在同一机器下必须不一样 (60行 改)
    transport.tcp.port: 9302
    # 设置集群自动发现机器ip:port集合，采用广播模式 (70行)
    discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
    # 防止脑裂。声明大于几个的投票主节点有效，请设置为（nodes / 2） + 1 (75行)
    discovery.zen.minimum_master_nodes: 2
    
    # 允许跨域 (92、93行)
    http.cors.enabled: true
    http.cors.allow-origin: "*"
    ```

  + 第三个节点配置(elasticsearch.yml)

    ```properties
    # 集群的名字，保证唯一，所有都必须一致 (17行)
    cluster.name: cluster-es
    # 节点名称，必须不一样 (23行 改)
    node.name: node-3
    # 必须为本机的ip地址 (55行)
    network.host: 127.0.0.1
    # 服务器断开，在同一机器下必须不一样 (59行 改)
    http.port: 9203
    # 集群间通讯端口号，在同一机器下必须不一样 (60行 改)
    transport.tcp.port: 9303
    # 设置集群自动发现机器ip:port集合，采用广播模式 (70行)
    discovery.zen.ping.unicast.hosts: ["127.0.0.1:9301","127.0.0.1:9302","127.0.0.1:9303"]
    # 防止脑裂。声明大于几个的投票主节点有效，请设置为（nodes / 2） + 1 (75行)
    discovery.zen.minimum_master_nodes: 2
    
    # 允许跨域 (92、93行)
    http.cors.enabled: true
    http.cors.allow-origin: "*"
    ```

+ 分别启动三个es节点

+ 通过es-header-master查看，连接任意一个节点都可以出现下面的效果，说明集群成功。

  ![1573534273127](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573534273127.png)  

### 集群操作

**编程步骤**

- 创建Settings配置信息对象
- 创建ES传输客户端对象
- 使用传输客户端对象创建索引库
- 释放资源

**核心代码**

+ 连接集群

  ```java
  // 1. 创建Settings配置信息对象(主要配置集群名称)
  // 参数一: 集群key (固定不变)
  // 参数二：集群环境名称,默认的ES的环境集群名称为 "elasticsearch"
  Settings settings = Settings.builder()
      .put("cluster.name", "cluster-es").build();
  // 2. 创建ES传输客户端对象
  TransportClient transportClient = new PreBuiltTransportClient(settings);
  // 2.1 添加传输地址对象(集群环境多个)
  // 参数一：主机
  // 参数二：端口
  transportClient.addTransportAddress(new
          TransportAddress(InetAddress.getByName("127.0.0.1"), 9301));
  transportClient.addTransportAddress(new
          TransportAddress(InetAddress.getByName("127.0.0.1"), 9302));
  transportClient.addTransportAddress(new
          TransportAddress(InetAddress.getByName("127.0.0.1"), 9303));
  ```

+ 设置分片与副本

  ```java
  // 3. 创建索引库(index)
  // 获取创建索引库请求构建对象
  CreateIndexRequestBuilder cirb = transportClient.admin().indices()
      .prepareCreate("blog1");
  // 3.1 创建Map集合封装分片与副本设置信息
  Map<String, Integer> source = new HashMap<String, Integer>();
  // 3.2 设置分片数量
  source.put("number_of_shards", 3);
  // 3.3 设置副本数量
  source.put("number_of_replicas", 1);
  // 3.4 设置map集合，执行请求
  cirb.setSettings(source).get();
  ```

  ![1573542875065](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573542875065.png) 

**完整代码**

```java
package cn.itcast.elasticsearch;

import org.elasticsearch.action.admin.indices.create.CreateIndexRequestBuilder;
import org.elasticsearch.action.search.SearchRequestBuilder;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.TransportAddress;
import org.elasticsearch.common.xcontent.XContentBuilder;
import org.elasticsearch.common.xcontent.XContentFactory;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.transport.client.PreBuiltTransportClient;
import org.junit.Test;

import java.net.InetAddress;
import java.util.HashMap;
import java.util.Map;

public class Demo3 {

    /** 创建索引库(集群环境) */
    @Test
    public void test1() throws Exception{
        // 1. 创建Settings配置信息对象(主要配置集群名称)
        // 参数一: 集群key (固定不变)
        // 参数二：集群环境名称,默认的ES的环境集群名称为 "elasticsearch"
        Settings settings = Settings.builder()
                .put("cluster.name", "cluster-es").build();
        // 2. 创建ES传输客户端对象
        TransportClient transportClient = new PreBuiltTransportClient(settings);
        // 2.1 添加传输地址对象(集群环境多个)
        // 参数一：主机
        // 参数二：端口
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9301));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9302));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9303));

        // 3. 创建索引库(index)
        // 获取创建索引库请求构建对象
        CreateIndexRequestBuilder cirb = transportClient.admin().indices()
                .prepareCreate("blog1");
        // 3.1 创建Map集合封装分片与副本设置信息
        Map<String, Integer> source = new HashMap<String, Integer>();
        // 3.2 设置分片数量
        source.put("number_of_shards", 3);
        // 3.3 设置副本数量
        source.put("number_of_replicas", 1);
        // 3.4 设置map集合，执行请求
        cirb.setSettings(source).get();

        // 4. 释放资源
        transportClient.close();
    }

    /** 添加文档 (集群环境) */
    @Test
    public void test2() throws Exception{
        // 1. 创建Settings配置信息对象(主要配置集群名称)
        Settings settings = Settings.builder()
                .put("cluster.name", "cluster-es").build();
        // 2. 创建ES传输客户端对象
        TransportClient transportClient = new PreBuiltTransportClient(settings);
        // 2.1 添加传输地址对象
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9301));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9302));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9303));

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
    }


    /** 匹配全部 (集群环境) */
    @Test
    public void test3()throws Exception{

        // 1. 创建Settings配置信息对象(主要配置集群名称)
        Settings settings = Settings.builder()
                .put("cluster.name", "cluster-es").build();
        // 2. 创建ES传输客户端对象
        TransportClient transportClient = new PreBuiltTransportClient(settings);
        // 2.1 添加传输地址对象
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9301));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9302));
        transportClient.addTransportAddress(new
                TransportAddress(InetAddress.getByName("127.0.0.1"), 9303));

        // 3. 创建搜索请求构建对象
        SearchRequestBuilder searchRequestBuilder = transportClient
                .prepareSearch("blog1").setTypes("article");
        // 3.1 设置查询条件(匹配全部)
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

注意: 修改cluster.name的集群名字保持和配置中的一致。

![1573542567904](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/elasticsearch/1573542567904.png) 

> 说明: 测试关闭一个es节点，整个集群还是可以正常访问。