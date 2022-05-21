# 目录

[toc]

# Essays_05_Lombok 相关

时间：2022年5月8日

参考博客：

- Lombok 官网 API ：https://projectlombok.org/features/all
- Lombok  GitHub :https://github.com/projectlombok/lombok
- 使用博客教学：
  - https://juejin.cn/post/6844903557016076302





# 1、Lombok 的安装

步骤：

1. IDEA 插件安装 Lombok 

2. 如果是 maven 项目，导入 Lombok 依赖：

   ```xml
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.16.22</version>
   </dependency>
   ```

   

# 2、Lombok 注解的概述

- `val`：用在局部变量前面，相当于将变量声明为final

- `@NonNull`：给方法参数增加这个注解会自动在方法内对该参数进行是否为空的校验，如果为空，则抛出NPE（NullPointerException）

- `@Cleanup`：自动管理资源，用在局部变量之前，在当前变量范围内即将执行完毕退出之前会自动清理资源，自动生成try-finally这样的代码来关闭流

- `@Getter/@Setter`：用在属性上，再也不用自己手写setter和getter方法了，还可以指定访问范围

- `@ToString`：用在类上，可以自动覆写toString方法，当然还可以加其他参数，例如@ToString(exclude=”id”)排除id属性，或者@ToString(callSuper=true, includeFieldNames=true)调用父类的toString方法，包含所有属性

- `@EqualsAndHashCode`：用在类上，自动生成equals方法和hashCode方法

- `@NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor`：用在类上，自动生成无参构造和使用所有参数的构造函数以及把所有@NonNull属性作为参数的构造函数，如果指定staticName = “of”参数，同时还会生成一个返回类对象的静态工厂方法，比使用构造函数方便很多

- `@Data`：注解在类上，相当于同时使用了`@ToString`、`@EqualsAndHashCode`、`@Getter`、`@Setter`和`@RequiredArgsConstrutor`这些注解，对于`POJO类`十分有用

- `@Value`：用在类上，是@Data的不可变形式，相当于为属性添加final声明，只提供getter方法，而不提供setter方法

- `@Builder`：用在类、构造器、方法上，为你提供复杂的builder APIs，让你可以像如下方式一样调用    

  - ```java
    Person.builder()
        .name("Adam Savage")
        .city("SanFrancisco")
        .job("Mythbusters")
        .job("Unchained Reaction")
        .build();
    ```

  - 更多说明参考[Builder](https://link.juejin.cn?target=https%3A%2F%2Fprojectlombok.org%2Ffeatures%2FBuilder.html)

- `@SneakyThrows`：自动抛受检异常，而无需显式在方法上使用throws语句

- `@Synchronized`：用在方法上，将方法声明为同步的，并自动加锁，而锁对象是一个私有的属性`$lock`或`$LOCK`，而java中的synchronized关键字锁对象是this，锁在this或者自己的类对象上存在副作用，就是你不能阻止非受控代码去锁this或者类对象，这可能会导致竞争条件或者其它线程错误

- `@Getter(lazy=true)`：可以替代经典的Double Check Lock样板代码

- `@Log`：根据不同的注解生成不同类型的log对象，但是实例名称都是log，有六种可选实现类
  - `@CommonsLog` Creates log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);
  - `@Log` Creates log = java.util.logging.Logger.getLogger(LogExample.class.getName());
  - `@Log4j` Creates log = org.apache.log4j.Logger.getLogger(LogExample.class);
  - `@Log4j2` Creates log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);
  - `@Slf4j` Creates log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
  - `@XSlf4j` Creates log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);





# 3、Lombok 注解使用示例

## @EqualsAndHashCode

- `@EqualsAndHashCode`：用在类上，自动生成equals方法和hashCode方法
- 可以设置：
  - exclude: 哪些字段不包含进入 重写的 equals 和 hashCode 方法。
  - include:
  - callSuper: 重写的 equals 和 hashCode 方法是否包含父类的字段。默认是 false
- `@Data` 就包含了 `@EqualsAndHashCode`  且 callSuper 默认为 false。

使用示例：

```java 
@EqualsAndHashCode(exclude = {"id", "shape"}, callSuper = true)
public class Son extends Parents {
    private Integer id;
    private String shap;
    private String Message;
}

```

## @Builder

- 参考博客：https://www.jianshu.com/p/d08e255312f9

- `@Builder`注释为你的类生成相对略微复杂的构建器API。`@Builder`可以让你以下面显示的那样调用你的代码，来初始化你的实例对象：
- 可以用在类、构造器、方法上

### @Builder 做了什么

1. 创建一个名为`ThisClassBuilder`的内部静态类，并具有和实体类形同的属性（称为构建器）。
2. 在构建器中：对于目标类中的所有的属性和未初始化的`final`字段，都会在构建器中创建对应属性。
3. 在构建器中：创建一个无参的`default`构造函数。
4. 在构建器中：对于实体类中的每个参数，都会对应创建类似于`setter`的方法，只不过方法名与该参数名相同。 并且返回值是构建器本身（便于链式调用），如上例所示。
5. 在构建器中：一个`build()`方法，调用此方法，就会根据设置的值进行创建实体对象。
6. 在构建器中：同时也会生成一个`toString()`方法。
7. 在实体类中：会创建一个`builder()`方法，它的目的是用来创建构建器。

### 搭配 @Singular

- `@Builder`也可以为**集合类型**的参数或字段生成一种特殊的方法。 它采用修改列表中一个元素而不是整个列表的方式，可以是增加一个元素，也可以是删除一个元素。

- `@Singular`只能应用于`lombok`已知的集合类型。目前，支持的类型有
  + `Iterable`, `Collection`, 和`List` (一般情况下，由压缩的不可修改的`ArrayList`支持).
  + `Set`, `SortedSet`, and `NavigableSet` (一般情况下，生成可变大小不可修改的`HashSet`或者`TreeSet`).
  + `Map`, `SortedMap`, and `NavigableMap` (一般情况下，生成可变大小不可修改的`HashMap`或者`TreeMap`).

- `@Singular` 可以设置 value 属性  ，指定 build 时的名字。
- 在使用`@Singular`注释注释一个集合字段（使用`@Builder`注释类），`lombok`会将该构建器节点视为一个集合，并生成两个`adder`方法而不是`setter`方法。
  + 一个向集合添加单个元素
  + 一个将另一个集合的所有元素添加到集合中
- 将不生成仅设置集合（替换已添加的任何内容）的setter。 还生成了clear方法。 这些singular构建器相对而言是有些复杂的，主要是来保证以下特性：
  1. 在调用`build()`时，**生成的集合将是不可变的**。
  2. 在调用`build()`之后调用其中一个`adder`方法或`clear`方法不会修改任何已经生成的对象。如果对集合修改之后，再调用`build()`，则会创建一个基于上一个对象创建的对象实体。
  3. 生成的集合将被压缩到最小的可行格式，同时保持高效。



### 使用示例

#### **实体类：**

```java
@Builder
public class User {
    private final Integer id;
    private final String zipCode = "123456";
    private String username;
    private String password;
    @Singular
    private List<String> hobbies;
}
```



#### **编译后的样子：**

```java
// 编译后：
public class User {
    private final Integer id;
    private final String zipCode = "123456";
    private String username;
    private String password;
    private List<String> hobbies;
    User(Integer id, String username, String password, List<String> hobbies) {
        this.id = id; this.username = username;
        this.password = password; this.hobbies = hobbies;
    }

    public static User.UserBuilder builder() {return new User.UserBuilder();}

    public static class UserBuilder {
        private Integer id;
        private String username;
        private String password;
        private ArrayList<String> hobbies;
        UserBuilder() {}
        public User.UserBuilder id(Integer id) { this.id = id; return this; }
        public User.UserBuilder username(String username) { this.username = username; return this; }
        public User.UserBuilder password(String password) { this.password = password; return this; }

        public User.UserBuilder hobby(String hobby) {
            if (this.hobbies == null) {
                this.hobbies = new ArrayList();
            }
            this.hobbies.add(hobby);
            return this;
        }

        public User.UserBuilder hobbies(Collection<? extends String> hobbies) {
            if (this.hobbies == null) {
                this.hobbies = new ArrayList();
            }
            this.hobbies.addAll(hobbies);
            return this;
        }

        public User.UserBuilder clearHobbies() {
            if (this.hobbies != null) {
                this.hobbies.clear();
            }
            return this;
        }

        public User build() {
            List hobbies;
            switch(this.hobbies == null ? 0 : this.hobbies.size()) {
            case 0:
                hobbies = Collections.emptyList();
                break;
            case 1:
                hobbies = Collections.singletonList(this.hobbies.get(0));
                break;
            default:
                hobbies = Collections.unmodifiableList(new ArrayList(this.hobbies));
            }
            return new User(this.id, this.username, this.password, hobbies);
        }
        public String toString() {
            return "User.UserBuilder(id=" + this.id + ", username=" + this.username + ", password=" + this.password + ", hobbies=" + this.hobbies + ")";
        }
    }
}
```



- 其实，lombok的创作者还是很用心的，在进行build()来创建实例对象时，
   并没有直接使用`Collections.unmodifiableList(Collection)`此方法来床架实例，而是分为三种情况。

  + 第一种，当集合中没有元素时，创建一个空list
  + 第二种情况，当集合中存在一个元素时，创建一个不可变的单元素list
  + 第三种情况，根据当前集合的元素数量创建对应合适大小的list

  当然我们看编译生成的代码，创建了三个关于集合操作的方法：

  + `hobby(String hobby)`：向集合中添加一个元素
  + `hobbies(Collection<? extends String> hobbies)`：添加一个集合所有的元素
  + `clearHobbies()`：清空当前集合数据

#### **使用：**

```java
@Builder
public class User {
    private final Integer id;
    private final String zipCode = "123456";
    private String username;
    private String password;
    @Singular(value = "testHobbies")
    private List<String> hobbies;
}

// 测试类
public class BuilderTest {
    public static void main(String[] args) {
        User user = User.builder()
                .testHobbies("reading")
                .testHobbies("eat")
                .id(1)
                .password("admin")
                .username("admin")
                .build();
        System.out.println(user);
    }
}
```







# THE END

