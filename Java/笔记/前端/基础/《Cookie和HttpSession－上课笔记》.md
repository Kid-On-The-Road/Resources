# 《Cookie和HttpSession－上课笔记》

# 回顾

## 获取当前工程的资源文件

| ServletContext的方法                         | 功能                                      |
| -------------------------------------------- | ----------------------------------------- |
| InputStream getResourceAsStream(String path) | 得到web目录下资源，返回字节输入流         |
| String getRealPath(String path)              | 得到web目录下资源部署到服务器上的真实地址 |

## 获取作用域对象的值 

| ServeltContext的方法                           | 作用                   |
| ---------------------------------------------- | ---------------------- |
| Object getAttribute(String   name)             | 通过键获取上下文域中值 |
| void   setAttribute(String name, Object value) | 向上下文域添加键和值   |
| void   removeAttribute(String name)            | 删除上下文域中键和值   |

## response设置响应行的方法

| 状态码的方法          | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| setStatus(int status) | 作用：设置状态码 <br/>常见状态码：<br />404：找不到网页<br />500：服务器异常<br />304：缓存<br />302：重定向<br />405：没有重写doGet或doPost方法<br />200：正常响应 |

## response设置响应头的方法

| 响应头的方法                              | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| void setHeader(String name, String value) | 设置响应头的键和值                                           |
| void setContentType(String type)          | 1. 告诉浏览器，服务器使用的编码<br />2. 设置打印流的默认编码 |

## 输出响应的两种方式

| 响应体的方法                   | 描述           |
| ------------------------------ | -------------- |
| OutputStream getOutputStream() | 得到字节输出流 |
| PrintWriter getWriter()        | 得到字符输出流 |



#  学习目标

1. Cookie
   1. 能够说出会话的概念
   2. 能够说出cookie的概念
   3. <font color="red">能够创建、发送、接收、删除cookie</font>
   4. 能够说出cookie执行原理
2. Session
   1. 能够说出session的概念
   2. <font color="red">能够获取session对象、添加、删除、获取session中的数据</font>
   3. 能够完成登录验证码案例



# 学习内容

## 两种会话的技术

### 目标：

1. 什么是会话

2. 会话有哪两种技术

### 会话的概念

- 生活中的会话

  会话就是2个人一次交流，这交流包含了多次互动。

  创建会话：电话接通

  会话过程：多次通讯过程

  会话结束：有一方挂断了电话

  ![1552693743916](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552693743916.png)

- BS结构的会话

  - 创建会话：浏览器第1次发送请求给Web服务器，由Web服务器创建会话。
  - 会话过程：浏览器多次发送请求给服务器，服务器对请求做出响应。
  - 会话结束：浏览器关闭或服务器上会话过期
  - 组成：一个会话对应一个用户，代表这个用户的多次请求和响应。

  ![1552693761999](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552693761999.png)

- 为什么有会话跟踪技术：因为HTTP协议是无状态协议，不会记录用户的访问状态，服务器并不知道多次请求来源于一个用户，还是多个用户。所以我们需要提供会话跟踪技术来识别是不是同一个用户。

- 会话的两种技术：

  - 浏览器端会话技术：Cookie
  - 服务器端会话技术：HttpSession

   

### 小结：

  1. 什么是会话？ 一个用户发送的多次请求，并且服务器做出多次响应，一个用户对应一个会话。

  2. 会话有哪两种技术？

     1. 浏览器端Cookie(数据保存在浏览器端)
     2. 服务器端Session(数据保存在服务器端)

​    

## 如何查看Cookie的数据

### 目标

如何在不同的浏览器中查看Cookie的信息

### 什么是Cookie

本质上就是保存在浏览器上键值对

1. 保存在浏览器端的数据，键和值都是字符串类型。
2. 每个Cookie对象只能保存一个键和值。
3. 每个Cookie的大小最多不超过4K。

### 应用场景

将用户名和密码保存在Cookie中，下次访问的时候读取Cookie中的数据，发送给服务器，由服务器进行验证。如果用户名和密码正确就实现自动登录

![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552693881764.png) 

### 如何查看Cookie信息

#### 在Chrome和Edge中查看

![1572313109125](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572313109125.png) 

#### 在IE中

```
C:\Users\用户名\AppData\Local\Microsoft\Windows\INetCookies\
```

![1572313176361](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572313176361.png) 

#### 在Firefox中

![1572313340984](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572313340984.png) 

#### 疑问：不同浏览器之间的Cookie数据能不能共享？

```
不能
不同的浏览器，服务器会认为是不同的用户。
但同一个浏览器，不同的标签页，是同一个用户
```



### 小结

1. Chrome中如何查看Cookie
2. IE浏览器中如何查看Cookie



## Cookie的执行原理

### 目标

1. 什么是Cookie

2. Cookie的执行原理

### 什么是Cookie

特点：保存在浏览器端的键值对，一个Cookie保存1个键和值，都是字符串类型

大小：最多不超过4K

### Cookie运行的原理

![1572313785092](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572313785092.png)



### 小结

Cookie的数据结构是什么样的？

```
一个键值对
```





## <font color="red">Cookie的创建和写入</font>

### 目标

1. 如何创建Cookie

2. 如何将创建的Cookie写入到浏览器端

### 创建和写入的方法

| Cookie类的方法                   | 作用                           |
| -------------------------------- | ------------------------------ |
| Cookie(String name,String value) | 创建一个Cookie对象，指定键和值 |

| HttpServletResponse对象的方法 | 作用                                 |
| ----------------------------- | ------------------------------------ |
| addCookie(Cookie cookie)      | 将Cookie的键和值通过响应发送给浏览器 |



### 创建Cookie的案例演示

#### 需求：

在Servlet中创建一个Cookie("user","NewBoy")，并且写到浏览器端去。

#### 运行效果

![1572315074280](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572315074280.png) 



#### 代码

``` java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 创建一个Cookie写到浏览器端
 */
@WebServlet("/demo1")
public class Demo1WriteServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("向浏览器写入了一个Cookie的键和值");

        //1. 创建Cookie对象
        Cookie man = new Cookie("user", "NewBoy");
        //2. 使用响应对象将Cookie写到浏览器端
        response.addCookie(man);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### 查询响应头

Set-Cookie: user=NewBoy

### 小结

1. 如何创建Cookie:  new Cookie(键,值)
2. 如何写入Cookie： response.addCookie(cookie对象)



## 设置Cookie过期的时间

### 目标

如何设置Cookie的过期时间

### 设置Cookie过期时间的方法

默认情况下不会永久保存，浏览器关闭就失效

| Cookie的方法               | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| void setMaxAge(int expiry) | 设置Cookie过期的时间，单位是：秒<br />正数：以秒为单位的过期时间<br />零：表示删除Cookie<br />负数：无效(与不设置是一样的) |

### 案例：设置Cookie的过期时间

#### 需求：

在写入Cookie之前先设置Cookie过期的时间，设置为10分钟以后过期

#### 代码

```java 
//1. 创建Cookie对象
Cookie man = new Cookie("user", "NewBoy");
//2. 设置Cookie过期的时间，设置为10分钟以后过期
man.setMaxAge(60 * 10);
//3. 使用响应对象将Cookie写到浏览器端
response.addCookie(man);
```

#### 查询响应头和浏览器的信息

![1572315413840](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572315413840.png) 

Set-Cookie: user=NewBoy;Max-Age=600

### 小结

1. 如何设置Cookie过期的时间？  setMaxAge(秒)

2. 参数是正，负，0分别表示什么意思？



## <font color="red">读取Cookie的信息</font>

### 目标

如何从浏览器读取Cookie的信息到服务器

### 读取Cookie的方法

| HttpServletRequest对象 | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Cookie[] getCookies()  | 读取浏览器端所有发送过来的Cookie信息，封装成Cookie对象数组<br />如果没有Cookie，返回NULL |

| Cookie的方法      | 作用           |
| ----------------- | -------------- |
| String getName()  | 得到Cookie的键 |
| String getValue() | 得到Cookie的值 |

### 演示案例

#### 需求

1. 修改写入的Servlet，创建两个Cookie

   ![1572315772874](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572315772874.png) 

2. 创建Servlet读取所有Cookie信息，显示在浏览器上。



#### 代码

``` java

```

#### 执行效果

![1572316034844](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572316034844.png) 

#### 浏览器查看请求头

Cookie: user=NewBoy; age=18



### 小结

| HttpServletRequest对象 | 作用                                 |
| ---------------------- | ------------------------------------ |
| Cookie[ ] getCookies() | 读取浏览器发送过来的所有的Cookie信息 |

| Cookie类的方法    | 作用           |
| ----------------- | -------------- |
| String getName()  | 得到Cookie的键 |
| String getValue() | 得到Cookie的值 |



## Cookie中使用特殊字符

### 目标

Cookie中使用特殊字符的情况

### 特殊字符

<font color="red">注：在Tomcat8中cookie值中不能使用分号(;)、逗号(,)、等号(=)以及空格，可以直接使用汉字。</font>

![1572316240621](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572316240621.png)

### 使用特殊字符步骤

1. 在创建cookie之前使用URL将据编码
2. 将编码后的数据存入cookie
3. 读取时，获取到cookie之后，解码数据，显示正常的数据

### URL编码和解码的方法

![1552695168788](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552695168788.png)

### 案例：使用特殊字符

#### 需求

1. 向Cookie中写入一个字符串"Hello World"
2. 再读取出来显示到浏览器上

#### 步骤

1. 创建写入的Servlet
   1. 写入前URL编码
   2. 创建Cookie
   3. 设置过期的时间
   4. 写入Cookie
2. 创建读取的Servlet
   1. 得到浏览器端所有的Cookie信息
   2. 如果没有Cookie返回的空
   3. 如果有则遍历，读取其中的键和值
   4. 值需要使用URL解码后输出

#### 代码

##### 写入

```java
//1. 创建Cookie对象
Cookie man = new Cookie("user", URLEncoder.encode("New Boy","utf-8"));   //使用URL的编码
//2. 设置Cookie过期的时间，设置为10分钟以后过期
man.setMaxAge(60 * 10);
//3. 使用响应对象将Cookie写到浏览器端
response.addCookie(man);
```

##### 读取

```java
//2. 判断数组是否为空
if (cookies != null) {
    //3. 如果不为空，对数组进行遍历，输出每个键和值
    for (Cookie cookie : cookies) {
        String name = cookie.getName();
        String value = URLDecoder.decode(cookie.getValue(), "utf-8");
        out.print("Cookie的键：" + name + "，值：" + value + "<hr/>");
    }
}
```

#### 效果

![1572316495213](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572316495213.png) 

### 小结

1. 编码的方法是什么？ URLEncoder.encode()
2. 解码的方法是什么？ URLDecoder.decode()



## 创建Cookie的工具类

### 目标

编写一个工具类，用于读取Cookie指定键的值。如果键不存在，则返回null

### 代码

```java
package com.itheima.utils;

import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;

/**
 * Cookie的工具类
 */
public class CookieUtils {

    /**
     * 用于读取Cookie指定键的值
     * @param request 请求对象
     * @param name 要找的键
     * @return 如果键不存在，则返回null
     */
    public static String getCookieValue(HttpServletRequest request, String name) {
        //调用方法得到Cookie对象的数组
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            //遍历数组
            for (Cookie cookie : cookies) {
                //表示找到
                if (cookie.getName().equals(name)) {
                    return cookie.getValue();
                }
            }
        }
        //没有找到，返回null
        return null;
    }

}

```

使用工具类

```java
package com.itheima.servlet;

import com.itheima.utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
使用工具类
 */
@WebServlet("/demo3")
public class Demo3UseUtilsServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //输出age的cookie的值
        out.print("年龄：" + CookieUtils.getCookieValue(request, "age"));
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### 小结

1. 调用请求的方法得到所有的Cookie对象数组
2. 判断是否为空，不为空则遍历
3. 比较每个键是否与查找的名字相同，如果找到就返回它的值
4. 否则就返回NULL





## 设置Cookie的访问路径

### 目标

1. 如何设置Cookie的访问路径？
2. 设置访问路径

### 设置路径的方法

| Cookie设置路径的方法 | 功能                                                         |
| -------------------- | ------------------------------------------------------------ |
| cookie.setPath(路径) | 限制Cookie发送的访问路径，只有访问这个地址或它的子路径<br />浏览器才会将Cookie的信息发送到服务器 |

### 设置路径的案例

#### 需求

创建一个Cookie，设置过期时间，设置Cookie的访问路径

#### 效果

![1572317940094](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572317940094.png)  

#### 代码

``` java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 设置Cookie的访问路径
 */
@WebServlet("/demo4")
public class Demo4PathServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("写入一个Cookie");

        //1.创建一个Cookie对象
        Cookie cookie = new Cookie("woman", "林志玲");

        //2. 设置访问路径(默认是当前项目的访问地址)
        cookie.setPath(request.getContextPath() + "/aaa");

        //3. 写入到浏览器
        response.addCookie(cookie);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

访问的时候

```java
package com.itheima.servlet;

import com.itheima.utils.CookieUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

/*
使用工具类
 */
@WebServlet("/aaa/demo3")
public class Demo3UseUtilsServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //输出age的cookie的值
        out.print("Cookie：" + CookieUtils.getCookieValue(request, "woman"));
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



### 小结

如果访问路径是setPath(“/day-cookie”)，以下路径浏览器会发送Cookie给服务器？

| 访问地址                               | 会不会发送Cookie给服务器 |
| -------------------------------------- | ------------------------ |
| http://localhost:8080/day-cookie/      | 会 (正好匹配)            |
| http://localhost:8080/day-cookie/aa/bb | 会 (访问子路径)          |
| http://localhost:8080/day/             | 不会 (不匹配)            |
| http://localhost:8080/                 | 不会 (父路径)            |

 

## 删除Cookie

### 目标

​	学习删除Cookie的方法

### 方法

| Cookie的删除 | 说明                          |
| ------------ | ----------------------------- |
| setMaxAge(0) | 将过期的时间设置为0，表示删除 |

### 案例：删除指定的Cookie

#### 需求

删除指定的Cookie信息，注意Cookie的访问路径要相同

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo5")
public class Demo5DeleteServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("删除1个Cookie");

        //1. 创建一个Cookie
        Cookie cookie = new Cookie("user", null);
        //2. 设置过期时间为0
        cookie.setMaxAge(0);
        //3. 写入到浏览器(保证访问地址相同)
        response.addCookie(cookie);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 小结

删除Cookie使用哪个方法？ setMaxAge(0)



## 案例：使用Cookie实现自动登录

### 目标

能够使用Cookie保存用户名和密码

### 结构

![1568899204362](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1568899204362.png) 

### 分析

![1572319131509](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572319131509.png)

### Servlet步骤

1. 得到表单提交的数据：用户名和密码
2. 判断用户名和密码实现登录
3. 如果登录成功，则判断是否勾选了记住我。
4. 如果勾选了，则将用户名和密码写入Cookie中，保存一周的时间，并跳转到成功页面。
5. 否则跳转到登录失败页面

### JS代码步骤

1. 编写工具方法，读取指定键的值
2. 页面加载完成，读取cookie的信息
3. 如果有username和password
4. 则将用户名和密码赋值给文件框，并且提交表单

### 代码

login.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>用户登录</title>
    <!--导入js文件-->
    <script src="js/commons.js"></script>
</head>
<body>
<h2>用户登录</h2>
<form action="login" method="post" id="loginForm">
    <table>
        <tr>
            <td width="60">用户名</td>
            <td><input type="text" name="username" id="username"/></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password" id="password"/></td>
        </tr>
        <tr>
            <td>记住我</td>
            <!--没有value属性的前提下，点中它的值是on-->
            <td><input type="checkbox" name="remember"/></td>
        </tr>
        <tr>
            <td colspan="2" align="center"><input type="submit" value="登录"/></td>
        </tr>
    </table>
</form>
</body>
</html>
```

commons.js

```javascript
/*
在页面加载完毕访问cookie的数据
 */
window.onload = function () {
    //得到浏览器端所有的Cookie数据
    var username = getCookie("username");
    var password = getCookie("password");
    //开始自动登录
    if (username != null && password != null) {
        //将用户名和密码放在表单中，提交表单登录
        document.getElementById("username").value = username;
        document.getElementById("password").value = password;
        //提交表单
        document.getElementById("loginForm").submit();
    }
};

/*
通过名字得到值
 */
function getCookie(name) {
    //1.得到所有的Cookie字符串
    var cookie = document.cookie;
    if (cookie != "") {
        //2. 使用分号来拆分成一个数组
        var arr = cookie.split(";");
        //3. 遍历数组，将每个元素使用等于来拆分
        for (var i = 0; i < arr.length; i++) {
            var e = arr[i];  //每个元素
            var nameAndValue = e.split("=");  //又得到一个数组
            var cname = nameAndValue[0].trim();   //名字
            var cvalue = nameAndValue[1].trim();   //值
            //4. 判断名字是否等于第0个元素，如果相等就返回第1个元素
            if (name == cname) {
                return cvalue;
            }
        }
    }
    //5. 如果没有找到就返回null
    return null;
}
```

LoginServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 得到用户名和密码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //2. 判断是否登录成功
        if ("admin".equals(username) && "123".equals(password)) {
            //2.2 判断是否勾了"记住"
            String remember = request.getParameter("remember");
            //没有勾就是null，如果勾了就是on
            if (remember != null) {
                //保存用户名和密码
                //创建2个Cookie
                Cookie cookieUser = new Cookie("username", username);
                //设置访问地址为login.html
                cookieUser.setPath(request.getContextPath() + "/login.html");
                //设置过期的时间为一周
                cookieUser.setMaxAge(60 * 60 * 24 * 7);
                response.addCookie(cookieUser);

                Cookie cookiePassword = new Cookie("password", password);
                //设置访问地址为login.html
                cookiePassword.setPath(request.getContextPath() + "/login.html");
                //设置过期的时间为一周
                cookiePassword.setMaxAge(60 * 60 * 24 * 7);
                response.addCookie(cookiePassword);
            }
            //3. 如果登录成功，跳转到success
            response.sendRedirect("success.html");
        }
        else {
            //4. 否则跳转到failure页面
            response.sendRedirect("failure.html");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 小结

1. 服务器的Java代码判断用户名和密码		
2. 服务器发送Cookie信息给浏览器保存		
3. 浏览器使用JavaScript去发送Cookie的数据给服务器	



## HttpSession对象的方法

### 目标

1. 什么是HttpSession
2. 如何得到会话对象
3. 学习HttpSession接口中常用的方法

### Session存储数据的特点

1. 数据是保存在服务器端的内存中
2. 底层是Map结构，也是一个作用域对象。键是字符串类型，键是Object对象。
3. 每个用户对应一个会话对象，用户之间的数据不能共享。每个用户都有一个会话ID，通过这个会话ID去找自己的会话空间。

#### 类似于超市的储物柜

![1552711348283](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552711348283.png)



#### Session和Cookie的主要区别

1. Cookie：
   1. 键和值都是字符串类型
   2. 数据保存在浏览器端硬盘中
   3. 一个Cookie只能保存一个键和值
2. Session
   1. 键是字符串，值是Object
   2. 数据是保存在服务器的内存中
   3. 一个Session可以保存多个键和值

### HttpSession接口的方法

#### 得到会话对象的方法

| HttpServletRequest创建会话的方法 | 描述         |
| -------------------------------- | ------------ |
| HttpSession request.getSession() | 得到一个会话 |

创建会话的时机：用户第1次访问的时候创建一个会话，并且分配会话的ID给用户。第2次以后访问，不再创建新的会话，而是返回以前创建好的会话。



### 案例：会话接口中方法的演示

#### 步骤

1. 得到会话对象
2. 使用会话对象使用上面的各个方法

#### 效果

![1572321820138](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572321820138.png)  

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Timestamp;

@WebServlet("/demo1")
public class Demo1MethodServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        HttpSession session = request.getSession();

        out.print("得到会话对象：" + session + "<hr/>");
        out.print("会话ID: " +  session.getId() + "<hr/>");
        out.print("会话创建的时间：" + new Timestamp(session.getCreationTime()) + "<hr/>");
        out.print("会话上次访问的时间：" + new Timestamp(session.getLastAccessedTime()) + "<hr/>");
        out.print("是否新的会话：" + session.isNew() + "<hr/>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

| HttpSession接口方法    | 作用               |
| ---------------------- | ------------------ |
| String getId()         | 得到会话的ID       |
| long getCreationTime() | 得到创建会话的时间 |
| boolean isNew()        | 判断会话是否新的   |



## 上午的回顾

### 会话技术

两种会话技术：

1. 浏览器端技术：Cookie
2. 服务器端技术：Session

区别：

1. Cookie：
   1. 数据保存在浏览器的文件中
   2. 一个Cookie保存一个键值对，键和值都是字符串类型
2. Session：
   1. 数据保存在服务器的内存中
   2. 底层Map结构，可以保存多个键值对，键是String，值是Object



### Cookie中一些方法

1. 创建：new Cookie(键,值)
2. 写入：response.addCookie(Cookie对象)
3. 设置过期时间：setMaxAge(秒)
4. 读取Cookie的信息：Cookie[] request.getCookies()
5. 设置访问路径：setPath() 
6. 删除Cookie： setMaxAge(0)



### HttpSession对象中方法

1. 获取会话对象： request.getSession() 
2. 获取会话ID： session.getId()
3. 获取会话创建的时间：getCreationTime()
4. 获取上次访问的时间：getLastAccessedTime()
5. 判断会话是否是新的：isNew()



## <font color="red">会话域的操作方法</font>

### 目标

与会话域有关的操作方法

### 讲解

#### 会话域中的方法

会话范围的范围：一个用户所有的请求，包含了多次请求。

请求域 < 会话域 < 上下文域

- 创建：用户第一次访问
- 销毁：会话结束(浏览器关闭或服务器上会话过期)

![1552450034543](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552450034543.png)

### 演示案例

#### 需求

​	通过1个案例学习上面方法的使用

#### 执行效果

1. 先执行向会话域中添加值的Servlet

![1552450585063](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552450585063.png) 

2. 再执行从会话域中取值的Servlet

![1552450653128](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552450653128.png) 

3. 另一个浏览器执行的效果

![1552450684828](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1552450684828.png) 



#### 代码

##### SetServlet.java

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo2")
public class Demo2SetServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("向会话域中添加商品");

        //1.得到会话对象
        HttpSession session = request.getSession();
        //2. 向会话域中添加键和值
        session.setAttribute("product","洗衣机");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

##### GetServlet.java

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo3")
public class Demo3GetServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        //1. 得到会话对象(之前相同的对象)
        HttpSession session = request.getSession();
        //2. 从会话域中取出值
        String product = (String) session.getAttribute("product");
        //3. 输出到网页
        out.print("从会话域中取出：" + product);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### 小结：课堂提问

为什么另一个浏览器得不到放在会话域中的值？

```
因为不是同一个会话，不同会话之间的数据不能共享
```

 

## 会话访问的原理

### 目标

1. 学习Session的会话原理
2. 保存会话ID到Cookie中

### 原理分析

#### 查看SetServlet中响应头

```
Set-Cookie: JSESSIONID=F159453127D2FDB2263649920CA99E7B
```

服务器通过Cookie将会话ID发送给了浏览器



#### 查看GetServlet的请求头

```
Cookie: JSESSIONID=F159453127D2FDB2263649920CA99E7B
```

再次请求的时候，又使用Cookie将会话ID发送给了服务器



### 原理图

![1572332210853](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572332210853.png)



### 疑问

1. 如果浏览器关闭，服务器上的会话信息是否还存在？

   ```
   还存在，浏览器关闭不会影响服务器
   ```

   

2. 浏览器关闭以后，还能不能得到之前会话域中的信息？

   ```
   默认是不能的，因为Cookie在浏览器关闭以后就失效了，会话ID就失效了。
   相当于储物箱中的东西还在，但是密码条丢失了
   ```

   

3. 有没有可能让浏览器关闭还可以再访问服务器上没有过期的信息？

   ```
   可以，将Cookie信息会话ID，永久保存。
   ```

   

### 演示案例

#### 步骤

1. 创建会话对象
2. 得到会话的ID
3. 创建一个Cookie，将它的名字设置为JSESSIONID，值为上面的会话ID
4. 设置过期时间为30分钟
5. 写入到浏览器

#### 代码

``` java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo2")
public class Demo2SetServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("向会话域中添加商品");

        //1.得到会话对象
        HttpSession session = request.getSession();
        //2. 向会话域中添加键和值
        session.setAttribute("product","洗衣机");

        //将会话ID存下来
        Cookie cookie = new Cookie("JSESSIONID", session.getId());
        //设置过期的时间，半小时
        cookie.setMaxAge(60 * 30);
        //写到浏览器
        response.addCookie(cookie);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### 效果

![1572332704772](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572332704772.png) 

### 小结

服务器与浏览器之间是如何将会话ID发送给对方的

```
通过Cookie
```



## 修改服务器会话过期时间

### 目标

​	学习设置会话过期的三种方式

### 查看会话过期的时间

- 疑问：Session在服务器上默认的销毁时间是多久？如何查看？

### 修改会话过期时间的三种方式

#### 方式一：通过代码

```java
session.setMaxInactiveInterval(秒);
```

##### 示例：设置会话过期的时间为10秒，输出过期的时间，输出会话ID 

##### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo4")
public class Demo4IntervalServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        //得到会话对象
        HttpSession session = request.getSession();
        out.print("当前会话的ID: " + session.getId() + "<hr/>");

        //通过代码修改会话过期时间
        session.setMaxInactiveInterval(10);
        //获取最大的非活动时间间隔，单位是秒。间隔：只要在这段时间内访问一次服务器，服务器就重新开始计时
        out.print("会话过期的时间：" + session.getMaxInactiveInterval() + "秒<hr/>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



#### 方式二：可以通过web.xml的配置来实现

```xml
<!--配置会话过期的时间-->
<session-config>
    <!--单位是分钟-->
    <session-timeout>5</session-timeout>
</session-config>
```

疑问：如果同时使用代码和配置的方式以哪个为准？

```
就近原则，后面的覆盖前面的，以代码为准
```



#### 方式三：立刻失效

```java
通常用在用户退出的功能中
session.invalidate();
```




### 小结

修改会话过期的时间有哪三种方式？

1. 使用代码：setMaxInactiveInterval(秒)

2. 使用配置文件，修改web.xml

   ```xml
   <session-config>
   	<session-timeout>分钟</session-timeout>
   </session-config>
   ```

3. 立刻失效

   ```
   session.invalidate()
   ```

   



## 会话的钝化与激活技术

### 目标

了解服务器会话的钝化和激活技术

### 提问

判断下面的说法是否正确？

1. 浏览器关闭的时候，服务器端的会话就销毁？ 

   ```
   错
   ```

   

2. 服务器关闭的时候，服务端的会话就销毁？ 

   ```
   错，如果正常关闭服务器，tomcat会将会话域中对象通过序列化的方式写到硬盘文件中
   下次服务器再启动的时候，通过反序列化将硬盘上文件读取到内存中。
   ```

   

### 回顾

序列化和反序列化

```java
package com.itheima.entity;

import java.io.Serializable;

/**
 * 商品必须实现序列化的接口
 */
public class Product implements Serializable {

    private String name;
    private double price;

    @Override
    public String toString() {
        return "Product{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    public Product() {
    }

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}

```



```java
package com.itheima.ser;

import com.itheima.entity.Product;
import org.junit.Test;

import java.io.*;

public class TestSerializable {

    /*
    将内存中对象写到硬盘上，序列化
     */
    @Test
    public void testWrite() throws IOException {
        //创建文件输出流
        FileOutputStream outputStream = new FileOutputStream("d:/session.ser");
        //创建对象输出流
        ObjectOutputStream oos = new ObjectOutputStream(outputStream);
        //写入对象
        oos.writeObject(new Product("充气娃娃",3000d));
        //关闭流
        oos.close();
    }

    @Test
    public void testRead() throws IOException, ClassNotFoundException {
        //创建文件输入流
        FileInputStream fis = new FileInputStream("d:/session.ser");
        //创建对象输入流
        ObjectInputStream ois = new ObjectInputStream(fis);
        //从文件中读取对象
        Product o = (Product) ois.readObject();
        ois.close();
        
        System.out.println(o);
    }

}
```



### 会话的钝化

注：项目要部署在tomcat中才有效果，在idea中没有效果

```
在work目录下：
C:\apache-tomcat-8.5.5\work\Catalina\localhost\day27_03_session\SESSIONS.ser
```

### 会话的激活

下次启动服务器，会从文件中读取，这个文件会被删除              

### 效果

![1572335455443](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572335455443.png) 

### 小结

1. 什么是会话的钝化与激活？

   ```
   序列化和反序列化，这个由tomcat去完成
   ```

   

2. 钝化的对象需要实现哪个接口？

   ```
   必须实现在序列化的接口
   ```

   



## URL重写：禁用Cookie的处理[了解]

### 目标

1. Cookie禁用后重写向的处理
2. Cookie禁用后超连接的处理

### 禁用Cookie后会话中共享数据导致的问题

1. 把浏览器的Cookie禁用
2. 在一个Servlet1中向session中添加一个数据，重定向到Servlet2。
3. 在另一个Servlet2中的session中读取数据并显示出来
4. 无法得到原来会话中的信息

#### 查看第一次的响应头和第二次的请求头

JSESSIONID=C8AEF0083D114A45C7F0551D9C64F02B

JSESSIONID=C20461987DEA3DF37056470D90EF397B

### 重定向的解决方法

1. 什么是URL重写：重新生成URL，将会话的ID放在URL的后面提交给服务器，从而实现会话跟踪技术。


2. 重定向的解决方法：

| 方法                             | 说明                       |
| -------------------------------- | -------------------------- |
| response.encodeRedirectURL(path) | 对要跳转过去的地址进行重写 |

重写前的URL：

```
http://localhost:8080/day27_03_session_war_exploded/demo3
```

重写后的URL：     

```
http://localhost:8080/day27_03_session_war_exploded/demo3;jsessionid=F46334EF0FC9FD3DFCD0C3C7A7B60382
```

​              

#### 代码

```java
package com.itheima.servlet;

import com.itheima.entity.Product;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo2")
public class Demo2SetServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        out.print("向会话域中添加商品");

        //1.得到会话对象
        HttpSession session = request.getSession();
        session.setAttribute("product", new Product("充气娃娃",3000d));

        //URL的重写，将会话ID放在URL的后面
        String url = response.encodeRedirectURL("demo3");
        //重定向
        response.sendRedirect(url);

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```



### 超链接的解决方法 

| 方法                     | 说明                                              |
| ------------------------ | ------------------------------------------------- |
| response.encodeURL(path) | 对链接的地址进行URL的重写，将会话的ID放在地址后面 |

#### 步骤

1.  在servlet1中输出一个链接，跳转到servlet2中。
2.  使用上面的方法对href的地址进行URL重写，再设置href属性
3.  查看跳转的链接地址

#### 代码

```java
//超链接的重写
String url = response.encodeURL("demo3");
out.print("<a href='" + url + "'>跳转到demo3</a>");
```

### 两个方法的区别

1. 共同点：都可以重写URL，前提是Cookie被禁用。如果Cookie没有禁用，这2个方法是不起作用的。

2. 不同点：encodeURL功能上更强的，可以修改不正确访问地址。

   ```
   System.out.println( response.encodeRedirectURL(""));
   System.out.println( response.encodeURL(""));
   ```

   

### 思考

使用URL重写以后，不同的浏览器可不可以访问其它浏览器的会话数据？

```
可以，因为会话ID放在地址栏后面
```

### 小结

| 方法                             | 说明                |
| -------------------------------- | ------------------- |
| response.encodeRedirectURL(path) | 用于重定向的URL重写 |
| response.encodeURL(path)         | 用于链接的URL重写   |



## Servlet三个作用域总结

### 作用域的创建与销毁

| 作用域   | 接口名             | 作用范围           | 生命周期         |
| -------- | ------------------ | ------------------ | ---------------- |
| 请求域   | HttpServletRequest | 一个用户的一次请求 | 一次请求结束     |
| 会话域   | HttpSession        | 一个用户的所有请求 | 服务器会话过期   |
| 上下文域 | ServletContext     | 所有用户的所有请求 | 服务器关闭或重启 |

### 三个作用域共同的方法

| 功能     | 方法                      |
| -------- | ------------------------- |
| 存放数据 | setAttribute("键",值)     |
| 获取数据 | Object getAttribute("键") |
| 删除数据 | removeAttribute("键")     |

### 如何选择使用哪个作用域

原则：在满足功能的前提下，使用尽可能小的作用域。比较节省服务器资源。



## 案例part1：登录验证码的准备

### 目标

登录验证码环境的搭建

### 需求：用户登录的时候使用验证码进行验证

​	登录成功后将用户信息保存到会话域中，并且跳转到WelcomeServlet，然后在WeclcomeServlet中读取用户信息，显示欢迎信息。在WelcomeServlet上显示退出的链接，点退出，注销会话信息。

### 步骤

1. 复制登录的原型login.html，添加name属性
2. 复制昨天已经写好的验证码，修改验证码代码。将字符串保存在会话域中
3. 搭建整个项目，今天不访问数据库。

### 登录页面

```html
<!--指定为servlet，鼠标移上去变成手-->
<img src="code" style="cursor: pointer" title="看不清换一张" id="picCode"/>

<script type="text/javascript">
	//编写图片的点击事件
	document.getElementById("picCode").onclick = function () {
		//修改图片的src，并且指定一个变化的参数
		this.src = "code?n=" + new Date().getTime();
	};
</script>	
```

### 验证码的Servlet

```java
package com.itheima.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
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

        StringBuilder sb = new StringBuilder();
        //随机取4次字符
        for (int i = 0; i < 4; i++) {
            //随机得到0~长度-1 的索引号
            int index = ran.nextInt(arr.length);
            char c = arr[index];
            sb.append(c);  //添加到StringBuilder中
            //随机得到一种颜色
            graphics.setColor(getRanColor());
            //画这个字符：字符串，x和y的坐标   10+(i*20), 20
            graphics.drawString(String.valueOf(c), 10 + (i * 20), 20);
        }

        //把字符串放在会话域中
        HttpSession session = request.getSession();
        session.setAttribute("code", sb.toString());
        System.out.println("验证码：" + sb.toString());

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



## 案例part2：登录的Servlet

### 目标

实现登录的Servlet

### 流程分析

![1572337333452](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1572337333452.png)

### 步骤

1. 使用昨天的代码实现验证码的绘制
2. 将随机产生的字符串放在会话域中
3. 用户登录的时候提交验证码的字符串
4. 比较表单提交的字符串是否与会话域中的字符串相等，如果相等则验证成功
5. 登录一次以后删除会话域中的验证码字符串
6. 登录成功以后保存用户的信息到会话域中，并且跳转到WelcomeServlet

### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();

        // 1. 判断验证码是否正确
        //1.1 得到会话域中验证码
        HttpSession session = request.getSession();
        String code = (String) session.getAttribute("code");
        //把验证码删除
        session.removeAttribute("code");

        //1.2 得到用户提交的验证码
        String vcode = request.getParameter("vcode");

        //1.3 忽略大小写比较是否相等
        if (vcode.equalsIgnoreCase(code)) {
            // 2. 再去判断用户名和密码是否正确
            //2.1 得到用户名和密码
            String username = request.getParameter("username");
            String password = request.getParameter("password");
            //2.2 判断用户名和密码是否正确
            if ("admin".equals(username) && "123".equals(password)) {
                // 3. 将用户的信息放在会话域中
                session.setAttribute("user", username);
                // 4. 跳转到WelcomeServlet中
                response.sendRedirect("welcome");
            }
            else {
                out.print("<script>");
                out.print("alert('用户名或密码错误');");
                out.print("location.href='login.html';");
                out.print("</script>");
            }
        } else {
            out.print("<script>");
            out.print("alert('验证码错误');");
            out.print("location.href='login.html';");
            out.print("</script>");
            //不能使用重定向，否则看不到这个Servlet中内容
            //response.sendRedirect("login.html");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



## 案例part3：欢迎Servlet和注销的Servlet

### 目标

1. 在欢迎的Servlet上显示用户的信息
2. 点退出链接，用户注销
3. 如果没有正确登录的用户，不能访问WelcomeServlet

### 步骤

1. 从会话域中取出用户信息并且显示
2. 判断用户是否正常登录，如果是非法用户则跳转到登录页面
3. 在页面上输出一个注销的连接，点注销跳转到LogoutServlet
4. 注销：调用invalidate()，重定向到login.html

### 代码

#### WelcomeServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/welcome")
public class WelcomeServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.得到会话对象
        HttpSession session = request.getSession();

        //2. 从会话域中取出用户名字
        String user = (String) session.getAttribute("user");
        //如果会话域中有用户的信息，表示已经登录，否则跳转到登录页面
        if (user == null) {
            response.sendRedirect("login.html");  //重定向后，后面的代码还会执行
            return;
        }
        //3. 显示欢迎信息
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print("<h2>欢迎您，" + user + "，登录成功</h2>");
        //4. 显示退出链接
        out.print("<a href='logout'>退出</a>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### LogoutServlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.得到会话
        HttpSession session = request.getSession();
        //2. 让会话失效
        session.invalidate();
        //3. 重定向到登录页面
        response.sendRedirect("login.html");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

### 小结

1. 验证码放在会话域中
2. 登录的时候先从会话域中取出验证码，再判断用户名和密码
3. 如果登录成功跳转到欢迎页面，并从会话域中取出用户信息显示
4. 登录失败跳转到登录页面
5. 点退出注销会话，跳转到登录页面



# 学习总结

1. 能够说出会话的概念

   创建：用户第一次访问的时候，由服务器创建，并且会分配会话ID

   过程：用户多次发送请求给服务器，服务器做出响应，就是会话过程。

   结束：浏览器关闭或服务器上会话过期

2. 能够说出cookie的概念

   1) 保存在: 浏览器端的硬盘中

   2) 键和值都是: 字符串类型，一个Cookie只能保存一个键和值

   3) 默认浏览器关闭: Cookie就失效

3. 能够创建、发送、接收、删除cookie

   | 方法名                            | 作用                           |
   | --------------------------------- | ------------------------------ |
   | new Cookie(名字，值)              | 创建一个Cookie对象             |
   | response.addCookie(Cookie cookie) | 将Cookie对象写到浏览器端       |
   | Cookie[] request.getCookies()     | 获取所有的浏览器端的Cookie信息 |
   | setMaxAge(0)                      | 删除Cookie                     |

   

4. 能够说出cookie执行原理

   ![1544243690839](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/cookie&HTTPSession/1544243690839.png)

5. 能够说出session的概念

   1) 保存在: 服务器的内存中

   2) 键和值：键是String，值是Object

   3) 默认过期：30分钟过程

6. 能够获取session对象、添加、删除、获取session中的数据

   | 方法名                         | 作用             |
   | ------------------------------ | ---------------- |
   | request.getSession()           | 获取一个会话对象 |
   | session.setAttribute("键", 值) | 添加             |
   | session.getAttribute("键")     | 获取             |
   | session.removeAttribute("键")  | 删除             |

7. 能够完成登录验证码案例

