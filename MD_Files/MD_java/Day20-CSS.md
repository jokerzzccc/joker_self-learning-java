# Day20-CSS

如何学习 ：

- 什么是 CSS
-  CSS  怎么用
- ==CSS 选择器（重点）==
-  美化网页（文字，阴影，超链接，列表，渐变。。。）
- 盒子模型
- 浮动
- 定位
- 网页动画（特效）（专业前端可以多研究）

参考网址：

- 菜鸟：https://www.runoob.com/css/css-tutorial.html

# 1、什么是 CSS 

## 1.1、CSS 定义

Cas cading Style Sheet 层叠样式表

CSS : 表现（美化网页）

字体，颜色，边距，高度，宽度，背景图片，网页定位，网页浮动。。。



浏览器，可以直接点，（谷歌）检查，就能看见，右边的就是 CSS 相关的。



## 1.2、CSS 发展史

CSS 1.0

CSS 2.0 : div(块)+CSS ; HTML 与 CSS 结构分离的思想，网页变得简单

CSS 2.1 : 浮动，定位

CSS 3.0 : 圆角，阴影，动画。。。（此时就要考虑浏览器的兼容性了）



## 1.3、快速入门

### css 语法

**CSS 组成**

CSS 规则由两个主要的部分构成：

- **选择器**，
- 以及一条或多条**声明**:

![img](D:\2021java学习文件\mdFile\Day20-CSS.assets\632877C9-2462-41D6-BD0E-F7317E4C42AC.jpg)

注意：

- 选择器通常是您需要改变样式的 HTML 元素。

- 每条声明由一个属性和一个值组成。

- 属性（property）是您希望设置的样式属性（style attribute）。每个属性有一个值。属性和值被冒号分开。



CSS声明总是**以分号(;)结束**，声明总以**大括号({})括起来**:

**CSS 注释**

CSS注释以 **/\*** 开始, 以 ***/** 结束



**CSS 的优势**：

- 内容和表现分离
- 网页架构表现统一，可以实现复用
- 样式十分的丰富
  - 建议使用独立于 HTML 的 CSS 文件
- 利用 SEO ，容易被搜索引擎收录
  - 补充：如果 用 Vue ,就不容易被收录了。
- 

### CSS 三种导入方式

插入样式表的方法有三种:

- 外部样式表(External style sheet)
- 内部样式表(Internal style sheet)
- 内联样式(Inline style)

#### 内联样式 

- 尽量不用

- 就是**行内样式**，
- 在相关的**标签内**使用样式（style）属性。
- Style 属性可以包含任何 CSS 属性。本例展示如何改变段落的颜色和左外边距：

比如:

```html
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```

#### 内部样式表

当单个文档需要特殊的样式时，就应该使用内部样式表。

可以使用 `<style>` 标签在文档头部定义内部样式表。

比如:

```html
<head>
<style>
hr {color:sienna;}
p {margin-left:20px;}
body {background-image:url("images/back40.gif");}
</style>
</head>
```



#### 外部样式表

- 当样式需要应用于很多页面时，外部样式表将是理想的选择。
- 在使用外部样式表的情况下，你可以通过改变一个文件来改变整个站点的外观。
- 每个页面使用 **<link>** 标签链接到样式表。 <link> 标签在（文档的）头部。



比如：

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```



- 浏览器会从文件 mystyle.css 中读到样式声明，并根据它来格式文档。
- 外部样式表可以在任何文本编辑器中进行编辑。文件不能包含任何的 html 标签。样式表应该以 .css 扩展名进行保存。



**拓展： **

外部样式有两种写法

- 链接式（CSS 3.0），现在用的,link 是属于 HTML 标签。

  ```html
  <link rel="stylesheet" href="css/style.css">
  ```

  

- 导入式（CSS 2.1 特有）

  ```html
      <style>
          @import url("css/style.css");
      </style>
  ```

  

#### 多重样式

如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被**继承**过来。

例如，外部样式表拥有针对 h3 选择器的三个属性：

h3 {    color:red;    text-align:left;    font-size:8pt; }

而内部样式表拥有针对 h3 选择器的两个属性：

h3 {    text-align:right;    font-size:20pt; }

假如拥有内部样式表的这个页面同时与外部样式表链接，那么 h3 得到的样式是：

color:red; text-align:right; font-size:20pt;

即颜色属性将被继承于外部样式表，而文字排列（text-alignment）和字体尺寸（font-size）会被内部样式表中的规则取代。



**多重样式优先级**

- 优先级，其实就是 **就近原则**，谁里那段代码更近，谁就生效。

- 正常情况下是： **（内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式**

**注意：***如果 外部样式 放在 内部样式 的后面，则 外部样式 将 覆盖 内部样式。*





# 2、CSS 选择器

> 选择器作用：选择页面的 某一个 或者 某一类 元素。

选择器 参考网站：https://www.runoob.com/cssref/css-selectors.html

## 2.1、基本选择器

1. 标签选择器
2. id 选择器
3. class 选择器

**优先级：**

- 和样式表的导入方式不同，不遵循就近原则，是固定的
- id 选择器 > class 选择器 > 标签选择器

#### element 选择器

- 即**标签选择器**，会应用到，页面上**所有**的这个**标签的元素**
- 语法: ==标签名{}==
- 

比如：

```css
h1{
    color: red;
}
p{
    color: black;
}
```



#### id 选择器

- id 选择器可以为标有特定 id 的 HTML 元素指定特定的样式。
- 语法：==#id名{}==
- id 必须保证 **全局唯一**

例子：



#### class  选择器

- class 选择器用于描述**一组元素**的样式，class 选择器有别于id选择器，class可以在多个元素中使用。

- 语法：==.class名称{}==

- class 选择器的好处：可以多个标签归类，放到同一个 class 
- 就是把 多个标签 都添加同一个 class 属性，就可以给他们同时，设置 CSS 。

比如：

在以下的例子中，所有拥有 center 类的 HTML 元素均为居中。

```css
.center {
    text-align:center;
}
```

你也可以指定特定的HTML元素使用class。

在以下实例中, 所有的 p 元素使用 class="center" 让该元素的文本居中:

```html
p.center {
text-align:center;
}
```



## 2.2、层次选择器

- 要把 HTML 网页的所有标签看成是一个大树，就和数据结构的多叉树差不多，父类，子类，兄弟等概念。
- 层次选择器不改变自身样式，而是，改变 兄弟，子代 元素的样式。

1. 后代选择器
2. 子选择器
3. 相邻兄弟选择器
4. 通用选择器



###  后代选择器

- 在某个元素的后面 

例子：

body 后面的**所有** P 元素

```css
body p{
    background: yellow;
}
```



### 子选择器

- 同一代的选择器 ，才会生效

例子：

HTML 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>demo01</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body>
<p>
    选择器
</p>
<p>
    选择器
</p>
<ul>
    <li>
        <p> 选择器 </p>
    </li>
</ul>
</body>
</html>
```

CSS 代码

```css

body>p{
    background: purple;
}
```



### 相邻兄弟选择器

- 同一辈的选择器
- 只有一个，**向下相邻的一个**



比如：

HTML 代码

```html
<p>
    选择器
</p>
<p class="selector">
    选择器
</p>
<p>
    选择器
</p>
```

CSS 代码

```css
.selector+p{
    background: aqua;
}
```

### 通用选择器

- 当前选中元素的 **向下的所有兄弟元素**

比如：

HTML 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>demo01</title>
<link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1 style="color: blue">
demo01
</h1>
<p>
    选择器1
</p>
<p class="selector">
    选择器2
</p>
<p>
    选择器3
</p>
<p>
    选择器4
</p>
<ul>
    <li>
        <p> 选择器5 </p>
    </li>
</ul>
</body>
</html>
```

CSS 代码

```css
.selector~p{
    background: antiquewhite;
}
```



## 2.3、结构伪类选择器

- 跟结构相关的伪类选择器，就是为了定位结构的。

**伪类**的概念：

- 其实就是所有带冒号==:==的就是伪类。

- 作用：为了避免使用 id  , class 选择器，而定位到某一个具体的子标签。

HTML 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>伪类选择器</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1>h1</h1>
<p>p1</p>
<p>p2</p>
<p>p3</p>
<ul>
    <li>li 111</li>
    <li>li 222</li>
    <li>li 333</li>

</ul>
</body>
</html>
```

CSS 代码01

```css
/*选中ul 的第一个 li 元素*/
ul li:first-child{
    background: mediumpurple;
}
/*选中ul 的最后一个 li 元素*/
ul li:last-child{
    background: rebeccapurple;
}
```

CSS 代码02 ，（ 了解就好）

```css
/*选中p1
定位到父元素，选择当前的第 n 个元素
选中当前p 元素的父级元素，选中父级元素的的第 n 个元素。并且是当前元素才生效，比如 p 元素。
*/
p:nth-child(1){
    background: aqua;
}
p:nth-child(2){
    background: purple;
}
/*
选中当前 p 元素的父级元素，下p 元素的第 3 个类型，
*/
p:nth-of-type(3){
    background: yellow;
}
```

## 2.4、属性选择器（==重要==）

- 十分常用
- 比 id 选择器更好用，可以精确到某一个元素的具体的属性的具体的值
- 语法：==element[attribute=value]{}==
- ==element[属性名=属性的值]{声明;}==

- 高级之处，就在于可以添加正则表达式。

**总结：**

```css
/*
属性名=属性值（正则）
= 绝对等于属性值；
*= 包含这个属性值都可以； 等同于 ~=
^= 以这个值开头的；等同于 |=
$= 以这个值结尾的
*/
```



- 比如：

  ```css
  1、[attribute] 选择所有带有target属性的 <a>元素：
  
  a[target]
  {
  background-color:yellow;
  }
  ```

  ```css
  2、[attribute=value]选择所有使用target="_blank"的a元素,这里是绝对等于，而不是包含_blank
  a[target=_blank]
  {
  background-color:yellow;
  }
  ```

  ```html
  3、[attribute~=value] 选择器用于选择属性值包含一个指定单词的元素。
  ~= 与 *= 一样，都表示包含这个属性值就行。
  
  选择标题属性包含单词"flower"的所有元素
  [title~=flower]
  {
  background-color:yellow;
  }
  ```

  ```html
  4、[attribute|=value] 选择器用于选择以指定值开头的属性值的元素。等同与 JQuery 的 ^= 
  
  选择 lang 属性值以 "en" 开头的元素，并设置其样式：
  [lang|=en]
  { 
      background-color:yellow;
  }
  ```



# 3、美化网页元素

## 3.1、为什么要美化网页

1. 有效的传递页面信息
2. 美化网页，页面漂亮，吸引用户
3. 凸显页面的主题
4. 提高用户的体验



### span 标签

- 重点要突出的字 ，使用 span 标签 套起来，
- span 标签，是属于 HTML 标签的。

- <span> 用于对文档中的行内元素进行组合。

- <span> 标签没有固定的格式表现。当对它应用样式时，它才会产生视觉上的变化。
- 如果不对 <span> 应用样式，那么 <span> 元素中的文本与其他文本不会任何视觉上的差异。

- <span> 标签提供了一种将文本的一部分或者文档的一部分独立出来的方式。



比如：

可以给span 加上 id 或者 class 等属性。就更好设置样式。

```html
<h1>I AM <span style="color: yellow">JOKER</span> </h1>
```

## 3.2、字体样式

- 就是设置文本字体样式

常见的：

| 属性        | 解释             |
| ----------- | ---------------- |
| font-family | 字体类别         |
| font-size   | 字体大小         |
| font-weight | 字体粗细         |
| color       | 字体或者文本颜色 |

 HTML 文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1>故事短文</h1>
<p class="p1">
    鲁迅鲁迅，以1881年生于浙江之绍兴城内姓周的一个大家族里。
    父亲是秀才；母亲姓鲁，乡下人，她以自修到能看文学作品的程度。
<p/>
<p>
    家里原有祖遗的四五十亩田，但在父亲死掉之前，已经卖完了。
</p>
<P>
    I AM JOKER!
</P>
<p id="pSum">
    这时我大约十三四岁，但还勉强读了三四年多的中国书。
</p>

</body>
</html>
```

CSS 文件

```css
/*
font-family: 字体
font-size: 字体大小
font-weight: 字体粗细
color: 字体颜色
*/
body{
    font-family: 华文楷体;
}
h1{
    font-size: 50px;
}
.p1{
    font-weight: bold;
    color: purple;
}
```



同时，这些，font 的设置，也可以**写在一起**。



## 3.3、文本样式

常见的：

| 属性            | 描述                                 |
| --------------- | ------------------------------------ |
| color           | 颜色                                 |
| text-align      | 文本对齐方式                         |
| text-indent     | 首行缩进                             |
| ling-height     | 行高                                 |
| text-decoration | 文本装饰（下划线、上划线、删除线等） |



1. 文本颜色 color

2. 文本对齐方式 text-align

3. 首行缩进 text-indent

4. 行高 line-height

5. 文本装饰 text-decoration

6. 文本图片水平对齐：vertical-align:middle

   1. ```css
      img,span{
          vertical-align: middle;
      }
      ```



示例文档

HTML 文档

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1>故事短文</h1>
<p class="p1">
    鲁迅鲁迅，以1881年生于浙江之绍兴城内姓周的一个大家族里。
    父亲是秀才；母亲姓鲁，乡下人，她以自修到能看文学作品的程度。
<p/>
<p>
    I AM JOKER!
</P>
    家里原有祖遗的四五十亩田，但在父亲死掉之前，已经卖完了。
</p>
<P>
<p id="pSum">
    这时我大约十三四岁，但还勉强读了三四年多的中国书。
</p>

</body>
</html>
```

### color

表现方式，主要有：

- 颜色单词 ，
  - 比如：red
- RGB 16进制表达（一个颜色两位，16进制（0-f））
  - 比如：#ff0000
- RGB 函数二进制数字表达，
  - 比如：rgb(255,0,0)
- RGBA 函数二进制表达，
  - 允许设定一个颜色的透明度。a 表示透明度：0=透明；1=不透明。
  - 比如：rgba(255,0,0,0.3)

```css
h1{
    color: black;
}
.p1{
    color: #ff0000;
}
#pSum{
    color: rgb(255,0,0);
}
#p2{
    color: rgba(255,0,0,0.3);
}
```





### text-align

| 值      | 描述                                           |
| :------ | :--------------------------------------------- |
| left    | 把文本排列到左边。**默认值**：由浏览器决定。   |
| right   | 把文本排列到**右边**。                         |
| center  | 把文本排列到**中间**。                         |
| justify | 实现**两端对齐**文本效果。                     |
| inherit | 规定应该从父元素继承 text-align 属性的值。**** |

CSS 代码

```css
h1{
    color: black;
    text-align: center;
}
```



### text-indent

- 首行缩进

```css
#pSum {
    text-indent: 32px;
    color: rgb(255, 0, 0);
}
```



### line-height

在设定了块的高度（height）时：设定行高（line-height）和块的高度（height） 一致，文本就可以在块里，上下居中。

```css
.p1 {
    height: 300px;
    line-height: 300px;
    background: yellow;
}
```



### text-decoration

常见属性值

| 值           | 描述                                            |
| :----------- | :---------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。下划线                      |
| overline     | 定义文本上的一条线。上划线                      |
| line-through | 定义穿过文本下的一条线。中划线                  |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

语法：

```css
/*关键值*/
text-decoration: none;                     /*没有文本装饰*/
text-decoration: underline red;            /*红色下划线*/
text-decoration: underline wavy red;       /*红色波浪形下划线*/

/*全局值*/
text-decoration: inherit;
text-decoration: initial;
text-decoration: unset;
```



## 3.4、文本阴影 和超链接伪类

CSS 伪类：https://www.runoob.com/css/css-pseudo-classes.html

文本阴影：text-shadow

- 语法：text-shadow: *h-shadow v-shadow blur color*;

超链接伪类：

```css
a:link {color:#FF0000;} /* 未访问的链接 */
a:visited {color:#00FF00;} /* 已访问的链接 */
a:hover {color:#FF00FF;} /* 鼠标划过链接 */
a:active {color:#0000FF;} /* 已选中的链接 */
```



HTML 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<a href="#">
    <img src="image/img_zoro.png" alt="zoro">
</a>
<br>

<a href="#">
    <span id="zoro01">zoro from joker</span>
</a>
<p id="price">
    price=999
</p>
</body>
</html>
```

CSS 代码

```css
img {
    height: 100px;
    width: 100px;
}
/*去掉链接文字的下划线，用默认的颜色*/
a{
    text-decoration: none;
}
/*鼠标悬浮状态*/
a:hover{
    color: yellow;
    font-size: 30px;
}
/*按住鼠标未释放的状态*/
a:active{
    color: mediumpurple;
}
#price{
    text-shadow: 2px 2px #ff0000;
}
```



## 3.5、列表



无序列表: ul

有序列表: ol



## 3.6、背景

CSS backgrouds : https://www.runoob.com/css/css-background.html

CSS 背景属性

| Property                                                     | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [background](https://www.runoob.com/cssref/css3-pr-background.html) | 简写属性，作用是将背景属性设置在一个声明中。 |
| [background-attachment](https://www.runoob.com/cssref/pr-background-attachment.html) | 背景图像是否固定或者随着页面的其余部分滚动。 |
| [background-color](https://www.runoob.com/cssref/pr-background-color.html) | 设置元素的背景颜色。                         |
| [background-image](https://www.runoob.com/cssref/pr-background-image.html) | 把图像设置为背景。                           |
| [background-position](https://www.runoob.com/cssref/pr-background-position.html) | 设置背景图像的起始位置。                     |
| [background-repeat](https://www.runoob.com/cssref/pr-background-repeat.html) | 设置背景图像是否及如何重复。                 |



## 3.7、渐变

CSS3 定义了两种类型的渐变（gradients）：

- **线性渐变（Linear Gradients）- 向下/向上/向左/向右/对角方向**
- **径向渐变（Radial Gradients）- 由它们的中心定义**



# 4、盒子模型

## 4.1、什么是盒子模型

CSS 盒子模型：https://www.runoob.com/css/css-boxmodel.html

不同部分的说明：

- **Margin(外边距)** - 清除边框外的区域，外边距是透明的。
- **Border(边框)** - 围绕在内边距和内容外的边框。
- **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。
- **Content(内容)** - 盒子的内容，显示文本和图像。



**重要:** 当您指定一个 CSS 元素的宽度和高度属性时，你只是设置内容区域的宽度和高度。要知道，完整大小的元素，你还必须添加内边距，边框和外边距。



## 4.2、边框

CSS 边框：https://www.runoob.com/css/css-border.html

CSS3 边框: https://www.runoob.com/css3/css3-borders.html

1. 边框的粗细
2. 边框的样式
3. 边框的颜色



## 4.3、内外边距

内边距：padding 

外边距：margin

- 上下左右：top, bottom,left, right

## 4.4、圆角边框

border-radius



## 4.5、边框阴影

border-shadow

# 5、浮动

## 5.1、标准文档流



## 5.2、display

- 属性值：none, block , inline, inline-block

- 这也是行内元素排列的一种方式，但更多用 float
- 

## 5.3、float



## 5.4、父级边框塌陷的问题

解决方案：

1. 增加父级元素的高度

2. 增加一个空的 div 标签，清除浮动

3. overflow

   1. 在父级元素增加一个 overflow:hidden

4. ==常用，推荐==。父类添加一个伪类:after

   ```css
   #father:after{
       content: '';
       display: block;
       clear: both;
   }
   ```

## 5.5、display 与 float 对比

display : 

- 方向不可控制

float:

- 浮动起来，会脱离标准文档流，所以要解决父级边框塌陷的问题。



# 6、定位 position

## 6.1 相对定位 relative

相对定位元素的定位是相对其正常位置。

## 6.2 绝对定位 absolute

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>:

## 6.3 固定定位 fixed

元素的位置相对于浏览器窗口是固定位置。

## 6.4 z-index



# 7、动画

- CSS3 新增了很多动画效果



# 8、总结