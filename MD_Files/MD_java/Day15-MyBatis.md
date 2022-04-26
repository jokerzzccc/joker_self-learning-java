#  MyBatis

- 框架：看官方文档：https://mybatis.org/mybatis-3/zh/configuration.html

# 1. 简介



## 1.1. 什么是Mybatis

- MyBatis 是一款优秀的**持久层框架**，
- 它支持自定义 SQL、存储过程以及高级映射。
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- MyBatis 本是apache的一个[开源项目](https://baike.baidu.com/item/开源项目/3406069)iBatis, 2010年这个[项目](https://baike.baidu.com/item/项目/477803)由apache software foundation 迁移到了[google code](https://baike.baidu.com/item/google code/2346604)，并且改名为MyBatis 。
- 2013年11月迁移到[Github](https://baike.baidu.com/item/Github/10145341)。

如何**获得**Mybatis:

- maven仓库:

  ```xml
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
  </dependency>
  ```

- Github:https://github.com/mybatis/mybatis-3/releases

- 中文文档 ：https://mybatis.org/mybatis-3/zh/index.html



## 1.2. 持久化

数据持久化：

- 持久化：就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：**断电即失**
- 数据库（Jdbc），io文件持久化

为什么要持久化：

- 有一些对象，不能让它丢到
- 内存太贵了

## 1.3 持久层

Dao层，Service层，Controller层。。。

- 完成持久化工作的代码块
- 层是界限明显的

## 1.4 为什么需要mybatis

- 帮助程序员将数据存入到数据库中
- 传统的JDBC代码太复杂，要简化它，就出现 了框架。自动化。
- 不用Mybatis也可以。但用了，更容易上手
- 优点：
  - 简单易学
  - 灵活
  - sql和代码分离，提高了可维护性
  - 提供映射标签，支持对象与数据库的orm字段关系映射
  - 提供对象关系映射标签，支持对象关系组建维护
  - 提供xml标签，支持编写动态sql

# 2 第一个MyBatis程序

思路：搭建环境》导入mybatis》代码》测试

- 依据官方文档做

## 2.1 搭建环境

1. 创建数据库

```sql
CREATE DATABASE `mybatis`;
USE `mybatis`;

CREATE TABLE `user`(
`id` INT(20) NOT NULL,
`name` VARCHAR(10) NOT NULL,
`pwd` VARCHAR(10) NOT NULL,
PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `user` VALUES
(1,'张三','123456'),
(2,'李四','123456'),
(3,'王五','123456');
```

2. 创建项目：

   1. 创建一个普通的Maven项目

   2. 删除src目录，可以使得此项目为一个父项目

   3. 导入maven依赖

      ```xml
      <!--    导入依赖-->
          <dependencies>
      <!--        mysql 驱动-->
              <dependency>
                  <groupId>mysql</groupId>
                  <artifactId>mysql-connector-java</artifactId>
                  <version>5.1.47</version>
              </dependency>
      <!--        mybatis-->
              <dependency>
                  <groupId>org.mybatis</groupId>
                  <artifactId>mybatis</artifactId>
                  <version>3.5.6</version>
              </dependency>
      <!--        junit-->
              <dependency>
                  <groupId>junit</groupId>
                  <artifactId>junit</artifactId>
                  <version>4.12</version>
              </dependency>
          </dependencies>
      ```

      

## 2.2 创建一个模块

- 编写核心配置文件resources/mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8&amp;useUnicode=true&amp;useSSL=true"/>
                  <property name="username" value="root"/>
                  <property name="password" value="123456"/>
              </dataSource>
          </environment>
      </environments>
  </configuration>
  ```

- 编写mybatis工具类

```java

//sqlSessionFactory-->sqlSession
public class MybatisUtils {
    private static  SqlSessionFactory sqlSessionFactory;
    static{
        //使用mybatis第一步：获取SqlSessionFactory对象
        InputStream inputStream = null;
        try {
            String resource = "mybatis-config.xml";
            inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```



## 2.3 编写代码

1. 实体类 pojo

   ```java
   package com.joker.pojo;
   
   public class User {
       private int id;
       private String name;
       private String pwd;
   
       public User() {
       }
   
       public User(int id, String name, String pwd) {
           this.id = id;
           this.name = name;
           this.pwd = pwd;
       }
   
       public int getId() {
           return id;
       }
   
       public void setId(int id) {
           this.id = id;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public String getPwd() {
           return pwd;
       }
   
       public void setPwd(String pwd) {
           this.pwd = pwd;
       }
   
       @Override
       public String toString() {
           return "User{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   ", pwd='" + pwd + '\'' +
                   '}';
       }
   }
   ```

2. Mapper接口:mybaits 用的是mapper,等价于之前的Dao层

   ```java
   package com.joker.dao;
   
   import com.joker.pojo.User;
   
   import java.util.List;
   
   //mybaits 用的是mapper,等价于Dao
   public interface UserMapper {
       List<User> getUserList();
   }
   ```

3. 接口实现类:由原来的UserDaoImpl实现类转换为一个Mapper配置文件，UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 绑定了一个对应的Mapper 接口-->
<mapper namespace="com.joker.dao.UserMapper">
<!--    select 查询语句，-->
<!--    返回类型主要两种：resultType 返回单个，resultMap 返回一个集合-->
<!--    注意，要写类的完全限定名-->
<select id="getUserList" resultType="com.joker.pojo.User">
    select * from mybatis.user
</select>
</mapper>
```

4. 还要去mabatis-config.xml文件里添加对应的mappery映射。每一个Mapper.xml配置文件都需要在Mybatis核心配置文件 mabatis-config.xml 中注册

```xml
    
<mappers>
        <mapper resource="com/joker/dao/UserMapper.xml"/>
    </mappers>
```



## 2.4 测试

- 在test文件建立对应src的测试文件，为了规范

- 可能遇到的错误

  - java.lang.ExceptionInInitializerError,就是src里的xml文件导出资源失败了，在pom.xml 里导入之前讲过的resource解决方法就好

  - ```xml
    <!--build 里配置 resources ，防止资源导出失败-->
      <build>
        <resources>
          <resource>
            <directory>src/main/resources</directory>
            <includes>
              <include>**/*.properties</include>
              <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
          </resource>
          <resource>
            <directory>src/main/java</directory>
            <includes>
              <include>**/*.properties</include>
              <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
          </resource>
        </resources>
      </build>
    ```

  - 

junit测试

```java
package com.joker.dao;

import com.joker.pojo.User;
import com.joker.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserMapperTest {
    @Test
    public void test(){
        //第一步：获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行SQL
        //方式一：getMapper()
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.getUserList();
        for (User user : userList) {
            System.out.println(user);
        }
        //关闭
        sqlSession.close();
        //方式二：
    }
}
```

可能遇到的问题：

- 配置文件没有注册
- 绑定接口错误
- 方法名不对
- 返回类型不对
- Maven导出资源问题
- xml配置文件里如果写中文注释，会出错-,在target对应的xml文件里把乱码删了就可以运行了
- 



# 3 CRUD

## 1. namespace

namespace中的包名要和Dao/mapper接口的包名一致，完全限定包名

## 2. select

选择查询语句：

- id :就是对应namespace中的方法名
- resultType: sql语句执行的返回值
- parameterType：参数类型

只有select有resultType,增删改（insert,delete,update）都没有

1. 在UserMapper接口中写方法名

   ```java
   //根据id查询用户
   User getUserById(int id);
   ```

2. 在UserMapper.xml文件里添加sql语句

   ```xml
       <select id="getUserById" resultType="com.joker.pojo.User">
           select * from mybatis.user where id=#{id}
       </select>
   ```

   

3. 测试

   ```java
   @Test
   public void test_getUserById(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       User userById = mapper.getUserById(2);
       System.out.println(userById);
   
       sqlSession.close();
   }
   ```

## 3. insert

```xml
    <insert id="addUser" parameterType="com.joker.pojo.User">
        insert into mybatis.user values (#{id},#{name},#{pwd})
    </insert>
```



## 4.update

```xml
<update id="updateUser" parameterType="com.joker.pojo.User">
    update mybatis.user set name=#{name},pwd=#{pwd} where id=#{id};
</update>
```

## 5. delete

```xml
<delete id="deleteUser" parameterType="int">
    delete from mybatis.user where id = #{id};
</delete>
```

## 注意事项

- 增删改（insert \update\delete）必须提交事务：

- 如：

  ```java
  //增删改，必须提交事务
  @Test
  public void test_addUser(){
      SqlSession sqlSession = MybatisUtils.getSqlSession();
      UserMapper mapper = sqlSession.getMapper(UserMapper.class);
      int joker = mapper.addUser(new User(4, "joker", "342343"));
      System.out.println(joker);
      //提交事务
      sqlSession.commit();
      sqlSession.close();
  }
  ```

## 6 万能的Map

- 假设实体类或者数据库中的表，字段或者参数过多的，我们应当考虑使用map
- 优势：可以随便写名字，且不用传全参数
- 区别：
  - Map传递参数：直接在sql中取出key即可
  - 对象传递参数，直接在sql中取对象的属性即可

- 只有一个基本类型参数的情况下，可以直接在sql中取到。即，不用写paramType
- 多个参数用Map，**或者注解**

```java
    //万能的Map
    int addUser2(Map<String,Object> map);
```

```xml
    <insert id="addUser2" parameterType="map">
        insert into mybatis.user values (#{userid},#{username},#{userpwd});
    </insert>
```

```java
    //万能的Map
    @Test
    public void test_addUser2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Object> map = new HashMap<>();
        map.put("userid",5);
        map.put("username","kkkk");
        map.put("userpwd","dfdf");
        mapper.addUser2(map);
        //提交事务
        sqlSession.commit();
        sqlSession.close();
    }
```



## 7 模糊查询

- 方式一：在Java代码执行的时候传递通配符

```java
List<User> user = mapper.getUserLike("%张%");
```

- 方式二：	在sql拼接中使用通配符，但是存在SQL注入的问题

  ```xml
  <select id="getUserLike" resultType="com.joker.pojo.User">
      select * from mybatis.user where name like "%"#{value}"%";
  </select>
  ```



# 4. 配置解析

## 1、核心配置文件

- mybaties-config.xml （名字可以自己取）

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis2.png)

- 没圈的不用了解就好

## 2、环境变量（enviroments)

- MyBatis 可以配置成适应多种环境
- **不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**
- 学会使用多套配置环境
- Mybatis的默认事务管理器是JDBC ， 默认连接池：POOLED

## 3、属性（properties)-优化

我们可以通过properties 来实现引用配置文件

注意mybatis-config.xml里**标签**的**顺序是固定的**,如下图：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis1.png)

编写一个配置文件 ，比如 db.properties,建立在resources目录下，就可以直接写文件名了

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=UTF-8&useUnicode=true&useSSL=true
username=root
password=123456
```

在核心配置文件mybaties-config.xml中入

```xml
<!-- 引入外部配置文件-->
<properties resource="db.properties"/>
```

- 可以直接引入外部配置文件

- 可以在其中增加一些属性配置

- 如果两个文件有同一个字段，优先使用外部配置文件的,如：

- ```xml
  <properties resource="db.properties">
      <property name="username" value="root"/>
  </properties>
  ```



## 4、类型别名（typeAliases）-优化

- 类型别名可为 Java 类型设置一个缩写名字。 
- 它仅用于 XML 配置，意在降低冗余的全限定类名书写。
- 在总配置文件mybatis-config.xml里添加

方式一：给实体类起别名

```java
<typeAliases>
    <typeAlias type="com.joker.pojo.User" alias="user"/>
</typeAliases>
```

方式二：指定一个包名,Mybatis就会在包名下面搜索需要的javabean（实体类）；

扫描实体类的包，它的默认别名，就是这个类的首字母小写的名字，比如：User---user

```java
<typeAliases>
    <package name="com.joker.pojo"/>
</typeAliases>
```

注意：

- 在实体类较少的时候，用第一种

- 如果实体类十分多，用第二种

- 第一种，可以自己任意定义别民，第二种不行，如果非要改，需要在实体类上添加@Alias（）注解，如：

- ```java
  @Alias("uuuu")
  public class User {
  ```

- 

## 5、设置

- 记住几个重点的就行

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis3.png)

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis5.png)

## 6、其它设置

+ 不用了解
+ [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
+ [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
+ [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  + mybatis-generator-core
  + mybatis-plus
  + 通用mapper

## 7、映射器（mappers）

MapperRigstry: 注册绑定Mapper文件

方式一：{**最推荐**}

```xml
<mappers>
    <mapper resource="com/joker/dao/UserMapper.xml"/>
</mappers>
```

方式二：使用class文件绑定注册

```xml
    <mappers>
        <mapper class="com.joker.dao.UserMapper"/>
    </mappers>
```

方式三：使用扫描包进行注入绑定

```xml
    <mappers>
        <package name="com.joker.dao"/>
    </mappers>
```

方式二,方式三 注意点：

- 接口和它的Mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一个包下



## 8、生命周期和作用域

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis6.png)

作用域 和 生命周期是至关重要的，因为错误的使用会导致非常严重的**并发问题**。

**SqlSessionFactoryBuilder**

- 一旦创建了 SqlSessionFactory，就不再需要它了。
- 所以可以设置为局部变量

**SqlSessionFactory**

- 可以理解为就是数据库连接池
- SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，**没有任何理由丢弃它或重新创建另一个实例。**
- SqlSessionFactory 的最佳作用域是**应用作用域**（程序开启它开启，程序结束才结束）
- 有很多方法可以做到，最简单的就是使用**单例模式**或者**静态单例模式**。保证全局只有这一份变量

**SqlSession**

- 可以理解为，连接到连接池的一个请求
- SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是**请求或方法作用域**。
- 用完之后需要**赶紧关闭**，否则，占用资源

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis4.png)

这里面每一个Mapper 就代表一个具体的业务。



# 5、解决属性名和字段名不一致的问题

## 1、问题

数据库中的字段：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/mybatis7.png)

新建一个模块，拷贝之前的，测试实体类字段不一致的情况；

```java
public class User {
    private int id;
    private String name;
    private String password;
    ...
}
```

测试出现问题：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/1569898830704.png)

原因：

```sql
        select * from mybatis.user where id=#{id}
<!--        类型处理器使它等价于-->
        select id,name,pwd from mybatis.user where id=#{id}
```

解决办法：

- 起别名

- ```xml
  <select id="getUserById" resultType="com.joker.pojo.User">
      select id,name,pwd as password from mybatis.user where id=#{id}
  </select>
  ```



## 2、resultMap

结果集映射

```xml
<!--    结果集映射-->
<resultMap id="userMap" type="user">
<!--    column : 数据库中的字段，property：实体类中的属性-->
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>
    <select id="getUserById" resultMap="userMap">
        select * from mybatis.user where id=#{id}
    </select>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素

- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

- 这就是 `ResultMap` 的优秀之处——你完全可以不用显式地配置它们.可以**单独列出不匹配的字段和属性**

- ```java
  <resultMap id="userMap" type="user">
      <result column="pwd" property="password"/>
  </resultMap>
  ```

  



# 6、日志

## 6.1、日志工厂

如果一个数据库操作，出现了异常，我们需要排错。日志，就是最好的助手

曾经：sout\debug

现在：日志工厂

如下是，mybatis的setting里的配置：即，mybatis-config.xml的configuration标签里

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------ | ------ |
|         |                                                       |                                                              |        |

重点需要**掌握**的：

- LOG4J 
- STDOUT_LOGGING

在Mybatis 中具体使用哪一个日志，在设置中设定

- 注意，name和 value 的值一定要规范写，且，前后不能有空格

### 1、STDOUT_LOGGING 标准日志输出

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-08_00-35-25.png)



### 2、LOG4J 

什么是 LOG4J :

- Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件
- 我们也可以控制每一条日志的输出格式
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程
- 这些可以通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。

#### 准备配置

1. 导入LOG4J  的包

```xml
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
```

2. 建立配置文件log4j.properties，不用记住，百度就好

   ```properties
   ### 配置根,将等级为DEBUG的日志信息输出到cosole和file 这两个目的地 ###
   log4j.rootLogger = DEGUG,console ,file
   
   ### 设置输出sql的级别，其中logger后面的内容全部为jar包中所包含的包名 ###
   log4j.logger.org.mybatis=DEGUG
   log4j.logger.java.sql=DEGUG
   log4j.logger.java.sql.Statement=DEGUG
   log4j.logger.java.sql.PreparedStatement=DEGUG
   log4j.logger.java.sql.ResultSet=DEGUG
   ### 配置输出到控制台 ###
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold = DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern =  [%c]-%m%n
   
   ### 配置输出到文件 ###
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File = ./log/joker.log
   log4j.appender.file.MaxFileSize = 10mb
   log4j.appender.file.Threshold = DEBUG
   log4j.appender.file.layout = org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern = [%p][%d{yy-MM-dd}[%c]%m%n
   ```

3. 配置log4j 为日志的实现

   ```xml
   <settings>
       <setting name="logImpl" value="LOG4J"/>
   </settings>
   ```

4. log4j 的测试



#### 简单使用

1. 在要使用log4j的包里，导入包import org.apache.log4j.Logger;不要导错了
2. 日志对象，参数为当前类的class

```java
static Logger logger = Logger.getLogger(UserMapperTest.class);
```

3. 日志级别

   ```java
   //常用的三个
   logger.info("info: 进入了 log4j");//也就等价于输出，测试的时候用
   logger.debug("degug：进入了 log4j");//debug的时候用
   logger.error("error:get in error");//try catch里面用
   ```



# 7、分页

为什么要分页：

- 减少数据的处理量

## 7.1 使用limit分页

```sql
语法：select * from user limit startIndex,pageSize;
select * from mybatis.`user` limit 3;#[0,n)
```

使用mybatis分页

1. 接口

   ```java
   //分页
   List<User> getUserByLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" parameterType="map" resultMap="userMap">
       select * from mybatis.`user` limit #{startIndex},#{pageSize};
   </select>
   ```

3. 测试

   ```java
   @Test
   public void testLimit(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       HashMap<String, Integer> map = new HashMap<>();
       map.put("startIndex",0);
       map.put("pageSize",2);
       List<User> userByLimit = mapper.getUserByLimit(map);
       for (User user : userByLimit) {
           System.out.println(user);
       }
       sqlSession.close();
   
   }
   ```



## 7.2 RowBounds 分页，了解就好

- 不使用SQL分页，使用java代码分页

1. 接口

   ```java
   //分页二,早些年的，了解就好
   List<User> getUserByRowBounds();
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByRowBounds" resultMap="userMap">
       select * from mybatis.`user` ;
   </select>
   ```

3. 测试

   ```java
   @Test
   public void testRowBounds(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       RowBounds rowBounds = new RowBounds(1, 2);
   //通过java代码层面实现分页
       List<Object> userlist = sqlSession.selectList("com.joker.dao.UserMapper.getUserByRowBounds",null,rowBounds);
       for (Object o : userlist) {
           System.out.println(o);
       }
       sqlSession.close();
   }
     
   ```



## 7.3 分页插件（了解）

- mybatis分页插件 之一 PageHelper ,直接百度查看文档



# 8、注解开发

## 8.1 面向接口编程

- 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
- **根本原因 :  ==解耦== , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**
- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了； 
- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。



**关于接口的理解**

\- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。
\- 接口的本身反映了系统设计人员对系统的抽象理解。
\- 接口应有两类：
  \- 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  \- 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；
\- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。



**三个面向区别**

\- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
\- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
\- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构



## 8.2 使用注解开发

- 简单的sql可以用注解，复杂的用xml

1. 注解在接口上实现

   ```java
   @Select("select * from user")
   List<User> getUsers();
   ```

2. 需要再核心配置文件中绑定接口！

   ```xml
   <!--绑定接口-->
   <mappers>
       <mapper class="com.joker.dao.UserMapper"/>
   </mappers>
   ```

3. 测试

```java
@Test
public void test(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    //底层主要应用反射
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> users = mapper.getUsers();
    for (User user : users) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

本质：反射机制实现

底层：动态代理！

 ![1569898830704](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Temp.png)



**Mybatis详细的执行流程！**

![1569898830704](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-08_12-22-11.png)



## 8.3 注解实现 CRUD

1. 可以在工具类创建的时候实现自动提交事务

```java
public static SqlSession getSqlSession(){
    return sqlSessionFactory.openSession(true);
}
```

2. 编写接口，增加注解

   ```java
   public interface UserMapper {
       @Select("select * from user")
       List<User> getUsers();
   
       //方法存在多个参数，所有的参数必须前必须加上@Param() 注解
       //注意：数据的参数传递，与@Param里的值一致
       @Select("select * from user where id =#{idp}")
       User getUserById(@Param("idp") int id);
       @Insert("insert into user values (#{id},#{name},#{password})")
       int addUser(User user);
       @Update("update user set name=#{name},pwd=#{password},where id=#{id}")
       int updateUser(User user);
       @Delete("delete from user where id=#{uid}")
       int deleteUser(@Param("uid") int id);
   
   }
   ```

3. 测试



==注意==：必须把接口注册绑定到核心配置文件中

```xml
<mappers>
    <mapper class="com.joker.dao.UserMapper"/>
</mappers>
```



## 关于 @Param() 注解

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但建议大家都加上
- SQL引用的就是@Param()中设定的属性名



**#{} 与${} 区别**

- **#{}** 是预编译处理，像传进来的数据会加个" "（#将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号）

  

  **${}** 就是字符串替换。直接替换掉占位符。$方式一般用于传入数据库对象，例如传入表名.

  使用 ${} 的话会导致 sql 注入。

# 9、Lombok

- Lombok 插件官网：https://projectlombok.org/

- Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
  Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.
- 关键词： annotation，就是添加的一种注解

要么下载插件，要么直接导入jar包

1. 导入jar包

   ```xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.8</version>
   </dependency>
   ```

2. 在实体类上加注解



存在的注解

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-08_16-32-26.png)

几个常用的注解：

```java
@Data 包含：无参构造，get/set,	toString,hashcode,equals
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@Getter
@ToString
```



# 10、多对一处理

- 数据结果集映射的内容，可以对应官网看

比如 多个学生，对应一个老师

- 对于学生，**关联**（association），多个学生关联一个老师，多对一
- 对于老师而言，**集合**(collection)，一个老师集合很多学生 ，一对多

建表

```sql
CREATE TABLE `teacher` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师'); 

CREATE TABLE `student` (
  `id` INT(10) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  `tid` INT(10) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fktid` (`tid`),
  CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1'); 
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```



## 环境搭建

1. 导入lombok
2. 新建实体类 Teacher ， Student
3. 建立Mapper接口
4. 建立Mapper.xml文件
5. 在核心配置文件中绑定注册接口或者文件
6. 测试查询是否可以成功



## 方式一：按照查询嵌套处理（推荐）

```xml
<!--
思路：
1. 查询所有的学生信息
2. 根据查询出的学生tid,寻找对应的老师
-->
<select id="getStudent" resultMap="StudentTeacher">
    select * from mybatis.student;
</select>

    <resultMap id="StudentTeacher" type="Student">
<!-- 复杂的属性，单独处理，对象：association 集合 ： collection-->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select * from mybatis.teacher where id=#{id};
    </select>
```



## 方式二：按照结果查询

```xml
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id sid, s.name sname, t.name tname from mybatis.student s, mybatis.teacher t
    where s.tid=t.id;
</select>
<resultMap id="StudentTeacher2" type="Student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="Teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```



mysql 多对一查询 回顾

- 子查询，和方式一很像
- 联表查询 和方式二很像



# 11、一对多处理

- 比如，一个老师拥有多个学生
- 实体类

```java
@Data
public class Teacher {
    private int id;
    private String name;
    private List<Student> students;
}
```

```java
@Data
public class Student {
    private int id;
    private String name;
    private int tid;
}
```

- 接口

```java
public interface TeacherMapper {
    List<Teacher> getTeacher();
//获取指定老师下的所有学生，及老师的信息
    Teacher getTeacher(@Param("tid") int id);
    Teacher getTeacher2(@Param("tid") int id);
}
```

## 按照结果嵌套处理（推荐）

```xml
<select id="getTeacher" resultMap="TeacherStudent">
    select s.id sid,s.name sname ,t.name t_name ,t.id tid
    from mybatis.student s, mybatis.teacher t
    where s.tid=t.id
    and t.id=#{tid};
</select>
<resultMap id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="t_name"/>
    <collection property="students" ofType="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>
```



## 按照嵌套查询处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
    select * from mybatis.teacher where id=#{tid}
</select>
<resultMap id="TeacherStudent2" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentsByTeacherId2" column="id"/>
</resultMap>
<select id="getStudentsByTeacherId2" resultType="Student">
    select * from mybatis.student where tid = #{id}
</select>
```



## 小结

1. 关联- association- 多对一
2. 集合-collection-一对多
3. javaType       , ofType
   1. javaType 用来指定实体类中属性的类型
   2. ofType 用来指定映射到List或者集合中的pojo类型，即泛型中的约束类型

**注意点：**

- 保证SQL的可读性
- 注意一对多和多对一中，属性名和字段的问题
- 如果问题不好排查，可以使用日志排查，建议使用log4j

## 面试高频

- mysql引擎
- innodb的底层原理
- 索引
- 索引优化



# 12、动态SQL

- 可以看官方文档

定义：动态SQL就是指根据不同的条件生成不同的SQL语句

```xml
如果你之前用过 JSTL 或任何基于类 XML 语言的文本处理器，你对动态 SQL 元素可能会感觉似曾相识。在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。

if
choose (when, otherwise)
trim (where, set)
foreach
```



## 准备

创建数据库

```sql
CREATE TABLE `blog`(
`id` VARCHAR(50) NOT NULL COMMENT '博客id',
`title` VARCHAR(100) NOT NULL COMMENT '博客标题',
`author` VARCHAR(30) NOT NULL COMMENT '博客作者',
`create_time` DATETIME NOT NULL COMMENT '创建时间',
`views` INT(30) NOT NULL COMMENT '浏览量'
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

新建一个mybatis-08模块	



## If

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
        and title=#{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</select>
```



## choose (when, otherwise)

- 类似 switch ...case，当满足到哪一个时，就直接返回，不用管后面的条件了

```xml
    <select id="queryBlogChoose" parameterType="map" resultType="blog">
        select * from mybatis.blog
        <where>
            <choose>
                <when test="title != null">
                    title=#{title}
                </when>
                <when test="name != null  ">
                   and author=#{author}
                </when>
                <otherwise>
                    and views=#{views}
                </otherwise>
            </choose>
        </where>
    </select>
```



## trim (where, set)

- **where**

- *where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

- ```xml
  <select id="queryBlogIf" parameterType="map" resultType="blog">
      select * from mybatis.blog
      <where>
          <if test="title != null">
              and title=#{title}
          </if>
          <if test="author != null">
              and author = #{author}
          </if>
      </where>
  </select>
  ```

- **set**

- *set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号

```xml
<update id="updateBlog" parameterType="map">
    update mybatis.blog
    <set>
        <if test="title != null">
            title=#{title},
        </if>
        <if test="author != null">
            author=#{author}
        </if>
    </set>
    where id = #{id}
</update>
```

- trim ，就是它们的父级，

==所谓的动态SQL ,本质还是SQL语句，知识我们可以在SQL层面，去执行一个逻辑代码==

## SQL片段

- 将**公共**的sql语句提出来，方便**复用**



1. 使用<sql>标签抽取公共的部分

2. 在需要使用的地方使用<include>标签引用

```xml
    <update id="updateBlog" parameterType="map">
        update mybatis.blog
        <set>
            <include refid="if-title-author"/>
        </set>
        where id = #{id}
    </update>
<sql id="if-title-author">
    <if test="title != null">
        title=#{title},
    </if>
    <if test="author != null">
        author=#{author}
    </if>
</sql>
```



注意事项：

- 最好基于单表来定义SQL字段
- 不要存在where标签



## foreach

```sql
select 
```

![image-20210309000645913](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309002045467.png)

示例：

```xml
<select id="queryForeach" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
         <foreach collection="ids" item="id" open="and (" close=")" separator="or">
             id=#{id}
         </foreach>
    </where>
</select>
```

测试：

```
    @Test
    public void testForeach(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        HashMap<Object, Object> map = new HashMap<>();
        ArrayList<Integer> ids = new ArrayList<>();
        ids.add(1);
        ids.add(2);
        map.put("ids",ids);
        List<Blog> blogs = mapper.queryForeach(map);
        for (Blog blog : blogs) {
            System.out.println(blog);
        }
        sqlSession.close();
    }
}
```

结果：sql语句

![image-20210309002045467](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309012143805.png)

==动态SQL就是在拼接SQL语句，我们只要保证SQL的正确性，按照SQL的格式，去排列组合就好了==

## 建议

- 先在Mysql中写出正确的SQL，再对应的去修改为我们的动态SQL，实现通用，即可



# 13、缓存

## 13.1 简介

1. 什么是缓存 [ Cache ]？
   - 存在内存中的临时数据。
   - 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。
2. 为什么使用缓存？

   - 减少和数据库的交互次数，减少系统开销，提高系统效率。
3. 什么样的数据能使用缓存？

   - 经常查询并且不经常改变的数据。【可以使用缓存】

## 13.2 Mybatis 缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

### 一级缓存

- 一级缓存也叫本地缓存：  SqlSession 级别的
  - 与数据库同一次会话期间查询到的数据会放在本地缓存中。
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；



测试步骤：

1. 开启日志

2. 测试在一个Sesion中查询两次**相同**记录

   ```java
       @Test
       public void test(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.queryUserById(1);
           System.out.println(user);
           User user2 = mapper.queryUserById(1);
           System.out.println(user2);
           System.out.println(user==user2);
           sqlSession.close();
       }
   ```

   

3. 查看日志输出: 可得知，两个相同的记录使用的缓存

   ![image-20210309012143805](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309021503323.png)

**缓存失效**的情况：

1. 查询不同的东西

2. 增删改操作，可能会改变原来的数据，所以，必定会刷新缓存。比如再上例两个相同操作中间再执行一个update操作，就会刷新缓存。

3. 查询不同的Mapper.xml

4. 手动清理缓存，

   ```java
   //在，user,user2之间添加手动清理缓存
   sqlSession.clearCache();
   ```

**小结**：一级缓存是默认开启的，只在一次sqlSession 中有效，也就是拿到连接到关闭连接这个区间。



### 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

**步骤：**

1. 显示的开启全局缓存，在mybatis-config.xml配置文件中

   ```xml
   <setting name="cacheEnabled" value="true"/>
   ```

2. 在要使用二级缓存的Mapper.xml中开启，：

   ```xml
   <!--    使用二级缓存-->
       <cache/>
   ```

   也可以自定义参数：

   ```xml
   <cache
     eviction="FIFO"
     flushInterval="60000"
     size="512"
     readOnly="true"/>
   ```

3. 测试

   ```java
       @Test
       public void test(){
           SqlSession sqlSession = MybatisUtils.getSqlSession();
           SqlSession sqlSession2 = MybatisUtils.getSqlSession();
   
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           User user = mapper.queryUserById(1);
           System.out.println(user);
           sqlSession.close();
   
           UserMapper mapper2 = sqlSession2.getMapper(UserMapper.class);
           User user2 = mapper2.queryUserById(1);
           System.out.println(user2);
           sqlSession2.close();
       }
   ```

   

   1. 问题:我们需要将实体类序列化！否则就会报错！

      ```
      Caused by: java.io.NotSerializableException: com.kuang.pojo.User
      ```

   

   小结：

   - 只要开启了二级缓存，在同一个Mapper下就有效
   - 所有的数据都会先放在一级缓存中；
   - 只有当会话提交，或者关闭的时候，才会提交到二级缓冲中！

## 13.3 Mybatis 缓存原理

![image-20210309021503323](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309000645913.png)



## 13.4 自定义缓存 ehcache

- 现在也不咋用 ehcache 了，主要用**redis** 做数据库缓存





# 14、mybatis 的 sql 编写注意事项

## 14.1 大于> 和小于 < 符号

```sh
第一种写法（1）：

原符号       <        <=      >       >=       &        '        "
替换符号    &lt;    &lt;=   &gt;    &gt;=   &amp;   &apos;  &quot;
例如：sql如下：
create_date_time &gt;= #{startTime} and  create_date_time &lt;= #{endTime}

第二种写法（2）：
大于等于
<![CDATA[ >= ]]>
小于等于
<![CDATA[ <= ]]>
例如：sql如下：
create_date_time <![CDATA[ >= ]]> #{startTime} and  create_date_time <![CDATA[ <= ]]> #{endTime}
```

