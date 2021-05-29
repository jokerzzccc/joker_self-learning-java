

















# 目录

[toc]

# ES 6

始于：2021-05-29

学习资源：

- ES 6 W3C school：https://www.w3cschool.cn/escript6/
  - 推荐使用，更全一些
- ES 6 菜鸟教程：https://www.runoob.com/w3cnote/es6-tutorial.html



# 1、ES 6 简介

- ES 6 较之前的版本是一个非常大的飞跃，所以，一般学它
- 可以把 ES 6 约等于 JS，但是它们不是一样的。



主要要了解 ES 6 的新特性：

- ES 6 兼容性
- let , const
- 数组的新增方法
- 箭头函数
- Map , Set 数据结构
- 字符串新增方法和模板字符串
- 函数的参数
- 解构赋值
- Class 的语法
- JSON 的新应用
- Module 模块



也可以慢慢去了解 ES 7~ES 11 的新特性



> ES 6 的作用

- 对语法的改进，功能的增加
- Vue ,React,Node.js，小程序等框架都在使用，所以必须学



# 2、let , const 关键字

- 弥补 var 关键字的不足

## 2.1 let





## 2.2 const





# 3、箭头函数

-  Arrow function
-  对于回调函数的使用 ，是非常方便的





## 3.1 箭头函数定义



## 3.2 this 指向问题



# 4、数组高级函数

- filter 过滤

- map 映射
- reduce 汇总
- some() 
- every()



# 5、Map , Set 

## 5.1 Map

### Map 的属性和操作方法

- size 属性
- set(k,v)
- get(key)
- has(key)
- delete(key)
- clear(key)

### Map 的遍历

- keys()： 返回所有键名
- values() ：返回所有的键值
- entries() ：返回键值对的遍历器
- forEach() ：使用回调函数遍历每个成员

## 5.2 Set 



# 6、字符串的扩展

## 6.1 字符串新增方法

字符串示例的新增方法：

- includes()
- startsWith()
- endsWith()
- repeat()
- trimStart()
- trimEnd()
- matchAll()

## 6.2 模板字符串

模板字符串的**定义**：

- 相当于加强版的字符串，用反引号 **`**
- 除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。



模板字符串的**作用**：

- 可以生成换行字符串 

  - ```js
    let string = `Hello'\n'world`;
    ```

- 可以插入变量和表达式

  - ```js
    let name = "joker";
    let age = 21;
    let info = `My Name is ${name},I am ${age+1} years old next year.`
    console.log(info);
    // My Name is joker,I am 21 years old next year.
    ```

  - 





# 7、Iterator 和 for...of 循环

## 7.1 Iterator

Iterator 是 ES6 引入的一种新的遍历机制，迭代器有两个核心概念：

- 迭代器是一个统一的接口，它的作用是使各种数据结构可被便捷的访问，它是通过一个键为 `Symbol.iterator `的方法来实现。
- 迭代器是用于遍历数据结构元素的指针（如数据库中的游标）。



Iterator 的作用有三个：

- 一是为各种数据结构，提供一个统一的、简便的访问接口；

- 二是使得数据结构的成员能够按某种次序排列；

- 三是 ES6 创造了一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费。



> 原生具备 Iterator 接口的数据结构如下。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

## 7.2 for...of





# 8、解构赋值

解构 定义：ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值。

解构赋值**要求**：

- 左右两边必须结构一样
- 右边必须有值
- 声明和赋值不能分开



解构赋值允许指定**默认值**



## 8.1 数组的解构赋值



## 8.2 对象的解构赋值



## 8.3 字符串的解构赋值



## 8.4 数值和布尔值的解构赋值





# 9、运算符的扩展

- `...`三点运算符
  - 展开数组
  - 默认参数
- 



# 10、Class 类的概念



## 10.1 constructor 构造方法



## 10.2 this 关键字代表实例对象



## 10.3 取值函数（getter）和存值函数（setter）



## 10.4 extends ，super 关键字





# 11、JSON 对象的新应用



- JSON.stringfy() 串行化（JSON 对象转成字符串）
- JSON.parse() 反串行化
- 简写：
  - （属性和值）名字一样可以简写
  - 方法一样可以简写（可以去掉 `:function`）





# 12、Module 模块化

`模块`功能主要由两个命令构成：`export`和`import` 。 

- export 命令用于规定模块的对外接口，
- import 命令用于输入其他模块提供的功能。



module 模块化的**优点**：

- 减少命名冲突；
- 避免引入时的层层依赖
- 可以提升执行效率



## 12.1 export 命令

一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。

如果你希望外部能够读取模块内部的某个变量，就必须使用 export 关键字输出该变量。





## 12.2 import 命令

import 命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（ profile.js ）对外接口的名称相同。



# 13、Promise 对象



# 14、Generator 函数



# 15、Reflect 对象

Reflect 对象 的**作用**：

（1） 将 Object 对象的一些明显属于语言内部的方法（比如 Object.defineProperty ），放到 Reflect 对象上。现阶段，某些方法同时在 Object 和 Reflect 对象上部署，未来的新方法将只部署在 Reflect 对象上。也就是说，从 Reflect 对象上可以拿到语言内部的方法。

（2） 修改某些 Object 方法的返回结果，让其变得更合理。比如， Object.defineProperty(obj, name, desc) 在无法定义属性时，会抛出一个错误，而 Reflect.defineProperty(obj, name, desc) 则会返回 false 。



# 16、Proxy 对象

`Proxy`用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种`“元编程”（`meta programming），即对编程语言进行编程。

`Proxy`可以理解成，在目标对象之前架设一层`“拦截”`，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。



# OVER

