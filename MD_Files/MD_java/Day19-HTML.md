# 目录

[toc]

# HTML

# 初识html

学习资源：

- MDN 的 HTML 教程：https://developer.mozilla.org/zh-CN/docs/Web/HTML

介绍：

- HTML: Hyper Text Markup Language
- 超文本：文件，图片，音频，视频，动画 。。。等
- 现在都是：HTML5 +CSS3

html5的优势：

- 

常用IDE ：

- VS Code
- webStorm
- idea
- ...

# 1、网页结构

```html
<!--DOCTYPE 告诉浏览器，我们使用什么规范 默认是html-->
<!DOCTYPE html>
<html lang="en">
<!--head 标签代表网页头部 -->
<head>
<!--    meta : 描述性标签，描述网站的一些信息-->
<!--    meta 一般用来做SEO-->
    <meta charset="UTF-8">
<!--    title 网页标题-->
    <title>Title</title>
</head>
<!--body 网页主体-->
<body>
helloworld

</body>
</html>
```

# 2、基本标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>基本标签学习</title>
</head>
<body>
<!--标题标签 h1 - h6-->

<h1>一级标签</h1>
<!--段落标签-->
<p>html</p>
<p>Nihao</p>
<!--换行标签    -->
你好<br/>
我好<br/>
<!--水平线表现-->
<hr/>
<!--字体样式标签-->
<h1>字体样式标签</h1>
粗体：<strong>我爱你</strong><br/>
斜体:<em>我爱你</em><br/>

<!--特殊符号-->
<!--
特殊符号的格式，都是 &开头，;结尾
-->
<h1>特殊符号</h1>
空格：&nbsp;&nbsp;空格
<br/>
大于号：&gt;
<br/>
小于号：&lt;
<br/>
版权符号：&copy;版权所有
<br/>
</body>
</html>
```

# 3、图像标签

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-11_18-30-56.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图像标签</title>
</head>
<body>
<!--
img 标签
src:图片地址（必填）
相对地址（../ 表示上一级目录），绝对地址

alt : 图片名字（必填）

-->
<img src="../resources/image/1.png#down" alt="图片名字" title="悬停文字" width="300">
<a href="4 链接标签.html#down">跳转</a>
</body>
</html>
```

# 4、链接标签

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-11_18-44-45.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>链接标签</title>
</head>
<body>

<a name="top">顶部</a><br/>

<!--
a 标签
href 必填，表示跳转链接
target: 表示在窗口哪里打开  _blank : 在新标签页打开，_self 在原本窗口打开
-->
<a href="2 BasicTag.html" target="_blank">点击我跳转到2</a><br/>
<a href="https://www.baidu.com" target="_self">跳转百度</a><br/>
<!--图像变成超链接-->
<a href="2 BasicTag.html">
    <img src="../resources/image/1.png" alt="图片名字" title="悬停文字" width="1000">
</a>


<!--锚链接
1. 需要一个锚标记 <a name="top">顶部</a>
2. 跳转到锚标记 <a href="#top">回到顶部</a>
#
页内跳转 ， 还可以实现页面间的跳转
-->
<a href="#top">回到顶部</a>
<a name="down">来自3</a>

<!--    功能性链接
邮件链接：mailto
-->
<a href="mailto:kkkk@mail.com">点击联系我</a>
</body>
</html>
```

# 5、列表标签



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表</title>
</head>
<body>

<!--有序列表
应用范围：试卷 ，问答。。。
-->
<ol>
    <li>python</li>
    <li>java</li>
</ol>
<br/>

<!--无序列表
应用范围：导航 ，侧边栏。。。
-->
<ul>
   
    <li>python</li>
   <li>java</li>
</ul>
<!--自定义列表
dl :标签
dt 列表名称
dd 列表内容
应用范围：公司网站底部


-->
<dl>
   <dt>自定义列表</dt>

   <dd>java</dd>
   <dd>python</dd>
   <dd>linux</dd>

</dl>
</body>
</html>
```

# 6、表格

```html
<!DOCTYPE html>
<html>
<head>
   <title>表格</title>
</head>
<body>
<!-- table 标签
tr 行
td 列
 -->

 <table border="1px">
   <tr>
      <!-- colspan 跨几列 -->
      <td colspan="2">1-1</td>
      
   </tr>
   <tr>
      <!--  rowspan 跨行-->
      <td rowspan="2">2-1</td>
      <td>2-2</td>
   </tr>
    <tr>
      <td>3-1</td>
   </tr>
 </table>
</body>
</html>
```

# 7、媒体元素

```html
<!DOCTYPE html>
<html>
<head>
   <title>媒体元素</title>
</head>
<body>

<!-- 音频和视频 
src 资源路径
controls 控制条
autoplay 自动播放


-->
<video src="../resources/video/2.mp4" controls autoplay></video>

<audio src="../resources/audio/3.mp3" controls autoplay></audio>
 </table>
</body>
</html>
```

# 8、页面结构 

- 前三比较重要

1. header
2. footer
3. nav
4. article
5. section
6. aside

```html
<!DOCTYPE html>
<html>
<head>
   <title>页面结构</title>
</head>
<body>

<header><h2>网页头部</h2></header>
<section><h2>网页主体</h2></section>
<footer><h2>网页脚部</h2></footer>
</body>
</html>
```

# 9、iframe 内联框架

- 这个用的比较多，常用就是，在本页面，打开另一个页面，使用可以类似为弹窗

```html
<!DOCTYPE html>
<html>
<head>
   <title>iframe</title>
</head>
<body>
<!-- iframe
src: 引用页面地址
name: 框架标识名
width
height
 -->
 <!-- 方式一： -->
<iframe src="https://www.baidu.com" frameborder="0" width="1000px" height="200px"></iframe>

<!-- 方式二  -->
<iframe src="" name="hello" frameborder="0" width="1000px" height="200px" ></iframe>
<a href="https://www.bilibili.com/" target="hello">helllo </a>


</body>
</html>
```

​	

# 10、表单

![](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/Snipaste_2021-03-11_19-52-33.png)





表单的应用；

- 隐藏域 hidden
- 只读 readonly
- 禁用 disabled

表单的初级验证

- placeholder ：可以输入的框的提示信息
- required 非空判断，不能为空
- pattern 正则表达式 。直接去百度常用正则表达式，

```html
<!DOCTYPE html>
<html>
<head>
	<title>表单</title>
</head>
<body>
<!-- form 表单
action : 表单提交的位置，可以是网站，，也可以是一个请求处理地址
method : get ,post 提交方式
 -->
<form action="1 MyFirstWeb.html" method="get">
	<!-- 文本框 input type="text"
	value : 默认初始值
	maxlength 最长能写几个字符
	size 文本框的长度

	-->
	<p>name:<input type="text" name="username" placeholder="请输入用户名" required>	</p>
	<!-- 密码框  input type="password"-->
	<p>password:<input type="password" name="pwd"></p>


	<!-- radio 单选框
		value 值
		name 名字，要在同一组，才能单选
		checked 默认选中
	 -->
	<p>性别:
			<input type="radio" name="sex" value="boy" checked>男
			<input type="radio" name="sex" value="girl">女
	</p>

	<!-- checkbox 多选框 
checked 默认选中
	-->
	<p>爱好：
		<input type="checkbox" value="sleep"  name="hobbies">睡觉
		<input type="checkbox"  value="game"  name="hobbies">游戏
		<input type="checkbox"  value="music"  name="hobbies" checked>音乐
		<input type="checkbox"  value="dance"  name="hobbies">跳舞

	</p>

	<!-- 按钮 
input type="button" 普通按钮
input type="image" 图片按钮
input type="submit" 提交按钮
input type="reset" 重置按钮
	-->
	<p>按钮：
		<input type="button" name="btn1" value="点击变长">
		<input type="image" name="../resources/image/1.png">
		
	</p>

	<!-- 下拉框，列表框 
		selected 默认选中
	-->
	<p>下拉框:
		<select name="列表名称">
			<option value="China" selected>中国</option>
			<option value="American">美国</option>
			<option value="India">印度</option>
		</select>
	</p>


<!-- textarea 文本域 -->
<p>反馈：
	<textarea name="textarea" cols="50" rows="10">文本内容</textarea>
	
</p>

<!-- input type="file" 文件域

 -->
 <p>
 	<input type="file" name="files">
 </p>
<!-- 邮箱验证 -->
<p>邮箱
	<input type="email" name="emails">
</p>
<!-- URL验证 -->
<p>URL
	<input type="url" name="emails">
</p>
<!-- 数字验证 -->
<p>数字
	<input type="number" name="nums" max="100" min="0" step="1">
</p>
<!-- 滑块 -->
<p>音量滑块
	<input type="range" name="range" max="10" min="0" step="2">
</p>
<!-- 搜索框
删除比较方便 
 -->
<p>搜索框：	
	<input type="search" name="search">
	
</p>

<!-- 增强鼠标可用性 ，点文字文本框，就会弹到文本框
label  -->
<p>
	<label for="mark">你点我试试</label>
	<input type="text" name="labletest" id="mark">
</p>
	<p>
		<input type="submit">
		<input type="reset">
	</p>

</form>
</body>
</html>
```



# 11、常用事件

## onlick



## onkeyup

input 去空格相关：

```js
# 去掉所有空格
onkeyup="this.value=this.value.replace(/[, ]/g,'')"
# 去掉首尾空格，不去掉中间的
onkeyup="this.value=this.value.replace(/^\s+|\s+$/g,'')"
```









# OVER

