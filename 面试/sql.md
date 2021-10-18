# Sql脚本

## DDL脚本

```sql
DROP DATABASE dlei
-- 1.三种注释： -- # /* */

##################### DDL脚本:建库建表，删库删表 ########################
-- a.创建数据库 
CREATE DATABASE dlei;
-- b.判断是否存在数据库，如果不存在则创建数据库dlei01
CREATE DATABASE IF NOT EXISTS dlei01;
-- c.创建数据库并指定字符集为gbk，如果没有指定默认是utf-8，
-- 在安装的时候已经指定。
CREATE DATABASE dlei02 CHARACTER SET gbk ;
-- d.查询数据库
-- 查看所有的数据库 
SHOW DATABASES;
-- 查看某个数据库的定义信息
SHOW CREATE DATABASE dlei02;
-- e.修改和删除数据库,使用数据库
-- 删除dlei02数据库
DROP DATABASE dlei02;
-- 将dlei01数据库的字符集改成gbk
ALTER DATABASE dlei01 CHARACTER SET gbk;
-- 查看正在使用的数据库
SELECT DATABASE();
-- 使用db1数据库
USE dlei;
##################### DDL脚本：建表，看表，删表，改表操作 ###################
/*
a.创建表的语法脚本：
-- 字段名就是列名，字段类型：数据类型，约束。多个字段之间使用逗号来分隔
-- 最后一个字段后面不能加“,”
create table 表名 (
   字段名1 字段类型1 约束1,
   字段名2 字段类型2 约束2,
   字段名3 字段类型3 约束3
)
*/
USER dlei;
CREATE TABLE student(
	id INT ,
	NAME VARCHAR(20),
	sex VARCHAR(2),
	email VARCHAR(20),
	money DOUBLE,
	birthday DATETIME
)

-- b.查看表结构
-- 查看当前数据下全部的表。
SHOW TABLES;
-- 查看某个表的结构
DESC student;
-- 查看创建表使用的SQL语句
SHOW CREATE TABLE student;
-- 复制表结构,不会复制数据。
CREATE TABLE student01 LIKE student;

-- c.删除表
DROP TABLE student01;
-- 判断表是否存在再删除表 
DROP TABLE IF EXISTS student01;

-- d.修改表结构
-- 添加表列 ：ALTER TABLE 表名 ADD 字段名 数据类型;
ALTER TABLE student ADD phone VARCHAR(11);
-- 修改列类型MODIFY:alter table 表名 modify 字段名 新的数据类型;
ALTER TABLE student MODIFY phone VARCHAR(20);
-- 修改列名和类型 CHANGE: alter table 表名 change 旧列名 新列名 数据类型;
ALTER TABLE student CHANGE phone tel VARCHAR(21);
-- 删除列 DROP: alter table 表名 drop 列名;
ALTER TABLE student DROP tel;
-- 修改表名：rename table 旧表名 to 新的表名;
RENAME TABLE student TO newstudent;
```
## DML脚本
```sql
################ DML : 操作表数据：插入数据，删除数据，修改数据 ###############
## 插入表数据
-- 1.插入全部数据： insert into 表名 values(值1,值2...);
INSERT INTO student 
VALUES(1 , '小黑同学', '男' , '110@qq.com' , 1000000.0 , '1997-10-11 00:00:01');

-- 2.插入指定字段数据 ：INSERT INTO 表名(字段1,字段2,字段3) VALUES(值1 , 值2 , 值3)
INSERT INTO student(id , NAME , email) VALUES(2,'小白同学','112@qq.com');

-- 3.可以一次性插入多条记录（MySQL独有）
-- 添加3条记录
-- insert into 表名 values(值1,值2),(值1,值2),(值1,值2);
-- insert into 表名(字段1,字段2) values(值1,值2),(值1,值2),(值1,值2);
INSERT INTO student(id , NAME , email) VALUES (2,'小白同学','112@qq.com')
,(3,'小红同学','113@qq.com'),(4,'小绿同学','114@qq.com');

## 修改表数据
-- 1.不带条件修改：修改表全部的该字段值：update 表名 set 字段名=值
UPDATE student SET sex = '人妖';
-- 2.带条件修改：update 表名 set 字段名=值 where 条件
UPDATE student SET id = 2 , NAME = '小紫' WHERE id = 22;
UPDATE student SET id = 5 , sex = '女' , money = 11111 WHERE id = 21;

## 删除表数据 
-- 1.不带条件删除：
-- 删除表中所有的记录，只删除记录不删除表结构
DELETE FROM 表名;
DELETE FROM student; -- 只删除记录不删除表结构

-- 2.相当于先删除表结构：drop table 表名;
-- 创建一张相同结构的表：create table 表名;
-- 截断表：也会删除表的全部数据，删除之前的整个表结构，再建一个新表。
TRUNCATE student ; 

-- 3.带条件删除
-- 删除符合条件的记录
DELETE FROM 表名 WHERE 条件
DELETE FROM student WHERE id = 2;
```

## DQL脚本
```sql
############## DQL：查询表数据 ####################
-- 1.简单查询:
-- 查询表所有行和列的数据: *代表全部行和全部列数据。
SELECT * FROM student;
-- 查询所有行的某些字段值: select 列名1,列名2 from 表名;
SELECT id , NAME , email FROM student;
-- 2.查询指定列的别名(AS关键字可以省略不写)
SELECT id AS 编号 , NAME AS 姓名 , email AS 邮箱 FROM student;
SELECT id 编号 , NAME 姓名 , email 邮箱 FROM student;
-- 3.清除重复值 
-- 查询学生来至于哪些地方,并且去掉重复行
SELECT address FROM student;
-- DISTINCT去重复的关键字。
SELECT DISTINCT address FROM student;
-- 查询学生的姓名和地址,去掉重复行。必须几个列都相同，才会去除
SELECT DISTINCT address , NAME FROM student;
-- 4.查询结果参与运算
/*
CREATE TABLE student02 (
id int, -- 编号
name varchar(20), -- 姓名
age int, -- 年龄
sex varchar(5), -- 性别
address varchar(100), -- 地址
math int, -- 数学
english int -- 英语
);
INSERT INTO student02(id,NAME,age,sex,address,math,english) VALUES (1,'马云',55,'男','杭
州',66,78),(2,'马化腾',45,'女','深圳',98,87),(3,'马景涛',55,'男','香港',56,77),(4,'柳岩
',20,'女','湖南',76,65),(5,'柳青',20,'男','湖南',86,NULL),(6,'刘德华',57,'男','香港
',99,99),(7,'马德',22,'女','香港',99,99),(8,'德玛西亚',18,'男','南京',56,65);
*/
-- 查询的列运算 ： select 列+数值 from 表名;
SELECT math , math + 10 加10分 FROM student02;
-- 某列数据和其他列数据参与运算 
-- IFNULL : 内置函数，一旦查询出的字段为null ,选择用0代替。 
SELECT math , english , IFNULL(math,0) + IFNULL(english,0) 总分 FROM student02 ;
-- 注意: 参与运算的必须是数值类型
SELECT * , (math + IFNULL(english,0)) 总成绩 FROM student02 ;


## 2.条件查询
-- a. > < >= <= =  != <>
-- 查询math分数大于80分的学生
SELECT * FROM student02 WHERE math > 80;

-- 查询english分数小于或等于78分的学生
SELECT * FROM student02 WHERE english <=78;
 
-- 查询age等于20岁的学生
SELECT * FROM student02 WHERE age = 20;

-- 查询age不等于20岁的学生，注：不等于有两种写法
SELECT * FROM student02 WHERE age <> 20;
SELECT * FROM student02 WHERE age != 20;

-- b. 逻辑运算符 : && and  || or   ! not
-- 查询出女生中英文成绩大于等于80分的妹子。
SELECT * FROM student02 WHERE sex = '女' AND english >= 80
SELECT * FROM student02 WHERE sex = '女' && english >= 80

-- 查询性别是女生，或者数学成绩小于等于60的学生.
SELECT * FROM student02 WHERE sex = '女' OR math <= 60 
SELECT * FROM student02 WHERE sex = '女' || math <= 60 
-- 查询id是1或3或5的学生
SELECT * FROM student02 WHERE id=1 OR id=3 OR id=5;

-- 非： ! not .
-- 查询出id不是1 3 5的学生。
SELECT * FROM student02 WHERE id NOT IN ( 1 , 3 , 5 ) 

-- c.DQL条件中查询使用的关键字 
-- select 列名 from 表名 where 列名 in (值1，值2，值3);
-- 查询出id是1 3 5 7的学生。
SELECT * FROM student02 WHERE id IN ( 1, 3 , 5 , 7);
-- 查询出id不是1 3 5 7的学生。
SELECT * FROM student02 WHERE id NOT IN ( 1, 3 , 5 , 7);

-- d.范围查询(包前又包后)
-- select 列名 from 表名 where 列名 between 小值 and 大值
SELECT * FROM student02 WHERE age BETWEEN 20 AND 55 ;
SELECT * FROM student02 WHERE age >= 20 AND age <= 55 ;

-- e.模糊查询：like关键字
-- 查询名称包含马的：%可以匹配任意字符，可以没有。
SELECT * FROM student02 WHERE NAME LIKE '%马%';
-- 查询姓马的
SELECT * FROM student02 WHERE NAME LIKE '马%';
-- 查询姓马的，名称是2个字的。_代表任意字符但是只能1个。
SELECT * FROM student02 WHERE NAME LIKE '马_'

-- f.查询为空的列
SELECT * FROM student02 WHERE english = NULL;  -- 什么都查不到,错误写法！
SELECT * FROM student02 WHERE english IS NULL; -- 查询为null 应该使用的is null
SELECT NAME,IFNULL(english,0) 英语 FROM student02;

-- g. DQL：查询结果排序
-- select 列名 from 表名 order by 列名 asc/desc
-- 按照数学成绩升序：ASC
SELECT * FROM student02 ORDER BY math ASC;
-- 按照英语成绩降序排序
SELECT * FROM student02 ORDER BY english DESC;
-- 按照数学成绩升序排序，如果数学成绩相同，再按照英语成绩降序排序。
SELECT * FROM student02 ORDER BY math ASC , english DESC;
-- 按照总分降序排序 
SELECT * , IFNULL(english,0)  + IFNULL(math,0) 总分 FROM student02 ORDER BY 总分 DESC;

-- h.聚合函数
-- count : 统计行数 
-- sum : 求和
-- avg : 求平均值 
-- max : 求最大值
-- min : 求最小值
-- 聚合函数针对列来操作
-- 查询学生总数
SELECT COUNT(id) FROM student02;  -- 8
SELECT COUNT(english) FROM student02;  -- 7
SELECT COUNT(*) FROM student02; -- *是直接统计行数。最靠谱的：8 

-- 查询年龄大于40的总数
SELECT COUNT(*)  FROM student02 WHERE age > 40;

-- 查询数学成绩总分
SELECT SUM(math) 数学总分 FROM student02; 

-- 查询数学成绩平均分
SELECT AVG(math) 数学平均分 FROM student02; 

-- 查询数学成绩最高分
SELECT MAX(math) 数学最高分 FROM student02; 

-- 查询数学成绩最低分
SELECT MIN(math) 数学最高分 FROM student02; 

-- 需求：查询出数学成绩低于平均分的家伙。
-- select * from student02 where math < avg(math); -- 错误代码，聚合函数不能用在where之后。

-- i：分组查询：select 列名 from 表名 group by 列名 having 条件
-- 1.需求：查询出男人中和女人中的数学最高分。
SELECT sex , MAX(math) FROM student02 GROUP BY sex ;
-- 2.按性别进行分组，求男生和女生数学的平均分
SELECT sex , AVG(math) FROM student02 GROUP BY sex;
-- 3.按性别进行分组，求男女生的人数
SELECT sex, COUNT(*) FROM student02 GROUP BY sex; 

-- 分组中：聚合函数与条件一起使用
-- 查询年龄大于25岁的人,按性别分组,统计每组的人数
-- 先过滤，再分组,再统计
SELECT sex , COUNT(*) FROM student02 WHERE age > 25 GROUP BY sex;

-- 查询年龄大于25岁的人，按性别分组，统计每组的人数，并只显示性别人数大于2的数据
-- 在分组之后加条件，必须用Having跟条件。
SELECT sex , COUNT(*) FROM student02 WHERE age > 25 GROUP BY sex HAVING COUNT(*) > 2;
/*
	where:
		1. 先过滤，再进行分组   2. where后面不能使用聚合函数
        having		
		1. 先分组，再进行过滤   2. having后面可以使用聚合函数，having用在group by后面	
*/

-- j:分页查询。
INSERT INTO student02(id,NAME,age,sex,address,math,english) VALUES 
(9,'唐僧',25,'男','长安',87,78),
(10,'孙悟空',18,'男','花果山',100,66),
(11,'猪八戒',22,'男','高老庄',58,78),
(12,'沙僧',50,'男','流沙河',77,88),
(13,'白骨精',22,'女','白虎岭',66,66),
(14,'蜘蛛精',23,'女','盘丝洞',88,88);
-- 分页：select * from 表名称 ... limit 起始数据的索引, 查询多少条。
-- 一页展示：5条
-- 第一页：
SELECT * FROM student02 LIMIT 0 , 5;
SELECT * FROM student02 LIMIT 5;  -- 第一页可以省略起始数据的索引。

-- 第二页
SELECT * FROM student02 LIMIT 5 , 5;
-- 第三页：
SELECT * FROM student02 LIMIT 10 , 5;
-- 注意：limit永远只能写在Sql语句的最后面
-- select语句的关键字顺序
SELECT 字段 FROM 表 
WHERE 条件 
GROUP BY 分组列 
HAVING 过滤条件 
ORDER BY 排序列 
LIMIT 起始数据的索引 , 查询多少条。
```
## 表约束
```sql
############### 表约束 #############################
-- 1.主键约束。
CREATE TABLE USER(
	id INT PRIMARY KEY , -- 主键约束：非空，且唯一。
	NAME VARCHAR(20) NOT NULL , -- 不能为空 非空约束
	email VARCHAR(20) UNIQUE -- 唯一约束。
)

INSERT INTO USER(id , NAME , email) VALUES(1 , '苍老师','110@qq.com');
INSERT INTO USER(NAME , email) VALUES('苍老师2','112@qq.com');
INSERT INTO USER(id ,  email) VALUES(2 ,'113@qq.com');
INSERT INTO USER(id , NAME , email) VALUES(3 , NULL,'114@qq.com'); -- 报错!
INSERT INTO USER(id , NAME , email) VALUES(4 , '松岛老师','113@qq.com'); -- 报错!
INSERT INTO USER(id , NAME , email) VALUES(4 , '松岛老师','113@qq.com'); -- 报错!
INSERT INTO USER(id , NAME , email) VALUES(4 , '松岛老师','114@qq.com'); 

-- 2.建表后加主键（几乎不用）
-- alter table 表名 add primary key(列名);
ALTER TABLE USER ADD PRIMARY KEY(id);

-- 删除USER表的主键
ALTER TABLE USER DROP PRIMARY KEY;

-- 表存在的情况下，添加主键
ALTER TABLE USER ADD PRIMARY KEY(id);


## 主键自增配置与修改。 1000万条数据。
-- 程序员无需维护主键的数据，主键会由数据库方自动自增维护。
-- AUTO_INCREMENT用在主键上约束主键自增。
CREATE TABLE user01(
	id INT PRIMARY KEY AUTO_INCREMENT, -- 主键自增配置。
	loginName VARCHAR(23) NOT NULL ,
	username VARCHAR(23),
	PASSWORD VARCHAR(23) NOT NULL
)
INSERT INTO user01(loginName , username , PASSWORD) VALUES ('swk','孙悟空','huaguoshan110');

-- 将主键的起始值设置为1000
ALTER TABLE user01 AUTO_INCREMENT = 1000;
-- delete删除表不会影响自增的id值
DELETE FROM user01;
-- TRUNCATE删除表，自增恢复到1开始。
TRUNCATE user01;
```
## 外键约束与级联操作
```sql
#################### 表约束相关的技术 ##########################
DROP DATABASE dlei;
CREATE DATABASE dlei;
## 1.主键自增，非空，唯一，默认值
CREATE TABLE USER(
	id INT PRIMARY KEY AUTO_INCREMENT, -- 主键自增
	loginName VARCHAR(23) NOT NULL UNIQUE ,
	usernmae VARCHAR(23) NOT NULL,
	address VARCHAR(23) DEFAULT '广州'
)

INSERT INTO USER(loginName , usernmae ,address) VALUES('zbj','猪八戒',NULL);
INSERT INTO USER(loginName , usernmae ,address) VALUES('zbj1','猪八戒1','');
INSERT INTO USER(loginName , usernmae ) VALUES('zbj2','猪八戒2');

## 2.外键约束的介绍
-- a.引入：单张表存储数据的情况。
-- 创建一个员工表
CREATE TABLE emp (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键自增长
    NAME  VARCHAR(20),
    age INT,
    dep_name VARCHAR(10),  -- 部门名
    dep_location VARCHAR(20)  -- 部门所在城市
);

SELECT * FROM emp;
-- 添加数据
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');

INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');
-- 结论：单张表存储信息可能出现大量数据的冗余，例如此表中部门信息大量冗余。
-- 解决方案：员工表单独建一张表，部门表也单独建一张表。
-- 员工表中建立所谓的外键去部门表中关联员工的部门信息数据。
-- 创建外键约束：前提是至少两种表。
-- 解决方法：将部门设计成一张表
CREATE TABLE department (
   id INT PRIMARY KEY AUTO_INCREMENT,
   dep_name VARCHAR(10),  -- 部门名
   dep_location VARCHAR(20)  -- 部门所在城市
);
-- 添加部门表的记录
INSERT INTO department(dep_name,dep_location) VALUES('研发部', '广州'),('销售部', '深圳');
SELECT * FROM department;

-- 员工表
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键自增长
    NAME  VARCHAR(20),
    age INT,
    dept_id INT  -- 外键，引用部门表中主键
);
-- 添加员工信息
INSERT INTO employee (NAME, age, dept_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('王五', 20, 1);

INSERT INTO employee (NAME, age, dept_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王', 18, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王1', 18, 3); -- 与数据正确性违背。
-- 真正引入外键约束：
-- a.在建表之后添加外键约束
-- alter table 表名 add constraint 约束名 foreign key(外键字段名) references 主表(主键)
-- 以后删除外键要使用别名：fk_dept_id
ALTER TABLE employee ADD CONSTRAINT fk_dept_id FOREIGN KEY(dept_id) REFERENCES department(id);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王1', 18, 3); -- 错误
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王1', 18, 2); -- 正确

-- b.在建表时添加外键约束
DROP TABLE employee;
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键自增长
    NAME  VARCHAR(20),
    age INT,
    dept_id INT,  -- 外键，引用部门表中主键
    FOREIGN KEY (dept_id) REFERENCES department(id)
);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王1', 18, 3); -- 错误
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王1', 18, 2); -- 正确

-- c.删除外键约束
ALTER TABLE employee DROP FOREIGN KEY employee_ibfk_1;
-- 在employee表情存在的情况下添加外键
ALTER TABLE employee ADD CONSTRAINT fk_dept_id FOREIGN KEY(dept_id) REFERENCES department(id);

#### 外键级联更新和级联删除
-- a.准备数据
DROP TABLE employee;
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键自增长
    NAME  VARCHAR(20),
    age INT,
    dept_id INT,  -- 外键，引用部门表中主键
    FOREIGN KEY (dept_id) REFERENCES department(id)
);
INSERT INTO employee (NAME, age, dept_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('王五', 20, 1);

INSERT INTO employee (NAME, age, dept_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王', 18, 2);

-- 引入：
DELETE FROM department WHERE id = 1; -- 不能删除成功!!
UPDATE department SET id = 10 WHERE id = 1; -- 不能修改成功！
-- 原因：外键约束降低数据冗余的同时，还对数据的一致性有强力的约束。
-- 实现级联删除：在删除部门数据的同时，关联部门的全部员工也会被删除。
-- 实现级联修改：在修改部门主键的同时，关联部门的外键也会被修改。
-- 添加外键约束，级联更新和级联删除
-- a.建表后加级联删除和级联修改
ALTER TABLE employee ADD CONSTRAINT fk_emp_dept 
FOREIGN KEY (dept_id) REFERENCES department(id) ON UPDATE CASCADE ON DELETE CASCADE;
-- b.建表时添加联删除和级联修改
DROP TABLE employee;
CREATE TABLE employee (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键自增长
    NAME  VARCHAR(20),
    age INT,
    dept_id INT,  -- 外键，引用部门表中主键
    FOREIGN KEY (dept_id) REFERENCES department(id) ON UPDATE CASCADE ON DELETE CASCADE
);
INSERT INTO employee (NAME, age, dept_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dept_id) VALUES ('王五', 20, 1);

INSERT INTO employee (NAME, age, dept_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dept_id) VALUES ('小王', 18, 2);

DELETE FROM department WHERE id = 1; -- 删除成功!!
UPDATE department SET id = 10 WHERE id = 2; -- 修改成功！
```
## DCL创建用户和分配权限
```sql
-- root/root 超级管理员用户。 容易导致删库跑路。
-- 创建一些新用户，权限就没有那么多。可以精确分配权限。这是属于数据库管理员的工作。 

-- 1.创建新用户。
# CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
# 创建新用户，没有分配权限。
CREATE USER 'dlei'@'127.0.0.1' IDENTIFIED BY 'dlei'; # 只能用127.0.0.1==localhost登陆，无权限
# user1用户只能在localhost/127.0.0.1这个IP登录mysql服务器
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'user1';# 只能用127.0.0.1==localhost登陆，无权限
# user2用户可以在任何电脑上登录mysql服务器
CREATE USER 'user2'@'%' IDENTIFIED BY 'user2';

-- 2.给用户分配权限
# GRANT 权限1, 权限2... ON 数据库名.表名 TO '用户名'@'主机名';
# 把dlei数据库本身的权限都给dlei用户。
GRANT CREATE,ALTER,DROP,INSERT,UPDATE,DELETE,SELECT ON dlei.* TO 'dlei'@'localhost';
FLUSH PRIVILEGES; -- 刷新权限

#全部数据库的权限.相当于root权限的操作功能都给了user2
GRANT ALL ON *.* TO 'user2'@'%'; # 第一个*所有数据库，第二个*所有表

-- 3.撤销授权
#REVOKE  权限1, 权限2... ON 数据库.表名 FROM '用户名'@'主机名';
REVOKE ALL ON dlei.* FROM 'dlei'@'localhost';

-- 4.查看权限
# SHOW GRANTS FOR '用户名'@'主机名';
SHOW GRANTS FOR 'user2'@'%';

-- 5.删除用户
DROP USER 'user2'@'%';

-- 6.修改管理员密码：不能先登陆，在cmd直接改，输入旧密码即可改正!
# mysqladmin -uroot -p password 新密码  -- 新密码不需要加上引号
mysqladmin -uroot -p PASSWORD dlei
-- 7.修改普通用户自己密码，先登陆！
SET PASSWORD FOR 'dlei'@'127.0.0.1' = PASSWORD('666666');
-- 8.修改当前登陆用户root的密码
SET PASSWORD = PASSWORD('666666')
```
## 数据库备份与还原
```sql
############ 数据库的备份介绍################
-- 1.基础备份：把当前数据库的某些数据（库，表，数据）保存一份：只能保存此刻的全部数据。
-- 快照备份：
-- 在CMD中使用命令备份一次：mysqldump -u用户名 -p密码 数据库名 > 文件名
mysqldump -uroot -proot dlei > D:/itcast/dlei.sql
DROP DATABASE dlei;

-- 2.还原:在cmd中
mysql -uroot -proot
CREATE DATABASE dlei;
USE dlei ;
source D:/itcast/dlei.sql

-- 3.使用工具可以直接在数据库上进行数据的备份导出与还原导入
```
## 表关系的案例实现
```sql
#################表关系： 1-1  1-N  N-N #####################
-- 1.一对一的关系：学生信息 - 身份证信息。
-- 主表
CREATE TABLE stu(
   id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键
   NAME VARCHAR(20)  
);
-- 从表
CREATE TABLE info(
    id INT PRIMARY KEY AUTO_INCREMENT,  -- 主键
    address VARCHAR(20),
    use_name VARCHAR(10),
    weight DOUBLE,
    -- 直接把主键约束成外键。 主键也可以是外键。
    FOREIGN KEY(id) REFERENCES stu(id)
);

-- 2. 1-N的关系： 线路分类 - 旅行线路
-- 把1的一方的主键拿来作为多的一方的外键。
CREATE TABLE TYPE(
   cid INT PRIMARY KEY AUTO_INCREMENT,
   cname VARCHAR(100) NOT NULL UNIQUE   
);
CREATE TABLE route(
   rid INT PRIMARY KEY AUTO_INCREMENT,
   rname VARCHAR(100) NOT NULL UNIQUE,
   price DOUBLE,
   rdate DATE,
   cid INT,  -- 外键
   FOREIGN KEY(cid) REFERENCES TYPE(cid)
);

-- 3.N-N: 收藏表：用户与线路之间是多对多的关系，中间表就是收藏表。
-- 用户表
CREATE TABLE tab_user(
   uid INT PRIMARY KEY AUTO_INCREMENT,
   username VARCHAR(100) UNIQUE NOT NULL,
   PASSWORD VARCHAR(30) NOT NULL,
   NAME VARCHAR(100),
   birthday DATE,
   sex CHAR(1),
   telephone VARCHAR(11),
   email VARCHAR(100)
);
-- 收藏表：把用户表的主键和线路表的主键作为联合主键。
CREATE TABLE tab_favorite (
   r_id INT,  -- 线路id的外键
   DATE DATETIME, -- 收藏时间
   uid  INT ,  -- 用户id的外键
   -- 创建联合主键
   PRIMARY KEY(r_id, uid),
   FOREIGN KEY(r_id) REFERENCES route(rid),  -- 关联了线路的主键作为外键
   FOREIGN KEY(uid) REFERENCES tab_user(uid)  -- 关联了用户的主键作为外键
);
```
## 多表查询的技术：内连接，外连接，子查询
```sql
######################多表中数据的查询####################
-- 1.准备数据 
CREATE DATABASE day15;
# 创建部门表
CREATE TABLE dept(
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(20)
);

INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');  
# 创建员工表
CREATE TABLE employee (
  id INT PRIMARY KEY AUTO_INCREMENT,
  NAME VARCHAR(10),
  gender CHAR(1),   -- 性别
  salary DOUBLE,   -- 工资
  join_date DATE,  -- 入职日期
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES dept(id) -- 外键，关联部门表(部门表的主键)
);  

INSERT INTO employee(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO employee(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO employee(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO employee(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO employee(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);

-- 2.笛卡尔积现象:直接对两张表进行查询，会出现让两张表的数据进行交叉组合。
-- 笛卡尔积是一种错误的查询方式，直接简单粗暴的，把两表的数据完整组合，会产生很多错误的组合。
SELECT * FROM employee , dept;

-- 3.正确的多表查询的方式:
-- a.消除笛卡儿积现象，得到正确的组合 
SELECT * FROM employee , dept WHERE employee.`dept_id` = dept.`id` -- 隐式内连接。

-- 4.多表查询的正确完整方式：
多表查询：	
	-- 内连接：
	     隐式内连接。
	     显示内连接。
	     
	-- 外连接：
	     左外连接(左连接)			
	     由外连接(右连接)
		
        -- 子查询		
	     	
a.隐式内连接: SELECT 列名 FROM 左表,右表 WHERE 从表.外键=主表.主键
SELECT * FROM employee , dept WHERE employee.`dept_id` = dept.`id` -- 隐式内连接。

b.显示内连接: SELECT 列名 FROM 左表 INNER JOIN 右表 ON 从表.外键=主表.主键
SELECT * FROM employee INNER JOIN dept ON employee.`dept_id` = dept.id -- 显示内连接。
-- 可以省略inner不写
SELECT * FROM employee JOIN dept ON employee.`dept_id` = dept.id;
-- 建议使用别名定义表名
SELECT * FROM employee e JOIN dept d ON e.`dept_id` = d.`id`;

案例：查询唐僧的信息，显示员工id，姓名，性别，工资和所在的部门名称
分析几张表：employee , dept
简单粗暴连接表：隐式内连接|显示内连接。
过滤条件：
SELECT e.id,e.name,e.gender,e.salary,d.name FROM employee e JOIN dept d 
ON e.dept_id = d.id WHERE e.name = '唐僧';	  

-- 5.外连接的介绍：左连接，右连接。
-- 内连接的作用：只有可以匹配连接的数据全会被连接出来。
SELECT * FROM employee e JOIN dept d ON e.`dept_id` = d.`id`;

-- 左连接：在内连接的基础上，保全左表的全部数据，右边如果没有用null代替。
-- select 列名 from 左表 left join 右表 on 从表.外键=主表.主键
SELECT * FROM employee e LEFT JOIN dept d ON e.`dept_id` = d.`id`;

-- 右连接：在内连接的基础上，保全右表的全部数据，左边如果没有用null代替。
-- select 列名 from 左表 RIGHT join 右表 on 从表.外键=主表.主键
SELECT * FROM employee e RIGHT JOIN dept d ON e.`dept_id` = d.`id`;

-- 保全两表全部的数据：同时要实现内连接(拓展)
SELECT * FROM employee e LEFT JOIN dept d ON e.`dept_id` = d.`id`
UNION
SELECT * FROM employee e RIGHT JOIN dept d ON e.`dept_id` = d.`id`

-- 6.子查询 ：把一个查询的结果直接给另一个查询使用。
-- 这是一种查询语句的嵌套，嵌套的SQL查询称为子查询。
-- 如果使用子查询必须要使用括号
需求：查询开发部中有哪些员工
-- 先查询出开发部的编号。
SELECT id FROM dept WHERE NAME = '开发部';  1 
-- 查询员工表，必须部门dept_id=1 
SELECT * FROM employee WHERE dept_id = 1;
-- 以上两行结合成一行sql: 子查询 
-- 如果使用子查询必须要使用括号
SELECT * FROM employee WHERE dept_id = ( SELECT id FROM dept WHERE NAME = '开发部' );


-- 7.子查询结果：是单行单列的情况，子查询的结果是一个值的时候。
-- 如果子查询是单行单列，父查询使用比较运算符：> < = != 
-- 案例：查询工资最高的员工是谁？ 
-- a.查询出当前最高工资
SELECT MAX(salary) FROM employee;
-- b.查询员工表，工资是最高工资 
SELECT * FROM employee WHERE salary = (SELECT MAX(salary) FROM employee )

-- 查询工资大于"蜘蛛精"工资的员工
-- a.查询出蜘蛛精的工资
SELECT salary FROM employee WHERE NAME = '蜘蛛精'
-- b.查询员工的工资大于蜘蛛精工资的员工 
SELECT * FROM employee WHERE salary > (SELECT salary FROM employee WHERE NAME = '蜘蛛精')

-- 8. 子查询: 多行单列的情况
-- 多行单列认为是一个数组，父查询使用 in / any /all
-- 案例：查询工资大于5000的员工，来自于哪些部门，得到部门的名字。
-- a.查询出工资大于5000的员工的部门id
SELECT DISTINCT dept_id FROM employee WHERE salary > 5000; -- 1 , 2
-- b.查询部门表，部门id必须在上面的查询结果之内。
SELECT NAME FROM dept WHERE id IN (SELECT DISTINCT dept_id FROM employee WHERE salary > 5000)

-- 列出工资高于在1号部门工作的所有员工，显示员工姓名和工资、部门名称。
-- 1. 查询1号部门所有员工的工资，得到多行单列
SELECT salary FROM employee WHERE dept_id = 1;

-- 2. 使用大于号不能计算，怎么办 
SELECT * FROM employee WHERE salary > ALL (SELECT salary FROM employee WHERE dept_id=1);
-- any表示任何一个，all所有:
SELECT * FROM employee WHERE salary > ANY (SELECT salary FROM employee WHERE dept_id=1);

-- 9.子查询: 多行多列的情况
-- 认为它是一张虚拟表，可以使用表连接再次进行多表查询
-- 案例：查询出2011年以后入职的员工信息，包括部门名称。
-- >= 2012-01-01
-- a.查询出2012-01-01开始加入的全部员工信息。
SELECT * FROM employee WHERE join_date >= '2012-01-01';
-- b.查询出这些员工的部门信息：当成新表与部门信息进行内连接
SELECT * FROM (SELECT * FROM employee WHERE join_date >= '2012-01-01') e
JOIN dept d ON e.dept_id = d.id
```
## 多表查询的练习题
```sql
DROP DATABASE day15 ;
CREATE DATABASE day15 ;
USE day15 ;

#### 1.准备数据。
-- 部门表
CREATE TABLE dept (
  id INT PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');

-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);

-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,   -- 级别
  losalary INT,  -- 最低工资
  hisalary INT -- 最高工资
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);

1.查询所有员工姓名，工资，工资等级
-- a.确定表：员工表，工资等级
-- b.简单粗暴连接表。
-- c.加条件 
SELECT e.ename,e.salary,s.grade FROM emp e JOIN salarygrade s 
ON e.salary BETWEEN s.losalary AND hisalary;

2.查询经理的信息。显示经理姓名，工资，职务名称，部门名称，工资等级
-- a.确定表：员工表，职务表，工资等级 ,部门表
-- b.简单粗暴连接表。
-- c.加条件 
SELECT e.ename 姓名 ,e.salary 薪水 ,j.jname 职位 ,d.dname 部门 ,s.grade 职级 FROM emp e JOIN job j JOIN dept d JOIN `salarygrade` s
ON e.job_id = j.id AND e.dept_id = d.id AND e.salary BETWEEN s.losalary AND hisalary
WHERE j.jname = '经理' 

3.查询部门编号、部门名称、部门位置、部门人数
-- a.确定表：员工表，部门表
-- b.简单粗暴连接表。
-- c.加条件 
SELECT d.id , d.dname , d.loc , COUNT(e.id) 
FROM dept d LEFT JOIN emp e ON d.id = e.dept_id GROUP BY d.id; 

4.列出所有员工的姓名及其直接上级的姓名，没有领导的员工也需要显示
-- a.确定表：员工表。
-- b.简单粗暴连接表。
-- c.加条件 
SELECT e1.ename 自己 , e2.ename 领导 
FROM emp e1 LEFT JOIN emp e2 ON e1.mgr = e2.id;

5.查询工资高于公司平均工资的所有员工列：员工所有信息，部门名称，上级领导，工资等级。
注：所有员工都要显示出来，没有上级的员工显示为"自己"
-- a.确定表：员工表，部门，等级表
-- b.简单粗暴连接表。
-- c.加条件 注意：边连表边加条件。
-- 先找出工资高于公司平均工资的所有员工 
SELECT * FROM emp WHERE salary > (SELECT AVG(salary) FROM emp)
-- 简单粗暴连接表。
SELECT e1.* , d.dname 部门名称 , IFNULL(e2.ename,'自己') 领导 , s.grade 级别 
FROM (SELECT * FROM emp WHERE salary > (SELECT AVG(salary) FROM emp)) e1
LEFT JOIN emp e2 ON e1.mgr = e2.id JOIN dept d ON e1.dept_id = d.id 
JOIN `salarygrade` s ON e1.salary BETWEEN s.losalary AND hisalary
```
# 使用案例

## 第二高薪水

```sql
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary
```

第N高薪水

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
set N:=N-1;
  RETURN (
     select Salary 
     from Employee
     order by Salary desc 
     limit N,1
  );
END
```

## 分数排名

```sql
select a.Score as Score,
(select count(distinct b.Score) from Scores b where b.Score >= a.Score) as Rank
from Scores a
order by a.Score DESC
```



# 事物与隔离级别

```sql
###############事物#############################
事物：一批操作要么同时成功，要么同时失败
事物可以实现某种程度上业务的安全性。
事物的四大特征：
    原子性：事物是一个整体，里面的操作不可拆分。
    一致性：事物操作前后的数据要一致。
    隔离性（Isolation）: 事务是可以并发执行的，理想的情况应该是所有的事务之间不能相互影响。
    持久性（Durability）:如果事务对数据库进行了操作，事物一旦成功或者失败，对数据库中数据影响是持久的。
CREATE DATABASE day16;
USE day16;   	
-- 创建数据表
CREATE TABLE account (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    balance DOUBLE
);
 
-- 添加数据
INSERT INTO account (NAME, balance) VALUES ('Jack', 1000), ('Rose', 1000);

-- 如果要完成Jack转账给Rose : 500
-- 至少2条，Jack扣钱，Rose加钱，要有2条update语句
UPDATE account SET balance = balance - 500 WHERE NAME='Jack';
UPDATE account SET balance = balance + 500 WHERE NAME='Rose';

-- 之前的每个事物的每步操作都是自动提交的！
-- 1.手工开启事物：手动提交。
-- a.start transaction ： 开启一个新事物的手动提交。
START TRANSACTION;
UPDATE account SET balance = balance - 500 WHERE NAME='Jack';
-- 模拟代码出现异常，回滚事物的操作。
-- rollback;
UPDATE account SET balance = balance + 500 WHERE NAME='Rose';
-- 模拟两部操作都没有问题：提交事物，持久化数据。
COMMIT;

-- 2.事物的自动提交。
-- 在默认的情况下，mysql每条SQL语句都会创建一个事务，且自动自交：默认方案。
-- a.查看当前数据库的事物方案。
SELECT @@autocommit;  -- 1表示是自动提交， 0表示要手动提交。
-- b.开启全局事物手动提交: 以后任何操作都要手动提交才能生效。
SET @@autocommit = 0;

UPDATE account SET balance = balance - 500 WHERE NAME='Jack';
-- 模拟代码出现异常，回滚事物的操作。
-- rollback;
UPDATE account SET balance = balance + 500 WHERE NAME='Rose';
-- 模拟两部操作都没有问题：提交事物，持久化数据。
COMMIT;


-- 3.事物的回滚点设置。
START TRANSACTION;
UPDATE account SET balance = balance - 500 WHERE NAME='Jack';
UPDATE account SET balance = balance + 500 WHERE NAME='Rose';
-- 在这里设置一个回滚点上面的操作认为已经没有问题。这样以后出现异常，也只会回滚到此处再提交!
SAVEPOINT my_trasfer_sp;
-- 发送短信提示用户：出现异常!
ROLLBACK TO my_trasfer_sp;
COMMIT;

-- 4.四种隔离级别的演示。
a.事物的隔离级别（读未提交）：出现脏读, 一个事物读取到了其他事物未提交的数据。
SHOW VARIABLES LIKE '%isolation%';
-- 或
SELECT @@tx_isolation;
-- 设置隔离级别为读未提交（重新登陆才可以生效）
SET GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

b.事物的隔离级别（读已提交）：不会出现脏读，一个事物之内可以多次读取到已提交的数据。
不会读取到未提交的数据。
SELECT @@tx_isolation;
-- 设置隔离级别为读未提交（重新登陆才可以生效）
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;

c.事物的隔离级别(可重复读)：一个事物之内读取到的数据永远是一开始读取到的，会出现幻读
SET GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;

d.事物的隔离级别(可串行化）：serializable可串行化。所有问题都不会出现
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;

SELECT * FROM tb_user WHERE loginName = '蜘蛛1精' AND PASSWORD = '123456'
```
#事务与JDBC
##入门概述
```java
/**
    目标：JDBC的入门概述。

    Java DataBase Connectivity（JDBC）:Java的数据库连接。
    JDBC是Java提供出来的连接数据库的统一规范：是一个连接数据库的技术，是最基本的持久层技术。

    数据库：产品。
    编程语言：程序开发。

    JDBC的来历（给 大家一个思考模型）：
        （1）sun公司找了数据库厂商说：把你的数据库产品源码给我解剖，我要整合你。 不合适
        （2）sun公司又找了数据库厂商：说你们数据库厂商自己写好连接数据库的接口，Java直接调用。似乎可以。
        （3）程序员不干了：假如数据库有一千种，那么程序员要学习一千套API操作数据库。又尴尬了！！
        （4）sun公司又出来了：我来制定连接数据库的规范，数据库厂商实现这些规范，程序员使用这些规范。
              这样程序员只需要学习sun公司指定的一套连接数据库规范就可以操作全部数据库。
              这套规范叫：JDBC。
        总结：JDBC中全是Java提供的一些接口等API，具体实现由数据库厂商实现。
        Oracle                           MySQL
        getConnection                 getConnection

    JDBC的核心思想：
        Java定义操作数据库的规范（接口等API）: JDBC.
        数据库厂商要实现Java的数据库规范：JDBC
        程序员学习JDBC，使用JDBC.

    连接数据库的使用前提：
        1.JDBC规范（JDK中已经自带）。
        2.具体数据库厂商的实现（数据库驱动）：去官网下载。

    小结：
        JDBC是Java连接数据库的统一规范。
        JDBC连接数据库需要：JDBC规范(Java代码)，具体数据厂商提供的数据库驱动。
 */
```
##常用API
```java
/**
    目标：JDBC的常用API说明 。

    JDBC开发使用到的包：
        1.java.sql ， javax.sql
        2.数据库的驱动: 由数据库厂商来提供
    JDBC的开发步骤：
        a.JDBC规范是JDK自带的。
        b.去下载MySQL数据库驱动。（jar包，数据库厂商按照JDBC规范写好的操作数据库的代码）
        c.导入MySQL数据库的驱动（导入jar包）
        d.使用JDBC操作数据库。

    JDBC的常用API说明：
     | 接口或类             | 作用
     | ------------------- | ----------------
     | DriverManager类     | 驱动管理器，加载驱动，获取连接对象
     | Connection接口      | 代表一个连接对象
     | Statement接口       | 代表一条SQL语句
     | ResultSet接口       | 代表返回的结果集
 */
```
##JDBC加载驱动获取与数据库的连接对象Connection
```java
/**
        目标：JDBC加载驱动获取与数据库的连接对象Connection.

        开发步骤：
         a.JDBC规范是JDK自带的。
         b.去下载MySQL数据库驱动。（jar包，数据库厂商按照JDBC规范写好的操作数据库的代码）
         c.导入MySQL数据库的驱动（导入jar包）
         d.创建数据库驱动（加载数据库驱动）
         e.通过驱动管理器注册该驱动。
         f.通过驱动管理器去获取与具体数据库的连接通道：Connection对象。

        DriverManager:
                1.public static void registerDriver(java.sql.Driver driver):注册驱动
                2.public static Connection getConnection(String url, java.util.Properties info)
                    参数一：数据库的地址
                    参数二：封装用户名和密码对象。
        小结：
               获取连接可以直接使用驱动管理器：
               DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16"
               , "root","root");即可。
 */
public class DriverDemo {
    public static void main(String[] args) throws Exception {
        // 1.创建数据库驱动
        Driver driver = new com.mysql.jdbc.Driver();
        // 2.通过驱动管理器注册该驱动。
        DriverManager.registerDriver(driver);
        // 3.通过驱动管理器去获取与具体数据库的连接通道：Connection对象。
        // 数据库的地址
        String url = "jdbc:mysql://127.0.0.1:3306/day16" ;
        // 数据库的用户名和登录密码
        Properties pro = new Properties();
        pro.setProperty("user","root");
        pro.setProperty("password","root");
        Connection con = DriverManager.getConnection(url , pro);

        System.out.printf("连接对象："+con);
    }
}
//优化代码
public class DriverDemo02 {
    public static void main(String[] args) throws Exception {
        // 1.创建数据库驱动
        // Driver driver = new com.mysql.jdbc.Driver();
        // 加载驱动的,原理是驱动类中会触发静态代码块，自动创建驱动自动注册。
        // 以下代码也可以省略，驱动管理器会根据地址自动创建驱动对象，自动注册。
        // Class.forName("com.mysql.jdbc.Driver");
        /**
         *  static {
         *         try {
         *             DriverManager.registerDriver(new Driver());
         *         } catch (SQLException var1) {
         *             throw new RuntimeException("Can't register driver!");
         *         }
         * }
         */

        //通过驱动管理器注册该驱动(驱动一旦创建出来会自动加载给驱动管理器，以下代码可以省略不写！)
        // 2.DriverManager.registerDriver(driver);

        // 3.通过驱动管理器去获取与具体数据库的连接通道：Connection对象。
        // 驱动管理器会根据请求的数据库地址自动加载驱动，注册驱动。
        Connection con =
                DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16"
                        , "root","root");

        System.out.printf("连接对象："+con);
    }
}
```
##Statement对象
```java
/**
    目标：通过JDBC的连接Connection获取一个发送sql语句的对象：Statement对象。

    Connection接口的方法:
        Statement createStatement()：创建一个发送SQL语句的对象。
    Statement接口的方法：
        1.boolean execute(String sql): 执行DDL操作的，发送SQL语句到数据库执行。有结果返回，返回true ,反之！！
    小结：
            记住API.
 */
public class StatementDemo {
    public static void main(String[] args) throws Exception {
        // 1.直接获取与数据库的连接 。
        // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
        Connection con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16",
                "root","root");

        // 2.获取一个发送SQL语句的对象：Statement
        Statement stm = con.createStatement();

        // 3.拼接一个SQL语句
        // 建表
        StringBuilder sb = new StringBuilder();
        sb.append("create table tb_user(")
        .append("id int primary key auto_increment,")
        .append("loginName varchar(23),")
        .append("userName varchar(23),")
        .append("passWord varchar(23))");

        // 4.通过Statement开始发送SQL语句
        System.out.println(stm.execute(sb.toString()));
        System.out.println("建表成功！");
    }
}
/**
 目标：通过JDBC的连接Connection获取一个发送sql语句的对象：Statement对象。

 Connection接口的方法:
 Statement createStatement()：创建一个发送SQL语句的对象。
 Statement接口的方法：
 1.boolean execute(String sql): 执行DDL操作的，发送SQL语句到数据库执行。有结果返回，返回true ,反之！！
 2.int executeUpdate(String sql)：执行DML操作，发送SQL语句到数据库执行，返回影响的行数。
 小结：
 记住API.
 */
public class StatementDemo {
    @Test
    public void insert()  {
        Connection con = null ;
        Statement stm = null;
        try{
            // 1.直接获取与数据库的连接 。
            // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
            con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16",
                    "root","root");

            // 2.获取一个发送SQL语句的对象：Statement
            stm = con.createStatement();

            // 3.拼接sql语句插入数据。
            StringBuilder sb = new StringBuilder();
            sb.append("insert into tb_user(loginName , userName , passWord , create_date)")
                    .append("values('swk','孙悟空','003197', NOW()),")
                    .append("('蜘蛛精','zzj','123456',NOW()),")
                    .append("('猪刚鬣','zgl','111111',NOW()),")
                    .append("('猪刚鬣','zgl','111111','2018-10-10 10:10:10')");

            // 4.通过Statement发送SQL语句到数据库执行,返回影响的行数！！
            int count = stm.executeUpdate(sb.toString());
            System.out.println("插入数据["+count+"]条成功！");
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            // 5.释放资源
            try{
                if(stm!=null)stm.close();
                if(con!=null)con.close();
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }

    @Test
    public void delete()  {
        Connection con = null ;
        Statement stm = null;
        try{
            // 1.直接获取与数据库的连接 。
            // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
            con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16",
                    "root","root");

            // 2.获取一个发送SQL语句的对象：Statement
            stm = con.createStatement();

            // 3.拼接sql语句插入数据。
            // 4.通过Statement发送SQL语句到数据库执行,返回影响的行数！！
            int count = stm.executeUpdate("delete from tb_user where id <= 5");
            System.out.println("删除数据["+count+"]条成功！");
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            // 5.释放资源
            try{
                if(stm!=null)stm.close();
                if(con!=null)con.close();
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }

    @Test
    public void update()  {
        Connection con = null ;
        Statement stm = null;
        try{
            // 1.直接获取与数据库的连接 。
            // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
            con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16",
                    "root","root");

            // 2.获取一个发送SQL语句的对象：Statement
            stm = con.createStatement();

            // 3.拼接sql语句插入数据。
            // 4.通过Statement发送SQL语句到数据库执行,返回影响的行数！！
            int count = stm.executeUpdate("update tb_user set loginName='齐天大圣孙悟空' where id = 8");
            System.out.println("修改数据["+count+"]条成功！");
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            // 5.释放资源
            try{
                if(stm!=null)stm.close();
                if(con!=null)con.close();
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }
}
```
##得到一个结果集对象ResultSet
```java
/**
    目标：JDBC执行查询数据操作：得到一个结果集对象ResultSet。

    ResultSet结果集对象的方法：
        boolean next():移动到下一行，有数据返回true ,没有数据返回false.
        Xxxx getXxxx("列名称")：根据列名称取值返回。
    小结：
        查询会返回一个结果集对象ResultSet，然后分析ResultSet处理数据！
 */
public class StatementDemo {
    @Test
    public void query(){
        Connection con = null ;
        Statement stm = null;
        try{
            // 1.直接获取与数据库的连接 。
            // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
            con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/day16",
                    "root","root");

            // 2.获取一个发送SQL语句的对象：Statement
            stm = con.createStatement();

            // 3.拼接查询的sql语句
            // 4.发送sql语句到数据库执行返回结果集对象
            ResultSet rs = stm.executeQuery("select * from tb_user");

            List<User> users = new ArrayList<>();
            // 5.解析结果集对象
            while (rs.next()){
                // 当前行是一个User对象，创建一个User对象封装每行数据！
                User user = new User();
                user.setId(rs.getInt("id"));
                user.setLoginName(rs.getString("loginName"));
                user.setPassWord(rs.getString("passWord"));
                user.setUserName(rs.getString("userName"));
                user.setCreateDate(rs.getTimestamp("create_date"));
                users.add(user); // 加入到集合中去
            }
            // 6.输出集合对象即可！
            System.out.println(users);

        } catch (Exception e){
            e.printStackTrace();
        } finally {
            // 5.释放资源
            try{
                if(stm!=null)stm.close();
                if(con!=null)con.close();
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }
}
public class User {
    private int id ;
    private String loginName ;
    private String userName ;
    private String passWord ;
    private Date createDate ;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getLoginName() {
        return loginName;
    }

    public void setLoginName(String loginName) {
        this.loginName = loginName;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassWord() {
        return passWord;
    }

    public void setPassWord(String passWord) {
        this.passWord = passWord;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", loginName='" + loginName + '\'' +
                ", userName='" + userName + '\'' +
                ", passWord='" + passWord + '\'' +
                ", createDate=" + createDate +
                '}'+"\n";
    }
}
```
##连接工厂
```java
/**
    目标：连接工厂。

    负责生产连接，负责关闭资源。

 */
public class ConnectionFactory {
    /**
     * 数据库的地址
     */
    public static final String SERVER_URL = "jdbc:mysql://127.0.0.1:3306/day16";
    /**
     * 数据库的用户名和密码
     */
    public static final String USER_NAME = "root";
    public static final String PASS_WORD = "root";

    /**
     * 对外返回一个连接对象：Connection对象
     * @return Connection对象。
     */
    public static Connection getConnection() throws Exception {
        // 1.直接获取与数据库的连接 。
        // 驱动管理器会根据地址自动加载MySQL的驱动，自动创建驱动对象，自动注册驱动对象到驱动管理器
        Connection con = DriverManager.getConnection(SERVER_URL,USER_NAME,PASS_WORD);
        return con;
    }

    /**
      * 资源的关闭操作。
     * @param rs
     * @param stm
     * @param con
     */
    public static void close(ResultSet rs , Statement stm , Connection con){
        try{
            if(rs != null) rs.close();
            if(stm != null) stm.close();
            if(con != null) con.close();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
/**
 目标：测试连接工厂的使用。
 */
public class Test01 {
    public static void main(String[] args) {
        Connection con = null ;
        Statement stm = null ;
        try{
            // 1.通过连接工厂获取连接对象
            con = ConnectionFactory.getConnection();
            // 2.获取一个发送SQL语句的对象：Statement
            stm = con.createStatement();
            // 3.拼接sql语句插入数据。
            StringBuilder sb = new StringBuilder();
            sb.append("insert into tb_user(loginName , userName , passWord , create_date)")
                    .append("values('swk','孙悟空','003197', NOW()),")
                    .append("('蜘蛛精','zzj','123456',NOW()),")
                    .append("('猪刚鬣','zgl','111111',NOW()),")
                    .append("('猪刚鬣','zgl','111111','2018-10-10 10:10:10')");

            // 4.通过Statement发送SQL语句到数据库执行,返回影响的行数！！
            int count = stm.executeUpdate(sb.toString());
            System.out.println("插入数据["+count+"]条成功！");
        }catch (Exception e){
            e.printStackTrace();
        } finally {
            ConnectionFactory.close(null , stm ,con);
        }
    }
}
```
##登录业务开发
```java
/**
    目标：登录业务开发。

    步骤：
        1.接收用户输入的登录名和密码 。
        2.拿着登录名和密码去查询是否存在该用户，
        3.存在登录成功，不存在提示“用户名或者密码错误了”！


 */
public class LoginDemo {
    public static void main(String[] args) {
        Connection con = null ;
        Statement stm = null ;
        ResultSet rs = null ;
        try{
            // 1.接收用户输入的登录名和密码 。
            Scanner sc = new Scanner(System.in);
            System.out.println("---------登录界面----------");
            System.out.print("用户名：");
            String loginName = sc.nextLine();
            System.out.print("密码：");
            String passWord = sc.nextLine();
            System.out.println("--------------------------");

            // 2.获取与数据库的连接对象
            con = ConnectionFactory.getConnection();
            // 3.获取发送SQL语句的Statement对象
            stm = con.createStatement();
            // 4.拼接登录的SQL脚本
            StringBuilder sb = new StringBuilder();
            sb.append("select * from tb_user where loginName ='")
                    .append(loginName).append("' and passWord='")
                    .append(passWord).append("'");
            System.out.println("调试SQL:"+sb);

            // 5.发送sql语句到数据库进行用户查询。
            rs = stm.executeQuery(sb.toString());

            if(rs.next()){
                // 登录成功了！
                // 取出该用户的名称
                String userName = rs.getString("userName");
                System.out.println("欢迎："+userName+"进入系统，请随便看！！");
            }else{
                // 登录失败了
                System.err.println("用户名或者密码错误了");
            }

        }catch (Exception e){
            e.printStackTrace();
        } finally {
            // 关闭资源
            ConnectionFactory.close(rs, stm,con);
        }
    }
}
/**
 目标：登录业务开发。

 引入：
 Statement对象发送SQL语句存在安全漏洞。
 著名的SQL注入就可以攻击Statement开发的业务。
 使用SQL注入强制性的登入系统：
 用户名：dsadsdsads
 密码：dasdas' or '1'='1
 --------------------------
 调试SQL: select * from tb_user where loginName ='dsadsdsads'
 and passWord='dasdas' or '1'='1'
 数据库会认为SQL后的'1'='1'是恒成立的，所以可以查询出用户导致登录成功了！
 结论： Statement对象发送SQL语句存在安全漏洞。

 Java建议使用预编译的发送SQL语句的对象：PreparedStatement对象。
 PreparedStatement是Statement对象的子类：
 PreparedStatement的优势：
 1.支持占位符，可以使用？代替参数。后续注入实际值。拼写简化了。代码优雅！
 2.预编译：提前把sql语句先发送给数据库预编译准备。性能好！！
 3.可以避免SQL注入，更加安全。
 4.可以在后续注入参数值。

 Connection类下：
 1.PreparedStatement prepareStatement(String sql)
 PreparedStatement注入参数的API:
 public void setString(int parameterIndex , String value)
 public void setInt(int parameterIndex , int value)
 public void setObject(int parameterIndex , Object value)
 小结：
 以后如果要发送SQL语句的对象建议用预编译对象：PreparedStatement
 */
public class LoginDemo {
    public static void main(String[] args) {
        Connection con = null ;
        PreparedStatement pstm = null ;
        ResultSet rs = null ;
        try{
            // 1.接收用户输入的登录名和密码 。
            Scanner sc = new Scanner(System.in);
            System.out.println("---------登录界面----------");
            System.out.print("用户名：");
            String loginName = sc.nextLine();
            System.out.print("密码：");
            String passWord = sc.nextLine();
            System.out.println("--------------------------");

            // 2.获取与数据库的连接对象
            con = ConnectionFactory.getConnection();
            // 3.获取发送SQL语句的prepareStatement对象，同时把SQL语句发送给数据库预编译。
            pstm = con.prepareStatement("select * from tb_user where loginName = ? and passWord = ?");
            // 4.注入参数值:
            // “1”代表第一个？注入用户输入的登录名称
            pstm.setString(1 , loginName);
            // “2”代表第二个？注入用户输入的密码
            pstm.setString(2 , passWord);
            // 5.通知数据库执行之前预编译好的SQL语句得到查询的结果集对象
            rs = pstm.executeQuery();

            if(rs.next()){
                // 登录成功了！
                // 取出该用户的名称
                String userName = rs.getString("userName");
                System.out.println("欢迎："+userName+"进入系统，请随便看！！");
            }else{
                // 登录失败了
                System.err.println("用户名或者密码错误了");
            }
        }catch (Exception e){
            e.printStackTrace();
        } finally {
            // 关闭资源
            ConnectionFactory.close(rs, pstm,con);
        }
    }
}
```
##PreparedStatement的CRUD操作
```java
/**
     目标：预编译对象 PreparedStatement的CRUD操作

     PreparedStatement注入参数的API:
         public void setString(int parameterIndex , String value)
         public void setInt(int parameterIndex , int value)
         public void setObject(int parameterIndex , Object value)
         执行API:
         int executeUpdate():
         ResultSet executeQuery():
 */
public class PreparedStatementDemo {
    @Test
    public void insert(){
        Connection con = null;
        PreparedStatement pstm = null ;
        try{
            // 1.得到数据库的连接
            con = ConnectionFactory.getConnection();
            // 2.准备sql语句。
            String sql = "insert into tb_user(userName,loginName,passWord,create_Date)"
                    + "values(?,?,?,?),(?,?,?,?),(?,?,?,?)";
            // 3.发送sql语句到数据库预编译，返回一个预编译的对象PrepareadStatement对象。
            pstm = con.prepareStatement(sql);
            // 4.给占位符“？”注入实际的参数值。
            pstm.setString(1,"王熙凤");
            pstm.setString(2,"hahaha");
            pstm.setString(3,"xixixi");
            pstm.setTimestamp(4, Timestamp.valueOf(LocalDateTime.now()));

            pstm.setString(5,"王熙凤1");
            pstm.setString(6,"hahaha1");
            pstm.setString(7,"xixixi1");
            pstm.setTimestamp(8, Timestamp.valueOf(LocalDateTime.now()));

            pstm.setString(9,"王熙凤2");
            pstm.setString(10,"hahaha2");
            pstm.setString(11,"xixixi2");
            pstm.setString(12, new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));

            // 5.通知数据库执行
            int count = pstm.executeUpdate();
            System.out.println("保存了"+count+"条数据！");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            ConnectionFactory.close(null , pstm,con);
        }
    }

    @Test
    public void delete(){
        Connection con = null;
        PreparedStatement pstm = null ;
        try{
            // 1.得到数据库的连接
            con = ConnectionFactory.getConnection();
            // 2.准备sql语句。
            // 3.发送sql语句到数据库预编译，返回一个预编译的对象PrepareadStatement对象。
            pstm = con.prepareStatement("delete from tb_user where id >= ?");
            // 4.给占位符“？”注入实际的参数值。
            pstm.setInt(1 , 8);
            // 5.通知数据库执行
            int count = pstm.executeUpdate();
            System.out.println("删除了"+count+"条数据！");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            ConnectionFactory.close(null , pstm,con);
        }
    }

    @Test
    public void update(){
        Connection con = null;
        PreparedStatement pstm = null ;
        try{
            // 1.得到数据库的连接
            con = ConnectionFactory.getConnection();
            // 2.准备sql语句。
            // 3.发送sql语句到数据库预编译，返回一个预编译的对象PrepareadStatement对象。
            pstm = con.prepareStatement("update tb_user set loginName = ? where id = ?");
            // 4.给占位符“？”注入实际的参数值。
            pstm.setString(1 , "天蓬元帅");
            pstm.setInt(2 , 7);
            // 5.通知数据库执行
            int count = pstm.executeUpdate();
            System.out.println("修改了"+count+"条数据！");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            ConnectionFactory.close(null , pstm,con);
        }
    }

    @Test
    public void query(){
        Connection con = null;
        PreparedStatement pstm = null ;
        ResultSet rs = null ;
        try{
            // 1.得到数据库的连接
            con = ConnectionFactory.getConnection();
            // 2.准备sql语句。
            // 3.发送sql语句到数据库预编译，返回一个预编译的对象PrepareadStatement对象。
            pstm = con.prepareStatement("select * from tb_user where id = ? ");
            // 4.给占位符“？”注入实际的参数值。
            pstm.setInt(1 , 7);
            // 5.通知数据库执行
            rs = pstm.executeQuery();
            // 6.if遍历一行结果，原因是查询的结果只有1条数据！
            if(rs.next()){
                // 当前行是一个User对象，创建一个User对象封装每行数据！
                User user = new User();
                user.setId(rs.getInt("id"));
                user.setLoginName(rs.getString("loginName"));
                user.setPassWord(rs.getString("passWord"));
                user.setUserName(rs.getString("userName"));
                user.setCreateDate(rs.getTimestamp("create_date"));
                System.out.println(user);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            ConnectionFactory.close(rs , pstm,con);
        }
    }
}
```
##JDBC的事物操作
```java
/**
    目标：JDBC的事物操作。

    事物：一批操作要么同时成功要么同时失败。
    典型业务：转账。

  Connection接口中与事务有关的方法
     void setAutoCommit(boolean autoCommit) 设置为自动或手动提交事务，false表示手动提交事务
     void commit()   提交事务
     void rollback()回滚事务
 */
public class TransactionDemo {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement pstm = null ;
        try{
            // 1.得到数据库的连接
            con = ConnectionFactory.getConnection();
            /** a.开启事物 */
            con.setAutoCommit(false);

            // 2.准备sql语句。
            // 3.发送sql语句到数据库预编译，返回一个预编译的对象PrepareadStatement对象。
            // 給jack 减去 500万
            pstm = con.prepareStatement("update account set balance = balance - ? where id = ?");
            pstm.setInt(1 , 5000000);
            pstm.setInt(2 , 1);
            int count = pstm.executeUpdate();
            System.out.println("减账："+count+"条！");

            // int c = 10 / 0 ;

            // 給Rose 加上 500万
            pstm = con.prepareStatement("update account set balance = balance + ? where id = ?");
            pstm.setInt(1 , 5000000);
            pstm.setInt(2 , 2);
            int count1 = pstm.executeUpdate();
            System.out.println("加账："+count1+"条！");
            // 到这儿那么转账认为无问题。提交操作
            /** 提交事物 */
            con.commit();
        }catch (Exception e){
            e.printStackTrace();
            try {
                /** 代码出了问题：回滚操作*/
                con.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }finally {
            ConnectionFactory.close(null , pstm,con);
        }
    }
}
```



# Sql优化

## 通用的优化方案

| 解释                                                  | 优化方案 |
| ----------------------------------------------------- | -------- |
| 表的设计合理化(符合3NF，有时候要进行反三范式操作)     | 设计优化 |
| 添加适当索引(index）(重点)                            | 索引优化 |
| 写出高质量的sql，避免索引失效 (重点)                  | Sql优化  |
| 分表技术(水平分割、垂直分割) 主从复制，读写分离       | 架构优化 |
| 对mysql配置优化 [配置最大并发数my.ini, 调整缓存大小 ] | 配置优化 |
| 服务器的硬件优化                                      | 硬件优化 |



## 索引

### mysql的索引实现

#### MyISAM索引(非聚簇)

MyISAM 索引文件和数据文件是分离的 

![Snipaste_2021-10-11_16-21-15](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_16-21-15.png)

#### InnoDB索引(聚簇)

```tex
数据文件本身就是索引文件 
表数据文件本身就是按照B+Tree组织的一个索引结构文件 
聚集索引-叶节点包含了完整的数据记录
```

![Snipaste_2021-10-11_16-21-22](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_16-21-22.png)

#### 联合索引

![Snipaste_2021-10-11_16-21-28](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_16-21-28.png)

### 索引的优劣势

- 优势

```tex
1.可以通过建立唯一索引或者主键索引,保证数据库表中每一行数据的唯一性. 
2.建立索引可以大大提高检索的数据,以及减少表的检索行数 
3.在表连接的连接条件 可以加速表与表直接的相连 
4.在分组和排序字句进行数据检索,可以减少查询时间中 分组 和 排序时所消耗的时间(数据库的记录会重新排序) 
5.建立索引,在查询中使用索引 可以提高性能 
```

- 劣势

```tex
1.在创建索引和维护索引 会耗费时间,随着数据量的增加而增加 
2.索引文件会占用物理空间,除了数据表需要占用物理空间之外,每一个索引还会占用一定的物理空间 
3.当对表的数据进行INSERT,UPDATE,DELETE 的时候,索引也要动态的维护,这样就会降低数据的维护速度,(建立索引会占用磁盘空间的索引文件。一般情况这个问题不太严重，但如果你在一个大表上创建了多种组合索引，索引文件的会膨胀很快)。
```

### 索引的分类

```tex
1.普通索引index :加速查找 
2.唯一索引 
主键索引：primary key ：加速查找+约束（不为空且唯一） 
唯一索引：unique：加速查找+约束 （唯一） 
3.联合索引（组合索引） 
-primary key(id,name):联合主键索引 
-unique(id,name):联合唯一索引 
-index(id,name):联合普通索引 
4.全文索引fulltext :用于搜索很长一篇文章的时候，效果最好。
```

### 索引的操作

```sql
-- create [UNIQUE|primary|fulltext] index 索引名称 ON 表名(字段(长度))
CREATE INDEX emp_name_index ON employee(NAME);
```

### 查看索引

```
show index from 表名
```

### 删除索引

```sql
drop index 索引名称 on 表名 
DROP INDEX emp_name_index ON employee;
```

### 更改索引

```sql
alter table tab_name add primary key(column_list) 
-- 添加一个主键,索引必须是唯一索引,不能为NULL 
alter table tab_name add unque index_name(column_list) 
-- 创建的索引是唯一索引,可以为NULL 
alter table tab_name add index index_name(column_list) 
-- 普通索引,索引值可出现多次 
alter table tab_name add fulltext index_name(column_list)
```

### 索引的选择

#### 适合建立索引

```tex
1.主键自动建立唯一索引:primary 
2.频繁作为查询条件的字段应该创建索引 比如银行系统银行帐号,电信系统的手机号 
3.查询中与其它表关联的字段,外键关系建立索引 比如员工表的部门外键 
4.频繁更新的字段不适合建立索引 每次更新不单单更新数据,还要更新索引 
5.where条件里用不到的字段不建立索引 
6.查询中排序的字段,排序的字段若通过索引去访问将大大提升排序速度索引能够提高检索的速度和排序的速度 
7.查询中统计或分组的字段
```

#### 不适合建立索引

```tex
记录比较少 
经常增删改的表 
索引提高了查询的速度，同时却会降低更新表的速度,因为建立索引后, 如果对表进行INSERT,UPDATE 和 
DELETE, MYSQL不仅要保存数据,还要保存一下索引文件 
数据重复的表字段 
如果某个数据列包含了许多重复的内容,为它建立索引 就没有太大在的实际效果，比如表中的某一个字段为 
国籍,性别，数据的差异率不高,这种建立索引就没有太多意义。 
```

## 性能优化

### 慢查询日志

```tex
mysql的慢查询日志是mysql提供的一种日志记录，它用来记录在mysql中响应时间超过阀值的语句,mysql 的日志是 跟踪mysql性能瓶颈的最快和最直接的方式了，系统性能出现瓶颈的时候，首先要打开慢查询日志，进行跟踪，尽快的 分析和排查出执行效率较慢的SQL ,及时解决避免造成不好的影响。 
作用： 记录具体执行效率较低的SQL语句的日志信息。 
注意： 在默认情况下mysql的慢查询日志记录是关闭的。 同时慢查询日志默认不记录管理语句和不使用索引进行查询的语句
```

#### 慢查询日志操作

- 临时更改

```sql
-- 查看慢查询是否开启
show variables like 'slow_query%';
-- 查看慢查询阈值
show variables like 'long_query_time';
-- 开启慢查询
set global slow_query_log='ON'; 
-- 设置慢查询日志存放位置
set global slow_query_log_file='/var/lib/mysql/test-10-226-slow.log';
-- 设置慢查询阈值,超过1秒记录日志
set global long_query_time=1;
```

- 永久更改

```sql
slow_query_log=1 
slow_query_log_file=地址
```

### 慢查询日志分析

慢查询日志中可能会出现很多的日志记录，我们可以通过慢查询日志工具进行分析，MySQL默认安装了 mysqldumpslow工具实现对慢查询日志信息的分析。 

```sql
-- 得到返回记录集最多的10个SQL。 
mysqldumpslow.pl -s r -t 10 C:\soft\DESKTOP-8GVEK4U-slow.log 
-- 得到访问次数最多的10个SQL 
mysqldumpslow.pl -s c -t 10 C:\soft\DESKTOP-8GVEK4U-slow.log 
-- 得到按照时间排序的前10条里面含有左连接的查询语句。 
mysqldumpslow.pl -s t -t 10 -g “left join” C:\soft\DESKTOP-8GVEK4U-slow.log 
-- 另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现刷屏的情况。 
mysqldumpslow.pl -s r -t 20 C:\soft\DESKTOP-8GVEK4U-slow.log 
Count: 4（执行了多少次） 
Time=375.01s（每次执行的时间） 
(1500s)（一共执行了多少时间） 
Lock=0.00s(0s)（等待锁的时间） 
Rows=10200.3（每次返回的记录数） (40801)（总共返回的记录 数）, 
username[password]@[10.194.172.41]
```

参数说明

```sql
-s 按照那种方式排序 
c：访问计数 
l：锁定时间 
r:返回记录
al：平均锁定时间 
ar：平均访问记录数 
at：平均查询时间 
-t 是top 
n的意思，返回多少条数据。 
-g 可以跟上正则匹配模式，大小写不敏感。
```

## Explain

### 概念

```tex
使用explain关键字,可以模拟优化器执行的SQL语句 
从而知道MYSQL是如何处理sql语句的 
通过Explain可以分析查询语句或表结构的性能瓶颈 
具体作用：
查看表的读取顺序 
数据读取操作的操作类型 
查看哪些索引可以使用 
查看哪些索引被实际使用 
查看表之间的引用 
查看每张表有多少行被优化器执行
```

### 使用方法

```sql
使用Explain关键字 放到sql语句前 
explain select cus_id from testemployee where cus_id > 10
```

### 参数详解

#### id

```tex
select查询的序列号，包含一组数字,表示查询中执行select子句或操作表的顺序 值分为三种情况 
- id值相同
执行顺序由上到下 
- id不同
如果是子查询,id的序号会递增,id值越大优先级越高,优先被执行 
- id相同不同,同时存在
可以认为是一组,从上往下顺序执行 
在所有组中,id值越大,优先级越高,越先执行
```

```sql
-- id值相同 
EXPLAIN SELECT * from employee e,department d,customer c where e.dep_id = d.id and e.cus_id = c.id;
```

![Snipaste_2021-10-11_19-27-29](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-27-29.png)

```sql
-- id值不同 
EXPLAIN SELECT * from department WHERE id = (SELECT id from employee WHERE id=(SELECT id from customer WHERE id = 1))
```

![Snipaste_2021-10-11_19-27-33](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-27-33.png)

```sql
-- id值相同 不同都存在 deriverd 衍生出来的虚表 
EXPLAIN select * from department d, (select * from employee group by dep_id) t where d.id = t.dep_id;
```

![Snipaste_2021-10-11_19-27-37](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-27-37.png)

#### select_type

```sql
查询类型,主要用于区别普通查询,联合查询,子查询等复杂查询 
结果值 
SIMPLE 
简单select查询,查询中不包含子查询或者UNION 
PRIMARY 
查询中若包含任何复杂的子查询,最外层查询则被标记为primary 
SUBQUERY
在select或where中包含了子查询 
DERIVED 
在from列表中包含的子查询被标记为derived(衍生)把结果放在临时表当中 
UNION 
若第二个select出现的union之后,则被标记为union 
若union包含在from子句的子查询中,外层select将被标记为deriver 
UNION RESULT 
从union表获取结果select,两个UNION合并的结果集在最后
```

```sql
-- union 和 union result 示例 
EXPLAIN select * from employee e LEFT JOIN department d on e.dep_id = d.id 
UNION 
select * from employee e RIGHT JOIN department D ON e.dep_id = d.id
```

![Snipaste_2021-10-11_19-30-58](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-30-58.png)

#### table

显示这一行的数据是关于哪张表的

#### partitions

如果查询是基于分区表的话, 会显示查询访问的分区

#### type (重要) 

```tex
访问类型排列 结果值:(最好到最差) 
system > const > eq_ref > range > index > ALL
```

```sql
-- system 表中有一行记录(系统表) 这是const类型的特例,平时不会出现 
explain select HOST from mysql.db where HOST='localhost'
```

![Snipaste_2021-10-11_19-32-46](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-32-46.png)

```sql
-- const 表示通过索引一次就找到了，const用于比较primary 或者 unique索引. 直接查询主键或者唯一索 引，因为只匹配一行数据,所以很快
EXPLAIN select id from testemployee where id=1000
```

![Snipaste_2021-10-11_19-34-07](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-34-07.png)

```sql
-- eq_ref 唯一性索引扫描 对于每个索引键,表中只有一条记录与之匹配, 常见于主键或唯一索引扫描 
EXPLAIN select * from employee e,department d where e.id=d.id
```

![Snipaste_2021-10-11_19-34-12](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-34-12.png)

```sql
-- ref 非唯一性索引扫描,返回匹配某个单独值的所有行,本质上也是一种索引访问,它返回所有匹配某个单独值的行 可能会找到多个符合条件的行,所以它应该属于查找和扫描的混合体 
EXPLAIN select e.id,e.dep_id,d.id from employee e,department d where e.dep_id = d.id
```

![Snipaste_2021-10-11_19-34-18](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-34-18.png)

```sql
-- range 只检索给定范围的行,使用一个索引来选择行 一般就是在你的where语句中出现between\<\>\ in等查 询,这种范围扫描索引比全表扫描要好,因为它只需要开始于索引的某一点.而结束语另一点,不用扫描全部索引 
explain select * from employee where id>2
```

![Snipaste_2021-10-11_19-34-30](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-34-30.png)

```sql
-- index index与All区别为index类型只遍历索引树,通常比All要快,因为索引文件通常比数据文件要小all和 index都是读全表,但index是从索引中读取,all是从硬盘当中读取 
explain select id from employee
```

<img src="https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-34-34.png" alt="Snipaste_2021-10-11_19-34-34" style="zoom:200%;" />

```sql
-- ALL 将全表进行扫描,从硬盘当中读取数据,如果出现了All 切数据量非常大, 一定要去做优化 
explain select * from employee
```

![Snipaste_2021-10-11_19-45-34](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/Snipaste_2021-10-11_19-45-34.png)

要求: 一般来说,保证查询至少达到range级别 最好能达到ref

#### possible_keys

显示可能应用在这张表中的索引,一个或者多个查询涉及到的字段上若存在索引,则该索引将被列出,但不一定被查询实际使用 可能自己创建了4个索引,在执行的时候,可能根据内部的自动判断,只使用了3个

```sql
可能不会用到索引，实际用到索引
explain select dep_id from employee
```

![image-20211011195440591](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195440591.png)

```sql
-- 可能会使用索引，实际没用到索引 
EXPLAIN select * from employee e,department d where e.dep_id = d.id
```

![image-20211011195504001](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195504001.png)

#### key （重要）

```
实际使用的索引,如果为NULL,则没有使用索引,查询中若使用了覆盖索引    ,则该索引仅出现在key列表 
possible_keys与key关系,理论应该用到哪些索引         实际用到了哪些索引 
覆盖索引    查询的字段和建立的字段刚好吻合,这种我们称为覆盖索引
```

#### key_len

```sql
-- 表示索引中使用的字节数,可通过该列计算查询中使用的索引长度 
explain select * from employee where dep_id=1 and name='鲁班' and age=10
```

![image-20211011195719572](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195719572.png)

#### ref

索引是否被引入到, 到底引用到了哪几个索引

```sql
Explain select * from employee e,department d where e.dep_id = d.id and e.cus_id = 1
```

![image-20211011195754360](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195754360.png)

```sql
Explain select e.dep_id from employee e,department d,customer c where e.dep_id = d.id and e.cus_id = c.id and e.name='鲁班'
```

![image-20211011195811217](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195811217.png)

#### rows

根据表统计信息及索引选用情况,大致估算出找到所需的记录所需要读取的行数,每长表有多少行被优化器查询过

#### filtered

```sql
-- 满足查询的记录数量的比例，注意是百分比，不是具体记录数    . 值越大越好，filtered列的值依赖统计信息，并 不十分准确 
Explain select e.dep_id from employee e,department d where e.dep_id = d.id
```

#### Extra （重要）

注意：语句中出现了Using Filesort 和    Using Temporary说明没有使用到索引

#### 产生的值:

```sql
/* 
Using filesort (需要优化) 
说明mysql会对数据使用一个外部的索引排序, 
而不是按照表内的索引顺序进行 
Mysql中无法利用索引完成排序操作称为"文件排序" 
*/ 
explain select * from employee where dep_id =1 ORDER BY cus_id
```

![image-20211011195947481](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011195947481.png)

```sql
/* 
Using temporary (需要优化) 
使用了临时表保存中间结果,Mysql在对查询结果排序时, 使用了临时表, 
常见于排序orderby 和分组查询group by 
*/ 
explain select name from employee where dep_id in (1,2,3) GROUP BY cus_id
```

![image-20211011200003772](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/sql/image-20211011200003772.png)

```sql
/* 
impossible where (需要优化) 
where 子句的值总是false 不能用来获取任何元组 
*/ 
explain select name from employee where name='鲁班' and name='zs'
```

```sql
use index 
表示相应的select中使用了覆盖索引,避免访问了表的数据行, 效率很好 如果同时出现using where  表明索引被用来执行索引键值的查找 
如果没有同时出现using where 表明索引    用来读取数据而非执行查找动作 示例 
using where 
表明使用了where过滤 
using join buffer 
使用了连接缓存
```

## 避免索引失效

### 全职匹配

```sql
-- 创建组合索引 
create index idx_name_dep_age on employee(name,dep_id,age) 
-- 索引字段全部使用上 
explain select * from employee where name='鲁班' and dep_id=1 and age=10
```

### 最左匹配原则

```sql
-- 去掉name条件    索引全部失效 
explain select * from employee where dep_id=1 and age=10
```

```sql
-- 去掉dep_id   name索引生效 
explain select * from employee where name='鲁班' and age=10
```

```sql
-- 顺序错乱不会影响最左匹配 
explain select * from employee where dep_id=1 and age=10 and name='鲁班'
```

### 不再索引列上做任何操作

```sql
-- 在name字段上    加上去除空格的函数    索引失效 
explain select * from employee where TRIM(name)='鲁班' and dep_id=1 and age=10
```

### 范围条件右边的索引失效

```sql
-- 范围查找    会造成该组合索引字段的右侧索引全部失效 
explain select * from employee where name = '鲁班' and dep_id>1 and age=10
```

### mysql在使用不等于(!=或者<>)索引失效

```sql
explain select * from employee where age != 10
```

### is null,is not null无法使用索引

```sql
explain select * from employee where name is not NULL
```

### like以通配符开头(%qw)索引失效

```sql
explain select * from employee where name like '%鲁'
```

### 字符串不加引号索引失效

```sql
explain select * from employee where name = 200
```

### 使用or连接索引失效

```sql
explain select * from employee where name = '鲁班' or age>10
```

### 尽量使用覆盖索引

```sql
-- 覆盖索引: 要查询的字段全部是索引字段 
-- 上面情况会触发全表扫描，不过若使用了覆盖索引，则会只扫描索引文件 
explain select name,dep_id,age from employee where name = '鲁班' or age>10
```

## 排序与分组优化

### 使用order by出现Using ﬁlesort

```sql
-- 没有使用索引排序，而是内部创建新文件进行文件排序，所以需要优化 
-- 如果select语句未使用到索引，会出现    filesort 
explain select * from employee  order by name,dep_id,age 
-- 组合索引缺少索引字段    会出现    filesort 
explain select * from employee where name='鲁班' order by dep_id,age explain select * from employee  order by dep_id,age 
-- 组合索引顺序不一致(order by的后面) 会出现    filesort 
explain select * from employee where name='鲁班' order by dep_id,age 
explain select * from employee where name='鲁班' order by age,dep_id 
-- 当索引出现范围查找时    可能会出现    filesort 
explain select * from employee where name='鲁班' and dep_id>1 order by age -- 排序使用一升一降会造成filesort 
explain select * from employee where name='鲁班' order by dep_id desc,age
```

### 使用group by出现Using temporary

```
-- 同order by情况类似，    分组必定触发排序
```

### 大数据量分页优化

```sql
-- 分页是我们经常使用的功能，在数据量少时单纯的使用limit m,n 不会感觉到性能的影响 
-- 但我们的数据达到成百上千万时    ，    就会明显查询速度越来越低
-- 使用存储过程导入数据
-- 查看是否开启函数功能 
show variables like 'log_bin_trust_function_creators'; 
-- 设置开启函数功能 
set global log_bin_trust_function_creators=1; 
-- 创建函数用于生成随机字符串 
delimiter $$ 
create function rand_string(n int) returns varchar(255) 
begin 
declare chars_str varchar(100) default 'qwertyuiopasdfghjklzxcvbnm'; declare return_str varchar(255) default ''; 
declare i int default 0; 
while i<n do 
set return_str=concat(return_str,substring(chars_str,floor(1+rand()*52),1)); set i=i+1;
end while; 
return return_str; 
end $$ 
-- 创建存储过程用于插入数据 
delimiter $$ 
create procedure insert_emp(in start int(10),in max_num int(10)) begin 
declare i int default 0; 
/*把autocommit设置成0*/ 
set autocommit= 0; 
repeat 
set i=i+1; 
insert into testemployee(name,dep_id,age,salary,cus_id) 
values(rand_string(6),'2',24,3000,6); 
until i=max_num end repeat; 
commit; 
end $$ 
-- 调用存储过程插入数据 
call insert_emp(1,1000000);
-- 测试一下分页数据的相应时间
-- limit 0,20 时间: 0.001ms 
select * from testemployee limit 0,20 
-- limit 10000,20 时间: 0.004ms 
select * from testemployee limit 10000,20 -- limit 100000,20 时间: 0.044ms 
select * from testemployee limit 100000,20 -- limit 1000000,20 时间:  0.370ms 
select * from testemployee limit 1000000,20 -- limit 3000000,20 时间: 1.068ms 
select * from testemployee limit 3000000,20
```

### 子查询优化

```sql
-- 子查询优化 
-- 通过Explain发现，之前我们没有利用到索引，这次我们利用索引查询出对应的所有ID 
-- 在通过关联查询，查询出对应的全部数据，性能有了明显提升 
-- limit 3000000,20 时间: 1.068ms -> 时间: 0.742ms 
select * from testemployee e,(select id from testemployee limit 3000000,20) tmp where e.id=tmp.id 
-- 自增ID也可以用如下方式 
select * from testemployee where id> (select id from testemployee t limit 3000000,1) LIMIT 10
```

### 使用id限定方案

```sql
-- 使用id限定方案，将上一页的ID传递过来    根据id范围进行分页查询 
-- 通过程序的设计，持续保留上一页的ID，并且ID保证自增 
-- 时间: 0.010ms 
select * from testemployee where id>3000109 limit 20 
-- 虽然使用条件有些苛刻 但效率非常高，可以和方案一组合使用 ，跳转某页使用方案一 下一页使用方案2
```

## 小表驱动大表

### 表关联查询

MySQL 表关联的算法是    Nest Loop Join，是通过驱动表的结果集作为循环基础数据，然后一条一条地通过该结果集 中的数据作为过滤条件到下一个表中查询数据，然后合并结果。如果小的循环在外层，对于数据库连接来说就只连接5 次，进行5000次操作，如果1000在外，则需要进行1000次数据库连接，从而浪费资源，增加消耗。这就是为什么要小 表驱动大表。

==注意==:多表查询中，一定要让小表驱动大表

### in和exits查询

```sql
-- 使用in 时间: 3.292ms 
explain select * from testemployee where dep_id in (select id from department) 使用department表中数据作为外层循环    10次 
for( select id from department d) 
每次循环执行employee表中的查询 
for( select * from employee e where e.dep_id=d.id)
-- 使用exits 时间: 14.771ms 
select * from employee e where exists (select 1 from department d where d.id = e.dep_id) 
使用employee表中数据作为外层循环    3000000万次 
for(select * from employee e) 
每次循环执行department表中的查询 
for( select 1 from department d where d.id = e.dep_id)
```

总结:

```
当A表数据多于B表中的数据时，这是我们使用in优于Exists 
当B表数据多于A表中的数据时,这时我们使用Exists优于in 
如果数据量差不多，那么它们的执行性能差不多 
Exists子查询只返回true或false,因此子查询中的select * 可以是select 1或其它
```

### max函数优化

```sql
-- 给max函数中的字段添加索引 
select max(age) from testemployee
```

