# DOM高级

## 注册事件(绑定事件)

### 传统注册方式

特点：注册事件的==唯一性==

同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数会覆盖前面注册的处理函数

### 方法监听注册方式 （推荐）

> ```html
> target.addEventListener(type, listener, useCapture);
> ```

特点：同一个元素同一个事件可以注册多个监听器

按注册顺序依次执行

## 删除事件(解绑事件)

### 传统方式

> eventTarge.onclick = null

### 方法监听注册方式

```html
target.removeEventListener(type, listener[, useCapture]);
```

## DOM事件流

事件发生时，元素节点之间按照特定顺序传播的过程

```html
target.addEventListener(type, listener, useCapture);
```

第三个参数为true，事件捕获阶段；为false，事件==冒泡阶段==(使用更多)

## 事件对象

> 写在侦听函数的 小括号里面 ，当作==形参==来看

## 阻止事件冒泡

利用事件对象里的 event.stopPropagation() 方法

## 事件委托

> 原理：不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每个子节点

