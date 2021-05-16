# Day31-Nginx

2020-04-16

学习视频：狂神说

Nginx 学习中文网：https://www.nginx.cn/nginx-how-to



# 什么是 Nginx

Nginx是俄罗斯人Igor Sysoev编写的轻量级Web服务器，它不仅是一个**高性能的HTTP和反向代理服务器**，同时也是一个**IMAP/POP3/SMTP 代理服务器**（邮件服务）。



**特点**：

是占有内存少，并发能力强，事实上 nginx 的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。在全球活跃的网站中有12.18%的使用比率，大约为2220万个网站。

Nginx 是一个安装非常的简单、配置文件非常简洁（还能够支持perl语法）、Bug非常少的服务。Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够不间断服务的情况下进行软件版本的升级。

Nginx 代码完全用**C语言**从头写成。官方数据测试表明能够支持高达 50,000 个并发连接数的响应。

nginx 下载地址 http://nginx.org/en/download.html



# Nginx 作用

## Http 代理，反向代理

- 作为web服务器最常用的功能之一，尤其是反向代理。

> 正向代理

代理客户端的。

比如，VPN

![image-20210416005211842](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416005211842.png)

> 反向代理

代理服务器的。

对于客户端而言，永远访问的是一个域名，比如，www.baidu.com

![image-20210416005236524](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416005236524.png)



## 负载均衡

Nginx提供的负载均衡策略有2种：内置策略和扩展策略。

- 内置策略为轮询，加权轮询，Ip hash。

- 扩展策略，就天马行空，只有你想不到的没有他做不到的。

**轮询**

![image-20210416005753754](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416005753754.png)

**加权轮询**

![image-20210416005818339](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416005818339.png)

iphash对客户端请求的ip进行hash操作，然后根据hash结果将同一个客户端ip的请求分发给同一台服务器进行处理，可以解决session不共享的问题。

但是不建议使用 Nginx 来实现 session 共享，因为，一个用户信息都在一台服务器，丢了就丢了。建议使用 Redis 实现 session 共享。



## 动静分离

在我们的软件开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等等文件），这些不需要经过后台处理的文件称为静态文件。让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作。提高资源响应的速度。

![image-20210416010317326](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416010317326.png)

目前，通过使用Nginx大大提高了网站的响应速度，优化了用户体验，让网站的健壮性更上一层楼！



# Nginx 安装

## windows 安装

- 我没学

1、下载 解压

https://nginx.org/en/download.html 下载稳定的windows 版本，比如[ nginx/Windows-1.18.0](https://nginx.org/download/nginx-1.18.0.zip)，解压到一个没有中文的目录

**2、启动 Nginx:**

有很多种方法启动nginx

(1)直接双击nginx.exe，双击后一个黑色的弹窗一闪而过

(2)打开cmd命令窗口，切换到nginx解压目录下，输入命令 `nginx.exe` ，回车即可

**3、检查nginx是否启动成功**

直接在浏览器地址栏输入网址 [http://localhost:80](http://localhost/) 回车，出现welcome 页面说明启动成功！

**4、查看配置文件**

nginx的配置文件是conf目录下的nginx.conf，默认配置的nginx监听的端口为80，如果80端口被占用可以修改为未被占用的端口即可。

当我们修改了nginx的配置文件nginx.conf 时，不需要关闭nginx后重新启动nginx，只需要执行命令 `nginx -s reload` 即可让改动生效

**5、关闭nginx**

如果使用cmd命令窗口启动nginx， 关闭cmd窗口是不能结束nginx进程的，可使用两种方法关闭nginx

(1)输入nginx命令 `nginx -s stop`(快速停止nginx) 或 `nginx -s quit`(完整有序的停止nginx)

(2)使用taskkill `taskkill /f /t /im nginx.exe`

1. `taskkill是用来终止进程的，`
2. `/f是强制终止 .`
3. `/t终止指定的进程和任何由此启动的子进程。`
4. `/im示指定的进程名称 .`

## Linux 安装

- 最快的安装，当然使用 docker 安装了。直接拉取，创建容器，就可以访问 了。
- 不了解docker，就用下面的解压缩安装。

Nginx 下载稳定版本 ，比如：[ nginx-1.18.0](https://nginx.org/download/nginx-1.18.0.tar.gz)

**1、安装gcc**

安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：

```
yum install gcc-c++
```

**2、PCRE pcre-devel 安装**

PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：

```
yum install -y pcre pcre-devel
```

**3、zlib 安装**

zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。

```
yum install -y zlib zlib-devel
```

**4、OpenSSL 安装**
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。

```
yum install -y openssl openssl-devel
```

**5、下载安装包**

手动下载.tar.gz安装包，地址：https://nginx.org/en/download.html

**6、上传到服务器**

下载完毕上传到服务器上 /root 目录，并解压

```bash
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
```

**7、配置**

使用默认配置，在nginx根目录下执行

```
./configure
make
make install
```

查找安装路径： `whereis nginx`

**8、启动**

查找安装路径： `whereis nginx`



```bash
cd /usr/local/nginx
# 有个sbin ，一般放置执行文件
cd sbin/

./nginx
# 啥都没有，就是启动成功了

```

9、测试，直接访问 公网IP:80就可以了



# Nginx 常用命令

```bash
cd /usr/local/nginx/sbin/
./nginx  启动
./nginx -s stop  停止
./nginx -s quit  安全退出
./nginx -s reload  重新加载配置文件
ps aux|grep nginx  查看nginx进程
```

启动成功访问 服务器ip:80

注意：如果连接不上，检查阿里云安全组是否开放端口，或者服务器防火墙是否开放端口！
相关命令：

```bash
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

# Nginx 实战

两个不同的端口，启动两个相同的jar 包服务。



可以查看nginx.conf 文件。

默认组成：

- 全局配置
- events 配置
- http 配置
  - http 的全局配置
  - server 配置

理解图：

![image-20210416013559927](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day31-Nginx.assets/image-20210416013559927.png)



添加配置如下

upstream 负载均衡

location 用于反向代理

```bash
upstream joker_loadbalance{
    server 127.0.0.1:8080 weight=1;
    server 127.0.0.1:8081 weight=1;
}
# location 的位置，可以看原配置文件，添加如下即可。
location / {
    proxy_pass http://joker_loadbalance;
}
```

之后再重新加载启动 nginx

访问