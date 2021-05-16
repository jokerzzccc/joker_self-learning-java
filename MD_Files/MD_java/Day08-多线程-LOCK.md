#  Day08-多线程（Thread）

## 概念

- 程序：是一个静态的概念，是指令和数据的有序集合

### 进程

- 是一个动态的概念，是执行程序的一次执行过程，是系统**资源分配**的基本单位
  - 目前操作系统都是支持多进程的，可以同时执行多个进程，通过进程ID 区分
  - 单核 cpu 在同一个时刻只能运行一个进程：宏观串行，微观并行

### 线程

- 通常一个进程可以包含多个线程，一个进程中至少有个线程，否则进程没有意义。线程是CPU **基本调度和执行**的基本单位
- 注意：很多线程是模拟出来的，真正的多线程是指有多个CPU，即多核，如服务器。如果是模拟出来的多线程，即在一个cpu 的情况下，在同一个时间点，cpu 只能执行一个代码，因为切换的很快，就有同时执行的错觉。

1. 线程的组成（任何一个线程都有基本的组成部分）：

   - cpus 时间片：操作系统会为每个线程分配执行时间

   - 运行数据：

     （1）堆空间：存储线程需使用的**对象**，多个线程可以**共享**堆中的数据

     （2）栈空间：存储线程需使用的**局部变量**，每个线程都拥有**独立**的栈

2. 线程的特点：
   - 线程抢占式执行：效率高；可防止单一线程长时间独占CPU
   - 在单核 CPU中，宏观串行，微观并行



### 核心概念：

- 线程就是独立的执行路径
- 在程序运行时，即使没有自己创建线程，后台也会有多个线程，如主线程，gc 线程
- main()  称为主线程，是系统的入口，用于执行整个程序
- 在一个进程中，如果开辟了多个线程，线程的运行是由调度器安排调度，调度器是与操作系统紧密相关的，先后顺序是不能人为干预的
- 对同一份资源操作时，会存在资源抢夺问题，需要加入并发控制
- 线程会带来额外的开销，如：cpu 调度时间，并发控制开销
- 每个线程在自己的工作内存交互，内存控制不当，会造成数据不一致
- 进程间不能共享数据段地址，同一进程的线程之间可以



### 普通方法调用和多线程：

- 普通方法调用：主线程调用run() ，只有主线程一条调用路径
- 多线程：主线程调用 start() ,多天执行路径，主线程和子线程**交替执行**

## Thread 的创建

有三种创建方式：

- Thread class:继承 Thread 类（重点）
- Runnable 接口:实现Runnable 接口（重点）
- Callable 接口:实现Callable 接口（了解），工作之后常用

### 继承 Thread 类

- 自定义线程类继承 Thread 类
- 重写 run() 方法，编写线程执行体
- 创建线程对象，调用 start() 方法启动线程
- 注意：线程不一定立即执行，CPU 安排调度
- 不建议使用，避免 oop 单继承局限性

1. 获取线程 ID 和线程名称：
   1. 在Thread 的子类中调用 this.getId() 或 this.getName() ，这种方式只适合继承了Thread 类的线程类
   2. 推荐：使用 Thread.currentThread().getId() 和 Thread.currentThread().getName()
2. 修改线程名称：
   1. 调用线程对象的 setName() 方法
   2. 使用线程子类的构造方法赋值

```java
/**
 * 创建线程方式一：继承 Thread 类，调用start() 方法
 * 注：线程开启不一定立即执行，由 cpu 安排调度
 *
 * @author joker_chen
 */
public class testThread1 extends Thread{
    @Override
    public void run() {
        //run() 方法线程载体
        for (int i = 0; i < 10; i++) {
            System.out.println("支----线程" + i);
        }
    }

    public static void main(String[] args) {
        //main线程，主线程
        //创建一个线程对象
        testThread1 testThread1 = new testThread1();
        //调用 start() 开启线程
        testThread1.start();

        for (int i = 0; i < 200; i++) {
            System.out.println("主线程" + i);
        }
    }
}
```

#### 案例：多线程下载图片

- 要导入 commons-io库的jar包，直接百度
- 步骤:
  - 建立文件下载工具类
  - 重写run()
  - 创建多个线程，并 start()

```java 
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;

/**
 * 练习 Thread 类，实现多线程同步下载图片
 */
public class testThread2 extends Thread{

    private String name;
    private String url;

    public testThread2(String url,String name){
        this.url = url;
        this.name = name;
    }

//下载方法执行体
    @Override
    public void run() {
        webDownloader webDownloader = new webDownloader();
        webDownloader.downloader(url,name);
        System.out.println("下载了文件名为：" + name);
    }

    public static void main(String[] args) {
        testThread2 t1 = new testThread2("http://img17.3lian.com/201612/20/99b8790a4faae5fcebbe838eb24213ac.jpg","1.jpg");
        testThread2 t2 = new testThread2("http://img17.3lian.com/201612/20/99b8790a4faae5fcebbe838eb24213ac.jpg","2.jpg");
        testThread2 t3 = new testThread2("http://img17.3lian.com/201612/20/99b8790a4faae5fcebbe838eb24213ac.jpg","3.jpg");
//由下载结果可知，线程的执行顺序是由 cpu 调度的
        t1.start();
        t2.start();
        t3.start();
    }
}
//下载器
class webDownloader{
    //下载方法
    public void downloader(String url,String name){
        try {
            FileUtils.copyURLToFile(new URL(url),new File(name));//commons-io 库里的，把文件连接转为文件
        }catch (IOException e){
            e.printStackTrace();
            System.out.println("IO 异常，downloader方法出现问题");
        }

    }
}
```

#### 案例：四个窗口共卖100张票

```java
public class Ticket extends Thread{
    private  int ticket = 100;
    
    public Ticket(){
    }

    public Ticket(String name){
            super(name);

    }

    @Override
    public void run() {
        while (true){
            if (ticket > 0){
                ticket--;
                System.out.println(Thread.currentThread().getName() + "卖出了一张票，还剩" +ticket);

            }else {
                break;
            }
        }
    }
}
```

```java
/**
 * 线程案例：四个窗口共卖100张票
 */
public class testTickets {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        new Thread(ticket,"窗口1").start();
        new Thread(ticket,"窗口2").start();
        new Thread(ticket,"窗口3").start();
        new Thread(ticket,"窗口4").start();
    }
}
```



### 实现 Runable 接口

- 定义 myRunnable 类实现 Runnable 接口

- 实现 run() 方法，编写线程执行体

- 创建线程对象，调用 start() 启动线程

- 推荐使用，避免单继承局限性，方便同一个对象被多个线程使用

  - ```java
    //一份资源
    StartThread station = new StartThread();
    //多个代理
    new Thread(station,"小明").start();
    new Thread(station,"小红").start();
    new Thread(station,"小狗").start();
    ```

```java
/**
 * 创建线程方式2：实现 Runnable 接口，重写 run() 方法，执行线程需要丢入 Runnable 接口实现类，调用 start() 方法
 */
public class myRunnable implements Runnable{
    @Override
    public void run() {
        //run() 方法线程载体
        for (int i = 0; i < 10; i++) {
            System.out.println("支----线程" + i);
        }
    }

    public static void main(String[] args) {
        //main线程，主线程
        //创建 Runnable 接口的实现类对象
        myRunnable myRunnable = new myRunnable();
        //创建一个线程对象,通过线程对象来开启我们的线程，这叫代理
        new Thread(myRunnable).start();

        for (int i = 0; i < 200; i++) {
            System.out.println("主线程" + i);
        }
    }
}
```

#### 案例：一个资源被多个线程使用

```java
/**
 * 使用 Runnable 实现类，来完成多个线程操作同一个对象
 * 案例：买火车票
 * 发现问题：多个线程操作同一个资源的情况下，线程不安全，数据紊乱
 */
public class testThread4 implements Runnable {
    //票数
    private int ticketNums = 10;


    @Override
    public void run() {
        while (true){
            if (ticketNums <= 0){
                break;
            }

            try {
                Thread.sleep(1000);//模拟延时
            } catch (InterruptedException e){
                e.printStackTrace();
            }
            
            System.out.println(Thread.currentThread().getName()+"拿到了第" + ticketNums +"张票");
            ticketNums--;
        }

    }

    public static void main(String[] args) {
        testThread4 ticket = new testThread4();

        new Thread(ticket,"我").start();
        new Thread(ticket,"爱").start();
        new Thread(ticket,"你").start();

    }
}
```

#### 案例：龟兔赛跑-Race

- 要求：
  - 首先来个赛道距离，然后要离重点越来越近
  - 判断比赛是否结束
  - 打印出胜利者
  - 龟兔赛跑开始
  - 乌龟要赢，所以用延时模拟兔子睡觉
  - 乌龟赢得比赛

```java
/**
 * 案例：龟兔赛跑-Race
 * 要求:
 *
 */
public class Race implements Runnable{
    private static String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            //模拟兔子的休息，就是使用延时
            if (Thread.currentThread().getName().equals("rabbit") && i % 10 == 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e){
                    e.printStackTrace();
                }
            }

            //判断比赛是否结束
            boolean flag = geamOver(i);
            if (flag) {
                break;
            }
            System.out.println(Thread.currentThread().getName() + "跑了" + i +"步数");
        }
    }

    public static boolean geamOver(int steps){
        if (winner != null){
            return true;
        }
        if (steps == 100){
            winner = Thread.currentThread().getName();
            System.out.println("winner is " + winner);
            return true;
        }
        return false;
    }

    public static void main(String[] args) {
        Race race = new Race();

        new Thread(race,"rabbit").start();
        new Thread(race,"turtle").start();
    }
}
```

#### 案例：存钱、取钱

- 哥哥在卡里存钱，妹妹从卡里取钱
- 使用匿名内部类的方式完成

```java
/**
 * 银行卡
 */
public class BankCard {
    private double money;

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }
}

```



```java
/**
 * 案例：存钱，取钱，使用匿名内部类
 *
 * @author joker_chen
 */
public class TestBankCard{
    public static void main(String[] args) {
        BankCard bankCard = new BankCard();
        //创建存钱
        Runnable addMoney = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    bankCard.setMoney(bankCard.getMoney() + 1000);
                    System.out.println(Thread.currentThread().getName() + "存了1000,余额是： " + bankCard.getMoney()  );
                }
            }
        };

        //创建取钱
        Runnable subMoney = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    if (bankCard.getMoney() >= 1000){
                        bankCard.setMoney(bankCard.getMoney() - 1000);
                        System.out.println(Thread.currentThread().getName() + "取了1000，余额是：" + bankCard.getMoney());
                    } else {
                        System.out.println("余额不足");
                        i--;//保证可以取完，不会i到了10，还没取出钱
                    }

                }
            }
        };

        //创建线程对象
        new Thread(addMoney,"哥哥").start();
        new Thread(subMoney,"妹妹").start();
    }
}
```

## Lambda 表达式

- lambda 表达式的形式：
  -  
- 为什么要使用 lambda 表达式：
  - 避免内部类定义过多
  - 可以让你的代码看起来很简洁
  - 去掉了一堆没有意义的代码，只留下核心逻辑
- Functional interface (函数式接口) 是Java8 lambda 表达式的关键
- **函数式接口**的定义：
  - 任何接口，如果只包含唯一一个抽象方法，那么他就是一个函数式接口
  - 对于函数式接口，我们可以通过lambda 表达式来创建该接口的对象

### Lambda 表达式的推导

```java
/**
 * 推到 lambda 表达式
 */
public class TestLambda1 {
//3 静态内部类
    static class Like2 implements Ilike{

        @Override
        public void lambda() {
            System.out.println("I like lambda2");
        }
    }

    public static void main(String[] args) {
        Ilike like  = new Like();
        like.lambda();

        like = new Like2();
        like.lambda();

        //4 局部内部类
        class Like3 implements Ilike{
            @Override
            public void lambda() {
                System.out.println("I like lambda3");
            }
        }

        like = new Like3();
        like.lambda();
        //5匿名内部类
        like = new Ilike() {
            @Override
            public void lambda() {
                System.out.println("I like lambda4");
            }
        };
        like.lambda();

        //6使用labmda 表达式

        like = ()->{
            System.out.println("I like lambda5");
        };
        like.lambda();
    }
}


//1 定义一个函数式接口
interface Ilike{
    void lambda();
}

//2.实现类
class Like implements Ilike{

    @Override
    public void lambda() {
        System.out.println("I like lambda");
    }
}
```

### Lambda 表达式的简化

总结：

- 简化花括号,lambda 表达式只有一行的情况下，才能简化为一行，如果有多行，就要用代码块包裹
- 前提是函数式接口
- 多个参数也可以去点参数类型，要去掉就都去掉，必须加上括号
- 一般就是去掉参数类型

```java
/**
 * lambda 表达式的简化
 *
 * 前提是接口时函数式接口
 * 多个参数也可以去点参数类型，要去掉就都去掉，必须加上括号
 */
public class TestLambda2 {
    public static void main(String[] args) {
//        ILove love = (String a )->{
//            System.out.println("I love you" + a);
//
//        };
//        //简化1 ，去掉参数返回类型
//        love = (a)->{
//            System.out.println("I love you" + a);
//        };
//        //简化2 ，简化括号
//        love = a->{
//            System.out.println("I love you" + a);
//        };
//
        //简化3，简化花括号,lambda 表达式只有一行的情况下，才能简化为一行，如果有多行，就要用代码块包裹
         ILove love = a->System.out.println("I love you" + a);

        love.love("three thousand times 4");
    }
}
interface ILove{
    void love(String  str );
}
```



## 线程常用的方法

### 休眠 sleep() 

- public static void sleep(long mills) 当前线程主动休眠mills 毫秒

### 放弃 yield() 

- public static void yeild() 当前线程主动放弃时间片，回到就绪状态，竞争下一次时间片

### 加入 join() 

- public final void join()
- 允许其他线程加入到当前线程当中 ，并阻塞当前线程，直到加入线程执行完毕
- 只能由对象来调用，sleep() 和 yield() 是用类来调用

```java
public class JoinThread extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            System.out.println(Thread.currentThread().getName()+"-------"+i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public class TestJoin {
    public static void main(String[] args) throws InterruptedException {
        JoinThread joinThread = new JoinThread();

        joinThread.start();
        joinThread.join();
        //for 主线程，即默认 main() 是主线程
        for (int i = 0; i < 20; i++) {
            System.out.println(Thread.currentThread().getName() + "========"  + i);
            Thread.sleep(50);
        }
    }
}
```

### 优先级 setPriority() 

- 线程对象.setPriority()
- 线程优先级为1-10，默认为5，优先级越高，表示获取 CPU 机会越多

### 守护线程 setDaemon()

- 线程对象.setDaemon(true) ，设置为守护线程
- 线程有两类：用户线程（前台线程），守护线程（后台线程）
- 创建程序默认为前台线程。
- 如果程序中所有前台程序都执行完毕了，后台程序会自动结束
- 垃圾回收器线程属于守护线程

## 线程安全问题

- 当多线程并发访问临界资源时，如果破坏原子操作，可能会造成数据不一致
- 原子操作：不可分割的多步操作，被视作一个整体，其顺序和步骤不可打乱和缺省
- 临界资源：共享资源（同一对象），一次仅允许一个线程使用，才可保证其正确性

```java
/**
 * 线程安全问题,两个线程向同一个数组输入
 */
public class TreadSafe {
    private static int index;
    public static void main(String[] args) throws InterruptedException {
        String[] str = new String[5];

        //创建两个操作
        Runnable runnable1 = new Runnable() {
            @Override
            public void run() {
               
                    str[index] = "hello";
                    index++;
                

            }
        };

        Runnable runnable2 = new Runnable() {

            @Override
            public void run() {
                
                    str[index] = "world";
                    index++;
                

            }
        };

        //创建两个线程对象
        Thread a = new Thread(runnable1, "a");

        Thread b = new Thread(runnable2, "b");

        a.start();
        b.start();

        a.join();//加入线程
        b.join();

        System.out.println(Arrays.toString(str));

    }
}

```



### 同步

#### 方式一：同步代码块

- synchronized(临界资源对象) {//对临界资源加锁

  //代码（原子操作）

  }

```java
 synchronized (str){//同步代码块
                    str[index] = "hello";
                    index++;
                }
```

##### 对于卖票的使用

- 对于案例：四个窗口共卖100张票的 的使用，有两种加锁方式

  - 1 自己创建锁对象

  - ```java
    public class Ticket extends Thread{
        private  int ticket = 100;
        private Object obj = new Object();//自己创建锁
    
        public Ticket(){
        }
    
        public Ticket(String name){
                super(name);
    
        }
    
        @Override
        public void run() {
            while (true){
                synchronized (obj){
                    if (ticket > 0){
                        ticket--;
                        System.out.println(Thread.currentThread().getName() + "卖出了一张票，还剩" +ticket);
    
                    }else {
                        break;
                    }
                }
            }
        }
    }
    ```

  - 2 用this 当锁，表示当前对象

    ```java
    synchronized (this){//2 用this 当锁，便是当前对象，即 临界资源 ticket
                    if (ticket > 0){
                        ticket--;
                        System.out.println(Thread.currentThread().getName() + "卖出了一张票，还剩" +ticket);
    
                    }else {
                        break;
                    }
                }
    ```

- 注意：

  - 每个对象都有一个互斥锁标记，用来分配给进程
  - 只有拥有对象互斥锁标记的线程，才能进入该对象加锁的同步代码块
  - 线程退出同步代码块时，会释放相应的互斥锁标记

##### 对于存钱、取钱的使用

```java
public class TestBankCard{
    public static void main(String[] args) {
        BankCard bankCard = new BankCard();
        //创建存钱
        Runnable addMoney = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    synchronized (bankCard){
                        bankCard.setMoney(bankCard.getMoney() + 1000);
                        System.out.println(Thread.currentThread().getName() + "存了1000,余额是： " + bankCard.getMoney()  );
                    }

                }
            }
        };

        //创建取钱
        Runnable subMoney = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    synchronized (bankCard){
                        if (bankCard.getMoney() >= 1000){
                            bankCard.setMoney(bankCard.getMoney() - 1000);
                            System.out.println(Thread.currentThread().getName() + "取了1000，余额是：" + bankCard.getMoney());
                        } else {
                            System.out.println("余额不足");
                            i--;//保证可以取完，不会i到了10，还没取出钱
                        }
                    }
                }
            }
        };

        //创建线程对象
        Thread brother = new Thread(addMoney,"哥哥");
        Thread sister = new Thread(subMoney,"妹妹");

        brother.start();
        sister.start();
    }
}
```

#### 方式二：同步方法

- ```
  
  synchronized 返回值类型 方法名称（形参列表0）{//即对当前对象（this）加锁
      //代码（原子操作）
  }
  ```

##### 对于卖票的使用

```java
//同步方法的的使用
    @Override
    public void run() {
        while (true){
            if (!sale()){
                break;
            }
        }
    }
//方法不是静态的时候，就是 锁this,谁调用，就锁谁
//方法钱加了静态 static 的时候，public static synchronized boolean sale() ,就是锁 类对象，就相当于在同步代码块里写 Ticket.class
    public synchronized boolean sale(){
        if (ticket <= 0) {
            return false;
        }

        System.out.println(Thread.currentThread().getName() + "卖出了一张票，还剩" + ticket);
        ticket--;

        return true;
    }
```

#### 同步规则

- 只有在调用包含同步代码快的方法，或者同步方法时，才需要对象的锁标记
- 如调用不包含同步代码块的方法，或者普通方法时，则不需要锁标记 ，可直接调用
- 已知 JDK 中线程安全的类：
  - StringBuffer
  - Vector
  - Hashtable
  - 以上类中的公开方法，都是 synchonized 修饰的方法

#### 线程的状态 总结

- 

![线程的状态](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-11_01-09-45.png)

- 可以通过`getState()`方法获取当前线程的状态。
  - NEW
  - RUNNABLE  就绪态和执行态
  - BLOCKED 线程被一个监听锁所阻塞时的状态
  - WAITING 无期限等待（即没有设定等待时间），
  - TIMED_WAITING 期限等待，
  - TERMINATED

### 死锁（重点）

- 当第一个线程拥有A对象锁标记，并等待B对象锁标记，同时第二个线程拥有B对象锁标记，并等待A对象锁标记时，产生死锁。
- 一个线程可以同时拥有多个对象的锁标记，当线程阻塞时，不会释放已经拥有的锁标记，由此可能造成死锁。

```java
/**
 * 死锁
 * 创建两个锁（两根筷子）
 */
public class MyLock {

    public static   Object a =new Object();
    public static  Object b =new Object();

}
```



```java
/**
 * 死锁测试：两个人，两个筷子
 */
public class TestDeadLock {
    public static void main(String[] args) {

        Runnable boy = new Runnable() {
            @Override
            public void run() {
                synchronized (MyLock.a){
                    System.out.println("boy拿到了a筷子");
                }
                synchronized (MyLock.b){
                    System.out.println("boy拿到了b筷子");
                    System.out.println("boy可以吃东西了");
                }
            }
        };

        Runnable girl = new Runnable() {
            @Override
            public void run() {
                synchronized (MyLock.b){
                    System.out.println("girl拿到了b筷子");
                }
                synchronized (MyLock.a){
                    System.out.println("girl拿到了a筷子");
                    System.out.println("girl可以吃东西了");
                }
            }
        };

        new Thread(boy,"boy").start();
        try {
            Thread.sleep(100);//线程休眠，让男孩先吃，
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        new Thread(girl,"girl").start();
    }
}
```

### 线程通信

- 等待：
  - `public final void wait()`
  - `public final void wait(long timeout)`
  - 必须在对obj加锁的同步代码块中调用。在一个线程中，调用`obj.wait()`时，此线程会释放其拥有的所有锁标记。同时此线程阻塞在obj的等待队列中。总而言之，就是释放锁，进入等待队列。
- 通知（唤醒线程）：
  - `public final void notify()`随机唤醒一个等待队列里的线程
  - `public final void notifyAll()`
  - 进入等待的线程需要其他线程调用该线程的通知方法来将其唤醒。

#### 通过存钱、取钱案例来使用

- 一存一取
- 实现存一笔，取一笔

```java
/**
 * 线程通信
 */
public class AddMoney2 implements Runnable{
    private BankCard2 card2;

    public AddMoney2(BankCard2 card2) {
        this.card2 = card2;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            card2.sale(1000);
        }
    }
}
```

```java
/**
 * 线程通信
 */
public class SubMoney2 implements Runnable{
    private BankCard2 card2;

    public SubMoney2(BankCard2 card2) {
        this.card2 = card2;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            card2.take(1000);
        }
    }
}
```



```java
/**
 * 线程通信
 * 实现：存一笔，取一笔
 *
 */
public class BankCard2 {
    //余额
    private double money;
    //标记
    private boolean flag = false;//flag 为 true ，表示可以取钱，为false 可以存钱
//存钱
    public synchronized void sale(double m){//锁this,即当前对象

        if (flag){//
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        money += m;
        System.out.println(Thread.currentThread().getName() + "存---了1000，余额为："+money);
        //修改标记
        flag = true;
        //唤醒取钱线程
        this.notify();
    }

    //取钱
    public synchronized void take(double m){
        if (!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        money -= m;
        System.out.println(Thread.currentThread().getName() + "取---了1000，余额为："+money);
        //修改标记
        flag = false;

        //唤醒存钱线程
        this.notify();
    }
}
```

```java
public class TestBankCard2 {
    public static void main(String[] args) {
        //创建银行卡
        BankCard2 bankCard2 = new BankCard2();

        AddMoney2 addMoney2 = new AddMoney2(bankCard2);
        SubMoney2 subMoney2 = new SubMoney2(bankCard2);

        new Thread(addMoney2,"哥哥").start();
        new Thread(subMoney2,"妹妹").start();


    }
}
```

#### 多存多取问题分析

- 谁能抢到 cpu 是不确定的，
- 一个线程在哪休眠，唤醒时就在哪继续执行
- 问题就在于：唤醒之后没有判断标记 flag ，
- 解决：把 if 改为 while ,余额就不会是负数了
-  但是死锁(有可能全部线程进入等待队列)的问题还没解决

#### 全部等待问题分析

- 即，有可能所有线程都进入等待队列，造成死锁
- 解决：把 notify() 换成 notifyAll() 

### 生产者消费者问题

- 若干个生产者在生产产品，这些产品将提供给若干个消费者去消费，为了使生产者和消费者能并发执行，在两者之间设置一个能存储多个产品的缓冲区，生产者将生产的产品放入缓冲区中，消费者从缓冲区取走产品进行消费，显然生产者和消费者之间必须保持**同步**，即不允许消费者到一个空的缓冲区中取产品，也不允许生产者向一个满的缓冲区中放入产品。

- 有五个类

  - Bread类
  - BreadContainer
  - Product
  - Consume
  - TestBread

  ```java
  /**
   * 生产者消费者
   * 产品类：面包类
   */
  public class Bread {
      private int id;
      private String productName;
  
      public Bread() {
      }
  
      public Bread(int id, String productName) {
          this.id = id;
          this.productName = productName;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getProductName() {
          return productName;
      }
  
      public void setProductName(String productName) {
          this.productName = productName;
      }
  
      @Override
      public String toString() {
          return "Bread{" +
                  "id=" + id +
                  ", productName='" + productName + '\'' +
                  '}';
      }
  }
  ```

  ```java
  /**
   * 生产者消费者
   * 容器类；
   *
   */
  public class BreadContainer {
      //存放面包的数组
      private Bread[] cons = new Bread[6];
      //存放面包的位置，脚标
      private int index = 0;
      //放入面包，生产者
      public synchronized void input(Bread bread){
          while (index >= cons.length){
              try {
                  this.wait();
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
          }
  
          cons[index] = bread;
          System.out.println(Thread.currentThread().getName()+"生产了 "+bread.getId()+"");
          index++;
          this.notifyAll();
  
      }
      //取出面包，消费者
      public synchronized void output(Bread bread){
          while (index <= 0){
              try {
                  this.wait();
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
          }
  
          index--;//因为index 代表面包数量，需要的是数组下标
          System.out.println(Thread.currentThread().getName()+"消费了 "+bread.getId()+"");
          cons[index] = null;
  
          this.notifyAll();
  
      }
  
  }
  ```

  

```java
/**
 * 生产者
 */
public class Product implements Runnable
{
        private BreadContainer con;
        public  Product(BreadContainer breadContainer){
            super();
            this.con = breadContainer;
        }

        @Override
        public void run() {
            for (int i = 0; i < 20; i++) {
                con.input(new Bread(i,Thread.currentThread().getName()));
            }

        }

}

```

```java
/**
 * 消费者
 */
public class Consume implements Runnable{
    private BreadContainer con;
    public  Consume(BreadContainer breadContainer){
        super();
        this.con = breadContainer;
    }

    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            con.output(new Bread(i,Thread.currentThread().getName()));
        }

    }
}

```

```java
/**
 * 生产者消费者
 * 测试列
 *
 */
public class TestBread{
    public static void main(String[] args) {
        BreadContainer breadContainer = new BreadContainer();

        Product product = new Product(breadContainer);
        Consume consume = new Consume(breadContainer);

        new Thread(product,"brother").start();
        new Thread(consume,"sister").start();
        new Thread(product,"dad").start();
        new Thread(consume,"mom").start();


    }
}
```

# 线程池

多线程的使用会出现的**问题**：

1. 线程是宝贵的内存资源、单个线程约占1MB空间，过多分配易造成内存溢出。
2. 频繁的创建及销毁线程会增加虚拟机回收频率、资源开销，造成性能下降。

基于以上就出现了**线程池**：

- 线程容器，可设定线程分配的数量。
- 将预先创建的线程对象存入池中，并重用线程池中的线程对象。
- 避免频繁的创建和销毁。

线程池的**原理**：

- 将任务提交给线程池，由线程池分配线程、运行任务，并在当前任务结束后复用线程

## 创建线程池

- 线程池所在包：java.util.concurrent

- 常用的线程池接口和类：

  - Executor: 线程池的顶级接口
  - ExecutorService: 线程池接口，可通过 submit (Runnable task)提交任务代码
    - 常用的两个类：
      - ThreadPoolExecutor
      - ScheduledThreadPoolExecutor
  - Executors: (工厂类)，创建线程池的工具类，通过此类可以获得一个线程池
    - 创建固定线程个数的线程池。
      - Executors.newFixedThreadPool(4)
    - 创建缓存线程池，由任务的多少决定。
      - Executors.newCachedThreadPool()
    - 创建单线程池。
      - Executors.newSingleThreadExecutor()
    - 创建调度线程池。调度：周期/定时执行。
      - Executors.newScheduledThreadPool()
    - newFixedThreadPool(int nThreads) 获得**固定数量**的线程池。参数：指定线程池中线程的数量
    - newCachedThreadPool() 获得**动态数量**的线程池，如果不够则创建新的，没有上限

  

  ```java
  /**
   * 线程池的创建
   *
   * @author joker_chen
   */
  public class Demo01 {
      public static void main(String[] args) {
          //1.1 创建固定大小的线程池
  //        ExecutorService executorService = Executors.newFixedThreadPool(4);
          //1，2 创建缓存线程池，线程个数，由任务个数决定
          ExecutorService executorService = Executors.newCachedThreadPool();
          //1.3 创建单线程池
  //        Executors.newSingleThreadExecutor();
          //1.4 创建调度线程池
  //        Executors.newScheduledThreadPool();
  
  
          //创建任务
          Runnable runnable = new Runnable() {
              private int ticket = 100;
              @Override
              public void run() {
                  while (true){
                      if (ticket <= 0 ){
                          break;
                      }
                      System.out.println(Thread.currentThread().getName() + "卖出 了"+ticket+"票");
                      ticket--;
                  }
              }
          };
          //提交
          for (int i = 0; i < 5; i++) {//因为线程池里有4个线程
              executorService.submit(runnable);
          }
  
          //关闭线程池
          executorService.shutdown();//等待所有任务执行完毕后，然后关闭线程池，不接受新任务；推荐使用
  //        executorService.shutdownNow();//会试图停止所有正在执行的任务，暂停处理正在等待的任务，并返回等待执行的任务列表
      }
  }
  
  ```


## Callable 接口

```
public interface Callable<V>{
    public V call() throws Exception;
}
```

- JDK1.5加入，与Runnable接口类似，实现之后代表一个线程任务。
- Callable具有泛型返回值、可以声明异常。

Callable 与  Runnable 接口的区别：

- Callable,表示可调用的，Runnable 表示执行
- Callable接口中call方法有返回值，Runnable接口中run方法没有返回值。
- Callable接口中call方法有声明异常，Runnable接口中run方法没有异常。

```java
/**
 * Collable 接口的使用
 *
 *功能需求：求1-100的和
 * @author joker_chen
 */
public class Demo02 {
    public static void main(String[] args) throws Exception {
//创建 Callable 对象
        Callable<Integer> callable = new Callable<Integer>() {
            @Override
            public Integer call() throws Exception {
                System.out.println(Thread.currentThread().getName() + "开始计算");
                int sum = 0;
                for (int i = 0; i < 100; i++) {
                    sum +=i;
                    Thread.sleep(100);
                }
                return sum;
            }
        };
//把 Callable 对象转换成可执行任务，
        //因为Thread里没有 Callable 这个类型的参数
        FutureTask<Integer> integerFutureTask = new FutureTask<>(callable);
//创建线程
        Thread thread = new Thread(integerFutureTask);

        thread.start();
//获取结果get()
        Integer sum = integerFutureTask.get();//等待call 执行完毕，才会返回结果
        System.out.println("计算结果" + sum);
    }
}
```

## Callable 与线程池结合

```java
/**
 * 线程池与 Callable 结合使用
 * 计算1-100的和
 */
public class Demo03 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(1);
        //提交任务；Future:表示将要执行完任务的结果
        Future<Integer> future =  executorService.submit(new Callable<Integer>() {//submit() 返回值是Future 类型的
            @Override
            public Integer call() throws Exception {
                System.out.println(Thread.currentThread().getName() +"开始计算");
                int sum = 0;
                for (int i = 0; i < 100; i++) {
                    sum +=i;
                }
                return sum;
            }
        });

        System.out.println(future.get());
        executorService.shutdown();
    }
}
```

## Future 接口

- Future：表示将要完成任务的结果

- **表示**`ExecutorService.submit()`**所返回的状态结果**，**就是call的返回值**。

- **方法**:`V get()`**以阻塞形式等待Future中的异步处理结果**（**call的返回值**）。

- 案例：使用两个线程，并发计算1-50，51-100的和，再进行汇总统计

- ```java
  /**
   * Future 接口
   * 案例：使用两个线程，并发计算1-50，51-100的和，再进行汇总统计
   *
   * @author joker_chen
   */
  public class Demo04 {
      public static void main(String[] args) throws ExecutionException, InterruptedException {
          ExecutorService pool =  Executors.newFixedThreadPool(2);
          
         Future<Integer> future = pool.submit(new Callable<Integer>() {
              @Override
              public Integer call() throws Exception {
  
                  int sum = 0;
                  for (int i = 0; i <= 50; i++) {
                      sum +=i;
                  }
                  System.out.println("1-50计算完毕");
  
                  return sum;
              }
          });
  
          Future<Integer> future1 = pool.submit(new Callable<Integer>() {
              @Override
              public Integer call() throws Exception {
  
                  int sum = 0;
                  for (int i = 51; i <= 100; i++) {
                      sum +=i;
                  }
                  System.out.println("51-100计算完毕");
  
                  return sum;
              }
          });
  
          int sum = future.get() + future1.get();
          System.out.println(sum);
  
          pool.shutdown();
      }
  }
  ```

## 线程的同步异步

**同步**：

- 形容一次方法调用，同步一旦开始，调用者必须等待该方法返回，才能继续
- ![同步](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-13_02-00-55.png)
- 可理解为，主只要有等待，就是同步
- 注意：单条执行路径

**异步**：

- 形容一次方法调用，异步一旦开始，像是一次消息传递，调用者告知后立刻返回。二者竞争时间片，并发执行
- 注意：多条执行路径
- ![异步](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-12_17-40-47.png)

# Lock 接口

- JDK5 后加入，与 synchronized 比较，显示定义，结构更灵活
- 所在包: java.util.concurrent.locks
- 提供更多实用性方法，功能更强大、性能更优越
- 常用方法：
  - void lock() 获取锁，如果锁被占用，则等待
  - boolean tryLock() //尝试获取锁（成功返回true ,失败返回false ,不阻塞）
  - void unlock() //释放锁

## ReentrantLock类 （重入锁）

- ReentrantLock :Lock接口的**实现类**，与synchronized一样具有**互斥锁**功能。但功能更强大
- 重入锁：即一个线程拿到该锁后**，**还可以再次成功获取；而不会因为该锁已经被持有（尽管是自己所持有）而陷入等待状态（死锁）。

### 使用

#### **案例一**：多线程对同一数组存入字符串

```java
/**
 * ReentrantLock 的使用 重入锁
 *
 */
public class MyList {
    private Lock lock = new ReentrantLock();
    private String[] strings = {"I","love","","",""};
    private int count = 2;
    public  void add(String value){
        lock.lock();
        try {
            strings[count] = value;
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
        count++;
        System.out.println(Thread.currentThread().getName() + "添加了" + value );
    }

    public String[] getStr(){
        return strings;
    }

}
```

```java
public class TestMyList {
    public static void main(String[] args) throws InterruptedException {
        MyList myList = new MyList();
        ReentrantLock reentrantLock = new ReentrantLock();

        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                myList.add("java");

            }
        };

        Runnable runnable1 = new Runnable() {
            @Override
            public void run() {
                myList.add("python");

            }
        };

        Thread thread1 = new Thread(runnable);
        Thread thread2 = new Thread(runnable1);

        thread1.start();
        thread2.start();

        thread1.join();//保证线程执行完，才会执行主线程
        thread2.join();

        System.out.println(Arrays.toString(myList.getStr()));
    }
}

```



#### **案例二**：用四个线程卖票

```java
/**
 * ReentrantLock 的使用
 */
public class Ticket  implements Runnable{
    private int ticket  = 100;
    private Lock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true){
            lock.lock();
            try {
                if (ticket <= 0){
                    break;
                }
                System.out.println(Thread.currentThread().getName() + "卖了第" + ticket +"票");
                ticket--;
            }finally {
                lock.unlock();
            }
        }
    }
}
```

```java
/**
 * ReentrantLock
 * 用四个线程卖票，
 */
public class TestTicket {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        ExecutorService executorService = Executors.newFixedThreadPool(4);

        for (int i = 0; i < 4; i++) {
            executorService.submit(ticket);

        }
        executorService.shutdown();
    }
}
```

## ReentrantReadWriteLock（读写锁）

- 一种支持一写多读的同步锁，读写分离，可本别分配读锁、写锁
- 支持多次分配读锁，使多个读操作可以并发执行
- 读写锁的**互斥规则**：
  - 写-写：互斥，阻塞
  - 读-写：互斥，读阻塞写、写阻塞读
  - 读-读：不互斥、不阻塞
  - 在读操作远远高于写操作的环境中，可在保障线程安全的情况下，提高运行效率

### 使用

```java
/**
 * ReentrantReadWriteLock 读写锁 的使用
 *
 * @author joker_chen
 */
public class ReadWriteLock {
    private ReentrantReadWriteLock rrl = new ReentrantReadWriteLock();
    private ReentrantReadWriteLock.ReadLock readLock = rrl.readLock();
    private ReentrantReadWriteLock.WriteLock writeLock = rrl.writeLock();
//    private ReentrantLock lock = new ReentrantLock();//互斥锁,如果不用读写锁，使用互斥锁，就是都互斥，运行时间明显增加
    private String value = "java";

    //读取
    public String getValue(){
        readLock.lock();
        try{
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "读取了"+this.value);
            return this.value;
        }finally {
            readLock.unlock();
        }
    }
    //写入
    public void setValue (String value){
        writeLock.lock();
        try{
            try{
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "写入了" + value);
            this.value = value;
        }finally {
            writeLock.unlock();
        }
    }
}
```

```java
/**
 * ReentrantReadWriteLock
 *
 */
public class TestReadWriteLock {
    public static void main(String[] args) {
        ReadWriteLock readWriteLock = new ReadWriteLock();

        ExecutorService executorService = Executors.newFixedThreadPool(20);

        Runnable read = new Runnable() {
            @Override
            public void run() {
                readWriteLock.getValue();
            }
        };
        Runnable write = new Runnable() {
            @Override
            public void run() {
                readWriteLock.setValue("joker"+new Random().nextInt(100));
            }
        };
        long startTime = System.currentTimeMillis();
        //分配18个读取任务
        for (int i = 0; i < 18; i++) {
            executorService.submit(read);
        }
        //分配两个写入任务
        for (int i = 0; i < 2; i++) {
            executorService.submit(write);
        }

        executorService.shutdown();
        while (!executorService.isTerminated()){//isTerminated() 返回值是true 则代表所有任务已经结束
            //空转
        }
        long endTime = System.currentTimeMillis();

        System.out.println("运行时间" + (endTime - startTime));

    }
}
```

# 常见的线程安全的集合

线程不安全的时候，常出现的异常:ConcurrentModificationException 并发修改异常

Collection:

![Collection](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-12_17-47-49.png)

线程安全的：

- CopyOnWriteArraySet
- [Vector](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html) 已经不咋用了 
- [CopyOnWriteArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html)
-  [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)
- [ConcurrentLinkedQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentLinkedQueue.html)
- [BlockingQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)<E>
- [ArrayBlockingQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ArrayBlockingQueue.html)
- [LinkedBlockingQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/LinkedBlockingQueue.html)
- 

**Map**:

![Map](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-13_02-00-59.png)

线程安全的：

- [ConcurrentSkipListMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListMap.html)
- [Hashtable](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html) 不咋用了
- [Properties](https://docs.oracle.com/javase/8/docs/api/java/util/Properties.html)
- [ConcurrentMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html)<K,V>
- [ConcurrentHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html)

Collections 工具类中提供了多个可以获得线程安全集合的方法：

- 在 JDK1.5 之前就是使用这些
- `public static <T> Collection<T> synchronizedCollection(Collection<T> c)`
- `public static <T> List<T> synchronizedList(List<T> list)`
- `public static <T> Set<T> synchronizedSet(Set<T> s)`
- `public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)`
- `public static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)`
- `public static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V>)`

以上为JDK1.2提供，接口单一、维护性高，但性能没有提升，均以synchronized实现。现在已经很少使用，都用并发包里的。

## 问题演示

```java
/**
 * 使用多线程操作线程不安全集合
 * @author joker_chen
 */
public class Demo01 {
    public static void main(String[] args) {
//        //1 创建不安全的集合，ArrayList
//        ArrayList<String> arrayList = new ArrayList<>();//直接使用这个，就会出现并发问题
//        //1.1 使用Collections 中的线程安全方法 转成线程安全的集合
//        List<String> synchronizedList = Collections.synchronizedList(arrayList);
//        //1.2 使用 CopyOnWriteArrayList
        CopyOnWriteArrayList<String> strings = new CopyOnWriteArrayList<>();


        for (int i = 0; i < 20; i++) {
            int temp = i;
            new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int j = 0; j < 10; j++) {
                        strings.add(Thread.currentThread().getName() + "====="+temp +"=====" +j);
                        System.out.println(strings.toString());
                    }

                }
            }).start();
        }

    }
}	
```

## CopyOnWriteArrayList

- 线程安全的 ArrayList ,加强版的读写分离
- 写有锁，读无锁，读写之间不阻塞，优于读写锁（ReentrantReadWriteLock）
- 写入时，先 copy 一个容器副本、再添加新元素，最后替换引用；因此这是一个牺牲空间换取安全的
- 使用方式与 ArrayList 无异

### 使用示例

```java
/**
 * 使用多线程操作 CopyOnWriteArrayList
 *
 */
public class Demo02 {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> arrayList = new CopyOnWriteArrayList<>();
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        //创建了5个线程的线程池，每个线程池都分配了5个任务，每个任务都是向集合添加10个元素
        for (int i = 0; i < 5; i++) {
            executorService.submit(new Runnable() {
                @Override
                public void run() {
                    for (int j = 0; j < 10; j++) {
                        arrayList.add(Thread.currentThread().getName() + "---" + new Random().nextInt(1000));

                    }
                }
            });
        }

        executorService.shutdown();
        while (!executorService.isTerminated()){

        }
        System.out.println("元素个数：" + arrayList.size());
        for (String string :
                arrayList) {
            System.out.println(string);
        }
    }
}
```

### 源码分析

- `final transient ReentrantLock lock = new ReentrantLock();`

  此集合所使用的的锁 lock 是重入锁 ReentrantLock。

- `private transient volatile Object[] array;`

  此集合实际存储的数组array。

- add() 方法，加了锁

- remove(index) 加了锁

- get() 读取操作，没有加锁

- 有关修改数组的操作都上了锁，即写操作有锁

- 有关读操作都是直接返回，无锁，即，读操作无锁，写的同时可以读

## CopyOnWriteArraySet

- 线程安全的 Set ,底层使用 CopyOnWriteArrayList 实现，即是有序的
- 唯一不同在于，使用 addIfAbsent() 添加元素，会遍历数组
- 如果存在元素，则不添加（扔掉副本）。

### 使用

```java
/**
 * 演示 CopyOnWriteArraySet
 *
 */
public class Demo03 {
    public static void main(String[] args) {
        //创建集合
        CopyOnWriteArraySet<String> arraySet = new CopyOnWriteArraySet<>();
        //添加元素
        arraySet.add("java");
        arraySet.add("python");
        arraySet.add("redis");
        arraySet.add("kotlin");
        arraySet.add("java");//不可以添加重复元素
        //打印
        System.out.println("元素个数"+arraySet.size());
        System.out.println(arraySet.toString());
    }
}
```



### 源码分析

- 底层由 CopyOnWriteArrayList 实现，即是有序的
- add() 
- addIfAbsent()
- indexOf()
- 判断元素是否重复的依据 equals() 
- 同样是读操作无锁，写操作上锁

## Queue 接口（队列）

- Collection 的子接口，表示队列 FIFO（first in first out）先进先出

- 常用方法：

  - 抛出异常：

  - - `boolean add(E e)`

      顺序添加一个元素（到达上限后，再添加则会抛出异常）。

    - `E remove()`

      获得第一个元素并移除（如果队列没有元素时，则抛出异常）。

    - `E element()`

      获得第一个元素但不移除（如果队列没有元素时，则抛异常）。

  - 返回特殊值：（**建议使用**）

    - `boolean offer(E e)`

      顺序添加一个元素（到达上限后，再添加则会返回false）。

    - `E poll()`

      获得第一个元素并移除（如果队列没有元素时，则返回null）。

    - `E peek()`

      获得第一个元素但不移除（如果队列没有元素时，则返回null）。

使用示例：

```java
/**
 * Quene 接口的使用
 *
 * @author joker_chen
 */
public class Demo04 {
    public static void main(String[] args) {
        //创建队列，LinkedList() 不安全
        Queue<String> queue = new LinkedList<>();
        //2 入队
        queue.offer("apple");
        queue.offer("durian");
        queue.offer("orange");
        queue.offer("watermelon");
        queue.offer("grape");
        //3 出队
        System.out.println(queue.peek());//不会删除
        System.out.println("===========");
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            System.out.println(queue.poll());
        }
        System.out.println("出队完，队列元素个数"+queue.size());

    }
}
```

### ConcurrentLinkedQueue

- 线程安全、可高效读写的队列，高并发下**性能最好**的队列
- 无锁、CAS （compare and switch）比较交换算法，修改的方法包含三个核心参数（V,E,N)
  - V 要更新的变量
  - E 预期值
  - N 新值
- 只有当 V==E 时，V = N；否则表示已被更新过，则取消当前操作

#### 使用

```java
/**
 * ConcurrentLinkedQueue 的使用
 */
public class Demo05 {
    public static void main(String[] args) throws InterruptedException {
        ConcurrentLinkedQueue<Integer> queue = new ConcurrentLinkedQueue<>();
        //入队
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 1; i <= 5; i++) {
                    queue.offer(i);
                }
            }
        });
        
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 6; i <= 10; i++) {
                    queue.offer(i); 
                }
            }
        });
        
        thread.start();
        thread1.start();
        
        thread.join();
        thread1.join();

        System.out.println("==========出队");
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            System.out.println(queue.poll());
        }
        System.out.println("出队完之后的队列元素个数：" + queue.size());


    }
}
```

### BlockingQueue 接口（阻塞队列）

- 阻塞队列又称有界队列 
- Queue 的子接口，阻塞的队列，增加了两个线程状态为无期限等待的方法
- 两个新增的无期限等待的方法：
  - void put(E e) //将指定元素插入此队列中，如果没有可用空间，则等待
  - E take() //获取并移除此队列 头部元素，如果没有可用元素，则等待
- 可用于解决 生产者、消费者问题



#### 实现类

##### ArrayBlockingQueue

- 数组结构实现，有界队列。手工固定队列大小上限

##### LinkedBlockingQueue

- 链表结构实现，有界队列。默认上限Integer.MAX_VALUE ;也可以手动添加上限

#### BlockingQueue 的使用

##### 案例1 ：创建一个有界队列，添加数据

```java
/**
 * BlockingQueue 阻塞队列的使用
 *
 */
public class Demo06 {
    public static void main(String[] args) throws  Exception{
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(5);
        //添加元素
        queue.put("aaa");
        queue.put("bbb");
        queue.put("ccc");
        queue.put("ddd");
        queue.put("eee");
        System.out.println(
                "已经添加了5个元素"
        );
        //删除队头元素
        queue.take();
        queue.put("fff");
        System.out.println("已经添加了第6个元素");
        System.out.println(queue.toString());
    }
}

```

##### 案例2  ： 使用阻塞队列实现生产者和消费者

```java
/**
 * 阻塞队列
 * 案例：使用阻塞队列实现生产者和消费者
 */
public class Demo07 {
    public static void main(String[] args) {

        ArrayBlockingQueue<Integer> queue = new ArrayBlockingQueue<>(6);

        Thread produce = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    try {
                        queue.put(i);//一次最多只能放入6个面包
                        //打印并没有同步，所以，打印可能会出现问题
                        System.out.println(Thread.currentThread().getName() + "生产了" + i + "号面包");//这个并没有同步，所以，打印可能会出现问题
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        },"monster");

        Thread consume = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    try {
                        Integer num = queue.take();//一次最多只能放入6个面包
                        //打印并没有同步，所以，打印可能会出现问题
                        System.out.println(Thread.currentThread().getName() + "消费了" + i + "号面包");//这个并没有同步，所以，打印可能会出现问题
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        },"joker");

        produce.start();
        consume.start();
    }
}
```

## ConcurrentHashMap 

- 线程安全的 HashMap 集合，
- 初始容量默认为16段（segment）,使用**分段锁设计**
- 每个段代表一个哈希表
- 不对整个 Map 加锁，而是为每个 Segment 加锁；对一个Segment的操作不影响其他Segment
- 当多个对象存入同一个 Segment 时，才需要互斥
- 最理想状态为 16个对象分别存入16个 Segment ，并行数量为16；此时就不需要加锁，提高了运行效率
- 使用方式与 HashMap 无异
- 在jdk1.7之前是使用分段锁的设计方式，jdk1.8之后就是采用 一种无锁 算法（CAS）

### 使用

```java
/**
 * ConcurrentHashMap 的使用
 *
 */
public class Demo08 {
    public static void main(String[] args) {
        ConcurrentHashMap<String, String> hashMap = new ConcurrentHashMap<>();
        //使用多线程添加数据
        for (int i = 0; i < 5; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int j = 0; j < 10; j++) {
                        hashMap.put(Thread.currentThread().getName() + "--" + j,j + "");
                        System.out.println(hashMap);
                    }
                }
            }).start();
        }
    }
}
```



