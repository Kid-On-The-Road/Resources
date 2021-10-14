# Spring

介绍

- Spring是 **分层** 的轻量级开源框架 (重点)
- Spring核心是**IOC**(控制反转) 和 **AOP**(面向切面编程)
- Spring提供了对各种优秀框架的支持和 **整合**

# IOC

## 介绍

将对象的创建(控制)转交给第三方(工厂)完成, 也叫 **控制反转**

## 作用

| 作用                 | 解释                                     | 解决的问题             |
| -------------------- | ---------------------------------------- | ---------------------- |
| **解耦**             | 利用IOC的工厂模式解耦创建对象的过程      | 解决代码耦合度过高问题 |
| **存储对象**         | 可以将创建好的对象 **存储** 起来重复使用 | 解决对象个数问题       |
| **管理依赖关系**     | 可以将依赖对象注入需要的对象当中         | 解决依赖关系问题       |
| **管理对象创建顺序** | 可以根据依赖关系先后创建对象             | 解决创建顺序问题       |

## 依赖

```xml
<!-- 1. Spring IOC的依赖 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
```

## 创建IOC

- 资源文件创建方式

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
```

- 系统文件创建方式

```java
FileSystemXmlApplicationContext context = new FileSystemXmlApplicationContext("E:\\课堂\\Spring\\第1天\\04代码\\spring1\\spring-day01-ioc\\src\\main\\resources\\beans.xml");
```

- 注解配置创建方式

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

- ~~BeanFactory~~创建方式

```java
Resource resource = new ClassPathResource("beans.xml");
XmlBeanFactory context = new XmlBeanFactory(resource);
```

BeanFactory与ApplicationContext的区别

| BeanFactory            | Spring容器的顶层接口, 采用 **延迟创建** 对象的思想 |
| ---------------------- | -------------------------------------------------- |
| **ApplicationContext** | **BeanFactory的子接口, 采用 即时创建 对象的思想**  |

## 创建对象

### Bean标签

#### 配置文件

```xml
<!--Bean默认是在容器创建的时候创建的-->
 <bean class="xml.User" init-method="init" destroy-method="destroy" lazy-init="true" scope="prototype"/>
```

| 属性           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| id             | 对象的引用名称;一定要唯一； 一次只能定义一个引用名称         |
| name           | 对象的引用名称; 与id区别是：name一次可以定义多个引用名称。   |
| class          | 类的全限定名称                                               |
| init-method    | 指定类中初始化方法的名称，在构造方法执行完毕后立即执行【了解】 |
| destroy-method | 指定类中销毁方法名称，在销毁spring容器前执行【了解】         |
| lazy-init      | 延迟创建对象 (默认值是false: 不延迟);<br />设置为true表示在第一次使用对象的时候才创建，只对单例对象有效。 |
| scope          | 设置bean的作用范围, 取值：<br/>singleton：单例, 默认值; <br/>prototype：多例 (每次使用都创建一个: 不会存储: 不会执行销毁方法) <br/>request：web项目中，将对象存入request域中【了解】<br/>session：web项目中，将对象存入session域中【了解】<br/>globalsession：web项目中，将对象应用于集群环境，没有集群相当于session【了解】 |

#### 注解(生命周期)

| 注解           | 解释                                                         |
| -------------- | ------------------------------------------------------------ |
| @PostConstruct | 修饰方法为一个初始化方法                                     |
| @PreDestroy    | 修饰方法为一个销毁方法                                       |
| @Lazy          | 修饰类, 将对象创建延迟到使用时创建 (影响了生命周期中的创建时间) |
| @Scope         | 如果设置为prototype时会影响生命周期的创建时间(到使用时创建)  |

### 配置文件方式

- 构造方法 (默认:  调用构造方法)

```xml
<!-- 调用构造方法创建对象 -->
<bean id="user" class="com.itheima.ioc.User"/>
```

```java
public User() {
    System.out.println("1. 构造方法执行..");
}
```

- 静态方法 (调用工具类静态方法)

```xml
<bean id="user" class="UserFactory" factory-method="getBean"/>
```

```java
 public static User getBean(){
        return new User();
    }
```

- 实例方法 (调用对象的实例方法)

```xml
<bean id="userFactory" class="com.itheima.ioc.UserFactory"/>
<bean id="user" factory-bean="userFactory" factory-method="get"/>
```

```java
 public User get(){
        return new User();
    }
```

### 注解方式

```xml
<!--
        component-scan: Spring组件扫描配置
            base-package: 指定扫描的路径
    -->
    <context:component-scan base-package="com.itheima.anno"/>
```

| 注解        | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| @Component  | 修饰类, 在三层架构中衍生的业务类注解,不需要识别时可用@Component代替 |
| @Controller | 修饰类, 表示控制层对象, 属性value值用于给对象取名( 默认名称是首字母小写的类名) |
| @Service    | 修饰类, 表示业务层对象, 属性value值用于给对象取名( 默认名称是首字母小写的类名) |
| @Repositor  | 修饰类, 表示持久层对象, 属性value值用于给对象取名( 默认名称是首字母小写的类名) |

## 依赖注入

Spring IOC提供的给对象赋值的功能

### 配置文件方式

#### 注入方式

##### 构造方法

- 构造方法赋值

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

- C标签代替构造方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       <!-- c标签命名空间 -->
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
        本质上是调用构造方法注入
            c:id: 注入id参数的值
            c:name: 注入name参数的值
            c:_1: 指定参数列表中的下标注入值
    -->
    <bean class="com.itheima.xml.Person" c:id="3" c:_1="Jason3" />
```

##### set方法

- set方法赋值

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

- P标签代替set方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:c="http://www.springframework.org/schema/c"
       <!-- p标签命名空间 -->
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
        本质上是调用set方法注入
            p:id: 注指定参数名称注入值
            p:name: 指定参数名称注入值
    -->
<bean class="com.itheima.xml.Person" p:id="4" p:name="Jason4" />
```

#### 注入对象

```xml
<!-- 定义需要注入的对象
        class: 对象的全限定类名
     -->
<bean id="str" class="java.lang.String">
    <constructor-arg value="Jason5"/>
</bean>
<!--
        ref: 引用一个已经存在的对象
    -->
<bean class="xml.Person">
    <property name="name" ref="str"/>
</bean>
```

#### 注入集合

```xml
    <!--
        array: 表示参数的类型名称
            value: 需要注入的值
            entry: 需要注入的entry对象 (key/value)
            prop: 需要注入的prop对象值
    -->
    <bean class="xml.Employee">
        <!--private String[] array;-->
        <property name="array">
            <array>
                <value>1</value>
                <value>2</value>
                <value>3</value>
            </array>
        </property>
        <!--private List<String> list;-->
        <property name="list">
            <list>
                <value>3</value>
                <value>4</value>
            </list>
        </property>
        <!--private Set<String> set;-->
        <property name="set">
            <set>
                <value>5</value>
            </set>
        </property>
        <!--private Map<String, String> map;-->
        <property name="map">
            <map>
                <entry key="id" value="6"/>
                <entry key="name" value="Jason6"/>
            </map>
        </property>
        <!--private Properties props;-->
        <property name="props">
            <props>
                <prop key="id">7</prop>
                <prop key="name">Jason7</prop>
            </props>
        </property>
    </bean>
```

### 注解方式

【同时使用】@Autowired < @Value < @Resource (以优先级最大的为准)

| 注解名     | 位置             | 作用                                         | 意义                                                         | 属性值                                                   |
| ---------- | ---------------- | -------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| @Autowired | 属性, 方法, 参数 | 根据对象(类型+名称)注入属性值                | 代替bean标签中的子标签prototype                              | required: 注入的对象是否必须<br /> (true: 非空)          |
| @Qualifier | 属性, 方法, 参数 | 只根据对象名称注入                           | 通常配合@Autowired使用                                       | value: 注入指定名称的对象                                |
| @Resource  | 属性, 方法, 类   | 根据类型+名称 <br />或 单独指定名称注入      | 相当于@Autowired+@Qualifier <br />( JDK1.8以后不支持 )       | name: 注入指定名称的对象                                 |
| @Autowired | 属性, 方法, 参数 | 注入配置文件的参数 <br />或 基本数据类型数据 | 配合@PropertySource<br />代替<context:property-placeholder标签 | value: 注入指定内容<br /> (可以使用占位符获取参数值注入) |

## 注解与配置的关系

|              | 配置方式 (常用)                         | 注解方式 (常用)                         |
| ------------ | --------------------------------------- | --------------------------------------- |
| Bean定义     | <bean class=""/>                        | @Component                              |
| Bean名称     | bean标签的id或name属性                  | @Component的value值                     |
| Bean注入     | bean标签的子标签<property..             | @Autowired(1) + @Qualifier (?)          |
| Bean生命周期 | bean标签的init- \| destroy- method属性  | @PostConstruct \| @PreDestroy           |
| Bean延迟创建 | bean标签的lazy-init \| scope 属性       | @Lazy \| @Scope                         |
| 适用场景     | 第三方源码类的对象创建                  | 自定义类的简单应用对象创建              |
| 优势         | 解耦, 非侵入性 (代码与配置分开互不干扰) | 简单, 可读性高 (找到类就相当于找到配置) |

##   JdbcTemplate

-  SpringJDBC **封装了基础的 JDBC 操作**，让我们不用去关心 获取驱动、建立连接、关闭连接等非业务操作，让我们更加专注于业务的实现。 
-  SpringJDBC 采用的是 **模板设计模式**, 将 JDBC 封装在 **JdbcTemplate** 模板中, 使操作更简单。

### 案例

```java
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
```

### IOC管理Jdbc

```xml
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
```

```java
  // 1. 创建IOC容器
        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
        JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);
        // 2. 定义sql语句
        String sql = "select * from account";
        // 3. 操作数据库 (返回结果对象)
       //BeanPropertyRowMapper的作用是将结果集转换成对象
        List<Account> all = jdbcTemplate.query(sql, new BeanPropertyRowMapper(Account.class));
        // 4. 处理结果
        all.forEach(x -> System.out.println(x));
```

### 自定义映射器

- 作用: 将结果集( ResultSet )映射到对象( Bean )
- **前提**: 数据库表字段需要与对象属性一致, 否则不起作用

```Java
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

```Java
 // 3. 操作数据库 (返回结果对象)
        List<Account> all = jdbcTemplate.query(sql, new AccountRowMapper());
```

### CRUD

#### 依赖

```xml
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
```



#### 配置文件方式

- 持久层AccountDaoImpl

```java
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

- 业务层AccountServiceImpl

```java
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

- 数据库配置文件db.properties

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

- spring配置文件applicationContext.xml

```xml
<!-- 1. 加载外部配置文件 -->
    <!--
        property-placeholder: 加载指定的配置文件
            location: 本地文件路径
            classpath: 只要在spring配置中, 所有文件路径都需要在文件名前面加上该标识
    -->
    <context:property-placeholder location="classpath:db.properties"/>
    <!-- 2. 创建数据源 (连接池), ${}符号的作用是获取配置文件中的参数值-->
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
```

#### 注解方式

- 持久层AccountDaoImpl

```java
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

- 业务层AccountServiceImpl

```java
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

- spring配置文件applicationContext.xml

```xml
 <!-- 使用了注解需要添加Spring组件扫描配置 -->
    <context:component-scan base-package="crud"/>
```

## 整合Junit框架

### 依赖

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

```java
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
```

# AOP

## 代理模式

提供代理类 **控制(拦截)外部对目标对象的访问** 的一种代码设计

### 静态代理

- 明星接口: Star

```Java
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

- 目标类: LiuStar

```Java
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

- 代理类: LiuStarProxy

```java
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

- 测试: LiuStarProxyTests

```java
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

### 动态代理

代理类在代码运行时创建

#### JDK

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

#### cglib

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

####  两种动态代理的区别

|          | JDK                                           | CGLIB                                    |
| -------- | --------------------------------------------- | ---------------------------------------- |
| 实现方式 | 利用 **实现** 接口方法来拦截目标方法          | 利用 **继承** 覆写方法来拦截目标方法     |
| 性能高低 | JDK1.6以前较Cglib慢; 1.6以及1.7大量调用时较慢 | 1.8时被JDK代理超越                       |
| 适用场景 | 目标对象必须实现接口                          | 目标方法需要允许覆写(不能用final/static) |

## 介绍

- 面向切面编程: AOP (Aspect Oriented Programming) 的简写
- 面向切面编程: 是一种代码设计思想

## 作用

在不修改目标对象的情况下添加新的业务逻辑 (增强)

## 原理

**底层使用了动态代理技术**实现的AOP切面编程思想

```java
// 代理方法
public void sing(Integer money){
    if(money > 10000){ // 新增的业务逻辑 (增强)
        targer.method(money); // 调用目标方法
    }
}
```

## 相关术语

| 术语名 | 解释                                 |
| ------ | ------------------------------------ |
| 连接点 | 目标对象中的所有方法                 |
| 切入点 | 目标对象中需要增强的方法             |
| 通知   | 目标对象需要增强的内容 (和位置)      |
| 切面   | 切面是由**切入点** 和 **通知**组成的 |

## 依赖

```xml
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
```

## 案例

### 自动记录日志

- 业务处理: AccountServiceImpl

```java
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

- 记录日志: LogAdvice

```java
public class LogAdvice {
    /**
     * 增强方法/ 通知方法
     */
    public void log(){
        System.out.println("日志打印..");
    }
}
```

- 配置AOP: applicationContext.xml

```xml
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
```

### AOP动态代理细节

#### 动态代理的类型

- 定义业务接口: AccountService

```Java
public interface AccountService {
    /**
     * 转账业务方法.
     */
    void trans();
}
```

- 实现业务接口: AccountServiceImpl

```java
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

#### 固定使用Cglib

```xml
<!-- 
	<aop:config: 配置AOP
		proxy-target-class: 配置是否强制使用cglib动态代理(默认情况下, 如果目标对象实现了接口则采用jdk, 没有则采用cglib)
-->
<aop:config proxy-target-class="true">
```

## 切入点表达式

| 指示符        | 示例                                                    | 作用(细粒度)        |
| ------------- | ------------------------------------------------------- | ------------------- |
| bean          | bean(accountService)                                    | 精确到IOC容器的bean |
| within        | within(com.itheima.xml.service.impl.AccountServiceImpl) | 精确到类            |
| **execution** | execution(public void com.itheima..save(..))            | **精确到方法**      |

execution(modififiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?) 

- **modififiers-pattern**: 权限访问修饰符 (可填) 
- **ret-type-pattern**: 返回值类型 (必填) 
- **declaring-type-pattern**: 全限定类名 (可填) 
- **name-pattern**: 方法名 (必填) 
- **param-pattern**: 参数名 (必填) 
- **throws-pattern**: 异常类型 (可填) 

## 通知

### 配置文件方式

#### 通知类型

```Java
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

```java
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

```xml
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
```

#### 环绕通知

可以自由控制目标方法已经通知的执行顺序

```java
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

### 注解方式

```java
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

```xml
 <!-- 1. 创建对象 -->
    <context:component-scan base-package="com.itheima.xml"/>
    <!-- 2. Aop配置 -->
    <!--
        aop:aspectj-autoproxy: 开启AOP自动代理 (开启AOP注解支持)
    -->
    <aop:aspectj-autoproxy proxy-target-class="true"/>
</beans>
```

# 事务

## 作用

在业务中 **保证数据安全**

![1565246073783](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1565246073783.png)

## 特性

事务的四大特性也叫事务的 **ACID原则**

| 特性名                | 解释                                 |
| --------------------- | ------------------------------------ |
| 原子性（Atomicity）   | 事务是一个最小的执行单元, 不可再拆分 |
| 一致性（Consistency） | 事务前后的总账需要保持一致           |
| 隔离性（Isolation）   | 多个事务之间不会相互影响, 互不干扰   |
| 持久性（Durability）  | 事务执行结束, 结果会永久的保存起来   |

## 隔离级别

### ISOLATION_DEFAULT

- 默认采用数据库的隔离级别

### ISOLATION_READ_UNCOMMITTED

- 读取未提交数据: 可以读取到其他事务没有提交的数据  (有脏读问题)

| 时间 | 事务A                                                     | 事务B                                                        |
| ---- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 1    | 开启事务                                                  |                                                              |
| 2    |                                                           | 开启事务                                                     |
| 3    | // 原来是小明<br/>update user set name="小白" where id=1; |                                                              |
| 5    |                                                           | <font color='red'>// 结果是小白</font><br/>select name from user where id=1; |
| 6    | 回滚事务                                                  |                                                              |

### ISOLATION_READ_COMMITTED

- 读取已提交数据: 只能读取到其他事务已经提交的数据 (解决了脏读问题, 有不可重复读问题)

| 时间 | 事务A                                                     | 事务B                                                        |
| ---- | --------------------------------------------------------- | ------------------------------------------------------------ |
| 1    | 开启事务                                                  |                                                              |
| 2    |                                                           | 开启事务                                                     |
| 3    |                                                           | // 结果是小明<br/>select name from user where id=1;          |
| 5    | // 原来是小明<br/>update user set name="小白" where id=1; |                                                              |
| 6    | 提交事务                                                  |                                                              |
| 7    |                                                           | <font color='red'>// 结果不一样</font><br/>select name from user where id=1; |

### ISOLATION_REPEATABLE_READ

- 可重复读: 一个事务中的多次查询结果相同 (解决了不可重复读问题, 有幻读问题)

| 时间 | 事务A                                                        | 事务B                                               |
| ---- | ------------------------------------------------------------ | --------------------------------------------------- |
| 1    |                                                              | 开启事务                                            |
| 2    | 开启事务                                                     |                                                     |
| 3    | // 结果是小明<br/>select name from user where id=1;          | // 结果是小明<br/>select name from user where id=1; |
| 5    | <font color='red'>// 不能修改</font><br/>update user set name="小白" where id=1; | // 结果是一样<br/>select name from user where id=1; |
| 6    |                                                              | 提交事务                                            |
| 7    | // 可以修改<br/>update user set name="小白" where id=1;      |                                                     |

### ISOLATION_SERIALIZABLE

- 事务串行化: 序列化: 事务开启后加锁限制其他事务的开启   (建议不要用, 性能极差)

| 时间 | 事务A    | 事务B                                                   |
| ---- | -------- | ------------------------------------------------------- |
| 1    |          | 开启事务                                                |
| 2    |          | // 结果是小明<br/>select name from user where id=1;     |
| 3    |          | // 可以修改<br/>update user set name="小白" where id=1; |
| 5    |          | 提交事务                                                |
| 6    | 开启事务 |                                                         |

## 传播行为

当前方法对事务的要求

![1562831970253](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562831970253.png)

| 传播行为      | 解释                                                         |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 需要: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 创建新的事物运行 |
| SUPPORTS      | 支持: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 则正常运行 |
| MANDATORY     | **强制**: 外部如果有事务, 则加入事务一起运行, 如果外部没有事务, 则抛出异常 |
| REQUIRES_NEW  | 新: 无论外部是否有事务, 都将创建新的事物运行, 如果外部有则挂起 |
| NOT_SUPPORTED | 不支持: 如果外部有事务, 则挂起, 如果外部没有事务, 则正常运行 |
| NEVER         | **决不**: 外部如果有事务, 则抛出异常, 如果没有, 则正常运行   |
| NESTED        | <font color='red'>嵌套</font>:  如果外部有事务, 那么当前事务会嵌套在外部事务中一起运行, 如果外部没有事务, 则REQUIRED |

## 声明式事务

- 通过 **面向切面编程思想** 来实现的事务管理方式
- Spring声明式事务的简写: TX

![1565248277054](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1565248277054.png)

### 相关API

#### PlatformTransactionManager

- 事务管理接口: 定义了提交事务、获取事务、回滚事务的方法。 

![1562574934801](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562574934801.png) 

#### TransactionStatus

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



#### TransactionDefinition

- 事务信息接口: 定义了事务的隔离级别、传播行为、超时时间、是否只读等。

![1562575584950](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring三/1562575584950.png) 

### 案例

#### 依赖

```xml
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
```

#### 配置文件方式

```xml
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
```

#### 注解方式

```Java
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
```

```xml
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
```

## 编程式式事务

- 相关API

| 方法名                           | 解释                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| TransactionTemplate              | 它是事务管理模版类，继承DefaultTransactionDeﬁnition类。用于简化事务管理，事务管理由 模板类定义，主要是通过TransactionCallback回调接口或TransactionCallbackWithoutResult回 调接口指定，通过调用模板类的execute()方法来自动实现事务管理。 |
| TransactionCallback              | 通过实现该接口的“T doInTransaction(TransactionStatus status) ”方法来定义需要事务管理的 操作代码。适合于有返回值的目标方法。 |
| TransactionCallbackWithoutResult | 实现TransactionCallback接口，提供“void doInTransactionWithoutResult(TransactionStatus status)”。适合于不需要返回值的目标方法。 |

```xml
 <!-- 加载配置文件    -->
<context:property-placeholder location="classpath:db.properties"/>
        <!-- 使用注解需要指定扫描路包径    -->
<context:component-scan base-package="com.itheima.xml"/>
        <!-- 配置数据源    (使用druid连接池作为数据源) -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
<property name="driverClassName" value="${db.driver}"/>
<property name="url" value="${db.url}"/>
<property name="username" value="${db.username}"/>
<property name="password" value="${db.password}"/>
</bean>
        <!-- 创建JdbcTemplate对象并添加至IOC容器    -->
<bean id="jdbcTemplate"
      class="org.springframework.jdbc.core.JdbcTemplate">
<property name="dataSource" ref="dataSource"/>
</bean>
        <!-- 配置事务管理器    (切面类/通知类) -->
<bean id="transactionManager"
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="dataSource"/>
</bean>
        <!-- 配置事务管理模板    (代替切面编程) -->
<bean id="transactionTemplate"
      class="org.springframework.transaction.support.TransactionTemplate">
<property name="transactionManager" ref="transactionManager"/>
</bean>
```



```java
@Service
public class AccountServiceImpl implements AccountService {
    @Autowired
    AccountDao accountDao;
    @Autowired
    TransactionTemplate transactionTemplate;
    @Override
    public void save(Account account) {
        // 创建事务回调对象 
        TransactionCallback callback = new TransactionCallback() {
            /**
             * 在事务中运行的代码 
             * @param transactionStatus 当前事务状态 
             * @return 回调返回值
             */
            @Override
            public Object doInTransaction(TransactionStatustransactionStatus) {
                accountDao.save(account);
                int i = 1 / 0;
                return null;
            }
        };
        transactionTemplate.execute(callback);
    }
}
```



