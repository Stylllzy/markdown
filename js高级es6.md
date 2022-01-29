[toc]



# ES6

## 新增

### `let`

声明的变量只在所处于的`块级`有效（var声明的变量不具备块级作用域特性）

> for循环的计数器，适合使用`let`

- **let**不存在变量提升，之前的**var**会发生变量提升，即变量可以在声明之前使用，值为undefined

- let不允许重复声明
- 暂时性死区

### `const`

声明一个只读的常量，一旦声明，常量的值就不能改变

- 声明时必须赋值

- 作用域与let相同，只在声明所在的块级作用域内有效

> ES6有六种声明变量的方法
>
> var，function，let，const，import，class

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220117171422237.png" alt="image-20220117171422237" style="zoom: 67%;" />

### `Symbol`

数据类型，作用是声明一个独一无二的值

### `Set`

类似数组，成员唯一

语法：

	set.add/delete/has(成员) & set.clear()

场景：数组去重

### `Map`

类似对象，但是键不限于字符串（传统方式对象的键一定是字符串）

语法：

	map.set(键,值) & map.get/delete/has (键) & map.clear()

### `循环`

起初：for

ES5：forEach（仅支持数组），for…in（推荐遍历对象）

ES6：for…of（可以遍历数组，字符串，映射，集合等数据结构）

	var arr = ['a','b','c']
	for (var val of arr) {
		console.log('值：' + val)
	}

### `模块（module）`

通过 import 和 export 实现导入导出功能 （必须走HTTP协议访问）

### `Promise`

异步编程的解决方案，比传统的解决方案——回调函数和事件——更合理强大

语法上，Promise 是一个对象

```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

### `Generator 函数`

Generator 函数是一个状态机，封装了多个内部状态。

执行 Generator 函数会返回一个遍历器对象，

特征：

- `function`关键字与函数名之间有一个星号；
- 函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）。`yield`表达式就是暂停标志。

> 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是上一章介绍的遍历器对象（Iterator Object）

调用遍历器对象的`next`方法，使得指针移向下一个状态

### `ES7 Async Await`

async：Generator 函数的语法糖

- `async`函数就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`，仅此而已

- `async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。
- `async`函数的`await`命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。
- `async`函数的返回值是 Promise 对象，可以用`then`方法指定下一步的操作。
- `async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。

### 解构赋值

ES6允许从数组中提取值，按照对应位置对变量赋值。对象也可以实现解构

### 箭头函数

```js
() => {}
```

- 若函数体中只有一句代码，并且代码的执行结果就是函数的返回值，函数体大括号可以省略

  ```js
  const sum = (n1,n2) => n1 + n2;
  ```

- 若形参只有一个，可省略小括号

	```js
	const fn = v => v;
	```

使用场景:

当一个函数作为参数传入另一个函数时,例如

```
	setTimeout(() => {
	
	},)
```

ES6 引入 rest函数获取多余参数，替换 arguments对象

箭头函数中的this，指向的是函数定义位置的上下文this

### 内置对象拓展

见ES6文档

#### 字符串拓展

`repeat`函数：平铺指定次数，如：‘x’.repeat(3)

`模板字符串`:

### 数据结构

- Set

类似于数组，但是成员的值都是唯一的，没有重复的值

本身是一个构造函数

```js
	const s = new Set();
```



## 面向对象编程

### class类

> JS 中的成员只有公有的，私有的两种形式，不能之间通过public，private关键字修饰

- this 声明属性：共有的
- var 声明属性： 私有的

### 例子

比如我想要用代码描述一个场景，有一只叫做xiaoA的猫，吃了一个苹果，又吃了一条鱼，然后有一只叫做xiaoB的猫，吃了一根香蕉

	// 面向过程
	function xiaoAEatApple() {}
	function xiaoAEatFish() {}
	function xiaoBEatBanana() {}
	xiaoAEatApple();
	xiaoAEatFish();
	xiaoBEatBanana();
	
	// 面向对象
	function Cat(name) {
		this.name = name
	}
	Cat.prototype.eat = function(something) {}
	let xiaoA = new Cat('xiaoA')
	let xiaoB = new Cat('xiaoB')
	xiaoA.eat('apple')
	xiaoA.eat('fish')
	xiaoB.eat('banana')

### 特点

- 面向对象注重于抽象事务，而面向过程注重于叙述事务
- 面向对象逻辑清晰有条理，而面向过程比较方面
- JS通过函数和原型，模拟了传统面向对象编程中类的概念实现了面向对象的编程模式
- 面向对象的编程思想，主要为了实现3件事，封装，继承和多态

> 优点

易维护，易复用，易拓展，由于封装，继承，多态，可以设计出低耦合的系统，使系统更加灵活，易于维护。

> 缺点

性能低于面向过程

## ES6中的类和对象

> 类必须使用new实例化对象

类里面有个constructor 函数，接受传递过来的参数，同时返回实例对象

## 类的继承

extends

> 在声明函数的时候，会自动创建一个prototype属性，我们管他叫做原型, 一般用来存放实例公用的方法

super关键字

> 访问和调用对象父类上的函数，可以调用父类的构造函数，也可以调用父类的普通函数

==super 必须在子类this 之前调用==

> 注意

- 在ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象
- 类里面的共有的属性和方法一定要加 this 使用



以前：createElement创建元素，innerHTML赋值，再appendChild追加到父元素里

==现在：==利用==insertAdjacentHTML()==直接把字符串格式元素添加到父元素里



# ES5

## 构造函数和原型 

- 实例成员

  构造函数内部通过 this 添加的成员	uname	age	sing

  只能通过实例化的对象来访问

- 静态成员

  在构造函数本身上添加的成员

  只能通过构造函数来访问

> 构造函数存在==浪费内存==的问题

### 构造函数原型 prototype

> 构造函数通过原型分配的函数是所有对象所共享的

**prototype:**
是子类继承的父类的属性，也就是当调用子类构造函数时，总的来说，这里只能是继承一个具体的对象，不能是一个类（ES6后会有所改变）

### 对象原型 --proto--	

对象都会有一个属性--proto-- 指向构造函数的prototype原型对象

- -- proto-- 对象原型和原型对象prototype是等价的

 ### 构造函数、实例、原型对象三者之间的关系

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220116202057869.png" alt="image-20220116202057869" style="zoom: 50%;" />

### 原型链

![image-20220116202427818](C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220116202427818.png)

## 继承

ES6之前没用extends，可以通过构造函数和原型对象模拟继承，被称为组合继承

- call()

  ```
  function.call(thisArg, arg1, arg2, ...)
  ```

  修改函数运行时的this 指向

  **thisArg**	当前调用函数this的指向对象

  **arg1, arg2, ...**	传递的其他参数

- 父构造函数继承属性
- 原型对象继承方法

## ES5新增方法

### 数组

- ==遍历==

```
	arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
```

​		参数		

```
callback
```

为数组中每个元素执行的函数，该函数接收一至三个参数：

- `currentValue`

  数组中正在处理的当前元素。

- `index` 可选

  数组中正在处理的当前元素的索引。

- `array` 可选

  `forEach()` 方法正在操作的数组。

- `thisArg` 可选

​        可选参数。当执行回调函数 `callback` 时，用作 `this` 的值。

- ==筛选==

  ```
  var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
  ```

  参数

  ```
  callback
  ```

  用来测试数组的每个元素的函数。返回 `true` 表示该元素通过测试，保留该元素，`false` 则不保留。它接受以下三个参数：

  - `element`

    数组中当前正在处理的元素。

  - `index`可选

    正在处理的元素在数组中的索引。

  - `array`可选

    调用了 `filter` 的数组本身。

  `thisArg`可选

  执行 `callback` 时，用于 `this` 的值。

### 字符串

- 从一个字符串的两端删除空白字符

  str.trim()

### 对象方法

- 定义对象中新属性或修改原有的属性

  ```
  Object.defineProperty(obj, prop, descriptor)
  ```

  参数

  ```
  obj
  ```

  要定义属性的对象。

  ```
  prop
  ```

  要定义或修改的属性的名称或 [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 。

  ```
  descriptor
  ```

  要定义或修改的属性描述符。

  

# 函数进阶

## 函数的定义和调用

```js
var f = new Function(‘参数1’,’参数2’,’函数体’);
```

> 函数也属于对象

## this

> 改变函数内部this指向，常用的有bind(),call(),apply() 

- apply

> **注意：**call()方法的作用和 apply() 方法类似，区别就是`call()`方法接受的是**参数列表**，而`apply()`方法接受的是**一个参数数组**。

```js
func.apply(thisArg, [argsArray])
- thisArg
必选的。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。
- argsArray
可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。
```

- bind

> `bind()` 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

```
function.bind(thisArg[, arg1[, arg2[, ...]]])
- thisArg
调用绑定函数时作为 this 参数传递给目标函数的值。 如果使用new运算符构造绑定函数，则忽略该值。当使用 bind 在 setTimeout 中创建一个函数（作为回调提供）时，作为 thisArg 传递的任何原始值都将转换为 object。如果 bind 函数的参数列表为空，或者thisArg是null或undefined，执行作用域的 this 将被视为新函数的 thisArg。
- arg1, arg2, ...
当目标函数被调用时，被预置入绑定函数的参数列表中的参数。
返回值
返回一个 原函数的拷贝，并拥有指定的 this 值和初始参数。
```

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20220117100136098.png" alt="image-20220117100136098" style="zoom:67%;" />



## 严格模式

- 脚本

  - ‘use	strict’

- 函数

  严格模式下全局作用域中函数中的this是	undefined

## 高阶函数

对其他函数进行操作的函数，它接收函数作为参数或将函数作为返回值输出

## 闭包

指有权访问另一个函数作用域中局部变量的`函数`

被访问的变量所在函数就是闭包函数

- 作用：延伸了变量的作用范围

## 递归 

- 浅拷贝

浅拷贝只拷贝一层，更深层次对象级别的只拷贝引用（地址）

 `ES6新增方法可以浅拷贝`

```
Object.assign(target, ...sources)
参数
	target	目标对象。
	sources	源对象。
返回值
	目标对象。
```



- 深拷贝

拷贝多层，每一级别的数据都会拷贝



# 正则表达式

> 用于匹配字符串中字符组合的模式，在JS中，正则表达式也是对象

- 创建

1. 利用RegExp对象来创建正则表达式
2. 利用字面量创建正则表达式

- 测试

test() 正则对象方法，用于检测字符串是否符合该规则，该对象会返回true或false，其参数是测试字符串。

```
regexObj.test(str)
参数
	str	用来与正则表达式匹配的字符串
返回值
	如果正则表达式与指定的字符串匹配 ，返回true；否则false。
```

> 用于表单验证

