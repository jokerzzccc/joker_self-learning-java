---

---

# Day30-Docker

2021-04-08

学习视频：狂神说

docker 官方学习文档：https://docs.docker.com/

docker 官方文档是很好用的，不会就去查。

docker 中文社区：https://www.docker.org.cn/

菜鸟教程的学习文档：https://www.runoob.com/docker/docker-tutorial.html

学习前的基础：Linux 基础命令，SpringBoot (最好会点)，JS

docker 学习路径：

- Docker 概述
- Docker 安装
- Docker 命令
  - 镜像命令
  - 容器命令
  - 操作命令
  - 。。。
- Docker 镜像
- 容器数据卷
- DockerFile
- Docker网络原理
- IDEA 整合Docker 
- Docker Compose
- Docker Swarm
- CI\CD,Jenkins



# Docker 概述

## Docker 为什么出现

- 总结：一个项目，在不同环境（开发-运维）方便配置。及多个项目，在同一个服务器可以隔离部署。

一款产品从开发到上线，从操作系统，到运行环境，再到应用配置。作为开发+运维之间的协作我们需要关心很多东西，这也是很多互联网公司都不得不面对的问题，特别是各种版本的迭代之后，不同版本环境的兼容，对运维人员是极大的考验！

环境配置如此麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，**软件可以带环境安装**？也就是说，安装的时候，把原始环境一模一样地复制过来。解决开发人员说的“ 在我的机器上可正常工作”的问题。

之前在服务器配置一个应用的运行环境，要安装各种软件，就拿一个基本的工程项目的环境来说吧，Java/Tomcat/MySQL/JDBC驱动包等。安装和配置这些东西有多麻烦就不说了，它还不能跨平台。假如我们是在 Windows 上安装的这些环境，到了 Linux 又得重新装。况且就算不跨操作系统，换另一台同样操作系统的服务器，要移植应用也是非常麻烦的。

传统上认为，软件编码开发/测试结束后，所产出的成果即是程序或是能够编译执行的二进制字节码文件等（Java为例）。而为了让这些程序可以顺利执行，开发团队也得准备完整的部署文件，让维运团队得以部署应用程式，**开发需要清楚的告诉运维部署团队，用的全部配置文件所有软件环境。不过，即便如此，仍然常常发生部署失败的状况。**

Docker之所以发展如此迅速，也是因为它对此给出了一个标准化的解决方案。

**Docker镜像的设计，使得Docker得以打破过去「程序即应用」的观念。通过Docker镜像 ( images ) 将应用程序所需要的系统环境，由下而上打包，达到应用程序跨平台间的无缝接轨运作。**	



Docker的思想来自于集装箱，集装箱解决了什么问题？在一艘大船上，可以把货物规整的摆放起来。并且各种各样的货物被集装箱标准化了，集装箱和集装箱之间不会互相影响。那么我就不需要专门运送水果的船和专门运送化学品的船了。只要这些货物在集装箱里封装的好好的，那我就可以用一艘大船把他们都运走。



## Docker 的历史

2010年，几个搞IT的年轻人，在美国旧金山成立了一家名叫“dotCloud”的公司。

这家公司主要提供基于PaaS（platform as a service）的云计算技术服务。具体来说，是和LXC有关的容器技术。

后来，dotCloud公司将自己的容器技术进行了简化和标准化，并命名为——**Docker**。

Docker技术诞生之后，并没有引起行业的关注。而dotCloud公司，作为一家小型创业企业，在激烈的竞争之下，也步履维艰。正当他们快要坚持不下去的时候，脑子里蹦出了“开源”的想法。

2013年3月，dotCloud公司的创始人之一，Docker之父，28岁的**Solomon Hykes**正式决定，将Docker项目开源。不开则已，一开惊人。越来越多的IT工程师发现了Docker的优点，然后蜂拥而至，加入Docker开源社区。

Docker的人气迅速攀升，速度之快，令人瞠目结舌。

开源当月，Docker 0.1 版本发布。此后的每一个月，Docker都会发布一个版本。到2014年6月9日，Docker 1.0 版本正式发布。

此时的Docker，已经成为行业里人气最火爆的开源技术，没有之一。甚至像Google、微软、Amazon、VMware这样的巨头，都对它青睐有加，表示将全力支持。

Docker和容器技术为什么会这么火爆？说白了，就是因为它“轻”。

在容器技术之前，业界的网红是**虚拟机**。虚拟机技术的代表，是**VMWare**和**OpenStack**。相信很多人都用过虚拟机。虚拟机，就是在你的操作系统里面，装一个软件，然后通过这个软件，再模拟一台甚至多台“子电脑”出来。在“子电脑”里，你可以和正常电脑一样运行程序，例如开QQ。如果你愿意，你可以变出好几个“子电脑”，里面都开上QQ。“子电脑”和“子电脑”之间，是**相互隔离**的，互不影响。

虚拟机属于虚拟化技术。而Docker这样的容器技术，也是虚拟化技术，属于**轻量级的虚拟化**。

**虚拟机**虽然可以隔离出很多“子电脑”，但占用空间更大，启动更慢，虚拟机软件可能还要花钱（例如VMWare）。

而**容器技术**恰好没有这些缺点。它不需要虚拟出整个操作系统，只需要虚拟一个小规模的环境（类似“沙 箱”）。

它启动时间很快，几秒钟就能完成。而且，它对资源的利用率很高（一台主机可以同时运行几千个Docker容器）。此外，它占的空间很小，虚拟机一般要几GB到几十GB的空间，而容器只需要MB级甚至KB级。

正因为如此，容器技术受到了热烈的欢迎和追捧，发展迅速。



## 容器技术与虚拟机技术对比

### 虚拟机

![image-20210413012852267](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413012852267.png)

虚拟机（virtual machine）就是带环境安装的一种解决方案。

它可以在一种操作系统里面运行另一种操作系统，比如在Windows 系统里面运行Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。

虚拟机缺点：

1、资源占用多

2、冗余步骤多

3 、启动慢

### 容器虚拟化技术

![image-20210413013023264](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413013023264.png)

由于前面虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器（Linux Containers，缩

写为 LXC）。

**Linux 容器**不是模拟一个完整的操作系统，而是**对进程进行隔离**。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，**只需要软件工作所需的库资源和设置**。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

比较了 Docker 和传统虚拟化方式的不同之处：

- 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用

进程；

- 而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。

- 因此容器要比传统虚拟机更为轻便。

- 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。



### Dokcker 对于开发和运维（DevOps）

**更快速的应用交付和部署：**

**传统**的应用开发完成后，需要提供一堆安装程序和配置说明文档，安装部署后需根据配置文档进行繁杂的配置才能正常运行。

**Docker化**之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间。

**更便捷的升级和扩缩容：**

随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级。

**更简单的系统运维：**

应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复。

**更高效的计算资源利用：**

Docker是内核级虚拟化，其不像传统的虚拟化技术一样需要额外的Hypervisor [管理程序] 支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率。

## Docker 学习

Docker官网：http://www.docker.com 多看官方文档吧 https://docs.docker.com/

Docker Hub官网：https://hub.docker.com （仓库）

docker 中文社区：https://www.docker.org.cn/

菜鸟教程的 docker 学习文档：https://www.runoob.com/docker/docker-tutorial.html

# Dokcer 安装

## Docker 的基本组成

Docker 架构：  

![image-20210413013513577](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413013513577.png)

从官网可以看到每部分详细信息。 https://docs.docker.com/get-started/overview/#docker-architecture

- images （镜像）：
  - Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。 就好似 Java 中的 类和对象，类就是镜像，容器就是对象！ 
- containers (容器)：
  - 容器是用镜像创建的运行实例。Docker 利用容器（Container）独立运行的一个或一组应用。 它可以被启动、开始、停止、删除。每个容器都是相互隔离的，保证安全的平台。 
  - 可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等） 和运行在其中的应用程序。
  - 容器的定义和镜像几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写 的。 

- registry (注册表)：
  - Docker registry 存储Docker镜像。 Docker Hub是任何人都可以使用的公共注册表，并且默认情况下，Docker已配置为在Docker Hub上查找镜像。 您甚至可以运行自己的私人注册表。
  - Docker Hub 默认是国外的。所以速度慢，国内就用镜像。阿里云。配置镜像加速

## 安装 Docker

前提：

我是用阿里云服务器，centOS 7 

```bash
#系统内核 3.10
[root@joker /]# uname -r
3.10.0-1062.18.1.el7.x86_64
#我的服务器版本信息
[root@joker etc]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

[root@joker etc]# 

```



Dokcer 官网安装：https://docs.docker.com/engine/install/centos/

```bash
#卸载旧版本先：
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
                  
# 需要的安装包：
yum install -y yum-utils

# 设置镜像仓库。默认是国外的，需要用国内的阿里云镜像
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo #国外的，默认的
    
yum-config-manager \
    --add-repo \
	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 阿里云的
	
# 更新yum软件包索引（最好更新下）
yum makecache fast
# 安装docker 引擎。 docker-ce 社区版（推荐使用），还有ee 企业版
#默认是最新版，可以根据官网选择指定版本
yum install docker-ce docker-ce-cli containerd.io

#启动 Docker
systemctl start docker
# 测试 hello world
docker run hello-world
```

测试结果

```bash
[root@joker /]# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
# 查看下载的镜像 hello-world
[root@joker /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    d1165f221234   5 weeks ago   13.3kB
[root@joker /]# 


```

查看docker 版本

```bash
[root@joker /]# docker version
Client: Docker Engine - Community
 Version:           20.10.6
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        370c289
 Built:             Fri Apr  9 22:45:33 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.6
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8728dd2
  Built:            Fri Apr  9 22:43:57 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
[root@joker /]# 

```



**卸载 docker**

```bash
# 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
# 删除资源
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

Dokcker 的默认工作路径：/var/lib/docker 



## 阿里云镜像加速

1、登录自己的阿里云账号，查看容器镜像服务 （ACR ）。ACR介绍：https://www.aliyun.com/product/acr

![image-20210413022600146](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413022600146.png)

2、找到镜像加速器，选择自己的系统版本 (我是centOS 7)

![image-20210413022722689](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413022722689.png)

3 、根据网站说明，配置文件

![image-20210413023026531](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413023026531.png)

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://gn3o22mh.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```



阿里云镜像加速就配置好了



## Docker 的hello-world 运行原理

![image-20210413024648328](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413024648328.png)

## Docker 底层原理

Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上， 然后通过Socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。 容器，是一个运行时环境，就是我们前面说到的集装箱。

![image-20210413025409718](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413025409718.png)

**为什么Docker比较VM快** 

1、docker有着比虚拟机**更少的抽象层**。

由于docker不需要Hypervisor实现硬件资源虚拟化,运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势。

2、docker利用的是宿主机的内核,而不需要Guest OS。

因此,当新建一个容器时,docker不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引寻、加载操作系统内核返个比较费时费资源的过程,当新建一个虚拟机时,虚拟机软件需要加载Guest OS,返个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统,则省略了返个过程,因此新建一个docker容器只需要几秒钟。

![image-20210413025543474](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413025543474.png)

![image-20210413025758638](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413025758638.png)





# Docker 常用命令

- 相关命令，可以查看官网接口文档 https://docs.docker.com/reference/

## 帮助命令

``` bash
# docker 版本信息
docker version
# docker 详细系统信息，包括镜像和容器数量
docker info
# docker 帮助命令。万能命令，
docker [命令] --help
```



## 镜像命令 image

image （单个镜像操作）命令 https://docs.docker.com/engine/reference/commandline/image/

### docker images

images 列举镜像

```bash
[root@joker /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    d1165f221234   5 weeks ago   13.3kB
# --解释 
# REPOSITORY 镜像的仓库源 
# TAG 镜像的标签 
# IMAGE ID 镜像的ID 
# CREATED 镜像创建时间 
# SIZE 镜像大小
#  同一个仓库源可以有多个 TAG，代表这个仓库源的不同版本，我们使用REPOSITORY：TAG 定义不同 的镜像，如果你不定义镜像的标签版本，docker将默认使用 lastest 镜像！

# 查看images 命令可选项
[root@joker /]# docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs
[root@joker /]# 

# 常用
-a # 显示全部 image  
-q # 只显示 image id


```

### docker search

- https://docs.docker.com/engine/reference/commandline/search/

- Search the Docker Hub for images
- 也可以直接去 Docker Hub 上搜索

```bash
[root@joker /]# docker search --help

Usage:  docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
# 比如：
[root@joker /]# docker search mysql
# 带过滤器的 filter 后面是k-v
[root@joker /]# docker search --filter stars=3000 mysql
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   10730     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4041      [OK]       
[root@joker /]# 

```

### docker pull 下载

```bash
[root@joker /]# docker pull --help

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output
[root@joker /]# 
# 解释
# :tag 标签，不加这个就是默认的latest ，不能乱写，要是docker hub 上有的标签，可以在docker hub 上查看
# @digest 就是你要下载版本的签名，这个是唯一标识符

# 比如：
[root@joker etc]# docker pull mysql
Using default tag: latest  # 默认tag 就是latest
latest: Pulling from library/mysql 
f7ec5a41d630: Pull complete  # 分层下载 docker image 的核心，联合文件系统
9444bb562699: Pull complete 
6a4207b96940: Pull complete 
181cefd361ce: Pull complete 
8a2090759d8a: Pull complete 
15f235e0d7ee: Pull complete 
d870539cd9db: Pull complete 
5726073179b6: Pull complete 
eadfac8b2520: Pull complete 
f5936a8c3f2b: Pull complete 
cca8ee89e625: Pull complete 
6c79df02586a: Pull complete 
Digest: sha256:6e0014cdd88092545557dee5e9eb7e1a3c84c9a14ad2418d5f2231e930967a38 # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest   # 真实下载地址
[root@joker etc]# 

# docker pull mysql 命令等价于 docker pull docker.io/library/mysql:latest

# 指定版本下载 加:tag
[root@joker etc]# docker pull mysql:5.7
5.7: Pulling from library/mysql
f7ec5a41d630: Already exists  # 已经存在的镜像资源可以公用，不用下载。这就是分层下载。
9444bb562699: Already exists 
6a4207b96940: Already exists 
181cefd361ce: Already exists 
8a2090759d8a: Already exists 
15f235e0d7ee: Already exists 
d870539cd9db: Already exists 
7310c448ab4f: Pull complete 
4a72aac2e800: Pull complete 
b1ab932f17c4: Pull complete 
1a985de740ee: Pull complete 
Digest: sha256:e42a18d0bd0aa746a734a49cbbcc079ccdf6681c474a238d38e79dc0884e0ecc
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
[root@joker etc]# 

# 查看结果 ，镜像资源
[root@joker /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
mysql         5.7       450379344707   2 days ago    449MB
mysql         latest    cbe8815cbea8   2 days ago    546MB
hello-world   latest    d1165f221234   5 weeks ago   13.3kB
[root@joker /]# 

```

### docker rmi

- 删除镜像可以使用它的 image id , 或者 REPOSITORY:TAG  或者 digest

```bash
[root@joker /]# docker rmi --help

Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]

Remove one or more images

Options:
  -f, --force      Force removal of the image
      --no-prune   Do not delete untagged parents
[root@joker /]# 

```

常用命令

```bash
# 删除镜像 
docker rmi -f 镜像id # 删除单个 
docker rmi -f 镜像名:tag 镜像名:tag # 删除多个 
docker rmi -f $(docker images -qa) # 删除全部
```

例子：

```bash
[root@joker /]# docker rmi -f mysql:latest
Untagged: mysql:latest
Untagged: mysql@sha256:6e0014cdd88092545557dee5e9eb7e1a3c84c9a14ad2418d5f2231e930967a38
Deleted: sha256:cbe8815cbea8fb86ce7d3169a82d05301e7dfe1a8d4228941f23f4f115a887f2
Deleted: sha256:c74b92ab7fde96874c2f7afa77e338ebe739b829f5cb28a9975c9b0dcb47feb9
Deleted: sha256:fded7187915726c2d2d18c8178cd70ab9aceab27f49a68ead928a662664b9402
Deleted: sha256:217ef0e6aab8111068df664529c4bdcfc2b58701913028fd0d61b00265ad5a9b
Deleted: sha256:1ab4dbca7ef7a8eb6f7ea8ddd780b5d55aac2a0098f2c217c68e31216a2de140
Deleted: sha256:1fbdda78e87b76772be16bd4a745db7f95d9af70d5a3728260548380121ae711
[root@joker /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
mysql         5.7       450379344707   2 days ago    449MB
hello-world   latest    d1165f221234   5 weeks ago   13.3kB
[root@joker /]#
```



## 容器命令

- 注意：有了镜像，才可以创建容器。

### docker run 创建并启动

-  https://docs.docker.com/engine/reference/run/
- Docker runs processes in isolated containers. A container is a process which runs on a host.
- 就是创建并启动容器

```bash
[root@joker /]# docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

# 常用 options
--name string                    Assign a name to the container
--name="Name" # 给容器指定一个名字 ,比如tomcat9,tomcat10...
-d # 后台方式运行容器，并返回容器的id！ 
-i # 以交互模式运行容器，通过和 -t 一起使用 
-t # 给容器重新分配一个终端，通常和 -i 一起使用 
-it # 使用交互模运行容器，进入容器查看内容
-P # 随机端口映射（大写） 
-p # 指定端口映射（小结），一般可以有四种写法
	ip:hostPort:containerPort 
	ip::containerPort 
	hostPort:containerPort (常用) 
	containerPort

# 测试
# 启动并进入容器
[root@joker /]# docker run -it centos /bin/bash
[root@e9df806f7569 /]# ls # 注意这里 的路径已经变了。就是centos 镜像的id。就是另一个centos服务器。
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@e9df806f7569 /]# 
# 从容器退回主机
[root@e9df806f7569 /]#  exit
exit
[root@joker /]# ls
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  www
[root@joker /]# 

```

### docker ps 列举容器

- 查看正在运行的容器

```bash
[root@joker /]# docker ps --help

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display container IDs
  -s, --size            Display total file sizes
[root@joker /]#
```

```bash
[root@joker /]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# 常用参数说明 
-a # 列出当前所有正在运行的容器 + 历史运行过的容器 
-l # 显示最近创建的容器 
-n=? # 显示最近n个创建的容器 
-q # 静默模式，只显示容器编号。

[root@joker /]# docker ps -a # 查看曾经运行的容器。
CONTAINER ID   IMAGE         COMMAND       CREATED         STATUS                            PORTS     NAMES
e9df806f7569   centos        "/bin/bash"   3 minutes ago   Exited (130) About a minute ago             epic_tesla
6c9d13c812b6   hello-world   "/hello"      11 hours ago    Exited (0) 11 hours ago                     stupefied_stonebraker
b3746f12a78e   hello-world   "/hello"      11 days ago     Exited (0) 11 days ago                      zen_nash
5bc43152d45c   hello-world   "/hello"      11 days ago     Exited (0) 11 days ago                      elastic_moore
[root@joker /]# 

```

### 退出容器

- ctrl+d 等价于exit  停止容器并退出
- crtl+P+Q 退出容器但不停止容器 。（注意PQ是大写）

```bash
[root@joker /]# docker run -it centos /bin/bash
[root@dcb2f70725e6 /]# [root@joker /]# 
[root@joker /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS         PORTS     NAMES
dcb2f70725e6   centos    "/bin/bash"   10 seconds ago   Up 9 seconds             objective_buck
[root@joker /]#
```

### docker rm

- 删除容器

```bash
[root@joker /]# docker rm --help

Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers

Options:
  -f, --force     Force the removal of a running container (uses SIGKILL)
  -l, --link      Remove the specified link
  -v, --volumes   Remove anonymous volumes associated with the container
[root@joker /]# 

```

```bash
# 常用命令
docker rm 容器路径（就是容器的command ） 删除指定链接下的容器
docker rm 容器id # 删除指定容器,不能删除正在运行的。
docker rm -f 容器id #可以强制删除正在运行的容器
docker rm -f $(docker ps -aq) # 删除所有容器 
docker ps -a -q|xargs docker rm # 删除所有容器，这是通过管道符删除
docker rm $(docker ps --filter status=exited -q) # 删除所有已经停止的容器
```

### 启动和停止容器

```bash
docker start (容器id or 容器名) # 启动容器 
docker restart (容器id or 容器名) # 重启容器 
docker stop (容器id or 容器名) # 停止容器 
docker kill (容器id or 容器名) # 强制停止容器
```



## 常用其它命令

### 后台启动容器

- 也就是detached mode 启动容器

```bash
# 命令 
docker run -d 容器名 

# 例子 
docker run -d centos # 启动centos，使用后台方式启动 

# 问题： 使用docker ps 查看，发现容器已经退出了！ 
# 解释：Docker容器后台运行，就必须有一个前台进程，容器运行的命令如果不是那些一直挂起的命 令，就会自动退出。 
# 比如，你运行了nginx服务，但是docker前台没有运行应用，这种情况下，容器启动后，会立即自 杀，因为他觉得没有程序了，所以最好的情况是，将你的应用使用前台进程(foregroud)的方式运行启动。
```

### docker logs 查看日志

```bash
[root@joker /]# docker logs --help
Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
[root@joker /]# 
```

### docker top 查看容器中进程信息

```bash
[root@joker /]# docker top --help

Usage:  docker top CONTAINER [ps OPTIONS]

Display the running processes of a container
[root@joker /]#
```

### docker inspect （重点）

- 查看容器/镜像的元数据

```bash
[root@joker /]# docker inspect --help

Usage:  docker inspect [OPTIONS] NAME|ID [NAME|ID...]

Return low-level information on Docker objects

Options:
  -f, --format string   Format the output using the given Go template
  -s, --size            Display total file sizes if the type is container
      --type string     Return JSON for specified type
[root@joker /]#
```

### 进入正在运行的容器。

#### docker exec

```bash
[root@joker /]# docker exec --help

Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container

Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
      --env-file list        Read in a file of environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container
[root@joker /]# 

```

测试：

```bash
[root@joker /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
acba5df0c01b   centos    "/bin/bash"   25 seconds ago   Up 23 seconds             sharp_mcclintock
[root@joker /]# docker exec -it acba5df0c01b /bin/bash
[root@acba5df0c01b /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@acba5df0c01b /]# 
```



#### docker attach

```bash
[root@joker /]# docker attach --help

Usage:  docker attach [OPTIONS] CONTAINER

Attach local standard input, output, and error streams to a running container

Options:
      --detach-keys string   Override the key sequence for detaching a container
      --no-stdin             Do not attach STDIN
      --sig-proxy            Proxy all received signals to the process (default true)
[root@joker /]#
```



**exec 与attach 区别**

- exec 是在容器中打开新的终端，并且可以启动新的进程 

- attach 直接进入容器启动命令的终端，不会启动新的进程 



### docker cp

- 从容器内拷贝文件到主机

```bash
[root@joker /]# docker cp --help

Usage:  docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
	docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Copy files/folders between a container and the local filesystem

Use '-' as the source to read a tar archive from stdin
and extract it to a directory destination in a container.
Use '-' as the destination to stream a tar archive of a
container source to stdout.

Options:
  -a, --archive       Archive mode (copy all uid/gid information)
  -L, --follow-link   Always follow symbol link in SRC_PATH
[root@joker /]# 

```

测试

```bash
docker cp 容器id:容器内路径 目的主机路径

```

注意：拷贝是一个手动过程，未来，使用 -v 卷的技术，可以实现，容器与主机连通。

### docker stats

- 查看cpu的状态

```bash
[root@joker /]# docker stats --help

Usage:  docker stats [OPTIONS] [CONTAINER...]

Display a live stream of container(s) resource usage statistics

Options:
  -a, --all             Show all containers (default shows just running)
      --format string   Pretty-print images using a Go template
      --no-stream       Disable streaming stats and only pull the first result
      --no-trunc        Do not truncate output
[root@joker /]# 

```



```bash
# 如果直接使用 docker stats 它会一直实时刷新
[root@joker /]# docker stats --no-stream
CONTAINER ID   NAME               CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O         PIDS
d5dde9e68b80   tomcat9_01         0.11%     67.27MiB / 1.715GiB   3.83%     3.75kB / 4.53kB   11.8MB / 0B       29
401101f8ca83   nginx01            0.00%     3.164MiB / 1.715GiB   0.18%     674B / 1.27kB     6.13MB / 8.19kB   2
acba5df0c01b   sharp_mcclintock   0.00%     1.035MiB / 1.715GiB   0.06%     84B / 0B          0B / 0B           2

```



## 小结

![image-20210413153424812](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413153424812.png)

常用命令总结 docker 下

```bash
attach Attach to a running container # 当前 shell 下 attach 连接指定运行镜像 

build  Build an image from a Dockerfile # 通过 Dockerfile 定 制镜像 

commit Create a new image from a container changes # 提交当前容器为新的镜像 

cp Copy files/folders from the containers filesystem to the host path #从容器中拷贝指定文件或者目录到宿主机中 

create Create a new container # 创建一个新的容器，同 run，但不启动容器 

diff Inspect changes on a container's filesystem # 查看 docker 容器变化 

events Get real time events from the server # 从 docker 服务获取容 器实时事件 

exec Run a command in an existing container # 在已存在的容器上运行命 令

export Stream the contents of a container as a tar archive # 导出容器的内 容流作为一个 tar 归档文件[对应 import ] 

history Show the history of an image # 展示一个镜像形成历史 

images List images # 列出系统当前镜像 

import Create a new filesystem image from the contents of a tarball # 从tar包中的内容创建一个新的文件系统映像[对应export] 

info Display system-wide information # 显示系统相关信息 

inspect Return low-level information on a container # 查看容器详细信息 

kill Kill a running container # kill 指定 docker 容 器

load Load an image from a tar archive # 从一个 tar 包中加载一 个镜像[对应 save] 

login Register or Login to the docker registry server # 注册或者登陆一个 docker 源服务器 

logout Log out from a Docker registry server # 从当前 Docker registry 退出 

logs Fetch the logs of a container # 输出当前容器日志信息 

port Lookup the public-facing port which is NAT-ed to PRIVATE_PORT # 查看映射端口对应的容器内部源端口 

pause Pause all processes within a container # 暂停容器 

ps List containers # 列出容器列表 

pull Pull an image or a repository from the docker registry server # 从docker镜像源服务器拉取指定镜像或者库镜像 

push Push an image or a repository to the docker registry server # 推送指定镜像或者库镜像至docker源服务器 

restart Restart a running container # 重启运行的容器 

rm Remove one or more containers # 移除一个或者多个容器 

rmi Remove one or more images # 移除一个或多个镜像[无容器使用该 镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除] 

run Run a command in a new container # 创建一个新的容器并运行 一个命令 

save Save an image to a tar archive # 保存一个镜像为一个 tar 包[对应 load] 

search Search for an image on the Docker Hub # 在 docker hub 中搜 索镜像 

start Start a stopped containers # 启动容器 

stop Stop a running containers # 停止容器 

tag Tag an image into a repository # 给源中镜像打标签 

top Lookup the running processes of a container # 查看容器中运行的进程信息

unpause Unpause a paused container # 取消暂停容器 

version Show the docker version information # 查看 docker 版本号 

wait Block until a container stops, then print its exit code # 截取容 器停止时的退出状态值 
```

## 练习

### docker 安装nginx

注意：要打开，阿里云安全组，以及服务器防火墙的对应端口

端口映射：5200:80  

5200 是主机端口

80是容器端口

```bash
docker pull nginx
docker run -d --name nginx01 -p 5200:80 nginx

# 测试访问： 可以访问
[root@joker /]# curl localhost:5200

# 进入 nginx
[root@joker /]# docker exec -it nginx01 /bin/bash
root@401101f8ca83:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@401101f8ca83:/# cd /usr/lib/nginx
root@401101f8ca83:/usr/lib/nginx# ls
modules

```



### docker 安装 tomcat

```bash
[root@joker /]# docker pull tomcat:9.0
[root@joker /]# docker run -d --name tomcat9_01 -p 5201:8080 tomcat:9.0

# 测试访问,可以访问
[root@joker /]# curl localhost:5201

# 进入容器。
[root@joker /]# docker exec -it tomcat9_01 /bin/bash
root@d5dde9e68b80:/usr/local/tomcat# ls
BUILDING.txt	 LICENSE  README.md	 RUNNING.txt  conf  logs	    temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin	      lib   native-jni-lib  webapps  work
root@d5dde9e68b80:/usr/local/tomcat# ls -al
total 176
drwxr-xr-x 1 root root  4096 Apr 11 03:31 .
drwxr-xr-x 1 root root  4096 Apr 13 08:18 ..
-rw-r--r-- 1 root root 18984 Mar 30 10:29 BUILDING.txt
-rw-r--r-- 1 root root  5587 Mar 30 10:29 CONTRIBUTING.md
-rw-r--r-- 1 root root 57092 Mar 30 10:29 LICENSE
-rw-r--r-- 1 root root  2333 Mar 30 10:29 NOTICE
-rw-r--r-- 1 root root  3257 Mar 30 10:29 README.md
-rw-r--r-- 1 root root  6898 Mar 30 10:29 RELEASE-NOTES
-rw-r--r-- 1 root root 16507 Mar 30 10:29 RUNNING.txt
drwxr-xr-x 2 root root  4096 Apr 11 03:31 bin
drwxr-xr-x 1 root root  4096 Apr 13 08:18 conf
drwxr-xr-x 2 root root  4096 Apr 11 03:31 lib
drwxrwxrwx 1 root root  4096 Apr 13 08:18 logs
drwxr-xr-x 2 root root  4096 Apr 11 03:31 native-jni-lib
drwxrwxrwx 2 root root  4096 Apr 11 03:31 temp
drwxr-xr-x 2 root root  4096 Apr 11 03:31 webapps
drwxr-xr-x 7 root root  4096 Mar 30 10:29 webapps.dist
drwxrwxrwx 2 root root  4096 Mar 30 10:29 work
root@d5dde9e68b80:/usr/local/tomcat# cd webapps
root@d5dde9e68b80:/usr/local/tomcat/webapps# ls
root@d5dde9e68b80:/usr/local/tomcat/webapps# 
# 发现问题：1、linux命令少了，2、这里的webapps下面是空的
# 原因：阿里云镜像的原因，默认下载最小的镜像，不必要的都剔除
# 解决，它备份了一份在 webapps.dist 目录下，直接复制到webapps下就行
cp -r webapps.dist/* webapps


```

思考：我们以后要部署项目，还需要进入容器中，是不是十分麻烦，要是有一种技术，可以将容器 内和我们Linux进行**映射挂载**就好了？我们后面会将数据卷技术（volume）来进行挂载操作，也是一个核心内容，这 里大家先听听名词就好，我们很快就会讲到！



### docker 部署ES+Kibana

```bash
# 我们启动es这种容器需要考虑几个问题 
1、端口暴露问题,要暴露很多端口 9200、9300 
2、数据卷的挂载问题 data、plugins、conf 
3、吃内存，十分耗内存- "ES_JAVA_OPTS=-Xms512m -Xmx512m" 

# 扩展命令 
docker stats 容器id # 查看容器的cpu内存和网络状态 
# 1、启动es测试 
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-ode" elasticsearch:7.12.0
# 2、启动之后很卡，使用 docker stats 容器id 查看下cpu状态 ，发现占用的很大 
CONTAINER ID NAME CPU % MEM USAGE / LIMIT MEM % 
249ae46da625 elasticsearch 0.00% 1.036GiB / 1.716GiB 60.37% 
# 3、测试访问 
[root@kuangshen data]# curl localhost:9200

# 4、增加上内存限制启动 ,最小用64m，最大用512m
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

# 5、启动之后，使用 docker stats 查看下cpu状态 
CONTAINER ID NAME CPU % MEM USAGE / LIMIT MEM % 
d2860684e7e4 elasticsearch 0.24% 358.3MiB / 1.716GiB 20.40% 
# 6、测试访问，效果一样，ok！ 
[root@kuangshen data]# curl localhost:9200 

# 思考：如果我们要使用 kibana , 如果配置连接上我们的es呢？网络该如何配置呢？
```

![image-20210413170430130](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413170430130.png)



## 可视化

### portrainer（先用这个）

官网：https://www.portainer.io/

```bash
docker run -d -p 8088:9000 \ --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

访问：http://公网ip.8080/



### rancher(CI/CD再用)



# Docker 镜像讲解

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。



## Docker 镜像加载原理

### UnionFS（联合文件系统）

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual fifilesystem)。Union 文件系统是 Docker 镜像的基础。

镜像可以通过**分层**来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

**特性**：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

### docker 镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot fifile system)主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。 

rootfs (root fifile system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

![image-20210413215746924](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413215746924.png)

对于一个精简的OS，rootfs 可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。

## 分层理解

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载！



思考：为什么Docker镜像要采用这种分层的结构呢？

最大的好处，我觉得莫过于是**资源共享**了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过 docker image inspect 命令！



**理解：**

所有的 Docker 镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之

上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux 16.04 创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含 3 个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![image-20210413220105639](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413220105639.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。



下图中举了一个简单的例子，每个镜像层包含 3 个文件，而镜像包含了来自两个镜像层的 6 个文件。

![image-20210413220241200](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413220241200.png)

上图中的镜像层跟之前图中的略有区别，主要目的是便于展示文件。



下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有 6 个文件，这是因为最上层中的文件7 是文件 5 的一个更新版本。

![image-20210413220341289](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413220341289.png)

这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。

Docker 通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux 上可用的存储引擎有 AUFS、Overlay2、Device Mapper、Btrfs 以及 ZFS。顾名思义，每种存储引擎都基于 Linux 中对应的文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。

Docker 在 Windows 上仅支持 windowsfifilter 一种存储引擎，该引擎基于 NTFS 文件系统之上实现了分层和 CoW[1]。



下图展示了与系统显示相同的三层镜像。所有镜像层堆叠并合并，对外提供统一的视图。

![image-20210413220427858](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210413220427858.png)

**总结特点**

==Docker镜像都是只读的，当容器启动时，一个新的可写层（writable layer）被加载到镜像的顶部！这一层就是我们通常说的容器层，容器之下的都叫镜像层！==

## docker commit 提交镜像

docker commit 将容器创建为一个新的镜像

```bash
[root@joker ~]# docker commit --help

Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)
[root@joker ~]# 

```

测试

```bash
# 语法 
docker commit -m="提交的描述信息" -a="作者" 容器id 要创建的目标镜像名:[标签名]
```

可以使用tomcat测试，将webapps 补充好。再退出将容器打包为镜像。



# 容器数据卷

先了解 docker 数据持久化 https://docs.docker.com/get-started/05_persisting_data/

volume（数据卷） 部分：https://docs.docker.com/storage/volumes/



## 什么是容器数据卷

**总结一句话： 就是容器数据的持久化，以及容器间的继承和数据共享！**

自我理解：卷 其实就是，host 和容器挂载到同一个物理内存地址



**docker的理念回顾：**

将应用和运行的环境打包形成容器运行，运行可以伴随着容器，希望==数据可以持久化==

就好比，你安装一个MySQL，结果你把容器删了，就相当于删库跑路了，这TM也太扯了吧！

所以我们希望容器之间有可能可以共享数据，Docker容器产生的数据，如果不通过docker commit 生成新的镜像，使得数据作为镜像的一部分保存下来，那么当容器删除后，数据自然也就没有了！这样是行不通的！

为了能保存数据在Docker中我们就可以使用卷！让数据**挂载**到我们本地！这样数据就不会因为容器删除而丢失了！

**作用：**

**卷**就是目录或者文件，存在一个或者多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过 Union File System ， 提供一些用于持续存储或共享数据的特性。

卷的设计**目的**就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷。

**特点：**

1、数据卷可在容器之间共享或重用数据

2、卷中的更改可以直接生效

3、数据卷中的更改不会包含在镜像的更新中

4、数据卷的生命周期一直持续到没有容器使用它为止

 

## 使用 volume

```bash
[root@joker ~]# docker volume --help

Usage:  docker volume COMMAND

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

Run 'docker volume COMMAND --help' for more information on a command.
[root@joker ~]#

[root@joker ~]# docker volume create --help

Usage:  docker volume create [OPTIONS] [VOLUME]

Create a volume

Options:
  -d, --driver string   Specify volume driver name (default "local")
      --label list      Set metadata for a volume
  -o, --opt map         Set driver specific options (default map[])
[root@joker ~]#
```

### 方式一: -v

- 直接使用命令来挂载 -v

```bash
# 命令
docker run -it -v 宿主机绝对路径目录:容器内目录 镜像名

# 测试 /home/v_test 是host目录 /home 是容器目录
[root@joker home]# docker run -it -v /home/v_test:/home centos:latest /bin/bash
[root@f3f6c9322119 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@f3f6c9322119 /]# [root@joker home]# 
[root@joker home]# ls
joker  joker.txt  justin_chen  v_test
[root@joker home]#ome]# docker run -it -v /home/v_test:/home cestos /bin/bash

# 查看挂载
[root@joker home]# docker inspect f3f6c9322119
# 其中 有挂载（mounts）信息
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/v_test",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
             }
],
```

测试改变数据：

发现，不论是，container 不关，还是关闭了，再打开，在host 卷目录下的文件修改，都可以在container 里一样。反之亦然。

​	 

### 实战： mysql 数据持久化 

- 之前我已经安装了mysql

这里主要考虑 mysql 数据持久化

```bash
# 注意 MySQL 的连接需要设置密码。参考docker hub 的使用方法
#docker hub 的启动mysql : 
#$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

[root@joker home]# docker run -dp 5210:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
fa75247e9026282c27e4cc1a46595a09c027957ff8eed00a79551fe3c853a87a
[root@joker home]# 

#测试 启动成功后，此时再通过本地 sqlyog 连接服务器的数据库。注意，我的端口是5210 .连接成功
# 在本地创建一个数据库，发现服务器对应数据目录（/home/mysql/data）也会增加相应目录
```

![image-20210414132800777](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210414132800777.png)



注意：如果访问出现了 cannot open directory: Permission denied

解决办法：在挂载目录后多加一个 --privileged=true参数即可



### 具名和匿名挂载

官方文档详情：https://docs.docker.com/storage/#more-details-about-mount-types

具名（named）：一般都是用具名挂载，即用有名字的 volume

匿名（**anonymous**）:就是一开始没有给挂载点名字，docker 就会随机给它分配一串名字。

注意点：只要是路径，就肯定是 / 开头；是卷名（volume）就是没有 / 

```bash
# 匿名挂载 
-v 容器内路径 
docker run -d -P --name nginx01 -v /etc/nginx nginx 
# 匿名挂载的缺点，就是不好维护，通常使用命令 docker volume维护 
docker volume ls

# 具名挂载 
-v 卷名:容器内路径 
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx nginx 
# 查看挂载的路径
[root@kuangshen ~]# docker volume inspect nginx

# 怎么判断挂载的是卷名而不是本机目录名？ 
不是/开始就是卷名，是/开始就是目录名 
# 改变文件的读写权限 
# ro: readonly 
# rw: readwrite 
# 指定容器对我们挂载出来的内容的读写权限 
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:ro nginx 
docker run -d -P --name nginx02 -v nginxconfig:/etc/nginx:rw nginx
```

- dockers 默认容器的卷，目录：/var/lib/docker/volumes/

```bash
[root@joker docker]# ls
buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes
[root@joker docker]# pwd
/var/lib/docker
[root@joker docker]# 

```

- 平时都是使用具名挂载，方便找到。

**总结：**

- 指定路径挂载：-v host路径：容器内路径
- 具名挂载 -v volume名：容器内路径
- 匿名挂载 -v 容器内路径



### 初始 DockerFile



## 数据卷容器

官方文档 --volumes-from 的地方

- https://docs.docker.com/engine/reference/commandline/run/#mount-volumes-from-container---volumes-from
- https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes
- --volumes-from 实现容器间的数据共享

数据卷容器：就是挂载数据卷的容器（把容器当作数据卷）。命名的容器 挂载数据卷，其他容器通过挂载这个（父容器）实现数据共享。

![image-20210414204229148](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210414204229148.png)

主要命令：--volumes-from

测试：，建立容器docker01，docker02,docker03。02 和03都挂载在01卷，如果把容器01删除了。发现02，03，依旧可以访问01

因为，用docker inspect 检查，发现，三个挂载点，都映射在主机的同一个文件。

**结论：**

**容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止。**

**存储在本机的文件则会一直保留！**





# DockerFile

## 什么是 DockerFile

DockerFile ： https://docs.docker.com/engine/reference/builder/

dockerfile是用来**构建Docker镜像的构建文件**，是由一系列命令和参数构成的脚本。



构建步骤：

1、编写DockerFile文件

2、docker build 构建镜像

3、docker run 运行镜像

4、docker push 发布镜像（DockerHub，阿里云镜像仓库）



可以查看官方的DockerFile 怎么写的。

比如，在dockerhub 搜索centos,点击版本标签，就会跳转到对应的 DockerFile 

比如：https://github.com/CentOS/sig-cloud-instance-images/blob/b2d195220e/docker/Dockerfile

很多官方镜像都是基础包，很多功能都没有，我们通常会搭建自己的镜像。



- DockerFile 是面向开发的， 我们以后要发布项目，做镜像，就要编写 DockerFile 文件，但是，这个很简单。

## DockerFile 构建过程

**基础知识：**

1、每条保留字指令都必须为大写字母，且后面要跟随至少一个参数

2、指令按照从上到下，顺序执行

3、# 表示注释

4、每条指令都会创建一个新的镜像层，并对镜像进行提交

理解图：

![image-20210414212636767](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210414212636767.png)

**dockerfile 执行流程：**

1、docker从基础镜像运行一个容器

2、执行一条指令并对容器做出修改

3、执行类似 docker commit 的操作提交一个新的镜像层

4、Docker再基于刚提交的镜像运行一个新容器

5、执行dockerfile中的下一条指令直到所有指令都执行完成！

**说明：**

从应用软件的角度来看，DockerFile，docker镜像与docker容器分别代表软件的三个不同阶段。

DockerFile 是软件的原材料，构建文件，定义 了一切的步骤，源代码 （代码）

Docker 镜像则是软件的交付品 （.apk）

Docker 容器则是软件的运行状态 （客户下载安装执行）

DockerFile 面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可！



![image-20210414211934374](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210414211934374.png)

三者的理解：

DockerFile：需要定义一个DockerFile，DockerFile定义了进程需要的一切东西。DockerFile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程（当引用进行需要和系统服务和内核进程打交道，这时需要考虑如何设计 namespace的权

限控制）等等。

Docker镜像：在DockerFile 定义了一个文件之后，Docker build 时会产生一个Docker镜像，当运行Docker 镜像时，会真正开始提供服务；

Docker容器：容器是直接提供服务的。



## DockerFile 指令

指令（instruction）,

```bash
FROM 		# 基础镜像，当前新镜像是基于哪个镜像的  

RUN 		# 容器构建时需要运行的命令 

EXPOSE 		# 当前容器对外保留出的端口 

WORKDIR 	# 指定在创建容器后，终端默认登录的进来工作目录，一个落脚点 

ENV 		# 用来在构建镜像过程中设置环境变量 

ADD 		# 将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包 
COPY 		# 类似ADD，拷贝文件和目录到镜像中！ 

VOLUME 		# 容器数据卷，用于数据保存和持久化工作 

CMD 		# 指定一个容器启动时要运行的命令，dockerFile中可以有多个CMD指令，但只有最 后一个生效！ 
ENTRYPOINT 	# 指定一个容器启动时要运行的命令！和CMD类似，但，这个是追加命令

ONBUILD 	# 当构建一个被继承的DockerFile时运行命令，父镜像在被子镜像继承后，父镜像的 ONBUILD被触发
```

## 实战：构建自己的 centos

- Docker Hub 中99% 的镜像都是通过在base镜像（Scratch）中安装和配置需要的软件构建出来的
- 因为可以发现官方的centos镜像，很多命令都么有，比如vim,ifconfig 等。



1、新建一个目录 my_dockerfile ,新建一个文件 mycentos_dockerfile

```bash
[root@joker my_dockerfile]# pwd
/home/my_dockerfile
[root@joker my_dockerfile]# vim mycentos_dockerfile
[root@joker my_dockerfile]# ls
mycentos_dockerfile
[root@joker my_dockerfile]# 

```

2、编写文件mycentos_dockerfile 

```bash
FROM centos
LABEL maintainer="joker@qq.com"
ENV MY_PATH=/usr/local
WORKDIR $MY_PATH
RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 5200

CMD /bin/bash


```



3、构建镜像 docker build 

语法：docker build -f dockerfile地址 -t 新镜像名字:TAG .     

==注意最后有一个点，表示上下文目录（当前目录）==

```bash
[root@joker my_dockerfile]# docker build -f mycentos_dockerfile -t mycentos:1.0 .

# 构建完，会提示 成功
。。。。。。
Successfully built 75e331de89b5
Successfully tagged mycentos:1.0
```

4、运行 

docker run -it 新镜像名字:TAG

```bash
[root@joker my_dockerfile]# docker run -it mycentos:1.0
[root@4606643a3bed local]# pwd
/usr/local
[root@4606643a3bed local]# vim
[root@4606643a3bed local]# vim test
[root@4606643a3bed local]# 
```



5、测试，发现，工作目录变了，vim也可以用了

6、可以用 docker history 镜像名，列出镜像的构建历史。

```bash
[root@joker my_dockerfile]# docker history mycentos:1.0
IMAGE          CREATED              CREATED BY                                      SIZE      COMMENT
7f12722a6f7a   About a minute ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B        
545801627d06   11 minutes ago       /bin/sh -c #(nop)  EXPOSE 5200                  0B        
3e1e46040fc7   11 minutes ago       /bin/sh -c yum -y install net-tools             23.3MB    
561c961314c3   11 minutes ago       /bin/sh -c yum -y install vim                   58MB      
39048af4d9ad   12 minutes ago       /bin/sh -c #(nop) WORKDIR /usr/local            0B        
5a18bcff85b3   12 minutes ago       /bin/sh -c #(nop)  ENV MY_PATH=/usr/local       0B        
26d631bd538c   12 minutes ago       /bin/sh -c #(nop)  LABEL maintainer=joker@qq…   0B        
300e315adb2f   4 months ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      4 months ago         /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      4 months ago         /bin/sh -c #(nop) ADD file:bd7a2aed6ede423b7…   209MB     
[root@joker my_dockerfile]# 
```

- 所以，可以看一下其它官方的镜像是怎么构建的



## CMD 与 ENTRYPOINT 的区别

**CMD**：Dockerfifile 中可以有多个CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数

替换！

**ENTRYPOINT**： docker run 之后的参数会被当做参数传递给 ENTRYPOINT，之后形成新的命令组合！

### CMD 测试

CMD https://docs.docker.com/engine/reference/builder/#cmd

类似于 RUN 指令，用于运行程序，但二者运行的时间点不同:

+ CMD 在docker run 时运行。
+ RUN 是在 docker build。

**作用**：为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所**覆盖**。



### ENTRYPOINT 测试

ENTRYPOINT  https://docs.docker.com/engine/reference/builder/#entrypoint

类似于 CMD 指令，但其**不会被** docker run 的命令行参数指定的指令所**覆盖**，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序。就是**追加在 ENTRYPOINT 的参数里**。

但是, 如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 CMD 指令指定的程序。

**优点**：在执行 docker run 的时候可以指定 ENTRYPOINT 运行所需的参数。

**注意**：如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。



## 实战：构建自己的tomcat 镜像

1、 新建文件夹mytomcat ,准备好tomcat 与jdk的镜像。/home/joker/mytomcat

2、在上述目录下 touch read.txt

3、将 JDK 和 tomcat 安装的压缩包拷贝进上一步目录

4、在/home/joker/mytomcat 目录下新建一个 Dockerfifile 文件,一般还会新建一个 readme.txt文件。

 ```bash
[root@joker mytomcat]# pwd
/home/joker/mytomcat
[root@joker mytomcat]# ll
total 186524
-rw-r--r-- 1 root root  11487016 Apr 15 00:58 apache-tomcat-9.0.44.tar.gz
-rw-r--r-- 1 root root 179505388 Apr 15 00:56 jdk-8u221-linux-x64.rpm
[root@joker mytomcat]# 
 ```

编写Dockerfile 文件，官方命名就是 Dockerfile  ，docker build 会在目录下自动寻找这个文件（就不用加 -f ）

```bash
# vim Dockerfile 
FROM centos 
MAINTAINER kuangshen<24736743@qq.com> 

#把宿主机当前上下文的readme.txt拷贝到容器/usr/local/路径下 
COPY readme.txt /usr/local/readme.txt 
#把java与tomcat添加到容器中 ADD 命令会自动解压
ADD jdk-8u221-linux-x64.tar.gz /usr/local/ 
ADD apache-tomcat-9.0.22.tar.gz /usr/local/ 
#安装vim编辑器 
RUN yum -y install vim 
#设置工作访问时候的WORKDIR路径，登录落脚点 
ENV MYPATH /usr/local WORKDIR $MYPATH 
#配置java与tomcat环境变量 
ENV JAVA_HOME /usr/local/jdk1.8.0_221 
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar 
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.44 
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.44 
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin 
#容器运行时监听的端口 
EXPOSE 8080 
#启动时运行tomcat # 
ENTRYPOINT ["/usr/local/apache-tomcat-9.0.44/bin/startup.sh" ] 
# 
CMD ["/usr/local/apache-tomcat-9.0.44/bin/catalina.sh","run"] 
CMD /usr/local/apache-tomcat-9.0.44/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.44/bin/logs/catalina.out
```

docker build 构建镜像

```bash
docker build -t mytomcat
```

docker run 新建运行启动容器

```bash
docker run -d -p 5200:8080 --name mytomcat \
-v /home/joker/mytomcat/test:/usr/local/apache-tomcat- 9.0.44/webapps/test \
-v /home/joker/mytomcat/tomcat9logs/:/usr/local/apache-tomcat- 9.0.22/logs \
--privileged=true mytomcat
```

测试访问 ,ok

```bash
curl localhost:5200
```

就可以发布web项目了，因为做了卷挂载，在本地目录写项目就可以访问了。在test下添加web项目。快速构建的花，就是在test下新建一个WEB-INF ，在添加内容（随便一个web.xml）就行。

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?> 

<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xmlns="http://java.sun.com/xml/ns/javaee" 
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
							http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" 
	id="WebApp_ID" 
    version="2.5"> 
    
    
</web-app>
```

test.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%> 

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"> 
<html> 
<head> 
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8"> 
<title>joker_mytomcat</title> 
</head> 
<body> 
mytomcat
<% 
    System.out.println("-------my docker tomcat-------"); 
%> 
</body> 
</html> 
```



- 之后的一切都是用docker镜像来发布。

## 发布镜像

### 发布到 Docker Hub

1. 注册dockerhub https://hub.docker.com/signup，需要有一个账号
2. 在本地，docker login，docker push，就和git差不多。
3. 自己发布的镜像最好加上版本号。



### 发布到 阿里云镜像

1. 登录阿里云账号
2. 容器镜像服务-》创建一个命名空间-》镜像仓库，然后点进去那个个仓库，跟着操作就好了



# Docker 小结

![image-20210415020437028](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415020437028.png)



再往下学：就是更加偏向**运维**级别的了。

# Docker 网络讲解

Docker 网络 https://docs.docker.com/network/

## 理解 Docker0

准备工作：清空所有的容器，清空所有的镜像

```bash
docker rm -f $(docker ps -a -q) # 删除所有容器 
docker rmi -f $(docker images -qa) # 删除全部镜像
```



**测试**

查看本地ip 

```bash

[root@joker /]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:21:58:d6 brd ff:ff:ff:ff:ff:ff
    inet 172.24.73.169/20 brd 172.24.79.255 scope global dynamic eth0
       valid_lft 314080296sec preferred_lft 314080296sec
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:d5:ae:80:82 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
[root@joker /]# 

```

发现有三个网络

```bash
lo 127.0.0.1 # 本机回环地址 
eth0 172.24.73.169 # 阿里云的私有IP 
docker0 172.17.0.1 # docker网桥 

# 问题：Docker 是如何处理容器网络访问的？
```

![image-20210415150456703](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415150456703.png)



我们之前安装ES的时候，留过一个问题，就是安装Kibana的问题，Kibana得指定ES的地址！或者我们实际场景中，我们开发了很多微服务项目，那些微服务项目都要连接数据库，需要指定数据库的url地址，通过ip。但是我们用Docker管理的话，假设数据库出问题了，我们重新启动运行一个，这个时候数据库的地址就会发生变化，docker会给每个容器都分配一个ip，且容器和容器之间是可以互相访问的。

我们可以测试下容器之间能不能ping通过：

```bash
# 查看tomcat01的ip地址，发现启动时会得到一个eth0@if45 ，就是docker分配的
# docker 会给每个容器都分配一个ip！
[root@joker /]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
44: eth0@if45: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@joker /]# 

# 思考，我们的linux服务器是否可以ping通容器内的tomcat ？可以的
[root@joker /]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.096 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.064 ms

```

> 原理

1、每一个安装了Docker的linux主机都有一个**docker0**的虚拟网卡。这是个默认的桥接（bridge）网卡，使用了veth-pair技术！

```bash
# 再次查看ip,发现启动tomcat后，多了一个网络
[root@joker /]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:21:58:d6 brd ff:ff:ff:ff:ff:ff
    inet 172.24.73.169/20 brd 172.24.79.255 scope global dynamic eth0
       valid_lft 314077082sec preferred_lft 314077082sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:d5:ae:80:82 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
45: vetha15b0ee@if44: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 7a:1a:93:5e:a4:78 brd ff:ff:ff:ff:ff:ff link-netnsid 0
[root@joker /]# 
```

2、每启动一个容器，linux主机就会多了一对虚拟网卡。

```bash
# 我们启动了一个tomcat01，主机的ip地址多了一个 123: vethc8584ea@if122 
# 然后我们在tomcat01容器中查看容器的ip是 122: eth0@if123 
# 我们再启动一个tomcat02观察 
[root@joker ~]# docker run -dP --name tomcat02 tomcat 

# 然后发现linux主机上又多了一个网卡 125: veth021eeea@if124:
# 我们看下tomcat02的容器内ip地址是 124: eth0@if125: 
[root@joker ~]# docker exec -it tomcat02 ip addr 

# 观察现象： 只要启动一个容器，就有一对网卡 

# veth-pair 就是一对的虚拟设备接口，它都是成对出现的。一端连着协议栈，一端彼此相连着。 
# 正因为有这个特性，它常常充当着一个桥梁，连接着各种虚拟网络设备!
# “Bridge、OVS 之间的连接”，“Docker 容器之间的连接” 等等，以此构建出非常复杂的虚拟网络 结构，比如 OpenStack Neutron。
```

3、我测试下tomcat01和tomcat02容器间是否可以互相ping通 .发现是可以ping通。



docker0 的网络模型理解：

![image-20210415161347677](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415161347677.png)



**结论：**tomcat1和tomcat2共用一个路由器。就是docker0。任何一个容器启动默认都是docker0网络。

所有的容器不指定网络的情况下，都是docker0路由的，docker默认会给容器分配一个可用ip。 

docker 最多大概分配65535个。

> 小结

Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥(docker0)，Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，称为Container-IP，同时Docker网桥是每个容器的默认网关。因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接通信。

![image-20210415161928799](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415161928799.png)

Docker容器网络就很好的利用了Linux虚拟网络技术，在本地主机和容器内分别创建一个虚拟接口，并让他们彼此联通（这样一对接口叫veth pair）；

Docker中的网络接口默认都是**虚拟**的接口。虚拟接口的优势就是转发效率极高（因为Linux是在内核中进行数据的复制来实现虚拟接口之间的数据转发，无需通过外部的网络设备交换），对于本地系统和容器系统来说，虚拟接口跟一个正常的以太网卡相比并没有区别，只是他的速度快很多。

只要容器删除，对应的网桥就没了。



> 思考

思考一个场景，我们编写一个微服务，数据库连接地址原来是使用ip的，如果ip变化就不行了，那我们能不能使用服务名访问呢？

比如 jdbc:mysql://mysql:3306，这样的话哪怕mysql重启，我们也不需要修改配置了！docker提供了 --link的操作！

## --link(遗弃了)

https://docs.docker.com/network/links/

## docker network create 自定义网络

自定义bridge网络 https://docs.docker.com/network/network-tutorial-standalone/#use-user-defined-bridge-networks

```bash
[root@joker /]# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
[root@joker /]# 

```

**查看所有网络**

==docker network ls==

```bash
[root@joker /]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
2d54d1c97a7a   bridge    bridge    local
803498a2e87f   host      host      local
79f8ad6d6f9b   none      null      local
[root@joker /]# 
```

### network drivers 网络驱动模式

https://docs.docker.com/network/#network-drivers

![image-20210415163846982](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415163846982.png)

| 网络驱动模式 | 配置                    | 说明                                                         |
| ------------ | ----------------------- | ------------------------------------------------------------ |
| bridge       | --net=bridge            | 默认值，在Docker网桥docker0上为容器创建新的网络栈            |
| host         | --net=host              | 不配置网络，用户可以稍后进入容器，自行配置                   |
| container    | --net=container:name/id | 容器和另外一个容器共享Network namespace。kubernetes中的pod就是多个容器共享一个Network namespace。 |
| none         | --net=none              | 容器和宿主机共享Network namespace                            |
| 用户自定义   | --net=自定义网络        | 用户自己使用network相关命令定义网络，创建容器的              |

使用最多的是用户自定义的 bridge networks

### Network driver summary

+ **User-defined bridge networks** are best when you need multiple containers to communicate on the same Docker host.
+ **Host networks** are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.
+ **Overlay networks** are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.
+ **Macvlan networks** are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.
+ **Third-party network plugins** allow you to integrate Docker with specialized network stacks.



### 测试

docker0是默认的路由，不能通过服务名访问，

这里自己用create 创建一个网络。就是自定义一个网卡，代替docker0

#### docker network create

```bash
[root@joker /]# docker network create --help

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
[root@joker /]#
```

创建自定义网络 mynet,必须要配置的 --subnet ， --gateway，不写，就是默认再docker0的加1，比如172.18.0.1

```bash
[root@joker /]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

```

使用自己的网络

```bash
 docker run -d -P --name tomcat-net-01 --net mynet tomcat
 docker run -d -P --name tomcat-net-02 --net mynet tomcat
 # 查看详细信息
 docker network inspect mynet
 # ping服务名，是可以ping通的
 docker exec -it tomcat-net-01 ping tomcat-net-02
```



使用 docker network inspect 查看网络的详细信息。



总结：自定义的bridge 网络，修复了，docker0的缺点，不用--link也可以ping 容器名。

- 平时都用自定义的网络



## docker network connect 网络连通

network connect https://docs.docker.com/engine/reference/commandline/network_connect/

- 不通的网络之间连通，比如docker0 与 mynet 之间。
- 想要实现，如下图，tomcat1 可以访问tomcat-net-01,就需要tomcat 连通到 mynet

![image-20210415171944453](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day30-Docker.assets/image-20210415171944453.png)



docker0和自定义网络肯定不通，我们使用自定义网络的好处就是网络隔离：

大家公司项目部署的业务都非常多，假设我们有一个商城，我们会有订单业务（操作不同数据），会有订单业务购物车业务（操作不同缓存）。如果在一个网络下，有的程序猿的恶意代码就不能防止了，所以我们就在部署的时候网络隔离，创建两个桥接网卡，比如订单业务（里面的数据库，redis，mq，全部业务 都在order-net网络下）其他业务在其他网络。



```bash
[root@joker /]# docker network connect --help

Usage:  docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container
[root@joker /]# 
```

测试，容器tomcat01连接 mynet 网络,

```bash
[root@joker /]# docker network connect mynet tomcat01

# 检查mynet,发现直接加了tomcat01进来
[root@joker /]# docker network inspect mynet
# 限制tomcat01 就拥有了双ip,

# ping 测试访问,就ok
[root@joker /]# docker exec -it tomcat01 ping tomcat-net-01
```



结论：如果要跨网络操作别人，就需要使用 docker network connect [OPTIONS] NETWORK CONTAINER 连接



## 实战：部署 Redis 集群

- 建立三个主机，三个从机

```shell
# 创建网卡 
docker network create redis --subnet 172.38.0.0/16 
# 通过脚本创建六个redis配置 
for port in $(seq 1 6); \ 
do \ 
mkdir -p /mydata/redis/node-${port}/conf 
touch /mydata/redis/node-${port}/conf/redis.conf 
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf 
port 6379 
bind 0.0.0.0
cluster-enabled yes 
cluster-config-file nodes.conf 
cluster-node-timeout 5000 
cluster-announce-ip 172.38.0.1${port} 
cluster-announce-port 6379 
cluster-announce-bus-port 16379 
appendonly yes 
EOF 
done
# 通过脚本启动6个容器，也可以一个一个启动
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \ 
-v /mydata/redis/node-${port}/data:/data \ 
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \ 
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server/etc/redis/redis.conf

# 进入一个redis，注意这里是 sh命令 
docker exec -it redis-1 /bin/sh

# 创建集群 
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 -- cluster-replicas 1

# 连接集群 
redis-cli -c 
# 查看集群信息 
cluster info 
# 查看节点 
cluster nodes 
# 在集群里，存一个值。
set a b 
# 停止存值的容器 ，docker stop containername
# 然后再次get a，发现依旧可以获取值 ,就是从关掉上一个的主机的从机成为主机，里获取的。
# 查看节点，发现高可用完全没问题
```



# IDEA 整合 Docker

- 这里是一个 实例：SpringBoot 微服务打包成 Docker 镜像

1. 构建 SpringBoot 项目
2. 打包应用 。jar 包
3. 编写 Dockerfile
4. 服务器 新建一个目录，用来存放jar包，与 Dockerfile
5. 通过 Dockerfile 构建镜像 docker build
6. docker run 运行。
7. 可以发布试试。



简单的 Dockerfile 文件如下

```bash
FROM java:8 

# 服务器只有dockerfile和jar在同级目录 
COPY *.jar /app.jar 

CMD ["--server.port=8080"] 

# 指定容器内要暴露的端口 
EXPOSE 8080 

ENTRYPOINT ["java","-jar","/app.jar"]
```



# 学习分界线 

- 前面，就是玩单机，
- 后面就是docker 的企业级开发，
- 后面至少需要6台 服务器



# Dokcer Compose

- Dokcer Compose 文档 https://docs.docker.com/compose/
- Dokcer Compose 作用：轻松高效的管理多个容器，定义运行多个容器。

# Docker Swarm

- 学这个是为了之后学 kurbenetes (即 k8s)

# CI/CD 之 Jenkins

