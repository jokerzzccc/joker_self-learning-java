# Day 06

# 泛型

## 概念

1. 泛型的本质：是参数化类型，把类型作为参数传递
2. 常见形式有，泛型类、泛型接口、泛型方法
3. 语法：
   - <T,...> T称为类型占位符，表示一种引用类型
4. 好处：
   - 提高代码的复用性
   - 防止类型转换异常，提高代码的安全性

## 使用

### 泛型类

创建泛型类

1. 创建泛型变量，不能用new来创建对象，因为泛型是不确定的类型，构造方法可能是私有
2. 泛型作为方法的参数
3. 泛型作为方法的返回值

```java
    //1创建泛型变量
//    不能使用new来创建对象，因为泛型是不确定的类型，构造方法可能是私有
    T t;

    //2泛型作为方法的参数
    public void show(T t){
        System.out.println(t);
    }
//    3泛型作为方法的返回值
    public T getT(){
            return t;
    }
```



创建泛型类对象：

1. 泛型类型只能是引用类型

   

   ```java
   MyGeneric<String> stringMyGeneric = new MyGeneric<>();
           stringMyGeneric.t = "hello";
           stringMyGeneric.show("generic_test");
           String str = stringMyGeneric.getT();
           System.out.println(stringMyGeneric.t +"-"+ str);
   ```

   

2. 不同泛型类型对象之间不能相互赋值

### 泛型接口

创建泛型接口 ：

```java
/**
 * 泛型接口
 * 语法： 接口名<T>
 * 注意： 不能泛型静态常量，因为不知道T是什么类型，即：T t = new T();
 */
public interface GenericInterface<T> {
    String name = "chen";

    T server(T t);

}
```

实现方式有两种：

1. 创建实现类时确定接口类型，比如String

   ```java
   public class InterfaceTest1 implements GenericInterface<String>{
   
       @Override
       public String server(String s) {
           System.out.println(s);
           return s;
       }
   }
   ```

   对应实现代码

   ```java
   InterfaceTest1 interfaceTest1 = new InterfaceTest1();
   interfaceTest1.server("hello");
   ```

   

2. 创建类时时不确定接口类型,把实现类(interfaceTest2)当作泛型类

```java
public class InterfaceTest2<T> implements GenericInterface<T>{

    @Override
    public T server(T t) {
        System.out.println(t);
        return t;
    }
}
```

对应实现代码：

```java
InterfaceTest2<Integer> test2 = new InterfaceTest2<>();
test2.server(1000);
```



### 泛型方法

1. 创建

   ```java
   public class MyGenericMethod {
       public <T> void show(T t){
           System.out.println("泛型方法" + t);
       }
   }
   ```

2. 引用

   ```java
   MyGenericMethod myGenericMethod = new MyGenericMethod();
   myGenericMethod.show("fanxing");//t的类型由show里面的数据直接决定
   myGenericMethod.show(200);
   myGenericMethod.show(3.33);
   ```

### 泛型集合

- 概念：参数化类型、类型安全的集合，强制集合元素的类型必须一致
- 特点：
  - 编译时即可检查，而非运行时抛出异常
  - 访问时，不必类型转换（拆箱）
  - 不同泛型之间引用不能相互赋值，泛型不存在多态

- 注意：

  之前我们在创建LinkedList类型对象的时候并没有使用泛型，但是进到它的源码中会发现：

```
COPYpublic class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable{//略}
```

​	它是一个泛型类，而我之前使用的时候并没有传递，说明java语法是	允许的，这个时候传递的类型是**Object类**，虽然它是所有类的父类，	可以存储任意的类型，但是在遍历、获取元素时需要原来的类型就要	进行**强制转换**。这个时候就会出现一些问题，假如往链表里存储了许	多不同类型的数据，在强转的时候就要**判断**每一个原来的类型，这样	就很容易**出现错误**。



# Set子接口

- 特点：无序、无下标、元素不可重复
- 方法：全部继承自Collection中的方法

# Set实现类

- HashSet（重点，基于哈希表实现）：
  - 基于 HashCode 实现元素不重复
  - 当存入元素的哈希码相同时，会调用 equals 进行确认，如果结果为 true ，则拒绝后者存入
- TreeSet （二叉树实现）：
  - 基于排列顺序实现元素不重复
- Set接口的常用：
  - add()
  - remove()
  - 遍历：
    - foreach
    - Iterator
  - 判断：
    - contains()
    - isEmpty()

## HashSet(重点)

- 基于 HashCode 计算元素存放位置
- 存储结构：数组+链表+红黑树
- 存储过程：
  - （1）根据 hashcode 计算保存的位置，如果此位置为空，则直接保存，不为空，执行第二部
  - （2） 再执行equals() 方法，如果equals方法为true,则认为是重复，否则，形成链表

```java 
//实现类
package com.joker.collection;

import java.util.HashSet;
import java.util.Iterator;

/**
 * HashSet
 * 存储结构: (数组+链表+红黑树)
 */
public class Demo09 {
    public static void main(String[] args) {
        HashSet<Person> persons =  new HashSet<>();

        Person person1 = new Person("apple", 10);
        Person person2 = new Person("orange", 20);
        Person person3 = new Person("durian", 30);

        persons.add(person1);
        persons.add(person2);
        persons.add(person3);
//        persons.add(person1);//重复，添加不了

        persons.add(new Person("apple",10));//是可以添加的，如果要改成不可以添加，就要重写 equals()和hashCode()
        System.out.println("sum: " + persons.size());
        System.out.println(persons.toString());

        //删除
//        persons.remove(person1);

        //遍历
        //foreach
        for (Person person :
                persons) {
            System.out.println(person);
        }

        //Iterator
        System.out.println("====Iterator");
        Iterator<Person> iterator =persons.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //判断
        System.out.println(persons.contains(person1));


    }
}
```

```java
//Person类
package com.joker.collection;

import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

## TreeSet

- 基于排列顺序实现元素不重复
- 实现了SortedSet接口，对集合元素自动排序
- 元素对象的类型必须实现Comparable接口，指定排序规则;Comparble接口里只有一个comareTo()方法。
- 通过CompareTo（）方法确定是否为重复元素；若返回值是0，则认为是重复，不可添加。
- 存储结构：红黑树，是一个二叉查找树

### TreeSet常用实现

```java
package com.joker.collection;

import java.util.TreeSet;

/**
 * TreeSet
 * 存储结构：红黑树
 * 要求：元素类型必须实现 Comparable接口，compareTo()方法返回值是0，则认为是重复元素，不可添加。
 */
public class Demo10_TreeSet {
    public static void main(String[] args) {
        //创建集合
        TreeSet<Person> persons = new TreeSet<>();

        Person person1 = new Person("apple", 10);
        Person person2 = new Person("orange", 20);
        Person person3 = new Person("durian", 30);

        //添加
        persons.add(person1);
        persons.add(person2);
        persons.add(person3);
//要重写compareTo(),确定比较对象
        System.out.println("sum: "+persons.size());
        System.out.println(persons.toString());

//        //2.删除元素
//        persons.remove(p1);
//        persons.remove(new Person("apple", 10));//因为重写了compareTo()方法所以可是这样写
//        System.out.println(persons.toString());
        //3.遍历（略）
        //4.判断
        System.out.println(persons.contains(new Person("apple", 10)));


    }
}
```

Person类

```java
public class Person implements Comparable<Person>{
    ---------此处省略其他代码块------
        //先按姓名比，再按年龄比较
    @Override
    public int compareTo(Person o) {
        int n1  = this.getName().compareTo(o.getName());
        int n2  = this.getAge() - o.getAge();
        return n1 == 0 ? n2:n1;
    }
        
}
```

### Comparator(比较器)

- 除了实现 Comparable 接口里的比较方法，TreeSet 也提供了一个带比较器 Comparator 的构造方法，使用匿名内部类来实现它

```java
/**
 * TreeSet集合的使用
 * Comparator:实现定制比较（比较器）
 * Comparable:可比较的
 * @author joker_chen
 *

 */
public class Demo11 {
    public static void main(String[] args) {
        //创建集合，并使用Comparator指定比较规则，可以不用实现Comparable接口;使用匿名内部类来实现
        TreeSet<Person> persons = new TreeSet<>(new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                //先比年龄，后比名字
                int n1 = o1.getAge() - o2.getAge();
                int n2 = o1.getName().compareTo(o2.getName());
                return n1 == 0 ? n2:n1;
            }
        });

        Person person1 = new Person("apple", 10);
        Person person2 = new Person("orange", 20);
        Person person3 = new Person("durian", 30);
        Person person4 = new Person("egg", 30);

        persons.add(person1);
        persons.add(person2);
        persons.add(person3);
        persons.add(person4);//根据以上规则，相同年龄就按名字排序

        System.out.println("sum: "+persons.size());
        System.out.println(persons.toString());


    }
}
```

### TreeSet案例

- 要求：使用TreeSet集合，实现字符串按照长度进行排序

```java
/**
 * TreeSet案例
 * 要求:使用TreeSet集合，实现字符串按照长度进行排序
 * Comparator接口实现定制比较
 * @author joker_chen
 */
public class Demo12 {
    public static void main(String[] args) {
        TreeSet<String> stringTreeSet = new TreeSet<>(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                int r1 = o1.length() - o2.length();
                int r2 = o1.compareTo(o2);//用原来的比较方式

                return r1 == 0 ? r2:r1;//先用长度比较，长度相同，则用原来的比较方式，
            }
        });

        stringTreeSet.add("zhao");
        stringTreeSet.add("qian");
        stringTreeSet.add("li");
        stringTreeSet.add("sun");
        stringTreeSet.add("zheng");
        stringTreeSet.add("kkkkkkkkk");

        System.out.println(stringTreeSet);

    }
}
```

# Map集合

![Map](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/map.png)

- 特点：存储键值对，
  - 键：无序，无下标，不允许重复
  - 值：无序，无下标，允许重复
- 常用方法：
  - V put(K key,V value)`//将对象存入到集合中，关联键值。key重复则覆盖原值。
  - Object get(Object key)`//根据键获取相应的值
  - Set<K>`//返回所有的key
  - Collection<V> values() //返回包含所有值的Collection集合`
  - Set<Map.Entry<K,V>>`//键值匹配的set集合

```java
/**
 * Map接口的使用
 * 特点：
 * （1）存储键值对
 * （2）键不能重复，值可以重复
 * （3）无序
 * @author joker_chen
 */
public class Demo13_Map {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        //1. put(),添加元素
        map.put("apple","苹果");
        map.put("orange","橘子");
        map.put("durian","榴莲");
        map.put("apple","pingguo");//当键值相同时，后添加的会把以前添加的键值替换

        System.out.println("sum: " + map.size());
        System.out.println(map);

//        //remove() 删除
//        map.remove("apple");
//        System.out.println(map.size());
//        System.out.println(map);

        //遍历
        //3.1 使用keySet(),一个Set集合，只包含键值key
        System.out.println("============keySet()===========");
//        Set<String> keySet = map.keySet();
        for (String key :
                map.keySet()) {
            System.out.println(key + ":" + map.get(key));
        }
        //3.2 使用entrySet(),返回的是一个包含entry的Set集合，一个entry 就是一个键值对key-value
        //entrySet()效率高于keySet()
        System.out.println("============entrySet()==========");
        Set<Map.Entry<String, String>> entries = map.entrySet();
        for (Map.Entry<String, String> entry:
        entries){
            System.out.println(entry.getKey() + "-" + entry.getValue());

        }
        //判断
        System.out.println(map.containsKey("apple"));
        System.out.println(map.containsValue("榴莲"));

    }
}

```

# Map集合的实现类 

## HashMap(重点)

- 线程不安全，运行效率快；
- 允许用 null 作为 key 或是 value 。

```java
/**
 * HashMap集合的使用
 * 存储结构：哈希表（数组+链表+红黑树）
 * 使用key的hashCode()和equals()作为重复的判断依据:重写key类型的 hashCode()和 equals()
 * @author joker_chen
 *
 */
public class Demo02 {
    public static void main(String[] args) {
        //创建集合
        HashMap<Student_Map, String> students = new HashMap<>();

        Student_Map stu1 = new Student_Map("zhao",111);
        Student_Map stu2 = new Student_Map("qian",222);
        Student_Map stu3 = new Student_Map("sun",333);
//添加元素，put()
        students.put(stu1,"chongqing");
        students.put(stu2,"chengdu");
        students.put(stu3,"kunming");

        students.put(new Student_Map("zhao",111),"sichuan");//这个是可以加进来的，
        // 为了当姓名学号都相同时，加不进来，就要重写hashCode(),equals()

        System.out.println("the number of elements: " + students.size());
        System.out.println(students.toString());
//        //2.删除元素
//        students.remove(stu1);
//        System.out.println(students.toString());
        //3.遍历
        //3.1 使用keySet()遍历
        for (Student_Map key : students.keySet()) {
            System.out.println(key+" "+students.get(key));
        }
        //3.2 使用entrySet()遍历
        for (Map.Entry<Student_Map, String> entry : students.entrySet()) {
            System.out.println(entry.getKey()+" "+entry.getValue());
        }
        //4.判断
        //
        System.out.println(students.containsKey(new Student_Map("qian", 222)));
        System.out.println(students.containsValue("sichuan"));

    }
}
```

```java
public class Student_Map {

    private String name;
    private int stuNo;

    public Student_Map(){

    }

    public Student_Map(String name, int stuNo) {
        this.name = name;
        this.stuNo = stuNo;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getStuNo() {
        return stuNo;
    }

    public void setStuNo(int stuNo) {
        this.stuNo = stuNo;
    }

    @Override
    public String toString() {
        return "Student_Map{" +
                "name='" + name + '\'' +
                ", stuNo=" + stuNo +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student_Map that = (Student_Map) o;
        return stuNo == that.stuNo && Objects.equals(name, that.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, stuNo);
    }
}
```

## HashMap源码分析

### 基本属性

- ```
  //默认容量
  static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
  ```

- 默认加载因子：

  ```java
  static final float DEFAULT_LOAD_FACTOR = 0.75f;
  //0.75：假设容量是100，达到75时，就要扩容了
  ```

- 

- 链表调整为红黑树的链表长度阈值（JDK1.8）：`static final int TREEIFY_THRESHOLD = 8;`

- 红黑树调整为链表的链表长度阈值（JDK1.8）：`static final int UNTREEIFY_THRESHOLD = 6;`

- 链表调整为红黑树的数组最小阈值（JDK1.8）：`static final int MIN_TREEIFY_CAPACITY = 64;`当链表长度大于8，并且集合元素个数大于等于64，调整成红黑树

- HashMap存储的数组：`transient Node<K,V>[] table;`

- HashMap存储的元素个数：`transient int size;`

### 方法

1. 无参构造方法：hashMap()

   - 注：刚创建HashMap的时候,没有分配元素的时候，table = null,size = 0。目的：节省空间

2. put()

   - 每次扩容都是扩到原来的2倍

   ```java
   public V put(K key, V value) {
           return putVal(hash(key), key, value, false, true);
       }
   ```

### 总结

1. 刚创建HashMap的时候,没有分配元素的时候，table = null,size = 0。目的：节省空间。当添加第一个元素后，table容量调整为16
2. 当元素个数大于阈值（16*0.75 = 12）时，会进行扩容，扩容后大小时原来的2倍，目的：减少调整元素的个数
3. jdk1.8 后，当每个链表长度大于8，且，数组元素个数大于等于64时，链表会调整为红黑树，目的：提高执行效率
4. jdk1.8后，当红黑树长度小于6时，调整成链表
5. jdk1.8以前，链表是头插入，jdk1.8 以后，链表是尾插入
6. HashSet用的就是HashMap的东西，看源码

## HashTable(不咋用)

- 不允许null 作为key 或者是 value
- 线程安全，所以操作效率低

## Properties

- HashTable的子类，用的还是很多，与流密切相关

## TreeMap

- 实现了SortedMap接口（是Map的子接口），可以对key 自动排序。
- 存储结构：红黑树
- 判定重复依据方式有两种：
  - （1）对象类 Student_Map 继承Comparable接口，重写comparableTo() 方法
  - （2）使用定制比较，Comparator

```java
public class Demo03 {
    public static void main(String[] args) {
        TreeMap<Student_Map, String> students = new TreeMap<>();

        Student_Map stu1 = new Student_Map("zhao",111);
        Student_Map stu2 = new Student_Map("qian",222);
        Student_Map stu3 = new Student_Map("sun",333);
//添加元素，put(),要Student_Map类要实现Comparable接口，
        students.put(stu1,"chongqing");
        students.put(stu2,"chengdu");
        students.put(stu3,"kunming");

        System.out.println(students);

        //2.删除元素
        students.remove(new Student_Map("zhao", 111));
        System.out.println(students.toString());
        //3.遍历
        //3.1 使用keySet()
        for (Student_Map key : students.keySet()) {
            System.out.println(key+" "+students.get(key));
        }
        //3.2 使用entrySet()
        for (Map.Entry<Student_Map, String> entry : students.entrySet()) {
            System.out.println(entry.getKey()+" "+entry.getValue());
        }
        //4.判断
        System.out.println(students.containsKey(stu1));
        System.out.println(students.isEmpty());
    }
}
```

- 看源码可知，TreeSet 存储结构就是 TreeMap。它的 add 方法调用的就是 TreeMap 的 put 方法，将元素作为 key 传入到存储结构中。

# Collections(工具类)

- 概念：集合工具类，定义了除了存取以外的集合常用方法
- 常用的方法：
  - public static void reverse(List<?> list)`//反转集合中元素的顺序
  - public static void shuffle(List<?> list)`//随机重置集合元素的顺序
  - public static void sort(List<T> list)`//升序排序（元素类型必须实现Comparable接口）

```java
/**
 * Collections工具类的使用
 * @author joker_chen
 *
 */
public class Demo01 {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(20);
        list.add(10);
        list.add(30);
        list.add(44);
        list.add(66);

        //sort排序
        System.out.println("before sort: "  + list );
        Collections.sort(list);
        System.out.println("after sort:" + list);

        //bianrySearch()二分查找,排序之后才可以用
        int i = Collections.binarySearch(list,30);
        System.out.println(i);

        //copy()复制
        List<Integer> dest = new ArrayList<>();
        for (int k = 0;k < list.size();k++){
            dest.add(0);//使dest的空间大小与List一样
        }
        Collections.copy(dest,list);//要注意list，dest空间大小关系
        System.out.println(dest);

        //reverse() 反转
        Collections.reverse(list);
        System.out.println("reverse："+list);

        //shuffle() 打乱
        Collections.shuffle(list);
        System.out.println("shuffle: "+list );

        //补充，list转成数组
        Integer[] arr = list.toArray(new Integer[0]);//参数为0，是因为当参数小于list的长度时。arr的长度将于list 保持一致
//        当参数超过list 的长度时，arr的长度与参数一致，多余的为null
        System.out.println(arr.length);
        System.out.println(Arrays.toString(arr));

        //数组转成集合
        String[] names = {"zhao","qian","sun","li"};
        //转成后的集合，是一个受限集合，不能添加和删除，因为，数组本身长度时固定的
        List<String> list2 =  Arrays.asList(names);
        System.out.println(list2.toString());

        //把基本类型数组抓换成集合时，需要修改为包装类
        Integer[] nums = {1,2,3,4,5,6};//不能用int[]
        List<Integer>  list3 = Arrays.asList(nums);
        System.out.println(list3);
    }
}

```

