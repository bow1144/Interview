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

### 1.9 JVM内存结构介绍？
* 分为五个部分：栈、堆、元空间、程序计数器、本地方法栈
* **元空间**：对JVM规范中方法区的实现
* **栈**：线程私有，存放局部变量等
* **堆**：JVM所有线程共享，虚拟机启动时创建；唯一作用是存放**对象实例**和字符串常量
* **本地方法栈**：native方法
* **程序计数器**：当前线程执行的字节码的行号指示器

### 1.10 线程为什么需要本地内存，不直接读主内存？
* **Java线程之间的通信采用的是共享内存模型（JMM内存模型）**
* JMM决定一个线程对共享变量的写入何时对另一个线程可见

## 二、基本语法

### 2.1 注释
文档注释，单行注释，多行注释

### 2.2 移位运算符
* `>>`带符号右移，移动后正负不变
* `>>>`无符号右移，高位用`0`补齐

## 三、基本数据类型与数据结构

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

### 3.6 Java有什么常用的集合类？
* List：列表
* Set：集合
* Map：映射
* Queue：队列

### 3.7 数组和链表的区别？
* 访问效率
* 插入和删除效率
* 缓存命中率：数组连续存储，可以提高缓存命中率
* 应用场景：数组适合静态大小，频繁访问，链表适合动态大小，频繁插入删除

### 3.8 堆和栈的区别？
* 分配方式：堆是动态分配，用于存储动态数据结构和对象；栈静态分配，存储函数局部变量和调用信息
* 内存管理：堆需要手动管理，可能会溢出，栈由编译器自行管理
* 大小速度：堆比栈大，时间慢

### 3.9 Set集合有什么特点？如何实现key无重复？
* 特点：元素不重复
* 实现：哈希表、红黑树判断是否重复

### 3.10 有序Set是什么？记录插入顺序的集合是什么？
* TreeSet和LinkedHashSet，前者通过红黑树实现，后者通过双链表和哈希表实现
* LinkedHashSet确保元素唯一性，还可以保持插入顺序

### 3.11 HashMap是线程安全的吗？
* 不是，多线程下会出现死循环
* 可以通过加锁等方式实现线程安全`ConcurrentHashMap`

### 3.12 ConcurrentHashMap用了悲观锁还是乐观锁?
* 都用到了，添加元素时会先判断容器是否为空
* 为空则加乐观锁，不为空这个桶为空也加乐观锁
* 桶不为空则加悲观锁

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

### 6.10 多态解决了什么问题？
* 解决了代码冗余问题
* 是很多设计模式的基础

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
* 在常量池创建字符串
* 在堆中创建字符串
#### 常量池中存在"abc"
* 在堆中创建对象，并用常量池中"abc"进行初始化

### 8.6 String#intern 方法有什么用？
调用`intern()`时，若常量池中存在相同内容的字符串，返回常量池中对象引用；否则将其添加到常量池中引用；目的是确保字符串引用在常量池中的唯一性

### 8.7 String#equals() 和 Object#equals() 有何区别？
String.equals()是被重写过的，比较的是字符串的值是否相等

### 8.8 String 类型的变量和常量做“+”运算时发生了什么？

#### 不加final
```
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";   // 编译器常量折叠，等价于 String str3 = "string"
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);//false
System.out.println(str3 == str5);//true
System.out.println(str4 == str5);//false
```

#### 加final
```
final String str1 = "str";  // 被final修饰的string会被当作常量
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 常量池中的对象
System.out.println(c == d);// true
```

## 九、异常

### 9.1 异常类型
`Throwable`->`Exception`,`Error`  
`Exception`->`Checked Exception`,`Uncheaked Exception`

### 9.2 Exception和Error有什么区别？
* `Exception`是程序可以处理的异常，分为`Checked Exception`（受检查异常，必须处理）和`Uncheaked Exception`（不受检查异常，可以不处理）
* `Error`是程序无法处理的错误，不建议通过`catch`捕获，错误发生时JVM一般会选择线程终止

### 9.3 Checked Exception 和 Unchecked Exception 有什么区别？
* 如果受检查异常没有被catch或者throws关键字处理的话，就没办法通过编译
* 不受检查异常可以通过编译，`RuntimeException`及其子类都是

### 9.4 Throwable类常见的方法有哪些？
* `String getMessage()` 返回异常发生的详细信息
* `String toString()`返回简要描述
* `void printStackTrace()`在控制台上打印`Throwable`对象封装的异常信息

### 9.5 Try-catch-finally如何使用？
* `try`用于捕获异常，后面可加0或多个`catch`，若没有`catch`则必须跟一个`finally`
* `catch`用于处理捕获的异常
* `finally`无论是否有异常，`finally`中的语句都会执行，如果`try/catch`中遇到`return`，`finally`的语句都会被执行；不要在finally中使用return
```
try {
    System.out.println("Try to do something");
    throw new RuntimeException("RuntimeException");
} catch (Exception e) {
    System.out.println("Catch Exception -> " + e.getMessage());
} finally {
    System.out.println("Finally");
}
```

### 9.6 finally中的代码一定会执行吗？
不一定，比如虚拟机关机和CPU关机

### 9.7 异常使用有什么需要注意的地方？
* 不要把异常定义为静态变量，否则会引起栈信息混乱
* 抛出信息一定要有意义
* 异常能是子类就不能是父类
* 避免重复记录日志

## 十、泛型
### 10.1 什么是泛型？有什么作用？
* 泛型允许类、接口、方法在定义时使用类型参数而不是指定具体类型，避免了类型转化异常
* 作用：类型安全、代码复用性、减少类型转化

### 10.2 泛型的使用方式有哪几种？
泛型有三种使用方式：**泛型类、泛型接口、泛型方法**  

#### 泛型类
类似`vector<int> v`  
```
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}

Generic<Integer> genericInteger = new Generic<Integer>(123456);
```

#### 泛型接口
```
public interface Generator<T> {
    public T method();
}

// 不指定类型
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}

// 指定类型
class GeneratorImpl implements Generator<String> {
    @Override
    public String method() {
        return "hello";
    }
}
```

#### 泛型方法
```
   public static < E > void printArray( E[] inputArray )
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }

// 创建不同类型数组：Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```

### 10.3 项目中哪里用到泛型？
* 自定义接口通用返回类型
* 构筑工具集

## 十一、反射

### 11.1 什么是反射？
反射允许程序运行时查看类、接口、方法、字段的信息；通过反射程序可以动态获取类的信息，还可以自己创建对象、调用方法、修改类的结构

### 11.2 反射的优缺点
* 优点在于灵活代码、为工具提供了开箱即用的便利
* 缺点在于增加了安全问题，比如无视泛型参数的安全检查

### 11.3 反射的应用场景？
* 动态代理
* 注解

## 十二、注解
### 12.1 什么是注解？
一种特殊的注释，修饰类、方法、变量，提供信息在编译运行时使用；注解本质上时继承了`Annotation`的特殊接口

### 12.2 注解的解析方法有哪几种？
* 编译期直接处理：比如`@Override`，编译时会检测方法是否重写父类对应的方法
* 运行时通过反射处理：Spring框架的注解都是通过反射处理

## 十三、SPI

### 13.1 什么是SPI？
*Service Provider Interface*，服务提供者的接口，是专门给扩展功能的开发者提供的接口；SPI将服务接口与服务实现分离，提升程序的扩展性和可维护性。Spring框架、Dubbo框架都用了SPI机制

### 13.2 API和SPI有什么区别？
* API是面向应用程序的，实现了可以调用的功能和服务
* SPI是供插件开发者提供的接口，允许平台动态加载扩展机制

### 13.3 SPI的优缺点？
* 优点：大大提高接口的灵活性
* 需要遍历所有类，效率偏低、多个SPI一起`load`时会有并发问题

## 十四、序列化和反序列化
### 14.1 什么是序列化？什么是反序列化？
* 序列化是将数据结构和对象等转化为可以传输存储的方式，主要是二进制字节和`JSON`等文本格式
* 反序列化是将序列化数据转化为数据结构和对象

### 14.2 序列化与反序列化的应用场景？
* 对象要进行网络传输（RPC等），接收后再反序列化
* 存储到文件、数据库、内存

序列化应用于TCP/IP的应用层

### 14.3 有些字段不想进行序列化怎么办？
* 不想序列化的变量用`transient`修饰，阻止变量序列化；反序列化后会被设置为默认值
* `static`变量不属于任何对象，不会被序列化

### 14.4 常见的序列化协议有哪些？
基于二进制的序列化协议，`JSON,XML`等文本序列化协议性能较差，不会使用

### 14.5 为什么不推荐JDK自带的序列化？
不支持跨语言调用、性能差、存在安全问题

## 十五、I/O

### 15.1 JavaIO流了解吗？
I/O流即输入输出。数据输入到计算机内存即输入，从内存到外部存储即输出；根据数据的处理方式分为字节流和字符流。JavaIO是从四个抽象类派生出来的：
* `InputStream`,字节输入流
* `Reader`,字符输入流
* `OutputStream`,字节输出流
* `Writer`,字符输出流

### 15.2 IO流为什么分成字节流和字符流？
* 不知道编码类型的话，使用字节流传输很容易出现乱码问题
* 字符流通过JDK将字节转化来的，整个过程比较耗时

### 15.3 Java IO中的设计模式有哪些？
* 装饰器模式：通过组合替代继承扩展原始类功能，在继承关系复杂时更有用
* 适配器模式：协调接口互不兼容的类

### 15.4 BIO、NIO、AIO的区别？
* BIO：阻塞IO，适用于连接少而固定的场景
* NIO：非阻塞IO，适用于高并发场景
* AIO：异步IO，适用于连接数多且每个连接操作耗时较长的场景

## 十六、多线程
### 16.1 Java创建线程有几种方式？
* 继承Thread类并重写run()方法，缺点是继承了Thread类之后无法继承其它父类
* 实现Runnable接口并实现run()方法，缺点是编程较为复杂
* 使用Callable和Future接口创建线程

### 16.2 线程池有哪些优势？
* Java线程池（ThreadPool）是一个用于管理和复用线程的机制，它通过一组已创建的线程来执行多个任务，而不是每次任务来临时创建新线程。
* 优点：减少线程创建和销毁开销频繁地创建和销毁线程会消耗大量系统资源，线程池通过重用已存在的线程来减少这种开销
* 提高响应速度：当任务到达时，无需等待线程的创建即可立即执行，因为线程池中已经有等待的线程。

### 16.3 哪些集合类是线程安全的，哪些不是？
* 线程安全性是指多个线程在并发执行时，数据结构能够正确地维护其状态而不需要外部同步控制
* Vector、HashTable、Properties是线程安全的；
* ArrayList、LinkedList、HashSet、TreeSet、HashMap、TreeMap等都是线程不安全的

### 16.4 volatile和synchronized有什么区别？
* Volatile是一种轻量级同步机制，一个变量声明为Volatile时，线程直接从内存读写
* Synchronized是一种排他性同步机制，同一时刻只允许一个线程访问共享资源

## 十七、Spring

### 17.1 Spring是什么？
Spring是一个用于构建大型程序的开源框架，提供了一整套解决方案，简化了开发过程，提升了应用程序的模块化和可维护性

### 17.2 Spring框架的核心概念有哪些？
* 控制反转Ioc：将对象的创建和管理交给框架，Spring通过**依赖注入**实现Ioc
* 面向切面编程AOP：允许你将关注点分离，让业务逻辑更加清晰
* 事务管理：提供了一致的事务管理接口
* Spring容器

### 17.3 Spring的主要模块有哪些？
* Spring core：核心，Ioc和依赖注入
* Spring AOP：提供面向切面编程的支持
* Spring Data Access/Integration：对数据库的访问
* Spring MVC：Web模块
* Spring Security：认证和授权模块
* Spring Boot：零配置的应用程序，开箱即用的Web服务器

### 17.4 Spring的优势在于什么？
* 松耦合：依赖注入实现解耦
* 模块化
* 平台无关性：对不同技术栈的广泛支持
* 测试友好：支持单元测试
* 可扩展性

### 17.5 Spring中的事务隔离级别有哪些？
* Default：默认隔离级别，与所连接数据库隔离级别为准
* 读未提交，已提交，可重复读，串行化，和数据库一致

### 17.6 Spring 事务的传播行为有哪些？

<img width="511" alt="{888CAE25-9039-4487-A748-72D56B459BB7}" src="https://github.com/user-attachments/assets/84f9d96c-1f88-4c53-b3ec-4e5a1518607a">
