# 《Linux基础-上课笔记》

# 回顾

##  Redis常用命令

| string命令  | 功能               |
| ----------- | ------------------ |
| set 键   值 | 添加键和值         |
| setnx 键 值 | 如果不存在键就添加 |
| get 键      | 获取值             |
| del 键      | 通过键删除         |

| hash命令            | 功能                 |
| ------------------- | -------------------- |
| hset 键   字段   值 | 添加1个字段和值      |
| hget 键   字段      | 通过键，字段得到值   |
| hgetall 键          | 得到这个键下面的字段 |

| list命令                | 功能                                  |
| ----------------------- | ------------------------------------- |
| lpush 键   元素   元素  | 从左边添加1个或多个元素               |
| lpop 键                 | 从左边删除1个元素                     |
| lrange 键   开始   结束 | 获取一个范围所有元素，获取所有从0到-1 |

| set命令               | 功能               |
| --------------------- | ------------------ |
| sadd 键   元素   元素 | 添加元素           |
| smembers 键           | 获取键下所有的元素 |

| zset命令                    | 功能                                  |
| --------------------------- | ------------------------------------- |
| zadd 键 分数 值 分数 值     | 添加1个或多个元素                     |
| zrange 键 开始索引 结束索引 | 获取一个范围所有元素，获取所有从0到-1 |
| zrem 键 值                  | 删除元素                              |
| zcard 键                    | 得到元素个数                          |
| zrank 键 值                 | 得到索引号                            |

##  Jedis类常用方法

| 连接和关闭              | 功能                                       |
| ----------------------- | ------------------------------------------ |
| new Jedis(host,   port) | 创建一个连接对象，参数：主机名，端口号6379 |
| void close()            | 关闭连接                                   |

##  Jedis连接池API

| JedisPool连接池类               | 说明                                                 |
| ------------------------------- | ---------------------------------------------------- |
| new JedisPool(config,host,port) | 作用：创建连接池<br />参数：配置对象，主机名，端口号 |
| Jedis getResource()             | 从连接池中得到一个连接对象                           |
| void close()                    | 关闭连接池                                           |

 

# 学习目标

1. 能够独立搭建Linux环境

2. <font color="red">能够使用Linux进行目录操作的命令</font>

3. <font color="red">能够使用Linux进行文件操作的命令</font>

4. <font color="red">能够使用Linux进行目录文件压缩和解压的命令</font>

5. 能够使用Linux进行目录文件权限的命令

6. 能够使用其它常用的Linux命令

7. 能够使用客户端工具连接Linux系统

8. 能够使用Linux中的crontab命令



# 学习内容

## 为什么要学习Linux

### 目标

1. Linux有哪些优势

2. Linux的分类

### Windows的不足：

​	对于Windows操作系统而言，大家应该不陌生，但Windows操作系统也存在一些不足的地方

![1553472838464](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553472838464.png) 

1. 要收费
2. 长时间运行会变得不太稳定，速度也变慢
3. 有很多病毒和流氓软件

### Linux的优势

相反，上述Windows的不足，恰好是另一款操作系统Linux的优势所在：

![1553472903029](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553472903029.png)

1. 免费开源
2. 长时间运行也很稳定
3. 几乎没有病毒和流氓软件

### 学习Linux的目的

​	学会对Linux的基本操作是后端JavaEE程序员的必修课。做为一个后端JavaEE程序员，通常在Windows中开发完程序后，需要部署到一个相对比较安全，稳定的服务器中运行，这台服务器上安装的往往不是Windows操作系统，而是Linux操作系统。

###  Linux的分类

​	Linux是基于Unix的开源、免费、多用户、多任务的操作系统，由于系统的稳定性和安全性。几乎成为程序代码运行的最佳系统环境。

#### 根据市场需求不同切分

1. 图形界面版：注重用户体验，但目前没有成为主流，主流的普通用户还是习惯在Windows下使用。

   ![1553473127790](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553473127790.png)

2. 服务器版：没有好看的界面，在控制台窗口中输入命令来操作系统的，是我们架设服务器的最佳选择，类似于DOS界面。

   ![1553473150179](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553473150179.png) 

#### 根据原生程度不同

1. 内核版本：在托瓦兹(Linux之父)领导下的内核小组开发维护的系统内核的版本号。

   ![1553473207414](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553473207414.png) 

2. 发行版本：一些个人/组织/公司在内核版基础上进行二次开发而重新发行的版本号。如：ubuntu、redhat、centos、lubuntu、freeBSD等等。CentOS是一个Linux的发行版本，是目前企业中用来做应用服务器的主要版本。

   ![1553473315196](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553473315196.png)

##### 内核版本与发行版本的关系

​	内核版是唯一的，发行版可以很多，它封装了内核版，底层还是使用内核版操作机器的硬件设备。

![1553473254492](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553473254492.png)

### 小结

1. Linux有哪些优势？
   1. 免费开源
   2. 稳定
   3. 没有病毒和流氓软件
2. 按市场需求分类：
   1. 图形界面
   2. 字符界面
3. 按原生程度分类：
   1. 内核
   2. 发行版 CentOS



## 安装虚拟机软件并创建虚拟机

### 目标

1. 安装虚拟机软件

2. 创建虚拟机

### 安装方式

1. 使用双系统，在电脑开启的时候选择进入哪个操作系统，但不建议安装多个系统，一是浪费空间，二是可能造成系统不稳定。
2. 使用虚拟机，虚拟机就是Windows系统上的软件，通过软件来模拟一台电脑。我们可以在虚拟机中安装Linux系统。

### 虚拟机简介

​	虚拟机是一个软件，它可以使你在一台真实PC机器上同时运行两个或更多的操作系统，如：Windows或Linux。它可以模拟一个标准的PC环境，这个环境和真实的计算机一样，有芯片组、CPU、内存、显卡、声卡、网卡、软驱、硬盘、光驱、串口、并口、USB控制器。

​	目前市场上流行的虚拟机有两种：

1. VMware（威睿）公司的虚拟机软件，功能强大，收费产品，有30天试用期。

2. VirtualBox （甲骨文）公司的虚拟机软件，免费的商品。

### VMware Workstation Pro 安装

1. 双击如下安装文件进行安装

  ![1570061928194](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061928194.png)  

2. 出现欢迎界面

  ![1570061946875](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061946875.png)  

3. 接受软件许可协议

  ![1570061958218](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061958218.png)  

4. 选择安装目录

  ![1570061965889](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061965889.png)  

5. 用户体验设置

  ![1570061973571](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061973571.png)  

6. 创建快捷方式

  ![1570061981348](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061981348.png)  

7. 开始复制文件

  ![1570061989257](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570061989257.png)  

8. 复制文件

  ![1570062001696](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570062001696.png) 

9. 安装结束

  ![1570062009789](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570062009789.png)  

10. 输入正版的许可证

  ![1570062025029](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570062025029.png) 

11. 安装完成

   ![1570062034272](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570062034272.png)  

12. 安装完成，桌面就会启动图标，双击

   ![1570062055575](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1570062055575.png)  



### 创建虚拟机

1. 点击 文件 -> 新建虚拟机 创建一台新虚拟机

   ![1553474264700](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474264700.png) 

2. 在弹出框中选择典型安装

   ![1553474282745](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474282745.png) 

3. 选择稍后安装系统

   ![1553474297763](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474297763.png) 

4. 选择引导系统是Linux并选择系统版本是CentOS

   ![1553474314618](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474314618.png) 

5. 选择安装位置

   ![1553474328945](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474328945.png) 

6. 确定磁盘的最大使用空间

   ![1553474343261](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474343261.png) 

7. 准备安装前的硬件设置

   ![1553474356989](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474356989.png) 

8. 设置网络适配器为NAT连接网络模式

   ​	NAT英文全称是“Network Address Translation”，中文意思是“网络地址转换”，NAT 可以让内部网络连接到Internet或其它IP网络上。虚拟机中的linux系统与windows主机形成一个局域网，并共享windows主机外网网络。

   ![1553474378589](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474378589.png) 

### 小结

1. 什么是虚拟机软件？

   在一个操作系统中虚拟另一台电脑

2. 创建虚拟机的目的是什么？

   在电脑中安装另一个操作系统



## 安装CentOS7

### 目标

​	学会自己在虚拟机中安装CentOS7

### 步骤

1. 开启虚拟机

   ![1553474693239](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474693239.png) 

2. 启动机器安装系统

   ![1553474713229](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474713229.png) 

3. 引导安装，点击next

   ![1553474732053](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474732053.png) 

4. 软件选择，选择"基本网页服务器"-> "开发工具"

   ![1553474758157](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474758157.png)

5. 点击安装位置

   ![1553474778023](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474778023.png) 

6. 设置外网网卡打开

   ![1553474799199](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474799199.png)

7. 设置网卡自动连接

   ![1553474817551](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474817551.png)

8. 网络配置完成

   ![1553474834846](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474834846.png)

9. 配置完成，点开始安装系统

   ![1553474872512](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474872512.png) 

10. 复制文件的过程中可以设置root管理员密码

    ![1553474888552](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474888552.png) 

11. 密码设置为root，因为密码太短，点完成两次

    ![1553474907551](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474907551.png) 

12. 重新引导系统

    ![1553474923614](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553474923614.png)


### root用户登录和退出

1. root用户登录进入Linux

   ![1553478488147](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478488147.png) 

2. root用户退出Linux

   ![1553478507014](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478507014.png) 

### 小结

1. 安装的时候要选择：基本网页服务器，开发工具要勾上
2. 网络连接配置要打开，并且设置自动得到IP地址
3. 管理员的用户名是root，如果密码太短要点两次确认。



## 客户端连接Linux服务器前的工作

### 目标

1. 与IP地址有关的Linux命令
2. 查看连接的IP地址

### 与IP有关的命令

| 命令          | 功能说明                      |
| ------------- | ----------------------------- |
| ifconfig      | 显示Linux主机的IP地址         |
| ip addr       | 同上                          |
| ping 网络地址 | 检测2台计算机之间网络是否连通 |

疑问：windows主机与虚拟机linux系统为什么可以直接连通？

```
选择是NAT模式，window主机与Linux主机在同一个局域网中
```



### VMnet1和VMnet8

![1563701969969](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563701969969.png)



### 配置服务器端的网络

使用客户端连接CentOS服务器之前要先要查看CentOS的局域网ip地址。

1. 使用命令查看Linux在局域网中的IP地址

   ![1553475956507](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553475956507.png) 

2. 查看Windows主机在局域网中的IP地址，输入ipconfig

   ![1553475967684](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553475967684.png) 

### 小结

如何查看客户端与服务器端的IP地址？

```
Linux: ifconfig
windows：ipconfig
在windows中ping Linux
```



## 客户端的安装和使用

### 目标

1. 安装客户端软件
2. 使用客户端软件操作Linux服务器

### 软件下载和安装

1. 官网免费下载 https://mobaxterm.mobatek.net

   ![1567041150123](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567041150123.png)

   

2. 无需安装，直接解压就可以使用

![1567041016921](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567041016921.png) 



### 客户端连接服务器

1. 点左上角的Session创建一个会话连接

   ![1567042164375](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567042164375.png) 

   

2. 在如下窗体中进行会话的设置

   ![1567042210491](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567042210491.png)

   

3. 第1次连接，需要输入密码

   ![1567042234526](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567042234526.png)

   

### 使用客户端传输文件

![1567042652103](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1567042652103.png)

### 小结

发送命令和传输文件在Linux下分别使用什么协议？

```
命令：SSH
传输文件：SFTP
```

使用的客户端软件叫什么名字？

```
MobaXterm
```



## Linux中目录结构说明

### 目标

了解常用的Linux下目录的作用



### Linux 的目录结构

​	与Windows操作系统不同，Windows中最上面是盘符。在Linux中没有盘符概念，最顶层是根目录/。

![1553478633875](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478633875.png)

#### 目录说明

| 常用目录 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| root     | 管理员登录以后进入的目录，普通用户不能访问                   |
| home     | 普通用户登录以后进入的目录，每个用户在这下面都有一个自己的目录 |
| bin      | 可执行命令的目录，今天学习的命令都在这个目录下               |
| etc      | 配置文件所在目录                                             |
| usr      | 软件安装目录，可以共享资源                                   |

#### Linux下文件不同颜色表示的含义

| 颜色                             | 说明           |
| -------------------------------- | -------------- |
| 白色                             | 普通文件       |
| <font color="blue">深蓝色</font> | 目录           |
| <font color="red">红色</font>    | 压缩文件       |
| <font color="00b7ee">青色</font> | 链接(快捷方式) |
| <font color="orange">橙色</font> | 设备文件       |
| <font color='green'>绿色</font>  | 可执行文件     |

#### 命令提示符说明

 ![1573094001872](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1573094001872.png)



## 目录操作命令：cd、mkdir、ls

### 目录切换命令cd

#### 语法格式

| cd 目录名 | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| 作用      | 用于切换当前工作目录，Change Directory                       |
| .         | 当前目录                                                     |
| ..        | 上一级目录                                                   |
| -         | 后退到上一次目录                                             |
| ~         | 回到主目录，管理员回到root目录，普通用户回到home/用户名。~可以省略 |

#### 操作演示

1. 切换到系统根目录
2. 切换到该目录下usr目录
3. 切换到上一层目录
4. 切换到用户主目录，如果是root管理员，则是到root目录
5. 切换到上一个所在的目录

#### 执行结果

![1553478889913](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478889913.png) 



### 创建目录mkdir

####  语法格式

| mkdir 目录名 | 说明                    |
| ------------ | ----------------------- |
| 作用         | make directory 创建目录 |

#### 操作演示

1. 进入root目录
2. 在root目录下创建aaa目录
3. 使用.方式的相对路径，在当前目录下创建bbb目录
4. 在root目录下，在bbb目录下创建ccc目录
5. 使用..在上一级目录下创建ddd目录
6. 使用绝对路径在root下创建目录eee目录

#### 执行结果

![1553478932139](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478932139.png) 



### 查看当前目录内容ls

注：在Linux下大部分命令都可以使用 --help来显示它的用法

在Linux下隐藏文件以点号开头

#### 语法格式

| 语法：ls [参数] | 功能说明                       |
| --------------- | ------------------------------ |
| 无              | 查看当前目录下内容             |
| -l              | 以详细的方式显示               |
| -a              | 显示所有的文件，包括隐藏的文件 |

####  操作演示

1. 进入root目录，以精简形式查询当前目录下的内容
2. 以详细形式查询当前目录下的内容，可以缩写成ll
3. 在当前目录下创建一个隐藏的目录.ccc
4. 以精简形式查询当前目录下的所有的内容，包含隐藏文件
5. 以详细形式查询当前目录下的隐藏内容，-la和-al都可，也可以使用ll -a

#### 执行结果

![1553479027152](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479027152.png) 

![1553479038312](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479038312.png) 



#### 文件列表的含义

![1553479161559](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479161559.png)

### 小结

| 命令            | 功能                  |
| --------------- | --------------------- |
| cd              | 改变目录              |
| mkdir           | 创建目录              |
| ls              | 查看目录              |
| ll              | 以详细的方式查看目录  |
| 第1列字母的含义 | l 链接 d 目录  - 文件 |



## 目录操作命令：find、mv、cp、rm

### 目标

学习目录操作命令：find、mv、cp、rm

### 搜索find

#### 语法格式

| find [目录名] [-name '查询字符串'] | 功能                                       |
| ---------------------------------- | ------------------------------------------ |
| 无参名                             | 查找当前目录下所有的文件和目录，包括子目录 |
| 目录名                             | 查找指定目录下文件和子目录                 |
| -name '查询字符串'                 | 查询指定的字符串，可以使用通配符           |
|                                    | * 匹配多个字符                             |
|                                    | ? 匹配一个字符                             |

####  操作演示

1. 在/root目录下，查询当前目录下所有的文件和目录
2. 查询/根目录下（包括子目录），名以abc开头的目录和文件
3. 查询根目录即其子目录下以cc开头的三个字符的目录或文件

####  执行结果

![1553479660634](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479660634.png) 

![1553479665494](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479665494.png) 

![1553479669654](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479669654.png) 



### 重命名mv

#### 语法

| mv  旧名  新名 | 说明                    |
| -------------- | ----------------------- |
| 作用           | move 修改目录名或文件名 |

####  操作演示

1. 将root文件夹下的aaa目录改成abc
2. 使用touch创建一个空文件为aaa，再使用mv将aaa文件改名为xyz

#### 执行结果

![1553479737786](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479737786.png) 

![1553479742229](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479742229.png) 



### 移动mv

#### 语法

| mv  源目录  目标目录 | 说明                           |
| -------------------- | ------------------------------ |
| 作用                 | 将目录或文件移动到另一个目录下 |

#### 操作演示

1. 创建目录cc，再将cc目录移动到eee目录下

#### 执行结果

![1553479819145](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479819145.png) 

疑问：mv命令什么时候是改名，什么时候是移动？

```
目标目录如果不存在就是改名，如果存在就是移动
```



### 复制命令cp

#### 语法格式

| 语法 | cp [参数] 源文件或目录 目标目录    |
| ---- | ---------------------------------- |
| 作用 | copy 复制文件或目录                |
| -r   | recursion 递归，连同子目录一起复制 |

####  操作演示：

1. 当前是root目录，复制/proc/dma文件到root目录下的bbb目录中
2. 将/etc目录下所有c开头的文件复制到root目录下的bbb目录下
3. 将/etc目录下所有h开头的文件和目录复制到root目录下的eee目录，连同子目录下的内容一起复制

#### 执行结果

![1553486769826](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553486769826.png)

![1553486783149](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553486783149.png) 



### 删除文件或目录rm

注：按tab会自动补全，按2下tab可以出现所有匹配的文件名

#### 语法格式

| 语法：rm  [参数]  文件或目录1 文件或目录2 | 功能                     |
| ----------------------------------------- | ------------------------ |
| 作用                                      | remove 删除文件或目录    |
| -r                                        | 连同子目录一起删除       |
| -f                                        | 删除前没有确认，强制删除 |

####  操作演示

1. 同时删除eee目录下的hostname和hosts文件
2. 进入root下的bbb目录，删除所有文件名csh，任意扩展名的文件
3. 进入root目录下的eee目录，递归删除httpd目录和所有子目录的文件，不进行确认，强制删除。

#### 执行结果

![1553486859853](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553486859853.png) 

### 小结

| 目录操作命令 | 功能                           |
| ------------ | ------------------------------ |
| find         | 查找指定目录下所有的文件或目录 |
| mv           | 改名或移动                     |
| cp           | 复制                           |
| rm           | 删除                           |



## 文件的操作：显示文件内容、创建文件

### 目标

1. 如何显示文件内容

2. 如何创建空文件

### 查看文件

#### 语法格式

| 查看文件的内容的命令： | cat/more/head/tail/less                                      |
| ---------------------- | ------------------------------------------------------------ |
| cat 文件名             | 显示文本文件所有的内容                                       |
| more 文件名            | 分屏显示，显示一屏就暂停<br />回车：每按一次，多显示一行<br />空格：每按一次，多显示一屏<br />q: 退出 |
| head 文件名            | 显示前10行                                                   |
| head -n 行数   文件名  | 显示前面的n行                                                |
| tail 文件名            | 显示后面10行                                                 |
| tail -n 行数   文件名  | 显示后面的n行                                                |
| less 文件名            | 翻页显示，向前向后翻页<br />-N: 显示行数<br />PageUP:  向前翻页<br />PageDown: 向后翻页 |

####  操作演示：

1. 将素材目录的Demo.java文件，上传到root目录下。
2. 查看当前目录下Demo.java文件的全部内容
3. 分页查看当前目录下Demo.java文件内容，按回车键一行一行的看，按空格健一页一页的看
4. 查看当前目录下Demo.java文件的前10行内容
5. 查看当前目录下Demo.java文件的后10行内容
6. 查看当前目录下Demo.java文件的前5行内容
7. 查看当前目录下Demo.java文件的后5行内容
8. 使用less命令显示Demo.java文件，显示行号

#### 执行结果

![1553487483218](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553487483218.png) 



### 文件操作：创建文件touch

#### 语法格式

| touch 文件1 文件2 | 说明                          |
| ----------------- | ----------------------------- |
| 作用              | 创建一个空文件，大小是0个字节 |

####  操作演示

1. 在当前目录中创建Hello.java文件
2. 在当前目录中同时创建Hello.txt文件和Hello.xml

#### 执行结果

![1553487546243](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553487546243.png) 

### 小结

| 命令         | 功能         |
| ------------ | ------------ |
| cat 文件名   | 查看文件     |
| more 文件名  | 分屏显示     |
| head 文件名  | 显示前面10行 |
| tail 文件名  | 显示后面10行 |
| less 文件名  | 翻页显示     |
| touch 文件名 | 创建空文件   |



## 文件的操作：vim编辑文件

### 目标

1. vim编辑器的三种模式

2. vim底行模式有哪些常用的命令

###  vim介绍

​	vi(vim)是上Linux常用的编辑器，很多Linux发行版都默认安装了vi(vim)。vi是“Visual Interface”的缩写，vim是 (增强版的vi)。在一般的系统管理维护中vi就够用，如果想使用代码加亮的话可以使用vim。

###  vim编辑器的三种模式![1553488173588](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488173588.png) 

### vim三种模式的切换

![1553488196200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488196200.png)

### vim操作语法

#### 命令模式下按键

| 命令 | 描述                         |
| ---- | ---------------------------- |
| i    | 在当前光标的位置前面插入字符 |
| a    | 在当前光标的位置后面插入字符 |
| o    | 在当前光标的位置下面插入字符 |

####  命令模式下常用的编辑命令

| 命令         | 描述                                                      |
| ------------ | --------------------------------------------------------- |
| yy           | 复制行                                                    |
| p            | 粘贴                                                      |
| dd           | 删除行                                                    |
| u            | 撤销上一步的操作                                          |
| /字符串      | 搜索指定的字符串<br />n： 搜索下一个 <br />N： 搜索上一个 |
| **底行模式** | :                                                         |
| wq           | 存盘退出                                                  |
| q!           | 强制退出，不存盘                                          |
| wq!          | 强制存盘退出，通常用于只读文件                            |

### 操作1：

1. vim Hello.java  用vim编辑器创建/打开Hello.java文件，这时进入命令模式。

2. 按i键，进入编辑模式，输入以下内容：

   ![1553488534310](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488534310.png)                                            

3. 按Esc键，进入命令模式，按冒号进入底行模式

4. 输入:wq回车，表示存盘退出 

5. 使用cat Hello.java查看文件的内容

### 操作2

1. 使用vim打开Hello.java文件，进入命令模式。

2. 将光标移动到System.out这一行，按yy复制

3. 按3次p，粘贴这一行三次

   ![1553488649645](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488649645.png)                                                   

4. 按dd删除最后一行

5. 按i进入编辑模式

6. 再任意输入一些内容

7. 在按Esc进入命令，按冒号进入底行模式

8. 输入q!回车，不存盘强行退出。

9. 作用cat Hello.java发现Hello.java没有变化



### 小结

1. 三种操作模式：命令，编辑，底行

2. 命令模式-> 编辑模式：i a o

3. 编辑模式-> 命令模式：Esc

   1. yy  复制
   2. p 粘贴
   3. dd 删除

4. 命令模式-> 底行模式：冒号

   1. wq 保存退出
   2. q! 不存盘退出



## 上午回顾

### 安装Linux系统

1. 安装虚拟机软件：VMware 
2. 创建虚拟机，设置硬件配置
3. 在虚拟机安装Linux系统：CentOS7

### 客户端的安装和使用

1. 查看IP地址命令

```
ifconfig 查看IP地址
ip addr 查看IP地址
ping 检测网络是否连通

windows下查看IP地址：ipconfig
```

2. windows系统与Linux在同一个局域网

```
IP地址要在同一个网段
192.168.248.130  ~  192.168.248.1
```

3. 看网卡VMnet8 IP地址
4. 安装客户端软件：MobaXterm

### 目录有关的命令

```
cd：改变目录
mkdir：创建目录
ls：查看目录内容
find：查找目录和子目录
mv：移动或改名
cp：复制
rm：删除
```



### 文件有关的命令

```
文件显示命令: cat, more,head,tail,less
创建空文件: touch
```

### vim编辑文件

1. 三种编辑模式：命令，编辑，底行
2. 命令->编辑：a, i, o
3. 编辑->命令：Esc
4. 命令->底行：冒号

```
yy 复制
p 粘贴
dd 删除
/字符串 查找
u 撤销
```



## <font color="red">文件的压缩和解压命令tar</font>

### 目标

​	学习文件的压缩和解压

### 压缩文件扩展名

| 扩展名     | 分类                        |
| ---------- | --------------------------- |
| .zip或.rar | windows下压缩文件格式       |
| .tar       | Linux下打包文件(不一定压缩) |
| .gz        | Linux下压缩文件             |
| .tar.gz    | Linux下打包和压缩格式       |

 

### 打包并压缩文件

#### tar的参数

| 语法：tar [参数] 压缩包名 一个或多被打包的文件 | 功能                             |
| ---------------------------------------------- | -------------------------------- |
| 作用                                           | 压缩工具                         |
| -c                                             | 创建一个压缩包                   |
| -v                                             | 显示压缩的详情                   |
| -z                                             | 进行压缩(没有这个参数，只是打包) |
| -f <压缩文件名>                                | 指定压缩包的文件名               |

####  操作演示：

1. 定位于root目录，将当前目录下的Hello.java和Hello.txt文件打包成hello.tar文件，并显示详细信息。
2. 将当前目录下的Demo.* 打包并压缩成demo.tar.gz文件，显示详细信息。

#### 执行结果

![1553488875647](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488875647.png) 

![1553488881409](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553488881409.png) 

### 解压文件

#### 解压文件语法

| 语法1：tar [参数] 压缩文件 |
| -------------------------- |
| 解压文件到当前目录下       |


| 语法2：tar [参数] 压缩文件 -C 目录 | 参数说明             |
| ---------------------------------- | -------------------- |
| 解压到指定的目录下                 |                      |
| -x                                 | 解压                 |
| -v                                 | 显示解压详情         |
| -f<压缩文件>                       | 指定压缩包文件名     |
| -C                                 | 指定要解压到哪个目录 |

####  操作演示

1. 定位于root目录下，删除所有大写的Hello开头的文件

2. 解压hello.tar到当前目录

3. 释放demo.tar.gz文件到abc目录下

#### 执行结果

![1553489014878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553489014878.png) 

![1553489020319](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553489020319.png) 

### 小结

| tar参数的作用   | 参数说明         |
| --------------- | ---------------- |
| -c              | 创建压缩文件     |
| -v              | 显示详情         |
| -z              | 压缩             |
| -f <压缩文件名> | 指定压缩文件名   |
| -x              | 解压             |
| -C              | 解压到指定的目录 |



## 其它命令1

### 目标

学习命令pwd、ps、kill、top命令的使用

#### 查看当前绝对路径pwd

#### 语法格式

| pwd                                                          |
| ------------------------------------------------------------ |
| Print Working Directory 打印工作目录，显示当前是在哪个目录下 |

#### 操作演示

1. 进入根目录，显示当前的目录
2. 进入/bin，显示当前的目录
3. 进入/usr/bin目录，显示当前的目录

#### 执行结果

![1553490590589](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490590589.png) 



### 查看进程：ps

#### Linux进程启动的两种方式

1. 操作系统启动的时候自动启动的进程
2. 由用户在终端上(命令行中)输入的进程

#### bash进程

1. 每个用户登录以后都会分配一个终端操作的进程
2. 这个进程是所有终端命令的父进程bash，不要随意终止这个进程。

#### 语法格式

![1553588683900](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553588683900.png) 

#### 操作演示：

1. 在客户端中显示当前用户通过终端启动的所有进程
2. 在Linux命令行窗口运行vim Hello.txt编辑文件，在软件中显示所有用户通过终端启动的所有进程。
3. 显示所有用户通过终端启动的所有进程详细信息
4. 显示所有用户所有进程详细信息

#### 执行结果

![1553490699455](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490699455.png) 

![1553490716439](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490716439.png) 

#### 各列的说明

![1553490743216](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490743216.png)



### 杀死进程：kill

#### 语法格式

| kill  [参数]  进程号 | 功能：杀死进程                                          |
| -------------------- | ------------------------------------------------------- |
| 进程号               | 通过进程号来杀死进程                                    |
| -9                   | 如果直接输入进程号不能杀死进程，使用9的参数，强制进行。 |

#### 操作演示

1. 在Linux命令行上使用vim 编辑Hello.txt文件

   在客户端软件显示所有用户通过终端启动的所有进程，并杀死vim这个进程。

   在Linux命令行可以看到进程被终止

2. 在Linux命令行使用ping www.itcast.cn

   在客户端软件显示所有用户通过终端启动的所有进程，并强行杀死ping这个进程。

   在Linux命令行可以看到进程被杀掉

#### 执行结果

客户端命令行

![1553490882105](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490882105.png) 

Linux命令行，最后显示Terminated

![1553490892878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490892878.png) 

客户端命令行

![1553490909822](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490909822.png) 

Linux命令行，最后显示Killed

![1553490942799](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490942799.png) 



### 显示执行中的程序：top

#### 语法格式

| top命令作用 | 显示内存占用情况，每过1秒中刷新一次 |
| ----------- | ----------------------------------- |
| 按q         | 可以退出                            |



#### 操作演示

![1563702582166](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563702582166.png)

### 小结

| 说说以下命令的作用 | 说明                 |
| ------------------ | -------------------- |
| pwd                | 打印当前的工作目录   |
| ps                 | 查看进程             |
| kill               | 杀死进程，知道进程ID |
| top                | 显示内存状态         |



## 其它命令2

### 目标

学习其它命令grep,管道,关机,重启的作用

### 搜索文件内容grep

grep是一种强大的文本搜索工具，它能使用字符串搜索文本，并把匹配的行和行号打印出来。

- find命令：查找文件或目录
- grep命令：从文件中搜索指定的内容

#### 语法格式

| grep  [参数] 字符串 文件名 | 参数说明                           |
| -------------------------- | ---------------------------------- |
| 作用                       | 从指定的文件中搜索字符串所在的位置 |
| -n                         | 显示行号                           |
| -v                         | 显示不匹配行                       |
| -i                         | 区分大小写查找                     |

####  操作演示

1. 在Demo.java中搜索close字符串
2. 在Demo.java中搜索close字符串，并且显示行号
3. 在Demo.java中搜索没有close的行和行号
4. 在Demo.java中忽略大小写搜索insert字符串并且显示行号

#### 执行结果

![1553491140593](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491140593.png) 

![1553491160325](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491160325.png)



### 管道 |

#### 语法格式

| 语法 | 命令1 \| 命令2                                               |
| ---- | ------------------------------------------------------------ |
| 解释 | 管道至少有2条以上的命令，将1条命令的运行结果做为另1条命令的执行条件 |

#### 操作演示

1. 分页显示/etc目录所有文件的详细信息，将ll的输出做为more的输入，即分屏显示。
2. 在root目录下使用ll显示所有文件的详细信息，再在显示结果中使用grep查询Demo字符串
3. 显示Linux中所有进程的详细信息，查询ssh的字符串

####  执行结果

![1553491281710](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491281710.png)



### 查看内存

#### 语法格式

| 语法 | free -m            |
| ---- | ------------------ |
| 解释 | 显示内存的使用情况 |
| -m   | 以M为单位显示      |

#### 效果

![1563703317558](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703317558.png)

#### 各项说明

![1563703366041](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703366041.png)



### 查看硬盘df

#### 语法格式

| 语法    | df                           |
| ------- | ---------------------------- |
| 解释    | disk free 显示硬盘的使用情况 |
| -h      | human 更符合人类的阅读习惯   |
| --total | 在最后显示总数               |

#### 效果

![1563703525205](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703525205.png)



### 关机

#### 语法格式

| 语法 | shutdown now |
| ---- | ------------ |
| 解释 | 关机         |

#### 类似于以下操作

![1553491455398](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491455398.png) 

### 重启

#### 语法格式

| 语法 | reboot |
| ---- | ------ |
| 解释 | 重启   |

#### 类似于以下操作

![1553491489388](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491489388.png) 

### 命令小结

| 命令         | 功能                             |
| ------------ | -------------------------------- |
| grep         | 搜索文件中内容，只显示匹配的内容 |
| \|           | 管道                             |
| free         | 显示内存使用情况                 |
| df           | 显示硬盘的使用情况               |
| shutdown now | 关机                             |
| reboot       | 重启                             |



## 文件权限的操作

### 目标

1. 了解文件的权限

2. 学习操作文件权限的命令 chmod

### 用户和组

哪些用户对哪些目录或文件有访问的权限

### 分配权限的对象

| 概念     | 解释                      |
| -------- | ------------------------- |
| 属主     | User，文件或目录主人      |
| 属组     | Group，主人所在的组       |
| 其他用户 | Other，除了上面之外的用户 |

![1553491628988](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491628988.png)

### 权限的说明

#### 9个字母的含义

![1553491678584](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491678584.png)



#### 权限的范围

| 权限范围 | 说明               |
| -------- | ------------------ |
| u        | User 属主          |
| g        | Group 属组         |
| o        | Other 其他用户和组 |
| a        | All 所有人         |

#### 权限的操作

| 权限符号 | 说明     |
| -------- | -------- |
| +        | 添加权限 |
| =        | 修改权限 |
| -        | 删除权限 |

#### 权限的字母和数字

| 权限字母 | 权限数字 | 说明         |
| -------- | -------- | ------------ |
| r        | 4        | Read 读取    |
| w        | 2        | Write 写入   |
| x        | 1        | eXecute 执行 |
| -        | 0        | 没有权限     |

### 添加权限

#### 语法格式

![1553491810659](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491810659.png) 

#### 操作演示

1. 给Demo.java文件的拥有者添加执行权限
2. 给Hello.java所有的用户添加所有的权限 
3. 给Demo.html拥有者添加执行权限，其它用户添加写权限

#### 执行结果

![1553491851031](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491851031.png) 

![1553491863031](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491863031.png) 

![1553491874529](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491874529.png) 

### 修改权限

#### 语法格式

![1553491902313](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491902313.png) 

#### 操作演示

1. 给Hello.txt拥有者改为读写执行权限，所在的组有写的权限，其它用户有执行的权限
2. 修改Hello.txt的权限，使用数字的方式给拥有者，所在组，其它组都是读写权限

#### 执行结果

![1553491951166](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491951166.png) 

### 删除权限

#### 语法格式

![1553491979181](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491979181.png) 

#### 操作演示

1. 删除Hello.txt拥有者写入的权限，用户组写入权限
2. 使用数字的方式删除Demo.java所有的权限

#### 执行效果

![1553492014975](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492014975.png) 

### 小结

chmod修改权限的命令

| 权限范围 | 说明           |
| -------- | -------------- |
| u        | User 属主      |
| g        | Group 属组     |
| o        | Other 其他用户 |
| a        | All 所有用户   |

| 操作符号 | 说明 |
| -------- | ---- |
| +        | 添加 |
| =        | 修改 |
| -        | 删除 |

| 权限字母 | 说明   |
| -------- | ------ |
| r        | 读取 4 |
| w        | 写入 2 |
| x        | 执行 1 |



## crontab任务的编写格式

### 目标

了解定时任务的格式

### crontab命令参数

- 作用：制定定时任务，每过一段时间执行一次指定的任务。(JavaScript：setInterval())

#### 语法格式

| crontab [参数] | 参数说明             |
| -------------- | -------------------- |
| -l             | 显示已经有的定时任务 |
| -e             | 进入编辑状态         |
| -r             | 删除定时任务         |

####  操作演示

1. 显示当前root用户的定时任务
2. 进入任务编辑状态 

#### 执行结果

![1553492175415](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492175415.png) 

 

### 定时任务的配置

#### 定时任务的说明

1. 每一行创建一个任务
2. 时间格式：分 时 日 月 周 命令

#### 格式说明

![1553492263673](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492263673.png)

#### 特殊字符

![1553492281630](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492281630.png)

#### 解释以下配置的含义

```
* * * * * command
每分钟执行1次
```



```
30 21 * * * command
每天晚上的9点半执行
```



```
3,15 * * * * command
每小时的第3分钟和第15分钟
```



```
5,10 8-11 * * * command
每天的8点到11点的第5分钟和第10分钟执行
```



```
*/2 * * * * command
每2分钟执行一次
```



```
10 7 * 3,4,7 0 command
每年3月，4月，7月的星期天的上午7点过10分执行
```



### crontab案例

#### 案例需求

每隔一分钟，让Linux输出当前的系统时间到/root/mydate.log文件中。

#### 操作步骤

1. 输入crontab -e后，会启动vi编辑器，来编写新的定时任务，一行写一个定时任务。

   ```
   * * * * * date >> /root/mydate.log
   ```

2. 保存并退出vi编辑器后，定时任务立刻生效。

3. 等几分钟，显示mydate.log文件的内容

4. 最后删除当前的定时任务

#### 执行结果

1. 编辑后显示的命令行
2. 过几分钟查看mydate.log的文件内容

### crontab命令的格式小结

![1553492441756](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492441756.png) 



## 系统服务的管理

### 目标

1. 定时器服务的管理

2. 设置后台服务的自启动

### 定时服务的管理

crontab如果安装到Linux系统上，默认是开启服务的，会消耗一定的资源。类似于Windows下的服务：

![1553493038354](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493038354.png) 

#### 定时服务管理的命令

| crond服务管理命令         | 说明                         |
| ------------------------- | ---------------------------- |
| systemctl   start crond   | 开启定时服务                 |
| systemctl  stop   crond   | 停止定时服务                 |
| systemctl   status crond  | 显示定时服务状态             |
| systemctl restart   crond | 重启定时服务，先关闭，再开启 |
| systemctl   reload crond  | 重启加载定时服务的任务       |

#### 操作演示

1. 查看定时器服务的状态，默认处理开启状态
2. 关闭定时服务后，查看服务的状态
3. 重启服务定时服务，查看服务的状态

#### 执行结果

![1553493097878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493097878.png)

### 设置后台服务的自启动配置

类似于Windows下服务的自启动

![1553493113342](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493113342.png) 

#### 语法格式

| 命令                              | 说明                   |
| --------------------------------- | ---------------------- |
| systemctl   list-units \*.service | 显示所有正在运行中服务 |
| systemctl   enable 服务名         | 将服务设置为开机自启动 |
| systemctl   disable 服务名        | 关闭服务的开机自启动   |

#### 操作演示

1. 查看某项指定的服务是否开启，如crond.service
2. 禁止crond服务开机自启动；重新启动linux；查看crond服务是否已经开启
3. 再次将crond服务设置为开机自启动；重新启动linux；查看crond服务是否已经加载

#### 执行效果

![1553493187975](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493187975.png)

### 小结

![1553493208872](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493208872.png) 



## 网络服务的管理

### 目标

学习与网络服务有关的命令

### 操作命令

| 命令                      | 说明             |
| ------------------------- | ---------------- |
| systemctl start network   | 开启网络服务     |
| systemctl stop network    | 停止网络服务     |
| systemctl restart network | 重启网络服务     |
| systemctl status network  | 显示网络服务状态 |

### 操作演示

1. 查看网络当前的状态
2. ping www.baidu.com看能否ping通
3. 停止网络服务器，看能否ping通外网
4. 再次启动网络服务器 

### 执行结果

![1553589307554](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589307554.png) 

![1553589315904](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589315904.png) 

![1553589320877](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589320877.png) 

### 小结

| 命令                        | 说明             |
| --------------------------- | ---------------- |
| systemctl   start network   | 开启网络服务     |
| systemctl stop   network    | 停止网络服务     |
| systemctl   restart network | 重启网络服务     |
| systemctl   status network  | 显示网络服务状态 |



## 网卡的激活与关闭

### 目标

网卡的激活与关闭

### IP地址配置

如果别人想连接到你的机器，就必须知道你机器的IP地址。如果你的机器想要访问外网，就必须激活外网的网卡。

### 安装CentOS时关于网络的配置

如果你的网卡在CentOS安装的时候没有激活，则在虚拟机中无法连接网络。必须在Linux中进行配置，才能激活外网的网卡。                                                          

### 示例：关闭网卡

1. 查看网络配置，显示外网处于激活状态

```
命令：ip addr或ifconfig
```

2. 修改ifcfg-ens33文件可以修改网卡的激活状态

```
命令：vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

3. ONBOOT用于修改网卡激活的状态：no表示不激活，yes表示激活，我们先设置为no。
4. 重启网络服务 
5. 外网变成未激活状态
6. ping一下外网看到外网不能连接

```
命令：ping www.itcast.cn
```

### 执行结果

- 激活状态

![1553589480487](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589480487.png)    

- 修改配置文件

![1553589505916](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589505916.png)    

- 重启网络服务

  ![1553589519551](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589519551.png) 

- 查看变成未激活状态

  ![1553589532015](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589532015.png)  

- 网络连接失败

![1553589541764](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589541764.png)    

 

### 示例：激活网卡

1. 修改 /etc/sysconfig/network-scripts/ifcfg-ens33 配置文件，将ONBOOT="yes"。

```
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

2. 重启网络服务
3. 查看网卡已经激活
4. 测试外网网络是否连通

### 执行结果   ![1553589574901](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589574901.png)   

### 小结

1. 网卡设备名是什么

   ens33

2. 激活要设置哪个参数

   onboot=yes



## <font color="red">配置静态IP</font>

### 目标

1. 如何配置静态IP地址
2. 如何克隆虚拟机

### 配置静态IP地址

设置ip分为2种类型，dhcp和static。dhcp是动态获取ip，static是配置静态ip。dhcp动态获取ip可能ip经常会发生变化,导致客户端无法连接到。静态ip配置后就不会发生改变，这样客户端连接服务器具有更好的安全性。

### 操作步骤

1. 查看当前虚拟机网关（记住这个网关，后面使用）      ![1553589780321](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589780321.png)                                                     

2. 进入目录命令：cd /etc/sysconfig/network-scripts/

   ![1553589800921](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589800921.png) 

3. 编辑网卡配置文件命令：vim ifcfg-ens33

   ![1553589814267](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589814267.png)   

4. 配置静态IP，增加修改如下信息：

   修改的内容：

   ```
   BOOTPROTO=static
   ```

   增加的内容：

   ```
   IPADDR=192.168.248.99
   GATEWAY=192.168.248.2
   NETMASK=255.255.255.0
   DNS1=8.8.8.8
   DNS2=114.114.114.114
   ```

   

5. 重启网卡服务

   ![1553590702140](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553590702140.png) 

 

### 执行结果

1. 查看ip

   ![1553590714781](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553590714781.png) 

2. Ping外网，如下信息说明可以连接外网

   ![1553590723621](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553590723621.png) 

 

### 小结

```
BOOTPROTO=static

IPADDR=192.168.248.99
GATEWAY=192.168.248.2
NETMASK=255.255.255.0
DNS1=8.8.8.8
DNS2=114.114.114.114
```





# 学习总结

1. 能够独立搭建Linux环境

2. 安装VMware

   2. 在VM创建虚拟机
   3. 在虚拟机中安装CentOS7
   4. 安装客户端，连接到服务器上

   

3. 能够使用Linux进行目录操作的命令

| 命令  | 功能                 |
| ----- | -------------------- |
| cd    | 改变目录             |
| mkdir | 创建目录             |
| ls    | 查看目录             |
| find  | 查找目录和子目录文件 |
| mv    | 移动和改名           |
| cp    | 复制                 |
| rm    | 删除                 |

3. 能够使用Linux进行文件操作的命令

| 命令         | 功能         |
| ------------ | ------------ |
| cat 文件名   | 显示内容     |
| more 文件名  | 分屏显示     |
| head 文件名  | 显示前10行   |
| tail 文件名  | 显示后面10行 |
| less 文件名  | 分页显示     |
| touch 文件名 | 创建空文件   |

4. 能够使用Linux进行目录文件压缩和解压的命令

| tar参数的作用   | 功能             |
| --------------- | ---------------- |
| -c              | 创建压缩包       |
| -v              | 显示详情         |
| -z              | 压缩             |
| -f <压缩文件名> | 指定压缩包名字   |
| x               | 解压             |
| -C              | 解压到指定的目录 |

5. 能够使用Linux进行目录文件权限的命令

| chmod 用户或组 操作 权限 目录或文件 | 说明    |
| ----------------------------------- | ------- |
| 用户或组                            | u g o a |
| 操作                                | + = -   |
| 权限                                | r w x   |

6. 能够使用其它常用的Linux命令

| 命令         | 功能                         |
| ------------ | ---------------------------- |
| grep         | 查找字符串，只显示找到的内容 |
| \|           | 管道                         |
| shutdown now | 关机                         |
| reboot       | 重启                         |
| ps           | 显示进程                     |
| kill         | 杀死进程                     |
| pwd          | 显示路径                     |
| top          | 显示内存情况，每隔一秒刷新   |
| free         | 显示内存情况                 |
| df           | 显示硬盘的使用情况           |

7. 能够使用客户端工具连接Linux系统

   

8. 能够使用Linux中的crontab命令

| crontab [参数] | 定时执行任务 |
| -------------- | ------------ |
| -l             | 显示任务列表 |
| -e             | 编辑任务     |
| -r             | 删除任务     |

每行任务的格式：

 ![1553492441756](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492441756.png)