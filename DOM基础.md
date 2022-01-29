# JavaScript  DOM   笔记

[toc] 

## DOM 

### 重点核心

> 文本对象模型(Document Object Model，简称DOM)，是W3C组织推荐的==处理可拓展标记语言(HTML或者XML)的标准编程接口==

### 获取元素

#### 根据ID获取
==getElementById()==

==console.dir()==	打印我们返回的元素对象，更好查看里面的属性和方法

#### 根据标签名获取

==getElementsByTagName()==	返回带有指定标签名的对象的集合

#### 通过HTML5新增的方法获取

==getElementsByClassName()==		根据类名返回元素对象集合

==querySelector()==	根据指定选择器返回第一个元素对象 

==querySelectorAll()==	返回指定选择器的所有元素对象

#### 获取特殊元素

+ ##### 获取body元素
  var bodyEle = ==document.body==;
  console.log(bodyEle);
  console.dir(bodyEle);
+ ##### 获取html元素
+ var htmlEle = ==document.documentElement==;
+ console.log(htmlEle);

### 实践基础

#### 鼠标事件

![image-20220103172530785](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220103172530785.png)

### 操作元素

#### 改变元素内容

==element.innerText==	//从起始位置到终止位置的内容，但它去除(不识别)html标签，同时空格和会换行也会去掉

==element.innerHTML==	//起始位置到终止位置的全部内容，包括(识别)html标签，同时保留空格和换行

+ src,href

+ id,alt,t

#### 表单元素的属性

>type,value,checked,selected,disabled 

#### 样式属性操作

1. element.style			//行内样式操作
2. element.className		//类名样式操作

#### 获取属性值

- element.属性 								//内置属性

- element.getAttribute('属性')  //自定义属性

#### 设置属性值

- element.属性 = ‘值’

- element.setAttribute(‘属性’，‘值’);

#### 移除属性值

- element.removeAttribute(‘属性’);

#### H5自定义属性

- H5规定自定义属性data-开头做为属性名并且赋值
  - /<div data-index = “1"></div>/
- 或者使用JS设置
  - element.setAttribute(‘data-index’,2)
- 获取H5自定义属性
  - element.getAttribute(‘data-index’);
  - (H5)  element.dataset.index  或者  element.dataset[‘index’]

### 节点操作

#### 父节点 parentNode

>得到的是离元素最近的父级节点

#### 子节点 

+ ##### childNodes

> 包含所有的子节点，元素节点，文本节点

- ##### children (伪数组)

> 获取所有子元素节点

第一个子元素节点	    firstElementChild 或 .children[0]

最后一个子元素节点   lastElementChild  或 .children[ol.children.length - 1]

#### 兄弟节点

- ##### nextSibling

  下一个兄弟节点 ，包含元素节点或者文本节点

- nextElementSibing

  下一个兄弟元素节点

- previousSibing

  上一个兄弟节点 ，包含元素节点或者文本节点

- previousElementSibing

  上一个兄弟元素节点

#### 创建节点

document.createElement('tagName')

#### 添加节点

- node.appendChild(child)

>将一个节点添加到指定父节点的子节点列表末尾，类似于css中的after伪元素

- node.insertBefore(child,指定元素)

  在前面添加

#### 删除节点

node.removeChild(child)

#### 复制节点

node.cloneNode()

> 返回调用该方法的节点的一个副本，称为克隆节点或拷贝节点

如果括号参数为==空或false==，则是==浅拷贝==，即只克隆复制节点本身，不克隆里面子节点

括号里为==true==，==深拷贝==，复制标签和里面的内容

#### 动态创建元素

- document.write()  (了解)	重绘
- element.innerHTML
- document.createElement()

> innerHTML效率要比createElement高
