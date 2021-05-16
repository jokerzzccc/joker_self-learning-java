# VUE

SSM回顾

mybatis:

spring;

sprinmvc

JavaScript：主要学习：函数，标准对象，操作BOM,DOM	



**vuej简介：**

- vue 是一套用于构建用户界面的渐进式框架，

- Vue被设计为可以自底层向上逐渐应用。

- Vue的核心库只关注视图层，不仅易上手，还便于第三方库（如：vue-router:页面跳转 ， vue-resource:通信， vuex:管理）或既有项目整合

官网：https://v3.vuejs.org/guide/introduction.html

现在最新版本式v3

# 1、前端简介

- SoC原则（Separation of Concerns）关注点分离原则
- HTML（结构）+CSS（表现层）+JS（行为）：视图层
- vue只负责视图层



网络通信： axios

页面跳转：vue-router

状态管理：vuex

vue-UI : ICEworks

前端知识体系：

1. HTMl (结构）：决定网页的结构和内容
2. CSS （表现）：设计网页的表现样式
3. Javascript(行为)：是一种弱类型脚本语言，源代码不需要经过编译，而是由浏览器解释运行，用于控制网页的行为

## **2.1 CSS** 

![image-20210313163911955](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313163911955.png)

## **2.2 CSS 预处理器**

![image-20210313164025131](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313164025131.png)

- 前端css 开发基本都用 **LESS** 
- LESS 官网：https://less.bootcss.com/

## **3.1 javaScript** 

![image-20210313164529975](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313164814254.png)

![image-20210313164814254](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313165618705.png)

- 现在最多的是ES6
- webpack 打包



## **3.2 javaScript 框架**

![image-20210313164850773](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313164529975.png)

- Angular : 最先提出 MVVM
- React : 虚拟DOM，就是利用内存
- MVVM：双向数据绑定
- VUE : 支持MVVM+DOM 。即综合了Angular + react
- 前端的三大框架 ： Angular ， react , VUE
- Axios:前端通信框架



## 4.1 UI 框架

![image-20210313165618705](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313165826986.png)

- AmazeUI用的也很多
- Bootstrap ,最老资格的

## 4.2 javascript 构建工具

​	![image-20210313165652463](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313170214293.png)

- 都用webpack



## 5.1 三段统一

![image-20210313165826986](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313165652463.png)

## 6.1 后端技术

![image-20210313165941276](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313165941276.png)

- 重点，npm : js包管理工具 ，类似maven

## 6.2 主流前端框架

![image-20210313170214293](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313171000988.png)

- iview : https://www.iviewui.com/docs/introduce

- elementUI : https://element.eleme.cn/#/zh-CN/component/installation
- vue-element-admin : https://panjiachen.gitee.io/vue-element-admin-site/zh/ ==**很重要**==，必须学会

其他了解就好

![image-20210313171000988](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313171035401.png)

## 7.1 前后端分离史

- springmvc核心

![image-20210313171035401](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172305765.png)

**mvc的缺点**

![image-20210313171214844](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172644421.png)



## 8、ajax spa时代

![image-20210313172214103](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172214103.png)

![image-20210313172305765](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172526119.png)

![image-20210313172334686](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172659458.png)

## 9 mv* 时代-大前端时代

![image-20210313172526119](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172334686.png)

![image-20210313172644421](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173043012.png)

![image-20210313172659458](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313172958030.png)

- SEO ：搜索引擎优化

## 10 NodeJS 带来的全栈时代

![image-20210313172958030](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313175358582.png)

![image-20210313173043012](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313180148787.png)

![image-20210313173155273](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173409456.png)



# 2、第一个 VUE程序

## 2.1 MVVM

mvvm:双向数据绑定：

mvvm组成部分：

![image-20210313173409456](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173155273.png)

- mvvm 可以即时运行，即时编译，就是虚拟dom

**什么是MVVM**

![image-20210313175358582](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313175639149.png)	**为社么使用MVVM**

![image-20210313175639149](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313180250872.png)



**三层的理解**

![image-20210313180148787](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313180210516.png)

![image-20210313180210516](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173615058.png)

![image-20210313180250872](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173630684.png)



## 2.2 VUE

![image-20210313173615058](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313180429030.png)

![image-20210313173630684](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313173710233.png)

![image-20210313173710233](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313183616507.png)



## 2.3 开发第一个vue程序

- 前端正常开发用的工具： Vscode , Hbuilder , Sublime , WebStorm
- 但，我用IDEA **,安装vue.js插件**

- vue3 : https://v3.vuejs.org/guide/introduction.html

- vue cdn : https://cdn.baomitu.com/vue

- 本例使用版本 

- <script crossorigin="anonymous" integrity="sha512-h3aCJRk6tEHugDYUidF7GqixhzgeXSiSdq5U+5oLIJtIncSQq6eev48qqYfuTdrsH5Q1eO4IAmyJGDwzRaWNNQ==" src="https://lib.baomitu.com/vue/2.6.12/vue.common.dev.js"></script>

<script crossorigin="anonymous" integrity="sha512-PkyFg1MEb/EwsFAqzqvZqWdMT4ItV+E1NgOtfBC9X8UecJcO9bwirD+v/9tJwci4wTNHdNYBk4ev6ceb1hy73g==" src="https://lib.baomitu.com/vue/2.6.12/vue.common.dev.min.js"></script>

min 是迷你版，用下一个就好	

![image-20210313180429030](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313185001950.png)

## 2.4 vue 基本语法

- 前端的逻辑，无所谓就是 **判断，循环，本来就显示**

### 1 判断，循环

#### v-bind

- 关联哪个变量

#### v-if , v-else，v-else-if



#### v-for



### 2 事件

#### v-on

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

方法必须放在vue 的 methods 中， 

v-on 后面加的事件 ，有 ：jquery 文档里查看事件哪些 https://www.w3school.com.cn/jquery/jquery_ref_events.asp



### 3 vue 常用7 大属性

![image-20210313183616507](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313185217089.png)

## 2.5 vue 双向数据绑定

![image-20210313183753010](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313185123001.png)

### v-model

注意 **下拉框**的实现 ：

![image-20210313185001950](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313190201796.png)

## 2.6 vue 组件

### Vue.component()

**注册组件**

![image-20210313185123001](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313185341609.png)

**什么是组件**

- 组件是可复用的 Vue 实例，且带有一个名字
- 我们可以在一个通过 `new Vue` 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：
- 因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

![image-20210313185217089](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313190214158.png)

![image-20210313185341609](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313213844702.png)

**组件代码示例**

- 需要参数传递到组件，就必须用props 属性

![image-20210313190214158](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/lifecycle.png)

![image-20210313190201796](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313214354087.png)



## 2.7 网络通信-Axios

- 难点
- 实现网络通信也可以用 JQuery.ajax()

### 什么是 Axios

- Axios  官网 http://axios-js.com/
- GitHub https://github.com/axios/axios/

![image-20210313213844702](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313220418411.png)



### 为什么用Axios

![image-20210313214354087](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313220504208.png)



### axios **生命周期**（Lifecycle）

![Vue 实例生命周期](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313222632831.png)

即：

![image-20210313220418411](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313223335908.png)

![image-20210313220504208](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313220625035.png)

![image-20210313220625035](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313230241631.png)





### 钩子函数

monted() 

链式编程，ES6新特性



# 3 、计算属性（computed）

- 计算属性：计算出来的结果，保存在属性中。内存中运行虚拟dom
- 是 vue 的一个特色
- ![image-20210313222632831](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313225416915.png)

![image-20210313223335908](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313230132563.png)

# 4、内容分发 - 插槽-slot

slot 插槽 <slot>  **很重要**，难点

- 实现**动态拔插**，提高复用，



# 5、自定义事件

**难点**，重要

![image-20210313225416915](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313230708509.png)

Vue.component

emit

## 代码总结

![image-20210313230132563](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313230856387.png)

![image-20210313230241631](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313230733031.png)



# 6 vue 入门小结

![image-20210313230708509](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313231509630.png)

![image-20210313230733031](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313232001309.png)



![image-20210313230856387](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313232927039.png)

- NodeJS 
- **实际开发**用  vue-cli 脚手架，vue-router 路由跳转，vuex 状态管理 ， vueUI, 界面一般使用 ElementUI，或者 ICE 来快速搭建前端项目



# 7、第一个vue-cli 项目

**什么是vue-cli**

![image-20210313231509630](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105033291.png)

## 安装**需要的环境**

1、这里先安装，Nodejs

安装了Nodejs,就会有npm

在cmd里，输入

2、安装淘宝镜像 ： npm install -g cnpm -registry=https://registry.npm.taobao.org

- 注意：尽量少用cnpm,除非是国外的下载不了，就可以用了，因为，它容易出问题

3、安装vue-cli ：cnpm install -g @vue/cli

测试是否安装成功 vue -V

以上安装路径，都是我测试成功的

![image-20210313232001309](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314104826172.png)

![image-20210313232927039](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105010751.png)



## 创建一个vue程序

vue-cli 官网： https://cli.vuejs.org/zh/

1. 创建一个项目文件夹 ，比如 D:\2021_java_project\vue	
2. 用管理员模式 运行命令行，进入到我们的项目文件夹      f: | cd 2021_java_project\vue
3. 在vue 项目目录下， 输入，vue create my-vue
4. 然后就会有一堆选择，看情况自己选把，我这里，是安装某样的都是选no, 为了体验过程。vue build 选择 runtime + compiler
5. 创建完项目后，进入到myvue : 
   1. cd myvue
   2. npm install
   3. npm run serve 启动当前项目，就有点向tomcat l 
6. 如果报了什么错，根据npm的提示输入就可以了
7. 如果想要在 IDEA 里运行命令行终端，就 右键点击idea 图标|属性|兼容性|管理员身份运行



直接用idea 打开项目文件 myvue

**分析项目结构：**

- src 放置源码的
- build 构建 构建这个项目时的
- config 配置文件 端口号，就可以在 config|index.js 里面改





# 8、webpack

## 什么是webpack

![image-20210314104550664](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105352253.png)



## 模块化的演进

### script 标签

![image-20210314104826172](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105216659.png)



### CommonsJS

![image-20210314105010751](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105256366.png)

![image-20210314105033291](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105326803.png)



### AMD

![image-20210314105216659](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105931531.png)

![image-20210314105256366](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314105443660.png)

### CMD

![image-20210314105326803](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314111111834.png)

![image-20210314105352253](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314110036900.png)



### ES6

![image-20210314105443660](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314111827586.png)

![image-20210314105931531](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314111845876.png)



### webpack

- 作用，就是一个打包工具。把es6规范的代码，打包成es5规范，让所有浏览器都可以用

- 前端的模块化开发

- 安装webpack

  - npm install webpack -g
  - npm install webpack-cli -g

  查看是否安装成功 webpack -v

  

![image-20210314110036900](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314121742799.png)



**使用webpack**

![image-20210314111111834](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314122609129.png)



![image-20210314111827586](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314112502064.png)

![image-20210314111845876](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314130452206.png)





# 9、vue-router 路由

- 页面的跳转：转发，重定向，交给前端的vue-router 来做，因为vue本身只关注视图层
- 导入vue-router : npm install vue-router --save-dev
- 导入之后，需要显示声明，即，Vue.user(VueRouter);

vue-router 简介

官网：https://router.vuejs.org/zh/guide/#html



vue-router的安装

![image-20210314112502064](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314130034313.png)

 

- 如果vue文件里的，style标签，添加了scoped ,即表示，这个style只在本身vue文件生效，

- 路由，新建一个专门配置路由的文件夹，在src|router|index.js ，这里的index.js里，就放置路由

- 在app.vue 使用的时候，必须有两个

  - ```
    <router-link to="/content">内容页面</router-link>//跳转链接
    <router-view></router-view>//显示视图的
    ```



# 10、第一个项目hello-vue

![image-20210314121742799](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314130731102.png)

![image-20210314122609129](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313164850773.png)

1. 管理员命令行 进入到 项目放置的根目录
2. vue init webpack hello-vue
3. cd hello-vue
4. npm  install vue-router --save-dev
5. npm i element-ui -S
6. npm install
7. cnpm install sass-loader node-sass --save-dev
8. npm run dev



- jquery弹窗 layer插件 https://layer.layui.com/



# 11 、路由相关

## 1、嵌套路由

- 属于vue-router ,看官网

## 2 、参数传递，重定向

- 属于vue-router ,看官网

## 3、路由模式与404

- 属于vue-router ,看官网
  - hash
  - history
- ![image-20210314130034313](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313171214844.png)

- 404 界面，直接添加一个组件
- 放到路由器，path: '*'

## 4、路由钩子 与异步请求

路由钩子：

- beforeRouteEnter ，表示进入路由之前。可以用来添加加载数据
- beforeRouteLeave

这两个钩子函数，可以与axios 结合，实现异步请求

![image-20210314130452206](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210313183753010.png)

![image-20210314130731102](https://2021-joker.oss-cn-shanghai.aliyuncs.com/java_img/image-20210314104550664.png)

专业词汇,mock 文件夹用来放测试数据

Axios : 官网 http://axios-js.com/zh-cn/docs/vue-axios.html

