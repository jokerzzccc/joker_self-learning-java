# JavaWeb

# 1、基本概念

## 1.1 前言

web开发：

- 静态web:
  - html，css
  - 提供给所有人看的数据始终不会发生变化
- 动态web:
  - 几乎所有的网站；
  - 每个人在不同的时间，不同的地点，看到的信息各不相同
  - 技术栈：Servlet/JSP, ASP, PHP

在Java中，动态web资源开发的技术统称为**JavaWeb**

## 1.2 web 应用程序

web 应用程序：可以提供浏览器访问的程序

- 一个web应用程序由多部分组成：
  - html,css,js
  - jsp,servlet
  - java程序
  - jar包
  - 配置文件（properties）
- web应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理

## 1.3 静态web

- *.htm , *.html , 网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取，通信
- 静态Web存在的缺点：
  - web页面无法动态更新，所有用户看到的都是同一个页面
    - 轮播图，点击特效：伪动态
    - JavaScript【实际开发中，用的最多】
    - VBScript
  - 它无法和数据库交互，（数据无法持久化，用户无法交互）

## 1.4 动态web

页面会动态展示：web的页面展示因人而异

缺点：

- 加入服务器的动态web资源出现了错误我们需要重新编写我们的后台程序，重新发布
  - 停机维护

优点：

- 可以动态更新
- 可以与数据库交互（数据持久化：比如，注册，商品信息，用户信息等）

# 2、 web 服务器

## 2.1 技术讲解

**ASP**

- 微软的，国内最早流行的
- 在html中嵌入了VB的脚本，ASP+COM
- 在ASP开发中，基本一个页面都有几千行的业务代码，页面及其混乱；
- 维护成本高
- C#
- IIS （一种web服务器）

**PHP**

- 开发速度很快，功能很强大，跨平台，代码很简单，
- 无法承载大访问量的情况；（局限性 ）

**JSP**

- B/S架构，
- 基于java语言（所有的大公司，或者一些开源的组件，都是用Java写的）
- 可以承载三高问题带来的影响（高并发，高可用，高性能）
- 语法像ASP

...........

## 2.2 web服务器

服务器是一种被动的操作，用来处理用户的一些请求，和给用户一些响应信息；

### **IIS**

微软的；跑一些ASP程序，windows中自带的

### Tomcat

- 对于java初学web的人来说，它是最佳选择，在中小型系统，和并发访问用户不是很多的场合使用，是开发和调试JSP 程序的首选
- Tomcat 实际上运行JSP页面和Servlet
- 下载tomcat：
  - 安装，解压
  - 了解配置文件及目录结构
  - 这个东西的作用

# 3、 tomcat

## 3.1 安装启动

- 文件夹作用：

  ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/tomcat目录.png)

启动和关闭tomcat

- bin/startup.bat
- bin/shutdown.bat

访问测试，默认端口：http://localhost:8080/

可能遇到的问题：

- java环境变量
- 闪退问题：需要配置兼容性
- 乱码：配置文件中设置，尽量不要动

## 3.2 配置

核心配置文件：conf/server.xml

可以配置主机的端口号

- tomcat 默认端口：8080

- mysql 3306

- http 80

- https 443

  ```xml
  <Connector port="8080" protocol="HTTP/1.1"
                 connectionTimeout="20000"
                 redirectPort="8443" />
  ```

  



可以配置主机的名称:

- 默认的主机名：localhost,即127.0.0.1
- 默认网站应用存放位置webapps

```xml
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
```

### **高难度面试题：**

请你谈谈网站是如何进行访问的？

1. 输入一个域名；回车

2. 检查本机的 C:\Windows\System32\drivers\etc\hosts 配置文件下有没有这个域名映射；

   1. 有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

      比如，在文件默认添加，127.0.0.1      www.joker.com

   2. 没有，去DNS服务器找，找到就返回，找不到就返回找不到

   ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/网站请求过程.png)

## 3.3 发布一个web网站

直接在webapps里建一个文件夹，放入内容，就行：

- 将自己写的网站，放到服务器的（tomcat）中指定的 web 应用的的文件夹（webapps）下，就可以访问了

网站应该有的结构

```java
-- webapps : Tomcat 服务器的web目录
    - ROOT
    - joker:网站的目录名
        - WEB-INF
        	- classes :java程序
            - lib:web应用程序依赖的jar包
            - web.xml 网站配置文件
        - index.html 默认的首页
        - static 静态资源文件
             -css
                -style.css
             -js
             -img 
        - ......        
            
     
```



JSP ,Servlet 示例和源码演示：http://localhost:8080/examples/

# 4、Http

## 4.1 什么是Http

Http（超文本传输协议）是一个简单的请求协议，通常运行在TCP之上

- 文本：html , 字符串。。。。
- 超文本：图片，音乐，视频，定位，地图。。。
- 默认端口 80

Https: 安全的

- 443

## 4.2 Http 两个时代

- http/1.0
  - 客户端可以与web服务器连接后，只能获得一个web资源,断开连接
- http/1.1
  - 客户端可以与web服务器连接后，可以获得多个web资源

## 4.3 Http请求（Request）

- 客户端---发请求---服务器

### 1. 请求行

- 请求行中的请求方式：GET
- 请求方式有：**get , post** , head , delete , put , tract....
  - get : 请求能够携带的参数比较少，大小有限制，会在浏览器的url地址栏显示数据内容，不安全，但效率高
  - post： 请求能够携带的参数没有限制，大小没有限制，不会在浏览器的url地址栏显示数据内容，安全，但不高效

### 2. 消息头



## 4.4 Http响应（Responce)

- 服务器---响应---客户端

- 百度的：

- ```xml
  Cache-Control: private
  Connection: keep-alive
  Content-Encoding: gzip
  Content-Type: text/html;charset=utf-8
  ```

- 

### 1. 响应体

```java
Accept: 告诉浏览器，它所支持的数据类型
Accept-Encoding: 支持哪种编码格式
Accept-Language: 告诉浏览器，它的语言环境
Connection: 告诉浏览器，请求完成是断开还是保持连接

Host:主机
Refresh:告诉客户端，多久刷新一次
Location:网页重新定位
```



### 2. 响应状态码（重点)

200 ：请求响应成功

3xx :  请求重定向

- 重定向：你重新到我给你的新位置去；

4xx :  404 找不到资源

- 资源不存在

5xx :  

- 500 服务器代码错误 
- 502 网关错误

### 常见面试题

当你的浏览器地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？





# 5、Maven

用处：

​		在javaweb 开发中，需要使用的大量的jar包，为了不手动导入，Maven 可以自动帮助导入和配置这些jar包

## 5.1 Maven 项目架构管理工具

我们目前用来就是方便导入jar包

maven 的核心思想： **约定大于配置**

- 有约束，不要去违反

Maven 会规定好你该如何编写java代码，必须按照这个规范来

## 5.2 下载安装Maven

### 下载：

https://mirrors.bfsu.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip

### 配置环境变量

- M2_HOME maven的bin目录
- MAVEN_HOME maven目录
- 系统的PATH 下添加 %MAVEN_HOME%\bin

测试是否安装成功：cmd里输入 mvn -version

### 阿里云镜像

- 镜像 mirrors :

  - 作用：加速我们的下载

- 国内建议使用阿里云的镜像

- 在conf/settings.xml 文件中的mirror节点下添加

- ```xml
  <mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
  </mirror>
  ```



### 本地仓库

在本地的仓库，先在maven目录下新建一个maven-repo文件夹，再在conf/settings.xml 文件中的setting 节点中添加

```xml
<localRepository>D:\apache-maven-3.6.3\maven-repo</localRepository>
```



## 5.3 在IDEA中使用Maven

- 不要使用IDEA自带的Maven配置（性能不好），要使用本地配置的，

1. 创建一个Maven Web项目（使用带模板的）

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-09-46.png)

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-01-45.png)

2. 等待项目资源下载完毕

3. IDEA中的Maven配置

- 注意：IDEA项目创建成功后，看一眼Maven的配置

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-00-20.png)

4. 创建完成。一个Maven web应用

   ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_18-55-29.png)

## 5.4 创建一个普通的Maven项目

不使用模板直接创建一个普通的Maven项目：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-06-54.png)



## 5.5 标记文件夹功能

- 在Maven web项目中，为了变完整，新建，Java，resources 文件夹，并标记文件夹功能

![]()![Snipaste_2021-03-01_19-11-20](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_20-02-19.png)

target目录：就是，java-->class文件的目录，之前java 项目里是out目录

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-11-20.png)



## 5.6 Maven 标签的内容：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-01_19-15-45.png)

## 5.7 在IDEA中配置Tomcat

- 专业版才有tomcat server,社区版没有，不过可以下载在edit configurations的插件的smart tomcat 来安装tomcat，运行直接点击绿三角
- 直接访问http://localhost:8080/javaweb-01-maven01
- 这里访问到的hello world 就是默认的index.jsp文件中的内容

## 5.8 pom.xml 文件（核心配置文件）

Maven 的核心配置文件

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/pom.png)

- Maven 的高级之处在于，它会帮助导入这个jar包所依赖的其它jar包

- 注意：Maven可能存在的问题，
  - 由于约定大于配置，自己写的配置文件可能遇到无法导出或者生效的问题，解决方案
  - ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/pom_problem.png)

```xml
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



## 5.9 解决遇到的问题

- maven默认web项目中web.xml的版本问题
- ![image-20210301204153196](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210302151134891.png)

替换为tomcat，webapps\ROOT\WEB-INF\web.xml保持版本一致

![image-20210301204503167](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210301204503167.png)

如图中已经报红，则需要添加servlet-api的jar包即可，project structure|libraries|+|tomcat下的servlet-api.jar

![image-20210302151134891](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210301204153196.png)

# 6 Servlet

## 6.1 概念

- 就是一种开发动态web的技术
- Sun提供了一个接口 ： Servlet ，
- 开发一个Servlet程序，只需要两个步骤
  -  编写好一个类，实现Servlet 接口
  - 把开发好的Java 类部署到web服务器中
- **把实现了Servlet 接口的程序叫做，Servlet**
- Servlet 接口有两个默认的实现类：HttpServlet ， GenericServlet

## 6.2 HelloServlet

1. 构建一个Maven项目，删掉src目录，以后就在这个项目里建立Module，(点击项目名-》new module) ; 这个空工程就是Maven的主工程。

2. 关于Maven父子工程的理解：

   - 父项目中多了

     ```xml
         <modules>
             <module>servlet-01</module>
         </modules>
     
     ```

   - 子项目中会有

     ```xml
         <parent>
             <artifactId>javaweb-02-servlet</artifactId>
             <groupId>org.joker</groupId>
             <version>1.0-SNAPSHOT</version>
         </parent>
     ```

   - 父项目中的jar包子项目可以直接使用

3. Maven环境优化

   1. 修改web.xml为最新的
   2. 将maven的结构搭建完整（目录，java,resources）

4. 编写一个Servlet 程序

   1. 编写一个普通的类

   2. 实现Servlet接口，这里我们直接继承HttpServlet

      ```java
      public class HelloServlet extends HttpServlet {
          //由于 get 或post 只是请求实现的不同方式，可以互相调用，业务逻辑都一样
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              PrintWriter writer = resp.getWriter();//响应流
              writer.print("hello,Servlet");
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              super.doPost(req, resp);
          }
      }
      
      ```

   Servlet 接口里最重要的方法就是Service()

   ```java
   void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
   // 只在子类HttpServlet里实现
   ```

5. 编写Servlet的映射

   - 为什么需要映射：我们写的是Java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以，我们需要在web服务器中注册我们写的Servlet,还需要给它一个浏览器可以访问的路径

   - 在在项目的web.xml里配置

   - ```xml
     <!--注册Servlet-->
         <servlet>
             <servlet-name>hello</servlet-name>
             <servlet-class>com.joker.servlet.HelloServlet</servlet-class>
         </servlet>
     <!--Servlet的请求路径-->
         <servlet-mapping>
             <servlet-name>hello</servlet-name>
             <url-pattern>/hello</url-pattern>
         </servlet-mapping>
     </web-app>
     
     ```

   - 

6. 配置Tomcat

7. 启动测试，改成了tomcat9，但是，smart tomcat里的context path,不能和之前的tomcat10的一样，不然会报错

## 6.3 Servlet 原理

Servlet 是由web服务器调用，web服务器在收到浏览器请求之后，会：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/servlet原理2.png)

## 6.4 Mapping

1. 一个Servlet指定单个映射路径

   ```xml
   <!--Servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

2. 一个Servlet指定多个映射路径

   ```xml
   <!--Servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello3</url-pattern>
       </servlet-mapping>
   ```

3. 一个Servlet指定通用映射路径

   ```xml
   <!--Servlet的请求路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
       </servlet-mapping>
   ```

4. 默认请求路径(优先级高于首页index.jsp，所以不要这么写)

   ```xml
   <servlet-mapping>
       <servlet-name>hello</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>
   ```

   

5. 一个Servlet指定单个映射路径

   ```xml
   <!--    可以自定义后缀实现请求映射，
   注意点：*. 前面不能加项目映射的路径，
   但是在url 输入路径时可以，比如dkfjd/kkkk.joker
   通常都是用*.do-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.joker</url-pattern>
       </servlet-mapping>
   ```

6. 优先级问题：

- 指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

- 编写一个error类

- ```java
  public class ErrorServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          resp.setContentType("text/html");
          resp.setCharacterEncoding("utf-8");
  
          PrintWriter writer = resp.getWriter();
          writer.print("<h1>404</h1>");
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          super.doPost(req, resp);
      }
  }
  ```

- 添加Servlet

- ```xml
  <!--404-->
      <servlet>
          <servlet-name>error</servlet-name>
          <servlet-class>com.joker.servlet.ErrorServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>error</servlet-name>
          <url-pattern>/*</url-pattern>
      </servlet-mapping>
  ```

## 6.5 ServletContext

web容器启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用

 ServletContext的**作用**：

### 1. 共享数据：

在一个Servlet中保存的数据，可以在另一个Servlet中拿到

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/cookie.png)

- 三个步骤实现共享例子

- 第一个Servlet

- ```java
  public class HelloServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          ServletContext servletContext = this.getServletContext();//返回Servlet上下文
          String username = "joker陈";
          servletContext.setAttribute("username",username);//保存一个数据在ServletContext中
      }
  }
  ```

- 第二个Servlet

- ```java
  public class GetServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          ServletContext servletContext = this.getServletContext();
          String username =(String) servletContext.getAttribute("username");
  
          resp.setContentType("text/html");
          resp.setCharacterEncoding("utf-8");
          resp.getWriter().print("名字：" + username);
      }
  }
  
  ```

- 配置xml

- ```xml
      <servlet>
          <servlet-name>hello</servlet-name>
          <servlet-class>com.joker.servlet.HelloServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>hello</servlet-name>
          <url-pattern>/hello</url-pattern>
      </servlet-mapping>
      <servlet>
          <servlet-name>getc</servlet-name>
          <servlet-class>com.joker.servlet.GetServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>getc</servlet-name>
          <url-pattern>/getc</url-pattern>
      </servlet-mapping>
  
  ```

- 测试结果

### 2. 获取初始化参数

第一步:xml文件

```xml
<!--    配置一些web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost</param-value>
</context-param>
```

第二步：拿参数

```java
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        String username =(String) servletContext.getAttribute("username");

        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().print("名字：" + username);
    }
}
```

第三步：xml

```xml
    <servlet>
        <servlet-name>getParam</servlet-name>
        <servlet-class>com.joker.servlet.Demo03</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>getParam</servlet-name>
        <url-pattern>/getParam</url-pattern>
    </servlet-mapping>

```

测试结果

### 3. 请求转发 getRequestDispatcher（）

- 注意转发与重定向的区别：
  - 转发时，url显示不会变

```java
public class Demo04 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/getParam");//转发的请求路径
        requestDispatcher.forward(req,resp);//调用forward()实现请求转发
    }
}
```

```xml
    <servlet>
        <servlet-name>rp</servlet-name>
        <servlet-class>com.joker.servlet.Demo04</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>rp</servlet-name>
        <url-pattern>/rp</url-pattern>
    </servlet-mapping>

```

### 4. 读取资源文件（Properties）

properties:

- 在Java目录下，新建properties
- 在resources目录下，新建properties

发现：都被打包到 了同一个路径下，classes,俗称这个路径为classpath

思路：需要一个文件流

```properties
username=root
password=123456
```

```java
public class Demo05 extends HelloServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/db.properties");//路径根据target里的相对路径
        Properties prop = new Properties();
        prop.load(resourceAsStream);
        String username = prop.getProperty("username");
        String pwd = prop.getProperty("password");

        resp.getWriter().print(username + ":"+pwd);
    }
}
```

```xml
    <servlet>
        <servlet-name>d5</servlet-name>
        <servlet-class>com.joker.servlet.Demo05</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>d5</servlet-name>
        <url-pattern>/d5</url-pattern>
    </servlet-mapping>
```



## 6.6 HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，一个代表响应的HttpServletResponse；

- 如果要获取客户端请求过来的参数，找HttpServletRequest
- 如果要给客户端响应一些信息，找HttpServletResponse

### 1. 简单分类

负责向浏览器**发送数据**的方法：(最常用的)

```java
ServletOutputStream getOutputStream() throws IOException;

PrintWriter getWriter() throws IOException;
```

负责向浏览器**发送响应头**

```java
// 父类ServletResponse的
void setCharacterEncoding(String var1);

void setContentLength(int var1);

void setContentLengthLong(long var1);

void setContentType(String var1);
//自己的
    void setDateHeader(String var1, long var2);

    void addDateHeader(String var1, long var2);

    void setHeader(String var1, String var2);

    void addHeader(String var1, String var2);

    void setIntHeader(String var1, int var2);

    void addIntHeader(String var1, int var2);


```

响应的**状态码**：(知道几个常用的就行，)

```java
int SC_CONTINUE = 100;
int SC_SWITCHING_PROTOCOLS = 101;
int SC_OK = 200;
int SC_CREATED = 201;
int SC_ACCEPTED = 202;
int SC_NON_AUTHORITATIVE_INFORMATION = 203;
int SC_NO_CONTENT = 204;
int SC_RESET_CONTENT = 205;
int SC_PARTIAL_CONTENT = 206;
int SC_MULTIPLE_CHOICES = 300;
int SC_MOVED_PERMANENTLY = 301;
int SC_MOVED_TEMPORARILY = 302;
int SC_FOUND = 302;
int SC_SEE_OTHER = 303;
int SC_NOT_MODIFIED = 304;
int SC_USE_PROXY = 305;
int SC_TEMPORARY_REDIRECT = 307;
int SC_BAD_REQUEST = 400;
int SC_UNAUTHORIZED = 401;
int SC_PAYMENT_REQUIRED = 402;
int SC_FORBIDDEN = 403;
int SC_NOT_FOUND = 404;
int SC_METHOD_NOT_ALLOWED = 405;
int SC_NOT_ACCEPTABLE = 406;
int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
int SC_REQUEST_TIMEOUT = 408;
int SC_CONFLICT = 409;
int SC_GONE = 410;
int SC_LENGTH_REQUIRED = 411;
int SC_PRECONDITION_FAILED = 412;
int SC_REQUEST_ENTITY_TOO_LARGE = 413;
int SC_REQUEST_URI_TOO_LONG = 414;
int SC_UNSUPPORTED_MEDIA_TYPE = 415;
int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
int SC_EXPECTATION_FAILED = 417;
int SC_INTERNAL_SERVER_ERROR = 500;
int SC_NOT_IMPLEMENTED = 501;
int SC_BAD_GATEWAY = 502;
int SC_SERVICE_UNAVAILABLE = 503;
int SC_GATEWAY_TIMEOUT = 504;
int SC_HTTP_VERSION_NOT_SUPPORTED = 505;
```

### 2. 常见应用

1. 向浏览器输出消息（前面的都是）



#### 下载文件：

1. 获取下载文件的路径
2. 下载文件名
3. 设置浏览器可以支持下载我们的东西
4. 获取下载文件的数据流
5. 创建缓冲区
6. 获取OutputStream对象
7. 将FileOutputStream流写入到缓冲区（buffer）
8. 使用OutputSteam 将缓冲区中的数据输出到客户端

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1. 获取下载文件的路径
        String path = "D:\\2021_java_project\\javaweb\\javaweb-02-servlet\\responce\\src\\main\\resources\\t2.png";
        System.out.println("下载文件的路径：" + path);
        //2. 下载文件名
        String fileName = path.substring(path.lastIndexOf("\\" )+ 1);
        //3. 设置浏览器可以支持下载我们的东西(百度找头消息)
        //中午文件名需要URLEncoder.encode编码，否则可能下载文件名会乱码
        resp.setHeader("Content-Disposition","attachment;filename=" + URLEncoder.encode(fileName,"UTF-8"));
        //4. 获取下载文件的数据流
        FileInputStream inputStream = new FileInputStream(path);
        //5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024*10];
        //6. 获取OutputStream对象
        ServletOutputStream outputStream = resp.getOutputStream();
        //7. 将FileOutputStream流写入到缓冲区（buffer）,使用OutputSteam 将缓冲区中的数据输出到客户端
        while ((len = inputStream.read(buffer))>0){
                outputStream.write(buffer,0,len);
        }
        //8. 关闭
        inputStream.close();
        outputStream.close();
            }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

### 3. 验证码功能

验证实现：（没啥意义）

- 前端实现
- 后端实现，需要用到Java图片类，生成一个图片

### 4. 实现重定向

- 重定向：一个web资源B收到客户端A请求后，B通知A访问另一个Web资源C

- ```java
  void sendRedirect(String var1) throws IOException;
  ```

  

- 常见场景：

  - 用户登录

- 测试：

  ```java
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          /*
          重定向原理就是实现了如下两步
          resp.setHeader("Location","/responce/download");
          resp.setStatus(302);
           */
          resp.sendRedirect("/responce/download");
      }
  ```

#### 面试题：重定向与转发的区别

相同点：

- 页面都会实现跳转

不同点：

- getRequestDispatcher() : 请求转发的时候，url不会产生变化;307
- sendRedirect() ： 重定向的时候，url地址栏会发生变化;302

### 5. 简单实现登录重定向

- RequestDemo

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //处理请求
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println(username+" : "+password);
        //重定向的时候一定要注意路径问题，否则会404，
        resp.sendRedirect("/responce/success.jsp");
    }

```

```jsp
<!-- index.jsp -->

<html>
<body>
<h2>Hello World!</h2>
<!-- 这里提交的路径，需要寻找到项目的路径 -->
<!-- ${pageContext.request.contextPath} 代表当前项目 -->
<%@ page pageEncoding="UTF-8"%>
<form action="${pageContext.request.contextPath}/login" method="get">
    用户名：<input type="text" name="username">
    密码：<input type="password" name="password">
    <input type="submit">
</form>	
</body>
</html>

```

```jsp
<!-- success.jsp-->
<html>
<body>
<h2>Success</h2>

</body>
</html>
```



## 6.7 HttpServletRequest

- HttpServletRequest 代表客户端的请求，用户通过HTTP协议访问服务器，HTTP请求中的所有信息会被封装到 HttpServletRequest ，通过它的方法，获得客户端的所有信息。



###  获取前端传递的参数,请求转发

主要方法：

```java
String parameter = req.getParameter();
String[] parameterValues = req.getParameterValues();
```

```java
   @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //后台接收乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbies = req.getParameterValues("hobbies");

        System.out.println("===========================");
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbies));
        System.out.println("==============================");

        //通过请求转发
        //注意：这里的/代表当前web应用，对比重定向就是要写/request/success.jsp
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }
```





# 7、Cookie ,Session

## 7.1 会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个Web资源关闭浏览器，这个过程可以称之为会话

**有状态会话**：一个同学来过教室，下次再来教室，我们会知道这个同学曾经来过。这个过程，就是有状态会话

**一个网站，怎么证明你来过**：

- Cookie ：服务端给客户端一个信件，客户端下次访问带上信件就可以了
- Session ：服务器登记你来过，下次你来的时候我匹配你



## 7.2 保存会话的两种技术

**cookie**

- 客户端技术（响应，请求）

**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息。可以把信息或者数据放在Session中



常见例子：网站登录后，下次就不用登录了

## 7.3 Cookie

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/ServletContext.png)

1. 从请求中拿到cookie信息
2. 服务器响应给客户端cookie

常用方法：对应的类 Cookie

```java
 
Cookie[] cookies = req.getCookies();//从请求中获得cookie
cookie.getName();//获得cookie的key
cookie.getValue();//获得cookie的value
Cookie cookie1 = new Cookie("lastLoginTime", System.currentTimeMillis() + "");//新建一个cookie
cookie1.setMaxAge(24*60*60);//设置cookie的有效期
resp.addCookie(cookie1);//响应给客户端一个cookie

```

**cookie的细节**：

1. 一个cookie只能保存一个信息
2. 一个web站点（服务器）可以给浏览器发送多个cookie，一个web站点最多存放20个cookie
3. cookie的大小有限制，4kb
4. 浏览器也有cookie上限，大概300个

删除cookie:

- 不设置有效期，关闭浏览器，自动删除
- 设置有效期为0

#### cookie中文乱码问题

如果cookie的值输入中文存在乱码问题

则，新建cookie时，要添加编码

```java
Cookie cookie = new Cookie("name", URLEncoder.encode("小丑","utf-8"));
```

取值时，要解码

```java
URLDecoder.decode(cookie.getValue(),"utf-8");//解码
```

## 7.4 Session(重点)

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/session.png)

什么是Session：

- 服务器会给每一个用户（浏览器）创建一个Session 对象
- 一个Session独占一个浏览器 ，只要浏览器没关闭，这个Session 就存在
- 用户登录之后，整个网站它都可以访问

常用方法：对应的类 HttpSession 

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/session2.png)

### session 与 cookie的区别

- Cookie是把用户的数据写给用户的**浏览器**，**浏览器保存**，（可以保存多个）
- Session 是把用户的数据写到用户独占Sessioin中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- Session对象由服务器创建，

### session使用场景

- 保存一个登录用户的信息
- 购物车信息
- 在整个网站中经常会使用的数据，保存在session中

案例文件：

```java
public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        //给session中放东西
        session.setAttribute("name",new Person("joker_陈",10));
        //常用方法
        //获取session的ID
        String id = session.getId();
        //判断是不是新创建的
        if (session.isNew()){
            resp.getWriter().write("session创建成功，id="+id);
        }else {
            resp.getWriter().write("session已经存在，id="+id);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

```java
public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");
        //得到session
        HttpSession session = req.getSession();

        System.out.println(session.getAttribute("name").toString());
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    }
}
```

```java
public class SessionDemo03 extends HttpServlet   {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        session.removeAttribute("name");
        //手动注销session
        session.invalidate();
        //自动注销session在web.xml文件里配置
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

**自动注销session**:需要在web.xml中配置

```xml
    <session-config>
<!-- 设置    session 的自动注销时间，以分钟为单位-->
        <session-timeout>1</session-timeout>
    </session-config>
```

拓展：

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-04_16-41-31.png)

# 8 JSP (暂时不学)

## 什么是 jsp

Java Server Pages ： Java服务器端页面，也和Servlet一样，用于动态Web技术！

最大的特点：

- 写JSP就像在写HTML

- 区别：
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据；

## jsp 原理

思路：jsp 怎么执行的

思路：JSP到底怎么执行的！

- 代码层面没有任何问题

- 服务器内部工作
  - tomcat中有一个work目录；
  - IDEA中使用Tomcat的会在IDEA的tomcat中生产一个work目录



分析源码，可知道，jsp 本质上就是一个 Servlet

![image-20210426214914068](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210426214914068.png)



jsp 页面中

- 只要是 JAVA代码就会原封不动的输出；

- 如果是HTML代码，就会被转换为：

  ```html
  out.write("<html>\r\n");
  ```

  

这样的格式，输出到前端！

## jsp 的基础语法

任何语言都有自己的语法，JAVA中有。

 JSP 作为java技术的一种应用，它拥有一些自己扩充的语法（了

解，知道即可！），Java所有语法都支持！

 导包

```xml
<dependencies>
  <dependency>
    <groupId>apache-taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
  </dependency>
  <dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
  </dependency>
  <dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.3</version>
  </dependency>
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
  </dependency>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
  </dependency>
</dependencies>
```

**jsp 表达式**

```jsp
<%--JSP表达式 
    作用：用来将程序的输出，输出到客户端 
    <%= 变量或者表达式%> --%> 
<%= new java.util.Date()%>
```

**jsp 脚本片段**

```jsp
<%
%>
```



**jsp 声明**

```jsp
<%!
    static {
        System.out.println("Loading Servlet");
    }

    private int globalVar = 0;
    public void joker(){
        System.out.println("进入了方法joker()");
    }
%>
```



JSP声明：会被编译到JSP生成Java的类中！其他的，就会被生成到源码的_jspService方法中！

在JSP，嵌入Java代码即可！



### jsp 语法总结

```jsp
<%%> jsp 脚本片段
<%=%>  jsp 表达式
<%!%> jsp 声明
<%--注释--%>
```



JSP的注释，不会在客户端显示，HTML就会！



## jsp 的指令

```jsp
<%--<page 可以用来定制 一些错误页面 等等--%>
<%@page args.... %>

<%--include 一般是用来，写 导入 头部，和脚部--%>
<%@include file=""%> 

<%--@include会将两个页面合二为一--%> 
<%@include file="common/header.jsp"%>
<h1>网页主体</h1>
<%@include file="common/footer.jsp"%>
<hr> <%--jSP标签 jsp:include：拼接页面，本质还是三个 --%>
```



## jsp 9 大内置对象

- PageContext 存东西

- Request 存东西

- Response

- Session 存东西

- Application 【SerlvetContext】 存东西

- confifig 【SerlvetConfifig】

- out

- page ，不用了解

- exception





```jsp
<%--
  Created by IntelliJ IDEA.
  User: admin
  Date: 2021/4/26
  Time: 22:31
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>内置对象</title>
</head>
<body>
<%--脚本片段的代码，会原封不动的放到生成的 jsp.java 里
要求：必须保证java 的语法正确性
    下面四个，主要是**作用域**不同：
--%>
<%
    pageContext.setAttribute("joker","joker1");//保存的数据只在一个页面中有效
    request.setAttribute("n2","j2");//保存的数据只在一次请求中有效，请求转发会携 带这个数据
    session.setAttribute("n3","j3"); //保存的数据只在一次会话中有效，从打开浏览器 到关闭浏览器
    application.setAttribute("n4","j4");//保存的数据只在服务器中有效，从打开服 务器到关闭服务器

%>
<%--    通过 pageContext 取出，通过寻找的方式--%>
<%
    String n1 = (String ) pageContext.findAttribute("n1");
    String n2 = (String ) pageContext.findAttribute("n2");
    String n3 = (String ) pageContext.findAttribute("n3");
    String n4 = (String ) pageContext.findAttribute("n4");
    String n5 = (String ) pageContext.findAttribute("n5");//不存在

%>
<%--el 表达式取的值，会把不存在的自动过滤掉，而正常 的jsp 表达式不会--%>
<h1>输出：</h1>
<h3>${n1}</h3>
<h3>${n2}</h3>
<h3>${n3}</h3>
<h3>${n4}</h3>
<h3>${n5}</h3>
<h3><%=n5%></h3>
</body>
</html>

```

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！

session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；

application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：聊天数据；



## jsp 标签、jstl 标签、EL 表达式

- ==这才是常用的,一般不会用尖括号<==
- jstl 的作用：JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以供我们使用，标签的功能和Java代码一样！
- 所以，jsp ,一般就不应该用来写，java 代码，直接在后端java 写多好。

包含：前三个不用管，基本

**格式化标签**

**SQL****标签**

**XML** **标签**

**核心标签** （掌握部分）



EL表达式： ${ }

- **获取数据**

- **执行运算**

- **获取web开发的常用对象**

```jsp
<%-- EL表达式获取表单中的数据 ${param.参数名} --%>
```



```xml
<!-- standard标签库 -->
<dependency>
  <groupId>apache-taglibs</groupId>
  <artifactId>standard</artifactId>
  <version>1.1.2</version>
</dependency>
<!-- JSTL表达式的依赖 -->
<dependency>
  <groupId>javax.servlet.jsp.jstl</groupId>
  <artifactId>jstl-api</artifactId>
  <version>1.2</version>
</dependency>
```

**JSTL****标签库使用步骤**

- 引入对应的 taglib

- 使用其中的方法

- **在Tomcat 也需要引入jstl 和 standar 的包，否则会报错：JSTL解析错误**

# 9 JavaBean

- javabean 通常称为实体类
- JavaBean有特定的写法：
  - 必须要有一个无参构造
  - 属性必须私有化
  - 必须有对应的get/set方法
- 一般用来和数据库的字段做映射（ORM）
- ORM：对象关系映射
  - 表--->类
  - 字段--  -> 属性
  - 行记录---> 对象

# 10 mvc 三层架构

- MVC （Model View Controller）:模型 视图 控制器

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/filter.png)

- 除了微服务，都是用这个架构开发
  - Model:
    - 业务处理：业务逻辑（Service）
    - 数据持久层：CRUD （DAO）
  - View:
    - 展示数据
    - 提供链接发起Servlet请求（a,form, img....）
  - Controller:
    - 接受用户的请求：（req:请求参数、session信息。。。）
    - 交给业务层处理对应的代码
    - 控制视图的跳转

# 11 Filter（过滤器）（重点）

- 有 Filter 接口：javax.servlet.Filter （注意包不要导错）
- 很简单，只需实现三个方法
- 用来过滤网站的数据，最常见的应用场景
  - 处理中文的乱码
  - 用户登录权限的问题
  - 。。。

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/MVC.png)

- Filter **开发步骤：**

1. 导包
2. 编写过滤器，实现Filter接口
3. 在web.xml中配置Filter

实现接口类 ：CharacterEncodingFilter

```java
public class CharacterEncodingFilter implements Filter{

    @Override
    //初始化：web服务器启动，就已经初始化了，随时等待过滤对象出现
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    //chain: 链
    /*
    总结：
    1. 过滤器中的所有代码在过滤特定请求的时候都会执行
    2. 必须要让过滤器继续通行 chain.doFilter(request,response); 就是把代码往下转交
     */
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");

        System.out.println("CharacterEncodingFilter 执行前");
        //固定代码
        //让我们的请求继续走，如果不写，程序到这里就会被拦截停止
        chain.doFilter(request,response);
        System.out.println("CharacterEncodingFilter 执行后");
    }

    @Override
    //销毁：web服务器停止的时候，过滤器会销毁
    public void destroy() {

    }
}
```

web.xml文件的添加

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>com.joker.filter.CharacterEncodingFilter</filter-class>
</filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
<!--        过滤 /servlet 的任何请求，都会经过这个过滤器-->
<!--        一般会写多个mapping,而不是为了偷懒，直接写/*-->
        <url-pattern>/servlet/*</url-pattern>
    </filter-mapping>
</web-app>
```



# 12 监听器

- 用的很少，主要在GUI中，但是我没学

- 实现一个监听器的接口 （有N种）

- 步骤

  - 1 编写一个监听器

    ```java
    //统计网站在线人数：统计session
    public class OnlineCountListener implements HttpSessionListener {
        @Override//创建session监听
        //一旦创建一次，就会触发一次这个事件
        public void sessionCreated(HttpSessionEvent se) {
            ServletContext servletContext = se.getSession().getServletContext();
            Integer count =(Integer) servletContext.getAttribute("OnlineCount");
    
            if (count==null){
                count = new Integer(1);
            }else {
                int count2 = count.intValue();
                count = new Integer(count2 + 1);
            }
    
            servletContext.setAttribute("OnlineCount",count);
        }
    
        @Override
        public void sessionDestroyed(HttpSessionEvent se) {
            ServletContext servletContext = se.getSession().getServletContext();
            Integer count =(Integer) servletContext.getAttribute("OnlineCount");
    
            if (count==null){
                count = new Integer(0);
            }else {
                int count2 = count.intValue();
                count = new Integer(count2 - 1);
            }
    
            servletContext.setAttribute("OnlineCount",count);
        }
    }
    ```

  - 2 web.xml 中注册监听器

    ```xml
    <!--    监听器-->
        <listener>
            <listener-class>com.joker.listener.OnlineCountListener</listener-class>
        </listener>
    ```

  - 3 index.jsp编写交互代码，

  - 4 看情况是否使用，几乎不用

# 13 过滤器、监听器的常见应用  

- 监听器：GUI编程中经常使用，但我没学



## Filter 实现权限拦截

- 用户登录后才可以进入主页，用户注销后，就不能进入主页了

1. 用户登录之后，向Session中放入用户的数据
2. 进入主页的时候判断用户是否已经登录（要求在过滤器中实现）
3. 案例文件执行顺序：
   - login.jsp--->LoginServlet.java--->success.jsp/error.jsp--->LogoutServlet--->SysFilter
   - 还有一个自定义常量类：Constant

login.jsp

```jsp
<!DOCTYPE html>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
	<title>Title</title>
</head>
<body>
	<h1>登录</h1>
<form action="${pageContext.request.contextPath}/servlet/login" method="get">
	<input type="text" name="username">
	<input type="submit">
</form>
</body>
</html>
```

LoginServlet.java

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取前端请求的参数
        String username = req.getParameter("username");
        if (username.equals("admin")){//登录成功
            req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId());
            resp.sendRedirect(req.getContextPath()+"/sys/success.jsp");
        }else{
            resp.sendRedirect(req.getContextPath()+"/error.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

success.jsp

```jsp
<!DOCTYPE html>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
	<title>Title</title>
</head>
<body>

<h1>主页</h1>
<p><a href="/javaweb-filter/servlet/logout">注销</a></p>
</body>
</html>
```

error.jsp

```jsp
<!DOCTYPE html>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
	<title>Title</title>
</head>
<body>
<h1>失败页面</h1>
<a href="/javaweb-filter/login.jsp">返回登录页</a>
</body>
</html>
```

LogoutServlet.java

```java
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object user_session = req.getSession().getAttribute(Constant.USER_SESSION);
        if (user_session != null){
            req.getSession().removeAttribute(Constant.USER_SESSION);
            resp.sendRedirect(req.getContextPath()+"/login.jsp");
        } else{
            resp.sendRedirect(req.getContextPath()+"/login.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

SysFilter.java

```java
/**
 * 系统登录过滤器，即用户判断
 */
public class SysFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse resp = (HttpServletResponse) response;

        if (req.getSession().getAttribute(Constant.USER_SESSION) == null){
            resp.sendRedirect(req.getContextPath()+"/error.jsp");
        }
        chain.doFilter(req,resp);
    }
    @Override
    public void destroy() {

    }
}

```

Constant.java

```java
/**
 * 常量类，因为 USER_SESSION 经常用
 */
public class Constant {
    public static final String USER_SESSION = "USER_SESSION";
}

```



# 14 JDBC (复习)

注：使用org.junit.Test包里的@Test注解，只能用在方法上，可直接运行测试



# 15 拓展

## 15.1 文件上传



## 15.2  邮件发送