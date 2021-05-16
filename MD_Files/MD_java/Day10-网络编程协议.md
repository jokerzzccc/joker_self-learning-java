# 网络编程

# 计算机网络

- 定义：为实现资源共享和信息传递，通过通信线路连接起来的若干主机（Host）
- 按照地理范围，网络分为：
  - 局域网
  - 城域网
  - 广域网
    - 互联网：（Internet）点与点相连
    - 万维网：（WWW -World Wide Web）端与端相连
    - 物联网：(IoT - Internet of Things)物与物相连
- **网络编程的目的**：让计算机与计算机之间建立连接、进行通信

# 网络模型-OSI参考模型

- OSI(Open System Interconnection) 开放式系统互联
- ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-16_12-23-19.png)

- 协议：数据传输的格式

# 网络模型-TCP/IP 模型

- ​	一组用于实现网络互连的通信协议，将协议分成四个层次
- ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-16_12-30-03.png)

- 

# 网络通信协议

## 基于TCP和UDP的应用层协议

- 基于TCP 的应用层协议有：HTTP,FTP,SMTP,TELNET,SSH

  | 协议               | 全称                                             | 默认端口                           |
  | ------------------ | ------------------------------------------------ | ---------------------------------- |
  | HTTP （ 用的最多） | HyperText Transfer Protocol（超文本传输协议）    | 80                                 |
  | FTP                | File Transfer Protocol (文件传输协议)            | 20用于传输数据，21用于传输控制信息 |
  | SMTP               | Simple Mail Transfer Protocol (简单邮件传输协议) | 25                                 |
  | TELNET             | Teletype over the Network (网络电传)             | 23                                 |
  | SSH                | Secure Shell                                     | 22                                 |

  

- 基于 UDP 的应用层协议：DNS，TFTP，SNMP

  | 协议 | 全称                                                  | 默认端口                                         |
  | ---- | ----------------------------------------------------- | ------------------------------------------------ |
  | DNS  | Domain Name Service (域名服务)                        | 53                                               |
  | TFTP | Trivial File Transfer Protocol (简单文件传输协议)     | 69                                               |
  | SNMP | Simple Network Management Protocol (简单网络管理协议) | 通过UDP端口161接收，只有Trap信息采用UDP端口162。 |
  | NTP  | Network Time Protocol (网络时间协议)                  | 123                                              |

## TCP 协议

- TCP 协议: Transmission Control Protocol 传输控制协议
  - 是一种**面向连接的**、**可靠的**、**面向字节流**的传输层通信协议。**数据大小无限制**。建立连接的过程需要三次握手，断开连接的过程需要四次挥手。
  - 客户端、服务端
  - 传输完成，释放连接，效率低
  - 类似打电话



## UDP 协议

- User Datagram Protocol 用户数据报协议
  - 类似发短信
  - 是一种**无连接**的传输层协议，提供**面向事物**的简单**不可靠** 信息传送服务，每个包的大小是64KB
  - 优点：支持广播放送，效率非常高
  - 客户端，服务端：没有明确的界限（即，客户端可以给客户端发）
  - 不管有没有准备好，都可以发给你
  - 导弹
  - DDOS : 洪水攻击，（饱和攻击）

## IP 协议

- **Internet Protocol** 互联网协议/网际协议

  - 负责数据从一台机器发送到另一台机器
  - 给互联网的每一台设备分配一个**唯一标识**（IP地址）

- TCP/UDP 是基于IP 协议的

- IP地址分为两种：

  - IPV4：**4字节**32位整数，并分成4段8位的二进制数，每8位之间用圆点隔开，每8位整数可以转换为一个0~255的十进制整数。

    格式：D.D.D.D 例如：255.255.255.255

    IPV4面临着一个资源耗尽的问题。

  - IPV6：**16字节**128位整数，并分成8段十六进制数，每16位之间用圆点隔开，每16位整数可以转换为一个0~65535的十进制数。

    格式：X.X.X.X.X.X.X.X 例如：FFFF.FFFF.FFFF.FFFF.FFFF.FFFF.FFFF.FFFF

- IPV4的应用分类：

  - A类：政府机构，**1**.0.0.1~**126**.255.255.254

  - B类：中型企业，**128**.0.0.1~**191**.255.255.254

  - C类：个人用户，**192**.0.0.1~**223**.255.255.254

  - D类：用于组播，224.0.0.1~239.255.255.254

  - E类：用于实验，240.0.0.1~255.255.255.254

  - 回环地址：127.0.0.1，指本机，一般用于测试使用。

  - 测试IP命令：ping D.D.D.D

    比如打开cmd，输入`ping www.baidu.com`

  - 查看IP命令：ipconfig

## Port

- **端口**表示计算机上一个程序的进程：

  - 不同的进程有不同的**端口号**，用来区分软件

    

- 端口号：在**通信实体**（比如QQ，微信）上进行网络通信程序的**唯一标识**

- 端口分类（一般是两个字节，0-65535）：

  - 公认端口：0~1023  	是被占用的，我们不能使用。

    - HTTP: 80
    - HTTPS:443
    - FTP:21
    - Telent:23

  - 程序注册端口：1024~49151，分配给用户或者程序

    可以使用

  - 动态或私有端口：49152~65535，尽量不用

    - 这样的端口号是动态分配的，而且两个网络程序不可能出现端口号相同的情况，
    - 但是对于TCP/UDP协议，他们是两套端口号，它们端口号相同是不冲突的，每个协议都是**独立的**。
    
  - 命令行输入

  - ```bash
    netstat -ano #查看所有端口
    netstat -ano|findstr "端口号" #查看指定的端口
    tasklist|findstr "端口号" #查找具体的进程
    ```

  - 

- 常用端口：

  - MySql:3306
  - Oracle:1521
  - Tomcat:8080
  - SMTP:25
  - Web服务器:80
  - FTP服务器:21

## InetAddress 类

- Java.net.InetAddress 只包含静态方法
- 于网络编程相关的包都在java.net
- 概念：表示互联网协议（IP）地址对象，**封装**了与该IP地址相关的所有信息，并提供获取信息的常用方法。
- 常用方法：
  - `public static InetAddress getLocalHost()`获得本地主机地址对象。
  - `public static InetAddress getByName(String host)`根据主机名称获得地址。
  - `public static InetAddress[] getAllByName(String host)`获得所有相关地址对象。
  - `public String getHostAddress()`获取IP地址字符串
  - `public String getHostName()`获得IP地址主机名

- InetAddress 类的使用:
- 1. 创建本地 IP 地址对象

 * 2. 创建局域网 IP 地址对象
 * 3. 创建外网 IP 地址对象

```java
/**
 * InetAddress 的使用
 * 1. 创建本地 IP 地址对象
 * 2. 创建局域网 IP 地址对象
 * 3. 创建外网 IP 地址对象
 */
public class Demo01 {
    public static void main(String[] args) throws Exception{
        //1 创建本地 IP 地址对象,常用前两种
        //1.1 InetAddress.getLocalHost()方法
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println("IP地址: " +localHost.getHostAddress() + "  主机名" + localHost.getHostName());
        //1.2 InetAddress.getByName("ip地址")
        InetAddress byName = InetAddress.getByName("192.168.1.4");
        System.out.println("IP地址: " +byName.getHostAddress() + "  主机名" + byName.getHostName());
        //1.3getByName("127.0.0.1")
        InetAddress byName1 = InetAddress.getByName("127.0.0.1");
        System.out.println("IP地址: " +byName1.getHostAddress() + "  主机名" + byName1.getHostName());
        //1.4getByName("localhost")
        InetAddress byName2 = InetAddress.getByName("localhost");
        System.out.println("IP地址: " +byName2.getHostAddress() + "  主机名" + byName2.getHostName());

        //2 创建局域网 IP 地址对象
//        InetAddress byName3 = InetAddress.getByName("192.168.1.5");//随便写的地址，所以，不可达
//        System.out.println("IP地址: " +byName3.getHostAddress() + "  主机名" + byName3.getHostName());
//        System.out.println("2秒内是否可达：" + byName3.isReachable(2000));

        //3 创建外网 IP 地址对象
        InetAddress byName4 = InetAddress.getByName("www.baidu.com");
        System.out.println("IP地址: " +byName4.getHostAddress() + "  主机名" + byName4.getHostName());
        System.out.println("2秒内是否可达：" + byName4.isReachable(2000));
        InetAddress[] allByName = InetAddress.getAllByName("www.baidu.com");//获得所有的ip地址
        for (InetAddress inetAddress : allByName) {
            System.out.println(inetAddress.getHostAddress());
        }

    }
}

```

# 网络编程

## 基于 TCP 的Socket网络编程

- Socket 编程：
  - Socket（套接字）是网络中的一个**通信结点**。
  - 分为客户端Socket和服务器ServerSocket。
  - 通信要求：IP地址+端口号。

### 开发步骤

**服务器端**：

- 创建ServerSocket，指定端口号。
- 调用accept等待客户端接入。
- 使用**输入流**，**接受请求数据**到服务器（等待）。
- 使用**输出流**，**发送响应数据**给客户端。
- 释放资源。

**客户端**：

- 创建Socket，指定服务器IP + 端口号。
- 使用**输出流**，**发送请求**数据到服务器。
- 使用**输入流**，**接受响应**数据到客户端（等待）。
- 释放资源。

### TCP 编程案例1

- 实现：客户端发送数据给服务器端

- ServerSocket 类，服务器端套接字
- Socket 类，客户端套接字
- 先运行服务器，再运行客户端 
- 文件：
  - TcpServer 类
  - TcpClient 类

**TcpServer 类**

- listener 只负责与客户端socket 建立联系，三次握手
- 当服务器的listener.accept() 接受到时，返回一个socket，负责与客户端的socket相互发送数据

```java
/**
 * 基于 TCP 协议的服务器端开发
 * 1 创建ServerSocket，指定端口号。
 * 2 调用accept(),等待客户端接入。
 * 3 获取输入流，读取客户端发送的数据
 * 4 获取输出流，发送数据给客户端。
 * 5 关闭释放资源。
 * 住：先运行服务器，再运行客户端
 */
public class TcpServer {
    public static void main(String[] args) throws IOException {
        //1 创建ServerSocket，指定端口号。
        ServerSocket listener = new ServerSocket(5205);//测试端口最好在1024之后，因为之前的已经被占用

        //2 调用accept(),等待客户端接入。是一个阻塞方法（如果客户端没有请求，则阻塞）
        System.out.println("服务器已启动");
        Socket socket = listener.accept();//返回的是客户端 Socket

        //3 获取输入流，读取客户端发送的数据
        InputStream inputStream = socket.getInputStream();//因为这个字节流不能接受中文，所以有下面的代码
        //把字节流转换为字符流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8));
        String data = bufferedReader.readLine();
        System.out.println("客户发送的数据： "+ data);

        //4 获取输出流，发送数据给客户端。(因为是简单案例，所以可选)
        //5 关闭释放资源。
        bufferedReader.close();
        socket.close();
        listener.close();
    }

}
```

**TcpClient 类**

```java
/**
 * 基于TCP 的客户端开发
 * 1 创建客户端套接字（Socket），指定服务器IP + 端口号。
 * 2 获取输出流，发送数据给服务器
 * 3 获取输入流，读取服务器回复的数据
 * 4 关闭释放资源。
 */
public class TcpClient {
    public static void main(String[] args) throws Exception{
        //* 1 创建客户端套接字（Socket），指定服务器IP + 端口号。
        Socket socket = new Socket("192.168.1.4",5205);
        // * 2 获取输出流，发送数据给服务器
        OutputStream outputStream = socket.getOutputStream();//得到一个字节输出流
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream, StandardCharsets.UTF_8));
        bufferedWriter.write("你好服务器，我是客户端");

        //* 3 获取输入流，读取服务器回复的数据（因为服务器端没选，所以这里也可选）
        //  4 关闭释放资源。
        bufferedWriter.close();
        socket.close();
    }
}
```

### TCP 编程案例2

- 实现：客户端上传文件给服务器端

- 文件：
  - TcpFileServer 类
  - TcpFileClient 类

```java 
/**
 * 案例2
 * TCP 服务器端
 * 实现:客户端上传文件给服务器端
 */
public class TcpFileServer {
    public static void main(String[] args) throws IOException {
        //1 创建 ServerSocket
        ServerSocket listener = new ServerSocket(5202);
        System.out.println("服务器已经启动");
        Socket socket = listener.accept();
        InputStream inputStream = socket.getInputStream();
        //4 边读取，边保存
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\2021java学习文件\\learnphoto\\tcp.jpg");
        byte[] buffer = new byte[1024*4];
        int count = 0;
        while ((count = inputStream.read(buffer)) != -1){
            fileOutputStream.write(buffer,0,count);
        }

        fileOutputStream.close();
        inputStream.close();
        socket.close();
        listener.close();
        System.out.println("接受完毕");
    }
}
```

```java
/**
 * 案例2
 * TCP 开发
 * 客户端
 */
public class TcpFileClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("192.168.1.4", 5202);

        OutputStream outputStream = socket.getOutputStream();
        //边读取文件，边发送
        FileInputStream fileInputStream = new FileInputStream("E:\\精选image\\timg4.jpg");
        byte[] buf = new byte[1024*4];
        int count = 0;
        while ((count = fileInputStream.read(buf)) != -1){
            outputStream.write(buf,0,count);
        }

        fileInputStream.close();
        outputStream.close();
        socket.close();

        System.out.println("发送完毕");
    }
}

```

### TCP 编程案例3

- 实现：实现多个客户端发送数据给服务器

- 文件：
  - TcpServer3
  - SokertThread
  - TcpClient3

```java
/**
 * 案例3：实现多个客户端发送数据给服务器
 * 服务器端
 */
public class TcpServer3 {
    public static void main(String[] args) throws Exception{
        ServerSocket listener = new ServerSocket(5203);

        System.out.println("服务器已经启动");
        while (true){
            Socket socket = listener.accept();
            System.out.println(socket.getInetAddress() + "进来了");
            //创建线程对象，负责接受数据
            new SocketThread(socket).start();
        }
    }
}
```

```java

/**
 * 案例3 的线程类
 *
 */
public class SocketThread extends Thread{
    private Socket socket;

    public SocketThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        if (socket != null){
            BufferedReader bufferedReader = null;
            try {
                InputStream inputStream = socket.getInputStream();
                bufferedReader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8));

                while (true){
                    String data = bufferedReader.readLine();
                    if (data == null){//客户端已经关闭
                        break;//则服务器退出
                    }
                    System.out.println(socket.getInetAddress() + "说了：" + data);
                    if (data.equals("886") || data.equals("byebye")){
                        break;
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    bufferedReader.close();
                    socket.close();
                    System.out.println(socket.getInetAddress()+ "退出了");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            
        }
    }
}

```

```java
/**
 * 案例3
 * TCP 客户端，
 *  * 持续一直向服务器发送数据
 */
public class TcpClient3 {
    public static void main(String[] args) throws Exception{
        Socket socket = new Socket("192.168.1.4",5203);
        OutputStream outputStream = socket.getOutputStream();
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream, StandardCharsets.UTF_8));

        //3 控制输入
        Scanner input = new Scanner(System.in);
        while(true){
            String data = input.nextLine();
            bufferedWriter.write(data);
            bufferedWriter.newLine();//发送换行符，因为，服务器线程里是用的readLine()
            bufferedWriter.flush();
            if (data.equals("886") || data.equals("byebye")){
                break;
            }
        }
        //4 关闭
        bufferedWriter.close();
        socket.close();
    }
}

```

### TCP 编程案例4

- 实现：使用 Socket 实现注册登录
  - 使用 Socket 编程实现服务器端注册
    - 注册信息保存在properties 文件中
    - 封装格式
    - id = {id:"1001",name:"tom",pwd:"123",age:20}
    - 注册成功后返回字符串“注册成功”
  - 使用 Socket 编程实现客户端登陆
    - 获取 properties 文件中的用户信息，进行用户 id 与密码的校验
    - 校验成功后返回字符串“登录成功”

- 项目拥有的文件：
  - UserServer 类
  - Tools 类
  - RegistThread 类
  - LoginThread 类
  - UserClient 类

**UserServer** 类

```java
/**
 * 案例4 使用 Socket 实现注册登录
 */
public class UserServer {
    public static void main(String[] args) {
        new RegistThread().start();
        new LoginThread().start();
    }
}

```

**Tools 类**

```java
/**
 * 案例4
 * 工具类
 * 负责加载文件和保存所有文件
 */
public class Tools {
    //1 加载属性文件
    public static Properties loadProperties() {
        //1 创建集合
        Properties properties = new Properties();
        //2 判断文件是否存在
        File file = new File("users.properties");
        if (file.exists()){
            FileInputStream fileInputStream = null;
            try {
                fileInputStream = new FileInputStream(file);
                properties.load(fileInputStream);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                if (fileInputStream != null) {
                    try {
                        fileInputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }


        }
        return properties;
    }
    //2 保存属性文件
    public static void saveProperties(String json){
        String[] infos = json.substring(1, json.length() - 1).split(",");//去掉大括号,且用逗号分割
        String id = infos[0].split(":")[1];
        FileOutputStream fileOutputStream = null;
        //保存
        try {
            fileOutputStream = new FileOutputStream("users.properties",true);
            Properties properties = new Properties();
            properties.setProperty(id,json);
            properties.store(fileOutputStream,null);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (fileOutputStream != null){
                try {
                    fileOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

**RegistThread 类**

```java
/**
 * 案例4
 * 实现注册功能的线程类
 */
public class RegistThread extends Thread{
    @Override
    public void run() {
        try {
            ServerSocket listener = new ServerSocket(5204);
            System.out.println("注册服务器已经启动");
            Socket socket = listener.accept();
            //获取输入输出流
            BufferedReader bufferedReader =
                    new BufferedReader(new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
            BufferedWriter bufferedWriter =
                    new BufferedWriter(new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8));
            //4 接受客户端发送的数据 {id : 1001 , name : tang , pwd : 036 , age = 21}
            String json = bufferedReader.readLine();
            String[] infos = json.substring(1, json.length() - 1).split(",");//去掉大括号,且用逗号分割
            String id = infos[0].split(":")[1];
            //5 加载属性文件 写了一个工具类Tools.loadProperties()
            Properties properties = Tools.loadProperties();
            //6 判断id有没有
            if (properties.containsKey(id)){
                //有
                bufferedWriter.write("此用户已经存在");
            } else {//没有
                //保存属性文件 自己的工具类Tools.saveProperties()
                Tools.saveProperties(json);
                bufferedWriter.write("注册成功");
            }
            bufferedWriter.newLine();
            bufferedWriter.flush();
            bufferedWriter.close();
            bufferedReader.close();
            socket.close();
            listener.close();


        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**LoginThread 类**

```java
/**
 * 案例4
 * 负责登录的线程
 */
public class LoginThread extends Thread{
    @Override
    public void run() {
        try {
            //1 创建ServerSocket
            ServerSocket listener = new ServerSocket(5206);
            System.out.println("登录服务器已经启动");
            //2 调用accept()方法
            Socket socket = listener.accept();
            //获取输入输出流
            BufferedReader bufferedReader =
                    new BufferedReader(new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
            BufferedWriter bufferedWriter =
                    new BufferedWriter(new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8));
            //4 接受客户端发送的数据 {id : 1001  , pwd : 036 }
            String json = bufferedReader.readLine();
            // id : 1001  , pwd : 036
            String[] infos = json.substring(1, json.length() - 1).split(",");//去掉大括号,且用逗号分割
            String id = infos[0].split(":")[1];
            //5 加载属性文件 写了一个工具类Tools.loadProperties()
            Properties properties = Tools.loadProperties();
            //6 判断是否存在
            if (properties.containsKey(id)){
                //判断密码是否正确
                String pwd = infos[1].split(":")[1];
                String value = properties.getProperty(id);
                String[] arr = value.substring(1, value.length() - 1).split(",");
                String pwd2 = arr[2].split(":")[1];
                if (pwd.equals(pwd2)) {
                    bufferedWriter.write("登录成功");
                }else {
                    bufferedWriter.write("密码错误 ");
                }
            } else {//没有


                bufferedWriter.write("用户名或密码错误");
            }
            bufferedWriter.newLine();
            bufferedWriter.flush();
            bufferedWriter.close();
            bufferedReader.close();
            socket.close();
            listener.close();


        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

**UserClient 类**

```java
/**
 * 案例四
 * 用户注册登录客户端
 */
public class UserClient {
    public static void main(String[] args) throws Exception{
        System.out.println("请选择：1 注册，2 登录");
        Scanner input = new Scanner(System.in);
        int choice = input.nextInt();
        switch (choice){
            case 1:
                regist();
                break;
            case 2:
                login();
            default:
                break;
        }
    }
//注册
    public static void regist() throws Exception{
        //1 创建Socket
        Socket socket = new Socket("192.168.1.4", 5204);
        //2 获取输入输出流
        BufferedReader bufferedReader =
                new BufferedReader(new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
        BufferedWriter bufferedWriter =
                new BufferedWriter(new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8));
        //3 获取用户信息
        String json = getRegistInfo();
        //4 发送
        bufferedWriter.write(json);
        bufferedWriter.newLine();
        bufferedWriter.flush();
        //5 接收回复
        String reply = bufferedReader.readLine();
        System.out.println("服务器回复："+ reply);

        //6 关闭
        bufferedWriter.close();
        bufferedReader.close();
        socket.close();
    }
//获取注册信息
    public static String getRegistInfo(){
        Scanner input = new Scanner(System.in);
        System.out.println("请输入用户编号");
        int id = input.nextInt();
        System.out.println("请输入姓名");
        String name = input.next();
        System.out.println("请输入密码");
        String pwd = input.next();
        System.out.println("请输入年龄");
        int age = input.nextInt();
        //{id : 1001 , name : tang , pwd : 036 , age = 21}
        String json = "{id:"+id+",name:"+name+",pwd:"+pwd+",age:"+age+"}";
        return json;
        }
//登录
    public static void login() throws Exception{
//1 创建Socket
        Socket socket = new Socket("192.168.1.4", 5206);
        //2 获取输入输出流
        BufferedReader bufferedReader =
                new BufferedReader(new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
        BufferedWriter bufferedWriter =
                new BufferedWriter(new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8));
        //3 获取用户信息
        String json = getLoginInfo();
        //4 发送
        bufferedWriter.write(json);
        bufferedWriter.newLine();
        bufferedWriter.flush();
        //5 接收回复
        String reply = bufferedReader.readLine();
        System.out.println("服务器回复："+ reply);

        //6 关闭
        bufferedWriter.close();
        bufferedReader.close();
        socket.close();
    }
//获取登录信息
    public static String getLoginInfo(){
        Scanner input = new Scanner(System.in);
        System.out.println("请输入用户编号");
        int id = input.nextInt();
        System.out.println("请输入密码");
        String pwd = input.next();
        //{id : 1001  , pwd : 036 }
        String json = "{id:"+id+",pwd:"+pwd+"}";
        return json;
    }
}
```

## 基于UDP 

- 用java.net.**DatagramPacket**用于**发送**，java.net.**DatagramSocket **用于**接收**
- UDP 没有服务器、客户端这个说法

### UDP 实现消息发送

**发送端**

- 发送端也可以有一个端口，成为接收端
- 文件：
  - UdpClientDemo01
  - UdpServerDemo02

**发送端**

```java
/**
 *不需要连接服务器
 */
public class UdpClientDemo01 {
    public static void main(String[] args) throws Exception {
        //1. 建立一个Socket
        DatagramSocket datagramSocket = new DatagramSocket();

        //2 建一个包
        String message = "hello server";
        InetAddress localhost = InetAddress.getByName("localhost");
        int port = 5209;
        //数据，数据的起始长度，要发送给谁
        DatagramPacket datagramPacket = new DatagramPacket(message.getBytes(),0,message
        .getBytes().length,localhost,port);

        //3 发送包
        datagramSocket.send(datagramPacket);
        //4 关闭流
        datagramSocket.close();

    }
}
```

**接收端**

```java
/**
 * 还是要等待发送端的连接
 */
public class UdpServerDemo02 {
    public static void main(String[] args) throws Exception {
        DatagramSocket datagramSocket = new DatagramSocket(5209);

        byte[] buffer = new byte[1024];
        DatagramPacket datagramPacket = new DatagramPacket(buffer, 0, buffer.length);

        datagramSocket.receive(datagramPacket);//阻塞接受

        System.out.println(datagramPacket.getAddress().getHostAddress());
        System.out.println(new String(datagramPacket.getData(),0,datagramPacket.getLength()
        ));

        datagramSocket.close();


    }
}
```

### UDP 实现聊天

- 循环发送消息
- 文件：
  - UdpSender01
  - UdpReceive01
- 循环发送消息

```java
public class UdpSender01 {
    public static void main(String[] args) throws Exception{
        DatagramSocket datagramSocket = new DatagramSocket(5208);

        //准备数据 控制台读取 System.in
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        while (true){
            String data = bufferedReader.readLine();//因为不可读，所以必须转成字节数据
            byte[] datas = data.getBytes();
            DatagramPacket datagramPacket = new DatagramPacket(datas,0,datas.length,new InetSocketAddress("localhost",5210));

            datagramSocket.send(datagramPacket);
            if (data.equals("bye")){
                break;
            }
        }
        datagramSocket.close();
    }
}
```

- 循环接受消息

```java
public class UdpReceive01 {
    public static void main(String[] args) throws Exception{
        DatagramSocket datagramSocket = new DatagramSocket(5210);
        while (true){
            //准备接受包裹
            byte[] container = new byte[1024];
            DatagramPacket datagramPacket = new DatagramPacket(container,0,container.length);
            datagramSocket.receive(datagramPacket);//阻塞式接受包裹

            //断开连接 bye
            byte[] data = datagramPacket.getData();
            String receiveData = new String(data, 0, data.length);
            System.out.println(receiveData);
            if (receiveData.equals("bye")){
                break;
            }
        }
        datagramSocket.close();
    }
}
```

### UDP 多线程在线咨询

- 两个人都可以是发送方，也可以是接收方
- 文件：
  - SendThread
  - ReceiveThread
  - Student
  - Teacher

**SendThread**

```java
public class SendThread implements Runnable{
    DatagramSocket datagramSocket = null;
    BufferedReader bufferedReader = null;

    private String toIP;
    private int fromPort;
    private int toPort;

    public SendThread(int fromPort,String toIP, int toPort) {
        this.toIP = toIP;
        this.fromPort = fromPort;
        this.toPort = toPort;

        try {
            datagramSocket = new DatagramSocket(fromPort);
            bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true){
            String data = null;
            try {
                data = bufferedReader.readLine();//因为不可读，所以必须转成字节数据
                byte[] datas = data.getBytes();
                DatagramPacket datagramPacket = new DatagramPacket(datas,0,datas.length,new InetSocketAddress(this.toIP,this.toPort));

                datagramSocket.send(datagramPacket);
                if (data.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
        datagramSocket.close();
    }
}

```

**ReceiveThread**

```java
public class ReceiveThread implements Runnable {
    DatagramSocket datagramSocket = null;

    private int port;
    private String msgFrom;

    public ReceiveThread(int port,String msgFrom) {
        this.port = port;
        this.msgFrom  = msgFrom;
        try {
            datagramSocket = new DatagramSocket(port);
        } catch (SocketException e) {
            e.printStackTrace();
        }

    }

    @Override
    public void run() {
        while (true){
            try {
                //准备接受包裹
                byte[] container = new byte[1024];
                DatagramPacket datagramPacket = new DatagramPacket(container,0,container.length);
                datagramSocket.receive(datagramPacket);//阻塞式接受包裹

                //断开连接 bye
                byte[] data = datagramPacket.getData();
                String receiveData = new String(data, 0, data.length);

                System.out.println(msgFrom+ ":"+receiveData);

                if (receiveData.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        datagramSocket.close();
    }
}

```

**Student**

```java
public class Student {
    public static void main(String[] args) {
        //开启两个线程，因为又要接收，又要发送
        new Thread(new SendThread(7777,"localhost",9999)).start();
        new Thread(new ReceiveThread(8888,"老师")).start();
    }

}
```

**Teacher**

```java
public class Teacher {
    public static void main(String[] args) {
        new Thread(new SendThread(5555,"localhost",8888) ).start();
        new Thread(new ReceiveThread(9999,"学生")).start();
    }
}
```

# URL

- 统一资源定位符：定位资源的，定位互联网上的某一个资源

- DNS，是域名解析，比如，把www.baidi.com 解析成 URL

- ```java
  URL 格式：
      协议：/ip地址：端口/项目名/资源
  注意：可以少但是不能多，比如有些就没有项目名
  ```

- java 里有一个URL的类 java.net.URL

## **URL的常用方法**

```java
	/**
 * URL类 常用方法
 */
public class Demo01 {
    public static void main(String[] args) throws MalformedURLException {
        //创建一个url
        URL url = new URL("http://localhost:8080/helloworld/index.jsp?username=joker&password=123");
        //常用方法
        System.out.println(url.getProtocol());//协议
        System.out.println(url.getHost());//主机ip
        System.out.println(url.getPort());//端口
        System.out.println(url.getPath());//文件
        System.out.println(url.getFile());//全路径
        System.out.println(url.getQuery());//查询参数

    }
}
```

## URL 下载网络资源

```java
/**
 * URL 下载网络资源
 */
public class Demo02 {
    public static void main(String[] args) throws Exception {
        //随便找一个网上url
        URL url = new URL("https://img.souutu.com/2020/0430/20200430100647480.jpg");

        HttpURLConnection urlConnection =(HttpURLConnection) url.openConnection();

        InputStream inputStream = urlConnection.getInputStream();
        FileOutputStream fos = new FileOutputStream("urlTest.jpg");//注意文件格式，与url的对应

        byte[] buffer = new byte[1024];
        int len;
        while ((len = inputStream.read(buffer)) != -1){
            fos.write(buffer,0,len);
        }

        fos.close();
        inputStream.close();
        urlConnection.disconnect();
    }
}
```

