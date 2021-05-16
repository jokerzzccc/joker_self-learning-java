# SSM 整合

# 1、准备

## 1.1 环境

- idea 2020.3.2
- mysql 5.7.19
- maven 3.6.3
- tomcat 9
- jdk 1.8



## 1.2 项目搭建-模板

- 对应项目，SSMBuild  日后作为一个SSM初始模板

### 1.2.1 基本结构搭建

1. 数据库建表

```sql
CREATE DATABASE `ssmbuild`;

USE `ssmbuild`;

DROP TABLE IF EXISTS `books`;

CREATE TABLE `books` (
`bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID` (`bookID`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT  INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```



2. 新建一个maven web项目，

3. 导入依赖

   ```xml
   <dependencies>
     <!--Junit-->
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
     </dependency>
     <!--数据库驱动-->
     <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.47</version>
     </dependency>
     <!-- 数据库连接池 -->
     <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
     <dependency>
       <groupId>com.mchange</groupId>
       <artifactId>c3p0</artifactId>
       <version>0.9.5.5</version>
     </dependency>
   
   
     <!--Servlet - JSP -->
     <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>servlet-api</artifactId>
       <version>2.5</version>
     </dependency>
     <dependency>
       <groupId>javax.servlet.jsp</groupId>
       <artifactId>jsp-api</artifactId>
       <version>2.2</version>
     </dependency>
     <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
     </dependency>
   
     <!--Mybatis-->
     <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.6</version>
     </dependency>
     <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis-spring</artifactId>
       <version>2.0.6</version>
     </dependency>
   
     <!--Spring-->
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.2.4.RELEASE</version>
     </dependency>
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.2.4.RELEASE</version>
     </dependency>
   
     <!--        lombok-->
     <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.8</version>
     </dependency>
   </dependencies>
   ```

4. Maven资源过滤设置

   ```xml
   <build>
     <resources>
       <resource>
         <directory>src/main/java</directory>
         <includes>
           <include>**/*.properties</include>
           <include>**/*.xml</include>
         </includes>
         <filtering>false</filtering>
       </resource>
       <resource>
         <directory>src/main/resources</directory>
         <includes>
           <include>**/*.properties</include>
           <include>**/*.xml</include>
         </includes>
         <filtering>false</filtering>
       </resource>
     </resources>
   </build>
   ```

5. 建立基本结构和配置框架！

   - com.joker.pojo

   - com.joker.dao

   - com.joker.service

   - com.joker.controller

   - mybatis-config.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
     
     </configuration>
     ```

   - applicationContext.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
     
     </beans>
     ```

### 1.2.2 Mybatis 层 编写

1. 数据库配置文件 **database.properties**

```properties
jdbc.driver=com.mysql.jdbc.Driver
#如果使用mysql8.0+，需要增加时区的的配置  &serverTimezone=Asia/Shanghai
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123456
```

2. 编写mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    配置数据源，交给spring-->
    <typeAliases>
        <package name="com.joker.pojo"/>
    </typeAliases>

    <mappers>
        <mapper resource="com/joker/dao/BookMapper.xml"/>
    </mappers>
</configuration>
```

3. 编写数据库对应的实体类 com.kuang.pojo.Books

   使用lombok插件！,添加lombok依赖，

   ```java
   package com.joker.pojo;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Books {
       private int bookID;
       private int booCounts;
       private String bookName;
       private String detail;
   }
   
   ```

4. 编写Dao层的 Mapper接口！

   ```java
   package com.joker.dao;
   
   import com.joker.pojo.Books;
   import org.apache.ibatis.annotations.Param;
   
   import java.util.List;
   
   public interface BookMapper {
       //增加一本书
       public int addBook(Books book);
       //删除一本书
       public int deleteBookById(@Param("bookId") int id);
       //更新一本书
       public int updateBook(Books book);
       //查询一本书
       public Books queryBookById(@Param("bookId") int id);
       //查询全部的书
       public List<Books> queryAllBook();
   
   }
   ```

5. 编写接口对应的 Mapper.xml 文件。需要导入MyBatis的包；

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.joker.dao.BookMapper">
       <insert id="addBook" parameterType="Books">
           insert into ssmbuild.books values(#{bookName},#{bookCounts},#{detail})
       </insert>
       <delete id="deleteBookById" >
           delete from ssmbuild.books where bookID=#{bookId}
       </delete>
       <update id="updateBook" parameterType="Books">
           update ssmbuild.books
           set bookName=#{bookName} , bookCounts = #{bookCounts},detail=#{detail}
           where bookID=#{bookID}
       </update>
       <select id="queryBookById" resultType="Books">
           select * from ssmbuild.books
           where bookID=#{bookId}
       </select>
       <select id="queryAllBook" resultType="Books">
           select * from ssmbuild.books
       </select>
   </mapper>
   ```

6. 编写Service层的接口和实现类

   ```java
   package com.joker.service;
   
   import com.joker.pojo.Books;
   
   import java.util.List;
   
   public interface BookService {
       //增加一本书
       public int addBook(Books book);
       //删除一本书
       public int deleteBookById(int id);
       //更新一本书
       public int updateBook(Books book);
       //查询一本书
       public Books queryBookById(int id);
       //查询全部的书
       public List<Books> queryAllBook();
   
   }
   ```

   ```java
   package com.joker.service;
   
   import com.joker.dao.BookMapper;
   import com.joker.pojo.Books;
   
   import java.util.List;
   
   public class BookServiceImpl implements BookService{
       //service层调用dao层，组合Dao
       private BookMapper bookMapper;
       public void setBookMapper(BookMapper bookMapper) {
           this.bookMapper = bookMapper;
       }
   
       @Override
       public int addBook(Books book) {
           return bookMapper.addBook(book);
       }
   
       @Override
       public int deleteBookById(int id) {
           return bookMapper.deleteBookById(id);
       }
   
       @Override
       public int updateBook(Books book) {
           return bookMapper.updateBook(book);
       }
   
       @Override
       public Books queryBookById(int id) {
           return bookMapper.queryBookById(id);
       }
   
       @Override
       public List<Books> queryAllBook() {
           return bookMapper.queryAllBook();
       }
   }
   ```

### 1.2.3 Spring 层编写

1. 配置**Spring整合MyBatis**，我们这里数据源使用c3p0连接池；

2. 我们去编写Spring**整合Mybatis**的相关的配置文件；spring-dao.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
   
   <!--    1 关联数据库配置文件-->
       <context:property-placeholder location="classpath:database.properties"/>
   <!--    2 连接池 有哪些：
   dbcp 半自动化操作，不能自动连接
    c3p0 自动化操作，自动化的加载配置文件，并且可以自动设置到对象中
    druid hikari：后面springboot再学
    ...-->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jdbc.driver}"/>
           <property name="jdbcUrl" value="${jdbc.url}"/>
           <property name="user" value="${jdbc.username}"/>
           <property name="password" value="${jdbc.password}"/>
           
       </bean>
   <!--    3 sqlSessionFactory   -->
       <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <property name="dataSource" ref="dataSource"/>
   <!--        绑定mybatis配置文件-->
           <property name="configLocation" value="classpath:mybatis-config.xml"/>
       </bean>
   <!--    4 配置dao接口扫描包，动态的实现 了Dao接口可以注入到Spring容器 中,
   配置后，就不用再写dao接口的实现类了
   -->
       <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
   <!--        注入 sqlSessionFactory-->
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
   <!--        要扫描的dao包-->
           <property name="basePackage" value="com.joker.dao"/>
       </bean>
   </beans>
   ```

3. **Spring整合service层**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
   
   
   <!--        1. 扫描service下的包-->
       <context:component-scan base-package="com.joker.service"/>
   
   <!--    2. 将所有的业务类，注入到spring 。可以通过配置 ，或者注解实现-->
       <bean id="BookServiceImpl" class="com.joker.service.BookServiceImpl">
           <property name="bookMapper" ref="bookMapper"/>
       </bean>
   <!--    3. 声明式事务配置-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   <!--        4. 注入数据源-->
           <property name="dataSource" ref="dataSource"/>
       </bean>
   <!--    4. aop事务支持 ,要导入aspectjweaver 依赖-->
   </beans>
   ```



### 1.2.3 SpringMVC 层编写

1. 编写 web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
            http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
     <!--    DispatcherServlet-->
     <servlet>
       <servlet-name>springmvc</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:applicationContext.xml</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>springmvc</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   
     <!--    乱码过滤-->
     <filter>
       <filter-name>encodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
         <param-name>encoding</param-name>
         <param-value>utf-8</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>encodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   
     <!--    Session-->
     <session-config>
       <session-timeout>15</session-timeout>
     </session-config>
   </web-app>
   ```

2. **spring-mvc.xml**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
          http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--    1. 注解驱动-->
       <mvc:annotation-driven/>
       <!--    2. 静态资源过滤-->
       <mvc:default-servlet-handler/>
       <!--    3 扫描包：controller    -->
       <context:component-scan base-package="com.joker.controller"/>
       <!--    4 视图解析器-->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

3. **Spring配置整合文件，applicationContext.xml**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <import resource="classpath:spring-dao.xml"/>
       <import resource="classpath:spring-service.xml"/>
       <import resource="classpath:spring-mvc.xml"/>
   
   
   </beans>
   ```

项目目录如图：

![image-20210311165516556](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210311165516556.png)

- 基本配置就完成了，就剩下，Controller 和 视图层的jsp **编写测试**了



## 1.3 测试

- 



