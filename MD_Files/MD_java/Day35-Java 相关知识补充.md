# 目录

[toc]



# Day35-Java 相关知识补充

时间：2022年4月27日



# 1、VO，PO，TO等实体类之间的转换

## 1.1 用 xxx.copyProperties()

- 参考博客：https://www.cnblogs.com/xinglongbing521/p/10145440.html
- 官方接口：https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/BeanUtils.html

- 最好用 org.springframework.beans的，避免用 Apache 的
- 

## springframe 与 apache 的接口对比

1. 无论是org.springframework.beans或者org.apache.commons.beanutils，与get/set方式相比，都存在性能问题。

2. 效率由高到底：get/set 》PropertyUtils 》BeanUtils。

3. PropertyUtils和BeanUtils两个工具类都是对bean之间存在属性名相同的属性进行处理，无论是源bean或者是目标bean中多出来的属性均不处理。

4. 具体来说：

   BeanUtils.copyProperties()可以在一定范围内进行类型转换，同时还要注意一些不能转换时候，会将默认null值转化成0;

   Property.copyProperties()则是严格的类型转化，必须类型和属性名完全一致才转化。

   对于null的处理：PropertyUtils支持为null的场景；BeanUtils对部分属性不支持null，具体如下：

   a. java.util.Date类型不支持,但是它的子类java.sql.Date是被支持的。java.util.Date直接copy会报异常；

   b. Boolean，Integer，Long等不支持，会将null转化为0；

   c. String支持，转化后依然为null。

5. BeanUtils的高级功能org.apache.commons.beanutils.Converter接口可以自定义类型转化，也可以对部分类型数据的null值进行特殊处理，如ConvertUtils.register(new DateConverter(null), java.util.Date.class);但是PropertyUtils没有。



另外：值得注意的是，在测试过程中发现，commons-beanutils-1.8.0.jar版本中的BeanUtils类，支持Byte到Integer或int的转化。说明实际使用过程中，我们还是要多看源码，多做测试，并且注意版本号升级带来的微小变化。



两者接口的区别：

1、package org.springframework.beans;中的BeanUtils.copyProperties(A,B);是A中的值付给B

2、package org.apache.commons.beanutils;中的BeanUtils.copyProperties(A,B);是B中的值付给A



## Springframework 的 copyProperties() 

1、属性名相同，类型相同 可以被复制

2、基本类型 与 其对应的封装类型 可以被复制

3、封装类型 与 其对应的基本类型 可以被复制

4、其他统统不行

例如：

         Integer->Long 
         int->long  
         Date->String等

5、source会覆盖掉target中原有的值

6、如果希望哪个属性不被复制，使用重载方法

public static void copyProperties(Object source, Object target, String... ignoreProperties) throws BeansException
ignoreProperties传属性名称。

7、source 与 target 都是不能为null的，会报错。

8、复制实现是靠set、get，所以实体中的字段要有这两个方法，没有是不会被复制赋值的。



## ==注意事项：==

1. 实体类需要**无参构造**：因为反射构建实例的缘故

2. 实体类需要 **getter/setter**

3. 属性的命名上必须统一，所以系统中各属性的命名规则必须严格规范。

4. 不好除错，例如属性中有Enum型态，但资料库为字串等型态不一致，或是Entity类与DTO类命名不一致等也会造成属性结果为null的情况。

5. 

   

   

   













# THE END