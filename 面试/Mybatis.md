# 自定义Mybatis框架

## 步骤

```tex
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
```

## 代码结构

![Snipaste_2021-10-09_13-51-26](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/Snipaste_2021-10-09_13-51-26.png)

```java
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

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private String sex ;
    private String address;
    //private Date creataDate;
```

```java
public interface UserMapper {
    List<User> selectAllUsers();
}
```

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
```

```java
public class Mapper {
    private String tag; // select delete
    private String id ;
    private String resultType;
    private String sql ;
    private String namespace;
```

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

# 案例

## 步骤

```tex
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
```

## 代码结构

![Snipaste_2021-10-09_13-57-52](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/spring一/Snipaste_2021-10-09_13-57-52.png)

```java
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

```java
public class User {
    private int id ;
    private String userName ;
    private Date birthday ;
    private char sex ;
    private String address;
    private Date creataDate;
```

```java
public interface UserMapper {
    List<User> selectAllUsers();
    int deleteUserById(int id);
}
```

```java
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

```properties
### 设置Logger输出级别和输出目的地 ###
log4j.rootLogger=debug, stdout
### 把日志信息输出到控制台 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

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

# queryParam

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

```xml
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

# configImprove

```xml
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

# queryLike

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

```xml
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

# dml

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

```xml
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

# paramImprove

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    List<User> selectUserByAddrAndName(User user);
}
```

```xml
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

# result

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    int countUsers();
    List<User> selectAll();
}
```

```xml
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

# dynamicSQL

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

```xml
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

# foreach

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    int batchInsertUser(List<User> users);
    int batchDeleteUsers(String[] ids);
}
```

```xml
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

# sqlInclude

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

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<!-- user对象：
#{userName} = user.getUserName();
#{address.name} = user.getAddress().getName();
-->
<mapper namespace="dao.UserMapper">
    <!-- 手工配置结果集类型映射 :主要是配置不能自动映射的字段值-->
    <resultMap id="userResultMap" type="User">
        <result column="create_date" property="createDate"></result>
    </resultMap>
    <sql id="dynamicWhereSQL">
        <if test="userName !=null and userName!='' ">
            and username like #{userName}
        </if>
        <if test="sex!=''">
            and sex = #{sex}
        </if>
        <if test="address !=null and address!='' ">
            and address like #{address}
        </if>
    </sql>
    <sql id="selectUserSQL">
        select * from user
    </sql>
    <!-- 这个查询的结果集映射，采用自定义的结果映射配置：userResultMap
            user: (userName , sex , address)
        -->
    <select id="selectUserDynicmic" resultMap="userResultMap">
        <include refid="selectUserSQL"></include> where 1=1
        <!-- 导入上面定义的sql代码 -->
        <include refid="dynamicWhereSQL"></include>
    </select>
    <select id="selectUserDynicmic02" resultMap="userResultMap">
        <include refid="selectUserSQL"></include>
        <!--
            where: 相当于Where关键字，可以去掉多余的or and where
            where加上去，后面的动态sql可以都直接加and,无需担心语法错误！
                第一个条件前面的and可写可不写。
            -->
        <where>
               <include refid="dynamicWhereSQL"></include>
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

# 1-1

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

```xml
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

# 1-1-improve

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
    // 根据id查询出身份证对象
    UserCard getUserCardById(int id);
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
    <select id="getUserCardById" resultType="UserCard">
        select * from user_card where id = #{xxx}
    </select>
</mapper>
```

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

```xml
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

# 1-N

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.OrderMapper">
</mapper>
```

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

```xml
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

# 1-N-improve

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
    List<Order> getOrdersByUserId(int id);
}
```

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

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

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

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}

```

```xml
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

# N-N

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.OrderMapper">
</mapper>
```

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
}
```

```xml
<!--
    namespace命名空间:必须与服务的接口的路径一致。
-->
<mapper namespace="com.itheima.dao.UserCardMapper">
</mapper>
```

```java
/**
 * 用户表数据访问层接口
 */
public interface UserMapper {
    User getUserById(int id);
}
```

```xml
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

# annotation

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

# 1-N-annotation

```java
// 专门处理身份证表的CRUD操作的接口
public interface OrderMapper {
    @Select("select * , O_id oid , create_time createDate from tb_order where user_id = #{xxxx}")
    List<Order> selectOrdersByUserId(int userId);
}
```

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

```java
// 专门处理身份证表的CRUD操作的接口
public interface UserCardMapper {
    @Select("select * from user_card where id = #{xxxx}")
    UserCard getUserCardById(int id);
}
```

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

# cache

```tex
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
```

```java
public interface RoleMapper {
    List<Role> selectAllRoles();
    Role selectRoleById(int id);
}
```

```xml
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

```xml
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

