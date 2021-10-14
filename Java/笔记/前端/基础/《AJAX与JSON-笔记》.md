# 《AJAX与JSON-笔记》

# 回顾

## 基本选择器

| 基本选择器 | 语法   |
| ---------- | ------ |
| 标签选择器 | 标签名 |
| 类选择器   | .类名  |
| id选择器   | \#ID   |

## 层级选择器

| 层级选择器                 | 语法 |
| -------------------------- | ---- |
| 获得A元素内部的所有的B元素 | A B  |
| 获得A元素下面的所有B子元素 | A>B  |
| 获得A元素同级，下一个B元素 | A+B  |

## JS与JQ对象的转换

| 操作                   | 方法                       |
| ---------------------- | -------------------------- |
| 将JS对象-->jQuery对象  | $(JS对象)                  |
| 将jQuery对象--> JS对象 | JQ对象[0] 或 JQ对象.get(0) |

## jQuery对DOM的操作的方法

| jQuery中的方法  | 作用                 |
| --------------- | -------------------- |
| html()          | 相当于innerHTML      |
| text()          | innerText            |
| val()           | value                |
| attr()          | 操作属性             |
| prop()          | 布尔类型的属性       |
| addClass()      | 添加类样式名         |
| css()           | 添加行内样式         |
| $(标签全部内容) | 创建元素             |
| append()        | 添加到最后一个子元素 |
| prepend()       | 添加到第一个子元素   |
| before()        | 添加到当前元素的前面 |
| after()         | 添加到当前元素的后面 |
| remove()        | 删除自己             |
| empty()         | 清空子元素           |

## 事件绑定和解绑

| 语法 | 说明                       |
| ---- | -------------------------- |
| 绑定 | on("事件名", 事件处理函数) |
| 解绑 | off()                      |

## jQuery对象的遍历

| jQuery三种遍历数组的方法              |
| ------------------------------------- |
| JQ对象.each(function(index, element)) |
| $.each(集合, function(index,element)) |
| for (var 变量名 of 集合)              |



# 学习目标

1. 概念，原生ajax
   1. 能够理解异步的概念
   2. 能够了解原生js的ajax
2. jQuery访问ajax的方法
   1. 能够使用jQuery的\$.get()进行访问
   2. 能够使用jQuery的\$.post()进行访问
   3. 能够使用jQuery的\$.ajax()进行访问
   4. <font color="red">能够使用jQuery3.0的$.get()新增签名进行访问</font>
   5. <font color="red">能够使用jQuery3.0的$.post()新增签名进行访问</font>
3. JSON
   1. 能够掌握json的三种数据格式
   2. <font color="red">能够使用json转换工具Jackson进行json格式字符串的转换</font>
4. 案例
   1. 能够完成用户名是否存在的查重案例
   2. 能够完成自动补全的案例



# 学习内容

## jQuery的插件扩展机制

### 目标

学习jQuery的两种插件扩展的机制

### jQuery插件机制概述

插件网站www.jq22.com

![1565260846285](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1565260846285.png) 



### jQuery插件机制语法

对JQ对象进行功能的扩展

```javascript
$.fn.extend({函数名:处理函数})
```

对JQ全局函数进行扩展

```javascript
$.extend({函数名:处理函数})
```



### 对jQuery对象进行方法扩展

#### 需求

我们要开发一个功能，某个元素当双击它时，便alert 当前元素的值。

#### 效果

![1565260988094](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1565260988094.png) 

#### 步骤

1. 编写扩展函数，添加函数名：alertWhileDblclick

2. 在方法体内，编写当前元素的双击事件

3. 双击事件中弹出当前元素的值

4. 调用：得到所有的输入框元素，调用上面的alertWhileDblclick函数

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQ插件扩展</title>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
用户：<input type="text" value="张三"> <br/><br/>
密码：<input type="password"> <br/><br/>
性别：<input type="radio" name="gender" value="男">男  &nbsp; <input type="radio" name="gender" value="女">女
</body>
<script type="text/javascript">
    /*
    某个元素当双击它时，便alert 当前元素的值。
    语法：$.fn.extend({函数名:处理函数})
     */
    $.fn.extend({
        alertWhileDblclick: function () {
            //编写当前元素的双击事件
            $(this).dblclick(function () {
                alert($(this).val());
            });
        }
    });

    //使用JQ对象来调用上面函数
    $("input").alertWhileDblclick();
</script>

</html>
```

### 小结

| 功能说明               | 语法                                            |
| ---------------------- | ----------------------------------------------- |
| 对JQ对象进行功能的扩展 | $.fn.extend({函数名:处理函数, 函数名:处理函数}) |



## 对jQuery全局进行方法扩展

### 目标

1. 编写全局函数：找出一个数组中的最大值并且返回

2. 调用的格式为：$.maxArr(数组)，返回其中的最大值。

### 步骤

1. 自定义全局函数maxArr，带参数arr

2. 将第0个值赋值给一个临时变量

3. 循环判断每个元素，如果临时变量小于当前元素，则临时变量等于当前元素。

4. 返回最大值

### 效果

 ![1565261207482](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1565261207482.png)          

### 代码 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>全局函数扩展</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<script type="text/javascript">
    /**
     * 找出一个数组中的最大值并且返回
     *  语法：$.extend({函数名: 处理函数})
     */
    $.extend({
        maxArr: function (arr) {  //arr调用时提供的参数
            var m = arr[0];
            for (var i = 1; i < arr.length; i++) {
                if (arr[i] > m) {
                    m = arr[i];
                }
            }
            return m;
        }
    });

    //调用全局函数
    var arr = [4, 20, 183, 402, 401, 55];
    var m = $.maxArr(arr);
    document.write("最大值是：" + m + "<br/>");
</script>
</body>
</html>
```

### 小结

| 功能说明             | 语法                         |
| -------------------- | ---------------------------- |
| 对JQ全局函数进行扩展 | $.extend({函数名: 处理函数}) |



## AJAX的概述

### 目标

1. 什么是AJAX
2. 它的作用是什么

### 什么是ajax

![1553212487038](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553212487038.png) 

概念：Asynchronous JavaScript And XML 异步的JavaScript和XML

特点：

1. 服务器的访问是后台进行的。
2. 浏览器端网页的内容是局部刷新，没有页面的跳转
3. 以后的开发以异步为主，同步为辅。

好处：

1. 工作效率更高
2. 用户体验更好
3. 可以实现以前同步不能实现的功能

### 同步和异步的区别

同步方式：以前我们使用JSP的方式开发项目，就是同步方式。浏览器端与服务器端不能同时工作，以一种串行的方式工作的。浏览器工作的时候，服务器在等待的。服务器工作的时候，浏览器在等待的。

异步方式：今天学习的就是异步方式，浏览器与服务器是并行的方式工作的。浏览器与服务器可以同时工作。	

AJAX使用异步的提交方式，浏览器与服务器可以并行操作，即浏览器后台发送数据给服务器。用户在前台还是可以继续工作。用户感觉不到浏览器已经将数据发送给了服务器，并且服务器也已经返回了数据。

![1553212896200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553212896200.png)

#### ajax使用的技术

1. JavaScript: 用于发送请求，接收服务器的响应结果，局部刷新网页。
2. XML: 在不同的语言之间实现数据交换，因为XML的解析过于麻烦，目前已经淘汰，使用JSON这种格式。

![1553213029597](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553213029597.png)



### AJAX的应用场景

#### 检查用户名是否已经被注册

​	很多站点的注册页面都具备自动检测用户名是否存在的友好提示，该功能整体页面并没有刷新，但仍然可以异步与服务器端进行数据交换，查询用户的输入的用户名是否在数据库中已经存在。   

![1553212896200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553212896200.png)                                                     

###  省市下拉框联动

​	很多站点都存在输入用户地址的操作，在完成地址输入时，用户所在的省份是下拉框，当选择不同的省份时会出现不同的市区的选择，这就是最常见的省市联动效果。

![1553213415813](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553213415813.png)    

####  内容自动补全

百度的搜索补全功能：

![1553213438248](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553213438248.png)     

京东的搜索补全功能：

![1553213486539](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553213486539.png)

### 小结

1. AJAX的作用：异步访问服务器
2. JavaScript作用：
   1. 发送请求
   2. 接收响应的结果
   3. 局部刷新网页
3. XML的作用：数据交换，被JSON代替
4. 什么是异步请求？浏览器与服务器是并行工作的



## 原生ajax的访问流程

### 目标

1. 了解原生的ajax访问服务器的整个流程
2. 核心对象：XMLHttpRequest

### AJAX的执行流程

![1553214240418](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553213608295.png)

流程说明：

1. 在浏览器端通过JavaScript创建XMLHttpRequest对象
2. 由XMLHttpRequest请求对象发送请求的数据给服务器端
3. 在服务器端处理数据，进行数据的操作，进行业务处理等
4. 将处理结果以XML的格式发送回来
5. 在浏览器使用JavaScript处理回调函数，在回调函数中局部刷新网页

### 小结

1. 什么是原生的ajax？使用原始的JavaScript来实现AJAX的异步操作
2. 它的核心对象是？ XMLHttpRequest



## XMLHttpRequest对象的事件、方法和属性

### 目标

​	学习XMLHttpRequest对象有哪些事件，方法和属性

### 语法

| 创建XMLHttpRequest对象 | 说明                           |
| ---------------------- | ------------------------------ |
| new XMLHttpRequest()   | 使用无参的构造方法创建请求对象 |

| XMLHttpRequest对象的事件 | 说明                             |
| ------------------------ | -------------------------------- |
| on ready state change    | 准备状态发生改变就会激活这个事件 |

| XMLHttpRequest对象的属性 | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| readyState               | 获取请求对象的准备状态：<br />1. 开始发送请求<br />2. 请求结束<br />3. 开始接收响应数据<br />4. 响应数据接收完毕 |
| status                   | 服务器状态码，200表示服务器正常响应                          |
| responseText             | 接收服务器返回的字符串                                       |
| responseXML              | 接收服务器返回的XML格式数据                                  |

| XMLHttpRequest对象的方法 | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| open("GET","URL",true)   | 打开服务器的连接<br />1. GET 发送请求的方式<br />2. URL 服务器的地址<br />3. true 异步请求，false表示同步请求 |
| send()                   | 发送请求给服务器                                             |

### 小结

1. 创建这个对象：new XMLHttpRequest()
2. 事件：onreadystatechange
3. 属性：readyState, status, responseText
4. 方法：open() send()



## 案例：使用原生的AJAX判断用户名是否存在

### 目标

​	了解原生的ajax如何去实现这个案例

### 需求

​	用户注册时输入一个用户名，失去焦点以后，通过ajax后台判断用户名是否存在。服务器先不访问数据库，直接判断用户名，如果用户名为newboy，则表示用户已经存在，否则用户名可以注册使用。

### 效果

![1544836305809](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1544836305809.png) 

### 服务器端

#### 步骤

1. 编写Demo1UserExistsServlet
2. 设置响应的类型为text/plain;charset=utf-8，纯文本的数据。如：\<br/>不会换行
3. 得到客户端发送过来的数据：request.getParameter()
4. 如果用户名忽略大小写比较等于newboy，则向客户端打印"用户名已经存在"，否则打印"恭喜你，可以注册"

#### Servlet

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
public class Demo1UserServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //在后台输出纯文本: <br/> 不是换行，而是一个字符串
        response.setContentType("text/plain;charset=utf-8");
        PrintWriter out = response.getWriter();

        //1.得到客户端提交的用户名
        String user = request.getParameter("user");
        //2. 判断用户名是否存在
        if ("NewBoy".equals(user)) {
            //3. 如果存在输出存在
            out.print("用户名已经存在");
        }
        //4. 不存在就输出可以注册
        else {
            out.print("恭喜你，可以注册");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 客户端

#### 步骤

1. 文本框失去焦点，得到文本框中的姓名
2. 创建XMLHttpRequest请求对象
3. 设置请求对象的onreadystatechange事件，即"准备状态改变"事件。
4. 当readyState等于4，并且服务器status响应码为200则表示响应成功
5. 通过responseText得到响应的字符串
6. 如果用户存在，在后面的span显示"用户名已经存在"
7. 不存在，在后面的span中显示"恭喜你，可以注册"。
8. 设置请求的URL，将用户名以url参数传递
9. 调用open方法，设置提交给服务器的请求方式和访问地址
10. 调用send方法发送请求

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用原生的Ajax</title>
</head>
<body>
<h2>用户注册</h2>
用户名：
<input type="text" name="user" id="user"> <span id="info"></span>
</body>
<script type="text/javascript">
    //文本框改变事件
    document.getElementById("user").onchange = function () {
        //1.创建XMLHttpRequest对象
        var request = new XMLHttpRequest();
        //2. 设置回调函数，请求状态发生变化时调用这个函数
        request.onreadystatechange = function () {
            /*
            在回调函数中处理服务器返回的数据
            status：200 表示服务器正常响应
            readyState：4 表示服务器响应结束
             */
            if (request.status == 200 && request.readyState == 4) {
                //接收服务器返回的字符串
                var text = request.responseText;
                //显示在后面span中
                document.getElementById("info").innerText = text;
            }
        };
        //3. 打开服务器连接
        var url = "demo1?user=" + document.getElementById("user").value;
        request.open("GET", url, true);
        //4. 发送请求给服务器
        request.send();
    }
</script>
</html>
```

### 小结

浏览器端的访问流程

1. 创建XMLHttpRequest对象

2. 设置回调函数，请求状态发生变化时调用这个函数
   1. status：200 表示服务器正常响应
   2. readyState：4 表示服务器响应结束
   3. 接收服务器返回的字符串
   4. 显示在后面span中
3. 打开服务器连接
4. 发送请求给服务器





## 传统的\$.get()方法的使用

### 目标

传统$.get()方法的介绍

### 与ajax操作相关的jQuery方法

1. 前面2个语法相同，用于兼容3.0以前的版本
2. 后面3种语法相同，用于3.0以后的版本

![1553214556977](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553214556977.png)

### 语法

![1553214604704](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553214604704.png)

#### 参数说明

| 参数名称 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| url      | 访问服务器的地址                                             |
| data     | 发送给服务器的数据，有2种格式：<br />1. 键=值&键=值<br />2. {键:值, 键:值} |
| callback | 指定回调函数，参数就是服务器返回的数据                       |
| type     | 服务器返回的数据类型<br />取值可以是 xml, html, script, json, text, _default等<br />如果在服务器端指定响应的数据类型，则可以省略 |

### 演示案例

#### 步骤

1. 导入jQuery框架的js文件
2. 编写文本框失去焦点blur()事件
3. 得到文本框中的姓名
4. 使用$.get方法发送请求给服务器，回调函数的参数就是返回值
5. 根据返回的结果，在回调函数中设置span的text

#### 代码

#### HTML代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用jQuery的方法使用AJAX</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<h2>用户注册</h2>
用户名：
<input type="text" name="user" id="user"> <span id="info"></span>
</body>
<script type="text/javascript">
    //文本框的改变事件
    $("#user").change(function () {
        /*
        访问服务器
        1. url: 访问地址
        2. data：发送的数据
        3. callback：回调函数，参数是：服务器返回数据
        4. type：服务器返回的数据类型
         */
        $.get("demo1", "user=" + $(this).val(), function (text) {
            $("#info").text(text);
        });
    });
</script>
</html>
```

服务器端的代码不变

### 小结

| 参数名称 | 解释                               |
| -------- | ---------------------------------- |
| url      | 访问地址                           |
| data     | 发送的数据                         |
| callback | 回调函数，参数是：服务器返回的数据 |
| type     | 服务器返回的类型                   |



## 传统的\$.post()方法的使用

### 目标

​	学习使用post()方法发送请求给服务器

### 语法

![1553214793344](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553214793344.png)

#### 参数说明

- 与上面学习的$.get()方法相同

![1553215074171](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553215074171.png)

### 案例演示

#### 步骤

1. 将浏览器端的$.get请求换成\$.post请求
2. 在服务器端打印请求的方式

#### 代码

##### html变化的代码

```javascript
$.post("demo1", "user=" + $(this).val(), function (text) {
    $("#info").text(text);
});
```

##### Servlet变化的代码

```java
System.out.println("请求的方式：" + request.getMethod());

//1.得到客户端提交的用户名
String user = request.getParameter("user");
//在jQuery中POST方法提交汉字没有乱码
System.out.println("用户名：" + user);
```

### 小结

| 参数名称 | 解释                         |
| -------- | ---------------------------- |
| url      | 服务器地址                   |
| data     | 发送的数据                   |
| callback | 回调函数，参数是：返回的数据 |
| type     | 返回的数据类型               |



## \$.ajax()方法的使用

### 目标

1. $.ajax()方法的使用

2. 案例：使用ajax实现后台用户登录功能

### 语法

![1553216353594](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553216353594.png) 

settings是一个JSON形式的对象，格式是{name:value,name:value... ...}，常用的name属性名如下：

| 属性名称 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| url      | 访问服务器的地址                                             |
| async    | 默认是true，表示异步发送请求，false同步发送                  |
| method   | 指定是get或post请求，默认是get                               |
| data     | 发送给服务器的数据，有2种格式：<br />1. 键=值&键=值<br />2. {键:值, 键:值} |
| dataType | 服务器返回的数据类型                                         |
| success  | 服务器正常响应的回调函数，参数：服务器返回的数据             |
| error    | 服务器出现异常的回调函数，参数：XMLHttpRequest对象           |



### 案例：使用AJAX实现后台用户登录的功能

#### 技术点

![1553216438079](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553216438079.png)

#### 需求

1. 页面上有用户名和密码两个文本框，点登录按钮，使用后台AJAX完成登录成功。
2. 如果用户登录成功显示登录成功，否则显示登录失败。
3. 后台服务器暂不使用数据库，如果用户名为：newboy，密码为：123，则登录成功。

#### 效果

成功

![1553216482149](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553216482149.png)

失败

![1553216505203](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553216505203.png)

#### 服务器

##### 步骤

1. 设置响应类型text/plain;charset=utf-8，得到打印流
2. 得到用户名和密码
3. 判断用户名和密码是否正确
4. 如果正确，则打印：欢迎您，newboy，登录成功
5. 错误则打印：登录失败

##### 代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/demo2")
public class Demo2LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //响应的类型为纯文本
        response.setContentType("text/plain;charset=utf-8");
        PrintWriter out = response.getWriter();

        //1.得到用户名和密码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //2. 判断用户名和密码是否正确
        if ("newboy".equals(username) && "123".equals(password)) {
            //3. 如果正确输出登录成功
            out.print("登录成功，欢迎您：" + username);
        } else {
            //4. 否则输出登录失败
            out.print("登录失败，请重试");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}

```

#### 客户端

##### 步骤

1. 页面代码如下：login.html，页面上有一个登录的表单数据。注：登录按钮是一个普通的button
2. 给登录按钮添加点击事件
3. 得到表单中所有的数据项: serialize()
4. 使用$.ajax方法提交数据给服务器
   1. 设置url请求地址
   2. data数据
   3. success成功的回调函数
   4. 在回调函数中直接使用alert弹出服务器返回的数据

##### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>用户登录</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<h2>用户登录</h2>
<form id="loginForm">
    <table>
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username" id="username"/></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password" id="password"/></td>
        </tr>
        <tr>
            <td colspan="2" align="center"><input type="button" value="登录" id="btnLogin"/></td>
        </tr>
    </table>
</form>

<script type="text/javascript">
    //登录按钮的点击事件
    $("#btnLogin").click(function () {
        //1. 得到用户名和密码
        var username = $("#username").val();
        var password = $("#password").val();
        //参数需要自己手动拼接字符串
        //var param = "username=" + username + "&password=" + password;
        //使用表单对象的方法：把整个表单中所有的表单项名字和值，拼接成一个字符串
        var param = $("#loginForm").serialize();
        //2. 发送请求给服务器
        $.ajax({
            url: "demo2",    //访问地址
            //async: false,  //同步或异步
            data: param,    //发送给服务器的数据
            //method: "post",   //POST方法提交
            success: function (result) {   //登录成功的回调函数
                //3. 在回调函数中显示登录是否成功
                alert(result);
            },
            error: function (request) {   //参数是XMLHttpRequest对象
                alert("服务器出现异常，状态码是：" + request.status);
            }
        });

        //alert("JS代码继续运行");
    });
</script>
</body>
</html>
```

### 小结

| 属性名称 | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| url      | 服务器访问的地址                                             |
| async    | 异步true或同步false，默认是true。异步指发送请求和后面的代码是并行执行的。 |
| data     | 发送给服务器的数据                                           |
| method   | 提交的方式                                                   |
| dataType | 服务器返回的数据类型                                         |
| success  | 正常响应的回调函数，参数：服务器返回的值                     |
| error    | 服务器出现异常执行的回调函数，参数是：XMLHttpRequest对象     |



## <font color="red">jQuery3.0的\$.get()方法实现登录</font>

### 目标

1. 学习3.0新的$.get()方法，签名与ajax()完全一样
2. 使用$.get()方法实现登录

### 新增签名方式语法

![1553217838693](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553217838693.png)

#### 参数说明

![1553216394291.png](/assets/1553216394291.png)

###案例：GET新增签名方式实现上面的登录

#### 步骤

1. 给登录按钮添加点击事件
2. 得到表单中所有的数据项
3. 使用$.get()方法发送请求
   1. 设置url请求地址
   2. 设置发送的数据
   3. 设置success回调函数
   4. 在回调函数中处理返回的数据

#### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>用户登录</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<h2>用户登录</h2>
<form id="loginForm">
    <table>
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username" id="username"/></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password" id="password"/></td>
        </tr>
        <tr>
            <td colspan="2" align="center"><input type="button" value="登录" id="btnLogin"/></td>
        </tr>
    </table>
</form>

<script type="text/javascript">
    //登录按钮的点击事件
    $("#btnLogin").click(function () {
        //使用表单对象的方法：把整个表单中所有的表单项名字和值，拼接成一个字符串
        //var param = $("#loginForm").serialize();
        //发送请求给服务器
        $.get({
            url: "demo2",    //访问地址
            data: {
                username: $("#username").val(),
                password: $("#password").val()
            },    //发送给服务器的数据
            method: "post",   //如果指定了参数，这个优先
            success: function (result) {   //登录成功的回调函数
                //在回调函数中显示登录是否成功
                alert(result);
            }
        });
    });
</script>
</body>
</html>
```

- 服务器端代码不变

### 小结

get方法使用了三个参数：

| 属性名称 | 解释                          |
| -------- | ----------------------------- |
| url      | 访问地址                      |
| data     | 发送给服务器的数据，有2种格式 |
| success  | 服务器正常响应的回调函数      |



## <font color="red">jQuery3.0的$.post()方式实现登录</font>

### 目标

1. 理解$.post()方法的参数

2. 使用$.post()方法实现登录的功能

### POST新增签名方式语法

![1553218742245](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553218742245.png)

#### 参数说明

![1553218790852](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553218790852.png)

### 代码

##### html代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>用户登录</title>
    <script src="js/jquery-3.3.1.js"></script>
</head>
<body>
<h2>用户登录</h2>
<form id="loginForm">
    <table>
        <tr>
            <td>用户名</td>
            <td><input type="text" name="username" id="username"/></td>
        </tr>
        <tr>
            <td>密码</td>
            <td><input type="password" name="password" id="password"/></td>
        </tr>
        <tr>
            <td colspan="2" align="center"><input type="button" value="登录" id="btnLogin"/></td>
        </tr>
    </table>
</form>

<script type="text/javascript">
    //登录按钮的点击事件
    $("#btnLogin").click(function () {
        //使用表单对象的方法：把整个表单中所有的表单项名字和值，拼接成一个字符串
        //var param = $("#loginForm").serialize();
        //发送请求给服务器
        $.post({
            url: "demo2",    //访问地址
            data: {
                username: $("#username").val(),
                password: $("#password").val()
            },    //发送给服务器的数据
            success: function (result) {   //登录成功的回调函数
                //在回调函数中显示登录是否成功
                alert(result);
            }
        });
    });
</script>
</body>
</html>
```

##### Servlet代码

输出请求的方式，查看是GET还是POST方式

``` java
System.out.println(request.getMethod());
```

### 小结

1. $.ajax()、\$.get()、\$.post() 三个方法的签名在jQuery3.0中是一样的

2. 后期主要使用get()和post()方法



## 上午的回顾

### jQuery的插件扩展机制

1. 对JQ对象的功能扩展

   ```
   $.fn.extend({函数名:处理函数})
   ```

   

2. 全局的功能扩展

   ```
   $.extend({函数名:处理函数})
   ```

   

### 原生AJAX的访问流程

1. 创建XMLHttpRequest对象
2. 设置回调函数，用来处理服务器返回的数据
3. 发送请求给服务器
4. 服务器处理客户端发送过来的数据，业务处理，数据库访问。
5. 将操作结果发送回客户端
6. 客户端接收到数据，在回调函数中局部刷新网页

### XMLHttpRequest对象

1. 事件

   ```
   onreadystatechange 准备状态发生改变事件
   ```

   

2. 属性

   ```
   readyState 准备状态，等于4表示服务器响应结束
   status 服务器状态码，等于200表示服务器正常响应
   responseText 接收服务器返回的字符串
   ```

   

3. 方法

   ```
   open("GET",url,true) 打开连接
   send() 发送请求
   ```

   

### jQuery中访问Ajax的五个方法

五个方法2种语法，3.0以前，3.0以后

```
$.get(url,data,callback,type)
$.post(url,data,callback,type)
url: 服务器地址
data: 发送的数据
callback: 回调函数
type: 服务器返回的数据
```

```
$.ajax({})
$.get({})
$.post({})
url: 服务器地址
async: 同步或异步
data: 发送的数据
method: 提交方法
dataType: 服务器返回的数据类型
success: 服务器正常响应
error: 服务器出现异常
```



## JSON的格式介绍

### 目标

1. 什么是JSON
2. 典型的格式有哪些

### 什么是json

JSON:  JavaScript Object Notation (JS对象符号)，一种保存数据的格式，简单明了，没有过多多余的数据。

XML

```xml
<contactList>
  <contact>
    <name>潘金莲</name>
    <gender>false</gender>
    <age>18</age>
  </contact>
  <contact>
    <name>武松</name>
    <gender>true</gender>
    <age>20</age>
  </contact>
</contactList>
```

json

```
[{
  name:"潘金莲",
  gender: false,
  age:18
}, {
  name:"武松",
  gender: true,
  age:20
}]
```

### json的语法格式

| 类型          | JS语法                                | 对应Java中对象                           |
| ------------- | ------------------------------------- | ---------------------------------------- |
| 对象类型      | {属性名:值, 属性名:值}                | 对应Java中实体对象                       |
| 数组/集合类型 | [元素,元素,元素]                      | 对应Java中数组或集合                     |
| 混合类型      | [{属性名:值},{属性名:值},{属性名:值}] | 对应Java中集合，集合中每个元素是实体对象 |
|               | {属性名: [元素,元素,元素]}            | 对应Java中Map，值是一个数组或集合        |



### 练习1：创建一个JSON对象类型

#### 步骤

1. 创建这种格式的：{name:value,name:value...} 对象
2. 创建一个person对象的JSON对象，属性名：firstname、lastname、age给三个属性赋值
3. 输出每个属性的值。

### 练习2：创建一个数组，其中每个元素是JSON对象

#### 步骤

1. 创建这种格式的：[{key:value,key:value},{key:value,key:value}]
2. 创建一个数组，包含三个JSON对象
3. 每个JSON对象都有三个属性：firstname、lastname、age。给每个对象的属性赋不同的值
4. 遍历输出每个JSON对象的属性值

### 练习3：创建一个JSON对象

#### 要求

​	某个属性值是一个数组，数组中每个元素是JSON对象

#### 步骤

1. 创建这种格式的对象：{"param":[{key:value,key:value},{key:value,key:value}]}
2. 一个JSON对象，有一个brothers属性，属性值是一个数组
3. 数组中的每个元素是一个JSON对象，属性名为：firstname,lastname,age。
4. 数组中一共4个元素，给每个元素赋值
5. 得到JSON对象的brothers属性，返回一个数组，对数组进行遍历输出每个元素中的firstname和age属性值

#### 效果

![1572851152362](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572851152362.png) 

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSON格式</title>
</head>
<body>
<h2>JSON对象类型</h2>
<script type="text/javascript">
    //创建一个person对象的JSON对象，属性名：firstname、lastname、age给三个属性赋值
    var person = {
        firstname: "孙悟空",
        lastname: "弼马温",
        age: 28
    };

    //document.write(typeof person+ "<br/>");
    document.write("名字：" + person.firstname + " 外号：" + person.lastname + " 年龄:" + person.age + "<hr/>");
</script>

<h2>数组中包含JSON对象</h2>
<script type="text/javascript">
    var persons = [{
        firstname: "孙悟空",
        lastname: "弼马温",
        age: 28
    }, {
        firstname: "猪八戒",
        lastname: "天蓬元帅",
        age: 26
    }, {
        firstname: "沙悟净",
        lastname: "卷帘大将",
        age: 24
    }];
    //使用for-of进行遍历
    for (var person of persons) {
        document.write("名字：" + person.firstname + " 外号：" + person.lastname + " 年龄:" + person.age + "<hr/>");
    }
</script>

<h2>对象的属性中包含了集合</h2>
<script type="text/javascript">
    var master = {
        name: "唐僧",
        age: 20,
        brothers:  [{
            firstname: "孙悟空",
            lastname: "弼马温",
            age: 28
        }, {
            firstname: "猪八戒",
            lastname: "天蓬元帅",
            age: 26
        }, {
            firstname: "沙悟净",
            lastname: "卷帘大将",
            age: 24
        }]
    };

    document.write("姓名：" + master.name + " 年龄：" + master.age + "<br/>");
    document.write("徒弟：" + "<br/>");
    for (var person of master.brothers ) {
        document.write("名字：" + person.firstname + " 外号：" + person.lastname + " 年龄:" + person.age + "<hr/>");
    }
</script>
</body>
</html>
```



## JS中操作JSON的方法

### 目标

JS中与JSON有关的方法

### 语法

| 语法                     | 功能                     |
| ------------------------ | ------------------------ |
| JSON.parse(字符串)       | 将一个字符串转成JSON对象 |
| JSON.stringify(JSON对象) | 将JSON对象转成一个字符串 |

### 案例：parse方法的使用

#### 需求

1. 创建一个字符串：'{"name": "张三", "age": 20}'， 使用上面的方法转成JSON对象

2. 再使用上面的方法，转回成字符串

#### 效果

  ![1572851509769](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572851509769.png)                                                          

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JSON函数</title>
</head>
<body>
<script type="text/javascript">
    //将字符串转成JSON对象，JSON中键必须使用双引号
    var str = '{"name": "张三", "age": 20}';
    document.write("字符串：" + str + ", 类型：" + typeof(str) + "<hr/>");
    //将字符串转成JSON对象
    var obj = JSON.parse(str);
    document.write("对象：" + obj.name + ", " + obj.age + "，类型：" + typeof(obj)+  "<hr/>");

    //将JSON对象转成字符串
    var str = JSON.stringify(obj);
    document.write(obj  + "<br/>");
    document.write(str  + "<br/>");
</script>
</body>
</html>
```

 

### 小结

| 语法                     | 功能                 |
| ------------------------ | -------------------- |
| JSON.parse(字符串)       | 将字符串转成JSON对象 |
| JSON.stringify(JSON对象) | 将JSON对象转成字符串 |



## 将Java对象转成JSON字符串

### 目标

​	学习jackson工具将服务器的集合或对象转成一个JSON字符串

### 引入

#### 需求

​	在服务器端有如下User代码，要转成符合JSON格式的字符串，首先自己采用字符串拼接的方式输出。

#### 运行效果

![1553220609048](/assets/1553220609048.png)

```java
package com.itheima.entity;

public class User {

    private int id;
    private String username;
    private int age;

    public User(int id, String username, int age) {
        this.id = id;
        this.username = username;
        this.age = age;
    }

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

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### 服务器端 json的转换工具

#### json转换工具的概述

json的转换工具是通过java封装好的一些jar工具包，直接将java对象或集合转换成json格式的字符串。

#### 常见的json转换工具

![1553220531567](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553220531567.png)

### 步骤

1. 导入JackSon的jar包

   ![1572852062562](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572852062562.png) 

2. 创建一个对象：ObjectMapper

3. 调用对象的方法：String writeValueAsString(Object Java对象)


### 案例：转换代码实现

#### 步骤

1. 导入上面的三个jar包，复制到WEB-INF\lib目录下
2. 创建一个User类：包含id，username，age三个属性
3. 在Servlet中，实例化User对象，给属性赋值。使用JSON格式打印到浏览器。
4. 创建一个字符串数组对象，包含三个字符串
5. 创建一个List\<User>，包含三个用户对象
6. 创建一个Map对象，键名为：user，键为User对象
7. 实例化ObjectMapper对象，调用writeValueAsString()方法输出转换后的字符串对象

#### 代码

##### 转换代码

```java
package com.itheima.json;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.itheima.entity.User;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * 将Java对象转成JSON字符串
 */
public class TestJson {

    public static void main(String[] args) throws JsonProcessingException {
        User user = new User(100, "关羽", 30);
        //手动转成JSON格式的字符串
        String json1 = "{\"id\":" + user.getId() + ",\"username\": \"" + user.getUsername() + "\",\"age\":" + user.getAge() + "}";
        System.out.println(json1);

        //1.转换1个对象
        //创建ObjectMapper对象
        ObjectMapper mapper = new ObjectMapper();
        String json2 = mapper.writeValueAsString(user);
        System.out.println(json2);

        //2.转字符串的数组
        String[] arr = {"张三","李四","王二"};
        String json3 = mapper.writeValueAsString(arr);
        System.out.println(json3);

        //3. List集合中每个元素是User对象
        List<User> list = new ArrayList<>();
        list.add(new User(100, "关羽", 30));
        list.add(new User(200, "张飞", 23));
        list.add(new User(300, "赵云", 20));
        String json4 = mapper.writeValueAsString(list);
        System.out.println(json4);

        //4. Map数据，键是字符串，值是User对象
        HashMap<String, User> map = new HashMap<>();
        map.put("guan", new User(100, "关羽", 30));
        map.put("zhang", new User(200, "张飞", 20));
        String json5 = mapper.writeValueAsString(map);
        System.out.println(json5);
    }
}
```

### 小结

![1556672144970](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1556672144970.png)



## 案例part1：省市级联

### 目标

1. 流程分析
2. DAO层代码
3. 业务层代码

### 需求

页面上有两个下拉列表框，页面加载的时候访问数据库，得到所有的省份显示在第1个列表框中。当选择不同的省份的时候，加载这个省份下所有的城市显示在第二个下拉列表中。

### 效果

![1563276680217](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1563276680217.png) 

### 分析

![1572854011024](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572854011024.png)

### 表中的数据

pid所属上一级的id，如果没有上一级，pid为0

![1563276767853](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1563276767853.png) 

### 代码

#### sql语句

```mysql
-- 同一张表中包含省和市
create table `area` (
`id` int primary key,
`name` varchar (50),
`pid` int   -- 父级的id，如果为0，表示没有父元素
); 

-- pid为0表示省，否则为某一个省下面的市
insert into `area` (`id`, `name`, `pid`) values(1,'广东省',0);
insert into `area` (`id`, `name`, `pid`) values(2,'湖南省',0);
insert into `area` (`id`, `name`, `pid`) values(3,'广西省',0);
insert into `area` (`id`, `name`, `pid`) values(4,'深圳',1);
insert into `area` (`id`, `name`, `pid`) values(5,'广州',1);
insert into `area` (`id`, `name`, `pid`) values(6,'东莞',1);
insert into `area` (`id`, `name`, `pid`) values(7,'长沙',2);
insert into `area` (`id`, `name`, `pid`) values(8,'株洲',2);
insert into `area` (`id`, `name`, `pid`) values(9,'湘潭',2);
insert into `area` (`id`, `name`, `pid`) values(10,'南宁',3);
insert into `area` (`id`, `name`, `pid`) values(11,'柳州',3);
insert into `area` (`id`, `name`, `pid`) values(12,'桂林',3);

select * from `area`;
```

#### Area实体类

```java
package com.itheima.entity;

/**
 * 区域
 */
public class Area {

    private int id;  //主键
    private String name;  //名字
    private int pid;  //上一级的id

    @Override
    public String toString() {
        return "Area{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pid=" + pid +
                '}';
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

    public int getPid() {
        return pid;
    }

    public void setPid(int pid) {
        this.pid = pid;
    }
}
```

#### 核心配置文件

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
                <property name="url" value="jdbc:mysql://localhost:3306/day31"/>
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

#### 工具类

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

#### AreaDao

```java
package com.itheima.dao;

import com.itheima.entity.Area;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface AreaDao {

    /**
     * 查询指定pid的区域
     */
    @Select("select * from area where pid=#{pid}")
    List<Area> findAreasByPid(int pid);
}
```

#### AreaDaoImpl

```java
package com.itheima.dao.impl;

import com.itheima.dao.AreaDao;
import com.itheima.entity.Area;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class AreaDaoImpl implements AreaDao {
    
    @Override
    public List<Area> findAreasByPid(int pid) {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 得到代理对象
        AreaDao areaDao = session.getMapper(AreaDao.class);
        //3. 调用接口中方法
        List<Area> areas = areaDao.findAreasByPid(pid);
        //4. 关闭会话
        session.close();
        //5. 返回集合
        return areas;
    }
}

```

#### AreaService

```java
package com.itheima.service;

import com.itheima.dao.AreaDao;
import com.itheima.dao.impl.AreaDaoImpl;
import com.itheima.entity.Area;

import java.util.List;

/**
 * 业务层代码
 */
public class AreaService {

    private AreaDao areaDao = new AreaDaoImpl();

    public List<Area> findAreasByPid(int pid) {
        return areaDao.findAreasByPid(pid);
    }

}
```



### 小结

1. DAO：通过pid查询区域
2. Service：调用DAO层查询区域



## 案例part2：省市级联前端

### 目标

1. Servlet的代码
2. 前端ajax的代码

### Servlet

#### 步骤

1. 得到省份pid的值
2. 查询某个省下所有的城市
3. 转成JSON字符串输出到浏览器

#### 代码

```java
package com.itheima.servlet;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.itheima.entity.Area;
import com.itheima.service.AreaService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

@WebServlet("/area")
public class AreaServlet extends HttpServlet {

    private AreaService areaService = new AreaService();

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.得到pid的值
        int  pid = Integer.parseInt(request.getParameter("pid"));
        //2. 调用业务层访问数据库得到List集合
        List<Area> areas = areaService.findAreasByPid(pid);
        //3. 将List集合转成JSON对象
        String json = new ObjectMapper().writeValueAsString(areas);
        //4. 把JSON对象打印到浏览器，告诉浏览这是一个JSON对象
        response.setContentType("application/json;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print(json);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



### html

#### 步骤

1. 加载所有的省份
   1. 页面加载访问服务器，传递的数据是pid=0
   2. 在回调函数中操作，除了第1项option之外删除
   3. 遍历集合，每个元素添加成一个option。值是id属性，文本内容是name属性
2. 使用省份的改变事件
   1. 访问服务器，传递的数据是pid当前选中的值
   2. 在回调函数中操作，除了第1项option之外删除
   3. 遍历集合，每个元素添加成一个option。值是id属性，文本内容是name属性

#### 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>省市级联</title>
    <script src="js/jquery-3.3.1.js"></script>
    <script type="text/javascript">
        //页面加载完毕后执行
        $(function () {
            //1.使用get发送请求，pid=0
            $.get({
                url: "area",
                data: "pid=0",
                success: function (areas) {  //json集合
                    //2.在回调函数中将所有的省份显示在option中
                    $.each(areas, function (index, area) {  //area是一个对象
                        $("#province").append("<option value='" + area.id + "'>" + area.name + "</option>");
                    });
                },
                dataType: "json"  //指定为JSON格式
            });

            //省份的改变事件
            $("#province").change(function () {
                //1.得到当前选项的值
                var pid = $(this).val();
                //2. 访问服务器，将值提交给Servlet
                $.get({
                    url: "area",
                    data: "pid=" + pid,
                    success: function (areas) {
                        //除了第1个option，其它的自杀
                        $("#city>option:gt(0)").remove();
                        //3. 从回调函数中得到所有城市列表
                        $.each(areas, function (index, area) {  //area是一个对象
                            //4. 添加到option中
                            $("#city").append("<option value='" + area.id + "'>" + area.name + "</option>");
                        });
                    }
                });
            });
        });
    </script>
</head>
<body>
<select name="province" id="province">
    <option disabled="disabled" selected="selected">-- 请选择 --</option>
</select>

<select name="city" id="city">
    <option disabled="disabled" selected="selected">-- 请选择 --</option>
</select>
</body>
</html>
```



### 小结

1. 清除除了第1项的所有option：$("#city>option:gt(0)").remove();
2. 添加成最后一个子元素使用：append()



## 案例part1：自动补全项目搭建

#### 目标

1. 自动补全项目搭建
2. DAO层和业务层的编写

### 需求

在输入框输入关键字，下拉框中异步显示与该关键字相关的用户名称

### 分析

![1572857150644](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572857150644.png)

### 步骤

1. 查询以输入的关键字开头的用户
2. 文本框使用keyup事件，得到文本框中的值，去掉前后空格。如果文本框的内容为空则不提交给服务器。
3. 查询成功的回调函数中如果返回的用户数大于0才进行div的拼接，拼接以后显示在div中。
4. 否则要隐藏div。
5. 给大div中的子div绑定鼠标点击事件，点击某个名字则将div的文本内容显示在文本框中，并隐藏div

### 效果

![1553220989638](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553220989638.png)

### 项目结构

![1553221005839](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1553221005839.png) 

### 代码实现

#### 导入数据库脚本

```mysql
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL,
  `password` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`)
);

INSERT INTO `user` VALUES (1, '张三', '123');
INSERT INTO `user` VALUES (2, '李四', '123');
INSERT INTO `user` VALUES (3, '王五', '123');
INSERT INTO `user` VALUES (4, '赵六', '123');
INSERT INTO `user` VALUES (5, '田七', '123');
INSERT INTO `user` VALUES (6, '孙八', '123');
INSERT INTO `user` VALUES (7, '张三丰', '123');
INSERT INTO `user` VALUES (8, '张无忌', '123');
INSERT INTO `user` VALUES (9, '李寻欢', '123');
INSERT INTO `user` VALUES (10, '王维', '123');
INSERT INTO `user` VALUES (11, '李白', '123');
INSERT INTO `user` VALUES (12, '杜甫', '123');
INSERT INTO `user` VALUES (13, '李贺', '123');
INSERT INTO `user` VALUES (14, '李逵', '123');
INSERT INTO `user` VALUES (15, '宋江', '123');
INSERT INTO `user` VALUES (16, '王英', '123');
INSERT INTO `user` VALUES (17, '鲁智深', '123');
INSERT INTO `user` VALUES (18, '武松', '123');
INSERT INTO `user` VALUES (19, '张薇', '123');
INSERT INTO `user` VALUES (20, '张浩', '123');
INSERT INTO `user` VALUES (21, '刘小轩', '123');
INSERT INTO `user` VALUES (22, '刘浩宇', '123');
INSERT INTO `user` VALUES (23, '刘六', '123');
```

#### 编写实体类

```java
public class User {

	private int id;
	private String name;
	private String password;
	
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
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	
}
```

#### UserDao.java

```java
package com.itheima.dao;

import com.itheima.entity.User;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface UserDao {

    /**
     * 查询以什么开头的用户对象
     * @param user
     * @return
     */
    @Select("select * from user where name like concat(#{user},'%')")
    List<User> findUsersByPrefix(String user);
}
```

#### UserDaoImpl.java

```java
package com.itheima.dao.impl;

import com.itheima.dao.UserDao;
import com.itheima.entity.User;
import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class UserDaoImpl implements UserDao {
    @Override
    public List<User> findUsersByPrefix(String user) {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 得到代理对象
        UserDao userDao = session.getMapper(UserDao.class);
        //3. 调用接口中方法
        List<User> users = userDao.findUsersByPrefix(user);
        //4. 关闭会话
        session.close();
        //5. 返回集合
        return users;
    }
}

```

#### UserService.java

```java
package com.itheima.service;

import com.itheima.dao.UserDao;
import com.itheima.dao.impl.UserDaoImpl;
import com.itheima.entity.User;

import java.util.List;

/**
 * 业务层
 */
public class UserService {

    private UserDao userDao = new UserDaoImpl();

    public List<User> findUsersByPrefix(String user) {
        return userDao.findUsersByPrefix(user);
    }
}
```



## 案例part2：自动补全Web层

### 目标

1. 在HTML使用ajax后台访问servlet
2. 读取用户数据，并且更新页面显示，实现自动完成功能

### Servlet

#### 步骤

1. 要将响应的类型设置成：text/json;charset=utf-8
2. 得到客户端提交的用户名
3. 调用业务层查询以这个字符串开头的用户，返回List\<User>
4. 调用ObjectMapper对象中方法将List转成JSON字符串
5. 发送给浏览器

#### 代码

```java
package com.itheima.servlet;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.itheima.entity.User;
import com.itheima.service.UserService;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

@WebServlet("/user")
public class UserServlet extends HttpServlet {

    private UserService userService = new UserService();

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.得到用户名
        String user = request.getParameter("user");
        //2. 调用业务层去查询用户
        List<User> users = userService.findUsersByPrefix(user);
        //3. 转成JSON对象
        String json = new ObjectMapper().writeValueAsString(users);
        //4. 打印到浏览器端
        response.setContentType("application/json;charset=utf-8");
        PrintWriter out = response.getWriter();
        out.print(json);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



### HTML

![1572857839562](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/AJAX&JSON/1572857839562.png)

#### 步骤

1. 编写文本框的keyup事件
2. 得到word文本框的值
3. 去掉值前后的空格，判断值是否为空，如果为空，则返回不再继续。
4. 否则使用$.post方法发送请求给服务器
5. 在success回调函数中得到服务器返回数据：JSON格式用户集合
6. 如果集合不为空，进行字符串拼接。拼接完成以后使用html()方法填充到#show的div中
7. 如果集合为空，则隐藏#show的div

#### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>自动完成</title>
    <style type="text/css">
        .content {
            width: 400px;
            margin: 30px auto;
            text-align: center;
        }

        input[type='text'] {
            box-sizing: border-box;
            width: 280px;
            height: 30px;
            font-size: 14px;
            border: 1px solid #38f;
        }

        input[type='button'] {
            width: 100px;
            height: 30px;
            background: #38f;
            border: 0;
            color: #fff;
            font-size: 15px;
        }

        #show {
            box-sizing: border-box;
            position: relative;
            left: 7px;
            font-size: 14px;
            width: 280px;
            border: 1px solid dodgerblue;
            text-align: left;
            border-top: 0;
            /*一开始是隐藏不可见*/
            display: none;
        }

        #show div {
            padding: 4px;
            background-color: white;
        }

        #show div:hover {
            /*鼠标移上去背景变色*/
            background-color: #3388ff;
            color: white;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.3.1.js"></script>
</head>
<body>
<div class="content">
    <img alt="传智播客" src="img/logo.png"><br/><br/>
    <input type="text" name="word" id="word">
    <input type="button" value="搜索一下">
    <div id="show"></div>
</div>

<script type="text/javascript">
    //文本框松开按键的事件
    $("#word").keyup(function () {
        var word = $(this).val().trim();
        //要判断是否为空
        if (word == "") {
            $("#show").hide();
            return;  //不再继续
        }
        //文本框有值，访问服务器，提交user的值
        $.get({
            url: "user",
            data: "user=" + word,
            success: function (users) {  //集合对象
                if (users.length > 0) {  //判断是否有元素
                    var html = "";
                    for (var user of users) {  //表示每一个user对象
                        //每次创建一个div
                        html += "<div>" + user.name + "</div>";
                    }
                    //放在show中，会覆盖以前
                    $("#show").html(html);
                    //显示div
                    $("#show").slideDown("normal");
                }
                else {
                    //隐藏div
                    $("#show").hide();
                }
            }
        });
    });

    //每个小div绑定点击事件
    $("#show").on("click","div", function () {
       //得到当前点击的文本
        var text = $(this).text();
       //将文本显示在上面文本框
        $("#word").val(text);
       //隐藏show这个div
        $("#show").hide();
    });
</script>
</body>
</html>
```



### 小结

1. 使用文本框的键盘松开事件 keyup

2. $.get() 提交数据给服务器，返回JSON集合

3. 遍历集合，拼接成div的字符串

4. 使用html()方法将拼接好的字符串覆盖div中原有的内容

5. 显示show这个div

    

# 学习总结

1. 能够理解异步的概念

   浏览器与服务器是并行工作的

   

2. 能够了解原生js的ajax

   | 方法                 | 说明                   |
   | -------------------- | ---------------------- |
   | new XMLHttpRequest() | 创建核心请求对象       |
   | onreadystatechange   | 准备状态发生改变的事件 |
   | open("GET",URL,true) | 打开服务器的连接       |
   | send()               | 发送请求               |

3. 能够使用jQuery的\$.get()进行访问

4. 能够使用jQuery的\$.post()进行访问

   | 参数名称 | 解释                               |
   | -------- | ---------------------------------- |
   | url      | 服务器地址                         |
   | data     | 数据：键=值  {键:值}               |
   | callback | 回调函数，参数是：服务器返回的数据 |
   | type     | 返回的数据类型                     |

5. 能够使用jQuery的\$.ajax()进行访问

6. 能够使用jQuery3.0的$.get()新增签名进行访问

7. 能够使用jQuery3.0的$.post()新增签名进行访问

   | 属性名称 | 解释                                       |
   | -------- | ------------------------------------------ |
   | url      | 服务器地址                                 |
   | async    | 异步或同步                                 |
   | data     | 发送的数据                                 |
   | dataType | 返回的数据类型                             |
   | success  | 正常响应的回调函数，参数是服务器返回的数据 |
   | error    | 异常执行的回调函数，参数是XMLHttpRequest   |

8. 能够掌握json的三种数据格式

   | 类型          | 语法                             |
   | ------------- | -------------------------------- |
   | 对象类型      | {键:值}                          |
   | 数组/集合类型 | [元素,元素,元素]                 |
   | 混合类型      | [{},{},{}]<br />{键:[元素,元素]} |

9. 能够使用json转换工具Jackson进行json格式字符串的转换

   | ObjectMapper对象中的方法              | 说明                       |
   | ------------------------------------- | -------------------------- |
   | String writeValueAsString(Object   o) | 将任意的对象转成JSON字符串 |

10. 能够完成用户名是否存在的查重案例

11. 能够完成自动补全的案例