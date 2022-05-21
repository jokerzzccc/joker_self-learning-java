# 目录

[toc]

# IDEA 使用

- IDEA 2022.1 版本，官方使用教程：https://www.jetbrains.com/help/idea/getting-started.html

# 1、破解

- 后谈

### 一般就是三个破解方式：

1. jetbrains-agent（已停止维护）
2. **BetterIntelliJ**(据说后面被恶意植入病毒)
3. eval reset (无限重置激活版)（====）

### 1.2 IDEA 各个版本下载地址

- IDEA 下载地址：https://www.jetbrains.com/zh-cn/idea/download/other.html

# 2、IDEA 常用插件

1. one dark theme : 一个黑色主体，还可以改变字体的颜色。
2. Rainbow Brackets ：就是给括号加颜色，以便区分
3. VisualVM Laucher ：可以查看堆栈内存的占用情况
4. GenerateAllSetter : 可以一键生成对象所有的 set 字段
5. Translation：划词翻译
6. Lombok：实体类生成 setter | getter
7. Alibaba Java coding guide line ：编码规约扫描
8. Squaretest ：生成测试类（付费）
9. Spring Initializer and Assisdant：社区版的 IDEA 生成 springboot 项目
10. MybatisX ：方便Mybatis 项目 mapper 与 xml 文件跳转的
11. 



# 3、IDEA 配置 JavaDoc

## 3.1 类注释

- IDEA 配置路径: File|setting|Editor|File And Code Template|File Header
  - 这个是生成类的时候，类注释就自动生成了。

```sh
# JavaDoc-类注释模板
/**
* <p>
* TODO
* </p>
* 
* @author ${USER}
* @date ${DATE}
*/
```



## 3.2 方法注释

- IDEA 配置路径: File|setting|Editor|Live Templates

  - 点击右边的 + 号新建一个 group (比如：MyGroup)

  - 再点击右边的 + 号：新建一个 template

  - 输入模板后，点击 edit variables , 填写对应值：

    - params -default value: 

      - ```sh
        groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+='* @param: ' + params[i] + ((i < params.size() - 1) ? '\\n ' : '')};return result", methodParameters())
        ```

    - returns-default value:

      - 

```sh
# JavaDoc-方法注释模板

**
* <p>
* $description$
* </P>
* 
$params$
* @retrun $returns$
* @author $user$
* @date $date$
*/
```



==使用==：配置好内容后，在方法上方输入`/*m`，按下`Enter`即可添加方法注释





# 4、文本搜索（ctrl + shift + F ）快捷键冲突

- 如果安装了搜狗输入法，应该就是和搜狗的快捷键冲突了
- 解决：把搜狗的那个快捷键取消掉。





# 5、IDEA  布置热部署



## 热部署生效范围







> 注意 2022 版

IDEA 2022版热部署，需要在setting |  advance setting | allow auto-make to start... 

## 问题

- 问题：发送服务，经常postman 界面显示 `can not send request`。

1. 如果有用 mybatis，需要把 配置类的

   ```yaml
   mybatis:
   	mapper-location: classpath:
   	
   # 更改为：
   mybatis:
   	mapper-location: classpath*:
   ```