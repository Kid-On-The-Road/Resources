# 《request请求对象-笔记》

# 回顾

## 软件架构分成哪两种，分别是什么含义

BS：Browser Server 浏览器服务器模式

CS：Client Server 客户端服务器模式

##  Tomcat目录结构

| 目录名  | 作用                                       |
| ------- | ------------------------------------------ |
| bin     | tomcat可执行文件，启动和关闭都是在这个目录 |
| conf    | 配置文件                                   |
| lib     | jar包                                      |
| logs    | 日志记录                                   |
| temp    | 临时                                       |
| webapps | 项目发布目录<br />ROOT目录：默认管理页面   |
| work    | 项目执行过程中工作目录                     |

 

## Servlet与普通的Java程序的区别

1. 作用：生成动态网页
2. 实现什么接口：Servlet
3. 运行在tomcat，web容器
4. service中的方法参数是：请求和响应



## 说出这些注解的作用

| @WebServlet注解 | 作用     |
| --------------- | -------- |
| name            | 名字     |
| urlPatterns     | 访问地址 |
| value           | 访问地址 |

 

## Servlet的生命周期方法 

| 方法                                                    | 作用       | 运行次数                  |
| ------------------------------------------------------- | ---------- | ------------------------- |
| void   init(ServletConfig config)                       | 初始化方法 | 用户第1次访问，执行1次    |
| void   service(ServletRequest req, ServletResponse res) | 服务方法   | 每次请求都执行            |
| void   destroy()                                        | 销毁       | 服务器关闭的时候，执行1次 |



# 学习目标

1. HTTP协议格式
   1. 能够使用工具查看HTTP协议内容 
   2. 能够理解HTTP协议请求内容
2. request对象方法
   1. 能够使用Request对象获取HTTP协议请求内容
   2. <font color="red">能够处理HTTP请求参数的乱码问题</font> 
   3. <font color="red">能够使用Request域对象</font> 
   4. <font color="red">能够使用Request对象做请求转发</font>
3. 能够完成登录案例



# 学习内容

## HTTP协议的概念

### 目标

1. 什么是HTTP
2. 它有什么特点

### 概念

在访问网页的时候最前面出现的就是传输数据使用的协议

![1555154341398](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1555154341398.png) 

HTML: Hyper Text Markup Language 超文本标记语言

HTTP: Hyper Text Transfer Protocol 超文本传输协议，用来传输网页的。

默认端口号：80，如果浏览器使用的是80端口，这个端口号可以省略



### HTTP协议的特点：

1. 它是一种应用层的协议，基于TCP协议(传输层)之上，用于传输数据的格式。
2. 浏览器与服务器之间不需要长期创建连接，浏览器发送请求给服务器，服务器做出响应之后连接就有可能断开了。比较节省服务器的资源。
3. 一个用户多次发送请求给服务器，或多个用户发送一次请求给服务器，服务器通过HTTP协议并不能知道是不是同一个用户，不会保留用户的访问状态。
4. 因为不创建连接，也不保留用户的状态，所以它的传输速度会比较快。

我们主要学习它的格式：

HTTP协议分成：HTTP请求格式，HTTP响应的格式



### HTTPS

HTTPS：Hyper Text Transfer Protocol SecureSocket Layer 在HTTP协议的基础上加密的一种协议，数据的传输过程中会进行加密和解密。可以防止客户端或服务器端伪造。

不足：相对HTTP速度会慢一些

端口号是：443

![1552550764411](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552550764411.png)

### 

### 小结

1. 什么是HTTP? 

   ```
   超文本传输协议，80端口
   ```

   

2. 它由哪两个组成部分?

   ```
   HTTP请求，HTTP响应
   ```

   



## HTTP请求的三个组成

### 目标

请求有哪三个组成部分

### 什么是请求

英文：request 由浏览器发送给服务器的数据

### 查看HTTP请求

1. 在HTML页面上，制作2个表单提交页面，用户名和密码，get提交和post提交按钮，查看HTTP请求

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>登录</title>
   </head>
   <body>
   <h2>GET方式</h2>
   <form action="demo1" method="get">
       用户名：<input type="text" name="username"><br>
       密码：<input type="password" name="password"><br>
       <input type="submit" value="登录">
   </form>
   
   <hr/>
   <h2>POST方式</h2>
   <form action="demo1" method="post">
       用户名：<input type="text" name="username"><br>
       密码：<input type="password" name="password"><br>
       <input type="submit" value="登录">
   </form>
   </body>
   </html>
   ```

2. 查看浏览器与服务器的通讯，按F12打开窗口

  ![1572054006365](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572054006365.png) 

​    

### 请求的组成

![1572054184735](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572054184735.png) 



### 小结

请求由哪三个组成部分？分别是？

```
1. 请求行
2. 请求头
3. 请求体
```



## 请求信息的组成：请求行

### 目标

1. 请求行的格式
2. POST和GET请求的区别

### 请求行组成

```
POST /day25_01_request_war_exploded/demo1 HTTP/1.1
```

三个部分：请求方式  URI 协议和版本

1.0：每请求一个资源断开连接

1.1：每次请求可以获取多个资源，比较多使用1.1



### POST与GET的区别

|        | POST方式                                         | GET方式                                                      |
| ------ | ------------------------------------------------ | ------------------------------------------------------------ |
| 地址栏 | 参数不会显示在地址栏<br />数据在请求体中发送的。 | 请求的参数会在地址栏上显示<br />数据在请求行中发送           |
| 大小   | 理论上没有限制(上传文件)                         | 最多不能超过1K                                               |
| 安全性 | 安全性相对高一些                                 | 相对低一些                                                   |
| 缓存   | 把浏览器的数据发送到服务器上，不会使用缓存       | 从服务器获取数据，如果服务器的网页没有更新，<br />不再从服务器上再下载网页，而是使用本地的缓存，<br />提升访问速度。<br />缓存状态码是304，没有使用缓存状态码是200 |

### 小结

1. 请求行由哪三个组成部分？

   ```
   提交方式 URI 协议和版本
   ```

   

2. GET方法和POST方法传递数据有什么区别？

   ```
   1. 哪个会使用缓存？GET
   2. 哪个数据没有限制？ post
   3. 哪个更安全？ POST
   ```

   



## 请求信息的组成：请求头、请求体

### 目标

了解常见请求头的作用

### 常用请求头

| 请求头            | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| referer           | 得到上一个请求的页面地址<br />![1572055676064](assets/1572055676064.png) |
| if-modified-since | 如果使用了缓存，缓存的时间。因为东八区，时间差了8个小时<br />![1572055737434](assets/1572055737434.png) |
| user-agent        | 浏览器的类型<br />![1572055782064](assets/1572055782064.png) |
| connection        | TCP连接状态<br />![1572055907970](assets/1572055907970.png) 处于激活状态，close关闭状态 |
| host              | 访问主机名字和端口号<br />![1572055943633](assets/1572055943633.png) |

### 请求体

1. GET：没有请求体的，数据在请求行中发送。
2. POST：才有请求体，数据在请求体中发送。

### 小结

![1552551974236](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552551974236.png)



## 请求的方法：与请求行有关的方法[了解]

### 目标

与请求行相关的方法

### 什么是HttpServletRequest对象

概述：这是一个接口，它是由Tomcat实现这个接口，并且在执行的时候，由Tomcat创建。

![1572056263480](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572056263480.png) 

```
public class RequestFacade implements HttpServletRequest 
```



### 案例

#### 需求

创建一个RequestLineServlet，用于获取请求行中相关信息的方法，并且输出到网页上。

#### 效果

![1572056988049](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572056988049.png) 

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo1")
public class Demo1Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.设置响应类型
        response.setContentType("text/html;charset=utf-8");
        //2. 得到打印流
        PrintWriter out = response.getWriter();
        out.print("请求对象：" + request + "<hr/>");

        out.print("获取请求的方式：" + request.getMethod() + "<br/>");
        //URI: Uniform Resource Identifier 统一资源标识符，网络上资源的唯一标识，并不能直接访问
        out.print("得到请求URI:"+ request.getRequestURI() + "<br/>");
        //URL: Uniform Resource Locator 统一资源定位符，可以访问的网络地址
        out.print("得到请求URL:"+ request.getRequestURL() + "<br/>");
        out.print("得到协议和版本：" + request.getProtocol() + "<br/>");
        out.print("得到查询字符串(得到?后面的参数)：" + request.getQueryString() + "<br/>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| HttpServletRequest对象的方法  | 功能描述       |
| ----------------------------- | -------------- |
| String getMethod()            | 得到请求的方式 |
| String   getRequestURI()      | 得到URI        |
| StringBuffer  getRequestURL() | 得到URL        |
| String   getProtocol()        | 得到协议和版本 |
| String getQueryString()       | 得到查询字符串 |



## 请求的方法：与请求头有关的方法[了解]

### 目标

request对象中与请求头相关的方法

### 请求头相关方法

格式：多个键和值

| 请求方法                                | 功能描述         |
| --------------------------------------- | ---------------- |
| String   getHeader(String headName)     | 通过键得到一个值 |
| Enumeration\<String>   getHeaderNames() | 得到所有的键     |

| Enumeration接口中方法       | 说明                                   |
| --------------------------- | -------------------------------------- |
| boolean   hasMoreElements() | 判断是否还有下一个元素，如果有返回True |
| E   nextElement()           | 获取当前这个元素，向下移动一个元素     |

### 方法示例

#### 需求

1. 得到user-agent请求头的值
2. 得到所有的请求头信息，并输出所有的请求值信息

#### 效果

![1572057873048](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572057873048.png) 

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

@WebServlet("/demo2")
public class Demo2Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        //得到user-agent请求头的值，不区分大小写
        String userAgent = request.getHeader("user-agent");
        out.print("浏览器的类型：" + userAgent + "<br/>");
        //得到所有请求头的名字
        Enumeration<String> headerNames = request.getHeaderNames();
        while(headerNames.hasMoreElements()) {
            String head = headerNames.nextElement(); //得到请求头
            String value = request.getHeader(head);  //得到请求头的值
            out.print("请求头名字：" + head + "，值：" + value + "<hr/>");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 小结

| 请求方法                                | 功能描述             |
| --------------------------------------- | -------------------- |
| String getHeader(String  headName)      | 得到一个请求头的值   |
| Enumeration\<String>   getHeaderNames() | 得到所有请求头的名字 |



## 请求头应用案例：判断浏览器的类型

### 目标

判断客户端是什么类型的浏览器：Edg、OPR、Chrome、Safari、Firefox、IE浏览器或其它

### 效果

![1552556601532](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552556601532.png)

### 分析

1. 得到user-agent请求头，请求头的名字不区分大小写。
2. 判断它的值是否包含指定了字符串，如果包含则表示是指定的浏览器。
3. 否则就输出其它的浏览器

### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo3")
public class Demo3UserAgentServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        //得到user-agent请求头的值，返回字符串
        String userAgent = request.getHeader("user-agent");
        System.out.println(userAgent);
        out.print("您的浏览器类型是：");
        //判断字符串是否包含指定的浏览器字符串：contains
        //Edg、OPR、Chrome、Safari、Firefox、IE浏览器或其它
        if (userAgent.contains("Edg")) {
            out.print("Edge");
        }
        else  if (userAgent.contains("OPR")) {
            out.print("Opera");
        }
        else  if (userAgent.contains("Chrome")) {
            out.print("Chrome");
        }
        else  if (userAgent.contains("Safari")) {
            out.print("Safari");
        }
        else  if (userAgent.contains("Firefox")) {
            out.print("Firefox");
        }
        else  {
            out.print("IE浏览器或其它");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

案例的实现思路

1. 得到user-agent的请求头值
2. 判断字符串是否包含了指定浏览器的字符串
3. 从而判断是哪种浏览器的类型



## <font color="red">请求的方法：得到客户端提交的参数值</font>

### 目标

使用request中的方法得到表单或地址栏上提交的参数值

### 请求对象的方法

| 方法名                                     | 描述                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| String getParameter(String name)           | 通过参数名得到参数值                                         |
| String[] getParameterValues(String name)   | 通过参数名得到一组参数值，通常用于复选框                     |
| Enumeration\<String>   getParameterNames() | 得到所有的参数名，返回枚举类型                               |
| Map<String,String[]>   getParameterMap()   | 得到所有的参数名和值，封装成Map对象<br />其中键就是参数名，值就是参数值 |

### 获取参数的案例

#### 需求

1. 通过上面的方法得到注册页面的用户名和性别，输出到浏览器
2. 得到所有的爱好，输出到浏览器
3. 得到所有表单的名字和值，输出到浏览器
4. 得到封装好的表单数据Map对象，输出到浏览器

#### 效果

![1572060235529](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572060235529.png) 

#### 代码

##### HTML

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>这是登录页面</title>
</head>
<body>
<h2>用户注册</h2>
<form action="demo4" method="get">
    用户名： <input type="text" name="name"><br/>
    性别: <input type="radio" name="gender" value="男" checked="checked"/>男
    <input type="radio" name="gender" value="女"/>女 <br/>
    城市：
    <select name="city">
        <option value="广州">广州</option>
        <option value="深圳">深圳</option>
        <option value="珠海">珠海</option>
    </select>
    <br/>
    爱好：
    <input type="checkbox" name="hobby" value="上网"/>上网
    <input type="checkbox" name="hobby" value="上学"/>上学
    <input type="checkbox" name="hobby" value="上车"/>上车
    <input type="checkbox" name="hobby" value="上吊"/>上吊
    <br/>
    <input type="submit" value="注册"/>
</form>
</body>
</html>
```

##### Servlet代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Arrays;
import java.util.Enumeration;
import java.util.Map;

@WebServlet("/demo4")
public class Demo4Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //用户名和性别，输出到浏览器
        String name = request.getParameter("name");
        out.print("用户名：" + name + "<br/>");

        String gender = request.getParameter("gender");
        out.print("性别：" + gender + "<hr/>");

        //得到所有的爱好，输出到浏览器
        String[] hobbies = request.getParameterValues("hobby");
        out.print("爱好是：" + Arrays.toString(hobbies) + "<hr/>");

        //得到所有表单的名字和值，输出到浏览器
        Enumeration<String> parameterNames = request.getParameterNames();
        while(parameterNames.hasMoreElements()) {
            String parameterName = parameterNames.nextElement();  //得到一个参数名字
            String value = request.getParameter(parameterName);   //得到一个值
            out.print("参数名：" + parameterName + "，参数值：" + value + "<br/>");
        }

        out.print("<hr/>");

        //得到封装好的表单数据Map对象
        Map<String, String[]> map = request.getParameterMap();
        map.forEach((k,v) -> out.print("键：" + k + "，值：" + Arrays.toString(v) + "<br/>"));
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| 方法名                                   | 描述                                |
| ---------------------------------------- | ----------------------------------- |
| String getParameter(String name)         | 通过参数名得到参数值                |
| String[] getParameterValues(String name) | 通过参数名得到一组参数值            |
| Map<String,String[]> getParameterMap()   | 得到所有的参数名和值，封装成Map对象 |



## <font color="red">BeanUtils工具类的使用</font>

### 目标

BeanUtils工具类中方法的使用

### 什么是BeanUtils

BeanUtils是Apache Commons组件的成员之一，主要用于简化JavaBean封装数据的操作。

![1552556927816](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552556927816.png)

BeanUtils是第三方组织写的工具类，要使用它，得先下载对应的工具jar包。

下载地址：http://commons.apache.org/

### 相关的jar包

![1552557572028](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552557572028.png)

### BeanUtils常用方法

| BeanUtils方法                                          | 说明                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| public static void populate(Object target, Map source) | 将一个Map中键和值复制到一个实体类(POJO)<br />只会复制相同的名字，并且会自动转换类型 |

![1572061125733](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572061125733.png) 



### 案例：BeanUtils的使用

#### 需求

1. 得到表单所有的数据封装成Map
2. 使用BeanUtils封装成一个User对象，并且输出到浏览器。

#### 步骤

jar包必须放在web/WEB-INF/lib这个目录下

![1552557742580](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552557742580.png) 

#### 效果

![1572061441060](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572061441060.png) 

#### 代码

##### JavaBean

```java
package com.itheima.entity;

import java.util.Arrays;

public class User {

    private String name;
    private String gender;
    private String city;
    private String hobby[];

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", gender='" + gender + '\'' +
                ", city='" + city + '\'' +
                ", hobby=" + Arrays.toString(hobby) +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String[] getHobby() {
        return hobby;
    }

    public void setHobby(String[] hobby) {
        this.hobby = hobby;
    }
}
```

##### Servlet

```java
package com.itheima.servlet;

import com.itheima.entity.User;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/demo5")
public class Demo5Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //1.得到表单所有的参数，封装成Map对象
        Map<String, String[]> map = request.getParameterMap();
        //2.使用BeanUtils的工具类中方法将Map封装成User对象
        User user = new User();
        try {
            BeanUtils.populate(user, map);
        } catch (Exception e) {
            e.printStackTrace();
        }
        out.print("用户对象：" + user);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| BeanUtils常用方法                                       | 说明                                          |
| ------------------------------------------------------- | --------------------------------------------- |
| public static void populate(Object target, Map  source) | 将一个Map中所有的键和值复制到一个实体类对象中 |



## <font color="red">请求参数值汉字乱码的问题</font>

### 目标

解决POST方法提交乱码的问题

### 请求参数值产生乱码的原因

​	Tomcat中请求默认的编码：iso-8859-1，这是欧洲码表，不支持汉字。我们需要将请求的编码改成utf-8，才会支持汉字。

![1552557912477](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552557912477.png)

### POST方法乱码解决方法

1. 方法：request.setCharacterEncoding("字符集")
2. 上面代码出现的位置：必须在所有得到参数的方法之前
3. HTML页面编码：必须与网页的编码一致

### Tomcat8.0中GET方法乱码的问题

```
没有乱码，tomcat7.0中有乱码
```



### 小结

1. POST乱码如何解决

   ```
   setCharacterEncoding("编码")
   ```

   

2. GET乱码如何解决

   ```
   在8.0以上没有乱码的问题
   ```



## 上午回顾

### HTTP的协议组成

Hyper Text Transfer Protocol 超文本传输协议，用于传输HTML。

组成：

1. 请求行：请求方式 URI 协议和版本
2. 请求头：键值对
3. 请求体：POST才有请求体，GET没有请求体

端口号：80



### 与请求行有关的方法

```
getMethod() 得到请求方式
getRequestURI() 得到URI
getRequestURL() 得到URL
getProtocol() 得到协议和版本
getQueryString() 得到查询字符串
```



### 与请求头有关的方法

```
String getHeader("请求头名字") 通过请求头得到请求值
Enumeration<String> getHeaderNames() 得到所有请求头的名字
```



### 得到客户端提交的参数值

```
String getParameter("参数名")通过参数名得到参数的值
String[] getParameterValues("参数名") 通过一个参数名得到一组同名的值，用于复选框中
Enumeration<String> getParameterNames() 得到所有参数的名字
Map<String,String[]> getParameterMap() 得到表单提交的所有的键和值
```



### BeanUtils工具类

```
public static void populate(Object bean, Map map) 
将map中的所有的属性复制到实体类中相对应的属性中
```



### 汉字乱码问题

```
setCharacterEncoding("编码")
1. 用于POST方法，GET方法没有乱码 tomcat8.0以上
2. 放在所有获取参数值的方法前面
3. 必须与网页的编码一致
```



## <font color="red">请求域有关的方法</font>

### 目标

1. 有哪三个作用域
2. 操作作用域的方法

### 什么是作用域

![1572072990769](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572072990769.png)

### Servlet中的3个作用域

```
请求域 < 会话域 < 上下文域
```

### 作用域的操作方法

| request与域有关的方法                      | 作用                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| Object getAttribute("键")                  | 通过键获取值，可以是任意的对象                               |
| void setAttribute(String "键",Object 数据) | 向请求域中添加1个键和值，值是Object对象<br />如果作用域中没有这个键，就是添加<br />如果作用域中有这个键，就是覆盖 |
| void removeAttribute("键")                 | 删除作用域中键和值                                           |



### 案例：作用域的操作方法

#### 需求

1. Servlet创建一个键和值，在同一个Servlet中从请求域中取出信息，打印到浏览器上。
2. 删除请求域中的键值对，再打印到浏览器上。

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 向请求域中添加键和值
 */
@WebServlet("/one")
public class Demo6OneServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("向请求域中添加了键和值<hr/>");

        //向请求域中添加键和值
        request.setAttribute("user","孙悟空");
        request.setAttribute("age", 20);
     
        //从请求域中取出键和值
        String user = (String) request.getAttribute("user");  //向下转型
        int age = (int) request.getAttribute("age");

        out.print("用户名：" + user + "<hr/>");
        out.print("年龄：" + age + "<hr/>");

        //删除其中一个
        request.removeAttribute("age");

        //从请求域中取出键和值
        out.print("用户名：" + request.getAttribute("user") + "<hr/>");
        out.print("年龄：" + request.getAttribute("age") + "<hr/>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

1. 有哪三个作用域？

   ```
   请求域，会话域，上下文域
   ```

   

2. 说说作用域方法的作用：

   | request域有关的方法                 | 作用                 |
   | ----------------------------------- | -------------------- |
   | Object getAttribute("键")           | 通过键获取值         |
   | void setAttribute("键",Object 数据) | 向请求域中添加键和值 |
   | void removeAttribute("键")          | 删除键和值           |



## 页面的跳转：转发

### 目标

1. 转发的原理
2. 转发的方法

### 疑问

- 能否在OneServlet中保存值到请求域中，在另一个TwoServlet中打印出来？

  ```
  可以，使用转发，转发可以在一次请求中访问多个Servlet
  ```

### 转发与重定向的作用

用于页面的跳转，从一个页面跳转到另一个页面



### 什么是转发

#### 概念

在服务器端的跳转，称为转发。

#### 原理图

![1572074183738](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572074183738.png)

#### 转发的方法

```
1. 得到转发器
RequestDispatcher rd = request.getRequestDispatcher("/跳转的地址");
2. 使用转发器进行跳转
rd.forward(request, response);

通常写成一句话：
request.getRequestDispatcher("/跳转的地址").forward(request, response);
```



### 案例

#### 需求

​	实现从OneServlet中转发到TwoServlet

#### 步骤

1. OneServlet向请求域中添加了一个键和值，转发给TwoServlet
2. TwoServlet就从请求域中取出键和值，打印到浏览器上。

#### 效果

![1572074492677](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572074492677.png) 

#### 代码

##### OneServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 向请求域中添加键和值
 */
@WebServlet("/one")
public class Demo6OneServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("向请求域中添加了键和值<hr/>");

        //向请求域中添加键和值
        request.setAttribute("user","孙悟空");
        request.setAttribute("age", 20);

        //使用转发到two Servlet中 (参数是要跳转的地址)
        request.getRequestDispatcher("/two").forward(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

##### TwoServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 从请求域中取出键和值
 */
@WebServlet("/two")
public class Demo7TwoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("<h2>这是TwoServlet</h2>");
        out.print("用户名：" + request.getAttribute("user") + "<hr/>") ;
        out.print("年龄：" + request.getAttribute("age") + "<hr/>") ;
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 转发的特点

1. 地址栏：没有变化，还是显示上一个Servlet的地址
2. 请求次数：只有1次请求
3. 请求域中数据：请求域中数据没有丢失

### 小结

转发使用哪个方法？

```java
request.getRequestDispatcher("要跳转的地址").forward(request, response);
```



## 页面的跳转：重定向

### 目标

1. 重定向原理
2. 重定向的方法

### 什么是重定向

#### 概念

在浏览器端进行的页面的跳转，类似于location.href

#### 原理图

![1572075966380](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572075966380.png)

#### 重定向方法

```
response.sendRedirect("跳转的地址")
```



### 重定向案例

#### 需求

从OneServlet重定向到TwoServlet

#### 步骤

1. 在OneServlet中向请求域中添加键和值
2. 使用重定向到TwoServlet，在TwoServlet中看能否取出请求域的值

#### 效果

![1572076117901](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572076117901.png) 

#### 代码

##### OneServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 向请求域中添加键和值
 */
@WebServlet("/one")
public class Demo6OneServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("向请求域中添加了键和值<hr/>");

        //向请求域中添加键和值
        request.setAttribute("user","孙悟空");
        request.setAttribute("age", 20);

        //重定向
        response.sendRedirect("two");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

##### TwoServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 从请求域中取出键和值
 */
@WebServlet("/two")
public class Demo7TwoServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("<h2>这是TwoServlet</h2>");
        out.print("用户名：" + request.getAttribute("user") + "<hr/>") ;
        out.print("年龄：" + request.getAttribute("age") + "<hr/>") ;
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 重定向的特点

1. 地址栏：变化了，显示的是新的地址
2. 请求次数：是2次
3. 请求域中的数据：丢失

### 疑问

问：什么时候使用转发，什么时候使用重定向？

```
1. 如果要保留请求域中的数据，使用转发，否则使用重定向。
2. 在访问数据库的操作中，增删改使用重定向，查询使用转发。
```




### 小结：重定向和转发的区别

| 区别                   | 请求转发forward()                   | 响应重定向sendRedirect()                   |
| ---------------------- | ----------------------------------- | ------------------------------------------ |
| 地址栏是否发生变化     | 不会                                | 会                                         |
| 由哪一端进行的跳转     | 服务器端                            | 浏览器端                                   |
| 请求域中数据会不会丢失 | 不会                                | 会                                         |
| 根目录不同             | http://localhost:8080/项目访问地址/ | http://localhost:8080/                     |
| 跳转范围               | 只能在同一个项目中转发              | 可以跳转到任意的地址<br />甚至其它的服务器 |



## 登录案例part1：项目搭建

### 目标

搭建项目的结构

### 需求

1. 用户名和密码正确，将用户的信息保存在请求域中，转发到另一个页面，显示用户登录成功
2. 用户名和密码错误，重定向到另一个html页面，显示登录失败。
3. 使用表示层，业务层，数据访问层的三层结构实现
4. 数据库使用mybatis来访问

### 案例流程图

![1572077196357](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572077196357.png)



### 实现步骤

#### 创建用户表

```mysql
create table `user`(
    id int primary key auto_increment,
    username varchar(20),
    password varchar(32)
 );
 
 insert into `user`(username, password) values ('Jack','123'),('张三','456');
 
 select * from `user`;
```

#### 登录页面

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>用户登录</title>
</head>
<body>
<h2>用户登录</h2>
<form action="login" method="post">
    <table>
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username"/></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password"/></td>
        </tr>
        <tr>
           <td colspan="2"><input type="submit" value="登录"/></td>
        </tr>
    </table>
</form>
</body>
</html>
```

#### 创建WEB-INF\lib文件夹，导入jar包

![1572077444256](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1572077444256.png) 

#### 实体类User

```java
package com.itheima.entity;

public class User {
    
    private int id;
    private String username;
    private String password;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
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

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

```

#### sqlMapConfig.xml 放在src下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--定义实体类别名-->
    <typeAliases>
        <package name="com.itheima.entity"/>
    </typeAliases>

    <environments default="default">
        <!--环境变量-->
        <environment id="default">
            <!--事务管理器-->
            <transactionManager type="JDBC"/>
            <!--数据源-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/day25"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <!--加载其它映射文件-->
    <mappers>
        <package name="com.itheima.dao"/>
    </mappers>
</configuration>
```

#### 会话工具类

```java
package com.itheima.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class SqlSessionUtils {

    //会话工厂对象
    private static SqlSessionFactory factory;

    //在静态代码中创建会话工厂
    static {
        //会话工厂的创建类 --> 会话工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //得到配置文件的输入流
        try(InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml")) {
            factory = builder.build(resourceAsStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 得到会话对象
     */
    public static SqlSession getSession() {
        return factory.openSession();
    }
}
```



## 登录案例part2：数据访问层和业务层

### 目标

1. 编写UserDao代码
2. 编写UserService代码

### 步骤

#### UserDao

通过用户名和密码查询一个用户

#### UserService

业务类直接调用DAO类中相应的方法返回用户对象即可

### 代码

#### UserDao

```java
package com.itheima.dao;

import com.itheima.entity.User;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

/**
 * 数据访问层
 */
public interface UserDao {

    /**
     * 通过用户名和密码查询指定的用户
     * @param username 用户名
     * @param password 密码
     * @return 如果用户存在就返回对象，不存在就返回null
     */
    @Select("select * from user where username=#{username} and password=#{password}")
    User findUser(@Param("username") String username, @Param("password") String password);

}

```

```java
package com.itheima.dao.impl;

import com.itheima.dao.UserDao;
import com.itheima.entity.User;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

public class UserDaoImpl implements UserDao {
    /**
     * 通过用户名和密码查询指定的用户
     * @param username 用户名
     * @param password 密码
     * @return 如果用户存在就返回对象，不存在就返回null
     */
    @Override
    public User findUser(String username, String password) {
        //1. 得到会话
        SqlSession session = SqlSessionUtils.getSession();
        //2. 通过会话得到代理对象
        UserDao userDao = session.getMapper(UserDao.class);
        //3. 调用接口中方法，得到用户
        User user = userDao.findUser(username, password);
        //4. 关闭会话
        session.close();
        //5. 返回用户对象
        return user;
    }
}
```



#### 测试DAO

```java
package com.itheima.test;

import com.itheima.dao.UserDao;
import com.itheima.dao.impl.UserDaoImpl;
import com.itheima.entity.User;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

public class TestSession {

    @Test
    public void testSession() {
        SqlSession session = SqlSessionUtils.getSession();
        System.out.println(session);
    }

    @Test
    public void testFindUser() {
        UserDao userDao = new UserDaoImpl();
        User user = userDao.findUser("Jack", "456");
        System.out.println(user);
    }

}

```

#### UserService

```java
package com.itheima.service;

import com.itheima.dao.UserDao;
import com.itheima.dao.impl.UserDaoImpl;
import com.itheima.entity.User;

/**
 * 业务层
 */
public class UserService {

    private UserDao userDao = new UserDaoImpl();

    /**
     * 登录的方法
     */
    public User login(String username, String password) {
        return userDao.findUser(username, password);
    }

}

```



## 登录案例part3：Servlet

### 目标

1. 登录的LoginServlet
2. 登录成功的SuccessServlet

### LoginServlet

#### 步骤

1. 解决汉字乱码问题
2. 得到用户提交的用户名和密码
3. 调用业务层实现登录操作
4. 如果用户名不为空表示登录成功
5. 把当前的用户信息放在请求域中，以后放在会话域中
6. 转发到success
7. 如果登录失败重定向到failure.html页面

#### 代码

```java
package com.itheima.servlet;

import com.itheima.entity.User;
import com.itheima.service.UserService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //0. 设置请求的编码
        request.setCharacterEncoding("utf-8");
        //1. 得到用户名和密码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //2. 调用业务层去判断是否登录成功
        UserService userService = new UserService();
        User user = userService.login(username, password);
        //3. 如果登录成功，将用户放在请求域，转发到SuccessServlet
        if (user != null) {
            request.setAttribute("user", user);
            request.getRequestDispatcher("/success").forward(request, response);
        }
        //4. 如果登录失败，重定向到failure.html
        else {
            response.sendRedirect(request.getContextPath() + "/failure.html");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### SuccessServlet

#### 步骤

1. 设置响应的类型和字符集
2. 得到打印流对象
3. 从请求域中取出用户信息
4. 输出登录成功的信息

#### 代码

```java
package com.itheima.servlet;

import com.itheima.entity.User;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/success")
public class SuccessServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //从请求域中取出用户的信息
        User user = (User) request.getAttribute("user");
        out.print("<h2>登录成功，欢迎您！" + user.getUsername() + "</h2>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 小结

1. 用了三层结构：表示层(HTML+Servlet)，业务层，数据访问层
2. DAO层使用了查询方法: findUser()
3. 业务层直接调用DAO层中的方法，实现登录
4. Servlet中用到了得到表单参数的方法：getParameter()
5. Servlet中用到了转发和重定向
6. Servlet中用到了请求域有关的方法：getAttribute()取出请求域中的数据



# 学习总结

1. 能够使用工具查看HTTP协议内容 

   ![1552551173484](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552551173484.png)

2. 能够理解HTTP协议请求内容

   ![1552564604092](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/请求对象/1552564604092.png)

3. 能够使用Request对象获取HTTP协议请求内容

   | 请求方法                            | 功能描述           |
   | ----------------------------------- | ------------------ |
   | String getHeader(String   headName) | 得到一个请求头的值 |
   | String getMethod()                  | 得到请求的方式     |
   | String getRequestURI()              | 得到URI            |

   | 方法名                                   | 描述                              |
   | ---------------------------------------- | --------------------------------- |
   | String getParameter(String name)         | 得到参数值                        |
   | String[] getParameterValues(String name) | 得到一组参数值                    |
   | Map<String,String[]> getParameterMap()   | 得到所有的参数名和值，封装成了Map |

4. 能够处理HTTP请求参数的乱码问题 

   GET: 在8.0以上没有乱码的问题

   POST: 

   ```java
   setCharacterEncoding("编码")
   ```

5. 能够使用Request域对象 

   | request与域有关的方法               | 作用                 |
   | ----------------------------------- | -------------------- |
   | Object getAttribute("键")           | 获取请求域中值       |
   | void setAttribute("键",Object 数据) | 向请求域中添加键和值 |
   | void removeAttribute("键")          | 删除请求域中的键和值 |

6. 能够使用Request对象做请求转发

   转发的方法：

   ```java
   request.getRequestDispatcher("/地址").forward(request, response)
   ```

   重定向的方法：

   ```java
   response.sendRedirect("地址")
   ```

7. 能够完成登录案例