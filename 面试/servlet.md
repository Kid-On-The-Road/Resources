# Tomcat项目发布方式

## 方式一

复制到webapps

### 操作方法

1. 直接将项目复制到webapps目录下

2. 采用压缩文件.war的方式
   1. 将整个项目使用压缩工具打包成一个zip文件
   2. 改zip的扩展名为war
   3. 复制到webapps目录下，tomcat会自动解压成一个同名的目录。

![1571974586371](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571974586371.png) 

## 方式二

虚拟目录

### 配置方式

server.xml中host元素下

- 优点：不需要将项目复制到webapps下
- 缺点：要修改server.xml，重启服务器，如果配置出错，导致tomcat重启失败

### 代码

```xml
		<!-- 
       在host下，配置一个Context
		属性：
			path 访问的虚拟地址
			docBase 在服务器上真实地址
		修改了server.xml 需要重启服务器	
		-->
		<Context path="/aaa" docBase="e:/heima" />	   
```

### 浏览器上测试

![1571975166843](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571975166843.png) 



## 方式三

独立的XML文件

1. 在tomcat/conf/catalina/localhost中创建xml配置文件
2. 名称假设为：second.xml，这个名称就是项目的访问路径

3. 添加xml文件的内容为

```xml
<Context  docBase="项目所在的目录" />
```

### 浏览器上测试

![1571975342826](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571975342826.png)

# Servlet

本质上就是一个Java类，运行在服务器上小程序，由tomcat来调用。接收用的请求并且处理请求，做出响应。

![1571985966701](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571985966701.png)

## Servlet与普通的Java程序的区别

1. Servlet本质上就是一个Java类，没有main函数，由Tomcat去调用它来执行。
2. 所有的Servlet都必须实现javax.servlet.Servlet接口 
3. 运行在Tomcat中，Web容器。
4. 对浏览器发送的请求做出响应，并且在浏览器端生成动态的网页。

```java
/**
 * 1. 创建一个类，继承于HttpServlet抽象类，这个类已经实现了Servlet接口。
 * 2. 重写抽象类中doGet和doPost方法，分别用来处理浏览器发送的get或post请求。
 * 3. 需要在web.xml中配置Servlet访问地址和类全名。
 */
public class Demo1Servlet extends HttpServlet {
    /**
     * 处理get请求
     * @param request 请求对象
     * @param response 响应对象
     */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置动态网页的响应类型
        response.setContentType("text/html;charset=utf-8");
        //2. 得到打印流，向浏览器端输出文字
        PrintWriter out = response.getWriter();
        out.print("<h1 style='color: red;'>Hello Servlet!</h1>");
    }
}
```

## Servlet开发方式

### Servlet2.5

```xml
 <servlet>
        <!-- servlet的名字-->
        <servlet-name>demo1</servlet-name>
        <!-- 类全名-->
        <servlet-class>servlet.Demo1Servlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <!-- 与上面的名字对应-->
        <servlet-name>demo1</servlet-name>
        <!-- 访问地址,必须以/开头,/相当于web目录-->
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
```

### Servlet3.0

| @WebServlet注解的属性 | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| name                  | Servlet的名字，相当于配置文件中servlet-name(可以省略) |
| urlPatterns           | 访问地址，相当于配置文件中url-pattern                 |
| value                 | 默认属性，指定访问地址。                              |

```java
/**
 * 1. 编写一个类继承于HttpServlet
 * 2. 重写doGet或doPost方法
 * 3. 使用@WebServlet注解设置访问地址，必须以/开头
 */
@WebServlet("/demo2")   //Invalid <url-pattern> demo2 in servlet mapping
public class Demo2Servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置响应的类型
        response.setContentType("text/html;charset=utf-8");
        //2. 得到打印流，输出
        PrintWriter out = response.getWriter();
        out.print("你好，我是Servlet3.0");
    }
}
```

## Servlet的生命周期

- Servlet接口中的方法

| 方法                                                   | 作用         | 运行次数                                |
| ------------------------------------------------------ | ------------ | --------------------------------------- |
| void init(ServletConfig   config)                      | 初始化的方法 | 第1次有用户访问这个Servlet的时候执行1次 |
| void  service(ServletRequest req, ServletResponse res) | 服务的方法   | 每次用户发送请求都会执行这个方法        |
| void  destroy()                                        | 销毁的方法   | 在服务器关闭的时候执行1次               |

注：一个Servlet在tomcat容器中只会实例化一次，只会产生一个对象，而且常驻内存。要等到服务器关闭才会销毁。

![1571988229503](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571988229503.png)

```java
/**
 * 1. 创建一个类实现Servlet接口
 * 2. 重写接口中所有的方法，其中service方法用于处理用户的请求
 * 3. 使用@WebServlet配置访问地址
 */
@WebServlet("/demo3")
public class Demo3LifeCycleServlet implements Servlet {
    //1. 初始化：执行1次
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println(new Timestamp(System.currentTimeMillis()) + ", 初始化方法，第1次访问执行1次");
    }
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }
    //2. 服务：每次请求都会执行
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println(new Timestamp(System.currentTimeMillis()) + "，有用户发送了请求");
    }
    @Override
    public String getServletInfo() {
        return null;
    }
    //3. 销毁的方法：执行1次
    @Override
    public void destroy() {
        System.out.println(new Timestamp(System.currentTimeMillis()) + "，Servlet销毁");
    }
}
```

==注意==:service方法中输出request和response对象,由tomcat实现，并且由tomcat实例化这2个对象。

## Servlet映射多个路径

### 配置方式

- 一个\<servlet-mapping>中写多个\<url-pattern>

```xml
   <servlet-mapping>
        <!--与上面的名字对应-->
        <servlet-name>demo1</servlet-name>
        <!--访问地址，必须以/开头, /相当于web目录 -->
        <url-pattern>/demo1</url-pattern>
        <url-pattern>/demo1000</url-pattern>
    </servlet-mapping>
```

- 一个\<servlet>对应多个\<servlet-mapping>

```xml
<servlet>
    <!--servlet的名字-->
    <servlet-name>demo1</servlet-name>
    <!--类全名-->
    <servlet-class>com.itheima.servlet.Demo1Servlet</servlet-class>
</servlet>
<servlet-mapping>
    <!--与上面的名字对应-->
    <servlet-name>demo1</servlet-name>
    <!--访问地址，必须以/开头, /相当于web目录 -->
    <url-pattern>/demo1</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <!--与上面的名字对应-->
    <servlet-name>demo1</servlet-name>
    <!--访问地址，必须以/开头, /相当于web目录 -->
    <url-pattern>/demo1000</url-pattern>
</servlet-mapping>
```

### 注解方式

```java
@WebServlet(name = "demo2", urlPatterns = {"/demo2", "/demo2000"} )
```

## url-pattern的通配符

​     一个配置，可以通配多个访问地址

| 通配符格式 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| /开头      | /* 匹配所有的资源，不建议这么写，会导致所有的资源都不能访问。 |
|            | /目录/* 访问这个目录所有的资源都会访问这个Servlet            |
| *.扩展名   | 匹配某个扩展名结尾的访问地址                                 |

==注意==:以/开头的格式，以扩展名结尾的格式，不能同时出现

| 请求URL       | Servlet1 | Servlet2 | 访问哪个 |
| ------------- | -------- | -------- | -------- |
| /abc/a.html   | /abc/*   | /*       | Servlet1 |
| /abc          | /abc/*   | /abc     | Servlet2 |
| /abc/a.do     | /abc/*   | *.do     | Servlet1 |
| /a.do         | /*       | *.do     | Servlet1 |
| /xxx/yyy/a.do | /*       | *.do     | Servlet1 |

- 优先级：以/开头的高于以扩展名结尾
- 匹配原则：哪个更接近就匹配哪个，最佳匹配原则。

## Servlet的执行流程

![1552394697006](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552394697006.png)

1. 通过浏览器发送请求http://服务器:端口号/项目地址/Servlet地址
2. 项目地址找到tomcat中指定的项目
3. 通过Servlet地址找到项目中相应的Servlet
4. 通过反映对这个Servlet进行实例化的操作
5. tomcat创建request和response对象
6. 调用service()方法传入请求和响应对象
7. 由service()方法调用doGet或doPost方法
8. 处理完毕以后返回到浏览器端

## ServletConfig接口

如果Servlet中有一些配置的参数，直接写死在源代码中，以后修改就不灵活。可以将一些配置参数放在web.xml文件中。通过ServletConfig接口提供的方法来读取这些配置参数。

| ServletConfig接口中的方法                    | 功能                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| String getInitParameter("参数名")            | 通过参数名来读取相应的参数值<br />参数名和参数值都在web.xml配置文件中 |
| Enumeration\<String> getInitParameterNames() | 获取所有配置参数的名字，返回枚举接口                         |

| Enumeration接口的方法     | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
| boolean hasMoreElements() | 测试此枚举是否包含更多的元素。                               |
| E nextElement()           | 如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。 |

```xml
   <servlet>
        <servlet-name>demo5</servlet-name>
        <servlet-class>com.itheima.servlet.Demo5ConfigServlet</servlet-class>
        <!--指定配置参数-->
        <init-param>
            <param-name>user</param-name>
            <param-value>NewBoy</param-value>
        </init-param>
        <init-param>
            <param-name>age</param-name>
            <param-value>18</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo5</servlet-name>
        <url-pattern>/demo5</url-pattern>
    </servlet-mapping>
```

```java
/**
 * ServletConfig接口中方法的使用
 */
public class Demo5ConfigServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //因为子类已经继承了ServletConfig中所有的方法
        String user = getInitParameter("user");  //得到1个参数
        System.out.println("用户名：" + user);
        //得到所有的参数名
        Enumeration<String> names = getInitParameterNames();
        //遍历枚举类型
        while(names.hasMoreElements()) {
            String name = names.nextElement();  //得到一个名字
            String value = getInitParameter(name);
            System.out.println("参数名：" + name + "，参数值：" + value);
        }
    }
}
```

## load-on-startup参数

用户第1次访问这个Servlet实例化对象

```xml
配置文件：
<load-on-startup>1</load-on-startup>
必须设置为整数，数字越小越先加载。最小是0，如果设置为负数无效
```

```java
使用注解：
@WebServlet(urlPatterns = "/demo2", loadOnStartup = 0)
```

案例

```xml
    <servlet>
        <!--servlet的名字-->
        <servlet-name>demo1</servlet-name>
        <!--类全名-->
        <servlet-class>com.itheima.servlet.Demo1Servlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

```java
/**
 * 1. 创建一个类，继承于HttpServlet抽象类，这个类已经实现了Servlet接口。
 * 2. 重写抽象类中doGet和doPost方法，分别用来处理浏览器发送的get或post请求。
 * 3. 需要在web.xml中配置Servlet访问地址和类全名。
 */
public class Demo1Servlet extends HttpServlet {
    @Override
    public void init() throws ServletException {
        System.out.println("Demo1被实例化了");
    }
    /**
     * 处理get请求
     * @param request 请求对象
     * @param response 响应对象
     */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置动态网页的响应类型
        response.setContentType("text/html;charset=utf-8");
        //2. 得到打印流，向浏览器端输出文字
        PrintWriter out = response.getWriter();
        out.print("<h1 style='color: red;'>Hello Servlet!</h1>");
    }
}
```

