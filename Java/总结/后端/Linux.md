# 目录说明

| 常用目录 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| root     | 管理员登录以后进入的目录，普通用户不能访问                   |
| home     | 普通用户登录以后进入的目录，每个用户在这下面都有一个自己的目录 |
| bin      | 可执行命令的目录，今天学习的命令都在这个目录下               |
| etc      | 配置文件所在目录                                             |
| usr      | 软件安装目录，可以共享资源                                   |

# Linux下文件不同颜色表示的含义

| 颜色                             | 说明           |
| -------------------------------- | -------------- |
| 白色                             | 普通文件       |
| <font color="blue">深蓝色</font> | 目录           |
| <font color="red">红色</font>    | 压缩文件       |
| <font color="00b7ee">青色</font> | 链接(快捷方式) |
| <font color="orange">橙色</font> | 设备文件       |
| <font color='green'>绿色</font>  | 可执行文件     |

# 命令

## 与IP有关的命令

| 命令          | 功能说明                      |
| ------------- | ----------------------------- |
| ifconfig      | 显示Linux主机的IP地址         |
| ip addr       | 同上                          |
| ping 网络地址 | 检测2台计算机之间网络是否连通 |

![1553475956507](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553475956507.png)

## 目录切换命令cd

| cd 目录名 | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| 作用      | 用于切换当前工作目录，Change Directory                       |
| .         | 当前目录                                                     |
| ..        | 上一级目录                                                   |
| -         | 后退到上一次目录                                             |
| ~         | 回到主目录，管理员回到root目录，普通用户回到home/用户名。~可以省略 |

![1553478889913](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478889913.png)

## 创建目录mkdir

| mkdir 目录名 | 说明                    |
| ------------ | ----------------------- |
| 作用         | make directory 创建目录 |

![1553478932139](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553478932139.png)

## 查看当前目录内容ls

| 语法：ls [参数] | 功能说明                       |
| --------------- | ------------------------------ |
| 无              | 查看当前目录下内容             |
| -l              | 以详细的方式显示               |
| -a              | 显示所有的文件，包括隐藏的文件 |

![1553479027152](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479027152.png)

![1553479161559](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479161559.png)

## 搜索find

| find [目录名] [-name '查询字符串'] | 功能                                       |
| ---------------------------------- | ------------------------------------------ |
| 无参名                             | 查找当前目录下所有的文件和目录，包括子目录 |
| 目录名                             | 查找指定目录下文件和子目录                 |
| -name '查询字符串'                 | 查询指定的字符串，可以使用通配符           |
|                                    | * 匹配多个字符                             |
|                                    | ? 匹配一个字符                             |

![1553479660634](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479660634.png)

![1553479665494](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479665494.png)

![1553479669654](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479669654.png)

## 重命名mv

| mv  旧名  新名 | 说明                    |
| -------------- | ----------------------- |
| 作用           | move 修改目录名或文件名 |

![1553479737786](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479737786.png)

![1553479742229](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479742229.png)

## 移动mv

| mv  源目录  目标目录 | 说明                           |
| -------------------- | ------------------------------ |
| 作用                 | 将目录或文件移动到另一个目录下 |

![1553479819145](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553479819145.png)

==注意==:目标目录如果不存在就是改名，如果存在就是移动

## 复制命令cp

| 语法 | cp [参数] 源文件或目录 目标目录    |
| ---- | ---------------------------------- |
| 作用 | copy 复制文件或目录                |
| -r   | recursion 递归，连同子目录一起复制 |

![image-20211009154942674](C:\Users\guoshutao\AppData\Roaming\Typora\typora-user-images\image-20211009154942674.png)

![1553486783149](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553486783149.png)

## 删除文件或目录rm

==注==：按tab会自动补全，按2下tab可以出现所有匹配的文件名

| 语法：rm  [参数]  文件或目录1 文件或目录2 | 功能                     |
| ----------------------------------------- | ------------------------ |
| 作用                                      | remove 删除文件或目录    |
| -r                                        | 连同子目录一起删除       |
| -f                                        | 删除前没有确认，强制删除 |

![1553486859853](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553486859853.png)

## 查看文件

| 查看文件的内容的命令： | cat/more/head/tail/less                                      |
| ---------------------- | ------------------------------------------------------------ |
| cat 文件名             | 显示文本文件所有的内容                                       |
| more 文件名            | 分屏显示，显示一屏就暂停<br />回车：每按一次，多显示一行<br />空格：每按一次，多显示一屏<br />q: 退出 |
| head 文件名            | 显示前10行                                                   |
| head -n 行数   文件名  | 显示前面的n行                                                |
| tail 文件名            | 显示后面10行                                                 |
| tail -n 行数   文件名  | 显示后面的n行                                                |
| less 文件名            | 翻页显示，向前向后翻页<br />-N: 显示行数<br />PageUP:  向前翻页<br />PageDown: 向后翻页 |

![1553487483218](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553487483218.png)

## 创建文件touch

| touch 文件1 文件2 | 说明                          |
| ----------------- | ----------------------------- |
| 作用              | 创建一个空文件，大小是0个字节 |

![1553487546243](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553487546243.png)

## vim编辑文件

vi(vim)是上Linux常用的编辑器，很多Linux发行版都默认安装了vi(vim)。vi是“Visual Interface”的缩写，vim是 (增强版的vi)。在一般的系统管理维护中vi就够用，如果想使用代码加亮的话可以使用vim。

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

## 文件的压缩

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

![1553489014878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553489014878.png)

![1553489020319](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553489020319.png)

## 查看当前绝对路径pwd

| pwd                                                          |
| ------------------------------------------------------------ |
| Print Working Directory 打印工作目录，显示当前是在哪个目录下 |

![1553490590589](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490590589.png)

## 查看进程：ps

### Linux进程启动的两种方式

1. 操作系统启动的时候自动启动的进程
2. 由用户在终端上(命令行中)输入的进程

### bash进程

1. 每个用户登录以后都会分配一个终端操作的进程
2. 这个进程是所有终端命令的父进程bash，不要随意终止这个进程。

### ![1553588683900](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553588683900.png) 

![1553490699455](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490699455.png)

![1553490716439](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490716439.png)

![1553490743216](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490743216.png)

## 杀死进程：kill

| kill  [参数]  进程号 | 功能：杀死进程                                          |
| -------------------- | ------------------------------------------------------- |
| 进程号               | 通过进程号来杀死进程                                    |
| -9                   | 如果直接输入进程号不能杀死进程，使用9的参数，强制进行。 |

![1553490882105](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490882105.png)

![1553490892878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490892878.png)

![1553490909822](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490909822.png)

![1553490942799](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553490942799.png)

### 显示执行中的程序：top

| top命令作用 | 显示内存占用情况，每过1秒中刷新一次 |
| ----------- | ----------------------------------- |
| 按q         | 可以退出                            |

![1563702582166](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563702582166.png)

## 搜索文件内容grep

grep是一种强大的文本搜索工具，它能使用字符串搜索文本，并把匹配的行和行号打印出来。

- find命令：查找文件或目录
- grep命令：从文件中搜索指定的内容

| grep  [参数] 字符串 文件名 | 参数说明                           |
| -------------------------- | ---------------------------------- |
| 作用                       | 从指定的文件中搜索字符串所在的位置 |
| -n                         | 显示行号                           |
| -v                         | 显示不匹配行                       |
| -i                         | 区分大小写查找                     |

![1553491140593](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491140593.png)

![1553491160325](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491160325.png)

### 管道 |

| 语法 | 命令1 \| 命令2                                               |
| ---- | ------------------------------------------------------------ |
| 解释 | 管道至少有2条以上的命令，将1条命令的运行结果做为另1条命令的执行条件 |

![1553491281710](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491281710.png)

### 查看内存

| 语法 | free -m            |
| ---- | ------------------ |
| 解释 | 显示内存的使用情况 |
| -m   | 以M为单位显示      |

![1563703317558](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703317558.png)

![1563703366041](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703366041.png)

### 查看硬盘df

| 语法    | df                           |
| ------- | ---------------------------- |
| 解释    | disk free 显示硬盘的使用情况 |
| -h      | human 更符合人类的阅读习惯   |
| --total | 在最后显示总数               |

![1563703525205](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1563703525205.png)

### 关机

| 语法 | shutdown now |
| ---- | ------------ |
| 解释 | 关机         |

### 重启

| 语法 | reboot |
| ---- | ------ |
| 解释 | 重启   |

## 文件权限的操作

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

![1553491810659](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491810659.png) 

![1553491851031](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491851031.png)

![1553491863031](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491863031.png)

![1553491874529](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491874529.png)

### 修改权限

![1553491902313](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491902313.png) 

![1553491951166](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491951166.png)

### 删除权限

![1553491979181](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553491979181.png) 

![1553492014975](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553492014975.png)

## crontab任务

### crontab命令参数

- 作用：制定定时任务，每过一段时间执行一次指定的任务。(JavaScript：setInterval())

| crontab [参数] | 参数说明             |
| -------------- | -------------------- |
| -l             | 显示已经有的定时任务 |
| -e             | 进入编辑状态         |
| -r             | 删除定时任务         |

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

### 定时服务的管理

crontab如果安装到Linux系统上，默认是开启服务的，会消耗一定的资源。类似于Windows下的服务：

![1553493038354](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493038354.png) 

### 定时服务管理的命令

| crond服务管理命令         | 说明                         |
| ------------------------- | ---------------------------- |
| systemctl   start crond   | 开启定时服务                 |
| systemctl  stop   crond   | 停止定时服务                 |
| systemctl   status crond  | 显示定时服务状态             |
| systemctl restart   crond | 重启定时服务，先关闭，再开启 |
| systemctl   reload crond  | 重启加载定时服务的任务       |

![1553493097878](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493097878.png)

### 设置后台服务的自启动配置

类似于Windows下服务的自启动

![1553493113342](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493113342.png) 

| 命令                              | 说明                   |
| --------------------------------- | ---------------------- |
| systemctl   list-units \*.service | 显示所有正在运行中服务 |
| systemctl   enable 服务名         | 将服务设置为开机自启动 |
| systemctl   disable 服务名        | 关闭服务的开机自启动   |

![1553493187975](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553493187975.png)

## 网络服务的管理

| 命令                      | 说明             |
| ------------------------- | ---------------- |
| systemctl start network   | 开启网络服务     |
| systemctl stop network    | 停止网络服务     |
| systemctl restart network | 重启网络服务     |
| systemctl status network  | 显示网络服务状态 |

![1553589307554](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589307554.png)

![1553589315904](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589315904.png)

![1553589320877](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589320877.png)

## 网卡的激活与关闭

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

![1553589574901](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589574901.png)   

## 配置静态IP

设置ip分为2种类型，dhcp和static。dhcp是动态获取ip，static是配置静态ip。dhcp动态获取ip可能ip经常会发生变化,导致客户端无法连接到。静态ip配置后就不会发生改变，这样客户端连接服务器具有更好的安全性。

### 操作步骤

1. 查看当前虚拟机网关（记住这个网关，后面使用）      ![1553589780321](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589780321.png)                                                     

2. 进入目录命令：cd /etc/sysconfig/network-scripts/

   ![1553589800921](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux基础/1553589800921.png) 

3. 编辑网卡配置文件命令：vim ifcfg-ens33

   ```properties
   BOOTPROTO=static
   IPADDR=192.168.248.99
   GATEWAY=192.168.248.2
   NETMASK=255.255.255.0
   DNS1=8.8.8.8
   DNS2=114.114.114.114
   ```

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

## 主机名配置

​	需求：默认主机名是localhost，我们希望改为有含义的主机名。如：heima，itcast等主机名。类似于Windows中修改主机名：  ![1553589164649](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553589164649.png)                                          

| 修改主机名                          | 说明                         |
| ----------------------------------- | ---------------------------- |
| hostname                            | 显示当前的主机名             |
| hostname 新主机名                   | 临时修改主机名，重启以后失效 |
| hostnamectl   set-hostname 新主机名 | 永久修改主机名               |

![1553589230441](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553589230441.png)

## 域名映射

我们希望通过域名来访问互联网的主机，而不是输入又长又难记的IP地址。

![1553591048253](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591048253.png)                                                         

### 域名访问网站的过程

![1573177418290](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1573177418290.png)

Windows下本机的hosts文件是 c:\windows\System32\drivers\etc\hosts，这是一个文本文件。

## 修改Linux域名映射文件

在Linux中，/etc/hosts文件用于在主机名与IP地址之间进行映射。所以你想访问一个什么样的主机名，就需要把这个主机名和它对应的IP地址配置在/etc/hosts文件中。

### 操作步骤

1. 首先ping newboy看能否访问这个地址
2. 修改hosts文件命令：vim /etc/hosts
3. 将本机局域网的ip地址与newboy这个名字进行映射。一个ip地址后面可以映射多个域名地址。
4. 再次使用ping newboy，发现可以访问这个名字。实现域名与ip地址的映射。 

![1553591104227](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591104227.png) 

## 查看网络进程

### 语法格式

| netstat [参数] | 参数说明 | 功能                        |
| -------------- | -------- | --------------------------- |
| 无参           |          | 正在访问网络的所有的进程    |
| -n             | number   | 同时显示IP地址和端口号      |
| -t             | tcp      | 只显示使用TCP协议的网络进程 |
| -l             | listener | 显示正在监听中的进程        |
| -p             | programs | 同时显示程序的名字和PID     |

![1553591523827](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591523827.png)

![1553591539168](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591539168.png)

## 防火墙配置

| 命令                          | 作用             |
| ----------------------------- | ---------------- |
| systemctl start   firewalld   | 开启防火墙       |
| systemctl stop   firewalld    | 关闭防火墙       |
| systemctl   enable firewalld  | 开机自启动防火墙 |
| systemctl   disable firewalld | 关闭开机自启动   |
| systemctl   status firewalld  | 显示防火墙状态   |

![1553591585545](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553591585545.png)

## firewall-cmd

 外网或内网需要连接到当前系统内的程序进行操作，需要linux系统开放程序端口，否则无法访问。

| firewall-cmd           | 打开或关闭防火墙中指定的端口号，让端口号可以访问             |
| ---------------------- | ------------------------------------------------------------ |
| --zone                 | public: 开放的是外网，所有外部客户端都可以访问，默认值<br />internal: 开放的是内网，局域网 |
| --add-port=端口/tcp    | 打开指定的端口                                               |
| --remove-port=端口/tcp | 关闭指定的端口                                               |
| --permanent            | 永久的打开或关闭                                             |
| --list-all             | 显示已经打开的端口号                                         |
| --reload               | 重启加载防火墙的规则，让新的规则起作用                       |

![1563798960456](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563798960456.png)

![1563799088361](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1563799088361.png)

# SSH口令方式登录

## 为什么要登录

需要在同一个客户端，或不同的Linux之间相互访问。需要在一台Linux上登录另一台Linux.

Linux之间登录使用SSH协议，Secure Shell Protocol 安全壳协议，是一种在Linux中传输数据的加密协议。

## SSH工作机制

SSH登录有两种验证机制：

1)    输入用户名和密码的登录方式  

2)    免密码登录

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

## 添加用户

| useradd [参数] 用户名 | 创建一个普通用户(adduser是它的别名)                          |
| --------------------- | ------------------------------------------------------------ |
| -m                    | 创建用户的同时，在home下创建它的主目录                       |
| -g <组名>             | 指定用户在哪个组中，如果未指定组名：默认创建一个与用户名相同的组名 |
| 说明                  | 创建好的用户信息在/etc/passwd文件中                          |

![1553674003887](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674003887.png)

/etc/passwd文件内容

![1553674029790](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674029790.png)

## 设置密码

| passwd [用户名] | 功能                     |
| --------------- | ------------------------ |
| 无参            | 修改自己的密码           |
| 用户名          | 管理员可以修改其他的密码 |

![1553674118909](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674118909.png)

## 切换用户

| su 用户账号 | 功能                                                         |
| ----------- | ------------------------------------------------------------ |
| 作用        | 切换到指定的用户<br />管理员->普通用户不用输入密码<br />普通用户->管理员必须要输入密码 |

![1553674186237](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674186237.png)

## 删除用户

| userdel [-r] [用户帐号] | 删除指定的用户       |
| ----------------------- | -------------------- |
| -r                      | 同时删除用户的主目录 |

![1553674298200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674298200.png)

## 修改登录名

| usermod -l 新登录名 原登录名 | 功能             |
| ---------------------------- | ---------------- |
| 作用                         | 修改用户的信息   |
| -l <新登录名>                | 修改用户的登录名 |

![1553674380096](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674380096.png)

## 添加组

| groupadd 组名 | 说明                                   |
| ------------- | -------------------------------------- |
| 作用          | 创建一个组                             |
| 说明          | /etc/group文件，包含了所有创建组的信息 |

![1553674497860](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674497860.png)

## 修改用户所在组

注：一个用户只能有一个主组，可以有多个从组。

| usermod 参数 组名 用户名 | 修改用户所在的组               |
| ------------------------ | ------------------------------ |
| -g<组名>                 | 修改用户的主组                 |
| -G<组1,组2>              | 修改用户的从组，可以有多个从组 |

![1553674569689](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674569689.png)

![1553674576382](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674576382.png)

## 将用户从组中删除

注：用户无法从主组中删除，只可以修改主组，可以从从组中删除用户。

| gpasswd -d 用户名 组名 | 功能                     |
| ---------------------- | ------------------------ |
| -d<用户名>             | 将用户从指定的从组中删除 |

![1553674644560](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674644560.png)

![1553674649432](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674649432.png)

## 修改组的名字

| groupmod -n 新组名 原组名 | 功能         |
| ------------------------- | ------------ |
| -n <新组名>               | 修改组的名字 |

![1553674741115](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674741115.png)

## 删除组

删除的组的前提是：没有用户把它做为主组，如果要删除主组，则必须先删除这些用户后或将用户移动到其他主组中，才能删除主组。

| groupdel 组名 | 功能   |
| ------------- | ------ |
| 作用          | 删除组 |

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

### ![1553674912481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674912481.png)

### ![1553674930825](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1553674930825.png)    

# JDK安装

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

![1554277476762](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277476762.png)    



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

![1554277495378](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277495378.png)   

4. 在redis-3.2.11目录中，使用以下命令，将redis安装到/usr/local/redis指定的目录下

```
make PREFIX=/usr/local/redis install 
```

![1554277514726](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277514726.png)    

​	安装成功后在/usr/local/redis/bin目录下可以看到如下结构，redis.conf文件还没有，要从刚解压的目录下复制过来。

 ![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1550475815230.png)  



# redis的配置

### 前端模式启动

​	直接运行bin/redis-server以前端模式启动，前端模式启动的缺点是启动完成后，不能再进行其他操作，如果要操作必须使用ctrl+c，同时redis-server程序结束，不推荐使用此方法。

```
./redis-server
```

 ![](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1545820084354.png)  

使用CTRL+ C 停止前端模式

 ![1554277598623](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277598623.png)  

### 后端模式启动

1. 将之前解压出来的/soft/redis-3.2.11/redis.conf 配置文件复制到安装好的/usr/local/redis/bin/目录下

![1554277608467](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277608467.png)    

 

2. 修改redis.conf配置文件，修改daemonize yes 以后端模式启动。

```
vim redis.conf
```

![1554277619355](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277619355.png)   

 

3. 启动时，指定配置文件

```
cd /usr/local/redis/bin
 ./redis-server redis.conf
```

 

4. 查看启动的后台进程

```
ps -aux | grep redis
```

![1554277637067](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277637067.png)   

 

## 启动客户端

1. 进入redis/bin目录,启动"redis-cli"

```
./redis-cli
```

![1554277662649](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277662649.png)    

 

2. 操作Redis，添加一个字符串的键，显示字符串的键，删除字符串的键

![1554277667875](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277667875.png)    



## redis停止

在客户端向Redis发送shutdown命令

1. 方法1：在Linux命令行下输入 ./redis-cli shutdown

2. 方法2：在Redis客户端里面输入shutdown

![1554277680010](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277680010.png)    



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

 ![1554277725357](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277725357.png)    

 

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

![1554277770929](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Linux高级/1554277770929.png)       

 

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

# 将项目发布到Linux

## 目标

1.     将项目发布到Linux

2.     MySQL底层编码的设置                                                 

## Windows系统mysql到出数据库脚本文件

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
