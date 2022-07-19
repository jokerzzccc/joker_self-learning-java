# 目录

[toc]

# Essays_16_RestTemplate

- 时间：2022-07-19
- 参考链接：
  - spring RestTemplate 官网介绍：https://docs.spring.io/spring-framework/docs/5.1.9.RELEASE/spring-framework-reference/integration.html#rest-client-access
  - spring RestTemplate API : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html
  - 使用博客：
    - RestTemplate 使用详解；https://www.cnblogs.com/54chensongxia/p/11414923.html	
    - springboot + restTemplate : https://blog.csdn.net/weixin_40461281/article/details/83540604



# 1、介绍

- 作用：远程调用一个 HTTP 接口时，我们经常会用到 RestTemplate 这个类。这个类是 Spring 框架提供的一个工具类。
- Spring 官网对它的介绍如下：

> [`RestTemplate`](https://docs.spring.io/spring/docs/5.1.9.RELEASE/spring-framework-reference/integration.html#rest-resttemplate): The original Spring REST client with a synchronous, template method API.

从上面的介绍中我们可以知道：RestTemplate 是一个**同步的 Rest API 客户端**。下面我们就来介绍下 RestTemplate 的常用功能。(异步的就看 [WebClient](https://docs.spring.io/spring-framework/docs/5.1.9.RELEASE/spring-framework-reference/web-reactive.html#webflux-client))



# 2、RestTemplate 的 常用方法

表格：RestTemplate 的方法

| Method group      | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| `getForObject`    | Retrieves a representation via GET.                          |
| `getForEntity`    | Retrieves a `ResponseEntity` (that is, status, headers, and body) by using GET. |
| `headForHeaders`  | Retrieves all headers for a resource by using HEAD.          |
| `postForLocation` | Creates a new resource by using POST and returns the `Location` header from the response. |
| `postForObject`   | Creates a new resource by using POST and returns the representation from the response. |
| `postForEntity`   | Creates a new resource by using POST and returns the representation from the response. |
| `put`             | Creates or updates a resource by using PUT.                  |
| `patchForObject`  | Updates a resource by using PATCH and returns the representation from the response. Note that the JDK `HttpURLConnection` does not support the `PATCH`, but Apache HttpComponents and others do. |
| `delete`          | Deletes the resources at the specified URI by using DELETE.  |
| `optionsForAllow` | Retrieves allowed HTTP methods for a resource by using ALLOW. |
| `exchange`        | More generalized (and less opinionated) version of the preceding methods that provides extra flexibility when needed. It accepts a `RequestEntity` (including HTTP method, URL, headers, and body as input) and returns a `ResponseEntity`.These methods allow the use of `ParameterizedTypeReference` instead of `Class` to specify a response type with generics. |
| `execute`         | The most generalized way to perform a request, with full control over request preparation and response extraction through callback interfaces. |

上面的方法我们大致可以分为三组：

+ getForObject ~ optionsForAllow ：分为一组，这类方法是常规的 Rest API（GET、POST、DELETE 等）方法调用；
+ exchange：接收一个 `RequestEntity` 参数，可以自己设置 HTTP method，URL，headers 和 body，返回 ResponseEntity；
+ execute：通过 callback 接口，可以对请求和返回做更加全面的自定义控制。

一般情况下，我们使用第一组和第二组方法就够了。



RestTemplate定义了36个与REST资源交互的方法，其中的大多数都对应于HTTP的方法。 
其实，这里面只有**11个独立的方法**，其中有十个有三种重载形式，而第十一个则重载了六次，这样一共形成了36个方法。

1. delete() 在特定的URL上对资源执行HTTP DELETE操作
2. exchange() 在URL上执行特定的HTTP方法，返回包含对象的ResponseEntity，这个对象是从响应体中映射得到的
3. execute() 在URL上执行特定的HTTP方法，返回一个从响应体映射得到的对象
4. getForEntity() 发送一个HTTP GET请求，返回的ResponseEntity包含了响应体所映射成的对象
5. getForObject() 发送一个HTTP GET请求，返回的请求体将映射为一个对象
6. postForEntity() POST 数据到一个URL，返回包含一个对象的ResponseEntity，这个对象是从响应体中映射得到的
7. postForObject() POST 数据到一个URL，返回根据响应体匹配形成的对象
8. headForHeaders() 发送HTTP HEAD请求，返回包含特定资源URL的HTTP头
9. optionsForAllow() 发送HTTP OPTIONS请求，返回对特定URL的Allow头信息
10. postForLocation() POST 数据到一个URL，返回新创建资源的URL
11. put() PUT 资源到特定的URL



# 3、springboot + RestTemplate 的使用示例

## 3.1 准备工作

1、导入依赖：

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```
2、在启动类同包下创建[RestTemplate](https://so.csdn.net/so/search?q=RestTemplate&spm=1001.2101.3001.7020).java类

```java
@Configuration
public class RestTemplateConfig {
 
    @Bean
    public RestTemplate restTemplate(ClientHttpRequestFactory factory){
        return new RestTemplate(factory);
    }
 
    @Bean
    public ClientHttpRequestFactory simpleClientHttpRequestFactory(){
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
        factory.setConnectTimeout(15000);
        factory.setReadTimeout(5000);
        return factory;
    }
 
}
```

3、然后在Service类中注入使用即可

```java
@Service
public class demoService {
 
    @Autowired
    private RestTemplate restTemplate;
 
    public String get(Integer id){
        return restTemplate.getForObject("http://localhost:8080/user?userId=id",String.class);
    }
}
```

## 3.2 方法的具体使用

### getForEntity

- get请求就和正常在浏览器url上发送请求一样

下面是有参数的get请求

```java
@GetMapping("getForEntity/{id}")
public User getById(@PathVariable(name = "id") String id) {
    ResponseEntity<User> response = restTemplate.getForEntity("http://localhost/get/{id}", User.class, id);
    User user = response.getBody();
    return user;
}
```
### getForObject

- getForObject 和 getForEntity 用法几乎相同,**指示返回值返回的是 响应体**,省去了我们 再去 getBody() 

```java
    @GetMapping("getForObject/{id}")
    public User getById(@PathVariable(name = "id") String id) {
        User user = restTemplate.getForObject("http://localhost/get/{id}", User.class, id);
        return user;
    }
```



### postForEntity

```java
    @RequestMapping("saveUser")
    public String save(User user) {
        ResponseEntity<String> response = restTemplate.postForEntity("http://localhost/save", user, String.class);
        String body = response.getBody();
        return body;
    }
```



### postForObject

用法与 getForObject 一样

如果遇到 postForObject 方法在 Controller 接受不到参数问题 请参考的的另一篇博客 : 

[RestTemplate post请求 Controller 接收不到值的解决方案 postForObject方法源码解析](https://blog.csdn.net/weixin_40461281/article/details/83472648)

RestTemplate 的 postForObject 方法有**四个参数**:

- String url => 顾名思义 这个参数是请求的**url路径**
- Object request => **请求的body** 这个参数需要再controller类用 @RequestBody 注解接收
- Class<T> responseType => **接收响应体的类型**
- 第四个参数 postForObject 方法多种重构
  - Map<String,?> uriVariables => uri 变量 顾名思义 这是放置变量的地方
  - Object... uriVariables => 可变长 Object 类型 参数

### exchange

```java
@PostMapping("demo")
public void demo(Integer id, String name){
 
        HttpHeaders headers = new HttpHeaders();//header参数
        headers.add("authorization",Auth);
        headers.setContentType(MediaType.APPLICATION_JSON);
 
        JSONObject obj = new JSONObject();//放入body中的json参数
        obj.put("userId", id);
        obj.put("name", name);
 
        HttpEntity<JSONObject> request = new HttpEntity<>(content,headers); //组装
  
        ResponseEntity<String> response = template.exchange("http://localhost:8080/demo",HttpMethod.POST,request,String.class);
    }
```









# 4、应用场景

## 4.1 文件上传

### 准备工具类：

```java
// 自己实现了一个 Resource
public class InMemoryResource extends ByteArrayResource {
    private final String filename;
    private final long lastModified;

    public InMemoryResource(String filename, String description, byte[] content, long lastModified) {
        super(content, description);
        this.lastModified = lastModified;
        this.filename = filename;
    }
    
    @Override
    public long lastModified() throws IOException {
        return this.lastModified;
    }

    @Override
    public String getFilename() {
        return this.filename;
    }
}
```

### **上传代码：**

```java
 @PostMapping("/m3")
    public Object m3(@RequestBody JSONObject params) throws Exception{

        final String url = "http://localhost:8888/hello/m3";
        // 设置请求头
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.MULTIPART_FORM_DATA);
        // 设置请求体，注意是 LinkedMultiValueMap
        // 下面两个流从文件服务下载，这边省略（注意最后关闭流）
        InputStream fis1 = 
        InputStream fis2 = 

        InMemoryResource resource1 = new InMemoryResource("file1.jpg","description1", FileCopyUtils.copyToByteArray(fis1), System.currentTimeMillis());
        InMemoryResource resource2 = new InMemoryResource("file2.jpg","description2", FileCopyUtils.copyToByteArray(fis2), System.currentTimeMillis());
        MultiValueMap<String, Object> form = new LinkedMultiValueMap<>();
        form.add("file", resource1);
        form.add("file", resource2);
        form.add("param1","value1");

        HttpEntity<MultiValueMap<String, Object>> files = new HttpEntity<>(form, headers);
        JSONObject s = restTemplate.postForObject(url, files, JSONObject.class);
        return s;
    }

```

### 接收代码：

```java
@RequestMapping("/m3")
public Object fileUpload(@RequestParam("file") MultipartFile[] files, HttpServletRequest request) throws Exception {
    // 携带的其他参数可以使用 getParameter 方法接收
    String param1 = request.getParameter("param1");
    Response response = new Response();
    if (files == null) {
        response.failure("文件上传错误, 服务端未拿到上传的文件！");
        return response;
    }
    for (MultipartFile file : files) {
        if (!file.isEmpty() && file.getSize() > 0) {
            String fileName = file.getOriginalFilename();
            // 参考 FileCopyUtils 这个工具类
            file.transferTo(new File("D:\\" + fileName));
            logger.info("文件:{} 上传成功...",fileName);
        }
    }
    response.success("文件上传成功");
    return response;
    }

```



## 4.2 文件下载

```java
private InputStream downLoadVideoFromVod(String url) throws Exception {
  byte[] bytes = restTemplate.getForObject(url, byte[].class);
  return new ByteArrayInputStream(bytes);
}
```



# 5、简单总结

通过 RestTemplate，我们可以非常方便的进行 Rest API 调用。但是在 Spring 5 中已经不再建议使用 RestTemplate，而是建议使用 [WebClient](https://docs.spring.io/spring-framework/docs/5.1.9.RELEASE/spring-framework-reference/web-reactive.html#webflux-client)。WebClient 是一个支持**异步调用的 Client**。所以喜欢研究新东西的同学可以开始研究下新东西了。



# THE END