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

- 另外还有 接口名带 Bi 的比如：BiFunction，Bipredicate，BiConsumer等用法与不带 Bi 的一样，只是区别在于：`Bi---`接受两个参数。



>  最常见的使用：

1. `consumer` :接口

   ```java
   // 1 输出
   System.out::println
   ```

2. `supplier` 接口：

   ```java
   // 1、实体类的属性的getter 方法。
   // 2、类的无参构造
   ArrayList::new
   ```

3. `function`接口：就是一个功能转换型接口，把一个数据转换成另一组数据。

   ```java
   // 1、String 类的返回长度的方法：String -> Integer
   String::length
   ```

   



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

### 2.2.1 创建流的方式

生成流的方式主要有五种：

+ 通过**集合**生成，应用中最常用的一种

  ```
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  Stream<Integer> stream = integerList.stream();
  ```

  通过集合的`stream`方法生成流

+ 通过**数组**生成

  ```
  int[] intArr = new int[]{1, 2, 3, 4, 5};
  IntStream stream = Arrays.stream(intArr);
  ```

  通过`Arrays.stream`方法生成流，并且该方法生成的流是数值流【即`IntStream`】而不是`Stream<Integer>`。补充一点使用数值流可以避免计算过程中拆箱装箱，提高性能。`Stream API`提供了`mapToInt`、`mapToDouble`、`mapToLong`三种方式将对象流【即`Stream<T>`】转换成对应的数值流，同时提供了`boxed`方法将数值流转换为对象流

+ 通过**值**生成

  ```
  Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
  ```

  通过`Stream`的`of`方法生成流，通过`Stream`的`empty`方法可以生成一个空流

+ 通过**文件**生成

  ```
  Stream<String> lines = Files.lines(Paths.get("data.txt"), Charset.defaultCharset())
  ```

  通过`Files.line`方法得到一个流，并且得到的每个流是给定文件中的一行

+ 通过**函数**生成 提供了`iterate`和`generate`两个静态方法从函数中生成流

+ + iterator

    ```
    Stream<Integer> stream = Stream.iterate(0, n -> n + 2).limit(5);
    ```

    `iterate`方法接受两个参数，第一个为初始化值，第二个为进行的函数操作，因为`iterator`生成的流为无限流，通过`limit`方法对流进行了截断，只生成5个偶数

  + generator

    ```
    Stream<Double> stream = Stream.generate(Math::random).limit(5);
    ```

    `generate`方法接受一个参数，方法参数类型为`Supplier<T>`，由它为流提供值。`generate`生成的流也是无限流，因此通过`limit`对流进行了截断

### 2.2.2 常见的中间操作

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

### 2.2.3 终止操作

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

- 通过使用`filter`方法进行条件筛选，`filter`的方法参数为一个条件

- 方法：

  ```java
  Stream<T> filter(Predicate<? super T> predicate);
  ```



- 多个过滤器时，使用 and() （&&），or()  （||）, negate() （！=）连接。

  ```java
  ArrayList<String> list = new ArrayList<>();
  list.add("a");
  list.add("b");
  
  list.stream().filter(x -> x.contains("a") && x.contains("b")).forEach(System.out::println);
  
  // 等同于如下
  Predicate<String> p1 = x -> x.contains("a");
  Predicate<String> p2 = x -> x.contains("b");
          
  list.stream().filter(p1.and(p2)).forEach(System.out::println);
  ```

  

#### 2、foreach



#### 3、map

- 所谓流映射就是将接受的元素映射成另外一个元素

- ```java
  List<String> stringList = Arrays.asList("Java 8", "Lambdas",  "In", "Action");
  Stream<Integer> stream = stringList.stream().map(String::length);
  ```

- 通过`map`方法可以完成映射，该例子完成中`String -> Integer`的映射，之前上面的例子通过`map`方法完成了`Dish->String`的映射

#### 4、distinct

- 通过`distinct`方法快速去除重复的元素

#### 5、limit

- 通过`limit`方法指定返回流的个数，`limit`的参数值必须`>=0`，否则将会抛出异常

#### 6、skip

- 通过`skip`方法跳过流中的元素，如下例子跳过前两个元素，所以打印结果为`2,3,4,5`，`skip`的参数值必须`>=0`，否则将会抛出异常

- ```java
  List<Integer> integerList = Arrays.asList(1, 1, 2, 3, 4, 5);
  Stream<Integer> stream = integerList.stream().skip(2);
  ```

7、**flatMap**

- 将一个流中的每个值都转换为另一个流

  ```java
  List<String> wordList = Arrays.asList("Hello", "World");
  List<String> strList = wordList.stream()
          .map(w -> w.split(" "))
          .flatMap(Arrays::stream)
          .distinct()
          .collect(Collectors.toList());
  ```

- `map(w -> w.split(" "))`的返回值为`Stream<String[]>`，我们想获取`Stream<String>`，可以通过`flatMap`方法完成`Stream<String[]> ->Stream<String>`的转换





### 2.3.2  终止操作



#### 1、count

- 通过使用`count`方法统计出流中元素个数

- ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  Long result = integerList.stream().count();
  ```

#### 2、partitioningBy

- **进阶通过partitioningBy进行分区**

- 分区是特殊的分组，它分类依据是true和false，所以返回的结果最多可以分为两组

- ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  Map<Boolean, List<Integer>> result = integerList.stream().collect(partitioningBy(i -> i < 3));
  ```

- 返回值的键仍然是布尔类型，但是它的分类是根据范围进行分类的，分区比较适合处理根据范围进行分类

#### 3、groupingBy

- **进阶通过groupingBy进行分组**

- 在`collect`方法中传入`groupingBy`进行分组，其中`groupingBy`的方法参数为分类函数。还可以通过嵌套使用`groupingBy`进行多级分类

- ```java
  Map<Type, List<Dish>> result = dishList.stream().collect(groupingBy(Dish::getType));
  // 多级分类
  Map<Type, List<Dish>> result = menu.stream().collect(groupingBy(Dish::getType,
          groupingBy(dish -> {
              if (dish.getCalories() <= 400) return CaloricLevel.DIET;
                  else if (dish.getCalories() <= 700) return CaloricLevel.NORMAL;
                  else return CaloricLevel.FAT;
          })));
  ```

- 

#### 4、joining

- **通过joining拼接流中的元素**

- 默认如果不通过`map`方法进行映射处理拼接的`toString`方法返回的字符串，joining的方法参数为元素的分界符，如果不指定生成的字符串将是一串的，可读性不强

- ```java
  String result = menu.stream().map(Dish::getName).collect(Collectors.joining(", "));
  
  ```

- 

#### 5、**reduce**

- 接口：

  ```java
  T reduce(T identity, BinaryOperator<T> accumulator);
  
  Optional<T> reduce(BinaryOperator<T> accumulator);
  
  <U> U reduce(U identity,
                   BiFunction<U, ? super T, U> accumulator,
                   BinaryOperator<U> combiner);
  ```

  

- **将流中的元素组合起来**

- 假设我们对一个集合中的值进行求和

  ```java
  // 方式一
  int sum = 0;
  for (int i : integerList) {
    sum += i;
  }
  // 方式二
  int sum = integerList.stream().reduce(0, (a, b) -> (a + b));
  
  // 方式三
  int sum = integerList.stream().reduce(0, Integer::sum);
  
  ```

- `reduce`接受两个参数，一个初始值这里是`0`，一个`BinaryOperator<T> accumulator` 来将两个元素结合起来产生一个新值， 另外`reduce`方法还有一个没有初始化值的重载方法



### 2.3.3 按功能分

> **获取流中最小最大值**

+ 通过 **min/max** 获取最小最大值

  ```java
  Optional<Integer> min = menu.stream().map(Dish::getCalories).min(Integer::compareTo);
  Optional<Integer> max = menu.stream().map(Dish::getCalories).max(Integer::compareTo);
  ```

  也可以写成：

  ```java
  OptionalInt min = menu.stream().mapToInt(Dish::getCalories).min();
  OptionalInt max = menu.stream().mapToInt(Dish::getCalories).max();
  ```

  `min`获取流中最小值，`max`获取流中最大值，方法参数为`Comparator<? super T> comparator`

+ 通过 **minBy/maxBy** 获取最小最大值

  ```java
  Optional<Integer> min = menu.stream().map(Dish::getCalories).collect(minBy(Integer::compareTo));
  Optional<Integer> max = menu.stream().map(Dish::getCalories).collect(maxBy(Integer::compareTo));
  ```

  `minBy`获取流中最小值，`maxBy`获取流中最大值，方法参数为`Comparator<? super T> comparator`

+ 通过 **reduce** 获取最小最大值

  ```java
  Optional<Integer> min = menu.stream().map(Dish::getCalories).reduce(Integer::min);
  Optional<Integer> max = menu.stream().map(Dish::getCalories).reduce(Integer::max);
  ```





> **求和**

+ 通过 **summingInt**

  ```
  int sum = menu.stream().collect(summingInt(Dish::getCalories));
  ```

  如果数据类型为`double`、`long`，则通过`summingDouble`、`summingLong`方法进行求和

+ 通过 **reduce**

  ```
  int sum = menu.stream().map(Dish::getCalories).reduce(0, Integer::sum);
  ```

+ 通过 **sum**

  ```
  int sum = menu.stream().mapToInt(Dish::getCalories).sum();
  ```

在上面求和、求最大值、最小值的时候，对于相同操作有不同的方法可以选择执行。可以选择`collect`、`reduce`、`min/max/sum`方法，推荐使用`min`、`max`、`sum`方法。因为它最简洁易读，同时通过`mapToInt`将对象流转换为数值流，避免了装箱和拆箱操作





>  **通过averagingInt求平均值**

```
double average = menu.stream().collect(averagingInt(Dish::getCalories));
```

如果数据类型为`double`、`long`，则通过`averagingDouble`、`averagingLong`方法进行求平均



> **通过 summarizingInt 同时求总和、平均值、最大值、最小值**

```java
IntSummaryStatistics intSummaryStatistics = menu.stream().collect(summarizingInt(Dish::getCalories));
double average = intSummaryStatistics.getAverage();  //获取平均值
int min = intSummaryStatistics.getMin();  //获取最小值
int max = intSummaryStatistics.getMax();  //获取最大值
long sum = intSummaryStatistics.getSum();  //获取总和
```

- 如果数据类型为`double`、`long`，则通过`summarizingDouble`、`summarizingLong`方法



>  **返回集合**

```java
List<String> strings = menu.stream().map(Dish::getName).collect(toList());
Set<String> sets = menu.stream().map(Dish::getName).collect(toSet());
```





> **通过foreach进行元素遍历**

```java
List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
integerList.stream().forEach(System.out::println);
```





>  **查找**

提供了两种查找方式

+ **findFirst** 查找第一个

  ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  Optional<Integer> result = integerList.stream().filter(i -> i > 3).findFirst();
  ```

  - 通过`findFirst`方法查找到第一个大于三的元素并打印

+ **findAny** 随机查找一个

  ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  Optional<Integer> result = integerList.stream().filter(i -> i > 3).findAny();
  ```

  - 通过`findAny`方法查找到其中一个大于三的元素并打印，因为内部进行优化的原因，当找到第一个满足大于三的元素时就结束，该方法结果和`findFirst`方法结果一样。提供`findAny`方法是为了更好的利用并行流，`findFirst`方法在并行上限制更多.



>  **元素匹配**

提供了三种匹配方式

+ **allMatch** 匹配所有

  ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  if (integerList.stream().allMatch(i -> i > 3)) {
      System.out.println("值都大于3");
  }
  ```

  通过`allMatch`方法实现

+ **anyMatch** 匹配其中一个

  ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  if (integerList.stream().anyMatch(i -> i > 3)) {
      System.out.println("存在大于3的值");
  }
  ```

  等同于

  ```java
  for (Integer i : integerList) {
      if (i > 3) {
          System.out.println("存在大于3的值");
          break;
      }
  }
  ```

  存在大于3的值则打印，`java8`中通过`anyMatch`方法实现这个功能

+ **noneMatch** 全部不匹配

  ```java
  List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5);
  if (integerList.stream().noneMatch(i -> i > 3)) {
      System.out.println("值都小于3");
  }
  ```

  通过`noneMatch`方法实现







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