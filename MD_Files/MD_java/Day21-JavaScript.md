















# Day21-JavaScript

始于：2021-03-01

学习来源：狂神；菜鸟教程



前端知识体系：

- 逻辑：
  - 判断
  - 循环
- 事件
  - BOM浏览器事件：window,document
  - DOM事件：增，删，遍历，修改节点元素内容
  - JQuery



- 视图：
  - html
  - css
  - 框架：bootsrap , laiUI , easyUI...vue
- 通信
  - xhr
  - ajax

js 包含：基础语法，BOM ，DOM，



# 1、什么是 JavaScript 

## 入门

==必须精通 JavaScript==

由 <script>...</script> 包含的代码就是JavaScript代码，它将==直接被浏览器执行==。



第二种方法是把JavaScript代码放到一个单独的.js文件，然后在HTML中通过 <script src="..."> </script> 引入这个文件：

JavaScript代码可以直接嵌在网页的任何地方，不过通常我们都把JavaScript代码放到 head 中：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>helloworld</title>
<!--    外部引入-->
<!--    不要用自闭和标签-->
    <script src="js/hello.js"></script>
</head>
<body>

</body>
</html>
```

## 基本语法

- 可以在浏览器上 ，直接调试 js 代码
- 

## 数据类型

- js 不区分小数和整数

### 整数类型



### Number 

JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型：

```javascript
123; // 整数123 

0.456; // 浮点数0.456 

1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5 

-99; // 负数 

NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示 

Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就 

表示为Infinity 
```



### 字符串

字符串是以单引号'或双引号"括起来的任意文本，比如 'abc' ， "xyz" 等等

### 布尔值

布尔值和布尔代数的表示完全一致，一个布尔值只有 true 、 false 两种值，要么是 true ，要么是 false ，可以直接用 true 、 false 表示布尔值，也可以通过布尔运算计算出来：



### 比较运算符

当我们对Number做比较时，可以通过比较运算符得到一个布尔值：

要特别注意相等运算符 == 。JavaScript在设计时，有两种比较运算符：

第一种是 == 比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；

第二种是 === 比较，它不会自动转换数据类型，如果数据类型不一致，返回 false ，如果一致，再比较。

**由于JavaScript 这个设计缺陷，不要使用 == 比较，始终坚持使用=== 比较。**

另一个例外是 NaN 这个特殊的Number与所有其他值都不相等

唯一能判断 NaN 的方法是通过 isNaN() 函数：



### null ， undefined

null 表示一个“空”的值，它和 0 以及空字符串 '' 不同， 0 是一个数值， '' 表示长度为0的字符串，而 null 示“空”。



JavaScript的设计者希望用 null 表示一个空的值，而 undefined 表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用 null 。 undefined 仅仅在判断函数参数是否传递的情况下有用。

### 数组

数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。

上述数组包含6个元素。数组用 [] 表示，元素之间用 , 分隔。

另一种创建数组的方法是通过 Array() 函数实现：



然而，出于代码的可读性考虑，强烈建议直接使用 [] 。

数组的元素可以通过索引来访问。请注意，索引的起始值为 0 ：



### 对象

JavaScript的对象是一组由键-值组成的无序集合



JavaScript对象的键都是字符串类型，值可以是任意数据类型。上述 person 对象一共定义了6个键值对，其中每个键又称为对象的属性，例如， person 的 name 属性为 'Bob' ， zipcode 属性为 null 。

要获取一个对象的属性，我们用 ==对象变量.属性名== 的方式



### 变量

==var==

变量在JavaScript中就是用一个变量名表示，变量名是大小写英文、数字、 $ 和 _ 的组合，且不能用数字开头。变量名也不能是JavaScript的关键字，如 if 、 while 等。申明一个变量用 var 语句，



在JavaScript中，使用等号 = 对变量进行赋值。可以把任意数据类型赋值给变量，同一个变量可以反复赋值，而且可以是不同类型的变量，但是要注意只能用 var 申明一次.

请不要把赋值语句的等号等同于数学的等号。

打印变量X，要显示变量的内容，可以用 console.log(x) ，打开Chrome的控制台就可以看到结果。使用 console.log() 代替 alert() 的好处是可以避免弹出烦人的对话框。



使用 var 申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内（函数的概念将稍后讲解），同名变量在不同的函数体内互不冲突。

## strict 模式

'use strict';



不用 var 申明的变量会被视为全局变量，为了避免这一缺陷，所有的JavaScript代码都应该使用strict模式。我们在后面编写的JavaScript代码将全部采用strict模式。



## 字符串

 

JavaScript的字符串就是用 '' 或 "" 括起来的字符表示。如果 ' 本身也是一个字符，那就可以用 "" 括起来，比如 "I'm OK" 包含的字符是I ， ' ， m ，空格， O ， K 这6个字符。如果字符串内部既包含 ' 又包含 " 怎么办？可以用转义字符 \ 来标识



转义字符 \ 可以转义很多字符，比如 \n 表示换行， \t 表示制表符，字符 \ 本身也要转义，所以 \\ \表示的字符就是 \ 。

还可以用 \u#### 表示一个Unicode字符

'\u4e2d\u6587'; // 完全等同于 '中文' 



### 多行字符串

由于多行字符串用 \n 写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号 ``示：



### 模板字符串

要把多个字符串连接起来，可以用 + 号连接



如果有很多变量需要连接，用 + 号就比较麻烦。ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量

```js
var name = '小明'; 
var age = 20; 
var message = `你好, ${name}, 你今年${age}岁了!`; 
alert(message);
```



### 操作字符串

字符串常见的操作如下



==字符串不可变==



JavaScript为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串

- substring() 返回指定索引区间的子串
- indexOf() 会搜索指定字符串出现的位置
- toLowerCase() 把一个字符串全部变为小写
- toUpperCase() 把一个字符串全部变为大写



## 数组

JavaScript的 Array 可以包含任意数据类型，并通过索引来访问每个元素。

要取得 Array 的长度，直接访问 length 属性



Array 可以通过索引把对应的元素修改为新的值，因此，对 Array 的索引进行赋值会直接修改这个 Array 

请注意，如果通过索引赋值时，索引超过了范围，同样会引起 Array 大小的变化：



在编写代码时，不建议直接修改 Array 的大小，访问索引时要确保索引不会越界。



**常用方法**

- indexOf

  与String类似， Array 也可以通过 indexOf() 来搜索一个指定的元素的位置

- **slice**

  - slice() 就是对应String的 substring() 版本，它截取 Array 的部分元素，然后返回一个新的 Array
  - 注意到 slice() 的起止参数包括开始索引，不包括结束索引。
  - 如果不给 slice() 传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个Array ：

- push,pop

  - push() 向 Array 的末尾添加若干元素， pop() 则把 Array 的最后一个元素删除掉

- unshit, shift

  - 如果要往 Array 的头部添加若干元素，使用 unshift() 方法， shift() 方法则把Array 的第一个元素删掉：

- sort

  - sort() 可以对当前 Array 进行排序，它会直接修改当前 Array 的元素位置，直接调用时，按照默认顺序排序
  - 能否按照我们自己指定的顺序排序呢？完全可以，我们将在后面的函数中讲到。

- reverse

  - reverse() 把整个 Array 的元素给掉个个，也就是反转

- splice()

  - splice() 方法是修改 Array 的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

- contact

  - concat() 方法把当前的 Array 和另一个 Array 连接起来，并返回一个新的 Array 
  - 请注意， concat() 方法并没有修改当前 Array ，而是返回了一个新的 Array 。
  - 实际上， concat() 方法可以接收任意个元素和 Array ，并且自动把 Array 拆开，然后全部添加到新的 Array 里：

- join

  - join() 方法是一个非常实用的方法，它把当前 Array 的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

- 多维数组

  - 如果数组的某个元素又是一个 Array ，则可以形成多维数组



## 对象

JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成。

JavaScript的对象用于描述现实世界中的某个对象。



1**定义一个对象**

var 对象名 = { 

key: 'value', 

key: 'value', 

key: 'value' 

} 

2**获取对象的属性**

对象.属性



3由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性

```js
var xiaoming = { 

name: '小明' 

};

xiaoming.age; // undefined 

xiaoming.age = 18; // 新增一个age属性 

xiaoming.age; // 18 

delete xiaoming.age; // 删除age属性 

xiaoming.age; // undefined 

delete xiaoming['name']; // 删除name属性 

xiaoming.name; // undefined 

delete xiaoming.school; // 删除一个不存在的school属性也不会报错 
```



如果我们要检测对象是否拥有某一属性，可以用 in 操作符



不过要小心，如果用 in 判断一个属性存在，这个属性不一定是 这个对象的，它可能是这个对象继承得到的

要判断一个属性是否是 xiaoming 自身拥有的，而不是继承得到的，可以用 hasOwnProperty() 方法



## 流程控制

- if

- for
- 无限循环
- for...in
  - 由于 Array 也是对象，而它的每个元素的索引被视为对象的属性，所以遍历出来是下标
  - 请注意， for ... in 对 Array 的循环得到的是 String 而不是 Number 。
- while
- do...while

## Map , Set

JavaScript的默认对象表示方式 { } 可以视为其他语言中的 Map 或 Dictionary 的数据结构，即一组键值对。但是JavaScript的对象有个小问题，就是键必须是字符串。但实际上Number或者其他数据类型作为键也是非常合理的。



为了解决这个问题，最新的ES6规范引入了新的数据类型 Map 。

### map

Map 是一组键值对的结构，具有极快的查找速度。

举个例子，假设要根据同学的名字查找对应的成绩，如果用 Array 实现，需要两个 Array 



如果用Map实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查

找速度都不会变慢。用JavaScript写一个Map如下：

```js
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]); 

m.get('Michael'); // 95 
```



初始化 Map 需要一个二维数组，或者直接初始化一个空 Map 。 Map 具有以下方法：

```js
var m = new Map(); // 空Map 

m.set('Adam', 67); // 添加新的key-value 

m.set('Bob', 59); 

m.has('Adam'); // 是否存在key 'Adam': true 

m.get('Adam'); // 67 

m.delete('Adam'); // 删除key 'Adam' 

m.get('Adam'); // undefined 
```



由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉



### Set

Set 和 Map 类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在 Set 中，没有重复的key。

重复元素在 Set 中自动被过滤：

set 的方法

- add
- delete



## iterable

遍历 Array 可以采用下标循环，遍历Map 和 Set 就无法使用下标。

为了统一集合类型，ES6标准引入了新的 iterable 类型，Array，Map，Set 属于；

具有 iterable 类型的集合可以通过新的 for ... of 循环来遍历。



### 遍历集合

```js
var a = ['A', 'B', 'C']; 

var s = new Set(['A', 'B', 'C']); 

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); 

for (var x of a) { // 遍历Array 

console.log(x); 

}

for (var x of s) { // 遍历Set 

console.log(x); 

}

for (var x of m) { // 遍历Map 

console.log(x[0] + '=' + x[1]); 

} 

```

更好的方式是直接使用 iterable 内置的 forEach 方法，它接收一个函数，每次迭代就自动回调该函数。以 Array 为例

```js
a.forEach(function (element, index, array) { 

// element: 指向当前元素的值 

// index: 指向当前索引 

// array: 指向Array对象本身 

console.log(element + ', index = ' + index); 

});
```

forEach() 方法是ES5.1标准引入的，你需要测试浏览器是否支持。

Set 没有索引，因此回调函数的前两个参数都是元素本身：

```js
var s = new Set(['A', 'B', 'C']); 

s.forEach(function (element, sameElement, set) { 

console.log(element); 

});
```

 Map 的回调函数参数依次为 value 、 key 和 map 本身：

```js
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]); 

m.forEach(function (value, key, map) { 

console.log(value); 

}); 
```



# 2、函数

## 函数的定义和调用

### 函数的定义

有两种 定义函数 的方式

- function 
- var  变量=function



### 函数的调用

调用函数时，按顺序传入参数即可：



### arguments

JavaScript还有一个免费赠送的关键字 arguments ，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。

实际上 

arguments 最常用于判断传入参数的个数。



### rest

rest参数只能写在最后，前面用 ... 标识，从运行结果可知，传入的参数先绑定 a 、 b ，多余的参数以数组形式交给变量 rest ，所以，不再需要 arguments 我们就获取了全部参数。

```js
function foo(a, b, ...rest) { 

console.log('a = ' + a); 

console.log('b = ' + b); 

console.log(rest); 

}

foo(1, 2, 3, 4, 5); 

// 结果: 

// a = 1 

// b = 2 

// Array [ 3, 4, 5 ] 

foo(1); 

// 结果: 

// a = 1 

// b = undefined 

// Array [] 
```





## 变量作用域

在JavaScript中，用 var 申明的变量实际上是有作用域的。

如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量：



如果两个不同的函数各自申明了同一个变量，那么该变量只在各自的函数体内起作用。换句话说，不同

函数内部的同名变量互相独立，互不影响



由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行



### 全局作用域

不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象 window ，全

局作用域的变量实际上被绑定到 window 的一个属性



全局变量会绑定到 window 上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名

字的顶层函数，都会造成命名冲突，并且很难被发现。减少冲突的一个方法是把自己的所有变量和函数

全部绑定到一个全局变量中。例如：

```js
// 唯一的全局变量MYAPP: 

var MYAPP = {}; 

// 其他变量: 

MYAPP.name = 'myapp'; 

MYAPP.version = 1.0; 

// 其他函数: 

MYAPP.foo = function () { 

return 'foo'; 

}; 
```



把自己的代码全部放入唯一的名字空间 MYAPP 中，会大大减少全局变量冲突的可能。

许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。



### 局部作用域

由于JavaScript的变量作用域实际上是函数内部，我们在 for 循环等语句块中是无法定义具有局部作

用域的变量的：

为了解决块级作用域，ES6引入了新的关键字 let ，用 let 替代 var 可以申明一个块级作用域

的变量：

### 常量

由于 var 和 let 申明的是变量，如果要申明一个常量，在ES6之前是不行的，我们通常用全部大写

的变量来表示“这是一个常量，不要修改它的值”：



ES6标准引入了新的关键字 const 来定义常量， const 与 let 都具有块级作用域：



## 方法

在一个对象中绑定函数，称为这个对象的方法。



绑定到对象上的函数称为方法，和普通函数也没啥区别，但是它在内部使用了一个 this 关键字，这个东东是什么？

在一个方法内部， this 是一个特殊变量，它始终指向当前对象，也就是 xiaoming 这个变量。所以， this.birth 可以拿到 xiaoming 的 birth 属性。



# 3、标准对象

## 标准对象定义

在JavaScript的世界里，一切都是对象。但是某些对象还是和其他对象不太一样。为了区分对象的类型，我们用 typeof 操作符获取对象的类型，它总是返回一个字符串：

```js
typeof 123; // 'number' 

typeof NaN; // 'number' 

typeof 'str'; // 'string' 

typeof true; // 'boolean' 

typeof undefined; // 'undefined' 

typeof Math.abs; // 'function' 

typeof null; // 'object' 

typeof []; // 'object' 

typeof {}; // 'object' 
```



## Date

在JavaScript中， Date 对象用来表示日期和时间。

要获取系统当前时间，用

```js
var now = new Date(); 

now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST) 

now.getFullYear(); // 2015, 年份 

now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月 

now.getDate(); // 24, 表示24号 

now.getDay(); // 3, 表示星期三 

now.getHours(); // 19, 24小时制 

now.getMinutes(); // 49, 分钟 

now.getSeconds(); // 22, 秒 

now.getMilliseconds(); // 875, 毫秒数 

now.getTime(); // 1435146562875, 以number形式表示的时间戳 
```



## JSON

​	



# 4、BOM 对象

- 就是与==浏览器本身==相关的对象，
- 比如：浏览器自身的信息（类别，版本。。。），地址栏信息（url 信息等）
- 在判定 浏览器版本 比如 IE 或者 chrome 的时候，可以用到。
- 记忆：5+1，（因为 document 单列为 DOM 对象）

## Window 对象



## Navigator 对象



## Screen 对象



## Location 对象



## History 对象



## 存储对象（LocalStorage, SessionStorage）

- 存储在浏览器本地的K-V 键值对，可以设定值，也可以移除
- 谷歌浏览器的话，查看就在按下 F12 之后，Application 之下就可以看到了

### LocalStorage

- 存储在浏览器本地
- 除非手动删除，即使关闭了浏览器也还存在
- 

### SessionStorage

- 只存在于一次会话中，就是，打开浏览器，到关闭浏览器。关闭浏览器之后，自动删除。
- 

## Document 对象

- 在下一部分详解



# 5、DOM 对象

- DOM 对象其实也是属于 BOM 对象的，只不过很重要，独立出来讲，用的也最多。

- 就是整个 HTML 页面的文档树，整个页面都可以当作一个标签树，从 `<body>` 标签开始。





# 6、jQuery

- jQuery 是必学的，给 操作 JS 带来了很多方便。
- 需要在 head 里引入 jQuery 的 js 文件和 CSS。

jQuery 各种事件查询网站：https://jquery.cuishifeng.cn/

基本使用格式就是

```js
$("选择器").事件(){}
```



## 1、jQuery 操作 select

常用操作：

- 类似的可以举一反三。

```js
//获得 seleclt 选中框的值
$("#selectId").val();
//操作 select 选项
//获得 value 对应值的文本
$("#selectId").find("option[value="value值"]").text();
//设置 text='text值' 的 option 设置为选中
$("#selectId").find("option[text='text值']").attr("selected",true);
//获得选中项的文本
$("#selectId").find("option:selected").text();
//给 option 添加属性
$("#selectId“).find("option[value="value值"]").prop('selected',true);
$("#selectId“).find("option[value="value值"]").attr('selected',true);
//移除 option 
$("#selectId").find("option[value='value值']").remove();
```





# 7、ESlint

- 语法检查规则，只检查 JS 文件的。



# 补充

## 1、常用的 JavaScript 的函数

| 函数名    | 解释                                                   |
| --------- | ------------------------------------------------------ |
| `isNaN()` | 用于检查其参数是否是非数字值。  即 不是数字的返回 true |
|           |                                                        |
|           |                                                        |





# OVER
