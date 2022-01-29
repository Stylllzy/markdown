# Javascript基础笔记

[toc]



## 类型

***数字型：Number	|	字符串型：String 	|	布尔型：Boolean***

***空：null							未定义：undefine***

### 字符串类型

- #### 把数字型转换为字符串型	变量 **.toString**

  > var num = 10;
  > var str = num.toString();​	

  > console.log(str);

  + **（重点）**利用'+'拼接 **（隐式转换）** 

  > console.log(num + '');

- #### 转换为数字型

  >1.parseInt(变量)		字符串转数字	得到整数
  >
  >2.parseFloat(变量)	字符串转数字	得到小数，浮点数

  + **（重点）**利用算术运算 - * / （**隐式转换**）

    >console.log('12' - 0);

- #### 转换为布尔型

  >代表***空、否定的值***会被转换为false，如'',0,NaN,null,undefined
  >
  >其余转换为true

  > console.log(Boolean('')); //false

## 运算符

### 分类

- 算数运算符

  >TIPS：不能直接比较两个浮点数是否相等

- 递增和递减运算符

- 比较运算符

- 逻辑运算符

- 赋值运算符

## 分支结构

1. ### if-else-if

2. ### 三元运算符

3. ### switch

## 循环结构

1. ### for循环

2. ### while循环

3. ### do...while循环

> continue;
>
> break;

## 数组

### 创建

- #### 利用new创建数组

  var  数组名 = new Array();

  var arr  = new Array(); //创建一个新的空数组

- #### 利用数组字面量创建数组

  var 数组名 = []; //一个空的数组
  var 数组名 = ['a',,'b','c'];

> 数组中可以存放==任意类型==的数据，字符串，数字，布尔值

### 索引，获取数组元素

### 遍历数组，获取长度  

数组名.length

## 函数

(函数可以调用另外一个函数)

>### 函数形参和实参个数不匹配问题

实参 = 形参	——	输出正确结果

实参 > 形参	——	只取到形参个数

实参 < 形参	——	多的形参定义为 undefined，结果为NaN 

### arguments	

(只有函数才有argums对象)

> 伪数组

- 具有length属性
- 按索引方式存储数据
- 不具有数组的push，pop等方法

## 作用域

>js的作用域(es6)之前：全局作用域	局部作用域

(es6新增)==块级作用域==	{}	if{}	for{}

## 对象

- ### 利用对象字面量创建对象	{}

  var obj = {
  	name:'zy',
  	age:20,
  	sex:'男',
  	say:function(){
  		console.log('**');
  	}
  }

>调用对象属性：==对象名.属性名==	或者	==对象名[‘属性名’]==
>
>调用对象方法：==对象名.方法名()==

- ### 利用 new Object 创建对象

  var obj = new Object();
  obj.name = 'zy';
  obj.age = 20;
  obj.sex = '男';
  obj.say = function(){
  	console.log('**');
  }

### 构造函数创建对象

>对象的实例化

function 构造函数名() {
	this.属性 = 值;
    this.方法 = function() {}
}

new 构造函数名();

### 遍历对象
for(var 变量 in 对象){
	console.log(k); // k 变量 输出得到 属性名
	console.log(obj[k]); // obj[k] 得到的是 属性值
}

### 内置对象

(查文档为主)

- #### Math对象

- #### 日期对象

- #### 数组对象

  > 检测是否为数组
  >
  > 1. instanceof	运算符	可以用来检测是否为数组
  > 2. ==(H5新增)== Array.isArray(参数)

  ##### 数组排序

  ##### 数组去重

  for (var i = 0;i < arr.length;i++){
  	if(newArr.indexOf(arr[i]) === -1){ //新数组.indexOf(数组元素) 返回-1说明新数组没有该元素
  		newArr.push(arr[i]);
  	}
  }

  ##### 数组转换为字符串

  toString()

  join(‘分隔符’)

  ##### 添加删除数组元素

  + push		 在数组后追加元素

  + unshift    在数组开头添加元素
  + pop         删除数组最后一个元素
  + shift         删除数组第一个元素

- #### 字符串对象

  ##### 根据字符返回位置
  
  ##### 求某个字符出现的位置及次数
  
  ##### 根据位置返回字符
  
  + charAt(index)
  + chartCodeAt(index)    返回相应索引号的字符ASCII值    目的：判断用户按下了哪个键
  + str[index]    ==(H5新增)==
  
  ##### 字符串操作方法
  
  + concat		拼接
  + substr        从start开始，length取的个数
  + slice           从start开始，截取到end，end取不到
  + substring   同slice，不接受负值
  
  ##### 替换字符串
  
  replace(‘被替换字符’，‘替换为的字符’)，只替换第一个字符
  
  ##### 字符串转换为数组
  
  split(‘分隔符’)

