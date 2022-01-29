[toc]



# Vue

`编程范式：`声明式编程

原生js：命令式编程

## 创建Vue实例对象

传入 options：{}

- {} 包含 el属性：

  string	|	HTMLElement

  该属性决定 Vue 对象挂载到哪一个DOM元素

- {} 包含 data 属性：`（组件当中 data 必须是一个函数）`

  Object	|	Function

  存储数据（直接定义 或 从服务器加载）

- {} 包含 methods 属性：

  { [key: string]:Function }

  定义属于Vue的一些方法，可以在其他地方调用，也可以在指令中调用
  
- {} 包含 computed 属性：==计算属性==

  `计算属性和methods的区别`

  - 计算属性 性能更好。会进行缓存，如果多次使用，只调用一次

## Vue中的 MVVM

`Model–view–viewmodel`，一种软件[架构模式](https://zh.wikipedia.org/wiki/架构模式)

- View 层

  视图层，DOM层

  给用户展示各种信息

- Model 层

  数据层，可以是死数据，也可以是来自服务器，从网络上请求下来的数据

- VueModel 层

  视图模型层，是 View 和 Model 沟通的桥梁

  - 一方面实现 Data Binding （数据绑定），将 Model 的改变实时反应到 View
  - 另一方面实现 DOM Listen （DOM 监听），当 DOM 发生一些事件（点击，滚动等），可以监听到，并在需要的情况下改变对应的 Data

## Vue 生命周期

过程:	template -> ast(抽象语法树) ->render(渲染函数) -> 虚拟DOM -> UI(真实DOM)

`生命周期钩子`函数

比如 [`created`](https://cn.vuejs.org/v2/api/#created) 钩子可以用来在一个实例被创建之后执行代码：

	new Vue({
		data: {
			a: 1
		},
		created: function () {
			// `this` 指向 vm 实例
			console.log('a is: ' + this.a)
		}
	})
	// => "a is: 1"

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 [`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例

不要在选项 property 或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

## 语法糖

v-bind		：

v-on			 @

### v-on修饰符

```
<!--  停止冒泡-->
  <button @click.stop="doThis"></button>
<!--  阻止默认行为-->
  <button @click.prevent="doThis"></button>
<!--  阻止默认行为,没有表达式-->
  <form @submit.prevent></form>
<!--  串联修饰符-->
  <button @click.stop.prevent="doThis"></button>
<!--  键修饰符,键别名-->
  <input @keyup.enter="onEnter">
<!--  键修饰符,键代码-->
  <input @keyup.13="onEnter">
<!--  点击回调只会触发一次-->
  <button @click.once="doThis"></button>
```

### v-if	和	v-show

v-if 为 false，DOM 里没有该元素；v-show 为 false，display:none

> 切换频繁时，用v-show；	只有一次切换，用v-if

### v-for

- 获取 key 和 value ，格式：（value，key）
- 获取 key 和 value 和index，格式：（value，key，index）

> key 的作用是为了高效的更新 `虚拟DOM`

- 数组中哪些方法是响应式的

  - push方法

    	this.letters.push('aaa')
    
  - pop()：删除数组最后一个元素

    	this.letters.pop();
    
  - shift()：删除数组第一个元素

    	this letters.shift();
    
  - unshift()：在数组最前面添加元素

    	this.letters.unshift();
    
  - splice()：删除元素/插入元素/替换元素

    - 删除元素：第二个参数传入你要删除几个元素（如果没有传，就删除后面所有的元素）

      	this.letters.splice(1,3)

    - 替换元素：第二个参数，表示替换几个元素，后面是用于替换前面的元素

      	this.letters.splice(1,3,'m','n','l')

    - 插入元素：第二个参数，传入0，并且后面跟上要插入的元素

      	this.letters.splice(1,0,'x','y','z')	
		
	- set(要修改的对象，索引值，修改后的值)
	
	  	Vue.set(this.letters,0,'bbbb')

### v-model

实现表单元素和数据的双向绑定

实质是一个`语法糖`，本质包含两个操作：

- v-bind绑定一个value属性

- v-on指令给当前元素绑定input事件

	<input type="text" v-model="message">
	
	等同于
	
	<input type="text" v-bind:value="message" v-on:input="message = $event.target.value">

## 组件化

注册组件的基本步骤：

- 创建组件构造器	调用	Vue.extend()
- 注册组件                调用   Vue.component()
- 使用组件                

#### 全局组件和局部组件

通过调用Vue.component() 注册组件时，组件的注册是`全局`的

- 这意味着该组件可以在任意Vue示例中使用

如果注册组件是挂载在某个示例中，则为`局部组件`

#### 父组件和子组件

#### 组件应有自己保存数据的一个地址

组件里的data 应为函数类型,该函数返回一个对象	为什么？

​	因为data 如果为对象，和Vue实例中data 会共享地址，导致组件间会同步数据

​	为函数类型则不会共用数据，便于组件复用

#### 父子组件的通信

大组件将数据传递给小组件

- 通过 props 向子组件传递数据

- 通过事件向父组件发送消息

  - 在子组件中，通过$emit() 来触发事件
  - 在父组件中，通过v-on来监听子组件事件

  父模板绑定子组件的发射，绑定值是父组件定义的方法

> props驼峰标识，v-bind后面不支持驼峰

#### 父子组件的访问方式

父组件访问子组件：使用

- $children

  数组类型，包含所有子组件对象

- $refs（重点）

子组件访问父组件：使用$parent

### 组件化高级

#### 插槽

- 具名插槽

**`v-slot` 只能添加在 `<template>` 上** (只有[一种例外情况](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法))，这一点和已经废弃的 [`slot` attribute](https://cn.vuejs.org/v2/guide/components-slots.html#废弃了的语法) 不同

一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称

- 作用域插槽

## 模块化

### 模块化规范

CommonJS，AMD，CMD，Modules（ES6）

### 核心

> 导入，导出

- ES6

  export（导出），import（导入）

  类型需要设置为	module

  - 如果希望某个模块中所有信息都导入，使用 * 可以导入模块中所有的export变量

    	import * as '别名' from './index.js'
    	console.log('别名'.name)

# webpack

前端模块化打包工具

## grunt/gulp和webpack的区别

grunt/gulp 更加强调前端流程的自动化,模块化不是核心

webpack 更强调模块化开发管理,其中文件压缩合并,预处理等功能,是附带的功能

## webpack中的loader

- css-loader/style-loader
- less-loader
- url-loader/file-loader
- babel-loader                                                                                                                                                                                                                                                                                                                                                                                                                                                          

# Vue CLI@2

## runtime- compiler 和 runtime-only 的区别 

在 main.js文件中

only中没注册组件,直接使用render,周期变为	render -> vDOM -> UI

only 性能更高,代码量更少

# Vue CLI@3

## 与 Vue CLI @2区别

- vue-cli 3基于webpack 4,vue-cli2基于webpack3
- vue-cli3 设计原则"0配置",移除配置文件根目录下的 build 和 config 等目录
- vue-cli3提供 vue ui 命令,提供可视化配置
- 移除 static文件夹,新增 public 文件夹,并将 index.html 移动到public中

# Vue-Router (路由)

> 通过互联的网络把信息从源地址传输到目的地址的活动

1.后端渲染: jsp (Java server page)

​	后端路由: 后端处理URL 和 页面之间的映射关系

2.前端渲染: (前后端分离),浏览器中显示的大部分DOM内容,都是由前端写的js代码在浏览器中执行,渲染出来的网页

3.单页面富应用阶段(SPA):

​	主要特点:在前后端分离的基础上加了一层前端路由

​	前端路由:管理URL与静态资源服务器中对应组件的映射关系

```
<router-link>:该标签会被渲染成一个 <a>标签
<router-view>:该标签会根据当前的路径,动态渲染出不同的组件
网页其他内容,比如顶部的标题/导航,或者底部的版权信息等会和<router-view>处于同一个等级
路由切换时,切换的是<router-view>挂载的组件,其他内容不变
```

## 路由懒加载

主要作用:将路由对应的组件打包成一个个的js代码块,只有这个路由被访问到,才加载对应的组件

## $route 和 $router的区别

$router 为 VueRouter 实例，导航到不同URL，使用router.push

$route 为当前router跳转对象里面可以获取 name,path,query,params等

## 导航守卫

动态改变网页标题

```
router.beforeEach((to,from,next) => {
  // 从from 跳转到 to
  document.title = to.matched[0].meta.title
  console.log(to)
  // 执行下一步
  next()
})
```

> 补充:	如果是后置钩子（afterEach），不需要主动调用next()函数
>
> vuerouter项目中使用导航守卫为全局守卫
>
> - 路由独享的守卫
> - 组件内的守卫

## keep-alive

Vue内置的组件，可以被包含的组件保留状态，避免重新渲染

router-view也是一个组件，被包在keep-alive里面，所有路径匹配到的视图组件都会被缓存