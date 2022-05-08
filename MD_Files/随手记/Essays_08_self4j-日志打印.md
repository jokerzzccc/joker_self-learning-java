# Essays_08_self4j-log4j-日志打印

时间：2022年5月9日

参考博客：

- self4j 官网：https://www.slf4j.org/docs.html
- 日志输出接口：https://www.slf4j.org/api/org/slf4j/Logger.html

- https://www.cnblogs.com/lingyejun/p/9366533.html
- https://developer.aliyun.com/article/52943#slide-8
- self4j 原理详解：https://juejin.cn/post/6844903949598736391#heading-0
- 使用详解：https://www.cnblogs.com/qlqwjy/p/9275415.html





# 日志级别

slf4j的日志级别分为五种

　　**info、debug、error、warn、trane**

常用的是这是三个。

+ **info** 一般处理业务逻辑的时候使用，就跟 system.err打印一样，用于说明此处是干什么的。slf4j使用的时候是可以动态的传参的，使用占位符 {} 。后边一次加参数，会挨个对应进去。
+ **debug:** 一般放于程序的某个关键点的地方，用于打印一个变量值或者一个方法返回的信息之类的信息
+ **error：** 用户程序报错，必须解决的时候使用此级别打印日志。

不常用的有：

+ **warn：**警告，不会影响程序的运行，但是值得注意。
+ **trane:** 一般不会使用，在日志里边也不会打印出来，好像是很低的一个日志级别。



# sef4j 的 配置 pattern 

- 参考博客：
  - https://www.cnblogs.com/qlqwjy/p/9275415.html

```sh
%p: 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL%d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyyy-MM-dd HH:mm:ss,SSS}，SSS为毫秒数(也可以写为SS，只不过SSS如果不足三位会补0)，输出类似：2011-10-18 22:10:28,021%r: 输出自应用启动到输出该日志耗费的毫秒数%t: 输出产生日志的线程名称%l: 输出日志事件的位置，相当于%c.%M(%F:L)的组合，包括类全名、方法、文件名以及在代码中行数。例如:cn.xm.test.PlainTest.main(PlanTest.java:12)%c: 输出日志信息所属的类目，通常就是所在类的全名。可写为%c{num},表示取完整类名的层数，从后向前取，比如%c{2}取 "cn.qlq.exam"类为"qlq.exam"。%M: 输出产生日志信息的方法名%x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中 
%%: 输出一个"%"字符 
%F: 输出日志消息产生时所在的文件名称 
%L: 输出代码中的行号
%m: 输出代码中指定的消息,产生的日志具体信息 
%n: 输出一个回车换行符，Windows平台为"\r\n"，Unix平台为"\n"输出日志信息换行 
```



配置示例：







# log.error

- 参考博客：
  - https://www.cnblogs.com/lingyejun/p/9366533.html

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
 
public class TestLogError {
    public static final Logger LOGGER = LogManager.getLogger(TestLogError.class);
 
    public static void main(String[] args) {
        try{
            // 模拟空指针异常
            //Integer nullInt = Integer.valueOf(null);
            int[] array = {1,2,3,4,5};
            int outBoundInt = array[5];
        }catch (Exception e){
            // 直接打印，则只输出异常类型
            LOGGER.error(e);
            // 使用字符串拼接
            LOGGER.error("使用 + 号连接直接输出 e : " + e);
            LOGGER.error("使用 + 号连接直接输出 e.getMessage() : " + e.getMessage());
            LOGGER.error("使用 + 号连接直接输出 e.toString() : " + e.toString());
            // 使用逗号分隔，调用两个参数的error方法
            LOGGER.error("使用 , 号 使第二个参数作为Throwable : ", e);
            // 尝试使用分隔符,第二个参数为Throwable,会发现分隔符没有起作用，第二个参数的不同据，调用不同的重载方法
            LOGGER.error("第二个参数为Throwable，使用分隔符打印 {} : ", e);
            // 尝试使用分隔符，第二个参数为Object,会发现分隔符起作用了，根据第二个参数的不同类型，调用不同的重载方法
            LOGGER.error("第二个参数为Object，使用分隔符打印 {} ",123);
        }
    }
}
```

