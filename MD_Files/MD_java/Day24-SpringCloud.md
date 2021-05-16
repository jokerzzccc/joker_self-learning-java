# SpringCloud

回顾：

- javaSE
- 数据库
- 前端
- Servlet
- Http
- Mybatis
- Spring
- SpringMVC
- SpringBoot
- Dubbo,zookeeper, 分布式基础
- Maven,Git
- Ajax,JSON
- 。。。



这个阶段如何学：

即springBoot末尾的展望



# 1、常见面试题：

1. 什么是微服务？
2. 微服务之间是如何独立通讯的
3. SpringCloud与Dubbo有哪些区别
4. SpringBoot , SpringCloud ，谈谈你对他们的理解
5. 什么是服务熔断？什么是服务降级
6. 微服务的优缺点分别是什么？说下你在项目开发中遇到的坑
7. 你所知道的微服务技术栈？请列举一二
8. eureka 和 zookeeper 都可以提供给服务注册于发现功能，说说两个的区别？

。。。



# 2、回顾微服务

## 2.1 微服务概述

**什么是微服务**？

微服务是近几年流行的一种架构思想。很难一言概括。

![image-20210323142053659](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142053659.png)

![image-20210323142122874](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142222752.png)



## 2.2 微服务与微服务架构

### 微服务

![image-20210323142222752](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142514499.png)

### 微服务架构

![image-20210323142305408](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142122874.png)



## 2.3 微服务优缺点

### 优点

![image-20210323142514499](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142305408.png)



### 缺点

![image-20210323142552756](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323142552756.png)



## 2.4 微服务技术栈有哪些

| 微服务条目                               | 落地技术                                                 |
| ---------------------------------------- | -------------------------------------------------------- |
| 服务开发                                 | SpringBoot,Spring,SpringMVC                              |
| 服务配置与管理                           | Netflix 公司的Archaius，阿里的Diamond等                  |
| 服务注册与发现                           | Eureka，Consul，Zookeeper...                             |
| 服务调用                                 | Rest, RPC, gRPC(谷歌的)                                  |
| 服务熔断器                               | Hystrix, Envoy...                                        |
| 负载均衡                                 | Ribbon, Nginx...                                         |
| 服务接口调用（客户端调用服务的简化工具） | Feign...                                                 |
| 消息队列                                 | Kafka, RabbitMQ, ActiveMQ...                             |
| 服务配置中心管理                         | SpringCloudConfig, Chef...                               |
| 服务路由（API网关）                      | Zuul..                                                   |
| 服务监控                                 | Zabbix, Nagios, Metrics,Specatator...                    |
| 全链路追踪                               | Zipkin,Barve,Dapper...                                   |
| 服务部署                                 | Docker,OpenStack,Kubernetes...                           |
| 数据流操作开发包                         | SpringCloud Stream(封装Redis,Rabbit,kafka等发送接收消息) |
| 事件消息总线                             | SpringCloud Bus                                          |
| ....                                     | ...                                                      |
|                                          |                                                          |



## 2.5 为什么选择SpringCloud作为微服务架构

### 1、选型依据

- 整体解决方案和框架成熟度
- 社区热度(看github的曲线，越陡，越热)
- 可维护性
- 学习曲线

### 2、当前各个IT公司用的微服务架构有哪些

- 阿里：dubbo+HFS
- 京东：JSF
- 新浪：Motan
- 当当网：DubboX
- ...



### 3、各微服务框架对比

| 功能点/服务框架 | NetFlix/SpringCloud                                          | Motan                                                        | gRPC                       | Thrift   | Dubbo/DubboX                            |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | -------- | --------------------------------------- |
| 功能定位        | 完整的微服务框架                                             | RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册与发现 | PRC框架                    | PRC框架  | 服务框架                                |
| 支持Rest        | 是，Ribbon 支持多种可插拔的序列化选择                        | 否                                                           | 否                         | 否       | 否                                      |
| 支持RPC         | 否（但可以和Dubbo兼容）                                      | 是（Hession2)                                                | 是                         | 是       | 是                                      |
| 支持多语言      | 是（Rest形式）？                                             | 否                                                           | 是                         | 是       | 否                                      |
| 负载均衡        | 是（服务端Zuul + 客户端Ribbon）,zuul-服务，动态路由，云端负载均衡 Eureka（针对中间层服务器） | 是（客户端）                                                 | 否                         | 否       | 是（客户端）                            |
| 配置服务        | NetFlix Archarius,SpringCloud Config Server 集中配置         | 是（客户端）                                                 | 否                         | 否       | 是（客户端）                            |
| 服务调用链监控  | 是（zuul),zuul 提供边缘服务，API网关                         | 否                                                           | 否                         | 否       | 否                                      |
| 高可用/容错     | 是（服务端Hystrix + 客户端Ribbon)                            | 是（客户端）                                                 | 否                         | 否       | 是（客户端）                            |
| 典型应用案例    | Netflix                                                      | Sina                                                         | Google                     | Facebook |                                         |
| 社区活跃度      | 高                                                           | 一般                                                         | 高                         | 一般     | 2017年后重新开始了维护，之间中间断了5年 |
| 学习难度        | 中端                                                         | 低                                                           | 高                         | 高       | 低                                      |
| 文档丰富程度    | 高                                                           | 一般                                                         | 一般                       | 一般     | 高                                      |
| 其它            | SpringCloud Bus 为我们的应用程序带来了更多管理端点           | 支持降级                                                     | Netflix 内部在开发集成gRPC | IDL定义  | 实践的公司比较多                        |



# 3、SpringCloud 入门概述

## 3.1、SpringCloud 是什么

SpringCloud官网：https://spring.io/cloud

![image-20210323150729558](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323150838533.png)

SpringCloud架构：

![image-20210323150838533](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323150729558.png)

![image-20210323150943080](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323150943080.png)

springCloud ，基于SpringBoot 提供 了一套微服务解决法案，包括服务注册于发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于Netflix 的开源组件做高度抽象封装之外，还有一些选型中立的组件。

SpringCloud 利用SpringBoot 的开发便利性，	巧妙的简化了分布式系统基础设施的开发，SpringCloud 为开发人员提供了快速构建分布式系统的一些工具，**包括配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等**，它们都可以用SpringBoot 的开发风格做到一键启动和部署。

SpringBoot并没有重复造轮子，它只是将目前各家公司开发的比较成熟，经得起实际考研的服务框架组合起来，通过SpringBoot 风格进行再封装，屏蔽掉了复杂的配置和实现原理，**最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包。**

SpringCloud 是 分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称 微服务全家桶。





## 3.2、SpringCloud 与 SpringBoot 的关系

- 两者是渐进式的关系，SpringBoot 构建微服务，SpringCloud 协调微服务
- SpringBoot 专注于 快速方便的开发单个个体微服务。说白了就是一个个的jar包
- SpringCloud 是关注于全局的微服务协调整理 治理框架，它将SpringBoot 开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等集成服务。
- SpringBoot可以离开SpringCloud 独立使用，开发项目，但是SpringCloud 离不开SpringBoot 属于依赖关系
- **Spring Boot 专注于快速、方便的开发单个个体微服务，SpringCloud关注全局的服务治理框架**



## 3.3、Dubbo 和 SpringCloud 技术选型

### 1、分布式+服务治理Dubbo

- 这是**传统**的方式：

目前成熟的互联网架构：应用服务拆分+消息中间件

传统网站的架构图：

![image-20210323161247130](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323161247130.png)



CDN：加速器

负载均衡：lvs,nginx



### 2、Dubbo + SpringCloud 对比

可以看一下社区活跃度：

以前的Dubbo:https://github.com/dubbo

现在重启后的apache-dubbo:https://github.com/apache/dubbo

https://github.com/spring-cloud

**结果：**

|              | Dubbo         | SpringCloud                 |
| ------------ | ------------- | --------------------------- |
| 服务注册中心 | zookeeper     | SpringCloud Netflix Eureka  |
| 服务调用方式 | RPC           | REST API                    |
| 服务监控     | Dubbo-monitor | SpringBoot Admin            |
| 断路器       | 不完善        | SpringCloud Netflix Hystrix |
| 服务网关     | 无            | SpringCloud Netflix Zuul    |
| 分布式配置   |               | SpringCloud Config          |
| 服务跟踪     |               | SpringCloud Sleuth          |
| 消息总线     |               | SpringCloud Bus             |
| 数据流       |               | SpringCloud Stream          |
| 批量任务     |               | SpringCloud Task            |



**最大区别：SpringCloud  抛弃了Dubbo 的RPC 通信，采用 基于HTTP的REST 方式**

![image-20210323162725072](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323162806782.png)

![image-20210323162806782](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323163917006.png)

**解决的问题域不一样：**

- Dubbo的定位是一款RPC框架，

- SpringCloud 目标是微服务架构下的一站式解决方案

工作相关：

- 开会就是：要有 **设计模式+微服务拆分思想：**



## 3.4、SpringCloud 能干嘛

![image-20210323163438088](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323163438088.png)

Spring Cloud focuses on providing good out of box experience for typical use cases and extensibility mechanism to cover others.

+ Distributed/versioned configuration
+ Service registration and discovery
+ Routing
+ Service-to-service calls
+ Load balancing
+ Circuit Breakers
+ Global locks
+ Leadership election and cluster state
+ Distributed messaging
+ 。。。



## 3.5、SpringCloud 在哪里下载

官网：https://spring.io/projects/spring-cloud

![image-20210323163917006](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323164128003.png)

SNAPSHOT：快照版本，不建议使用

GA：通用稳定版本

![image-20210323164128003](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323171508215.png)



### 参考书：（中文文档）

- SpringCloud 中文网：https://www.springcloud.cc/

- https://www.springcloud.cc/spring-cloud-netflix.html
- 中文API文档：https://www.springcloud.cc/spring-cloud-dalston.html
- SpringCloud 中国社区：http://springcloud.cn/



# 4、项目

## 4.1、总体介绍

- 使用一个Dept部门模块 做一个微服务通用案例 Consumer 消费者（Client）通过REST 调用Provider提供者（Server）提供的服务

- 回忆 SSM 等以往的知识

- Maven的分包分模块架构复习：

  ```yaml
  一个简单的Maven模块结构是这样的：
  -- app-parent: 一个父项目（app-parent）聚合很多子项目（app-util,app-dao,app-web...)
      -- pom.xml
      |
      |--app-core
      ||----pom.xml
      |
      |--app-web
      ||----pom.xml
      ...
          
  ```

  一个父工程带着多个Module 字模块

  MicroServieCloud 父工程（Project）下初次带着3个子模块（Module)

  - microservicecloud-api 【封装整体entity/接口/公共配置等】
  - microservicecloud-provider-dept-8081 【服务提供者】
  - microservicecloud-consumer-dept-80 【服务消费者】

## 4.2、SpringCloud 版本选择

- 目前是SpringCloud 2020.0.2 对应spring boot 2.4.x

- 使用就使用最新的稳定版本就好

  

![image-20210323171508215](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323171550150.png)

![image-20210323171550150](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323232144982.png)



# 5、Rest环境搭建

步骤总结：

1. 建立一个空的Maven 项目 作为父项目
2. 导入相关依赖
3. 建立数据库，并连接
4. 建立一个接口模块，只负责实体类，ORM
5. 建立一个服务提供者模块
6. 建立 服务消费者模块

## 父项目（空项目）

导入依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.joker</groupId>
    <artifactId>springCloud</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <junit.version>4.13</junit.version>
        <lombok.version>1.18.18</lombok.version>
        <log4j.version>1.2.17</log4j.version>
    </properties>
<!--    打包方式 pom-->
    <packaging>pom</packaging>
    
<dependencyManagement>
    <dependencies>
<!--        springCloud 的依赖-->
        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-dependencies -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2020.0.2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
<!--        springBoot 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.4.3</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
<!--        连接数据库-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.5</version>
        </dependency>
<!--        springBoot 启动器-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
<!--        日志测试-->
<!--        junit 为了单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
<!--        lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
<!--        log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.2.3</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 接口

模块 ：springcloud-api

- 只负责实体类ORM

1. 导入依赖

   ```xml
   <!--    当前的Module自己需要的依赖，如果父依赖中已经配置了版本，这里就不用写版本了-->
       <dependencies>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
       </dependencies>
   ```

2. 编写实体类 src/main/java/com/joker/springcloud/pojo/Dept.java

   ```java
   package com.joker.springcloud.pojo;
   
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import lombok.experimental.Accessors;
   
   import java.io.Serializable;
   
   /**
    * 分布式的实体类，都必须实现序列化
    * ORM
    */
   @Data
   @NoArgsConstructor
   @Accessors(chain = true)//链式写法
   public class Dept implements Serializable {
       private Long deptno;//主键
       private String dname;
       //db_source 字段：是这个数据存在哪个数据库 的字段
       //因为微服务，一个服务对应一个数据库，同一个信息可能存在不同的数据库
       private String db_source;
       //有参构造，只有dname 是因为，deptno主键自增，db_source 自动生成
       public Dept(String dname) {
           this.dname = dname;
       }
   
       /*
       链式写法
       Dept dept = new Dept();
       dept.setDeptno().setDname()....;
        */
   }
   ```

## 服务提供者

模块：springcloud-provider-dept-8001

**步骤总结：**

1. 导入依赖
2. 编写主配置文件application.yml和mybatis 配置文件 mybatis-config.xml
3. 编写mapper 文件DeptMapper
4. 编写mapper配置文件 DeptMapper.xml
5. 编写Service层，DeptService 和DeptServiceImpl
6. 编写controller 层 DeptController
7. 编写主启动类 DeptProvider_8001

**步骤实现：**

1. 导入依赖

   ```xml
      <dependencies>
   <!--        第一步，拿到实体类，配置api Module-->
           <dependency>
               <groupId>org.joker</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   <!--        junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
           </dependency>
           <dependency>
               <groupId>ch.qos.logback</groupId>
               <artifactId>logback-core</artifactId>
           </dependency>
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
           </dependency>
   <!--        test-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-test</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   <!--        服务器，除了tomcat 还有jetty 和tomcat没啥区别-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jetty</artifactId>
           </dependency>
   <!--        热部署工具-->
   <!--        自动更新写完的代码，测试的时候不用每次都去重启服务器-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
   
   ```

2. 编写主配置文件application.yml和mybatis 配置文件 mybatis-config.xml

   - src/main/resources/application.yml

     ```yaml
     server:
       port: 8001
     #mybatis配置
     mybatis:
       type-aliases-package: com.joker.springcloud.pojo
       config-location: classpath:mybatis/mybatis-config.xml
       mapper-locations: classpath:mybatis/mapper/*.xml
     #spring的配置
     spring:
       application:
         name: springcloud-provider-dept
       datasource:
         type: com.alibaba.druid.pool.DruidDataSource
         driver-class-name: org.gjt.mm.mysql.Driver
         url: jdbc:mysql://localhost:3306/db01?userUnicode=true&characterEncoding=utf-8&useSSL=true
         username: root
         password: 123456
     ```

   - src/main/resources/mybatis/mybatis-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
         <settings>
     <!--        开启二级缓存-->
             <setting name="cacheEnabled" value="true"/>
         </settings>
     </configuration>
     ```

3. 编写mapper 文件DeptMapper

   - src/main/java/com/joker/springcloud/mapper/DeptMapper.java

   ```java
   package com.joker.springcloud.mapper;
   
   import com.joker.springcloud.pojo.Dept;
   import org.apache.ibatis.annotations.Mapper;
   import org.springframework.stereotype.Repository;
   
   import java.util.List;
   
   @Mapper
   @Repository
   public interface DeptMapper {
       //默认修饰符就是public 所以也可以不用写
       public boolean addDept(Dept dept);
       Dept queryById(Long id);
       List<Dept> queryAll();
   }
   ```

   

4. 编写mapper配置文件 DeptMapper.xml

   - src/main/resources/mybatis/mapper/DeptMapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.joker.springcloud.mapper.DeptMapper">
       <insert id="addDept" parameterType="Dept">
           insert into dept (dname,db_source)
           values (#{dname},DATABASE())
       </insert>
       <select id="queryById" parameterType="Long" resultType="Dept">
           select *
           from dept
           where deptno=#{deptno};
       </select>
       <select id="queryAll" resultType="Dept">
           select * from dept;
       </select>
   </mapper>
   ```

   

5. 编写Service层，DeptService 和DeptServiceImpl

   ```java
   package com.joker.springcloud.service;
   
   import com.joker.springcloud.pojo.Dept;
   
   import java.util.List;
   
   public interface DeptService {
           boolean addDept(Dept dept);
           Dept queryById(Long id);
           List<Dept> queryAll();
   
   }
   ```

   ```java
   package com.joker.springcloud.service;
   
   import com.joker.springcloud.mapper.DeptMapper;
   import com.joker.springcloud.pojo.Dept;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   
   import java.util.List;
   @Service
   public class DeptServiceImpl implements DeptService{
       @Autowired
       DeptMapper deptMapper;
       @Override
       public boolean addDept(Dept dept) {
           return deptMapper.addDept(dept);
       }
   
       @Override
       public Dept queryById(Long id) {
           return deptMapper.queryById(id);
       }
   
       @Override
       public List<Dept> queryAll() {
           return deptMapper.queryAll();
       }
   }
   
   ```

   

6. 编写controller 层 DeptController

   ```java
   package com.joker.springcloud.controller;
   
   import com.joker.springcloud.pojo.Dept;
   import com.joker.springcloud.service.DeptService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.util.List;
   
   //提供Restful服务
   @RestController
   public class DeptController {
       @Autowired
       private DeptService deptService;
       @PostMapping("/dept/add")
       public boolean addDept(Dept dept){
           return deptService.addDept(dept);
       }
       @GetMapping("/dept/get/{id}")
       public Dept queryById(@PathVariable("id") Long id) {
           return deptService.queryById(id);
       }
   
       @GetMapping("/dept/list")
       public List<Dept> queryAll() {
           return deptService.queryAll();
       }
   }
   ```

   

7. 编写主启动类 DeptProvider_8001

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   
   //启动类
   @SpringBootApplication
   @EnableEurekaClient//在服务启动后，自动注册到eureka 中
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
   }
   
   ```

   

## 服务消费者

模块：springcloud-consumer-dept-80

**步骤总结：**

1. 导入依赖
2. 编写配置文件
3. 编写配置类
4. 编写controller层
5. 编写主启动文件

**步骤实现：**

1. 导入依赖

   ```xml
   <!--    消费者，只需要实体类+web,不需要连接数据库，和前端打交道-->
       <dependencies>
           <dependency>
               <groupId>org.joker</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
       </dependencies>
   ```

   

2. 编写配置文件 src/main/resources/application.yml

   ```yaml
   server:
     port: 80 #80端口，表示本地，测试的时候可以不写这个端口号，
   ```

   

3. 编写配置类 src/main/java/com/joker/springcloud/config/ConfigBean.java

   ```java
   package com.joker.springcloud.config;
   
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.client.RestTemplate;
   
   @Configuration //相当于 spring 的 applicationContext.xml 配置
   public class ConfigBean {
       @Bean
       public RestTemplate getRestTemplate(){
           return new RestTemplate();
       }
   }
   ```

   

4. 编写controller层 src/main/java/com/joker/springcloud/contoller/DeptConsumerController.java

   ```java
   package com.joker.springcloud.contoller;
   
   import com.joker.springcloud.pojo.Dept;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.*;
   import org.springframework.web.client.RestTemplate;
   
   import java.util.List;
   
   @RestController
   public class DeptConsumerController {
       //理解：消费者，不应该有service层，所以，要用Restful风格去请求
       //RestTemplate : springboot的自带的模板，供我们直接调用，注册到spring中，就要去建一个config类
       //三个参数:url，实体：(如果多个可以用放在map里），Class<T> responseType
       //说到底，RestTemplate 就是提供多种便捷访问远程http服务的方法，简单的Restful风格的服务模板
       @Autowired
       private RestTemplate restTemplate;
   
       private static final String REST_URL_PREFIX="http://localhost:8001";
   
       @RequestMapping("/consumer/dept/add")
       public boolean addDept(Dept dept){
           return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class);
       }
   
       //要去请求：http://localhost:8001/dept/list
       @RequestMapping("/consumer/dept/get/{id}")
       public Dept get(@PathVariable("id") Long id){
           //因为服务端用的get，这里也只能用get方式
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
       }
   
       @RequestMapping("consumer/dept/list")
       public List<Dept> queryAll(){
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
       }
   
   }
   
   ```

   

5. 编写主启动文件 src/main/java/com/joker/springcloud/DeptConsumer_80.java

   ```java
   package com.joker.springcloud.contoller;
   
   import com.joker.springcloud.pojo.Dept;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.*;
   import org.springframework.web.client.RestTemplate;
   
   import java.util.List;
   
   @RestController
   public class DeptConsumerController {
       //理解：消费者，不应该有service层，所以，要用Restful风格去请求
       //RestTemplate : springboot的自带的模板，供我们直接调用，注册到spring中，就要去建一个config类
       //三个参数:url，实体：(如果多个可以用放在map里），Class<T> responseType
       //说到底，RestTemplate 就是提供多种便捷访问远程http服务的方法，简单的Restful风格的服务模板
       @Autowired
       private RestTemplate restTemplate;
   
       private static final String REST_URL_PREFIX="http://localhost:8001";
   
       @RequestMapping("/consumer/dept/add")
       public boolean addDept(Dept dept){
           return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class);
       }
   
       //要去请求：http://localhost:8001/dept/list
       @RequestMapping("/consumer/dept/get/{id}")
       public Dept get(@PathVariable("id") Long id){
           //因为服务端用的get，这里也只能用get方式
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
       }
   
       @RequestMapping("consumer/dept/list")
       public List<Dept> queryAll(){
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
       }
   
   }
   
   ```

   

# 6、Eureka 服务注册与发现

## 6.1 什么是Eureka

- Netflix 在设计 Eureka 时，遵循的就是AP原则
- Eureka 是 Netflix 的一个子模块，也是核心模块之一。Eureka 是一个基于REST的服务，用于定位服务，以实现云端中间层服务发现和故障转移，服务注册于发现对于微服务来说是非常重要的，有了服务注册于发现，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了。功能类似于Dubbo的注册中心，比如Zookeeper.



## 6.2 原理讲解

- Eureka 的基本架构
  - SpringCloud 封装了Netflix 公司开发的Eureka模块来实现服务注册与发现（对比zookeeper）
  - Eureka 采用了 C-S 的架构设计，EurekaServer 作为服务注册功能的服务器，他是服务注册中心。
  - 而系统中的其他微服务。使用Eureka 的客户端连接到 EurekaServer 并维持心跳连接。这样系统的维护人员就可以通过EurekaServer 来监控系统中各个微服务是否正常运行，SpringCloud 的一些其它模块（比如 Zuul ）就可以通过EurekaServer 来发现系统中的其它微服务，并执行相关逻辑；
  - Eureka 包含两个组件： Eureka Server 和 Eureka Client
  - Eureka Server 提供服务注册，各个节点启动后，会在EurekaServer 中进行注册；这样Eureka Server 中的注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中值观的看到。
  - Eureka Client 是一个Java 客户端，用于简化EurekaServer 的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会E向EurekaServer发送心跳（默认周期为30s) 。如果EurekaServer 在多个心跳周期内没有接受到某个节点的心跳，EurekaServer 将会从服务注册表中把这个服务节点移除掉（默认周期为90秒）
- 三大角色：
  - Eureka Server ： 提供服务注册于发现。
  - Service Provider : 将自身的服务注册到Eureka  中，从而使消费方能够找到。
  - Service Consumer : 服务消费方从Eureka 中获取注册服务列表，从而找到并消费服务



## 6.3 EurekaServer注册中心搭建

模块：springcloud-eureka-7001

**步骤总结**：

1. 建立一个新模块 springcloud-eureka-7001
2. 根据SpringCloud 官网，导入对应版本的依赖
3. 编写配置类 src/main/resources/application.yml
4. 编写主启动类 src/main/java/com/joker/springcloud/EurekaServer_7001.java
5. 启动 ，测试访问： http://localhost:7001/

**步骤实现：**

1. 建立一个新模块 springcloud-eureka-7001

2. 根据SpringCloud 官网，导入对应版本的依赖

   ```xml
   <dependencies>
       <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-server -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
           <version>3.0.2</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
       </dependency>
   </dependencies>
   ```

3. 编写配置类 src/main/resources/application.yml

   ```yaml
   server:
     port: 7001
   #Eureka 配置
   eureka:
     instance:
       hostname: localhost #Eureka 服务端的实例名称
     client:
       register-with-eureka: false #表示eureka是否向eureka注册中心 注册自己
       fetch-registry: false # fetch-registry 如果为false 表示自己就是注册中心
       service-url: #就是一个服务监控页面，可以看源码得知它有一个默认的地址，要改为自己的
         defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
   ```

4. 编写主启动类 src/main/java/com/joker/springcloud/EurekaServer_7001.java

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
   
   @SpringBootApplication
   @EnableEurekaServer// @EnableEurekaServer 表示：它是服务端的启动类，可以接受别人注册进来
   public class EurekaServer_7001 {
       public static void main(String[] args) {
           SpringApplication.run(EurekaServer_7001.class,args);
       }
   }
   ```

5. 启动 ，测试访问： http://localhost:7001/

![image-20210324010150545](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323225744118.png)

DS Replicas: EurekaServer的集群

## 6.4 把服务提供者注册到Eureka中

服务提供者：springcloud-provider-dept-8001

**步骤总结：**

1. 导入依赖
2. 添加配置
3. 主启动类添加@EnableEurekaClient
4. 启动测试 http://localhost:7001/

**步骤实现：**

1. 导入依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-client -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       <version>3.0.2</version>
   </dependency>
   ```

2. 添加配置

   ```yaml
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
   ```

3. 主启动类添加@EnableEurekaClient

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   
   //启动类
   @SpringBootApplication
   @EnableEurekaClient//在服务启动后，自动注册到eureka 中
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
   }
   ```

   

4. 启动测试 http://localhost:7001/

5. 结果。可以看见，服务已经注册进来了

   ![image-20210323225744118](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324010150545.png)

   这个status状态名 可以在配置文件里改掉

   ```yaml
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/
     instance:
       instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息 status
   ```

   修改完如下

   ![image-20210323230528137](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323230528137.png)



### **完善监控信息**：actuator

1. 导入依赖

   ```xml
   <!--        actuator 完善监控信息-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   ```

2. 添加配置

   ```yaml
   # info 配置
   info:
     app.name: joker-springcloud
     company.name: blog.joker.com
   ```

3. 这个时候点击 status下的那个 连接路径，跳转到http://desktop-d09l4ab:8001/actuator/info，就不会报错了

   ![image-20210323232144982](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323232247633.png)

### 自我保护机制

注意： 如果出现 ![image-20210323232247633](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324004150641.png)

就是出发了Eureka 的自我保护机制

![image-20210323232805293](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324004222314.png)

### 服务发现 Discovery

- 用于团队协作开发中，如果要知道其它服务是干嘛的，就可以用这个，可有可无，不是必须的
- 就是通过微服务的id 即 Application 下面那个名字,来 得到它是谁，它在哪

步骤总结：

1. 在DeptController注入 DiscoveryClient
2. 在DeptController中 写一个接口
3. 主启动类 添加@EnableDiscoveryClient//服务发现
4. 启动查看

步骤实现：

1. 在DeptController注入 DiscoveryClient

   ```java
   //得到具体的微服务信息，可以通过源码看见，接口DiscoveryClient  实现类有一个 EurekaDiscoveryClient
   @Autowired
   private DiscoveryClient discoveryClient;
   ```

2. 写一个接口

   ```java
       //注册进来的服务，通过服务发现来获取一些相关消息
       @RequestMapping("/dept/discovery")
       public Object discovery(){
           //获取微服务列表清单
           List<String> services = discoveryClient.getServices();
           System.out.println("discovery--->services"+services);
           //得到一个具体的微服务信息,通过具体的微服务id,也就是 ApplicationName
           List<ServiceInstance> instances = discoveryClient.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
           for (ServiceInstance instance : instances) {
               System.out.println(
                       instance.getHost()+"\t"+
                       instance.getPort()+"\t"+
                       instance.getUri()+"\t"+
                       instance.getServiceId());
           }
           return this.discoveryClient;
       }
   }
   ```

   

3. 主启动类 添加@EnableDiscoveryClient//服务发现

   ```java
   //启动类
   @SpringBootApplication
   @EnableEurekaClient//在服务启动后，自动注册到eureka 中
   @EnableDiscoveryClient//服务发现
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
   }
   ```

4. 启动查看 http://desktop-d09l4ab:8001/dept/discovery

   服务端：

   ![image-20210324004150641](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324005439695.png)

   浏览器窗口：

![image-20210324004222314](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324111800329.png)



# 7、Eureka 集群环境配置

- 建立集群，是因为，如果一个服务注册中心崩了，还有其它的可以用

建立Eureka 注册中心集群,使相互关联

![image-20210324005439695](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324013430859.png)



步骤总结：

1. 新增两个模块

   - 新增两个模块，和springcloud-eureka-7001 一毛一样，改端口号,还有主启动文件的run
     - springcloud-eureka-7002
     - springcloud-eureka-7003

2. 在C:\Windows\System32\drivers\etc\hosts文件里，添加三个域名，配置，其实都是映射都一个本地172.0.0.1，这里是为了好理解

   ```java
   127.0.0.1      eureka7001.com
   127.0.0.1      eureka7002.com
   127.0.0.1      eureka7003.com
       
   ```

   

3. 修改三个注册中心模块的 hostname 

   比如eureka7001的

   ```
   eureka:
     instance:
       hostname: eureka7001.com
   ```

4. 修改三个服务注册中心的defaultZone ，各自对应的其它集群服务注册中心

   比如比如eureka7001的

   ```yaml
   defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ```

5. 修改 修改服务提供者的spring.datasource.url 和 defaultZone ，把三个集群地址都添加进去

   ```yaml
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ```

6. 测试访问http://eureka7001.com:7001/

   ![image-20210324111800329](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324012732512.png)

# 8、Eureka对比Zookeeper

## CAP 原则

RDBMS（Mysql, Oracle, sqlServer）===>ACID

NoSQL (Redis, MongDB)====>CAP

**ACID 是什么？**

- A (Atomicity) 原子性
- C（Consistency) 一致性
- I（Isolation）隔离性
- D（Durability）持久性

**CAP 是什么？**

- C（Consistency) 强一致性
- A（Availability) 可用性
- P（Partition tolerance) 分区容错性

CAP 的三进二： CA、AP、CP

==CAP理论的核心==

- 一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
- 根据CAP 原理，将NoSQL 数据库分成了满足CA 原则，满足CP 原则，满足AP 原则三大类
  - CA： 单点集群，满足一致性、可用性的系统，通常可扩展性较差
  - CP：满足一致性、分区容错性的系统，通常性能不是特别高
  - AP：满足可用性、分区容错性的系统，通常可能对一致性要求低一些

## 对比：

著名的CAP理论提出：一个分布式系统不可能同时C,A,P

由于分区容错性P 在分布式系统中是必须要保证的，因此只能在A 和C 里面权衡：

- Zookeeper 保证的是 CP
- Eureka 保证 AP

![image-20210324013430859](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324102819108.png)

![image-20210324012732512](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324114635264.png)



==因此，Eureka 可以很好的应对网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪==



# 9、Ribbon 负载均衡

## ribbon 是什么？

- SpringCloud Ribbon 是基于Netflix Ribbon 实现的一套==**客户端负载均衡的工具**==
- Ribbon 是Netflix 发布的开源项目，主要功能是提供客户但的软件负载均衡算法，将Netflix 的中间层服务连接在一起。Ribbon 的客户端组件提供一系列完整的配置须知：连接超市、重试等。简单的说，就是在配置文件中列出LoadBalancer（简称LB：负载均衡）后面所有的机器，Ribbon 会自动的帮助你基于某种规则（比如简单轮询（默认），随机连接等）去连接这些机器。我们也很容易使用Ribbon 实现自定义的负载均衡算法。

## ribbon 能干嘛？

- LB ，即负载均衡，在微服务或分布式集群中常用的一种应用
- **负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）**。
- 常见的负载均衡软件有Nginx, Lvs 等。tomcat+
- dubbo,SpringCloud 中均给我们提供了负载均衡，SpringCloud 的负载均衡算法可以自定义。
- 负载均衡简单分类：
  - 集中式LB
    - 即在服务的消费方和提供方之间是同独立的LB设施，如 **Nginx** ：反向代理服务器，由该设施负责把访问请求 通过某种策略转发至服务的提供方
  - 进程式LB
    - 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
    - **Ribbon 就属于进程内LB**，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。

## ribbon实现

在消费者模块中：springcloud-consumer-dept-80

步骤总结：

1. 导入依赖
2. application.yml 中添加eureka配置
3. ConfigBean 里添加@LoadBalanced 注解
4. 主启动类 添加 @EnableEurekaClient
5. 修改DeptConsumerController.java 的访问地址
6. 启动集群注册中心，启动服务提供者8001，再启动服务消费者 80 ，
7. 访问测试

- Ribbon 源码：接口Irule,有很多实现类，定义了多种负载均衡的算法规则

- Ribbon 和Eureka 整合以后，客户端可以直接调用服务，不用关心IP 地址和端口号



## ribbon 实现负载均衡

![image-20210324102819108](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210324130453463.png)

步骤总结

1. 将db01数据库导出为sql,新建两个数据库 db02,db03

2. 新建两个服务器提供者模块，复制8001就行，这三个的区别，就是自己连接的自己的数据库

   1. 注意修改配置文件端口号，数据库连接，eureka实例id
   2. 修改主启动类

3. 启动测试：

   - 先启动一个注册中心7001
   - 再启动三个服务 8001，8002，8003，
   - 访问http://eureka7001.com:7001/

4. 结果，可以看到，同一个微服务下有三个实例

   ![image-20210324114635264](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325010429893.png)

5. 再启动 消费 80

6. 访问 ，http://localhost/consumer/dept/list 一致刷新，就可以看见来自不同的数据库，默认方法是轮询



注意：

- 三个服务名称一致是前提 springcloud-provider-dept

步骤实现：

1. 将db01数据库导出为sql,新建两个数据库 db02,db03

   ```sql
   CREATE DATABASE /*!32312 IF NOT EXISTS*/`db03` /*!40100 DEFAULT CHARACTER SET utf8 */;
   
   USE `db03`;
   
   /*Table structure for table `dept` */
   
   DROP TABLE IF EXISTS `dept`;
   
   CREATE TABLE `dept` (
     `deptno` BIGINT(20) NOT NULL AUTO_INCREMENT,
     `dname` VARCHAR(60) DEFAULT NULL,
     `db_source` VARCHAR(60) DEFAULT NULL,
     PRIMARY KEY (`deptno`)
   ) ENGINE=INNODB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COMMENT='部门表';
   
   /*Data for the table `dept` */
   
   INSERT  INTO `dept`(`deptno`,`dname`,`db_source`) VALUES 
   (1,'开发部',DATABASE()),
   (2,'人事部',DATABASE()),
   (3,'财务部',DATABASE()),
   (4,'市场部',DATABASE()),
   (5,'运维部',DATABASE());
   ```

2. 



## ribbon 自定义负载均衡算法

- ribbon 核心接口：Irule,他有很多的实现类，规定负载均衡算法

  - ```
    AvailabilityFilteringRule //会先过滤掉 跳闸，访问故障的服务~，对剩下的进行轮询
    RoundRobinRule //轮询（默认的）
    RandomRule //随机
    RetryRule //重试。会先按照轮询获取服务，如果服务获取失败，则会在指定的时间内进行重试。
    ```

- 查看官网 https://www.springcloud.cc/spring-cloud-dalston.html#_customizing_the_ribbon_client
- ![image-20210324130453463](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325011058802.png)

自定义负载均衡算法：

- 不要和主启动类同级，因为会被扫描到

- 新建一个路径com/joker/myrule/JokerRule.java，创建一个自定义算法文件 JokerRule

- 在主启动类添加注解

  ```java
  //在微服务启动的时候，就能去加载我们自定义的Ribbon 类
  @RibbonClient(name = "SPRINGCLOUD-PROVIDER-DEPT",configuration = JokerRule.class)
  ```

- 就是模范一个已有的算法，进行改造就好了，比如改造RandomRule 为 JokerRandomRule，总共也就两部分

  - 第一步，获得服务，死代码，不变
  - 第二步，自己定义负载均衡规则

  ```java
  
  package com.joker.myrule;
  
  import com.netflix.client.config.IClientConfig;
  import com.netflix.loadbalancer.AbstractLoadBalancerRule;
  import com.netflix.loadbalancer.ILoadBalancer;
  import com.netflix.loadbalancer.Server;
  
  import java.util.List;
  import java.util.concurrent.ThreadLocalRandom;
  
  
  public class JokerRandomRule extends AbstractLoadBalancerRule {
  // 自定义规则：每个服务访问5次，换下一个服务（这里只有3个）
  //自定义两个变量
      //total = 0 ,默认=0，  如果=5 ，指向下一个服务节点
      //index = 0,默认 = 0，如果total = 5 ,index + 1
      private static int total=0;//当前服务被调用的次数
      private static int currentIndex=0;//当前是谁在提供服务
  
      //@edu.umd.cs.findbugs.annotations.SuppressWarnings(value = "RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE")
      public Server choose(ILoadBalancer lb, Object key) {
          if (lb == null) {
              return null;
          }
          Server server = null;
  
          while (server == null) {
              if (Thread.interrupted()) {
                  return null;
              }
              List<Server> upList = lb.getReachableServers();//获得活着的服务
              List<Server> allList = lb.getAllServers();//获得全部的服务
  
              int serverCount = allList.size();
              if (serverCount == 0) {
                  return null;
              }
  //原本的算法
  //            int index = chooseRandomInt(serverCount);//生成区间随机数
  //            server = upList.get(index);//从活着的服务中 随机获取一个
              //自定义部分
              //====================
              if(total<5){
                  server = upList.get(currentIndex);
                  total++;
              } else {
                  total=0;
                  currentIndex++;
                  if (currentIndex > upList.size()){
                      currentIndex=0;
                  }
                  server = upList.get(currentIndex);//从活着的服务，获取指定的服务来进行操作
              }
  
              //=======================
  
              if (server == null) {
                  Thread.yield();
                  continue;
              }
  
              if (server.isAlive()) {
                  return (server);
              }
              server = null;
              Thread.yield();
          }
  
          return server;
  
      }
  
      protected int chooseRandomInt(int serverCount) {
          return ThreadLocalRandom.current().nextInt(serverCount);
      }
  
  	@Override
  	public Server choose(Object key) {
  		return choose(getLoadBalancer(), key);
  	}
  
  	@Override
  	public void initWithNiwsConfig(IClientConfig clientConfig) {
  		// TODO Auto-generated method stub
  		
  	}
  }
  
  ```

- 修改MyRule 里的bean,

  ```java
  package com.joker.myrule;
  
  import com.netflix.loadbalancer.IRule;
  import org.springframework.context.annotation.Bean;
  
  public class JokerRule {
      //体验随机算法
      @Bean
      public IRule myRule(){
          return new JokerRandomRule();
      }
  }
  
  ```

- 测试访问，http://localhost/consumer/dept/list ， 一直刷新，就可以看见符合自己的算法规则



# 10、Feign 负载均衡

## 10.1 简介

Feign 是声明式的web service 客户端，它让微服务之间的调用更简单 了，类似controller 调用service 。SpringCloud集成了Ribbon 和Eureka，可在使用Feign 时提供负载均衡的客户端。

- 只需要创建一个接口，然后添加注解即可。

Feign ，主要是社区，大家都习惯面向接口编程。这个是很多开发人员的规范。

**调用微服务访问**两种方法：

1. 微服务接口【Ribbon】
2. 接口和注解 【Feign】



**Feign 能干什么？**

- Feign 旨在使编写Java Http 客户端变得更容易
- 前面在使用Ribbon + RestTemplate 时，利用RestTemplate 对Http 请求的封装处理，形成了一套模板化的调用方法。
- 但是在实际开发中由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。
- 所以，Feign 在此基础上做了进一步封装，由它来帮助我们定义和实现依赖服务接口的定义，==**在Feign 的实现下，我们只需要创建一个接口并使用注解的方式来配置它（类似于以前Dao接口上标注Mapper 注解，现在时一个微服务接口上面标注一个Feign 注解即可）**==。即可完成对服务提供方的接口绑定，简化了使用SpringCloud Ribbon时，自动封装服务调用客户端的开发量。



**Feign 集成了Ribbon**

- 利用Ribbon 维护 了 MicroServiceCloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而与Ribbon 不同的是，通过Feign 只需要定义服务绑定接口且以声明的方法，优雅而简单的实现 了服务调用。



## 10.2 Feign 使用

1. 新建一个模块springcloud-consumer-dept-feign，复制消费80的文件
2. 在springcloud-api模块在新建一个文件夹service,
3. springcloud-api 与 feign模块导入feign依赖
4. service 里新建一个接口 DeptClientService 添加注解：@FeignClient
5. 重写消费feign模块的controller 微服务调用方式
6. feign模块主启动类，添加 @EnableFeignClients
7. 启动7001,8001,8002,feign,测试，访问 http://localhost/consumer/dept/list,发现一样可以实现

步骤实现：

1. 新建一个模块，复制消费80的文件

2. 在springcloud-api模块在新建一个文件夹service,

3. springcloud-api 与 feign模块导入feign依赖

   ```xml
   <!--        feign-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
               <version>3.0.2</version>
           </dependency>
   ```

4. service 里新建一个接口 DeptClientService 添加注解：@FeignClient

   ```java
   @Component
   @FeignClient(value = "http://SPRINGCLOUD-PROVIDER-DEPT")
   public interface DeptClientService {
       @GetMapping("/dept/get/{id}")
      public Dept queryById(@PathVariable("id") Long id);
       @GetMapping("/dept/list")
      public List<Dept> queryAll();
       @PostMapping("/dept/add")
      public boolean addDept(Dept dept);
   }
   ```

   

5. 重写消费feign模块的controller 微服务调用方式

   ```java
   @RestController
   public class DeptConsumerController {
   
       @Qualifier("com.joker.springcloud.service.DeptClientService")
       @Autowired
       private DeptClientService service=null;
   
       @RequestMapping("/consumer/dept/add")
       public boolean addDept(Dept dept){
           return this.service.addDept(dept);
       }
   
       //要去请求：http://localhost:8001/dept/list
       @RequestMapping("/consumer/dept/get/{id}")
       public Dept get(@PathVariable("id") Long id){
           return this.service.queryById(id);
       }
   
       @RequestMapping("/consumer/dept/list")
       public List<Dept> queryAll(){
           return this.service.queryAll();
       }
   
   }
   ```

   

6. feign模块主启动类，添加 @EnableFeignClients

   ```java
   //Ribbon 和Eureka 整合以后，客户端可以直接调用服务，不用关心IP 地址和端口号
   //启动类
   @SpringBootApplication
   @EnableEurekaClient
   @EnableFeignClients(basePackages = {"com.joker.springcloud"})
   public class FeignDeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(FeignDeptConsumer_80.class,args);
       }
   }
   ```



# 11、Hystrix

## 11.1 简介

官方文档：https://github.com/Netflix/hystrix/wiki

- 官网里图片，需要*翻**强*才能看到

我参考的Hystrix 中文博客：https://www.cnblogs.com/flashsun/p/12579367.html

![image-20210325010429893](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325011212607.png)

![image-20210325011058802](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325011440366.png)

**分布式依赖系统面临的问题**

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免的失败。



### 服务雪崩

![image-20210325011212607](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325011809247.png)



### 什么是Hystrix

![image-20210325011440366](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325041630663.png)

### Hystrix 能干嘛

- 服务降级
- 服务熔断
- 服务限流
- 接近实时的监控
- 。。。

## 11.2 服务熔断

### **是什么：**

- 熔断机制是对应雪崩效应的一种微服务链路保护机制。

![image-20210325011809247](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325044730362.png)



熔断机制的注解：@HystrixCommand

### 实现

三步：

- 导入依赖
- 编写熔断机制，加注解@HystrixCommand
- 主启动类添加 @EnableHystrix

1. 新建一个模块 ：springcloud-provider-dept-hystrix-8001，复制之前8001的代码过来

2. 导入依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       <version>2.2.7.RELEASE</version>
   </dependency>
   ```

3. 修改  配置文件的eureka实例名称

   ```yaml
   eureka:
     instance:
       instance-id: springcloud-provider-dept-hystirx-8001 
   ```

4. 重写Controller,指定熔断方案

   ```java
   package com.joker.springcloud.controller;
   
   import com.joker.springcloud.pojo.Dept;
   import com.joker.springcloud.service.DeptService;
   import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.client.ServiceInstance;
   import org.springframework.cloud.client.discovery.DiscoveryClient;
   import org.springframework.web.bind.annotation.*;
   
   import java.util.List;
   
   //提供Restful服务
   @RestController
   public class DeptController {
       @Autowired
       private DeptService deptService;
   
       @GetMapping("/dept/get/{id}")
       @HystrixCommand(fallbackMethod = "hystrixGet")
       public Dept get(@PathVariable("id") Long id){
           Dept dept = deptService.queryById(id);
           if (dept == null){
               throw new RuntimeException("id->"+id+",不存在该用户，或者信息无法找到");
           }
           return dept;
       }
       //备选方法 熔断版
       public Dept hystrixGet(@PathVariable("id") Long id){
   
           return new Dept()
                   .setDeptno(id)
                   .setDname("id->"+id+"没有对应的信息，null--@Hystrix")
                   .setDb_source("no this database in Mysql");
       }
   }
   ```

5. 主启动类添加  @EnableHystrix

6. 启动 7001，7002，hystrix8001，dept-80,测试。http://localhost/consumer/dept/get/1



## 11.3 服务降级

- 和客户端相关

步骤：

1. 在模块 springcloud-api 中，service 里添加实现类，DeptClientServiceFallbackFactory

   ```java
   package com.joker.springcloud.service;
   
   import com.joker.springcloud.pojo.Dept;
   import org.springframework.cloud.openfeign.FallbackFactory;
   import org.springframework.stereotype.Component;
   
   import java.util.List;
   
   //服务降级
   @Component
   public class DeptClientServiceFallbackFactory implements FallbackFactory {
       @Override
       public DeptClientService create(Throwable cause) {
           return new DeptClientService() {
               @Override
               public Dept queryById(Long id) {
                   return new Dept()
                           .setDeptno(id)
                           .setDname("id->" + id + "没有对应的信息,客户端提供了降级的信息，这个服务现在已经被关闭")
                           .setDb_source("no database ");
               }
   
               @Override
               public List<Dept> queryAll() {
                   return null;
               }
   
               @Override
               public boolean addDept(Dept dept) {
                   return false;
               }
           };
       }
   }
   ```

2. 在 @FeignClient 中添加 fallbackFactory

   ```
   @FeignClient(value = "http://SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptClientServiceFallbackFactory.class)
   public interface DeptClientService {
   ```

3. 需要去消费 feign 中配置降级

   ```yaml
   # 开启降级，
   feign:
     hystrix:
       enabled: ture
   ```

4. 启动测试，此时，如果正常访问，是可以的，如果把一个服务器关掉，再访问，就会提示信息



## 服务熔断与服务降级对比

**服务熔断：**

- 服务端
- 某个服务超市或者异常，就会引起熔断，类似保险丝

**服务降级：**

- 客户端
- 从整体网站请求负载考虑。当某个服务熔断或者关闭后，服务将不再被调用，
- 此时，在客户端，可以准备一个 FallbackFactory ,返回一个缺省值（默认的值）
- 好处：确实会让整体负载变低
- 坏处：整体的服务水平下降了，但是好歹还能用（返回默认值），比直接挂掉好

## 11.4 Hystrix Dashboard 流监控

步骤总结：

1. 一个监控页面 dashboard模块
2. 往监控里面放服务，这里放的是hystrix-8001,添加一个Servlet
3. 启动一个集群70001，hystrix-8001,9001,测试访问

新建模块：springcloud-consumer-hystrix-dashboard

1. 导入consumer-80的依赖，添加hystix ，dashboard的依赖

   ```xml
     <dependencies>
   <!--        hystrix-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix-dashboard -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
   
           <dependency>
               <groupId>org.joker</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
           <!--        ribbon-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-ribbon -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
           <!--        eureka-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-client -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
               <version>3.0.2</version>
           </dependency>
   
   
       </dependencies>
   ```

   

2. 配置文件application.yml

   ```yaml
   server:
     port: 9001
   ```

3. 创建主启动类

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
   
   import javax.swing.*;
   
   @SpringBootApplication
   @EnableHystrixDashboard//开启
   public class DeptConsumerDashboard_9001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumerDashboard_9001.class,args);
       }
   }
   
   ```

4. 保证服务里有 actuator 依赖

5. 启动dashboard 模块，查看 http://localhost:9001/hystrix ，可以看到一个监控页面

   ![image-20210325041630663](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325052649445.png)

6. 在hystrix-8001模块中，导入 hystrix 依赖

   ```xml
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       <version>2.2.7.RELEASE</version>
   </dependency>
   ```

7. dept-8001 主启动类中，增加一个servlet

   ```java
   package com.joker.springcloud;
   
   import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.boot.web.servlet.ServletRegistrationBean;
   import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   
   //启动类
   @SpringBootApplication
   @EnableEurekaClient//在服务启动后，自动注册到eureka 中
   @EnableDiscoveryClient//服务发现
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
       //增加一个servlet
       //固定代码
       public ServletRegistrationBean hystrixMetricsStreamServlet(){
           ServletRegistrationBean registrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
           registrationBean.addUrlMappings("/actuator/hystrix.stream");
           return registrationBean;
       }
   }
   ```

8. 启动7001，9001，dept-8001,访问测试，



# 12、Zuul路由网关

## 概述

**什么是Zuul?**

![image-20210325044730362](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325052841744.png)

**Zuul 能干嘛**

- 路由
- 过滤

Zuul 就是对 

Zuul 官方文档：https://github.com/Netflix/zuul

## 实现

1. 新建模块 springcloud-zuul-9527

2. 导入依赖，dashboard的依赖，加 zuul依赖

   ```xml
   <dependencies>
   <!--        zuul-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-zuul -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
   
           <!--        hystrix-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix-dashboard -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
   
           <dependency>
               <groupId>org.joker</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
           <!--        ribbon-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-ribbon -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
               <version>2.2.7.RELEASE</version>
           </dependency>
           <!--        eureka-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-client -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
               <version>3.0.2</version>
           </dependency>
   
   
       </dependencies>
   ```

3. 编辑application.yml文件

   ```yaml
   server:
     port: 9527
   spring:
     application:
       name: springcloud-zuul
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
     instance:
       instance-id: zuul9527.com #可以随便写，status的
       prefer-ip-address: true # 显示真实地址
   info:
     app.name: joker-springcloud
     company.name: joker.com
   ```

4. 可以改hosts文件，增加 127.0.0.1   www.joker.com

5. 添加主启动类 

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.zuul.EnableZuulProxy;
   import org.springframework.cloud.netflix.zuul.EnableZuulServer;
   
   @SpringBootApplication
   @EnableZuulProxy
   public class ZuulApplication_9527 {
       public static void main(String[] args) {
           SpringApplication.run(ZuulApplication_9527.class,args);
       }
   }
   ```

6. 启动 7001，hystrix-8001，9527，访问测试http://eureka7001.com:7001/

7. 再访问，http://www.joker.com:9527/springcloud-provider-dept/dept/get/1 ,发现也是可以访问的，把真实地址隐藏了

8. 想要隐藏服务器名，就zuul模块里配置

   ```yaml
   zuul:
     routes: #看源码，是个map,这下面就是键值对，可以随意配置
       mydept.serverId: springcloud-provider-dept
       mydept.path: /mydept/** #这两行的意思，就是，本来是用 springcloud-provider-dept 访问，现在用/mydept/**访问，达到隐藏效果
     ignored-services: springcloud-provider-dept #使原路径不能访问
   ```

9. 启动，访问，http://www.joker.com:9527/mydept/dept/get/1  发现也可以访问。且，原路径不能访问。

10. 一般有很多微服务，则，

    ```yaml
    ignored-services: "*" #隐藏全部的真实服务名，使都不能访问
    prefix: /joker #设置一个公共的前缀
    ```

11. 此时访问，http://www.joker.com:9527/joker/mydept/dept/get/1 才能访问



# 13、SpringCloud config 分布式配置

可以看中文文档：https://www.springcloud.cc/spring-cloud-dalston.html 

springCloud config 官网：https://docs.spring.io/spring-cloud-config/docs/current/reference/html/

## 概述

![image-20210325052649445](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325133317189.png)

这里的Client A。。。就是一个个的微服务

![image-20210325052841744](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325054539041.png)

##  SpringCloud config 分布式配置 能干什么

![image-20210325133317189](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210325174843008.png)



- 国内，git托管平台，最好用码云，也可以用coding,或者github,本质没啥区别，但 github 特别特别特别慢

## Git 环境搭建

步骤：

1. 下载Git :https://git-scm.com/downloads，配置初始信息，

   ```bash
   $ git config [--global] user.name "[name]"
   $ git config [--global] user.email "[email address]"
   ```

2. 建一个仓库

3. 一般下载HTTPS链接(不需要密钥) ；SSH，需要密钥

4. 在你想要放仓库的文件夹，右键，打开git, 输入命令 git clone +项目链接

5. 新建一个application.yml ,

6. 提交到远程仓库

   ```bash
   #切到项目目录下
   cd springcloud-config/
   git add .
   git status #查看文件
   git commit -m "描述信息 first commot" 
   git push origin master #这是push到远程仓库
   
   ```



## 服务端连接Git配置

可以看官网：https://www.springcloud.cc/spring-cloud-dalston.html 

步骤：

1. 创建模块 springcloud-config-server-3344

2. 导入依赖

   ```xml
   <dependencies>
   <!--    config-->
       <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-config-server -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-config-server</artifactId>
           <version>3.0.3</version>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
   </dependencies>
   ```

3. 编写配置文件

   ```yaml
   server:
     port: 3344
   spring:
     application:
       name: springcloud-config-server
     #连接远程仓库
     cloud:
       config:
         server:
           git:
             uri: https://gitee.com/joker_monster/sringcloud-config.git #https的，不是git的
   ```

4. 编写主启动类 @EnableConfigServer

   ```java
   package com.joker.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.config.server.EnableConfigServer;
   
   @SpringBootApplication
   @EnableConfigServer//开启
   public class ConfigServer_3344 {
       public static void main(String[] args) {
           SpringApplication.run(ConfigServer_3344.class,args);
       }
   }
   ```

5. 访问测试 http://localhost:3344/application-dev.yml

6. 访问格式有很多种：

   HTTP服务具有以下格式的资源：

   ```
   /{application}/{profile}[/{label}]
   /{application}-{profile}.yml
   /{label}/{application}-{profile}.yml
   /{application}-{profile}.properties
   /{label}/{application}-{profile}.properties
   ```

   http://localhost:3344/application/dev/master 等



## 客户端连接服务端访问

1. 项目文件路径springcloud-config下新建config-server.yml

   ```yaml
   spring:
     profiles:
       active: dev
   ---
   server:
     port: 8201
   #spring的配置
   spring:
     profiles: dev
     application:
       name: springcloud-provider-dept
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ---
   
   server:
     port: 8202
   #spring的配置
   spring:
     profiles: test
     application:
       name: springcloud-provider-dept
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ```

   

2. push到远程仓库

3. 新建一个模块 springcloud-config-client-3355

4. 导入依赖 

   注意，使用bootstrap时，一定要导入 对应的 bootstrap starter依赖

   ```xml
   <dependencies>
   <!--    config-->
       <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-config -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-config</artifactId>
           <version>3.0.3</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-bootstrap -->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-bootstrap</artifactId>
           <version>3.0.2</version>
       </dependency>
   
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
   
   </dependencies>
   ```

   

5. 新建一个 bootsrap.yml

   ```yaml
   #bootstrap.yml系统级别的配置，而application.yml是用户级别的配置
   spring:
     cloud:
       config:
         name: config-client #需要从git上读取的资源，不需要后缀
         profile: dev
         label: master
         uri: http://localhost:3344
   
   ```

   

6. 新建一个application.yml

   ```yaml
   #用户级别的配置
   spring:
     application:
       name: springcloud-config-client-3355
   ```

   

7. 新建一个Controller, ConfigClientController

   ```java
   package com.joker.springcloud.controller;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @RestController
   public class ConfigClientController {
       @Value("${spring.application.name}")
       private String applicationName;
       @Value("${eureka.client.service-url.defaultZone}")
       private String eurekaServer;
       @Value("${server.port}")
       private String port;
       @RequestMapping("/config")
       public String getConfig(){
           return "applicationName:"+applicationName+
                   "eurekaServer:"+eurekaServer+
                   "port"+port;
       }
   }
   ```

8. 编写主启动类

   ```java
   @SpringBootApplication
   public class ConfigClient_3355 {
       public static void main(String[] args) {
           SpringApplication.run(ConfigClient_3355.class,args);
       }
   }
   ```

9. 访问测试 http://localhost:8201/config 

   ![image-20210325174843008](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323162725072.png)

## config 远程配置实战测试

1. 在Git文件路径下新建 config-eureka.yml
2. 在Git文件路径下新建 config-dept.yml
3. push到远程
4. 新建一个模块 springcloud-config-eureka-7001 表示接受远程配置的eureka
5. 更改  springcloud-config-eureka-7001 文件，就像上个client-3355一样
6. 启动 3344，先本地自测访问发现ok  http://localhost:3344/config-eureka-dev.yml
7. 再启动config-eureka-7001,访问 http://localhost:7001/  。是可以访问的，说明以及读到 了远程的信息。
8. 再新建模块 springcloud-config-dept-8001 ，
   1. 导入原8001的依赖
   2. 把原8001的src 复制过来
   3. 添加config依赖
   4. 增加bootstrap.yml
   5. 修改 application.yml
9. 启动 config-dept-8001，访问，http://localhost:7001/ ，发现8001以及注册进来了
10. 再访问 http://localhost:8001/dept/get/1 ，也是可以访问的
11. 码云上，点编辑，远程修改 config-dept.yml 文件 
    - 把第二个的spring.profiles改为test
    - 把dev的数据库,即第一个的数据库改成db03
12. 改完之后，需要在，config-dept-8001 build 一下（热部署）才会生效，即，不用重启服务。不过，我还是重启服务 了，热部署好慢啊
13. 再次访问 http://localhost:8001/dept/get/1 发现数据来源变为了db03，是可以生效的。



**步骤实现：**

1. 在Git文件路径下新建 config-eureka.yml

   ```yaml
   spring:
     profiles:
       active: dev
   
   ---
   server:
     port: 7001
   #spring配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-eureka
   #Eureka 配置
   eureka:
     instance:
       hostname: eureka7001.com #Eureka 服务端的实例名称
     client:
       register-with-eureka: false #表示eureka是否向eureka注册中心 注册自己
       fetch-registry: false # fetch-registry 如果为false 表示自己就是注册中心
       service-url: #就是一个服务监控页面，可以看源码得知它有一个默认的地址，要改为自己的
         #单机
   #      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
         #集群
         defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ---
   
   server:
     port: 7001
   #spring配置
   spring:
     profiles: test
     application:
       name: springcloud-config-eureka
   #Eureka 配置
   eureka:
     instance:
       hostname: eureka7001.com #Eureka 服务端的实例名称
     client:
       register-with-eureka: false #表示eureka是否向eureka注册中心 注册自己
       fetch-registry: false # fetch-registry 如果为false 表示自己就是注册中心
       service-url: #就是一个服务监控页面，可以看源码得知它有一个默认的地址，要改为自己的
         #单机
   #      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
         #集群
         defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ```

   

2. 在Git文件路径下新建 config-dept.yml

   ```yaml
   spring:
     profiles:
       active: dev
   
   ---
   server:
     port: 8001
   #mybatis配置
   mybatis:
     type-aliases-package: com.joker.springcloud.pojo
     config-location: classpath:mybatis/mybatis-config.xml
     mapper-locations: classpath:mybatis/mapper/*.xml
   #spring的配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?userUnicode=true&characterEncoding=utf-8&useSSL=true
       username: root
       password: 123456
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
     instance:
       instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息 status
       prefer-ip-address: true # 修改为ture，eureka status 跳转连接的域名，显示真实的ip地址
   
   # info 配置
   info:
     app.name: joker-springcloud
     company.name: blog.joker.com
   
   ---
   server:
     port: 8001
   #mybatis配置
   mybatis:
     type-aliases-package: com.joker.springcloud.pojo
     config-location: classpath:mybatis/mybatis-config.xml
     mapper-locations: classpath:mybatis/mapper/*.xml
   #spring的配置
   spring:
     profiles: dev
     application:
       name: springcloud-config-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db02?userUnicode=true&characterEncoding=utf-8&useSSL=true
       username: root
       password: 123456
   #eureka 配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
     instance:
       instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息 status
       prefer-ip-address: true # 修改为ture，eureka status 跳转连接的域名，显示真实的ip地址
   
   # info 配置
   info:
     app.name: joker-springcloud
     company.name: blog.joker.com
   ```

3. 对应总结的第8步。再新建模块 springcloud-config-dept-8001 ，

   1. 导入原8001的依赖

   2. 把原8001的src 复制过来

   3. 添加config依赖

      ```xml
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-config</artifactId>
          <version>3.0.3</version>
      </dependency>
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-bootstrap</artifactId>
          <version>3.0.2</version>
      </dependency>
      ```

   4. 增加bootstrap.yml

      ```yaml
      spring:
        cloud:
          config:
            name: config-dept
            label: master
            profile: dev
            uri: http://localhost:3344
      ```

   5. 修改 application.yml

      ```yaml
      spring:
        application:
          name: springcloud-config-dept-8001
      ```

**总结**：

- 就是把所有配置都放在远程一起配置，本地，就只有很简单的config配置
- 有问题，因为版本有差，就去看官网 https://docs.spring.io/spring-cloud-config/docs/current/reference/html/



# 14、展望

![image-20210325054539041](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210323232805293.png)