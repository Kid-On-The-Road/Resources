# Linux高级-笔记

# 回顾

## 目录操作命令

| 命令名 | 功能             |
| ------ | ---------------- |
| cd     | 改变目录         |
| mkdir  | 创建目录         |
| ls     | 显示目录         |
| find   | 查找文件和子目录 |
| mv     | 移动或改名       |
| cp     | 复制             |
| rm     | 删除             |

 

## 文件操作命令

| 命令名 | 功能       |
| ------ | ---------- |
| cat    | 显示内容   |
| more   | 分屏显示   |
| less   | 分页显示   |
| touch  | 创建空文件 |
| tar    | 压缩和解压 |

### vim编辑文件

#### 三种模式

命令，编辑，底行

#### 模式切换

命令->编辑： i a o

编辑->命令：Esc

命令->底行：冒号

#### 常用的命令

```
yy 复制
p 粘贴
dd 删除
u 撤销
/字符串 搜索指定的字符串
```



## 其他命令

| 命令名       | 功能             |
| ------------ | ---------------- |
| pwd          | 打印当前工作目录 |
| ps           | 显示进程         |
| kill         | 杀死进程         |
| free         | 显示内存         |
| df           | 显示硬盘使用情况 |
| shutdown now | 关机             |
| reboot       | 重启             |

 

## 文件权限

| 命令名 | 功能                                                         |
| ------ | ------------------------------------------------------------ |
| chmod  | 角色：User Group Other All<br />操作：+ = -<br />权限：r w x |

 

## 服务管理的命令

| 服务管理命令               | 功能           |
| -------------------------- | -------------- |
| systemctl start 服务名     | 开启服务       |
| systemctl stop 服务名      | 停止服务       |
| systemctl   status 服务名  | 显示服务状态   |
| systemctl   enable 服务名  | 开机自启动服务 |
| systemctl   disable 服务名 | 关闭开机自启动 |

 

# 学习目标

1. 与网络有关的命令
   1. 能够使用Linux中的网络管理命令
   2. 能够使用Linux中防火墙配置命令
   3. 能够使用Linux中SSH免密登录命令
2. 能够在Linux安装各种软件
   1. 能够在Linux系统中安装jdk软件
   2. 能够在Linux系统中安装mysql软件
   3. 能够在Linux系统中安装tomcat软件
   4. 能够在linux系统中安装redis软件
   5. 能够在Linux系统的tomcat中部署发布项目
3. 能够使用Linux中基本用户管理命令



# 主机名的修改

## 目标

1. 修改服务器主机名

2. 网络服务的开启和关闭

## 主机名配置

​	需求：默认主机名是localhost，我们希望改为有含义的主机名。如：heima，itcast等主机名。类似于Windows中修改主机名：  ![1553589164649](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553589164649.png)                                          

### 语法格式

| 修改主机名                          | 说明                         |
| ----------------------------------- | ---------------------------- |
| hostname                            | 显示当前的主机名             |
| hostname 新主机名                   | 临时修改主机名，重启以后失效 |
| hostnamectl   set-hostname 新主机名 | 永久修改主机名               |

### 操作演示：

1. 查看当前的主机名
2. 设置新的主机名为itheima，再查看当前的主机名
3. 设置新的主机名为itheima，重启以后仍然有效

### 执行结果 ![1553589230441](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553589230441.png) 

## 小结

| 命令                                | 说明           |
| ----------------------------------- | -------------- |
| hostname 新主机名                   | 临时修改主机名 |
| hostnamectl   set-hostname 新主机名 | 永久修改主机名 |




# 域名映射

## 目标

在Linux中配置服务器的域名

## 为什么要配置域名

我们希望通过域名来访问互联网的主机，而不是输入又长又难记的IP地址。

![1553591048253](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591048253.png)                                                         

## 域名访问网站的过程

![1573177418290](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1573177418290.png)

Windows下本机的hosts文件是 c:\windows\System32\drivers\etc\hosts，这是一个文本文件。



## 修改Linux域名映射文件

在Linux中，/etc/hosts文件用于在主机名与IP地址之间进行映射。所以你想访问一个什么样的主机名，就需要把这个主机名和它对应的IP地址配置在/etc/hosts文件中。

## 操作步骤

1. 首先ping newboy看能否访问这个地址
2. 修改hosts文件命令：vim /etc/hosts
3. 将本机局域网的ip地址与newboy这个名字进行映射。一个ip地址后面可以映射多个域名地址。
4. 再次使用ping newboy，发现可以访问这个名字。实现域名与ip地址的映射。 

## 执行结果

![1553591104227](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591104227.png)   

## 小结

域名映射修改哪个文件

```
/etc/hosts
```



# 查看网络进程

## 目标

1. 如何查看活动的网络进程

2. 理解管理防火墙的命令

## 查看网络进程

### 语法格式

| netstat [参数] | 参数说明 | 功能                        |
| -------------- | -------- | --------------------------- |
| 无参           |          | 正在访问网络的所有的进程    |
| -n             | number   | 同时显示IP地址和端口号      |
| -t             | tcp      | 只显示使用TCP协议的网络进程 |
| -l             | listener | 显示正在监听中的进程        |
| -p             | programs | 同时显示程序的名字和PID     |

### 操作演示

1. 显示TCP协议的网络连接
2. 显示TCP协议和监听中的网络连接
3. 显示TCP协议和监听中的网络连接，显示IP地址和端口号
4. 显示TCP协议和监听中的网络连接，显示IP地址和端口号，显示程序的名字

### 执行结果

![1553591523827](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591523827.png)

![1553591539168](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591539168.png)

## 小结

| netstat [参数] | 显示所有访问网络的进程信息    |
| -------------- | ----------------------------- |
| 无参           | 显示所有的网络进程            |
| -n             | number 显示IP地址和端口号     |
| -t             | tcp 只显示TCP协议             |
| -l             | listener 同时再显示监听中进程 |
| -p             | programs 还显示进程的ID和名字 |



# 虚拟机快照

## 目标

1. 什么是快照
2. 快照的操作

## 介绍

虚拟机“快照”是虚拟机磁盘文件在某个时间点及时的副本备份。系统崩溃或系统异常，你可以使用恢复到快照指定时间点系统状态。

## 工具栏

![1563793774832](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563793774832.png) 

## 定义快照

![1563793797124](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563793797124.png) 

![1563793823429](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563793823429.png) 



## 使用快照

当系统环境崩溃或系统异常时，我们希望当前系统恢复到以前某个时间点正常状态，可以使用快照进行恢复

![1563793859264](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563793859264.png)



## 小结

快照的作用是什么？

```
保存系统的某个状态，以后可以还原到当前的状态
```



#克隆虚拟机

## 概念

就是复制已有的虚拟机系统创建出一个一模一样的虚拟机系统。

## 作用

克隆可以快速复制创建出多台linux系统，便于后面我们学习负载均衡多台linux系统进行测试。

## 实现步骤

1. 关闭现有itheima虚拟机

   ![1553590763233](/assets/1553590763233.png) 

2. 点击虚拟机itheima,出现如下效果

   ![1553590771853](/assets/1553590771853.png)      

3. 如图操作

   ![1553590831620](/assets/1553590831620.png)            

4. 点击下一步

   ![1553590847667](/assets/1553590847667.png) 

5. 点击下一步

   ![1553590856740](/assets/1553590856740.png) 

6. 如图操作下一步

   ![1553590862646](/assets/1553590862646.png) 

7. 如下图操作

   ![1553590870726](/assets/1553590870726.png) 

   ![1553590875407](/assets/1553590875407.png) 

   ![1553590882897](/assets/1553590882897.png) 

8. 效果

   ![1553590891609](/assets/1553590891609.png)     

9. 启动itheima2虚拟机

10. 执行命令：vim /etc/sysconfig/network-scripts/ifcfg-ens33

    修改其中配置的固定ip，避免与itheima虚拟机ip冲突

    ![1553590899337](/assets/1553590899337.png)		 	          

11. 可选：修改主机名

12. 重启网络服务

    ![1553590917847](/assets/1553590917847.png) 

## 小结

1. 为什么要克隆？

   快速的创建一台新的虚拟机

2. 克隆虚拟机后要做什么事情？

   1. 修改IP地址
   2. 修改主机名



# <font color="red">防火墙配置</font>

## 目标

1. 防火墙的管理命令
2. 开放端口允许外部连接
3. 移除端口不允许外部连接

## 介绍

防火墙类似于一个关卡检查人员，当你访问其他人的电脑，或者其他人访问你的电脑，都要进行拦截并进行处理，有的阻止，有的放行，有的转发。默认情况下防火墙在开机以后就自动启动了。

### 语法格式

| 命令                          | 作用             |
| ----------------------------- | ---------------- |
| systemctl start   firewalld   | 开启防火墙       |
| systemctl stop   firewalld    | 关闭防火墙       |
| systemctl   enable firewalld  | 开机自启动防火墙 |
| systemctl   disable firewalld | 关闭开机自启动   |
| systemctl   status firewalld  | 显示防火墙状态   |

### 操作演示

1. 确认当前是管理员的账户，查看防火墙当前的状态
2. 关闭防火墙，再查看防火墙的状态
3. 再次开启防火墙，查看防火墙的状态

### 执行结果

![1553591585545](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591585545.png)   



## <font color="red">firewall-cmd</font>

 外网或内网需要连接到当前系统内的程序进行操作，需要linux系统开放程序端口，否则无法访问。

| firewall-cmd           | 打开或关闭防火墙中指定的端口号，让端口号可以访问             |
| ---------------------- | ------------------------------------------------------------ |
| --zone                 | public: 开放的是外网，所有外部客户端都可以访问，默认值<br />internal: 开放的是内网，局域网 |
| --add-port=端口/tcp    | 打开指定的端口                                               |
| --remove-port=端口/tcp | 关闭指定的端口                                               |
| --permanent            | 永久的打开或关闭                                             |
| --list-all             | 显示已经打开的端口号                                         |
| --reload               | 重启加载防火墙的规则，让新的规则起作用                       |

### 步骤

1. 永久开放443端口，添加到公开区域，允许外部连接
2. 重新加载防火墙的规则
3. 显示所有打开的端口号 
4. 从公共区域中，永久移除443端口，不允许外部连接
5. 重新加载防火墙规则
6. 显示打开的端口号

### 效果

![1563798960456](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563798960456.png) 

![1563799088361](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563799088361.png)

##  小结

说说下面代码的作用

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
添加8080的端口到公共区域，永久添加

firewall-cmd --reload
重新加载新的规则

firewall-cmd --list-all
显示防火墙端口信息
```



# SSH口令方式登录

## 目标

1. 输入密码登录另一台服务器
2. 免密登录的原理
3. 使用免密登录另一台服务器

## 为什么要登录

需要在同一个客户端，或不同的Linux之间相互访问。需要在一台Linux上登录另一台Linux.

Linux之间登录使用SSH协议，Secure Shell Protocol 安全壳协议，是一种在Linux中传输数据的加密协议。

## SSH工作机制

SSH登录有两种验证机制：

1)    输入用户名和密码的登录方式  

2)     免密码登录

## 基于口令的安全验证

只要你知道另一台机器上用户的帐号和口令，就可以登录到远程主机。

| 命令           | 功能                           |
| -------------- | ------------------------------ |
| ssh 服务器地址 | 一个Linux登录另一台Linux的命令 |

### 具体操作：

itheima这台机器有密登录itcast这台机器

 ![1553591730548](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591730548.png)                                                  

### 操作演示：

1. 先查看itcast这台机器的ip地址，假设是192.168.56.101
2. 在客户端itheima这台机器中输入：ssh 192.168.56.101 连接itcast服务端机器。
3. 输入root用户在itcast服务端 192.168.56.101这台机器上的密码：root。登录成功，后期操作的就是itcast这台机器了。
4. 如果输入exit，则退出itcast 192.168.56.101这台机器，回到itheima客户端这台机器。
5. 这时在itheima这台机器下会生成一个隐藏的.ssh目录，下面有一个known_hosts文件，用于保存itcast这台机器的数字签名。

### 执行效果 

![1553591776267](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591776267.png)  

## 小结

| 命令           | 功能           |
| -------------- | -------------- |
| ssh 服务器地址 | 登录另一台主机 |



# SSH免密方式登录

​       如果网络中Linux服务器比较多，需要记住每台服务器的密码也是比较痛苦的事。你无需知道另一台机器上的帐号和口令，也可以登录到远程主机。我们说的SSH免密登录，就说的是这种方式。

### 公钥与私钥

1. 公钥和私钥是成对出现
2. 公钥是公开的，给所有人使用，私钥是私有的，只有自己知道。
3. 公钥这里用于加密，私钥用于解密

### 免密登录的原理

 ![1573183224287](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1573183224287.png)

###  操作指令

| 命令                    | 说明                       |
| ----------------------- | -------------------------- |
| ssh-keygen              | 在客户端生成一对公钥和私钥 |
| ssh-copy-id  服务器地址 | 将公钥发送给服务器         |

###  客户端免密登录服务器

需求：itheima这台机器免密登录itcast这台机器

![1553591929818](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591929818.png)    

### 生成公钥和私钥：

1. 在客户端itheima生成公钥和私钥
2. 客户端itheima将公钥发送给itcast服务器端保存
3. 以后客户端itheima就可以免密登录itcast服务端

### 免密登录操作

1. 在itheima客户端这台机器上输入：ssh-keygen，如有提示，按回车，生成itheima客户端这台机器的公钥和私钥。生成的公钥和私钥在root/.ssh目录下。id_rsa是私钥，id_rsa.pub是公钥。
2. 在itheima客户端这台机器上输入：ssh-copy-id 192.168.56.101, itcast的ip地址，按回车，将刚刚生成的itheima这台机器的公钥复制到itcast服务器这台机器的用户目录下，保存在root/.ssh/authorized_keys文件中。这个文件的内容与生成的公钥id_rsa.pub是一样的。
3. 输入itcast这台服务器的登录密码，这时免密配置成功。
4. 在itheima客户端这台机器上输入：ssh 192.168.56.101，再也不用密码了。 

### 免密登录执行结果

 ![1553591975598](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591975598.png)   

![1553591995049](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591995049.png)

##  小结

| 命令                    | 功能               |
| ----------------------- | ------------------ |
| ssh-keygen              | 生成一对公钥和私钥 |
| ssh-copy-id  服务器地址 | 把公钥发送给服务器 |



# 用户管理：创建用户、设置密码、切换用户

## 目标

1.     创建用户

2.     设置用户密码

3.     不同用户之间的切换

## 添加用户

### 语法格式

| useradd [参数] 用户名 | 创建一个普通用户(adduser是它的别名)                          |
| --------------------- | ------------------------------------------------------------ |
| -m                    | 创建用户的同时，在home下创建它的主目录                       |
| -g <组名>             | 指定用户在哪个组中，如果未指定组名：默认创建一个与用户名相同的组名 |
| 说明                  | 创建好的用户信息在/etc/passwd文件中                          |

### 操作演示

1. 进入/home目录，查看目录的内容
2. 创建用户Jack，并且创建用户主目录 
3. 再次ll查看/home目录下存在Jack目录，默认用户Jack在一个叫Jack的组中。
4. 创建用户Tom，把用户放在Jack这个组中，并且创建Tom主目录
5. 通过ll查看/home目录下的信息
6. 查看/etc/passwd文件，可以看到创建的用户信息

### 执行结果

![1553674003887](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674003887.png)                                                   

/etc/passwd文件内容

![1553674029790](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674029790.png)



## 设置密码

### 语法格式

| passwd [用户名] | 功能                     |
| --------------- | ------------------------ |
| 无参            | 修改自己的密码           |
| 用户名          | 管理员可以修改其他的密码 |

### 操作演示

1. 设置Jack的密码为abc123。如果提示无效的密码，忽略即可，再次输入同一个密码，修改成功。
2. 在Linux控制台先logout登出。以Jack登录，注：用户名和密码大小写是敏感的

- 普通用户命令行前面的提示符是$，管理员命令提示符是#，用户主目录显示为~

### 执行结果

![1553674118909](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674118909.png)    



## 切换用户

### 语法格式

| su 用户账号 | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| 作用        | 切换到指定的用户<br />管理员->普通用户不用输入密码<br />普通用户->管理员必须要输入密码 |

### 操作演示

1. 当前用户是root，临时切换成Jack，不用输入密码
2. cd进入用户主目录，pwd显示当前的目录，观察前面的提示符
3. 从Jack用户切换回root，需要输入密码。
4. cd进入用户主目录，pwd显示当前的目录
5. 输入2次exit再回到root用户

### 执行结果

![1553674186237](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674186237.png)    

## 小结

1.     添加用户的命令是什么：useradd

2.     设置密码的命令是什么：passwd

3.     切换用户的命令是什么： su



# 用户管理：删除用户、修改登录名字

## 目标

1.     删除用户

2.     修改用户登录的名字

## 删除用户

### 语法格式

| userdel [-r] [用户帐号] | 删除指定的用户       |
| ----------------------- | -------------------- |
| -r                      | 同时删除用户的主目录 |

### 操作演示

1. 使用root删除用户Tom，同时删除Tom的主目录。如果Tom已经登录，则删除失败
2. 在/home目录下已经找不到Tom主目录了

### 执行结果

 ![1553674298200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674298200.png)                                                  



## 修改登录名

### 语法格式

| usermod -l 新登录名 原登录名 | 功能             |
| ---------------------------- | ---------------- |
| 作用                         | 修改用户的信息   |
| -l <新登录名>                | 修改用户的登录名 |

### 操作演示

1. 确认Jack这个用户不在线，如果在线需要将Jack登出
2. 修改Jack的登录名为Rose
3. 修改以后在Linux控制台，使用Rose重新登录

### 执行结果

![1553674380096](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674380096.png)       

## 小结

1. 删除用户的名字：userdel

2. 修改用户登录的名字: usermod



# 上午的回顾

## 主机名的修改

```
hostname 显示主机名
hostname 主机名 
hostnamectl set-hostname 主机名
```



## 域名映射

```
修改/etc/hosts文件
将IP地址与指定的域名映射
```



## 查看网络进程

```
netstat 显示所有访问网络的进程
-t TCP
-l listener
-n number IP地址和端口号
-p program 进程名字和PID
```



## 虚拟机创建快照和克隆

创建快照目的：用于快速还原到系统的某个状态

克隆的目的：快速创建一台新的虚拟机

1. 修改IP地址
2. 修改主机名



## 防火墙配置

1. 防火墙服务管理

   ```
   systemctl start firewalld
   stop, restart,enable,disable
   ```

2. 防火墙的操作命令

   ```
   firewall-cmd
   --zone=public/internal 默认是public
   --add-port=端口号/tcp 
   --remove-port=端口号/tcp
   --list-all 
   --reload
   ```

   

## SSH免密登录

```
ssh 服务器名
ssh-keygen 生成公钥和私钥
ssh-copy-id 将公钥发送给服务器
```



## 用户管理命令

```
创建用户：useradd或adduser
设置密码：passwd
切换用户：su 用户名
删除用户：userdel
修改登录名：usermod
```



# 组的管理：创建组、修改用户所在组、将用户从组中删除

## 目标

1. 创建一个组

2. 修改用户所在的组

3. 将用户从组中删除

## 添加组

### 语法格式

| groupadd 组名 | 说明                                   |
| ------------- | -------------------------------------- |
| 作用          | 创建一个组                             |
| 说明          | /etc/group文件，包含了所有创建组的信息 |

### 操作演示

1. 添加一个新的组America
2. 查看/etc/group文件，做为主组的用户不会显示出来。

### 执行结果

 ![1553674497860](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674497860.png)                                                  



## 修改用户所在组

注：一个用户只能有一个主组，可以有多个从组。

### 语法格式

| usermod 参数 组名 用户名 | 修改用户所在的组               |
| ------------------------ | ------------------------------ |
| -g<组名>                 | 修改用户的主组                 |
| -G<组1,组2>              | 修改用户的从组，可以有多个从组 |

### 操作演示

1. 进入/home目录，ll显示home目录的内容可以看到Rose是属于Jack这个组
2. 将Rose这个用户的主组Jack修改为America组
3. 使用ll显示/home目录，查看Rose用户所属组名变成了America
4. 创建两个组：China和Japan
5. 将Rose添加进China和Japan两个从组中，组名之间使用逗号隔开。
6. 查看/etc/group文件，查看到Rose与组的关系。文件中主组看不到用户的列表。

### 执行结果

![1553674569689](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674569689.png)    ![1553674576382](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674576382.png)   



## 将用户从组中删除

注：用户无法从主组中删除，只可以修改主组，可以从从组中删除用户。

### 语法格式

| gpasswd -d 用户名 组名 | 功能                     |
| ---------------------- | ------------------------ |
| -d<用户名>             | 将用户从指定的从组中删除 |

### 操作演示

1. 把Rose从America这个组中删除，删除失败。
2. 把Rose从China这个组中删除
3. 查看/etc/group文件，发现Rose已经从China组中删除

### 执行结果

 ![1553674644560](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674644560.png) 

![1553674649432](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674649432.png)     

## 小结

1. 添加组：groupadd
2. 修改用户所在组：usermod 
3. 将用户从组中删除：gpasswd



# 组的管理：修改组名字，删除组，使用sudo权限

## 目标

1.     修改组的名字

2.     删除组

3.     临时使用管理员命令

## 修改组的名字

### 语法格式

| groupmod -n 新组名 原组名 | 功能         |
| ------------------------- | ------------ |
| -n <新组名>               | 修改组的名字 |

### 操作演示

1. 在/home目录下ll查看修改前的信息
2. 将Rose的主组名America改成USA
3. 查看/home目录信息，发现Rose的组名已经改成USA

### 执行结果

![1553674741115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674741115.png)                                                   



## 删除组

删除的组的前提是：没有用户把它做为主组，如果要删除主组，则必须先删除这些用户后或将用户移动到其他主组中，才能删除主组。

### 语法格式

| groupdel 组名 | 功能   |
| ------------- | ------ |
| 作用          | 删除组 |

### 操作演示

1. 删除USA组，删除失败，因为还有Rose把它做为主组
2. 删除Japan这个组
3. 删除China这个组
4. 查看/etc/group文件，发现没有这两个组的信息了

### 执行结果

![1553674787514](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674787514.png) 



## 设置sudo权限

### 普通用户没有权限的演示

1. 使用su切换用户到Rose，进入自己的用户目录
2. 在主目录下创建一个文件rose.txt，可以创建成功。
3. 添加一个用户Mary，发现创建失败。
4. 退出Rose用户，返回到root 用户

### 执行结果

![1553674824780](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674824780.png)    

### sudo权限的说明

Rose是普通用户，可以使用基本的命令，如：创建文件等操作，但不能执行系统级别的指令。



### 语法格式

| sudo 系统命令 | 可以执行管理员命令                   |
| ------------- | ------------------------------------ |
| 作用          | 使用命令的前提：必须要给用户赋予权限 |



### 设置普通用户sudo权限的步骤

1)  编辑/etc/sudoers文件，在root ALL=(ALL) ALL下复制一行(92行)，将用户名改成Rose，因为是只读的文件，所以要强制保存退出 :wq!

```
Rose ALL=(ALL) ALL
```

2)  切换回Rose用户，进入/home目录。ll查看目录下目前的状态。

3)  使用sudo命令添加一个用户Mary，这时需要输入Rose的密码才能操作成功。

```
sudo useradd  -m  Mary
```

4) 进入/home查看已经创建Mary用户和它的主目录

5) 退回Rose，回到管理员root的账户

### 执行结果![1553674912481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674912481.png)

### ![1553674930825](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674930825.png)    

## 小结

1. 修改组的名字：groupmod
2. 删除组：groupdel
3. 执行管理员权限的命令：sudo




# JDK安装

## 目标

在Linux下安装JDK 

## JDK安装步骤

1. 进入“/soft”目录，解压jdk到指定目录/usr/local下

```
tar -xvf jdk-8u221-linux-x64.tar.gz -C /usr/local/
```



2. 查看解压后的目录,目录中有jdk1.8.0_221为jdk解压的目录

   ![1570534275516](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1570534275516.png)



3. 配置jdk环境变量，打开/etc/profile配置文件，将下面配置拷贝进去。export命令用于将shell变量输出为环境变量

```
#set java environment
JAVA_HOME=/usr/local/jdk1.8.0_221
CLASSPATH=.:$JAVA_HOME/lib
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

 命令1：vim /etc/profile

 命令2：在文件末尾处，输入o(表示在光标下插入新行)，复制上面的环境变量配置粘贴，并写入保存



4. 重新加载/etc/profile配置文件

```
source /etc/profile
```



5. 判断JDK是否安装成功

![1570534333177](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1570534333177.png)     



## 编写Hello World程序运行

1. 使用vim编写Hello World

   ![1553592224391](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553592224391.png) 

2. 编译成字节码文件

   ![1553592236037](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553592236037.png)    

3. 执行Java程序

   ![1553592244275](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553592244275.png)    



##  JDK安装小结

1. 解压到压缩包到：/usr/local

2. 配置环境变量：

   JAVAHOME, CLASSPATH, PATH

3. 重新加载配置: 

   ```
   source /etc/profile
   ```

   



# 软件安装命令

## 目标

1.     rpm软件管理器命令

2.     yum软件安装和删除命令

## 命令：rpm

类似于一个软件管家，功能：可以离线安装一个rpm的文件

![1553592387972](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553592387972.png)                                                        

### 语法格式

| rpm 参数 软件包 | 参数说明                     |
| --------------- | ---------------------------- |
| 作用            | 可以离线安装一个rpm的文件    |
| -v              | 显示安装详情                 |
| -q <软件名>     | 查询已经安装的软件           |
| -a              | 显示所有已经安装的软件       |
| -h              | 显示安装过程中进度条，百分比 |
| -i <软件名>     | 安装软件 install             |
| -e <软件名>     | 删除软件 erase               |
| --nodeps        | 安装过程中不检查包的依赖     |
| --force         | 如果有相同包，强制覆盖       |

### 常用的组合

1. 查看所有安装的软件

   ```
   rpm -qa
   ```

   

2. 查看gcc-c++这个软件包是否安装

   ```
   rpm -q gcc-c++
   ```

   

3. 安装指定的软件

```
rpm -ivh 软件名
```

4. 删除指定的软件

```
rpm -ev 软件名
```



###  执行结果

   ![1553599088279](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553599088279.png) 



## 命令：yum 

### 功能

​	yum（全称为 Yellow dog Updater, Modified）能够从指定的服务器自动下载包并且安装，可以自动处理依赖关系，一次安装所有依赖的软件包。

### 语法格式

| yum [参数] [命令] <安装包> | 说明                          |
| -------------------------- | ----------------------------- |
| 参数：-y                   | 安装过程中所有的提问都回答yes |
| 命令：install              | 安装                          |
| 命令：remove               | 删除                          |

### 案例：在线安装c++的运行环境

在线安装gcc-c++，所有的提示都回答yes

```
yum -y install gcc-c++
```



## 小结

rpm 离线安装本地rpm文件

yum 在线安装，从服务器下载安装包和依赖包



# 安装mysql

1. 上传mysql-community-5.6.45.tar.gz到soft目录下

2. 创建目录mysql，将压缩包解压到mysql目录下

   ```
   mkdir mysql
   tar -xvf mysql-community-5.6.45.tar.gz -C mysql
   ```

   ![1569333443358](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1569333443358.png)

3. 安装所有的rpm文件，强制安装，不检测依赖

   ```
   rpm -ivh * --force --nodeps
   ```

![1569333537372](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1569333537372.png) 



4. 检查安装好的mysql包

   ```
   rpm -qa |grep mysql
   ```

 ![1553599927115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553599927115.png)  



## 启动MySQL服务并登录

1)  启动mysql的服务

```
systemctl start mysqld
```

2)  将mysql加到系统服务中并设置开机启动

```
systemctl enable mysqld
```

3)  登录mysql，root用户默认没有密码

```
mysql -uroot
```

![1553599970843](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553599970843.png) 

4) 在mysql中修改自己的密码

```
set password = password('密码'); 
```

 ![1553599991396](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553599991396.png)  



## 设置远程访问权限、开放端口号

1)  开启mysql的远程登录权限，默认情况下mysql为安全起见，不支持远程登录mysql，所以需要设置开启，并且刷新权限缓存。远程登录mysql的权限登录mysql后输入如下命令：

```mysql
-- 给root指定所有的权限，在任何电脑上可以远程登录
grant all privileges on *.* to 'root'@'%' identified by 'root';
-- 从mysql数据库中的授权表重新载入权限
flush privileges;
```

2)  开放Linux的对外访问的端口3306

```
#开放的端口永久保存到防火墙
firewall-cmd --zone=public --add-port=3306/tcp --permanent

#重启防火墙
systemctl restart firewalld
```



## 客户端Windows连接MySQL

1)  在本地Windows系统使用SQLyogEnt.exe软件连接虚拟机中的Linux系统安装的mysql

![1553600078271](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600078271.png)   

 2)  在客户端操作，创建数据库test

![1553600095629](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600095629.png)           

 3)  在Linux上可以看到新创建的数据库test

![1553600107838](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600107838.png)    



## 小结

1. 解压得到rpm安装包
2. rpm命令去安装所有rpm包
3. 启动mysql服务，并且设置为自动启动
4. 开放远程访问权限，开放3306端口号
5. 可以通过客户端Sqlyog去连接服务器



# 安装Redis

## 安装过程介绍

1. 安装编译环境并且编译 gcc-c++
2. 安装
3. 设置成后台运行，修改redis的配置文件
4. 打开防火墙6379端口号
5. 设置IP地址的bind



## 安装环境C语言的编译环境

redis是C语言开发，安装redis需要先将官网下载的源码进行编译，编译依赖gcc环境。如果没有gcc环境，需要安装gcc，在线安装的命令如下，但前面已经离线安装好了。

```
yum -y install gcc-c++
```

​	安装过程信息如下，大约需要下载39M。

![1554277476762](/assets/1554277476762.png)    



## redis安装

1. 上传"redis-3.2.11.tar.gz"到Linux系统/soft目录下

2. 进入soft目录,将"redis-3.2.11.tar.gz"解压到当前目录

```
cd /soft  
tar -xvf redis-3.2.11.tar.gz
```

3. 进入redis-3.2.11目录，使用make命令编译redis 

```
make
如下信息代表编译成功
```

![1554277495378](/assets/1554277495378.png)   

4. 在redis-3.2.11目录中，使用以下命令，将redis安装到/usr/local/redis指定的目录下

```
make PREFIX=/usr/local/redis install 
```

![1554277514726](/assets/1554277514726.png)    

​	安装成功后在/usr/local/redis/bin目录下可以看到如下结构，redis.conf文件还没有，要从刚解压的目录下复制过来。

 ![](/assets/1550475815230.png)  



# redis的配置

### 前端模式启动

​	直接运行bin/redis-server以前端模式启动，前端模式启动的缺点是启动完成后，不能再进行其他操作，如果要操作必须使用ctrl+c，同时redis-server程序结束，不推荐使用此方法。

```
./redis-server
```

 ![](/assets/1545820084354.png)  

使用CTRL+ C 停止前端模式

 ![1554277598623](/assets/1554277598623.png)  

### 后端模式启动

1. 将之前解压出来的/soft/redis-3.2.11/redis.conf 配置文件复制到安装好的/usr/local/redis/bin/目录下

![1554277608467](/assets/1554277608467.png)    

 

2. 修改redis.conf配置文件，修改daemonize yes 以后端模式启动。

```
vim redis.conf
```

![1554277619355](/assets/1554277619355.png)   

 

3. 启动时，指定配置文件

```
cd /usr/local/redis/bin
 ./redis-server redis.conf
```

 

4. 查看启动的后台进程

```
ps -aux | grep redis
```

![1554277637067](/assets/1554277637067.png)   

 

## 启动客户端

1. 进入redis/bin目录,启动"redis-cli"

```
./redis-cli
```

![1554277662649](/assets/1554277662649.png)    

 

2. 操作Redis，添加一个字符串的键，显示字符串的键，删除字符串的键

![1554277667875](/assets/1554277667875.png)    



## redis停止

在客户端向Redis发送shutdown命令

1. 方法1：在Linux命令行下输入 ./redis-cli shutdown

2. 方法2：在Redis客户端里面输入shutdown

![1554277680010](/assets/1554277680010.png)    



## 远程访问Redis

### 打开防火墙

如需远程连接redis，需配置redis端口6379在Linux防火墙中开放

```
#开放的端口永久保存到防火墙
firewall-cmd --zone=public --add-port=6379/tcp --permanent

#重启防火墙
systemctl restart firewalld
```

使用windows下的客户端软件连接，发现客户端依然无法连接redis，这是由于redis本身安全机制的限制，默认只允许本机访问

 ![1554277725357](/assets/1554277725357.png)    

 

### 修改配置允许其它机器访问

1. 默认redis只允许本机访问，如果想要其它机器访问，需要编辑redis.conf配置内容如下，添加当前Linux局域网的IP地址。

```
bind 127.0.0.1 192.168.248.99
```

 

2. 关闭redis，再重新启动Redis读取新的配置文件，才会起作用

```
./redis-cli shutdown
./redis-server redis.conf
```

 

3. 在Windows下使用Redis客户端访问Linux服务器上的Redis服务器

![1554277770929](/assets/1554277770929.png)       

 

## 安装小结

1. 安装C语言编译环境
2. 解压得到C语言源代码
3. 编译并且安装到指定的目录下 make
4. 配置后台启动模式 daemonize yes
5. 防火墙开放6379端口
6. 修改redis.conf配置文件添加当前主机的IP地址



# Tomcat安装

## 目标

在Linux上安装Tomcat

## 安装Tomcat的步骤

1)  进入soft文件夹，解压Tomcat到/usr/local下

```
tar -xvf apache-tomcat-8.5.27.tar.gz  -C /usr/local/
```

2)  开放Linux的对外访问的端口8080

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
```

3)  重启防火墙

```
systemctl restart firewalld
```

4)  进入bin目录，启动Tomcat

```
./startup.sh
```

![1553600237063](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600237063.png) 

5) 在Windows下打开浏览器访问Linux的8080端口

![1553600267150](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600267150.png)           

 6)  进入bin目录下，关闭服务器。关闭服务器以后，浏览器不能再访问。

```
./shutdown.sh
```

![1553600283881](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600283881.png)   



## 小结

Tomcat的安装有哪几步？

1. 解压
2. 打开端口号8080



# 将项目发布到Linux

## 目标

1.     将项目发布到Linux

2.     MySQL底层编码的设置

​                                                    

## Window系统mysql到出数据库脚本文件

   ![1553600390295](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600390295.png)        

## Linux系统myql导入windows系统数据库导出的脚本文件

1) 连接Linux的MySQL，创建数据库day35，在数据库上右键，选择执行SQL语句，导入数据库文件

 ![1553600408760](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600408760.png)          



### 安装过程

将整个项目复制到Tomcat webapps目录下





# 修改mysql底层码表

### 出现问题：

上面部署的项目发现搜索功能无效,这是由于mysql软件底层码表使用的不是utf-8导致执行sql语句中文乱码,最终导致无法查询的数据。需要设置客户端和服务器端的编码为utf-8

### 解决方法：

1)  修改mysql的配置文件 

```
vim /etc/my.cnf
```

2) 在mysqld条目下增加以下配置，指定服务器的字符集为utf-8

```
[mysqld]
character-set-server=utf8
```

3)  增加客户端的默认字符集的配置，指定为utf-8，将下面的配置放到文件的结尾处

```
[client]
default-character-set=utf8
```

![1553600626609](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600626609.png) 

4)  文件保存退出后，重启mysql服务

```
systemctl restart mysqld
```

5)  重新搜索结果如下

![1553600635822](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553600635822.png)            

## 小结

1. 发布项目到tomcat有哪几步
2. 如何解决MySQL的编码问题




# 学习总结

1. 能够使用Linux中的网络管理命令

| 命令                               | 作用           |
| ---------------------------------- | -------------- |
| hostname                           | 显示主机名     |
| hostnamectl  set-hostname 新主机名 | 永久修改主机名 |

| netstat [参数] | 功能                         |
| -------------- | ---------------------------- |
| 无参           | 显示所有的网络访问进程       |
| -n             | number 显示IP和端口号        |
| -t             | tcp 显示TCP协议              |
| -l             | listener 显示监听中进程      |
| -p             | programs 显示进程的名字和PID |

2. 能够使用Linux中SSH免密登录命令

| 命令                    | 作用               |
| ----------------------- | ------------------ |
| ssh 服务器地址          | 登录               |
| ssh-keygen              | 生成公钥和私钥     |
| ssh-copy-id  服务器地址 | 将公钥复制到服务器 |

3. 能够使用Linux中基本用户管理命令

| 命令     | 作用                         |
| -------- | ---------------------------- |
| useradd  | 添加用户                     |
| passwd   | 修改密码                     |
| userdel  | 删除用户                     |
| usermod  | 修改用户登录名，修改用户的组 |
| groupadd | 添加组                       |
| gpasswd  | 将用户从组中删除             |
| groupmod | 修改组的名字                 |
| groupdel | 删除组                       |
| su       | 切换用户                     |
| sudo     | 执行管理员命令               |

4. 能够使用Linux中防火墙配置命令

   | firewall-cmd           | 参数                             |
   | ---------------------- | -------------------------------- |
   | --zone=public          | public: 外网<br />internal: 内网 |
   | --add-port=端口/tcp    | 添加端口                         |
   | --remove-port=端口/tcp | 删除端口                         |
   | --permanent            | 永久的                           |
   | --list-all             | 显示所有的端口                   |
   | --reload               | 重新加载规则                     |

5. 能够在Linux系统中安装jdk软件

6. 能够在Linux系统中安装mysql软件

7. 能够在Linux系统中安装tomcat软件

8. 能够在linux系统中安装redis软件

9. 能够在Linux系统的tomcat中部署发布项目