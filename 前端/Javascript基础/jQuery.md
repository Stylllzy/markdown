# jQuery

[toc]



## 入口函数

- 第一种写法
  $(document).ready(function() {
  	
  });

- 第二种写法

  $(function() {  

  });

> JS的入口函数要等到页面中所有资源加载完成才开始执行；
>
> jQuery的入口函数只会等待文档树加载完成就开始执行，并不会等待图片，文件的加载。

## DOM对象和jQuery对象

### DOM:	原生js获取过来的对象就是DOM对象

### jQuery对象: 	用jQuery方式获取过来的对象是jQuery对象。

> 本质：通过==$== 把DOM元素进行了包装
>
> jQuery对象只能使用jQuery方法；DOM对象则使用原生的JavaScript属性和方法

### 两者转换

1. DOM对象转换成jQuery对象：==$(DOM对象)==

	​	$(‘div’)

2. jQuery对象转换成DOM对象：

   ​	$(‘div’)[index]	==index索引号==

   ​	$(‘div’).get(index)

## jQuery APIs

### jQuery选择器

#### 基础选择器：

​	$(“选择器”) 	// 里面选择器直接写CSS选择器即可，但是要加引号 

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220112161826426.png" alt="image-20220112161826426" style="zoom: 80%;" />

#### 层级选择器

![image-20220112162044859](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220112162044859.png)

#### 隐式迭代

> 遍历内部DOM元素（伪数组形式存储）的过程就叫做隐式迭代

#### 过滤选择器

![image-20220112163052466](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220112163052466.png)

#### 筛选选择器

![image-20220112162731391](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220112162731391.png)

==重点：==parent()	children()	find()	sibling()	eq()

#### jQuery 排他思想

> 当前元素设置样式，其余的兄弟元素清除样式

#### 链式编程写法

```javascript
$(this).css("color", "red");
$(this).siblings().css("color", "");
```

上述代码可写成

```javascript
$(this).css("color","red").siblings().css("color","");
```

### jQuery 样式操作

#### 设置类样式方法

> 作用等同于以前的classList，可以操作类样式，注意操作里面的参数不要加点

例如:	
			$("div").addClass("current");

- 添加类addClass
- 删除类removeClass
- 切换类toggleClass

#### 类操作与className区别

原生JS中className会覆盖元素原先里面的类名

jQuery里面类操作只是对指定类进行操作，不影响原先的类名

### jQuery效果

- 显示隐藏

  show()	hide()	toggle()

- 滑动

  slideDown()	slideUp()	slideToggle()

- 淡入淡出

  fadIn()	fadeOut()	fadeToggle()	fadeTo()

- 自定义动画

  animate()

    /*
    jQuery中有个动画队列的机制。
    当我们对一个对象添加多次动画效果时后添加的动作就会被放入这个动画队列中，  
    等前面的动画完成后再开始执行。
    可是用户的操作往往都比动画快，  
    如果用户对一个对象频繁操作时不处理动画队列就会造成队列堆积，
    影响到效果。
    */

```js
/*1.停止当前动画  如果动画队列当中还有动画立即执行*/
//$('div').stop();
/*2.和stop()效果一致  说明这是默认设置*/
//$('div').stop(false,false);
/*3.停止当前动画  清除动画队列*/
//$('div').stop(true,false);
/*4.停止当前动画并且到结束位置  清除了动画队列*/
//$('div').stop(true,true);
/*5.停止当前动画并且到结束位置  如果动画队列当中还有动画立即执行*/
$('div').stop(false,true);
```

### jQuery 属性操作

#### 设置或获取元素固有属性值 prop()

```js
prop(“属性”)

prop(“属性”,“属性值”)
```

#### 设置或获取元素自定义属性值 attr()

```javascript
attr(“属性”)	//类似原生getAttribute()

attr(“属性”,”属性值”)	//类似原生setAttribute()
```

#### 数据缓存 data()

在指定元素上存取数据，并不会修改DOM元素结构，一旦页面刷新。之前存放的数据都将被移除

```js
date(“name”,“value”)	// 向被选元素附加数据

data(“name”)	// 向被选元素获取数据
```

同时，还可以读取H5自定义属性 data-index,得到的是数字型

### jQuery内容文本值

#### 普通元素内容html() （相当于原生innerHTML）

```js
html()	//获取元素内容
html("内容")	//	设置元素内容
```

#### 普通元素文本内容text()	（相当于原生innerText）

```js
text()	//获取元素文本内容
text("文本内容")	//设置元素文本内容
```

### jQuery元素操作

#### 遍历元素

> jQuery的隐式迭代会对所有的DOM对象设置相同的值，但是如果我们需要给每一个对象设置不同的值的时候，就需要自己进行迭代了。

作用：遍历jQuery对象集合，为每个匹配的元素执行一个函数

```js
// 参数一表示当前元素在所有匹配元素中的索引号
// 参数二表示当前元素（DOM对象）
$(selector).each(function(index,element){});
```

#### 创建元素

    /*创建节点*/
    var li = $('<li>后创的li</li>');

#### 添加元素

- 内部添加

      /*创建节点*/
      var $a = $('<a href="http://www.baidu.com" target="_blank">百度1</a>');

  append	最后面

  prepend	最前面

- 外部添加

  element.after("内容")	//目标元素后面
  element.before("内容")	//目标元素前面

> 内部添加元素，生成后是父子关系
>
> 外部添加元素，生成后是兄弟关系

#### 删除元素

	element.remove()	// 删除匹配的元素本身
	element.empty()		// 删除匹配的元素集合中的所有子节点
	element.htm("")		// 清空匹配的元素内容

### jQuery尺寸，位置操作

#### width方法与height方法

> 设置或者获取高度

    //带参数表示设置高度
    $('img').height(200);
    //不带参数获取高度
    $('img').height();

获取网页的可视区宽高

    //获取可视区宽度
    $(window).width();
    //获取可视区高度
    $(window).height();

#### offset方法与position方法

> offset方法获取元素距离文档document的位置（偏移）
>
> position方法获取的是元素距离有定位的父元素的位置。

    //获取元素距离document的位置,返回值为对象：{left:100, top:100}
    $(selector).offset();
    //获取相对于其最近的有定位的父元素的位置。
    $(selector).position();

#### scrollTop与scrollLeft

> 设置或者获取垂直滚动条的位置

    //获取页面被卷曲的高度
    $(window).scrollTop();
    //获取页面被卷曲的宽度
    $(window).scrollLeft();

### jQuery事件

> 简单事件绑定>>bind事件绑定>>delegate事件绑定>>on事件绑定(推荐)

#### on注册事件(重点)

- on注册简单事件

    // 表示给$(selector)绑定事件，并且由自己触发，不支持动态绑定。
    $(selector).on( "click", function() {});

- on注册委托事件

    // 表示给$(selector)绑定代理事件，当必须是它的内部元素span才能触发这个事件，支持动态绑定
    $(selector).on( "click",'span', function() {});

==on注册事件的语法：==

    // 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
    // 第二个参数：selector, 执行事件的后代元素（可选），如果没有后代元素，那么事件将有自己执行。
    // 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用（不常使用）
    // 第四个参数：handler，事件处理函数
    $(selector).on(events,[selector],[data],handler);

#### off事件解绑

    // 解绑匹配元素的所有事件
    $(selector).off();
    // 解绑匹配元素的所有click事件
    $(selector).off("click");

#### 触发事件

    $(selector).click(); //触发 click事件
    $(selector).trigger("click");

### jQuery对象拷贝

- $.extend()

语法：$.extend([deep],target,object,[objectN])

- deep：true为深拷贝，默认为false 浅拷贝
- target：要拷贝的对象
- object1：待拷贝到第一个对象的对象

### 多库共存

> jQuery使用作 为 标 示 符 ， 但 是 如 果 与 其 他 框 架 中 的 作为标示符，但是如果与其他框架中的作为标示符，但是如果与其他框架中的冲突时，jQuery可以释放$符的控制权.

​    var c =$$.noConflict();//释放$的控制权,并且把$的能力给了c

### jQuery 插件

- 瀑布流
- 懒加载
- 全屏滚动 