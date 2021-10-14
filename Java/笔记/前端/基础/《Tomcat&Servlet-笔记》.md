# 《Tomcat&Servlet-笔记》

# 回顾

## 正则表达式

1. 如何创建正则表达式

   ```
   new RegExp("正则表达式","匹配模式")
   
   /正则表达式/匹配模式
   ```

   

2. 测试正则表达式是否匹配的方法

   ```
   boolean test("字符串") 如果匹配就返回true
   ```

   



## 什么是响应式设计

同一个网页在不同设备上自动适配宽度和分辨率

 

## Bootstrap的目录结构的作用

```
bootstrap/
	├── css/  bootstrap.css样式文件
	├── js/   JavaScript插件，jquery.js复制在这里
	└── fonts/   字体图标 
```

​     

## 创建模板文件的步骤

1. 复制上面三个目录到项目中
2. 复制jquery.js文件到js目录
3. 复制"起步-->基本模板" 到网页中
4. 修改2个css文件和1个js文件为本地地址

 

## 栅格系统的组成

| 类样式名             | 作用                         |
| -------------------- | ---------------------------- |
| .container           | 容器：固定宽度的容器         |
| .contaner-fluid      | 容器：在所有的设备上都是100% |
| .row                 | 一行                         |
| .col-xs-n            | 一列：在微型设备上一列占n格  |
| .col-sm-n            | 小型                         |
| .col-md-n            | 中型                         |
| .col-lg-n            | 大型                         |
| .hidden-lg/md/sm/xs  | 在指定的设备上隐藏           |
| .visible-lg/md/sm/xs | 在指定的设备上显示           |



## JavaWeb基础

![1571966077343](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571966077343.png) 

1. Web容器入门，Servlet编写
2. 请求和响应对象(方法)
3. Cookie和Session(方法)
4. JSP(表示层技术，动态网页)
5. 过滤器(重点)，监听器(了解)



# 学习目标

1. 概念：
   1. 能够理解软件的架构 
   2. 能够理解WEB资源概念 
2. Tomcat的操作
   1. 能够理解WEB服务器
   2. 能够启动关闭Tomcat服务器 
   3. 能够解决Tomcat服务器启动时遇到的问题 
   4. 能够运用Tomcat服务器部署WEB项目 
3. <font color="red">Servlet(重点)</font>
   1. 能够使用idea编写Servlet
   2. 能够使用idea配置Tomcat方式发布项目
   3. 能够使用注解开发Servlet
   4. 能够说出Servlet生命周期



# 学习内容

## 软件的架构：BS和CS

### 目标

1. 什么是BS
2. 什么是CS

### BS和CS概述

- CS（Client/Server）：客户机服务器模式
- BS（Browser/Server）：浏览器服务器模式


### CS特点

#### 软件

![1552379873269](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552379873269.png) 

#### 大型游戏

![1552379995450](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552379995450.png)

1. 在本地的客户机上必须要安装软件
2. 如果服务器端进行升级，所有的客户端都需要升级安装。
3. 程序的开发工作量主要在客户端

![1552380039467](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552380039467.png)

### BS特点

#### 企业管理软件

![1552380075819](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552380075819.png)

1. 客户端不需要安装，有浏览器就可以了
2. 如果服务器升级，客户端浏览器不需要升级
3. 程序员开发工作量主要服务器端



### 小结

1. 什么是BS？

   浏览器服务器模式

2. 什么是CS？

   客户端服务器模式



## Web资源的分类：静态和动态

### 目标

Web资源分成哪两类

### 分类依据

1. 分类：静态资源和动态资源，运行在静态网站或动态网站上
2. 判断依据是什么？程序是否运行在服务器上，如果运行在服务器上就是动态资源，否则就是静态资源。

前面学习的4天内容都是静态资源，从今天开始学习动态资源。动态技术必须以静态技术为基础。

### 静态网站的特点：

使用的技术：HTML，CSS和JavaScript

这个网页如果制作好了，没有人去更新和修改，永远是这个样子的。

### 动态网站的特点：

1. 运行在服务器上，后台有数据库的。
2. 功能上更加强大，可以实现静态网站不能实现的功能，如：注册，转账，挂号，购物。
3. 我们在浏览器端看到的网页，只是动态程序运行的结果。
4. 使用的技术：JSP，Servlet，C#, PHP

目前很多网站，既用到了静态资源，又用到了动态资源

### 小结

Web资源分成哪两类？

```
静态资源
动态资源
```



## 什么是Web服务器

### 目标

什么是Web服务器

### 什么是Web服务器

1. Web服务器又称为Web容器，是运行在服务器上一种软件。这种软件可以调用其它的应用程序。
2. 可以将服务器上的资源共享给其它的用户
3. 通过浏览器发送请求来访问Web服务器，由Web服务器做出响应来实现浏览器与服务器之间的交互。

### JavaEE规范

Java分为三种版本：

1. JavaSE Java标准版 Java Platform Standard Edition 
2. JavaEE Java企业版  Java Platform Enterprise Edition，又必须以SE为基础
3. JavaME Java移动版 (主要用于嵌入式开发，移动的开发)

在Java中所有的服务器厂商都要实现一组Oracle公司规定的接口，这些接口是称为JavaEE规范。不同厂商的JavaWeb服务器都实现了这些接口，在JavaEE中一共有13种规范。实现的规范越多，功能越强。

### 常见的Web服务器

#### WebLogic

​	WebLogic是Oracle公司的产品，是目前应用最广泛的Web服务器，支持J2EE规范。WebLogic最早由 WebLogic Inc. 开发，后并入BEA 公司，最终BEA公司又并入 Oracle公司。BEA WebLogic是用于开发、集成、集群、部署和管理大型分布式Web应用、网络应用和数据库应用的Java应用服务器。

![1552381082395](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552381082395.png)

####  WebSphere

​	另一个常用的Web服务器是IBM公司的WebSphere，支持JavaEE规范。

![1552380951862](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552380951862.png) 

#### 其它中小型服务器

![1552380988233](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552380988233.png)

#### 今天主角：Tomcat

​	在小型的应用系统或者有特殊需要的系统中，可以使用一个免费的Web服务器：Tomcat。支持的规范：该服务器支持全部JSP以及Servlet规范。

![1552381020336](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552381020336.png)

### 小结

1. Web服务器是什么？

   又称为web容器，我们写的程序运行在web容器中。web容器也是由Java写的

2. 说出三个常用的Web服务器

   1. tomcat
   2. glass fish
   3. weblogic



## 案例：自己模拟一个Tomcat[了解]

### 目标

使用多线程和Socket模拟一个简单的Web服务器，将服务器上的资源共享给浏览器

### 说明

实现的功能：将服务器上的HTML网页分享给用户

使用到技术点：

1. Socket网络编程
2. 多线程 Thread

### 步骤

1. 采用多线程的方法，每个用户创建一个线程。
2. 当有用户连接的时候在服务器控制台输出信息
3. 在线程的run方法中读取本地服务器的资源，得到输入流对象
4. 通过Socket得到输出流，输出到客户端
5. 客户端使用IE浏览器访问

### 执行效果

![1571970541014](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571970541014.png) 

 

### 代码

```java
package com.itheima;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.sql.Timestamp;

/**
 * 模拟tomcat
 * 使用多线程的技术
 */
public class MyTomcat extends Thread {

    private Socket socket;

    //通过构造方法传入Socket对象
    public MyTomcat(Socket socket) {
        this.socket = socket;
    }

    //要执行的任务
    @Override
    public void run() {
        try (//1.读取本地(服务器)上资源，得到文件输入流
             FileInputStream inputStream = new FileInputStream("e:/MyWeb/index.html");
             //2. 通过Socket对象得到网络输出流
             OutputStream outputStream = socket.getOutputStream()) {
            //3. 创建一个字节数组，将输入流写到输出流中去
            byte[] buf = new byte[512];
            int len = 0;
            while ((len = inputStream.read(buf)) != -1) {
                outputStream.write(buf, 0, len);
            }
            //4. 关闭流(自动关闭)
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws IOException {
        //创建服务器端
        ServerSocket serverSocket = new ServerSocket(8080);
        System.out.println(new Timestamp(System.currentTimeMillis()) + " 服务器启动了");
        //不断的接收用的请求
        while(true) {
            Socket socket = serverSocket.accept();  //得到一个客户端
            System.out.println(new Timestamp(System.currentTimeMillis()) + ", " + socket.getInetAddress().getHostAddress() + "，连接了");
            //创建对象，开启事务
            new MyTomcat(socket).start();
        }
    }

}

```

### 小结

以后我们需要自己编写Web服务器吗？

```
不需要，我们只需要使用厂商提供的web服务器即可
```



## <font color="red">Tomcat的安装、配置</font>

### 目标

1. tomcat的安装
2. 环境变量的配置

### 下载页面

tomcat的产品页面： http://tomcat.apache.org/

![1552386691227](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552386691227.png)

### 安装

![1552386751732](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552386751732.png)

1. 不要有中文的目录
2. 目录层级不要太深
3. 直接解压zip包到任意的目录就安装完成

### 回顾

Java环境变量配置：

1. JAVA_HOME=安装目录
2. Path=%JAVA_HOME%\bin;

### Tomcat环境变量的配置

1. 添加Tomcat的安装目录

   ![1552386800008](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552386800008.png)

2. 配置Path，可以在任何路径下访问bin文件夹的可执行文件

   ![1552386873573](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552386873573.png)

### 小结

1. tomcat如何安装

   解压就可以了

2. 需要配置哪两个环境变量

   ```
   CATALINA_HOME=安装目录
   Path=%CATALINA_HOME%\bin;
   ```

   



## Tomcat的启动与关闭

### 目标

1. 启动与关闭tomcat的命令
2. Tomcat每个目录的作用

### 启动与关闭命令

1. 启动的命令：startup.bat

   ![1571971250794](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571971250794.png)

   在浏览器中输入http://localhost:8080  (ROOT目录)

   ![1571971290392](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571971290392.png) 

2. 关闭的命令：shutdown.bat


### Tomcat的目录结构

| 目录名                           | 作用                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| bin                              | 可执行文件所在的目录，如：startup.bat和shutdown.bat          |
| conf                             | 服务器的配置文件，如：server.xml                             |
| lib                              | 第三方的jar包，因为tomcat是用java语言编写                    |
| logs                             | 日志记录目录，可以查询程序执行的情况                         |
| temp                             | 临时目录，中间的文件可以删除。tomcat执行过程中生成的一些垃圾文件 |
| <font color="red">webapps</font> | 项目部署的目录，我们发布的项目放复制在这个目录下。每个目录就是一个项目(模块) |
|                                  | ROOT目录： tomcat默认的管理页面                              |
| work                             | 每个项目在执行过程中生成的一些文件，这是它的工作目录         |

### 小结

1. tomcat的启动和关闭的命令是什么？

   ```
   startup.bat
   shutdown.bat
   ```

   

2. bin和webapps目录的作用是什么？

   ```
   bin：可执行文件所在的目录
   webapps：放置自己写的项目的目录
   ```

   

## Tomcat启动时常见的问题

### 目标

启动时常见的两个问题

### 问题1：未设置JAVA_HOME环境变量

1. 出错信息

![1571973748467](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571973748467.png) 

2. 解决办法

   ```
   配置JAVA_HOME环境变量
   ```

   



### 问题2：端口号被占用

1. 出错信息

![1571973874765](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571973874765.png) 

2. 查看日志文件：

```
25-Oct-2019 11:24:12.994 SEVERE [main] org.apache.coyote.AbstractProtocol.init Failed to initialize end point associated with ProtocolHandler ["http-nio-8080"]
 java.net.BindException: Address already in use: bind
	at sun.nio.ch.Net.bind0(Native Method)
	at sun.nio.ch.Net.bind(Net.java:433)
	at sun.nio.ch.Net.bind(Net.java:425)
```

3. 解决方法：

   1. 方法一：找到占用端口号的程序，进程杀掉。

      ![1552387382007](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552387382007.png)

   2. 方法二：修改tomcat的端口号

      ```
      1. 打开conf/server.xml文件
      2. 修改69行，将端口号换成其它的：8888
      3. 重新启动服务器，新的端口号才起作用
      ```

      ![1571974236899](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571974236899.png) 



### 小结

常见的问题有哪两个？如何解决？

1. JDK没有配置
2. 端口号被占用：
   1. 找到被占用的端口程序，终止
   2. 修改tomcat的端口号



## Tomcat项目发布的三种方式

### 目标

tomcat项目发布的三种方式

### 发布方式1：复制到webapps

#### 操作方法

1. 直接将项目复制到webapps目录下

2. 采用压缩文件.war的方式
   1. 将整个项目使用压缩工具打包成一个zip文件
   2. 改zip的扩展名为war
   3. 复制到webapps目录下，tomcat会自动解压成一个同名的目录。

![1571974586371](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571974586371.png) 



### 发布方式2：虚拟目录

#### 配置方式

server.xml中host元素下

- 优点：不需要将项目复制到webapps下
- 缺点：要修改server.xml，重启服务器，如果配置出错，导致tomcat重启失败

#### 代码

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

#### 浏览器上测试

![1571975166843](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571975166843.png) 



### 发布方式3：独立的XML文件

1. 在tomcat/conf/catalina/localhost中创建xml配置文件
2. 名称假设为：second.xml，这个名称就是项目的访问路径

3. 添加xml文件的内容为

```xml
<Context  docBase="项目所在的目录" />
```

#### 浏览器上测试

![1571975342826](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571975342826.png)

### 小结

方式1：直接复制到webapps

方式2：修改server.xml文件，在host元素下添加添加Context元素，属性：path docBase

方式3：在%CATALINA_HOME%\conf\Catalina\localhost\ 创建1个文件名，文件名就是访问地址，添加Context元素，指定docBase指向真实地址



## 在idea中配置Tomcat

### 目标

1. 在idea中创建一个静态网站
2. 配置tomcat，部署静态网站运行

### idea中配置tomcat

模块结构

![1571975792154](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571975792154.png) 



我们要将idea和tomcat集成到一起，可以通过idea就控制tomcat的启动和关闭

1. 编辑运行配置

   ![1552390374991](/assets/1552390374991.png) 

2. 添加Tomcat的配置服务器信息

   ![1552390386430](/assets/1552390386430.png)

   注：如果看不到Tomcat Server，点下面的33 items more

3. 配置服务器的详细信息

   ![1552390413507](/assets/1552390413507.png)

4. 修改项目发布的访问地址

   ![1552390423678](/assets/1552390423678.png)

5. 选项卡的各项参数说明

   ![1552390434381](/assets/1552390434381.png)

### 在idea中启动服务器

1. 点右上角的启动图标，启动Tomcat服务器

   ![1552390496416](/assets/1552390496416.png) 

   

2. 控制台显示服务器启动的状态信息

  ![1571976136999](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571976136999.png)  

3. 浏览器测试访问

![1571976143433](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571976143433.png) 

### 小结

如果要调试Servlet程序，需要运行在什么模式下？

```
debug模式
```

idea中使用的是哪种方式运行Tomcat

```
第三种，创建了xml文件
```



## 上午回顾

### 概念

1. 软件架构：

   1. BS：Browser Server 浏览器服务器模式，我们的学习方向
   2. CS：Client Server 客户端服务器模式，在本地必须安装软件

2. Web资源的分类：

   区别动态或静态原则：程序是否在服务器端执行，如果运行在服务器端就是动态资源。

   1. 静态：技术HTML, CSS, JavaScript 网页由人去制作，如果没有人修改，网页就不会变化。
   2. 动态：技术Servlet, JSP, PHP等，网页由程序运行出来的结果。

### Web服务器

作用：

1. 用于调用动态网页执行
2. 用于资源共享

也是由Java编写



### Tomcat安装配置

安装：

1. 直接解压到任意的目录
2. 不要用中文目录
3. 目录层次不要太深

### Tomcat启动和关闭

1. 启动：startup.bat
2. 关闭：shutdown.bat

### Tomcat启动时常见问题

1. 没有配置JAVA环境变量，因为Tomcat是用Java，必须运行在Java的JVM中
2. 端口号被占用
   1. 把占用端口的程序kill
   2. 修改tomcat的端口号：server.xml 

###  发布项目的三种方式

1. 直接复制到webapps目录

2. 修改server.xml在host元素下添加1个子元素Context path docBase

3. 在%CATALINA_HOME%\conf\catalina\localhost\文件名.xml 文件名就是访问的地址，内容同上。

   注：idea使用第3种的方式



## 配置文件的方式：创建第1个Servlet

### 目标

使用web.xml配置方式编写第1个Servlet

### 什么是Servlet

本质上就是一个Java类，运行在服务器上小程序，由tomcat来调用。接收用的请求并且处理请求，做出响应。

### Tomcat与Servlet的关系

![1571985966701](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571985966701.png)

### Servlet与普通的Java程序的区别

1. Servlet本质上就是一个Java类，没有main函数，由Tomcat去调用它来执行。
2. 所有的Servlet都必须实现javax.servlet.Servlet接口 
3. 运行在Tomcat中，Web容器。
4. 对浏览器发送的请求做出响应，并且在浏览器端生成动态的网页。

### 编写Servlet的步骤

1. 创建一个类，继承于HttpServlet抽象类，这个类已经实现了Servlet接口。
2. 重写抽象类中doGet和doPost方法，分别用来处理浏览器发送的get或post请求。
3. 需要在web.xml中配置Servlet访问地址和类全名。

### 步骤

1. 创建web工程

   ![1552390081885](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552390081885.png)

2. 通常web结构如下

   ![1552390098127](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552390098127.png)

3. 如果WEB-INF下没有web.xml，在idea使用以下方法添加web.xml。(注：要加web目录)

   ![1552390139905](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552390139905.png)

4. 在com.itheima.servlet包下创建一个类Demo1HelloServlet继承于HttpServlet类，并重写doGet()方法。在浏览器上使用打印流输出Hello Servlet!

注：如果直接在浏览器上输入地址访问Servlet，它调用的是doGet方法

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

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

5. 编辑web.xml中配置servlet，设置访问地址为/demo1

![1571986905212](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571986905212.png) 

### 小结

使用Servlet2.5的方式创建Servlet的主要步骤是什么？

1. 创建一个类继承于HttpServlet
2. 重写doGet或doPost方法
3. 配置web.xml：访问地址和类全名



## <font color="red">Servlet3.0的方式开发Servlet</font>

### 目标

使用注解的方式创建Servlet



### 步骤

1. 创建Servlet类，使用注解@WebServlet

   | @WebServlet注解的属性 | 说明                                                  |
   | --------------------- | ----------------------------------------------------- |
   | name                  | Servlet的名字，相当于配置文件中servlet-name(可以省略) |
   | urlPatterns           | 访问地址，相当于配置文件中url-pattern                 |
   | value                 | 默认属性，指定访问地址。                              |


​    

2. 代码：

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

3. 点右上角启动按钮执行

   ![1552391203018](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552391203018.png) 

4. 浏览器上执行效果

![1571987558080](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571987558080.png) 

### 小结

说说使用Servlet3.0编写Servlet的步骤

1. 编写一个类继承于HttpServlet
2. 重写doGet或doPost方法
3. 使用注解@WebServlet("/访问地址")或@WebServlet(name = "demo2", urlPatterns = "/demo200")



## <font color="red">Servlet的生命周期</font>

### 目标

Servlet生命周期有哪些方法

### Servlet接口中的方法

| 方法                                                   | 作用         | 运行次数                                |
| ------------------------------------------------------ | ------------ | --------------------------------------- |
| void init(ServletConfig   config)                      | 初始化的方法 | 第1次有用户访问这个Servlet的时候执行1次 |
| void  service(ServletRequest req, ServletResponse res) | 服务的方法   | 每次用户发送请求都会执行这个方法        |
| void  destroy()                                        | 销毁的方法   | 在服务器关闭的时候执行1次               |

### Servlet的生命周期

注：一个Servlet在tomcat容器中只会实例化一次，只会产生一个对象，而且常驻内存。要等到服务器关闭才会销毁。

![1571988229503](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571988229503.png)



### 生命周期演示案例

#### 步骤

1. 创建一个类实现Servlet接口
2. 重写接口中所有的方法，其中service方法用于处理用户的请求
3. 使用@WebServlet配置访问地址

#### 执行结果

![1571988715013](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571988715013.png) 

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.sql.Timestamp;

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

#### 疑问：service方法中输出request和response对象，这两个对象从哪里来的？

```
由tomcat实现，并且由tomcat实例化这2个对象。
```



### 小结

1. Servlet是什么时候实例化的？

   ```
   第1次用户访问的时候实例化，由tomcat实例化
   ```

   

2. 一个Servlet在Tomcat中会生成几个对象？

   ```
   生成一个对象，并且常驻内存
   ```

   

## Servlet映射多个路径

### 目标

同一个Servlet可以配置多个不同的访问地址

### 配置方式

#### 方式1

一个\<servlet-mapping>中写多个\<url-pattern>

```xml
   <servlet-mapping>
        <!--与上面的名字对应-->
        <servlet-name>demo1</servlet-name>
        <!--访问地址，必须以/开头, /相当于web目录 -->
        <url-pattern>/demo1</url-pattern>
        <url-pattern>/demo1000</url-pattern>
    </servlet-mapping>
```



#### 方式2

一个\<servlet>对应多个\<servlet-mapping>

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

​           

### 注解方式

```
@WebServlet(name = "demo2", urlPatterns = {"/demo2", "/demo2000"} )
```



### 小结

1个Servlet可以有多个访问地址





## url-pattern的通配符

### 目标

学习使用通配符的方式来配置访问地址

### 介绍

1. 什么是通配符： *

​     一个配置，可以通配多个访问地址

2. 两种通配符的格式:

| 通配符格式 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| /开头      | /* 匹配所有的资源，不建议这么写，会导致所有的资源都不能访问。 |
|            | /目录/* 访问这个目录所有的资源都会访问这个Servlet            |
| *.扩展名   | 匹配某个扩展名结尾的访问地址                                 |

问：能否2种格式同时出现呢？

```
/admin/*.jsp
Invalid <url-pattern> /admin/*.jsp in servlet mapping
结论：以/开头的格式，以扩展名结尾的格式，不能同时出现
```

### 演示案例

```
//@WebServlet("/demo2")   //Invalid <url-pattern> demo2 in servlet mapping
//@WebServlet(name = "demo2", urlPatterns = {"/demo2", "/demo2000"} )
//@WebServlet(name = "demo2", urlPatterns = "/admin/*" )
//@WebServlet(name = "demo2", urlPatterns = "*.jsp" )
@WebServlet(name = "demo2", urlPatterns = "/admin/*.jsp" )  //Invalid <url-pattern> demo2 in servlet mapping
```



### 面试题

创建2个Servlet，一个Servlet1，一个Servlet2，在下列情况下，访问哪个Servlet

| 请求URL       | Servlet1 | Servlet2 | 访问哪个 |
| ------------- | -------- | -------- | -------- |
| /abc/a.html   | /abc/*   | /*       | Servlet1 |
| /abc          | /abc/*   | /abc     | Servlet2 |
| /abc/a.do     | /abc/*   | *.do     | Servlet1 |
| /a.do         | /*       | *.do     | Servlet1 |
| /xxx/yyy/a.do | /*       | *.do     | Servlet1 |

### 小结

- 优先级：以/开头的高于以扩展名结尾
- 匹配原则：哪个更接近就匹配哪个，最佳匹配原则。



## 继承于GenericServlet[了解]

### 目标

通过继承于GenericServlet编写Servlet

### Servlet继承结构

![1552393738408](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552393738408.png)

### 继承GenericServlet实现Servlet

#### 步骤

1. 创建一个类继承于GenericServlet
2. 重写的方法是：service()，测试：输出request和response对象
3. 使用注解@WebServlet设置访问地址

#### 效果

![1571991542617](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571991542617.png) 

#### 代码

```java
package com.itheima.servlet;

import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * 1. 编写一个类继承于GenericServlet
 * 2. 重写抽象方法：service()，处理用户的请求
 * 3. 使用注解@WebServlet来配置访问地址
 */
@WebServlet("/demo4")
public class Demo4Servlet extends GenericServlet {
    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        //1. 设置响应类型
        response.setContentType("text/html;charset=utf-8");
        //2. 得到打印流输出
        PrintWriter out = response.getWriter();
        out.print("你好，Servlet，I am coming!");
    }
}

```

### 小结

service方法中request和response对象是接口，它的实现类从哪里来的？

```
由tomcat来实现
```



## HttpServlet的源码分析[了解]

### 目标

![1552394436903](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552394436903.png)

### 代码

```java
//这是一个抽象类，它的父类是GenericServlet，并且已经实现了Servlet接口
public abstract class HttpServlet extends GenericServlet {

	//定义请求方式的字符串：GET或POST
    private static final String METHOD_GET = "GET";
    private static final String METHOD_POST = "POST";

	//HttpServlet自己新增的方法，这个方法就是抛出错误，说明子类一定要重写这个方法。
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_get_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }

    }

	//说明子类一定要重写这个方法，处理post的请求的
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_post_not_supported");
        if (protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }
    }

	//不是父类中的方法，这自己新增的方法。参数是：HttpServletRequest 它是 ServletRequest 的子接口
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();   //得到请求的方式：GET或POST
        long lastModified;
        if (method.equals("GET")) {   //判断是什么请求方式，再调用相应的方法
            this.doGet(req, resp);
        } else if (method.equals("POST")) {
            this.doPost(req, resp);
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }

	//这也是生命周期的方法，由tomcat去调用
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request;   //声明2个子接口
        HttpServletResponse response;
        try {
            request = (HttpServletRequest)req;   //向下转型，将父接口转成子接口
            response = (HttpServletResponse)res;
        } catch (ClassCastException var6) {
            throw new ServletException("non-HTTP request or response");
        }

        this.service(request, response);   //调用上面的方法
    }
}

```



##### 疑问：什么时候重写doGet()方法？什么时候重写doPost()方法？

```
只有form method="post"，使用post方法提交，我们就重写doPost方法，其它所有的情况都重写doGet
```



### 小结：service()方法的作用

![1571992527495](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571992527495.png)





## Servlet的执行流程

### 目标

了解tomcat的执行过程

### Tomcat的执行过程

![1552394697006](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1552394697006.png)

### 执行步骤

1. 通过浏览器发送请求http://服务器:端口号/项目地址/Servlet地址
2. 项目地址找到tomcat中指定的项目
3. 通过Servlet地址找到项目中相应的Servlet
4. 通过反映对这个Servlet进行实例化的操作
5. tomcat创建request和response对象
6. 调用service()方法传入请求和响应对象
7. 由service()方法调用doGet或doPost方法
8. 处理完毕以后返回到浏览器端



### 小结

1. Servlet对象是通过什么技术实例化的？

   ```
   反射
   ```

   

2. 我们在浏览器上看到的动态网页是怎么来的？

   ```
   由Servlet执行结果生成的
   ```

   



## ServletConfig接口[了解]

### 目标

ServletConfig的作用和方法

### 作用

读取web.xml配置文件中配置参数



之前出现在哪个方法中？

```
init(ServletConfig config)
```



### 为什么要配置初始化参数

如果Servlet中有一些配置的参数，直接写死在源代码中，以后修改就不灵活。可以将一些配置参数放在web.xml文件中。通过ServletConfig接口提供的方法来读取这些配置参数。



### 接口中方法

| ServletConfig接口中的方法                    | 功能                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| String getInitParameter("参数名")            | 通过参数名来读取相应的参数值<br />参数名和参数值都在web.xml配置文件中 |
| Enumeration\<String> getInitParameterNames() | 获取所有配置参数的名字，返回枚举接口                         |

| Enumeration接口的方法     | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
| boolean hasMoreElements() | 测试此枚举是否包含更多的元素。                               |
| E nextElement()           | 如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。 |



###  案例：读取配置文件中信息

#### 需求

1. 在web.xml中创建一个Servlet
2. 配置姓名和年龄2个属性
3. 读取信息，在Servlet上显示出来
4. 输出所有配置信息的名字和值

#### 代码

web.xml

``` xml
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

servlet代码

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;

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

#### 效果

 ![1571994644700](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Tomcat&Servlet/1571994644700.png) 

### 小结

| ServletConfig接口中的方法                    | 功能                         |
| -------------------------------------------- | ---------------------------- |
| String getInitParameter("参数名")            | 通过参数名得到初始化的参数值 |
| Enumeration\<String> getInitParameterNames() | 得到所有的初始化的参数名     |



## load-on-startup参数

### 目标

学习load-on-startup参数使用

### 作用

1. Servlet什么时候实例化对象？

   ```
   用户第1次访问这个Servlet的时候
   ```

2. 如果希望tomcat一启动就实例化Servlet可不可以？

   ```
   在servlet中配置load-on-startup参数即可
   ```

   

3. 参数的使用说明：

   ```
   配置文件：
   <load-on-startup>1</load-on-startup>
   必须设置为整数，数字越小越先加载。最小是0，如果设置为负数无效
   
   使用注解：
   @WebServlet(urlPatterns = "/demo2", loadOnStartup = 0)
   ```



### 案例

#### 需求      

在一个Servlet中的init方法中直接输出一句话到控制台上，比较加不加的\<load-on-startup>的区别

#### 代码

web.xml

```xml
    <servlet>
        <!--servlet的名字-->
        <servlet-name>demo1</servlet-name>
        <!--类全名-->
        <servlet-class>com.itheima.servlet.Demo1Servlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

servlet

```java
package com.itheima.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

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

### 小结

```
如果需在服务器启动的时候就加载某个Servlet，需要指定哪个参数？
load-on-startup
```



 

# 学习总结

1. 能够理解软件的架构 

   1. BS：浏览器服务器
   2. CS：客户端服务器

2. 能够理解WEB资源概念

   1. 动态

   2. 静态

      程序是否运行在服务器端，如果运行在服务器它就是动态资源 

3. 能够理解WEB服务器

   1. 作用：
      1. 调用我们写的Servlet程序
      2. 实现资源的共享
      3. 也是Java写的，运行在JVM
   2. 我们使用：Tomcat

4. 能够启动关闭Tomcat服务器 

   1. 启动：startup.bat
   2. 关闭：shutdown.bat

5. 能够解决Tomcat服务器启动时遇到的问题 

   1. JDK没有配置环境变量
   2. 端口号被占用

6. 能够运用Tomcat服务器部署WEB项目 

   1. 直接复制到webapps
   2. 修改server.xml配置文件，添加Context path docBase
   3. 在tomcat/conf/catalina/localhost配置1个xml文件，文件名就是访问地址，内容是Context

7. 能够使用idea编写Servlet

   1. 创建一个类继承于HttpServlet
   2. 重写doGet或doPost
   3. 使用配置web.xml文件或@WebServlet注解，配置访问地址

8. 能够使用idea配置Tomcat方式发布项目

9. 能够使用注解开发Servlet

   | @WebServlet属性 | 说明                                   |
   | --------------- | -------------------------------------- |
   | name            | 名字                                   |
   | urlPatterns     | 访问地址：可以使用多个，可以使用通配符 |
   | value           | 同上                                   |

10. 能够说出Servlet生命周期

| 方法                                                   | 作用         | 运行次数         |
| ------------------------------------------------------ | ------------ | ---------------- |
| void init(ServletConfig   config)                      | 初始化的方法 | 1次              |
| void  service(ServletRequest req, ServletResponse res) | 服务方法     | 每次请求都会执行 |
| void   destroy()                                       | 销毁方法     | 1次              |

