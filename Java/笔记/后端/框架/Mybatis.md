# Mybatis框架

## C3P0

```java
/**
    目标：连接池的概念和分类，以及C3P0连接池的使用。

    连接池的概念：
        是提供了若干个已经做好的连接对象，可供重复使用。
    连接池的作用：
         1)提高创建连接的速度，快速的得到一个连接对象。
         2)提高连接对象的使用率。
         3)提高系统的性能，可以尽力避免服务器奔溃。（只是一种方案）

        创建时 不再自己创建，而是从连接池中获取一个创建好的连接对象
        使用时 与以前的使用方式相同
        关闭时 不再真的关闭连接对象，而是放回到连接池中，供下一个操作继续使用。

    连接池的API（数据源）： 连接池==数据源。
         javax.sql.DataSource ：数据源接口，又称为连接池，只是接口，没有实现，
         由第三方厂商来实现。我们只需要学会使用连接池的API即可。
         JDBC全部是规范，实现交给第三方公司。

    所以使用连接池必须要有两个关键字：
         1）JDBC规范接口：javax.sql.DataSource
         2）连接池的实现厂商。

    常用连接池实现商的介绍：
         1)阿里巴巴-德鲁伊druid连接池：Druid是阿里巴巴开源平台上的一个项目
         2)DBCP(DataBase Connection Pool)数据库连接池，是Apache上的一个Java连接池项目，
              也是Tomcat使用的连接池组件。
         3)C3P0是一个开源的JDBC连接池，目前使用它的开源项目有Hibernate，Spring等。
                C3P0有自动回收空闲连接功能。

    常用连接池参数：
         1.初始连接数 initialPoolSize	一开始连接池创建的时候创建多少个连接对象在连接池中
         2.最大连接数 maxPoolSize	连接池中最多有多少个连接对象
         3.最长等待时间 maxWait	如果连接池中没有空闲连接对象，当前用户等待多久以后抛出异常，单位是毫秒。
         4.最长空闲回收时间	如果一个连接对象在连接池中长时间没有人使用，
                            多久以后被服务器回收。有些连接池并不支持这个功能。默认是0，0表示不回收。
 */
/**
 目标：使用C3P0连接池

 使用连接池必须要有两个关键：
 1）JDBC规范接口：javax.sql.DataSource（接口）
 2）连接池的实现厂商。（实现类）

 使用步骤：
 （1）去官网下载C3p0框架包。（都是一些jar包）
 （2）导入连接池的Jar包到项目中去。
 （3）导入数据库的驱动：连接数据库还是必须要依赖数据库驱动
 （4）找到C3p0的配置文件，放到src下面去。（强制性要求）
 （5）文件名默认应该使用：c3p0-config.xml
 （6）开始写源代码 得到 C3P0连接池。
 （7）使用C3p0连接池得到连接操作数据库。

 C3P0的连接池实现：ComboPooledDataSource实现了javax.sql.DataSource
 构造器：
 -- public ComboPooledDataSource():自动使用src下的默认配置c3p0-config.xml来创建连接池对象返回。
 -- public ComboPooledDataSource(String name):指定加载某个c3p0的核心配置文件创建连接池对象

 javax.sql.DataSource获取连接的API:
 Connection getConnection():从连接池中得到一个连接对象

 小结：
 记住操作和API.
 */
public class C3P0Demo02 {
    public static void main(String[] args) throws Exception {
        // 1.加载src下核心配置文件：c3p0-config.xml。成为一个C3P0的连接池对象（数据源DataSource）
        DataSource dataSource = new ComboPooledDataSource();
        // 加载c3p0-config.xml文件中对应的某个配置
        // DataSource dataSource = new ComboPooledDataSource("config");

        // 2.通过连接池对象获取连接。
        for(int i = 1 ; i <= 10 ; i++ ){
            Connection con = dataSource.getConnection();
            System.out.println("输出第"+i+"个连接："+con);
            if(i==10)con.close(); // 关闭了第10个连接。
        }

        Connection con = dataSource.getConnection();
        System.out.println("输出第11个连接："+con);

        // 2.拼接一个SQL语句
        // 建表
        StringBuilder sb = new StringBuilder();
        sb.append("create table tb_user1(")
                .append("id int primary key auto_increment,")
                .append("loginName varchar(23),")
                .append("userName varchar(23),")
                .append("passWord varchar(23))");

        // 3.获取一个发送SQL语句的对象：
        PreparedStatement pstm = con.prepareStatement(sb.toString());

        // 4.通过Statement开始发送SQL语句
        System.out.println(pstm.execute());
        System.out.println("建表成功！");
        con.close();
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<c3p0-config>
  <!-- 注释 -->
  <default-config>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/day17</property>
    <property name="user">root</property>
    <property name="password">root</property>
  
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">10</property>
    <property name="checkoutTimeout">3000</property>
  </default-config>

  <named-config name="otherc3p0"> 
  </named-config>
</c3p0-config>
```

## druid

```java
/**
    目标：Druid连接池，阿里巴巴实现的连接池使用。

    使用步骤：
     （1）去官网下载Druid框架包。（都是一些jar包）
     （2）导入连接池的Jar包到项目中去。
     （3）导入数据库的驱动（已经做了）：连接数据库还是必须要依赖数据库驱动
     （4）找到Druid的配置文件，放到src下面去。（强制性要求）
            #driverClassName=com.mysql.jdbc.Driver  驱动是自动加载可以省略不写
     （5）核心配置文件名称可以随意。
     （6）开始写源代码

   Class类：
        -- InputStream getResourceAsStream(String path)：
        读取类路径(src目录)下的属性配置文件 , 转成一个字节输入流对象

   Druid创建连接池对象的API:
        DruidDataSourceFactory的方法
        -- public static DataSource createDataSource(Properties properties):
            通过属性对象创建一个数据源。

   小结：
        记住步骤和API.
 */
public class DruidDemo {
    public static void main(String[] args) throws Exception {
        // 1. 读取类路径(src目录)下的属性配置文件，转成一个字节输入流对象
        InputStream is = DruidDemo.class.getResourceAsStream("/druid.properties");
        Properties properties = new Properties();
        properties.load(is);
        // 2.可以通过德鲁伊的数据源工厂类：DruidDataSourceFactory 去加载字节输入流返回一个数据源对象。
        DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
        // 3.通过连接池对象获取连接。
        for(int i = 1 ; i <= 10 ; i++ ){
            Connection con = dataSource.getConnection();
            System.out.println("输出第"+i+"个连接："+con);
            if(i==10)con.close(); // 关闭了第10个连接。
        }

        Connection con = dataSource.getConnection();
        System.out.println("输出第11个连接："+con);

        // 4.拼接一个SQL语句
        // 建表
        StringBuilder sb = new StringBuilder();
        sb.append("create table tb_user2(")
                .append("id int primary key auto_increment,")
                .append("loginName varchar(23),")
                .append("userName varchar(23),")
                .append("passWord varchar(23))");

        // 5.获取一个发送SQL语句的对象。
        PreparedStatement pstm = con.prepareStatement(sb.toString());

        // 6.通过Statement开始发送SQL语句
        System.out.println(pstm.execute());
        System.out.println("建表成功！");
        con.close();
    }
}
```

## 自定义Mybatis框架

### action

#### TestDemo

```java
/**
    目标：完全自己模拟写一个自定义Mybatis框架，实现对象关系映射。

    1.准备sqlMapConfig.xml,导入数据库驱动,准备数据访问层接口和映射文件。
      注意：不需要导入Mybatis框架了，因为要自己写。

    2.提供一个类加载所有的配置文件中的数据(sqlMapConfig.xml，UserMapper.xml文件)。
        提供一个类Configuration负责加载这些xml数据。
        使用dom4J技术解析这些xml文件数据到Configuration类中去
        Configuration类创建对象的时候会自动加载：sqlMapConfig.xml UserMapper.xml中的数据。

    3.自定义Mybatis框架的会话对象：SqlSession,做链接工厂。
         a.自定义一个会话类SqlSession(封装连接对象Connection，提供一个获取接口的代理对象)
         b.从配置文件数据中做一个连接工厂（封装连接池,导入C3p0框架）
         c.连接工厂负责返回一个会话对象sqlSession

    4.拿会话对象操作数据库
        a.先获取与数据库的会话对象。
        b.通过会话对象去得到当前接口的代理对象。(重点，难点)

    小结：
         自定义Mybatis框架。
         0.加载核心配置文件得到configuration对象中去。
         1.得到会话对象-加载核心配置文件得到数据源返回会话对象.
         2.得到会话对象：通过会话对象去得到数据访问层接口的代理对象。
         3.以后我们调用数据访问层接口的方法，其实是去找代理执行。
         4.代理在会话中，会话自带了Connection连接，代理也可以拿到当前Connection连接执行方法的sql语句。
         5.代理加载全部的映射文件数据，根据方法的名称去提取对应映射文件中的sql语句。
         6.执行该sql语句返回结果。


 */
public class TestDemo {
    public static void main(String[] args) throws Exception {
        // a.先获取与数据库的会话对象。
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.通过会话对象去得到当前接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.开始使用数据访问层接口的对象查询数据
        List<User> users = userMapper.selectAllUsers(); // 走代理！
        System.out.println(users);
        // d.释放会话
        sqlSession.close();
    }
}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private String sex ;
    private String address;
    //private Date creataDate;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
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

//    public Date getCreataDate() {
//        return creataDate;
//    }
//
//    public void setCreataDate(Date creataDate) {
//        this.creataDate = creataDate;
//    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                '}';
    }
}
```

### dao

#### UserMapper

```java
public interface UserMapper {
    List<User> selectAllUsers();
}
```

### framework

#### Configuration

```java
public class Configuration {
    private String driver;
    private String url ;
    private String userName ;
    private String passWord;
    // 存储映射文件的数据对象。
    private List<Mapper> mappers = new ArrayList<>();

    public Configuration() {
        try {
            // 数据自加载：加载sqlMapconfig.xml和关联的映射文件的数据到本对象中去。
            loadXmlDataToSelf();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void loadXmlDataToSelf() throws Exception {
        // 1.创建一个dom4j的解析对象
        SAXReader saxReader = new SAXReader();

        // 2.加载被解析的xml文件成为一个字节输入流
        InputStream is =
                Configuration.class.getResourceAsStream("/sqlMapConfig.xml");

        // 3.把字节输入流通过解析对象读成一个document文档对象
        Document document = saxReader.read(is);

        // 4.得到当前文档的根元素对象
        Element root = document.getRootElement();

        // 5.得到environments元素对象
        Element environments = root.element("environments");

        // 6.得到environment子元素
        Element environment = environments.element("environment");

        // 7.得到dataSource子元素
        Element dataSource = environment.element("dataSource");

        // 8.获取dataSource下的四个子元素
        List<Element> properties = dataSource.elements();
        for(Element pro : properties){
            switch (pro.attributeValue("name")){
                case "driver":
                    this.driver = pro.attributeValue("value");
                case "url":
                    this.url = pro.attributeValue("value");
                case "username":
                    this.userName = pro.attributeValue("value");
                case "password":
                    this.passWord = pro.attributeValue("value");
            }
        }

        // 9.得到mappers元素
        Element mappers = root.element("mappers");
        List<Element> mapperPaths = mappers.elements("mapper");
        for(Element mapper : mapperPaths){
            String pathData = mapper.attributeValue("resource");
            // com/itheima/dao/UserMapper.xml
            loadMapperXMLDatas(pathData);
        }

    }

    private void loadMapperXMLDatas(String pathData) throws Exception {
        // 1.创建一个dom4j的解析对象
        SAXReader saxReader = new SAXReader();
        // 2.加载被解析的xml文件成为一个字节输入流
        InputStream is =
                Configuration.class.getResourceAsStream("/"+pathData);
        // 3.把字节输入流通过解析对象读成一个document文档对象
        Document document = saxReader.read(is);

        // 4.得到当前文档的根元素对象
        Element root = document.getRootElement();
        String namespace = root.attributeValue("namespace");

        // 5.拿到所有的子Mapper
        List<Element> mappers = root.elements();
        for(Element sqlEle : mappers){
            Mapper mapper = new Mapper();
            mapper.setNamespace(namespace);
            mapper.setTag(sqlEle.getName());
            mapper.setId(sqlEle.attributeValue("id"));
            mapper.setResultType(sqlEle.attributeValue("resultType"));
            mapper.setSql(sqlEle.getTextTrim());
            this.mappers.add(mapper);
        }
    }


    public String getDriver() {
        return driver;
    }

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassWord() {
        return passWord;
    }

    public void setPassWord(String passWord) {
        this.passWord = passWord;
    }

    public List<Mapper> getMappers() {
        return mappers;
    }

    public void setMappers(List<Mapper> mappers) {
        this.mappers = mappers;
    }

    @Override
    public String toString() {
        return "Configuration{" +
                "driver='" + driver + '\'' +
                ", url='" + url + '\'' +
                ", userName='" + userName + '\'' +
                ", passWord='" + passWord + '\'' +
                ", mappers=" + mappers +
                '}';
    }
}
```

#### Mapper

```java
public class Mapper {
    private String tag; // select delete
    private String id ;
    private String resultType;
    private String sql ;
    private String namespace;

    public Mapper() {
    }


    public String getNamespace() {
        return namespace;
    }

    public void setNamespace(String namespace) {
        this.namespace = namespace;
    }

    public String getTag() {
        return tag;
    }

    public void setTag(String tag) {
        this.tag = tag;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getResultType() {
        return resultType;
    }

    public void setResultType(String resultType) {
        this.resultType = resultType;
    }

    public String getSql() {
        return sql;
    }

    public void setSql(String sql) {
        this.sql = sql;
    }

    @Override
    public String toString() {
        return "Mapper{" +
                "tag='" + tag + '\'' +
                ", id='" + id + '\'' +
                ", resultType='" + resultType + '\'' +
                ", sql='" + sql + '\'' +
                ", namespace='" + namespace + '\'' +
                '}';
    }
}
```

#### SqlSession

```java
public class SqlSession {
    // 会话对象一定要封装连接对象
    private Connection connection;

    public SqlSession() {
    }

    public SqlSession(Connection connection) {
        this.connection = connection;
    }

    public Connection getConnection() {
        return connection;
    }

    public void setConnection(Connection connection) {
        this.connection = connection;
    }

    /**
     * 为当前接口做一个代理对象返回。
     */
    public <T> T getMapper(Class<T> t) {
        /**
         * 参数一：类加载器，加载代理对象。
         * 参数二：代理的方法：就是接口自己里面的方法。接口本身作为参数即可！
         * 参数三：处理函数。
         */
        return (T) Proxy.newProxyInstance(t.getClassLoader(),
                new Class[]{t}, new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                       // method == selectAllUsers
                        String methodName = method.getName();
                        // args == 调用方法的参数值
                        // 代理的使命
                        //   1.拿到连接 connection
                        //   2.执行这个方法具体要干啥？直接根据方法名称拿对于的映射文件中的sql语句。
                        //   3.拿着连接执行这个sql语句，封装结果返回： OK !

                        // a.获取当前执行方法的sql语句 : 直接根据方法名称拿对应的映射文件中的sql语句。
                        // 直接定义一个变量存储包含当前sql语句的mapper
                        Mapper m = null;
                        Configuration configuration = new Configuration();
                        List<Mapper> mappes = configuration.getMappers();
                        for(Mapper mapper : mappes){
                            if(mapper.getId().equals(methodName)){
                                m = mapper;
                                break;
                            }
                        }

                        // 开始执行这个sql语句得到结果
                        PreparedStatement pstm = connection.prepareStatement(m.getSql());
                        ResultSet rs = pstm.executeQuery();
                        // 当前结果对象的类型： m.getResultType()
                        Class c = Class.forName(m.getResultType());
                        List lists = new ArrayList();
                        // 帮它封装集合对象数据！！
                        while(rs.next()){
                            // 创建当前返回的对象。
                            Object obj = c.getDeclaredConstructor().newInstance();
                            // 为当前对象的全部字段赋值成查询的结果集值
                            Field[] fields = c.getDeclaredFields();
                            for(Field f : fields){
                                // 为当前对象的这些字段赋值
                                f.setAccessible(true);
                                f.set(obj, rs.getObject(f.getName()));
                            }
                            lists.add(obj);
                        }
                        return lists;
                    }
                });
    }

    public void close(){
        try {
            if(connection!=null) connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


}
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 创建Mybatis的连接工厂：
 *     封装连接池。
 * 返回一个会话对象。
 * 关闭会话对象。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个连接池对象：用于提供Mybatis的连接。
    // 拿一个C3p0做连接池。
    private static DataSource dataSource;
    static{
        try{
            // 2.解析sqlMapConfig.xml文件创建一个数据源对象。
            // 创建一个Configuration对象代表了核心配置文件的内容。
            Configuration configuration = new Configuration();
            // 3.依据这些数据信息创建一个新的连接池对象。
            // 创建C3p0连接池
            ComboPooledDataSource ds = new ComboPooledDataSource();
            // 注入驱动
            ds.setDriverClass(configuration.getDriver());
            // 注入地址
            ds.setJdbcUrl(configuration.getUrl());
            // 注入用户名
            ds.setUser(configuration.getUserName());
            // 注入密码
            ds.setPassword(configuration.getPassWord());
            dataSource = ds ;
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession() throws Exception {
        // 会话对象中封装了一个来自于数据源的链接对象：Connection
        SqlSession sqlSession = new SqlSession();
        sqlSession.setConnection(dataSource.getConnection());
        return sqlSession;
    }
}
```

### 配置文件

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day17"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 关联系统当前的映射文件 :直接从src下寻找-->
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

## Mybatis入门案例

### action

```java
/**
     目标：Mybatis入门使用。

     准备一些数据：
     create table user (
         id int primary key auto_increment,
         username varchar(20) not null,
         birthday date,
         sex char(1) default '男',
         address varchar(50),
         creata_date datetime
     );
     INSERT INTO USER VALUES (NULL, '孙悟空','1980-10-24','男','花果山水帘洞' , NOW());
     INSERT INTO USER VALUES (NULL, '白骨精','1992-11-12','女','白虎岭白骨洞', NOW());
     INSERT INTO USER VALUES (NULL, '猪八戒','1983-05-20','男','福临山云栈洞', NOW());
     INSERT INTO USER VALUES (NULL, '蜘蛛精','1995-03-22','女','盤丝洞', NOW());

     select * from user;

    详细步骤：
        1.写好软件的分层。
        2.导入Mybatis框架的jar包。
        3.导入具体操作数据库的驱动。
        4.导入log4j日志框架包，可以帮助我们追踪且输出详细的mybatis底层执行日志信息，帮助程序员定位错误，理解技术。
             jar包放在lib下，log4j.properties放在src下。
        5.把Mybatis的核心配置文件（sqlMapConfig.xml）放在src下。
        6.加载核心配置文件创建一个Mybatis的连接数据库的连接工厂：util/MybatisSqlSessionFactoryUtils.java.
        7.创建一个数据访问层dao的接口。
             public interface UserMapper {}
        8.在数据访问层dao中定义一个XML形式的映射文件表示接口的实现类。
             UserMapper.xml
             -- 映射文件的名称建议与接口的名称一致。
             -- 映射文件中namespace命名空间:必须与服务的接口的路径一致。
        9.在核心配置文件sqlMapConfig.xml中关联映射文件路径。
             <mappers>
                 关联系统当前的映射文件
                 <mapper resource="src/com/itheima/dao/UserMapper.xml"></mapper>
             </mappers>
        10.开始查询数据库。
            a.先获取与数据库的会话对象。
            b.创建一个数据访问层接口的代理对象操作数据库。
            c.开始使用数据访问层接口的代理对象查询数据
            d.在数据访问层接口对应的映射文件中配置操作的sql语句。
            e.得到结果。
            f.释放会话.
        11.详细原理继续深入。
 */
public class MybatisDemo {
    @Test
    public void query() {
        // a.先获取与数据库的会话对象。
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.创建一个数据访问层接口的对象操作数据库。
        // 原理：通过会话对象去加载UserMapper.xml文件，把该映射文件
        // 做成当前接口UserMapper的动态代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.开始使用数据访问层接口的对象查询数据
        List<User> users = userMapper.selectAllUsers();
        System.out.println(users);
        // d.释放会话
        MybatisSqlSessionFactoryUtils.close(sqlSession);


    }

    @Test
    public void delete() {
        // a.先获取与数据库的会话对象。
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.创建一个数据访问层接口的代理对象操作数据库。
        // 原理：通过会话对象去加载UserMapper.xml文件，把该映射文件
        // 做成当前接口UserMapper的动态代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.开始使用数据访问层接口的对象操作数据
        int count = userMapper.deleteUserById(2);
        System.out.println("删除："+count);
        // d.提交事物
        sqlSession.commit();
        // d.释放会话
        MybatisSqlSessionFactoryUtils.close(sqlSession);
    }
}

```

### bean

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private char sex ;
    private String address;
    private Date creataDate;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", creataDate=" + creataDate +
                '}';
    }
}
```

### dao

```java
public interface UserMapper {
    List<User> selectAllUsers();
    int deleteUserById(int id);
}
```

### utils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day17"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 关联系统当前的映射文件 :直接从src下寻找-->
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

## queryParam

### action

#### TestDemo

```java
/**
 * 目标：参数查询数据。
 1.一个参数：select * from user where id = #{xxx};
 占位符 #{xxx}，如果只有一个参数值接收，那么占位符中的名称确实可以随便写

 2.两个参数： select * from user where id = #{id} and userName = #{name};
   传入参数的方法必须为参数取别名。
   User selectUserByIdAndName(@Param("id") int id, @Param("name") String userName);
 3.入参是对象，字段可以直接取，实际上还是调用字段的get方法：
     如果参数是对象，里面的自段值可以直接获取。
     入参是User对象：u
     #{id} = u.getId();
     #{userName} = u.getUserName();
     #{address} = u.getAddress()
 */
public class TestDemo {
    /**
     * 需求1：通过id查询用户（1参数）
     */
    @Test
    public void testOneParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = userMapper.selectUserById(2);
        // d.输出对象
        System.out.println(user);
    }

    /**
     * 需求2：通过id和用户名查询用户（2参数）
     */
    @Test
    public void testTwoParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = userMapper.selectUserByIdAndName(1,"孙悟空");
        // d.输出对象
        System.out.println(user);
    }

    /**
     * 需求： 根据名称和地址，id查询用户信息（对象传输）
     */
    @Test
    public void testObjectParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        // 使用对象封装查询的入参
        User u = new User();
        u.setAddress("盤丝洞");
        u.setId(4);
        u.setUserName("蜘蛛精");

        User user = userMapper.selectUserByUser(u);
        // d.输出对象
        System.out.println(user);
    }

}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private char sex ;
    private String address;
    private Date creataDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", creataDate=" + creataDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User selectUserById(int id);

    /**
     * 多个参数，必须取别名，否则识别不了！
     * @param id
     * @param userName
     * @return
     */
    User selectUserByIdAndName(@Param("id") int id, @Param("name") String userName);

    User selectUserByUser(User u);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <!--
        id: 必须与接口中服务的方法方法的名称一致。
        parameterType:查询传输过来的入参（永远可以省略不写）
        resultType:结果类型，申明查询结果集映射的对象类型（如果有结果对象返回，这个必须申明）
        接收参数的占位符格式：#{xxxx}
        注意：如果只是一个参数，那么#{中的内容可以瞎搞}
    -->
    <select id="selectUserById" resultType="com.itheima.bean.User">
        select * from user where id = #{xxx};
    </select>

    <select id="selectUserByIdAndName" resultType="com.itheima.bean.User">
        select * from user where id = #{id} and userName = #{name};
    </select>

    <!--
      如果参数是对象，里面的自段值可以直接获取。
      入参是User对象：u
      #{id} = u.getId();
      #{userName} = u.getUserName();
      #{address} = u.getAddress()
     -->
    <select id="selectUserByUser" resultType="com.itheima.bean.User">
        select * from user where id = #{id} and userName = #{userName} and address = #{address};
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day18"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 关联系统当前的映射文件 :直接从src下寻找-->
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

## configImprove

### action

#### TestDemo

```java
/**
    目标：核心配置文件sqlMapConfig.xml的配置优化。

    1.properties:数据库信息外部配置。
    2.typeAliases：查询结果返回类型的别名配置。
    3.mappers:配置扫描外部的映射文件。
 */
public class TestDemo {
    /**
     * 需求1：通过id查询用户（1参数）
     */
    @Test
    public void testOneParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = userMapper.selectUserById(2);
        // d.输出对象
        System.out.println(user);
    }

    /**
     * 需求2：通过id和用户名查询用户（2参数）
     */
    @Test
    public void testTwoParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = userMapper.selectUserByIdAndName(1,"孙悟空");
        // d.输出对象
        System.out.println(user);
    }

    /**
     * 需求： 根据名称和地址，id查询用户信息（对象传输）
     */
    @Test
    public void testObjectParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        // 使用对象封装查询的入参
        User u = new User();
        u.setAddress("盤丝洞");
        u.setId(4);
        u.setUserName("蜘蛛精");

        User user = userMapper.selectUserByUser(u);
        // d.输出对象
        System.out.println(user);
    }

}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private char sex ;
    private String address;
    private Date creataDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", creataDate=" + creataDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User selectUserById(int id);

    /**
     * 多个参数，必须取别名，否则识别不了！
     * @param id
     * @param userName
     * @return
     */
    User selectUserByIdAndName(@Param("id") int id, @Param("name") String userName);

    User selectUserByUser(User u);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <!--
        id: 必须与接口中服务的方法方法的名称一致。
        parameterType:查询传输过来的入参（永远可以省略不写）
        resultType:结果类型，申明查询结果集映射的对象类型（如果有结果对象返回，这个必须申明）
        接收参数的占位符格式：#{xxxx}
        注意：如果只是一个参数，那么#{中的内容可以瞎搞}
    -->
    <select id="selectUserById" resultType="User">
        select * from user where id = #{xxx};
    </select>

    <select id="selectUserByIdAndName" resultType="User">
        select * from user where id = #{id} and userName = #{name};
    </select>

    <!--
      如果参数是对象，里面的自段值可以直接获取。
      入参是User对象：u
      #{id} = u.getId();
      #{userName} = u.getUserName();
      #{address} = u.getAddress()
     -->
    <select id="selectUserByUser" resultType="User">
        select * from user where id = #{id} and userName = #{userName} and address = #{address};
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## queryLike

### action

#### TestDemo

```java
/**
    目标：Mybaits对于模糊查询的支持。
 */
public class TestDemo {
    /**
     * 需求1：查询包含“精”的用户。
     */
    @Test
    public void testOneParamSelect(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        // List<User> users = userMapper.selectUsersByName("精");
        // List<User> users = userMapper.selectUsersByName1("精");
        List<User> users = userMapper.selectUsersByName2("%精%");
        // d.输出对象
        System.out.println(users);
    }



}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private char sex ;
    private String address;
    private Date creataDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", creataDate=" + creataDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {

    List<User> selectUsersByName(String name);
    List<User> selectUsersByName1(String name);
    List<User> selectUsersByName2(String name);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <!-- 1.模糊匹配前后粘连% ，但是占位符必须是$
        此时取参数，名称不能瞎搞，必须用value
     -->
    <select id="selectUsersByName" resultType="User">
        select * from user where userName like '%${value}%'
    </select>

    <!-- 2.可以直接用CONCAT前后拼接%%，中间可以随便写！-->
    <select id="selectUsersByName1" resultType="User">
        select * from user where userName like CONCAT('%',#{xxxx},'%')
    </select>

    <!-- 3.Java代码的业务帮助维护模糊查询的参数，Mybaits不做任何参数处理-->
    <select id="selectUsersByName2" resultType="User">
        select * from user where userName like #{xxxx}
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## dml

### action

#### TestDemo

```java
/**
 目标：Mybaits对于DML操作

 1.Mybaits默认开启了事物,DML 必须提交事物才能有效。
 */
public class TestDemo {
    /**
     * 需求1：插入用户数据。建议用对象传输。
     */
    @Test
    public void testInsert(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("铁扇公主");
        user.setAddress("火焰山");
        user.setBirthday("1996-10-23");
        user.setCreataDate(new Date());
        user.setSex('仙');
        int count = userMapper.insertUser(user);
        // d.输出对象
        System.out.println("插入："+count+"条数据!");
        // 提交事物！
        sqlSession.commit();
    }

    /**
     * 需求2：插入记录返回主键。主键最终返回给对象的字段id中，我们就可以拿到了。
     */
    @Test
    public void testInsertGetKey(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("牛魔王");
        user.setAddress("火焰山");
        user.setBirthday("1997-10-23");
        user.setCreataDate(new Date());
        user.setSex('魔');
        int count = userMapper.insertUserGetKey(user);
        // d.输出对象
        System.out.println("插入："+count+"条数据!");
        System.out.println("主键："+user.getId());
        // 提交事物！
        sqlSession.commit();
    }

    /**
     * 需求3：插入记录返回主键。主键最终返回给对象的字段id中，我们就可以拿到了。
     */
    @Test
    public void testInsertGetKey2(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("牛魔王");
        user.setAddress("火焰山");
        user.setBirthday("1997-10-23");
        user.setCreataDate(new Date());
        user.setSex('魔');
        int count = userMapper.insertUserGetKey2(user);
        // d.输出对象
        System.out.println("插入："+count+"条数据!");
        System.out.println("主键："+user.getId());
        // 提交事物！
        sqlSession.commit();
    }

    /**
     * 需求4：修改数据
     */
    @Test
    public void updateUser(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setId(9);
        user.setUserName("大魔头");
        user.setAddress("狐狸洞");
        user.setBirthday("0023-10-23");
        user.setSex('魔');
        int count = userMapper.updateUser(user);
        // d.输出对象
        System.out.println("修改了："+count+"条数据!");
        // 提交事物！
        sqlSession.commit();
        sqlSession.close();
    }

    /**
     * 需求5： 根据id删除用户。
     */
    @Test
    public void deleteById(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        int count = userMapper.deleteById(8);
        // d.输出对象
        System.out.println("删除了："+count+"条数据!");
        // 提交事物！
        sqlSession.commit();
        sqlSession.close();
    }
}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private String address;
    private Date creataDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", creataDate=" + creataDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    int insertUser(User user);
    int insertUserGetKey(User user);
    int insertUserGetKey2(User user);
    int updateUser(User user);
    int deleteById(int id);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserMapper">
   <insert id="insertUser">
            INSERT INTO `user` (
              `username`,
              `birthday`,
              `sex`,
              `address`,
              `creata_date`
            ) VALUES(
                #{userName},
                #{birthday},
                #{sex},
                #{address},
                #{creataDate}
              )
   </insert>

    <!--插入数据获取主键第一种方式-->
    <insert id="insertUserGetKey">
            INSERT INTO `user` (
              `username`,
              `birthday`,
              `sex`,
              `address`,
              `creata_date`
            ) VALUES(
                #{userName},
                #{birthday},
                #{sex},
                #{address},
                #{creataDate}
              )
        <!--
        keyColumn:主键在表中对应的列名
        keyProperty:主键在实体类中对应的属性名
        resultType:主键的数据类型
        order:
          BEFORE: 在添加语句前执行查询主键的语句
          AFTER: 在添加语句后执行查询主键的语句
        最终：把数据主键注入到当前对象的id字段中去
                user.setId(主键)
        -->
        <selectKey keyColumn="id" keyProperty="id" resultType="int" order="AFTER">
            select last_insert_id()
        </selectKey>
   </insert>
    <!--插入数据获取主键第二种方式
           useGeneratedKeys="true"：使用主键值。
           keyColumn:主键在表中对应的列名
           keyProperty:主键在实体类中对应的属性名
           最终：把数据主键注入到当前对象的id字段中去
                user.setId(主键)
    -->
    <insert id="insertUserGetKey2" useGeneratedKeys="true"
            keyColumn="id" keyProperty="id" >
        INSERT INTO `user` (
        `username`,
        `birthday`,
        `sex`,
        `address`,
        `creata_date`
        ) VALUES(
        #{userName},
        #{birthday},
        #{sex},
        #{address},
        #{creataDate}
        )
    </insert>

    <update id="updateUser" >
        UPDATE
          `user`
        SET
          `username` = #{userName},
          `birthday` = #{birthday},
          `sex` = #{sex},
          `address` = #{address}
        WHERE `id` = #{id} ;
    </update>
    <delete id="deleteById">
        delete from user where id = #{xxxx}
    </delete>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## paramImprove

### action

#### TestDemo

```java
/**
    目标：Mybatis的参数类型。
    1.基本数据类型。（int 删除id值为2的数据）
    2.对象数据类型（User 添加用户对象：user , Map）
    3.复合参数类型：User(Teacher) User(Address)
         #{address.name} = user.getAddress().getName();
 */
public class TestDemo {
    /**
     * 需求1： 复合参数类型传输查询数据。
     */
    @Test
    public void selectUserByAddrAndName(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("蜘蛛精");
        Address addr = new Address();
        addr.setName("盤丝洞");
        user.setAddress(addr);

        List<User> users = userMapper.selectUserByAddrAndName(user);
        // d.输出对象
        System.out.println(users);
        // 提交事物！
        sqlSession.commit();
        sqlSession.close();
    }
}
```

### bean

#### Address

```java
public class Address {
    private int id ;
    private String name ;
    private String code ;
    private double c ;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public double getC() {
        return c;
    }

    public void setC(double c) {
        this.c = c;
    }

    @Override
    public String toString() {
        return "Address{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", code='" + code + '\'' +
                ", c=" + c +
                '}';
    }
}
```

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private Date creataDate;
    private Address address;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public Date getCreataDate() {
        return creataDate;
    }

    public void setCreataDate(Date creataDate) {
        this.creataDate = creataDate;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", creataDate=" + creataDate +
                ", address=" + address +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    List<User> selectUserByAddrAndName(User user);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <select id="selectUserByAddrAndName" resultType="User">
        select * from user where username=#{userName} and address = #{address.name}
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## result

### action

#### TestDemo

```java
/**
    目标：结果类型的深入：ResultType / ResultMap.

    1.如果查询结果的结果是存在的直接类型：映射文件中ResultType必须申明类型，否则报错！
    2.ResultMap（结果集映射成自定义结果集）：解决数据库字段不能与实体字段自动映射的情况。
        a.只有实体类字段与数据库名称类型一致的字段才可以被自动映射数据进入。
        b.如果实体类字段与数据库字段不一致如何解决呢：ResultMap可以解决。
 */
public class TestDemo {
    /**
     * 需求1： ResultType结果类型申明的深入。
     * 如果是查询结果：映射文件中ResultType必须申明类型，否则报错！
     */
    @Test
    public void selectUserByAddrAndName(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        int nums = userMapper.countUsers();
        // d.输出对象
        System.out.println(nums);
        // 提交事物！
        sqlSession.commit();
        sqlSession.close();
    }

    /**
     * 需求2：ResultMap解决数据库字段不能与实体字段自动映射的情况。
     */
    @Test
    public void selectAll(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        List<User> users = userMapper.selectAll();
        // d.输出对象
        System.out.println(users);
        sqlSession.close();
    }
}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private String address;
    private Date createDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", createDate=" + createDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    int countUsers();
    List<User> selectAll();

}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 手工配置结果集类型映射 :主要是配置不能自动映射的字段值-->
    <resultMap id="userResultMap" type="User">
        <!--
        id表示定义主键列
        property: 实体类中属性名
        column: 表中主键列
        -->
        <!--<id property="id" column="id"/>-->
        <result column="create_date" property="createDate"></result>
    </resultMap>

    <select id="countUsers" resultType="int">
        select count(*) from user
    </select>

    <!-- 这个查询的结果集映射，采用自定义的结果映射配置：userResultMap-->
    <select id="selectAll" resultMap="userResultMap">
        select * from user
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## dynamicSQL

### action

#### TestDemo

```java
/**
    目标：动态sql.
       需求：查询条件可能是：姓名，地址，性别。三个参数有多少个不确定的有则一起查询。
 */
public class TestDemo {

    /**
     * 需求：查询条件可能是：姓名，地址，性别。三个参数有多少个不确定的有则一起查询。
     */
    @Test
    public void selectUserDynicmic(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("%精%");
        user.setAddress("%洞%");
        user.setSex('女');
        // d.开始查询
        //List<User> users = userMapper.selectUserDynicmic(user);
        List<User> users = userMapper.selectUserDynicmic02(user);
        System.out.println(users);
        sqlSession.close();
    }

    /**
     * 需求：修改名称，地址，性别。
     * 有这个参数传入则修改，没有则不修改（动态sql）
     */
     @Test
     public void updateUserDynicmic(){
         // a.获取会话对象
         SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
         // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
         UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
         // c.通过数据访问层接口调用方法查询数据: select/find/get
         User user = new User();
         user.setId(3); // 修改一般要带id值
         user.setUserName("嫦娥仙子");
         user.setAddress("广寒宫");
         user.setSex('女');

         int count = userMapper.updateUserDynicmic(user);
         System.out.println("修改了："+count+"条数!");
         sqlSession.commit();
         sqlSession.close();
     }


}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private String address;
    private Date createDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", createDate=" + createDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    List<User> selectUserDynicmic(User user);
    List<User> selectUserDynicmic02(User user);
    int updateUserDynicmic(User user);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 手工配置结果集类型映射 :主要是配置不能自动映射的字段值-->
    <resultMap id="userResultMap" type="User">
        <result column="create_date" property="createDate"></result>
    </resultMap>


    <!-- 这个查询的结果集映射，采用自定义的结果映射配置：userResultMap
            user: (userName , sex , address)
        -->
    <select id="selectUserDynicmic" resultMap="userResultMap">
        select * from user where 1=1
        <if test="userName !=null and userName!='' ">
            and username like #{userName}
        </if>
        <if test="sex!=''">
            and sex = #{sex}
        </if>
        <if test="address !=null and address!='' ">
            and address like #{address}
        </if>
    </select>

    <select id="selectUserDynicmic02" resultMap="userResultMap">
        select * from user
        <!--
            where: 相当于Where关键字，可以去掉多余的or and where
            where加上去，后面的动态sql可以都直接加and,无需担心语法错误！
                第一个条件前面的and可写可不写。
            -->
        <where>
            <if test="userName !=null and userName!='' ">
                username like #{userName}
            </if>
            <if test="sex!=''">
                and sex = #{sex}
            </if>
            <if test="address !=null and address!='' ">
                and address like #{address}
            </if>
        </where>
    </select>

   <!-- user: (id userName , sex , address)-->
    <update id="updateUserDynicmic">
        update user
        <!--
             set: 更新，去掉多余的逗号
            set加上去，后面的动态sql可以都直接加",",无需担心语法错误！
            最后一个条件结尾的”,“可写可不写！
        -->
        <set>
            <if test="userName !=null and userName!='' ">
                 username = #{userName} ,
            </if>
            <if test="sex!=''">
                sex = #{sex} ,
            </if>
            <if test="address !=null and address!='' ">
                 address = #{address}
            </if>
        </set>
        where id = #{id}
    </update>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## foreach

### action

#### TestDemo

```java
/**
    目标：参数类型是集合或者数组，映射的文件处理方式。
 */
public class TestDemo {

    /**
     *  需求：批量插入三个用户对象数据。
     */
    @Test
    public void selectUserDynicmic(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        List<User> users = new ArrayList<>();
        // (String userName, String birthday, char sex, String address, Date createDate) {
        users.add(new User("唐唐1","1000-10-11",'男',"大唐1",new Date()));
        users.add(new User("唐唐2","1000-10-12",'男',"大唐2",new Date()));
        users.add(new User("唐唐3","1000-10-13",'男',"大唐3",new Date()));
        // d.开始
        int count  = userMapper.batchInsertUser(users);
        System.out.println(count+"条数据被保存！");
        sqlSession.commit();
        sqlSession.close();
    }

    /**
     * 需求：参数类型是数组，可以批量删除。
     * 批量删除 1 3 9的用户数据。
     */
    @Test
    public void batchDelete(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        String[] ids = new String[]{"1" , "3" , "9"};
        int count =  userMapper.batchDeleteUsers(ids);
        System.out.println(count+"条数据被删除！");
        sqlSession.commit();
        sqlSession.close();
    }

}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private String address;
    private Date createDate;

    public User() {
    }

    public User( String userName, String birthday, char sex, String address, Date createDate) {
        this.userName = userName;
        this.birthday = birthday;
        this.sex = sex;
        this.address = address;
        this.createDate = createDate;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", createDate=" + createDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    int batchInsertUser(List<User> users);
    int batchDeleteUsers(String[] ids);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <!-- List(user1 , user2 , user3)-->
    <insert id="batchInsertUser">
        INSERT INTO `user` (
        `username`,
        `birthday`,
        `sex`,
        `address`,
        `create_date`
        ) VALUES
        <!--
            collection: 两个取值：list表示集合 array表示数组
            item:变量集合的变量，名称可以随意，是一个遍历集合的游标变量。
            separator:每次遍历后添加分隔符
        -->
        <foreach collection="list" item="u" separator=",">
            (#{u.userName} , #{u.birthday} , #{u.sex} , #{u.address} , #{u.createDate})
        </foreach>
    </insert>

    <!-- arrray : [1 , 3 , 9]-->
    <delete id="batchDeleteUsers" >
        delete from user where id in
        <!-- open拼接以什么开始 :( -->
        <!-- item以什么遍历，separator以什么隔开 : 1, 3, 9 -->
        <!-- close拼接以什么结束: ) -->
        <foreach collection="array" open="(" item="id" separator="," close=")">
            #{id}
        </foreach>
    </delete>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## sqlInclude

### action

#### TestDemo

```java
/**
    目标：sql标签与include标签.
    如果映射文件中存在大量重复的sql代码。我们可以把这些相同的代码提到一个
    <sql>标签中</sql>。然后哪里需要这个sql哪里就include
 */
public class TestDemo {

    /**
     * 需求：查询条件可能是：姓名，地址，性别。三个参数有多少个不确定的有则一起查询。
     */
    @Test
    public void selectUserDynicmic(){
        // a.获取会话对象
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.通过数据访问层接口调用方法查询数据: select/find/get
        User user = new User();
        user.setUserName("%精%");
        user.setAddress("%洞%");
        user.setSex('女');
        // d.开始查询
        //List<User> users = userMapper.selectUserDynicmic(user);
        List<User> users = userMapper.selectUserDynicmic02(user);
        System.out.println(users);
        sqlSession.close();
    }

    /**
     * 需求：修改名称，地址，性别。
     * 有这个参数传入则修改，没有则不修改（动态sql）
     */
     @Test
     public void updateUserDynicmic(){
         // a.获取会话对象
         SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
         // b.获取数据访问层接口的代理对象：原理是加载映射文件成为当前数据访问层接口的代理对象。
         UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
         // c.通过数据访问层接口调用方法查询数据: select/find/get
         User user = new User();
         user.setId(3); // 修改一般要带id值
         user.setUserName("嫦娥仙子");
         user.setAddress("广寒宫");
         user.setSex('女');

         int count = userMapper.updateUserDynicmic(user);
         System.out.println("修改了："+count+"条数!");
         sqlSession.commit();
         sqlSession.close();
     }


}
```

### bean

#### User

```java
public class User {
    private int id ;
    private String userName ;
    private String birthday ;
    private char sex ;
    private String address;
    private Date createDate;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", userName='" + userName + '\'' +
                ", birthday=" + birthday +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", createDate=" + createDate +
                '}';
    }
}
```

### dao

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    List<User> selectUserDynicmic(User user);
    List<User> selectUserDynicmic02(User user);
    int updateUserDynicmic(User user);
}
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day18
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## 1-1

### action

#### TestDemo

```java
/**
    目标：搭建Mybatis的1-1测试环境和架构。实现1-1的关联映射多表查询。

    1.准备sql数据.
    2.搭建Mybatis框架的1-1代码。
    3.为实体类User和UserCard建立关联关系。
    4.配置数据访问层接口的映射文件和接口： 一张表对应一个接口和映射文件。
    5.实现1-1关联查询的技术（重点）
        1-1的关联字段映射使用的标签必须是：association
        关联映射后，所有的字段都要映射，即使名称和数据库一致也要配置。（这种方案其实很不好）

 */
public class TestDemo {
    /**
     * 需求：查询2号用户信息和她的身份证信息。
     * SELECT * FROM USER JOIN user_card ON user.`id` = user_card.`id` WHERE user.id = 2
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        System.out.println(user);
        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1:定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}

```

#### UserCardMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">


</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 身份证的结果集自定义映射-->
    <resultMap id="userCardResultMap" type="UserCard">
        <id property="id" column="id"/>
        <result property="height" column="height"/>
        <result property="weight" column="weight"/>
        <result property="married" column="married"/>
    </resultMap>

    <!-- 用户信息的自定义结果集映射-->
    <resultMap id="userResultMap" type="User">
        <!--映射主键-->
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="birthday" column="birthday"/>
        <result property="sex" column="sex"/>
        <result property="address" column="address"/>
    </resultMap>

    <!-- 配置用户和身份证的1-1自定义结果集映射
        extends="userResultMap"：代表先使用用户的自定义结果集映射用户对象的字段值。
        association：为关联的对象映射字段值。
        所有的字段映射都必须配置。
    -->
    <resultMap id="userResultMap1_1" type="user" extends="userResultMap">
        <!--一对一关联的字段映射必须用：association-->
        <association property="userCard" resultMap="userCardResultMap"/>
    </resultMap>

    <select id="getUserById" resultMap="userResultMap1_1">
         SELECT * FROM USER JOIN user_card ON user.`id` = user_card.`id`
         WHERE user.id = #{id}
    </select>

</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## 1-N

### action

#### TestDemo

```java
/**
    目标：搭建Mybatis的1-1测试环境和架构。实现1-1的关联映射多表查询。

    1.准备sql数据.
    2.搭建Mybatis框架的1-1代码。
    3.为实体类User和Order建立关联关系。
    4.配置数据访问层接口的映射文件和接口： 一张表对应一个接口和映射文件。
    5.实现1-N关联查询的技术（重点）


 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息和她的全部订单信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        System.out.println(user);

        List<Order> orders = user.getOrders();
        orders.forEach(s -> {
            System.out.println(s);
        });
        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### OrderMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
}
```

#### OrderMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.OrderMapper">
</mapper>
```

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

#### UserCardMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 用户信息的自定义结果集映射-->
    <resultMap id="userResultMap" type="User">
        <!--映射主键-->
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="birthday" column="birthday"/>
        <result property="sex" column="sex"/>
        <result property="address" column="address"/>
    </resultMap>

    <!-- 订单字段的结果集映射-->
    <resultMap id="orderResultMap" type="Order">
        <id property="oid" column="o_id"/>
        <result property="number" column="number"/>
        <result property="createDate" column="create_time"/>
        <result property="note" column="note"/>
    </resultMap>

    <!-- 配置用户和身份证的1-1自定义结果集映射
        extends="userResultMap"：代表先使用用户的自定义结果集映射用户对象的字段值。
    -->
    <resultMap id="userResultMap1_1" type="user" extends="userResultMap">
        <!--
            注意：1-N的关联字段是集合，关联映射的时候必须用collection标签
            property: 对象关联的多的一方的字段名称：orders
            javaType：对象关联的多的一方的数据类型:list集合
            ofType: 对象关联的多的一方中的每个元素的类型
            resultMap：多方的结果集映射
            -->
        <collection property="orders"
                    javaType="list" ofType="Order" resultMap="orderResultMap"/>
    </resultMap>

    <select id="getUserById" resultMap="userResultMap1_1">
            SELECT * FROM USER u JOIN tb_order o ON u.id = o.user_id
            WHERE u.id = #{xxxx}
    </select>

</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## N-N

### action

#### TestDemo

```java
/**
    目标：搭建Mybatis的1-1测试环境和架构。实现1-1的关联映射多表查询。

    1.准备sql数据.
    2.搭建Mybatis框架的N-N代码。
    3.为实体类User和Role建立关联关系。
    4.配置数据访问层接口的映射文件和接口： 一张表对应一个接口和映射文件。
    5.实现N-N关联查询的技术（重点）
        与1-N的做法是完全一样的。

 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息和她的全部角色信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        System.out.println(user);

        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### Role

```java
public class Role {
    private int roleId ;
    private String roleName;
    private String roleDetail;

    public int getRoleId() {
        return roleId;
    }

    public void setRoleId(int roleId) {
        this.roleId = roleId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDetail() {
        return roleDetail;
    }

    public void setRoleDetail(String roleDetail) {
        this.roleDetail = roleDetail;
    }

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDetail='" + roleDetail + '\'' +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;
    // N-N: 虽然用户和角色是多对多的关系。但是一个用户对象对应一批角色对象在代码中相当于1-N
    private List<Role> roles;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                ", roles=" + roles +
                '}';
    }
}
```

### dao

#### OrderMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
}
```

#### OrderMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.OrderMapper">
</mapper>
```

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

#### UserCardMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 用户信息的自定义结果集映射-->
    <resultMap id="userResultMap" type="User">
        <!--映射主键-->
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="birthday" column="birthday"/>
        <result property="sex" column="sex"/>
        <result property="address" column="address"/>
    </resultMap>

    <resultMap id="roleResultMap" type="Role">
        <id column="role_id" property="roleId"></id>
        <result column="role_name" property="roleName"></result>
        <result column="role_detail" property="roleDetail"></result>
    </resultMap>

    <!-- 配置用户和身份证的1-1自定义结果集映射
        extends="userResultMap"：代表先使用用户的自定义结果集映射用户对象的字段值。
    -->
    <resultMap id="userResultMap1_1" type="user" extends="userResultMap">
        <!--
            注意：N-N的关联字段是集合跟1-N是完全一样的，关联映射的时候必须用collection标签
            property: 对象关联的多的一方的字段名称：orders
            javaType：对象关联的多的一方的数据类型:list集合
            ofType: 对象关联的多的一方中的每个元素的类型
            resultMap：多方的结果集映射
            -->
        <collection property="roles"
                    javaType="list" ofType="Role" resultMap="roleResultMap" />
    </resultMap>

    <select id="getUserById" resultMap="userResultMap1_1">
        SELECT * FROM USER u JOIN user_role ur ON u.`id`=ur.`user_id`
        JOIN role r ON ur.`role_id` = r.`role_id` WHERE u.`id` = #{id}
    </select>

</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
    <properties resource="db.properties"></properties>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
     <properties resource="db.properties">
          备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
         <property name="username" value="root"/>
         <property name="password" value="dlei"/>
     </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## 1-1-improve

### action

#### TestDemo

```java
/**
    目标：实现1-1的关联映射多表查询的新方式

    1.准备1-1的查询环境，使用了1-1的新方式：

    2.在这种1-1的新方式中可以实现延迟加载技术：
        在关联映射下，一开始不要立即去查询关联对象的信息，等待需要的时候才去查询，这就是延迟加载。
        开启延迟加载。直接在核心配置中加入配置即可：
        <setting name="lazyLoadingEnabled" value="true"/>
 */
public class TestDemo {
    /**
     * 需求：查询2号用户信息和她的身份证信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        // 这里包括以上是没有查询用户对象的身份证对象的，原因是开启了延迟加载！
        System.out.println(user.getAddress());
        System.out.println("-----------------");
        // 这里开始要延迟加载的身份证对象，开始触发身份证信息的查!
        System.out.println(user.getUserCard());
        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1:定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
    // 根据id查询出身份证对象
    UserCard getUserCardById(int id);
}

```

#### UserCardMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
    <select id="getUserCardById" resultType="UserCard">
        select * from user_card where id = #{xxx}
    </select>
</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">

    <!-- 配置用户和身份证的1-1自定义结果集映射
        extends="userResultMap"：代表先使用用户的自定义结果集映射用户对象的字段值。
        association：为关联的对象映射字段值。
    -->
    <resultMap id="userResultMap1_1" type="User">
        <id column="id" property="id"></id>
        <!--一对一关联的字段映射必须用：association
            把这个关联的对象给查询出来注入给当前用户对象的身份证字段
            column:通过哪个字段去查询。
            javaType：查询出来的对象类型UserCard
        -->
        <association property="userCard" column="id" javaType="UserCard"
                     select="com.itheima.dao.UserCardMapper.getUserCardById"/>
    </resultMap>

    <select id="getUserById" resultMap="userResultMap1_1">
        select * from user where id = #{xxx}
    </select>

</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

    <!--配置mybatis中全局设置-->
    <settings>
        <!--开启延迟加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## 1-N-improve

### action

#### TestDemo

```java
/**
    目标：搭建Mybatis的1-1测试环境和架构。实现1-1的关联映射多表查询。

    1.准备sql数据.
    2.搭建Mybatis框架的1-1代码。
    3.为实体类User和Order建立关联关系。
    4.配置数据访问层接口的映射文件和接口： 一张表对应一个接口和映射文件。
    5.实现1-N关联查询的技术（重点）


 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息和她的全部订单信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        System.out.println(user.getUsername());
        System.out.println("--------------------");
        // 开始真正去触发延迟加载中没有加载的订单数据！这里真正查询订单数据了！
        List<Order> orders = user.getOrders();
        orders.forEach(s -> {
            System.out.println(s);
        });
        // d.关闭会话
        sqlSession.close();
    }
}

```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### OrderMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
    List<Order> getOrdersByUserId(int id);
}

```

#### OrderMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.OrderMapper">
    <select id="getOrdersByUserId" resultType="Order">
        select * , o_id as oid , create_time as createDate from tb_order
        where user_id = #{zdsad}
    </select>
</mapper>
```

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

#### UserCardMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}

```

#### UserMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.UserMapper">
    <!-- 配置用户和身份证的1-1自定义结果集映射
        extends="userResultMap"：代表先使用用户的自定义结果集映射用户对象的字段值。
    -->
    <resultMap id="userResultMap1_1" type="User" >
        <id column="id" property="id"></id>
        <!--
            注意：1-N的关联字段是集合，关联映射的时候必须用collection标签
                property: 对象关联的多的一方的字段名称：orders
                javaType：对象关联的多的一方的数据类型:list集合
                ofType: 对象关联的多的一方中的每个元素的类型
                 fetchType="eager"立即加载 默认的
                 fetchType="lazy"延迟加载
                resultMap：多方的结果集映射
            -->
        <collection property="orders" column="id"
                    select="com.itheima.dao.OrderMapper.getOrdersByUserId"/>
    </resultMap>

    <select id="getUserById" resultMap="userResultMap1_1">
           select * from user where id = #{xxxd}
    </select>
</mapper>
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

   <!--配置mybatis中全局设置-->
    <settings>
        <!--开启延迟加载 -->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## annotation

### action

#### TestDemo

```java
/**
    目标：Mybatis的注解操作开发。

 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        User user = userMapper.getUserById(2);
        System.out.println(user);
        // d.关闭会话
        sqlSession.close();
    }

    @Test
    public void delete(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        int count = userMapper.deleteById(5);
        System.out.println("删除了："+count+"条数据！");
        // d.关闭会话
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void add(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        User user = new User();
        user.setUsername("牛魔王");
        user.setAddress("火焰山");
        user.setBirthday("1997-10-23");
        user.setSex('魔');
        int count = userMapper.addUser(user);
        System.out.println("插入了："+count+"条数据！");
        // d.关闭会话
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void addGetKey(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        User user = new User();
        user.setUsername("牛魔王");
        user.setAddress("火焰山");
        user.setBirthday("1997-10-23");
        user.setSex('魔');
        int count = userMapper.addGetKey(user);
        System.out.println("插入了："+count+"条数据！");
        System.out.println("主键："+user.getId());
        // d.关闭会话
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void selectResult(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        RoleMapper roleMapper = sqlSession.getMapper(RoleMapper.class);

        List<Role> roles = roleMapper.selectAllRoles();
        System.out.println(roles);
        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### Role

```java
public class Role {
    private int roleId ;
    private String roleName;
    private String roleDetail;

    public int getRoleId() {
        return roleId;
    }

    public void setRoleId(int roleId) {
        this.roleId = roleId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDetail() {
        return roleDetail;
    }

    public void setRoleDetail(String roleDetail) {
        this.roleDetail = roleDetail;
    }

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDetail='" + roleDetail + '\'' +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### RoleMapper

```java
public interface RoleMapper {
    //@Select("select role_id roleId , role_name roleName , role_detail roleDetail from role")
    @Select("select * from role")
    @Results({
            @Result(column = "role_id", property = "roleId"),
            @Result(column = "role_name", property = "roleName"),
            @Result(column = "role_detail", property = "roleDetail")
    })
//  @ResultMap("roleResultMap"): 直接去映射文件中找结果集配置。
    List<Role> selectAllRoles();

}
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    @Select("select * from user where id = #{xxx}")
    User getUserById(int id);

    @Insert("insert into user(username , address, birthday,sex) values" +
            "(#{username} , #{address},#{birthday},#{sex} )")
    int addUser(User user);

    @Delete("delete from user where id = #{xxxx}")
    int deleteById(int id);

    @Insert("insert into user(username , address, birthday,sex) values" +
            "(#{username} , #{address},#{birthday},#{sex} )")
    @SelectKey(statement = "select last_insert_id()", keyProperty = "id",
            keyColumn = "id", resultType = Integer.class, before = false)
    int addGetKey(User user);
}
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
   <properties resource="db.properties"></properties>

  <!-- 加载系统属性文件，默认直接去src类路径下寻找文件
   <properties resource="db.properties">
        备用配置：优先用外部配置，如果外部没有这个配置再用这里的配置！
       <property name="username" value="root"/>
       <property name="password" value="dlei"/>
   </properties>-->

    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## 1-N-annotation

### action

#### TestDemo

```java
/**
    目标：搭建Mybatis的1-1测试环境和架构。实现1-1的关联映射多表查询。

    1.准备sql数据.
    2.搭建Mybatis框架的N-N代码。
    3.为实体类User和Role建立关联关系。
    4.配置数据访问层接口的映射文件和接口： 一张表对应一个接口和映射文件。
    5.实现N-N关联查询的技术（重点）
        与1-N的做法是完全一样的。

 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息和她的全部角色信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        User user = userMapper.getUserById(2);
        System.out.println(user);

        // d.关闭会话
        sqlSession.close();
    }
}
```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### Role

```java
public class Role {
    private int roleId ;
    private String roleName;
    private String roleDetail;

    public int getRoleId() {
        return roleId;
    }

    public void setRoleId(int roleId) {
        this.roleId = roleId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDetail() {
        return roleDetail;
    }

    public void setRoleDetail(String roleDetail) {
        this.roleDetail = roleDetail;
    }

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDetail='" + roleDetail + '\'' +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;
    // N-N: 虽然用户和角色是多对多的关系。但是一个用户对象对应一批角色对象在代码中相当于1-N
    private List<Role> roles;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                ", roles=" + roles +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### OrderMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
    @Select("select * , O_id oid , create_time createDate from tb_order where user_id = #{xxxx}")
    List<Order> selectOrdersByUserId(int userId);
}

```

#### RoleMapper

```java
public interface RoleMapper {
    @Select("SELECT * FROM user_role JOIN role ON user_role.role_id = role.`role_id` WHERE\n" +
            "user_role.`user_id` = #{xxx}")
    @Results({
            @Result(column = "role_id", property = "roleId"),
            @Result(column = "role_name", property = "roleName"),
            @Result(column = "role_detail", property = "roleDetail")
    })
    List<Role> selectRolesByUserId(int userId);

}

```

#### UserCardMapper

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
    @Select("select * from user_card where id = #{xxxx}")
    UserCard getUserCardById(int id);
}
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    @Select("select * from user where id = #{xxxx}")
    // 演示了 1-1   @Result(column = "id" , property = "userCard" ..
    // 演示了1-N    @Result(column = "id" , property = "orders"  ..
    // 演示了N-N    @Result(column = "id" , property = "roles"   ..
    // FetchType.LAZY:开启了该关联字段的延迟加载!!
    // FetchType.EAGER:开启了立即加载该字段的关联数据！无论是否需要这个数据！
    @Results({
            @Result(column = "id" , property = "id") ,
            @Result(column = "id" , property = "userCard"
               , one=@One(select = "com.itheima.dao.UserCardMapper.getUserCardById" , fetchType = FetchType.LAZY)),
            @Result(column = "id" , property = "orders"
                    , many=@Many(select = "com.itheima.dao.OrderMapper.selectOrdersByUserId" , fetchType = FetchType.LAZY)),
            @Result(column = "id" , property = "roles"
                    , many=@Many(select = "com.itheima.dao.RoleMapper.selectRolesByUserId" , fetchType = FetchType.LAZY)),
    })
    // 对象关系映射ORM, 实现分层的软件架构 ， “更加灵活”项目更加容易维护。
    User getUserById(int id);
}
```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
    <properties resource="db.properties"></properties>


    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

## cache

### action

#### TestDemo

```java
/**
    目标：一级/二级缓存的研究

    一级缓存是默认开启的:在一个会话中相同查询，只有第一次操作会查询数据库，然后缓存数据到内存，第二次操作直接进入到内存中找该数据！
     1. 什么是一级缓存：会话级别的缓存，只在同一个会话中起作用。
     2. 如何清除一级缓存：添加，删除，修改，提交，回滚，关闭都会自动清除一级缓存。
     3. 一级缓存是Mybaits默认开启的，是自带的，程序员无需理会。

     二级缓存的作用范围在整个会话工厂阶段有效，可以在不同的会话间共享。
      1.在核心配置文件中加入：<setting name="cacheEnabled" value="true"/>
        开启二级缓存。（二级缓存默认是关闭的）
      2.缓存的数据对象必须实现序列化接口：Serializable
      3. 给当前需要缓存的映射文件查询操作使用二级缓存： <cache/>
        注解怎么使用二级缓存：@CacheNamespace // 接口中的全部查询使用二级缓存！

 */
public class TestDemo {
    /**
     * 需求：查询2号用户的信息和她的全部角色信息。
     */
    @Test
    public void getUserAndUserCard(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // c.调用方法查询对象
        List<User> users = userMapper.selectAll();
        System.out.println(users);

        // 触发了DML操作：缓存失效。
        System.out.println(userMapper.deleteById(8));

        System.out.println("------------------");
        List<User> users1 = userMapper.selectAll();
        System.out.println(users1);

        System.out.println("------------------");
        User user = userMapper.selectUserById(2);
        System.out.println(user);
        // d.关闭会话
        sqlSession.close();
    }

    @Test
    public void getRoles(){
        // a.获取会话
        SqlSession sqlSession = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        RoleMapper roleMapper = sqlSession.getMapper(RoleMapper.class);
        // 查询全部角色数据已经进入到二级缓存中去了
        List<Role> roles = roleMapper.selectAllRoles();
        System.out.println(roles);
        sqlSession.close(); // 即使会话关闭了，二级缓存依然存在！

        System.out.println("-----------------------");
        // a.获取会话
        SqlSession sqlSession1 = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        RoleMapper roleMapper1 = sqlSession1.getMapper(RoleMapper.class);
        // 会直接进入到二级缓存中提取数据，不会查询数据库了！！
        List<Role> roles1 = roleMapper1.selectAllRoles();
        System.out.println(roles1);
        sqlSession1.close(); // 即使会话关闭了，二级缓存依然存在！

        System.out.println("-----------------------");

        // a.获取会话
        SqlSession sqlSession2 = MybatisSqlSessionFactoryUtils.getSqlSession();
        // b.获取数据访问层接口的代理对象
        RoleMapper roleMapper2 = sqlSession2.getMapper(RoleMapper.class);
        // 会直接进入到二级缓存中提取数据，不会查询数据库了！！
        Role roles2 = roleMapper2.selectRoleById(2);
        System.out.println(roles2);
        sqlSession2.close(); // 即使会话关闭了，二级缓存依然存在！

    }
}
```

### bean

#### Order

```java
/**
 o_id INT PRIMARY KEY AUTO_INCREMENT ,   -- 主键
 user_id INT NOT NULL,   -- 用户id，外键
 number VARCHAR(20),   -- 订单编号
 create_time DATETIME,  -- 下单时间
 note VARCHAR(100),   -- 备注
 */
public class Order {
    private int oid;
    private String number;
    private Date createDate;
    private String note;

    public int getOid() {
        return oid;
    }

    public void setOid(int oid) {
        this.oid = oid;
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    public String getNote() {
        return note;
    }

    public void setNote(String note) {
        this.note = note;
    }

    @Override
    public String toString() {
        return "Order{" +
                "oid=" + oid +
                ", name='" + number + '\'' +
                ", createDate=" + createDate +
                ", note='" + note + '\'' +
                '}';
    }
}
```

#### Role

```java
public class Role implements Serializable {
    private int roleId ;
    private String roleName;
    private String roleDetail;

    public int getRoleId() {
        return roleId;
    }

    public void setRoleId(int roleId) {
        this.roleId = roleId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    public String getRoleDetail() {
        return roleDetail;
    }

    public void setRoleDetail(String roleDetail) {
        this.roleDetail = roleDetail;
    }

    @Override
    public String toString() {
        return "Role{" +
                "roleId=" + roleId +
                ", roleName='" + roleName + '\'' +
                ", roleDetail='" + roleDetail + '\'' +
                '}';
    }
}
```

#### User

```java
/**
 *   id INT PRIMARY KEY AUTO_INCREMENT,
 *   username VARCHAR(20) NOT NULL,
 *   birthday DATE,
 *   sex CHAR(1) DEFAULT '男',
 *   address VARCHAR(50)
 */
public class User {
    private int id ;
    private String username ;
    private String birthday ;
    private char sex ;
    private String address;
    // 1-1: 定义一个身份证对象数据。查询用户对象的时候自然可以关联查询到此用户身份证对象
    private UserCard userCard ;
    // 1-N：定义一个订单信息的数据。一个用户对象包含多个订单。
    private List<Order> orders ;
    // N-N: 虽然用户和角色是多对多的关系。但是一个用户对象对应一批角色对象在代码中相当于1-N
    private List<Role> roles;

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public UserCard getUserCard() {
        return userCard;
    }

    public void setUserCard(UserCard userCard) {
        this.userCard = userCard;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex=" + sex +
                ", address='" + address + '\'' +
                ", userCard=" + userCard +
                ", orders=" + orders +
                ", roles=" + roles +
                '}';
    }
}
```

#### UserCard

```java
/**
 id INT PRIMARY KEY,   -- 既是主键又是外键
 height DOUBLE,  -- 身高厘米
 weight DOUBLE,   -- 体重公斤
 married TINYINT,    -- 是否结婚，1为结婚，0为未婚
 */
public class UserCard {
    private int id ;
    private double height;
    private double weight ;
    private int married;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    public int getMarried() {
        return married;
    }

    public void setMarried(int married) {
        this.married = married;
    }

    @Override
    public String toString() {
        return "UserCard{" +
                "id=" + id +
                ", height=" + height +
                ", weight=" + weight +
                ", married=" + married +
                '}';
    }
}
```

### dao

#### RoleMapper

```java
public interface RoleMapper {
    List<Role> selectAllRoles();

    Role selectRoleById(int id);
}

```

#### RoleMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="com.itheima.dao.RoleMapper">

    <!-- cache：代表当前映射文件中的全部查询操作全部使用二级缓存   -->
    <cache/>

    <select id="selectAllRoles" resultType="Role" >
           select role_id roleId , role_name roleName , role_detail roleDetail from role
    </select>

    <select id="selectRoleById" resultType="Role" >
           select role_id roleId , role_name roleName , role_detail roleDetail
           from role where role_id = #{xxzxz}
    </select>
</mapper>
```

#### UserMapper

```java
/**
 * 用户表数据访问层接口
 */
@CacheNamespace // 本接口中的全部查询使用二级缓存！
public interface UserMapper {
    @Select("select * from user")
    List<User> selectAll();

    @Select("select * from user where id = #{xxxx}")
    User selectUserById(int id);

    @Delete("delete from user where id = #{xxx}")
    boolean deleteById(int id);
}

```

### util

#### MybatisSqlSessionFactoryUtils

```java
/**
 * 加载Mybatis的核心配置文件：sqlMapConfig.xml
 * 得到一个Mybaits的连接工厂对象：
 *
 * 用于生产连接。
 * 释放连接。

 JDBC连接：Connection
 MyBatis连接：SqlSession

 Mybatis加载连接工厂或者连接的相关API
     1. SqlSessionFactoryBuilder：这是一个临时对象，用完就不需要了。
        通过这个工厂建造类来创建一个会话工厂。
     2. SqlSessionFactory：从一个工厂类中得到一个会话对象，一个项目中只需要创建一个会话工厂对
        象即可。通过会话工厂对象来创建会话对象。
     3. SqlSession： 每次访问数据库都需要创建一个会话对象，这个会话对象不能共享。访问完成以后
       会话需要关闭。
 */
public class MybatisSqlSessionFactoryUtils {
    // 1.定义一个静态成员变量存储Mybatis连接工厂:暂时还没有加载该连接工厂。
    public static SqlSessionFactory sqlSessionFactory;
    // 2.定义一个静态代码块:负责加载Mybatis的核心配置文件成为一个连接工厂对象。
    static {
        try {
            // a.读取核心配置文件成为一个字节输入流
            // Resources自动到类路径下寻找配置文件
            InputStream is = Resources.getResourceAsStream("sqlMapConfig.xml");
            // b.把核心配置文件加载成连接工厂对象
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 提供一个获取连接的方法出去。
     */
    public static SqlSession getSqlSession(){
        // 调用连接工厂开启一个新会话返回出去。
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }

    /**
     * 释放连接的方法
     */
    public static void close(SqlSession sqlSession){
        if(sqlSession!=null)sqlSession.close();
    }

//    public static void main(String[] args) {
//        SqlSession sqlSession = getSqlSession();
//        System.out.println(sqlSession);
//    }
}
```

### 配置文件

#### db

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day19
username=root
password=root
```

#### log4j

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout

### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout

```

#### sqlMapConfig

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载系统属性文件，默认直接去src类路径下寻找文件 -->
    <properties resource="db.properties"></properties>

    <settings>
        <!-- 开启二级缓存，二级缓存默认是关闭的！！-->
        <setting name="cacheEnabled" value="true"/>
    </settings>


    <typeAliases>
        <!--
            1.配置类型别名：type真实类型，tlias配置的别名名称
            建议类型的别名，默认用它的类名称。
        <typeAlias type="com.itheima.bean.User" alias="User"></typeAlias>
        <typeAlias type="com.itheima.bean.Book" alias="User"></typeAlias>-->

        <!--
            2.包扫描：不需要一行一行的配置别名
             默认会扫描当前包下的全部实体类，用类名作为它的别名
             以后映射文件直接用别名申明即可!
        -->
        <package name="com.itheima.bean"/>
    </typeAliases>

    <!--mybatis环境的配置-->
    <environments default="mysql">
        <!--通常我们只需要配置一个就可以了， id是环境的名字 -->
        <environment id="mysql">
            <!--事务管理器：由JDBC来管理.默认开启了DML的事物 -->
            <transactionManager type="JDBC"/>
            <!-- 整合连接池：数据源的配置：mybatis自带的连接池
                ${driver}:占位符，会去db.properties取对应的值。
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <!-- 1.关联系统当前的映射文件 :直接从src下寻找
        <mapper resource="com/itheima/dao/UserMapper.xml"></mapper>
        <mapper resource="com/itheima/dao/BookMapper.xml"></mapper>
        -->
        <!-- 2.关联映射文件的包扫描：默认会去当前包下扫描全部xml结尾的映射文件。
        -->
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

#### 生成数据sql

```sql
/*
SQLyog Ultimate v12.14 (64 bit)
MySQL - 5.5.40 : Database - day19
*********************************************************************
*/


/*!40101 SET NAMES utf8 */;

/*!40101 SET SQL_MODE=''*/;

/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
CREATE DATABASE /*!32312 IF NOT EXISTS*/`day19` /*!40100 DEFAULT CHARACTER SET utf8 */;

USE `day19`;

/*Table structure for table `role` */

DROP TABLE IF EXISTS `role`;

CREATE TABLE `role` (
  `role_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '角色id(主键)',
  `role_name` varchar(32) NOT NULL COMMENT '角色名称',
  `role_detail` varchar(100) DEFAULT NULL COMMENT '角色描述',
  PRIMARY KEY (`role_id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

/*Data for the table `role` */

insert  into `role`(`role_id`,`role_name`,`role_detail`) values 

(1,'校长','全校的管理者'),

(2,'讲师','传道授业解惑'),

(3,'班主任','班级的灵魂'),

(4,'助教','大家的朋友');

/*Table structure for table `tb_order` */

DROP TABLE IF EXISTS `tb_order`;

CREATE TABLE `tb_order` (
  `o_id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL,
  `number` varchar(20) DEFAULT NULL,
  `create_time` datetime DEFAULT NULL,
  `note` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`o_id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `tb_order_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

/*Data for the table `tb_order` */

insert  into `tb_order`(`o_id`,`user_id`,`number`,`create_time`,`note`) values 

(1,1,'10001001','2019-10-17 10:36:13','请提前安装'),

(2,1,'10001002','2019-10-17 10:36:13','包邮'),

(3,1,'10001003','2019-10-17 10:36:13','选择红色'),

(4,2,'10001004','2019-10-17 10:36:13','购买多件'),

(5,2,'10001005','2019-10-17 10:36:13','安全带');

/*Table structure for table `user` */

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL,
  `birthday` date DEFAULT NULL,
  `sex` char(1) DEFAULT '男',
  `address` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

/*Data for the table `user` */

insert  into `user`(`id`,`username`,`birthday`,`sex`,`address`) values 

(1,'孙悟空','1980-10-24','男','花果山水帘洞'),

(2,'白骨精','1992-11-12','女','白虎岭白骨洞'),

(3,'猪八戒','1983-05-20','男','福临山云栈洞'),

(4,'蜘蛛精','1995-03-22','女','盤丝洞'),

(6,'牛魔王','1997-10-23','魔','火焰山'),

(7,'牛魔王','1997-10-23','魔','火焰山'),

(8,'牛魔王','1997-10-23','魔','火焰山');

/*Table structure for table `user_card` */

DROP TABLE IF EXISTS `user_card`;

CREATE TABLE `user_card` (
  `id` int(11) NOT NULL,
  `height` double DEFAULT NULL,
  `weight` double DEFAULT NULL,
  `married` tinyint(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  CONSTRAINT `user_card_ibfk_1` FOREIGN KEY (`id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

/*Data for the table `user_card` */

insert  into `user_card`(`id`,`height`,`weight`,`married`) values 

(1,185,90,1),

(2,170,60,0);

/*Table structure for table `user_role` */

DROP TABLE IF EXISTS `user_role`;

CREATE TABLE `user_role` (
  `user_id` int(11) NOT NULL COMMENT '用户id',
  `role_id` int(11) NOT NULL COMMENT '角色id',
  PRIMARY KEY (`user_id`,`role_id`),
  KEY `role_id` (`role_id`),
  CONSTRAINT `user_role_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`),
  CONSTRAINT `user_role_ibfk_2` FOREIGN KEY (`role_id`) REFERENCES `role` (`role_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

/*Data for the table `user_role` */

insert  into `user_role`(`user_id`,`role_id`) values 

(1,1),

(2,1),

(2,2),

(1,3),

(2,4);

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

```
