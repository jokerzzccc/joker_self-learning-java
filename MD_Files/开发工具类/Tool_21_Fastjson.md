# 目录

[toc]

# Tool_21_Fastjson

- 时间：2022-07-15
- fastjson version 1.2.49
- 参考链接：
  - fastjson 1.2.83 官方文档：https://github.com/alibaba/fastjson/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98
  - fastjson API （oracle 官方文档）：https://javadoc.io/doc/com.alibaba/fastjson/1.2.80/com/alibaba/fastjson/JSON.html
  - fastjson API （中文版 W3CSchool）：https://www.w3cschool.cn/fastjson/fastjson-api.html
  - fastjson 最佳实践：http://i.kimmking.cn/2017/06/06/json-best-practice/



# 1、介绍

- 作用：操作 JSON 对象，主要用于**序列化与反序列化**



maven 仓库：

```xml
<dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>fastjson</artifactId>
     <version> 1.2.70</version>
</dependency>
```



- fastjson入口类是com.alibaba.fastjson.JSON，**主要的API**是JSON.toJSONString，和parseObject。



## 1.1 基本使用

实体类：

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class JSONEntity {

    @JSONField(name = "ID")
    private Integer id;

    @JSONField(name = "NAME")
    private String name;

    @JSONField(name = "PASSWORD")
    private String pwd;

    @JSONField(format = DateUtil.DATE_YYYYMMDD)
    private LocalDateTime time;

    private UserEntity userEntity;

    private List<StudentEntity> studentEntityList;

}
```



使用测试：

```java
@Test
void testFastjson(){
    JSONEntity entity = JSONEntity.builder()
            .id(1)
            .name("cbl")
            .time(LocalDateTime.now())
            .build();

    String beanToJsonStr = JSON.toJSONString(entity);
    JSONEntity strToBean = JSON.parseObject(beanToJsonStr, JSONEntity.class);

    log.info("实体类 : {}", entity);
    log.info("序列化：实体 转 json : {}: ", beanToJsonStr);
    log.info("反序列化：jsonStr 转 bean : {}", strToBean);
}
```

结果：

![image-20220716195445278](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220716195445278.png)



# 2、注解介绍

## 2.1 @JSONField

- 参考链接：
  - https://github.com/alibaba/fastjson/wiki/JSONField
  - https://blog.csdn.net/u013541707/article/details/108336497



- 作用：**定制**实体字段的序列化与反序列化。



### 1、源码

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER })
public @interface JSONField {
  		int ordinal() default 0; //是根据fieldName的字母序进行序列的，你可以通过ordinal指定字段的顺序

        String name() default ""; //序列化和反序列化时候的别名

        String format() default ""; //用于字符串格式的日期转换

        boolean serialize() default true; // 是否参与序列化

        boolean deserialize() default true; //是否参与反序列化

        SerializerFeature[] serialzeFeatures() default {}; //序列化选项 SerializerFeature.WriteNullNumberAsZero 如空Number填充0

        Feature[] parseFeatures() default {}; //反序列化选项

        String label() default ""; //标签，

        boolean jsonDirect() default false; //当你有⼀个字段是json字符串的数据，你希望直接输出，⽽不是经过转义之后再输出。

        Class<?> serializeUsing() default Void.class; // 属性的序列化类，可定制。可有现存的，比如本来是Long，序列化的时候转为String：serializeUsing= ToStringSerializer.class

    	Class<?> deserializeUsing() default Void.class; // 属性的反序列化类，可定制。

    	String[] alternateNames() default {}; //参与反序列化时候的别名

        boolean unwrapped() default false; // 对象映射到父对象上。不进行子对象映射。简单而言，就是属性为对象的时候，属性对象里面的属性直接输出当做父对象的属性输出

        String defaultValue() default ""; //设置默认值
}

```



### 2、简单几个

- ordinal 、name 、serialize、 deserialize、 format、defaultValue

使用示例：

```java
public class JSONController {
    public static void main(String[] args) {
        Nation nationBean1 = Nation.builder().dress("现代服饰").num(1314).build();
        String str = JSON.toJSONString(nationBean1);
        System.out.println(str);                            //{"name":"汉族","number":1314}
        Nation nation = JSON.parseObject(str, Nation.class);
        System.out.println(JSON.toJSONString(nation));      //{"name":"汉族"}
    }
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    @JSONField(defaultValue = "汉族")
    private String name;

    @JSONField(ordinal = 1, name = "DRESS", serialize = false)
    private String dress;

    @JSONField(ordinal = 2, name = "number", deserialize = false)
    private Integer num;

    @JSONField(ordinal = 3, format = "yyyy-MM-dd")
    private Date celebrateHoliday;
}

```





### 2、serialzeFeatures、parseFeatures

他们是序列化、反序列化时候的一些可选的特征：例如

- 序列化的时候,
  -  比如fastjson默认是不会将为null的属性输出的，若是我们也想输出，可以加入`	@JSONField(serialzeFeatures = SerializerFeature.WriteMapNullValue)`。
  - 还有就是，数值型为null的话，就输出0，可以使用`@JSONField(serialzeFeatures = SerializerFeature.WriteNullNumberAsZero)`

- 反序列化的时候，
  比如parser是否将允许使用非双引号属性名字。`@JSONField(parseFeatures = Feature.AllowSingleQuotes)`



使用示例：

```java
public class JSONController {
    public static void main(String[] args) {
        Nation nationBean1 = Nation.builder().name("汉族").build();
        String str = JSON.toJSONString(nationBean1);
        System.out.println(str);
        //{"celebrateHoliday":null,"name":"汉族","num":0}
    }
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    private String name;
    private String dress;

    @JSONField(serialzeFeatures = SerializerFeature.WriteNullNumberAsZero)
    private Integer num;

    @JSONField(serialzeFeatures = SerializerFeature.WriteMapNullValue)
    private Date celebrateHoliday;
}
```



具体可设置的特性源码：

1、**序列化时的特性**：serialzeFeatures 对应的 SerializerFeature 源码：

```java
public enum SerializerFeature {
    QuoteFieldNames,
    UseSingleQuotes,
    WriteMapNullValue,
    WriteEnumUsingToString,
    WriteEnumUsingName,
    UseISO8601DateFormat,
    WriteNullListAsEmpty,
    WriteNullStringAsEmpty,
    WriteNullNumberAsZero,
    WriteNullBooleanAsFalse,
    SkipTransientField,
    SortField,
    /** @deprecated */
    @Deprecated
    WriteTabAsSpecial,
    PrettyFormat,
    WriteClassName,
    DisableCircularReferenceDetect,
    WriteSlashAsSpecial,
    BrowserCompatible,
    WriteDateUseDateFormat,
    NotWriteRootClassName,
    /** @deprecated */
    DisableCheckSpecialChar,
    BeanToArray,
    WriteNonStringKeyAsString,
    NotWriteDefaultValue,
    BrowserSecure,
    IgnoreNonFieldGetter,
    WriteNonStringValueAsString,
    IgnoreErrorGetter,
    WriteBigDecimalAsPlain,
    MapSortField;
    
    // 省略方法
    ...
}
```



**解释：**

```java
QuoteFieldNames 输出key时是否使用双引号,默认为true
UseSingleQuotes 使用单引号而不是双引号,默认为false
WriteMapNullValue 是否输出值为null的字段,默认为false
WriteEnumUsingToString Enum输出name()或者original,默认为false
UseISO8601DateFormat Date使用ISO8601格式输出，默认为false
WriteNullListAsEmpty List字段如果为null,输出为[],而非null
WriteNullStringAsEmpty 字符类型字段如果为null,输出为”“,而非null
WriteNullNumberAsZero 数值字段如果为null,输出为0,而非null
WriteNullBooleanAsFalse Boolean字段如果为null,输出为false,而非null
SkipTransientField 如果是true，类中的Get方法对应的Field是transient，序列化时将会被忽略。默认为true
SortField 按字段名称排序后输出。默认为false
WriteTabAsSpecial 把\t做转义输出，默认为false不推荐设为true
PrettyFormat 结果是否格式化,默认为false
WriteClassName 序列化时写入类型信息，默认为false。反序列化是需用到
DisableCircularReferenceDetect 消除对同一对象循环引用的问题，默认为false
WriteSlashAsSpecial 对斜杠’/’进行转义
BrowserCompatible 将中文都会序列化为\uXXXX格式，字节数会多一些，但是能兼容IE 6，默认为false
WriteDateUseDateFormat 全局修改日期格式,默认为false。
DisableCheckSpecialChar 一个对象的字符串属性中如果有特殊字符如双引号，将会在转成json时带有反斜杠转移符。如果不需要转义，可以使用这个属性。默认为false
BeanToArray 将对象转为array输出

```



2、**反序列化时**，parseFeatures 对应的 Feature 的可选特性源码：

```java
package com.alibaba.fastjson.parser;

public enum Feature {
    AutoCloseSource,
    AllowComment,
    AllowUnQuotedFieldNames,
    AllowSingleQuotes,
    InternFieldNames,
    AllowISO8601DateFormat,
    AllowArbitraryCommas,
    UseBigDecimal,
    IgnoreNotMatch,
    SortFeidFastMatch,
    DisableASM,
    DisableCircularReferenceDetect,
    InitStringFieldAsEmpty,
    SupportArrayToBean,
    OrderedField,
    DisableSpecialKeyDetect,
    UseObjectArray,
    SupportNonPublicField,
    IgnoreAutoType,
    DisableFieldSmartMatch,
    SupportAutoType,
    NonStringKeyAsString,
    CustomMapDeserializer,
    ErrorOnEnumNotMatch,
    SafeMode;
    
    // 省略
    ...
}
```



### 3、label

- 作用：可以给属性设置标签，这样可以批量处理某一类的属性，比如不序列化某一类属性。

```java
public class JSONController {
    public static void main(String[] args) {
        Nation nationBean1 = Nation.builder().name("汉族").dress("便服").num(12).celebrateHoliday(new Date()).build();
        String str = JSON.toJSONString(nationBean1, Labels.includes("a"));
        System.out.println(str); //{"num":12}
        String str2 = JSON.toJSONString(nationBean1, Labels.excludes("a"));
        System.out.println(str2); //{"celebrateHoliday":1598929877786,"dress":"便服","name":"汉族"}
    }
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    private String name;

    private String dress;

    @JSONField(label = "a")
    private Integer num;

    @JSONField(label = "b")
    private Date celebrateHoliday;
}

```

### 4、jsonDirect

- 作用是：当你有⼀个字段是json字符串的数据，你希望直接输出，而不是经过转义之后再输出。
- 默认为 false

```java
public class JSONController {
    public static void main(String[] args) {
        Nation nationBean1 = Nation.builder().name("{}").dress("{}").build();
        String str = JSON.toJSONString(nationBean1);
        System.out.println(str); //{"dress":"{}","name":{}}

    }
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    @JSONField(jsonDirect = true)
    private String name;
    @JSONField(jsonDirect = false)
    private String dress;
    private Integer num;
    private Date celebrateHoliday;
}
```



### 5、serializeUsing和deserializeUsing

- 作用：可定制的序列化和反序列化的类，但是也有原生的。
  - 比如原生：比如字段本来是Long，序列化的时候转为String。
  - 比如自定义：我对某个字段加上我想要的处理结果“中国的”

```java
public class JSONController {
    public static void main(String[] args) {
        Nation nationBean1 = Nation.builder().name("汉族").num(2323).build();
        String str = JSON.toJSONString(nationBean1);
        System.out.println(str);
        //{"name":"中国的汉族","num":"2323"}

    }

    public static class MySerializer implements ObjectSerializer {
        @Override
        public void write(JSONSerializer serializer, Object object, Object fieldName, Type fieldType, int features) throws IOException {
            String text = "中国的" + (String) object;
            serializer.write(text);
        }
    }
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    @JSONField(serializeUsing = JSONController.MySerializer.class)
    private String name;
    private String dress;
    @JSONField(serializeUsing = ToStringSerializer.class)
    private Integer num;
    private Date celebrateHoliday;
}

```



### 6、alternateNames

- 作用：反序列化时候的别名，可以写多个。

```java
public class JSONController {
    public static void main(String[] args) {
        String str ="{\"Name\":\"汉族\",\"num\":2323}";
        System.out.println(JSON.toJSONString(JSON.parseObject(str, Nation.class)));
        //{"name":"汉族","num":2323}
    }

}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    @JSONField(alternateNames = {"name", "Name"})
    private String name;
    private String dress;
    private Integer num;
    private Date celebrateHoliday;
}

```



### 7、unwrapped

- 作用：对象映射到父对象上。不进行子对象映射。简单而言，就是属性为对象的时候，属性对象里面的属性直接输出当做父对象的属性输出
- 默认为 false

```java
public class JSONController {
    public static void main(String[] args) {
        QSM qsm = new QSM();
        qsm.setName("传闻中的陈芊芊");
        qsm.setCity("花垣城");
        QSM qsm2 = new QSM();
        qsm2.setName("传闻中的韩烁");
        qsm2.setCity("玄虎城");

        Nation nation1 = Nation.builder().name("中国").qsm(qsm).qsm2(qsm2).build();
        System.out.println(JSON.toJSONString(nation1));
        //{"name":"中国","city":"花垣城","name":"传闻中的陈芊芊","qsm2":{"city":"玄虎城","name":"传闻中的韩烁"}}
    }

}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
class Nation {
    private String name;

    @JSONField(unwrapped = true)
    private QSM qsm;

    @JSONField(unwrapped = false)
    private QSM qsm2;
}

@Data
class QSM {
    String name;
    String city;
}

```



## 2.2 @JSONType

- 和JSONField类似，但 JSONType **配置在类上**，而不是field或者getter/setter方法上。



### 1、源码

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
public @interface JSONType {
    boolean asm() default true;

    String[] orders() default {};

    String[] includes() default {};

    String[] ignores() default {};

    SerializerFeature[] serialzeFeatures() default {};

    Feature[] parseFeatures() default {};

    boolean alphabetic() default true;

    Class<?> mappingTo() default Void.class;

    Class<?> builder() default Void.class;

    String typeName() default "";

    String typeKey() default "";

    Class<?>[] seeAlso() default {};

    Class<?> serializer() default Void.class;

    Class<?> deserializer() default Void.class;

    boolean serializeEnumAsJavaBean() default false;

    PropertyNamingStrategy naming() default PropertyNamingStrategy.CamelCase;

    Class<? extends SerializeFilter>[] serialzeFilters() default {};
}
```







# 3、 定制序列化

参考链接：

- https://github.com/alibaba/fastjson/wiki/%E5%AE%9A%E5%88%B6%E5%BA%8F%E5%88%97%E5%8C%96

fastjson支持多种方式定制序列化：

+ 通过 @JSONField 定制序列化，见上注解
+ 通过 @JSONType 定制序列化，见上注解
+ 通过 SerializeFilter 定制序列化
+ 通过 ParseProcess 定制反序列化



## 3.1 SerializeFilter 定制序列化

### 1、介绍

- SerializeFilter 是一个接口



通过SerializeFilter可以使用**扩展编程的方式实现定制序列化**。fastjson提供了多种SerializeFilter：

+ PropertyPreFilter 根据PropertyName判断是否序列化
+ PropertyFilter 根据PropertyName和PropertyValue来判断是否序列化
+ NameFilter 修改Key，如果需要修改Key,process返回值则可
+ ValueFilter 修改Value
+ BeforeFilter 序列化时在最前添加内容
+ AfterFilter 序列化时在最后添加内容

以上的SerializeFilter在JSON.toJSONString中可以使用。

```
  SerializeFilter filter = ...; // 可以是上面5个SerializeFilter的任意一种。
  JSON.toJSONString(obj, filter);
```

更多看这里： https://github.com/alibaba/fastjson/wiki/SerializeFilter



### 2、源码

SerializeFilter 的实现类如下：

![image-20220716201411543](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220716201411543.png)



### 3、实现类或接口介绍

参考链接：

- https://github.com/alibaba/fastjson/wiki/SerializeFilter
- SerializeFilter 的实现类的使用博客：https://blog.csdn.net/liupeifeng3514/article/details/79167734

#### 1、**PropertyFilter** 

- **根据PropertyName和PropertyValue来判断是否序列化**

接口：

```java
  public interface PropertyFilter extends SerializeFilter {
      boolean apply(Object object, String propertyName, Object propertyValue);
  }
```

可以通过扩展实现根据object或者属性名称或者属性值进行判断是否需要序列化。例如：

```java
package json.fastjson.SerializeFilter;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.PropertyFilter;

public class TestPropertyFilter {

    public static void main(String[] args) {
        PropertyFilter filter = new PropertyFilter() {

            @Override
            public boolean apply(Object source, String name, Object value) {

                System.out.println("----------------source=" + source);
                System.out.println("----------------name=" + name);
                System.out.println("----------------value=" + value);
                System.out.println("");
                // 属性是id并且大于等于100时进行序列化
                if ("id".equals(name)) {
                    long id = ((Long) value).longValue();
                    return id >= 100;
                }
                return false;
            }
        };

        User user = new User();
        user.setId(9L);
        user.setName("挖坑埋你");

        String jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("序列化,id=9：" + jsonString + "\n");

        user.setId(200L);
        jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("序列化,id=200：" + jsonString);
    }

}

```

输出结果：

```sh
----------------source=User [id=9, name=挖坑埋你]
----------------name=id
----------------value=9

----------------source=User [id=9, name=挖坑埋你]
----------------name=name
----------------value=挖坑埋你

序列化,id=9：{}

----------------source=User [id=200, name=挖坑埋你]
----------------name=id
----------------value=200

----------------source=User [id=200, name=挖坑埋你]
----------------name=name
----------------value=挖坑埋你

序列化,id=200：{"id":200}
```

#### 2、**NameFilter** 

- **序列化时修改Key**

NameFilter 接口：

```java
public interface NameFilter extends SerializeFilter {
    String process(Object object, String propertyName, Object propertyValue);
}
```

如果需要修改Key，process返回值则可，process 返回值就是 key 值。例如：

```java
package json.fastjson.SerializeFilter;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.NameFilter;
import com.alibaba.fastjson.serializer.PascalNameFilter;

public class TestNameFilter {

    public static void main(String[] args) {
        User user = new User();
        user.setId(9L);
        user.setName("挖坑埋你");

        String jsonString = JSON.toJSONString(user); // 序列化的时候传入filter
        System.out.println("普通序列化：" + jsonString + "\n");

        NameFilter filter = new NameFilter() {

            @Override
            public String process(Object object, String name, Object value) {
                System.out.println("----------------object=" + object);
                System.out.println("----------------name=" + name);
                System.out.println("----------------value=" + value);
                System.out.println("");
                // 属性是id是修改id的名字
                if ("id".equals(name)) {
                    return name + "$";
                }
                return name;
            }
        };

        jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("NameFilter序列化：" + jsonString + "\n");

        // fastjson内置一个PascalNameFilter，用于输出将首字符大写的Pascal风格
        jsonString = JSON.toJSONString(user, new PascalNameFilter()); // 序列化的时候传入filter
        System.out.println("PascalNameFilter序列化：" + jsonString + "\n");
    }

}

```

输出结果：

```sh
普通序列化：{"id":9,"name":"挖坑埋你"}

----------------object=User [id=9, name=挖坑埋你]
----------------name=id
----------------value=9

----------------object=User [id=9, name=挖坑埋你]
----------------name=name
----------------value=挖坑埋你

NameFilter序列化：{"id$":9,"name":"挖坑埋你"}

PascalNameFilter序列化：{"Id":9,"Name":"挖坑埋你"}
```



#### 3、**ValueFilter** 

- **序列化时修改Value**

ValueFilter 接口：

```java
  public interface ValueFilter extends SerializeFilter {
      Object process(Object object, String propertyName, Object propertyValue);
  }
```

如果需要修改Value，process返回值则可，例如：

```java
package json.fastjson.SerializeFilter;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.ValueFilter;

public class TestValueFilter {

    public static void main(String[] args) {
        User user = new User();
        user.setId(9L);
        user.setName("挖坑埋你");

        String jsonString = JSON.toJSONString(user); // 序列化的时候传入filter
        System.out.println("普通序列化：" + jsonString + "\n");

        ValueFilter filter = new ValueFilter() {

            @Override
            public Object process(Object object, String name, Object value) {
                System.out.println("----------------object=" + object);
                System.out.println("----------------name=" + name);
                System.out.println("----------------value=" + value);
                System.out.println("");
                // 属性是id时修改id的值
                if ("id".equals(name)) {
                    long id = ((Long) value).longValue();
                    return id + "$";
                }
                return value;
            }
        };

        jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("ValueFilter序列化：" + jsonString + "\n");
    }

}
```

输出结果：

```sh
普通序列化：{"id":9,"name":"挖坑埋你"}

----------------object=User [id=9, name=挖坑埋你]
----------------name=id
----------------value=9

----------------object=User [id=9, name=挖坑埋你]
----------------name=name
----------------value=挖坑埋你

ValueFilter序列化：{"id":"9$","name":"挖坑埋你"}
```



#### 4、**BeforeFilter** 

- **序列化时在最前添加内容**：即序列化之前需要对实体执行的操作。

BeforeFilter 接口：

```java
  public abstract class BeforeFilter implements SerializeFilter {
      protected final void writeKeyValue(String key, Object value) { ... }
      // 需要实现的抽象方法，在实现中调用writeKeyValue添加内容
      public abstract void writeBefore(Object object);
  }
```

在序列化对象的所有属性之前执行某些操作，例如调用 writeKeyValue 添加内容：

```java

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.BeforeFilter;

public class TestBeforeFilter {

    public static void main(String[] args) {
        User user = new User();
        user.setId(9L);
        user.setName("挖坑埋你");

        String jsonString = JSON.toJSONString(user); // 序列化的时候传入filter
        System.out.println("普通序列化：" + jsonString + "\n");

        BeforeFilter filter = new BeforeFilter() {

            @Override
            public void writeBefore(Object object) {

                System.out.println("----------------object=" + object);

                User user = (User) object;
                System.out.println("----------------User.id=" + user.getId() + " " + "User.name=" + user.getName() + "\n");
                user.setName(user.getName() + "$$$");
            }
        };

        jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("BeforeFilter序列化：" + jsonString + "\n");
    }

}
```



输出结果：

```sh
普通序列化：{"id":9,"name":"挖坑埋你"}

----------------object=User [id=9, name=挖坑埋你]
----------------User.id=9 User.name=挖坑埋你

BeforeFilter序列化：{"id":9,"name":"挖坑埋你$$$"}
```





#### 5、**AfterFilter** 

- ##### **序列化时在最后添加内容**：序列化之后，再改变实体类。

AfterFilter 接口：

```java
  public abstract class AfterFilter implements SerializeFilter {
      protected final void writeKeyValue(String key, Object value) { ... }
      // 需要实现的抽象方法，在实现中调用writeKeyValue添加内容
      public abstract void writeAfter(Object object);
  }
```

在序列化对象的所有属性之后执行某些操作,例如调用 writeKeyValue 添加内容，例如：

```java
package json.fastjson.SerializeFilter;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.AfterFilter;

public class TestAfterFilter {

    public static void main(String[] args) {
        User user = new User();
        user.setId(9L);
        user.setName("挖坑埋你");

        String jsonString = JSON.toJSONString(user); // 序列化的时候传入filter
        System.out.println("普通序列化：" + jsonString + "\n");

        AfterFilter filter = new AfterFilter() {

            @Override
            public void writeAfter(Object object) {

                User user = (User) object;
                System.out.println("------------User.id=" + user.getId() + " " + "User.name=" + user.getName() + "\n");
                user.setName(user.getName() + "$$$");
            }
        };

        jsonString = JSON.toJSONString(user, filter); // 序列化的时候传入filter
        System.out.println("AfterFilter序列化：" + jsonString + "\n");
        System.out.println(user);
    }

}

```



输出结果：

```sh
普通序列化：{"id":9,"name":"挖坑埋你"}

------------User.id=9 User.name=挖坑埋你

AfterFilter序列化：{"id":9,"name":"挖坑埋你"}

User [id=9, name=挖坑埋你$$$]

```



## 3.2 ParseProcess 定制反序列化

参考链接：

- 官方介绍：https://github.com/alibaba/fastjson/wiki/ParseProcess	
- 使用博客：https://blog.csdn.net/liupeifeng3514/article/details/79179642

### 1、介绍

ParseProcess是**编程扩展定制反序列化**的接口。fastjson支持如下ParseProcess：

+ ExtraProcessor 用于处理多余的字段
+ ExtraTypeProvider 用于处理多余字段时提供类型信息

ParseProcess 的实现接口/类：

![image-20220716212433578](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20220716212433578.png)

### 2、实现类/接口的使用

示例对象：

```java
package json.fastjson.ParseProcess;

import java.util.HashMap;
import java.util.Map;

public class VO {
    private int id;
    private Map<String, Object> attributes = new HashMap<String, Object>();

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Map<String, Object> getAttributes() {
        return attributes;
    }

    @Override
    public String toString() {
        return "VO [id=" + id + ", attributes=" + attributes + "]";
    }
}
```



#### **ExtraProcessor** 

- ExtraProcessor 用于处理多余的字段

使用示例：

```java
package json.fastjson.ParseProcess;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.parser.deserializer.ExtraProcessor;

public class TestExtraProcessor {

    public static void main(String[] args) {

        ExtraProcessor processor = new ExtraProcessor() {
            public void processExtra(Object object, String key, Object value) {

                System.out.println("---------------object = " + object);
                System.out.println("---------------key = " + key);
                System.out.println("---------------value = " + value);
                System.out.println();

                VO vo = (VO) object;
                vo.setId(789);// 修改一下id值
                vo.getAttributes().put(key, value);
            }
        };
        // 这里name和phone是多余的，在VO里没有
        VO vo = JSON.parseObject("{\"id\":123,\"name\":\"abc\",\"phone\":\"18603396954\"}", VO.class, processor);

        System.out.println("vo.getId() = " + vo.getId());
        System.out.println("vo.getAttributes().get(\"name\") = " + vo.getAttributes().get("name"));
        System.out.println("vo.getAttributes().get(\"phone\") = " + vo.getAttributes().get("phone"));
    }

}

```



输出结果：

```sh
---------------object = VO [id=123, attributes={}]
---------------key = name
---------------value = abc

---------------object = VO [id=789, attributes={name=abc}]
---------------key = phone
---------------value = 18603396954

vo.getId() = 789
vo.getAttributes().get("name") = abc
vo.getAttributes().get("phone") = 18603396954

```



#### **ExtraTypeProvider** 

- ExtraTypeProvider 用于处理多余字段时提供类型信息

使用示例：

```java
package json.fastjson.ParseProcess;

import java.lang.reflect.Type;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.parser.deserializer.ExtraProcessor;
import com.alibaba.fastjson.parser.deserializer.ExtraTypeProvider;

public class TestExtraTypeProvider {

    public static void main(String[] args) {

        ExtraProcessor processor = new MyExtraProcessor();

        VO vo = JSON.parseObject("{\"id\":123,\"value\":\"123456\"}", VO.class, processor);

        System.out.println("vo.getId() = " + vo.getId());
        System.out.println("vo.getAttributes().get(\"value\") = " + vo.getAttributes().get("value"));
        System.out.println("vo.getAttributes().get(\"value\").getClass() = " + vo.getAttributes().get("value").getClass());
        // value本应该是字符串类型的，通过getExtraType的处理变成Integer类型了。
    }

}

class MyExtraProcessor implements ExtraProcessor, ExtraTypeProvider {
    public void processExtra(Object object, String key, Object value) {
        VO vo = (VO) object;
        vo.getAttributes().put(key, value);
    }

    public Type getExtraType(Object object, String key) {

        System.out.println("---------------object = " + object);
        System.out.println("---------------key = " + key);
        System.out.println();

        if ("value".equals(key)) {
            return int.class;
        }
        return null;
    }
};
```

输出结果：

```sh
---------------object = VO [id=123, attributes={}]
---------------key = value

vo.getId() = 123
vo.getAttributes().get("value") = 123456
vo.getAttributes().get("value").getClass() = class java.lang.Integer

```









# THE END