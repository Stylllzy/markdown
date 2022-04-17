# Vuex

> 为Vue.js 应用程序开发的==状态管理模式==

- 它采用 集中式存储管理 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
- Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

<img src="https://img-blog.csdnimg.cn/20200114014359407.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1eXhpbnU=,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"  />

<img src="https://img-blog.csdnimg.cn/20200114014421796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1eXhpbnU=,size_16,color_FFFFFF,t_70" alt="img"  />

## 核心概念

- State（单一状态树）

- Getters

- Mutation

  > Vuex的store状态的更新唯一方式：提交Mutation

  - 定义：

    ```js
    mutations: {
    	increment(state) {
    		state.count++
    	}
    }
    ```

  - 更新：

    ```
    increment: function() {
    	this.$store.commit('increment')
    }
    ```

  - Mutation传递参数

    在通过mutation更新数据的时候, 有可能我们希望携带一些额外的参数
    参数被称为是mutation的载荷(Payload);

    当有多个参数时，payload是一个对象

  - 提交风格

    普通：commit

    type：

    ```
    this.$store.commit({
    	type:'changeCount',
    	count:100
    })
    ```

  - 响应规则

    - 提前在store中初始化好所需的属性
    - 当给state中的对象添加新属性时, 使用下面的方式:
      - 方式一: 使用Vue.set(obj, ‘newProp’, 123)
      - 方式二: 用心对象给旧对象重新赋值

  - 同步函数

    通常情况下, Vuex要求我们Mutation中的方法必须是同步方法

- Action

  代替Mutation进行异步操作

  context是和store对象具有相同方法和属性的对象

  ```
  actions: {
  	increment(context) {
  		context.commit('increment')
  	}
  }
  ```

  在Vue组件中使用dispatch

  **Action返回的Promise**

  在action 的increment 函数返回一个Promise 对象，在Promise对象里 resolve 写异步操作，在Vue实例方法中dispatch操作后 .then(res => {console.log(‘操作’)})

- Module

  Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutation、action、getters等

  - actions写法

    接收一个context参数对象，局部状态通过 context.state 暴露出来，根节点状态则为 context.rootState

    如果getters中也需要使用全局的状态, 可以接受更多的参数

- 项目结构

![img](https://img-blog.csdnimg.cn/20200114014933843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1eXhpbnU=,size_16,color_FFFFFF,t_70)

