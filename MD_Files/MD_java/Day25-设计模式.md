# 目录

[toc]

# 设计模式(Design Patterns)

- 参考网址：
  - 入门教学：http://c.biancheng.net/design_pattern/

- 前13个（工厂方法里有两个，就是14个）设计模式，参考忘在head First 里有详细介绍

# 1、工厂模式（Factory）

- 工厂模式的抽象工厂和工厂方法，分别算一个，所以共23个设计模式

## 1.1 抽象工厂(Abstract Factory)



## 1.2 工厂方法(Factory Method)



# 2、单例模式（Singleton)



# 3、代理模式(Proxy)



# 4、模板方法模式(Template Method)



# 5、观察者模式(Observer)



# 6、装饰者模式(Decorator)



# 7、组合模式（Composite）



# 8、原型模式(Prototype)



# 9、适配器模式(Adapter)



# 10、迭代器模式(Iterator)



# 11、命令模式(Command)





# 12、外观模式(Facade)



# 13、策略模式(Strategy)

- 参考博客：
  - 原理与使用示例：https://juejin.cn/post/6971587940715757598
  - 反射优化，：https://cloud.tencent.com/developer/article/1691946
  - https://blog.51cto.com/u_15127651/2902220

## 13.1 是什么

策略（Strategy）模式的**定义**：

- 该模式定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的变化不会影响使用算法的客户。
- 策略模式属于对象行为模式，它通过对算法进行封装，把使用算法的责任和算法的实现分割开来，并委派给不同的对象对这些算法进行管理。



策略模式的**主要角色**如下。

1. 抽象策略（Strategy）类：定义了一个公共接口，各种不同的算法以不同的方式实现这个接口，环境角色使用这个接口调用不同的算法，一般使用**接口或抽象类**实现。
2. 具体策略（Concrete Strategy）类：实现了抽象策略定义的接口，提供具体的算法实现。
3. 环境（Context）类：持有一个策略类的引用，最终给客户端调用。



策略模式的**结构图**：

![image-20220605212502711](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220605212502711.png)

## 13.2 干什么

+ 策略模式利用组合，委托等技术和思想，有效的避免很多if条件语句
+ 策略模式提供了开放-封闭原则，使代码更容易理解和扩展
+ 策略模式中的代码可以复用

## 13.3 优缺点

策略模式的**主要优点**如下。

1. 多重条件语句不易维护，而使用策略模式可以避免使用多重条件语句，如 if...else 语句、switch...case 语句。
2. 策略模式提供了一系列的可供重用的算法族，恰当使用继承可以把算法族的公共代码转移到父类里面，从而避免重复的代码。
3. 策略模式可以提供相同行为的不同实现，客户可以根据不同时间或空间要求选择不同的。
4. 策略模式提供了对开闭原则的完美支持，可以在不修改原代码的情况下，灵活增加新算法。
5. 策略模式把算法的使用放到环境类中，而算法的实现移到具体策略类中，实现了二者的分离。


其**主要缺点**如下。

1. 客户端必须理解所有策略算法的区别，以便适时选择恰当的算法类。
2. 策略模式造成很多的策略类，增加维护难度。



## 13.4 不用策略模式时的代码：

- 在业务的方法代码里 if-else, switch 语句：

```java
public void cook(Integer mode) {
        //...
    
        if (mode == CookingWayEnum.DUCK.getCode()){
            System.out.println("老鸭汤");
        }
        if (mode == CookingWayEnum.CHICKEN.getCode()){
            System.out.println("宫保鸡丁");
        }
        if (mode == CookingWayEnum.FISH.getCode()){
            System.out.println("红烧鱼");
        }

        // ...
    }
```



**对比**

```java
public void cook(Integer mode) {
        //...
    
        CookingStrategy instance = CookingContext.getInstance(mode);
    	instance.cooking();

        // ...
    }
```



## 13.5 策略模式实践示例

代码结构：

- 枚举类：CookingWayEnum.java

- 策略接口：CookingStrategy.java
- 策略实现类：
  - DuckCooking.java 
  - ChickenCooking.java
  - FishCooking.java
- 环境类：CookingContext.java
- 使用测试：CookingContextTest.java



- 如果有新的行为分支（此例为枚举），添加一个枚举值，再新增一个具体实现类就行了。





### CookingWayEnum.java

```java
public enum CookingWayEnum {

    CHICKEN(0, "鸡", "ChickenCooking", "chickenCooking"),
    DUCK(1, "鸭", "DuckCooking", "duckCooking"),
    FISH(2, "鱼", "FishCooking", "fishCooking");

    private final Integer code;

    private final String stuff;

    private final String clazz;

    private final String beanName;

    private static final String PACKAGE_PATH = "com.joker.demo.service.impl.";

    CookingWayEnum(Integer code, String stuff, String clazz, String beanName) {
        this.code = code;
        this.stuff = stuff;
        this.clazz = clazz;
        this.beanName = beanName;
    }

    public Integer getCode() {
        return code;
    }

    public String getStuff() {
        return stuff;
    }

    public String getClazz() {
        return clazz;
    }

    public String getBeanName() {
        return beanName;
    }

    public static Map<Integer, String> getAllClazz(){
        HashMap<Integer, String> map = new HashMap<>(16);
        Arrays.stream(CookingWayEnum.values())
                .forEach(cookingWayEnum -> map.put(cookingWayEnum.getCode(), PACKAGE_PATH + cookingWayEnum.getClazz()));
        return map;
    }

}
```

### CookingStrategy.java

```java
@Service
public interface CookingStrategy {
    void cooking();
}
```

### ChickenCooking.java

```java
@Component
public class ChickenCooking implements CookingStrategy {
    @Override
    public void cooking() {
        System.out.println("宫保鸡丁");
    }
}
```

### DuckCooking.java 

```java
@Component
public class DuckCooking implements CookingStrategy {
    @Override
    public void cooking() {
        System.out.println("老鸭汤");
    }
}
```

### FishCooking.java

```java
@Component
public class FishCooking implements CookingStrategy {
    @Override
    public void cooking() {
        System.out.println("红烧鱼");
    }
}
```

### CookingContext.java

```java
@Component
public class CookingContext {

    public static CookingStrategy getInstance(Integer code) throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        CookingStrategy cookingStrategy = null;
        Map<Integer, String> allClazz = CookingWayEnum.getAllClazz();
        String clazz = allClazz.get(code);
        if (!clazz.isEmpty()) {
            cookingStrategy = (CookingStrategy) Class.forName(clazz).newInstance();
        }
        return cookingStrategy;
    }

    
}
```

### CookingContextTest.java

```java
@Test
void testGetInstance() throws IllegalAccessException, InstantiationException, ClassNotFoundException {
    CookingStrategy instance = CookingContext.getInstance(0);
    instance.cooking();
}
```



### 



# 14、中介者模式（Mediator）



# 15、备忘录模式（Memento）



# 16、状态模式(State)

- 参考博客：
  - 状态模式详解：http://c.biancheng.net/view/1388.html
  - 状态模式与策略模式的区别：
    - https://www.runoob.com/w3cnote/state-vs-strategy.html
    - https://www.cnblogs.com/yssjun/p/11116652.html
  - 

# 17、访问者模式（Visitor）



# 18、桥接模式（Bridge）



# 19、生成器模式（Builder）



# 20、责任链模式（Chain of Responsibility）



# 21、蝇量模式（Flyweight）



# 22、解释器模式（Interpreter）



# THE END