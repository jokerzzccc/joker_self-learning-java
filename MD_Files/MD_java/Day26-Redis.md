Day26-Redis

时间：2021-03-29

学习视频：狂神说

之前要学习：git,linux,云端服务器，mybatis-plus	

学习顺序：

- nosql 讲解
- 阿里巴巴架构演讲
- NoSql 数据模型
- NoSql 四大分类
- CAP
- BASE
- Redis 入门
- Redis 安装
- 五大基本数据类型
  - String
  - List
  - Set
  - Hash
  - Zset
- 三种特殊数据类型
  - geo
  - hyperloglog
  - bitmap
- Redis 配置详解
- Redis 持久化
  - RDB
  - AOF
- Redis 事务操作
- Redis 实现订阅发布
- Redis 主从复制
- Redis 哨兵模式
  - 现在所有公司的集群都用哨兵模式
- 缓存穿透及解决方案
- 缓存击穿及解决方案
- 缓存雪崩及解决方案
- 基础API之 Jedis 详解
- SpringBoot 集成 Redis 操作
- Redis 的实践分析



# Nosql 概述

## 为什么要用Nosql

- 先了解历史。

> 1、单机Mysql的年代

![image-20210329221626141](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329221626141.png)

在90年代，一个网站的访问量一般不大，用单个数据库完全可以轻松应付！ 

在那个时候，更多的都是静态网页，动态交互类型的网站不多。

上述架构下，我们来看看数据存储的瓶颈是什么？

1. 数据量的总大小，一个机器放不下时

2. 数据的索引（B+ Tree）一个机器的内存放不下时

3. 访问量（读写混合）一个实例不能承受

如果满足了上述 1 or 3个，进化....

DAL：数据库访问层



> 2、Memcached(缓存)+Mysql+垂直拆分

网站80%的情况都是在读，每次都要去查询数据库的话就十分麻烦。所以希望减轻数据库的压力，可以使用缓存来保证效率。

发展过程：优化数据结构和索引--》文件缓存（IO）---》Memcached(当时最热门的技术)

![image-20210329223410406](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329223410406.png)



>  3、MySQL主从读写分离

由于数据库的写入压力增加，Memcached只能缓解数据库的读取压力，读写集中在一个数据库上让数   据库不堪重负，大部分网站开始使用主从复制技术来达到读写分离，以提高读写性能和读库的可扩展  性，MySQL的master-slave模式成为这个时候的网站标配了。

![img](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/wps4.png)



> 4、分表分库 + 水平拆分 + Mysql 集群

在Memcached的高速缓存，MySQL的主从复制，读写分离的基础之上，这时MySQL主库的写压力开始出现瓶颈，而数据量的持续猛增，由于MyISAM使用表锁，在高并发下会出现严重的锁问题，大量的高   并发MySQL应用开始使用InnoDB引擎代替MyISAM。

同时，开始流行使用分表分库来缓解写压力和数据增长的扩展问题，这个时候，分表分库成了一个热门  技术，是面试的热门问题，也是业界讨论的热门技术问题。也就是在这个时候，MySQL推出了还不太稳   定的表分区，这也给技术实力一般的公司带来了希望。虽然MySQL推出了MySQL Cluster集群，但性能也不能很好满足互联网的需求，只是在高可靠性上提供了非常大的保证。

|      |                                                              |
| ---- | ------------------------------------------------------------ |
|      | ![img](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/wps9.png) |

> 5、MySQL 的扩展性瓶颈

MySQL数据库也经常存储一些大文本的字段，导致数据库表非常的大，在做数据库恢复的时候就导致非  常的慢，不容易快速恢复数据库，比如1000万4KB大小的文本就接近40GB的大小，如果能把这些数据从MySQL省去，MySQL将变的非常的小。

关系数据库很强大，但是它并不能很好的应付所有的应用场景，MySQL的扩展性差（需要复杂的技术来实现），大数据下IO压力大，表结构更改困难，正是当前使   用MySQL的开发人员面临的问题。

> 6、今天是什么样子

![image-20210329225008254](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329225008254.png)

> 7、为什么要用NoSQL

今天我们可以通过第三方平台（如：Google，FaceBook等）可以很容易的访问和抓取数据。用户的个人信息，社交网络，地理位置，用户生成的数据和用户操作日志已经成倍的增加。

我们如果要对这些用 户数据进行挖掘，那SQL数据库已经不适合这些应用了，而NoSQL数据库的发展却能很好的处理这些大   的数据！



## 什么是NoSQL

- Not Only SQL

泛指非关系型的数据库，随着互联网Web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的社交网络服务类型的Web2.0纯动态网站已经显得力不从心，暴露了很多难以克服   的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展，

NoSQL数据库的产生就是为   了解决大规模数据集合多种数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。

（例如谷歌或Facebook每天为他们的用户收集万亿比特的数据）。这些类型的数据存储不需要固定的模   式，无需多余操作就可以**横向扩展**。

### NoSQL的特点

**1、易扩展**

NoSQL 数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。

数据之间无关系，这样就非常容易扩展，也无形之间，在架构的层面上带来了可扩展的能力。

**2、大数据量高性能**

NoSQL数据库都具有非常高的读写性能，尤其是在大数据量下，同样表现优秀。这得益于它的非关系   性，数据库的结构简单。

一般MySQL使用Query Cache，每次表的更新Cache就失效，是一种大力度的Cache，在针对Web2.0的交互频繁应用，Cache性能不高，而NoSQL的Cache是记录级的，是一种细粒度的Cache，所以NoSQL  在这个层面上来说就要性能高很多了。

官方记录：Redis 一秒可以写8万次，读11万次！

**3、多样灵活的数据模型**

NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式，而在关系数据库里，增删   字段是一件非常麻烦的事情。如果是非常大数据量的表，增加字段简直就是噩梦。

**4、传统的 RDBMS VS NoSQL**

 

| 1    | 传统的关系型数据库 RDBMS                   |
| ---- | ------------------------------------------ |
| 2    | - 高度组织化结构化数据                     |
| 3    | - 结构化查询语言（SQL）                    |
| 4    | - 数据和关系都存储在单独的表中             |
| 5    | - 数据操纵语言，数据定义语言               |
| 6    | - 严格的一致性                             |
| 7    | - 基础事务                                 |
| 8    |                                            |
| 9    | **NoSQL**                                  |
| 10   | - 代表着不仅仅是SQL                        |
| 11   | - 没有固定的查询语言                       |
| 12   | - 没有预定义的模式                         |
| 13   | - 键值对存储，列存储，文档存储，图形数据库 |
| 14   | - 最终一致性，而非ACID属性                 |
| 15   | - 非结构化和不可预知的数据                 |
| 16   | - CAP定理                                  |
| 17   | - 高性能，高可用性 和 高可扩               |

**了解：3V+3高**

大数据时代的3V：主要是用来描述问题的

- 海量 Volume
- 多样 Variety 
  - （数据的类型，很多样，比如位置 信息，聊天信息）
- 实时 Velocity

大数据时代的3高：主要是用来描述问题的

- 高并发

- 高可扩

  - 随时可以水平拆分，机器不够了，可以扩展机器，集群

- 高性能

  ==正真的在公司种的实践：NoSQL+RDBMS 一起使用才是最强的==

  技术没有高低之分

## 阿里巴巴架构的演进分析

参考文档：《何崚 - 阿里巴巴中文站架构设计实践》

- 主要去看文档



![image-20210329232833513](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329232833513.png)

数据层：

![image-20210329233327790](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329233327790.png)

- 没有什么是加一层解决不了的

```bash
#1.商品的基本信息
名称、价格、出厂日期、生产厂商等 
关系型数据库：mysql、oracle。（淘宝早年就去IOE了，推荐文章，王坚，：阿里云的这群疯子）
注意，淘宝内部用的MySQL是里面的大牛自己改造过的。
#2、商品的描述、评论
文档型数据库：MongoDB

#3、图片
分布式文件系统 FastDFS
 - 淘宝自己的 TFS 
 - Google的 GFS 
 - Hadoop的 HDFS
 - 阿里云的 oss
 

# 4、商品的关键字（搜索）
 - 搜索引擎 solr ElasticSearch
 - 淘宝用的 ： Isearch。名人关注：多隆
 
# 5、商品热门的波段信息
   - 内存数据库
   - Redis ,Tair,Memacached....
# 6、商品的交易、外部的支付接口
 - 第三方应用
 
```

一个简单的网页背后的技术不是那么简单。



**大型互联网应用问题：**

- 数据类型太多 了
- 数据源繁多，经常重构
- 数据要改造，大面积改造？

解决问题： 阿里 就 加一层

![image-20210329235418699](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329235418699.png)

![image-20210329235430327](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210329235430327.png)



## NoSQL 的四大分类

**我要学的**：Redis,MongoDB,HBase,Neo4j





**kv键值对：**

- 新浪：BerkeleyDB+**redis**
- 美团：redis+Tair
- 阿里、百度：memcache+redis

**文档型数据库**：（bson格式，和json一样）-

- CouchDB（不做了解，国外的）
- **MongoDB** （必须学）
  - MongoDB 是一个**基于分布式文件存储的数据库**。由 C++ 语言编写。主要用来处理大量的文档。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
  - MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

**列存储数据库**：

- HBase
- 分布式文件系统

**图形数据库**：

- 它不是放图形的，放的是关系比如:朋友圈社交网络、广告推荐系统
- 社交网络，推荐系统等。专注于构建关系图谱
- Neo4J, InfoGrid

比如：

![image-20210330000543020](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210330000543020.png)

### 四者的对比

![image-20210330000655852](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210330000655852.png)

# Redis 入门

## 概述

百度就知道 了

> 1、什么是Redis

Redis：REmote DIctionary Server（远程字典服务器）

是完全开源免费的，用C语言编写的，遵守BSD协议，是一个高性能的（Key/Value）分布式内存数据库，基于内存运行，并支持持久化的NoSQL数据库，是当前最热门的NoSQL数据库之一，也被人们称为结构化数据库。提供多种语言的API。

Redis与其他key-value缓存产品有以下三个**特点**：

- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。

- Redis不仅仅支持简单的 key-value 类型的数据，同时还提供list、set、zset、hash等数据结构的存储。

- Redis支持数据的备份，即master-slave模式的数据备份。

> 2、Redis能干什么

1. 内存存储和持久化：因为内存中式断电即失的，所以说持久化很重要（RDB,AOF）
2. 效率高，可以用于高速缓存
3. 发布、订阅消息系统
4. 地图信息分析
5. 定时器、计数器（比如浏览量！incre decre）
6. ......

> 3、特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务
5. 。。。。

> 学习中需要用到的配置

官网：https://redis.io/

github ： https://github.com/redis/redis

中文：http://www.redis.cn/

下载地址：官网下载-（是Linux的）

- 注意windows 在github上下载，（windows的redis停更很久了）
- Redis 推荐都是在linux服务器上搭建。不建议使用

## Redis 安装

### windows 下安装（不要用）



### Linux 下安装

1、官网下载最新安装包 ： redis-6.2.1.tar.gz，放到 /opt 目录下

redis 官网：https://redis.io/

2、程序一般建议放到 /opt目录下面。即先移到/opt，并解压，

tar -zxvf redis-6.2.1.tar.gz 

![image-20210401215247357](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401215247357.png)

3、进入解压后的文件，可以看到redis 的配置文件 redis.conf

![image-20210401215404782](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401215404782.png)



4、基本环境安装

- 在redis目录下，运行make命令

- 再运行make install

这里如果，出现出现install error，就是需要升级gcc版本，至少我下载的是gcc-7.5.0.tar.gz 。 centOS 7自带的是4.几的

gcc 所有版本：http://ftp.gnu.org/gnu/gcc 

升级gcc 参考博客 https://blog.csdn.net/flyspace/article/details/88718298

```bash
1. 安装gcc (gcc是linux下的一个编译程序，是c程序的编译工具) 
能上网: yum install 	gcc-c++ 
版本测试: gcc -v 
2. make 
3. Jemalloc/jemalloc.h: 没有那个文件或目录 
运行 make distclean 之后再make 
4. Redis Test（可以不用执行）
```

5、查看默认安装目录：usr/local/bin

/usr 这是一个非常重要的目录，类似于windows下的Program Files,存放用户的程序 

![image-20210401224837756](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401224837756.png)

6、将 redis 配置文件，复制到我们当前目录下。

保留原本的配置文件，保证可以回到原来的配置

![image-20210401225144491](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401225144491.png)

7 、启动。redis 默认不是后台启动的,修改配置文件。改成后台启动。

需要把redis.conf 里的daemonize 改为yes 

![image-20210401225454179](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401225454179.png)

**daemonize设置yes或者no区别**

**daemonize:yes**

redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项

pidfifile设置的文件中，此时redis将一直运行，除非手动kill该进程。

**daemonize:no**

当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭连接工具(putty,xshell等)都会导致redis进程退出。

8、启动redis

通过指定的配置文件启动服务

redis-server joker_config/redis.conf

```bash
[root@joker bin]# pwd
/usr/local/bin
[root@joker bin]# redis-se
redis-sentinel  redis-server    
[root@joker bin]# redis-server joker_config/redis.conf
```

9、使用reids-cli （redis 客户端）进行连接测试

redis-cli -p 6379

![image-20210401225947433](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401225947433.png)

10、查看redis 进程 是否开启

ps -ef|grep redis

![image-20210401230142316](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401230142316.png)

11、如何关闭 redis 

shutdown

![image-20210401230355228](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401230355228.png)

12 再次查看redis 进程是否存在

redis-server,redis-cli都没有了

![image-20210401230439220](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210401230439220.png)



后面，会使用单机多redis 启动集群测试。



## Redis-benchmark 性能测试

Redis-benchmark 是一个redis官方自带的性能测试工具，

官网： http://www.redis.cn/topics/benchmarks.html

redis-benchmark 命令参数 

```bash
以下参数被支持：

Usage: redis-benchmark [-h <host>] [-p <port>] [-c <clients>] [-n <requests]> [-k <boolean>]

 -h <hostname>      Server hostname (default 127.0.0.1)
 -p <port>          Server port (default 6379)
 -s <socket>        Server socket (overrides host and port)
 -a <password>      Password for Redis Auth
 -c <clients>       Number of parallel connections (default 50)
 -n <requests>      Total number of requests (default 100000)
 -d <size>          Data size of SET/GET value in bytes (default 2)
 -dbnum <db>        SELECT the specified db number (default 0)
 -k <boolean>       1=keep alive 0=reconnect (default 1)
 -r <keyspacelen>   Use random keys for SET/GET/INCR, random values for SADD
  Using this option the benchmark will expand the string __rand_int__
  inside an argument with a 12 digits number in the specified range
  from 0 to keyspacelen-1. The substitution changes every time a command
  is executed. Default tests use this to hit random keys in the
  specified range.
 -P <numreq>        Pipeline <numreq> requests. Default 1 (no pipeline).
 -q                 Quiet. Just show query/sec values
 --csv              Output in CSV format
 -l                 Loop. Run the tests forever
 -t <tests>         Only run the comma separated list of tests. The test
                    names are the same as the ones produced as output.
 -I                 Idle mode. Just open N idle connections and wait.
```

![image-20210331144123249](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210331144123249.png)

**简单测试：**

```bash
# 测试一：100个并发连接，每个并发100000个请求，检测host为localhost 端口为6379的redis服务器性 能
redis-benchmark -h localhost -p 6379 -c 100 -n 100000 
#解释测试出来的命令 
#测试出来的所有命令，这里只举例一个！ 
====== SET ====== 
100000 requests completed in 1.88 seconds # 对集合（10万个请求）进行写入测试 
100 parallel clients # 每次请求有100个并发客户端 
3 bytes payload # 每次写入3个字节的数据，有效载荷 
keep alive: 1 # 保持一个连接，只有一台服务器来处理这些请求 ，单机性能
17.05% <= 1 milliseconds 
97.35% <= 2 milliseconds 
99.97% <= 3 milliseconds 
100.00% <= 3 milliseconds # 所有请求在 3 毫秒内完成 
53248.14 requests per second # 每秒处理 53248.14 次请求
```



## Redis 基础的知识

redis 默认有16个数据库,类似数组下标从零开始，初始默认使用0号库。

可以 查看配置文件 redis.conf，

```bash
#查看 redis.conf ，里面有默认的配置 

databases 16 
# Set the number of databases. The default database is DB 0, you can select 
# a different one on a per-connection basis using SELECT <dbid> where 
# dbid is a number between 0 and 'databases'-1 
databases 16
```

Select 命令 切换数据库

```bash
127.0.0.1:6379> select 7 
OK
127.0.0.1:6379[7]> 
# 不同的库可以存不同的数据
```

Dbsize 查看当前数据库的key的数量

```bash
127.0.0.1:6379[7]> DBSIZE 
(integer) 0 
127.0.0.1:6379[7]> select 0 
OK
127.0.0.1:6379> DBSIZE 
(integer) 5 
127.0.0.1:6379> keys * # 查看具体的key 
1) "counter:__rand_int__" 
2) "mylist" 
3) "k1" 
4) "myset:__rand_int__" 
5) "key:__rand_int__"
```

Flushdb：清空当前库

Flushall：清空全部的库

```bash
127.0.0.1:6379> DBSIZE 
(integer) 5 
127.0.0.1:6379> FLUSHDB 
OK
127.0.0.1:6379> DBSIZE 
(integer) 0
```



>  **思考**： 为什么redis 默认端口是6379？

- 了解一下乐趣

就是一个明星 的名字的缩写

> **Redis 是单线程的**

- redis 6代支持多线程了

我们首先要明白，Redis很快！官方表示，因为Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis

的瓶颈最有可能是机器内存的大小或者网络带宽。既然单线程容易实现，而且CPU不会成为瓶颈，那就

顺理成章地采用单线程的方案了！

Redis采用的是基于内存的采用的是单进程单线程模型的 KV 数据库，由C语言编写，官方提供的数据是

可以达到100000+的QPS（每秒内查询次数）。这个数据不比采用单进程多线程的同样基于内存的 KV

数据库 Memcached 差！

**Redis为什么这么快？**

1）误区一：高性能服务器 一定是多线程来实现的

误区二：多线程（cpu上下文切换） 一定比 单线程 效率高，其实不然！

在说这个事前希望大家都能对 CPU >内存 >硬盘的速度都有了解了！

2）redis **核心**就是: 如果我的数据全都在内存里，我单线程的去操作 就是效率最高的，为什么呢，因为

多线程的本质就是 CPU 模拟出来多个线程的情况，这种模拟出来的情况就有一个代价，就是上下文的切

换，对于一个内存的系统来说，它没有上下文的切换就是效率最高的。redis 用 单个CPU 绑定一块内存

的数据，然后针对这块内存的数据进行多次读写的时候，都是在一个CPU上完成的，所以它是单线程处

理这个事。在内存的情况下，这个方案就是最佳方案。

因为一次CPU上下文的切换大概在 1500ns 左右。从内存中读取 1MB 的连续数据，耗时大约为 250us，

假设1MB的数据由多个线程读取了1000次，那么就有1000次时间上下文的切换，那么就有1500ns *

1000 = 1500us ，我单线程的读完1MB数据才250us ,你光时间上下文的切换就用了1500us了，我还不

算你每次读一点数据 的时间。



# 五大数据类型

- 看官网介绍：

![image-20210331153831039](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210331153831039.png)

- Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作**数据库、缓存和消息中间件**。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

## Redis-key的操作

有关redis 的key的一些基本操作

命令不懂，就看官网。

redis 命令官网：http://www.redis.cn/commands.html#generic

redis 不区分大小写

常用：

- set key value
- get key
- exists key
- move key db
- keys  *
- expire key seconds
- ttl key 
- type key
- 

```bash
# keys * 查看所有的key 
127.0.0.1:6379> keys * 
(empty list or set) 
127.0.0.1:6379> set name qinjiang
OK
127.0.0.1:6379> keys * 
1) "name" 
# exists key 的名字，判断某个key是否存在 1,存在，0，不存在
127.0.0.1:6379> EXISTS name 
(integer) 1 
127.0.0.1:6379> EXISTS name1 
(integer) 0 

# move key db ---> 当前库就没有了，被移除了 
127.0.0.1:6379> move name 1 
(integer) 1 
127.0.0.1:6379> keys * 
(empty list or set) 
# expire key 秒钟：为给定 key 设置生存时间，当 key 过期时(生存时间为 0 )，它会被自动删 除。
# ttl key 查看还有多少秒过期，-1 表示永不过期，-2 表示已过期 
127.0.0.1:6379> set name joker 
OK
127.0.0.1:6379> EXPIRE name 10 
(integer) 1 
127.0.0.1:6379> ttl name 
(integer) 4 
127.0.0.1:6379> ttl name 
(integer) 3 
127.0.0.1:6379> ttl name 
(integer) 2 
127.0.0.1:6379> ttl name 
(integer) 1 
127.0.0.1:6379> ttl name 
(integer) -2 
127.0.0.1:6379> keys * 
(empty list or set) 
# type key 查看你的key是什么类型 
127.0.0.1:6379> set name joker
OK
127.0.0.1:6379> get name 
"qinjiang" 
127.0.0.1:6379> type name 
string
```



## Strings（字符串）

String有关的的命令 常用的：

-  set、get、del、append、strlen、exists
- incr、 decr、 incrby 、decrby
- getrange key start end
- setrange key offset value
- SETEX key seconds value
- SETNX key value
- MSET、 MGET 、MSETNX
- 对象缓存存储的格式
- GETSET key value

测试：

```bash
#=================================================
# set、get、del、append、strlen、exists
#=================================================
127.0.0.1:6379> set key1 v1  # 设置值
OK
127.0.0.1:6379> set key2 v2
OK
127.0.0.1:6379> KEYS *
1) "key2"
2) "key1"
127.0.0.1:6379> get key1  # 获得key
"v1"
127.0.0.1:6379> del key1 # 查看全部的key
(integer) 1
127.0.0.1:6379> EXISTS key1 # 判断 key1 是否存在
(integer) 0
127.0.0.1:6379> APPEND key1 "hello" # 对不存在的 key 进行 APPEND ，等同于 SET key1 "hello"
(integer) 5 # 字符长度
127.0.0.1:6379> APPEND key1 ",world" # 对已存在的字符串进行 APPEND
(integer) 11
127.0.0.1:6379> STRLEN key1 # 获取字符串的长度
(integer) 11
127.0.0.1:6379> get key1
"hello,world"
127.0.0.1:6379> 
#=================================================# incr key
# incr、decr 一定要是数字才能进行加减，+1 和 -1。 一般用于网站浏览量之类的。
# incrby key increment
# incrby、decrby 命令将 key 中储存的数字加上指定的增量值。 (步长)
# 这是原子操作
#=================================================
127.0.0.1:6379> set views 0 # 设置key为0
OK
127.0.0.1:6379> incr views # 浏览 + 1
(integer) 1
127.0.0.1:6379> incr views 
(integer) 2
127.0.0.1:6379> decr views # 浏览 - 1
(integer) 1
127.0.0.1:6379> incrby views 10 # +10
(integer) 11
127.0.0.1:6379> DECRBY views 10 # -10
(integer) 1
127.0.0.1:6379> 

#=================================================
# getrange key start end
# 获取指定区间范围内的值，类似between...and的关系，从零到负一表示全部 
#=================================================
127.0.0.1:6379> FLUSHDB
OK
127.0.0.1:6379> set key1 "hello,redis"
OK
127.0.0.1:6379> GETRANGE key1 0 4 # 截取部分字符串
"hello"
127.0.0.1:6379> GETRANGE key1 0 -1 # 获得全部的值
"hello,redis"
127.0.0.1:6379> 
#=================================================# setrange key offset value
# setrange 设置指定区间范围内的值，即从offset 开始替换
#=================================================
127.0.0.1:6379> SETRANGE key1 0 joker
(integer) 11
127.0.0.1:6379> get key1
"joker,redis"
127.0.0.1:6379> 
#=================================================# SETEX key seconds value
# setex（set with expire）设置过期时间
# SETNX key value
# setnx（set if not exist） key不存在才set
#=================================================
127.0.0.1:6379> SETEX key1 30 "joker"
OK
127.0.0.1:6379> ttl key1
(integer) 5
127.0.0.1:6379> ttl key1
(integer) -2
127.0.0.1:6379> 
127.0.0.1:6379> SETNX key2 "redis" #如果key2不存在，创建key2
(integer) 1 # 设置成功返回1
127.0.0.1:6379> SETNX key2 "MogonDB"
(integer) 0 # 设置失败返回0
127.0.0.1:6379> get key2
"redis"
127.0.0.1:6379> 
#=================================================# MSET key value [key value ...]
# Mset 命令用于同时设置一个或多个 key-value 对。 
# MGET key [key ...]
# Mget 命令返回所有(一个或多个)给定 key 的值。 
# 如果给定的 key 里面，有某个 key 不存在，那么这个 key 返回特殊值 nil 。 
# MSETNX key value [key value ...]
# msetnx 当所有 key 都成功设置，返回 1 。 
# 如果所有给定 key 都设置失败(至少有一个 key 已经存在)，那么返回 0 。原子操 作
#=================================================
127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3 #同时设置多个k-v 
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
4) "key2"
127.0.0.1:6379> MGET k1 k2
1) "v1"
2) "v2"
127.0.0.1:6379> MSETNX k1 v1 k5 v5
(integer) 0 #k1 已经存在，失败返回0
127.0.0.1:6379> MSETNX k5 v5 k6 v6
(integer) 1 # 成功，返回1 
127.0.0.1:6379> keys *
1) "key2"
2) "k2"
3) "k5"
4) "k6"
5) "k3"
6) "k1"
127.0.0.1:6379> 

#项目中的 对象，对象缓存存储
# 方法1 传统用json 字符串缓存
set user:1 {name:joker,age:3}
# 方法2 reids 中key的巧妙设计。user:{id}:{field},
127.0.0.1:6379> mset user:1:name joker user:1:age 3
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "joker"
2) "3"
127.0.0.1:6379> 
#================================================= # GETSET key value
# getset（先get再set）
#=================================================
127.0.0.1:6379> GETSET db mongodb #  没有旧值，返回 nil,并设置值
(nil)
127.0.0.1:6379> get db
"mongodb"
127.0.0.1:6379> GETSET db redis #存在旧值，返回旧值，并设置新值
"mongodb"
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> 


```

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。

String 使用场景：常规key-value缓存应用：

- 常规计数：微博数，粉丝数等。
- 统计多单位的数量
- 对象缓存存储



后来会学学习 Jedis : Redis 官方推荐的java 操作的客户端，包含了很多的方法，现在的key的操作，都会变成一个方法。

## Lists (列表)

![image-20210402151331745](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210402151331745.png)

Redis  里，可以把list 玩成 队列，栈，阻塞队列

- 大部分的list 的命令都是 L 开头

List 常用命令：

- LPUSH、RPUSH、LRANGE
- LPOP、RPOP
- LINDEX
- LLEN
- LREM key count value
- LTRIM key start stop
- RPOPLPUSH source destination
- lset key index value
- linsert key before/after pivot value

```bash
#=================================================# Lpush：将一个或多个值插入到列表头部。（左） 
# rpush：将一个或多个值插入到列表尾部。（右） 
# lrange：返回列表中指定区间内的元素，区间以偏移量 START 和 END 指定。
# 其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。 
# 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此 类推。 
# ==================================================
127.0.0.1:6379> LPUSH list one two 
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> RPUSH list r1 r2
(integer) 5
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "r1"
5) "r2"
127.0.0.1:6379>

#=================================================# lpop 命令用于移除并返回列表的第一个元素。当列表 key 不存在时，返回 nil 。 
# rpop 移除列表的最后一个元素，返回值为移除的元素。 
#=================================================
127.0.0.1:6379> LPOP list 
"three"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
3) "r1"
4) "r2"
127.0.0.1:6379> RPOP list
"r2"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"
3) "r1"
127.0.0.1:6379> 
#=================================================# Lindex，按照索引下标获得元素（-1代表最后一个，0代表是第一个） 
#=================================================

127.0.0.1:6379> LINDEX list 0
"two"
127.0.0.1:6379> LINDEX list 2
"r1"
127.0.0.1:6379> LINDEX list -1
"r1"
127.0.0.1:6379>

#=================================================# llen 用于返回列表的长度。 
#=================================================
127.0.0.1:6379> LLEN list
(integer) 3
127.0.0.1:6379> 
#================================================= # lrem key 根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素。
#从存于 key 的列表里移除前 count 次出现的值为 value 的元素。 这个 count 参数通过下面几种方式影响这个操作：

# count > 0: 从头往尾移除值为 value 的元素。
# count < 0: 从尾往头移除值为 value 的元素。
# count = 0: 移除所有值为 value 的元素。
#=================================================

127.0.0.1:6379> lrem list 0 two
(integer) 1
#=================================================# Ltrim key 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区 间之内的元素都将被删除。
#================================================

127.0.0.1:6379> lpush list l1 l2 l3 l4
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "l4"
2) "l3"
3) "l2"
4) "l1"
127.0.0.1:6379> LTRIM list 1 2
OK
127.0.0.1:6379> LRANGE list 0 -1
1) "l3"
2) "l2"
127.0.0.1:6379> 

#=================================================# rpoplpush 移除列表的最后一个元素，并将该元素添加到另一个列表并返回。 
#=================================================
127.0.0.1:6379> RPOPLPUSH list list2
"l2"
127.0.0.1:6379> LRANGE list 0 -1
1) "l3"
127.0.0.1:6379> LRANGE list2 0 -1
1) "l2"
127.0.0.1:6379> 
#=================================================# lset key index value 将列表 key 下标为 index 的元素的值设置为 value 。 
#=================================================
127.0.0.1:6379> LPUSH list l1 l2 l3 l4
(integer) 4
127.0.0.1:6379> LSET list 1 l11 #对非空列表进行LSET
OK
127.0.0.1:6379> LRANGE list 0 -1
1) "l4"
2) "l11"
3) "l2"
4) "l1"
127.0.0.1:6379> 
127.0.0.1:6379> exists list # 对空列表(key 不存在)进行 LSET 
(integer) 0 
127.0.0.1:6379> lset list 0 item # 报错 
(error) ERR no such key
#=================================================# linsert key before/after pivot value 用于在列表的元素前或者后插入元素。 
# 将值 value 插入到列表 key 当中，位于值 pivot 之前或之后。
#=================================================
127.0.0.1:6379> LINSERT list before l2 l22
(integer) 5
127.0.0.1:6379> LRANGE list 0 -1
1) "l4"
2) "l11"
3) "l22"
4) "l2"
5) "l1"
127.0.0.1:6379> 

```

### list 小结

- list 实际上就是一个链表
- before node after ,left，right 都可以插入添加
- 如果键不存在，创建新的链表
- 如果键已存在，新增内容
- 如果值全移除，对应的键也就消失了
- 链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。

List 的应用：

- 消息队列 （LPUSH，Rpop) 左进，右出
- 栈(Lpush,Lpop) 左进左出



## Sets (集合)

- 一个set 中的值，不能重复
- set 无序不重合

set 常用命令：

- SADD
- SMEMBERS 
- SISMEMBER
- scard
- srem
- spop key [count]
- SMOVE SOURCE DESTINATION MEMBER
- sdiff , sinter , sunion

```bash
#=================================================
# sadd 将一个或多个成员元素加入到集合中，不能重复
# smembers 返回集合中的所有的成员。 
# sismember 命令判断成员元素是否是集合的成员。
#=================================================
127.0.0.1:6379> SADD myset "hello"
(integer) 1
127.0.0.1:6379> SADD myset "joker"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "joker"
2) "hello"
127.0.0.1:6379> 
127.0.0.1:6379> SISMEMBER myset hello
(integer) 1
127.0.0.1:6379> SISMEMBER myset world
(integer) 0
127.0.0.1:6379> 
#=================================================
# scard，获取集合里面的元素个数 
#=================================================
127.0.0.1:6379> SCARD myset
(integer) 2

#================================================= 
# srem key value 用于移除集合中的一个或多个成员元素 
#=================================================
127.0.0.1:6379> srem myset hello
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "joker"
127.0.0.1:6379> 
#=================================================
# srandmember key [count]命令用于返回集合中的一个随机元素。 count如果设置了，就是随机返回count个元素
#=================================================
127.0.0.1:6379> SMEMBERS myset
1) "monster"
2) "world"
3) "hello"
4) "joker"
127.0.0.1:6379> SRANDMEMBER myset
"joker"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "monster"
2) "hello"
127.0.0.1:6379> 
#================================================= 
# spop key [count] 从存储在key的集合中移除并返回一个或多个随机元素#=================================================
127.0.0.1:6379> SMEMBERS myset
1) "monster"
2) "world"
3) "joker"
127.0.0.1:6379> spop myset
"monster"
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "joker"
127.0.0.1:6379> 

#=================================================
# smove SOURCE DESTINATION MEMBER
# 将指定成员 member 元素从 source 集合移动到 destination 集合。 
#=================================================
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "joker"
127.0.0.1:6379> SMOVE myset myset2 joker
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
127.0.0.1:6379> SMEMBERS myset2
1) "joker"
127.0.0.1:6379> 
#=================================================
- 数字集合类：
	-差集：sdiff A B 结果就是A-AB
	-交集：sinter 共同好友就可以这么实现
	-并集：sunion
#=================================================
127.0.0.1:6379> sadd set1 a b c d 
(integer) 4
127.0.0.1:6379> sadd set2 c d e f 
(integer) 4
127.0.0.1:6379> SDIFF set1 set2
1) "a"
2) "b"
127.0.0.1:6379> SDIFF set2 set1
1) "f"
2) "e"
127.0.0.1:6379> SINTER set1 set2
1) "d"
2) "c"
127.0.0.1:6379> SUNION set1 set2
1) "d"
2) "f"
3) "c"
4) "b"
5) "e"
6) "a"
127.0.0.1:6379> SUNION set2 set1
1) "e"
2) "d"
3) "f"
4) "b"
5) "c"
6) "a"
127.0.0.1:6379> 

```

### 小结

**set 应用：**

在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis还为集合提供了求交集、并集、差集等操作，可以非常方便的实现如共同关注、共同喜好、二度好友等功能，对上面的所有集合操作，你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。



## Hashes (哈希)

- key-map.这时候 值 是一个map集合

- **kv**模式不变，但**V是一个键值对**
- 本质上和String 区别不大

Hash 常用命令：

- HSET key field value [field value ...]
- HGET key field
- HGETALL  key
- HMSET （已经弃用），可以直接用 hset
- HMGET key field [field...]
- HDEL key field [field...]
- HLEN key
- HEXISTS key field
- HKEYS key
- HVALS key
- HINCRBY key field increment
- HSETNX key field value

测试

```bash
#=================================================
# hset、hget 命令用于为哈希表中的字段赋值 。 
# hmset、hmget 同时将多个field-value对设置到哈希表中。会覆盖哈希表中已存在的字段。 
# hgetall 用于返回哈希表中，所有的字段和值。 
# hdel 用于删除哈希表 key 中的一个或多个指定字段 ，对应的value值也没有了
#=================================================
127.0.0.1:6379> hset myhash field1 joker
(integer) 1
127.0.0.1:6379> hget myhash field1
"joker"
127.0.0.1:6379> hset myhash field2 monster field3 hello
(integer) 2
127.0.0.1:6379> HGETALL myhash
1) "field1"
2) "joker"
3) "field2"
4) "monster"
5) "field3"
6) "hello"
127.0.0.1:6379> HMGET myhash field1 field2
1) "joker"
2) "monster"
127.0.0.1:6379> 
127.0.0.1:6379> HDEL myhash field1 field2
(integer) 2
127.0.0.1:6379> HGETALL myhash
1) "field3"
2) "hello"
127.0.0.1:6379> 
#================================================
# hlen 获取哈希表中字段的数量。 
#=================================================
127.0.0.1:6379> HLEN myhash
(integer) 1
127.0.0.1:6379> 
#=================================================
# hexists 查看哈希表的指定字段是否存在。 
#=================================================
127.0.0.1:6379> HEXISTS myhash field1
(integer) 0
127.0.0.1:6379> HEXISTS myhash field3
(integer) 1
127.0.0.1:6379> 
#=================================================
# hkeys 获取哈希表中的所有域（field）
# hvals 返回哈希表所有域(field)的值。
#=================================================
127.0.0.1:6379> hset myhash field1 joker field2 monster
(integer) 2
127.0.0.1:6379> HKEYS myhash
1) "field3"
2) "field1"
3) "field2"
127.0.0.1:6379> hvals myhash
1) "hello"
2) "joker"
3) "monster"
127.0.0.1:6379> 

#=================================================
# hincrby 为哈希表中的字段值加上指定增量值。 字段的值必须是数字
#=================================================
127.0.0.1:6379> hset myhash field4 1
(integer) 1
127.0.0.1:6379> HINCRBY myhash field4 1
(integer) 2
127.0.0.1:6379> HINCRBY myhash field4 1
(integer) 3
127.0.0.1:6379> HINCRBY myhash field4 3
(integer) 6
127.0.0.1:6379> HINCRBY myhash field4 -10
(integer) -4
127.0.0.1:6379> 
#================================================
# hsetnx 为哈希表中不存在的的字段赋值 。 
#================================================
127.0.0.1:6379> HSETNX myhash field1 vvvv
(integer) 0 # feild1 已经存在，设置失败返回 0
127.0.0.1:6379> HSETNX myhash field5 vvv555
(integer) 1 # 设置成功返回1
127.0.0.1:6379> HKEYS myhash
1) "field3"
2) "field1"
3) "field2"
4) "field4"
5) "field5"
127.0.0.1:6379> HVALS myhash
1) "hello"
2) "joker"
3) "monster"
4) "-4"
5) "vvv555"
127.0.0.1:6379> 

```



### hashes 小结

Redis hash是一个string类型的fifield和value的映射表，**hash适合用于存储对象，String适合字符串存储**。

适合 存储 **经常变动** 的数据，如**用户信息**等。



## Sorted Sets(Zset 有序集合)

在set基础上，加一个score值。之前set是k1 v1 v2 v3，现在zset是 k1 score1 v1 score2 v2

常用命令：

- ZADD 
- ZRANGE ， ZREVRANGE
- ZRANGEBYSCORE ， ZREVRANGEBYSCORE
- ZREM
- ZCARD
- ZCOUNT
- ZRANK , ZREVRANK

测试：

```bash
# =================================================== 
# zadd 将一个或多个成员元素及其分数值加入到有序集当中。 
# zrange 根据scores 递增返回。返回有序集中，指定区间内的成员 。
# ZREVRANGE 递减返回，递减返回
# ===================================================
127.0.0.1:6379> ZADD myset 1 "one"
(integer) 1
127.0.0.1:6379> ZADD myset 2 "two" 3 "three"
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> ZREVRANGE myset 0 -1 withscores
1) "three"
2) "3"
3) "two"
4) "2"
5) "one"
6) "1"
127.0.0.1:6379> ZREVRANGE myset 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> 

# =================================================== 
# ZRANGEBYSCORE 返回有序集合中指定分数区间的成员列表。有序集成员按分数值递增(从小到大) 次序排列。 ，默认是闭区间，
# ZREVRANGEBYSCORE  递减排序返回
# ===================================================
127.0.0.1:6379> ZADD salary 200 user1 300 user2 400 user3
(integer) 3
# Inf无穷大量+∞,同样地,-∞可以表示为-Inf。
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf #显示整个有序集
1) "user1"
2) "user2"
3) "user3"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores #带scores 递增排列
1) "user1"
2) "200"
3) "user2"
4) "300"
5) "user3"
6) "400"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf 300 WITHSCORES # 显示工资 <=300
1) "user1"
2) "200"
3) "user2"
4) "300"
127.0.0.1:6379> 

# =================================================== 
# zrem 移除有序集中的一个或多个成员
# ===================================================
127.0.0.1:6379> ZREM salary user1 user2
(integer) 2
127.0.0.1:6379> ZRANGE salary 0 -1
1) "user3"
127.0.0.1:6379>

# =================================================== 
# zcard 命令用于计算集合中元素的数量。 
# ===================================================
127.0.0.1:6379> ZCARD myset
(integer) 3
127.0.0.1:6379> 

# =================================================== 
# zcount 计算有序集合中指定分数区间的成员数量。
# ===================================================
127.0.0.1:6379> ZRANGE myset 0 -1 withscores
1) "one"
2) "1"
3) "two"
4) "2"
5) "three"
6) "3"
127.0.0.1:6379> ZCOUNT myset 1 2
(integer) 2
127.0.0.1:6379> 
# =================================================
# zrank 返回有序集中指定成员的排名。其中有序集成员按分数值递增(从小到大)顺序排列。
# =================================================
127.0.0.1:6379> ZADD salary 500 user1 1000 user2 2000 user3
(integer) 3
127.0.0.1:6379> ZRANGE salary 0 -1 withscores
1) "user1"
2) "500"
3) "user2"
4) "1000"
5) "user3"
6) "2000"
127.0.0.1:6379> ZRANK salary user1 # 显示 user1 的薪水排名，最少
(integer) 0
127.0.0.1:6379> ZRANK salary user3 # 显示 user33 的薪水排名，第三
(integer) 2
127.0.0.1:6379> 
# =================================================
# zrevrank 返回有序集中成员的反向排名。其中有序集成员按分数值递减(从大到小)排序。 
# =================================================
127.0.0.1:6379> ZREVRANK salary user1
(integer) 2
127.0.0.1:6379> ZREVRANK salary user3
(integer) 0
127.0.0.1:6379> 

```

### Sorted Sets 小结

应用场景：

- 和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列，比如

一个存储全班同学成绩的sorted set，其集合value可以是同学的学号，而score就可以是其考试得分，

这样在数据插入集合的时候，就已经进行了天然的排序。

- 可以用sorted set来做**带权重的队列**，比如普通消息的score为1，重要消息的score为2，然后工作线程可以选择按score的倒序来获取工作任务。让重要的任务优先执行。

- 排行榜应用，取TOP N操作 ！





# 三种特殊数据类型

## GEOSPATIAL 地理位置

### 简介

Redis 的 GEO 特性在 Redis 3.2 版本中推出， 这个功能可以将用户给定的地理位置信息储存起来， 并对这些信息进行操作。来实现诸如**附近位置、摇一摇**这类依赖于地理位置信息的功能。geo的数据类型为zset。

GEO 的数据结构总共有六个常用命令：geoadd、geopos、geodist、georadius、georadiusbymember、gethash

定义主要在官方文档里看，哪里不懂，就去看

官方文档：https://www.redis.net.cn/order/3685.html

### GEO 命令

#### geoadd

- 格式： GEOADD key longitude latitude member [longitude latitude member ...]

定义解析：

```bash
# 将给定的空间元素(纬度、经度、名字)添加到指定的key里面。 
# 这些数据会以有序集sorted set的形式被储存在键里面，从而使得georadius和georadiusbymember这样的 命令可以在之后通过位置查询取得这些元素。 
# geoadd命令以标准的x,y格式接受参数,所以用户必须先输入经度,然后再输入纬度。 
# geoadd能够记录的坐标是有限的:非常接近两极的区域无法被索引。 
# 有效的经度介于-180-180度之间，有效的纬度介于-85.05112878 度至 85.05112878 度之间。， 当用户尝试输入一个超出范围的经度或者纬度时,geoadd命令将返回一个错误。
```

测试：百度经纬度，模拟真实数据

```bash
127.0.0.1:6379> geoadd china:city 116.43 40.22 "北京"
(integer) 1
127.0.0.1:6379> GEOADD china:city 121.48 31.40 shanghai 113.88 22.55 shenzhen
(integer) 2

```



#### geopos

- 格式：GEOPOS key member [member ...]

解析：从`key`里返回 所有 给定位置元素的位置（经度和纬度）。

测试:

```bash
127.0.0.1:6379> GEOPOS china:city shanghai
1) 1) "121.48000091314315796"
   2) "31.40000025319353938"
127.0.0.1:6379> GEOPOS china:city 北京
1) 1) "116.43000215291976929"
   2) "40.2200010338739844"
127.0.0.1:6379> GEOPOS china:city shanghai shenzhen
1) 1) "121.48000091314315796"
   2) "31.40000025319353938"
2) 1) "113.87999922037124634"
   2) "22.5500010475923105"
127.0.0.1:6379> 

```

#### geodist

- 语法：GEODIST key member1 member2 [unit]

- 解析：

  - 返回两个给定位置之间的距离。

    如果两个位置之间的其中一个不存在， 那么命令返回空值。

    指定单位的参数 unit 必须是以下单位的其中一个：

    + **m** 表示单位为米。
    + **km** 表示单位为千米。
    + **mi** 表示单位为英里。
    + **ft** 表示单位为英尺。

    如果用户没有显式地指定单位参数， 那么 `GEODIST` **默认**使用**米**作为单位。

    `GEODIST` 命令在计算距离时会假设地球为完美的球形， 在极限情况下， 这一假设最大会造成 0.5% 的误差。

测试：

```bash
127.0.0.1:6379> GEODIST china:city shanghai shenzhen
"1238673.2335"
127.0.0.1:6379> GEODIST china:city shanghai shenzhen km
"1238.6732"
127.0.0.1:6379> 
```

#### georadius

- 语法：GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]

解析：

- 以给定的经纬度为中心， 找出某一半径内的元素

- 在给定以下可选项时， 命令会返回额外的信息：

  + `WITHDIST`: 在返回位置元素的同时， 将**位置元素与中心之间的距离**也一并返回。 距离的单位和用户给定的范围单位保持一致。
  + `WITHCOORD`: 将**位置元素的经度和维度**也一并返回。
  + `WITHHASH`: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。

  命令默认返回未排序的位置元素。 通过以下两个参数， 用户可以指定被返回位置元素的**排序方式**：

  + `ASC`: 根据中心的位置， 按照从近到远的方式返回位置元素。
  + `DESC`: 根据中心的位置， 按照从远到近的方式返回位置元素。

  在**默认**情况下， GEORADIUS 命令会返回所有匹配的位置元素。 虽然用户可以使用 **COUNT `<count>`** 选项去获取前 N 个匹配元素， 但是因为命令在内部可能会需要对所有被匹配的元素进行处理， 所以在对一个非常大的区域进行搜索时， 即使只使用 `COUNT` 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 `COUNT` 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

测试: 

重新连接 redis-cli，增加参数 --raw ，可以强制输出中文，不然会乱码

```bash
[root@joker bin]# redis-cli --raw  -p 6379
# 默认返回位置名称
127.0.0.1:6379> GEORADIUS china:city 100 30 2000 km
北京
127.0.0.1:6379> 
# withdist 返回位置名称和中心距离
127.0.0.1:6379> GEORADIUS china:city 100 30 2000 km withdist
北京
1872.7455
#withcoord 返回位置名称和经纬度
127.0.0.1:6379> GEORADIUS china:city 100 30 2000 km withdist withcoord
北京
1872.7455
116.43000215291976929
40.2200010338739844
127.0.0.1:6379> 

```



#### georadiusbymember

- 语法：GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count]

解析：

这个命令和 [GEORADIUS](http://www.redis.cn/commands/georadius.html) 命令一样， 都可以找出位于指定范围内的元素， 但是 `GEORADIUSBYMEMBER` 的**中心点是由给定的位置元素**决定的， 而不是像 [GEORADIUS](http://www.redis.cn/commands/georadius.html) 那样， 使用输入的经度和纬度来决定中心点

指定成员的位置被用作查询的中心。

测试：

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 2000 km
shanghai
北京
127.0.0.1:6379> 
```



#### geohash

- 语法：GEOHASH key member [member ...]

解析：

- 返回一个或多个位置元素的 [Geohash](https://en.wikipedia.org/wiki/Geohash) 表示。

- Redis使用geohash将二维经纬度转换为一维字符串，字符串越长表示位置更精确,两个字符串越相似 表示距离越近。
- geohash字符串属性：
  - 该命令将返回**11个字符**的Geohash字符串，所以没有精度Geohash，损失相比，使用内部52位表示。返回的geohashes具有以下特性：
    1. 他们可以缩短从右边的字符。它将失去精度，但仍将指向同一地区。
    2. 它可以在 `geohash.org` 网站使用，网址 `http://geohash.org/<geohash-string>`。查询例子：http://geohash.org/sqdtr74hyu0.
    3. 与类似的前缀字符串是附近，但相反的是不正确的，这是可能的，用不同的前缀字符串附近。

测试：

```bash
127.0.0.1:6379> GEOHASH china:city shanghai shenzhen
1) "wtw6sk5n300"
2) "ws0br3hgk20"
127.0.0.1:6379> 

```



#### zrem、zrange

- GEO没有提供删除成员的命令，但是因为GEO的底层实现是zset(Sorted Sets)，所以可以借用zrem命令实现对地理位置信息的删除. 

测试：

```bash
#先退出，再重进，--raw。如果不添加，redis返回中文会乱码
[root@joker bin]# redis-cli --raw  -p 6379
127.0.0.1:6379> ZRANGE china:city 0 -1 #查看所有城市
shenzhen
shanghai
北京
127.0.0.1:6379> ZREM china:city shenzhen #移除shenzhen
1
127.0.0.1:6379> ZRANGE china:city 0 -1
shanghai
北京
127.0.0.1:6379> 

```



## HyperLogLog

### 简介

Redis 在 2.8.9 版本添加了 HyperLogLog **数据结构**。

**应用场景**：Redis HyperLogLog 是用来做**基数统计的算法**，HyperLogLog 的**优点**是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

HyperLogLog则是一种算法，它提供了**不精确**的**去重计数方案**。

**举个例子：**

假如我要**统计网页的UV**（浏览用户数量，一天内同一个用户多次访问只能算一次），

- **传统的解决方案**是使用Set来保存用户id，然后统计Set中的元素数量来获取页面UV。但这种方案只能承载少量用户，一旦用户数量大起来就需要消耗大量的空间来存储用户id。我的目的是统计用户数量而不是保存用户，这简直是个吃力不讨好的方案！
- 而使用Redis的**HyperLogLog**最多需要12k就可以统计大量的用户数，尽管它大概有0.81%的错误率，但对于统计UV这种不需要很精确的数据是可以忽略不计的。

### 什么是基数

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, **基数**(不重复元素的个数)为5。

**基数估计**就是在误差可接受的范围内，快速计算基数。



### 基本命令

#### PFADD , PFCOUNT , PFMERGE

| **命令**                         | 描述                                          |
| -------------------------------- | --------------------------------------------- |
| [PFADD key element [element ...] | 添加指定元素到 HyperLogLog 中。               |
| [PFCOUNT key [key ...]           | 返回给定 HyperLogLog 的基数估算值。           |
| [PFMERGE destkey sourcekey       | 将多个 HyperLogLog 合并为一个 HyperLogLog，并 |

测试使用：

```bash
127.0.0.1:6379> PFADD mykey a b c d e # 去重添加
(integer) 1
127.0.0.1:6379> PFCOUNT mykey 
(integer) 5
127.0.0.1:6379> PFADD mykey2 e f g h 
(integer) 1
127.0.0.1:6379> PFCOUNT mykey2
(integer) 4
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2
OK
127.0.0.1:6379> PFCOUNT mykey3
(integer) 8
127.0.0.1:6379> 

```



总结：

- 如果允许容错，那么一定可以使用hyperloglog 
- 如果不允许容错，就使用set 或者自己的数据类型即可。	

## BitMap

### 简介

**应用场景**：在开发中，可能会遇到这种情况：需要统计用户的某些信息，如活跃或不活跃，登录或者不登录；又如需要记录用户一年的打卡情况，打卡了是1， 没有打卡是0，如果使用普通的 key/value存储，则要记录365条记录，如果用户量很大，需要的空间也会很大，所以 Redis 提供了 Bitmap 位图这中数据结构，

Bitmap 就是通过**操作二进制位**来进行记录，**只能为为0 或者 1**；如果要记录 365 天的打卡情况，使用 Bitmap表示的形式大概如下：0101000111000111...........................，这样有什么好处呢？当然就是**节约内存**了，365 天相当于 365 bit，又 1 字节 = 8 bit , 所以相当于使用 46 个字节即可。

**定义**：BitMap 就是通过一个 bit 位来表示某个元素对应的值或者状态, 其中的 key 就是对应元素本身，实际上底层也是通过对字符串的操作来实现。Redis 从 2.2 版本之后新增了setbit, getbit, bitcount 等几个bitmap 相关命令。

BitMap 位图，也是一种**数据结构**。

### 命令

#### setbit

语法：SETBIT key offset value

测试：

比如：使用bitmap 来记录，周一到周日的打卡

```bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 1
(integer) 0
127.0.0.1:6379> setbit sign 3 0
(integer) 0
127.0.0.1:6379> setbit sign 4 0
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> setbit sign 6 1
(integer) 0
127.0.0.1:6379> 

```

#### getbit

语法：GETBIT key offset

查看某一天是否打卡

```bash
127.0.0.1:6379> getbit sign 3
(integer) 0
127.0.0.1:6379> getbit sign 6
(integer) 1
127.0.0.1:6379> 
```

#### bitcount

语法： BITCOUNT key [start end]

统计操作，统计打卡的天数。

```bash
127.0.0.1:6379> BITCOUNT sign  # 统计这一周打开的天数，就可以判定，是否全勤
(integer) 4

```



# Redis 事务

## 理论

redis 事务官网：http://www.redis.cn/topics/transactions.html

==Redis 单条命令是保证原子性的，但是Redis事务是不保证原子性==

Redis 事务**本质**：是一组命令的集合。事务支持一次执行多个命令，一个事务中所有命令都会被序列化。在事务执行过程，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令序列中。

redis 事务特性: **一次性，顺序性，排它性。**

**Redis事务没有隔离级别的概念：**

批量操作在发送 EXEC 命令前被放入队列缓存，并不会被实际执行！

**Redis不保证原子性：**

Redis中，单条命令是原子性执行的，但事务不保证原子性，且**没有回滚**。事务中任意命令执行失败，其余的命令仍会被执行。

**Redis事务的三个阶段：**

- 开始事务 multi

- 命令入队 (...)

- 执行事务 exec

**Redis事务相关命令：**

```bash
watch key1 key2 ... #监视一或多个key,如果在事务执行之前，被监视的key被其他命令改动，则 事务被打断 （ 类似乐观锁 ） 
multi # 标记一个事务块的开始（ queued ） 
exec # 执行所有事务块的命令 （ 一旦执行exec后，之前加的监控锁都会被取消掉 ） 
discard # 取消事务，放弃事务块中的所有命令 
unwatch # 取消watch对所有key的监控
```

## 测试使用

### multi ，exec

[MULTI](http://www.redis.cn/commands/multi.html) 命令用于开启一个事务，它总是返回 `OK` 。 [MULTI](http://www.redis.cn/commands/multi.html) 执行之后， 客户端可以继续向服务器发送任意多条命令， 这些命令不会立即被执行， 而是被放到一个队列中， 当 [EXEC](http://www.redis.cn/commands/exec.html)命令被调用时， 所有队列中的命令才会被执行。

另一方面， 通过调用 [DISCARD](http://www.redis.cn/commands/discard.html) ， 客户端可以清空事务队列， 并放弃执行事务。

```bash
#开启事务
127.0.0.1:6379> MULTI 
OK
#命令入队
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
# 执行事务
127.0.0.1:6379(TX)> EXEC
1) OK
2) OK
3) "v2"
4) OK
127.0.0.1:6379> 
```

### discard

放弃事务

```bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> multi #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> DISCARD #取消事务
OK
127.0.0.1:6379> get k1 # 返回为空，得知，取消后，事务队列中的命令都不会执行
(nil)
127.0.0.1:6379> 

```



### 事务中的错误

使用事务时可能会遇上以下两种错误：

+ 类似java 编译性错误：事务在执行 [EXEC](http://www.redis.cn/commands/exec.html) 之前，入队的命令可能会出错。比如说，命令可能会产生语法错误（参数数量错误，参数名错误，等等），或者其他更严重的错误，比如内存不足（如果服务器使用 `maxmemory` 设置了最大内存限制的话）。
+  类似Java运行时错误：命令可能在 [EXEC](http://www.redis.cn/commands/exec.html) 调用之后失败。举个例子，事务中的命令可能处理了错误类型的键，比如将列表命令用在了字符串键上面，诸如此类。

对于发生在 [EXEC](http://www.redis.cn/commands/exec.html) 执行之前的错误，客户端以前的做法是检查命令入队所得的返回值：如果命令入队时返回 `QUEUED` ，那么入队成功；否则，就是入队失败。如果有命令在入队时失败，那么大部分客户端都会停止并取消这个事务。

不过，从 Redis 2.6.5 开始，服务器会对命令入队失败的情况进行记录，并在客户端调用 [EXEC](http://www.redis.cn/commands/exec.html) 命令时，拒绝执行并自动放弃这个事务。

#### **错误一：**

若在事务队列中存在命令性错误（类似于java编译性错误），则执行EXEC命令时，所有命令都不会执行

```bash
127.0.0.1:6379> clear
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> getset k4 # 语法错误，参数数量错误
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k5 v5
QUEUED
127.0.0.1:6379(TX)> EXEC# 执行事务报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k2 # 事务所有的命令都不会执行
(nil)
127.0.0.1:6379> 
```



#### **错误二：**

若在事务队列中存在语法性错误（类似于java的1/0的运行时异常），则执行EXEC命令时，其他正确命令会被执行，错误命令抛出异常。

```bash
127.0.0.1:6379> set k1 "v1"
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> INCR k1 #incr只能对数字自增
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (error) ERR value is not an integer or out of range #虽然第一个命令执行报错，但是不影响后面的
2) OK
3) OK
4) "v3"
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> 

```



### Watch 监控

watch 语法：WATCH key [key ...]

解释：标记所有指定的key 被监视起来，在事务中有条件的执行（乐观锁）。

==Redis实现乐观锁的方式，就是用watch==

**悲观锁：**

悲观锁(Pessimistic Lock),顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿到这个数据就会block直到它拿到锁。传统的关系型数据库里面就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在操作之前先上锁。

**乐观锁：**

乐观锁(Optimistic Lock),顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁。但是在更新的时候会判断一下再此期间别人有没有去更新这个数据，可以使用版本号（version）等机制，乐观锁适用于多读的应用类型，这样可以提高吞吐量，乐观锁策略：提交版本必须大于记录当前版本才能执行更新。

#### Redis watch 监控测试

比如，初始化存钱 money，和 花钱out

1、事务期间 money 数据未变动，事务正常执行成功，

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money #监视money对象
OK
127.0.0.1:6379> multi #事务正常结束，数据期间没有发生变动，这个时候就正常执行成功
OK
127.0.0.1:6379(TX)> DECRBY money 20
QUEUED
127.0.0.1:6379(TX)> INCRBY out 20
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (integer) 80
2) (integer) 20
127.0.0.1:6379> 

```

2、事务期间 money 数据变动，事务执行失败！

新建一个窗口二，在，原窗口一输入事务队列的时候，改变money值，模仿多线程，再在窗口一中 执行事务 exec ,则执行失败

```bash
#原窗口一
127.0.0.1:6379> watch money 
OK 
127.0.0.1:6379> multi # 执行完毕后，执行窗口二代码测试
OK
127.0.0.1:6379(TX)> DECRBY money 10
QUEUED
127.0.0.1:6379(TX)> incrby out 10
QUEUED
127.0.0.1:6379(TX)> EXEC  # 修改失败！
(nil)
127.0.0.1:6379> 

```

```bash
#窗口二
127.0.0.1:6379> get money
"80"
127.0.0.1:6379> set money 1000
OK
127.0.0.1:6379> 

```

出现问题后放弃监视（unwatch），然后重来！

```bash
#窗口一：出现问题后放弃监视，然后重来！
127.0.0.1:6379> UNWATCH #放弃监视
OK
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> DECRBY money 30
QUEUED
127.0.0.1:6379(TX)> incrby out 30
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (integer) 970
2) (integer) 50
127.0.0.1:6379> 
```



#### watch 小结

watch指令类似于乐观锁，在事务提交时，如果watch监控的多个KEY中任何KEY的值已经被其他客户端更改，则使用EXEC执行事务时，事务队列将不会被执行，同时返回Nullmulti-bulk应答以通知调用者事务执行失败。



# Jedis

- 使用java 连接 Redis
- Jedis 的 github官网：https://github.com/redis/jedis ，可以看它的测试代码。研究怎么实现

 **Jedis 定义** :Redis 官方推荐的java 连接开发工具。使用java 操作 redis 的中间件。

如果要使用java 操作redis ，一定要熟悉 Jedis



## 测试使用

1. 新建一个普通的Maven项目

2. 导入redis的依赖！

   ```bash
       <dependencies>
   <!--        jedis 包-->
           <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
           <dependency>
               <groupId>redis.clients</groupId>
               <artifactId>jedis</artifactId>
               <version>3.5.2</version>
           </dependency>
   <!--fastjson 用来存数据-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.75</version>
           </dependency>
       </dependencies>
   ```

3. 编码测试

   1. 远程连接服务器需要的操作：参考博客：https://blog.csdn.net/weixin_43423864/article/details/109087670
      1. 修改 /usr/local/bin/joker_config/redis.conf 配置文件
         1. 这个是我之前就做 了。保证redis 可以后台运行，daemonize yes
         
         2. 在# requirepass foobared 下添加 requirepass 123456 。注意：这里如果再要在服务器上使用客户端redis-cli ,需要认证，输入密码，
         
            ```bash
            [root@joker ~]# redis-cli -p 6379
            127.0.0.1:6379> keys *
            (error) NOAUTH Authentication required.
            127.0.0.1:6379> AUTH 123456
            OK
            
            ```
         
            
         
         3. 注释掉 bind 127.0.0.1 -::1
         
         4. 找到protected-mode yes 修改为no
         
      2. 重新启动配置文件。redis-server joker_config/redis.conf
      
      3. 注意开启防火墙和安全组的6379端口
      
      4. 修改完，可能要等一会再运行程序，才可以连接成功。也可以重启一下防火墙(centos7) （ firewall-cmd --reload ）
      
   2. 连接redis数据库

   3. 操作命令

   4. 断开连接

   

   测试ping 

   ```java
   package com.joker;
   
   import redis.clients.jedis.Jedis;
   
   public class TestPing {
       public static void main(String[] args) {
           //new Jedis 对象即可
           Jedis jedis = new Jedis("47.xxx.xxx.xxx",6379);
           jedis.auth("your_password");
           //Jedis 所有的函数，就是redis的所有指令，
           System.out.println(jedis.ping());
   
       }
   }
   ```

   

   结果：

   ![image-20210404013526600](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404013526600.png)

   

## jedis 常用api

就是之前讲的redis的命令：String,List,Set,Hash,Zset。。。

### 基本连接操作

```java
public class TestPassword {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        //验证reids配置文件的 requirepass 密码，如果没有设置密码这段代码省略
        jedis.auth("123456");
        jedis.connect();//连接
        jedis.disconnect(); //断开连接
        jedis.flushAll(); //清空所有的key
    }
}
```

### 关于key的操作

```java
package com.joker;

import redis.clients.jedis.Jedis;

import java.util.Set;

/**
 * Jedis 的关于key 的操作
 */
public class TestKey {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");
        jedis.connect();
        
        System.out.println("清空数据："+jedis.flushDB());
        System.out.println("判断某个键是否存在："+jedis.exists("username"));
        System.out.println("新增<'username','kuangshen'>的键值 对："+jedis.set("username","kuangshen"));
        System.out.println("新增<'password','password'>的键值 对："+jedis.set("password","password"));
        System.out.print("系统中所有的键如下：");

        Set<String> keys = jedis.keys("*");
        System.out.println(keys);
        System.out.println("删除键password:"+jedis.del("password"));
        System.out.println("判断键password是否存 在："+jedis.exists("password"));
        System.out.println("查看键username所存储的值的类 型："+jedis.type("username"));
        System.out.println("随机返回key空间的一个："+jedis.randomKey());
        System.out.println("重命名key："+jedis.rename("username","name"));
        System.out.println("取出改后的name："+jedis.get("name"));
        System.out.println("按索引查询："+jedis.select(0));
        System.out.println("删除当前选择数据库中的所有key："+jedis.flushDB());
        System.out.println("返回当前数据库中key的数目："+jedis.dbSize());
        System.out.println("删除所有数据库中的所有key："+jedis.flushAll());
    }


}
```

### String 的操作

```java
package com.joker;

import redis.clients.jedis.Jedis;

import java.util.concurrent.TimeUnit;

public class TestString {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");

        jedis.flushDB();
        System.out.println("===========增加数据===========");
        System.out.println(jedis.set("key1","value1"));
        System.out.println(jedis.set("key2", "value2"));
        System.out.println(jedis.set("key3", "value3"));
        System.out.println("删除键key2:" + jedis.del("key2"));
        System.out.println("获取键key2:"+jedis.get("key2"));
        System.out.println("修改key1:"+jedis.set("key1", "value1Changed"));
        System.out.println("获取key1的值："+jedis.get("key1"));
        System.out.println("在key3后面加入值："+jedis.append("key3", "End"));
        System.out.println("key3的值："+jedis.get("key3"));
        System.out.println("增加多个键值 对："+jedis.mset("key01","value01","key02","value02","key03","value03"));
        System.out.println("获取多个键值 对："+jedis.mget("key01","key02","key03"));
        System.out.println("获取多个键值 对："+jedis.mget("key01","key02","key03","key04"));
        System.out.println("删除多个键值对："+jedis.del("key01","key02"));
        System.out.println("获取多个键值 对："+jedis.mget("key01","key02","key03"));

        jedis.flushDB();
        System.out.println("===========新增键值对防止覆盖原先值==============");
        System.out.println(jedis.setnx("key1", "value1"));
        System.out.println(jedis.setnx("key2", "value2"));
        System.out.println(jedis.setnx("key2", "value2-new"));
        System.out.println(jedis.get("key1"));
        System.out.println(jedis.get("key2"));

        System.out.println("===========新增键值对并设置有效时间=============");
        System.out.println(jedis.setex("key3", 2, "value3"));
        System.out.println(jedis.get("key3"));
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(jedis.get("key3"));

        System.out.println("===========获取原值，更新为新值==========");
        System.out.println(jedis.getSet("key2", "key2GetSet"));
        System.out.println(jedis.get("key2"));

        System.out.println("获得key2的值的字串：" + jedis.getrange("key2", 2, 4));
    }
}
```

### List的操作

```java
package com.joker;

import redis.clients.jedis.Jedis;

/**
 * jedis 关于List操作的命令
 */
public class TestList {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");

        jedis.flushDB(); System.out.println("===========添加一个list===========");
        jedis.lpush("collections", "ArrayList", "Vector", "Stack", "HashMap", "WeakHashMap", "LinkedHashMap");
        jedis.lpush("collections", "HashSet");
        jedis.lpush("collections", "TreeSet");
        jedis.lpush("collections", "TreeMap");
        //-1代表倒数第一个元素，-2代表倒数第二个元素,end为-1表示查询全部
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("collections区间0-3的元 素："+jedis.lrange("collections",0,3));
        System.out.println("===============================");
        // 删除列表指定的值 ，第二个参数为删除的个数（有重复时），后add进去的值先被删，类 似于出栈
        System.out.println("删除指定元素个数："+jedis.lrem("collections", 2, "HashMap"));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("删除下表0-3区间之外的元 素："+jedis.ltrim("collections", 0, 3));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("collections列表出栈（左 端）："+jedis.lpop("collections"));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("collections添加元素，从列表右端，与lpush相对 应："+jedis.rpush("collections", "EnumMap"));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("collections列表出栈（右 端）："+jedis.rpop("collections"));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("修改collections指定下标1的内 容："+jedis.lset("collections", 1, "LinkedArrayList"));
        System.out.println("collections的内容："+jedis.lrange("collections", 0, -1));
        System.out.println("===============================");
        System.out.println("collections的长度："+jedis.llen("collections"));
        System.out.println("获取collections下标为2的元 素："+jedis.lindex("collections", 2));
        System.out.println("===============================");
        jedis.lpush("sortedList", "3","6","2","0","7","4");
        System.out.println("sortedList排序前："+jedis.lrange("sortedList", 0, -1));
        System.out.println(jedis.sort("sortedList"));
        System.out.println("sortedList排序后："+jedis.lrange("sortedList", 0, -1));
    }
}
```



### Set 的操作

```java
package com.joker;

import redis.clients.jedis.Jedis;

/**
 * Jedis 的set操作命令
 */
public class TestSet {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");

        jedis.flushDB();
        System.out.println("============向集合中添加元素（不重复） ============");
        System.out.println(jedis.sadd("eleSet", "e1","e2","e4","e3","e0","e8","e7","e5"));
        System.out.println(jedis.sadd("eleSet", "e6"));
        System.out.println(jedis.sadd("eleSet", "e6"));
        System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
        System.out.println("删除一个元素e0："+jedis.srem("eleSet", "e0"));
        System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
        System.out.println("删除两个元素e7和e6："+jedis.srem("eleSet", "e7","e6"));
        System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
        System.out.println("随机的移除集合中的一个元素："+jedis.spop("eleSet"));
        System.out.println("随机的移除集合中的一个元素："+jedis.spop("eleSet"));
        System.out.println("eleSet的所有元素为："+jedis.smembers("eleSet"));
        System.out.println("eleSet中包含元素的个数："+jedis.scard("eleSet"));
        System.out.println("e3是否在eleSet中："+jedis.sismember("eleSet", "e3"));
        System.out.println("e1是否在eleSet中："+jedis.sismember("eleSet", "e1"));
        System.out.println("e1是否在eleSet中："+jedis.sismember("eleSet", "e5"));
        System.out.println("=================================");
        System.out.println(jedis.sadd("eleSet1", "e1","e2","e4","e3","e0","e8","e7","e5"));
        System.out.println(jedis.sadd("eleSet2", "e1","e2","e4","e3","e0","e8"));
        System.out.println("将eleSet1中删除e1并存入eleSet3 中："+jedis.smove("eleSet1", "eleSet3", "e1"));//移到集合元素
        System.out.println("将eleSet1中删除e2并存入eleSet3 中："+jedis.smove("eleSet1", "eleSet3", "e2"));
        System.out.println("eleSet1中的元素："+jedis.smembers("eleSet1"));
        System.out.println("eleSet3中的元素："+jedis.smembers("eleSet3"));
        System.out.println("============集合运算=================");
        System.out.println("eleSet1中的元素："+jedis.smembers("eleSet1"));
        System.out.println("eleSet2中的元素："+jedis.smembers("eleSet2"));
        System.out.println("eleSet1和eleSet2的交 集:"+jedis.sinter("eleSet1","eleSet2"));
        System.out.println("eleSet1和eleSet2的并 集:"+jedis.sunion("eleSet1","eleSet2"));
        System.out.println("eleSet1和eleSet2的差 集:"+jedis.sdiff("eleSet1","eleSet2"));//eleSet1中有，eleSet2中没有
        jedis.sinterstore("eleSet4","eleSet1","eleSet2");//求交集并将交集保存到 dstkey的集合
        System.out.println("eleSet4中的元素："+jedis.smembers("eleSet4"));
    }
}
```

### Hash的操作

```java
package com.joker;

import redis.clients.jedis.Jedis;

import java.util.HashMap;
import java.util.Map;

/**
 * Jedis 的关于key 的操作
 */
public class TestHash {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");

        jedis.flushDB();
        Map<String,String> map = new HashMap<>();
        map.put("key1","value1");
        map.put("key2","value2");
        map.put("key3","value3");
        map.put("key4", "value4");
        //添加名称为hash（key）的hash元素
        jedis.hmset("hash",map);
        // 向名称为hash的hash中添加key为key5，value为value5元素
        jedis.hset("hash", "key5", "value5");
        System.out.println("散列hash的所有键值对 为："+jedis.hgetAll("hash"));//return Map<String,String>
        System.out.println("散列hash的所有键为："+jedis.hkeys("hash"));//return Set<String>
        System.out.println("散列hash的所有值为："+jedis.hvals("hash"));//return List<String>
        System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加 key6："+jedis.hincrBy("hash", "key6", 6));
        System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
        System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加 key6："+jedis.hincrBy("hash", "key6", 3));
        System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
        System.out.println("删除一个或者多个键值对："+jedis.hdel("hash", "key2"));
        System.out.println("散列hash的所有键值对为："+jedis.hgetAll("hash"));
        System.out.println("散列hash中键值对的个数："+jedis.hlen("hash"));
        System.out.println("判断hash中是否存在 key2："+jedis.hexists("hash","key2"));
        System.out.println("判断hash中是否存在 key3："+jedis.hexists("hash","key3"));
        System.out.println("获取hash中的值："+jedis.hmget("hash","key3"));
        System.out.println("获取hash中的 值："+jedis.hmget("hash","key3","key4"));
    }
}
```

### 关于事务的操作

```java
package com.joker;

import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

/**
 * Jedis 关于事务的操作
 */
public class TestMulti {
    public static void main(String[] args) {
        //创建客户端连接服务端，redis服务端需要被开启
        Jedis jedis = new Jedis("47.xxx.xxx.xxx", 6379);
        jedis.auth("123456");
        jedis.flushDB();
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello", "world");
        jsonObject.put("name", "java");
        //开启事务
        Transaction multi = jedis.multi();
        String result = jsonObject.toJSONString();
        try {
            //向redis存入一条数据
            multi.set("json", result);
            //再存入一条数据
            multi.set("json2", result);
            //这里引发了异常，用0作为被除数
            int i = 100 / 0;
            //如果没有引发异常，执行进入队列的命令
            multi.exec();
        } catch (Exception e) {
            e.printStackTrace();
            //如果出现异常，回滚
            multi.discard();
        } finally {
            System.out.println(jedis.get("json"));
            System.out.println(jedis.get("json2"));
            //最终关闭客户端
            jedis.close();
        }

    }

}
```



# SpringBoot 整合

- Spring Boot 操作数据 都封装在 SpringData 里：比如操作 jpa, jdbc , mongodb , redis。。。
- SpringData 也是和Spring Boot齐名的项目。
- SpringData  官网：https://spring.io/projects/spring-data
- 说明，在SpringBoot2.x 之后,底层原来使用的 Jedis 已经被替换为 lettuce客户端
  - Jedis ：采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全，使用jedis pool 连接池。类似于 BIO 模式
  - lettuce : 采用的netty ,实例可以在多个线程中共享。不存在，线程不安全的情况。可以减少线程数量。更像NIO模式。

**源码分析：**

- springBoot 可以看到

  - 自动配置类 RedisAutoConfiguration
  - 配置文件类 RedisProperties

- RedisAutoConfiguration 里：

  ```java
  @Bean
  @ConditionalOnMissingBean(
    name = {"redisTemplate"}
  )//我们可以自己定义一个 redisTemplate 来替换这个默认的
  @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
  public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
      //默认的 RedisTemplate 没有过多的设置，但 redis对象都需要序列化，所以等会会加操作
      //两个泛型都是Object ,我们后面使用想要强制转换<String,Object>
      RedisTemplate<Object, Object> template = new RedisTemplate();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
  }
  
  @Bean
  @ConditionalOnMissingBean
  @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
  public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
      //因为String 是redis最常用的，所以单独有个模板
      StringRedisTemplate template = new StringRedisTemplate();
      template.setConnectionFactory(redisConnectionFactory);
      return template;
  }
  ```

## 整合测试

步骤总结：

- 导入依赖（新建一个SpringBoot项目）
- 配置连接
- 测试

1. 新建一个Springboot 模块，选择好redis的依赖，第一个Spring Native 可以不勾选，我这下载不到依赖。这里就已经下载好依赖了

   ![image-20210404170409920](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404170409920.png)

   ```xml
   <!--        操作redis-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-data-redis</artifactId>
           </dependency>
   ```

2. 配置连接 application.properties

   ```properties
   spring.redis.host=47.xxx.xxx.xxx
   spring.redis.port=6379
   spring.redis.password=123456
   ```

3. 编写测试 就这test文件夹下测试

   ```java
   @SpringBootTest
   class Redis02SpringbootApplicationTests {
       @Autowired
       private RedisTemplate redisTemplate;
       @Test
       void contextLoads() {
           //redisTemplate 操作不同的
           //opsForValue 操作字符串，类似String
           //opsForList 操作List，类似List
           //....
           //常用的方法，都可以直接用 redisTemplate 操作，比如事务和基本的增删改查
   
           //获取redis的连接对象 一般很少用
   //        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
   //        connection.flushDb();
   //        connection.flushAll();
   
           redisTemplate.opsForValue().set("mykey","joker");
           System.out.println(redisTemplate.opsForValue().get("mykey"));
       }
   
   }
   ```



### 看源码：RedisTemplate 类

![image-20210404175506433](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404175506433.png)

![image-20210404175628914](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404175628914.png)

所以，需要自己定义一个配置类



关于序列化的保存

```java
    @Test
    public void test() throws JsonProcessingException {
        //真实的开发，一般用json来传递对象数据
        User user = new User("joker", 22);
        //ObjectMapper 是jackson 里面的，这里是把对象转换为字符串
//        String jsonUser = new ObjectMapper().writeValueAsString(user);
        //如果直接传递一个没有序列化的对象，会直接报错，所以需要把对象序列化
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
```

![image-20210404181133398](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404181133398.png)



### 自定义 redisTemplate

就是主要重新配置序列化方式。 因为默认的序列化方式jdk，存储中文到服务器会乱码。

创建配置类的**原理讲解**

```java
//编写自己的redisTemplate 类，把源码的复制过来，改一下
@Bean
public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    RedisTemplate<String, Object> template = new RedisTemplate();
    Jackson2JsonRedisSerializer<Object> objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Object>();
    //配置具体的序列化方式
    //看源码  setKeySerializer 参数类型是 RedisSerializer 接口，它有很多实现类，就是各种序列化方式，默认是jdk序列化
    //用那种序列方式，就去创建一个该方式的对象，比如这里用jackson的
    template.setKeySerializer(objectJackson2JsonRedisSerializer);

    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
```

自定义 redisTemplate 的**完整版，**可以在开发中，拿来即用。

```java
package com.joker.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {
    //
    //自己定义 了一个 RedisTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        //我们为了自己开发方便，一般直接使用<String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);
        //序列化配置
        //ObjectMapper用来转义
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        //String 的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

### 编写 RedisUtil 类

```java
package com.joker.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

@Component
public class RedisUtil {
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // =============================common============================
    // /*** 指定缓存失效时间 * @param key 键 * @param time 时间(秒) */
    public boolean expire(String key, long time) {
        try {

            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     *
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }

    /**
     * 判断key是否存在
     * * @param key 键
     * * @return true 存在 false不存在
     * */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * * 删除缓存
     * * @param key 可以传一个值 或多个
     * */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete((Collection<String>) CollectionUtils.arrayToList(key));
            }
        }
    }
    // ============================String=============================

    /**
     * * 普通缓存获取
     * * @param key 键
     * * @return 值
     * */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /*** 普通缓存放入 * @param key 键 * @param value 值 * @return true成功 false失败 */
    public boolean set(String key, Object value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 普通缓存放入并设置时间
     * @param key 键
     * @param value 值
     * @param time 时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */
    public boolean set(String key, Object value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 递增
     * @param key 键
     * @param delta 要增加几(大于0)
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }

    /**
     * 递减
     * @param key 键
     * @param delta 要减少几(小于0)
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }

    // ================================Map=================================

    /**
     * HashGet
     * @param key 键 不能为null
     * @param item 项 不能为null
     */
    public Object hget(String key, String item) {
        return redisTemplate.opsForHash().get(key, item);
    }

    /**
     * 获取hashKey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     * @param key 键
     * @param map 对应多个键值
     */
    public boolean hmset(String key, Map<String, Object> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * HashSet 并设置时间
     * @param key 键
     * @param map 对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, Object> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     * @param key 键 * @param item 项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建 
     * @param key 键 * @param item 项
     * @param value 值 * @param time 时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     *  删除hash表中的值 
     *  @param key 键 不能为null
     *  @param item 项 可以使多个 不能为null 
     */
    public void hdel(String key, Object... item) {
        redisTemplate.opsForHash().delete(key, item);
    }

    /**
     * 判断hash表中是否有该项的值 
     * @param key 键 不能为null 
     * @param item 项 不能为null 
     * @return true 存在 false不存在 
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }

    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回 
     * @param key 键 * @param item 项 
     * @param by 要增加几(大于0) 
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }

    /**
     *  hash递减 
     *  @param key 键 
     *  @param item 项 
     *  @param by 要减少记(小于0) 
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }

    // ============================set=============================

    /**
     * 根据key获取Set中的所有值 
     * @param key 键 
     */
    public Set<Object> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     *  根据value从一个set中查询,是否存在 
     *  @param key 键 * @param value 值 
     *  @return true 存在 false不存在 
     */
    public boolean sHasKey(String key, Object value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     *  将数据放入set缓存 
     *  @param key 键 
     *  @param values 值 可以是多个 
     *  @return 成功个数 
     */
    public long sSet(String key, Object... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     *  将set数据放入缓存 
     *  @param key 键 
     *  @param time 时间(秒) 
     *  @param values 值 可以是多个 
     *  @return 成功个数 
     */
    public long sSetAndTime(String key, long time, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0) expire(key, time);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     *  获取set缓存的长度
     *  @param key 键 
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     *  移除值为value的 
     *  @param key 键 
     *  @param values 值 可以是多个 
     *  @return 移除的个数 
     */
    public long setRemove(String key, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    // ===============================list=================================

    /**
     * 获取list缓存的内容 
     * @param key 键 
     * @param start 开始 
     * @param end 结束 0 到 -1代表所有值 
     */
    public List<Object> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 获取list缓存的长度 *
     * @param key 键 
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 通过索引 获取list中的值 *
     * @param key 键 
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0 时，-1，表尾，-2倒数第二个元素，依次类推 
     */
    public Object lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 将list放入缓存 *
     * @param key 键 
     * @param value 值 
     */
    public boolean lSet(String key, Object value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存 
     * @param key 键 
     * @param value 值 
     * @param time 时间(秒) 
     */
    public boolean lSet(String key, Object value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0) expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存 *
     * @param key 键 
     * @param value 值 
     * @return 
     */
    public boolean lSet(String key, List<Object> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存
     * @param key 键
     * @param value 值
     * @param time 时间(秒) 
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0) expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据索引修改list中的某条数据
     * @param key 键
     * @param index 索引
     * @param value 值
     * @return
     */
    public boolean lUpdateIndex(String key, long index, Object value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 移除N个值为value
     * @param key 键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */
    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }
}
```

### 总结：

所有redis操作，其实非常简单，对于java开发人员，更重要是去理解 redis 的思想和每一种数据结构的用处。



# redis.conf 配置文件详解

- redis版本6.X
- redis6.0 官网配置文件 （网页版，要翻墙才看得到）:  https://raw.githubusercontent.com/redis/redis/6.0/redis.conf

启动redis的时候，就是通过 redis.conf 配置文件来启动。

redis.conf  ：最开始是描述启动redis-server 的语法

工作中，修改一下配置，就可以很厉害。

下面只列举基础重要的部分。

## units单位

![image-20210404214919580](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404214919580.png)

1. 配置文件 unit 单位 对大小写不敏感

## INCLUDES 包含

![image-20210404215158698](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404215158698.png)

1. 可以包含其它的配置文件，就类似springBoot的import



## NETWORK 网络

```bash
bind 127.0.0.1 -::1 #绑定的ip
protected-mode no #保护模式，一般是开启的，只是我这里之前远程连接，所以改为了no
port 6379 #端口
```



## GENERAL 通用

```bash
daemonize yes #以守护进程的方式运行(可以后台运行)，默认是no,需要手动设置为yes
pidfile /var/run/redis_6379.pid #如果以后台方式运行，就需要指定一个pid文件 （进程id文件）
# 日志
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably) 默认，生产环境
# warning (only very important / critical messages are logged)
loglevel notice  # 日志级别
logfile "" # 日志的文件位置名，为空，就是一个标准的输出
databases 16 #数据库的数量，默认是16个
always-show-logo no #是否总是显示redis的logo

```

## SNAPSHOTTING 快照(RDB配置)

持久化的时候，会使用。

持久化：在规定的时间内，执行了多少次操作，则会持久化到文件 （.rdb , .aof）

redis 是内存数据库，如果不持久化，数据就没了，数据断电及失。

![image-20210404231208332](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404231208332.png)



```bash
stop-writes-on-bgsave-error yes #持久化如果出错，是否还要继续工作，一般默认是要不继续的
rdbcompression yes #是否压缩rdb文件，需要消耗cpu资源。
rdbchecksum yes #保存rdb文件的时候，进行错误的校验
dir ./ #rdb文件保存的目录，默认当前文件夹
```



## REPLICATION  复制

- 主从复制相关，后面再说



## SECURITY 安全

```bash
# requirepass foobared # redis-server的密码，默认是没有密码的
```

一般是在客户端里设置密码， config set requirepass 123456

![image-20210404233237771](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210404233237771.png)



## CLIENTS 客户端

- 限制客户端的配置

```bash
# maxclients 10000 #能连接上redis 的最大客户端数量
```

## MEMORY MANAGEMENT 内存管理

```bash
# maxmemory <bytes> # redis配置最大的内存容量
maxmemory-policy noeviction # 内存达到上限的处理策略
#处理策略有下面6种
# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select one from the following behaviors:
#
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.

```



## APPEND ONLY MODE（AOF配置）

```bash
appendonly no #默认不开启AOF，默认使用RDB 方式持久化的。因为，在大部分情况下，rdb完全够用
appendfilename "appendonly.aof" #AOF 持久化文件的名字
# appendfsync always # 每次修改都会 同步。消耗性能。
appendfsync everysec #默认每秒执行一次同步，可能会丢失这1S的数据
# appendfsync no # 不执行同步。这个时候操作系统自己同步数据。速度最快

```

具体的配置，后面 redis 的持久化，再说



# Redis Persistence 持久化

- 面试和工作，持久化都是重点

Redis 是**内存数据库**，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以 Redis 提供了持久化功能。

## RDB (Redis DataBase)

![image-20210405142756067](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405142756067.png)

### 什么是RDB：

- 在指定的时间间隔内将内存中的**数据集快照 写入磁盘**，也就是行话讲的**Snapshot快照**，它恢复时是将快照文件直接读到内存里。

- Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了**极高的性能**。

- 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的**缺点**是**最后一次持久化后**的**数据可能丢失**。

- 默认的就是RDB，一般情况下不需要修改配置

- 有时候，生产环境中，会将这个文件备份。

- ==rdb保存的文件，就是 dump.rdb==

  ![image-20210405141228950](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405141228950.png)

- 如果想禁用RDB持久化的策略，只要不设置任何save指令，或者给save传入一个空字符串参数也可以。

- 若要修改完毕需要立马生效，可以手动使用 save 命令！立马生效 !

rdb save 规则配置:

![image-20210405143740649](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405143740649.png)

### FORK

Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量，环境变量，程序计数器等）数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程。

### rdb 其余命令解析

Stop-writes-on-bgsave-error：如果配置为no，表示你不在乎数据不一致或者有其他的手段发现和控制，默认为yes。

rbdcompression：对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩，如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。

rdbchecksum：在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能。默认为yes。

### rdb触发规则

1. save 规则满足的情况下，会自动触发 RDB规则

2. 执行flushALL命令，也会触发 RDB规则。不过是空的，没有任何意义。

3. 关掉 redis  服务时（shutdown）也会产生rdb文件

4. 命令save或者是bgsave

   - save 时只管保存，其他不管，全部阻塞

   - bgsave，Redis 会在后台异步进行快照操作，快照同时还可以响应客户端请求。可以通过 lastsave 命令获取最后一次成功执行快照的时间。

备份就会自动生成一个 dump.rdb 文件



### 如何回复rdb 文件

1. 只需要将rdb 文件放到，redis的启动目录下，就可以了。redis 启动的时候会自动检查dump.rdb文件，恢复其中的数据。

2. 查看dump.rdb存放的位置 

   ```bash
   127.0.0.1:6379> config get dir
   1) "dir"
   2) "/usr/local/bin/joker_config" #如果这个目录下存在 dump.rdb文件，启动就会恢复其中的数据。
   127.0.0.1:6379> 
   
   ```

总结：rdb 默认的配置几乎就够用了，但是还是要学习 AOF



### RDB优缺点

#### RDB 优点：

1、适合大规模的数据恢复

2、对数据完整性和一致性要求不高

#### RDB 缺点：

1、在一定间隔时间做一次备份，所以如果redis意外宕机的话，就会丢失最后一次快照后的所有修改

2、Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑。



### RDB 小结

![image-20210405143456487](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405143456487.png)

## AOF（Append Only File）

AOF：将我们所有的命令都记录下来，就像一个 history。恢复的时候，就把这个文件全部执行一遍

![image-20210405183945996](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405183945996.png)

### AOF 是什么

以**日志**的形式来记录**每个写操作**，将Redis执行过的所有指令记录下来（读操作不记录），**只许追加文件**但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

==Aof保存文件 是 appendonly.aof 文件==

### AOF 配置

![image-20210405182449234](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405182449234.png)

 ```bash
appendonly no # 是否以append only模式作为持久化方式，默认不开启，默认使用的是rdb方式持久化，这 种方式在许多应用中已经足够用了
appendfilename "appendonly.aof" # appendfilename AOF 文件名称 
appendfsync everysec # appendfsync aof持久化策略的配置 
	# no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快。 
	# always表示每次写入都执行fsync，以保证数据同步到磁盘。 
	# everysec表示每秒执行一次fsync，可能会导致丢失这1s数据。 
	
No-appendfsync-on-rewrite #重写时是否可以运用Appendfsync，用默认no即可，保证数据安 全性
Auto-aof-rewrite-min-size # 设置重写的基准值 
Auto-aof-rewrite-percentage #设置重写的基准值

 ```



### AOF 启动/修复/恢复

**启动：**一般开启AOF ，只需要修改 appendnoly 为yes 。其它保持默认就好。重启redis-server就可以生效了

**正常恢复：**

- 启动：设置Yes，修改默认的appendonly no，改为yes

- 将有数据的aof文件复制一份保存到对应目录（config get dir）

- 恢复：重启redis然后重新加载

**异常恢复：**

- 启动：设置Yes

- 故意破坏 appendonly.aof 文件！（即在里面随便加一些字符就行）
- 这时候启动redis-server 就会报错。

- 修复： redis-check-aof --fix appendonly.aof 进行修复

- 恢复：重启 redis 然后重新加载

### Rewrite 重写

![image-20210405184554121](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405184554121.png)

**是什么：**

AOF 采用文件追加方式，文件会越来越大，为避免出现此种情况，新增了重写机制，当AOF文件的大小超过所设定的阈值时，Redis 就会启动AOF 文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令 bgrewriteaof ！

**重写原理：**

AOF 文件持续增长而过大时，会fork出一条新进程来将文件重写（也是先写临时文件最后再rename），遍历新进程的内存中数据，每条记录有一条的Set语句。重写aof文件的操作，并没有读取旧的aof文件，这点和快照有点类似！

**触发机制：**

Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的已被且文件大于64M的触发。



### AOF 优缺点

**优点：**

1. 每一次修改都同步，文件的完整性会更好。
2. 默认是每秒同步一次，可能丢失那一秒的数据。

**缺点**：

1. 相对于数据文件来说，AOF 远远大于 RDB ，修复的速度，也比rdb慢。
2.  AOF 运行效率比 RDB 慢，所以redis 默认是RDB。



## 持久化总结

1、RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储

2、AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。

3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化

4、同时开启两种持久化方式

- 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
- RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。

5、性能建议

- 主从复制中， RDB 在从机上就是备用的。aof几乎不适用

- 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 save 900 1 这条规则。

- 如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。

- 如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时宕机（比如：服务器断电），会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。



# Redis Pub/Sub (发布订阅)

Redis Pub/Sub :  https://redis.io/topics/pubsub

## 是什么

Redis 发布订阅(pub/sub)是一种**消息通信模式**：发送者(pub)发送消息，订阅者(sub)接收消息。

Redis 客户端可以订阅任意数量的频道。

订阅/发布消息图：

![image-20210405190156316](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405190156316.png)

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![image-20210405190312683](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405190312683.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20210405190332405](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405190332405.png)

## 命令

这些命令被广泛用于构建即时通信应用，比如网络聊天室(chatroom)和实时广播、实时提醒等。

+ [PSUBSCRIBE](https://redis.io/commands/psubscribe)
+ [PUBLISH](https://redis.io/commands/publish)
+ [PUBSUB](https://redis.io/commands/pubsub)
+ [PUNSUBSCRIBE](https://redis.io/commands/punsubscribe)
+ [SUBSCRIBE](https://redis.io/commands/subscribe)
+ [UNSUBSCRIBE](https://redis.io/commands/unsubscribe)

![image-20210405190538613](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405190538613.png)



## 测试

步骤：

- 开两个客户端窗口
- 先输入订阅者
- 再输入发布者
- 就可以即时在订阅者看到发布者发的消息

订阅者窗口：

```bash
127.0.0.1:6379> clear
127.0.0.1:6379> SUBSCRIBE joker-channel #订阅一个channel (频道) joker-channel
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "joker-channel"
3) (integer) 1
1) "message"
2) "joker-channel"
3) "hello,pub/sub"

```

**发布者窗口**

```bash
127.0.0.1:6379> PUBLISH joker-channel "hello,pub/sub"
(integer) 1
127.0.0.1:6379> 

```



## 原理

Redis是使用C实现的，通过分析 Redis 源码里的 pubsub.c 文件，了解发布和订阅机制的底层实现，籍此加深对 Redis 的理解。

Redis 通过 PUBLISH 、SUBSCRIBE 和 PSUBSCRIBE 等命令实现发布和订阅功能。

通过 SUBSCRIBE 命令订阅某频道后，redis-server 里维护了一个字典，字典的键就是一个个 channel（频道），而字典的值则是一个链表，链表中保存了所有订阅这个 channel 的客户端。SUBSCRIBE 命令的关键，就是将客户端添加到给定 channel 的订阅链表中。

通过 PUBLISH 命令向订阅者发送消息，redis-server 会使用给定的频道作为键，在它所维护的 channel字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。

**Pub/Sub使用场景**：

- 实时消息系统

- 实时聊天系统（频道当做聊天室，将信息回显给所有人即可）

- 订阅，关注系统

稍微复杂的场景就会使用 消息中间件 MQ 来做



# Redis Replication 主从复制

全名：leader follower (master-slave) replication

Redis Replication 官网：https://redis.io/topics/replication



## 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower)；**数据的复制是单向的，只能由主节点到从节点**。Master以**写**为主，Slave 以**读**为主。



默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。



主从复制的**作用**主要包括：

1、数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2、故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。

3、负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。

4、高可用基石：除了上述作用以外，主从复制还是**哨兵和集群**能够实施的基础，因此说主从复制是Redis高可用的基础。高可用大部分都是针对集群。



一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的，原因如下：

1、从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大；

2、从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，**单台Redis最大使用内存不应该超过20G**。



电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"多读少写"。

对于这种场景，我们可以使如下这种架构：

![image-20210405193309377](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405193309377.png)



注：

- 主从复制，读写分离。因为80%情况都是在读。减缓服务器的压力，架构中经常使用，最低配就是一主二从
- 只要在公司中，主从复制就是必须要使用的。因为真实的项目中，不可能单机使用redis



## 环境配置

- 这里的话，就是单机多集群，因为我只有一个服务器

### 基本配置

- 只配置从库，不配置主库。因为 redis 默认自己本身就是一个主库

```bash
127.0.0.1:6379> info replication #查看当前库的信息
# Replication
role:master # 角色 
connected_slaves:0 #没有从机
master_failover_state:no-failover
master_replid:0a3ee1f1d5fc644ab07c05b387e81687909e135a
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> 

```

- 真实的主从配置，需要在配置文件里修改。这里，我只是在客户端命令里修改。
- 每次与 master 断开之后，都需要重新连接，除非你配置进 redis.conf 文件！

**准备工作**：我们配置主从复制，至少需要三个，一主二从！配置三个客户端！还需要开一个窗口来测试用。

1、拷贝多个redis.conf 文件

![image-20210405212545535](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405212545535.png)

2、指定端口 6379，依次类推

3、开启daemonize yes

4、Pid文件名字 pidfile /var/run/redis_6379.pid , 依次类推

5、Log文件名字 logfile "6379.log" , 依次类推

6、Dump.rdb 名字 dbfilename dump6379.rdb , 依次类推

上面都配置完毕后，3个服务通过3个不同的配置文件开启，我们的准备环境就OK 了！

![image-20210405212853386](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405212853386.png)

## 一主二从

- ==默认情况下，每台Redis服务器都是主节点==，一般情况下，只用配置从机就好。
- 真实的主从配置，需要在配置文件里修改，才可以永久的主从配置。这里，我只是在客户端命令里修改，只是暂时的主从，每次与 master 断开之后，都需要重新连接。

- 一主（6379）二从（6380，6381）



1、环境初始化，客户端redis-cli连接，默认三个都是Master 主节点

2、配置主从配置为一个Master 两个Slave。主机不用变，两个从机配置slaveof 

```bash
slaveof 主库ip 主库端口 # 配置主从
slaveof 127.0.0.1 6379
```



结果：在主机设置值，在从机都可以取到！但是从机不能写值！



测试一：

- 主机挂了 （shutdown），查看从机信息。
  - 结果：从机还是从机，连接到主机，除非你从新配置，它才会变成主机
- 主机再恢复，再次查看信息
  - 从机依旧可以直接获取主机写进去的信息

测试二：

- 从机挂了，查看主机信息
  -  结果：主机的信息里挂的从机就没有了。
- 从机再恢复，查看从机信息
  - 结果：如果是命令行配置的主从复制，从机会变回主机。
  - 如果再把从机设置为主机的从机。立马就会从主机中获取值。依然可以读到，从机断开时，主机写入的值。



主从配置，如果在**配置文件**里修改：

需要修改 replicaof，如果有必要，可以添加masterauth 配置完就是从机了，主机不用配置

![image-20210405215038314](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405215038314.png)



细节

- 主机可以写，从机不能写只能读。主机中所有信息和数据，都会自动被从机保存

### 复制原理

Slave 启动成功连接到 master 后会发送一个sync（同步）命令

Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，**master将传送整个数据文件到slave，并完成一次完全同步。**



**全量复制**：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

**增量复制**：Master 继续将新的所有收集到的修改命令依次传给slave，完成同步

但是只要是重新连接master，一次完全同步（全量复制）将被自动执行。也就是说，主机的数据一定可以在从机中看到。

### 层层链路

层层链路：这个名字随便取的，表达这种模式。

上一个Slave 可以是下一个slave 和 Master，Slave 同样可以接收其他 slaves 的连接和同步请求，那么该 slave 作为了链条中下一个的master，可以有效减轻 master 的写压力！

![image-20210405221532201](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405221532201.png)

测试：6379 设置值以后 6380 和 6381 都可以获取到！OK！

### 谋朝篡位

这个名字也是乱取的

一主二从的情况下，如果主机断了，从机可以使用命令 SLAVEOF NO ONE 将自己改为主机！这个时候其余的从机链接到这个节点。对一个从属服务器执行命令 SLAVEOF NO ONE 将使得这个从属服务器关闭复制功能，并从从属服务器转变回主服务器，原来同步所得的数据集不会被丢弃。

如果原来的主机回来了，那它就没有从机了，只能重新配置。



**注意**：上面的 一主二从，层层链路，谋朝篡位啥的，都不会实际应用，只是为了**了解原理**。主要还是用哨兵模式。哨兵模式才是重点。



## 哨兵模式 （Sentinel）

- ==重点==

- 自动选举 主节点 的模式
- 在/usr/local/bin 下可以看到一个 redis-sentinel 链接
- ![image-20210405223713590](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405223713590.png)

### 概述

主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了Sentinel（哨兵） 架构来解决这个问题。

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了根据投票数==自动将从库转换为主库。==

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个**独立的进程**，作为进程，它会独立运行。其**原理**是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

![image-20210405222905177](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405222905177.png)

这里的哨兵有两个**作用**

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。

- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让它们切换主机。



然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。

各个哨兵之间还会进行监控，这样就形成了**多哨兵模式**。

![image-20210405223144604](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405223144604.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**客观下线**。

哨兵切换主机的投票算法，可以去 github 看看 redis 源码.

### 测试

1、调整结构，6379带着80、81

2、自定义的 /joker-config 目录下新建 sentinel.conf 文件，名字千万不要错

3、配置哨兵，填写内容。（实际配置哨兵肯定不止这么点，但这是最核心的）

- 语法：sentinel monitor 被监控主机名字 hostName hostPort 1  。【被监控主机名字】 可以随便写,表示当前主节点
  - 比如完整的 ：senitel monitor myredis 127.0.0.1 6379 1

- 上面最后一个数字1，表示主机挂掉后，哨兵投票 slave 看让谁接替成为主机，得票数多少后成为主机

4、启动哨兵，在/usr/local/bin 目录下

- redis-sentinel /joker-config/sentinel.conf

- 上述目录依照各自的实际情况配置，可能目录不同

5、正常主从演示

6、原有的Master 挂了，

7、则会投票新选，从从机中随机选择一个服务器，当主机。（这里有一个投票算法，可以看redis源码 ）

8、重新主从继续开工，info replication 查看 剩余的两个从机，随机一个成 了主机。

9、问题：如果之前的master 重启回来，会不会双master 冲突？ 之前的主机回来只能做从机了。



**哨兵日志**

![image-20210405225509843](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405225509843.png)

可以直接看到最新的主机是谁，



### 优缺点

**优点**

1. 哨兵集群模式是基于主从模式的，所有主从配置的优点，哨兵模式同样具有。

2. 主从可以切换，故障可以转移，系统可用性更好。

3. 哨兵模式是主从模式的升级，系统更健壮，可用性更高。从手动到自动。

**缺点**

1. Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。

2. 实现哨兵模式的配置也不简单，甚至可以说有些繁琐



### 哨兵模式的正式全部配置

- 这些配置，一般都是运维来配置

```bash
# Example sentinel.conf 

# 哨兵sentinel实例运行的端口 默认26379
# 如果哨兵有集群，那每个都要配置端口。
port 26379 

# 哨兵sentinel的工作目录 
dir /tmp 

# 哨兵sentinel监控的redis主节点的 ip port 
# master-name 可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。 
# quorum 配置多少个sentinel哨兵统一认为master主节点失联 那么这时客观上认为主节点失联了 
# sentinel monitor <master-name> <ip> <redis-port> <quorum> 
sentinel monitor mymaster 127.0.0.1 6379 2

# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都 要提供密码 
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码 
# sentinel auth-pass <master-name> <password> 
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd 

# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒 
# sentinel down-after-milliseconds <master-name> <milliseconds> 
sentinel down-after-milliseconds mymaster 30000 

# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同 步，
# 这个数字越小，完成failover所需的时间就越长， 
# 但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。 
# 可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。 
# sentinel parallel-syncs <master-name> <numslaves> 
sentinel parallel-syncs mymaster 1 

# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。 
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的 master那里同步数据时。 
#3.当想要取消一个正在进行的failover所需要的时间。 
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超 时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了 
# 默认三分钟 # sentinel failover-timeout <master-name> <milliseconds> 
sentinel failover-timeout mymaster 180000 

# SCRIPTS EXECUTION 

#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮 件通知相关人员。 
#对于脚本的运行结果有以下规则： 
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10 
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执 行。

# 通知型脚本:
# 当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等 等），将会去调用这个脚本，这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常 运行的信息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果 sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执 行的，否则sentinel无法正常启动成功。 
# 通知脚本
# 需要去了解一下 shell编程
# sentinel notification-script <master-name> <script-path> 
sentinel notification-script mymaster /var/redis/notify.sh 

# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master 地址已经发生改变的信息。 
# 以下参数将会在调用脚本时传给脚本: 
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port> 
# 目前<state>总是“failover”, 
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的 slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。 
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh


```



# Redis 缓存穿透与雪崩

- **面试高频，工作常用**

- 现在不会详细的分析 解决方案的底层的源码，后面再说。
- 解决的是服务的 **高可用**问题



Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。



## Redis 缓存穿透

### 概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中（比如：秒杀），于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

![image-20210405233948540](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405233948540.png)



### 解决方案

#### **布隆过滤器**

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则

丢弃，从而避免了对底层存储系统的查询压力；

![image-20210405234338865](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405234338865.png)



#### **缓存空对象**

当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数

据将会从缓存中获取，保护了后端数据源；

![image-20210405234435554](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405234435554.png)



但是这种方法会存在两个问题：

1、如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键；

2、即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。



## Redis 缓存击穿

### 概述

这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在**失效的瞬间**，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是**热点数据**，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。



**总结**：穿透和击穿的区别：

- 缓存穿透：查不到的太多了
- 缓存击穿：对同一个点，查的太多，在缓存过期的瞬间



### 解决方案

#### **设置热点数据永不过期**

从缓存层面来看，没有设置过期时间，所以不会出现热点 key 过期后产生的问题。

#### **加互斥锁**

分布式锁：使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

## Redis 雪崩

### 概念

缓存雪崩，是指在某一个时间段，缓存集中过期失效。（比如：Redis宕机（比如停电））

产生雪崩的原因之一，比如在写本文的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

![image-20210405235632077](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day26-Redis.assets/image-20210405235632077.png)



其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。



### 解决方案

#### **redis高可用**

这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。（比如：异地多活）

#### **限流降级**

- 在springCloud 中有学习

这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

#### **数据预热**

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。
