# 类型别名

`用来给一个类型起一个新名字`

**type** 创建类型别名，作用是用于联合类型

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```



# 字符串字面量类型

`用来约束取值只能是某几个字符串中的一个`

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'
```

上例中，我们使用 `type` 定了一个字符串字面量类型 `EventNames`，它只能取三种字符串中的一种。

注意，**类型别名与字符串字面量类型都是使用 `type` 进行定义。**



# 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

```typescript
let tom: [string, number] = ['Tom', 25];

// 赋值
tom[0] = 'Tom';
tom[1] = 25;

// 当直接对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项
let tom: [string, number];
tom = ['Tom', 25];
```



# 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

## 常数项和计算所得项

```typescript
enum Color {Red, Green, Blue = "blue".length};
// "blue".length 就是一个计算所得项。
```



# 类

## public private 和 protected

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

## 类的类型

给类加上 TypeScript 的类型很简单，与接口类似：

```typescript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}

let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

