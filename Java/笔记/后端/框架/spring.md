### 01复习

#### 目标

- 理解开发中的 **三层架构**
- 理解分层架构的作用



#### 1. 理解三层架构

##### 1.1 分层概念

- 三层架构(3-tier architecture): **视图层**, **业务层**, **持久层**

![1559290090936](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1559290090936.png)

##### 1.2 分层案例

1. 创建项目: spring01_tier_01

2. 创建持久层: com.itheima.dao

   - com.itheima.tier.dao.UserDao

     ```java
     package com.itheima.tier.dao;
       /**
           * 用户持久层接口 (操作类).
        *
           * @author : Jason.lee
           * @version : 1.0
           */
       public interface UserDao {
                /**
           * 保存用户.
           */
          void save();
      }
          
     
     ```

   - com.itheima.tier.dao.impl.UserDaoImpl

     ```java
     package com.itheima.tier.dao.impl;
     
     import com.itheima.tier.dao.UserDao;
     
     /**
      * 用户持久层实现类.
      *
      * @author : Jason.lee
      * @version : 1.0
      */
     public class UserDaoImpl implements UserDao {
     
         @Override
         public void save() {
             System.out.println("用户保存成功..");
         }
     }
     
     ```

   

3. 创建业务层: com.itheima.service

   - com.itheima.tier.service.UserService

     ```java
     package com.itheima.tier.service;
       /**
           * 用户业务层接口.
        *
           * @author : Jason.lee
           * @version : 1.0
           */
       public interface UserService {
                /**
           * 保存用户.
           */
          void save();
      }
     
     ```

   - com.itheima.tier.service.impl.UserServiceImpl

     ```java
     package com.itheima.tier.service.impl;
     
     import com.itheima.tier.dao.UserDao;
     import com.itheima.tier.dao.impl.UserDaoImpl;
     import com.itheima.tier.service.UserService;
     
     /**
      * 用户业务层实现类.
      *
      * @author : Jason.lee
      * @version : 1.0
      */
     public class UserServiceImpl implements UserService {
     
     
         UserDao userDao = new UserDaoImpl();
     
         @Override
         public void save() {
             // 1. 做一些业务逻辑处理
     
     
             // 2. 调用dao操作数据库
             userDao.save();
         }
     }
     
     ```

4. 创建视图层: com.itheima.tier.controller.UserController

   ```java
   package com.itheima.tier.controller;
   
   import com.itheima.tier.service.UserService;
   import com.itheima.tier.service.impl.UserServiceImpl;
   
   /**
    * 用户视图层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class UserController {
   
   
       /**
        * 模拟用户发起请求
        * @param args
        */
       public static void main(String[] args) {
           // 1. 处理参数
   
   
           // 2. 业务处理
           UserService userService = new UserServiceImpl();
           userService.save();
       }
   
   }
   
   ```



#### 2. 分层架构的作用

1. 项目结构清晰 (提高代码可读性)

  |-- tier

  |---- **controller** // 处理页面信息

  |---- **service** // 处理业务逻辑

  |---- **dao** // 处理数据

2. 便于团队协同开发 (提高开发效率)

  > ​	职责分明后, 三个层的代码可以同步进行开发



#### 小结

- 三层架构分为哪三层?
  - 视图层
  - 业务层
  - 持久层
- 为什么要分层架构呢?
  - 提高开发效率
  - 提高代码可读性



### 02常见的依赖问题【了解】

#### 目标

- **了解依赖问题**



#### 1. 了解依赖问题

- 代码耦合度过高



##### 1.1 实现类丢失

- 如果编译时丢失UserDaoImpl.java文件将无法编译

![1564640574099](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1564640574099.png) 

##### 1.2 切换实现类

- 希望使用Oracle的实现类, 要改动代码重新编译

1. 创建实现类: com.itheima.tier.dao.impl.UserDaoOracleImpl

   ```java
   package com.itheima.tier.dao.impl;
   
   import com.itheima.tier.dao.UserDao;
   
   /**
    * Oracle用户数据库操作实现类.
    *
    * @author : Jason.lee
    */
   public class UserDaoOracleImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("保存Oracle用户成功");
       }
   }
   ```

2. 需要改动代码: ~~com.itheima.tier.service.impl.UserServiceImp~~



#### 小结

- 三层架构中存在什么问题?
  - 代码耦合度过高
- 代码耦合度过高有哪些例子?
  - 实现类丢失问题
  - 实现类切换问题



### 03解决高耦合问题【理解】

#### 目标

- 解决实现类丢失问题
- 解决实现类切换问题



#### 1. 解决实现类丢失

1. 【祸水东引】使UserService正常

   - com.itheima.tier.BeanFactory

   ```java
   package com.itheima.tier;
   
   import com.itheima.tier.dao.UserDao;
   import com.itheima.tier.dao.impl.UserDaoImpl;
   
   /**
    * 创建持久层接口实现类.
    */
   public class BeanFactory {
       /**
        * 祸水东引.
        * 将创建实现类对象的过程放置到BeanFactory.
        */
       public static UserDao getBean(){
           return new UserDaoImpl();
       }
   }
   ```

1. 【彻底解决】使项目编译正常

   > 学习Class.forName("com.mysql.jdbc.Driver")运行时再加载字节码文件

   ```java
   
   ```



#### 2. 解决实现类切换

- 【一劳永逸】希望切换实现类不用重新编译

1. 添加配置: beans.properties

   ```properties
   # 用户持久层实现类
   UserDao=com.itheima.tier.dao.impl.UserDaoImpl
   # 用户服务层实现类
   UserService=com.itheima.tier.service.impl.UserServiceImpl
   ```

2. 添加支持: com.itheima.tier.BeanFactory

   ```java
   /**
        * 根据配置文件的key创建具体的实现类对象
        * @param name 接口名称
        * @return 对象
        */
   public static Object getBean(String name) {
       try {
           Properties props = new Properties();
           // 1. 加载配置文件
           InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("beans.properties");
           props.load(in);
           // 2. 获取字节码路径
           String clazz = props.getProperty(name);
           // 3. 创建对象
           return getBean2(clazz);
       } catch (IOException e) {
           e.printStackTrace();
           return null;
       }
   }
   ```



#### 3. [扩展] 工厂模式

- 以上解决代码耦合度过高的方案应用广泛, 称之为 **工厂设计模式**
- 工厂设计模式的作用: 
  1. 创建对象
  2. 降低耦合
  3. 消除重复



#### 小结

- 依赖问题 ( 代码耦合度过高 ) 如何解决?
  - 使用工厂设计模式解决



### 04分层的其他问题【了解】

#### 目标

- 了解分层架构的其他问题



#### 1. 依赖关系问题

![1564713528158](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1564713528158.png) 



#### 2. 创建顺序问题

![1564716026508](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1564716026508.png) 

#### 3. 对象个数问题

- 每次请求都创建新的对象 (消耗内存)

  ```java
  public static Object getBean(String name){
      try {
          Properties props = new Properties();
  		props.load(BeanFactory.class.getClassLoader()
             				.getResourceAsStream("beans.properties"))
          String clazz = props.getProperty(name);
          // 创建对象 (每次使用都创建新的对象)
          return getBean2(clazz);
      } catch (IOException e) {
          System.out.println("文件读取失败");
      }
      return null;
  }
  ```



#### 小结

- 分层架构还有哪些问题?
  - 依赖关系问题
  - 创建顺序问题
  - 对象个数问题



### 05Spring框架概述【了解】

#### 目标

- 了解Spring框架
- 了解Spring结构
- 了解Spring优点



#### 1. Spring框架

##### 1.1 官网

- 官网: https://spring.io/

- 官网文档地址: https://spring.io/projects/spring-framework#learn

- 依赖下载地址: https://repo.spring.io/libs-release-local/org/springframework/spring/



##### 1.2 介绍

- Spring是 **分层** 的轻量级开源框架 (重点)
- Spring核心是**IOC**(控制反转) 和 **AOP**(面向切面编程)
- Spring提供了对各种优秀框架的支持和 **整合**



#### 2. Spring结构

- Spring框架采用分层架构，根据不同的功能被划分成了多个模块

![1559212182908](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1557482552440.png)

###### Data Access/Integration

- JDBC: 对各大数据库厂商进行抽象处理
- ORM: 集成orm框架支持对象关系映射处理
- OXM: 提供了对 Object/XML映射实现的抽象层
- JMS: 主要包含了一些制造和消费消息的特性
- Transactions: 支持编程和声明式事务管理



###### Web

- Websocket: 提供了WebSocket和SocketJS的实现
- Servlet: 利用MVC(model-view-controller)的实现分离代码
- Web: 提供了基础的面向 Web 的集成特性(如: 文件上传)
- Portlet: 提供了Portlet环境下的MVC实现



###### 中间层

- AOP: 提供了符合AOP要求的面向切面的编程实现

- Aspects: 提供了与AspectJ的集成功能

- Instrumentation: 提供了类植入（Instrumentation）的支持和类加载器的实现
- Messaging: 用于构建基于消息的应用程序



###### Core Container

- Beans: Bean工厂与bean的装配
- Core: 依赖注入IoC与DI的最基本实现
- Content: IOC容器的企业服务扩展
- SpEl: 用于在运行时查询和操纵对象的表达式

###### Test

- 支持使用 JUnit 和 TestNG 对 Spring 组件进行测试



#### 3. Spring优点

##### 3.1 IOC解耦

- 可以将对象间的依赖关系交由spring管理
- 避免硬编码造成的程序间过渡耦合

##### 3.2 支持AOP

- 可以使用切面编程思想对方法进行增强

##### 3.3 支持声明式事务

- 可以通过配置或者注解的方式管理事务
- 不需要硬编码管理事务

##### 3.4 方便测试

- 可以通过注解方便的测试Spring程序

##### 3.5 方便集成

- 其内部提供了对各种优秀框架的直接支持

##### 3.6 使用简单

- Spring对很多难用的API做了简单的封装

##### 2.7 设计精良

- Spring的源码设计精妙、结构清晰值得学习



#### 小结

- Spring是什么?

  - 分层的轻量级开源框架

- 至少说出3个Spring的优点
  - 解耦
  - 支持aop
  - 支持声明式事务



### 06IOC容器概念【理解】

#### 目标

- 理解IOC是什么
- IOC有哪些作用



#### 1. IOC是什么

- IOC(Inversion Of Control): 将对象的创建(控制)转交给第三方(工厂)完成, 也叫 **控制反转**

![1564726290419](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1564726290419.png) 

#### 2. IOC的作用

1. **解耦**: 利用IOC的工厂模式解耦创建对象的过程
   - 解决代码耦合度过高问题
2. **存储对象**: 可以将创建好的对象 **存储** 起来重复使用 
   - 解决对象个数问题
3. **管理依赖关系**: 可以将依赖对象注入需要的对象当中
   - 解决依赖关系问题
4. **管理对象创建顺序**: 可以根据依赖关系先后创建对象
   - 解决创建顺序问题



#### 小结

- IOC是什么?
  - 控制反转: 将对象创建交给第三方(工厂)完成
- IOC有什么作用?
  - 解耦, 存储对象, 管理依赖关系, 管理对象的创建顺序



### 07IOC入门案例【掌握】

#### 目标

- 了解IOC容器的依赖的jar包

- 掌握创建对象的Bean标签
- 掌握创建IOC容器的代码



#### 1. IOC容器依赖的jar包

1. 工程名称: spring01_ioc_02

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring01_ioc_02</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
   
       <dependencies>
           <!-- 1. Spring IOC的依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
       </dependencies>
   </project>
   ```



#### 2. IOC容器的Bean标签

1. 创建实体: com.itheima.ioc.User

   ```java
   package com.itheima.ioc;
   
   import java.util.Date;
   
   /**
    * 用户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class User {
   
       private Integer id;
       private String username;
       private Date birthday;
       private String sex;
       private String address;
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", username='" + username + '\'' +
                   ", birthday=" + birthday +
                   ", sex='" + sex + '\'' +
                   ", address='" + address + '\'' +
                   '}';
       }
   }
   
   ```

2. 定义对象: beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
       <!-- 1. 定义对象的字节码 -->
       <!--
           bean: 定义对象
               id: 对象名称 (唯一)
               class: 全限定类名
       -->
       <bean id="user" class="com.itheima.ioc.User"/>
   </beans>
   ```



#### 3. IOC容器的创建案例

1. 单元测试: IocTests

   ```java
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 测试IOC代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class IocTests {
   
   
       /**
        * 启动Spring框架 (创建IOC容器)
        * @param args
        */
       public static void main(String[] args) {
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
           // 2. 使用对象
           Object user = ioc.getBean("user");
           System.out.println(user);
       }
   }
   
   ```





#### 小结

- 使用IOC容器需要添加哪些依赖?
  - spring-context: aop, core, beans, expression

- 在配置文件中使用什么标签定义对象?
  - bean标签

- 如何创建IOC容器?
  - new ClassPathXmlApplicationContext(..)



### 08Bean-标签属性【了解】

#### 目标

- 了解bean标签的属性



#### 1. bean标签的属性

1. 工程名称: spring01_xml_03

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring01_xml_03</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. Spring IOC -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. Junit -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   </project>
   ```

3. 提供方法: com.itheima.xml.User

   ```java
   package com.itheima.xml;
   
   import java.util.Date;
   
   /**
    * 用户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class User {
       private Integer id;
       private String username;
       private Date birthday;
       private String sex;
       private String address;
   
       public User() {
           System.out.println("构造方法执行..");
       }
       public void init() {
           System.out.println("初始化方法执行..");
       }
       public void destroy() {
           System.out.println("销毁方法执行..");
       }
   
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getUsername() {
           return username;
       }
   
       public void setUsername(String username) {
           this.username = username;
       }
   
       public Date getBirthday() {
           return birthday;
       }
   
       public void setBirthday(Date birthday) {
           this.birthday = birthday;
       }
   
       public String getSex() {
           return sex;
       }
   
       public void setSex(String sex) {
           this.sex = sex;
       }
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", username='" + username + '\'' +
                   ", birthday=" + birthday +
                   ", sex='" + sex + '\'' +
                   ", address='" + address + '\'' +
                   '}';
       }
   }
   
   ```

4. 属性配置: beans.xml

   ```xml
       <!--
           不取名称可以根据类型获取 (前提是: 容器中只有一个类型)
           lazy-init: 延迟创建对象 (默认值是false: 不延迟)
               true: 延迟创建 (延迟到使用时)
       -->
       <bean class="com.itheima.xml.User" init-method="init" destroy-method="destroy" lazy-init="true"/>
   ```

5. 单元测试: XmlTests

   ```java
       @Test
       public void testMethod (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
           System.out.println("3. 容器已经创建..");
           // 2. 使用对象 (也可以根据别名获取)
           Object user = ioc.getBean("user4");
           System.out.println(user);
           // 3. 销毁容器
           ioc.close();
       }
   ```

| 属性           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| id             | 对象的引用名称;一定要唯一； 一次只能定义一个引用名称         |
| name           | 对象的引用名称; 与id区别是：name一次可以定义多个引用名称。   |
| class          | 类的全限定名称                                               |
| init-method    | 指定类中初始化方法的名称，在构造方法执行完毕后立即执行【了解】 |
| destroy-method | 指定类中销毁方法名称，在销毁spring容器前执行【了解】         |
| lazy-init      | 设置为true表示在第一次使用对象的时候才创建，只对单例对象有效。 |
| scope          | 设置bean的作用范围, 取值：<br/>singleton：单例, 默认值; <br/>prototype：多例 <br/>request：web项目中，将对象存入request域中【了解】<br/>session：web项目中，将对象存入session域中【了解】<br/>globalsession：web项目中，将对象应用于集群环境，没有集群相当于session【了解】 |



#### 小结

- 至少说出3个bean标签的属性?
  - id, class, name, init-method, destroy-method, lazy-init

- Bean默认在什么时候创建的?
  - 默认是在容器创建的时候创建的



### 09Bean-作用范围【理解】

#### 目标

- 理解Bean的作用范围



#### 1. Bean的作用范围

1. 配置范围: beans.xml

   ```xml
   <!--
           scope: 设置对象的作用范围
               singleton: 对象是单例模式 (默认)
               prototype: 设置为多例模式 (每次使用都创建一个: 不会存储: 不会执行销毁方法)
       -->
   <bean class="com.itheima.xml.User" init-method="init" destroy-method="destroy" scope="prototype"/>
   ```

2. 单元测试: XmlTests

   ```java
       @Test
       public void testScope (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("beans.xml");
           System.out.println("3. 容器已经创建..");
           // 2. 使用对象 (也可以根据别名获取)
           Object user1 = ioc.getBean(User.class);
           Object user2 = ioc.getBean(User.class);
           // 多例返回false, 单例true
           System.out.println(user1==user2);
           // 3. 销毁容器
           ioc.close();
       }
   ```



#### 小结

- 单例与多例的区别?
  - 单例只会创建一次对象 , 多例是每次使用时都会创建新的对象



### 10创建IOC的方式【理解】

#### 目标

- 了解创建IOC常用的3种方式



#### 1. 创建IOC常用的3种方式

- IOC容器接口结构图

![1560327236910](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1560327236910.png) 

##### 1.1 资源文件创建方式

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
```



##### 1.2 系统文件创建方式

```java
FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext("E:\\课堂\\Spring\\第1天\\04代码\\spring1\\spring-day01-ioc\\src\\main\\resources\\beans.xml");
```



##### 1.3 注解配置创建方式

```java
@Test
public void testAnnotation() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
}

@Configuration
static class Config{

    @Bean("user")
    public User createUser(){
        return new User();
    }
}
```



##### 1.4 ~~BeanFactory~~创建方式

```java
Resource resource = new ClassPathResource("beans.xml");
XmlBeanFactory context = new XmlBeanFactory(resource);
```

- 【了解】BeanFactory与ApplicationContext的区别?
  - BeanFactory是Spring容器的顶层接口, 采用 **延迟创建** 对象的思想 
  - ApplicationContext是BeanFactory的子接口, 采用 **即时创建** 对象的思想 



#### 小结

- 创建IOC容器的常用方式有哪几种?

  - new ClassPathXmlApplicationContext
  - new FileSystemXmlApplicationContext

  - new AnnotationConfigApplicationContext



### 11创建对象的方式【理解】

#### 目标

- **构造方法创建** (默认:  调用构造方法)
- **静态方法创建** (调用工具类静态方法)
- **实例方法创建** (调用对象的实例方法)



#### 1. 构造方法创建

1. 构造方法: com.itheima.ioc.User

   ```java
   public User() {
       System.out.println("1. 构造方法执行..");
   }
   ```

2. 添加配置: beans.xml

   ```xml
   <!-- 调用构造方法创建对象 -->
   <bean id="user" class="com.itheima.ioc.User"/>
   ```

#### 2. 静态方法创建

1. 添加静态方法: com.itheima.ioc.UserFactory

   ```java
   package com.itheima.ioc;
   
   import com.itheima.xml.User;
   
   /**
    * 创建用户的工具类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class UserFactory {
   
   
       /**
        * 静态方法.
        *
        * @return the user
        */
       public static User getBean(){
           return new User();
       }
   
       /**
        * 实例方法.
        * @return
        */
       public User get(){
           return new User();
       }
   }
   
   ```

2. 配置静态方法: beans.xml

   ```xml
   <bean id="user" class="UserFactory" factory-method="getBean"/>
   ```



#### 3. 实例方法创建

1. 配置实例和方法: beans.xml

   ```xml
   <bean id="userFactory" class="com.itheima.ioc.UserFactory"/>
   <!--
           创建对象的方式:
               3. 调用实例方法创建对象
               factory-bean: 实例方法所在的对象
               factory-method: 实例方法名
       -->
   <bean id="user" factory-bean="userFactory" factory-method="get"/>
   ```



#### 小结

- 创建对象的方式有哪几种?
  - 调用构造方法
  - 调用静态方法
  - 调用实例方法





### 12依赖注入 - 概念【掌握】

#### 目标

- 理解什么是依赖注入



#### 1. 什么是依赖注入

- 如何在IOC创建对象后给对象赋值?
- IOC提供了这种赋值功能: 依赖注入( **DI** : Dependency Injection)

![1560332836105](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/1560332836105.png) 

#### 小结

- 什么是依赖注入 ( **DI** )?
  - DI: Spring IOC提供的给对象赋值的功能



### 13DI - 注入方式【理解】

#### 目标

- 掌握依赖注入的两种方式



#### 1. 构造方法赋值

1. 创建实体: com.itheima.xml.Person

   ```java
   package com.itheima.xml;
   
   /**
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Person {
   
       private Integer id;
       private String name;
   
       public Person() {
       }
   
       public Person(Integer id, String name) {
           this.id = id;
           this.name = name;
       }
   
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       @Override
       public String toString() {
           return "Person{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   '}';
       }
   }
   
   ```

2. 配置注入: beans.xml

   ```xml
       <!--
           依赖注入:
               1. constructor-arg: 调用构造方法注入参数值
                      name: 参数列表中的参数名称
                      value: 需要注入的内容
                      index: 参数列表的下标值
   
       -->
       <bean class="com.itheima.xml.Person">
           <constructor-arg name="id" value="1"/>
           <constructor-arg index="1" value="Jason"/>
       </bean>
   ```

#### 2. set方法赋值(常用)

```xml
<!--
        依赖注入:
            1. property: 调用set方法注入参数值
                   name: 参数列表中的参数名称
                   value: 需要注入的内容
    -->
<bean class="com.itheima.xml.Person">
    <property name="id" value="2"/>
    <property name="name" value="Jason2"/>
</bean>
```

#### 3. C标签代替构造方法

1. 引入C名称空间

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          <!-- c标签命名空间 -->
          xmlns:c="http://www.springframework.org/schema/c"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   ```

2. 使用c标签

   ```xml
       <!--
           本质上是调用构造方法注入
               c:id: 注入id参数的值
               c:name: 注入name参数的值
               c:_1: 指定参数列表中的下标注入值
       -->
       <bean class="com.itheima.xml.Person" c:id="3" c:_1="Jason3" />
   ```

#### 4. P标签代替set方法

1. 引入P名称空间

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:c="http://www.springframework.org/schema/c"
          <!-- p标签命名空间 -->
          xmlns:p="http://www.springframework.org/schema/p"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   ```

2. 使用p标签

   ```xml
   <!--
           本质上是调用set方法注入
               p:id: 注指定参数名称注入值
               p:name: 指定参数名称注入值
       -->
   <bean class="com.itheima.xml.Person" p:id="4" p:name="Jason4" />
   ```



#### 小结

- 依赖注入有哪几种方式?
  - 构造方法注入
  - set方法注入



### 14DI - 注入对象【理解】

#### 目标

- 掌握注入bean的方式



#### 1. 注入对象属性

1. 配置需要注入的对象

   ```xml
   <!-- 定义需要注入的对象
           class: 对象的全限定类名
        -->
   <bean id="str" class="java.lang.String">
       <constructor-arg value="Jason5"/>
   </bean>
   ```

2. 注入bean对象属性值

   ```xml
   <!--
           ref: 引用一个已经存在的对象
   
       -->
   <bean class="com.itheima.xml.Person">
       <property name="name" ref="str"/>
   </bean>
   ```



#### 小结

- ref标签属性的作用是什么?
  - 引用一个已经存在的对象



### 15DI - 注入集合【理解】

#### 目标

- 理解注入集合的方式



#### 1. 注入集合属性

1. 创建实体: com.itheima.xml.Employee

   ```java
   package com.itheima.xml;
   
   import java.util.*;
   
   /**
    * 员工类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Employee {
   
       private String[] array;
       private List<String> list;
       private Set<String> set;
       private Map<String, String> map;
       private Properties props;
   
   
       public String[] getArray() {
           return array;
       }
   
       public void setArray(String[] array) {
           this.array = array;
       }
   
       public List<String> getList() {
           return list;
       }
   
       public void setList(List<String> list) {
           this.list = list;
       }
   
       public Set<String> getSet() {
           return set;
       }
   
       public void setSet(Set<String> set) {
           this.set = set;
       }
   
       public Map<String, String> getMap() {
           return map;
       }
   
       public void setMap(Map<String, String> map) {
           this.map = map;
       }
   
       public Properties getProps() {
           return props;
       }
   
       public void setProps(Properties props) {
           this.props = props;
       }
   
       @Override
       public String toString() {
           return "Employee{" +
                   "array=" + Arrays.toString(array) +
                   ", list=" + list +
                   ", set=" + set +
                   ", map=" + map +
                   ", props=" + props +
                   '}';
       }
   }
   
   ```

2. 演示注入: beans.xml

   ```xml
       <!--
           array: 表示参数的类型名称
               value: 需要注入的值
               entry: 需要注入的entry对象 (key/value)
               prop: 需要注入的prop对象值
   
       -->
       <bean class="com.itheima.xml.Employee">
           <property name="array">
               <array>
                   <value>1</value>
                   <value>2</value>
                   <value>3</value>
               </array>
           </property>
           <property name="list">
               <list>
                   <value>3</value>
                   <value>4</value>
               </list>
           </property>
           <property name="set">
               <set>
                   <value>5</value>
               </set>
           </property>
           <property name="map">
               <map>
                   <entry key="id" value="6"/>
                   <entry key="name" value="Jason6"/>
               </map>
           </property>
           <property name="props">
               <props>
                   <prop key="id">7</prop>
                   <prop key="name">Jason7</prop>
               </props>
           </property>
       </bean>
   ```



#### 小结

- 注入了那些集合属性?
  - array,list,set,map,props







### 16基于注解创建对象【掌握】

#### 目标

- 使用@Component注解创建对象



#### 1. 使用注解创建对象

1. 工程名称: spring01_anno_04

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring01_anno_04</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. Spring IOC依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. Junit单元测试依赖 -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 创建实体: com.itheima.anno.Account

   ```java
   package com.itheima.anno;
   
   import org.springframework.stereotype.Component;
   
   /**
    * @Component: 修饰类
    *  作用: 将对象创建并加入到IOC容器 <bean id= class...
    *      value: 给对象取名: 对象名称  : 默认名称是首字母小写的类名
    */
   @Component
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

4. 添加配置: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
   
       <!--
           component-scan: Spring组件扫描配置
               base-package: 指定扫描的路径
       -->
       <context:component-scan base-package="com.itheima.anno"/>
   
   
   
       <!-- 因为已经被@Component代替了 -->
       <!--<bean id="account" class="com.itheima.anno.Account"/>-->
   
   
   
   
   </beans>
   ```

5. 单元测试: AnnoTests

   ```java
   import org.junit.Test;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 注解代码测试.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AnnoTests {
   
   
       @Test
       public void testComponent (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 使用对象
           Object account = ioc.getBean("account");
           System.out.println(account);
       }
   }
   
   ```





#### 小结

- @Component的作用?
  - 代替bean标签的 (创建对象并加入IOC容器中)

- 使用注解需要写什么配置?
  - <context:component-scan base-package="com.itheima.anno"/>



### 17创建对象的注解【掌握】

#### 目标

- 创建对象的其他注解



#### 1. 创建对象的其他注解

- 为了在3层架构中识别不同层的对象延伸了3个注解

1. 注解演示: com.itheima.anno.Account

   ```java
   package com.itheima.anno;
   
   import org.springframework.stereotype.Component;
   import org.springframework.stereotype.Controller;
   import org.springframework.stereotype.Repository;
   import org.springframework.stereotype.Service;
   
   /**
    * @Component: 修饰类
    * @Repository: 修饰类, 表示持久层对象
    * @Service: 修饰类, 表示业务层对象
    * @Controller: 修饰类, 表示控制层对象
    *  作用: 将对象创建并加入到IOC容器 <bean id= class...
    *      value: 给对象取名: 对象名称  : 默认名称是首字母小写的类名
    */
   //@Component
   //@Repository
   //@Service
   @Controller// ("account2")
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```



#### 小结

- 在3层架构中延伸出了哪3个注解?
  - @Repository, @Service, @Controller



### 18依赖注入的注解【掌握】

#### 目标

- 了解依赖注入的前提
- 使用依赖注入的注解



#### 1. 依赖注入的前提

- 需要将对象交由IOC容器管理



#### 2. 使用注解注入值

1. 创建实体: com.itheima.anno.User

   ```java
   package com.itheima.anno;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.beans.factory.annotation.Qualifier;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.stereotype.Component;
   
   import javax.annotation.Resource;
   import java.util.Date;
   
   /**
    * User.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Component
   public class User {
   
       @Value("1")
       private Integer id;
       /**
        * @Value: 修饰属性,
        *  作用: 注入一个固定的参数值 , 代替的是bean标签中的prototype子标签中的value属性
        */
   //    @Value("Jason")
   
       /**
        * @Autowired: 修饰属性, 方法
        *  作用: 注入一个已经存在的对象, 代替的是bean标签中的prototype子标签中的ref属性
        *  细节: 根据属性名称+类型注入参数值
        * @Qualifier: 修饰属性, 方法  (一般配合@Autowired使用)
        *  细节: 根据对象名称注入值 (一但使用就只会根据对象名称注入值)
        */
   
   //    @Autowired
   //    @Qualifier("username2")
       /**
        * jdk1.8之后已经去除 (以防企业中还在用)
        */
       @Resource(name = "username2")
       private String username;
       private Date birthday;
       private String sex;
       private String address;
   
   
   
   //    @Autowired
       public void setUsername(String username) {
           this.username = username;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", username='" + username + '\'' +
                   ", birthday=" + birthday +
                   ", sex='" + sex + '\'' +
                   ", address='" + address + '\'' +
                   '}';
       }
   }
   
   ```

2. 配置对象: applicationContext.xml

   ```xml
       <!-- 因为已经被@Component代替了 -->
       <!--<bean id="account" class="com.itheima.anno.Account"/>-->
   
       <!-- 定义需要注入的对象 -->
       <bean id="username1" class="java.lang.String">
           <constructor-arg index="0" value="Jason1"/>
       </bean>
       <bean id="username2" class="java.lang.String">
           <constructor-arg index="0" value="Jason2"/>
       </bean>
   
   ```

3. 单元测试: AnnoTests

   ```java
       @Test
       public void testDi (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 使用对象
           Object account = ioc.getBean(User.class);
           System.out.println(account);
       }
   ```



#### 小结

- 使用依赖注入的前提是什么?
  - 将对象交给IOC容器管理
- 赋值(依赖注入)的注解有哪几个?
  - @Value
  - @Autowired
  - @Qualifier
  - @Resource



### 19生命周期的注解【了解】

#### 目标

- 了解生命周期的相关概念
- 了解生命周期的相关注解



#### 1. 生命周期的相关概念

- 生命周期: 对象的创建到销毁的过程
- 如: 创建、创建后、销毁前、销毁



#### 2. 生命周期的相关注解

1. 注解演示: com.itheima.anno.Account

   ```java
   package com.itheima.anno;
   
   import org.springframework.context.annotation.Lazy;
   import org.springframework.context.annotation.Scope;
   import org.springframework.stereotype.Component;
   import org.springframework.stereotype.Controller;
   import org.springframework.stereotype.Repository;
   import org.springframework.stereotype.Service;
   
   import javax.annotation.PostConstruct;
   import javax.annotation.PreDestroy;
   
   /**
    * @Component: 修饰类
    * @Repository: 修饰类, 表示持久层对象
    * @Service: 修饰类, 表示业务层对象
    * @Controller: 修饰类, 表示控制层对象
    *  作用: 将对象创建并加入到IOC容器 <bean id= class...
    *      value: 给对象取名: 对象名称  : 默认名称是首字母小写的类名
    */
   //@Component
   //@Repository
   //@Service
   @Controller// ("account2")
   /**
    * @Lazy: 修饰类
    *  作用: 将对象的创建延迟到使用时创建
    *      value: 设置是否懒加载, 默认值是true: 表示需要延迟加载
    *          false: 不需要懒加载
    *
    * @Scope: 修饰类
    *  作用: 设置对象的声明周期
    *      value: 指定对象的作用范围
    *  如果设置为prototype: 会影响生命周期的创建时间  (延迟到使用时创建)
    */
   //@Lazy(false)
   @Scope("prototype")
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
   
       public Account() {
           System.out.println("1. 构造方法执行..");
       }
   
       /**
        * @PostConstruct: 修饰方法
        *  修饰为一个初始化方法, 在构造方法之后执行, 代替了init-method属性
        */
       @PostConstruct
       public void init() {
           System.out.println("2. 初始化方法执行..");
       }
   
   
       /**
        * @PreDestroy: 修饰方法
        *  修饰为一个销毁方法, 在容器销毁之前执行, 代替了destroy-method属性
        */
       @PreDestroy
       public void destroy() {
           System.out.println("4. 销毁方法执行..");
       }
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

2. 单元测试: AnnoTests

   ```java
       @Test
       public void testScope (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           System.out.println("3. 容器已经创建..");
           // 2. 使用对象
           Object account = ioc.getBean(Account.class);
           System.out.println(account);
           // 3. 销毁容器
           ioc.close();
       }
   ```



#### 小结

- 影响对象生命周期的注解有哪些?
  - @PostConstruct: 修饰方法为一个初始化方法
  - @PreDestroy: 修饰方法为一个销毁方法
  - @Lazy: 修饰类, 将对象创建延迟到使用时创建 (影响了生命周期中的创建时间)
  - @Scope: 如果设置为prototype时会影响生命周期的创建时间(到使用时创建)



### 20注解与配置的关系【了解】

#### 目标

- 了解注解和配置的关系



#### 1. 注解和配置的关系

- 注解是配置的代替方案 (是配置的一种)

|              | 配置方式 (常用)                        | 注解方式 (常用)                |
| ------------ | -------------------------------------- | ------------------------------ |
| Bean定义     | <bean class=""/>                       | @Component                     |
| Bean名称     | bean标签的id或name属性                 | @Component的value值            |
| Bean注入     | bean标签的子标签<property..            | @Autowired(1) + @Qualifier (?) |
| Bean生命周期 | bean标签的init- \| destroy- method属性 | @PostConstruct \| @PreDestroy  |
| Bean延迟创建 | bean标签的lazy-init \| scope 属性      | @Lazy \| @Scope                |
| **适用场景** | 第三方源码类的对象创建                 | 自定义类的简单应用对象创建     |

- 注解的优势: 简单, 可读性高 (找到类就相当于找到配置)
- 配置的优势: 解耦, 非侵入性 (代码与配置分开互不干扰)





#### 小结

- 注解和配置是什么关系?
  - 注解是代替配置的一种方案
- 注解和配置能否混搭使用?
  - 可以, 企业中混搭最多





### 21总结

1. Spring是什么?
   - Spring是分层的轻量级开源框架
2. 什么是IOC ?
   - 控制反转: 将对象交给第三方创建 (完成)
3. 如何创建IOC容器 ?
   - new ClassPathXmlApplicationContext
   - new FileSystemXmlApplicationContext
   - new AnnotationConfigApplicationContext

4. 创建对象有哪几种方式?
   - 构造方法
   - 静态方法
   - 实例方法

5. IOC通过什么功能做到依赖关系管理?
   - DI
6. 什么是DI ?
   - 依赖注入: Spring IOC提供的给对象赋值的功能
7. 创建对象的注解有哪些?
   - @Component
   - @Repository
   - @Service
   - @Controller
8. 依赖注入的注解有哪些?
   - @Value: 注入固定的参数值
   - @Autowired: 注入已经存在的对象
   - @Qualifier: 根据对象名称注入值
   - @Resource: 相当于@Autowired+@Qualifier
9. 生命周期相关注解有哪些?
   - @PostConstruct: 修饰方法为初始化方法
   - @PreDestroy: 修饰方法为销毁方法
   - @Lazy: 修饰类延迟对象的创建时间到使用时
   - @Scope: 修饰类如果指定value值为prototype, 那么创建时间会延迟到使用时创建
10. 注解与配置是什么关系?
    - 注解是配置的代替方案 (本质上都是配置)



### 01复习

#### 目标

- 了解 Spring 相关概念
- 搭建 Spring 项目环境



#### 1. 了解 Spring 相关概念

1. Spring是什么?
   - Spring是分层的轻量级开源框架
2. Spring IOC是什么?
   - 控制反转: 将对象的创建交给第三方完成
3. DI是什么?
   - 依赖注入: Spring IOC提供的一种给对象赋值的功能



#### 2. 搭建 Spring 项目环境

1. 工程名称: spring02_jdbc_01

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring02_jdbc_01</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. Spring IOC依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. Spring JDBC依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 3. Mysql驱动依赖 -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <!-- 建议同学们使用5.x -->
               <version>8.0.17</version>
           </dependency>
           <!-- 4. JUnit单元测试 -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <!-- 5. Log4j日志依赖 -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>1.7.7</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
               <version>1.7.26</version>
           </dependency>
       </dependencies>
   </project>
   ```

3. 创建实体: com.itheima.jdbc.Account

   ```java
   package com.itheima.jdbc;
   
   /**
    * 账户类 (数据模型).
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Account {
   
       private Integer id;
       private Integer uid;
       private Double money;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public Integer getUid() {
           return uid;
       }
   
       public void setUid(Integer uid) {
           this.uid = uid;
       }
   
       public Double getMoney() {
           return money;
       }
   
       public void setMoney(Double money) {
           this.money = money;
       }
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

4. 添加配置: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
       <!-- 1. 定义对象: 创建对象并加入IOC容器 -->
       <bean class="com.itheima.jdbc.Account"/>
   
   </beans>
   ```

5. 日志配置: log4.properties

   ```properties
   # 日志级别, 输出位置
   log4j.rootLogger=DEBUG, stdout
   # 控制台输出器
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   # 布局处理器
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   # 格式转换器
   log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
   ```

6. 环境测试: JdbcTests.java

   ```java
   import com.itheima.jdbc.Account;
   import org.junit.Test;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 测试JDBC操作.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class JdbcTests {
   
       @Test
       public void testEnv (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 使用对象
           Account account = ioc.getBean(Account.class);
           System.out.println(account);
       }
   }
   
   ```



#### 小结

- 注解与配置的关系是什么?
  - 注解是代替配置的方案
- 3层架构中哪些需要Spring?
  - 三层都需要



### 02使用JdbcTemplate【掌握】

#### 目标

- 了解JdbcTemplate概念
- 编写JdbcTemplate案例



#### 1. JdbcTemplate概念

-  SpringJDBC **封装了基础的 JDBC 操作**，让我们不用去关心 获取驱动、建立连接、关闭连接等非业务操作，让我们更加专注于业务的实现。 
-  SpringJDBC 采用的是 **模板设计模式**, 将 JDBC 封装在 **JdbcTemplate** 模板中, 使操作更简单。



#### 2. JdbcTemplate案例

1. 需求: ==查询所有账户==

2. 数据: account.sql

   ```sql
   DROP TABLE IF EXISTS `account`;
   CREATE TABLE `account` (
     `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
     `uid` int(11) DEFAULT '1' COMMENT '用户编号',
     `money` decimal(10,2) DEFAULT '0.00' COMMENT '余额',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
   
   INSERT INTO `account` VALUES ('1', '1', '10.00');
   INSERT INTO `account` VALUES ('2', '10', '0.00');
   INSERT INTO `account` VALUES ('3', '24', '99.00');
   ```

3. 测试: JdbcTests

   ```java
       @Test
       public void testJdbc (){
           // 1. 创建模板对象
           JdbcTemplate jdbcTemplate = new JdbcTemplate();
   
           // 2. 提供数据源
           DriverManagerDataSource ds = new DriverManagerDataSource();
           ds.setDriverClassName("com.mysql.jdbc.Driver");
           ds.setUrl("jdbc:mysql:///mybatisdb?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true&characterEncoding=UTF-8");
           ds.setUsername("root");
           ds.setPassword("root");
           jdbcTemplate.setDataSource(ds);
   
           // 3. 定义sql语句
           String sql = "select * from account";
   
   
           // 4. 操作数据库 (返回结果对象)
           List<Account> all = jdbcTemplate.query(sql, new BeanPropertyRowMapper(Account.class));
   
   
           // 5. 处理结果
           all.forEach(x -> System.out.println(x));
       }
   ```



#### 小结

- JdbcTemplate是什么?
  - 是Spring封装的操作数据库的模板工具
- 案例中如何注入连接池?
  - set方式





### 03使用IOC管理Jdbc【了解】

#### 目标

- 使用IOC管理jdbcTemplate



#### 1. 使用IOC管理

1. 添加配置: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
       <!-- 1. 定义对象: 创建对象并加入IOC容器 -->
       <bean class="com.itheima.jdbc.Account"/>
   
   
   
       <!-- 2. 创建模板对象 -->
       <bean id="jdbcTemplate"  class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!-- 3. 创建数据源对象 (Spring连接池) -->
       <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql:///mybatisdb?serverTimezone=UTC&amp;useSSL=false&amp;allowPublicKeyRetrieval=true&amp;characterEncoding=UTF-8"/>
           <property name="username" value="root"/>
           <property name="password" value="root"/>
       </bean>
   </beans>
   ```

2. 单元测试: JdbcTests

   ```java
       /**
        * 使用Spring内部连接池操作数据库
        */
       @Test
       public void testIocJdbc (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);
   
           // 2. 定义sql语句
           String sql = "select * from account";
   
   
           // 3. 操作数据库 (返回结果对象)
           List<Account> all = jdbcTemplate.query(sql, new BeanPropertyRowMapper(Account.class));
   
   
           // 4. 处理结果
           all.forEach(x -> System.out.println(x));
       }
   ```



#### 小结

- 如何配置Spring的连接池?
  - 使用bean标签配置再使用property注入
- BeanPropertyRowMapper的作用是什么?
  - 将结果集转换成对象



### 04自定义映射器【理解】

#### 目标

- 结果集映射器的前提
- 自定义结果集映射器



#### 1. 结果集映射器的前提

- 类名: org.springframework.jdbc.core.BeanPropertyRowMapper
- 接口: **org.springframework.jdbc.core.RowMapper**
- 作用: 将结果集( ResultSet )映射到对象( Bean )
- **前提**: 数据库表字段需要与对象属性一致, 否则不起作用



#### 2.自定义结果集映射器

1. 实现类: com.itheima.jdbc.AccountRowMapper

   ```java
   package com.itheima.jdbc;
   
   
   import org.springframework.jdbc.core.RowMapper;
   
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   /**
    * 账户与行数据的映射器.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountRowMapper implements RowMapper<Account> {
   
   
       /**
        * jdbc操作完成之后会遍历结果集
        *  每次获得一行都会调用该方法
        * @param rs 结果集
        * @param i 行号 (0开始)
        * @return
        * @throws SQLException
        */
       @Override
       public Account mapRow(ResultSet rs, int i) throws SQLException {
           System.out.println("=====================第"+i+"行========================");
           // 1. 创建对象
           Account account = new Account();
           // 2. 封装数据
           account.setId(rs.getInt("id"));
           account.setUid(rs.getInt("uid"));
           account.setMoney(rs.getDouble("money"));
           // 3. 返回结果
           return account;
       }
   }
   
   ```

2. 测试类: JdbcTests

   ```java
       @Test
       public void testCj (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);
   
           // 2. 定义sql语句
           String sql = "select * from account";
   
   
           // 3. 操作数据库 (返回结果对象)
           List<Account> all = jdbcTemplate.query(sql, new AccountRowMapper());
   
   
           // 4. 处理结果
           all.forEach(x -> System.out.println(x));
       }
   ```




#### 小结

- 自定义结果集映射器的好处是什么?
  - 可以解决字段与属性名称不一致的问题



### 05完成CRUD - 操作【理解】

#### 目标

- 创建JdbcTemplate 环境
- 创建3层架构的CRUD **代码**



#### 1. 创建CRUD环境

1. 创建工程: spring02_crud_02

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring02_crud_02</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. Spring IOC依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. Spring JDBC依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 3. Druid连接池依赖 -->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.1.12</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.17</version>
           </dependency>
           <!-- 4. Junit单元测试依赖 -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <!-- 5. Log4j日志工具依赖 -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>1.7.7</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
               <version>1.7.26</version>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 创建实体: com.itheima.domain.Account


```java
 package com.itheima.domain;

/**
    * 账户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Account {
   private Integer id;
   private Integer uid;
   private Double money;
   
   public Integer getId() {
       return id;
   }
   
   public void setId(Integer id) {
       this.id = id;
   }
   
   public Integer getUid() {
       return uid;
   }
   
   public void setUid(Integer uid) {
       this.uid = uid;
   }
   
   public Double getMoney() {
       return money;
   }
   
   public void setMoney(Double money) {
       this.money = money;
   }
   
   @Override
   public String toString() {
       return "Account{" +
               "id=" + id +
               ", uid=" + uid +
               ", money=" + money +
               '}';
   }
```

   }

   ```java
   
   

#### 2. 添加CRUD操作

1. 实现持久层: com.itheima.crud.dao.impl.AccountDaoImpl

    ```java
    package com.itheima.crud.dao.impl;
    
    import com.itheima.domain.Account;
    import org.springframework.jdbc.core.BeanPropertyRowMapper;
    import org.springframework.jdbc.core.JdbcTemplate;
    
    /**
     * 账户持久层实现类.
     *
     * @author : Jason.lee
     * @version : 1.0
     */
    public class AccountDaoImpl {
    
        JdbcTemplate jdbcTemplate;
    
        /**
         * 需要通过set方法注入参数值.
         *
         * @param jdbcTemplate the jdbc template
         */
        public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
            this.jdbcTemplate = jdbcTemplate;
        }
    
    
        /**
         * 保存新的账户.
         *
         * @param account 账户信息
         */
        public void save(Account account){
            String sql = "insert into account values(?, ?, ?)";
            jdbcTemplate.update(sql, account.getId(), account.getUid(), account.getMoney());
        }
        public Account findById(Account account){
            String sql = "select * from account where id=?";
            return jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Account.class), account.getId());
        }
        public void update(Account account){
            String sql = "update account set money=? where id=?";
            jdbcTemplate.update(sql, account.getMoney(), account.getId());
        }
        public void del(Account account){
            String sql = "delete from account where id=?";
            jdbcTemplate.update(sql, account.getId());
        }
    }
    
   ```

2. 实现业务层: com.itheima.crud.service.impl.AccountServiceImpl

   ```java
   package com.itheima.crud.service.impl;
   
   import com.itheima.crud.dao.impl.AccountDaoImpl;
   import com.itheima.domain.Account;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountServiceImpl {
   
   
       AccountDaoImpl accountDao;
   
       /**
        * 需要通过set方法进行依赖注入
        * @param accountDao
        */
       public void setAccountDao(AccountDaoImpl accountDao) {
           this.accountDao = accountDao;
       }
   
       public void save(Account account){
           // 1. 业务逻辑处理
   
           // 2. 操作数据库
           accountDao.save(account);
       }
       public void findById(Account account){
           accountDao.findById(account);
       }
       public void update(Account account){
           accountDao.update(account);
       }
       public void del(Account account){
           accountDao.del(account);
       }
   
   }
   
   ```




#### 小结

- 为什么要给jdbcTemplate提供set方法?
  - 需要使用set方法进行依赖注入
- jdbcTemplate操作是否可以进行测试?
  - 不能, 对象还没有创建, 容器还没有启动



### 06完成CRUD - 配置【掌握】

#### 目标

- 加载外部配置文件
- 配置IOC依赖注入



#### 1. 加载外部配置文件

1. 添加配置文件: db.properties

   ```properties
   db.driver=com.mysql.jdbc.Driver
   # 针对Mysql 8.x数据库的参数
   #   serverTimezone: 指定时区(UTC)
   #   useSSL: 指定是否使用加密安全连接(false)
   #   allowPublicKeyRetrieval: 是否允许检索公钥(true)
   db.url=jdbc:mysql:///mybatisdb?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
   db.username=root
   db.password=root
   ```

2. 加载外部文件: applicationContext.xml

   ```xml
   <!--
       context:property-placeholder: 加载指定的资源文件
           location: 本地资源文件路径
   -->
   <context:property-placeholder location="classpath:db.properties"/>
   ```



#### 2. 配置IOC依赖注入

1. 添加配置: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
   
       <!-- 1. 加载外部配置文件 -->
       <!--
           property-placeholder: 加载指定的配置文件
               location: 本地文件路径
               classpath: 只要在spring配置中, 所有文件路径都需要在文件名前面加上该标识
       -->
       <context:property-placeholder location="classpath:db.properties"/>
       <!-- 2. 创建数据源 (连接池) -->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="${db.driver}"/>
           <property name="url" value="${db.url}"/>
           <property name="username" value="${db.username}"/>
           <property name="password" value="${db.password}"/>
       </bean>
       <!-- 3. 创建JDBC操作模板 -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!-- 4. 创建持久层对象 -->
       <bean id="accountDao" class="com.itheima.crud.dao.impl.AccountDaoImpl">
           <property name="jdbcTemplate" ref="jdbcTemplate"/>
       </bean>
       <!-- 5. 创建业务层对象 -->
       <bean id="accountService" class="com.itheima.crud.service.impl.AccountServiceImpl">
           <property name="accountDao" ref="accountDao"/>
       </bean>
   </beans>
   ```

2. 单元测试: CrudTests

   ```java
   import com.itheima.crud.service.impl.AccountServiceImpl;
   import com.itheima.domain.Account;
   import org.junit.Test;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 测试完整的CRUD操作.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class CrudTests {
   
   
       @Test
       public void testSave (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
       @Test
       public void testFindBy (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           Account acc = accountService.findById(account);
           System.out.println(acc);
       }
       @Test
       public void testUpdate (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setMoney(11D);
           accountService.update(account);
       }
       @Test
       public void testDel (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           accountService.del(account);
       }
   }
   
   ```



#### 小结

- Spring主配置文件中`${}`符号的作用?
  - 获取配置文件中的参数值

- jdbcTemplate是通过什么方法注入的?
  - set方式



### 07使用注解改造案例【掌握】

#### 目标

- 使用注解改造CRUD案例工程



#### 1. 改造CRUD案例工程

1. 改造工程: spring02_ax_03

2. 改造持久层: com.itheima.crud.dao.impl.AccountDaoImpl

   ```java
   package com.itheima.crud.dao.impl;
   
   import com.itheima.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.jdbc.core.BeanPropertyRowMapper;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.stereotype.Repository;
   
   /**
    * 账户持久层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Repository
   public class AccountDaoImpl {
   
   
       @Autowired
       JdbcTemplate jdbcTemplate;
   
   
       /**
        * 保存新的账户.
        *
        * @param account 账户信息
        */
       public void save(Account account){
           String sql = "insert into account values(?, ?, ?)";
           jdbcTemplate.update(sql, account.getId(), account.getUid(), account.getMoney());
       }
       public Account findById(Account account){
           String sql = "select * from account where id=?";
           return jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Account.class), account.getId());
       }
       public void update(Account account){
           String sql = "update account set money=? where id=?";
           jdbcTemplate.update(sql, account.getMoney(), account.getId());
       }
       public void del(Account account){
           String sql = "delete from account where id=?";
           jdbcTemplate.update(sql, account.getId());
       }
   }
   
   ```

3. 改造业务层: com.itheima.crud.service.impl.AccountServiceImpl

   ```java
   package com.itheima.crud.service.impl;
   
   import com.itheima.crud.dao.impl.AccountDaoImpl;
   import com.itheima.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Service
   public class AccountServiceImpl {
   
       @Autowired
       AccountDaoImpl accountDao;
   
       public void save(Account account){
           // 1. 业务逻辑处理
   
           // 2. 操作数据库
           accountDao.save(account);
       }
       public Account findById(Account account){
           return accountDao.findById(account);
       }
       public void update(Account account){
           accountDao.update(account);
       }
       public void del(Account account){
           accountDao.del(account);
       }
   
   }
   
   ```

4. 添加注解扫描: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 使用了注解需要添加Spring组件扫描配置 -->
       <context:component-scan base-package="com.itheima.crud"/>
   
       <!-- 1. 加载外部配置文件 -->
       <!--
           property-placeholder: 加载指定的配置文件
               location: 本地文件路径
               classpath: 只要在spring配置中, 所有文件路径都需要在文件名前面加上该标识
       -->
       <context:property-placeholder location="classpath:db.properties"/>
       <!-- 2. 创建数据源 (连接池) -->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="${db.driver}"/>
           <property name="url" value="${db.url}"/>
           <property name="username" value="${db.username}"/>
           <property name="password" value="${db.password}"/>
       </bean>
       <!-- 3. 创建JDBC操作模板 -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   </beans>
   ```

5. 模拟视图层: CrudTests

   ```java
   import com.itheima.crud.service.impl.AccountServiceImpl;
   import com.itheima.domain.Account;
   import org.junit.Test;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 测试完整的CRUD操作.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class CrudTests {
   
   
       @Test
       public void testSave (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
       @Test
       public void testFindBy (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           Account acc = accountService.findById(account);
           System.out.println(acc);
       }
       @Test
       public void testUpdate (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setMoney(11D);
           accountService.update(account);
       }
       @Test
       public void testDel (){
           // 1. 创建IOC容器
           ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           accountService.del(account);
       }
   }
   
   ```



#### 小结

- 为什么accountService名称获取不到对象 ?
  - 因为注解创建对象的名称是首字母小写的类名



### 08纯注解开发【了解】

#### 目标

- 将剩余的配置使用注解代替
- 掌握纯注解开发的容器创建



#### 1. 使用注解代替剩余配置

- 配置文件的替代注解: @Configuration
- 注解扫描配置的替代注解: @ComponentScan
- 加载配置文件的替代注解: @PropertySource
- 导入配置文件的替代注解: @Import
- 第3方对象创建的替代注解: @Bean

1. com.itheima.crud.SpringConfig

   ```java
   package com.itheima.crud;
   
   import org.springframework.context.annotation.ComponentScan;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Import;
   
   /**
    * @Import: 修饰类
    *  作用: 导入其他的配置类
    */
   @Configuration
   @Import(JdbcConfig.class)
   @ComponentScan("com.itheima.crud")
   public class SpringConfig {
   
   
   }
   
   ```

2. com.itheima.crud.JdbcConfig

   ```java
   package com.itheima.crud;
   
   import com.alibaba.druid.pool.DruidDataSource;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   import javax.sql.DataSource;
   
   /**
    * @Configuration: 修饰类
    *  作用: 将类修饰为Spring的配置类  (代替配置文件的)
    * @PropertySource: 修饰类
    *  作用: 加载外部配置文件
    *      value: 配置文件的名称
    *
    *
    */
   @Configuration
   @PropertySource("classpath:db.properties")
   public class JdbcConfig {
   
       @Value("${db.driver}")
       String driver;
       @Value("${db.url}")
       String url;
       @Value("${db.username}")
       String username;
       @Value("${db.password}")
       String password;
   
   
       /**
        * @Bean: 修饰方法
        *  作用: 讲方法的返回值添加到IOC容器中
        */
       @Bean
       public DataSource dataSource(){
           DruidDataSource ds = new DruidDataSource();
           ds.setDriverClassName(driver);
           ds.setUrl(url);
           ds.setUsername(username);
           ds.setPassword(password);
           return ds;
       }
       @Bean("ds")
       public DataSource dataSource2(){
           DruidDataSource ds = new DruidDataSource();
           ds.setDriverClassName(driver);
           ds.setUrl(url);
           ds.setUsername(username);
           ds.setPassword(password);
           return ds;
       }
   
       /**
        * @Bean: 使用该注解修饰的方法如果需要参数, 将会从容器中获取
        */
       @Bean
       public JdbcTemplate jdbcTemplate(DataSource ds){
           return new JdbcTemplate(ds);
       }
   
   }
   ```



#### 2. 纯注解开发的容器创建

1. 创建注解实现的容器: AnnoTests

   ```java
   import com.itheima.crud.SpringConfig;
   import com.itheima.crud.service.impl.AccountServiceImpl;
   import com.itheima.domain.Account;
   import org.junit.Test;
   import org.springframework.context.annotation.AnnotationConfigApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   /**
    * 测试纯注解代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AnnoTests {
   
       @Test
       public void testSave (){
           // 1. 创建IOC容器
           AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(SpringConfig.class);
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
       @Test
       public void testFindBy (){
           // 1. 创建IOC容器
           AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(SpringConfig.class);
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           Account acc = accountService.findById(account);
           System.out.println(acc);
       }
       @Test
       public void testUpdate (){
           // 1. 创建IOC容器
           AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(SpringConfig.class);
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           account.setMoney(11D);
           accountService.update(account);
       }
       @Test
       public void testDel (){
           // 1. 创建IOC容器
           AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(SpringConfig.class);
           // 2. 获取业务层对象
           AccountServiceImpl accountService = ioc.getBean(AccountServiceImpl.class);
   
           Account account = new Account();
           account.setId(10);
           accountService.del(account);
       }
   }
   
   ```

   

#### 小结

- 请说出以下注解的作用?
  - @Configuration: 修饰类, 修饰为一个Spring配置类
  - @ComponentScan:  修饰类, 指定一个Spring组件扫描路径
  - @PropertySource: 修饰类, 加载外部配置文件
  - @Import: 修饰类, 导入其他Spring配置文件
  - @Bean: 修饰方法, 将方法的返回值添加到IOC容器



### 09整合Junit框架【了解】

#### 目标

- 了解Junit单元测试的问题
- 了解Spring测试框架的使用



#### 1. Junit单元测试的问题

- 每次测试都需要手动创建IOC容器



#### 2. Spring测试框架的使用

1. 添加依赖: pom.xml

   ```xml
   <!-- 4. Junit单元测试依赖 -->
   <!-- Spring-test框架 -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-test</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
   </dependency>
   ```

2. 单元测试: SpringTests

   ```java
   import com.itheima.crud.SpringConfig;
   import com.itheima.crud.service.impl.AccountServiceImpl;
   import com.itheima.domain.Account;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.context.annotation.AnnotationConfigApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试纯注解代码.
    * @RunWith: 修饰类
    *  指定一个启动器启动IOC容器
    * @ContextConfiguration: 修饰类
    *  配合RuWith启动IOC容器指定配置所在位置
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   //@ContextConfiguration(locations = {"classpath:applicationContext.xml"})
   @ContextConfiguration(classes = SpringConfig.class)
   public class AnnoTests {
   
   
       @Autowired
       AccountServiceImpl accountService;
   
       @Test
       public void testSave (){
   
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
       @Test
       public void testFindBy (){
           Account account = new Account();
           account.setId(10);
           Account acc = accountService.findById(account);
           System.out.println(acc);
       }
       @Test
       public void testUpdate (){
           Account account = new Account();
           account.setId(10);
           account.setMoney(11D);
           accountService.update(account);
       }
       @Test
       public void testDel (){
           Account account = new Account();
           account.setId(10);
           accountService.del(account);
       }
   }
   
   ```



#### 小结

- @RunWith注解的作用是什么?
  - 指定启动器启动IOC容器
- @ContextConfiguration的作用?
  - 指定创建IOC容器的配置





### 10代理模式【了解】

#### 目标

- 了解什么是代理模式
- 实现静态代理的案例



#### 1. 什么是代理模式

- 提供代理类 **控制(拦截)外部对目标对象的访问** 的一种代码设计

![1562124381705](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562124381705.png) 



#### 2. 静态代理的案例

1. 工程名称: spring02_proxy_04

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring02_proxy_04</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. 备用: Spring Cglib -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. Junit 依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-test</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   </project>
   ```

3. 定义明星接口: com.itheima.proxy.Star

   ```java
   package com.itheima.proxy;
   
   /**
    * 明星接口.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public interface Star {
   
   
       /**
        * 唱歌.
        *
        * @param money 费用 (出场费)
        */
       void sing(Integer money);
   
   
       /**
        * 拍戏
        */
       void pay(Integer money);
   }
   
   ```

4. 创建目标类: com.itheima.proxy.LiuStar

   ```java
   package com.itheima.proxy;
   
   /**
    * 刘德华本人  (目标对象).
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class LiuStar implements Star {
       @Override
       public void sing(Integer money) {
           System.out.println("刘德华唱歌真好听, 收费: "+money);
       }
   
       @Override
       public void pay(Integer money) {
           System.out.println("刘德华拍戏棒棒哒, 收费: "+money);
       }
   }
   
   ```

5. 创建代理类: com.itheima.proxy.LiuStarProxy

   ```java
   package com.itheima.proxy;
   
   /**
    * 刘德华的经纪人.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class LiuStarProxy implements Star {
   
       LiuStar liuStar = new LiuStar();
   
   
       @Override
       public void sing(Integer money) {
           // 限制外部对象目标对象的访问
           if(money > 10000){
               // 调用目标方法
               liuStar.sing(money);
           } else {
               System.out.println("档期忙..");
           }
       }
   
       @Override
       public void pay(Integer money) {
           if(money > 10000){
               // 调用目标方法
               liuStar.pay(money);
           } else {
               System.out.println("档期忙..");
           }
       }
   }
   
   ```

6. 单元测试: LiuStarProxyTests.java

   ```java
   import com.itheima.proxy.LiuStarProxy;
   import org.junit.Test;
   
   /**
    * 测试代理模式的代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class LiuStarProxyTests {
   
   
       /**
        * 静态代理的特点:
        *  1. 通过实现与目标对象相同的接口完成代理模式
        *  2. 需要在代理类中维护一个目标对象
        *  3. 代理类需要手工创建  (需要在代码运行前提供)
        */
       @Test
       public void testStatic (){
           // 1. 创建代理类  (经纪人)
           LiuStarProxy star = new LiuStarProxy();
           // 2. 操作代理方法
           star.sing(100000);
           star.pay(2);
       }
   }
   
   ```



#### 小结

- 什么是代理模式?
  - 提供代理类限制外部对目标对象的访问
- 目标对象是哪个?
  - LiuStar
- 代理对象是哪个?
  - LiuStarProxy
- 静态代理的特点?
  - 实现目标对象相同的接口
  - 需要在代理类中维护目标对象
  - 代理类需要手工创建



### 11动态代理-jdk【理解】

#### 目标

- 了解动态代理的特点
- 实现jdk动态代理案例



#### 1. 动态代理的特点

- 代理类在代码运行时创建



#### 2. jdk动态代理案例

1. 官网API (Proxy)

   ```java
   public static Object newProxyInstance(
      						// 1. 类加载器
                           ClassLoader loader,
       					// 2. 代理对象需要实现的接口数组
                           Class<?>[] interfaces,
       					// 3. 代理对象方法调用时的处理器
                           InvocationHandler h
   ) throws IllegalArgumentException
   ```

2. 单元测试: LiuStarProxyTests.java

   ```java
       /**
        * Jdk动态代理的特点:
        *  1. 必须实现目标对象相同的接口
        */
       @Test
       public void testJdk (){
           // 1. 创建目标对象
           LiuStar liuStar = new LiuStar();
           // 2. 创建代理对象
           Star star = (Star) Proxy.newProxyInstance(
                   // 1. 类加载器
                   LiuStarProxyTests.class.getClassLoader(),
                   // 2. 实现的接口字节码数组 (必须提供一个接口才能代理)
                   new Class[]{Star.class},
                   // 3. 代理方法的调用处理器 (触发器)
                   new InvocationHandler() {
                       /**
                        * 代理方法每次调用都会触发(调用)该方法
                        * @param proxy 代理对象
                        * @param method 代理方法
                        * @param args 参数列表
                        * @return 代理方法的返回值
                        * @throws Throwable 异常信息
                        */
                       @Override
                       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                           Integer money = (Integer) args[0];
                           // 限制外部对象目标对象的访问
                           if(money > 10000){
                               // 调用目标方法
                               method.invoke(liuStar, args);
                           } else {
                               System.out.println("档期忙..");
                           }
                           return null;
                       }
                   }
           );
           // 3. 操作代理方法
           star.sing(100000);
           star.pay(2);
       }
   ```



#### 小结

- 动态代理有什么特点?
  - 代理类在运行时创建 (自动创建)
- 使用jdk动态代理对目标对象有什么要求?
  - 目标对象必须实现接口



### 12动态代理-cglib【理解】

#### 目标

- cglib动态代理的案例
- 两种动态代理的区别



#### 1. cglib动态代理的案例

1. 添加依赖: pom.xml

   ```xml
   <!-- 添加Cglib实现 (Spring已整合) -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.0.2.RELEASE</version>
   </dependency>
   ```

2. 官网API (Enhancer)

   ```java
   public static Object create(
       					// 1. 代理对象需要继承的类
                           java.lang.Class superclass,
   						// 2. 代理对象需要实现的接口
                           java.lang.Class[] interfaces,
   						// 3. 代理对象方法调用时的处理器 InvocationHandler
                           Callback[] callbacks
   )
   ```

3. 单元测试: LiuStarProxyTests.java

   ```java
   /**
        * Cglib动态代理的特点:
        *  1. 可以不实现接口
        *  2. 必须继承目标对象
        */
   @Test
   public void testCglib (){
       // 1. 创建目标对象
       LiuStar liuStar = new LiuStar();
       // 2. 创建代理对象
       Star star = (Star) Enhancer.create(
           // 1. 父类字节码 (必须)
           LiuStar.class,
           // 2. 实现的接口字节码数组 (可以不实现接口)
           new Class[]{Star.class},
           // 3. 代理方法的调用处理器 (触发器)
           new org.springframework.cglib.proxy.InvocationHandler() {
               /**
                        * 代理方法每次调用都会触发(调用)该方法
                        * @param proxy 代理对象
                        * @param method 代理方法
                        * @param args 参数列表
                        * @return 代理方法的返回值
                        * @throws Throwable 异常信息
                        */
               @Override
               public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                   Integer money = (Integer) args[0];
                   // 限制外部对象目标对象的访问
                   if(money > 10000){
                       // 调用目标方法
                       method.invoke(liuStar, args);
                   } else {
                       System.out.println("档期忙..");
                   }
                   return null;
               }
           }
       );
       // 3. 操作代理方法
       star.sing(100000);
       star.pay(2);
   }
   ```



#### 2. 两种动态代理的区别

|          | JDK                                           | CGLIB                                    |
| -------- | --------------------------------------------- | ---------------------------------------- |
| 实现方式 | 利用 **实现** 接口方法来拦截目标方法          | 利用 **继承** 覆写方法来拦截目标方法     |
| 性能高低 | JDK1.6以前较Cglib慢; 1.6以及1.7大量调用时较慢 | 1.8时被JDK代理超越                       |
| 适用场景 | 目标对象必须实现接口                          | 目标方法需要允许覆写(不能用final/static) |



#### 小结

- 常见的动态代理有哪些?
  - jdk: 通过实现拦截目标方法
- cglib: 通过继承拦截目标方法

- 两种动态代理有什么区别?
  - jdk: 必须实现接口
  - cglib: 方法不能使用final/static修饰









### 13总结

1. jdbcTemplate如何注入数据源
   - 通过set方法调用
2. 可以使用外部数据源吗?
   - 可以使用任何外部的连接池
3. RowMapper的作用?
   - 将结果集封装成对象   (结果集映射器)
4. 纯注解开发涉及哪些注解?
   - @Configuration
   - @ComponentScan
   - @Imoprt
   - @Bean: 讲方法的返回值添加到IOC容器
   - @PropertySource: 修饰类, 加载外部配置文件
5. Spring-test涉及哪些注解?
   - @RunWith: 修饰类, 指定一个启动器启动IOC容器
   - @ContextConfiguaration: 加载一个配置启动IOC容器 (配合@RunWith使用)
6. 什么是代理模式?
   - 提供代理类, 限制外部对目标对象的访问 
7. 静态代理和动态代理的区别?
   - 静态代理的代理类需要手动创建
   - 动态代理的代理类在运行时自动创建
8. 两种动态代理的区别?
   - jdk: 通过实现接口完成完成代理
   - cglib: 通过继承目标对象完成代理

### 01复习

#### 目标

- 了解Spring中的设计模式



#### 1. 了解Spring中的设计模式

1. IOC的底层/原理是什么?
   - 工厂设计模式
2. IOC中的Bean都是单例的吗?
   - 默认是单例的, 可以配置成多例 : scope="protorype"
3. JdbcTeamplate用了什么设计模式?
   - 模板设计模式



#### 小结

- Spring中学习了哪些设计模式?
  - 工厂设计模式
  - 模板设计模式
  - 单例设计模式
- AOP的底层/原理是什么?
  - 代理设计模式



### 02AOP的概念【理解】

#### 目标

- 理解AOP相关的概念
- 理解AOP相关的术语



#### 1. 相关的概念

##### 1.1 AOP是什么

- 面向切面编程: AOP (Aspect Oriented Programming) 的简写
- 面向切面编程: 是一种代码设计思想

##### 1.2 AOP的作用

- 在不修改目标对象的情况下添加新的业务逻辑 (增强)

![1562124381705](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562124381705-1571725882391.png) 



##### 1.3 AOP的原理

- **底层使用了动态代理技术**实现的AOP切面编程思想

  ```java
  // 代理方法
  public void sing(Integer money){
      if(money > 10000){ // 新增的业务逻辑 (增强)
          targer.method(money); // 调用目标方法
      }
  }
  ```

  



#### 2. 相关的术语

##### 2.1 连接点

- 目标对象中的所有方法

##### 2.2 切入点

- 目标对象中需要增强的方法

##### 2.3 通知

- 目标对象需要增强的内容 (和位置)

##### 2.4 切面

- 切面是由**切入点** 和 **通知**组成的





#### 小结

- Spring AOP 的原理是什么?
  - 动态代理模式
- 请描述下列术语的含义:
  - 连接点: 目标对象中所有的方法
  - 切入点: 目标对象中需要增强的方法
  - 通知: 目标方法需要增强的内容 
  - 切面: 是切入点与通知的组成关系



### 03AOP的XML案例【掌握】

#### 目标

- 使用AOP自动记录日志
- 掌握AOP动态代理细节



#### 1. AOP自动记录日志

1. 工程名称: spring03_aop_01

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring03_aop_01</artifactId>
       <version>1.0-SNAPSHOT</version>
   
   
       <dependencies>
           <!-- 1. Spring IOC 和 AOP依赖 -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. AspectJ框架依赖  (切入点表达式) -->
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.8.13</version>
           </dependency>
           <!-- 3. Spring Test -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-test</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
       </dependencies>
   </project>
   ```

3. 业务处理: com.itheima.xml.impl.AccountServiceImpl

   ```java
   ackage com.itheima.xml.impl;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountServiceImpl {
   
   
       /**
        * 转账业务
        */
       public void trans(){
           // 转账业务
   
           // 记录日志.... (记录日志不该写在业务类中..)
   //        System.out.println("日志打印..");
           // 在不改动转账业务的前提下进行日志打印
       }
   }
   
   ```

4. 记录日志: com.itheima.xml.advice.LogAdvice

   ```java
   package com.itheima.xml.advice;
   
   /**
    * 日志工具类  (切面类/通知类).
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class LogAdvice {
   
   
       /**
        * 增强方法/ 通知方法
        */
       public void log(){
           System.out.println("日志打印..");
       }
   }
   
   ```

5. 配置AOP: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
   
   
       <!-- 1. 创建业务层对象 -->
       <bean id="accountService" class="com.itheima.xml.impl.AccountServiceImpl"/>
       <!-- 2. 创建切面/通知对象 -->
       <bean id="logAdvice" class="com.itheima.xml.advice.LogAdvice"/>
       <!-- 3. Aop配置 -->
       <aop:config>
           <!-- 1. 配置切入点 -->
           <!--
               aop:pointcut: 定义切入点
                   id: 切入点的名称
                   expression: 定义切入点表达式 (规则)
           -->
           <aop:pointcut id="pt" expression="execution(public void com.itheima.xml.impl.AccountServiceImpl.trans(..))"/>
           <!-- 2. 配置切面 (通知和切入点的关系) -->
           <!--
               aop:aspect: 定义切面
                   ref: 引用一个已经存在的通知类/切面类
           -->
           <aop:aspect ref="logAdvice">
               <!--
                   aop:after: 后置通知的配置
                   表示: 在pt方法之后执行log方法
                       method: 通知方法
                       pointcut-ref: 引用一个已经定义好的切入点
               -->
               <aop:after method="log" pointcut-ref="pt"/>
           </aop:aspect>
       </aop:config>
   
   </beans>
   ```

6. 单元测试: XmlTests

   ```java
   import com.itheima.xml.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试AOP代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   public class XmlTests {
   
   
       @Autowired
       AccountServiceImpl accountService;
   
   
       @Test
       public void testAop (){
           System.out.println("==========================="+accountService.getClass());
           accountService.trans();
       }
   }
   
   ```



#### 2. AOP动态代理细节

##### 2.1 动态代理的类型

1. 定义业务接口: com.itheima.xml.AccountService

   ```java
   package com.itheima.xml;
   
   /**
    * 账户业务层接口.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public interface AccountService {
   
   
       /**
        * 转账业务方法.
        */
       void trans();
   }
   
   ```

2. 实现业务接口: com.itheima.xml.impl.AccountServiceImpl

   ```java
   package com.itheima.xml.impl;
   
   import com.itheima.xml.AccountService;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountServiceImpl implements AccountService {
   
   
       /**
        * 转账业务
        */
       public void trans(){
           // 转账业务
   
           // 记录日志.... (记录日志不该写在业务类中..)
   //        System.out.println("日志打印..");
           // 在不改动转账业务的前提下进行日志打印
       }
   }
   
   ```

3. 单元测试: XmlTests

   ```java
   import com.itheima.xml.AccountService;
   import com.itheima.xml.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试AOP代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   public class XmlTests {
   
   
       @Autowired
       AccountService accountService;
   
   
       @Test
       public void testAop (){
           System.out.println("==========================="+accountService.getClass());
           accountService.trans();
       }
   }
   
   ```

   

##### 2.2 固定使用Cglib

- applicationContext.xml

  ```xml
  <!-- 
  	<aop:config: 配置AOP
  		proxy-target-class: 配置是否强制使用cglib动态代理
  -->
  <aop:config proxy-target-class="true">
  ```



#### 小结

- 自动记录日志的原理是什么?
  - AOP动态代理技术

- 两种动态代理Spring是如何选择的?
  - 默认情况下, 如果目标对象实现了接口则采用jdk, 没有则采用cglib



### 04切入点表达式【理解】

#### 目标

- 理解什么是切入点表达式
- 了解切入点表达式指示符



#### 1. 什么是切入点表达式

- 定位切入点的1种语法规则



#### 2. 切入点表达式指示符

- 切入点表达式常见的 指示符 (PCD)有以下三种

| 指示符        | 示例                                                    | 作用(细粒度)        |
| ------------- | ------------------------------------------------------- | ------------------- |
| bean          | bean(accountService)                                    | 精确到IOC容器的bean |
| within        | within(com.itheima.xml.service.impl.AccountServiceImpl) | 精确到类            |
| **execution** | execution(public void com.itheima..save(..))            | **精确到方法**      |

##### 2.1 bean

1. 演示: pointcut.xml

   ```xml
           <!--
               bean: 精确到容器中的对象 (对象中所有的方法都是切入点)
                   value: 容器中对象的名称
                   *: 可以使用通配符匹配任何字符串
           -->
           <aop:pointcut id="pt" expression="bean(*Service)"/>
   ```

2. 测试: XmlTests

   ```java
   import com.itheima.xml.AccountService;
   import com.itheima.xml.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试AOP代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   //@ContextConfiguration(locations = "classpath:applicationContext.xml")
   @ContextConfiguration(locations = "classpath:pointcut.xml")
   public class XmlTests {
   
   
       @Autowired
       AccountService accountService;
   
   
       @Test
       public void testAop (){
           System.out.println("==========================="+accountService.getClass());
           accountService.trans();
       }
   }
   
   ```

##### 2.2 within

1. 演示: pointcut.xml

   ```xml
   <!--
               within: 精确到项目中的类 (类中所有的方法都是切入点)
                   value: 全限定类名
                   *: 匹配任何字符串
                   ..: 匹配多级目录
           -->
   <!--<aop:pointcut id="pt" expression="within(com.itheima.xml.impl.AccountServiceImpl)"/>-->
   <aop:pointcut id="pt" expression="within(com.itheima..impl.*Service*)"/>
   ```

##### 2.3 execution

1. 演示: pointcut.xml

   ```xml
   <!--
               execution: 精确到方法
                   value: 定义方法的表达式
                   *: 匹配任何字符串 (参数列表中表示任意1个参数{public由于可以省略, 所以不需要使用*匹配})
                   ..: 匹配多级目录 (参数列表中表示任意个任意参数)
           -->
   
   <!--<aop:pointcut id="pt" expression="execution(public void com.itheima.xml.impl.AccountServiceImpl.trans())"/>-->
   <!--<aop:pointcut id="pt" expression="execution(public void com.itheima.xml.impl.*Service*.*())"/>-->
   <!--<aop:pointcut id="pt" expression="execution(* com.itheima.xml.impl.*Service*.*())"/>-->
   <!--<aop:pointcut id="pt" expression="execution(* com..*Service*.*(..))"/>-->
   <!-- 最简洁的方式, 但是不推荐使用 -->
   <aop:pointcut id="pt" expression="execution(* *.*(..))"/>
   ```

`execution(modififiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?) `

- **modififiers-pattern**: 权限访问修饰符 (可填) 
- **ret-type-pattern**: 返回值类型 (必填) 
- **declaring-type-pattern**: 全限定类名 (可填) 
- **name-pattern**: 方法名 (必填) 
- **param-pattern**: 参数名 (必填) 
- **throws-pattern**: 异常类型 (可填) 



#### 小结

- 什么是切入点表达式?
  - 定义切入点的语法规则
- 常用的指示符有什么区别?
  - bean: 精确到容器中的对象
  - within: 精确到项目中的类
  - execution: 精确到方法





### 05AOP的通知类型【了解】

#### 目标

- 理解AOP的通知类型
- 理解四种通知的配置



#### 1. AOP的通知类型

- SpringAOP的通知类型有以下4种: 

  ```java
  try{
      [前置通知]
      // 执行目标对象方法..
      targer.method(..);
      [后置通知]
  } catch (Exception e){
      [异常通知]
  } finally {
      [最终通知]
  }
  ```

#### 2. 四种通知的配置

1. 通知类型分类: 前置通知、后置通知、异常通知、最终通知

2. 提供通知方法: com.itheima.xml.advice.LogAdvice

   ```java
   package com.itheima.xml.advice;
   
   /**
    * 日志工具类  (切面类/通知类).
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class LogAdvice {
   
   
       /**
        * 增强方法/ 通知方法
        */
       public void log(){
           System.out.println("日志打印..");
       }
   
   
       public void before(){
           System.out.println("前置通知..");
       }
       public void afterReturning(){
           System.out.println("后置通知..");
       }
       public void afterThrowing(){
           System.out.println("异常通知..");
       }
       public void after(){
           System.out.println("最终通知..");
       }
   }
   
   ```

3. 配置通知类型: advice.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
   
   
       <!-- 1. 创建业务层对象 -->
       <bean id="accountService" class="com.itheima.xml.impl.AccountServiceImpl"/>
       <!-- 2. 创建切面/通知对象 -->
       <bean id="logAdvice" class="com.itheima.xml.advice.LogAdvice"/>
       <!-- 3. Aop配置 -->
       <!--
           proxy-target-class: 是否固定使用cglib生成代理类
               true: 使用cglib
               false: 默认值, 自由选择代理工具  (目标对象实现了接口则采用jdk, 没有则采用cglib)
       -->
       <aop:config proxy-target-class="true">
           <!-- 1. 配置切入点 -->
           <!--
               aop:pointcut: 定义切入点
                   id: 切入点的名称
                   expression: 定义切入点表达式 (规则)
           -->
           <aop:pointcut id="pt" expression="execution(public void com.itheima.xml.impl.AccountServiceImpl.trans(..))"/>
           <!-- 2. 配置切面 (通知和切入点的关系) -->
           <!--
               aop:aspect: 定义切面
                   ref: 引用一个已经存在的通知类/切面类
           -->
           <aop:aspect ref="logAdvice">
               <!-- 前置通知 -->
               <aop:before method="before" pointcut-ref="pt"/>
               <!-- 后置通知 -->
               <aop:after-returning method="afterReturning" pointcut-ref="pt"/>
               <!-- 异常通知 -->
               <aop:after-throwing method="afterThrowing" pointcut-ref="pt"/>
               <!-- 最终通知 -->
               <aop:after method="after" pointcut-ref="pt"/>
           </aop:aspect>
       </aop:config>
   
   </beans>
   ```

4. 测试通知类型: XmlTests

   ```java
   import com.itheima.xml.AccountService;
   import com.itheima.xml.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试AOP代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   //@ContextConfiguration(locations = "classpath:applicationContext.xml")
   //@ContextConfiguration(locations = "classpath:pointcut.xml")
   @ContextConfiguration(locations = "classpath:advice.xml")
   public class XmlTests {
   
   
       @Autowired
       AccountService accountService;
   
   
       @Test
       public void testAop (){
           System.out.println("==========================="+accountService.getClass());
           accountService.trans();
       }
   }
   
   ```

   

#### 小结

- 通知分为哪几类?
  - 前置, 后置, 异常, 最终



### 06AOP的环绕通知【掌握】

#### 目标

- 掌握通用通知的使用
- 掌握环绕通知的优点



#### 1. 环绕通知

1. com.itheima.xml.advice.LogAdvice

   ```java
       /**
        * 环绕通知.
        *
        * @param pjp 目标方法  (切入点)
        * @return 目标方法的返回值
        */
       public Object around(ProceedingJoinPoint pjp){
           // 获取参数
           Object[] args = pjp.getArgs();
           System.out.println(args);
           try {
               before();
               // 执行目标方法
               Object res = pjp.proceed(args);
               afterReturning();
               return res;
           } catch (Throwable throwable) {
               throwable.printStackTrace();
               afterThrowing();
           } finally {
               after();
           }
           return null;
       }
   ```

1. advice.xml

   ```xml
       <aop:config proxy-target-class="true">
        <!-- 1. 配置切入点 -->
           <!--
               aop:pointcut: 定义切入点
                   id: 切入点的名称
                   expression: 定义切入点表达式 (规则)
           -->
           <aop:pointcut id="pt" expression="execution(public void com.itheima.xml.impl.AccountServiceImpl.trans(..))"/>
           <!-- 2. 配置切面 (通知和切入点的关系) -->
           <!--
               aop:aspect: 定义切面
                   ref: 引用一个已经存在的通知类/切面类
           -->
           <aop:aspect ref="logAdvice">
               <!--
               &lt;!&ndash; 前置通知 &ndash;&gt;
               <aop:before method="before" pointcut-ref="pt"/>
               &lt;!&ndash; 后置通知 &ndash;&gt;
               <aop:after-returning method="afterReturning" pointcut-ref="pt"/>
               &lt;!&ndash; 异常通知 &ndash;&gt;
               <aop:after-throwing method="afterThrowing" pointcut-ref="pt"/>
               &lt;!&ndash; 最终通知 &ndash;&gt;
               <aop:after method="after" pointcut-ref="pt"/>
               -->
               <!-- 环绕通知 -->
               <aop:around method="around" pointcut-ref="pt"/>
           </aop:aspect>
       </aop:config>
   ```

   

#### 小结

- 环绕通知的优点?
  - 可以自由控制目标方法已经通知的执行顺序



### 07AOP的注解案例【理解】

#### 目标

- 使用AOP相关注解改造XML案例



#### 1. 注解改造XML案例

##### 1.1 改造工程

- 工程名称: spring03_anno_02

##### 1.2 改造通知

- com.itheima.xml.advice.LogAdvice

  ```java
  package com.itheima.xml.advice;
  
  import org.aspectj.lang.ProceedingJoinPoint;
  import org.aspectj.lang.annotation.*;
  import org.springframework.stereotype.Component;
  
  /**
   * 日志工具类  (切面类/通知类).
   * @Aspect: 修饰类
   *  修饰为一个切面
   */
  @Component
  @Aspect
  public class LogAdvice {
  
      /**
       * 增强方法/ 通知方法
       * @Pointcut: 修饰方法
       *  定义一个切入点表达式
       */
      @Pointcut("execution(public void com.itheima.xml.impl.AccountServiceImpl.trans(..))")
      public void log(){
  
          // 作为切入点表达式的变量时: 该方法不会执行
          System.out.println("日志打印..");
      }
  
  
      /**
       * @Before:　修饰方法
       *  修饰方法为前置通知方法
       */
  //    @Before("execution(public void com.itheima.xml.impl.AccountServiceImpl.trans(..))")
      public void before(){
          System.out.println("前置通知..");
      }
  
      /**
       * @AfterReturning：　修饰方法
       *  修饰方法为后置通知方法
       */
  //    @AfterReturning("log()")
      public void afterReturning(){
          System.out.println("后置通知..");
      }
  
      /**
       * @AfterThrowing: 修饰方法
       *  修饰方法为异常通知方法
       */
  //    @AfterThrowing("log()")
      public void afterThrowing(){
          System.out.println("异常通知..");
      }
  
      /**
       * @After: 修饰方法
       *  修饰方法为最终通知方法
       */
  //    @After("log()")
      public void after(){
          System.out.println("最终通知..");
      }
  
  
      /**
       * 环绕通知.
       *
       * @param pjp 目标方法  (切入点)
       * @return 目标方法的返回值
       */
      @Around("log()")
      public Object around(ProceedingJoinPoint pjp){
          // 获取参数
          Object[] args = pjp.getArgs();
          System.out.println(args);
          try {
              before();
              // 执行目标方法
              Object res = pjp.proceed(args);
              afterReturning();
              return res;
          } catch (Throwable throwable) {
              throwable.printStackTrace();
              afterThrowing();
          } finally {
              after();
          }
          return null;
      }
  }
  
  ```

##### 1.3 改造配置

- advice.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  
  
      <!-- 1. 创建对象 -->
      <context:component-scan base-package="com.itheima.xml"/>
      <!-- 2. Aop配置 -->
      <!--
          aop:aspectj-autoproxy: 开启AOP自动代理 (开启AOP注解支持)
      -->
      <aop:aspectj-autoproxy proxy-target-class="true"/>
  </beans>
  ```



#### 小结

- 请描述以下注解的作用?
  - @Aspect: 修饰类, 修饰为切面类
  - @Pointcut: 修饰方法, 定义一个切入点表达式
  - @Around: 修饰方法, 讲方法修饰为环绕通知方法





### 08事务的概念【了解】

#### 目标

- 了解事务的作用
- 理解事务的特性



#### 1. 事务的作用

- 在业务中 **保证数据安全**

![1565246073783](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1565246073783.png)  



#### 2. 事务的特性

- 事务的四大特性也叫事务的 **ACID原则**

##### 2.1 原子性（Atomicity）

- 事务是一个最小的执行单元, 不可再拆分

##### 2.2 一致性（Consistency）

- 事务前后的总账需要保持一致

##### 2.3 隔离性（Isolation）

- 多个事务之间不会相互影响, 互不干扰

##### 2.4 持久性（Durability）

- 事务执行结束, 结果会永久的保存起来



#### 小结

- 事务的作用是什么?
  - 在业务中保证数据安全的
- 事务的ACID原则分别是什么?
  - 原子性, 一致性, 隔离性, 持久性



### 09Spring声明式事务【了解】

#### 目标

- 理解什么是声明式事务?
- 理解Spring事务相关API



#### 1. 什么是声明式事务

- 通过 **面向切面编程思想** 来实现的事务管理方式
- Spring声明式事务的简写: TX

![1565248277054](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1565248277054.png) 

#### 2. Spring事务相关API

##### 2.1 PlatformTransactionManager

- 事务管理接口: 定义了提交事务、获取事务、回滚事务的方法。 

![1562574934801](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562574934801.png) 

##### 2.2 TransactionStatus

- 事务状态接口: 定义了Spring内部的事务状态规范

![1562575940075](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562575940075.png) 

- 存储点: 用于设置事务回滚的位置

```java
// 创建存储点
Object savepoint = TransactionAspectSupport.currentTransactionStatus().createSavepoint();
// 执行数据库操作
userService.save();
// 回滚到存储点
TransactionAspectSupport.currentTransactionStatus().rollbackToSavepoint(savepoint);
```

![1566116589347](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1566116589347.png) 



##### 2.3 TransactionDefinition

- 事务信息接口: 定义了事务的隔离级别、传播行为、超时时间、是否只读等。

![1562575584950](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562575584950.png) 

#### 小结

- 什么是声明式事务?
  - 只需要提供声明即可获得事务管理功能 (不需要编写事务代码)
- 事务管理接口中有哪些方法?
  - 提交方法, 回滚方法, 获取事务状态方法



### 10事务的隔离级别【理解】

#### 目标

- 理解事务的隔离级别



#### 1. 事务的隔离级别

- 事务的隔离级别表示的是: 多个事务之间的影响程度

![1565251783277](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1565251783277.png) 

##### 1.1 ISOLATION_DEFAULT

- 默认采用数据库的隔离级别

##### 1.2 ISOLATION_READ_UNCOMMITTED

- 读取未提交数据: 可以读取到其他事务没有提交的数据  (有脏读问题)

| 时间 | 事务A                                                     | 事务B                                                        |
| ---- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 1    | 开启事务                                                  |                                                              |
| 2    |                                                           | 开启事务                                                     |
| 3    | // 原来是小明<br/>update user set name="小白" where id=1; |                                                              |
| 5    |                                                           | <font color='red'>// 结果是小白</font><br/>select name from user where id=1; |
| 6    | 回滚事务                                                  |                                                              |

##### 1.3 ISOLATION_READ_COMMITTED

- 读取已提交数据: 只能读取到其他事务已经提交的数据 (解决了脏读问题, 有不可重复读问题)

| 时间 | 事务A                                                     | 事务B                                                        |
| ---- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 1    | 开启事务                                                  |                                                              |
| 2    |                                                           | 开启事务                                                     |
| 3    |                                                           | // 结果是小明<br/>select name from user where id=1;          |
| 5    | // 原来是小明<br/>update user set name="小白" where id=1; |                                                              |
| 6    | 提交事务                                                  |                                                              |
| 7    |                                                           | <font color='red'>// 结果不一样</font><br/>select name from user where id=1; |

##### 1.4 ISOLATION_REPEATABLE_READ

- 可重复读: 一个事务中的多次查询结果相同 (解决了不可重复读问题, 有幻读问题)

| 时间 | 事务A                                                        | 事务B                                               |
| ---- | ------------------------------------------------------------ | --------------------------------------------------- |
| 1    |                                                              | 开启事务                                            |
| 2    | 开启事务                                                     |                                                     |
| 3    | // 结果是小明<br/>select name from user where id=1;          | // 结果是小明<br/>select name from user where id=1; |
| 5    | <font color='red'>// 不能修改</font><br/>update user set name="小白" where id=1; | // 结果是一样<br/>select name from user where id=1; |
| 6    |                                                              | 提交事务                                            |
| 7    | // 可以修改<br/>update user set name="小白" where id=1;      |                                                     |

##### 1.5 ISOLATION_SERIALIZABLE

- 事务串行化: 序列化: 事务开启后加锁限制其他事务的开启   (建议不要用, 性能极差)

| 时间 | 事务A    | 事务B                                                   |
| ---- | -------- | ------------------------------------------------------- |
| 1    |          | 开启事务                                                |
| 2    |          | // 结果是小明<br/>select name from user where id=1;     |
| 3    |          | // 可以修改<br/>update user set name="小白" where id=1; |
| 5    |          | 提交事务                                                |
| 6    | 开启事务 |                                                         |



#### 小结

- 什么是脏读?

  - 可以读取到其他事务没有提交的数据
- 什么是幻读?
  - 可以读取到其他事务增删的数据



### 11事务的传播行为【理解】

#### 目标

- 理解事务的传播行为



#### 1. 事务的传播行为

- 当前方法对事务的要求

![1562831970253](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562831970253.png) 

##### 1.1 REQUIRED

- 需要: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 创建新的事物运行

##### 1.2 SUPPORTS

- 支持: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 则正常运行

##### 1.3 MANDATORY

- **强制**: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 则抛出异常

##### 1.4 REQUIRES_NEW

- 新: 无论外部是否有事务, 都将创建新的事物运行, 如果外部有则挂起

##### 1.5 NOT_SUPPORTED

- 不支持: 如果外部有事务, 则挂起, 如果外部没有事务, 则正常运行

##### 1.6 NEVER

- **决不**: 外部如果有事务, 则抛出异常, 如果没有, 则正常运行

##### 1.7 NESTED

- <font color='red'>嵌套</font>:  如果外部有事务, 那么当前事务会嵌套在外部事务中一起运行, 如果外部没有事务, 则REQUIRED



#### 小结

- SUPPORTS与NOT_SUPPORTS的区别?
  - SUPPORTS: 支持, 当前方法可以有事务, 也可以没事务
  - NOT_SUPPORTS: 当前方法不支持以事务的方式运行
- 哪些传播行为不满足要求会抛出异常?
  - MANDATORY: 强制外部需要事务
  - NEVER: 决不允许外部有事务



### 12TX - XML案例【掌握】

#### 目标

- 使用XML实现声明式事务管理



#### 1. 实现声明式事务管理

##### 1.1 搭建环境

1. 工程名称: spring03_xml_03

2. 添加依赖: pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.itheima</groupId>
       <artifactId>spring03_xml_03</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <dependencies>
           <!-- 1. Spring IOC和AOP -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 2. AspectJ -->
           <dependency>
               <groupId>org.aspectj</groupId>
               <artifactId>aspectjweaver</artifactId>
               <version>1.8.13</version>
           </dependency>
           <!-- 3. Spring TX-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-tx</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 4. Spring JDBC -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <!-- 5. Mysql驱动 -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.17</version>
           </dependency>
           <!-- 6. Druid连接池 -->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.1.12</version>
           </dependency>
           <!-- 7. Spring Test -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-test</artifactId>
               <version>5.0.2.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
           </dependency>
           <!-- 8. Log4j工具包 -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>1.7.7</version>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-api</artifactId>
               <version>1.7.26</version>
           </dependency>
       </dependencies>
   
   </project>
   ```

##### 1.2 业务代码

1. 实体类: com.itheima.xml.domain.Account

   ```java
   package com.itheima.xml.domain;
   
   /**
    * 账户类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class Account {
       private Integer id;
       private Integer uid;
       private Double money;
   
       public Integer getId() {
           return id;
       }
   
       public void setId(Integer id) {
           this.id = id;
       }
   
       public Integer getUid() {
           return uid;
       }
   
       public void setUid(Integer uid) {
           this.uid = uid;
       }
   
       public Double getMoney() {
           return money;
       }
   
       public void setMoney(Double money) {
           this.money = money;
       }
   
       @Override
       public String toString() {
           return "Account{" +
                   "id=" + id +
                   ", uid=" + uid +
                   ", money=" + money +
                   '}';
       }
   }
   
   ```

2. 持久类: com.itheima.xml.dao.impl.AccountDaoImpl

   ```java
   package com.itheima.xml.dao.impl;
   
   import com.itheima.xml.domain.Account;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   /**
    * 账户持久层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountDaoImpl {
   
   
       JdbcTemplate jdbcTemplate;
   
       public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
           this.jdbcTemplate = jdbcTemplate;
       }
   
   
   
       public void save(Account account){
           String sql = "insert into account values(null,?,?)";
           jdbcTemplate.update(sql, account.getUid(), account.getMoney());
       }
   }
   
   ```

3. 业务类: com.itheima.xml.service.impl.AccountServiceImpl

   ```java
   package com.itheima.xml.service.impl;
   
   import com.itheima.xml.dao.impl.AccountDaoImpl;
   import com.itheima.xml.domain.Account;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   public class AccountServiceImpl {
   
   
       AccountDaoImpl accountDao;
   
       public void setAccountDao(AccountDaoImpl accountDao) {
           this.accountDao = accountDao;
       }
   
   
       /**
        * 需要提供事务: 可能会多次操作数据库
        */
       public void save(Account account) {
           accountDao.save(account);
           // 如果抛出异常, 希望不会插入任何数据, 如果事务不存在, 则会插入一条数据
           int i = 1 / 0;
           accountDao.save(account);
       }
   }
   
   ```



##### 1.3 事务配置

1. 添加配置文件: db.properties

   ```properties
   db.driver=com.mysql.jdbc.Driver
   # 针对Mysql 8.x数据库的参数
   #   serverTimezone: 指定时区(UTC)
   #   useSSL: 指定是否使用加密安全连接(false)
   #   allowPublicKeyRetrieval: 是否允许检索公钥(true)
   db.url=jdbc:mysql:///mybatisdb?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true&characterEncoding=UTF-8
   db.username=root
   db.password=root
   ```

2. 配置事务管理: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
   
   
       <!-- 1. 加载外部配置文件 -->
       <context:property-placeholder location="classpath:db.properties"/>
       <!-- 2. 创建数据源连接池 -->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="${db.driver}"/>
           <property name="url" value="${db.url}"/>
           <property name="username" value="${db.username}"/>
           <property name="password" value="${db.password}"/>
       </bean>
       <!-- 3. 创建JDBC操作模板 -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!-- 4. 创建持久层对象 -->
       <bean id="accountDao" class="com.itheima.xml.dao.impl.AccountDaoImpl">
           <property name="jdbcTemplate" ref="jdbcTemplate"/>
       </bean>
       <!-- 5. 创建业务层对象 -->
       <bean id="accountService" class="com.itheima.xml.service.impl.AccountServiceImpl">
           <property name="accountDao" ref="accountDao"/>
       </bean>
       <!-- 6. 配置AOP -->
       <aop:config proxy-target-class="true">
           <aop:pointcut id="pt" expression="execution(* com.itheima..impl.*.*(..))"/>
           <!--
               aop:advisor： 定义通知
                   advice-ref: 引用一个通知规则
                   pointcut-ref: 引用一个切入点
           -->
           <aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
       </aop:config>
       <!-- 7. 配置通知规则 -->
       <!--
           tx:advice: 定义事务的通知规则
                  transaction-manager: 管理事务的事务管理器
       -->
       <tx:advice id="txAdvice" transaction-manager="transactionManager">
           <tx:attributes>
               <!-- 查数据库 -->
               <!--
                   tx:method: 指定一些方法的事务规则
                       name: 方法的名称规则 (支持*匹配所有字符串)
                       isolation: 指定隔离级别
                       propagation: 传播行为
                       timeout: 超时时间
                       read-only: 是否只读
               -->
               <tx:method name="find*" isolation="DEFAULT" propagation="SUPPORTS" timeout="1000" read-only="true" />
               <tx:method name="query*" isolation="DEFAULT" propagation="SUPPORTS" timeout="1000" read-only="true" />
               <tx:method name="get*" isolation="DEFAULT" propagation="SUPPORTS" timeout="1000" read-only="true" />
               <!-- 写数据库 -->
               <tx:method name="save*" isolation="DEFAULT" propagation="REQUIRED" timeout="-1" read-only="false"/>
               <tx:method name="update*" />
               <tx:method name="insert*" />
               <tx:method name="del*" />
           </tx:attributes>
       </tx:advice>
       <!-- 8. 创建事务管理器 -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   </beans>
   ```



##### 1.4 单元测试

1. 单元测试: XmlTests.java

   ```java
   import com.itheima.xml.domain.Account;
   import com.itheima.xml.service.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试声明式事务代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   public class XmlTests {
   
       @Autowired
       AccountServiceImpl accountService;
   
   
       @Test
       public void testTx (){
           System.out.println("======================"+accountService.getClass());
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
   
   }
   
   ```



#### 小结

- 声明式事务有什么好处?
  - 只需要提供一点小小的声明即可获得事务管理功能



### 13TX - 注解案例【掌握】

#### 目标

- 使用注解改造案例



#### 1. 使用注解改造案例

1. 改造工程: spring03_anno_04

2. 使用注解: com.itheima.xml.dao.impl.AccountDaoImpl

   ```java
   package com.itheima.xml.dao.impl;
   
   import com.itheima.xml.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.stereotype.Repository;
   
   /**
    * 账户持久层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Repository
   public class AccountDaoImpl {
   
       @Autowired
       JdbcTemplate jdbcTemplate;
   
   
       public void save(Account account){
           String sql = "insert into account values(null,?,?)";
           jdbcTemplate.update(sql, account.getUid(), account.getMoney());
       }
   }
   
   ```

3. 使用注解: com.itheima.xml.service.impl.AccountServiceImpl

   ```java
   package com.itheima.xml.service.impl;
   
   import com.itheima.xml.dao.impl.AccountDaoImpl;
   import com.itheima.xml.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import org.springframework.transaction.annotation.Transactional;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Service
   public class AccountServiceImpl {
   
       @Autowired
       AccountDaoImpl accountDao;
   
   
       /**
        * 需要提供事务: 可能会多次操作数据库
        * @Transactional: 修饰方法, 类
        *  作用: 给房方法添加事务管理功能
        */
       @Transactional
       public void save(Account account) {
           accountDao.save(account);
           // 如果抛出异常, 希望不会插入任何数据, 如果事务不存在, 则会插入一条数据
           int i = 1 / 0;
           accountDao.save(account);
       }
   }
   
   ```

4. 添加配置: applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
   
       <!-- 提供IOC注解的支持 -->
       <context:component-scan base-package="com.itheima.xml"/>
       <!--
           tx:annotation-driven: 提供事务注解的支持
               proxy-target-class: 是否固定使用Cglib代理技术
       -->
       <tx:annotation-driven proxy-target-class="true" />
   
   
   
       <!-- 1. 加载外部配置文件 -->
       <context:property-placeholder location="classpath:db.properties"/>
       <!-- 2. 创建数据源连接池 -->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="${db.driver}"/>
           <property name="url" value="${db.url}"/>
           <property name="username" value="${db.username}"/>
           <property name="password" value="${db.password}"/>
       </bean>
       <!-- 3. 创建JDBC操作模板 -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   
       <!--<tx:annotation-driven proxy-target-class="true" />-->
   
   
       <!-- 8. 创建事务管理器 -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
   </beans>
   ```

5. 单元测试: XmlTests.java

   ```java
   import com.itheima.xml.domain.Account;
   import com.itheima.xml.service.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试声明式事务代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   public class XmlTests {
   
       @Autowired
       AccountServiceImpl accountService;
   
   
       @Test
       public void testTx (){
           System.out.println("======================"+accountService.getClass());
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
   
   }
   
   ```



#### 小结

- @Transactional注解的作用是什么?
  - 修饰方法, 或者类, 给方法加上事务管理功能



### 14TX - 纯注解案例【理解】

#### 目标

- 使用注解代替所有配置



#### 1. 使用注解代替所有配置

1. 创建配置类: com.itheima.xml.config.SpringConfig

   ```java
   package com.itheima.xml.config;
   
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.ComponentScan;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Import;
   import org.springframework.jdbc.datasource.DataSourceTransactionManager;
   import org.springframework.transaction.PlatformTransactionManager;
   import org.springframework.transaction.annotation.EnableTransactionManagement;
   
   import javax.sql.DataSource;
   
   /**
    * @EnableTransactionManagement: 修饰类
    *  作用: 开启事务注解的支持
    */
   @Configuration
   @ComponentScan("com.itheima.xml")
   @Import(JdbcConfig.class)
   @EnableTransactionManagement
   public class SpringConfig {
   
   
   
       @Bean
       public PlatformTransactionManager transactionManager(DataSource ds){
           return new DataSourceTransactionManager(ds);
       }
   }
   
   ```

2. 创建数据库配置类: com.itheima.xml.config.JdbcConfig

   ```java
   package com.itheima.xml.config;
   
   import com.alibaba.druid.pool.DruidDataSource;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   import javax.sql.DataSource;
   
   /**
    * JdbcConfig.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Configuration
   @PropertySource("classpath:db.properties")
   public class JdbcConfig {
   
       @Value("${db.driver}")
       String driver;
       @Value("${db.url}")
       String url;
       @Value("${db.username}")
       String username;
       @Value("${db.password}")
       String password;
   
   
       @Bean
       public DataSource dataSource(){
           DruidDataSource ds = new DruidDataSource();
           ds.setDriverClassName(driver);
           ds.setUrl(url);
           ds.setUsername(username);
           ds.setPassword(password);
           return ds;
       }
   
       @Bean
       public JdbcTemplate jdbcTemplate(DataSource ds){
           return new JdbcTemplate(ds);
       }
   
   
   }
   
   ```

3. 单元测试: AnnoTests

   ```java
   import com.itheima.xml.config.SpringConfig;
   import com.itheima.xml.domain.Account;
   import com.itheima.xml.service.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试声明式事务代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = SpringConfig.class)
   public class AnnoTests {
   
       @Autowired
       AccountServiceImpl accountService;
   
   
       @Test
       public void testTx (){
           System.out.println("======================"+accountService.getClass());
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
   
   }
   
   ```



#### 小结

- @EnableTransactionManagement注解的作用?
  - 



### 15Spring编程式事务【了解】

#### 目标

- 了解编程式事务概念
- 实现编程式事务案例



#### 1. 编程式事务概念

- 通过 **编码** 来实现的事务管理方式
- 以下是Spring封装的API

##### 1.1 TransactionTemplate

- 

##### 1.2 TransactionCallback

- 





#### 2.编程式事务案例

##### 2.1 改造代码

1. 改造工程: spring03_code_05

2. 事务编码: com.itheima.xml.service.impl.AccountServiceImpl

   ```java
   package com.itheima.xml.service.impl;
   
   import com.itheima.xml.dao.impl.AccountDaoImpl;
   import com.itheima.xml.domain.Account;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import org.springframework.transaction.TransactionStatus;
   import org.springframework.transaction.annotation.Isolation;
   import org.springframework.transaction.annotation.Propagation;
   import org.springframework.transaction.annotation.Transactional;
   import org.springframework.transaction.support.TransactionCallback;
   import org.springframework.transaction.support.TransactionTemplate;
   
   /**
    * 账户业务层实现类.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @Service
   public class AccountServiceImpl {
   
       @Autowired
       AccountDaoImpl accountDao;
   
   
       @Autowired
       TransactionTemplate transactionTemplate;
   
   
       /**
        * 需要提供事务: 可能会多次操作数据库
        * @Transactional: 修饰方法, 类
        *  作用: 给房方法添加事务管理功能
        */
       public void save(Account account) {
   
           // 创建存在事务的 方法/接口函数(只有一个方法的接口)
           TransactionCallback tc = new TransactionCallback() {
               /**
                * 该方法存在事务
                * @param status
                * @return
                */
               @Override
               public Object doInTransaction(TransactionStatus status) {
                   accountDao.save(account);
                   // 如果抛出异常, 希望不会插入任何数据, 如果事务不存在, 则会插入一条数据
                   int i = 1 / 0;
                   accountDao.save(account);
                   return null;
               }
           };
   
   
           // 执行接口函数
           transactionTemplate.execute(tc);
       }
   }
   
   ```



##### 2.2 添加配置

1. 添加配置: applicationContext.xml

   ```xml
   package com.itheima.xml.config;
   
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.ComponentScan;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Import;
   import org.springframework.jdbc.datasource.DataSourceTransactionManager;
   import org.springframework.transaction.PlatformTransactionManager;
   import org.springframework.transaction.annotation.EnableTransactionManagement;
   import org.springframework.transaction.support.TransactionTemplate;
   
   import javax.sql.DataSource;
   
   /**
    * @EnableTransactionManagement: 修饰类
    *  作用: 开启事务注解的支持
    */
   @Configuration
   @ComponentScan("com.itheima.xml")
   @Import(JdbcConfig.class)
   @EnableTransactionManagement
   public class SpringConfig {
   
   
       @Bean
       public PlatformTransactionManager transactionManager(DataSource ds){
           return new DataSourceTransactionManager(ds);
       }
       
       
       @Bean
       public TransactionTemplate transactionTemplate(PlatformTransactionManager transactionManager){
           return new TransactionTemplate(transactionManager);
       }
   }
   
   ```



##### 2.3 单元测试

1. 单元测试: XmlTests

   ```java
   import com.itheima.xml.config.SpringConfig;
   import com.itheima.xml.domain.Account;
   import com.itheima.xml.service.impl.AccountServiceImpl;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   /**
    * 测试声明式事务代码.
    *
    * @author : Jason.lee
    * @version : 1.0
    */
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(classes = SpringConfig.class)
   public class AnnoTests {
   
       @Autowired
       AccountServiceImpl accountService;
   
   
       @Test
       public void testTx (){
           System.out.println("======================"+accountService.getClass());
           Account account = new Account();
           account.setId(10);
           account.setUid(10);
           account.setMoney(10D);
           accountService.save(account);
       }
   
   }
   
   ```



#### 小结

- 编程式事务有什么好处?
  - 





### 16总结

1. 什么是代理模式?

   - 

2. 动态代理JDK和CGLIB的区别?

   - jdk: 
   - cglib: 

3. Spring AOP的底层是什么?

   - 

4. 切面和切入点以及通知是什么?

   - 
   - 
   - 

5. 通知类型有哪些?

   - 

   - 

   - 

   - 

   - 

6. AOP相关的注解有哪些?

   - 

   - 

   - 

   - 

   - 

7. 事务的作用是什么?

   - 

8. 什么是ACID原则?

   - Atomicity:

   - Consistency: 

   - Isolation: 

   - Durability:

9. 以下隔离级别分别解决了什么问题?

   - ISOLATION_READ_UNCOMMITTED:

   - ISOLATION_READ_COMMITTED: 

   - ISOLATION_REPEATABLE_READ:

   - ISOLATION_SERIALIZABLE: 序列化事务:解决幻读问题

10. 请说出以下传播行为的含义:

    - REQUIRED: 需要事务

    - SUPPORTS: 支持事务

    - MANDATORY: 强制

    - REQUIRES_NEW: 新事物

    - NOT_SUPPORTED: 不支持

    - NEVER: 绝不

    - NESTED: 嵌套

11. 声明式事务的好处是什么?

    - 只需要提供一点点声明即可获得事务管理功能

12. 请描述以下注解的作用?

    - @EnableTransactionManagement: 开启事务管理功能

    - @Transactional:  给方法添加事务管理功能

13. Spring的优点有哪些?

    - IOC
    - AOP
    - 声明式事务