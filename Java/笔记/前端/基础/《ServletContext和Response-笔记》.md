# 《ServletContext和Response-笔记》

# 回顾

### 请求有哪三个的组成部分

请求行，请求头，请求体

###  获取请求行相关的方法

| HttpServletRequest对象的方法 | 功能描述       |
| ---------------------------- | -------------- |
| String getMethod()           | 得到请求的方式 |
| String  getRequestURI()      | 得到请求的URI  |
| String getProtocol()         | 得到协议和版本 |
| String getQueryString()      | 得到查询字符串 |

 

### 获取请求头相关的方法

| 请求方法                                | 功能描述                     |
| --------------------------------------- | ---------------------------- |
| String getHeader(String   headName)     | 通过请求头的名字得到请求头值 |
| Enumeration\<String>   getHeaderNames() | 得到所有的请求头的名字       |

 

### 获取请求参数的方法

| 方法名                                     | 描述                                |
| ------------------------------------------ | ----------------------------------- |
| String getParameter(String   name)         | 通过参数名得到参数值                |
| String[]   getParameterValues(String name) | 通过参数名得到一组参数值            |
| Map<String,String[]>   getParameterMap()   | 得到所有的参数名和值，封装成Map对象 |


| POST方式乱码解决方法   | 说明                                                   |
| ---------------------- | ------------------------------------------------------ |
| setCharacterEncoding() | 1. 放在所有获取值方法前面<br />2. 必须与网页的编码一致 |

## 请求域有关的方法

| request与域有关的方法         | 作用                   |
| ----------------------------- | ---------------------- |
| Object getAttribute("键")     | 获取请求域中值         |
| setAttribute("键",Object数据) | 添加或更新请求域中的值 |
| removeAttribute("键")         | 删除指定的键和值       |

## 转发与重定向的区别

| 区别                   | 转发               | 重定向         |
| ---------------------- | ------------------ | -------------- |
| 方法                   | forward()          | sendRedirect() |
| 地址栏会不会变         | 不会               | 会             |
| 跳转方                 | 在服务器端         | 在浏览器端     |
| 请求域中数据会不会丢失 | 不会               | 会             |
| 根目录                 | 包含了项目访问地址 | 不包含         |

 

# 学习目标

1. HTTP响应的格式
   1. 能够使用使用浏览器开发工具查看响应
   2. 能够理解响应行（状态行）的内容
   3. 能够理解常见的状态码
2. 响应对象的方法
   1. 能够使用Response对象操作HTTP响应内容
   2. 能够处理响应乱码
   3. 能够完成文件下载案例
3. 能够使用ServletContext域对象



# 学习内容

## 响应的三个组成部分

### 目标

HTTP响应由哪三个组成部分

### 什么是HTTP响应

响应行(状态行)，响应头，响应体

### 数据准备

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

### 响应信息的三个组成

![1572225666722](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572225666722.png) 

### 小结

响应有哪三个组成部分？

```
响应行，响应头，响应体
```



## 响应行、响应头、响应体的格式

### 目标

1. 响应行的格式
2. 响应头的格式
3. 响应体的格式

### 响应行

3个组成： 协议和版本 状态码 状态信息

```
HTTP/1.1 200 OK
```

不同的状态码表示服务器不同的运行状态

### 响应头

| 响应头信息                                                | 说明                                                         |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| Location:   http://www.itcast.cn                          | 发生了重定向，显示下一个要访问的地址                         |
| Server:Apache Tomcat                                      | 说明服务器的名字 Server: BWS/1.1                             |
| Content-Encoding:   gzip                                  | 服务器使用的数据压缩格式                                     |
| Content-Length: 80                                        | 响应数据的长度，单位是字节                                   |
| Content-Type:   text/html; charset=utf-8                  | 响应的数据类型                                               |
| Refresh:  1;url=/hello.html                               | 过几秒后跳转到另一个页面                                     |
| Content-Disposition:   attachment; filename=文件名.扩展名 | 设置文件打开的方式<br />默认是在浏览器中打开<br />可以设置为下载的方式，并且指定文件 |

### 响应体

服务器发送给浏览器的数据

1. 文本：网页，CSS，JavaScript等。使用字符流来处理

2. 二进制数据：图片，声音，视频以二进制的方式处理。使用字节流来处理

### 小结：响应的组成

![1552645116046](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552645116046.png)



## 响应对象的方法：与状态码有关

### 目标

与响应行有关的方法

### 什么是HttpServletResponse对象

代表一个响应对象

![1572226697077](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572226697077.png) 

1. 由谁提供实现类：Tomcat
2. 由谁创建此对象：Tomcat



### 设置状态码的方法

通常单独设置状态码没有太多的意义

### 演示代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
// 静态导入，导入所有的static的内容，调用静态的方法或变量不需要使用类名
import static javax.servlet.http.HttpServletResponse.*;

@WebServlet("/demo1")
public class Demo1Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.设置响应的状态码
        //response.setStatus(203);
        //在接口中也定义大量的常量
        //response.setStatus(SC_NON_AUTHORITATIVE_INFORMATION);

        //2. 发送错误码
        //response.sendError(402);

        //3. 发送错误码，同时指定错误的信息
        response.sendError(404, "找不到了");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| 状态码的方法          | 描述           |
| --------------------- | -------------- |
| setStatus(int status) | 单独设置状态码 |
| sendError(int   sc)   | 发送错误码     |



## 常见的状态码

### 目标

1. 响应行中常见状态码的含义
2. 404与405出现的原因

### 常见状态码的含义

#### 200

服务器正常的响应

#### 302

在浏览器端进行了页面的跳转(重定向)

#### 304

在浏览器端使用的是缓存的网页

#### 404

1. 输入错误：在tomcat访问地址是区分大小写的。

2. 访问了不能访问的资源，如：WEB-INF资源是不能通过浏览器直接访问的。

   ![1572228821457](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572228821457.png) 

3. 重定向或转发的地址不正确

   ![1572228882459](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572228882459.png) 

#### 405

错误是父类:HttpServlet

使用了get方式请求资源，但Servlet中没有doGet方法

![1572228953693](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572228953693.png)

使用了post方式请求，但Servlet中没有doPost方法
![1572229026035](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572229026035.png)



#### 500

服务器出现了异常，通常就是Java代码有错误

![1572229239277](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572229239277.png) 

### 小结

| 状态码 | 含义                      |
| ------ | ------------------------- |
| 200    | 服务器正常响应            |
| 302    | 页面跳转                  |
| 304    | 使用缓存                  |
| 404    | 找不到页面                |
| 405    | 没有重写doGet或doPost方法 |
| 500    | 服务器出现异常            |



## 响应对象：响应头相关方法

### 目标

1. 学习响应头相关的方法
2. 案例：过3秒跳转到其它页面

### 响应头的方法

| 响应头的方法                                | 描述                                                |
| ------------------------------------------- | --------------------------------------------------- |
| void setHeader(String   name, String value) | 设置某个响应头的键和值                              |
| void setContentType(String   type)          | 这个方法其实是<br />setHeader("content-type", "值") |

### 案例：设置响应头过3秒跳转

#### 步骤

1. 创建ResponseRefreshServlet

2. 调用setHeader，设置消息头（"Refresh","3;url=http://www.itcast.cn"）


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

/*
设置响应头过3秒跳转
 */
@WebServlet("/demo2")
public class Demo2HeaderServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置响应头
        response.setHeader("refresh","3;url=http://www.baidu.com");

        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("<h2>过3秒以后跳转到百度</h2>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### 案例：使用location实现页面跳转

#### 步骤

1. 只设置location响应头
2. 同时设置302状态码
3. 使用重定向的方法跳转

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 使用location响应头，实现重定向
 */
@WebServlet("/demo3")
public class Demo3LocationServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置响应头：location
        //response.setHeader("location", "login.html");
        //如果要跳转，必须设置状态码为302
        //response.setStatus(302);

        //专门用于重定向的方法，本质上就是使用了上面的2个方法
        response.sendRedirect("login.html");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| 响应头的方法                                | 描述                   |
| ------------------------------------------- | ---------------------- |
| void setHeader(String   name, String value) | 设置某个响应头的键和值 |



## 案例：响应头数据压缩

### 目标

在服务器端对数据压缩后在浏览器端显示出来

### 需求

使用数据压缩之后再从服务器传输数据到浏览器, 可以减少网络的传输量,提高网页的下载速度。

### GZIPOutputStream类

创建压缩输出流，将数据以gzip的格式进行压缩

### 步骤

1. 如果需要对数据进行压缩，需要使用压缩流，将响应的输出流做为参数。

2. 使用压缩流的write方法：首先会对数据进行压缩处理，然后调用传递进去OutputStream对象的write方法把压缩的数据写出去。

3. 完成将压缩数据写入输出流的操作，如果没有调用结束的方法，则浏览器上看不到东西。

### 要点

要设置相应响应头信息，告诉浏览器目前内容是以压缩包的形式传输的，不然就会以附件的方式下载下来了。 

```
response.setHeader("Content-Encoding","gzip")
```

### 响应头       

![1572230774020](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572230774020.png) 

### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.util.zip.GZIPOutputStream;

@WebServlet("/demo4")
public class Demo4GzipServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //创建一个大的字符串
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 5000; i++) {
            sb.append("abcdefghijk");
        }
        System.out.println("在服务器端的数据长度：" + sb.length());

        //在浏览器端打印, plain表示这是纯文本(类似于innerHTML和innerText)
        response.setContentType("text/plain;charset=utf-8");

        //告诉浏览器，先解压再显示
        response.setHeader("content-encoding","gzip");

        //得到响应的字节输出流
        OutputStream os = response.getOutputStream();
        //压缩流，对数据进行压缩以后再输出。构造方法输入需要压缩的字节流(要对响应的输出流进行压缩)
        GZIPOutputStream outputStream = new GZIPOutputStream(os);
        //把要输出的内容转成一个字节数组
        byte[] buf = sb.toString().getBytes("utf-8");
        //调用压缩流的方法写到浏览器端
        outputStream.write(buf);
        //清空缓存
        outputStream.finish();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

GZIPOutputStream类构造方法，带什么参数？ 字节输出流

```
字节流和字符流不能同时使用，否则会出现500的错误
```



| GZIPOutputStream类的方法    | 说明                   |
| --------------------------- | ---------------------- |
| public void write(byte[] b) | 将字节数组写到浏览器端 |
| void finish()               | 清空缓存               |



## 响应体：处理响应乱码的问题

### 目标

1. 响应体数据的两种方式
2. 处理汉字乱码的问题

### 响应体的两种数据

1. 字节流
2. 字符流

正好对应以下2个方法

| 响应体的方法                     | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| OutputStream   getOutputStream() | 处理字节流，二进制的数据，如：图片，声音，视频               |
| PrintWriter getWriter()          | 处理字符流，字节符流有编码的过程。处理文本的数据。<br />如果没有指定编码，默认的码表 |

### 打印流的一些疑问

直接使用打印输出流，输出汉字的结果是什么？

  

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

@WebServlet("/demo5")
public class Demo5EncodingServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置一个响应头：告诉浏览器服务器端使用的编码是什么
        //response.setHeader("content-type","text/html;charset=utf-8");
        /*
        单独设置一个方法相当于上面的响应头，这个方法有2个功能：
        1. 告诉浏览器，服务器端使用的是什么编码
        2. 也会设置打印流默认的编码，相当于response.setCharacterEncoding("utf-8")这句话
         */
        response.setContentType("text/html;charset=utf-8");
        //字符流必须要指定码表，但浏览器并不知道(浏览器就会默认使用GBK的编码)
        //response.setCharacterEncoding("utf-8");
        //得到打印流
        PrintWriter out = response.getWriter();
        //没有指定码表，默认是ISO-8859-1编码，欧洲码不支持汉字
        out.print("你好，爱老虎油! 印度油! 地沟油！");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

content-type响应头的作用：

1. 告诉浏览器，服务器端使用的什么编码
2. 设置打印流默认的编码



## 如何得到ServletContext对象

### 目标

如何得到上下文对象

### 什么是ServletContext

概念：上下文对象，每个项目都对应一个上下文对象。

![1552648163439](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552648163439.png)

### 主要作用

1. 读取配置文件中web.xml，全局的配置参数
2. 获取web目录下资源
3. 获取web目录下资源的真实地址
4. 它也是一个作用域

### 得到上下文域的方法

| ServletConfig接口中方法              | 描述                                             |
| ------------------------------------ | ------------------------------------------------ |
| ServletContext   getServletContext() | 从父类中继承下来的这个方法可以获取一个上下文对象 |

### 回顾Servlet的继承结构

![1552648301076](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552648301076.png)

### 得到ServletContext的代码

```java
//如何得到上下文对象，从父类继承下来的方法
ServletContext application = getServletContext();
```

### 小结

1. 每个Web项目对应几个上下文对象？

   ```
   1个
   ```

   

2. 如何得到ServletContext？

   ```
   getServletContext()
   ```

   



## 读取全局的配置参数

### 目标

获取全局的配置参数

### 需求

在web.xml中设置2个全局的参数，username=NewBoy, age=18 在不同的Servlet中去获取这2个值，并使用上面2个方法。

### 代码

web.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--定义全局变量-->
    <context-param>
        <param-name>user</param-name>
        <param-value>Rose</param-value>
    </context-param>
    <context-param>
        <param-name>age</param-name>
        <param-value>18</param-value>
    </context-param>

    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>com.itheima.servlet.Demo1Servlet</servlet-class>
        <!--初始化配置参数：只能用在当前这个Servlet中，是一个局部变量 -->
        <init-param>
            <param-name>user</param-name>
            <param-value>Jack</param-value>
        </init-param>
        <init-param>
            <param-name>age</param-name>
            <param-value>20</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>demo2</servlet-name>
        <servlet-class>com.itheima.servlet.Demo2Servlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>demo2</servlet-name>
        <url-pattern>/demo2</url-pattern>
    </servlet-mapping>

</web-app>
```

servlet代码

```java
package com.itheima.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

//使用ServletConfig中方法：得到的是自己局部的配置参数
public class Demo1Servlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //使用ServletConfig读取自己的Servlet参数
        String user = getInitParameter("user");
        String age = getInitParameter("age");

        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("用户名：" + user + "<br/>");
        out.print("年龄：" + age + "<br/>");

        //读取全局参数，必须使用上下文对象相应的方法
        ServletContext application = getServletContext();
        //读取参数，调用上下文对象的方法
        String user2 = application.getInitParameter("user");
        out.print("全局用户名：" + user2 + "<br/>");

        //得到所有全局参数的名字
        Enumeration<String> names = application.getInitParameterNames();
        while(names.hasMoreElements()) {
            String name = names.nextElement();  //得到每个参数名
            String value = application.getInitParameter(name);  //调用上面的方法得到值
            out.print("全局参数名：" + name + "，全局参数值：" + value + "<hr/>");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 效果

![1572234797481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572234797481.png) 

### 小结

| ServletContext方法                           | 功能                         |
| -------------------------------------------- | ---------------------------- |
| String getInitParameter(String name)         | 通过参数名得到全局的参数值   |
| Enumeration\<String> getInitParameterNames() | 得到所有的全局的参数名字枚举 |



## 上午回顾

### Http响应的格式

三个组成：

1. 响应行(状态行)： HTTP/1.1 200 OK  协议和版本 状态码 状态信息
2. 响应头：由多个键值对组成
3. 响应体：服务器发送给浏览器的数据
   1. 字节流：处理二进制数据
   2. 字符流：处理文本类型数据

### 常见的状态码

- 200：服务器正常响应
- 302：重定向，浏览器端进行页面跳转
- 304：使用缓存
- 404：找不到资源
- 405：没有重写doGet或doPost方法
- 500：服务器出现异常

### HttpServletResponse响应对象的方法

```
setStatus() 设置状态码
sendError() 发送错误码

setHeader(键,值) 设置一个响应头的键和值
setContentType() 设置响应内容的类型

getOutputStream() 处理字节流
getWriter() 处理字符流
```



### 响应头应用的案例

| 响应头                 | 说明                                            |
| ---------------------- | ----------------------------------------------- |
| refresh: 秒; url=地址  | 过多少秒跳转到另一个地址                        |
| location: URL          | 重定向，下一个要跳转到的地址。必须指定302状态码 |
| content-encoding: gzip | 服务器端数据进行压缩，浏览器端要进行解压        |

### 响应乱码问题

1. 如果直接使用getWriter()输出汉字有乱码问题
2. response.setCharacterEncoding() 设置响应编码
3. response.setContentType() 设置编码



### ServletContext上下文对象

每个运行的项目都有一个上下文对象，1对1关系。

如何获取上下文对象？

```
方法定义在ServletConfig接口中，HttpServlet继承了这个方法，在我们的Servlet中可以直接使用
getServletContext()
```

作用：

1. 获取全局的配置参数
2. 读取web目录下资源
3. 获取web目录资源的真实地址
4. 也是一个作用域对象

### 获取全局配置参数

```
getInitParameter() 通过键得到全局参数值
getInitParameterNames() 得到所有全局参数的名字
```



## <font color="red">案例：获取当前工程的资源</font>

### 目标

得到web目录下的某个图片资源在浏览器上显示出来

### 方法

| ServletContext的方法                          | 功能                                |
| --------------------------------------------- | ----------------------------------- |
| InputStream  getResourceAsStream(String path) | 读取web目录下资源，返回成一个输入流 |

### 案例：读取Web目录下的资源文件

#### 执行效果

![1572245747062](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572245747062.png) 

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

@WebServlet("/demo2")
public class Demo2ResourceServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置响应类型为图片(可选)
        response.setContentType("image/jpg");
        //2. 得到web目录下资源，转成输入流
        InputStream inputStream = getServletContext().getResourceAsStream("/WEB-INF/404.jpg");
        //3. 得到响应的字节输出流
        OutputStream outputStream = response.getOutputStream();
        //4. 循环将输入流中数据写到输出流中
        byte[] buf = new byte[1024];
        int len = 0;
        while ((len = inputStream.read(buf)) != -1) {
            outputStream.write(buf, 0, len);
        }
        //5. 关闭流
        inputStream.close();
        outputStream.close();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

| 使用IOUtils工具类的方法                  | 功能 |
| ---------------------------------------- | ---- |
| copy(InputStream   in, OutputStream out) |      |

#### 改进代码

```java
package com.itheima.servlet;

import org.apache.commons.io.IOUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

@WebServlet("/demo2")
public class Demo2ResourceServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置响应类型为图片(可选)
        response.setContentType("image/jpg");
        //2. 得到web目录下资源，转成输入流
        InputStream inputStream = getServletContext().getResourceAsStream("/WEB-INF/404.jpg");
        //3. 得到响应的字节输出流
        OutputStream outputStream = response.getOutputStream();
        //4. 循环将输入流中数据写到输出流中
        IOUtils.copy(inputStream, outputStream);

        //5. 关闭流
        inputStream.close();
        outputStream.close();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| ServletContext的方法                           | 功能                              |
| ---------------------------------------------- | --------------------------------- |
| InputStream   getResourceAsStream(String path) | 读取web目录下资源，返回输入流对象 |

```
IOUtils.copy(InputStream in, OutputStream out)
```



## 案例：获取资源在服务器的真实地址

### 目标

1. 使用上下文对象得到web资源真实的地址
2. 删除指定的文件

### 方法

```
File类中delete()方法，删除指定的文件
```

### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo3")
public class Demo3RealPathServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 得到要删除文件在服务器上真实地址
        //1.1 得到上下文对象
        ServletContext application = getServletContext();
        //1.2 调用方法得到真实地址
        String realPath = application.getRealPath("/img/ff.jpg");
        //1.3 输出到网页上
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("图片真实地址： " + realPath + "<hr/>");

        //2. 创建文件对象
        File file = new File(realPath);

        //3. 调用文件对象删除方法
        file.delete();

        out.print("文件被删除了");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 执行结果

![1572246755505](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572246755505.png) 

### 小结

| ServletContext的方法              | 功能                                                |
| --------------------------------- | --------------------------------------------------- |
| String   getRealPath(String path) | 获取web目录下指定资源的真实地址(部署到服务器上地址) |



## 上下文域

### 目标

1. 上下文域的操作方法
2. 得到第几个登录用户的分析

### 上下文域

请求域只在一次请求中起作用，如果想要多次请求共享相同的数据，或不同的用户之间需要共享数据。需要选择更大的作用域。

作用域的功能：在不同的Servlet之间或不同的用户之间实现数据的共享。

![1552655112707](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552655112707.png)

### 上下文域的作用范围

只要服务器没有关闭或重启，上下文域中数据就一直存在。

![1552655319293](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552655319293.png)



## 案例：判断是第几个登录的用户

### 案例流程

![1572248144323](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1572248144323.png)

### LoginServlet分析

![1552655394873](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552655394873.png)

### 

## 案例：得到当前是第几个登录的用户实现

### 目标

得到当前是第几个登录的用户的代码实现

### 步骤

1. 在init方法中得到上下文对象

2. 创建count=0，并且将值放入上下文域中.

3. 登录方法中得到用户名和密码

4. 判断用户名和密码是否正确

5. 如果正确，得到上下文对象

6. 从上下文域中取出count

7. 加1再更新上下文域

8. 重定向到另一个Servlet

9. 否则跳转到登录页面

### 代码

#### HTML

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

#### 登录的Servlet

```java
package com.itheima.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/login")
public class Demo4LoginServlet extends HttpServlet {

    /*
    第一次用户访问的时候执行1次
     */
    @Override
    public void init() throws ServletException {
        //向上下文域中添加一个count=0
        getServletContext().setAttribute("count", 0);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //得到上下文对象
        ServletContext application = getServletContext();
        //1. 登录
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if ("admin".equals(username) && "123".equals(password)) {
            //2. 从上下文域中取出原来的值
            int count = (int) application.getAttribute("count");
            //3. 更新上下文域中值
            application.setAttribute("count", ++count);
            //4. 转发到count地址
            //request.getRequestDispatcher("/count").forward(request, response);
            //使用重定向可以避免重复提交的问题(以后还有其它解决方法)
            response.sendRedirect(request.getContextPath() + "/count");
        }
        else {
            response.setContentType("text/html;charset=utf-8");
            PrintWriter out = response.getWriter();
            out.print("登录失败，<a href='login.html'>请重试</a>");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### 显示人数的Servlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/count")
public class Demo5CountServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("<h2>恭喜，登录成功，您是第" + getServletContext().getAttribute("count") + "个登录的用户</h2>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| ServletContext的方法                | 作用                 |
| ----------------------------------- | -------------------- |
| Object getAttribute("键")           | 从上下文域中取出值   |
| void setAttribute("键",Object 数据) | 向上下文域添加键和值 |
| void removeAttribute("键")          | 删除上下文域中键和值 |



## URL编码和解码

### 目标

1. 为什么需要URL编码和解码
2. 如何实现URL编码和解码

### 为什么需要URL编码和解码

1. 表单中提交汉字，使用GET提交，查看请求行的数据

   ```
   demo1?name=张三
   URL编码以后，使用UTF-8码表：
   demo1?name=%E5%BC%A0%E4%B8%89
   ```

   请求：浏览器在默认的情况下对汉字或特殊字符使用URL编码，服务器就需要进行URL的解码。

   响应：服务器端编码，浏览器也对会服务器返回的特殊字符使用URL解码

   

2. 在URL中 &,?,# 这些符号有什么含义？

   ```
   ? 分隔地址和参数
   & 分隔多个参数
   # 锚点
   ```

   



### 有关的方法

| URL编码和解码有关的方法             | 功能                         |
| ----------------------------------- | ---------------------------- |
| URLEncoder.encode("字符串","utf-8") | 对参数1中的字符串使用URL编码 |
| URLDecoder.decode("字符串","utf-8") | 对参数1中的字符串使用URL解码 |

### 案例演示代码

```java
package com.itheima.test;

import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.net.URLEncoder;

public class TestCode {

    public static void main(String[] args) throws UnsupportedEncodingException {
        String name = "张三";
        System.out.println("原字符串：" + name);
        //1. 使用URL编码
        String str = URLEncoder.encode(name, "utf-8");
        System.out.println("URL编码后：" + str);
        //2. URL解码
        String decode = URLDecoder.decode(str, "utf-8");
        System.out.println("URL解码后：" + decode);
    }

}

```

### 小结

URL编码的方法： URLEncoder.encode(name, "utf-8");

URL解码的方法：URLDecoder.decode(str, "utf-8");



## 案例：使用链接下载文件不足

### 目标

链接下载文件存在的问题

### 页面效果

&nbsp;![1552655687657](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552655687657.png)

### 下载页面

```html
<!DOCTYPE html>
<html>
  <head>
    <title>资源下载列表</title>
    <meta charset="utf-8">
  </head>
  
  <body>
   <h2>文件下载页面列表</h2>
   <h3>超链接的下载</h3>
   <a href="download/file.txt">文本文件</a><br/>
   <a href="download/file.jpg">图片文件</a><br/>
   <a href="download/file.zip">压缩文件</a><br/>
   <hr/>
   <h3>手动编码的下载方式</h3>
   <!-- 注：down这里是下载的Servlet的访问地址 -->
   <a href="down?filename=file.txt">文本文件</a><br/>
   <a href="down?filename=file.jpg">图片文件</a><br/>
   <a href="down?filename=file.zip">压缩文件</a><br/>
   <a href="down?filename=美女.jpg">美女</a><br/>
  </body>
</html>
```

### 小结：使用超链接下载的不足

1. 不是所有的文件都可以下载，如果浏览器能够识别的文件类型，是直接打开的
2. 会暴露资源在服务器上真实地址，会有被盗链的可能。
3. 不利于代码的业务逻辑控制，如：下载前要扣积分，判断是否是会员



## 案例：使用Servlet下载文件

### 目标

1. 实现使用Servlet的方式下载文件
2. 文件名有汉字的处理

### 步骤

1. 从链接上得到文件名
2. 设置content-disposition头
3. 得到文件的输入流
4. 得到response的输出流
5. 写出到浏览器端

### 汉字乱码原理

![1552656145066](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552656145066.png)

<font color="red">注：IE、Chrome下载中文采用的是URL编码，注：FireFox不能采用这种方式。</font>

### 下载的Servlet代码

```java
package com.itheima.servlet;

import org.apache.commons.io.IOUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URLEncoder;

@WebServlet("/down")
public class Demo6DownServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 得到要下载的文件名
        String filename = request.getParameter("filename");
        //1.5 如果让浏览器下载文件，必须指定响应头：content-disposition，对汉字进行编码
        response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode(filename,"utf-8"));
        //2. 得到资源的输入流, / 表示web目录
        InputStream in = getServletContext().getResourceAsStream("/download/" + filename);
        //3. 得到响应的输出流
        OutputStream out = response.getOutputStream();
        //4. 将输入流复制到输出流中
        IOUtils.copy(in, out);
        //5. 关闭资源
        in.close();
        out.close();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

1. 下载需要设置哪个响应头？

   ```
   content-disposition: attachment;filename=文件名
   ```

   

2. 如果下载文件名中有汉字使用哪个方法编码？

   ```
   URLEncoder.encode("文件名","utf-8")
   ```



## 绘图有关的类和方法介绍

### 目标

1. 了解Java中与绘图有关的类
2. Graphics类中的方法

### 绘图有关的类

![1552656901525](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552656901525.png)

### Graphics类的方法

![1552656949821](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552656949821.png)



## 验证码的绘制

### 目标

​	实现验证码的绘制

### 原理

​	验证码就是一张图片，验证码图片不是真实的资源图片而是缓存图片，图片的数据在缓存里面或内存里面，将内存中的缓存图片输出到浏览器上。

### 步骤

#### 写一个方法随机获取颜色

```java
private Color getRandomColor()
```

#### 创建生成验证码的代码

1. 绘制矩形区域
   1. 创建缓存图片：指定宽width=90,height=30
   2. 获取画笔对象
   3. 设置画笔颜色
   4. 填充矩形区域
2. 随机绘制4个验证码
   1. 从字符数组中随机得到字符 char[] arr = { 'A', 'B', 'C', 'D', 'N', 'E', 'W', 'b', 'o', 'y', '1', '2', '3', '4','5','6','7','8' };
   2. 循环4次，画4个字符
   3. 设置字的颜色为随机
   4. 设置字体，大小为18， 
   5. 将每个字符画到图片，x增加，y不变。10+(i*20), 20
3. 绘制8条干扰线
   1. 线的位置是随机的，x范围在width之中，y的范围在height之中。
   2. 画8条干扰线，每条线的颜色不同
   3. 将缓存的图片输出到响应输出流中

###  验证码Servlet代码

```java
package com.itheima.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

@WebServlet("/code")
public class PicCodeServlet extends HttpServlet {

    private Random ran = new Random();

    /*
    随机得到一种颜色
     */
    private Color getRanColor() {
        int r = ran.nextInt(256);
        int g = ran.nextInt(256);
        int b = ran.nextInt(256);
        //构造方法有三个参数：红，绿，蓝
        return new Color(r, g, b);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("image/jpg");

        int width = 90, height = 30;
        //创建图片对象，参数1：宽，参数2：高，参数3：图片模式
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //得到画笔对象
        Graphics graphics = image.getGraphics();
        //设置为白色
        graphics.setColor(Color.WHITE);
        //画一个矩形区域：参数1,2 ：起点x,y坐标，参数3，4：宽和高
        graphics.fillRect(0, 0, width, height);

        //创建一个数组
        char[] arr = {'A', 'B', 'C', 'D', 'N', 'E', 'W', 'b', 'o', 'y', '1', '2', '3', '4', '5', '6', '7', '8'};

        //设置字体，字体构造方法：名字，样式，大小
        graphics.setFont(new Font(Font.DIALOG, Font.BOLD + Font.ITALIC, 19));

        //随机取4次字符
        for (int i = 0; i < 4; i++) {
            //随机得到0~长度-1 的索引号
            int index = ran.nextInt(arr.length);
            char c = arr[index];
            //随机得到一种颜色
            graphics.setColor(getRanColor());
            //画这个字符：字符串，x和y的坐标   10+(i*20), 20
            graphics.drawString(String.valueOf(c), 10 + (i * 20), 20);
        }

        //画8条干扰线
        for (int i = 0; i < 8; i++) {
            //在宽度上随机取x，在高度上随机取y
            graphics.setColor(getRanColor());
            int x1 = ran.nextInt(width);
            int y1 = ran.nextInt(height);
            int x2 = ran.nextInt(width);
            int y2 = ran.nextInt(height);
            graphics.drawLine(x1, y1, x2, y2);
        }

        //输出到浏览器上，参数1: 图片对象，参数2: 图片格式，参数3：输出流
        ImageIO.write(image, "jpg", response.getOutputStream());
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



## 验证码的使用

### 目标

1. 将上面创建的验证码用在HTML页面中
2. 点击验证码图片刷新验证码的功能

&nbsp;![1552656787141](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552656787141.png)

### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>登录页面</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.2.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
  </head>
  <body>
    <div class="container" style="max-width:400px"">
	    <h3 style="text-align: center">用户登录</h3>
	    <form action="login" method="post" >
	    	<div class="form-group">
	    		<label for="name">用户名：</label>
	    		<input type="text" name="name" class="form-control" id="name" placeholder="请输入用户名">
	    	</div>
	    	<div class="form-group">
	    		<label for="password">密码：</label>
	    		<input type="password" name="password" class="form-control" id="password" placeholder="请输入密码"/>
	    	</div>
			<div class="form-inline">
	    		<label for="vcode">验证码：</label>
	    		<input type="text" name="vcode" class="form-control" id="vcode" placeholder="验证码" style="width: 70px" maxlength="4"/>&nbsp;
                <!--src指定servlet的访问地址-->
				<img id="picCode" src="code" style="width: 90px; height: 30px; cursor: pointer;" title="看不清，点击刷新">
                <script type="text/javascript">
                     //编写图片的点击事件
                    document.getElementById("picCode").onclick = function () {
                        //每次从服务器读取图片，每次传递不同的参数
                        this.src = "code?a=" + new Date().getTime();
                    }
                </script>

	    	</div>
	    	<div style="text-align: center; padding-top: 20px;">
		    	<input type="submit" value=" 登 录 " class="btn btn-primary"/>
	    	</div>
	    </form>
	    </div>
  </body>
</html>
```

### 小结

1. 在Servlet中使用java.awt包中的类绘制图片(了解)
2. 在HTML上使用img标签访问Servlet，显示验证码
3. 实现点击验证码刷新的功能





# 学习总结

1. 能够使用使用浏览器开发工具查看响应

   ![1552645116046](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/响应对象/1552645116046.png)

2. 能够理解响应行（状态行）的内容

   1. 协议和版本
   2. 状态码
   3. 状态码描述

3. 能够理解常见的状态码

   | 状态码 | 含义                  |
   | ------ | --------------------- |
   | 200    | 正常响应              |
   | 302    | 重定向                |
   | 304    | 缓存                  |
   | 404    | 找不到页面            |
   | 405    | 没有重写doGet或doPost |
   | 500    | 服务器异常            |

4. 能够使用Response对象操作HTTP响应内容

   | 响应头的方法                                | 描述                   |
   | ------------------------------------------- | ---------------------- |
   | setStatus(int status)                       | 设置状态码             |
   | void setHeader(String   name, String value) | 设置响应头的键和值     |
   | OutputStream getOutputStream()              | 得到字节输出流         |
   | PrintWriter getWriter()                     | 得到字符输出流(打印流) |

5. 能够处理响应乱码

   | 处理响应乱码的方法                      | 描述                                                         |
   | --------------------------------------- | ------------------------------------------------------------ |
   | void setContentType(String type)        | 1. 告诉浏览器，服务器使用的编码<br />2. 设置打印流默认的编码，包含了下面的功能 |
   | response.setCharacterEncoding("字符集") | 设置打印流默认的编码                                         |

6. 能够完成文件下载案例

   | 设置响应头                                         | 参数说明                   |
   | -------------------------------------------------- | -------------------------- |
   | Content-Disposition:   attachment; filename=文件名 | 设置使用附件的方式实现下载 |

   1. 得到web目录下文件输入流
   2. 得到响应的输出流
   3. 将输入流复制到输出流
   4. 关闭流

7. 能够使用ServletContext域对象

   | ServletConfig接口中方法              | 描述           |
   | ------------------------------------ | -------------- |
   | ServletContext   getServletContext() | 得到上下文对象 |

   | ServletContext的方法                            | 作用                        |
   | ----------------------------------------------- | --------------------------- |
   | InputStream getResourceAsStream(String path)    | 得到web目录下资源的输入流   |
   | String getRealPath(String path)                 | 得到web目录下资源的真实地址 |
   | setAttribute() getAttribute() removeAttribute() | 操作作用域的方法            |

   

