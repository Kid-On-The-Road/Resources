# 《JSP&El、JSTL、MVC上课笔记》

# 回顾

## 与Cookie操作相关的方法

| Cookie类的方法                    | 作用                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| Cookie(String  name,String value) | 创建一个Cookie对象，参数是：键，值                           |
| void setMaxAge(int   expiry)      | 添加过期时间<br />正：以秒为单位<br />零：删除Cookie<br />负：无效 |

 

## 服务器写入到浏览器端的Cookie方法

| response的方法        |
| --------------------- |
| addCookie(Cookie对象) |

 

## 浏览器读取到服务器的Cookie的方法

| request的方法         |
| --------------------- |
| Cookie[] getCookies() |

 

## 得到Session对象的方法

| request的方法 |
| ------------- |
| getSession()  |

 

## HttpSession类的API

| HttpSession接口方法            | 作用                           |
| ------------------------------ | ------------------------------ |
| String   getId()               | 得到会话的ID                   |
| void invalidate()              | 会话失效                       |
| void  setMaxInactiveInterval() | 设置过期的最大时间，单位是秒。 |

 

## 三个作用域的范围

| 作用域   | 范围               | 销毁的时机       |
| -------- | ------------------ | ---------------- |
| 请求域   | 一个用户的一次请求 | 一次请求结束     |
| 会话域   | 一个用户的所有请求 | 会话过期         |
| 上下文域 | 所有用户的所有请求 | 服务器关闭或重启 |



#  学习目标

1. JSP
   1. 能够说出jsp的优势
   2. 能够编写jsp代码片段、声明、脚本表达式
2. EL
   1. 能够说出el表达式的作用
   2. <font color="red">能够使用el表达式获取javabean的属性</font>
3. JSTL
   1. 能够使用jstl标签库的if标签
   2. <font color="red">能够使用jstl标签库的foreach标签</font>
   3. 能够使用jstl标签库的choose标签
4. MVC
   1. 能够说出开发模式的作用
   2. <font color="red">能够使用三层架构模式完成显示用户案例</font>



# 学习内容

## JSP的优势

### 目标

1. 了解JSP的特点
2. 体验第1个JSP的编写

### 概念

JSP：Java Server Page 运行在服务器端的Java代码，用来制作动态网页的。看起来像个HTML页面，但是又可以在HTML中写Java代码。

### 各种技术的特点

| 技术    | 特点                                                         |
| ------- | ------------------------------------------------------------ |
| HTML    | 制作静态网页，优：方便编写JavaScript和CSS代码。 缺：不能编写动态的内容 |
| Servlet | 制作动态网页，优：可以编写动态的内容，缺：不方便编写JavaScript和CSS |
| JSP     | JSP=HTML+Servlet，同时有这2者的优点。既方便编写CSS代码，又可以编写动态的内容 |

JSP用于同步开发，服务器与浏览器是串行工作，同时只能有一个在工作。

### JSP的第1个案例

#### 需求

在浏览器上输出服务器当前的时间，并且设置样式

#### 运行效果

![1572485818094](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572485818094.png) 

#### 代码

```jsp
<%@ page import="java.util.Date" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>JSP</title>
    <style>
        h1 {
            background-color: yellow;
            color: red;
            border: 1px dashed black;
        }
    </style>
</head>
<body>
<%-- 输出现在的时间(服务器端)，并且对时间设置样式 --%>
<h1>
    <%--JSP代码片段：用于编写Java代码 --%>
    <%
        Date date = new Date();
        String format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(date);
        /*
        是JSP的内置对象(JspWriter)，不用声明可以直接使用，在JSP中有9个内置对象可以直接使用，常用的是：
        out, request, response ,session, application(上下文对象)
         */
        out.print(format);
    %>
</h1>
</body>
</html>
```

### 小结

  1. JSP上可以同时编写： 静态和动态内容

  2. Java代码写在：

     ```
     <% %>
     ```

  3. 在浏览器上能否看到Java代码？

     ```
     不能看到Java代码，只能看到Java代码运行的结果
     ```

     



## JSP与Servlet之间的关系

### 目标

了解JSP与Servlet之间是什么关系

### JSP的执行过程

![1544403634800](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1544403634800.png)

### 疑问

1. Servlet生成在哪个位置？

   idea中tomcat的目录：

   ![1572486262998](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572486262998.png)

   ```
   C:\Users\Administrator\.IntelliJIdea2019.1\system\tomcat\_JavaEE123\work\Catalina\localhost\day28_01_JSP_war_exploded\org\apache\jsp\
   ```

     ![1572486242572](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572486242572.png)

2. JSP和Servlet是什么关系？

   ```java
   public final class demo01_jsp extends org.apache.jasper.runtime.HttpJspBase
   public abstract class HttpJspBase extends HttpServlet 
   ```

    由此证明JSP本质上就是一个Servlet


### 小结

1. JSP的执行过程有哪两个阶段？

   1. 翻译：由Tomcat将JSP页面翻译成一个Servlet
   2. 编译：由JDK将Servlet编译成class字节码文件

2. JSP与Servlet是什么关系？

   JSP本质上就是一个Servlet



## JSP的3种脚本元素

### 目标

​	学习使用JSP的三种脚本元素

1. 代码片段
2. 表达式
3. 声明

### JSP表达式

#### 语法和作用

```
作用：输出一个变量或计算结果
语法：<%= %> 
```

#### 代码演示

1. 定义一个字符串输出
2. 定义一个运算符输出

```jsp
<h2>JSP表达式</h2>
<%--看到网页只是JSP的运行结果--%>
<%
    String name="超级赛亚人";
%>
用户名： <%=name%> <hr/>
计算表达式： <%=4 + 8%> <hr/>
```

#### 生成的Servlet代码

```java
本质上是这句话：out.print();
```



### JSP代码片段

#### 语法和作用

```
作用：用于在JSP中编写Java代码，只要符合Java的语法规则都可以使用。
语法：<% Java代码  %>
```



#### 需求

在脚本中创建一个List\<String>，添加三个名字，在ol-li中有序列表输出，使用Java中的for循环输出。

#### 执行效果

&nbsp;![1552904885394](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552904885394.png)

#### 代码

```jsp
<h2>代码片段</h2>
<%
    //以后尽可能少的在JSP页面中编写JAVA代码
    List<String> names = new ArrayList<>();
    names.add("孙悟空");
    names.add("猪八戒");
    names.add("包青天");
%>

<ol>
    <% for(String str : names) { %>
    <li>
        <%=str%>
    </li>
    <% } %>
</ol>
```



### JSP的声明

#### 语法和作用

```
作用：声明全局变量和方法
语法：<%! 声明 %>
```

#### 需求

1. 在脚本中创建一个字符串，使用表达式输出
2. 在声明中也创建一个同名的字符，表达式输出，会不会有问题？

#### 效果

![1572487353312](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572487353312.png) 

#### 代码

```jsp
<h2>JSP声明</h2>
<%!
    String name = "魔人布欧";
%>

<%=name%><br/>
<%=this.name%><br/>
```

### 小结

| 组成部分    | 功能                          | 语法   |
| ----------- | ----------------------------- | ------ |
| JSP代码片段 | 运行Java代码                  | <%%>   |
| JSP表达式   | 输出变量，本质上是out.print() | <%=%>  |
| JSP声明     | 声明全局(成员)变量            | <%! %> |



## JSP的3大指令

### 三大指令是

1. page 指令
2. taglib 指令
3. include 指令

### 指令的格式

```jsp
<%@指令名字 属性名="属性值"%>
```

### taglib指令

1. 作用：导入JSTL标签库

2. 语法：

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

### include指令

1. 作用：在一个JSP中包含另一个JSP

2. 语法： 

```jsp
<%@include file="被包含的页面"%>
```

### include指令演示

#### 需求

一个index.jsp和login.jsp包含另一个head.jsp

#### 代码

```jsp
<!--引入头部-->
<div id="header">
    <%@include file="header.jsp"%>
</div>
```

#### 效果

![1563115464530](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1563115464530.png)

### 小结

三大指令是什么？分别的作用是？

1. page：设置网页中各种属性
2. taglib：导入JSTL标签库
3. include：包含另一个JSP页面



## <font color="red">page指令</font>

### page指令概述

1. 作用：设置网页中各种属性，告诉web容器如何将JSP翻译成Servlet

2. 位置：可以出现在网页中任何位置，通常放在网页的最前面。

### 导包的属性

```
language="java" 网页使用的编程语言，默认就是Java(运行在服务器端)
import="java.util.*" 导入包
```

导包有2种写法：

1. 一个import属性导入1个包

```
<%@ page import="java.util.Date" %>
<%@ page import="java.text.SimpleDateFormat" %>
```

2. 一个import属性导入多个包，包之间使用逗号分隔

   ```
   <%@ page import="java.util.Date,java.text.SimpleDateFormat" %>
   ```

   

### 与编码相关的属性

``` jsp
contentType="text/html; charset=utf-8"
相当于Servlet中：response.setContentType("text/html; charset=utf-8")
删除的话会有中文乱码的问题
```



### 与错误相关的属性

```
errorPage="错误页面的URL"  指定如果当前JSP出错，转发到哪个页面去
```

```
isErrorPage="false" 说明当前页面是否是错误页面，true表示是，默认是false。
如果是错误页面，内置对象exception可以使用
```



### 错误页面的跳转

#### 方式一：errorPage

```
在每个JSP上可以设置一个属性：errorPage="错误页面的URL"
只要页面出错，就会自动转发到另一个友好的JSP页面
```



#### 方式二：错误码

```xml
<!--指定错误码跳转到不同的JSP页面-->
<error-page>
    <error-code>500</error-code>
    <!--/表示web目录-->
    <location>/500.jsp</location>
</error-page>

<error-page>
    <error-code>404</error-code>
    <!--/表示web目录-->
    <location>/404.jsp</location>
</error-page>
```



#### 方式三：指定错误的类型

```xml
<!--指定错误类型-->
<error-page>
    <exception-type>java.lang.ArithmeticException</exception-type>
    <location>/500.jsp</location>
</error-page>

<error-page>
    <exception-type>java.lang.NullPointerException</exception-type>
    <location>/404.jsp</location>
</error-page>
```

### 小结

| 属性名                                           | 作用                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| language="java"                                  | 运行在服务器上编程语言                                       |
| import="{package.class \| package.\*}"           | 导包：<br />1. 一个import导入一个包<br />2. 一个import导入多个包，用逗号分隔 |
| errorPage="relative_url"                         | 指定错误页面是哪一个                                         |
| isErrorPage="true\|false"                        | 指定当前页面是否是错误页面                                   |
| contentType="mimeType [ ;charset=characterSet ]" | 设置内容类型和编码                                           |
| isELIgnored="true \| false"                      | 在JSP上是否可以使用EL，默认是false表示可以使用               |
| session="true\|false"                            | 表示是否可以在当前页面中使用会话对象，<br />默认是可用，true |



## JSP的3个动作

### 三个动作

```
语法：<jsp:名字/>
```

![1563116192201](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1563116192201.png)

### \<jsp:include>

1. 作用：用于包含另一个JSP页面，动态包含

2. 语法： 

   ```jsp
   <jsp:include page="被包含的页面"/>
   ```

### include动作与指令的区别

|                 | 静态包含                                                     | 动态包含                                                     |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 语法格式        | 指令：<%@include file="header.jsp"%>                         | 动作：<jsp:include page="被包含的页面"/>                     |
| 包含的方式      | 静态包含，包含的是内容。<br />如果A包含B，则将B中所有的内容复制到A中，再运行A页面。 | 动态包含，包含是运行的结果<br />如果A包含B，先运行B，再将B的结果包含到A中。 |
| 生成Servlet个数 | 最终只会生成1个Servlet                                       | 最终会生成2个Servlet                                         |

注：如果A和B中有相同的变量定义，只能使用动态包含。

### 案例演示

#### 需求

1. 使用动态包含和静态包含分别实现index.jsp中包含header.jsp
2. 观察tomcat的work目录下生成几个Servlet

#### 代码

```jsp
<div id="header">
<%--
    静态包含：只会生成一个Servlet,不能有相同的变量名.
    <%@include file="header.jsp"%>
 --%>
    <%--动态包含: 生成2个Servlet,可以有相同的变量名 --%>
    <jsp:include page="header.jsp"/>
</div>
```

### 小结

|                 | 静态包含                   | 动态包含                     |
| --------------- | -------------------------- | ---------------------------- |
| 语法格式        | <%@include file="文件名"%> | <jsp:include page="文件名"/> |
| 包含的方式      | 包含对方的内容             | 包含对方的运行结果           |
| 生成Servlet个数 | 1个                        | 2个                          |

 

## forward和param动作

### 目标

1. forward作用
2. param的功能

### forward

1. 功能：相当于转发
2. 语法

```jsp
<jsp:forward page="要跳转的页面"/>
与request.getRequestDispatcher("/页面").forward(request,response)一样
```

### param

1. 功能：给转发提供参数，带给下一个页面，做为forward和include的子元素
2. 语法

```jsp
<jsp:param name="参数名" value="参数值"/>
```

### 案例演示

1. 从demo1.jsp转发到demo2.jsp
2. demo1在请求域中添加键和值，demo2看能够得到值并且输出
3. demo1转发的时候带两个参数，user和age，在demo2中获取并且输出
4. 汉字乱码问题的解决

```JSP
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
    //设置编码：如果有乱码要设置在转发前的页面
    request.setCharacterEncoding("utf-8");
%>
第一个页面
<jsp:forward page="demo05.jsp">
    <jsp:param name="user" value="张三"/>
    <jsp:param name="age" value="20"/>
</jsp:forward>
</body>
</html>
```



### 效果

![1572492932279](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572492932279.png) 

### 小结

1. forward作用：转发
2. param的功能：提供转发的参数



## EL的概念和它的两个作用

### 目标

​	什么是EL？

​	它有什么作用？

### 引入案例

#### 需求

1. 有一个脚本变量int m=5

2. 将脚本变量保存在请求作用域中起名为n，并且输出

#### 效果

&nbsp;![1552905330565](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552905330565.png)

#### 代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>EL表达式</title>
</head>
<body>
<%
    int m= 5;
    //将变量放在请求域中
    request.setAttribute("n", m);
%>
脚本变量: <%=m%> <br/>
作用域变量: <%=request.getAttribute("n")%> <br/>
作用域变量: <%=request.getAttribute("m")%> <br/>
EL输出n:  ${n}<br/>
EL输出m:  ${m}<br/>
</body>
</html>

```



### EL表达式的作用

#### 什么是EL

Expression Language：表达式语言，为了减少JSP页面上的Java代码。

#### 作用

1. 输出作用域(请求域，会话域，上下文域)中值
2. 可以进行简单的计算

#### 语法

通过键得到值

```
${作用域中键}
```

### 小结

| 区别         | JSP表达式              | EL表达式        |
| ------------ | ---------------------- | --------------- |
| 语法         | <%=变量名%>            | ${作用域变量键} |
| 输出哪里的值 | 输出脚本中局部变量的值 | 输出作用域中值  |



## 使用JSP和EL取出作用域中的值

### 目标

分别使用JSP代码和EL从作用域中取数据

### 从指定的作用域中获取数据

#### 什么是页面域

1. 对象名：pageContext，它也是一个作用域。页面域只用于JSP中，Servlet中不可用。

2. 范围：只在一个JSP页面起作用

3. 作用域大小比较：页面域 < 请求域 < 会话域 < 上下文域

4. 底层数据结构：无论哪个作用域，底层的结构都是Map结构


#### 四个与页面域有关的方法

![1552907105674](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552907105674.png)

### 示例：取出不同作用域中的值

#### 效果

![1572494229039](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572494229039.png) 

#### 代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>页面域中方法</title>
</head>
<body>
<%
    //向四个域中添加同名的键
    pageContext.setAttribute("person","孙悟空");
    request.setAttribute("person","猪八戒");
    session.setAttribute("person","白骨精");
    application.setAttribute("person","蜘蛛精");

    //删除四个作用域中所有同名的值
    //pageContext.removeAttribute("person");
    //只删除指定域中值
    pageContext.removeAttribute("person", PageContext.PAGE_SCOPE);
%>
<h2>JSP表达式输出</h2>
页面域:  <%=pageContext.getAttribute("person")%><br/>
请求域:  <%=request.getAttribute("person")%><br/>
会话域:  <%=session.getAttribute("person")%><br/>
上下文域:  <%=application.getAttribute("person")%><br/>
自动查找:  <%=pageContext.findAttribute("person")%><br/>
<hr/>

<h2>EL表达式输出</h2>
<%--语法:${作用域.键}--%>
页面域:  ${pageScope.person}<br/>
请求域:  ${requestScope.person}<br/>
会话域:  ${sessionScope.person}<br/>
上下文域:  ${applicationScope.person}<br/>
自动查找:  ${person}<br/>
</body>
</html>
```

### 小结

​	在EL中从四个域中取出变量值写法，四个作用域底层都是Map结构

| 作用域   | EL的写法               |
| -------- | ---------------------- |
| 页面域   | ${pageScope.键}        |
| 请求域   | ${requestScope.键}     |
| 会话域   | ${sessionScope.键}     |
| 上下文域 | ${applicationScope.键} |
| 自动查找 | ${键}                  |



## 上午回顾

### JSP概述

JSP执行过程：

	1. 翻译：把JSP翻译成Servlet
	2. 编译：在Servlet编译成字节码

### JSP的三种脚本元素

| JSP脚本  | 作用                              | 语法   |
| -------- | --------------------------------- | ------ |
| 代码片段 | 用于编写Java代码                  | <% %>  |
| 表达式   | 用于输出变量的值，就是out.print() | <%= %> |
| 声明     | 声明全局变量或方法                | <%! %> |

### JSP的三大指令

| 指令    | 作用                                                |
| ------- | --------------------------------------------------- |
| page    | 设置JSP网页中一些属性，指定如何将JSP翻译成Servlet   |
| taglib  | 导入JSTL标签库                                      |
| include | 静态包含其它的JSP页面，包含的是内容，生成1个Servlet |

#### page指令属性

```
language 指定服务器上编程语言
contentType 设置内容的类型和编码
import 导入包，可以使用逗号分隔，一个import可以导入多个包
errorPage 指定当前页面如果出错，跳转到哪个错误页面
isErrorPage 设置当前页面是否是错误页面
session 是否可以使用会话内置对象
isELIgnored 是否可以使用EL，true表示不可用
```



### JSP的三个动作

| 标准动作        | 功能                                         |
| --------------- | -------------------------------------------- |
| \<jsp:include/> | 动态包含，包含的是运行的结果，生成2个Servlet |
| \<jsp:forward/> | 相当于转发的功能                             |
| \<jsp:param/>   | 用于给上面2个标签添加参数的                  |



### EL概述

Expression Language： 表达式语言

作用：

1. 用于获取作用域中值
2. 可以简单计算功能

### PageContext

```
页面域 < 请求域 < 会话域 < 上下文域
setAttribute()
getAttribute()
removeAttribute() 默认是删除域的同名键和值
removeAttribute(键,作用域) 删除指定作用域中值
findAttribute() 默认从小到大查找指定的键
```

#### EL中几个内置对象

```
pageScope, requestScope, sessionScope, applicationScope
```



## <font color="red">使用EL取出不同数据类型的值</font>

### 目标

​	使用EL：分别从JavaBean，Map，集合List，数组中取出它们的值

### 效果

![1572505493745](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572505493745.png) 

### 代码

```jsp
<%@ page import="com.itheima.entity.Student" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>使用EL取出不同数据类型的值</title>
</head>
<body>
<%--EL取的是作用域中值, 前提要将数据放在作用域中--%>
<h2>得到JavaBean中的属性值</h2>
<%
    Student student = new Student(100, "白骨精", false, 94);
    pageContext.setAttribute("stu", student);
%>
<%--其实还是调用的JavaBean的get方法--%>
名字: ${stu.name}   性别: ${stu.gender?"男":'女'}
<hr>

<h2>得到List集合中的值</h2>
<%
    List<String> names = new ArrayList<>();
    names.add("孙悟空");
    names.add("猪八戒");
    names.add("包青天");
    //放在作用域
    request.setAttribute("names", names);
%>
<%--EL中索引越界,不会出现异常, EL不能遍历,如果要遍历要使用JSTL--%>
${names[0]}  ${names[1]} ${names[3]}
<hr>

<h2>得到数组中的值</h2>
<%
    int [] arr = {20,12,36,2,8};
    session.setAttribute("arr", arr);
%>
<%--取出来的方式与List一样--%>
${arr[0]} ${arr[1]} ${arr[7]}
<hr>

<h2>得到Map中的值</h2>
<%
    HashMap<String,String> users = new HashMap<>();
    users.put("u100", "刘备");
    users.put("u200", "张飞");
    users.put("utf-8", "关羽");
    application.setAttribute("users", users);
%>
<%--语法: ${map集合.键} 如果键中包含一些特殊字符,使用["键"] --%>
${users.u200}   ${users["utf-8"]}  ${users['utf-8']}
</body>
</html>

```

### 小结

使用EL中从域中取出不同数据类型的写法

| 数据类型 | 写法                          |
| -------- | ----------------------------- |
| JavaBean | ${对象名.属性名}              |
| List     | ${list[索引]}                 |
| 数组     | ${arr[索引]}                  |
| Map集合  | \${map.键}  或  \${map["键"]} |



## EL中各种运算符

### 目标

​	学习EL中各种运算符

### 算术运算符

| 算术运算符 | 说明 | 范例       | 结果 |
| ---------- | ---- | ---------- | ---- |
| +          | 加   | ${1+1}     | 2    |
| -          | 减   | ${2-1}     | 1    |
| *          | 乘   | ${1*1}     | 1    |
| /或div     | 除   | ${5 div 2} | 2.5  |
| %或mod     | 取余 | ${5 mod 2} | 1    |

​    

### 比较运算符

| 关系运算符 | 说明                              | 范例      | 结果  |
| ---------- | --------------------------------- | --------- | ----- |
| ==或 eq    | 等于(equal)                       | ${1 eq 1} | true  |
| != 或 ne   | 不等于(not equal)                 | ${1 != 1} | false |
| <    或 lt | 小于(Less than)                   | ${1 lt 2} | true  |
| <=或 le    | 小于等于(Less than or equal)      | ${1 <= 1} | true  |
| >    或 gt | 大于(Greater than)                | ${1 > 2}  | false |
| >=或 ge    | 大于等于(Greater than or   equal) | ${1 >=1}  | true  |

 

### 逻辑运算符 : 

| 逻辑运算符 | 说明     | 范例                | 结果  |
| ---------- | -------- | ------------------- | ----- |
| && 或 and  | 交集(与) | ${true and false}   | false |
| \|\| 或 or | 并集(或) | ${true \|\| false } | true  |
| ! 或 not   | 非       | ${not true}         | false |

 

### 三元运算符：

```jsp
${条件表达式?真值:假值}
```

 

### 判空运算符:

```
语法:  ${empty 作用域中键}
1. 如果键在作用域中为空, 返回true
2. 如果键是一个空字符串,返回true
3. 如果键是一个集合,集合中没有元素,返回true
```



### 代码

```jsp
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>EL中运算符</title>
</head>
<body>
<h2>算术运算符</h2>
${5 div 2}  或 ${5 / 2} <br/>
${5 mod 2}  或 ${5 % 2} <br/>

<h2>比较运算符</h2>
${1 ge 2} <br/>
${1 >= 2}  <br/>

<h2>逻辑运算符</h2>
${true and false} <br/>
${true && false} <br/>
${true or false} <br/>

<h2>三元运算符</h2>
<%--
EL中内置对象,语法: ${param.参数名}
功能相当于request.getParameter("参数名"),并且进行类型转换,如果不能转换就报错
  --%>
${param.age >=18? "成年": "未成年,请绕行"}

<h2>判空运算符</h2>
<%--
语法:  ${empty 作用域中键}
1. 如果键在作用域中为空, 返回true
2. 如果键是一个空字符串,返回true
3. 如果键是一个集合,集合中没有元素,返回true
--%>

<%
    List<String> names = new ArrayList<>();
    names.add("张三");
    pageContext.setAttribute("people", names);
%>
${empty num} <br/>
${empty "abc"} <br/>
${empty people} <br/>
</body>
</html>

```

### 小结

```
算术运算符：div mod 
比较运算符：eq gt le 
逻辑运算符： and or not  && || !
三元运算符：${条件?真:假}
判空运行符： 1. 为null  2. 空串  3. 集合中没有元素
```





## JSTL的概念和作用

### 目标

1. 什么是JSTL
2. 它的作用是什么

### 什么是JSTL

JSP Standard Tag Library：JSP标准标签库，一组标签。用于减少JSP页面上的Java脚本。

JSTL是一组标签库，如下几个定制标签库：

1. 核心标签库 (今天学习的内容)
2. SQL：访问数据库
3. XML：用于在JSP中解析XML
4. 函数标签：可以在页面上使用16个字符串函数
5. 国际化标签：让一页面显示不同国家的语言

### 使用核心标签

导入标签库

| JSTL       | prefix前缀 | URI   标识                        | 作用                              |
| ---------- | ---------- | --------------------------------- | --------------------------------- |
| 核心标签库 | c          | http://java.sun.com/jsp/jstl/core | 用于在JSP上进行程序执行的流程控制 |



### 小结

1. JSTL: 就是一组标签库，由多个标签组成。

2. 核心标签库：if, choose, forEach

3. 使用标签的语法：

   ```
   <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```



## JSTL的标签：if标签

### 目标

在JSP页面上使用if标签

### 作用

判断条件是否为真，如果为真则执行相应的代码

#### if标签的属性

| 属性名 | 是否支持EL | 属性类型 | 描述                                 |
| ------ | ---------- | -------- | ------------------------------------ |
| test   | 支持EL     | 布尔类型 | 如果这个条件为真，执行标签体中的内容 |

### JSTL的使用步骤

1. 复制jar包到web/WEB-INF/lib下

   ![1572508111942](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572508111942.png) 

2. 在JSP页面上使用taglib标签

   ```
   <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

   

3. 在JSP中使用标签

   ```
   格式：
   <c:if test="判断条件">
   	为真才执行这中间的代码
   </c:if>
   ```

   

### 示例：if标签的案例

#### 需求：

如果用户提交的age值大于18，则显示你已经成年。

#### 执行效果

  ![1572508620297](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572508620297.png)                                                        

#### 代码

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>标签库</title>
</head>
<body>
<c:if test="${param.age>=18}">
    可以访问,必须是会员
</c:if>
</body>
</html>

```

### 小结

使用JSTL标签的步骤：

1. 复制jar包到WEB-INF/lib
2. 在JSP页面上使用taglib标签，并且指定uri和prefix这2个属性
3. 在页面中使用\<c:if>



## JSTL的标签：choose标签

### 目标

学习使用choose,when,otherwise三个标签的使用

### 作用

用于2个以上的条件判断，多条件判断，类似于Java中switch语句

#### 属性

| 标签名    | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| choose    | 容器，包含以下2个标签。类似于switch                          |
| when      | 其中的一个判断条件，有test属性做为条件。<br />如果为真就执行标签体中代码。类似于case，可以出现多个。 |
| otherwise | 如果上面所有的条件都不成立，就执行这个标签体。类似于default  |



### 案例：通过输入分数，得到评级

#### 需求

​	在一个表单输入一个分数，提交给自己。然后在同一个页面中根据分数输出相应的等级，80~100优秀 60~80及格 0~60不及格 其它输出分数不正确。

#### 效果

![1544411428657](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1544411428657.png) 

#### 代码

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>choose标签的使用</title>
</head>
<body>
<form action="demo06.jsp" method="get">
    请输入分数:
    <input type="text" name="score" value="${param.score}"/>
    <input type="submit" value="评级"/>
</form>

<c:if test="${not empty param.score}">
    <c:choose>
        <c:when test="${param.score >=80 && param.score<=100}">
            优秀
        </c:when>
        <c:when test="${param.score >=60 && param.score<80}">
            及格
        </c:when>
        <c:when test="${param.score >=0 && param.score<60}">
            不及格
        </c:when>
        <c:otherwise>
            分数有误
        </c:otherwise>
    </c:choose>
</c:if>
</body>
</html>
```

### 小结

1. choose：容器，包含下面2个标签。类似于switch
2. when: 一个判断条件，类似于case
3. otherwise：类似于default



## <font color="red">JSTL的标签：forEach标签</font>

### 目标

循环遍历标签的使用

### forEach的作用

用于循环：

1. fori循环固定的次数
2. 遍历一个集合或数组

#### 需求

输出1到100所有的奇数

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>forEach标签</title>
</head>
<body>
<%--属性:
var 指定变量名, 把值存在页面域中PageContext中
begin 开始的值
end 结束的值
step 步长,每次加几个数
--%>
<c:forEach var="i" begin="1" end="100" step="2">
${i}
</c:forEach>
</body>
</html>
```



### 属性

![1552909020357](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552909020357.png)

#### varStatus属性

![1552909056306](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552909056306.png)

### 案例：遍历集合

#### 需求：

学生信息：包含编号，姓名，性别，成绩。在Servlet中得到所有的学生信息，转发到JSP页面，在JSP页面上使用forEach标签，从List中输出所有的学生信息，以表格的方式显示。

#### 流程图

![1572510040775](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572510040775.png) 

#### 效果

![1572510663562](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572510663562.png) 

#### 代码

##### Student.java

```java
package com.itheima.entity;

/
 * 学生对象
 */
public class Student {

    private int id;  //编号
    private String name;  //名字
    private boolean gender; //true男
    private double score;  //成绩

    public Student(int id, String name, boolean gender, double score) {
        this.id = id;
        this.name = name;
        this.gender = gender;
        this.score = score;
    }

    public Student() {
    }

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

    public boolean isGender() {
        return gender;
    }

    public void setGender(boolean gender) {
        this.gender = gender;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }
}
```

##### Servlet代码

```java
package com.itheima.servlet;

import com.itheima.entity.Student;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

@WebServlet("/student")
public class StudentServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 查询所有学生
        List<Student> students = new ArrayList<>();
        students.add(new Student(1000, "猪八戒", true, 65));
        students.add(new Student(2000, "白骨精", false, 85));
        students.add(new Student(3000, "嫦娥", true, 90));
        students.add(new Student(4000, "唐僧", true, 87));
        students.add(new Student(5000, "蜘蛛精", false, 85));
        //2. 将List集合放在请求域
        request.setAttribute("students", students);
        //3. 转发到JSP页面显示
        request.getRequestDispatcher("/demo08_student.jsp").forward(request,response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

##### JSP代码

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>显示学生列表</title>
</head>
<body>
<h2>学生信息列表</h2>
<table border="1" width="500">
    <tr>
        <th>状态</th>
        <th>编号</th>
        <th>姓名</th>
        <th>性别</th>
        <th>成绩</th>
    </tr>
<%--  forEach属性:
items: 放在作用域中集合或数组, 要遍历的集合
var: 代表集合中每个元素变量名,放在页面域中, 表示其中每一个学生对象
varStatus: 是一个状态对象,给它一个变量名,也是放在页面域中.又有4个属性:
    index: 当前行的索引号
    count: 当前已经遍历了多少行
    first: 如果是第1个元素,返回true
    last: 如果是最后1个元素,返回true
--%>
    <c:forEach items="${students}" var="student" varStatus="row">
    <tr>
        <td>${row.last}</td>
        <td>${student.id}</td>
        <td>${student.name}</td>
        <td>${student.gender?"男":"女"}</td>
        <td>${student.score}</td>
    </tr>
    </c:forEach>
</table>
</body>
</html>

```

### 小结

1. forEach标签的作用是：
   1. 固定循环次数
   2. 遍历集合或数组
2. 以下两个属性的作用是：
   1. items：要遍历的集合或数组
   2. var：变量名，放在页面域，代表每个元素。



## <font color="red">什么是MVC</font>

### 目标

​	什么是MVC，并且使用MVC架构开发自己项目

### 概念

是一种开发模式，不同于三层架构。

三层架构是软件开发分层的方式：表示层，业务层和持久层

MVC：

| 组成       | 说明                                                         | JavaWeb中的实现                   |
| ---------- | ------------------------------------------------------------ | --------------------------------- |
| Model      | 模型：用于操作数据                                           | JavaBean(实体类，数据访问对象DAO) |
| View       | 视图：用于展示数据                                           | JSP+JSTL+EL                       |
| Controller | 控制器：核心组件<br />1. 得到视图提交过来的数据<br />2. 用于调用模型来获取数据<br />3. 将模型中得到的数据发送给视图去显示 | Servlet                           |

### 流程图

![1544413994941](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1544413994941.png)

### JavaWeb中MVC实现

![1572512012247](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572512012247.png)

#### 访问流程说明

1. 用户发送请求Servlet
2. Servlet调用JavaBean访问数据库
3. 数据返回到JavaBean，再返回到Servlet
4. 由Servlet将数据发送到作用域，并且跳转到JSP
5. JSP上显示数据

### 小结

MVC是哪三个单词缩写：

- M: Model
- V: View
- C: Controller



##  案例part1：项目结构的搭建、数据访问层、业务层

### 目标

1. 搭建MVC和三层架构
2. 编写数据访问层和业务层

### 案例需求

使用三层架构和MVC模式开发代码，完成用户显示列表功能

### 案例效果

![1552910157095](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1552910157095.png)

### 案例分析

![1572512255812](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572512255812.png)

### 实现步骤

#### SQL语句

```mysql
create table contact( 
   id int primary key auto_increment , 
   name varchar(20) not null , 
   sex char(1) , 
   age int(3) unsigned ,   -- 无符号
   address varchar(10) ,   -- 籍贯
   qq varchar(18) , 
   email varchar(25) 
 ) ;
 
-- 插入记录
insert  into contact(name,sex,age,address,qq,email) values 
('猪八戒','男',25,'广东','834523234','zhuzhuxia@itcast.cn'),
('貂蝉','女',18,'湖南','59869834','diaochan@qq.com'),
('孙悟空','男',28,'湖南','87967822','wukong@itheima.com'),
('周瑜','男',25,'广西','2743759345','zhou@163.com');

select * from contact;
```

#### 导入jar包

![1572512365603](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572512365603.png) 

#### 核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <!--在控制台显示SQL语句-->
        <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>
    
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
                <property name="url" value="jdbc:mysql://localhost:3306/day28"/>
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

#### 实体类

```java
package com.itheima.entity;

public class Contact {

    private int id;
    private String name;
    private String sex;
    private int age;
    private String address;
    private String qq;
    private String email;

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

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getQq() {
        return qq;
    }

    public void setQq(String qq) {
        this.qq = qq;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Contact{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                ", qq='" + qq + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
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

/**
 会话工厂工具类 */
public class SqlSessionUtils {
    private static SqlSessionFactory factory;

    static {
        //实例化工厂建造类
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //读取核心配置文件
        try (InputStream inputStream = Resources.getResourceAsStream("sqlMapConfig.xml")) {
            //创建工厂对象
            factory = builder.build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     得到会话对象
     @return 会话对象
     */
    public static SqlSession getSession() {
        return factory.openSession();
    }

    /**
     得到会话对象
     @param isAutoCommit 是否自动提交事务
     */
    public static SqlSession getSession(boolean isAutoCommit) {
        return factory.openSession(isAutoCommit);
    }

    /**
     得到工厂对象
     @return 会话工厂对象
     */
    public static SqlSessionFactory getSqlSessionFactory() {
        return factory;
    }

}

```

#### 编写DAO代码

```java
package com.itheima.dao;

import com.itheima.entity.Contact;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * 联系人DAO
 */
public interface ContactDao {

    /**
     * 查询所有的联系人
     */
    @Select("select * from contact")
    List<Contact> findAll();
}
```

```java
package com.itheima.dao.impl;

import com.itheima.dao.ContactDao;
import com.itheima.entity.Contact;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class ContactDaoImpl implements ContactDao {
    @Override
    public List<Contact> findAll() {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 通过会话对象得到代理对象
        ContactDao contactDao = session.getMapper(ContactDao.class);
        //3. 调用接口中方法
        List<Contact> contacts = contactDao.findAll();
        //4. 关闭会话
        session.close();
        //5. 返回查询的联系人
        return contacts;
    }
}

```

#### 编写业务层代码

```java
package com.itheima.service;

import com.itheima.dao.ContactDao;
import com.itheima.dao.impl.ContactDaoImpl;
import com.itheima.entity.Contact;

import java.util.List;

/**
 * 业务层
 */
public class ContactService {

    private ContactDao contactDao = new ContactDaoImpl();

    public List<Contact> findAll() {
        return contactDao.findAll();
    }
}
```



##  案例part2：查询所有联系人的Web层

### 目标

1. 使用Servlet来调用业务层，得到所有的联系人

2. 在JSP上使用JSTL显示请求域中数据

### 效果

![1544415844278](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1544415844278.png)

### 步骤

#### Servlet代码

```java
package com.itheima.servlet;

import com.itheima.entity.Contact;
import com.itheima.service.ContactService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

/**
 * 查询所有的联系人
 */
@WebServlet("/list")
public class ListContactServlet extends HttpServlet {

    private ContactService contactService = new ContactService();

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 调用业务层得到所有的联系人
        List<Contact> contacts = contactService.findAll();
        //2. 将联系人放在请求域
        request.setAttribute("contacts", contacts);
        //3. 转发到JSP页面显示
        request.getRequestDispatcher("/list.jsp").forward(request, response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### 导入Bootstrap页面的文件夹

复制"资料/原型"下的三个文件夹到web根路径

#### 导入list.jsp页面

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<!-- 网页使用的语言 -->
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>显示联系人</title>

    <!-- 1. 导入CSS的全局样式 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
    <script src="js/jquery-2.1.0.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>
    <style type="text/css">
        td, th {
            text-align: center;
        }
    </style>
</head>
<body>
<div class="container">
    <h3 style="text-align: center">显示所有联系人</h3>
    <table border="1" class="table table-bordered table-hover">
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
            <th>籍贯</th>
            <th>QQ</th>
            <th>邮箱</th>
            <th>操作</th>
        </tr>
        <c:forEach items="${contacts}" var="contact">
        <tr>
            <td>${contact.id}</td>
            <td>${contact.name}</td>
            <td>${contact.sex}</td>
            <td>${contact.age}</td>
            <td>${contact.address}</td>
            <td>${contact.qq}</td>
            <td>${contact.email}</td>
            <td><a class="btn btn-default btn-sm" href="修改联系人.html">修改</a>&nbsp;<a class="btn btn-default btn-sm" href="修改联系人.html">删除</a></td>
        </tr>
        </c:forEach>
        <tr>
            <td colspan="8" align="center"><a class="btn btn-primary" href="添加联系人.html">添加联系人</a></td>
        </tr>
    </table>
</div>
</body>
</html>
```



## 案例part3：添加联系人

### DAO层

```java
package com.itheima.dao;

import com.itheima.entity.Contact;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * 联系人DAO
 */
public interface ContactDao {

    /**
     * 查询所有的联系人
     */
    @Select("select * from contact")
    List<Contact> findAll();

    /**
     * 插入一条记录
     */
    @Insert("insert into contact (name, sex, age, address, qq, email) values (#{name},#{sex},#{age},#{address},#{qq},#{email})")
    int addContact(Contact contact);
}

```

```java
package com.itheima.dao.impl;

import com.itheima.dao.ContactDao;
import com.itheima.entity.Contact;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class ContactDaoImpl implements ContactDao {
    @Override
    public List<Contact> findAll() {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 通过会话对象得到代理对象
        ContactDao contactDao = session.getMapper(ContactDao.class);
        //3. 调用接口中方法
        List<Contact> contacts = contactDao.findAll();
        //4. 关闭会话
        session.close();
        //5. 返回查询的联系人
        return contacts;
    }

    /**
     * 插入一条记录
     * @param contact
     */
    @Override
    public int addContact(Contact contact) {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 通过会话对象得到代理对象
        ContactDao contactDao = session.getMapper(ContactDao.class);
        //3. 调用接口中方法
        int row = contactDao.addContact(contact);
        //4. 关闭会话
        session.commit();
        session.close();
        return row;
    }
}
```

### 业务层

```java
package com.itheima.service;

import com.itheima.dao.ContactDao;
import com.itheima.dao.impl.ContactDaoImpl;
import com.itheima.entity.Contact;

import java.util.List;

/**
 * 业务层
 */
public class ContactService {

    private ContactDao contactDao = new ContactDaoImpl();

    public List<Contact> findAll() {
        return contactDao.findAll();
    }

    /**
     * 添加联系人
     * @param contact
     * @return 影响的行数
     */
    public int addContact(Contact contact) {
        return contactDao.addContact(contact);
    }
}
```

### Servlet

```java
package com.itheima.servlet;

import com.itheima.entity.Contact;
import com.itheima.service.ContactService;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

/**
 * 添加联系人
 */
@WebServlet("/add")
public class AddContactServlet extends HttpServlet {

    private ContactService contactService = new ContactService();

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 设置请求的编码
        request.setCharacterEncoding("utf-8");
        //2. 得到表单提交的所有的参数
        Map<String, String[]> map = request.getParameterMap();
        //3. 将表单的数据封装成Contact对象
        Contact contact = new Contact();
        try {
            BeanUtils.populate(contact, map);
        } catch (Exception e) {
            e.printStackTrace();
        }
        //4. 调用业务层添加联系人
        contactService.addContact(contact);
        //5. 跳转到/list Servlet中
        response.sendRedirect(request.getContextPath() + "/list");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### list.jsp

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<!-- 网页使用的语言 -->
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>显示联系人</title>

    <!-- 1. 导入CSS的全局样式 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
    <script src="js/jquery-2.1.0.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>
    <style type="text/css">
        td, th {
            text-align: center;
        }
    </style>
</head>
<body>
<div class="container">
    <h3 style="text-align: center">显示所有联系人</h3>
    <table border="1" class="table table-bordered table-hover">
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
            <th>籍贯</th>
            <th>QQ</th>
            <th>邮箱</th>
            <th>操作</th>
        </tr>
        <c:forEach items="${contacts}" var="contact">
        <tr>
            <td>${contact.id}</td>
            <td>${contact.name}</td>
            <td>${contact.sex}</td>
            <td>${contact.age}</td>
            <td>${contact.address}</td>
            <td>${contact.qq}</td>
            <td>${contact.email}</td>
            <td><a class="btn btn-default btn-sm" href="修改联系人.html">修改</a>&nbsp;<a class="btn btn-default btn-sm" href="修改联系人.html">删除</a></td>
        </tr>
        </c:forEach>
        <tr>
            <td colspan="8" align="center"><a class="btn btn-primary" href="add.jsp">添加联系人</a></td>
        </tr>
    </table>
</div>
</body>
</html>
```



# 学习总结

1. 能够说出jsp的优势

   既方便美化，又可以编写动态网页

   JSP=Servlet+HTML

    

2. 能够编写jsp代码片段、声明、脚本表达式

| 组成部分      | 功能                      | 语法      |
| ------------- | ------------------------- | --------- |
| JSP代码片段   | 编写Java代码              | <%%>      |
| JSP脚本表达式 | 输出变量值，是out.print() | <%=%>     |
| JSP声明       | 声明全局变量              | <%! %>    |
| JSP注释       | 在网页源代码上看不到      | <%-- --%> |

3. 能够说出el表达式的作用

   1. 获取作用域中值
   2. 用计算

4. 能够使用el表达式获取javabean的属性

```
${javabean.属性名} 调用get方法
```

5. 能够使用jstl标签库的if标签

| 属性名 | 属性类型 | 属性描述                 |
| ------ | -------- | ------------------------ |
| test   | boolean  | 如果为真执行标签体中代码 |

6. 能够使用jstl标签库的choose标签

| 标签名    | 作用                 |
| --------- | -------------------- |
| choose    | 多分支，类似于switch |
| when      | case                 |
| otherwise | default              |

7. 能够使用jstl标签库的foreach标签

| 属性名    | 属性描述                                  |
| --------- | ----------------------------------------- |
| items     | 要遍历集合或数组                          |
| var       | 其中一个元素                              |
| varStatus | 元素的状态对象：index, count, first ,last |
| begin     | 从哪里开始                                |
| end       | 到哪里结束                                |
| step      | 步长                                      |

8. 能够说出开发模式的作用

MVC开发模式：

M: Model

V: View

C: Controller

![1572514276358](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/JSP&El/1572514276358.png)



9. 能够使用三层架构模式完成显示用户案例

