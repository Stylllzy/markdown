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

`this`

当前对象 或 当前正在创建的对象，可以调用的结构：***属性、方法；构造器***

方法中，我们可以使用"this.属性"或"this.方法"的方式，调用当前对象属性或方法。但是，通常情况下，我们都省略"this."。特殊情况下，如果***方法的形参和类的属性同名时***，我们必须显式的使用"this.变量"的方式，表明此变量是属性，而非形参。

构造器中，我们可以使用"this.属性"或"this.方法"的方式，调用当前正在创建的对象属性或方法。但是，通常情况下，我们都择省略"this."。特殊情况下，如果***构造器的形参和类的属性同名***时，我们必须显式的使用"this.变量"的方式，表明此变量是属性，而非形参。

`super`

父类的，可以用来调用的结构：***属性、方法、构造器***

- 尤其当子父类出现同名成员时，可以用super表明调用的是父类中的成员
- super的追溯不仅限于直接父类
- super和this的用法相像，this代表本类对象的引用， super代表父类的内存空间的标识

使用方式与 this 相同，只不过调用的是父类的属性

`两者区别`

| 区别       | this                                                   | super                                    |
| ---------- | ------------------------------------------------------ | ---------------------------------------- |
| 访问属性   | 访问本类中的属性，如果本类没有此属性则从父类中继续查找 | 直接访问父类中的属性                     |
| 调用方法   | 访问本类中的方法，如果本类没有此方法则从父类中继续查找 | 直接访问父类中的方法                     |
| 调用构造器 | 调用本类构造器，必须放在构造器的首行                   | 调用父类构造器，必须放在子类构造器的首行 |

`final`

可修饰类，方法，变量

1. final 用来修饰一个类:此类不能被其他类所继承。

   比如：String类、System类、StringBuffer类

2. final 用来修饰方法：表明此方法不可以被重写

   比如：Object类中getClass();

3. final 用来修饰变量：此时的"变量"就称为是一个常量

   - final修饰属性：可以考虑赋值的位置：显式初始化、代码块中初始化、构造器中初始化
   - final修饰局部变量：尤其是使用final修饰形参时，表明此形参是一个常量。当我们调用此方法时，给常量形参赋一个实参。一旦赋值以后，就只能在方法体内使用此形参，但不能进行重新赋值。

4. static final 用来修饰属性：全局常量

`abstract`

 抽象的，可以用来修饰：类、方法

==抽象类==

- 此类不能实例化
- 抽象类中一定有构造器，便于子类实例化时调用（涉及：子类对象实例化的全过程）
- 开发中，都会提供抽象类的子类，让子类对象实例化，完成相关的操作 --->抽象的使用前提：继承性

==抽象方法==

- 抽象方法只有方法的声明，没方法体
- 包含抽象方法的类，一定是一个抽象类。反之，抽象类中可以没有抽象方法的。
- 若子类重写了父类中的所的抽象方法后，此子类方可实例化
- 若子类没重写父类中的所的抽象方法，则此子类也是一个抽象类，需要使用abstract修饰

> 注意：
>
> - abstract不能用来修饰：属性、构造器等结构
> - abstract不能用来修饰私有方法、静态方法（静态方法不能被重写，抽象方法不能调用）、final的方法、final的类

`interface`

1. 接口中不能定义构造器的！意味着接口不可以实例化

2. 接口通过让类去实现(implements)的方式来使用。

   - 如果实现类覆盖了接口中的所抽象方法，则此实现类就可以实例化
   - 如果实现类没覆盖接口中所的抽象方法，则此实现类仍为一个抽象类

3. Java类可以实现多个接口 --->弥补了Java单继承性的局限性

   格式：class AA extends BB implements CC,DD,EE

4. 接口与接口间可以继承，多继承

5. 体现多态

Java 8 接口使用总结

- 知识点1：接口中定义的静态方法，只能通过接口来调用。
- 知识点2：通过实现类的对象，可以调用接口中的默认方法。如果实现类重写了接口中的默认方法，调用时，仍然调用的是重写以后的方法
- 知识点3：如果子类(或实现类)继承的父类和实现的接口中声明了同名同参数的默认方法，那么子类在没重写此方法的情况下，默认调用的是父类中的同名同参数的方法。-->类优先原则
- 知识点4：如果实现类实现了多个接口，而这多个接口中定义了同名同参数的默认方法，那么在实现类没重写此方法的情况下，报错。-->接口冲突。这就需要我们必须在实现类中重写此方法
- 知识点5：如何在子类(或实现类)的方法中调用父类、接口中被重写的方法



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





# 类与对象

二者关系： 对象，是类 new 出来的，派生出来的。

概念说明：

- 属性 = 成员变量 = field = 域、字段
- 方法 = 成员方法 = 函数 = method
- 创建类的对象 = 类的实例化 = 实例化类

## 匿名对象

创建对象但没显式的赋给一个变量名，即为匿名对象

特点：匿名对象只能调用一次

```java
PhoneMall mall = new PhoneMall();

//匿名对象的使用
mall.show(new Phone());
其中，
class PhoneMall{
    public void show(Phone phone){
        phone.sendEmail();
        phone.playGame();
    }
    
}
```

## 属性

与局部变量比较：

- 相同点
  - 定义变量的格式：数据类型 变量名 = 变量值
  - 先声明，后使用
  - 变量都其对应的作用域
- 不同点
  - 在类中声明的位置的不同
    - 属性：直接定义在类的一对{}内
    - 局部变量：声明在方法内、方法形参、代码块内、构造器形参、构造器内部的变量。
  - 关于权限修饰符的不同
    - 属性：可以在声明属性时，指明其权限，使用权限修饰符。
    - 常用的权限修饰符：private、public、缺省、protected --->封装性
    - 目前，声明属性时，使用缺省就可以。
    - 局部变量：不可以使用权限修饰符。
  - 在内存中加载的位置：
    - 属性：加载到堆空间中 （非static）
    - 局部变量：加载到栈空间

## 方法

`同类，同方法名，不同参数个数，不同参数类型`

重载：在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可。

重写（override）：子类继承父类以后，可以对父类中同名同参数的方法，进行覆盖操作。

> 子类不能重写父类中声明为private权限的方法

二者区别：

1. 重载：不表现为多态性。 重写：表现为多态性。
2. 从编译和运行的角度看：

- 重载，是指允许存在多个同名方法，而这些方法的参数不同。编译器根据方法不同的参数表，对同名方法的名称做修饰。对于编译器而言，这些同名方法就成了不同的方法。它们的调用地址在编译期就绑定了。Java的重载是可以包括父类和子类的，即子类可以重载父类的同名不同参数的方法。
- 所以对于重载而言，在方法调用之前，编译器就已经确定了所要调用的方法，这称为“早绑定”或“静态绑定”；
- 而对于多态，只等到方法调用的那一刻，解释运行器才会确定所要调用的具体方法，这称为“晚绑定”或“动态绑定”。

## 构造器

如果没显式的定义类的构造器的话，则系统默认提供一个空参的构造器

定义构造器的格式：`权限修饰符 类名(形参列表){ }`

一个类中定义的***多个构造器***，彼此构成重载

一旦我们显式的定义了类的构造器之后，系统就不再提供默认的空参构造器

一个类中，***至少会有一个构造器***。

## 内部类

Java中允许将一个类A声明在另一个类B中，则类A就是内部类，类B称为外部类。

- 成员内部类
  - 调用外部类的结构
  - 可以被static修饰
  - 可以被4种不同的权限修饰
  - 类内可以定义属性、方法、构造器等
  - 可以被final修饰，表示此类不能被继承。言外之意，不使用final，就可以被继承
  - 可以被abstract修饰

- 局部内部类
  - jdk 7及之前版本：要求此局部变量显式的声明为final的
  - jdk 8及之后的版本：可以省略final的声明





# 常用类

## String类

String:字符串，使用一对""引起来表示。

1. String声明为final的，不可被继承，代表不可变的字符序列
2. String 实现了 `Serializable` 接口：表示字符串是支持序列化的。 实现了  `Comparable` 接口：表示String可以比较大小
3. String内部定义了 `final char[] value` 用于存储字符串数据
4. String:代表不可变的字符序列。简称：不可变性。

`实例化方法`

- 字面量定义的方式
- new + 构造器 的方式

`转换`

String --> 基本数据类型、包装类：调用包装类的静态方法：`parseXxx(str)`

基本数据类型、包装类 --> String:调用String重载的 `valueOf(xxx)`

与字符数组之间的转换

String --> char[]:调用String的 `toCharArray() char[]` --> String:调用String的构造器

## StringBuffer类

代表可变的字符序列，JDK1.0中声明，可以对字符串内容进行增删，此时不会产生新的对象。 很多方法与 String相同 作为参数传递时，方法内部可以改变值。

StringBuffer类不同于 String，其对象必须使用构造器生成。

有三个构造器:

- `StringBuffer()`：初始容量为16的字符串缓冲区
- `StringBuffer(int size)`：构造指定容量的字符串缓冲区
- `StringBuffer(String str)`：将内容初始化为指定字符串内容

总结：

- 增：`append(xxx)` ；
- 删：`delete(int start,int end)` ；
- 改：`setCharAt(int n ,char ch) `/` replace(int start, int end, String str)` ；
- 查：`charAt(int n )` ；
- 插：`insert(int offset, xxx)` ；
- 长度：`length()`;
- 遍历：`for() + charAt() `/` toString()`；

## StringBuilder类

StringBuilder和 StringBuffer非常类似，均代表可变的字符序列，而且提供相关功能的方法也一样，只是**StringBuilder类没有加线程锁，执行效率更高。**

==三者比较==

- String:不可变的字符序列；底层使用 `char[]` 存储；占用内存（会不断的创建和回收对象）
- StringBuffer:可变的字符序列；线程安全的，效率低；线程安全；底层使用char[]存储；
- StringBuilder:可变的字符序列；jdk5.0新增的，线程不安全的，效率高；线程不安全；底层使用 `char[]` 存储

> 参数传递，方法内部String不会改变其值，StringBuffer 和 StringBuilder 会改变其值

## Date类

`构造器`

Date():使用无参的构造器创建对象可以获取本地当前时间

Date(long date)

`方法`

getTime()：返回自1970年1月1日00：00：00GMT以来此Date对象表示的毫秒数

tostring()：把此Date对象转换为以下形式的 String：

`jdk 1.8 新API`

java.time	–	包含值对象的基础包

java.time.format	–	格式化和解析时间和日期

- LocalDate
- LocalTime
- LocalDateTime

## Math类

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e243229d76c4468a5c88224f7de2562~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="image-20200421115050704" style="zoom:67%;" />





# 枚举类和注解

`枚举类`：类的对象只有有限个，确定的。

当需要用到一组常量时，就使用枚举类。

`枚举类的属性`：枚举类对象的属性不应允许被改动，所以应该使用 private final修饰 枚举类的使用 private final修饰的属性应该在构造器中为其赋值 若枚举类显式的定义了带参数的构造器，则在列出枚举值时也必须对应的传入参数。

## 如何自定义枚举类

1. 私有化构造器，保证不能在类的外部创建其对象；
2. 在类的内部创建枚举类的示例。声明为：public static final；
3. 对象如果有实例变量，应该声明为private final，并在构造器中初始化；

```java
//自定义枚举类
class Season{
    //1.声明Season对象的属性:private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //2.私化类的构造器,并给对象属性赋值
    private Season(String seasonName,String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //3.提供当前枚举类的多个对象：public static final的
    public static final Season SPRING = new Season("春天","春暖花开");
    public static final Season SUMMER = new Season("夏天","夏日炎炎");
    public static final Season AUTUMN = new Season("秋天","秋高气爽");
    public static final Season WINTER = new Season("冬天","冰天雪地");

    //4.其他诉求1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }
    //4.其他诉求1：提供toString()
    @Override
    public String toString() {
        return "Season{" +
            "seasonName='" + seasonName + '\'' +
            ", seasonDesc='" + seasonDesc + '\'' +
            '}';
    }
}
```

## enum定义枚举类

- 使用enum定义的枚举类默认继承了 `java.lang.Enum` 类，因此不能再继承其他类
- 枚举类的构造器只能使用private权限修饰符
- 枚举类的所有实例必须在枚举类中显式列出(`,` 分隔 `;` 结尾)。列出的实例系统会自动添加`public static final` 修饰
- 必须在枚举类的第一行声明枚举类对象

```java
//使用enum关键字枚举类
enum Season1 {
    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
    SPRING("春天","春暖花开"),
    SUMMER("夏天","夏日炎炎"),
    AUTUMN("秋天","秋高气爽"),
    WINTER("冬天","冰天雪地");

    //2.声明Season对象的属性:private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //2.私化类的构造器,并给对象属性赋值

    private Season1(String seasonName,String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //4.其他诉求1：获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

}
```

`Enum类的常用方法`

`values()`方法：返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。

`valueOf(String str)`：可以把一个字符串转为对应的枚举类对象。要求字符串必须是枚举类对象的“名字”.如不是，会有运行时异常 `IllegalArgumentException`

`toString()`：返回当前枚举类对象常量的名称

```java
Season1 summer = Season1.SUMMER;
//toString():返回枚举类对象的名称
System.out.println(summer.toString());

//        System.out.println(Season1.class.getSuperclass());
System.out.println("****************");
//values():返回所的枚举类对象构成的数组
Season1[] values = Season1.values();
for(int i = 0;i < values.length;i++){
    System.out.println(values[i]);
}
System.out.println("****************");
Thread.State[] values1 = Thread.State.values();
for (int i = 0; i < values1.length; i++) {
    System.out.println(values1[i]);
}

//valueOf(String objName):返回枚举类中对象名是objName的对象。
Season1 winter = Season1.valueOf("WINTER");
//如果没objName的枚举类对象，则抛异常：IllegalArgumentException
//        Season1 winter = Season1.valueOf("WINTER1");
System.out.println(winter);
```

`用Enum类定义的枚举类对象分别实现接口`

1. 和普通Java类一样，枚举类可以实现一个或多个接口
2. 若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只要统一实现该方法即可。
3. 若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式，则可以让每个枚举值分别来实现该方法

```java
interface Info{
    void show();
}

//使用enum关键字枚举类
enum Season1 implements Info{
    //1.提供当前枚举类的对象，多个对象之间用","隔开，末尾对象";"结束
    SPRING("春天","春暖花开"){
        @Override
        public void show() {
            System.out.println("春天在哪里？");
        }
    },
    SUMMER("夏天","夏日炎炎"){
        @Override
        public void show() {
            System.out.println("宁夏");
        }
    },
    AUTUMN("秋天","秋高气爽"){
        @Override
        public void show() {
            System.out.println("秋天不回来");
        }
    },
    WINTER("冬天","冰天雪地"){
        @Override
        public void show() {
            System.out.println("大约在冬季");
        }
    };
}
```

## 注解的使用

Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。通过使用 Annotation,程序员可以在不改变原逻辑的情况下, 在源文件中嵌入一些补充信息。

Annotation可以像修饰符一样使用，可以用来修饰包、类、构造器、方法、成员变量、参数、局部变量的声明，这些信息被保存在Annotation的 `name = value` 对中。

在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Android 中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗 代码和XML配置等。

框架 = 注解 + 反射机制 + 设计模式

==示例一==： 生成文档相关的注解

- `@author` 标明开发该类模块的作者，多个作者之间使用，分割 `@version` 标明该类模块的版本；
- `@see` 参考转向，也就是相关主题；
- `@since` 从哪个版本开始增加的；
- `@param` 对方法中某参数的说明，如果没有参数就不能写 ` @return` 对方法返回值的说明，如果方法的返回值类型是 `void` 就不能写 `@exception` 对方法可能抛出的异常进行说明，如果方法没有用 `throws` 显式抛出的异常就不能写；
- 其中 `@param` 、 `@return` 和 `@exception` 这三个标记都是只用于方法的。
- `@param` 的格式要求：`@param` 形参名形参类型形参说明；
- `@return` 的格式要求：`@return` 返回值类型返回值说明；
- `@exception` 的格式要求：`@exception` 异常类型异常说明；
- `@param` 和 `@exception` 可以并列多个；

```java
/**
 * @author bruce
 * @project_name JavaSenior
 * @package_name com.bruce.java
 * @create 2020-04-26 10:58
 */
public class AnnotationTest {
    /**
     *程序的主方法
     * @param args 传入命令行参数
     */
    public static void main(String[] args) {

    }

    /**
     * 求圆形面积
     * @param radius 所求面积的半径
     * @return 面积值
     */
    public static double getArea(double radius){
        return Math.PI * radius * radius;
    }
}
```

==示例二==：编译时进行格式检查

- `@Override`: 限定重写父类方法, 该注解只能用于方法；
- `@Deprecated`: 用于表示所修饰的元素(类, 方法等)已过时。通常是因为所修饰的结构危险或存在更好的择；
- `@SuppressWarnings`: 抑制编译器警告；

```java
public class AnnotationTest{
    public static void mian (String [] args){
        @SuppressWarning("unused")
        int a = 0;
    }
    @Deprecated
    public void print(){
        System.out.print("过时的方法");
    }
    @Override
    public String toString(){
        return "重写的toString方法";
    }
}
```





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





# 集合

==弥补数组存储的不足==

数组存储的弊端：

1. 一旦初始化以后，其长度就不可修改。
2. 数组中提供的方法非常限，对于添加、删除、插入数据等操作，非常不便，同时效率不高。
3. 获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用
4. 数组存储数据的特点：有序、可重复。对于无序、不可重复的需求，不能满足。

==分类==

Java集合可分为Collection和Map两种体系

- Collection接口：单列数据，定义了存取一组对象的方法的集合
  - List：元素有序、可重复的集合
  - Set：元素无序、不可重复的集
- Map接口：双列数据，保存具有映射关系“key-value对”的集合

==框架结构==

```java
|----Collection接口：单列集合，用来存储一个一个的对象
     |----List接口：存储有序的、可重复的数据。  -->“动态”数组
           |----ArrayList：作为List接口的主要实现类，线程不安全的，效率高;底层采用Object[] elementData数组存储
           |----LinkedList：对于频繁的插入删除操作，使用此类效率比ArrayList效率高底层采用双向链表存储
           |----Vector：作为List的古老实现类，线程安全的，效率低;底层采用Object[]数组存储
           
     |----Set接口：存储无序的、不可重复的数据   -->数学概念上的“集合”
           |----HashSet：作为Set接口主要实现类;线程不安全;可以存null值
           		|----LinkedHashSet：作为HashSet的子类;遍历其内部数据时，可以按照添加顺序遍历;对于频繁的遍历操作，LinkedHashSet效率高于HashSet.
           |----TreeSet：可以按照添加对象的指定属性，进行排序。


|----Map:双列数据，存储key-value对的数据   ---类似于高中的函数：y = f(x)
     |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value
          |----LinkedHashMap:保证在遍历map元素时，可以照添加的顺序实现遍历。
                    原因：在原的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
                    对于频繁的遍历操作，此类执行效率高于HashMap。
     |----TreeMap:保证照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序
                      底层使用红黑树
     |----Hashtable:作为古老的实现类；线程安全的，效率低；不能存储null的key和value
          |----Properties:常用来处理配置文件。key和value都是String类型
```

## Collection

- Collection接口是List、Set和Queue接口的父接口，该接口里定义的方法既可用于操作Set集合，也可用于操作List和 Queue集合。
- JDK不提供此接口的任何直接实现，而是提供更具体的子接口（如：Set和List）实现。
- 在JDK 5.0之前，Java集合会丢失容器中所有对象的数据类型，把所有对象都当成 Object类型处理；从JDK 5.0增加了泛型以后，Java集合可以记住容器中对象的数据类型。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a12d4e0b0d4d46dba04719c62c10359c~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="image-20200427090109218" style="zoom: 50%;" />

==接口常用方法==

添加

- `add(Object obj)`
- `addAll(Collection coll)`

获取有效元素个数

- `int size()`

清空集合

- `void clear()`

是否为空集合

- `boolean isEmpty()`

是否包含某个元素

- `boolean contains(Object obj)`:是通过元素的equals方法来判断是否是同一个对象
- `boolean containsAll(Collection c)`:也是调用元素的equals方法来比较的。用两个两个集合的元素逐一比较

删除

- `boolean remove(Object obj)`:通过元素的equals方法判断是否是要删除的那个元素。只会删除找到的第一个元素
- `boolean removeAll(Collection coll)`:取当前集合的差集

取两个集合的交集

- `boolean retainAll(Collection c)`:把交集的结果存在当前的集合中，不影响c

集合是否相等

- `boolean equals(Object obj)`

转换成对象数组

- `Object [] toArray()`

获取集合对象的哈希值

```java
-   `hashCode()`
```

遍历

```java
-   `iterator()`:返回迭代器对象，用于集合遍历
```

`Collection集合与数组间的转换`

```java
//集合 --->数组：toArray()
Object[] arr = coll.toArray();
for(int i = 0;i < arr.length;i++){
    System.out.println(arr[i]);
}

//拓展：数组 --->集合:调用Arrays类的静态方法asList(T ... t)
List<String> list = Arrays.asList(new String[]{"AA", "BB", "CC"});
System.out.println(list);

List arr1 = Arrays.asList(new int[]{123, 456});
System.out.println(arr1.size());//1

List arr2 = Arrays.asList(new Integer[]{123, 456});
System.out.println(arr2.size());//2
```

> 使用 Collection 集合存储对象，要求对象所属的类满足：
>
> 向 Collection 接口的实现类的对象中添加数据 obj 时，要求 obj 所在类要重写 `equals()`。

`遍历Collection的两种方式`

- 使用迭代器Iterator

```java
Iterator iterator = coll.iterator();
//hasNext():判断是否还下一个元素
while(iterator.hasNext()){
    //next():①指针下移 ②将下移以后集合位置上的元素返回
    System.out.println(iterator.next());
}
```

- foreach循环（或增强for循环）

```java
@Test
public void test1(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);

    //for(集合元素的类型 局部变量 : 集合对象)
    
    for(Object obj : coll){
        System.out.println(obj);
    }
}
```

### List

```java
@Test
public void test2(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(456);
    list.add("AA");
    list.add(new Person("Tom",12));
    list.add(456);
    //int indexOf(Object obj):返回obj在集合中首次出现的位置。如果不存在，返回-1.
    int index = list.indexOf(4567);
    System.out.println(index);

    //int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置。如果不存在，返回-1.
    System.out.println(list.lastIndexOf(456));

    //Object remove(int index):移除指定index位置的元素，并返回此元素
    Object obj = list.remove(0);
    System.out.println(obj);
    System.out.println(list);

    //Object set(int index, Object ele):设置指定index位置的元素为ele
    list.set(1,"CC");
    System.out.println(list);

    //List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的左闭右开区间的子集合
    List subList = list.subList(2, 4);
    System.out.println(subList);
    System.out.println(list);
}


@Test
public void test1(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(456);
    list.add("AA");
    list.add(new Person("Tom",12));
    list.add(456);

    System.out.println(list);

    //void add(int index, Object ele):在index位置插入ele元素
    list.add(1,"BB");
    System.out.println(list);

    //boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
    List list1 = Arrays.asList(1, 2, 3);
    list.addAll(list1);
    //        list.add(list1);
    System.out.println(list.size());//9

    //Object get(int index):获取指定index位置的元素
    System.out.println(list.get(0));

}
```

==ArrayList==

- ArrayList是List接口的典型实现类、主要实现类

- 本质上，ArrayList是对象引用的一个”变长”数组

- Array Listi的JDK 1.8之前与之后的实现区别？

  - JDK 1.7：ArrayList像饿汉式，直接创建一个初始容量为10的数组

  - JDK 1.8：ArrayList像懒汉式，一开始创建一个长度为0的数组，当添加第一个元素时再创建一个始容量为10的数组

    `Arrays.asList(...)`方法返回的List集合，既不是 ArrayList实例，也不是Vector实例。`Arrays.asList(...)`返回值是一个固定长度的List集合

==linkedList==

- 对与对于频繁的插入和删除元素操作，建议使用LinkedList类，效率更高
- 新增方法：
  - `void addFirst(Object obj)`
  - `void addLast(Object obj)`
  - `Object getFirst()`
  - `Object getlast)()`
  - `Object removeFirst()`
  - `Object removeLast()`
- Linkedlist：双向链表，内部没有声明数组，而是定义了Node类型的frst和last，用于记录首末元素。同时，定义内部类Node，作为 Linkedlist中保存数据的基本结构。Node除了保存数据，还定义了两个变量：
  - prev：变量记录前一个元素的位置
  - next：变量记录下一个元素的位置

### Set

- Set接口是Collection的子接口，set接口没有提供额外的方法
- Set集合不允许包含相同的元素，如果试把两个相同的元素加入同一个Set集合中，则添加操作失败。（多用于过滤操作，去掉重复数据）
- Set判断两个对象是否相同不是使用==运算符，而是根据equals()方法

==HashSet==

- Hashset是Set接口的典型实现，大多数时候使用Set集合时都使用这个实现类。
- HashSet按Hash算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。
- HashSet具有以下特点：
  - 不能保证元素的排列顺序
  - HashSet不是线程安全的
  - 集合元素可以是null
- HashSet集合判断两个元素相等的标准：两个对象通过hashCode()方法比较相等，并且两个对象的equals()方法返回值也相等。
- 对于存放在Set容器中的对象，对应的类一定要重写equals()和hashCode(Object obj)方法，以实现对象相等规则。即：“相等的对象必须具有相等的散列码”

==LinkedHashSet==

- LinkedhashSet是HashSet的子类
- LinkedhashSet根据元素的hashCode值来决定元素的存储位置但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入顺序保存的。
- LinkedhashSet插入性能略低于HashSet，但在迭代访问Set里的全部元素时有很好的性能。
- LinkedhashSet不允许集合元素重复。

==TreeSet==

- Treeset是SortedSet接口的实现类，TreeSet可以确保集合元素处于排序状态。
- TreeSet底层使用红黑树结构存储数据
- TreeSet两种排序方法：自然排序和定制排序。默认情况下，TreeSet采用自然排序。

`存储对象所在类的要求`

HasSet / LinkedHashSet

- 所在类一定要重写hashCode()，equals()
- 重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码

TreeSet

- 自然排序中，比较两个对象是否相同的标准为：`compareTo()` 返回0.不再是 `equals()`
- 定制排序中，比较两个对象是否相同的标准为：`compare()` 返回0.不再是 `equals()`

## Map

- Map与Collection并列存在。用于保存具有映射关系的数据:key-value
- Map中的key和value都可以是任何引用类型的数据
- Map中的key用set来存放，不允许重复，即同一个Map对象所对应的类，须重写 `hashCode()` 和 `equals()` 方法
- 常用 String类作为Map的“键”
- key和value之间存在单向一对一关系，即通过指定的key总能找到唯一的、确定的value
- Map接口的常用实现类:HashMap、TreeMap、LinkedHashMap和Properties。其中，HashMap是Map接口使用频率最高的实现类

```java
|----Map:双列数据，存储key-value对的数据   ---类似于高中的函数：y = f(x)
     |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value
          |----LinkedHashMap:保证在遍历map元素时，可以照添加的顺序实现遍历。
                    原因：在原的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
                    对于频繁的遍历操作，此类执行效率高于HashMap。
     |----TreeMap:保证照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序
                      底层使用红黑树
     |----Hashtable:作为古老的实现类；线程安全的，效率低；不能存储null的key和value
          |----Properties:常用来处理配置文件。key和value都是String类型


HashMap的底层： 数组+链表  （JDK 7.0及之前)
               数组+链表+红黑树 （JDK 8.0以后)
```

### HashMap

- HashMap是Map接口使用频率最高的实现类。
- 允许使用null键和null值，与 HashSet一样，不保证映射的顺序。
- 所有的key构成的集合是set：无序的、不可重复的。所以，key所在的类要重写equals()和 hashCode()
- 所有的value构成的集合是Collection：无序的、可以重复的。所以，value所在的类要重写:equals()
- 一个key-value构成一个entry
- 所有的entry构成的集合是Set：无序的、不可重复的
- HashMap判断两个key相等的标准是：两个key通过 `equals()` 方法返回true，hashCode值也相等。
- HashMap判断两个value相等的标准是：两个value通过 `equals()` 方法返回true.

```java
@Test
public void test1(){
    Map map = new HashMap();

    map.put(null,123);

}
```

### LinkedHashMap

- LinkedHashMap底层使用的结构与HashMap相同，因为LinkedHashMap继承于HashMap.
- 区别就在于：LinkedHashMap内部提供了Entry，替换HashMap中的Node.
- 与Linkedhash Set类似，LinkedHashMap可以维护Map的迭代顺序：迭代顺序与Key-value对的插入顺序一致

```java
@Test
public void test2(){
    Map map = new LinkedHashMap();
    map.put(123,"AA");
    map.put(345,"BB");
    map.put(12,"CC");

    System.out.println(map);
} 
```

### TreeMap

- TreeMap存储Key-Value对时，需要根据key-value对进行排序。TreeMap可以保证所有的 Key-Value对处于有序状态。
- TreeSet底层使用红黑树结构存储数据
- TreeMap的Key的排序:
  - 自然排序： TreeMap的所有的Key必须实现Comparable接口，而且所有的Key应该是同一个类的对象，否则将会抛出ClasssCastEXception()
  - 定制排序：创建 TreeMap时，传入一个 Comparator对象，该对象负责对TreeMap中的所有key进行排序。此时不需要Map的Key实现Comparable接口
- TreeMap判断两个key相等的标准：两个key通过 compareTo()方法或者compare()方法返回0.

### Hashtable

- Hashtable是个古老的Map实现类，JDK1.0就提供了。不同于 HashMap，Hashtable是线程安全的.
- Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用
- 与HashMap.不同，Hashtable不允许使用null作为key和value.
- 与HashMap一样，Hashtable也不能保证其中Key-value对的顺序.
- Hashtable判断两个key相等、两个value相等的标准，与HashMap一致.

### Properties

- Properties类是Hashtable的子类，该对象用于处理属性文件
- 由于属性文件里的key、value都是字符串类型，所以Properties里的key和value都是字符串类型
- 存取数据时，建议使用 `setProperty(String key,String value)` 方法和 `getProperty(String key)` 方法

### 常用方法

- `Object put(Object key,Object value)`：将指定key-value添加到(或修改)当前map对象中
- `void putAll(Map m)`:将m中的所有key-value对存放到当前map中
- `Object remove(Object key)`：移除指定key的key-value对，并返回value
- `void clear()`：清空当前map中的所有数据

```java
@Test
public void test1() {
    Map map = new HashMap();
    //Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
    map.put("AA",123);
    map.put("ZZ",251);
    map.put("CC",110);
    map.put("RR",124);
    map.put("FF",662);
    System.out.println(map);//{AA=123, ZZ=251, CC=110, RR=124, FF=662}

    //Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
    map.put("ZZ",261);
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662}

    //void putAll(Map m):将m中的所有key-value对存放到当前map中
    HashMap map1 = new HashMap();
    map1.put("GG",435);
    map1.put("DD",156);
    map.putAll(map1);
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662, GG=435, DD=156}

    //Object remove(Object key)：移除指定key的key-value对，并返回value
    Object value = map.remove("GG");
    System.out.println(value);//435
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662, DD=156}

    //void clear()：清空当前map中的所有数据
    map.clear();
    System.out.println(map.size());//0  与map = null操作不同
    System.out.println(map);//{}
}
```

- 添加：`put(Object key,Object value)`
- 删除：`remove(Object key)`
- 修改：`put(Object key,Object value)`
- 查询：`get(Object key)`
- 长度：`size()`
- 遍历：`keySet()` / `values()` / `entrySet()`





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

## instanceof

1. a instanceof A:==判断对象a==是否是==类A的实例==。如果是，返回true；如果不是，返回false。
2. 如果 a instanceof A返回true,则 a instanceof B也返回true.其中，类B是类A的父类。
3. 要求a所属的类与类A必须是子类和父类的关系，否则编译错误。





# 泛型

- 所谓泛型，就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返 回值及参数类型。这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、 创建对象时确定（即传入实际的类型参数，也称为类型实参）。

`集合中使用泛型的例子`

```java
@Test
public void test1(){
    ArrayList list = new ArrayList();
    //需求：存放学生的成绩
    list.add(78);
    list.add(76);
    list.add(89);
    list.add(88);
    //问题一：类型不安全
    //        list.add("Tom");

    for(Object score : list){
        //问题二：强转时，可能出现ClassCastException
        int stuScore = (Integer) score;
        System.out.println(stuScore);
    }
}
```

==例子1==

```java
//在集合中使用泛型，以ArrayList为例
@Test
public void test1(){
    ArrayList<String> list = new ArrayList<>();
    list.add("AAA");
    list.add("BBB");
    list.add("FFF");
    list.add("EEE");
    list.add("CCC");
	//遍历方式一：
    Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
    System.out.println("-------------");
    //便利方式二：
    for (String str:
         list) {
        System.out.println(str);
    }
}
```

==例子2==

```java
@Test
//在集合中使用泛型的情况：以HashMap为例
public void test2(){
    Map<String,Integer> map = new HashMap<>();//jdk7新特性：类型推断
    map.put("Tom",26);
    map.put("Jarry",30);
    map.put("Bruce",28);
    map.put("Davie",60);
    //嵌套循环
    Set<Map.Entry<String, Integer>> entries = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator = entries.iterator();

    while (iterator.hasNext()){
        Map.Entry<String, Integer> entry = iterator.next();
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.println(key+"="+value);
    }
}
```

## 声明

- interface `List<T>` 和 `class GenTest<K,V>` 其中，T，K，V，不代表值，而是表示类型。这里使用任意字母都可以。
- 常用T表示，是Type的缩写。

`实例化`

一定要在类名后面指定类型参数的值（类型）。如：

```java
List<String> strList =new ArrayList<String>();
Iterator<Customer> iterator = customers.iterator();

//JDK 5.0以前
Comparable c = new Date();
System.out.println(c.comparaTo("red");
                   
//JDK 5.0以后
Comparable <Date> c = new Date();
System.out.println(c.comparaTo("red");                   
```

> 泛型的主要优点在于能在编译时而不是在运行时检测错误

`通配符`

- 使用类型通配符：`?`

  比如：`List<?>`，`Map<?,?>`

  `List<?>` 是 `List<String>`、`List<Object>` 等各种泛型 List 的父类。

- 读取 `List<?>` 的对象list中的元素时，永远是安全的，因为不管list的真实类型是什么，它包含的都是Object

- 写入list中的元素时，不可以。因为我们不知道c的元素类型，我们不能向其中添加对象。 除了添加null之外。





# 反射

- Reflection（反射)是被视为动态语言的关键，反射机制允许程序在执行期借助于 `Reflection API` 取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。
- 加载完类之后，在堆内存的方法区中就产生了一个 `Class` 类型的对象（一个类只有一个 `Class` 对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为：反射。

通常的方式：引入需要的“包类”名称---->通过 `new` 实例化---->获得实例化对象

反射的方式：实例化对象----> `getClass()` 方法---->得到完整的“包类”名称

框架 = 注解 + 反射 + 设计模式







# IO流

## File类

```java
@Test
public void test1() {
    //构造器1
    File file1 = new File("hello.txt");
    File file2 = new File("E:\\workspace_idea\\JavaSenic\\IO\\hello.txt");
    System.out.println(file1);
    System.out.println(file2);

    //构造器2
    File file3 = new File("E:\\workspace_idea\\JavaSenior", "hello.txt");
    System.out.println(file3);

    //构造器3
    File file4 = new File(file3, "hi.txt");
    System.out.println(file4);
}
```

`路径分类`

- IDEA中：
  - 如果使用JUnit中的单元测试方法测试，相对路径即为当前Module下。
  - 如果使用main()测试，相对路径即为当前的Project下。

`分隔符`

- windows和DOS系统默认使用 `\` 来表示
- UNIX和URL使用 `/` 来表示
- Java程序支持跨平台运行，因此路径分隔符要慎用。
- 为了解决这个隐患，File类提供了一个常量： `public static final String separator`。根据操作系统，动态的提供分隔符。

```java
//windows和DOS系统
File file1 = new File("E:\\io\\test.txt");
//UNIX和URL
File file = new File("E:/io/test.txt");
//java提供的常量
File file = new File("E:"+File.separator+"io"+File.separator+"test.txt");
```

## 流的分类

**操作数据单位：字节流、字符流**

- 对于文本文件(`.txt,.java,.c,.cpp`)，使用字符流处理
- 对于非文本文件(`.jpg,.mp3,.mp4,.avi,.doc,.ppt,...`)，使用字节流处理

**数据的流向：输入流、输出流**

- 输入 input:读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
- 输出 output:将程序（内存）数据输出到磁盘、光盘等存储设备中。

**流的角色：节点流、处理流**

- 节点流：直接从数据源或目的地读写数据。
- 处理流：不直接连接到数据源或目的地，而是“连接”在已存在的流（节点流或处理流）之上，通过对数据的处理为程序提供更为强大的读写功能。



==实现文本文件的复制==

```java
@Test
public void testFileReaderFileWriter() {
    FileReader fr = null;
    FileWriter fw = null;
    try {
        //1.创建File类的对象，指明读入和写出的文件
        File srcFile = new File("hello.txt");
        File destFile = new File("hello2.txt");

        //不能使用字符流来处理图片等字节数据
        //            File srcFile = new File("test.jpg");
        //            File destFile = new File("test1.jpg");

        //2.创建输入流和输出流的对象
        fr = new FileReader(srcFile);
        fw = new FileWriter(destFile);

        //3.数据的读入和写出操作
        char[] cbuf = new char[5];
        int len;//记录每次读入到cbuf数组中的字符的个数
        while((len = fr.read(cbuf)) != -1){
            //每次写出len个字符
            fw.write(cbuf,0,len);

        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.关闭流资源
        try {
            if(fw != null)
                fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        try {
            if(fr != null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

剩下见 

[掘金Java]:https://juejin.cn/post/6961701925108580365#heading-28





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





# 多线程（操作系统）

进程可以分化为多个线程，各个线程拥有自己独立的 栈，程序计数器，多个线程共享一个进程中的结构，方法区，堆。

多核CPU才能更好发挥多线程的效率

一个 java应用程序 java.exe，至少三个线程，

- main() 主线程
- gc() 垃圾回收线程
- 异常处理线程

若发生异常，会影响到主线程

## `Thread类`

构造器

- Thread()：创建新的 Thread对象

- Thread（String threadName）：创建线程并指定线程实例名

- Thread（Runnable target）：指定创建线程的目标对象，它实现了 Runnable接口中的run方法

- Thread（Runnable target， String name）：创建新的 Thread对象

`创建多线程的两种方式`

1. 继承Thread类的方式

   1. 创建一个继承于Thread类的子类
   2. 重写Thread类的run() --> 将此线程执行的操作声明在run()中
   3. 创建Thread类的子类的对象
   4. 通过此对象调用start()：①启动当前线程 ② 调用当前线程的run()

   ```java
   //1.继承Thread类
   class MyThread extends Thread {
       public MyThread() {
       }
   
       //2.重run方法
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               if (i % 2 == 0) {
                   System.out.println(i);
               }
           }
       }
   }
   
   public class ThreadTest {
       public static void main(String[] args) {
   //3.新建Thread对象
           MyThread myThread = new MyThread();
           //4.调用start方法
           myThread.start();
       }
   }
   ```

   

2. 实现Runnable接口的方式

   1. 创建一个实现了Runnable接口的类
   2. 实现类去实现Runnable中的抽象方法：run()
   3. 创建实现类的对象
   4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
   5. 通过Thread类的对象调用start()

   ```java
   //1. 创建一个实现了Runnable接口的类
   public class RunnableTest implements Runnable {
       // 2. 实现类去实现Runnable中的抽象方法：run()
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               System.out.println(i);
           }
       }
   }
   
   class test {
       public static void main(String[] args) {
           //3. 创建实现类的对象
           RunnableTest runnableTest = new RunnableTest();
           //4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
           Thread thread = new Thread(runnableTest);
           //5. 通过Thread类的对象调用start()
           thread.start();
   
       }
   }
   ```

两种方式，在开发中优先Runnable接口的方式

理由：

- 实现的方式没类的单继承性的局限性
- 实现的方式更适合来处理多个线程共享数据的情况。

## `同步机制`

解决线程的安全问题

方式一

```java
synchronized(同步监视器){//同步监视器就是需要同步线程的公共对象
   //需要被同步的代码
    
}
```

- 在实现Runnable接口创建多线程的方式中，我们可以考虑使用this充当同步监视器。
- 在继承Thread类创建多线程的方式中，慎用this充当同步监视器，考虑使用当前类充当同步监视器。

==继承Runnable接口形式同步代码块==

```java
public class Ticket implements Runnable {
    private int tick = 100;

    @Override
    public void run() {

        while (true) {
            synchronized (this) {
                if (tick > 0) {
                    System.out.println(Thread.currentThread().getName() + "号窗口买票，票号为：" + tick--);
                } else {
                    break;
                }
            }
        }
    }
}

class TicketTest {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        Thread thread1 = new Thread(ticket);
        Thread thread2 = new Thread(ticket);
        Thread thread3 = new Thread(ticket);

        thread1.setName("窗口1");
        thread2.setName("窗口2");
        thread3.setName("窗口3");

        thread1.start();
        thread2.start();
        thread3.start();

    }
}
```

方式二：同步方法

如果操作共享数据的代码完整的声明在一个方法中，我们不妨将此方法声明同步的。

```java
public synchronized void show(String namer){
....
}
```

方式三：Lock锁 --- JDK 5.0新增

- 从JDK 5.0开始，Java提供了更强大的线程同步机制--通过显式定义同步锁对象来实现同步。同步锁使用Lock对象充当。

- java.util.concurrent.locks.Lock接口是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对Lock对象加锁，线程开始访问共享资源之前应先获得Lock对象。

- ReentrantLock类实现了Lock，它拥有与 synchronized相同的并发性和内存语义，在实现线程安全的控制中，比较常用的是 Reentrantlock，可以显式加锁、释放锁。

  ```java
  class A {
      //1.实例化ReentrantLock对象
      private final ReenTrantLock lock = new ReenTrantLook();
      public void m (){
          lock.lock//2.先加锁
          try{
              //保证线程同步的代码
          }finally{
              lock.unlock();//3.后解锁
          }
      }
  }
  
  //注意：如果同步代码块有异常，要将unlock()写入finally语句块中
  ```

## 面试题

==**synchronized 与 Lock的异同？**==

相同：二者都可以解决线程安全问题

不同：

- synchronized机制在执行完相应的同步代码以后，自动的释放同步监视器

- Lock需要手动的启动同步（lock()，同时结束同步也需要手动的实现（unlock()）

==**使用的优先顺序：**==

Lock---> 同步代码块（已经进入了方法体，分配了相应资源 ) --->同步方法（在方法体之外)

==**利弊：**==

同步的方式，解决了线程的安全问题。---好处 操作同步代码时，只能一个线程参与，其他线程等待。相当于是一个单线程的过程，效率低。

==**Java是如何解决线程安全问题的，有几种方式？并对比几种方式的不同**==

利用同步锁的方式，有三种方式

- 同步代码块、同步方法和用lock方法

==**sleep() 和 wait()的异同？**==

相同点：一旦执行方法，都可以使得当前的线程进入阻塞状态。

不同点：

1）两个方法声明的位置不同：Thread类中声明sleep() , Object类中声明wait()

2）调用的要求不同：sleep()可以在任何需要的场景下调用。 wait()必须使用在同步代码块或同步方法中

3）关于是否释放同步监视器：如果两个方法都使用在同步代码块或同步方法中，sleep()不会释放锁，wait()会释放锁。

==**Java中多线程的创建有几种方式？四种。**==

JDK 5.0以前：

- 即继承Thread类重run方法
- 实现Runnable接口实现run方法

JDK 5.0以后：

- 实现callable接口，实现call方法
- 利用线程池



## 线程通讯

解决死锁问题

三个方法：

- wait():一旦执行此方法，当前线程就进入阻塞状态，并释放同步监视器。
- notify():一旦执行此方法，就会唤醒被wait的一个线程。如果有多个线程被wait，就唤醒优先级高的那个。
- notifyAll():一旦执行此方法，就会唤醒所有被wait的线程。

`示例`

```java
class MyThread implements Runnable {
    private int number = 1;
    private Object object = new Object();

    @Override
    public void run() {
        while (true) {

            synchronized (object) {
                object.notify();//调用notify()方法唤醒线程
                if (number <= 100) {
                    //线程休眠
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println(Thread.currentThread().getName() + number);
                    number++;

                    try {
                        object.wait();//打印输出一次后调用wait()方法将线程阻塞
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                } else {
                    break;
                }
            }
        }
    }
}

public class CommunicationTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();

        Thread thread1 = new Thread(myThread);
        Thread thread2 = new Thread(myThread);

        thread1.setName("线程1:");
        thread2.setName("线程2:");

        thread1.start();
        thread2.start();
    }
}
```



## 新增

`新增方式一：实现Callable接口`

- 创建一个实现Callable的实现类
- 实现call方法，将此线程需要执行的操作声明在call()中
- 创建Callable接口实现类的对象
- 将此Callable接口实现类的对象作为传递到FutureTask构造器中，创建FutureTask的对象
- 将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()
- 获取Callable中call方法的返回值

```java
//1.创建一个实现Callable的实现类
class NumThread implements Callable{
    //2.实现call方法，将此线程需要执行的操作声明在call()中
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            if(i % 2 == 0){
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}


public class ThreadNew {
    public static void main(String[] args) {
        //3.创建Callable接口实现类的对象
        NumThread numThread = new NumThread();
        //4.将此Callable接口实现类的对象作为传递到FutureTask构造器中，创建FutureTask的对象
        FutureTask futureTask = new FutureTask(numThread);
        //5.将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()
        new Thread(futureTask).start();

        try {
            //6.获取Callable中call方法的返回值
            //get()返回值即为FutureTask构造器参数Callable实现类重写的call()的返回值。
            Object sum = futureTask.get();
            System.out.println("总和为：" + sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }

}
```

优点：

1. call()可以返回值的。
2. call()可以抛出异常，被外面的操作捕获，获取异常的信息
3. Callable是支持泛型的

`新增方式二：线程池`

提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。

1. 提供指定线程数量的线程池
2. 执行指定的线程的操作。需要提供实现Runnable接口或Callable接口实现类的对象
3. 关闭连接池

```java
JDK 5.0起提供了线程池相关AP|： Executor Service和 Executors

Executor Service：真正的线程池接口。常见子类 Thread Poolexecutor
void execute(Runnable command）：执行任务/命令，没有返回值，一般用来执行Runnable
<T> Future<T> submit（Callable<T>task）：执行任务，有返回值，一般又来执行Callable
void shutdown()：关闭连接池

Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池
Executors. newCachedThreadPool()：创建一个可根据需要创建新线程的线程池
Executors.newFⅸedthreadPool(n)；创建一个可重用固定线程数的线程池
EXecutors. newSingleThreadEXecutor()：创建一个只有一个线程的线程池
Executors. new thread Poo(n)：创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。
             
示例
class NumberThread implements Runnable{

    @Override
    public void run() {
        for(int i = 0;i <= 100;i++){
            if(i % 2 == 0){
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}

class NumberThread1 implements Runnable{

    @Override
    public void run() {
        for(int i = 0;i <= 100;i++){
            if(i % 2 != 0){
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}

public class ThreadPool {

    public static void main(String[] args) {
        //1. 提供指定线程数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        ThreadPoolExecutor service1 = (ThreadPoolExecutor) service;
        //设置线程池的属性
//        System.out.println(service.getClass());
//        service1.setCorePoolSize(15);
//        service1.setKeepAliveTime();


        //2.执行指定的线程的操作。需要提供实现Runnable接口或Callable接口实现类的对象
        service.execute(new NumberThread());//适合适用于Runnable
        service.execute(new NumberThread1());//适合适用于Runnable

//        service.submit(Callable callable);//适合使用于Callable
        //3.关闭连接池
        service.shutdown();
    }

}
```



# [Java8新特性](https://juejin.cn/post/6962035387787116551#heading-22)

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1eb50aacef9949b1be6b379536569e4f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="image-20200507101934984" style="zoom:67%;" />

改进：

- 速度更快
- 代码更少（增加了新的语法：Lambda表达式）
- 引入强大的 `Stream APl`
- 便于并行
- 最大化减少空指针异常：`Optional`
- `Nashorn` 引擎，允许在JVM上运行 `JS` 应用
- 并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。相比较串行的流，并行的流可以很大程度上提高程序的执行效率。
- Java 8中将并行进行了优化，我们可以很容易的对数据进行并行操作。`Stream API` 可以声明性地通过 `parallel()` 与 `sequential()` 在并行流与顺序流之间进行切换



## Lambda 表达式

Lambda 是一个匿名函数，可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使 Java 的语言表达能力得到了提升。

`示例一`

```java
@Test
public void test1(){
    //未使用Lambda表达式的写法
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            System.out.println("hello Lambda!");
        }
    };
    r1.run();

    System.out.println("========================");
    //Lamdba表达式写法
    Runnable r2 = () -> System.out.println("hi Lambda!");
    r2.run();
}
```

`示例二`

```java
@Test
public void test2(){
    //未使用Lambda表达式的写法
    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o1,o2);
        }
    };

    int compare1 = com1.compare(12, 32);
    System.out.println(compare1);//-1
    System.out.println("===================");

    //Lambda表达式的写法
    Comparator<Integer> com2 = (o1,o2) -> Integer.compare(o1,o2);

    int compare2 = com2.compare(54, 21);
    System.out.println(compare2);//1
    System.out.println("===================");

    //方法引用
    Comparator<Integer> cpm3 = Integer::compareTo;
    int compare3 = cpm3.compare(12, 12);
    System.out.println(compare3);//0
}
```

`Lambda 表达式基本语法`

格式：

- `->` ：lambda 操作符 或 箭头操作符
- `->` 左边：lambda 形参列表 （其实就是接口中的抽象方法的形参列表）
- `->` 右边：lambda 体（其实就是重写的抽象方法的方法体）

`使用情况`

```java
// 语法格式一：无参，无返回值
Runnable r1 = () -> {System.out.println(“hello Lamdba!”)}

// 语法格式二：Lamdba需要一个参数，但没有返回值
Consumer<String> con = (String str) -> {System.out.println(str)}

// 语法格式三：数据类型可省略，因为可由编译器推断得出，称为类型推断
Consumer<String> con = (str) -> {System.out.println(str)}

// 语法格式四：Lamdba若只需要一个参数时，小括号可以省略
Consumer<String> con = str -> {System.out.println(str)}

// 语法格式五：Lamdba需要两个以上的参数，多条执行语句，并且可以有返回值
Comparator<Integer>com = (o1,o1) -> {
	Syste.out.println("Lamdba表达式使用");
    return Integer.compare(o1,o2);
}

// 语法格式六：当Lamdba体只有一条语句时，return和大括号若有，都可以省略
Comparator<Integer>com = (o1,o1) ->	Integer.compare(o1,o2);
```

`总结`

- Lambda 表达式的本质：作为函数式接口的实例
- 如果一个接口中，只声明了一个抽象方法，则此接口就称为函数式接口。我们可以在一个接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口。
- 因此以前用匿名实现类表示的现在都可以用 Lambda 表达式来写。

## 函数式接口

- 只包含一个抽象方法的接口，称为函数式接口。
- 可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda 表达式抛出一个受检异常（即：非运行时异常），那么该异常需要在目标接口的抽象方法上进行声明）。
- 可以在一个接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口。同时 `javadoc` 也会包含一条声明，说明这个接口是一个函数式接口。
- Lambda 表达式的本质：作为函数式接口的实例
- 在 `java.util.function` 包下定义了Java 8的丰富的函数式接口

```java
@FunctionalInterface
public interface MyInterface {
    void method1();
}
```

```java
public class LambdaTest3 {
    //    消费型接口 Consumer<T>     void accept(T t)
    @Test
    public void test1() {
        //未使用Lambda表达式
        Learn("java", new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println("学习什么？ " + s);
            }
        });
        System.out.println("====================");
        //使用Lambda表达
        Learn("html", s -> System.out.println("学习什么？ " + s));

    }

    private void Learn(String s, Consumer<String> stringConsumer) {
        stringConsumer.accept(s);
    }

    //    供给型接口 Supplier<T>     T get()
    @Test
    public void test2() {
        //未使用Lambdabiaodas
        Supplier<String> sp = new Supplier<String>() {
            @Override
            public String get() {
                return new String("我能提供东西");
            }
        };
        System.out.println(sp.get());
        System.out.println("====================");
        //使用Lambda表达
        Supplier<String> sp1 = () -> new String("我能通过lambda提供东西");
        System.out.println(sp1.get());
    }

    //函数型接口 Function<T,R>   R apply(T t)
    @Test
    public void test3() {
        //使用Lambda表达式
        Employee employee = new Employee(1001, "Tom", 45, 10000);

        Function<Employee, String> func1 =e->e.getName();
        System.out.println(func1.apply(employee));
        System.out.println("====================");

        //使用方法引用
        Function<Employee,String>func2 = Employee::getName;
        System.out.println(func2.apply(employee));

    }

    //断定型接口 Predicate<T>    boolean test(T t)
    @Test
    public void test4() {
        //使用匿名内部类
        Function<Double, Long> func = new Function<Double, Long>() {
            @Override
            public Long apply(Double aDouble) {
                return Math.round(aDouble);
            }
        };
        System.out.println(func.apply(10.5));
        System.out.println("====================");

        //使用Lambda表达式
        Function<Double, Long> func1 = d -> Math.round(d);
        System.out.println(func1.apply(12.3));
        System.out.println("====================");

        //使用方法引用
        Function<Double,Long>func2 = Math::round;
        System.out.println(func2.apply(12.6));

    }
}
```

`总结`

**何时使用lambda表达式？**

当需要对一个函数式接口实例化的时候，可以使用 lambda 表达式。

**何时使用给定的函数式接口？**

如果我们开发中需要定义一个函数式接口，首先看看在已有的 jdk 提供的函数式接口是否提供了能满足需求的函数式接口。如果有，则直接调用即可，不需要自己再自定义了。

## 方法的引用

类(或对象) :: 方法名

- 情况1 对象 `::` 非静态方法
- 情况2 类 `::` 静态方法
- 情况3 类 `::` 非静态方法

## 构造器和数组的引用

方法引用：类名 `::new`

数组引用：数组类型 `[] :: new`

## StreamAPI

- `Stream` 关注的是对数据的运算，与 `CPU` 打交道;集合关注的是数据的存储，与内存打交道;
- Java 8 提供了一套 `api` ，使用这套 `api` 可以对内存中的数据进行过滤、排序、映射、归约等操作。类似于 `sql` 对数据库中表的相关操作。
- `Stream` 是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。**“集合讲的是数据， Stream讲的是计算！”**

### 步骤一 创建 Stream

 **创建方式一：通过集合**

Java 8的 `Collection` 接口被扩展，提供了两个获取流的方法：

- `default Stream\<E> stream()` : 返回一个顺序流
- `default Stream\<E> parallelStream()` : 返回一个并行流

**创建方式二：通过数组**

Java 8中的 `Arrays` 的静态方法 `stream()` 可以获取数组流

- 调用 `Arrays` 类的 `static\<T> Stream\<T> stream(T[] array)`: 返回一个流
- 重载形式，能够处理对应基本类型的数组：
  - `public static IntStream stream（int[] array）`
  - `public static LongStream stream（long[] array）`
  - `public static DoubleStream stream（double[] array）`

**创建方式三：通过Stream的of()方法**

可以调用Stream类静态方法of()，通过显示值创建一个流。可以用于接收任意数量的参数

**创建方式四：创建无限流**

- 迭代: `public static\<T> Stream\<T> iterate(final T seed, final UnaryOperator\<T> f)`
- 生成: `public static\<T> Stream\<T> generate(Supplier\<T> s)`

### 步骤二 中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为**惰性求值**。

**筛选与切片**

**映射**

**排序**

### 步骤三 终止操作

**匹配与查找**

**归约**

**收集**

## Optional 类的使用

- 为了解决 java 中的空指针问题而生！
- `Optional<T> 类(java.util.Optional)` 是一个容器类，它可以保存类型 `T` 的值，代表这个值存在。或者仅仅保存 `null`，表示这个值不存在。原来用 `null` 表示一个值不存在，现在 `Optional` 可以更好的表达这个概念。并且可以避免空指针异常。

# 设计模式

`23种经典的设计模式`

创建型模式，共5种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。

结构型模式，共7种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。

行为型模式，共11种：策略模式、模板方法模式、观察者模式、迭代器模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

==单例模式==

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例。

```java
饿汉式1：
class Bank{
	
	//1.私化类的构造器
	private Bank(){
		
	}
	
	//2.内部创建类的对象
	//4.要求此对象也必须声明为静态的
	private static Bank instance = new Bank();
	
	//3.提供公共的静态的方法，返回类的对象
	public static Bank getInstance(){
		return instance;
	}
}

饿汉式2：使用了静态代码块
class Order{
	
	//1.私化类的构造器
	private Order(){
		
	}
	
	//2.声明当前类对象，没初始化
	//4.此对象也必须声明为static的
	private static Order instance = null;

	static{
		instance = new Order();
 	}
	
	//3.声明public、static的返回当前类对象的方法
	public static Order getInstance(){
		return instance;
	}
	
}

//懒汉式：
class Order{
	
	//1.私化类的构造器
	private Order(){
		
	}
	
	//2.声明当前类对象，没初始化
	//4.此对象也必须声明为static的
	private static Order instance = null;
	
	//3.声明public、static的返回当前类对象的方法
	public static Order getInstance(){
		
		if(instance == null){
			
			instance = new Order();
			
		}
		return instance;
	}
	
}
```

`方式对比`

- 饿汉式：上来就创建对象
  - 坏处：对象加载时间过长。
  - 好处：饿汉式是线程安全的
- 懒汉式：什么时候用什么时候造对象
  - 好处：延迟对象的创建。
  - 目前的写法坏处：线程不安全。--->到多线程内容时，再修改