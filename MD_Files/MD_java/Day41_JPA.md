# 目录

[toc]

# Day41_JPA

- 时间：2022-07-14
- 参考链接：
  - JPA：
    - JPA 2.2 接口文档：https://javadoc.io/doc/javax.persistence/javax.persistence-api/latest/index.html
    - JPA oracle 接口文档：https://docs.oracle.com/javaee/7/api/javax/persistence/package-summary.html
  - Spring Data JPA
    - Spring Data JPA 2.7.1 API ：https://docs.spring.io/spring-data/jpa/docs/current/api/
    - Spring Data JPA 2.7.1 ReferenceDoc: https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#preface
    - Spring Data JPA 的使用介绍：
      - https://juejin.cn/post/6972475771663155236
      - https://www.cnblogs.com/ityouknow/p/5891443.html
    - spring boot 引入 JPA ：https://cloud.tencent.com/developer/article/1415399
  - mybatis 对比 JPA：
    - https://blog.csdn.net/qq_45735705/article/details/115988506
    - https://www.cnblogs.com/july-sunny/p/12439390.html
    - https://juejin.cn/post/6880696204297142280#comment





# 1、JPA 介绍



### 首先了解 Jpa 是什么？

Jpa (Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象/关联映射工具来管理 Java 应用中的关系数据。它的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。

值得注意的是，Jpa是在充分吸收了现有 Hibernate，TopLink，JDO 等 ORM 框架的基础上发展而来的，具有易于使用，伸缩性强等优点。从目前的开发社区的反应上看，Jpa 受到了极大的支持和赞扬，其中就包括了 Spring 与 EJB3. 0的开发团队。

> 注意:Jpa 是一套规范，不是一套产品，那么像 Hibernate,TopLink,JDO 他们是一套产品，如果说这些产品实现了这个 Jpa 规范，那么我们就可以叫他们为 Jpa 的实现产品。





# 2、Spring Data JPA 介绍

- **Spring Data JPA** 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套 JPA 应用框架，底层使用了 Hibernate 的 JPA 技术实现，可使开发者用极简的代码即可实现对数据的访问和操作。









# THE END