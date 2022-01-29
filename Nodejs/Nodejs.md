[toc]

# Node.js

## 系统模块

`fs`模块，操作文件

- fs.readFile()	读取指定文件中的内容

- fs.writeFile()   向指定文件写入内容

  处理文件路径问题：__dirname	当前文件路径

`path`模块，处理路径

- path.join()	用来将多个路径片段拼接成一个完整的路径字符串

  涉及到路径拼接的操作

- path.basename()   用来从路径字符串中，将文件名解析出来

- path.extname()      获取路径中的拓展名部分

`http模块`，创建web服务器

- http.createServer()	
- res.end()		返回请求内容
- res.setHeader		解决返回内容中文乱码

## 模块化

- 内置模块
  - fs
  - path
  - http
  - etc…
- 自定义模块
  - 用户直接创建的.js文件
- 第三方模块
  - 第三方开发模块，需要下载

### 模块作用域

> 类似于函数作用域，在自定义模块中定义的变量，方法等成员，只能在当前模块内被访问

作用：防止全局变量污染的问题

- module对象 （向外共享模块作用域中的成员）

在每一个.js自定义模块中都有一个module对象，存储了和当前模块有关的信息

- module.exports()对象

在自定义模块中，使用该对象，将模块内的成员共享出去，供外界使用

## npm与包

包：第三方模块

- node_modules文件夹存放所有已安装到项目中的包
- package-lock.json 配置文件记录node_modules目录下的每一个包的下载信息

> npm i	一次性安装所有的依赖包
>
> npm uninstall	卸载指定的包
>
> npm i 包名 -D	记录到devDependencie节点

## 模块加载机制



# express

> 基于Node.js的web开发框架

- 启动web服务器

		const express = require('express')
		
		const app = express()
		
		app.listen(80, () => {
  		console.log('server is runing at http://127.0.0.1');
		})
	
- 监听客户端`GET`请求和`POST`请求

		app.get('/', (req, res) => {
  		res.send('404')		//res.send 响应一个文本字符串
		})
		
		app.post('/', (req, res) => {
  		res.send('404')
		})

## 获取URL中携带的查询参数

- 通过`req.query`对象，可以访问到客户端通过查询字符串的形式，发送到服务器的参数

- 通过`req.params`对象，可以访问到URL中，通过：匹配到的==动态参数==

  获取URL中的动态参数

## 托管静态资源

- `express.static()`，可以创建一个静态资源服务器

  将public目录下的图片，CSS文件，JS文件对外开放访问

  	app.use(express.static('public'))
  
- 挂载路径前缀

  		app.use('/public', express.static('public'))
  	  
   	通过带有/public前缀地址来访问public目录中的文件

## 路由

> 广义上来讲，路由就是映射关系
>
> `express`中，路由指==客户端请求==与==服务端处理函数==之间的映射关系

- 组成：请求的类型，请求的URL地址，处理函数

- 格式：		

  		app.METHOD(PATH, function)

### 模块化路由

express不建议将路由直接挂载到app上，而是推荐将路由抽离为单独的模块

步骤：

1. 调用express.Router()函数创建路由对象
2. 向路由对象上挂载具体的路由
3. 使用module.exports 向外共享路由对象
4. 使用app.use()函数注册路由模块

> `app.use()`函数的作用，就是来注册全局中间件

## 中间件

> 对请求进行预处理

==注意：==中间件函数的形参列表中，必须包含next参数；而路由处理函数只包含req和res

- next函数作用

  实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由

- 全局生效的中间件 （客户端发起的任何请求，到达服务器后，都会触发的中间件）

  通过调用 app.use(中间件函数)，即可定义一个全局生效的中间件

- 不使用 app.use() 定义的中间件，叫局部生效的中间件，只在当前路由中生效

`注意事项`

1. 要在路由之前注册中间件
2. 客户端发送过来的请求，可以连续调用多个中间件进行处理
3. 不要忘记调用next() 函数
4. next() 后面不要再写额外的代码
5. 连续调用多个中间件时，多个中间件之间，共享req和res对象

## 接口

- 接口的跨域问题

解决方法：

1. CORS（主流方案，推荐）
2. JSONP（有缺陷的方案，只支持GET请求）

