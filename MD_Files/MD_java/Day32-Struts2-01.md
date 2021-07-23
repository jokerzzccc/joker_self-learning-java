# Part01-Struts

- 2021.4.21
- 学习视频：尚硅谷

# Struts2 概述

- struts2 框架应用javaee三层结构中web层框架

- struts2 框架在struts1和webwork基础之上发展全新的框架

- struts2 解决的问题（添加一个过滤器）

  ![01-struts2概述](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\01-struts2概述.png)

  - 注意：这里的aciton,就是html 里表单标签 form 的 action 属性

- 使用版本：Struts-2.3.24-all.zip

- web层常见框架

  - struts2
  - springMVC



# Struts2 入门

- 解压 安装包,目录
  - apps: 示例文档
  - struts2 相关文档：入门文档，API 文档，。。
  - lib : 核心类库以及第三方插件库
  - src : Struts2 的源代码
  - 

1. 导入 jar 包

   （1）在lib中有jar包，不能把这些jar都导入到项目中

   （2）到apps目录里面，找到示例程序，从示例程序复制jar包



## Struts2 的执行流程

![image-20210421202255337](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210421202255337.png)

## Struts2 主要内容分析

### struts.xml 配置文件

一个完整的struts.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
  "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
  "http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <!--<constant>元素用常量的配置-->
    <constant name="struts.enable.DynamicMethodInvocation" value="false" />
    <constant name="struts.devMode" value="true" />
    <!--<package>元素用于包配置-->
    <package name="default" namespace="/" extends="struts-default">
        <!--配置Action-->
        <action name="index" class="Xxx"/>
            <!--配置Result-->
            <result type="dispatcher">
                <param name="location">/index.jsp</param>
            </result>
        </action>
    </package>
    <!-- <include>元素用于包含配置 -->
    <include file="example.xml"/>
</struts>
```



### Action 控制类

Action 是用于处理请求操作的，它是由 StrutsPrepareAndExecuteFilter 分发过来的。



在 [Struts2](http://c.biancheng.net/struts2/) 框架中，Action 是框架的核心类，被称为业务逻辑控制器，主要用于实现对用户请求的处理。

一个 Action 类代表一次请求或调用，每个请求的动作都对应一个相应的 Action 类。也就是说，用户的每次请求，都会转到一个相应的 Action 类中，由这个 Action 类进行处理。简而言之，Action 就是用于处理一次用户请求的对象。

#### Action 实现方式

实现 Action 控制类通常采用两种方式，分别是实现 Action 接口和继承 ActionSupport 类。

##### 实现 Action 接口





##### 继承 ActionSupport 类 （更常用）

ActionSupport 是 Action 接口的默认实现类，所以继承 ActionSupport 就相当于实现了 Action 接口。除 Action 接口以外，ActionSupport 类还实现了 Validateable、ValidationAware、TextProvider、LocaleProvider 和 Serializable 等接口，这为用户提供了更多的功能。



### Result 结果类型



# Struts2 helloworld

1. 建立一个web application
2. WEB-INF 目录下要有



## 总结

- struts2 的控制器就是 Action

- struts2 的值封装在 ValueStack

- struts2 可以用自己的标签来代替部分难看的 jsp 脚本.

  - struts2 的标签

  ```jsp
  <%@taglib prefix="s" uri="/struts-tags" %>
  ```



# struts2 的标签

struts2 标签官网：https://struts.apache.org/tag-developers/

- Struts Tags
  - [Generic Tags](https://struts.apache.org/tag-developers/generic-tags)
  - [UI Tags](https://struts.apache.org/tag-developers/ui-tags)
  - [Themes and Templates](https://struts.apache.org/tag-developers/themes-and-templates)
  - [Tag Reference](https://struts.apache.org/tag-developers/tag-reference)
  - Ajax Tags
    - [Ajax and JavaScript Recipes](https://struts.apache.org/tag-developers/ajax-and-javascript-recipes)
- OGNL
  - [OGNL Basics](https://struts.apache.org/tag-developers/ognl-basics)
  - [OGNL Expression compilation](https://struts.apache.org/tag-developers/ognl-expression-compilation)
- [Tag Syntax](https://struts.apache.org/tag-developers/tag-syntax)
- [Alt Syntax](https://struts.apache.org/tag-developers/alt-syntax)
- JSP
  - [specific tags](https://struts.apache.org/tag-developers/jsp-tags)
- FreeMarker
  - [specific tags](https://struts.apache.org/tag-developers/freemarker-tags)
- Velocity
  - [specific tags](https://struts.apache.org/tag-developers/velocity-tags)



[Struts2](http://c.biancheng.net/struts2/) 是一个优秀的 MVC 框架，其实现重点主要放在了业务逻辑控制器部分和视图页面部分。

- **控制器部分**主要由 **Action** 提供支持，
- 而**视图页面部分**则由大量的**标签**提供支持。

Struts2 的标签库是一个比较完善且功能强大的标签库，它将所有标签都统一到一个标签库中，从而简化了标签的使用；它提供了对主题和模板的支持，极大地简化了视图页面代码的编写；它还提供了对 Ajax 的支持，极大地丰富了视图页面的展示效果。



struts2 的标签引入jsp

```jsp
<%@taglib prefix="s" uri="/struts-tags" %>
```

## struts2 的标签分类

![image-20210422144236902](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422144236902.png)

### 普通标签（generic tags)

#### **Control Tags**



#### Data Tags





### UI 标签 (UI tags)

#### Form Tags



#### Non-Form Tags



### Ajax Tags



# struts2 基本流程

struts2 框架由3个部分组成：

- 核心控制器 FilterDispatcher
- 业务控制器 （Action 类）
-  用户实现的业务逻辑组件（JSP）

## 核心控制器 FilterDispatcher



## 业务控制器 （Action 类）





# ONGL

OGNL 的全称是“Object-Graph Navigation Language”，即对象图导航语言，它是一种功能强大的开源表达式语言。使用这种表达式语言可以通过某种表达式语法存取 [Java](http://c.biancheng.net/java/) 对象的任意属性，调用 Java 对象的方法，以及实现类型转换等。



# struts2 的基本配置



## 配置web.xml

- 主要就是配置 filter ,使得 struts2 框架可以被容器调用



## 配置 struts.xml 

- 
- 主要负责管理业务控制器 Action 类的映射

- 为了避免过于该文件过于臃肿，可以使用 include属性，来包含其它配置文件



结合 spring 只需要在lib 目录下 放一个jar 包



## struts.properties 配置文件

- 定义了struts2 框架的大量属性（struts2 常量）
- 开发人员可以通过改变这些常量的值，来满足应用需求



struts2 框架里配置常量值的地方有三个：

- struts.properties

- web.xml 文件中配置 FilterDispatcher 指定初始化参数

- struts.xml 中使用 

  ```xml
  - <constant name="" value=""></constant>
  ```



## struts.xml 的文件结构

结构：

```xml
<struts>
	<constant></constant>
    <bean></bean>
    <include></include>
    <package></package>
    	<action></action>
    		<result></result>
</struts>
```



![image-20210422110258322](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422110258322.png)

![image-20210422110402436](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422110402436.png)

![image-20210422110422076](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422110422076.png)

![image-20210422110445888](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422110445888.png)

![image-20210422110501465](D:\鼎信开发文档资料\鼎信_MD_File\Part01-Struts.assets\image-20210422110501465.png)



# 深入 struts2 的配置文件

## bean 配置

对于绝大部分 struts2 应用，都不需要 更改核心组件 bean



- struts2 框架 用自己的 ioc 容器来管理框架的核心组件。
- bean 配置，就是用来自己扩展核心组件的。
- 比如 struts-default.xml 里就有很多 bean



## 常量配置

==建议在 struts.xml 里配置常量==，而不是在struts.properties 里配置或者web.xml 里配置常量。



## 包配置

- struts2 用包来管理 Action , 拦截器等。

package 里只有name 是必须的。



| Attribute | Required | Description                                                  |
| :-------- | :------- | :----------------------------------------------------------- |
| name      | **yes**  | key to for other packages to reference                       |
| extends   | no       | inherits package behavior of the package it extends          |
| namespace | no       | see [Namespace Configuration](https://struts.apache.org/core-developers/namespace-configuration.html) |
| abstract  | no       | declares package to be abstract (no action configurations required in package) |



## 命名空间配置

- package 的 namespace 属性
- 默认是“”
- 原因：一个web 应用 中需要同名的 Action , Struts2 以命名空间的方式来管理 Action 。
- 同一个命名空间里不能有同名的 Action ,不同的命名空间可以有同名的 Action.



## 包含配置

- 所包含的 xxx.xml 配置文件，也是标准的struts2 的配置文件。
- 通常 struts2 所有的配置文件都放在 WEB-INF/classes 目录下。

```xml
<include file="xxx.xml"></include>
```



## 拦截器配置

拦截器实在 package 之下

```xml
<interceptors>
    <interceptor name="" class=""></interceptor>
    <interceptor-stack name="">
         <interceptor-ref name="">
               <param name="">
               </param>
         </interceptor-ref>
    </interceptor-stack>
</interceptors>
```





- 拦截器就是 AOP 的思想。拦截器允许在处理 Action 之前或之后插入开发者自定义的代码。

拦截器完成的功能一般是：

- 权限控制
- 跟踪日志
- 跟踪系统的性能瓶颈



struts2 把多个拦截器组合在一起，就是一个**拦截器栈**。

- 一个拦截器栈可以包含多个拦截器。
- 对于 struts2 系统而言，拦截器栈对外 也表现成一个拦截器。



**建议**：使用小粒度的拦截器，可以提高拦截器的复用性。



