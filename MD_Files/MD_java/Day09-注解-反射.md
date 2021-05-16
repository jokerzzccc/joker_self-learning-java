

# 注解与反射

# 类对象

- 类的对象：基于某个类 new 出来的对象，也成为实例对象

- **类对象**：类加载的产物，封装了一个类的所有信息（类名、父类、接口、属性、方法、构造方法）。即一个类被加载后整个类的机构都会被封装在类对象中

  - 就是把类当成对象
  - 每个类加载到内存后都对应一个 Class 对象，
  - 每个类有且只有一个 Class 对象

- -verbose:class 显示类的加载过程。
  
  - 在idea 里：run->edit configurations->vm option->输入-verbose:class
  
- 有哪些类型可以有Class 对象

  - class :外部类，成员（成员内部类，静态内部类），局部内部类，匿名内部类
  - interface 
  - [] 数组
  - enum 枚举
  - annotation 注解@interface
  - primitive type 基本数据类型
  - void

- ```java
  /**
   * 所有类型的Class
   * /只要元素类型与维度一样，就是同一个Class
   */
  public class Test {
      public static void main(String[] args) {
          Class c1 = Object.class;//类
          Class c2 = Comparable.class;//接口
          Class c3 = String[].class;//一维数组
          Class c4 = int[][].class;//二维数组
          Class c5 = Override.class;//注解
          Class c6 = ElementType.class;//枚举
          Class c7 = Integer.class;//基本数据类型
          Class c8 = void.class;//void
          Class c9 = Class.class;//Class
  
          System.out.println(c1);
          System.out.println(c2);
          System.out.println(c3);
          System.out.println(c4);
          System.out.println(c5);
          System.out.println(c6);
          System.out.println(c7);
          System.out.println(c8);
          System.out.println(c9);
  
          //只要元素类型与维度一样，就是同一个类
          int[] a = new int[10];
          int[] b = new int[100];
          System.out.println(a.getClass().hashCode());
          System.out.println(b.getClass().hashCode());
      }
  }
  ```

- 

## 获取类对象

- 有三种基本方式

- 方式1：通过类的对象，获取类对象

- ```java
  Student stu = new Student();
  Class c = stu.getClass();
  ```

- 方式2： 

- ```java
  Class c = 类名.class;
  ```

- 方式3： 通过静态方法获取类对象；**推荐使用**，因为是传递字符串，编译的时候包可以不存在，只要在运行的时候有就行了。通用性更强，前两种依赖性太强

- ```java
  Class c = Class.forName("包名.类名");
  ```

- 特殊的，
  - 方式4：基本内置类型的包装类都有一个TYPE 属性
  - 方式5：还可以利用 ClassLoader

获取示例：

```java
package com.joker.day09;

public class Demo01 {
    public static void main(String[] args) throws ClassNotFoundException {
        getClazz();
    }
    public static void getClazz() throws ClassNotFoundException {
        //1.使用类的对象获取类对象
        Person person = new Person();
        Class class1  = person.getClass();
        System.out.println(class1.hashCode());//获取类对象的地址

        //2 通过类名获取类对象，类名.class
        Class class2 = Person.class;
        System.out.println(class2.hashCode());

        //3 使用 Class 的静态方法,推荐使用
        Class<?> class3 = Class.forName("com.joker.day09.Person");
        System.out.println(class3.hashCode());
        //4 基本内置类型的 TYPE 属性
        Class class4 = Integer.TYPE;
        

    }
}
```

## java 内存分析

- Java 内存：
- 1. 堆：
     1. 存放new 的对象和数组
     2. 可以被所有的线程共享，不会存放别的对象引用
  2. 栈：
     1. 存放基本变量类型（会包含这个基本类型的具体数值）
     2. 引用对象的变量（会存放这个引用在堆里面的具体地址）
  3. 方法区：（也属于堆，但jdk1.8 之后就不属于堆了）
     1. 可以被所有的线程共享
     2. 包含了所有的 class 和 static 变量

## 类的加载过程

- 当程序主动使用某个类时，如果该类还未被加载到内存，则系统会通过如下**三个步骤**来堆该类进行进行**初始化**：
  - 1 类的**加载**（Load）：将类的Class 文件读入内存，并为之创建一个 java.lang.Class 对象。此过程由**类加载器**完成
  - 2 类的**链接**（Link）：将类的二进制数据合并到 JRE 中
    - 准备：正式为类变量（static）分配内存，并设置内变量默认初始值的阶段，这些内存都将在方法区中分配。
  - 3 类的**初始化**（Initialize）：JVM 负责对类进行初始化

### 什么时候会发生类的初始化

1. 类的主动引用（一定会发生类的初始化）：
   - 当虚拟机启动时，先初始化main 方法所在的类
   - new 一个类的对象
   - 调用类的静态成员（除了 final 常量）和静态方法
   - 使用 java.lang.reflect 包的方法对类进行反射调用
   - 当初始化一个类，如果其父类没有被初始化，则会先初始化它的父类
   - 
2. 类的被动引用（不会发生类的初始化）：
   - 当访问一个静态域时，只有真正声明这个域的类 才会被初始化。如：当通过子类引用父类的静态变量，不会导致子类初始化
   - 通过数组定义类引用，不会触发此类的初始化
   - 引用常量不会触发此类的初始化（常量在链接阶段就存入调用类的常量池中了）

### 类加载器的作用

- 类加载器的作用：将 class 文件字节码内容加载到内存中，并将这些静态数据转换成 方法区的运行时数据结构，然后在堆中生成一个代表这个类的 java.lang.Class 对象，作为方法区中类数据的访问入口。
- 类缓存：标准的JavaSE 类加载器可以按要求查找类，但一旦被某个类加载到类加载器中，它将维持加载（缓存）一段时间。不过 JVM 垃圾回收机制可以回收这些Class 对象

#### 类加载器的种类

- 有三种

  1. 引导类加载器（Bootstap Classloader）：用c++ 编写，是 JVM 自带的类加载器，负责Java平台核心库（rt.jar）,用来装载核心类库。该加载器无法直接获取。获取会返回null
  2. 扩展类加载器(Extension Classloader)：负责jre/lib/ext目录下的jar 包或-D java.ext.libs 指定目录下的jar包装入工作库
  3. 系统类加载器(System Classloader 或 AppClassLoader(用户加载器))：负责java -classpath 或 -D java.class.path （即自己的项目包）所指的目录下的类与 jar 包装入工作，是**最常用**的加载器。

- 类加载的获取：

- ```java
  /**
   * 获取 类加载器
   */
  public class Test2 {
      public static void main(String[] args) {
          //获得系统类加载器
          ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
          System.out.println(
                  systemClassLoader
          );
          //获得系统类加载器的父类，扩展类加载器
          ClassLoader parent = systemClassLoader.getParent();
          System.out.println(parent);
          //获得扩展类加载器的父类，根加载器（c/c++编写）
          ClassLoader parent1 = parent.getParent();
          System.out.println(parent1);
      }
  }
  ```

- 

# 反射 Reflection

## 动态语言

- 是一类在运行时可以改变其结构的语言：例如新的函数、对象，甚至代码可以引进，已有的函数可以被删除或是其他结构上的变化。通俗说，就是在运行时，代码可以根据某些条件改变自身结构
- 主要动态语言：Object-C, C# ,javaScript，PHP, Python等。

## 静态语言

- 与动态语言相对应的，运行时结构不可改变的语言就是静态语言。如：java,C, C++
- Java 不是动态语言，但Java 可以称为“准动态语言”。即Java 有一定的动态性，可以利用**反射机制**获得类似动态语言的特性。Java 的动态性让编程更加灵活

## 反射的概念

- 反射是Reflection 是Java动态的关键，反射机制允许程序在执行时借助于ReflectionAPI 取得任何类的内部信息，并能直接操作任意对象的内部属性及方法

- 加载完类之后，在堆内存的方法区 就产生了一个 Class 类型的对象，这个类对象包含了完整的类的结构信息。我们可以通过类对象看到类的结构。类对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象称之为：**反射**

- Java 反射机制提供的**功能**：

  - 在运行时 判断任意一个对象所属的类

  - 在运行时构造任意一个类对象

  - 在运行时判断任意一个类所具有的成员变量和方法

  - 在运行时获取泛型信息

  - 在运行时调用任意一个对象的成员变量和方法

  - 在运行时处理注解

  - 生成**动态代理**

  - ...

    

- 反射**优缺点**：
  - 优点：可以实现动态创建对象和编译，体现除很大的灵活性
  - 缺点：对性能有影响。会慢很多。使用反射基本上是一种解释操作，我们可以告诉 JVM ,我们希望做什么并且它满足我们的要求。这类操作总是慢于执行相同的操作

## 反射的常用方法

- `public String getName()`

  获取类对象所代表的类的名字(包名+类名)。

- public String getSimpleName()

  获取类名（只有类名）

- `public Package getPackage()`

  获取类对象所代表的包的名字。

- `public Class<? super T> getSuperclass()`

  获取类对象所代表的类的父类。

- `public Class<?>[] getInterfaces()`

  获取类对象所代表的类或接口实现的接口。

- `public Constructors<?>[] getConstructors()`

  获取类对象所代表的类的所有公共构造方法。

- `public T newInstance()`

  创建类对象所代表的类一个新实例。

- `public Method[] getMethods()`

  获取类对象所代表的类或接口的公共成员方法。

- `public Field[] getFields()`

  获取类对象所代表的类或接口的公共访问字段。

## 反射的常见操作

- 创建类 Person

```java
public class Person {
    String name;
    int age;
    public Person(){
        System.out.println("无参构造执行");
    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("带参构造执行");
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

### 操作一：获取类的名字、包名、父类、接口

```java
    
public class Demo01 {
    public static void main(String[] args) throws Exception {
        reflectOpe1();
    }
//1 使用反射获取类的名字、包名、父类、接口
    public static void reflectOpe1() throws Exception{
        Class<?> class1 = Class.forName("com.joker.day09.Person");
        System.out.println(class1.getName());//获取类的名字
        System.out.println(class1.getPackage());//获取包名
        System.out.println(class1.getSuperclass());//获取父类名
        System.out.println(Arrays.toString(class1.getInterfaces()));//获取接口名

    }
}
```



### 操作二：获取类的构造方法，创建对象

- 使用反射获取类的构造方法，创建对象
- getConstructors()
- getConstructor()
- newInstance()
  - 类必须有一个无参数的构造器
  - 类的构造器的访问权限需要足够

```java
    public static void main(String[] args) throws Exception {
//        getClazz();
        reflectOpe2();
    }

//2 使用反射获取类的构造方法，创建对象
    public static void reflectOpe2() throws Exception{
        //1 获取类的类对象
        Class<?> class1 = Class.forName("com.joker.day09.Person");
        //2 获取类的构造方法
        Constructor<?>[] constructors = class1.getConstructors();

        for (Constructor<?> cons :
                constructors) {
            System.out.println(cons.toString());
        }
        //3 获取类中的无参构造
        Constructor<?> constructor = class1.getConstructor();
        Person person1 =(Person) constructor.newInstance();
        System.out.println(person1.toString());
        //简便方法：类对象.newInstance()
        Person person2 =(Person) class1.newInstance();//已经废弃
        System.out.println(person2.toString());

        //4 获取类带参构造
        Constructor<?> constructor1 = class1.getConstructor(String.class, int.class);
        Person person3 =(Person) constructor1.newInstance("joker", 20);
        System.out.println(person3.toString());

    }
}
```

### 操作三：获取类中的方法，并调用

- 使用反射获取类中的方法，并调用方法
  - 调用方法：  public Object invoke(Object obj, Object... args)
  - 第一个参数是调用这个方法的该类的对象，第二个是形参列表，有就写，没有就不写
- 常用的获取方法的方法：
  - getMethods()
  - getDeclardMethods()
  - getMethod()
  - getDeclaredMethod()
- 

```java
    //3 使用反射获取类中的方法，并调用方法
    public static void reflectOpe3() throws Exception{
        //1 获取类对象
        //2 获取方法， Method 对象
        Class<?> class1 = Class.forName("com.joker.day09.Person");
        //2.1 getMethos() 只能获取公开的方法，包括从父类继承的方法
//        Method[] methods = class1.getMethods();
        //2.2 getDeclaredMethods() 获取类中的所有方法，包括私有、默认、保护的方法，不包含继承的方法
//        Method[] declaredMethods = class1.getDeclaredMethods();
//        for (Method method :
//                declaredMethods) {
//            System.out.println(method.toString());
//        }
        //3 获取单个方法
        //3.1 eat()
        Method eatMethod = class1.getMethod("eat");
        //调用方法
        Person person1 =(Person) class1.newInstance();
        eatMethod.invoke(person1);
        System.out.println("==========");
        //3.2 toString()
        Method toStringMethod = class1.getMethod("toString");
        Object invoke = toStringMethod.invoke(person1);
        System.out.println(invoke);
        System.out.println("==========");
        //3.3 调用带参eat()
        Method eatMethod2 = class1.getMethod("eat", String.class);
        eatMethod2.invoke(person1, "炸鸡");
        //3.4 获取一个私有的方法 privateMethod()
        Method privateMethod = class1.getDeclaredMethod("privateMethod");
        //必须设置访问权限无效
        privateMethod.setAccessible(true);
        privateMethod.invoke(person1);
        //3.5 获取一个静态方法
        Method staticMethod = class1.getMethod("staticMethod");
        //因为正常调用静态方法是使用类名.方法，即Person.staticMethod(),所以对象就不用传递，直接写 null
        staticMethod.invoke(null);
    }
}
```

### 操作四： invokeAny()

- 使用反射实现一个可以调用任何对象的通用方法

```java
public class Demo01 {
    public static void main(String[] args) throws Exception {
        //正常调用方法
        Properties properties = new Properties();
//        properties.setProperty("username","joker");
//        System.out.println(properties.toString());
        //使用反射通用方法
        invokeAny(properties,"setProperty",new Class[]{String.class,String.class},"username","joker");
        System.out.println(properties.toString());
    }
    //4 使用反射实现一个可以调用任何对象的通用方法
    //obj:表示调用的对象，methodName:表示方法名，types: 方法的参数类，args:参数值

    public static Object invokeAny (Object obj,String methodName,Class<?>[] types,Object...args ) throws Exception{
        //获取类对象
        Class<?> class1 = obj.getClass();
        //获取调用方法
        Method method = class1.getMethod(methodName, types);
        //调用
        return method.invoke(obj,args);
    }
}
```

### 操作五：获取类中的字段

- 使用反射来获取类中的字段（属性）
- getFields()
- getDeclaredFields()
- getField()
- getDeclaredField()

```java
public class Demo01 {
    public static void main(String[] args) throws Exception {reflectOpe4();
    }
    //5 使用反射来获取类中的属性
    public static void reflectOpe4() throws Exception{
        //1 获取类对象
        Class<?> class1 = Class.forName("com.joker.day09.Person");
        //2 读取字段（属性）
        //getFields() 只能获取公开的字段，父类继承的字段
//        Field[] fields = class1.getFields();
        //getDeclaredFields() 可以获取所有的属性，包括私有，保护，默认，不包括继承的
        Field[] declaredFields = class1.getDeclaredFields();
        System.out.println(declaredFields.length);
        for (Field field:declaredFields
             ) {
            System.out.println(field.toString());
        }

        //3 只获取 name 属性
        Field name = class1.getDeclaredField("name");
        //3.1获取私有属性的时候，必须设置权限
        Field age = class1.getDeclaredField("age");
        age.setAccessible(true);

        //4 赋值
        Person person1 = (Person) class1.newInstance();
        name.set(person1,"joker");
        age.set(person1,20);
        //5 获取值
        System.out.println(name.get(person1));
        System.out.println(age.get(person1));
    }
}
```

### 反射获取泛型信息

- java 采用泛型擦除 机制来引入泛型，Java中的泛型仅仅是个编译器 javac 使用的，确保数据的安全性和免去强制类型转换问题，但是，一旦编译完成，所有和泛型有关的类型全部擦除
- 为了通过反射操作这些类型（泛型），Java 新增了 ParameterizedType, GenericArrayType , TypeVariable 和 WildcardType 几种类型来代表不能被归一到Class类中的类型，但是又和原始类型齐名的类型
- ParameterizedType ：表示一种参数化类型，比如Collection<String> 里的String
- GenericArrayType ：表示一种元素类型是参数化类型或者类型变量的数组类型
- TypeVariable：是各种类型变量的公共父接口
- WildcardType  ：代表一种通配符类型表达式

```java
/**
 * 反射获取泛型信息
 * 得到泛型信息后，可以给它增加约束，以及修改等
 */
public class Test3 {
    public void test01(Map<String,Person> map, List<Person> list){
        System.out.println("test01");
    }

    public Map<String,Person> test02(){
        System.out.println("test02");
        return null;
    }

    public static void main(String[] args) throws NoSuchMethodException {
        //返回参数类型的泛型
        Method method = Test3.class.getMethod("test01", Map.class, List.class);
        Type[] genericParameterTypes = method.getGenericParameterTypes();
        for (Type genericParameterType : genericParameterTypes) {
//            System.out.println(genericParameterType);
            if (genericParameterType instanceof ParameterizedType){//判断是不是参数化类型
                Type[] actualTypeArguments = ((ParameterizedType) genericParameterType).getActualTypeArguments();//获得真实的参数
                for (Type actualTypeArgument :
                        actualTypeArguments) {
                    System.out.println(actualTypeArgument);
                }
            }
        }

        //获取返回值类型的泛型
        Method method1 = Test3.class.getMethod("test02");
        Type genericReturnType = method1.getGenericReturnType();

        if (genericReturnType instanceof ParameterizedType){
            Type[] actualTypeArguments = ((ParameterizedType) genericReturnType).getActualTypeArguments();
            for (Type actualTypeArgument :
                    actualTypeArguments) {
                System.out.println(actualTypeArgument);
            }
        }
    }
}

```

### 反射获取注解信息

- 练习ORM (Object relationship Mapping)(对象关系映射)

- 要求：利用注解和反射完成类和表结构的映射关系

  - 类和表结构对应
  - 属性和字段对应
  - 对象和记录对应

- ```java
  /**
   * 发射获取注解信息
   */
  public class Test4 {
      public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
          Class<?> class1 = Class.forName("com.joker.reflection.Student2");
  
          //通过反射获取注解
          Annotation[] annotations = class1.getAnnotations();
          for (Annotation annotation : annotations) {
              System.out.println(annotation);
          }
          //获得注解 value 的值
          TableJoker tableJoker = class1.getAnnotation(TableJoker.class);
          String value = tableJoker.value();
          System.out.println(value);
  
          //获得类指定的注解,即获得指定类的指定属性的注解
          Field name = class1.getDeclaredField("id");
          FieldJoker annotation = name.getAnnotation(FieldJoker.class);
          System.out.println(annotation.columnName());
          System.out.println(annotation.type());
          System.out.println(annotation.length());
  
      }
  }
  @TableJoker("db_student")
  class Student2{
      @FieldJoker(columnName = "db_id",type = "int",length = 10)
      private int id;
      @FieldJoker(columnName = "db_age",type = "int",length = 10)
      private int age;
      @FieldJoker(columnName = "db_name",type = "varchar",length = 4)
      private String name;
  
      public Student2() {
      }
  
      public Student2(int id, int age, String name) {
          this.id = id;
          this.age = age;
          this.name = name;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public int getAge() {
          return age;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      @Override
      public String toString() {
          return "Student2{" +
                  "id=" + id +
                  ", age=" + age +
                  ", name='" + name + '\'' +
                  '}';
      }
  }
  
  //类名的注解
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.TYPE)
  @interface TableJoker{
      String value();
  }
  //属性的注解
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.FIELD)
  @interface FieldJoker{
      String columnName();
      String type();
      int length();
  }
  ```

- 

# 设计模式

- 定义：一套被反复使用、多数人知晓的、经过分类编目、代码设计经验的总结。简单理解：特定问题的固定解决方法。
- 好处：使用设计模式是为了可重用代码，让代码更容易被他人理解，保证代码可靠性、重用性。
- 有23种，这里只说两种

## 工厂设计模式

- 主要负责对象创建的问题
- 开发中有一个非常重要的原则“开闭原则”，对拓展开放，对修改关闭
- 可通过**反射**进行工厂模式的设计，完成动态的对象创建

一个案例

拥有的文件：

- ```
  public interface Usb 
  ```

- ```
  public class Mouse implements Usb
  ```

- ```
  public class Fan implements Usb
  ```

- ```
  public class Upan implements Usb
  ```

- ```
  public class UsbFactory//usb工厂类
  ```

- ```
  public class Demo01//客户程序，
  ```

```java
/**
 * 工厂设计模式
 *表示 父类产品
 */
public interface Usb {
    void service();
}
```

```java
    /**
     * Usb 接口实现类，即子类产品 鼠标 Mouse
     */
    public class Mouse implements Usb{
        @Override
        public void service() {
            System.out.println("鼠标开始工作");
        }
    }

```

```java
/**
 * Usb 接口实现类，风扇 Fan
 */
public class Fan implements Usb{
    @Override
    public void service() {
        System.out.println("风扇--开始工作");
    }
}
```

```java
/**
 * Usb 接口实现类 U盘 Upand
 */
public class Upan implements Usb{
    @Override
    public void service() {
        System.out.println("u盘---开始工作");
    }
}
```

```java
/**
 * 工厂类
 * 负责产品的具体创建工作
 */

public class UsbFactory {
	public static Usb createUsb(int type) {//type表示产品类型
		Usb usb=null;
		if (type==1) {//1表示鼠标
			usb=new Mouse();
		}else if(type==2){//2 键盘
			usb=new KeyBoard();
		}else if (type==3) {//3 U盘
			usb=new Upan();
		}
		return usb;		
	}
}
```

```java

public class Demo01 {
	public static void main(String[] args) {
		System.out.println("----选择购买：1 鼠标 2 风扇 3 U盘----");
		Scanner in=new Scanner(System.in);
		int type=in.nextInt();
		Usb usb=UsbFactory.createUsb(type);
		if(usb!=null) {
			System.out.println("购买成功");
			usb.service();
		}else {
			System.out.println("购买的产品不存在");
		}
	}
}
```

### 反射优化工厂模式

- 因为原工厂模式，如果每添加一产品，都需要在 UsbFactory 里添加，很麻烦，所以用反射来优化

拥有的文件：

- ```
  public interface Usb 
  ```

- ```
  public class Mouse implements Usb
  ```

- ```
  public class Fan implements Usb
  ```

- ```
  public class Upan implements Usb
  ```

- ```
  public class Keyboard implements Usb//新增产品
  ```

- ```
  public class UsbFactory//usb工厂类
  ```

- ```
  public class Demo01//客户程序，
  ```

- usb.properties

优化后：

**UsbFactory**

```java
/**
 * 工厂类
 * 负责产品的具体创建工作
 */
public class UsbFactory {
    public static Usb creatUsb(String type) throws ClassNotFoundException {//type 优化后表示产品类型的全名称 com.joker...
        Usb usb = null;
        //反射优化之后
        Class<?> class1 = Class.forName(type);
        try {
            usb =(Usb) class1.newInstance();
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

        return usb;
    }
}
```

**Demo01**

```java
/**
 * 客户程序
 * 住如果每添加一产品，都需要在 UsbFactory 里添加，很麻烦，所以用反射来优化
 */
public class Demo01 {
    public static void main(String[] args) throws Exception
    {
        System.out.println("----选择购买：1 鼠标 2 风扇 3 U盘----");
        Scanner input = new Scanner(System.in);
        String choice = input.next();
        //1 =com.joker.designPattern.Mouse
        //2 =com.joker.designPattern.Fan
        //3 =com.joker.designPattern.Upan
        //4 =com.joker.designPattern.Keyboard
        //创建一个 properties 文件后
        Properties properties = new Properties();
        FileInputStream fis = new FileInputStream("src/com/joker/designPattern/usb.properties");
        properties.load(fis);
        fis.close();

        Usb usb = UsbFactory.creatUsb(properties.getProperty(choice));
        if (usb != null ){
            System.out.println("购买成功");
            usb.service();
        }else {
            System.out.println("购买的产品不存在");
        }
    }
}
```

**usb.properties**

```java
1 =com.joker.designPattern.Mouse
2 =com.joker.designPattern.Fan
3 =com.joker.designPattern.Upan
4 =com.joker.designPattern.Keyboard
```

## 单例设计模式

- 单例（Singleton ）：只允许创建一个该类的对象
- 方式1：**饿汉式**（类加载时创建，天生线程安全）
  - 步骤：
  - 1. 首先创建一个常量
    2. 构造方法改成私有的，类外部不能创建对象
    3. 通过一个公开的方法，返回这个对象
- 方式2：**懒汉式**（使用时创建，线程不安全，加同步-> 保证线程安全）
  - 步骤:
  - 1. 首先创建一个对象，赋值为 null
    2. 构造方法改成私有的，类外部不能创建对象
    3. 通过一个公开的方法，返回这个对象
- 方式3：**静态内部类写法**。也是一种懒汉式，（使用时创建，线程安全）
- **总结**：前两种方式的优缺点
  - 饿汉式：
    - 优点：线程安全
    - 缺点：生命周期太长，浪费空间。因为不用它，一进来，也已经实例化了，
  - 懒汉式：
    - 优点：生命周期短，节省空间
    - 缺点：有线程安全问题

### 饿汉式

```java
/**
 * 饿汉式单例
 * 1. 首先创建一个常量
 * 2. 构造方法改成私有的，类外部不能创建对象
 * 3. 通过一个公开的方法，返回这个对象
 */
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton(){}
    public static Singleton getInstance(){
        return instance;
    }
}
```

```java
/**
 * 多线程来测试饿汉式单例
 * 测试结果三个线程都是拿到了同一个对象
 */
public class TestSingleton {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    System.out.println(Singleton.getInstance().hashCode());//打印地址
                }
            }).start();
        }
    }
}
```

### 懒汉式

```java
/**
 * 懒汉式单例
 *  * 1. 首先创建一个对象，赋值为 null
 *  * 2. 构造方法改成私有的，类外部不能创建对象
 *  * 3. 通过一个公开的方法，返回这个对象
 *  加同步的方式有两种:
 *  1. 直接在方法上加锁     public static synchronized Singleton2 getInstance()
 *  2. 用同步代码块
 */
public class Singleton2 {
    private static Singleton2 instance = null;
    private Singleton2(){}
    public static  Singleton2 getInstance(){
        if (instance == null){//这个if 是为了提高执行效率，不必每个线程进来都判断锁
            synchronized (Singleton2.class){//不能用instance 作为锁
                if (instance == null){
                    instance = new Singleton2();
                }
            }
        }
        return instance;
    }
}
```

### 静态内部类写法（懒汉式2）

```java
/**
 * 单例第三种写法：静态内部类写法
 */
public class Singleton3 {
    private Singleton3(){};
    public static class Holder{//静态内部类，在不使用的时候，不会执行
        static Singleton3 instance = new Singleton3();//使用new 本身就是线程安全的
    }
    public static Singleton3 getInstance(){
        return Holder.instance;
    }
}
```

# 枚举

- jdk1.5 之后加入
- 定义：枚举是一个引用类型，枚举是一个规定了取值范围的数据类型
- 规定了取值范围的，比如星期，性别。。。
- 枚举变量不能使用其它的数据，只能使用枚举中常量赋值，提高程序安全性
- 定义枚举使用 enum 关键字
- 枚举的**本质**：由查看 .class 文件可知
  - 枚举类是一个终止类（final class) ,并继承了 Enum 抽象类。如：public final class Gender extends Enum{...}
  - 枚举中常量式当前类型的静态常量

```java
/**
 * 性别枚举
 * 注意;
 * 1. 枚举中必须要包含枚举常量，也可以包好属性、方法。构造方法必须是私有
 * 2. 枚举常量必须在所有代码的前面，而且多个常量之间使逗号隔开 而且分号可写可不写(常量后有代码就写)
 */
public enum Gender {
    MALE,FEMALE;
}
```

```java
public class TestGender {
    public static void main(String[] args) {
        Gender gender = Gender.MALE;
        System.out.println(gender);
    }
}

```

## 枚举与 switch 结合使用

```java
/**
 * 枚举与switch 结合使用
 */
public class Demo01 {
    public static void main(String[] args) {
        Season season = Season.SPRING;
        switch (season){//switch 的判断类型可以是byte short int char String 枚举，但不能是long
            case SUMMER -> {
                System.out.println("夏天");
                break;
            }
            case SPRING -> {
                System.out.println("春天");
                break;
            }
            case AUTUMN -> {
                System.out.println("秋天");
                break;
            }
            case WINTER -> {
                System.out.println("冬天");
                break;
            }
            default -> {
                break;
            }

        }
    }
}
```

# 注解 Annotation

- jdk1.5 之后加入
- 定义：注解（Annotation）是代码里的特殊标记，程序可以读取注解，一般用于替代配置文件
- 开发人员可以通过注解告诉类如何运行
  - 在 Java 技术里注解的典型应用是：
    - 可以通过反射技术去得到类里面的注解，以决定怎么去运行类
- 常见注解：
  - @Override
  - @Deprecated
  - @SuppressWarnings 用来抑制编译时的警告信息
    - 与前两个不同，这个需添加一个参数才能正确使用，参数都是定义好的
      - @SuppressWarnings("all")
      - @SuppressWarnings("unchecked")
      - @SuppressWarnings(value = {"unchecked","deprecation"})
      - ...等
- 
- 定义注解使用 @interface 关键字，注解中只能包含属性
- 注解的**本质**：就是一个接口，其中的属性，就是一个抽象方法。由查看.class 文件可知
  - 如：publc interface MyAnnotation extends Annotation
  - public abstract String name()

## 注解属性类型

只能是：

- String类型
- 基本数据类型
- Class 类型 Class<?> class1();
- 枚举类型
- 注解类型
- 以上类型的一维数组

```java
/**
 * 创建一个注解类型
 */
public @interface MyAnnotation {
    //属性，（类似方法）
    String name() default "joker";//属性可以设置默认值，也可以不设置
    int age();
    //Class 类型
    Class<?> class1();
    //枚举类型
    Gender gender();
    //注解类型
    MyAnnotation2 annotation();
}

```

```java
public class Person {
    @MyAnnotation(name = "monster",age = 20)//注解里只有一个类型时，可以把 变量省略，直接赋值
    public void show(){

    }
}
```

## 元注解

- 元注解：用来描述注解的注解

### @Retention

- @Retention :用于指定注解可以保留的域
  - RetentionPolicy.CLASS`：注解记录在class文件中，运行Java程序时，JVM**不会保留**。
    - 这是注解的**默认值**。
  - RetentionPolicy.RUNTIME`：注解记录在class文件中，运行Java程序中，JVM**会保留**，程序可以通过反射获取该注释。
  - RetentionPolicy.SOURCE`：编译时**直接丢弃**这种策略的注释。

### @Target

- @Target :指定注解用于修饰类的哪个成员,不定义就默认全部都可以使用

  - 常用的:
  - ElementType.TYPE 可以修饰类和接口
  - ElementType.FIELD 修饰字段
  - ElementType.METHOD 修饰方法
  - 使用

  ```java
  @Target(ElementType.TYPE)
      public @interface PersonInfo {...}
  ```

  

## 反射获取注解信息

拥有的文件：

- public @interface PersonInfo

- public class Person 
- public class Demo



**PersonInfo**注解类型：

```java
@Retention(RetentionPolicy.RUNTIME)//元注解，
    public @interface PersonInfo {
    String name();
    int age();
    String sex();
}
```

**Person** 类：

```java
	public class Person {

    @PersonInfo(name = "joker",age = 20,sex = "哥哥")
    public void show(String name,int age,String sex){
        System.out.println(name +"-----" + age +"--------" + sex);
    }
}
```

**Demo**

```java
public class Demo {
    public static void main(String[] args) throws Exception {
        //1 获取类对象
        Class<?> class1 = Class.forName("com.joker.annotation.Person");
        //2 获取方法
        Method showMethod = class1.getMethod("show", String.class, int.class, String.class);
        //3 获取方法上面的注解信息
        PersonInfo annotation = showMethod.getAnnotation(PersonInfo.class);
        //4 打印注解信息 会出现异常，因为 annotation 默认为null，使用元注解解决
        System.out.println(annotation.name());
        System.out.println(annotation.age());
        System.out.println(annotation.sex());

        //5 调用方法
        Person person = (Person) class1.newInstance();
        showMethod.invoke(person,annotation.name(),annotation.age(),annotation.sex());

  }
}
```

