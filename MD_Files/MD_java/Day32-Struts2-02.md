# Part01-Struts2-video

学习资源：

- struts2 官网：https://struts.apache.org/index.html
- 中文，教程：http://c.biancheng.net/struts2/

# 入门案例

1. 添加struts2相关依赖

2. 修改web.xml,加载struts2配置

3. 编写index.jsp页面

4. 编写Action控制器

5. 编写struts.xml文件

# 实现用户登录案例



# Action 接口

- 一般不会实现这个接口，而是实现 ActionSupport 类

```java
public interface Action {
    String SUCCESS = "success";//成功
    String NONE = "none";//无
    String ERROR = "error";//错误
    String INPUT = "input";//未输入数据
    String LOGIN = "login";//登录

    String execute() throws Exception;
}
```



# struts2 访问Servlet API

- 就是为了用 session 保存用户信息
- 访问方式有两种
  - struts2 与 Servlet API 解耦
  - struts2 与 Servlet API 耦合



## 解耦访问

通过 ActionContext





## 耦合访问(不要用)

通过 ServletActionContext

还需要装一个依赖 javax.servlet-api



# 数据校验

第1步：控制器Action继承 ActionSupport 类 

第2步：控制器Action重写 validate() 方法

第3步：在页面中使用 <s:fielderror> 标签显示验证信息

第4步：在 struts.xml 文件的action标签中配置 input 返回结果



- 注意 struts2 的大小写严格区分

# struts2 标签

## 通用标签

最多用的就

- 条件标签
  - <s:if>
  - <s:else>
  - <s:elseif>
- 迭代 标签
  - <s:iterator>

## UI 标签

### 表单标签



### 非表单标签



### Ajax 标签



# Struts2 配置

## Struts2 基本架构

1. 浏览器将请求发送到服务器

2. 服务器接收请求，根据web.xml配置文件中，找到struts2的核心过滤器

3. 核心过滤器会将请求传递给struts.xml文件，struts2会根据action的配置找到相应的

Action控制器，根据execute方法返回的结果字符串找result标签

4. 根据result标签的配置响应到jsp页面

![image-20210423171235103](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210423171235103.png)

### web.xml



### Action



### Result



## Struts.xml 配置

- constant
- include
- package



### 加载顺序

![image-20210423172034902](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210423172034902.png)



## Action 配置

### action 作用：

- 封装工作单元
- 数据转移场所（就是值栈）
- 返回结果字符串



### 访问 Action 的三种方式

#### method 属性



#### Action 动态方法调用

- 这是在**jsp页面使用**

- 通过动态方法调用方法可以减少Action标签的配置数量，其用法为在jsp页面的路径使用： actionName!methodName.action

禁用动态方法调用：将常struts.enable.DynamicMethodInvocation 设置为 false 。

#### 通配符（*）调用

- 这是**在struts.xml 里使用**
- 另一种形式的动态方法调用
- 

![image-20210423173758999](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210423173758999.png)



## Result 配置

### 常用的结果类型(type)

- dispatcher类型：**默认**结果类型，后台使用RequestDispatcher转发请求

- redirect类型 ：后台使用的sendRedirect()将请求重定向至指定URL 

- redirectAction类型 ：主要用于重定向到Action 



注意：

![image-20210423175022385](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210423175022385.png)



### 全局结果配置

概述：通过标签配置全局结果，注意该标签的顺序。

作用：定义公共的result返回页面，为了减少 <result> 标签配置的冗余

注意：当全局结果与局部结果配置冲突时，优先使用局部结果

```xml
<!-- 配置全局结果 --> 
<global-results> 
    <result name="login">/login.jsp</result> 		<result name="error">/error.jsp</result> 
</global-results>
```



# struts2 里获取表单数据方式

在struts2获取表单数据或提交路径的参数值的方式有4种，分别是· 

- 原始Servlet方式 、 
- 属性封装 、 
- 表达式封装 、 
- 模型驱动封装（最方便，用着最好）

### 原始Servlet方式



### 属性封装





### 模型驱动封装

使用模型驱动封装的步骤如下：

第1步：控制器实现 ModelDriven<T> 

接口 

第2步：重写 getModel() 方法 

```java
/*** 模型驱动封装 */ 
public class UserAction3 extends ActionSupport implements ModelDriven<User> { 
    //创建用户对象 
    private User user = new User(); 
    @Override 
    public User getModel() { 
        return user;//返回用户对象 
    }
    /*** 注册 * @return */ 
    public String register(){ 		System.out.println(user); return SUCCESS; 
                            } 
}
```



**注意** 

1. input标签的name属性值、提交路径的参数名称必须与实体类的属性名相同(严格区分大

小写)，控制器不需要提供模型对象的get、set方法

2. 模型驱动封装与属性封装一起使用时，同名优先使用模型驱动封装，属性封装无法获取数据



### 表达式封装

使用表达式封装的方式获取表单数据，步骤如下：

第1步：在控制器中 定义类类型的成员变量 ，提供get、set方法如：

第2步：页面使用 对象名.实体类属性名





### 比较表达式封装和模型驱动封装** 

**共同点**

使用表达式封装和模型驱动封装都可以把数据封装到实体类对象里面

**不同点** 

1. 使用模型驱动只能把数据封装到一个实体类对象里面

注意：在一个action里面不能使用模型驱动把数据封装到不同的实体类对象里面

2. 使用表达式封装可以把数据封装到不同的实体类对象里面



# OGNL 表达式

## OGNL 作用

OGNL 在struts2 中两个作用:

- 类型转换
- 表达式语言



## 值栈与 OGNL 



# struts2 拦截器

**为什么**使用拦截器：

Struts 2将核心功能放到多个拦截器中实现，拦截器可自由选择和组合，增强了灵活性，有利于系统的解耦。



**拦截器简介**：

- Struts 2大多数核心功能是通过拦截器实现的，每个拦截器完成某项功能拦截器方法在Action执行之前和之后执行拦截器栈
  - 从结构上看，拦截器栈相当于多个拦截器的组合
  - 在功能上看，拦截器栈也是拦截器
- 拦截器与过滤器原理很相似
- 为Action提供附加功能时，无需修改Action代码，使用拦截器来提供

![image-20210424140108472](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210424140108472.png)



## 拦截器的使用

### 自定义创建拦截器：

自定义拦截器类可以继承 

AbstractInterceptor 或 MethodFilterInterceptor



### 配置拦截器

```xml
<interceptors>
            <interceptor />
            
            <interceptor-stack >
                <interceptor-ref />
                
                    <param </param>
                    <param </param>
                </interceptor-ref>
            </interceptor-stack>
        </interceptors>
```

注意：在struts.xml配置文件中的<package>标签 下定义拦截器

### 引用拦截器

<interceptor-ref>

在action标签下引用

引用自定义的拦截器之后==必须引用默认拦截器== 

```xml
<interceptor-ref name="defaultStack"/>
```



## struts2 的默认拦截器

可以在 struts2-core jar包里 struts.xml 里找到 defaultStack



## 拦截器栈

- 就是为了可以自由组合拦截器，
- 相当于一个大 的拦截器



## 实现自定义登录拦截器

- 有个login.jsp
- UserAction 类
- 写拦截器
- 配置 struts.xml



# 文件上传

Commons是Apache开放源代码组织的一个Java子项目，其中的FileUpload是用来处理HTTP文件上传的子项目

Commons-FileUpload组件特点

1. 使用简单：可以方便地嵌入到JSP文件中，编写少量代码即可完成文件的上传功能

2. 能够全程控制上传内容

3. 能够对上传文件的大小、类型进行控制



注意：在struts2中， struts-core 依赖已经包含 commona-fileupload 组件的相关依赖，所以在struts2的项目中无需导入 

commona-fileupload 组件的相关依赖。

## 单文件上传

### upload.jsp 上传页面

**页面三要素**：

1. form表单的提交方式必须是 post 提交

2. form表单必须设置 enctype="multipart/form-data" 属性

3. form表单需要提供文件域组件( type="file" ) 

### Action 控制器

在Action控制器中需要提供3个成员变量，分别是**文件、文件类型、文件名称**，代码如下所示：



## 多文件上传



# 文件下载



# struts2 开发问题记录

1. ignoreHierarchy 参数：
   表示**是否忽略等级**，也就是继承关系，比如：TestAction 继承于 BaseAction，那么 TestAction 中返回的 json 字符串默认是不会包含父类 BaseAction 的属性值，ignoreHierarchy 值默认为 true，设置为 false 后**会将父类和子类的属性一起返回**。 
2. 