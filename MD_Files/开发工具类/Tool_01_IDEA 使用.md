# 目录

[toc]

# IDEA 使用

# 1、破解

- 后谈



# 2、IDEA 常用插件

1. one dark theme : 一个黑色主体，还可以改变字体的颜色。
2. Rainbow Brackets ：就是给括号加颜色，以便区分
3. VisualVM Laucher ：可以查看堆栈内存的占用情况
4. GenerateAllSetter : 可以一键生成对象所有的 set 字段
5. Translation：划词翻译



# 3、IDEA 配置 JavaDoc

## 3.1 类注释

- IDEA 配置路径: File|setting|Editor|File And Code Template|File Header
  - 这个是生成类的时候，类注释就自动生成了。

```sh
# JavaDoc-类注释模板
/**
* @description TODO
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
* @descriptoin $description$
* 
$params$
* @retrun $returns$
* @author $user$
* @date $date$
*/
```



==使用==：配置好内容后，在方法上方输入`/*m`，按下`Enter`即可添加方法注释