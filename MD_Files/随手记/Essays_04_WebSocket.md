# 目录

[toc]



# Essays_04_WebSocket

时间：2022年5月4日

参考博客：

- webSocket 基础详解：https://www.cnblogs.com/jingmoxukong/p/7755643.html
- SpringBoot 集成 WebSocket ：https://www.cnblogs.com/xuwenjin/p/12664650.html
- SpringBoot 结合 webSocket 实战且 源码解析：https://segmentfault.com/a/1190000023194240



# 1、WebSocket 的概述

## 1.1 为什么需要 WebSocket 

HTTP 协议的缺点：

- 了解计算机网络协议的人，应该都知道：HTTP 协议是一种无状态的、无连接的、单向的应用层协议。它采用了请求/响应模型。通信请求只能由客户端发起，服务端对请求做出应答处理。

- 这种通信模型有一个弊端：HTTP 协议无法实现服务器主动向客户端发起消息。

- 这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。大多数 Web 应用程序将通过频繁的异步 JavaScript 和 XML（AJAX）请求实现长轮询。轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。

WebSocket 被发明：

- 因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。
- WebSocket 连接允许客户端和服务器之间进行全双工通信，以便任一方都可以通过建立的连接将数据推送到另一端。WebSocket 只需要建立一次连接，就可以一直保持连接状态。这相比于轮询方式的不停建立连接显然效率要大大提高。



> webSocket 优点：

说到优点，这里的对比参照物是HTTP协议，概括地说就是：支持双向通信，更灵活，更高效，可扩展性更好。

1. 支持双向通信，实时性更强。
2. 更好的二进制支持。
3. 较少的控制开销。连接创建后，ws客户端、服务端进行数据交换时，协议控制的数据包头部较小。在不包含头部的情况下，服务端到客户端的包头只有2~10字节（取决于数据包长度），客户端到服务端的的话，需要加上额外的4字节的掩码。而HTTP协议每次通信都需要携带完整的头部。
4. 支持扩展。ws协议定义了扩展，用户可以扩展协议，或者实现自定义的子协议。（比如支持自定义压缩算法等）

对于后面两点，没有研究过WebSocket协议规范的同学可能理解起来不够直观，但不影响对WebSocket的学习和使用。



>  WebSocket 缺点

缺点：

- 少部分浏览器可能不支持，浏览器支持的程度与方式有区别；

- 长连接对后端业务的代码稳定性要求更高，后端推送功能相对复杂；

- 成熟的 HTTP生态下有大量的组件可以复用，WebSocket较少；

## 1.1 WebSocket 是什么

- [WebSocket](http://websocket.org/) 是一种网络通信协议。[RFC6455](https://tools.ietf.org/html/rfc6455) 定义了它的通信标准。

- WebSocket 是 HTML5 开始提供的一种**在单个 TCP 连接上进行全双工通讯的协议**。
- 只要记住：
  - WebSocket可以在浏览器里使用
  - 支持双向通信
  - 使用很简单



> HTTP 协议 与 WebSocket 协议 生命周期对比：

![image-20220504163455425](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220504163455425.png)



## 1.2 WebSocket 做什么

- WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。
- 在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。



> 应用场景：

- 即时聊天通讯，网站消息通知，

- 在线协同编辑，如腾讯文档；

- 多玩家在线游戏，视频弹幕，股票基金实时报价；

## 1.3 WebSocket 如何做





# 2、WebSocket 的实践

## 2.1 WebSocket 与 SpringBoot 结合

- SpringBoot 官网 WebSocket ：

  - https://docs.spring.io/spring-boot/docs/2.6.7/reference/htmlsingle/#messaging.websockets

  - https://docs.spring.io/spring-framework/docs/5.3.19/reference/html/web-reactive.html#webflux-websocket

- 参考博客：

  - https://www.cnblogs.com/xuwenjin/p/12664650.html
  - https://segmentfault.com/a/1190000020591725?utm_source=sf-similar-article





### maven依赖

SpringBoot2.0对WebSocket的支持简直太棒了，直接就有包可以引入

```xml
<dependency>  
     <groupId>org.springframework.boot</groupId>  
     <artifactId>spring-boot-starter-websocket</artifactId>  
</dependency> 
```

### WebSocketConfig

启用WebSocket的支持也是很简单，几句代码搞定

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

/**
 * 开启WebSocket支持
 * @author zhengkai
 */
@Configuration  
public class WebSocketConfig {  
    @Bean  
    public ServerEndpointExporter serverEndpointExporter() {  
        return new ServerEndpointExporter();  
    }  
} 
```

### WebSocketServer

因为WebSocket是类似客户端服务端的形式(采用ws协议)，那么这里的WebSocketServer其实就相当于一个ws协议的Controller直接@ServerEndpoint("/websocket")、@Component启用即可，然后在里面实现@OnOpen,@onClose,@onMessage等方法

```java
import java.io.IOException;
import java.util.concurrent.CopyOnWriteArraySet;

import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;
import org.springframework.stereotype.Component;
import cn.hutool.log.Log;
import cn.hutool.log.LogFactory;
import lombok.extern.slf4j.Slf4j;


@ServerEndpoint("/websocket/{sid}")
@Component
public class WebSocketServer {
    
    static Log log=LogFactory.get(WebSocketServer.class);
    //静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
    private static int onlineCount = 0;
    //concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。
    private static CopyOnWriteArraySet<WebSocketServer> webSocketSet = new CopyOnWriteArraySet<WebSocketServer>();

    //与某个客户端的连接会话，需要通过它来给客户端发送数据
    private Session session;

    //接收sid
    private String sid="";
    /**
     * 连接建立成功调用的方法*/
    @OnOpen
    public void onOpen(Session session,@PathParam("sid") String sid) {
        this.session = session;
        webSocketSet.add(this);     //加入set中
        addOnlineCount();           //在线数加1
        log.info("有新窗口开始监听:"+sid+",当前在线人数为" + getOnlineCount());
        this.sid=sid;
        try {
             sendMessage("连接成功");
        } catch (IOException e) {
            log.error("websocket IO异常");
        }
    }

    /**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose() {
        webSocketSet.remove(this);  //从set中删除
        subOnlineCount();           //在线数减1
        log.info("有一连接关闭！当前在线人数为" + getOnlineCount());
    }

    /**
     * 收到客户端消息后调用的方法
     *
     * @param message 客户端发送过来的消息*/
    @OnMessage
    public void onMessage(String message, Session session) {
        log.info("收到来自窗口"+sid+"的信息:"+message);
        //群发消息
        for (WebSocketServer item : webSocketSet) {
            try {
                item.sendMessage(message);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 
     * @param session
     * @param error
     */
    @OnError
    public void onError(Session session, Throwable error) {
        log.error("发生错误");
        error.printStackTrace();
    }
    /**
     * 实现服务器主动推送
     */
    public void sendMessage(String message) throws IOException {
        this.session.getBasicRemote().sendText(message);
    }


    /**
     * 群发自定义消息
     * */
    public static void sendInfo(String message,@PathParam("sid") String sid) throws IOException {
        log.info("推送消息到窗口"+sid+"，推送内容:"+message);
        for (WebSocketServer item : webSocketSet) {
            try {
                //这里可以设定只推送给这个sid的，为null则全部推送
                if(sid==null) {
                    item.sendMessage(message);
                }else if(item.sid.equals(sid)){
                    item.sendMessage(message);
                }
            } catch (IOException e) {
                continue;
            }
        }
    }

    public static synchronized int getOnlineCount() {
        return onlineCount;
    }

    public static synchronized void addOnlineCount() {
        WebSocketServer.onlineCount++;
    }

    public static synchronized void subOnlineCount() {
        WebSocketServer.onlineCount--;
    }
}
```

### 消息推送

至于推送新信息，可以再自己的Controller写个方法调WebSocketServer.sendInfo();即可

```java
@Controller
@RequestMapping("/checkcenter")
public class CheckCenterController {

    //页面请求
    @GetMapping("/socket/{cid}")
    public ModelAndView socket(@PathVariable String cid) {
        ModelAndView mav=new ModelAndView("/socket");
        mav.addObject("cid", cid);
        return mav;
    }
    //推送数据接口
    @ResponseBody
    @RequestMapping("/socket/push/{cid}")
    public ApiReturnObject pushToWeb(@PathVariable String cid,String message) {  
        try {
            WebSocketServer.sendInfo(message,cid);
        } catch (IOException e) {
            e.printStackTrace();
            return ApiReturnUtil.error(cid+"#"+e.getMessage());
        }  
        return ApiReturnUtil.success(cid);
    } 
} 
```

### 前端页面

在页面用js代码调用socket，当然，太古老的浏览器是不行的，一般新的浏览器或者谷歌浏览器是没问题的。

```js
<!DOCTYPE HTML>
<html>
   <head>
   <meta charset="utf-8">
   <title>菜鸟教程(runoob.com)</title>
    
      <script type="text/javascript">
         function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               
               // 打开一个 web socket
               var ws = new WebSocket("ws://127.0.0.1:8080/websocket/1212");
                
               ws.onopen = function()
               {
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
                
               ws.onmessage = function (evt) 
               { 
                  var received_msg = evt.data;
                  alert(received_msg)
                  alert("数据已接收...");
               };
                
               ws.onclose = function()
               { 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
            
            else
            {
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
      </script>
        
   </head>
   <body>
   
      <div id="sse">
         <a href="javascript:WebSocketTest()">运行 WebSocket</a>
      </div>
      
   </body>
</html>
```

### 运行

1、启动springboot项目。

2、直接打开页面test.html，点击链接

![img](https://segmentfault.com/img/remote/1460000020591730)

3、服务端发送消息：

```awk
http://localhost:8080/checkcenter/socket/push/1212?message=xxxx
```

















# THE END