# SpringBoot

# 1 SpringBoot

## 回顾什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson  。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

## Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；



## 什么是SpringBoot

什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，

官方说是简化开发，约定大于配置，  you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。

Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

**Spring Boot的主要优点：**

+ 为所有Spring开发者更快的入门
+ **开箱即用**，提供各种默认配置来简化项目配置
+ 内嵌式容器简化Web项目
+ 没有冗余代码生成和XML配置的要求



# 2 微服务阶段

MVC 三层架构

MVVM 微服务架构

微服务就是：

业务：service：UserService==》模块

springMVC,controller ===>提供接口

什么是微服务？

- 微服务是一种风格，它要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过	http的方式，进行互通。要说微服务架构，先要了解过去的单体架构

微服务架构了解：https://martinfowler.com/articles/microservices.html

![image-20210318131630716](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318131630716.png)

![image-20210318131720384](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318131720384.png)



![image-20210318131818617](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318131818617.png)

![image-20210318132644484](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210312163044232.png)

# 3 第一个springboot程序

环境：

- jdk1.8
- maven3.6.3
- springboot:最新
- IDEA

- 

**注意** 项目的包必须放在与自动生成的文件，放在同一目录下。

Springboot01HelloworldApplication.java

## 创建基础项目说明

Spring官方提供了非常方便的工具让我们快速构建应用

Spring Initializr：https://start.spring.io/

**项目创建方式一：**使用Spring Initializr 的 Web页面创建项目

1、打开  https://start.spring.io/

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。



**项目创建方式二：**使用 IDEA 直接创建项目

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功



**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml



## pom.xml 分析

打开pom.xml，看看Spring Boot项目的依赖：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
<!--    父依赖-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.joker</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-01-helloworld</name>
    <description>firstproject for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
<!--        启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
<!--        web场景启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.4.3</version>
        </dependency>
        <!-- springboot单元测试 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- 打包插件 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.4.3</version>
            </plugin>
        </plugins>
    </build>

</project>
```

- **遇到报错** spring-boot-maven-plugin不存在。本来是不用添加版本号的，因为父依赖有。
  - 解决：添加版本号即可，其它不存在，也可以添加版本号，看是否可以解决



## 编写一个http接口

1、在主程序的同级目录下，新建一个controller包，**一定要在同级目录下，否则识别不到**

2、在包中新建一个HelloController类

```java
package com.joker.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/hello")
public class HelloController {
    @GetMapping("hello")
    @ResponseBody
    public String hello(){
        return "hello";
    }

}
```

简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的**微服务项目**！

## 项目打包

将项目打成jar包，点击 maven的 package，

打包成功，则会在target目录下生成一个 jar 包

打成了jar包后，就可以在任何地方运行了！OK



彩蛋：

可以自己设置web的banner, 直接百度，springboot banner ,复制一个banner , 在resources目录下新建一个banner.txt，就可以了



# 4、原理探究

## 4.1**自动配置：**



**pom.xml**

- spring-boot-dependencies : 核心依赖在父工程中
- 引入springboot依赖的时候，不需要些版本，就是因为有这些版本仓库

**启动器：**

```xml
<!--        启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
```

- 启动器，就是springboot的启动场景
- 比如 ： spring-boot-starter-web ，它会自动导入，web环境的所有依赖
- springboot会将所有的功能场景，都变成一个个的启动器。
- 我们要使用什么功能，就只需要找到对应的启动器，

**主程序：**

```java
@SpringBootApplication//标注这个类是一个springboot应用，	
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        //将springboot应用启动
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}
```

- 注解;

- @SpringBootApplication

  - ```java
    @SpringBootConfiguration springboot的配置
    	@Configuration   spring配置类
    		@Component	说明这也是一个Spring的组件
    @EnableAutoConfiguration	自动配置
    	@AutoConfigurationPackage	自动配置包
    		@Import({Registrar.class})	导入，自动配置包注册类
    			
    	@Import({AutoConfigurationImportSelector.class})	 自动配置导入选择器
    //AutoConfigurationImportSelector 里面的
    //    获取所有的配置    
    List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
    //获取候选的配置
        protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
            List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
            Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
            return configurations;
        }
    
    
    ```

自动配置的核心文件： META-INF/spring.factories

![image-20210312163044232](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210312205600396.png)



## 4.2 **结论：**

springboot 所有自动配置 都是在启动的时候 扫描并加载 ; 

所有的自动配置类都在 spring.factories ，但是不一定生效，要判端条件是否成立; 

只要导入了对应的starter , 就有对应的启动器了；

有了启动器，自动装配就会生效，然后配置成功。



Springboot 注解执行原理执行结论：

1. SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值
2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
3. 整个J2EE的整体解决方案和自动配置都在spring-boot-autoconfigure-2.4.3.jar的jar包中；它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器，
4. 容器中，也会存在非常多的xxxAutoConfiguration的文件（@Bean）,就是这些类，给容器中导入了这个场景所需要的所有租价，并自动配置，@Configuration ，就是以前spring的JavaConfig
5. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；



**run方法**



**面试：关于Springboot 谈谈你的理解？**

- 自动装配，
- run（） ,这里简略说说



# 5、YAML

- YAML Ain't Markup Language

可以把原有的配置文件 application.properties文件 删除，改为，application.yaml文件

- 普通的properties文件只能保存键值对，
- yaml文件的强大在于，可以**注入到配置类**
- yaml 可以直接给实体类赋值 
- yaml 还可以使用占位符
- 当然，yaml 也可以给所有类赋值，不仅只有实体类

yaml的基本语法

```yaml
#普通的key-value：
#对空格的要求十分严格
# key:空格value
name: joker

# 对象
student:
  name: joker
  age: 3
#对象行内写法
teacher: {name: joker,age: 3}

#数组
toys:
  - cat
  - dog
  - pig

#数组行内写法
pets: [cat,dog,pig]
```



YAML 结合注解，**给实体类赋值**

- 使用注解：@ConfigurationProperties(prefix = "person")

- 这时候要引入一个依赖,不然会报红，虽然没啥影响

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
  </dependency>
  ```

实体类

```java
package com.joker.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;

@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties(prefix = "person")//绑定yaml里的person
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

}
```

application.yaml

```yaml
#yaml里还可以配置占位符${}
person:
  name: joker${random.uuid}
  age: 3
  happy: true
  birth: 2020/2/1
  maps: {k1: v1,k2: v2}
  xxx: xxx
  lists:
    - code
    - music
    - basketball
  dog:
  #    ${person.xxx:hello} 表示，如果前者存在，就用前者，不存在就用后者
    name: ${person.xxx:hello}_monster
    age: 2

```



# 6、JSR303 校验

## 6.1 使用

- 拓展-了解即可
- 导入依赖

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
```

Springboot中可以用 @validated来 校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。

我们这里来写个注解让我们的name只能支持Email格式；message,可以自定义输出

```java
@Validated//数据校验
public class Person {
    @Email(message = "邮箱错了")
    private String name;
```

运行结果 ：default message [不是一个合法的电子邮件地址];



**使用数据校验，可以保证数据的正确性；** 



## 6.2 常见参数

```xml
空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

![image-20210312205600396](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210312213006481.png)

# 7、多环境切换

## 多配置文件

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

**例如：**

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

## yaml的多文档块

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件

使用一个文件，就可以配置多个环境端口

```yml
server:
  port: 8080
spring:
  profiles:
    active: test

---
server:
  port: 8081
spring:
  config:
    activate:
      on-profile: test
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**



## 配置文件加载位置

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

官方外部配置文件说明参考文档: 从高到低

![image-20210312213006481](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313030908391.png)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

## 拓展，运维小技巧

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

```
java -jar spring-boot-config.jar --spring.config.location=F:/application.properties
```



# 8、自动配置原理

- 分析，在配置文件application.properties中，的自动配置原理

## 分析自动配置原理

我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

- 注意：这里的 HttpEncodingAutoConfiguration已经过期，如今版本代替是

```java

//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

+ 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
+ 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
+ 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
+ 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {    
// .....
}
```

**这就是自动装配的原理！**

## 精髓-总结

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**



## 了解：@Conditional

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![image-20210312221026767](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210312221026767.png)

**那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**

我们怎么知道哪些自动配置类生效？

**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**



```properties
#开启springboot的调试类
debug=true
```

【演示：查看输出的日志】,分一下三类

**Positive matches:（自动配置类启用的：正匹配）**

**Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**

**Unconditional classes: （没有条件的类）**



# 9、springboot web 开发

jar: webapp

自动装配

spring，到底帮我们配置了什么，我们能不能修改？能修改哪些东西？能不能扩展？

- xxxAutoConfiguration 向容器中，自动配置组件
- xxxProperties: 自动配置类，装配 配置文件中自定义的一些内容



要解决的问题：

- 导入静态资源。。。html,css....
- 首页
- jsp(springboot里没地方写jsp) , 所以学习-》模板引擎Thymeleaf
- 装配扩展SpringMVC
- 增删改查
- 拦截器
- 国际化（中英文切换）



## 静态资源

源码分析

- ```
  
  WebMvcAutoConfiguration.java--->EnableWebMvcConfiguration-->addResourceHandlers()
  ```

```java
		protected void addResourceHandlers(ResourceHandlerRegistry registry) {
			super.addResourceHandlers(registry);
            //方式一：自定义静态资源路径
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
            //路径二 webjars
			ServletContext servletContext = getServletContext();
			addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
            //路径三 静态资源默认扫描路径 staticLocations
			addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
				registration.addResourceLocations(this.resourceProperties.getStaticLocations());
				if (servletContext != null) {
					registration.addResourceLocations(new ServletContextResource(servletContext, SERVLET_LOCATION));
				}
			});
		}

```

getStaticLocations-->staticLocations--> CLASSPATH_RESOURCE_LOCATIONS = 

```java
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
      "classpath:/resources/", "classpath:/static/", "classpath:/public/" };
```

```java
webmvcProperties
/**
 * Path pattern used for static resources 默认的静态资源路径，
 */
private String staticPathPattern = "/**";
```



==**总结：**==

在springboot 里，使用如下方式处理静态资源,扫描如下路径

- webjars  访问路径 localhost:8080/webjars/...   基本不用
  - 百度，webjars,添加maven依赖，就可以在上述路径找到资源
- { "classpath:/META-INF/resources/",
        "classpath:/resources/", "classpath:/static/", "classpath:/public/" } 访问路径 localhost:8080/...
  - 路径优先级：resources > static（默认） > public
  - classpath 就是在原本resources 目录下建立这些目录,比如，root/resources/resources , root/resources/static



## 首页

WebMvcAutoConfiguration.java---->welcomePageHandlerMapping()

首页 ，系统自动扫描，静态资源路径，寻找index.html,

所以一般首页放在static里



## 模板引擎 - 

现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，**他现在默认是不支持jsp的**。

模板引擎的**作用**就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去

模板殷勤的**思想**

![image-20210313030908391](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313032008463.png)



- 模板引擎有很多，thymeleaf，是一种

### thymeleaf

Thymeleaf 官网：https://www.thymeleaf.org/

Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

Spring官方文档：找到我们对应的版本

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter 

导入依赖：也要看官网对应的依赖

```xml
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
        </dependency>
```

- 标准语法，使用，可以看官网

- 原本是用jsp,现在使用thymeleaf

**结论**：只要需要使用 thymeleaf ，就导入对应的依赖，将htmL放在templates目录下即可

**原理分析**

查看：Thymeleaf的自动配置类：ThymeleafProperties

```java
public static final String DEFAULT_PREFIX = "classpath:/templates/";    
public static final String DEFAULT_SUFFIX = ".html";
```

**连接测试**

1. 建一个cotroller

   ```java
   //templates 访问，必须通过controller跳转
   //需要thymeleaf引擎
   @Controller
   public class IndexController {
       @RequestMapping("/test")
       public String test(Model model){
           model.addAttribute("msg","hello,thymeleaf");
           return "test";
       }
   }
   ```

2. 模板文件在 templates 目录下，test.html ,html 标签要添加命名空间的约束  xmlns:th="http://www.thymeleaf.org"

   ```xml
   <html xmlns:th="http://www.thymeleaf.org">
   <meta charset="UTF-8"/>
   <title>title</title>
   <head>
       <body>
   <!--    所有的html元素，都可以thymeleaf 替换接管，th:元素名-->
   <div th:text="${msg}"></div>
       </body>
   </head>
   </html>
   ```

   

3. 测试 localhost:8080/test



**thymeleaf 标准表达式语法：**

```xml
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================

  Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
  Message Expressions: #{...}：获取国际化内容
  Link URL Expressions: @{...}：定义URL；
  Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…
      Number literals: 0 , 34 , 3.0 , 12.3 ,…
      Boolean literals: true , false
      Null literal: null
      Literal tokens: one , sometext , main ,…
      
Text operations:（文本操作）
    String concatenation: +
    Literal substitutions: |The name is ${name}|
    
Arithmetic operations:（数学运算）
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
    
Boolean operations:（布尔运算）
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
    
Comparisons and equality:（比较运算）
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
    
Conditional operators:条件运算（三元运算符）
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
    
Special tokens:
    No-Operation: _
```





**我们可以使用任意的 th:attr 来替换Html中原生属性的值！**

属性值作用，和**优先级**如下图

![image-20210313032008463](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210316185829324.png)





# 10、springMVC 配置

mvc扩展：

官网：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration

示例一：

```java
//如果你想定制化一些功能，只要写这个组件，然后将它交给springboot ,  它就会帮我们自动装配
//扩展springmvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }

    //public interface ViewResolver 实现了视图解析器接口的类，就可以看作视图解析器
//定义了一个自己的视图解析器
    public static class MyViewResolver implements ViewResolver{
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}
```

示例二：

```java
//如果我们要扩展springmvc,官方建议我们，用这种方式，@Configuration
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    //视图跳转的方法
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/joker").setViewName("test");
    }
}
```

**@EnableWebMvc**  全面接管

在自定义一个springmvc配置的时候，不能加 @EnableWebMvc 的原因是：

- 它导入了一个类 DelegatingWebMvcConfiguration --》继承了	WebMvcConfigurationSupport

- 而 WebMvcAutoConfiguration 自动配置类上有一个注解，表示，有了 WebMvcConfigurationSupport 这个类，整个自动配置就会失效

- ```java
  @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
  public class WebMvcAutoConfiguration {
  ```

  



**总结**：

- springboot中，有很多的xxxConfiguration 帮助我们进行扩展配置，

## 修改SpringBoot的默认配置

这么多的自动配置，原理都是一样的，通过这个WebMVC的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。

SpringBoot的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；

如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！

**扩展使用SpringMVC**  官方文档如下：

If you want to keep Spring Boot MVC features and you want to add additional MVC configuration (interceptors, formatters, view controllers, and other features), you can add your own @Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

**操作总结**

我们要做的就是编写一个@Configuration注解类，并且类型要为WebMvcConfigurer，还不能标注@EnableWebMvc注解；我们去自己写一个；我们新建一个包叫config，写一个类MyMvcConfig；



# 11、web项目

- 导入bootstrap模板 ,静态资源



## 11.1 首页配置

- 注意：所有页面的静态资源都需要使用，模板引擎 thmeleaf
- 对照thmeleaf语法，更改 html文件里静态资源的路径
- url语法：@{}

## 11.2 页面国际化

- 确保，settings|editor|code Style|file encodings，编码是utf-8
- 自动配置类，MessageSourceAutoConfiguration
- 封装的文件类 MessageSourceProperties

- 源码

- ```java
  AcceptHeaderLocaleResolver
      
      
     public Locale resolveLocale(HttpServletRequest request) {
          Locale defaultLocale = this.getDefaultLocale();
          if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
              return defaultLocale;
          } else {
              Locale requestLocale = request.getLocale();
              List<Locale> supportedLocales = this.getSupportedLocales();
              if (!supportedLocales.isEmpty() && !supportedLocales.contains(requestLocale)) {
                  Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
                  if (supportedLocale != null) {
                      return supportedLocale;
                  } else {
                      return defaultLocale != null ? defaultLocale : requestLocale;
                  }
              } else {
                  return requestLocale;
              }
          }
      }
      
  
  
  ```

- 



国际化**操作总结**：

1. 需要配置 i18n 文件
2. 如果我们需要在项目中，进行按钮自行切换 ，我们需要自定义一个组件 实现 LocaleResolver接口
3. 自己的组件要添加@Bean,配置到spring容器
4. message语法：#{}



代码示例

1. index.html

```html
			<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
			<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

2. MyLocaleResolver.java

```java
package com.joker.config;

import org.springframework.util.StringUtils;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

public class MyLocaleResolver implements LocaleResolver {
  //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        String language = httpServletRequest.getParameter("l");
        Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
        //如果请求链接不为空
        if (!StringUtils.isEmpty(language)){
            //分割请求参数
            String[] split = language.split("_");
            //国家，地区
            locale = new Locale(split[0],split[1]);
        }
        return locale;
    }
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}
```

3. 添加组件

   ```java
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
       //自定义的国际化
       @Bean
       public LocaleResolver localeResolver(){
           return new MyLocaleResolver();
       }
   }
   ```

## 11.3 登录页面+拦截器



## 11.4 CRUD

- 员工列表展示
  - 提取公共页面，
    - th:fragment="sidebar"
    - th:replace="~{commons/commons::topbar}"
    - 如果要传递参数，可以直接使用()接收即可，接收用三元表达式判断即可
  - 列表展示



## 11.5 404



# 12 如何开发一个网站

1、前端：

- 模板： 直接百度，别人写好的，拿来改成自己需要的
- 框架 ：组件，：自己手动组合拼接 Bootstrap,layUI,semantic ，EasyUI.......
  - 栅格系统
  - 导航栏
  - 侧边栏
  - 表单

2、设计数据库，（难点，）

3、同时前端能够让它自动运行，独立化工程

4、数据接口如何对接：json ; 不会json的话，直接手动对接，all in one

5、前后端联调测试



所以：**总结** ，**开发一个网站**

1. 有一套自己熟悉的**后台模板**，工作必要：比如 X-admin
2. 前端界面：至少能够通过前端框架，组合出来一个网站页面
   1. index 首页
   2. about 关于
   3. blog
   4. post 提交页面
   5. user 用户页面
3. 让这个网站跑起来，



# 13、整合JDBC

## SpringData

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。

Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。

Sping Data 官网：https://spring.io/projects/spring-data

数据库相关的启动器 ：可以参考官方文档：

https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter



## 整合JDBC

- 官网：https://spring.io/projects/spring-data-jdbc#learn
-  也有JdbcProperties , JdbcTemplateAutoConfiguration
- xxxTemplate,都可以拿来直接用

1. 建项目的时候，选中Spring web , JDBC API , MYSQL Driver,

2. 配置文件写，数据源 

   ```yaml
   spring:
     datasource:
       username: root
       password: 123456
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characeterEncoding=utf-8&serverTimezone=UTC
   ```

3. 可以得知，默认的数据源是 hikari

   ```java
   @Test
   void contextLoads() throws SQLException {
       System.out.println(dataSource.getClass());
       Connection connection = dataSource.getConnection();
       System.out.println(connection);
   
       connection.close();
   
   }
   ```

4. 数据库的 CRUD,新建一个Controller

   ```java
   @RestController
   public class JdbcController {
       final JdbcTemplate jdbcTemplate;
       public JdbcController(JdbcTemplate jdbcTemplate) {
           this.jdbcTemplate = jdbcTemplate;
       }
   
       @GetMapping("/userList")
       public List<Map<String, Object>> userList(){
           String sql ="select * from mybatis.user";
           List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
   
           return maps;
       }
       @GetMapping("/addUser")
       public  String addUser(){
           String sql="insert into mybatis.user values(10,'小明','111111')";
       int update = jdbcTemplate.update(sql);
       return "insert-ok";
   }
       @GetMapping("/updateUser/{id}")
       public String updateUser(@PathVariable("id") int id){
           String sql="update mybatis.user set name=? ,pwd=? where id="+id;
   
           //封装
       Object[] objects = new Object[2];
       objects[0] = "小明2";
       objects[1] = "22222";
       jdbcTemplate.update(sql,objects);
       return "update ok";
   }
   
       @GetMapping("/deleteUser/{id}")
       public String deleteUser(@PathVariable("id") int id){
           String sql = "delete from mybatis.user where id=?";
           jdbcTemplate.update(sql,id);
           return "delete-ok";
   ```



# 14、整合Druid

## 简介

- Druid 主要作用，**监控**

- Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。

- Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。

Github地址：https://github.com/alibaba/druid/

**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

![image-20210316122206626](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317113554415.png)

![image-20210316122229040](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/swagger.png)

## 操作

1、导入依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.5</version>
</dependency>

```

2、切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以 通过 spring.datasource.type 指定数据源。

```yaml
spring:
  datasource:
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characeterEncoding=utf-8&serverTimezone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
```



3、切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

这些一般都是直接导入公司的

```yaml

spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

4、导入Log4j 的依赖

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

5、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

新建一个DruidConfig.java

```java

@Configuration//这个注解就相当于，bean的xmL文件
public class DruidConfig {
    //自定义组件
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }
    //  后台监控
    //相当于web.xml, ServletRegistrationBean
    //因为SpringBoot 内置了servlet 容器，所以，没有web.xml,所以，就用替代方法 ： ServletRegistrationBean
    @Bean
    public ServletRegistrationBean StatViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");

        //后台需要有任登录，账号密码设置
        HashMap<String, String> initParameters = new HashMap<>();
        //增加配置
        //登录的key值是固定的，loginUsername loginPassword ，不能随便写
        initParameters.put("loginUsername","admin");
        initParameters.put("loginPassword","123456");

        //允许谁可以访问
        //这个的value值，可以是一个具体的 比如，localhost，也可以不写，就是都可以访问
        initParameters.put("allow","");
//        //禁止谁能访问
//        initParameters.put("joker","ip地址");//配置了ip,这个ip就禁止访问了

        bean.setInitParameters(initParameters);//设置初始化参数
        return bean;
    }

    //filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean<Filter> bean = new FilterRegistrationBean<>();
        bean.setFilter(new WebStatFilter());
        //过滤的请求
        HashMap<String, String> initParameters = new HashMap<>();
        //这些东西不进行统计
        //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
        initParameters.put("exclusions","*.js,*.css,/druid/*");
        bean.setInitParameters(initParameters);
//        //"/*" 表示过滤所有请求
//        bean.setUrlPatterns(Arrays.asList("/*"));
        return bean;
    }

}
```

6 、测试，直接启动后输入http://localhost:8080/druid

- 这个会自动跳转到http://localhost:8080/druid/login.html



# 15、整合mybatis

步骤:

- 导入依赖
- 建立实体类
- 建立mapper接口
- application.yaml中配置整合myatis的参数
- 建立mapper.xml配置文件
- 此处省略了service层，
- 建立controller
- 测试

1、导入依赖

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>

```

2 、建立实体类

```java

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
   private int id;
   private String name;
   private String pwd;

}
```

3、mapper接口

```java
//也可以在，主程序，添加，扫描mapper包，@MapperScan("com.joker.mapper")
@Mapper//这个注解表示这个一个mybaits的mapper接口
@Repository
public interface UserMapper {
    public List<User> queryUserList();

}
```

4、在application.yaml中配置整合myatis的参数

```yaml
#整合mybatis的相关属性
mybatis:
  type-aliases-package: com.joker.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```

5、建立配置文件，在resources|mybatis|mapper|UserMapper.xml

注意，这个使用后，就不能在mapper里使用sql注解了

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.joker.mapper.UserMapper">

<select id="queryUserList" resultType="User">
    select * from mybatis.user
</select>
</mapper>
```



6、controller

```java
@RestController
public class MybatisController {
    @Autowired
    private UserMapper userMapper;
    @GetMapping("/queryUserList")
    public List<User> queryUserList(){
        List<User> users = userMapper.queryUserList();
        for (User user : users) {
            System.out.println(user);
        }
        return users;
    }
}
```

7、启动	， 测试 http://localhost:8080/queryUserList

## over



# 16、SpringSecurity(重点)

## 简介

- 安全
- 功能性需求：否
- 做网站什么时候考虑安全：设计之初

SpringSecurity 官网： https://spring.io/projects/spring-security

文档 https://docs.spring.io/spring-security/site/docs/5.4.5/reference/html5/

官网介绍：

- Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于spring的应用程序的标准。

- Spring Security是一个框架，侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样，Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求

知名 的**安全框架**：

- shiro
- SpringSecurity
- 这两个东西很像，除了类不一样，名字不一样
- 主要 干的事情：**认证**（用户名，密码...）、**授权**(比如：vip1,vip2...)

权限一般有：

- 功能权限
- 访问权限
- 菜单权限
- 。。。

如果不用这个，就需要用大量的拦截器、过滤器。太过复杂

- 添加安全功能，使用AOP思想

Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括**用户认证**（Authentication）和**用户授权**（Authorization）两个部分。

- 用户认证： 指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。

- 用户授权： 指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。

## 操作

### 认证和授权	

- 只需要引入  spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理

- ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
  ```

**需要了解的类：**

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

根据官网文档 helloworld 文档写，https://github.com/spring-projects/spring-security/blob/5.4.5/samples/boot/helloworld/src/main/java/org/springframework/security/samples/config/SecurityConfig.java

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
    }
}
```

**步骤：**

- 导入 spring-boot-starter-security 模块，导入 静态资源（静态资源是用SemanticUI），导入thymeleaf模板引擎

- 关闭缓存

  ```
  spring.thymeleaf.cache=false
  ```

- 测试静态资源

  ```java
  @Controller
  public class RouterController {
      @RequestMapping({"/","/index"})
      public String index(){
          return "index";
      }
      @RequestMapping("/toLogin")
      public String toLogin(){
          return "views/login";
      }
      @RequestMapping("/level1/{id}")
      public String level1(@PathVariable("id") int id){
          return "views/level1/"+id;
      }
  
      @RequestMapping("/level2/{id}")
      public String level2(@PathVariable("id") int id){
          return "views/level2/"+id;
      }
      @RequestMapping("/level3/{id}")
      public String level3(@PathVariable("id") int id){
          return "views/level3/"+id;
      }
  }
  ```

- **授权和认证**的代码

- ```java
  package com.joker.config;
  
  import org.springframework.context.annotation.Configuration;
  import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
  import org.springframework.security.config.annotation.web.builders.HttpSecurity;
  import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
  import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
  import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
  
  @EnableWebSecurity
  public class SecurityConfig extends WebSecurityConfigurerAdapter {
      //这个是 链式编程
      //授权
      @Override
      protected void configure(HttpSecurity http) throws Exception {
          //首页所有人可以访问，功能页，只有对应的有权限的人才可以访问
          //请求授权的规则
          http.authorizeRequests()
                  .antMatchers("/").permitAll()
                  .antMatchers("level1/**").hasRole("vip1")
                  .antMatchers("level2/**").hasRole("vip2")
                  .antMatchers("level3/**").hasRole("vip3");
  
          //没有权限会默认跳到登录页 /login，需要开启登录的页面
          //这里看看formLogin的源码
          http.formLogin();
         
      }
      //认证
      @Override
      protected void configure(AuthenticationManagerBuilder auth) throws Exception {
          //这些数据应该从数据库读，
          //这里只是为了测试，用的缓存操作
          //格式 可以从formLogin()源码里看见
          //srping security 5 有passwordEncoder 就是加密
          auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                  .withUser("joker").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2")
                  .and()
                  .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3");
      }
  }
  ```

### 权限控制和注销 logout()

开启自动配置的注销的功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
   //....
   //开启自动配置的注销的功能
      // /logout 注销请求
   http.logout();
}
```

2、我们在前端，增加一个注销的按钮，index.html 导航栏中

```
<a class="item" th:href="@{/logout}">
   <i class="address card icon"></i> 注销
</a>
```

3、我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！

4、但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

```
// .logoutSuccessUrl("/"); 注销成功来到首页
http.logout().logoutSuccessUrl("/");
```

5、测试，注销完毕后，发现跳转到首页OK

6、我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

我们需要结合thymeleaf中的一些功能

sec：authorize="isAuthenticated()":是否认证登录！来显示不同的页面

Maven依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
<dependency>
   <groupId>org.thymeleaf.extras</groupId>
   <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   <version>3.0.4.RELEASE</version>
</dependency>
```

7、修改我们的 前端页面

1. 导入命名空间

2. ```
   xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
   ```

3. 修改导航栏，增加认证判断

4. ```html
   <!--登录注销-->
   <div class="right menu">
   
      <!--如果未登录-->
      <div sec:authorize="!isAuthenticated()">
          <a class="item" th:href="@{/login}">
              <i class="address card icon"></i> 登录
          </a>
      </div>
   
      <!--如果已登录-->
      <div sec:authorize="isAuthenticated()">
          <a class="item">
              <i class="address card icon"></i>
             用户名：<span sec:authentication="principal.username"></span>
             角色：<span sec:authentication="principal.authorities"></span>
          </a>
      </div>
   
      <div sec:authorize="isAuthenticated()">
          <a class="item" th:href="@{/logout}">
              <i class="address card icon"></i> 注销
          </a>
      </div>
   </div>
   ```

8、重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；

9、如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

```
http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
http.logout().logoutSuccessUrl("/");
```

10、我们继续将下面的角色功能块认证完成！

```html
<!-- sec:authorize="hasRole('vip1')" -->
<div class="column" sec:authorize="hasRole('vip1')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 1</h5>
               <hr>
               <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
               <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
               <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
           </div>
       </div>
   </div>
</div>

<div class="column" sec:authorize="hasRole('vip2')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 2</h5>
               <hr>
               <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
               <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
               <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
           </div>
       </div>
   </div>
</div>

<div class="column" sec:authorize="hasRole('vip3')">
   <div class="ui raised segment">
       <div class="ui">
           <div class="content">
               <h5 class="content">Level 3</h5>
               <hr>
               <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
               <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
               <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
           </div>
       </div>
   </div>
</div>
```

11、测试一下！

12、权限控制和注销搞定！

### remembeMe()

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单

1、开启记住我功能

```java
//定制请求的授权规则
@Override
protected void configure(HttpSecurity http) throws Exception {
//。。。。。。。。。。。
   //记住我
   http.rememberMe();
}
```

2、我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

思考：如何实现的呢？其实非常简单

我们可以查看浏览器的cookie



3、我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

4、结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，

### 定制登录页

现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？

1、在刚才的登录页配置后面指定 loginpage

```java
http.formLogin().loginPage("/toLogin");
```

2、然后前端也需要指向我们自己定义的 login请求

```html
<a class="item" th:href="@{/toLogin}">
   <i class="address card icon"></i> 登录
</a>
```

3、我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:

在 loginPage()源码中的注释上有写明：

![image-20210316185829324](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317142326878.png)

```html
<form th:action="@{/login}" method="post">
   <div class="field">
       <label>Username</label>
       <div class="ui left icon input">
           <input type="text" placeholder="Username" name="username">
           <i class="user icon"></i>
       </div>
   </div>
   <div class="field">
       <label>Password</label>
       <div class="ui left icon input">
           <input type="password" name="password">
           <i class="lock icon"></i>
       </div>
   </div>
   <input type="submit" class="ui blue submit button"/>
</form>
```

4、这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！

```java
http.formLogin()
  .usernameParameter("username")
  .passwordParameter("password")
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

5、在登录页增加记住我的多选框

```
<input type="checkbox" name="remember"> 记住我
```

6、后端验证处理！

```
//定制记住我的参数！
http.rememberMe().rememberMeParameter("remember");
```

7、测试，OK



## 总和-完整配置代码

```java
package com.joker.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {

       http.authorizeRequests().antMatchers("/").permitAll()
      .antMatchers("/level1/**").hasRole("vip1")
      .antMatchers("/level2/**").hasRole("vip2")
      .antMatchers("/level3/**").hasRole("vip3");


       //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
           // /login 请求来到登录页
           // /login?error 重定向到这里表示登录失败
       http.formLogin()
          .usernameParameter("username")
          .passwordParameter("password")
          .loginPage("/toLogin")
          .loginProcessingUrl("/login"); // 登陆表单提交请求

       //开启自动配置的注销的功能
           // /logout 注销请求
           // .logoutSuccessUrl("/"); 注销成功来到首页

       http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
       http.logout().logoutSuccessUrl("/");

       //记住我
       http.rememberMe().rememberMeParameter("remember");
  }

   //定义认证规则
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       //在内存中定义，也可以在jdbc中去拿....
       //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
       //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
       //spring security 官方推荐的是使用bcrypt加密方式。

       auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
              .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
              .and()
              .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
              .and()
              .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
  }
}
```



# 17、Shiro

shiro 官网：http://shiro.apache.org/

github : https://github.com/apache/shiro

- 首先学习，官网的10分钟，quidk start ，先快速抄过来那个起手项目 http://shiro.apache.org/10-minute-tutorial.html
- 项目压缩包下载地址 https://www.apache.org/dyn/closer.lua/shiro/1.7.1/shiro-root-1.7.1-source-release.zip
- shiro **核心**
  - Subject 用户，
  - Security Manager 管理所有用户
  - realm  连接数据

## hell-shiro

- 官网的10分钟快速开始

步骤：

- 导入依赖
- 配置文件
- HelloWorld

这里的QuickStart 文件，主要是和Subject 有关，用户

QuickStart  的重点：这部分，Spring Security都有

```java
Subject currentUser = SecurityUtils.getSubject();
Session session = currentUser.getSession();
if (!currentUser.isAuthenticated());
if (currentUser.hasRole("schwartz"));
if (currentUser.isPermitted("lightsaber:wield")) ;
if (currentUser.isPermitted("winnebago:drive:eagle5"));
currentUser.logout();
```



## 结合springBoot

- 导入thymeleaf依赖

### 1、环境搭建

### 2、登录拦截

### 3、用户认证

### 4、整合Mybatis

导入依赖

```xml
<!--        整合mabatis-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.5</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
```



- 负责加密的接口，CredentialsMatcher



### 5、授权



### 6、整合Thymeleaf

- 实现，各个用户，只显示，自己有权限的部分 

导包

```xml
<!--        shiro 整合 thymeleaf的包-->
        <dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>
```

## 总和-代码

**文件目录**：

![image-20210317113554415](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317155545106.png)

### maven配置文件 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.joker</groupId>
    <artifactId>springboo-shiro</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboo-shiro</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
<!--        shiro 整合 thymeleaf的包-->
        <dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.18</version>
        </dependency>
<!--        整合mabatis-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.5</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
<!--        shiro 整合spring的包-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring-boot-web-starter</artifactId>
            <version>1.7.1</version>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
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

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

### 总配置文件 application.yml

```yaml
spring:
  datasource:
    username: root
    password: 123456
    #?serverTimezone=UTC解决时区的报错
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500

mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.joker.pojo
```

### config|ShiroConfig

```java
package com.joker.config;

import at.pollux.thymeleaf.shiro.dialect.ShiroDialect;
import org.apache.commons.collections.map.LinkedMap;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.LinkedHashMap;

/**
 * 配置的时候，倒着写，顺序从realm->securityManager->subject,因为会依次调用
 */
@Configuration
public class ShiroConfig {
    //ShiroFilterFactoryBean    第三步
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(@Qualifier("getDefaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置securityManager
        bean.setSecurityManager(defaultWebSecurityManager);
        //添加内置过滤器 ,有5种
        /*
         anon : 无需认证就可以访问
         authc : 必须认证了才能访问
         user : 必须拥有 记住我 rememberMe 功能才能用
         perms  ；拥有对某个资源的权限才能用
         role : 拥有某个角色权限才能访问
         */
        //拦截
        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();
        //授权,正常情况下，未授权会跳转到未授权页面
        //perms[user:add] 表示要有user:add，这个权限才可以，访问，一般会在数据库设置权限字段
        filterChainDefinitionMap.put("/user/add","perms[user:add]");
        filterChainDefinitionMap.put("/user/update","perms[user:update]");

        filterChainDefinitionMap.put("/user/add","authc");
        filterChainDefinitionMap.put("/user/update","authc");
        bean.setFilterChainDefinitionMap(filterChainDefinitionMap);
        //设置登录的请求
        bean.setLoginUrl("/toLogin");
        //未授权页面
        bean.setUnauthorizedUrl("/unauthorized");

        return bean;

    }
    //第二步   DefaultWebSecurityManager
    //@Qualifier("userRealm") 就是用来绑定自定义的realm
    @Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        //关联realm
        securityManager.setRealm(userRealm);
        return securityManager;
    }
    //创建realm对象 第一步 需要自定义
    @Bean
    public UserRealm userRealm(){
        return new UserRealm();
    }

    //ShiroDialect 用来整合 shiro thymeleaf
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
}
```

### config|UserRealm

```java
package com.joker.config;

import com.joker.pojo.User;
import com.joker.service.UserService;
import com.joker.service.UserServiceImpl;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;

//自定义的realm ,要继承 AuthorizingRealm
public class UserRealm extends AuthorizingRealm {
    @Autowired
    UserServiceImpl userService;

    @Override//授权
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println("执行了授权");
        //添加授权
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        info.addStringPermission("user:add");

        //拿到当前登录的对象
        Subject subject = SecurityUtils.getSubject();
        User currentUser =(User) subject.getPrincipal();//拿到User

        info.addStringPermission(currentUser.getPerms());
        return info;
    }

    @Override//认证
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("执行了认证");
        //用户名，密码 ，本来应该在数据库里取出
//        String name = "root";
//        String pwd = "123456";
        UsernamePasswordToken usernamePasswordToken = (UsernamePasswordToken) token;
        //连接真实数据库
        User user = userService.queryUserByName(usernamePasswordToken.getUsername());
        if (user==null){
            return null;
        }

//        if (!usernamePasswordToken.getUsername().equals((name))){
//            return null;//返回null,就是抛出异常 会自动抛出用户名异常
//        }

        //密码认证，shiro做，我们不用做
        //可以在这里设置断点，看到
        // CredentialsMatcher 接口，用于加密，有很多实现类  比如MD5 ,md5盐值加密
        return new SimpleAuthenticationInfo(user,user.getPwd(),"");
    }
}
```

### pojo|User

```java
package com.joker.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
    private String perms;
}
```

### mapper|UserMapper

```java
package com.joker.mapper;

import com.joker.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

@Repository
@Mapper
public interface UserMapper {
    public User queryUserByName(String name);
}
```

### resources|mapper|UserMapper.xml

```html
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.joker.mapper.UserMapper">

    <select id="queryUserByName" resultType="User">
        select * from mybatis.user where name=#{name}
    </select>
</mapper>
```

### service|UserService

```java
package com.joker.service;

import com.joker.pojo.User;

public interface UserService {
    public User queryUserByName(String name);
}
```

### service|UserServiceImpl

```java
package com.joker.service;

import com.joker.mapper.UserMapper;
import com.joker.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {
    @Autowired
    UserMapper userMapper;
    @Override
    public User queryUserByName(String name) {
        return userMapper.queryUserByName(name);
    }
}
```

### index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>shiro-index</title>
</head>
<body>
<h1>首页</h1>
<!--shiro:guest 可以是登录进去后，登录连接，，消失-->
<div shiro:guest="">
    <a th:href="@{/toLogin}">登录</a>
</div>


<p th:text="${msg}"> </p>
<hr>

<div shiro:hasPermission="user:add">
    <a th:href="@{/user/add}">add</a>
</div>
<div shiro:hasPermission="user:update">
    <a th:href="@{/user/update}">update</a>
</div>

</body>
</html>
```

### login.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>shiro-index</title>
</head>
<body>
<h1>首页</h1>
<!--shiro:guest 可以是登录进去后，登录连接，，消失-->
<div shiro:guest="">
    <a th:href="@{/toLogin}">登录</a>
</div>


<p th:text="${msg}"> </p>
<hr>

<div shiro:hasPermission="user:add">
    <a th:href="@{/user/add}">add</a>
</div>
<div shiro:hasPermission="user:update">
    <a th:href="@{/user/update}">update</a>
</div>

</body>
</html>
```

### templates|user|update.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>update</h1>
</body>
</html>
```

### templates|user|add.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>add</h1>
</body>
</html>
```

### SpringbooShiroApplication

- 这个不用管

```java
package com.joker;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbooShiroApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbooShiroApplication.class, args);
    }

}
```



# 18、一个开源项目分析

- 分析一个开源的博客
- 自己找个后端模板练手



# 19、Swagger

学习目标：

- 了解Swagger 的作用和概念
- 了解前后端分离
- 在sprinboot 种集成Swagger

## Swagger 简介

**前后端分离时代**：

目前主流的技术栈： Vue + SpringBoot

- 后端：后端控制层，服务层，数据访问层
- 前端：前端控制层，视图层
  - 可以伪造后端数据，json,前端工程依旧可以跑起来，
- 前后端如何交互？--》API
- 前后端相互独立，松耦合；
- 前后端甚至可以部署在不同的服务器上



产生一个问题：

- 前后端集成联调，前后端人员无法做到，“即时协商，尽早解决”，	最终导致问题集中爆发

解决方案：

- 首先指定一个schema[计划的提纲]，实时更新最新API，降低集成的风险；
- 早些年：制定word计算文档；
- 现在，前后端分离：
  - 前端测试后端接口：postman[一个测试工具]
  - 后端提供接口：需要实时更新最新的消息及改动	

## Swagger

swagger 官网：https://swagger.io/

- 号称世界上最流行的api框架
- RestFul风格的API
- 文档在线自动生成工具-->API文档与API 定义同步更新
- 可以直接运行，在线测试API接口
- 支持多种语言（java,php，。。。）



在项目中使用swagger  ， 需要 springfox:

- swagger2
- swagger ui



## springBoot 集成Swagger

1. 新建一个springboot web项目

2. 导入相关依赖 3.0以上版本，可以直接导入

   ```xml
   <!-- https://mvnrepository.com/artifact/io.springfox/springfox-boot-starter -->
   <dependency>
       <groupId>io.springfox</groupId>
       <artifactId>springfox-boot-starter</artifactId>
       <version>3.0.0</version>
   </dependency>
   
   ```

3. 建立一个hello工程

4. 配置Swagger--》建立一个Config.

   ```java
   @Configuration
   @EnableOpenApi//开启 swagger2
   public class SwaggerConfig {
   
   }
   ```

5. 测试运行 http://localhost:8080/swagger-ui/index.html

   ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317215541104.png)



## 配置Swagger

Docket 

```java
@Configuration
@EnableOpenApi
public class SwaggerConfig {
    @Bean//配置swagger的Docket的bean实例
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }
    //配置swagger信息 =  ApiInfo
    //看源码，看构造函数
    public ApiInfo apiInfo(){
        //自定义作者信息
        Contact DEFAULT_CONTACT = new Contact("joker", "", "");
        return new ApiInfo(
                "joker-Api Documentation",
                "joker-Api Documentation",
                "1.0",
                "urn:tos",
                DEFAULT_CONTACT,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                 new ArrayList());
    }
}
```



## Swagger配置扫描接口

Docket.select()

```java
    @Bean//配置swagger的Docket的bean实例
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                //RequestHandlerSelectors 配置要扫描接口的方式：
                //basePackage 指定要扫描的包basePackage("com.joker.swagger.controller")
                //any() 扫描全部
                //none() 都不扫描
                //withMethodAnnotation()  扫描方法上的注解
                //withClassAnnotation() 扫描类上的注解，参数是一个注解的反射对象
                .apis(RequestHandlerSelectors.basePackage("com.joker.swagger.controller"))
                //过滤路径，即，只在joker下的才扫描
                .paths(PathSelectors.ant("/joker/**"))
                .build();
    }
```

配置是否启动swagger

enable

```java
return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .enable(false)//enable 是否启动swagger，如果为false,则swagger，不能在浏览器种访问
        .select()
```

> **例题**：

希望swagger只在特定环境中，才能使用，比如，在开发的时候使用，在发布的时候不使用：

- 判断是不是生产环境， flag=false
- 注入enable(flag)

```java
public class SwaggerConfig {
    @Bean//配置swagger的Docket的bean实例
    public Docket docket(Environment environment){
        //设置要显示的swagger 环境
        Profiles profiles=Profiles.of("dev");
        //environment.acceptsProfiles 判断是否处在自己设定的环境当中
        boolean flag = environment.acceptsProfiles(profiles);
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .enable(flag)//enable 是否启动swagger，如果为false,则swagger，不能在浏览器种访问
                .select()
```

新建两个配置文件 

application-pro.yml

```yml
server:
  port: 8082
```

application-dev.yml

```yml
server:
  port: 8081
```

设置，application.yml

```yml
spring:
  profiles:
    active: dev
```

## Swagger 配置API 文档分组

Docket().groupName();

- 配置多个Docket实例即可

```java
public class SwaggerConfig {
    @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("A");
    }
    @Bean
    public Docket docket2(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("B");
    }
    @Bean
    public Docket docket3(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("C");
    }
```

![image-20210317142326878](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317220241865.png)



## 实体类配置

1、新建一个实体类

```
@ApiModel("用户实体")
public class User {
   @ApiModelProperty("用户名")
   public String username;
   @ApiModelProperty("密码")
   public String password;
}
```

2、只要这个实体在**请求接口**的返回值上（即使是泛型），都能映射到实体项中：

```
@RequestMapping("/getUser")
public User getUser(){
   return new User();
}
```

3、重启查看测试



注：并不是因为@ApiModel这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel和@ApiModelProperty这两个注解只是为实体添加注释的。

@ApiModel为类添加注释

@ApiModelProperty为类属性添加注释



**Swagger总结：**

- 通过swagger, 给一些比较难理解的属性或者接口，增加注释信息
- 接口文档实时更新
- 可以在线测试

注意：正式发布的时候，关闭Swagger



## 常用注解

Swagger的所有注解定义在io.swagger.annotations包下

下面列一些经常用到的，未列举出来的可以另行查阅说明：

| Swagger注解                                            | 简单说明                                             |
| ------------------------------------------------------ | ---------------------------------------------------- |
| @Api(tags = "xxx模块说明")                             | 作用在模块类上                                       |
| @ApiOperation("xxx接口说明")                           | 作用在接口方法上                                     |
| @ApiModel("xxxPOJO说明")                               | 作用在模型类上：如VO、BO                             |
| @ApiModelProperty(value = "xxx属性说明",hidden = true) | 作用在类方法和属性上，hidden设置为true可以隐藏该属性 |
| @ApiParam("xxx参数说明")                               | 作用在参数、方法和字段上，类似@ApiModelProperty      |

我们也可以给请求的接口配置一些注释

```
@ApiOperation("狂神的接口")
@PostMapping("/kuang")
@ResponseBody
public String kuang(@ApiParam("这个名字会被返回")String username){
   return username;
}
```

这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读！

相较于传统的Postman或Curl方式测试接口，使用swagger简直就是傻瓜式操作，不需要额外说明文档(写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击Execute，如果再配合自动化框架，可以说基本就不需要人为操作了。

Swagger是个优秀的工具，现在国内已经有很多的中小型互联网公司都在使用它，相较于传统的要先出Word接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。当然了，提醒下大家在正式环境要记得关闭Swagger，一来出于安全考虑二来也可以节省运行时内存。



## 拓展：其他皮肤

我们可以导入不同的包实现不同的皮肤定义：

1、默认的  **访问 http://localhost:8080/swagger-ui.html**

```
<dependency>
   <groupId>io.springfox</groupId>
   <artifactId>springfox-swagger-ui</artifactId>
   <version>2.9.2</version>
</dependency>
```

2、bootstrap-ui  **访问 http://localhost:8080/doc.html**

```
<!-- 引入swagger-bootstrap-ui包 /doc.html-->
<dependency>
   <groupId>com.github.xiaoymin</groupId>
   <artifactId>swagger-bootstrap-ui</artifactId>
   <version>1.9.1</version>
</dependency>
```

3、Layui-ui  **访问 http://localhost:8080/docs.html**

```
<!-- 引入swagger-ui-layer包 /docs.html-->
<dependency>
   <groupId>com.github.caspar-chen</groupId>
   <artifactId>swagger-ui-layer</artifactId>
   <version>1.1.3</version>
</dependency>
```

4、mg-ui  **访问 http://localhost:8080/document.html**

```
<!-- 引入swagger-ui-layer包 /document.html-->
<dependency>
   <groupId>com.zyplayer</groupId>
   <artifactId>swagger-mg-ui</artifactId>
   <version>1.0.6</version>
</dependency>
```



# 20、任务

## 1、异步任务

- 就是页面，即时刷新，但是后台，等会在处理数据

1、创建一个service包

2、创建一个类AsyncService

异步处理还是非常常用的，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。

编写方法，假装正在处理数据，使用线程设置一些延时，模拟同步等待的情况；

```java
@Service
public class AsyncService {

   public void hello(){
       try {
           Thread.sleep(3000);
      } catch (InterruptedException e) {
           e.printStackTrace();
      }
       System.out.println("业务进行中....");
  }
}
```

3、编写controller包

4、编写AsyncController类

我们去写一个Controller测试一下

```java
@RestController
public class AsyncController {

   @Autowired
   AsyncService asyncService;

   @GetMapping("/hello")
   public String hello(){
       asyncService.hello();
       return "success";
  }

}
```

5、访问http://localhost:8080/hello进行测试，3秒后出现success，这是同步等待的情况。

问题：我们如果想让用户直接得到消息，就在后台使用多线程的方式进行处理即可，但是每次都需要自己手动去编写多线程的实现的话，太麻烦了，我们只需要用一个简单的办法，在我们的方法上加一个简单的注解即可，如下：

6、给hello方法添加@Async注解；

```java
//告诉Spring这是一个异步方法
@Async
public void hello(){
   try {
       Thread.sleep(3000);
  } catch (InterruptedException e) {
       e.printStackTrace();
  }
   System.out.println("业务进行中....");
}
```

SpringBoot就会自己开一个线程池，进行调用！但是要让这个注解生效，我们还需要在主程序上添加一个注解@EnableAsync ，开启异步注解功能；

```java
@EnableAsync //开启异步注解功能
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

7、重启测试，网页瞬间响应，后台代码依旧执行！

## 2、定时任务

- 表达式

- 重要的自带的接口

  - TaskExecutor	任务执行
  - TaskScheduler 任务调度

- ```
  @EnableScheduling//开启定时功能的注解
  @Scheduled //表示什么时候执行
  Cron 表达式 设置时间的表达式，怎么写，自己百度，总结
  ```

原话：

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

+ TaskExecutor接口
+ TaskScheduler接口

两个注解：

+ @EnableScheduling
+ @Scheduled

**cron表达式：**

![image-20210317155545106](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317220035104.png)



**测试步骤：**

1、创建一个ScheduledService

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```java
@Service
public class ScheduledService {
   
   //秒   分   时     日   月   周几
   //0 * * * * MON-FRI
   //注意cron表达式的用法；
   @Scheduled(cron = "0 * * * * 0-7")
   public void hello(){
       System.out.println("hello.....");
  }
}
```

2、这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

```java
@EnableAsync //开启异步注解功能
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {

   public static void main(String[] args) {
       SpringApplication.run(SpringbootTaskApplication.class, args);
  }

}
```

3、我们来详细了解下cron表达式；

http://www.bejson.com/othertools/cron/

4、常用的表达式

```text
（1）0/2 * * * * ?   表示每2秒 执行任务
（1）0 0/2 * * * ?   表示每2分钟 执行任务
（1）0 0 2 1 * ?   表示在每月的1日的凌晨2点调整任务
（2）0 15 10 ? * MON-FRI   表示周一到周五每天上午10:15执行作业
（3）0 15 10 ? 6L 2002-2006   表示2002-2006年的每个月的最后一个星期五上午10:15执行作
（4）0 0 10,14,16 * * ?   每天上午10点，下午2点，4点
（5）0 0/30 9-17 * * ?   朝九晚五工作时间内每半小时
（6）0 0 12 ? * WED   表示每个星期三中午12点
（7）0 0 12 * * ?   每天中午12点触发
（8）0 15 10 ? * *   每天上午10:15触发
（9）0 15 10 * * ?     每天上午10:15触发
（10）0 15 10 * * ?   每天上午10:15触发
（11）0 15 10 * * ? 2005   2005年的每天上午10:15触发
（12）0 * 14 * * ?     在每天下午2点到下午2:59期间的每1分钟触发
（13）0 0/5 14 * * ?   在每天下午2点到下午2:55期间的每5分钟触发
（14）0 0/5 14,18 * * ?     在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
（15）0 0-5 14 * * ?   在每天下午2点到下午2:05期间的每1分钟触发
（16）0 10,44 14 ? 3 WED   每年三月的星期三的下午2:10和2:44触发
（17）0 15 10 ? * MON-FRI   周一至周五的上午10:15触发
（18）0 15 10 15 * ?   每月15日上午10:15触发
（19）0 15 10 L * ?   每月最后一日的上午10:15触发
（20）0 15 10 ? * 6L   每月的最后一个星期五上午10:15触发
（21）0 15 10 ? * 6L 2002-2005   2002年至2005年的每月的最后一个星期五上午10:15触发
（22）0 15 10 ? * 6#3   每月的第三个星期五上午10:15触发
```



## 3、邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

+ 邮件发送需要引入spring-boot-start-mail
+ SpringBoot 自动配置MailSenderAutoConfiguration
+ 定义MailProperties内容，配置在application.yml中
+ 自动装配JavaMailSender
+ 测试邮件发送

**测试：**

1、引入pom依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

看它引入的依赖，可以看到 jakarta.mail

```xml
<dependency>
   <groupId>com.sun.mail</groupId>
   <artifactId>jakarta.mail</artifactId>
   <version>1.6.4</version>
   <scope>compile</scope>
</dependency>
```

2、查看自动配置类：MailSenderAutoConfiguration

这个类中存在bean，JavaMailSenderImpl



然后我们去看下配置文件

```java
@ConfigurationProperties(
   prefix = "spring.mail"
)
public class MailProperties {
   private static final Charset DEFAULT_CHARSET;
   private String host;
   private Integer port;
   private String username;
   private String password;
   private String protocol = "smtp";
   private Charset defaultEncoding;
   private Map<String, String> properties;
   private String jndiName;
}
```

3、配置文件：application.properties

```
spring.mail.username=24736743@qq.com
spring.mail.password=你的qq授权码
spring.mail.host=smtp.qq.com
# qq需要配置ssl
spring.mail.properties.mail.smtp.ssl.enable=true
```

获取授权码：在QQ邮箱中的设置->账户->开启pop3和smtp服务

4、Spring单元测试

```java
@Autowired
JavaMailSenderImpl mailSender;

@Test
public void contextLoads() {
   //邮件设置1：一个简单的邮件
   SimpleMailMessage message = new SimpleMailMessage();
   message.setSubject("通知-明天来狂神这听课");
   message.setText("今晚7:30开会");

   message.setTo("24736743@qq.com");
   message.setFrom("24736743@qq.com");
   mailSender.send(message);
}

@Test
public void contextLoads2() throws MessagingException {
   //邮件设置2：一个复杂的邮件
   MimeMessage mimeMessage = mailSender.createMimeMessage();
   MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

   helper.setSubject("通知-明天来狂神这听课");
   helper.setText("<b style='color:red'>今天 7:30来开会</b>",true);

   //发送附件
   helper.addAttachment("1.jpg",new File("路径"));
   helper.addAttachment("2.jpg",new File("路径"));

   helper.setTo("24736743@qq.com");
   helper.setFrom("24736743@qq.com");

   mailSender.send(mimeMessage);
}
```

查看邮箱，邮件接收成功！

- 可以自己把这个类封装起来，一般在service 层

我们只需要使用Thymeleaf进行前后端结合即可开发自己网站邮件收发功能了！





# 21 、分布式 dubbo+zookeeper+springboot

## 21.1  分布式理论

- 在《分布式系统原理与范型》一书中有如下**定义**：

  “分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

- 分布式系统是 **由**一组 **通过**网络进行通信、**为了**完成共同的任务而协调工作**的 计算机节点** 组成**的系统**。

- 分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。

  其**目的**是**利用更多的机器，处理更多的数据**。

- 分布式系统（distributed system）是**建立在网络之上**的软件系统。

- 什么时候引入分布式及带来的问题？
  - 首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。
  - 因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

## 21.2 Dubbo文档

Dubbo 官网： https://dubbo.apache.org/zh/

Dubbo框架设计：https://dubbo.apache.org/zh/docs/v2.7/dev/design/#%E6%95%B4%E4%BD%93%E8%AE%BE%E8%AE%A1

### 背景

- 随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。

在Dubbo的官网文档有这样一张图：

​	![image-20210317215541104](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317220551496.png)

#### 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

![image-20210317220035104](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317220637131.png)

适用于小型网站，小型管理系统，将所有功能都部署到一个部署节点里，简单易用。

**缺点：**

1、性能扩展比较难

2、协同开发问题

3、不利于升级维护



#### 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的**加速度**越来越小，提升效率的方法之一是将**应用拆成互不相干的几个应用**，以**提升效率**。此时，用于加速前端页面开发的Web框架(MVC)是关键。

![image-20210317220241865](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317221441526.png)

通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

**缺点**：公用模块无法重复利用，开发性的浪费



#### 分布式服务架构

当垂直应用越来越多，应用之间**交互**不可避免，将**核心业务抽取出来**，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高**业务复用**及**整合的分布式服务框架(RPC)**是关键。

![image-20210317220551496](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317221743210.png)



#### 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需**增加一个调度中心基于访问压力实时管理集群容量**，提高集群利用率。此时，**用于提高机器利用率的资源调度和治理中心**(SOA)（Service Oriented Architecture）是关键。

![image-20210317220637131](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317223148799.png)





## 21.3 RPC

- Remote Procedure Call 远程过程调用

通信协议：

- http : 无状态协议
- RPC



​		RPC 是指远程过程调用，是一种进程间通信方式，他是**一种**技术的**思想**，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

- 也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。
- 为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b

RPC两个**核心模块**：**通讯，序列化**。

- 通讯：是为了通信连接，
- 序列化：数据传输需要转换

**RPC基本原理**

![image-20210317221441526](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210317233019504.png)



**步骤解析：** 对于上图的时序图

![image-20210317221743210](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318022436139.png)

## 21.4 Dubbo

Dubbo 官网： https://dubbo.apache.org/zh/

dubbo 快速开始：https://dubbo.apache.org/zh/docs/v2.7/user/quick-start/



### 1、**Dubbo架构**

https://dubbo.apache.org/zh/docs/v2.7/user/preface/architecture/

![image-20210317223148799](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030108337.png)

#### 节点角色说明

| 节点        | 角色说明                               |
| ----------- | -------------------------------------- |
| `Provider`  | 暴露服务的服务提供方                   |
| `Consumer`  | 调用远程服务的服务消费方               |
| `Registry`  | 服务注册与发现的注册中心               |
| `Monitor`   | 统计服务的调用次数和调用时间的监控中心 |
| `Container` | 服务运行容器                           |

- **服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

- **服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

- **注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

- **监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

#### 调用关系说明

1. 服务容器负责启动，加载，运行服务提供者。
2. 服务提供者在启动时，向注册中心注册自己提供的服务。
3. 服务消费者在启动时，向注册中心订阅自己所需的服务。
4. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
5. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
6. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

Dubbo 架构具有以下几个特点，分别是**连通性**、**健壮性**、**伸缩性**、以及向未来架构的**升级性**。

### 2、Dubbo环境搭建

点进dubbo官方文档，推荐我们使用Zookeeper 注册中心

什么是zookeeper呢？可以查看官方文档 ：https://zookeeper.apache.org/

Dubbo-Zookeeper注册中心：https://dubbo.apache.org/zh/docs/v2.7/user/references/registry/zookeeper/

安装ZooKeeper ：https://zookeeper.apache.org/doc/r3.6.2/zookeeperStarted.html#sc_Download

#### Window下安装zookeeper

1、下载zookeeper ：3.6.2 最新

2、修改conf/zoo.cfg配置文件

将conf文件夹下面的zoo_sample.cfg复制一份改名为zoo.cfg即可。

注意几个重要位置：

dataDir=./  临时数据存储的目录（可写相对路径）

clientPort=2181  zookeeper的端口号

修改完成后再次启动zookeeper

如果此时看见 ZooKeeper audit is disabled

- 由源码可知 是因为zookeeper新版本启动的过程中，zookeeper新增的审核日志是默认关闭，所以控制台输出ZooKeeper audit is disabled，标准的修改方式应该是在zookeeper的配置文件zoo.cfg新增一行audit.enable=true即可

4、使用zkCli.cmd测试 ， 要在命令行管理员模式下

ls /：列出zookeeper根下保存的所有节点

```
[zk: 127.0.0.1:2181(CONNECTED) 4] ls /
[zookeeper]
```

create –e /joker123：创建一个kuangshen节点，值为123

get /joker：获取/kuangshen节点的值

我们再来查看一下节点

![image-20210317233019504](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318023711044.png)



#### window下安装dubbo-admin

- dubbo-admin ： 可视化的监控程序。可以不要

dubbo本身并不是一个服务软件。它其实就是一个**jar包**，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。

dubbo-admin 下载地址：https://github.com/apache/dubbo-admin/tree/master

**1、下载dubbo-admin**

地址 ：https://github.com/apache/dubbo-admin/tree/master

**2、解压进入目录**

修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址

```
server.port=7001
spring.velocity.cache=false
spring.velocity.charset=UTF-8
spring.velocity.layout-url=/templates/default.vm
spring.messages.fallback-to-system-locale=false
spring.messages.basename=i18n/message
spring.root.password=root
spring.guest.password=guest

dubbo.registry.address=zookeeper://127.0.0.1:2181
```

**3、在项目目录下**打包dubbo-admin

```
mvn clean package -Dmaven.test.skip=true
```

**第一次打包的过程有点慢，需要耐心等待！直到成功！**

4、执行 dubbo-admin\target 下的dubbo-admin-0.0.1-SNAPSHOT.jar

【注意：要先打开zookeeper的服务zkServer.cmd】

```
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

执行完毕，我们去访问一下 http://localhost:7001/ ， 这时候我们需要输入登录账户和密码，我们都是默认的root-root；

登录成功后，查看界面

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7JJjARRqcZibY4ZPv60renshjHbZUAW6UOLfJhknMjgemFYgr2hz27iaBE4tiaKA86ZqIhOjd3vttV5w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装完成！



### 3、服务注册于发现

步骤：

前提：zookeeper服务已经开启 即zkServer.cmd

1. 提供者提供服务
   1. 导入依赖
   2. 配置注册中心的地址，以及服务发现名，和要扫描的包
   3. 在想要被注册的服务增加注解@DubboService
2. 消费者如何消费
   1. 导入依赖
   2. 配置注册中心的地址，配置自己的服务名
   3. 从远程注入服务，@DubboReference

#### 框架搭建

1. 启动zookeeper ！

2. IDEA创建一个空项目*

3.创建一个**模块**，实现**服务提供者**：provider-server ， 选择web依赖即可

4.项目创建完毕，我们写一个服务，比如卖票的服务；

编写接口

```java
package com.joker.service;

public interface TicketService {
   public String getTicket();
}
```

编写实现类

```java
package com.joker.service;

public class TicketServiceImpl implements TicketService {
   @Override
   public String getTicket() {
       return "joker-provider";
  }
}
```

5.创建一个**模块**，实现**服务消费者**：consumer-server ， 选择web依赖即可

6.项目创建完毕，我们写一个服务，比如用户的服务*

编写service

```java
package com.joker.service;

public class UserService {
   //我们需要去拿去注册中心的服务
}
```

需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？



#### 服务提供者

1、将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导

我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包

```xml
        <!--        导入依赖 Dubbo+zooKeeper-->
        <!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.8</version>
        </dependency>    
```

zookeeper的包我们去maven仓库下载，zkclient；

```xml
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
   <groupId>com.github.sgroschupf</groupId>
   <artifactId>zkclient</artifactId>
   <version>0.1</version>
</dependency>
```

【新版的坑】zookeeper及其依赖包，解决日志冲突，还需要剔除日志依赖；

```xml
<!--        日志会冲突-->
        <!--        引入zooKeeper-->
        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.6.2</version>
            <!--排除这个slf4j-log4j12-->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.1.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>5.1.0</version>
        </dependency>
```

2、在springboot配置文件中配置dubbo相关属性！

```properties
#当前应用名字
dubbo.application.name=provider-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
#扫描指定包下服务
dubbo.scan.base-packages=com.joker.service
```

3、在service的实现类中配置服务注解，发布服务！注意导包问题

```java
package com.joker.service;

import org.apache.dubbo.config.annotation.DubboService;
import org.springframework.stereotype.Component;

@DubboService//可以被扫描到，在项目一启动就注册到注册中心
public class TicketServiceImpl implements TicketService{
    @Override
    public String getTicket() {
        return "joker";
    }
}
```

逻辑理解 ：应用启动起来，dubbo就会扫描指定的包下带有@DubboService注解的服务，将它发布在指定的注册中心中！



#### 服务消费者

1、导入依赖，和之前的依赖一样；

```xml
<!--        导入依赖 Dubbo+zooKeeper-->
        <!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.8</version>
        </dependency>
<!--        zkClient-->
        <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
<!--        日志会冲突-->
<!--        引入zooKeeper-->
        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.6.2</version>
            <!--排除这个slf4j-log4j12-->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.1.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>5.1.0</version>
        </dependency><!--dubbo-->

```

2、配置参数

```
#当前应用名字
dubbo.application.name=consumer-server
#注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

3. 本来正常步骤是需要将服务提供者的接口打包，然后用pom文件导入，我们这里使用简单的方式，直接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；

4. 完善消费者的服务类

```java
package com.joker.service;

import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Service;

@Service//放到容器中
public class UserService {
    //想拿到provider-server提供的票，要去注册中心拿到服务
    @DubboReference//远程应用指定的服务，它会完全按照全类名进行匹配，看谁给注册中心注册了这个全类名，com.joker.service.TicketService
    TicketService ticketService;

    public void buyTicket(){
        String ticket = ticketService.getTicket();
        System.out.println("在注册中心拿到了"+ticket);
    }
}
```

5. 测试类编写；

```java
@SpringBootTest
class ConsumerServerApplicationTests {
    @Autowired
    UserService userService;
    @Test
    void contextLoads() {
        userService.buyTicket();
    }

}

```

#### 启动测试

1. 开启zookeeper

2. 打开dubbo-admin实现监控【可以不用做】,只是用来可视化监控的，对执行没影响

3. 开启服务者

4. 消费者消费测试，结果：

![image-20210318022436139](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030605114.png)



ok , 这就是SpingBoot + dubbo + zookeeper实现分布式开发的应用，其实就是一个服务拆分的思想；



#### 总和：

项目目录结构：

<img src="https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030402457.png" alt="image-20210318023711044" style="zoom:50%;" />



# 22、springBoot总结和展望

![image-20210318030108337](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030455404.png)

![image-20210318030402457](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030710964.png)

![image-20210318030455404](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318030735743.png)

![image-20210318030605114](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210318132644484.png)

![image-20210318030710964](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210316122206626.png)

![image-20210318030735743](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210316122229040.png)

