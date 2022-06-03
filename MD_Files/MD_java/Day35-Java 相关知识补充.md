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





# 5、Optional 的用法

- 参考博客：
  - Optional java 8 API:https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html
  - Optional 使用博客：
    - https://juejin.cn/post/6844903945714794503
    - https://juejin.cn/post/6844903960050925581



## 5.1 Optional 的基本信息

### 5.1.1 为什么引入 Optional

- 来源：始于 java 8
- 作用：优雅的解决 NPE （`NullPointerException`），空指针异常

## 5.2 Optional 接口

Optional 的全部方法：

![image-20220520220624127](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220520220624127.png)

| Modifier and Type        | Method and Description                                       |
| :----------------------- | :----------------------------------------------------------- |
| `static <T> Optional<T>` | `empty()`Returns an empty `Optional` instance.               |
| `boolean`                | `equals(Object obj)`Indicates whether some other object is "equal to" this Optional. |
| `Optional<T>`            | `filter(Predicate<? super T> predicate)`If a value is present, and the value matches the given predicate, return an `Optional` describing the value, otherwise return an empty `Optional`. |
| `<U> Optional<U>`        | `flatMap(Function<? super T,Optional<U>> mapper)`If a value is present, apply the provided `Optional`-bearing mapping function to it, return that result, otherwise return an empty `Optional`. |
| `T`                      | `get()`If a value is present in this `Optional`, returns the value, otherwise throws `NoSuchElementException`. |
| `int`                    | `hashCode()`Returns the hash code value of the present value, if any, or 0 (zero) if no value is present. |
| `void`                   | `ifPresent(Consumer<? super T> consumer)`If a value is present, invoke the specified consumer with the value, otherwise do nothing. |
| `boolean`                | `isPresent()`Return `true` if there is a value present, otherwise `false`. |
| `<U> Optional<U>`        | `map(Function<? super T,? extends U> mapper)`If a value is present, apply the provided mapping function to it, and if the result is non-null, return an `Optional` describing the result. |
| `static <T> Optional<T>` | `of(T value)`Returns an `Optional` with the specified present non-null value. |
| `static <T> Optional<T>` | `ofNullable(T value)`Returns an `Optional` describing the specified value, if non-null, otherwise returns an empty `Optional`. |
| `T`                      | `orElse(T other)`Return the value if present, otherwise return `other`. |
| `T`                      | `orElseGet(Supplier<? extends T> other)`Return the value if present, otherwise invoke `other` and return the result of that invocation. |
| `<X extends Throwable>T` | `orElseThrow(Supplier<? extends X> exceptionSupplier)`Return the contained value, if present, otherwise throw an exception to be created by the provided supplier. |
| `String`                 | `toString()`Returns a non-empty string representation of this Optional suitable for debugging. |

### 5.2.1 三种静态方法构建 Optional 对象

#### 1、Optional.of(T value)

```java
public static <T> Optional<T> of(T value) {
    return new Optional<>(value);
}
```

- 该方法通过一个**非 `null`** 的 *value* 来构造一个 `Optional`，返回的 `Optional` 包含了 *value* 这个值。

- 对于该方法，传入的参数一定不能为 `null`，否则便会抛出 `NullPointerException`。

  

#### 2、Optional.ofNullble(T value)

```java
public static <T> Optional<T> ofNullable(T value) {
    return value == null ? empty() : of(value);
}
```

- 该方法和 `of` 方法的区别在于，传入的参数**可以为 `null`**
- 该方法会判断传入的参数是否为 `null`，如果为 `null` 的话，返回的就是 `Optional.empty()`。

#### 3、Optional.empty()

```java
public static<T> Optional<T> empty() {
    @SuppressWarnings("unchecked")
    Optional<T> t = (Optional<T>) EMPTY;
    return t;
}
```

- 该方法用来构造一个空的 `Optional`，即该 `Optional` 中不包含值 

-  其实底层实现还是 **如果 Optional 中的 value 为 null 则该 Optional 为不包含值的状态**，然后在 API 层面将 `Optional` 表现的不能包含 `null`值，使得 `Optional` 只存在 **包含值** 和 **不包含值** 两种状态。



**注意：**

- javadoc 也有提到，`Optional` 的 `isPresent()` 方法用来判断是否包含值，`get()` 用来获取 `Optional` 包含的值 —— **值得注意的是，如果值不存在，即在一个Optional.empty 上调用 get() 方法的话，将会抛出 NoSuchElementException异常**。



### 5.2.2 Optional 的使用方法

#### 1、isPresent

```java
public boolean isPresent() {
    return value != null;
}
```

- 如果存在值，则返回true；反之，返回false。

- 如果所包含的对象不为null，则返回true，反之返回false。
- 通常在对对象执行任何其他操作之前，先在Optional上调用此方法。

**使用示例：**

```java
Optional optional1 = Optional.of("javaone");
if (optional1.isPresent()){
//Do something, normally a get
}
```



#### 2、get

```java
public T get() {
        if (value == null) {
            throw new NoSuchElementException("No value present");
        }
        return value;
    }
```

- 如果此Optional中存在值，则返回该值，否则抛出 NoSuchElementException。

- 在这之后，我们想要的是存储在Optional中的值，我们可以通过get()来获取它。

- 但是，当该值为null时，此方法将引发异常。这就需要 orElse() 方法来紧急救援。

**使用示例：**

```java
Optional<String> optional1 = Optional.of("javaone");
if (optional1.isPresent()){ 
  String value = optional1.get();
}
```



#### 3、ifPresent

```java
public void ifPresent(Consumer<? super T> consumer) {
    if (value != null)
        consumer.accept(value);
}
```

- 如果存在值，则使用该值调用指定的使用者；
- 否则，什么都不做。

**使用示例：**

```java
//ifpresent
Optional<User> user = Optional.ofNullable(getUserById(id));
user.ifPresent(u -> System.out.println("Username is: " + u.getUsername()));
```



#### 4、orElse

```java
public T orElse(T other) {
    return value != null ? value : other;
}
```

- 如果 `Optional` 中有值则将其返回，否则返回 `orElse` 方法传入的参数。

**使用示例：**

```java
User user = Optional
        .ofNullable(getUserById(id))
        .orElse(new User(0, "Unknown"));
System.out.println("Username is: " + user.getUsername());
```



#### 5、orElseGet

```java
public T orElseGet(Supplier<? extends T> other) {
    return value != null ? value : other.get();
}
```

- `orElseGet` 与 `orElse` 方法的区别在于:
  - `orElseGet` 方法传入的参数为一个 `Supplier`接口的实现,
    - 当 `Optional` 中有值的时候，返回值；
    - 当 `Optional` 中没有值的时候，返回从该 `Supplier` 获得的值。

**使用示例：**

```java
User user = Optional
        .ofNullable(getUserById(id))
        .orElseGet(() -> new User(0, "Unknown")); 
System.out.println("Username is: " + user.getUsername());
```

#### 6、orElseThrow

```java
public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X {
    if (value != null) {
        return value;
    } else {
        throw exceptionSupplier.get();
    }
}
```

- `orElseThrow` 与 `orElse` 方法的区别在于:
  - `orElseThrow` 方法当 `Optional` 中有值的时候，返回值；
  - 没有值的时候会抛出异常，抛出的异常由传入的 *exceptionSupplier* 提供。

**使用示例：**

- 在 SpringMVC 的控制器中，我们可以配置统一处理各种异常。
- 查询某个实体时，如果数据库中有对应的记录便返回该记录，否则就可以抛出 `EntityNotFoundException` ，处理 `EntityNotFoundException` 的方法中我们就给客户端返回Http 状态码 404 和异常对应的信息 —— `orElseThrow` 完美的适用于这种场景。

```java
@RequestMapping("/{id}")
public User getUser(@PathVariable Integer id) {
    Optional<User> user = userService.getUserById(id);
    return user.orElseThrow(() -> new EntityNotFoundException("id 为 " + id + " 的用户不存在"));
}

@ExceptionHandler(EntityNotFoundException.class)
public ResponseEntity<String> handleException(EntityNotFoundException ex) {
    return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
}
```

#### 6、map

```java
public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent())
        return empty();
    else {
        return Optional.ofNullable(mapper.apply(value));
    }
}
```

- 如果当前 `Optional` 为 `Optional.empty`，则依旧返回 `Optional.empty`；
  - 否则返回一个新的 `Optional`，该 `Optional` 包含的是：函数 *mapper* 在以 *value* 作为输入时的输出值。

**使用示例：**

```java
String username = Optional.ofNullable(getUserById(id))
                        .map(user -> user.getUsername())
                        .orElse("Unknown")
                        .ifPresent(name -> System.out.println("Username is: " + name));

```

而且我们可以多次使用 `map` 操作（即 `map` 可以**无限级联**）：

```java
Optional<String> username = Optional.ofNullable(getUserById(id))
                                .map(user -> user.getUsername())
                                .map(name -> name.toLowerCase())
                                .map(name -> name.replace('_', ' '))
                                .orElse("Unknown")
                                .ifPresent(name -> System.out.println("Username is: " + name));
```



#### 7、flatMap

```java
public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent())
        return empty();
    else {
        return Objects.requireNonNull(mapper.apply(value));
    }
}
```

- `flatMap` 方法与 `map` 方法的区别在于：
  - `map` 方法参数中的函数 `mapper` 输出的是值，然后 `map` 方法会使用 `Optional.ofNullable` 将其包装为 `Optional`；
  - 而 `flatMap` 要求参数中的函数 `mapper` 输出的就是 `Optional`。

**使用示例：**

```java
Optional<String> username = Optional.ofNullable(getUserById(id))
                                .flatMap(user -> Optional.of(user.getUsername()))
                                .flatMap(name -> Optional.of(name.toLowerCase()))
                                .orElse("Unknown")
                                .ifPresent(name -> System.out.println("Username is: " + name));
```



#### 8、filter

```java
public Optional<T> filter(Predicate<? super T> predicate) {
    Objects.requireNonNull(predicate);
    if (!isPresent())
        return this;
    else
        return predicate.test(value) ? this : empty();
}
```

- `filter` 方法接受一个 `Predicate` 来对 `Optional` 中包含的值进行过滤，
  - 如果包含的值满足条件，那么还是返回这个 `Optional`；
  - 否则返回 `Optional.empty()`。

**使用示例：**

```java
Optional<String> username = Optional.ofNullable(getUserById(id))
                                .filter(user -> user.getId() < 10)
                                .map(user -> user.getUsername());
                                .orElse("Unknown")
                                .ifPresent(name -> System.out.println("Username is: " + name));
```





#### 使用小结

Optional 使用步骤：

1. 先使用三种静态构建方法，**构建**一个 Optional 对象；
2. 再根据业务需求，使用方法**操作** Optional 对象。

```java
public class OptionalTest {
 2     public static void main(String[] arg) {
 3         //创建Optional对象，如果参数为空直接抛出异常
 4         Optional<String> str=Optional.of("a");
 5 
 6         //获取Optional中的数据,如果不存在，则抛出异常
 7         System.out.println(str.get());
 8 
 9         //optional中是否存在数据
10         System.out.println(str.isPresent());
11 
12         //获取Optional中的值，如果值不存在，返回参数指定的值
13         System.out.println(str.orElse("b"));
14 
15         //获取Optional中的值，如果值不存在，返回lambda表达式的结果
16         System.out.println(str.orElseGet(()->new Date().toString()));
17 
18         //获取Optional中的值，如果值不存在，抛出指定的异常
19         System.out.println(str.orElseThrow(()->new RuntimeException()));
20 
21 
22 
23         Optional<String> str2=Optional.ofNullable(null);
24 
25         //optional中是否存在数据
26         System.out.println(str2.isPresent());
27 
28         //获取Optional中的值，如果值不存在，返回参数指定的值
29         System.out.println(str2.orElse("b"));
30 
31         //获取Optional中的值，如果值不存在，返回lambda表达式的结果
32         System.out.println(str2.orElseGet(()->new Date().toString()));
33 
34         //获取Optional中的值，如果值不存在，抛出指定的异常
35         System.out.println(str2.orElseThrow(()->new RuntimeException()));
36     }
37 }
```



## 5.3 Optional 实践注意事项

### 5.3.1 何时使用

- **Optional**的预期用途**主要是作为返回类型**。获取此类型的实例后，可以提取该值（如果存在）或提供其他行为（如果不存在）。

- Optional类的一个非常有用的用例是将其与流或返回Optional值以构建流畅的API的其他方法结合。请参见下面的代码段

```java
User user = users.stream().findFirst().orElse(new User("default", "1234"));
```

### 5.3.2 什么时候不使用

a）不要将其用作类中的字段，因为它不可序列化

如果确实需要序列化包含Optional值的对象，则Jackson库提供了将Optionals视为普通对象的支持。这意味着Jackson将空对象视为空，将具有值的对象视为包含该值的字段。可以在jackson-modules-java8项目中找到此功能。

b）不要将其用作构造函数和方法的参数，因为这会导致不必要的复杂代码。

```
User user = new User("john@gmail.com", "1234", Optional.empty());
```







# 6、method.invoke()

- 参考博客：https://blog.csdn.net/qq_34562093/article/details/84889499

- 作用，method.invoke(Object obj,Object args[])的作用就是调用method类代表的方法，
  - 其中obj是对象名，（这个方法所在的对象）
  - args是传入method方法的参数

举个例子：如果接口中没有close方法，但是实现类中提供了close，那么就可以用反射来处理，调用实现类的close方法

注意：

- invoke()方法接收的参数必须为**对象**,如果参数为基本类型数据,必须转换为相应的包装类型的对象。

- invoke()方法的返回值总是对象,如果实际被调用的方法的返回类型是基本类型数据,那么invoke()方法会把它转换为相应的包装类型的对象,再将其返回.



有四种**获得method对象的方法**，返回结果是method对象，或者说是 方法的全限定名：
　一共有4种方法,全部都在Class类中:

   - getMethods(): 获得类的public类型的方法
   - getMethod(String name, Class[] params): 获得类的特定方法,
           - name参数：指定方法的名字,
           - params参数：指定方法的参数类型
   - getDeclaredMethods(): 获取类中所有的方法(public、protected、default、private)
   - getDeclaredMethod(String name, Class[] params): 获得类的特定方法,name参数指定方法的名字,params参数指定方法的参数。















# THE END