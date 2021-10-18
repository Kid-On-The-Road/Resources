# SpringCloud

## 服务架构的演变

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

## 远程调用

| 调用方式 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| RPC      | Remote Produce Call远程过程调用，**RPC基于Socket，工作在会话层。自定义数据格式**，速度快，效率高。早期的webservice，现在热门的dubbo，都是RPC的典型代表 |
| Http     | http其实是**一种网络传输协议，基于TCP，工作在应用层，规定了数据传输的格式**。现在客户端浏览器与服务端通信基本都是采用Http协议，也可以用来进行远程服务调用。缺点是消息封装臃肿，优势是对服务的提供和调用方没有任何技术限定，自由灵活，更符合微服务理念。 |

==**区别**==：RPC的机制是根据语言的API（language API）来定义的，而不是根据基于网络的应用来定义的。

![1572877711327](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877711327.png) 

![1572877756745](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572877756745.png) 



## 使用RestTemplate进行远程调用

### 依赖

```xml
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
```

```java
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
```

```java
/**
	 * Spring中的RestTemplate完成调用
	 * getForObject(Url，把返回值转成什么类型)
	 * @throws Exception
	 */
	@Test
	public void test() throws Exception {
		RestTemplate rt = new RestTemplate();
		Book book = rt.getForObject("http://localhost:8080/book/3", Book.class);
		System.out.println(book);
	}
```

## 介绍

现在非常流行的一些技术整合到一起，实现了诸如：配置管理，服务发现，智能路由，负载均衡，熔断器，控制总线，集群状态等等功能。协调分布式环境中各个系统，为各类服务提供模板性配置。其主要 涉及的组件包括：

- Eureka：注册中心 
- Zuul/Gateway：服务网关 
- Ribbon：负载均衡
- Feign：服务调用
- Hystix：熔断器
- 统一配置中心
- 服务总线

## 案例

### 父工程

springcloud-demo

#### 依赖

```xml
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
            <!-- springCloud 通过 `scope` 的import可以继承 `spring-cloud-dependencies` 工程中的依赖。含义是：也就是说接下来的项目中，包括这些项目中应用的版本都是来自于`spring-cloud-dependencies`的这些版本信息。也就是在后续的应用spring cloud 组件的时候，不要写版本号.-->
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
```

### 服务提供者

用户服务工程user-service：整合mybatis查询数据库中用户数据；提供查询用户服务

#### 依赖

```xml
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
```

#### 配置文件

```yml
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

#### 启动类

开启通用mapper注解

```java
@SpringBootApplication
@MapperScan("com.itheima.mapper")
public class UserApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserApplication.class, args);
    }
}
```

#### 用户POJO类

```java
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

#### 测试代码

- 编写 user-service\src\main\java\com\itheima\mapper\UserMapper.java

```java
public interface UserMapper extends Mapper<User> {
}
```

- 编写 user-service\src\main\java\com\itheima\service\UserService.java

```java
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

- 添加一个对外查询的接口处理器
  user-service\src\main\java\com\itheima\user\controller\UserController.java

```java
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

### 服务消费者

服务消费工程consumer：利用查询用户服务获取用户数据并输出到浏览器

#### 依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

#### 创建启动引导类

（注册RestTemplate）和配置文件

```java
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

#### 测试代码

```java
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

## 注册中心Eureka

### 介绍

负责管理、记录服务提供者的信息。服务调用者无需自己寻找服务，而是把自己的需求告诉
Eureka，然后Eureka会把符合你需求的服务告诉你。
同时，服务提供方与Eureka之间通过 “心跳” 机制进行监控，当某个服务提供方出现问题，Eureka自然会把它从服务
列表中剔除。
这就实现了服务的自动注册、发现、状态监控。

### 原理图

Eureka的主要功能是进行服务管理，定期检查服务状态，返回服务地址列表。

![1560439174201](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1560439174201.png)

| 组件名        | 解释                                                         |
| ------------- | ------------------------------------------------------------ |
| renewal       | 续约                                                         |
| Eureka-Server | 服务注册中心（可以是一个集群），对外暴露自己的地址。         |
| 提供者        | 启动后向Eureka注册自己信息（地址，服务名称等），并且定期进行服务续约 |
| 消费者        | 服务调用方，会定期去Eureka拉取服务列表，然后使用负载均衡算法选出一个服务进行调用。 |
| 心跳(续约)    | 提供者定期通过http方式向Eureka刷新自己的状态                 |

### 注册中心

#### 依赖

```xml
<dependencies>
		<!-- Eureka服务端 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>
	</dependencies>
```

#### 启动类

```java
@SpringBootApplication
//开启eureka服务
@EnableEurekaServer
public class EurekaServerApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```

#### 配置文件

```yml
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

### 服务提供者

==注册服务，就是在服务上添加Eureka的客户端依赖，客户端代码会自动把服务注册到EurekaServer中。==

在服务提供工程user-service上添加Eureka客户端依赖；自动将服务注册到EurekaServer服务地址列表

#### 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### 改造启动引导类

添加开启Eureka客户端发现的注解

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

#### 修改配置文件

设置Eureka 服务地址

```yml
# 服务端口
server:
  port: 8081
# 数据链接，请注意修改你自己数据库和账号密码
spring:
  application:
  #这里我们添加了spring.application.name属性来指定应用名称，将来会作为服务的id使用。
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

#### 其他配置

```yml
eureka:
	instance:
    	ip-address: 127.0.0.1 # ip地址 固定上报一个IP地址给eureka server
    	prefer-ip-address: true # 更倾向于使用ip，而不是host名,
             instance-id: ${eureka.instance.ip-address}:${server.port} #修改实例id
             #在注册服务完成以后，服务提供者会维持一个心跳（定时向EurekaServer发起Rest请求），告诉#EurekaServer：“我还活着”。这个                我们称为服务的续约（renew）；
             #有两个重要参数可以修改服务续约的行为；可以在 user-service 中添加如下配置
             lease-expiration-duration-in-seconds: 90 # 服务失效时间 默认值90秒
	    lease-renewal-interval-in-seconds: 30 #续约的间隔时间,默认为30秒
```

![1569405096470](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1569405096470.png)

### 服务消费者

在服务消费工程consumer上添加Eureka客户端依赖；可以使用工具类根据服务名称获取对应的服务地址列表。

#### 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### 添加启动引导类注解

```java
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

#### 修改配置

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

#### 使用eureka来发现服务

```java
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

### 高可用配置

Eureka Server即服务的注册中心，在刚才的案例中，我们只有一个EurekaServer，事实上EurekaServer也可以是一个集群，形成高可用的Eureka中心。

多个Eureka Server之间也会互相注册为服务，当服务提供者注册到Eureka Server集群中的某个节点时，该节点会把服务的信息同步给集群中的每个节点，从而实现高可用集群。因此，无论客户端访问到Eureka Server集群中的任意一个节点，都可以获取到完整的服务列表信息。

![1572884794115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1572884794115.png)

#### 修改配置

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

#### 复制配置

修改VM参数

```bash
-Dserver.port=10088 -DdefaultZone=http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```

#### 客户端注册服务到集群

user-service 项目注册服务或者 consumer-demo 获取服务的时候，service-url参
数需要修改

```yml
eureka:
    client:
    	service-url: # EurekaServer地址,多个地址以','隔开
    		defaultZone: http://127.0.0.1:10086/eureka,http://127.0.0.1:10087/eureka
```

#### 其他配置

```yml
eureka:
  client:
    registry-fetch-interval-seconds: 3 #默认30秒拉取一次注册信息【生产环境不改；测试环境该小】
    instance-info-replication-interval-seconds: 5 # 默认30秒替换实例的信息【生产环境不改；测试环境该小】
```

### 服务下线、失效剔除

当服务未按时进行心跳续约时，Eureka会统计服务实例最近15分钟心跳续约的比例是否低于了85%。如果低于85%就会触发Eureka的自我保护机制,此时Eureka不会剔除任何服务实例，直到网络恢复正常.

```yml
eureka:
  server:
    # 服务失效剔除时间间隔，默认60秒 eureka服务定时扫描失效的服务，把失效的服务剔除
    eviction-interval-timer-in-ms: 60000
    # 关闭自我保护模式（默认是打开的）
    enable-self-preservation: false
```

#### 消费者配置

```yml
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

#### 提供者配置

```yml
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

#### 注册中心配置

```yml
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

## 负载均衡Ribbon

```java
//在RestTemplate的配置方法上添加 @LoadBalanced 注解
//Ribbon默认的负载均衡策略是轮询。
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

```java
 //todo http://localhost:8080/consumber/8
 @GetMapping("/{id}")
 public User getUser(@PathVariable("id")Long id){
     //通过服务名称调用
 	String url = "http://user-service/user/8";
 	return restTemplate.getForObject(url,User.class);
 }
```

```yml
user-service:
	ribbon:
		NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

| **内置负载均衡规则类**    | **规则描述**                                                 |
| ------------------------- | ------------------------------------------------------------ |
| RoundRobinRule            | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。 |
| AvailabilityFilteringRule | 对以下两种服务器进行忽略：<br>（1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。注意：可以通过修改配置loadbalancer.<clientName>.connectionFailureCountThreshold来修改连接失败多少次之后被设置为短路状态。默认是3次。<br>（2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上线，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule  | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| ZoneAvoidanceRule         | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。 |
| BestAvailableRule         | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                | 随机选择一个可用的服务器。                                   |
| Retry                     | 重试机制的选择逻辑                                           |

- 原理

  LoadBalancerInterceptor --- > 2：找到intercept方法 ----> 3：RibbonLoadBalancerClient-的execute()方法----->找到加载负载均衡器和具体的服务

## 熔断器Hystrix

隔离访问远程服务、第三方库，防止出现级联失败。

### 雪崩问题

微服务中，服务间调用关系错综复杂，一个请求，可能需要调用多个微服务接口才能实现，会形成非常复杂的调用链路,服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用，形成雪崩效应

### 线程隔离

Hystrix为每个依赖服务调用分配一个小的线程池，如果线程池已满调用将被立即拒绝，默认不采用排队，加速失败判定时间。用户的请求将不再直接访问服务，而是通过线程池中的空闲线程来访问服务，如果线程池已满，或者请求超时，则会进行降级处理

### 服务降级

优先保证核心服务，而非核心服务不可用或弱可用。用户的请求故障时，不会被阻塞，更不会无休止的等待或者看到系统崩溃，至少可以看到一个执行结果（例如返回友好的提示信息） 。服务降级虽然会导致请求失败，但是不会导致阻塞，而且最多会影响这个依赖服务对应的线程池中的资源，对其它服务没有响应。

#### 降级逻辑

```java
@RestController
@Slf4j
@RequestMapping("/consumber")
public class UserController {
    @Autowired
    private RestTemplate restTemplate;
    //todo http://localhost:8080/consumber/8
    @GetMapping("{id}")
    @HystrixCommand(fallbackMethod = "queryByIdFallBack") //用来声明一个降级逻辑的方法
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

- 默认的Fallback

```java
@RestController
@Slf4j
@RequestMapping("/consumber")
@DefaultProperties(defaultFallback = "defaultFallback")//在类上指明统一的失败降级方法；该类中所有方法返回类型要与处理失败的方法的返回类型一致。
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

#### 超时配置

```yml
#修改超时配置,默认为1秒
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 2000
```

```java
@GetMapping("{id}")@HystrixCommand(fallbackMethod = "queryByIdFallBack",commandProperties = {        @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value="2000")})
public String getUser(@PathVariable("id")Long id){
```

### 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

### 开启熔断

在启动类 ConsumerApplication 上添加注解：@EnableCircuitBreaker

```java
//@EnableCircuitBreaker 等价于  @EnableHystrix
@SpringBootApplication
@EnableDiscoveryClient
@EnableCircuitBreaker
@RibbonClient(name = "SPRING-MICROSERVICECLOUD-RIBBON",configuration= MySelfRule.class)
public class ConsumberApplication{}
```

```java
//@SpringCloudApplication代替上面四个注解
@SpringCloudApplication
public class ConsumberApplication {
}
```

### 熔断原理

![1573444713021](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444713021.png)

![1573444732452](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud一/1573444732452.png)

| 状态      | 解释                                                         |
| --------- | ------------------------------------------------------------ |
| Closed    | 关闭状态（断路器关闭），所有请求都正常访问。                 |
| Open      | 打开状态（断路器打开），所有请求都会被降级。Hystrix会对请求情况计数，当一定时间内失败请求百分比达到阈值，则触发熔断，断路器会完全打开。默认失败比例的阈值是50%，请求次数最少不低于20次。 |
| Half Open | 半开状态，不是永久的，断路器打开后会进入休眠时间（默认是5S）。随后断路器会自动进入半开状态。此时会释放部分请求通过，若这些请求都是健康的，则会关闭断路器，否则继续保持打开，再次进行休眠计时。 |

### 配置

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

==为什么SpringClound进行远程服务调用时，实体类为什么不需要序列化了？HTTP协议是以什么格式传输数据呢==

> Http协议：Java对象  --->  Json字符串(String已经实现序列化) ---->  二进制 ----> 网络传输
>
> RPC协议：Java对象  -->  (pojo序列化)二进制 ----> 网络传输



## Feign

### 案例

Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。你不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。 

#### 依赖

```xml
<!-- 配置Feign启动器 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

#### 编写Feign的客户端

```java
//首先这是一个接口，Feign会通过动态代理，帮我们生成实现类。这点跟mybatis的mapper很像。
//@FeignClient注解，声明这是一个Feign客户端，同时通过value属性指定服务名称。
//接口中的方法，完全采用SpringMVC的注解，Feign会根据注解帮我们生成URL，并访问获取结果
//@GetMapping中的/user，请不要忘记；因为Feign需要拼接可访问的地址。
@FeignClient("user-service")
public interface UserClient {
    @GetMapping("/user/{id}")
    User findOne(@PathVariable("id") Long id);
}
```

#### 编写控制器

```java
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

#### 开启Feign的支持

```java
/** @SpringBootApplication
 @EnableDiscoveryClient
 @EnableCircuitBreaker 
 Feign中已经自动集成了Ribbon负载均衡，因此不需要自己定义RestTemplate进行负载均衡的配置。 */
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

### Ribbon的支持

```yml
ribbon:
  ConnectTimeout: 2000 # 建立连接超时时长
  ReadTimeout: 2000 # 读取响应数据超时时长
  MaxAutoRetries: 0 # 当前服务器的重试次数
  MaxAutoRetriesNextServer: 1 # 重试多少个服务节点
  OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试(只对查询)
```

### Hystrix的支持

```yml
feign:
  hystrix:
    enabled: true # 开启Feign的熔断功能
```

- 配置Feign中的Fallback

  - 定义UserClientFallback类，实现刚才编写的UserClient，作为fallback的处理类

    ```java
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


### 请求压缩&日志级别

#### 请求压缩

Spring Cloud Feign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗。通过下面的参数即可开启请求与响应的压缩功能(consumer中进行配置)：

```properties
feign:
  compression:
    request:
        enabled: true # 开启请求压缩
    response:
        enabled: true  # 开启响应压缩
```

#### 日志级别

前面讲过，通过 logging.level.xx=debug 来设置日志级别。然而这个对Fegin客户端而言不会产生效果。因为 @FeignClient 注解修改的客户端在被代理时，都会创建一个新的Fegin.Logger实例。我们需要额外指定这个日志的级别才可以。

- 在consumer的配置文件中设置com.itheima包下的日志级别都为debug

  ```properties
  logging:
    level:
      com.itheima: debug
  ```

- 在consumer编写配置类，定义日志级别

  ```java
  @Configuration
  public class FeignConfig {
      @Bean
      public Logger.Level feignLoggerLevel(){
          // 记录所有请求和响应的明细，包括头信息、请求体、元数据
          return Logger.Level.FULL;
      }
  }
  ```

  | 级别    | 解释                                                 |
  | ------- | ---------------------------------------------------- |
  | NONE    | 不记录任何日志信息，这是默认值                       |
  | BASIC   | 仅记录请求的方法，URL以及响应状态码和执行时间        |
  | HEADERS | 在BASIC的基础上，额外记录了请求和响应的头信息        |
  | FULL    | 记录所有请求和响应的明细，包括头信息、请求体、元数据 |

- 在consumer的UserClient中指定配置类

  ```java
  @FeignClient(value = "user-service",
          fallback = UserClientFallback.class,
          configuration = FeignConfig.class)
  public interface UserClient {
      @GetMapping("/user/{id}")
      User findOne(@PathVariable("id") Long id);
  }
  ```

## 网关Gateway

Spring Cloud Gateway组件的核心是一系列的过滤器，通过这些过滤器可以将客户端发送的请求转发（路由）到对应的微服务。 Spring Cloud Gateway是加在整个微服务最前沿的防火墙和代理器，隐藏微服务结点IP端口信息，从而加强安全保护。Spring Cloud Gateway本身也是一个微服务，需要注册到Eureka服务注册中心。

| 概念              | 解释                                                         |
| ----------------- | ------------------------------------------------------------ |
| 路由（route）     | 路由信息的组成：由一个ID、一个目的URL、一组断言工厂、一组Filter组成。如果路由断言为真，说明请求URL和配置路由匹配。 |
| 断言（Predicate） | Spring Cloud Gateway中的断言函数输入类型是Spring 5.0框架中的 ServerWebExchange。Spring Cloud Gateway的断言函数允许开发者去定义匹配来自于Http Request中的任何信息比如请求头和参数。 |
| 过滤器（Filter）  | 一个标准的Spring WebFilter。 Spring Cloud Gateway中的Filter分为两种类型的Filter，分别是Gateway Filter和Global Filter。过滤器Filter将会对请求和响应进行修改处理。 |

### 案例

#### 依赖

```xml
<dependencies>
        <!-- 配置gateway启动器 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
    </dependencies>
```

#### 编写启动类

```java
@SpringBootApplication
public class GatewayApplication {
    public static void main(String[] args){
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

#### 编写配置

```yml
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
          #uri: http://127.0.0.1:8082
         # 代理的服务地址；lb表示负载均衡(从eureka中获取具体服务)
          uri: lb://user-service
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/api/user/**
          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
            - StripPrefix=1
```

### 面向服务的路由

#### 依赖

```xml
<!-- 配置eureka客户端启动器 -->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### 引导类中开启使用

```java
@EnableDiscoveryClient //开启服务发现
```

#### 配置

```yml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:10086/eureka/,http://localhost:10087/eureka/
```

### 过滤器

Gateway作为网关的其中一个重要功能，就是实现请求的鉴权。而这个动作往往是通过网关提供的过滤器来实现的。前面的 路由前缀 章节中的功能也是使用过滤器实现的。

| 过滤器名称           | 说明                         |
| :------------------- | :--------------------------- |
| AddRequestHeader     | 对匹配上的请求添加Header     |
| AddRequestParameters | 对匹配上的请求添加参数       |
| AddResponseHeader    | 对从网关返回的响应添加Header |
| StripPrefix          | 对匹配上的请求路径去除前缀   |

- 全局过滤器:  自定义全局过滤器，不需要在配置文件中配置，作用在所有的路由上，实现 GlobalFilter 接口即可。

- 局部过滤器：通过spring.cloud.routes.fifilters 配置在具体路由下，只作用在当前路由上；自带的过滤器都可以配置或者自定义按照自带过滤器的方式。 

```yml
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

#### 自定义过滤器

- 在gateway-server模块中编写过滤器工厂类MyParamGatewayFilterFactory

  ```java
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


#### 自定义全局过滤器

- 在gateway-server模块中编写全局过滤器类MyGlobalFilter

```java
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

### 集成Ribbon

Gateway中默认就已经集成了Ribbon负载均衡，我们添加配置即可。

```yml
# 负载均衡
ribbon:
  ConnectTimeout: 2000 # 建立连接超时时长
  ReadTimeout: 2000 # 读取响应数据超时时长
  MaxAutoRetries: 0 # 当前服务器的重试次数
  MaxAutoRetriesNextServer: 1 # 重试多少个服务节点
  OkToRetryOnAllOperations: false # 是否对所有的请求方式都重试(只对查询)
```

### 集成Hystrix

- 在gateway-server中，添加hystrix启动器

  ```xml
  <!-- 配置hystrix启动器 -->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
  </dependency>
  ```

- 配置线程隔离时间

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

- 在默认过滤器中配置Hystrix过滤器(对全部路由有效)

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

- 创建FallbackController.java控制器，提供服务降级方法

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


### 高可用

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

## 配置中心Config

在分布式系统中，由于服务数量非常多，配置文件分散在不同的微服务项目中，管理不方便。为了方便配置文件集中管理，需要分布式配置中心组件。在Spring Cloud中，提供了Spring Cloud Config，它支持配置文件放在配置服务的本地，也支持放在远程Git仓库（GitHub、码云）。

![1573113840392](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springcloud二/1573113840392.png)

### 案例

#### 依赖

```xml
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
```

#### 启动类

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args){
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

#### 配置文件

```yml
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

### 获取配置中心配置

#### 依赖

在user-service模块中，添加如下依赖:

```xml
<!-- 配置config启动器  -->
<dependency>
  	<groupId>org.springframework.cloud</groupId>
  	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

#### 修改配置

- 删除user-service模块中的application.yml文件（因为该文件从配置中心获取）

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
        # 第二种方式 , 直接指定配置中心的uri
        uri: http://localhost:12000
        #第一种方式
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

## 消息总线Bus

Spring Cloud Bus是用轻量的消息代理将分布式的节点连接起来,可以用于广播配置文件的更改或者服务的监控管理。一个关键的思想就是,消息总线可以为微服务做监控,也可以实现应用程序之间相互通信。Spring Cloud Bus可选的消息代理有RabbitMQ和Kfaka。

![image-20211007213036713](C:\Users\guoshutao\AppData\Roaming\Typora\typora-user-images\image-20211007213036713.png)

![image-20211007213044448](C:\Users\guoshutao\AppData\Roaming\Typora\typora-user-images\image-20211007213044448.png)

### 改造配置中心

#### 依赖

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

#### 配置

```yml
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

### 改造用户服务

#### 依赖

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

#### 配置

```yml
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

## 技术体系综合应用概览

![image-20211007213324976](C:\Users\guoshutao\AppData\Roaming\Typora\typora-user-images\image-20211007213324976.png)