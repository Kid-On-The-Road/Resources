### 01、课程目标

1：了解什么是微服务架构和系统架构的演进

2：了解HTTP和RPC的区别

3：掌握HttpClient的基本使用

4：能够使用RestTemplate发送请求 

5：能够说出SpringCloud的作用 

6：能够搭建Eureka注册中心 

7：能够使用Robbin负载均衡 

8：能够使用Hystrix熔断器



























### 02、服务架构的演变

**目标：**

了解项目架构的演变历程





演变过程：

```
1、集中式架构
2、垂直拆分
3、分布式服务
4、服务治理（SOA）
5、微服务
```





==**1、集中式架构：**==

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本

![1572876520974](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572876520974.png) 

```
优点：
	- 系统开发速度快 
	- 维护成本低 
	- 适用于并发要求较低的系统

缺点：
	- 代码耦合度高，
	- 后期维护困难 
	- 无法针对不同模块进行针对性优化 
	- 无法水平扩展 
	- 单点容错率低，
	- 并发能力差
```



==**2、垂直拆分：**==

当访问量逐渐增大，单一应用无法满足需求，此时为了应对更高的并发和业务需求，我们根据业务功能对系统进行拆 分：

![1572876617378](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572876617378.png) 

```
优点：
	- 系统拆分实现了流量分担，解决了并发问题
	- 可以针对不同模块进行优化
	- 方便水平扩展，负载均衡，容错率提高
缺点：
	- 系统间相互独立，会有很多重复开发工作，影响开发效率
```



==**3、分布式服务：**==

当垂直应用越来越多，应用之间交互不可避免，将==核心业务抽取出来==，作为独立的服务，逐渐形成稳定的服务中心， 使前端应用能更快速的响应多变的市场需求。

![1572877220559](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877220559.png) 

```
优点：
	- 将基础服务进行了抽取，系统间相互调用，提高了代码复用和开发效率
缺点：
	- 系统间耦合度变高，调用关系错综复杂，难以维护
	
出现了什么问题？
	- 服务越来越多，需要管理每个服务的地址
	- 调用关系错综复杂，难以理清依赖关系
	- 服务过多，服务状态难以管理，无法根据服务情况动态管理	
```



==**4、服务治理（SOA）：**==

SOA（Service Oriented Architecture）面向服务的架构：它是一种设计方法，其中包含多个服务， 服务之间通过相 互依赖终提供一系列的功能。一个服务 通常以独立的形式存在与操作系统进程中。各个服务之间 通过网络调用。 SOA结构图：

![1572877279458](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877279458.png)

ESB（企业服务总线），简单 来说 ESB 就是一根管道，用来连接各个服务节点。为了集 成不同系统，不同协议的服务，ESB 做了消息的转化解释和路由工作，让不同的服务互联互通。

**SOA缺点：**每个供应商提供的ESB产品有偏差，自身实现较为复杂；应用服务粒度较大，ESB集成整合所有服务和协 议、数据转换使得运维、测试部署困难。所有服务都通过一个通路通信，直接降低了通信速度。

 

![1572877485365](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877485365.png) 

```
服务治理要做什么？
	- 服务注册中心，实现服务自动注册和发现，无需人为记录服务地址
	- 服务自动订阅，服务列表自动推送，服务调用透明化，无需关心依赖关系
	- 动态监控服务状态监控报告，人为控制服务状态
缺点：
	- 服务间会有依赖关系，一旦某个环节出错会影响较大
	- 服务关系复杂，运维、测试部署困难，不符合DevOps思想
```



==**5、微服务：**==

![1572877429523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877429523.png) 

```
微服务的特点：
	- 单一职责：微服务中每一个服务都对应唯一的业务能力，做到单一职责
	- 微：微服务的服务拆分粒度很小，例如一个用户管理就可以作为一个服务。每个服务虽小，但“五脏俱全”。
	- 面向服务：面向服务是说每个服务都要对外暴露服务接口API。并不关心服务的技术实现，
	  做到与平台和语言无关，也不限定用什么技术实现，只要提供Rest的接口即可。
	- 自治：自治是说服务间互相独立，互不干扰
  	- 团队独立：每个服务都是一个独立的开发团队，人数不能过多。
  	- 技术独立：因为是面向服务，提供Rest接口，使用什么技术没有别人干涉
  	- 前后端分离：采用前后端分离开发，提供统一Rest接口，后端不用再为PC、移动段开发不同接口
  	- 数据库分离：每个服务都使用自己的数据源
  	- 部署独立，服务间虽然有调用，但要做到服务重启不影响其它服务。有利于持续集成和持续交付。
  	  每个服务都是独立的组件，可复用，可替换，降低耦合，易维护
```





**小结：**

![1572877554264](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877554264.png) 















### 03、远程调用的方式

**目标**：能够说出服务调用方式种类





无论是微服务还是SOA，都面临着服务间的远程调用。那么服务间的远程调用方式有哪些呢？

常见的远程调用方式有以下2种：

1：**RPC**：Remote Produce Call远程过程调用，**RPC基于Socket，工作在会话层。自定义数据格式**，速度快，效率高。早期的webservice，现在热门的dubbo，都是RPC的典型代表

![1572877711327](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877711327.png) 



2：**Http**：http其实是**一种网络传输协议，基于TCP，工作在应用层，规定了数据传输的格式**。现在客户端浏览器与服务端通信基本都是采用Http协议，也可以用来进行远程服务调用。缺点是消息封装臃肿，优势是对服务的提供和调用方没有任何技术限定，自由灵活，更符合微服务理念。

![1572877756745](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877756745.png) 



现在热门的Rest风格，就可以通过http协议来实现。



**区别**：RPC的机制是根据语言的API（language API）来定义的，而不是根据基于网络的应用来定义的。

如果你们公司全部采用Java技术栈，那么使用Dubbo作为微服务架构是一个不错的选择。

相反，如果公司的技术栈多样化，而且你更青睐Spring家族，那么Spring Cloud搭建微服务是不二之选。在我们的项目中，会选择Spring Cloud套件，因此会使用Http方式来实现服务间调用。



















### 04、使用RestTemplate进行远程调用



进行远程调用：

​		1、HttpClient

​		2、RestTemplate



Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化，非常方便。RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持：

- HttpClient

- OkHttp

- JDK原生的URLConnection（默认的）



导入依赖：

```xml
	<dependencies>
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
    </dependencies>
```







测试代码1：

```java
package com.itheima.test;

import com.itheima.pojo.Book;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.BasicResponseHandler;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.junit.Test;

import com.fasterxml.jackson.databind.ObjectMapper;

public class Test01_http {
	
	private CloseableHttpClient httpClient = HttpClients.createDefault();
	
	/**
	 * get请求
	 * @throws Exception
	 */
	@Test
	public void test1() throws Exception {
		HttpGet request = new HttpGet("http://localhost:8080/book/3");
		String response = this.httpClient.execute(request, new BasicResponseHandler());
		System.out.println(response);
		
	}
	
	
	/**
	 * post请求
	 * @throws Exception
	 */
	@Test
	public void test2() throws Exception {
		HttpPost post = new HttpPost("http://localhost:8080/savebook");
		//httpPost.setHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36");
		/*
		 * post带参数开始
		 */
		//请求内容
		String content = "{\"bookname\":\"SpringCloud入门到精通\",\"bookdesc\":\"这是一本好书啊好书啊！\",\"price\":111999}";
		//使用addHeader方法添加请求头部,诸如User-Agent, Accept-Encoding等参数.
		post.setHeader("Content-Type", "application/json;charset=UTF-8");
		//组织数据
		StringEntity se = new StringEntity(content,"UTF-8");
		//设置编码格式
		se.setContentEncoding("UTF-8");
		//设置数据类型
		se.setContentType("application/json");
		//对于POST请求,把请求体填充进HttpPost实体.
		post.setEntity(se);
		
		//执行请求
		String response = this.httpClient.execute(post, new BasicResponseHandler());
		System.out.println(response);
	}
	
	/**
	 * 返回字符串转成对象
	 * @throws Exception
	 */
	@Test
	public void test3() throws Exception {
		HttpGet request = new HttpGet("http://localhost:8080/book/3");
		String response = this.httpClient.execute(request, new BasicResponseHandler());
		System.out.println(response);
		
		System.out.println("字符串转成对象。。。。。");
		ObjectMapper om = new ObjectMapper();
		Book book = om.readValue(response, Book.class);
		System.out.println(book);
		System.out.println(book.getBookname());
		
	}

}
```



测试代码2：

```java
package com.itheima.test;

import com.itheima.pojo.Book;
import org.junit.Test;
import org.springframework.web.client.RestTemplate;

public class Test02_RestTemplate {

	/**
	 * Spring中的RestTemplate完成调用
	 * @throws Exception
	 */
	@Test
	public void test() throws Exception {
		RestTemplate rt = new RestTemplate();
		Book book = rt.getForObject("http://localhost:8080/book/3", Book.class);
		System.out.println(book);
	}

}
```





getForObject(Url，把返回值转成什么类型)

















### 05、SpringCloud介绍

**目标**：Spring Cloud整合的组件和版本特征



微服务是一种架构方式，终肯定需要技术架构去实施。---SpringBoot

微服务的实现方式很多，但是火的莫过于Spring Cloud了。为什么？

> - 后台硬：作为Spring家族的一员，有整个Spring全家桶靠山，背景十分强大。 
> - 技术强：Spring作为Java领域的前辈，可以说是功力深厚。有强力的技术团队支撑，一般人还真比不了 
> - 群众基础好：可以说大多数程序员的成长都伴随着Spring框架，试问：现在有几家公司开发不用Spring？ Spring Cloud与Spring的各个框架无缝整合，对大家来说一切都是熟悉的配方，熟悉的味道。 
> - 使用方便：相信大家都体会到了SpringBoot给我们开发带来的便利，而Spring Cloud完全支持Spring Boot的开 发，用很少的配置就能完成微服务框架的搭建

SpringCloud是Spring旗下的项目之一，[官网地址：http://projects.spring.io/spring-cloud/](http://projects.spring.io/spring-cloud/)

![1569396164776](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1569396164776.png) 

Spring最擅长的就是集成，把世界上最好的框架拿过来，集成到自己的项目中。

SpringCloud也是一样，它将现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。协调分布式环境中各个系统，为各类服务提供模板性配置。其主要 涉及的组件包括：

- Eureka：注册中心 
- Zuul/Gateway：服务网关 
- Ribbon：负载均衡
- Feign：服务调用
- Hystix：熔断器
- 统一配置中心
- 服务总线

以上只是其中一部分，架构图：

![1562414270990](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1562414270990.png)  





**SpringCloud的版本**：

Spring Cloud不是一个组件，而是许多组件的集合；它的版本命名比较特殊，是以A到Z的为首字母的一些单词（其实是伦敦地铁站的名字）组成：

==1、传统的版本号规则是什么？==

springfremaework-4.3.11.release

`4.3.1.release` 分别是：`主版本号.次版本号.增强版本号.里程碑版本号。`

- 主版本号：项目的重大重构
- 次版本号：新功能的添加和变换。
- 增强版本号：BUG的修复
- 里程碑版本号：release

==2、Springcloud的版本号规则==

<https://spring.io/projects/spring-cloud#learn>

- 版本特征：以英文单词命名（伦敦地铁站名）

这是 版本号都是英国伦敦的地铁站的站名。依次从A-Z。

![1572881064543](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881064543.png)

3、为什么springcloud用的是单词而不是数字呢？

设计的目的是为了管理好每个springcloud的子项目清单，避免自己的版本和子项目版本的版本号混淆。

4、什么是版本的发布计划。

也就是上面的SR5,SR2之类的。 

![1572881129963](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881129963.png) 



==注意：我们在项目中，使用新稳定的Greenwich版本。==



其中包含的组件，也都有各自的版本，如下表：

| Component                 | Edgware.SR4    | Finchley.SR1  | Finchley.BUILD-SNAPSHOT |
| ------------------------- | -------------- | ------------- | ----------------------- |
| spring-cloud-aws          | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-bus          | 1.3.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-cli          | 1.4.1.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-commons      | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-contract     | 1.2.5.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-config       | 1.4.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-netflix      | 1.4.5.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-security     | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-cloudfoundry | 1.1.2.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-consul       | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-sleuth       | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-stream       | Ditmars.SR4    | Elmhurst.SR1  | Elmhurst.BUILD-SNAPSHOT |
| spring-cloud-zookeeper    | 1.2.2.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-boot               | 1.5.14.RELEASE | 2.0.4.RELEASE | 2.0.4.BUILD-SNAPSHOT    |
| spring-cloud-task         | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-vault        | 1.1.1.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-gateway      | 1.0.2.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-openfeign    |                | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-function     | 1.0.0.RELEASE  | 1.0.0.RELEASE | 1.0.1.BUILD-SNAPSHOT    |

接下来，我们就一一学习SpringCloud中的重要组件。







### 06、SpringBoot和SpringCloud与其他框架的对比

**目标**

掌握和了解springboot和springcloud的区别和差异。

**概述**

==springboot是spring的一套快速配置的脚手架，==可以基于Springboot快速开发单个应用，SpringCloud是一个基于SpringBoot实现的云应用开发工具，SpringBoot专注快速，方便集成的单个微服务个体，==SpringCloud 关注全局的服务治理框架，==SpringBoot使用了默认大于配置的理念，很多集成方案已经帮你选好了，能不配置就不配置，SpringCloud很大的一部分是基于SpringBoot来实现，可以不基于SpringBoot吗？答案是：不可以。



SpringBoot可以离开springcloud独立开发项目，但是SpringCloud不能离开springboot，属于依赖关系。

**SpringBoot和SpringCloud对照表：**

| spring-boot-starter-parent | spring-cloud-dependencies |                         |
| -------------------------- | ------------------------- | ----------------------- |
| 版本号                     | 发布日期                  | 版本号                  |
| 1.5.2.RELEASE              | 2017年3月                 | Dalston.RC1             |
| 1.5.9.RELEASE              | 2017年11月                | Edgware.RELEASE         |
| 1.5.16.RELEASE             |                           | Edgware.SR5             |
| 1.5.20.RELEASE             |                           | Edgware.SR5             |
| 2.0.2.RELEASE              | 2018年5月                 | Finchley.BUILD-SNAPSHOT |
| 2.0.6.RELEASE              |                           | Finchley.SR2            |
| 2.1.4.RELEASE              |                           | Greenwich.SR1           |
| 2.1.5.RELEASE              |                           | Greenwich.SR2           |
| 2.1.6.RELEASE              |                           | Greenwich.SR3           |
| 2.2.x                      |                           | Hoxton                  |
| 未完待续......             |                           |                         |



**springcloud和dubbo 的比较**

许很多人会说Spring Cloud和Dubbo的对比有点不公平，Dubbo只是实现了服务治理，而Spring Cloud下面有17个子项目（可能还会新增）分别覆盖了微服务架构下的方方面面，服务治理只是其中的一个方面，一定程度来说，Dubbo只是Spring Cloud Netflix中的一个子集。但是在选择框架上，方案完整度恰恰是一个需要重点关注的内容。

根据Martin Fowler对 [微服务架构](http://martinfowler.com/articles/microservices.html) 的描述中，虽然该架构相较于单体架构有模块化解耦、可独立部署、技术多样性等诸多优点，但是由于分布式环境下解耦，也带出了不少[测试](http://lib.csdn.net/base/softwaretest)与运维复杂度。

根据微服务架构在各方面的要素，看看Spring Cloud和Dubbo都提供了哪些支持。

|              | Dubbo     | Spring Cloud                                                 |
| :----------- | :-------- | :----------------------------------------------------------- |
| 服务注册中心 | Zookeeper | Spring Cloud Netflix Eureka                                  |
| 服务调用方式 | RPC       | REST API http                                                |
| 服务网关     | 无        | Spring Cloud Netflix Zuul            \|  Spring Cloud Gateway |
| 断路器       | 不完善    | Spring Cloud Netflix Hystrix                                 |
| 分布式配置   | 无        | Spring Cloud Config                                          |
| 服务跟踪     | 无        | Spring Cloud Sleuth                                          |
| 消息总线     | 无        | Spring Cloud Bus                                             |
| 数据流       | 无        | Spring Cloud Stream                                          |
| 批量任务     | 无        | Spring Cloud Task                                            |

虽然，Dubbo自身只是实现了服务治理的基础，其他为保证集群安全、可维护、可测试等特性方面都没有很好的实现，但是几乎大部分关键组件都能找到第三方开源来实现，这些组件主要来自于国内各家大型互联网企业的开源产品。

![1572881423647](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881423647.png) 

具体对比分析

<https://mp.weixin.qq.com/s/omVAEzQTcV5o5AGsSU_u7Q>

![1572881462658](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881462658.png) 

























### 07、前期准备工作：搭建微服务场景

**目标**：

创建微服务父工程springcloud-demo、用户服务工程user-service、服务消费工程consumer



**分析**：

需求：查询数据库中的用户数据并输出到浏览器

![1572881710094](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881710094.png) 



#### 父工程

- 父工程springcloud-demo：添加spring boot父坐标和管理其它组件的依赖

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
  
      <groupId>net.cfei</groupId>
      <artifactId>springcloud-demo</artifactId>
      <packaging>pom</packaging>
      <version>1.0-SNAPSHOT</version>
  
      <modules>
          <module>user-service</module>
          <module>consumer</module>
      </modules>
  
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.1.4.RELEASE</version>
          <relativePath/>
      </parent>
  
      <properties>
          <java.version>1.8</java.version>
          <spring-cloud.version>Greenwich.SR1</spring-cloud.version>
          <mapper.starter.version>2.1.5</mapper.starter.version>
          <mysql.version>5.1.46</mysql.version>
      </properties>
  
      <dependencyManagement>
          <dependencies>
              <!-- springCloud -->
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>${spring-cloud.version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
  
              <!-- 通用Mapper启动器 -->
              <dependency>
                  <groupId>org.mybatis.spring.boot</groupId>
                  <artifactId>mybatis-spring-boot-starter</artifactId>
                  <version>2.0.1</version>
              </dependency>
              <dependency>
                  <groupId>com.github.pagehelper</groupId>
                  <artifactId>pagehelper-spring-boot-starter</artifactId>
                  <version>1.2.10</version>
              </dependency>
              <dependency>
                  <groupId>tk.mybatis</groupId>
                  <artifactId>mapper-spring-boot-starter</artifactId>
                  <version>2.1.5</version>
              </dependency>
              <!-- mysql驱动 -->
              <dependency>
                  <groupId>mysql</groupId>
                  <artifactId>mysql-connector-java</artifactId>
                  <version>${mysql.version}</version>
              </dependency>
  
          </dependencies>
      </dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
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



#### 服务提供者

- 用户服务工程user-service：整合mybatis查询数据库中用户数据；提供查询用户服务

1：添加启动器依赖（web、通用Mapper）；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud-demo</artifactId>
        <groupId>net.cfei</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>user-service</artifactId>

    <dependencies>
        <!--web的依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- mybatis和mysql的驱动 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--mybatis的分页插件-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
        </dependency>
        <!-- 通用Mapper启动器 -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

</project>
```

2：创建启动引导类和配置文件；

```yaml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springcloud?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
    username: root
    password: passw0rd
    driver-class-name: com.mysql.jdbc.Driver
# mybatis的配置
mybatis:
  mapper-locations: classpath:/mappers/*.xml
  type-aliases-package: com.itheima.pojo
  configuration:
    map-underscore-to-camel-case: true  #驼峰命名

```

3：启动类开启通用mapper注解

```java
package com.itheima.user;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import tk.mybatis.spring.annotation.MapperScan;

@SpringBootApplication
@MapperScan("com.itheima.mapper")
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}

```

4：用户POJO类

```java
package com.itheima.user.pojo;

import lombok.Data;
import tk.mybatis.mapper.annotation.KeySql;
import javax.persistence.Id;
import javax.persistence.Table;
import java.util.Date;

@Data
@Table(name = "tb_user")
public class User{
    // id
    @Id
    //开启主键自动回填
    @KeySql(useGeneratedKeys = true)
    private Long id;
    // 用户名
    private String userName;
    // 密码
    private String password;
    // 姓名
    private String name;
    // 年龄
    private Integer age;
    // 性别，1男性，2女性
    private Integer sex;
    // 出生日期
    private Date birthday;
    // 创建时间
    private Date created;
    // 更新时间
    private Date updated;
    // 备注
    private String note;
}
```



5：编写测试代码（UserMapper，UserService，UserController）；

编写 user-service\src\main\java\com\itheima\mapper\UserMapper.java

```java
package com.itheima.mapper;

import com.itheima.pojo.User;
import tk.mybatis.mapper.common.Mapper;

public interface UserMapper extends Mapper<User> {
}
```

编写 user-service\src\main\java\com\itheima\service\UserService.java

```java
package com.itheima.service;

import com.itheima.mapper.UserMapper;
import com.itheima.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 根据主键查询用户
     * @param id 用户id
     * @return 用户
     */
    public User queryById(Long id){
        return userMapper.selectByPrimaryKey(id);
    }
}

```

添加一个对外查询的接口处理器
user-service\src\main\java\com\itheima\user\controller\UserController.java

```java
package com.itheima.controller;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    //todo:http://localhost:9091/user/8
    @GetMapping("/{id}")
    public User queryById(@PathVariable Long id){
        return userService.queryById(id);
    }
}

```

7：启动并测试 

启动 user-service 项目，访问接口：http://localhost:8081/user/8









#### 服务消费者

- 服务消费工程consumer：利用查询用户服务获取用户数据并输出到浏览器



需求：

访问http://localhost:8080/consumer/8 使用RestTemplate获取http://localhost:9091/user/8的数据

==与上面类似，这里不再赘述，需要注意的是，我们调用 user-service 的功能，因此不需要mybatis相关依赖了。==

实现步骤：

1：添加启动器依赖；

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

2：创建启动引导类（注册RestTemplate）和配置文件；

```java
package com.itheima.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class ConsumberApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate(){
         return new RestTemplate();
    }
}
```



3：编写测试代码（ConsumerController中使用restTemplate访问服务获取数据）

```java
package com.itheima.consumer.web;

import com.itheima.consumer.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    //todo http://localhost:8080/consumer/8
    @GetMapping("/{id}")
    public User getUser(@PathVariable("id")Long id){
        String url = "http://localhost:9091/user/8";
        return restTemplate.getForObject(url,User.class);
    }
}

```

4：测试

启动 consumer-demo 引导启动类；因为 consumer-demo 项目没有配置端口，那么默认就是8080

我们访问：http://localhost:8080/consumer/8



**小结**：

```xml
<!-- springCloud -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-dependencies</artifactId>
    <version>${spring-cloud.version}</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

通过 `scope` 的import可以继承 `spring-cloud-dependencies` 工程中的依赖。含义是：也就是说接下来的项目中，包括这些项目中应用的版本都是来自于`spring-cloud-dependencies`的这些版本信息。也就是在后续的应用spring cloud 组件的时候，不要写版本号。







####  思考问题



简单回顾一下，刚才我们写了什么：

- user-service：对外提供了查询用户的接口 

- consumer：通过RestTemplate访问 http://locahost:8081/user/{id} 接口，查询用户数据

  

存在什么问题？

- 在consumer中，我们把url地址硬编码到了代码中，不方便后期维护 

- consumer需要记忆user-service的地址，如果出现变更，可能得不到通知，地址将失效 

- consumer不清楚user-service的状态，服务宕机也不知道 

- user-service只有1台服务，不具备高可用性 

- 即便user-service形成集群，consumer还需自己实现负载均衡

  

其实上面说的问题，概括一下就是分布式服务必然要面临的问题

- 服务管理 

  如何自动注册和发现 

  如何实现状态监管 

- 如何实现动态路由 

  服务如何实现负载均衡 

- 服务如何解决容灾问题

  服务如何实现统一配置

以上的问题，都将在SpringCloud中得到答案。



















### 08、SpringCloud：Eureka的原理



**目标**：

说出Eureka的主要功能

<strong style="font-size:24px">**问题分析**：</strong>

在刚才的案例中，user-service对外提供服务，需要对外暴露自己的地址。而consumer-demo（调用者）需要记录服务提供者的地址。将来地址出现变更，还需要及时更新。这在服务较少的时候并不觉得有什么，但是在现在日益复杂的互联网环境，一个项目可能会拆分出十几，甚至几十个微服务。此时如果还人为管理地址，不仅开发困难，将来
测试、发布上线都会非常麻烦，这与DevOps的思想是背道而驰的。

DevOps的思想是系统可以通过一组过程、方法或系统；提高应用发布和运维的效率,降低管理成本。

> ==网约车==

这就好比是 网约车出现以前，人们出门叫车只能叫出租车。一些私家车想做出租却没有资格，被称为黑车。而很多
人想要约车，但是无奈出租车太少，不方便。私家车很多却不敢拦，而且满大街的车，谁知道哪个才是愿意载人的。
一个想要，一个愿意给，就是缺少引子，缺乏管理啊。
此时滴滴这样的网约车平台出现了，所有想载客的私家车全部到滴滴注册，记录你的车型（服务类型），身份信息
（联系方式）。这样提供服务的私家车，在滴滴那里都能找到，一目了然。
此时要叫车的人，只需要打开APP，输入你的目的地，选择车型（服务类型），滴滴自动安排一个符合需求的车到你
面前，为你服务，完美！





> ==Eureka做什么？==

Eureka就好比是滴滴，负责管理、记录服务提供者的信息。服务调用者无需自己寻找服务，而是把自己的需求告诉
Eureka，然后Eureka会把符合你需求的服务告诉你。
同时，服务提供方与Eureka之间通过 “心跳” 机制进行监控，当某个服务提供方出现问题，Eureka自然会把它从服务
列表中剔除。
这就实现了服务的自动注册、发现、状态监控。

**原理图**

Eureka的主要功能是进行服务管理，定期检查服务状态，返回服务地址列表。

![1560439174201](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1560439174201.png)&nbsp;





renewal：续约

- Eureka-Server：就是服务注册中心（可以是一个集群），对外暴露自己的地址。
- 提供者：启动后向Eureka注册自己信息（地址，服务名称等），并且定期进行服务续约
- 消费者：服务调用方，会定期去Eureka拉取服务列表，然后使用负载均衡算法选出一个服务进行调用。
- 心跳(续约)：提供者定期通过http方式向Eureka刷新自己的状态



















### 09、SpringCloud：Eureka服务端：注册中心

**目标**：

添加eureka对应依赖和编写引导类搭建eureka服务并可访问eureka服务界面

**分析**：

Eureka是服务注册中心，只做服务注册；自身并不提供服务也不消费服务。可以搭建web工程使用Eureka，可以使用Spring Boot方式搭建。

**搭建步骤：**

- 导入Eureka服务端的起步依赖
- 编写启动类：开启Eureka服务
- 编写配置文件：配置相应信息



==**核心代码：**==

- 起步依赖【父工程有spring-boot-starter-parent】

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.itheima</groupId>
		<artifactId>springcloud-demo</artifactId>
		<version>1.0-SNAPSHOT</version>
	</parent>
	<artifactId>eureka-server</artifactId>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<!-- Eureka服务端 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>
	</dependencies>

</project>
```



- 启动类

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
//开启eureka服务
@EnableEurekaServer
public class EurekaServerApplication {
	
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}

}

```



- 配置文件

```yaml
#端口
server:
  port: 10086
#服务id
spring:
  application:
    name: eureka-server
#配置eureka客户端
eureka:
  client:
    register-with-eureka: false  #不注册
    fetch-registry: false #不拉取
    service-url:
      defaultZone: http://localhost:10086/eureka/  #告诉现在的注册中心的地址是什么
```



访问地址： http://localhost:10086/













### 10、SpringCloud：Eureka客户端：服务注册

**目标**：

将user-service的服务注册到eureka并在consumer-demo中可以根据服务名称调用

**对象工程**

user-service

**概述：**

==注册服务，就是在服务上添加Eureka的客户端依赖，客户端代码会自动把服务注册到EurekaServer中。==

<strong style="font-size:24px">服务注册</strong>

在服务提供工程user-service上添加Eureka客户端依赖；自动将服务注册到EurekaServer服务地址列表。

- 添加依赖；

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

- 改造启动引导类；添加开启Eureka客户端发现的注解；

```java
@SpringBootApplication
@MapperScan("com.itheima.mapper")
@EnableDiscoveryClient // 开启Eureka客户端发现功能
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}

```

- 修改配置文件；设置Eureka 服务地址

```yml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  application:
    name: user-service
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springcloud?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
    username: root
    password: passw0rd
    driver-class-name: com.mysql.jdbc.Driver
# mybatis的配置
mybatis:
  mapper-locations: classpath:/mappers/*.xml
  type-aliases-package: com.itheima.pojo
  configuration:
    map-underscore-to-camel-case: true  #驼峰命名
#注册中心配置
eureka:
    client:
      service-url:
        defaultZone: http://127.0.0.1:10086/eureka
```

注意：
==这里我们添加了spring.application.name属性来指定应用名称，将来会作为服务的id使用。==

- 测试

重启 user-service 项目，访问Eureka监控页面

![1567605575959](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567605575959.png)



**小结**

@EnableDiscoveryClient 和 @EnableEurekaClient的区别

1：@EnableDiscoveryClient 可以注册到任何类型的注册中心，更加通用，比如未来是zookeeper

2：@EnableEurekaClient 只能注册到eureka服务中。

















### 11、SpringCloud：Eureka客户端：服务拉取



**目标**

完成eureka服务的拉取



<strong style="font-size:24px">服务发现</strong>

在服务消费工程consumer上添加Eureka客户端依赖；可以使用工具类根据服务名称获取对应的服务地址列表。

**步骤**：

分别给user-service和consumer都配置如下代码：

1：添加Eureka客户端依赖；

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2：添加启动引导类注解；

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@EnableDiscoveryClient
public class ConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

}

```

3：修改配置

```yml
server:
  port: 8080
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
spring:
  application:
    name: consumer
```

4：在consumer-demo中使用eureka来发现服务

```java
package com.itheima.controller;

import com.itheima.consumber.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    DiscoveryClient discoveryClient;


    //todo http://localhost:8080/consumber/8
    @GetMapping("/{id}")
    public User getUser(@PathVariable("id")Long id){
        //String url = "http://localhost:9091/user/8";
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        ServiceInstance serviceInstance = instances.get(0);
        String url = "http://"+serviceInstance.getHost()+":"+serviceInstance.getPort()+"/user/"+id;
        return restTemplate.getForObject(url,User.class);
    }
}

```

5：Debug跟踪运行

重启 consumer项目；然后再浏览器中再次访问 http://localhost:8080/consumer/8 ；







### 12、SpringCloud：Eureka服务端：高可用配置

**目标**：

可以启动两台eureka-server实例；在eureka管理界面看到两个实例

**分析**：

Eureka Server即服务的注册中心，在刚才的案例中，我们只有一个EurekaServer，事实上EurekaServer也可以是一个集群，形成高可用的Eureka中心。

多个Eureka Server之间也会互相注册为服务，当服务提供者注册到Eureka Server集群中的某个节点时，该节点会把服务的信息同步给集群中的每个节点，从而实现高可用集群。因此，无论客户端访问到Eureka Server集群中的任意一个节点，都可以获取到完整的服务列表信息。

![1567606046494](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606046494.png)

如果有三个Eureka，则每一个EurekaServer都需要注册到其它几个Eureka服务中，例如：有三个分别为10086、
10087、10088，则：
10086要注册到10087和10088上
10087要注册到10086和10088上 
10088要注册到10086和10087上 

![1572884794115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572884794115.png) 







==**配置高可用EurekaServer：【配置两台：10086和10087】**==

- 第一台：【自己端口为10086，Eureka注册中心端口为10087】
- 第二台：【自己端口为10087，Eureka注册中心端口为10086】

1）修改原来的EurekaServer配置；修改 eureka-server\src\main\resources\application.yml 如下：

```yml
server:
  port: 10086
spring:
  application:
    name: eureka-server
eureka:
  client:
    service-url:
      # eureka 服务地址，如果是集群的话；需要指定其它集群eureka地址,多个使用逗号分开
      defaultZone: ${defaultZone:http://127.0.0.1:10086/eureka}
    # 不注册自己 ，单机版是false,集群版本就是：true
    #register-with-eureka: false
    # 不拉取服务 单机版是false,集群版本就是：true
    #fetch-registry: false
```

所谓的高可用注册中心，其实就是把EurekaServer自己也作为一个服务，注册到其它EurekaServer上，==这样多个
EurekaServer之间就能互相发现对方==，从而形成==集群==。因此我们做了以下修改：

```properties
注意把register-with-eureka和fetch-registry修改为true或者注释掉
在上述配置文件中的${}表示在jvm启动时候若能找到对应port或者defaultZone参数则使用，若无则使用后面
的默认值
```



2)：另外一台在启动的时候可以指定端口port和defaultZone配置：

![1567606824340](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606824340.png)&nbsp;

修改原来的启动配置组件；在如下界面中的 VM options 中
设置 -DdefaultZone=http//:127.0.0.1:10087/eureka -Dserver.port=10086



复制一份并修改；在如下界面中的 VM options 中
设置 -Dport=10087 -DdefaultZone=http:127.0.0.1:10086/eureka

```properties
10086要注册到10087和10088上   
-Dport=10086 -DdefaultZone=http://127.0.0.1:10087/eureka,http://127.0.0.1:10088/eureka
10087要注册到10086和10088上   
-Dport=10087 -DdefaultZone=http://127.0.0.1:10086/eureka,http://127.0.0.1:10088/eureka
10088要注册到10086和10087上   
-Dport=10088 -DdefaultZone=http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```



![1572939458846](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572939458846.png) 





启动测试；同时启动三台eureka server

![1567606890875](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606890875.png)



4：）客户端注册服务到集群

因为EurekaServer不止一个，因此 user-service 项目注册服务或者 consumer-demo 获取服务的时候，service-url参
数需要修改为如下：

```yml
eureka:
    client:
    	service-url: # EurekaServer地址,多个地址以','隔开
    		defaultZone: http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```

为了方便上课和后面内容的修改，在测试完上述配置后可以再次改回单个eureka server的方式。













### 13、SpringCloud：Eureka客户端：服务提供者的其他配置

**目标**：

配置eureka客户端user-service的注册、续约等配置项.





#### 服务注册更换IP

==当服务往注册中心注册时：==

服务提供者在启动时，会检测配置属性中的： eureka.client.register-with-erueka=true 参数是否正确，事实上默认就是true。如果值确实为true，则会向EurekaServer发起一个Rest请求，并携带自己的元数据信息，EurekaServer会把这些信息保存到一个双层Map结构中。
1：第一层Map的Key就是服务id，一般是配置中的 spring.application.name 属性
2：第二层Map的key是服务的实例id。一般host+ serviceId + port，例如： localhost:user-service:8081
3：值则是服务的实例对象，也就是说一个服务，可以同时启动多个不同实例，形成集群。
默认注册： host使用的是主机名或者localhost，如果想用ip进行注册，可以在 user-service 中添加配置如下：

```yml
eureka:
	instance:
    	ip-address: 127.0.0.1 # ip地址 固定上报一个IP地址给eureka server
    	prefer-ip-address: true # 更倾向于使用ip，而不是host名,
```



==结构图：==

![1569405096470](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1569405096470.png) 

实例id：  小map中的key



实例id的指定

```yaml
#配置注册中心的地址
eureka:
  instance:
    #修改实例id
    instance-id: ${eureka.instance.ip-address}:${server.port}
```









#### 客户端与服务端配置服务续约

**user-service 服务提供者或者服务消费者都可以配置**

```yml
#在注册服务完成以后，服务提供者会维持一个心跳（定时向EurekaServer发起Rest请求），告诉#EurekaServer：“我还活着”。这个我们称为服务的续约（renew）；
#有两个重要参数可以修改服务续约的行为；可以在 user-service 中添加如下配置
eureka:
	instance:
		lease-expiration-duration-in-seconds: 90 # 服务失效时间
		lease-renewal-interval-in-seconds: 30 #续约的间隔时间
```

> lease-renewal-interval-in-seconds：服务续约(renew)的间隔，默认为30秒
> lease-expiration-duration-in-seconds：服务失效时间，默认值90秒

也就是说，默认情况下每隔30秒服务会向注册中心发送一次心跳，证明自己还活着。如果超过90秒没有发送心跳，EurekaServer就会认为该服务宕机，会定时（eureka.server.eviction-interval-timer-in-ms设定的时间）从服务列表中移除，这两个值在生产环境不要修改，默认即可。



==核心配置==

```yaml
spring:
  application:
    name: user-service   #不能有大写字母，不能有下划线
    
#Eureka配置
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka/    
  instance:
    prefer-ip-address: true  #上报ip地址
    ip-address: 127.0.0.1  #指定ip地址
    lease-renewal-interval-in-seconds: 5  #30秒一次心跳 【生产环境不用改；测试可以改小】
    lease-expiration-duration-in-seconds: 90 #实例过期时间【和服务剔除有关】
    instance-id: ${eureka.instance.ip-address}:${server.port} #指定实例id

```













### 14、SpringCloud：Eureka客户端：服务消费者的其他配置



#### 获取服务列表

当服务消费者启动时，会检测 eureka.client.fetch-registry=true 参数的值，如果为true，则会从Eureka
Server服务的列表拉取只读备份，然后缓存在本地。并且 每隔30秒 会重新拉取并更新数据。可以在 consumer-demo项目中通过下面的参数来修改：

**作用对象：consumer**

```yml
eureka:
  instance:
    registry-fetch-interval-seconds: 3 #默认30秒拉取一次注册信息【生产环境不改；测试环境该小】
    instance-info-replication-interval-seconds: 5 # 30秒替换实例的信息【生产环境不改；测试环境该小】
```

![1572942244261](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572942244261.png) 



==核心配置==

```yaml
#端口
server:
  port: 8080

#四要素
spring:
  #指定服务id
  application:
    name: consumer
    
#Eureka配置
eureka:
  client:
    registry-fetch-interval-seconds: 3 #默认30秒拉取一次注册信息【生产环境不改；测试环境该小】
    instance-info-replication-interval-seconds: 5 # 30秒替换实例的信息【生产环境不改；测试环境该小】
    service-url:
      defaultZone: http://localhost:10086/eureka/    

```















### 15、SpringCloud：服务下线、失效剔除

**目标**：

配置eureka失效剔除和自我保护

**作用对象：eureka-server**

如下的配置都是在Eureka Server服务端进行：

<strong style="font-size:18px">服务下线</strong>

当服务进行正常关闭操作时，它会触发一个服务下线的REST请求给Eureka Server，告诉服务注册中心：“我要下线了”。服务中心接受到请求之后，将该服务置为下线状态。

<strong style="font-size:18px">失效剔除</strong>

有时我们的服务可能由于内存溢出或网络故障等原因使得服务不能正常的工作，而服务注册中心并未收到“服务下线”的请求。相对于服务提供者的“服务续约”操作，服务注册中心在启动时会创建一个定时任务，默认每隔一段时间（默认为60秒）将当前清单中超时（默认为90秒）没有续约的服务剔除，这个操作被称为失效剔除。可以通过 eureka.server.eviction-interval-timer-in-ms 参数对其进行修改，单位是毫秒。

<strong style="font-size:18px">自我保护</strong>
我们关停一个服务，很可能会在Eureka面板看到一条警告：

操作方式：

1：先正常的启动所有的服务。能够正常的提供服务和消费服务。

2：把user-service服务关停。模拟网络抖动或者故障造成服务下线。这个时候eureka-server会启动保护机制

模拟场景：

```ruby
# 查看所有java的运行进程
> jps
```

![1567686991577](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567686991577.png)

2：通过 `taskkill /PID 57640 /F`

![1572885736781](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885736781.png) 

日志信息如下：

```properties
 Running the evict task with compensationTime 0ms
```

> 摘抄自：https://blog.csdn.net/a897180673/article/details/88412130
>
> 看到中间的 红色的大写字母了么?
> 翻译一下:
>
> ```properties
> 注意,Eureka可能会错误的将已经下线的实例申明为上线,续订低于阈值因此实例不会过期,仅仅是为了安全起见
> ```
>
> 这句话比较拗口,但是大概的意思就是,即使微服务挂掉了,我Eureka也会说他没有挂掉,这个是为了安全考虑.
>
> WTF,为了啥安全? 怎么就安全了？既然微服务下线了,Eureka你直接说微服务下线不就行了,微服务上线,你就请求路由到上线的微服务这不就行了?
>
> 为什么需要搞这一套了?
>
> 直觉告诉我事情肯定没有那么的简单,既然他弄了肯定会有他的道理
>
> 查了一些资料,看了一些介绍,大概的意思就是为了防止网络阻塞的问题
>
> 想象一个场景,以前在农村的小黑网吧里面,也就2m的网络 大概也就200kb/s的下载速度,却要给4-6台左右的电脑使用,带宽就比较的紧张了的,我以前在农村网吧上网的时候,老板就说,不要开迅雷下载啊,影响大家玩游戏
>
> 这个时候游戏服务器就相当于是Eureka,想一想,如果有人在网吧下载东西,导致其他的几个人网卡,玩游戏连接不了服务器,那么游戏服务器该怎么办呢?
>
> 立刻判断用户已经下线?
>
> 当然不是,比如最近的手机 吃鸡游戏 刺激战场,在网络卡的情况下是提醒再次等待连接,多次连接不行才让你下线重新登录.
> ![å¨è¿éæå¥å¾çæè¿°](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/20190314100610426.png)
>
> 所以Eureka也是这个道理.只不过他的保护模式有点过头了.
>
> 他为什么要这么保护么？
>
> 我们没有看源码只能猜测下:我觉得可能有这几种情况,一种是体验不好,如果网络环境糟糕,频繁上下线会很麻烦,在游戏中本身就存在很多的用户的上线和下线行为，游戏服务器必定会对此做一些优化，但是微服务是跑得应用怎么能随随便便的下线了，尽管对于EurekaServer而言，微服务是Client端，但是对于前端而言，他们都是Server，必须要稳定
>
> 第二个就是浪费时间,Tcp通信都还要3次握手,频繁的通信断开连接,时间大量的花费在沟通上,得不偿失.



这是触发了Eureka的自我保护机制。当服务未按时进行心跳续约时，Eureka会统计服务实例最近15分钟心跳续约的比例是否低于了85%。在生产环境下，因为网络延迟等原因，心跳失败实例的比例很有可能超标，但是此时就把服务剔除列表并不妥当，因为服务可能没有宕机。Eureka在这段时间内不会剔除任何服务实例，直到网络恢复正常。生产环境下这很有效，保证了大多数服务依然可用，不过也有可能获取到失败的服务实例，因此服务调用者必须做好服务的失败容错。



**问题1：85%是如何计算出来的** 85%活着的服务继续提供

这种85%是如何计算出来的呢？比如你有10个user-service节点注册到eureka-server中，这个时候挂了两台。那么比例就是：(10-2)/10 = 80% 。这个时候就会引发自我保护机制，15分钟内如果是挂一台当然就是90%是不会触发保护机制。

![1572885692715](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885692715.png) 



**问题2：自我保护机制的效果**

一旦进入到保护模式，所以的eureka服务都不再被踢出来。

> 一种是体验不好,如果网络环境糟糕,频繁上下线会很麻烦,在游戏中本身就存在很多的用户的上线和下线行为，游戏服务器必定会对此做一些优化，但是微服务是跑得应用怎么能随随便便的下线了，尽管对于EurekaServer而言，微服务是Client端，但是对于前端而言，他们都是Server，必须要稳定
>
> 第二个就是浪费时间,Tcp通信都还要3次握手,频繁的通信断开连接,时间大量的花费在沟通上,得不偿失.

**问题3：eureka为什么要有自我保护机制**

eureka的保护机制是指：在短时间内发现有15%的服务出现短暂的挂掉，那么在生产环境中是非常危险的，那么如果继续任eureka服务继续挂掉，剩下的85%某个服务列表 也会继续挂掉，会把某服务进行剔除。就会造成100%全部踢出完毕，那么在eureka中就没有这个服务地址信息，那么消息方就没有办法在消费服务。这个时候就会启动服务保护机制来防止短暂的网络问题而造成服务无法续约的问题，而被误解以为是服务器挂了问题。这种误解是非常可怕，如果没有保护机制那么就被误认为服务挂了这种是很片面的。保护机制是一种很好很有必要的机制，当然你也可以关闭这种保护机制。如下：

```yml
eureka:
  server:
    # 服务失效剔除时间间隔，默认60秒 eureka服务定时扫描失效的服务，把失效的服务剔除
    eviction-interval-timer-in-ms: 60000
    # 关闭自我保护模式（默认是打开的）
    enable-self-preservation: false
```

一句话：eureka在短时间内发现15%的服务迅速失败，但是服务是可用的，根据Eureka失效剔除，Eureka有可能把剩下的85%继续踢出，（其实可能是一种误解）。那么整个系统无法继续运作。这个时候才会启动Eureka的自我保护机制，就会把85%保护起来。





**1）消费者**

```yaml
# 服务端口
server:
  port: 8080
spring:
  application:
    name: consumer   # serviceID :  不能大写；不能有下划线
#注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
    registry-fetch-interval-seconds: 3 # 去注册中心拉取注册信息，默认值30秒一次，生产环境不改，测试环境改小
    instance-info-replication-interval-seconds: 3 #替换本地的信息
    initial-instance-info-replication-interval-seconds: 4 #替换初始化信息
```



**2）提供者**

```yaml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  application:
    name: user-service   # serviceID :  不能大写；不能有下划线
#注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
  instance:
    prefer-ip-address: true  # 上报ip地址
    ip-address: 127.0.0.1 # 告诉ip地址
    #instance-id: aaaaa  #实例id
    lease-renewal-interval-in-seconds: 3 # 生产环境不会改；测试环境改小；默认值是30
    lease-expiration-duration-in-seconds: 9 #剔除失效的实例；默认值90秒，生产环境不要改

```





**3）注册中心**

```yaml
server:
  port: 10086
spring:
  application:
    name: eureka-server
eureka:
  server:
    # 服务失效剔除时间间隔，默认60秒 eureka服务定时扫描失效的服务，把失效的服务剔除
    eviction-interval-timer-in-ms: 60000
    # 关闭自我保护模式（默认是打开的）
    enable-self-preservation: false
  client:
    service-url:
      defaultZone : http://localhost:10087/eureka

```









三步走：

1）启动器依赖   pom.xml

2）开启启动器    XXXXApplication启动类中

3）配置			    application.yml



















### 16、SpringCloud：Ribbon负载均衡





**目标**：

描述负载均衡和ribbon的作用

配置启动两个用户服务，在consumer中使用服务名实现根据用户id获取用户



**分析**：

负载均衡是一个算法，可以通过该算法实现从地址列表中获取一个地址进行服务调用。

在Spring Cloud中提供了负载均衡器：Ribbon

在刚才的案例中，我们启动了一个 user-service ，然后通过DiscoveryClient来获取服务实例信息，然后获取ip和端口来访问。

![1572885961221](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885961221.png)

但是实际环境中，往往会开启很多个 user-service 的集群。此时获取的服务列表中就会有多个，到底该访问哪一个呢？一般这种情况下就需要编写负载均衡算法，在多个实例列表中进行选择。不过Eureka中已经集成了负载均衡组件：Ribbon，简单修改代码即可使用。

![1572885983333](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885983333.png)

没错就是Ribbon来做这个事情。

![1572886002499](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572886002499.png)



需求：可以使用RestTemplate访问http://user-service/user/8获取服务数据。

可以使用Ribbon负载均衡：在执行RestTemplate发送服务地址请求的时候，使用负载均衡拦截器拦截，根据服务名获取服务地址列表，使用Ribbon负载均衡算法从服务地址列表中选择一个服务地址，访问该地址获取服务数据。







Eureka客户端已经内置了Ribbon负载均衡策略，所以不用再导入依赖

![1572944657198](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944657198.png) 







![1572944993672](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944993672.png) 







实现步骤：



<strong style="font-size:24px">1：启动多个user-service实例（8082,8083）；</strong>

也可以在eureka-server中查看到user-service服务已被注册。

![1572944222966](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944222966.png) 



<strong style="font-size:24px">2：修改RestTemplate实例化方法，添加负载均衡注解；</strong>

因为Eureka中已经集成了Ribbon，所以我们无需引入新的依赖。
直接修改 consumer-demo\src\main\java\com\itheima\consumer\ConsumerApplication.java
在RestTemplate的配置方法上添加 @LoadBalanced 注解：

<strong style="color:red;">Ribbon默认的负载均衡策略是轮询。</strong>

```java
@SpringBootApplication
@EnableDiscoveryClient
public class ConsumberApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

}

```

<strong style="font-size:24px">3：修改ConsumerController；</strong>

修改 consumer-demo\src\main\java\com\itheima\controller\ConsumerController.java 调用方
式，不再手动获取ip和端口，而是直接通过服务名称调用；

```java
 //todo http://localhost:8080/consumber/8
 @GetMapping("/{id}")
 public User getUser(@PathVariable("id")Long id){
 	String url = "http://user-service/user/8";
 	return restTemplate.getForObject(url,User.class);
 }
```

访问页面，查看结果；并可以在8081和8082的控制台查看执行情况：
了解：Ribbon默认的负载均衡策略是轮询。SpringBoot也帮提供了修改负载均衡规则的配置入口在consumer的配置文件中添加如下，就变成随机的了：

```yml
user-service:
	ribbon:
		NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

==格式是： {服务名称}.ribbon.NFLoadBalancerRuleClassName==







其他的策略

| **内置负载均衡规则类**    | **规则描述**                                                 |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：<br>（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。注意：可以通过修改配置loadbalancer.<clientName>.connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。<br>（2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上线，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| ZoneAvoidanceRule         | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。 |
| BestAvailableRule         | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| Retry                     | 重试机制的选择逻辑                                           |



重启服务生效。通过下面的源码追踪才看效果。



<strong style="font-size:24px">5：源码追踪</strong>

为什么只输入了service名称就可以访问了呢？之前还要获取ip和端口。显然是有组件根据service名称，获取到了服务实例的ip和端口。因为 consumer-demo 使用的是RestTemplate，spring的负载均衡自动配置类LoadBalancerAutoConfiguration.LoadBalancerInterceptorConfig 会自动配置负载均衡拦截器（在spring-cloud-commons-**.jar包中的spring.factories中定义的自动配置类）， 

它就是LoadBalancerInterceptor ，这个类会在对RestTemplate的请求进行拦截，然后从Eureka根据服务id获取服务列表，随后利用负载均衡算法得到真实的服务地址信息，替换服务id。
我们进行源码跟踪：

![1572908354313](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908354313.png) 



![1567651684455](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651684455.png)

继续跟入execute方法：发现获取了8082端口的服务

![1567651668799](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651668799.png)

再跟下一次，发现获取的是8081、8082之间切换：

![1567651739714](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651739714.png)

多次访问 consumer 的请求地址；然后跟进代码，发现其果然实现了负载均衡。并且在getServer中找到了配置的随机策略。

![1567661798680](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567661798680.png)

<strong style="font-size:24px">6：测试</strong>

在实例化RestTemplate的时候使用@LoadBalanced，服务地址直接可以使用服务名。





**小结**：

Ribbon提供了轮询、随机两种负载均衡算法（默认是轮询）可以实现从地址列表中使用负载均衡算法获取地址进行服务调用。

关键类：

1：LoadBalancerInterceptor --- > 2：找到intercept方法 ----> 3：RibbonLoadBalancerClient-的execute()方法----->找到加载负载均衡器和具体的服务。

















### 17、SpringCloud：熔断器Hystrix简介

**目标**：了解熔断器Hystrix的作用

**概述**

Hystrix 在英文里面的意思是 豪猪，它的logo 看下面的图是一头豪猪，它在微服务系统中是一款提供保护机制的组件，和eureka一样也是由netflix公司开发。

主页：https://github.com/Netflix/Hystrix/

![1567664082422](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664082422.png)

那么Hystrix的作用是什么呢？具体要保护什么呢？
Hystrix是Netflix开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败。



<strong style="font-size:24px"> 雪崩问题</strong>

微服务中，服务间调用关系错综复杂，一个请求，可能需要调用多个微服务接口才能实现，会形成非常复杂的调用链路

如图：



![1567664157448](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664157448.png)&nbsp;



如图，一次业务请求，需要调用A、P、H、I四个服务，这四个服务又可能调用其它服务。
如果此时，某个服务出现异常：

![1567664228865](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664228865.png)

例如： 微服务I 发生异常，请求阻塞，用户请求就不会得到响应，则tomcat的这个线程不会释放，于是越来越多的
用户请求到来，越来越多的线程会阻塞：

![1567664256022](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664256022.png)

服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用，形成雪崩效应。这就好比，一个汽车生产线，生产不同的汽车，需要使用不同的零件，如果某个零件因为种种原因无法使用，那么就会造成整台车无法装配，陷入等待零件的状态，直到零件到位，才能继续组装。  此时如果有很多个车型都需要这个零件，那么整个工厂都将陷入等待的状态，导致所有生产都陷入瘫痪。一个零件的波及范围不断扩大。

Hystrix解决雪崩问题的手段主要是服务降级，包括：

> 线程隔离
>
> 服务降级
>
> 服务熔断 



















### 18、SpringCloud：线程隔离&服务降级

**目标**：

了解什么是线程隔离和服务降级

**线程隔离示意图：**

![1567664783655](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664783655.png) 

**分析**：

Hystrix解决雪崩效应：

1：线程隔离：Hystrix为每个依赖服务调用分配一个小的线程池，如果线程池已满调用将被立即拒绝，默认不采用排队，加速失败判定时间。用户的请求将不再直接访问服务，而是通过线程池中的空闲线程来访问服务，如果线程池已满，或者请求超时，则会进行降级处理，什么是服务降级？

2：服务降级：优先保证核心服务，而非核心服务不可用或弱可用。：

用户的请求故障时，不会被阻塞，更不会无休止的等待或者看到系统崩溃，至少可以看到一个执行结果（例如返回友好的提示信息） 。服务降级虽然会导致请求失败，但是不会导致阻塞，而且最多会影响这个依赖服务对应的线程池中的资源，对其它服务没有响应。



触发Hystrix服务降级的情况：

> 线程池已满
> 请求超时



**步骤**

1：在消费方加入hystrix起步依赖

2：在启动类加入注解

3：在需要隔离得方法上进行配置



<strong style="font-size:24px">添加依赖</strong>

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

<strong style="font-size:24px">开启熔断</strong>

在启动类 ConsumerApplication 上添加注解：@EnableCircuitBreaker

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker
@RibbonClient(name = "SPRING-MICROSERVICECLOUD-RIBBON",configuration= MySelfRule.class)
public class ConsumberApplication{}
```

可以看到，我们类上的注解越来越多，在微服务中，经常会引入上面的三个注解，于是Spring就提供了一个组合注解：@SpringCloudApplication,自定义注解如下：

```java
package com.itheima.consumber.core;

import com.itheima.consumber.ribbon.MySelfRule;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

import java.lang.annotation.*;


@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker  // 等价于  @EnableHystrix
@RibbonClient(name = "SPRING-MICROSERVICECLOUD-RIBBON",configuration= MySelfRule.class)
public @interface SpringCloudApplication {
}

```

因此，我们可以使用这个组合注解来代替之前的4个注解，优化如下：

```java
@SpringCloudApplication
public class ConsumberApplication {
}
```

==@EnableCircuitBreaker 等价于  @EnableHystrix==



<strong style="font-size:24px">降级逻辑</strong>

当目标服务的调用出现故障，我们希望快速失败，给用户一个友好提示。因此需要提前编写好失败时的降级处理逻
辑，要使用HystrixCommand来完成。
改造 consumer-demo\src\main\java\com\itheima\consumer\controller\ConsumerController.java 处理器
类，如下： 

```java
package com.itheima.consumber.web;

import com.itheima.consumber.pojo.User;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.client.loadbalancer.LoadBalancerInterceptor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@Slf4j
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;


    //todo http://localhost:8080/consumber/8
    @GetMapping("{id}")
    @HystrixCommand(fallbackMethod = "queryByIdFallBack")
    public String getUser(@PathVariable("id")Long id){
        String url = "http://user-service/user/8";
        return restTemplate.getForObject(url,String.class);
    }

    public String queryByIdFallBack(Long id){
        System.out.println("查询用户失败: id = " + id);
        return "对不起网络太拥挤，请稍后再试...";
    }
}

```

> ==要注意；因为熔断的降级逻辑方法必须跟正常逻辑方法保证：相同的参数列表和返回值声明。失败逻辑中返回User对象没有太大意义，一般会返回友好提示。所以把queryById的方法改造为返回String，反正也是Json数据。这样失败逻辑中返回一个错误说明，会比较方便。==

说明：

@HystrixCommand(fallbackMethod = "queryByIdFallBack")：用来声明一个降级逻辑的方法

测试：
当 user-service 正常提供服务时，访问与以前一致。但是当将 user-service 停机时，会发现页面返回了降级处理

![1572908478982](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908478982.png) 





<strong style="font-size:24px">修改超时配置</strong>

在之前的案例中，请求在超过1秒后都会返回错误信息，这是因为Hystrix的默认超时时长为1，我们可以通过配置修
改这个值；修改 consumer-demo\src\main\resources\application.yml 添加如下配置：

```yml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
```

这个配置会作用于全局所有方法。为了方便复制到yml配置文件中，可以复制

```properties
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000 
```

到yml文件中会自动格式化后再进行修改。



局部配置是通过：commandProperties属性配合@HystrixProperty来实现得。当然这样更精准。因为在实际得运维生产环境中，我们每个服务调用时间和执行实际情况都不一样。

```java
@GetMapping("{id}")@HystrixCommand(fallbackMethod = "queryByIdFallBack",commandProperties = {        @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value="2000")})
public String getUser(@PathVariable("id")Long id){
```

这些属性得名字都在`HystrixCommandProperties`类中查询获取。多个属性用逗号分开。

![1572908841233](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908841233.png) 

```java
execution.isolation.thread.timeoutInMilliseconds 默认情况下是：1秒
```



**服务提供方模拟超时**

为了触发超时，可以在 user-service\src\main\java\com\itheima\user\service\UserService.java 的方法中随机秒来进行测试；来模拟网络网络超时：

```java
package com.itheima.controller;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Random;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    //todo:http://localhost:9091/user/8
    @GetMapping("/{id}")
    public User queryById(@PathVariable Long id){
        try {
            //模拟网络拥堵的情况，如果请求的时间没有超过hystrix的超时时间直接放行，如果超过就会启动熔断机制
            Thread.sleep(new Random().nextInt(10000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return userService.queryById(id);
    }
}

```

可以发现，请求的时长已经到了2s+，证明配置生效了。如果把修改时间修改到2秒以下，又可以正常访问

![1567666986156](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567666986156.png)

![1567667044523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567667044523.png)





<strong style="font-size:24px">默认的Fallback</strong>

刚才把fallback写在了某个业务方法上，如果这样的方法很多，那岂不是要写很多。所以可以把Fallback配置加在类上，实现默认fallback；再次改造 consumer-demo\src\main\java\com\itheima\consumer\controller\ConsumerController.java

```java
package com.itheima.consumer.web;

import com.itheima.consumer.pojo.User;
import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.client.loadbalancer.LoadBalancerInterceptor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@Slf4j
@RequestMapping("/consumber")
@DefaultProperties(defaultFallback = "defaultFallback")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    //todo http://localhost:8080/consumber/8
    @GetMapping("{id}")
    @HystrixCommand
    public String getUser(@PathVariable("id")Long id){
        String url = "http://user-service/user/8";
        return restTemplate.getForObject(url,String.class);
    }

    public String defaultFallback(){
        return "默认提示：对不起，网络太拥挤了！";
    }

}

```

```java
@DefaultProperties(defaultFallback = "defaultFallBack")：在类上指明统一的失败降级方法；该类中所有方法
返回类型要与处理失败的方法的返回类型一致。
```

![1567668656183](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567668656183.png)

















### 19、SpringCloud：服务熔断原理

**目标**：了解熔断器工作原理

**概述** 

在服务熔断中，使用的熔断器，也叫断路器，其英文单词为：Circuit Breaker熔断机制与家里使用的电路熔断原理类似；当如果电路发生短路的时候能立刻熔断电路，避免发生灾难。在分布式系统中应用服务熔断后；服务调用方可以自己进行判断哪些服务反应慢或存在大量超时，可以针对这些服务进行主动熔断，防止整个系统被拖垮。
==Hystrix的服务熔断机制，可以实现弹性容错；当服务请求情况好转之后，可以自动重连。通过断路的方式，将后续请求直接拒绝，一段时间（默认5秒）之后允许部分请求通过，如果调用成功则回到断路器关闭状态，否则继续打开，拒绝请求的服务。==

Hystrix的熔断状态机模型：



![1573444713021](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444713021.png) 





![1573444732452](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444732452.png) 

状态机有3个状态：
1：Closed：关闭状态（断路器关闭），所有请求都正常访问。
2：Open：打开状态（断路器打开），所有请求都会被降级。Hystrix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断，断路器会完全打开。默认失败比例的阈值是50%，请求次数最少不低于20次。
3：Half Open：半开状态，不是永久的，断路器打开后会进入休眠时间（默认是5S）。随后断路器会自动进入半开状态。此时会释放部分请求通过，若这些请求都是健康的，则会关闭断路器，否则继续保持打开，再次进行休眠计时。



<strong style="font-size:24px">动手实践</strong>

为了能够精确控制请求的成功或失败，在 consumer 的处理器业务方法中加入一段逻辑；
修改 consumer-demo\src\main\java\com\itheima\controller\ConsumerController.java

```java
@GetMapping("{id}")
@HystrixCommand
public String getUser(@PathVariable("id")Long id){
    if(id == 1){
        throw new RuntimeException("太忙了");
    }
    String url = "http://user-service/user/"+id;
    return restTemplate.getForObject(url,String.class);
}
```

增加模拟出现异常。

这样如果参数是id为1，一定失败，其它情况都成功。（不要忘了清空user-service中的休眠逻辑）
我们准备两个请求窗口：
一个请求：http://localhost:8080/consumer/1，注定失败
一个请求：http://localhost:8080/consumer/2，肯定成功
==当我们疯狂访问id为1的请求时（超过20次） 超时的次数通过：requestVolumeThreshold=20修改，就会触发熔断。断路器会打开，一切请求都会被降级处理。==
此时你访问id为2的请求，会发现返回的也是失败，而且失败时间很短，只有20毫秒左右，可以通过 sleepWindowInMilliseconds进行修改配置；因进入半开状态之后2是可
以的。

![1567670138646](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567670138646.png)

不过，默认的熔断触发要求较高，休眠时间窗较短，为了测试方便，我们可以通过配置修改熔断策略：



可以通过配置服务熔断参数修改默认：

```yml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
      circuitBreaker:
        errorThresholdPercentage: 50 # 触发熔断错误比例阈值，默认值50% 也就是说requestVolumeThreshold 出现错误得次数超过5层就会激活熔断
        sleepWindowInMilliseconds: 10000 # 熔断后休眠时长，默认值5秒
        requestVolumeThreshold: 10 # 熔断触发最小请求次数，默认值是20
```

application的properties文件可以用下面的方式：

```properties
hystrix.command.default.circuitBreaker.requestVolumeThreshold=10
hystrix.command.default.circuitBreaker.sleepWindowInMilliseconds=10000
hystrix.command.default.circuitBreaker.errorThresholdPercentage=50
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000
```





**问题：**

为什么SpringClound进行远程服务调用时，实体类为什么不需要序列化了？HTTP协议是以什么格式传输数据呢？

> Http协议：Java对象  --->  Json字符串(String已经实现序列化) ---->  二进制 ----> 网络传输
>
> RPC协议：Java对象  -->  (pojo序列化)二进制 ----> 网络传输















### 20、课程总结





![1577785758268](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1577785758268.png) 

### 01、课程目标

1：了解什么是微服务架构和系统架构的演进

2：了解HTTP和RPC的区别

3：掌握HttpClient的基本使用

4：能够使用RestTemplate发送请求 

5：能够说出SpringCloud的作用 

6：能够搭建Eureka注册中心 

7：能够使用Robbin负载均衡 

8：能够使用Hystrix熔断器



























### 02、服务架构的演变

**目标：**

了解项目架构的演变历程





演变过程：

```
1、集中式架构
2、垂直拆分
3、分布式服务
4、服务治理（SOA）
5、微服务
```





==**1、集中式架构：**==

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本

![1572876520974](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572876520974.png) 

```
优点：
	- 系统开发速度快 
	- 维护成本低 
	- 适用于并发要求较低的系统

缺点：
	- 代码耦合度高，
	- 后期维护困难 
	- 无法针对不同模块进行针对性优化 
	- 无法水平扩展 
	- 单点容错率低，
	- 并发能力差
```



==**2、垂直拆分：**==

当访问量逐渐增大，单一应用无法满足需求，此时为了应对更高的并发和业务需求，我们根据业务功能对系统进行拆 分：

![1572876617378](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572876617378.png) 

```
优点：
	- 系统拆分实现了流量分担，解决了并发问题
	- 可以针对不同模块进行优化
	- 方便水平扩展，负载均衡，容错率提高
缺点：
	- 系统间相互独立，会有很多重复开发工作，影响开发效率
```



==**3、分布式服务：**==

当垂直应用越来越多，应用之间交互不可避免，将==核心业务抽取出来==，作为独立的服务，逐渐形成稳定的服务中心， 使前端应用能更快速的响应多变的市场需求。

![1572877220559](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877220559.png) 

```
优点：
	- 将基础服务进行了抽取，系统间相互调用，提高了代码复用和开发效率
缺点：
	- 系统间耦合度变高，调用关系错综复杂，难以维护
	
出现了什么问题？
	- 服务越来越多，需要管理每个服务的地址
	- 调用关系错综复杂，难以理清依赖关系
	- 服务过多，服务状态难以管理，无法根据服务情况动态管理	
```



==**4、服务治理（SOA）：**==

SOA（Service Oriented Architecture）面向服务的架构：它是一种设计方法，其中包含多个服务， 服务之间通过相 互依赖终提供一系列的功能。一个服务 通常以独立的形式存在与操作系统进程中。各个服务之间 通过网络调用。 SOA结构图：

![1572877279458](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877279458.png)

ESB（企业服务总线），简单 来说 ESB 就是一根管道，用来连接各个服务节点。为了集 成不同系统，不同协议的服务，ESB 做了消息的转化解释和路由工作，让不同的服务互联互通。

**SOA缺点：**每个供应商提供的ESB产品有偏差，自身实现较为复杂；应用服务粒度较大，ESB集成整合所有服务和协 议、数据转换使得运维、测试部署困难。所有服务都通过一个通路通信，直接降低了通信速度。

 

![1572877485365](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877485365.png) 

```
服务治理要做什么？
	- 服务注册中心，实现服务自动注册和发现，无需人为记录服务地址
	- 服务自动订阅，服务列表自动推送，服务调用透明化，无需关心依赖关系
	- 动态监控服务状态监控报告，人为控制服务状态
缺点：
	- 服务间会有依赖关系，一旦某个环节出错会影响较大
	- 服务关系复杂，运维、测试部署困难，不符合DevOps思想
```



==**5、微服务：**==

![1572877429523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877429523.png) 

```
微服务的特点：
	- 单一职责：微服务中每一个服务都对应唯一的业务能力，做到单一职责
	- 微：微服务的服务拆分粒度很小，例如一个用户管理就可以作为一个服务。每个服务虽小，但“五脏俱全”。
	- 面向服务：面向服务是说每个服务都要对外暴露服务接口API。并不关心服务的技术实现，
	  做到与平台和语言无关，也不限定用什么技术实现，只要提供Rest的接口即可。
	- 自治：自治是说服务间互相独立，互不干扰
  	- 团队独立：每个服务都是一个独立的开发团队，人数不能过多。
  	- 技术独立：因为是面向服务，提供Rest接口，使用什么技术没有别人干涉
  	- 前后端分离：采用前后端分离开发，提供统一Rest接口，后端不用再为PC、移动段开发不同接口
  	- 数据库分离：每个服务都使用自己的数据源
  	- 部署独立，服务间虽然有调用，但要做到服务重启不影响其它服务。有利于持续集成和持续交付。
  	  每个服务都是独立的组件，可复用，可替换，降低耦合，易维护
```





**小结：**

![1572877554264](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877554264.png) 















### 03、远程调用的方式

**目标**：能够说出服务调用方式种类





无论是微服务还是SOA，都面临着服务间的远程调用。那么服务间的远程调用方式有哪些呢？

常见的远程调用方式有以下2种：

1：**RPC**：Remote Produce Call远程过程调用，**RPC基于Socket，工作在会话层。自定义数据格式**，速度快，效率高。早期的webservice，现在热门的dubbo，都是RPC的典型代表

![1572877711327](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877711327.png) 



2：**Http**：http其实是**一种网络传输协议，基于TCP，工作在应用层，规定了数据传输的格式**。现在客户端浏览器与服务端通信基本都是采用Http协议，也可以用来进行远程服务调用。缺点是消息封装臃肿，优势是对服务的提供和调用方没有任何技术限定，自由灵活，更符合微服务理念。

![1572877756745](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877756745.png) 



现在热门的Rest风格，就可以通过http协议来实现。



**区别**：RPC的机制是根据语言的API（language API）来定义的，而不是根据基于网络的应用来定义的。

如果你们公司全部采用Java技术栈，那么使用Dubbo作为微服务架构是一个不错的选择。

相反，如果公司的技术栈多样化，而且你更青睐Spring家族，那么Spring Cloud搭建微服务是不二之选。在我们的项目中，会选择Spring Cloud套件，因此会使用Http方式来实现服务间调用。



















### 04、使用RestTemplate进行远程调用



进行远程调用：

​		1、HttpClient

​		2、RestTemplate



Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化，非常方便。RestTemplate并没有限定Http的客户端类型，而是进行了抽象，目前常用的3种都有支持：

- HttpClient

- OkHttp

- JDK原生的URLConnection（默认的）



导入依赖：

```xml
	<dependencies>
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
    </dependencies>
```







测试代码1：

```java
package com.itheima.test;

import com.itheima.pojo.Book;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.BasicResponseHandler;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.junit.Test;

import com.fasterxml.jackson.databind.ObjectMapper;

public class Test01_http {
	
	private CloseableHttpClient httpClient = HttpClients.createDefault();
	
	/**
	 * get请求
	 * @throws Exception
	 */
	@Test
	public void test1() throws Exception {
		HttpGet request = new HttpGet("http://localhost:8080/book/3");
		String response = this.httpClient.execute(request, new BasicResponseHandler());
		System.out.println(response);
		
	}
	
	
	/**
	 * post请求
	 * @throws Exception
	 */
	@Test
	public void test2() throws Exception {
		HttpPost post = new HttpPost("http://localhost:8080/savebook");
		//httpPost.setHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36");
		/*
		 * post带参数开始
		 */
		//请求内容
		String content = "{\"bookname\":\"SpringCloud入门到精通\",\"bookdesc\":\"这是一本好书啊好书啊！\",\"price\":111999}";
		//使用addHeader方法添加请求头部,诸如User-Agent, Accept-Encoding等参数.
		post.setHeader("Content-Type", "application/json;charset=UTF-8");
		//组织数据
		StringEntity se = new StringEntity(content,"UTF-8");
		//设置编码格式
		se.setContentEncoding("UTF-8");
		//设置数据类型
		se.setContentType("application/json");
		//对于POST请求,把请求体填充进HttpPost实体.
		post.setEntity(se);
		
		//执行请求
		String response = this.httpClient.execute(post, new BasicResponseHandler());
		System.out.println(response);
	}
	
	/**
	 * 返回字符串转成对象
	 * @throws Exception
	 */
	@Test
	public void test3() throws Exception {
		HttpGet request = new HttpGet("http://localhost:8080/book/3");
		String response = this.httpClient.execute(request, new BasicResponseHandler());
		System.out.println(response);
		
		System.out.println("字符串转成对象。。。。。");
		ObjectMapper om = new ObjectMapper();
		Book book = om.readValue(response, Book.class);
		System.out.println(book);
		System.out.println(book.getBookname());
		
	}

}
```



测试代码2：

```java
package com.itheima.test;

import com.itheima.pojo.Book;
import org.junit.Test;
import org.springframework.web.client.RestTemplate;

public class Test02_RestTemplate {

	/**
	 * Spring中的RestTemplate完成调用
	 * @throws Exception
	 */
	@Test
	public void test() throws Exception {
		RestTemplate rt = new RestTemplate();
		Book book = rt.getForObject("http://localhost:8080/book/3", Book.class);
		System.out.println(book);
	}

}
```





getForObject(Url，把返回值转成什么类型)

















### 05、SpringCloud介绍

**目标**：Spring Cloud整合的组件和版本特征



微服务是一种架构方式，终肯定需要技术架构去实施。---SpringBoot

微服务的实现方式很多，但是火的莫过于Spring Cloud了。为什么？

> - 后台硬：作为Spring家族的一员，有整个Spring全家桶靠山，背景十分强大。 
> - 技术强：Spring作为Java领域的前辈，可以说是功力深厚。有强力的技术团队支撑，一般人还真比不了 
> - 群众基础好：可以说大多数程序员的成长都伴随着Spring框架，试问：现在有几家公司开发不用Spring？ Spring Cloud与Spring的各个框架无缝整合，对大家来说一切都是熟悉的配方，熟悉的味道。 
> - 使用方便：相信大家都体会到了SpringBoot给我们开发带来的便利，而Spring Cloud完全支持Spring Boot的开 发，用很少的配置就能完成微服务框架的搭建

SpringCloud是Spring旗下的项目之一，[官网地址：http://projects.spring.io/spring-cloud/](http://projects.spring.io/spring-cloud/)

![1569396164776](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1569396164776.png) 

Spring最擅长的就是集成，把世界上最好的框架拿过来，集成到自己的项目中。

SpringCloud也是一样，它将现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。协调分布式环境中各个系统，为各类服务提供模板性配置。其主要 涉及的组件包括：

- Eureka：注册中心 
- Zuul/Gateway：服务网关 
- Ribbon：负载均衡
- Feign：服务调用
- Hystix：熔断器
- 统一配置中心
- 服务总线

以上只是其中一部分，架构图：

![1562414270990](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1562414270990.png)  





**SpringCloud的版本**：

Spring Cloud不是一个组件，而是许多组件的集合；它的版本命名比较特殊，是以A到Z的为首字母的一些单词（其实是伦敦地铁站的名字）组成：

==1、传统的版本号规则是什么？==

springfremaework-4.3.11.release

`4.3.1.release` 分别是：`主版本号.次版本号.增强版本号.里程碑版本号。`

- 主版本号：项目的重大重构
- 次版本号：新功能的添加和变换。
- 增强版本号：BUG的修复
- 里程碑版本号：release

==2、Springcloud的版本号规则==

<https://spring.io/projects/spring-cloud#learn>

- 版本特征：以英文单词命名（伦敦地铁站名）

这是 版本号都是英国伦敦的地铁站的站名。依次从A-Z。

![1572881064543](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881064543.png)

3、为什么springcloud用的是单词而不是数字呢？

设计的目的是为了管理好每个springcloud的子项目清单，避免自己的版本和子项目版本的版本号混淆。

4、什么是版本的发布计划。

也就是上面的SR5,SR2之类的。 

![1572881129963](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881129963.png) 



==注意：我们在项目中，使用新稳定的Greenwich版本。==



其中包含的组件，也都有各自的版本，如下表：

| Component                 | Edgware.SR4    | Finchley.SR1  | Finchley.BUILD-SNAPSHOT |
| ------------------------- | -------------- | ------------- | ----------------------- |
| spring-cloud-aws          | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-bus          | 1.3.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-cli          | 1.4.1.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-commons      | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-contract     | 1.2.5.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-config       | 1.4.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-netflix      | 1.4.5.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-security     | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-cloudfoundry | 1.1.2.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-consul       | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-sleuth       | 1.3.4.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-stream       | Ditmars.SR4    | Elmhurst.SR1  | Elmhurst.BUILD-SNAPSHOT |
| spring-cloud-zookeeper    | 1.2.2.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-boot               | 1.5.14.RELEASE | 2.0.4.RELEASE | 2.0.4.BUILD-SNAPSHOT    |
| spring-cloud-task         | 1.2.3.RELEASE  | 2.0.0.RELEASE | 2.0.1.BUILD-SNAPSHOT    |
| spring-cloud-vault        | 1.1.1.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-gateway      | 1.0.2.RELEASE  | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-openfeign    |                | 2.0.1.RELEASE | 2.0.2.BUILD-SNAPSHOT    |
| spring-cloud-function     | 1.0.0.RELEASE  | 1.0.0.RELEASE | 1.0.1.BUILD-SNAPSHOT    |

接下来，我们就一一学习SpringCloud中的重要组件。







### 06、SpringBoot和SpringCloud与其他框架的对比

**目标**

掌握和了解springboot和springcloud的区别和差异。

**概述**

==springboot是spring的一套快速配置的脚手架，==可以基于Springboot快速开发单个应用，SpringCloud是一个基于SpringBoot实现的云应用开发工具，SpringBoot专注快速，方便集成的单个微服务个体，==SpringCloud 关注全局的服务治理框架，==SpringBoot使用了默认大于配置的理念，很多集成方案已经帮你选好了，能不配置就不配置，SpringCloud很大的一部分是基于SpringBoot来实现，可以不基于SpringBoot吗？答案是：不可以。



SpringBoot可以离开springcloud独立开发项目，但是SpringCloud不能离开springboot，属于依赖关系。

**SpringBoot和SpringCloud对照表：**

| spring-boot-starter-parent | spring-cloud-dependencies |                         |
| -------------------------- | ------------------------- | ----------------------- |
| 版本号                     | 发布日期                  | 版本号                  |
| 1.5.2.RELEASE              | 2017年3月                 | Dalston.RC1             |
| 1.5.9.RELEASE              | 2017年11月                | Edgware.RELEASE         |
| 1.5.16.RELEASE             |                           | Edgware.SR5             |
| 1.5.20.RELEASE             |                           | Edgware.SR5             |
| 2.0.2.RELEASE              | 2018年5月                 | Finchley.BUILD-SNAPSHOT |
| 2.0.6.RELEASE              |                           | Finchley.SR2            |
| 2.1.4.RELEASE              |                           | Greenwich.SR1           |
| 2.1.5.RELEASE              |                           | Greenwich.SR2           |
| 2.1.6.RELEASE              |                           | Greenwich.SR3           |
| 2.2.x                      |                           | Hoxton                  |
| 未完待续......             |                           |                         |



**springcloud和dubbo 的比较**

许很多人会说Spring Cloud和Dubbo的对比有点不公平，Dubbo只是实现了服务治理，而Spring Cloud下面有17个子项目（可能还会新增）分别覆盖了微服务架构下的方方面面，服务治理只是其中的一个方面，一定程度来说，Dubbo只是Spring Cloud Netflix中的一个子集。但是在选择框架上，方案完整度恰恰是一个需要重点关注的内容。

根据Martin Fowler对 [微服务架构](http://martinfowler.com/articles/microservices.html) 的描述中，虽然该架构相较于单体架构有模块化解耦、可独立部署、技术多样性等诸多优点，但是由于分布式环境下解耦，也带出了不少[测试](http://lib.csdn.net/base/softwaretest)与运维复杂度。

根据微服务架构在各方面的要素，看看Spring Cloud和Dubbo都提供了哪些支持。

|              | Dubbo     | Spring Cloud                                                 |
| :----------- | :-------- | :----------------------------------------------------------- |
| 服务注册中心 | Zookeeper | Spring Cloud Netflix Eureka                                  |
| 服务调用方式 | RPC       | REST API http                                                |
| 服务网关     | 无        | Spring Cloud Netflix Zuul            \|  Spring Cloud Gateway |
| 断路器       | 不完善    | Spring Cloud Netflix Hystrix                                 |
| 分布式配置   | 无        | Spring Cloud Config                                          |
| 服务跟踪     | 无        | Spring Cloud Sleuth                                          |
| 消息总线     | 无        | Spring Cloud Bus                                             |
| 数据流       | 无        | Spring Cloud Stream                                          |
| 批量任务     | 无        | Spring Cloud Task                                            |

虽然，Dubbo自身只是实现了服务治理的基础，其他为保证集群安全、可维护、可测试等特性方面都没有很好的实现，但是几乎大部分关键组件都能找到第三方开源来实现，这些组件主要来自于国内各家大型互联网企业的开源产品。

![1572881423647](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881423647.png) 

具体对比分析

<https://mp.weixin.qq.com/s/omVAEzQTcV5o5AGsSU_u7Q>

![1572881462658](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881462658.png) 

























### 07、前期准备工作：搭建微服务场景

**目标**：

创建微服务父工程springcloud-demo、用户服务工程user-service、服务消费工程consumer



**分析**：

需求：查询数据库中的用户数据并输出到浏览器

![1572881710094](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572881710094.png) 



#### 父工程

- 父工程springcloud-demo：添加spring boot父坐标和管理其它组件的依赖

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
  
      <groupId>net.cfei</groupId>
      <artifactId>springcloud-demo</artifactId>
      <packaging>pom</packaging>
      <version>1.0-SNAPSHOT</version>
  
      <modules>
          <module>user-service</module>
          <module>consumer</module>
      </modules>
  
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.1.4.RELEASE</version>
          <relativePath/>
      </parent>
  
      <properties>
          <java.version>1.8</java.version>
          <spring-cloud.version>Greenwich.SR1</spring-cloud.version>
          <mapper.starter.version>2.1.5</mapper.starter.version>
          <mysql.version>5.1.46</mysql.version>
      </properties>
  
      <dependencyManagement>
          <dependencies>
              <!-- springCloud -->
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>${spring-cloud.version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
  
              <!-- 通用Mapper启动器 -->
              <dependency>
                  <groupId>org.mybatis.spring.boot</groupId>
                  <artifactId>mybatis-spring-boot-starter</artifactId>
                  <version>2.0.1</version>
              </dependency>
              <dependency>
                  <groupId>com.github.pagehelper</groupId>
                  <artifactId>pagehelper-spring-boot-starter</artifactId>
                  <version>1.2.10</version>
              </dependency>
              <dependency>
                  <groupId>tk.mybatis</groupId>
                  <artifactId>mapper-spring-boot-starter</artifactId>
                  <version>2.1.5</version>
              </dependency>
              <!-- mysql驱动 -->
              <dependency>
                  <groupId>mysql</groupId>
                  <artifactId>mysql-connector-java</artifactId>
                  <version>${mysql.version}</version>
              </dependency>
  
          </dependencies>
      </dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
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



#### 服务提供者

- 用户服务工程user-service：整合mybatis查询数据库中用户数据；提供查询用户服务

1：添加启动器依赖（web、通用Mapper）；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud-demo</artifactId>
        <groupId>net.cfei</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>user-service</artifactId>

    <dependencies>
        <!--web的依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- mybatis和mysql的驱动 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--mybatis的分页插件-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
        </dependency>
        <!-- 通用Mapper启动器 -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

</project>
```

2：创建启动引导类和配置文件；

```yaml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springcloud?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
    username: root
    password: passw0rd
    driver-class-name: com.mysql.jdbc.Driver
# mybatis的配置
mybatis:
  mapper-locations: classpath:/mappers/*.xml
  type-aliases-package: com.itheima.pojo
  configuration:
    map-underscore-to-camel-case: true  #驼峰命名

```

3：启动类开启通用mapper注解

```java
package com.itheima.user;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import tk.mybatis.spring.annotation.MapperScan;

@SpringBootApplication
@MapperScan("com.itheima.mapper")
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}

```

4：用户POJO类

```java
package com.itheima.user.pojo;

import lombok.Data;
import tk.mybatis.mapper.annotation.KeySql;
import javax.persistence.Id;
import javax.persistence.Table;
import java.util.Date;

@Data
@Table(name = "tb_user")
public class User{
    // id
    @Id
    //开启主键自动回填
    @KeySql(useGeneratedKeys = true)
    private Long id;
    // 用户名
    private String userName;
    // 密码
    private String password;
    // 姓名
    private String name;
    // 年龄
    private Integer age;
    // 性别，1男性，2女性
    private Integer sex;
    // 出生日期
    private Date birthday;
    // 创建时间
    private Date created;
    // 更新时间
    private Date updated;
    // 备注
    private String note;
}
```



5：编写测试代码（UserMapper，UserService，UserController）；

编写 user-service\src\main\java\com\itheima\mapper\UserMapper.java

```java
package com.itheima.mapper;

import com.itheima.pojo.User;
import tk.mybatis.mapper.common.Mapper;

public interface UserMapper extends Mapper<User> {
}
```

编写 user-service\src\main\java\com\itheima\service\UserService.java

```java
package com.itheima.service;

import com.itheima.mapper.UserMapper;
import com.itheima.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    /**
     * 根据主键查询用户
     * @param id 用户id
     * @return 用户
     */
    public User queryById(Long id){
        return userMapper.selectByPrimaryKey(id);
    }
}

```

添加一个对外查询的接口处理器
user-service\src\main\java\com\itheima\user\controller\UserController.java

```java
package com.itheima.controller;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    //todo:http://localhost:9091/user/8
    @GetMapping("/{id}")
    public User queryById(@PathVariable Long id){
        return userService.queryById(id);
    }
}

```

7：启动并测试 

启动 user-service 项目，访问接口：http://localhost:8081/user/8









#### 服务消费者

- 服务消费工程consumer：利用查询用户服务获取用户数据并输出到浏览器



需求：

访问http://localhost:8080/consumer/8 使用RestTemplate获取http://localhost:9091/user/8的数据

==与上面类似，这里不再赘述，需要注意的是，我们调用 user-service 的功能，因此不需要mybatis相关依赖了。==

实现步骤：

1：添加启动器依赖；

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

2：创建启动引导类（注册RestTemplate）和配置文件；

```java
package com.itheima.consumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class ConsumberApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate(){
         return new RestTemplate();
    }
}
```



3：编写测试代码（ConsumerController中使用restTemplate访问服务获取数据）

```java
package com.itheima.consumer.web;

import com.itheima.consumer.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    //todo http://localhost:8080/consumer/8
    @GetMapping("/{id}")
    public User getUser(@PathVariable("id")Long id){
        String url = "http://localhost:9091/user/8";
        return restTemplate.getForObject(url,User.class);
    }
}

```

4：测试

启动 consumer-demo 引导启动类；因为 consumer-demo 项目没有配置端口，那么默认就是8080

我们访问：http://localhost:8080/consumer/8



**小结**：

```xml
<!-- springCloud -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-dependencies</artifactId>
    <version>${spring-cloud.version}</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

通过 `scope` 的import可以继承 `spring-cloud-dependencies` 工程中的依赖。含义是：也就是说接下来的项目中，包括这些项目中应用的版本都是来自于`spring-cloud-dependencies`的这些版本信息。也就是在后续的应用spring cloud 组件的时候，不要写版本号。







####  思考问题



简单回顾一下，刚才我们写了什么：

- user-service：对外提供了查询用户的接口 

- consumer：通过RestTemplate访问 http://locahost:8081/user/{id} 接口，查询用户数据

  

存在什么问题？

- 在consumer中，我们把url地址硬编码到了代码中，不方便后期维护 

- consumer需要记忆user-service的地址，如果出现变更，可能得不到通知，地址将失效 

- consumer不清楚user-service的状态，服务宕机也不知道 

- user-service只有1台服务，不具备高可用性 

- 即便user-service形成集群，consumer还需自己实现负载均衡

  

其实上面说的问题，概括一下就是分布式服务必然要面临的问题

- 服务管理 

  如何自动注册和发现 

  如何实现状态监管 

- 如何实现动态路由 

  服务如何实现负载均衡 

- 服务如何解决容灾问题

  服务如何实现统一配置

以上的问题，都将在SpringCloud中得到答案。



















### 08、SpringCloud：Eureka的原理



**目标**：

说出Eureka的主要功能

<strong style="font-size:24px">**问题分析**：</strong>

在刚才的案例中，user-service对外提供服务，需要对外暴露自己的地址。而consumer-demo（调用者）需要记录服务提供者的地址。将来地址出现变更，还需要及时更新。这在服务较少的时候并不觉得有什么，但是在现在日益复杂的互联网环境，一个项目可能会拆分出十几，甚至几十个微服务。此时如果还人为管理地址，不仅开发困难，将来
测试、发布上线都会非常麻烦，这与DevOps的思想是背道而驰的。

DevOps的思想是系统可以通过一组过程、方法或系统；提高应用发布和运维的效率,降低管理成本。

> ==网约车==

这就好比是 网约车出现以前，人们出门叫车只能叫出租车。一些私家车想做出租却没有资格，被称为黑车。而很多
人想要约车，但是无奈出租车太少，不方便。私家车很多却不敢拦，而且满大街的车，谁知道哪个才是愿意载人的。
一个想要，一个愿意给，就是缺少引子，缺乏管理啊。
此时滴滴这样的网约车平台出现了，所有想载客的私家车全部到滴滴注册，记录你的车型（服务类型），身份信息
（联系方式）。这样提供服务的私家车，在滴滴那里都能找到，一目了然。
此时要叫车的人，只需要打开APP，输入你的目的地，选择车型（服务类型），滴滴自动安排一个符合需求的车到你
面前，为你服务，完美！





> ==Eureka做什么？==

Eureka就好比是滴滴，负责管理、记录服务提供者的信息。服务调用者无需自己寻找服务，而是把自己的需求告诉
Eureka，然后Eureka会把符合你需求的服务告诉你。
同时，服务提供方与Eureka之间通过 “心跳” 机制进行监控，当某个服务提供方出现问题，Eureka自然会把它从服务
列表中剔除。
这就实现了服务的自动注册、发现、状态监控。

**原理图**

Eureka的主要功能是进行服务管理，定期检查服务状态，返回服务地址列表。

![1560439174201](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1560439174201.png)&nbsp;





renewal：续约

- Eureka-Server：就是服务注册中心（可以是一个集群），对外暴露自己的地址。
- 提供者：启动后向Eureka注册自己信息（地址，服务名称等），并且定期进行服务续约
- 消费者：服务调用方，会定期去Eureka拉取服务列表，然后使用负载均衡算法选出一个服务进行调用。
- 心跳(续约)：提供者定期通过http方式向Eureka刷新自己的状态



















### 09、SpringCloud：Eureka服务端：注册中心

**目标**：

添加eureka对应依赖和编写引导类搭建eureka服务并可访问eureka服务界面

**分析**：

Eureka是服务注册中心，只做服务注册；自身并不提供服务也不消费服务。可以搭建web工程使用Eureka，可以使用Spring Boot方式搭建。

**搭建步骤：**

- 导入Eureka服务端的起步依赖
- 编写启动类：开启Eureka服务
- 编写配置文件：配置相应信息



==**核心代码：**==

- 起步依赖【父工程有spring-boot-starter-parent】

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.itheima</groupId>
		<artifactId>springcloud-demo</artifactId>
		<version>1.0-SNAPSHOT</version>
	</parent>
	<artifactId>eureka-server</artifactId>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<!-- Eureka服务端 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>
	</dependencies>

</project>
```



- 启动类

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
//开启eureka服务
@EnableEurekaServer
public class EurekaServerApplication {
	
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}

}

```



- 配置文件

```yaml
#端口
server:
  port: 10086
#服务id
spring:
  application:
    name: eureka-server
#配置eureka客户端
eureka:
  client:
    register-with-eureka: false  #不注册
    fetch-registry: false #不拉取
    service-url:
      defaultZone: http://localhost:10086/eureka/  #告诉现在的注册中心的地址是什么
```



访问地址： http://localhost:10086/













### 10、SpringCloud：Eureka客户端：服务注册

**目标**：

将user-service的服务注册到eureka并在consumer-demo中可以根据服务名称调用

**对象工程**

user-service

**概述：**

==注册服务，就是在服务上添加Eureka的客户端依赖，客户端代码会自动把服务注册到EurekaServer中。==

<strong style="font-size:24px">服务注册</strong>

在服务提供工程user-service上添加Eureka客户端依赖；自动将服务注册到EurekaServer服务地址列表。

- 添加依赖；

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

- 改造启动引导类；添加开启Eureka客户端发现的注解；

```java
@SpringBootApplication
@MapperScan("com.itheima.mapper")
@EnableDiscoveryClient // 开启Eureka客户端发现功能
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}

```

- 修改配置文件；设置Eureka 服务地址

```yml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  application:
    name: user-service
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springcloud?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
    username: root
    password: passw0rd
    driver-class-name: com.mysql.jdbc.Driver
# mybatis的配置
mybatis:
  mapper-locations: classpath:/mappers/*.xml
  type-aliases-package: com.itheima.pojo
  configuration:
    map-underscore-to-camel-case: true  #驼峰命名
#注册中心配置
eureka:
    client:
      service-url:
        defaultZone: http://127.0.0.1:10086/eureka
```

注意：
==这里我们添加了spring.application.name属性来指定应用名称，将来会作为服务的id使用。==

- 测试

重启 user-service 项目，访问Eureka监控页面

![1567605575959](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567605575959.png)



**小结**

@EnableDiscoveryClient 和 @EnableEurekaClient的区别

1：@EnableDiscoveryClient 可以注册到任何类型的注册中心，更加通用，比如未来是zookeeper

2：@EnableEurekaClient 只能注册到eureka服务中。

















### 11、SpringCloud：Eureka客户端：服务拉取



**目标**

完成eureka服务的拉取



<strong style="font-size:24px">服务发现</strong>

在服务消费工程consumer上添加Eureka客户端依赖；可以使用工具类根据服务名称获取对应的服务地址列表。

**步骤**：

分别给user-service和consumer都配置如下代码：

1：添加Eureka客户端依赖；

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2：添加启动引导类注解；

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@EnableDiscoveryClient
public class ConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

}

```

3：修改配置

```yml
server:
  port: 8080
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
spring:
  application:
    name: consumer
```

4：在consumer-demo中使用eureka来发现服务

```java
package com.itheima.controller;

import com.itheima.consumber.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    DiscoveryClient discoveryClient;


    //todo http://localhost:8080/consumber/8
    @GetMapping("/{id}")
    public User getUser(@PathVariable("id")Long id){
        //String url = "http://localhost:9091/user/8";
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        ServiceInstance serviceInstance = instances.get(0);
        String url = "http://"+serviceInstance.getHost()+":"+serviceInstance.getPort()+"/user/"+id;
        return restTemplate.getForObject(url,User.class);
    }
}

```

5：Debug跟踪运行

重启 consumer项目；然后再浏览器中再次访问 http://localhost:8080/consumer/8 ；







### 12、SpringCloud：Eureka服务端：高可用配置

**目标**：

可以启动两台eureka-server实例；在eureka管理界面看到两个实例

**分析**：

Eureka Server即服务的注册中心，在刚才的案例中，我们只有一个EurekaServer，事实上EurekaServer也可以是一个集群，形成高可用的Eureka中心。

多个Eureka Server之间也会互相注册为服务，当服务提供者注册到Eureka Server集群中的某个节点时，该节点会把服务的信息同步给集群中的每个节点，从而实现高可用集群。因此，无论客户端访问到Eureka Server集群中的任意一个节点，都可以获取到完整的服务列表信息。

![1567606046494](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606046494.png)

如果有三个Eureka，则每一个EurekaServer都需要注册到其它几个Eureka服务中，例如：有三个分别为10086、
10087、10088，则：
10086要注册到10087和10088上
10087要注册到10086和10088上 
10088要注册到10086和10087上 

![1572884794115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572884794115.png) 







==**配置高可用EurekaServer：【配置两台：10086和10087】**==

- 第一台：【自己端口为10086，Eureka注册中心端口为10087】
- 第二台：【自己端口为10087，Eureka注册中心端口为10086】

1）修改原来的EurekaServer配置；修改 eureka-server\src\main\resources\application.yml 如下：

```yml
server:
  port: 10086
spring:
  application:
    name: eureka-server
eureka:
  client:
    service-url:
      # eureka 服务地址，如果是集群的话；需要指定其它集群eureka地址,多个使用逗号分开
      defaultZone: ${defaultZone:http://127.0.0.1:10086/eureka}
    # 不注册自己 ，单机版是false,集群版本就是：true
    #register-with-eureka: false
    # 不拉取服务 单机版是false,集群版本就是：true
    #fetch-registry: false
```

所谓的高可用注册中心，其实就是把EurekaServer自己也作为一个服务，注册到其它EurekaServer上，==这样多个
EurekaServer之间就能互相发现对方==，从而形成==集群==。因此我们做了以下修改：

```properties
注意把register-with-eureka和fetch-registry修改为true或者注释掉
在上述配置文件中的${}表示在jvm启动时候若能找到对应port或者defaultZone参数则使用，若无则使用后面
的默认值
```



2)：另外一台在启动的时候可以指定端口port和defaultZone配置：

![1567606824340](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606824340.png)&nbsp;

修改原来的启动配置组件；在如下界面中的 VM options 中
设置 -DdefaultZone=http//:127.0.0.1:10087/eureka -Dserver.port=10086



复制一份并修改；在如下界面中的 VM options 中
设置 -Dport=10087 -DdefaultZone=http:127.0.0.1:10086/eureka

```properties
10086要注册到10087和10088上   
-Dport=10086 -DdefaultZone=http://127.0.0.1:10087/eureka,http://127.0.0.1:10088/eureka
10087要注册到10086和10088上   
-Dport=10087 -DdefaultZone=http://127.0.0.1:10086/eureka,http://127.0.0.1:10088/eureka
10088要注册到10086和10087上   
-Dport=10088 -DdefaultZone=http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```



![1572939458846](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572939458846.png) 





启动测试；同时启动三台eureka server

![1567606890875](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567606890875.png)



4：）客户端注册服务到集群

因为EurekaServer不止一个，因此 user-service 项目注册服务或者 consumer-demo 获取服务的时候，service-url参
数需要修改为如下：

```yml
eureka:
    client:
    	service-url: # EurekaServer地址,多个地址以','隔开
    		defaultZone: http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```

为了方便上课和后面内容的修改，在测试完上述配置后可以再次改回单个eureka server的方式。













### 13、SpringCloud：Eureka客户端：服务提供者的其他配置

**目标**：

配置eureka客户端user-service的注册、续约等配置项.





#### 服务注册更换IP

==当服务往注册中心注册时：==

服务提供者在启动时，会检测配置属性中的： eureka.client.register-with-erueka=true 参数是否正确，事实上默认就是true。如果值确实为true，则会向EurekaServer发起一个Rest请求，并携带自己的元数据信息，EurekaServer会把这些信息保存到一个双层Map结构中。
1：第一层Map的Key就是服务id，一般是配置中的 spring.application.name 属性
2：第二层Map的key是服务的实例id。一般host+ serviceId + port，例如： localhost:user-service:8081
3：值则是服务的实例对象，也就是说一个服务，可以同时启动多个不同实例，形成集群。
默认注册： host使用的是主机名或者localhost，如果想用ip进行注册，可以在 user-service 中添加配置如下：

```yml
eureka:
	instance:
    	ip-address: 127.0.0.1 # ip地址 固定上报一个IP地址给eureka server
    	prefer-ip-address: true # 更倾向于使用ip，而不是host名,
```



==结构图：==

![1569405096470](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1569405096470.png) 

实例id：  小map中的key



实例id的指定

```yaml
#配置注册中心的地址
eureka:
  instance:
    #修改实例id
    instance-id: ${eureka.instance.ip-address}:${server.port}
```









#### 客户端与服务端配置服务续约

**user-service 服务提供者或者服务消费者都可以配置**

```yml
#在注册服务完成以后，服务提供者会维持一个心跳（定时向EurekaServer发起Rest请求），告诉#EurekaServer：“我还活着”。这个我们称为服务的续约（renew）；
#有两个重要参数可以修改服务续约的行为；可以在 user-service 中添加如下配置
eureka:
	instance:
		lease-expiration-duration-in-seconds: 90 # 服务失效时间
		lease-renewal-interval-in-seconds: 30 #续约的间隔时间
```

> lease-renewal-interval-in-seconds：服务续约(renew)的间隔，默认为30秒
> lease-expiration-duration-in-seconds：服务失效时间，默认值90秒

也就是说，默认情况下每隔30秒服务会向注册中心发送一次心跳，证明自己还活着。如果超过90秒没有发送心跳，EurekaServer就会认为该服务宕机，会定时（eureka.server.eviction-interval-timer-in-ms设定的时间）从服务列表中移除，这两个值在生产环境不要修改，默认即可。



==核心配置==

```yaml
spring:
  application:
    name: user-service   #不能有大写字母，不能有下划线
    
#Eureka配置
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka/    
  instance:
    prefer-ip-address: true  #上报ip地址
    ip-address: 127.0.0.1  #指定ip地址
    lease-renewal-interval-in-seconds: 5  #30秒一次心跳 【生产环境不用改；测试可以改小】
    lease-expiration-duration-in-seconds: 90 #实例过期时间【和服务剔除有关】
    instance-id: ${eureka.instance.ip-address}:${server.port} #指定实例id

```













### 14、SpringCloud：Eureka客户端：服务消费者的其他配置



#### 获取服务列表

当服务消费者启动时，会检测 eureka.client.fetch-registry=true 参数的值，如果为true，则会从Eureka
Server服务的列表拉取只读备份，然后缓存在本地。并且 每隔30秒 会重新拉取并更新数据。可以在 consumer-demo项目中通过下面的参数来修改：

**作用对象：consumer**

```yml
eureka:
  instance:
    registry-fetch-interval-seconds: 3 #默认30秒拉取一次注册信息【生产环境不改；测试环境该小】
    instance-info-replication-interval-seconds: 5 # 30秒替换实例的信息【生产环境不改；测试环境该小】
```

![1572942244261](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572942244261.png) 



==核心配置==

```yaml
#端口
server:
  port: 8080

#四要素
spring:
  #指定服务id
  application:
    name: consumer
    
#Eureka配置
eureka:
  client:
    registry-fetch-interval-seconds: 3 #默认30秒拉取一次注册信息【生产环境不改；测试环境该小】
    instance-info-replication-interval-seconds: 5 # 30秒替换实例的信息【生产环境不改；测试环境该小】
    service-url:
      defaultZone: http://localhost:10086/eureka/    

```















### 15、SpringCloud：服务下线、失效剔除

**目标**：

配置eureka失效剔除和自我保护

**作用对象：eureka-server**

如下的配置都是在Eureka Server服务端进行：

<strong style="font-size:18px">服务下线</strong>

当服务进行正常关闭操作时，它会触发一个服务下线的REST请求给Eureka Server，告诉服务注册中心：“我要下线了”。服务中心接受到请求之后，将该服务置为下线状态。

<strong style="font-size:18px">失效剔除</strong>

有时我们的服务可能由于内存溢出或网络故障等原因使得服务不能正常的工作，而服务注册中心并未收到“服务下线”的请求。相对于服务提供者的“服务续约”操作，服务注册中心在启动时会创建一个定时任务，默认每隔一段时间（默认为60秒）将当前清单中超时（默认为90秒）没有续约的服务剔除，这个操作被称为失效剔除。可以通过 eureka.server.eviction-interval-timer-in-ms 参数对其进行修改，单位是毫秒。

<strong style="font-size:18px">自我保护</strong>
我们关停一个服务，很可能会在Eureka面板看到一条警告：

操作方式：

1：先正常的启动所有的服务。能够正常的提供服务和消费服务。

2：把user-service服务关停。模拟网络抖动或者故障造成服务下线。这个时候eureka-server会启动保护机制

模拟场景：

```ruby
# 查看所有java的运行进程
> jps
```

![1567686991577](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567686991577.png)

2：通过 `taskkill /PID 57640 /F`

![1572885736781](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885736781.png) 

日志信息如下：

```properties
 Running the evict task with compensationTime 0ms
```

> 摘抄自：https://blog.csdn.net/a897180673/article/details/88412130
>
> 看到中间的 红色的大写字母了么?
> 翻译一下:
>
> ```properties
> 注意,Eureka可能会错误的将已经下线的实例申明为上线,续订低于阈值因此实例不会过期,仅仅是为了安全起见
> ```
>
> 这句话比较拗口,但是大概的意思就是,即使微服务挂掉了,我Eureka也会说他没有挂掉,这个是为了安全考虑.
>
> WTF,为了啥安全? 怎么就安全了？既然微服务下线了,Eureka你直接说微服务下线不就行了,微服务上线,你就请求路由到上线的微服务这不就行了?
>
> 为什么需要搞这一套了?
>
> 直觉告诉我事情肯定没有那么的简单,既然他弄了肯定会有他的道理
>
> 查了一些资料,看了一些介绍,大概的意思就是为了防止网络阻塞的问题
>
> 想象一个场景,以前在农村的小黑网吧里面,也就2m的网络 大概也就200kb/s的下载速度,却要给4-6台左右的电脑使用,带宽就比较的紧张了的,我以前在农村网吧上网的时候,老板就说,不要开迅雷下载啊,影响大家玩游戏
>
> 这个时候游戏服务器就相当于是Eureka,想一想,如果有人在网吧下载东西,导致其他的几个人网卡,玩游戏连接不了服务器,那么游戏服务器该怎么办呢?
>
> 立刻判断用户已经下线?
>
> 当然不是,比如最近的手机 吃鸡游戏 刺激战场,在网络卡的情况下是提醒再次等待连接,多次连接不行才让你下线重新登录.
> ![å¨è¿éæå¥å¾çæè¿°](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/20190314100610426.png)
>
> 所以Eureka也是这个道理.只不过他的保护模式有点过头了.
>
> 他为什么要这么保护么？
>
> 我们没有看源码只能猜测下:我觉得可能有这几种情况,一种是体验不好,如果网络环境糟糕,频繁上下线会很麻烦,在游戏中本身就存在很多的用户的上线和下线行为，游戏服务器必定会对此做一些优化，但是微服务是跑得应用怎么能随随便便的下线了，尽管对于EurekaServer而言，微服务是Client端，但是对于前端而言，他们都是Server，必须要稳定
>
> 第二个就是浪费时间,Tcp通信都还要3次握手,频繁的通信断开连接,时间大量的花费在沟通上,得不偿失.



这是触发了Eureka的自我保护机制。当服务未按时进行心跳续约时，Eureka会统计服务实例最近15分钟心跳续约的比例是否低于了85%。在生产环境下，因为网络延迟等原因，心跳失败实例的比例很有可能超标，但是此时就把服务剔除列表并不妥当，因为服务可能没有宕机。Eureka在这段时间内不会剔除任何服务实例，直到网络恢复正常。生产环境下这很有效，保证了大多数服务依然可用，不过也有可能获取到失败的服务实例，因此服务调用者必须做好服务的失败容错。



**问题1：85%是如何计算出来的** 85%活着的服务继续提供

这种85%是如何计算出来的呢？比如你有10个user-service节点注册到eureka-server中，这个时候挂了两台。那么比例就是：(10-2)/10 = 80% 。这个时候就会引发自我保护机制，15分钟内如果是挂一台当然就是90%是不会触发保护机制。

![1572885692715](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885692715.png) 



**问题2：自我保护机制的效果**

一旦进入到保护模式，所以的eureka服务都不再被踢出来。

> 一种是体验不好,如果网络环境糟糕,频繁上下线会很麻烦,在游戏中本身就存在很多的用户的上线和下线行为，游戏服务器必定会对此做一些优化，但是微服务是跑得应用怎么能随随便便的下线了，尽管对于EurekaServer而言，微服务是Client端，但是对于前端而言，他们都是Server，必须要稳定
>
> 第二个就是浪费时间,Tcp通信都还要3次握手,频繁的通信断开连接,时间大量的花费在沟通上,得不偿失.

**问题3：eureka为什么要有自我保护机制**

eureka的保护机制是指：在短时间内发现有15%的服务出现短暂的挂掉，那么在生产环境中是非常危险的，那么如果继续任eureka服务继续挂掉，剩下的85%某个服务列表 也会继续挂掉，会把某服务进行剔除。就会造成100%全部踢出完毕，那么在eureka中就没有这个服务地址信息，那么消息方就没有办法在消费服务。这个时候就会启动服务保护机制来防止短暂的网络问题而造成服务无法续约的问题，而被误解以为是服务器挂了问题。这种误解是非常可怕，如果没有保护机制那么就被误认为服务挂了这种是很片面的。保护机制是一种很好很有必要的机制，当然你也可以关闭这种保护机制。如下：

```yml
eureka:
  server:
    # 服务失效剔除时间间隔，默认60秒 eureka服务定时扫描失效的服务，把失效的服务剔除
    eviction-interval-timer-in-ms: 60000
    # 关闭自我保护模式（默认是打开的）
    enable-self-preservation: false
```

一句话：eureka在短时间内发现15%的服务迅速失败，但是服务是可用的，根据Eureka失效剔除，Eureka有可能把剩下的85%继续踢出，（其实可能是一种误解）。那么整个系统无法继续运作。这个时候才会启动Eureka的自我保护机制，就会把85%保护起来。





**1）消费者**

```yaml
# 服务端口
server:
  port: 8080
spring:
  application:
    name: consumer   # serviceID :  不能大写；不能有下划线
#注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
    registry-fetch-interval-seconds: 3 # 去注册中心拉取注册信息，默认值30秒一次，生产环境不改，测试环境改小
    instance-info-replication-interval-seconds: 3 #替换本地的信息
    initial-instance-info-replication-interval-seconds: 4 #替换初始化信息
```



**2）提供者**

```yaml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  application:
    name: user-service   # serviceID :  不能大写；不能有下划线
#注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
  instance:
    prefer-ip-address: true  # 上报ip地址
    ip-address: 127.0.0.1 # 告诉ip地址
    #instance-id: aaaaa  #实例id
    lease-renewal-interval-in-seconds: 3 # 生产环境不会改；测试环境改小；默认值是30
    lease-expiration-duration-in-seconds: 9 #剔除失效的实例；默认值90秒，生产环境不要改

```





**3）注册中心**

```yaml
server:
  port: 10086
spring:
  application:
    name: eureka-server
eureka:
  server:
    # 服务失效剔除时间间隔，默认60秒 eureka服务定时扫描失效的服务，把失效的服务剔除
    eviction-interval-timer-in-ms: 60000
    # 关闭自我保护模式（默认是打开的）
    enable-self-preservation: false
  client:
    service-url:
      defaultZone : http://localhost:10087/eureka

```









三步走：

1）启动器依赖   pom.xml

2）开启启动器    XXXXApplication启动类中

3）配置			    application.yml



















### 16、SpringCloud：Ribbon负载均衡





**目标**：

描述负载均衡和ribbon的作用

配置启动两个用户服务，在consumer中使用服务名实现根据用户id获取用户



**分析**：

负载均衡是一个算法，可以通过该算法实现从地址列表中获取一个地址进行服务调用。

在Spring Cloud中提供了负载均衡器：Ribbon

在刚才的案例中，我们启动了一个 user-service ，然后通过DiscoveryClient来获取服务实例信息，然后获取ip和端口来访问。

![1572885961221](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885961221.png)

但是实际环境中，往往会开启很多个 user-service 的集群。此时获取的服务列表中就会有多个，到底该访问哪一个呢？一般这种情况下就需要编写负载均衡算法，在多个实例列表中进行选择。不过Eureka中已经集成了负载均衡组件：Ribbon，简单修改代码即可使用。

![1572885983333](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572885983333.png)

没错就是Ribbon来做这个事情。

![1572886002499](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572886002499.png)



需求：可以使用RestTemplate访问http://user-service/user/8获取服务数据。

可以使用Ribbon负载均衡：在执行RestTemplate发送服务地址请求的时候，使用负载均衡拦截器拦截，根据服务名获取服务地址列表，使用Ribbon负载均衡算法从服务地址列表中选择一个服务地址，访问该地址获取服务数据。







Eureka客户端已经内置了Ribbon负载均衡策略，所以不用再导入依赖

![1572944657198](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944657198.png) 







![1572944993672](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944993672.png) 







实现步骤：



<strong style="font-size:24px">1：启动多个user-service实例（8082,8083）；</strong>

也可以在eureka-server中查看到user-service服务已被注册。

![1572944222966](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572944222966.png) 



<strong style="font-size:24px">2：修改RestTemplate实例化方法，添加负载均衡注解；</strong>

因为Eureka中已经集成了Ribbon，所以我们无需引入新的依赖。
直接修改 consumer-demo\src\main\java\com\itheima\consumer\ConsumerApplication.java
在RestTemplate的配置方法上添加 @LoadBalanced 注解：

<strong style="color:red;">Ribbon默认的负载均衡策略是轮询。</strong>

```java
@SpringBootApplication
@EnableDiscoveryClient
public class ConsumberApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumberApplication.class, args);
    }

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

}

```

<strong style="font-size:24px">3：修改ConsumerController；</strong>

修改 consumer-demo\src\main\java\com\itheima\controller\ConsumerController.java 调用方
式，不再手动获取ip和端口，而是直接通过服务名称调用；

```java
 //todo http://localhost:8080/consumber/8
 @GetMapping("/{id}")
 public User getUser(@PathVariable("id")Long id){
 	String url = "http://user-service/user/8";
 	return restTemplate.getForObject(url,User.class);
 }
```

访问页面，查看结果；并可以在8081和8082的控制台查看执行情况：
了解：Ribbon默认的负载均衡策略是轮询。SpringBoot也帮提供了修改负载均衡规则的配置入口在consumer的配置文件中添加如下，就变成随机的了：

```yml
user-service:
	ribbon:
		NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

==格式是： {服务名称}.ribbon.NFLoadBalancerRuleClassName==







其他的策略

| **内置负载均衡规则类**    | **规则描述**                                                 |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：<br>（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。注意：可以通过修改配置loadbalancer.<clientName>.connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。<br>（2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上线，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| ZoneAvoidanceRule         | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。 |
| BestAvailableRule         | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| Retry                     | 重试机制的选择逻辑                                           |



重启服务生效。通过下面的源码追踪才看效果。



<strong style="font-size:24px">5：源码追踪</strong>

为什么只输入了service名称就可以访问了呢？之前还要获取ip和端口。显然是有组件根据service名称，获取到了服务实例的ip和端口。因为 consumer-demo 使用的是RestTemplate，spring的负载均衡自动配置类LoadBalancerAutoConfiguration.LoadBalancerInterceptorConfig 会自动配置负载均衡拦截器（在spring-cloud-commons-**.jar包中的spring.factories中定义的自动配置类）， 

它就是LoadBalancerInterceptor ，这个类会在对RestTemplate的请求进行拦截，然后从Eureka根据服务id获取服务列表，随后利用负载均衡算法得到真实的服务地址信息，替换服务id。
我们进行源码跟踪：

![1572908354313](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908354313.png) 



![1567651684455](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651684455.png)

继续跟入execute方法：发现获取了8082端口的服务

![1567651668799](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651668799.png)

再跟下一次，发现获取的是8081、8082之间切换：

![1567651739714](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567651739714.png)

多次访问 consumer 的请求地址；然后跟进代码，发现其果然实现了负载均衡。并且在getServer中找到了配置的随机策略。

![1567661798680](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567661798680.png)

<strong style="font-size:24px">6：测试</strong>

在实例化RestTemplate的时候使用@LoadBalanced，服务地址直接可以使用服务名。





**小结**：

Ribbon提供了轮询、随机两种负载均衡算法（默认是轮询）可以实现从地址列表中使用负载均衡算法获取地址进行服务调用。

关键类：

1：LoadBalancerInterceptor --- > 2：找到intercept方法 ----> 3：RibbonLoadBalancerClient-的execute()方法----->找到加载负载均衡器和具体的服务。

















### 17、SpringCloud：熔断器Hystrix简介

**目标**：了解熔断器Hystrix的作用

**概述**

Hystrix 在英文里面的意思是 豪猪，它的logo 看下面的图是一头豪猪，它在微服务系统中是一款提供保护机制的组件，和eureka一样也是由netflix公司开发。

主页：https://github.com/Netflix/Hystrix/

![1567664082422](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664082422.png)

那么Hystrix的作用是什么呢？具体要保护什么呢？
Hystrix是Netflix开源的一个延迟和容错库，用于隔离访问远程服务、第三方库，防止出现级联失败。



<strong style="font-size:24px"> 雪崩问题</strong>

微服务中，服务间调用关系错综复杂，一个请求，可能需要调用多个微服务接口才能实现，会形成非常复杂的调用链路

如图：



![1567664157448](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664157448.png)&nbsp;



如图，一次业务请求，需要调用A、P、H、I四个服务，这四个服务又可能调用其它服务。
如果此时，某个服务出现异常：

![1567664228865](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664228865.png)

例如： 微服务I 发生异常，请求阻塞，用户请求就不会得到响应，则tomcat的这个线程不会释放，于是越来越多的
用户请求到来，越来越多的线程会阻塞：

![1567664256022](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664256022.png)

服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用，形成雪崩效应。这就好比，一个汽车生产线，生产不同的汽车，需要使用不同的零件，如果某个零件因为种种原因无法使用，那么就会造成整台车无法装配，陷入等待零件的状态，直到零件到位，才能继续组装。  此时如果有很多个车型都需要这个零件，那么整个工厂都将陷入等待的状态，导致所有生产都陷入瘫痪。一个零件的波及范围不断扩大。

Hystrix解决雪崩问题的手段主要是服务降级，包括：

> 线程隔离
>
> 服务降级
>
> 服务熔断 



















### 18、SpringCloud：线程隔离&服务降级

**目标**：

了解什么是线程隔离和服务降级

**线程隔离示意图：**

![1567664783655](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567664783655.png) 

**分析**：

Hystrix解决雪崩效应：

1：线程隔离：Hystrix为每个依赖服务调用分配一个小的线程池，如果线程池已满调用将被立即拒绝，默认不采用排队，加速失败判定时间。用户的请求将不再直接访问服务，而是通过线程池中的空闲线程来访问服务，如果线程池已满，或者请求超时，则会进行降级处理，什么是服务降级？

2：服务降级：优先保证核心服务，而非核心服务不可用或弱可用。：

用户的请求故障时，不会被阻塞，更不会无休止的等待或者看到系统崩溃，至少可以看到一个执行结果（例如返回友好的提示信息） 。服务降级虽然会导致请求失败，但是不会导致阻塞，而且最多会影响这个依赖服务对应的线程池中的资源，对其它服务没有响应。



触发Hystrix服务降级的情况：

> 线程池已满
> 请求超时



**步骤**

1：在消费方加入hystrix起步依赖

2：在启动类加入注解

3：在需要隔离得方法上进行配置



<strong style="font-size:24px">添加依赖</strong>

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

<strong style="font-size:24px">开启熔断</strong>

在启动类 ConsumerApplication 上添加注解：@EnableCircuitBreaker

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker
@RibbonClient(name = "SPRING-MICROSERVICECLOUD-RIBBON",configuration= MySelfRule.class)
public class ConsumberApplication{}
```

可以看到，我们类上的注解越来越多，在微服务中，经常会引入上面的三个注解，于是Spring就提供了一个组合注解：@SpringCloudApplication,自定义注解如下：

```java
package com.itheima.consumber.core;

import com.itheima.consumber.ribbon.MySelfRule;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

import java.lang.annotation.*;


@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker  // 等价于  @EnableHystrix
@RibbonClient(name = "SPRING-MICROSERVICECLOUD-RIBBON",configuration= MySelfRule.class)
public @interface SpringCloudApplication {
}

```

因此，我们可以使用这个组合注解来代替之前的4个注解，优化如下：

```java
@SpringCloudApplication
public class ConsumberApplication {
}
```

==@EnableCircuitBreaker 等价于  @EnableHystrix==



<strong style="font-size:24px">降级逻辑</strong>

当目标服务的调用出现故障，我们希望快速失败，给用户一个友好提示。因此需要提前编写好失败时的降级处理逻
辑，要使用HystrixCommand来完成。
改造 consumer-demo\src\main\java\com\itheima\consumer\controller\ConsumerController.java 处理器
类，如下： 

```java
package com.itheima.consumber.web;

import com.itheima.consumber.pojo.User;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.client.loadbalancer.LoadBalancerInterceptor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@Slf4j
@RequestMapping("/consumber")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;


    //todo http://localhost:8080/consumber/8
    @GetMapping("{id}")
    @HystrixCommand(fallbackMethod = "queryByIdFallBack")
    public String getUser(@PathVariable("id")Long id){
        String url = "http://user-service/user/8";
        return restTemplate.getForObject(url,String.class);
    }

    public String queryByIdFallBack(Long id){
        System.out.println("查询用户失败: id = " + id);
        return "对不起网络太拥挤，请稍后再试...";
    }
}

```

> ==要注意；因为熔断的降级逻辑方法必须跟正常逻辑方法保证：相同的参数列表和返回值声明。失败逻辑中返回User对象没有太大意义，一般会返回友好提示。所以把queryById的方法改造为返回String，反正也是Json数据。这样失败逻辑中返回一个错误说明，会比较方便。==

说明：

@HystrixCommand(fallbackMethod = "queryByIdFallBack")：用来声明一个降级逻辑的方法

测试：
当 user-service 正常提供服务时，访问与以前一致。但是当将 user-service 停机时，会发现页面返回了降级处理

![1572908478982](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908478982.png) 





<strong style="font-size:24px">修改超时配置</strong>

在之前的案例中，请求在超过1秒后都会返回错误信息，这是因为Hystrix的默认超时时长为1，我们可以通过配置修
改这个值；修改 consumer-demo\src\main\resources\application.yml 添加如下配置：

```yml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
```

这个配置会作用于全局所有方法。为了方便复制到yml配置文件中，可以复制

```properties
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000 
```

到yml文件中会自动格式化后再进行修改。



局部配置是通过：commandProperties属性配合@HystrixProperty来实现得。当然这样更精准。因为在实际得运维生产环境中，我们每个服务调用时间和执行实际情况都不一样。

```java
@GetMapping("{id}")@HystrixCommand(fallbackMethod = "queryByIdFallBack",commandProperties = {        @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value="2000")})
public String getUser(@PathVariable("id")Long id){
```

这些属性得名字都在`HystrixCommandProperties`类中查询获取。多个属性用逗号分开。

![1572908841233](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572908841233.png) 

```java
execution.isolation.thread.timeoutInMilliseconds 默认情况下是：1秒
```



**服务提供方模拟超时**

为了触发超时，可以在 user-service\src\main\java\com\itheima\user\service\UserService.java 的方法中随机秒来进行测试；来模拟网络网络超时：

```java
package com.itheima.controller;

import com.itheima.pojo.User;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Random;

@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    //todo:http://localhost:9091/user/8
    @GetMapping("/{id}")
    public User queryById(@PathVariable Long id){
        try {
            //模拟网络拥堵的情况，如果请求的时间没有超过hystrix的超时时间直接放行，如果超过就会启动熔断机制
            Thread.sleep(new Random().nextInt(10000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return userService.queryById(id);
    }
}

```

可以发现，请求的时长已经到了2s+，证明配置生效了。如果把修改时间修改到2秒以下，又可以正常访问

![1567666986156](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567666986156.png)

![1567667044523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567667044523.png)





<strong style="font-size:24px">默认的Fallback</strong>

刚才把fallback写在了某个业务方法上，如果这样的方法很多，那岂不是要写很多。所以可以把Fallback配置加在类上，实现默认fallback；再次改造 consumer-demo\src\main\java\com\itheima\consumer\controller\ConsumerController.java

```java
package com.itheima.consumer.web;

import com.itheima.consumer.pojo.User;
import com.netflix.hystrix.contrib.javanica.annotation.DefaultProperties;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.client.loadbalancer.LoadBalancerInterceptor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
@Slf4j
@RequestMapping("/consumber")
@DefaultProperties(defaultFallback = "defaultFallback")
public class UserController {

    @Autowired
    private RestTemplate restTemplate;

    //todo http://localhost:8080/consumber/8
    @GetMapping("{id}")
    @HystrixCommand
    public String getUser(@PathVariable("id")Long id){
        String url = "http://user-service/user/8";
        return restTemplate.getForObject(url,String.class);
    }

    public String defaultFallback(){
        return "默认提示：对不起，网络太拥挤了！";
    }

}

```

```java
@DefaultProperties(defaultFallback = "defaultFallBack")：在类上指明统一的失败降级方法；该类中所有方法
返回类型要与处理失败的方法的返回类型一致。
```

![1567668656183](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567668656183.png)

















### 19、SpringCloud：服务熔断原理

**目标**：了解熔断器工作原理

**概述** 

在服务熔断中，使用的熔断器，也叫断路器，其英文单词为：Circuit Breaker熔断机制与家里使用的电路熔断原理类似；当如果电路发生短路的时候能立刻熔断电路，避免发生灾难。在分布式系统中应用服务熔断后；服务调用方可以自己进行判断哪些服务反应慢或存在大量超时，可以针对这些服务进行主动熔断，防止整个系统被拖垮。
==Hystrix的服务熔断机制，可以实现弹性容错；当服务请求情况好转之后，可以自动重连。通过断路的方式，将后续请求直接拒绝，一段时间（默认5秒）之后允许部分请求通过，如果调用成功则回到断路器关闭状态，否则继续打开，拒绝请求的服务。==

Hystrix的熔断状态机模型：



![1573444713021](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444713021.png) 





![1573444732452](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444732452.png) 

状态机有3个状态：
1：Closed：关闭状态（断路器关闭），所有请求都正常访问。
2：Open：打开状态（断路器打开），所有请求都会被降级。Hystrix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断，断路器会完全打开。默认失败比例的阈值是50%，请求次数最少不低于20次。
3：Half Open：半开状态，不是永久的，断路器打开后会进入休眠时间（默认是5S）。随后断路器会自动进入半开状态。此时会释放部分请求通过，若这些请求都是健康的，则会关闭断路器，否则继续保持打开，再次进行休眠计时。



<strong style="font-size:24px">动手实践</strong>

为了能够精确控制请求的成功或失败，在 consumer 的处理器业务方法中加入一段逻辑；
修改 consumer-demo\src\main\java\com\itheima\controller\ConsumerController.java

```java
@GetMapping("{id}")
@HystrixCommand
public String getUser(@PathVariable("id")Long id){
    if(id == 1){
        throw new RuntimeException("太忙了");
    }
    String url = "http://user-service/user/"+id;
    return restTemplate.getForObject(url,String.class);
}
```

增加模拟出现异常。

这样如果参数是id为1，一定失败，其它情况都成功。（不要忘了清空user-service中的休眠逻辑）
我们准备两个请求窗口：
一个请求：http://localhost:8080/consumer/1，注定失败
一个请求：http://localhost:8080/consumer/2，肯定成功
==当我们疯狂访问id为1的请求时（超过20次） 超时的次数通过：requestVolumeThreshold=20修改，就会触发熔断。断路器会打开，一切请求都会被降级处理。==
此时你访问id为2的请求，会发现返回的也是失败，而且失败时间很短，只有20毫秒左右，可以通过 sleepWindowInMilliseconds进行修改配置；因进入半开状态之后2是可
以的。

![1567670138646](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1567670138646.png)

不过，默认的熔断触发要求较高，休眠时间窗较短，为了测试方便，我们可以通过配置修改熔断策略：



可以通过配置服务熔断参数修改默认：

```yml
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
      circuitBreaker:
        errorThresholdPercentage: 50 # 触发熔断错误比例阈值，默认值50% 也就是说requestVolumeThreshold 出现错误得次数超过5层就会激活熔断
        sleepWindowInMilliseconds: 10000 # 熔断后休眠时长，默认值5秒
        requestVolumeThreshold: 10 # 熔断触发最小请求次数，默认值是20
```

application的properties文件可以用下面的方式：

```properties
hystrix.command.default.circuitBreaker.requestVolumeThreshold=10
hystrix.command.default.circuitBreaker.sleepWindowInMilliseconds=10000
hystrix.command.default.circuitBreaker.errorThresholdPercentage=50
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000
```





**问题：**

为什么SpringClound进行远程服务调用时，实体类为什么不需要序列化了？HTTP协议是以什么格式传输数据呢？

> Http协议：Java对象  --->  Json字符串(String已经实现序列化) ---->  二进制 ----> 网络传输
>
> RPC协议：Java对象  -->  (pojo序列化)二进制 ----> 网络传输















### 20、课程总结





![1577785758268](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1577785758268.png) 





## 01、SpringCloud第一天回顾



1）服务架构的演变

```
1、集中式架构
2、垂直拆分
3、分布式服务
4、服务治理（SOA）
5、微服务
```



2）远程调用方式：RestTemplate使用

```
Spring提供了一个RestTemplate模板工具类，对基于Http的客户端进行了封装，并且实现了对象与json的序列化和反序列化，非常方便
```



3）SpringCloud的介绍

```
SpringCloud将现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。协调分布式环境中各个系统，为各类服务提供模板性配置。其主要 涉及的组件包括：

- Eureka：注册中心 
- Zuul/Gateway：服务网关 
- Ribbon：负载均衡
- Feign：服务调用
- Hystix：熔断器
- 统一配置中心
- 服务总线

Spring Cloud不是一个组件，而是许多组件的集合；它的版本命名比较特殊，是以A到Z的为首字母的一些单词（其实是伦敦地铁站的名字）组成。
```



4）Eureka注册中心

```
Eureka的主要功能是进行服务管理，定期检查服务状态，返回服务地址列表。
```



5）Ribbon负载均衡

```
在Spring Cloud中提供了负载均衡器：Ribbon

在执行RestTemplate发送服务地址请求的时候，使用负载均衡拦截器拦截，根据服务名获取服务地址列表，使用Ribbon负载均衡算法从服务地址列表中选择一个服务地址，访问该地址获取服务数据。
```



6）Hystrix熔断器

```
Hystrix 在英文里面的意思是 豪猪，它的logo 看下面的图是一头豪猪，它在微服务系统中是一款提供保护机制的组件，和eureka一样也是由netflix公司开发。
```

































##  02、SpringCloud第二天课程目标

1、能够使用Feign进行远程调用 

2、能够搭建Spring Cloud Gateway网关服务 

3、能够配置Spring Cloud Gateway路由过滤器 

4、能够编写Spring Cloud Gateway全局过滤器 

5、能够搭建Spring Cloud Config配置中心服务 

6、能够使用Spring Cloud Bus实时更新配置 

















## 03、SpringCloud：Feign：介绍与使用

在前面的学习中，使用了Ribbon的负载均衡功能，大大简化了远程调用时的代码： 

```java
// 定义服务实例访问URL
String url = "http://user-service/user/" + id;
return restTemplate.getForObject(url, String.class);
```

如果就学到这里，你可能以后需要编写类似的大量重复代码，格式基本相同，无非参数不一样。有没有更优雅的方式，来对这些代码再次优化呢？ 

这就是接下来要学的Feign的功能了。 

### 3.1 介绍

Feign也叫伪装： 

![1572166999223](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/001.png) 

Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。你不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。 

项目主页：https://github.com/OpenFeign/feign 

![1573006676151](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573006676151.png) 

### 3.2 使用【掌握】

#### 目标

> 使用Feign进行远程调用

#### 操作步骤

- 配置依赖

  - 在consumer中添加如下依赖：

    ```xml
    <!-- 配置Feign启动器 -->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
    ```

- 编写Feign的客户端

  - 在consumer中编写如下Feign客户端接口类：

    ```java
    package com.itheima.client;
    
    import com.itheima.pojo.User;
    import org.springframework.cloud.openfeign.FeignClient;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.PathVariable;
    
    @FeignClient("user-service")
    public interface UserClient {
    
        @GetMapping("/user/{id}")
        User findOne(@PathVariable("id") Long id);
    }
    ```

  - 首先这是一个接口，Feign会通过动态代理，帮我们生成实现类。这点跟mybatis的mapper很像。

  - @FeignClient注解，声明这是一个Feign客户端，同时通过value属性指定服务名称。

  - 接口中的方法，完全采用SpringMVC的注解，Feign会根据注解帮我们生成URL，并访问获取结果

  - @GetMapping中的/user，请不要忘记；因为Feign需要拼接可访问的地址。

- 编写控制器

  - 定义新的控制器类ConsumerFeignController，使用UserClient访问： 

    ```java
    package com.itheima.controller;
    
    import com.itheima.client.UserClient;
    import com.itheima.pojo.User;
    import lombok.extern.slf4j.Slf4j;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.PathVariable;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    @RequestMapping("/consumer")
    @Slf4j
    public class ConsumerFeignController {
        @Autowired(required = false)
        private UserClient userClient;
        
        @GetMapping("/{id}")
        public User findOne(@PathVariable("id") Long id){
            return userClient.findOne(id);
        }
    }
    ```

- 开启Feign的支持

  - 在ConsumerApplication启动类上，添加注解，开启Feign功能

    ```java
    package com.itheima;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.cloud.client.SpringCloudApplication;
    import org.springframework.cloud.client.loadbalancer.LoadBalanced;
    import org.springframework.cloud.openfeign.EnableFeignClients;
    import org.springframework.context.annotation.Bean;
    import org.springframework.web.client.RestTemplate;
    
    /** @SpringBootApplication
     @EnableDiscoveryClient
     @EnableCircuitBreaker */
    @SpringCloudApplication
    @EnableFeignClients // 开启Feign的支持
    public class ConsumerApplication {
        public static void main(String[] args){
            SpringApplication.run(ConsumerApplication.class, args);
        }
        @Bean
        @LoadBalanced
        public RestTemplate restTemplate(){
            return new RestTemplate();
        }
    }
    ```

    > 说明：Feign中已经自动集成了Ribbon负载均衡，因此不需要自己定义RestTemplate进行负载均衡的配置。 

- 启动测试

  - 访问接口：http://localhost:8080/consumer/2

    ![1573007125457](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007125457.png) 







## 04、SpringCloud：Feign：Ribbon的支持【了解】

- Feign中本身已经集成了Ribbon依赖和自动配置:

  ![1573006954818](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573006954818.png)

  因此我们不需要额外引入依赖，也不需要再注册`RestTemplate`对象。

- Fegin内置的Ribbon默认设置了请求超时时长，可以通过手动配置来修改这个超时时长：

  ```properties
  ribbon:
    ConnectTimeout: 2000 # 建立链接的超时时长(默认)
    ReadTimeout: 5000 # 读取响应数据超时时长(默认)
  ```

- Ribbon内部有重试机制，一旦超时，会自动重新发起请求。如果不希望重试，可以添加配置：

  ```properties
  ribbon:
    ConnectTimeout: 2000 # 建立连接超时时长
    ReadTimeout: 2000 # 读取响应数据超时时长
    MaxAutoRetries: 0 # 当前服务器的重试次数
    MaxAutoRetriesNextServer: 1 # 重试多少个服务节点
    OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试(只对查询)
  ```

  > 注意：Hystrix线程隔离的超时时间，应该比重试的总时间要大，比如当前案例中，应该配 大于或等于 3000*2 = 6000

  说明：com.netflix.client.config.DefaultClientConfigImpl.java类中的默认配置

  ![1573007200216](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007200216.png) 

  ![1573007240593](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007240593.png) 







## 05、SpringCloud：Feign：Hystrix的支持【了解】

- Feign默认也有对Hystrix的集成:

  ![1573007289622](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007289622.png)  

  > 说明：只不过，默认情况下是关闭的。

- 需要通过下面的参数来开启:  application.yml

  ```properties
  feign:
    hystrix:
      enabled: true # 开启Feign的熔断功能
  ```

  但是，Feign中的Fallback配置不像Ribbon中那样简单了。 

- 配置Feign中的Fallback

  - 定义UserClientFallback类，实现刚才编写的UserClient，作为fallback的处理类

    ```java
    package com.itheima.client;
    
    import com.itheima.client.UserClient;
    import com.itheima.pojo.User;
    import org.springframework.stereotype.Component;
    
    @Component
    public class UserClientFallback implements UserClient {
        @Override
        public User findOne(Long id) {
            User user = new User();
            user.setId(id);
            user.setName("用户异常");
            return user;
        }
    }
    ```

  - 然后在UserClient中，指定刚才编写的实现类

    ```java
    @FeignClient(value = "user-service", fallback = UserClientFallback.class)
    public interface UserClient {
        @GetMapping("/user/{id}")
        User findOne(@PathVariable("id") Long id);
    }
    ```

  - 重启测试

    关闭user-service服务，然后在页面访问：

    ![1573007348886](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007348886.png) 







## 06、SpringCloud：Feign：请求压缩&日志级别【了解】

#### 6.1 请求压缩

------

Spring Cloud Feign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗。通过下面的参数即可开启请求与响应的压缩功能(consumer中进行配置)：

```properties
feign:
  compression:
    request:
        enabled: true # 开启请求压缩
    response:
        enabled: true  # 开启响应压缩
```

#### 6.2 日志级别

------

前面讲过，通过 logging.level.xx=debug 来设置日志级别。然而这个对Fegin客户端而言不会产生效果。因为 @FeignClient 注解修改的客户端在被代理时，都会创建一个新的Fegin.Logger实例。我们需要额外指定这个日志的级别才可以。

- 在consumer的配置文件中设置com.itheima包下的日志级别都为debug

  ```properties
  logging:
    level:
      com.itheima: debug
  ```

- 在consumer编写配置类，定义日志级别

  ```java
  package com.itheima.config;
  
  import feign.Logger;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  
  @Configuration
  public class FeignConfig {
      @Bean
      public Logger.Level feignLoggerLevel(){
          // 记录所有请求和响应的明细，包括头信息、请求体、元数据
          return Logger.Level.FULL;
      }
  }
  ```

  这里指定的Level级别是FULL，Feign支持4种级别： 

  - NONE：不记录任何日志信息，这是默认值。 
  - BASIC：仅记录请求的方法，URL以及响应状态码和执行时间 
  - HEADERS：在BASIC的基础上，额外记录了请求和响应的头信息 
  - FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据。 

- 在consumer的UserClient中指定配置类

  ```java
  package com.itheima.client;
  
  import com.itheima.client.fallback.UserClientFallback;
  import com.itheima.config.FeignConfig;
  import com.itheima.pojo.User;
  import org.springframework.cloud.openfeign.FeignClient;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.PathVariable;
  
  @FeignClient(value = "user-service",
          fallback = UserClientFallback.class,
          configuration = FeignConfig.class)
  public interface UserClient {
  
      @GetMapping("/user/{id}")
      User findOne(@PathVariable("id") Long id);
  }
  ```

- 重启项目，即可看到每次访问的日志：

  ![1573009332972](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573009332972.png) 

  

  

  

  

   

## 07、Spring Cloud Gateway：网关介绍

官网学习地址：<https://spring.io/projects/spring-cloud-gateway#learn>

> Spring Cloud Gateway简介

- Spring Cloud Gateway是Spring官方基于Spring 5.0、 Spring Boot 2.0、Project Reactor等技术开发的网关服务。 
- Spring Cloud Gateway基于Filter链提供网关基本功能：安全、监控／埋点、限流等。 
- Spring Cloud Gateway为微服务架构提供简单、有效且统一的API路由管理方式。 
- Spring Cloud Gateway是替代Netflflix Zuul的一套解决方案。 

Spring Cloud Gateway组件的核心是一系列的过滤器，通过这些过滤器可以将客户端发送的请求转发（路由）到对应的微服务。 Spring Cloud Gateway是加在整个微服务最前沿的防火墙和代理器，隐藏微服务结点IP端口信息，从而加强安全保护。Spring Cloud Gateway本身也是一个微服务，需要注册到Eureka服务注册中心。

Geteway网关的核心功能是：**过滤和路由**

> Geteway加入后的架构

![1573007783793](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573007783793.png) 

说明：不管是来自于客户端（PC或移动端）的请求，还是服务内部调用。一切对服务的请求都可经过网关，然后再由网关来实现鉴权、动态路由等等操作。Gateway就是我们服务的统一入口。 

> Gateway核心概念

- **路由（route）**路由信息的组成：由一个ID、一个目的URL、一组断言工厂、一组Filter组成。如果路由断言为真，说明请求URL和配置路由匹配。 
- **断言（Predicate）**Spring Cloud Gateway中的断言函数输入类型是Spring 5.0框架中的 ServerWebExchange。Spring Cloud Gateway的断言函数允许开发者去定义匹配来自于Http Request中的任何信息比如请求头和参数。 
- **过滤器（Filter）**一个标准的Spring WebFilter。 Spring Cloud Gateway中的Filter分为两种类型的Filter，分别是Gateway Filter和Global Filter。过滤器Filter将会对请求和响应进行修改处理。 











## 08、Spring Cloud Gateway：快速入门

#### 8.1 创建模块

- 填写基本信息

  ![1573009434598](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573009434598.png) 

- 添加依赖

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
           http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <parent>
          <artifactId>springcloud-demo</artifactId>
          <groupId>com.itheima</groupId>
          <version>1.0-SNAPSHOT</version>
      </parent>
      <modelVersion>4.0.0</modelVersion>
      <artifactId>gateway-server</artifactId>
  
      <dependencies>
          <!-- 配置gateway启动器 -->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-gateway</artifactId>
          </dependency>
      </dependencies>
  </project>
  ```

#### 8.2 编写启动类

在gateway-server中创建com.itheima.GatewayApplication启动类

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
public class GatewayApplication {
    public static void main(String[] args){
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

#### 8.3 编写配置

在gateway-server中创建application.yml文件，内容如下：

```properties
server:
  port: 10010
spring:
  application:
    name: api-gateway
```

#### 8.4 编写路由规则

- 启动三个Spring Boot应用:

  ![1573009769268](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573009769268.png) 

- 需要用网关来代理user-service服务，先看一下控制面板中的服务状态:

  ![1573009946136](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573009946136.png) 

  - ip为：127.0.0.1 
  - 端口为：8082 

- 修改gateway-server的application.yml文件为:

  ```properties
  server:
    port: 10010
  spring:
    application:
      name: api-gateway
    cloud:
      gateway:
        routes:
          # 路由id,可以随意写
          - id: user-service-route
            # 代理的服务地址
            uri: http://127.0.0.1:8082
            # 路由断言，可以配置映射路径
            predicates:
              - Path=/user/**
  ```

  - 将符合 Path 规则的一切请求，都代理到 uri 参数指定的地址 
  - 本例中，我们将路径中包含有 /user/** 开头的请求，代理到http://127.0.0.1:9001 

#### 8.5 启动测试

访问的路径中需要加上配置规则的映射路径，我们访问：http://localhost:10010/user/1

![1573022239096](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573022239096.png) 



















 

## 09、Spring Cloud Gateway：面向服务的路由

在刚才的路由规则中，把路径对应的服务地址写死了！如果同一服务有多个实例的话，这样做显然不合理。 应该根据服务的名称，去Eureka注册中心查找服务对应的所有实例列表，然后进行动态路由！ 



- 先把当前服务注册到Eureka中，三步走：添加起步依赖、引导类中开启使用、配置文件中加入配置

1、加入起步依赖

```xml
<!-- 配置eureka客户端启动器 -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2、引导类中开启使用

```java
@EnableDiscoveryClient //开启服务发现
```

3、配置文件中加入配置

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka/,http://localhost:10087/eureka/
```





- 修改映射配置，通过服务名称获取

  因为已经配置了Eureka客户端，可以从Eureka获取服务的地址信息。修改application.yml文件： 

  ```properties
  server:
    port: 10010
  spring:
    application:
      name: api-gateway
    cloud:
      gateway:
        routes:
          # 路由id,可以随意写
          - id: user-service-route
            # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
            uri: lb://user-service
            # 路由断言，可以配置映射路径
            predicates:
              - Path=/user/**
  
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
  ```

  > 路由配置中uri所用的协议为lb时（以uri: lb://user-service为例），gateway将使用 LoadBalancerClient把user-service通过eureka解析为实际的主机和端口，并进行ribbon负载均衡。

  ![1573023051723](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573023051723.png) 

- 启动测试

  再次启动，这次gateway进行代理时，会利用Ribbon进行负载均衡访问： 

  日志中可以看到使用了负载均衡器：

  ![1573023163846](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573023163846.png) 

  

  

  

   

## 10、Spring Cloud Gateway：路由前缀

#### 10.1 添加前缀

在gateway中可以通过配置路由的过滤器PrefixPath，实现映射路径中的地址添加前缀，修改application.yml文件：

```properties
server:
  port: 10010
spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      routes:
        # 路由id,可以随意写
        - id: user-service-route
          # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://user-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/**
          filters:
            # 添加请求路径的前缀
            - PrefixPath=/user

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka,http://localhost:8762/eureka
```

通过 **PrefixPath=/xxx** 来指定了路由要添加的前缀。

- PrefixPath=/user http://localhost:10010/1 => http://localhost:9001/user/1 

- PrefixPath=/user/abc http://localhost:10010/1 => http://localhost:9001/user/abc/1

  以此类推。

  ![1573023784710](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573023784710.png) 

#### 10.2 去除前缀

在gateway中可以通过配置路由的过滤器StripPrefix，实现映射路径中的地址去除前缀，修改application.yml文件：

```properties
server:
  port: 10010
spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      routes:
        # 路由id,可以随意写
        - id: user-service-route
          # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://user-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/api/user/**
          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
            - StripPrefix=1

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka,http://localhost:8762/eureka
```

通过 StripPrefix=1 来指定了路由要去掉的前缀个数。如：路径 /api/user/1 将会被代理到 /user/1。 

- StripPrefix=1 http://localhost:10010/api/user/1 => http://localhost:9001/user/1
- StripPrefix=2 http://localhost:10010/api/user/1 => http://localhost:9001/1

 以此类推。

![1573024121960](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573024121960.png) 











## 11、Spring Cloud Gateway：过滤器介绍

> 过滤器简介

Gateway作为网关的其中一个重要功能，就是实现请求的鉴权。而这个动作往往是通过网关提供的过滤器来实现的。前面的 路由前缀 章节中的功能也是使用过滤器实现的。

- Gateway自带过滤器有几十个，常见自带过滤器有：

  | 过滤器名称           | 说明                         |
  | :------------------- | :--------------------------- |
  | AddRequestHeader     | 对匹配上的请求添加Header     |
  | AddRequestParameters | 对匹配上的请求添加参数       |
  | AddResponseHeader    | 对从网关返回的响应添加Header |
  | StripPrefix          | 对匹配上的请求路径去除前缀   |

  ![1573024240670](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573024240670.png) 

  详细的说明在: <a href='https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.0.RC1/reference/html/#gatewayfilter-factories'>官网链接</a>

- 过滤器类型: Gateway有两种类型的过滤器

  - 全局过滤器：自定义全局过滤器，不需要在配置文件中配置，作用在所有的路由上，实现 GlobalFilter 接口即可。
  - 局部过滤器：通过spring.cloud.routes.fifilters 配置在具体路由下，只作用在当前路由上；自带的过滤器都可以配置或者自定义按照自带过滤器的方式。 

> 过滤器执行生命周期

Spring Cloud Gateway 的 Filter 的生命周期也类似有两个：“pre” 和 “post”。“pre”和 “post” 分别会在请求被执行前调用和被执行后调用。

![1573024303043](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573024303043.png) 

这里的 pre 和 post 可以通过过滤器的 GatewayFilterChain 执行fifilter方法前后来实现。 

> 过滤器使用场景

场景非常多(例举):

- 请求鉴权: 一般 GatewayFilterChain 执行fifilter方法前，如果发现没有访问权限，直接就返回空。 
- 异常处理: 一般 GatewayFilterChain 执行fifilter方法后，记录异常并返回。 
- 服务调用时长统计: GatewayFilterChain 执行fifilter方法前后根据时间统计。 







## 12、Spring Cloud Gateway：全局默认过滤器

我们也可以将Spring Cloud Gateway自带的过滤器配置成: 不只是针对某个路由；而是可以对所有路由生效，也就是配置默认过滤器。

![1573098719510](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573098719510.png) 

```properties
server:
  port: 10010
spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      # 默认过滤器，对所有路由生效
      default-filters:
        # 添加响应头过滤器，添中一个响应头为name，值为admin
        - AddResponseHeader=name,admin
      routes:
        # 路由id,可以随意写
        - id: user-service-route
          # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://user-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/api/user/**
          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
            - StripPrefix=1

eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
```

运行测试:

![1573024603518](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573024603518.png) 













## 13、Spring Cloud Gateway：自定义过滤器

#### 13.1 自定义局部过滤器【了解】

> 需求：在application.yml中对某个路由配置过滤器，该过滤器可以在控制台输出配置文件中指定名称的请求参数的值。 

- 在gateway-server模块中编写过滤器工厂类MyParamGatewayFilterFactory

  ```java
  package com.itheima.filter;
  
  import org.springframework.cloud.gateway.filter.GatewayFilter;
  import org.springframework.cloud.gateway.filter.factory.AbstractGatewayFilterFactory;
  import org.springframework.http.server.reactive.ServerHttpRequest;
  import org.springframework.stereotype.Component;
  
  import java.util.Arrays;
  import java.util.List;
  
  /** 自定义局部过滤器 */
  @Component
  public class MyParamGatewayFilterFactory extends
          AbstractGatewayFilterFactory<MyParamGatewayFilterFactory.Config> {
  
      private static final String PARAM_KEY = "param";
  
      /** 定义构造器(必须) */
      public MyParamGatewayFilterFactory(){
          super(Config.class);
      }
  
      /** 接收过滤器传进来的字段集合(可选) */
      @Override
      public List<String> shortcutFieldOrder() {
          return Arrays.asList(PARAM_KEY);
      }
  
      /** 重写拦截方法(必须) */
      @Override
      public GatewayFilter apply(Config config) {
          return (exchange, chain) -> {
              // 获取请求对象
              ServerHttpRequest request = exchange.getRequest();
              // 获取请求参数
              if (request.getQueryParams().containsKey(config.param)){
                  request.getQueryParams().get(config.param).forEach(value -> {
                      System.out.println(config.param + " = " + value);
                  });
              }
              // 放行
              return chain.filter(exchange);
          };
      }
  
      /** 定义配置类，接收配置文件中的属性(必须) */
      public static class Config {
          private String param;
          public String getParam() {
              return param;
          }
          public void setParam(String param) {
              this.param = param;
          }
      }
  }
  ```

- 在gateway-server模块中修改application.yml配置文件

  ```properties
  server:
    port: 10010
  spring:
    application:
      name: api-gateway
    cloud:
      gateway:
        # 默认过滤器，对所有路由生效
        default-filters:
          # 添加响应头过滤器，添加一个响应头为name，值为admin
          - AddResponseHeader=name,admin
        routes:
          # 路由id,可以随意写
          - id: user-service-route
            # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
            uri: lb://user-service
            # 路由断言，可以配置映射路径
            predicates:
              - Path=/api/user/**
            filters:
              # 表示过滤1个路径，2表示两个路径，以此类推
              - StripPrefix=1
              # 自定义过滤器
              - MyParam=name
  
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:8761/eureka,http://localhost:8762/eureka
  ```

- 测试访问

  http://localhost:10010/api/user/1?name=admin 检查后台是否输出name和admin

  http://localhost:10010/api/user/1?name2=admin 则是不会输出的。

  

#### 13.2 自定义全局过滤器

> 需求：模拟一个登录的校验。基本逻辑：如果请求中有token参数，则认为请求有效，放行。 

- 在gateway-server模块中编写全局过滤器类MyGlobalFilter

```java
package com.itheima.filter;

import org.apache.commons.lang.StringUtils;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

/** 自定义全局过滤器 */
@Component
public class MyGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("==全局过滤器MyGlobalFilter==");
        String token = exchange.getRequest().getQueryParams().getFirst("token");
        if (StringUtils.isBlank(token)){
            // 设置响应状态码: 401 未授权
            exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
            // 返回响应完成
            return exchange.getResponse().setComplete();
        }
        // 放行
        return chain.filter(exchange);
    }
    @Override
    public int getOrder() {
        // 值越小越先执行
        return 1;
    }
}
```

- 测试访问

  - 访问 http://localhost:10010/api/user/1

    ![1573026283541](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573026283541.png) 

  - 访问 http://localhost:10010/api/user/1?token=admin

    ![1573026488210](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573026488210.png) 

    

    

    

    

    

    

     

## 14、Spring Cloud Gateway：负载均衡&熔断【了解】





### 1）集成Ribbon

Gateway中默认就已经集成了Ribbon负载均衡，我们添加配置即可。

```properties
# 负载均衡
ribbon:
  ConnectTimeout: 2000 # 建立连接超时时长
  ReadTimeout: 2000 # 读取响应数据超时时长
  MaxAutoRetries: 0 # 当前服务器的重试次数
  MaxAutoRetriesNextServer: 1 # 重试多少个服务节点
  OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试(只对查询)
```

![1573111015362](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573111015362.png) 



重试机制gateway没有开启，目的不会让网关阻塞。





### 2）集成Hystrix

Gateway默认是没有集成Hystrix的，官方说明如下：

![1577874408679](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1577874408679.png)



> 如果集成Hystrix我们需要进行如下的步骤：

 

- 第一步: 在gateway-server中，添加hystrix启动器

  ```xml
  <!-- 配置hystrix启动器 -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
  </dependency>
  ```

- 第二步: 配置线程隔离时间

  ```properties
  # 线程隔离
  hystrix:
    command:
      default:
        execution:
          isolation:
            thread:
              timeoutInMilliseconds: 1000
  ```

- 第三步: 在默认过滤器中配置Hystrix过滤器(对全部路由有效)

  ```properties
  spring:
    cloud:
      gateway:
        # 配置默认过滤器(对全部路由有效)
        default-filters:
        # 添加响应头，响应头的名称为name 值为admin
        - AddResponseHeader=name,admin
        - name: Hystrix  # 配置Hystrix过滤器
          args:          # 配置两个参数
            name: fallbackcmd
            fallbackUri: forward:/fallback
  ```

- 第四步: 创建FallbackController.java控制器，提供服务降级方法

  ```java
  package com.leyou.controller;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.RestController;
  
  @RestController
  public class FallbackController {
      @GetMapping("/fallback")
      public String fallback(){
          return "您好，服务器正忙，请稍候再试。。。";
      }
  }
  ```

- 第五步: 测试

  ![1575308843439](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1575308843439.png) 













## 15、Spring Cloud Gateway：高可用【了解】 

- 启动多个Gateway服务，自动注册到Eureka，形成集群。如果是服务内部访问，访问Gateway，自动负载均衡，没问题。 

- Gateway更多是外部访问，PC端、移动端等。它们无法通过Eureka进行负载均衡，那么该怎么办？

  此时，可以使用其它的服务网关，来对Gateway进行代理。比如：Nginx、Apache

  

  Eureka(注册中心)、Ribbon(负载均衡)、Hystrix(熔断)、Feign(客户端调用)、Gateway(网关)

  

  

- Gateway与Feign的区别

  - Gateway 作为整个应用的流量入口，接收所有的请求，如PC、移动端等，并且将不同的请求转发至不同的处理微服务模块，其作用可视为nginx；大部分情况下用作权限鉴定、服务端流量控制。
  - Feign 则是将当前微服务的部分服务接口暴露出来，并且主要用于各个微服务之间的服务调用。 





```properties
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
	
	upstream feifeiServer {
	 	#权重配置
		server localhost:10000 weight=5;  
		server localhost:10010;
	}
	
	server {
		listen		 80;
		server_name	 localhost;
		
		location /api {
            proxy_pass   http://feifeiServer;
        }
	}


}

```











## 16、Spring Cloud Config：配置中心介绍

在分布式系统中，由于服务数量非常多，配置文件分散在不同的微服务项目中，管理不方便。为了方便配置文件集中管理，需要分布式配置中心组件。在Spring Cloud中，提供了Spring Cloud Config，它支持配置文件放在配置服务的本地，也支持放在远程Git仓库（GitHub、码云）。

使用Spring Cloud Config配置中心后的架构如下图：

![1573027756995](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573027756995.png) 

官网学习文档：<https://cloud.spring.io/spring-cloud-static/spring-cloud-config/2.2.0.RC1/reference/html/> 

> 配置中心本质上也是一个微服务，同样需要注册到Eureka服务注册中心！ 



![1573113307416](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573113307416.png) 









## 17、Spring Cloud Config：Git配置管理

#### 17.1 远程Git仓库

- 知名的Git远程仓库有国外的GitHub和国内的码云（gitee）；但是使用GitHub时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况。如果希望体验更好一些，可以使用国内的Git托管服务——码云（gitee.com）。 
- 与GitHub相比，码云也提供免费的Git仓库。此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务。本章中使用的远程Git仓库是码云。 
- 码云访问地址：https://gitee.com/

#### 17.2 创建远程仓库

首先要使用码云上的私有远程git仓库需要先注册帐号；请先自行访问网站并注册帐号，然后使用帐号登录码云控制台并创建公开仓库。

![1573027982266](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573027982266.png) 

![1577874812254](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1577874812254.png) 

注意：==去掉上面的√，需要使用命令，从本地 把文件推送上去，如果勾上，则可以在该页面创建文件==





==**我们上课的时候是勾选的**==







#### 17.3 创建配置文件



在新建的仓库中创建需要被统一配置管理的配置文件。

**配置文件的命名方式：**

 {application}-{profile}.yml 或 {application}-{profile}.properties 

- application为应用名称 
- profile用于区分开发环境，测试环境、生产环境等

如user-dev.yml，表示用户微服务开发环境下使用的配置文件。这里将user-service工程的配置文件application.yml文件的内容复制作为user-dev.yml文件的内容，具体配置如下：

![1573028350283](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573028350283.png) 

![1573028547571](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573028547571.png) 

![1573028606334](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573028606334.png) 

创建完user-dev.yml配置文件之后，gitee中的仓库如下：

![1573028676294](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573028676294.png) 



==**细节：仓库中不要有application.yml或者application.yaml的文件**==

![1573113840392](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573113840392.png) 











## 18、Spring Cloud Config：搭建配置中心微服务

#### 18.1 创建模块

- 创建配置中心微服务模块

![1573028767093](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573028767093.png) 



- 添加依赖，修改pom.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
           http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <parent>
          <artifactId>springcloud-demo</artifactId>
          <groupId>cn.itcast</groupId>
          <version>1.0-SNAPSHOT</version>
      </parent>
      <modelVersion>4.0.0</modelVersion>
      <artifactId>config-server</artifactId>
      
      <dependencies>
          <!-- 配置eureka客户端 -->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
          </dependency>
          <!-- 配置config服务端 -->
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-config-server</artifactId>
          </dependency>
      </dependencies>
  </project>
  ```





#### 18.2 启动类

创建配置中心模块config-server启动类ConfigServerApplication.java:

```java
package com.itheima;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    
    public static void main(String[] args){
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

#### 18.3 配置文件

创建配置中心工程config-server的配置文件application.yml:

```properties
server:
  port: 12000
  
spring:
  application:
    name: config-server
  cloud:
    config:
      server:
        git:
          uri: https://gitee.com/cfei_net/config.git
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
```

> 注意上面的 spring.cloud.confifig.server.git.uri 则是在码云创建的仓库地址

#### 18.4 启动测试

启动eureka注册中心和配置中心，然后访问http://localhost:12000/user-dev.yml ，查看能否输出在码云存储管理的user-dev.yml文件。并且可以在gitee上修改user-dev.yml然后刷新上述测试地址也能及时到最新数据。

![1573029273142](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573029273142.png) 







## 19、Spring Cloud Config：获取配置中心配置

前面已经完成了配置中心微服务的搭建，下面我们就需要改造一下用户微服务user-service，配置文件信息不再由微服务项目提供，而是从配置中心获取。如下对user-service工程进行改造。

#### 19.1 添加依赖

在user-service模块中，添加如下依赖:

```xml
<!-- 配置config启动器  -->
<dependency>
  	<groupId>org.springframework.cloud</groupId>
  	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

#### 19.2 修改配置

- 删除user-service模块中的application.yml文件（因为该文件从配置中心获取）

  说明：暂保留application.yml 更名为 temp.yml

- 创建user-service模块bootstrap.yml配置文件 

  ```properties
  spring:
    cloud:
      config:
        # 与远程仓库中的配置文件的application保持一致
        name: user
        # 与远程仓库中的配置文件的profile保持一致
        profile: dev
        # 与远程仓库中的版本保持一致
        label: master
        discovery:
          # 启用配置中心
          enabled: true
          # 配置中心服务id
          service-id: config-server
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:10086/eureka/,http://localhost:10087/eureka/
  ```

user-service模块，修改后的结构:

![1573029766192](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573029766192.png) 

- bootstrap.yml文件也是Spring Boot的默认配置文件，而且其加载的时间相比于application.yml更早。 
  - application.yml和bootstrap.yml虽然都是Spring Boot的默认配置文件，但是定位却不相同。bootstrap.yml可以理解成系统级别的一些参数配置，这些参数一般是不会变动的。application.yml可以用来定义应用级别的参数，如果搭配 spring cloud config 使用，application.yml里面定义的文件可以实现动态替换。 
  - 总结就是：bootstrap.yml文件相当于项目启动时的引导文件，内容相对固定。application.yml文件是微服务的一些常规配置参数，变化比较频繁。

#### 19.3 启动测试

启动注册中心、配置中心、用户服务user-service，如果启动没有报错其实已经使用上配置中心内容，可以到注册中心查看，也可以检验user-service的服务。

![1573030303004](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030303004.png) 







还有第二种配置：直接指定配置中心的地址url

```yaml
#配置配置中心的地址
spring:
  cloud:
    config:
      # 与远程仓库中的配置文件的application保持一致
      name: user
      # 与远程仓库中的配置文件的profile保持一致
      profile: test
      # 与远程仓库中的版本保持一致
      label: master
	  #配置中心的uri
      uri: http://localhost:12000

```











## 20、Spring Cloud Bus：消息总线介绍

> 存在问题

- 前面已经完成了将微服务中的配置文件集中存储在远程Git仓库，并且通过配置中心微服务从Git仓库拉取配置文件，当用户微服务启动时会连接配置中心获取配置信息从而启动用户微服务。
- 如果我们更新Git仓库中的配置文件，那用户微服务是否可以及时接收到新的配置信息并更新呢？ 

> 测试是否更新Git仓库中的配置文件

- 修改在码云上的user-dev.yml文件，添加一个属性test.name

  ![1573030643704](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030643704.png) 

- 修改user-service工程中的UserController.java

  ```java
  package com.itheima.user.controller;
  
  import com.itheima.user.pojo.User;
  import com.itheima.user.service.UserService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.web.bind.annotation.GetMapping;
  import org.springframework.web.bind.annotation.PathVariable;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.RestController;
  
  @RestController
  @RequestMapping("/user")
  public class UserController {
  
      @Autowired
      private UserService userService;
      
      @Value("${test.name}")
      private String name;
  
      /** 根据主键id查询用户 */
      @GetMapping("/{id}")
      public User findOne(@PathVariable("id")Long id){
          System.out.println("配置文件中的test.name = " + name);
          return userService.findOne(id);
      }
  }
  ```

- 启动测试

  - 依次启动Eureka、配置中心微服务、用户微服务、然后修改Git仓库中的配置信息，访问用户微服务，查看输出内容。http://localhost:9001/user/1

    ![1573030643704](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030643704.png) 

    ![1573030681747](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030681747.png) 

  - 结论：通过查看用户微服务控制台的输出结果可以发现，我们对于Git仓库中配置文件的修改并没有及时更新到订单微服务，只有重启用户微服务才能生效。

    ![1573030759731](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030759731.png) 

  - 如果想在不重启微服务的情况下更新配置该如何实现呢? 可以使用Spring Cloud Bus来实现配置的自动更新。需要注意的是Spring Cloud Bus底层是基于RabbitMQ实现的，默认使用本地的消息队列服务，所以需要提前启动本地RabbitMQ服务。

> Spring Cloud Bus 消息总线介绍

Spring Cloud Bus是用轻量的消息代理将分布式的节点连接起来,可以用于广播配置文件的更改或者服务的监控管理。一个关键的思想就是,消息总线可以为微服务做监控,也可以实现应用程序之间相互通信。Spring Cloud Bus可选的消息代理有RabbitMQ和Kfaka。

使用了Bus之后: 

![1573030993005](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573030993005.png) 

![1573031047972](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031047972.png) 











## 21、Spring Cloud Bus：改造配置中心

- 在config-server模块的pom.xml文件中加入Spring Cloud Bus相关依赖

  ```xml
  <!-- 配置spring-cloud-bus -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-bus</artifactId>
  </dependency>
  <!-- 配置rabbit -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
  </dependency>
  ```

- 在config-server模块中修改application.yml文件

  ```properties
  server:
    port: 12000
  
  spring:
    application:
      name: config-server
    cloud:
      config:
        server:
          git:
            uri: https://gitee.com/cfei_net/config.git
  
    # rabbitmq的配置信息；如下配置的rabbit都是默认值，其实可以完全不配置
    rabbitmq:
      host: localhost
      port: 5672
      username: guest
      password: guest
  
  eureka:
    client:
      service-url:
        defaultZone: http://localhost:8761/eureka,http://localhost:8762/eureka
  
  management:
    endpoints:
      web:
        exposure:
          # 暴露触发消息总线的地址
          include: bus-refresh
  ```

- 重启配置中心，查看RabbitMQ管理员界面    ![1573031337485](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031337485.png)







## 22、Spring Cloud Bus：改造用户服务

### 1）相关配置

- 在user-service模块的pom.xml中加入Spring Cloud Bus相关依赖

  ```xml
  <!-- 配置spring-cloud-bus -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-bus</artifactId>
  </dependency>
  <!-- 配置rabbit -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-stream-binder-rabbit</artifactId>
  </dependency>
  <!-- 配置actuator启动器 -->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
  </dependency>
  ```

- 修改user-service模块的bootstrap.yml

  ```properties
  spring:
    cloud:
      config:
        # 与远程仓库中的配置文件的application保持一致
        name: user
        # 与远程仓库中的配置文件的profile保持一致
        profile: dev
        # 与远程仓库中的版本保持一致
        label: master
        discovery:
          # 启用配置中心
          enabled: true
          # 配置中心服务id
          service-id: config-server
    # rabbitmq的配置信息；如下配置的rabbit都是默认值，其实可以完全不配置
    rabbitmq:
      host: localhost
      port: 5672
      username: guest
      password: guest
  
  # 配置eureka
  eureka:
    client:
      service-url: # EurekaServer地址,多个地址以','隔开
        defaultZone: http://localhost:10086/eureka,http://localhost:10087/eureka
  ```





### 2）如果是固定的配置信息

如果是固定的配置信息，例如，我们动态修改数据源的链接ip，我们只需要修改完成后，提交一下请求即可。

使用Postman或者RESTClient工具发送POST方式，请求访问地址

http://127.0.0.1:12000/actuator/bus-refresh

可以看到数据库就切换过来了，而且是不需要重启的情况下完成的。





### 3）如果是自定义的配置信息


- 改造user-service模块的UserController.java

  ![1573031555949](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031555949.png)  

- 启动测试

  前面已经完成了配置中心微服务和用户微服务的改造，下面来测试一下，当我们修改了Git仓库中的配置文件，用户微服务是否能够在不重启的情况下自动更新配置信息。

  #### 测试步骤

  - 第一步：依次启动Eureka、配置中心微服务、用户微服务

  - 第二步：访问用户微服务查看输出结果

  - 第三步：修改Git仓库中配置文件内容

  - 第四步：使用Postman或者RESTClient工具发送POST方式，请求访问地址

    ​       http://127.0.0.1:12000/actuator/bus-refresh

    ![1573031767362](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031767362.png) 

  - 第五步：访问用户微服务系统控制台查看输出结果

    ![1573031837610](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031837610.png) 

  #### 说明

  - Postman或者Insomnia是一个可以模拟浏览器发送各种请求（POST、GET、PUT、DELETE等）的工具。
  - 请求地址http://127.0.0.1:12000/actuator/bus-refresh中 /actuator是固定的,/bus-refresh对应的是配置中心config-server中的application.yml文件的配置项include的内容。
  - 请求http://127.0.0.1:12000/actuator/bus-refresh地址的作用是访问配置中心的消息总线服务，消息总线服务接收到请求后会向消息队列中发送消息，订单微服务会监听消息队列。当订单微服务接收到队列中的消息后，会重新从配置中心获取最新的配置信息。







## 23、Spring Cloud：技术体系综合应用概览

![1573031949671](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573031949671.png) 

























## 24、课程总结

- Eureka： 服务注册中心
- Ribbon： 负载均衡器
- Hystrix： 熔断器

- Feign： 服务调用(调用微服务)
- Spring Cloud Gateway： 服务网关(路由到微服务)
- Spring Cloud Config： 配置中心(配置文件统一管理)
- Spring Cloud Bus： 消息总线(服务之间数据同步)















































