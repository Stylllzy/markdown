# Java基本语法

## 语言特点

- 面向对象性
  - 两要素：类，对象
  - 三特征：封装，继承，多态
- 健壮性
  - 去除了C语言中的指针
  - 自动的垃圾回收机制
- 跨平台性
  - 得益于JVM（Java编译器）

## JDK，JRE，JVM

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/52e6db73c2b54f9ebde73963c4e32575~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom: 67%;" />

`程序编译过程`

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ed446a37566460688988ad140d818ef~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom:67%;" />

`Tips`

一个Java源文件中可以声明多个class。但最多只能有一个类声明为public，并且声明为public的类必须与源文件名相同。

`命名风格`

- 包名：全小写，xxxyyyzzz

- 类名，接口名：大驼峰，XxxYyyZzz

- 变量名，方法名：小驼峰，xxxYyyZzz

- 常量名：全大写，下划线连接，XXX_YYY_ZZZ

## 关键字

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9cad3f2fafca4578957dde92072c1aae~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="关键字" style="zoom:67%;" />

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c0a5f6c66a34bf8a063867617aba046~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom:67%;" />

`数据类型`

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91642222d45b4d14af408497cb420074~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom: 80%;" />

## 进制转换

二进制形式存储，所有数值，不论正负，底层都以补码的方式存储

`原码，反码，补码`

- 正数：三码合一
- 负数：
  - 负数的原码：直接将一个数值换成二进制数。最高位是符号位。
  - 负数的反码：是对原码按位取反，只是最高位（符号位）确定为1。
  - 负数的补码：其反码加1。

`方法`

- 除二取余的逆
- 乘二取整



# 数组

`一维数组`

```java
int num;//声明
num = 10;//初始化
int id = 1001;//声明 + 初始化		
int[] ids;//声明
//1.1 静态初始化:数组的初始化和数组元素的赋值操作同时进行
ids = new int[]{1001,1002,1003,1004};
//1.2动态初始化:数组的初始化和数组元素的赋值操作分开进行
String[] names = new String[5];
int[] arr4 = {1,2,3,4,5};//类型推断
```

`二维数组`

```java
int[] arr = new int[]{1,2,3};//一维数组
//静态初始化
int[][] arr1 = new int[][]{{1,2,3},{4,5},{6,7,8}};
//动态初始化1
String[][] arr2 = new String[3][2];
//动态初始化2
String[][] arr3 = new String[3][];
//也是正确的写法：
int[] arr4[] = new int[][]{{1,2,3},{4,5,9,10},{6,7,8}};
int[] arr5[] = {{1,2,3},{4,5},{6,7,8}};//类型推断
```

`工具类的使用`

```java
int arr1[] = new int[] { 21, 43, 542, 432, 4, 2, 5, 1 };
int arr2[] = new int[] { 32, 43, 4, 1, 4, 76, 54, 68, 4 };

// 1.Arrays.equals(arr1, arr2):判断数组是否相等
System.out.println(Arrays.equals(arr1, arr2));
// 2.Arrays.toString(arr1):输出数组信息
System.out.println(Arrays.toString(arr1));

// 3.Arrays.fill(arr1, 2):将指定值填充到数组中
Arrays.fill(arr1, 2);
System.out.println(Arrays.toString(arr1));
// 4.Arrays.sort(arr2):对数组进行排序
Arrays.sort(arr2);
System.out.println(Arrays.toString(arr2));
// 5.Arrays.binarySearch(arr2, 1):堆排序好的数组用二分法检索指定值
int index = Arrays.binarySearch(arr2, 1);
System.out.println(index);
```

`常见异常`

- 数组角标越界异常(==ArrayIndexOutOfBoundsException==)

```java
int[] arr = new int[]{1,2,3,4,5};

for(int i = 0;i <= arr.length;i++){
    System.out.println(arr[i]);
}

System.out.println(arr[-2]);

System.out.println("hello");
```

- 空指针异常(==NullPointerException==)

```java
//情况一：
int[] arr1 = new int[]{1,2,3};
arr1 = null;
System.out.println(arr1[0]);

//情况二：
int[][] arr2 = new int[4][];
System.out.println(arr2[0][0]);

//情况：
String[] arr3 = new String[]{"AA","BB","CC"};
arr3[0] = null;
System.out.println(arr3[0].toString());
```



# 异常处理

分为两类：`编译时异常`和`运行时异常`

- ==Error==： Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。比如： `StackOverflowError` 和OOM。一般不编写针对性的代码进行处理。
- ==Exception==：其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。如：
  - 空指针访问
  - 试图读取不存在的文件
  - 网络连接中断
  - 数组角标越界

```java
异常的体系结构
 * java.lang.Throwable
 * 		|-----java.lang.Error:一般不编写针对性的代码进行处理。
 * 		|-----java.lang.Exception:可以进行异常的处理
 * 			|------编译时异常(checked)不会生成字节码文件
 * 					|-----IOException
 * 						|-----FileNotFoundException
 * 					|-----ClassNotFoundException
 * 			|------运行时异常(unchecked,RuntimeException)
 * 					|-----NullPointerException//空指针异常
 * 					|-----ArrayIndexOutOfBoundsException//数组角标越界
 * 					|-----ClassCastException//类型转化异常
 * 					|-----NumberFormatException//编码格式异常
 * 					|-----InputMismatchException//输入不匹配
 * 					|-----ArithmeticException//算术异常
```

## 异常处理的抓抛模型

- try-catch-finally

```java
try{
    //可能出现异常的代码

}catch(异常类型1 变量名1){
    //处理异常的方式1
}catch(异常类型2 变量名2){
    //处理异常的方式2
}catch(异常类型3 变量名3){
    //处理异常的方式3
}
....
    finally{
        //一定会执行的代码
    }
```

> finally 是可选的；
>
> 该结构可嵌套；
>
> 常用异常对象处理方式：① `String getMessage()` ② `printStackTrace()`

- throws + 异常类型

写在方法声明出，一旦方法体执行时出现异常，会在异常代码处生成异常类对象，该对象满足`throws`后异常类型时，就会被抛出，异常代码后续代码不再执行。

## 对比

`try-catch-finally` 真正的将异常给处理掉了。 `throws` 的方式只是将异常抛给了方法的调用者。并没真正将异常处理掉。

`经典面试题`

`throw` 和 `throws`区别： `throw` 表示抛出一个异常类的对象，生成异常对象的过程。声明在方法体内。 `throws` 属于异常处理的一种方式，声明在方法的声明处。

```java
class Student{

    private int id;

    public void regist(int id) throws Exception {
        if(id > 0){
            this.id = id;
        }else{
            //手动抛出异常对象
            //			throw new RuntimeException("您输入的数据非法！");
            //			throw new Exception("您输入的数据非法！");
            throw new MyException("不能输入负数");

        }		
    }

    @Override
    public String toString() {
        return "Student [id=" + id + "]";
    }

}
```





# 面向对象

三特征：封装，继承，多态

## 封装

程序追求“高内聚，低耦合”

- 高内聚：类的内部数据操作细节自己完成，不运行外部干涉
- 低耦合：仅对外暴露少量方法

`具体体现`

- 将类属性 私有化 ==private==，同时，提供公共的 ==public== 方法来获取 ==get== 和设置 ==set== 此属性的值

```java
private double radius;
public void setRadius(double radius){
	this.radius = radius;
}
public double getRadius(){
	return radius;
}
```

- 不对外暴露的私有方法
- 单例模式(将构造器私有化)
- if 不希望类在包外被调用，将类设置为缺省的，即修饰符为空

`权限修饰符`

- 权限从小到大顺序为：private < 缺省 < protected < public
- 范围

|  修饰符   | 类内部 | 同一个包 | 不同包的子类 | 同一个工程 |
| :-------: | :----: | :------: | :----------: | :--------: |
|  private  |  yes   |          |              |            |
|   缺省    |  yes   |   yes    |              |            |
| protected |  yes   |   yes    |     yes      |            |
|  public   |  yes   |   yes    |     yes      |    yes     |

**权限修饰符可用来修饰的结构说明**：

- 4种权限都可以用来修饰**类的内部结构**：属性、方法、构造器、内部类
- **修饰类**，只能使用：缺省、public 

## 继承

好处：提高代码复用，功能扩展，为多态提供前提

```java
class A extends B{}
 *    A:子类、派生类、subclass
 *    B:父类、超类、基类、superclass
```

> Java 单继承性：一个类只能有一个父类

`Object类`

Java中所有类的父类。

- 如果我们没显式的声明一个类的父类的话，未使用extends关键字指明其父类，则此类继承于java.lang.Object类

- 所有的java类（除java.lang.Object类之外都直接或间接的继承于java.lang.Object类

- 所有的java类具有java.lang.Object类声明的功能。

- java.lang.Object类中定义的一些方法

  - | 方法名                             | 类型     | 描述           |
    | ---------------------------------- | -------- | -------------- |
    | public Object()                    | 构造方法 | 构造器         |
    | public boolean equals( Object obj) | 普通方法 | 对象比较       |
    | public int hashCode()              | 普通方法 | 获取哈希码     |
    | public String toString()           | 普通方法 | 对象打印时调用 |

## 多态

`例子`：数据库连接，定义好数据库的连接以及连接步骤，但我们不知道用户会用什么数据库，我们需要针对不同的数据库写不同的连接方法；而有了多态，我们只需定义好数据库的类以及连接方法，让所有的数据库继承该数据库类，你用什么数据库，你就重写该数据库的连接方法。

`使用前提`：

- 类的继承
- 方法重写

`注意&总结`：

- 只适用于方法，不适用于属性
- 提高代码复用性，常称作 接口重用
- 成员方法
  - 编译时：查看引用变量所声明的类中是否有所调用的方法
  - 运行时：调用实际new的对象所属的类中的重写方法

## 向上转型与向下转型

==父类引用指向子类对象，反之不行==

`例子`：

- 有两个类，Father是父类，Son类继承自Father，
  - Father f1 = new Son();	// 向上转型
  - Son s1 = (Son)f1;    // 向下转型
- 出错
  - Father f2 = new Father()
  - Son s2 = (Son)f2
  - 错误原因，子类引用不能指向父类对象

> 例1中f1指向子类对象，它的子类s1引用当然可以指向子类对象；
>
> 例2中f2指向父类对象，它的子类s2引用不能指向父类对象（子类引用不能指向父类对象）