# axios（Ajax	i/o	system）

## jsonp

常见的网络请求方式就是JSONP，使用JSONP最主要的原因往往是为了解决==跨域访问==的问题.

## axios特点

- 在浏览器中发送 XMLHttpRequests 请求
- 在 node.js 中发送 http请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 等等

## axios请求方式

- axios(config)
- axios.request(config)
- axios.get(url[, config])
- axios.delete(url[, config])
- axios.head(url[, config])
- axios.post(url[, data[, config]])
- axios.put(url[, data[, config]])
- axios.patch(url[, data[, config]])

### 发送并发请求

有时候, 我们可能需求同时发送两个请求
使用axios.all, 可以放入多个请求的数组.
axios.all([]) 返回的结果是一个数组，使用 axios.spread 可将数组 [res1,res2] 展开为 res1, res2

## axios的实例

为什么要创建axios的实例呢?

- 当我们从axios模块中导入对象时, 使用的实例是默认的实例.
- 当给该实例设置一些默认配置时, 这些配置就被固定下来了.
- 但是后续开发中, 某些配置可能会不太一样.
- 比如某些请求需要使用特定的baseURL或者timeout或者content-Type等.
- 这个时候, 我们就可以创建新的实例, 并且传入属于该实例的配置信息.

