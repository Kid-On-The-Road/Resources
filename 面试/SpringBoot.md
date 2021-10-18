# SpringBoot

![1112095-20181115161924186-928667393](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/1112095-20181115161924186-928667393.png)

## 介绍

Spring Boot使您可以轻松地创建独立的、生产级的、基于Spring的应用程序，您可以“立即运行”。

## 解决的问题

- 复杂的配置

- 混乱的依赖管理

## 特点

+ 创建独立的Spring应用。 

+ 直接嵌入Web服务器，如tomcat、jetty、undertow等；不需要去部署war包。

+ 提供固定的启动器依赖去简化组件配置；实现开箱即用（启动器starter其实就是Spring Boot提供的一个jar包），通过自己设置参数（.properties或.yml的配置文件），即可快速使用。 

+ 自动地配置Spring和其它有需要的第三方依赖。

+ 提供了一些大型项目中常见的非功能性特性，如内嵌服务器、安全、指标，健康检测、外部化配置等。 

+ 绝对没有代码生成，也无需 XML 配置。

## 依赖

```xml
 <!-- 1. 配置父级(作用：依赖版本锁定) -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
    </parent>
    <!-- 配置全局属性 -->
    <properties>
        <!-- 2. 覆盖父级属性(jdk版本) -->
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!-- 3. 配置web启动器(作用：自动整合SpringMVC、jackson、内嵌tomcat) -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

## 案例

- 启动类

```java
/** SpringBoot启动类 
*@SpringBootApplication 注解的普通Java类【是运行SpringBoot项目的入口类】
*内嵌tomcat的端口号默认是8080
*@SpringBootApplication已经有默认的包扫描【默认为启动类的包名】,这样同级或下级都可扫描。
*/
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args){
        // 运行Spring应用 (一个run方法就可以运行web项目)
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

- 编写Controller

```java
@RestController
public class QuickController {
    @GetMapping("/quick")
    public String quick(){
        return "SpringBoot 从入门到精通！！";
    }
}
```

## 自定义Banner

### 关闭banner横幅

- 方式一: controller

```java
/** SpringBoot启动类 */
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args){
        // 运行Spring应用 (一个run方法就可以运行web项目)
        //SpringApplication.run(DemoApplication.class, args);
        SpringApplication springApplication = new SpringApplication(DemoApplication.class);
        // 设置banner为关闭模式
        springApplication.setBannerMode(Banner.Mode.OFF);
        springApplication.run(args);
   }
}
```

- 方式二: application.properties

```properties
# 设置banner模式为关闭
spring.main.banner-mode=off
```

### 自定义banner横幅

第一种方式: 在resource目录下，创建banner.txt (默认加载)

第二种方式: 在resource目录下，创建banner.jpg或banner.png (默认加载)

## 起步依赖原理

+ 我们的项目springboot-demo需配置起步依赖

  ![1573608298438](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573608298438.png)   

+ spring-boot-starter-parent项目环境属性锁定

  ![1573608073529](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573608073529.png)  

+ spring-boot-dependencies依赖版本锁定

  ![1573609774763](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573609774763.png)  

## 启动器&自动配置

### 启动器介绍

+ Spring Boot提供的启动器

  ![1573615036960](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573615036960.png) 

  ![1573615055747](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573615055747.png) 

  ![1573615078536](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573615078536.png) 

+ Spring Boot启动器的作用

  + 配置一个启动器它会把整合这个框架或模块的依赖全部导入。
  + 每一个启动器都有一个自动配置类,实现自动整合Spring。
  + 每一个启动器都有一个配置属性类,提供了默认整合的属性配置。

### 自动配置原理

+ 一切魔力的开始，来自启动类的main方法:

  ```java
  /** SpringBoot启动类*/
  @SpringBootApplication
  public class DemoApplication {
      public static void main(String[] args) {
          // 运行Spring应用 (一个run方法就可以运行web项目)
          SpringApplication.run(DemoApplication.class, args);
      }
  }
  ```

- @SpringBootApplication(相当于三个注解的组合)

  | 注解名                   | 作用         | 原理                                                         |
  | ------------------------ | ------------ | ------------------------------------------------------------ |
  | @SpringBootConfiguration | 定义配置类   |                                                              |
  | @EnableAutoConfiguration | 启用自动配置 | 告诉SpringBoot基于你所添加的依赖,去“猜测”你想要如何配置Spring。<br />比如我们引入了`spring-boot-starter-web`,而这个启动器中帮我们添加了`tomcat`、`SpringMVC`的依赖。<br />此时自动配置就知道你是要开发一个web应用，所以就帮你完成了web及SpringMVC的默认配置了！ |
  | @ComponentScan           | 组件扫描     | 配置组件扫描的指令。提供了类似与`<context:component-scan>`标签的作用<br />通过basePackageClasses或者basePackages属性来指定要扫描的包。<br />如果没有指定这些属性，那么将从声明这个注解的类所在的包开始，扫描包及子包 |

- 自动配置实现流程

  + SpringApplication在运行Spring应用时，是通过SpringFactoriesLoader的loadSpringFactories()方法,初始化Spring工厂实例。

    ![1573630323322](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573630323322.png) 

    ![1573630573746](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573630573746.png) 

  + META-INF/spring.factories工厂配置文件

    ![1573631186998](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573631186998.png)

     加载该文件，判断对应的启动器是否导入，如果导入就创建实例，自动注入默认属性。![1573631374003](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573631374003.png) 

    这个key所对应的值，就是所有的自动配置类，可以在当前的jar包中找到这些自动配置类:

    ![1573632197919](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573632197919.png) 

    每个包都有一个XxxAutoConfiguration配置类，都是一个基于纯注解的配置类，是各种框架整合的代码。该自动配置类，如果有默认属性，在该包下就会有一个XxxProperties属性类:

    ![1573632733872](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573632733872.png) 

> 总结:
>
> - SpringApplication会寻找 META-INF/spring.factories 文件，读取其中以EnableAutoConfiguration 为key的所有类的名称, 这些类就是提前写好的自动配置类。
>
> - 这些类都声明了@Configuration注解，并且通过@Bean注解提前配置了我们所需要的一切实例，完成自动配置。
>
> - 这些配置类不一定全部生效，因为有@ConditionalOn注解，满足一定条件才会生效。(类存在条件)
>
> - 我们可以通过配置application.yml或application.properties文件，来覆盖自动配置中的属性。

### 配置属性

#### 介绍

Spring Boot为所有的启动器提供了默认的属性配置,如果我们需要修改就必须要知道哪些属性是属于哪些启动器的默认配置。

#### 配置属性流程

+ 第一步：配置一个自动配置类的属性，先到spring-boot-autoconfigure-2.1.6.RELEASE.jar找到对应的模块。

  ![1573638549025](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573638549025.png) 

+ 第二步：如果该自动配置类有可以配置的属性，那么对应的整合模块中一定有一个XxxProperties属性类，在里面找可以配置的属性。

  ![1573638579014](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573638579014.png) 

   第三步：在resources目录下的application.properties或application.yml文件里面可以修改		XxxProperties类中默认的属性。

  ![1573638600474](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573638600474.png) 

## 配置文件

### properties配置

- application.properties属性文件

```properties
# 设置tomcat端口号
server.port=9001
# 设置项目的访问路径
server.servlet.context-path=/
```

### yml配置

- application.yml

```yml
# 注意: 冒号后面必须有空格
# 覆盖自动配置类的默认属性
server:
  port: 9002
  servlet:
    contextPath: /
# 定义自已项目中需要的属性
# 标量类型
my:
 host: 127.0.0.1
 port: 3306
 # 对象类型
 user:
   name: 小华华
   age: 18
   sex: 男
 # 数组类型
 address:
   - 天河九巷
   - 天河八巷
   - 天河十巷
 # 对象数组类型
 users:
   - name: 李小三
     age: 18
     sex: 女
   - name: 李小一
     age: 20
     sex: 男	
```

<font color="red">注意</font>:

当application.properties与application.yml两个文件同时存在时，当属性名相同时application.properties中的属性会 覆盖 application.yml中的属性。

### 访问配置文件

- 方式一

```java
@RestController
/** java语言中的全部数据类型*/
@ConfigurationProperties(prefix = "my") //尽量指定前缀，避免冲突
public class PropController1 {
    private String host;
    private int port;
    private User user;
    private String[] address;
    private List<User> users;
    @GetMapping("/test1")
    public String test1(){
        System.out.println("======test1=======");
        System.out.println("host = " + host);
        System.out.println("port = " + port);
        System.out.println("user = " + user);
        System.out.println("address = " + Arrays.toString(address));
        System.out.println("users = " + users);
        return "test1方法，访问成功！";
    }
    /** 注入的属性必须提供setter方法 */
    public void setHost(String host) {
        this.host = host;
    }
    public void setPort(int port) {
        this.port = port;
    }
    public void setUser(User user) {
        this.user = user;
    }
    public void setAddress(String[] address) {
        this.address = address;
    }
    public void setUsers(List<User> users) {
        this.users = users;
    }
}
```

- 方式二

```java
/**
* @Value注解的属性，不需要提供setter方法
* @Value只能注入: 基本数据类型与String
*/
@RestController
@ConfigurationProperties(prefix = "my")
public class PropController2 {
    @Value("${my.host}")
    private String host;
    @Value("${my.port}")
    private int port;
    private User user;
    private String[] address;
    private List<User> users;
    @GetMapping("/test2")
    public String test2(){
        System.out.println("======test2=======");
        System.out.println("host = " + host);
        System.out.println("port = " + port);
        System.out.println("user = " + user);
        System.out.println("address = " + Arrays.toString(address));
        System.out.println("users = " + users);
        return "test2方法，访问成功！";
    }
    public void setUser(User user) {
        this.user = user;
    }
    public void setAddress(String[] address) {
        this.address = address;
    }
    public void setUsers(List<User> users) {
        this.users = users;
    }
}
```

- 方式三

```java
//定义属性类,将需要注入的属性定义成一个属性类，可以重复使用。
@ConfigurationProperties(prefix = "my")
public class UserProperties {
    private String host;
    private int port;
    private User user;
    private String[] address;
    private List<User> users;
    public String getHost() {
        return host;
    }
    public void setHost(String host) {
        this.host = host;
    }
    public int getPort() {
        return port;
    }
    public void setPort(int port) {
        this.port = port;
    }
    public User getUser() {
        return user;
    }
    public void setUser(User user) {
        this.user = user;
    }
    public String[] getAddress() {
        return address;
    }
    public void setAddress(String[] address) {
        this.address = address;
    }
    public List<User> getUsers() {
        return users;
    }
    public void setUsers(List<User> users) {
        this.users = users;
    }
}
```

```java
/**启动配置类 */
@RestController
@EnableConfigurationProperties(UserProperties.class) // 启动配置属性(创建对象)
public class PropController3 {
    @Autowired(required = false)
    private UserProperties userProperties;
    @GetMapping("/test3")
    public String test3(){
        System.out.println("======test3======");
        System.out.println(userProperties.getHost());
        System.out.println(userProperties.getPort());
        System.out.println(userProperties.getUser());
        System.out.println(userProperties.getAddress());
        System.out.println(userProperties.getUsers());
        return "test3方法，访问成功！";
    }
}
```

## 框架整合

### 整合lombok

#### 依赖

```xml
<!-- 引入lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <scope>provided</scope>
</dependency>
```

####  注解介绍

| 注解名              | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| @Data               | 自动生成getter、setter、hashCode、equals、toString方法       |
| @AllArgsConstructor | 自动生成全参构建器                                           |
| @NoArgsConstructor  | 自动生成无参构建器                                           |
| @Setter             | 自动生成setter方法                                           |
| @Getter             | 自动生成getter方法                                           |
| @EqualsAndHashCode  | 自动生成equals、hashCode方法                                 |
| @ToString           | 自动生成toString方法                                         |
| @Slf4j              | 自动在bean中提供log变量，其实用的是slf4j的日志功能。         |
| @NonNull            | 这个注解可以用在**成员方法**或者**构造方法的参数**前面，会自动产生一个关于此参数的非空检查，如果参数为空，则抛出一个空指针异常。 |

```java
@ToString // toString
@Data // getter、setter、toString、equals、hashCode
@AllArgsConstructor // 全参构造器
@NoArgsConstructor // 无参构造器
public class User {
    private String name;
    private int age;
    private String sex;
}
```

### 整合SpringMVC

#### 日志控制

```yml
# 配置日志
logging:
  level:
    cn.itcast: debug
    org.springframework: debug
```

| 参数名                         | 解释                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| logging.level                  | 是固定写法，说明下面是日志级别配置，日志相关其它配置也可以使用。 |
| cn.itcast、org.springframework | 是指定包名，后面的配置仅对这个包有效。                       |
| debug                          | 日志的级别                                                   |

```java
@RestController
@Slf4j // 日志注解
public class LogController {
    @GetMapping("/log")
    public String log(){
        log.debug("====debug====");
        log.info("====info====");
        log.warn("====warn====");
        log.error("====error====");
        return "log....";
    }
}
```

#### 访问静态资源

![1573703089523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573703089523.png) 

![1573703509681](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573703509681.png) 

![1573703587017](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/springboot/1573703587017.png) 

默认的静态资源访问路径有四个:

- classpath:/META-INF/resources/
- classpath:/resources/
- classpath:/static/
- classpath:/public

<font color="red">注意:</font> 只要静态资源放在这些目录中任何一个，SpringMVC都会帮我们处理。

#### 添加拦截器

- 自定义拦截器实现HandlerInterceptor接口

```java
/** 定义拦截器 */
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
    /** 前置处理 */
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler) throws Exception {
        log.debug("== preHandle 方法执行! ==");
        return true;
    }
    /** 后置处理 */
    @Override
    public void postHandle(HttpServletRequest request,
                           HttpServletResponse response,
                           Object handler,
                           ModelAndView modelAndView) throws Exception {
        log.debug("== postHandle 方法执行! ==");
    }
    /** 视图渲染之后 */
    @Override
    public void afterCompletion(HttpServletRequest request,
                                HttpServletResponse response,
                                Object handler, Exception ex) throws Exception {
        log.debug("== afterCompletion 方法执行! ==");
    }
}
```

- 自定义配置类实现WebMvcConfigurer接口,注册拦截器

```java
@Configuration
public class MvcConfiguration implements WebMvcConfigurer {
    /** 重写接口中的addInterceptors方法，添加自定义拦截器 */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 添加拦截器，通过addPathPatterns来添加拦截路径
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/**");
    }
}
```

### 整合JDBC&事务

#### 依赖

```xml
<!-- 配置jdbc启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<!-- 配置mysql驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

#### 配置连接池

```yml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springboot_db
    username: root
    password: root
```

### 整合Mybatis

#### 依赖

引入mybatis启动器依赖(**它依赖了jdbc启动器,jdbc启动器可以删除**)

```xml
<!-- 配置mybatis启动器 -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.0</version>
</dependency>
```

#### 配置mybatis相关属性

```yml
mybatis:
  # 配置类型别名包扫描
  type-aliases-package: cn.itcast.springboot.pojo
  # sql语句映射文件路径
  mapper-locations:
    - classpath:mappers/*.xml
  # 驼峰映射
  configuration:
    map-underscore-to-camel-case: true
# 配置日志
logging:
  level:
    cn.itcast: debug
```

#### 数据访问接口

- 方式一 (@Mapper注解)

```java
@Mapper // 声明数据访问接口，产生代理对象
public interface UserMapper {
    // 查询全部用户
    List<User> findAll();
}
```

- 方式二 (@MapperScan注解)

```java
public interface UserMapper {
    // 查询全部用户
    List<User> findAll();
}
```

```java
//导入的是org包下的MapperScan
import org.mybatis.spring.annotation.MapperScan;
/** 启动类 */
@SpringBootApplication
// 数据访问接口包扫描
@MapperScan(basePackages = {"cn.itcast.springboot.mapper"})
public class HighApplication {
    public static void main(String[] args){
        // 运行spring应用
        SpringApplication.run(HighApplication.class, args);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.itcast.springboot.mapper.UserMapper">

	<select id="findAll" resultType="User">
		SELECT * FROM tb_user
	</select>
</mapper>
```

```java
@Service
@Transactional
public class UserService {
    @Autowired(required = false)
    private UserMapper userMapper;
    // 查询全部用户
    public List<User> findAll(){
        return userMapper.findAll();
    }
}
```

```java
@RestController
public class UserController {
    @Autowired
    private UserService userService;
    // 查询全部用户
    @GetMapping("/findAll")
    public List<User> findAll(){
        return userService.findAll();
    }
}
```

### 整合通用mapper

#### 依赖

引入通用Mapper启动器依赖(**它依集成了mybatis,mybatis启动器可以删除**)

```xml
<!-- 配置通用Mapper启动器 -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.1.5</version>
</dependency>
```

```java
//在实体类上加JPA注解 (User)
@Data
@Table(name = "tb_user")
public class User{
	// 用户id
	@Id // 主键
	@KeySql(useGeneratedKeys = true) // 开启自增主键返回功能
	private Long id;
	// 用户名
	private String userName;
	// 密码
	private String password;
	// 姓名
	private String name;
	// 年龄
	private Integer age;
	// 性别 1: 男 2: 女
	private Short sex;
	// 出生日期
	private Date birthday;
	// 备注
	private String note;
	// 创建时间
	private Date created;
	// 修改时间
	private Date updated;
}
```

#### 数据访问接口

```java
//@Mapper // 声明数据访问接口，产生代理对象
public interface UserMapper extends Mapper<User> {
}
```

```java
@Service
@Transactional
public class UserService {
    @Autowired(required = false)
    private UserMapper userMapper;
    // 查询全部用户
    public List<User> findAll(){
        return userMapper.selectAll();
    }
}
```

```java
//导入的包是tk下的MapperScan
import tk.mybatis.spring.annotation.MapperScan;
/** 启动类 */
@SpringBootApplication
// 数据访问接口包扫描
@MapperScan(basePackages = {"cn.itcast.springboot.mapper"})
public class HighApplication {
    public static void main(String[] args){
        // 运行spring应用
        SpringApplication.run(HighApplication.class, args);
    }
}
```

### 整合Junit

#### 依赖

```xml
<!-- 配置test启动器(自动整合spring-test、junit) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

```java
// 运行主类
@RunWith(SpringRunner.class)
// 如果测试类在启动类的同级目录或者子目录下可以省略指定启动类
//@SpringBootTest(classes = {HighApplication.class})
@SpringBootTest
public class UserServiceTest {
    @Autowired
    private UserService userService;
    @Test
    public void findAll(){
        List<User> users = userService.findAll();
        System.out.println(users);
    }
}
```

### 整合Redis

#### 依赖

```xml
<!-- 配置redis启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 配置Redis连接属性

```yml
# 配置Redis
spring:
    redis:
      host: localhost # 主机
      port: 6379      # 端口
```

#### 注入RedisTemplate操作Redis

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class RedisTemplateTest {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    public void redisTest(){
        // 设置值
        redisTemplate.opsForValue().set("name", "admin");
        // 获取值
        Object name = redisTemplate.opsForValue().get("name");
        System.out.println("name = " + name);
        // 删除值
        redisTemplate.delete("name");
    }
}
```

## 项目打包部署

### 打成Jar包

- 引入Spring Boot打包插件

```xml
<build>
    <plugins>
        <!-- 配置spring-boot的maven插件
                1. 用它可以运行spring-boot项目
                2. 需要用它构建打jar、war资料
             -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

- 执行命令

```properties
# 清理、打包
mvn clean package
# 清理、打包 跳过测试
mvn clean package -Dmaven.test.skip=true
```

- 运行

```properties
java -jar xxx.jar
```

### 打成war包

- 修改pom.xml

```xml
<!-- 打包方式(默认为jar) -->
<packaging>war</packaging>
```

- 排除springboot自带的tomcat

```xml
<!-- 配置Web启动器(集成SpringMVC、内嵌tomcat、Jackson) -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--
   配置tomcat启动器，就会排除spring-boot-starter-web中依赖过来的tomcat启动器
   指定scope为provided: 代表打war包时，不需要它的依赖jar包(我们有自己的tomcat)
-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

**注意: **spring-boot-starter-tomcat 是原来被传递过来的依赖，默认会打到包里，所以我们需要配置tomcat启动器，这样就会排除spring-boot-starter-web中依赖过来的tomcat启动器，并指定依赖范围为provided，这样tomcat相关的jar就不会打包到war里了。

目的: 我们用自己tomcat，不用它内嵌的tomcat，这样内嵌的tomcat相关jar包就不需要。

- 自定义Web应用入口类继承SpringBootServletInitializer(相当于web.xml)

```java
/** web应用入口 */
public class WebServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        // 设置启动类
        builder.sources(HighApplication.class);
        // 返回spring应用构建对象
        return builder;
    }
}
```

- 在pom.xml修改工程的名称为`ROOT`

```xml
<build>
    <!-- 指定最终打成war的项目名称 -->
    <finalName>ROOT</finalName>
</build>
```

说明: ROOT是tomcat的默认工程名，也是唯一一个不需要加工程访问的目录，所以我们打包的时候用finalName指定的名字打包就直接生成的WAR包就是ROOT.war

- 部署项目`ROOT.war`

> 安装JDK1.8环境
>
> 安装Tomcat 把 ROOT.war部署到webapps下即可
>
> 启动Tomcat，`bin/startup.bat` 即可,会自动解压ROOT.war
>
> 访问 http://localhost:8080/findAll

