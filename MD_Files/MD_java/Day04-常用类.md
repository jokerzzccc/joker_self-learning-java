# Day 04 

# 常用类

## 1. Object类

### 定义

1. object类是所有类的父类
2. Object类中所定义的方法，是所有对象都具备的方法
3. Object类型可以存储任何对象

### 常用方法

1. getClass()方法：

   ```java
   public final Class<?> getClass(){}
   ```

   - 返回引用中存储的实际类型对象
   - 应用：通常用于判断两个引用中实际存储对象类型是否一致

2. hashCode()方法：

   ```java
   public int hashCode() {}
   ```

   - 返回该对象的哈希码值
   - 哈希值是， 根据对象的地址，或字符串，或数字，使用hash算法计算出来的int类型的数值
   - 一般情况下，相同对象返回相同哈希码

3. toString()方法

   ```java
   public String toString() {}
   ```

   - 返回该对象的字符串表示（表现形式）
   - 可以根据程序需求覆盖该方法，如：展示对象各个属性值

4. equals() 方法

   ```java
   public boolean equals(Object obj){}
   ```

   - 默认实现为（this == obj）,比较两个对象地址是否相同
   - 可以进行覆盖，比较两个对象的内容是否相同
   - equals()方法覆盖步骤：
     - 比较两个引用是否指向同一个对象
     - 判断obj是否为null
     - 判断两个引用指向的实际对象类型是否一致
     - 强制类型转换
     - 依次比较各个属性值是否相同

5. finalize()方法（已经废弃）

   1. 当对象被判定为垃圾对象时，由 JVM 自动调用此方法，用以标记垃圾对象，进入回收队列
   2. 垃圾对象：没有有效引用指向此对象时，为垃圾对象
   3. 垃圾回收：由 GC 销毁垃圾对象，释放数据存储空间
   4. 自动回收机制：JVM 的内存耗尽，一次性回收所有垃圾对象
   5. 手动回收机制：使用 System.gc()；通知 jvm 执行垃圾回收

# 包装类

## 定义

1. 基本数据类型所对应的引用数据类型
2. Object可统一所有数据，包装类的默认值是null

## 类型转换和装箱、拆箱

1. 装箱：基本类型转成引用类型
2. 拆箱：引用类型转成基本类型
3. 基本类型放在栈里，引用类型放在堆里
4. jdk1.5 后提供自动装箱、拆箱

## 基本类型和字符串的转换

1. 基本类型转换成字符串

   - 使用+号

   - ```java
     int n1 = 10;
     String s1 = n1 + "";
     ```

   - 使用integer的toString（）方法

   - ```java
     String s2 = Integer.toString(n1,16);
     ```

2. 字符串转换成基本类型

   - 使用Integer.parseXXX();

   - ```java
     String str = "150";//不能出现非字母的数字
     int num2 = Integer.parseInt(str);
     ```

3. Boolean字符串形式转成基本类型。

   ```;;
   String str2 = "true";//只要是非true的字母，都是转成false	
   booleanm b1 = Boolean.parseBoolean(str2);
   ```

## 整数缓冲区

1. Java预先创建了256个常用的整数包装类型对象



# String类

## 定义

1. 字符串是常量，创建之后不可改变：给字符串变量name1重新赋值时，并没有修改数据，而是重新在方法区里开辟一个空间
2. 字符串字面值存储在字符串池中，可以共享：又定义一个name2赋值于name1 则，共享字符串池中相同的值
3. 字符串池在方法区中

## 创建字符串

- 方式1：

  ```java
  String s = "hello";//产生一个对象，字符串池中存储
  ```

- 方式2：

  ```java
  String s = new String("hello");//产生两个对象，堆，字符串池中各存储一个，
  ```

  

## 常用方法

- public int length() :返回字符串长度

- public char charAt(int index) : 根据下标获取字符

- public boolean contains(String str): 判断当前的字符串中是否包含str

- public char[] toCharArray() :将字符串转换成数组 

  ```java
  String content1 = "java is the best programming language"; System.out.println(Arrays.toString(content1.toCharArray()) );
  ```

  

- public int indexOf(String str) :查找str首次出现的下标，存在，则返回该下标，不存在则返回-1

  ```java
  content1.indexOf(java);
  ```

  

- public int lastIndexOf(String str) :查找字符串在当前字符串中最后一次出现的下标索引

- public String trim() :去点字符串前后的空格

- public String toUpperCase() ：将小写转换成大写，toLowerCase(), 把大写转换为小写

- public boolean endWith(String str) ：判断字符串是否以str结尾

- public boolean startWith(String str) ：判断字符串是否以str开头

- public String replace(char oldChar, char newChar):将旧字符串替换成新字符串

- public String[] split(String str): 根据str做拆分

- 补充：

- equals()方法比较值是否相等

- equalsIgnoreCase()方法，忽略大小写

- compareTo()；比较大小,字母的位置

- subString（）方法：按索引截取字符串的一部分

## StringBuffer与StringBuilder类

1. StringBuffer：可变长字符串，运行效率慢，线程安全性高，有锁，无锁
2. StringBuider：可变长字符串，运行快，线程不安全
3. 两者和String的区别：
   - 比String的效率高
   - 比String节省内存

## StringBuffer

方法：

1. append() 方法:追加，打印的时候要加toString()
2. insert() 方法：插入，可以在某个位置，插入各种类型的数据
3. replace() 方法：可以自定义位置，替换，含头不含尾原则，
4. delete() 方法：删除，指定索引区间，可以用来清空

## StringBuilder

与StringBuffer的方法一样，效率高于StringBuffer,

# BigDecimal类

1. double是近似存储值，在精确运算中，如银行，就需要用BigDecimal类。

2. 位置，java.math.bigdecima

3. 作用：精确计算浮点数

4. 创建方式：方法中一定要选择字符串

   ```java
   BigDecimal bd1 = new BigDecimal("1.0");
   BigDecimal bd2 = new BigDecimal("0.9");
   ```

5. 方法：

   - 减法：subtract()

     ```java
     BigDecimal res = bd1.subtract(bd2);
     ```

     

   - 加法：add()

   - 乘法：multiply()

   - 除法：divide() ，有除不尽的时候，要保留几位小数，选择四舍五入，

     ```java
     BigDecimal bd3 = new BigDecimal("10").divide(new BigDecimal("3"),3,RoundingMode.HALF_UP);
     ```

# Date类

- Date类表示特定的瞬间，精确到毫秒，Date类中的大部分方法都已经被Calendar类中的方法取代。

# Calendar类

1. Calendar提供了获取或设置各种日历字段的方法
2. 构造方法：protected Calendar() ：由于修饰符是protected,所以无法直接创建该对象，要使用Calendar.getInstance()来创建对象

## 方法

1. getInstance()方法,创建Calendar对象

   ```java
   Calendar calendar = Calendar.getInstance();
   ```

   

2. get()方法，取年月日等字段值

   ```java
   int year = calendar.get(calendar.YEAR);
   ```

   

3. set()修改字段值

   ```java
   Calendar calendar1 = Calendar.getInstance();
   calendar1.set(Calendar.DAY_OF_MONTH,4);
   ```

   

4. add()，添加方法修改字段值

   ```java
   calendar1.add(Calendar.HOUR,1);
   ```

   

5. getActualMaxnum(),getActualMinnum()取得最大值，比如：当月有多少天

   ```java
   	int min = calendar1.getActualMinimum(Calendar.DAY_OF_MONTH);
   ```

# SimpleDateFormat类

1. java.text.SimpleDateFormat;

2. 是一个以 与语言环境有关的方式来格式化和解析日期的具体类。

3. 进行格式化（日期->文本）；解析（文本 -> 日期）

4. 常用的时间模式字母：

   字母|日期或时间|示例
   --|--|--
   y|年|2019
   M|年中月份|
   d|月中天数|
   H|1天中小时数（0-23）|
   m|分钟|
   s|秒|
   S|毫秒|



# System类

1. System系统类，主要用于获取系统的属性数据和其他操作，构造方法私有的

   |             方法名              | 说明                                                   |
   | :-----------------------------: | ------------------------------------------------------ |
   |     static void arraycopy()     | 复制数组                                               |
   | static long currentTimeMillis() | 获取当前系统时间，返回的是毫秒值                       |
   |        static void gc()         | 建议jvm赶快启动垃圾回收器回收垃圾                      |
   |  static void exit(int status)   | 退出 jvm,如果参数为0，则是正常推出，非0表示异常退出jvm |

   

