# 目录

[toc]

# Java8 新特性



- Java 8 新特性 官方文档：https://docs.oracle.com/javase/8/docs/technotes/guides/collections/changes8.html
- 

主要是：

- lambda表达式
- 函数式接口
- 新的Stream API
- 新的日期 API
- 其它

# 1、Lambda 表达式

- 特殊的匿名内部类，语法更简洁

- lambda 表达式允许把函数作为一个方法的参数（函数作为方法的参数传递），将代码像数据一样传递

- 基本语法：

- ```java
  <函数式接口> <变量名> = (参数列表)->{
      //方法体
  };
  ```

- Lambda引入了新的操作符：-> (箭头操作符) ，将表达式分成两部分

  - 左侧：（参数1，参数2，。。。）表示参数列表
  - 右侧：{}内部式方法体

- 简化注意事项：

  - 形参列表的数据类型会自动推断
  - 如果形参列表为空，只需保留()
  - 如果形参只有一个，（）可以省略，只需要参数的名称即可
  - 如果执行语句只有一句，且无返回值，{}可以省略，若有返回值，则若想省去{}，则必须同时省略return , 且执行语句也保证只有一句
  - Lambda 不会生成一个单独的内部类文件（即 .class 文件）

## 1.1 函数式接口

- 定义：一个接口只有一个抽象方法，则该接口为函数式接口
- 只有函数式接口可以使用Lambda 表达式，Lambda表达式会匹配到这个抽象方法上。
- @FunctionalInterface 注解可以检测接口是否符合函数式接口

### 1.1.1 常见函数式接口

| 函数式接口                   | 参数类型 | 返回类型 | 说明                                                         |
| ---------------------------- | -------- | -------- | ------------------------------------------------------------ |
| Consumer<T>消费型接口        | T        | void     | void accept(T t) 对类型为T的对象应用操作                     |
| Supplier<T>供给型接口        | 无       | T        | T get() 返回类型为T 的对象                                   |
| Function<T,R>函数型接口      | T        | R        | R apply(T t) 对类型为T的对象应用操作，并返回类型为R 的对象   |
| Predicate<T>断言型接口(判断) | T        | boolean  | boolean test(T t) 确定类型为T的对象是否满足条件，并返回 boolean 类型 |
| Runnable                     |          |          | void run();                                                  |
| Comparator<T>                | T        |          | int compare(T o1, T o2);                                     |

### 1.1.2 使用

```java
/**
 * 常见函数式接口的Lambda 表达式使用
 */
public class Demo02 {
    public static void main(String[] args) {
        //Consumer 消费型接口
        Consumer<Double> consumer = t-> System.out.println("聚会消费:" + t);
        test1(consumer,10000);
//        test1(t-> System.out.println("聚会消费:" + t)，1000);//也可以直接用

        //Supplier 供给型接口
        int[] nums = getNums(() -> new Random().nextInt(100), 5);
        System.out.println(Arrays.toString(nums));

        //Function 函数式接口
        System.out.println(testFunction(s -> s.toUpperCase(),"function"));

        //Predicate 断言式接口(判断)
        ArrayList<String> arrayList = new ArrayList<>();
        arrayList.add("1orange1");
        arrayList.add("1purple22");
        arrayList.add("3durian333");
        arrayList.add("watermelon4");

        System.out.println(testPredicate(s -> s.startsWith("1"),arrayList).toString());
        System.out.println(testPredicate(s -> s.length() > 8,arrayList));

    }
    public static void test1(Consumer<Double> consumer,double money){
        consumer.accept(money);
    }

    public static int[] getNums(Supplier<Integer> supplier,int count){
        int[] arr = new int[count];
        for (int i = 0; i < count; i++) {
            arr[i] = supplier.get();
        }

        return arr;

    }

    public static String testFunction(Function<String,String> function,String str){
        return function.apply(str);
    }

    public static List<String> testPredicate(Predicate<String> predicate,ArrayList<String> list){
        ArrayList<String> arrayList = new ArrayList<>();
        for (String string : list) {
            if (predicate.test(string)){
                arrayList.add(string);
            }
        }
        return arrayList;
    }
}

```

## 1.2 方法引用

- 方法引用式 Lambda 表达式的一种简写形式。
- 如果 Lambda 表达式方法体中只是调用一个特定的已经存在的方法，则可以使用方法引用
- 常见形式：（只要 引用方法的特征 和 接口的特点是一样的，就可以用，）
  - 对象::实例方法
  - 类::静态方法
  - 类::实例方法
  - 类::new

### 1.2.1 方法引用的使用

```java
/**
 * 方法引用 的使用
 * 只要引用方法的特征和接口的特点是一样的，就可以用，
 * - 1 对象::实例方法
 * - 2 类::静态方法
 * - 3 类::实例方法
 * - 4 类::new
 */
public class Demo03 {
    public static void main(String[] args) {
        //1 对象::实例方法
        //比如 println()也是带参无返回值型，Consumer接口的accept()也是带参无返回值型
        //普通lambda
        Consumer<String> consumer = s -> System.out.println(s);
        consumer.accept("hello");
        //方法引用
        Consumer<String> consumer1 = System.out::println;
        System.out.println("java");

        // 2 类::静态方法
        Comparator<Integer> comparator = (o1,o2)->Integer.compare(o1,o2);
        Comparator<Integer> comparator1 = Integer::compare;

        //3 类::实例方法,自己构造的类 Employee
        Function<Employee,String> function = (e)->e.getName();
        Function<Employee,String> function1 = Employee::getName;
        System.out.println(function1.apply(new Employee("apple")));

        //4 类::new
        Supplier<Employee> supplier = ()->new Employee();
        Supplier<Employee> supplier1 = Employee::new;
        Employee employee = supplier1.get();
        System.out.println(employee.toString());

    }
}
```

# 2、Stream API

- Java 8 Stream API 官方文档 ：
  - https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html
- Stream 使用实例：https://mp.weixin.qq.com/s/tJh3yRR0CUxv6y8oVX8lng

- 流（Stream）中**保存**对集合或数组数据的**操作**。和集合类似，但集合中保存的是数据

  ![Snipaste_2021-02-21_14-19-40](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-02-21_14-19-40.png)

- lambda 表达式与 函数式接口 为的就是要在Stream API 中来使用

## 2.1 Stream 的特点

- Stream 自己不会存储元素。存储的是操作
- Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream 
- Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行

## 2.2 Stream的使用步骤

- 创建
  - 新建一个流
- 中间操作
  - 在一个或多个步骤中，将初始化Stream转化到另一个Stream的中间操作
- 终止操作
  - 使用一个终止操作来产生结果。该操作会强制它之前的延迟操作立即执行。在这之后，该Stream 就不能使用了。



### 2.2.1 常见的中间操作

- filter,limit,skip,distinct（去重）, sorted
  - 这些中间操作都是和前面的函数式接口与lambda表达式结合使用
- map（把流映射成另一组数据）
- parallel（获取一个并行流，采用多线程，效率高）

```java
/**
 * Stream 中间操作
 * filter 过滤,limit 限制（元素个数）,skip 跳过,distinct（去重）, sorted 排序
 * map 把数据映射成另外一组数据
 * parallel 获取一个并行流，采用多线程，效率高
 */
public class Demo04 {
    public static void main(String[] args) {
        ArrayList<Employee> list = new ArrayList<>();
        list.add(new Employee("张三",1000));
        list.add(new Employee("李四",2000));
        list.add(new Employee("王五",300));
        list.add(new Employee("孙子",4000));
        list.add(new Employee("荀子",5000));
        list.add(new Employee("荀子",5000));

        //1 filter
        list.stream()
                .filter(e->e.getMoney() > 3000)
                .forEach(System.out::println);
        //2 limit
        System.out.println("===============");
        list.stream()
                .limit(2)
                .forEach(System.out::println);
        //3 skip
        System.out.println("===============");
        list.stream()
                .skip(3)
                .forEach(System.out::println);
        //4 distinct 需要重写hashCode()和equals()
        System.out.println("==================");
        list.stream()
                .distinct()
                .forEach(System.out::println);
        //5 sorted
        System.out.println("==================");
        list.stream()
                .sorted((o1, o2) -> Double.compare(o1.getMoney(), o2.getMoney()))//按照工资排序
                .forEach(System.out::println);
        //6 map 把数据映射成另外一组数据
        list.stream()
                .map(employee -> employee.getName())
                .forEach(System.out::println);
        //7 parallel 获取一个并行流，采用多线程，效率高
        list.parallelStream()
                .forEach(System.out::println);
    }
}
```

### 2.2.2 终止操作

- forEach, min, max , count
- reduce , collect

```java
/**
 * Stream 的使用 终止操作
 * forEach , min , max , count , reduce , collect
 *
 */
public class Demo06 {
    public static void main(String[] args) {
        ArrayList<Employee> list = new ArrayList<>();
        list.add(new Employee("张三",1000));
        list.add(new Employee("李四",2000));
        list.add(new Employee("王五",300));
        list.add(new Employee("孙子",4000));
        list.add(new Employee("荀子",5000));

        //forEach 遍历，用的最多，
        list.stream()
                .filter(employee -> employee.getMoney() > 3000)
                .forEach(System.out::println);
        //min 最小 max 最大 count(计算数据个数)
        System.out.println("==========");
        Optional<Employee> min = list.stream()//用Optional 是为了解决控制异常问题，比如没找到，或者表达式有问题
                .min(((o1, o2) -> Double.compare(o1.getMoney(), o2.getMoney())));
        System.out.println(min.get());

        System.out.println("==========");
        Optional<Employee> max = list.stream()
                .max(((o1, o2) -> Double.compare(o1.getMoney(), o2.getMoney())));
        System.out.println(max.get());

        long count = list.stream().count();
        System.out.println("员工个数：" + count);
        //reduce 规约
        //计算所有员工的工资和
        Optional<Double> sum = list.stream()
                .map(e -> e.getMoney())
                .reduce((x, y) -> x + y);//reduce里是一个二元操作符，就是两个参数，一个返回值
        System.out.println(sum.get());
        //collection 收集
        //获取所有员工的姓名，封装成一个list 集合
        List<String> collect = list.stream()
                .map(employee -> employee.getName())
                .collect(Collectors.toList());
        for (String name : collect) {
            System.out.println(name);
        }

    }
}
```



## 2.3 补充：Stream API 详解

### 2.3.1 中间操作

#### 1、filter



#### 2、foreach



#### 3、map



#### 4、distinct



#### 5、limit



#### 6、skip



7、







### 2.3.2  终止操作



#### 1、count



#### 2、partitioningBy



#### 3、groupingBy



#### 4、joining



5、

# 3、新时间API

- 之前的时间API 存在问题：线程安全问题，设计混乱

## 新的时间API分类

- 本地化日期API 的类
  - LocalDate类 只有日期
  - LocalTime 类只有时间
  - LocalDateTime类 日期+时间
- Instant:时间戳，表示从1970年0时0分0秒到现在，表示一个瞬间
- ZoneId: 时区
- DateTimeFormatter (格式化类)

## LocalDateTime

````java
/**
 * 新的时间API
 * LocalDateTime类 的使用
 *
 */
public class Demo07 {
    public static void main(String[] args) {
        //1 创建本地时间
        LocalDateTime localDateTime = LocalDateTime.now();
//        LocalDateTime localDateTime1 = LocalDateTime.of();
        System.out.println(localDateTime);
        System.out.println(localDateTime.getDayOfMonth());
        System.out.println(localDateTime.getYear());
        System.out.println(localDateTime.getMonthValue());
        System.out.println(localDateTime.getMonth());

        //2 添加两天，调用方法都是返回一个新的对象，LocalDateTime 的对象也具有不可变性
        LocalDateTime localDateTime1 = localDateTime.plusDays(2);
        System.out.println(localDateTime1);

        //3 减少
        LocalDateTime localDateTime2 = localDateTime.minusMonths(1);
        System.out.println(localDateTime2);
    }
}
````

## Instant 和ZoneId 

```java
/**
 * Instant 时间戳
 * ZoneId 时区
 */
public class Demo08 {
    public static void main(String[] args) {
        //1 创建 Instant
        Instant instant = Instant.now();
//        Instant.ofEpochMilli();//也可以用给毫秒数来创建
        System.out.println(instant.toString());//打印的是格林尼治时区的时间
        System.out.println(instant.toEpochMilli());//打印1970到现在的毫秒数
        //2 添加减少时间
        Instant instant1 = instant.plusSeconds(10);
        //打印时间差Duration.between().to--()
        System.out.println(Duration.between(instant,instant1).toMillis());

        //3 ZoneId
//        Set<String> availableZoneIds = ZoneId.getAvailableZoneIds();//所有的时区
//        availableZoneIds.forEach(System.out::println);
        System.out.println(ZoneId.systemDefault().toString());//当前时区
    }
}
```

## Date , Instant , LocalDateTime 的转换

```java
 //4 Date , Instant , LocalDateTime 的转换
        System.out.println("===============");
        //4.1   Date--->Instant--->LocalDateTime

        Date date = new Date();
        Instant instant2 = date.toInstant();
        System.out.println(instant2);

        LocalDateTime localDateTime = LocalDateTime.ofInstant(instant2, ZoneId.systemDefault());
        System.out.println(localDateTime);

        //4.2 LocalDateTime--->Instant--->Date
        System.out.println("========");
        Instant instant3 = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
        System.out.println(instant3 );
        Date from = Date.from(instant3);
        System.out.println(from);
```

## DateTimeFormatter (格式化类)

- 线程安全的格式化类，而SimpleDateFormatter 是线程不安全的

```java
/**
 * DateTimeFormatter 的使用
 */
public class Demo09 {
    public static void main(String[] args) {
        //创建DateTimeFormatter
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");//创建里of--里可以有很多种，
        //把时间格式化成字符串
        String format = dtf.format(LocalDateTime.now());
        System.out.println(format);
        //把字符串解析成时间
        LocalDateTime parse = LocalDateTime.parse("2021-03-10 10:10:20",dtf);
        System.out.println(parse);
    }
}

```



# THE END