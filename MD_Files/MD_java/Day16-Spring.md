# 目录

[toc]





 	 Spring

导入jar包

```xml

<dependency>
    <groupId>....org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
```



#  Spring

# 1、认识Spring

## 1.1 简介

- 官网文档：https://docs.spring.io/spring-framework/docs/current/reference/html/index.html
- github 地址：https://github.com/spring-projects/spring-framework
- Spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合 了现有的技术框架
- SSM：Spring +SpringMVC + Mybatis



- maven 导包：

- 导入webmvc这个包，因为会把很多关联的包也导进来

  ```xml
  <dependency>
      <groupId>....org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.4.RELEASE</version>
  </dependency>
  
  <dependency>
      <groupId>....org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.4.RELEASE</version>
  </dependency>
  ```

## 1.2 优点

- Spring是一个开源的免费的框架（容器）

- Spring是一个轻量级的、非入侵式的框架
- 面试核心：控制反转（IOC），面向切面编程(AOP)
- 支持事务的处理，对框架整合的支持

总结：Spring就是一个轻量级的控制反转（ioc）和面向切面编程（AOP)的框架。



## 1.3 组成

![image-20210309130621114](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309125716203.png)

## 1.4 拓展

![image-20210309125716203](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309134657829.png)

- 现代化的java开发，就是基于：Spring的开发
- SpringBoot :
  - 一个快速开发的脚手架
  - 基于SpringBoot 可以快速开发单个微服务
  - 约定大于配置
- SpringCloud：
  - 基于SpringBoot 实现的
- 现在大多数公司都在用SpringBoot进行开发，学习SpringBoot的前提，是完全掌握Spring 和SpringMVC
- Spring现在的弊端：发展了太久，违背了原来的理念，配置十分繁琐：人称“配置地狱”。
  - 所以SpringBoot出现 了



# 2、IOC

- Inversion of Control 是一种**设计思想**

## 2.1 IOC 理论推导

- 之前是程序员主动创建对象，控制权在程序员手中
- 使用了set注入后，程序不再具有主动性，而变成了被动的接收对象

原本的程序: 在service层调用dao 层

```java
 private UserDao userDao = new UserDaoImpl() ;

```

使用一个set接口实现后，就可以直接在测试中，由用户选择用哪种对象

```java
private UserDao userDao;
public void setUserDao(UserDao userDao){
    this.userDao = useDao;
}
```

- 这种思想，从本质上解决了问题。程序员不用再去管理对象的创建。系统的耦合性大大降低，可以更加专注的在业务的实现上。
- 这是IOC的原型

使用IOC前后对比

![image-20210309133810893](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309130621114.png)

## 2.2 IOC 本质

- **控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：**获得依赖对象的方式反转了**。



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KtDiaOqFy5ourlJ8FTVV2FFuYibmavlBHq9e4cDqiclpYSG8VT4EicVsnqKp65yJKQeNibsVdTiahQibJSg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- **IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

- Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

  ![image-20210309134657829](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309133810893.png)

- 采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

- **控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的==方式==。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**



# 3、helloSpring

## 编写代码

1、编写一个Hello实体类

```java
public class Hello {
   private String name;

   public String getName() {
       return name;
  }
   public void setName(String name) {
       this.name = name;
  }

   public void show(){
       System.out.println("Hello,"+ name );
  }
}
```

2、编写我们的spring文件 , 这里我们命名为beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <!--bean就是java对象 , 由Spring创建和管理-->
   <bean id="hello" class="com.kuang.pojo.Hello">
       <property name="name" value="Spring"/>
   </bean>

</beans>
```

3、我们可以去进行测试了 .

```java
@Test
public void test(){
   //解析beans.xml文件 , 生成管理相应的Bean对象
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //getBean : 参数即为spring配置文件中bean的id .
   Hello hello = (Hello) context.getBean("hello");
   hello.show();
}
```



## 思考

+ Hello 对象是谁创建的 ?  【hello 对象是由Spring创建的
+ Hello 对象的属性是怎么设置的 ?  hello 对象的属性是由Spring容器设置的

这个过程就叫控制反转 :

+ 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
+ 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的.

 IOC是一种编程思想，由主动的编程变成被动的接收

可以通过newClassPathXmlApplicationContext去浏览一下底层源码 .



## 修改案例一

我们在案例一中， 新增一个Spring配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="MysqlImpl" class="com.kuang.dao.impl.UserDaoMySqlImpl"/>
   <bean id="OracleImpl" class="com.kuang.dao.impl.UserDaoOracleImpl"/>

   <bean id="ServiceImpl" class="com.kuang.service.impl.UserServiceImpl">
       <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
       <!--引用另外一个bean , 不是用value 而是用 ref-->
       <!-- value:具体的值，基本类型 ；ref:Spring创建的对象-->
       <property name="userDao" ref="OracleImpl"/>
   </bean>

</beans>
```

测试！

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl");
   serviceImpl.getUser();
}
```

- OK , 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : **对象由Spring 来创建 , 管理 , 装配 !** 



# 4、IOC创建对象的方式

1. 使用无参构造创建对象，默认。

2. 要使用有参构造创建对象 ，有三种方式

> 通过有参构造方法来创建

1、UserT . java

```java
public class UserT {

   private String name;

   public UserT(String name) {
       this.name = name;
  }

   public void setName(String name) {
       this.name = name;
  }

   public void show(){
       System.out.println("name="+ name );
  }

}
```

2、beans.xml 有三种方式编写

```xml
<!-- 第一种根据index参数下标设置 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <!-- index指构造方法 , 下标从0开始 -->
   <constructor-arg index="0" value="kuangshen2"/>
</bean>
<!-- 第二种根据参数名字设置 ，就用这个就行 了 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <!-- name指参数名 -->
   <constructor-arg name="name" value="kuangshen2"/>
</bean>
<!-- 第三种根据参数类型设置 不推荐 -->
<bean id="userT" class="com.kuang.pojo.UserT">
   <constructor-arg type="java.lang.String" value="kuangshen2"/>
</bean>
```

**总结**：在配置文件加载的时候。其中管理的对象都已经初始化了！

# 5、Spring 配置

## 5.1 别名

- 可以用单独 的alias 标签

```xml

<alias name="user" alias="dkfdf"/>
```

- 也可以直接在<bean>标签的name赋值,且可以有多个别名，分隔符也有很多比如空格，逗号 分号等

  

## 5.2 Bean 的配置

```xml
<!--bean就是java对象,由Spring创建和管理-->

<!--
   id ：是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
   如果配置id,又配置了name,那么name是别名
   name：可以设置多个别名,可以用逗号,分号,空格隔开
   如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;

class是bean的全限定名=包名+类名
-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello">
   <property name="name" value="Spring"/>
</bean>
```

## 5.3 import

- import 一般用于团队开发，将多个配置文件，导入合并为一个
- 一般都是新建一个applicationContext.xml ，再把其它 配置文件，导入，合并为一个，之后，Spring 直接获取这个总的就好了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="beans.xml"/>
    <import resource="beans2.xml"/>

</beans>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```



# 6、DI （依赖注入）

- Dependency Injection : 对比官方文档学习

## 6.1 构造器注入

- 如前面所讲，有参无参构造器那里
- 

## 6.2 setter方式注入[重点]

- 依赖注入
  - 依赖：bean 对象的创建依赖于容器
  - 注入：bean 对象的所有属性由容器来注入 

准备：

1. 复杂类型

   ```java
   package com.joker.pojo;
   
   public class Address {
       private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   }
   
   ```

   

2. 真实对象

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> habbies;
       private Map<String,String> card;
       private Set<String > games;
       private Properties info;
       private String flag;//代表空指针
       ...
   }
   ```

3. beans.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   <bean id="student" class="com.joker.pojo.Student">
   <!--    普通值注入-->
       <property name="name" value="joker"/>
   </bean>
   
   </beans>
   ```

4. 测试

   ```java
   public class MyTest {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
           Student student =(Student) context.getBean("student");
           System.out.println(student.getName());
       }
   }
   
   ```

各种类型的注入：

```xml
    <bean id="student" class="com.joker.pojo.Student">
<!--    1. 普通值注入-->
    <property name="name" value="joker"/>
<!--        2.bean注入-->
    <property name="address" ref="address"/>
<!--        3 数组注入-->
        <property name="books">
            <array>
                <value>红楼</value>
                <value>水浒</value>
                <value>三国</value>
                <value>西游</value>
            </array>
        </property>
<!--        list-->
        <property name="habbies">
            <list>
                <value>sing</value>
            </list>
        </property>
<!--        map-->
        <property name="card">
            <map>
                <entry key="id" value="123"/>
                <entry key="bankCard" value="456"/>
            </map>
        </property>
<!--        set-->
        <property name="games">
            <set>
                <value>lol</value>
                <value>roe</value>
            </set>
        </property>
<!--        null-->
        <property name="flag">
            <null/>
        </property>
<!--        properties :也是K-V，不过value值在外面-->
        <property name="info">
            <props>
                <prop key="学号">34234242</prop>
                <prop key="姓名">joker</prop>
            </props>
        </property>
    </bean>

```



## 6.3 其它方式注入

- ​	

### p-namespace

- 设置属性值，属于setter注入
- 在applicationContext.xml里添加
  -  xmlns:p="http://www.springframework.org/schema/p"

### c-namespace

- 设置构造参数值，属于构造器注入
  - ​    xmlns:c="http://www.springframework.org/schema/c" 

![image-20210309185215090](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309185215090.png)

**注意点：**

- 两个需要导入xml约束

## 6.4 bean scrope(bean的作用域)

![image-20210309192450559](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309215617773.png)

- 重点是前两种，singleton(单例) , prototype(原型模式)

1. singleton （默认）

- 默认是singleton,可以在bean标签里显示表示，



2. prototype

- 原型模式：每次从容器中get的时候，都会产生一个新对象

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>

```

3. 其它：

- request、session、application,websocket 这些只能在web开发中使用

# 7、Bean的自动装配（autowire)

- 自动装配是Spring 满足 bean 依赖的一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性

在Spring 中有三种装配的方式：

1. 在xml中显示的配置
2. 在java 中显示的配置
3. 隐士的自动装配bean ,==**重点**==

## 7.1 测试

1. 环境搭建
   - 一个人有两个宠物



## 7.2 byName自动装配

```xml
    <bean id="cat" class="com.joker.pojo.Cat"/>
    <bean id="dog" class="com.joker.pojo.Dog"/>
<!--    autowire

byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid-->
    <bean id="person" class="com.joker.pojo.Person" autowire="byName">
        <property name="name" value="joker陈"/>
    </bean>
```

## 7.3 byType 自动装配

```xml
    <bean id="cat" class="com.joker.pojo.Cat"/>
    <bean id="dog123" class="com.joker.pojo.Dog"/>
<!--    autowire

byType:会自动在容器上下文中查找，和自己对象属性类型相同的的值对应的bean-->
    <bean id="person" class="com.joker.pojo.Person" autowire="byType">
        <property name="name" value="joker陈"/>
    </bean>
```



## 小结

- byName ：的时候需要保证所有的bean 的**id唯一**，并且，这个bean 需要和自动注入的属性的**set方**法的值一致
- byType ：的时候需要保证所有的bean 的**class唯**一，并且，这个bean 需要和自动注入的**属性的类型**一致

## 7.4 使用注解实现自动装配

使用注解准备：

- 导入约束：context 约束   
- 配置注解的支持;<context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```



### **@Autowired**

- 直接在属性上使用即可，也可以在set方法上使用
- 使用了Autowired 之后可以不用编写set方法，前提是自动装配的属性在IOC容器中存在，且名字符合byName	

**科普：**

@Nullable 标记某个字段，表示这个字段可以为空

@Autowired(required=false)  说明：false，对象可以为null；true，对象必须存对象，不能为null。

```java
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
private Cat cat;
```

#### @Qualifier

+ @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
+ @Qualifier不能单独使用。配合@Autowired使用，指定唯一的bean对象注入

测试实验步骤：

1、配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

```xml
<bean id="dog1" class="com.kuang.pojo.Dog"/>
<bean id="dog2" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>
```

2、没有加Qualifier测试，直接报错

3、在属性上添加Qualifier注解

```java
@Autowired
@Qualifier(value = "cat2")
private Cat cat;
@Autowired
@Qualifier(value = "dog2")
private Dog dog;
```



### @Resource

+ @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
+ 其次再进行默认的byName方式进行装配；
+ 如果以上都不成功，则按byType的方式自动装配。
+ 都不成功，则报异常。

实体类：

```java
public class User {
   //如果允许对象为null，设置required = false,默认为true
   @Resource(name = "cat2")
   private Cat cat;
   @Resource
   private Dog dog;
   private String str;
}
```

beans.xml

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>

<bean id="user" class="com.kuang.pojo.User"/>
```

测试：结果OK

配置文件2：beans.xml ， 删掉cat2

```xml
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
```

实体类上只保留注解

```java
@Resource
private Cat cat;
@Resource
private Dog dog;
```

结果：OK

结论：先进行byName查找，失败；再进行byType查找，成功。



### 小结-区别

@Autowired 和@Resource 的区别

- 相同点：都是用来自动装配的，都可以放在属性字段上
- 不同点：
  - @Autowired  通过byType的方式实现，而且要求，这个对象必须存在。**常用**
  - @Resource 通过byName+byType两种方式
- 执行顺序不同：
  - @Autowired   先byType
  - @Resource  先byName

# 8、使用注解开发

- Spring4之后，使用注解，必须导入AOP的包
- 使用注解，需要导入context约束



## 1. Bean的实现

### @Component

- @Component 放在类上，

我们之前都是使用 bean 的标签进行bean注入，但是实际开发中，我们一般都会使用注解！

1、配置扫描哪些包下的注解

```xml
<!--指定注解扫描包-->
<context:component-scan base-package="com.joker.pojo"/>
```

2、在指定包下编写类，增加注解

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
public class User {
   public String name = "joker";
}
```

3、测试

```java
@Test
public void test(){
   ApplicationContext applicationContext =
       new ClassPathXmlApplicationContext("beans.xml");
   User user = (User) applicationContext.getBean("user");
   System.out.println(user.name);
}
```



## 2. 属性注入

### @value

使用注解注入属性

1、可以不用提供set方法，直接在直接属性名上添加@value("值")

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
public class User {
   @Value("joker")
   // 相当于配置文件中 <property name="name" value="joker"/>
   public String name;
}
```

2、如果提供了set方法，在set方法上添加@value("值");

```java
@Component("user")
public class User {

   public String name;

   @Value("joker")
   public void setName(String name) {
       this.name = name;
  }
}	
```

## 3、衍生注解

### @Repository

### @Service

### @Controller

@Component有几个衍生注解，web开发中，会按照mvc三层架构分层：

- dao--> @Repository

- service--> @Service
- controller--> @Controller

这四个注解的功能都是一样的，都是代表将某个类注册到Spring 中，装配bean

## 4、自动装配

- 之前讲了

## 5、作用域

### @Scope("模式")

- 放在类上

- ```java
  @Component
  @Scope("prototype")
  public class User {
      @Value("joker")
      public String name;
  }
  ```

## 6 、小结-区别

xml  与 注解：

- xml ：更加万能，适用于任何场合，维护简单方便
- 注解：不是自己类使用不了，维护相对复杂

最佳实践：

- xml 用来管理bean

- 注解完成属性的注入

- 使用过程中，只需要注意一个问题，：必须让注解生效，就需要开启注解的支持

  - ```xml
    <!--    指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.joker.pojo"/>
        <context:annotation-config/>
    ```

  - 



# 9、使用Java的方式配置Spring

- 完全不使用Spring的xml配置 了
- **JavaConfig** 是Spring的一个子项目，Spring4之后，它成为 了**核心**功能

示例代码：

实体类

```java
@Component
public class User {
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }
    @Value("joker")
    public void setName(String name) {
        this.name = name;
    }
}

```

配置文件

```java
//@Configuration 代表这是一个配置类，和之前的beans.xml文件一样
@Configuration//这个也会注册到容器中，它本来就是一个@Component
@ComponentScan("com.joker.pojo")
@Import(JokerConfig2.class)
public class JokerConfig {
    //注册一个bean，相当于bean标签
    //  这个方法的名字，就相当于id属性
    //方法的放回置，就相当于class属性
    @Bean
    public User getUser(){
        return new User();//就是返回要注入到bean的对象
    }

}
```

测试文件

```java
public class MyTest {
    public static void main(String[] args) {
        //如果按照配置类的方式去做，就只能通过，AnnotationConfig 上下文来获取容器，通过配置类的class对象来加载
        ApplicationContext context = new AnnotationConfigApplicationContext(JokerConfig.class);
        User getUser =(User) context.getBean("getUser");
        System.out.println(getUser.getName());

    }
}

```



- 这种纯java的配置方式，在SpringBoot里随处可见



# 10、代理模式	

为什么学习代理模式？

- 这是Spring 的AOP底层原理，

**面试高点** SpringAOP和SpringMVC

代理模式的**分类**：

- 静态代理
- 动态代理

![image-20210309215617773](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309192450559.png)

## 10.1 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作
- 客户：访问代理对象的人



代码步骤：

1. 接口

   ```java
   //租房：抽象角色
   public interface Rent {
       public void rent();
   }
   
   ```

   

2. 真实角色

   ```java
   //房东
   public class Host implements Rent {
       @Override
       public void rent() {
           System.out.println("房东要出租房子");
       }
   }
   ```

   

3. 代理角色

   ```java
   //代理，中介
   public class Proxy implements Rent{
       private Host host;
   
       public Proxy() {
       }
   
       public Proxy(Host host) {
           this.host = host;
       }
   
       @Override
       public void rent() {
           viewHouse();
           host.rent();
           signContract();
           takeFee();
       }
       //看房
       public void  viewHouse(){
           System.out.println("中介带你看房");
       }
       //收中介费
       public void  takeFee(){
           System.out.println("收中介费");
       }
       //签合同
       public void  signContract(){
           System.out.println("签合同");
       }
   }
   
   ```

   

4. 客户端访问代理角色

   ```java
   //客户
   public class Client {
       public static void main(String[] args) {
           //房东要租房子
           Host host = new Host();
           //代理，中介帮 房东租房子，但是，代理角色一般会有一些附属操作
           Proxy proxy = new Proxy(host);
           //客户不用面对房东，直接找中介
           proxy.rent();
       }
   }
   
   ```

   

### 静态代理的**好处**：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共业务
- 公共业务就交给代理角色，实现了业务的分工
- 公共业务发生扩展的时候，方便集中管理

**缺点：**

- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率降低

### 静态代理-加深理解

示例代码;

```java
package com.joker.demo02;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

```java
package com.joker.demo02;
//改动原有的业务代码，是大忌
//真实对象
public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加");
    }

    @Override
    public void delete() {
        System.out.println("删除");
    }

    @Override
    public void update() {
        System.out.println("修改");
    }

    @Override
    public void query() {
        System.out.println("查询");
    }
}
```

- 想要添加一个查看日志功能，就是用一个代理，可以不改变原有业务代码

```java
package com.joker.demo02;

public class UserServiceProxy implements UserService{
        private UserServiceImpl userService;

    public void setUserService(UserServiceImpl userService) {
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");
        userService.add();
    }

    @Override
    public void delete() {
        log("delete");
        userService.delete();
    }

    @Override
    public void update() {
        log("update");
        userService.update();
    }

    @Override
    public void query() {
        log("query");
        userService.query();
    }
    //添加日志
    public void log(String str){
        System.out.println("使用了"+str);
    }
}
```

```java
package com.joker.demo02;

public class Client {
    public static void main(String[] args) {

        UserServiceImpl userService = new UserServiceImpl();
        UserServiceProxy proxy = new UserServiceProxy();
        proxy.setUserService(userService);
        proxy.query();
    }
}
```



**AOP** 的理解

![image-20210309223754911](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210309223754911.png)



## 10.2 动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是**动态生成**的
- 动态代理分为两大类：
  - 基于接口的动态代理：
    - 典型如：JDK动态代理-**本例使用j**dk文档	
  - 基于类的动态代理
    - 如：cglib
    - java字节码实现：javasist

- 包jdk.lang.reflect

jdk里需要了解的：Proxy 代理类 , InvocationHandler 调用处理接口

- 动态代理的**本质**，就是使用**反射**机制实现

### Proxy 与 InvocationHandler

- Proxy ：负责得到代理实例
- InvocationHandler ： 负责处理代理代码，并返回结果

**万能动态代理模板：**

动态代理类

```java
package com.joker.demo04;

import com.joker.demo03.Rent;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * 动态代理类，万能模板，
 */
//自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler {
    //被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    //生成得到代理类
    public Object getProxy() {
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }

    @Override//处理代理实例，并返回结果
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        //动态代理的本质，就是使用反射机制实现
        Object result = method.invoke(target, args);
        return result;
    }
    //假设添加一个附属功能，输出日志
    public  void log(String msg){
        System.out.println(msg);
    }
}
```

客户端使用动态代理

```java
package com.joker.demo04;

import com.joker.demo02.UserService;
import com.joker.demo02.UserServiceImpl;

public class Client {
    public static void main(String[] args) {
        //真实角色,也是唯一需要自己创建的
        UserServiceImpl userService = new UserServiceImpl();
        //代理角色，不存在，
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        pih.setTarget(userService);//设置要代理的对象
        //动态生成代理类
        UserService proxy =(UserService) pih.getProxy();

        proxy.delete();
    }
}

```



### 动态代理的好处

- 静态代理的好处，它都有
- 一个动态代理类 代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类，可以代理多个类



# 11、AOP

## 11.1 什么是AOP

- AOP（Aspect Oriented Programming）意为：面向切面编程，通过**预编译方式和运行期动态代理** 实现程序功能的**统一维护**的一种技术。 
- AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。

- 利用AOP可以对业务逻辑的各个部分进行**隔离**，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的可重用性，同时提高了开发的效率。

![图片](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/640)

## 11.2 AOP 在Spring中的作用

提供声明式事务；允许用户自定义切面

以下名词需要了解下：

+ 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
+ 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
+ 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
+ 目标（Target）：被通知对象。
+ 代理（Proxy）：向目标对象应用通知之后创建的对象。
+ 切入点（PointCut）：切面通知 执行的 “地点”的定义。
+ 连接点（JointPoint）：与切入点匹配的执行点。



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JAeTYOaaH6rZ6WmLLgwQLHVOZ1JpRb7ViaprZCRXsUbH0bZpibiaTjqib68LQHOWZicSvuU8Y1dquUVGw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:



![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JAeTYOaaH6rZ6WmLLgwQLHbAWH8haUQeJ0LVBxxX0icC5TZlBkEBGibibey7jFrCbibPzQcRhkNFcGAA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

即 Aop 在 不改变原有代码的情况下 , 去增加新的功能 .

## 11.3 使用Spring 实现AOP

【重点】使用AOP织入，需要导入一个依赖包！

```
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

### 方式一：使用Spring的API接口

- 主要是Spring API 接口实现

步骤:

1. 编写业务接口和实现类

```java
package com.joker.service;

public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void query();
}
```

```java
package com.joker.service;

public class UserServiceImpl implements UserService{
    @Override
    public void add() {
        System.out.println("增加");
    }

    @Override
    public void delete() {
        System.out.println("删除");
    }

    @Override
    public void update() {
        System.out.println("修改");
    }

    @Override
    public void query() {
        System.out.println("查询");
    }
}
```

2. 编写日志增强类，一个前置，一个后置

```java
package com.joker.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    //method: 要执行的目标对象的方法
    //args : 参数
    //target : 目标对象
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}
```

```java
package com.joker.log;
import org.springframework.aop.AfterReturningAdvice;
import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {
    //returnValue : 返回值
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName() + "方法， 返回结果为" +returnValue);
    }
}
```

3. 在spring配置文件中注册，实现aop切入，，注意导入约束

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
           https://www.springframework.org/schema/aop/spring-aop.xsd">
   <!--注册bean-->
       <bean id="userService" class="com.joker.service.UserServiceImpl"/>
       <bean id="log" class="com.joker.log.Log"/>
       <bean id="afterLog" class="com.joker.log.AfterLog"/>
   <!--方式一：使用原生的Spring api接口-->
       <aop:config>
   <!--        切入点 expression：表达式，execution(要执行的位置)-->
           <aop:pointcut id="pointcut" expression="execution(* com.joker.service.UserServiceImpl.*(..))"/>
   <!--        执行环绕增加-->
           <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
           <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
       </aop:config>
   </beans>
   ```

4. 测试

```java
public class MyTest {
    public static void main(String[] args) {

        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService =(UserService) context.getBean("userService");

        userService.query();
    }
}
```



### 方式二：自定义类实现

- 主要是切面定义

步骤：

- 目标业务类依旧是UserServiceImpl

1. 自定义类

```java
package com.joker.diy;
//方式二：自定义的一个切面类
public class DiyPointCut {
    public void before(){
        System.out.println("方法执行前前前前=========");
    }
    public void after(){
        System.out.println("方法执行后后后后=========");
    }
}
```

2. 配置文件

```xml
<!--    方式二：自定义类实现aop-->
    <bean id="diy" class="com.joker.diy.DiyPointCut"/>
    <aop:config>
<!--            自定义切面，ref : 要引用的类-->
        <aop:aspect ref="diy">
<!--          切入点-->
            <aop:pointcut id="pointcut" expression="execution(* com.joker.service.UserServiceImpl.*(..))"/>
<!--         通知-->
            <aop:before method="before" pointcut-ref="pointcut"/>
            <aop:after method="after" pointcut-ref="pointcut"/>
        </aop:aspect>
    </aop:config>
```

3. 测试，与前一致

### 方式三：使用注解实现

步骤：

1. 编写一个增强类

```java
package com.joker.diy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

//方式三：使用注解方式实现AOP
@Aspect//标注这个类是一个切面
public class AnnotationPointCut {
    @Before("execution(* com.joker.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("方法执行前前前前=========");
    }
    @After("execution(* com.joker.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("方法执行后后后后=========");
    }
    //在环绕增强中，我们可以定义一个参数，代表，我们要获取处理切入的点
    @Around("execution(* com.joker.service.UserServiceImpl.*(..))")
    public void aroud(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");
        //执行方法
        joinPoint.proceed();
        System.out.println("环绕后");
    }
}
```

2. 在Spring配置文件中，注册bean，并增加支持注解的配置

```xml
<!--    方式三-->
    <bean id="annotationPointCut" class="com.joker.diy.AnnotationPointCut"/>
<!--    开启注解支持 jdk(默认）proxy-target-class为false,如果显示设置为proxy-target-class="true",则变为cglib,没啥区别，用默认的就好-->
    <aop:aspectj-autoproxy />
```

3. 测试，与上一样





注意：

aop:aspectj-autoproxy：说明

```xml
通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了

<aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。
```



## 总结

Aop的重要性 : 很重要 . 一定要理解其中的思路 , 主要是思想的理解这一块 .

==Spring的Aop就是将公共的业务 (日志 , 安全等) 和领域业务结合起来 , 当执行领域业务时 , 将会把公共业务加进来 . 实现公共业务的重复利用 . 领域业务更纯粹 , 程序猿专注领域业务 , 其本质还是动态代理 .==



# 12、整合Mybatis

## 12.1 准备

步骤:

1. 导入相关jar包

   junit

   ```
   <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
   </dependency>
   ```

   mybatis

   ```
   <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
   </dependency>
   ```

   mysql-connector-java

   ```
   <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
   </dependency>
   ```

   spring相关

   ```
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.10.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.1.10.RELEASE</version>
   </dependency>
   ```

   aspectJ AOP 织入器

   ```
   <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
   <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
   </dependency>
   ```

   mybatis-spring整合包 【重点】:有官方文档

   ```
   <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.2</version>
   </dependency>
   ```

2. 编写配置文件

   配置Maven静态资源过滤问题！

   ```xml
   <build>
      <resources>
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

3. 测试



## 12.2 回忆 MyBatis

1. 编写实体类
2. 编写配置文件
3. UserMapper接口
4. UserMapper.xml映射文件
5. 测试

## 12.3 Mybatis-spring

- 官方文档：http://mybatis.org/spring/zh/index.html

- 有两种方式，主要看方式一，方式二本质还是方式一，只不过依靠继承一个写好的类来实现

步骤:

1. spring配置文件里
   - 编写数据源DataSource
   - sqlSessionFactory
   - sqlSessionTemplate
2. 需要给接口添加实现类
3. 将实现类，注入到Spring中
4. 测试使用

### **实体类**：

```java
package com.joker.pojo;

import lombok.Data;

@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

### **接口及映射文件**

UserMapper.java

```java
package com.joker.mapper;

import com.joker.pojo.User;

import java.util.List;

public interface UserMapper {
    public List<User> selectUser();
}
```

UserMapper.xml

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.joker.mapper.UserMapper">
    <select id="selectUser" resultType="user">
    select * from mybatis.user
    </select>
</mapper>
```

### **spring整合配置文件**

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
<!--DataSource:使用spring的数据源替换Mybatis的，c3p0,dbcp,druid
这里使用spring 提供的jdbc:-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8&amp;useUnicode=true&amp;useSSL=true"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>
<!--SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
<!--        绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/joker/mapper/*.xml"/>
    </bean>
<!--    SqlSessionTemplate: 就是原本使用的的sqlSession-->
    <!--    这里是方式一，才需要，方式二可以直接省略-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
<!--        只能用构造器注入sqlSessionFactory,因为没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.joker.pojo"/>
    </typeAliases>

</configuration>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.joker.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
<!--  整合方式二-->
    <bean id="userMapper2" class="com.joker.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
</beans>
```

### **编写实现类，注入sqlSession**

方式一:UserMapperImpl.java

```java
package com.joker.mapper;

import com.joker.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

/**
 * 整合方式一 SqlSessionTemplate
 */
public class UserMapperImpl implements UserMapper{
    //整合spring之前，所有操作由sqlSession执行，整合之后，由SqlSessionTemplate执行
    //注入
    private SqlSessionTemplate sqlSession;
    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        return mapper.selectUser();
    }
}
```

方式二：UserMapperImpl2.java

```java
/**
 * 整合方式二 SqlSessionDaoSupport
 */
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{
    @Override
    public List<User> selectUser() {
        return getSqlSession().getMapper(UserMapper.class).selectUser();
    }
}
```

### **测试:**

这里是方式二的，要测试方式一，直接，把userMapper2 改成userMapper

```java
import com.joker.mapper.UserMapper;
import com.joker.pojo.User;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.io.IOException;
import java.util.List;

public class MyTest {
    @Test
    public void test() throws IOException {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
        List<User> users = userMapper.selectUser();
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```



# 13、事务声明

## 13.1 回顾事务

+ 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
+ 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。

事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。

**事务四个属性ACID**

1. 原子性（atomicity）

2. + 事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用

3. 一致性（consistency）

4. + 一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中

5. 隔离性（isolation）

6. + 可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏

7. 持久性（durability）

8. + 事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

## 13.2 Spring中的事务管理

### 概念

Spring支持编程式事务管理和声明式的事务管理。

- 声明式事务（交由容器管理事务）： AOP
- 编程式事务：



**编程式事务管理**

+ 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
+ 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**声明式事务管理**

+ 一般情况下比编程式事务好用。
+ 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
+ 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明式事务管理。

**使用Spring管理事务，注意头文件的约束导入 : tx**

```xml
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```

**事务管理器**

+ 无论使用Spring的哪种事务管理策略（编程式或者声明式）事务管理器都是必须的。
+ 就是 Spring的核心事务管理抽象，管理封装了一组独立于技术的方法。

### 代码准备

在之前的案例中，我们给UserMapper接口新增两个方法，删除和增加用户；

```java
//添加一个用户
int addUser(User user);

//根据id删除用户
int deleteUser(int id);
```

mapper文件，我们故意把 deletes 写错，测试！

```xml
<insert id="addUser" parameterType="com.kuang.pojo.User">
insert into user (id,name,pwd) values (#{id},#{name},#{pwd})
</insert>
<!--故意写错deletes-->
<delete id="deleteUser" parameterType="int">
deletes from user where id = #{id}
</delete>
```

编写接口的实现类，在实现类中，我们去操作一波

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper{

    @Override
    public List<User> selectUser() {
        User user = new User(9,"bbababab","dfdfd");
        UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
        mapper.addUser(user);
        mapper.deleteUser(3);
        return mapper.selectUser();
    }

    @Override
    public int addUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).addUser(user);
    }

    @Override
    public int deleteUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
    }
}
```

测试

```java
public class MyTest {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        List<User> users = userMapper.selectUser();
        for (User user : users) {
            System.out.println(user);
        }
    }
}
```

报错：sql异常，delete写错了

结果 ：插入成功！

没有进行事务的管理；我们想让他们都成功才成功，有一个失败，就都失败，我们就应该需要**事务！**

以前我们都需要自己手动管理事务，十分麻烦！

但是Spring给我们提供了事务管理，我们只需要配置即可；

### 事务配置

步骤：

在spring配置文件中配置：

- **JDBC事务**
- **配置好事务管理器后我们需要去配置事务的通知**
- **配置AOP**

```xml
<!--    配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>
<!--    结合AOP实现事务的织入-->
<!--    配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
<!--        给哪些方法配置事务-->
<!--        配置事务的传播特性 propagation:默认是REQUIRED(不用显示表示就是它)-->
<!--        其实只用写 * -->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete"/>
            <tx:method name="query"/>
            <tx:method name="update"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>
<!--    配置事务织入 AOP-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.joker.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
```

测试：,就不会插入了，



注意：

**spring事务传播特性：**看看就好

事务传播行为就是多个事务方法相互调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

+ propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
+ propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
+ propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
+ propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
+ propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
+ propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
+ propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

假设 ServiveX#methodX() 都工作在事务环境下（即都被 Spring 事务增强了），假设程序中存在如下的调用链：Service1#method1()->Service2#method2()->Service3#method3()，那么这 3 个服务类的 3 个方法通过 Spring 的事务传播机制都工作在同一个事务中。

就好比，我们刚才的几个方法存在调用，所以会被放在一组事务当中！



> 思考问题？

为什么需要配置事务？

+ 如果不配置，就需要我们手动提交控制事务；
+ 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！





# 14、Spring 相关补充

## 14.1 @Primary

参考博客：https://www.jianshu.com/p/a42f3c835b20



- 作用：当你一个接口的实现类有**多个**的时候，你通过`@Component`来注册你的实现类有多个，但是在注入的时候使用`@Autowired`，对应实现类 加上 `@Primary` 注解，表示，优先使用这个实现类的 bean
- 通常雨 `@Component` 或 `@Bean` 结合使用。
- 相同类型被`@Primary`声明的Bean最多只能有一个。

使用示例：

```java
public interface Worker {
    public String work();
}

@Component
public class Singer implements Worker {
    @Override
    public String work() {
        return "歌手的工作是唱歌";
    }
}

@Component
public class Doctor implements Worker {
    @Override
    public String work() {
        return "医生工作是治病";
    }
}

// 启动，调用接口
@SpringBootApplication
@RestController
public class SimpleWebTestApplication {

    @Autowired
    private Worker worker;

    @RequestMapping("/info")
    public String getInfo(){
        return worker.work();
    }

    public static void main(String[] args) {
        SpringApplication.run(SimpleWebTestApplication.class, args);
    }

}
```

当一个接口有多个实现，且通过@Autowired注入属性，由于@Autowired是通过byType形式，用来给指定的字段或方法注入所需的外部资源。

Spring无法确定具体注入的类（有多个实现，不知道选哪个），启动会报错并提示：
 Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed。

当给指定的组件添加@Primary是，默认会注入@Primary配置的组件。

```java
@Component
@Primary
public class Doctor implements Worker {
    @Override
    public String work() {
        return "医生工作是治病";
    }
}
```



### @Primary 对比 @Qualifier

当一个接口有多个实现类时，注入这个接口的 bean 时，Spring就不知道你注入哪个，那现在就可以通过下面两个办法解决：

+ `@Primary` 优先考虑，优先考虑被注解的对象注入
+ `@Qualifier` 名字声明，声明后对名字（bean 名，一般就是类名小写首字母）进行使用



# THE END