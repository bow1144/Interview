## 一、基础概念与常识

### 1.1 Java语言有哪些特点？
Java的特点主要是面向对象、平台无关、支持多线程等

### 1.2 Java SE 与 Java EE
* Java SE 是基础版的Java平台，支持核心库与虚拟机等核心组件
* Java EE 是企业版平台，支持企业级的应用程序开发和标准规范

### 1.3 JVM、JDK、JRE
* JDK 是Java开发工具包，包含了JRE和其它工具
* JRE 是运行已编译Java查询所需的环境，主要包含JVM和Java基础类库
* JVM 是允许Java字节码的虚拟机，使用相同的字节码会给出相同的结果，是Java*随处运行*的关键，可以运行`.class`文件

### 1.4 Java程序从源代码到运行是什么样的？
`.java`、`javac`编译、`.class`、解释器、机器代码

### 1.5 什么是字节码，使用字节码有什么好处？
JVM可以理解的代码是**字节码**，即扩展名为`.class`的文件，只面向虚拟机，不针对特定的机器。因此Java程序无须重新编译便可在不同OD的计算机中运行。现在引入了JIT，可以直接保存热点机器码

JDK > JRE > JVM > JIT

### 1.6 为什么Java语言“编译与解释并存”？
Java先通过**编译**生成字节码`.class`，再由解释器执行

### 1.7 AOT与JIT有什么区别？
* AOT是一种新的编译方式，在程序执行前就编译成机器码，属于静态编译
* AOT避免了JIT预热等方面的开销，提高启动速度，减少内存占用，增加安全性（AOT编译的代码不容易被反编译）
* 但AOT无法支持反射、动态代理等动态特性
* JIT的主要优势是具备更高的极限处理能力

### 1.8 Java和C++的区别？
他们都是面向对象的语言，支持封装、继承、多态   
* Java不用指针直接访问内存、程序内存更加安全
* Java类不支持多继承，只能通过接口实现；C++支持多继承
* Java有自动内存垃圾回收机制，不需要`clear`等方法
* Java不支持操作符重载

## 二、基本语法

### 2.1 注释
文档注释，单行注释，多行注释

### 2.2 移位运算符
* `>>`带符号右移，移动后正负不变
* `>>>`无符号右移，高位用`0`补齐

## 三、基本数据类型

### 3.1 八种数据类型
* 整数类型 `byte`,`short`,`int`,`long`
* 浮点类型 `float`,`double`
* 字符类型 `char`
* 布尔类型 `boolean`

### 3.2 基本类型与包装类型的区别
包装类型是Java八种基础类型对应的对象类型，每种基本类型都有对应的类，允许将基本类型作为对象处理。

* 用途：在常量和局部变量使用基本类型，其它大多使用包装类型
* 存储方式：基本类型在栈中，包装类型**作为对象实例存储在堆中**
* 占用空间：基本数据类型的占用空间非常小
* 默认值：基本数据类型有默认值，包装类型默认为`null`
* 比较方式：`==`对于基本数据类型比较的是值，包装类型比较的是**地址**，要比较值应该用`equals()`方法
* 作为局部变量的基本类型存储在栈中，成员变量则存储在其它地方

### 3.3 包装类型的缓存机制了解么？
Java的基本数据类型的包装类型大部分用了缓存机制提升内存；整数类型是`[-128, 127]`，`Character`是`[0, 127]`，boolean直接返回True/False

```
Integer i1 = 33;
Integer i2 = 33;
System.out.println(i1 == i2);// 输出 true

Float i11 = 333f;
Float i22 = 333f;
System.out.println(i11 == i22);// 输出 false

Double i3 = 1.2;
Double i4 = 1.2;
System.out.println(i3 == i4);// 输出 false
```

```
Integer i1 = 40; // 直接使用缓存
Integer i2 = new Integer(40); // 创建新的对象
System.out.println(i1==i2); // 输出false
```

### 3.4 了解自动装箱与拆箱吗？原理是什么？
* 装箱：将基本类型用对应包装类型包装起来
* 拆箱：将包装类型转化为基础数据类型
* 频繁拆箱装箱会影响性能
```
Integer i = 10;  //装箱  
int n = i;   //拆箱
```

### 3.5 为什么浮点数运算会有精度丢失的风险，如何解决？
* 浮点数用二进制存储，宽度有限，无限小数存储时会被截断导致了精度损失
* 使用`BigDecimal`，还可以存储任意大小的整型数据

## 四、变量

### 4.1 成员变量与局部变量的区别？
```
public class add_1 {
  private int add; // 成员变量

  public add_1 (int num) {
    int a = 10; // 局部变量
    this.add = num;
  }
}
```

* **语法形式**：成员变量属于**类**，可以被`private`、`static`等修饰符修饰
* **存储空间**：如果成员变量用`static`修饰，属于类，存储在JVM的***元空间***；没有用`static`修饰则属于实例，存储与堆空间
* **生存时间**：成员变量是对象的一部分，随着对象的生命结束而结束；局部变量随着方法调用生成，调用结束则消亡
* **默认值**：成员变量会被赋默认值，局部变量不会

### 4.2 静态变量有什么用？
静态变量可以被类的所有实例共享，通常情况下被`final`修饰成为常量

## 五、方法

### 5.1 静态方法为什么不能调用非静态成员？
静态方法属于类，在类加载时就被分配内存；非静态成员属于对象，在对象实例化后才存在。总之在非静态成员存在时静态方法就存在了

### 5.2 静态方法和实例方法有什么区别？
1. 调用静态方法无序创建对象
2. 实例方法可以访问静态方法和实例方法

### 5.3 重载和重写有什么区别？
* 重载是对于一个方法，根据不同的输入做出不同处理
* 重写是子类继承父类的相同方法，有相同的输入但是有别于父类的响应。但是子类的`return`,`throw`必须小于等于父类

### 5.4 什么是可变长参数？
```
public static void method1(String... args) {
   //......
}
```
* 这个方法可以接收0或多个参数
* 可变长参数只能放在最后一个
* 方法重载优先匹配固定长参数

## 六、面向对象

### 6.1 面向对象和面向过程的区别？
面向对象会先抽出对象，再用对象执行方式解决问题；面向过程会把解决问题拆解称一个个方法

面向对象有以下优点：
* 易维护：有良好的封装性和结构
* 易复用：继承和多态
* 易扩展：模块化设计是系统

### 6.2 如何创建对象？对象实体与对象引用有什么区别？
* 用`new`创建对象实例（对象实例存储在堆中）
* 对象引用指向对象实例：对象引用放在栈中
* 一个对象可以有多个引用

### 6.3 对象相等于引用相等有什么区别？
对象的相等是内容相等；引用相等是内存地址的相等
```
String str1 = "hello";
String str2 = new String("hello");
String str3 = "hello";
// 使用 == 比较字符串的引用相等
System.out.println(str1 == str2);         // false
System.out.println(str1 == str3);         // true，比较的是字符串引用是否相等
// 使用 equals 方法比较字符串的相等
System.out.println(str1.equals(str2));    // true
System.out.println(str1.equals(str3));    // true
```

### 6.4 构造方法的特点？
* 名称与类相同
* 没有返回，不能用`void`声明
* 自动执行，无序显式调用

### 6.5 一个类没有构造方法的话可以执行吗？
会执行，因为类有无参的默认构造法

### 6.6 构造是否可以被重写？
不能`override`但可以`overload`，对应不同的参数列表

### 6.7 面向对象三大特征？
面向对象三大特征指的是**封装、继承、多态**

#### 封装
**封装指的是将对象的数据与方法封装在一起，对外部隐藏内部实现细节，只暴露必要的接口**。封装的目的是通过控制访问实现数据隐藏和保护，提高程序的安全性和可维护性

#### 继承
**继承允许子类继承父类的数据和方法，实现代码复用和扩展，建立了类之间的层次关系**。子类可以对父类进行重写和扩展`extend`。Java只能继承一个父类，但可以实现多个接口。子类拥有自己的方法就是对父类**扩展**；父类的私有属性和方法子类无法访问，只是拥有

#### 多态
**多态是同一个方法操作作用于不同类型的对象，并做出不同的行为；Java中多态的实现依赖于方法的重载与重写**。多态不能调用只在子类存在不在父类存在的方法

### 6.8 接口和抽象类有什么共同点和区别？
#### 共同点
* 实例化：二者都不能直接实例化，只能被实现（接口）或继承（抽象类）后才能创建具体对象
* 抽象方法：二者都可以包含抽象方法

#### 区别
* 接口强调约束，抽象类强调所属关系
* 一个类只能继承一个抽象类，但可以实现多个接口
* 接口中的成员变量只能是`public static final`，不能被修改；抽象类的成员变量可以被修改

Java8中引入`default`土工接口方法的默认实现
```
public interface MyInterface {
    default void defaultMethod() {
        System.out.println("This is a default method.");
    }
}
```

### 6.9 深拷贝和浅拷贝的区别？什么是引用拷贝？
* 引用拷贝：将两个不同引用指向一个对象
* 浅拷贝：在堆上创建一个新对象，如果内部属性是引用类型的话，直接复制其引用地址
* 深拷贝：完全复制整个对象，包括其内部变量

![三个拷贝](https://camo.githubusercontent.com/64f55124852392bc697b47e20eb943f40ab751511a1a1b11cea6db1d28cbb003/68747470733a2f2f6f73732e6a61766167756964652e636e2f6769746875622f6a61766167756964652f6a6176612f62617369732f7368616c6c6f7726646565702d636f70792e706e67)

## 七、Object

### 7.1 Object类的常见方法有哪些？
> Object类是一个特殊的类，是所有类的父类，主要有11个方法

```
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```

### 7.2 ==与equals()的区别
* 对于基本数据类型，`==`比较的是值
* 对于引用数据类型，`==`比较的是内存地址
* `equals()`不能比较基本数据类型，只能判断两个对象是否相等

```
String a = new String("ab"); // a 为一个引用
String b = new String("ab"); // b为另一个引用,对象的内容一样
String aa = "ab"; // 放在常量池中
String bb = "ab"; // 从常量池中查找
System.out.println(aa == bb);// true     // 二者地址相同
System.out.println(a == b);// false      // 二者地址不同
System.out.println(a.equals(b));// true
System.out.println(42 == 42.0);// true
```

### 7.3 hashcode() 有什么用
获取哈希码，确定对象在哈希表中的索引位置；作用和`equals()`类似，用于比较两个对象是否相等

### 7.4 为什么要有hashcode和equals两种方法？
* 两个对象`hashcode`不相等，可以认为两个对象不相等
* 两个对象`hashcode`相等，对象不一定相等（哈希碰撞）
* 两个对象`hashcode`和`equals`都相等，我们才认为两个对象相等

### 7.5 为什么重写equals()必须重写hashcode()？
如果没有重写hashcode，会导致equals()判断相等而hashcode()判断不相等，导致一些原本相等的对象被误判为不相等

## 八、String
### 8.1 String、StringBuffer、StringBuilder的区别？
* `String`不可变，`StringBuffer`和`StringBuilder`继承自`AbstractStringBuilder`类，有很多修改字符串的方法
* `String`对象不可变，线程安全，`StringBuffer`加锁了故线程安全，`StringBuilder`非线程安全，但性能略高
* 使用：
  - `String`适合少量数据
  - `StringBuilder`适合单线程操作
  - `StringBuffer`适合多线程操作
 
### 8.2 为什么String不可变？
1. 保存String的数组被`private final`修饰，且`String`类没有提供修改方法
2. `String`被`final`修饰不可被继承，避免子类破坏

### 8.3 字符串拼接用+还是StringBuilder？
Java语言仅有的两个重载操作符：面对字符串的`+=`和`+`，`+`操作实际上是调用`StringBuilder`的`append()`方法，但不建议在循环中用，因为每次循环都会创建一个`StringBuilder`对象

### 8.4 字符串常量池了解吗？
**字符串常量池**是JVM为提升性能针对字符串专门开辟的区域，目的是避免字符串的重复创建
```
// 在字符串常量池中创建字符串对象 ”ab“
// 将字符串对象 ”ab“ 的引用赋值给 aa
String aa = "ab";
// 直接返回字符串常量池中字符串对象 ”ab“，赋值给引用 bb
String bb = "ab";
System.out.println(aa==bb); // true
```

### 8.5 String s1 = new String("abc");这句话创建了几个字符串对象？
#### 常量池中不存在"abc"