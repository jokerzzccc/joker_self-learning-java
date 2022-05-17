# 目录

[toc]



# Day35-Java 相关知识补充

时间：2022年4月27日



# 1、VO，PO，TO等实体类之间的转换

## 1.1 用 xxx.copyProperties()

- 参考博客：https://www.cnblogs.com/xinglongbing521/p/10145440.html
- 官方接口：https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/BeanUtils.html

- 最好用 org.springframework.beans的，避免用 Apache 的
- 

## springframe 与 apache 的接口对比

1. 无论是org.springframework.beans或者org.apache.commons.beanutils，与get/set方式相比，都存在性能问题。

2. 效率由高到底：get/set 》PropertyUtils 》BeanUtils。

3. PropertyUtils和BeanUtils两个工具类都是对bean之间存在属性名相同的属性进行处理，无论是源bean或者是目标bean中多出来的属性均不处理。

4. 具体来说：

   BeanUtils.copyProperties()可以在一定范围内进行类型转换，同时还要注意一些不能转换时候，会将默认null值转化成0;

   Property.copyProperties()则是严格的类型转化，必须类型和属性名完全一致才转化。

   对于null的处理：PropertyUtils支持为null的场景；BeanUtils对部分属性不支持null，具体如下：

   a. java.util.Date类型不支持,但是它的子类java.sql.Date是被支持的。java.util.Date直接copy会报异常；

   b. Boolean，Integer，Long等不支持，会将null转化为0；

   c. String支持，转化后依然为null。

5. BeanUtils的高级功能org.apache.commons.beanutils.Converter接口可以自定义类型转化，也可以对部分类型数据的null值进行特殊处理，如ConvertUtils.register(new DateConverter(null), java.util.Date.class);但是PropertyUtils没有。



另外：值得注意的是，在测试过程中发现，commons-beanutils-1.8.0.jar版本中的BeanUtils类，支持Byte到Integer或int的转化。说明实际使用过程中，我们还是要多看源码，多做测试，并且注意版本号升级带来的微小变化。



两者接口的区别：

1、package org.springframework.beans;中的BeanUtils.copyProperties(A,B);是A中的值付给B

2、package org.apache.commons.beanutils;中的BeanUtils.copyProperties(A,B);是B中的值付给A



## Springframework 的 copyProperties() 

官方接口介绍：

+ #### copyProperties

  ```java
  public static void copyProperties(Object source,
                                    Object target)
                             throws BeansException
  ```

  Copy the property values of the given source bean into the target bean.

  Note: The source and target classes do not have to match or even be derived from each other, as long as the properties match. Any bean properties that the source bean exposes but the target bean does not will silently be ignored.

  This is just a convenience method. For more complex transfer needs, consider using a full [`BeanWrapper`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/BeanWrapper.html).

  As of Spring Framework 5.3, this method honors generic type information when matching properties in the source and target objects.

  The following table provides a non-exhaustive set of examples of source and target property types that can be copied as well as source and target property types that cannot be copied.

  | source property type | target property type     | copy supported |
  | -------------------- | ------------------------ | -------------- |
  | `Integer`            | `Integer`                | yes            |
  | `Integer`            | `Number`                 | yes            |
  | `List<Integer>`      | `List<Integer>`          | yes            |
  | `List<?>`            | `List<?>`                | yes            |
  | `List<Integer>`      | `List<?>`                | yes            |
  | `List<Integer>`      | `List<? extends Number>` | yes            |
  | `String`             | `Integer`                | no             |
  | `Number`             | `Integer`                | no             |
  | `List<Integer>`      | `List<Long>`             | no             |
  | `List<Integer>`      | `List<Number>`           | no             |

  + **Parameters:**

    `source` - the source bean

    `target` - the target bean

  + **Throws:**

    `BeansException` - if the copying failed

  + **See Also:**

    [`BeanWrapper`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/BeanWrapper.html)



总结大概如下：

1、属性名相同，类型相同 可以被复制

2、基本类型 与 其对应的封装类型 可以被复制

3、封装类型 与 其对应的基本类型 可以被复制

4、其他统统不行

例如：

         Integer->Long 
         int->long  
         Date->String等

5、source会覆盖掉target中原有的值

6、如果希望哪个属性不被复制，使用重载方法

public static void copyProperties(Object source, Object target, String... ignoreProperties) throws BeansException
ignoreProperties传属性名称。

7、source 与 target 都是不能为null的，会报错。

8、复制实现是靠set、get，所以实体中的字段要有这两个方法，没有是不会被复制赋值的。



## ==注意事项：==

1. 实体类需要**无参构造**：因为反射构建实例的缘故

2. 实体类需要 **getter/setter**

3. 属性的命名上必须统一，所以系统中各属性的命名规则必须严格规范。

4. 不好除错，例如属性中有Enum型态，但资料库为字串等型态不一致，或是Entity类与DTO类命名不一致等也会造成属性结果为null的情况。

5. 

   

   


# 2、AtomicInteger 的用法

- 参考博客：
  - https://www.jianshu.com/p/509aca840f6d

## 2.1 使用

- JDK1.5之后的java.util.concurrent.atomic包里，多了一批**原子处理类**。AtomicBoolean、AtomicInteger、AtomicLong、AtomicReference。

- 主要用于在**高并发环境下**的高效程序处理,来帮助我们**简化同步处理.**



AtomicInteger

- 一个提供原子操作的Integer的类。

- 在Java语言中，++i和i++操作并不是线程安全的，在使用的时候，不可避免的会用到synchronized关键字。

- 而AtomicInteger则通过一种线程安全的加减操作接口。





我们先来看看AtomicInteger给我们提供了什么接口:

> public final int get() //获取当前的值
>  public final int getAndSet(int newValue)//获取当前的值，并设置新的值
>  public final int getAndIncrement()//获取当前的值，并自增
>  public final int getAndDecrement() //获取当前的值，并自减
>  public final int getAndAdd(int delta)  //获取当前的值，并加上预期的值

下面通过两个简单的例子来看一下 AtomicInteger 的优势在哪:

**普通线程同步:**



```java
class Test2 {
        private volatile int count = 0;

        public synchronized void increment() {
                  count++; //若要线程安全执行执行count++，需要加锁
        }

        public int getCount() {
                  return count;
        }
}
```

**使用AtomicInteger:**



```csharp
class Test2 {
        private AtomicInteger count = new AtomicInteger();

        public void increment() {
                  count.incrementAndGet();
        }
   //使用AtomicInteger之后，不需要加锁，也可以实现线程安全。
       public int getCount() {
                return count.get();
        }
}
```

从上面的例子中我们可以看出：使用AtomicInteger是非常的安全的.而且因为AtomicInteger由硬件提供原子操作指令实现的。在非激烈竞争的情况下，开销更小，速度更快。



## 2.2 原理

我们来看看AtomicInteger是如何使用非阻塞算法来实现并发控制的:
 AtomicInteger的关键域只有一下3个：

```cpp
// setup to use Unsafe.compareAndSwapInt for updates
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;
static {    
         try {        
                valueOffset = unsafe.objectFieldOffset (AtomicInteger.class.getDeclaredField("value"));   
        } catch (Exception ex) { 
               throw new Error(ex); 
        }
    }
private volatile int value;
```

这里， 

- unsafe是java提供的获得对对象内存地址访问的类，注释已经清楚的写出了，它的作用就是在更新操作时提供“比较并替换”的作用。实际上就是AtomicInteger中的一个工具。
-  valueOffset是用来记录value本身在内存的便宜地址的，这个记录，也主要是为了在更新操作在内存中找到value的位置，方便比较。
  -  注意：value是用来存储整数的时间变量，这里被声明为volatile，就是为了保证在更新操作时，当前线程可以拿到value最新的值（并发环境下，value可能已经被其他线程更新了）。






这里，我们以自增的代码为例，可以看到这个并发控制的核心算法：

```kotlin
/**
*Atomicallyincrementsbyonethecurrentvalue.
*
*@returntheupdatedvalue
*/
publicfinalintincrementAndGet(){
for(;;){
//这里可以拿到value的最新值
intcurrent=get();
intnext=current+1;
if(compareAndSet(current,next))
returnnext;
}
}

publicfinalbooleancompareAndSet(intexpect,intupdate){
//使用unsafe的native方法，实现高效的硬件级别CAS
        returnunsafe.compareAndSwapInt(this,valueOffset,expect,update);
}
```



## **优点总结:**

 最大的好处就是可以**避免多线程的优先级倒置和死锁情况的发生**，提升在高并发处理下的性能。





# 3、后端接收以及发送 LocalDateTime

- 参考博客：https://www.liaoxuefeng.com/wiki/1252599548343744/1303985694703650

**后端接收**：需要使用注解：`@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")`

示例：

```java 
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
 private LocalDateTime birthday;

```



**传给前端**，一般使用注解：`@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")`



# 4、在数据库中存储日期和时间

除了旧式的`java.util.Date`，我们还可以找到另一个`java.sql.Date`，它继承自`java.util.Date`，但会自动忽略所有时间相关信息。这个奇葩的设计原因要追溯到数据库的日期与时间类型。

在数据库中，也存在几种日期和时间类型：

+ `DATETIME`：表示日期和时间；
+ `DATE`：仅表示日期；
+ `TIME`：仅表示时间；
+ `TIMESTAMP`：和`DATETIME`类似，但是数据库会在创建或者更新记录的时候同时修改`TIMESTAMP`。

在使用Java程序操作数据库时，我们需要把数据库类型与Java类型映射起来。下表是数据库类型与Java新旧API的映射关系：

| 数据库    | 对应Java类（旧）   | 对应Java类（新） |
| :-------- | :----------------- | :--------------- |
| DATETIME  | java.util.Date     | LocalDateTime    |
| DATE      | java.sql.Date      | LocalDate        |
| TIME      | java.sql.Time      | LocalTime        |
| TIMESTAMP | java.sql.Timestamp | LocalDateTime    |

实际上，在数据库中，我们需要存储的最常用的是时刻（`Instant`），因为有了时刻信息，就可以根据用户自己选择的时区，显示出正确的本地时间。所以，最好的方法是直接用长整数`long`表示，在数据库中存储为`BIGINT`类型。

通过存储一个`long`型时间戳，我们可以编写一个`timestampToString()`的方法，非常简单地为不同用户以不同的偏好来显示不同的本地时间：

`import java.time.*; import java.time.format.*; import java.util.Locale; ` Run

对上述方法进行调用，结果如下：

```
2019年11月20日 上午8:15
Nov 19, 2019, 7:15 PM
```

### 小结

- 处理日期和时间时，尽量使用新的`java.time`包；

- 在数据库中存储时间戳时，尽量使用`long`型时间戳，它具有省空间，效率高，不依赖数据库的优点。





# THE END