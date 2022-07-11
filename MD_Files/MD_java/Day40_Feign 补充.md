# 目录

[toc]

# Day40_Feign 补充

时间：2022-07-11

参考链接：

- Feign Github：https://github.com/OpenFeign/feign
- SpringCloud 官网 结合 Feign : https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-feign.html
- Feign 使用博客教学：https://segmentfault.com/a/1190000011675354
- Feign 注解详解：https://www.cnblogs.com/smiler/p/10689894.html
- 



# 1、介绍



# 2、注解详解

## @FeignClient

 FeignClient注解被@Target(ElementType.TYPE)修饰，表示FeignClient注解的作用目标在**接口上**

```java
@FeignClient(name = "github-client", url = "https://api.github.com", configuration = GitHubExampleConfig.class)
public interface GitHubClient {
    @RequestMapping(value = "/search/repositories", method = RequestMethod.GET)
    String searchRepo(@RequestParam("q") String queryStr);
}
```

　声明接口之后，在代码中通过@Resource注入之后即可使用。@FeignClient标签的常用属性如下：

+ **name**：指定FeignClient的名称，如果项目使用了Ribbon，name属性会作为微服务的名称，用于服务发现。微服务中的服务名，中间不能有 _ 可以用 - 代替
+ **url**: url一般用于调试，可以手动指定@FeignClient调用的地址。
  + 如果不是微服务调用的话可以手动设置地址这个值可以设置为 http://ip:端口/
+ **decode404**:当发生http 404错误时，默认false，如果该字段位true，会调用decoder进行解码，否则抛出FeignException
+ **configuration**: Feign配置类，可以自定义Feign的Encoder、Decoder、LogLevel、Contract
+ **fallback**: 定义容错的处理类，当调用远程接口失败或超时时，会调用对应接口的容错逻辑，fallback指定的类必须实现@FeignClient标记的接口
+ **fallbackFactory**: 工厂类，用于生成fallback类示例，通过这个属性我们可以实现每个接口通用的容错逻辑，减少重复的代码
+ **path**: 定义当前FeignClient的**统一前缀**，例如controller上有加接口前缀的话就要写在这里。

```java
@FeignClient(name = "github-client",
        url = "https://api.github.com",
        configuration = GitHubExampleConfig.class,
        fallback = GitHubClient.DefaultFallback.class)
public interface GitHubClient {
    @RequestMapping(value = "/search/repositories", method = RequestMethod.GET)
    String searchRepo(@RequestParam("q") String queryStr);
 
    /**
     * 容错处理类，当调用失败时，简单返回空字符串
     */
    @Component
    public class DefaultFallback implements GitHubClient {
        @Override
        public String searchRepo(@RequestParam("q") String queryStr) {
            return "";
        }
    }
```

　

在使用fallback属性时，需要使用@Component注解，保证fallback类被Spring容器扫描到,GitHubExampleConfig内容如下：

```java
@Configuration
public class GitHubExampleConfig {
    @Bean
    Logger.Level feignLoggerLevel() {
        return Logger.Level.FULL;
    }
}
```

　

　在使用FeignClient时，Spring会按name创建不同的ApplicationContext，通过不同的Context来隔离FeignClient的配置信息，在使用配置类时，不能把配置类放到Spring App Component scan的路径下，否则，配置类会对所有FeignClient生效.





# THE END 