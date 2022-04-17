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