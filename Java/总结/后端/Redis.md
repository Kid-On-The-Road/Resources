# 概念

Not Only SQL，不仅仅是SQL，指的是非关系型数据库。不能代替关系型数据库，只是关系型数据库有益的补充。

# 分类

| 类别                      | 解释                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 键值(Key-Value)存储数据库 | 这一类数据库主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据。今天学习的Redis就是这种存储方式。 |
| 列存储数据库              | 通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。如：Cassandra, HBase, Riak. |
| 文档型数据库              | 文档型数据库可以看作是键值数据库的升级版，允许嵌套键值对。而且文档型数据库比键值数据库的查询效率更高。如：CouchDB, MongoDb。 |
| 图形(Graph)数据库         | 图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上。 |

# 关系型与非关系型数据库比较

| 数据库名称     | 解释                                                         | 优点                                                         | 缺点                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 关系型数据库   | 关系型数据库最典型的数据结构是表，由二维表及其之间的联系所组成的一个数据组织 | 1.易于维护：都是使用表结构，格式一致； <br />2.使用方便：SQL语言通用，可用于复杂查询； <br />3.复杂操作：支持SQL，可用于一个表以及多个表之间非常复杂的查询。 | 1.读写性能比较差，尤其是海量数据的高效率读写； <br />2.固定的表结构，灵活度不佳； <br />3.高并发读写需求，传统关系型数据库来说，硬盘I/O是一个很大的瓶颈。 |
| 非关系型数据库 | 非关系型数据库严格上不是一种数据库，应该是一种数据结构化存储方法的集合，可以是文档或者键值对等。 | 1.格式灵活：存储数据的格式可以是key,value形式、文档形式、图片形式等等，文档形式、图片形式等等，使用灵活，应用场景广泛，而关系型数据库则只支持基础类型。 <br />2.速度快：nosql可以使用硬盘或者随机存储器RAM作为载体，而关系型数据库只能使用硬盘； <br />3.成本低：nosql数据库部署简单，基本都是开源软件。 | 1.不提供sql支持，学习和使用成本较高； <br />2.无事务处理； <br />3.数据结构相对复杂，复杂查询方面不方便。 |

# NOSQL解决的问题

| 问题                                                         | 解释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| High Performance - 数据库高并发访问                          | 在同一个时间点，同时有海量的用户并发访问。往往要达到每秒上万次读写请求。关系数据库应付上万次SQL查询还勉强顶得住，但是应付上万次SQL写数据请求，硬盘IO就已经无法承受了。<br />如天猫的双11，从凌晨0点到2点这段时间，每秒达到上千万次的访问量。<br />12306春运期间，过年回家买火车抢票的时间，用户不断查询有没有剩余票。 |
| Huge Storage - 海量数据的存储                                | 数据库中数据量特别大，数据库表中每天产生海量的数据。<br />类似QQ，微信，微博，每天用户产生海量的用户动态，每天产生几千万条记录。对于关系数据库来说，在一张几亿条记录的表里面进行SQL查询，效率是极其低下乃至不可忍受的。 |
| High Scalability && High Availability- 高可扩展性和高可用性的需求 | 关系型数据库进行扩展和升级是比较麻烦的一样事，对于很多需要提供24小时不间断服务的网站来说，对数据库系统进行升级和扩展是非常痛苦的事情，往往需要停机维护和数据迁移。<br />非关系型数据库可以通过不断的添加服务器节点来实现扩展，而不需对原有的数据库进行维护。 |

# Redis的目录文件 

| 目录或文件           | 作用                |
| -------------------- | ------------------- |
| redis-benchmark.exe  | redis性能的检测工具 |
| redis-check-aof.exe  | AOF的检查和修复工具 |
| redis-check-dump.exe | RDB的检查和修复工具 |
| redis-cli.exe        | 客户端工具          |
| redis-server.exe     | 服务器端            |
| redis.window.conf    | 配置文件            |

# Redis的5种数据类型

redis是一种高级的key-value的存储系统，其中value支持五种数据类型。

| 值的数据类型 | 说明                                       |
| ------------ | ------------------------------------------ |
| string       | 字符串类型的值                             |
| hash         | 值是：键和值类型                           |
| list         | 值是列表类型：元素有序，可以重复           |
| set          | 集合类型：元素无序，不可重复               |
| zset         | 可以排序的集合类型：存放是无序的，不可重复 |

## string类型

Redis不是用Java编写的，用C语言编写的。在Redis中以二进制保存，没有字符的编码和解码的过程。无论存入的是字符串、整数、浮点类型都会以字符串写入。

在Redis中字符串类型的值最多可以容纳的数据长度是512M，这是以后最常用的数据类型。

![1553421626543](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553421626543.png) 

### 常用命令

| 命令        | 功能                                                |
| ----------- | --------------------------------------------------- |
| set 键 值   | 向redis中添加字符串类型的键和值，如果键存在就会覆盖 |
| setnx 键 值 | 如果这个键不存在就添加，存在就不添加                |
| get 键      | 通过键得到值                                        |
| del 键      | 通过键删除键和值                                    |

### 命令演示

1. 添加一个键为company，值为itcast
2. 再设置一个键为company，值为heima
3. 得到company的元素
4. 使用setnx设置company的值为：sun，查看结果
5. 使用setnx设置address的值为：guangzhou，查看结果
6. 删除company元素
7. 再次删除company看返回值是否相同
8. 得到company看返回值是多少

![1573006758748](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573006758748.png)

## hash类型

Redis中的Hash类型可以看成是键和值都是String类型的Map容器，每一个Hash可以存储4G个键值对。

![1553421960003](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553421960003.png) 

​	该类型非常适合于存储对象的信息。如一个用户有姓名，密码，年龄等信息，则可以有username、password和age等键。它的存储结构如下：

![1553421986390](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553421986390.png) 

### 常用命令

| 命令                     | 功能                       |
| ------------------------ | -------------------------- |
| hset 键 字段 值          | 添加1个字段和值            |
| hget 键 字段             | 通过键，字段得到值         |
| hmset 键 字段 值 字段 值 | 一次设置多个字段和值       |
| hmget 键 字段 字段       | 通过键和多个字段得到多个值 |
| hdel 键 字段 字段        | 删除多个字段和值           |
| hgetall 键               | 通过键得到它所有的字段和值 |

### 命令演示

1. 创建hash类型的键为user，并且添加一个字段为username，值为newboy
2. 向user中添加字段为age，值为18
3. 分别得到user中的username和age的字段值
4. 向user中同时添加多个字段和值，birthday 2018-01-01 sex male
5. 同时取得多个字段：age 和 sex
6. 得到user中所有的字段和值
7. 删除user中的生日和性别字段

![1573007305236](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573007305236.png)

![1573007320654](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573007320654.png)

## list类型

在Redis中，List类型是按照插入顺序排序的字符串链表。和数据结构中的普通链表一样，我们可以在其左部(left)和右部(right)添加新的元素。

在插入时，如果该键并不存在，Redis将为该键创建一个新的链表。

如果链表中所有的元素均被移除，那么该键也将会被从数据库中删除。

List中可以包含的最大元素数量是4G个。

![1553422277890](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553422277890.png) 

### 常用命令

| 命令                | 行为                                                         |
| ------------------- | ------------------------------------------------------------ |
| lpush 键 元素 元素  | 从左边添加1个或多个元素                                      |
| rpush 键 元素 元素  | 从右边添加1个或多个元素                                      |
| lpop 键             | 从左边删除1个元素，返回被删除的元素                          |
| rpop 键             | 从右边删除1个元素，返回被删除的元素                          |
| lrange 键 开始 结束 | 查询指定范围内的多个元素，开始和结束是它的索引号。<br />从左到右索引号是0到长度-1，从右到左从-1开始编号。<br />如果到得到列表中所有的元素：0~-1 |
| llen 键             | 得到元素的长度                                               |



### 命令演示

![1553422354555](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553422354555.png) 

1. 向mylist键的列表中，从左边添加a b c三个元素
2. 从右边添加one two three三个元素
3. 查询所有的元素
4. 从右边添加一个重复的元素three
5. 删除最右边的元素three
6. 删除最左边的元素c
7. 获取列表中元素的个数

![1573007952018](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573007952018.png)

![1573007978162](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573007978162.png)

## set类型

在Redis中，我们可以将Set类型看作为没有排序的字符集合，Set可包含的最大元素数量是4G，和List类型不同的是，Set集合中不允许出现重复的元素。

![1553422461249](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553422461249.png)

### 常用命令

| 命令              | 行为                      |
| ----------------- | ------------------------- |
| sadd 键 元素 元素 | 向集合中添加1个或多个元素 |
| smembers 键       | 获取指定键中所有的元素    |
| sismember 键 元素 | 判断键中某个元素是否存在  |
| srem 键 元素 元素 | 删除指定键中多个元素      |

### 命令演示

1. 向myset集合中添加A B C 1 2 3 六个元素
2. 再向myset中添加B元素，看能否添加成功
3. 显示所有的成员，发现与添加的元素顺序不同，元素是无序的
4. 删除其中的C这个元素，再查看结果
5. 判断A是否在myset集合中
6. 判断D是否在myset集合中

![1573008330850](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573008330850.png)

![1573008343656](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573008343656.png)

## zset类型

Redis 有序集合和集合一样也是无序不可以重复。不同的是每个元素都会关联一个分数。

redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复，每个集合可存储40多亿个成员。

![1563279583231](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563279583231.png)

### 常用命令

| 命令                        | 行为                                       |
| --------------------------- | ------------------------------------------ |
| zadd 键 分数 值 分数 值     | 向键中添加1个或多个元素(分数和值)          |
| zrange 键 开始索引 结束索引 | 查询指定范围内的元素，如果查询所有的：0~-1 |
| zrem 键 值 值               | 删除一个或多个键和值                       |
| zcard 键                    | 得到一共有多少个元素                       |
| zrank 键 值                 | 得到元素的索引号                           |
| zscore 键 值                | 得到元素的分数                             |

### 命令演示

1. 添加键country，分数是10，值是Japan
2. 添加键country，分数是5，值是USA，添加键country，分数是50，值是Russian
3. 添加键country，分数是1，值是China，分数是120，值是Korea
4. 查询country中所有的元素
5. 查询Japan的索引号(从0开始)
6. 删除值为USA的元素
7. 查询country中还有多少个元素
8. 显示Russian的分数值

![1573008792438](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573008792438.png)

# Redis的通用命令

| 命令               | 功能                                                |
| ------------------ | --------------------------------------------------- |
| keys 匹配字符      | 查询所有的键<br />* 匹配多个字符<br />? 匹配1个字符 |
| del 键1 键2        | 删除任何类型的键                                    |
| exists 键          | 判断指定的键是否存在，如果存在返回1，不存在就返回0  |
| type 键            | 判断指定的键类型，如果键不存在就返回none            |
| select 数据库编号  | 选择操作哪个数据库                                  |
| move 键 数据库编号 | 将指定的键移动到另一个数据库中                      |

### 命令演示

1. 显示所有的键
2. 显示所有以my开头的键
3. 显示所有my后面有三个字符的键
4. 添加字符串man的值为Jack
5. 从左边添加list的键是woman，值是：Rose Mary Kate
6. 添加一个字符串：person NewBoy
7. 显示所有的键
8. 一次删除woman和man这两个键
9. 分别判断person和woman是否存在
10. 分别判断person user myset mylist分别是什么类型
11. 切换数据库到15，向15中添加一个字符串的：student Lina
12. 将15中的student移到0号数据库中
13. 切换到数据库0，显示所有的键

![1573011139541](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573011139541.png) 

![1573011347726](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573011347726.png) 

# Redis的持久化

将内存中的数据写到硬盘文件中，将数据永久的保存下来，称为持久化。

| 方式 | 解释                                                      |
| ---- | --------------------------------------------------------- |
| RDB  | Redis DataBase 默认的存储方式，二进制的格式。             |
| AOF  | Append Only File 以日志追加的方式保存数据，文本文件格式。 |

## RDB存储方式

### RDB持久化机制优点

| 优点           | 解释                                                         |
| -------------- | ------------------------------------------------------------ |
| 方便备份与恢复 | 整个Redis数据库将只包含一个文件，默认是dump.rdb，这对于文件备份和恢复而言是非常完美的。因为我们可以非常轻松的将一个单独的文件压缩后再转移到其它存储介质上。一旦系统出现灾难性故障，我们可以非常容易的进行恢复。 |
| 性能最大化     | 对于Redis的服务进程而言，在开始持久化时，它唯一需要做的只是分叉出子进程，由子进程完成这些持久化的工作，这样就可以极大的避免服务进程执行IO操作了。 |
| 启动效率更高   | 相比于AOF机制，如果数据集很大，RDB的启动效率会更高           |

### RDB持久化机制缺点

| 缺点                   | 解释                                                         |
| ---------------------- | ------------------------------------------------------------ |
| 不能完全避免数据丢失   | 因为RDB是每隔一段时间写入数据，所以系统一旦在定时持久化之前出现宕机现象，此前没有来得及写入磁盘的数据都将丢失。 |
| 会导致服务器暂停的现象 | 由于RDB是通过子进程来协助完成数据持久化工作的，因此当数据集较大时，可能会导致整个服务器停止服务几百毫秒，甚至是1秒钟。 |

### RDB持久化机制的配置

在redis.windows.conf配置文件中的SNAPSHOTTING中有如下说明：

| 语法                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| save <时间间隔> <修改键数> | 在指定的时间间隔，修改了指定的键数<br />同时满足2个条件就写到硬盘中<br />修改：增删改 |

 	如下面配置的是RDB方式数据持久化时机，必须两个条件都满足的情况下才进行持久化的操作：

| 关键字 | 时间(秒) | 修改键数 | 解释               |
| ------ | -------- | -------- | ------------------ |
| save   | 900      | 1        | 15分钟修改了1个键  |
| save   | 300      | 10       | 5分钟修改了10个键  |
| save   | 60       | 10000    | 1分钟修改了1万个键 |

### 演示：RDB持久化

1. 修改redis.windows.conf 文件的101行
   添加1行：save 20 3 (表示20秒内修改3个键，则写入到dump.rdb文件中)

2. 使用指定的配置文件启动服务器：redis-server redis.windows.conf 

3. 向数据库中添加2个键，直接关闭服务器窗口。再开启服务器，查看所有的keys，刚才添加的数据丢失。

   ![1553423429159](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553423429159.png) 

4. 在客户端添加3个键，发现服务器端有如下输出信息，表示写入到数据库dump.rdb文件中

   ![1553423438325](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553423438325.png) 

5. 直接关闭服务器窗口，再开启服务器，查看所有的keys，数据没有丢失。

   ![1553423445306](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553423445306.png) 

## AOF的存储方式

### OF持久化机制优点

​	AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作。也可以通过该文件完成数据的重建。该机制可以带来更高的数据安全性，所有的操作都是异步完成的。

###  AOF持久化机制缺点

1. 文件比RDB更大：对于相同数量的数据集而言，AOF文件通常要大于RDB文件。

2. 运行效率比RDB更慢：根据同步策略的不同，AOF在运行效率上往往会慢于RDB。

### AOF持久化机制配置

- 开启AOF持久化

AOF默认是关闭的，首先需要开启AOF模式

| 参数配置            | 说明                         |
| ------------------- | ---------------------------- |
| appendonly   no/yes | AOF模式的开启(yes)和关闭(no) |

- AOF持久化时机

| 关键字      | 持久化时机 | 解释       |
| ----------- | ---------- | ---------- |
| appendfsync | everysec   | 每秒同步   |
| appendfsync | always     | 每修改同步 |
| appendfsync | no         | 不同步     |

### 演示：AOF的持久化

1. 打开AOF的配置，找到APPEND ONLY MODE配置块，392行。设置appendonly yes

2. 通过redis-server redis.windows.conf 启动服务器，在服务器目录下出现appendonly.aof文件。大小是0个字节。

3. 添加3个键和值

   ![1553423644984](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553423644984.png) 

4. 打开appendonly.aof文件，查看文件的变化。会发现文件记录了所有操作的过程。                                     

## AOF重写机制

将AOF文件重新写过,减小AOF文件大小，并且提升读取速度

### AOF 文件重写的实现原理

AOF重写并不需要对原有AOF文件进行任何的读取，写入，分析等操作，这个功能是通过读取服务器当前的数据库状态来实现的。

### AOF重写触发的方式

1. 手动触发：用户通过调用bgrewriteaof手动触发
2. 自动触发：如果全部满足的话，就触发自动的AOF重写操作：
   1. 没有RDB持久化/AOF持久化在执行，没有bgrewriteaof在进行；
   2. 当前AOF文件大小要大于redis.conf配置的auto-aof-rewrite-min-size大小；
   3. 当前AOF文件大小和最后一次重写后的大小之间的比率等于或者等于指定的增长百分比（在配置文件设置了auto-aof-rewrite-percentage参数，不设置默认为100%）

### 演示：AOF手动重写

1. 关闭服务器，删除生成的aof和rdb文件

   ![1563283369062](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283369062.png) 

2. 执行以下命令

   1. 从右边添加一个键为list，值为A B
   2. 从右边添加C
   3. 从右边添加D E
   4. 从左边弹出一个元素
   5. 从左边弹出一个元素
   6. 从右边添加元素F G
   7. 显示列表中的所有元素

   ![1563283403726](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283403726.png) 

3. 输入命令：bgrewriteaof，则aof被重写

   ![1563283417153](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283417153.png) 

4. 生成旧的文件

   ![1563283429470](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283429470.png) 

5. 服务器上出现提示

   ![1563283460687](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283460687.png) 


### 演示：AOF后台自动重写

1. 关闭服务器，删除生成的aof和rdb文件

2. 修改配置文件如下：

   ```
   # 大于原来的100%就自动重写
   auto-aof-rewrite-percentage 100
   # 自动重写的最小尺寸
   auto-aof-rewrite-min-size 100b
   ```

3. 带配置文件启动服务器： redis-server redis.windows.conf

4. 进行如下操作

   ![1563283612553](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1563283612553.png) 

5. 生成的重写文件

   ![1569797365576](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1569797365576.png)  

6. 服务器端的输出

   ![1569797398727](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1569797398727.png) 

# Jedis

Redis不仅可以使用命令来操作，现在基本上主流的语言都有API支持，比如Java、C#、C++、PHP、Node.js、Go等。在官方网站里列一些Java的客户端，有Jedis、Redisson、Jredis、JDBC-Redis等其中官方推荐使用Jedis和Redisson。 Jedis操作redis需要导入jar包如下：

![1553423812156](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553423812156.png) 

## Jedis的API

### jedis类常用方法

注：每个方法就是redis中的命令名，方法的参数就是命令的参数。每个Jedis对象似于JDBC中Connection对象。

| 连接和关闭              | 功能                                                     |
| ----------------------- | -------------------------------------------------------- |
| new Jedis(host,   port) | 创建一个连接对象<br />host: 服务器的IP<br />port: 端口号 |
| void close()            | 关闭连接，不然会导致后面无法连接                         |


| 对string操作的方法           | 说明                |
| ---------------------------- | ------------------- |
| set(String key,String value) | 添加字符串的键和值  |
| String get(String key)       | 通过键获取值        |
| del(String ... keys)         | 删除1个或多个键和值 |


| 对hash操作的方法                           | 说明                     |
| ------------------------------------------ | ------------------------ |
| hset(String key,String field,String value) | 添加hash类型键，字段，值 |
| Map<String,String> hgetall(String key)     | 通过键得到所有的字段和值 |

| 对list操作的方法                                     | 说明                          |
| ---------------------------------------------------- | ----------------------------- |
| lpush(String key,String...values)                    | 添加到list类型中，1个或多个值 |
| List\<String> lrange(String key,long start,long end) | 查询指定范围内的元素          |

| 对set操作的方法                   | 说明                      |
| --------------------------------- | ------------------------- |
| sadd(String key,String...values)  | 向set中添加1个或多个元素  |
| Set\<String> smembers(String key) | 通过键获取set中所有的元素 |

### jedis的基本使用

```java
/**
1. 创建Jedis：new Jedis(host, port)
2. 添加字符串键：set
3. 添加list键：lpush
4. 得到字符串键：get
5. 得到list所有值：lrange
6. 关闭连接：close
*/
public class Demo1 {
    /**
     * 写入字符串和list类型，并且取出打印控制台上
     */
    public static void main(String[] args) {
        //1.创建Jedis连接对象
        Jedis jedis = new Jedis("localhost", 6379);
        //2. 调用方法写入字符串
        jedis.set("person", "张三");
        jedis.lpush("cities", "广州","上海","东莞");
        //3. 调用方法读取字符串并且打印
        String person = jedis.get("person");
        System.out.println("person: " + person);
        List<String> cities = jedis.lrange("cities", 0, -1);
        System.out.println("城市：" + cities);
        //4. 关闭连接
        jedis.close();
    }
}
```

- 控制台输出

![1553424037739](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553424037739.png) 

- 数据库中的效果

![1553424051234](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553424051234.png) 

## Jedis连接池

​	jedis连接资源的创建与销毁是很消耗程序性能，所以jedis为我们提供了jedis的连接池技术，jedis连接池在创建时初始化一些连接对象存储到连接池中，使用jedis连接资源时不需要自己创建jedis对象，而是从连接池中获取一个资源进行redis的操作。使用完毕后，不需要销毁该jedis连接资源，而是将该资源归还给连接池，供其他请求使用。

### Jedis连接池API

| JedisPoolConfig配置类   | 功能说明                               |
| ----------------------- | -------------------------------------- |
| JedisPoolConfig()       | 通过无参的构造方法创建一个配置对象     |
| void setMaxTotal()      | 设置最大的连接数                       |
| void setMaxWaitMillis() | 设置获取连接的最长等待时间，单位是毫秒 |

| JedisPool连接池类                   | 说明                       |
| ----------------------------------- | -------------------------- |
| JedisPool(配置对象,服务器名,端口号) | 通过构造方法创建连接池     |
| Jedis getResource()                 | 从连接池中得到一个连接对象 |

### JedisPool的基本使用

```java
public class Demo2 {
    public static void main(String[] args) {
        //1. 创建一个配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(10);  //最大连接数
        config.setMaxWaitMillis(2000);  //最长等待时间
        //2. 通过配置对象创建连接池
        JedisPool pool = new JedisPool(config, "localhost", 6379);
        //3. 从连接池中得到一个连接对象
        Jedis jedis = pool.getResource();
        //4. 使用连接对象
        //4.1 向redis添加中一个set集合
        jedis.sadd("students", "白骨精", "孙悟空", "猪八戒");
        //4.2 取出set集合输出
        Set<String> students = jedis.smembers("students");
        System.out.println(students);
        //5. 关闭连接
        jedis.close();
    }
}
```

- 运行效果

![1553426166646](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553426166646.png)                       

## ResourceBundle类

使用ResourceBundle类得到jedis.properties中的port属性

| java.util.ResourceBundle类                      | 功能                        |
| ----------------------------------------------- | --------------------------- |
| static ResourceBundle getBundle("配置文件基名") | 创建对象，读取src下属性文件 |
| String getString("键名")                        | 通过键读取指定的值          |

```java
/**
 * 读取配置文件
 */
public class Demo3 {
    public static void main(String[] args) throws IOException {
        /*
        //1.创建集合对象
        Properties properties = new Properties();
        //2. 在src目录下读取输入流
        InputStream in = Demo3.class.getResourceAsStream("/jedis.properties");
        //3. 数据读取到集合中
        properties.load(in);
        //4. 通过键得到值
        String port = properties.getProperty("port");
         */
        //专门用来读取src下属性配置文件，方法的参数是要读取的配置文件主文件名(基名)，不用写扩展名
        ResourceBundle bundle = ResourceBundle.getBundle("jedis");
        //通过这个对象的getString("键")方法读取指定的值
        String port = bundle.getString("port");
        System.out.println("端口号：" + port);
    }
}
```

## Jedis连接池工具类

1. ResourceBundle读取配置文件，得到所有的属性
2. 创建配置对象，参数从配置文件读取
3. 通过配置对象创建连接池
4. 编写方法从连接池中获取一个连接对象

### jedis.properties配置文件

```properties
# 主机名
host=localhost
# 端口号
port=6379
# 最大连接数
maxTotal=20
# 最长等待时间
maxWaitMillis=3000
```

### JedisUtils.java

```java
/**
 * 1. 读取配置文件创建连接池
 * 2. 调用方法从连接池中获取连接对象
 */
public class JedisUtils {
    private static JedisPool pool;   //声明连接池对象
    static {
        //1.读取配置文件中属性
        ResourceBundle bundle = ResourceBundle.getBundle("jedis");
        String host = bundle.getString("host");
        int maxTotal = Integer.parseInt(bundle.getString("maxTotal"));
        int maxWaitMillis = Integer.parseInt(bundle.getString("maxWaitMillis"));
        int port = Integer.parseInt(bundle.getString("port"));
        //2.创建配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(maxTotal);
        config.setMaxWaitMillis(maxWaitMillis);
        //3.创建连接池
        pool = new JedisPool(config, host, port);
    }
    /**
     * 从连接池中得到连接对象
     */
    public static Jedis getJedis() {
        return pool.getResource();
    }
}
```

### 使用工具类

```java
/**
 * 使用连接池工具类
 */
public class Demo4 {
    public static void main(String[] args) {
        //1. 使用工具类得到连接对象
        Jedis jedis = JedisUtils.getJedis();
        //2. 使用连接对象，添加hash类型字段和值
        jedis.hset("employee", "name", "NewBoy");
        jedis.hset("employee", "salary", "3000");
        //3. 获取hash类型的键中所有的字段和值
        Map<String, String> employee = jedis.hgetAll("employee");
        System.out.println(employee);
        //4. 关闭连接对象
        jedis.close();
    }
}
```

# 案例

## 适合放在redis中缓存的数据

```
经常查询，比较少修改的数据，类似于数据库中一个缓存
```

![1573028295212](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573028295212.png) 

## 需求

​	访问index.html页面，点击页面上的加载联系人按钮，使用ajax请求异步加载所有联系人列表。用户第一次访问从数据库中获取数据，以后都从redis缓存里面获取。

## 执行效果

### 页面效果

![1553426691167](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553426691167.png)

### 服务器控制台信息

![1553426713385](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553426713385.png) 

### MySQL中的数据

![1553426727532](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553426727532.png) 

### Redis中数据

![1553426746481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1553426746481.png) 



### 项目分析图

![1573029588751](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1573029588751.png)

## 数据访问层和业务层

- 导入的包

![1565516290169](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1565516290169.png)  

- 实体类

```java

/**
 * 联系人实体类
 */
public class Contact {

    private int id;  //编号
    private String name;  //姓名
    private String phone;  //电话
    private String email;  //邮箱
    private Date birthday;  //生日
```

- sqlMapConfig.xml

```xml
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
                <property name="url" value="jdbc:mysql://localhost:3306/day32"/>
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

- SqlSessionUtils.java

```java
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

- jedis.properties

```properties
# 主机名
host=localhost
# 端口号
port=6379
# 最大连接数
maxTotal=30
# 最长等待时间
maxWaitMillis=3000
```

- JedisUtils.java

```java
/
 * 创建Jedis工具类
 */
public class JedisUtils {
    private static JedisPool pool;
    static {
        //读取src下的配置文件，得到ResourceBundle对象
        ResourceBundle bundle = ResourceBundle.getBundle("jedis");
        //得到所有的属性并且赋值
        String host = bundle.getString("host");
        //以下属性要转成整数类型
        int port = Integer.parseInt(bundle.getString("port"));
        int maxTotal = Integer.parseInt(bundle.getString("maxTotal"));
        int maxWaitMillis = Integer.parseInt(bundle.getString("maxWaitMillis"));
        //创建配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxWaitMillis(maxWaitMillis);
        config.setMaxTotal(maxTotal);
        //创建连接池对象
        pool = new JedisPool(config, host, port);
    }
    /
     * 从连接池中得到Jedis连接对象
     */
    public static Jedis getJedis() {
        return pool.getResource();
    }
}
```

### 项目结构

![1565516502428](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/Redis/1565516502428.png)  

### 数据访问层

```java
public interface ContactDao {
    /**
     * 查询所有的联系人
     * @return
     */
    @Select("select * from contact")
    List<Contact> findAll();
}
```

```java
public class ContactDaoImpl implements ContactDao {
    @Override
    public List<Contact> findAll() {
        //1.得到会话对象
        SqlSession session = SqlSessionUtils.getSession();
        //2. 通过会话得到代理接口对象
        ContactDao contactDao = session.getMapper(ContactDao.class);
        //3. 调用接口中方法
        List<Contact> contacts = contactDao.findAll();
        //4. 关闭会话
        session.close();
        //5. 返回结果
        return contacts;
    }
}
```

### 业务层

```java 
/**
 * 业务层
 */
public class ContactService {
    private ContactDao contactDao = new ContactDaoImpl();
    /**
     * 查询所有联系人返回JSON字符串
     */
    public String findAll() {
        //1. 从Redis中读取联系人JSON字符串
        Jedis jedis = JedisUtils.getJedis();
        String contacts = jedis.get("contacts");
        //2. 如果为空，调用DAO读取List<Contact>
        if (contacts == null) {
            System.out.println("访问MySQL读取数据");
            List<Contact> contactList = contactDao.findAll();
            //3. 转成JSON字符串，在Redis中存入结果
            try {
                contacts = new ObjectMapper().writeValueAsString(contactList);
                jedis.set("contacts", contacts);
            } catch (JsonProcessingException e) {
                e.printStackTrace();
            }
        }
        //4. 如果不空，从Redis中读取JSON字符串
        else {
            System.out.println("访问Redis读取数据");
        }
        //关闭连接
        jedis.close();
        return contacts;
    }
}
```

### Servlet

```java
@WebServlet("/list")
public class ListContactServlet extends HttpServlet {
    private ContactService contactService = new ContactService();
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 调用业务层得到JSON字符串
        String json = contactService.findAll();
        //2. 打印到浏览器
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

```html
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
<script type="text/javascript">
    //编写加载按钮的点击事件
    $("#btnLoad").click(function () {
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
    });
    //清空联系人
    $("#btnClear").click(function () {
        //清空子元素
        $("#contacts").empty();
    });
</script>
</body>
</html>
```

