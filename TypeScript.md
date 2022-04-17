# TypeScript

## 原始数据类型

- 布尔值
- 数值
- 字符串
- null
- undefined
- Symbol (ES6)
- BigInt (ES10)

### 布尔

```typescript
let isDone: boolean = false;

构造函数 Boolean 创造的对象不是布尔值，直接调用 Boolean 返回一个 boolean 类型
let createdByNewBoolean: boolean = new Boolean(1);
```

### 字符串

其中 `` 用来定义 ES6 中的模板字符串，${expr} 用来在模板字符串中嵌入表达式

### 空值

引入 void 

```typescript
function alertName(): void {
    alert('My name is Tom');
}
```

### Null , Undefined

与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量



## 任意值

任意值（Any）用来表示允许赋值为任意类型

变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型

## 联合类型

联合类型使用 `|` 分隔每个类型。

这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

访问联合类型的属性或方法：只能访问共有属性



## 接口

```typescript
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `Person`。这样，我们就约束了 `tom` 的形状必须和接口 `Person` 一致

**赋值的时候，变量的形状必须和接口的形状保持一致**

> 可选属性

可选属性的含义是该属性可以不存在

```typescript
interface Person {
    name: string;
    age?: number;	// 加个？号
}

let tom: Person = {
    name: 'Tom'
};
```

> 任意属性

在上面基础上加入  [propName: string]: string;

也可以 [propName: string]: string | number;

使用 `[propName: string]` 定义了任意属性取 `string` 类型的值。

需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**

> 只读属性

在上面基础上加入 readonly id: number;

**只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**



## 数组

**[类型 + 方括号] 表示法**

### 数组泛型

Array<elemType>

### 接口表示数组

```typescript
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

用来表示类数组，arguments

### any的应用

表示数组中允许出现任意类型

```typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```



## 函数

输入输出类型都要考虑

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```

### 函数表达式

```typescript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

> 注意：TypeScript中 => 与 ES6 中 => 不一样

在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。







