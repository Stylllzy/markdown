# BOM && PC端网页特效

[toc] 

## BOM

> Browser Object Model （浏览器模板对象）

提供独立于内容与浏览器窗口进行交互的对象，核心对象是==window==

BOM比DOM更大，包含DOM

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220109090154144.png" alt="image-20220109090154144" style="zoom: 67%;" />

### window对象的常见事件

#### 窗口加载事件

> window.onload

当文档内容完全加载完成会触发事件

#### 调整窗口大小事件

> window.onresize

窗口大小发生像素变化，触发该事件

### 定时器

#### setTimeout()

> window.setTimeout(调用函数，[延迟的毫秒数]);

==回调函数==,需要等待时间，时间到了再调用

- 停止setTimeout()计时器

  window.clearTimeout(timeout ID) //取消先前通过调用setTimeout()建立的计时器

#### setInterval()

> window.setInterval(回调函数，[间隔的毫秒数]);

重复调用一个函数，每隔这个时间，就去调用一次回调函数

- 停止setInterval()计时器

  clearInterval();

### JS执行机制

> 同步和异步

先执行同步任务，再执行异步任务（回调函数）

### location 对象

获取URL等信息

### navigator 对象

包含有关浏览器的信息，最常用的是==userAgent==属性，该属性可以返回由客户机发送服务器的user-agent头部的值

### history 对象

与浏览器历史记录进行交互，该对象包含用户访问过的URL

- back()        后退
- forward()   前进
- go(参数)    前进后退功能  参数为1，前进1个页面，参数为-1，后退1个页面



## PC

### 元素偏移量offset

动态得到该元素的位置（偏移），大小等

![image-20220109124721183](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220109124721183.png)

> offset 与 style 区别

- 获取元素大小位置，用offset
- 更改元素的值，用style

### 元素scroll

动态得到该元素的大小，滚动距离

![image-20220109163504802](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220109163504802.png)

> ==注意==：

- offset系列用于获得元素位置	offsetLeft	offsetTop
- client用于获取元素大小     clientWidth   clientHeight
- scroll用于获取滚动距离      scrollTop     scrollLeft

> 注意页面滚动的距离通过  ==window.pageXOffset== 获得 （这是 `scrollX 的别名`）

### mouseenter 和 mouseover 的区别

mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发；

mouseenter 只会经过自身盒子触发（mouseenter 不会冒泡）

### 动画函数封装

> 实现核心原理

==通过定时器setInterval() 不断移动盒子位置==

### 立即执行函数

(function() {})()	或者	(function(){}());

作用：独立创建了一个作用域 



## 案例

### 节流阀

> 防止轮播图按钮连续点击造成播放过快

核心思路：利用回调函数，添加一个变量来控制，锁住函数和解锁函数

### 带有动画的返回顶部

