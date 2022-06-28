---
typora-copy-images-to: md-imgs
---

# Day28-MybatisPlus

学习视频：狂神说

2021-04-06

MybatisPlus 3.4.2

# 1、MybatisPlus 概述

需要的基础：MyBatis、Spring、SpringMVC

为什么要学习它呢？

MyBatisPlus可以节省我们大量工作时间，所有的CRUD代码它都可以**自动化完成**！

三个最常用的偷懒的自动化代码工具：JPA 、 tk-mapper、MyBatisPlus

## 1.1 简介

是什么？ MyBatis 本来就是简化 JDBC 操作的！而 MybatisPlus  简化 MyBatis 。

MybatisPlus  官网：https://baomidou.com/

MybatisPlus 各类示例：https://github.com/baomidou/mybatis-plus-samples

好的博客：https://janycode.github.io/2021/06/13/05_%E6%95%B0%E6%8D%AE%E5%BA%93/03_MyBatis-Plus/02-MyBatis-Plus%20%E9%AB%98%E7%BA%A7%E4%BD%BF%E7%94%A8/index.html

一切都可以根据**官网指南**学习

## 1.2 特性

+ **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
+ **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
+ **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求。（**简单的CRUD不用自己写了，自动生成**）
+ **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
+ **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
+ **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
+ **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
+ **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用 （**自动生成代码**）
+ **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
+ **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
+ **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
+ **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作



# 2、快速入门

快速入门 官网：https://baomidou.com/guide/quick-start.html#%E5%88%9D%E5%A7%8B%E5%8C%96%E5%B7%A5%E7%A8%8B

使用第三方组件的套路：

1、导入对应的依赖

2、研究依赖如何配置

3、代码如何编写

4、提高扩展技术能力！



## 2.1 步骤

- 对应官方文档就好

1. 创建数据库 mybatis-plus

```sql
-- 其对应的数据库 Schema 脚本如下：
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
-- 其对应的数据库 Data 脚本如下：
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');

-- 真实开发中，version（乐观锁）、deleted（逻辑删除）、gmt_create、gmt_modified
```

2. 编写项目，初始化项目！使用SpringBoot初始化！

3. 导入依赖

   说明：我们使用 mybatis-plus 可以节省我们大量的代码，尽量不要同时导入 mybatis 和 mybatis-plus！因为有版本的差异！

   ```xml
       <dependencies>
   <!--        数据库驱动-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-boot-starter</artifactId>
               <version>3.4.2</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
   ```

4. 连接数据库。就是编写配置文件，和mybatis相同。

```yaml
#mysql5 驱动com.mysql.jdbc.Driver
#mysql8
# 驱动不同 com.mysql.cj.jdbc.Driver，但是，高版本的驱动兼容低版本去驱动，所以，这里可以配置8的驱动。
# url 且需要设置时区。serverTimezone=Asia/Shanghai
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    schema: classpath:db/schema-h2.sql
    data: classpath:db/data-h2.sql
    url: jdbc:mysql://localhost:3306/mybatis-plus?useSSL=false&useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456

```

 5. 传统方式：pojo-dao（连接mybatis，配置mapper.xml文件）-service-controller

    使用了mybatis-plus 之后：

    - pojo类 User

    ```java
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    public class User {
        private Long id;
        private String name;
        private Integer age;
        private String email;
    }
    ```

    - mapper接口：UserMapper

    ```java
    import com.baomidou.mybatisplus.core.mapper.BaseMapper;
    import com.joker.mybatis_plus.pojo.User;
    import org.apache.ibatis.annotations.Mapper;
    
    //在对应的mapper上面实现基本的类 BaseMapper
    @Mapper
    public interface UserMapper extends BaseMapper<User> {
        //只需要写这个接口，所有的 CRUD 操作都已经完成。
    }
    ```

    - 主启动类，添加 @MapperScan("com.joker.mybatis_plus.mapper")//扫描mapper文件夹

    - 测试使用

      ```java
      package com.joker.mybatis_plus;
      
      import com.joker.mybatis_plus.mapper.UserMapper;
      import com.joker.mybatis_plus.pojo.User;
      import org.junit.jupiter.api.Test;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      
      import java.util.List;
      
      @SpringBootTest
      class MybatisPlusApplicationTests {
          //继承 了BaseMapper ，所有的方法都来自于它，也可以编写自己的扩展方法。
          @Autowired
          private UserMapper userMapper;
          @Test
          void contextLoads() {
              //查询所有用户
              //selectList() 参数是一个 Wrapper 。条件构造器。这里先不用，写为null
              List<User> users = userMapper.selectList(null);
              users.forEach(System.out::println);
          }
      
      }
      ```

	6. 结果，全部用户输出。

![image-20210406211414513](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406211414513.png)

## 2.2 思考问题

1、SQL谁帮我们写的 ? MyBatis-Plus 都写好了

2、方法哪里来的？ MyBatis-Plus 都写好了



## 2.3 配置日志

- 所有的SQL 现在是不可见的，希望知道它是怎么执行 的，所以必须看日志。

添加配置：

```yaml
# 配置日志 ,这是mybatis本身的日志，也可以配置其它的，比如log4j,要导依赖
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

结果：

![image-20210406212341651](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406212341651.png)



配置完毕日志之后，后面的学习就需要注意这个自动生成的SQL。



# 3、Mappper 的 CRUD 扩展

- 

## 3.1 Insert 插入

```java
    @Test//测试插入
    public void testInsert() {
        User user = new User();
        user.setName("joker");
        user.setAge(3);
        user.setEmail("fdsfasf@qq.com");
        int result = userMapper.insert(user); // 帮我们自动生成id
        System.out.println(result); // 受影响的行数
        System.out.println(user); // 发现，id会自动回填
    }
```

结果：插入成功

![image-20210406213555010](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406213555010.png)

- 数据库插入的id的默认值为：全局的唯一id



## 3.2 主键生成策略

参考博客：https://www.cnblogs.com/haoxinyue/p/5208136.html

分布式系统唯一id生成的几种策略：自增，UUID ， redis , 雪花算法（snowflake），zookeeper,

重点还是雪花算法。

### 雪花算法

snowflflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其**核心思想**是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。可以保证几乎全球唯一！ 



看 @TableId 源码可知，IdType.ASSIGN_ID 使用于雪花算法，保证id全局唯一。

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId(type = IdType.ASSIGN_ID )
    private Long id;
```

### 主键自增

源码分析：@TableId->IdType

```java

package com.baomidou.mybatisplus.annotation;

import lombok.Getter;

/**
 * 生成ID类型枚举类
 *
 * @author hubin
 * @since 2015-11-10
 */
@Getter
public enum IdType {
    /**
     * 数据库ID自增
     * <p>该类型请确保数据库设置了 ID自增 否则无效</p>
     */
    AUTO(0),
    /**
     * 该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT)
     */
    NONE(1),
    /**
     * 用户输入ID
     * <p>该类型可以通过自己注册自动填充插件进行填充</p>
     */
    INPUT(2),

    /* 以下3种类型、只有当插入对象ID 为空，才自动填充。 */
    /**
     * 分配ID (主键类型为number或string）,
     * 默认实现类 {@link com.baomidou.mybatisplus.core.incrementer.DefaultIdentifierGenerator}(雪花算法)
     *
     * @since 3.3.0
     */
    ASSIGN_ID(3),
    /**
     * 分配UUID (主键类型为 string)
     * 默认实现类 {@link com.baomidou.mybatisplus.core.incrementer.DefaultIdentifierGenerator}(UUID.replace("-",""))
     */
    ASSIGN_UUID(4),
    /**
     * @deprecated 3.3.0 please use {@link #ASSIGN_ID}
     */
    @Deprecated
    ID_WORKER(3),
    /**
     * @deprecated 3.3.0 please use {@link #ASSIGN_ID}
     */
    @Deprecated
    ID_WORKER_STR(3),
    /**
     * @deprecated 3.3.0 please use {@link #ASSIGN_UUID}
     */
    @Deprecated
    UUID(4);

    private final int key;

    IdType(int key) {
        this.key = key;
    }
}

```

IdType.AUTO：

1、实体类字段上 @TableId(type = IdType.AUTO) 

2、数据库字段一定要是自增！

3、再次测试插入即可！

IdType.INPUT：

必须手动设置ID 即加代码，setId();





## 3.3 update 更新

```java
@Test//测试更新
public void testUpdate(){
    User user = new User();
    //通过条件，自动拼接动态SQL
    user.setId(3L);
    user.setName("joker_update");
    user.setAge(18);
    //updateById 参数是一个对象
    int i = userMapper.updateById(user);
    System.out.println(i);
}
```



所有的sql都是自动帮你配置的



## 3.4 自动填充

创建时间、修改时间！这些个操作一遍都是自动化完成的，我们不希望手动更新！

阿里巴巴开发手册：所有的数据库表：gmt_create、gmt_modifified 几乎所有的表都要配置上！而且需要自动化！

gmt：代表格林尼治时间

自动填充 官网：https://baomidou.com/guide/auto-fill-metainfo.html

### 方式一：数据库级别

- 工作中不允许你修改数据库,只是了解有这种方式。

1、在表中新增字段 create_time, update_time

![image-20210406221958510](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406221958510.png)

2、再次测试插入方法，我们需要先把实体类同步！

```java
    private Date createTime;
    private Date updateTime;
```

3、再次更新查看结果即可

![image-20210406222548631](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406222548631.png)

### 方式二：代码级别

1、删除数据库的默认值、更新操作！

![image-20210406222749615](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406222749615.png)

2、实体类字段属性上需要增加注解

```java
//字段添加填充内容
@TableField(fill = FieldFill.INSERT)
private Date createTime;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date updateTime;
```

3、编写处理器来处理这个注解即可！

```java
package com.joker.mybatis_plus.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;
//官网复制的
@Slf4j
@Component //一定不要忘记使用 @Component ，把处理器加到IOC容器中。
public class MyMetaObjectHandler implements MetaObjectHandler {
    //插入时的填充策略
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        //strictInsertFill(MetaObject metaObject, String fieldName, Class<T> fieldType, E fieldVal)
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)

    }
    //更新时的填充策略
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐)

    }
}
```

4、因为handler 使用的是 LocalDateTime ,所以需要把实体类的对应的也改为LocalDateTime  。测试插入,观察插入时间

5、测试更新、观察时间即可！

![image-20210406224825376](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406224825376.png)



## 3.5 乐观锁

- **面试重点**，我们经常会被问道乐观锁，悲观锁！这个其实非常简单！
- 乐观锁 官网：https://baomidou.com/guide/interceptor-optimistic-locker.html#optimisticlockerinnerinterceptor

**乐观锁** : 故名思意十分乐观，它总是认为不会出现问题，无论干什么不去上锁！如果出现了问题，

再次更新值测试

**悲观锁**：故名思意十分悲观，它总是认为总是出现问题，无论干什么都会上锁！再去操作！



我们这里主要讲解 乐观锁机制！

**乐观锁实现方式**：

- 取出记录时，获取当前 version

- 更新时，带上这个version

- 执行更新时， set version = newVersion where version = oldVersion

- 如果version不对，就更新失败

```sql
-- 乐观锁：1、先查询，获得版本号 version = 1 
-- 线程A 
update user set name = "kuangshen", version = version + 1 where id = 2 and version = 1 
-- 线程B抢先完成 
-- ，这个时候 version = 2，会导致 A 修改失败！ 
update user set name = "kuangshen", version = version + 1 where id = 2 and version = 1
```



### 测试一下MP的乐观锁插件

有关插件的，从3.4.0开始有改变。

插件变化 官网：https://baomidou.com/guide/interceptor.html#mybatisplusinterceptor



**实现乐观锁：**

1、给数据库中增加version字段！

2、我们实体类加对应的字段

```java
@Version//乐观锁注解
private Integer version;
```

3、注册组件。就是一个配置一个乐观锁插件

```java
package com.joker.mybatis_plus.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

//配置一个乐观锁插件
@MapperScan("com.joker.mybatis_plus.mapper")//扫描mapper文件夹
@EnableTransactionManagement
@Configuration //代表一个配置类
public class MyBatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor MybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

4、测试一下！

```java
// 测试乐观锁成功！这是在单线程情况下，一定可以成功修改。
@Test
public void testOptimisticLocker() {
    // 1、查询用户信息
    User user = userMapper.selectById(1L);
    // 2、修改用户信息
    user.setName("joker");
    user.setEmail("wulawulawula@qq.com");
    // 3、执行更新操作
    userMapper.updateById(user);
}

// 测试乐观锁失败！多线程下
@Test
public void testOptimisticLocker2() {
    // 线程 1
    User user = userMapper.selectById(1L);
    user.setName("joker111111");
    user.setEmail("wahhhhhhhh@qq.com");
    // 模拟另外一个线程执行了插队操作
    User user2 = userMapper.selectById(1L);
    user2.setName("joker222");
    user2.setEmail("wullllllll@qq.com");
    userMapper.updateById(user2);
    // 自旋锁来多次尝试提交
    userMapper.updateById(user); // 如果没有乐观锁就会覆盖插队线程的值！
}
```

最终结果：

![image-20210406232500694](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Day28-MybatisPlus.assets/image-20210406232500694.png)



## 3.6 select 查询

```java
//测试查询 selectById
@Test
public void testSelectById(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
// 测试批量查询！ selectBatchIds
@Test
public void testSelectByBatchId(){
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    users.forEach(System.out::println);
}
// 按条件查询之一 使用map操作 selectByMap
@Test
public void testSelect(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name","joker");
    map.put("age","3");
    List<User> users = userMapper.selectByMap(map);
    users.forEach(System.out::println);
}
```



##  3.7 page 分页查询

MP 分页插件 官网：https://baomidou.com/pages/97710a/#paginationinnerinterceptor

分页在网站使用的十分之多！

一般的分页方式：

1、原始的 limit 进行分页

2、pageHelper 第三方插件

3、MP 其实也内置了分页插件！



### 3.7.1 MP 分页基本信息

- MP 内部有 Page 类（实现了 IPage 接口），负责分页接口功能提供。

- BaseMapper中提供了2个方法，进行分页查询:

  - `selectPage()`: 将查询的结果封装成Java实体对象

    ```java
    <E extends IPage<T>> E selectPage(E page, @Param("ew") Wrapper<T> queryWrapper);
    ```

  - `selectMapsPage()`：会封装成Map<String,Object>。

    ```java
    <E extends IPage<Map<String, Object>>> E selectMapsPage(E page, @Param("ew") Wrapper<T> queryWrapper);
    ```

    

### 3.7.2 MP分页实现步骤

1、在配置类中 ，配置拦截器组件

```java
//分页插件
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
    return interceptor;
}
```

2、测试使用。直接使用Page对象

```java
//测试分页查询
@Test
public void testPage(){
    //参数一：当前页
    //参数二：页面大小
    //使用过了分页插件，所有的分页都变简单了。
    Page<User> page = new Page<>(2,5);
    userMapper.selectPage(page,null);

    page.getRecords().forEach(System.out::println);
    System.out.println(page.getTotal());

}
```



### 3.7.3 Page 类的局部变量

```java
# Page 类的局部变量
protected List<T> records; // 查询到的记录列表
protected long total; // 返回查询到的记录的总数
protected long size;// 页面大小（每页显示条数，默认为10）
protected long current; // 当前页码，（默认为1 ）
protected List<OrderItem> orders;// 排序字段信息
protected boolean optimizeCountSql;// 自动优化 COUNT SQL,默认true
// 是否进行 count 查询(及查询数据库总记录数),默认true,设置为false 后，不会返回 total
protected boolean isSearchCount;
protected boolean hitCount;// 是否命中 count 缓存,默认false
// countId是分页查询方法用来查询所有结果数量的方法，可以是当前Mapper接口的方法名（或者说是当前Mapper.xml文件中查询总数的select标签的id属性），也可以是其他Mapper接口的方法名（需要加上接口的全限定名）。
protected String countId;
protected Long maxLimit;// 单页分页条数限制
```

### 3.7.4 Page 类的方法

![image-20220502144621471](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220502144621471.png)

![image-20220502144811963](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220502144811963.png)

| 常用方法                                                     | 释义               |
| ------------------------------------------------------------ | ------------------ |
| ` public Page(long current, long size, long total, boolean isSearchCount)` | 构造函数           |
| `public List<T> getRecords()`                                | 返回记录列表       |
| `public long getTotal()`                                     | 返回查询到的记录数 |
| `public long getSize()`                                      | 返回页码大小       |
| `public long getCurrent()`                                   | 返回当前页码       |
|                                                              |                    |



### 3.7.5 mybatis plus github 使用示例

- GitHub 使用示例：https://github.com/baomidou/mybatis-plus-samples/tree/master/mybatis-plus-sample-pagination

```java
package com.baomidou.mybatisplus.samples.pagination;

import static org.assertj.core.api.Assertions.assertThat;

import java.util.List;

import javax.annotation.Resource;

import org.apache.ibatis.session.RowBounds;
import org.assertj.core.util.Maps;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.util.CollectionUtils;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.TypeReference;
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.core.metadata.OrderItem;
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import com.baomidou.mybatisplus.extension.conditions.query.LambdaQueryChainWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.samples.pagination.entity.User;
import com.baomidou.mybatisplus.samples.pagination.mapper.UserMapper;
import com.baomidou.mybatisplus.samples.pagination.model.MyPage;
import com.baomidou.mybatisplus.samples.pagination.model.ParamSome;
import com.baomidou.mybatisplus.samples.pagination.model.UserChildren;
import com.baomidou.mybatisplus.samples.pagination.service.IUserService;

import lombok.extern.slf4j.Slf4j;

/**
 * @author miemie
 * @since 2018-08-10
 */
@Slf4j
@SpringBootTest
class PaginationTest {

    @Resource
    private UserMapper mapper;

    @Test
    void lambdaPagination() {
        Page<User> page = new Page<>(1, 3);
        Page<User> result = mapper.selectPage(page, Wrappers.<User>lambdaQuery().ge(User::getAge, 1).orderByAsc(User::getAge));
        assertThat(result.getTotal()).isGreaterThan(3);
        assertThat(result.getRecords().size()).isEqualTo(3);
    }

    @Test
    void tests1() {
        log.error("----------------------------------baseMapper 自带分页-------------------------------------------------------");
        Page<User> page = new Page<>(1, 5);
        page.addOrder(OrderItem.asc("age"));
        Page<User> userIPage = mapper.selectPage(page, Wrappers.<User>lambdaQuery().eq(User::getAge, 20).like(User::getName, "Jack"));
        assertThat(page).isSameAs(userIPage);
        log.error("总条数 -------------> {}", userIPage.getTotal());
        log.error("当前页数 -------------> {}", userIPage.getCurrent());
        log.error("当前每页显示数 -------------> {}", userIPage.getSize());
        List<User> records = userIPage.getRecords();
        assertThat(records).isNotEmpty();

        log.error("----------------------------------json 正反序列化-------------------------------------------------------");
        String json = JSON.toJSONString(page);
        log.info("json ----------> {}", json);
        Page<User> page1 = JSON.parseObject(json, new TypeReference<Page<User>>() {
        });
        List<User> records1 = page1.getRecords();
        assertThat(records1).isNotEmpty();
        assertThat(records1.get(0).getClass()).isEqualTo(User.class);

        log.error("----------------------------------自定义 XML 分页-------------------------------------------------------");
        MyPage<User> myPage = new MyPage<User>(1, 5).setSelectInt(20).setSelectStr("Jack");
        ParamSome paramSome = new ParamSome(20, "Jack");
        MyPage<User> userMyPage = mapper.mySelectPage(myPage, paramSome);
        assertThat(myPage).isSameAs(userMyPage);
        log.error("总条数 -------------> {}", userMyPage.getTotal());
        log.error("当前页数 -------------> {}", userMyPage.getCurrent());
        log.error("当前每页显示数 -------------> {}", userMyPage.getSize());
    }

    @Test
    void tests2() {
        /* 下面的 left join 不会对 count 进行优化,因为 where 条件里有 join 的表的条件 */
        MyPage<UserChildren> myPage = new MyPage<>(1, 5);
        myPage.setSelectInt(18).setSelectStr("Jack");
        MyPage<UserChildren> userChildrenMyPage = mapper.userChildrenPage(myPage);
        List<UserChildren> records = userChildrenMyPage.getRecords();
        records.forEach(System.out::println);

        /* 下面的 left join 会对 count 进行优化,因为 where 条件里没有 join 的表的条件 */
        myPage = new MyPage<UserChildren>(1, 5).setSelectInt(18);
        userChildrenMyPage = mapper.userChildrenPage(myPage);
        records = userChildrenMyPage.getRecords();
        records.forEach(System.out::println);
    }

    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }


    @Test
    void testMyPageMap() {
        MyPage<User> myPage = new MyPage<User>(1, 5).setSelectInt(20).setSelectStr("Jack");
        mapper.mySelectPageMap(myPage, Maps.newHashMap("name", "%a"));
        myPage.getRecords().forEach(System.out::println);
    }

    @Test
    void testMap() {
        mapper.mySelectMap(Maps.newHashMap("name", "%a")).forEach(System.out::println);
    }

    @Test
    void myPage() {
        MyPage<User> page = new MyPage<>(1, 5);
        page.setName("a");
        mapper.myPageSelect(page).forEach(System.out::println);
    }

    @Test
    void iPageTest() {
        IPage<User> page = new Page<User>(1, 5) {
            private String name = "%";

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }
        };

        List<User> list = mapper.iPageSelect(page);
        System.out.println("list.size=" + list.size());
        System.out.println("page.total=" + page.getTotal());
    }

    /**
     * 只查询当前页的记录，不查询总记录数
     */
    @Test
    void currentPageListTest() {
        //使用三参数的构造器创建Page对象
        //第三个参数isSearchCount：传true则查询总记录数;传false则不查询总记录数（既不进行count查询）
        Page<User> page = new Page<>(1,3,false);
        Page<User> result = mapper.selectPage(page, Wrappers.<User>lambdaQuery().ge(User::getAge, 20));
        assertThat(result.getRecords().size()).isEqualTo(3);
        //因为没有进行count查询，total值为0
        assertThat(result.getTotal()).isEqualTo(0);
    }

    @Test
    void rowBoundsTest() {
        RowBounds rowBounds = new RowBounds(0, 5);
        List<User> list = mapper.rowBoundList(rowBounds, Maps.newHashMap("name", "%"));
        System.out.println("list.size=" + list.size());
    }

    @Test
    void selectAndGroupBy() {
        LambdaQueryWrapper<User> lq = new LambdaQueryWrapper<>();
        lq.select(User::getAge).groupBy(User::getAge);
        for (User user : mapper.selectList(lq)) {
            System.out.println(user.getAge());
        }
    }

    @Autowired
    IUserService userService;

    @Test
    void lambdaPageTest() {
        LambdaQueryChainWrapper<User> wrapper2 = userService.lambdaQuery();
        wrapper2.like(User::getName, "a");
        userService.page(new Page<>(1, 10), wrapper2.getWrapper()).getRecords().forEach(System.out::print);
    }

    @Test
    void test() {
        userService.lambdaQuery().like(User::getName, "a").list().forEach(System.out::println);

        Page page = userService.lambdaQuery().like(User::getName, "a").page(new Page<>(1, 10));
        page.getRecords().forEach(System.out::println);
    }
}
```



## 3.8 delete 删除

delete 的接口：

```java
// 根据 entity 条件，删除记录
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
// 删除（根据ID 批量删除）
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 ID 删除
int deleteById(Serializable id);
// 根据 columnMap 条件，删除记录
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```

参数说明

|                类型                |  参数名   |                描述                |
| :--------------------------------: | :-------: | :--------------------------------: |
|             Wrapper<T>             |  wrapper  | 实体对象封装操作类（可以为 null）  |
| Collection<? extends Serializable> |  idList   | 主键ID列表(不能为 null 以及 empty) |
|            Serializable            |    id     |               主键ID               |
|        Map<String, Object>         | columnMap |          表字段 map 对象           |



基本接口的测试：

```java
//测试删除 delete
@Test
public void testDeleteById(){
    userMapper.deleteById(1379425851690692609L);
}
//批量删除
@Test
public void testDeleteBatchIds(){
    int batchIds = userMapper.deleteBatchIds(Arrays.asList(1379439563847753729L, 1379444113316921345L));
}
//通过map删除
@Test
public void testDeleteByMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name","joker");
    int batchIds = userMapper.deleteByMap(map);
}
```



## 3.9 逻辑删除

- 逻辑删除 官网：https://baomidou.com/guide/logic-delete.html#%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95

物理删除 ：从数据库中直接移除 

逻辑删除 ：再数据库中没有被移除，而是通过一个变量来让他失效！ deleted = 0 => deleted = 1



管理员可以查看被删除的记录！防止数据的丢失，类似于回收站！

测试：

1、在数据表中增加一个 deleted 字段

```sql
    `deleted` int(1)  default 0 null comment '逻辑删除'
```

2、实体类中增加属性

```java
@TableLogic//逻辑删除注解
private Integer deleted;
```

3、配置！

```yaml
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: deleted  # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置 @TableLogic注解)
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

4、测试删除。发现逻辑删除，本质是一个更新操作，不是删除操作。

结果：

```bash
JDBC Connection [HikariProxyConnection@1761011037 wrapping com.mysql.cj.jdbc.ConnectionImpl@6fff46bf] will not be managed by Spring
==>  Preparing: UPDATE user SET deleted=1 WHERE id=? AND deleted=0
==> Parameters: 1379445215932407810(Long)
<==    Updates: 1
```

5、执行查询操作，查询刚才删除的id。发现查不到。

结果：

```bash
JDBC Connection [HikariProxyConnection@132338135 wrapping com.mysql.cj.jdbc.ConnectionImpl@43c57161] will not be managed by Spring
==>  Preparing: SELECT id,name,age,email,version,deleted,create_time,update_time FROM user WHERE id=? AND deleted=0
==> Parameters: 1379445215932407810(Long)
<==      Total: 0
```



**总结**：以上的所有CRUD操作及其扩展操作，必须精通掌握！会大大提高工作和写项目的效率！



## 3.10 执行 SQL 分析打印插件

- 执行 SQL 分析打印插件 是MP 的  **性能分析插件**
- 官网名 https://baomidou.com/guide/p6spy.html

我们在平时的开发中，会遇到一些慢sql。一般通过，测试， druid,,,,,等方法，来查出。



MP也提供性能分析插件，如果超过这个时间就停止运行！

作用：性能分析拦截器，用于输出每条 SQL 语句及其执行时间



- 这个因为更新了，后面再说，看官网学习把





# 4、AutoGenerator  代码自动生成器

代码自动生成器 官网：https://baomidou.com/guide/generator.html

https://github.com/baomidou/generator/blob/develop/README.md

AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

要先导入依赖：

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>
<dependency>
   	<groupId>org.apache.velocity</groupId>
  	<artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

测试代码

```java
package com.joker.mybatis_plus;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.po.TableFill;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import java.util.ArrayList;
//MP代码自动生成器
public class AutoGeneratorCode {
    public static void main(String[] args) {
        // 需要构建一个 代码自动生成器 对象
        AutoGenerator mpg = new AutoGenerator();

        // 1、全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("joker");
        gc.setOpen(false);
        gc.setFileOverride(false); // 是否覆盖
        gc.setServiceName("%sService"); // 去Service的I前缀,正则表达式
        gc.setIdType(IdType.ID_WORKER);
        gc.setDateType(DateType.ONLY_DATE);
        gc.setSwagger2(true);
        mpg.setGlobalConfig(gc);
        
        //2、设置数据源
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/mybatis-plus? useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("123456");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);
        
        //3、包的配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("blog");
        pc.setParent("com.joker");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setController("controller");
        mpg.setPackageInfo(pc);
        
        //4、策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setInclude("blog_tags", "course", "links", "sys_settings", "user_record", " user_say"); // 设置要映射的表名
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true); // 自动lombok；
        strategy.setLogicDeleteFieldName("deleted");//逻辑删除
        // 自动填充配置
        TableFill gmtCreate = new TableFill("gmt_create", FieldFill.INSERT);
        TableFill gmtModified = new TableFill("gmt_modified", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(gmtCreate);
        tableFills.add(gmtModified);
        strategy.setTableFillList(tableFills);
        // 乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true); // localhost:8080/hello_id_2
        
        mpg.setStrategy(strategy);
        
        mpg.execute(); //执行
    }
}
```



# 5、wrapper 条件构造器 （重要）

条件构造器 官网：https://baomidou.com/pages/10c804/#abstractwrapper

==十分重要==

我们写一些**复杂的sql**就可以使用它来替代！根据官网学习。



## 5.1 Wrapper 的基本信息

- Wrapper 作用：就是用来**构造 sql 的 where  条件**。
- 

### 5.1.1 Wrapper 类图

- （version 3.4.1）

![image-20220502001228158](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220502001228158.png)



主要的类就是: 

- AbstractWrapper ：下面两个的父类：提供公共方法构建 sql  的where 条件。

- UpdateWrapper：针对 update 语句，
- QueryWrapper：主要 是 select 语句。
- (另外就是分别有对应的 Lambda 形式的实现类，对应的接口的入参写法不同)。



### 5.1.2 Wrapper 接口的注意事项

#### condition 

- 出现的第一个入参`boolean condition`表示该条件**是否**加入最后生成的sql中，例如：query.like(StringUtils.isNotBlank(name), Entity::getName, name) .eq(age!=null && age >= 0, Entity::getAge, age)

## 5.2 Wrapper 的创建及使用方式

### 5.2.1 普通接口类

- UpdateWrapper， QueryWrapper
- 使用示例详见后面，接口见官方文档。

```java
// new 一个就可以了
```



### 5.2.2 lambda 类 

- LambdaUpdateWrapper，lambdaQueryWrapper
- 使用示例详见后面

### 5.2.3 工具类 Wrappers

- 工具类 Wrappers ：可以使用静态方法构建上面两个，就不用 new 上面两个对象了。
- Wrapper 的方法：

![image-20220502005215191](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220502005215191.png)

- 使用示例：

  ```java
  // 普通接口和 Wrappers 的使用对比
  // 1、普通接口
  // 查询name不为空的用户，并且邮箱不为空的用户，年龄大于等于12
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  wrapper
          .isNotNull("name")
          .isNotNull("email")
          .ge("age", 20);//大于等于 >= age>=20
  userMapper.selectList(wrapper);//可以和map对比一下。
  
  // 2、Wrappers 工具类的使用示例
  userMapper.selectList(Wrappers.lambdaQuery(User.class)
          .isNotNull(User::getName)
          .isNotNull(User::getEmail)
          .ge(User::getAge, 20));
  ```

  

## 5.3 Wrapper GitHub 使用示例

### 5.3.1 WrapperTest

```java 
package com.baomidou.mybatisplus.samples.wrapper;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
import com.baomidou.mybatisplus.samples.wrapper.entity.User;
import com.baomidou.mybatisplus.samples.wrapper.mapper.RoleMapper;
import com.baomidou.mybatisplus.samples.wrapper.mapper.UserMapper;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.util.CollectionUtils;

import javax.annotation.Resource;
import java.util.List;

/**
 * @author miemie
 * @since 2018-08-10
 */
@SpringBootTest
public class WrapperTest {

    @Resource
    private UserMapper userMapper;
    @Resource
    private RoleMapper roleMapper;

    @Test
    public void tests() {
        System.out.println("----- 普通查询 ------");
        List<User> plainUsers = userMapper.selectList(new QueryWrapper<User>().eq("role_id", 2L));
        List<User> lambdaUsers = userMapper.selectList(new QueryWrapper<User>().lambda().eq(User::getRoleId, 2L));
        Assertions.assertEquals(plainUsers.size(), lambdaUsers.size());
        print(plainUsers);

        System.out.println("----- 带子查询(sql注入) ------");
        List<User> plainUsers2 = userMapper.selectList(new QueryWrapper<User>()
                .inSql("role_id", "select id from role where id = 2"));
        List<User> lambdaUsers2 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .inSql(User::getRoleId, "select id from role where id = 2"));
        Assertions.assertEquals(plainUsers2.size(), lambdaUsers2.size());
        print(plainUsers2);

        System.out.println("----- 带嵌套查询 ------");
        List<User> plainUsers3 = userMapper.selectList(new QueryWrapper<User>()
                .nested(i -> i.eq("role_id", 2L).or().eq("role_id", 3L))
                .and(i -> i.ge("age", 20)));
        List<User> lambdaUsers3 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .nested(i -> i.eq(User::getRoleId, 2L).or().eq(User::getRoleId, 3L))
                .and(i -> i.ge(User::getAge, 20)));
        Assertions.assertEquals(plainUsers3.size(), lambdaUsers3.size());
        print(plainUsers3);

        System.out.println("----- 自定义(sql注入) ------");
        // 方式一
        List<User> plainUsers4 = userMapper.selectList(new QueryWrapper<User>()
                .apply("role_id = 2"));
/*        List<User> lambdaUsers4 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .apply("role_id = 2"));*/
        // 方式二
        List<User> plainUsers5 = userMapper.selectList(new QueryWrapper<User>()
                .apply("role_id = {0}",2));
/*        List<User> lambdaUsers5 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .apply("role_id = {0}",2));*/
        print(plainUsers4);
        Assertions.assertEquals(plainUsers4.size(), plainUsers5.size());

        UpdateWrapper<User> uw = new UpdateWrapper<>();
        uw.set("email", null);
        uw.eq("id", 4);
        userMapper.update(new User(), uw);
        User u4 = userMapper.selectById(4);
        Assertions.assertNull(u4.getEmail());


    }

    @Test
    public void lambdaQueryWrapper() {
        System.out.println("----- 普通查询 ------");
        List<User> plainUsers = userMapper.selectList(new LambdaQueryWrapper<User>().eq(User::getRoleId, 2L));
        List<User> lambdaUsers = userMapper.selectList(new QueryWrapper<User>().lambda().eq(User::getRoleId, 2L));
        Assertions.assertEquals(plainUsers.size(), lambdaUsers.size());
        print(plainUsers);

        System.out.println("----- 带子查询(sql注入) ------");
        List<User> plainUsers2 = userMapper.selectList(new LambdaQueryWrapper<User>()
                .inSql(User::getRoleId, "select id from role where id = 2"));
        List<User> lambdaUsers2 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .inSql(User::getRoleId, "select id from role where id = 2"));
        Assertions.assertEquals(plainUsers2.size(), lambdaUsers2.size());
        print(plainUsers2);

        System.out.println("----- 带嵌套查询 ------");
        List<User> plainUsers3 = userMapper.selectList(new LambdaQueryWrapper<User>()
                .nested(i -> i.eq(User::getRoleId, 2L).or().eq(User::getRoleId, 3L))
                .and(i -> i.ge(User::getAge, 20)));
        List<User> lambdaUsers3 = userMapper.selectList(new QueryWrapper<User>().lambda()
                .nested(i -> i.eq(User::getRoleId, 2L).or().eq(User::getRoleId, 3L))
                .and(i -> i.ge(User::getAge, 20)));
        Assertions.assertEquals(plainUsers3.size(), lambdaUsers3.size());
        print(plainUsers3);

        System.out.println("----- 自定义(sql注入) ------");
        List<User> plainUsers4 = userMapper.selectList(new QueryWrapper<User>()
                .apply("role_id = 2"));
        print(plainUsers4);

        UpdateWrapper<User> uw = new UpdateWrapper<>();
        uw.set("email", null);
        uw.eq("id", 4);
        userMapper.update(new User(), uw);
        User u4 = userMapper.selectById(4);
        Assertions.assertNull(u4.getEmail());
    }

    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }

    /**
     * SELECT id,name,age,email,role_id FROM user
     * WHERE ( 1 = 1 ) AND ( ( name = ? AND age = ? ) OR ( name = ? AND age = ? ) )
     */
    @Test
    public void testSql() {
        QueryWrapper<User> w = new QueryWrapper<>();
        w.and(i -> i.eq("1", 1))
                .nested(i ->
                        i.and(j -> j.eq("name", "a").eq("age", 2))
                                .or(j -> j.eq("name", "b").eq("age", 2)));
        userMapper.selectList(w);
    }

    /**
     * SELECT id,name FROM user
     * WHERE (age BETWEEN ? AND ?) ORDER BY role_id ASC,id ASC
     */
    @Test
    public void testSelect() {
        QueryWrapper<User> qw = new QueryWrapper<>();
        qw.select("id","name").between("age",20,25)
                .orderByAsc("role_id","id");
        List<User> plainUsers = userMapper.selectList(qw);

        LambdaQueryWrapper<User> lwq = new LambdaQueryWrapper<>();
        lwq.select(User::getId,User::getName).between(User::getAge,20,25)
                .orderByAsc(User::getRoleId,User::getId);
        List<User> lambdaUsers = userMapper.selectList(lwq);

        print(plainUsers);
        Assertions.assertEquals(plainUsers.size(), lambdaUsers.size());
    }
}

```

### 5.3.2 UpdateWrapperTest

```java
package com.baomidou.mybatisplus.samples.wrapper;

import com.baomidou.mybatisplus.core.conditions.update.LambdaUpdateWrapper;
import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
import com.baomidou.mybatisplus.samples.wrapper.entity.User;
import com.baomidou.mybatisplus.samples.wrapper.mapper.UserMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

/**
 * @author sundongkai
 * @since 2021-02-04
 */
@SpringBootTest
public class UpdateWrapperTest {

    @Autowired
    private UserMapper userMapper;

    /**
     * UPDATE user SET age=?, email=? WHERE (name = ?)
     */
    @Test
    public void tests() {

        //方式一：
        User user = new User();
        user.setAge(29);
        user.setEmail("test3update@baomidou.com");

        userMapper.update(user,new UpdateWrapper<User>().eq("name","Tom"));

        //方式二：
        //不创建User对象
        userMapper.update(null,new UpdateWrapper<User>()
                .set("age",29).set("email","test3update@baomidou.com").eq("name","Tom"));
 
    }

    /**
     * 使用lambda条件构造器
     * UPDATE user SET age=?, email=? WHERE (name = ?)
     */
    @Test
    public void testLambda() {

        //方式一：
        User user = new User();
        user.setAge(29);
        user.setEmail("test3update@baomidou.com");

        userMapper.update(user,new LambdaUpdateWrapper<User>().eq(User::getName,"Tom"));

        //方式二：
        //不创建User对象
        userMapper.update(null,new LambdaUpdateWrapper<User>()
                .set(User::getAge,29).set(User::getEmail,"test3update@baomidou.com").eq(User::getName,"Tom"));

    }


}
```



# 6、MybatisPlus 补充

## 6.1 使用 MybatisPlus 内置分页

- 参考博客：
  - 官网：https://baomidou.com/pages/97710a/#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9A%84-mapper-method-%E4%BD%BF%E7%94%A8%E5%88%86%E9%A1%B5

### 6.1.1 自定义的 mapper#method 使用分页

```java
IPage<UserVo> selectPageVo(IPage<?> page, Integer state);
// or (class MyPage extends Ipage<UserVo>{ private Integer state; })
MyPage selectPageVo(MyPage page);
// or
List<UserVo> selectPageVo(IPage<UserVo> page, Integer state);
```



```xml
<select id="selectPageVo" resultType="xxx.xxx.xxx.UserVo">
    SELECT id,name FROM user WHERE state=#{state}
</select>
```

> 如果返回类型是 IPage 则入参的 IPage 不能为null,因为 返回的IPage == 入参的IPage
> 如果返回类型是 List 则入参的 IPage 可以为 null(为 null 则不分页),但需要你手动 入参的IPage.setRecords(返回的 List);
> 如果 xml 需要从 page 里取值,需要 `page.属性` 获取

### 6.1.2 其他:

> 生成 countSql 会在 `left join` 的表不参与 `where` 条件的情况下,把 `left join` 优化掉
> 所以建议任何带有 `left join` 的sql,都写标准sql,即给于表一个别名,字段也要 `别名.字段`

### 6.1.3 mybatis 分页的坑一（Collection 子查询）

- mybatis 的分页查询，不可以用与，一个实体类的属性和实体本身是一对多的关系。这时候就可以用mybatis 的子查询映射。

例子：

```xml
 <!--子查询map，解决分页问题-->
    <resultMap id="resultMapId" type="com.xx.POJO1">
        <id column="id" property="id"/>
        // ...
        <collection property="roleDOList" column="id" select="listPOJO2"/>
    </resultMap>

<select id="listPOJO1" resultMap="resultMapId">
        SELECT 
    	...
    	pa_ram
    	FROM ...
    </select>

<select id="listPOJO2" resultType="com.xx.POJO2">
        select
    	...
    where id = #{id}
    </select>

```



**其中 collection 可以传参**

例子：

```xml
 <!--子查询map，解决分页问题-->
    <resultMap id="resultMapId" type="com.xx.POJO1">
        <id column="id" property="id"/>
        // ...
        <result column="pa_ram" property="param"/>
        <collection property="roleDOList" column="{id = id, param : pa_ram}" select="listPOJO2"/>
    </resultMap>

<select id="listPOJO1" resultMap="resultMapId">
        SELECT 
    	...
   		pa_ram
    	FROM ...
    </select>

<select id="listPOJO2" resultType="com.xx.POJO2">
        SELECT
    	...
    where id = #{id} and pa_ram = #{param}
    </select>

```







## 6.2 @TableLogic

### 介绍

- 作用：在 MyBatis Plus 中，@TableLogic 注解用于实现数据库数据逻辑删除。注意，该注解只对自动注入的 sql 起效：

- 用法：在**实体属性字段**上加`@TableLogic`注解，使用`MyBatis-Plus`**自带方法**删除（`在执行BaseMapper的删除方法时，删除方法会变成修改`）和查找都会附带逻辑删除功能 (自己写的xml不会)。

- ```java
  @TableLogic
  private Integer deleted;
  ```

- value 默认为 0 ， delval 默认为 1



==注意==

**@TableLogic 字段类型支持说明：**

+ 支持所有数据类型（推荐使用 Integer、Boolean、LocalDateTime）
+ 如果数据库字段使用 datetime，逻辑未删除值和已删除值支持配置为字符串 null，另一个值支持配置为函数来获取值如now()

> 附录：
>
> （1）逻辑删除是为了方便数据恢复和保护数据本身价值等等的一种方案，但实际就是删除。
>
> （2）如果你需要频繁查出来看就不应使用逻辑删除，而是以一个状态去表示。



### @TableLogic 属性

该注解提供了两个属性，分别如下：

- value
  - 用来指定逻辑未删除值，默认为空字符串。

- delval
  - 用来指定逻辑删除值，默认为空字符串。

当然，你也可以不在 @TableLogic 注解中指定 value 和 delval 属性的值。使用全局逻辑删除配置信息，配置如下：

```yml
# application.yml
mybatis-plus:
  global-config:
    db-config:
      # 全局逻辑删除的实体字段名 (since 3.3.0, 配置后可以忽略 @TableLogic 中的配置)
      logic-delete-field: flag
      # 逻辑已删除值(默认为 1)
      logic-delete-value: 1
      # 逻辑未删除值(默认为 0)
      logic-not-delete-value: 0
```

### 使用：

#### 插入（insert）

不作限制

#### 查找（select）

@TableLogic 注解将会在 select 语句的 where 条件添加条件，过滤掉已删除数据，且使用 wrapper.entity 生成的 where 条件会忽略该字段。例如：

```
SELECT` `user_id,``name``,sex,age,deleted ``FROM` `user` `WHERE` `user_id=1 ``AND` `deleted=``'0'
```

#### 更新（update）

@TableLogic 注解将会在 update 语句的 where 条件后追加条件，防止更新到已删除数据，且使用 wrapper.entity 生成的 where条件会忽略该字段。例如：

```
UPDATE` `user` `SET` `deleted=``'1'` `WHERE` `user_id=1 ``AND` `deleted=``'0'
```

#### 删除（delete）

@TableLogic 注解会将 delete 语句转变为 update 语句，例如：

```
update` `user` `set` `deleted=1 ``where` `id = 1 ``and` `deleted=0
```



## 6.3 FieldStrategy

参考博客：

- FieldStrategy 官网：https://baomidou.com/pages/223848/#fieldstrategy

- https://www.cnblogs.com/aibilim/p/f83eda1a60d17b176f30343fa4c11b6d.html
- https://www.yisu.com/zixun/374610.html



- 作用：这个字段主要会影响`sql`拼接。我们知道，就是使用 MP  的自动注入 sql 时，当通过`Entity`对表进行更新时，只会更新非空字段。对于那些值为`NULL`的字段是不会出现在`sql`语句里的。

- 该策略约定了如何产出注入的sql,涉及`insert`,`update`以及`wrapper`内部的`entity`属性生成的 where 条件

- 在包里找到了这个枚举 `com.baomidou.mybatisplus.enums.FieldStrategy`，后面查找这个枚举的引用就很容易找到用这些配置值的地方了。

  ```java
  public enum FieldStrategy {
      IGNORED(0, "忽略判断"),
      NOT_NULL(1, "非 NULL 判断"),
      NOT_EMPTY(2, "非空判断");
  }
  ```

- FieldStrategy 有三个值：

  - IGNORED：“忽略判断”，所有字段都更新和插入。
  - NOT_NULL：“非 NULL 判断”，只更新和插入非NULL值。**FieldStrategy  默认值**
  - NOT_EMPTY：“非空判断”， 只更新和插入非NULL值且非空字符串。
  - DEFAULT： 默认的，一般只用于注解里。



在`MP`中，该设置会影响`sql`语句的拼接行为。在`2.x`版本中，没有对这个进行区分，不可单独设置字段策略。
下面通过具体的配置来解释差异

```yaml
# 2.x配置
mybatis-plus:
  mapper-locations:
    - classpath*:mapper/**/*Mapper.xml
  typeAliasesPackage: com.test.assist.dao.domain
  global-config:
    id-type: 0
    field-strategy: 2
    db-column-underline: true
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: false
```



```yaml
#3.x的配置
mybatis-plus:
  typeAliasesPackage: com.test.assist.dao.domain
  mapper-locations:
    - classpath*:mapper/**/*Mapper.xml
  global-config:
    db-config:
      select-strategy: not_empty
      insert-strategy: not_empty
      update-strategy: not_empty
      id-type: auto
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: false
```



- 通过上面的代码可以总结出：

  + NOT_EMPTY：会对字段值进行 `null` 和 `''` 比较操作
  + NOT_NULL: 只会进行null检查

  同时在`3.x`版本中，支持对`select`、`update`、`insert`设置不同的字段策略，由此看来大家对这种一刀切的行为还是比较反感的。这一点，我在github也看到`issue`。



### 如果 非要 插入 null 

### 实际项目解决方法示例一

实际项目中，配置文件中配置全局字段策略为NOT_NULL。

![mybatisPlus 中field-strategy配置失效如何解决](D:\2021java学习文件\Git Repositories\self-learning-java\MD_Files\MD_java\md-imgs\2033.jpg)

需求：实际项目中，apply_teacher字段当它为null时需要把null值更新进去。

困难：因为全局字段策略为NOT_NULL，所以默认不会更新null值进去。

**解决方法**：

利用条件构造器当值为null时set为null。

代码：

```java
Wrapper<StuApplyInfoEntity> updateWrapper = new UpdateWrapper<>();
((UpdateWrapper) updateWrapper).set(saveApply.getApplyTeacher() == null, "apply_teacher", null);
```

### 实际项目解决方法示例二

需求：state字段所有值都更新和插入。

困难：因为全局字段策略为NOT_NULL，所以默认不会更新null值进去。

解决方法：

在entity中设置state设置注解 `@TableField()`，配置 `FieldStrategy` 为 `IGNORED`。意思是"忽略判断"，所有值都更新和插入。

代码：

```java
@TableField(strategy = FieldStrategy.IGNORED, el = "state, jdbcType=VARCHAR")
private String state;
```

或者，可以指定 `select`、`update`、`insert` 的字段策略

比如：

```java
@TableField(updateStrategy=FieldStrategy.IGNORED)
private String userInfo;
```



## 6.4 @TableField

参考博客：

- 官网：https://baomidou.com/pages/223848/#tablefield



- 使用方法：见  `FieldStrategy`

+ 描述：字段注解（非主键）

```java
@TableName("sys_user")
public class User {
    @TableId
    private Long id;
    @TableField("nickname")
    private String name;
    private Integer age;
    private String email;
}
```

| 属性             | 类型                         | 必须指定 | 默认值                   | 描述                                                         |
| :--------------- | :--------------------------- | :------- | :----------------------- | :----------------------------------------------------------- |
| value            | String                       | 否       | ""                       | 数据库字段名                                                 |
| exist            | boolean                      | 否       | true                     | 是否为数据库表字段                                           |
| condition        | String                       | 否       | ""                       | 字段 `where` 实体查询比较条件，有值设置则按设置的值为准，没有则为默认全局的 `%s=#{%s}`，[参考(opens new window)](https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/SqlCondition.java) |
| update           | String                       | 否       | ""                       | 字段 `update set` 部分注入，例如：当在version字段上注解`update="%s+1"` 表示更新时会 `set version=version+1` （该属性优先级高于 `el` 属性） |
| insertStrategy   | Enum                         | 否       | FieldStrategy.DEFAULT    | 举例：NOT_NULL `insert into table_a(<if test="columnProperty != null">column</if>) values (<if test="columnProperty != null">#{columnProperty}</if>)` |
| updateStrategy   | Enum                         | 否       | FieldStrategy.DEFAULT    | 举例：IGNORED `update table_a set column=#{columnProperty}`  |
| whereStrategy    | Enum                         | 否       | FieldStrategy.DEFAULT    | 举例：NOT_EMPTY `where <if test="columnProperty != null and columnProperty!=''">column=#{columnProperty}</if>` |
| fill             | Enum                         | 否       | FieldFill.DEFAULT        | 字段自动填充策略                                             |
| select           | boolean                      | 否       | true                     | 是否进行 select 查询                                         |
| keepGlobalFormat | boolean                      | 否       | false                    | 是否保持使用全局的 format 进行处理                           |
| jdbcType         | JdbcType                     | 否       | JdbcType.UNDEFINED       | JDBC 类型 (该默认值不代表会按照该值生效)                     |
| typeHandler      | Class<? extends TypeHandler> | 否       | UnknownTypeHandler.class | 类型处理器 (该默认值不代表会按照该值生效)                    |
| numericScale     | String                       | 否       | ""                       | 指定小数点后保留的位数                                       |



## 6.5 Service 的 CRUD

- 参考网址：
  - 官网：https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%A3





# THE END

