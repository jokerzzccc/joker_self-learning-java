# Day05

# 集合（Collection）

## 概念

1. 定义：	对象的容器，定义 了对多个对象进行操作的常用方法。可实现数组的功能

2. 集合与数组的区别：

   - 数组长度固定，集合长度不固定
   - 数组可以存储基本类型和引用类型，集合只能存储引用类型

3. 位置：java.util.*

4. 结构图：

   ![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/collection.png)

5. 常用方法，查看jdk文档

   - boolean add(abject obj)
   - boolean addAll(Collection c)
   - void clear()
   - boolean contains(Object o)
   - boolean equals(Object o)
   - boolean isEmpty()
   - boolean remove(Object o)
   - int size()返回此集合的元素个数
   - Object[] toArray()将此集合转换为数组
   - Iterator() 迭代器

## 使用

1. collection.add():添加

2. collection.remove()删除

3. collection.clear() 清空

4. 遍历元素，重点：

   - 4.1用增强for，即for-each,不能用for因为集合不一定有下标
   - 4.2用迭代器Iterator（专门设计来遍历集合的一种方式）

   ```java
   Iterator iterator = collection.iterator();
           while (iterator.hasNext()){
               Object str = iterator.next();
               System.out.println(str);
               //使用迭代器时，不可同时使用collection.remove(),但可以是使用iterator.remove();
               iterator.remove();
           }
   ```

5. collection.contains(),判断是否包含某一个对象

6. collection.isEmpty(),判断是否为空

# List子接口

1. 特点：有序、有下标、元素可以重复
2. 方法：继承了collection的所有方法，也有自己的独自方法
   - void add(int index, Object o)//在index位置插入对象o，
   - boolean addAll(int index, Collection c)//将一个集合中的元素，添加到此集合的index之后。
   - Object get(int index)//返回集合中指定位置的元素
   - List subList(int fromIndex, int toIndex)//返回区间里的集合元素

## List的使用

1. 新建

   ```java 
   List list = new Arraylist();
   ```

2. list.add()

3. list.remove()

4. 遍历：

   - 4.1 使用for,因为List有下标
   - 4.2 使用foreach
   - 4.3 使用Iterator
   - 4.4 （重点）使用ListIterator:与Iterator的区别，可以向前向后遍历元素，增加，删除，修改元素

5. 判断

   - contains(),是否存在某对象
   - isEmpty() 判空

6. list.indexOf(object) 由对象获取下标

进阶：

1. 添加基本类型，比如数字时，进行了自动装箱：即把基本类型转换成了包装类，
2. 删除remove(),直接写数字，是根据下标来删除，不是根据元素，但数字可以强制转换为对象
3. subList()方法，返回子集合，含头不含尾

# List实现类

## ArrayList（重点）

1. 动态数组结构实现，查询快、增删慢；
2. jdk1.2,运行效率快，线程不安全
3. 必须开辟连续空间

### ArrayList的使用

1. 添加
2. 删除
3. 遍历
4. 判断
5. 查找

### ArrayList源码分析

1. 默认容量：DEFAULT_CAPACITY = 10
   - 如果集合中没有添加任何元素，容量是0；添加一个元素之后，容量是10；达到10个之后，每次扩容大小，是原来的1.5倍
2. 存放元素的数组：elementData
3. size常量,实际元素的个数，一定 <= 默认容量
4. add() 源码分析



## Vector(不咋用)

- 数组结构实现，查询快、增删慢
- jdk1.0版本，运行效率慢、线程安全

### Vector的使用

1. add()

2. remove()

3. 遍历，使用枚举器Enumeration

4. 其它差不多

5. 。。。

   

## LinkedList

- 双向链表结构实现，增删快，查询慢,无需开辟连续空间

### LinkedList的使用

1. add()
2. remove()
3. clear() 清空
4. 遍历
   - for遍历
   - foreach
   - 迭代器 Iterator
   - 列表迭代器ListIterator
5. 判断：存在否，contains()
6. isEmpty()判空
7. indexOf() 获取对象下标位置

### LinkedList源码分析

1. size,集合大小
2. first,指向头节点
3. last，指向尾节点
4. add(),
5. linkLast()
6. Node()