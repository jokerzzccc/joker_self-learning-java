# 目录

[TOC]

# VUE2.5

始于：2021-05-29

学习资源：

- 尚硅谷的 Vue 学习视频：https://www.bilibili.com/video/BV1hs411E7ps
- VUE 2 官方教程（中文）：https://cn.vuejs.org/v2/guide/
  - 其实学习流程就是按照官网教程来的
- VUE 2 官方 API 文档：https://cn.vuejs.org/v2/api/



学习前需具备：

- HTML
- CSS
- JS
- ES6 语法



# 1、认识 VUE

MVVM:

- Model
- view
- ViewMode:
  - Data bindings（数据绑定）
  - DOM Listeners （DOM 监听）





vue 的浏览器调试插件：Vue devtools



Vue 是==声明式编程==



声明式与命令式的区别：

- 声明式：就是只需要声明，它就会自己做
- 命令式：都需要自己去做



每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的

```js
var vm = new Vue({
  // 选项
})
```



## 1.1 模板语法

1. 模板的理解：
   - 动态的 HTML 页面
   - 包含了 JS 语法的代码
     - 双大括号表达式
     - 指令：（以 `v-` 开头的自定义标签属性）
2. 双大括号表达式
   - 语法：`{{exp}}`
   - 功能：
     - 向页面输出数据
     - 可以调用对象的方法

3. 指令一：强制数据绑定
   - 功能：指定变化的属性值
   - 完整写法：`v-bind: xxx='yyy'` //yyy会作为==表达式解析==
   - 简介写法：`:xxx= 'yyy'`
4. 指令二：绑定事件监听
   - 功能：绑定指定事件名的回调函数
   - 完整写法：`v-on: click='xxx'`
   - 简洁写法：`@click= 'xxx'`
5. 



## 1.2 计算属性和监视

属性：就是，在 vue 实例的属性，

```js
new Vue({
    el: '#deomo'
    data: {
    	name1: 'a',
    	name2: 'b'
	},
    computed: {
        test(){
    	
	}
        },
    watch: {
        name1: function(value){
            
        }
    }
})
```



vue 实例的 this 都是指 vm 实例对象



计算属性：

- computed
- 在 computed 计算属性对象中定义计算属性的方法
- 在页面中使用 `{{方法名}}` 来显示计算的结果
- 什么时候执行：
  - 初始化显示
  - 相关的 data 属性数据发生改变





监听属性：

- 通过 vm 对象的 `$watch()` 或者 `watch`配置 来监视指定的属性
- 当属性变化时，回调函数自动调用，在函数内部进行计算



```html
watch: {

}


vm.$watch()
```





计算属性高级：

- 通过 `getter/setter` 实现对属性数据的显示和监视
- 计算属性存在在缓存，多次读取 只执行一次 `getter` 计算



### 回调函数

满足三个条件：

- 你定义的
- 你没有调用
- 但最终它执行了



回调函数：

`get`

- 什么时候调用：
  - 当需要读取当前属性值时回调
- 用来做什么：
  - 根据相关的数据，计算并返回当前属性值

`set`

- 什么时候调用：
  - 当属性值发生改变时
- 用来做什么：
  - 更新相关的属性数据



## 1.3 Class 与 Style 绑定

理解：

### 绑定 HTML Class

包含：

- 对象语法
- 数组语法
- 用在组件上





- 给 `v-bind:class` 一个对象，以动态地切换 class
- 我们也可以在这里绑定一个返回对象的[计算属性](https://cn.vuejs.org/v2/guide/computed.html)。这是一个常用且强大的模式：





### 绑定内联样式

- `v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。
- CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名



### truthy 值（真值）

- https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy

在 [JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/JavaScript) 中，**truthy**（真值）指的是在[布尔值](https://developer.mozilla.org/zh-CN/docs/Glossary/Boolean)上下文中，转换后的值为真的值。所有值都是真值，除非它们被定义为 [假值](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)（即除 `false`、`0`、`""`、`null`、`undefined` 和 `NaN` 以外皆为真值）。



## 1.4 条件渲染

- `v-if`
- `v-else`
- `v-else-if`
- `v-show`
- `v-for`

都是接指令。

### v-if

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

也可以用 `v-else` 添加一个“else 块”：

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```



### v-show

另一个用于根据条件展示元素的选项是 `v-show` 指令。

- 不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。
- 注意，`v-show` 不支持 `<template>` 元素，也不支持 `v-else`。



==用key 管理可复用的元素==

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```



### `v-if` vs `v-show`



- `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

- `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

- 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

- 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。





## 1.5 列表渲染（v-for）

### v-for 使用数组

- `v-for` 指令基于一个==数组==来渲染一个列表。

- `v-for` 指令需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据数组，而 `item` 则是被迭代的数组元素的**别名**。
- 在 `v-for` 块中，我们可以访问所有父作用域的 property。
- `v-for` 还支持一个可选的第二个参数，即当前项的索引。
- 可以用 `of` 替代 `in` 作为分隔符

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
```

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```



### v-for 使用对象

- 用 `v-for` 来遍历一个对象的 property。
- 可以提供第二个的参数为 property 名称 (也就是键名)（name 可选）
- 还可以用第三个参数作为索引：（index 可选）

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```



### 维护状态使用 key

- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` attribute：

- 建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

```html
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```





### 数组更新检测

- 变更方法
- 替换数组

### 显示过滤/排序后的结果

- 有时，我们想要显示一个数组经过过滤或排序后的版本，而不实际变更或重置原始数据。在这种情况下，可以创建一个计算属性，来返回过滤或排序后的数组。

### [在 `v-for` 里使用值范围](https://cn.vuejs.org/v2/guide/list.html#在-v-for-里使用值范围)

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```





### 在`<template>` 里使用 `v-for`

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```



### [`v-for` 与 `v-if` 一同使用](https://cn.vuejs.org/v2/guide/list.html#v-for-与-v-if-一同使用)

- **不**推荐在同一元素上使用 `v-if` 和 `v-for`
- 当它们处于同一节点，`v-for` 的优先级比 `v-if` 更高



### [在组件上使用 `v-for`](https://cn.vuejs.org/v2/guide/list.html#在组件上使用-v-for)



​	

## 1.6 事件处理

### 1、监听事件 v-on



### 2、事件处理方法

- 然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 还可以接收一个需要调用的方法名称。



### 3、内联处理器中的方法



### 4、事件修饰符

- 方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。
- 为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。
  - `.stop`
  - `.prevent`
  - `.capture`
  - `.self`
  - `.once`
  - `.passive`



注意：

- 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。



后面新增：

- `.once`
- Vue 还对应 [`addEventListener` 中的 `passive` 选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)提供了 `.passive` 修饰符。



### 5、[按键修饰符](https://cn.vuejs.org/v2/guide/events.html#按键修饰符)

#### [按键码](https://cn.vuejs.org/v2/guide/events.html#按键码)

`keyCode` 的事件用法[已经被废弃了](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)并可能不会被最新的浏览器支持。



### 6、[系统修饰键](https://cn.vuejs.org/v2/guide/events.html#系统修饰键)

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`



#### [`.exact` 修饰符](https://cn.vuejs.org/v2/guide/events.html#exact-修饰符)

> 2.5.0 新增

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

#### [鼠标按钮修饰符](https://cn.vuejs.org/v2/guide/events.html#鼠标按钮修饰符)

> 2.2.0 新增

- `.left`
- `.right`
- `.middle`



## 1.7 表单输入绑定

### 1、基础用法

- 你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。
- `v-model` 本质上不过是语法糖。
- 它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。



注意：

- `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。
- `v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：
  - text 和 textarea 元素使用 `value` property 和 `input` 事件；
  - checkbox 和 radio 使用 `checked` property 和 `change` 事件；
  - select 字段将 `value` 作为 prop 并将 `change` 作为事件。





用法包括：

- 文本
- 多行文本
- 复选框 checkbox
  - 单个复选框，绑定到布尔值：
  - 多个复选框，绑定到同一个数组：
- 单选按钮
- 选择框
- 



### 2、值绑定

- 对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

- 但是有时我们可能想把值绑定到 Vue 实例的一个动态 property 上，这时可以用 `v-bind` 实现，并且这个 property 的值可以不是字符串。



### 3、修饰符

- `.lazy`
  - 在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了[上述](https://cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：
- `.number`
  - 自动将用户的输入值转为数值类型
- `.tirm`
  - 自动过滤用户输入的首尾空白字符



```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">

<input v-model.number="age" type="number">

<input v-model.trim="msg">
```



### 4、[在组件上使用 `v-model`](https://cn.vuejs.org/v2/guide/forms.html#在组件上使用-v-model)





## 1.8 组件基础

### 1 基本实例

- 组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

- 我们可以在一个通过 `new Vue` 创建的 Vue 根实例中，把这个组件作为自定义元素来使用

### 2 组件的复用

- **一个组件的 `data` 选项必须是一个函数**

### 3 组件的组织

- 通常一个应用会以一棵嵌套的组件树的形式来组织
- 为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：**全局注册**和**局部注册**。至此，我们的组件都只是通过 `Vue.component` 全局注册的

### 4 [通过 Prop 向子组件传递数据](https://cn.vuejs.org/v2/guide/components.html#通过-Prop-向子组件传递数据)

- Prop 是你可以在组件上注册的一些自定义 attribute
- 当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。
- 



### 5 [单个根元素](https://cn.vuejs.org/v2/guide/components.html#单个根元素)



### 6 [监听子组件事件](https://cn.vuejs.org/v2/guide/components.html#监听子组件事件)



### 7 [通过插槽分发内容](https://cn.vuejs.org/v2/guide/components.html#通过插槽分发内容)



### 8 [动态组件](https://cn.vuejs.org/v2/guide/components.html#动态组件)





### 9 [解析 DOM 模板时的注意事项](https://cn.vuejs.org/v2/guide/components.html#解析-DOM-模板时的注意事项)



### 10 [解析 DOM 模板时的注意事项](https://cn.vuejs.org/v2/guide/components.html#解析-DOM-模板时的注意事项)



# 2、深入了解组件

## 2.1 组件注册

### 1 组件名

有两种方式：

- 使用 kebab-case 中划线(==推荐==)
- 使用 PascalCase 驼峰

```js
Vue.component('my-component-name', { /* ... */ })
Vue.component('MyComponentName', { /* ... */ })
```



### 2 全局注册

- 全局注册：注册之后可以用在任何新创建的 Vue 根实例 (`new Vue`) 的模板中。
- 在所有子组件中也是如此，也就是说这三个组件*在各自内部*也都可以相互使用。

```js
Vue.component('my-component-name', {
  // ... 选项 ...
})
```



### 3 局部注册

- **局部注册的组件在其子组件中\*不可用\***



在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 `components` 选项中定义你想要使用的组件：

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```



### 4 模块系统

#### [在模块系统中局部注册](https://cn.vuejs.org/v2/guide/components-registration.html#在模块系统中局部注册)

- 使用了诸如 Babel 和 webpack 的模块系统。在这些情况下，我们推荐创建一个 `components` 目录，并将每个组件放置在其各自的文件中。



然后你需要在局部注册之前导入每个你想使用的组件。

例如，在一个假设的 `ComponentB.js` 或 `ComponentB.vue` 文件中：

```
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
```

现在 `ComponentA` 和 `ComponentC` 都可以在 `ComponentB` 的模板中使用了。

#### [基础组件的自动化全局注册](https://cn.vuejs.org/v2/guide/components-registration.html#基础组件的自动化全局注册)

- 基础组件：可能你的许多组件只是包裹了一个输入框或按钮之类的元素，是相对通用的。我们有时候会把它们称为[基础组件](https://cn.vuejs.org/v2/style-guide/#基础组件名-强烈推荐)，它们会在各个组件中被频繁的用到。
- 

如果你恰好使用了 webpack (或在内部使用了 webpack 的 [Vue CLI 3+](https://github.com/vuejs/vue-cli))，那么就可以使用 `require.context` 只全局注册这些非常通用的基础组件。这里有一份可以让你在应用入口文件 (比如 `src/main.js`) 中全局导入基础组件的示例代码：

```js
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 获取和目录深度无关的文件名
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```



**==全局注册的行为必须在根 Vue 实例 (通过 `new Vue`) 创建之前发生==**。



## 2.2 Prop (属性)

### 1 Prop 的大小写

HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名.



### 2 Prop 类型





### 3 [传递静态或动态 Prop](https://cn.vuejs.org/v2/guide/components-props.html#传递静态或动态-Prop)

-  prop 可以通过 `v-bind` 动态赋值



#### 1 [传入一个数字](https://cn.vuejs.org/v2/guide/components-props.html#传入一个数字)

```html
<!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:likes="42"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:likes="post.likes"></blog-post>
```



#### 2 [传入一个布尔值](https://cn.vuejs.org/v2/guide/components-props.html#传入一个布尔值)

```html
<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

#### 3 [传入一个数组](https://cn.vuejs.org/v2/guide/components-props.html#传入一个数组)

```html
<!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
```

#### 4 [传入一个对象](https://cn.vuejs.org/v2/guide/components-props.html#传入一个对象)

```html
<!-- 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post
  v-bind:author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:author="post.author"></blog-post>
```

#### 5 [传入一个对象的所有 property](https://cn.vuejs.org/v2/guide/components-props.html#传入一个对象的所有-property)

```html
如果你想要将一个对象的所有 property 都作为 prop 传入，你可以使用不带参数的 v-bind (取代 v-bind:prop-name)。例如，对于一个给定的对象 post：

post: {
  id: 1,
  title: 'My Journey with Vue'
}
下面的模板：

<blog-post v-bind="post"></blog-post>
等价于：

<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```

### 4 单向数据流

- 所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**

这里有两种常见的试图变更一个 prop 的情形：

1. **这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。**在这种情况下，最好定义一个本地的 data property 并将这个 prop 用作其初始值：

   ```js
   props: ['initialCounter'],
   data: function () {
     return {
       counter: this.initialCounter
     }
   }
   ```

2. **这个 prop 以一种原始的值传入且需要进行转换。**在这种情况下，最好使用这个 prop 的值来定义一个计算属性：

   ```js
   props: ['size'],
   computed: {
     normalizedSize: function () {
       return this.size.trim().toLowerCase()
     }
   }
   ```



> 注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变变更这个对象或数组本身**将会**影响到父组件的状态。



###  5 [Prop 验证](https://cn.vuejs.org/v2/guide/components-props.html#Prop-验证)

- 我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```



> 注意那些 prop 会在一个组件实例创建**之前**进行验证，所以实例的 property (如 `data`、`computed` 等) 在 `default` 或 `validator` 函数中是不可用的。





#### [类型检查](https://cn.vuejs.org/v2/guide/components-props.html#类型检查)

`type` 可以是下列原生构造函数中的一个：

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

额外的，`type` 还可以是一个自定义的构造函数，并且通过 `instanceof` 来进行检查确认。例如，给定下列现成的构造函数

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```



### 6 [非 Prop 的 Attribute](https://cn.vuejs.org/v2/guide/components-props.html#非-Prop-的-Attribute)

- 一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 prop 定义的 attribute。



#### [替换/合并已有的 Attribute](https://cn.vuejs.org/v2/guide/components-props.html#替换-合并已有的-Attribute)

- 对于绝大多数 attribute 来说，从外部提供给组件的值会替换掉组件内部设置好的值。
- 庆幸的是，`class` 和 `style` attribute 会稍微智能一些，即两边的值会被合并起来，从而得到最终的值：



#### [禁用 Attribute 继承](https://cn.vuejs.org/v2/guide/components-props.html#禁用-Attribute-继承)

如果你**不**希望组件的根元素继承 attribute，你可以在组件的选项中设置 `inheritAttrs: false`。例如：

```js
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```

这尤其适合配合实例的 `$attrs` property 使用，该 property 包含了传递给一个组件的 attribute 名和 attribute 值，例如：

```js
{
  required: true,
  placeholder: 'Enter your username'
}
```



## 2.3 自定义事件

### 1 事件名 

- 不同于组件和 prop，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。
  - 就是驼峰命名和短横线不会再转换了
  - 



>  并且 `v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。推荐你**始终使用 kebab-case 的事件名**。





### 2 [自定义组件的 `v-model`](https://cn.vuejs.org/v2/guide/components-custom-events.html#自定义组件的-v-model)





### 3 [将原生事件绑定到组件](https://cn.vuejs.org/v2/guide/components-custom-events.html#将原生事件绑定到组件)



### 4 [`.sync` 修饰符](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-修饰符)



> 注意带有 `.sync` 修饰符的 `v-bind` **不能**和表达式一起使用 (例如 `v-bind:title.sync=”doc.title + ‘!’”` 是无效的)。取而代之的是，你只能提供你想要绑定的 property 名，类似 `v-model`。



## 2.4 插槽

















# Vue 常用指令

## v-html

- innerHtml
- 就是把字符串理解为 html 语句，

```html
<p v-html='msg'>
 
</p>
```



## v-text

- textContent
- 就直接理解为字符串

```html
<p v-text='msg'>
 
</p>
```



## v-bind

- 强制数据绑定

- 会把标签里的 ，作为表达式解析

比如

```html
<img v-bind:src="imgUrl">
// 或者简写
<img :src="imgUrl">
```



## v-on

- 语法：`v-on:事件名="方法名"`
  - 简写：`@事件名="方法名"`

比如：

```html
<button v-on:click="test">
    
</button>
// 简写
<button @click="test">
    
</button>
```



## v-model

- 就是 

























# 思考：

## 1、property 与 attribute 

property : 更多是和对象有关的属性

attribute: 更多是和 HTML 标签的属性相关 ，比如： id, class。。。





# OVER