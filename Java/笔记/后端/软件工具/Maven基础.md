# Maven基础

# 回顾

## 网络管理命令

| 命令                               | 作用             |
| ---------------------------------- | ---------------- |
| hostname                           | 显示主机名       |
| hostnamectl  set-hostname 新主机名 | 永久的修改主机名 |

| netstat [参数] | 功能                       |
| -------------- | -------------------------- |
| 无参           | 查看访问网络的进程         |
| -n             | number  显示IP地址和端口号 |
| -t             | tcp 只显示TCP协议          |
| -l             | listener 显示监听状态      |
| -p             | programs 显示程序名和PID   |

## firewall-cmd

| firewall-cmd           | 添加端口号到防火墙中                 |
| ---------------------- | ------------------------------------ |
| --zone=public          | public: 添加外网<br />internal: 内网 |
| --add-port=端口/tcp    | 添加端口号                           |
| --remove-port=端口/tcp | 删除端口号                           |
| --permanent            | 永久添加                             |
| --list-all             | 显示防火墙状态                       |
| --reload               | 重新加载规则                         |

## SSH登录

| 命令                    | 说明                     |
| ----------------------- | ------------------------ |
| ssh 服务器地址          | 登录另一台服务器         |
| ssh-keygen              | 生成一对公钥和私钥       |
| ssh-copy-id  服务器地址 | 把公钥发送给指定的服务器 |

## 安装有关的命令

| 命令             | 说明                                   |
| ---------------- | -------------------------------------- |
| yum -y   install | 在线安装软件<br />-y 所有的提问回答yes |

```
rpm 离线安装rpm的文件
```



## 用户管理的命令

| 命令     | 作用             |
| -------- | ---------------- |
| useradd  | 添加用户         |
| passwd   | 密码             |
| userdel  | 删除用户         |
| usermod  | 修改用户         |
| groupadd | 添加组           |
| gpasswd  | 将用户从组中删除 |
| groupmod | 修改组名字       |
| groupdel | 删除组           |
| su       | 切换用户         |
| sudo     | 执行管理员权限   |

 

# 学习目标

1. maven的基本使用
   1. 能够了解Maven的作用
   2. 能够理解Maven仓库的作用
   3. <font color="red">能够理解Maven的坐标概念</font>
   4. 能够掌握Maven的安装
2. 在idea中使用maven来创建web工程
   1. <font color="red">能够掌握IDEA配置本地Maven</font>
   2. 能够使用IDEA创建JavaWeb的Maven工程
   3. <font color="red">能够自定义JaveWeb的Maven工程和使用插件优化项目目录结构</font>
3. 能够掌握依赖引入的配置方式
4. 能够了解依赖范围的概念



# Maven概述和作用

## 目标

1.     为什么要使用maven 

2.     maven有什么作用

## 为什么要使用Maven

![1553676070115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676070115.png)                                                   

### 以前项目中jar包会导致如下的问题：

1. 每个模块都需要导入jar包，会出现相同的jar包多次导入，出现包的冗余，出现大量重复的包。
2. 同一个开发小组中的成员使用了同一个工具包，但可能使用了不同的jar版本。
3. 使用某个jar包，这个jar包依赖了其它的jar包，依赖了哪些包我们并不知道。

## Maven作用

1. 对项目中的jar包进行统一的管理
2. 如果使用maven开发项目，所有的项目都有一个统一的项目开发阶段，统一的生命周期。
3. 对一些大型的项目进行管理，将一个大项目拆分成多个子模块来开发。

## 小结

基础部分学习的内容：

1. jar包的管理
2. 项目的生命周期管理
3. 使用maven创建web项目

 

# Maven的安装

## 目标

1. 下载和安装maven

2. 配置环境变量

## 下载Maven

   ![1553676497053](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676497053.png)        

## 安装Maven

   ![1553676503816](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676503816.png)        

安装直接解压即可

1. 不要有中文
2. 目录层次不要太深



## Maven目录介绍

| 目录名 | 功能说明                                                |
| ------ | ------------------------------------------------------- |
| bin    | 可执行文件所在的目录，其中mvn.cmd是maven的执行文件      |
| boot   | 第三方的jar包，类加载器，类似于Java中ClassLoader        |
| conf   | maven的配置文件所在目录，其中settings.xml是它的配置文件 |
| lib    | maven也是用Java语言编写的，这下面就是它的依赖包         |



## 配置环境变量

1)  我的电脑右键，选择属性

![1553676545810](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676545810.png)    

 2)  左上角选择"高级系统设置"

![1553676564030](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676564030.png)       

3)     选择高级下面的"环境变量"的按钮

![1553676572365](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676572365.png)    

4)     添加Maven的主目录为你的安装目录，设置Maven的可执行文件访问路径为Maven主目录的bin目录下

```
MAVEN_HOME=e:\apache-maven-3.3.9
Path=%MAVEN_HOME%\bin
```

![1553676610398](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676610398.png)   



## 测试安装好的Maven

出现如下提示信息表示配置成功：

![1553676631754](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553676631754.png)   

## 小结

maven的安装要配置哪2个环境变量

1. MAVEN_HOME
2. Path

 

# Maven仓库的概念和配置

## 目录

1.     什么是POM

2.     三种仓库的特点

3.     配置本地仓库

## 什么是POM

Project Object Model 项目对象模型，以面向对象的方式来管理项目。

核心是一个pom.xml文件。这个文件中包含如下信息：

1. 当前项目坐标
2. 项目的信息：JDK版本，网站，项目名字等
3. 当前项目依赖的jar包
4. 当前项目中使用了哪些第三方插件



## Maven的仓库概念

以后开发项目都不再需要自己复制jar包到lib目录中，以后所有的jar包都从仓库中去取。maven仓库就是很多的jar包。通常分成以下三种仓库：	

### 三种仓库

![1553677076418](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677076418.png) 

### 访问仓库的过程

![1553677159162](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677159162.png)   

 

### 配置本地仓库

1)  将软件文件夹中的Repository解压，可以放在任意的位置，这是老师提供的本地仓库。

```
e:\repository
```

2)  配置本地仓库，修改maven的安装目录中conf/settings.xml文件，在53行配置本地仓库为上面的目录。

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <localRepository>e:\repository</localRepository>
</settings>
```

 3)  可选配置，修改settings.xml文件，159行指定中央仓库的镜像。这里使用的是阿里云的中央仓库，速度比官方的快很多。

```xml
<mirror>  
	<id>nexus-aliyun</id>  
	<mirrorOf>central</mirrorOf>    
	<name>Nexus aliyun</name>  
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>  
</mirror>
```

4) 可选配置，修改maven使用JDK的版本，200行。如果不配置就需要在idea中配置。maven默认使用的是JDK1.5的版本，这里使用1.8的版本。

```xml
<profile>    
    <id>jdk-1.8</id>    
     <activation>    
          <activeByDefault>true</activeByDefault>    
          <jdk>1.8</jdk>    
      </activation>    
	<properties>    
		<maven.compiler.source>1.8</maven.compiler.source>    
		<maven.compiler.target>1.8</maven.compiler.target>    
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>    
	</properties>    
</profile>
```



## 小结

1. 什么是POM？

   Project Object Model，每个项目都有一个pom.xml

2. 有哪三种仓库

   本地，中央，私服

3. settings.xml需要配置哪几项？

   1. 本地仓库目录
   2. 中央仓库的镜像
   3. 指定JDK的版本



# Maven的坐标

## 目标

1.     坐标的作用

2.     定义坐标有哪些元素

## Maven坐标的概念

在成千上万个jar包中快速的定位到某个我们需要的jar包，就需要使用坐标。

## 坐标的元素定义   

| 元素名称   | 说明(使用这三个元素来定位jar包所在的目录) |
| ---------- | ----------------------------------------- |
| groupId    | 代表一级或多级目录                        |
| artifactId | 代表groupId下一级目录                     |
| version    | 代表artifactId下一组目录                  |

### 起名的规则

![1553677429064](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677429064.png)  

## 坐标的示例

​	例如：要引入junit的jar包，只需要在pom.xml配置文件中配置引入junit的坐标即可：

​	可以通过官方的中央仓库地址：http://repo1.maven.org/maven2/

![1553677453346](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677453346.png)   

​	通过以上三层结构就可以在仓库里面定位到一个jar包，坐标的每个组成部分都对应仓库里面的一级目录结构。如果之前已经访问过，则在本地仓库中也可以找到。

​	有一些groupId项目名字对应两级目录，而artifactId和version元素都对应一级目录

![1553677473658](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677473658.png)   

## 小结

1. maven中坐标的作用？

   从仓库中定位一个jar包

2. 定义坐标有哪几个元素

   1. groupId
   2. artifactId
   3. version

 

# 演示：IDEA中配置Maven环境

## 目标

在idea中配置maven的环境

## IDEA绑定本地Maven服务器

1)     选择File-->Other Settings   ![1562466004194](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1562466004194.png) 

2)     选择Build,Execution,Deployment-->Build Tools-->Maven 

![1553677574065](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677574065.png)    

3)   配置以下三项信息：Maven的主目录，配置文件settings.xml和本地仓库repository目录

![1553677590028](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677590028.png)           

4)   选择Build,Execution,Deployment-->Build Tools -->Maven-->Runner，设置Maven启动虚拟机的选项：VMOption，设置所有资源先从本地仓库查找，如果本地仓库中没有才去互联网找。

```
-DarchetypeCatalog=internal
```

![1553677618970](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677618970.png) 

5)   可选，如果汉字没有乱码则不用设置，Maven控制台输出中文如果出现乱码，则进行如下设置。

```
VMoptions: -Dfile.encoding=GBK  或者 -Dfile.encoding=UTF-8
```

VMOptions配置多个参数需要使用空格隔开



## 小结

在idea中配置maven环境需要指定哪些选项?

1. 指定maven的安装目录
2. 指定配置文件conf/settings.xml
3. 指定本地仓库的目录



# 演示：使用Maven骨架创建JavaWeb工程

## 目标

使用骨架创建JavaWeb工程

## Maven对JavaWeb工程目录结构的要求

![1556673655696](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1556673655696.png)   

 

## 使用骨架创建JavaWeb工程

1)   选择Maven骨架时，要选择maven-archetype-webapp，注：不要选成cocoon-22-archetype-webapp。

![1553677953704](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677953704.png)            

 

2)   将模块添加到哪个项目，指定其父模块，都设置为none。指定项目的坐标。

![1553677958738](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677958738.png)            

 

3)  确认Web的骨架信息，不用修改，直接选择下一步。

![1553677964857](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677964857.png)            

 

4)  指定模块名和保存模块所在的目录

![1553677970852](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677970852.png)            

 

5)  点finish按钮，运行Maven构建web项目，这时要连接网络，一段时间后显示Maven exeution finished。

![1553677989058](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677989058.png)            

 

6)  默认骨架创建好的JavaWeb工程如下，选右边的Maven Projects中的第2个项目刷新。左边的webapp图标上出现蓝色小点。表示这是Web页面的根目录，类似于以前的web目录。

![1553677998679](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553677998679.png)            

![1553678003285](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678003285.png)            

   

## 手动完善JavaWeb的目录结构

1)     手动在main目录下创建java目录和resources，如图

![1553678019057](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678019057.png)            

 

2)     在src目录下，与main目录同级，创建test目录，并在test目录下创建java文件夹和resources，如图

![1553678023703](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678023703.png)            

 

3)     在右边的Maven Projects刷新，左边的工程各个文件夹的图标都发生变化

![1553678028780](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678028780.png)            

 

4)     最终目录结构，如图红色方框中的文件夹都是手动创建的。

 ![1553678034328](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678034328.png)           

## 小结

使用骨架(原型)创建web项目注意事项：

1. 需要连接互联网，因为骨架的xml文件在服务器上
2. 需要手动完善目录结构：main下只有一个webapp，手动创建java和resources。test可选。
3. 在右边要刷新



# 在项目中创建Servlet

## 目标

创建Servlet并且运行

## 步骤

1)     打包的类型已经自动设置成了war，修改JDK默认的编译版本为1.8

```xml
<groupId>com.itheima</groupId>
<artifactId>day39-02-JavaWeb1</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>war</packaging>

<name>day39-02-JavaWeb1 Maven Webapp</name>
<url>http://www.example.com</url>

<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

 

2)     在pom.xml加入依赖Servlet的依赖，artifactId设置为javax.servlet-api，选择3.1.0的版本，指定scope为provided

```xml
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
  </dependency>

    <!--指定Servlet的依赖包-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

 

3)     刷新maven project窗口，导入Servlet 3.1.0的依赖包。

![1553678174224](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678174224.png)            

如果没有导入jar包，可以在事件日志中设置为自动导入

![1553678179040](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678179040.png)            

![1553678184408](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678184408.png)            

 

4)     创建Servlet，注：要勾选使用注解的方式。注：如果webapp文件夹上如果没有蓝色小点，则在上面右键是不能选择Servlet的。

![1553678190302](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678190302.png)            



## 更新本地仓库的索引

作用：在pom.xml文件中，导入依赖的jar包的时候有提示

## ![1570589963565](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1570589963565.png) 



## 发布Web工程

1)     点上面的执行工具栏，编辑运行配置

 ![1553678689184](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678689184.png)           

2)     点Deployment，删除原有的项目，添加一个新的Artifact

![1553678696901](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678696901.png)            

 

3)     选择两个都可以，区别是：exploded是未打包成war的文件夹，访问速度更快一点，适合于开发阶段。

![1553678702221](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678702221.png)            

 

4)     Application context的访问地址可以不变，设置成/

![1553678707553](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678707553.png)            

 

5)     Server标签页的设置，这里默认的端口号是80

 ![1553678712471](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678712471.png)           

 

6)     注意Tomcat默认访问的是index.jsp，要输入地址访问demo1

![1553678718078](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678718078.png)            

 

## 在线查询依赖的写法

在maven中只需要导入依赖坐标，无需手动导入jar包就可以引入jar。在pom.xml中使用\<dependency>标签引入依赖。如果pom模板中没有配置过的依赖，可以通过http://mvnrepository.com/ 在线搜索坐标写法。

 

1)     输入网址，在文本框中输入要查找的包名，如：servlet

![1553689131430](/assets/1553689131430.png)            

 

2)     找到相应的结果，选择自己想要的jar

![1553689136318](/assets/1553689136318.png)            

 

3)     点进去以后，选择相应的Servlet版本，如：3.1.0

![1553689142040](/assets/1553689142040.png)            

 

4)     将相应的Maven依赖文本复制过来即可

 ![1553689146427](/assets/1553689146427.png)         



## 小结

在项目中创建Servlet的步骤

1. 需要在pom.xml文件中导入servlet的jar包
2. 编写servlet
3. 手动在tomcat中部署
4. 运行，如果不会写jar包，可以在线去查询



# <font color="red">演示：使用插件来创建JavaWeb工程</font>

## 目标

1. 插件的安装

2. 使用插件创建Web工程

## idea插件安装

1. 将插件“JBLJavaToWeb.zip”拷贝到idea安装目录(C:\IntelliJ IDEA\plugins，我安装到了C盘，根据自己具体安装位置确定)

  ![1553678979131](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553678979131.png)          

 

2. 打开idea开发工具，如图安装插件

![1562466050150](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1562466050150.png)        

3. 重启idea

4. 查看是否安装从插件成功

![1553679014990](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679014990.png)   

 

## 创建Web项目

1. 创建maven模块

![1553679038242](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679038242.png)   

2. 创建工程坐标

![1553679055656](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679055656.png)           

3. 创建工程本地存储目录

![1553679067258](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679067258.png)   

 

4. 使用插件转换为web项目

![1553679083710](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679083710.png)   

 

5. 出现如下中文提示

![1553679099519](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679099519.png)   

 

6. 点击项目同步

![1553679115908](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679115908.png)           

 

7. 最后使用插件生成目录效果如下

![1553679133353](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679133353.png)    



## 小结

1. 插件的安装，将Java项目转成Web项目
2. 使用插件创建Web工程
   1. 不使用骨架创建Java项目
   2. 项目上点右键转成Web项目
   3. 编写Servlet

 

# maven的生命周期：clean、test、compile

## 目标

1. 了解maven的生命周期

2. 学习生命周期的命令：clean、test、compile

## 生命周期的概念和组成

什么是生命周期：软件开发过程中各个阶段，软件开发开始到软件开发结束的整个过程。

![1553679264114](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679264114.png) 

   

## Maven生命周期命令

以下所有的操作针对第1个通过向导创建的Web项目

## clean命令

### 命令作用

target目录是项目执行过程中生成的文件，如：字节码文件，项目部署的目录。

清除target目录

### 操作演示

1)     选择day39_02_Web项目，可以看到target文件夹

![1553679321366](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679321366.png)            

 

2)     双击右边的Maven Projects-->Lifecycle-->clean执行清除操作

![1553679325845](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679325845.png)            

 

3)     操作结果如下 

![1553679330487](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679330487.png)            

 

4)     可以看到删除了target目录。

![1553679335322](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679335322.png)            

 

5)     也可以选择Execute Maven Goal选择clean命令执行，注意选择正确的路径。这种方式主要用于命令带参数的情况。

![1553679348143](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679348143.png)            

 

## test命令

### 命令作用

 执行test目录下所有的测试类和测试方法，注：类名和方法应该以test开头

有可能汉字会出现乱码，如果有乱码，就VM Options中设置编码

### 操作演示

1)     确认JUnit已经导入

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
</dependency>
```

 

2)     在src/test/java下创建测试类AppTest

```java
package com.itheima.test;

import org.junit.Test;

public class TestOne {

    @Test
    public void test1() {
        System.out.println("测试方法1");
    }

    @Test
    public void test2() {
        System.out.println("测试方法2");
    }

}
```

 

3)     执行test命令测试，双击或输入运行的test命令

![1553679404886](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679404886.png)            

 

4)     在Run查看测试结果

![1553679408957](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679408957.png)            

- 如果有多个测试类，每个测试类中有多个方法，每个测试方法都会执行。




## compile命令

### 命令作用

 只编译main目录下的源代码，生成字节码文件

### 操作演示

1)     选择day39_02_Web项目，删除target中classes目录下和test-classes目录下所有的文件，不删除目录。

![1553679429356](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679429356.png)    

 

2)     双击compile命令执行 

![1553679433848](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679433848.png)            

 

3)     在Run下查看结果 

![1553679438499](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679438499.png)            

 

4)     只编译了java中的代码，test-classes中的代码没有编译。

![1553679442632](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553679442632.png)    



## 小结

1. clean：清除target目录

2. test：执行test目录下测试类和方法

3. compile：编译main目录下源代码



# maven的生命周期：package，install

## 目标

1.     学习使用package和install命令

2.     在命令行方式下使用生命周期的命令

## package命令

### 命令作用

 将当前的项目打包，打包到target目录下。如果是Java项目打包成jar包，如果是web项目打包成war包。需在pom.xml文件中使用packaing元素来指定打包的类型

打包前还会执行编译和测试的命令，如果前面的命令执行失败，则打包也失败。

### 操作演示

1)     确认在Web项目中已经设置了打包的方式

```
<packaging>war</packaging>
```

 

2)     双击package或者输入命令执行

 ![1553688524317](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688524317.png)           

 

3)     在Run查看日志输出结果。注：每次打包前还会再次执行所有test命令

![1553688531279](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688531279.png)            

 

4)     在target或windows目录下都可以看到生成的war文件

![1553688538245](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688538245.png)            

 

## install命令

### 命令作用

 将打包好的war或jar安装到本地仓库，安装前会先执行编译，测试，打包。如果有一个失败，则安装失败。

### 操作演示

1)     双击install或者输入命令，执行安装命令

![1553688561730](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688561730.png)            

 

2)     执行以后在Run窗口看到如下信息

![1553688569534](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688569534.png)            

 

3)     安装完毕后，在本地仓库中可以找到当前项目创建的war包信息

![1553688574923](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688574923.png)            

 

4)     可以将war复制到tomcat/webapps目录下

![1553688582051](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688582051.png)    

 

5)     通过浏览器访问

![1553688588207](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688588207.png)            



## 命令行下执行Maven命令

### 操作演示：

1)     打开命令提示符窗口

 ![1553688600960](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688600960.png)   

 

2)     运行命令之前，必须进入项目所在的根目录，可以看到pom.xml所在的目录

![1553688605765](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688605765.png)    

 

3)     执行Maven的清除命令：mvn clean

  ![1553688611389](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688611389.png)  

 

4)     查看执行结果，发现target文件夹已经没有

 ![1553688616654](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688616654.png) 

  

## 生命周期的命令

| 命令        | 说明                               |
| ----------- | ---------------------------------- |
| mvn clean   | 清除target目录                     |
| mvn compile | 编译main目录                       |
| mvn test    | 执行test目录，要注意类名以test开头 |
| mvn package | 打包到target目录                   |
| mvn install | 安装到本地仓库                     |

 

# 上午回顾

## Maven的安装

1. 直接解压zip到指定的位置

2. 配置环境变量

   ```
   MAVEN_HOME=安装的目录
   Path=%MAVEN_HOME%\bin
   ```

   

## 仓库的概念

作用：由很多的jar包组成，我们只需要在pom.xml文件中通过坐标去引用。

三种仓库：

1. 本地仓库
2. 中央仓库
3. 私服仓库



## 坐标概念

1. groupId：公司，团队，组织名字，在仓库中代表1级或多级目录
2. artfactId：模块，项目名字，在仓库中表示1级目录
3. version：项目版本，在仓库表示1级目录



## 在idea中配置maven

1. 指定maven的安装目录
2. 指定maven的配置文件：conf/settings.xml
3. 指定本地仓库的位置



## 在idea中通过maven创建web项目

1. 通过骨架创建

   1. 必须要连接网络

   2. 自动生成pom.xml中配置信息

   3. 手动创建符合maven的目录结构：

      ```
      src --
       		 |-- main
       		 		   |-- java 源代码
       		 		   |-- resources 配置文件
       		 		   |-- webapp 网页
       		 |-- test (可选)
       		 			 |-- java 源代码
       		 			 |-- resources 配置文件
      pom.xml
      ```

      

2. 通过插件创建(不需要联网)

   1. 安装插件
   2. 创建一个Java项目
   3. 将Java项目转成web项目

   

## 与生命周期有关的命令

| 命令    | 功能                             |
| ------- | -------------------------------- |
| clean   | 删除target目录                   |
| compile | 编译main目录                     |
| test    | 执行test目录下所有的类和方法     |
| package | 打包到target目录下，jar包或war包 |
| install | 安装到本地仓库                   |



# 案例：创建自己的jar包

## 目标

1. 创建自己的jar包

2. 在其它maven工程中使用

## 案例需求：

1. 在01_JavaSE的项目中创建一个工具类MyUtils，类中包含静态方法sum(int m,int n)实现两个数相加

2. 将工具类打包成jar，安装到本地仓库中。

3. 在02_Web项目使用pom.xml文件引入本地仓库中的jar包

4. 创建类使用上面的工具类中的方法

## 实现步骤

1)     在main/java中创建MyUtils类和sum()方法

```java
package com.itheima;

/
 * 我的工具类
 */
public class MyUtils {

    /
     * 实现两个数相加
     * @param m 第1个数
     * @param n 第2个数
     * @return 两个数的和
     */
    public static int sum(int m, int n) {
        return m + n;
    }

}
```

 

2)     修改pom.xml文件，确认设置了packaging元素为jar，并且修改artifactId和version元素

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--公司名-->
    <groupId>com.itheima</groupId>
    <!--项目名-->
    <artifactId>myutils</artifactId>
    <!--版本-->
    <version>1.0</version>
    <!--打包类型-->
    <packaging>jar</packaging>

    <name>day39_01_Java</name>
    <url>http://www.example.com</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

 

3)     双击install，打包并且将jar包发布到本地仓库。

![1553688749289](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688749289.png)            

 

4)     检查本地仓库有了相应的jar包

![1553688754122](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688754122.png)            

 

5)     在另一个web项目中引用刚刚创建的jar包，并且点右边的Maven Projects刷新。

```xml
 <!--引用myutils-->
 <dependency>
     <groupId>com.itheima</groupId>
     <artifactId>myutils</artifactId>
     <version>1.0</version>
 </dependency>
```

 

6)     在web项目的main/java下创建一个类调用sum方法

```java
package com.itheima.maven;

import com.itheima.MyUtils;

/
 * 引用自己创建的工具类
 */
public class Demo3MyUtils {

    public static void main(String[] args) {
        int sum = MyUtils.sum(5, 3);
        System.out.println("两个数的和：" + sum);
    }

}
```

 

7)     运行结果如下：

![1553688837967](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553688837967.png)    

##  小结

创建自己的jar包的步骤：

1. 创建一个java工程
2. 编写自己的代码，使用install将项目打包到本地仓库
3. 在其他的项目中使用dependency引入上面自己写的jar包
4. 调用jar包中的类和方法

 

# Tomcat7插件的使用

## 目标

学习maven中tomcat7插件的使用

## 插件的使用

​	Maven只提供了基本的项目处理能力和管理，它的功能主要是通过其它插件来实现的。就好比jQuery和jQuery插件的关系。每个Maven插件可以完成一些特定的功能。

​	如：集成Tomcat插件后，无需安装Tomcat服务器就可以运行Tomcat进行项目的发布与测试。只需在pom.xml中通过plugin标签引入maven的功能插件。

### 配置Tomcat7插件步骤

Maven官方目前只有tomcat7插件，没有提供tomcat8的版本。

1)     选择day39_02_Web项目，将下面的代码复制成plugins的子元素。这时刷新Maven Projects不会出现任何变化。可以修改端口号和访问地址。

```xml
<build>
	<plugins>
		<!--Tomcat7插件-->
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<port>8080</port>
				<path>/</path>
				<uriEncoding>UTF-8</uriEncoding>
				<server>tomcat7</server>
			</configuration>
		</plugin>
	</plugins>
</build>
```

 

2)     刷新maven工程，即可使用maven来运行tomcat

![1553689020355](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689020355.png)            

 

3)     完整的pom.xml配置如下

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>day39_02_Web</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--打包成war-->
    <packaging>war</packaging>

    <name>day39_02_Web Maven Webapp</name>
    <url>http://www.example.com</url>

    <properties>
        <!--源代码的字符集-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--JDK编译的版本-->
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <!--JUnit包-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!--Servlet的配置-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <!--指定作用范围-->
            <scope>provided</scope>
        </dependency>
    </dependencies>

<build>
	<plugins>
		<!--Tomcat7插件-->
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<port>8080</port>
				<path>/</path>
				<uriEncoding>UTF-8</uriEncoding>
				<server>tomcat7</server>
			</configuration>
		</plugin>
	</plugins>
</build>
</project>
```

 

### 使用Maven运行Tomcat

1)     运行Tomcat7插件(注意要使用jdk1.8)，这时通过Maven来启动Tomcat7。

 ![1553689059159](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689059159.png)          

 

2)     可以点Run中出现的http://localhost:8080地址栏，打开浏览器看到结果。

![1553689066011](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689066011.png)            



## 小结

1. 在pom.xml中配置tomcat7的插件
2. 点tomcat7:run 就可以启动tomcat，并且部署我们的项目

 

# 包的依赖范围

## 目标

1. 有哪三种不同的类路径

2. 常用的依赖范围有哪些  



## 依赖范围

### 三种不同的类路径

类路径：用来告诉Java虚拟机，到哪些路径下去找jar包或.class文件。

Maven在执行过程中有三种不同的classpath，它们运行Java代码的时候，使用不同的classpath类路径下的jar包来执行。三种classpath范围如下：

| classpath范围 | 理解为                     |
| ------------- | -------------------------- |
| 编译类路径    | main目录下源代码可用       |
| 测试类路径    | test目录下源代码可以用     |
| 运行时类路径  | target下字节码运行时类路径 |

 ![1553689173481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689173481.png) 

​           

### 常用依赖范围

- 什么是依赖范围：三种类路径的不同的组合，就称为依赖范围。

- 通过标签：scope引用不同的依赖范围

- 默认是：compile


### 依赖范围见下表

| 依赖范围 | 编译类路径(main) | 测试类路径(test) | 运行时类路径(target) |
| -------- | ---------------- | ---------------- | -------------------- |
| compile  | Y                | Y                | Y                    |
| provided | Y                | Y                | -                    |
| runtime  | -                | Y                | Y                    |
| test     | -                | Y                | -                    |



### 示例：test依赖范围

1)     在02_Web项目中，main目录下创建一个类的，写一个测试类，加上@Test注解，发现报错。

![1553689215756](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689215756.png)            

 

2)     因为在pom.xml的dependency中配置了scope为test，表示只在test目录下可以使用，如果改成compile或provided并且刷新Maven Projects，则可以使用。

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.11</version>
  <!--依赖范围，配置test，只有test目录下可以，使用-->
  <scope>test</scope>
</dependency>
```

 

### 示例：provided依赖范围

​	例如：servlet-api就是编译和测试的时候才有用，在运行时不用，因为Tomcat容器已经提供了。如果打包到war文件中，可能会导致与Tomcat容器中的Servlet有冲突。

![1553689246159](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689246159.png)            

 

1)     在现在我们删除Servlet中的scope，这时它的依赖范围变成默认compile，即在编译，测试，运行时(打包到war中)都可以使用。

```xml
<!--Servlet的配置-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
</dependency>
```

 

2)     通过Maven启动Tomcat7的方式运行，发现出现如下错误，这是因为我们的Servlet被打包到了war包中，与Tomcat容器中的Servlet API发生了冲突导致。

![1553689279155](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689279155.png)           

 

3)     这时如果使用生成周期package打包，会发现在lib下有多余的jar包

![1553689284261](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689284261.png)    

 

4)     将servlet的scope改成provided。停止Tomcat7，执行生命周期的clean命令，再次运行package，可以看到新产生的lib下没有servlet的jar包

![1553689289160](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689289160.png)            

 

5)     再次启动tomcat7，执行正常

![1553689293534](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689293534.png)            

 

### 示例：runtime依赖范围

​	JDBC访问MySQL数据库，在写代码的过程中是针对接口开发，不会牵涉到任何JDBC的实现类。如：得到连接对象Connection不是通过我们在代码中写new JDBC4Connection子类来得到的，而是程序在执行过程中才读取MySQL的驱动类生成它的子类对象，只在测试和运行期间才会用到MySQL的子类，所以设置为runtime范围。



1)     在pom.xml中配置mysql驱动的依赖，指定依赖范围为runtime，只在测试和运行时期间起作用。

```xml
<!-- mysql的驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.22</version>
    <!--指定依赖范围-->
    <scope>runtime</scope>
</dependency>
```

 

2)     在test中，创建一个JDBC的测试方法，得到连接对象，可以发现JDBC的实现类在编译阶段是用不到的。因为编译阶段没有用到任何与JDBC4Connection相关的类，只在测试阶段和运行时才会用到。

```java
@Test
public void testConnection() throws SQLException {
    Connection connection = DriverManager.getConnection("jdbc:mysql:///test", "root", "root");
    System.out.println(connection);
}
```

 

3)     得到MySQL的连接对象

![1553689378784](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689378784.png)            

 

## 小结

1. 有哪三种不同的类路径?

   1. 编译类路径，main目录可用
   2. 测试类路径，test目录可用
   3. 运行时类路径，target目录可用

2. 常用的依赖范围有哪些

   ![1553689204735](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689204735.png)



# 添加联系人part1：搭建maven的环境

## 目标

使用maven搭建添加联系人的环境

## 需求：完成添加联系人信息的操作

### 用户列表页面

![1553689464043](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689464043.png)  

### 添加联系人页面

点添加联系人的按钮，弹出如下的模态框，使用原型文件夹的页面

![1553689484664](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689484664.png)   

###  模态框的方法

![1553689500210](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689500210.png) 

### 输入联系人数据

点保存添加一行，使用ajax向后台数据库添加一行记录，更新表格，隐藏模态框。

 ![1553689529109](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689529109.png)   

### 添加成功后数据库的记录

![1553689545909](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689545909.png)   

### redis中的数据

业务层代码，添加成功以后，再次查询所有的联系人，转成JSON对象，更新redis中的contacts数据。

​    ![1553689566690](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689566690.png)        

### 添加后更新联系人页面   ![1553689997686](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553689997686.png)

## 创建数据表

```mysql
-- 联系人
create table contact (
   id int primary key auto_increment,
   name varchar(20) not null,  -- 姓名
   phone varchar(20),  -- 电话
   email varchar(50),  -- 邮箱
   birthday date  -- 生日
);

insert into contact (name,phone,email,birthday) values
('孙悟空','13423431234','wukong@itcast.cn', '1993-11-23'),
('猪八戒','13525678909','bajie@itcast.cn', '1953-05-02'),
('白骨精','18642343123','xiaobai@itcast.cn', '1943-03-12');

select * from contact;
```



## 创建Maven项目

1. 使用插件方式创建webapp项目 

![1563979570384](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1563979570384.png)             

2. 修改JDK编译版本为1.8 

```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
```

 

## 导入坐标

### 导入所需的依赖和插件

1. MySQL驱动程序
2. MyBatis
3. redis 2.7.2
4. servlet-api 3.1.0（Servlet的API）
5. 其它的一些支持包，如：common包


### 依赖如下

![1563979633523](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1563979633523.png)  

### pom.xml完整的代码

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.itheima</groupId>
    <artifactId>day35_04_contact</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <properties>
        <!--源代码的字符集-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--JDK编译的版本-->
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!--servlet-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>

        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.30</version>
            <scope>runtime</scope>
        </dependency>

        <!--BeanUtils-->
        <dependency>
            <groupId>commons-beanutils</groupId>
            <artifactId>commons-beanutils</artifactId>
            <version>1.9.2</version>
        </dependency>

        <!--jackson-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.3.3</version>
        </dependency>

        <!--jedis-->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.7.2</version>
        </dependency>

        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.0</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!--Tomcat7插件-->
            <plugin>
            	<groupId>org.apache.tomcat.maven</groupId>
            	<artifactId>tomcat7-maven-plugin</artifactId>
            	<version>2.2</version>
            	<configuration>
            		<port>8080</port>
            		<path>/</path>
            		<uriEncoding>UTF-8</uriEncoding>
            		<server>tomcat7</server>
            	</configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

- 注：上面这些jar包依赖了其它的jar包，也会自动导入。导入的结果如下：


![1563980199620](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1563980199620.png) 



# 添加联系人part2：业务层和DAO层

## 目标

创建业务层和DAO层



### 将原来的web项目进行转换

1. 把原来src的复制到java目录
2. 将src下配置文件复制到resources
3. 将web目录下文件复制到webapp
4. 删除WEB-INF/lib目录

![1573372370647](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1573372370647.png) 

### 部署运行

![1553690207961](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553690207961.png)            

 

## DAO的代码

添加联系人

```java
/**
 * 添加联系人
 */
@Insert("insert into contact (name, phone, email, birthday) values (#{name},#{phone},#{email},#{birthday})")
int addContact(Contact contact);
```

```java
@Override
public int addContact(Contact contact) {
    //1.得到会话对象
    SqlSession session = SqlSessionUtils.getSession();
    //2. 通过会话得到代理接口对象
    ContactDao contactDao = session.getMapper(ContactDao.class);
    //3. 调用接口中方法
    int row = contactDao.addContact(contact);
    //4. 提交事务
    session.commit();
    //5. 关闭会话
    session.close();
    return row;
}
```



## Service

### 步骤

1. 调用DAO的方法，添加联系人到数据库
2. 如果返回影响的行数大于0，则使用工具类得到Jedis对象
3. 删除redis中键为contacts的字符串
4. 关闭jedis对象
5. 返回影响的行数


### 代码

```java
/**
 * 添加联系人
 */
public int addContact(Contact contact) {
    //1. 调用DAO将新的联系人写到数据库中
    int row = contactDao.addContact(contact);
    //2. 如果添加成功
    if (row > 0) {
        //得到Jedis对象
        Jedis jedis = JedisUtils.getJedis();
        //3. 删除redis中contacts键
        jedis.del("contacts");
        //4. 关闭jedis
        jedis.close();
    }
    return row;
}
```

###  

# 添加联系人part3：Servlet的开发

## 目标

Servlet的编写

### 步骤

1. 使用BeanUtils封装表单的数据

2. 调用业务层添加数据

3. 如果添加成功，返回文本字符串success，添加失败时抛出一个运行时异常。

4. 注：服务器返回的数据类型，要设置成text/plain类型

### 代码

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
import java.io.PrintWriter;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/add")
public class AddContactServlet extends HttpServlet {

    private ContactService contactService = new ContactService();

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        //1. 得到表单提交的数据
        Map<String, String[]> map = request.getParameterMap();
        //2. 封装成contact对象
        Contact contact = new Contact();
        try {
            BeanUtils.populate(contact, map);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        //3. 调用业务层添加联系人
        int row = contactService.addContact(contact);
        //4. 添加打印success到浏览器
        if (row > 0) {
            //打印文本
            response.setContentType("text/plain;charset=utf-8");
            PrintWriter out = response.getWriter();
            out.print("success");
        }
        //5. 如果添加失败，抛出异常
        else {
            throw new RuntimeException("添加失败");
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```



# 添加联系人part4：HTML页面

### 目标

HTML和jQuery代码的编写

### 步骤

1. 给模态框上的保存添加点击事件

2. 得到表单提交的所有参数并且拼接

3. 访问添加数据的Servlet

4. 返回字符串success表示添加成功

5. 成功以后重新加载表格

6. 清空模态框的内容，调用reset方法，reset()是js对象的方法，需要将jq对象转成js对象

7. 关闭模态框

8. 如果出现异常，则alert添加失败信息


### 代码

```html
<!DOCTYPE html>
<html lang="zh-CN">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>通讯录管理</title>
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <script src="js/jquery-3.3.1.min.js"></script>
    <script src="js/bootstrap.min.js"></script>
</head>

<body>
<div class="container">
    <br>
    <input id="btnLoad" type="button" class="btn btn-primary" value="加载联系人列表">
    <input id="btnClear" type="button" class="btn btn-danger" value="清空联系人列表">
    <input id="btnAdd" type="button" class="btn btn-success" value="添加联系人">
    <hr>
    <table class="table table-bordered table-hover table-striped">
        <thead>
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>电话</th>
            <th>邮箱</th>
            <th>生日</th>
        </tr>
        </thead>
        <tbody id="contacts">

        </tbody>
    </table>
</div>

<!-- 模态框 -->
<div class="modal fade" id="contactAdd" tabindex="-1" role="dialog">
    <div class="modal-dialog" role="document">
        <form id="contactForm">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span
                            aria-hidden="true">&times;</span></button>
                    <h4 class="modal-title" id="myModalLabel">输入联系人</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group">
                        <label for="name">姓名</label>
                        <input type="text" name="name" class="form-control" id="name" placeholder="请输入姓名">
                    </div>
                    <div class="form-group">
                        <label for="name">电话</label>
                        <input type="tel" name="phone" class="form-control" id="phone" placeholder="请输入电话号码">
                    </div>
                    <div class="form-group">
                        <label for="name">邮箱</label>
                        <input type="email" name="email" class="form-control" id="email" placeholder="请输入邮箱">
                    </div>
                    <div class="form-group">
                        <label for="name">生日</label>
                        <input type="date" name="birthday" class="form-control" id="birthday" placeholder="请选择生日">
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-primary" id="btnSave">保存</button>
                </div>
            </div>
        </form>
    </div>
</div>


<script type="text/javascript">
    //编写加载按钮的点击事件
    $("#btnLoad").click(function () {
        listContacts();
    });

    //显示所有的联系人
    function listContacts() {
        //1. 后台访问服务器
        $.get({
            url: "list",
            success: function (contacts) {
                var html = "";
                //2. 得到所有的联系人JSON对象
                for (var contact of contacts) {
                    //3. 遍历集合，每个对象创建一个tr
                    html += "<tr><td>" + contact.id + "</td><td>" + contact.name + "</td><td>" + contact.phone
                        + "</td><td>" + contact.email + "</td><td>" + contact.birthday + "</td></tr>";
                }
                //4. 将tr添加到tbody中
                $("#contacts").html(html);
            }
        });
    }

    //清空联系人
    $("#btnClear").click(function () {
        //清空子元素
        $("#contacts").empty();
    });

    //添加联系人按钮
    $("#btnAdd").click(function () {
        //显示模态框
        $("#contactAdd").modal("show");
    });
    
    //保存按钮的点击事件
    $("#btnSave").click(function () {
        //1.得到表单中所有的数据
        var param = $("#contactForm").serialize();
        //2.使用post方法后台提交给Servlet
        $.post({
            url: "add",
            data: param,
            success: function (result) {
                //3.在回调函数中如果得到success字符串，表示添加成功
                if (result == "success") {
                    //4. 添加成功清空表单
                    $("#contactForm")[0].reset();
                    //5. 隐藏模态框
                    $("#contactAdd").modal("hide");
                    //6. 显示所有的联系人
                    listContacts();
                }
            },
            error: function () {  //添加失败
                alert("添加失败");
            }
        });
    });
</script>
</body>
</html>
```



## 小结

1. DAO层实现什么功能？添加联系人
2. 业务层实现了什么功能？添加了联系人，清空redis
3. 项目中使用了哪些技术? mybatis, redis,jQuery,maven,ajax



# ThreadLocal的介绍

## 作用

就是一个容器，底层结构是Map。它的键是当前线程对象，值是任意对象。

![1573375535584](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1573375535584.png) 

## ThreadLocal中的方法

| 方法                   | 说明                 |
| ---------------------- | -------------------- |
| new  ThreadLocal\<T>() | 构造方法创建一个容器 |
| void set(T value)      | 向容器中添加一个值   |
| T get()                | 从容器中取得一个值   |
| void remove()          | 删除容器中值         |



## 案例：使用工具类得到2次会话

## 需求

在main函数中使用工具类得到2次会话，输出它们的对象，观察是否是同一个对象。

## 代码

```java
package com.itheima.test;

import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

public class TestThreadLocal {

    /**
     * 使用工具类得到2次会话，输出它们的对象，观察是否是同一个对象。
     */
    public static void main(String[] args) {
        SqlSession s1 = SqlSessionUtils.getSession();
        SqlSession s2 = SqlSessionUtils.getSession();
        System.out.println(s1);
        System.out.println(s2);
        System.out.println("是否相同：" + (s1 == s2));
    }

}
```

## 效果

![1573375804109](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1573375804109.png) 

## 小结

1. ThreadLocal底层结构是什么？键和值分别是？

   ```
   Map结构
   键是：当前线程对象
   值是：泛型，可以任意对象
   ```

   

2. 添加对象和获取对象的方法是什么？

   ```
   set(值)
   get()
   ```

   



# 改造SqlSessionUtils工具类

## 目标

使用ThreadLocal改造会话工具类

## 步骤

1. 创建私有静态ThreadLocal容器

2. 得到会话对象方法
   1. 从容器中得到会话
   2. 如果会话为空，则从工厂中创建会话
   3. 并且放入容器中
   4. 返回会话对象

3. 关闭会话方法
   1. 从容器中得到会话
   2. 如果会话不会为空，则关闭会话
   3. 从容器中删除会话对象


## 代码

```java
package com.itheima.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

/**
 * 会话工厂工具类
 */
public class SqlSessionUtils {
    private static SqlSessionFactory factory;
    private static ThreadLocal<SqlSession> threadLocal = new ThreadLocal<>();  //容器用来存放会话对象

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
     * 得到会话对象
     * @return 会话对象
     */
    public static SqlSession getSession() {
        //1.从容器中得到会话对象
        SqlSession session = threadLocal.get();
        //2. 如果对象为空，从工厂中得到会话
        if (session == null) {
            session = factory.openSession();
            //3. 将这个会话放在容器中
            threadLocal.set(session);
        }
        //4. 返回会话对象
        return session;
    }

    /**
     * 关闭会话需要将对象从容器中删除
     */
    public static void closeSession() {
        //1.从容器中得到会话
        SqlSession session = threadLocal.get();
        //2.判断会话是否为空
        if (session != null) {
            //3.如果不为空则关闭会话
            session.close();
            //4.并且从容器中删除会话
            threadLocal.remove();
        }
    }

}
```

## 测试

使用工具类得到2次会话

```java
package com.itheima.test;

import com.itheima.utils.SqlSessionUtils;
import org.apache.ibatis.session.SqlSession;

public class TestThreadLocal {

    /**
     * 使用工具类得到2次会话，输出它们的对象，观察是否是同一个对象。
     */
    public static void main(String[] args) {
        //第1个线程
        new Thread(() -> {
            SqlSession s1 = SqlSessionUtils.getSession();
            SqlSession s2 = SqlSessionUtils.getSession();
            System.out.println(s1);
            System.out.println(s2);
            System.out.println("是否相同：" + (s1 == s2));
        }).start();

        //第2个线程
        new Thread(() -> {
            SqlSession s1 = SqlSessionUtils.getSession();
            SqlSession s2 = SqlSessionUtils.getSession();
            System.out.println(s1);
            System.out.println(s2);
            System.out.println("是否相同：" + (s1 == s2));
        }).start();

    }

}
```

## 效果

![1573376540422](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1573376540422.png) 

## 小结

使用ThreadLocal可以让同一个线程多次得到同一个会话，应用场景：以后在事务中可以使用，操作多个DML的时候，必须是同一个会话。



# 制作通用的DAO实现类

## 目标

制作通用的DAO实现类

## 回顾动态代理

### 代理模式的组成

抽象角色：ContactDao

真实角色：ContactDaoImpl

代理角色：今天要写的实现类

![1554285221866](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1554285221866.png)



### 动态代理的好处

1. 接口的代理对象由程序在执行的过程中动态生成，不用我们自己去写一个类实现接口中所有的方法
2. 可以动态生成任意接口的对象

### Proxy类中的方法

![1557839380147](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1557839380147.png)

### InvocationHandler接口

![1557839397638](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1557839397638.png) 



## 步骤

1. 创建类：DaoInstanceFactory，使用动态代理创建DAO的实现类
2. 创建静态方法得到DAO的实现类，方法签名：

```java
public static <T> T getBean(Class<T> daoInterface)
```

3. 在每个被代理的方法中
   1. 得到会话对象
   2. 通过会话得到Mapper对象
   3. 调用原来DAO接口中的方法
   4. 提交事务
   5. 关闭会话
   6. 返回原来方法的值

## 代码

```java
package com.itheima.utils;

import com.itheima.dao.ContactDao;
import org.apache.ibatis.session.SqlSession;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class DaoInstanceFactory {

    /**
     * 得到DAO接口的代理对象
     */
    public static <T> T getBean(Class<T> clazz) {
        /*
        返回生成的代理对象
        参数1：类加载器，我们写的类是同一个类加载器
        参数2：所有代理的接口数组
        参数3：回调函数，代理接口中每个方法
         */
        return (T) Proxy.newProxyInstance(
                DaoInstanceFactory.class.getClassLoader(),
                new Class[]{clazz},
                new InvocationHandler() {
                    /**
                     * 回调函数中方法参数
                     * @param proxy 代理对象
                     * @param method 代理的方法
                     * @param args 方法的参数
                     * @return 方法的返回值
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //1.得到会话对象
                        SqlSession session = SqlSessionUtils.getSession();
                        //2. 通过会话得到代理接口对象
                        T mapper = session.getMapper(clazz);
                        //3. 调用接口中方法
                        Object result = method.invoke(mapper, args);
                        //4. 提交事务
                        session.commit();
                        //5. 关闭会话
                        SqlSessionUtils.closeSession();
                        return result;
                    }
                });
    }

}
```

## 业务层代码修改

```java
private ContactDao contactDao = DaoInstanceFactory.getBean(ContactDao.class);
```

## 小结

使用动态代理对象来代理DAO接口，以后就不需要自己写实现类



# 学习总结

1. 能够了解Maven的作用

   1. jar包的管理
   2. 生命周期中命令
   3. 将大项目拆分成小模块

   

2. 能够理解Maven仓库的作用

   1. 本地仓库
   2. 中央仓库
   3. 私服仓库

   

3. 能够理解Maven的坐标概念

   ![1553675678291](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553675678291.png) 

4. 能够掌握Maven的安装

   1. 直接解压即可

   2. 配置环境变量

      ```
      MAVEN_HOME
      Path
      ```

      

   

5. 能够掌握IDEA配置本地Maven

   ![1553675824793](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553675824793.png)

   

6. 能够使用IDEA创建JavaWeb的Maven工程

   ![1553675887061](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553675887061.png) 

7. 能够自定义JaveWeb的Maven工程和使用插件优化项目目录结构

   1. 安装插件
   2. 创建Java项目
   3. 将Java项目转成web项目

   

8. 能够掌握依赖引入的配置方式

   ![1553675954725](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/maven基础/1553675954725.png)

9. 能够了解依赖范围的概念

   | 依赖范围 | 编译classpath | 测试classpath | 运行时classpath |
   | -------- | ------------- | ------------- | --------------- |
   | compile  | Y             | Y             | Y               |
   | provided | Y             | Y             |                 |
   | runtime  |               | Y             | Y               |
   | test     |               | Y             |                 |

    

 