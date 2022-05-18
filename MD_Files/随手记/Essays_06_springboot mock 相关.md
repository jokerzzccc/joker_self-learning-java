# 目录

[toc]

# Essays_06_springboot mock 相关

时间：2022年5月8日



参考博客：

- https://www.baeldung.com/java-spring-mockito-mock-mockbean
- spring mock 常用接口参考：http://www.codebaoku.com/it-java/it-java-229914.html
- mockito 相关：
  - 使用参考：https://blog.51cto.com/u_15239532/2835627
  - API :https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html
  - mockito 使用详解https://juejin.cn/post/6844903631137800206
- springboot 单元测试实践==最佳==：https://juejin.cn/post/6918758473131884558#heading-0
- Squaretest IDEA 插件，自动生成单元测试，的使用：https://mp.weixin.qq.com/s/o1Q0jCnwE5dCOaSVG37WeA



# 1、生成测试数据

- 参考博客：
  - https://juejin.cn/post/6856925705402318856#heading-0



## 1.1 Java Faker

- Java Faker  JavaDoc 官方 API :https://javadoc.io/doc/com.github.javafaker/javafaker/latest/com/github/javafaker/Faker.html
- API ：http://dius.github.io/java-faker/apidocs/index.html
- Java Faker GitHub 官网：https://github.com/DiUS/java-faker
- Java Fake 使用示例：https://www.cnblogs.com/yangzhilong/p/11277575.html



## 1.2 JMockData

- JMockData GitHub :https://github.com/jsonzou/jmockdata
- JMockData 使用示例：
  - GitHub 官方使用示例：https://github.com/jsonzou/jmockdata/blob/master/src/test/java/com/github/jsonzou/jmockdata/JMockDataTest.java
  - https://daimajiaoliu.com/daima/9eab467ebedd405
  - https://daimajiaoliu.com/daima/60bcd791fc31c03



### JMockDataTest

- GitHub 官方使用示例：https://github.com/jsonzou/jmockdata/blob/master/src/test/java/com/github/jsonzou/jmockdata/JMockDataTest.java

```java
package com.github.jsonzou.jmockdata;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.github.jsonzou.jmockdata.bean.*;
import com.github.jsonzou.jmockdata.bean.circular.AXB;
import com.github.jsonzou.jmockdata.bean.enums.DayEnum;
import com.github.jsonzou.jmockdata.bean.enums.ErrorEnum;
import org.junit.Test;

import java.lang.reflect.Field;
import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Date;
import java.util.List;
import java.util.Map;
import java.util.Set;

import static org.junit.Assert.*;

public class JMockDataTest {

  @Test
  public void testBasic() {
    //基本类型模拟
    int intNum = JMockData.mock(int.class);
    assertNotNull(intNum);
    int[] intArray = JMockData.mock(int[].class);
    assertNotNull(intArray);
    Integer integer = JMockData.mock(Integer.class);
    assertNotNull(integer);
    Integer[] integerArray = JMockData.mock(Integer[].class);
    assertNotNull(integerArray);
    //常用类型模拟
    BigDecimal bigDecimal = JMockData.mock(BigDecimal.class);
    assertNotNull(bigDecimal);
    BigInteger bigInteger = JMockData.mock(BigInteger.class);
    assertNotNull(bigInteger);
    Date date = JMockData.mock(Date.class);
    assertNotNull(date);
    String str = JMockData.mock(String.class);
    assertNotNull(str);
    DayEnum dayEnum = JMockData.mock(DayEnum.class);
    assertNotNull(dayEnum);

    try {
      JMockData.mock(ErrorEnum.class);
      fail();
    } catch (Exception e) {
      // Ignore
    }
  }

  @Test
  public void testBasicData() {
    BasicBean basicBean = JMockData.mock(BasicBean.class);
    System.out.println(JSON.toJSONString(basicBean,true));
    assertNotNull(basicBean);

    try {
      JMockData.mock(ErrorBean.class);
      fail();
    } catch (Exception e) {
      // Ignore
    }
  }

  @Test
  public void testCircular() {
    MockConfig mockConfig = new MockConfig().setEnabledCircle(true);
    AXB axb = JMockData.mock(AXB.class,mockConfig);
    AXB circularAxb = axb.getBXA().getAXB();
    assertSame(axb, circularAxb);
  }

  @Test
  public void testSelf() {
    MockConfig mockConfig = new MockConfig().setEnabledCircle(true);
    SelfRefData selfRefData = JMockData.mock(SelfRefData.class,mockConfig);
    assertSame(selfRefData.getParent(), selfRefData);
  }

  @Test
  //******注意TypeReference要加{}才能模拟******
  public void testTypeRefrence() {
    //模拟基础类型，不建议使用这种方式，参考基础类型章节直接模拟。
    Integer integerNum = JMockData.mock(new TypeReference<Integer>() {
    });
    assertNotNull(integerNum);
    Integer[] integerArray = JMockData.mock(new TypeReference<Integer[]>() {
    });
    assertNotNull(integerArray);
    //模拟集合
    List<Integer> integerList = JMockData.mock(new TypeReference<List<Integer>>() {
    });
    assertNotNull(integerList);
    //模拟数组集合
    List<Integer[]> integerArrayList = JMockData.mock(new TypeReference<List<Integer[]>>() {
    });
    assertNotNull(integerArrayList);
    //模拟集合数组
    List<Integer>[] integerListArray = JMockData.mock(new TypeReference<List<Integer>[]>() {
    });
    assertNotNull(integerListArray);
    //模拟集合实体
    List<BasicBean> basicBeanList = JMockData.mock(new TypeReference<List<BasicBean>>() {
    });
    assertNotNull(basicBeanList);
    //各种组合忽略。。。。map同理。下面模拟一个不知道什么类型的map
    Map<List<Map<Integer, String[][]>>, Map<Set<String>, Double[]>> some = JMockData
        .mock(new TypeReference<Map<List<Map<Integer, String[][]>>, Map<Set<String>, Double[]>>>() {
        });
    assertNotNull(some);
  }

  @Test
  public void testGenericData() {
    GenericData<Integer, String, BasicBean> genericData = JMockData.mock(new TypeReference<GenericData<Integer, String, BasicBean>>() {
    });
    System.out.println(JSON.toJSONString(genericData,true));
    assertNotNull(genericData);
  }

  @Test
  public void testMockConfig() {
    MockConfig mockConfig = new MockConfig()
        .byteRange((byte) 0, Byte.MAX_VALUE)
        .shortRange((short) 0, Short.MAX_VALUE)
        .intRange(0, Integer.MAX_VALUE)
        .floatRange(0.0f, Float.MAX_EXPONENT)
        .doubleRange(0.0, Double.MAX_VALUE)
        .longRange(0, Long.MAX_VALUE)
        .dateRange("2010-01-01", "2020-12-30")
        .sizeRange(5, 10)
        .stringSeed("a", "b", "c")
        .charSeed((char) 97, (char) 98);
    BasicBean basicBean = JMockData.mock(BasicBean.class, mockConfig);
    assertNotNull(basicBean);

    try {
      JMockData.mock(BasicBean.class, new MockConfig().dateRange("20100101", "20301230"));
      fail();
    } catch (Exception e) {
      // Ignore
    }
  }

  /**
   * 自定义配置测试
   * 排除字段测试
   */
  @Test
  public void testCustomDataConfigAndExclude(){
    MockConfig mockConfig = new MockConfig()
            // 全局配置
            .globalConfig()
            .sizeRange(1,1)
            .charSeed((char) 97, (char) 98)
            .byteRange((byte) 0, Byte.MAX_VALUE)
            .shortRange((short) 0, Short.MAX_VALUE)

            // 某些字段（名等于integerNum的字段、包含float的字段、double开头的字段）配置
            .subConfig("integerNum","*float*","double*")
            .intRange(10, 11)
            .floatRange(1.22f, 1.50f)
            .doubleRange(1.50,1.99)

            // 某个类的某些字段（long开头的字段、date结尾的字段、包含string的字段）配置。
            .subConfig(BasicBean.class,"long*","*date","*string*")
            .longRange(12, 13)
            .dateRange("2018-11-20", "2018-11-30")
            .stringSeed("SAVED", "REJECT", "APPROVED")
            .sizeRange(1,1)

            // 全局配置
            .globalConfig()
            // 排除所有包含list/set/map字符的字段。表达式不区分大小写。
            .excludes("*List*","*Set*","*Map*")
            // 排除所有Array开头/Boxing结尾的字段。表达式不区分大小写。
            .excludes(BasicBean.class,"*Array","Boxing*");
    BasicBean basicBean = JMockData.mock(BasicBean.class, mockConfig);
    assertNotNull(basicBean);
    List<BasicBean> basicBeans = JMockData.mock(new TypeReference<List<BasicBean>>(){}, mockConfig);
    assertNotNull(basicBeans);
    System.out.println(JSON.toJSONString(basicBean,true));
    System.out.println("==============================");
    System.out.println(JSON.toJSONString(basicBeans,true));
  }
  @Test
  public void testBooleanMock() {
    MockConfig mockConfig = new MockConfig()
           // .booleanSeed(true,true)
            .subConfig(Boolean.class)
           // .booleanSeed(false,false)
            .globalConfig();
    for (int i=0;i<100;i++){
      System.out.print(JMockData.mock(Boolean.class,mockConfig)+" ");
    }
  }

  /**
   * 根据正则模拟数据
   */
  @Test
  public void testXegerMock() {
    MockConfig mockConfig = new MockConfig()
            // 随机段落字符串
            .stringRegex("I'am a nice man\\.And I'll just scribble the characters, like：[a-z]{2}-[0-9]{2}-[abc123]{2}-\\w{2}-\\d{2}@\\s{1}-\\S{1}\\.?-.")
            // 邮箱
            .subConfig(RegexTestDataBean.class,"userEmail")
            .stringRegex("[a-z0-9]{5,15}\\@\\w{3,5}\\.[a-z]{2,3}")
            // 用户名规则
            .subConfig(RegexTestDataBean.class,"userName")
            .stringRegex("[a-zA-Z_]{1}[a-z0-9_]{5,15}")
            // 年龄
            .subConfig(RegexTestDataBean.class,"userAge")
            .numberRegex("[1-9]{1}\\d?")
            // 用户现金
            .subConfig(RegexTestDataBean.class,"userMoney")
            .numberRegex("[1-9]{2}\\.\\d?")
            // 用户的得分
            .subConfig(RegexTestDataBean.class,"userScore")
            .numberRegex("[1-9]{1}\\d{1}")
            // 用户身价
            .subConfig(RegexTestDataBean.class,"userValue")
            .numberRegex("[1-9]{1}\\d{3,8}")
            .globalConfig();

    System.out.println(JSONObject.toJSONString(JMockData.mock(RegexTestDataBean.class,mockConfig),true));

  }

  /**
   * 测试小数位数
   */
  @Test
  public void testDecimalScaleMock() {
    MockConfig mockConfig = new MockConfig()
            .doubleRange(-1.1d,9999.99999d)
            .floatRange(-1.11111f,9999.99999f)
            .decimalScale(3) // 设置小数位数为3，默认是2
            .globalConfig();
    for (int i=0;i<100;i++){
      System.out.print(JMockData.mock(BigDecimal.class,mockConfig)+" ");
      System.out.print(JMockData.mock(Double.class,mockConfig)+" ");
      System.out.println(JMockData.mock(Float.class,mockConfig)+" ");
    }
  }

  /**
   * 测试无域对象
   */
  @Test
  public void testNoneFieldBeanMock() {
    NoneFieldObject noneFieldObject = JMockData.mock(NoneFieldObject.class);
    System.out.println(noneFieldObject);
  }

  /**
   * 测试无域对象
   */
  @Test
  public void testInnerBeanMock() {
    InnerBeanObject innerBeanObject = JMockData.mock(InnerBeanObject.class);
    InnerBeanObject.InnerBean innerBean = JMockData.mock(InnerBeanObject.InnerBean.class);
    System.out.println(innerBeanObject);
    System.out.println(innerBean);
  }
  /**
   * 测试Lombok对象
   */
  @Test
  public void testLombokBeanMock() throws IllegalAccessException {
    LombokBean lombokBean = JMockData.mock(LombokBean.class);
    printBeanFieldInfo(LombokBean.class,lombokBean);
  }
  /**
   * 测试Timestamp,LocalDateTime,LocalDate,LocalTime的模拟
   */
  @Test
  public void testLocalDateTimeMock() throws IllegalAccessException {
    LocalDateTimeBean localDateTimeBean = JMockData.mock(LocalDateTimeBean.class);
    printBeanFieldInfo(LocalDateTimeBean.class,localDateTimeBean);
  }
  /**
   * 测试Timestamp,LocalDateTime,LocalDate,LocalTime的模拟
   */
  @Test
  public void noneGetterSetterAndPlainAndChainBeanMock() throws IllegalAccessException {
    NoneGetterSetterAndFluentAndChainBean noneGetterSetterAndFluentAndChainBean = JMockData.mock(NoneGetterSetterAndFluentAndChainBean.class);
    printBeanFieldInfo(NoneGetterSetterAndFluentAndChainBean.class,noneGetterSetterAndFluentAndChainBean);
  }

  /**
   * 测试BeanMockerInterceptor
   */
  @Test
  public void noneBeanMockerInterceptor() throws IllegalAccessException {
      MockConfig mockConfig = new MockConfig();
//      mockConfig.registerBeanMockerInterceptor(new BeanMockerInterceptor() {
      mockConfig.registerBeanMockerInterceptor(SimpleBean.class,new BeanMockerInterceptor<SimpleBean>() {
          @Override
          public Object mock(Class<SimpleBean> clazz, Field field, SimpleBean bean, DataConfig dataConfig) {
              // 场景1
              if(field.getName().startsWith("string")){
                bean.setStringValue("abc");
                return InterceptType.UNMOCK;
              }
              // 场景2
              if(field.getName().startsWith("integer")){
                return 888888999;
              }
               // 场景3
              if(field.getName().startsWith("byte")){
                return InterceptType.UNMOCK;
              }
              // 场景4
              return InterceptType.MOCK;
          }
      });
    SimpleBean simpleBean = JMockData.mock(SimpleBean.class, mockConfig);
      printBeanFieldInfo(SimpleBean.class, simpleBean);
  }

    /**
     * 测试字段修饰符
     * @throws IllegalAccessException
     */
  @Test
  public void noneModifierBean_1() throws IllegalAccessException {
      MockConfig mockConfig = new MockConfig();
      mockConfig.setEnabledStatic(true);
      mockConfig.setEnabledPrivate(false);
      mockConfig.setEnabledPublic(false);
      mockConfig.setEnabledProtected(false);
      ModifierBean basicBean = JMockData.mock(ModifierBean.class, mockConfig);
      printBeanFieldInfo(ModifierBean.class,basicBean);
  }

  /**
   * 打印bean 属性信息	
   * @param clazz
   * @param result
   * @throws IllegalAccessException
   */
  private void printBeanFieldInfo(Class<?>clazz,Object result) throws IllegalAccessException {
    Field[] fields = clazz.getDeclaredFields();
    for(Field field:fields){
      field.setAccessible(true);
      System.out.println(field.getName()+" <---> "+JSON.toJSONString(field.get(result)));
    }
  }


}
```



# 2、springboot 测试 ：mockito + JUnit 视频



# 2、Mockito

- mockito 官网：https://site.mockito.org/
- mockito GitHub :https://github.com/mockito/mockito
- mockito 相关：
  - 使用参考：https://blog.51cto.com/u_15239532/2835627
  - mockito 使用详解https://juejin.cn/post/6844903631137800206
- Mockito Java API :https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html 
- API 位置在官方文档的这：
- ![image-20220515130552437](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515130552437.png)

## 2.1 为什么使用 mockito

Mock 可以理解为创建一个虚假的对象，或者说模拟出一个对象（因为测试往往不需要对真实的数据库数据进行操作），在测试环境中用来替换真实的对象。以达到一下目的：

- 验证该对象的某些方法的调用情况：调用了多少次，参数是多少。
- 给这个对象的行为做一些定义，来指定返回结果，或者指定特定的动作。



基本介绍：

![image-20220515170129250](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515170129250.png)

## 2.2 Mock 方法

测试类可以静态导入 Mockito ，就不用一直使用类名调用了

```java
import static org.mockito.Mockito.*;
```



mock 相关方法：

![image-20220515130934300](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515130934300.png)



```java
 public static <T> T mock(Class<T> classToMock)
```

- classToMock : 待 mock 方法的 class 类
- 返回值：返回 mock 出来的类。

```java
Random mock = Mockito.mock(Random.class);
```

> 当使用 mock 对象时，如果不对其行为进行定义，则 mock 对象方法的返回值，为返回类型的默认值。



## 2.3 验证（verify）和断言（Assert)

### Verify 验证

**验证**：是校验待验证对象是否发生过某些行为，

- 方法：`public static <T> T verify(T mock, VerificationMode mode)`



使用 `verify()` 方法验证：

- 结合 `time()` 方法， 可以校验某些操作发生的次数。

```java
@Test
void add() {
    Random mock = Mockito.mock(Random.class);
    System.out.println(mock.nextInt());
    verify(mock, Mockito.times(2)).nextInt();
}

// 返回结果
0

org.mockito.exceptions.verification.TooFewActualInvocations: 
random.nextInt();
Wanted 2 times: // 期望的次数
-> at com.joker.demo.demo01Test.add(demo01Test.java:19)
But was 1 time: // 实际的次数
-> at com.joker.demo.demo01Test.add(demo01Test.java:18)

```



### 断言 Assert 

- 断言：
  - 断言是一种除错机制，**用于验证代码是否符合编码人员的预期**。
  - 仅用于测试阶段。
  - 在运行过程中，如果assert 的参数为假，那么程序就会中止（一般地还会出现提示对话，说明在什么地方引发了assert）。

- 使用的类是：`Assertions`



```java
@Test
void add() {
    Random mock = Mockito.mock(Random.class);
    System.out.println(mock.nextInt());
    Assertions.assertEquals(1,mock.nextInt());
}

// 返回结果
org.opentest4j.AssertionFailedError: 
Expected :1
Actual   :0

```

## 2.4 给 mock 对象打桩（Stub)

- **打桩的定义**：给 mock 对象规定一行的**行为**，使其安装我们的要求来执行具体的操作。

```java
// 这个就是打桩
Mockito.when(mock.nextInt()).thenReturn(100);
```



常见的打桩方法：

- when().thenReturn()
  - 设定返回值
- when().thenThrow()
  - 抛出异常
  - 应用场景，就是，当编写测试的时候，进入异常情况
- when().thenCallRealMethod()
  - 调用真实的方法

![image-20220515170416867](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515170416867.png)









## 2.5 @Mock 注解

### @Mock 官方 文档：



+ Minimizes repetitive mock creation code.
+ Makes the test class more readable.
+ Makes the verification error easier to read because the **field name** is used to identify the mock.

```java
   public class ArticleManagerTest {

       @Mock 
       private ArticleCalculator calculator;
       @Mock 
       private ArticleDatabase database;
       @Mock 
       private UserProvider userProvider;

       private ArticleManager manager;

       @org.junit.jupiter.api.Test
       void testSomethingInJunit5(@Mock ArticleDatabase database) {
 
```

**Important!** This needs to be somewhere in the base class or a test runner:

```Java
 MockitoAnnotations.openMocks(testClass);
 
```

You can use built-in runner: [`MockitoJUnitRunner`](https://javadoc.io/static/org.mockito/mockito-core/4.5.1/org/mockito/junit/MockitoJUnitRunner.html) or a rule: [`MockitoRule`](https://javadoc.io/static/org.mockito/mockito-core/4.5.1/org/mockito/junit/MockitoRule.html). For JUnit5 tests, refer to the JUnit5 extension described in [section 45](https://javadoc.io/static/org.mockito/mockito-core/4.5.1/org/mockito/Mockito.html#45).

Read more here: [`MockitoAnnotations`](https://javadoc.io/static/org.mockito/mockito-core/4.5.1/org/mockito/MockitoAnnotations.html)



### @Mock 使用示例

注意：**@Mock 注解需要搭配 MockitoAnnotations.openMocks(testClass) 一起使用**



新建一个 java 对象：

```java
public class Demo01 {
    public int add(int a, int b){
        return  a + b;
    }
}
```

```java
class demo01Test {
    @Mock
    private Random mock;

    @Test
    void add() {
        MockitoAnnotations.openMocks(this);
        System.out.println(mock.nextInt());
        verify(mock, Mockito.times(2)).nextInt();
        Mockito.when(mock.nextInt()).thenReturn(100);
        Assertions.assertEquals(1,mock.nextInt());
    }
}
```



## 2.6 @BeforeEach 与 @BeforeAfter 

- 看名字就知道这两个方法干嘛的
- Junit 5 是这两个，而 Junit 4 是对应的 @Before 与 @After

```java
class demo01Test {
    @Mock
    private Random mock;

    @BeforeEach
    void before(){
        System.out.println("测试方法执行前的执行操作======");
    }

    @Test
    void add() {
        MockitoAnnotations.openMocks(this);
        System.out.println(mock.nextInt());
        verify(mock, Mockito.times(2)).nextInt();
        Mockito.when(mock.nextInt()).thenReturn(100);

//        System.out.println(mock.nextInt());
//        Assertions.assertEquals(1,mock.nextInt());
    }

    @AfterEach
    void after(){
        System.out.println("测试方法执行前的执行操作=======");
    }
}


// 返回结果
测试方法执行前的执行操作======
0
测试方法执行前的执行操作=======
```



## 2.7 Spy 方法与 @Spy

`spy()` 与 `mock()` 不同的是：

- 被 spy 的对象会走真实的方法，而 mock 对象不会。
- spy() 方法的参数是对象实例，mock 的参数是 class 。
- spy 的对象，如果不打桩，就会走真实的对象实例的方法操作。



`spy()` 打桩的使用示例：

```java
// 打桩的
	@Spy
    private Demo01 demo01;

    @BeforeEach
    void before(){
        MockitoAnnotations.openMocks(this);
        System.out.println("测试方法执行前的执行操作======");
    }

    @Test
    void add() {
        when(demo01.add(1, 2)).thenReturn(0);
        System.out.println(demo01.add(1, 2));
        Assertions.assertEquals(3, demo01.add(1, 2));
    }

    @AfterEach
    void after(){
        System.out.println("测试方法执行前的执行操作=======");
    }

```

返回结果：

```sh
测试方法执行前的执行操作======
0
测试方法执行前的执行操作=======

org.opentest4j.AssertionFailedError: 
Expected :3
Actual   :0

```



**没有打桩的**的示例：

- 就会走**真实**的方法

```java
@Spy
    private Demo01 demo01;

    @BeforeEach
    void before(){
        MockitoAnnotations.openMocks(this);
        System.out.println("测试方法执行前的执行操作======");
    }

    @Test
    void add() {
        System.out.println(demo01.add(1, 2));
        Assertions.assertEquals(3, demo01.add(1, 2));
    }

    @AfterEach
    void after(){
        System.out.println("测试方法执行前的执行操作=======");
    }
```

返回结果：

```sh
测试方法执行前的执行操作======
3
测试方法执行前的执行操作=======
```



## 2.8 mock 静态方法 mockStatic 的使用

- 静态方法的使用只是使用方式不同，和非静态方法的功能一样的。
- 官方 API 的位置
- ![image-20220515171223967](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515171223967.png)



注意，这里 依赖需要变成：

```xml
<!-- https://mvnrepository.com/artifact/org.mockito/mockito-inline -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-inline</artifactId>
    <version>4.5.1</version>
    <scope>test</scope>
</dependency>

// 不可以再使用	mockito-core 这里依赖，不然会报错。
```



新建一个java 对象：

```java
public class StaticUtils {
    private StaticUtils() {}

    // 返回指定区间的 Integer List
    public static List<Integer> range(int start, int end){
        return IntStream.range(start, end).boxed().collect(Collectors.toList());
    }

    // 返回 Echo 字符串
    public static String name(){
        return "Echo";
    }
}
```

使用示例：

```java
class StaticUtilsTest {

    @Test
    void range() {
        MockedStatic<StaticUtils> demo = Mockito.mockStatic(StaticUtils.class);
        demo.when(() -> StaticUtils.range(2, 6)).thenReturn(Arrays.asList(10, 11, 12));
        Assertions.assertTrue(StaticUtils.range(2, 6).contains(10));
    }

    @Test
    void name() {
        MockedStatic<StaticUtils> demo = Mockito.mockStatic(StaticUtils.class);
        demo.when(StaticUtils::name).thenReturn("joker");
        Assertions.assertSame("joker", StaticUtils.name());
    }
}
```



## 2.9 总结 实战示例

步骤：

- @Spy 注入一个当前类
- 其它的相关涉及到的，这里类里使用的 java bean ，都使用 @Mock 注入
- 再@Spy 那个对象上加 @InjectMocks
- 编写测试



示例：

需要测试的类：

![image-20220515180700842](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515180700842.png)

![image-20220515180723469](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515180723469.png)



测试类：

![image-20220515180356962](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515180356962.png)

![image-20220515180514155](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220515180514155.png)



### @InjectMocks

- 这个注解就是：把其他的  @Mock 的对象，注入到当前对象。



## 2.10 Matchers

- matchers：参数匹配器

- Matchers Java API :https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/ArgumentMatchers.html

- **用处**：有时，在编写行为模拟时，您希望将任何类型作为函数的参数类型。这种情况下，您可以使用参数匹配器。Mockito 参数方法被 `org.mockito.ArgumentMatchers`定义为类中的**静态方法**。

- 注意：

  - 当我们使用参数匹配器时，所有参数都应使用匹配器。 如果要为参数使用**特定值**，则可以使用`eq()`方法。

  - 如果要使用**数组**，可以使用如下所示的 any() 方法。

    ```java
    any(byte[].class)
    any(Object[].class)
    ```

- Mockito 参数匹配器只能与 when() 和 verify() 方法一起使用。

- **AdditionalMatchers**： 都是不常用的 matchers





## 相关注解

### @InjectMocks

**@InjectMocks：创建一个实例，简单的说是这个Mock可以调用真实代码的方法，其余用@Mock（或@Spy）注解创建的mock将被注入到用该实例中。**



## Mockito 使用问题

问题一：结合 MybatisPlus 的 LambdaQuery 时，如果，Wrappers ，有加了 select 语句，就会报错（can not find lambda cache 。。。）的错，

解决：把原先正确 LambdaQuery 的注释掉，重写一个，把 select 语句去掉，就可以了。（然后再用返回的对象，使用 getter 就可以了）

# 3、JUnit 

- 单元测试用的
- 参考链接：
  - JUnit 5 官网：https://junit.org/junit5/
  - JUnit 5.8.2 API ：https://junit.org/junit5/docs/current/api/
  - Junit 5 学习笔记博客：https://vitzhou.gitbooks.io/junit5/content/
  - springboot + Junit 实践博客：https://zhuanlan.zhihu.com/p/32584548

## 3.1 JUnit 5 基本信息



### JUnit 5 常用注解

| 注解               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| @Test              | 表示该方法是一个测试方法。修饰符可以是public/protected/private。  但是修饰为private时编译器会通过，测试也不会报错，但不会跑测试，相当于将这个Test关闭。所以不建议使用private |
| @ParameterizedTest | 表示该方法是一个参数化测试                                   |
| @RepeatedTest      | 表示该方法是一个重复测试的测试模板                           |
| @TestFactory       | 表示该方法是一个动态测试的测试工厂                           |
| @TestInstance      | 用于配置所标注的测试类的 测试实例生命周期                    |
| @TestTemplate      | 表示该方法是一个测试模板，它会依据注册的 提供者所返回的调用上下文的数量被多次调用。 |
| @DisplayName       | 为测试类或测试方法声明一个自定义的显示名称。该注解不能被继承 |
| @BeforeEach        | 表示使用了该注解的方法应该在当前类中每一个使用了`@Test`、`@RepeatedTest`、`@ParameterizedTest`或者`@TestFactory`注解的方法之前 执行；类似于JUnit 4的 @Before。这样的方法会被继承，除非它们被覆盖。 |
| @AfterEach         | 表示使用了该注解的方法应该在当前类中每一个使用了`@Test`、`@RepeatedTest`、`@ParameterizedTest`或者`@TestFactory`注解的方法之后 执行；类似于JUnit 4的`@After`。这样的方法会被继承，除非它们被覆盖。 |
| @BeforeAll         | 表示使用了该注解的方法应该在当前类中**所有**使用了`@Test`、`@RepeatedTest`、`@ParameterizedTest`或者`@TestFactory`注解的方法*之前* 执行；类似于JUnit 4的 `@BeforeClass`。这样的方法会被*继承*（除非它们被*隐藏* 或*覆盖*），并且它必须是 `static`方法（除非`"per-class"` 测试实例生命周期） |
| @AfterAll          | 表示使用了该注解的方法应该在当前类中所有使用了`@Test`、`@RepeatedTest`、`@ParameterizedTest`或者`@TestFactory`注解的方法之后执行；类似于JUnit 4的 `@AfterClass`。这样的方法会被*继承*（除非它们被*隐藏* 或*覆盖*），并且它必须是 `static`方法（除非`"per-class"` 测试实例生命周期 被使用） |
|                    |                                                              |
| @Nested            | 表示使用了该注解的类是一个内嵌、非静态的测试类。`@BeforeAll`和`@AfterAll`方法不能直接在`@Nested`测试类中使用，（除非`"per-class"` 测试实例生命周期 被使用）。该注解不能被继承 |
| @Tag               | 用于声明过滤测试的tags，该注解可以用在方法或类上；类似于TesgNG的测试组或JUnit 4的分类。该注解能被继承，但仅限于类级别，而非方法级别 |
| @Disable           | 用于禁用一个测试类或测试方法；类似于JUnit 4的@Ignore。该注解不能被继承 |
| @ExtendWith        | 用于注册自定义 扩展该注解不能被*继承*。                      |



## 相关注解



### @ExtendWith

参考博客：

- https://vitzhou.gitbooks.io/junit5/content/junit/extension_model.html

- 属于 Junit 5 的注解，属于 Jupiter 模块。
- 定义：通过@ExtendWith 注解可以声明在测试方法和类的执行中启用相应的扩展。 扩展的启用是继承的，这既包括测试类本身的层次结构，也包括测试类中的测试方法。也就是说，测试类会继承其父类中的扩展，测试方法会继承其所在类中的扩展。除此之外，在一个测试上下文中，每一个扩展只能出现一次。



**应用场景：**

**当涉及Spring时：**

如果您想在测试中使用**Spring测试框架功能**（例如）`@MockBean`，则必须使用`@ExtendWith(SpringExtension.class)`。它取代了不推荐使用的JUnit4`@RunWith(SpringJUnit4ClassRunner.class)`

**当不涉及Spring时：**

例如，如果您只想涉及Mockito而不必涉及Spring，那么当您只想使用`@Mock`/ `@InjectMocks`批注时，您就想使用`@ExtendWith(MockitoExtension.class)`，因为它不会加载到很多不需要的Spring东西中。它替换了不推荐使用的JUnit4 `@RunWith(MockitoJUnitRunner.class)`。



可以只使用`@ExtendWith(SpringExtension.class)`，但是如果测试中没有涉及Spring测试框架功能，那么可以只使用`@ExtendWith(MockitoExtension.class)`。



# 4、AssertJ

- 流式断言，有更多的接口
- 参考博客：
  - AssertJ 使用博客：https://cloud.tencent.com/developer/article/1465401
  - AssertJ 官方文档：https://assertj.github.io/doc/
  - AssertJ 官方Java API ：https://www.javadoc.io/doc/org.assertj/assertj-core/latest/index.html







# ==问题：==

# @ExtendWith



# THE END 

