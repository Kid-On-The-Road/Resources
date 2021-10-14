## 01、虚拟化介绍

> 目标: 了解虚拟化及虚拟化种类。

#### 1.1 什么是虚拟化

+ 在计算机中，虚拟化（英语：Virtualization）是一种资源管理技术，是将计算机的各种实体资源，如服务器、网络、内存及存储等，予以抽象、转换后呈现出来，打破实体结构间的不可切割的障碍，使用户可以比原本的组态更好的方式来应用这些资源。这些资源的新虚拟部分是不受现有资源的架设方式，地域或物理组态所限制。一般所指的虚拟化资源包括计算能力和资料存储。
+ 在实际的生产环境中，虚拟化技术主要用来解决高性能的物理硬件产能过剩和老的旧的硬件产能过低的重组重用，透明化底层物理硬件，**从而最大化的利用物理硬件对资源充分利用**
+ 虚拟化技术种类很多，例如：软件虚拟化、硬件虚拟化、内存虚拟化、网络虚拟化、桌面虚拟化、服务虚拟化、虚拟机等等。 

#### 1.2 虚拟化种类

+ 全虚拟化架构

  虚拟机的监视器（hypervisor）是类似于用户的应用程序运行在主机的OS之上，如VMware的workstation，这种虚拟化产品提供了虚拟的硬件)。

  ![1574128224856](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574128224856.png) 

  > 说明: 虚拟出来的操作系统可以与本机操作系统**不一样**(内核)。

+ OS层虚拟化架构

  ![1574128530943](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574128530943.png)  

  > 说明: 虚拟出来的操作系统与本机操作系统**一样**(内核)。

+ 硬件层虚拟化

  ![1574129008763](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574129008763.png)  

  **硬件层的虚拟化具有高性能和隔离性**，因为hypervisor直接在硬件上运行，有利于控制VM的OS访问硬件资源，使用这种解决方案的产品有VMware ESXi 和 Xen server

  Hypervisor是一种运行在物理服务器和操作系统之间的中间软件层,可允许多个操作系统和应用共享一套基础物理硬件，因此也可以看作是虚拟环境中的“元”操作系统，它可以协调访问服务器上的所有物理设备和虚拟机，也叫虚拟机监视器（Virtual Machine Monitor，VMM）。

  Hypervisor是所有虚拟化技术的核心。当服务器启动并执行Hypervisor时，它会给每一台虚拟机分配适量的内存、CPU、网络和磁盘，并加载所有虚拟机的客户操作系统。

  Hypervisor是所有虚拟化技术的核心，软硬件架构和管理更高效、更灵活，硬件的效能能够更好地发挥出来。常见的产品有：VMware、KVM、Xen等等。

  > 说明: 虚拟出来的操作系统与本机操作系统**不一样**(内核)。但**没有宿主机**操作系统。

**小结**

> 虚拟化:（英语: Virtualization）是一种资源管理技术，是将计算机的各种实体资源，如服务器、网络、内存及存储等，予以抽象、转换后呈现出来，打破实体结构间的不可切割的障碍，使用户可以比原本的组态更好的方式来应用这些资源。
>
> 虚拟化技术: 软件虚拟化、硬件虚拟化、内存虚拟化、网络虚拟化(vip)、桌面虚拟化、服务虚拟化、虚拟机。

## 02、什么是docker

> 目标: 了解什么是docker?

#### 2.1 docker简介 

![1556243001933](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1556243001933.png)

+ Docker 是一个开源的**应用容器引擎**，基于 Go 语言开发。Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。 

+ Docker 应用场景

  - Web 应用的自动化打包和发布 
  - 自动化测试和持续集成、发布 
  - 在服务型环境中部署和调整数据库或其他的后台应用

+ 使用Docker可以实现开发人员的开发环境、测试人员的测试环境、运维人员的生产环境的一致性。

  ![1574126073144](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574126073144.png) 

  > Docker借鉴了标准集装箱的概念。标准集装箱将货物运往世界各地，Docker将这个模型运用到自己的设计中，唯一不同的是：集装箱运输货物，而Docker运输软件。 

#### 2.2 docker特点

+ **上手快**
  + 用户只需要几分钟，就可以把自己的程序“Docker化”。Docker依赖于“写时复制”（copy-on-write）模型，使修改应用程序也非常迅速，可以说达到“随心所致，代码即改”的境界。
  + 随后，就可以创建容器来运行应用程序了。大多数Docker容器只需要不到1秒中即可启动。由于去除了管理程序的开销，Docker容器拥有很高的性能，同时同一台宿主机中也可以运行更多的容器，使用户尽可能的充分利用系统资源。
+ **职责明确**
  + 使用Docker，开发人员只需要关心容器中运行的应用程序，而运维人员只需要关心如何管理容器。Docker设计的目的就是要加强开发人员写代码的开发环境与应用程序要部署的生产环境一致性。从而降低那种“开发时一切正常，肯定是运维的问题（测试环境都是正常的，上线后出了问题就归结为肯定是运维的问题）。
+ **快速高效的开发生命周期**
  + Docker的目标之一就是缩短代码从开发、测试到部署、上线运行的周期，让你的应用程序具备可移植性，易于构建，并易于协作。（通俗一点说，Docker就像一个盒子，里面可以装很多物件，如果需要这些物件的可以直接将该大盒子拿走，而不需要从该盒子中一件件的取）。
+ **鼓励使用面向服务的架构**
  + Docker还鼓励面向服务的体系结构和微服务架构。Docker推荐单个容器只运行一个应用程序或进程，这样就形成了一个分布式的应用程序模型，在这种模型下，应用程序或者服务都可以表示为一系列内部互联的容器，从而使分布式部署应用程序，扩展或调试应用程序都变得非常简单，同时也提高了程序的内省性。（当然，可以在一个容器中运行多个应用程序）。

**小结**

> Docker是一个开源应用容器引擎,使用go语言开发，适用于企业**应用部署解决方案**。



## 03、容器&虚拟机对比

> 目标: 说出Docker容器与虚拟机相比的优势。

#### 3.1 本质上的区别

![1574129620520](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574129620520.png)  

+ **VMs虚拟服务:**
  + Server基础设施服务，它可以是你的个人电脑，数据中心的服务器或者云主机。
  + Host OS当前的操作系统，比如windows和linux系统等。
  + hypervisor: 虚拟机监视器是一种虚拟化技术。可以在主操作系统之上运行多个不同的操作系统。比如vmware和virturebox等。
  + Guest OS就是虚拟子系统也就是我们的centos。
  + Bins、Libs安装应用需要依赖的组件和环境。比如gcc,gcc++或者yum等。
  + App 安装我们对应的应用，比如: mysql、tomcat、jdk等。应用安装之后，就可以在各个操作系统分别运行应用了，这样各个应用就是相互隔离的。
+ **Docker容器：**
  + Server基础设施服务，它可以是你的个人电脑，数据中心的服务器或者云主机。
  + Host OS当前的操作系统，比如windows和linux系统等。
  + Docker Engine: 负责和底层的系统进行交互和共享底层系统的资源。取代了Hypevisor.它是运行在操作系统之上的后台进程，负责管理Docker容器。
  + 各种依赖，对于Docker，应用的所有依赖都打包在Docker镜像中，Docker容器是基于Docker镜像创建的。
  + App应用，应用的源代码与他的依赖都打包在Docker镜像中，不同的应用需要不同的Docker镜像，不同的应用运行在不同的Docker容器中，它们是相互隔离的。

#### 3.2 使用上的区别

![1574072004851](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574072004851.png) 

|                  | **虚拟机**                     | **容器**               |
| ---------------- | ------------------------------ | ---------------------- |
| **占用磁盘空间** | 非常大，GB级                   | 小，MB甚至KB级         |
| **启动速度**     | 慢，分钟级                     | 快，秒级               |
| **运行形态**     | 运行于Hypervisor上             | 直接运行在宿主机内核上 |
| **并发性**       | 一台宿主机上十几个，最多几十个 | 上百个，甚至数百上千个 |
| **性能**         | 逊于宿主机                     | 接近宿主机本地进程     |
| **资源利用率**   | 低                             | 高                     |

**小结**

> Docker容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统虚拟机则是在硬件层面实现虚拟化。与传统的虚拟机相比，Docker优势体现为启动速度快、占用体积小。 

## 04、Docker：组成结构

> 目标: 了解Docker的组成结构

![1574129782413](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574129782413.png) 

|              名称               | 说明                                                         |
| :-----------------------------: | :----------------------------------------------------------- |
|      Docker 镜像 (Images)       | Docker 镜像是用于创建 Docker 容器的模板。镜像是基于联合文件系统的一种层式结构， 由一系列指令一步一步构建出来(只读不能修改)。 |
|     Docker 容器 (Container)     | 容器是独立运行的一个或一组应用。镜像相当于类，容器相当于类的实例 |
|      Docker 客户端(Client)      | Docker 客户端通过命令行或者其他工具使用 Docker API 与 Docker 的守护进程通信。 |
|        Docker 主机(Host)        | 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。       |
|         Docker守护进程          | 是Docker服务器端进程，负责支撑Docker 容器的运行以及镜像的管理。 |
| Docker 仓库 DockerHub(Registry) | Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。 Docker Hub提供了庞大 的镜像集合供使用。用户也可以将自己本地的镜像推送到Docker仓库供其他人下载。 |

**小结**

> Docker组成部分: 镜像、容器、客户端、主机、守护进程、仓库。



## 05、Docker：仓库介绍

> 目标: 了解什么是仓库？

+ Docker用Registry来保存用户构建的镜像。Registry分为**公共**和**私有**两种。Docker公司运营公共的Registry叫做Docker Hub。用户可以在Docker Hub注册账号，分享并保存自己的镜像（说明：在Docker Hub下载镜像巨慢，可以自己构建私有的Registry）。

+ 官方仓库: <https://hub.docker.com/>

  ![1572835410624](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1572835410624.png) 

## 06、Docker：安装

> 目标: 掌握Docker的安装

+ Docker可以运行在MAC、Windows、CentOS、DEBIAN、UBUNTU等操作系统上，提供社区版和企业版，本课程基于CentOS安装Docker。 

  > 注意：这里**建议安装在CentOS7.x以上的版本**，在CentOS6.x的版本中，安装前需要安装其他很多的环境而且Docker很多补丁不支持更新。 root 123456

+ `资料`已经准备了安装好的镜像，直接挂载即可，挂载后，使用ifconfig命令查看本地ip:

  ![1574132472149](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574132472149.png) 

+ 以下是在CentOS7中安装Docker的步骤:

  ```shell
  # 1. yum 包更新到最新
  yum update
  
  # 2. 安装需要的软件包，yum-util提供yum-config-manager功能，另外两个是devicemapper驱动依赖
  yum install -y yum-utils device-mapper-persistent-data lvm2
  
  # 3. 设置yum源为阿里云
  yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  
  # 4. 安装docker【docker-ce: 社区版，免费；docker-ee：企业版，收费】
  yum install docker-ce -y
  
  # 5. 安装后查看docker版本
  docker -v
  ```

  ![1574133798713](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574133798713.png) 

## 07、Docker：设置ustc镜像源

> 目标: 掌握镜像源的设置

ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.0.4版本的时候就在用。ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是不需要注册，是真正的公共服务。 

访问地址: <https://lug.ustc.edu.cn/wiki/mirrors/help/docker>

![1574134043126](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574134043126.png) 

**操作步骤**

+ 编辑文件/etc/docker/daemon.json

  ```shell
  # 执行如下命令
  mkdir /etc/docker
  vi /etc/docker/daemon.json  
  ```

+ 在文件中加入下面内容:

  ```shell
  {
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
  }
  ```

## 08、Docker：服务相关命令

> 目标: 掌握docker服务相关命令

常用命令:

```properties
# 启动docker服务
systemctl start docker
# 停止docker服务
systemctl stop docker
# 重启docker服务
systemctl restart docker
# 查看docker服务状态
systemctl status docker
# 设置开机启动docker服务
systemctl enable docker
# 查看docker概要信息
docker info
# 查看docker帮助文档
docker --help
```

注意: systemctl命令是系统服务管理器指令。

![1574134702001](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574134702001.png) 

**小结**

+ 启动docker服务: systemctl start docker
+ 停止docker服务: systemctl stop docker

## 09、Docker：镜像相关命令

> 目标: 掌握操作镜像的常用命令

#### 9.1 镜像介绍

+ Docker镜像是由文件系统叠加而成（是一种文件的存储形式）；是docker中的核心概念，可以认为镜像就是对某些运行环境或者软件打的包，用户可以从docker仓库中下载基础镜像到本地，比如开发人员可以从docker仓库拉取（下载）一个只包含centos7系统的基础镜像，然后在这个镜像中安装jdk、mysql、Tomcat和自己开发的应用，最后将这些环境打成一个新的镜像。开发人员将这个新的镜像提交给测试人员进行测试，测试人员只需要在测试环境下运行这个镜像就可以了，这样就可以保证开发人员的环境和测试人员的环境完全一致。

  ![1574135167278](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574135167278.png)  

#### 9.2 查看镜像

```properties
# 查看镜像可以使用如下命令:
docker images
```

![1574136867098](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574136867098.png) 

+ REPOSITORY: 镜像名称
+ TAG: 镜像标签 (默认是可以省略的,也就是latest)
+ IMAGE ID: 镜像ID
+ CREATED: 镜像的创建日期（不是获取该镜像的日期）
+ SIZE: 镜像大小

> 说明: 这些镜像都是存储在docker宿主机的/var/lib/docker目录下。

#### 9.3 搜索镜像

```shell
# 如果你需要从网络中查找需要的镜像，可以通过以下命令搜索
docker search 镜像名称
```

![1574137244207](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574137244207.png) 

+ NAME: 镜像名称
+ DESCRIPTION: 镜像描述
+ STARS: 用户评价，反应一个镜像的受欢迎程度
+ OFFICIAL: 是否官方
+ AUTOMATED: 自动构建，表示该镜像由Docker Hub自动构建流程创建的

推荐大家去官方搜索(http://hub.docker.com):

![1572838326986](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1572838326986.png) 

#### 9.4 拉取镜像

```properties
# 拉取镜像就是从Docker仓库下载镜像到本地，镜像名称格式为 名称:版本号，如果版本号不指定则是最新的版本 命令如下:
docker pull 镜像名称

# 拉取centos 7
docker pull centos:7
# 拉取centos 最后版本镜像
docker pull centos:latest
```

![1574138674249](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574138674249.png) 

#### 9.5 删除镜像

```properties
# 按照镜像id删除镜像
docker rmi 镜像ID
# 删除所有镜像(谨慎操作)
docker rmi `docker images -q`
```

![1574138776049](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574138776049.png) 

![1574138925582](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574138925582.png) 

## 10、Docker：容器相关命令

> 目标: 掌握操作容器相关的命令

#### 10.1 查看容器

+ 查看正在运行容器: docker ps

+ 查看所有容器: docker ps -a 

+ 查看最后一次运行的容器: docker ps –l

  ![1574146204730](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574146204730.png) 

#### 10.2 创建与运行容器

可以基于已有的镜像来创建容器，创建与运行容器使用命令: docker run

**参数说明:**

```txt
-i: 表示运行容器

-t: 表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。

--name: 为创建的容器命名。

-v: 表示目录映射关系（前者是宿主机目录，后者是容器的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。

-d: 在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。

-p: 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射。
```

##### 10.2.1 交互式容器

+ 以**交互式**方式创建并运行容器，启动完成后，直接进入当前容器。使用exit命令退出容器。需要注意的是以此种方式启动容器，如果退出容器，则容器会进入**停止**状态。

  ```shell
  # 先拉取一个镜像；这一步不是每次启动容器都要做的，而是因为前面我们删除了镜像，
  # 无镜像可用所以才再拉取一个
  docker pull centos:7
  
  # 创建并启动名称为 mycentos7 的交互式容器
  # 容器名称 mycentos7
  # 镜像名称:TAG (centos:7)  也可以使用镜像id (5e35e350aded)
  # /bin/bash: 进入容器命令行
  docker run -it --name=mycentos7 centos:7 /bin/bash
  ```

  ![1574149291840](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574149291840.png)  

##### 10.2.2 守互式容器

+ 创建一个守护式容器；如果对于一个需要长期运行的容器来说，我们可以创建一个守护式容器。命令如下（容器名称不能重复）。

  ```shell
  # 创建并启动守护式容器
  # 容器名称: mycentos2
  # 镜像名称:TAG (centos:7)  也可以使用镜像id (5e35e350aded)
  docker run -di --name=mycentos2 centos:7
  
  # 进入容器：
  # docker exec -it container_name (或者 container_id) /bin/bash
  # exit退出时，容器不会停止
  docker exec -it mycentos2 /bin/bash
  ```

  ![1574150692267](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574150692267.png) 

  > 说明: 守护式容器是一直运行的，退出只是退出终端，它还是在后台运行。

#### 10.3 停止或启动容器

```shell
# 停止正在运行的容器: docker stop 容器名称|容器ID
docker stop mycentos2
# 关闭所有容器
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
# 启动已运行过的容器: docker start 容器名称|容器ID
docker start mycentos2
# 启动所有容器
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
```

![1574151218515](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574151218515.png) 

#### 10.4 重启容器

```shell
# 重启正在运行的容器: docker restart 容器名称|容器ID
docker restart mycentos2
```

![1574151366035](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574151366035.png) 

#### 10.5 查看容器IP

我们可以通过以下命令查看容器运行的各种数据:

```shell
# 在linux宿主机下查看 mycentos2 的ip
# docker inspect 容器名称（容器ID）

docker inspect mycentos2
```

![1574152554022](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574152554022.png) 

> 容器之间在一个局域网内，linux宿主机器可以与容器进行通信；但是外部的物理机笔记本是不能与容器直接通信的，如果需要则需要通过宿主机器端口的代理。 

#### 10.6 删除容器

+ 删除指定的容器: docker rm 容器名称|容器ID
+ 删除所有容器: docker rm \`docker ps -a -q`

```shell
docker rm mycentos2
# 或者
docker rm 2095a22bee70

# 删除所有容器
docker rm `docker ps -a -q`
```

![1574153377358](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574153377358.png) 

![1574153254626](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574153254626.png) 

> 说明: 如果容器是运行状态则删除失败，需要停止容器才能删除 

**注意: 只能删除停止的容器**

## 11、Docker：容器文件拷贝

> 目标: 掌握文件拷贝命令

+ 将linux宿主机中的文件拷贝到容器内可以使用命令:

  ```shell
  # docker cp 需要拷贝的文件或目录 容器名称:容器目录
  
  # 创建一个文件abc.txt 
  touch abc.txt
  
  # 复制 abc.txt 到 mycentos2 的容器的 / 目录下 
  docker cp abc.txt mycentos2:/ 
  
  # 进入mycentos2容器 
  docker exec -it mycentos2 /bin/bash 
  
  # 查看容器 / 目录下文件
  ll
  ```

  ![1574154371433](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574154371433.png) 

+ 将文件从容器内拷贝出来到linux宿主机使用命令:

  ```shell
  # docker cp 容器名称:容器目录 需要拷贝的文件或目录
  
  # 进入容器后创建文件aaa.txt
  touch aaa.txt
  
  # 退出容器
  exit
  
  # 在Linux宿主机器执行复制；将容器mycentos2的/aaa.txt文件复制到 宿主机器的/root目录下
  docker cp mycentos2:/aaa.txt /root
  ```

  ![1574154905200](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574154905200.png) 

  > 注意: 停止状态的容器也是可以进行文件拷贝的，可以拷进去，也可以拷出来。

## 12、Docker：容器目录挂载

> 目标: 掌握目录挂载命令(其实就是目录映射)

+ 可以在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器。 

+ 创建容器时添加-v参数，后边为宿主机目录:容器目录，例如： docker run -di -v /usr/local/test:/usr/local/test --name=mycentos3 centos:7

  ```shell
  # 创建linux宿主机器要挂载的目录 
  mkdir /usr/local/test 
  
  # 创建并启动容器mycentos3
  # 并挂载 linux中的/usr/local/test目录到容器的/usr/local/test
  # 也就是在 linux中的/usr/local/test中操作相当于对容器相应目录操作 
  docker run -di -v /usr/local/test:/usr/local/test --name=mycentos3 centos:7
  
  # 在linux下创建文件 
  touch /usr/local/test/bbb.txt
  
  # 进入容器 
  docker exec -it mycentos3 /bin/bash
  
  # 在容器中查看目录中是否有对应文件bbb.txt
  cd /usr/local/test
  ll
  ```

  ![1574156479325](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574156479325.png) 

  > 注意: 如果你共享的是多级的目录，可能会出现权限不足的提示。 这是因为CentOS7中的安全模块selinux把权限禁掉了，需要添加参数 --privileged=true 来解决挂载的目录没有权限的问题。 

## 13、Docker：安装mysql容器

> 目标: 掌握在docker中安装mysql容器

**操作步骤**

+ 拉取镜像

  ```shell
  # 拉取MySQL 5.7镜像
  docker pull centos/mysql-57-centos7
  ```

  ![1574157836005](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574157836005.png) 

+ 创建容器

  docker run -di --name=mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql

  + -p 代表端口映射，格式为 宿主机映射端口:容器运行端口
  + -e 代表添加环境变量 MYSQL_ROOT_PASSWORD 是root用户的远程登陆密码（如果是在容器中使用root登录的话,那么其**密码为空**）

  ```shell
  # 创建mysql5.7容器 
  docker run -di --name=mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root centos/mysql-57-centos7
  ```

  ![1574158421287](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574158421287.png)  

+ 操作容器mysql

  ```shell
  # 进入mysql5.7容器
  docker exec -it mysql5.7 /bin/bash 
  
  # 登录容器里面的mysql 
  mysql -u root -p
  ```

  ![1574160456871](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574160456871.png) 

+ 远程登录mysql

  ```shell
  # 查看ip；如果以后要内部连接该mysql，如其他容器中要连接mysql容器的mysql的时候，可以使用如下命令查看IP
  docker inspect mysql5.7
  ```

  ![1574160657423](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574160657423.png) 

  使用SQLyou在windows中进行远程登录在docker容器中的mysql。  

  ![1574160942998](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574160942998.png) 

## 14、Docker：安装tomcat容器

> 目标: 掌握在docker中安装tomcat容器

**操作步骤**

+ 拉取镜像

  ```shell
  # 拉取tomcat镜像
  docker pull tomcat
  ```

  ![1574215667463](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574215667463.png) 

+ 创建容器

  ```shell
  # 创建tomcat容器;并挂载了webapps目录
  docker run -di --name=mytomcat -p 9000:8080 -v /usr/local/tomcat/webapps:/usr/local/tomcat/webapps tomcat
  
  # 查看日志
  docker logs -f mytomcat
  
  
  # 如果出现 WARNING: IPv4 forwarding is disabled. Networking will not work. 
  # 执行如下操作 
  # 1、编辑 sysctl.conf 
  vi /etc/sysctl.conf 
  
  # 2、在上述打开的文件中后面添加 
  net.ipv4.ip_forward=1
  
  # 3、重启network
  systemctl restart network
  ```

  ![1574216589588](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574216589588.png) 

  测试访问宿主机的端口号为9000的 tomcat。地址：http://宿主机ip:9000，也可以往/user/local/tomcat/webapps下部署应用，然后再访问。

+ 部署web应用

  + 创建springboot_db数据库,再创建tb_user表

    ![1574218496038](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218496038.png) 

  + 查看mysql5.7容器的ip地址(docker inspect mysql5.7)

    ![1574218688435](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218688435.png) 

  + 修改springboot-high工程的application.yml

    ![1574218750102](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218750102.png) 

  + 项目打成war包

    ![1574218794796](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218794796.png) 

     执行: mvn package -Dmaven.test.skip=true

  + 上传ROOT.war到/usr/local/tomcat/webapps/目录下。

    ![1574218936784](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218936784.png) 

  + 浏览器访问 (<http://192.168.12.132:9000/user.html>)

    ![1574218968488](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574218968488.png) 

## 15、Docker：安装Nginx容器

> 目标: 掌握在docker中安装nginx容器

**操作步骤**

+ 拉取镜像

  ```shell
  # 拉取nginx镜像 
  docker pull nginx
  ```

+ 创建容器

  ```shell
  # 创建nginx容器 
  docker run -di --name=mynginx -p 80:80 nginx
  
  # 查看日志
  docker logs -f mynginx
  ```

  ![1574220424119](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574220424119.png) 

+ 测试访问(启动后再宿主机上访问: http://宿主机IP/)

  ![1574220471997](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574220471997.png) 

+ 配置反向代理(去掉9000端口)

  官方的nginx镜像，nginx配置文件 nginx.conf 在/etc/nginx/目录下。

  在容器内编辑配置文件不方便，我们可以先将配置文件从容器内拷贝到宿主机，编辑修改后再拷贝回去。

  + 第一步: 从mynginx容器拷贝配置文件到宿主机

    ```shell
    docker cp mynginx:/etc/nginx/nginx.conf nginx.conf
    ```

    ![1574222059993](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574222059993.png) 

  + 第二步: 查看mytomcat容器的ip地址(docker inspect mytomcat)

    ![1574222289095](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574222289095.png) 

  + 第三步: 配置nginx.conf

    ```conf
    server {
    	listen 80;
    	server_name 127.0.0.1;
    
    	location / {
    	    proxy_pass http://172.17.0.2:8080;
    	}
    }
    ```

    ![1574222458673](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574222458673.png) 

  + 将修改后的配置文件拷贝到容器

    ```shell
    docker cp nginx.conf mynginx:/etc/nginx/nginx.conf
    ```

    ![1574222603952](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574222603952.png) 

  + 重新启动容器

    ```shell
    docker restart mynginx
    ```

    ![1574223715466](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574223715466.png) 

## 16、Docker：安装Redis容器

> 目标: 掌握在docker中安装redis容器

**操作步骤**

+ 拉取镜像

  ```shell
  # 拉取redis镜像
  docker pull redis
  ```

+ 创建容器

  ```shell
  # 创建redis容器
  docker run -di --name=myredis -p 6379:6379 redis
  
   # 查看redis日志
  docker logs -f myredis 
  ```

  ![1574224235332](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574224235332.png) 

+ 操作redis容器

  ```shell
  # 进入redis容器
  docker exec -it myredis /bin/bash 
  
  # 进入redis安装目录 
  cd /usr/local/bin
  
  # 连接redis 
  ./redis-cli
  ```

   ![1574224343951](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574224343951.png) 

+ 远程连接redis

  可以使用redis图形界面客户端工具连接redis，端口也是6379。

  ![1574224541883](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574224541883.png) 

## 17、Docker：安装Rabbitmq容器

> 目标: 掌握在docker中安装rabbitmq容器

**操作步骤**

+ 拉取镜像

  ```shell
  # 拉取rabbitmq镜像
  docker pull rabbitmq:management
  ```

+ 创建容器

  ```shell
  # 创建Rabbitmq容器
  # 创建容器，rabbitmq需要有映射以下端口:
  # 15672: web管理的端口
  # 5672：(客户端连接端口)
  docker run -di --name=myrabbitmq  -p 5672:5672 -p 15672:15672 rabbitmq:management
  
   # 查看日志
  docker logs -f myrabbit
  ```

  ![1574230927161](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574230927161.png) 

+ 访问rabbit控制台: http://192.168.12.132:15672

  ![1574230965900](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574230965900.png) 

  ![1574231068763](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574231068763.png)  

## 18、Docker：容器备份与迁移

> 目标: 掌握docker中容器的备份与迁移

![1574231546282](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574231546282.png) 

主要作用: 就是让配置好的容器，可以得到复用，后面用到得的时候就不需要重新配置。

其中涉及到的命令有:

+ docker commit 将容器保存为镜像 
+ docker save 将镜像备份为tar文件 
+ docker load 根据tar文件恢复为镜像

**操作步骤**

+ 容器保存为镜像 (使用docker commit命令可以将容器保存为镜像)。 

  命令格式: docker commit 容器名称 新的镜像名称

  ```sh
  # 将容器保存为镜像
  # mynginx：容器名称、mynginx：新的镜像名称
  docker commit mynginx mynginx
  ```

  ![1574232069718](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574232069718.png) 

  说明: 此镜像的内容就是当前容器的内容，接下来你可以用此镜像再次运行新的容器.

+ 镜像备份 (使用docker save命令可以将已有镜像保存为tar文件)

  命令格式: docker save –o tar文件名 镜像名

  ```shell
  # 保存镜像为文件 
  docker save -o mynginx.tar mynginx
  ```

  ![1574232348861](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574232348861.png) 

+ 镜像恢复与迁移 (使用docker load命令可以根据tar文件恢复为docker镜像)

  命令格式: docker load -i tar文件名

  ```shell
  # 停止mynginx容器 
  docker stop mynginx
  
  # 删除mynginx容器 
  docker rm mynginx 
  
  # 删除mynginx镜像 
  docker rmi mynginx 
  
  # 加载恢复mynginx镜像 
  docker load -i mynginx.tar 
  
  # 在镜像恢复之后，基于该镜像再次创建启动容器 
  docker run -di --name=mynginx -p 80:80 mynginx
  ```

  ![1574233036494](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574233036494.png) 

  ![1574233127035](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574233127035.png) 

  ![1574233194863](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574233194863.png) 

**总结**

+ 容器保存镜像: docker commit 容器名称 新的镜像的名称
+ 导出镜像: docker save -o 镜像名称.tar 新的镜像的名称
+ 导入镜像: docker load -i 镜像名称.tar

## 19、Docker：Dockerfile构建镜像

> 目标: 掌握Dockerfile构建镜像

#### 19.1 Dockerfile文件

+ 前面的课程中已经知道了，要获得镜像，可以从Docker仓库中进行下载。那如果我们想自己开发一个镜像，那该如何做呢？答案是: Dockerfile 
+ Dockerfile其实就是一个文本文件，由一系列命令和参数构成，Docker可以读取Dockerfile文件并根据Dockerfile文件的描述来构建镜像。
+ Dockerfile文件内容一般分为4部分
  + 基础镜像信息
  + 维护者信息
  + 镜像操作指令
  + 容器启动时执行的指令

#### 19.2 常用命令

| **命令**                           | **作用**                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| FROM image_name:tag                | 定义了使用哪个基础镜像启动构建流程                           |
| MAINTAINER user_name               | 声明镜像的创建者                                             |
| ENV key value                      | 设置环境变量 (可以写多条)                                    |
| RUN command                        | 是Dockerfile的核心部分(可以写多条)                           |
| ADD source_dir/file dest_dir/file  | 将宿主机的文件复制到容器内，如果是一个压缩文件，将会在复制后自动解压 |
| COPY source_dir/file dest_dir/file | 和ADD相似，但是如果有压缩文件并不能解压                      |
| WORKDIR path_dir                   | 设置工作目录                                                 |

#### 19.3 构建镜像

```shell
# 1、创建目录 
mkdir -p /usr/local/dockerjdk8 
cd /usr/local/dockerjdk8 

# 2、下载jdk-8u171-linux-x64.tar.gz并上传到服务器（虚拟机）中的/usr/local/dockerjdk8目录 

# 3、在/usr/local/dockerjdk8目录下创建Dockerfile文件，文件内容如下:
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

# 4、执行命令构建镜像；不要忘了后面的那个 . 
docker build -t='jdk1.8' .

# 5、查看镜像是否建立完成 
docker images
```

/usr/local/dockerjdk8/Dockerfile 文件中的内容:

![1574236736749](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574236736749.png) 

![1574236923379](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574236923379.png) 

![1574236957603](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574236957603.png) 

![1574237020272](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574237020272.png) 

> 说明: 创建文件Dockerfile 这里的D必须大写，不能有任何的偏差。

#### 19.4 镜像创建容器

基于刚刚创建的镜像 jdk1.8 创建并启动容器进行测试

```shell
# 创建并启动容器 
docker run -it --name=testjdk jdk1.8 /bin/bash
# 在容器中测试jdk是否已经安装 
java -version
```

![1574237271724](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574237271724.png) 

## 20、Docker：registry私服仓库

> 目标: 掌握docker私服仓库搭建

#### 20.1 私有仓库搭建与配置

Docker官方的Docker hub（https://hub.docker.com）是一个用于管理公共镜像的仓库，我们可以从上面拉取镜像到本地，也可以把我们自己的镜像推送上去。但是，有时候我们的服务器无法访问互联网，或者你不希望将自己的镜像放到公网当中，那么我们就需要搭建自己的私有仓库来存储和管理自己的镜像。

私有仓库搭建步骤:

```shell
# 1、拉取私有仓库镜像 
docker pull registry

# 2、启动私有仓库容器
docker run -di --name=registry -p 5000:5000 registry 

# 3、打开浏览器 输入地址http://宿主机ip:5000/v2/_catalog，看到{"repositories":[]} 表示私有仓库 搭建成功 

# 4、修改daemon.json 
vi /etc/docker/daemon.json

# 在上述文件中添加一个key，保存退出。此步用于让 docker 信任私有仓库地址；注意将宿主机ip修改为自己宿主 机真实ip 
{"insecure-registries":["宿主机ip:5000"]} 

# 5、重启docker 服务 
systemctl restart docker 

docker start registry
```

访问: <http://192.168.12.132:5000/v2/_catalog> 

![1574238847936](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574238847936.png) 

![1574238940458](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574238940458.png) 

#### 20.2 将镜像上传至私有仓库

```shell
# 1、标记镜像为私有仓库的镜像 
# 语法: docker tag jdk1.8 宿主机IP:5000/jdk1.8
docker tag jdk1.8 192.168.12.132:5000/jdk1.8

# 2、再次启动私有仓库容器 
docker restart registry 

# 3、上传标记的镜像到私有仓库
# 语法: docker push 宿主机IP:5000/jdk1.8 
docker push 192.168.12.132:5000/jdk1.8

# 4、输入网址查看仓库效果
```

![1574239723576](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574239723576.png) 

![1574239743318](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574239743318.png) 

#### 20.3 从私有仓库拉取镜像

若是在私有仓库所在的服务器上去拉取镜像；那么直接执行如下命令:

```shell
# 因为私有仓库所在的服务器上已经存在相关镜像；所以先删除；请指定镜像名，不是id 
# 语法: docker rmi 服务器ip:5000/jdk1.8
docker rmi 192.168.12.132:5000/jdk1.8

# 拉取镜像 
# 语法: docker pull 服务器ip:5000/jdk1.8
docker pull 192.168.12.132:5000/jdk1.8

#可以通过如下命令查看 docker 的信息；了解到私有仓库地址 
docker info
```

![1574241484821](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574241484821.png) 

![1574241554923](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片/docker/1574241554923.png) 

## 21.安装elasticsearch

### 安装kibana

#### 镜像官网搜索 `kibana`

#### 直接拉取镜像:

```shell
sudo docker pull kibana:7.4.2
```

#### 安装kibana容器

##### 创建实例并启动：

kibana:

> docker run --name kibana --link elasticsearch:elasticsearch -p 5601:5601 -d kibana:7.4.2

参数说明:

> -p 5601:5601 将容器的9200端口映射到主机的9200端口;
> --name kibana 给当前启动的容器取名叫 elasticsearch
> -d 以后台方式运行(daemon)
> --link 这个参数值得细说一下, 主要是实现容器互联, 实现容器间的通信

##### 容器互联实现容器间通信(--link)

关于docker环境下安装elasticsearch+kibana连不上问题

容器的link是除了端口映射之外另一种可以与容器中应用进行交互的方式. 它是在源和接收容器之间形成一个隧道, 接收容器可以看到源容器指定的信息;

1. 自定义容器名(--name)

   也就是我们docker run时指定的 `--name` 参数值, 连接的时候可以作为连接依据, 比如上面的:

   > docker run --name kibana --link elasticsearch:elasticsearch -p 5601:5601 -d kibana:7.4.2

   就是说在 kibana 的容器运行时, 指定 一个连接的容器: name是 elasticsearch

2. --link语法

   --link name:alias

   > --link elasticsearch:elasticsearch

   我们可以看看 kibana的环境变量和hosts文件, 这两个文件里会体现出这种link关系:

3. kibana 容器的环境变量信息:

   可以看到: ELASTICSEARCH开头的环境变量,是提供 kibana 连接 elasticsearch容器使用, 前缀采用大写的连接别名;

   ```shell
   [root@localhost ~]# docker exec -it kibana /bin/bash
   bash-4.2$ env
   HOSTNAME=9c3655df879c
   ELASTICSEARCH_PORT_9200_TCP_PORT=9200
   TERM=xterm
   ELASTIC_CONTAINER=true
   ELASTICSEARCH_ENV_ELASTIC_CONTAINER=true
   ELASTICSEARCH_NAME=/kibana/elasticsearch
   ELASTICSEARCH_PORT_9200_TCP_PROTO=tcp
   ELASTICSEARCH_PORT_9300_TCP_PORT=9300
   ELASTICSEARCH_PORT_9300_TCP_ADDR=172.17.0.4
   PATH=/usr/share/kibana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
   PWD=/usr/share/kibana
   SHLVL=1
   HOME=/usr/share/kibana
   ELASTICSEARCH_PORT=tcp://172.17.0.4:9200
   ELASTICSEARCH_ENV_discovery.type=single-node
   ELASTICSEARCH_PORT_9200_TCP=tcp://172.17.0.4:9200
   ELASTICSEARCH_PORT_9300_TCP=tcp://172.17.0.4:9300
   ELASTICSEARCH_ENV_ES_JAVA_OPS=-Xms128m -Xmx512m
   ELASTICSEARCH_PORT_9300_TCP_PROTO=tcp
   ELASTICSEARCH_PORT_9200_TCP_ADDR=172.17.0.4
   _=/usr/bin/env    
   ```

4. kibana的hosts文件:

   ```shell
   bash-4.2$ cat /etc/hosts
   127.0.0.1    localhost
   ::1    localhost ip6-localhost ip6-loopback
   fe00::0    ip6-localnet
   ff00::0    ip6-mcastprefix
   ff02::1    ip6-allnodes
   ff02::2    ip6-allrouters
   172.17.0.4    elasticsearch 6aa29c59ba9a
   172.17.0.5    9c3655df879c
   ```

   注意看这里的

   `172.17.0.5 9c3655df879c` 9c3655df879c是 kibana 的 容器id

   `172.17.0.4 elasticsearch 6aa29c59ba9a` 6aa29c59ba9a 是 elasticsearch 的 容器id

   ```shell
   [root@localhost ~]# docker ps
   CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
   9c3655df879c        kibana:7.4.2          "/usr/local/bin/dumb…"   40 minutes ago      Up 40 minutes       0.0.0.0:5601->5601/tcp                           kibana
   6aa29c59ba9a        elasticsearch:7.4.2   "/usr/local/bin/dock…"   3 hours ago         Up 3 hours          0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   elasticsearch
   ```

##### 启动kibana容器

```shell
docker start kibana
# 设置docker启动自动运行kibana
docker update kibana --restart=always
```

#### 访问kibana

[http://192.168.56.100](https://link.segmentfault.com/?enc=t6PpkIF6C1uY0DFHKQ52cw%3D%3D.nVZF96liaQO1wX8M5HBCBEszdn3m%2F%2BaBBWBjBRqAvMY%3D):5601/

#### 问题小结

原来用的是`docker run --name kibana -e ELASTICSEARCH_HOSTS=http://192.168.56.103:9200 -p 5601:5601 -d kibana:7.4.2` , kibana和es连不上,看 `docker logs kibana `日志提示: kibana中设置es的ip不对的问题, 导致一直报:

```shell
kibana Unable to revive connection: http://elasticsearch:9200
```

使用了`--link elasticsearch:elasticsearch`参数之后, kibana可以访问 es了:

> docker run --name kibana --link elasticsearch:elasticsearch -p 5601:5601 -d kibana:7.4.2

## docker安装elasticsearch

1.设置max_map_count不能启动es会启动不起来
查看max_map_count的值 默认是65530

```bash
cat /proc/sys/vm/max_map_count
```

重新设置max_map_count的值

```bash
sysctl -w vm.max_map_count=262144
```

2.下载镜像并运行

#拉取镜像

```bash
docker pull elasticsearch:7.7.0
```

#启动镜像

```bash
docker run --name elasticsearch -d -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 elasticsearch:7.7.0
```

参数说明

> --name表示镜像启动后的容器名称  

> -d: 后台运行容器，并返回容器ID；

> -e: 指定容器内的环境变量

> -p: 指定端口映射，格式为：主机(宿主)端口:容器端口

3.浏览器访问ip:9200 如果出现以下界面就是安装成功

![20201223190232693](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201223190232693.png)

4.安装elasticsearch-head

```bash
#拉取镜像

docker pull mobz/elasticsearch-head:5

#创建容器

docker create --name elasticsearch-head -p 9100:9100 mobz/elasticsearch-head:5

#启动容器

docker start elasticsearch-head

or

docker start 容器id （docker ps -a 查看容器id ）
```

5.浏览器打开: http://IP:9100

![20201223171452568](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201223171452568.png)

尝试连接easticsearch会发现无法连接上，由于是前后端分离开发，所以会存在跨域问题，需要在服务端做CORS的配置。
解决办法

6.修改docker中elasticsearch的elasticsearch.yml文件

```bash
docker exec -it elasticsearch /bin/bash （进不去使用容器id进入）

vi config/elasticsearch.yml
```

在最下面添加2行

```bash
http.cors.enabled: true 
http.cors.allow-origin: "*"
```

![20201223171735464](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201223171735464.png)

退出并重启服务

```bash
exit
docker restart 容器id
```

![20201223171815168](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201223171815168.png)

7.ElasticSearch-head 操作时不修改配置，默认会报 406错误码

```bash
#复制vendor.js到外部

docker cp fa85a4c478bf:/usr/src/app/_site/vendor.js /usr/local/

#修改vendor.js

vim vendor.js
```

![20201224103442628](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201224103442628.png)

修改完成在复制回容器

```bash
docker cp /usr/local/vendor.js  fa85a4c478bf:/usr/src/app/_site
```

重启elasticsearch-head

```bash
docker restart 容器id
```

最后就可以查询到es数据了

![20201224103651968](https://raw.githubusercontent.com/Kid-On-The-Road/Resources/main/笔记图片\docker\20201224103651968.png)

8.安装ik分词器
这里采用离线安装

下载分词器压缩包
下载地址：

https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.7.0/elasticsearch-analysis-ik-7.7.0.zip
将IK分词器上传到/tmp目录中（xftp）

将分词器安装进容器中

```bash
#将压缩包移动到容器中

docker cp /tmp/elasticsearch-analysis-ik-7.7.0.zip elasticsearch:/usr/share/elasticsearch/plugins

#进入容器

docker exec -it elasticsearch /bin/bash  

#创建目录

mkdir /usr/share/elasticsearch/plugins/ik

#将文件压缩包移动到ik中

mv /usr/share/elasticsearch/plugins/elasticsearch-analysis-ik-7.7.0.zip /usr/share/elasticsearch/plugins/ik

#进入目录

cd /usr/share/elasticsearch/plugins/ik

#解压

unzip elasticsearch-analysis-ik-7.7.0.zip

#删除压缩包

rm -rf elasticsearch-analysis-ik-7.7.0.zip
```

退出并重启镜像

