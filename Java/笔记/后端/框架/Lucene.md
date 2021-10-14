## 课程目标

目标1: 掌握索引流程的代码实现

目标2: 掌握检索流程的代码实现

目标3: 熟悉Lucene分词器

目标4: 了解第三方IK分词器

目标5: 了解Lucene的Field域



## 01、搜索实现方案

> 目标: 了解两种搜索实现方案

思考: 什么是搜索？如何实现高并发搜索？

**方案一(传统方案)**

![1552028934964](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1552028934964.png)&nbsp;

**方案二(Lucene方案)**

![1552029202403](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1552029202403.png)&nbsp;

+ 实现搜索有哪两种方案？

  ```txt
  1. 传统方案  // select * from article where content like '%java%'
     弊端：不能用到索引, 'java%',速度比较慢
     
  2. Lucene方案  // 全文搜索
  ```

+ Lucene搜索，需要访问数据库吗?

  ```txt
  不需要 //索引库
  ```

  

## 02、搜索技术应用场景

**应用场景:**

- 单机软件搜索（idea）

- 站内搜索（京东，淘宝）

- 垂直搜索（限定行业，比如教育，医疗搜索）

- 平台搜索（百度，360，搜狗）

  

## 03、数据查询方法

> 目标: 掌握 两种数据查询 方法

#### 3.1 顺序扫描法

+ 举个例子：比如我们有大量的文件，文件编号从A，B，C。。。。。。

+ 需求：要找出文件内容中包含有java的所有文件

+ 需求实现：从A文件开始查找，再找B文件，然后再找C文件，以此类推。。。。。

  **特点：如果文件数量很多，查找速度慢！！！**

  

#### 3.2 倒排索引法

+ 举个例子：使用新华字典查找汉字，先找到汉字的偏旁部首，再根据偏旁部首对应的目录（索引）找到目标汉字。

  ![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1552028373084.png) 

+ 以lucene为例建立倒排索引：

  + 文件一(编号0): we like java java java
  + 文件二(编号1): we like lucene lucene lucene

| （Term 词条） | (Doc 文档，Freq 频率) | （Pos 位置） |
| ------------- | --------------------- | ------------ |
| we            | （0,1）（1,1）        | (0)(0)       |
| like          | (0,1) (1,1)           | (1,1)        |
| java          | (0,3)                 | (2,3,4)      |
| lucene        | (1,3)                 | (2,3,4)      |

![1568083158063](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1568083158063.png)

#### 3.3 小结

+ 建立倒排索引，就是建立词语与文件的对应关系（词语在什么文件出现，出现了几次，在什么位置出现）

+ 搜索的时候，直接根据搜索关键词（java），在倒排索引中找到目标内容。

  

## 04、Lucene介绍

#### 4.1 全文检索是什么?

- 索引流程：计算机通过索引程序扫描文件中的每一个词语，建立词语与文件的对应关系
- 检索流程：计算机通过检索程序，根据搜索关键词，在索引库查找目标内容

#### 4.2 Lucene是什么?

+ Lucene是apache软件基金会下的一个子项目。是一个成熟、免费、开放源代码的全文检索引擎工具包。提供了一套简单易用的API，方便在目标系统中实现全文检索功能。目前已经有很多应用系统的搜索功能是基于lucene来实现。比如eclipse、idea帮助系统的搜索功能。
+ Lucene能够为文本类型的数据建立索引，只需要把数据转换成文本格式，lucene就可以对文档进行索引和搜索。比如常见的word文档、html文档、pdf文档。首先将文档内容转换成文本格式，交给lucene进行索引，把建立好的索引保存在硬盘或者内存中。然后根据用户输入的查询条件，在索引文件中查找，返回查询结果给用户。

#### 4.3 Lucene&搜索引擎区别

+ Lucene是一个全文检索引擎工具包，相当于汽车发动机；搜索引擎基于全文检索实现，是一个可以独立运行的软件产品，相当于汽车。

#### 4.4 Lucene官方网站

+ 官方网站：http://lucene.apache.org/

  ![1573369910275](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573369910275.png)

## 05、全文检索流程介绍

> 目标: 了解全文检索流程

#### 5.1 索引和检索流程图

![img](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/wps1.png)

#### 5.2 索引流程详细介绍

+ 原始数据

  + 保存在关系数据库中的业务数据
  + 保存在文件中的数据
  + 网络上的网页文件数据

+ 采集数据(获取文档)

  + 通过JDBC操作获取到关系数据库中的业务数据

  + 通过IO流获取文件上的数据

  + 通过爬虫（蜘蛛）程序获取网络上的网页数据

    信息采集的开源软件：

    ```txt
    1. Solr（http://lucene.apache.org/solr），solr是apache的一个子项目，支持从关系数据库、xml文档中提取原始数据。
    
    2. Nutch（http://lucene.apache.org/nutch）, Nutch是apache的一个子项目，包括大规模爬虫工具，能够抓取和分辨web网站数据。
    
    3. jsoup（http://jsoup.org/），jsoup 是一款Java 的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSS以及类似于jQuery的操作方法来取出和操作数据。
    ```

+ 创建文档对象【重点】

  + 说明：文档对象（Document），一个文档对象包含有多个域（Field）。一个文档对象就相当于关系数据库表中的一条记录，一个域就相当于一个字段。

    ![1573370577508](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573370577508.png)

+ 分析文档对象

  + 把原始数据，转换成文档对象后，使用分析器（分词器）把文档域中的数据切分成一格一格词语。为后续建立索引做准备。

+ 建立索引

  + 建立词语和文档的对应关系，词语在什么文档出现，出现了几次，在什么位置出现（倒排索引）。并且保存到索引库。

#### 5.3 检索流程详细介绍

+ 用户

  + 用户可以是自然人，也可以是程序。

+ 用户查询

  + 用户在搜索入口界面，输入搜索关键词，执行搜索

    ![1573371438460](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573371438460.png)

+ 建立查询对象【重点】

  + 根据用户输入的搜索关键词，使用分析器分词以后，建立查询对象（Query），Query对象会生成具体的查询语法。bookName:java，表示搜索图书名称域中包含有java的图书。

+ 执行搜索

  + 根据查询对象（Query），和Query生成的语法，在索引库中查询目标内容。

+ 返回查询结果

  + 提供一个搜索结果页面，把搜索结果友好的展示给用户（搜索关键词是高亮显示，搜索结果有排序）

    ![1573371510715](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573371510715.png)

#### 5.4 数据库&索引库对比

|  数据库   |   表   |  行  |   列    |
| :-------: | :----: | :--: | :-----: |
| Databases | Tables | Rows | Cloumns |

|       索引库       |    行     |   列   |
| :----------------: | :-------: | :----: |
| Lucene索引存储目录 | Documents | Fields |

## 06、Lucene搭建测试环境

#### 6.1 准备环境

+ jdk：1.8+
+ ide：idea
+ 数据库：mysql
+ Lucene：4.10.3

#### 6.2 准备数据

   先创建“lucene_db”数据库，再执行“资料\book.sql”

 ![1573372151188](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573372151188.png)

#### 6.3 准备Lucene

![1573372332543](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573372332543.png)

#### 6.4 创建项目

![1573372395265](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573372395265.png) 

#### 6.5 安装lombok插件

我们编写pojo时，经常需要编写: 构造器、getter setter方法,当属性多的时候，就非常浪费时间，使用lombok插件可以解决这个问题:

![1573698250140](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573698250140.png)

#### 6.6 配置pom.xml

+ 引入数据库驱动包、lucene依赖包、lombok依赖包、junit依赖包

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
           http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
  
      <groupId>cn.itcast</groupId>
      <artifactId>lucene-test</artifactId>
      <packaging>jar</packaging>
      <version>1.0-SNAPSHOT</version>
  
      <properties>
          <mysql.version>5.1.47</mysql.version>
          <lucene.version>4.10.3</lucene.version>
          <junit.version>4.12</junit.version>
      </properties>
  
      <dependencies>
          <!-- mysql -->
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>${mysql.version}</version>
          </dependency>
          <!-- 分词器 --> 
          <dependency> 
              <groupId>org.apache.lucene</groupId> 
              <artifactId>lucene-analyzers-common</artifactId> 
              <version>${lucene.version}</version> 
          </dependency>
          <!-- 查询解析器 -->
          <dependency>
              <groupId>org.apache.lucene</groupId>
              <artifactId>lucene-queryparser</artifactId>
              <version>${lucene.version}</version>
          </dependency>
          <!-- junit -->
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>${junit.version}</version>
              <scope>test</scope>
          </dependency>
          <!-- lombok -->
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <version>1.18.6</version>
              <scope>provided</scope>
          </dependency>
      </dependencies>
  
      <build>
          <plugins>
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-compiler-plugin</artifactId>
                  <version>3.3</version>
                  <configuration>
                      <source>1.8</source>
                      <target>1.8</target>
                      <encoding>utf-8</encoding>
                  </configuration>
              </plugin>
          </plugins>
      </build>
  </project>
  ```

## 07、原始数据采集

#### 7.1 编写图书实体类

lombok的@Data注解: 自动为实体类提供getter、setter、hashCode、equals、toString方法。

```java
package cn.itcast.lucene.pojo;
import lombok.Data;

/** 图书实体类 */
@Data
public class Book {
    // 图书id
    private Integer id;
    // 图书名称
    private String bookName;
    // 图书价格
    private float price;
    // 图书图片
    private String pic;
    // 图书描述
    private String bookDesc;
}
```

#### 7.2 编写图书dao接口

```java
package cn.itcast.lucene.dao;
import cn.itcast.lucene.pojo.Book;
import java.util.List;

/** 图书数据访问接口 */
public interface BookDao {
    /** 查询全部图书 */
    List<Book> findAll();
}
```

#### 7.3 编写图书dao实现类

```java
package cn.itcast.lucene.dao.impl;

import cn.itcast.lucene.dao.BookDao;
import cn.itcast.lucene.pojo.Book;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

/** 图书数据访问接口实现类 */
public class BookDaoImpl implements BookDao {

    public List<Book> findAll() {
        // 创建List集合封装查询结果
        List<Book> bookList = new ArrayList<Book>();
        Connection connection = null;
        PreparedStatement psmt = null;
        ResultSet rs = null;
        try{
            // 加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 创建数据库连接对象
            connection = DriverManager.
                 getConnection("jdbc:mysql://localhost:3306/lucene_db","root","root");
            // 编写sql语句
            String sql = "select * from book";
            // 创建statement
            psmt = connection.prepareStatement(sql);
            // 执行查询
            rs = psmt.executeQuery();
            // 处理结果集
            while (rs.next()){
                // 创建图书对象
                Book book = new Book();
                book.setId(rs.getInt("id"));
                book.setBookName(rs.getString("bookname"));
                book.setPrice(rs.getFloat("price"));
                book.setPic(rs.getString("pic"));
                book.setBookDesc(rs.getString("bookdesc"));
                bookList.add(book);
            }
        }catch (Exception ex){
            ex.printStackTrace();
        }finally {
            // 释放资源
            try{
                if(rs != null) rs.close();
                if(psmt != null) psmt.close();
                if(connection != null) connection.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        return bookList;
    }
}
```

## 08、索引流程实现【掌握】

##### 目标

> 掌握索引流程实现代码

##### 实现步骤

+ 采集数据
+ 创建文档对象（Document）
+ 创建分析器（Analyzer），用于分词
+ 创建索引库配置对象（IndexWriterConfig），配置索引库
+ 创建索引库目录对象（Directory），指定索引库的位置
+ 创建索引库操作对象（IndexWriter），把文档对象写入索引库
+ 提交事务
+ 释放资源

##### 核心代码

```java
package cn.itcast.lucene;

import cn.itcast.lucene.dao.BookDao;
import cn.itcast.lucene.dao.impl.BookDaoImpl;
import cn.itcast.lucene.pojo.Book;
import org.apache.lucene.analysis.Analyzer;
import org.apache.lucene.analysis.standard.StandardAnalyzer;
import org.apache.lucene.document.Document;
import org.apache.lucene.document.Field;
import org.apache.lucene.document.TextField;
import org.apache.lucene.index.IndexWriter;
import org.apache.lucene.index.IndexWriterConfig;
import org.apache.lucene.store.Directory;
import org.apache.lucene.store.FSDirectory;
import org.apache.lucene.util.Version;
import org.junit.Test;
import java.io.File;
import java.util.ArrayList;
import java.util.List;

/** 索引管理类 */
public class IndexManager {

    @Test
    public void createIndex() throws Exception{
        // 采集数据
        BookDao bookDao = new BookDaoImpl();
        List<Book> bookList = bookDao.findAll();
      
        // 创建文档集合对象
        List<Document> documents = new ArrayList<Document>();
        for (Book book : bookList){
            // 创建文档对象
            Document doc = new Document();
            /**
             * 给文档对象添加域
             * 方法：add（）
             * 参数：TextField
             * TextField参数：
             *   参数一：域的名称
             *   参数二：域的值
             *   参数三：指定是否把域值存储到文档对象中
             */
            // 图书id
            doc.add(new TextField("id", book.getId() + "", Field.Store.YES));
            // 图书名称
            doc.add(new TextField("bookName",book.getBookName(), Field.Store.YES));
            // 图书价格
            doc.add(new TextField("bookPrice",book.getPrice() + "", Field.Store.YES));
            // 图书图片
            doc.add(new TextField("bookPic",book.getPic(), Field.Store.YES));
            // 图书描述
            doc.add(new TextField("bookDesc",book.getBookDesc(), Field.Store.YES));
            documents.add(doc);
        }
        // 创建分词器(Analyzer)，用于分词
        Analyzer analyzer = new StandardAnalyzer();
        // 创建索引库配置对象，用于配置索引库
        IndexWriterConfig indexWriterConfig =
                new IndexWriterConfig(Version.LUCENE_4_10_3, analyzer);
        // 设置索引库打开模式(每次都重新创建)
        indexWriterConfig.setOpenMode(IndexWriterConfig.OpenMode.CREATE);
        // 创建索引库目录对象，用于指定索引库存储位置
        Directory directory = FSDirectory.open(new File("F:\\index"));
        // 创建索引库操作对象，用于把文档写入索引库
        IndexWriter indexWriter = new IndexWriter(directory, indexWriterConfig);
        // 循环文档，写入索引库
        for (Document document : documents){
            /** addDocument方法：把文档对象写入索引库 */
            indexWriter.addDocument(document);
		   // 提交事务
		   indexWriter.commit();
	    }
        // 释放资源
        indexWriter.close();
    }
}
```

执行结果:

![1573374085637](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573374085637.png)&nbsp;

##### 小结

+ 创建索引的代码流程是？

  ```txt
  采集数据 -> 转换文档对象 -> 数据分析（分词） -> 创建索引库 -> 写入索引库 -> 释放资源
  ```

+ 用到哪些核心API?

  + Document 文档(相当于数据库表中的一行数据)

  + TextField 文本域(表中的一列数据)

  + Analyzer 分词器(为文档中的Field的value进行分值)

  + IndexWriterConfig 写索引配置信息

  + Directory 存储目录

  + **IndexWriter 写索引**


## 09、luke查看索引数据

##### 目标

> 了解luke客户端工具的使用

##### 实现步骤

+ 【资料\luke\start.bat】

  ![1573374491899](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573374491899.png)&nbsp;

+ 运行界面一

  ![1573374519863](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573374519863.png)

+ 运行界面二

  ![1573374554833](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573374554833.png)

+ 运行界面三

  ![1573374577960](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573374577960.png)

+ 通过luke客户端工具进行搜索

  ![1573375216305](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573375216305.png)

##### 小结

+ 索引、片段、文档、域、词条 之间关系

  ![1552036730510](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1552036730510.png)&nbsp;

## 10、检索流程实现【掌握】

##### 目标

> 掌握检索流程实现代码

##### 实现步骤

- 创建分析器对象（Analyzer），用于分词
- 创建查询对象（Query）
- 创建索引库的目录（Directory），指定索引库的位置
- 创建索引读取对象（IndexReader），把索引数据读取到内存中
- 创建索引搜索对象（IndexSearcher），执行搜索
- 使用IndexSearcher执行搜索，返回搜索结果集（TopDocs）
- 处理结果集
- 释放资源

##### 核心代码

```java
/** 搜索引索库 */
@Test
public void searchIndex()throws Exception{

    // 创建分析器对象，用于分词
    Analyzer analyzer = new StandardAnalyzer();
    // 创建查询解析器对象
    QueryParser queryParser = new QueryParser("bookName", analyzer);
    // 解释查询字符串，得到查询对象
    Query query = queryParser.parse("bookName:java");
    // 创建索引库存储目录
    Directory directory = FSDirectory.open(new File("F:\\index"));
    // 创建IndexReader读取索引库对象
    IndexReader indexReader = DirectoryReader.open(directory);
    // 创建IndexSearcher，执行搜索索引库
    IndexSearcher indexSearcher = new IndexSearcher(indexReader);
    /**
     * search方法：执行搜索
     * 参数一：查询对象
     * 参数二：指定搜索结果排序后的前n个（前10个）
     */
    TopDocs topDocs = indexSearcher.search(query, 10);
    // 处理结果集
    System.out.println("总命中的记录数：" + topDocs.totalHits);
    // 获取搜索到得文档数组
    ScoreDoc[] scoreDocs = topDocs.scoreDocs;
    // ScoreDoc对象：只有文档id和分值信息
    for (ScoreDoc scoreDoc : scoreDocs){
        System.out.println("--------华丽分割线----------");
        System.out.println("文档id: " + scoreDoc.doc
                + "\t文档分值：" + scoreDoc.score);
        // 根据文档id获取指定的文档
        Document doc = indexSearcher.doc(scoreDoc.doc);
        System.out.println("图书Id：" + doc.get("id"));
        System.out.println("图书名称：" + doc.get("bookName"));
        System.out.println("图书价格：" + doc.get("bookPrice"));
        System.out.println("图书图片：" + doc.get("bookPic"));
        System.out.println("图书描述：" + doc.get("bookDesc"));
    }
    // 释放资源
    indexReader.close();
}
```

##### 小结

+ 检索程序执行流程是？

  ```txt
  创建Analyzer分析对象
  创建Query查询对象
  创建Directory目录对象 
  创建IndexReader 读取索引对象
  创建IndexSearcher搜索对象、执行搜索、释放资源。
  ```

+ 用到哪些核心API?

  - Analyzer 分词器

  - QueryParser 查询解析器

  - Query 查询

  - Directory 存储目录

  - IndexReader 读索引

  - **IndexSearcher 搜索索引**

  - TopDocs 前面文档

  - ScoreDoc 分数文档

    

## 11、分词器：介绍与作用

##### 目标

> 了解分词器的基本概念与作用

##### 分词器介绍

+ 在对文档（Document）中的内容进行索引前，需要对域（Field）中的内容使用分析对象（分词器）进行分词。分词的目的是为了索引，索引的目的是为了搜索。过程是先分词，再过滤。

  + 分词: 将Document中Field域的值切分成一个一个的单词。具体的切分方法（算法）根据使用的分词器而不同。
  + 过滤: 去除标点符号，去除停用词（的、啊、是、is、the、a等），词的大写转换小写。
  + 停用词说明: 停用词是指为了节省存储空间和提高搜索效率，搜索引擎在索引内容或处理搜索请求时会自动忽略的字词，这些字或词被称为“stop words”。比如语气助词、副词、介词、连接词等等，通常自身没有明确的含义，只有放在一个上下文语句中才有意义，比如常见的有：的、在、是、啊等。

  ![1573378532056](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573378532056.png)

+ Lucene自带分词器(一般都不使用)

  ![1573379038081](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573379038081.png)

##### 分词器作用

+ 【索引流程】: 把原始数据转换成文档对象后，使用分词器把文档域中的内容切分成一个一个的词语，目的是方便后续建立索引。

+ 【检索流程】: 根据用户输入的搜索关键词，使用分词器对象分析以后，建立成查询对象（Query），在索引库中查找目标内容。

  **注意事项：索引流程和检索流程使用分词器要一致。**


## 12、分词器：中文分词器

##### 目标

> 了解常见的中文分词器

##### 中文分词器介绍

+ 我们知道英文本身是以单词为单位，单词与单词之间，句子之间通常是空格、逗号、句号分隔。因此对于英文，可以简单的以空格来判断某个字符串是否是一个词，比如：I love China，love和China很容易被程序处理。
+ 但是中文是以字为单位的，字与字再组成词，词再组成句子。中文：我爱中国，电脑不知道“爱中”是一个词，还是“中国”是一个词？所以我们需要一定的规则来告诉电脑应该怎么切分，这就是中文分词器所要解决的问题。常见的有一元切分法“我爱中国”：我、爱、中、国。二元切分法“我爱中国”：我爱，爱中、中国。

##### 常用开源分词器

+ paoding：庖丁解牛分词器，最新版本在<https://code.google.com/p/paoding/>可以下载。由于没有持续更新，只支持到lucene3.0，项目中不予以考虑使用。
+ mmseg4j：最新版已从<https://code.google.com/p/mmseg4j/>移至<https://github.com/chenlb/mmseg4j-solr>。支持Lucene4.10，且在github中有持续更新，使用的是mmseg算法。
+ IK-analyzer：最新版在https://code.google.com/p/ik-analyzer/上，支持Lucene 4.10从2006年12月推出1.0版开始， IKAnalyzer已经推出了4个大版本。最初，它是以开源项目Luence为应用主体的，结合词典分词和文法分析算法的中文分词组件。从3.0版本开 始，IK发展为面向Java的公用分词组件，独立于Lucene项目，同时提供了对Lucene的默认优化实现。**适合在项目中应用**。

##### 小结

+ 常见的中文分词器有哪些？

  ```txt
  paoding / mmseg / ik
  ```



## 13、分词器：IK分词器使用

##### 目标

> 掌握IK中文分词器的使用

  IK分词器本身就是对Lucene提供的分词器Analyzer扩展实现，使用方式与Lucene的分词器一致。【资料\IK Analyzer 2012FF_hf1.zip】:

![1573380242197](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380242197.png)&nbsp;

##### 基本使用

+ 引入依赖(pom.xml)

  ```xml
  <!-- ik分词器 -->
  <dependency>
      <groupId>com.janeluo</groupId>
      <artifactId>ikanalyzer</artifactId>
      <version>2012_u6</version>
  </dependency>
  ```

+ 修改索引流程使用IK分词器

  ![1573380377887](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380377887.png)&nbsp;

+ 修改检索流程使用IK分词器

  ![1573380397600](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380397600.png)&nbsp;

+ 重新创建索引

  ![1573380426282](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380426282.png)&nbsp;

##### 扩展中文词库

+ 在企业项目中，有一些词语根据业务需要不需要分词，需要作为一个整体，比如：传智播客。有一些词语会过时，不需要，要作为停用词处理。通过配置文件方式实现。

![1573380611021](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380611021.png)

+ 拷贝ik分词器的配置文件

  ![1573380663112](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380663112.png)&nbsp;

+ 扩展词演示

  ![1573381041291](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573381041291.png)&nbsp;

  在ext.dic中增加扩展词: 传智播客

  ![1573380814545](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380814545.png)&nbsp;

  重新创建索引

  ![1573380856670](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573380856670.png)&nbsp;

+ 停用词演示

  ![1573381177737](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573381177737.png)&nbsp;

  在stopword.dic文件中增加停用词: 从、到

  ![1573381237083](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573381237083.png)&nbsp;

  重新创建索引

  ![1573381271725](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573381271725.png)&nbsp;

##### 小结

+ IK分词器怎么使用？

  ```txt
  导入依赖，然后new IKAnalyzer();
  ```

+ IK 扩展词库的作用是？

  ```txt
  扩展2012年后的新的词
  ```

+ IK 自定义停用词库的作用是？

  ```txt
  不给别人搜索的词
  ```

## 14、Field：特性与种类

##### 目标

> 熟悉 Field 的特性与种类

##### Field的特性

Document（文档）是Field（域）的承载体，一个Document是由多个Field组成。Field由名称和值两部分组成，Field的值是要索引的内容，也是要搜索的内容。

+ 是否分词（**tokenized**）

  ```txt
  是：将Field的值进行分词处理，分词的目的是为了索引。
  比如：商品名称，商品描述。这些内容用户需要输入关键词进行查询，由于内容格式大，内容多，需要进行分词处理建立索引。
  
  否：不做分词处理
  比如：订单编号，身份证号。是一个整体，分词以后没有意义，不需要分词。
  ```

+ 是否索引（**indexed**）

  ```txt
  是：将Field内容进行分词处理后得到的词或者整体Field内容建立索引，存储到索引域。索引的目的是为了搜索。
  比如：商品名称，商品描述需要分词建立索引。订单编号，身份证号作为整体建立索引。只要将来要作为用户查询条件的词，都需要索引。
  
  否：不索引。
  比如：商品图片路径，不作为查询条件，不需要建立索引。
  ```

+ 是否存储（**stored**）

  ```txt
  是：将Field值保存到Document中。
  比如：商品名称，商品价格。凡是将来在搜索结果页面展现给用户的内容，都需要存储。
  
  否：不存储。
  比如：商品描述。内容多格式大，不需要直接在搜索结果页面展现，不做存储。需要的时候可以从关系数据库取。
  ```

##### Field的种类

| Field种类                                      | 数据   类型            | 是否   分词 | 是否   索引 | 是否   存储 | 说明                                                         |
| ---------------------------------------------- | ---------------------- | ----------- | ----------- | ----------- | ------------------------------------------------------------ |
| StringField(FieldName,   FieldValue,Store.YES) | 字符串                 | N           | Y           | Y或N        | 字符串类型Field，不分词，作为一个整体进行索引（比如：身份证号，订单编号），是否需要存储根据Store.YES或Store.NO决定 |
| LongField(FieldName,   FieldValue,Store.YES)   | 数值型代表             | Y           | Y           | Y或N        | Long数值型Field代表，分词并且索引（比如：价格），是否需要存储根据Store.YES或Store.NO决定 |
| StoredField(FieldName,   FieldValue)           | 重载方法，支持多种类型 | N           | N           | Y           | 构建不同类型的Field，不分词，不索引，存储。   （比如：商品图片路径） |
| TextField(FieldName,   FieldValue,Store.NO)    | 文本类型               | Y           | Y           | Y或N        | 文本类型Field，分词并且索引，是否需要存储根据Store.YES或Store.NO决定 |

##### 小结

> Field的特性有哪些？

- 是否分词（tokenized）
- 是否索引（indexed）
- 是否存储（stored）

> Field的种类有哪些？

- StringField 字符串类型(不分词)

- IntField int类型(分词)

- FloatField float类型(分词)

- LongField long类型(分词)

- DoubleField double类型(分词)

- StoredField 存储类型(不分词、不索引)

- TextField 文本类型(分词)


## 15、Field：常用的Field使用

##### 目标

> 掌握常用的Field使用

##### 需求分析

+ 图书Id

  ```txt
  是否分词：不需要分词
  是否索引：需要索引
  是否存储：需要存储
  -- StringField
  ```

+ 图书名称

  ```txt
  是否分词：需要分词
  是否索引：需要索引
  是否存储：需要存储
  -- TextField
  ```

+ 图书价格

  ```txt
  是否分词：（数值型的Field lucene使用内部的分词）
  是否索引：需要索引
  是否存储：需要存储
  -- DoubleField
  ```

+ 图书图片

  ```txt
  是否分词：不需要分词
  是否索引：不需要索引
  是否存储：需要存储
  -- StoredField
  ```

+ 图书描述

  ```txt
  是否分词：需要分词
  是否索引：需要索引
  是否存储：不需要存储
  -- TextField
  ```

##### 核心代码

```java
/** 创建索引库 */
@Test
public void createIndex() throws Exception{
    // 采集数据
    BookDao bookDao = new BookDaoImpl();
    List<Book> bookList = bookDao.findAll();
    // 创建文档集合对象
    List<Document> documents = new ArrayList<Document>();
    for (Book book : bookList){
        // 创建文档对象
        Document doc = new Document();
        /**
         * 图书id
         * 是否分词：不需要分词
           是否索引：需要索引
           是否存储：需要存储
           -- StringField
         */
        doc.add(new StringField("id", book.getId() + "", Field.Store.YES));
        /**
         * 图书名称
           是否分词：需要分词
           是否索引：需要索引
           是否存储：需要存储
           -- TextField
         */
        doc.add(new TextField("bookName",book.getBookName(), Field.Store.YES));
        /**
         * 图书价格
           是否分词：（数值型的Field lucene使用内部的分词）
           是否索引：需要索引
           是否存储：需要存储
           -- DoubleField
         */
        doc.add(new DoubleField("bookPrice",book.getPrice(), Field.Store.YES));
        /**
         * 图书图片
           是否分词：不需要分词
           是否索引：不需要索引
           是否存储：需要存储
           -- StoredField
         */
        doc.add(new StoredField("bookPic",book.getPic()));
        /**
         * 图书描述
           是否分词：需要分词
           是否索引：需要索引
           是否存储：不需要存储
           -- TextField
         */
        doc.add(new TextField("bookDesc",book.getBookDesc(), Field.Store.NO));
        documents.add(doc);
    }
    // 创建分词器(Analyzer)，用于分词
    //Analyzer analyzer = new StandardAnalyzer();
    Analyzer analyzer = new IKAnalyzer();
    // 创建索引库配置对象，用于配置索引库
    IndexWriterConfig indexWriterConfig =
            new IndexWriterConfig(Version.LUCENE_4_10_3, analyzer);
    // 设置索引库打开模式(每次都重新创建)
    indexWriterConfig.setOpenMode(IndexWriterConfig.OpenMode.CREATE);
    // 创建索引库目录对象，用于指定索引库存储位置
    Directory directory = FSDirectory.open(new File("F:\\index"));
    // 创建索引库操作对象，用于把文档写入索引库
    IndexWriter indexWriter = new IndexWriter(directory, indexWriterConfig);
    // 循环文档，写入索引库
    for (Document document : documents){
        /** addDocument方法：把文档对象写入索引库 */
        indexWriter.addDocument(document);
	    // 提交事务
	    indexWriter.commit();
	}
    // 释放资源
    indexWriter.close();
}
```

+ 图一:

  ![1573403049141](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573403049141.png)

+ 图二:

  ![1573403065101](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573403065101.png)&nbsp;

## 16、索引维护：添加文档

##### 目标

> 掌握索引文档的添加

##### 核心代码

```java
/** 添加文档 */
@Test
public void save() throws Exception{

    // 创建索引库存储目录
    Directory directory = FSDirectory.open(new File("F:\\lucene_index"));
    // 创建分词器
    Analyzer analyzer = new IKAnalyzer();
    // 创建索引写配置信息对象
    IndexWriterConfig iwc = new IndexWriterConfig(Version.LATEST, analyzer);
    // 设置索引库打开模式
    iwc.setOpenMode(IndexWriterConfig.OpenMode.CREATE_OR_APPEND);

    // 创建写索引对象
    IndexWriter indexWriter = new IndexWriter(directory, iwc);

    // 创建文档对象
    Document doc = new Document();
    // 图书Id
    doc.add(new StringField("id", "6", Field.Store.YES));
    // 图书名称
    doc.add(new TextField("bookName", "mysql数据库", Field.Store.YES));
    // 图书价格
    doc.add(new DoubleField("bookPrice", 80, Field.Store.YES));
    // 图书图片
    doc.add(new StoredField("bookPic", "6.jpg"));
    // 图书描述
    doc.add(new TextField("bookDesc", "删库到跑路", Field.Store.NO));
    // 添加文档
    indexWriter.addDocument(doc);
    // 提交到索引库
    indexWriter.commit();
    // 释放资源
    indexWriter.close();
}
```

##### 小结

> 添加文档核心方法: indexWriter.addDocument();

## 17、索引维护：删除文档

##### 目标

> 掌握 索引文档 的删除

##### 核心代码

```java
/** 删除文档 */
@Test
public void delete() throws Exception{
    // 创建索引库存储目录
    Directory directory = FSDirectory.open(new File("F:\\lucene_index"));
    // 创建分词器
    Analyzer analyzer = new IKAnalyzer();
    // 创建索引写配置信息对象
    IndexWriterConfig iwc = new IndexWriterConfig(Version.LATEST, analyzer);
    // 设置索引库打开模式
    iwc.setOpenMode(IndexWriterConfig.OpenMode.CREATE_OR_APPEND);
    // 创建写索引对象
    IndexWriter indexWriter = new IndexWriter(directory, iwc);

    // 1. 根据主键id删除
    //indexWriter.deleteDocuments(new Term("id", "6"));
    // 2. 删除全部
    indexWriter.deleteAll();

    // 提交到索引库
    indexWriter.commit();
    // 释放资源
    indexWriter.close();
}
```

##### 小结

> 1. 删除索引文档的方法: indexWriter.deleteDocuments(Term ...);
> 2. 删除全部索引文档的方法: indexWriter.deleteAll();

## 18、索引维护：修改文档

##### 目标

> 掌握 索引文档 的修改

##### 核心代码

```java
/** 修改文档 */
@Test
public void update() throws Exception{
    // 创建索引库存储目录
    Directory directory = FSDirectory.open(new File("F:\\lucene_index"));
    // 创建分词器
    Analyzer analyzer = new IKAnalyzer();
    // 创建索引写配置信息对象
    IndexWriterConfig iwc = new IndexWriterConfig(Version.LATEST, analyzer);
    // 设置索引库打开模式
    iwc.setOpenMode(IndexWriterConfig.OpenMode.CREATE_OR_APPEND);
    // 创建写索引对象
    IndexWriter indexWriter = new IndexWriter(directory, iwc);

    // 创建文档对象
    Document doc = new Document();
    // 图书Id
    doc.add(new StringField("id", "6", Field.Store.YES));
    // 图书名称
    doc.add(new TextField("bookName", "mysql数据库", Field.Store.YES));
    // 图书价格
    doc.add(new DoubleField("bookPrice", 80, Field.Store.YES));
    // 图书图片
    doc.add(new StoredField("bookPic", "6.jpg"));
    // 图书描述
    doc.add(new TextField("bookDesc", "删库到跑路", Field.Store.NO));
    // 修改文档
    indexWriter.updateDocument(new Term("id", "6"),doc);

    // 提交到索引库
    indexWriter.commit();
    // 释放资源
    indexWriter.close();
}
```

##### 小结

> 更新索引文档的方法: indexWriter.updateDocument();
>
> 注意: 如果修改条件存在，则修改，否则添加。

## 19、高级搜索：词条搜索

##### 目标

> 掌握Lucene词条搜索 TermQuery

需求：查询图书名称域中包含有java的图书。

##### 核心代码

```java
package cn.itcast.lucene;

import org.apache.lucene.analysis.Analyzer;
import org.apache.lucene.document.Document;
import org.apache.lucene.index.DirectoryReader;
import org.apache.lucene.index.IndexReader;
import org.apache.lucene.index.Term;
import org.apache.lucene.search.*;
import org.apache.lucene.store.Directory;
import org.apache.lucene.store.FSDirectory;
import org.junit.Test;
import org.wltea.analyzer.lucene.IKAnalyzer;
import java.io.File;

/** 高级搜索测试 */
public class QueryTest {
    
    /**
     * 需求：查询图书名称域中包含有java的图书
     * TermQuery: 词条查询
     */
    @Test
    public void test1() throws Exception{
        // 创建词条查询
        TermQuery termQuery  = new TermQuery(new Term("bookName", "java"));
        search(termQuery);
    }

    /** 通用搜索方法 */
    private void search(Query query) throws Exception{
        System.out.println("查询语法：" + query);
        // 创建索引库存储目录
        Directory directory = FSDirectory.open(new File("F:\\lucene_index"));
        // 创建索引读取对象(把索引库中索引数据加载到内存中)
        IndexReader indexReader = DirectoryReader.open(directory);
        // 创建索引搜索对象
        IndexSearcher indexSearcher = new IndexSearcher(indexReader);
        // 创建分词器(对查询条件做分词)
        Analyzer analyzer = new IKAnalyzer();
        // 搜索，得到最前面的文档对象
        TopDocs topDocs = indexSearcher.search(query, 10);
        System.out.println("命中的数量：" + topDocs.totalHits);
        System.out.println("最大分数：" + topDocs.getMaxScore());
        
        // 获取分数文档数组
        ScoreDoc[] scoreDocs = topDocs.scoreDocs;
        // 迭代分数文档数组
        // scoreDoc: 文档doc 分数:score
        for (ScoreDoc scoreDoc : scoreDocs) {
            System.out.println("=====华丽分割线=====");
            System.out.println("文档id: " + scoreDoc.doc + "\t文档分数：" 
                               + scoreDoc.score);
            // 根据文档获取文档
            Document doc = indexSearcher.doc(scoreDoc.doc);
            System.out.println("图书id:" + doc.get("id"));
            System.out.println("图书名称:" + doc.get("bookName"));
            System.out.println("图书价格:" + doc.get("bookPrice"));
            System.out.println("图书图片:" + doc.get("bookPic"));
            System.out.println("图书描述:" + doc.get("bookDesc"));
        }
        // 释放资源
        indexReader.close();
    }
}
```

![1573437760608](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573437760608.png)&nbsp;

##### 小结

> Lucene 词条搜索的Query是: TermQuery
>
> 注意：TermQuery中的查询条件不会分词。

## 20、高级搜索：范围查找

##### 目标

> 掌握 Lucene 范围查询

需求：搜索图书价格大于80，小于100的图书

##### 核心代码

```java
/**
  *	需求：查询图书价格在80到100之间的图书。不包含边界值80和100
  * NumericRangeQuery: 数字范围查询
  */
@Test
public void test2() throws Exception{
    // 数字范围查询
    // String field: 字段名称
    // Double min: 最小值
    // Double max: 最大值
    // boolean minInclusive: 是否包含最小值
    // boolean maxInclusive: 是否包含最大值
    Query query = NumericRangeQuery.newDoubleRange("bookPrice", 80d, 100d, false, false);
    search(query);
}
```

![1573438338468](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573438338468.png)&nbsp;

##### 小结

```java
// 数字范围查询
// String field: 字段名称
// Double min: 最小值
// Double max: 最大值
// boolean minInclusive: 是否包含最小值
// boolean maxInclusive: 是否包含最大值
NumericRangeQuery.newDoubleRange("bookPrice", 80d, 100d, true, true);
```

## 21、高级搜索：组合查询

##### 目标

> 掌握Lucene组合条件查询

需求: 查询图书名称中包含有lucene，并且图书价格在80到100之间的图书

组合方式: 

![1573441055542](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573441055542.png)

说明: 组合条件查询也叫布尔查询,布尔查询本身没有查询条件，可以把其它查询通过逻辑运算进行组合。

##### 核心代码

```java
/**
 * 需求：查询图书名称中包含有lucene，并且图书价格在80到100之间的图书
 * BooleanQuery: 布尔查询(多条件组合查询)
 */
@Test
public void test3 () throws Exception{
    // 查询图书名称中包含有lucene
    Query q1 = new TermQuery(new Term("bookName", "lucene"));
    // 图书价格在80到100之间的图书
    Query q2 = NumericRangeQuery.newDoubleRange("bookPrice", 80d, 100d, true,true);

    // 创建布尔查询
    BooleanQuery booleanQuery = new BooleanQuery();
    // 添加组合条件 与 组合方式
    // 组合方式:
    // 1. BooleanClause.Occur.MUST : 必须
    // 2. BooleanClause.Occur.MUST_NOT: 必须不
    // 3. BooleanClause.Occur.SHOULD: 有可能
    booleanQuery.add(q1, BooleanClause.Occur.MUST);
    // 添加组合条件 与 组合方式
    booleanQuery.add(q2, BooleanClause.Occur.MUST);
    
    search(booleanQuery);
}
```

![1573441739296](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573441739296.png) 

##### 小结

> 组合条件的 Query 是: BooleanQuery


## 22、高级搜索：条件表达式

##### 思考

> 组合条件查询时，如果条件比较多，存在特殊组合而无法发现。能否像 sql 一样编写查询条件呢？

##### 目标

> 掌握luncene条件表达式基本语法

##### 表达式语法

+ 词条查询：fieldName:关键词，比如: bookName:lucene

+ 范围查询：fieldName:{最小值 TO 最大值]，比如: price:{80 TO 100]

+ 组合查询:

  ![1573442069558](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573442069558.png) 

需求: 查询图书名称中，包含有java，并且包含有lucene的图书

##### 核心代码

```java
/**
 * 需求：查询图书名称域中，包含有java，并且包含有lucene的图书。
 * QueryParser: 查询解析器，把查询表达式解析成Query对象。
 */
@Test
public void test4() throws Exception {
    // 创建分词器
    Analyzer analyzer = new IKAnalyzer();
    // 创建queryParser对象
    QueryParser queryParser = new QueryParser("bookName", analyzer);

    // 设置查询条件
    // 含义：搜索bookName中包含java的，或者包含lucene的文档
    //Query query = queryParser.parse("bookName:java OR bookName:lucene");
    // 含义：搜索bookName中包含java的，但是不包含lucene的文档
    //Query query = queryParser.parse("bookName:java NOT bookName:lucene");
    // 含义：搜索bookName中包含java的，并且包含lucene的文档
    Query query = queryParser.parse("bookName:java AND bookName:lucene");
    // 执行搜索
    search(query);
}
```

![1573442890035](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Lucene/1573442890035.png) 

##### 小结

> 组合条件表达式，用什么来进行解析? QueryParser.parse()方法

## 23、高级搜索：分页和排序

##### 目标

> 掌握 Lucene API 的排序和分页

##### 核心代码

```java
package cn.itcast.lucene;

import org.apache.lucene.analysis.Analyzer;
import org.apache.lucene.document.Document;
import org.apache.lucene.index.DirectoryReader;
import org.apache.lucene.index.IndexReader;
import org.apache.lucene.queryparser.classic.QueryParser;
import org.apache.lucene.search.*;
import org.apache.lucene.store.Directory;
import org.apache.lucene.store.FSDirectory;
import org.junit.Test;
import org.wltea.analyzer.lucene.IKAnalyzer;

import java.io.File;

/** 搜索分页与排序 */
public class QueryPager {

    /** 搜索分页与排序 */
    @Test
    public void queryForPage() throws Exception{

        // 创建索引库存储目录
        Directory directory = FSDirectory.open(new File("F:\\lucene_index"));
        // 创建索引读取对象(把索引库中索引数据加载到内存中)
        IndexReader indexReader = DirectoryReader.open(directory);
        // 创建索引搜索对象
        IndexSearcher indexSearcher = new IndexSearcher(indexReader);
        // 创建分词器(对查询条件做分词)
        Analyzer analyzer = new IKAnalyzer();

        // 创建查询对象
        QueryParser queryParser = new QueryParser("bookName", analyzer);
        Query query = queryParser.parse("bookName:java OR bookName:lucene");

        // 当前页码
        int pageNo = 1;
        // 每页显示的记录数
        int pageSize  = 5;

        // 创建排序字段对象: false: 升序 true : 降序
        SortField sortField = new SortField("bookPrice", SortField.Type.DOUBLE, false);
        // 创建排序对象
        Sort sort = new Sort(sortField);

        // 搜索，得到最前面的文档对象
        TopDocs topDocs = indexSearcher.search(query, pageNo * pageSize, sort);
        System.out.println("命中的数量：" + topDocs.totalHits);

        // 获取分数文档数组
        ScoreDoc[] scoreDocs = topDocs.scoreDocs;
        // 迭代分数文档数组
        // scoreDoc: 文档doc 分数:score
        for (int i = (pageNo - 1) * pageSize; i < scoreDocs.length; i++) {
            System.out.println("=====华丽分割线=====");
            ScoreDoc scoreDoc = scoreDocs[i];
            System.out.println("文档id: " + scoreDoc.doc
                            + "\t文档分数：" + scoreDoc.score);
            // 根据文档获取文档
            Document doc = indexSearcher.doc(scoreDoc.doc);
            System.out.println("图书id:" + doc.get("id"));
            System.out.println("图书名称:" + doc.get("bookName"));
            System.out.println("图书价格:" + doc.get("bookPrice"));
            System.out.println("图书图片:" + doc.get("bookPic"));
            System.out.println("图书描述:" + doc.get("bookDesc"));
        }
        // 释放资源
        indexReader.close();
    }
}
```

搜索排序关键代码:

```java
 // 创建排序字段对象: false: 升序 true : 降序
SortField sortField = new SortField("bookPrice", SortField.Type.DOUBLE, false);
// 创建排序对象
Sort sort = new Sort(sortField);
// 搜索，得到最前面的文档对象
TopDocs topDocs = indexSearcher.search(query, pageNo * pageSize, sort);
```

## 24、总结

+ 索引流程
+ 检索流程
+ 分词器
+ Field(是否分词、是否索引、是否存储)
+ 索引库维护
+ 高级搜索(TermQuery、NumericRangeQuery、BooleanQuery)
+ 分页与排序