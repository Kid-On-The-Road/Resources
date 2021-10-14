# 镜像相关命令
## 查看镜像

```markdown
docker images
```
+ REPOSITORY: 镜像名称
+ TAG: 镜像标签 (默认是可以省略的,也就是latest)
+ IMAGE ID: 镜像ID
+ CREATED: 镜像的创建日期（不是获取该镜像的日期）
+ SIZE: 镜像大小
## 搜索镜像

```markdown
docker search centos7
```
+ NAME: 镜像名称
+ DESCRIPTION: 镜像描述
+ STARS: 用户评价，反应一个镜像的受欢迎程度
+ OFFICIAL: 是否官方
+ AUTOMATED: 自动构建，表示该镜像由Docker Hub自动构建流程创建的
+ 搜索网站[http://hub.docker.com]
## 拉取镜像

```markdown
拉取特定镜像
docker pull centos/mysql-57-centos7
docker pull tomcat
docker pull nginx
docker pull redis
docker pull rabbitmq:management
拉取最后版本镜像
docker pull centos:latest
```
## 删除镜像

```markdown
根据镜像ID删除
docker rmi 镜像ID
根据镜像名称删除镜像
docker rmi mynginx
删除所有镜像
docker rmi $(docker images | awk '{print $3}' |tail -n +2)
docker rmi $(docker images -q)
```
# 容器相关命令
## 创建容器

+ -e 代表添加环境变量 MYSQL_ROOT_PASSWORD 是root用户的远程登陆密码（如果是在容器中使用root登录的话,那么其**密码为空**）
+ -i: 表示运行容器
+ -t: 表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。
+ --name: 为创建的容器命名。
+ -v: 表示目录映射关系（前者是宿主机目录，后者是容器的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。
+ -d: 在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。
+ -p: 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射。
```markdown
docker run -di --name=mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql
docker run -di --name=mytomcat -p 9000:8080 -v /usr/local/tomcat/webapps:/usr/local/tomcat/webapps tomcat
docker run -di --name=mynginx -p 80:80 nginx
docker run -di --name=myredis -p 6379:6379 redis
docker run -di --name=myrabbitmq  -p 5672:5672 -p 15672:15672 rabbitmq:management
```
## 查看容器

```markdown
+ 查看正在运行容器: docker ps
+ 查看所有容器: docker ps -a
+ 查看最后一次运行的容器: docker ps –l
```
## 进入容器

```markdown
docker exec -it mysql5.7 /bin/bash 
```
## 交互式容器

```markdown
先拉取一个镜像；这一步不是每次启动容器都要做的，而是因为前面我们删除了镜像，
无镜像可用所以才再拉取一个
docker pull centos:7
创建并启动名称为 mycentos7 的交互式容器
容器名称 mycentos7
镜像名称:TAG (centos:7)  也可以使用镜像id (5e35e350aded)
/bin/bash: 进入容器命令行
docker run -it --name=mycentos7 centos:7 /bin/bash
```
## 守护式容器

```markdown
创建并启动守护式容器
容器名称: mycentos2
镜像名称:TAG (centos:7)  也可以使用镜像id (5e35e350aded)
docker run -di --name=mycentos2 centos:7
进入容器：
docker exec -it container_name (或者 container_id) /bin/bash
exit退出时，容器不会停止
docker exec -it mycentos2 /bin/bash
```
## 停止或启动容器

```markdown
停止正在运行的容器: docker stop 容器名称|容器ID
docker stop mycentos2
启动已运行过的容器: docker start 容器名称|容器ID
docker start mycentos2
```
## 重启容器

```markdown
~重启正在运行的容器: docker restart 容器名称|容器ID~
docker restart mycentos2
```
## 查看容器ip

```markdown
在linux宿主机下查看 mycentos2 的ip
docker inspect 容器名称（容器ID）
docker inspect mycentos2
```
## 启动所有容器

```markdown
docker start $(docker ps -a -q)
```
## 关闭所有容器

```markdown
docker stop $(docker ps -a -q)
```
## 删除容器

```markdown
docker rm mycentos2
或者
docker rm 2095a22bee70

删除所有容器
docker rm $(docker ps -a -q) 
```
## 容器拷贝文件

```markdown
docker cp 需要拷贝的文件或目录 容器名称:容器目录
创建一个文件abc.txt 
touch abc.txt
复制 abc.txt 到 mycentos2 的容器的 / 目录下 
docker cp abc.txt mycentos2:/
进入mycentos2容器 
docker exec -it mycentos2 /bin/bash
查看容器 / 目录下文件
ll
docker cp 容器名称:容器目录 需要拷贝的文件或目录
进入容器后创建文件aaa.txt
touch aaa.txt
退出容器
exit
在Linux宿主机器执行复制；将容器mycentos2的/aaa.txt文件复制到 宿主机器的/root目录下
docker cp mycentos2:/aaa.txt /root
```
##容器目录挂载
```markdown
创建linux宿主机器要挂载的目录 
mkdir /usr/local/test
创建并启动容器mycentos3
并挂载 linux中的/usr/local/test目录到容器的/usr/local/test
也就是在 linux中的/usr/local/test中操作相当于对容器相应目录操作 
docker run -di -v /usr/local/test:/usr/local/test --name=mycentos3 centos:7
在linux下创建文件 
touch /usr/local/test/bbb.txt
进入容器 
docker exec -it mycentos3 /bin/bash
在容器中查看目录中是否有对应文件bbb.txt
cd /usr/local/test
ll
```
## 容器备份与迁移
### 将容器保存为镜像

```markdown
docker commit mynginx mynginx
```
### 保存镜像为文件

```markdown
docker save -o mynginx.tar mynginx
```
### 恢复镜像

```markdown
docker load -i mynginx.tar
```
# docker中MySQL相关命令
## 登陆容器中的mysql

```sql
mysql -u root -p
```
## 远程登陆

+ 查看ip；如果以后要内部连接该mysql，如其他容器中要连接mysql容器的mysql的时候，可以使用如下命令查看IP
```markdown
docker inspect mysql5.7
```
## 修改密码
### 授权

```sql
GRANT ALL ON *.* TO 'root'@'%';
```
### 刷新权限

```sql
flush privileges;
```
### 更新加密规则

```sql
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'root' PASSWORD EXPIRE NEVER;
```
### 更新root用户密码

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
```
### 刷新权限

```sql
flush privileges;
```
# docker中centos7相关命令
## firewall(防火墙)命令

```markdown
安装
yum install firewalld
启动
systemctl start firewalld
关闭
systemctl stop firewalld
查看状态
systemctl status firewalld
开机禁用
systemctl disable firewalld
开机启动
systemctl enable firewalld
查看开放端口
firewall-cmd --list-ports
添加端口
firewall-cmd --add-port=22/tcp --permanent （–permanent永久生效，没有此参数重启后失效）
```
## 创建容器
Docker的设计理念是在容器里面不运行后台服务，容器本身就是宿主机上的一个独立的主进程，也可以间接的理解为就是容器里运行服务的应用进程。一个容器的生命周期是围绕这个主进程存在的，所以正确的使用容器方法是将里面的服务运行在前台。
再说到systemd，这个套件已经成为主流Linux发行版（比如CentOS7、Ubuntu14+）默认的服务管理，取代了传统的SystemV风格服务管理。systemd维护系统服务程序，它需要特权去会访问Linux内核。而容器并不是一个完整的操作系统，只有一个文件系统，而且默认启动只是普通用户这样的权限访问Linux内核，也就是没有特权，所以自然就用不了！
因此，请遵守容器设计原则，一个容器里运行一个前台服务！

```markdown
以特权模式运行容器
docker run --privileged -d  --name=mycentos7 mycentos /usr/sbin/init
```
## centos设置相关命令
### 密码相关

```markdown
进入容器
docker exec -it mycentos7 /bin/bash
清除密码
passwd -d root
设置密码
passwd root
```
### ssh相关

```markdown
查看是否安装了ssh-server服务
yum list installed | grep openssh-server
安装ssh-client
yum install openssh-clients
安装ssh-server
yum install openssh-server
找到 /etc/ssh/ 目录下的sshd服务配置文件 sshd_config，用Vim编辑器打开
vi sshd_config
去掉端口、监听端口、监听地址前的#号
Port 22
#Add ressfamlly any
Listenadd ress 0.0.0.0
Listenadd ress
开启允许远程登录
Permltrootlogin yes
开启使用用户名密码来作为连接验证
Pubkeyauthentication yes
检查是否开启了ssh服务
ps -e | grep sshd 
检查 22 号端口是否开启监听 
netstat -an | grep 22
开启ssh服务
service sshd start 或 systemctl start sshd.service 
安装service
yum install initscripts -y
查看service版本
yum list | grep initscripts
启动sshd服务命令 
systemctl start sshd.service
重启 sshd服务命令 
systemctl restart sshd.service
设置服务开启自启命令 
systemctl enable sshd.service
```
## 提交容器镜像

```markdown
docker commit mycentos7 mycentos
```
## 将新的镜像启动，并将docker服务器的22端口映射到容器的22端口上

```markdown
docker run --privileged -d -p 50001:22 --name=mycentos7 mycentos /usr/sbin/init
```
# centos7中安装docker相关命令
## 安装docker

```markdown
更新yum
yum update
安装需要的软件包，yum-util提供yum-config-manager功能，另外两个是devicemapper驱动依赖
yum install -y yum-utils device-mapper-persistent-data lvm2
设置yum源为阿里云
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
安装docker【docker-ce: 社区版，免费；docker-ee：企业版，收费】
yum install docker-ce -y
安装后查看docker版本
docker -v
```
## 设置ustc镜像源

```markdown
编辑文件/etc/docker/daemon.json
mkdir /etc/docker
vi /etc/docker/daemon.json  
在文件中加入下面内容
{
"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```
## 服务相关

```markdown
启动docker服务
systemctl start docker
停止docker服务
systemctl stop docker
重启docker服务
systemctl restart docker
查看docker服务状态
systemctl status docker
设置开机启动docker服务
systemctl enable docker
查看docker概要信息
docker info
查看docker帮助文档
docker --help
```
# Dockerfile构建镜像
## 构建镜像

```markdown
创建目录 
mkdir -p /usr/local/dockerjdk8 
cd /usr/local/dockerjdk8
下载jdk-8u171-linux-x64.tar.gz并上传到服务器（虚拟机）中的/usr/local/dockerjdk8目录
在/usr/local/dockerjdk8目录下创建Dockerfile文件，文件内容如下:
vi Dockerfile
FROM centos:7
MAINTAINER ITCAST
WORKDIR /usr
RUN mkdir /usr/local/java
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH
执行命令构建镜像；
docker build -t='jdk1.8'.
查看镜像是否建立完成 
docker images
```
## 创建容器

```markdown
创建并启动容器 
docker run -it --name=testjdk jdk1.8 /bin/bash
在容器中测试jdk是否已经安装 
java -version
```
# registry私服仓库
## 创建容器

```markdown
拉取私有仓库镜像 
docker pull registry
启动私有仓库容器
docker run -di --name=registry -p 5000:5000 registry
打开浏览器 输入地址http://宿主机ip:5000/v2/_catalog，看到{"repositories":[]} 表示私有仓库 搭建成功
修改daemon.json 
vi /etc/docker/daemon.json
在上述文件中添加一个key，保存退出。此步用于让 docker 信任私有仓库地址；注意将宿主机ip修改为自己宿主 机真实ip 
{"insecure-registries":["宿主机ip:5000"]}
重启docker 服务 
systemctl restart docker
docker start registry
```
## 将镜像上传至私有仓库

```markdown
标记镜像为私有仓库的镜像 
语法: docker tag jdk1.8 宿主机IP:5000/jdk1.8
docker tag jdk1.8 192.168.12.132:5000/jdk1.8
再次启动私有仓库容器 
docker restart registry
上传标记的镜像到私有仓库
语法: docker push 宿主机IP:5000/jdk1.8 
docker push 192.168.12.132:5000/jdk1.8
输入网址查看仓库效果
```
## 从私有仓库拉取镜像

```markdown
因为私有仓库所在的服务器上已经存在相关镜像；所以先删除；请指定镜像名，不是id 
语法: docker rmi 服务器ip:5000/jdk1.8
docker rmi 192.168.12.132:5000/jdk1.8
拉取镜像 
语法: docker pull 服务器ip:5000/jdk1.8
docker pull 192.168.12.132:5000/jdk1.8
可以通过如下命令查看 docker 的信息；了解到私有仓库地址 
docker info
```

























