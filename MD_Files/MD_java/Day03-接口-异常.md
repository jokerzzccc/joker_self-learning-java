# Day03

# static

- 属性

1. 加了static的属性 即静态变量，可以直接使用类名访问：Studeng.age
2. 非静态变量必须使用对象访问

- 方法

1. 加了static的类方法可以直接访问，非静态的方法可以直接调用类里面的方法	
2. 静态方法不能直接调用非静态方法
3.  

- 匿名代码块

```java
{
    匿名代码块，直接只有两括号，可以用来赋初始值
}
```

- 静态代码块

```java
static {
    静态代码块，最先运行，只执行一次
}
```

- 静态导入包

```java
import static java.lang.Math.random;
在后面的代码就可以直接用random(),而不用Math.random();
```

# 抽象（abstract）

1. abstract,有抽象类与抽象方法（只有方法名，没有方法实现）
2. 抽象类的所有方法，继承了这个类的子类都必须要实现它的方法，除非子类也是抽象的
3. 接口可以多继承
4. 不能new这个抽象类，只能靠子类去实现它；约束
5. 抽象类中可以写普通方法，抽象方法必须在抽象类中
6. 抽象的抽象：约束
7. 存在的意义，把特性抽象出来，提高开发效率，类的可扩展性

- 抽象方法和抽象类必须使用abstract修饰符来定义，有抽象方法的类只能被定义为抽象类，抽象类里可以没有抽象方法。

- 抽象类和抽象方法的规则：
  - 抽象类必须使用abstract修饰符来修饰，抽象方法必须使用abstract修饰符来修饰，抽象方法不能有方法体。
  - 抽象类不定被实例化。即使抽象类里不包含抽象方法，这个抽象类也不能创建实例。
  - 抽象类可以包含成员变量、方法（普通方法和抽象方法）、构造器、初始化块、内部类（接口、枚举）5种成分。抽象类的构造器不能用于创建实例，主要用于被其子类调用。
  - 含有抽象方法的类（包括直接定义一个抽象方法；或继承了一个抽象父类，但没有完全实现父类包含的抽象方法；或实现了一个接口，但没有完全实现接口包含的抽象方法三种情况）只能定义为抽象类。

**注意：**

- abstract不能用于修饰成员变量，不能用于修饰局部变量，即没有抽象变量、没有抽象成员变量等说法；abstract也不能用于修饰构造器、没有抽象构造器，抽象类里定义的构造器只能是普通构造器。

- static和abstract不能同时修饰某个方法，但它们可以同时修饰内部类。

- abstract关键字修饰的方法必须被子类重写才有意义，否则这个方法永远不会有方法体，因此abstract方法不能定义为private访问权限，即private和abstract不能同时修饰方法。

一个抽象类

```java

public abstract class Shape
{
   {
      System.out.println("执行Shape的初始化块...");
   }
   private String color;
   // 定义一个计算周长的抽象方法
   public abstract double calPerimeter();
   // 定义一个返回形状的抽象方法
   public abstract String getType();
   // 定义Shape的构造器，该构造器并不是用于创建Shape对象，
   // 而是用于被子类调用
   public Shape(){}
   public Shape(String color)
   {
      System.out.println("执行Shape的构造器...");
      this.color = color;
   }
   // 省略color的setter和getter方法
   public void setColor(String color)
   {
      this.color = color;
   }
   public String getColor()
   {
      return this.color;
   }
```



# 接口（interface）

## 定义、性质

- 普通类：只有具体实现
- 抽象类：具体实现和规范（抽象方法）都有
- 接口:只有规范，自己无法写方法，专业的约束；约束和实现分离：面向接口编程
- 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是。。。，就必须能。。。”的思想
- 接口的本质是契约，就像人间的法律，制定好大家都遵守它
- OO的精髓，是对对象的抽象，最能体现这一个点的就是接口。
- 设计模式所研究的，实际就是如何合理的去抽象

## 代码中

1. 接口中的所有定义其实都是抽象的，默认为public abstract,可以直接写方法名

```java
public interface interfTest {
    //方法名
    add (String name);//默认为public abstract,且只能为public
    delete (String name);
    ...
}
```



1. 类可以实现接口

```java
public class User implements interfTest{
    
}
```

3. 实现了接口的类，就必须重写接口中的所有方法
4. 接口可以多继承，
5. 接口中也可以定义属性，且都是默认为常量，public static final,但一般不在接口里定义属性，都定义方法

## 总结

1. 接口是一种约束

2. 定义一些方法，让不同的类使用
3. public abstract
4. public static final
5. 接口不能被实例化，接口中没有构造方法
6. implements可以实现多个接口
7. 必须重写接口的方法

# 内部类

1. 定义：就是在一个类的内部再定义一个类
2. 分类

- 成员内部类

- ```java 
  public class Outer{
      
      pulic class Inner{
          
      }
  }
  //通过这个外部类来实例化内部类
  Outer.Inner inner = Outer.new Inner();
  //内部类可以获得外部类的私有属性
  
  ```

- 静态内部类（static）

- 局部内部类

```java
public void method () {
    class In{
        
    }
}
```

- 匿名内部类

```java
public class Test {
    public static void main (String[] args){
        //没有名字初始化类，不用将实例保存到变量中
        new Apple.eat();
    }
}
class Apple{
    public void eat(){
        System.out.println("1");
    }
}
```





3. 一个Java类中可以有多个class类，但只能有一个public class

# 异常（exception）

## 异常体系结构

分类：

- 检查性异常
- 运行时异常
- 错误（error）：错误不是异常，而是脱离程序员控制的问题。例如栈溢出

1. 常见的异常RuntimeException

- ArrayIndexOutOfBoundException(数组下标越界)
- NullPointerException（空指针异常）
- ArithmeticException（算术异常）
- MissingResourceException（丢失资源）
- ClassNotFoundException（找不到类）

2. error和exception的区别：

- error通常是灾难性的致命错误，是程序无法控制和处理的，当出现时，jvm一般会选择终止线程
- exception通常是可以被程序处理的

## 异常处理机制

- 抛出异常
- 捕获异常
- 异常处理的五个关键字try,catch,finally,throw,throws

```java
try {//try 监控区域
    
    System.out.println(a/b);
    
}catch (ArithmeticException e){//catch(想要捕获的异常类型) 捕获异常
    System.out.println("程序出现异常");
}finally {//处理善后工作
    System.out.println("finally");
}
```

1. 可以不要finally，一般用于IO，资源，关闭
2. 异常最高的类型是，Throwable
3. catch可以写多个，层层递进，但等级高的类型要写在后面
4. 假设要捕获多个异常：从小到大
5. ctrl+alt+T:直接生成try,catch
6. throw，一般用在方法里面

```java
if (b == 0){
        throw new ArithmeticException();//throw 主动抛出异常
    }
```

7. throws,假设在方法中，处理不了这个异常。方法上抛出异常

```java
public void test(int a,int b) throws ArithmeticException {
    
}
```

## 自定义异常

1. 一般用不上自定义异常，

2. 用户自定义异常类，只需要继承Exception类

   ```java
   public class MyException extends Exception{
       private int detail;
       public MyException(int a){
           this.detail = a;
       }
   
       //toString:异常的打印信息
   
       @Override
       public String toString() {
           return "MyException{" +
                   "detail=" + detail +
                   '}';
       }
   }
   ```

## 经验总结

- 异常的最佳实践博客：https://segmentfault.com/a/1190000015028573

- 处理运行异常时，采用逻辑去合理规避同时辅助try-catch处理
- 在多重 catch 块后面，可以加一个catch (Exception) 来处理可能会被遗漏的异常。
- 对于不确定的代码，也可以加上 try-catch,处理潜在的异常
- 尽量去处理异常，切忌这是简单地调用 printStackTrace() 去打印输出
- 具体如何处理异常，要根据不同的业务需求和异常类型去决定
- 尽量添加 finally 语句块去释放占用的资源

# 错误（error）

- error对象由Java虚拟机生成并抛出，大多数错误与代码编写者所执行的操作无关
- 