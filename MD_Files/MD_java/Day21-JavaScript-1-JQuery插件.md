# Day21-JavaScript-jQuery

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







# JQuery 第三方插件

## 1、jqGrid

- jQuery 的表格插件，基于 Ajax 实现
- 只需引入相关 JS 文件。

参考网址：

- 查询相关：https://blog.mn886.net/jqGrid/
- 官网：http://www.trirand.com/jqgridwiki/doku.php?id=wiki:jqgriddocs

### 常用 初始化参数

| 名称        | 类型    | 描述                                                         | 默认值  | 可修改 |
| ----------- | ------- | ------------------------------------------------------------ | ------- | ------ |
| postData    | array   | 此数组内容直接赋值到url上，参数类型：{name1:value1…}         | 空array | 是     |
| datatype    | string  | 从服务器端返回的数据类型，默认xml。可选类型：xml，local，json，jsonnp，script，xmlstring，jsonstring，clientside |         |        |
| mtype       | string  | ajax提交方式。POST或者GET，默认GET                           |         |        |
| colNames    | Array   | 列显示名称，是一个数组对象                                   |         |        |
| colModel    | Array   | 常用到的属性：**name** 列显示的名称；**index** 传到服务器端用来排序用的列名称；**width** 列宽度；**align** 对齐方式；**sortable** 是否可以排序 |         |        |
| pager       | string  | 定义翻页用的导航栏，必须是有效的html元素。翻页工具栏可以放置在html页面任意位置 |         |        |
| rowNum      | int     | 在grid上显示记录条数，这个参数是要被传递到后台               |         |        |
| rowList     | array   | 一个**下拉选择框**，用来改变显示记录数，当选择时会覆盖rowNum参数传递到后台 |         |        |
| sortname    | string  | 默认的排序列。可以是列名称或者是一个数字，这个参数会被提交到后台 |         |        |
| autowidth   | boolean | 如果为ture时，则当表格在首次被创建时会根据父元素比例重新调整表格宽度。如果父元素宽度改变，为了使表格宽度能够自动调整则需要实现函数：setGridWidth | false   | 否     |
| multiselect | boolean | 定义是否可以多选                                             | false   | 否     |
| rownumbers  | boolean | 如果为ture则会在表格左边新增一列，显示行顺序号，从1开始递增。此列名为'rn'. | false   | 否     |
| shrinkToFit | boolean | 此属性用来说明当初始化列宽度时候的计算类型，如果为ture，则按比例初始化列宽度。如果为false，则列宽度使用colModel指定的宽度 |         |        |
| sortorder   | string  | 排序顺序，升序或者降序（asc or desc）                        | asc     | 是     |
| viewrecords | boolean | 是否要显示总记录数                                           | false   | 否     |
| jsonReader  | array   | 描述json 数据格式的数组                                      |         | 否     |
|             |         |                                                              |         |        |
|             |         |                                                              |         |        |



### 常用 事件



| 事件          | 参数              | 备注                                                         |
| ------------- | ----------------- | ------------------------------------------------------------ |
| loadComplete  | xhr               | 当从服务器返回响应时执行，xhr：XMLHttpRequest 对象           |
| ondblClickRow | rowid,iRow,iCol,e | 双击行时触发。rowid：当前行id；iRow：当前行索引位置；iCol：当前单元格位置索引；e:event对象 |
| gridComplete  | none              | 当表格所有数据都加载完成而且其他的处理也都完成时触发此事件，排序，翻页同样也会触发此事件 |
| onSelectRow   | rowid,status      | 当选择行时触发此事件。rowid：当前行id；status：选择状态，当multiselect 为true时此参数才可用 |
|               |                   |                                                              |
|               |                   |                                                              |

### 常用方法

| 方法          | 参数                                   | 返回值           |                                                              |
| ------------- | -------------------------------------- | ---------------- | ------------------------------------------------------------ |
| setCell       | rowid,colname, data, class, properties | jqGrid对象       | 改变单元格的值。<br />rowid：当前行id；<br />colname：列名称，也可以是列的位置索引，从0开始；<br />data：改变单元格的内容，如果为空则不更 新；<br />class：如果是string则会使用addClass方法将其加入到单元格的css中，如果是array则会直接加到style属性中；<br />properties：设置单元格属性 |
| getDataIDs    | none                                   | array[]          | 返回当前grid里所有数据的id（就是可以理解为行号 rowid）       |
| getCell       | rowid, iCol                            | 单元格内容       | 返回指定rowid，iCol的单元格内容，iCol既可以是当前列在colModel中的位置索引也可以是name值。<br />注意：在编辑行或者单元格时不能使用此方法，此时返回的并不是改变的值，而是原始值 |
| getCol        | colname, returntype, mathoperation     | array[] or value | 返回列的值。<br />colname既可以是当前列在colModel中的位置索引也可以是name值。<br />returntype指定返回数据的类型，默认为false。当为false时，返回的数组中只包含列的值，当为true时返回数组是对象数组，具体格式 {id:rowid, value:cellvalue} ，id为行的id，value为列的值。如： [{id:1,value:1},{id:2,value:2}…]。mathoperation 可选值为'sum, 'avg', 'count' |
| clearGridData | clearfooter                            | jqGrid对象       | 清除表格当前加载的数据。如果clearfooter为true时则此方法删除表格最后一行的数据 |
|               |                                        |                  |                                                              |
|               |                                        |                  |                                                              |
|               |                                        |                  |                                                              |



### **jqGrid 使用示例**



## 2、xcConfirm

- 弹出对话框插件

- 只需引入相关的 css ,js 

参考博客：

- https://blog.csdn.net/Good_Luck_Kevin2018/article/details/87716087

常见的调用 xcConfirm

```js

<script type="text/javascript">
    $(function(){
        
        $("#btn1").click(function(){
            var txt=  "提示文字";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.info);
        });
        
        $("#btn2").click(function(){
            var txt=  "提示文字";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.confirm);
 
        });
        
        $("#btn3").click(function(){
            var txt=  "提示文字";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.warning);
        });
        
        $("#btn4").click(function(){
            var txt=  "提示文字";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.error);
        });
        
        $("#btn5").click(function(){
            var txt=  "提示文字";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.success);
        });
        
        $("#btn6").click(function(){
            var txt=  "请输入";
            window.wxc.xcConfirm(txt, window.wxc.xcConfirm.typeEnum.input,{
                onOk:function(v){
                    console.log(v);
                }
            });
        });
        
        $("#btn7").click(function(){
            var txt=  "自定义呀";
            var option = {
                title: "自定义",
                btn: parseInt("0011",2),
                onOk: function(){
                    console.log("确认啦");
                }
            }
            window.wxc.xcConfirm(txt, "custom", option);
        });
        
        $("#btn8").click(function(){
            var txt=  "默认";
            window.wxc.xcConfirm(txt);
        });
    });
</script>

```



## 3、AjaxFileUpload

- ajaxFileUpload是一个**异步上传文件**的jQuery插件
- 使用 ：先引入jquery和ajaxFileUpload插件（js 文件），注意先后顺序



参考网址：

- https://www.jb51.net/article/95140.htm

### **常见参数**

| 属性          | 解释                                                         |
| ------------- | ------------------------------------------------------------ |
| url           | 上传处理程序地址                                             |
| fileElementId | 需要上传的文件域的ID，即<input type="file">的ID。            |
| secureuri     | 是否启用安全提交，默认为false。                              |
| dataType      | 服务器返回的数据类型。可以为xml,script,json,html。如果不填写，jQuery会自动判断。 |
| success       | 提交成功后自动执行的处理函数，参数data就是服务器返回的数据。 |
| error         | 提交失败自动执行的处理函数。                                 |
| data          | 自定义参数。这个东西比较有用，当有数据是与上传的图片相关的时候，这个东西就要用到了。 |
| type          | 当要提交自定义参数时，这个参数要设置成post                   |

### 常见错误提示

1、SyntaxError: missing ; before statement错误
　　如果出现这个错误就需要检查url路径是否可以访问
2、SyntaxError: syntax error错误
　　如果出现这个错误就需要检查处理提交操作的服务器后台处理程序是否存在语法错误
3、SyntaxError: invalid property id错误
　　如果出现这个错误就需要检查文本域属性ID是否存在
4、SyntaxError: missing } in XML expression错误
　　如果出现这个错误就需要检查文件name是否一致或不存在
5、其它自定义错误
　　大家可使用变量$error直接打印的方法检查各参数是否正确，比起上面这些无效的错误提示还是方便很多。

### **上传文件的 JS 示例代码：**

使用语法：==$.ajaxFileUpload([options])==

```js
$.ajaxFileUpload({
	type: "POST",
	url: "/toolkit/importPicFile.do",
	data:{picParams:text},//要传到后台的参数，没有可以不写
	secureuri : false,//是否启用安全提交，默认为false
    fileElementId:'ImportPicInput',//文件选择框的id属性
    dataType: 'json',//服务器返回的格式
    async : false,
	success: function(data){
		if(data.result=='success'){
			//coding
		}else{
			//coding
		}
    },
    error: function (data, status, e){
    	/coding
    }
});

```





## 4、jQuery UI 

参考网址：

- 中文官网：https://www.jqueryui.org.cn/



- jQuery UI 是建立在 jQuery JavaScript 库上的一组用户界面交互、特效、小部件及主题。我们可以直接用它来构建具有很好交互性的web 应用程序。



- jQuery UI 包含了许多维持状态的小部件（Widget）,
- 比如：
  - dialog() 对话框
  - 



### dialog()

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |



## 5、JQuery 的 Validate 插件

参考：https://www.runoob.com/jquery/jquery-plugin-validate.html

- 表单校验插件



## 6、jQuery ligerUI 插件

官网：http://www.ligerui.com/demo.html

- 网页布局，UI组件



## 7、jQuery zTree插件

官网：http://www.treejs.cn/v3/main.php#_zTreeInfo

- 树形插件



简介：

 zTree 是一个依靠 jQuery 实现的多功能 “树插件”。优异的性能、灵活的配置、多种功能的组合是 zTree 最大优点。

  zTree 是开源免费的软件（MIT 许可证）。如果您对 zTree 感兴趣或者愿意资助 zTree 继续发展下去，可以进行捐助。

- zTree v3.0 将核心代码按照功能进行了分割，不需要的代码可以不用加载
- 采用了 ==延迟加载== 技术，上万节点轻松加载，即使在 IE6 下也能基本做到秒杀
- 兼容 IE、FireFox、Chrome、Opera、Safari 等浏览器
- 支持 ==JSON== 数据
- 支持静态 和==Ajax== 异步加载节点数据
- 支持任意更换皮肤 / 自定义图标（依靠css）
- 支持极其灵活的 checkbox 或 radio 选择功能
- 提供多种事件响应回调
- 灵活的编辑（增/删/改/查）功能，可随意拖拽节点，还可以多节点拖拽哟
- 在一个页面内可同时生成多个 Tree 实例
- 简单的参数配置实现 灵活多变的功能



方法：

|        方法名        | 参数       | 描述                   | 示例                                                         |
| :------------------: | ---------- | ---------------------- | ------------------------------------------------------------ |
| $.fn.zTree.destroy() | **treeId** | zTree 的 DOM 容器的 id | 1. 销毁 id 为 "treeDemo" 的 zTree<br />$.fn.zTree.destroy("treeDemo"); |
|                      |            |                        |                                                              |
|                      |            |                        |                                                              |

